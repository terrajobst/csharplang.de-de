---
ms.openlocfilehash: f61039abd6bd557ac0ea625e6aac1c8bafa57b02
ms.sourcegitcommit: e134bb7058e9848120b93b345f96d6ac0cb8c815
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/17/2020
ms.locfileid: "71704089"
---
# <a name="expressions"></a>Ausdrücke

Ein Ausdruck ist eine Folge von Operatoren und Operanden. In diesem Kapitel werden die Syntax, die Reihenfolge der Auswertung von Operanden und Operatoren sowie die Bedeutung von Ausdrücken definiert.

## <a name="expression-classifications"></a>Ausdrucks Klassifizierungen

Ein Ausdruck ist eines der folgenden Elemente:

*  Ein Wert. Jeder Wert verfügt über einen zugeordneten Typ.
*  Eine Variable. Jede Variable verfügt über einen zugeordneten Typ, nämlich den deklarierten Typ der Variablen.
*  Ein Namespace. Ein Ausdruck mit dieser Klassifizierung kann nur auf der linken Seite eines *member_access* ([Member Access](expressions.md#member-access)) angezeigt werden. In jedem anderen Kontext verursacht ein Ausdruck, der als Namespace klassifiziert ist, einen Kompilierzeitfehler.
*  Ein Typ. Ein Ausdruck mit dieser Klassifizierung kann nur als linke Seite eines *member_access* ([Member Access](expressions.md#member-access)) oder als Operand für den `as`-Operator ([den as-Operator](expressions.md#the-as-operator)), den `is`-Operator ([der is-Operator](expressions.md#the-is-operator)) oder den `typeof`-Operator ([der typeof](expressions.md#the-typeof-operator)-Operator) angezeigt werden. In jedem anderen Kontext verursacht ein Ausdruck, der als Typ klassifiziert ist, einen Kompilierzeitfehler.
*  Eine Methoden Gruppe, bei der es sich um eine Reihe von überladenen Methoden handelt, die sich aus einer Element Suche ([Member Suche](expressions.md#member-lookup)) ergeben. Eine Methoden Gruppe kann über einen zugeordneten Instanzausdruck und eine zugeordnete Typargument Liste verfügen. Wenn eine Instanzmethode aufgerufen wird, wird das Ergebnis der Auswertung des Instanzausdrucks zur Instanz, die durch `this` ([dieser Zugriff](expressions.md#this-access)) dargestellt wird. Eine Methoden Gruppe ist in einem *invocation_expression* ([Aufruf Ausdrücke](expressions.md#invocation-expressions)), einem *delegate_creation_expression* ([delegaterstellungs Ausdruck](expressions.md#delegate-creation-expressions)) und als linke Seite eines is-Operators zulässig und kann implizit in einen kompatiblen Delegattyp ([Methoden Gruppen Konvertierungen](conversions.md#method-group-conversions)) konvertiert werden. In jedem anderen Kontext verursacht ein Ausdruck, der als Methoden Gruppe klassifiziert ist, einen Kompilierzeitfehler.
*  Ein NULL-Literale. Ein Ausdruck mit dieser Klassifizierung kann implizit in einen Verweistyp oder einen Werte zulässt-Typ konvertiert werden.
*  Eine anonyme Funktion. Ein Ausdruck mit dieser Klassifizierung kann implizit in einen kompatiblen Delegattyp oder Ausdrucks Strukturtyp konvertiert werden.
*  Ein Eigenschaften Zugriff. Jeder Eigenschaften Zugriff verfügt über einen zugeordneten Typ, nämlich den Typ der Eigenschaft. Außerdem kann ein Eigenschaften Zugriff über einen zugeordneten Instanzausdruck verfügen. Wenn ein Accessor (der `get` oder `set` Block) eines Instanzeigenschaft Zugriffs aufgerufen wird, wird das Ergebnis der Auswertung des Instanzausdrucks zur durch `this` dargestellten Instanz ([dieser Zugriff](expressions.md#this-access)).
*  Ein Ereignis Zugriff. Jedem Ereignis Zugriff ist ein Typ zugeordnet, nämlich der Typ des Ereignisses. Außerdem kann ein Ereignis Zugriff über einen zugeordneten Instanzausdruck verfügen. Ein Ereignis Zugriff kann als Linker Operand des `+=` und `-=` Operatoren ([Ereignis Zuweisung](expressions.md#event-assignment)) angezeigt werden. In jedem anderen Kontext verursacht ein Ausdruck, der als Ereignis Zugriff klassifiziert ist, einen Kompilierzeitfehler.
*  Ein Indexer-Zugriff. Jedem Indexer-Zugriff ist ein Typ zugeordnet, nämlich der Elementtyp des Indexers. Außerdem verfügt ein Indexer-Zugriff über einen zugeordneten Instanzausdruck und eine zugeordnete Argumentliste. Wenn ein Accessor (der `get` oder `set` Block) eines Indexer-Zugriffs aufgerufen wird, wird das Ergebnis der Auswertung des Instanzausdrucks zu der durch `this` dargestellten Instanz ([dieser Zugriff](expressions.md#this-access)), und das Ergebnis der Auswertung der Argumentliste wird zur Parameterliste des aufgerufenen.
*  Nichts. Dies tritt auf, wenn der Ausdruck ein Aufruf einer Methode mit dem Rückgabetyp `void`ist. Ein Ausdruck, der als Nothing klassifiziert ist, ist nur im Kontext einer *statement_expression* ([Ausdrucks Anweisungen](statements.md#expression-statements)) gültig.

Das Endergebnis eines Ausdrucks ist nie ein Namespace, ein Typ, eine Methoden Gruppe oder ein Ereignis Zugriff. Wie oben bereits erwähnt, sind diese Kategorien von Ausdrücken zwischenkonstrukte, die nur in bestimmten Kontexten zulässig sind.

Ein Eigenschaften Zugriff oder Indexer-Zugriff wird immer als Wert neu klassifiziert, indem ein Aufruf des *Get-Accessor* oder des *set*-Accessors ausgeführt wird. Der jeweilige Accessor wird durch den Kontext der Eigenschaft oder des Indexerzugriffs bestimmt: Wenn der Zugriff das Ziel einer Zuweisung ist, wird der *Set-Accessor* aufgerufen, um einen neuen Wert zuzuweisen ([einfache Zuweisung](expressions.md#simple-assignment)). Andernfalls wird der *Get-Accessor* aufgerufen, um den aktuellen Wert zu erhalten ([Werte von Ausdrücken](expressions.md#values-of-expressions)).

### <a name="values-of-expressions"></a>Werte von Ausdrücken

Die meisten Konstrukte, die einen Ausdruck einschließen, erfordern letztendlich, dass der Ausdruck einen ***Wert***angibt. In solchen Fällen tritt ein Kompilierzeitfehler auf, wenn der tatsächliche Ausdruck einen Namespace, einen Typ, eine Methoden Gruppe oder nichts angibt. Wenn der Ausdruck jedoch einen Eigenschaften Zugriff, einen Indexer-Zugriff oder eine Variable angibt, wird der Wert der Eigenschaft, des Indexers oder der Variablen implizit ersetzt:

*  Der Wert einer Variablen ist einfach der Wert, der zurzeit an dem von der Variablen identifizierten Speicherort gespeichert ist. Eine Variable muss als definitiv zugewiesen ([definitive Zuweisung](variables.md#definite-assignment)) betrachtet werden, bevor ihr Wert abgerufen werden kann. andernfalls tritt ein Kompilierzeitfehler auf.
*  Der Wert eines Eigenschafts Zugriffs Ausdrucks wird abgerufen, indem der *Get-Accessor* der Eigenschaft aufgerufen wird. Wenn die Eigenschaft keinen *Get-Accessor*aufweist, tritt ein Kompilierzeitfehler auf. Andernfalls wird ein Funktionsmember-Aufruf ([Kompilierzeit Überprüfung der dynamischen Überladungs Auflösung](expressions.md#compile-time-checking-of-dynamic-overload-resolution)) ausgeführt, und das Ergebnis des aufzurufenden Ausdrucks wird zum Wert des Eigenschafts Zugriffs Ausdrucks.
*  Der Wert eines indexerzugriffsausdrucks wird abgerufen, indem der *Get-Accessor* des Indexers aufgerufen wird. Wenn der Indexer keinen *Get-Accessor*aufweist, tritt ein Kompilierzeitfehler auf. Andernfalls wird ein Funktionsmember-Aufruf ([Kompilierzeit Überprüfung der dynamischen Überladungs Auflösung](expressions.md#compile-time-checking-of-dynamic-overload-resolution)) mit der Argumentliste ausgeführt, die dem indexerzugriffsausdruck zugeordnet ist, und das Ergebnis des aufzurufenden Ausdrucks wird zum Wert des Zugriffs Ausdrucks des Indexers.

## <a name="static-and-dynamic-binding"></a>Statische und dynamische Bindung

Der Prozess der Bestimmung der Bedeutung eines Vorgangs basierend auf dem Typ oder Wert von konstituierenden Ausdrücken (Argumente, Operanden, Empfänger) wird häufig als ***Bindung***bezeichnet. Zum Beispiel wird die Bedeutung eines Methoden Aufrufes basierend auf dem Typ des Empfängers und der Argumente bestimmt. Die Bedeutung eines Operators wird basierend auf dem Typ seiner Operanden bestimmt.

In C# der Bedeutung eines Vorgangs wird in der Regel zur Kompilierzeit bestimmt, basierend auf dem Kompilier Zeittyp der enthaltenen Ausdrücke. Wenn ein Ausdruck einen Fehler enthält, wird der Fehler auch vom Compiler erkannt und gemeldet. Diese Vorgehensweise wird als ***statische Bindung***bezeichnet.

Wenn ein Ausdruck jedoch ein dynamischer Ausdruck ist (d. h. den Typ `dynamic`), gibt dies an, dass jede Bindung, an der Sie teilnimmt, auf dem Lauf Zeittyp (d. h. dem tatsächlichen Typ des Objekts, das Sie zur Laufzeit bezeichnet) und nicht auf dem Typ, der zur Kompilierzeit vorhanden ist, basieren soll. Die Bindung eines solchen Vorgangs wird daher bis zu der Zeit verzögert, in der der Vorgang während der Ausführung des Programms ausgeführt werden soll. Dies wird als ***dynamische Bindung***bezeichnet.

Wenn ein Vorgang dynamisch gebunden ist, wird vom Compiler nur eine kleine oder keine Überprüfung durchgeführt. Wenn die Lauf Zeitbindung fehlschlägt, werden Fehler zur Laufzeit als Ausnahmen gemeldet.

Die folgenden Vorgänge in C# unterliegen der Bindung:

*  Mitgliederzugriff: `e.M`
*  Methodenaufruf: `e.M(e1, ..., eN)`
*  Delegataufruf:`e(e1, ..., eN)`
*  Element Zugriff: `e[e1, ..., eN]`
*  Objekt Erstellung: `new C(e1, ..., eN)`
*  Überladene unäre Operatoren: `+`, `-`, `!`, `~`, `++`, `--`, `true`, `false`
*  Überladene binäre Operatoren: `+`, `-`, `*`, `/`, `%`, `&`, `&&`, `|`, `||`, `??`, `^`, `<<`, `>>`, `==`,`!=`, `>`, `<`, `>=`, `<=`
*  Zuweisungs Operatoren: `=`, `+=`, `-=`, `*=`, `/=`, `%=`, `&=`, `|=`, `^=`, `<<=`
*  Implizite und explizite Konvertierungen

Wenn keine dynamischen Ausdrücke beteiligt sind, C# wird standardmäßig die statische Bindung verwendet. Dies bedeutet, dass die Kompilierzeit Typen von konstituierenden Ausdrücken im Auswahl Vorgang verwendet werden. Wenn jedoch einer der in den oben aufgeführten Vorgängen aufgeführten Ausdrücke ein dynamischer Ausdruck ist, wird der Vorgang stattdessen dynamisch gebunden.

### <a name="binding-time"></a>Bindungs Zeit

Die statische Bindung erfolgt zur Kompilierzeit, während die dynamische Bindung zur Laufzeit stattfindet. In den folgenden Abschnitten bezieht sich der Begriff " ***Bindungs Zeit*** " entweder auf die Kompilierzeit oder die Laufzeit, je nachdem, wann die Bindung stattfindet.

Das folgende Beispiel veranschaulicht die Begriffe der statischen und dynamischen Bindung und der Bindungs Zeit:
```csharp
object  o = 5;
dynamic d = 5;

Console.WriteLine(5);  // static  binding to Console.WriteLine(int)
Console.WriteLine(o);  // static  binding to Console.WriteLine(object)
Console.WriteLine(d);  // dynamic binding to Console.WriteLine(int)
```

Die ersten beiden Aufrufe sind statisch gebunden: die Überladung von `Console.WriteLine` wird basierend auf dem Kompilier Zeittyp ihres Arguments ausgewählt. Folglich ist die Bindungs Zeit die Kompilierzeit.

Der dritte Aufruf ist dynamisch gebunden: die Überladung von `Console.WriteLine` wird basierend auf dem Lauf Zeittyp des Arguments ausgewählt. Dies liegt daran, dass es sich bei dem Argument um einen dynamischen Ausdruck handelt, dessen Kompilier Zeittyp `dynamic`ist. Folglich ist die Bindungs Zeit für den dritten Aufruf Lauf Zeit.

### <a name="dynamic-binding"></a>Dynamische Bindung

Der Zweck der dynamischen Bindung besteht darin, C# den Programmen die Interaktion mit ***dynamischen Objekten***zu ermöglichen, d. h. Objekten, die nicht den C# normalen Regeln des Typsystems entsprechen. Dynamische Objekte können Objekte aus anderen Programmiersprachen mit unterschiedlichen Typen Systemen sein, oder es handelt sich um Objekte, die Programm gesteuert eingerichtet werden, um Ihre eigene Bindungs Semantik für verschiedene Vorgänge zu implementieren.

Der Mechanismus, mit dem ein dynamisches Objekt seine eigene Semantik implementiert, ist die Implementierung definiert. Eine bestimmte Schnittstelle, die wieder implementiert wird, wird von dynamischen Objekten implementiert, um der C# Laufzeit zu signalisieren, dass Sie über eine besondere Semantik verfügen. Wenn also Vorgänge für ein dynamisches Objekt dynamisch gebunden werden, übernehmen Sie die eigene Bindungs Semantik anstelle der C# in diesem Dokument angegebenen.

Der Zweck der dynamischen Bindung besteht darin, die Interoperabilität mit dynamischen Objekten zuzulassen, C# aber die dynamische Bindung für alle Objekte, unabhängig davon, ob Sie dynamisch sind oder nicht. Dies ermöglicht eine reibungslosere Integration dynamischer Objekte, da die Ergebnisse von Vorgängen für diese nicht selbst dynamische Objekte sein können, aber immer noch ein Typ ist, der dem Programmierer zur Kompilierzeit unbekannt ist. Außerdem kann die dynamische Bindung helfen, fehleranfälligen reflektionsbasierten Code zu eliminieren, auch wenn keine Objekte an dynamischen Objekten beteiligt sind.

In den folgenden Abschnitten wird für jedes Konstrukt in der Sprache exakt beschrieben, wann die dynamische Bindung angewendet wird, welche Kompilierzeit Überprüfung (sofern vorhanden) angewendet wird und wie das Ergebnis und die Ausdrucks Klassifizierung der Kompilierung ist.

### <a name="types-of-constituent-expressions"></a>Typen von konstituierenden Ausdrücken

Wenn ein Vorgang statisch gebunden ist, wird der Typ eines einzelnen Ausdrucks (z. b. ein Empfänger, ein Argument, ein Index oder ein Operand) immer als der Kompilier Zeittyp dieses Ausdrucks betrachtet.

Wenn ein Vorgang dynamisch gebunden wird, wird der Typ eines einzelnen Ausdrucks basierend auf dem Kompilier Zeittyp des konstituierenden Ausdrucks auf unterschiedliche Weise bestimmt:

*  Ein konstituierender Ausdruck der Kompilier Zeittyp `dynamic` wird als der Typ des tatsächlichen Werts betrachtet, den der Ausdruck zur Laufzeit auswertet.
*  Ein konstituierender Ausdruck, dessen Kompilier Zeittyp ein Typparameter ist, wird als Typ betrachtet, an den der Typparameter zur Laufzeit gebunden ist.
*  Andernfalls wird der entsprechende Kompilier Zeittyp für den enthaltenen Ausdruck berücksichtigt.

## <a name="operators"></a>Operatoren

Ausdrücke werden aus ***Operanden*** und ***Operatoren***erstellt. Die Operatoren eines Ausdrucks geben an, welche Operationen auf die Operanden angewendet werden. Beispiele für Operatoren sind `+`, `-`, `*`, `/` und `new`. Beispiele für Operanden sind Literale, Felder, lokale Variablen und Ausdrücke.

Es gibt drei Arten von Operatoren:

*  Unäre Operatoren. Die unären Operatoren nehmen einen der Operanden an und verwenden entweder eine Präfix Notation (z. b. `--x`) oder eine Postfix Notation (z. b. `x++`).
*  Binäre Operatoren. Die binären Operatoren nehmen zwei Operanden an und verwenden die Infix-Notation (z. b. `x + y`).
*  Ternärer Operator. Es ist nur ein ternärer Operator, `?:`, vorhanden. Es werden drei Operanden benötigt, und die Infix-Notation (`c ? x : y`) wird verwendet.

Die Reihenfolge der Auswertung von Operatoren in einem Ausdruck wird durch die ***Rangfolge*** und ***Assoziativität*** der Operatoren ([Operator Rangfolge und Assoziativität](expressions.md#operator-precedence-and-associativity)) bestimmt.

Operanden in einem Ausdruck werden von links nach rechts ausgewertet. Beispielsweise wird in `F(i) + G(i++) * H(i)`die Methode `F` mit dem alten Wert von `i`aufgerufen. Anschließend wird Method `G` mit dem alten Wert von `i`aufgerufen, und schließlich wird die Methode `H` mit dem neuen Wert `i`aufgerufen. Dies ist getrennt von und nicht mit der Operator Rangfolge.

Bestimmte Operatoren können ***überladen*** werden. Die Operator Überladung ermöglicht das Angeben von benutzerdefinierten Operator Implementierungen für Vorgänge, bei denen einer oder beide der Operanden eine benutzerdefinierte Klasse oder ein Strukturtyp sind ([Operator Überladung](expressions.md#operator-overloading)).

### <a name="operator-precedence-and-associativity"></a>Operatorrangfolge und Assoziativität

Wenn ein Ausdruck mehrere Operatoren enthält, bestimmt die ***Rangfolge*** der Operatoren die Reihenfolge, in der die einzelnen Operatoren ausgewertet werden. Beispielsweise wird der Ausdruck `x + y * z` als `x + (y * z)` ausgewertet, da der `*`-Operator eine höhere Rangfolge aufweist als der binäre `+`-Operator. Die Rangfolge eines Operators wird durch die Definition der zugehörigen Grammatik Produktion festgelegt. Ein *additive_expression* besteht z. b. aus einer Sequenz von *multiplicative_expression*s, die durch `+` oder `-` Operatoren getrennt sind, sodass die `+`-und `-` Operatoren Vorrang vor den Operatoren `*`, `/`und `%` haben.

In der folgenden Tabelle werden alle Operatoren in der Rangfolge von der höchsten zur niedrigsten aufgeführt:

| __Section__                                                                                   | __Kategorie__                | __Operatoren__ | 
|-----------------------------------------------------------------------------------------------|-----------------------------|---------------|
| [Primary expressions (Primäre Ausdrücke)](expressions.md#primary-expressions)                                     | Primary                     | `x.y`  `f(x)`  `a[x]`  `x++`  `x--`  `new`  `typeof`  `default`  `checked`  `unchecked`  `delegate` | 
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
| [Zuweisungs Operatoren](expressions.md#assignment-operators), [Anonyme Funktions Ausdrücke](expressions.md#anonymous-function-expressions)  | Zuweisungs- und Lambda-Ausdrücke | `=`  `*=`  `/=`  `%=`  `+=`  `-=`  `<<=`  `>>=`  `&=`  `^=`  <code>&#124;=</code>  `=>` | 

Wenn ein Operand zwischen zwei Operatoren mit der gleichen Rangfolge auftritt, steuert die Assoziativität der Operatoren die Reihenfolge, in der die Vorgänge ausgeführt werden:

*  Mit Ausnahme der Zuweisungs Operatoren und des NULL-Sammel Operators sind alle binären Operatoren ***Links***bündig. Dies bedeutet, dass Vorgänge von links nach rechts ausgeführt werden. `x + y + z` wird beispielsweise als `(x + y) + z`ausgewertet.
*  Die Zuweisungs Operatoren, der NULL-Sammel Operator und der bedingte Operator (`?:`) sind ***Rechts assoziativ***, was bedeutet, dass Vorgänge von rechts nach links ausgeführt werden. `x = y = z` wird beispielsweise als `x = (y = z)`ausgewertet.

Rangfolge und Assoziativität können mit Klammern gesteuert werden. In `x + y * z` wird beispielsweise zuerst `y` mit `z` multipliziert und dann das Ergebnis zu `x` addiert, aber in `(x + y) * z` werden zunächst `x` und `y` addiert, und dann wird das Ergebnis mit `z` multipliziert.

### <a name="operator-overloading"></a>Überladen von Operatoren

Alle unären und binären Operatoren verfügen über vordefinierte Implementierungen, die automatisch in jedem Ausdruck verfügbar sind. Zusätzlich zu den vordefinierten Implementierungen können benutzerdefinierte Implementierungen durch Einschließen von `operator` Deklarationen in Klassen und Strukturen ([Operatoren](classes.md#operators)) eingeführt werden. Implementierungen von benutzerdefinierten Operatoren haben immer Vorrang vor vordefinierten Operator Implementierungen: nur wenn keine anwendbaren benutzerdefinierten Operator Implementierungen vorhanden sind, werden die vordefinierten Operator Implementierungen in Erwägung gezogen, wie unter [unäre Operator Überladungs Auflösung](expressions.md#unary-operator-overload-resolution) und [binäre Operator Überladungs Auflösung](expressions.md#binary-operator-overload-resolution)beschrieben.

Die ***über ladbaren unären Operatoren*** sind:
```csharp
+   -   !   ~   ++   --   true   false
```

Obwohl `true` und `false` nicht explizit in Ausdrücken verwendet werden (und daher nicht in der Rang folgen Tabelle in der [Operator Rangfolge und Assoziativität](expressions.md#operator-precedence-and-associativity)enthalten sind), werden Sie als Operatoren angesehen, da Sie in mehreren Ausdrucks Kontexten aufgerufen werden: boolesche Ausdrücke ([boolesche Ausdrücke](expressions.md#boolean-expressions)) und Ausdrücke mit bedingtem ([bedingtem Operator](expressions.md#conditional-operator)) und bedingten logischen Operatoren ([bedingte logische Operatoren](expressions.md#conditional-logical-operators)).

Die ***über ladbaren binären Operatoren*** sind:
```csharp
+   -   *   /   %   &   |   ^   <<   >>   ==   !=   >   <   >=   <=
```

Nur die oben aufgeführten Operatoren können überladen werden. Insbesondere ist es nicht möglich, Element Zugriffe, Methodenaufrufe oder die Operatoren "`=`", "`&&`", "`||`", "`??`", "`?:`", "`=>`", "`checked`", "`unchecked`", "`new`" und "`typeof`" zu überladen.

Wenn ein binärer Operator überladen ist, wird der zugehörige Zuweisungsoperator, sofern er vorhanden ist, auch implizit überladen. Beispielsweise ist eine Überladung des Operators `*` auch eine Überladung des Operator `*=`. Dies wird in der [Verbund Zuweisung](expressions.md#compound-assignment)weiter unten beschrieben. Beachten Sie, dass der Zuweisungs Operator selbst (`=`) nicht überladen werden kann. Eine Zuweisung führt immer eine einfache bitweise Kopie eines Werts in eine Variable aus.

Umwandlungs Vorgänge, wie z. b. `(T)x`, werden durch die Bereitstellung von benutzerdefinierten Konvertierungen ([benutzerdefinierte Konvertierungen](conversions.md#user-defined-conversions)) überladen.

Der Element Zugriff, z. b. `a[x]`, wird nicht als über ladbarer Operator angesehen. Stattdessen wird die benutzerdefinierte Indizierung durch Indexer ([Indexer](classes.md#indexers)) unterstützt.

In Ausdrücken wird mithilfe der Operator Notation auf Operatoren verwiesen, und in Deklarationen wird auf Operatoren mithilfe der funktionalen Notation verwiesen. In der folgenden Tabelle wird die Beziehung zwischen Operator-und Funktions Notizen für unäre und binäre Operatoren veranschaulicht. Im ersten Eintrag bezeichnet *op* alle über ladbaren unären Präfix Operatoren. Im zweiten Eintrag bezeichnet *op* die unären postfix-`++` und `--` Operatoren. Im dritten Eintrag bezeichnet *op* jeden über ladbaren binären Operator.


| __Operator Notation__ | __Funktionale Notation__ |
|-----------------------|-------------------------|
| `op x`                | `operator op(x)`        | 
| `x op`                | `operator op(x)`        | 
| `x op y`              | `operator op(x,y)`      | 

Benutzerdefinierte Operator Deklarationen erfordern immer, dass mindestens einer der Parameter vom Klassen-oder Strukturtyp ist, der die Operator Deklaration enthält. Daher ist es nicht möglich, dass ein benutzerdefinierter Operator dieselbe Signatur wie ein vordefinierter Operator hat.

Benutzerdefinierte Operator Deklarationen können die Syntax, Rangfolge oder Assoziativität eines Operators nicht ändern. Beispielsweise ist der `/`-Operator immer ein binärer Operator, verfügt immer über die in [Operator Rangfolge und Assoziativität](expressions.md#operator-precedence-and-associativity)angegebene Rang folgen Ebene und ist immer links assoziativ.

Obwohl es möglich ist, dass ein benutzerdefinierter Operator jede beliebige Berechnung durchführt, wird dringend davon abgeraten, Implementierungen, die andere Ergebnisse als diejenigen ergeben, die intuitiv erwartet werden. Beispielsweise sollte eine Implementierung von `operator ==` die beiden Operanden auf Gleichheit vergleichen und ein entsprechendes `bool` Ergebnis zurückgeben.

Die Beschreibungen der einzelnen Operatoren in [Primary Expressions](expressions.md#primary-expressions) durch [bedingte logische Operatoren](expressions.md#conditional-logical-operators) geben die vordefinierten Implementierungen der Operatoren sowie alle zusätzlichen Regeln an, die für die einzelnen Operatoren gelten. In den Beschreibungen werden die Begriffe ***unäre Operator Überladungs Auflösung***, ***binäre Operator Überladungs Auflösung***und ***numerische***herauf Stufung verwendet. diese Definitionen finden Sie in den folgenden Abschnitten.

### <a name="unary-operator-overload-resolution"></a>Überladungs Auflösung für unären Operator

Ein Vorgang der Form `op x` oder `x op`, bei dem `op` ein über ladbarer unärer Operator ist und `x` ein Ausdruck vom Typ `X`ist, wird wie folgt verarbeitet:

*  Der Satz von benutzerdefinierten Operatoren, die von `X` für den Vorgang bereitgestellt werden `operator op(x)` wird mithilfe der Regeln von [benutzerdefinierten Operatoren des Kandidaten](expressions.md#candidate-user-defined-operators)bestimmt.
*  Wenn der Satz von benutzerdefinierten Operatoren des Kandidaten nicht leer ist, wird dies zur Gruppe der Kandidaten Operatoren für den Vorgang. Andernfalls werden die vordefinierten unären `operator op` Implementierungen, einschließlich ihrer angehobenen Formulare, zur Gruppe der Kandidaten Operatoren für den Vorgang. Die vordefinierten Implementierungen eines angegebenen Operators werden in der Beschreibung des Operators ([primär Ausdrücke](expressions.md#primary-expressions) und [unäre Operatoren](expressions.md#unary-operators)) angegeben.
*  Die Regeln zur Überladungs Auflösung der [Überladungs Auflösung](expressions.md#overload-resolution) werden auf den Satz von Kandidaten Operatoren angewendet, um den besten Operator in Bezug auf die Argumentliste `(x)`auszuwählen, und dieser Operator wird zum Ergebnis des Überladungs Auflösungsprozesses. Wenn bei der Überladungs Auflösung nicht der einzige beste Operator ausgewählt werden kann, tritt ein Bindungs Zeitfehler auf.

### <a name="binary-operator-overload-resolution"></a>Binäre Operator Überladungs Auflösung

Ein Vorgang der Form `x op y`, bei dem `op` ein über ladbarer binärer Operator ist, `x` ein Ausdruck vom Typ `X`ist, und `y` ein Ausdruck vom Typ `Y`ist, wird wie folgt verarbeitet:

*  Der Satz von benutzerdefinierten Operatoren, die von `X` bereitgestellt werden und `Y` für den Vorgang `operator op(x,y)` bestimmt wird. Der Satz besteht aus der Vereinigung der Kandidaten Operatoren, die von `X` bereitgestellt werden, und den Kandidaten Operatoren, die von `Y`bereitgestellt werden, die jeweils mit den Regeln von [benutzerdefinierten Operatoren des Kandidaten](expressions.md#candidate-user-defined-operators)bestimmt Wenn `X` und `Y` denselben Typ haben oder wenn `X` und `Y` von einem gemeinsamen Basistyp abgeleitet sind, treten freigegebene Kandidaten Operatoren nur einmal in der kombinierten Menge auf.
*  Wenn der Satz von benutzerdefinierten Operatoren des Kandidaten nicht leer ist, wird dies zur Gruppe der Kandidaten Operatoren für den Vorgang. Andernfalls werden die vordefinierten Binär `operator op`-Implementierungen, einschließlich ihrer angehobenen Formulare, zur Gruppe der Kandidaten Operatoren für den Vorgang. Die vordefinierten Implementierungen eines angegebenen Operators werden in der Beschreibung des Operators ([arithmetische Operatoren](expressions.md#arithmetic-operators) durch [bedingte logische Operatoren](expressions.md#conditional-logical-operators)) angegeben. Bei vordefinierten Aufzählungs-und delegatoperatoren sind die einzigen Operatoren, die von einem Aufzählungs-oder Delegattyp definiert werden, der der Bindungstyp von einem der Operanden ist.
*  Die Regeln zur Überladungs Auflösung der [Überladungs Auflösung](expressions.md#overload-resolution) werden auf den Satz von Kandidaten Operatoren angewendet, um den besten Operator in Bezug auf die Argumentliste `(x,y)`auszuwählen, und dieser Operator wird zum Ergebnis des Überladungs Auflösungsprozesses. Wenn bei der Überladungs Auflösung nicht der einzige beste Operator ausgewählt werden kann, tritt ein Bindungs Zeitfehler auf.

### <a name="candidate-user-defined-operators"></a>Benutzerdefinierte Operatoren für Kandidaten

Bei Angabe eines Typs `T` und eines Vorgangs `operator op(A)`, bei dem `op` ein über ladbarer Operator ist und `A` eine Argumentliste ist, wird der Satz von benutzerdefinierten Operatoren, die von `T` für `operator op(A)` bereitgestellt werden, wie folgt bestimmt:

*  Bestimmen Sie den Typ `T0`. Wenn `T` ein Typ ist, der NULL-Werte zulässt, ist `T0` der zugrunde liegende Typ, andernfalls ist `T0` gleich `T`.
*  Wenn mindestens ein Operator (anwendbarer Funktionsmember) in Bezug auf die Argumentliste `A`anwendbar ist, `T0`besteht für alle `operator op` Deklarationen in `T0` und allen aufzurufenden Formen solcher Operatoren, wenn mindestens ein Operator anwendbar ist ([anwendbarer Funktionsmember](expressions.md#applicable-function-member)).
*  Andernfalls ist der Satz von Kandidaten Operatoren leer, wenn `T0` `object`ist.
*  Andernfalls ist der Satz von Kandidaten Operatoren, der von `T0` bereitgestellt wird, die Menge der Kandidaten Operatoren, die von der direkten Basisklasse `T0`bereitgestellt werden, oder die effektive Basisklasse von `T0`, wenn `T0` ein Typparameter ist.

### <a name="numeric-promotions"></a>Numerische Aktionen

Die numerische herauf Stufung besteht aus dem automatischen Ausführen bestimmter impliziter Konvertierungen der Operanden der vordefinierten unären und binären numerischen Operatoren. Die numerische herauf Stufung ist kein eindeutiger Mechanismus, sondern wirkt sich eher auf die Anwendung der Überladungs Auflösung auf die vordefinierten Operatoren aus. Die numerische herauf Stufung wirkt sich nicht auf die Auswertung von benutzerdefinierten Operatoren aus, obwohl benutzerdefinierte Operatoren implementiert werden können, um ähnliche Effekte zu erzeugen.

Sehen Sie sich als Beispiel für die numerische herauf Stufung die vordefinierten Implementierungen des Binary `*`-Operators an:

```csharp
int operator *(int x, int y);
uint operator *(uint x, uint y);
long operator *(long x, long y);
ulong operator *(ulong x, ulong y);
float operator *(float x, float y);
double operator *(double x, double y);
decimal operator *(decimal x, decimal y);
```

Wenn Regeln zur Überladungs Auflösung ([Überladungs](expressions.md#overload-resolution)Auflösung) auf diese Gruppe von Operatoren angewendet werden, besteht der Effekt darin, den ersten Operator auszuwählen, für den implizite Konvertierungen aus den Operanden Typen vorhanden sind. Beispiel: für den Vorgang `b * s`, bei dem `b` ein `byte` ist und `s` `short`ist, wählt die Überladungs Auflösung `operator *(int,int)` als optimalen Operator aus. Folglich ist der Effekt, dass `b` und `s` in `int`konvertiert werden und der Ergebnistyp `int`ist. Entsprechend wählt die Überladungs Auflösung für den Vorgang `i * d`, bei dem `i` ein `int` und `d` ein `double`ist, `operator *(double,double)` als besten Operator aus.

#### <a name="unary-numeric-promotions"></a>Unäre numerische Aktionen

Unäre numerische herauf Stufung tritt für die Operanden der vordefinierten `+`, `-`und `~` unären Operatoren auf. Die Unäre numerische herauf Stufung besteht einfach aus der Umstellung von Operanden vom Typ `sbyte`, `byte`, `short`, `ushort`oder `char` in den Typ `int`. Außerdem konvertiert die Unäre numerische herauf Stufung für den unären `-` Operator Operanden vom Typ `uint` in den Typ `long`.

#### <a name="binary-numeric-promotions"></a>Binäre numerische Aktionen

Binäre numerische herauf Stufung erfolgt bei den Operanden der vordefinierten `+`-, `-`-, `*`-, `/`-, `%`-, `&`-, `|`-, `^`-, `==`-, `!=`-und `>`-Binär Operatoren. Die binäre numerische herauf Stufung konvertiert beide Operanden implizit in einen gemeinsamen Typ, der bei nicht relationalen Operatoren auch zum Ergebnistyp des Vorgangs wird. Die binäre numerische herauf Stufung besteht aus der Anwendung der folgenden Regeln in der Reihenfolge, in der Sie angezeigt werden:

*  Wenn einer der beiden Operanden vom Typ `decimal`ist, wird der andere Operand in den Typ `decimal`konvertiert, oder es tritt ein Bindungs Zeitfehler auf, wenn der andere Operand vom Typ `float` oder `double`ist.
*  Andernfalls, wenn einer der beiden Operanden vom Typ `double`ist, wird der andere Operand in den Typ `double`konvertiert.
*  Andernfalls, wenn einer der beiden Operanden vom Typ `float`ist, wird der andere Operand in den Typ `float`konvertiert.
*  Andernfalls, wenn einer der beiden Operanden vom Typ `ulong`ist, wird der andere Operand in den Typ `ulong`konvertiert, oder es tritt ein Bindungs Fehler auf, wenn der andere Operand vom Typ `sbyte`, `short`, `int`oder `long`ist.
*  Andernfalls, wenn einer der beiden Operanden vom Typ `long`ist, wird der andere Operand in den Typ `long`konvertiert.
*  Andernfalls werden beide Operanden in den Typ `long`konvertiert, wenn einer der beiden Operanden vom Typ `uint` und der andere Operand vom Typ `sbyte`, `short`oder `int`ist.
*  Andernfalls, wenn einer der beiden Operanden vom Typ `uint`ist, wird der andere Operand in den Typ `uint`konvertiert.
*  Andernfalls werden beide Operanden in den Typ `int`konvertiert.

Beachten Sie, dass die erste Regel Vorgänge, die den `decimal` Typ mit den Typen `double` und `float` mischen, nicht zulässt. Die Regel folgt der Tatsache, dass keine impliziten Konvertierungen zwischen dem `decimal`-Typ und den Typen `double` und `float` vorhanden sind.

Beachten Sie außerdem, dass es nicht möglich ist, dass ein Operand vom Typ `ulong` ist, wenn der andere Operand einen ganzzahligen Typ mit Vorzeichen hat. Der Grund dafür ist, dass kein ganzzahliger Typ vorhanden ist, der den gesamten Bereich von `ulong` sowie die ganzzahligen Typen mit Vorzeichen darstellen kann.

In beiden oben genannten Fällen kann ein Umwandlungs Ausdruck verwendet werden, um einen Operanden explizit in einen Typ zu konvertieren, der mit dem anderen Operanden kompatibel ist.

Im Beispiel
```csharp
decimal AddPercent(decimal x, double percent) {
    return x * (1.0 + percent / 100.0);
}
```
ein Fehler bei der Bindungs Zeit tritt auf, weil ein `decimal` nicht mit einem `double`multipliziert werden kann. Der Fehler wird behoben, indem der zweite Operand explizit in `decimal`umgerechnet wird, wie im folgenden dargestellt:

```csharp
decimal AddPercent(decimal x, double percent) {
    return x * (decimal)(1.0 + percent / 100.0);
}
```

### <a name="lifted-operators"></a>„Lifted“ Operatoren

Mithilfe von aufzurufenden ***Operatoren*** können vordefinierte und benutzerdefinierte Operatoren, die nicht auf NULL festleg Bare Werttypen angewendet werden, auch mit null-fähigen Formularen dieser Typen verwendet werden Gesteigerte Operatoren werden aus vordefinierten und benutzerdefinierten Operatoren erstellt, die bestimmte Anforderungen erfüllen, wie im folgenden beschrieben:

*   Für die unären Operatoren

    ```csharp
    +  ++  -  --  !  ~
    ```

    eine angehobene Form eines Operators ist vorhanden, wenn der Operand und die Ergebnistypen beide Werttypen sind, die keine NULL-Werte zulassen. Das angefügte Formular wird erstellt, indem ein einzelner `?` Modifizierer zu den Operanden-und Ergebnistypen hinzugefügt wird. Der Operator "angehoben" erzeugt einen NULL-Wert, wenn der Operand NULL ist. Andernfalls entpackt der angehobene Operator den Operanden, wendet den zugrunde liegenden Operator an und umschließt das Ergebnis.

*   Für die binären Operatoren

    ```csharp
    +  -  *  /  %  &  |  ^  <<  >>
    ```

    ein angezeigter Typ eines Operators ist vorhanden, wenn der Operand und die Ergebnistypen alle Werttypen darstellen, die keine NULL-Werte zulassen. Das angefügte Formular wird erstellt, indem jedem Operand und Ergebnistyp ein einzelner `?` Modifizierer hinzugefügt wird. Der gesteigerte Operator erzeugt einen NULL-Wert, wenn ein oder beide Operanden NULL sind (eine Ausnahme ist die `&`-und `|` Operatoren des `bool?` Typs, wie in [booleschen logischen Operatoren](expressions.md#boolean-logical-operators)beschrieben). Andernfalls entpackt der angehobene Operator die Operanden, wendet den zugrunde liegenden Operator an und umschließt das Ergebnis.

*   Für die Gleichheits Operatoren

    ```csharp
    ==  !=
    ```

    eine angehobene Form eines Operators ist vorhanden, wenn die Operanden Typen sowohl nicht auf NULL festleg Bare Werttypen als auch, wenn der Ergebnistyp `bool`ist. Das angefügte Formular wird erstellt, indem jedem Operanden ein einzelner `?` Modifizierer hinzugefügt wird. Der Operator "angehoben" berücksichtigt zwei NULL-Werte gleich, und ein NULL-Wert entspricht keinem Wert, der ungleich NULL ist. Wenn beide Operanden nicht NULL sind, entpackt der angehobene Operator die Operanden und wendet den zugrunde liegenden Operator an, um das `bool` Ergebnis zu erhalten.

*   Für die relationalen Operatoren

    ```csharp
    <  >  <=  >=
    ```

    eine angehobene Form eines Operators ist vorhanden, wenn die Operanden Typen sowohl nicht auf NULL festleg Bare Werttypen als auch, wenn der Ergebnistyp `bool`ist. Das angefügte Formular wird erstellt, indem jedem Operanden ein einzelner `?` Modifizierer hinzugefügt wird. Der Operator "angehoben" erzeugt den Wert `false`, wenn ein oder beide Operanden NULL sind. Andernfalls entpackt der angehobene Operator die Operanden und wendet den zugrunde liegenden Operator an, um das `bool` Ergebnis zu erhalten.

## <a name="member-lookup"></a>Mitglieder Suche

Bei der Suche nach Membern handelt es sich um den Prozess, bei dem die Bedeutung eines Namens im Kontext eines Typs bestimmt wird. Eine Element Suche kann als Teil der Auswertung eines *Simple_name* ([einfache Namen](expressions.md#simple-names)) oder eines *member_access* ([Member Access](expressions.md#member-access)) in einem Ausdruck auftreten. Wenn die *Simple_name* oder *member_access* als *primary_expression* einer *invocation_expression* ([Methodenaufrufe](expressions.md#method-invocations)) auftritt, wird der Member als aufgerufen bezeichnet.

Wenn ein Member eine Methode oder ein Ereignis ist, oder wenn es sich um eine Konstante, ein Feld oder eine Eigenschaft eines Delegattyps (Delegaten) oder des Typs[`dynamic` (](delegates.md)[dynamischer Typ](types.md#the-dynamic-type)) handelt, wird der Member als *Aufruf Kabel*bezeichnet.

Die Element Suche berücksichtigt nicht nur den Namen eines Members, sondern auch die Anzahl der Typparameter, die der Member hat und ob auf den Member zugegriffen werden kann. Für die Suche nach Membern verfügen generische Methoden und generische generische Typen über die Anzahl der Typparameter, die in den jeweiligen Deklarationen angegeben sind, und alle anderen Member haben keine Typparameter.

Eine Member-Suche mit einem Namen `N` mit `K` Typparametern in einem Typ `T` wird wie folgt verarbeitet:

*  Zuerst wird ein Satz barrierefreier Member mit dem Namen `N` bestimmt:
    * Wenn `T` ein Typparameter ist, ist die Menge die Menge der zugreif baren Member mit dem Namen `N` in jedem der Typen, die als Primary-Einschränkung oder sekundäre Einschränkung ([Typparameter Einschränkungen](classes.md#type-parameter-constraints)) für `T`angegeben sind, zusammen mit dem Satz der zugänglichen Member, die `N` in `object`genannt werden.
    * Andernfalls besteht der Satz aus allen zugänglichen Membern ([Member Access](basic-concepts.md#member-access)) mit dem Namen `N` in `T`, einschließlich der geerbten Member und der zugänglichen Member, die `N` in `object`benannt sind. Wenn `T` ein konstruierter Typ ist, wird der Satz von Membern durch Ersetzen von Typargumenten abgerufen, wie in [Members von konstruierten Typen](classes.md#members-of-constructed-types)beschrieben. Member, die einen `override` Modifizierer einschließen, werden aus dem Satz ausgeschlossen.
*  Wenn `K` 0 (null) ist, werden alle in Form von Typen, deren Deklarationen Typparameter enthalten, entnommenen Typen entfernt. Wenn `K` nicht 0 (null) ist, werden alle Elemente mit einer anderen Anzahl von Typparametern entfernt. Beachten Sie Folgendes: Wenn `K` 0 (null) ist, werden Methoden mit Typparametern nicht entfernt, da derTyprückschluss-Prozess ([Typrückschluss](expressions.md#type-inference)) möglicherweise die Typargumente ableiten kann.
*  Wenn der Member *aufgerufen*wird, werden alle nicht*Aufruf* baren Member aus dem Satz entfernt.
*  Als nächstes werden Elemente, die von anderen Membern ausgeblendet werden, aus dem Satz entfernt. Für jedes Element `S.M` in der Menge, wobei `S` der Typ ist, in dem der Member `M` deklariert ist, werden die folgenden Regeln angewendet:
    * Wenn `M` eine Konstante, ein Feld, eine Eigenschaft, ein Ereignis oder ein Enumerationsmember ist, werden alle Member, die in einem Basistyp von `S` deklariert sind, aus dem Satz entfernt.
    * Wenn `M` eine Typdeklaration ist, werden alle nicht-Typen, die in einem Basistyp von `S` deklariert sind, aus dem Satz entfernt, und alle Typdeklarationen mit der gleichen Anzahl von Typparametern, wie `M`, die in einem Basistyp von `S` deklariert sind, werden aus dem Satz entfernt.
    * Wenn `M` eine Methode ist, werden alle nicht-Methoden Member, die in einem Basistyp von `S` deklariert sind, aus dem Satz entfernt.
*  Als nächstes werden Schnittstellenmember, die von Klassenmembern ausgeblendet werden, aus dem Satz entfernt. Dieser Schritt hat nur dann Auswirkungen, wenn `T` ein Typparameter ist und `T` sowohl eine effektive Basisklasse als `object` als auch einen nicht leeren effektiven Schnittstellen Satz ([Typparameter Einschränkungen](classes.md#type-parameter-constraints)) aufweist. Für jedes Element `S.M` in der Menge, wobei `S` der Typ ist, in dem der Member `M` deklariert ist, werden die folgenden Regeln angewendet, wenn `S` eine andere Klassen Deklaration als `object`ist:
    * Wenn `M` eine Konstante, ein Feld, eine Eigenschaft, ein Ereignis, ein Enumerationsmember oder eine Typdeklaration ist, werden alle in einer Schnittstellen Deklaration deklarierten Member aus dem Satz entfernt.
    * Wenn `M` eine Methode ist, werden alle nicht-Methoden Member, die in einer Schnittstellen Deklaration deklariert sind, aus dem Satz entfernt, und alle Methoden mit derselben Signatur wie `M`, die in einer Schnittstellen Deklaration deklariert sind, werden aus dem Satz entfernt.
*  Schließlich wird das Ergebnis der Suche bestimmt, wenn verborgene Member entfernt wurden:
    * Wenn der Satz aus einem einzelnen Member besteht, der keine Methode ist, dann ist dieser Member das Ergebnis der Suche.
    * Andernfalls ist diese Gruppe von Methoden das Ergebnis der Suche, wenn der Satz nur Methoden enthält.
    * Andernfalls ist die Suche mehrdeutig, und es tritt ein Bindungs Zeitfehler auf.

Für Member-suchen in anderen Typen als Typparameter und Schnittstellen und Member-suchen in Schnittstellen, die nur eine einzige Vererbung sind (jede Schnittstelle in der Vererbungs Kette weist genau null oder eine direkte Basisschnittstelle auf), hat die Auswirkung der Such Regeln den Wert einfach, dass abgeleitete Member Basiselemente mit dem gleichen Namen oder der gleichen Signatur ausblenden. Solche Suchvorgänge mit einer einzelnen Vererbung sind nie mehrdeutig. Die Mehrdeutigkeiten, die möglicherweise von Member-suchen in Schnittstellen mit mehreren Vererbungen auftreten können, werden unter Zugreifen auf die [Benutzeroberfläche](interfaces.md#interface-member-access)beschrieben.

### <a name="base-types"></a>Basis Typen

Für Zwecke der Element Suche wird ein Type-`T` als die folgenden Basis Typen betrachtet:

*  Wenn `T` `object`ist, hat `T` keinen Basistyp.
*  Wenn `T` ein *enum_type*ist, sind die Basis Typen von `T` die Klassentypen `System.Enum`, `System.ValueType`und `object`.
*  Wenn `T` ein *struct_type*ist, sind die Basis Typen von `T` die Klassentypen `System.ValueType` und `object`.
*  Wenn `T` ein *class_type*ist, sind die Basis Typen von `T` die Basisklassen von `T`, einschließlich des Klassen Typs `object`.
*  Wenn `T` ein *INTERFACE_TYPE*ist, sind die Basis Typen von `T` die Basis Schnittstellen `T` und der Klassentyp `object`.
*  Wenn `T` ein *array_type*ist, sind die Basis Typen von `T` die Klassentypen `System.Array` und `object`.
*  Wenn `T` ein *delegate_type*ist, sind die Basis Typen von `T` die Klassentypen `System.Delegate` und `object`.

## <a name="function-members"></a>Funktionsmember

Funktionsmember sind Elemente, die ausführbare Anweisungen enthalten. Funktionsmember sind immer Member von Typen und können keine Member von Namespaces sein. C#definiert die folgenden Kategorien von Funktionsmembern:

*  Methoden
*  Eigenschaften
*  Ereignisse
*  Indexer
*  Benutzerdefinierte Operatoren
*  Instanzkonstruktoren
*  Statische Konstruktoren
*  Destruktoren verschoben.

Mit Ausnahme der destrukturtoren und statischer Konstruktoren (die nicht explizit aufgerufen werden können) werden die in Funktionsmembern enthaltenen Anweisungen durch Funktionselement Aufrufe ausgeführt. Die tatsächliche Syntax zum Schreiben eines Funktionsmember-aufzurufenden ist von der jeweiligen Funktionsmember-Kategorie abhängig.

Die Argumentliste ([Argumentlisten](expressions.md#argument-lists)) eines Funktionsmember-aufzurufenden enthält tatsächliche Werte oder Variablen Verweise für die Parameter des Funktionsmembers.

Aufrufe generischer Methoden können den Typrückschluss verwenden, um den Satz von Typargumenten zu ermitteln, die an die Methode übergeben werden sollen. Dieser Prozess wird unter [Typrückschluss](expressions.md#type-inference)beschrieben.

Aufrufe von Methoden, Indexern, Operatoren und Instanzkonstruktoren verwenden die Überladungs Auflösung, um zu bestimmen, welcher Satz von Funktions Membern aufgerufen werden soll. Dieser Prozess wird in der [Überladungs Auflösung](expressions.md#overload-resolution)beschrieben.

Sobald ein bestimmter Funktionsmember zur Bindungs Zeit (möglicherweise durch Überladungs Auflösung) identifiziert wurde, wird der tatsächliche Lauf Zeit Prozess des Aufrufs des Funktionsmembers in der [Kompilierzeit Überprüfung der dynamischen Überladungs Auflösung](expressions.md#compile-time-checking-of-dynamic-overload-resolution)beschrieben.

In der folgenden Tabelle wird die Verarbeitung zusammengefasst, die in-Konstrukten mit den sechs Kategorien von Funktionsmembern stattfindet, die explizit aufgerufen werden können. In der Tabelle werden `e`, `x`, `y`und `value` Ausdrücke angeben, die als Variablen oder Werte klassifiziert sind. `T` gibt einen Ausdruck an, der als Typ klassifiziert ist, `F` ist der einfache Name einer Methode, und `P` ist der einfache Name einer Eigenschaft.


| __Erstellen__     | __Beispiel__    | __Beschreibung__ |
|-------------------|----------------|-----------------|
| Methodenaufruf | `F(x,y)`       | Die Überladungs Auflösung wird angewendet, um die beste Methode `F` in der enthaltenden Klasse oder Struktur auszuwählen. Die-Methode wird mit der Argumentliste `(x,y)`aufgerufen. Wenn die Methode nicht `static`ist, wird der Instanzausdruck `this`. | 
|                   | `T.F(x,y)`     | Die Überladungs Auflösung wird angewendet, um die beste Methode `F` in der Klasse oder Struktur `T`auszuwählen. Ein Fehler bei der Bindungs Zeit tritt auf, wenn die Methode nicht `static`ist. Die-Methode wird mit der Argumentliste `(x,y)`aufgerufen. | 
|                   | `e.F(x,y)`     | Die Überladungs Auflösung wird angewendet, um die beste Methode F in der Klasse, Struktur oder Schnittstelle auszuwählen, die durch den Typ der `e`angegeben wird. Ein Bindungs Zeit Fehler tritt auf, wenn die Methode `static`ist. Die-Methode wird mit dem Instanzausdruck `e` und der Argumentliste `(x,y)`aufgerufen. | 
| Eigenschaftenzugriff   | `P`            | Der `get` Accessor der Eigenschaft, die in der enthaltenden Klasse oder Struktur `P` wird, wird aufgerufen. Ein Kompilierzeitfehler tritt auf, wenn `P` schreibgeschützt ist. Wenn `P` nicht `static`ist, wird der Instanzausdruck `this`. | 
|                   | `P = value`    | Der `set` Accessor der Eigenschaft `P` in der enthaltenden Klasse oder Struktur wird mit der Argumentliste `(value)`aufgerufen. Ein Kompilierzeitfehler tritt auf, wenn `P` schreibgeschützt ist. Wenn `P` nicht `static`ist, wird der Instanzausdruck `this`. | 
|                   | `T.P`          | Der `get` Accessor der Eigenschaft `P` in der Klasse oder Struktur `T` aufgerufen wird. Ein Kompilierzeitfehler tritt auf, wenn `P` nicht `static` ist oder wenn `P` schreibgeschützt ist. | 
|                   | `T.P = value`  | Der `set` Accessor der Eigenschaft `P` in der Klasse oder Struktur `T` der mit der Argumentliste `(value)`aufgerufen wird. Ein Kompilierzeitfehler tritt auf, wenn `P` nicht `static` ist oder wenn `P` schreibgeschützt ist. | 
|                   | `e.P`          | Der `get` Accessor der Eigenschaft `P` in der Klasse, Struktur oder Schnittstelle, die durch den Typ von `e` angegeben wird, wird mit dem Instanzausdruck `e`aufgerufen. Ein Bindungs Zeit Fehler tritt auf, wenn `P` `static` ist oder wenn `P` schreibgeschützt ist. | 
|                   | `e.P = value`  | Der `set` Accessor der Eigenschaft `P` in der Klasse, Struktur oder Schnittstelle, die durch den Typ von `e` angegeben wird, wird mit dem Instanzausdruck `e` und der Argumentliste `(value)`aufgerufen. Ein Bindungs Fehler tritt auf, wenn `P` `static` ist oder wenn `P` schreibgeschützt ist. | 
| Ereignis Zugriff      | `E += value`   | Der `add` Accessor des Ereignisses, `E` in der enthaltenden Klasse oder Struktur aufgerufen wird. Wenn `E` nicht statisch ist, wird der Instanzausdruck `this`. | 
|                   | `E -= value`   | Der `remove` Accessor des Ereignisses, `E` in der enthaltenden Klasse oder Struktur aufgerufen wird. Wenn `E` nicht statisch ist, wird der Instanzausdruck `this`. | 
|                   | `T.E += value` | Der `add` Accessor des Ereignisses `E` in der Klasse oder Struktur `T` aufgerufen wird. Ein Fehler bei der Bindungs Zeit tritt auf, wenn `E` nicht statisch ist. | 
|                   | `T.E -= value` | Der `remove` Accessor des Ereignisses `E` in der Klasse oder Struktur `T` aufgerufen wird. Ein Fehler bei der Bindungs Zeit tritt auf, wenn `E` nicht statisch ist. | 
|                   | `e.E += value` | Der `add` Accessor des Ereignisses `E` in der Klasse, Struktur oder Schnittstelle, die durch den Typ von `e` angegeben wird, mit dem Instanzausdruck `e`aufgerufen wird. Ein Bindungs Zeit Fehler tritt auf, wenn `E` statisch ist. | 
|                   | `e.E -= value` | Der `remove` Accessor des Ereignisses `E` in der Klasse, Struktur oder Schnittstelle, die durch den Typ von `e` angegeben wird, mit dem Instanzausdruck `e`aufgerufen wird. Ein Bindungs Zeit Fehler tritt auf, wenn `E` statisch ist. | 
| Indexerzugriff    | `e[x,y]`       | Die Überladungs Auflösung wird angewendet, um den besten Indexer in der Klasse, Struktur oder Schnittstelle auszuwählen, die durch den Typ von e angegeben wird. Der `get` Accessor des Indexers wird mit dem Instanzausdruck `e` und der Argumentliste `(x,y)`aufgerufen. Ein Bindungs Zeit Fehler tritt auf, wenn der Indexer schreibgeschützt ist. | 
|                   | `e[x,y] = value` | Die Überladungs Auflösung wird angewendet, um den besten Indexer in der Klasse, Struktur oder Schnittstelle auszuwählen, die durch den Typ der `e`angegeben wird. Der `set` Accessor des Indexers wird mit dem Instanzausdruck `e` und der Argumentliste `(x,y,value)`aufgerufen. Ein Bindungs Zeit Fehler tritt auf, wenn der Indexer schreibgeschützt ist. | 
| Operator Aufruf | `-x`         | Die Überladungs Auflösung wird angewendet, um den besten unären Operator in der Klasse oder Struktur auszuwählen, die durch den Typ der `x`angegeben wird. Der ausgewählte Operator wird mit der Argumentliste `(x)`aufgerufen. | 
|                     | `x + y`      | Die Überladungs Auflösung wird angewendet, um den besten binären Operator in den Klassen oder Strukturen auszuwählen, die von den Typen von `x` und `y`angegeben werden. Der ausgewählte Operator wird mit der Argumentliste `(x,y)`aufgerufen. | 
| Instanzkonstruktoraufruf | `new T(x,y)` | Die Überladungs Auflösung wird angewendet, um den besten Instanzkonstruktor in der Klasse oder Struktur `T`auszuwählen. Der Instanzkonstruktor wird mit der Argumentliste `(x,y)`aufgerufen. | 

### <a name="argument-lists"></a>Argument Listen

Jeder Funktionsmember und Delegataufruf enthält eine Argumentliste, die tatsächliche Werte oder Variablen Verweise für die Parameter des Funktionsmembers bereitstellt. Die Syntax zum Angeben der Argumentliste eines Funktionsmember-aufzurufenden ist von der Funktionsmember-Kategorie abhängig:

*  Bei Instanzkonstruktoren, Methoden, Indexern und Delegaten werden die Argumente als *argument_list*angegeben, wie unten beschrieben. Bei Indexers schließt beim Aufrufen des `set` Accessors die Argumentliste zusätzlich den Ausdruck ein, der als rechter Operand des Zuweisungs Operators angegeben ist.
*  Bei Eigenschaften ist die Argumentliste leer, wenn der `get` Accessor aufgerufen wird, und besteht aus dem Ausdruck, der beim Aufrufen des `set` Accessors als rechter Operand des Zuweisungs Operators angegeben wurde.
*  Bei Ereignissen besteht die Argumentliste aus dem Ausdruck, der als rechter Operand des `+=` oder `-=` Operators angegeben wird.
*  Bei benutzerdefinierten Operatoren besteht die Argumentliste aus dem einzelnen Operanden des unären Operators oder den beiden Operanden des binären Operators.

Die Argumente von Eigenschaften ([Eigenschaften](classes.md#properties)), Ereignissen ([Ereignissen](classes.md#events)) und benutzerdefinierten Operatoren ([Operatoren](classes.md#operators)) werden immer als Wert Parameter ([value-Parameter](classes.md#value-parameters)) übermittelt. Die Argumente von Indexer ([Indexer](classes.md#indexers)) werden immer als Wert Parameter ([Wert Parameter](classes.md#value-parameters)) oder Parameter Arrays ([Parameter Arrays](classes.md#parameter-arrays)) übergeben. Verweis-und Ausgabeparameter werden für diese Kategorien von Funktionsmembern nicht unterstützt.

Die Argumente eines Instanzkonstruktors, einer Methode, eines Indexers oder eines delegataufrufers werden als *argument_list*angegeben:

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

Ein *argument_list* besteht aus einem oder mehreren *Argumenten*, getrennt durch Kommas. Jedes Argument besteht aus einem optionalen *argument_name* gefolgt von einem *argument_value*. Ein *Argument* mit einem *argument_name* wird als ***benanntes Argument***bezeichnet, wohingegen ein *Argument* ohne *argument_name* ein ***Positions Argument***ist. Es ist ein Fehler für ein Positions Argument, das nach einem benannten Argument in einem *argument_list*angezeigt wird.

Der *argument_value* kann eine der folgenden Formen annehmen:

*  Ein *Ausdruck*, der angibt, dass das Argument als Wert Parameter ([Wert Parameter](classes.md#value-parameters)) übergeben wird.
*  Das Schlüsselwort `ref` gefolgt von einem *variable_reference* ([Variablen Verweise](variables.md#variable-references)), das angibt, dass das Argument als Verweis Parameter ([Verweis Parameter](classes.md#reference-parameters)) übergeben wird. Eine Variable muss definitiv zugewiesen werden ([definitive Zuweisung](variables.md#definite-assignment)), bevor Sie als Verweis Parameter übergeben werden kann. Das Schlüsselwort `out` gefolgt von einem *variable_reference* ([Variablen Verweise](variables.md#variable-references)), das angibt, dass das Argument als Ausgabeparameter ([Ausgabeparameter](classes.md#output-parameters)) übergeben wird. Eine Variable wird als definitiv zugewiesen ([definitive Zuweisung](variables.md#definite-assignment)) nach einem Funktionselement Aufruf, bei dem die Variable als Output-Parameter übergeben wird.

#### <a name="corresponding-parameters"></a>Entsprechende Parameter

Für jedes Argument in einer Argumentliste muss ein entsprechender Parameter im Funktions Member oder Delegaten vorhanden sein, der aufgerufen wird.

Die im folgenden verwendete Parameterliste wird wie folgt bestimmt:

*  Bei virtuellen Methoden und indexatoren, die in Klassen definiert sind, wird die Parameterliste aus der spezifischsten Deklaration oder Überschreibung des Funktionsmembers ausgewählt, beginnend mit dem statischen Typ des Empfängers und Durchsuchen der zugehörigen Basisklassen.
*  Für Schnittstellen Methoden und Indexer wird die Parameterliste aus der spezifischsten Definition des Members ausgewählt, beginnend mit dem Schnittstellentyp und Durchsuchen der Basis Schnittstellen. Wenn keine eindeutige Parameterliste gefunden wird, wird eine Parameterliste mit nicht zugänglichen Namen und optionalen Parametern erstellt, sodass Aufrufe keine benannten Parameter verwenden können oder keine optionalen Argumente weglassen.
*  Bei partiellen Methoden wird die Parameterliste der definierenden partiellen Methoden Deklaration verwendet.
*  Für alle anderen Funktionsmember und Delegaten gibt es nur eine einzige Parameterliste, die verwendet wird.

Die Position eines Arguments oder Parameters wird als die Anzahl der Argumente oder Parameter definiert, die in der Argumentliste oder Parameterliste vorangestellt sind.

Die entsprechenden Parameter für Funktionsmember-Argumente werden wie folgt festgelegt:

*  Argumente im *argument_list* von Instanzkonstruktoren, Methoden, Indexern und Delegaten:
    * Ein Positions Argument, bei dem ein fester Parameter an derselben Position in der Parameterliste auftritt, entspricht diesem Parameter.
    * Ein Positions Argument eines Funktionsmembers mit einem Parameter Array, das in der normalen Form aufgerufen wird, entspricht dem Parameter Array, das an derselben Position in der Parameterliste vorkommen muss.
    * Ein Positions Argument eines Funktionsmembers mit einem Parameter Array, das in der erweiterten Form aufgerufen wird, wobei kein fester Parameter an derselben Position in der Parameterliste auftritt, entspricht einem Element im Parameter Array.
    * Ein benanntes Argument entspricht dem-Parameter mit dem gleichen Namen in der Parameterliste.
    * Bei Indexers entspricht der als rechter Operand des Zuweisungs Operators angegebene Ausdruck dem impliziten `value` Parameter der `set` Accessor-Deklaration, wenn der `set` Accessor aufgerufen wird.
*  Bei-Eigenschaften gibt es keine Argumente, wenn der `get`-Accessor aufgerufen wird. Wenn der `set`-Accessor aufgerufen wird, entspricht der als rechter Operand des Zuweisungs Operators angegebene Ausdruck dem impliziten `value` Parameter der `set` Accessor-Deklaration.
*  Bei benutzerdefinierten unären Operatoren (einschließlich Konvertierungen) entspricht der einzelne Operand dem einzelnen Parameter der Operator Deklaration.
*  Für benutzerdefinierte binäre Operatoren entspricht der linke Operand dem ersten Parameter, und der rechte Operand entspricht dem zweiten Parameter der Operator Deklaration.

#### <a name="run-time-evaluation-of-argument-lists"></a>Lauf Zeit Auswertung von Argumentlisten

Während der Lauf Zeit Verarbeitung eines Funktionsmember-aufzurufenden ([Kompilierzeit Überprüfung der dynamischen Überladungs Auflösung](expressions.md#compile-time-checking-of-dynamic-overload-resolution)) werden die Ausdrücke oder Variablen Verweise einer Argumentliste in der Reihenfolge von links nach rechts ausgewertet, wie folgt:

*  Bei einem value-Parameter wird der Argument Ausdruck ausgewertet und eine implizite Konvertierung ([implizite Konvertierungen](conversions.md#implicit-conversions)) in den entsprechenden Parametertyp durchgeführt. Der resultierende Wert wird zum Anfangswert des value-Parameters im Funktionselement Aufruf.
*  Bei einem Verweis-oder Ausgabeparameter wird der Variablen Verweis ausgewertet, und der resultierende Speicherort wird zu dem Speicherort, der durch den-Parameter im Aufruf der Funktionselemente dargestellt wird. Wenn der als Verweis oder Ausgabeparameter angegebene Variablen Verweis ein Array Element einer *reference_type*ist, wird eine Lauf Zeit Überprüfung durchgeführt, um sicherzustellen, dass der Elementtyp des Arrays mit dem Parametertyp identisch ist. Wenn bei dieser Überprüfung ein Fehler auftritt, wird eine `System.ArrayTypeMismatchException` ausgelöst.

Methoden, Indexer und Instanzkonstruktoren können Ihren äußersten ganz rechts Parameter als Parameter Array deklarieren ([Parameter Arrays](classes.md#parameter-arrays)). Solche Funktionsmember werden entweder in ihrer normalen Form oder in der erweiterten Form aufgerufen, je nachdem, welche anwendbar ist ([anwendbares Funktionsmember](expressions.md#applicable-function-member)):

*  Wenn ein Funktionsmember mit einem Parameter Array in der normalen Form aufgerufen wird, muss das für das Parameter Array angegebene Argument ein einzelner Ausdruck sein, der implizit konvertierbar ([implizite Konvertierungen](conversions.md#implicit-conversions)) in den Typ des Parameter Arrays ist. In diesem Fall verhält sich das Parameter Array genau wie ein value-Parameter.
*  Wenn ein Funktionsmember mit einem Parameter Array in der erweiterten Form aufgerufen wird, muss der Aufruf NULL oder mehr Positions Argumente für das Parameter Array angeben, wobei jedes Argument ein Ausdruck ist, der implizit konvertierbar ([implizite Konvertierungen](conversions.md#implicit-conversions)) in den Elementtyp des Parameter Arrays ist. In diesem Fall erstellt der Aufruf eine Instanz des Parameter Array Typs mit einer Länge, die der Anzahl der Argumente entspricht, initialisiert die Elemente der Array Instanz mit den angegebenen Argument Werten und verwendet die neu erstellte Array Instanz als die tatsächliche gestritten.

Die Ausdrücke einer Argumentliste werden immer in der Reihenfolge ausgewertet, in der Sie geschrieben werden. Das Beispiel
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
```console
x = 0, y = 1, z = 2
x = 4, y = -1, z = 3
```

Die Array-Co-Varianz Regeln ([Array-Kovarianz](arrays.md#array-covariance)) erlauben, dass ein Wert eines Arraytyps `A[]` ein Verweis auf eine Instanz eines Arraytyps `B[]`ist, vorausgesetzt, dass eine implizite Verweis Konvertierung von `B` in `A`vorhanden ist. Aufgrund dieser Regeln ist eine Lauf Zeit Überprüfung erforderlich, wenn ein Array Element einer *reference_type* als Verweis-oder Ausgabeparameter übergeben wird, um sicherzustellen, dass der tatsächliche Elementtyp des Arrays mit dem des Parameters identisch ist. Im Beispiel
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
der zweite Aufruf von `F` bewirkt, dass eine `System.ArrayTypeMismatchException` ausgelöst wird, da der tatsächliche Elementtyp von `b` `string` und nicht `object`ist.

Wenn ein Funktionsmember mit einem Parameter Array in der erweiterten Form aufgerufen wird, wird der Aufruf genau so verarbeitet, als ob ein Array Erstellungs Ausdruck mit einem Arrayinitialisierer ([Array Erstellungs Ausdrücke](expressions.md#array-creation-expressions)) um die erweiterten Parameter eingefügt wurde. Beispielsweise mit der Deklaration
```csharp
void F(int x, int y, params object[] args);
```
die folgenden Aufrufe der erweiterten Form der Methode
```csharp
F(10, 20);
F(10, 20, 30, 40);
F(10, 20, 1, "hello", 3.0);
```
genau entsprechen
```csharp
F(10, 20, new object[] {});
F(10, 20, new object[] {30, 40});
F(10, 20, new object[] {1, "hello", 3.0});
```

Beachten Sie insbesondere, dass ein leeres Array erstellt wird, wenn keine Argumente für das Parameter Array angegeben werden.

Wenn Argumente von einem Funktionsmember mit entsprechenden optionalen Parametern ausgelassen werden, werden die Standardargumente der Deklaration des Funktions Members implizit übermittelt. Da diese immer konstant sind, wirkt sich Ihre Auswertung nicht auf die Auswertungs Reihenfolge der restlichen Argumente aus.

### <a name="type-inference"></a>Typrückschluss

Wenn eine generische Methode ohne Angabe von Typargumenten aufgerufen wird, versucht ein ***Typrückschluss*** -Prozess, Typargumente für den Aufruf abzuleiten. Das vorhanden sein des Typrückschlusses ermöglicht die Verwendung einer bequemeren Syntax zum Aufrufen einer generischen Methode und ermöglicht dem Programmierer, das Angeben von redundanten Typinformationen zu vermeiden. Beispielsweise mit der Methoden Deklaration:
```csharp
class Chooser
{
    static Random rand = new Random();

    public static T Choose<T>(T first, T second) {
        return (rand.Next(2) == 0)? first: second;
    }
}
```
Es ist möglich, die `Choose`-Methode aufzurufen, ohne explizit ein Typargument anzugeben:
```csharp
int i = Chooser.Choose(5, 213);                 // Calls Choose<int>

string s = Chooser.Choose("foo", "bar");        // Calls Choose<string>
```

Durch den Typrückschluss werden die Typargumente `int` und `string` von den Argumenten der Methode bestimmt.

Der Typrückschluss tritt als Teil der Bindungs Zeit Verarbeitung eines Methoden Aufrufs ([Methodenaufrufe](expressions.md#method-invocations)) auf und findet vor dem Schritt zur Überladungs Auflösung des Aufrufs statt. Wenn eine bestimmte Methoden Gruppe in einem Methodenaufruf angegeben wird und im Rahmen des Methoden aufruschlusses keine Typargumente angegeben werden, wird der Typrückschluss auf jede generische Methode in der Methoden Gruppe angewendet. Wenn der Typrückschluss erfolgreich ist, werden die Typargumente abgeleitet, um die Argument Typen für die nachfolgende Überladungs Auflösung zu bestimmen. Wenn die Überladungs Auflösung eine generische Methode als die aufzurufende Methode auswählt, werden die abgerufenen Typargumente als tatsächliche Typargumente für den Aufruf verwendet. Wenn der Typrückschluss für eine bestimmte Methode fehlschlägt, wird diese Methode nicht an der Überladungs Auflösung beteiligt. Der Fehler beim Typrückschluss in und von sich führt nicht zu einem Bindungs Fehler. Allerdings führt dies häufig zu einem Bindungs Fehler, wenn die Überladungs Auflösung dann keine anwendbaren Methoden findet.

Wenn sich die angegebene Anzahl von Argumenten von der Anzahl der Parameter in der Methode unterscheidet, schlägt die Ableitung sofort fehl. Angenommen, die generische Methode hat die folgende Signatur:
```csharp
Tr M<X1,...,Xn>(T1 x1, ..., Tm xm)
```

Mit einem Methoden aufzurufenden Formular `M(E1...Em)` die Aufgabe des Typrückschlusses das Suchen nach eindeutigen Typargumenten `S1...Sn` für jeden Typparameter `X1...Xn`, damit der-Aufrufvorgang `M<S1...Sn>(E1...Em)` gültig wird.

Während des *Abschlusses* jedes Typparameters `Xi` entweder auf einen bestimmten Typ *fest* `Si` oder auf einen zugeordneten Satz von *Begrenzungen*festgelegt werden. Jede der Begrenzungen ist ein Typ `T`. Anfänglich werden für jede Typvariable `Xi` ein leerer Satz von Begrenzungen festgelegt.

Der Typrückschluss findet in Phasen statt. In jeder Phase wird versucht, Typargumente für weitere Typvariablen basierend auf den Ergebnissen der vorherigen Phase abzuleiten. In der ersten Phase werden einige anfängliche Rückschlüsse von Begrenzungen erstellt, während in der zweiten Phase Typvariablen für bestimmte Typen korrigiert und weitere Begrenzungen abgeleitet werden. Die zweite Phase muss möglicherweise mehrmals wiederholt werden.

*Hinweis:* Der Typrückschluss findet nicht nur statt, wenn eine generische Methode aufgerufen wird. Der Typrückschluss für die Konvertierung von Methoden Gruppen wird unter [Typrückschluss für die Konvertierung von Methoden Gruppen](expressions.md#type-inference-for-conversion-of-method-groups) beschrieben und der beste allgemeine Typ eines Satzes von Ausdrücken finden Sie unter [Ermitteln des besten allgemeinen Typs eines Satzes von Ausdrücken](expressions.md#finding-the-best-common-type-of-a-set-of-expressions).

#### <a name="the-first-phase"></a>Die erste Phase

Für jedes der Methodenargumente `Ei`:

*   Wenn `Ei` eine anonyme Funktion ist, wird ein *expliziter parametertyprückschluss* ([explizite Parametertyp Rückschlüsse](expressions.md#explicit-parameter-type-inferences)) von `Ei` auf `Ti`
*   Andernfalls, wenn `Ei` über einen Typ verfügt `U` und `xi` ein Wert Parameter ist, wird ein Rück *Schluss* *von* `U` *zum* `Ti`hergestellt.
*   Andernfalls, wenn `Ei` über einen Typ verfügt `U` und `xi` ein `ref`-oder `out` Parameter ist, wird eine *genaue Ableitung* von `U` *auf* `Ti`*durch* geführt.
*   Andernfalls wird kein Rückschluss für dieses Argument gemacht.


#### <a name="the-second-phase"></a>Die zweite Phase

Die zweite Phase verläuft wie folgt:

*   Alle nicht *fixierten* Typvariablen `Xi` die nicht *abhängig* sind ([Abhängigkeit](expressions.md#dependence)), werden `Xj` korrigiert ([Korrektur](expressions.md#fixing)).
*   Wenn keine derartigen Typvariablen vorhanden sind, werden alle *nicht fixierten* Typvariablen `Xi` *korrigiert* , für die Folgendes gilt:
    *   Es ist mindestens eine Typvariable `Xj` vorhanden, die von abhängt `Xi`
    *   `Xi` weist einen nicht leeren Satz von Begrenzungen auf.
*   Wenn keine Typvariablen vorhanden sind und noch immer *unfixe* Typvariablen vorhanden sind, schlägt der Typrückschluss fehl.
*   Andernfalls ist der Typrückschluss erfolgreich, wenn keine weiteren *unfixed* -Typvariablen vorhanden sind.
*   Andernfalls gilt für alle Argumente `Ei` mit dem entsprechenden Parametertyp `Ti`, wobei die *Ausgabetypen* ([Ausgabetypen](expressions.md#output-types)) *unfixed* Typvariablen enthalten `Xj`, aber die *Eingabetypen* ([Eingabetypen](expressions.md#input-types)) nicht. der *Ausgabetyp Rückschluss* ([Ausschlüsse auf Ausgabetyp](expressions.md#output-type-inferences)) erfolgt *von* `Ei` *bis* `Ti`. Die zweite Phase wird wiederholt.

#### <a name="input-types"></a>Eingabetypen

Wenn `E` eine Methoden Gruppe oder implizit typisierte anonyme Funktion ist und `T` ein Delegattyp oder ein Ausdrucks Strukturtyp ist, sind alle Parametertypen von `T` *Eingabetypen* von `E` *mit dem Typ* `T`.

####  <a name="output-types"></a>Ausgabetypen

Wenn `E` eine Methoden Gruppe oder eine anonyme Funktion ist und `T` ein Delegattyp oder ein Ausdrucks bauentyp ist, ist der Rückgabetyp von `T` ein *Ausgabetyp von* `E` *mit dem Typ* `T`.

#### <a name="dependence"></a>Hän

Eine Variable vom Typ " *unfixed* " `Xi` ist direkt von einer Variablen vom Typ "unfester Typ" *abhängig* `Xj` wenn für ein Argument `Ek` vom Typ "`Tk`" `Xj` in einem *Eingabetyp* von "`Ek`" mit dem Typ "`Tk`" auftritt und `Xi` in einem *Ausgabetyp* `Ek`

`Xj` *hängt* von `Xi` ab, wenn `Xj` *direkt* von `Xi` abhängig ist oder wenn `Xi` *direkt auf* `Xk` *und `Xk` von `Xj`abhängig* ist. Daher ist "hängt von" der transitiv, aber nicht reflexive Abschluss von "ist direkt abhängig".

#### <a name="output-type-inferences"></a>Ausschlüsse auf Ausgabetyp

Ein *ausgabetyprückschluss* erfolgt auf folgende Weise *von* einem Ausdruck `E` *auf* einen Typ `T`:

*  Wenn `E` eine anonyme Funktion mit dem abzurufenden Rückgabetyp `U` (abzurufender[Rückgabetyp](expressions.md#inferred-return-type)) ist und `T` ein Delegattyp oder ein Ausdrucks Strukturtyp mit dem Rückgabetyp `Tb`ist, wird ein untergeordneter Daten *Rückschluss* ([niedrigerer gebundener Schlussfolgerung](expressions.md#lower-bound-inferences)) *von* `U` *bis* `Tb`vorgenommen.
*  Andernfalls, wenn `E` eine Methoden Gruppe ist und `T` ein Delegattyp oder ein Ausdrucks Strukturtyp mit Parametertypen `T1...Tk` und der Rückgabetyp `Tb`ist und die Überladungs Auflösung von `E` mit den Typen `T1...Tk` eine einzelne Methode mit dem Rückgabetyp `U`ergibt, wird ein *niedrigerer gebundener Rückschluss* *von* `U` *bis* `Tb`ausgelöst.
*  Andernfalls, wenn `E` ein Ausdruck mit dem Typ `U`ist, wird ein Rück *Schluss* *von* `U` *zum* `T`gemacht.
*  Andernfalls werden keine Rückschlüsse gemacht.

#### <a name="explicit-parameter-type-inferences"></a>Explizite Parametertyp Rückschlüsse

Ein *expliziter Parameter-Typrückschluss* wird *von* einem Ausdruck `E` *auf* einen Typ `T` folgendermaßen erstellt:

*  Wenn `E` eine explizit typisierte anonyme Funktion mit Parametertypen ist `U1...Uk` und `T` ein Delegattyp oder ein Ausdrucks Strukturtyp mit Parametertypen ist `V1...Vk` dann für jede `Ui` einen *exakten* Rückschluss ([exakte Rückschlüsse](expressions.md#exact-inferences)) *von* `Ui` *bis* zum entsprechenden `Vi`ergibt.

#### <a name="exact-inferences"></a>Exakte Rückschlüsse

Ein *genauer Rückschluss* *von* einem Typ `U` *zu* einem Typ `V` der folgendermaßen erfolgt:

*  Wenn `V` einer der *unfixen* `Xi` ist, wird `U` dem Satz von exakten Begrenzungen für `Xi`hinzugefügt.

*  Andernfalls legt `V1...Vk` fest, und `U1...Uk` werden bestimmt, indem überprüft wird, ob eines der folgenden Fälle zutrifft:

   *  `V` ist ein Arraytyp `V1[...]` und `U` ist ein Arraytyp `U1[...]` desselben Rang.
   *  `V` ist der Typ `V1?` und `U` ist der Typ `U1?`
   *  `V` ist ein konstruierter Typ `C<V1...Vk>`und `U` ist ein konstruierter Typ `C<U1...Uk>`

   Wenn einer dieser Fälle zutrifft, wird ein *genauer Rückschluss* *von* jeder `Ui` *auf* den entsprechenden `Vi`vorgenommen.

*  Andernfalls werden keine Rückschlüsse gemacht.

#### <a name="lower-bound-inferences"></a>Unten gebundene Rückschlüsse

Ein untergeordneter Daten *Rückschluss* *von* einem Typ `U` *zu* einem Typ `V` der wie folgt erstellt wird:

*  Wenn `V` einer der *nicht fixierten* `Xi` ist, wird `U` dem Satz von unteren Grenzen für `Xi`hinzugefügt.
*  Andernfalls, wenn `V` der Typ `V1?`und `U` den Typ `U1?`, wird ein niedrigerer gebundener Rückschluss von `U1` bis `V1`vorgenommen.
*  Andernfalls legt `U1...Uk` fest, und `V1...Vk` werden bestimmt, indem überprüft wird, ob eines der folgenden Fälle zutrifft:
   *  `V` ist ein Arraytyp `V1[...]` und `U` ist ein Arraytyp `U1[...]` (oder ein Typparameter, dessen effektiver Basistyp `U1[...]`ist) desselben Rang.
   *  `V` ist eine `IEnumerable<V1>`, `ICollection<V1>` oder `IList<V1>` und `U` ist ein eindimensionaler Arraytyp `U1[]`(oder ein Typparameter, dessen effektiver Basistyp `U1[]`ist).
   *  `V` ist eine konstruierte Klasse, Struktur, Schnittstelle oder Delegattyp `C<V1...Vk>` und es gibt einen eindeutigen Typ `C<U1...Uk>` der `U` (oder, wenn `U` ein Typparameter ist, die effektive Basisklasse oder ein beliebiger Member seines effektiven Schnittstellen Satzes) mit identisch ist, von (direkt oder indirekt) `C<U1...Uk>`erbt oder (direkt oder indirekt) implementiert.

      (Die "Eindeutigkeits Einschränkung" bedeutet, dass bei der `C<T> {} class U: C<X>, C<Y> {}`der Fall Schnittstelle beim Ableiten von `U` zu `C<T>` kein Rückschluss erfolgt, da `U1` `X` oder `Y`werden könnte.)

   Wenn einer dieser Fälle zutrifft, erfolgt ein Rückschluss *von* jeder `Ui` *zum* entsprechenden `Vi` wie folgt:

   *  Wenn `Ui` nicht bekannt ist, dass es sich um einen Verweistyp handelt, wird ein *genauer Rückschluss* gemacht.
   *  Andernfalls, wenn `U` ein Arraytyp ist, wird ein Rück *Schluss mit niedrigerer Grenze* hergestellt.
   *  Wenn `V` `C<V1...Vk>`, ist der Rückschluss vom i-th-Typparameter von `C`abhängig:
      *  Wenn Sie kovariant ist, wird ein Rück *Schluss* ausgelöst.
      *  Wenn Sie kontra Variant ist, wird eine *obere gebundene Ableitung* vorgenommen.
      *  Wenn Sie invariante ist, wird ein *genauer Rückschluss* gemacht.
*  Andernfalls werden keine Rückschlüsse gemacht.

#### <a name="upper-bound-inferences"></a>Obere gebundene Rückschlüsse

Eine *obere gebundene Ableitung* *von* einem Typ `U` *zu* einem Typ `V` wird wie folgt durchgeführt:

*  Wenn `V` einer der *unfixen* `Xi` ist, wird `U` dem Satz von oberen Begrenzungen für `Xi`hinzugefügt.
*  Andernfalls legt `V1...Vk` fest, und `U1...Uk` werden bestimmt, indem überprüft wird, ob eines der folgenden Fälle zutrifft:
   *  `U` ist ein Arraytyp `U1[...]` und `V` ist ein Arraytyp `V1[...]` desselben Rang.
   *  `U` ist eine `IEnumerable<Ue>`, `ICollection<Ue>` oder `IList<Ue>` und `V` ist ein eindimensionaler Arraytyp `Ve[]`
   *  `U` ist der Typ `U1?` und `V` ist der Typ `V1?`
   *  `U` ist eine Klasse, eine Struktur, eine Schnittstelle oder ein Delegattyp `C<U1...Uk>` und `V` ist eine Klasse, Struktur, Schnittstelle oder Delegattyp, der mit identisch ist, von (direkt oder indirekt) und (direkt oder indirekt) einen eindeutigen Typ implementiert `C<V1...Vk>`

      (Die "Eindeutigkeits Einschränkung" bedeutet, dass bei `interface C<T>{} class V<Z>: C<X<Z>>, C<Y<Z>>{}`) kein Rückschluss erfolgt, wenn von `C<U1>` zu `V<Q>`abgeleitet wird. Rückschlüsse werden nicht von `U1` zu `X<Q>` oder `Y<Q>`vorgenommen.)

   Wenn einer dieser Fälle zutrifft, erfolgt ein Rückschluss *von* jeder `Ui` *zum* entsprechenden `Vi` wie folgt:
   *  Wenn `Ui` nicht bekannt ist, dass es sich um einen Verweistyp handelt, wird ein *genauer Rückschluss* gemacht.
   *  Andernfalls, wenn `V` ein Arraytyp ist, wird eine *obere gebundene Ableitung* vorgenommen.
   *  Wenn `U` `C<U1...Uk>`, ist der Rückschluss vom i-th-Typparameter von `C`abhängig:
      *  Wenn Sie kovariant ist, wird eine *obere gebundene Ableitung* vorgenommen.
      *  Wenn Sie kontra Variant ist, wird ein Rück *Schluss* festgestellt.
      *  Wenn Sie invariante ist, wird ein *genauer Rückschluss* gemacht.
*  Andernfalls werden keine Rückschlüsse gemacht.   

#### <a name="fixing"></a>Festnahme

Eine Variable vom Typ " *unfixed* " `Xi` mit einem Satz von Begrenzungen ist wie folgt *festgelegt* :

*  Der Satz von *Kandidaten Typen* `Uj` beginnt als Satz aller Typen im Satz von Begrenzungen für `Xi`.
*  Anschließend untersuchen wir die einzelnen gebundenen `Xi` wiederum: für jede exakte gebundene `U` `Xi` alle Typen `Uj` die nicht mit `U` identisch sind, werden aus dem Kandidaten Satz entfernt. Für jede Untergrenze `U` `Xi` alle Typen `Uj` die *keine* implizite Konvertierung von `U` aus dem Kandidaten Satz entfernt werden. Für jede obere Grenze `U` `Xi` alle Typen `Uj` aus denen *keine* implizite Konvertierung in `U` aus dem Kandidaten Satz entfernt werden.
*  Wenn zwischen den verbleibenden Kandidaten Typen `Uj` ein eindeutiger Typ `V` der eine implizite Konvertierung in alle anderen Kandidaten Typen enthält, wird `Xi` auf `V`korrigiert.
*  Andernfalls schlägt der Typrückschluss fehl.

#### <a name="inferred-return-type"></a>Rückgabetyp abgeleitet

Der abzurufende Rückgabetyp einer anonymen Funktion `F` bei der Typrückschluss-und Überladungs Auflösung verwendet werden. Der zurückgegebene Rückgabetyp kann nur für eine anonyme Funktion bestimmt werden, in der alle Parametertypen bekannt sind, da Sie entweder explizit angegeben werden, durch eine anonyme Funktions Konvertierung bereitgestellt oder während des Typrückschlusses auf einem einschließenden generischen abgeleitet werden. Methodenaufruf.

Der abhergelegte ***Ergebnistyp*** wird wie folgt bestimmt:

*  Wenn der Text des `F` ein *Ausdruck* ist, der über einen-Typ verfügt, ist der abherausgestellte Ergebnistyp von `F` der Typ dieses Ausdrucks.
*  Wenn der Text des `F` ein *Block* ist und der Satz von Ausdrücken in den `return` Anweisungen des Blocks einen am besten verbreiteten Typ `T` hat ([Ermitteln des am häufigsten verbreiteten Typs eines Satzes von Ausdrücken](expressions.md#finding-the-best-common-type-of-a-set-of-expressions)), wird der abhergelegte Ergebnistyp `F` `T`.
*  Andernfalls kann ein Ergebnistyp nicht für `F`abgeleitet werden.

Der ***zurückgegebene Rückgabetyp*** wird wie folgt bestimmt:

*  Wenn `F` asynchron ist und der Text des `F` entweder ein Ausdruck ist, der als Nothing ([Ausdrucks Klassifizierungen](expressions.md#expression-classifications)) klassifiziert ist, oder ein Anweisungsblock, bei dem keine Return-Anweisungen über Ausdrücke verfügen, ist der Rückschluss Rückgabetyp `System.Threading.Tasks.Task`
*  Wenn `F` asynchron ist und über einen abhergelegten Ergebnistyp `T`verfügt, ist der zurückgegebene Rückgabetyp `System.Threading.Tasks.Task<T>`.
*  Wenn `F` nicht Async ist und über einen abhergelegten Ergebnistyp `T`verfügt, ist der abherleitet Rückgabetyp `T`.
*  Andernfalls kann für `F`kein Rückgabetyp abgeleitet werden.

Sehen Sie sich als Beispiel für den Typrückschluss mit anonymen Funktionen die in der `System.Linq.Enumerable`-Klasse deklarierte `Select` Erweiterungsmethode an:
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

Angenommen, der `System.Linq`-Namespace wurde mit einer `using`-Klausel importiert, und eine Klasse `Customer` mit einer `Name`-Eigenschaft des Typs `string`, kann die `Select`-Methode verwendet werden, um die Namen einer Kundenliste auszuwählen:
```csharp
List<Customer> customers = GetCustomerList();
IEnumerable<string> names = customers.Select(c => c.Name);
```

Der Aufruf der Erweiterungsmethode ([Erweiterungs Methodenaufrufe](expressions.md#extension-method-invocations)) von `Select` wird verarbeitet, indem der Aufruf in einen statischen Methodenaufruf umgeschrieben wird:
```csharp
IEnumerable<string> names = Enumerable.Select(customers, c => c.Name);
```

Da Typargumente nicht explizit angegeben wurden, wird der Typrückschluss verwendet, um die Typargumente abzuleiten. Zuerst ist das `customers`-Argument mit dem `source`-Parameter verknüpft, sodass `T` `Customer`abgeleitet werden. Anschließend wird mit dem oben beschriebenen Prozess der anonymen Funktionstyp Ableitung `c` der Typ `Customer`und der Ausdruck `c.Name` mit dem Rückgabetyp des Parameters `selector` verknüpft, wobei `S` `string`werden. Daher entspricht der Aufruf dem
```csharp
Sequence.Select<Customer,string>(customers, (Customer c) => c.Name)
```
und das Ergebnis ist vom Typ `IEnumerable<string>`.

Im folgenden Beispiel wird veranschaulicht, wie der anonyme Funktionstyp Rückschluss das Eingeben von Typinformationen zwischen Argumenten in einem generischen Methodenaufruf zulässt. Bei Angabe der-Methode:
```csharp
static Z F<X,Y,Z>(X value, Func<X,Y> f1, Func<Y,Z> f2) {
    return f2(f1(value));
}
```

Typrückschluss für den Aufruf:
```csharp
double seconds = F("1:15:30", s => TimeSpan.Parse(s), t => t.TotalSeconds);
```
geht wie folgt vor: zuerst wird das Argument `"1:15:30"` mit dem `value`-Parameter verknüpft, sodass `X` `string`abgeleitet werden. Anschließend erhält der-Parameter der ersten anonymen Funktion (`s`) den abzurufenden Typ `string`, und der Ausdruck `TimeSpan.Parse(s)` ist mit dem Rückgabetyp `f1`verknüpft, wobei `Y` `System.TimeSpan`werden. Zum Schluss erhält der-Parameter der zweiten anonymen Funktion (`t`) den abzurufenden Typ `System.TimeSpan`, und der Ausdruck `t.TotalSeconds` ist mit dem Rückgabetyp `f2`verknüpft, wobei `Z` `double`werden. Daher ist das Ergebnis des auf-aufgabens vom Typ `double`.

#### <a name="type-inference-for-conversion-of-method-groups"></a>Typrückschluss für die Konvertierung von Methoden Gruppen

Ähnlich wie bei Aufrufen von generischen Methoden muss der Typrückschluss auch angewendet werden, wenn eine Methoden Gruppe `M` die eine generische Methode enthält, in einen angegebenen Delegattyp konvertiert wird `D` ([Methoden Gruppen Konvertierungen](conversions.md#method-group-conversions)). Bei Angabe einer Methode
```csharp
Tr M<X1...Xn>(T1 x1 ... Tm xm)
```
und die Methoden Gruppe `M` dem Delegattyp zugewiesen, `D` die Aufgabe des Typrückschlusses darin besteht, Typargumente `S1...Sn` zu suchen, sodass der Ausdruck:
```csharp
M<S1...Sn>
```
wird mit `D`kompatibel ([Delegatdeklarationen](delegates.md#delegate-declarations)).

Im Gegensatz zum Typrückschluss-Algorithmus für generische Methodenaufrufe gibt es in diesem Fall nur Argument *Typen*, keine Argument *Ausdrücke*. Insbesondere gibt es keine anonymen Funktionen und daher keinen Bedarf an mehreren Phasen des Inferenz.

Stattdessen werden alle `Xi` als *nicht korrigiert*betrachtet, und es wird ein Rück *Schluss* *von* jedem Argumenttyp `Uj` von `D` *zum* entsprechenden Parametertyp `Tj` von `M`ausgelöst. Wenn für eine der `Xi` keine Begrenzungen gefunden wurden, schlägt der Typrückschluss fehl. Andernfalls werden alle `Xi` mit den entsprechenden `Si`*korrigiert* , die das Ergebnis des Typrückschlusses sind.

#### <a name="finding-the-best-common-type-of-a-set-of-expressions"></a>Ermitteln des am häufigsten verbreiteten Typs eines Satzes von Ausdrücken

In einigen Fällen muss ein gemeinsamer Typ für einen Satz von Ausdrücken abgeleitet werden. Insbesondere werden die Elementtypen von implizit typisierten Arrays und die Rückgabe Typen anonymer Funktionen mit *Block* Text auf diese Weise gefunden.

Intuitiv, wenn ein Satz von Ausdrücken `E1...Em` dieser Rückschluss sollte dem Aufrufen einer Methode entsprechen.
```csharp
Tr M<X>(X x1 ... X xm)
```
mit dem `Ei` als Argumente.

Genauer ist, beginnt der Rückschluss mit einer Variablen vom Typ " *unfixed* Type" `X`. Dann werden *Ausgabetyp Rückschlüsse* *von* jeder *`Ei` `X`* . Zum Schluss wird `X` *korrigiert* , und wenn der Vorgang erfolgreich ist, ist der resultierende Typ `S` der resultierende am besten geeignete Typ für die Ausdrücke. Wenn keine dieser `S` vorhanden ist, haben die Ausdrücke keinen besten allgemeinen Typ.

### <a name="overload-resolution"></a>Überladungsauflösung

Bei der Überladungs Auflösung handelt es sich um einen Bindungs zeitmechanismus zum Auswählen des besten Funktionsmembers, der beim Aufrufen einer Argumentliste und eines Satzes von Kandidaten Funktions Membern aufgerufen wird. Die Überladungs Auflösung wählt den Funktions Member aus, der in den folgenden C#unterschiedlichen Kontexten in aufgerufen werden soll:

*  Aufruf einer Methode mit dem Namen in einem *invocation_expression* ([Methodenaufrufe](expressions.md#method-invocations)).
*  Aufruf eines Instanzkonstruktors, der in einem *object_creation_expression* namens ([Objekt Erstellungs Ausdrücke](expressions.md#object-creation-expressions)) genannt wird.
*  Aufruf eines Indexeraccessors über einen *element_access* ([Element Zugriff](expressions.md#element-access)).
*  Aufruf eines vordefinierten oder benutzerdefinierten Operators, auf den in einem Ausdruck verwiesen wird ([unäre Operator Überladungs Auflösung](expressions.md#unary-operator-overload-resolution) und [binäre Operator Überladungs Auflösung](expressions.md#binary-operator-overload-resolution)).

Jeder dieser Kontexte definiert den Satz von Kandidaten Funktionsmembern und die Liste der Argumente in der eigenen, eindeutigen Weise, wie in den oben aufgeführten Abschnitten ausführlich beschrieben. Beispielsweise enthält der Satz von Kandidaten für einen Methodenaufruf keine Methoden, die als `override` ([Member Suche](expressions.md#member-lookup)) markiert sind, und Methoden in einer Basisklasse sind keine Kandidaten, wenn eine Methode in einer abgeleiteten Klasse anwendbar ist (Methoden[Aufrufe](expressions.md#method-invocations)).

Nachdem die Kandidaten Funktions Member und die Argumentliste identifiziert wurden, ist die Auswahl des besten Funktionsmembers in allen Fällen identisch:

*  In Anbetracht der Menge der anwendbaren Kandidaten Funktionsmember befindet sich das beste Funktionsmember in dieser Gruppe. Wenn die Menge nur ein Funktionsmember enthält, ist dieses Funktionsmember das beste Funktionsmember. Andernfalls ist das beste Funktionsmember der ein Funktionsmember, der besser als alle anderen Funktionsmember in Bezug auf die angegebene Argumentliste ist, vorausgesetzt, dass jeder Funktionsmember mit allen anderen Funktionsmembern verglichen wird, wobei die Regeln in einem [besseren Funktionsmember](expressions.md#better-function-member)verwendet werden. Wenn nicht genau ein Funktionsmember vorhanden ist, der besser als alle anderen Funktionsmember ist, dann ist der Funktionselement Aufruf mehrdeutig, und ein Bindungs Fehler tritt auf.

In den folgenden Abschnitten wird die genaue Bedeutung des ***anwendbaren Funktionsmembers*** und des ***besseren Funktionsmembers***definiert.

#### <a name="applicable-function-member"></a>Anwendbares Funktionsmember

Ein Funktionsmember ist ein ***anwendbares Funktionsmember*** in Bezug auf eine Argumentliste `A`, wenn alle der folgenden Aussagen zutreffen:

*  Jedes Argument in `A` entspricht einem Parameter in der Deklaration des Funktions Members, wie in den [entsprechenden Parametern](expressions.md#corresponding-parameters)beschrieben, und jeder Parameter, dem kein Argument entspricht, ist ein optionaler Parameter.
*  Für jedes Argument in `A`ist der Parameter Übergabe Modus des Arguments (d. h. Value, `ref`oder `out`) mit dem Parameter Übergabe Modus des entsprechenden Parameters identisch.
   *  für einen value-Parameter oder ein Parameter Array ist eine implizite Konvertierung ([implizite Konvertierungen](conversions.md#implicit-conversions)) vom-Argument in den Typ des entsprechenden Parameters vorhanden, oder
   *  bei einem `ref`-oder `out`-Parameter ist der Typ des Arguments mit dem Typ des entsprechenden Parameters identisch. Schließlich handelt es sich bei einem `ref`-oder `out` Parameter um einen Alias für das übergebenen Argument.

Wenn ein Funktionsmember, der ein Parameter Array enthält, das Funktionsmember durch die oben genannten Regeln anwendbar ist, wird es in der ***normalen Form***als anwendbar bezeichnet. Wenn ein Funktionsmember, der ein Parameter Array enthält, nicht in der normalen Form anwendbar ist, kann der Funktions Member stattdessen in der ***erweiterten Form***angewendet werden:

*  Das erweiterte Formular wird erstellt, indem das Parameter Array in der Deklaration des Funktions Members durch Null oder mehr Wert Parameter des Elementtyps des Parameter Arrays ersetzt wird, sodass die Anzahl der Argumente in der Argumentliste `A` mit der Gesamtanzahl der Parameter übereinstimmt. Wenn `A` weniger Argumente aufweist als die Anzahl fester Parameter in der Deklaration des Funktions Members, kann die erweiterte Form des Funktionsmembers nicht erstellt werden und ist daher nicht anwendbar.
*  Andernfalls ist das erweiterte Formular anwendbar, wenn für jedes Argument in `A` der Parameter Übergabe Modus des Arguments mit dem Parameter Übergabe Modus des entsprechenden Parameters identisch ist, und
   *  bei einem Parameter mit festem Wert oder einem Wert Parameter, der durch die Erweiterung erstellt wurde, ist eine implizite Konvertierung ([implizite Konvertierungen](conversions.md#implicit-conversions)) vom Typ des Arguments in den Typ des entsprechenden Parameters vorhanden.
   *  bei einem `ref`-oder `out`-Parameter ist der Typ des Arguments mit dem Typ des entsprechenden Parameters identisch.

#### <a name="better-function-member"></a>Besseres Funktionsmember

Zum Ermitteln des besseren Funktionsmembers wird eine Liste der aus dem ausgebauten Argument enthaltenen Elemente erstellt, die nur die Argument Ausdrücke selbst in der Reihenfolge enthält, in der Sie in der ursprünglichen Argumentliste angezeigt werden.

Parameter Listen für jeden der Kandidaten Funktionsmember werden wie folgt erstellt:

*  Das erweiterte Formular wird verwendet, wenn das Funktionsmember nur in der erweiterten Form anwendbar ist.
*  Optionale Parameter ohne entsprechende Argumente werden aus der Parameterliste entfernt.
*  Die Parameter werden neu angeordnet, sodass Sie an derselben Position wie das entsprechende Argument in der Argumentliste auftreten.

Wenn eine Argumentliste `A` mit einem Satz von Argument Ausdrücken `{E1, E2, ..., En}` und zwei anwendbaren Funktionsmember `Mp` und `Mq` mit den Parametertypen `{P1, P2, ..., Pn}` und `{Q1, Q2, ..., Qn}`werden, wird `Mp` als ein ***besseres Funktionsmember*** definiert als `Mq`, wenn

*  die implizite Konvertierung von `Ex` in `Qx` ist für jedes Argument nicht besser geeignet als bei der impliziten Konvertierung von `Ex` in `Px`.
*  für mindestens ein Argument ist die Konvertierung von `Ex` in `Px` besser als die Konvertierung von `Ex` in `Qx`.

Wenn `Mp` oder `Mq` in der erweiterten Form anwendbar ist, verweist `Px` oder `Qx` auf einen Parameter in der erweiterten Form der Parameterliste.

Falls die Parametertyp Sequenzen `{P1, P2, ..., Pn}` und `{Q1, Q2, ..., Qn}` äquivalent sind (d. h., jede `Pi` hat eine Identitäts Konvertierung in die entsprechende `Qi`), werden die folgenden Regeln zum Durchbrechen von Regeln angewendet, um den besseren Funktionsmember zu ermitteln.

*  Wenn `Mp` eine nicht generische Methode ist und `Mq` eine generische Methode ist, ist `Mp` besser als `Mq`.
*  Wenn `Mp` in der normalen Form anwendbar ist und `Mq` ein `params` Array hat und nur in der erweiterten Form anwendbar ist, ist `Mp` besser als `Mq`.
*  Andernfalls ist `Mp` besser als `Mq`, wenn `Mp` über mehr deklarierte Parameter als `Mq`verfügt. Dies kann vorkommen, wenn beide Methoden `params` Arrays aufweisen und nur in ihren erweiterten Formularen anwendbar sind.
*  Andernfalls, wenn alle Parameter von `Mp` ein entsprechendes Argument aufweisen, während Standardargumente mindestens einen optionalen Parameter in `Mq` ersetzen müssen, ist `Mp` besser als `Mq`.
*  Andernfalls ist `Mp` besser als `Mq`, wenn `Mp` über spezifischere Parametertypen als `Mq`verfügt. Lassen Sie `{R1, R2, ..., Rn}` und `{S1, S2, ..., Sn}` die nicht instanziierten und nicht erweiterten Parametertypen `Mp` und `Mq`darstellen. die Parametertypen `Mp`sind spezifischer als `Mq`, wenn für jeden Parameter `Rx` nicht weniger spezifisch als `Sx`ist, und für mindestens einen Parameter ist `Rx` spezifischer als `Sx`:
   *  Ein Typparameter ist weniger spezifisch als ein nicht-Typparameter.
   *  Ein konstruierter Typ ist rekursiv spezifischer als ein anderer konstruierter Typ (mit der gleichen Anzahl von Typargumenten), wenn mindestens ein Typargument spezifischer ist und kein Typargument weniger spezifisch als das entsprechende Typargument in der anderen ist.
   *  Ein Arraytyp ist spezifischer als ein anderer Arraytyp (mit der gleichen Anzahl von Dimensionen), wenn der Elementtyp des ersten spezifischeren ist als der Elementtyp der zweiten.
*  Wenn ein Member ein nicht gesperrter Operator und der andere ein gesperrter Operator ist, ist der nicht--gesteigerte-Operator besser.
*  Andernfalls ist keines der Funktionsmember besser.

#### <a name="better-conversion-from-expression"></a>Bessere Konvertierung von Ausdrücken

Wenn ein impliziter Konvertierungs `C1`, der von einem Ausdruck `E` in einen Typ `T1`konvertiert, und ein impliziter Konvertierungs `C2`, der von einem Ausdruck `E` in einen Typ `T2`konvertiert, ist `C1` eine ***bessere Konvertierung*** als `C2`, wenn `E` nicht exakt mit `T2` übereinstimmt und mindestens einer der folgenden Punkte enthält:

* `E` genau Übereinstimmungen `T1` ([exakt übereinstimmenden Ausdruck](expressions.md#exactly-matching-expression))
* `T1` ist ein besseres Konvertierungs Ziel als `T2` ([besseres Konvertierungs Ziel](expressions.md#better-conversion-target)).

#### <a name="exactly-matching-expression"></a>Exakt übereinstimmender Ausdruck

Bei Angabe eines Ausdrucks `E` und eines Typs `T``E` genau mit `T` überein, wenn eine der folgenden Punkte enthält:

*  `E` hat einen Typ `S`, und eine Identitäts Konvertierung von `S` in `T`
*  `E` ist eine anonyme Funktion, `T` entweder ein Delegattyp `D` oder ein Ausdrucks bauentyp `Expression<D>` und eine der folgenden Punkte enthält:
   *  Ein abherkehrter Rückgabetyp `X` für `E` im Kontext der Parameterliste von `D` (abzurufenden[Rückgabetyp](expressions.md#inferred-return-type)) vorhanden ist, und eine Identitäts Konvertierung ist vom `X` zum Rückgabetyp `D`
   *  Entweder ist `E` nicht Async, und `D` hat einen Rückgabetyp `Y` oder `E` ist async, und `D` hat einen Rückgabetyp `Task<Y>`. eine der folgenden Optionen enthält:
      * Der Hauptteil der `E` ist ein Ausdruck, der genau übereinstimmt `Y`
      * Der Hauptteil des `E` ist ein Anweisungsblock, bei dem jede Return-Anweisung einen exakt übereinstimmenden Ausdruck zurückgibt `Y`

#### <a name="better-conversion-target"></a>Besseres Konvertierungs Ziel

Bei zwei verschiedenen Typen `T1` und `T2`ist `T1` ein besseres Konvertierungs Ziel als `T2`, wenn keine implizite Konvertierung von `T2` in `T1` vorhanden ist, und mindestens eine der folgenden Punkte enthält:

*  Eine implizite Konvertierung von `T1` in `T2` vorhanden ist.
*  `T1` ist entweder ein Delegattyp `D1` oder ein Ausdrucks bauentyp `Expression<D1>`, `T2` ist entweder ein Delegattyp `D2` oder ein Ausdrucks bauentyp `Expression<D2>`. `D1` hat einen Rückgabetyp `S1` und einer der folgenden Punkte:
   * die Rückgabe von `D2` ist ungültig.
   * `D2` hat einen Rückgabetyp `S2`, und `S1` ist ein besseres Konvertierungs Ziel als `S2`
*  `T1` ist `Task<S1>`, `T2` ist `Task<S2>`, und `S1` ist ein besseres Konvertierungs Ziel als `S2`
*  `T1` ist `S1` oder `S1?`, bei dem `S1` ein ganzzahliger Typ mit Vorzeichen ist, und `T2` `S2` oder `S2?` ist, wobei `S2` ein ganzzahliger Typ ohne Vorzeichen ist. Dies gilt insbesondere in folgenden Fällen:
   * `S1` ist `sbyte`, und `S2` ist `byte`, `ushort`, `uint`oder `ulong`
   * `S1` ist `short`, und `S2` ist `ushort`, `uint`oder `ulong`
   * `S1` ist `int`, und `S2` ist `uint`, oder `ulong`
   * `S1` ist `long`, und `S2` ist `ulong`

#### <a name="overloading-in-generic-classes"></a>Überladen in generischen Klassen

Obwohl Signaturen wie deklariert eindeutig sein müssen, ist es möglich, dass die Ersetzung von Typargumenten zu identischen Signaturen führt. In solchen Fällen werden die oben genannten Regeln der Überladungs Auflösung über den spezifischsten Member ausgewählt.

Die folgenden Beispiele zeigen über Ladungen, die gemäß dieser Regel gültig und ungültig sind:

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

### <a name="compile-time-checking-of-dynamic-overload-resolution"></a>Kompilierzeit Überprüfung der Auflösung dynamischer Überladungen

Bei den meisten dynamisch gebundenen Vorgängen ist der Satz möglicher Kandidaten für die Auflösung zur Kompilierzeit nicht bekannt. In bestimmten Fällen ist der Kandidaten Satz jedoch zum Zeitpunkt der Kompilierung bekannt:

*  Statische Methodenaufrufe mit dynamischen Argumenten
*  Instanzmethodenaufrufe, bei denen der Empfänger kein dynamischer Ausdruck ist
*  Indexer-Aufrufe, bei denen der Empfänger kein dynamischer Ausdruck ist
*  Konstruktoraufrufe mit dynamischen Argumenten

In diesen Fällen wird für jeden Kandidaten eine beschränkte Kompilierzeit Überprüfung durchgeführt, um festzustellen, ob eine dieser Vorgänge möglicherweise zur Laufzeit angewendet werden könnte. Diese Überprüfung umfasst die folgenden Schritte:

*  Partieller Typrückschluss: alle Typargumente, die nicht direkt oder indirekt auf ein Argument vom Typ "`dynamic`" angewiesen sind, werden mithilfe der Regeln des [Typrückschlusses](expressions.md#type-inference) abgeleitet. Die übrigen Typargumente sind unbekannt.
*  Partielle Anwendbarkeit der Anwendbarkeit: die Anwendbarkeit wird gemäß dem [anwendbaren Funktionsmember](expressions.md#applicable-function-member)überprüft, wobei Parameter, deren Typen unbekannt sind, ignoriert werden.
*  Wenn kein Kandidat diesen Test durchläuft, tritt ein Kompilierzeitfehler auf.

### <a name="function-member-invocation"></a>Funktionselement Aufruf

In diesem Abschnitt wird der Prozess beschrieben, der zur Laufzeit ausgeführt wird, um einen bestimmten Funktionsmember aufzurufen. Es wird davon ausgegangen, dass ein Bindungs Zeit Prozess bereits den aufzurufenden Member ermittelt hat, möglicherweise durch Anwenden der Überladungs Auflösung auf einen Satz von Kandidaten Funktionsmembern.

Um den Aufrufprozess zu beschreiben, werden Funktionsmember in zwei Kategorien unterteilt:

*  Statische Funktionsmember. Dabei handelt es sich um Instanzkonstruktoren, statische Methoden, statische Eigenschaftenaccessoren und benutzerdefinierte Operatoren. Statische Funktionsmember sind immer nicht virtuell.
*  Instanzfunktionsmember. Dabei handelt es sich um Instanzmethoden, Accessoren für Instanzeigenschaften und Indexer-Accessoren. Instanzfunktionsmember sind entweder nicht virtuell oder virtuell und werden immer für eine bestimmte Instanz aufgerufen. Die Instanz wird durch einen Instanzausdruck berechnet und kann innerhalb des Funktionsmembers als `this` ([dieser Zugriff](expressions.md#this-access)) aufgerufen werden.

Die Lauf Zeit Verarbeitung eines Funktionsmember-aufzurufenden besteht aus den folgenden Schritten, wobei `M` das Funktionsmember ist und, wenn `M` ein Instanzmember ist, `E` der Instanzausdruck ist:

*  Wenn `M` ein statisches Funktionsmember ist:
   * Die Argumentliste wird entsprechend der Beschreibung in den [Argumentlisten](expressions.md#argument-lists)ausgewertet.
   * `M` wird aufgerufen.

*  Wenn `M` ein Instanzfunktionsmember ist, der in einem *value_type*deklariert ist:
   * `E` wird ausgewertet. Wenn diese Auswertung eine Ausnahme verursacht, werden keine weiteren Schritte ausgeführt.
   * Wenn `E` nicht als Variable klassifiziert ist, wird eine temporäre lokale Variable des `E`Typs erstellt und der Wert von `E` der Variablen zugewiesen. `E` wird dann als Verweis auf diese temporäre lokale Variable neu klassifiziert. Der Zugriff auf die temporäre Variable ist in `M``this`, aber nicht auf andere Weise möglich. Daher kann der Aufrufer nur dann, wenn `E` eine echte Variable ist, die Änderungen beobachten, die `M` an `this`vornimmt.
   * Die Argumentliste wird entsprechend der Beschreibung in den [Argumentlisten](expressions.md#argument-lists)ausgewertet.
   * `M` wird aufgerufen. Die Variable, auf die `E` verweist, wird zur Variablen, auf die `this`verweist.

*  Wenn `M` ein Instanzfunktionsmember ist, der in einem *reference_type*deklariert ist:
   * `E` wird ausgewertet. Wenn diese Auswertung eine Ausnahme verursacht, werden keine weiteren Schritte ausgeführt.
   * Die Argumentliste wird entsprechend der Beschreibung in den [Argumentlisten](expressions.md#argument-lists)ausgewertet.
   * Wenn der `E` *value_type*ist, wird eine Boxing-Konvertierung ([Boxing-Konvertierungen](types.md#boxing-conversions)) ausgeführt, um `E` in den Typ `object`zu konvertieren, und `E` wird in den folgenden Schritten als Typ `object` eingestuft. In diesem Fall können `M` nur Mitglied `System.Object`sein.
   * Der Wert `E` wird als gültig geprüft. Wenn der Wert von `E` `null`ist, wird ein `System.NullReferenceException` ausgelöst, und es werden keine weiteren Schritte ausgeführt.
   * Die aufzurufende Funktionsmember-Implementierung wird bestimmt:
     * Wenn der Bindungstyp `E` eine Schnittstelle ist, ist das aufzurufende Funktionsmember die Implementierung von `M` der vom Lauf Zeittyp der Instanz bereitgestellt wird, auf die von `E`verwiesen wird. Dieses Funktionsmember wird bestimmt, indem die Schnittstellen Zuordnungs Regeln ([Schnittstellen Zuordnung](interfaces.md#interface-mapping)) angewendet werden, um die Implementierung von `M` zu bestimmen, die durch den Lauf Zeittyp der Instanz bereitgestellt wird, auf die `E`verweist.
     * Andernfalls ist das aufzurufende Funktionsmember die Implementierung von `M`, die vom Lauf Zeittyp der Instanz bereitgestellt wird, auf die von `E`verwiesen wird, wenn `M` ein virtuelles Funktionsmember ist. Dieses Funktionsmember wird bestimmt, indem die Regeln angewendet werden, um die am häufigsten abgeleitete Implementierung ([virtuelle Methoden](classes.md#virtual-methods)) `M` in Bezug auf den Lauf Zeittyp der Instanz zu bestimmen, auf die `E`verweist.
     * Andernfalls ist `M` ein nicht virtuelles Funktionsmember, und der aufzurufende Funktionsmember ist `M` selbst.
   * Die im obigen Schritt festgelegte Funktionsmember-Implementierung wird aufgerufen. Das Objekt, auf das `E` verweist, wird zu dem Objekt, auf das `this`verweist.

#### <a name="invocations-on-boxed-instances"></a>Aufrufe für geboxte Instanzen

Ein Funktionsmember, der in einem *value_type* implementiert ist, kann über eine geachtelte Instanz von aufgerufen werden, die in den folgenden Situationen *value_type* :

*  Wenn das Funktionsmember eine `override` einer Methode ist, die vom Typ `object` geerbt wird und durch einen Instanzausdruck vom Typ `object`aufgerufen wird.
*  Wenn das Funktionsmember eine Implementierung eines Schnittstellenfunktionsmembers ist und durch einen Instanzausdruck eines *INTERFACE_TYPE*aufgerufen wird.
*  Wenn der Funktionsmember durch einen Delegaten aufgerufen wird.

In diesen Fällen wird die geschachtelte Instanz als eine Variable des *value_type*enthalten, und diese Variable wird zu der Variablen, auf die `this` innerhalb des Funktionselement aufzurufenden. Dies bedeutet insbesondere, dass beim Aufrufen eines Funktionsmembers für eine geachtelte Instanz der Funktionsmember den in der geboxten Instanz enthaltenen Wert ändern kann.

## <a name="primary-expressions"></a>Primäre Ausdrücke

Primäre Ausdrücke enthalten die einfachsten Formen von Ausdrücken.

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

Primäre Ausdrücke werden zwischen *array_creation_expression*s und *primary_no_array_creation_expression*s aufgeteilt. Wenn Sie Array-Creation-Expression auf diese Weise behandeln, anstatt sie zusammen mit den anderen einfachen Ausdrucks Formularen aufzulisten, ermöglicht die Grammatik, potenziell verwirrenden Code, wie z. b.
```csharp
object o = new int[3][1];
```
der andernfalls interpretiert werden würde.
```csharp
object o = (new int[3])[1];
```

### <a name="literals"></a>Literale

Eine *primary_expression* , die aus einem Literalzeichen ([Literale](lexical-structure.md#literals)) besteht, wird als Wert klassifiziert.


### <a name="interpolated-strings"></a>Interpolierte Zeichenfolgen

Eine *interpolated_string_expression* besteht aus einem `$` Vorzeichen, gefolgt von einem regulären oder ausführlichen Zeichenfolgenliteral, bei dem Lücken durch `{` und `}`, einschließen von Ausdrücken und Formatierungs Spezifikationen getrennt werden. Ein interintererter Zeichen folgen Ausdruck ist das Ergebnis einer *interpolated_string_literal* , die in einzelne Token unterteilt wurde, wie in [interpoliert-Zeichenfolgenliteralen](lexical-structure.md#interpolated-string-literals)beschrieben.

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

Der *constant_expression* in einer interpolung muss über eine implizite Konvertierung in `int`verfügen.

Ein *interpolated_string_expression* wird als Wert klassifiziert. Wenn Sie sofort in `System.IFormattable` oder `System.FormattableString` mit einer impliziten interkrelierten Zeichen folgen Konvertierung ([implizite interpolierte Zeichen folgen Konvertierungen](conversions.md#implicit-interpolated-string-conversions)) konvertiert wird, hat der interpoliert Zeichen folgen Ausdruck diesen Typ. Andernfalls weist Sie den Typ `string`auf.

Wenn der Typ einer interinterierten Zeichenfolge `System.IFormattable` oder `System.FormattableString`ist, ist die Bedeutung ein aufrufender `System.Runtime.CompilerServices.FormattableStringFactory.Create`. Wenn der Typ `string`ist, ist die Bedeutung des Ausdrucks ein Aufruf`string.Format`. In beiden Fällen besteht die Argumentliste des Aufrufes aus einem Format Zeichenfolgenliteralzeichen mit Platzhaltern für jede interpolung und einem Argument für jeden Ausdruck, der den Platzhaltern entspricht.

Der Format Zeichenfolgenliterale wird wie folgt erstellt, wobei `N` die Anzahl der Interpolationen in der *interpolated_string_expression*ist:

*  Wenn ein *interpolated_regular_string_whole* oder eine *interpolated_verbatim_string_whole* auf das `$` Vorzeichen folgt, ist das Format Zeichenfolgenliteralzeichen dieses Token.
*  Andernfalls besteht das Format Zeichenfolgenliterale aus folgendem: 
   *  Zuerst die *interpolated_regular_string_start* oder *interpolated_verbatim_string_start*
   *  Dann für jede Anzahl `I` von `0` bis `N-1`: 
      * Die Dezimal Darstellung von `I`
      * Wenn die entsprechende *Interpolations* *constant_expression*ist, wird ein `,` (Komma) gefolgt von der Dezimal Darstellung des Werts des *constant_expression*
      * Anschließend werden die *interpolated_regular_string_mid*, die *interpolated_regular_string_end*, *interpolated_verbatim_string_mid* oder *interpolated_verbatim_string_end* unmittelbar auf die entsprechende interpolung folgt.

Die nachfolgenden Argumente sind einfach die *Ausdrücke* aus den *Interpolationen* (sofern vorhanden), in der richtigen Reihenfolge.

TODO: Beispiele.


### <a name="simple-names"></a>Einfache Namen

Eine *Simple_name* besteht aus einem Bezeichner, optional gefolgt von einer Typargument Liste:

```antlr
simple_name
    : identifier type_argument_list?
    ;
```

Eine *Simple_name* hat entweder das Formular `I` oder die Form `I<A1,...,Ak>`, wobei `I` ein einzelner Bezeichner und `<A1,...,Ak>` ein optionaler *type_argument_list*ist. Wenn keine *type_argument_list* angegeben ist, sollten Sie `K` NULL angeben. Die *Simple_name* wird wie folgt ausgewertet und klassifiziert:

*  Wenn `K` 0 (null) ist und die *Simple_name* in einem- *Block* angezeigt wird und der *Block*(oder ein einschließender *Block*) der Deklaration der lokalen Variablen Deklaration ([Deklarationen](basic-concepts.md#declarations)) eine lokale Variable, einen Parameter oder eine Konstante mit dem Namen `I`enthält, verweist der *Simple_name* auf diese lokale Variable, den Parameter oder die Konstante und wird als Variable oder Wert klassifiziert.
*  Wenn `K` NULL ist und die *Simple_name* im Text einer generischen Methoden Deklaration angezeigt wird und diese Deklaration einen Typparameter mit dem Namen `I`enthält, verweist der *Simple_name* auf diesen Typparameter.
*  Andernfalls für jeden Instanztyp `T` ([der Instanztyp](classes.md#the-instance-type)), beginnend mit dem Instanztyp der unmittelbar einschließenden Typdeklaration und fortsetzen mit dem Instanztyp jeder einschließenden Klasse oder Struktur Deklaration (sofern vorhanden):
   *  Wenn `K` NULL ist und die Deklaration von `T` einen Typparameter mit dem Namen `I`enthält, verweist der *Simple_name* auf diesen Typparameter.
   *  Andernfalls führt ein Element Suchvorgang ([Member Suche](expressions.md#member-lookup)) von `I` in `T` mit `K` Typargumenten zu einer Entsprechung:
      * Wenn `T` der Instanztyp der unmittelbar einschließenden Klasse oder des Struktur Typs ist und die Suche eine oder mehrere Methoden identifiziert, ist das Ergebnis eine Methoden Gruppe mit einem zugeordneten Instanzausdruck `this`. Wenn eine Typargument Liste angegeben wurde, wird Sie beim Aufrufen einer generischen Methode ([Methodenaufrufe](expressions.md#method-invocations)) verwendet.
      * Andernfalls, wenn `T` der Instanztyp der unmittelbar einschließenden Klasse oder des Struktur Typs ist, wenn die Suche einen Instanzmember identifiziert und der Verweis innerhalb des Texts eines Instanzkonstruktors, einer Instanzmethode oder eines Instanzaccessors auftritt, ist das Ergebnis das gleiche wie ein Element Zugriff ([Member Access](expressions.md#member-access)) der Formular `this.I`. Dies kann nur vorkommen, wenn `K` 0 (null) ist.
      * Andernfalls ist das Ergebnis das gleiche wie ein Element Zugriff ([Member Access](expressions.md#member-access)) der Form `T.I` oder `T.I<A1,...,Ak>`. In diesem Fall handelt es sich um einen Bindungs Zeit Fehler, damit der *Simple_name* auf einen Instanzmember verweist.

*  Andernfalls werden die folgenden Schritte für jeden Namespace `N`, beginnend mit dem Namespace, in dem der *Simple_name* auftritt, mit jedem einschließenden Namespace (sofern vorhanden) und mit dem globalen Namespace fortgesetzt, bis eine Entität gefunden wird:
   *  Wenn `K` NULL und `I` der Name eines Namespace in `N`ist, dann gilt Folgendes:
      * Wenn der Speicherort, an dem der *Simple_name* auftritt, von einer Namespace Deklaration für `N` eingeschlossen ist und die Namespace Deklaration eine *extern_alias_directive* oder *using_alias_directive* enthält, die den Namen `I` einem Namespace oder Typ zuordnet, ist die *Simple_name* mehrdeutig, und es tritt ein Kompilierungsfehler auf.
      * Andernfalls verweist der *Simple_name* auf den Namespace mit dem Namen `I` in `N`.
   *  Wenn `N` einen zugänglichen Typ mit dem Namen `I` und `K` Typparameter enthält, andernfalls:
      * Wenn `K` NULL ist und der Speicherort der *Simple_name* auftritt, wird eine Namespace Deklaration für `N` eingeschlossen, und die Namespace Deklaration enthält eine *extern_alias_directive* oder *using_alias_directive* , die den Namen `I` einem Namespace oder Typ zuordnet. der *Simple_name* ist dann mehrdeutig und ein Kompilierungsfehler tritt auf.
      * Andernfalls verweist der *namespace_or_type_name* auf den Typ, der mit den angegebenen Typargumenten erstellt wurde.
   *  Andernfalls, wenn der Speicherort, an dem der *Simple_name* auftritt, von einer Namespace Deklaration für `N`eingeschlossen wird:
      * Wenn `K` NULL ist und die-Namespace Deklaration eine *extern_alias_directive* oder *using_alias_directive* enthält, die den Namen `I` einem importierten Namespace oder Typ zuordnet, verweist der *Simple_name* auf diesen Namespace oder Typ.
      * Wenn die Namespaces und Typdeklarationen, die von den *using_namespace_directive*s und *using_static_directive*s der Namespace Deklaration importiert werden, genau einen zugreif baren Typ oder einen nicht-Erweiterungs statischen Member mit dem Namen `I` und `K` Typparametern enthalten, verweist der *Simple_name* auf diesen Typ oder Member, der mit den angegebenen Typargumenten erstellt wurde.
      * Wenn die Namespaces und Typen, die durch die *using_namespace_directive*s der Namespace Deklaration importiert werden, mehr als einen zugreif baren Typ oder ein statisches Element ohne Erweiterungsmethode mit dem Namen `I` und `K` Typparametern enthalten, ist die *Simple_name* mehrdeutig, und es tritt ein Fehler auf.

   Beachten Sie, dass der gesamte Schritt genau mit dem entsprechenden Schritt bei der Verarbeitung eines *namespace_or_type_name* ([Namespace-und Typnamen](basic-concepts.md#namespace-and-type-names)) identisch ist.

*  Andernfalls ist der *Simple_name* nicht definiert, und es tritt ein Kompilierzeitfehler auf.


### <a name="parenthesized-expressions"></a>Ausdrücke in Klammern

Ein *parenthesized_expression* besteht aus einem *Ausdruck* , der in Klammern eingeschlossen ist.

```antlr
parenthesized_expression
    : '(' expression ')'
    ;
```

Eine *parenthesized_expression* wird ausgewertet, indem der *Ausdruck* innerhalb der Klammern ausgewertet wird. Wenn der *Ausdruck* innerhalb der Klammern einen Namespace oder Typ angibt, tritt ein Kompilierzeitfehler auf. Andernfalls ist das Ergebnis der *parenthesized_expression* das Ergebnis der Auswertung des enthaltenen *Ausdrucks*.

### <a name="member-access"></a>Memberzugriff

Eine *member_access* besteht aus einem *primary_expression*, einem *predefined_type*oder einem *qualified_alias_member*, gefolgt von einem "`.`"-Token, gefolgt von einem *Bezeichner*, optional gefolgt von einem *type_argument_list*.

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

Die *qualified_alias_member* Produktion wird in [Namespacealias-Qualifizierern](namespaces.md#namespace-alias-qualifiers)definiert.

Ein *member_access* hat entweder das Formular `E.I` oder die Form `E.I<A1, ..., Ak>`, wobei `E` ein primärer Ausdruck ist, `I` ein einzelner Bezeichner und `<A1, ..., Ak>` ein optionaler *type_argument_list*ist. Wenn keine *type_argument_list* angegeben ist, sollten Sie `K` NULL angeben.

Eine *member_access* mit einer *primary_expression* vom Typ `dynamic` ist dynamisch gebunden ([dynamische Bindung](expressions.md#dynamic-binding)). In diesem Fall klassifiziert der Compiler den Member Access als Eigenschaften Zugriff vom Typ `dynamic`. Die nachstehenden Regeln zum Bestimmen der Bedeutung der *member_access* werden dann zur Laufzeit angewendet und verwenden den Lauf Zeittyp anstelle des Kompilierzeit Typs der *primary_expression*. Wenn diese Lauf Zeit Klassifizierung zu einer Methoden Gruppe führt, muss der Element Zugriff die *primary_expression* eines *invocation_expression*sein.

Die *member_access* wird wie folgt ausgewertet und klassifiziert:

*  Wenn `K` NULL ist und `E` ein Namespace und `E` einen schsted Namespace mit dem Namen `I`enthält, ist das Ergebnis dieser Namespace.
*  Wenn `E` ein Namespace ist und `E` einen zugänglichen Typ mit dem Namen `I` und `K` Typparametern enthält, ist das Ergebnis der Typ, der mit den angegebenen Typargumenten erstellt wurde.
*  Wenn `E` ein *predefined_type* oder ein *primary_expression* ist, das als Typ klassifiziert ist, und wenn `E` kein Typparameter ist und eine Element Suche (Element[Suche](expressions.md#member-lookup)) `I` in `E` mit `K`Typparametern eine Entsprechung erzeugt, wird  wie folgt ausgewertet und klassifiziert:
   *  Wenn `I` einen Typ identifiziert, ist das Ergebnis der Typ, der mit den angegebenen Typargumenten erstellt wurde.
   *  Wenn `I` eine oder mehrere Methoden identifiziert, ist das Ergebnis eine Methoden Gruppe ohne zugeordneten Instanzausdruck. Wenn eine Typargument Liste angegeben wurde, wird Sie beim Aufrufen einer generischen Methode ([Methodenaufrufe](expressions.md#method-invocations)) verwendet.
   *  Wenn `I` eine `static` Eigenschaft identifiziert, ist das Ergebnis ein Eigenschaften Zugriff ohne zugeordneten Instanzausdruck.
   *  Wenn `I` ein `static` Feld identifiziert:
      * Wenn das Feld `readonly` und der Verweis außerhalb des statischen Konstruktors der Klasse oder Struktur auftritt, in der das Feld deklariert ist, dann ist das Ergebnis ein Wert, d. h. der Wert des statischen Felds `I` in `E`.
      * Andernfalls ist das Ergebnis eine Variable, d. h. das statische Feld `I` in `E`.
   *  Wenn `I` ein `static` Ereignis identifiziert:
      * Wenn der Verweis innerhalb der Klasse oder Struktur auftritt, in der das Ereignis deklariert ist, und das Ereignis ohne *event_accessor_declarations* ([Ereignisse](classes.md#events)) deklariert wurde, wird `E.I` genau so verarbeitet, als wäre `I` ein statisches Feld.
      * Andernfalls ist das Ergebnis ein Ereignis Zugriff ohne zugeordneten Instanzausdruck.
   *  Wenn `I` eine Konstante identifiziert, ist das Ergebnis ein Wert, nämlich der Wert dieser Konstante.
    * Wenn `I` einen Enumerationsmember identifiziert, ist das Ergebnis ein Wert, nämlich der Wert dieses Enumerationsmembers.
    * Andernfalls ist `E.I` ein ungültiger Element Verweis, und es tritt ein Kompilierzeitfehler auf.
*  Wenn `E` ein Eigenschaften Zugriff, Indexer-Zugriff, Variable oder Wert ist, der Typ, der `T`ist, und eine Element Suche (Element[Suche](expressions.md#member-lookup)) von `I` in `T` mit `K` Typargumenten eine Entsprechung erzeugt, wird `E.I` wie folgt ausgewertet und klassifiziert:
   *  Wenn `E` eine Eigenschaft oder ein Indexer-Zugriff ist, wird der Wert der Eigenschaft oder des Indexerzugriffs abgerufen ([Werte von Ausdrücken](expressions.md#values-of-expressions)), und `E` wird als Wert neu klassifiziert.
   *  Wenn `I` eine oder mehrere Methoden identifiziert, ist das Ergebnis eine Methoden Gruppe mit einem zugeordneten Instanzausdruck `E`. Wenn eine Typargument Liste angegeben wurde, wird Sie beim Aufrufen einer generischen Methode ([Methodenaufrufe](expressions.md#method-invocations)) verwendet.
   *  Wenn `I` eine Instanzeigenschaft identifiziert,
      * Wenn `E` `this`ist, identifiziert `I` eine automatisch implementierte Eigenschaft ([automatisch implementierte Eigenschaften](classes.md#automatically-implemented-properties)) ohne Setter, und der Verweis erfolgt innerhalb eines Instanzkonstruktors für einen Klassen-oder Strukturtyp `T`. das Ergebnis ist eine Variable, d. h. das verborgene dahinter liegende Feld für die automatische Eigenschaft, die durch `I` in der von `T` angegebenen Instanz von angegeben wird.
      * Andernfalls ist das Ergebnis ein Eigenschaften Zugriff mit einem zugeordneten Instanzausdruck `E`.
   *  Wenn `T` ein *class_type* ist und `I` ein Instanzfeld dieses *class_type*angibt:
      * Wenn der Wert `E` `null`ist, wird eine `System.NullReferenceException` ausgelöst.
      * Andernfalls ist das Ergebnis ein Wert, wenn das Feld `readonly` und der Verweis außerhalb eines Instanzkonstruktors der Klasse auftritt, in der das Feld deklariert ist. das Ergebnis ist ein Wert, d. h. der Wert des Felds `I` in dem Objekt, auf das von `E`verwiesen wird.
      * Andernfalls ist das Ergebnis eine Variable, d. h. das Feld `I` in dem Objekt, auf das von `E`verwiesen wird.
   *  Wenn `T` ein *struct_type* ist und `I` ein Instanzfeld dieses *struct_type*angibt:
      * Wenn `E` ein Wert ist oder wenn das Feld `readonly` ist und der Verweis außerhalb eines Instanzkonstruktors der Struktur auftritt, in der das Feld deklariert ist, dann ist das Ergebnis ein Wert, d. h. der Wert des Felds `I` in der Struktur Instanz, die von `E`angegeben wird.
      * Andernfalls ist das Ergebnis eine Variable, nämlich das Feld, das in der Struktur Instanz `I`, die von `E`angegeben wird.
   *  Wenn `I` ein Instanzereignis identifiziert:
      * Wenn der Verweis innerhalb der Klasse oder Struktur auftritt, in der das Ereignis deklariert ist, und das Ereignis ohne *event_accessor_declarations* ([Ereignisse](classes.md#events)) deklariert wurde, und der Verweis nicht als linke Seite eines `+=` oder `-=` Operators auftritt, wird `E.I` genau so verarbeitet, als wäre `I` ein Instanzfeld.
      * Andernfalls ist das Ergebnis ein Ereignis Zugriff mit einem zugeordneten Instanzausdruck `E`.
*  Andernfalls wird versucht, `E.I` als Aufruf der Erweiterungsmethode zu verarbeiten ([Erweiterungs Methodenaufrufe](expressions.md#extension-method-invocations)). Wenn dies nicht möglich ist, ist `E.I` ein ungültiger Element Verweis, und es tritt ein Bindungs Zeitfehler auf.

#### <a name="identical-simple-names-and-type-names"></a>Identische einfache Namen und Typnamen

Bei einem Element Zugriff auf das Formular `E.I`, wenn `E` ein einzelner Bezeichner ist und die Bedeutung von `E` als *Simple_name* ([einfache Namen](expressions.md#simple-names)) eine Konstante, ein Feld, eine Eigenschaft, eine lokale Variable oder ein Parameter mit demselben Typ wie die Bedeutung von `E` als *TYPE_NAME* ([Namespace-und Typnamen](basic-concepts.md#namespace-and-type-names)) ist, sind beide möglichen Bedeutungen von `E` zulässig. Die beiden möglichen Bedeutungen von `E.I` sind nie mehrdeutig, da `I` notwendigerweise ein Member des Typs `E` in beiden Fällen sein muss. Mit anderen Worten: die Regel gestattet den Zugriff auf die statischen Member und die schsted Typen von `E`, in denen andernfalls ein Kompilierzeitfehler aufgetreten ist. Beispiel:
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

Die Produktion für *Simple_name* ([einfache Namen](expressions.md#simple-names)) und *member_access* ([Member Access](expressions.md#member-access)) kann zu Mehrdeutigkeiten in der Grammatik für Ausdrücke führen. Beispielsweise ist die-Anweisung:
```csharp
F(G<A,B>(7));
```
kann als `F` mit zwei Argumenten interpretiert werden, `G < A` und `B > (7)`. Alternativ könnte Sie auch als `F` mit einem Argument interpretiert werden, bei dem es sich um einen aufzurufenden generischen Methoden `G` mit zwei Typargumenten und einem regulären Argument handelt.

Wenn eine Sequenz von Token (im Kontext) als *Simple_name* ([einfache Namen](expressions.md#simple-names)), *member_access* ([Member Access](expressions.md#member-access)) oder *pointer_member_access* ([Zeiger Element Zugriff](unsafe-code.md#pointer-member-access)) mit einer *type_argument_list* ([Typargumente](types.md#type-arguments)) analysiert werden kann, wird das Token, das unmittelbar auf das schließende `>` Token folgt, untersucht. Wenn eine von
```csharp
(  )  ]  }  :  ;  ,  .  ?  ==  !=  |  ^
```
Anschließend wird der *type_argument_list* als Teil der *Simple_name*, *member_access* oder *pointer_member_access* aufbewahrt, und jede andere mögliche Analyse der Sequenz von Token wird verworfen. Andernfalls wird der *type_argument_list* nicht als Teil der *Simple_name*, *member_access* oder *pointer_member_access*betrachtet, auch wenn es keine andere Möglichkeit gibt, die Sequenz von Token zu analysieren. Beachten Sie, dass diese Regeln beim Durchlaufen einer *type_argument_list* in einer *namespace_or_type_name* ([Namespace-und Typnamen](basic-concepts.md#namespace-and-type-names)) nicht angewendet werden. Die Anweisung
```csharp
F(G<A,B>(7));
```
wird gemäß dieser Regel als ein `F` mit einem Argument interpretiert, bei dem es sich um einen aufzurufenden generischen Methoden `G` mit zwei Typargumenten und einem regulären Argument handelt. Die Anweisungen
```csharp
F(G < A, B > 7);
F(G < A, B >> 7);
```
wird als `F` mit zwei Argumenten interpretiert. Die Anweisung
```csharp
x = F < A > +y;
```
wird als Operator kleiner als, größer als und Unärer Plus-Operator interpretiert, als ob die Anweisung `x = (F < A) > (+y)`geschrieben worden wäre, anstatt als *Simple_name* mit einem *type_argument_list* gefolgt von einem binären Plus-Operator. In der-Anweisung
```csharp
x = y is C<T> + z;
```
die Token `C<T>` werden als *namespace_or_type_name* mit einem *type_argument_list*interpretiert.

### <a name="invocation-expressions"></a>Aufrufausdrücke

Ein *invocation_expression* wird verwendet, um eine Methode aufzurufen.

```antlr
invocation_expression
    : primary_expression '(' argument_list? ')'
    ;
```

Ein- *invocation_expression* ist dynamisch gebunden ([dynamische Bindung](expressions.md#dynamic-binding)), wenn mindestens einer der folgenden Punkte enthält:

* Der *primary_expression* hat den Typ `dynamic`der Kompilierzeit.
* Mindestens ein Argument des optionalen *argument_list* hat den Typ "Kompilierzeit" `dynamic`, und der *primary_expression* weist keinen Delegattyp auf.

In diesem Fall klassifiziert der Compiler die *invocation_expression* als Wert des Typs `dynamic`. Die nachstehenden Regeln zum Bestimmen der Bedeutung der *invocation_expression* werden dann zur Laufzeit angewendet. dabei wird der Lauf Zeittyp anstelle des Kompilier Zeittyps der *primary_expression* und Argumente verwendet, die den Typ der Kompilierzeit `dynamic`haben. Wenn die *primary_expression* keinen Kompilier Zeittyp `dynamic`hat, wird der Methodenaufruf einer begrenzten Kompilierungszeit Überprüfung unterzogen, wie in der [Kompilierzeit Überprüfung der dynamischen Überladungs Auflösung](expressions.md#compile-time-checking-of-dynamic-overload-resolution)beschrieben.

Der *primary_expression* eines *invocation_expression* muss eine Methoden Gruppe oder ein Wert eines *delegate_type*sein. Wenn die *primary_expression* eine Methoden Gruppe ist, ist die *invocation_expression* ein Methodenaufruf ([Methodenaufrufe](expressions.md#method-invocations)). Wenn die *primary_expression* ein Wert eines *delegate_type*ist, ist der *invocation_expression* ein Delegataufruf ([Delegataufrufe](expressions.md#delegate-invocations)). Wenn die *primary_expression* weder eine Methoden Gruppe noch der Wert eines *delegate_type*ist, tritt ein Bindungs Fehler auf.

Die optionalen *argument_list* ([Argumentlisten](expressions.md#argument-lists)) stellen Werte oder Variablen Verweise für die Parameter der Methode bereit.

Das Ergebnis der Auswertung eines *invocation_expression* wird wie folgt klassifiziert:

*  Wenn die *invocation_expression* eine Methode oder einen Delegaten aufruft, die `void`zurückgibt, ist das Ergebnis "Nothing". Ein Ausdruck, der als "Nothing" klassifiziert wird, ist nur im Kontext einer *statement_expression* ([Ausdrucks Anweisungen](statements.md#expression-statements)) oder als Text eines *lambda_expression* ([Anonyme Funktions Ausdrücke](expressions.md#anonymous-function-expressions)) zulässig. Andernfalls tritt ein Bindungs Zeitfehler auf.
*  Andernfalls ist das Ergebnis ein Wert des Typs, der von der-Methode oder dem-Delegaten zurückgegeben wird.

#### <a name="method-invocations"></a>Methodenaufrufe

Bei einem Methodenaufruf muss die *primary_expression* der *invocation_expression* eine Methoden Gruppe sein. Die Methoden Gruppe identifiziert die eine Methode, die aufgerufen werden soll, oder den Satz überladener Methoden, von denen eine bestimmte aufzurufende Methode ausgewählt werden soll. Im letzteren Fall basiert die Bestimmung der aufzurufenden Methode auf dem Kontext, der von den Typen der Argumente im *argument_list*bereitgestellt wird.

Die Bindungs Zeit Verarbeitung eines Methoden aufruhens der Form `M(A)`, wobei `M` eine Methoden Gruppe (möglicherweise ein *type_argument_list*) und `A` eine optionale *argument_list*ist, besteht aus den folgenden Schritten:

*  Der Satz von Kandidaten Methoden für den Methodenaufruf wird erstellt. Für jede Methode `F`, die der Methoden Gruppe `M`zugeordnet ist:
   *  Wenn `F` nicht generisch ist, ist `F` in folgenden den folgenden Kandidaten:
      * `M` hat keine Typargument Liste und
      * `F` ist in Bezug auf `A` ([anwendbares Funktionsmember](expressions.md#applicable-function-member)) anwendbar.
   *  Wenn `F` generisch ist und `M` keine Typargument Liste aufweist, ist `F` in folgenden den folgenden Kandidaten:
      * Der Typrückschluss ([Typrückschluss](expressions.md#type-inference)) ist erfolgreich, wobei eine Liste der Typargumente für den-Befehl abgeleitet wird.
      * Nachdem die abzurufenden Typargumente die entsprechenden Methodentypparameter ersetzt haben, erfüllen alle konstruierten Typen in der Parameterliste von F ihre Einschränkungen (die die[Einschränkungen erfüllen](types.md#satisfying-constraints)), und die Parameterliste `F` ist in Bezug auf `A` ([anwendbares Funktionsmember](expressions.md#applicable-function-member)) anwendbar.
   *  Wenn `F` generisch ist und `M` eine Typargument Liste enthält, ist `F` in folgenden den folgenden Kandidaten:
      * `F` hat dieselbe Anzahl von Methodentypparametern, die in der Typargument Liste angegeben wurden, und
      * Nachdem die Typargumente die entsprechenden Methodentypparameter ersetzt haben, erfüllen alle konstruierten Typen in der Parameterliste von F ihre Einschränkungen (die die[Einschränkungen erfüllen](types.md#satisfying-constraints)), und die Parameterliste `F` ist in Bezug auf `A` ([anwendbares Funktionsmember](expressions.md#applicable-function-member)) anwendbar.
*  Der Satz von Kandidaten Methoden wird so reduziert, dass er nur Methoden der am weitesten abgeleiteten Typen enthält: für jede Methode `C.F` in der Menge, bei der `C` der Typ ist, in dem die Methode `F` deklariert ist, werden alle in einem Basistyp von `C` deklarierten Methoden aus dem Satz entfernt. Wenn `C` ein anderer Klassentyp als `object`ist, werden alle in einem Schnittstellentyp deklarierten Methoden aus dem Satz entfernt. (Diese letztere Regel hat nur Auswirkungen, wenn die Methoden Gruppe das Ergebnis einer Member-Suche nach einem Typparameter ist, der eine effektive Basisklasse als Object und eine nicht leere effektive Schnittstellen Menge aufweist.)
*  Wenn der resultierende Satz von Kandidaten Methoden leer ist, wird die weitere Verarbeitung der folgenden Schritte abgebrochen, und stattdessen wird versucht, den Aufruf als Erweiterungs Methodenaufruf ([Erweiterungs Methodenaufrufe](expressions.md#extension-method-invocations)) zu verarbeiten. Wenn dies fehlschlägt, gibt es keine anwendbaren Methoden, und ein Bindungs Fehler tritt auf.
*  Die beste Methode des Satzes von Kandidaten Methoden wird mithilfe der Regeln zur Überladungs Auflösung der [Überladungs Auflösung](expressions.md#overload-resolution)identifiziert. Wenn eine einzige optimale Methode nicht identifiziert werden kann, ist der Methodenaufruf mehrdeutig, und es tritt ein Fehler bei der Bindung auf. Beim Durchführen der Überladungs Auflösung werden die Parameter einer generischen Methode berücksichtigt, nachdem die Typargumente (bereitgestellt oder abgeleitet) für die entsprechenden Methodentyp Parameter ersetzt wurden.
*  Die endgültige Überprüfung der ausgewählten optimalen Methode wird ausgeführt:
   * Die-Methode wird im Kontext der-Methoden Gruppe überprüft: Wenn es sich bei der besten Methode um eine statische Methode handelt, muss die Methoden Gruppe aus einer *Simple_name* oder einer *member_access* durch einen Typ resultieren. Wenn die beste Methode eine Instanzmethode ist, muss die Methoden Gruppe durch eine *Simple_name*, eine *member_access* durch eine Variable oder einen Wert oder eine *base_access*resultieren. Wenn keine dieser Anforderungen erfüllt ist, tritt ein Fehler bei der Bindung auf.
   * Wenn es sich bei der besten Methode um eine generische Methode handelt, werden die Typargumente (bereitgestellt oder abgeleitet) mit den Einschränkungen (die sich durch die[Erfüllung der Einschränkungen](types.md#satisfying-constraints)in der generischen Methode) überprüfen Wenn ein Typargument die entsprechenden Einschränkungen für den Typparameter nicht erfüllt, tritt ein Bindungs Zeitfehler auf.

Nachdem eine Methode mithilfe der obigen Schritte zur Bindungs Zeit ausgewählt und überprüft wurde, wird der tatsächliche Lauf Zeit Aufruf gemäß den Regeln für den Funktionselement Aufruf verarbeitet, der unter [Kompilierzeit Überprüfung der dynamischen Überladungs Auflösung](expressions.md#compile-time-checking-of-dynamic-overload-resolution)beschrieben wird.

Die intuitiver Auswirkung der oben beschriebenen Auflösungs Regeln lautet wie folgt: um die von einem Methodenaufruf aufgerufene Methode zu suchen, beginnen Sie mit dem Typ, der durch den Methodenaufruf angegeben wird, und setzen Sie die Vererbungs Kette fort, bis mindestens eine zutreffend ist. Barrierefreie, nicht über schreibende Methoden Deklaration gefunden. Führen Sie dann den Typrückschluss und die Überladungs Auflösung für den Satz der anwendbaren, zugänglichen, nicht überschreibenden Methoden aus, die in diesem Typ deklariert sind, und rufen Sie die Methode auf Wenn keine Methode gefunden wurde, versuchen Sie stattdessen, den Aufruf als Erweiterungs Methodenaufruf zu verarbeiten.

#### <a name="extension-method-invocations"></a>Erweiterungs Methodenaufrufe

In einem Methodenaufruf ([Aufrufe für geboxte Instanzen](expressions.md#invocations-on-boxed-instances)) eines der Formulare
```csharp
expr . identifier ( )

expr . identifier ( args )

expr . identifier < typeargs > ( )

expr . identifier < typeargs > ( args )
```
Wenn bei der normalen Verarbeitung des aufzurufenden-aufzurufenden keine anwendbaren Methoden gefunden werden, wird versucht, das Konstrukt als Erweiterungs Methodenaufruf zu verarbeiten. Wenn *expr* oder eines der *args* `dynamic`vom Typ "Kompilierzeit" ist, werden keine Erweiterungs Methoden angewendet.

Ziel ist es, die beste *TYPE_NAME* `C`zu finden, damit der entsprechende Aufruf der statischen Methode stattfinden kann:
```csharp
C . identifier ( expr )

C . identifier ( expr , args )

C . identifier < typeargs > ( expr )

C . identifier < typeargs > ( expr , args )
```

Eine Erweiterungsmethode `Ci.Mj` ist ***berechtigt*** , wenn Folgendes gilt:

*  `Ci` ist eine nicht generische, nicht-nicht-Klassen-Klasse.
*  Der Name `Mj` ist ein *Bezeichner* .
*  `Mj` ist verfügbar und anwendbar, wenn Sie auf die Argumente als statische Methode angewendet wird, wie oben gezeigt.
*  Eine implizite Identitäts-, Verweis-oder Boxingkonvertierung ist von *expr* zum Typ des ersten Parameters `Mj`vorhanden.

Die Suche nach `C` verläuft wie folgt:

*  Beginnend mit der nächstgelegenen, einschließenden Namespace Deklaration, der Fortsetzung der einschließenden Namespace Deklaration und dem Ende der enthaltenden Kompilierungseinheit werden nachfolgende Versuche unternommen, um einen Kandidaten Satz von Erweiterungs Methoden zu finden:
   * Wenn der angegebene Namespace oder die Kompilierungseinheit direkt nicht generische Typdeklarationen `Ci` mit berechtigten Erweiterungs Methoden `Mj`enthält, ist der Satz dieser Erweiterungs Methoden der Kandidaten Satz.
   * Wenn `Ci` Typen von *using_static_declarations* importiert und direkt in Namespaces deklariert werden, die von *using_namespace_directive*s in der angegebenen Namespace-oder Kompilierungseinheit importiert werden, sind die entsprechenden Erweiterungs Methoden `Mj`enthalten, dann ist der Satz dieser Erweiterungs Methoden der Kandidaten Satz.
*  Wenn kein Kandidaten Satz in einer einschließenden Namespace Deklaration oder Kompilierungseinheit gefunden wird, tritt ein Kompilierzeitfehler auf.
*  Andernfalls wird die Überladungs Auflösung auf den Kandidaten Satz angewendet, wie in ([Überladungs Auflösung](expressions.md#overload-resolution)) beschrieben. Wenn keine einzige optimale Methode gefunden wird, tritt ein Kompilierzeitfehler auf.
*  `C` ist der Typ, in dem die beste Methode als Erweiterungsmethode deklariert wird.

Wenn Sie `C` als Ziel verwenden, wird der Methodenaufruf als statischer Methodenaufruf verarbeitet ([Kompilierzeit Überprüfung der dynamischen Überladungs Auflösung](expressions.md#compile-time-checking-of-dynamic-overload-resolution)).

Die vorstehenden Regeln bedeuten, dass Instanzmethoden Vorrang vor Erweiterungs Methoden haben, dass die in inneren Namespace Deklarationen verfügbaren Erweiterungs Methoden Vorrang vor Erweiterungs Methoden haben, die in äußeren Namespace Deklarationen verfügbar sind, und diese Erweiterung Methoden, die direkt in einem Namespace deklariert werden, haben Vorrang vor Erweiterungs Methoden, die in den gleichen Namespace mit einer using-namespace Direktive importiert werden. Beispiel:
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

Im Beispiel hat die-Methode `B`Vorrang vor der ersten Erweiterungsmethode, und `C`-Methode hat Vorrang vor beiden Erweiterungs Methoden.

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

Die Ausgabe dieses Beispiels lautet wie folgt:
```console
E.F(1)
D.G(2)
C.H(3)
```
`D.G` hat Vorrang vor `C.G`, und `E.F` hat Vorrang vor `D.F` und `C.F`.

#### <a name="delegate-invocations"></a>Delegataufrufe

Bei einem Delegataufruf muss die *primary_expression* der *invocation_expression* ein Wert eines *delegate_type*sein. Außerdem muss der *delegate_type* in Bezug auf den *argument_list* der *invocation_expression*anwendbar sein, wenn der *delegate_type* ein [Funktionsmember](expressions.md#applicable-function-member) mit derselben Parameterlistewie der *delegate_type*ist.

Die Lauf Zeit Verarbeitung eines delegataufruens in der Form `D(A)`, wobei `D` ein *primary_expression* einer *delegate_type* ist und `A` ein optionaler *argument_list*ist, besteht aus den folgenden Schritten:

*  `D` wird ausgewertet. Wenn diese Auswertung eine Ausnahme verursacht, werden keine weiteren Schritte ausgeführt.
*  Der Wert `D` wird als gültig geprüft. Wenn der Wert von `D` `null`ist, wird ein `System.NullReferenceException` ausgelöst, und es werden keine weiteren Schritte ausgeführt.
*  Andernfalls ist `D` ein Verweis auf eine Delegatinstanz. Funktionsmember-Aufrufe ([Kompilierungszeit Überprüfung der dynamischen Überladungs Auflösung](expressions.md#compile-time-checking-of-dynamic-overload-resolution)) werden für jede der Aufruf baren Entitäten in der Aufruf Liste des Delegaten ausgeführt. Bei Aufruf baren Entitäten, die aus einer Instanz-und Instanzmethode bestehen, ist die Instanz für den Aufruf die Instanz, die in der Aufruf baren Entität enthalten ist.

### <a name="element-access"></a>Element Zugriff

Eine *element_access* besteht aus einem *primary_no_array_creation_expression*, gefolgt von einem "`[`"-Token, gefolgt von einem *argument_list*, gefolgt von einem "`]`"-Token. Der *argument_list* besteht aus mindestens einem *Argument*s, getrennt durch Kommas.

```antlr
element_access
    : primary_no_array_creation_expression '[' expression_list ']'
    ;
```

Der *argument_list* eines *element_access* darf keine `ref` oder `out` Argumente enthalten.

Ein- *element_access* ist dynamisch gebunden ([dynamische Bindung](expressions.md#dynamic-binding)), wenn mindestens einer der folgenden Punkte enthält:

* Der *primary_no_array_creation_expression* hat den Typ `dynamic`der Kompilierzeit.
* Mindestens ein Ausdruck des *argument_list* hat den Typ "Kompilierzeit" `dynamic`, und der *primary_no_array_creation_expression* weist keinen Arraytyp auf.

In diesem Fall klassifiziert der Compiler die *element_access* als Wert des Typs `dynamic`. Die nachstehenden Regeln zum Bestimmen der Bedeutung der *element_access* werden dann zur Laufzeit angewendet. dabei wird der Lauf Zeittyp anstelle des Kompilier Zeittyps der *primary_no_array_creation_expression* und *argument_list* Ausdrücke verwendet, die den Typ `dynamic`der Kompilierzeit aufweisen. Wenn die *primary_no_array_creation_expression* keinen Kompilier Zeittyp `dynamic`hat, durchläuft der Element Zugriff eine begrenzte Kompilierungszeit Überprüfung, wie in der [Kompilierzeit Überprüfung der dynamischen Überladungs Auflösung](expressions.md#compile-time-checking-of-dynamic-overload-resolution)beschrieben.

Wenn die *primary_no_array_creation_expression* eines *element_access* ein Wert eines *array_type*ist, ist der *element_access* ein Array Zugriff ([Array Zugriff](expressions.md#array-access)). Andernfalls muss es sich bei der *primary_no_array_creation_expression* um eine Variable oder einen Wert einer Klasse, Struktur oder eines Schnittstellen Typs handeln, die mindestens ein Indexer-Element aufweist. in diesem Fall ist der *element_access* ein Indexer-Zugriff ([Indexer-Zugriff](expressions.md#indexer-access)).

#### <a name="array-access"></a>Arrayzugriff

Bei einem Array Zugriff muss die *primary_no_array_creation_expression* der *element_access* ein Wert eines *array_type*sein. Außerdem darf die *argument_list* eines Array Zugriffs keine benannten Argumente enthalten. Die Anzahl der Ausdrücke in der *argument_list* muss mit dem Rang des *array_type*identisch sein, und jeder Ausdruck muss den Typ `int`, `uint`, `long`, `ulong`oder implizit in einen oder mehrere dieser Typen konvertieren.

Das Ergebnis der Auswertung eines Array Zugriffs ist eine Variable des Elementtyps des Arrays, d. h. das Array Element, das von den Werten der Ausdrücke im *argument_list*ausgewählt wird.

Die Lauf Zeit Verarbeitung eines Array Zugriffs auf das Formular `P[A]`, wobei `P` eine *primary_no_array_creation_expression* einer *array_type* und *`A` argument_list ist, besteht*aus den folgenden Schritten:

*  `P` wird ausgewertet. Wenn diese Auswertung eine Ausnahme verursacht, werden keine weiteren Schritte ausgeführt.
*  Die Index Ausdrücke der *argument_list* werden in der Reihenfolge von links nach rechts ausgewertet. Nach der Auswertung der einzelnen Index Ausdrücke wird eine implizite Konvertierung ([implizite Konvertierungen](conversions.md#implicit-conversions)) in einen der folgenden Typen ausgeführt: `int`, `uint``long`, `ulong`. Der erste Typ in dieser Liste, für den eine implizite Konvertierung vorhanden ist, wird ausgewählt. Wenn der Index Ausdruck beispielsweise vom Typ `short` ist, wird eine implizite Konvertierung in `int` durchgeführt, da implizite Konvertierungen von `short` in `int` und von `short` zu `long` möglich sind. Wenn die Auswertung eines Index Ausdrucks oder der nachfolgenden impliziten Konvertierung eine Ausnahme verursacht, werden keine weiteren Index Ausdrücke ausgewertet, und es werden keine weiteren Schritte ausgeführt.
*  Der Wert `P` wird als gültig geprüft. Wenn der Wert von `P` `null`ist, wird ein `System.NullReferenceException` ausgelöst, und es werden keine weiteren Schritte ausgeführt.
*  Der Wert jedes Ausdrucks im *argument_list* wird anhand der tatsächlichen Begrenzungen der einzelnen Dimensionen der Array Instanz überprüft, auf die `P`verweist. Wenn mindestens ein Wert außerhalb des gültigen Bereichs liegt, wird eine `System.IndexOutOfRangeException` ausgelöst, und es werden keine weiteren Schritte ausgeführt.
*  Der Speicherort des Array Elements, das durch den Index Ausdruck (n) angegeben wird, wird berechnet, und dieser Speicherort wird zum Ergebnis des Array Zugriffs.

#### <a name="indexer-access"></a>Indexerzugriff

Für einen Indexer-Zugriff muss die *primary_no_array_creation_expression* der *element_access* eine Variable oder ein Wert einer Klasse, Struktur oder eines Schnittstellen Typs sein, und dieser Typ muss einen oder mehrere Indexer implementieren, die in Bezug auf die *argument_list* der *element_access*anwendbar sind.

Die Bindungs Zeit Verarbeitung eines Indexer-Zugriffs auf das Formular `P[A]`, wobei `P` eine *primary_no_array_creation_expression* eines Klassen-, Struktur-oder Schnittstellen Typs `T`ist, und *`A` argument_list ist, besteht*aus den folgenden Schritten:

*  Der von `T` bereitgestellte Indexer-Satz wird erstellt. Der Satz besteht aus allen indexermembern, die in `T` deklariert sind, oder einem Basistyp von `T`, die keine `override` Deklarationen sind und auf die im aktuellen Kontext ([Member Access) zugegriffen](basic-concepts.md#member-access)werden kann.
*  Der Satz wird auf die Indexer reduziert, die anwendbar sind und von anderen Indexer nicht ausgeblendet werden. Die folgenden Regeln werden auf jeden Indexer `S.I` im Satz angewendet, wobei `S` der Typ ist, in dem der Indexer `I` deklariert ist:
   * Wenn `I` in Bezug auf `A` ([anwendbares Funktionsmember](expressions.md#applicable-function-member)) nicht anwendbar ist, wird `I` aus dem Satz entfernt.
   * Wenn `I` in Bezug auf `A` ([anwendbares Funktionsmember](expressions.md#applicable-function-member)) anwendbar ist, werden alle Indexer, die in einem Basistyp von `S` deklariert sind, aus dem Satz entfernt.
   * Wenn `I` in Bezug auf `A` ([anwendbares Funktionsmember](expressions.md#applicable-function-member)) anwendbar ist und `S` ein anderer Klassentyp als `object`ist, werden alle in einer Schnittstelle deklarierten Indexer aus dem Satz entfernt.
*  Wenn der resultierende Satz von Kandidaten Indexer leer ist, sind keine anwendbaren Indexer vorhanden, und es tritt ein Bindungs Zeitfehler auf.
*  Der beste Indexer der Gruppe von Kandidaten Indexern wird mithilfe der Regeln zur Überladungs Auflösung der [Überladungs Auflösung](expressions.md#overload-resolution)identifiziert. Wenn ein einzelner optimaler Indexer nicht identifiziert werden kann, ist der Indexerzugriff mehrdeutig, und es tritt ein Fehler bei der Bindung auf.
*  Die Index Ausdrücke der *argument_list* werden in der Reihenfolge von links nach rechts ausgewertet. Das Ergebnis der Verarbeitung des Indexer-Zugriffs ist ein Ausdruck, der als Indexer-Zugriff klassifiziert ist. Der indexerzugriffsausdruck verweist auf den Indexer, der im obigen Schritt festgelegt wurde, und verfügt über einen zugeordneten Instanzausdruck `P` und eine zugeordnete Argumentliste `A`.

Abhängig vom Kontext, in dem Sie verwendet werden, bewirkt ein Indexer-Zugriff, dass entweder der *Get-Accessor* oder der *Set-Accessor* des Indexers aufgerufen wird. Wenn der Indexer-Zugriff das Ziel einer Zuweisung ist, wird der *Set-Accessor* aufgerufen, um einen neuen Wert zuzuweisen ([einfache Zuweisung](expressions.md#simple-assignment)). In allen anderen Fällen wird der *Get-Accessor* aufgerufen, um den aktuellen Wert zu erhalten ([Werte von Ausdrücken](expressions.md#values-of-expressions)).

### <a name="this-access"></a>Dieser Zugriff

Ein *this_access* besteht aus dem reservierten Wort `this`.

```antlr
this_access
    : 'this'
    ;
```

Eine *this_access* ist nur im- *Block* eines Instanzkonstruktors, einer Instanzmethode oder eines Instanzaccessors zulässig. Dies hat eine der folgenden Bedeutungen:

*  Wenn `this` in einem *primary_expression* in einem Instanzkonstruktor einer Klasse verwendet wird, wird es als Wert klassifiziert. Der Typ des Werts ist der Instanztyp ([der Instanztyp](classes.md#the-instance-type)) der Klasse, in der die Verwendung erfolgt, und der Wert ist ein Verweis auf das Objekt, das erstellt wird.
*  Wenn `this` in einer *primary_expression* in einer Instanzmethode oder einem Instanzaccessor einer Klasse verwendet wird, wird Sie als Wert klassifiziert. Der Typ des Werts ist der Instanztyp ([der Instanztyp](classes.md#the-instance-type)) der Klasse, in der die Verwendung erfolgt, und der Wert ist ein Verweis auf das Objekt, für das die Methode oder der Accessor aufgerufen wurde.
*  Wenn `this` in einem *primary_expression* innerhalb eines Instanzkonstruktors einer Struktur verwendet wird, wird es als Variable klassifiziert. Der Typ der Variablen ist der Instanztyp ([der Instanztyp](classes.md#the-instance-type)) der Struktur, in der die Verwendung erfolgt, und die Variable stellt die Struktur dar, die erstellt wird. Die `this` Variable eines Instanzkonstruktors einer Struktur verhält sich genau wie ein `out` Parameter des Struktur Typs – insbesondere bedeutet dies, dass die Variable definitiv in jedem Ausführungs Pfad des Instanzkonstruktors zugewiesen werden muss.
*  Wenn `this` in einer *primary_expression* in einer Instanzmethode oder einem Instanzaccessor einer Struktur verwendet wird, wird Sie als Variable klassifiziert. Der Typ der Variablen ist der Instanztyp ([der Instanztyp](classes.md#the-instance-type)) der Struktur, in der die Verwendung erfolgt.
   * Wenn die Methode oder der Accessor kein Iterator ([Iteratoren](classes.md#iterators)) ist, stellt die `this` Variable die Struktur dar, für die die Methode oder der Accessor aufgerufen wurde, und verhält sich genau wie ein `ref` Parameter des Struktur Typs.
   * Wenn es sich bei der Methode oder dem Accessor um einen Iterator handelt, stellt die `this` Variable eine Kopie der Struktur dar, für die die Methode oder der Accessor aufgerufen wurde, und verhält sich genau wie ein Wert Parameter des Struktur Typs.

Die Verwendung von `this` in einem *primary_expression* in einem anderen als dem oben aufgeführten Kontext ist ein Kompilierzeitfehler. Insbesondere ist es nicht möglich, auf `this` in einer statischen Methode, einem statischen Eigenschafts Accessor oder in einer *variable_initializer* einer Feld Deklaration zu verweisen.

### <a name="base-access"></a>Basis Zugriff

Ein *base_access* besteht aus dem reservierten Wort `base` gefolgt von einem "`.`"-Token und einem Bezeichner oder einem in eckige Klammern eingeschlossenen *argument_list* :

```antlr
base_access
    : 'base' '.' identifier
    | 'base' '[' expression_list ']'
    ;
```

Ein *base_access* wird für den Zugriff auf Basisklassenmember verwendet, die durch ähnlich benannte Member in der aktuellen Klasse oder Struktur ausgeblendet werden. Eine *base_access* ist nur im- *Block* eines Instanzkonstruktors, einer Instanzmethode oder eines Instanzaccessors zulässig. Wenn `base.I` in einer Klasse oder Struktur auftritt, müssen `I` einen Member der Basisklasse dieser Klasse oder Struktur angeben. Ebenso muss ein anwendbarer Indexer in der Basisklasse vorhanden sein, wenn `base[E]` in einer Klasse auftritt.

Bei der Bindungs Zeit werden *base_access* Ausdrücke der Form `base.I` und `base[E]` genau so ausgewertet, als wären Sie `((B)this).I` und `((B)this)[E]`geschrieben worden, wobei `B` die Basisklasse der Klasse oder Struktur ist, in der das Konstrukt auftritt. Folglich entsprechen `base.I` und `base[E]` `this.I` und `this[E]`, mit dem Unterschied, dass `this` als Instanz der Basisklasse angezeigt wird.

Wenn eine *base_access* auf einen virtuellen Funktionsmember (eine Methode, eine Eigenschaft oder einen Indexer) verweist, wird bestimmt, welcher Funktionsmember zur Laufzeit aufgerufen werden soll (über[Prüfung der Auflösung der dynamischen Überlastung](expressions.md#compile-time-checking-of-dynamic-overload-resolution)). Der aufgerufene Funktionsmember wird durch die Suche nach der am weitesten abgeleiteten Implementierung ([virtuelle Methoden](classes.md#virtual-methods)) des Funktionsmembers in Bezug auf `B` bestimmt (und nicht in Bezug auf den Lauf Zeittyp `this`, wie es bei einem nicht-Basis Zugriff üblich wäre). Daher kann in einer `override` eines `virtual` Funktionsmembers eine *base_access* verwendet werden, um die geerbte Implementierung des Funktionsmembers aufzurufen. Wenn das von einem *base_access* referenzierte Funktionsmember abstrakt ist, tritt ein Fehler bei der Bindung auf.

### <a name="postfix-increment-and-decrement-operators"></a>Postfix-Inkrementoperator und Postfix-Dekrementoperator

```antlr
post_increment_expression
    : primary_expression '++'
    ;

post_decrement_expression
    : primary_expression '--'
    ;
```

Der Operand eines Postfix-Inkrement-oder dekrementvorgangs muss ein Ausdruck sein, der als Variable, Eigenschafts Zugriff oder Indexerzugriff klassifiziert ist. Das Ergebnis des Vorgangs ist ein Wert desselben Typs wie der Operand.

Wenn die *primary_expression* den Kompilier Zeittyp aufweist `dynamic` dann wird der Operator dynamisch gebunden ([dynamische Bindung](expressions.md#dynamic-binding)), der *post_increment_expression* oder *post_decrement_expression* hat den Kompilier Zeittyp `dynamic`, und die folgenden Regeln werden zur Laufzeit mithilfe des Lauf Zeit Typs der *primary_expression*angewendet.

Wenn der Operand einer Postfix-Inkrement-oder Dekrementoperation eine Eigenschaft oder ein Indexer-Zugriff ist, muss die Eigenschaft oder der Indexer sowohl einen `get` als auch einen `set` Accessor aufweisen. Wenn dies nicht der Fall ist, tritt ein Bindungs Zeitfehler auf.

Unäre Operator Überladungs Auflösung ([unäre Operator Überladungs Auflösung](expressions.md#unary-operator-overload-resolution)) wird angewendet, um eine bestimmte Operator Implementierung auszuwählen. Vordefinierte `++` und `--` Operatoren sind für die folgenden Typen vorhanden: `sbyte`, `byte`, `short`, `ushort`, `int`, `uint`, `long`, `ulong`, `char`, `float`, `double`, `decimal`und beliebigen Enumerationstyp. Der vordefinierte `++`-Operatoren gibt den Wert zurück, der beim Hinzufügen von 1 zum Operanden erzeugt wurde, und die vordefinierten `--` Operatoren geben den Wert zurück, der durch die Subtraktion von 1 vom Operanden erzeugt Wenn in einem `checked` Kontext das Ergebnis dieser Addition oder Subtraktion außerhalb des Bereichs des Ergebnis Typs liegt und der Ergebnistyp ein ganzzahliger Typ oder Enumeration-Typ ist, wird eine `System.OverflowException` ausgelöst.

Die Lauf Zeit Verarbeitung eines Postfix-Inkrement-oder dekrementvorgangs in der Form `x++` oder `x--` besteht aus den folgenden Schritten:

*   Wenn `x` als Variable klassifiziert ist:
    * `x` wird ausgewertet, um die Variable zu entwickeln.
    * Der Wert `x` wird gespeichert.
    * Der ausgewählte Operator wird mit dem gespeicherten Wert `x` als Argument aufgerufen.
    * Der vom Operator zurückgegebene Wert wird an dem Speicherort gespeichert, der durch die Auswertung der `x`angegeben wird.
    * Der gespeicherte Wert von `x` wird das Ergebnis des Vorgangs.
*   Wenn `x` als Eigenschaft oder Indexer-Zugriff klassifiziert ist:
    * Der Instanzausdruck (wenn `x` nicht `static`ist) und die Argumentliste (wenn `x` ein Indexer-Zugriff ist), der `x` zugeordnet ist, werden ausgewertet, und die Ergebnisse werden in den nachfolgenden `get`-und `set` Accessoraufrufen verwendet.
    * Der `get` Accessor von `x` wird aufgerufen, und der zurückgegebene Wert wird gespeichert.
    * Der ausgewählte Operator wird mit dem gespeicherten Wert `x` als Argument aufgerufen.
    * Der `set` Accessor von `x` wird mit dem Wert aufgerufen, der vom Operator als `value` Argument zurückgegeben wurde.
    * Der gespeicherte Wert von `x` wird das Ergebnis des Vorgangs.

Die Operatoren "`++`" und "`--`" unterstützen auch Präfix Notation ([Präfix Inkrement-und Dekrementoperator](expressions.md#prefix-increment-and-decrement-operators) In der Regel ist das Ergebnis von `x++` oder `x--` der Wert von `x` vor dem Vorgang, wohingegen das Ergebnis von `++x` oder `--x` der Wert von `x` nach dem Vorgang ist. In beiden Fällen hat `x` selbst nach dem Vorgang denselben Wert.

Eine `operator ++`-oder `operator --`-Implementierung kann entweder mithilfe der Postfix-oder Präfix Notation aufgerufen werden. Es ist nicht möglich, separate Operator Implementierungen für die beiden Notationen zu haben.

### <a name="the-new-operator"></a>Der new-Operator

Der `new`-Operator wird verwendet, um neue Instanzen von-Typen zu erstellen.

Es gibt drei Formen von `new` Ausdrücken:

*  Objekt Erstellungs Ausdrücke werden verwendet, um neue Instanzen von Klassentypen und Werttypen zu erstellen.
*  Array Erstellungs Ausdrücke werden verwendet, um neue Instanzen von Array Typen zu erstellen.
*  Delegaterstellungs-Ausdrücke werden verwendet, um neue Instanzen von Delegattypen zu erstellen.

Der `new`-Operator impliziert die Erstellung einer Instanz eines Typs, impliziert jedoch nicht notwendigerweise die dynamische Speicher Belegung. Vor allem sind für Instanzen von Werttypen außerhalb der Variablen, in denen Sie sich befinden, kein zusätzlicher Arbeitsspeicher erforderlich. wenn `new` zum Erstellen von Instanzen von Werttypen verwendet wird, erfolgt keine dynamische Zuordnung.

#### <a name="object-creation-expressions"></a>Objekt Erstellungs Ausdrücke

Eine *object_creation_expression* wird zum Erstellen einer neuen Instanz einer *class_type* oder einer *value_type*verwendet.

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

Der *Typ* eines *object_creation_expression* muss ein *class_type*, ein *value_type* oder ein *type_parameter*sein. Der *Typ* darf keine `abstract` *class_type*sein.

Die optionalen *argument_list* ([Argumentlisten](expressions.md#argument-lists)) sind nur zulässig, wenn es sich bei dem *Typ* um einen *class_type* oder eine *struct_type*handelt.

Ein Objekt Erstellungs Ausdruck kann die Liste der Konstruktorargumente weglassen und einschließende Klammern einschließen, wenn er einen Objektinitialisierer oder einen Auflistungsinitialisierer enthält. Das Weglassen der Konstruktorargumentliste und das Einschließen von Klammern entspricht der Angabe einer leeren Argumentliste.

Die Verarbeitung eines Ausdrucks zur Objekt Erstellung, der einen Objektinitialisierer oder Auflistungsinitialisierer enthält, besteht aus der ersten Verarbeitung des Instanzkonstruktors und der anschließenden Verarbeitung der Element-oder Element Initialisierungen, die vom Objektinitialisierer ([Objektinitialisierer](expressions.md#object-initializers)) oder sammlungsinitialisierer ([sammlungsinitialisierer](expressions.md#collection-initializers)

Wenn eines der Argumente im optionalen *argument_list* den Kompilier Zeittyp aufweist `dynamic` dann wird der *object_creation_expression* dynamisch gebunden ([dynamische Bindung](expressions.md#dynamic-binding)), und die folgenden Regeln werden zur Laufzeit verwendet, wobei der Lauf Zeittyp der Argumente der *argument_list* verwendet wird, die den Kompilier Zeittyp `dynamic`haben. Allerdings wird bei der Objekt Erstellung eine beschränkte Kompilierungszeit Überprüfung durchgeführt, wie in der [Kompilierzeit Überprüfung der dynamischen Überladungs Auflösung](expressions.md#compile-time-checking-of-dynamic-overload-resolution)beschrieben.

Die Bindungs Zeit Verarbeitung einer *object_creation_expression* der Form `new T(A)`, bei der `T` ein *class_type* oder ein *value_type* ist, und `A` ein optionaler *argument_list*ist, besteht aus den folgenden Schritten:

*   Wenn `T` ein *value_type* ist und `A` nicht vorhanden ist:
    * Der *object_creation_expression* ist ein standardkonstruktoraufruf. Das Ergebnis der *object_creation_expression* ist ein Wert vom Typ `T`, d. h. der Standardwert für `T`, wie im [System. ValueType-Typ](types.md#the-systemvaluetype-type)definiert.
*   Andernfalls, wenn `T` ein *type_parameter* ist und `A` nicht vorhanden ist:
    * Wenn keine Werttyp Einschränkung oder Konstruktoreinschränkung ([Typparameter Einschränkungen](classes.md#type-parameter-constraints)) für `T`angegeben wurde, tritt ein Bindungs Zeitfehler auf.
    * Das Ergebnis der *object_creation_expression* ist ein Wert des Lauf Zeittyps, an den der Typparameter gebunden wurde, nämlich das Ergebnis des Aufrufs des Standardkonstruktors dieses Typs. Der Lauf Zeittyp kann ein Referenztyp oder ein Werttyp sein.
*   Andernfalls, wenn `T` ein *class_type* oder ein *struct_type*ist:
    * Wenn `T` eine `abstract` *class_type*ist, tritt ein Kompilierzeitfehler auf.
    * Der aufzurufende Instanzkonstruktor wird mithilfe der Regeln zur Überladungs Auflösung der [Überladungs Auflösung](expressions.md#overload-resolution)bestimmt. Der Satz von Kandidaten Instanzkonstruktoren besteht aus allen zugänglichen Instanzkonstruktoren, die in `T` deklariert sind und in Bezug auf `A` ([anwendbares Funktionsmember](expressions.md#applicable-function-member)) anwendbar sind. Wenn der Satz von standardinstanzkonstruktoren leer ist, oder wenn ein einzelner Konstruktor mit der besten Instanz nicht identifiziert werden kann, tritt ein Bindungs Zeitfehler auf.
    * Das Ergebnis der *object_creation_expression* ist ein Wert vom Typ `T`, d. h. der Wert, der durch Aufrufen des Instanzkonstruktors erzeugt wird, der im obigen Schritt festgelegt wurde.
*  Andernfalls ist der *object_creation_expression* ungültig, und ein Bindungs Fehler tritt auf.

Auch wenn die *object_creation_expression* dynamisch gebunden ist, ist der Kompilier Zeittyp weiterhin `T`.

Die Lauf Zeit Verarbeitung einer *object_creation_expression* der Form `new T(A)`, bei der `T` *class_type* ist, oder ein *struct_type* und `A` ein optionaler *argument_list*ist, besteht aus den folgenden Schritten:

*   Wenn `T` ein *class_type*ist:
    * Es wird eine neue Instanz der-Klasse `T` zugeordnet. Wenn nicht genügend Arbeitsspeicher zum Zuordnen der neuen Instanz verfügbar ist, wird ein `System.OutOfMemoryException` ausgelöst, und es werden keine weiteren Schritte ausgeführt.
    * Alle Felder der neuen Instanz werden mit ihren Standardwerten initialisiert ([Standardwerte](variables.md#default-values)).
    * Der Instanzkonstruktor wird entsprechend den Regeln für den Funktionselement Aufruf ([Kompilierzeit Überprüfung der dynamischen Überladungs Auflösung](expressions.md#compile-time-checking-of-dynamic-overload-resolution)) aufgerufen. Ein Verweis auf die neu zugeordnete Instanz wird automatisch an den Instanzkonstruktor übergeben, und die Instanz kann innerhalb dieses Konstruktors als `this`aufgerufen werden.
*   Wenn `T` ein *struct_type*ist:
    * Eine Instanz vom Typ `T` wird erstellt, indem eine temporäre lokale Variable zugewiesen wird. Da ein Instanzkonstruktor einer *struct_type* erforderlich ist, um jedem Feld der erstellten Instanz einen Wert definitiv zuzuweisen, ist keine Initialisierung der temporären Variablen erforderlich.
    * Der Instanzkonstruktor wird entsprechend den Regeln für den Funktionselement Aufruf ([Kompilierzeit Überprüfung der dynamischen Überladungs Auflösung](expressions.md#compile-time-checking-of-dynamic-overload-resolution)) aufgerufen. Ein Verweis auf die neu zugeordnete Instanz wird automatisch an den Instanzkonstruktor übergeben, und die Instanz kann innerhalb dieses Konstruktors als `this`aufgerufen werden.

#### <a name="object-initializers"></a>Objektinitialisierer

Ein ***Objektinitialisierer*** gibt Werte für NULL oder mehr Felder, Eigenschaften oder indizierte Elemente eines Objekts an.

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

Ein Objektinitialisierer besteht aus einer Sequenz von Elementinitialisierern, die durch `{`-und `}`-Token eingeschlossen und durch Kommas getrennt sind. Jede *member_initializer* bestimmt ein Ziel für die Initialisierung. Ein *Bezeichner* muss ein barrierefreies Feld oder eine Eigenschaft des Objekts benennen, das initialisiert wird, wohingegen eine in eckige Klammern eingeschlossene *argument_list* Argumente für einen zugreif baren Indexer für das Objekt angeben muss, das initialisiert wird. Es ist ein Fehler, wenn ein Objektinitialisierer mehr als einen Elementinitialisierer für das gleiche Feld oder die gleiche Eigenschaft einschließt.

Auf jede *initializer_target* folgt ein Gleichheitszeichen und entweder ein Ausdruck, ein Objektinitialisierer oder ein Auflistungsinitialisierer. Es ist nicht möglich, dass Ausdrücke im Objektinitialisierer auf das neu erstellte Objekt verweisen, das initialisiert wird.

Ein Member-Initialisierer, der einen Ausdruck angibt, nachdem das Gleichheitszeichen auf die gleiche Weise wie eine Zuweisung ([einfache Zuweisung](expressions.md#simple-assignment)) zum Ziel verarbeitet wird.

Ein Member-Initialisierer, der einen Objektinitialisierer angibt, nachdem dasGleichheitszeichen ein instanzinitialisierer ist, d. h. eine Initialisierung eines eingebetteten Objekts. Anstatt dem Feld oder der Eigenschaft einen neuen Wert zuzuweisen, werden die Zuweisungen im Initialisierer für den initialisierten Objektinitialisierer als Zuweisungen zu Membern des Felds oder der Eigenschaft behandelt. Initialisierer für ein Objekt können nicht auf Eigenschaften mit einem Werttyp oder auf schreibgeschützte Felder mit einem Werttyp angewendet werden.

Ein Member-Initialisierer, der einen Auflistungsinitialisierer angibt, nachdem das Gleichheitszeichen eine Initialisierung einer eingebetteten Auflistung ist. Anstatt dem Zielfeld, der Eigenschaft oder dem Indexer eine neue Auflistung zuzuweisen, werden die im Initialisierer angegebenen Elemente der Auflistung hinzugefügt, auf die das Ziel verweist. Das Ziel muss einen Sammlungstyp aufweisen, der die in [Auflistungsinitialisierer](expressions.md#collection-initializers) angegebenen Anforderungen erfüllt.

Die Argumente für einen indexinitialisierer werden immer genau einmal ausgewertet. Folglich werden Sie, selbst wenn die Argumente am Ende nie verwendet werden (z. b. aufgrund eines leeren Initialisierers), auf Ihre Nebeneffekte ausgewertet.

Die folgende Klasse stellt einen Punkt mit zwei Koordinaten dar:
```csharp
public class Point
{
    int x, y;

    public int X { get { return x; } set { x = value; } }
    public int Y { get { return y; } set { y = value; } }
}
```

Eine Instanz von `Point` kann wie folgt erstellt und initialisiert werden:
```csharp
Point a = new Point { X = 0, Y = 1 };
```
Dies hat die gleiche Wirkung wie
```csharp
Point __a = new Point();
__a.X = 0;
__a.Y = 1; 
Point a = __a;
```
, wenn `__a` eine anderweitig unsichtbare und nicht zugängliche temporäre Variable ist. Die folgende Klasse stellt ein Rechteck dar, das aus zwei Punkten erstellt wurde:
```csharp
public class Rectangle
{
    Point p1, p2;

    public Point P1 { get { return p1; } set { p1 = value; } }
    public Point P2 { get { return p2; } set { p2 = value; } }
}
```

Eine Instanz von `Rectangle` kann wie folgt erstellt und initialisiert werden:
```csharp
Rectangle r = new Rectangle {
    P1 = new Point { X = 0, Y = 1 },
    P2 = new Point { X = 2, Y = 3 }
};
```
Dies hat die gleiche Wirkung wie
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
bei `__r`sind `__p1` und `__p2` temporäre Variablen, die andernfalls unsichtbar und nicht zugänglich sind.

Wenn der Konstruktor von `Rectangle`die beiden eingebetteten `Point` Instanzen ordnet
```csharp
public class Rectangle
{
    Point p1 = new Point();
    Point p2 = new Point();

    public Point P1 { get { return p1; } }
    public Point P2 { get { return p2; } }
}
```
Das folgende Konstrukt kann verwendet werden, um die eingebetteten `Point` Instanzen zu initialisieren, anstatt neue Instanzen zuzuweisen:
```csharp
Rectangle r = new Rectangle {
    P1 = { X = 0, Y = 1 },
    P2 = { X = 2, Y = 3 }
};
```
Dies hat die gleiche Wirkung wie
```csharp
Rectangle __r = new Rectangle();
__r.P1.X = 0;
__r.P1.Y = 1;
__r.P2.X = 2;
__r.P2.Y = 3;
Rectangle r = __r;
```

Im folgenden Beispiel wird eine entsprechende Definition von C angegeben:
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
entspricht dieser Reihe von Zuweisungen:
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
Dabei sind `__c`, usw., generierte Variablen, die unsichtbar sind und für den Quellcode nicht zugänglich sind. Beachten Sie, dass die Argumente für `[0,0]` nur einmal ausgewertet werden, und die Argumente für `[1,2]` werden einmal ausgewertet, auch wenn Sie nie verwendet werden.

#### <a name="collection-initializers"></a>Auflistungsinitialisierer

Ein Auflistungsinitialisierer gibt die Elemente einer Auflistung an.

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

Ein Auflistungsinitialisierer besteht aus einer Sequenz von Elementinitialisierern, die durch `{`-und `}`-Token eingeschlossen und durch Kommas getrennt sind. Jeder Elementinitialisierer gibt ein Element an, das dem Auflistungs Objekt hinzugefügt werden soll, das initialisiert wird, und besteht aus einer Liste von Ausdrücken, die durch `{`-und `}` Token eingeschlossen und durch Kommas getrennt sind.  Ein Elementinitialisierer mit einem Ausdruck kann ohne geschweifte Klammern geschrieben werden, kann jedoch kein Zuweisungs Ausdruck sein, um Mehrdeutigkeit mit Member-Initialisierern zu vermeiden. Die *non_assignment_expression* Produktion ist in [Expression](expressions.md#expression)definiert.

Im folgenden finden Sie ein Beispiel für einen Objekt Erstellungs Ausdruck, der einen Auflistungsinitialisierer umfasst:
```csharp
List<int> digits = new List<int> { 0, 1, 2, 3, 4, 5, 6, 7, 8, 9 };
```

Das Auflistungs Objekt, auf das ein Auflistungsinitialisierer angewendet wird, muss einen Typ aufweisen, der `System.Collections.IEnumerable` implementiert, oder ein Kompilierzeitfehler tritt auf. Für jedes angegebene Element in der Reihenfolge ruft der Auflistungsinitialisierer eine `Add` Methode für das Zielobjekt mit der Ausdrucks Liste des elementinitialisierers als Argumentliste auf und wendet normale Member-Suche-und Überladungs Auflösung für jeden Aufruf an. Folglich muss das Auflistungs Objekt über eine gültige-Instanz oder eine Erweiterungsmethode mit dem Namen `Add` für jeden Elementinitialisierer verfügen.

Die folgende Klasse stellt einen Kontakt mit einem Namen und einer Liste von Telefonnummern dar:
```csharp
public class Contact
{
    string name;
    List<string> phoneNumbers = new List<string>();

    public string Name { get { return name; } set { name = value; } }

    public List<string> PhoneNumbers { get { return phoneNumbers; } }
}
```

Eine `List<Contact>` kann wie folgt erstellt und initialisiert werden:
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
Dies hat die gleiche Wirkung wie
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
bei `__clist`sind `__c1` und `__c2` temporäre Variablen, die andernfalls unsichtbar und nicht zugänglich sind.

#### <a name="array-creation-expressions"></a>Ausdrücke zum Erstellen von Arrays

Eine *array_creation_expression* wird zum Erstellen einer neuen Instanz eines- *array_type*verwendet.

```antlr
array_creation_expression
    : 'new' non_array_type '[' expression_list ']' rank_specifier* array_initializer?
    | 'new' array_type array_initializer
    | 'new' rank_specifier array_initializer
    ;
```

Ein Array Erstellungs Ausdruck des ersten Formulars ordnet eine Array Instanz des Typs zu, der sich aus dem Löschen der einzelnen Ausdrücke aus der Ausdrucks Liste ergibt. Beispielsweise erzeugt der Ausdruck für die Array Erstellung `new int[10,20]` eine Array Instanz vom Typ `int[,]`, und der Array Erstellungs Ausdruck `new int[10][,]` ein Array vom Typ `int[][,]`erzeugt. Jeder Ausdruck in der Ausdrucks Liste muss den Typ "`int`", "`uint`", "`long`" oder "`ulong`" aufweisen oder implizit in einen oder mehrere dieser Typen konvertiert werden. Der Wert jedes Ausdrucks bestimmt die Länge der entsprechenden Dimension in der neu zugeordneten Array Instanz. Da die Länge einer Array Dimension nicht negativ sein muss, ist dies ein Kompilierzeitfehler, bei dem ein *constant_expression* mit einem negativen Wert in der Ausdrucks Liste vorliegt.

Das Layout von Arrays ist nicht festgelegt, es sei denn, es ist ein unsicherer Kontext ([unsichere Kontexte](unsafe-code.md#unsafe-contexts)).

Wenn ein Array Erstellungs Ausdruck des ersten Formulars einen Arrayinitialisierer enthält, muss jeder Ausdruck in der Ausdrucks Liste eine Konstante sein, und die von der Ausdrucks Liste angegebenen Rang-und Dimensions Längen müssen mit denen des Arrayinitialisierers identisch sein.

In einem Array Erstellungs Ausdruck des zweiten oder dritten Formulars muss der Rang des angegebenen Arraytyps oder rangspezifizierers mit dem des Arrayinitialisierers identisch sein. Die einzelnen Dimensions Längen werden von der Anzahl von Elementen in jeder der entsprechenden Schachtelungs Ebenen des Arrayinitialisierers abgeleitet. Folglich ist der Ausdruck
```csharp
new int[,] {{0, 1}, {2, 3}, {4, 5}}
```
entspricht genau
```csharp
new int[3, 2] {{0, 1}, {2, 3}, {4, 5}}
```

Ein Array Erstellungs Ausdruck des dritten Formulars wird als ***implizit typisierter Array Erstellungs Ausdruck***bezeichnet. Sie ähnelt dem zweiten Formular, mit dem Unterschied, dass der Elementtyp des Arrays nicht explizit angegeben wird, sondern als der am besten geeignete Typ (suchen[des besten allgemeinen Typs eines Satzes von Ausdrücken](expressions.md#finding-the-best-common-type-of-a-set-of-expressions)) des Satzes von Ausdrücken im Arrayinitialisierer festgelegt ist. Bei einem mehrdimensionalen Array, d. h. einer, bei dem der *rank_specifier* mindestens ein Komma enthält, besteht dieser Satz aus allen *Ausdrücken*, die in den *array_initializer*s gefunden wurden.

Arrayinitialisierer werden in [arrayinitialisierern](arrays.md#array-initializers)ausführlicher beschrieben.

Das Ergebnis der Auswertung eines Array Erstellungs Ausdrucks wird als Wert klassifiziert, nämlich als Verweis auf die neu zugeordnete Array Instanz. Die Lauf Zeit Verarbeitung eines Ausdrucks zur Array Erstellung besteht aus den folgenden Schritten:

*  Die Dimensions Längen Ausdrücke der *expression_list* werden in der Reihenfolge von links nach rechts ausgewertet. Nach der Auswertung der einzelnen Ausdrücke wird eine implizite Konvertierung ([implizite Konvertierungen](conversions.md#implicit-conversions)) in einen der folgenden Typen ausgeführt: `int`, `uint``long`, `ulong`. Der erste Typ in dieser Liste, für den eine implizite Konvertierung vorhanden ist, wird ausgewählt. Wenn die Auswertung eines Ausdrucks oder der nachfolgenden impliziten Konvertierung eine Ausnahme verursacht, werden keine weiteren Ausdrücke ausgewertet, und es werden keine weiteren Schritte ausgeführt.
*  Die berechneten Werte für die Dimensions Längen werden wie folgt überprüft. Wenn mindestens einer der Werte kleiner als 0 (null) ist, wird ein `System.OverflowException` ausgelöst, und es werden keine weiteren Schritte ausgeführt.
*  Eine Array Instanz mit den angegebenen Dimensions Längen ist zugeordnet. Wenn nicht genügend Arbeitsspeicher zum Zuordnen der neuen Instanz verfügbar ist, wird ein `System.OutOfMemoryException` ausgelöst, und es werden keine weiteren Schritte ausgeführt.
*  Alle Elemente der neuen Array Instanz werden mit ihren Standardwerten initialisiert ([Standardwerte](variables.md#default-values)).
*  Wenn der Ausdruck für die Array Erstellung einen Arrayinitialisierer enthält, wird jeder Ausdruck im Arrayinitialisierer ausgewertet und dem entsprechenden Array Element zugewiesen. Die Auswertungen und Zuweisungen werden in der Reihenfolge ausgeführt, in der die Ausdrücke in den Arrayinitialisierer geschrieben werden – mit anderen Worten, Elemente werden in steigender Index Reihenfolge initialisiert, wobei die äußerste Rechte Dimension zuerst zunimmt. Wenn die Auswertung eines angegebenen Ausdrucks oder der nachfolgenden Zuweisung zum entsprechenden Array Element eine Ausnahme auslöst, werden keine weiteren Elemente initialisiert (und die restlichen Elemente haben daher ihre Standardwerte).

Ein Array Erstellungs Ausdruck ermöglicht die Instanziierung eines Arrays mit Elementen eines Arraytyps, aber die Elemente eines solchen Arrays müssen manuell initialisiert werden. Beispielsweise ist die-Anweisung
```csharp
int[][] a = new int[100][];
```
erstellt ein eindimensionales Array mit 100 Elementen vom Typ `int[]`. Der Anfangswert jedes Elements ist `null`. Es ist nicht möglich, dass derselbe Array Erstellungs Ausdruck auch die Unterarrays und die-Anweisung instanziiert.
```csharp
int[][] a = new int[100][5];        // Error
```
führt zu einem Kompilierzeitfehler. Die Instanziierung der Unterarrays muss stattdessen manuell ausgeführt werden, wie in
```csharp
int[][] a = new int[100][];
for (int i = 0; i < 100; i++) a[i] = new int[5];
```

Wenn ein Array von Arrays eine "rechteckige" Form hat, d. h., wenn die Teil Arrays die gleiche Länge haben, ist es effizienter, ein mehrdimensionales Array zu verwenden. Im obigen Beispiel erstellt die Instanziierung des Arrays von Arrays 101 Objekte – ein äußeres Array und 100 Teil Arrays. Im Gegensatz dazu
```csharp
int[,] = new int[100, 5];
```
erstellt nur ein einzelnes-Objekt, ein zweidimensionales Array und erreicht die Zuordnung in einer einzelnen Anweisung.

Im folgenden finden Sie Beispiele für implizit typisierte Array Erstellungs Ausdrücke:
```csharp
var a = new[] { 1, 10, 100, 1000 };                       // int[]

var b = new[] { 1, 1.5, 2, 2.5 };                         // double[]

var c = new[,] { { "hello", null }, { "world", "!" } };   // string[,]

var d = new[] { 1, "one", 2, "two" };                     // Error
```

Der letzte Ausdruck verursacht einen Kompilierzeitfehler, weil weder `int` noch `string` implizit in den anderen konvertiert werden kann, sodass es keinen optimalen allgemeinen Typ gibt. In diesem Fall muss ein explizit typisierter Array Erstellungs Ausdruck verwendet werden, z. b. die Angabe des Typs, der `object[]`werden soll. Alternativ kann eines der Elemente in einen allgemeinen Basistyp umgewandelt werden, der dann zum abzurufenden Elementtyp wird.

Implizit typisierte Array Erstellungs Ausdrücke können mit anonymen Objektinitialisierern ([anonymen Objekt Erstellungs Ausdrücken](expressions.md#anonymous-object-creation-expressions)) kombiniert werden, um anonym typisierte Datenstrukturen zu erstellen. Beispiel:
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

#### <a name="delegate-creation-expressions"></a>Delegatenerstellungs Ausdrücke

Eine *delegate_creation_expression* wird zum Erstellen einer neuen Instanz einer *delegate_type*verwendet.

```antlr
delegate_creation_expression
    : 'new' delegate_type '(' expression ')'
    ;
```

Das Argument eines delegaterstellungs-Ausdrucks muss eine Methoden Gruppe, eine anonyme Funktion oder ein Wert entweder der Kompilier Zeittyp `dynamic` oder eine *delegate_type*sein. Wenn das Argument eine Methoden Gruppe ist, identifiziert es die-Methode und für eine Instanzmethode das-Objekt, für das ein Delegat erstellt werden soll. Wenn das Argument eine anonyme Funktion ist, werden die Parameter und der Methoden Text des delegatziels direkt definiert. Wenn das-Argument ein Wert ist, wird eine Delegatinstanz identifiziert, von der eine Kopie erstellt werden soll.

Wenn der *Ausdruck* den Kompilier Zeittyp `dynamic`aufweist, wird der *delegate_creation_expression* dynamisch gebunden ([dynamische Bindung](expressions.md#dynamic-binding)), und die unten aufgeführten Regeln werden zur Laufzeit mithilfe des Lauf Zeittyps des *Ausdrucks*angewendet. Andernfalls werden die Regeln zur Kompilierzeit angewendet.

Die Bindungs Zeit Verarbeitung einer *delegate_creation_expression* der Form `new D(E)`, bei der `D` ein *delegate_type* und `E` ein *Ausdruck*ist, besteht aus den folgenden Schritten:

*  Wenn `E` eine Methoden Gruppe ist, wird der delegaterstellungs-Ausdruck auf die gleiche Weise wie eine Methoden Gruppen Konvertierung ([Methoden Gruppen Konvertierungen](conversions.md#method-group-conversions)) von `E` in `D`verarbeitet.
*  Wenn `E` eine anonyme Funktion ist, wird der delegaterstellungs-Ausdruck auf die gleiche Weise wie eine anonyme Funktions Konvertierung ([Anonyme Funktions Konvertierungen](conversions.md#anonymous-function-conversions)) von `E` zu `D`verarbeitet.
*  Wenn `E` ein Wert ist, muss `E` kompatibel sein ([Delegatdeklarationen](delegates.md#delegate-declarations)) mit `D`, und das Ergebnis ist ein Verweis auf einen neu erstellten Delegaten vom Typ `D`, der auf dieselbe Aufruf Liste wie `E`verweist. Wenn `E` nicht mit `D`kompatibel ist, tritt ein Kompilierzeitfehler auf.

Die Lauf Zeit Verarbeitung einer *delegate_creation_expression* der Form `new D(E)`, bei der `D` ein *delegate_type* und `E` ein *Ausdruck*ist, besteht aus den folgenden Schritten:

*   Wenn `E` eine Methoden Gruppe ist, wird der delegaterstellungs-Ausdruck als Methoden Gruppen Konvertierung ([Methoden Gruppen Konvertierungen](conversions.md#method-group-conversions)) von `E` zu `D`ausgewertet.
*   Wenn `E` eine anonyme Funktion ist, wird die Delegaterstellung als anonyme Funktions Konvertierung von `E` in `D` ([Anonyme Funktions Konvertierungen](conversions.md#anonymous-function-conversions)) ausgewertet.
*   Wenn `E` ein *delegate_type*Wert ist:
    * `E` wird ausgewertet. Wenn diese Auswertung eine Ausnahme verursacht, werden keine weiteren Schritte ausgeführt.
    * Wenn der Wert von `E` `null`ist, wird ein `System.NullReferenceException` ausgelöst, und es werden keine weiteren Schritte ausgeführt.
    * Eine neue Instanz des Delegattyps `D` zugeordnet wird. Wenn nicht genügend Arbeitsspeicher zum Zuordnen der neuen Instanz verfügbar ist, wird ein `System.OutOfMemoryException` ausgelöst, und es werden keine weiteren Schritte ausgeführt.
    * Die neue Delegatinstanz wird mit der gleichen Aufruf Liste initialisiert wie die Delegatinstanz, die von `E`angegeben wird.

Die Aufruf Liste eines Delegaten wird bestimmt, wenn der Delegat instanziiert wird, und wird dann für die gesamte Lebensdauer des Delegaten konstant bleiben. Anders ausgedrückt: Es ist nicht möglich, die Ziel Aufruf baren Entitäten eines Delegaten zu ändern, nachdem er erstellt wurde. Wenn zwei Delegaten kombiniert werden oder eine aus einer anderen entfernt wird ([Delegatdeklarationen](delegates.md#delegate-declarations)), ergibt sich ein neuer Delegat. der Inhalt eines vorhandenen Delegaten wurde nicht geändert.

Es ist nicht möglich, einen Delegaten zu erstellen, der auf eine Eigenschaft, einen Indexer, einen benutzerdefinierten Operator, einen Instanzkonstruktor, einen Dekonstruktor oder einen statischen Konstruktor verweist.

Wie oben beschrieben, wird beim Erstellen eines Delegaten aus einer Methoden Gruppe die Liste der formalen Parameter und der Rückgabetyp des Delegaten bestimmt, welche der überladenen Methoden ausgewählt werden sollen. Im Beispiel
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
Das `A.f` Feld wird mit einem Delegaten initialisiert, der auf die zweite `Square` Methode verweist, da diese Methode exakt mit der formalen Parameterliste und dem Rückgabetyp `DoubleFunc`übereinstimmt. Wenn die zweite `Square` Methode nicht vorhanden wäre, wäre ein Kompilierzeitfehler aufgetreten.

#### <a name="anonymous-object-creation-expressions"></a>Ausdrücke zum Erstellen anonymer Objekte

Ein *anonymous_object_creation_expression* wird zum Erstellen eines Objekts eines anonymen Typs verwendet.

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

Ein Anonymer Objektinitialisierer deklariert einen anonymen Typ und gibt eine Instanz dieses Typs zurück. Ein anonymer Typ ist ein namenloser Klassentyp, der direkt von `object`erbt. Die Member eines anonymen Typs sind eine Sequenz von schreibgeschützten Eigenschaften, die vom anonymen Objektinitialisierer abgeleitet werden, um eine Instanz des Typs zu erstellen. Genauer gesagt: ein Anonymer Objektinitialisierer des Formulars
```csharp
new { p1 = e1, p2 = e2, ..., pn = en }
```
deklariert einen anonymen Typ des Formulars.
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
wobei jede `Tx` der Typ des entsprechenden Ausdrucks `ex`ist. Der Ausdruck, der in einem *member_declarator* verwendet wird, muss einen Typ aufweisen. Daher ist es ein Kompilierzeitfehler für einen Ausdruck in einer *member_declarator* NULL oder eine anonyme Funktion zu sein. Es ist auch ein Kompilierzeitfehler, wenn der Ausdruck einen unsicheren Typ hat.

Die Namen eines anonymen Typs und des Parameters für die `Equals` Methode werden vom Compiler automatisch generiert, und im Programmtext kann nicht auf Sie verwiesen werden.

Innerhalb desselben Programms werden zwei anonyme Objektinitialisierer, die eine Sequenz von Eigenschaften mit denselben Namen und Kompilierzeit Typen in derselben Reihenfolge angeben, Instanzen desselben anonymen Typs erstellen.

Im Beispiel
```csharp
var p1 = new { Name = "Lawnmower", Price = 495.00 };
var p2 = new { Name = "Shovel", Price = 26.95 };
p1 = p2;
```
die Zuweisung in der letzten Zeile ist zulässig, da `p1` und `p2` denselben anonymen Typ haben.

Die Methoden `Equals` und `GetHashcode` für anonyme Typen überschreiben die Methoden, die von `object`geerbt wurden, und werden im Hinblick auf die `Equals` und `GetHashcode` der Eigenschaften definiert, sodass zwei Instanzen desselben anonymen Typs nur dann gleich sind, wenn alle Eigenschaften gleich sind.

Ein Element Deklarator kann mit einem einfachen Namen ([Typrückschluss](expressions.md#type-inference)), einem Element Zugriff ([Kompilierzeit Überprüfung der dynamischen Überladungs Auflösung](expressions.md#compile-time-checking-of-dynamic-overload-resolution)), einem Basis Zugriff ([Basis Zugriff](expressions.md#base-access)) oder einem NULL bedingten Member-Zugriff ([null bedingte Ausdrücke als Projektions Initialisierer](expressions.md#null-conditional-expressions-as-projection-initializers)) abgekürzt werden. Dies wird als ***Projektions Initialisierer*** bezeichnet und ist eine Abkürzung für eine Deklaration von und die Zuweisung zu einer Eigenschaft mit demselben Namen. Insbesondere Member-Deklaratoren der Formulare
```csharp
identifier
expr.identifier
```
entsprechen genau den folgenden:
```csharp
identifier = identifier
identifier = expr.identifier
```

Folglich wählt der *Bezeichner* in einem Projektions Initialisierer sowohl den Wert als auch das Feld oder die Eigenschaft aus, dem der Wert zugewiesen wird. Intuitiv projiziert ein Projektions Initialisierer nicht nur einen Wert, sondern auch den Namen des Werts.

### <a name="the-typeof-operator"></a>Der typeof-Operator

Der `typeof`-Operator wird zum Abrufen des `System.Type`-Objekts für einen Typ verwendet.

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

Die erste Form *typeof_expression* besteht aus einem `typeof` *Schlüsselwort*, gefolgt von einem Typ in Klammern. Das Ergebnis eines Ausdrucks dieses Formulars ist das `System.Type` Objekt für den bestimmten Typ. Es ist nur ein `System.Type` Objekt für einen bestimmten Typ vorhanden. Dies bedeutet, dass für einen Typ `T``typeof(T) == typeof(T)` immer true ist. Der *Typ* kann nicht `dynamic`werden.

Die zweite Form von *typeof_expression* besteht aus einem `typeof` Schlüsselwort, gefolgt von einer *unbound_type_name*in Klammern. Ein- *unbound_type_name* ähnelt einem *TYPE_NAME* ([Namespace und Typnamen](basic-concepts.md#namespace-and-type-names)), mit dem Unterschied, dass ein *unbound_type_name* *generic_dimension_specifier*s enthält, in denen eine *TYPE_NAME* *type_argument_list*s enthält. Wenn der Operand einer *typeof_expression* eine Sequenz von Token ist, die die Grammatiken von *unbound_type_name* und *TYPE_NAME*erfüllt, d. h., wenn weder ein *generic_dimension_specifier* noch ein *type_argument_list*enthalten ist, wird die Sequenz der Token als *TYPE_NAME*betrachtet. Die Bedeutung eines *unbound_type_name* wird wie folgt bestimmt:

*  Konvertieren Sie die Sequenz von Token in eine *TYPE_NAME* , indem Sie alle *generic_dimension_specifier* durch eine *type_argument_list* ersetzen, die dieselbe Anzahl von Kommas und das Schlüsselwort `object` wie jede *type_argument*.
*  Werten Sie die resultierende *TYPE_NAME*aus, während alle Typparameter Einschränkungen ignoriert werden.
*  Der *unbound_type_name* wird in den ungebundenen generischen Typ aufgelöst, der dem resultierenden konstruierten Typ zugeordnet ist ([gebundene und ungebundene Typen](types.md#bound-and-unbound-types)).

Das Ergebnis des *typeof_expression* ist das `System.Type`-Objekt für den resultierenden ungebundenen generischen Typ.

Die dritte Form von *typeof_expression* besteht aus einem `typeof` Schlüsselwort, gefolgt von einem `void` Schlüsselwort in Klammern. Das Ergebnis eines Ausdrucks dieses Formulars ist das `System.Type` Objekt, das das Fehlen eines Typs darstellt. Das von `typeof(void)` zurückgegebene Typobjekt unterscheidet sich vom Typobjekt, das für einen beliebigen Typ zurückgegeben wurde. Dieses besondere Typobjekt eignet sich für Klassenbibliotheken, die die Reflektion auf Methoden in der Sprache zulassen, wobei diese Methoden eine Möglichkeit haben möchten, den Rückgabetyp jeder Methode, einschließlich void-Methoden, mit einer Instanz von `System.Type`darzustellen.

Der `typeof`-Operator kann für einen Typparameter verwendet werden. Das Ergebnis ist das `System.Type` Objekt für den Lauf Zeittyp, der an den Typparameter gebunden wurde. Der `typeof`-Operator kann auch für einen konstruierten Typ oder einen ungebundenen generischen Typ ([gebundene und ungebundene Typen](types.md#bound-and-unbound-types)) verwendet werden. Das `System.Type`-Objekt für einen ungebundenen generischen Typ ist nicht mit dem `System.Type` Objekt des Instanztyps identisch. Der Instanztyp ist zur Laufzeit immer ein geschlossener konstruierter Typ, damit das `System.Type` Objekt von den verwendeten Lauf Zeittyp Argumenten abhängt, während der ungebundene generische Typ keine Typargumente aufweist.

Das Beispiel
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
```console
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

Beachten Sie, dass `int` und `System.Int32` denselben Typ haben.

Beachten Sie auch, dass das Ergebnis `typeof(X<>)` nicht vom Typargument abhängt, aber das Ergebnis von `typeof(X<T>)` ist.

### <a name="the-checked-and-unchecked-operators"></a>Checked- und Unchecked-Operatoren

Mithilfe der Operatoren "`checked`" und "`unchecked`" wird der ***Überlauf Überprüfungs Kontext*** für arithmetische Operationen und Konvertierungen im integralen Typ gesteuert.

```antlr
checked_expression
    : 'checked' '(' expression ')'
    ;

unchecked_expression
    : 'unchecked' '(' expression ')'
    ;
```

Der `checked`-Operator wertet den enthaltenen Ausdruck in einem überprüften Kontext aus, und der `unchecked` Operator wertet den enthaltenen Ausdruck in einem nicht überprüften Kontext aus. Ein *checked_expression* oder *unchecked_expression* entspricht genau einem *parenthesized_expression* (in[Klammern Klammern](expressions.md#parenthesized-expressions)), mit dem Unterschied, dass der enthaltene Ausdruck im angegebenen Überlauf Prüfungs Kontext ausgewertet wird.

Der Überlauf Überprüfungs Kontext kann auch durch die Anweisungen `checked` und `unchecked` gesteuert werden ([die](statements.md#the-checked-and-unchecked-statements)aktivierten und deaktivierten Anweisungen).

Die folgenden Vorgänge wirken sich auf den Überlauf Prüfungs Kontext aus, der durch die `checked`-und `unchecked` Operatoren und-Anweisungen festgelegt wird:

*  Der vordefinierte `++` und `--` unäre Operatoren ([postfix-Inkrement-und Dekrementoperatoren](expressions.md#postfix-increment-and-decrement-operators) und [Präfix Inkrement-und Dekrementoperatoren](expressions.md#prefix-increment-and-decrement-operators)), wenn der Operand ein ganzzahliger Typ ist.
*  Der vordefinierte `-` unärer Operator ([unärer Minus Operator](expressions.md#unary-minus-operator)), wenn der Operand ein ganzzahliger Typ ist.
*  Der vordefinierte `+`-, `-`-, `*`-und `/` binäre Operatoren ([arithmetische Operatoren](expressions.md#arithmetic-operators)), wenn beide Operanden ganzzahlige Typen sind.
*  Explizite numerische Konvertierungen ([explizite numerische Konvertierungen](conversions.md#explicit-numeric-conversions)) von einem ganzzahligen Typ in einen anderen ganzzahligen Typ oder von `float` oder `double` in einen ganzzahligen Typ.

Wenn eine der obigen Vorgänge ein Ergebnis erzeugt, das zu groß für die Darstellung im Zieltyp ist, steuert der Kontext, in dem der Vorgang ausgeführt wird, das resultierende Verhalten:

*  In einem `checked` Kontext tritt ein Kompilierzeitfehler auf, wenn es sich bei dem Vorgang um einen konstanten Ausdruck ([Konstante Ausdrücke](expressions.md#constant-expressions)) handelt. Andernfalls wird eine `System.OverflowException` ausgelöst, wenn der Vorgang zur Laufzeit ausgeführt wird.
*  In einem `unchecked` Kontext wird das Ergebnis abgeschnitten, indem alle höherwertigen Bits verworfen werden, die nicht in den Zieltyp passen.

Für nicht konstante Ausdrücke (Ausdrücke, die zur Laufzeit ausgewertet werden), die nicht von `checked`-oder `unchecked` Operatoren oder-Anweisungen eingeschlossen werden, wird der Standardkontext für die Überlauf Überprüfung `unchecked`, es sei denn, externe Faktoren (z. b. Compilerschalter und Ausführungs Umgebungs Konfiguration) werden für `checked` Auswertung aufgerufen.

Für konstante Ausdrücke (Ausdrücke, die zur Kompilierzeit vollständig ausgewertet werden können) ist der Standardkontext der Überlauf Überprüfung immer `checked`. Wenn ein konstanter Ausdruck nicht explizit in einem `unchecked` Kontext abgelegt wird, verursachen über Flüsse, die während der Kompilierzeit Auswertung des Ausdrucks auftreten, immer Kompilierzeitfehler.

Der Text einer anonymen Funktion ist nicht von `checked` oder `unchecked` Kontexten betroffen, in denen die anonyme Funktion auftritt.

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
Es werden keine Kompilierzeitfehler gemeldet, da keiner der Ausdrücke zur Kompilierzeit ausgewertet werden kann. Zur Laufzeit löst die `F`-Methode eine `System.OverflowException`aus, und die `G`-Methode gibt-727379968 (die unteren 32 Bits des Ergebnisses außerhalb des gültigen Bereichs) zurück. Das Verhalten der `H`-Methode hängt vom Standardkontext der Überlauf Überprüfung für die Kompilierung ab, ist jedoch entweder identisch mit `F` oder identisch mit `G`.

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
die über Flüsse, die beim Auswerten der Konstanten Ausdrücke in `F` und `H` auftreten, bewirken, dass Kompilierzeitfehler gemeldet werden, weil die Ausdrücke in einem `checked` Kontext ausgewertet werden. Ein Überlauf tritt auch auf, wenn der Konstante Ausdruck in `G`ausgewertet wird. da die Auswertung jedoch in einem `unchecked` Kontext stattfindet, wird der Überlauf nicht gemeldet.

Die Operatoren "`checked`" und "`unchecked`" wirken sich nur auf den Überlauf Überprüfungs Kontext für die Vorgänge aus, die in den Token "`(`" und "`)`" textuell enthalten sind. Die Operatoren haben keine Auswirkung auf Funktionsmember, die als Ergebnis der Auswertung des enthaltenen Ausdrucks aufgerufen werden. Im Beispiel
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
die Verwendung von `checked` in `F` wirkt sich nicht auf die Auswertung von `x * y` in `Multiply`aus, sodass `x * y` im Standardkontext der Überlauf Überprüfung ausgewertet wird.

Der `unchecked`-Operator ist praktisch, wenn Konstanten der ganzzahligen Typen mit Vorzeichen in hexadezimal Notation geschrieben werden. Beispiel:
```csharp
class Test
{
    public const int AllBits = unchecked((int)0xFFFFFFFF);

    public const int HighBit = unchecked((int)0x80000000);
}
```

Beide hexadezimal Konstanten oben sind vom Typ `uint`. Da sich die Konstanten außerhalb des `int` Bereichs ohne den `unchecked`-Operator befinden, würden die Umwandlungen in `int` Kompilierzeitfehler verursachen.

Die Operatoren und Anweisungen von `checked` und `unchecked` ermöglichen es Programmierern, bestimmte Aspekte einiger numerischer Berechnungen zu steuern. Das Verhalten einiger numerischer Operatoren hängt jedoch von den Datentypen ihrer Operanden ab. Beispielsweise führt die Multiplikation von zwei Dezimalzahlen immer zu einer Ausnahme bei einem Überlauf, auch innerhalb eines explizit `unchecked` Konstrukts. Analog dazu führt das Multiplizieren von zwei Gleit Komma zahlen niemals zu einer Ausnahme bei einem Überlauf, auch innerhalb eines explizit `checked` Konstrukts. Außerdem sind andere Operatoren nie von dem Überprüfungs Modus betroffen, ob Standard oder explizit.

### <a name="default-value-expressions"></a>Standardwert Ausdrücke

Ein Standardwert Ausdruck wird zum Abrufen des Standardwerts ([Standardwerte](variables.md#default-values)) eines Typs verwendet. In der Regel wird ein Standardwert Ausdruck für Typparameter verwendet, da möglicherweise nicht bekannt ist, ob der Typparameter ein Werttyp oder ein Verweistyp ist. (Es ist keine Konvertierung vom `null` literalen zu einem Typparameter vorhanden, es sei denn, der Typparameter ist bekannt als Verweistyp.)

```antlr
default_value_expression
    : 'default' '(' type ')'
    ;
```

Wenn der *Typ* in einer *default_value_expression* zur Laufzeit als Verweistyp ausgewertet wird, wird das Ergebnis `null` in diesen Typ konvertiert. Wenn der *Typ* in einer *default_value_expression* zur Laufzeit auf einen Werttyp ausgewertet wird, ist das Ergebnis der Standardwert der *value_type*([Standardkonstruktoren](types.md#default-constructors)).

Ein *default_value_expression* ist ein konstanter Ausdruck ([Konstante Ausdrücke](expressions.md#constant-expressions)), wenn der Typ ein Verweistyp oder ein Typparameter ist, der als Verweistyp bekannt ist ([Typparameter Einschränkungen](classes.md#type-parameter-constraints)). Außerdem ist ein *default_value_expression* ein konstanter Ausdruck, wenn der Typ einem der folgenden Werttypen entspricht: `sbyte`, `byte`, `short`, `ushort`, `int`, `uint`, `long`, `ulong`, `char`, `float`, `double`, `decimal`, `bool`oder beliebiger Enumerationstyp.


### <a name="nameof-expressions"></a>Nameof-Ausdrücke

Ein *nameof_expression* wird zum Abrufen des Namens einer Programm Entität als Konstante Zeichenfolge verwendet.

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

Der *named_entity* Operand ist immer ein Ausdruck. Da `nameof` kein reserviertes Schlüsselwort ist, ist ein nameof-Ausdruck immer syntaktisch mehrdeutig, wenn der einfache Name `nameof`ist. Aus Kompatibilitätsgründen wird der Ausdruck als *invocation_expression* behandelt, wenn eine Namenssuche ([simple names](expressions.md#simple-names)) des Namens `nameof` erfolgreich ist. Dies gilt unabhängig davon, ob der Aufruf zulässig ist. Andernfalls handelt es sich um einen *nameof_expression*.

Die Bedeutung des *named_entity* eines *nameof_expression* ist die Bedeutung des Ausdrucks als Ausdruck. Das heißt, entweder als *Simple_name*, als *base_access* oder als *member_access*. Wenn die in [einfachen Namen](expressions.md#simple-names) und dem Element [Zugriff](expressions.md#member-access) beschriebene Suche jedoch zu einem Fehler führt, weil ein Instanzmember in einem statischen Kontext gefunden wurde, erzeugt eine *nameof_expression* keinen derartigen Fehler.

Es handelt sich um einen Kompilierzeitfehler für eine *named_entity* die eine Methoden Gruppe für eine *type_argument_list*festlegt. Es handelt sich um einen Kompilierzeitfehler für eine *named_entity_target* , die den Typ `dynamic`.

Ein *nameof_expression* ist ein konstanter Ausdruck vom Typ `string`und hat zur Laufzeit keine Auswirkung. Insbesondere wird der *named_entity* nicht ausgewertet, und wird für die konkrete Zuweisungs Analyse ignoriert ([Allgemeine Regeln für einfache Ausdrücke](variables.md#general-rules-for-simple-expressions)). Der Wert ist der letzte Bezeichner des *named_entity* vor der optionalen abschließenden *type_argument_list*, wie folgt transformiert:

* Wenn diese verwendet wird, wird das Präfix "`@`" entfernt.
* Jede *unicode_escape_sequence* wird in das entsprechende Unicode-Zeichen transformiert.
* Alle *formatting_characters* werden entfernt.

Dabei handelt es sich um dieselben Transformationen, die beim Testen der Gleichheit zwischen bezeichlern in [Bezeichner](lexical-structure.md#identifiers) angewendet werden.

TODO: Beispiele

### <a name="anonymous-method-expressions"></a>Anonyme Methoden Ausdrücke

Eine *anonymous_method_expression* ist eine von zwei Möglichkeiten, eine anonyme Funktion zu definieren. Diese werden in [Anonyme Funktions Ausdrücke](expressions.md#anonymous-function-expressions)weiter unten beschrieben.

## <a name="unary-operators"></a>Unäre Operatoren

Die Operatoren `?`, `+`, `-`, `!`, `~`, `++`, `--`, Cast und `await` werden als unäre Operatoren bezeichnet.

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

Wenn der Operand einer *unary_expression* den Kompilier Zeittyp `dynamic`hat, ist er dynamisch gebunden ([dynamische Bindung](expressions.md#dynamic-binding)). In diesem Fall wird der Kompilier Zeittyp der *unary_expression* `dynamic`, und die unten beschriebene Auflösung erfolgt zur Laufzeit mithilfe des Lauf Zeit Typs des Operanden.

### <a name="null-conditional-operator"></a>NULL Bedingter Operator

Der NULL bedingte Operator wendet eine Liste von Vorgängen auf seinen Operanden nur an, wenn dieser Operand nicht NULL ist. Andernfalls wird das Ergebnis der Anwendung des Operators `null`.

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

Die Liste der Vorgänge kann Member-und Element Zugriffs Vorgänge (die selbst bedingt NULL sein können) sowie Aufrufe einschließen.

Beispielsweise ist der Ausdruck `a.b?[0]?.c()` ein *null_conditional_expression* mit einer *primary_expression* `a.b` und *null_conditional_operations* `?[0]` (null bedingter Element Zugriff), `?.c` (null bedingter Member-Zugriff) und `()` (Aufruf).

Bei einem *null_conditional_expression* `E` mit einer *primary_expression* `P``E0` der Ausdruck sein, der durch das textuellen Entfernen der führenden `?` aus den *null_conditional_operations* `E` der abgerufen werden kann. Konzeptionell ist `E0` der Ausdruck, der ausgewertet wird, wenn keine der durch die `?`s dargestellten Null-Überprüfungen eine `null`finden.

Verwenden Sie `E1` auch den Ausdruck, der durch das textumen Entfernen der führenden `?` aus nur dem ersten *null_conditional_operations* in `E`erreicht werden kann. Dies kann zu einem *primären Ausdruck* (falls nur ein `?`) oder zu einem anderen *null_conditional_expression*führen.

Wenn `E` z. b. der Ausdruck `a.b?[0]?.c()`ist, ist `E0` der Ausdruck `a.b[0].c()` und `E1` ist der Ausdruck `a.b[0]?.c()`.

Wenn `E0` als "Nothing" klassifiziert wird, wird `E` als "Nothing" klassifiziert. Andernfalls wird E als Wert klassifiziert.

`E0` und `E1` werden verwendet, um die Bedeutung von `E`zu bestimmen:

*  Wenn `E` als *statement_expression* auftritt, ist die Bedeutung von `E` identisch mit der-Anweisung.

   ```csharp
   if ((object)P != null) E1;
   ```

   mit der Ausnahme, dass P nur einmal ausgewertet wird.

*  Andernfalls, wenn `E0` als "Nothing" klassifiziert wird, tritt ein Kompilierzeitfehler auf.

*  Andernfalls `T0` Sie den Typ der `E0`.

   *  Wenn `T0` ein Typparameter ist, der nicht bekannt ist, dass es sich um einen Verweistyp oder einen nicht auf NULL festleg baren Werttyp handelt, tritt ein Kompilierzeitfehler auf.

   *  Wenn `T0` ein Werttyp ist, der keine NULL-Werte zulässt, ist der Typ der `E` `T0?`, und die Bedeutung von `E` ist identisch mit.

      ```csharp
      ((object)P == null) ? (T0?)null : E1
      ```

      mit der Ausnahme, dass `P` nur einmal ausgewertet wird.

   *  Andernfalls ist der Typ von e t0, und die Bedeutung von e ist identisch mit.

      ```csharp
      ((object)P == null) ? null : E1
      ```

      mit der Ausnahme, dass `P` nur einmal ausgewertet wird.

Wenn `E1` selbst ein *null_conditional_expression*ist, werden diese Regeln erneut angewendet, wobei die Tests für `null` geschachtelt werden, bis keine weiteren `?`vorhanden sind, und der Ausdruck wurde bis zum primär Ausdrucks `E0`reduziert.

Wenn z. b. der Ausdruck `a.b?[0]?.c()` als-Anweisungs Ausdruck auftritt, wie in der-Anweisung:
```csharp
a.b?[0]?.c();
```
seine Bedeutung ist äquivalent zu:
```csharp
if (a.b != null) a.b[0]?.c();
```
Das wiederum entspricht:
```csharp
if (a.b != null) if (a.b[0] != null) a.b[0].c();
```
Mit der Ausnahme, dass `a.b` und `a.b[0]` nur einmal ausgewertet werden.

Wenn Sie in einem Kontext auftritt, in dem ihr Wert verwendet wird, wie in:
```csharp
var x = a.b?[0]?.c();
```
und vorausgesetzt, dass der Typ des letzten aufzurufenden aufzurufenden nicht auf NULL festleg baren Werttypen ist, entspricht seine Bedeutung folgenden Werten:
```csharp
var x = (a.b == null) ? null : (a.b[0] == null) ? null : a.b[0].c();
```
mit der Ausnahme, dass `a.b` und `a.b[0]` nur einmal ausgewertet werden.

#### <a name="null-conditional-expressions-as-projection-initializers"></a>NULL bedingte Ausdrücke als Projektions Initialisierer

Ein NULL bedingter Ausdruck ist nur als *member_declarator* in einer *anonymous_object_creation_expression* zulässig (Ausdrücke zum[Erstellen anonymer Objekte](expressions.md#anonymous-object-creation-expressions)), wenn er mit einem (optional NULL bedingten) Member-Zugriff endet. Diese Anforderung kann wie folgt ausgedrückt werden:

```antlr
null_conditional_member_access
    : primary_expression null_conditional_operations? '?' '.' identifier type_argument_list?
    | primary_expression null_conditional_operations '.' identifier type_argument_list?
    ;
```

Dies ist ein Sonderfall der Grammatik für *null_conditional_expression* oben. Die Produktion für *member_declarator* in [Ausdrücken der anonymen Objekt Erstellung](expressions.md#anonymous-object-creation-expressions) enthält dann nur *null_conditional_member_access*.

#### <a name="null-conditional-expressions-as-statement-expressions"></a>NULL bedingte Ausdrücke als Anweisungs Ausdrücke

Ein NULL bedingter Ausdruck ist nur als *statement_expression* ([Ausdrucks Anweisungen](statements.md#expression-statements)) zulässig, wenn er mit einem Aufruf endet. Diese Anforderung kann wie folgt ausgedrückt werden:

```antlr
null_conditional_invocation_expression
    : primary_expression null_conditional_operations '(' argument_list? ')'
    ;
```

Dies ist ein Sonderfall der Grammatik für *null_conditional_expression* oben. Die Produktion für *statement_expression* in [Ausdrucks Anweisungen](statements.md#expression-statements) enthält dann nur *null_conditional_invocation_expression*.


### <a name="unary-plus-operator"></a>Unärer Plus-Operator

Bei einem Vorgang im Formular `+x`wird die Überladungs Auflösung des unären Operators ([unäre Operator Überladungs Auflösung](expressions.md#unary-operator-overload-resolution)) angewendet, um eine bestimmte Operator Implementierung auszuwählen. Der Operand wird in den Parametertyp des ausgewählten Operators konvertiert, und der Ergebnistyp ist der Rückgabetyp des Operators. Die vordefinierten unären plus Operatoren sind:

```csharp
int operator +(int x);
uint operator +(uint x);
long operator +(long x);
ulong operator +(ulong x);
float operator +(float x);
double operator +(double x);
decimal operator +(decimal x);
```

Für jeden dieser Operatoren ist das Ergebnis einfach der Wert des Operanden.

### <a name="unary-minus-operator"></a>Unärer Minusoperator

Bei einem Vorgang im Formular `-x`wird die Überladungs Auflösung des unären Operators ([unäre Operator Überladungs Auflösung](expressions.md#unary-operator-overload-resolution)) angewendet, um eine bestimmte Operator Implementierung auszuwählen. Der Operand wird in den Parametertyp des ausgewählten Operators konvertiert, und der Ergebnistyp ist der Rückgabetyp des Operators. Die vordefinierten Negations Operatoren lauten wie folgt:

*  Ganzzahlige Negation:

   ```csharp
   int operator -(int x);
   long operator -(long x);
   ```

   Das Ergebnis wird berechnet, indem `x` von NULL subtrahieren. Wenn der Wert von `x` der kleinste darstellbare Wert des Operanden Typs (-2 ^ 31 für `int` oder-2 ^ 63 für `long`) ist, kann die mathematische Negation der `x` innerhalb des Operanden Typs nicht darstellbar sein. Wenn dies in einem `checked` Kontext auftritt, wird eine `System.OverflowException` ausgelöst. Wenn Sie in einem `unchecked` Kontext auftritt, ist das Ergebnis der Wert des Operanden, und der Überlauf wird nicht gemeldet.

   Wenn der Operand des Negations Operators vom Typ `uint`ist, wird er in den Typ `long`konvertiert, und der Ergebnistyp ist `long`. Eine Ausnahme ist die Regel, die zulässt, dass der `int` Wert-2147483648 (-2 ^ 31) als dezimales Ganzzahlliteral ([ganzzahlige Literale](lexical-structure.md#integer-literals)) geschrieben wird.

   Wenn der Operand des Negations Operators vom Typ `ulong`ist, tritt ein Kompilierzeitfehler auf. Eine Ausnahme ist die Regel, die zulässt, dass der `long` Wert-9.223.372.036.854.775.808 (-2 ^ 63) als dezimales Ganzzahlliteral ([ganzzahlige Literale](lexical-structure.md#integer-literals)) geschrieben wird.

*  Gleit Komma Negation:

   ```csharp
   float operator -(float x);
   double operator -(double x);
   ```

   Das Ergebnis ist der Wert von `x` mit umgekehrtem Vorzeichen. Wenn `x` NaN ist, ist das Ergebnis ebenfalls NaN.

*  Dezimale Negation:

   ```csharp
   decimal operator -(decimal x);
   ```

   Das Ergebnis wird berechnet, indem `x` von NULL subtrahieren. Die Dezimale Negation entspricht der Verwendung des unären Minus Operators vom Typ `System.Decimal`.

### <a name="logical-negation-operator"></a>Logischer Negationsoperator

Bei einem Vorgang im Formular `!x`wird die Überladungs Auflösung des unären Operators ([unäre Operator Überladungs Auflösung](expressions.md#unary-operator-overload-resolution)) angewendet, um eine bestimmte Operator Implementierung auszuwählen. Der Operand wird in den Parametertyp des ausgewählten Operators konvertiert, und der Ergebnistyp ist der Rückgabetyp des Operators. Es ist nur ein vordefinierter logischer Negations Operator vorhanden:
```csharp
bool operator !(bool x);
```

Dieser Operator berechnet die logische Negation des Operanden: Wenn der Operand `true`ist, wird das Ergebnis `false`. Wenn der Operand `false`ist, wird das Ergebnis `true`.

### <a name="bitwise-complement-operator"></a>Bitweiser Komplement Operator

Bei einem Vorgang im Formular `~x`wird die Überladungs Auflösung des unären Operators ([unäre Operator Überladungs Auflösung](expressions.md#unary-operator-overload-resolution)) angewendet, um eine bestimmte Operator Implementierung auszuwählen. Der Operand wird in den Parametertyp des ausgewählten Operators konvertiert, und der Ergebnistyp ist der Rückgabetyp des Operators. Die vordefinierten bitweisen Komplement Operatoren sind:
```csharp
int operator ~(int x);
uint operator ~(uint x);
long operator ~(long x);
ulong operator ~(ulong x);
```

Für jeden dieser Operatoren ist das Ergebnis des Vorgangs das bitweise Komplement von `x`.

Jeder Enumerationstyp `E` implizit den folgenden bitweisen Komplement Operator bereit:

```csharp
E operator ~(E x);
```

Das Ergebnis der Auswertung von `~x`, wobei `x` ein Ausdruck eines Enumerationstyps ist, der mit einem zugrunde liegenden Typ `U``E` ist, entspricht exakt dem Auswerten von `(E)(~(U)x)`, mit der Ausnahme,[dass die Konvertierung](expressions.md#the-checked-and-unchecked-operators)in `E` immer so ausgeführt wird, als ob es sich um einen `unchecked` Kontext handelt

### <a name="prefix-increment-and-decrement-operators"></a>Präfix-Inkrementoperator und Präfix-Dekrementoperator

```antlr
pre_increment_expression
    : '++' unary_expression
    ;

pre_decrement_expression
    : '--' unary_expression
    ;
```

Der Operand eines Präfix Inkrement-oder dekrementvorgangs muss ein Ausdruck sein, der als Variable, Eigenschafts Zugriff oder Indexerzugriff klassifiziert ist. Das Ergebnis des Vorgangs ist ein Wert desselben Typs wie der Operand.

Wenn der Operand einer Präfix Inkrement-oder Dekrementoperation eine Eigenschaft oder ein Indexer-Zugriff ist, muss die Eigenschaft oder der Indexer sowohl einen `get` als auch einen `set` Accessor aufweisen. Wenn dies nicht der Fall ist, tritt ein Bindungs Zeitfehler auf.

Unäre Operator Überladungs Auflösung ([unäre Operator Überladungs Auflösung](expressions.md#unary-operator-overload-resolution)) wird angewendet, um eine bestimmte Operator Implementierung auszuwählen. Vordefinierte `++` und `--` Operatoren sind für die folgenden Typen vorhanden: `sbyte`, `byte`, `short`, `ushort`, `int`, `uint`, `long`, `ulong`, `char`, `float`, `double`, `decimal`und beliebigen Enumerationstyp. Der vordefinierte `++`-Operatoren gibt den Wert zurück, der beim Hinzufügen von 1 zum Operanden erzeugt wurde, und die vordefinierten `--` Operatoren geben den Wert zurück, der durch die Subtraktion von 1 vom Operanden erzeugt Wenn in einem `checked` Kontext das Ergebnis dieser Addition oder Subtraktion außerhalb des Bereichs des Ergebnis Typs liegt und der Ergebnistyp ein ganzzahliger Typ oder Enumeration-Typ ist, wird eine `System.OverflowException` ausgelöst.

Die Lauf Zeit Verarbeitung eines Präfix Inkrement-oder dekrementvorgangs in der Form `++x` oder `--x` besteht aus den folgenden Schritten:

*   Wenn `x` als Variable klassifiziert ist:
    * `x` wird ausgewertet, um die Variable zu entwickeln.
    * Der ausgewählte Operator wird mit dem Wert `x` als Argument aufgerufen.
    * Der vom Operator zurückgegebene Wert wird an dem Speicherort gespeichert, der durch die Auswertung der `x`angegeben wird.
    * Der vom Operator zurückgegebene Wert wird zum Ergebnis des Vorgangs.
*   Wenn `x` als Eigenschaft oder Indexer-Zugriff klassifiziert ist:
    * Der Instanzausdruck (wenn `x` nicht `static`ist) und die Argumentliste (wenn `x` ein Indexer-Zugriff ist), der `x` zugeordnet ist, werden ausgewertet, und die Ergebnisse werden in den nachfolgenden `get`-und `set` Accessoraufrufen verwendet.
    * Der `get` Accessor von `x` wird aufgerufen.
    * Der ausgewählte Operator wird mit dem Wert aufgerufen, der vom `get`-Accessor als Argument zurückgegeben wurde.
    * Der `set` Accessor von `x` wird mit dem Wert aufgerufen, der vom Operator als `value` Argument zurückgegeben wurde.
    * Der vom Operator zurückgegebene Wert wird zum Ergebnis des Vorgangs.

Die Operatoren `++` und `--` unterstützen auch Postfix-Notation ([postfix-Inkrement-und Dekrementoperatoren](expressions.md#postfix-increment-and-decrement-operators)). In der Regel ist das Ergebnis von `x++` oder `x--` der Wert von `x` vor dem Vorgang, wohingegen das Ergebnis von `++x` oder `--x` der Wert von `x` nach dem Vorgang ist. In beiden Fällen hat `x` selbst nach dem Vorgang denselben Wert.

Eine `operator++`-oder `operator--`-Implementierung kann entweder mithilfe der Postfix-oder Präfix Notation aufgerufen werden. Es ist nicht möglich, separate Operator Implementierungen für die beiden Notationen zu haben.

### <a name="cast-expressions"></a>Umwandlungs Ausdrücke

Ein *cast_expression* wird verwendet, um einen Ausdruck explizit in einen bestimmten Typ zu konvertieren.

```antlr
cast_expression
    : '(' type ')' unary_expression
    ;
```

Eine *cast_expression* der Form `(T)E`, bei der `T` ein *Typ* ist und `E` ein *unary_expression*ist, führt eine explizite Konvertierung ([explizite Konvertierungen](conversions.md#explicit-conversions)) des Werts `E` für den Typ `T`durch. Wenn keine explizite Konvertierung von `E` in `T`vorhanden ist, tritt ein Bindungs Zeitfehler auf. Andernfalls ist das Ergebnis der Wert, der von der expliziten Konvertierung erzeugt wird. Das Ergebnis wird immer als Wert klassifiziert, auch wenn `E` eine Variable bezeichnet.

Die Grammatik für eine *cast_expression* führt zu bestimmten syntaktischen Mehrdeutigkeiten. Beispielsweise könnte der Ausdruck `(x)-y` als *cast_expression* interpretiert werden (eine Umwandlung von `-y` in den Typ `x`) oder als *additive_expression* in Kombination mit einer *parenthesized_expression* (der den Wert `x - y)`berechnet.

Um *cast_expression* Mehrdeutigkeiten aufzulösen, ist die folgende Regel vorhanden: eine Sequenz von einem oder mehreren *Token*([Leerraum](lexical-structure.md#white-space)), die in Klammern eingeschlossen ist, wird nur als Anfang eines *cast_expression* betrachtet, wenn mindestens einer der folgenden Punkte zutrifft:

*  Die Reihenfolge der Token ist die korrekte Grammatik für einen *Typ*, jedoch nicht für einen *Ausdruck*.
*  Die Reihenfolge der Token ist für einen *Typ*eine korrekte Grammatik, und das Token, das unmittelbar auf die schließenden Klammern folgt, ist das Token "`~`", das Token "`!`", das Token "`(`", ein *Bezeichner* ([Escapesequenzen für Unicode-Zeichen](lexical-structure.md#unicode-character-escape-sequences)). ), ein *Literalzeichen* ([Literale](lexical-structure.md#literals)) oder ein beliebiges *Schlüsselwort* ([Schlüsselwörter](lexical-structure.md#keywords)) außer `as` und `is`.

Der Begriff "korrekte Grammatik" oben bedeutet nur, dass die Abfolge der Token der jeweiligen grammatikerischen Produktion entsprechen muss. Dabei wird insbesondere nicht die tatsächliche Bedeutung von einzelnen Bezeichner berücksichtigt. Wenn beispielsweise `x` und `y` Bezeichner sind, ist `x.y` die richtige Grammatik für einen Typ, auch wenn `x.y` nicht tatsächlich einen Typ bezeichnet.

Aus der disambiguations-Regel folgt dies, wenn `x` und `y` Bezeichner sind, `(x)y`, `(x)(y)`und `(x)(-y)` *cast_expression*s sind, `(x)-y` jedoch nicht ist, auch wenn `x` einen Typ identifiziert. Wenn `x` jedoch ein Schlüsselwort ist, das einen vordefinierten Typ (z. b. `int`) identifiziert, sind alle vier Formulare *cast_expression*s (da ein solches Schlüsselwort nicht möglicherweise ein Ausdruck allein sein kann).

### <a name="await-expressions"></a>Erwartungs Ausdrücke

Der Erwartungs Operator wird verwendet, um die Auswertung der einschließenden Async-Funktion anzuhalten, bis der asynchrone Vorgang, der durch den Operanden dargestellt wird, abgeschlossen ist.

```antlr
await_expression
    : 'await' unary_expression
    ;
```

Ein- *await_expression* ist nur im Text einer Async-Funktion ([Iteratoren](classes.md#iterators)) zulässig. In der nächsten einschließenden asynchronen Funktion tritt möglicherweise keine *await_expression* an folgenden Stellen auf:

*  Innerhalb einer geschachtelten (nicht Async) anonymen Funktion
*  Innerhalb des Blocks einer *lock_statement*
*  In einem unsicheren Kontext

Beachten Sie, dass ein *await_expression* an den meisten Stellen innerhalb eines *query_expression*nicht vorkommen kann, da diese syntaktisch transformiert werden, um nicht asynchrone Lambda-Ausdrücke zu verwenden.

Innerhalb einer Async-Funktion können `await` nicht als Bezeichner verwendet werden. Daher gibt es keine syntaktische Mehrdeutigkeit zwischen "Erwartung-Ausdrücke" und verschiedenen Ausdrücken mit bezeichmern. Außerhalb der Async-Funktionen fungiert `await` als normaler Bezeichner.

Der Operand eines *await_expression* wird als ***Aufgabe***bezeichnet. Sie stellt einen asynchronen Vorgang dar, der zu dem Zeitpunkt, zu dem die *await_expression* ausgewertet wird, ausgeführt werden kann. Der erwartete Operator besteht darin, die Ausführung der einschließenden Async-Funktion anzuhalten, bis die erwartete Aufgabe abgeschlossen ist, und dann das Ergebnis zu erhalten.

#### <a name="awaitable-expressions"></a>Awanutzbare Ausdrücke

Die Aufgabe eines Erwartungs ***Ausdrucks muss erwartet werden.*** Wenn eine der folgenden Punkte enthält, kann ein Ausdruck `t` werden.

*  `t` vom Typ "Kompilierzeit" ist `dynamic`
*  `t` verfügt über eine barrierefreie Instanz-oder Erweiterungsmethode, die `GetAwaiter` ohne Parameter und ohne Typparameter und einem Rückgabetyp `A` aufgerufen wird, für den Folgendes gilt:
   * `A` implementiert die-Schnittstelle `System.Runtime.CompilerServices.INotifyCompletion` (im folgenden als `INotifyCompletion` bekannt).
   * `A` verfügt über eine barrierefreie, lesbare Instanzeigenschaft `IsCompleted` vom Typ `bool`
   * `A` verfügt über eine barrierefreie Instanzmethode `GetResult` ohne Parameter und ohne Typparameter.

Der Zweck der `GetAwaiter` Methode ist das Abrufen eines ***akellners*** für die Aufgabe. Der Typ `A` wird als ***akellnertyp*** für den Erwartungs Ausdruck bezeichnet.

Der Zweck der `IsCompleted` Eigenschaft ist, zu bestimmen, ob die Aufgabe bereits beendet ist. Wenn dies der Fall ist, muss die Evaluierung nicht angehalten werden.

Der Zweck der `INotifyCompletion.OnCompleted` Methode ist das Registrieren einer "Fortsetzung" für die Aufgabe. d. h. ein Delegat (vom Typ "`System.Action`"), der nach Abschluss der Aufgabe aufgerufen wird.

Der Zweck der `GetResult`-Methode besteht darin, das Ergebnis der Aufgabe zu erhalten, sobald Sie fertig ist. Dieses Ergebnis kann erfolgreich abgeschlossen werden, möglicherweise mit einem Ergebniswert, oder es handelt sich um eine Ausnahme, die von der `GetResult`-Methode ausgelöst wird.

#### <a name="classification-of-await-expressions"></a>Klassifizierung von Erwartungs Ausdrücken

Der Ausdruck `await t` wird auf die gleiche Weise klassifiziert wie der Ausdruck `(t).GetAwaiter().GetResult()`. Wenn der Rückgabetyp von `GetResult` `void`ist, wird die *await_expression* daher als "Nothing" klassifiziert. Wenn ein nicht-void-Rückgabetyp `T`ist, wird der *await_expression* als Wert des Typs `T`klassifiziert.

#### <a name="runtime-evaluation-of-await-expressions"></a>Lauf Zeit Auswertung von Erwartungs Ausdrücken

Zur Laufzeit wird der Ausdruck `await t` wie folgt ausgewertet:

*  Ein akellner-`a` wird durch Auswerten des Ausdrucks `(t).GetAwaiter()`abgerufen.
*  Eine `bool` `b` wird durch Auswerten des Ausdrucks `(a).IsCompleted`abgerufen.
*  Wenn `b` `false`, ist die Auswertung davon abhängig, ob `a` die-Schnittstelle implementiert `System.Runtime.CompilerServices.ICriticalNotifyCompletion` (wird aus Gründen der Kürze als `ICriticalNotifyCompletion` bekannt). Diese Überprüfung erfolgt zur Bindungs Zeit. Dies ist zur Laufzeit, wenn `a` den Kompilier Zeittyp `dynamic`und andernfalls zur Kompilierzeit. Geben Sie `r` den Wiederaufnahme Delegaten ([Iteratoren](classes.md#iterators)) an:
    * Wenn `a` `ICriticalNotifyCompletion`nicht implementiert, wird der Ausdruck `(a as (INotifyCompletion)).OnCompleted(r)` ausgewertet.
    * Wenn `a` `ICriticalNotifyCompletion`implementiert, wird der Ausdruck `(a as (ICriticalNotifyCompletion)).UnsafeOnCompleted(r)` ausgewertet.
    * Die Auswertung wird dann angehalten, und die Steuerung wird an den aktuellen Aufrufer der Async-Funktion zurückgegeben.
*  Entweder unmittelbar nach (wenn `b` `true`) oder nach einem späteren Aufruf des Wiederaufnahme Delegaten (wenn `b` `false`wurde), wird der Ausdrucks `(a).GetResult()` ausgewertet. Wenn ein Wert zurückgegeben wird, ist dieser Wert das Ergebnis der *await_expression*. Andernfalls ist das Ergebnis "Nothing".

Die Implementierung der Schnittstellen Methoden eines akellers `INotifyCompletion.OnCompleted` und `ICriticalNotifyCompletion.UnsafeOnCompleted` sollte bewirken, dass der Delegat`r` höchstens einmal aufgerufen wird. Andernfalls ist das Verhalten der einschließenden Async-Funktion nicht definiert.

## <a name="arithmetic-operators"></a>Arithmetische operatoren

Die Operatoren `*`, `/`, `%`, `+`und `-` werden als arithmetische Operatoren bezeichnet.

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

Wenn ein Operand eines arithmetischen Operators den Kompilier Zeittyp `dynamic`aufweist, wird der Ausdruck dynamisch gebunden ([dynamische Bindung](expressions.md#dynamic-binding)). In diesem Fall wird der Kompilier Zeittyp des Ausdrucks `dynamic`, und die unten beschriebene Auflösung findet zur Laufzeit statt, wobei der Lauf Zeittyp der Operanden verwendet wird, für die der Typ der Kompilierzeit `dynamic`ist.

### <a name="multiplication-operator"></a>Multiplikationsoperator

Bei einem Vorgang der Formular `x * y`wird die binäre Operator Überladungs Auflösung ([binäre Operator Überladungs Auflösung](expressions.md#binary-operator-overload-resolution)) angewendet, um eine bestimmte Operator Implementierung auszuwählen. Die Operanden werden in die Parametertypen des ausgewählten Operators konvertiert, und der Ergebnistyp ist der Rückgabetyp des Operators.

Die vordefinierten Multiplikations Operatoren sind unten aufgeführt. Alle Operatoren berechnen das Produkt `x` und `y`.

*  Integer-Multiplikation:

   ```csharp
   int operator *(int x, int y);
   uint operator *(uint x, uint y);
   long operator *(long x, long y);
   ulong operator *(ulong x, ulong y);
   ```

   Wenn sich das Produkt in einem `checked` Kontext außerhalb des Bereichs des Ergebnis Typs befindet, wird eine `System.OverflowException` ausgelöst. In einem `unchecked` Kontext werden Überläufe nicht gemeldet, und alle signifikanten höherwertigen Bits außerhalb des Bereichs des Ergebnis Typs werden verworfen.


*  Gleit Komma Multiplikation:

   ```csharp
   float operator *(float x, float y);
   double operator *(double x, double y);
   ```

   Das Produkt wird gemäß den Regeln der IEEE 754-Arithmetik berechnet. In der folgenden Tabelle werden die Ergebnisse aller möglichen Kombinationen von nicht NULL Endwerten, Nullen, Infinities und Nan-Werten aufgelistet. In der Tabelle sind `x` und `y` positive, endliche Werte. `z` ist das Ergebnis `x * y`. Wenn das Ergebnis für den Zieltyp zu groß ist, ist `z` unendlich. Wenn das Ergebnis für den Zieltyp zu klein ist, `z` gleich 0 (null) ist.

   |      |      |      |     |     |      |      |     |
   |:----:|-----:|:----:|:---:|:---:|:----:|:----:|:----|
   |      | +y   | -y   | +0  | -0  | +inf | -inf | NaN | 
   | +x   | \+ z   | -z   | +0  | -0  | +inf | -inf | NaN | 
   | -x   | -z   | \+ z   | -0  | +0  | -inf | +inf | NaN | 
   | +0   | +0   | -0   | +0  | -0  | NaN  | NaN  | NaN | 
   | -0   | -0   | +0   | -0  | +0  | NaN  | NaN  | NaN | 
   | +inf | +inf | -inf | NaN | NaN | +inf | -inf | NaN | 
   | -inf | -inf | +inf | NaN | NaN | -inf | +inf | NaN | 
   | NaN  | NaN  | NaN  | NaN | NaN | NaN  | NaN  | NaN | 

*  Dezimale Multiplikation:

   ```csharp
   decimal operator *(decimal x, decimal y);
   ```

   Wenn der resultierende Wert zu groß ist, um im `decimal`-Format dargestellt zu werden, wird eine `System.OverflowException` ausgelöst. Wenn der Ergebniswert zu klein ist, um ihn im `decimal` Format darzustellen, ist das Ergebnis 0 (null). Die Dezimalstellen des Ergebnisses, vor der Rundung, sind die Summe der Skalen der beiden Operanden.

   Die Dezimal Multiplikation entspricht der Verwendung des Multiplikations Operators vom Typ `System.Decimal`.


### <a name="division-operator"></a>Divisionsoperator

Bei einem Vorgang der Formular `x / y`wird die binäre Operator Überladungs Auflösung ([binäre Operator Überladungs Auflösung](expressions.md#binary-operator-overload-resolution)) angewendet, um eine bestimmte Operator Implementierung auszuwählen. Die Operanden werden in die Parametertypen des ausgewählten Operators konvertiert, und der Ergebnistyp ist der Rückgabetyp des Operators.

Die vordefinierten Divisions Operatoren sind unten aufgeführt. Die Operatoren berechnen alle den Quotienten `x` und `y`.

*  Ganzzahlige Division:

   ```csharp
   int operator /(int x, int y);
   uint operator /(uint x, uint y);
   long operator /(long x, long y);
   ulong operator /(ulong x, ulong y);
   ```

   Wenn der Wert des rechten Operanden 0 (null) ist, wird ein `System.DivideByZeroException` ausgelöst.

   Die Division rundet das Ergebnis auf 0 (null). Daher ist der absolute Wert des Ergebnisses die größtmögliche Ganzzahl, die kleiner oder gleich dem absoluten Wert des Quotienten der beiden Operanden ist. Das Ergebnis ist 0 (null) oder positiv, wenn die beiden Operanden das gleiche Vorzeichen aufweisen und 0 (null) oder negativ ist, wenn die beiden Operanden gegenteilige Vorzeichen verfügen.

   Wenn der linke Operand der kleinste darstellbare `int` oder `long` Wert ist und der rechte Operand `-1`ist, tritt ein Überlauf auf. In einem `checked` Kontext bewirkt dies, dass eine `System.ArithmeticException` (oder eine Unterklasse davon) ausgelöst wird. In einem `unchecked` Kontext ist die Implementierung definiert, ob eine `System.ArithmeticException` (oder eine Unterklasse) ausgelöst wird, oder der Überlauf wird nicht gemeldet, und der resultierende Wert ist der des linken Operanden.

*  Gleit Komma Division:

   ```csharp
   float operator /(float x, float y);
   double operator /(double x, double y);
   ```

   Der Quotienten wird gemäß den Regeln der IEEE 754-Arithmetik berechnet. In der folgenden Tabelle werden die Ergebnisse aller möglichen Kombinationen von nicht NULL Endwerten, Nullen, Infinities und Nan-Werten aufgelistet. In der Tabelle sind `x` und `y` positive, endliche Werte. `z` ist das Ergebnis `x / y`. Wenn das Ergebnis für den Zieltyp zu groß ist, ist `z` unendlich. Wenn das Ergebnis für den Zieltyp zu klein ist, `z` gleich 0 (null) ist.

   |      |      |      |      |      |      |      |      |
   |:----:|:----:|:----:|:----:|:----:|:----:|:----:|:----:|
   |      | +y   | -y   | +0   | -0   | +inf | -inf | NaN  | 
   | +x   | \+ z   | -z   | +inf | -inf | +0   | -0   | NaN  | 
   | -x   | -z   | \+ z   | -inf | +inf | -0   | +0   | NaN  | 
   | +0   | +0   | -0   | NaN  | NaN  | +0   | -0   | NaN  | 
   | -0   | -0   | +0   | NaN  | NaN  | -0   | +0   | NaN  | 
   | +inf | +inf | -inf | +inf | -inf | NaN  | NaN  | NaN  | 
   | -inf | -inf | +inf | -inf | +inf | NaN  | NaN  | NaN  | 
   | NaN  | NaN  | NaN  | NaN  | NaN  | NaN  | NaN  | NaN  | 

*  Dezimale Division:

   ```csharp
   decimal operator /(decimal x, decimal y);
   ```

   Wenn der Wert des rechten Operanden 0 (null) ist, wird ein `System.DivideByZeroException` ausgelöst. Wenn der resultierende Wert zu groß ist, um im `decimal`-Format dargestellt zu werden, wird eine `System.OverflowException` ausgelöst. Wenn der Ergebniswert zu klein ist, um ihn im `decimal` Format darzustellen, ist das Ergebnis 0 (null). Die Skala des Ergebnisses ist die kleinste Skala, die ein Ergebnis mit dem nächstgelegenen darstellbaren Dezimalwert dem tatsächlichen mathematischen Ergebnis beibehält.

   Die Dezimal Division entspricht der Verwendung des Divisions Operators vom Typ `System.Decimal`.


### <a name="remainder-operator"></a>Restoperator

Bei einem Vorgang der Formular `x % y`wird die binäre Operator Überladungs Auflösung ([binäre Operator Überladungs Auflösung](expressions.md#binary-operator-overload-resolution)) angewendet, um eine bestimmte Operator Implementierung auszuwählen. Die Operanden werden in die Parametertypen des ausgewählten Operators konvertiert, und der Ergebnistyp ist der Rückgabetyp des Operators.

Die vordefinierten Rest-Operatoren sind unten aufgeführt. Die Operatoren berechnen alle den Rest der Division zwischen `x` und `y`.

*  Ganzzahliger Rest:

   ```csharp
   int operator %(int x, int y);
   uint operator %(uint x, uint y);
   long operator %(long x, long y);
   ulong operator %(ulong x, ulong y);
   ```

   Das Ergebnis `x % y` ist der Wert, der von `x - (x / y) * y`erzeugt wird. Wenn `y` NULL ist, wird eine `System.DivideByZeroException` ausgelöst.

   Wenn der linke Operand der kleinste `int` oder `long` Wert und der rechte Operand `-1`ist, wird ein `System.OverflowException` ausgelöst. In keinem Fall löst `x % y` eine Ausnahme aus, wenn `x / y` keine Ausnahme auslösen würde.

*  Gleit Komma Restwert:

   ```csharp
   float operator %(float x, float y);
   double operator %(double x, double y);
   ```

   In der folgenden Tabelle werden die Ergebnisse aller möglichen Kombinationen von nicht NULL Endwerten, Nullen, Infinities und Nan-Werten aufgelistet. In der Tabelle sind `x` und `y` positive, endliche Werte. `z` ist das Ergebnis `x % y` und wird als `x - n * y`berechnet, wobei `n` der größtmöglichen ganzzahligen Wert ist, der kleiner oder gleich `x / y`ist. Diese Methode zum Berechnen des Restwerts ist analog zu der für ganzzahlige Operanden verwendeten, unterscheidet sich jedoch von der IEEE 754-Definition (bei der `n` die ganze Zahl ist, die `x / y`).

   |      |      |      |      |      |      |      |      |
   |:----:|:----:|:----:|:----:|:----:|:----:|:----:|:----:|
   |      | +y   | -y   | +0   | -0   | +inf | -inf | NaN  | 
   | +x   | \+ z   | \+ z   | NaN  | NaN  | t    | t    | NaN  | 
   | -x   | -z   | -z   | NaN  | NaN  | -x   | -x   | NaN  | 
   | +0   | +0   | +0   | NaN  | NaN  | +0   | +0   | NaN  | 
   | -0   | -0   | -0   | NaN  | NaN  | -0   | -0   | NaN  | 
   | +inf | NaN  | NaN  | NaN  | NaN  | NaN  | NaN  | NaN  | 
   | -inf | NaN  | NaN  | NaN  | NaN  | NaN  | NaN  | NaN  | 
   | NaN  | NaN  | NaN  | NaN  | NaN  | NaN  | NaN  | NaN  | 

*  Dezimal Restwert:

   ```csharp
   decimal operator %(decimal x, decimal y);
   ```

   Wenn der Wert des rechten Operanden 0 (null) ist, wird ein `System.DivideByZeroException` ausgelöst. Die Dezimalstellen des Ergebnisses, vor jeder Rundung, sind die größere der Skalen der beiden Operanden, und das Vorzeichen des Ergebnisses ist, wenn ungleich NULL, mit dem von `x`identisch.

   Der Dezimal Rest entspricht der Verwendung des Rest-Operators vom Typ `System.Decimal`.


### <a name="addition-operator"></a>Additionsoperator

Bei einem Vorgang der Formular `x + y`wird die binäre Operator Überladungs Auflösung ([binäre Operator Überladungs Auflösung](expressions.md#binary-operator-overload-resolution)) angewendet, um eine bestimmte Operator Implementierung auszuwählen. Die Operanden werden in die Parametertypen des ausgewählten Operators konvertiert, und der Ergebnistyp ist der Rückgabetyp des Operators.

Die vordefinierten Additions Operatoren sind unten aufgeführt. Für numerische und Enumerationstypen berechnen die vordefinierten Additions Operatoren die Summe der beiden Operanden. Wenn ein oder beide Operanden vom Typ Zeichenfolge sind, verketten die vordefinierten Additions Operatoren die Zeichen folgen Darstellung der Operanden.

*  Ganzzahlige Addition:

   ```csharp
   int operator +(int x, int y);
   uint operator +(uint x, uint y);
   long operator +(long x, long y);
   ulong operator +(ulong x, ulong y);
   ```

   Wenn sich in einem `checked` Kontext die Summe außerhalb des Bereichs des Ergebnis Typs befindet, wird eine `System.OverflowException` ausgelöst. In einem `unchecked` Kontext werden Überläufe nicht gemeldet, und alle signifikanten höherwertigen Bits außerhalb des Bereichs des Ergebnis Typs werden verworfen.

*  Gleit Komma Addition:

   ```csharp
   float operator +(float x, float y);
   double operator +(double x, double y);
   ```

   Die Summe wird gemäß den Regeln der IEEE 754-Arithmetik berechnet. In der folgenden Tabelle werden die Ergebnisse aller möglichen Kombinationen von nicht NULL Endwerten, Nullen, Infinities und Nan-Werten aufgelistet. In der Tabelle sind `x` und `y` Endwerte ungleich NULL, und `z` das Ergebnis `x + y`ist. Wenn `x` und `y` das gleiche Ausmaß, aber gegen übergesetzte Vorzeichen aufweisen, ist `z` positiv 0 (null). Wenn `x + y` zu groß ist, um im Zieltyp darzustellen, ist `z` unendlich mit demselben Vorzeichen wie `x + y`.

   |      |      |      |      |      |      |      |
   |:----:|:----:|:----:|:----:|:----:|:----:|:----:|
   |      | j    | +0   | -0   | +inf | -inf | NaN  | 
   | t    | z    | t    | t    | +inf | -inf | NaN  | 
   | +0   | j    | +0   | +0   | +inf | -inf | NaN  | 
   | -0   | j    | +0   | -0   | +inf | -inf | NaN  | 
   | +inf | +inf | +inf | +inf | +inf | NaN  | NaN  | 
   | -inf | -inf | -inf | -inf | NaN  | -inf | NaN  | 
   | NaN  | NaN  | NaN  | NaN  | NaN  | NaN  | NaN  | 

*  Dezimal Addition:

   ```csharp
   decimal operator +(decimal x, decimal y);
   ```

   Wenn der resultierende Wert zu groß ist, um im `decimal`-Format dargestellt zu werden, wird eine `System.OverflowException` ausgelöst. Die Dezimalstellen des Ergebnisses, vor der Rundung, sind die größere der Skalen der beiden Operanden.

   Die Dezimal Addition entspricht der Verwendung des Additions Operators vom Typ `System.Decimal`.

*  Enumerationsaddition. Jeder Enumerationstyp stellt implizit die folgenden vordefinierten Operatoren bereit, wobei `E` dem Enumerationstyp entspricht und `U` der zugrunde liegende Typ von `E`ist:

   ```csharp
   E operator +(E x, U y);
   E operator +(U x, E y);
   ```

   Zur Laufzeit werden diese Operatoren genau so ausgewertet, wie `(E)((U)x + (U)y)`.

*  Verkettung von Zeichen folgen:

   ```csharp
   string operator +(string x, string y);
   string operator +(string x, object y);
   string operator +(object x, string y);
   ```

   Diese über Ladungen des binären `+`-Operators führen eine Zeichen folgen Verkettung aus. Wenn ein Operand der Zeichen folgen Verkettung `null`ist, wird eine leere Zeichenfolge ersetzt. Andernfalls wird jedes nicht-Zeichen folgen Argument in seine Zeichen folgen Darstellung konvertiert, indem die virtuelle `ToString` Methode aufgerufen wird, die vom Typ `object`geerbt wurde. Wenn `ToString` `null`zurückgibt, wird eine leere Zeichenfolge ersetzt.

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

   Das Ergebnis des Zeichenfolgenverkettungs-Operators ist eine Zeichenfolge, die aus den Zeichen des linken Operanden besteht, gefolgt von den Zeichen des rechten Operanden. Der Operator für die Zeichen folgen Verkettung gibt nie einen `null` Wert zurück. Eine `System.OutOfMemoryException` kann ausgelöst werden, wenn nicht genügend Arbeitsspeicher zur Verfügung steht, um die resultierende Zeichenfolge zuzuordnen.

*  Delegatkombination. Jeder Delegattyp stellt implizit den folgenden vordefinierten Operator bereit, wobei `D` der Delegattyp ist:

   ```csharp
   D operator +(D x, D y);
   ```

   Der binäre `+` Operator führt eine Delegatkombination aus, wenn beide Operanden einen Delegattyp `D`haben. (Wenn die Operanden unterschiedliche Delegattypen aufweisen, tritt ein Bindungs Zeitfehler auf.) Wenn der erste Operand `null`ist, ist das Ergebnis der Operation der Wert des zweiten Operanden (auch wenn dies ebenfalls `null`ist). Andernfalls, wenn der zweite Operand `null`ist, ist das Ergebnis der Operation der Wert des ersten Operanden. Andernfalls ist das Ergebnis des Vorgangs eine neue Delegatinstanz, die beim Aufrufen den ersten Operanden aufruft und dann den zweiten Operanden aufruft. Beispiele für die Kombination von Delegaten finden Sie [unter Subtraktions Operator](expressions.md#subtraction-operator) und [Delegataufruf](delegates.md#delegate-invocation). Da `System.Delegate` kein Delegattyp ist, ist `operator` `+` nicht dafür definiert.

### <a name="subtraction-operator"></a>Subtraktionsoperator

Bei einem Vorgang der Formular `x - y`wird die binäre Operator Überladungs Auflösung ([binäre Operator Überladungs Auflösung](expressions.md#binary-operator-overload-resolution)) angewendet, um eine bestimmte Operator Implementierung auszuwählen. Die Operanden werden in die Parametertypen des ausgewählten Operators konvertiert, und der Ergebnistyp ist der Rückgabetyp des Operators.

Die vordefinierten Subtraktions Operatoren sind unten aufgeführt. Alle Operatoren subtrahieren `y` von `x`.

*  Ganzzahlige Subtraktion:

   ```csharp
   int operator -(int x, int y);
   uint operator -(uint x, uint y);
   long operator -(long x, long y);
   ulong operator -(ulong x, ulong y);
   ```

   Wenn sich der Unterschied in einem `checked` Kontext außerhalb des Bereichs des Ergebnis Typs befindet, wird eine `System.OverflowException` ausgelöst. In einem `unchecked` Kontext werden Überläufe nicht gemeldet, und alle signifikanten höherwertigen Bits außerhalb des Bereichs des Ergebnis Typs werden verworfen.

*  Gleit Komma Subtraktion:

   ```csharp
   float operator -(float x, float y);
   double operator -(double x, double y);
   ```

   Der Unterschied wird gemäß den Regeln der IEEE 754-Arithmetik berechnet. In der folgenden Tabelle werden die Ergebnisse aller möglichen Kombinationen von nicht NULL Endwerten, Nullen, Infinities und Nane aufgelistet. In der Tabelle sind `x` und `y` Endwerte ungleich NULL, und `z` das Ergebnis `x - y`ist. Wenn `x` und `y` gleich sind, ist `z` positiv 0 (null). Wenn `x - y` zu groß ist, um im Zieltyp darzustellen, ist `z` unendlich mit demselben Vorzeichen wie `x - y`.

   |      |      |      |      |      |      |     |
   |:----:|:----:|:----:|:----:|:----:|:----:|:---:|
   |      | j    | +0   | -0   | +inf | -inf | NaN | 
   | t    | z    | t    | t    | -inf | +inf | NaN | 
   | +0   | -y   | +0   | +0   | -inf | +inf | NaN | 
   | -0   | -y   | -0   | +0   | -inf | +inf | NaN | 
   | +inf | +inf | +inf | +inf | NaN  | +inf | NaN | 
   | -inf | -inf | -inf | -inf | -inf | NaN  | NaN | 
   | NaN  | NaN  | NaN  | NaN  | NaN  | NaN  | NaN | 

*  Dezimale Subtraktion:

   ```csharp
   decimal operator -(decimal x, decimal y);
   ```

   Wenn der resultierende Wert zu groß ist, um im `decimal`-Format dargestellt zu werden, wird eine `System.OverflowException` ausgelöst. Die Dezimalstellen des Ergebnisses, vor der Rundung, sind die größere der Skalen der beiden Operanden.

   Die dezimale Subtraktion entspricht der Verwendung des Subtraktions Operators vom Typ `System.Decimal`.

*  Enumerationssubtraktion. Jeder Enumerationstyp stellt implizit den folgenden vordefinierten Operator bereit, wobei `E` den Enumerationstyp darstellt und `U` der zugrunde liegende Typ von `E`ist:

   ```csharp
   U operator -(E x, E y);
   ```

   Dieser Operator wird genau wie `(U)((U)x - (U)y)`ausgewertet. Anders ausgedrückt berechnet der Operator den Unterschied zwischen den Ordinalwerten von `x` und `y`, und der Ergebnistyp ist der zugrunde liegende Typ der Enumeration.

   ```csharp
   E operator -(E x, U y);
   ```

   Dieser Operator wird genau wie `(E)((U)x - y)`ausgewertet. Mit anderen Worten, der Operator subtrahiert einen Wert vom zugrunde liegenden Typ der Enumeration und gibt einen Wert der-Enumeration aus.

*  Entfernen von Delegaten. Jeder Delegattyp stellt implizit den folgenden vordefinierten Operator bereit, wobei `D` der Delegattyp ist:

   ```csharp
   D operator -(D x, D y);
   ```

   Der binäre `-` Operator führt die Delegatentfernung aus, wenn beide Operanden einen Delegattyp `D`haben. Wenn die Operanden unterschiedliche Delegattypen aufweisen, tritt ein Bindungs Zeitfehler auf. Ist der erste Operand `null`, ist das Ergebnis des Vorgangs `null`. Andernfalls, wenn der zweite Operand `null`ist, ist das Ergebnis der Operation der Wert des ersten Operanden. Andernfalls stellen beide Operanden Aufruf Listen ([Delegatdeklarationen](delegates.md#delegate-declarations)) dar, die über mindestens einen Eintrag verfügen, und das Ergebnis ist eine neue Aufruf Liste, die aus der ersten Operanden Liste besteht, aus der die Einträge des zweiten Operanden entfernt wurden, vorausgesetzt, die Liste des zweiten Operanden ist eine ordnungsgemäße zusammenhängende unter Liste der ersten.     (Um die Übereinstimmung zu ermitteln, werden die entsprechenden Einträge als für den Delegat-Gleichheits Operator (Delegat-[Gleichheits Operatoren](expressions.md#delegate-equality-operators)) verglichen.) Andernfalls ist das Ergebnis der Wert des linken Operanden. Die Listen der Operanden werden im Prozess nicht geändert. Wenn die Liste des zweiten Operanden mit mehreren Unterlisten zusammenhängender Einträge in der Liste der ersten Operanden übereinstimmt, wird die am weitesten rechts gerichtete unter Liste von zusammenhängenden Einträgen entfernt. Sollte durch die Entfernung eine leere Liste entstehen, ist das Ergebnis `null`. Beispiel:

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

Die Operatoren `<<` und `>>` werden verwendet, um Bitverschiebungs Vorgänge auszuführen.

```antlr
shift_expression
    : additive_expression
    | shift_expression '<<' additive_expression
    | shift_expression right_shift additive_expression
    ;
```

Wenn für einen Operanden einer *shift_expression* der Typ der Kompilierzeit `dynamic`ist, wird der Ausdruck dynamisch gebunden ([dynamische Bindung](expressions.md#dynamic-binding)). In diesem Fall wird der Kompilier Zeittyp des Ausdrucks `dynamic`, und die unten beschriebene Auflösung findet zur Laufzeit statt, wobei der Lauf Zeittyp der Operanden verwendet wird, für die der Typ der Kompilierzeit `dynamic`ist.

Bei einem Vorgang im Formular `x << count` oder `x >> count`wird die binäre Operator Überladungs Auflösung ([binäre Operator Überladungs Auflösung](expressions.md#binary-operator-overload-resolution)) angewendet, um eine bestimmte Operator Implementierung auszuwählen. Die Operanden werden in die Parametertypen des ausgewählten Operators konvertiert, und der Ergebnistyp ist der Rückgabetyp des Operators.

Beim Deklarieren eines überladenen Verschiebungs Operators muss der Typ des ersten Operanden immer die Klasse oder Struktur sein, die die Operator Deklaration enthält, und der Typ des zweiten Operanden muss immer `int`sein.

Die vordefinierten Shift-Operatoren sind unten aufgeführt.

*  Nach links verschieben:

   ```csharp
   int operator <<(int x, int count);
   uint operator <<(uint x, int count);
   long operator <<(long x, int count);
   ulong operator <<(ulong x, int count);
   ```

   Der `<<`-Operator verschiebt `x` nach links um eine Anzahl von Bits, die wie unten beschrieben berechnet werden.

   Die höherwertigen Bits außerhalb des Bereichs des Ergebnis Typs `x` werden verworfen, die restlichen Bits werden nach links verschoben, und die unteren leeren Bitpositionen werden auf 0 (null) festgelegt.

*  Nach rechts verschieben:

   ```csharp
   int operator >>(int x, int count);
   uint operator >>(uint x, int count);
   long operator >>(long x, int count);
   ulong operator >>(ulong x, int count);
   ```

   Der `>>`-Operator verschiebt `x` rechts um eine Anzahl von Bits, die wie unten beschrieben berechnet werden.

   Wenn `x` vom Typ "`int`" oder "`long`" ist, werden die nieder wertigen Bits der `x` verworfen, die restlichen Bits werden nach rechts verschoben, und die hochwertigen leeren Bitpositionen werden auf 0 (null) festgelegt, wenn `x` nicht negativ ist, und auf einen Wert festgelegt, wenn `x` negativ ist.

   Wenn `x` vom Typ "`uint`" oder "`ulong`" ist, werden die nieder wertigen Bits der `x` verworfen, die restlichen Bits werden nach rechts verschoben, und die hochwertigen leeren Bitpositionen werden auf 0 (null) festgelegt.

Für die vordefinierten Operatoren wird die Anzahl der zu Verschiebungs Bits wie folgt berechnet:

*  Wenn der Typ der `x` `int` oder `uint`ist, wird die UMSCHALT Anzahl durch die nieder wertigen fünf Bits `count`angegeben. Mit anderen Worten: die UMSCHALT Anzahl wird aus `count & 0x1F`berechnet.
*  Wenn der Typ der `x` `long` oder `ulong`ist, wird die UMSCHALT Anzahl durch die nieder wertigen sechs Bits `count`angegeben. Mit anderen Worten: die UMSCHALT Anzahl wird aus `count & 0x3F`berechnet.

Wenn die resultierende Verschiebungs Anzahl 0 (null) ist, geben die Schiebe Operatoren einfach den Wert `x`zurück.

Verschiebungs Vorgänge verursachen niemals Überläufe und erzeugen dieselben Ergebnisse in `checked` und `unchecked` Kontexten.

Wenn der linke Operand des `>>` Operators einen ganzzahligen Typ mit Vorzeichen hat, führt der Operator eine arithmetische Verschiebung nach rechts aus, bei der der Wert des signifikantesten Bits (das Signier Bit) des Operanden an die übergeordnete leere Bitpositionen weitergegeben wird. Wenn der linke Operand des `>>`-Operators einen ganzzahligen Typ ohne Vorzeichen aufweist, führt der Operator eine logische Schiebe nach rechts aus, bei der die Positionen für die hohe Reihenfolge von leeren Bitpositionen immer auf NULL festgelegt sind. Explizite Umwandlungen können verwendet werden, um den umgekehrten Vorgang von auszuführen, der vom Operanden-Typ abgeleitet wird. Wenn `x` z. b. eine Variable vom Typ `int`ist, führt der Vorgang `unchecked((int)((uint)x >> y))` eine logische Verschiebung rechts von `x`aus.

## <a name="relational-and-type-testing-operators"></a>Relationale und Typtestoperatoren

Die Operatoren `==`, `!=`, `<`, `>`, `<=`, `>=`, `is` und `as` werden als relationale und Typtest Operatoren bezeichnet.

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

Der `is`-Operator wird im [is-Operator](expressions.md#the-is-operator) beschrieben, und der `as`-Operator wird im [as-Operator](expressions.md#the-as-operator)beschrieben.

Die Operatoren `==`, `!=`, `<`, `>`, `<=` und `>=` sind ***Vergleichs Operatoren***.

Wenn ein Operand eines Vergleichs Operators den Kompilier Zeittyp `dynamic`aufweist, wird der Ausdruck dynamisch gebunden ([dynamische Bindung](expressions.md#dynamic-binding)). In diesem Fall wird der Kompilier Zeittyp des Ausdrucks `dynamic`, und die unten beschriebene Auflösung findet zur Laufzeit statt, wobei der Lauf Zeittyp der Operanden verwendet wird, für die der Typ der Kompilierzeit `dynamic`ist.

Bei einem Vorgang der Form `x` *op* -`y`, bei dem *op* ein Vergleichs Operator ist, wird die Überladungs Auflösung ([binäre Operator Überladungs Auflösung](expressions.md#binary-operator-overload-resolution)) angewendet, um eine bestimmte Operator Implementierung auszuwählen. Die Operanden werden in die Parametertypen des ausgewählten Operators konvertiert, und der Ergebnistyp ist der Rückgabetyp des Operators.

Die vordefinierten Vergleichs Operatoren werden in den folgenden Abschnitten beschrieben. Alle vordefinierten Vergleichs Operatoren geben ein Ergebnis des Typs `bool`zurück, wie in der folgenden Tabelle beschrieben.


| __Vorgang__ | __Ergebnis__                                                       |
|---------------|------------------------------------------------------------------|
| `x == y`      | `true`, wenn `x` gleich `y`ist, andernfalls `false`                 | 
| `x != y`      | `true`, wenn `x` nicht gleich `y`ist, andernfalls `false`             | 
| `x < y`       | `true`, wenn `x` kleiner als `y`ist, andernfalls `false`                | 
| `x > y`       | `true`, wenn `x` größer als `y`ist `false` andernfalls             | 
| `x <= y`      | `true`, wenn `x` kleiner oder gleich `y`ist, andernfalls `false`    | 
| `x >= y`      | `true`, wenn `x` größer oder gleich `y`ist, andernfalls `false` | 

### <a name="integer-comparison-operators"></a>Integer-Vergleichs Operatoren

Die vordefinierten ganzzahligen Vergleichs Operatoren lauten wie folgt:
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

Jeder dieser Operatoren vergleicht die numerischen Werte der beiden ganzzahligen Operanden und gibt einen `bool` Wert zurück, der angibt, ob die jeweilige Beziehung `true` oder `false`ist.

### <a name="floating-point-comparison-operators"></a>Vergleichs Operatoren für Gleit Komma Zahlen

Die vordefinierten Gleit Komma Vergleichs Operatoren lauten wie folgt:
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

Die Operanden werden von den Operatoren gemäß den Regeln des IEEE 754-Standards verglichen:

*  Wenn einer der beiden Operanden NaN ist, wird das Ergebnis für alle Operatoren mit Ausnahme von `!=``false`, für die das Ergebnis `true`ist. Bei zwei-Operanden erzeugt `x != y` immer dasselbe Ergebnis wie `!(x == y)`. Wenn jedoch ein oder beide Operanden NaN sind, werden die `<`-, `>`-, `<=`-und `>=`-Operatoren nicht die gleichen Ergebnisse wie die logische Negation des umgekehrten Operators ergeben. Wenn z. b. einer `x` und `y` NaN ist, wird `x < y` `false`, aber `!(x >= y)` `true`ist.
*  Wenn keiner der Operanden NaN ist, vergleichen die Operatoren die Werte der beiden Gleit Komma Operanden in Bezug auf die Reihenfolge.

   ```csharp
   -inf < -max < ... < -min < -0.0 == +0.0 < +min < ... < +max < +inf
   ```

   Dabei sind `min` und `max` die kleinsten und größten positiven Endwerte, die im angegebenen Gleit Komma Format dargestellt werden können. Wichtige Auswirkungen dieser Reihenfolge:
   * Negative und positive Nullen werden als gleich betrachtet.
   * Minus unendlich gilt als kleiner als alle anderen Werte, aber gleichbedeutend mit einem anderen negativen unendlich.
   * Eine positive Unendlichkeit gilt als größer als alle anderen Werte, aber gleichbedeutend mit einer anderen positiven unendlich.

### <a name="decimal-comparison-operators"></a>Dezimale Vergleichs Operatoren

Die vordefinierten dezimalen Vergleichs Operatoren lauten wie folgt:
```csharp
bool operator ==(decimal x, decimal y);
bool operator !=(decimal x, decimal y);
bool operator <(decimal x, decimal y);
bool operator >(decimal x, decimal y);
bool operator <=(decimal x, decimal y);
bool operator >=(decimal x, decimal y);
```

Jeder dieser Operatoren vergleicht die numerischen Werte der beiden Decimal-Operanden und gibt einen `bool` Wert zurück, der angibt, ob die jeweilige Beziehung `true` oder `false`ist. Jeder Dezimal Vergleich entspricht der Verwendung des entsprechenden relationalen or-Gleichheits Operators vom Typ `System.Decimal`.

### <a name="boolean-equality-operators"></a>Boolesche Gleichheits Operatoren

Die vordefinierten booleschen Gleichheits Operatoren lauten wie folgt:
```csharp
bool operator ==(bool x, bool y);
bool operator !=(bool x, bool y);
```

Das Ergebnis `==` ist `true`, wenn sowohl `x` als auch `y` `true` sind oder sowohl `x` als auch `y` `false`sind. Andernfalls ist das Ergebnis `false`.

Das Ergebnis `!=` ist `false`, wenn sowohl `x` als auch `y` `true` sind oder sowohl `x` als auch `y` `false`sind. Andernfalls ist das Ergebnis `true`. Wenn die Operanden vom Typ `bool`sind, erzeugt der `!=` Operator dasselbe Ergebnis wie der `^`-Operator.

### <a name="enumeration-comparison-operators"></a>Enumerationsvergleichs-Operatoren

Jeder Enumerationstyp stellt implizit die folgenden vordefinierten Vergleichs Operatoren bereit:
```csharp
bool operator ==(E x, E y);
bool operator !=(E x, E y);
bool operator <(E x, E y);
bool operator >(E x, E y);
bool operator <=(E x, E y);
bool operator >=(E x, E y);
```

Das Ergebnis der Auswertung von `x op y`, wobei `x` und `y` Ausdrücke eines Enumerationstyps sind `E` mit einem zugrunde liegenden Typ `U`, und `op` ist einer der Vergleichs Operatoren, entspricht dem Auswerten `((U)x) op ((U)y)`. Mit anderen Worten, die Vergleichs Operatoren des Enumerationstyps vergleichen einfach die zugrunde liegenden ganzzahligen Werte der beiden Operanden.

### <a name="reference-type-equality-operators"></a>Verweistyp-Gleichheits Operatoren

Die vordefinierten Verweistyp-Gleichheits Operatoren sind:
```csharp
bool operator ==(object x, object y);
bool operator !=(object x, object y);
```

Die Operatoren geben das Ergebnis des Vergleichs der beiden Verweise auf Gleichheit oder nicht Gleichheit zurück.

Da die vordefinierten Verweistyp-Gleichheits Operatoren Operanden vom Typ `object`akzeptieren, gelten Sie für alle Typen, die keine anwendbaren `operator ==` und `operator !=` Member deklarieren. Im Gegensatz dazu Blenden alle anwendbaren benutzerdefinierten Gleichheits Operatoren die vordefinierten Verweistyp-Gleichheits Operatoren aus.

Die vordefinierten Verweistyp-Gleichheits Operatoren erfordern eine der folgenden:

*  Beide Operanden sind ein Wert eines Typs, der bekanntermaßen als *reference_type* oder Literal`null`bezeichnet wird. Darüber hinaus ist eine explizite Verweis Konvertierung ([explizite Verweis Konvertierungen](conversions.md#explicit-reference-conversions)) vom Typ eines der beiden Operanden bis zum Typ des anderen Operanden vorhanden.
*  Ein Operand ist ein Wert vom Typ `T`, wobei `T` ein *type_parameter* und der andere Operand der Literal`null`ist. Außerdem `T` nicht über die Werttyp Einschränkung verfügen.

Wenn eine dieser Bedingungen nicht zutrifft, tritt ein Fehler bei der Bindung auf. Wichtige Implikationen dieser Regeln sind:

*  Es handelt sich um einen Bindungs Fehler, bei dem die vordefinierten Verweistyp-Gleichheits Operatoren verwendet werden, um zwei Verweise zu vergleichen, die bekanntermaßen bei der Bindungs Zeit unterschiedlich sind. Wenn die Bindungs Zeit Typen der Operanden z. b. zwei Klassentypen sind `A` und `B`, und wenn weder `A` noch `B` von der anderen abgeleitet werden, ist es unmöglich, dass die beiden Operanden auf das gleiche Objekt verweisen. Daher wird der Vorgang als Bindungs Zeit Fehler betrachtet.
*  Die vordefinierten Verweistyp-Gleichheits Operatoren lassen nicht zu, dass Werttyp Operanden verglichen werden. Daher ist es nicht möglich, Werte dieses Struktur Typs zu vergleichen, es sei denn, ein Strukturtyp deklariert seine eigenen Gleichheits Operatoren.
*  Die vordefinierten Verweistyp-Gleichheits Operatoren bewirken nie, dass Boxing-Vorgänge für ihre Operanden ausgeführt werden. Es wäre bedeutungslos, solche Boxing-Vorgänge auszuführen, da Verweise auf die neu zugeordneten geachtelten Instanzen notwendigerweise von allen anderen verweisen abweichen.
*  Wenn ein Operand eines Typparameter Typs `T` mit `null`verglichen wird und der Lauf Zeittyp von `T` ein Werttyp ist, wird das Ergebnis des Vergleichs `false`.

Im folgenden Beispiel wird überprüft, ob ein Argument eines nicht eingeschränkten Typparameter Typs `null`ist.
```csharp
class C<T>
{
    void F(T x) {
        if (x == null) throw new ArgumentNullException();
        ...
    }
}
```

Das `x == null` Konstrukt ist zulässig, auch wenn `T` einen Werttyp darstellen könnte und das Ergebnis einfach so definiert ist, dass es `false` ist, wenn `T` ein Werttyp ist.

Wenn eine beliebige `operator ==` oder `operator !=` vorhanden ist, wird für einen Vorgang der Form `x == y` oder `x != y`der Operator Überladungs Auflösung ([binäre Operator Überladungs Auflösung](expressions.md#binary-operator-overload-resolution)) diesen Operator anstelle des vordefinierten Verweistyp Gleichheits Operators auswählen. Es ist jedoch immer möglich, den vordefinierten Verweistyp Gleichheits Operator auszuwählen, indem eine oder beide der Operanden explizit in den Typ `object`umgewandelt werden. Das Beispiel
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
```console
True
False
False
False
```

Die Variablen `s` und `t` verweisen auf zwei unterschiedliche `string` Instanzen, die die gleichen Zeichen enthalten. Der erste Vergleich gibt `True` aus, da der vordefinierte Zeichen folgen Gleichheits Operator ([Zeichen folgen Gleichheits Operatoren](expressions.md#string-equality-operators)) ausgewählt ist, wenn beide Operanden den Typ `string`haben. Bei den restlichen vergleichen werden alle Ausgabe `False`, da der vordefinierte Verweistyp Gleichheits Operator ausgewählt wird, wenn einer oder beide Operanden den Typ `object`haben.

Beachten Sie, dass die oben beschriebene Technik für Werttypen nicht sinnvoll ist. Das Beispiel
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
gibt `False` aus, da die Umwandlungen Verweise auf zwei separate Instanzen von geachtelten `int` Werten erstellen.

### <a name="string-equality-operators"></a>Operatoren für Zeichen folgen

Die vordefinierten Zeichen folgen-Gleichheits Operatoren sind:
```csharp
bool operator ==(string x, string y);
bool operator !=(string x, string y);
```

Zwei `string` Werte werden als gleich betrachtet, wenn eine der folgenden Punkte zutrifft:

*  Beide Werte sind `null`.
*  Beide Werte sind Verweise ungleich NULL auf Zeichen folgen Instanzen, die identische Längen und identische Zeichen an jeder Zeichenposition aufweisen.

Die Gleichheits Operatoren für Zeichen folgen vergleichen Zeichen folgen Werte anstelle von Zeichen folgen verweisen. Wenn zwei separate Zeichen folgen Instanzen genau dieselbe Zeichenfolge enthalten, sind die Werte der Zeichen folgen gleich, aber die Verweise unterscheiden sich. Wie in [Verweistyp Gleichheits Operatoren](expressions.md#reference-type-equality-operators)beschrieben, können die Verweistyp-Gleichheits Operatoren verwendet werden, um Zeichen folgen Verweise anstelle von Zeichen folgen Werten zu vergleichen.

### <a name="delegate-equality-operators"></a>Delegatenoperatoren

Jeder Delegattyp stellt implizit die folgenden vordefinierten Vergleichs Operatoren bereit:

```csharp
bool operator ==(System.Delegate x, System.Delegate y);
bool operator !=(System.Delegate x, System.Delegate y);
```

Zwei Delegatinstanzen werden wie folgt als gleich betrachtet:

*  Wenn eine der Delegatinstanzen `null`ist, sind Sie nur dann gleich, wenn beide `null`werden.
*  Wenn die Delegaten einen anderen Lauf Zeittyp aufweisen, sind Sie nie gleich.
*  Wenn beide Delegatinstanzen über eine Aufruf Liste ([Delegatdeklarationen](delegates.md#delegate-declarations)) verfügen, sind diese Instanzen nur dann gleich, wenn Ihre Aufruf Listen dieselbe Länge aufweisen und jeder Eintrag in einer Aufruf Liste (wie unten definiert) dem entsprechenden Eintrag in der Reihenfolge in der Aufruf Liste eines anderen entspricht.

Die folgenden Regeln bestimmen die Gleichheit von Aufruf Listeneinträgen:

*  Wenn zwei Aufruf Listeneinträge auf dieselbe statische Methode verweisen, sind die Einträge gleich.
*  Wenn zwei Aufruf Listeneinträge auf dieselbe nicht statische Methode im gleichen Zielobjekt verweisen (wie durch die Verweis Gleichheits Operatoren definiert), sind die Einträge gleich.
*  Aufruf Listeneinträge, die aus der Auswertung semantisch identischer *anonymous_method_expression*s oder *lambda_expression*s mit demselben (möglicherweise leeren) Satz erfasster externer Variablen Instanzen erstellt werden, sind zulässig (aber nicht erforderlich).

### <a name="equality-operators-and-null"></a>Gleichheits Operatoren und NULL

Mit den Operatoren `==` und `!=` kann ein Operand ein Wert eines Typs sein, der NULL-Werte zulässt, und der andere als `null` Literale, auch wenn kein vordefinierter oder benutzerdefinierter Operator für den Vorgang vorhanden ist.

Für einen Vorgang eines der Formulare
```csharp
x == null
null == x
x != null
null != x
```
Wenn `x` ein Ausdruck eines Typs ist, der NULL-Werte zulässt, wenn die Operator Überladungs Auflösung ([binäre Operator Überladungs Auflösung](expressions.md#binary-operator-overload-resolution)) einen anwendbaren Operator nicht finden kann, wird das Ergebnis stattdessen aus der `HasValue`-Eigenschaft von `x`berechnet. Insbesondere werden die ersten beiden Formulare in `!x.HasValue`übersetzt, und die letzten zwei Formen werden in `x.HasValue`übersetzt.

### <a name="the-is-operator"></a>Der is-Operator

Mit dem `is`-Operator wird dynamisch überprüft, ob der Lauf Zeittyp eines Objekts mit einem bestimmten Typ kompatibel ist. Das Ergebnis des Vorgangs `E is T`, bei dem `E` ein Ausdruck und `T` ein Typ ist, ist ein boolescher Wert, der angibt, ob `E` erfolgreich in den Typ konvertiert werden kann, der durch eine Verweis Konvertierung, eine Boxing-Konvertierung oder eine Unboxing-Konvertierung in den Typ `T` konvertiert werden kann. Der Vorgang wird wie folgt ausgewertet, nachdem Typargumente für alle Typparameter ersetzt wurden:

*  Wenn `E` eine anonyme Funktion ist, tritt ein Kompilierzeitfehler auf.
*  Wenn `E` eine Methoden Gruppe oder das `null` Literale ist, wenn der Typ von `E` ein Referenztyp oder ein Werte zulässt-Typ ist und der Wert `E` NULL ist, ist das Ergebnis false.
*  Stellen Sie andernfalls `D` den dynamischen Typ der `E` wie folgt dar:
   * Wenn der `E` Typ ein Verweistyp ist, ist `D` der Lauf Zeittyp des instanzverweises durch `E`.
   * Wenn der Typ `E` ein Typ ist, der NULL-Werte zulässt, ist `D` der zugrunde liegende Typ dieses Typs, der NULL-Werte zulässt.
   * Wenn der Typ `E` ein Werttyp ist, der keine NULL-Werte zulässt, ist `D` der Typ des `E`.
*  Das Ergebnis des Vorgangs hängt von `D` und `T` wie folgt ab:
   * Wenn `T` ein Referenztyp ist, ist das Ergebnis true, wenn `D` und `T` denselben Typ haben, wenn `D` ein Referenztyp und eine implizite Verweis Konvertierung von `D` in `T` vorhanden ist, oder wenn `D` ein Werttyp ist und eine Boxing-Konvertierung von `D` in `T` vorhanden ist.
   * Wenn `T` ein Typ ist, der NULL-Werte zulässt, ist das Ergebnis true, wenn `D` der zugrunde liegende Typ von `T`ist.
   * Wenn `T` ein Werttyp ist, der keine NULL-Werte zulässt, ist das Ergebnis true, wenn `D` und `T` denselben Typ haben.
   * Andernfalls ist das Ergebnis false.

Beachten Sie, dass benutzerdefinierte Konvertierungen vom `is`-Operator nicht berücksichtigt werden.

### <a name="the-as-operator"></a>Der as-Operator

Der `as`-Operator wird verwendet, um explizit einen Wert in einen angegebenen Verweistyp oder Werte zulässt-Typ zu konvertieren. Anders als bei einem Umwandlungs Ausdruck ([Cast-Ausdrücke](expressions.md#cast-expressions)) löst der `as`-Operator nie eine Ausnahme aus. Wenn die angegebene Konvertierung nicht möglich ist, wird stattdessen der resultierende Wert `null`.

Bei einem Vorgang der Form `E as T`muss `E` ein Ausdruck sein, und `T` muss ein Verweistyp sein, ein Typparameter, der als Verweistyp bekannt ist, oder ein Typ, der NULL-Werte zulässt. Außerdem muss mindestens einer der folgenden Punkte zutreffen. andernfalls tritt ein Kompilierzeitfehler auf:

*  Eine Identität ([Identitäts Konvertierung](conversions.md#identity-conversion)), implizite NULL-Werte zulassen ([implizite Konvertierungen](conversions.md#implicit-nullable-conversions), die NULL zulassen), impliziter Verweis ([implizite Verweis Konvertierungen](conversions.md#implicit-reference-conversions)), Boxing ([Boxing-Konvertierungen](conversions.md#boxing-conversions)), explizite NULL-Werte zulassen ([explizite Konvertierungen](conversions.md#explicit-nullable-conversions)`T`auf NULL zulassen), expliziter Verweis ([explizite Verweis Konvertierungen](conversions.md#explicit-reference-conversions)) oder Unboxing-`E` Konvertierung ([Unboxing](conversions.md#unboxing-conversions)-Konvertierung).
*  Der Typ des `E` oder `T` ist ein offener Typ.
*  `E` ist das `null` Literale.

Wenn der Kompilier Zeittyp `E` nicht `dynamic`ist, erzeugt der Vorgang `E as T` dasselbe Ergebnis wie
```csharp
E is T ? (T)(E) : (T)null
```
außer dass `E` nur einmal überprüft wird. Es ist zu erwarten, dass der Compiler `E as T`, dass höchstens eine dynamische Typüberprüfung durchgeführt wird, und nicht auf die beiden dynamischen Typüberprüfungen, die von der obigen Erweiterung impliziert werden.

Wenn der Kompilier Zeittyp `E` `dynamic`ist, wird der `as`-Operator im Gegensatz zum Cast Operator nicht dynamisch gebunden ([dynamische Bindung](expressions.md#dynamic-binding)). Daher ist die Erweiterung in diesem Fall:
```csharp
E is T ? (T)(object)(E) : (T)null
```

Beachten Sie, dass einige Konvertierungen, z. b. benutzerdefinierte Konvertierungen, mit dem `as`-Operator nicht möglich sind und stattdessen mithilfe von Umwandlungs Ausdrücken ausgeführt werden müssen.

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
der Typparameter `T` von `G` ist als Verweistyp bekannt, da er die-Klassen Einschränkung aufweist. Der Typparameter `U` von `H` ist jedoch nicht. Daher ist die Verwendung des `as`-Operators in `H` nicht zulässig.

## <a name="logical-operators"></a>Logische Operatoren

Die Operatoren `&`, `^`und `|` werden als logische Operatoren bezeichnet.

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

Wenn ein Operand eines logischen Operators den Kompilier Zeittyp `dynamic`aufweist, wird der Ausdruck dynamisch gebunden ([dynamische Bindung](expressions.md#dynamic-binding)). In diesem Fall wird der Kompilier Zeittyp des Ausdrucks `dynamic`, und die unten beschriebene Auflösung findet zur Laufzeit statt, wobei der Lauf Zeittyp der Operanden verwendet wird, für die der Typ der Kompilierzeit `dynamic`ist.

Bei einem Vorgang der Form `x op y`, wobei `op` einer der logischen Operatoren ist, wird die Überladungs Auflösung ([binäre Operator Überladungs Auflösung](expressions.md#binary-operator-overload-resolution)) angewendet, um eine bestimmte Operator Implementierung auszuwählen. Die Operanden werden in die Parametertypen des ausgewählten Operators konvertiert, und der Ergebnistyp ist der Rückgabetyp des Operators.

Die vordefinierten logischen Operatoren werden in den folgenden Abschnitten beschrieben.

### <a name="integer-logical-operators"></a>Ganz Zahl logische Operatoren

Die vordefinierten ganzzahligen logischen Operatoren lauten wie folgt:
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

Der `&`-Operator berechnet die bitweise logische `AND` der beiden-Operanden, der `|`-Operator berechnet das bitweise logische `OR` der beiden Operanden, und der `^`-Operator berechnet die bitweise logische exklusive `OR` der beiden-Operanden. Von diesen Vorgängen können keine über Flüsse durchlaufen werden.

### <a name="enumeration-logical-operators"></a>Logische Enumerationsoperatoren

Jeder Enumerationstyp `E` implizit die folgenden vordefinierten logischen Operatoren bereit:

```csharp
E operator &(E x, E y);
E operator |(E x, E y);
E operator ^(E x, E y);
```

Das Ergebnis der Auswertung von `x op y`, wobei `x` und `y` Ausdrücke eines Enumerationstyps sind `E` mit einem zugrunde liegenden Typ `U`, und `op` ist einer der logischen Operatoren, ist mit dem Auswerten von `(E)((U)x op (U)y)`identisch. Mit anderen Worten: die logischen Operatoren des Enumerationstyps führen einfach die logische Operation für den zugrunde liegenden Typ der beiden Operanden aus.

### <a name="boolean-logical-operators"></a>Logische boolesche Operatoren

Die vordefinierten booleschen logischen Operatoren lauten wie folgt:
```csharp
bool operator &(bool x, bool y);
bool operator |(bool x, bool y);
bool operator ^(bool x, bool y);
```

Das Ergebnis von `x & y` ist `true`, wenn sowohl `x` als auch `y` zu `true` ausgewertet werden. Andernfalls ist das Ergebnis `false`.

Das Ergebnis `x | y` ist `true`, wenn entweder `x` oder `y` `true`ist. Andernfalls ist das Ergebnis `false`.

Das Ergebnis `x ^ y` ist `true`, wenn `x` `true` und `y` `false`ist, `x` `false` und `y` `true`ist. Andernfalls ist das Ergebnis `false`. Wenn die Operanden vom Typ `bool`sind, berechnet der `^` Operator dasselbe Ergebnis wie der `!=`-Operator.

### <a name="nullable-boolean-logical-operators"></a>Boolesche logische Operatoren, die NULL-Werte zulassen

Der booleschen-Typ `bool?` der NULL-Werte zulässt, kann drei Werte, `true`, `false`und `null`darstellen und ist konzeptionell vergleichbar mit dem dreiwertigen Typ, der für boolesche Ausdrücke in SQL verwendet wird. Um sicherzustellen, dass die Ergebnisse, die von den `&`-und `|` Operatoren für `bool?`-Operanden generiert werden, mit der dreiwertigen SQL-Logik übereinstimmen, werden die folgenden vordefinierten Operatoren bereitgestellt:

```csharp
bool? operator &(bool? x, bool? y);
bool? operator |(bool? x, bool? y);
```

In der folgenden Tabelle werden die Ergebnisse aufgelistet, die von diesen Operatoren für alle Kombinationen der Werte `true`, `false`und `null`generiert werden.

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

Die Operatoren `&&` und `||` werden als bedingte logische Operatoren bezeichnet. Sie werden auch als "Kurzschluss" logische Operatoren bezeichnet.

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

Die Operatoren "`&&`" und "`||`" sind bedingte Versionen der `&`-und `|`-Operatoren:

*  Der `x && y` Vorgang entspricht dem Vorgang `x & y`, mit dem Unterschied, dass `y` nur ausgewertet wird, wenn `x` nicht `false`ist.
*  Der `x || y` Vorgang entspricht dem Vorgang `x | y`, mit dem Unterschied, dass `y` nur ausgewertet wird, wenn `x` nicht `true`ist.

Wenn ein Operand eines bedingten logischen Operators den Kompilier Zeittyp `dynamic`aufweist, wird der Ausdruck dynamisch gebunden ([dynamische Bindung](expressions.md#dynamic-binding)). In diesem Fall wird der Kompilier Zeittyp des Ausdrucks `dynamic`, und die unten beschriebene Auflösung findet zur Laufzeit statt, wobei der Lauf Zeittyp der Operanden verwendet wird, für die der Typ der Kompilierzeit `dynamic`ist.

Ein Vorgang der Form `x && y` oder `x || y` wird verarbeitet, indem die Überladungs Auflösung ([binäre Operator Überladungs Auflösung](expressions.md#binary-operator-overload-resolution)) angewendet wird, als ob der Vorgang `x & y` oder `x | y`geschrieben worden wäre. Dies ergibt folgende Szenarien:

*  Wenn die Überladungs Auflösung keinen einzelnen optimalen Operator findet oder wenn die Überladungs Auflösung einen der vordefinierten logischen ganzzahligen Operatoren auswählt, tritt ein Bindungs Zeitfehler auf.
*  Andernfalls wird der Vorgang wie in [booleschen bedingten logischen](expressions.md#boolean-conditional-logical-operators)Operatoren beschrieben verarbeitet, wenn der ausgewählte Operator einer der vordefinierten booleschen logischen Operatoren ([boolesche logische Operatoren](expressions.md#boolean-logical-operators)) oder NULL[-Werte zulässt](expressions.md#nullable-boolean-logical-operators).
*  Andernfalls ist der ausgewählte Operator ein benutzerdefinierter Operator, und der Vorgang wird wie unter [Benutzerdefinierte bedingte logische Operatoren](expressions.md#user-defined-conditional-logical-operators)beschrieben verarbeitet.

Es ist nicht möglich, die bedingten logischen Operatoren direkt zu überladen. Da die bedingten logischen Operatoren jedoch in Bezug auf die regulären logischen Operatoren ausgewertet werden, sind über Ladungen der regulären logischen Operatoren mit bestimmten Einschränkungen auch als über Ladungen der bedingten logischen Operatoren zu berücksichtigen. Dies wird weiter unten unter [Benutzerdefinierte bedingte logische Operatoren](expressions.md#user-defined-conditional-logical-operators)beschrieben.

### <a name="boolean-conditional-logical-operators"></a>Boolesche bedingte logische Operatoren

Wenn die Operanden von `&&` oder `||` vom Typ `bool`sind oder wenn es sich bei den Operanden um Typen handelt, die keinen anwendbaren `operator &` oder `operator |`definieren, aber implizite Konvertierungen in `bool`definieren, wird der Vorgang wie folgt verarbeitet:

*  Der Vorgang `x && y` der als `x ? y : false`ausgewertet wird. Das heißt, `x` zuerst ausgewertet und in den Typ `bool`konvertiert. Wenn `x` `true`wird, wird `y` ausgewertet und in den Typ `bool`konvertiert, und dies wird das Ergebnis des Vorgangs. Andernfalls wird das Ergebnis des Vorgangs `false`.
*  Der Vorgang `x || y` der als `x ? true : y`ausgewertet wird. Das heißt, `x` zuerst ausgewertet und in den Typ `bool`konvertiert. Wenn `x` `true`ist, wird das Ergebnis des Vorgangs `true`. Andernfalls wird `y` ausgewertet und in den Typ `bool`konvertiert, und dies wird das Ergebnis des Vorgangs.

### <a name="user-defined-conditional-logical-operators"></a>Benutzerdefinierte bedingte logische Operatoren

Wenn es sich bei den Operanden von `&&` oder `||` um Typen handelt, die einen anwendbaren benutzerdefinierten `operator &` oder `operator |`deklarieren, müssen die beiden folgenden Optionen zutreffen, wobei `T` der Typ ist, in dem der ausgewählte Operator deklariert ist:

*  Der Rückgabetyp und der Typ jedes Parameters des ausgewählten Operators müssen `T`werden. Mit anderen Worten, der Operator muss die logische `AND` oder die logische `OR` zweier Operanden des Typs `T`berechnen und ein Ergebnis des Typs `T`zurückgeben.
*  `T` müssen Deklarationen von `operator true` und `operator false`enthalten.

Ein Fehler bei der Bindungs Zeit tritt auf, wenn eine dieser Anforderungen nicht erfüllt wird. Andernfalls wird der `&&` oder `||` Vorgang ausgewertet, indem die benutzerdefinierten `operator true` oder `operator false` mit dem ausgewählten benutzerdefinierten Operator kombiniert werden:

*  Der Vorgang `x && y` wird als `T.false(x) ? x : T.&(x, y)`ausgewertet, wobei `T.false(x)` ein Aufruf der in `T`deklarierten `operator false` ist, und `T.&(x, y)` ein Aufruf des ausgewählten `operator &`ist. Das heißt, `x` zuerst ausgewertet und `operator false` für das Ergebnis aufgerufen wird, um zu bestimmen, ob `x` definitiv false ist. Wenn `x` definitiv false ist, ist das Ergebnis der Operation der Wert, der zuvor für `x`berechnet wurde. Andernfalls wird `y` ausgewertet, und der ausgewählte `operator &` wird für den Wert aufgerufen, der zuvor für `x` berechnet wurde, und der für `y` berechnete Wert, um das Ergebnis des Vorgangs zu erhalten.
*  Der Vorgang `x || y` wird als `T.true(x) ? x : T.|(x, y)`ausgewertet, wobei `T.true(x)` ein Aufruf der in `T`deklarierten `operator true` ist, und `T.|(x,y)` ein Aufruf des ausgewählten `operator|`ist. Das heißt, `x` zuerst ausgewertet und `operator true` für das Ergebnis aufgerufen wird, um zu bestimmen, ob `x` definitiv true ist. Wenn `x` definitiv true ist, ist das Ergebnis der Operation der Wert, der zuvor für `x`berechnet wurde. Andernfalls wird `y` ausgewertet, und der ausgewählte `operator |` wird für den Wert aufgerufen, der zuvor für `x` berechnet wurde, und der für `y` berechnete Wert, um das Ergebnis des Vorgangs zu erhalten.

Bei beiden Vorgängen wird der von `x` angegebene Ausdruck nur einmal ausgewertet, und der von `y` angegebene Ausdruck wird entweder nicht genau einmal ausgewertet oder ausgewertet.

Ein Beispiel für einen Typ, der `operator true` und `operator false`implementiert, finden Sie unter [Daten Bank boolescher Typ](structs.md#database-boolean-type).

## <a name="the-null-coalescing-operator"></a>Der NULL-Sammel Operator

Der `??`-Operator wird als NULL-Sammel Operator bezeichnet.

```antlr
null_coalescing_expression
    : conditional_or_expression
    | conditional_or_expression '??' null_coalescing_expression
    ;
```

Ein NULL-Sammel Ausdruck der Form `a ?? b` erfordert, dass `a` einen Typ oder Verweistyp vom Typ NULL-Werte zulässt. Wenn `a` ungleich NULL ist, ist das Ergebnis der `a ?? b` `a`. Andernfalls ist das Ergebnis `b`. Der Vorgang wertet `b` nur aus, wenn `a` NULL ist.

Der NULL-Sammel Operator ist rechts assoziativ, was bedeutet, dass Vorgänge von rechts nach Links gruppiert werden. Beispielsweise wird ein Ausdruck der Form `a ?? b ?? c` als `a ?? (b ?? c)`ausgewertet. In der Regel gibt ein Ausdruck der Form `E1 ?? E2 ?? ... ?? En` den ersten der Operanden zurück, der nicht NULL ist, oder NULL, wenn alle Operanden NULL sind.

Der Typ des Ausdrucks `a ?? b` abhängig davon, welche impliziten Konvertierungen für die Operanden verfügbar sind. In der bevorzugten Reihenfolge ist der Typ der `a ?? b` `A0`, `A`oder `B`, wobei `A` der Typ der `a` ist (vorausgesetzt, dass `a` einen Typ aufweist), `B` der Typ des `b` ist (vorausgesetzt, dass `b` einen Typ aufweist), und `A0` ist der zugrunde liegende Typ von `A`, wenn `A` ein Typ ist, der NULL-Werte zulässt, oder andernfalls `A`. `a ?? b` wird wie folgt verarbeitet:

*  Wenn `A` vorhanden und kein Werte zulässt-Typ oder Verweistyp ist, tritt ein Kompilierzeitfehler auf.
*  Wenn `b` ein dynamischer Ausdruck ist, wird der Ergebnistyp `dynamic`. Zur Laufzeit wird `a` zuerst ausgewertet. Wenn `a` nicht NULL ist, wird `a` in Dynamic konvertiert, und dies wird zum Ergebnis. Andernfalls wird `b` ausgewertet und das Ergebnis.
*  Andernfalls ist der Ergebnistyp `A0`, wenn `A` vorhanden ist und ein Typ ist, der NULL-Werte zulässt, und eine implizite Konvertierung von `b` zum `A0`durchzuführen ist. Zur Laufzeit wird `a` zuerst ausgewertet. Wenn `a` nicht NULL ist, wird `a` in den Typ `A0`entpackt, und dies wird zum Ergebnis. Andernfalls wird `b` ausgewertet und in den Typ `A0`konvertiert, und dies wird zum Ergebnis.
*  Andernfalls ist der Ergebnistyp `A`, wenn `A` vorhanden und eine implizite Konvertierung von `b` zu `A`vorhanden ist. Zur Laufzeit wird `a` zuerst ausgewertet. Wenn `a` nicht NULL ist, wird `a` das Ergebnis. Andernfalls wird `b` ausgewertet und in den Typ `A`konvertiert, und dies wird zum Ergebnis.
*  Andernfalls ist der Ergebnistyp `B`, wenn `b` einen-Typ aufweist `B` und eine implizite Konvertierung von `a` in `B`vorhanden ist. Zur Laufzeit wird `a` zuerst ausgewertet. Wenn `a` nicht NULL ist, wird `a` in den Typ `A0` entpackt (wenn `A` vorhanden und NULL-Werte zulässt) und in den Typ `B`konvertiert werden. Dies wird zum Ergebnis. Andernfalls wird `b` ausgewertet und zum Ergebnis.
*  Andernfalls sind `a` und `b` nicht kompatibel, und es tritt ein Kompilierzeitfehler auf.

## <a name="conditional-operator"></a>Bedingter Operator

Der `?:`-Operator wird als Bedingter Operator bezeichnet. Es wird manchmal auch als ternärer Operator bezeichnet.

```antlr
conditional_expression
    : null_coalescing_expression
    | null_coalescing_expression '?' expression ':' expression
    ;
```

Ein bedingter Ausdruck der Form, `b ? x : y` zuerst die Bedingung `b`auswertet. Wenn `b` `true`ist, wird `x` ausgewertet und zum Ergebnis des Vorgangs. Andernfalls wird `y` ausgewertet und zum Ergebnis des Vorgangs. Ein bedingter Ausdruck wertet weder `x` noch `y`aus.

Der bedingte Operator ist rechts assoziativ, was bedeutet, dass Vorgänge von rechts nach Links gruppiert werden. Beispielsweise wird ein Ausdruck der Form `a ? b : c ? d : e` als `a ? b : (c ? d : e)`ausgewertet.

Der erste Operand des `?:` Operators muss ein Ausdruck sein, der implizit in `bool`konvertiert werden kann, oder ein Ausdruck eines Typs, der `operator true`implementiert. Wenn keine dieser Anforderungen erfüllt ist, tritt ein Kompilierzeitfehler auf.

Mit dem zweiten und dritten Operanden, `x` und `y`des `?:` Operators, wird der Typ des bedingten Ausdrucks gesteuert.

*  Wenn `x` den Typ `X` hat und `y` den Typ `Y` hat,
   * Wenn eine implizite Konvertierung ([implizite Konvertierungen](conversions.md#implicit-conversions)) von `X` in `Y`, aber nicht von `Y` zu `X`vorhanden ist, ist `Y` der Typ des bedingten Ausdrucks.
   * Wenn eine implizite Konvertierung ([implizite Konvertierungen](conversions.md#implicit-conversions)) von `Y` in `X`, aber nicht von `X` zu `Y`vorhanden ist, ist `X` der Typ des bedingten Ausdrucks.
   * Andernfalls kann kein Ausdruckstyp ermittelt werden, und es tritt ein Kompilierzeitfehler auf.
*  Wenn nur eine `x` und `y` einen-Typ aufweist und sowohl `x` als auch `y`von implizit in diesen Typ konvertiert werden können, ist dies der Typ des bedingten Ausdrucks.
*  Andernfalls kann kein Ausdruckstyp ermittelt werden, und es tritt ein Kompilierzeitfehler auf.

Die Lauf Zeit Verarbeitung eines bedingten Ausdrucks der Form `b ? x : y` besteht aus den folgenden Schritten:

*  Zuerst wird `b` ausgewertet und der `bool` Wert von `b` bestimmt:
   * Wenn eine implizite Konvertierung vom Typ des `b` in `bool` vorhanden ist, wird diese implizite Konvertierung durchgeführt, um einen `bool` Wert zu erhalten.
   * Andernfalls wird der vom Typ `b` definierte `operator true` aufgerufen, um einen `bool` Wert zu erhalten.
*  Wenn der `bool` Wert, der mit dem obigen Schritt erstellt wurde, `true`ist, wird `x` ausgewertet und in den Typ des bedingten Ausdrucks konvertiert, und dies wird das Ergebnis des bedingten Ausdrucks.
*  Andernfalls wird `y` ausgewertet und in den Typ des bedingten Ausdrucks konvertiert, und dies wird das Ergebnis des bedingten Ausdrucks.

## <a name="anonymous-function-expressions"></a>Anonyme Funktions Ausdrücke

Eine ***anonyme Funktion*** ist ein Ausdruck, der eine "Inline"-Methoden Definition darstellt. Eine anonyme Funktion verfügt nicht über einen Wert oder einen Typ in und von sich selbst, kann jedoch in einen kompatiblen Delegaten oder Ausdrucks Strukturtyp konvertiert werden. Die Auswertung einer anonymen Funktions Konvertierung hängt vom Zieltyp der Konvertierung ab: Wenn es sich um einen Delegattyp handelt, wird die Konvertierung zu einem Delegatwert ausgewertet, der auf die Methode verweist, die von der anonymen Funktion definiert wird. Wenn es sich um einen Ausdrucks bauentyp handelt, wird die Konvertierung zu einer Ausdrucks Baumstruktur ausgewertet, die die Struktur der Methode als Objektstruktur darstellt.

Aus historischen Gründen gibt es zwei syntaktische Varianten von anonymen Funktionen, nämlich *lambda_expression*s und *anonymous_method_expression*s. Für fast alle Zwecke sind *lambda_expression*s präziser und ausdrucksstarker als *anonymous_method_expression*s, die für die Abwärtskompatibilität in der Sprache verbleiben.

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

Eine anonyme Funktion mit dem `async`-Modifizierer ist eine Async-Funktion und befolgt die in [Iteratoren](classes.md#iterators)beschriebenen Regeln.

Die Parameter einer anonymen Funktion in Form eines *lambda_expression* können explizit oder implizit eingegeben werden. In einer explizit typisierten Parameterliste wird der Typ jedes Parameters explizit angegeben. In einer implizit typisierten Parameterliste werden die Parametertypen aus dem Kontext abgeleitet, in dem die anonyme Funktion auftritt – insbesondere wenn die anonyme Funktion in einen kompatiblen Delegattyp oder Ausdrucks Strukturtyp konvertiert wird, stellt dieser Typ die Parametertypen ([Anonyme Funktions Konvertierungen](conversions.md#anonymous-function-conversions)) bereit.

In einer anonymen Funktion mit einem einzelnen, implizit typisierten Parameter können die Klammern in der Parameterliste weggelassen werden. Anders ausgedrückt: eine anonyme Funktion der Form
```csharp
( param ) => expr
```
kann abgekürzt werden zu
```csharp
param => expr
```

Die Parameterliste einer anonymen Funktion in Form einer *anonymous_method_expression* ist optional. Wenn angegeben, müssen die Parameter explizit typisiert werden. Andernfalls kann die anonyme Funktion in einen Delegaten mit einer beliebigen Parameterliste konvertiert werden, die keine `out` Parameter enthält.

Ein *Block* Körper einer anonymen Funktion ist erreichbar ([Endpunkte und Erreichbarkeit](statements.md#end-points-and-reachability)), es sei denn, die anonyme Funktion tritt innerhalb einer nicht erreichbaren Anweisung auf.

Im folgenden finden Sie einige Beispiele für anonyme Funktionen:

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

Das Verhalten von *lambda_expression*s und *anonymous_method_expression*s ist mit Ausnahme der folgenden Punkte identisch:

*  *anonymous_method_expression*s erlauben, dass die Parameterliste vollständig ausgelassen wird, sodass die Konvertierungs Möglichkeit zum Delegieren von Typen von Wert Parametern ermöglicht wird.
*  *lambda_expression*s zulassen, dass Parametertypen ausgelassen und abgeleitet werden, während *anonymous_method_expression*s Parametertypen explizit angeben müssen.
*  Der Text eines *lambda_expression* kann ein Ausdruck oder ein Anweisungsblock sein, während der Text eines *anonymous_method_expression* ein Anweisungsblock sein muss.
*  Nur *lambda_expression*s verfügen über Konvertierungen in kompatible Ausdrucks Baumstruktur Typen ([Ausdrucks Baumstruktur Typen](types.md#expression-tree-types)).

### <a name="anonymous-function-signatures"></a>Anonyme Funktions Signaturen

Der optionale *anonymous_function_signature* einer anonymen Funktion definiert die Namen und optional die Typen der formalen Parameter für die anonyme Funktion. Der Gültigkeitsbereich der Parameter der anonymen Funktion ist der *anonymous_function_body*. ([Bereiche](basic-concepts.md#scopes)) In Verbindung mit der Parameterliste (falls angegeben) bildet der anonyme Methoden Text einen Deklarations Raum ([Deklarationen](basic-concepts.md#declarations)). Daher ist es ein Kompilierzeitfehler, wenn der Name eines Parameters der anonymen Funktion mit dem Namen einer lokalen Variablen, lokalen Konstante oder eines Parameters identisch ist, deren Bereich die *anonymous_method_expression* oder *lambda_expression*enthält.

Wenn eine anonyme Funktion über eine *explicit_anonymous_function_signature*verfügt, ist der Satz kompatibler Delegattypen und Ausdrucks Baum Typen auf diejenigen beschränkt, die dieselben Parametertypen und Modifizierer in der gleichen Reihenfolge aufweisen. Im Gegensatz zu Methoden Gruppen Konvertierungen ([Methoden Gruppen Konvertierungen](conversions.md#method-group-conversions)) wird die kontra Varianz anonymer Funktionsparameter Typen nicht unterstützt. Wenn eine anonyme Funktion keine *anonymous_function_signature*hat, ist der Satz kompatibler Delegattypen und Ausdrucks Baum Typen auf diejenigen beschränkt, die keine `out` Parameter haben.

Beachten Sie, dass ein *anonymous_function_signature* keine Attribute oder ein Parameter Array enthalten kann. Dennoch kann ein *anonymous_function_signature* mit einem Delegattyp kompatibel sein, dessen Parameterliste ein Parameter Array enthält.

Beachten Sie auch, dass bei der Konvertierung in einen Ausdrucks bauentyp, auch wenn diese kompatibel ist, während der Kompilierzeit ([Ausdrucks Baumstruktur Typen](types.md#expression-tree-types)) weiterhin Fehler auftreten können.

### <a name="anonymous-function-bodies"></a>Anonyme Funktions Texte

Der Text (*Ausdruck* oder *Block*) einer anonymen Funktion unterliegt den folgenden Regeln:

*  Wenn die anonyme Funktion eine Signatur enthält, sind die in der Signatur angegebenen Parameter im Textkörper verfügbar. Wenn die anonyme Funktion keine Signatur aufweist, kann Sie in einen Delegattyp oder einen Ausdruckstyp mit Parametern ([Anonyme Funktions Konvertierungen](conversions.md#anonymous-function-conversions)) konvertiert werden, aber auf die Parameter kann im Text nicht zugegriffen werden.
*  Mit Ausnahme von `ref` oder `out` Parametern, die in der Signatur (sofern vorhanden) der nächstliegenden einschließenden anonymen Funktion angegeben sind, handelt es sich um einen Kompilierzeitfehler, damit der Text auf einen `ref` oder `out` Parameter zugreifen kann.
*  Wenn der Typ der `this` ein Strukturtyp ist, ist dies ein Kompilierzeitfehler, damit der Text auf `this`zugreifen kann. Dies gilt unabhängig davon, ob der Zugriff explizit (wie in `this.x`) oder implizit ist (wie in `x`, bei dem `x` ein Instanzmember der Struktur ist). Diese Regel verhindert einen solchen Zugriff und wirkt sich nicht darauf aus, ob die Member-Suche zu einem Member der Struktur führt.
*  Der Text hat Zugriff auf die äußeren Variablen ([äußere Variablen](expressions.md#outer-variables)) der anonymen Funktion. Der Zugriff auf eine äußere Variable verweist auf die Instanz der Variablen, die zum Zeitpunkt der Auswertung der *lambda_expression* oder *anonymous_method_expression* ([Auswertung anonymer Funktions Ausdrücke](expressions.md#evaluation-of-anonymous-function-expressions)) aktiv ist.
*  Es ist ein Kompilierzeitfehler, wenn der Text eine `goto` Anweisung, eine `break` Anweisung oder eine `continue`-Anweisung enthält, deren Ziel außerhalb des Texts oder innerhalb des Texts einer enthaltenen anonymen Funktion liegt.
*  Eine `return`-Anweisung im Text gibt die Steuerung von einem Aufruf der nächstgelegenen anonymen Funktion zurück, nicht vom einschließenden Funktionsmember. Ein Ausdruck, der in einer `return`-Anweisung angegeben ist, muss implizit in den Rückgabetyp des Delegattyps oder Ausdrucks Struktur Typs konvertiert werden, in den die nächstgelegene, einschließende *lambda_expression* oder *anonymous_method_expression* konvertiert werden ([Anonyme Funktions Konvertierungen](conversions.md#anonymous-function-conversions)).

Es ist explizit nicht angegeben, ob es eine Möglichkeit gibt, den Block einer anonymen Funktion auszuführen, außer durch Auswertung und Aufruf der *lambda_expression* oder *anonymous_method_expression*. Insbesondere kann der Compiler eine anonyme Funktion durch das Zusammenführen einer oder mehrerer benannter Methoden oder Typen implementieren. Die Namen dieser Elemente mit synthetischer kompilarverwendung müssen ein für die Compilerverwendung reserviertes Formular sein.

### <a name="overload-resolution-and-anonymous-functions"></a>Überladungs Auflösung und Anonyme Funktionen

Anonyme Funktionen in einer Argumentliste sind an der Typrückschluss-und Überladungs Auflösung beteiligt. Die genauen Regeln finden Sie unter [Typrückschluss](expressions.md#type-inference) und [Überladungs Auflösung](expressions.md#overload-resolution) .

Das folgende Beispiel veranschaulicht die Auswirkung anonymer Funktionen auf die Überladungs Auflösung.

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

Die `ItemList<T>`-Klasse verfügt über zwei `Sum`-Methoden. Jede übernimmt ein `selector` Argument, das den Wert aus einem Listenelement in Summen extrahiert. Der extrahierte Wert kann entweder ein `int` oder ein `double` sein, und die resultierende Summe ist gleichermaßen entweder eine `int` oder eine `double`.

Die `Sum`-Methoden könnten beispielsweise verwendet werden, um Summen aus einer Liste von Detail Zeilen in einer Reihenfolge zu berechnen.

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

Beim ersten Aufruf von `orderDetails.Sum`sind beide `Sum` Methoden anwendbar, da die anonyme Funktion `d => d. UnitCount` mit `Func<Detail,int>` und `Func<Detail,double>`kompatibel ist. Die Überladungs Auflösung wählt jedoch die erste `Sum` Methode aus, da die Konvertierung in `Func<Detail,int>` besser als die Konvertierung in `Func<Detail,double>`ist.

Beim zweiten Aufruf von `orderDetails.Sum`ist nur die zweite `Sum` Methode anwendbar, da die anonyme Funktion `d => d.UnitPrice * d.UnitCount` einen Wert vom Typ `double`erzeugt. Daher wählt die Überladungs Auflösung die zweite `Sum` Methode für diesen Aufruf aus.

### <a name="anonymous-functions-and-dynamic-binding"></a>Anonyme Funktionen und dynamische Bindung

Eine anonyme Funktion kann kein Empfänger, Argument oder Operand eines dynamisch gebundenen Vorgangs sein.

### <a name="outer-variables"></a>Äußere Variablen

Alle lokalen Variablen, Wert Parameter oder Parameter Arrays, deren Bereich die *lambda_expression* oder *anonymous_method_expression* enthält, werden als ***äußere Variable*** der anonymen Funktion bezeichnet. In einem Instanzfunktionsmember einer Klasse wird der `this` Wert als Wert Parameter angesehen und ist eine äußere Variable einer beliebigen anonymen Funktion, die im Funktionsmember enthalten ist.

#### <a name="captured-outer-variables"></a>Erfasste äußere Variablen

Wenn auf eine äußere Variable durch eine anonyme Funktion verwiesen wird, wird die äußere Variable als von der anonymen Funktion ***aufgezeichnet*** . Normalerweise ist die Lebensdauer einer lokalen Variable auf die Ausführung des Blocks oder der Anweisung beschränkt, mit der Sie verknüpft ist ([lokale Variablen](variables.md#local-variables)). Die Lebensdauer einer erfassten äußeren Variable wird jedoch mindestens so lange verlängert, bis der Delegat oder die Ausdrucks Struktur, der aus der anonymen Funktion erstellt wurde, für Garbage Collection qualifiziert wird.

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
die `x` der lokalen Variablen wird von der anonymen Funktion aufgezeichnet, und die Lebensdauer von `x` wird mindestens so lange verlängert, bis der von `F` zurückgegebene Delegat für Garbage Collection infrage kommt (was bis zum Ende des Programms nicht erfolgt). Da jeder Aufruf der anonymen Funktion auf derselben Instanz von `x`ausgeführt wird, lautet die Ausgabe des Beispiels wie folgt:
```console
1
2
3
```

Wenn eine lokale Variable oder ein value-Parameter von einer anonymen Funktion aufgezeichnet wird, wird die lokale Variable oder der Parameter nicht mehr als eine festgelegte Variable ([fest-und verschiebbare Variablen](unsafe-code.md#fixed-and-moveable-variables)) betrachtet, sondern als eine verschiebbare Variable angesehen. Daher muss jeder `unsafe` Code, der die Adresse einer erfassten äußeren Variablen annimmt, zuerst die `fixed`-Anweisung verwenden, um die Variable zu korrigieren.

Beachten Sie, dass im Gegensatz zu einer nicht erfassten Variablen eine aufgezeichnete lokale Variable gleichzeitig für mehrere Ausführungs Threads verfügbar gemacht werden kann.

#### <a name="instantiation-of-local-variables"></a>Instanziierung von lokalen Variablen

Eine lokale Variable wird als ***instanziiert*** betrachtet, wenn die Ausführung in den Gültigkeitsbereich der Variablen eintritt. Wenn z. b. die folgende Methode aufgerufen wird, wird die lokale Variable `x` dreimal instanziiert und initialisiert – einmal für jede Iterationen der Schleife.

```csharp
static void F() {
    for (int i = 0; i < 3; i++) {
        int x = i * 2 + 1;
        ...
    }
}
```

Das Verschieben der Deklaration von `x` außerhalb der Schleife führt jedoch zu einer einzelnen Instanziierung von `x`:
```csharp
static void F() {
    int x;
    for (int i = 0; i < 3; i++) {
        x = i * 2 + 1;
        ...
    }
}
```

Bei nicht Erfassung kann nicht genau beachtet werden, wie oft eine lokale Variable instanziiert wird – da die Lebensdauer der Instanziierungen disjunkt ist, kann jede Instanziierung einfach denselben Speicherort verwenden. Wenn eine anonyme Funktion jedoch eine lokale Variable erfasst, werden die Auswirkungen der Instanziierung offensichtlich.

Das Beispiel
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
```console
1
3
5
```

Wenn jedoch die Deklaration von `x` außerhalb der Schleife verschoben wird:
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
sieht die Ausgabe folgendermaßen aus:
```console
5
5
5
```

Wenn eine for-Schleife eine Iterations Variable deklariert, wird die Variable selbst als außerhalb der Schleife deklariert. Wenn das Beispiel so geändert wird, dass die Iterations Variable selbst aufgezeichnet wird:

```csharp
static D[] F() {
    D[] result = new D[3];
    for (int i = 0; i < 3; i++) {
        result[i] = () => { Console.WriteLine(i); };
    }
    return result;
}
```
Es wird nur eine Instanz der Iterations Variablen aufgezeichnet, die die Ausgabe erzeugt:
```console
3
3
3
```

Es ist möglich, dass anonyme Funktions Delegaten einige erfasste Variablen freigeben, aber über separate Instanzen anderer Instanzen verfügen. Wenn `F` z. b. in geändert wird.
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
die drei Delegaten erfassen dieselbe Instanz von `x` aber separate Instanzen von `y`, und die Ausgabe lautet:
```console
1 1
2 1
3 1
```

Separate anonyme Funktionen können dieselbe Instanz einer äußeren Variablen erfassen. Im folgenden Beispiel
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
die beiden anonymen Funktionen erfassen dieselbe Instanz der lokalen Variablen `x`und können dadurch über diese Variable kommunizieren. Die Ausgabe des Beispiels lautet wie folgt:
```console
5
10
```

### <a name="evaluation-of-anonymous-function-expressions"></a>Auswertung anonymer Funktions Ausdrücke

Eine anonyme Funktion `F` muss immer in einen Delegattyp konvertiert werden `D` oder ein Ausdrucks bauentyp `E`, entweder direkt oder durch die Ausführung eines Ausdrucks `new D(F)`der Delegaterstellung. Diese Konvertierung bestimmt das Ergebnis der anonymen Funktion, wie in [Anonyme Funktions Konvertierungen](conversions.md#anonymous-function-conversions)beschrieben.

## <a name="query-expressions"></a>Abfrageausdrücke

***Abfrage Ausdrücke*** bieten eine sprach integrierte Syntax für Abfragen, die relationalen und hierarchischen Abfrage Sprachen wie SQL und XQuery ähneln.

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

Ein Abfrage Ausdruck beginnt mit einer `from`-Klausel und endet entweder mit einer `select` oder `group`-Klausel. Auf die anfängliche `from`-Klausel können NULL oder mehr `from`-, `let`-, `where`-, `join`-oder `orderby`-Klauseln folgen. Jede `from`-Klausel ist ein Generator, der eine ***Bereichs Variable*** einführt, die sich auf die Elemente einer ***Sequenz***erstreckt. Jede `let`-Klausel führt eine Bereichs Variable ein, die einen Wert darstellt, der mithilfe vorheriger Bereichs Variablen berechnet wurde. Jede `where`-Klausel ist ein Filter, der Elemente aus dem Ergebnis ausschließt. Jede `join`-Klausel vergleicht die angegebenen Schlüssel der Quell Sequenz mit Schlüsseln einer anderen Sequenz und gibt passende Paare aus. Jede `orderby`-Klausel ordnet Elemente gemäß den angegebenen Kriterien neu an. Die abschließende `select` oder `group`-Klausel gibt die Form des Ergebnisses in Bezug auf die Bereichs Variablen an. Zum Schluss kann eine `into`-Klausel verwendet werden, um Abfragen zu "Splice" zu verwenden, indem die Ergebnisse einer Abfrage als Generator in einer nachfolgenden Abfrage behandelt werden.

### <a name="ambiguities-in-query-expressions"></a>Mehrdeutigkeiten in Abfrage Ausdrücken

Abfrage Ausdrücke enthalten eine Reihe von "Kontext Schlüsselwörtern", d. h. Bezeichner, die in einem bestimmten Kontext eine besondere Bedeutung haben. Dabei handelt es sich insbesondere um `from`, `where`, `join`, `on`, `equals`, `into`, `let`, `orderby`, `ascending`, `descending`, `select`, `group` und `by`. Um Mehrdeutigkeiten in Abfrage Ausdrücken zu vermeiden, die durch die gemischte Verwendung dieser Bezeichner als Schlüsselwörter oder einfache Namen verursacht werden, werden diese Bezeichner als Schlüsselwörter betrachtet, wenn Sie an einer beliebigen Stelle innerhalb eines Abfrage Ausdrucks auftreten.

Zu diesem Zweck ist ein Abfrage Ausdruck ein beliebiger Ausdruck, der mit "`from identifier`" beginnt, gefolgt von einem beliebigen Token mit Ausnahme von "`;`", "`=`" oder "`,`".

Damit diese Wörter als Bezeichner in einem Abfrage Ausdruck verwendet werden können, kann "`@`[" (](lexical-structure.md#identifiers)Bezeichner) als Präfix verwendet werden.

### <a name="query-expression-translation"></a>Abfrage Ausdrucks Übersetzung

Die C# Sprache gibt nicht die Ausführungs Semantik von Abfrage Ausdrücken an. Stattdessen werden Abfrage Ausdrücke in Aufrufe von Methoden übersetzt, die dem *Abfrage Ausdrucksmuster* ([dem Abfrage Ausdrucksmuster](expressions.md#the-query-expression-pattern)) entsprechen. Abfrage Ausdrücke werden insbesondere in Aufrufe von Methoden mit den Namen "`Where`", "`Select`", "`SelectMany`", "`Join`", "`GroupJoin`", "`OrderBy`", "`OrderByDescending`", "`ThenBy`", "`ThenByDescending`" Diese Methoden verfügen über bestimmte Signaturen und Ergebnistypen, wie im [Abfrage Ausdrucksmuster](expressions.md#the-query-expression-pattern)beschrieben. Diese Methoden können Instanzmethoden des Objekts sein, das abgefragt wird, oder Erweiterungs Methoden, die sich außerhalb des Objekts befinden, und Sie implementieren die tatsächliche Ausführung der Abfrage.

Die Übersetzung von Abfrage Ausdrücken in Methodenaufrufe ist eine syntaktische Zuordnung, die vor der Durchführung einer Typbindung oder Überladungs Auflösung auftritt. Es ist garantiert, dass die Übersetzung syntaktisch korrekt ist, aber es wird nicht garantiert, dass C# semantisch korrekter Code erzeugt wird. Nach der Übersetzung von Abfrage Ausdrücken werden die resultierenden Methodenaufrufe als reguläre Methodenaufrufe verarbeitet. Dies kann wiederum zu Fehlern führen, z. b. wenn die Methoden nicht vorhanden sind, wenn Argumente falsche Typen aufweisen oder wenn die Methoden generisch sind. der Typrückschluss schlägt fehl.

Ein Abfrage Ausdruck wird durch wiederholtes Anwenden der folgenden Übersetzungen verarbeitet, bis keine weiteren Reduzierungen möglich sind. Die Übersetzungen werden in der Reihenfolge der Anwendung aufgelistet: in jedem Abschnitt wird davon ausgegangen, dass die Übersetzungen in den vorangehenden Abschnitten umfassend ausgeführt wurden, und sobald Sie aufgebraucht sind, wird ein Abschnitt bei der Verarbeitung desselben Abfrage Ausdrucks nicht mehr untersucht.

Die Zuweisung zu Bereichs Variablen ist in Abfrage Ausdrücken nicht zulässig. Eine C# -Implementierung ist jedoch berechtigt, diese Einschränkung nicht immer zu erzwingen, da dies möglicherweise manchmal nicht mit dem hier dargestellten syntaktische Translation-Schema möglich ist.

Bestimmte Übersetzungen fügen Bereichs Variablen mit transparenten Bezeichner ein, die durch `*`gekennzeichnet sind. Die besonderen Eigenschaften von transparenten bezeichmern werden in [transparenten bezeichmern](expressions.md#transparent-identifiers)weiter erläutert.

#### <a name="select-and-groupby-clauses-with-continuations"></a>SELECT-und GroupBy-Klauseln mit Fortsetzungen

Ein Abfrage Ausdruck mit einer Fortsetzung
```csharp
from ... into x ...
```
wird übersetzt in
```csharp
from x in ( from ... ) ...
```

Bei den Übersetzungen in den folgenden Abschnitten wird davon ausgegangen, dass für Abfragen keine `into` Fortsetzungen vorhanden sind.

Das Beispiel
```csharp
from c in customers
group c by c.Country into g
select new { Country = g.Key, CustCount = g.Count() }
```
wird übersetzt in
```csharp
from g in
    from c in customers
    group c by c.Country
select new { Country = g.Key, CustCount = g.Count() }
```
die letzte Übersetzung, von der
```csharp
customers.
GroupBy(c => c.Country).
Select(g => new { Country = g.Key, CustCount = g.Count() })
```

#### <a name="explicit-range-variable-types"></a>Explizite Bereichs Variablen Typen

Eine `from`-Klausel, die explizit einen Bereichs Variablentyp angibt.
```csharp
from T x in e
```
wird übersetzt in
```csharp
from x in ( e ) . Cast < T > ( )
```

Eine `join`-Klausel, die explizit einen Bereichs Variablentyp angibt.
```csharp
join T x in e on k1 equals k2
```
wird übersetzt in
```csharp
join x in ( e ) . Cast < T > ( ) on k1 equals k2
```

Bei den Übersetzungen in den folgenden Abschnitten wird davon ausgegangen, dass Abfragen keine expliziten Bereichs Variablen Typen aufweisen.

Das Beispiel
```csharp
from Customer c in customers
where c.City == "London"
select c
```
wird übersetzt in
```csharp
from c in customers.Cast<Customer>()
where c.City == "London"
select c
```
die letzte Übersetzung, von der
```csharp
customers.
Cast<Customer>().
Where(c => c.City == "London")
```

Explizite Bereichs Variablen Typen sind nützlich für das Abfragen von Auflistungen, die die nicht generische `IEnumerable`-Schnittstelle implementieren, aber nicht die generische `IEnumerable<T>` Schnittstelle. Im obigen Beispiel wäre dies der Fall, wenn `customers` vom Typ `ArrayList`wären.

#### <a name="degenerate-query-expressions"></a>Degenerierte Abfrage Ausdrücke

Ein Abfrage Ausdruck der Form
```csharp
from x in e select x
```
wird übersetzt in
```csharp
( e ) . Select ( x => x )
```

Das Beispiel
```csharp
from c in customers
select c
```
wird übersetzt in
```csharp
customers.Select(c => c)
```

Ein degenerierter Abfrage Ausdruck ist ein Ausdruck, der die Elemente der Quelle trivial auswählt. In einer späteren Phase der Übersetzung werden degenerierte Abfragen entfernt, die durch andere Übersetzungsschritte eingeführt wurden, indem Sie durch ihre Quelle ersetzt werden. Es ist jedoch wichtig zu gewährleisten, dass das Ergebnis eines Abfrage Ausdrucks nie das Quell Objekt selbst ist, da dadurch der Typ und die Identität der Quelle für den Client der Abfrage offengelegt werden. Daher schützt dieser Schritt degenerierte Abfragen, die direkt im Quellcode geschrieben wurden, indem `Select` auf der Quelle explizit aufgerufen wird. Dann werden die Implementierer von `Select` und anderen Abfrage Operatoren übernommen, um sicherzustellen, dass diese Methoden nie das Quell Objekt selbst zurückgeben.

#### <a name="from-let-where-join-and-orderby-clauses"></a>From-, Let-, WHERE-, Join-und OrderBy-Klauseln

Ein Abfrage Ausdruck mit einer zweiten `from`-Klausel, gefolgt von einer `select`-Klausel.
```csharp
from x1 in e1
from x2 in e2
select v
```
wird übersetzt in
```csharp
( e1 ) . SelectMany( x1 => e2 , ( x1 , x2 ) => v )
```

Ein Abfrage Ausdruck mit einer zweiten `from`-Klausel, gefolgt von einem anderen als einer `select`-Klausel:

```csharp
from x1 in e1
from x2 in e2
...
```
wird übersetzt in
```csharp
from * in ( e1 ) . SelectMany( x1 => e2 , ( x1 , x2 ) => new { x1 , x2 } )
...
```

Einen Abfrage Ausdruck mit einer `let`-Klausel
```csharp
from x in e
let y = f
...
```
wird übersetzt in
```csharp
from * in ( e ) . Select ( x => new { x , y = f } )
...
```

Einen Abfrage Ausdruck mit einer `where`-Klausel
```csharp
from x in e
where f
...
```
wird übersetzt in
```csharp
from x in ( e ) . Where ( x => f )
...
```

Ein Abfrage Ausdruck mit einer `join`-Klausel ohne `into` gefolgt von einer `select`-Klausel.
```csharp
from x1 in e1
join x2 in e2 on k1 equals k2
select v
```
wird übersetzt in
```csharp
( e1 ) . Join( e2 , x1 => k1 , x2 => k2 , ( x1 , x2 ) => v )
```

Ein Abfrage Ausdruck mit einer `join`-Klausel ohne `into` gefolgt von einer `select`-Klausel.
```csharp
from x1 in e1
join x2 in e2 on k1 equals k2
...
```
wird übersetzt in
```csharp
from * in ( e1 ) . Join( e2 , x1 => k1 , x2 => k2 , ( x1 , x2 ) => new { x1 , x2 })
...
```

Ein Abfrage Ausdruck mit einer `join`-Klausel mit einer `into` gefolgt von einer `select`-Klausel.
```csharp
from x1 in e1
join x2 in e2 on k1 equals k2 into g
select v
```
wird übersetzt in
```csharp
( e1 ) . GroupJoin( e2 , x1 => k1 , x2 => k2 , ( x1 , g ) => v )
```

Ein Abfrage Ausdruck mit einer `join`-Klausel mit einem `into` gefolgt von einem anderen als einer `select`-Klausel.
```csharp
from x1 in e1
join x2 in e2 on k1 equals k2 into g
...
```
wird übersetzt in
```csharp
from * in ( e1 ) . GroupJoin( e2 , x1 => k1 , x2 => k2 , ( x1 , g ) => new { x1 , g })
...
```

Einen Abfrage Ausdruck mit einer `orderby`-Klausel
```csharp
from x in e
orderby k1 , k2 , ..., kn
...
```
wird übersetzt in
```csharp
from x in ( e ) . 
OrderBy ( x => k1 ) . 
ThenBy ( x => k2 ) .
... .
ThenBy ( x => kn )
...
```

Wenn eine Sortier Klausel einen `descending` Richtungsindikator angibt, wird stattdessen ein Aufruf von `OrderByDescending` oder `ThenByDescending` erzeugt.

Bei den folgenden Übersetzungen wird davon ausgegangen, dass es keine `let`-, `where`-, `join`-oder `orderby`-Klauseln und nicht mehr als eine anfängliche `from`-Klausel in jedem Abfrage Ausdruck gibt.

Das Beispiel
```csharp
from c in customers
from o in c.Orders
select new { c.Name, o.OrderID, o.Total }
```
wird übersetzt in
```csharp
customers.
SelectMany(c => c.Orders,
     (c,o) => new { c.Name, o.OrderID, o.Total }
)
```

Das Beispiel
```csharp
from c in customers
from o in c.Orders
orderby o.Total descending
select new { c.Name, o.OrderID, o.Total }
```
wird übersetzt in
```csharp
from * in customers.
    SelectMany(c => c.Orders, (c,o) => new { c, o })
orderby o.Total descending
select new { c.Name, o.OrderID, o.Total }
```
die letzte Übersetzung, von der
```csharp
customers.
SelectMany(c => c.Orders, (c,o) => new { c, o }).
OrderByDescending(x => x.o.Total).
Select(x => new { x.c.Name, x.o.OrderID, x.o.Total })
```
Dabei ist `x` ein vom Compiler generierter Bezeichner, der andernfalls unsichtbar und nicht zugänglich ist.

Das Beispiel
```csharp
from o in orders
let t = o.Details.Sum(d => d.UnitPrice * d.Quantity)
where t >= 1000
select new { o.OrderID, Total = t }
```
wird übersetzt in
```csharp
from * in orders.
    Select(o => new { o, t = o.Details.Sum(d => d.UnitPrice * d.Quantity) })
where t >= 1000 
select new { o.OrderID, Total = t }
```
die letzte Übersetzung, von der
```csharp
orders.
Select(o => new { o, t = o.Details.Sum(d => d.UnitPrice * d.Quantity) }).
Where(x => x.t >= 1000).
Select(x => new { x.o.OrderID, Total = x.t })
```
Dabei ist `x` ein vom Compiler generierter Bezeichner, der andernfalls unsichtbar und nicht zugänglich ist.

Das Beispiel
```csharp
from c in customers
join o in orders on c.CustomerID equals o.CustomerID
select new { c.Name, o.OrderDate, o.Total }
```
wird übersetzt in
```csharp
customers.Join(orders, c => c.CustomerID, o => o.CustomerID,
    (c, o) => new { c.Name, o.OrderDate, o.Total })
```

Das Beispiel
```csharp
from c in customers
join o in orders on c.CustomerID equals o.CustomerID into co
let n = co.Count()
where n >= 10
select new { c.Name, OrderCount = n }
```
wird übersetzt in
```csharp
from * in customers.
    GroupJoin(orders, c => c.CustomerID, o => o.CustomerID,
        (c, co) => new { c, co })
let n = co.Count()
where n >= 10 
select new { c.Name, OrderCount = n }
```
die letzte Übersetzung, von der
```csharp
customers.
GroupJoin(orders, c => c.CustomerID, o => o.CustomerID,
    (c, co) => new { c, co }).
Select(x => new { x, n = x.co.Count() }).
Where(y => y.n >= 10).
Select(y => new { y.x.c.Name, OrderCount = y.n)
```
Dabei sind `x` und `y` von compilergenerierten Bezeichnungen, die andernfalls unsichtbar und nicht zugänglich sind.

Das Beispiel
```csharp
from o in orders
orderby o.Customer.Name, o.Total descending
select o
```
hat die endgültige Übersetzung
```csharp
orders.
OrderBy(o => o.Customer.Name).
ThenByDescending(o => o.Total)
```

#### <a name="select-clauses"></a>Select-Klauseln

Ein Abfrage Ausdruck der Form
```csharp
from x in e select v
```
wird übersetzt in
```csharp
( e ) . Select ( x => v )
```
mit der Ausnahme, dass v der Bezeichner x ist, ist die Übersetzung einfach
```csharp
( e )
```

Beispiel:
```csharp
from c in customers.Where(c => c.City == "London")
select c
```
wird einfach in übersetzt
```csharp
customers.Where(c => c.City == "London")
```

#### <a name="groupby-clauses"></a>GroupBy-Klauseln

Ein Abfrage Ausdruck der Form
```csharp
from x in e group v by k
```
wird übersetzt in
```csharp
( e ) . GroupBy ( x => k , x => v )
```
außer wenn v der Bezeichner x ist, ist die Übersetzung
```csharp
( e ) . GroupBy ( x => k )
```

Das Beispiel
```csharp
from c in customers
group c.Name by c.Country
```
wird übersetzt in
```csharp
customers.
GroupBy(c => c.Country, c => c.Name)
```

#### <a name="transparent-identifiers"></a>Transparente Bezeichner

Bestimmte Übersetzungen fügen Bereichs Variablen mit ***transparenten Bezeichner*** ein, die durch `*`gekennzeichnet sind. Transparente Bezeichner sind keine ordnungsgemäße Sprachfunktion. Sie sind nur als Zwischenschritt im Übersetzungsprozess des Abfrage Ausdrucks vorhanden.

Wenn eine Abfrage Übersetzung einen transparenten Bezeichner einfügt, verbreiten weitere Übersetzungsschritte den transparenten Bezeichner an anonyme Funktionen und anonyme Objektinitialisierer. In diesen Kontexten weisen transparente Bezeichner folgendes Verhalten auf:

*  Wenn ein transparenter Bezeichner als Parameter in einer anonymen Funktion auftritt, werden die Member des zugeordneten anonymen Typs automatisch im Gültigkeitsbereich des Texts der anonymen Funktion angezeigt.
*  Wenn sich ein Element mit einem transparenten Bezeichner im Gültigkeitsbereich befindet, sind die Member dieses Members ebenfalls im Gültigkeitsbereich.
*  Wenn ein transparenter Bezeichner als Member-Deklarator in einem anonymen Objektinitialisierer auftritt, führt er einen Member mit einem transparenten Bezeichner ein.
*  In den oben beschriebenen Übersetzungs Schritten werden transparente Bezeichner immer mit anonymen Typen eingeführt, mit dem Ziel, mehrere Bereichs Variablen als Member eines einzelnen Objekts zu erfassen. Eine Implementierung von C# darf einen anderen Mechanismus als anonyme Typen verwenden, um mehrere Bereichs Variablen zu gruppieren. In den folgenden Übersetzungs Beispielen wird davon ausgegangen, dass anonyme Typen verwendet werden, und es wird gezeigt, wie transparente Bezeichner übersetzt werden können.

Das Beispiel
```csharp
from c in customers
from o in c.Orders
orderby o.Total descending
select new { c.Name, o.Total }
```
wird übersetzt in
```csharp
from * in customers.
    SelectMany(c => c.Orders, (c,o) => new { c, o })
orderby o.Total descending
select new { c.Name, o.Total }
```

weiter übersetzt in
```csharp
customers.
SelectMany(c => c.Orders, (c,o) => new { c, o }).
OrderByDescending(* => o.Total).
Select(* => new { c.Name, o.Total })
```
, die beim Löschen transparenter Bezeichner entspricht.
```csharp
customers.
SelectMany(c => c.Orders, (c,o) => new { c, o }).
OrderByDescending(x => x.o.Total).
Select(x => new { x.c.Name, x.o.Total })
```
Dabei ist `x` ein vom Compiler generierter Bezeichner, der andernfalls unsichtbar und nicht zugänglich ist.

Das Beispiel
```csharp
from c in customers
join o in orders on c.CustomerID equals o.CustomerID
join d in details on o.OrderID equals d.OrderID
join p in products on d.ProductID equals p.ProductID
select new { c.Name, o.OrderDate, p.ProductName }
```
wird übersetzt in
```csharp
from * in customers.
    Join(orders, c => c.CustomerID, o => o.CustomerID, 
        (c, o) => new { c, o })
join d in details on o.OrderID equals d.OrderID
join p in products on d.ProductID equals p.ProductID
select new { c.Name, o.OrderDate, p.ProductName }
```
Dies wird weiter reduziert auf
```csharp
customers.
Join(orders, c => c.CustomerID, o => o.CustomerID, (c, o) => new { c, o }).
Join(details, * => o.OrderID, d => d.OrderID, (*, d) => new { *, d }).
Join(products, * => d.ProductID, p => p.ProductID, (*, p) => new { *, p }).
Select(* => new { c.Name, o.OrderDate, p.ProductName })
```
die letzte Übersetzung, von der
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
Dabei sind `x`, `y`und `z` von compilergenerierten Bezeichner, die ansonsten unsichtbar sind und nicht zugänglich sind.

### <a name="the-query-expression-pattern"></a>Das Abfrage Ausdrucksmuster

Das ***Abfrage Ausdrucksmuster*** stellt ein Muster von Methoden dar, die von Typen implementiert werden können, um Abfrage Ausdrücke zu unterstützen. Da Abfrage Ausdrücke über eine syntaktische Zuordnung in Methodenaufrufe übersetzt werden, haben Typen bei der Implementierung des Abfrage Ausdrucks Musters eine beträchtliche Flexibilität. Beispielsweise können die Methoden des Musters als Instanzmethoden oder als Erweiterungs Methoden implementiert werden, da beide die gleiche Aufruf Syntax aufweisen und die Methoden Delegaten oder Ausdrucks Baumstrukturen anfordern können, da anonyme Funktionen in beides konvertierbar sind.

Die empfohlene Form eines generischen Typs `C<T>` der das Abfrage Ausdrucksmuster unterstützt, ist unten dargestellt. Ein generischer Typ wird verwendet, um die richtigen Beziehungen zwischen Parameter-und Ergebnistypen zu veranschaulichen, aber es ist auch möglich, das Muster für nicht generische Typen zu implementieren.

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

In den obigen Methoden werden die generischen Delegattypen `Func<T1,R>` und `Func<T1,T2,R>`verwendet, aber Sie könnten auch andere Delegattypen von Delegaten oder Ausdrücken mit denselben Beziehungen in Parameter-und Ergebnistypen verwenden.

Beachten Sie die empfohlene Beziehung zwischen `C<T>` und `O<T>`, mit der sichergestellt wird, dass die `ThenBy` und `ThenByDescending` Methoden nur für das Ergebnis einer `OrderBy` oder `OrderByDescending`verfügbar sind. Beachten Sie auch die empfohlene Form des Ergebnisses `GroupBy`--eine Sequenz von Sequenzen, wobei jede innere Sequenz über eine zusätzliche `Key` Eigenschaft verfügt.

Der `System.Linq`-Namespace stellt eine Implementierung des Abfrage Operator Musters für jeden Typ bereit, der die `System.Collections.Generic.IEnumerable<T>`-Schnittstelle implementiert.

## <a name="assignment-operators"></a>Zuweisungsoperatoren

Die Zuweisungs Operatoren weisen einer Variablen, einer Eigenschaft, einem Ereignis oder einem Indexer-Element einen neuen Wert zu.

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

Der linke Operand einer Zuweisung muss ein Ausdruck sein, der als Variable, Eigenschaften Zugriff, Indexerzugriff oder Ereignis Zugriff klassifiziert ist.

Der `=`-Operator wird als ***einfacher Zuweisungs Operator***bezeichnet. Er weist den Wert des rechten Operanden der Variablen, der Eigenschaft oder dem Indexer-Element zu, die durch den linken Operanden angegeben werden. Der linke Operand des einfachen Zuweisungs Operators ist möglicherweise kein Ereignis Zugriff (außer wie in [Feld ähnlichen Ereignissen](classes.md#field-like-events)beschrieben). Der einfache Zuweisungs Operator wird unter [einfache Zuweisung](expressions.md#simple-assignment)beschrieben.

Die anderen Zuweisungs Operatoren als der `=`-Operator werden als ***Verbund Zuweisungs Operatoren***bezeichnet. Diese Operatoren führen den angegebenen Vorgang für die beiden Operanden aus und weisen dann den resultierenden Wert der Variablen, der Eigenschaft oder dem Indexer-Element zu, das durch den linken Operanden angegeben wird. Die Verbund Zuweisungs Operatoren werden unter [Verbund Zuweisung](expressions.md#compound-assignment)beschrieben.

Die `+=`-und `-=` Operatoren mit einem Ereignis Zugriffs Ausdruck als Linker Operand werden als *Ereignis Zuweisungs Operatoren*bezeichnet. Kein anderer Zuweisungs Operator ist mit einem Ereignis Zugriff als Linker Operand gültig. Die Ereignis Zuweisungs Operatoren werden unter [Ereignis Zuweisung](expressions.md#event-assignment)beschrieben.

Die Zuweisungs Operatoren sind rechts assoziativ, was bedeutet, dass Vorgänge von rechts nach Links gruppiert werden. Beispielsweise wird ein Ausdruck der Form `a = b = c` als `a = (b = c)`ausgewertet.

### <a name="simple-assignment"></a>Einfache Zuweisung

Der `=`-Operator wird als einfacher Zuweisungs Operator bezeichnet.

Wenn der linke Operand einer einfachen Zuweisung das Formular `E.P` oder `E[Ei]` ist, in dem `E` den Typ der Kompilierzeit `dynamic`hat, wird die Zuweisung dynamisch gebunden ([dynamische Bindung](expressions.md#dynamic-binding)). In diesem Fall wird der Kompilier Zeittyp des Zuweisungs Ausdrucks `dynamic`, und die unten beschriebene Auflösung erfolgt zur Laufzeit basierend auf dem Lauf Zeittyp `E`.

In einer einfachen Zuweisung muss der rechte Operand ein Ausdruck sein, der implizit in den Typ des linken Operanden konvertiert werden kann. Der-Vorgang weist den Wert des rechten Operanden der Variablen, der Eigenschaft oder dem Indexer-Element zu, die durch den linken Operanden angegeben werden.

Das Ergebnis eines einfachen Zuweisungs Ausdrucks ist der Wert, der dem linken Operanden zugewiesen wird. Das Ergebnis hat denselben Typ wie der linke Operand und wird immer als Wert klassifiziert.

Wenn der linke Operand eine Eigenschaft oder ein Indexer-Zugriff ist, muss die Eigenschaft oder der Indexer über einen `set` Accessor verfügen. Wenn dies nicht der Fall ist, tritt ein Bindungs Zeitfehler auf.

Die Lauf Zeit Verarbeitung einer einfachen Zuweisung der Form `x = y` besteht aus den folgenden Schritten:

*  Wenn `x` als Variable klassifiziert ist:
   * `x` wird ausgewertet, um die Variable zu entwickeln.
   * `y` wird ausgewertet und, falls erforderlich, durch eine implizite Konvertierung in den Typ der `x` konvertiert ([implizite Konvertierungen](conversions.md#implicit-conversions)).
   * Wenn die von `x` angegebene Variable ein Array Element eines *reference_type*ist, wird eine Lauf Zeit Überprüfung durchgeführt, um sicherzustellen, dass der für `y` berechnete Wert mit der Array Instanz kompatibel ist, von der `x` ein Element ist. Die Überprüfung ist erfolgreich, wenn `y` `null`ist, oder wenn eine implizite Verweis Konvertierung ([implizite Verweis Konvertierungen](conversions.md#implicit-reference-conversions)) aus dem tatsächlichen Typ der Instanz vorhanden ist, auf die durch `y` auf den eigentlichen Elementtyp der Array Instanz, die `x`enthält, verwiesen wird. Andernfalls wird eine `System.ArrayTypeMismatchException` ausgelöst.
   * Der von der Auswertung und der Konvertierung `y` resultierende Wert wird an dem Speicherort gespeichert, der durch die Auswertung `x`angegeben wird.
*  Wenn `x` als Eigenschaft oder Indexer-Zugriff klassifiziert ist:
   * Der Instanzausdruck (wenn `x` nicht `static`ist) und die Argumentliste (wenn `x` ein Indexer-Zugriff ist), der `x` zugeordnet ist, werden ausgewertet, und die Ergebnisse werden im nachfolgenden `set` Accessor-Aufruf verwendet.
   * `y` wird ausgewertet und, falls erforderlich, durch eine implizite Konvertierung in den Typ der `x` konvertiert ([implizite Konvertierungen](conversions.md#implicit-conversions)).
   * Der `set` Accessor von `x` wird mit dem für `y` berechneten Wert als `value` Argument aufgerufen.

Die Array-Co-Varianz Regeln ([Array-Kovarianz](arrays.md#array-covariance)) erlauben, dass ein Wert eines Arraytyps `A[]` ein Verweis auf eine Instanz eines Arraytyps `B[]`ist, vorausgesetzt, dass eine implizite Verweis Konvertierung von `B` in `A`vorhanden ist. Aufgrund dieser Regeln erfordert die Zuweisung zu einem Array Element einer *reference_type* eine Lauf Zeit Überprüfung, um sicherzustellen, dass der zugewiesene Wert mit der Array Instanz kompatibel ist. Im Beispiel
```csharp
string[] sa = new string[10];
object[] oa = sa;

oa[0] = null;               // Ok
oa[1] = "Hello";            // Ok
oa[2] = new ArrayList();    // ArrayTypeMismatchException
```
die letzte Zuweisung bewirkt, dass eine `System.ArrayTypeMismatchException` ausgelöst wird, weil eine Instanz von `ArrayList` nicht in einem Element eines `string[]`gespeichert werden kann.

Wenn eine in einem *struct_type* deklarierte Eigenschaft oder ein Indexer das Ziel einer Zuweisung ist, muss der Instanzausdruck, der der Eigenschaft oder dem Indexer-Zugriff zugeordnet ist, als Variable klassifiziert werden. Wenn der Instanzausdruck als Wert klassifiziert wird, tritt ein Fehler bei der Bindung auf. Aufgrund des [Mitglieds Zugriffs](expressions.md#member-access)gilt die gleiche Regel auch für Felder.

Bei den Deklarationen:
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
im Beispiel
```csharp
Point p = new Point();
p.X = 100;
p.Y = 100;
Rectangle r = new Rectangle();
r.A = new Point(10, 10);
r.B = p;
```
die Zuweisungen zu `p.X`, `p.Y`, `r.A`und `r.B` sind zulässig, weil `p` und `r` Variablen sind. Im Beispiel
```csharp
Rectangle r = new Rectangle();
r.A.X = 10;
r.A.Y = 10;
r.B.X = 100;
r.B.Y = 100;
```
die Zuweisungen sind ungültig, da `r.A` und `r.B` keine Variablen sind.

### <a name="compound-assignment"></a>Verbundzuweisung

Wenn der linke Operand einer Verbund Zuweisung das Formular `E.P` oder `E[Ei]` ist, in dem `E` den Typ der Kompilierzeit `dynamic`hat, wird die Zuweisung dynamisch gebunden ([dynamische Bindung](expressions.md#dynamic-binding)). In diesem Fall wird der Kompilier Zeittyp des Zuweisungs Ausdrucks `dynamic`, und die unten beschriebene Auflösung erfolgt zur Laufzeit basierend auf dem Lauf Zeittyp `E`.

Ein Vorgang der Form `x op= y` wird verarbeitet, indem die binäre Operator Überladungs Auflösung ([binäre Operator Überladungs Auflösung](expressions.md#binary-operator-overload-resolution)) angewendet wird, als ob der Vorgang `x op y`geschrieben wurde. Dies ergibt folgende Szenarien:

*  Wenn der Rückgabetyp des ausgewählten Operators implizit in den Typ der `x`konvertiert werden kann, wird der Vorgang als `x = x op y`ausgewertet, mit dem Unterschied, dass `x` nur einmal ausgewertet wird.
*  Wenn es sich bei dem ausgewählten Operator um einen vordefinierten Operator handelt, wird der Vorgang als `x = (T)(x op y)`ausgewertet, wenn der Rückgabetyp des ausgewählten Operators explizit in den Typ von `x`konvertiert werden kann, und wenn `y` implizit in den Typ von `x` konvertiert werden kann oder der Operator ein Verschiebungs Operator ist, wird der Vorgang als ausgewertet, wobei `T` der Typ der `x`ist.
*  Andernfalls ist die Verbund Zuweisung ungültig, und es tritt ein Bindungs Zeitfehler auf.

Der Begriff "nur einmal ausgewertet" bedeutet, dass bei der Auswertung der `x op y`die Ergebnisse eines beliebigen eingebenden Ausdrucks `x` temporär gespeichert und dann wieder verwendet werden, wenn die Zuweisung zum `x`durchgeführt wird. Beispielsweise werden im Zuweisungs `A()[B()] += C()`, in dem `A` eine Methode ist, die `int[]`zurückgibt, und `B` und `C` sind Methoden, die `int`zurückgeben, die Methoden nur einmal in der Reihenfolge `A`, `B`, `C`aufgerufen werden.

Wenn der linke Operand einer Verbund Zuweisung ein Eigenschaften Zugriff oder ein Indexer-Zugriff ist, muss die Eigenschaft oder der Indexer sowohl einen `get` Accessor als auch einen `set` Accessor aufweisen. Wenn dies nicht der Fall ist, tritt ein Bindungs Zeitfehler auf.

Mit der zweiten obigen Regel können `x op= y` in bestimmten Kontexten als `x = (T)(x op y)` ausgewertet werden. Die Regel besteht darin, dass die vordefinierten Operatoren als Verbund Operatoren verwendet werden können, wenn der linke Operand vom Typ `sbyte`, `byte`, `short`, `ushort`oder `char`ist. Auch wenn beide Argumente von einem dieser Typen sind, führen die vordefinierten Operatoren ein Ergebnis des Typs `int`aus, wie in [Binäre numerische](expressions.md#binary-numeric-promotions)Erweiterungen beschrieben. Daher wäre es ohne Umwandlung nicht möglich, das Ergebnis dem linken Operanden zuzuweisen.

Die intuitive Auswirkung der Regel auf vordefinierte Operatoren besteht darin, dass `x op= y` zulässig ist, wenn sowohl `x op y` als auch `x = y` zulässig sind. Im Beispiel
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
der intuitive Grund für jeden Fehler ist, dass eine entsprechende einfache Zuweisung ebenfalls ein Fehler wäre.

Dies bedeutet auch, dass aufgesetzte Zuweisungs Vorgänge aufgesetzte Vorgänge unterstützen. Im Beispiel
```csharp
int? i = 0;
i += 1;             // Ok
```
der `+(int?,int?)` "angehobene Operator" wird verwendet.

### <a name="event-assignment"></a>Ereignis Zuweisung

Wenn der linke Operand eines `+=`-oder `-=` Operators als Ereignis Zugriff klassifiziert ist, wird der Ausdruck wie folgt ausgewertet:

*  Der Instanzausdruck des Ereignis Zugriffs wird ausgewertet, falls vorhanden.
*  Der rechte Operand des `+=` oder `-=` Operators wird ausgewertet und, falls erforderlich, durch eine implizite Konvertierung in den Typ des linken Operanden konvertiert ([implizite Konvertierungen](conversions.md#implicit-conversions)).
*  Ein Ereignis Accessor des Ereignisses wird mit einer Argumentliste aufgerufen, die den rechten Operanden nach der Auswertung und ggf. Konvertierung umfasst. Wenn der Operator `+=`wurde, wird der `add` Accessor aufgerufen. Wenn der Operator `-=`wurde, wird der `remove` Accessor aufgerufen.

Ein Ereignis Zuweisungs Ausdruck ergibt keinen Wert. Daher ist ein Ereignis Zuweisungs Ausdruck nur im Kontext einer *statement_expression* ([Ausdrucks Anweisungen](statements.md#expression-statements)) gültig.

## <a name="expression"></a>expression

Ein *Ausdruck* ist entweder eine *non_assignment_expression* oder eine *Zuweisung*.

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

Ein *constant_expression* ist ein Ausdruck, der zur Kompilierzeit vollständig ausgewertet werden kann.

```antlr
constant_expression
    : expression
    ;
```

Ein konstanter Ausdruck muss das `null` Literals oder ein Wert mit einem der folgenden Typen sein: `sbyte`, `byte`, `short`, `ushort`, `int`, `uint`, `long`, `ulong`, `char`, `float`, `double`, `decimal`, `bool`, `object`, `string`oder beliebiger Enumerationstyp. Nur die folgenden Konstrukte sind in konstanten Ausdrücken zulässig:

*  Literale (einschließlich des `null` Literals).
*  Verweise auf `const` Member der Klassen-und Strukturtypen.
*  Verweise auf Member von Enumerationstypen.
*  Verweise auf `const` Parameter oder lokale Variablen
*  Unter Ausdrücke in Klammern, die selbst Konstante Ausdrücke sind.
*  Umwandlungs Ausdrücke, wenn der Zieltyp einem der oben aufgeführten Typen entspricht.
*  `checked`-und `unchecked` Ausdrücke
*  Standardwert Ausdrücke
*  Nameof-Ausdrücke
*  Die vordefinierten `+`-, `-`-, `!`-und `~` unären Operatoren.
*  Der vordefinierte `+`-, `-`-, `*`-, `/`-, `%`-, `<<`-, `>>`-, `&`-, `|`-, `^`-, `&&`-, `||`-, `==`-, `!=`-und `<`-Binär Operatoren, sofern jeder Operand einen oben aufgeführten Typ hat.
*  Der `?:` bedingte Operator.

Die folgenden Konvertierungen sind in konstanten Ausdrücken zulässig:

*  Identitäts Konvertierungen
*  Numerische Konvertierungen
*  Enumerationskonvertierungen
*  Konstante Ausdrucks Konvertierungen
*  Implizite und explizite Verweis Konvertierungen, vorausgesetzt, dass die Quelle der Konvertierungen ein konstanter Ausdruck ist, der zu einem NULL-Wert ausgewertet wird.

Andere Konvertierungen, einschließlich Boxing, Unboxing und impliziter Verweis Konvertierungen von nicht-NULL-Werten, sind in konstanten Ausdrücken nicht zulässig. Beispiel:
```csharp
class C {
    const object i = 5;         // error: boxing conversion not permitted
    const object str = "hello"; // error: implicit reference conversion
}
```
die Initialisierung von i ist ein Fehler, da eine Boxing-Konvertierung erforderlich ist. Die Initialisierung von Str ist ein Fehler, da eine implizite Verweis Konvertierung von einem nicht-NULL-Wert erforderlich ist.

Wenn ein Ausdruck die oben aufgeführten Anforderungen erfüllt, wird der Ausdruck zur Kompilierzeit ausgewertet. Dies gilt auch, wenn der Ausdruck ein unter Ausdruck eines größeren Ausdrucks ist, der nicht konstante Konstrukte enthält.

Die Kompilierzeit Auswertung konstanter Ausdrücke verwendet dieselben Regeln wie die Lauf Zeit Auswertung von nicht konstanten Ausdrücken, mit dem Unterschied, dass die Kompilierzeit Auswertung dazu führt, dass ein Kompilierzeitfehler auftritt, wenn die Lauf Zeit Auswertung eine Ausnahme ausgelöst hätte.

Wenn ein konstanter Ausdruck nicht explizit in einem `unchecked` Kontext abgelegt wird, verursachen über Flüsse, die in arithmetischen Operationen für ganzzahlige Typen und Konvertierungen während der Kompilierzeit Auswertung des Ausdrucks auftreten, immer Kompilierzeitfehler ([Konstante Ausdrücke](expressions.md#constant-expressions)).

In den unten aufgeführten Kontexten treten Konstante Ausdrücke auf. In diesen Kontexten tritt ein Kompilierzeitfehler auf, wenn ein Ausdruck zur Kompilierzeit nicht vollständig ausgewertet werden kann.

*  Konstante Deklarationen ([Konstanten](classes.md#constants)).
*  Deklarationen von Enumerationsmembern ([enum](enums.md#enum-members)-Member).
*  Standardargumente von formalen Parameterlisten ([Methoden Parameter](classes.md#method-parameters))
*  `case` Bezeichnungen einer `switch` Anweisung ([Switch-Anweisung](statements.md#the-switch-statement)).
*  `goto case`-Anweisungen ([die GoTo-Anweisung](statements.md#the-goto-statement)).
*  Dimensions Längen in einem Array Erstellungs Ausdruck ([Array Erstellungs Ausdrücke](expressions.md#array-creation-expressions)), der einen Initialisierer einschließt.
*  Attribute ([Attribute](attributes.md)).

Eine implizite Konstante Ausdrucks Konvertierung ([implizite Konstante Ausdrucks Konvertierungen](conversions.md#implicit-constant-expression-conversions)) ermöglicht das Konvertieren eines konstanten Ausdrucks vom Typ `int` in `sbyte`, `byte`, `short`, `ushort`, `uint`oder `ulong`, vorausgesetzt, der Wert des konstanten Ausdrucks liegt innerhalb des Bereichs des Zieltyps.

## <a name="boolean-expressions"></a>Boolesche Ausdrücke

Ein *Boolean_expression* ist ein Ausdruck, der ein Ergebnis vom Typ "`bool`" ergibt. entweder direkt oder über die Anwendung von `operator true` in bestimmten Kontexten, wie im folgenden angegeben.

```antlr
boolean_expression
    : expression
    ;
```

Der steuernde bedingte Ausdruck eines *if_statement* ([der if-Anweisung](statements.md#the-if-statement)), *while_statement* ([die while](statements.md#the-while-statement)-Anweisung), *do_statement* ([die Do-Anweisung](statements.md#the-do-statement)) oder *for_Statement* ([die for-Anweisung](statements.md#the-for-statement)) ist eine *Boolean_expression*. Der steuernde bedingte Ausdruck des `?:` Operators ([Bedingter Operator](expressions.md#conditional-operator)) befolgt die gleichen Regeln wie ein *Boolean_expression*, aber aus Gründen der Operator Rangfolge wird als *conditional_or_expression*klassifiziert.

Ein *Boolean_expression* `E` ist erforderlich, um einen Wert vom Typ `bool`wie folgt zu erhalten:

*  Wenn `E` implizit in `bool` konvertierbar ist, wird zur Laufzeit die implizite Konvertierung angewendet.
*  Andernfalls wird die Überladungs Auflösung des unären Operators ([unäre Operator Überladungs Auflösung](expressions.md#unary-operator-overload-resolution)) verwendet, um eine eindeutige beste Implementierung der Operator `true` auf `E`zu finden, und diese Implementierung wird zur Laufzeit angewendet.
*  Wenn kein solcher Operator gefunden wird, tritt ein Bindungs Zeitfehler auf.

Der `DBBool` Strukturtyp in einem [booleschen Datenbanktyp](structs.md#database-boolean-type) enthält ein Beispiel für einen Typ, der `operator true` und `operator false`implementiert.
