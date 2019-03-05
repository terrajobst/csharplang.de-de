---
ms.openlocfilehash: 67019511d49a786a5d6edf6fea442f745fc40f3f
ms.sourcegitcommit: 0a80f26b8e455c4f09843a10e11e29c24d2d922e
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/05/2019
ms.locfileid: "57347273"
---
# <a name="expressions"></a>Ausdrücke

Ein Ausdruck ist eine Sequenz von Operatoren und Operanden. In diesem Kapitel wird die Syntax, die Reihenfolge der Auswertung von Operanden und Operatoren und Bedeutung von Ausdrücken definiert.

## <a name="expression-classifications"></a>Ausdrucksklassifizierungen

Ein Ausdruck ist eines der folgenden Elemente:

*  Ein Wert. Jeder Wert verfügt über einen zugeordneten Typ.
*  Eine Variable. Jede Variable hat einen zugeordneten Typ, d. h. den deklarierten Typ der Variablen.
*  Ein Namespace. Ein Ausdruck mit dieser Klassifizierung kann nur verwendet werden, als der linken Seite von einem *Member_access* ([Memberzugriff](expressions.md#member-access)). In einem anderen Kontext bewirkt, dass ein Ausdruck, der als Namespace klassifiziert einen Fehler während der Kompilierung.
*  Ein Typ. Ein Ausdruck mit dieser Klassifizierung kann nur verwendet werden, als der linken Seite von einem *Member_access* ([Memberzugriff](expressions.md#member-access)), oder als Operand für die `as` Operator ([den Operator ](expressions.md#the-as-operator)), wird die `is` Operator ([der is-Operator](expressions.md#the-is-operator)), oder die `typeof` Operator ([der Typeof-Operator](expressions.md#the-typeof-operator)). In einem anderen Kontext bewirkt, dass ein Ausdruck, der als Typ klassifiziert einen Fehler während der Kompilierung.
*  Eine Methodengruppe, ein Satz von überladenen Methoden, die sich aus einer Membersuche ergibt ([Membersuche](expressions.md#member-lookup)). Eine Methodengruppe kann es sich um einen zugeordneten Instanzausdruck und einer Argumentliste für den zugeordneten Typ haben. Wenn eine Instanzmethode aufgerufen wird, wird das Ergebnis der Auswertung des Ausdrucks für die Instanz die Instanz, die durch dargestellt `this` ([diesen Zugriff](expressions.md#this-access)). Eine Methodengruppe ist zulässig, eine *Invocation_expression* ([Aufrufausdrücke](expressions.md#invocation-expressions)), ein *Delegate_creation_expression* ([Delegaterstellung Ausdrücke](expressions.md#delegate-creation-expressions)) sowie der linken Seite des ein is-Operator, und in einen kompatiblen Delegattyp implizit konvertiert werden kann ([Gruppe Konvertierungen](conversions.md#method-group-conversions)). In einem anderen Kontext bewirkt, dass ein Ausdruck, der als eine Methodengruppe klassifiziert einen Fehler während der Kompilierung.
*  Ein null-Literal. Ein Ausdruck mit dieser Klassifizierung kann implizit in einem Referenztyp oder nullable-Typ konvertiert werden.
*  Eine anonyme Funktion. Ein Ausdruck mit dieser Klassifizierung kann implizit in einen kompatiblen Delegattyp bzw. den Typ für die Ausdrucksbaumstruktur konvertiert werden.
*  Ein Eigenschaftenzugriff. Jedem Eigenschaftenzugriff ist einen zugeordneten Typ, d. h. den Typ der Eigenschaft. Darüber hinaus möglicherweise Zugriff auf eine Eigenschaft einen zugeordneten Instanzausdruck. Wenn ein Accessor (die `get` oder `set` Block) einer Instanz der Zugriff auf Eigenschaften aufgerufen wird, das Ergebnis der Auswertung des Ausdrucks für die Instanz wird die Instanz, die durch dargestellt `this` ([diesen Zugriff](expressions.md#this-access)).
*  Ein Event-Zugriff. Jedes Ereigniszugriff verfügt über einen zugeordneten Typ, d. h. den Typ des Ereignisses. Darüber hinaus kann ein Ereignis einen zugeordneten Instanzausdruck zugreifen. Ein Ereigniszugriff möglicherweise angezeigt, wie der linke Operand des der `+=` und `-=` Operatoren ([Ereignis Zuweisung](expressions.md#event-assignment)). In einem anderen Kontext bewirkt, dass ein Ausdruck, der klassifiziert als Zugriffs-Ereignis einen Fehler während der Kompilierung.
*  Indexzugriff. Jeder Indexzugriff verfügt über einen zugeordneten Typ, d. h. der Elementtyp des Indexers. Darüber hinaus hat Indexzugriff einen zugeordneten Instanzausdruck und eine Liste zugeordnete Argument. Wenn ein Accessor (die `get` oder `set` Block) eines Indexers Zugriff aufgerufen wird, das Ergebnis der Auswertung des Ausdrucks für die Instanz wird die Instanz, die durch dargestellt `this` ([diesen Zugriff](expressions.md#this-access)), und das Ergebnis des Auswerten der Argumentliste, wird die Parameterliste des Aufrufs.
*  "Nothing". Dies tritt auf, wenn der Ausdruck einen Aufruf einer Methode mit einem Rückgabetyp von `void`. Ein Ausdruck, der als "nothing" nur im Kontext gültig ist klassifiziert einen *Statement_expression* ([ausdrucksanweisungen](statements.md#expression-statements)).

Das Endergebnis eines Ausdrucks ist nie ein Namespace, Typ, Methodengruppe oder Ereigniszugriff. Stattdessen sind diese Kategorien von Ausdrücken wie oben erwähnt, intermediate-Konstrukte, die nur in bestimmten Kontexten zulässig sind.

Eine Eigenschaft oder Indexzugriff ist immer neu klassifiziert als Wert durch Aufrufen von der *get-Accessor* oder *set-Accessor*. Die bestimmte Zugriffsmethode richtet sich nach der im Rahmen der Eigenschaft oder der Indexer-Zugriff: Wenn der Zugriff auf das Ziel einer Zuweisung, ist die *set-Accessor* wird aufgerufen, um einen neuen Wert zuzuweisen ([einfache Zuweisung](expressions.md#simple-assignment)). Andernfalls die *get-Accessor* wird aufgerufen, um den aktuellen Wert zu erhalten ([Werte Ausdrücke](expressions.md#values-of-expressions)).

### <a name="values-of-expressions"></a>Werte der Ausdrücke

Die meisten Konstrukte, bei denen einen Ausdruck erfordern schließlich den Ausdruck zur Angabe einer ***Wert***. In solchen Fällen, wenn der tatsächliche Ausdruck steht für einen Namespace, einen Typ, einer Methodengruppe oder nichts, tritt ein Fehler während der Kompilierung. Wenn der Ausdruck einen Eigenschaftenzugriff, Indexzugriff oder eine Variable kennzeichnet, wird der Wert der Eigenschaft, Indexer oder Variable jedoch implizit ersetzt:

*  Der Wert einer Variablen ist einfach der Wert, die derzeit in den Speicherort identifiziert, die von der Variablen gespeichert. Eine Variable muss definitiv zugewiesen berücksichtigt werden ([definitive Zuweisung](variables.md#definite-assignment)), bevor der Wert abgerufen werden kann, oder andernfalls tritt ein Fehler während der Kompilierung auf.
*  Der Wert eines Eigenschaftsausdrucks Zugriff wird ermittelt, durch den Aufruf der *get-Accessor* der Eigenschaft. Wenn die Eigenschaft keine *get-Accessor*, ein Fehler während der Kompilierung auftritt. Andernfalls einen Member-Funktionsaufruf ([Überprüfungen zur Kompilierzeit der dynamischen überladungsauflösung](expressions.md#compile-time-checking-of-dynamic-overload-resolution)) ausgeführt wird, und das Ergebnis des Aufrufs wird der Wert der Eigenschaftsausdruck den Zugriff.
*  Der Wert eines Ausdrucks der Indexer-Zugriff wird ermittelt, durch den Aufruf der *get-Accessor* des Indexers. Wenn der Indexer keine *get-Accessor*, ein Fehler während der Kompilierung auftritt. Andernfalls einen Member-Funktionsaufruf ([Überprüfungen zur Kompilierzeit der dynamischen überladungsauflösung](expressions.md#compile-time-checking-of-dynamic-overload-resolution)) erfolgt mit dem Argument Liste der Indexer-Access-Ausdruck zugeordnet ist, und das Ergebnis des Aufrufs wird der Wert der Zugriff Indexerausdrucks.

## <a name="static-and-dynamic-binding"></a>Statische und dynamische Bindung

Der Prozess des bestimmens der Bedeutung eines Vorgangs basierend auf den Typ oder den Wert der enthaltenen Ausdrücke (Argumente, Operanden, Empfänger) wird häufig als bezeichnet ***Bindung***. Beispielsweise wird die Bedeutung eines Methodenaufrufs bestimmt basierend auf dem Typ, der den Empfänger und die Argumente. Die Bedeutung eines Operators wird basierend auf den Typ des Operanden bestimmt.

In C# geschrieben, die Bedeutung eines Vorgangs in der Regel richtet sich zum Zeitpunkt der Kompilierung, basierend auf dem Zeitpunkt der Kompilierung von der enthaltenen Ausdrücke. Ebenso, wenn ein Ausdruck einen Fehler enthält, der Fehler wird erkannt und vom Compiler gemeldet. Dieser Ansatz wird bezeichnet als ***statische Bindung***.

Allerdings ist ein Ausdruck eines dynamischen Ausdrucks (z. B. weist den Typ `dynamic`) Dies bedeutet, dass eine Bindung, die sie ein Teil ist basieren soll auf die Laufzeit-Typinformationen (d. h. der tatsächliche Typ des Objekts zur Laufzeit Dies bedeutet) anstatt den Typ am hat während der Kompilierung. Die Bindung eines solchen Vorgangs ist die Zeit aus diesem Grund zurückgestellt, in dem der Vorgang ist während der Ausführung des Programms ausgeführt werden. Dies wird als bezeichnet ***dynamische Bindung***.

Wenn ein Vorgang dynamisch gebunden ist, wird vom Compiler nur wenig oder keine Überprüfung ausgeführt. Wenn die Bindung zur Laufzeit ein Fehler auftritt, werden stattdessen Fehler als Ausnahmen zur Laufzeit gemeldet.

Die folgenden Vorgänge in c# unterliegen Bindung aus:

*  Memberzugriff: `e.M`
*  Methodenaufruf: `e.M(e1, ..., eN)`
*  Delegataufruf:`e(e1, ..., eN)`
*  Der Elementzugriff: `e[e1, ..., eN]`
*  Erstellen von Objekten: `new C(e1, ..., eN)`
*  Überladen von Unäroperatoren: `+`, `-`, `!`, `~`, `++`, `--`, `true`, `false`
*  Binäre Operatoren überladen: `+`, `-`, `*`, `/`, `%`, `&`, `&&`, `|`, `||`, `??`, `^`, `<<` , `>>`, `==`,`!=`, `>`, `<`, `>=`, `<=`
*  Zuweisungsoperatoren: `=`, `+=`, `-=`, `*=`, `/=`, `%=`, `&=`, `|=`, `^=`, `<<=`, `>>=`
*  Implizite und explizite Konvertierungen

Wenn Sie keine dynamischen Ausdrücke beteiligt sind, standardmäßig c# statische Bindung, was bedeutet, dass die Typen während der Kompilierung der enthaltenen Ausdrücke in den Prozess verwendet werden. Wenn einer der enthaltenen Ausdrücke in den oben aufgeführten Vorgängen eines dynamischen Ausdrucks ist, ist jedoch, der Vorgang, stattdessen dynamisch gebunden.

### <a name="binding-time"></a>Bindung-time

Statische Bindung erfolgt zum Zeitpunkt der Kompilierung, während dynamische Bindung zur Laufzeit stattfindet. In den folgenden Abschnitten der Begriff ***Bindung Kompilierzeit*** bezieht sich entweder während der Kompilierung oder zur Laufzeit, je nachdem, wann die Bindung erfolgt.

Das folgende Beispiel veranschaulicht die Konzepte der statische und dynamische Bindung und der Bindung Zeit:
```csharp
object  o = 5;
dynamic d = 5;

Console.WriteLine(5);  // static  binding to Console.WriteLine(int)
Console.WriteLine(o);  // static  binding to Console.WriteLine(object)
Console.WriteLine(d);  // dynamic binding to Console.WriteLine(int)
```

Die ersten beiden Aufrufe statisch gebunden sind: die Überladung von `Console.WriteLine` wird ausgewählt, basierend auf dem Zeitpunkt der Kompilierung von deren Argument. Daher ist die Bindung-die Zeit während der Kompilierung.

Der dritte Aufruf dynamisch gebunden ist: die Überladung von `Console.WriteLine` wird ausgewählt, basierend auf dem Laufzeit-Typ des Arguments. Dies liegt daran, dass das Argument eines dynamischen Ausdrucks – der Kompilierzeittyp `dynamic`. Daher ist die Bindung und die Uhrzeit für die dritte Aufruf zur Laufzeit.

### <a name="dynamic-binding"></a>dynamische Bindung

Die dynamische Bindung dient zum Zulassen von C#-Programme für die Interaktion mit ***dynamische Objekte***, d. h. Objekte, die nicht die üblichen Regeln der C# folgen-Typsystem. Dynamische Objekte können Objekte aus anderen Programmiersprachen mit verschiedenen Systemen werden, oder sie möglicherweise Objekte, die programmgesteuert einrichten, um ihre eigene Bindung-Semantik für verschiedene Vorgänge zu implementieren sind.

Der Mechanismus, mit dem ein dynamisches Objekt eine eigene Semantik implementiert, ist die Implementierung definiert. Eine bestimmte Schnittstelle – erneut Implementierung definiert – wird durch dynamische Objekte für die c#-Laufzeit zu signalisieren, dass sie spezielle Semantik haben implementiert. Daher, wenn Vorgänge für ein dynamisches Objekt dynamisch gebunden sind, haben ihre eigene Bindung-Semantik, anstatt die C# -Code wie in diesem Dokument angegeben.

Während der Zweck der dynamischen Bindung ist um interoperation mit dynamischen Objekten zu ermöglichen, ermöglicht c# dynamische Bindung für alle Objekte, ob sie dynamisch oder nicht sind. Dies ermöglicht eine nahtlosere Integration der dynamische Objekte, wie die Ergebnisse der Vorgänge auf diesen können selbst keine dynamische Objekte, aber immer noch von einem Typ, der dem Programmierer zum Zeitpunkt der Kompilierung nicht bekannt sind. Dynamische Bindung können fehleranfällige Reflection basierender Code entfernen, selbst, wenn Sie keine beteiligten Objekte dynamische Objekte sind.

In den folgenden Abschnitten beschreiben für jedes Konstrukt in der Sprache, genau Wenn dynamische Bindung übernommen wird, was zur Kompilierzeit – Wenn--wird angewendet, und welche die während der Kompilierung Result und Expression-Klassifizierung ist.

### <a name="types-of-constituent-expressions"></a>Typen von einzelnen Ausdrücken

Wenn ein Vorgang statisch gebunden ist, gilt der Typ eines einzelnen Ausdrucks (z. B. einen Empfänger, ein Argument, einen Index oder ein Operand) immer der Kompilierzeit-Typ dieses Ausdrucks sein.

Wenn ein Vorgang dynamisch gebunden ist, wird der Typ eines einzelnen Ausdrucks auf unterschiedliche Weise abhängig vom Zeitpunkt der Kompilierung der enthaltenen Ausdruck bestimmt:

*  Einen einzelnen Ausdruck Kompilierzeittyp `dynamic` gilt den Typ des tatsächlichen Werts haben die Auswertung des Ausdrucks, zur Laufzeit
*  Gilt als ein einzelnen Ausdruck, dessen Typ während der Kompilierung ein Typparameter ist, den Typ verfügen, die der Type-Parameter zur Laufzeit gebunden ist
*  Andernfalls gilt der enthaltenen Ausdruck der Kompilierzeit-Typ aufweisen.

## <a name="operators"></a>Operatoren

Ausdrücke bestehen aus ***Operanden*** und ***Operatoren***. Die Operatoren eines Ausdrucks geben an, welche Operationen auf die Operanden angewendet werden. Beispiele für Operatoren sind `+`, `-`, `*`, `/` und `new`. Beispiele für Operanden sind Literale, Felder, lokale Variablen und Ausdrücke.

Es gibt drei Arten von Operatoren:

*  Unäre Operatoren. Die Unäroperatoren verwenden einen Operanden, und verwenden Sie entweder Präfix-Notation (z. B. `--x`) oder postfix-Notation (z. B. `x++`).
*  Binäre Operatoren. Die binären Operatoren nehmen zwei Operanden, und alle Infix-Notation verwenden (z. B. `x + y`).
*  ternärer operator Nur ein ternärer Operator, `?:`, vorhanden ist; er übernimmt drei Operanden und infix-Notation (`c ? x : y`).

Die Reihenfolge der Auswertung von Operatoren in einem Ausdruck richtet sich nach der ***Rangfolge*** und ***Assoziativität*** der Operatoren ([Operatorrangfolge und Assoziativität](expressions.md#operator-precedence-and-associativity)) .

Die Operanden in einem Ausdruck werden von links nach rechts ausgewertet. Z. B. in `F(i) + G(i++) * H(i)`, Methode `F` wird aufgerufen, mit der alte Wert des `i`, then-Methode `G` wird aufgerufen, mit dem alten Wert der `i`, und schließlich-Methode `H` wird aufgerufen, mit dem neuen Wert der `i`. Dadurch wird unabhängig vom Operatorrangfolge.

Bestimmte Operatoren können werden ***überladen***. Überladen von Operatoren ermöglicht eine benutzerdefinierte operatorimplementierungen für Vorgänge, bei denen ein angegeben werden, oder beide der Operanden sind von einer benutzerdefinierten Klasse oder Struktur-Typ ([operatorüberladung](expressions.md#operator-overloading)).

### <a name="operator-precedence-and-associativity"></a>Operatorrangfolge und Assoziativität

Wenn ein Ausdruck mehrere Operatoren enthält, bestimmt die ***Rangfolge*** der Operatoren die Reihenfolge, in der die einzelnen Operatoren ausgewertet werden. Beispiel: der Ausdruck `x + y * z` wird als ausgewertet, `x + (y * z)` da die `*` Operator hat Vorrang vor der Binärdatei `+` Operator. Die Rangfolge von einem Operator wird durch die Definition ihrer zugeordneten Grammatik Produktion eingerichtet. Z. B. eine *Additive_expression* besteht aus einer Sequenz von *Multiplicative_expression*s getrennt durch `+` oder `-` Operatoren, wodurch die `+` und `-` Operatoren niedrigere Priorität als die `*`, `/`, und `%` Operatoren.

In der folgende Tabelle sind alle Operatoren in der Reihenfolge der Rangfolge von oben nach unten zusammengefasst:

| __Bereich__                                                                                   | __Kategorie__                | __Operatoren__ | 
|-----------------------------------------------------------------------------------------------|-----------------------------|---------------|
| [Primary expressions (Primäre Ausdrücke)](expressions.md#primary-expressions)                                     | Primär                     | `x.y`  `f(x)`  `a[x]`  `x++`  `x--`  `new`  `typeof`  `default`  `checked`  `unchecked`  `delegate` | 
| [Unary operators (Unäre Operatoren)](expressions.md#unary-operators)                                             | Unär                       | `+`  `-`  `!`  `~`  `++x`  `--x`  `(T)x` | 
| [Arithmetic operators (Arithmetische Operatoren)](expressions.md#arithmetic-operators)                                   | Multiplikativ              | `*`  `/`  `%` | 
| [Arithmetic operators (Arithmetische Operatoren)](expressions.md#arithmetic-operators)                                   | Additiv                    | `+`  `-`      | 
| [Shift operators (Schiebeoperatoren)](expressions.md#shift-operators)                                             | Shift                       | `<<`  `>>`    | 
| [Relational and type-testing operators (Relationale und Typtestoperatoren)](expressions.md#relational-and-type-testing-operators) | Relational und Typtest | `<`  `>`  `<=`  `>=`  `is`  `as` | 
| [Relational and type-testing operators (Relationale und Typtestoperatoren)](expressions.md#relational-and-type-testing-operators) | Gleichheit                    | `==`  `!=`    | 
| [Logical operators (Logische Operatoren)](expressions.md#logical-operators)                                         | Logisches AND                 | `&`           | 
| [Logical operators (Logische Operatoren)](expressions.md#logical-operators)                                         | Logisches XOR                 | `^`           | 
| [Logical operators (Logische Operatoren)](expressions.md#logical-operators)                                         | Logisches OR                  | <code>&#124;</code>           |
| [Conditional logical operators (Bedingte logische Operatoren)](expressions.md#conditional-logical-operators)                 | Bedingtes AND             | `&&`          | 
| [Conditional logical operators (Bedingte logische Operatoren)](expressions.md#conditional-logical-operators)                 | Bedingtes OR              | <code>&#124;&#124;</code>          | 
| [The null coalescing operator (Der NULL-Sammeloperator)](expressions.md#the-null-coalescing-operator)                   | NULL-Sammeloperator             | `??`          | 
| [Conditional operator (Bedingte Operatoren)](expressions.md#conditional-operator)                                   | Bedingt                 | `?:`          | 
| [Zuweisungsoperatoren](expressions.md#assignment-operators), [anonyme Funktionsausdrücke](expressions.md#anonymous-function-expressions)  | Zuweisungs- und Lambda-Ausdrücke | `=`  `*=`  `/=`  `%=`  `+=`  `-=`  `<<=`  `>>=`  `&=`  `^=`  <code>&#124;=</code>  `=>` | 

Tritt ein Operand zwischen zwei Operatoren mit gleicher Rangfolge auf, steuert die Assoziativität der Operatoren die Reihenfolge, in der die Vorgänge ausgeführt werden:

*  Mit Ausnahme der Zuweisungsoperatoren und der null-Sammeloperator, alle binären Operatoren sind ***linksassoziativ***, was bedeutet, dass Vorgänge von links nach rechts ausgeführt werden. `x + y + z` wird beispielsweise als `(x + y) + z` ausgewertet.
*  Die Zuweisungsoperatoren, die null-Sammeloperator und der bedingte Operator (`?:`) sind ***rechtsassoziativ***, was bedeutet, dass Vorgänge, die von rechts nach links ausgeführt werden. `x = y = z` wird beispielsweise als `x = (y = z)` ausgewertet.

Rangfolge und Assoziativität können mit Klammern gesteuert werden. In `x + y * z` wird beispielsweise zuerst `y` mit `z` multipliziert und dann das Ergebnis zu `x` addiert, aber in `(x + y) * z` werden zunächst `x` und `y` addiert, und dann wird das Ergebnis mit `z` multipliziert.

### <a name="operator-overloading"></a>Überladen von Operatoren

Alle unären und binären Operatoren sind Implementierungen vordefiniert, die in jedem Ausdruck automatisch verfügbar sind. Zusätzlich zu den vordefinierten Implementierungen können benutzerdefinierte Implementierungen dazu eingeleitet werden `operator` Deklarationen in Klassen und Strukturen ([Operatoren](classes.md#operators)). Benutzerdefinierte Implementierungen haben immer Vorrang vor vordefinierte operatorimplementierungen: Nur bei der keine Implementierungen der entsprechenden benutzerdefinierten Operator wird vorhanden sind die vordefinierten operatorimplementierungen berücksichtigt werden wie in beschrieben, [unäroperator überladungsauflösung](expressions.md#unary-operator-overload-resolution) und [binärer Operator-Überladung Auflösung](expressions.md#binary-operator-overload-resolution).

Die ***überladbarer Unäroperatoren*** sind:
```csharp
+   -   !   ~   ++   --   true   false
```

Obwohl `true` und `false` nicht explizit in Ausdrücken verwendet werden (und sind daher nicht in der Rangfolgentabelle in enthalten [Operatorrangfolge und Assoziativität](expressions.md#operator-precedence-and-associativity)), diese werden als Operatoren betrachtet, da sie sich befinden in mehrere ausdruckskontexten aufgerufen: boolesche Ausdrücke ([boolesche Ausdrücke](expressions.md#boolean-expressions)) und im Zusammenhang mit der bedingten Ausdrücke ([Bedingungsoperator](expressions.md#conditional-operator)), und bedingte logische Operatoren ([bedingten logischen Operatoren](expressions.md#conditional-logical-operators)).

Die ***Überladbare binäre Operatoren*** sind:
```csharp
+   -   *   /   %   &   |   ^   <<   >>   ==   !=   >   <   >=   <=
```

Nur die oben aufgeführten Operatoren können überladen werden. Insbesondere ist es nicht möglich, Memberzugriff, Methodenaufruf zu überladen oder `=`, `&&`, `||`, `??`, `?:`, `=>`, `checked`, `unchecked`, `new`, `typeof`, `default`, `as`, und `is` Operatoren.

Wenn ein binärer Operator überladen ist, wird der zugehörige Zuweisungsoperator, sofern er vorhanden ist, auch implizit überladen. Angenommen, eine Überladung des Operators `*` ist auch eine Überladung des Operators `*=`. Dies wird weiter in [Verbundzuweisung](expressions.md#compound-assignment). Beachten Sie, dass der Zuweisungsoperator selbst (`=`) kann nicht überladen werden. Eine Zuweisung immer führt eine einfache bitweise Kopie eines Werts in einer Variablen an.

Wandeln Sie die Vorgänge, z. B. `(T)x`, werden durch die Bereitstellung von benutzerdefinierten Konvertierungen überladen ([benutzerdefinierte Konvertierungen](conversions.md#user-defined-conversions)).

Element zuzugreifen, z. B. `a[x]`, einen überladbaren Operators wird nicht berücksichtigt. Benutzerdefinierte Indizierung wird jedoch unterstützt über Indexer ([Indexer](classes.md#indexers)).

In Ausdrücken werden Operatoren mithilfe der Operatornotation und in Deklarationen, Operatoren sind verwiesen funktionale Notation verwenden. Die folgende Tabelle zeigt die Beziehung zwischen den Operator und funktionalen Notationen für unären und binären Operatoren. In den ersten Eintrag *Op* kennzeichnet alle überladbarer unärer Präfix-Operator. In den zweiten Eintrag *Op* kennzeichnet das Postfix unäre `++` und `--` Operatoren. Im dritten Eintrag *Op* kennzeichnet alle überladbaren binären Operator.


| __Operator-notation__ | __Funktionale notation__ |
|-----------------------|-------------------------|
| `op x`                | `operator op(x)`        | 
| `x op`                | `operator op(x)`        | 
| `x op y`              | `operator op(x,y)`      | 

Eine benutzerdefinierte Operatordeklarationen erfordern immer mit mindestens einer der Parameter des Typs Klasse oder Struktur sein, die sich die Operatordeklaration enthält. Daher ist es nicht möglich, für einen benutzerdefinierten Operator auf die gleiche Signatur wie einen vordefinierten Operator haben.

Benutzerdefinierte Deklarationen können nicht der Syntax, die Rangfolge oder die Assoziativität von einem Operator geändert werden. Z. B. die `/` -Operators ist immer ein binärer Operator, immer wurde die rangfolgenebene angegeben [Operatorrangfolge und Assoziativität](expressions.md#operator-precedence-and-associativity), und ist immer linksassoziativ.

Es ist, zwar möglich, dass ein benutzerdefinierter Operator eine Berechnung ausführen, die es pleases sind Implementierungen, die Ergebnisse nicht zu erzeugen, die intuitiv erwartet werden dringend abgeraten. Zum Beispiel eine Implementierung von `operator ==` sollten die beiden Operanden hinsichtlich ihrer Gleichheit und Zurückgeben eines entsprechenden `bool` Ergebnis.

Die Beschreibungen der einzelnen Operatoren in [primärausdrücke](expressions.md#primary-expressions) über [bedingten logischen Operatoren](expressions.md#conditional-logical-operators) Geben Sie die vordefinierte Implementierungen von Operatoren und alle zusätzlichen Regeln, die angewendet werden zu jedem Operator. Stellen Sie die Beschreibungen der Begriffe verwenden ***überladungsauflösung für unäroperator***, ***binärer Operator überladungsauflösung***, und ***numerische heraufstufung***, Definitionen sind finden Sie in den folgenden Abschnitten.

### <a name="unary-operator-overload-resolution"></a>Überladungsauflösung für Unären operator

Ein Vorgang des Formulars `op x` oder `x op`, wobei `op` ein überladbarer unärer Operator, und `x` ist ein Ausdruck vom Typ `X`, wird wie folgt verarbeitet:

*  Der Satz von Kandidaten benutzerdefinierten Operatoren, die mit der gebotenen `X` für den Vorgang `operator op(x)` wird bestimmt, wobei die Regeln der [Candidate benutzerdefinierte Operatoren](expressions.md#candidate-user-defined-operators).
*  Wenn der Satz von Kandidaten benutzerdefinierte Operatoren nicht leer ist, wird dies die Gruppe der Kandidaten-Operatoren für den Vorgang. Andernfalls die vordefinierten unären `operator op` Implementierungen, einschließlich deren angehobene Forms, werden der Satz von Kandidaten-Operatoren für den Vorgang. Die vordefinierte Implementierung eines bestimmten Operators werden in der Beschreibung des Operators angegeben ([primärausdrücke](expressions.md#primary-expressions) und [unäre Operatoren](expressions.md#unary-operators)).
*  Die Regeln der überladungsauflösung des [Überladungsauflösung](expressions.md#overload-resolution) gelten für den Satz von Kandidaten-Operatoren, um die beste Operator in Bezug auf die Argumentliste auszuwählen `(x)`, und dieser Operator wird das Ergebnis der Überladung der Auflösungsvorgang. Überladungsauflösung keinen einzelnen bewährte Operator auswählen, tritt ein Fehler während der Bindung.

### <a name="binary-operator-overload-resolution"></a>Binärer Operator Auflösung von funktionsüberladungen

Ein Vorgang des Formulars `x op y`, wobei `op` ist ein Überladbarer binärer Operator `x` ist ein Ausdruck vom Typ `X`, und `y` ist ein Ausdruck vom Typ `Y`, wird wie folgt verarbeitet:

*  Der Satz von Kandidaten benutzerdefinierten Operatoren, die mit der gebotenen `X` und `Y` für den Vorgang `operator op(x,y)` bestimmt ist. Der Satz besteht aus der Union der Kandidat Operatoren gebotenen `X` und die Candidate-Operatoren, die von bereitgestellte `Y`, jeweils festgelegt, wobei die Regeln der [Candidate benutzerdefinierte Operatoren](expressions.md#candidate-user-defined-operators). Wenn `X` und `Y` denselben Typ aufweisen, oder wenn `X` und `Y` über einen gemeinsamen Basistyp abgeleitet werden, und klicken Sie dann gemeinsam genutzte potentielle Operatoren nur einmal in der kombinierten Gruppe vorkommen.
*  Wenn der Satz von Kandidaten benutzerdefinierte Operatoren nicht leer ist, wird dies die Gruppe der Kandidaten-Operatoren für den Vorgang. Andernfalls die vordefinierten Binärdatei `operator op` Implementierungen, einschließlich deren angehobene Forms, werden der Satz von Kandidaten-Operatoren für den Vorgang. Die vordefinierte Implementierung eines bestimmten Operators werden in der Beschreibung des Operators angegeben ([arithmetische Operatoren](expressions.md#arithmetic-operators) über [bedingten logischen Operatoren](expressions.md#conditional-logical-operators)). Für vordefinierte für Enumerationen und Delegaten-Operatoren sind Operatoren die nur berücksichtigt die durch eine Enumeration oder Delegat-Typ, der der Bindung-Time-Typ, der einer der Operanden ist definierten.
*  Die Regeln der überladungsauflösung des [Überladungsauflösung](expressions.md#overload-resolution) gelten für den Satz von Kandidaten-Operatoren, um die beste Operator in Bezug auf die Argumentliste auszuwählen `(x,y)`, und dieser Operator wird das Ergebnis der Überladung der Auflösungsvorgang. Überladungsauflösung keinen einzelnen bewährte Operator auswählen, tritt ein Fehler während der Bindung.

### <a name="candidate-user-defined-operators"></a>Kandidat benutzerdefinierte Operatoren

Bei einem vorgegebenen Typ `T` und einen Vorgang `operator op(A)`, wobei `op` ist ein überladbaren Operators und `A` ist eine Argumentliste, die den Satz von Kandidaten benutzerdefinierten Operatoren, die von bereitgestellte `T` für `operator op(A)` richtet sich wie folgt:

*  Bestimmen Sie den Typ `T0`. Wenn `T` ist ein NULL-Werte zulässt, `T0` ist die zugrunde liegenden Typ und andernfalls `T0` gleich `T`.
*  Für alle `operator op` Deklarationen in `T0` und alle Arten von solcher Operatoren, aufgehoben wird, wenn mindestens ein Operator anwendbar ist ([Anwendbarer Funktionsmember](expressions.md#applicable-function-member)) in Bezug auf die Argumentliste `A`, klicken Sie dann den Satz von Kandidat Operatoren besteht aus solchen entsprechenden Operatoren in `T0`.
*  Andernfalls gilt: Wenn `T0` ist `object`, der Satz von Kandidaten-Operatoren ist leer.
*  Andernfalls der Satz von Kandidaten Standardabfrageoperatoren gebotenen `T0` ist ein Satz von Kandidaten-Operatoren, die durch die direkte Basisklasse bereitgestellt `T0`, oder die effektive Basisklasse `T0` Wenn `T0` ein Parameter vom Typ ist.

### <a name="numeric-promotions"></a>Numerische heraufstufungen

Numerische heraufstufung besteht aus automatisch bestimmte implizite Konvertierungen von Operanden der unären und binären numerischen Operatoren für Sie ausgeführt. Numerische heraufstufung ist nicht auf, eine unterschiedliche Methode, sondern vielmehr eine Auswirkung der Auflösung von funktionsüberladungen auf die vordefinierten Operatoren angewendet. Numerische heraufstufung wirkt speziell Auswertung von benutzerdefinierten Operatoren, sich nicht auch benutzerdefinierte Operatoren weisen ähnliche Auswirkungen implementiert werden können.

Ein Beispiel für numerische heraufstufung, sollten Sie die vordefinierte Implementierungen der Binärdatei `*` Operator:

```csharp
int operator *(int x, int y);
uint operator *(uint x, uint y);
long operator *(long x, long y);
ulong operator *(ulong x, ulong y);
float operator *(float x, float y);
double operator *(double x, double y);
decimal operator *(decimal x, decimal y);
```

Wenn Regeln zur überladungsauflösung ([Auflösen der Überladung](expressions.md#overload-resolution)) gelten für diesen Satz von Operatoren, der Effekt ist, die Operandentypen auswählen, das erste der Operatoren für die implizite Konvertierungen vorhanden sind. Z. B. für den Vorgang `b * s`, wobei `b` ist eine `byte` und `s` ist eine `short`, überladen Sie die Lösung wählt `operator *(int,int)` als bester Operator. Daher das wird also `b` und `s` werden in konvertiert `int`, und der Typ des Ergebnisses ist `int`. Ebenso für den Vorgang `i * d`, wobei `i` ist ein `int` und `d` ist eine `double`, überladen Sie die Lösung wählt `operator *(double,double)` als bester Operator.

#### <a name="unary-numeric-promotions"></a>Unäre numerischen heraufstufungen

Unäre numerische heraufstufung tritt auf, für den Operanden des vordefinierten `+`, `-`, und `~` unäre Operatoren. Unäre numerische heraufstufung besteht nur aus der Konvertierung von Operanden des Typs `sbyte`, `byte`, `short`, `ushort`, oder `char` eingeben `int`. Darüber hinaus für den unären `-` Operator, Unäre numerische heraufstufung konvertiert Operanden vom Typ `uint` eingeben `long`.

#### <a name="binary-numeric-promotions"></a>Binäre numerische heraufstufungen

Binäre numerische heraufstufung tritt auf, für den Operanden des vordefinierten `+`, `-`, `*`, `/`, `%`, `&`, `|`, `^`, `==`, `!=`, `>`, `<`, `>=`, und `<=` binären Operatoren. Binäre numerische heraufstufung konvertiert beide Operanden implizit in einen gemeinsamen Typ, der bei der nicht-relationalen Operatoren der Ergebnistyp des Vorgangs wird. Binäre numerische heraufstufung besteht aus der Anwendung der folgenden Regeln, in der Reihenfolge, die sie hier angezeigt werden:

*  Wenn ein Operand vom Typ `decimal`, der andere Operand wird in den Typ konvertiert `decimal`, oder es sich bei ein Fehler während der Datenbindung tritt auf, wenn der andere Operand vom Typ `float` oder `double`.
*  Andernfalls, wenn ein Operand vom Typ `double`, der andere Operand wird in den Typ konvertiert `double`.
*  Andernfalls, wenn ein Operand vom Typ `float`, der andere Operand wird in den Typ konvertiert `float`.
*  Andernfalls, wenn ein Operand vom Typ `ulong`, der andere Operand wird in den Typ konvertiert `ulong`, oder es sich bei ein Fehler während der Datenbindung tritt auf, wenn der andere Operand vom Typ `sbyte`, `short`, `int`, oder `long`.
*  Andernfalls, wenn ein Operand vom Typ `long`, der andere Operand wird in den Typ konvertiert `long`.
*  Andernfalls, wenn ein Operand vom Typ `uint` und der andere Operand ist vom Typ `sbyte`, `short`, oder `int`, beide Operanden sind in den Typ konvertiert `long`.
*  Andernfalls, wenn ein Operand vom Typ `uint`, der andere Operand wird in den Typ konvertiert `uint`.
*  Andernfalls werden beide Operanden Typ konvertiert `int`.

Beachten Sie, dass die erste Regel keine Vorgänge lässt nicht zu, die Mischen der `decimal` Geben Sie mit der `double` und `float` Typen. Die Regel ergibt sich aus der Tatsache, die es keine impliziten Konvertierungen zwischen gibt den `decimal` Typ und die `double` und `float` Typen.

Beachten Sie außerdem, dass es nicht möglich, dass ein Operand vom Typ `ulong` bei der andere Operand ist, der einen ganzzahligen Typ mit Vorzeichen. Der Grund ist, dass kein ganzzahliger Typ, der vorhanden ist kann das gesamte Spektrum darstellen `ulong` sowie die ganzzahligen Typen mit Vorzeichen.

In beiden oben genannten Fällen kann ein Cast-Ausdruck verwendet werden, zu einer der Operanden explizit in einen Typ zu konvertieren, die mit der andere Operand kompatibel ist.

Im Beispiel
```csharp
decimal AddPercent(decimal x, double percent) {
    return x * (1.0 + percent / 100.0);
}
```
ein Fehler während der Datenbindung tritt auf, weil eine `decimal` kann nicht multipliziert eine `double`. Der Fehler wurde behoben, durch explizite Konvertierung der zweite Operand `decimal`wie folgt:

```csharp
decimal AddPercent(decimal x, double percent) {
    return x * (decimal)(1.0 + percent / 100.0);
}
```

### <a name="lifted-operators"></a>Transformierten Operatoren

***Operatoren angehoben*** zulassen von vordefinierten und benutzerdefinierten Operatoren, die Vorgänge an nicht auf NULL festlegbare Werttypen auch mit NULL-Werte zulassen Forms dieser Typen verwendet werden soll. Transformierten Operatoren bestehen aus vordefinierten und benutzerdefinierten Operatoren, die Anforderungen erfüllen, wie im folgenden beschrieben:

*   Für die Unäroperatoren

    ```csharp
    +  ++  -  --  !  ~
    ```

    eine angehobene Form eines Operators ist vorhanden, wenn die Operanden und Ergebnis beide nicht auf NULL festlegbare Werttypen sind. Die transformierten Form wird durch Hinzufügen eines einzelnen erstellt `?` Modifizierer, um die Typen von Operanden und Ergebnis. Der transformierten Operator erzeugt einen null-Wert, wenn der Operand null ist. Der transformierten Operator hingegen entpackt der Operand, der zugrunde liegenden-Operator angewendet und bindet das Ergebnis.

*   Für die binären Operatoren

    ```csharp
    +  -  *  /  %  &  |  ^  <<  >>
    ```

    eine angehobene Form eines Operators ist vorhanden, wenn die Operanden und Ergebnis alle nicht auf NULL festlegbare Werttypen sind. Die transformierten Form wird durch Hinzufügen eines einzelnen erstellt `?` Modifizierer für jeden Operanden und Ergebnis. Der transformierten Operator erzeugt einen null-Wert, wenn eine oder beide Operanden null sind (eine Ausnahme wird die `&` und `|` Operatoren mit den `bool?` eingeben, wie in beschrieben [Boolesche logische Operatoren](expressions.md#boolean-logical-operators)). Andernfalls der transformierten Operator entpackt die Operanden der zugrunde liegenden-Operator angewendet und dient als Wrapper für das Ergebnis.

*   Für die Gleichheitsoperatoren

    ```csharp
    ==  !=
    ```

    eine angehobene Form der Bediener vorhanden ist, wenn die Operandentypen sind nicht auf NULL festlegbare Werttypen und ist der Ergebnistyp `bool`. Die transformierten Form wird durch Hinzufügen eines einzelnen erstellt `?` Modifizierer für die einzelnen Operanden. Der transformierten Operator betrachtet, zwei null-Werte identisch sind, und ein null-Wert auf einen beliebigen nicht-Null-Wert ungleich. Wenn beide Operanden ungleich Null sind, wird der transformierten-Operator entpackt die Operanden und wendet den zugrunde liegenden Operator zum Erzeugen der `bool` Ergebnis.

*   Für die relationalen Operatoren

    ```csharp
    <  >  <=  >=
    ```

    eine angehobene Form der Bediener vorhanden ist, wenn die Operandentypen sind nicht auf NULL festlegbare Werttypen und ist der Ergebnistyp `bool`. Die transformierten Form wird durch Hinzufügen eines einzelnen erstellt `?` Modifizierer für die einzelnen Operanden. Die transformierten Operator gibt den Wert `false` Wenn einer oder beide der Operanden null sind. Andernfalls den transformierten Operator entpackt die Operanden und gilt den zugrunde liegenden Operator erzeugt die `bool` Ergebnis.

## <a name="member-lookup"></a>Suche nach Membern

Eine Suche nach Membern ist der Prozess, bei dem die Bedeutung eines Namens im Kontext eines Typs bestimmt wird. Eine Suche nach Membern kann auftreten, als Teil der Auswertung einer *Simple_name* ([einfache Namen](expressions.md#simple-names)) oder ein *Member_access* ([Memberzugriff](expressions.md#member-access)) in ein -Ausdruck. Wenn die *Simple_name* oder *Member_access* tritt ein, wenn die *Primary_expression* von einem *Invocation_expression* ([ Methodenaufrufe](expressions.md#method-invocations)), gilt der Member aufgerufen werden.

Wenn ein Member eine Methode oder ein Ereignis ist, oder wenn es sich um Konstanten, Felder oder Eigenschaften entweder einen Delegattyp handelt ([Delegaten](delegates.md)) oder den Typ `dynamic` ([der dynamische Typ](types.md#the-dynamic-type)), und klicken Sie dann das Element wird als bezeichnet *aufrufbare*.

Suche nach Membern berücksichtigt nicht nur den Namen der ein Element, sondern auch die Anzahl von Typparametern, die das Element und gibt an, ob der Member zugegriffen werden. Für die Zwecke der Suche nach Membern generische Methoden und geschachtelten generischen Typen haben die Anzahl von Typparametern, die in ihren jeweiligen Deklarationen angegeben, und alle anderen Elemente haben keine Typparameter.

Eine Suche nach Membern des Namens eines `N` mit `K`  Typ von Parametern in einem Typ `T` wird wie folgt verarbeitet:

*  Zuerst, eine Reihe von zugängliche Member, die mit dem Namen `N` bestimmt wird:
    * Wenn `T` Typparameter ist ein, und klicken Sie dann die Gruppe der Union der Sätze von zugängliche Member, die mit dem Namen ist `N` in jedem der Typen, die als sekundäre Einschränkung oder primary-Einschränkung angegeben ([Geben Sie die Einschränkungen für Typparameter](classes.md#type-parameter-constraints)) für  `T`, sowie die zugängliche Member, die mit dem Namen `N` in `object`.
    * Andernfalls der Satz besteht aus allen zugegriffen werden kann ([Memberzugriff](basic-concepts.md#member-access)) Elemente, die mit dem Namen `N` in `T`, einschließlich der geerbte Member und den zugängliche Member mit dem Namen `N` in `object`. Wenn `T` ein konstruierter Typ ist, wird die Menge der Elemente wird abgerufen, indem Typargumente ersetzen, wie in beschrieben [Mitglieder der konstruierte Typen](classes.md#members-of-constructed-types). Elemente, die enthalten eine `override` Modifizierer aus der Gruppe ausgeschlossen werden.
*  Als Nächstes If `K` ist 0 (null), alle geschachtelten Typen, deren Deklarationen enthalten die Typparameter, werden entfernt. Wenn `K` nicht gleich NULL ist, alle Elemente mit einer anderen Anzahl von Parametern entfernt werden, Typ. Hinweis: Wenn `K` ist NULL, Methoden mit einem Typ, die Parameter nicht werden, da durch die Herleitung Typ entfernt werden ([Typrückschluss](expressions.md#type-inference)) möglicherweise die Typargumente abzuleiten.
*  Weiter, wenn der Member ist *aufgerufen*, allen nicht-*aufrufbare* Mitglieder aus dem Satz entfernt werden.
*  Als Nächstes werden die Elemente, die durch andere Member ausgeblendet werden aus der Gruppe entfernt. Für jedes Element `S.M` in der Gruppe, in denen `S` ist der Typ, in dem das Element `M` deklariert ist, werden die folgenden Regeln angewendet:
    * Wenn `M` ist eine Konstante, ein Feld, Eigenschaft, Ereignis oder -Enumerationsmember, alle Elemente in dem Basistyp deklariert `S` aus der Gruppe entfernt werden.
    * Wenn `M` ist eine Typdeklaration, dann alle nicht-Typen, die deklariert in einem Basistyp von `S` werden aus der Gruppe, entfernt und alle Typdeklarationen sind mit der gleichen Anzahl von Typparametern als `M` , die in dem Basistyp deklariert `S` werden entfernt aus dem Satz.
    * Wenn `M` ist eine Methode, alle Nichtmethode-Member in dem Basistyp deklariert `S` aus der Gruppe entfernt werden.
*  Als Nächstes werden Schnittstellenmember, die von der Klasse, Elemente ausgeblendet werden aus dem Satz entfernt werden. Dieser Schritt ist nur wirksam, wenn `T` Typparameter und `T` hat sowohl eine effektive Basisklasse außer `object` und eine effektive nicht leere-Schnittstelle, die festgelegt ([Geben Sie die Einschränkungen für Typparameter](classes.md#type-parameter-constraints)). Für jedes Element `S.M` in der Gruppe, in denen `S` ist der Typ, in dem das Element `M` deklariert ist, werden die folgenden Regeln angewendet werden, wenn `S` ist eine Deklaration der Klasse nicht `object`:
    * Wenn `M` ist eine Konstante, ein Feld, Eigenschaft, Ereignis, Enumerationsmember oder Typdeklaration, und klicken Sie dann alle Member, die in der Schnittstellendeklaration einer deklariert, die aus dem Satz entfernt werden.
    * Wenn `M` ist eine Methode, alle nicht-Method-Elemente, die in der Schnittstellendeklaration einer deklariert entfernt werden, aus dem Satz und alle Methoden, mit der gleichen Signatur wie `M` deklariert in einer Schnittstelle Deklaration aus dem Satz entfernt werden.
*  Dass werden ausgeblendete Elemente entfernt, wird schließlich das Ergebnis der Suche bestimmt werden:
    * Wenn die Gruppe ein einzelnes Element, die keine Methode ist besteht, dann ist dieser Member das Ergebnis der Suche.
    * Andernfalls, wenn der Satz nur Methoden enthält, ist diese Gruppe von Methoden das Ergebnis der Suche.
    * Andernfalls die Suche ist nicht eindeutig, und beim Auftreten eines Laufzeitfehlers-Bindung.

Für Member-Suchvorgänge in andere Typen als Parameter vom Typ und die Schnittstellen und Elementsuchvorgänge in Schnittstellen, die ausschließlich mit einfacher Vererbung (jede Schnittstelle in der Vererbungskette weist genau Null oder eine direkte Basisschnittstelle), wird die Suche nach Regeln einfach abgeleitet, die Member ausblenden Basis Member mit demselben Namen bzw. derselben Signatur. Diese Suchvorgänge mit einfacher Vererbung sind stets eindeutig. Die Mehrdeutigkeiten, der aus Element-Suchvorgängen in mehrere Vererbungen Schnittstellen möglicherweise auftreten können, werden in beschrieben [Schnittstelle Memberzugriff](interfaces.md#interface-member-access).

### <a name="base-types"></a>Basistypen

Zum Zweck der Suche nach Membern, einen Typ `T` gilt folgenden Basistypen haben:

*  Wenn `T` ist `object`, klicken Sie dann `T` keinen Basistyp hat.
*  Wenn `T` ist ein *Enum_type*, der Basistypen des `T` Klassentypen sind `System.Enum`, `System.ValueType`, und `object`.
*  Wenn `T` ist eine *Struct_type*, der Basistypen des `T` Klassentypen sind `System.ValueType` und `object`.
*  Wenn `T` ist eine *Class_type*, der Basistypen des `T` sind die Basisklassen von `T`, einschließlich des Klassentyps `object`.
*  Wenn `T` ist ein *Interface_type*, der Basistypen des `T` sind die Basisschnittstellen von `T` und den Klassentyp `object`.
*  Wenn `T` ist ein *Array_type*, der Basistypen des `T` Klassentypen sind `System.Array` und `object`.
*  Wenn `T` ist eine *Delegate_type*, der Basistypen des `T` Klassentypen sind `System.Delegate` und `object`.

## <a name="function-members"></a>Funktionsmember

Funktionsmember sind Elemente, die ausführbaren Anweisungen enthalten. Funktionsmember sind immer Member von Typen und Member von Namespaces nicht möglich. C# -Code definiert die folgenden Kategorien von Funktionsmembern:

*  Methoden
*  Eigenschaften
*  Ereignisse
*  Indexer
*  Benutzerdefinierte Operatoren
*  Instanzkonstruktoren
*  Statische Konstruktoren
*  Destruktoren

Mit Ausnahme von Destruktoren und statische Konstruktoren (die explizit nicht aufgerufen werden können), werden die Anweisungen im Funktionsmember über Funktionsaufrufe-Element ausgeführt. Die eigentliche Syntax für das Schreiben von einem Funktionsaufruf für das Element abhängig ist, auf die Memberkategorie bestimmte Funktion.

Die Argumentliste ([Argumentlisten](expressions.md#argument-lists)) eines Elements für die Funktion bietet Aufruf tatsächliche Werte oder Variablenverweise für die Parameter, der das Funktionselement.

Aufrufe von generischen Methoden können es sich um den Typrückschluss, um zu bestimmen, den Satz der Argumente des Typs der Methode zu übergebenden einsetzen. Dieser Prozess wird hier beschrieben [Typrückschluss](expressions.md#type-inference).

Aufrufe von Methoden, Indexer, Operatoren und Instanzkonstruktoren nutzen die Auflösung von funktionsüberladungen ermitteln, welche verschiedenen möglichen Funktionsmember aufrufen. Dieser Prozess wird hier beschrieben [Überladungsauflösung](expressions.md#overload-resolution).

Nachdem eine bestimmte Funktionsmember zum Zeitpunkt der Bindung identifiziert wurde, möglicherweise über die Auflösung von funktionsüberladungen, der tatsächliche Laufzeit-Prozess des Aufrufs der Funktionsmember der Member wird beschrieben in [Überprüfungen zur Kompilierzeit der dynamischen überladungsauflösung](expressions.md#compile-time-checking-of-dynamic-overload-resolution).

Die folgende Tabelle enthält die Verarbeitung, die findet in Konstrukten, die im Zusammenhang mit den sechs Kategorien Funktionsmember, die explizit aufgerufen werden können. In der Tabelle `e`, `x`, `y`, und `value` klassifizierte Ausdrücke als Variablen oder Werte, `T` gibt einen Ausdruck, der als Typ klassifiziert `F` ist der einfache Name einer Methode, und klicken Sie auf `P` ist der einfache Name einer Eigenschaft.


| __Erstellen__     | __Beispiel__    | __Beschreibung__ |
|-------------------|----------------|-----------------|
| Methodenaufruf | `F(x,y)`       | Überladungsauflösung wird angewendet, um die beste Methode wählen `F` in der enthaltenden Klasse oder Struktur. Die Methode wird aufgerufen, mit der Argumentliste `(x,y)`. Wenn die Methode nicht `static`, ist der Instanzausdruck `this`. | 
|                   | `T.F(x,y)`     | Überladungsauflösung wird angewendet, um die beste Methode wählen `F` in der Klasse oder Struktur `T`. Ein Fehler während der Datenbindung tritt auf, wenn die Methode nicht `static`. Die Methode wird aufgerufen, mit der Argumentliste `(x,y)`. | 
|                   | `e.F(x,y)`     | Überladungsauflösung wird angewendet, um die beste Methode F wählen Sie die Klasse, Struktur oder Schnittstelle, die durch den Typ des angegebenen `e`. Ein Fehler während der Datenbindung tritt auf, wenn die Methode `static`. Die Methode wird aufgerufen, mit dem Instanzausdruck `e` und deren Argumentliste `(x,y)`. | 
| Eigenschaftenzugriff   | `P`            | Die `get` -Accessor der Eigenschaft `P` in der enthaltenden Klasse oder Struktur wird aufgerufen. Ein Fehler während der Kompilierung tritt auf, wenn `P` ist lesegeschützt. Wenn `P` nicht `static`, ist der Instanzausdruck `this`. | 
|                   | `P = value`    | Die `set` -Accessor der Eigenschaft `P` in der enthaltenden Klasse oder Struktur wird mit der Argumentliste aufgerufen `(value)`. Ein Fehler während der Kompilierung tritt auf, wenn `P` ist schreibgeschützt. Wenn `P` nicht `static`, ist der Instanzausdruck `this`. | 
|                   | `T.P`          | Die `get` -Accessor der Eigenschaft `P` in der Klasse oder Struktur `T` aufgerufen wird. Ein Fehler während der Kompilierung tritt auf, wenn `P` nicht `static` oder, wenn `P` ist lesegeschützt. | 
|                   | `T.P = value`  | Die `set` -Accessor der Eigenschaft `P` in der Klasse oder Struktur `T` wird aufgerufen, mit der Argumentliste `(value)`. Ein Fehler während der Kompilierung tritt auf, wenn `P` nicht `static` oder, wenn `P` ist schreibgeschützt. | 
|                   | `e.P`          | Die `get` -Accessor der Eigenschaft `P` in der Klasse, Struktur oder Schnittstelle, die durch den Typ des angegebenen `e` wird aufgerufen, mit dem Instanzausdruck `e`. Ein Fehler während der Datenbindung tritt auf, wenn `P` ist `static` oder, wenn `P` ist lesegeschützt. | 
|                   | `e.P = value`  | Die `set` -Accessor der Eigenschaft `P` in der Klasse, Struktur oder Schnittstelle, die durch den Typ des angegebenen `e` wird aufgerufen, mit dem Instanzausdruck `e` und deren Argumentliste `(value)`. Ein Fehler während der Datenbindung tritt auf, wenn `P` ist `static` oder, wenn `P` ist schreibgeschützt. | 
| Ereigniszugriff      | `E += value`   | Die `add` Accessor des Ereignisses `E` in der enthaltenden Klasse oder Struktur wird aufgerufen. Wenn `E` ist nicht statisch ist, der Instanzausdruck gibt `this`. | 
|                   | `E -= value`   | Die `remove` Accessor des Ereignisses `E` in der enthaltenden Klasse oder Struktur wird aufgerufen. Wenn `E` ist nicht statisch ist, der Instanzausdruck gibt `this`. | 
|                   | `T.E += value` | Die `add` Accessor des Ereignisses `E` in der Klasse oder Struktur `T` aufgerufen wird. Ein Fehler während der Datenbindung tritt auf, wenn `E` ist nicht statisch. | 
|                   | `T.E -= value` | Die `remove` Accessor des Ereignisses `E` in der Klasse oder Struktur `T` aufgerufen wird. Ein Fehler während der Datenbindung tritt auf, wenn `E` ist nicht statisch. | 
|                   | `e.E += value` | Die `add` Accessor des Ereignisses `E` in der Klasse, Struktur oder Schnittstelle, die durch den Typ des angegebenen `e` wird aufgerufen, mit dem Instanzausdruck `e`. Ein Fehler während der Datenbindung tritt auf, wenn `E` ist statisch. | 
|                   | `e.E -= value` | Die `remove` Accessor des Ereignisses `E` in der Klasse, Struktur oder Schnittstelle, die durch den Typ des angegebenen `e` wird aufgerufen, mit dem Instanzausdruck `e`. Ein Fehler während der Datenbindung tritt auf, wenn `E` ist statisch. | 
| Indexerzugriff    | `e[x,y]`       | Auflösung von funktionsüberladungen wird angewendet, um den besten Indexer in der-Klasse, Struktur oder Schnittstelle erhalten durch den Typ des e auszuwählen. Die `get` Accessor des Indexers aufgerufen, mit dem Instanzausdruck `e` und deren Argumentliste `(x,y)`. Ein Fehler während der Datenbindung tritt auf, wenn der Indexer nur Schreibzugriff ist. | 
|                   | `e[x,y] = value` | Auflösung von funktionsüberladungen zur angewendet wird, wählen Sie den besten Indexer in der Klasse, Struktur oder Schnittstelle, die durch den Typ des angegebenen `e`. Die `set` Accessor des Indexers aufgerufen, mit dem Instanzausdruck `e` und deren Argumentliste `(x,y,value)`. Ein Fehler während der Datenbindung tritt auf, wenn der Indexer schreibgeschützt ist. | 
| Operator-Aufruf | `-x`         | Auflösung von funktionsüberladungen zur angewendet wird, wählen Sie den besten unäroperator in der Klasse oder Struktur, die durch den Typ des angegebenen `x`. Der ausgewählte Operator wird aufgerufen, mit der Argumentliste `(x)`. | 
|                     | `x + y`      | Auflösung von funktionsüberladungen zur angewendet wird, wählen Sie den am besten binären Operator in Klassen oder Strukturen, die von den Typen der angegebenen `x` und `y`. Der ausgewählte Operator wird aufgerufen, mit der Argumentliste `(x,y)`. | 
| Instanz-Konstruktoraufruf | `new T(x,y)` | Überladungsauflösung wird angewendet, um den besten Instanzkonstruktor wählen Sie in der Klasse oder Struktur `T`. Der Instanzkonstruktor wird aufgerufen, mit der Argumentliste `(x,y)`. | 

### <a name="argument-lists"></a>Argumentlisten

Jede Funktion Member und Delegataufruf enthält eine Argumentliste, die tatsächlichen Werte oder Variablenverweise für die Parameter, der das Funktionselement bereitstellt. Die Syntax zum Angeben der Argumentliste eines Aufrufs der Funktion Mitglied hängt davon ab, auf die Memberkategorie Funktion:

*  Z. B. Konstruktoren, Methoden, Indexer und Delegaten, die Argumente angegeben werden als eine *Argument_list*wie unten beschrieben. Für Indexer können beim Aufrufen der `set` -Accessor, die Argumentliste darüber hinaus enthält den Ausdruck, der als der Rechte Operand des Zuweisungsoperators angegeben.
*  Informationen zu Eigenschaften, die Argumentliste ist leer, beim Aufrufen der `get` Accessor und besteht aus den Ausdruck, der als der Rechte Operand des Zuweisungsoperators angegeben wird, beim Aufrufen der `set` Accessor.
*  Für Ereignisse, die Argumentliste des als der Rechte Operand des angegebenen Ausdrucks besteht aus den `+=` oder `-=` Operator.
*  Für benutzerdefinierte Operatoren besteht aus die Argumentliste den einzigen Operanden des unären Operators oder beiden Operanden des binären Operators.

Die Argumente von Eigenschaften ([Eigenschaften](classes.md#properties)), Ereignisse ([Ereignisse](classes.md#events)), und benutzerdefinierte Operatoren ([Operatoren](classes.md#operators)) werden immer als Parameter übergeben ([ -Wertparameter](classes.md#value-parameters)). Die Argumente von Indexern ([Indexer](classes.md#indexers)) werden immer als Parameter übergeben ([-Wertparameter](classes.md#value-parameters)) oder Parameterarrays ([Parameterarrays](classes.md#parameter-arrays)). Referenz und Ausgabeparameter werden für diese Kategorien Funktionsmember nicht unterstützt.

Die Argumente für einen Instanz-Konstruktor, Methode, Indexer oder Delegat Aufruf angegeben werden, als ein *Argument_list*:

```antlr
argument_list
    : argument (',' argument)*
    ;

argument
    : argument_name? argument_value
    ;

argument_name
    : identifier ':'
    ;

argument_value
    : expression
    | 'ref' variable_reference
    | 'out' variable_reference
    ;
```

Ein *Argument_list* besteht aus einem oder mehreren *Argument*s, die durch Kommas getrennt. Jedes Argument besteht aus einem optionalen *Argument_name* gefolgt von einem *Argument_value*. Ein *Argument* mit einer *Argument_name* wird als bezeichnet ein ***benanntes Argument***, während ein *Argument* ohne eine  *Argument_name* ist eine ***Positionsargument***. Es ist ein Fehler für positionelle Argumente nach der ein benanntes Argument in angezeigt werden ein *Argument_list*.

Die *Argument_value* kann einen der folgenden Formen annehmen:

*  Ein *Ausdruck*, der angibt, der das Argument als ein Value-Parameter übergeben wird ([-Wertparameter](classes.md#value-parameters)).
*  Das Schlüsselwort `ref` gefolgt von einem *Variable_reference* ([Variablenverweise](variables.md#variable-references)), der angibt, der das Argument als Verweisparameter übergeben wird ([Verweisparameter ](classes.md#reference-parameters)). Eine Variable definitiv zugewiesen werden muss ([definitive Zuweisung](variables.md#definite-assignment)), bevor es als Verweisparameter übergeben werden kann. Das Schlüsselwort `out` gefolgt von einem *Variable_reference* ([Variablenverweise](variables.md#variable-references)), der angibt, der das Argument als Output-Parameter übergeben wird ([Ausgabeparameter](classes.md#output-parameters)). Gilt als eine Variable definitiv zugewiesen ([definitive Zuweisung](variables.md#definite-assignment)) nach einem Member-Funktionsaufruf, in dem die Variable als Output-Parameter übergeben wird.

#### <a name="corresponding-parameters"></a>Entsprechenden Parameter

Für jedes Argument in einer Argumentliste es muss ein entsprechender Parameter in der Funktionsmember der Member oder der Delegat aufgerufen wird.

Die Parameterliste, die in der folgenden verwendet wird wie folgt bestimmt:

*  Virtuelle Methoden und Indexer, die in Klassen definiert die Parameterliste wird aus der Deklaration der spezifischste ausgewählt oder außer Kraft setzen, der das Funktionselement, beginnend mit den statischen Typ des Empfängers, und ihre Basisklassen zu durchsuchen.
*  Für Schnittstellenmethoden und Indexer die Liste ausgewählt ist die spezifischste Definition des Elements, beginnen mit dem Schnittstellentyp und das Durchsuchen von Basisschnittstellen bilden. Wenn keine unique-Parameter-Liste gefunden wird, wird eine Parameterliste kann nicht zugegriffen werden Namen und keine optionalen Parameter erstellt wird, sodass Aufrufe nicht benannte Parameter verwenden oder optionale Argumente auslassen.
*  Für partielle Methoden wird die Liste der definierenden Deklaration der partiellen Methode Parameter verwendet.
*  Ist es nur eine einzelner Parameter-Liste, die verwendet wird, für alle anderen Funktionsmember und Delegaten.

Die Position des Arguments oder Parameter ist als die Anzahl der Argumente oder Parameter in der Argumentliste oder Parameterliste vorhergehenden definiert.

Die entsprechenden Parameter für die Argumente der Funktion Mitglied werden wie folgt festgelegt:

*  Argumente in der *Argument_list* Instanzkonstruktoren, Methoden, Indexer und Delegaten:
    * Dieser Parameter entspricht ein Positionsargument, in ein fixed-Parameter an der gleichen Position in der Parameterliste auftritt.
    * Positionelle Argumente eines Members der Funktion mit einem Parameterarray, das aufgerufen wird, in seiner normalen Form entspricht das Parameterarray, die an der gleichen Position in der Parameterliste erfolgen muss.
    * Positionelle Argumente eines Members der Funktion mit einem Parameterarray, das aufgerufen wird, in der erweiterten Form, in denen keine festen Parameter an der gleichen Position in der Parameterliste auftritt, entspricht einem Element im Parameterarray.
    * Ein benanntes Argument entspricht der Parameter mit dem gleichen Namen in der Parameterliste.
    * Für Indexer können beim Aufrufen der `set` -Accessor, der Ausdruck angegeben, wie der Rechte Operand des Zuweisungsoperators impliziter entspricht `value` Parameter, der die `set` Zugriffsmethoden-Deklaration.
*  Informationen zu Eigenschaften, die beim Aufrufen der `get` Accessor gibt es keine Argumente sind. Beim Aufrufen der `set` -Accessor, der Ausdruck angegeben, wie der Rechte Operand des Zuweisungsoperators impliziter entspricht `value` Parameter, der die `set` Zugriffsmethoden-Deklaration.
*  Der einzige Parameter der Operatordeklaration entspricht der einzelne Operanden aus, für den benutzerdefinierten unäre Operatoren (einschließlich Konvertierungen).
*  Für eine benutzerdefinierte binäre Operatoren der linke Operand entspricht des ersten Parameters, und der Rechte Operand des zweiten Parameters der Operator-Deklaration entspricht.

#### <a name="run-time-evaluation-of-argument-lists"></a>Laufzeitauswertung von Argumentenlisten

Bei der Verarbeitung zur Laufzeit eine Member-Funktionsaufruf ([Überprüfungen zur Kompilierzeit der dynamischen überladungsauflösung](expressions.md#compile-time-checking-of-dynamic-overload-resolution)), die Ausdrücke oder Variablenverweise einer Argumentliste werden in der Reihenfolge von links nach rechts ausgewertet, als Die folgende:

*  Für eine Value-Parameter der Argumentausdruck ausgewertet und eine implizite Konvertierung ([implizite Konvertierungen](conversions.md#implicit-conversions)) an den entsprechenden Parameter ausgeführt wird. Der resultierende Wert wird der Anfangswert des Value-Parameters in der Aufruf an.
*  Für einen Verweis oder Ausgabe-Parameter der Variable Verweis ausgewertet wird und der daraus resultierende Speicherort der Speicherort, der durch den Parameter im Funktionsaufruf Element dargestellt wird. Der Variablenverweis als ein Verweis oder Ausgabe-Parameter ist ein Arrayelement einen *Reference_type*, eine laufzeitüberprüfung ausgeführt, um sicherzustellen, dass der Elementtyp des Arrays in den Typ des Parameters identisch ist. Wenn diese Überprüfung fehlschlägt, eine `System.ArrayTypeMismatchException` ausgelöst.

Methoden, Indexer und Instanzkonstruktoren deklarieren, können der am weitesten rechts befindlichen-Parameter ein Parameterarray sein ([Parameterarrays](classes.md#parameter-arrays)). Solche Funktionsmember werden aufgerufen, entweder in ihrer normalen Form oder in ihre erweiterte Form, die je nach geltenden ([Anwendbarer Funktionsmember](expressions.md#applicable-function-member)):

*  Wenn ein Funktionsmember mit einem Parameterarray in seiner normalen Form aufgerufen wird, muss das Argument für das Parameterarray erhält einen einzelnen Ausdruck, der implizit konvertiert werden kann ([implizite Konvertierungen](conversions.md#implicit-conversions)) in den Parameter-Arraytyp. In diesem Fall verhält sich das Parameterarray genau wie ein Value-Parameter.
*  Wenn ein Funktionsmember mit einem Parameterarray in erweiterter Form aufgerufen wird, muss der Aufruf NULL oder mehr positionelle Argumente für das Parameterarray, wobei jedes Argument ist ein Ausdruck, der implizit konvertiert werden angeben ([implizite Konvertierungen](conversions.md#implicit-conversions)), die den Elementtyp des Parameterarrays. In diesem Fall der Aufruf erstellt eine Instanz des Parameterarraytyps mit einer Länge, die Anzahl von Argumenten entspricht, initialisiert die Elemente der Arrayinstanz mit den Werten des angegebenen Arguments und verwendet die neu erstellte Array-Instanz als die tatsächliche Argument.

Die Ausdrücke einer Argumentliste werden immer in der Reihenfolge ausgewertet, die sie geschrieben werden. Das Beispiel
```csharp
class Test
{
    static void F(int x, int y = -1, int z = -2) {
        System.Console.WriteLine("x = {0}, y = {1}, z = {2}", x, y, z);
    }

    static void Main() {
        int i = 0;
        F(i++, i++, i++);
        F(z: i++, x: i++);
    }
}
```
erzeugt die Ausgabe
```
x = 0, y = 1, z = 2
x = 4, y = -1, z = 3
```

Die Array-Co Varianz-Regeln ([Array-Kovarianz](arrays.md#array-covariance)) zulassen den Wert eines Arraytyps `A[]` sollen einen Verweis auf eine Instanz des Arraytyps `B[]`, sofern eine implizite verweiskonvertierung von vorhanden ist `B` zu `A`. Aufgrund dieser Regeln, wenn ein Arrayelement einen *Reference_type* übergeben als Verweis oder Ausgabe, wird eine laufzeitüberprüfung erforderlich, um sicherzustellen, dass der tatsächliche Elementtyp des Arrays, des Parameters entspricht. Im Beispiel
```csharp
class Test
{
    static void F(ref object x) {...}

    static void Main() {
        object[] a = new object[10];
        object[] b = new string[10];
        F(ref a[0]);        // Ok
        F(ref b[1]);        // ArrayTypeMismatchException
    }
}
```
der zweite Aufruf von `F` bewirkt, dass eine `System.ArrayTypeMismatchException` ausgelöst, weil der tatsächliche Elementtyp des `b` ist `string` und nicht `object`.

Wenn ein Funktionsmember mit einem Parameterarray in erweiterter Form aufgerufen wird, wird der Aufruf verarbeitet, als ob ein Ausdruck zur Arrayerstellung mit einem Arrayinitialisierer ([Array-Ausdrücke für die Arrayerstellung](expressions.md#array-creation-expressions)) eingefügt wurde, um die Erweiterte Parameter. Z. B. im Falle folgender Deklaration
```csharp
void F(int x, int y, params object[] args);
```
die folgenden Aufrufe, der die erweiterte Form der Methode
```csharp
F(10, 20);
F(10, 20, 30, 40);
F(10, 20, 1, "hello", 3.0);
```
genau überein mit
```csharp
F(10, 20, new object[] {});
F(10, 20, new object[] {30, 40});
F(10, 20, new object[] {1, "hello", 3.0});
```

Beachten Sie insbesondere, dass ein leeres Array erstellt wird, wenn keine Argumente angegeben, für das Parameterarray vorhanden sind.

Wenn ein Funktionsmember mit entsprechenden optionalen Parametern Argumente ausgelassen werden, werden die Standardargumente von der Members-Funktionsdeklaration implizit übergeben. Da diese immer konstant sind, wirkt sich ihrer Auswertung nicht die Auswertungsreihenfolge der restlichen Argumente.

### <a name="type-inference"></a>Typrückschluss

Wenn eine generische Methode aufgerufen wird, ohne Angabe von Typargumenten, eine ***Typrückschluss*** Prozess versucht, die Typargumente für den Aufruf abzuleiten. Das Vorhandensein des Typrückschlusses können eine komfortablere Syntax zum Aufrufen einer generischen Methode verwendet werden soll, und ermöglicht es den Programmierer, vermeiden Sie redundante Informationen angeben. Betrachten Sie z. B. die Deklaration der Methode ein:
```csharp
class Chooser
{
    static Random rand = new Random();

    public static T Choose<T>(T first, T second) {
        return (rand.Next(2) == 0)? first: second;
    }
}
```
Es ist möglich, rufen Sie die `Choose` Methode ohne explizite Angabe ein Argument vom Typ:
```csharp
int i = Chooser.Choose(5, 213);                 // Calls Choose<int>

string s = Chooser.Choose("foo", "bar");        // Calls Choose<string>
```

Mithilfe des Typrückschlusses die Typargumente `int` und `string` werden aus den Argumenten der Methode bestimmt.

Der Typrückschluss tritt als Teil der Bindung-Time-Verarbeitung eines Methodenaufrufs ([Methodenaufrufe](expressions.md#method-invocations)) und erfolgt vor dem Schritt zur überladungsauflösung des Aufrufs. Wenn eine bestimmte Methode, die Gruppe in einem Methodenaufruf angegeben ist und keine Typargumente werden als Teil des Methodenaufrufs angegeben, wird ein Typrückschluss auf jede generische Methode in der Methodengruppe angewendet. Wenn Typrückschluss erfolgreich ist, werden die abgeleiteten Typargumente verwendet, um die Typen der Argumente für die nachfolgenden überladungsauflösung zu bestimmen. Wenn der Auflösung von funktionsüberladungen eine generische Methode wie das aufzurufende auswählt, werden dann die abgeleiteten Typargumente als die tatsächliche Typargumente für den Aufruf verwendet. Wenn Typrückschluss für eine bestimmte Methode fehlschlägt, diese Methode an der überladungsauflösung nicht teilnehmen. Der Ausfall des Typrückschlusses an und für sich selbst, bewirkt einen Fehler während der Bindung nicht. Allerdings führt es häufig zu einem Fehler während der Bindung bei der Auflösung von funktionsüberladungen funktioniert nicht mehr alle entsprechenden Methoden zu finden.

Wenn die angegebene Anzahl von Argumenten anders als die Anzahl von Parametern in der Methode ist, klicken Sie dann Typrückschluss schlägt sofort fehl. Andernfalls wird angenommen Sie, dass die generische Methode die folgende Signatur:
```csharp
Tr M<X1,...,Xn>(T1 x1, ..., Tm xm)
```

Durch einen Methodenaufruf des Formulars `M(E1...Em)` die Aufgabe des Typrückschlusses besteht darin, eindeutige Typargumente finden `S1...Sn` für jeden der Typparameter `X1...Xn` , damit der Aufruf `M<S1...Sn>(E1...Em)` gültig wird.

Bei der Ableitung der einzelnen Typparameter `Xi` ist entweder *festen* auf einen bestimmten Typ `Si` oder *unfixed* mit einem zugeordneten Satz *Grenzen*. Jede der Grenzen ist eine Art `T`. Zunächst jede Variable vom Typ `Xi` ist mit einem leeren Satz von Grenzen unfixed.

Typrückschluss erfolgt in mehreren Phasen. Jede Phase versucht, die Typargumente für weitere basierten auf die Ergebnisse der vorherigen Phase Variablen abzuleiten. In der erste Phase macht einige anfängliche Rückschlüsse ziehen, der den angegebenen Begrenzungen, während die zweite Phase Typvariablen auf bestimmte Typen korrigiert und weiter Grenzen leitet. Die zweite Phase möglicherweise werden eine Anzahl von Malen wiederholt.

*Hinweis*: Typ erfolgt Typrückschluss nicht nur, wenn eine generische Methode aufgerufen wird. Typrückschluss für die Konvertierung von Methodengruppen finden Sie im [Typrückschluss für die Konvertierung von Methodengruppen](expressions.md#type-inference-for-conversion-of-method-groups) und suchen den besten common-Typ, der einen Satz von Ausdrücken finden Sie im [Suchen des beste allgemeine Typs einer Gruppe Ausdrücke](expressions.md#finding-the-best-common-type-of-a-set-of-expressions).

#### <a name="the-first-phase"></a>Die erste phase

Für jede der Methodenargumente `Ei`:

*   Wenn `Ei` ist eine anonyme Funktion, ein *expliziter Parameter Typrückschluss* ([expliziter Parameter Typrückschlüsse](expressions.md#explicit-parameter-type-inferences)) besteht aus `Ei` auf `Ti`
*   Andernfalls gilt: Wenn `Ei` weist einen Typ `U` und `xi` ist ein Werteparameter ein *untere Begrenzung Typrückschluss* erfolgt *aus* `U` *zu* `Ti`.
*   Andernfalls gilt: Wenn `Ei` weist einen Typ `U` und `xi` ist eine `ref` oder `out` Parameter ein *genauer Typrückschluss* erfolgt *aus* `U` *zu* `Ti`.
*   Andernfalls erfolgt kein Rückschluss für dieses Argument.


#### <a name="the-second-phase"></a>Die zweite phase

In der zweite Phase wird wie folgt aus:

*   Alle *unfixed* Variablen des Typs `Xi` der nicht *richten sich nach* ([Abhängigkeit](expressions.md#dependence)) alle `Xj` haben eine feste ([Festsetzung von](expressions.md#fixing)).
*   Wenn kein solcher Typvariablen vorhanden sind, alle *unfixed* Variablen des Typs `Xi` sind *festen* für die alle der folgenden enthalten:
    *   Es ist mindestens ein Typvariable `Xj` das hängt vom `Xi`
    *   `Xi` verfügt über ein nicht leerer Satz von Grenzen
*   Wenn kein solcher Typvariablen vorhanden sind und es immer noch gibt *unfixed* Variablen des Typs, geben der Typrückschluss schlägt fehl.
*   Andernfalls, wenn keine weiteren *unfixed* Variablen vom Typ vorhanden ist, wird ein Typrückschluss erfolgreich ausgeführt wird.
*   Andernfalls für alle Argumente `Ei` mit entsprechenden Parametertyps `Ti` , in denen die *Ausgabetypen* ([Ausgabetypen](expressions.md#output-types)) enthalten *unfixed* Variablen vom Typ `Xj` jedoch *Eingabetypen* ([Eingabetypen](expressions.md#input-types)) ist dies kein *Ausgabe Typrückschluss* ([Typrückschlüsse Ausgabe ](expressions.md#output-type-inferences)) erfolgt *aus* `Ei` *zu* `Ti`. Anschließend wird die zweite Phase wiederholt.

#### <a name="input-types"></a>Eingabetypen

Wenn `E` ist eine Methodengruppe oder anonyme Funktion von implizit typisierten und `T` ist ein Delegat, Typ oder Typ für die Ausdrucksbaumstruktur und die Parametertypen der `T` sind *Eingabetypen* von `E` *mit Typ* `T`.

####  <a name="output-types"></a>Ausgabetypen

Wenn `E` ist einer Methodengruppe oder einer anonymen Funktion und `T` ist ein Delegat, Typ oder Typ für die Ausdrucksbaumstruktur und den Rückgabetyp der `T` ist ein *Ausgabetyp der* `E` *mit Typ*  `T`.

#### <a name="dependence"></a>Abhängigkeit

Ein *unfixed* Variablen vom Typ `Xi` *hängt direkt von* eine Variable unfixed vom Typ `Xj` If für einige Argument `Ek` mit Typ `Tk` `Xj` Tritt auf, eine *Eingabetyp* von `Ek` mit Typ `Tk` und `Xi` tritt auf, eine *Ausgabetyp* von `Ek` mit Typ `Tk`.

`Xj` *hängt von* `Xi` Wenn `Xj` *hängt direkt von* `Xi` oder, wenn `Xi` *hängt direkt von* `Xk` und `Xk` *hängt* `Xj`. Daher ist "hängt" transitiv, aber keine reflexive Abschluss von "hängt direkt von".

#### <a name="output-type-inferences"></a>Ausgabe Typrückschlüsse

Ein *Ausgabe Typrückschluss* erfolgt *aus* eines Ausdrucks `E` *zu* ein `T` auf folgende Weise:

*  Wenn `E` ist eine anonyme Funktion hergeleiteten Rückgabetyp `U` ([abgeleiteter Rückgabetyp](expressions.md#inferred-return-type)) und `T` ist ein Delegat oder Typ für die Ausdrucksbaumstruktur mit Rückgabetyp `Tb`, klicken Sie dann eine *untere Begrenzung Typrückschluss* ([untere Begrenzung Rückschlüsse](expressions.md#lower-bound-inferences)) erfolgt *aus* `U` *zu* `Tb`.
*  Andernfalls gilt: Wenn `E` ist eine Methodengruppe und `T` ist ein Delegat oder Typ mit den Parametertypen für die Ausdrucksbaumstruktur `T1...Tk` und den Rückgabetyp `Tb`, und die überladungsauflösung von `E` mit den Typen `T1...Tk` ergibt eine einzige Methode, mit dem Rückgabetyp `U`, ein *untere Begrenzung Typrückschluss* erfolgt *aus* `U` *zu* `Tb`.
*  Andernfalls gilt: Wenn `E` ist ein Ausdruck mit Typ `U`, ein *untere Begrenzung Typrückschluss* erfolgt *aus* `U` *zu* `T`.
*  Andernfalls werden keine Rückschlüsse vorgenommen.

#### <a name="explicit-parameter-type-inferences"></a>Expliziter Parameter Typrückschlüsse

Ein *expliziter Parameter Typrückschluss* erfolgt *aus* eines Ausdrucks `E` *zu* ein `T` auf folgende Weise:

*  Wenn `E` ist eine explizit typisierte anonyme Funktion mit den Parametertypen `U1...Uk` und `T` ist ein Delegat oder Typ mit den Parametertypen für die Ausdrucksbaumstruktur `V1...Vk` klicken Sie dann für jeden `Ui` ein *genaue Typrückschluss* ([genauer Rückschlüsse](expressions.md#exact-inferences)) erfolgt *aus* `Ui` *zu* entsprechenden `Vi`.

#### <a name="exact-inferences"></a>Genaue Rückschlüsse

Ein *genauer Typrückschluss* *aus* ein `U` *zu* ein `V` erfolgt wie folgt:

*  Wenn `V` ist eines der der *unfixed* `Xi` dann `U` hinzugefügt wird, auf den Satz der genauen Grenzen für `Xi`.

*  Andernfalls legt `V1...Vk` und `U1...Uk` werden bestimmt, indem Sie überprüfen, ob eine der folgenden Fälle zutrifft:

   *  `V` ist ein Arraytyp `V1[...]` und `U` ist ein Arraytyp `U1[...]` von den gleichen Rang
   *  `V` Der Typ `V1?` und `U` ist der Typ `U1?`
   *  `V` wird von ein konstruierter Typ `C<V1...Vk>`und `U` ein konstruierter Typ ist `C<U1...Uk>`

   Wenn es sich bei jedem dieser Fälle wenden Sie dann eine *genauer Typrückschluss* erfolgt *aus* jedes `Ui` *zu* der entsprechenden `Vi`.

*  Andernfalls werden keine Rückschlüsse vorgenommen.

#### <a name="lower-bound-inferences"></a>Untere Begrenzung Rückschlüsse

Ein *untere Begrenzung Typrückschluss* *aus* ein `U` *zu* ein `V` erfolgt wie folgt:

*  Wenn `V` ist eines der der *unfixed* `Xi` dann `U` hinzugefügt wird, auf die unteren Grenzen `Xi`.
*  Andernfalls gilt: Wenn `V` ist der Typ `V1?`und `U` ist der Typ `U1?` eine untere Grenze Rückschluss aus erfolgt `U1` zu `V1`.
*  Andernfalls legt `U1...Uk` und `V1...Vk` werden bestimmt, indem Sie überprüfen, ob eine der folgenden Fälle zutrifft:
   *  `V` ist ein Arraytyp `V1[...]` und `U` ist ein Arraytyp `U1[...]` (oder ein Typparameter, dessen effektive Basistyp ist `U1[...]`) von den gleichen Rang
   *  `V` ist eine der `IEnumerable<V1>`, `ICollection<V1>` oder `IList<V1>` und `U` ist ein eindimensionales Array `U1[]`(oder ein Typparameter, dessen effektive Basistyp ist `U1[]`)
   *  `V` ist ein konstruierter Typ der Klasse, Struktur, Schnittstelle oder Delegaten `C<V1...Vk>` und es ist ein eindeutiger Typ `C<U1...Uk>` so, dass `U` (oder, wenn Sie `U` ist ein Typparameter, effektive Basisklasse oder ein Mitglied der effektive Satz) ist mit dieser identisch (direkt oder indirekt) erbt, oder implementiert (direkt oder indirekt) `C<U1...Uk>`.

      (Die Einschränkung "Eindeutigkeit" bedeutet, dass in der Groß-/Kleinschreibung Schnittstelle `C<T> {} class U: C<X>, C<Y> {}`, erfolgt kein Rückschluss beim Ableiten von `U` zu `C<T>` da `U1` möglicherweise `X` oder `Y`.)

   Wenn eine dieser Fälle zutrifft, ein Typrückschluss erfolgt *aus* jedes `Ui` *zu* entsprechenden `Vi` wie folgt:

   *  Wenn `Ui` ist ein Verweistyp ist nicht bekannt wird eine *genauer Typrückschluss* erfolgt
   *  Andernfalls gilt: Wenn `U` ist ein Arraytyp wird eine *untere Begrenzung Typrückschluss* erfolgt
   *  Andernfalls gilt: Wenn `V` ist `C<V1...Vk>` und klicken Sie dann die i-th-Typparameter der Rückschluss abhängt `C`:
      *  Wenn es kovariant ist ein *untere Begrenzung Typrückschluss* erfolgt.
      *  Wenn es sich um kontravariant ist ein *Obergrenze Typrückschluss* erfolgt.
      *  Wenn er unveränderlich ist ein *genauer Typrückschluss* erfolgt.
*  Andernfalls werden keine Rückschlüsse vorgenommen.

#### <a name="upper-bound-inferences"></a>Obere Grenze Rückschlüsse

Ein *Obergrenze Typrückschluss* *aus* ein `U` *zu* ein `V` erfolgt wie folgt:

*  Wenn `V` ist eines der der *unfixed* `Xi` dann `U` hinzugefügt wird, auf die oberen Grenzen `Xi`.
*  Andernfalls legt `V1...Vk` und `U1...Uk` werden bestimmt, indem Sie überprüfen, ob eine der folgenden Fälle zutrifft:
   *  `U` ist ein Arraytyp `U1[...]` und `V` ist ein Arraytyp `V1[...]` von den gleichen Rang
   *  `U` ist eine der `IEnumerable<Ue>`, `ICollection<Ue>` oder `IList<Ue>` und `V` ist ein eindimensionales Array-Typ `Ve[]`
   *  `U` Der Typ `U1?` und `V` ist der Typ `V1?`
   *  `U` ist der konstruierten Klasse, Struktur, Schnittstelle oder ein Delegattyp `C<U1...Uk>` und `V` wird von einer Klasse, Struktur, Schnittstellen oder Delegate-Typ, der mit erbt (direkt oder indirekt) identisch ist oder implementiert (direkt oder indirekt) ein eindeutiger Typ `C<V1...Vk>`

      (Die Einschränkung "Eindeutigkeit" bedeutet, dass bei wir `interface C<T>{} class V<Z>: C<X<Z>>, C<Y<Z>>{}`, erfolgt kein Rückschluss beim Ableiten von `C<U1>` zu `V<Q>`. Rückschlüsse erfolgen keine `U1` entweder `X<Q>` oder `Y<Q>`.)

   Wenn eine dieser Fälle zutrifft, ein Typrückschluss erfolgt *aus* jedes `Ui` *zu* entsprechenden `Vi` wie folgt:
   *  Wenn `Ui` ist ein Verweistyp ist nicht bekannt wird eine *genauer Typrückschluss* erfolgt
   *  Andernfalls gilt: Wenn `V` ist ein Arraytyp wird eine *Obergrenze Typrückschluss* erfolgt
   *  Andernfalls gilt: Wenn `U` ist `C<U1...Uk>` und klicken Sie dann die i-th-Typparameter der Rückschluss abhängt `C`:
      *  Wenn es kovariant ist ein *Obergrenze Typrückschluss* erfolgt.
      *  Wenn es sich um kontravariant ist ein *untere Begrenzung Typrückschluss* erfolgt.
      *  Wenn er unveränderlich ist ein *genauer Typrückschluss* erfolgt.
*  Andernfalls werden keine Rückschlüsse vorgenommen.   

#### <a name="fixing"></a>Beheben von

Ein *unfixed* Variablen vom Typ `Xi` ist mit einem Satz von Grenzen *festen* wie folgt:

*  Der Satz von *potenzieller Typen* `Uj` als Satz aller Typen in den Satz von Grenzen für beginnt `Xi`.
*  Klicken Sie dann untersuchen wir jede Grenze für `Xi` wiederum: Für jede genau gebundene `U` von `Xi` alle Typen `Uj` die sind nicht identisch mit `U` werden aus dem Satz von Kandidaten entfernt. Für jede untere Grenze `U` von `Xi` alle Typen `Uj` ist auf die dort *nicht* eine implizite Konvertierung von `U` werden aus dem Satz von Kandidaten entfernt. Für jede Obergrenze `U` von `Xi` alle Typen `Uj` wird die dort *nicht* eine implizite Konvertierung in `U` werden aus dem Satz von Kandidaten entfernt.
*  If zu den Typen der verbleibenden Candidate `Uj` es ist ein eindeutiger Typ `V` von dem es eine implizite Konvertierung auf alle anderen möglichen Typen, klicken Sie dann `Xi` ist fest in `V`.
*  Andernfalls schlägt die Typrückschluss fehl.

#### <a name="inferred-return-type"></a>Abgeleiteter Rückgabetyp

Die hergeleiteten Rückgabetyp von einer anonymen Funktion `F` wird während der typauflösung inferenz und die Überladung verwendet. Der hergeleitete Rückgabetyp kann nur für eine anonyme Funktion, in denen alle Parameter, den Typen bekannt sind, entweder weil sie explizit angegeben sind, über eine anonyme Funktion Konvertierung bereitgestellt oder abgeleitet wird, während der Typrückschluss in einer einschließenden generischen, ermittelt werden Methodenaufruf.

Die ***abgeleitet Ergebnistyp*** wird wie folgt bestimmt:

*  Wenn der Text der `F` ist ein *Ausdruck* , bei dem ein Typ, und klicken Sie dann auf die hergeleiteten Rückgabetyp des `F` ist der Typ dieses Ausdrucks.
*  Wenn der Text der `F` ist eine *Block* und der Satz von Ausdrücken im Block `return` -Anweisungen verfügt auch über einen bewährten gemeinsamen Typ `T` ([Suchen nach den besten common-Typ, der einen Satz von Ausdrücken](expressions.md#finding-the-best-common-type-of-a-set-of-expressions)), klicken Sie dann die hergeleiteten Rückgabetyp des `F` ist `T`.
*  Andernfalls kann kein Ergebnistyp abgeleitet werden, für die `F`.

Die ***Rückgabetyp abgeleitet*** wird wie folgt bestimmt:

*  Wenn `F` ist asynchron und der Text der `F` ist entweder ein Ausdruck als "nothing" klassifiziert sind ([ausdrucksklassifizierungen](expressions.md#expression-classifications)), oder einen Anweisungsblock, die keine return-Anweisungen, in dem Ausdrücke haben, die hergeleiteten Rückgabetyp ist `System.Threading.Tasks.Task`
*  Wenn `F` Async, verfügt über eine abgeleitete Ergebnistyp `T`, die hergeleiteten Rückgabetyp ist `System.Threading.Tasks.Task<T>`.
*  Wenn `F` nicht asynchronen, verfügt über eine abgeleitete Ergebnistyp `T`, die hergeleiteten Rückgabetyp ist `T`.
*  Andernfalls kann kein Rückgabetyp abgeleitet werden, für die `F`.

Betrachten Sie als ein Beispiel für den Typrückschluss, die im Zusammenhang mit anonymen Funktionen, die `Select` Erweiterungsmethode deklariert wird, der `System.Linq.Enumerable` Klasse:
```csharp
namespace System.Linq
{
    public static class Enumerable
    {
        public static IEnumerable<TResult> Select<TSource,TResult>(
            this IEnumerable<TSource> source,
            Func<TSource,TResult> selector)
        {
            foreach (TSource element in source) yield return selector(element);
        }
    }
}
```

Vorausgesetzt die `System.Linq` Namespace importiert wurde, mit einer `using` -Klausel, und erhalten Sie eine Klasse `Customer` mit eine `Name` Eigenschaft vom Typ `string`, `Select` Methode kann verwendet werden, um die Namen der eine Liste der Kunden auswählen:
```csharp
List<Customer> customers = GetCustomerList();
IEnumerable<string> names = customers.Select(c => c.Name);
```

Der Aufruf der Erweiterungsmethode ([Erweiterung Methodenaufrufe](expressions.md#extension-method-invocations)) der `Select` verarbeitet wird, durch den Aufruf zu Aufruf einer statischen Methode umschreiben:
```csharp
IEnumerable<string> names = Enumerable.Select(customers, c => c.Name);
```

Da Sie nicht explizit Typargumente angegeben wurden, werden den Typrückschluss die Typargumente abzuleiten. Zuerst die `customers` Argument bezieht sich auf die `source` Parameter Ableiten von `T` sein `Customer`. Mithilfe der anonymen Funktion geben Sie dann oben beschriebenen Rückschlussprozess `c` erhält den Typ `Customer`, und der Ausdruck `c.Name` bezieht sich auf den Rückgabetyp der `selector` Parameter Ableiten von `S` sein`string`. Daher ist der Aufruf entspricht
```csharp
Sequence.Select<Customer,string>(customers, (Customer c) => c.Name)
```
und das Ergebnis ist vom Typ `IEnumerable<string>`.

Im folgende Beispiel wird veranschaulicht, wie anonyme Funktionstyp Typrückschluss ermöglicht die Typinformationen für "flow zwischen Argumenten im Aufruf einer generischen Methode". Betrachten Sie die Methode ein:
```csharp
static Z F<X,Y,Z>(X value, Func<X,Y> f1, Func<Y,Z> f2) {
    return f2(f1(value));
}
```

Der Typrückschluss für den Aufruf:
```csharp
double seconds = F("1:15:30", s => TimeSpan.Parse(s), t => t.TotalSeconds);
```
wird wie folgt: Zunächst wird das Argument `"1:15:30"` bezieht sich auf die `value` Parameter Ableiten von `X` sein `string`. Klicken Sie dann, den Parameter der ersten anonymen Funktion `s`, erhält den abgeleiteten Typ `string`, und der Ausdruck `TimeSpan.Parse(s)` bezieht sich auf den Rückgabetyp der `f1`, ableiten von `Y` sein `System.TimeSpan`. Abschließend den Parameter der zweiten anonyme Funktion `t`, erhält den abgeleiteten Typ `System.TimeSpan`, und der Ausdruck `t.TotalSeconds` bezieht sich auf den Rückgabetyp der `f2`, ableiten von `Z` sein `double`. Daher ist das Ergebnis des Aufrufs, vom Typ `double`.

#### <a name="type-inference-for-conversion-of-method-groups"></a>Typrückschluss für die Konvertierung von Methodengruppen

Ähnlich wie Aufrufe der Methoden, Typrückschluss muss auch angewendet, wenn eine Methodengruppe `M` einer generischen Methode mit einem bestimmten Delegattyp konvertiert `D` ([Gruppe Konvertierungen](conversions.md#method-group-conversions)). Wenn eine Methode
```csharp
Tr M<X1...Xn>(T1 x1 ... Tm xm)
```
und die Methodengruppe `M` zugewiesen wird, in den Delegattyp `D` die Aufgabe des Typrückschlusses besteht darin, finden Sie Typargumente `S1...Sn` , damit der Ausdruck:
```csharp
M<S1...Sn>
```
kompatibel ist ([delegieren Deklarationen](delegates.md#delegate-declarations)) mit `D`.

Im Gegensatz zum Typ Typrückschluss für generische Methodenaufrufen, in diesem Fall stehen nur Argument *Typen*, kein Argument *Ausdrücke*. Vor allem keine anonymen Funktionen vorhanden sind und daher keine Notwendigkeit, mehrere Phasen der Typrückschluss.

Stattdessen alle `Xi` gelten *unfixed*, und ein *untere Begrenzung Typrückschluss* erfolgt *aus* jeder Argumenttyp `Uj` von `D` *zu* entsprechenden Parametertyps `Tj` von `M`. If für eines der `Xi` keine Grenzen gefunden, der Typrückschluss schlägt fehl. Andernfalls alle `Xi` sind *behoben* entsprechenden `Si`, die das Ergebnis des Typrückschlusses sind.

#### <a name="finding-the-best-common-type-of-a-set-of-expressions"></a>Suchen den besten common-Typ, der einen Satz von Ausdrücken

In einigen Fällen muss kein gemeinsamer Typ für einen Satz von Ausdrücken abgeleitet werden. Insbesondere die Elementtypen eines implizit typisierte Arrays und die Rückgabetypen der anonymen Funktionen mit *Block* Stellen auf diese Weise gefunden werden.

Intuitiv anhand eines Satzes von Ausdrücken `E1...Em` dieser Rückschluss muss dem Aufruf einer Methode
```csharp
Tr M<X>(X x1 ... X xm)
```
mit der `Ei` als Argumente.

Genauer gesagt: die Ableitung beginnt mit einem *unfixed* Variablen vom Typ `X`. *Ausgabe Typrückschlüsse* werden durchgeführt, *aus* jedes `Ei` *zu* `X`. Zum Schluss `X` ist *festen* und geben Sie bei erfolgreicher Ausführung der resultierenden `S` ist der resultierende bewährte gemeinsame Typ für die Ausdrücke. Wenn keine solche `S` vorhanden ist, die Ausdrücke besitzen kein optimaler Typ für die allgemeine.

### <a name="overload-resolution"></a>Auflösung von funktionsüberladungen

Überladungsauflösung ist eine Bindung-Time-Mechanismus für die Auswahl der am besten geeigneten Funktionsmembers angegebenen Argumentliste und einen Satz von möglichen Funktionsmembern aufrufen. Wählt die überladungsauflösung die Funktionsmember der Member in den folgenden unterschiedlichen Kontexten in C# -Code aufrufen:

*  Aufruf einer Methode, die mit dem Namen in einem *Invocation_expression* ([Methodenaufrufe](expressions.md#method-invocations)).
*  Aufruf eines Instanzkonstruktors mit dem Namen in einem *Object_creation_expression* ([Erstellung Objektausdrücke](expressions.md#object-creation-expressions)).
*  Aufruf eines Indexeraccessors über eine *Element_access* ([Elementzugriff](expressions.md#element-access)).
*  Aufruf eines vordefinierten oder benutzerdefinierten Operators in einem Ausdruck verwiesen wird ([unäroperator überladungsauflösung](expressions.md#unary-operator-overload-resolution) und [binärer Operator überladungsauflösung](expressions.md#binary-operator-overload-resolution)).

Alle diese Kontexte definiert den Satz von möglichen Funktionsmembern und die Liste der Argumente in ihre eigenen Methoden, wie in den oben aufgeführten Abschnitten ausführlich beschrieben. Der Satz von Kandidaten für einen Methodenaufruf enthält z. B. keine markierte Methoden `override` ([Membersuche](expressions.md#member-lookup)), und Methoden in einer Basisklasse sind nicht Kandidaten, wenn es sich bei einer beliebigen Methode in einer abgeleiteten Klasse gilt ([ Methodenaufrufe](expressions.md#method-invocations)).

Nach der möglichen Funktionsmembern und deren Argumentliste identifiziert wurden, ist die Auswahl der am besten geeigneten Funktionsmembers in allen Fällen identisch:

*  Die am besten, die den Satz von anwendbaren möglichen Funktionsmembern wird angegeben, Funktion die Member, Satz befindet. Wenn der Satz nur einen Funktionsmember enthält, ist dieser Funktionsmember die am besten geeigneten Funktionsmembers. Andernfalls ist die am besten geeigneten Funktionsmembers Funktionsmember, das besser als andere Funktionsmember hinsichtlich der angegebenen Argumentliste, vorausgesetzt, dass jeder Funktionsmember verglichen wird, für alle Mitglieder anderer Funktion mit den Regeln in [ Geeigneterer Funktionsmember](expressions.md#better-function-member). Wenn nicht genau ein Funktionsmember, das besser als andere Funktionsmember vorhanden ist, klicken Sie dann der Element-Funktionsaufruf ist mehrdeutig, und beim Auftreten eines Laufzeitfehlers-Bindung.

Die folgenden Abschnitte definieren die genauen Bedeutungen der Begriffe ***anwendbarer Funktionsmember*** und ***geeigneterer Funktionsmember***.

#### <a name="applicable-function-member"></a>Anwendbarer Funktionsmember

Ein Funktionsmember gilt eine ***anwendbarer Funktionsmember*** in Bezug auf eine Argumentliste `A` wenn Folgendes zutrifft:

*  Jedes Argument im `A` entspricht einem Parameter in der Funktionsmemberdeklaration Siehe [entsprechende Parameter](expressions.md#corresponding-parameters), und alle Parameter, der kein Argument entspricht, ist ein optionaler Parameter.
*  Für jedes Argument im `A`, den Modus des Arguments für die Parameterübergabe (d. h. Wert `ref`, oder `out`) ist identisch mit dem Parameter übergebenden Modus des entsprechenden Parameters und
   *  für eine Value-Parameter oder ein Parameterarray, eine implizite Konvertierung ([implizite Konvertierungen](conversions.md#implicit-conversions)) aus dem Argument vorhanden ist, in den Typ des entsprechenden Parameters oder
   *  für eine `ref` oder `out` Parameter, der der Typ des Arguments ist identisch mit den Typ des entsprechenden Parameters. Schließlich eine `ref` oder `out` Parameter ist ein Alias für das übergebene Argument.

Für eine Funktionselement, das ein Parameterarray enthält, wenn der Funktionsmember anhand der obigen Regeln gilt, gilt in angewendet werden. die ***Normalform***. Ist ein Funktionselement, das ein Parameterarray enthält nicht anwendbar, in seiner normalen Form, das Funktionselement kann stattdessen fallen in die ***erweitert Formular***:

*  Diese erweiterte Form wird erstellt, indem Sie ersetzt das Parameterarray in der Funktionsmemberdeklaration mit 0 (null) oder ein array, z. B. weitere Parameter der Wert des Elementtyps des Parameters, die Anzahl der Argumente in der Argumentliste `A` entspricht die Summe Anzahl von Parametern. Wenn `A` weniger Argumente als die Anzahl der festen Parameter in der Funktionsmemberdeklaration enthält, ist die erweiterte Form der Funktionsmember der Member kann nicht erstellt werden und ist somit nicht verwendbar.
*  Andernfalls ist diese erweiterte Form anwendbar, wenn Sie für jedes Argument im `A` den Parameter übergebenden Modus des Arguments ist identisch mit dem Parameter übergebenden Modus des entsprechenden Parameters und
   *  für einen festen Wert-Parameter oder eine Value-Parameter, die durch die Erweiterung, die eine implizite Konvertierung erzeugt ([implizite Konvertierungen](conversions.md#implicit-conversions)) vorhanden ist aus dem Typ des Arguments in den Typ des entsprechenden Parameters oder
   *  für eine `ref` oder `out` Parameter, der der Typ des Arguments ist identisch mit den Typ des entsprechenden Parameters.

#### <a name="better-function-member"></a>Geeigneterer Funktionsmember

Im Rahmen den geeignetere Funktionsmember festlegen wird eine reduzierte Argumentliste A erstellt, enthält nur die Argumentausdrücken selbst in der Reihenfolge, die sie in der ursprünglichen Argumentliste angezeigt werden.

Parameterlisten für jeden die Candidate-Funktion, die Mitglieder auf folgende Weise erstellt werden:

*  Diese erweiterte Form wird verwendet, wenn das Funktionselement betrifft nur die erweiterte Form wurde.
*  Optionale Parameter ohne entsprechenden Argumente werden aus der Liste entfernt.
*  Die Parameter sind neu angeordnet, damit sie an der gleichen Position wie das entsprechende Argument in der Argumentliste auftreten.

Eine Argumentliste `A` mit einem Satz von Argumentausdrücken `{E1, E2, ..., En}` und zwei anwendbaren Funktionsmembern `Mp` und `Mq` mit Parametertypen `{P1, P2, ..., Pn}` und `{Q1, Q2, ..., Qn}`, `Mp` definiert eine ***geeigneterer Funktionsmember*** als `Mq` Wenn

*  für jedes Argument, das die implizite Konvertierung von `Ex` zu `Qx` ist nicht besser als die implizite Konvertierung von `Ex` zu `Px`, und
*  für mindestens ein Argument, um die Konvertierung von `Ex` zu `Px` ist besser als die Konvertierung von `Ex` zu `Qx`.

Beim Ausführen dieser Evaluierung Wenn `Mp` oder `Mq` gilt in der erweiterten Form dann `Px` oder `Qx` bezieht sich auf einen Parameter in die erweiterte Form der Parameterliste.

Für den Fall, dass der Typparameter Sequenzen `{P1, P2, ..., Pn}` und `{Q1, Q2, ..., Qn}` äquivalent sind (d. h. jede `Pi` verfügt über eine identitätskonvertierung in den entsprechenden `Qi`), die folgenden Regeln für die aktuelle Verbindung angewendet werden, in der Reihenfolge, um zu bestimmen, desto besser Funktionsmember.

*  Wenn `Mp` ist eine nicht generische Methode und `Mq` ist eine generische Methode `Mp` ist besser als `Mq`.
*  Andernfalls gilt: Wenn `Mp` gilt in seiner normalen Form und `Mq` verfügt über eine `params` array und gilt nur in der erweiterten Form dann `Mp` ist besser als `Mq`.
*  Andernfalls gilt: Wenn `Mp` verfügt über mehrere deklarierte Parameter als `Mq`, klicken Sie dann `Mp` ist besser als `Mq`. Dies kann auftreten, wenn beide Methoden haben `params` Arrays und sind nur in ihre erweiterte Formate angewendet.
*  Andernfalls Wenn alle Parameter der `Mp` ein entsprechenden Arguments haben, während die Standardargumente für mindestens ein optionaler Parameter in ersetzt werden müssen `Mq` dann `Mp` ist besser als `Mq`.
*  Andernfalls gilt: Wenn `Mp` hat spezifische Parametertypen als `Mq`, klicken Sie dann `Mp` ist besser als `Mq`. Lassen Sie `{R1, R2, ..., Rn}` und `{S1, S2, ..., Sn}` die nicht instanziierte und nicht erweiterten Parametertypen darstellen `Mp` und `Mq`. `Mp`die Parametertypen sind präziser als `Mq`des If, für jeden Parameter `Rx` ist nicht als weniger spezifischen `Sx`, und für mindestens ein Parameter `Rx` sind präziser als `Sx`:
   *  Ein Typparameter ist weniger spezifisch als Nichttyp-Parameter.
   *  Rekursiv, bezieht sich ein konstruierter Typ mehr als einen anderen konstruierten Typ (mit derselben Anzahl von Typargumenten), wenn mindestens eine Typargument spezifischere ist und keine Typargument weniger spezifisch als das entsprechende Typargument in der anderen ist.
   *  Ein Arraytyp ist spezifischer als einen anderen Arraytyp (mit derselben Anzahl von Dimensionen), wenn der Typ des Elements der ersten spezifischer als der Elementtyp der zweiten ist.
*  Andernfalls ist, wenn ein Element ein Operator nicht aufgehoben ist, und die andere ein transformierten Operator ist, die nicht transformiert eine besser.
*  Andernfalls ist keine Funktionsmember besser.

#### <a name="better-conversion-from-expression"></a>Bessere Konvertierung von Ausdruck

Erhalten eine implizite Konvertierung `C1` , der von einem Ausdruck konvertiert `E` auf einen Typ `T1`, und eine implizite Konvertierung `C2` , der von einem Ausdruck konvertiert `E` auf einen Typ `T2`, `C1` ist eine ***bessere Konvertierung*** als `C2` Wenn `E` entspricht nicht genau `T2` und mindestens eine der folgenden enthält:

* `E` genau übereinstimmt `T1` ([genau übereinstimmenden Ausdrucks](expressions.md#exactly-matching-expression))
* `T1` ist eine bessere Konvertierung Ziel als `T2` ([besser Konvertierung ausrichten](expressions.md#better-conversion-target))

#### <a name="exactly-matching-expression"></a>-Ausdruck genau entsprechen.

Erhält einen Ausdruck `E` und einen Typ `T`, `E` genau Übereinstimmungen `T` , wenn eine der folgenden enthält:

*  `E` weist einen Typ `S`, und eine identitätskonvertierung vorhanden ist, von `S` auf `T`
*  `E` ist eine anonyme Funktion, `T` ist entweder ein Delegattyp `D` oder ein Typ für die Ausdrucksbaumstruktur `Expression<D>` und eine der folgenden enthält:
   *  Einem hergeleiteten Rückgabetyp `X` vorhanden ist, für die `E` im Rahmen der Liste der Parameter `D` ([abgeleiteter Rückgabetyp](expressions.md#inferred-return-type)), und eine identitätskonvertierung vorhanden ist, von `X` in den Rückgabetyp der `D`
   *  Entweder `E` ist nicht asynchronen und `D` hat einen Rückgabetyp `Y` oder `E` ist asynchron und `D` hat einen Rückgabetyp `Task<Y>`, und eine der folgenden enthält:
      * Der Text der `E` ist ein Ausdruck, die genau übereinstimmt `Y`
      * Der Text der `E` ist ein Anweisungsblock, in dem alle return-Anweisung gibt einen Ausdruck, die genau übereinstimmt zurück, `Y`

#### <a name="better-conversion-target"></a>Bessere Konvertierung-Ziel

Erhalten zwei verschiedene Arten `T1` und `T2`, `T1` ist eine bessere Konvertierung Ziel als `T2` Wenn keine implizite Konvertierung von `T2` zu `T1` vorhanden ist, und mindestens eine der folgenden enthält:

*  Eine implizite Konvertierung von `T1` zu `T2` vorhanden ist
*  `T1` ist entweder ein Delegattyp `D1` oder ein Typ für die Ausdrucksbaumstruktur `Expression<D1>`, `T2` ist entweder ein Delegattyp `D2` oder ein Typ für die Ausdrucksbaumstruktur `Expression<D2>`, `D1` hat einen Rückgabetyp `S1` und eines der folgenden enthält:
   * `D2` ist die "void" zurückgeben
   * `D2` hat einen Rückgabetyp `S2`, und `S1` ist eine bessere Konvertierung Ziel als `S2`
*  `T1` ist `Task<S1>`, `T2` ist `Task<S2>`, und `S1` ist eine bessere Konvertierung Ziel als `S2`
*  `T1` ist `S1` oder `S1?` , in denen `S1` ist ein ganzzahliger Typ mit Vorzeichen, und `T2` ist `S2` oder `S2?` , in denen `S2` ein Integraltyp ohne Vorzeichen ist. Dies gilt insbesondere in folgenden Fällen:
   * `S1` ist `sbyte` und `S2` ist `byte`, `ushort`, `uint`, oder `ulong`
   * `S1` ist `short` und `S2` ist `ushort`, `uint`, oder `ulong`
   * `S1` ist `int` und `S2` ist `uint`, oder `ulong`
   * `S1` ist `long` und `S2` ist `ulong`

#### <a name="overloading-in-generic-classes"></a>In der generischen Klassen überladen

Während Signaturen, die gemäß der Deklaration eindeutig und sein müssen ist es möglich, dass die Ersetzung der Argumente des Typs zu identischen Signaturen führt. In solchen Fällen werden die aktuelle Tie Regeln der überladungsauflösung, die oben genannten das spezifischste Element auswählen.

Die folgenden Beispiele zeigen die Überladungen, die gültige und ungültige nach dieser Regel sind:

```csharp
interface I1<T> {...}

interface I2<T> {...}

class G1<U>
{
    int F1(U u);                  // Overload resolution for G<int>.F1
    int F1(int i);                // will pick non-generic

    void F2(I1<U> a);             // Valid overload
    void F2(I2<U> a);
}

class G2<U,V>
{
    void F3(U u, V v);            // Valid, but overload resolution for
    void F3(V v, U u);            // G2<int,int>.F3 will fail

    void F4(U u, I1<V> v);        // Valid, but overload resolution for    
    void F4(I1<V> v, U u);        // G2<I1<int>,int>.F4 will fail

    void F5(U u1, I1<V> v2);      // Valid overload
    void F5(V v1, U u2);

    void F6(ref U u);             // valid overload
    void F6(out V v);
}
```

### <a name="compile-time-checking-of-dynamic-overload-resolution"></a>Überprüfungen zur Kompilierzeit der dynamischen überladungsauflösung

Für die meisten dynamisch gebundene Vorgänge ist der Satz von möglichen Kandidaten für die Auflösung zum Zeitpunkt der Kompilierung unbekannt. In bestimmten Fällen ist jedoch der Satz von Kandidaten zum Zeitpunkt der Kompilierung bekannt:

*  Aufrufen statischer Methoden, mit dem dynamische Argumente
*  Instanz-Methodenaufrufen, in denen der Empfänger keine dynamischen Ausdruck ist
*  Indexer-Aufrufe, in denen der Empfänger keine dynamischen Ausdruck ist
*  Ruft der Konstruktor mit dem dynamische Argumente

In diesen Fällen wird eine begrenzte Überprüfung der während der Kompilierung ausgeführt, bei jedem Kandidaten aus, um festzustellen, ob diese möglicherweise zur Laufzeit anwenden könnten. Diese Überprüfung umfasst die folgenden Schritte aus:

*  Partielle Typrückschluss: Jeder Typ Argument, das nicht direkt oder indirekt auf ein Argument vom Typ abhängt `dynamic` abgeleitet wird, wobei die Regeln der [Typrückschluss](expressions.md#type-inference). Die verbleibenden Typargumente sind unbekannt.
*  Partielle anwendbarkeitsprüfung: Anwendbarkeit wird überprüft, gemäß [Anwendbarer Funktionsmember](expressions.md#applicable-function-member), aber der Parameter, deren Typen unbekannt sind, werden ignoriert.
*  Wenn kein Kandidat dieser Test erfolgreich ist, tritt ein Fehler während der Kompilierung.

### <a name="function-member-invocation"></a>Member-Funktionsaufruf

Dieser Abschnitt beschreibt den Prozess, der erfolgt zur Laufzeit eine bestimmte Funktionsmember aufgerufen werden soll. Es wird vorausgesetzt, dass ein Bindung-Time-Prozess bereits das bestimmte Element um aufzurufen, möglicherweise durch Anwenden der überladungsauflösung, um einen Satz von möglichen Funktionsmembern festgestellt hat.

Rahmen, den Aufrufvorgang beschreiben sind Funktionsmember in zwei Kategorien unterteilt:

*  Mitglieder der statische Funktion. Dies sind Instanzkonstruktoren, statische Methoden, statische Eigenschaftenaccessoren und benutzerdefinierten Operatoren. Statische Funktionsmember sind immer nicht virtuell.
*  Instanzmember-Funktion. Dies sind Instanzmethoden, Instanz-Eigenschaftenaccessoren und Indexeraccessoren. Instanzmember-Funktion sind entweder nicht virtuell oder virtuell sein, und Sie werden immer in einer bestimmten Instanz aufgerufen. Die Instanz wird berechnet, indem ein Instanzenausdruck, und es wird innerhalb der Funktionsmember der Member als zugegriffen werden `this` ([diesen Zugriff](expressions.md#this-access)).

Die laufzeitverarbeitung der einen Funktionsaufruf für das Element besteht aus den folgenden Schritten, in denen `M` ist das Funktionselement und, falls `M` ist ein Instanzmember `E` ist die Instanzausdruck:

*  Wenn `M` ist eine statische Funktion-Element:
   * Die Argumentliste ausgewertet wird, wie in beschrieben [Argumentlisten](expressions.md#argument-lists).
   * `M` wird aufgerufen.

*  Wenn `M` wird in ein Instanzmember der Funktion deklariert eine *Value_type*:
   * `E` wird ausgewertet. Wenn diese Evaluierungsversion auf eine Ausnahme auslöst, werden dann keine weiteren Schritte ausgeführt werden.
   * Wenn `E` wird nicht als eine Variable, und klicken Sie dann auf eine temporäre lokale Variable vom klassifiziert `E`Typ erstellt wird und der Wert der `E` diese Variable zugewiesen wird. `E` als ein Verweis auf diese temporäre lokale Variable wird dann neu klassifiziert werden. Die temporäre Variable ist als `this` in `M`, aber nicht auf andere Weise. Daher nur, wenn `E` ist eine Variable "true" ist es möglich, dass der Aufrufer die Änderungen zu prüfen, `M` stellt `this`.
   * Die Argumentliste ausgewertet wird, wie in beschrieben [Argumentlisten](expressions.md#argument-lists).
   * `M` wird aufgerufen. Die Variable verweist `E` wird die Variable verweist `this`.

*  Wenn `M` wird in ein Instanzmember der Funktion deklariert eine *Reference_type*:
   * `E` wird ausgewertet. Wenn diese Evaluierungsversion auf eine Ausnahme auslöst, werden dann keine weiteren Schritte ausgeführt werden.
   * Die Argumentliste ausgewertet wird, wie in beschrieben [Argumentlisten](expressions.md#argument-lists).
   * Wenn der Typ des `E` ist eine *Value_type*, einer Boxing-Konvertierung ([Boxing-Konvertierung](types.md#boxing-conversions)) ausgeführt wird, um konvertieren `E` eingeben `object`, und `E` gilt Der Typ `object` in den folgenden Schritten. In diesem Fall `M` möglicherweise nur ein Mitglied `System.Object`.
   * Der Wert des `E` wird überprüft, um gültig sein. Wenn der Wert des `E` ist `null`, `System.NullReferenceException` wird ausgelöst, und keine weiteren Schritte ausgeführt werden.
   * Die Implementierung des Schnittstellenmembers Funktion aufrufen, wird bestimmt:
     * Wenn der Typ der Bindung-Time `E` ist eine Schnittstelle, der aufzurufenden Funktionsmember ist die Implementierung der `M` bereitgestellt, die von der Run-Time-Typ der Instanz verweist `E`. Diese Funktionsmember richtet sich nach Anwenden der Standardregeln für Schnittstellen-Zuordnung ([schnittstellenzuordnung](interfaces.md#interface-mapping)) um zu bestimmen, die Implementierung der `M` bereitgestellt, die von der Run-Time-Typ der Instanz verweist `E`.
     * Andernfalls gilt: Wenn `M` gehört eine virtuelle Funktion, der aufzurufenden Funktionsmember ist die Implementierung der `M` bereitgestellt, die von der Run-Time-Typ der Instanz verweist `E`. Dieser Funktionsmember richtet sich nach Anwenden der Regeln für die Bestimmung der am weitesten abgeleiteten Implementierung ([virtuelle Methoden](classes.md#virtual-methods)) der `M` in Bezug auf die Run-Time-Typ der Instanz verweist `E`.
     * Andernfalls `M` nicht virtuelle Funktion angehört, und wird von der aufzurufenden Funktionsmember `M` selbst.
   * Wird aufgerufen, die Implementierung des Schnittstellenmembers Funktion im vorherigen Schritt bestimmt. Das Objekt, das auf `E` wird das Objekt, das auf `this`.

#### <a name="invocations-on-boxed-instances"></a>Aufrufe in geschachtelten Instanzen

Ein Funktionsmember implementiert eine *Value_type* kann über eine geschachtelte Instanz, die aufgerufen werden *Value_type* in den folgenden Situationen:

*  Wenn der Funktionsmember der Member ist ein `override` einer Methode geerbt von Typ `object` und wird durch einen Instanzausdruck des Typs aufgerufen `object`.
*  Wenn der Funktionsmember der Member ist eine Implementierung eines Schnittstellenmembers-Funktion und wird aufgerufen, durch einen Instanzausdruck, der eine *Interface_type*.
*  Wenn der Funktionsmember der Member über einen Delegaten aufgerufen wird.

In diesen Fällen gilt eine Variable enthält die geschachtelte Instanz die *Value_type*, und diese Variable wird die Variable verweist `this` innerhalb von der Members-Funktionsaufruf. Insbesondere bedeutet dies, dass wenn ein Funktionsmember für eine geschachtelte Instanz aufgerufen wird, für den Funktionsmember "zum Ändern des Werts in die geschachtelte Instanz enthalten kann.

## <a name="primary-expressions"></a>Primäre Ausdrücke

Primärausdrücke umfassen die einfachsten Formen von Ausdrücken.

```antlr
primary_expression
    : primary_no_array_creation_expression
    | array_creation_expression
    ;

primary_no_array_creation_expression
    : literal
    | interpolated_string_expression
    | simple_name
    | parenthesized_expression
    | member_access
    | invocation_expression
    | element_access
    | this_access
    | base_access
    | post_increment_expression
    | post_decrement_expression
    | object_creation_expression
    | delegate_creation_expression
    | anonymous_object_creation_expression
    | typeof_expression
    | checked_expression
    | unchecked_expression
    | default_value_expression
    | nameof_expression
    | anonymous_method_expression
    | primary_no_array_creation_expression_unsafe
    ;
```

Ausdrücke (primär) aufgeteilt sind *Array_creation_expression*s und *Primary_no_array_creation_expression*s. Zum Behandeln von Arrayerstellungsausdruck – auf diese Weise, anstatt sie zusammen mit anderen einfachen Ausdruck Formulare auflisten ermöglicht es, die die Grammatik, die möglicherweise irreführende Code wie z. B. zu unterbinden
```csharp
object o = new int[3][1];
```
die andernfalls als interpretiert werden sollen
```csharp
object o = (new int[3])[1];
```

### <a name="literals"></a>Literale

Ein *Primary_expression* besteht aus einem *literal* ([Literale](lexical-structure.md#literals)) als Wert klassifiziert wird.


### <a name="interpolated-strings"></a>Interpolierte Zeichenfolgen

Ein *Interpolated_string_expression* besteht aus einem `$` anmelden, gefolgt von einer regulären oder wörtliche Zeichenfolge, die Literale, bei dem Löcher, getrennt durch `{` und `}`, setzen Sie Ausdrücke und Formatierung Spezifikationen. Ein Ausdruck eine interpolierte Zeichenfolge ist das Ergebnis einer *Interpolated_string_literal* , hat wurde unterteilt in einzelne Token, wie in beschrieben [interpolierte Zeichenfolgenliterale](lexical-structure.md#interpolated-string-literals).

```antlr
interpolated_string_expression
    : '$' interpolated_regular_string
    | '$' interpolated_verbatim_string
    ;

interpolated_regular_string
    : interpolated_regular_string_whole
    | interpolated_regular_string_start interpolated_regular_string_body interpolated_regular_string_end
    ;

interpolated_regular_string_body
    : interpolation (interpolated_regular_string_mid interpolation)*
    ;

interpolation
    : expression
    | expression ',' constant_expression
    ;

interpolated_verbatim_string
    : interpolated_verbatim_string_whole
    | interpolated_verbatim_string_start interpolated_verbatim_string_body interpolated_verbatim_string_end
    ;

interpolated_verbatim_string_body
    : interpolation (interpolated_verbatim_string_mid interpolation)+
    ;
```

Die *Constant_expression* eine Interpolation benötigen eine implizite Konvertierung in `int`.

Ein *Interpolated_string_expression* wird als Wert klassifiziert. Wenn sie sofort in konvertiert wird `System.IFormattable` oder `System.FormattableString` mit eine implizite interpolierte Zeichenfolge-Konvertierung ([implizite interpolierte zeichenfolgenkonvertierungen](conversions.md#implicit-interpolated-string-conversions)), des interpolierten Zeichenfolgenausdrucks Typ verfügt. Andernfalls ist es den Typ `string`.

Wenn der Typ einer interpolierten Zeichenfolge `System.IFormattable` oder `System.FormattableString`, die Bedeutung ist, einen Aufruf von `System.Runtime.CompilerServices.FormattableStringFactory.Create`. Wenn der Typ ist `string`, die Bedeutung des Ausdrucks ist ein Aufruf `string.Format`. In beiden Fällen besteht aus die Argumentliste des Aufrufs ein Formatzeichenfolgenliteral mit Platzhalter für jede Interpolation, und ein Argument für die einzelnen Ausdrücke, die Platzhalter entspricht.

Das Formatzeichenfolgenliteral wie folgt erstellt wird, in denen `N` ist die Anzahl von Interpolationen in die *Interpolated_string_expression*:

*  Wenn ein *Interpolated_regular_string_whole* oder *Interpolated_verbatim_string_whole* folgt die `$` anmelden, und klicken Sie dann das Format-Zeichenfolgenliteral, das Token ist.
*  Andernfalls besteht aus den Formatzeichenfolgenliteral: 
   *  Erste die *Interpolated_regular_string_start* oder *Interpolated_verbatim_string_start*
   *  Klicken Sie dann für jede Zahl `I` aus `0` zu `N-1`: 
      * Die dezimale Darstellung `I`
      * Wenn danach der entsprechenden *Interpolation* verfügt über eine *Constant_expression*, `,` (Komma) gefolgt von der dezimale Darstellung des Werts der *Constant_expression*
      * Die *Interpolated_regular_string_mid*, *Interpolated_regular_string_end*, *Interpolated_verbatim_string_mid* oder *Interpolated_ Verbatim_string_end* unmittelbar nach der entsprechenden Interpolation.

Die nachfolgende Argumente werden einfach die *Ausdrücke* aus der *Interpolationen* (sofern vorhanden), in der Reihenfolge.

TODO: Beispiele.


### <a name="simple-names"></a>Einfache Namen

Ein *Simple_name* eines Bezeichners steht, optional gefolgt von einer Liste der Typargumente besteht aus:

```antlr
simple_name
    : identifier type_argument_list?
    ;
```

Ein *Simple_name* ist entweder der Form `I` oder des Formulars `I<A1,...,Ak>`, wobei `I` ist eine einmalige Kennung und `<A1,...,Ak>` ist eine optionale *Type_argument_list*. Wenn kein *Type_argument_list* wird angegeben, sollten Sie `K` 0 (null). Die *Simple_name* ausgewertet und wie folgt klassifiziert:

*  Wenn `K` ist 0 (null) und die *Simple_name* Teil einer *Block* und, wenn die *Block*des (oder einer einschließenden *Block*des) lokalen Variablendeklaration-Speicherplatz ([Deklarationen](basic-concepts.md#declarations)) enthält lokale Variablen, Parameter oder Konstante mit dem Namen `I`, und klicken Sie dann die *Simple_name* bezieht sich auf dieser lokalen Variablen verwenden, Parameter oder Konstanten und wird als eine Variable oder einem Wert klassifiziert.
*  Wenn `K` ist 0 (null) und die *Simple_name* erscheint im Text der Deklaration einer generischen Methode und wenn dieser Deklaration einen Typparameter mit dem Namen enthält `I`, und klicken Sie dann die *Simple_name*bezieht sich auf den Typparameter.
*  Für jeden Instanztyp andernfalls `T` ([den Instanztyp](classes.md#the-instance-type)), beginnend mit dem Instanztyp der Deklaration des unmittelbar einschließenden und mit dem Instanztyp jeden einschließenden Klasse oder Struktur Deklaration (sofern vorhanden):
   *  Wenn `K` ist 0 (null) und die Deklaration von `T` enthält einen Typparameter mit dem Namen `I`, und klicken Sie dann die *Simple_name* bezieht sich auf den Typparameter.
   *  Andernfalls, wenn eine Suche nach Membern ([Membersuche](expressions.md#member-lookup)) der `I` in `T` mit `K`  Typargumente eine Übereinstimmung ergibt:
      * Wenn `T` ist die Instanztyp des unmittelbar umschließende Klasse oder Struktur und die Suche nach identifiziert einen oder mehrere Methoden, die das Ergebnis ist eine Methodengruppe mit einem zugeordneten Instanzausdruck von `this`. Wenn eine Liste der Typargumente angegeben wurde, wird es verwendet eine generische Methode aufzurufen ([Methodenaufrufe](expressions.md#method-invocations)).
      * Andernfalls gilt: Wenn `T` der Instanztyp der unmittelbar umschließende Klasse oder Struktur-Typ ist, wenn die Suche auf einen Instanzmember identifiziert und der Verweis innerhalb eines Texts der Instanzkonstruktor, eine Instanzmethode oder einem Instanzaccessor auftritt, die Ergebnis ist identisch mit Memberzugriff ([Memberzugriff](expressions.md#member-access)) des Formulars `this.I`. Dies kann nur, wenn `K` ist 0 (null).
      * Andernfalls das Ergebnis ist identisch mit Memberzugriff ([Memberzugriff](expressions.md#member-access)) des Formulars `T.I` oder `T.I<A1,...,Ak>`. In diesem Fall es ist ein Fehler während der Bindung für die *Simple_name* zum Verweisen auf ein Instanzmember.

*  Für jeden Namespace, andernfalls `N`, beginnend mit dem Namespace, in dem die *Simple_name* wird mit jeder einschließenden Namespace (sofern vorhanden) und bis hin zu den globalen Namespace, weiterhin werden die folgenden Schritte aus ausgewertet, bis eine Entität befindet:
   *  Wenn `K` ist 0 (null) und `I` ist der Name eines Namespace im `N`, klicken Sie dann:
      * Wenn der Speicherort, in dem die *Simple_name* tritt auf, steht eine Namespace-Deklaration für `N` und die Namespacedeklaration enthält eine *Extern_alias_directive* oder  *Using_alias_directive* , die den Namen zuordnet `I` mit einem Namespace oder Typ, der *Simple_name* ist mehrdeutig und ein Fehler während der Kompilierung auftritt.
      * Andernfalls die *Simple_name* verweist auf den Namespace mit dem Namen `I` in `N`.
   *  Andernfalls gilt: Wenn `N` enthält einen zugreifbarer Typ, der mit Namen `I` und `K`  Typparameter, klicken Sie dann:
      * Wenn `K` ist 0 (null) und den Speicherort, in denen die *Simple_name* tritt auf, wird durch eine Namespacedeklaration zur eingeschlossen `N` und die Namespacedeklaration enthält eine *Extern_alias_directive*oder *Using_alias_directive* , die den Namen zuordnet `I` mit einem Namespace oder Typ, der *Simple_name* ist mehrdeutig und ein Fehler während der Kompilierung auftritt.
      * Andernfalls die *Namespace_or_type_name* verweist auf den Typ mit den Argumenten angegebenen Typs erstellt.
   *  Andernfalls gilt: Wenn der Speicherort, in dem die *Simple_name* tritt auf, wird durch eine Namespacedeklaration zur eingeschlossen `N`:
      * Wenn `K` ist 0 (null) und die Namespacedeklaration enthält eine *Extern_alias_directive* oder *Using_alias_directive* , die den Namen verknüpft `I` mit einem importierten Namespace oder Der Typ, und klicken Sie dann die *Simple_name* bezieht sich auf diesen Namespace oder Typ.
      * Andernfalls, wenn die Deklarationen für Namespaces und importiert die *Using_namespace_directive*s und *Using_static_directive*s der Namespacedeklaration enthalten genau ein zugreifbarer Typ oder ohne Erweiterung statischen Member mit Namen `I` und `K`  Typparameter, und klicken Sie dann die *Simple_name* bezieht sich auf den Typ oder Member, die mit den Argumenten angegebenen Typs erstellt.
      * Andernfalls, wenn die Namespaces und Typen von importiert die *Using_namespace_directive*s der Namespacedeklaration enthalten mehr als ein zugreifbarer Typ oder nicht-Erweiterungsmethode statischen Member mit Namen `I` und `K`  Typparameter, und klicken Sie dann die *Simple_name* ist mehrdeutig und ein Fehler auftritt.

   Beachten Sie, dass dieser gesamte Schritt genau mit dem entsprechenden Schritt bei der Verarbeitung von parallelen ist eine *Namespace_or_type_name* ([Namespace und Typnamen](basic-concepts.md#namespace-and-type-names)).

*  Andernfalls die *Simple_name* ist nicht definiert und ein Fehler während der Kompilierung auftritt.


### <a name="parenthesized-expressions"></a>Ausdrücke in Klammern

Ein *Parenthesized_expression* besteht aus einem *Ausdruck* in Klammern eingeschlossen.

```antlr
parenthesized_expression
    : '(' expression ')'
    ;
```

Ein *Parenthesized_expression* wird ausgewertet, durch die Auswertung der *Ausdruck* innerhalb der Klammern. Wenn die *Ausdruck* innerhalb der Klammern gibt an einen Namespace oder Typ, ein Fehler während der Kompilierung auftritt. Andernfalls das Ergebnis der *Parenthesized_expression* ist das Ergebnis der Auswertung der enthaltenen *Ausdruck*.

### <a name="member-access"></a>Memberzugriff

Ein *Member_access* besteht aus einem *Primary_expression*, *Predefined_type*, oder ein *Qualified_alias_member*, gefolgt von einem"`.`"token, gefolgt von einem *Bezeichner*, optional gefolgt von einem *Type_argument_list*.

```antlr
member_access
    : primary_expression '.' identifier type_argument_list?
    | predefined_type '.' identifier type_argument_list?
    | qualified_alias_member '.' identifier
    ;

predefined_type
    : 'bool'   | 'byte'  | 'char'  | 'decimal' | 'double' | 'float' | 'int' | 'long'
    | 'object' | 'sbyte' | 'short' | 'string'  | 'uint'   | 'ulong' | 'ushort'
    ;
```

Die *Qualified_alias_member* Produktion wird definiert, [Namespace Alias Qualifiers](namespaces.md#namespace-alias-qualifiers).

Ein *Member_access* ist entweder der Form `E.I` oder des Formulars `E.I<A1, ..., Ak>`, wobei `E` ist ein primärer Ausdruck, `I` ist eine einmalige Kennung und `<A1, ..., Ak>` ist eine optionale  *Type_argument_list*. Wenn kein *Type_argument_list* wird angegeben, sollten Sie `K` 0 (null).

Ein *Member_access* mit einem *Primary_expression* des Typs `dynamic` dynamisch gebunden ist ([dynamische Bindung](expressions.md#dynamic-binding)). In diesem Fall der Compiler den Memberzugriff als Eigenschaftszugriff vom Typ klassifiziert `dynamic`. Den Regeln unten, um zu bestimmen, die Bedeutung von der *Member_access* gelten dann zur Laufzeit, der Kompilierzeittyp von der Laufzeit-Typinformationen anstelle der *Primary_expression*. Wenn diese Klassifizierung Laufzeit zu einer Methodengruppe führt, und klicken Sie dann der Memberzugriff muss die *Primary_expression* von einer *Invocation_expression*.

Die *Member_access* ausgewertet und wie folgt klassifiziert:

*  Wenn `K` ist 0 (null) und `E` ist ein Namespace und `E` enthält einen geschachtelten Namespace mit dem Namen `I`, lautet das Ergebnis dieses Namespace.
*  Andernfalls gilt: Wenn `E` ist ein Namespace und `E` enthält einen zugreifbarer Typ, der mit Namen `I` und `K`  Typparameter, und klicken Sie dann das Ergebnis dieses Typs mit den Argumenten angegebenen Typs erstellt wird.
*  Wenn `E` ist eine *Predefined_type* oder *Primary_expression* als Typ klassifiziert werden, wenn `E` ist es sich nicht um ein Typparameter, und wenn eine Suche nach Membern ([Membersuche](expressions.md#member-lookup)) der `I` in `E` mit `K`  Typparameter erzeugt eine Übereinstimmung, `E.I` ausgewertet und wie folgt klassifiziert:
   *  Wenn `I` identifiziert einen Typ, lautet das Ergebnis dieses Typs mit den Argumenten angegebenen Typs erstellt.
   *  Wenn `I` eine oder mehrere Methoden, identifiziert, und klicken Sie dann das Ergebnis einer Methodengruppe ohne zugeordneten Instanzausdruck ist. Wenn eine Liste der Typargumente angegeben wurde, wird es verwendet eine generische Methode aufzurufen ([Methodenaufrufe](expressions.md#method-invocations)).
   *  Wenn `I` identifiziert eine `static` -Eigenschaft, und klicken Sie dann auf das Ergebnis ist ein Eigenschaftenzugriff ohne zugeordneten Instanzausdruck.
   *  Wenn `I` identifiziert eine `static` Feld:
      * Wenn das Feld `readonly` der Verweis findet im statischen Konstruktor der Klasse oder Struktur, die in der das Feld deklariert wird, und das Ergebnis ist ein Wert, d. h. der Wert des statischen Felds `I` in `E`.
      * Andernfalls ist das Ergebnis einer Variablen, d. h. das statische Feld `I` in `E`.
   *  Wenn `I` identifiziert eine `static` Ereignis:
      * Im Falle der Verweis innerhalb der Klasse oder Struktur, die in der das Ereignis deklariert ist, und das Ereignis deklariert wurde, ohne *Event_accessor_declarations* ([Ereignisse](classes.md#events)), klicken Sie dann `E.I` genau verarbeitet wird als ob `I` wurden von ein statischen Feld.
      * Andernfalls ist das Ergebnis ein Ereigniszugriff ohne zugeordneten Instanzausdruck.
   *  Wenn `I` eine Konstante ist, identifiziert, und klicken Sie dann das Ergebnis ein Wert, nämlich den Wert dieser Konstanten ist.
    * Wenn `I` einen Enumerationsmember identifiziert, und klicken Sie dann das Ergebnis ein Wert, nämlich der Wert dieses Elements der Enumeration ist.
    * Andernfalls `E.I` ist ein ungültiger Memberverweis, und ein Fehler während der Kompilierung auftritt.
*  Wenn `E` ist ein Zugriff auf Eigenschaften, Indexerzugriff, Variable oder -Wert, dessen Typ von `T`, und eine Suche nach Membern ([Membersuche](expressions.md#member-lookup)) der `I` in `T` mit `K`  Typargumente erzeugt eine Übereinstimmung, `E.I` ausgewertet und wie folgt klassifiziert:
   *  Wenn zunächst `E` ist eine Eigenschaft oder Indexerzugriff, und klicken Sie dann den Wert der Eigenschaft oder Indexerzugriff abgerufen ([Werte Ausdrücke](expressions.md#values-of-expressions)) und `E` ist, die als Wert neu klassifiziert.
   *  Wenn `I` eine oder mehrere Methoden, identifiziert, ist das Ergebnis einer Methodengruppe mit einem zugeordneten Instanzausdruck von `E`. Wenn eine Liste der Typargumente angegeben wurde, wird es verwendet eine generische Methode aufzurufen ([Methodenaufrufe](expressions.md#method-invocations)).
   *  Wenn `I` identifiziert eine Instance-Eigenschaft
      * Wenn `E` ist `this`, `I` identifiziert eine automatisch implementierte Eigenschaft ([automatisch implementierten Eigenschaften](classes.md#automatically-implemented-properties)) ohne einen Setter und der Verweis erfolgt innerhalb eines Instanzkonstruktors für einen Klassen-oder Strukturtyp `T`, lautet das Ergebnis einer Variablen, d. h. das ausgeblendete Unterstützungsfeld für den Auto-Eigenschaft vom `I` in der Instanz von `T` von bestimmten `this`.
      * Andernfalls ist das Ergebnis ein Eigenschaftszugriff mit einem zugeordneten Instanzausdruck von `E`.
   *  Wenn `T` ist eine *Class_type* und `I` identifiziert ein Instanzenfeld, *Class_type*:
      * Wenn der Wert des `E` ist `null`, ein `System.NullReferenceException` ausgelöst.
      * Wenn das Feld ist, andernfalls `readonly` der Verweis findet keinen Instanzenkonstruktor der Klasse in der das Feld deklariert wird, und das Ergebnis ist ein Wert, nämlich der Wert des Felds `I` in das Objekt, das auf `E`.
      * Andernfalls ist das Ergebnis einer Variablen, d. h. das Feld `I` in das Objekt, das auf `E`.
   *  Wenn `T` ist eine *Struct_type* und `I` identifiziert ein Instanzenfeld, *Struct_type*:
      * Wenn `E` ist ein Wert, oder wenn das Feld `readonly` der Verweis findet keinen Instanzenkonstruktor der Struktur in der das Feld deklariert wird, und das Ergebnis ist ein Wert, nämlich der Wert des Felds `I` in der Strukturinstanz, die vom  `E`.
      * Andernfalls ist das Ergebnis einer Variablen, d. h. das Feld `I` in der Strukturinstanz, die vom `E`.
   *  Wenn `I` bezeichnet ein Instanzereignis:
      * Im Falle der Verweis innerhalb der Klasse oder Struktur, die in der das Ereignis deklariert ist, und das Ereignis deklariert wurde, ohne *Event_accessor_declarations* ([Ereignisse](classes.md#events)), und der Verweis findet nicht statt, als die linke Seite des eine `+=` oder `-=` Operator an, klicken Sie dann `E.I` verarbeitet wird, genau wie `I` wurde ein Instanzfeld.
      * Das Ergebnis ist, andernfalls ein Ereigniszugriff mit einem zugeordneten Instanzausdruck von `E`.
*  Es wird versucht, andernfalls verarbeitet `E.I` als ein Aufruf der Erweiterungsmethode ([Erweiterung Methodenaufrufe](expressions.md#extension-method-invocations)). Wenn dies fehlschlägt, `E.I` ist ein ungültiger Memberverweis, und beim Auftreten eines Laufzeitfehlers-Bindung.

#### <a name="identical-simple-names-and-type-names"></a>Identische einfache Namen und Typ

In ein Memberzugriff des Formulars `E.I`, wenn `E` ist ein einzelner Bezeichner, und wenn die Bedeutung der `E` als eine *Simple_name* ([einfache Namen](expressions.md#simple-names)) ist eine Konstante, Feld, Eigenschaft lokale Variable oder Parameter mit den gleichen Typ wie die Bedeutung der `E` als eine *Type_name* ([Namespace und Typnamen](basic-concepts.md#namespace-and-type-names)), und klicken Sie dann beide Bedeutungen von `E` sind zulässig. Die zwei möglichen Bedeutungen der `E.I` sind nie mehrdeutig, da `I` muss unbedingt ein Member des Typs `E` in beiden Fällen. Das heißt, die Regel einfach ermöglicht Zugriff auf die statische Member und geschachtelte Typen von `E` , in denen ein Fehler während der Kompilierung würde andernfalls aufgetreten. Zum Beispiel:
```csharp
struct Color
{
    public static readonly Color White = new Color(...);
    public static readonly Color Black = new Color(...);

    public Color Complement() {...}
}

class A
{
    public Color Color;                // Field Color of type Color

    void F() {
        Color = Color.Black;           // References Color.Black static member
        Color = Color.Complement();    // Invokes Complement() on Color field
    }

    static void G() {
        Color c = Color.White;         // References Color.White static member
    }
}
```

#### <a name="grammar-ambiguities"></a>Grammatik Mehrdeutigkeiten

Die Produktionen für *Simple_name* ([einfache Namen](expressions.md#simple-names)) und *Member_access* ([Memberzugriff](expressions.md#member-access)) erhalten, allerdings haben Mehrdeutigkeiten in Anstieg der Grammatik für Ausdrücke. Beispielsweise ist die Anweisung:
```
F(G<A,B>(7));
```
interpretiert werden konnte, wie ein Aufruf von `F` mit zwei Argumenten `G < A` und `B > (7)`. Alternativ dazu, wie ein Aufruf von interpretiert werden konnte `F` mit einem Argument, das einen Aufruf einer generischen Methode ist `G` mit zwei Typargumente und eine reguläre Argument.

Wenn eine Folge von Token (in Kontext) kann als analysiert werden eine *Simple_name* ([einfache Namen](expressions.md#simple-names)), *Member_access* ([Memberzugriff](expressions.md#member-access)), oder *Pointer_member_access* ([Zeigermemberzugriff](unsafe-code.md#pointer-member-access)) endet mit einem *Type_argument_list* ([Typargumente](types.md#type-arguments)), wird die Token, die direkt nach dem schließenden `>` Token wird untersucht. Wenn einer der
```csharp
(  )  ]  }  :  ;  ,  .  ?  ==  !=  |  ^
```
die *Type_argument_list* wird beibehalten, als Teil der *Simple_name*, *Member_access* oder *Pointer_member_access* sowie andere mögliche Analyse der Sequenz von Token wird verworfen. Andernfalls die *Type_argument_list* ist nicht als Teil der *Simple_name*, *Member_access* oder *Pointer_member_access*, auch wenn es keine andere mögliche Analyse der Sequenz von Token. Beachten Sie, dass diese Regeln nicht angewendet, bei der Analyse einer *Type_argument_list* in einem *Namespace_or_type_name* ([Namespace und Typnamen](basic-concepts.md#namespace-and-type-names)). Die Anweisung
```csharp
F(G<A,B>(7));
```
wird nach dieser Regel interpretiert werden als Aufruf an `F` mit einem Argument, das einen Aufruf einer generischen Methode ist `G` mit zwei Typargumente und eine reguläre-Argument. Die Anweisungen
```csharp
F(G < A, B > 7);
F(G < A, B >> 7);
```
Jede interpretiert als Aufruf an `F` mit zwei Argumenten. Die Anweisung
```csharp
x = F < A > +y;
```
wird als ein kleiner-als-Operator, der größer als-Operator, und unäre plus -Operator, interpretiert werden, als wäre die Anweisung geschrieben worden `x = (F < A) > (+y)`, anstatt als eine *Simple_name* mit einer *Type_argument_list* gefolgt von einem binären plus -Operator. In der Anweisung
```csharp
x = y is C<T> + z;
```
die Token `C<T>` interpretiert werden eine *Namespace_or_type_name* mit einem *Type_argument_list*.

### <a name="invocation-expressions"></a>Aufrufausdrücke

Ein *Invocation_expression* wird verwendet, um eine Methode aufzurufen.

```antlr
invocation_expression
    : primary_expression '(' argument_list? ')'
    ;
```

Ein *Invocation_expression* dynamisch gebunden ist ([dynamische Bindung](expressions.md#dynamic-binding)), wenn mindestens eine der folgenden enthält:

* Die *Primary_expression* weist den Typ der Kompilierzeit `dynamic`.
* Mindestens ein Argument des optionalen *Argument_list* weist den Typ der Kompilierzeit `dynamic` und *Primary_expression* verfügt nicht über einen Delegattyp aufweisen.

In diesem Fall der Compiler klassifiziert die *Invocation_expression* als ein Wert vom Typ `dynamic`. Den Regeln unten, um zu bestimmen, die Bedeutung des das *Invocation_expression* gelten dann zur Laufzeit, der Kompilierzeittyp, der von der Laufzeit-Typinformationen anstelle der *Primary_expression* und Argumente, die den Zeitpunkt der Kompilierung Typ `dynamic`. Wenn die *Primary_expression* hat keinen Kompilierzeittyp `dynamic`, und klicken Sie dann der Methodenaufruf eine begrenzte Kompilierung Zeit Überprüfung durchgeführt wird, wie in beschrieben [Überprüfungen zur Kompilierzeit der dynamischen überladungsauflösung ](expressions.md#compile-time-checking-of-dynamic-overload-resolution).

Die *Primary_expression* von einer *Invocation_expression* einer Methodengruppe oder der Wert muss eine *Delegate_type*. Wenn die *Primary_expression* eine Methodengruppe wird die *Invocation_expression* ist der Aufruf einer Methode ([Methodenaufrufe](expressions.md#method-invocations)). Wenn die *Primary_expression* ist ein Wert, der eine *Delegate_type*, *Invocation_expression* einem Delegataufruf aufgerufen wird ([AufrufeDelegieren](expressions.md#delegate-invocations)). Wenn die *Primary_expression* ist weder eine Methodengruppe noch den Wert einer *Delegate_type*, tritt ein Fehler während der Bindung.

Der optionale *Argument_list* ([Argumentlisten](expressions.md#argument-lists)) Werte oder Variablenverweise für die Parameter der Methode enthält.

Das Ergebnis der Auswertung einer *Invocation_expression* wird wie folgt klassifiziert:

*  Wenn die *Invocation_expression* Ruft eine Methode oder ein Delegat, der gibt `void`, das Ergebnis ist "nothing". Ein Ausdruck, der Klassifizierung als "nothing" nur im Kontext zulässig ist eine *Statement_expression* ([ausdrucksanweisungen](statements.md#expression-statements)) oder als Text einer *Lambda_expression*([Anonyme Funktionsausdrücke](expressions.md#anonymous-function-expressions)). Andernfalls tritt ein Fehler während der Bindung auf.
*  Andernfalls ist das Ergebnis den Wert der von der Methode oder Delegat zurückgegebene Typ.

#### <a name="method-invocations"></a>Methodenaufrufe

Für den Aufruf einer Methode die *Primary_expression* von der *Invocation_expression* muss eine Methodengruppe sein. Die Methodengruppe identifiziert, die eine aufzurufende Methode oder den Satz von überladenen Methoden zur Auswahl einer bestimmten Methode aufrufen. Im letzteren Fall Bestimmung der bestimmten Methode aufzurufende wird auf Grundlage des Kontexts bereitgestellt, die von den Typen der Argumente in der *Argument_list*.

Die Bindung-Time-Verarbeitung eines Methodenaufrufs des Formulars `M(A)`, wobei `M` ist eine Methodengruppe (möglicherweise auch eine *Type_argument_list*), und `A` ist eine optionale *Argument_ Liste*, umfasst die folgenden Schritte aus:

*  Der Satz von Kandidaten-Methoden für den Methodenaufruf wird erstellt. Für jede Methode `F` die Methodengruppe zugeordneten `M`:
   *  Wenn `F` ist nicht generisch, `F` ist ein Kandidat bei:
      * `M` verfügt über keine Liste der Typargumente, und
      * `F` gilt in Bezug auf `A` ([Anwendbarer Funktionsmember](expressions.md#applicable-function-member)).
   *  Wenn `F` ist generisch und `M` verfügt über keine Liste der Typargumente, `F` ist ein Kandidat bei:
      * Typrückschluss ([Typrückschluss](expressions.md#type-inference)) erfolgreich ist, wird eine Liste der Typargumente für den Aufruf ableiten und
      * Sobald die abgeleiteten Typargumente für die entsprechende Methode Typparameter ersetzt werden, erfüllen alle konstruierte Typen in der Parameterliste von F deren Einschränkungen ([Einschränkungen erfüllen](types.md#satisfying-constraints)), und der Parameterliste der `F` gilt in Bezug auf `A` ([Anwendbarer Funktionsmember](expressions.md#applicable-function-member)).
   *  Wenn `F` ist generisch und `M` enthält eine Liste der Typargumente, `F` ist ein Kandidat bei:
      * `F` die gleiche Anzahl von Methodentypparametern aufweist, wie in der Liste der Typargumente, bereitgestellt wurden und
      * Sobald die Typargumente für die entsprechende Methode Typparameter ersetzt werden, erfüllen alle konstruierte Typen in der Parameterliste von F deren Einschränkungen ([Einschränkungen erfüllen](types.md#satisfying-constraints)), und der Liste der Parameter `F` gilt in Bezug auf `A` ([Anwendbarer Funktionsmember](expressions.md#applicable-function-member)).
*  Die Gruppe der möglichen Methoden werden reduziert, um nur die Methoden aus den am stärksten abgeleiteten Typen enthalten: Für jede Methode `C.F` in der Gruppe, in denen `C` ist der Typ, in dem die Methode `F` deklariert ist, werden alle Methoden deklariert werden, in dem Basistyp `C` aus der Gruppe entfernt werden. Darüber hinaus Wenn `C` ist ein Klassentyp außer `object`, alle Methoden, die in einen Schnittstellentyp deklariert werden aus der Gruppe entfernt. (Diese zweite Regel hat nur Auswirkungen, wenn die Methodengruppe das Ergebnis einer Suche nach Membern für einen Typparameter mit einer effektiven Klasse als Objekt und eine effektive nicht leere-Schnittstelle festgelegt ist.)
*  Wenn die resultierende Menge der möglichen Methoden leer ist, klicken Sie dann auf die folgenden Schritte aus weiteren Verarbeitung abgebrochen werden, und stattdessen wird versucht, den Aufruf als ein Aufruf der Erweiterungsmethode verarbeiten ([Erweiterung Methodenaufrufe](expressions.md#extension-method-invocations)). Wenn dies fehlschlägt, klicken Sie dann keine anwendbaren Methoden vorhanden, und beim Auftreten eines Laufzeitfehlers-Bindung.
*  Die beste Methode, der den Satz von möglichen Methoden wird mit den Regeln der überladungsauflösung des identifiziert [Überladungsauflösung](expressions.md#overload-resolution). Wenn eine einzelne bewährten Methode kann nicht identifiziert werden, der Methodenaufruf ist mehrdeutig, und beim Auftreten eines Laufzeitfehlers-Bindung. Bei der Auflösung von funktionsüberladungen durchführen zu können, die Parameter einer generischen Methode gelten nach und Ersetzen Sie dabei die Typargumente (angegeben oder abgeleitet) für die entsprechenden Typparameter der Methode.
*  Abschließende Überprüfung von der gewählten bewährte Methode wird ausgeführt:
   * Die Methode wird im Rahmen der Methodengruppe überprüft: Wenn die beste Methode, eine statische Methode handelt, muss die Methodengruppe zurückzuführen sein, aus einer *Simple_name* oder ein *Member_access* über einen Typ. Wenn die beste Methode eine Instanzmethode ist, muss die Methodengruppe zurückzuführen sein, aus einer *Simple_name*, eine *Member_access* durch eine Variable oder ein Wert, oder ein *Base_access*. Wenn keine dieser Anforderungen auf "true" ist, tritt ein Fehler während der Bindung.
   * Wenn die bewährte Methode eine generische Methode ist, werden die Typargumente (angegeben oder abgeleitet) für die Einschränkungen überprüft ([Einschränkungen erfüllen](types.md#satisfying-constraints)) für die generische Methode deklariert. Wenn jedes Typargument der entsprechenden Einschränkung(en): für den Typparameter nicht erfüllt, tritt ein Fehler während der Bindung.

Sobald eine Methode ausgewählt und zum Zeitpunkt der Bindung durch die oben genannten Schritte überprüft wurde, wird der tatsächlichen Laufzeit-Aufruf verarbeitet, gemäß den Regeln von der Members-Funktionsaufruf in beschrieben [Überprüfungen zur Kompilierzeit der dynamischen überladungsauflösung ](expressions.md#compile-time-checking-of-dynamic-overload-resolution).

Die Auswirkung der oben beschriebenen Regeln intuitivere lautet wie folgt aus: Zum Suchen von bestimmten Methode durch Aufruf einer Methode aufgerufen, beginnen Sie mit der durch Aufruf der Methode angegebenen Typ und entlang der Vererbungskette fortgesetzt, bis mindestens ein anwendbar, zugegriffen werden kann, nicht-Override-Methodendeklaration gefunden werden. Klicken Sie dann führen Sie den Typrückschluss, und der überladungsauflösung, auf den Satz von anwendbar, zugegriffen werden kann, nicht-Override-Methoden, die in diesem Typ deklariert, und rufen die Methode so ausgewählt. Wenn keine Methode gefunden wurde, versuchen Sie es stattdessen den Aufruf als ein Aufruf der Erweiterungsmethode zu verarbeiten.

#### <a name="extension-method-invocations"></a>Extension-Methodenaufrufe

In einem Methodenaufruf ([Aufrufe in geschachtelten Instanzen](expressions.md#invocations-on-boxed-instances)) von einem der Formate
```csharp
expr . identifier ( )

expr . identifier ( args )

expr . identifier < typeargs > ( )

expr . identifier < typeargs > ( args )
```
Wenn der normale Verarbeitung des Aufrufs keine anwendbaren Methoden findet, wird versucht, das Konstrukt als ein Aufruf der Erweiterungsmethode zu verarbeiten. Wenn *Expr* oder eines der *Args* weist den Typ der Kompilierzeit `dynamic`, Erweiterungsmethoden werden nicht angewendet.

Das Ziel besteht darin, mit der besten finden *Type_name* `C`, sodass die entsprechenden Aufruf der statischen Methode durchgeführt werden kann:
```csharp
C . identifier ( expr )

C . identifier ( expr , args )

C . identifier < typeargs > ( expr )

C . identifier < typeargs > ( expr , args )
```

Eine Erweiterungsmethode `Ci.Mj` ist ***berechtigte*** wenn:

*  `Ci` ist eine nicht generische und nicht geschachtelten-Klasse
*  Der Name des `Mj` ist *Bezeichner*
*  `Mj` ist verfügbar und anwendbar, wenn auf die Argumente als statische Methode angewendet werden, wie oben gezeigt
*  Eine implizite identitätsänderung, Verweis- oder Boxing-Konvertierung vorhanden ist, von *Expr* in den Typ des ersten Parameters von `Mj`.

Die Suche nach `C` wird wie folgt:

*  Beginnend mit der nächsten einschließenden Deklaration des Namespaces anhand jedes einschließenden Namespace-Deklaration und endet mit der enthaltenden Kompilierungseinheit, werden aufeinander folgende Versuche unternommen, um einen möglichen Satz von Erweiterungsmethoden ermitteln:
   * Wenn der angegebene Namespace oder die Kompilierung Organizational Unit direkt nicht generische Typdeklarationen aus `Ci` mit berechtigten Erweiterungsmethoden `Mj`, und klicken Sie dann der Satz von dieser Erweiterungsmethoden die Candidate-Gruppe ist.
   * Wenn Typen `Ci` importiert *Using_static_declarations* und direkt im von importierten Namespaces deklariert *Using_namespace_directive*s in der angegebenen Namespace oder die Kompilierung Einheit direkt enthält Erweiterungsmethoden für berechtigte `Mj`, und klicken Sie dann der Satz von dieser Erweiterungsmethoden die Candidate-Gruppe ist.
*  Wenn kein Kandidat festgelegt, die in jeder einschließenden Namespace-Deklaration oder Kompilierung Einheit gefunden wird, tritt ein Fehler während der Kompilierung.
*  Andernfalls wird die Auflösung von funktionsüberladungen Kandidaten festlegen, wie in beschrieben angewendet ([Überladungsauflösung](expressions.md#overload-resolution)). Wenn bewährte Methoden gefunden wird, tritt ein Fehler während der Kompilierung.
*  `C` ist der Typ, in dem die beste Methode als eine Erweiterungsmethode deklariert ist.

Mithilfe von `C` als Ziel, Aufruf der Methode dann als Aufruf einer statischen Methode verarbeitet ([Überprüfungen zur Kompilierzeit der dynamischen überladungsauflösung](expressions.md#compile-time-checking-of-dynamic-overload-resolution)).

Die vorangehenden Regeln bedeuten, dass es sich bei Instanzmethoden Vorrang vor Erweiterungsmethoden haben, dass Erweiterungsmethoden zur Verfügung, in der inneren Namespacedeklarationen gegenüber den Erweiterungsmethoden in den äußeren Namespace-Deklarationen und die Erweiterung verfügbar Vorrang Methoden, die direkt in einem Namespace deklariert haben Vorrang vor Erweiterungsmethoden, die in diesen gleichen Namespace mit einem importierten Namespace-Anweisung. Zum Beispiel:
```csharp
public static class E
{
    public static void F(this object obj, int i) { }

    public static void F(this object obj, string s) { }
}

class A { }

class B
{
    public void F(int i) { }
}

class C
{
    public void F(object obj) { }
}

class X
{
    static void Test(A a, B b, C c) {
        a.F(1);              // E.F(object, int)
        a.F("hello");        // E.F(object, string)

        b.F(1);              // B.F(int)
        b.F("hello");        // E.F(object, string)

        c.F(1);              // C.F(object)
        c.F("hello");        // C.F(object)
    }
}
```

Im Beispiel `B`Methode hat Vorrang vor die erste Erweiterungsmethode, und `C`Methode hat Vorrang vor beiden Erweiterungsmethoden.

```csharp
public static class C
{
    public static void F(this int i) { Console.WriteLine("C.F({0})", i); }
    public static void G(this int i) { Console.WriteLine("C.G({0})", i); }
    public static void H(this int i) { Console.WriteLine("C.H({0})", i); }
}

namespace N1
{
    public static class D
    {
        public static void F(this int i) { Console.WriteLine("D.F({0})", i); }
        public static void G(this int i) { Console.WriteLine("D.G({0})", i); }
    }
}

namespace N2
{
    using N1;

    public static class E
    {
        public static void F(this int i) { Console.WriteLine("E.F({0})", i); }
    }

    class Test
    {
        static void Main(string[] args)
        {
            1.F();
            2.G();
            3.H();
        }
    }
}
```

Die Ausgabe dieses Beispiels ist:
```
E.F(1)
D.G(2)
C.H(3)
```
`D.G` hat Vorrang vor `C.G`, und `E.F` hat Vorrang vor beide `D.F` und `C.F`.

#### <a name="delegate-invocations"></a>Delegieren von Aufrufen

Für einen Delegataufruf der *Primary_expression* von der *Invocation_expression* muss sein Wert eine *Delegate_type*. Darüber hinaus in Betracht ziehen die *Delegate_type* ein Funktionsmember mit derselben Parameterliste als sein die *Delegate_type*, wird die *Delegate_type* muss anwendbar ( [Anwendbarer Funktionsmember](expressions.md#applicable-function-member)) in Bezug auf die *Argument_list* von der *Invocation_expression*.

Die Run-Time-Verarbeitung von einem Delegataufruf des Formulars `D(A)`, wobei `D` ist eine *Primary_expression* von einer *Delegate_type* und `A` ist eine optionale *Argument_list*, umfasst die folgenden Schritte aus:

*  `D` wird ausgewertet. Wenn diese Evaluierungsversion auf eine Ausnahme auslöst, werden keine weiteren Schritte ausgeführt.
*  Der Wert des `D` wird überprüft, um gültig sein. Wenn der Wert des `D` ist `null`, `System.NullReferenceException` wird ausgelöst, und keine weiteren Schritte ausgeführt werden.
*  Andernfalls `D` ist ein Verweis auf eine Delegatinstanz. Member Funktionsaufrufen ([Überprüfungen zur Kompilierzeit der dynamischen überladungsauflösung](expressions.md#compile-time-checking-of-dynamic-overload-resolution)) jede der aufrufbaren Entitäten in der Aufrufliste des Delegaten ausgeführt werden. Für aufrufbare Entitäten, die aus einer Instanz und eine Instanzmethode besteht ist die Instanz für den Aufruf der Instanz, die in die aufrufbare Entität enthalten sind.

### <a name="element-access"></a>Elementzugriff

Ein *Element_access* besteht aus einer *Primary_no_array_creation_expression*, gefolgt von einer "`[`" token, gefolgt von ein *Argument_list*, gefolgt von einer " `]`"token. Die *Argument_list* besteht aus einem oder mehreren *Argument*s, die durch Kommas getrennt.

```antlr
element_access
    : primary_no_array_creation_expression '[' expression_list ']'
    ;
```

Die *Argument_list* von einem *Element_access* darf nicht enthalten `ref` oder `out` Argumente.

Ein *Element_access* dynamisch gebunden ist ([dynamische Bindung](expressions.md#dynamic-binding)), wenn mindestens eine der folgenden enthält:

* Die *Primary_no_array_creation_expression* weist den Typ der Kompilierzeit `dynamic`.
* Mindestens ein Ausdruck, der die *Argument_list* weist den Typ der Kompilierzeit `dynamic` und *Primary_no_array_creation_expression* verfügt nicht über einen Arraytyp.

In diesem Fall der Compiler klassifiziert die *Element_access* als ein Wert vom Typ `dynamic`. Den Regeln unten, um zu bestimmen, die Bedeutung des das *Element_access* gelten dann zur Laufzeit, der Kompilierzeittyp, der von der Laufzeit-Typinformationen anstelle der *Primary_no_array_creation_expression*und *Argument_list* Ausdrücke mit der Kompilierzeittyp `dynamic`. Wenn die *Primary_no_array_creation_expression* hat keinen Kompilierzeittyp `dynamic`, und klicken Sie dann den Elementzugriff eine begrenzte Kompilierung Zeit Überprüfung durchgeführt wird, wie in beschrieben [Überprüfungen zur Kompilierzeit dynamisch Auflösen der Überladung](expressions.md#compile-time-checking-of-dynamic-overload-resolution).

Wenn die *Primary_no_array_creation_expression* von eine *Element_access* ist ein Wert, der eine *Array_type*, wird die *Element_access* ist ein Array-Zugriff ([Array Zugriff](expressions.md#array-access)). Andernfalls die *Primary_no_array_creation_expression* muss eine Variable oder den Wert der Klasse, Struktur oder Schnittstellentyp, der einen oder mehrere Indexer-Member, in diesem Fall verfügt die *Element_access* ist ein Indexzugriff ([Indexerzugriff](expressions.md#indexer-access)).

#### <a name="array-access"></a>Arrayzugriff

Für ein Arrayzugriff verfügt die *Primary_no_array_creation_expression* von der *Element_access* muss sein Wert eine *Array_type*. Darüber hinaus die *Argument_list* eines Arrays Zugriff ist nicht zulässig, benannte Argumente enthält. Die Anzahl der Ausdrücke in der *Argument_list* muss identisch mit den Rang der *Array_type*, und jeder Ausdruck muss vom Typ `int`, `uint`, `long`, `ulong`, oder muss implizit in eine oder mehrere der folgenden Typen sein.

Das Ergebnis der Auswertung ein Arrayzugriff verfügt, wird eine Variable vom Typ Elements des Arrays, nämlich dem Array-Element, das ausgewählt, indem die Werte der Ausdrücke in der *Argument_list*.

Die Verarbeitung zur Laufzeit ein Arrayzugriff verfügt der Form `P[A]`, wobei `P` ist eine *Primary_no_array_creation_expression* von einer *Array_type* und `A` ist ein *Argument_list*, umfasst die folgenden Schritte aus:

*  `P` wird ausgewertet. Wenn diese Evaluierungsversion auf eine Ausnahme auslöst, werden keine weiteren Schritte ausgeführt.
*  Der Index von Ausdrücken für die *Argument_list* werden in der Reihenfolge von links nach rechts ausgewertet. Befolgen Sie die Auswertung jedes Ausdrucks Index, eine implizite Konvertierung ([implizite Konvertierungen](conversions.md#implicit-conversions)) auf einen der folgenden Typen erfolgt: `int`, `uint`, `long`, `ulong`. Der erste Typ in dieser Liste, die für die eine implizite Konvertierung vorhanden ist, wird ausgewählt. Z. B. wenn der Indexausdruck vom Typ `short` klicken Sie dann eine implizite Konvertierung in `int` ausgeführt, da implizite Konvertierungen aus `short` zu `int` und `short` zu `long` sind möglich. Wenn die Auswertung von Ausdruckswerten Index oder die nachfolgenden implizite Konvertierung eine Ausnahme verursacht hat, klicken Sie dann keine weiteren Indexausdrücke werden ausgewertet, und keine weiteren Schritte ausgeführt werden.
*  Der Wert des `P` wird überprüft, um gültig sein. Wenn der Wert des `P` ist `null`, `System.NullReferenceException` wird ausgelöst, und keine weiteren Schritte ausgeführt werden.
*  Der Wert der einzelnen Ausdrücke aus der *Argument_list* aktiviert ist, für die eigentlichen Grenzen jeder Arraydimension der Arrayinstanz verweist `P`. Wenn eine oder mehrere Werte außerhalb des gültigen Bereichs, sind ein `System.IndexOutOfRangeException` wird ausgelöst, und keine weiteren Schritte ausgeführt werden.
*  Der Speicherort des Array-Elements vom Index-Ausdrücke wird berechnet, und Hier wird das Ergebnis der Arrayzugriff.

#### <a name="indexer-access"></a>Indexerzugriff

Indexzugriff die *Primary_no_array_creation_expression* von der *Element_access* muss eine Variable oder ein Wert, der eine Klasse, Struktur oder der Schnittstellentyp, und dieser Typ muss eine oder mehrere implementieren Indexer, die in Bezug auf gelten die *Argument_list* von der *Element_access*.

Die Bindung-Time-Verarbeitung der Indexzugriff des Formulars `P[A]`, wobei `P` ist eine *Primary_no_array_creation_expression* der Klasse, Struktur oder Schnittstellentyp `T`, und `A` ist ein *Argument_list*, umfasst die folgenden Schritte aus:

*  Der Satz von Indexern, die von bereitgestellte `T` erstellt wird. Der Satz besteht aus allen in deklarierten Indexer `T` oder einem Basistyp von `T` , die sich nicht `override` Deklarationen und sind im aktuellen Kontext zugegriffen werden kann ([Memberzugriff](basic-concepts.md#member-access)).
*  Die Gruppe ist für diesen Indexer, die anwendbar und nicht versteckt sind von anderen Indexern reduziert. Die folgenden Regeln gelten, jeder Indexer `S.I` in der Gruppe, in denen `S` ist der Typ, in dem der Indexer `I` deklariert wird:
   * Wenn `I` ist nicht anwendbar, in Bezug auf `A` ([Anwendbarer Funktionsmember](expressions.md#applicable-function-member)), klicken Sie dann `I` aus der Gruppe entfernt wird.
   * Wenn `I` gilt in Bezug auf `A` ([Anwendbarer Funktionsmember](expressions.md#applicable-function-member)), in dem Basistyp deklariert alle Indexer `S` aus der Gruppe entfernt werden.
   * Wenn `I` gilt in Bezug auf `A` ([Anwendbarer Funktionsmember](expressions.md#applicable-function-member)) und `S` ist ein Klassentyp außer `object`, alle in einer Schnittstelle deklarierten Indexer aus der Gruppe entfernt werden.
*  Wenn die resultierende Menge der Kandidat Indexer leer ist, klicken Sie dann keine anwendbaren Indexer vorhanden, und beim Auftreten eines Laufzeitfehlers-Bindung.
*  Der beste Indexer, der den Satz von Kandidaten Indexer wird mit den Regeln der überladungsauflösung des identifiziert [Überladungsauflösung](expressions.md#overload-resolution). Wenn ein einzelner bewährte Indexer kann nicht identifiziert werden, der Indexer ist nicht eindeutig, und beim Auftreten eines Laufzeitfehlers-Bindung.
*  Der Index von Ausdrücken für die *Argument_list* werden in der Reihenfolge von links nach rechts ausgewertet. Das Ergebnis der Verarbeitung der Indexerzugriff ist ein Ausdruck, der als Indexzugriff klassifiziert sind. Der Indexer Zugriffsausdruck verweist auf den Indexer, der im vorherigen Schritt bestimmt und verfügt über einen zugeordneten Instanzausdruck von `P` sowie eine Liste zugeordnete Argument `A`.

Je nach Kontext, in denen es verwendet wird, verursacht das Indexzugriff Aufrufen des entweder die *get-Accessor* oder *set-Accessor* des Indexers. Wenn der Indexzugriff auf das Ziel einer Zuweisung, ist die *set-Accessor* wird aufgerufen, um einen neuen Wert zuzuweisen ([einfache Zuweisung](expressions.md#simple-assignment)). In allen anderen Fällen die *get-Accessor* wird aufgerufen, um den aktuellen Wert zu erhalten ([Werte Ausdrücke](expressions.md#values-of-expressions)).

### <a name="this-access"></a>Dieser Zugriff

Ein *This_access* besteht aus dem reservierten Wort `this`.

```antlr
this_access
    : 'this'
    ;
```

Ein *This_access* ist zulässig, nur in der *Block* Instanzkonstruktor, eine Instanzmethode oder einen Instanzaccessor. Es verfügt über eine der folgenden Bedeutungen:

*  Wenn `this` werden in einem *Primary_expression* im Instanzenkonstruktor eine einer Klasse, die sie als Wert klassifiziert wird. Der Typ des Werts ist der Instanztyp ([den Instanztyp](classes.md#the-instance-type)) der Klasse in der ein Fehler auftritt, und der Wert ist ein Verweis auf das Objekt erstellt wird.
*  Wenn `this` werden in einem *Primary_expression* innerhalb einer Instanzenmethode oder Instanzaccessor einer Klasse, wird es als Wert klassifiziert. Der Typ des Werts ist der Instanztyp ([den Instanztyp](classes.md#the-instance-type)) der Klasse in der ein Fehler auftritt, und der Wert ist ein Verweis auf das Objekt für die die Methode oder der Accessor aufgerufen wurde.
*  Wenn `this` werden in einem *Primary_expression* innerhalb eines Instanzkonstruktors einer Struktur, wird er als Variable klassifiziert. Der Typ der Variablen ist der Instanztyp ([den Instanztyp](classes.md#the-instance-type)) der Struktur klicken Sie in die Nutzung wird, und die Variable verweist auf die Struktur, die erstellt wird. Die `this` Variable eines Instanzkonstruktors einer Struktur verhält sich genau wie ein `out` Parameter des Strukturtyps – insbesondere bedeutet dies, dass die Variable auf jeden Fall in allen Ausführungspfaden der Instanz zugewiesen werden muss Konstruktor.
*  Wenn `this` werden in einem *Primary_expression* innerhalb einer Instanzenmethode oder Instanzaccessor einer Struktur, wird er als Variable klassifiziert. Der Typ der Variablen ist der Instanztyp ([den Instanztyp](classes.md#the-instance-type)) der Struktur, in dem sich die Verwendung stattfindet.
   * Wenn die Methode oder der Accessor kein Iterator ist ([Iteratoren](classes.md#iterators)), wird die `this` Variable darstellt, die Struktur für die die Methode oder der Accessor wurde aufgerufen, und verhält sich genauso wie eine `ref` Parameter des Strukturtyps.
   * Wenn die Methode oder der Accessor ein Iterator ist, der `this` Variable darstellt, eine Kopie der Struktur für die die Methode oder der Accessor wurde aufgerufen, und verhält sich genau wie ein Value-Parameter des Strukturtyps.

Verwenden von `this` in einem *Primary_expression* in einem anderen Kontext als oben aufgeführt ist ein Fehler während der Kompilierung. Insbesondere ist es nicht möglich, verweisen auf `this` in einer statischen Methode, eine statische Eigenschaftenaccessor oder in einem *Variable_initializer* einer Felddeklaration.

### <a name="base-access"></a>Basiszugriff

Ein *Base_access* besteht aus dem reservierten Wort `base` gefolgt von entweder eine "`.`" Zugriffstoken und ein Bezeichner oder ein *Argument_list* in eckige Klammern eingeschlossen:

```antlr
base_access
    : 'base' '.' identifier
    | 'base' '[' expression_list ']'
    ;
```

Ein *Base_access* wird verwendet, um der Member der Basisklasse zuzugreifen, die von den entsprechend benannten Elemente in der aktuellen Klasse oder Struktur ausgeblendet werden. Ein *Base_access* ist zulässig, nur in der *Block* Instanzkonstruktor, eine Instanzmethode oder einen Instanzaccessor. Wenn `base.I` tritt auf, in einer Klasse oder Struktur `I` muss ein Member der Basisklasse der Klasse oder Struktur zu kennzeichnen. Auch wenn `base[E]` tritt auf, in einer Klasse ein passender Indexer muss vorhanden sein, in der Basisklasse.

Zum Zeitpunkt der Bindung *Base_access* Ausdrücken der Form `base.I` und `base[E]` werden ausgewertet, als ob sie geschrieben wurden `((B)this).I` und `((B)this)[E]`, wobei `B` ist die Basisklasse der Klasse oder eine Struktur, die in dem das Konstrukt auftritt. Daher `base.I` und `base[E]` entsprechen `this.I` und `this[E]`, mit Ausnahme von `this` als eine Instanz der Basisklasse angezeigt wird.

Wenn eine *Base_access* verweist auf ein virtuelle Funktion-Element (eine Methode, Eigenschaft, oder Indexer), die Bestimmung der Funktion zur Laufzeit aufzurufenden Member ([Überprüfungen zur Kompilierzeit der dynamischen überladungsauflösung ](expressions.md#compile-time-checking-of-dynamic-overload-resolution)) geändert wird. Der Funktionsmember der Member, die aufgerufen wird, richtet sich nach der Suche nach der am weitesten abgeleiteten Implementierung ([virtuelle Methoden](classes.md#virtual-methods)) des Funktionsmembers in Bezug auf `B` (anstelle von in Bezug auf den Laufzeittyp der `this`, wie üblich in einer nicht-Base Zugriff wäre). Daher innerhalb eine `override` von einer `virtual` Funktionsmember der Member, eine *Base_access* kann zum Aufrufen der geerbten Implementierung von der Funktionsmember der Member verwendet werden. Wenn das Funktionselement verweist eine *Base_access* ist eine abstrakte tritt ein Fehler während der Bindung.

### <a name="postfix-increment-and-decrement-operators"></a>Postfix-Inkrement und Dekrement-Operatoren

```antlr
post_increment_expression
    : primary_expression '++'
    ;

post_decrement_expression
    : primary_expression '--'
    ;
```

Der Operand eines Postfixinkrement oder Dekrement Operation muss ein Ausdruck, der als eine Variable, einen Eigenschaftenzugriff oder Indexzugriff klassifiziert sein. Das Ergebnis des Vorgangs ist ein Wert des gleichen Typs als Operand.

Wenn die *Primary_expression* weist den Typ der Kompilierzeit `dynamic` und klicken Sie dann der Operator dynamisch gebunden ist ([dynamische Bindung](expressions.md#dynamic-binding)), wird die *Post_increment_expression*oder *Post_decrement_expression* weist den Typ der Kompilierzeit `dynamic` und die folgenden Regeln gelten zur Laufzeit mit dem Laufzeit-Typ, der die *Primary_expression*.

Wenn der Operand einem Postfix erhöhen oder dekrementvorgang ist eine Eigenschaft oder Zugriffs auf Indexer, Eigenschaft oder des Indexers benötigen Sie Folgendes ein `get` und `set` Accessor. Wenn dies nicht der Fall ist, tritt ein Fehler während der Bindung.

Unäroperator der überladungsauflösung ([unäroperator überladungsauflösung](expressions.md#unary-operator-overload-resolution)) wird angewendet, um einen bestimmten Operator-Implementierung auswählen. Vordefinierte `++` und `--` Operatoren vorhanden, für die folgenden Typen: `sbyte`, `byte`, `short`, `ushort`, `int`, `uint`, `long`, `ulong`, `char` , `float`, `double`, `decimal`, und alle Enumerationstypen. Die vordefinierten `++` Operatoren zurückgegeben, den von den Operanden und den vordefinierten 1 hinzugefügt erzeugten Wert `--` Operatoren geben einen Wert durch Subtrahieren der Operand 1 zurück. In einem `checked` Kontext, wenn das Ergebnis dieser Addition oder Subtraktion außerhalb des Bereichs des Ergebnistyps ist und der Ergebnistyp ein ganzzahliger Typ oder ein Enumerationstyp ist, eine `System.OverflowException` ausgelöst.

Die Verarbeitung zur Laufzeit ein Postfixinkrement oder dekrementvorgang des Formulars `x++` oder `x--` umfasst die folgenden Schritte aus:

*   Wenn `x` wird als Variable klassifiziert:
    * `x` wird ausgewertet, um die Variable zu erstellen.
    * Der Wert des `x` gespeichert wird.
    * Der ausgewählte Operator wird aufgerufen, mit dem gespeicherten Wert von `x` als Argument.
    * Der Wert, der vom Operator zurückgegebenen befindet sich in den Speicherort, durch die Auswertung von `x`.
    * Der gespeicherte Wert des `x` wird das Ergebnis des Vorgangs.
*   Wenn `x` wird als eine Eigenschaft oder der Indexer Zugriff klassifiziert:
    * Der Instanzausdruck (Wenn `x` ist nicht `static`) und die Argumentliste (Wenn `x` Indexzugriff ist) zugeordneten `x` werden ausgewertet, und die Ergebnisse werden in der nachfolgenden verwendet `get` und `set` Accessor-Aufrufe.
    * Die `get` Accessor `x` wird aufgerufen, und der zurückgegebene Wert gespeichert wird.
    * Der ausgewählte Operator wird aufgerufen, mit dem gespeicherten Wert von `x` als Argument.
    * Die `set` Accessor `x` wird aufgerufen, mit dem Wert, der zurückgegeben wird, durch den Operator als seine `value` Argument.
    * Der gespeicherte Wert des `x` wird das Ergebnis des Vorgangs.

Die `++` und `--` Operatoren unterstützt auch die Präfix-Notation ([Präfix-Inkrement und Dekrement-Operatoren](expressions.md#prefix-increment-and-decrement-operators)). In der Regel das Ergebnis des `x++` oder `x--` ist der Wert des `x` vor dem Vorgang, während das Ergebnis des `++x` oder `--x` ist der Wert des `x` nach Abschluss des Vorgangs. In beiden Fällen `x` selbst hat den gleichen Wert nach Abschluss des Vorgangs.

Ein `operator ++` oder `operator --` Implementierung kann mithilfe der Präfix oder Postfix-Notation aufgerufen werden. Es ist nicht möglich, separate Implementierungen für die beiden Notationen zu haben.

### <a name="the-new-operator"></a>Der new-Operator

Die `new` Operator wird verwendet, um neue Instanzen von Typen zu erstellen.

Es gibt drei Arten von `new` Ausdrücke:

*  Objektausdrücke-Erstellung werden verwendet, um neue Instanzen der Klasse und Werttypen zu erstellen.
*  Array-erstellen-Ausdrücke werden verwendet, zum Erstellen neuer Instanzen von Arraytypen dar.
*  Delegat erstellen Ausdrücke werden verwendet, um neue Instanzen von Delegaten erstellt.

Die `new` Operator impliziert die Erstellung einer Instanz eines Typs, aber es ist nicht notwendigerweise, dass dynamische Zuordnung von Arbeitsspeicher. Insbesondere bei Instanzen von Werttypen benötigt keinen zusätzlichen Speicher über die Variablen in der sie sich befinden, und keine dynamischen Zuordnungen auftreten, wenn `new` wird verwendet, um Instanzen von Werttypen zu erstellen.

#### <a name="object-creation-expressions"></a>Objektausdrücke-Erstellung

Ein *Object_creation_expression* wird verwendet, um eine neue Instanz der Erstellen einer *Class_type* oder *Value_type*.

```antlr
object_creation_expression
    : 'new' type '(' argument_list? ')' object_or_collection_initializer?
    | 'new' type object_or_collection_initializer
    ;

object_or_collection_initializer
    : object_initializer
    | collection_initializer
    ;
```

Die *Typ* von einer *Object_creation_expression* muss eine *Class_type*, *Value_type* oder ein *Type_parameter* . Die *Typ* nicht möglich, eine `abstract` *Class_type*.

Der optionale *Argument_list* ([Argumentlisten](expressions.md#argument-lists)) ist nur zulässig, wenn die *Typ* ist eine *Class_type* oder *Struct_ Typ*.

Ein Objekterstellungsausdruck kann die Liste des Konstruktorarguments weglassen und einschließende Klammern angegeben, dass es sich um ein Objektinitialisierer oder Auflistungsinitialisierer enthält. Die Liste des Konstruktorarguments auslassen und durch Einschließen von Klammern ist gleichbedeutend mit der Angabe einer leeren Argumentliste.

Verarbeitung von einer Objekterstellungsausdruck, der einen Objektinitialisierer oder Auflistungsinitialisierer enthält besteht aus den Instanzkonstruktor zuerst zu verarbeiten und wird bei der Verarbeitung der Member oder Element Initialisierungen, die durch den Objektinitialisierer (angegeben[ Objektinitialisierer](expressions.md#object-initializers)) oder einem Auflistungsinitialisierer ([Auflistungsinitialisierer](expressions.md#collection-initializers)).

Wenn eines der Argumente in der optionalen *Argument_list* weist den Typ der Kompilierzeit `dynamic` die *Object_creation_expression* dynamisch gebunden ist ([dynamische Bindung](expressions.md#dynamic-binding)) und die folgenden Regeln gelten zur Laufzeit mit dem Laufzeit-Typ, der diese Argumente von der *Argument_list* Kompilierzeittyp deren `dynamic`. Allerdings wird die objekterstellung überprüfen eine begrenzte Kompilieren der Zeit vorgenommen, wie in beschrieben [Überprüfungen zur Kompilierzeit der dynamischen überladungsauflösung](expressions.md#compile-time-checking-of-dynamic-overload-resolution).

Die laufzeitverarbeitung der Bindung einer *Object_creation_expression* des Formulars `new T(A)`, wobei `T` ist eine *Class_type* oder *Value_type* und `A` ist eine optionale *Argument_list*, umfasst die folgenden Schritte aus:

*   Wenn `T` ist eine *Value_type* und `A` ist nicht vorhanden:
    * Die *Object_creation_expression* ist ein Standard-Konstruktor-Funktionsaufruf. Das Ergebnis der *Object_creation_expression* ist ein Wert vom Typ `T`, d. h. der Standardwert für `T` gemäß [die System.ValueType-Typ](types.md#the-systemvaluetype-type).
*   Andernfalls gilt: Wenn `T` ist eine *Type_parameter* und `A` ist nicht vorhanden:
    * Wenn kein werttypeinschränkung oder Konstruktoreinschränkung ([Geben Sie die Einschränkungen für Typparameter](classes.md#type-parameter-constraints)) wurde angegeben `T`, tritt ein Fehler während der Bindung.
    * Das Ergebnis der *Object_creation_expression* ist ein Wert von der Run-Time-Typ, der der Type-Parameter gebunden wurde, d. h. das Ergebnis von Aufrufen des Standardkonstruktors des Typs. Der Laufzeit-Typ kann es sich um ein Verweistyp oder ein Werttyp sein.
*   Andernfalls gilt: Wenn `T` ist eine *Class_type* oder *Struct_type*:
    * Wenn `T` ist ein `abstract` *Class_type*, ein Fehler während der Kompilierung auftritt.
    * Die Instanzkonstruktor aufrufen wird bestimmt, mit den Regeln der überladungsauflösung des [Überladungsauflösung](expressions.md#overload-resolution). Der Satz von Kandidaten Instanzkonstruktoren besteht aus allen verfügbaren Instanzkonstruktoren in deklarierten `T` der gelten in Bezug auf `A` ([Anwendbarer Funktionsmember](expressions.md#applicable-function-member)). Wenn der Satz von Kandidaten Instanzkonstruktoren leer ist, oder ein einzelnen bewährte Instanzenkonstruktor nicht identifiziert werden kann, tritt auf, ein Fehler während der Bindung.
    * Das Ergebnis der *Object_creation_expression* ist ein Wert vom Typ `T`, d. h. der Wert erzeugt, durch Aufrufen des Instanzkonstruktors im vorherigen Schritt bestimmt.
*  Andernfalls die *Object_creation_expression* ist ungültig, und ein Fehler während der Bindung erfolgt.

Auch wenn die *Object_creation_expression* dynamisch gebunden, der während der Kompilierung-Typ ist immer noch `T`.

Die Verarbeitung zur Laufzeit eine *Object_creation_expression* des Formulars `new T(A)`, wobei `T` ist *Class_type* oder *Struct_type* und `A` ist eine optionale *Argument_list*, umfasst die folgenden Schritte aus:

*   Wenn `T` ist eine *Class_type*:
    * Eine neue Instanz der Klasse `T` zugeordnet ist. Wenn nicht genügend Arbeitsspeicher verfügbar, um die neue Instanz einer `System.OutOfMemoryException` wird ausgelöst, und keine weiteren Schritte ausgeführt werden.
    * Alle Felder der neuen Instanz werden auf ihre Standardwerte initialisiert ([Standardwerte](variables.md#default-values)).
    * Der Instanzkonstruktor wird aufgerufen, gemäß den Regeln von der Members-Funktionsaufruf ([Überprüfungen zur Kompilierzeit der dynamischen überladungsauflösung](expressions.md#compile-time-checking-of-dynamic-overload-resolution)). Ein Verweis auf das neu zugeordnete Instanz automatisch an die Instanzkonstruktor übergeben wird und die Instanz kann zugegriffen werden aus diesen Konstruktor als `this`.
*   Wenn `T` ist eine *Struct_type*:
    * Eine Instanz des Typs `T` wird erstellt, indem Sie eine temporäre lokale Variable zuordnen. Seit einem Instanzenkonstruktor der ein *Struct_type* ist erforderlich, um auf jeden Fall wird jedes Feld der-Instanz erstellt wird, wird keine Initialisierung der temporären Variable muss einen Wert zuzuweisen.
    * Der Instanzkonstruktor wird aufgerufen, gemäß den Regeln von der Members-Funktionsaufruf ([Überprüfungen zur Kompilierzeit der dynamischen überladungsauflösung](expressions.md#compile-time-checking-of-dynamic-overload-resolution)). Ein Verweis auf das neu zugeordnete Instanz automatisch an die Instanzkonstruktor übergeben wird und die Instanz kann zugegriffen werden aus diesen Konstruktor als `this`.

#### <a name="object-initializers"></a>Objektinitialisierer

Ein ***Objektinitialisierer*** gibt Werte für NULL oder mehr Felder, Eigenschaften oder indizierte Elemente eines Objekts.

```antlr
object_initializer
    : '{' member_initializer_list? '}'
    | '{' member_initializer_list ',' '}'
    ;

member_initializer_list
    : member_initializer (',' member_initializer)*
    ;

member_initializer
    : initializer_target '=' initializer_value
    ;

initializer_target
    : identifier
    | '[' argument_list ']'
    ;

initializer_value
    : expression
    | object_or_collection_initializer
    ;
```

Ein Objektinitialisierer besteht aus einer Sequenz der Standardmember-Initialisierer, eingeschlossene `{` und `}` Token und durch Kommas getrennt. Jede *Member_initializer* kennzeichnet ein Ziel für die Initialisierung. Ein *Bezeichner* müssen den Namen einer zugänglichen Feld oder -Eigenschaft des Objekts initialisiert wird, während ein *Argument_list* eingeschlossen in eckigen Klammern müssen die Argumente für einen Indexer zugegriffen werden kann angeben, auf die das Objekt initialisiert wird. Es ist ein Fehler für einen Objektinitialisierer, um mehr als ein Member-Initialisierer für das gleiche Feld oder Eigenschaft enthalten.

Jede *Initializer_target* ist, gefolgt von einem Gleichheitszeichen und entweder ein Ausdruck, einen Objektinitialisierer oder einem Auflistungsinitialisierer. Es ist nicht möglich, für die Ausdrücke in den Objektinitialisierer, um auf das neu erstellte Objekt zu verweisen, die es initialisiert wird.

Ein Member-Initialisierer, der einen Ausdruck angibt, nach die Gleichheitszeichen auf die gleiche Weise wie eine Zuordnung verarbeitet werden ([einfache Zuweisung](expressions.md#simple-assignment)) an das Ziel.

Ein Member-Initialisierer, der einen Objektinitialisierer angibt, nach die Gleichheitszeichen ist eine ***geschachtelte Objektinitialisierer***, d. h. eine Initialisierung eines eingebetteten Objekts. Anstatt das Feld oder die Eigenschaft einen neuen Wert zuzuweisen, werden die Zuweisungen aus dem geschachtelten Objektinitialisierer als Zuweisungen auf Member des Felds oder der Eigenschaft behandelt. Geschachtelte Objektinitialisierer können nicht angewendet werden, um Eigenschaften mit einem Werttyp oder schreibgeschützte Felder mit einem Werttyp.

Ein Member-Initialisierer, der angibt, einen Auflistungsinitialisierer nach dem Gleichheitszeichen ist eine Initialisierung einer eingebetteten Auflistung. Anstatt das Feld "Ziel", Eigenschaft oder der Indexer eine neue Sammlung zuzuweisen, werden die Elemente im Initialisierer erhält der Auflistung, die auf das Ziel hinzugefügt. Das Ziel eines Auflistungstyps, das die in angegebenen Anforderungen erfüllt sein muss [Auflistungsinitialisierer](expressions.md#collection-initializers).

Die Argumente für einen Index Initialisierer werden immer genau einmal ausgewertet. Also auch wenn die Argumente am Ende niemals verwendet (z. B. aufgrund eines leeren geschachtelten Initialisierers), werden sie für ihre Nebeneffekte ausgewertet.

Die folgende Klasse stellt zwei Koordinaten dar:
```csharp
public class Point
{
    int x, y;

    public int X { get { return x; } set { x = value; } }
    public int Y { get { return y; } set { y = value; } }
}
```

Eine Instanz von `Point` erstellt und wie folgt initialisiert werden:
```csharp
Point a = new Point { X = 0, Y = 1 };
```
Das hat dieselbe Wirkung wie das
```csharp
Point __a = new Point();
__a.X = 0;
__a.Y = 1; 
Point a = __a;
```
wo `__a` ist eine temporäre Variable andernfalls nicht sichtbare und kann nicht zugegriffen werden. Die folgende Klasse stellt ein Rechteck, das aus zwei Punkten erstellt:
```csharp
public class Rectangle
{
    Point p1, p2;

    public Point P1 { get { return p1; } set { p1 = value; } }
    public Point P2 { get { return p2; } set { p2 = value; } }
}
```

Eine Instanz von `Rectangle` erstellt und wie folgt initialisiert werden:
```csharp
Rectangle r = new Rectangle {
    P1 = new Point { X = 0, Y = 1 },
    P2 = new Point { X = 2, Y = 3 }
};
```
Das hat dieselbe Wirkung wie das
```csharp
Rectangle __r = new Rectangle();
Point __p1 = new Point();
__p1.X = 0;
__p1.Y = 1;
__r.P1 = __p1;
Point __p2 = new Point();
__p2.X = 2;
__p2.Y = 3;
__r.P2 = __p2; 
Rectangle r = __r;
```
wo `__r`, `__p1` und `__p2` werden temporäre Variablen, die andernfalls nicht sichtbar und kann nicht zugegriffen werden.

Wenn `Rectangle`Konstruktor weist die beiden eingebettet `Point` Instanzen
```csharp
public class Rectangle
{
    Point p1 = new Point();
    Point p2 = new Point();

    public Point P1 { get { return p1; } }
    public Point P2 { get { return p2; } }
}
```
Das folgende Konstrukt verwendet werden kann, um das eingebettete initialisieren `Point` Instanzen, anstatt neue Instanzen:
```csharp
Rectangle r = new Rectangle {
    P1 = { X = 0, Y = 1 },
    P2 = { X = 2, Y = 3 }
};
```
Das hat dieselbe Wirkung wie das
```csharp
Rectangle __r = new Rectangle();
__r.P1.X = 0;
__r.P1.Y = 1;
__r.P2.X = 2;
__r.P2.Y = 3;
Rectangle r = __r;
```

Betrachten Sie eine Definition von C, im folgenden Beispiel an:
```csharp
var c = new C {
    x = true,
    y = { a = "Hello" },
    z = { 1, 2, 3 },
    ["x"] = 5,
    [0,0] = { "a", "b" },
    [1,2] = {}
};
```
entspricht diese Reihe von Zuweisungen:
```csharp
C __c = new C();
__c.x = true;
__c.y.a = "Hello";
__c.z.Add(1); 
__c.z.Add(2);
__c.z.Add(3);
string __i1 = "x";
__c[__i1] = 5;
int __i2 = 0, __i3 = 0;
__c[__i2,__i3].Add("a");
__c[__i2,__i3].Add("b");
int __i4 = 1, __i5 = 2;
var c = __c;
```
wo `__c`usw. werden generierte Variablen, die nicht sichtbar und für den Quellcode nicht zugänglich sind. Beachten Sie, dass die Argumente für `[0,0]` sind nur ein Mal ausgewertet, und die Argumente für `[1,2]` einmal ausgewertet werden, auch wenn sie nicht verwendet werden.

#### <a name="collection-initializers"></a>Auflistungsinitialisierer

Ein Auflistungsinitialisierer gibt an, die Elemente einer Auflistung.

```antlr
collection_initializer
    : '{' element_initializer_list '}'
    | '{' element_initializer_list ',' '}'
    ;

element_initializer_list
    : element_initializer (',' element_initializer)*
    ;

element_initializer
    : non_assignment_expression
    | '{' expression_list '}'
    ;

expression_list
    : expression (',' expression)*
    ;
```

Ein Auflistungsinitialisierer besteht aus einer Folge von elementinitialisierern, eingeschlossene `{` und `}` Token und durch Kommas getrennt. Jeder Elementinitialisierer gibt ein Element, das initialisierte Objekt hinzugefügt werden, und besteht aus einer Liste von Ausdrücken, die vom eingeschlossen `{` und `}` Token und durch Kommas getrennt.  Einen einzelnen Ausdrücken Elementinitialisierer ohne geschweifte Klammern geschrieben werden kann, jedoch nicht klicken Sie dann ein Zuweisungsausdruck, um Mehrdeutigkeiten zu vermeiden, mit der Standardmember-Initialisierer. Die *Non_assignment_expression* Produktion wird definiert, [Ausdruck](expressions.md#expression).

Im folgenden finden ein Beispiel für eine Objekterstellungsausdruck, die einen auflistungs-bzw. Arrayinitialisierers enthält:
```csharp
List<int> digits = new List<int> { 0, 1, 2, 3, 4, 5, 6, 7, 8, 9 };
```

Das Auflistungsobjekt, das für die ein Auflistungsinitialisierer gilt muss von einem Typ sein, die implementiert `System.Collections.IEnumerable` oder ein Fehler während der Kompilierung auftritt. Für jedes Element in der Reihenfolge angegeben, wird der Auflistungsinitialisierer Ruft eine `Add` Methode auf der Ziel-Objekt mit der Liste mit Ausdrücken des elementinitialisierers als Argumentliste normalen Membersuche anwenden und für jeden Aufruf der überladungsauflösung. Daher müssen das Objekt eine anwendbare Instanz- oder Erweiterungsmethode-Methode mit dem Namen `Add` für jedes Elementinitialisierer.

Die folgende Klasse stellt einen Kontakt mit einem Namen und eine Liste der Telefonnummern dar:
```csharp
public class Contact
{
    string name;
    List<string> phoneNumbers = new List<string>();

    public string Name { get { return name; } set { name = value; } }

    public List<string> PhoneNumbers { get { return phoneNumbers; } }
}
```

Ein `List<Contact>` erstellt und wie folgt initialisiert werden:
```csharp
var contacts = new List<Contact> {
    new Contact {
        Name = "Chris Smith",
        PhoneNumbers = { "206-555-0101", "425-882-8080" }
    },
    new Contact {
        Name = "Bob Harris",
        PhoneNumbers = { "650-555-0199" }
    }
};
```
Das hat dieselbe Wirkung wie das
```csharp
var __clist = new List<Contact>();
Contact __c1 = new Contact();
__c1.Name = "Chris Smith";
__c1.PhoneNumbers.Add("206-555-0101");
__c1.PhoneNumbers.Add("425-882-8080");
__clist.Add(__c1);
Contact __c2 = new Contact();
__c2.Name = "Bob Harris";
__c2.PhoneNumbers.Add("650-555-0199");
__clist.Add(__c2);
var contacts = __clist;
```
wo `__clist`, `__c1` und `__c2` werden temporäre Variablen, die andernfalls nicht sichtbar und kann nicht zugegriffen werden.

#### <a name="array-creation-expressions"></a>Arrayausdrücke-Erstellung

Ein *Array_creation_expression* wird verwendet, um eine neue Instanz der Erstellen einer *Array_type*.

```antlr
array_creation_expression
    : 'new' non_array_type '[' expression_list ']' rank_specifier* array_initializer?
    | 'new' array_type array_initializer
    | 'new' rank_specifier array_initializer
    ;
```

Ein Arrayausdruck für die Erstellung der ersten Form weist eine Arrayinstanz des Typs, das Löschen der einzelnen Ausdrücke aus der Liste mit Ausdrücken ergibt. Z. B. die Arrayerstellungsausdruck `new int[10,20]` erzeugt eine Arrayinstanz des Typs `int[,]`, und der Ausdruck zur Arrayerstellung `new int[10][,]` erzeugt ein Array vom Typ `int[][,]`. Jeder Ausdruck in der Liste mit Ausdrücken muss vom Typ `int`, `uint`, `long`, oder `ulong`, oder implizit in eine oder mehrere dieser Typen. Der Wert jedes Ausdrucks bestimmt die Länge der entsprechenden Dimension in der neu zugewiesenen Arrayinstanz fest. Da die Länge einer Dimension des Arrays nicht negativ sein muss, ist es ein Fehler während der Kompilierung, damit eine *Constant_expression* mit einem negativen Wert in der Liste mit Ausdrücken.

Mit Ausnahme der in einem unsicheren Kontext ([nicht sicheren Kontexten](unsafe-code.md#unsafe-contexts)), das Layout des Arrays ist nicht angegeben.

Wenn einem Arrayerstellungsausdruck der ersten Form einen Arrayinitialisierer enthält, wird jeder Ausdruck in der Liste mit Ausdrücken muss eine Konstante sein, und der Rang jede Dimension angegebenen Länge durch die Liste der Ausdrücke angegeben, müssen mit denen des Arrayinitialisierers übereinstimmen.

In einem Arrayerstellungsausdruck der zweite oder dritte Form muss es sich bei der Rang des dem angegebenen Array-Typ oder Rang Bezeichner mit des Arrayinitialisierers übereinstimmen. Die Längen der einzelnen Dimension werden von der Anzahl der Elemente in jedem von der entsprechenden Schachtelungsebenen des Arrayinitialisierers hergeleitet. Daher wird, ist der Ausdruck
```csharp
new int[,] {{0, 1}, {2, 3}, {4, 5}}
```
genau entspricht.
```csharp
new int[3, 2] {{0, 1}, {2, 3}, {4, 5}}
```

Die dritte Form einem Arrayerstellungsausdruck wird als bezeichnet ein ***implizit typisierten Ausdruck zur Arrayerstellung***. Es ähnelt der zweiten Form, mit dem Unterschied, dass der Elementtyp des Arrays nicht explizit jedoch als der am besten verwendete Typ bestimmt angegeben wird, ([Suchen nach den besten common-Typ, der einen Satz von Ausdrücken](expressions.md#finding-the-best-common-type-of-a-set-of-expressions)) des Satzes von Ausdrücken im Array Initialisierer. Für ein mehrdimensionales Array d. h. eine Where der *Rank_specifier* enthält mindestens ein Komma, dieser Satz besteht aus allen *Ausdruck*s finden Sie in geschachtelten *Array_initializer*s.

Arrayinitialisierer sind unter [Array Initializers](arrays.md#array-initializers).

Das Ergebnis der Auswertung von einem Arrayerstellungsausdruck wird als ein Wert, d. h. einen Verweis auf die neu zugewiesene Arrayinstanz klassifiziert. Die Verarbeitung zur Laufzeit von einem Arrayerstellungsausdruck umfasst die folgenden Schritte aus:

*  Der Dimension Länge von Ausdrücken für die *Expression_list* werden in der Reihenfolge von links nach rechts ausgewertet. Befolgen Sie die Auswertung der einzelnen Ausdrücke, die eine implizite Konvertierung ([implizite Konvertierungen](conversions.md#implicit-conversions)) auf einen der folgenden Typen erfolgt: `int`, `uint`, `long`, `ulong`. Der erste Typ in dieser Liste, die für die eine implizite Konvertierung vorhanden ist, wird ausgewählt. Wenn die Auswertung eines Ausdrucks oder einer nachfolgenden impliziten Konvertierung eine Ausnahme auslöst, klicken Sie dann keine weitere Ausdrücke werden ausgewertet, und keine weiteren Schritte ausgeführt werden.
*  Die Werte für die Längen der Dimension werden wie folgt überprüft werden. Wenn eine oder mehrere der Werte sind kleiner als 0 (null), eine `System.OverflowException` wird ausgelöst, und keine weiteren Schritte ausgeführt werden.
*  Es wird eine Arrayinstanz mit Längen der angegebenen Dimension zugeordnet. Wenn nicht genügend Arbeitsspeicher verfügbar, um die neue Instanz einer `System.OutOfMemoryException` wird ausgelöst, und keine weiteren Schritte ausgeführt werden.
*  Alle Elemente der neuen Arrayinstanz werden auf ihre Standardwerte initialisiert ([Standardwerte](variables.md#default-values)).
*  Wenn der Ausdruck zur Arrayerstellung einen Arrayinitialisierer enthält, wird jeder Ausdruck im Arrayinitialisierer ausgewertet und das entsprechende Arrayelement zugewiesen. Die auswertungen und Zuweisungen erfolgen in der Reihenfolge der Ausdrücke im Arrayinitialisierer geschrieben werden, in anderen Worten: Elemente werden in aufsteigender Indexreihenfolge, mit die Dimension ganz rechts erhöhen zuerst initialisiert. Wenn die Auswertung eines angegebenen Ausdrucks oder der nachfolgenden Zuweisung zu dem entsprechenden Array-Element eine Ausnahme verursacht hat, klicken Sie dann keine weiteren Elemente initialisiert werden (und die übrigen Elemente werden daher Standardwerte haben).

Ein Ausdruck zur Arrayerstellung ermöglicht die Instanziierung eines Arrays mit Elementen eines Arraytyps, aber die Elemente dieses Arrays müssen manuell initialisiert werden. Beispielsweise wird die Anweisung
```csharp
int[][] a = new int[100][];
```
erstellt ein eindimensionales Array mit 100 Elementen des Typs `int[]`. Der anfängliche Wert jedes Elements ist `null`. Es ist nicht möglich, für die gleiche Arrayerstellungsausdruck Teilarrays aus, und die Anweisung auch instanziieren
```csharp
int[][] a = new int[100][5];        // Error
```
führt zu einem Fehler während der Kompilierung. Die Instanziierung der Teilarrays muss stattdessen manuell, wie im ausgeführt werden
```csharp
int[][] a = new int[100][];
for (int i = 0; i < 100; i++) a[i] = new int[5];
```

Wenn ein Array von Arrays mit eine Form "rechteckige", die d. h. untergeordneten Arrays gleicher Länge sind aufweist, ist es effizienter, ein mehrdimensionales Array verwenden. Im obigen Beispiel erstellt die Instanziierung des Arrays von Arrays 101 Objekte – eine äußere Array und 100 Teilarrays. Im Gegensatz dazu sind
```csharp
int[,] = new int[100, 5];
```
erstellt nur ein einzelnes Objekt, ein zweidimensionales Array, und die Zuweisung in einer einzelnen Anweisung erreicht.

Es folgen Beispiele für Ausdrücke für implizit typisierte Arrays erstellen:
```csharp
var a = new[] { 1, 10, 100, 1000 };                       // int[]

var b = new[] { 1, 1.5, 2, 2.5 };                         // double[]

var c = new[,] { { "hello", null }, { "world", "!" } };   // string[,]

var d = new[] { 1, "one", 2, "two" };                     // Error
```

Der letzte Ausdruck verursacht einen Kompilierungsfehler, da weder `int` noch `string` implizit in den anderen, und es kommt nicht am besten geben. Ein explizit typisierte Ausdruck zur Arrayerstellung muss verwendet werden in diesem Fall z. B. Angabe des Typs werden `object[]`. Eines der Elemente kann auch in einen gemeinsamen Basistyp umgewandelt werden die klicken Sie dann den abgeleitete Elementtyp aufweisen würden.

Implizit typisiertes Array erstellen Ausdrücke mit anonyme Objektinitialisierer kombiniert werden können ([anonyme Erstellung Objektausdrücke](expressions.md#anonymous-object-creation-expressions)) zum Erstellen anonym typisierte Datenstrukturen. Zum Beispiel:
```csharp
var contacts = new[] {
    new {
        Name = "Chris Smith",
        PhoneNumbers = new[] { "206-555-0101", "425-882-8080" }
    },
    new {
        Name = "Bob Harris",
        PhoneNumbers = new[] { "650-555-0199" }
    }
};
```

#### <a name="delegate-creation-expressions"></a>Delegieren Sie die Erstellung von Ausdrücken

Ein *Delegate_creation_expression* wird verwendet, um eine neue Instanz der Erstellen einer *Delegate_type*.

```antlr
delegate_creation_expression
    : 'new' delegate_type '(' expression ')'
    ;
```

Das Argument ein Delegatausdruck für die Erstellung muss eine Methodengruppe, eine anonyme Funktion oder einen Wert, der entweder den Kompilierzeittyp `dynamic` oder *Delegate_type*. Wenn das Argument eine Methodengruppe ist, kennzeichnet es die Methode und eine Instanzmethode, die das Objekt für die Erstellung ein Delegaten. Wenn das Argument eine anonyme Funktion, die sie direkt die Parameter und den Methodentext, der das Delegatziel definiert ist. Wenn das Argument ein Wert identifiziert eine Delegatinstanz, von dem eine Kopie erstellt ist.

Wenn die *Ausdruck* weist den Typ der Kompilierzeit `dynamic`, *Delegate_creation_expression* dynamisch gebunden ist ([dynamische Bindung](expressions.md#dynamic-binding)), und die folgenden Regeln gelten zur Laufzeit mit dem Laufzeit-Typ, der die *Ausdruck*. Andernfalls gelten die Regeln zum Zeitpunkt der Kompilierung.

Die laufzeitverarbeitung der Bindung einer *Delegate_creation_expression* des Formulars `new D(E)`, wobei `D` ist eine *Delegate_type* und `E` ist ein *Ausdruck* , umfasst die folgenden Schritte aus:

*  Wenn `E` eine Methodengruppe wird der Delegaterstellungsausdruck in die gleiche Weise wie eine Methode gruppenkonvertierung verarbeitet wird ([Gruppe Konvertierungen](conversions.md#method-group-conversions)) von `E` zu `D`.
*  Wenn `E` ist eine anonyme Funktion, der Delegaterstellungsausdruck wird verarbeitet, in die gleiche Weise wie eine anonyme Funktion-Konvertierung ([anonyme Funktion Konvertierungen](conversions.md#anonymous-function-conversions)) von `E` zu `D`.
*  Wenn `E` ist ein Wert, `E` kompatibel sein muss ([delegieren Deklarationen](delegates.md#delegate-declarations)) mit `D`, und das Ergebnis ist ein Verweis auf einen neu erstellten Delegaten vom Typ `D` , verweist auf den gleichen Aufruf als Liste `E`. Wenn `E` ist nicht kompatibel mit `D`, ein Fehler während der Kompilierung auftritt.

Die Verarbeitung zur Laufzeit eine *Delegate_creation_expression* des Formulars `new D(E)`, wobei `D` ist eine *Delegate_type* und `E` ist ein *Ausdruck* , umfasst die folgenden Schritte aus:

*   Wenn `E` eine Methodengruppe wird der Delegaterstellungsausdruck wird als eine Methode gruppenkonvertierung ausgewertet ([Gruppe Konvertierungen](conversions.md#method-group-conversions)) von `E` zu `D`.
*   Wenn `E` ist eine anonyme Funktion, die delegaterstellung wird ausgewertet, wie eine anonyme Funktion Konvertierung von `E` zu `D` ([anonyme Funktion Konvertierungen](conversions.md#anonymous-function-conversions)).
*   Wenn `E` ist ein Wert, der eine *Delegate_type*:
    * `E` wird ausgewertet. Wenn diese Evaluierungsversion auf eine Ausnahme auslöst, werden keine weiteren Schritte ausgeführt.
    * Wenn der Wert des `E` ist `null`, `System.NullReferenceException` wird ausgelöst, und keine weiteren Schritte ausgeführt werden.
    * Eine neue Instanz des Delegattyps `D` zugeordnet ist. Wenn nicht genügend Arbeitsspeicher verfügbar, um die neue Instanz einer `System.OutOfMemoryException` wird ausgelöst, und keine weiteren Schritte ausgeführt werden.
    * Die neue Delegatinstanz wird mit der gleichen Aufrufliste als die Delegatinstanz, die vom initialisiert `E`.

Die Aufrufliste eines Delegaten wird bestimmt, wenn der Delegat instanziiert wird, und klicken Sie dann für die gesamte Lebensdauer des Delegaten unverändert bleibt. Das heißt, ist es nicht möglich, die Ziel um aufrufbare Entitäten eines Delegaten zu ändern, nachdem es erstellt wurde. Wenn zwei Delegaten kombiniert werden, oder eine von einem anderen entfernt wird ([delegieren Deklarationen](delegates.md#delegate-declarations)), dazu, dass ein neuer Delegat; keine bestehende Delegat hat seinen Inhalt geändert.

Es ist nicht möglich, einen Delegaten zu erstellen, der auf eine Eigenschaft, Indexer, benutzerdefinierten Operator, Instanzenkonstruktor, Destruktor oder statischen Konstruktor verweist.

Wie oben beschrieben, wenn ein Delegat erstellt wird aus einer Methodengruppe, die Liste der formalen Parameter und Rückgabetyp des Delegaten bestimmen, welche der überladenen Methoden auswählen. Im Beispiel
```csharp
delegate double DoubleFunc(double x);

class A
{
    DoubleFunc f = new DoubleFunc(Square);

    static float Square(float x) {
        return x * x;
    }

    static double Square(double x) {
        return x * x;
    }
}
```
die `A.f` Feld wird initialisiert, indem ein Delegat, der auf den zweiten verweist `Square` Methode, da diese Methode genau mit der Liste formaler Parameter und Rückgabetyp übereinstimmt `DoubleFunc`. Haben Sie die zweite `Square` -Methode nicht vorhanden war, ein Fehler während der Kompilierung würde aufgetreten.

#### <a name="anonymous-object-creation-expressions"></a>Anonyme Erstellung Objektausdrücke

Ein *Anonymous_object_creation_expression* wird verwendet, um ein Objekt eines anonymen Typs zu erstellen.

```antlr
anonymous_object_creation_expression
    : 'new' anonymous_object_initializer
    ;

anonymous_object_initializer
    : '{' member_declarator_list? '}'
    | '{' member_declarator_list ',' '}'
    ;

member_declarator_list
    : member_declarator (',' member_declarator)*
    ;

member_declarator
    : simple_name
    | member_access
    | base_access
    | null_conditional_member_access
    | identifier '=' expression
    ;
```

Ein anonymer Objektinitialisierer deklariert einen anonymen Typ und gibt eine Instanz dieses Typs zurück. Ein anonymer Typ ist ein namenloses Klassentyp, der direkt erbt `object`. Die Member eines anonymen Typs sind, eine Sequenz von schreibgeschützten Eigenschaften, die von der anonymer Objektinitialisierer, die zum Erstellen einer Instanz des Typs abgeleitet wird. Insbesondere ein anonymer Objektinitialisierer des Formulars
```csharp
new { p1 = e1, p2 = e2, ..., pn = en }
```
deklariert einen anonymen Typ des Formulars
```csharp
class __Anonymous1
{
    private readonly T1 f1;
    private readonly T2 f2;
    ...
    private readonly Tn fn;

    public __Anonymous1(T1 a1, T2 a2, ..., Tn an) {
        f1 = a1;
        f2 = a2;
        ...
        fn = an;
    }

    public T1 p1 { get { return f1; } }
    public T2 p2 { get { return f2; } }
    ...
    public Tn pn { get { return fn; } }

    public override bool Equals(object __o) { ... }
    public override int GetHashCode() { ... }
}
```
in dem jede `Tx` ist der Typ des der entsprechende Ausdruck `ex`. Der Ausdruck verwendet eine *Member_declarator* muss einen Typ haben. Daher wird ein Fehler während der Kompilierung für einen Ausdruck in einem *Member_declarator* Null oder eine anonyme Funktion sein. Es ist auch ein Fehler während der Kompilierung für den Ausdruck zu einen unsicheren Typ haben.

Die Namen eines anonymen Typs und des Parameters, der die `Equals` Methode werden automatisch vom Compiler generiert und kann nicht in den Programmtext verwiesen werden.

Innerhalb desselben Programms erzeugt zwei anonyme Objektinitialisierer, die eine Sequenz von Eigenschaften den gleichen Namen und die während der Kompilierung-Typen in derselben Reihenfolge angeben, dass Instanzen desselben anonymen Typs.

Im Beispiel
```csharp
var p1 = new { Name = "Lawnmower", Price = 495.00 };
var p2 = new { Name = "Shovel", Price = 26.95 };
p1 = p2;
```
die Zuweisung in der letzten Zeile ist zulässig, weil `p1` und `p2` desselben anonymen Typs sind.

Die `Equals` und `GetHashcode` Methoden von anonymen Typen Überschreiben von geerbten Methoden `object`, und werden in Form von definiert die `Equals` und `GetHashcode` -Eigenschaften, so, dass zwei Instanzen desselben anonymen Typs gleich sind. wenn und nur dann, wenn alle Eigenschaften gleich sind.

Ein Member-Declarator kann abgekürzt werden, um einen einfachen Namen ([Typrückschluss](expressions.md#type-inference)), Memberzugriff ([Überprüfungen zur Kompilierzeit der dynamischen überladungsauflösung](expressions.md#compile-time-checking-of-dynamic-overload-resolution)), Basiszugriff ([Zugriffbasieren](expressions.md#base-access)) oder ein Null-bedingte Memberzugriff ([bedingten Null-Ausdrücke als Projektionsinitialisierer](expressions.md#null-conditional-expressions-as-projection-initializers)). Dies wird als bezeichnet ein ***Projektion Initialisierer*** und ist die Kurzform für eine Deklaration und Zuweisung zu einer Eigenschaft mit dem gleichen Namen. Insbesondere memberdeklaratoren der Formen
```csharp
identifier
expr.identifier
```
sind bzw. genaue Entsprechung der folgenden:
```csharp
identifier = identifier
identifier = expr.identifier
```

Daher in einer Projektion Initialisierung der *Bezeichner* wählt den Wert und an das Feld oder die Eigenschaft, der der Wert zugewiesen wird. Intuitiv projiziert ein Initialisierer für die Projektion, nicht nur ein Wert, sondern auch den Namen des Werts.

### <a name="the-typeof-operator"></a>Der Typeof-operator

Die `typeof` Operator wird zum Abrufen der `System.Type` Objekt für einen Typ.

```antlr
typeof_expression
    : 'typeof' '(' type ')'
    | 'typeof' '(' unbound_type_name ')'
    | 'typeof' '(' 'void' ')'
    ;

unbound_type_name
    : identifier generic_dimension_specifier?
    | identifier '::' identifier generic_dimension_specifier?
    | unbound_type_name '.' identifier generic_dimension_specifier?
    ;

generic_dimension_specifier
    : '<' comma* '>'
    ;

comma
    : ','
    ;
```

Die erste Form der *Typeof_expression* besteht aus einem `typeof` Schlüsselwort, gefolgt von einer in Klammern gesetzte *Typ*. Das Ergebnis eines Ausdrucks, der diese Form ist die `System.Type` -Objekt für den angegebenen Typ. Es gibt nur ein `System.Type` Objekt für jeden angegebenen Typ. Dies bedeutet, dass für einen Typ `T`, `typeof(T) == typeof(T)` ist immer "true". Die *Typ* nicht `dynamic`.

Die zweite Form der *Typeof_expression* besteht aus einem `typeof` Schlüsselwort, gefolgt von einer in Klammern gesetzte *Unbound_type_name*. Ein *Unbound_type_name* ähnelt sehr einer *Type_name* ([Namespace und Typnamen](basic-concepts.md#namespace-and-type-names)) mit dem Unterschied, dass ein *Unbound_type_name* enthält *Generic_dimension_specifier*s, in denen eine *Type_name* enthält *Type_argument_list*s. Wenn der Operand des eine *Typeof_expression* ist eine Folge von Token, das die Grammatiken beider erfüllt *Unbound_type_name* und *Type_name*, es nämlich Wenn enthält weder ein *Generic_dimension_specifier* noch ein *Type_argument_list*, die Abfolge der Token gilt ein *Type_name*. Die Bedeutung des einen *Unbound_type_name* wird wie folgt bestimmt:

*  Konvertieren Sie die Sequenz von Token, die eine *Type_name* durch Ersetzen jedes *Generic_dimension_specifier* mit einem *Type_argument_list* müssen die gleiche Anzahl von Kommas und die Schlüsselwort `object` während jedes *Type_argument*.
*  Bewerten Sie die resultierende *Type_name*, und ignoriert alle Einschränkungen für Typparameter.
*  Die *Unbound_type_name* aufgelöst, die für den ungebundenen generischen Typ, der den resultierenden konstruierten Typ zugeordnet wird ([gebunden und Typen von nicht gebundenen](types.md#bound-and-unbound-types)).

Das Ergebnis der *Typeof_expression* ist die `System.Type` -Objekt für generische Typ der resultierenden aufgehoben werden.

Die dritte Form der *Typeof_expression* besteht aus einem `typeof` Schlüsselwort, gefolgt von einer in Klammern gesetzte `void` Schlüsselwort. Das Ergebnis eines Ausdrucks, der diese Form ist die `System.Type` -Objekt, das Fehlen eines Typs darstellt. Das Typobjekt, das von zurückgegebene `typeof(void)` unterscheidet sich von das Typobjekt für den beliebigen Typs zurückgegeben. Dieses besondere Art-Objekt ist nützlich in Klassenbibliotheken, mit denen Reflektion für Methoden in der Sprache, in dem diese Methoden haben eine Methode zur Darstellung des Rückgabetyp einer Methode, einschließlich der void-Methoden, mit einer Instanz von möchten `System.Type`.

Die `typeof` -Operator kann für einen Typparameter verwendet werden. Das Ergebnis ist die `System.Type` -Objekt für den Laufzeittyp, der dem Typparameter gebunden war. Die `typeof` Operator kann auch auf einen konstruierten Typ oder einen ungebundenen generischen Typ verwendet werden ([gebunden und Typen von nicht gebundenen](types.md#bound-and-unbound-types)). Die `System.Type` Objekt für ein ungebundener generischer Typ ist nicht identisch mit der `System.Type` Objekt des Instanztyps. Der Instanztyp ist also immer ein geschlossener konstruierter Typ zur Laufzeit die `System.Type` Objekt abhängig ist für die Laufzeit-Typargumente verwendet werden, während die ungebundenen generischen Typs keine Typargumente aufweist.

Im Beispiel
```csharp
using System;

class X<T>
{
    public static void PrintTypes() {
        Type[] t = {
            typeof(int),
            typeof(System.Int32),
            typeof(string),
            typeof(double[]),
            typeof(void),
            typeof(T),
            typeof(X<T>),
            typeof(X<X<T>>),
            typeof(X<>)
        };
        for (int i = 0; i < t.Length; i++) {
            Console.WriteLine(t[i]);
        }
    }
}

class Test
{
    static void Main() {
        X<int>.PrintTypes();
    }
}
```
erzeugt die folgende Ausgabe:
```
System.Int32
System.Int32
System.String
System.Double[]
System.Void
System.Int32
X`1[System.Int32]
X`1[X`1[System.Int32]]
X`1[T]
```

Beachten Sie, dass `int` und `System.Int32` denselben Typ aufweisen.

Beachten Sie, dass das Ergebnis des `typeof(X<>)` hängt nicht das Typargument, aber das Ergebnis des `typeof(X<T>)` ist.

### <a name="the-checked-and-unchecked-operators"></a>Die Operatoren checked und unchecked

Die `checked` und `unchecked` Operatoren werden verwendet, um zu steuern die ***Kontext der überlaufprüfung*** für arithmetische Operationen für ganzzahlige Typen und Konvertierungen.

```antlr
checked_expression
    : 'checked' '(' expression ')'
    ;

unchecked_expression
    : 'unchecked' '(' expression ')'
    ;
```

Die `checked` -Operator wertet den enthaltenen Ausdruck in einem überprüften Kontext und die `unchecked` -Operator wertet die enthaltenen Ausdruck in einem ungeprüften Kontext. Ein *Checked_expression* oder *Unchecked_expression* entspricht genau einer *Parenthesized_expression* ([Ausdrücke in Klammern](expressions.md#parenthesized-expressions)), außer dass der enthaltene Ausdruck in der angegebenen Kontext der überlaufprüfung ausgewertet wird.

Kontext der überlaufprüfung kann auch über gesteuert werden die `checked` und `unchecked` Anweisungen ([die checked und unchecked Anweisungen](statements.md#the-checked-and-unchecked-statements)).

Die folgenden Vorgänge werden von hergestellt, indem Kontext der überlaufprüfung beeinflusst die `checked` und `unchecked` Operatoren und Anweisungen:

*  Die vordefinierten `++` und `--` unäre Operatoren ([Postfix-Inkrement und Dekrement-Operatoren](expressions.md#postfix-increment-and-decrement-operators) und [Präfix-Inkrement und Dekrement-Operatoren](expressions.md#prefix-increment-and-decrement-operators)), wenn der Operand ein ganzzahliger ist Geben Sie ein.
*  Die vordefinierten `-` unäroperator ([unäre Operator minus](expressions.md#unary-minus-operator)), wenn der Operand ein ganzzahliger Typ ist.
*  Die vordefinierten `+`, `-`, `*`, und `/` binäre Operatoren ([arithmetische Operatoren](expressions.md#arithmetic-operators)), wenn beide Operanden Ganzzahltypen sind.
*  Explizite numerische Konvertierungen ([explizite numerische Konvertierungen](conversions.md#explicit-numeric-conversions)) von einem ganzzahligen Typ in einen anderen ganzzahligen Typ oder von `float` oder `double` in einen ganzzahligen Typ.

Wenn eine der oben genannten Vorgänge erzeugen kann ein Ergebnis, das ist zu groß, um die Darstellung in den Zieltyp, der den Kontext, in dem der Vorgang ausgeführt Steuerelemente ist, des resultierenden Verhaltens:

*  In einem `checked` Kontext, wenn der Vorgang ein konstanter Ausdruck ist ([Konstante Ausdrücke](expressions.md#constant-expressions)), ein Fehler während der Kompilierung auftritt. Wenn der Vorgang zur Laufzeit, ausgeführt wird, andernfalls eine `System.OverflowException` ausgelöst.
*  In einer `unchecked` Kontext ist das Ergebnis abgeschnitten, indem alle höherwertigen Bits, die nicht in den Zieltyp passen verworfen.

Für nicht Konstante Ausdrücke (Expressions, die zur Laufzeit ausgewertet werden), die von einer nicht eingeschlossen sind `checked` oder `unchecked` Operatoren oder -Anweisungen, wird der Kontext der standardmäßigen überlaufprüfung `unchecked` , wenn die externen (z. B. Compiler Faktoren Switches und Konfiguration der ausführungsumgebung) rufen Sie für `checked` Auswertung.

Für Konstante Ausdrücke (Expressions, die zum Zeitpunkt der Kompilierung vollständig ausgewertet werden können), ist der Standard-überlaufprüfung-Kontext immer `checked`. Es sei denn, Sie befindet sich ein konstanter Ausdruck explizit in ein `unchecked` Kontext Überläufe, die auftreten, während der Kompilierung Auswertung des Ausdrucks immer Fehler während der Kompilierung.

Der Text einer anonymen Funktion ist nicht betroffen von `checked` oder `unchecked` Kontexte, in dem die anonyme Funktion auftritt.

Im Beispiel
```csharp
class Test
{
    static readonly int x = 1000000;
    static readonly int y = 1000000;

    static int F() {
        return checked(x * y);      // Throws OverflowException
    }

    static int G() {
        return unchecked(x * y);    // Returns -727379968
    }

    static int H() {
        return x * y;               // Depends on default
    }
}
```
keine Kompilierungsfehler werden gemeldet, da keiner der Ausdrücke zur Kompilierzeit ausgewertet werden kann. Zur Laufzeit die `F` -Methode löst eine `System.OverflowException`, und die `G` Methodenrückgabe-727379968 (die unteren 32 Bits des Ergebnisses wird außerhalb des gültigen Bereichs). Das Verhalten der `H` Methode hängt von der Standard-überlaufprüfung-Kontext für die Kompilierung, aber es ist entweder dasselbe `F` oder gleich `G`.

Im Beispiel
```csharp
class Test
{
    const int x = 1000000;
    const int y = 1000000;

    static int F() {
        return checked(x * y);      // Compile error, overflow
    }

    static int G() {
        return unchecked(x * y);    // Returns -727379968
    }

    static int H() {
        return x * y;               // Compile error, overflow
    }
}
```
Beim Auswerten der Konstanten Ausdrücke in auftretenden `F` und `H` verursachen Kompilierzeitfehler gemeldet werden, da die Ausdrücke, in ausgewertet werden einem `checked` Kontext. Ein Überlauf tritt auch auf, wenn es sich bei der Auswertung des konstanten Ausdrucks in `G`, aber da die Auswertung in erfolgt einem `unchecked` Kontext, der Überlauf wird nicht gemeldet.

Die `checked` und `unchecked` Operatoren wirken sich nur auf der überlaufprüfung Kontext für diese Vorgänge, die textlich innerhalb der "`(`"und"`)`" Token. Die Operatoren haben keine Auswirkungen auf die Funktionsmember, die aufgerufen werden, als Ergebnis der Auswertung des Ausdrucks enthalten. Im Beispiel
```csharp
class Test
{
    static int Multiply(int x, int y) {
        return x * y;
    }

    static int F() {
        return checked(Multiply(1000000, 1000000));
    }
}
```
die Verwendung von `checked` in `F` wirkt sich nicht auf die Auswertung von `x * y` in `Multiply`, sodass `x * y` wird in der Standardeinstellung überlaufprüfung-Kontext ausgewertet.

Die `unchecked` Operator eignet sich, beim Schreiben von Konstanten der ganzzahligen Typen mit Vorzeichen in hexadezimaler Schreibweise. Zum Beispiel:
```csharp
class Test
{
    public const int AllBits = unchecked((int)0xFFFFFFFF);

    public const int HighBit = unchecked((int)0x80000000);
}
```

Sowohl der oben genannten hexadezimalen Konstanten sind vom Typ `uint`. Da sind die Konstanten außerhalb der `int` liegen, ohne die `unchecked` Operator, die Umwandlungen in `int` Fehler während der Kompilierung erzeugt.

Die `checked` und `unchecked` Operatoren und Anweisungen ermöglichen es Programmierern, um bestimmte Aspekte der einige numerischen Berechnungen zu steuern. Das Verhalten einiger numerischen Operatoren hängt jedoch von ihrer Operanden des Bereichsoperators-Datentypen. Z. B. immer Multiplikation von zwei Dezimalstellen löst eine Ausnahme bei einem Überlauf sogar innerhalb einer explizit `unchecked` zu erstellen. Auf ähnliche Weise Multiplizieren zweier gleitet nie Ergebnisse zu einer Ausnahme bei einem Überlauf sogar innerhalb einer explizit `checked` zu erstellen. Andere Operatoren sind außerdem nicht betroffen, durch den Modus der Überprüfung, ob der Standard- oder explizit.

### <a name="default-value-expressions"></a>Ausdrücke mit Standardwert

Ein Standardwertausdruck verwendet, um den Standardwert abzurufen ([Standardwerte](variables.md#default-values)) eines Typs. Ein Standardwertausdruck wird normalerweise für Typparameter verwendet werden, da nicht bekannt sein kann, wenn der Typparameter einen Werttyp oder ein Verweistyp ist. (Keine Konvertierung vorhanden ist, aus der `null` Zeichenfolgenliteral auf einen Typparameter, wenn der Typparameter bekannt ist, dass ein Verweistyp sein.)

```antlr
default_value_expression
    : 'default' '(' type ')'
    ;
```

Wenn die *Typ* in einem *Default_value_expression* wertet zur Laufzeit in einen Verweistyp, der das Ergebnis ist `null` auf diesen Typ konvertiert. Wenn die *Typ* in eine *Default_value_expression* wertet zur Laufzeit in einen Werttyp, der das Ergebnis ist der *Value_type*des default-Wert ([Standard Konstruktoren](types.md#default-constructors)).

Ein *Default_value_expression* ist ein konstanter Ausdruck ([Konstante Ausdrücke](expressions.md#constant-expressions)) Wenn der Typ ist ein Verweistyp oder ein Typparameter, der bekannt ist, dass ein Verweistyp sein ([Typparameter Einschränkungen](classes.md#type-parameter-constraints)). Darüber hinaus eine *Default_value_expression* ist ein konstanter Ausdruck, wenn der Typ eine der folgenden Werttypen: `sbyte`, `byte`, `short`, `ushort`, `int`, `uint`, `long`, `ulong`, `char`, `float`, `double`, `decimal`, `bool`, oder ein Enumerationstyp.


### <a name="nameof-expressions"></a>Nameof-Ausdrücke

Ein *Nameof_expression* verwendet, um den Namen des eine Programmentität als eine Konstante Zeichenfolge abzurufen.

```antlr
nameof_expression
    : 'nameof' '(' named_entity ')'
    ;

named_entity
    : simple_name
    | named_entity_target '.' identifier type_argument_list?
    ;

named_entity_target
    : 'this'
    | 'base'
    | named_entity 
    | predefined_type 
    | qualified_alias_member
    ;
```

Grammatisch gesehen die *Named_entity* Operand ist immer ein Ausdruck. Da `nameof` ist kein reserviertes Schlüsselwort, ein Nameof-Ausdruck ist immer mit einem Aufruf der der einfache Name syntaktisch mehrdeutig `nameof`. Aus Kompatibilitätsgründen, wenn einer Namenssuche ([einfache Namen](expressions.md#simple-names)) mit dem Namen `nameof` erfolgreich ist, wird der Ausdruck als behandelt eine *Invocation_expression* – unabhängig davon, ob der Aufruf ist Rechtlich. Andernfalls ist es eine *Nameof_expression*.

Die Bedeutung von der *Named_entity* von eine *Nameof_expression* ändert sich die Bedeutung des Zertifikats als ein Ausdruck, also entweder als eine *Simple_name*, *Base_access*  oder *Member_access*. Jedoch, in dem die Suche in beschrieben [einfache Namen](expressions.md#simple-names) und [Memberzugriff](expressions.md#member-access) führt zu einem Fehler, da ein Instanzmember in einem statischen Kontext gefunden wurde ein *Nameof_expression*keine derartigen Fehler erzeugt.

Es ist ein Fehler während der Kompilierung für eine *Named_entity* Festlegen einer Methodengruppe haben eine *Type_argument_list*. Es ist ein Kompilierzeitfehler für eine *Named_entity_target* der Typ `dynamic`.

Ein *Nameof_expression* ist ein konstanter Ausdruck des Typs `string`, und hat keine Auswirkungen zur Laufzeit. Insbesondere die *Named_entity* wird nicht ausgewertet, und wird ignoriert, für die Zwecke der Untersuchung der Zuweisung ([allgemeinen Regeln für einfache Ausdrücke](variables.md#general-rules-for-simple-expressions)). Der Wert ist der letzte Bezeichner, der die *Named_entity* vor dem letzten optionalen *Type_argument_list*, transformierten auf folgende Weise:

* Das Präfix "`@`", wenn verwendet, wird entfernt.
* Jede *Unicode_escape_sequence* wird umgewandelt in das entsprechende Unicode-Zeichen.
* Alle *Formatting_characters* werden entfernt.

Hierbei handelt es sich um die gleichen Transformationen angewendet [Bezeichner](lexical-structure.md#identifiers) beim Testen der Gleichheit zwischen Bezeichner.

TODO: Beispiele

### <a name="anonymous-method-expressions"></a>Anonyme Methode Ausdrücke

Ein *Anonymous_method_expression* ist eines der zwei Möglichkeiten, eine anonyme Funktion definieren. Diese werden weiter beschrieben [anonyme Funktionsausdrücke](expressions.md#anonymous-function-expressions).

## <a name="unary-operators"></a>Unäre Operatoren

Die `?`, `+`, `-`, `!`, `~`, `++`, `--`, umgewandelt, und `await` Operatoren werden als die Unäroperatoren bezeichnet.

```antlr
unary_expression
    : primary_expression
    | null_conditional_expression
    | '+' unary_expression
    | '-' unary_expression
    | '!' unary_expression
    | '~' unary_expression
    | pre_increment_expression
    | pre_decrement_expression
    | cast_expression
    | await_expression
    | unary_expression_unsafe
    ;
```

Wenn der Operand ein *Unary_expression* weist den Typ der Kompilierzeit `dynamic`, dynamisch gebunden ist ([dynamische Bindung](expressions.md#dynamic-binding)). In diesem Fall geben Sie die während der Kompilierung von der *Unary_expression* ist `dynamic`, und die unten beschriebene Lösung wird zur Laufzeit mit der Run-Time-Typ des Operanden.

### <a name="null-conditional-operator"></a>NULL-bedingten operator

Der nullbedingungsoperator gilt Operanden eine Liste der Vorgänge nur, wenn dieser Operand ungleich Null ist. Das Ergebnis der Anwendung des Operators ist andernfalls `null`.

```antlr
null_conditional_expression
    : primary_expression null_conditional_operations
    ;

null_conditional_operations
    : null_conditional_operations? '?' '.' identifier type_argument_list?
    | null_conditional_operations? '?' '[' argument_list ']'
    | null_conditional_operations '.' identifier type_argument_list?
    | null_conditional_operations '[' argument_list ']'
    | null_conditional_operations '(' argument_list? ')'
    ;
```

Memberzugriff und Element-Zugriffsvorgänge (der selbst bedingten Null-sein können) als auch aufrufen, kann die Liste der Vorgänge enthalten.

Beispiel: der Ausdruck `a.b?[0]?.c()` ist eine *Null_conditional_expression* mit einer *Primary_expression* `a.b` und *Null_conditional_operations* `?[0]` (bedingte Null-Elementzugriff), `?.c` (bedingte Null-Memberzugriff) und `()` (Aufruf).

Für eine *Null_conditional_expression* `E` mit einem *Primary_expression* `P`ermöglichen `E0` des Ausdrucks durch entfernen das vorangestellte textlichabgerufenwerden`?`von jedem der *Null_conditional_operations* von `E` , die über eines verfügen. Im Prinzip `E0` ist der Ausdruck, der ausgewertet wird, wenn keines der null-Überprüfungen durch dargestellt die `?`s finde einen `null`.

Darüber hinaus können `E1` des Ausdrucks durch Entfernen Sie die führende textlich abgerufen werden `?` aus nur die zuerst die *Null_conditional_operations* in `E`. Kann dies zu einem *primären Ausdruck* (wenn gab es nur eine `?`) oder einem anderen *Null_conditional_expression*.

Z. B. wenn `E` ist der Ausdruck `a.b?[0]?.c()`, klicken Sie dann `E0` ist der Ausdruck `a.b[0].c()` und `E1` ist der Ausdruck `a.b[0]?.c()`.

Wenn `E0` wird dann als nichts, klassifiziert `E` wird als "nothing" klassifiziert. Andernfalls wird E als Wert klassifiziert.

`E0` und `E1` werden verwendet, um zu bestimmen, die Bedeutung der `E`:

*  Wenn `E` tritt ein, wenn eine *Statement_expression* die Bedeutung der `E` ist identisch mit der Anweisung

   ```csharp
   if ((object)P != null) E1;
   ```

   mit dem Unterschied, dass P nur einmal ausgewertet wird.

*  Andernfalls gilt: Wenn `E0` wird klassifiziert als "nothing" ein Fehler während der Kompilierung auftritt.

*  Andernfalls können `T0` den Typ der `E0`.

   *  Wenn `T0` ist ein Typparameter, die ein Verweistyp oder ein NULL-Werte werden tritt ein Fehler während der Kompilierung nicht bekannt ist.

   *  Wenn `T0` ist ein NULL-Werte, und klicken Sie dann auf den Typ des `E` ist `T0?`, und die Bedeutung der `E` ist identisch mit

      ```csharp
      ((object)P == null) ? (T0?)null : E1
      ```

      mit dem Unterschied, dass `P` wird nur einmal ausgewertet.

   *  Andernfalls ist des Typs von E T0, und die Bedeutung von E ist identisch mit

      ```csharp
      ((object)P == null) ? null : E1
      ```

      mit dem Unterschied, dass `P` wird nur einmal ausgewertet.

Wenn `E1` ist selbst ein *Null_conditional_expression*, klicken Sie dann diese Regeln werden angewendet, in diesem Fall die Tests für die Schachtelung `null` bis keine weiteren `?`des, und der Ausdruck wurde reduziert ganz nach unten auf dem primären Ausdruck `E0`.

Z. B. wenn der Ausdruck `a.b?[0]?.c()` tritt ein, wenn eine Anweisung-Ausdruck, wie die Anweisung:
```csharp
a.b?[0]?.c();
```
die Bedeutung ist äquivalent zu:
```csharp
if (a.b != null) a.b[0]?.c();
```
Dies entspricht erneut dem:
```csharp
if (a.b != null) if (a.b[0] != null) a.b[0].c();
```
Mit dem Unterschied, dass `a.b` und `a.b[0]` werden nur einmal ausgewertet.

Wenn sie in einem Kontext tritt auf, in dem sein Wert, wie in verwendet wird:
```csharp
var x = a.b?[0]?.c();
```
und vorausgesetzt, dass der Typ des letzten Aufrufs kein Typ NULL-Werte ist, entspricht die Bedeutung:
```csharp
var x = (a.b == null) ? null : (a.b[0] == null) ? null : a.b[0].c();
```
Mit dem Unterschied, dass `a.b` und `a.b[0]` werden nur einmal ausgewertet.

#### <a name="null-conditional-expressions-as-projection-initializers"></a>NULL-bedingte Ausdrücke als Projektionsinitialisierer

Ein Null-bedingten Ausdruck ist nur zulässig, wie eine *Member_declarator* in einer *Anonymous_object_creation_expression* ([anonyme Erstellung Objektausdrücke](expressions.md#anonymous-object-creation-expressions)) Wenn Es endet mit ein (optional auch nullbedingte) Memberzugriff. Grammatisch, kann diese Anforderung ausgedrückt werden:

```antlr
null_conditional_member_access
    : primary_expression null_conditional_operations? '?' '.' identifier type_argument_list?
    | primary_expression null_conditional_operations '.' identifier type_argument_list?
    ;
```

Dies ist ein Sonderfall der Grammatik für *Null_conditional_expression* oben. Die Produktion *Member_declarator* in [anonyme Erstellung Objektausdrücke](expressions.md#anonymous-object-creation-expressions) dann schließt nur *Null_conditional_member_access*.

#### <a name="null-conditional-expressions-as-statement-expressions"></a>NULL-bedingte Ausdrücke als Anweisungsausdrücke

Ein Null-bedingten Ausdruck ist nur zulässig, als eine *Statement_expression* ([ausdrucksanweisungen](statements.md#expression-statements)) Wenn sie mit einem Aufruf beendet wird. Grammatisch, kann diese Anforderung ausgedrückt werden:

```antlr
null_conditional_invocation_expression
    : primary_expression null_conditional_operations '(' argument_list? ')'
    ;
```

Dies ist ein Sonderfall der Grammatik für *Null_conditional_expression* oben. Die Produktion *Statement_expression* in [ausdrucksanweisungen](statements.md#expression-statements) dann schließt nur *Null_conditional_invocation_expression*.


### <a name="unary-plus-operator"></a>Unärer Plus-Operator

Für einen Vorgang des Formulars `+x`, Auflösen der Überladung unäroperator ([unäroperator überladungsauflösung](expressions.md#unary-operator-overload-resolution)) wird angewendet, um einen bestimmten Operator-Implementierung auswählen. Der Operand wird in der Parametertyp des ausgewählten Operator konvertiert, und der Typ des Ergebnisses ist der Rückgabetyp des Operators. Die vordefinierten unären plus -Operatoren sind:

```csharp
int operator +(int x);
uint operator +(uint x);
long operator +(long x);
ulong operator +(ulong x);
float operator +(float x);
double operator +(double x);
decimal operator +(decimal x);
```

Für die einzelnen Operatoren ist das Ergebnis einfach der Wert des Operanden.

### <a name="unary-minus-operator"></a>Unäres minus-operator

Für einen Vorgang des Formulars `-x`, Auflösen der Überladung unäroperator ([unäroperator überladungsauflösung](expressions.md#unary-operator-overload-resolution)) wird angewendet, um einen bestimmten Operator-Implementierung auswählen. Der Operand wird in der Parametertyp des ausgewählten Operator konvertiert, und der Typ des Ergebnisses ist der Rückgabetyp des Operators. Die vordefinierten Negationsoperatoren sind:

*  Ganze Zahl Negation:

   ```csharp
   int operator -(int x);
   long operator -(long x);
   ```

   Das Ergebnis wird durch das Subtrahieren berechnet `x` 0 (null). Wenn der Wert von der `x` ist die kleinste darstellbare Wert des Typs Operand (-2 ^ 31 für `int` oder-2 ^ 63 für `long`), klicken Sie dann die mathematische Negation des `x` ist nicht in den Operandentyp dargestellt werden kann. Wenn dies in erfolgt eine `checked` Kontext eine `System.OverflowException` ausgelöst; wenn es erfolgt eine `unchecked` Kontext das Ergebnis ist der Wert des Operanden und der Überlauf wird nicht gemeldet.

   Wenn der Operand der Negationsoperator vom Typ `uint`, es ist in den Typ konvertiert `long`, und der Typ des Ergebnisses ist `long`. Eine Ausnahme ist die Regel, die `int` Wert von – 2147483648 (-2 ^ 31) als ein Ganzzahlliteral im Dezimalformat geschrieben werden ([Integer-Literale](lexical-structure.md#integer-literals)).

   Wenn der Operand der Negationsoperator vom Typ `ulong`, ein Fehler während der Kompilierung auftritt. Eine Ausnahme ist die Regel, die `long` Wert-9223372036854775808 (-2 ^ 63) als ein Ganzzahlliteral im Dezimalformat geschrieben werden ([Integer-Literale](lexical-structure.md#integer-literals)).

*  Gleitkomma Negation:

   ```csharp
   float operator -(float x);
   double operator -(double x);
   ```

   Das Ergebnis ist der Wert des `x` mit umgekehrtem Vorzeichen. Wenn `x` NaN ist, ist das Ergebnis ist ebenfalls NaN.

*  Decimal Negation:

   ```csharp
   decimal operator -(decimal x);
   ```

   Das Ergebnis wird durch das Subtrahieren berechnet `x` 0 (null). Decimal Negation ist äquivalent zur Verwendung der unäre Minusoperator des Typs `System.Decimal`.

### <a name="logical-negation-operator"></a>Logischer Negationsoperator

Für einen Vorgang des Formulars `!x`, Auflösen der Überladung unäroperator ([unäroperator überladungsauflösung](expressions.md#unary-operator-overload-resolution)) wird angewendet, um einen bestimmten Operator-Implementierung auswählen. Der Operand wird in der Parametertyp des ausgewählten Operator konvertiert, und der Typ des Ergebnisses ist der Rückgabetyp des Operators. Nur eine vordefinierte Logischer Negationsoperator vorhanden ist:
```csharp
bool operator !(bool x);
```

Dieser Operator wird die logische Negation des Operanden berechnet: Wenn der Operand ist `true`, das Ergebnis ist `false`. Wenn der Operand ist `false`, das Ergebnis ist `true`.

### <a name="bitwise-complement-operator"></a>Bitweiser Komplementoperator

Für einen Vorgang des Formulars `~x`, Auflösen der Überladung unäroperator ([unäroperator überladungsauflösung](expressions.md#unary-operator-overload-resolution)) wird angewendet, um einen bestimmten Operator-Implementierung auswählen. Der Operand wird in der Parametertyp des ausgewählten Operator konvertiert, und der Typ des Ergebnisses ist der Rückgabetyp des Operators. Die vordefinierten bitweiser Komplementoperatoren sind:
```csharp
int operator ~(int x);
uint operator ~(uint x);
long operator ~(long x);
ulong operator ~(ulong x);
```

Für die einzelnen Operatoren, ist das Ergebnis des Vorgangs das bitweise Komplement des `x`.

Jeder Enumerationstyp `E` implizit bietet die folgenden bitweiser Komplementoperator:

```csharp
E operator ~(E x);
```

Das Ergebnis der Auswertung `~x`, wobei `x` ist ein Ausdruck eines Enumerationstyps `E` mit einem zugrunde liegenden Typ `U`, entspricht genau dem Auswerten von `(E)(~(U)x)`, außer dass die Konvertierung in `E` ist als immer ausgeführt. sofern es sich um ein `unchecked` Kontext ([checked und unchecked Operatoren](expressions.md#the-checked-and-unchecked-operators)).

### <a name="prefix-increment-and-decrement-operators"></a>Präfix-Inkrement und Dekrement-Operatoren

```antlr
pre_increment_expression
    : '++' unary_expression
    ;

pre_decrement_expression
    : '--' unary_expression
    ;
```

Der Operand eines Präfix Inkrement oder Dekrement Operation muss ein Ausdruck, der als eine Variable, einen Eigenschaftenzugriff oder Indexzugriff klassifiziert sein. Das Ergebnis des Vorgangs ist ein Wert des gleichen Typs als Operand.

Wenn der Operand eines Präfixes erhöhen oder dekrementvorgang ist eine Eigenschaft oder Zugriffs auf Indexer, Eigenschaft oder des Indexers benötigen Sie Folgendes ein `get` und `set` Accessor. Wenn dies nicht der Fall ist, tritt ein Fehler während der Bindung.

Unäroperator der überladungsauflösung ([unäroperator überladungsauflösung](expressions.md#unary-operator-overload-resolution)) wird angewendet, um einen bestimmten Operator-Implementierung auswählen. Vordefinierte `++` und `--` Operatoren vorhanden, für die folgenden Typen: `sbyte`, `byte`, `short`, `ushort`, `int`, `uint`, `long`, `ulong`, `char` , `float`, `double`, `decimal`, und alle Enumerationstypen. Die vordefinierten `++` Operatoren zurückgegeben, den von den Operanden und den vordefinierten 1 hinzugefügt erzeugten Wert `--` Operatoren geben einen Wert durch Subtrahieren der Operand 1 zurück. In einem `checked` Kontext, wenn das Ergebnis dieser Addition oder Subtraktion außerhalb des Bereichs des Ergebnistyps ist und der Ergebnistyp ein ganzzahliger Typ oder ein Enumerationstyp ist, eine `System.OverflowException` ausgelöst.

Die Verarbeitung zur Laufzeit eine "Präfix"-Inkrementoperator oder dekrementvorgang des Formulars `++x` oder `--x` umfasst die folgenden Schritte aus:

*   Wenn `x` wird als Variable klassifiziert:
    * `x` wird ausgewertet, um die Variable zu erstellen.
    * Der ausgewählte Operator wird aufgerufen, mit dem Wert des `x` als Argument.
    * Der Wert, der vom Operator zurückgegebenen befindet sich in den Speicherort, durch die Auswertung von `x`.
    * Der Wert, der vom Operator zurückgegeben wird das Ergebnis des Vorgangs.
*   Wenn `x` wird als eine Eigenschaft oder der Indexer Zugriff klassifiziert:
    * Der Instanzausdruck (Wenn `x` ist nicht `static`) und die Argumentliste (Wenn `x` Indexzugriff ist) zugeordneten `x` werden ausgewertet, und die Ergebnisse werden in der nachfolgenden verwendet `get` und `set` Accessor-Aufrufe.
    * Die `get` Accessor `x` aufgerufen wird.
    * Der ausgewählte Operator wird aufgerufen, mit den Rückgabewert von der `get` Accessor als Argument.
    * Die `set` Accessor `x` wird aufgerufen, mit dem Wert, der zurückgegeben wird, durch den Operator als seine `value` Argument.
    * Der Wert, der vom Operator zurückgegeben wird das Ergebnis des Vorgangs.

Die `++` und `--` Operatoren unterstützt auch Postfix Notation ([Postfix-Inkrement und Dekrement-Operatoren](expressions.md#postfix-increment-and-decrement-operators)). In der Regel das Ergebnis des `x++` oder `x--` ist der Wert des `x` vor dem Vorgang, während das Ergebnis des `++x` oder `--x` ist der Wert des `x` nach Abschluss des Vorgangs. In beiden Fällen `x` selbst hat den gleichen Wert nach Abschluss des Vorgangs.

Ein `operator++` oder `operator--` Implementierung kann mithilfe der Präfix oder Postfix-Notation aufgerufen werden. Es ist nicht möglich, separate Implementierungen für die beiden Notationen zu haben.

### <a name="cast-expressions"></a>CAST-Ausdrücke

Ein *Cast_expression* wird verwendet, um einen Ausdruck explizit in einen angegebenen Typ zu konvertieren.

```antlr
cast_expression
    : '(' type ')' unary_expression
    ;
```

Ein *Cast_expression* des Formulars `(T)E`, wobei `T` ist eine *Typ* und `E` ist eine *Unary_expression*, führt eine explizite Konvertierung ([explizite Konvertierungen](conversions.md#explicit-conversions)) des Werts der `E` eingeben `T`. Wenn keine explizite Konvertierung von vorhanden ist `E` zu `T`, tritt ein Fehler während der Bindung. Andernfalls ist das Ergebnis der durch die explizite Konvertierung erzeugte Wert. Das Ergebnis wird immer als Wert klassifiziert, wenn `E` steht für eine Variable.

Die Grammatik für eine *Cast_expression* führt zu einer bestimmten syntaktischen Mehrdeutigkeiten. Beispielsweise der Ausdruck `(x)-y` entweder als interpretiert werden konnte eine *Cast_expression* (eine Umwandlung von `-y` eingeben `x`) oder als ein *Additive_expression* in Kombination mit einer *Parenthesized_expression* (die den Wert berechnet `x - y)`.

Auflösen *Cast_expression* Mehrdeutigkeiten, die folgende Regel vorhanden ist: Eine Sequenz einer oder mehreren *token*s ([Leerraum](lexical-structure.md#white-space)) eingeschlossen in Klammern wird als Anfang betrachtet eine *Cast_expression* nur dann, wenn mindestens eine der folgenden erfüllt sind:

*  Die Sequenz von Token ist richtig Grammatik für eine *Typ*, aber nicht für eine *Ausdruck*.
*  Die Sequenz von Token ist richtig Grammatik für eine *Typ*, und das Token direkt nach der schließenden Klammer des Tokens "`~`", das Token "`!`", das Token "`(`", wird eine  *Bezeichner* ([Unicode-Escape-Zeichensequenzen](lexical-structure.md#unicode-character-escape-sequences)), ein *literal* ([Literale](lexical-structure.md#literals)), oder ein beliebiges *Schlüsselwort*([Schlüsselwörter](lexical-structure.md#keywords)) mit Ausnahme von `as` und `is`.

Der Begriff "korrekte Grammatik" bedeutet, dass nur, die, die die Folge von Token, die bestimmten grammatische Produktion entsprechen muss. Dabei wird insbesondere die eigentliche Bedeutung der einzelnen Bezeichner nicht berücksichtigt. Z. B. wenn `x` und `y` Bezeichner sind, klicken Sie dann `x.y` entspricht der korrekten Grammatik für einen Typ, selbst wenn `x.y` nicht tatsächlich einen Typ anzugeben.

Von der Mehrdeutigkeit Regel folgt, dass, wenn `x` und `y` Bezeichner sind, `(x)y`, `(x)(y)`, und `(x)(-y)` sind *Cast_expression*s, aber `(x)-y` nicht, selbst wenn `x` identifiziert einen Typ. Jedoch wenn `x` ist ein Schlüsselwort, das einen vordefinierten Typ angibt (wie z. B. `int`), dann sind alle vier Formen *Cast_expression*s (da solches Schlüsselwort nicht möglicherweise einen Ausdruck selbst handeln kann).

### <a name="await-expressions"></a>Await-Ausdrücken

Der Await-Operator wird verwendet, die einschließenden Async-Funktion angehalten, bis zum Abschluss der asynchronen Vorgangs, durch den Operanden dargestellt.

```antlr
await_expression
    : 'await' unary_expression
    ;
```

Ein *Await_expression* ist nur im Text einer asynchronen Funktion zulässig ([Iteratoren](classes.md#iterators)). In der nächsten einschließenden Async-Funktion, ein *Await_expression* kann nicht an diesen Orten auftreten:

*  In einer anonymen Funktion für geschachtelte (nicht-Async)
*  Innerhalb des Blocks, der eine *Lock_statement*
*  In einem unsicheren Kontext

Beachten Sie, dass ein *Await_expression* kann nicht in den meisten Stellen im Auftreten einer *Query_expression*, da die syntaktisch transformiert werden, um nicht-Async-Lambda-Ausdrücke verwenden.

Innerhalb einer Async-Funktion `await` kann nicht als Bezeichner verwendet werden. Daher besteht keine syntaktischen Mehrdeutigkeit zwischen await-Ausdrücken und verschiedene Ausdrücke, die im Zusammenhang mit Bezeichnern. Außerhalb von asynchronen Funktionen `await` fungiert als ein normaler Bezeichner.

Der Operand ein *Await_expression* heißt die ***Aufgabe***. Es stellt einen asynchronen Vorgang, die möglicherweise oder ist möglicherweise unvollständig zum Zeitpunkt der *Await_expression* ausgewertet wird. Der Zweck des Await-Operators werden von der einschließenden Funktion für die asynchrone Ausführung anzuhalten, bis die erwartete Aufgabe abgeschlossen ist, und klicken Sie dann ihr Ergebnis zu erhalten.

#### <a name="awaitable-expressions"></a>Awaitable-Ausdrücke

Die Aufgabe, ein Await-Ausdruck ist erforderlich, um werden ***awaitable***. Ein Ausdruck `t` ist "awaitable", wenn eine der folgenden enthält:

*  `t` ist vom Kompilierzeittyp `dynamic`
*  `t` verfügt über eine zugegriffen werden kann oder Erweiterungsmethode-Methode wird aufgerufen, `GetAwaiter` ohne Parameter und keine Typparameter und einen Rückgabetyp `A` für die alle der folgenden enthalten:
   * `A` implementiert die Schnittstelle `System.Runtime.CompilerServices.INotifyCompletion` (im folgenden genannten `INotifyCompletion` aus Gründen der Übersichtlichkeit)
   * `A` verfügt über eine Instanzeigenschaft zugegriffen werden kann, lesbaren `IsCompleted` des Typs `bool`
   * `A` verfügt über eine zugängliche Instanzmethode Methode `GetResult` ohne Parameter und keine Typparameter

Der Zweck der `GetAwaiter` Methode besteht darin, erhalten eine ***"awaiter"*** für den Task. Der Typ `A` heißt die ***"awaiter"-Typ*** für den Ausdruck "await".

Der Zweck der `IsCompleted` Eigenschaft ist, um festzustellen, ob die Aufgabe bereits abgeschlossen ist. Wenn dies der Fall ist, besteht keine Notwendigkeit, Auswertung anzuhalten.

Der Zweck der `INotifyCompletion.OnCompleted` Methode besteht darin, registrieren eine "fortsetzungs" an die Aufgabe, d. h. einen Delegaten (des Typs `System.Action`), wird aufgerufen, nachdem die Aufgabe abgeschlossen ist.

Der Zweck der `GetResult` Methode besteht darin, das Ergebnis des Vorgangs zu erhalten, sobald sie abgeschlossen ist. Dieses Ergebnis möglicherweise den erfolgreichen Abschluss, möglicherweise mit einem Ergebniswert, oder es kann sein, eine Ausnahme, indem ausgelöst wird die `GetResult` Methode.

#### <a name="classification-of-await-expressions"></a>Klassifizierung des await-Ausdrücken

Der Ausdruck `await t` wird die gleiche Weise wie der Ausdruck klassifiziert `(t).GetAwaiter().GetResult()`. Daher, wenn der Rückgabetyp der `GetResult` ist `void`, *Await_expression* wird als "nothing" klassifiziert. Wenn sie einen nicht-Void-Rückgabetyp verfügt `T`, *Await_expression* wird als ein Wert vom Typ klassifiziert `T`.

#### <a name="runtime-evaluation-of-await-expressions"></a>Runtime-Auswertung von await-Ausdrücken

Zur Laufzeit wird der Ausdruck `await t` wird wie folgt ausgewertet:

*  Ein awaiter-Element `a` erhalten Sie durch die Auswertung des Ausdrucks `(t).GetAwaiter()`.
*  Ein `bool` `b` erhalten Sie durch die Auswertung des Ausdrucks `(a).IsCompleted`.
*  Wenn `b` ist `false` und Auswertung, ob abhängt `a` implementiert die Schnittstelle `System.Runtime.CompilerServices.ICriticalNotifyCompletion` (im folgenden genannten `ICriticalNotifyCompletion` aus Gründen der Übersichtlichkeit). Diese Überprüfung wird durchgeführt, auf die Bindungszeit; zur Laufzeit z. B. wenn `a` hat den Kompilierzeittyp `dynamic`, und zum Zeitpunkt der Kompilierung andernfalls. Lassen Sie `r` Wiederaufnahmedelegat zu kennzeichnen ([Iteratoren](classes.md#iterators)):
    * Wenn `a` implementiert nicht `ICriticalNotifyCompletion`, klicken Sie dann den Ausdruck `(a as (INotifyCompletion)).OnCompleted(r)` ausgewertet wird.
    * Wenn `a` implementiert `ICriticalNotifyCompletion`, klicken Sie dann den Ausdruck `(a as (ICriticalNotifyCompletion)).UnsafeOnCompleted(r)` ausgewertet wird.
    * Auswertung wird angehalten, und die Kontrolle an den aktuellen Aufrufer der Async-Funktion zurückgegeben wird.
*  Entweder unmittelbar auf (Wenn `b` wurde `true`), oder bei späteren Aufruf der Wiederaufnahmedelegat (Wenn `b` wurde `false`), der Ausdruck `(a).GetResult()` ausgewertet wird. Wenn sie einen Wert zurückgibt, ist dieser Wert das Ergebnis der *Await_expression*. Andernfalls ist das Ergebnis mit "nothing".

Ein awaiter-Element-Implementierung der Schnittstellenmethoden `INotifyCompletion.OnCompleted` und `ICriticalNotifyCompletion.UnsafeOnCompleted` sollten dazu führen, dass der Delegat `r` höchstens einmal aufgerufen werden. Andernfalls ist das Verhalten der einschließenden Async-Funktion nicht definiert.

## <a name="arithmetic-operators"></a>Arithmetische Operatoren

Die `*`, `/`, `%`, `+`, und `-` Operatoren werden die arithmetischen Operatoren bezeichnet.

```antlr
multiplicative_expression
    : unary_expression
    | multiplicative_expression '*' unary_expression
    | multiplicative_expression '/' unary_expression
    | multiplicative_expression '%' unary_expression
    ;

additive_expression
    : multiplicative_expression
    | additive_expression '+' multiplicative_expression
    | additive_expression '-' multiplicative_expression
    ;
```

Wenn ein Operand eines arithmetischen Operators der Kompilierzeit-Typ hat `dynamic`, und klicken Sie dann der Ausdruck dynamisch gebunden ist ([dynamische Bindung](expressions.md#dynamic-binding)). In diesem Fall den Kompilierzeittyp des Ausdrucks ist `dynamic`, und die unten beschriebene Lösung wird zur Laufzeit mit dem Laufzeit-Typ, der diese Operanden mit der Kompilierzeittyp `dynamic`.

### <a name="multiplication-operator"></a>Multiplikationsoperator

Für einen Vorgang des Formulars `x * y`, binärer Operator der überladungsauflösung ([binärer Operator überladungsauflösung](expressions.md#binary-operator-overload-resolution)) wird angewendet, um einen bestimmten Operator-Implementierung auswählen. Die Operanden werden in die Parametertypen des ausgewählten Operator konvertiert, und der Typ des Ergebnisses ist der Rückgabetyp des Operators.

Der vordefinierte Multiplikationsoperatoren sind unten aufgeführt. Alle Operatoren ermittelt das Produkt der `x` und `y`.

*  Multiplikation:

   ```csharp
   int operator *(int x, int y);
   uint operator *(uint x, uint y);
   long operator *(long x, long y);
   ulong operator *(ulong x, ulong y);
   ```

   In einer `checked` Kontext, wenn außerhalb des Bereichs des Ergebnistyps, das Produkt ist eine `System.OverflowException` ausgelöst. In einer `unchecked` Kontext Überläufe werden nicht gemeldet, und alle signifikanten höherwertigen Bits außerhalb des Bereichs des Ergebnistyps werden verworfen.


*  Multiplikation mit Gleitkommawerten:

   ```csharp
   float operator *(float x, float y);
   double operator *(double x, double y);
   ```

   Das Produkt wird gemäß den Regeln der IEEE 754-Arithmetik berechnet. Die folgende Tabelle enthält die Ergebnisse der alle möglichen Kombinationen von endlichen Werten ungleich NULL, Nullen, unendliche und NaN. In der Tabelle `x` und `y` positive endliche Werten sind. `z` ist das Ergebnis des `x * y`. Wenn das Ergebnis zu groß für den Zieltyp `z` unendlich ist. Wenn das Ergebnis für den Zieltyp zu klein ist `z` ist 0 (null).

   |      |      |      |     |     |      |      |     |
   |:----:|-----:|:----:|:---:|:---:|:----:|:----:|:----|
   |      | +y   | -y   | +0  | -0  | +inf | -inf | NaN | 
   | +x   | +z   | -z   | +0  | -0  | +inf | -inf | NaN | 
   | -x   | -z   | +z   | -0  | +0  | -inf | +inf | NaN | 
   | +0   | +0   | -0   | +0  | -0  | NaN  | NaN  | NaN | 
   | -0   | -0   | +0   | -0  | +0  | NaN  | NaN  | NaN | 
   | +inf | +inf | -inf | NaN | NaN | +inf | -inf | NaN | 
   | -inf | -inf | +inf | NaN | NaN | -inf | +inf | NaN | 
   | NaN  | NaN  | NaN  | NaN | NaN | NaN  | NaN  | NaN | 

*  Dezimale Multiplikation:

   ```csharp
   decimal operator *(decimal x, decimal y);
   ```

   Ist der resultierende Wert zu groß, um die Darstellung in der `decimal` Format eine `System.OverflowException` ausgelöst. Ist der Ergebniswert zu klein, um das Darstellen der `decimal` Format, das Ergebnis ist 0 (null). Die Dezimalstellen des Ergebnisses wird vor der Rundung, ist die Summe aus der Skalierung der beiden Operanden.

   Decimal Multiplikation ist äquivalent zur Verwendung des Multiplikationsoperators des Typs `System.Decimal`.


### <a name="division-operator"></a>Divisionsoperator

Für einen Vorgang des Formulars `x / y`, binärer Operator der überladungsauflösung ([binärer Operator überladungsauflösung](expressions.md#binary-operator-overload-resolution)) wird angewendet, um einen bestimmten Operator-Implementierung auswählen. Die Operanden werden in die Parametertypen des ausgewählten Operator konvertiert, und der Typ des Ergebnisses ist der Rückgabetyp des Operators.

Der vordefinierte Divisionsoperatoren sind unten aufgeführt. Alle Operatoren berechnen den Quotienten `x` und `y`.

*  Division ganzer Zahlen kommt:

   ```csharp
   int operator /(int x, int y);
   uint operator /(uint x, uint y);
   long operator /(long x, long y);
   ulong operator /(ulong x, ulong y);
   ```

   Wenn der Wert des rechten Operanden NULL ist, ist eine `System.DivideByZeroException` ausgelöst.

   Die Division rundet das Ergebnis in Richtung 0 (null). Daher ist der Absolute Wert des Ergebnisses wird die größte ganze Zahl möglich, die kleiner oder gleich dem absoluten Wert des Quotienten aus zwei Operanden ist. Das Ergebnis ist 0 (null) oder positiv, wenn die beiden Operanden der gleichen Anmeldebenutzeroberfläche und 0 (null) oder negativ, wenn die beiden Operanden entgegengesetzten signiert haben.

   Wenn der linke Operand der kleinste darstellbare ist `int` oder `long` Wert und der Rechte Operand ist `-1`, einem Überlauf kommt. In einem `checked` Kontext Dies bewirkt, dass eine `System.ArithmeticException` (oder eine Unterklasse davon) ausgelöst wird. In einer `unchecked` Kontext es wird durch die Implementierung definiert, ob eine `System.ArithmeticException` (oder eine Unterklasse davon) wird ausgelöst, oder der Überlauf wird mit den sich ergebenden Wert, der der linke Operand wird nicht gemeldet.

*  Gleitkommadivision:

   ```csharp
   float operator /(float x, float y);
   double operator /(double x, double y);
   ```

   Der Quotient ist gemäß den Regeln der IEEE 754-Arithmetik berechnet. Die folgende Tabelle enthält die Ergebnisse der alle möglichen Kombinationen von endlichen Werten ungleich NULL, Nullen, unendliche und NaN. In der Tabelle `x` und `y` positive endliche Werten sind. `z` ist das Ergebnis des `x / y`. Wenn das Ergebnis zu groß für den Zieltyp `z` unendlich ist. Wenn das Ergebnis für den Zieltyp zu klein ist `z` ist 0 (null).

   |      |      |      |      |      |      |      |      |
   |:----:|:----:|:----:|:----:|:----:|:----:|:----:|:----:|
   |      | +y   | -y   | +0   | -0   | +inf | -inf | NaN  | 
   | +x   | +z   | -z   | +inf | -inf | +0   | -0   | NaN  | 
   | -x   | -z   | +z   | -inf | +inf | -0   | +0   | NaN  | 
   | +0   | +0   | -0   | NaN  | NaN  | +0   | -0   | NaN  | 
   | -0   | -0   | +0   | NaN  | NaN  | -0   | +0   | NaN  | 
   | +inf | +inf | -inf | +inf | -inf | NaN  | NaN  | NaN  | 
   | -inf | -inf | +inf | -inf | +inf | NaN  | NaN  | NaN  | 
   | NaN  | NaN  | NaN  | NaN  | NaN  | NaN  | NaN  | NaN  | 

*  Dezimale Division:

   ```csharp
   decimal operator /(decimal x, decimal y);
   ```

   Wenn der Wert des rechten Operanden NULL ist, ist eine `System.DivideByZeroException` ausgelöst. Ist der resultierende Wert zu groß, um die Darstellung in der `decimal` Format eine `System.OverflowException` ausgelöst. Ist der Ergebniswert zu klein, um das Darstellen der `decimal` Format, das Ergebnis ist 0 (null). Die Dezimalstellen des Ergebnisses ist der kleinste Skala, die ein Ergebnis gleich beibehalten werden das am nächsten darstellbaren decimal-Wert auf "true" mathematische Ergebnis.

   Dezimale Division ist äquivalent zur Verwendung des Divisionsoperators des Typs `System.Decimal`.


### <a name="remainder-operator"></a>Restoperator

Für einen Vorgang des Formulars `x % y`, binärer Operator der überladungsauflösung ([binärer Operator überladungsauflösung](expressions.md#binary-operator-overload-resolution)) wird angewendet, um einen bestimmten Operator-Implementierung auswählen. Die Operanden werden in die Parametertypen des ausgewählten Operator konvertiert, und der Typ des Ergebnisses ist der Rückgabetyp des Operators.

Der vordefinierte restoperatoren sind unten aufgeführt. Alle Operatoren berechnen den Rest der Division zwischen `x` und `y`.

*  Ganzzahligen Rest:

   ```csharp
   int operator %(int x, int y);
   uint operator %(uint x, uint y);
   long operator %(long x, long y);
   ulong operator %(ulong x, ulong y);
   ```

   Das Ergebnis des `x % y` wird durch der Wert erzeugt `x - (x / y) * y`. Wenn `y` ist 0 (null), eine `System.DivideByZeroException` ausgelöst.

   Wenn der linke Operand der kleinsten ist `int` oder `long` Wert und der Rechte Operand ist `-1`, `System.OverflowException` ausgelöst. Ist in keinem Fall `x % y` löst eine Ausnahme aus, in denen `x / y` wird keine Ausnahme ausgelöst.

*  Gleitkommarest:

   ```csharp
   float operator %(float x, float y);
   double operator %(double x, double y);
   ```

   Die folgende Tabelle enthält die Ergebnisse der alle möglichen Kombinationen von endlichen Werten ungleich NULL, Nullen, unendliche und NaN. In der Tabelle `x` und `y` positive endliche Werten sind. `z` ist das Ergebnis des `x % y` und wird berechnet als `x - n * y`, wobei `n` ist die größte ganze Zahl mögliche, die kleiner als oder gleich `x / y`. Diese Methode, der Berechnung des Rests ist analog zu, die für ganzzahlige Operanden verwendet, unterscheidet sich jedoch die IEEE 754-Definition (in der `n` ist eine ganze Zahl, die am nächsten `x / y`).

   |      |      |      |      |      |      |      |      |
   |:----:|:----:|:----:|:----:|:----:|:----:|:----:|:----:|
   |      | +y   | -y   | +0   | -0   | +inf | -inf | NaN  | 
   | +x   | +z   | +z   | NaN  | NaN  | w    | w    | NaN  | 
   | -x   | -z   | -z   | NaN  | NaN  | -x   | -x   | NaN  | 
   | +0   | +0   | +0   | NaN  | NaN  | +0   | +0   | NaN  | 
   | -0   | -0   | -0   | NaN  | NaN  | -0   | -0   | NaN  | 
   | +inf | NaN  | NaN  | NaN  | NaN  | NaN  | NaN  | NaN  | 
   | -inf | NaN  | NaN  | NaN  | NaN  | NaN  | NaN  | NaN  | 
   | NaN  | NaN  | NaN  | NaN  | NaN  | NaN  | NaN  | NaN  | 

*  Decimal Rest:

   ```csharp
   decimal operator %(decimal x, decimal y);
   ```

   Wenn der Wert des rechten Operanden NULL ist, ist eine `System.DivideByZeroException` ausgelöst. Die Dezimalstellen des Ergebnisses wird vor der Rundung, ist der jeweils größere der Skalen der beiden Operanden aus, und die Vorzeichen des Ergebnisses ausgeführt, wenn ungleich NULL ist, entspricht der `x`.

   Decimal Rest ist äquivalent zur Verwendung der Restoperator vom Typ `System.Decimal`.


### <a name="addition-operator"></a>Addition-operator

Für einen Vorgang des Formulars `x + y`, binärer Operator der überladungsauflösung ([binärer Operator überladungsauflösung](expressions.md#binary-operator-overload-resolution)) wird angewendet, um einen bestimmten Operator-Implementierung auswählen. Die Operanden werden in die Parametertypen des ausgewählten Operator konvertiert, und der Typ des Ergebnisses ist der Rückgabetyp des Operators.

Die vordefinierten Additionsoperatoren sind unten aufgeführt. Für die numerischen Typen und Enumerationstypen berechnen die vordefinierten Additionsoperatoren die Summe der beiden Operanden. Wenn einer oder beide Operanden vom Typzeichenfolge sind, verketten Sie die vordefinierten Additionsoperatoren eine Zeichenfolgendarstellung der Operanden.

*  Ganze Zahl hinzufügen:

   ```csharp
   int operator +(int x, int y);
   uint operator +(uint x, uint y);
   long operator +(long x, long y);
   ulong operator +(ulong x, ulong y);
   ```

   In einem `checked` Kontext, wenn die Summe außerhalb des Bereichs des Ergebnistyps, ist eine `System.OverflowException` ausgelöst. In einer `unchecked` Kontext Überläufe werden nicht gemeldet, und alle signifikanten höherwertigen Bits außerhalb des Bereichs des Ergebnistyps werden verworfen.

*  Gleitkommaaddition:

   ```csharp
   float operator +(float x, float y);
   double operator +(double x, double y);
   ```

   Die Summe wird gemäß den Regeln der IEEE 754-Arithmetik berechnet. Die folgende Tabelle enthält die Ergebnisse der alle möglichen Kombinationen von endlichen Werten ungleich NULL, Nullen, unendliche und NaN. In der Tabelle `x` und `y` sind begrenzte Werte ungleich NULL, und `z` ist das Ergebnis des `x + y`. Wenn `x` und `y` dieselbe Größenordnung haben, aber umgekehrte meldet, `z` plus null ist. Wenn `x + y` ist zu groß für den Zieltyp darstellen `z` unendlich und hat das gleichen Vorzeichen wie `x + y`.

   |      |      |      |      |      |      |      |
   |:----:|:----:|:----:|:----:|:----:|:----:|:----:|
   |      | y    | +0   | -0   | +inf | -inf | NaN  | 
   | w    | z    | w    | w    | +inf | -inf | NaN  | 
   | +0   | y    | +0   | +0   | +inf | -inf | NaN  | 
   | -0   | y    | +0   | -0   | +inf | -inf | NaN  | 
   | +inf | +inf | +inf | +inf | +inf | NaN  | NaN  | 
   | -inf | -inf | -inf | -inf | NaN  | -inf | NaN  | 
   | NaN  | NaN  | NaN  | NaN  | NaN  | NaN  | NaN  | 

*  Dezimalstellen hinzufügen:

   ```csharp
   decimal operator +(decimal x, decimal y);
   ```

   Ist der resultierende Wert zu groß, um die Darstellung in der `decimal` Format eine `System.OverflowException` ausgelöst. Die Dezimalstellen des Ergebnisses wird vor der Rundung, ist der jeweils größere der Skalen der beiden Operanden.

   Dezimale Addition ist äquivalent zur Verwendung des Addition-Operators vom Typ `System.Decimal`.

*  Hinzufügen der Enumeration. Jeder Enumerationstyp implizit bietet die folgenden vordefinierten Operatoren, in denen `E` ist der Enumerationstyp und `U` ist der zugrunde liegenden Typ des `E`:

   ```csharp
   E operator +(E x, U y);
   E operator +(U x, E y);
   ```

   Zur Laufzeit diese Operatoren ausgewertet werden, genau wie `(E)((U)x + (U)y)`.

*  Verketten von Zeichenfolgen:

   ```csharp
   string operator +(string x, string y);
   string operator +(string x, object y);
   string operator +(object x, string y);
   ```

   Diese Überladungen der Binärdatei `+` Operator führen eine zeichenfolgenverkettung aus. Wenn ein Operand vom zeichenfolgenverkettung wird `null`, eine leere Zeichenfolge ersetzt wird. Andernfalls jedes Argument keine Zeichenfolge durch Aufrufen der virtuellen in seine Zeichenfolgendarstellung konvertiert ist `ToString` Methode geerbt vom Typ `object`. Wenn `ToString` gibt `null`, eine leere Zeichenfolge ersetzt wird.

   ```csharp
   using System;
   
   class Test
   {
       static void Main() {
           string s = null;
           Console.WriteLine("s = >" + s + "<");        // displays s = ><
           int i = 1;
           Console.WriteLine("i = " + i);               // displays i = 1
           float f = 1.2300E+15F;
           Console.WriteLine("f = " + f);               // displays f = 1.23E+15
           decimal d = 2.900m;
           Console.WriteLine("d = " + d);               // displays d = 2.900
       }
   }
   ```

   Das Ergebnis der Operator für zeichenfolgenverkettungen ist eine Zeichenfolge, die aus der Zeichen des linken Operanden gefolgt von den Zeichen des rechten Operanden. Gibt zurück, der Operator für zeichenfolgenverkettung nie eine `null` Wert. Ein `System.OutOfMemoryException` möglicherweise ausgelöst, wenn nicht genügend Arbeitsspeicher verfügbar, um die resultierende Zeichenfolge steht.

*  Delegieren Sie die Kombination aus. Alle Delegattypen implizit bietet die folgenden vordefinierten Operator, in denen `D` ist der Delegattyp:

   ```csharp
   D operator +(D x, D y);
   ```

   Die Binärdatei `+` Operator führt Delegatkombination aus, wenn beide Operanden vom Delegattyp sind `D`. (Wenn die Operanden unterschiedliche Delegattypen aufweisen, tritt ein Fehler während der Bindung.) Wenn der erste Operand `null`, das Ergebnis des Vorgangs ist der Wert des zweiten Operanden (selbst wenn das auch ist `null`). Wenn der zweite Operand ist, andernfalls `null`, lautet das Ergebnis des Vorgangs der Wert des ersten Operanden. Andernfalls ist das Ergebnis des Vorgangs ein neuer Delegaten Instanz, die, wenn aufgerufen wird, ruft der erste Operand und ruft dann den zweiten Operanden auf. Beispiele für Delegatkombination, finden Sie unter [Subtraktionsoperator](expressions.md#subtraction-operator) und [Delegataufruf](delegates.md#delegate-invocation). Da `System.Delegate` ist es sich nicht um ein Delegattyp, `operator`  `+` ist nicht definiert.

### <a name="subtraction-operator"></a>Subtraktionsoperator

Für einen Vorgang des Formulars `x - y`, binärer Operator der überladungsauflösung ([binärer Operator überladungsauflösung](expressions.md#binary-operator-overload-resolution)) wird angewendet, um einen bestimmten Operator-Implementierung auswählen. Die Operanden werden in die Parametertypen des ausgewählten Operator konvertiert, und der Typ des Ergebnisses ist der Rückgabetyp des Operators.

Die vordefinierten Subtraktionsoperatoren sind unten aufgeführt. Die Operatoren, die alle subtrahieren `y` aus `x`.

*  Ganze Zahl Subtraktion:

   ```csharp
   int operator -(int x, int y);
   uint operator -(uint x, uint y);
   long operator -(long x, long y);
   ulong operator -(ulong x, ulong y);
   ```

   In einem `checked` Kontext, wenn außerhalb des Bereichs des Ergebnistyps, der Unterschied ist eine `System.OverflowException` ausgelöst. In einer `unchecked` Kontext Überläufe werden nicht gemeldet, und alle signifikanten höherwertigen Bits außerhalb des Bereichs des Ergebnistyps werden verworfen.

*  Subtraktion von Gleitkommazahlen:

   ```csharp
   float operator -(float x, float y);
   double operator -(double x, double y);
   ```

   Der Unterschied wird gemäß den Regeln der IEEE 754-Arithmetik berechnet. Die folgende Tabelle enthält die Ergebnisse der alle möglichen Kombinationen von endlichen Werten ungleich NULL, Nullen, unendliche und NaN-Werte. In der Tabelle `x` und `y` sind begrenzte Werte ungleich NULL, und `z` ist das Ergebnis des `x - y`. Wenn `x` und `y` gleich sind, `z` plus null ist. Wenn `x - y` ist zu groß für den Zieltyp darstellen `z` unendlich und hat das gleichen Vorzeichen wie `x - y`.

   |      |      |      |      |      |      |     |
   |:----:|:----:|:----:|:----:|:----:|:----:|:---:|
   |      | y    | +0   | -0   | +inf | -inf | NaN | 
   | w    | z    | w    | w    | -inf | +inf | NaN | 
   | +0   | -y   | +0   | +0   | -inf | +inf | NaN | 
   | -0   | -y   | -0   | +0   | -inf | +inf | NaN | 
   | +inf | +inf | +inf | +inf | NaN  | +inf | NaN | 
   | -inf | -inf | -inf | -inf | -inf | NaN  | NaN | 
   | NaN  | NaN  | NaN  | NaN  | NaN  | NaN  | NaN | 

*  Dezimale Subtraktion:

   ```csharp
   decimal operator -(decimal x, decimal y);
   ```

   Ist der resultierende Wert zu groß, um die Darstellung in der `decimal` Format eine `System.OverflowException` ausgelöst. Die Dezimalstellen des Ergebnisses wird vor der Rundung, ist der jeweils größere der Skalen der beiden Operanden.

   Dezimale Subtraktion ist äquivalent zur Verwendung des Subtraktionsoperators des Typs `System.Decimal`.

*  Subtraktion der Enumeration. Jeder Enumerationstyp enthält implizit den folgenden vordefinierten-Operator, in denen `E` der Enum-Typs ist und `U` ist der zugrunde liegenden Typ des `E`:

   ```csharp
   U operator -(E x, E y);
   ```

   Dieser Operator wird ausgewertet, genau wie `(U)((U)x - (U)y)`. Das heißt, der Operator berechnet den Unterschied zwischen der Ordinalwerte der `x` und `y`, und der Typ des Ergebnisses ist der zugrunde liegende Typ der Enumeration.

   ```csharp
   E operator -(E x, U y);
   ```

   Dieser Operator wird ausgewertet, genau wie `(E)((U)x - y)`. Das heißt, subtrahiert der Operator einen Wert aus der zugrunde liegende Typ der Enumeration, dies ergibt den Wert der Enumeration.

*  Delegieren Sie zum Entfernen. Alle Delegattypen implizit bietet die folgenden vordefinierten Operator, in denen `D` ist der Delegattyp:

   ```csharp
   D operator -(D x, D y);
   ```

   Die Binärdatei `-` Operator führt delegatentfernung aus, wenn beide Operanden vom Delegattyp sind `D`. Wenn die Operanden unterschiedliche Delegattypen haben, tritt ein Fehler während der Bindung. Wenn der erste Operand `null`, ist das Ergebnis des Vorgangs `null`. Wenn der zweite Operand ist, andernfalls `null`, lautet das Ergebnis des Vorgangs der Wert des ersten Operanden. Beide Operanden darstellen, andernfalls Aufruflisten ([delegieren Deklarationen](delegates.md#delegate-declarations)) ein oder mehrere Einträge, und das Ergebnis ist eine neue Aufrufliste besteht aus den ersten Operanden Liste mit den zweiten Operanden Einträge daraus Diese Liste mit den zweiten Operanden angegeben ist eine ordnungsgemäße zusammenhängenden Unterliste der des ersten.     (Um Unterliste auf Gleichheit zu bestimmen, werden entsprechende Einträge verglichen, wie für den Delegaten Equality-Operator ([delegieren Gleichheitsoperatoren](expressions.md#delegate-equality-operators)).) Andernfalls ist das Ergebnis den Wert des linken Operanden. Keiner der Operanden des Bereichsoperators Listen wird im Prozess geändert. Wenn der zweite Operand Liste mehrerer Unterlisten zusammenhängende Einträge aus den ersten Operanden übereinstimmt, wird die am weitesten rechts befindlichen übereinstimmende Unterliste zusammenhängende Einträge entfernt. Wenn die Entfernung in einer leeren Liste führt, wird das Ergebnis ist `null`. Zum Beispiel:

   ```csharp
   delegate void D(int x);
   
   class C
   {
       public static void M1(int i) { /* ... */ }
       public static void M2(int i) { /* ... */ }
   }

   class Test
   {
       static void Main() { 
           D cd1 = new D(C.M1);
           D cd2 = new D(C.M2);
           D cd3 = cd1 + cd2 + cd2 + cd1;   // M1 + M2 + M2 + M1
           cd3 -= cd1;                      // => M1 + M2 + M2
   
           cd3 = cd1 + cd2 + cd2 + cd1;     // M1 + M2 + M2 + M1
           cd3 -= cd1 + cd2;                // => M2 + M1
   
           cd3 = cd1 + cd2 + cd2 + cd1;     // M1 + M2 + M2 + M1
           cd3 -= cd2 + cd2;                // => M1 + M1
   
           cd3 = cd1 + cd2 + cd2 + cd1;     // M1 + M2 + M2 + M1
           cd3 -= cd2 + cd1;                // => M1 + M2
   
           cd3 = cd1 + cd2 + cd2 + cd1;     // M1 + M2 + M2 + M1
           cd3 -= cd1 + cd1;                // => M1 + M2 + M2 + M1
       }
   }
   ```

## <a name="shift-operators"></a>Schiebeoperatoren

Die `<<` und `>>` Operatoren werden verwendet, um wechselnden Bit-Vorgänge ausführen.

```antlr
shift_expression
    : additive_expression
    | shift_expression '<<' additive_expression
    | shift_expression right_shift additive_expression
    ;
```

Wenn ein Operand vom eine *Shift_expression* weist den Typ der Kompilierzeit `dynamic`, und klicken Sie dann der Ausdruck dynamisch gebunden ist ([dynamische Bindung](expressions.md#dynamic-binding)). In diesem Fall den Kompilierzeittyp des Ausdrucks ist `dynamic`, und die unten beschriebene Lösung wird zur Laufzeit mit dem Laufzeit-Typ, der diese Operanden mit der Kompilierzeittyp `dynamic`.

Für einen Vorgang des Formulars `x << count` oder `x >> count`, binärer Operator der überladungsauflösung ([binärer Operator überladungsauflösung](expressions.md#binary-operator-overload-resolution)) wird angewendet, um einen bestimmten Operator-Implementierung auswählen. Die Operanden werden in die Parametertypen des ausgewählten Operator konvertiert, und der Typ des Ergebnisses ist der Rückgabetyp des Operators.

Beim Deklarieren eines überladenen Schiebeoperators der Typ des ersten Operanden muss immer der Klasse oder Struktur, die die Operatordeklaration enthält, und der Typ des zweiten Operanden muss immer `int`.

Die vordefinierten Shift-Operatoren sind unten aufgeführt.

*  Shift-Left:

   ```csharp
   int operator <<(int x, int count);
   uint operator <<(uint x, int count);
   long operator <<(long x, int count);
   ulong operator <<(ulong x, int count);
   ```

   Die `<<` verschiebt der Operator `x` um eine Anzahl von Bits nach links berechnet werden, wie unten beschrieben.

   Die höherwertigen Bits außerhalb des Bereichs des Ergebnistyps der `x` werden verworfen, die verbleibenden Bits nach links verschoben, und die niederwertigen leeren Bitpositionen werden auf 0 (null) festgelegt.

*  Verschiebung nach rechts:

   ```csharp
   int operator >>(int x, int count);
   uint operator >>(uint x, int count);
   long operator >>(long x, int count);
   ulong operator >>(ulong x, int count);
   ```

   Die `>>` verschiebt der Operator `x` rechts um eine Anzahl von Bits berechnet, wie unten beschrieben.

   Wenn `x` ist vom Typ `int` oder `long`, die niederwertigen Bits von `x` werden verworfen, die verbleibenden Bits nach rechts verschoben werden, und die höherwertige leere Bitpositionen werden auf NULL, wenn festgelegt `x` ist negativ und auf, wenn ein `x` ist negativ.

   Wenn `x` ist vom Typ `uint` oder `ulong`, die niederwertigen Bits von `x` werden verworfen, die verbleibenden Bits werden nach rechts verschoben, und die höherwertige leere Bitpositionen werden auf 0 (null) festgelegt.

Die Anzahl der zu verschiebenden Bits wird in der für die vordefinierten Operatoren wird wie folgt berechnet:

*  Wenn der Typ des `x` ist `int` oder `uint`, die Umschaltanzahl durch die niederwertigen fünf Bits des `count`. Das heißt, wird die Umschaltanzahl aus berechnet `count & 0x1F`.
*  Wenn der Typ des `x` ist `long` oder `ulong`, die Umschaltanzahl durch die niederwertigen sechs Bits des `count`. Das heißt, wird die Umschaltanzahl aus berechnet `count & 0x3F`.

Ist die UMSCHALT-Rekursionsergebnis 0 (null), die Schiebeoperatoren einfach den Wert des zurückgeben `x`.

Verschiebevorgänge nie lösen Überläufe und erzeugen identische Ergebnisse im `checked` und `unchecked` Kontexte.

Wenn der linke Operand des der `>>` Operator einen ganzzahligen Typ mit Vorzeichen, der Operator führt eine arithmetische Verschiebung nach rechts in dem der Wert, der das höchstwertige Bit (das signierte Bit) des Operanden weitergegeben wird, das höherwertige leere Bitpositionen. Wenn der linke Operand des der `>>` Operator ein Integraltyp ohne Vorzeichen, der Operator führt eine logische Verschiebung nach rechts in der höherwertige leere Bitpositionen immer auf 0 (null) festgelegt werden. Um den umgekehrten Vorgang aus, die von der Operandentyp abgeleitet auszuführen, können die explizite Umwandlungen verwendet werden. Z. B. wenn `x` ist eine Variable vom Typ `int`, den Vorgang `unchecked((int)((uint)x >> y))` führt eine logische Verschiebung rechts `x`.

## <a name="relational-and-type-testing-operators"></a>Relationale und Typtestoperatoren

Die `==`, `!=`, `<`, `>`, `<=`, `>=`, `is` und `as` Operatoren werden als relationalen und Typtest Operatoren bezeichnet.

```antlr
relational_expression
    : shift_expression
    | relational_expression '<' shift_expression
    | relational_expression '>' shift_expression
    | relational_expression '<=' shift_expression
    | relational_expression '>=' shift_expression
    | relational_expression 'is' type
    | relational_expression 'as' type
    ;

equality_expression
    : relational_expression
    | equality_expression '==' relational_expression
    | equality_expression '!=' relational_expression
    ;
```

Die `is` Operator finden Sie im [der is-Operator](expressions.md#the-is-operator) und `as` Operator finden Sie im [den Operator](expressions.md#the-as-operator).

Die `==`, `!=`, `<`, `>`, `<=` und `>=` Operatoren sind ***Vergleichsoperatoren***.

Wenn ein Operand eines Vergleichsoperators der Kompilierzeit-Typ hat `dynamic`, und klicken Sie dann der Ausdruck dynamisch gebunden ist ([dynamische Bindung](expressions.md#dynamic-binding)). In diesem Fall den Kompilierzeittyp des Ausdrucks ist `dynamic`, und die unten beschriebene Lösung wird zur Laufzeit mit dem Laufzeit-Typ, der diese Operanden mit der Kompilierzeittyp `dynamic`.

Für einen Vorgang des Formulars `x` *Op* `y`, wobei *Op* ein Vergleichsoperator, Auflösung von funktionsüberladungen ([binärer Operator überladungsauflösung](expressions.md#binary-operator-overload-resolution)) wird angewendet, um einen bestimmten Operator-Implementierung auswählen. Die Operanden werden in die Parametertypen des ausgewählten Operator konvertiert, und der Typ des Ergebnisses ist der Rückgabetyp des Operators.

Die vordefinierten Vergleichsoperatoren werden in den folgenden Abschnitten beschrieben. Alle vordefinierten Vergleichsoperatoren zurückgeben ein Ergebnisses vom Typ `bool`, wie in der folgenden Tabelle beschrieben.


| __Vorgang__ | __Ergebnis__                                                       |
|---------------|------------------------------------------------------------------|
| `x == y`      | `true` Wenn `x` gleich `y`, `false` andernfalls                 | 
| `x != y`      | `true` Wenn `x` ist nicht gleich `y`, `false` andernfalls             | 
| `x < y`       | `true` Wenn `x` ist kleiner als `y`, `false` andernfalls                | 
| `x > y`       | `true` Wenn `x` ist größer als `y`, `false` andernfalls             | 
| `x <= y`      | `true` Wenn `x` ist kleiner als oder gleich `y`, `false` andernfalls    | 
| `x >= y`      | `true` Wenn `x` ist größer als oder gleich `y`, `false` andernfalls | 

### <a name="integer-comparison-operators"></a>Integer-Vergleichsoperatoren

Die vordefinierten Integer-Vergleichsoperatoren sind:
```csharp
bool operator ==(int x, int y);
bool operator ==(uint x, uint y);
bool operator ==(long x, long y);
bool operator ==(ulong x, ulong y);

bool operator !=(int x, int y);
bool operator !=(uint x, uint y);
bool operator !=(long x, long y);
bool operator !=(ulong x, ulong y);

bool operator <(int x, int y);
bool operator <(uint x, uint y);
bool operator <(long x, long y);
bool operator <(ulong x, ulong y);

bool operator >(int x, int y);
bool operator >(uint x, uint y);
bool operator >(long x, long y);
bool operator >(ulong x, ulong y);

bool operator <=(int x, int y);
bool operator <=(uint x, uint y);
bool operator <=(long x, long y);
bool operator <=(ulong x, ulong y);

bool operator >=(int x, int y);
bool operator >=(uint x, uint y);
bool operator >=(long x, long y);
bool operator >=(ulong x, ulong y);
```

Die einzelnen Operatoren vergleicht die numerischen Werte der zwei ganzzahlige Operanden und gibt eine `bool` Wert, der angibt, ob die jeweilige Beziehung ist `true` oder `false`.

### <a name="floating-point-comparison-operators"></a>Gleitkomma-Vergleichsoperatoren

Die vordefinierten Gleitkomma-Vergleichsoperatoren sind:
```csharp
bool operator ==(float x, float y);
bool operator ==(double x, double y);

bool operator !=(float x, float y);
bool operator !=(double x, double y);

bool operator <(float x, float y);
bool operator <(double x, double y);

bool operator >(float x, float y);
bool operator >(double x, double y);

bool operator <=(float x, float y);
bool operator <=(double x, double y);

bool operator >=(float x, float y);
bool operator >=(double x, double y);
```

Die Operatoren vergleichen die Operanden gemäß den Regeln der IEEE 754-standard:

*  Wenn einer der Operanden NaN ist, wird das Ergebnis ist `false` für alle Operatoren außer `!=`, für die das Ergebnis ist `true`. Für alle zwei Operanden `x != y` erzeugt immer das gleiche Ergebnis wie `!(x == y)`. Wenn einer oder beide Operanden sind jedoch NaN ist, die `<`, `>`, `<=`, und `>=` Operatoren erzeugen die gleichen Ergebnisse wie die logische Negation des den entgegengesetzten Operator nicht. Z. B., wenn entweder der `x` und `y` ist NaN, `x < y` ist `false`, aber `!(x >= y)` ist `true`.
*  Wenn keiner der Operanden NaN ist, vergleichen Sie die Operatoren die Werte von zwei Gleitkommazahlen Operanden in Bezug auf die Reihenfolge

   ```
   -inf < -max < ... < -min < -0.0 == +0.0 < +min < ... < +max < +inf
   ```

   wo `min` und `max` sind die kleinstmöglichen und größtmöglichen positiven endlichen Werten, die in der angegebenen Gleitkomma-Format dargestellt werden können. Diese Sortierung wesentliche Auswirkungen sind:
   * Negative und positive Nullen werden als gleich betrachtet.
   * Eine negative Unendlichkeit gilt als kleiner als alle anderen Werte, aber gleich einem anderen minus unendlich.
   * Eine positive Unendlichkeit gilt als größer als alle anderen Werte, aber gleich einem anderen plus unendlich.

### <a name="decimal-comparison-operators"></a>Decimal-Vergleichsoperatoren

Die vordefinierten decimal Vergleichsoperatoren sind:
```csharp
bool operator ==(decimal x, decimal y);
bool operator !=(decimal x, decimal y);
bool operator <(decimal x, decimal y);
bool operator >(decimal x, decimal y);
bool operator <=(decimal x, decimal y);
bool operator >=(decimal x, decimal y);
```

Die einzelnen Operatoren vergleicht die numerischen Werte der beiden decimal Operanden und gibt eine `bool` Wert, der angibt, ob die jeweilige Beziehung ist `true` oder `false`. Jeder dezimale Vergleich ist äquivalent zur Verwendung des entsprechenden relationalen oder Equality-Operator vom Typ `System.Decimal`.

### <a name="boolean-equality-operators"></a>Boolesche Operatoren

Die vordefinierten booleschen Operatoren sind:
```csharp
bool operator ==(bool x, bool y);
bool operator !=(bool x, bool y);
```

Das Ergebnis des `==` ist `true` sowohl `x` und `y` sind `true` oder wenn beide `x` und `y` sind `false`. Andernfalls ist das Ergebnis `false`.

Das Ergebnis des `!=` ist `false` sowohl `x` und `y` sind `true` oder wenn beide `x` und `y` sind `false`. Andernfalls ist das Ergebnis `true`. Wenn die Operanden sind vom Typ `bool`, `!=` Operator erzeugt das gleiche Ergebnis wie die `^` Operator.

### <a name="enumeration-comparison-operators"></a>Enumeration-Vergleichsoperatoren

Jeder Enumerationstyp bietet implizit die folgenden vordefinierten Vergleichsoperatoren:
```csharp
bool operator ==(E x, E y);
bool operator !=(E x, E y);
bool operator <(E x, E y);
bool operator >(E x, E y);
bool operator <=(E x, E y);
bool operator >=(E x, E y);
```

Das Ergebnis der Auswertung `x op y`, wobei `x` und `y` sind Ausdrücke eines Enumerationstyps `E` mit einem zugrunde liegenden Typ `U`, und `op` ist eine der folgenden Vergleichsoperatoren, entspricht genau dem Auswerten von `((U)x) op ((U)y)`. Das heißt, vergleichen die Enumeration-Typ-Vergleichsoperatoren einfach die zugrunde liegenden ganzzahligen Werte der beiden Operanden.

### <a name="reference-type-equality-operators"></a>Gleichheitsoperatoren

Die Gleichheitsoperatoren vordefinierten Typ sind:
```csharp
bool operator ==(object x, object y);
bool operator !=(object x, object y);
```

Die Operatoren geben das Ergebnis des Vergleichs die beiden Verweise auf Gleichheit oder wertungleichheit zurück.

Da die vordefinierten Typ Gleichheitsoperatoren Operanden vom Typ akzeptieren `object`, gelten für alle Typen, die keinen entsprechenden deklarieren `operator ==` und `operator !=` Member. Im Gegensatz dazu ausblenden, alle anwendbaren benutzerdefinierte Operatoren effektiv die Gleichheitsoperatoren vordefinierten Typ.

Die Gleichheitsoperatoren vordefinierten Typ erfordert einen der folgenden:

*  Beide Operanden sind, einen Wert eines Typs, die bekanntermaßen eine *Reference_type* oder das Literal `null`. Darüber hinaus eine explizite Konvertierung ([explizite Konvertierungen](conversions.md#explicit-reference-conversions)) aus dem Typ der Operanden in den Typ des anderen Operanden vorhanden.
*  Einer der Operanden ist ein Wert vom Typ `T` , in denen `T` ist eine *Type_parameter* und der andere Operand ist das Literal `null`. Außerdem `T` verfügt nicht über die werteinschränkung-Typ.

Wenn eine der folgenden Bedingungen erfüllt sind, tritt ein Fehler während der Bindung. Wesentliche Auswirkungen auf die diese Regeln sind:

*  Es ist ein Fehler während der Bindung, die Gleichheitsoperatoren vordefinierten Typ zu verwenden, um zwei Verweise zu vergleichen, die bekannt sind, zum Zeitpunkt der Bindung unterschiedlich sein. Wenn die Bindung-Time-Typen der Operanden zwei Klassentypen sind z. B. `A` und `B`, und wenn weder `A` noch `B` von der anderen abgeleitet ist, und es unmöglich, dass die beiden Operanden wäre auf dasselbe Objekt verweisen. Daher gilt der Vorgang einen Fehler während der Bindung.
*  Die vordefinierten Typ Gleichheitsoperatoren lassen nicht Wert Operanden vom Typ zu vergleichende. Es sei denn, eine eigene Operatoren ein Strukturtyp deklariert wird, ist es daher nicht möglich, zum Vergleichen von Werten dieses Strukturtyps.
*  Die vordefinierten Typ Gleichheitsoperatoren verursachen nie Boxing-Vorgänge für ihre Operanden sicher auftreten. Es wäre ohne Bedeutung für solche Boxing-Vorgänge ausführen, da Verweise auf das neu zugeordneten geschachtelten Instanzen unbedingt von allen anderen verweisen voneinander abweichen.
*  Wenn ein Operand vom Typ Parametertyp `T` im Vergleich zu `null`, und welche Laufzeit `T` ein Werttyp ist, ist das Ergebnis des Vergleichs `false`.

Das folgende Beispiel überprüft, ob ein Argument des Typs einer beschränkten Typ-Parameter ist `null`.
```csharp
class C<T>
{
    void F(T x) {
        if (x == null) throw new ArgumentNullException();
        ...
    }
}
```

Die `x == null` Konstrukt ist zulässig, obwohl `T` könnte einen Werttyp darstellen, und das Ergebnis wird lediglich definiert `false` beim `T` ein Werttyp ist.

Für einen Vorgang des Formulars `x == y` oder `x != y`ggf. alle `operator ==` oder `operator !=` vorhanden ist, handelt es sich bei der überladungsauflösung für Operator ([binärer Operator überladungsauflösung](expressions.md#binary-operator-overload-resolution)) Regeln auswählen, werden Operator anstelle der vordefinierten Gleichheitsoperator. Es ist jedoch immer möglich, wählen Sie die vordefinierten Gleichheitsoperator, indem Sie explizit umwandeln, eine oder beide der Operanden in den Typ `object`. Im Beispiel
```csharp
using System;

class Test
{
    static void Main() {
        string s = "Test";
        string t = string.Copy(s);
        Console.WriteLine(s == t);
        Console.WriteLine((object)s == t);
        Console.WriteLine(s == (object)t);
        Console.WriteLine((object)s == (object)t);
    }
}
```
erzeugt die Ausgabe
```
True
False
False
False
```

Die `s` und `t` Variablen verweisen auf zwei unterschiedliche `string` Instanzen, die dieselben Zeichen enthält. Der erste Vergleich gibt `True` da die vordefinierte Zeichenfolge Equality-Operator ([Zeichenfolge Gleichheitsoperatoren](expressions.md#string-equality-operators)) wird aktiviert, wenn beide Operanden vom Typ sind `string`. Die verbleibenden Vergleiche, die alle Ausgabe `False` , da die vordefinierte Gleichheitsoperator ausgewählt ist, wenn eine oder beide der Operanden des Typs sind `object`.

Beachten Sie, dass die oben genannten Technik nicht für Werttypen von Bedeutung ist. Im Beispiel
```csharp
class Test
{
    static void Main() {
        int i = 123;
        int j = 123;
        System.Console.WriteLine((object)i == (object)j);
    }
}
```
Gibt `False` geschachtelt, da die Umwandlungen Verweise auf zwei separate Instanzen des erstellen `int` Werte.

### <a name="string-equality-operators"></a>Zeichenfolge-Gleichheitsoperatoren

Die Gleichheitsoperatoren vordefinierte Zeichenfolge sind:
```csharp
bool operator ==(string x, string y);
bool operator !=(string x, string y);
```

Zwei `string` Werte werden als gleich betrachtet, wenn eine der folgenden Bedingungen zutrifft:

*  Beide Werte sind `null`.
*  Beide Werte sind nicht-Null-Verweise auf String-Instanzen, die dieselbe Länge haben und die identische Zeichen in jeder Position des Zeichens.

Die Gleichheitsoperatoren vergleichen Zeichenfolgenwerte anstelle der Zeichenfolge verweisen. Wenn zwei unterschiedliche Instanzen genaue dieselbe Sequenz von Zeichen enthalten, die Werte der Zeichenfolgen gleich sind, aber die Verweise unterscheiden. Siehe [Gleichheitsoperatoren](expressions.md#reference-type-equality-operators), die Gleichheitsoperatoren können verwendet werden, um Verweise auf Zeichenfolgen anstelle von String-Werten verglichen werden soll.

### <a name="delegate-equality-operators"></a>Delegieren von Gleichheitsoperatoren

Alle Delegattypen bietet implizit die folgenden vordefinierten Vergleichsoperatoren:

```csharp
bool operator ==(System.Delegate x, System.Delegate y);
bool operator !=(System.Delegate x, System.Delegate y);
```

Zwei Delegatinstanzen wie folgt gelten als gleich:

*  Wenn eine Delegatinstanz `null`, sie gleich sind, wenn beide `null`.
*  Wenn die Delegaten unterschiedliche Laufzeit-Typinformationen, die sie nicht gleich sind haben.
*  Wenn beide Delegatinstanzen eine Aufrufliste haben ([delegieren Deklarationen](delegates.md#delegate-declarations)), die Instanzen gleich sind, wenn ihre Aufruflisten die gleiche Länge sind, und jeder Eintrag in der Aufrufliste gleich ist (wie unten definiert) mit dem entsprechenden Eintrag in der Reihenfolge, in die andere Aufrufliste.

Die folgenden Regeln bestimmen die Gleichheit von Listeneinträgen Aufruf:

*  Wenn zwei Aufruf Listeneinträge sowohl auf die gleiche statische finden Sie unter sind Methode und dann die Einträge gleich.
*  Wenn zwei Aufruf Listeneinträge beide auf dieselbe nicht statische Methode auf dem gleichen Zielobjekt verweisen (wie durch die Gleichheitsoperatoren definiert) klicken Sie dann sind die Einträge gleich.
*  Aufruf Einträge erstellt, bei der Auswertung des semantisch identisch *Anonymous_method_expression*s oder *Lambda_expression*mit den gleichen Satz erfassten äußere Variable (möglicherweise leere) Instanzen sind zulässig (jedoch nicht erforderlich) gleich sind.

### <a name="equality-operators-and-null"></a>Operatoren und null

Die `==` und `!=` Operatoren ermöglichen einer der Operanden ein als Wert für einen nullable-Typ und die andere, werden die `null` literal, selbst wenn keine vordefinierte oder benutzerdefinierte-Operator (in unlifted oder Formular aufgehoben) für den Vorgang vorhanden ist.

Für einen Vorgang von einem der Formate
```csharp
x == null
null == x
x != null
null != x
```
in denen `x` ist ein Ausdruck, der einen nullable-Typ, wenn überladungsauflösung für Operator ([binärer Operator überladungsauflösung](expressions.md#binary-operator-overload-resolution)) ein Fehler auftritt, finden Sie einen entsprechenden Operator das Ergebnis wird stattdessen aus berechnet die `HasValue` Eigenschaft des `x`. Insbesondere werden die ersten beiden Formulare in übersetzt `!x.HasValue`, und die letzten zwei Formen werden in übersetzt `x.HasValue`.

### <a name="the-is-operator"></a>Der is-Operator

Die `is` Operator wird verwendet, um dynamisch zu überprüfen, ob der Laufzeittyp eines Objekts mit einem angegebenen Typ kompatibel ist. Das Ergebnis des Vorgangs `E is T`, wobei `E` ist ein Ausdruck und `T` ist ein Typ, ist ein boolescher Wert, der angibt, der Wert, ob `E` erfolgreich Typ konvertiert werden kann `T` von einer verweiskonvertierung, einer Boxing Konvertierung oder einer unboxing-Konvertierung. Die Operation wird wie folgt ausgewertet, nachdem für alle Typparameter Typargumente ersetzt wurden:

*  Wenn `E` wird eine anonyme Funktion, ein Fehler während der Kompilierung tritt ein,
*  Wenn `E` ist eine Methodengruppe oder `null` Zeichenliteral zurück, wenn der Typ des `E` ist ein Verweistyp oder einem nullable-Typ und den Wert der `E` ist null das Ergebnis ist "false".
*  Andernfalls können `D` darstellen den dynamischen Typ der `E` wie folgt:
   * Wenn der Typ des `E` ist ein Verweistyp, `D` ist der Run-Time-Typ, der den Instanzverweis von `E`.
   * Wenn der Typ des `E` ist ein NULL-Werte zulässt, `D` ist der zugrunde liegende Typ dieses nullable-Typ.
   * Wenn der Typ des `E` ist ein NULL-Werttyp, `D` ist der Typ des `E`.
*  Das Ergebnis des Vorgangs hängt `D` und `T` wie folgt:
   * Wenn `T` ein Verweistyp ist das Ergebnis ist "true" Wenn `D` und `T` desselben Typs sind, wenn `D` ist ein Verweistyp und eine implizite verweiskonvertierung von `D` zu `T` vorhanden ist, oder wenn `D` ist ein Werttyp und eine Boxingkonvertierung von `D` zu `T` vorhanden ist.
   * Wenn `T` ein nullable-Typ ist das Ergebnis ist "true" Wenn `D` ist der zugrunde liegenden Typ des `T`.
   * Wenn `T` ist ein NULL-Werttyp, das Ergebnis ist True, wenn `D` und `T` denselben Typ aufweisen.
   * Das Ergebnis ist, andernfalls "false".

Beachten Sie, dass benutzerdefinierte Konvertierungen werden nicht berücksichtigt, indem die `is` Operator.

### <a name="the-as-operator"></a>Den Operator

Die `as` Operator wird verwendet, um explizit einen Wert in einen angegebenen Referenztyp oder ein nullable-Typ zu konvertieren. Im Gegensatz zu Cast-Ausdruck ([Umwandlungsausdrücke](expressions.md#cast-expressions)), wird die `as` Operator niemals eine Ausnahme auslöst. Stattdessen ist die angegebene Konvertierung nicht möglich, die der resultierende Wert ist `null`.

In einem Vorgang des Formulars `E as T`, `E` muss ein Ausdruck sein und `T` muss ein Verweistyp, ein Typparameter ein Verweistyp oder einem nullable-Typ bezeichnet. Darüber hinaus muss mindestens eine der folgenden "true" sein, oder andernfalls ein Kompilierungsfehler tritt auf:

*  Eine Identität ([identitätskonvertierung](conversions.md#identity-conversion)), implizite NULL-Werte zulässt ([implizite NULL-Werte zulassen Konvertierungen](conversions.md#implicit-nullable-conversions)), ein impliziter Verweis ([implizite Verweis-](conversions.md#implicit-reference-conversions)), Boxing ([ Boxing-Konvertierung](conversions.md#boxing-conversions)), explizite NULL-Werte zulässt ([explizite NULL-Werte zulassen Konvertierungen](conversions.md#explicit-nullable-conversions)), expliziten Verweis ([explizite Konvertierungen](conversions.md#explicit-reference-conversions)), oder durch unboxing ([Unboxing-Konvertierungen](conversions.md#unboxing-conversions)) Konvertierung vorhanden ist, von `E` zu `T`.
*  Der Typ des `E` oder `T` ist ein offener Typ.
*  `E` ist die `null` literal.

Wenn die Kompilierung von eingeben `E` nicht `dynamic`, den Vorgang `E as T` führt zum gleiche Ergebnis wie
```csharp
E is T ? (T)(E) : (T)null
```
außer dass `E` nur einmal überprüft wird. Der Compiler zur Optimierung erwarten `E as T` höchstens einen Typ "dynamic" Überprüfung statt zwei dynamischen Typprüfungen impliziert durch die Erweiterung, die oben genannten ausführen.

Wenn die Kompilierung von eingeben `E` ist `dynamic`, anders als bei der Cast-Operator der `as` Operator ist nicht dynamisch gebunden ([dynamische Bindung](expressions.md#dynamic-binding)). Aus diesem Grund ist die Erweiterung in diesem Fall ein:
```csharp
E is T ? (T)(object)(E) : (T)null
```

Beachten Sie, dass einige Konvertierungen wie die benutzerdefinierte Konvertierungen nicht zulässig sind die `as` Operator und stattdessen mithilfe der Cast-Ausdrücke ausgeführt werden soll.

Im Beispiel
```csharp
class X
{

    public string F(object o) {
        return o as string;        // OK, string is a reference type
    }

    public T G<T>(object o) where T: Attribute {
        return o as T;             // Ok, T has a class constraint
    }

    public U H<U>(object o) {
        return o as U;             // Error, U is unconstrained 
    }
}
```
der Typparameter `T` von `G` wird ein Verweistyp sein bezeichnet, da sie die klasseneinschränkung verfügt. Der Typparameter `U` von `H` ist jedoch; nicht daher die Verwendung von der `as` -Operator in `H` ist nicht zulässig.

## <a name="logical-operators"></a>Logische Operatoren

Die `&`, `^`, und `|` Operatoren werden als die logischen Operatoren bezeichnet.

```antlr
and_expression
    : equality_expression
    | and_expression '&' equality_expression
    ;

exclusive_or_expression
    : and_expression
    | exclusive_or_expression '^' and_expression
    ;

inclusive_or_expression
    : exclusive_or_expression
    | inclusive_or_expression '|' exclusive_or_expression
    ;
```

Wenn ein Operand vom ein logischer Operator, der während der Kompilierung-Typ hat `dynamic`, und klicken Sie dann der Ausdruck dynamisch gebunden ist ([dynamische Bindung](expressions.md#dynamic-binding)). In diesem Fall den Kompilierzeittyp des Ausdrucks ist `dynamic`, und die unten beschriebene Lösung wird zur Laufzeit mit dem Laufzeit-Typ, der diese Operanden mit der Kompilierzeittyp `dynamic`.

Für einen Vorgang des Formulars `x op y`, wobei `op` ist eine logische Operatoren, Auflösung von funktionsüberladungen ([binärer Operator überladungsauflösung](expressions.md#binary-operator-overload-resolution)) wird angewendet, um einen bestimmten Operator-Implementierung auswählen. Die Operanden werden in die Parametertypen des ausgewählten Operator konvertiert, und der Typ des Ergebnisses ist der Rückgabetyp des Operators.

Die vordefinierten logische Operatoren werden in den folgenden Abschnitten beschrieben.

### <a name="integer-logical-operators"></a>Logische Operatoren für ganze Zahlen

Die vordefinierten Ganzzahl logischen Operatoren sind:
```csharp
int operator &(int x, int y);
uint operator &(uint x, uint y);
long operator &(long x, long y);
ulong operator &(ulong x, ulong y);

int operator |(int x, int y);
uint operator |(uint x, uint y);
long operator |(long x, long y);
ulong operator |(ulong x, ulong y);

int operator ^(int x, int y);
uint operator ^(uint x, uint y);
long operator ^(long x, long y);
ulong operator ^(ulong x, ulong y);
```

Die `&` -Operator berechnet das bitweise logische `AND` der zwei Operanden, die `|` -Operator berechnet das bitweise logische `OR` der zwei Operanden, und die `^` -Operator berechnet das bitweise logische XOR `OR` der beiden Operanden. Keine Überläufe sind von diesen Vorgängen möglich.

### <a name="enumeration-logical-operators"></a>Logische Operatoren Enumeration

Jeder Enumerationstyp `E` implizit bietet die folgenden vordefinierten logische Operatoren:

```csharp
E operator &(E x, E y);
E operator |(E x, E y);
E operator ^(E x, E y);
```

Das Ergebnis der Auswertung `x op y`, wobei `x` und `y` sind Ausdrücke eines Enumerationstyps `E` mit einem zugrunde liegenden Typ `U`, und `op` ist einer der logischen Operatoren, entspricht genau dem Auswerten von `(E)((U)x op (U)y)`. Das heißt, führen Sie die logischen Operatoren für die Enumeration einfach die logische Operation mit den zugrunde liegenden Typ, der zwei Operanden.

### <a name="boolean-logical-operators"></a>Boolesche logische Operatoren

Die vordefinierten booleschen logischen Operatoren sind:
```csharp
bool operator &(bool x, bool y);
bool operator |(bool x, bool y);
bool operator ^(bool x, bool y);
```

Das Ergebnis von `x & y` ist `true`, wenn sowohl `x` als auch `y` zu `true` ausgewertet werden. Andernfalls ist das Ergebnis `false`.

Das Ergebnis des `x | y` ist `true` Wenn `x` oder `y` ist `true`. Andernfalls ist das Ergebnis `false`.

Das Ergebnis des `x ^ y` ist `true` Wenn `x` ist `true` und `y` ist `false`, oder `x` ist `false` und `y` ist `true`. Andernfalls ist das Ergebnis `false`. Wenn die Operanden sind vom Typ `bool`, `^` -Operator berechnet das gleiche Ergebnis wie die `!=` Operator.

### <a name="nullable-boolean-logical-operators"></a>Auf NULL festlegbaren booleschen logischen Operatoren

Der auf NULL festlegbaren booleschen Typ `bool?` kann drei Werte darstellen `true`, `false`, und `null`, und ist konzeptionell identisch mit den drei Werten-Typ für boolesche Ausdrücke, die in SQL verwendet. Um sicherzustellen, dass die Ergebnisse von den Ergebnissen der `&` und `|` Operatoren für `bool?` Operanden sind konsistent mit der SQL dreiwertige Logik, die folgenden vordefinierten Operatoren werden bereitgestellt:

```csharp
bool? operator &(bool? x, bool? y);
bool? operator |(bool? x, bool? y);
```

Die folgende Tabelle enthält die Ergebnisse dieser Operatoren für den Werten aller Kombinationen von `true`, `false`, und `null`.

| `x`     | `y`     | `x & y` | <code>x &#124; y</code> |
|:-------:|:-------:|:-------:|:-------:|
| `true`  | `true`  | `true`  | `true`  | 
| `true`  | `false` | `false` | `true`  | 
| `true`  | `null`  | `null`  | `true`  | 
| `false` | `true`  | `false` | `true`  | 
| `false` | `false` | `false` | `false` | 
| `false` | `null`  | `false` | `null`  | 
| `null`  | `true`  | `null`  | `true`  | 
| `null`  | `false` | `false` | `null`  | 
| `null`  | `null`  | `null`  | `null`  | 

## <a name="conditional-logical-operators"></a>Bedingte logische Operatoren

Die `&&` und `||` Operatoren werden als die bedingten logischen Operatoren bezeichnet. Sie werden auch die "verkürzte" logischen Operatoren bezeichnet.

```antlr
conditional_and_expression
    : inclusive_or_expression
    | conditional_and_expression '&&' inclusive_or_expression
    ;

conditional_or_expression
    : conditional_and_expression
    | conditional_or_expression '||' conditional_and_expression
    ;
```

Die `&&` und `||` Operatoren sind bedingte Versionen der `&` und `|` Operatoren:

*  Der Vorgang `x && y` entspricht dem Vorgang `x & y`, außer dass `y` nur ausgewertet, wenn `x` nicht `false`.
*  Der Vorgang `x || y` entspricht dem Vorgang `x | y`, außer dass `y` nur ausgewertet, wenn `x` nicht `true`.

Wenn ein Operand eines bedingten logischen Operators der Kompilierzeit-Typ hat `dynamic`, und klicken Sie dann der Ausdruck dynamisch gebunden ist ([dynamische Bindung](expressions.md#dynamic-binding)). In diesem Fall den Kompilierzeittyp des Ausdrucks ist `dynamic`, und die unten beschriebene Lösung wird zur Laufzeit mit dem Laufzeit-Typ, der diese Operanden mit der Kompilierzeittyp `dynamic`.

Ein Vorgang des Formulars `x && y` oder `x || y` wird durch Anwenden der Auflösung von funktionsüberladungen verarbeitet ([binärer Operator überladungsauflösung](expressions.md#binary-operator-overload-resolution)), als ob der Vorgang geschrieben wurde `x & y` oder `x | y`. Klicken Sie dann

*  Wenn Auflösung von funktionsüberladungen finden einen einzelnen bewährte Operator oder wählt die überladungsauflösung eine der vordefinierten Ganzzahl logischen Operatoren, tritt ein Fehler während der Bindung.
*  Andernfalls, wenn der ausgewählte Operator, eine der vordefinierten booleschen logische Operatoren ist ([Boolesche logische Operatoren](expressions.md#boolean-logical-operators)) oder auf NULL festlegbaren booleschen logischen Operatoren ([auf NULL festlegbaren booleschen logischen Operatoren](expressions.md#nullable-boolean-logical-operators)), wird die Vorgang wird verarbeitet, wie in beschrieben [booleschen bedingten logischen Operatoren](expressions.md#boolean-conditional-logical-operators).
*  Andernfalls der ausgewählte Operator ist ein benutzerdefinierter Operator und der Vorgang verarbeitet wird, wie in beschrieben [bedingten logischen Operatoren eine benutzerdefinierte](expressions.md#user-defined-conditional-logical-operators).

Es ist nicht möglich, direkt die bedingten logischen Operatoren überladen. Aber da die bedingten logischen Operatoren in Bezug auf die regulären logischen Operatoren ausgewertet werden, gelten Überladungen der regulären logischen Operatoren mit gewissen Einschränkungen auch Überladungen der bedingten logischen Operatoren als. Dies wird weiter in [bedingten logischen Operatoren eine benutzerdefinierte](expressions.md#user-defined-conditional-logical-operators).

### <a name="boolean-conditional-logical-operators"></a>Boolesche bedingte logische Operatoren

Wenn die Operanden des `&&` oder `||` sind vom Typ `bool`, oder wenn die Operanden sind Typen, die keinem zutreffenden definieren `operator &` oder `operator |`, aber implizite Konvertierungen in definieren `bool`, der Vorgang ist wie folgt verarbeitet:

*  Der Vorgang `x && y` wird als ausgewertet, `x ? y : false`. Das heißt, `x` wird zuerst ausgewertet und in den Typ konvertiert `bool`. Wenn danach `x` ist `true`, `y` ausgewertet und in den Typ konvertiert `bool`, und dies ist das Ergebnis des Vorgangs. Andernfalls ist das Ergebnis des Vorgangs `false`.
*  Der Vorgang `x || y` wird als ausgewertet, `x ? true : y`. Das heißt, `x` wird zuerst ausgewertet und in den Typ konvertiert `bool`. Wenn danach `x` ist `true`, ist das Ergebnis des Vorgangs `true`. Andernfalls `y` ausgewertet und in den Typ konvertiert `bool`, und dies ist das Ergebnis des Vorgangs.

### <a name="user-defined-conditional-logical-operators"></a>Eine benutzerdefinierte bedingten logischen Operatoren

Wenn die Operanden des `&&` oder `||` sind Typen, die dem zutreffenden deklarieren Sie eine benutzerdefinierte `operator &` oder `operator |`, die folgenden beiden muss "true", in denen `T` ist der Typ, in dem der ausgewählte Operator deklariert wird:

*  Der Rückgabetyp und den Typ jedes Parameters, der den ausgewählten Operator muss `T`. Das heißt, muss der Operator die logische compute `AND` oder die logische `OR` der beiden Operanden vom Typ `T`, und ein Ergebnis vom Typ zurückgeben müssen `T`.
*  `T` Deklarationen von darf `operator true` und `operator false`.

Ein Fehler während der Bindung tritt auf, wenn der Anforderungen nicht erfüllt wird. Andernfalls die `&&` oder `||` Operation wird ausgewertet, durch die Kombination der benutzerdefinierte `operator true` oder `operator false` mit den ausgewählten benutzerdefinierten Operator:

*  Der Vorgang `x && y` wird als ausgewertet, `T.false(x) ? x : T.&(x, y)`, wobei `T.false(x)` ist ein Aufruf von der `operator false` in deklarierten `T`, und `T.&(x, y)` ist ein Aufruf des ausgewählten `operator &`. Das heißt, `x` wird zuerst ausgewertet und `operator false` wird aufgerufen, auf das Ergebnis aus, um zu bestimmen, ob `x` ist definitiv "false". Wenn danach `x` ist definitiv "false" werden, das Ergebnis des Vorgangs ist der Wert, der zuvor für berechneten `x`. Andernfalls `y` ausgewertet wird, und der ausgewählten `operator &` wird aufgerufen, für die zuvor für `x` und den berechneten Wert für `y` um das Ergebnis des Vorgangs zu erstellen.
*  Der Vorgang `x || y` wird als ausgewertet, `T.true(x) ? x : T.|(x, y)`, wobei `T.true(x)` ist ein Aufruf von der `operator true` in deklarierten `T`, und `T.|(x,y)` ist ein Aufruf des ausgewählten `operator|`. Das heißt, `x` wird zuerst ausgewertet und `operator true` wird aufgerufen, auf das Ergebnis aus, um zu bestimmen, ob `x` auf jeden Fall ist. Wenn danach `x` auf jeden Fall true ist, das Ergebnis des Vorgangs ist der Wert, der zuvor für berechneten `x`. Andernfalls `y` ausgewertet wird, und der ausgewählten `operator |` wird aufgerufen, für die zuvor für `x` und den berechneten Wert für `y` um das Ergebnis des Vorgangs zu erstellen.

In einer dieser Vorgänge, die der angegebene Ausdruck `x` ist nur einmal ausgewertet, und der angegebene Ausdruck `y` wird nicht ausgewertet oder genau einmal ausgewertet.

Ein Beispiel für einen Typ, der implementiert `operator true` und `operator false`, finden Sie unter [Datenbank booleschen Typ](structs.md#database-boolean-type).

## <a name="the-null-coalescing-operator"></a>Die null-Sammeloperator

Die `??` Operator, der null-Sammeloperator aufgerufen wird.

```antlr
null_coalescing_expression
    : conditional_or_expression
    | conditional_or_expression '??' null_coalescing_expression
    ;
```

Ein null-sammelausdrücken im Format `a ?? b` erfordert `a` vom einen Nullable-Typ oder Verweis-Typ sein. Wenn `a` ungleich Null sind, ist das Ergebnis des `a ?? b` ist `a`ist, andernfalls ist das Ergebnis `b`. Der Vorgang ermittelt `b` nur, wenn `a` ist null.

Die null-Sammeloperator ist rechtsassoziativ, was bedeutet, dass Vorgänge, die von rechts nach links gruppiert werden. Z. B. ein Ausdruck der Form `a ?? b ?? c` wird als ausgewertet, `a ?? (b ?? c)`. Im allgemeinen Begriffe, einen Ausdruck der Form `E1 ?? E2 ?? ... ?? En` gibt den ersten Operanden, der ist ungleich Null verwendet werden soll, oder null, wenn alle Operanden null sind.

Der Typ des Ausdrucks `a ?? b` hängt von der impliziten Konvertierungen auf die Operanden verfügbar sind. In der Reihenfolge ihrer Priorität, die den Typ des `a ?? b` ist `A0`, `A`, oder `B`, wobei `A` ist der Typ des `a` (vorausgesetzt, dass `a` verfügt über einen Typ), `B` ist der Typ des `b` () vorausgesetzt, dass `b` verfügt über einen Typ), und `A0` ist der zugrunde liegenden Typ des `A` Wenn `A` ist ein NULL-Werte zulässt, oder `A` andernfalls. Insbesondere `a ?? b` wird wie folgt verarbeitet:

*  Wenn `A` vorhanden ist und keinen nullable-Typ oder ein Verweistyp, ein Fehler während der Kompilierung auftritt.
*  Wenn `b` ein dynamisches Ausdrucks, ist der Ergebnistyp ist `dynamic`. Zur Laufzeit `a` wird zuerst ausgewertet. Wenn `a` ist nicht null, `a` wird konvertiert in einen dynamischen, und dies ist das Ergebnis. Andernfalls `b` ausgewertet wird, und dies ist das Ergebnis.
*  Andernfalls gilt: Wenn `A` vorhanden ist und ein nullable-Typ und eine implizite Konvertierung vorhanden ist aus `b` zu `A0`, der Ergebnistyp ist `A0`. Zur Laufzeit `a` wird zuerst ausgewertet. Wenn `a` ist nicht null, `a` wird entpackt, um geben `A0`, und dies ist das Ergebnis. Andernfalls `b` ausgewertet und in den Typ konvertiert `A0`, und dies ist das Ergebnis.
*  Andernfalls gilt: Wenn `A` vorhanden ist und eine implizite Konvertierung von `b` zu `A`, ist der Ergebnistyp `A`. Zur Laufzeit `a` wird zuerst ausgewertet. Wenn `a` ist nicht null, `a` wird das Ergebnis. Andernfalls `b` ausgewertet und in den Typ konvertiert `A`, und dies ist das Ergebnis.
*  Andernfalls gilt: Wenn `b` weist einen Typ `B` und eine implizite Konvertierung von `a` zu `B`, der Ergebnistyp ist `B`. Zur Laufzeit `a` wird zuerst ausgewertet. Wenn `a` ist nicht null, `a` wird entpackt, um geben `A0` (Wenn `A` vorhanden ist, und NULL-Werte zulässt) und in den Typ konvertiert `B`, und dies ist das Ergebnis. Andernfalls `b` ausgewertet wird und das Ergebnis.
*  Andernfalls `a` und `b` nicht kompatibel sind und ein Fehler während der Kompilierung auftritt.

## <a name="conditional-operator"></a>Bedingter Operator

Die `?:` Operator wird den bedingten Operator aufgerufen. Sie wird gelegentlich auch den ternären Operator aufgerufen.

```antlr
conditional_expression
    : null_coalescing_expression
    | null_coalescing_expression '?' expression ':' expression
    ;
```

Ein bedingter Ausdruck der Form `b ? x : y` zuerst wertet die Bedingung `b`. Wenn danach `b` ist `true`, `x` ausgewertet wird und das Ergebnis des Vorgangs. Andernfalls `y` ausgewertet wird und das Ergebnis des Vorgangs. Ein bedingter Ausdruck ist nie ergibt, beide `x` und `y`.

Der bedingte Operator ist rechtsassoziativ, was bedeutet, dass Vorgänge, die von rechts nach links gruppiert werden. Z. B. ein Ausdruck der Form `a ? b : c ? d : e` wird als ausgewertet, `a ? b : (c ? d : e)`.

Der erste Operand der `?:` Operator muss ein Ausdruck, der implizit in konvertiert werden kann `bool`, oder ein Ausdruck, der ein Typ, der implementiert `operator true`. Wenn keine dieser Anforderungen nicht erfüllt ist, tritt ein Fehler während der Kompilierung.

Die zweiten und dritten Operanden, `x` und `y`, der die `?:` -Operators bestimmen den Typ des bedingten Ausdrucks.

*  Wenn `x` weist den Typ `X` und `y` weist den Typ `Y` dann
   * Wenn eine implizite Konvertierung ([implizite Konvertierungen](conversions.md#implicit-conversions)) vorhanden ist, von `X` zu `Y`, jedoch nicht von `Y` zu `X`, klicken Sie dann `Y` ist der Typ des bedingten Ausdrucks.
   * Wenn eine implizite Konvertierung ([implizite Konvertierungen](conversions.md#implicit-conversions)) vorhanden ist, von `Y` zu `X`, jedoch nicht von `X` zu `Y`, klicken Sie dann `X` ist der Typ des bedingten Ausdrucks.
   * Andernfalls keine Ausdruckstyp bestimmt werden kann, und ein Fehler während der Kompilierung auftritt.
*  Wenn nur eine der `x` und `y` verfügt über einen Typ und beide `x` und `y`, der implizit in diesen Typ konvertiert werden, dann wird der Typ des bedingten Ausdrucks.
*  Andernfalls keine Ausdruckstyp bestimmt werden kann, und ein Fehler während der Kompilierung auftritt.

Die Run-Time-Verarbeitung von einem bedingten Ausdruck der Form `b ? x : y` umfasst die folgenden Schritte aus:

*  Zuerst `b` ausgewertet wird, und die `bool` Wert `b` bestimmt wird:
   * Wenn eine implizite Konvertierung vom Typ des `b` zu `bool` vorhanden ist, und klicken Sie dann diese implizite Konvertierung ausgeführt wird, erzeugt eine `bool` Wert.
   * Andernfalls die `operator true` definiert durch den Typ der `b` wird aufgerufen, um zu erzeugen eine `bool` Wert.
*  Wenn die `bool` ist der Wert, der vom vorherigen Schritt generiert `true`, klicken Sie dann `x` ausgewertet und konvertiert Sie in den Typ des bedingten Ausdrucks, und dies ist das Ergebnis des bedingten Ausdrucks.
*  Andernfalls `y` ausgewertet und konvertiert Sie in den Typ des bedingten Ausdrucks, und dies ist das Ergebnis des bedingten Ausdrucks.

## <a name="anonymous-function-expressions"></a>Anonyme Funktionsausdrücke

Ein ***anonyme Funktion*** ist ein Ausdruck, der eine "inline" Methodendefinition darstellt. Eine anonyme Funktion verfügt nicht über einen Wert oder einen Typ in und von sich selbst, aber in einen kompatiblen Delegat- oder ausdrucksbaumtyp konvertierbar ist. Die Auswertung einer anonymen Funktion Konvertierung hängt von den Zieltyp der Konvertierung ab: Wenn sie einen Delegattyp ist, ergibt die Konvertierung eines Delegatwerts verweisen auf die Methode, die die anonyme Funktion definiert. Wenn sie einen Typ für die Ausdrucksbaumstruktur ist, ergibt sich die Konvertierung in eine Ausdrucksbaumstruktur, die die Struktur der Methode, wie eine Objektstruktur darstellt.

Aus Verlaufsgründen es gibt zwei syntaktische Varianten von anonymen Funktionen, d. h. *Lambda_expression*s und *Anonymous_method_expression*s. Fast alle Gründen *Lambda_expression*s sind präziser und ausdrucksbasierte als *Anonymous_method_expression*s, die in der Sprache für die Abwärtskompatibilität Kompatibilität bleiben.

```antlr
lambda_expression
    : anonymous_function_signature '=>' anonymous_function_body
    ;

anonymous_method_expression
    : 'delegate' explicit_anonymous_function_signature? block
    ;

anonymous_function_signature
    : explicit_anonymous_function_signature
    | implicit_anonymous_function_signature
    ;

explicit_anonymous_function_signature
    : '(' explicit_anonymous_function_parameter_list? ')'
    ;

explicit_anonymous_function_parameter_list
    : explicit_anonymous_function_parameter (',' explicit_anonymous_function_parameter)*
    ;

explicit_anonymous_function_parameter
    : anonymous_function_parameter_modifier? type identifier
    ;

anonymous_function_parameter_modifier
    : 'ref'
    | 'out'
    ;

implicit_anonymous_function_signature
    : '(' implicit_anonymous_function_parameter_list? ')'
    | implicit_anonymous_function_parameter
    ;

implicit_anonymous_function_parameter_list
    : implicit_anonymous_function_parameter (',' implicit_anonymous_function_parameter)*
    ;

implicit_anonymous_function_parameter
    : identifier
    ;

anonymous_function_body
    : expression
    | block
    ;
```

Der Operator `=>` verfügt über die gleiche Rangfolge wie die Zuweisung (`=`) und ist rechtsassoziativ.

Eine anonyme Funktion mit dem `async` -Modifizierer ist eine asynchrone Funktion und die in beschriebenen Regeln folgt [Iteratoren](classes.md#iterators).

Die Parameter einer anonymen Funktion in Form einer *Lambda_expression* können explizit oder implizit typisiert werden. In der Parameterliste eine explizit typisierte ist der Typ jedes Parameters explizit angegeben. In einer implizit typisierten Parameterliste, die Typen der Parameter aus dem Kontext, in dem die anonyme Funktion tritt auf, abgeleitet werden – insbesondere, wenn die anonyme Funktion konvertiert wird auf einen kompatiblen Delegaten-Typ oder den Typ für die Ausdrucksbaumstruktur, den bereitstellt die Parametertypen ([anonyme Funktion Konvertierungen](conversions.md#anonymous-function-conversions)).

In einer anonymen Funktion mit einem einzigen, implizit typisierte Parameter können die Klammern aus der Liste ausgelassen werden. Das heißt, eine anonyme Funktion des Formulars
```csharp
( param ) => expr
```
kann abgekürzt werden zu
```csharp
param => expr
```

Der Parameterliste eine anonyme Funktion in Form einer *Anonymous_method_expression* ist optional. Wenn angegeben, müssen die Parameter explizit typisiert sein. Wenn nicht der Fall, die anonyme Funktion konvertiert werden kann, um einen Delegaten mit einer der Parameter ist aufgelistet, die nicht enthalten `out` Parameter.

Ein *Block* Text einer anonymen Funktion erreichbar ist ([Endpunkte und Erreichbarkeit](statements.md#end-points-and-reachability)), wenn die anonyme Funktion in einer nicht erreichbar-Anweisung auftritt.

Einige Beispiele für anonyme Funktionen führen Sie die folgenden:

```csharp
x => x + 1                              // Implicitly typed, expression body
x => { return x + 1; }                  // Implicitly typed, statement body
(int x) => x + 1                        // Explicitly typed, expression body
(int x) => { return x + 1; }            // Explicitly typed, statement body
(x, y) => x * y                         // Multiple parameters
() => Console.WriteLine()               // No parameters
async (t1,t2) => await t1 + await t2    // Async
delegate (int x) { return x + 1; }      // Anonymous method expression
delegate { return 1 + 1; }              // Parameter list omitted
```

Das Verhalten der *Lambda_expression*s und *Anonymous_method_expression*s ist gleichermaßen, weisen die folgenden Punkte:

*  *Anonymous_method_expression*s zulassen die Parameterliste komplett weggelassen werden, stellt die Konvertierung zu einer Liste von Wertparameter Delegattypen.
*  *Lambda_expression*s zulassen, Parametertypen, die nicht angegeben werden, und abgeleitet werden, während *Anonymous_method_expression*erfordern Parametertypen explizit angegeben werden.
*  Der Text der ein *Lambda_expression* kann ein Ausdruck oder einen Anweisungsblock sein werden, während der Text der ein *Anonymous_method_expression* muss ein Anweisungsblock sein.
*  Nur *Lambda_expression*s haben Konvertierungen in kompatibel ausdrucksbaumstrukturtypen ([ausdrucksbaumstrukturtypen](types.md#expression-tree-types)).

### <a name="anonymous-function-signatures"></a>Anonyme Funktionssignaturen

Der optionale *Anonymous_function_signature* eine anonyme Funktion definiert, die Namen und optional auch die Typen der formalen Parameter für die anonyme Funktion. Der Bereich der Parameter für die anonyme Funktion ist die *Anonymous_function_body*. ([Bereiche](basic-concepts.md#scopes)) zusammen mit der Parameterliste (sofern vorhanden) bildet die anonyme-Methode-Body einen Deklarationsabschnitt ([Deklarationen](basic-concepts.md#declarations)). Es ist daher ein Fehler während der Kompilierung für den Namen eines Parameters, der anonyme Funktion, die mit dem Namen des eine lokale Variable, die lokale Konstante oder die Parameter, deren Bereich umfasst, die *Anonymous_method_expression* oder *Lambda_ Ausdruck*.

Wenn eine anonyme Funktion verfügt über eine *Explicit_anonymous_function_signature*, und klicken Sie dann der Satz von kompatible Delegattypen und ausdrucksbaumstrukturtypen auf diejenigen beschränkt ist, die den gleichen Parametertypen und -Modifizierern in der gleichen Reihenfolge aufweisen. Im Gegensatz zu Group Konvertierungen ([Gruppe Konvertierungen](conversions.md#method-group-conversions)), Kontravarianz von Parametertypen der anonymen Funktion wird nicht unterstützt. Wenn eine anonyme Funktion kein *Anonymous_function_signature*, und klicken Sie dann der Satz von kompatible Delegattypen und ausdrucksbaumstrukturtypen auf diejenigen beschränkt ist, die keine `out` Parameter.

Beachten Sie, dass ein *Anonymous_function_signature* darf keine Attribute oder ein Parameterarray. Dennoch eine *Anonymous_function_signature* kann mit einem Delegattyp, dessen Parameterliste ein Parameterarray enthält, kompatibel sein.

Beachten Sie auch diese Konvertierung in einen Typ für die Ausdrucksbaumstruktur, selbst wenn kompatibel, kann immer noch ausfallen, zum Zeitpunkt der Kompilierung ([ausdrucksbaumstrukturtypen](types.md#expression-tree-types)).

### <a name="anonymous-function-bodies"></a>Anonyme funktionsrümpfe

Der Text (*Ausdruck* oder *Block*) eine anonyme Funktion unterliegt den folgenden Regeln:

*  Wenn die anonyme Funktion eine Signatur enthält, sind die Parameter, die in der Signatur angegeben im Text zur Verfügung. Wenn die anonyme Funktion keine Signatur hat es konvertiert werden kann einem Delegattyp bzw. den Ausdruckstyp Parametern ([anonyme Funktion Konvertierungen](conversions.md#anonymous-function-conversions)), aber der Parameter können nicht zugegriffen werden, im Text.
*  Mit Ausnahme von `ref` oder `out` Parameter angegeben wird, in der Signatur (sofern vorhanden) der nächsten einschließenden anonyme Funktion wird ein Fehler während der Kompilierung für den Text, den Zugriff auf eine `ref` oder `out` Parameter.
*  Wenn der Typ des `this` ist ein Strukturtyp, es ist ein Fehler während der Kompilierung für den Text, den Zugriff auf `this`. Dies ist "true" gibt an, ob der Zugriff explizit ist (wie in `this.x`) oder implizit (wie in `x` , in denen `x` ist ein Instanzmember der Struktur). Diese Regel verhindert, dass dieser Zugriff einfach und wirkt sich nicht, ob die Suche nach Membern in ein Member der Struktur ergibt.
*  Der Text hat Zugriff auf die äußeren Variablen ([äußeren Variablen](expressions.md#outer-variables)) der anonymen Funktion. Zugriff auf eine äußere Variable verweist auf die Instanz der Variablen, die zu dem Zeitpunkt aktiv ist die *Lambda_expression* oder *Anonymous_method_expression* ausgewertet wird ([Auswertung anonyme Funktionsausdrücke](expressions.md#evaluation-of-anonymous-function-expressions)).
*  Es ist ein Fehler während der Kompilierung für den Textkörper enthält ein `goto` -Anweisung `break` -Anweisung oder `continue` -Anweisung ein, deren Ziel außerhalb des Texts oder im Text einer enthaltenen anonymen Funktion.
*  Ein `return` -Anweisung im Hauptteil übergibt die Kontrolle über einen Aufruf der nächsten einschließenden anonyme Funktion, die nicht aus der einschließenden Funktionsmember. Ein im angegebenen Ausdruck eine `return` -Anweisung muss implizit in den Rückgabetyp des Delegattyps oder Typ, der für die Ausdrucksbaumstruktur sein der nächsten einschließenden *Lambda_expression* oder *Anonymous_ Method_expression* konvertiert wird ([anonyme Funktion Konvertierungen](conversions.md#anonymous-function-conversions)).

Ist nicht explizit angegeben, ob es eine Möglichkeit gibt, führen Sie den Block, eine anonyme Funktion außer durch Auswertung und der Aufruf der *Lambda_expression* oder *Anonymous_method_expression*. Insbesondere kann der Compiler so implementieren Sie eine anonyme Funktion synthetisieren eine oder mehrere benannte Methoden und Typen. Die Namen dieser alle synthetischen Elemente müssen eines Formulars, das für die Verwendung durch den Compiler reserviert sein.

### <a name="overload-resolution-and-anonymous-functions"></a>Auflösung von funktionsüberladungen und anonymen Funktionen

Anonyme Funktionen in einer Argumentliste Typrückschluss teilnehmen und Auflösen der Überladung. Näheres [Typrückschluss](expressions.md#type-inference) und [Überladungsauflösung](expressions.md#overload-resolution) die genauen Regeln.

Das folgende Beispiel veranschaulicht die Auswirkungen von anonymen Funktionen auf überladungsauflösung.

```csharp
class ItemList<T>: List<T>
{
    public int Sum(Func<T,int> selector) {
        int sum = 0;
        foreach (T item in this) sum += selector(item);
        return sum;
    }

    public double Sum(Func<T,double> selector) {
        double sum = 0;
        foreach (T item in this) sum += selector(item);
        return sum;
    }
}
```

Die `ItemList<T>` -Klasse verfügt über zwei `Sum` Methoden. Jede hat eine `selector` -Argument, das der Wert auf Summe von einem Listenelement extrahiert. Der extrahierte Wert kann entweder ein `int` oder `double` und die resultierende Summe ist ebenso entweder ein `int` oder ein `double`.

Die `Sum` Methoden können z. B. zum Berechnen von Summen aus einer Liste der Detailzeilen in einer Reihenfolge verwendet werden.

```csharp
class Detail
{
    public int UnitCount;
    public double UnitPrice;
    ...
}

void ComputeSums() {
    ItemList<Detail> orderDetails = GetOrderDetails(...);
    int totalUnits = orderDetails.Sum(d => d.UnitCount);
    double orderTotal = orderDetails.Sum(d => d.UnitPrice * d.UnitCount);
    ...
}
```

In den ersten Aufruf des `orderDetails.Sum`, beide `Sum` Methoden gelten. da die anonyme Funktion `d => d. UnitCount` ist kompatibel mit sowohl `Func<Detail,int>` und `Func<Detail,double>`. Auflösung von funktionsüberladungen wählt jedoch die erste `Sum` Methode da die Konvertierung in `Func<Detail,int>` ist besser als die Konvertierung zu `Func<Detail,double>`.

Im zweiten Aufruf von `orderDetails.Sum`, nur der zweite `Sum` Methode gilt. da die anonyme Funktion `d => d.UnitPrice * d.UnitCount` erzeugt einen Wert vom Typ `double`. Daher überladen Auflösung auswählt, die zweite `Sum` Methode für diesen Aufruf.

### <a name="anonymous-functions-and-dynamic-binding"></a>Anonyme Funktionen und dynamische Bindung

Eine anonyme Funktion darf nicht über einen Empfänger, Argument oder zum Operand eine dynamisch gebundene Vorgang sein.

### <a name="outer-variables"></a>Äußere Variablen

Alle lokalen Variablen, "Value"-Parameter oder Parameterarray, deren Bereich umfasst, die *Lambda_expression* oder *Anonymous_method_expression* heißt ein ***äußere Variable*** der anonyme Funktion. In einer Instanz Funktionsmember der Member einer Klasse die `this` Wert gilt, dass einen Value-Parameter, und eine äußere Variable von jeder anonyme Funktion, die in der Funktionsmember enthalten sind.

#### <a name="captured-outer-variables"></a>Äußere Variablen erfasst

Wenn eine äußere Variable, die von einer anonymen Funktion verwiesen wird, gilt die äußere Variable wurden als ***erfasst*** von der anonymen Funktion. Die Lebensdauer einer lokalen Variablen ist normalerweise begrenzt, für die Ausführung von Block oder der Anweisung, mit denen sie zugeordnet ist ([lokale Variablen](variables.md#local-variables)). Jedoch die Lebensdauer einer erfassten äußeren Variablen wird mindestens erweitert, bis der Delegat oder Ausdrucksbaumstruktur, die aus der anonymen Funktion erstellt wurde, wird für die Garbagecollection.

Im Beispiel
```csharp
using System;

delegate int D();

class Test
{
    static D F() {
        int x = 0;
        D result = () => ++x;
        return result;
    }

    static void Main() {
        D d = F();
        Console.WriteLine(d());
        Console.WriteLine(d());
        Console.WriteLine(d());
    }
}
```
die lokale Variable `x` wird erfasst, indem Sie die anonyme Funktion und die Lebensdauer des `x` mindestens erweitert wurde, bis der Delegat, der von zurückgegebene `F` wird zum Kandidaten für die Garbagecollection (die erst ganz am Ende nicht der Fall ist das Programm). Da der gleichen Instanz von jedem Aufruf der anonymen Funktion arbeitet `x`, die Ausgabe des Beispiels ist:
```
1
2
3
```

Wenn eine lokale Variable oder ein Value-Parameter, die von einer anonymen Funktion erfasst sind, die lokale Variable oder Parameter wird nicht mehr als eine feste Variable sein ([für feste und verschiebbare Variablen](unsafe-code.md#fixed-and-moveable-variables)), aber stattdessen gilt eine Moveable Variable. Daher alle `unsafe` Code, der die Adresse der erfassten äußere Variable ist, muss zunächst mithilfe der `fixed` Anweisung, um die Variable zu beheben.

Beachten Sie, dass im Gegensatz zu einer Variable uncaptured eine aufgezeichnete lokale Variable gleichzeitig für mehrere Ausführungsthreads verfügbar gemacht werden kann.

#### <a name="instantiation-of-local-variables"></a>Instanziierung von lokalen Variablen

Eine lokale Variable gilt ***instanziiert*** bei Ausführung des Gültigkeitsbereichs der Variablen wechselt. Z. B. wenn die folgende Methode aufgerufen wird, die lokale Variable `x` instanziiert und initialisiert drei Mal – einmal für jede Iteration der Schleife.

```csharp
static void F() {
    for (int i = 0; i < 3; i++) {
        int x = i * 2 + 1;
        ...
    }
}
```

Verschieben Sie jedoch die Deklaration von `x` außerhalb der Schleife Ergebnisse in eine einzelne Instanziierung `x`:
```csharp
static void F() {
    int x;
    for (int i = 0; i < 3; i++) {
        x = i * 2 + 1;
        ...
    }
}
```

Wenn nicht erfasst werden, besteht keine Möglichkeit, beachten Sie, wie oft eine lokale Variable instanziiert wird, da die Lebensdauer der die Instanziierungen disjunkt sind, kann für alle Instanziierungen einfach am gleichen Speicherort verwenden. Wenn eine anonyme Funktion eine lokale Variable erhält, werden die Auswirkungen der Instanziierung jedoch offensichtlich.

Im Beispiel
```csharp
using System;

delegate void D();

class Test
{
    static D[] F() {
        D[] result = new D[3];
        for (int i = 0; i < 3; i++) {
            int x = i * 2 + 1;
            result[i] = () => { Console.WriteLine(x); };
        }
        return result;
    }

    static void Main() {
        foreach (D d in F()) d();
    }
}
```
erzeugt die Ausgabe:
```
1
3
5
```

Aber wenn die Deklaration von `x` wird außerhalb der Schleife verschoben:
```csharp
static D[] F() {
    D[] result = new D[3];
    int x;
    for (int i = 0; i < 3; i++) {
        x = i * 2 + 1;
        result[i] = () => { Console.WriteLine(x); };
    }
    return result;
}
```
Die Ausgabe lautet:
```
5
5
5
```

Wenn eine Iterationsvariable in eine for-Schleife deklariert wird, gilt die Variable selbst außerhalb der Schleife deklariert werden. Also, wenn das Beispiel so geändert wird, um die Iterationsvariable selbst zu erfassen:

```csharp
static D[] F() {
    D[] result = new D[3];
    for (int i = 0; i < 3; i++) {
        result[i] = () => { Console.WriteLine(i); };
    }
    return result;
}
```
nur eine Instanz der Iterationsvariablen wird erfasst, das die Ausgabe generiert:
```
3
3
3
```

Es ist möglich, dass anonyme Funktion freigeben einige erfassten Variablen noch verfügen über separate Instanzen von anderen Delegaten. Z. B. wenn `F` in geändert wird
```csharp
static D[] F() {
    D[] result = new D[3];
    int x = 0;
    for (int i = 0; i < 3; i++) {
        int y = 0;
        result[i] = () => { Console.WriteLine("{0} {1}", ++x, ++y); };
    }
    return result;
}
```
die drei Delegaten erfassen die gleiche Instanz von `x` jedoch separaten Instanzen von `y`, und die Ausgabe:
```
1 1
2 1
3 1
```

Separate anonyme Funktionen können mit die gleiche Instanz von eine äußere Variable erfassen. Im folgenden Beispiel
```csharp
using System;

delegate void Setter(int value);

delegate int Getter();

class Test
{
    static void Main() {
        int x = 0;
        Setter s = (int value) => { x = value; };
        Getter g = () => { return x; };
        s(5);
        Console.WriteLine(g());
        s(10);
        Console.WriteLine(g());
    }
}
```
die beiden anonymen Funktionen erfassen die gleiche Instanz von der lokalen Variablen `x`, und sie können daher "kommunizieren" über diese Variable. Die Ausgabe des Beispiels ist:
```
5
10
```

### <a name="evaluation-of-anonymous-function-expressions"></a>Auswertung von Ausdrücken für anonyme Funktion

Eine anonyme Funktion `F` muss immer in einen Delegattyp konvertiert werden `D` oder ein Typ für die Ausdrucksbaumstruktur `E`, entweder direkt oder über die Ausführung von Delegaterstellungsausdrücke `new D(F)`. Diese Konvertierung bestimmt das Ergebnis der anonymen Hauptfunktion unter [anonyme Funktion Konvertierungen](conversions.md#anonymous-function-conversions).

## <a name="query-expressions"></a>Abfrageausdrücke

***Abfrageausdrücke*** bieten eine integrierte Sprachsyntax für Abfragen, die relationale und hierarchische Abfragesprachen wie SQL oder XQuery ähnelt.

```antlr
query_expression
    : from_clause query_body
    ;

from_clause
    : 'from' type? identifier 'in' expression
    ;

query_body
    : query_body_clauses? select_or_group_clause query_continuation?
    ;

query_body_clauses
    : query_body_clause
    | query_body_clauses query_body_clause
    ;

query_body_clause
    : from_clause
    | let_clause
    | where_clause
    | join_clause
    | join_into_clause
    | orderby_clause
    ;

let_clause
    : 'let' identifier '=' expression
    ;

where_clause
    : 'where' boolean_expression
    ;

join_clause
    : 'join' type? identifier 'in' expression 'on' expression 'equals' expression
    ;

join_into_clause
    : 'join' type? identifier 'in' expression 'on' expression 'equals' expression 'into' identifier
    ;

orderby_clause
    : 'orderby' orderings
    ;

orderings
    : ordering (',' ordering)*
    ;

ordering
    : expression ordering_direction?
    ;

ordering_direction
    : 'ascending'
    | 'descending'
    ;

select_or_group_clause
    : select_clause
    | group_clause
    ;

select_clause
    : 'select' expression
    ;

group_clause
    : 'group' expression 'by' expression
    ;

query_continuation
    : 'into' identifier query_body
    ;
```

Ein Abfrageausdruck beginnt mit einer `from` -Klausel und endet mit einem `select` oder `group` Klausel. Der ursprüngliche `from` Klausel gefolgt von NULL oder mehr `from`, `let`, `where`, `join` oder `orderby` Klauseln. Jede `from` -Klausel ist ein Generator Einführung in eine ***Bereichsvariable*** der Bereiche, über die Elemente einer ***Sequenz***. Jede `let` Klausel führt eine Bereichsvariable, der ein über den vorherigen Bereichsvariablen berechnet. Jede `where` -Klausel ist ein Filter, der Elemente aus dem Ergebnis ausgeschlossen sind. Jede `join` -Klausel vergleicht die angegebenen Schlüssel der Quellsequenz mit Schlüsseln von einer anderen Sequenz, die übereinstimmende Paare Rückgabe. Jede `orderby` Klausel sortiert die Elemente gemäß den angegebenen Kriterien. Die endgültige `select` oder `group` -Klausel gibt die Form des Ergebnisses in Bezug auf die Bereichsvariablen. Zum Schluss eine `into` -Klausel kann verwendet werden, für die Abfragen "Zusammenführung", indem Sie die Ergebnisse einer Abfrage als einen Generator in einer nachfolgenden Abfrage zu behandeln.

### <a name="ambiguities-in-query-expressions"></a>Allerdings haben Mehrdeutigkeiten in Abfrageausdrücken

Abfrageausdrücke enthalten eine Anzahl von "kontextabhängige Schlüsselwörter", d. h. die Bezeichner, die in einem bestimmten Kontext eine besondere Bedeutung haben. Insbesondere Hierbei handelt es sich `from`, `where`, `join`, `on`, `equals`, `into`, `let`, `orderby`, `ascending`, `descending`, `select`, `group` und `by`. Um Mehrdeutigkeiten in Abfrageausdrücken aufgrund gemischter Verwendung dieser Bezeichner als Schlüsselwörter oder einfache Namen zu vermeiden, gelten diese Bezeichner Schlüsselwörter, wenn eine beliebige Stelle in einem Abfrageausdruck stattfinden.

Zu diesem Zweck ein Abfrageausdruck ist ein Ausdruck, der mit "`from identifier`"gefolgt von jedes Token, mit Ausnahme von"`;`","`=`"oder"`,`".

Um diese Wörter als Bezeichner in einem Abfrageausdruck verwenden, sie können dem Präfix "`@`" ([Bezeichner](lexical-structure.md#identifiers)).

### <a name="query-expression-translation"></a>Übersetzung Abfrageausdrucks

Die C#-Sprache gibt nicht die Ausführungssemantik von Abfrageausdrücken an. Stattdessen Abfrageausdrücke werden in übersetzt, Aufrufe von Methoden, die folgen, der *Abfrageausdrucksmuster* ([das Abfrageausdrucksmuster](expressions.md#the-query-expression-pattern)). Insbesondere werden die Abfrageausdrücke in Aufrufe von Methoden, die mit dem Namen übersetzt `Where`, `Select`, `SelectMany`, `Join`, `GroupJoin`, `OrderBy`, `OrderByDescending`, `ThenBy`, `ThenByDescending`, `GroupBy`, und `Cast`. Diese Methoden werden voraussichtlich bestimmter Signaturen und Ergebnistypen, siehe [das Abfrageausdrucksmuster](expressions.md#the-query-expression-pattern). Diese Methoden können sein, Instanzmethoden des abgefragten Objekts oder von Erweiterungsmethoden, die auf das Objekt extern sind, und implementieren sie die eigentliche Ausführung der Abfrage.

Die Übersetzung von Abfrageausdrücken Methodenaufrufe ist eine syntaktische Zuordnung, die vor jeder typbindung oder die Auflösung von funktionsüberladungen ausgeführt wurde. Die Übersetzung wird garantiert, syntaktisch korrekt zu sein, aber nicht notwendigerweise um semantisch richtige C#-Code zu erzeugen. Nach der Übersetzung von Abfrageausdrücken, die sich ergebende Methodenaufrufe werden als reguläre Methodenaufrufe verarbeitet, und dies kann wiederum Fehler, z. B. aufzudecken, wenn die Methoden nicht vorhanden sind, wenn Argumente falsch aufweisen, oder die Methoden generisch sind und Typrückschluss schlägt fehl.

Ein Abfrageausdruck wird verarbeitet, durch die folgenden Übersetzungen wiederholtes anwenden, bis keine weiteren Reduzierung möglich sind. Die Übersetzungen sind in der Reihenfolge der Anwendung aufgeführt: jeder Abschnitt wird davon ausgegangen, dass die Übersetzungen in den vorherigen Abschnitten wurden vollständig ausgeführt, und nachdem erreicht, ein Abschnitt nicht höher sein bei der Verarbeitung der selben Abfrageausdruck Revisited wird.

Zuweisung zu einer Bereichsvariablen darf nicht in Abfrageausdrücken. Eine C#-Implementierung darf jedoch nicht immer dieser Einschränkung zu erzwingen, da dies in einigen Fällen möglicherweise nicht mit dem hier vorgestellten syntaktische Übersetzung-Schema werden kann.

Bestimmte Übersetzungen injizieren Bereichsvariablen mit transparenter Bezeichner gekennzeichnet durch `*`. Die speziellen Eigenschaften transparenter Bezeichner werden erläutert. im weiteren [transparenter Bezeichner](expressions.md#transparent-identifiers).

#### <a name="select-and-groupby-clauses-with-continuations"></a>Wählen Sie "und" Groupby-Klausel mit Fortsetzungen

Ein Abfrageausdruck mit einer Fortsetzung
```csharp
from ... into x ...
```
wird übersetzt
```csharp
from x in ( from ... ) ...
```

Die Übersetzungen in den folgenden Abschnitten wird davon ausgegangen, dass Abfragen keine `into` Fortsetzungen.

Im Beispiel
```csharp
from c in customers
group c by c.Country into g
select new { Country = g.Key, CustCount = g.Count() }
```
wird übersetzt
```csharp
from g in
    from c in customers
    group c by c.Country
select new { Country = g.Key, CustCount = g.Count() }
```
die endgültige Übersetzung der ist
```csharp
customers.
GroupBy(c => c.Country).
Select(g => new { Country = g.Key, CustCount = g.Count() })
```

#### <a name="explicit-range-variable-types"></a>Explizite Bereich Variablentypen

Ein `from` -Klausel, die einen Bereichsvariablentyp gibt explizit an
```csharp
from T x in e
```
wird übersetzt
```csharp
from x in ( e ) . Cast < T > ( )
```

Ein `join` -Klausel, die einen Bereichsvariablentyp gibt explizit an
```
join T x in e on k1 equals k2
```
wird übersetzt
```
join x in ( e ) . Cast < T > ( ) on k1 equals k2
```

Die Übersetzungen in den folgenden Abschnitten wird davon ausgegangen, dass Abfragen keine expliziten Bereich Variablentypen.

Im Beispiel
```csharp
from Customer c in customers
where c.City == "London"
select c
```
wird übersetzt
```csharp
from c in customers.Cast<Customer>()
where c.City == "London"
select c
```
die endgültige Übersetzung der ist
```csharp
customers.
Cast<Customer>().
Where(c => c.City == "London")
```

Variablentypen expliziten Bereich sind nützlich zum Abfragen von Auflistungen bereit, die nicht generische implementieren `IEnumerable` -Schnittstelle, aber nicht die generische `IEnumerable<T>` Schnittstelle. Im obigen Beispiel wäre dies der Fall, wenn `customers` wurden vom Typ `ArrayList`.

#### <a name="degenerate-query-expressions"></a>Degenerierte-Abfrageausdrücke

Ein Abfrageausdruck den Wert des Formulars
```csharp
from x in e select x
```
wird übersetzt
```csharp
( e ) . Select ( x => x )
```

Im Beispiel
```csharp
from c in customers
select c
```
wird übersetzt
```csharp
customers.Select(c => c)
```

Ein degenerierter Abfrageausdruck ist eine, die einfach die Elemente der Quelle auswählt. Eine spätere Phase der Übersetzung entfernt degenerierte Abfragen durch andere Übersetzungsschritten, indem diese zu ihrer Quelle ersetzt. Es ist wichtig, jedoch um sicherzustellen, dass das Ergebnis einer Abfrage Ausdruck nie das Quellobjekt selbst ist, wie die, die den Typ und die Identität der Quelle an den Client der Abfrage anzeigen würden. Aus diesem Grund schützt dadurch degenerierte Abfragen, die direkt im Quellcode geschrieben wird, durch explizites Aufrufen von `Select` auf dem Quellcomputer. Es ist Ihre Aufgabe die Implementierer von `Select` und andere Abfrageoperatoren, um sicherzustellen, dass diese Methoden geben das Quellobjekt selbst nie zurück.

#### <a name="from-let-where-join-and-orderby-clauses"></a>Zu ermöglichen, where, Join und Orderby-Klausel

Ein Abfrageausdruck mit einer Sekunde `from` Klausel gefolgt von einem `select` Klausel
```csharp
from x1 in e1
from x2 in e2
select v
```
wird übersetzt
```csharp
( e1 ) . SelectMany( x1 => e2 , ( x1 , x2 ) => v )
```

Ein Abfrageausdruck mit einer Sekunde `from` Klausel gefolgt von etwas anders als eine `select` Klausel:

```csharp
from x1 in e1
from x2 in e2
...
```
wird übersetzt
```csharp
from * in ( e1 ) . SelectMany( x1 => e2 , ( x1 , x2 ) => new { x1 , x2 } )
...
```

Ein Abfrageausdruck mit einer `let` Klausel
```csharp
from x in e
let y = f
...
```
wird übersetzt
```csharp
from * in ( e ) . Select ( x => new { x , y = f } )
...
```

Ein Abfrageausdruck mit einer `where` Klausel
```csharp
from x in e
where f
...
```
wird übersetzt
```csharp
from x in ( e ) . Where ( x => f )
...
```

Einen Abfrageausdruck mit einer `join` -Klausel ohne eine `into` gefolgt von einem `select` Klausel
```csharp
from x1 in e1
join x2 in e2 on k1 equals k2
select v
```
wird übersetzt
```csharp
( e1 ) . Join( e2 , x1 => k1 , x2 => k2 , ( x1 , x2 ) => v )
```

Einen Abfrageausdruck mit einer `join` -Klausel ohne eine `into` gefolgt von etwas anders als eine `select` Klausel
```csharp
from x1 in e1
join x2 in e2 on k1 equals k2
...
```
wird übersetzt
```csharp
from * in ( e1 ) . Join( e2 , x1 => k1 , x2 => k2 , ( x1 , x2 ) => new { x1 , x2 })
...
```

Einen Abfrageausdruck mit einer `join` -Klausel mit einer `into` gefolgt von einer `select` Klausel
```csharp
from x1 in e1
join x2 in e2 on k1 equals k2 into g
select v
```
wird übersetzt
```csharp
( e1 ) . GroupJoin( e2 , x1 => k1 , x2 => k2 , ( x1 , g ) => v )
```

Einen Abfrageausdruck mit einer `join` -Klausel mit einer `into` gefolgt von etwas anders als eine `select` Klausel
```csharp
from x1 in e1
join x2 in e2 on k1 equals k2 into g
...
```
wird übersetzt
```csharp
from * in ( e1 ) . GroupJoin( e2 , x1 => k1 , x2 => k2 , ( x1 , g ) => new { x1 , g })
...
```

Ein Abfrageausdruck mit einer `orderby` Klausel
```csharp
from x in e
orderby k1 , k2 , ..., kn
...
```
wird übersetzt
```csharp
from x in ( e ) . 
OrderBy ( x => k1 ) . 
ThenBy ( x => k2 ) .
... .
ThenBy ( x => kn )
...
```

-Klausel gibt an, wenn eine Sortierung, die eine `descending` Richtung-Symbol, einen Aufruf der `OrderByDescending` oder `ThenByDescending` stattdessen erzeugt wird.

Die folgenden Übersetzungen wird davon ausgegangen, dass es sind keine `let`, `where`, `join` oder `orderby` -Klauseln und nicht mehr als einen ersten `from` -Klausel in jedem Abfrageausdruck.

Im Beispiel
```csharp
from c in customers
from o in c.Orders
select new { c.Name, o.OrderID, o.Total }
```
wird übersetzt
```csharp
customers.
SelectMany(c => c.Orders,
     (c,o) => new { c.Name, o.OrderID, o.Total }
)
```

Im Beispiel
```csharp
from c in customers
from o in c.Orders
orderby o.Total descending
select new { c.Name, o.OrderID, o.Total }
```
wird übersetzt
```csharp
from * in customers.
    SelectMany(c => c.Orders, (c,o) => new { c, o })
orderby o.Total descending
select new { c.Name, o.OrderID, o.Total }
```
die endgültige Übersetzung der ist
```csharp
customers.
SelectMany(c => c.Orders, (c,o) => new { c, o }).
OrderByDescending(x => x.o.Total).
Select(x => new { x.c.Name, x.o.OrderID, x.o.Total })
```
wo `x` ist ein vom Compiler generierten Bezeichner, der ansonsten nicht sichtbar und kann nicht zugegriffen werden.

Im Beispiel
```csharp
from o in orders
let t = o.Details.Sum(d => d.UnitPrice * d.Quantity)
where t >= 1000
select new { o.OrderID, Total = t }
```
wird übersetzt
```csharp
from * in orders.
    Select(o => new { o, t = o.Details.Sum(d => d.UnitPrice * d.Quantity) })
where t >= 1000 
select new { o.OrderID, Total = t }
```
die endgültige Übersetzung der ist
```csharp
orders.
Select(o => new { o, t = o.Details.Sum(d => d.UnitPrice * d.Quantity) }).
Where(x => x.t >= 1000).
Select(x => new { x.o.OrderID, Total = x.t })
```
wo `x` ist ein vom Compiler generierten Bezeichner, der ansonsten nicht sichtbar und kann nicht zugegriffen werden.

Im Beispiel
```csharp
from c in customers
join o in orders on c.CustomerID equals o.CustomerID
select new { c.Name, o.OrderDate, o.Total }
```
wird übersetzt
```csharp
customers.Join(orders, c => c.CustomerID, o => o.CustomerID,
    (c, o) => new { c.Name, o.OrderDate, o.Total })
```

Im Beispiel
```csharp
from c in customers
join o in orders on c.CustomerID equals o.CustomerID into co
let n = co.Count()
where n >= 10
select new { c.Name, OrderCount = n }
```
wird übersetzt
```csharp
from * in customers.
    GroupJoin(orders, c => c.CustomerID, o => o.CustomerID,
        (c, co) => new { c, co })
let n = co.Count()
where n >= 10 
select new { c.Name, OrderCount = n }
```
die endgültige Übersetzung der ist
```csharp
customers.
GroupJoin(orders, c => c.CustomerID, o => o.CustomerID,
    (c, co) => new { c, co }).
Select(x => new { x, n = x.co.Count() }).
Where(y => y.n >= 10).
Select(y => new { y.x.c.Name, OrderCount = y.n)
```
wo `x` und `y` sind vom Compiler generierten Bezeichner, die andernfalls nicht sichtbar und kann nicht zugegriffen werden.

Im Beispiel
```csharp
from o in orders
orderby o.Customer.Name, o.Total descending
select o
```
hat die letzte Übersetzung
```csharp
orders.
OrderBy(o => o.Customer.Name).
ThenByDescending(o => o.Total)
```

#### <a name="select-clauses"></a>Select-Klauseln

Ein Abfrageausdruck den Wert des Formulars
```csharp
from x in e select v
```
wird übersetzt
```csharp
( e ) . Select ( x => v )
```
es sei denn, V den Bezeichner X, die Übersetzung ist einfach
```csharp
( e )
```

Beispiel:
```csharp
from c in customers.Where(c => c.City == "London")
select c
```
In wird einfach übersetzt werden.
```csharp
customers.Where(c => c.City == "London")
```

#### <a name="groupby-clauses"></a>GroupBy-Klausel

Ein Abfrageausdruck den Wert des Formulars
```csharp
from x in e group v by k
```
wird übersetzt
```csharp
( e ) . GroupBy ( x => k , x => v )
```
es sei denn, V den Bezeichner X, die Übersetzung ist.
```csharp
( e ) . GroupBy ( x => k )
```

Im Beispiel
```csharp
from c in customers
group c.Name by c.Country
```
wird übersetzt
```csharp
customers.
GroupBy(c => c.Country, c => c.Name)
```

#### <a name="transparent-identifiers"></a>Transparenter Bezeichner

Bestimmte Übersetzungen einfügen Bereichsvariablen mit ***transparenter Bezeichner*** gekennzeichnet durch `*`. Transparente-IDs sind keine geeignete Sprache-Funktion. Sie sind nur als ein Zwischenschritt bei der Abfrage Ausdruck Übersetzungsprozess vorhanden.

Wenn eine abfrageübersetzung einen transparenten Bezeichner einbettet sollten, weiter weitergegeben Übersetzungsschritten den transparenten Bezeichner in anonymen Funktionen und anonyme Objektinitialisierer. In diesen Kontexten weisen transparenter Bezeichner folgendes Verhalten auf:

*  Wenn ein transparenter Bezeichner als Parameter in einer anonymen Funktion auftritt, werden die Member des anonymen Typs zugeordnet automatisch im Bereich des Texts der anonymen Funktion.
*  Wenn ein Element mit einem transparenten Bezeichner im Gültigkeitsbereich befindet, sind im Bereich auch die Elemente dieses Elements.
*  Tritt ein transparenter Bezeichner als ein Member-Declarator in einem anonyme Objektinitialisierer, führt er einen Member mit einem transparenten Bezeichner.
*  In den oben beschriebenen Übersetzungsschritten werden transparent Bezeichner immer zusammen mit anonymen Typen, mit der Absicht an mehrere Bereichsvariablen als Mitglieder eines einzelnen Objekts erfassen eingeführt. Eine Implementierung von c# ist zulässig, einen anderen Mechanismus als anonyme Typen zu verwenden, um mehrere Bereichsvariablen zu gruppieren. In den folgenden Beispielen für die Übersetzung wird davon ausgegangen, dass anonyme Typen werden verwendet, und wie transparent Bezeichner zeigen sofort übersetzt werden kann.

Im Beispiel
```csharp
from c in customers
from o in c.Orders
orderby o.Total descending
select new { c.Name, o.Total }
```
wird übersetzt
```csharp
from * in customers.
    SelectMany(c => c.Orders, (c,o) => new { c, o })
orderby o.Total descending
select new { c.Name, o.Total }
```

die übersetzt weiteren in
```csharp
customers.
SelectMany(c => c.Orders, (c,o) => new { c, o }).
OrderByDescending(* => o.Total).
Select(* => new { c.Name, o.Total })
```
die transparente Bezeichner gelöscht werden, entspricht
```csharp
customers.
SelectMany(c => c.Orders, (c,o) => new { c, o }).
OrderByDescending(x => x.o.Total).
Select(x => new { x.c.Name, x.o.Total })
```
wo `x` ist ein vom Compiler generierten Bezeichner, der ansonsten nicht sichtbar und kann nicht zugegriffen werden.

Im Beispiel
```csharp
from c in customers
join o in orders on c.CustomerID equals o.CustomerID
join d in details on o.OrderID equals d.OrderID
join p in products on d.ProductID equals p.ProductID
select new { c.Name, o.OrderDate, p.ProductName }
```
wird übersetzt
```csharp
from * in customers.
    Join(orders, c => c.CustomerID, o => o.CustomerID, 
        (c, o) => new { c, o })
join d in details on o.OrderID equals d.OrderID
join p in products on d.ProductID equals p.ProductID
select new { c.Name, o.OrderDate, p.ProductName }
```
die weiteren wird nach einer Verkleinerung auf
```csharp
customers.
Join(orders, c => c.CustomerID, o => o.CustomerID, (c, o) => new { c, o }).
Join(details, * => o.OrderID, d => d.OrderID, (*, d) => new { *, d }).
Join(products, * => d.ProductID, p => p.ProductID, (*, p) => new { *, p }).
Select(* => new { c.Name, o.OrderDate, p.ProductName })
```
die endgültige Übersetzung der ist
```csharp
customers.
Join(orders, c => c.CustomerID, o => o.CustomerID,
    (c, o) => new { c, o }).
Join(details, x => x.o.OrderID, d => d.OrderID,
    (x, d) => new { x, d }).
Join(products, y => y.d.ProductID, p => p.ProductID,
    (y, p) => new { y, p }).
Select(z => new { z.y.x.c.Name, z.y.x.o.OrderDate, z.p.ProductName })
```
wo `x`, `y`, und `z` sind vom Compiler generierten Bezeichner, die andernfalls nicht sichtbar und kann nicht zugegriffen werden.

### <a name="the-query-expression-pattern"></a>Das Abfrageausdrucksmuster

Die ***Abfrageausdrucksmuster*** richtet ein Muster von Methoden, Typen implementiert werden können, um Abfrageausdrücke unterstützen. Da durch eine syntaktische Zuordnung Abfrageausdrücke Methodenaufrufe übersetzt werden, haben Typen beträchtliche Flexibilität bei, wie sie das Abfrageausdrucksmuster implementieren. Beispielsweise können die Methoden des Musters als Instanzmethoden oder als Erweiterungsmethoden implementiert werden, da die beiden die gleiche Aufrufsyntax haben und die Methoden Delegaten oder Ausdrucksbaumstrukturen anfordern können, da anonyme Funktionen sowohl konvertiert werden.

Die empfohlene Form eines generischen Typs `C<T>` , unterstützt Abfragemusters Ausdruck unten dargestellt ist. Ein generischer Typ wird verwendet, um die richtigen Beziehungen zwischen Parameter und Ergebnis zu veranschaulichen, aber es ist möglich, die das Muster für nicht generische Typen ebenfalls implementieren.

```csharp
delegate R Func<T1,R>(T1 arg1);

delegate R Func<T1,T2,R>(T1 arg1, T2 arg2);

class C
{
    public C<T> Cast<T>();
}

class C<T> : C
{
    public C<T> Where(Func<T,bool> predicate);

    public C<U> Select<U>(Func<T,U> selector);

    public C<V> SelectMany<U,V>(Func<T,C<U>> selector,
        Func<T,U,V> resultSelector);

    public C<V> Join<U,K,V>(C<U> inner, Func<T,K> outerKeySelector,
        Func<U,K> innerKeySelector, Func<T,U,V> resultSelector);

    public C<V> GroupJoin<U,K,V>(C<U> inner, Func<T,K> outerKeySelector,
        Func<U,K> innerKeySelector, Func<T,C<U>,V> resultSelector);

    public O<T> OrderBy<K>(Func<T,K> keySelector);

    public O<T> OrderByDescending<K>(Func<T,K> keySelector);

    public C<G<K,T>> GroupBy<K>(Func<T,K> keySelector);

    public C<G<K,E>> GroupBy<K,E>(Func<T,K> keySelector,
        Func<T,E> elementSelector);
}

class O<T> : C<T>
{
    public O<T> ThenBy<K>(Func<T,K> keySelector);

    public O<T> ThenByDescending<K>(Func<T,K> keySelector);
}

class G<K,T> : C<T>
{
    public K Key { get; }
}
```

Die oben genannten Methoden verwenden die generische Delegattypen `Func<T1,R>` und `Func<T1,T2,R>`, aber sie ebenso gut hätten auch andere Typen für Delegat- oder Struktur mit den gleichen Beziehungen in Parameter und Ergebnis.

Beachten Sie, dass die empfohlene Beziehung zwischen `C<T>` und `O<T>` wird sichergestellt, dass die `ThenBy` und `ThenByDescending` Methoden stehen nur auf das Ergebnis einer `OrderBy` oder `OrderByDescending`. Beachten Sie auch die empfohlene Form des Ergebnisses `GroupBy` – eine Sequenz von Sequenzen, die jeder innere Sequenz verfügt, in denen über einen zusätzlichen `Key` Eigenschaft.

Die `System.Linq` Namespace stellt eine Implementierung des Abfragemusters Operator für jeden Typ, der implementiert die `System.Collections.Generic.IEnumerable<T>` Schnittstelle.

## <a name="assignment-operators"></a>Zuweisungsoperatoren

Die Zuweisungsoperatoren zuweisen eine Variable, eine Eigenschaft, ein Ereignis oder ein Indexerelement einen neuen Wert.

```antlr
assignment
    : unary_expression assignment_operator expression
    ;

assignment_operator
    : '='
    | '+='
    | '-='
    | '*='
    | '/='
    | '%='
    | '&='
    | '|='
    | '^='
    | '<<='
    | right_shift_assignment
    ;
```

Der linke Operand einer Zuweisung muss ein Ausdruck, der als eine Variable, einen Eigenschaftenzugriff, Indexzugriff oder ein Ereigniszugriff klassifiziert sind.

Die `=` Operator wird aufgerufen, die ***einfacher Zuweisungsoperator***. Es weist den Wert des rechten Operanden, auf die Variable, Eigenschaft oder der Indexer-Element, durch den linken Operanden angegeben. Der linke Operand des der einfache Zuweisungsoperator möglicherweise kein Event-Zugriff (mit Ausnahme beschriebener in [Feldähnliche Ereignisse](classes.md#field-like-events)). Der einfache Zuweisungsoperator finden Sie im [einfache Zuweisung](expressions.md#simple-assignment).

Die Zuweisungsoperatoren außer der `=` Operator heißen die ***zusammensetzungszuweisungsoperatoren***. Diese Operatoren den angegebenen Vorgang für die beiden Operanden ausführen, und klicken Sie dann den resultierenden Wert zuweisen, auf die Variable, Eigenschaft oder der Indexer-Element, durch den linken Operanden angegeben. Die zusammengesetzten Zuweisungsoperatoren werden in beschrieben [Verbundzuweisung](expressions.md#compound-assignment).

Die `+=` und `-=` Operatoren mit einem Event-Access-Ausdruck wie der linke Operand heißen die *Ereignis Zuweisungsoperatoren*. Keine anderen Zuweisungsoperator ist wie der linke Operand ein Ereigniszugriff gültig. Die Ereignis-Zuweisungsoperatoren werden in beschrieben [Ereignis Zuweisung](expressions.md#event-assignment).

Die Zuweisungsoperatoren sind rechtsassoziativ, was bedeutet, dass Vorgänge, die von rechts nach links gruppiert werden. Z. B. ein Ausdruck der Form `a = b = c` wird als ausgewertet, `a = (b = c)`.

### <a name="simple-assignment"></a>Einfache Zuweisung

Die `=` Operator wird den einfache Zuweisungsoperator aufgerufen.

Wenn der linke Operand, der eine einfache Zuweisung des Formulars ist `E.P` oder `E[Ei]` , in denen `E` weist den Typ der Kompilierzeit `dynamic`, und klicken Sie dann die Zuweisung dynamisch gebunden ist ([dynamische Bindung](expressions.md#dynamic-binding)). In diesem Fall den Kompilierzeittyp des Zuweisungsausdrucks ist `dynamic`, und die unten beschriebene Lösung wird zur Laufzeit basierend auf den Laufzeittyp der `E`.

In einer einfachen Zuweisung muss der Rechte Operand ein Ausdruck sein, der implizit in den Typ des linken Operanden ist. Der Vorgang weist den Wert des rechten Operanden auf die Variable, Eigenschaft oder der Indexer-Element, durch den linken Operanden angegeben.

Das Ergebnis eines Ausdrucks für die einfache Zuweisung wird der Wert, der linke Operand zugewiesen. Das Ergebnis weist den gleichen Typ wie der linke Operand, und es wird immer als Wert klassifiziert.

Wenn der linke Operand eine Eigenschaft oder der Indexer-Zugriff ist, Eigenschaft oder des Indexers benötigen eine `set` Accessor. Wenn dies nicht der Fall ist, tritt ein Fehler während der Bindung.

Die Verarbeitung zur Laufzeit eine einfache Zuweisung des Formulars `x = y` umfasst die folgenden Schritte aus:

*  Wenn `x` wird als Variable klassifiziert:
   * `x` wird ausgewertet, um die Variable zu erstellen.
   * `y` wird ausgewertet und, falls erforderlich, konvertiert in den Typ des `x` über eine implizite Konvertierung ([implizite Konvertierungen](conversions.md#implicit-conversions)).
   * Wenn die Variable, die vom `x` ist ein Arrayelement einen *Reference_type*, eine laufzeitüberprüfung ausgeführt, um sicherzustellen, dass der Wert für berechnet `y` ist kompatibel mit der Arrayinstanz, von denen `x` ist ein Element. Die Überprüfung ist erfolgreich, wenn `y` ist `null`, oder wenn implizite verweiskonvertierung ([implizite Verweis-](conversions.md#implicit-reference-conversions)) vorhanden ist, aus dem tatsächlichen Typ der Instanz verweist `y` in den tatsächlichen Elementtyp der Instanz für das Array mit `x`. Andernfalls wird eine `System.ArrayTypeMismatchException` ausgelöst.
   * Der Wert der Auswertung und Konvertierung von `y` befindet sich in den Speicherort durch die Auswertung von `x`.
*  Wenn `x` wird als eine Eigenschaft oder der Indexer Zugriff klassifiziert:
   * Der Instanzausdruck (Wenn `x` ist nicht `static`) und die Argumentliste (Wenn `x` Indexzugriff ist) zugeordneten `x` werden ausgewertet, und die Ergebnisse werden in der nachfolgenden verwendet `set` Accessor-Aufruf.
   * `y` wird ausgewertet und, falls erforderlich, konvertiert in den Typ des `x` über eine implizite Konvertierung ([implizite Konvertierungen](conversions.md#implicit-conversions)).
   * Die `set` Accessor `x` wird aufgerufen, mit dem Wert für berechnet `y` als seine `value` Argument.

Die Array-Co Varianz-Regeln ([Array-Kovarianz](arrays.md#array-covariance)) zulassen den Wert eines Arraytyps `A[]` sollen einen Verweis auf eine Instanz des Arraytyps `B[]`, sofern eine implizite verweiskonvertierung von vorhanden ist `B` zu `A`. Aufgrund dieser Regeln, die Zuweisung an ein Arrayelement einen *Reference_type* erfordert eine laufzeitüberprüfung aus, um sicherzustellen, dass der zugewiesene Wert mit dem Array-Instanz kompatibel ist. Im Beispiel
```csharp
string[] sa = new string[10];
object[] oa = sa;

oa[0] = null;               // Ok
oa[1] = "Hello";            // Ok
oa[2] = new ArrayList();    // ArrayTypeMismatchException
```
bewirkt, dass die letzte Zuweisung einer `System.ArrayTypeMismatchException` ausgelöst werden, da eine Instanz von `ArrayList` kann nicht gespeichert werden, in einem Element des eine `string[]`.

Wenn eine Eigenschaft oder der Indexer in deklariert eine *Struct_type* ist das Ziel einer Zuweisung, die den Instanzausdruck, die mit der Eigenschaft verknüpften oder Indexerzugriff muss als Variable klassifiziert werden. Wenn der Instanzausdruck als Wert klassifiziert ist, tritt ein Fehler während der Bindung. Aufgrund der [Memberzugriff](expressions.md#member-access), die gleiche Regel gilt auch für Felder.

Betrachten Sie die Deklarationen:
```csharp
struct Point
{
    int x, y;

    public Point(int x, int y) {
        this.x = x;
        this.y = y;
    }

    public int X {
        get { return x; }
        set { x = value; }
    }

    public int Y {
        get { return y; }
        set { y = value; }
    }
}

struct Rectangle
{
    Point a, b;

    public Rectangle(Point a, Point b) {
        this.a = a;
        this.b = b;
    }

    public Point A {
        get { return a; }
        set { a = value; }
    }

    public Point B {
        get { return b; }
        set { b = value; }
    }
}
```
Im Beispiel
```csharp
Point p = new Point();
p.X = 100;
p.Y = 100;
Rectangle r = new Rectangle();
r.A = new Point(10, 10);
r.B = p;
```
die Zuweisungen zu `p.X`, `p.Y`, `r.A`, und `r.B` sind zulässig, da `p` und `r` Variablen sind. Im Beispiel allerdings
```csharp
Rectangle r = new Rectangle();
r.A.X = 10;
r.A.Y = 10;
r.B.X = 100;
r.B.Y = 100;
```
die Zuweisungen sind ungültig, da `r.A` und `r.B` sind keine Variablen.

### <a name="compound-assignment"></a>Verbundzuweisung

Wenn der linke Operand des eine zusammengesetzte Zuweisung des Formulars ist `E.P` oder `E[Ei]` , in denen `E` weist den Typ während der Kompilierung `dynamic`, und klicken Sie dann die Zuweisung dynamisch gebunden ist ([dynamische Bindung](expressions.md#dynamic-binding)). In diesem Fall den Kompilierzeittyp des Zuweisungsausdrucks ist `dynamic`, und die unten beschriebene Lösung wird zur Laufzeit basierend auf den Laufzeittyp der `E`.

Ein Vorgang des Formulars `x op= y` wird durch Anwenden der Auflösung von funktionsüberladungen binärer Operator verarbeitet ([binärer Operator überladungsauflösung](expressions.md#binary-operator-overload-resolution)), als ob der Vorgang geschrieben wurde `x op y`. Klicken Sie dann

*  Wenn der Rückgabetyp des ausgewählten Operators implizit in den Typ des `x`, der Vorgang wird als ausgewertet `x = x op y`, außer dass `x` wird nur einmal ausgewertet.
*  Andernfalls, wenn der ausgewählte Operator einen vordefinierten Operator ist, ist der Rückgabetyp von den ausgewählten Operator explizit konvertiert werden kann, in den Typ des `x`, und wenn `y` wird implizit in den Typ des `x` oder der Operator ist ein Shift-Operator, und klicken Sie dann der Vorgang als ausgewertet wird `x = (T)(x op y)`, wobei `T` ist der Typ des `x`, außer dass `x` wird nur einmal ausgewertet.
*  Andernfalls die zusammengesetzte Zuweisung ist ungültig, und beim Auftreten eines Laufzeitfehlers-Bindung.

Der Begriff "nur einmal ausgewertet" bedeutet, dass bei der Auswertung `x op y`, die Ergebnisse der enthaltenen Ausdrücke von `x` vorübergehend gespeichert und dann wiederverwendet werden, bei der Zuweisung zu `x`. Z. B. in der Zuweisung `A()[B()] += C()`, wobei `A` ist eine Methode zurückgeben `int[]`, und `B` und `C` sind Methoden, die zurückgeben `int`, die Methoden werden nur einmal aufgerufen, in der Reihenfolge `A`, `B`, `C`.

Wenn der linke Operand des eine zusammengesetzte Zuweisung einer Eigenschaft oder Indexzugriff ist, Eigenschaft oder des Indexers benötigen Sie Folgendes ein `get` Accessor und einen `set` Accessor. Wenn dies nicht der Fall ist, tritt ein Fehler während der Bindung.

Die zweite Regel oben ermöglicht `x op= y` als auszuwertenden `x = (T)(x op y)` in bestimmten Kontexten. Die Regel vorhanden ist, dass die vordefinierten Operatoren an, die als zusammengesetzte Operatoren verwendet werden können, wenn der linke Operand vom Typ `sbyte`, `byte`, `short`, `ushort`, oder `char`. Auch wenn beide Argumente einer dieser Typen sind, die vordefinierten Operatoren erzeugt ein Ergebnis vom Typ `int`, wie in beschrieben [binären numerischen heraufstufungen](expressions.md#binary-numeric-promotions). Daher ohne eine Umwandlung wäre nicht möglich, einem der linke Operand das Ergebnis zuzuweisen es.

Die intuitive Auswirkung der Regel für die vordefinierten Operatoren ist einfach, `x op= y` ist zulässig, wenn sowohl der `x op y` und `x = y` sind zulässig. Im Beispiel
```csharp
byte b = 0;
char ch = '\0';
int i = 0;

b += 1;             // Ok
b += 1000;          // Error, b = 1000 not permitted
b += i;             // Error, b = i not permitted
b += (byte)i;       // Ok

ch += 1;            // Error, ch = 1 not permitted
ch += (char)1;      // Ok
```
die Fehlerursache für jeden Fehler ist, dass eine entsprechende einfache Zuweisung auch ein Fehler gewesen wäre.

Dies bedeutet auch, dass die verbundzuweisung, in die Vorgänge unterstützen Vorgänge aufgehoben. Im Beispiel
```csharp
int? i = 0;
i += 1;             // Ok
```
der transformierten Operator `+(int?,int?)` verwendet wird.

### <a name="event-assignment"></a>Ereignis-Zuweisung

Wenn der linke Operand des eine `+=` oder `-=` Operator wird als Ereigniszugriff klassifiziert, und der Ausdruck wird wie folgt ausgewertet:

*  Der Instanzausdruck wird ggf. von der Ereigniszugriff ausgewertet.
*  Der Rechte Operand des der `+=` oder `-=` Operator wird ausgewertet, und, falls erforderlich, konvertiert in den Typ des linken Operanden über eine implizite Konvertierung ([implizite Konvertierungen](conversions.md#implicit-conversions)).
*  Ein Ereignisaccessor des Ereignisses aufgerufen wird, mit der Argumentliste, die mit dem rechten Operanden ist, nach der Auswertung und, falls erforderlich, Konvertierung. Wenn der Operator war `+=`, `add` -Accessor wird aufgerufen, wenn der Operator war `-=`, `remove` -Accessor wird aufgerufen.

Zuweisungsausdruck Ereignis ergibt sich nicht auf einen Wert aus. Daher Zuweisungsausdruck Ereignis nur im Kontext gültig ist eine *Statement_expression* ([ausdrucksanweisungen](statements.md#expression-statements)).

## <a name="expression"></a>Ausdruck

Ein *Ausdruck* ist entweder ein *Non_assignment_expression* oder *Zuweisung*.

```antlr
expression
    : non_assignment_expression
    | assignment
    ;

non_assignment_expression
    : conditional_expression
    | lambda_expression
    | query_expression
    ;
```

## <a name="constant-expressions"></a>Konstante Ausdrücke

Ein *Constant_expression* ist ein Ausdruck, der zum Zeitpunkt der Kompilierung vollständig ausgewertet werden kann.

```antlr
constant_expression
    : expression
    ;
```

Es muss ein konstanter Ausdruck sein. die `null` Literal oder einen Wert mit einem der folgenden Typen: `sbyte`, `byte`, `short`, `ushort`, `int`, `uint`, `long`, `ulong`, `char` , `float`, `double`, `decimal`, `bool`, `object`, `string`, oder ein Enumerationstyp. Nur die folgenden Konstrukte sind in Konstanten Ausdrücken zulässig:

*  Literale (einschließlich der `null` literal).
*  Verweise auf `const` Member der Klasse und Strukturtypen.
*  Verweise auf Member von Enumerationstypen.
*  Verweise auf `const` Parameter oder lokale Variablen
*  Untergeordnete Ausdrücke in Klammern, die Konstante Ausdrücke sind.
*  CAST-Ausdrücke, die den Typ des bereitgestellten ist einer der oben aufgeführten Typen.
*  `checked` und `unchecked` Ausdrücke
*  Ausdrücke mit Standardwert
*  Nameof-Ausdrücke
*  Die vordefinierten `+`, `-`, `!`, und `~` unäre Operatoren.
*  Die vordefinierten `+`, `-`, `*`, `/`, `%`, `<<`, `>>`, `&`, `|`, `^`, `&&`, `||`, `==`, `!=`, `<`, `>`, `<=`, und `>=` binäre Operatoren bereitgestellten jeder Operand ist ein Typ, der oben aufgeführten.
*  Die `?:` bedingter Operator.

Die folgenden Konvertierungen sind in Konstanten Ausdrücken zulässig:

*  Identity-Konvertierungen
*  Numerische Konvertierungen
*  Enumeration-Konvertierungen
*  Konstanter Ausdruck-Konvertierungen
*  Implizite und explizite Verweis-, vorausgesetzt, dass die Quelle der Konvertierungen ein konstanter Ausdruck ist, der den null-Wert ergibt.

Andere Konvertierungen, einschließlich Boxing, sind unboxing und implizite Verweis-, der nicht-Null-Werte in Konstanten Ausdrücken nicht zulässig. Zum Beispiel:
```csharp
class C {
    const object i = 5;         // error: boxing conversion not permitted
    const object str = "hello"; // error: implicit reference conversion
}
```
die Initialisierung von i ist ein Fehler auf, da eine Boxing-Konvertierung erforderlich ist. Die Initialisierung von str ist ein Fehler auf, da implizite verweiskonvertierung aus einem Wert ungleich Null erforderlich ist.

Wenn ein Ausdruck die oben aufgeführten Anforderungen erfüllt, wird der Ausdruck zur Kompilierzeit ausgewertet. Dies gilt auch, wenn der Ausdruck einen Unterausdruck eines umfassenderen Ausdrucks ist, die nicht Konstante Konstrukte enthält.

Die Auswertung der Kompilierzeit konstanter Ausdrücke verwendet die gleichen Regeln als laufzeitauswertung von Ausdrücken für nicht-Konstante, außer dass bewirkt, einen Fehler während der Kompilierung auftreten dass, in denen laufzeitauswertung ausgelöst worden wäre, eine Ausnahme, die kompilierzeitauswertung.

Es sei denn, Sie befindet sich ein konstanter Ausdruck explizit in ein `unchecked` Kontext, während der Kompilierung Auswertung des Ausdrucks immer arithmetische Operationen für ganzzahlige Typen und Konvertierungen auftretenden verursachen Kompilierzeitfehler ([Konstante Ausdrücke](expressions.md#constant-expressions)).

Konstante Ausdrücke werden in den folgenden Kontexten auftreten. In diesen Kontexten und tritt auf, ein Fehler während der Kompilierung, wenn ein Ausdruck zum Zeitpunkt der Kompilierung vollständig ausgewertet werden kann.

*  Konstante Deklarationen ([Konstanten](classes.md#constants)).
*  Member-Enumerationsdeklarationen ([Enumerationsmember](enums.md#enum-members)).
*  Standardargumente Listen der formalen Parameter ([Methodenparameter](classes.md#method-parameters))
*  `case` Bezeichnungen von einem `switch` Anweisung ([der Switch-Anweisung](statements.md#the-switch-statement)).
*  `goto case` Anweisungen ([Goto-Anweisung](statements.md#the-goto-statement)).
*  Länge in einem Arrayerstellungsausdruck Dimension ([Array-Ausdrücke für die Arrayerstellung](expressions.md#array-creation-expressions)), die einen Initialisierer enthält.
*  Attribute ([Attribute](attributes.md)).

Eine implizite Konstantenausdruck-Konvertierung ([Konstantenausdruck für implizite Konvertierungen](conversions.md#implicit-constant-expression-conversions)) ermöglicht einen konstanten Ausdruck des Typs `int` zu konvertierenden `sbyte`, `byte`, `short`, `ushort`, `uint`, oder `ulong`, sofern der Wert des konstanten Ausdrucks innerhalb des Bereichs des Zieltyps liegt.

## <a name="boolean-expressions"></a>Boolesche Ausdrücke

Ein *Boolean_expression* ist ein Ausdruck ein Ergebnis vom Typ ergibt `bool`– entweder direkt oder durch Anwenden von `operator true` in bestimmten Kontexten wie im folgenden angegeben.

```antlr
boolean_expression
    : expression
    ;
```

Der bedingte kontrollausdruck einer *If_statement* ([If Anweisung](statements.md#the-if-statement)), *While_statement* ([die while-Anweisung](statements.md#the-while-statement)), *Do_statement* ([die Do-Anweisung](statements.md#the-do-statement)), oder *For_statement* ([der für die Anweisung](statements.md#the-for-statement)) ist eine *Boolean_ Ausdruck*. Der steuernde bedingten Ausdruck die `?:` Operator ([Bedingungsoperator](expressions.md#conditional-operator)) folgt den gleichen Regeln wie eine *Boolean_expression*, aber aus Gründen der Operator wird die Rangfolge klassifiziert als eine *Conditional_or_expression*.

Ein *Boolean_expression* `E` ist erforderlich, um einen Wert vom Typ zu erzeugen können `bool`wie folgt:

*  Wenn `E` wird implizit in `bool` und dann zur Laufzeit diese implizite Konvertierung angewendet wird.
*  Andernfalls unäroperator überladungsauflösung ([überladungsauflösung für unäroperator](expressions.md#unary-operator-overload-resolution)) wird verwendet, um eine eindeutige best-Implementierung des Operators finden `true` auf `E`, und diese Implementierung wird zur Laufzeit angewendet.
*  Wenn es wurde kein Operator gefunden wird, tritt ein Fehler während der Bindung.

Die `DBBool` Strukturtyp in [Datenbank booleschen Typ](structs.md#database-boolean-type) enthält ein Beispiel für einen Typ, der implementiert `operator true` und `operator false`.
