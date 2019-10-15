---
ms.openlocfilehash: 7248a91976c479dc1b6b64b799639635617a7bec
ms.sourcegitcommit: 892af9016b3317a8fae12d195014dc38ba51cf16
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/01/2019
ms.locfileid: "71704042"
---
# <a name="statements"></a>Anweisungen

C#stellt eine Reihe von-Anweisungen bereit. Die meisten dieser Anweisungen werden Entwicklern vertraut sein, die in C und C++programmiert haben.

```antlr
statement
    : labeled_statement
    | declaration_statement
    | embedded_statement
    ;

embedded_statement
    : block
    | empty_statement
    | expression_statement
    | selection_statement
    | iteration_statement
    | jump_statement
    | try_statement
    | checked_statement
    | unchecked_statement
    | lock_statement
    | using_statement
    | yield_statement
    | embedded_statement_unsafe
    ;
```

Das *embedded_statement* nicht Terminal wird für Anweisungen verwendet, die in anderen Anweisungen angezeigt werden. Die Verwendung von *embedded_statement* anstelle von- *Anweisungen schließt die* Verwendung von Deklarations Anweisungen und bezeichneten Anweisungen in diesen Kontexten aus. Das Beispiel
```csharp
void F(bool b) {
    if (b)
        int i = 44;
}
```
führt zu einem Kompilierzeitfehler, da eine `if`-Anweisung anstelle einer- *Anweisung* für Ihre if-Verzweigung eine *embedded_statement* -Anweisung erfordert. Wenn dieser Code zulässig ist, wird die Variable `i` deklariert, Sie konnte aber nie verwendet werden. Beachten Sie jedoch, dass das Beispiel `i`durch Platzieren der Deklaration in einem-Block gültig ist.

## <a name="end-points-and-reachability"></a>Endpunkte und Erreichbarkeit

Jede-Anweisung verfügt über einen ***Endpunkt***. Der Endpunkt einer-Anweisung ist in intuitiver Hinsicht der Speicherort, der direkt auf die-Anweisung folgt. Die Ausführungs Regeln für zusammengesetzte Anweisungen (Anweisungen, die eingebettete Anweisungen enthalten) geben die Aktion an, die durchgeführt wird, wenn das Steuerelement den Endpunkt einer eingebetteten Anweisung erreicht. Wenn das Steuerelement z. b. den Endpunkt einer-Anweisung in einem-Block erreicht, wird die Steuerung an die nächste Anweisung im-Block übertragen.

Wenn eine-Anweisung möglicherweise durch die Ausführung erreicht werden kann, wird die-Anweisung als ***erreichbar***bezeichnet. Wenn es nicht möglich ist, dass eine-Anweisung ausgeführt wird, wird die-Anweisung als ***nicht erreichbar***bezeichnet.

Im Beispiel
```csharp
void F() {
    Console.WriteLine("reachable");
    goto Label;
    Console.WriteLine("unreachable");
    Label:
    Console.WriteLine("reachable");
}
```
der zweite Aufruf von `Console.WriteLine` ist nicht erreichbar, da es nicht möglich ist, dass die Anweisung ausgeführt wird.

Eine Warnung wird gemeldet, wenn der Compiler feststellt, dass eine-Anweisung nicht erreichbar ist. Es ist kein Fehler, wenn eine-Anweisung nicht erreichbar ist.

Um zu ermitteln, ob eine bestimmte Anweisung oder ein Endpunkt erreichbar ist, führt der Compiler die Fluss Analyse gemäß den für jede Anweisung definierten Erreichbarkeits Regeln aus. Die Fluss Analyse berücksichtigt die Werte konstanter Ausdrücke ([Konstantenausdrücke](expressions.md#constant-expressions)), die das Verhalten von-Anweisungen steuern, aber die möglichen Werte von nicht konstanten Ausdrücken werden nicht berücksichtigt. Dies bedeutet, dass ein nicht konstanter Ausdruck eines bestimmten Typs für die Ablauf Steuerungs Analyse einen möglichen Wert dieses Typs hat.

Im Beispiel
```csharp
void F() {
    const int i = 1;
    if (i == 2) Console.WriteLine("unreachable");
}
```
der boolesche Ausdruck der `if` -Anweisung ist ein konstanter Ausdruck, da beide Operanden `==` des Operators Konstanten sind. Da der Konstante Ausdruck zum Zeitpunkt der Kompilierung ausgewertet wird und den Wert `false`erzeugt, `Console.WriteLine` wird der Aufruf als nicht erreichbar betrachtet. Wenn `i` jedoch in eine lokale Variable geändert wird
```csharp
void F() {
    int i = 1;
    if (i == 2) Console.WriteLine("reachable");
}
```
der `Console.WriteLine` Aufruf wird als erreichbar betrachtet, obwohl er in Wirklichkeit nie ausgeführt wird.

Der *Block* eines Funktionsmembers gilt immer als erreichbar. Wenn Sie die Erreichbarkeits Regeln der einzelnen Anweisungen in einem Block nacheinander auswerten, kann die Erreichbarkeit einer beliebigen Anweisung bestimmt werden.

Im Beispiel
```csharp
void F(int x) {
    Console.WriteLine("start");
    if (x < 0) Console.WriteLine("negative");
}
```
die Erreichbarkeit der zweiten `Console.WriteLine` wird wie folgt bestimmt:

*  Die erste `Console.WriteLine` Ausdrucks Anweisung ist erreichbar, da der-Block `F` der-Methode erreichbar ist.
*  Der Endpunkt der ersten `Console.WriteLine` Ausdrucks Anweisung ist erreichbar, da diese Anweisung erreichbar ist.
*  Die `if` -Anweisung ist erreichbar, da der Endpunkt der ersten `Console.WriteLine` Ausdrucks Anweisung erreichbar ist.
*  Die zweite `Console.WriteLine` Ausdrucks Anweisung ist erreichbar, da der boolesche Ausdruck `if` der Anweisung nicht über den konstanten Wert `false`verfügt.

Es gibt zwei Situationen, in denen es sich um einen Kompilierzeitfehler für den Endpunkt einer-Anweisung handelt, der erreichbar ist:

*  Da die `switch` Anweisung nicht zulässt, dass ein Switch-Abschnitt zum nächsten switch-Abschnitt "durchläuft", ist dies ein Kompilierzeitfehler für den Endpunkt der Anweisungs Liste eines Switch-Abschnitts, der erreichbar ist. Wenn dieser Fehler auftritt, ist dies in der Regel ein Hinweis `break` darauf, dass eine-Anweisung fehlt.
*  Es handelt sich um einen Kompilierzeitfehler für den Endpunkt des Blocks eines Funktionsmembers, der einen Wert berechnet, der erreichbar ist. Wenn dieser Fehler auftritt, ist dies in der Regel ein Hinweis `return` darauf, dass eine-Anweisung fehlt.

## <a name="blocks"></a>Blöcke

Ein *Block* ermöglicht, mehrere Anweisungen in Kontexten zu schreiben, in denen eine einzelne Anweisung zulässig ist.

```antlr
block
    : '{' statement_list? '}'
    ;
```

Ein- *Block* besteht aus einer optionalen *statement_list* ([Anweisungs Liste](statements.md#statement-lists)), die in geschweiften Klammern eingeschlossen ist. Wenn die Anweisungs Liste weggelassen wird, wird der Block als leer bezeichnet.

Ein-Block kann Deklarations Anweisungen ([Deklarations Anweisungen](statements.md#declaration-statements)) enthalten. Der Gültigkeitsbereich einer lokalen Variablen oder Konstanten, die in einem-Block deklariert ist, ist der-Block.

Ein-Block wird wie folgt ausgeführt:

*  Wenn der-Block leer ist, wird die Steuerung an den Endpunkt des-Blocks übertragen.
*  Wenn der Block nicht leer ist, wird die Steuerung an die Anweisungs Liste übertragen. Wenn und wenn das Steuerelement den Endpunkt der Anweisungs Liste erreicht, wird die Steuerung an den Endpunkt des Blocks übertragen.

Die Anweisungs Liste eines-Blocks ist erreichbar, wenn der Block selbst erreichbar ist.

Der Endpunkt eines-Blocks ist erreichbar, wenn der-Block leer ist oder der Endpunkt der Anweisungs Liste erreichbar ist.

Ein- *Block* , der eine oder `yield` mehrere-Anweisungen ([die yield-Anweisung](statements.md#the-yield-statement)) enthält, wird als Iteratorblock bezeichnet. Iteratorblöcke werden verwendet, um Funktionsmember als Iteratoren ([Iteratoren](classes.md#iterators)) zu implementieren. Für iteratorblöcke gelten einige zusätzliche Einschränkungen:

*  Es ist ein Kompilierzeitfehler, wenn `return` eine Anweisung in einem Iteratorblock angezeigt wird ( `yield return` Anweisungen sind jedoch zulässig).
*  Es ist ein Kompilierzeitfehler für einen Iteratorblock, der einen unsicheren Kontext ([unsichere Kontexte](unsafe-code.md#unsafe-contexts)) enthalten soll. Ein Iteratorblock definiert immer einen sicheren Kontext, auch wenn seine Deklaration in einem unsicheren Kontext eingebettet ist.

### <a name="statement-lists"></a>Anweisungs Listen

Eine ***Anweisungs Liste*** besteht aus einer oder mehreren Anweisungen, die nacheinander geschrieben werden. Anweisungs Listen treten in *Block*s ([Blocks](statements.md#blocks)) und in *switch_block*s ([der Switch-Anweisung](statements.md#the-switch-statement)) auf.

```antlr
statement_list
    : statement+
    ;
```

Eine Anweisungs Liste wird ausgeführt, indem die Steuerung an die erste Anweisung übertragen wird. Wenn und wenn das Steuerelement den Endpunkt einer-Anweisung erreicht, wird die Steuerung an die nächste Anweisung übertragen. Wenn und wenn das Steuerelement den Endpunkt der letzten Anweisung erreicht, wird die Steuerung an den Endpunkt der Anweisungs Liste übertragen.

Eine-Anweisung in einer Anweisungs Liste ist erreichbar, wenn mindestens einer der folgenden Punkte zutrifft:

*  Die-Anweisung ist die erste Anweisung, und die Anweisungs Liste selbst ist erreichbar.
*  Der Endpunkt der vorhergehenden Anweisung ist erreichbar.
*  Die-Anweisung ist eine Anweisung mit Bezeichnung, und auf die Bezeichnung wird `goto` durch eine erreichbare Anweisung verwiesen.

Der Endpunkt einer Anweisungs Liste ist erreichbar, wenn der Endpunkt der letzten Anweisung in der Liste erreichbar ist.

## <a name="the-empty-statement"></a>Die leere Anweisung

Ein *empty_statement* tut nichts.

```antlr
empty_statement
    : ';'
    ;
```

Eine leere-Anweisung wird verwendet, wenn keine Vorgänge in einem Kontext durchgeführt werden müssen, in dem eine-Anweisung erforderlich ist.

Durch die Ausführung einer leeren-Anweisung wird die Steuerung einfach an den Endpunkt der Anweisung übertragen. Daher ist der Endpunkt einer leeren Anweisung erreichbar, wenn die leere Anweisung erreichbar ist.

Eine leere Anweisung kann beim Schreiben einer `while` Anweisung mit einem NULL-Text verwendet werden:
```csharp
bool ProcessMessage() {...}

void ProcessMessages() {
    while (ProcessMessage())
        ;
}
```

Außerdem kann eine leere Anweisung verwendet werden, um eine Bezeichnung direkt vor dem schließenden "`}`" eines Blocks zu deklarieren:
```csharp
void F() {
    ...
    if (done) goto exit;
    ...
    exit: ;
}
```

## <a name="labeled-statements"></a>Anweisungen mit Bezeichnung

Ein *labeled_statement* ermöglicht einer-Anweisung, eine Bezeichnung als Präfix festzustellen. Anweisung mit Bezeichnung ist in-Blöcken zulässig, aber nicht als eingebettete Anweisungen zulässig.

```antlr
labeled_statement
    : identifier ':' statement
    ;
```

Eine Anweisung mit Bezeichnung deklariert eine Bezeichnung mit dem Namen, der vom *Bezeichner*angegeben wird. Der Gültigkeitsbereich einer Bezeichnung ist der gesamte Block, in dem die Bezeichnung deklariert wird, einschließlich aller Blöcke, die in der Liste enthalten sind. Es handelt sich um einen Kompilierzeitfehler für zwei Bezeichnungen mit dem gleichen Namen, die sich überlappende Bereiche befinden.

Auf eine Bezeichnung kann in- `goto` Anweisungen ([der GOTO-Anweisung](statements.md#the-goto-statement)) innerhalb des Bereichs der Bezeichnung verwiesen werden. Dies bedeutet, `goto` dass-Anweisungen die Steuerung innerhalb von Blöcken und aus Blöcken, aber nie in Blöcke übertragen können.

Bezeichnungen verfügen über einen eigenen Deklarations Bereich und stören andere Bezeichner nicht. Das Beispiel
```csharp
int F(int x) {
    if (x >= 0) goto x;
    x = -x;
    x: return x;
}
```
ist gültig und verwendet den Namen `x` sowohl als Parameter als auch als Bezeichnung.

Die Ausführung einer Anweisung mit Bezeichnung entspricht genau der Ausführung der Anweisung nach der Bezeichnung.

Zusätzlich zur Erreichbarkeit der normalen Ablauf Steuerung ist eine Anweisung mit der Bezeichnung erreichbar, wenn von einer erreichbaren `goto` Anweisung auf die Bezeichnung verwiesen wird. Distanzieren Wenn sich `goto` eine-Anweisung innerhalb `try` eines-Blocks `finally` befindet `try`, der einen-Block enthält, und die Anweisung mit der Bezeichnung außerhalb von `finally` liegt und der Endpunkt des Blocks nicht erreichbar ist, ist die Anweisung mit der Bezeichnung nicht erreichbar. Diese `goto` Anweisung.)

## <a name="declaration-statements"></a>Deklarationsanweisungen

Ein *declaration_statement* deklariert eine lokale Variable oder Konstante. Deklarations Anweisungen sind in-Blöcken zulässig, aber nicht als eingebettete Anweisungen zulässig.

```antlr
declaration_statement
    : local_variable_declaration ';'
    | local_constant_declaration ';'
    ;
```

### <a name="local-variable-declarations"></a>Deklarationen von lokalen Variablen

Ein *local_variable_declaration* deklariert eine oder mehrere lokale Variablen.

```antlr
local_variable_declaration
    : local_variable_type local_variable_declarators
    ;

local_variable_type
    : type
    | 'var'
    ;

local_variable_declarators
    : local_variable_declarator
    | local_variable_declarators ',' local_variable_declarator
    ;

local_variable_declarator
    : identifier
    | identifier '=' local_variable_initializer
    ;

local_variable_initializer
    : expression
    | array_initializer
    | local_variable_initializer_unsafe
    ;
```

Der *local_variable_type* eines *local_variable_declaration* gibt entweder direkt den Typ der Variablen an, die von der Deklaration eingeführt wurden, oder gibt mit dem Bezeichner an, `var`, dass der Typ basierend auf einem Initialisierer abgeleitet werden soll. Auf den Typ folgt eine Liste von *local_variable_declarator*s, von denen jeder eine neue Variable einführt. Ein *local_variable_declarator* besteht aus einem *Bezeichner* , der die Variable benennt, optional gefolgt von einem "`=`"-Token und einem *local_variable_initializer* , das den Anfangswert der Variablen liefert.

Im Kontext einer lokalen Variablen Deklaration fungiert der Bezeichner var als kontextbezogenes Schlüsselwort ([Schlüssel](lexical-structure.md#keywords)Wort). Wenn *local_variable_type* als `var` angegeben wird und sich kein Typ mit dem Namen `var` im Gültigkeitsbereich befindet, ist die Deklaration eine ***implizit typisierte lokale Variablen Deklaration***, deren Typ vom Typ des zugeordneten initialisiererausdrucks abgeleitet wird. Implizit typisierte lokale Variablen Deklarationen unterliegen den folgenden Einschränkungen:

*  *Local_variable_declaration* kann nicht mehrere *local_variable_declarator*s enthalten.
*  *Local_variable_declarator* muss ein *local_variable_initializer*enthalten.
*  Der *local_variable_initializer* muss ein *Ausdruck*sein.
*  Der initialisiererausdruck muss einen Kompilier Zeittyp aufweisen.
*  Der initialisiererausdruck kann nicht auf die deklarierte Variable selbst verweisen.

Im folgenden finden Sie Beispiele für falsche implizit typisierte lokale Variablen Deklarationen:

```csharp
var x;               // Error, no initializer to infer type from
var y = {1, 2, 3};   // Error, array initializer not permitted
var z = null;        // Error, null does not have a type
var u = x => x + 1;  // Error, anonymous functions do not have a type
var v = v++;         // Error, initializer cannot refer to variable itself
```

Der Wert einer lokalen Variablen wird in einem Ausdruck mithilfe eines *Simple_name* ([simple names](expressions.md#simple-names)) abgerufen, und der Wert einer lokalen Variablen wird mithilfe einer *Zuweisung* ([Zuweisungs Operatoren](expressions.md#assignment-operators)) geändert. Eine lokale Variable muss an jedem Speicherort, an dem ihr Wert abgerufen wird, definitiv zugewiesen werden ([definitive Zuweisung](variables.md#definite-assignment)).

Der Gültigkeitsbereich einer lokalen Variablen, die in einem *local_variable_declaration* deklariert ist, ist der Block, in dem die Deklaration auftritt. Es ist ein Fehler, auf eine lokale Variable in einer Textposition zu verweisen, die der *local_variable_declarator* der lokalen Variablen vorangestellt ist. Innerhalb des Gültigkeits Bereichs einer lokalen Variablen ist es ein Kompilierzeitfehler, eine andere lokale Variable oder Konstante mit dem gleichen Namen zu deklarieren.

Eine lokale Variablen Deklaration, die mehrere Variablen deklariert, entspricht mehreren Deklarationen von einzelnen Variablen mit demselben Typ. Außerdem entspricht ein Variableninitialisierer in einer lokalen Variablen Deklaration genau einer Zuweisungsanweisung, die unmittelbar nach der Deklaration eingefügt wird.

Das Beispiel
```csharp
void F() {
    int x = 1, y, z = x * 2;
}
```
entspricht genau
```csharp
void F() {
    int x; x = 1;
    int y;
    int z; z = x * 2;
}
```

In einer implizit typisierten lokalen Variablen Deklaration wird der Typ der zu deklarierenden lokalen Variablen mit dem Typ des Ausdrucks, der zum Initialisieren der Variablen verwendet wird, identisch sein. Zum Beispiel:
```csharp
var i = 5;
var s = "Hello";
var d = 1.0;
var numbers = new int[] {1, 2, 3};
var orders = new Dictionary<int,Order>();
```

Die oben genannten implizit typisierten lokalen Variablen Deklarationen entsprechen genau den folgenden explizit typisierten Deklarationen:
```csharp
int i = 5;
string s = "Hello";
double d = 1.0;
int[] numbers = new int[] {1, 2, 3};
Dictionary<int,Order> orders = new Dictionary<int,Order>();
```

### <a name="local-constant-declarations"></a>Lokale Konstante Deklarationen

Ein *local_constant_declaration* deklariert eine oder mehrere lokale Konstanten.

```antlr
local_constant_declaration
    : 'const' type constant_declarators
    ;

constant_declarators
    : constant_declarator (',' constant_declarator)*
    ;

constant_declarator
    : identifier '=' constant_expression
    ;
```

Der *Typ* eines *local_constant_declaration* gibt den Typ der Konstanten an, die von der Deklaration eingeführt wurden. Auf den Typ folgt eine Liste von *constant_declarator*s, von denen jeder eine neue Konstante einführt. Ein *constant_declarator* besteht aus einem *Bezeichner* , der die Konstante benennt, gefolgt von einem "`=`"-Token, gefolgt von einem *constant_expression* ([Konstantenausdrücken](expressions.md#constant-expressions)), der den Wert der Konstante liefert.

Der- *Typ* und der *constant_expression* -Wert einer lokalen Konstanten Deklaration müssen dieselben Regeln wie die einer Konstanten Element Deklaration ([Konstanten](classes.md#constants)) einhalten.

Der Wert einer lokalen Konstante wird in einem Ausdruck mithilfe eines *Simple_name* ([simple names](expressions.md#simple-names)) abgerufen.

Der Gültigkeitsbereich einer lokalen Konstante ist der Block, in dem die Deklaration auftritt. Es ist ein Fehler, auf eine lokale Konstante in einer Textposition zu verweisen, die der *constant_declarator*vorausgeht. Innerhalb des Gültigkeits Bereichs einer lokalen Konstante handelt es sich um einen Kompilierzeitfehler, um eine andere lokale Variable oder Konstante mit dem gleichen Namen zu deklarieren.

Eine lokale Konstantendeklaration, die mehrere Konstanten deklariert, entspricht mehreren Deklarationen von einzelnen Konstanten desselben Typs.

## <a name="expression-statements"></a>Ausdrucksanweisungen

Ein *expression_statement* wertet einen angegebenen Ausdruck aus. Der von dem Ausdruck berechnete Wert, falls vorhanden, wird verworfen.

```antlr
expression_statement
    : statement_expression ';'
    ;

statement_expression
    : invocation_expression
    | null_conditional_invocation_expression
    | object_creation_expression
    | assignment
    | post_increment_expression
    | post_decrement_expression
    | pre_increment_expression
    | pre_decrement_expression
    | await_expression
    ;
```

Nicht alle Ausdrücke sind als Anweisungen zulässig. Insbesondere Ausdrücke wie `x + y` und, die nur `x == 1` einen Wert berechnen (der verworfen wird), sind nicht als-Anweisungen zulässig.

Die Ausführung eines *expression_statement* wertet den enthaltenen Ausdruck aus und überträgt dann die Steuerung an den Endpunkt des *expression_statement*. Der Endpunkt eines *expression_statement* ist erreichbar, wenn dieser *expression_statement* erreichbar ist.

## <a name="selection-statements"></a>Auswahlanweisungen

Auswahl Anweisungen wählen Sie eine der möglichen Anweisungen für die Ausführung basierend auf dem Wert eines Ausdrucks aus.

```antlr
selection_statement
    : if_statement
    | switch_statement
    ;
```

### <a name="the-if-statement"></a>Die if-Anweisung

Die `if` -Anweisung wählt eine Anweisung für die Ausführung basierend auf dem Wert eines booleschen Ausdrucks aus.

```antlr
if_statement
    : 'if' '(' boolean_expression ')' embedded_statement
    | 'if' '(' boolean_expression ')' embedded_statement 'else' embedded_statement
    ;
```

Ein `else` Teil ist mit dem lexikalisch nächstgelegenen `if` vorangehenden-Element verknüpft, das von der Syntax zugelassen wird. Folglich eine `if` -Anweisung der Form
```csharp
if (x) if (y) F(); else G();
```
für die folgende Syntax:
```csharp
if (x) {
    if (y) {
        F();
    }
    else {
        G();
    }
}
```

Eine `if` -Anweisung wird wie folgt ausgeführt:

*  Die *Boolean_expression* ([boolesche Ausdrücke](expressions.md#boolean-expressions)) werden ausgewertet.
*  Wenn der boolesche Ausdruck ergibt `true`, wird die Steuerung an die erste eingebettete Anweisung übertragen. Wenn und wenn das Steuerelement den Endpunkt dieser Anweisung erreicht, wird die Steuerung an den Endpunkt `if` der Anweisung übertragen.
*  Wenn der boolesche Ausdruck ergibt `false` und ein `else` Teil vorhanden ist, wird die Steuerung an die zweite eingebettete Anweisung übertragen. Wenn und wenn das Steuerelement den Endpunkt dieser Anweisung erreicht, wird die Steuerung an den Endpunkt `if` der Anweisung übertragen.
*  Wenn der boolesche Ausdruck ergibt `false` und ein `else` Teil nicht vorhanden ist, wird die Steuerung an `if` den Endpunkt der Anweisung übertragen.

Die erste eingebettete Anweisung einer `if` -Anweisung ist erreichbar, `if` wenn die-Anweisung erreichbar ist und der boolesche Ausdruck nicht über den `false`Konstanten Wert verfügt.

Die zweite eingebettete Anweisung einer `if` -Anweisung (falls vorhanden) ist erreichbar, `if` wenn die-Anweisung erreichbar ist und der boolesche Ausdruck nicht über den `true`Konstanten Wert verfügt.

Der Endpunkt `if` einer-Anweisung ist erreichbar, wenn der Endpunkt von mindestens einer der eingebetteten Anweisungen erreichbar ist. Außerdem ist der `if` Endpunkt einer-Anweisung ohne `else` Teil erreichbar, wenn die `if` -Anweisung erreichbar ist und der boolesche Ausdruck nicht über den konstanten Wert `true`verfügt.

### <a name="the-switch-statement"></a>Die Switch-Anweisung

Die Switch-Anweisung wählt für die Ausführung eine Anweisungs Liste mit einer zugeordneten Switch-Bezeichnung aus, die dem Wert des switch-Ausdrucks entspricht.

```antlr
switch_statement
    : 'switch' '(' expression ')' switch_block
    ;

switch_block
    : '{' switch_section* '}'
    ;

switch_section
    : switch_label+ statement_list
    ;

switch_label
    : 'case' constant_expression ':'
    | 'default' ':'
    ;
```

Ein *switch_statement* besteht aus dem Schlüsselwort `switch`, gefolgt von einem Ausdruck in Klammern (der als Switch-Ausdruck bezeichnet wird), gefolgt von einem *switch_block*. Der *switch_block* besteht aus null oder mehr *switch_section*s, der in geschweiften Klammern eingeschlossen ist. Jede *switch_section* besteht aus mindestens einer *switch_label*s, gefolgt von einer *statement_list* ([Anweisungs Liste](statements.md#statement-lists)).

Der ***governancetyp*** einer `switch` Anweisung wird durch den Switch-Ausdruck festgelegt.

*  Wenn der Switch-Ausdruck `sbyte`, `byte`, `short`, `ushort`, `int`, `uint`, `long`, `ulong`, `bool`, `char`, 0 oder ein *enum_type*, oder wenn es sich um den Typ handelt, der NULL-Werte zulässt, der einem dieser Typen entspricht. , dann ist dies der regierende Typ der 2-Anweisung.
*  Andernfalls muss genau eine benutzerdefinierte implizite Konvertierung ([benutzerdefinierte Konvertierungen](conversions.md#user-defined-conversions)) `sbyte`vom Typ des switch-Ausdrucks bis zu einem der folgenden möglichen governancetypen vorhanden sein:, `byte`,, `ushort` `short` , `int`, `uint`, ,`long` ,`char`,oder, ein Typ, der NULL-Werte zulässt, der einem dieser Typen entspricht. `ulong` `string`
*  Andernfalls tritt ein Kompilierzeitfehler auf, wenn keine solche implizite Konvertierung vorhanden ist oder wenn mehr als eine implizite Konvertierung vorhanden ist.

Der Konstante Ausdruck jeder `case` Bezeichnung muss einen Wert angeben, der implizit konvertierbar ([implizite Konvertierungen](conversions.md#implicit-conversions)) in den richtentyp `switch` der Anweisung ist. Ein Kompilierzeitfehler tritt auf, wenn zwei `case` oder mehr Bezeichnungen in `switch` derselben Anweisung denselben Konstanten Wert angeben.

In einer Switch-Anweisung darf `default` höchstens eine Bezeichnung vorhanden sein.

Eine `switch` -Anweisung wird wie folgt ausgeführt:

*  Der Switch-Ausdruck wird ausgewertet und in den regierenden Typ konvertiert.
*  Wenn eine der Konstanten, die in einer `case` Bezeichnung in derselben `switch` Anweisung angegeben ist, gleich dem Wert des switch-Ausdrucks ist, wird die Steuerung an die Anweisungs Liste nach `case` der passenden Bezeichnung übertragen.
*  Wenn keine der Konstanten, die in `case` Bezeichnungen in derselben `switch` Anweisung angegeben sind, gleich dem Wert des switch-Ausdrucks ist und eine `default` Bezeichnung vorhanden ist, wird die Steuerung an die Anweisungs Liste übertragen `default` , die nach dem ETI.
*  Wenn keine der Konstanten, die in `case` Bezeichnungen in derselben `switch` Anweisung angegeben sind, gleich dem Wert des switch-Ausdrucks ist, und wenn `default` keine Bezeichnung vorhanden ist, `switch` wird die Steuerung an den Endpunkt der Anweisung übertragen.

Wenn der Endpunkt der Anweisungs Liste eines Switch-Abschnitts erreichbar ist, tritt ein Kompilierzeitfehler auf. Dies wird als "No FallThrough"-Regel bezeichnet. Das Beispiel
```csharp
switch (i) {
case 0:
    CaseZero();
    break;
case 1:
    CaseOne();
    break;
default:
    CaseOthers();
    break;
}
```
ist gültig, da kein Switch-Abschnitt über einen erreichbaren Endpunkt verfügt. Anders als bei C++C und ist die Ausführung eines Switch-Abschnitts nicht zulässig, um zum nächsten switchabschnitt zu wechseln, und das Beispiel
```csharp
switch (i) {
case 0:
    CaseZero();
case 1:
    CaseZeroOrOne();
default:
    CaseAny();
}
```
führt zu einem Kompilierzeitfehler. Wenn die Ausführung eines Switch-Abschnitts durch die Ausführung eines anderen Switch-Abschnitts befolgt werden soll, `goto case` muss `goto default` eine explizite-oder-Anweisung verwendet werden:
```csharp
switch (i) {
case 0:
    CaseZero();
    goto case 1;
case 1:
    CaseZeroOrOne();
    goto default;
default:
    CaseAny();
    break;
}
```

Mehrere Bezeichnungen sind in einem *switch_section*zulässig. Das Beispiel
```csharp
switch (i) {
case 0:
    CaseZero();
    break;
case 1:
    CaseOne();
    break;
case 2:
default:
    CaseTwo();
    break;
}
```
ist gültig. Das Beispiel verstößt nicht gegen die Regel "No FallThrough", da die Bezeichnungen `case 2:` und `default:` Teil desselben *switch_section*sind.

Die "No FallThrough"-Regel verhindert eine gängige Klasse von Fehlern, die in C C++ auftreten `break` , und wenn Anweisungen versehentlich ausgelassen werden. Außerdem können die Switch-Abschnitte einer `switch` Anweisung aufgrund dieser Regel beliebig neu angeordnet werden, ohne dass sich dies auf das Verhalten der Anweisung auswirkt. Beispielsweise können die Abschnitte der `switch` obigen Anweisung umgekehrt werden, ohne dass sich dies auf das Verhalten der-Anweisung auswirkt:
```csharp
switch (i) {
default:
    CaseAny();
    break;
case 1:
    CaseZeroOrOne();
    goto default;
case 0:
    CaseZero();
    goto case 1;
}
```

Die Anweisungs Liste eines Switch-Abschnitts endet in der `break`Regel `goto case`mit einer `goto default` -,-oder-Anweisung, aber alle Konstrukte, die den Endpunkt der Anweisungs Liste nicht erreichbar rendern, sind zulässig. Beispielsweise ist es `while` bekannt, dass eine vom booleschen `true` Ausdruck gesteuerte Anweisung den Endpunkt niemals erreicht. Ebenso überträgt eine `throw` - `return` oder-Anweisung immer an eine andere Stelle und erreicht ihren Endpunkt nicht. Daher ist das folgende Beispiel gültig:
```csharp
switch (i) {
case 0:
    while (true) F();
case 1:
    throw new ArgumentException();
case 2:
    return;
}
```

Der governancetyp einer `switch` Anweisung kann der Typ `string`sein. Zum Beispiel:
```csharp
void DoCommand(string command) {
    switch (command.ToLower()) {
    case "run":
        DoRun();
        break;
    case "save":
        DoSave();
        break;
    case "quit":
        DoQuit();
        break;
    default:
        InvalidCommand(command);
        break;
    }
}
```

Wie bei Zeichen folgen Gleichheits Operatoren ([Zeichen folgen Gleichheits Operatoren](expressions.md#string-equality-operators)) wird bei der Anweisung die `switch` Groß-/Kleinschreibung beachtet, und es wird nur ein bestimmter Schalter Abschnitt ausgeführt, wenn die `case` Zeichenfolge des switch-Ausdrucks

Wenn der governancetyp einer `switch` -Anweisung `string`ist, ist `null` der Wert als Konstante für die Case-Bezeichnung zulässig.

Die *statement_list*s eines *switch_block* können Deklarations Anweisungen ([Deklarations Anweisungen](statements.md#declaration-statements)) enthalten. Der Gültigkeitsbereich einer lokalen Variablen oder Konstanten, die in einem Switch-Block deklariert ist, ist der Switch-Block.

Die Anweisungs Liste eines bestimmten switchabschnitts ist erreichbar, wenn `switch` die-Anweisung erreichbar ist und mindestens einer der folgenden Punkte zutrifft:

*  Der Switch-Ausdruck ist ein nicht konstanter Wert.
*  Der Switch-Ausdruck ist ein konstanter Wert, der `case` mit einer Bezeichnung im Switch-Abschnitt übereinstimmt.
*  Der Switch-Ausdruck ist ein konstanter Wert, der keiner `case` Bezeichnung entspricht, und der Switch-Abschnitt `default` enthält die Bezeichnung.
*  Auf eine Switch-Bezeichnung des Switch-Abschnitts wird durch eine `goto case` erreichbare-oder `goto default` -Anweisung verwiesen.

Der Endpunkt einer `switch` -Anweisung ist erreichbar, wenn mindestens einer der folgenden Punkte zutrifft:

*  Die `switch` -Anweisung enthält eine `break` erreichbare Anweisung, `switch` die die-Anweisung beendet.
*  Die `switch` -Anweisung ist erreichbar, der Switch-Ausdruck ist ein nicht konstanter Wert, und `default` es ist keine Bezeichnung vorhanden.
*  Die `switch` -Anweisung ist erreichbar, der Switch-Ausdruck ist ein konstanter Wert, der `case` keiner Bezeichnung entspricht, `default` und es ist keine Bezeichnung vorhanden.

## <a name="iteration-statements"></a>Iterationsanweisungen

Iterations Anweisungen führen eine eingebettete Anweisung wiederholt aus.

```antlr
iteration_statement
    : while_statement
    | do_statement
    | for_statement
    | foreach_statement
    ;
```

### <a name="the-while-statement"></a>Die while-Anweisung

Die `while` -Anweisung führt eine eingebettete Anweisung bedingt NULL oder mehrmals aus.

```antlr
while_statement
    : 'while' '(' boolean_expression ')' embedded_statement
    ;
```

Eine `while` -Anweisung wird wie folgt ausgeführt:

*  Die *Boolean_expression* ([boolesche Ausdrücke](expressions.md#boolean-expressions)) werden ausgewertet.
*  Wenn der boolesche Ausdruck ergibt `true`, wird die Steuerung an die eingebettete Anweisung übertragen. Wenn und wenn das Steuerelement den Endpunkt der eingebetteten Anweisung erreicht (möglicherweise aus der Ausführung `continue` einer-Anweisung), wird die Steuerung an den Anfang `while` der Anweisung übertragen.
*  Wenn der boolesche Ausdruck ergibt `false`, wird die Steuerung an den Endpunkt `while` der Anweisung übertragen.

Innerhalb der eingebetteten Anweisung `while` einer-Anweisung kann eine `break` -Anweisung ([die Break-Anweisung](statements.md#the-break-statement)) verwendet werden, um die `while` Steuerung an den Endpunkt der-Anweisung zu übertragen (wodurch die Iterationen der eingebetteten- Anweisungbeendetwerden).`continue` die-Anweisung ([die Continue-Anweisung](statements.md#the-continue-statement)) kann verwendet werden, um die Steuerung an den Endpunkt der eingebetteten Anweisung zu übertragen (wodurch eine `while` andere Iterations Anweisung ausgeführt wird).

Die eingebettete Anweisung einer `while` -Anweisung ist erreichbar, `while` wenn die-Anweisung erreichbar ist und der boolesche Ausdruck nicht über den `false`Konstanten Wert verfügt.

Der Endpunkt einer `while` -Anweisung ist erreichbar, wenn mindestens einer der folgenden Punkte zutrifft:

*  Die `while` -Anweisung enthält eine `break` erreichbare Anweisung, `while` die die-Anweisung beendet.
*  Die `while` -Anweisung ist erreichbar, und der boolesche Ausdruck weist nicht den Konstanten `true`Wert auf.

### <a name="the-do-statement"></a>Die Do-Anweisung

Die `do` Anweisung führt mindestens einmal eine eingebettete Anweisung aus.

```antlr
do_statement
    : 'do' embedded_statement 'while' '(' boolean_expression ')' ';'
    ;
```

Eine `do` -Anweisung wird wie folgt ausgeführt:

*  Das Steuerelement wird an die eingebettete Anweisung übertragen.
*  Wenn und wenn das Steuerelement den Endpunkt der eingebetteten Anweisung erreicht (möglicherweise aus der Ausführung einer `continue`-Anweisung), wird die *Boolean_expression* ([boolesche Ausdrücke](expressions.md#boolean-expressions)) ausgewertet. Wenn der boolesche Ausdruck ergibt `true`, wird die Steuerung an den Anfang `do` der Anweisung übertragen. Andernfalls wird die Steuerung an den Endpunkt `do` der Anweisung übertragen.

Innerhalb der eingebetteten Anweisung `do` einer-Anweisung kann eine `break` -Anweisung ([die Break-Anweisung](statements.md#the-break-statement)) verwendet werden, um die `do` Steuerung an den Endpunkt der-Anweisung zu übertragen (wodurch die Iterationen der eingebetteten- Anweisungbeendetwerden).`continue` die-Anweisung ([die Continue-Anweisung](statements.md#the-continue-statement)) kann verwendet werden, um die Steuerung an den Endpunkt der eingebetteten Anweisung zu übertragen.

Die eingebettete Anweisung einer `do` -Anweisung ist erreichbar, `do` wenn die-Anweisung erreichbar ist.

Der Endpunkt einer `do` -Anweisung ist erreichbar, wenn mindestens einer der folgenden Punkte zutrifft:

*  Die `do` -Anweisung enthält eine `break` erreichbare Anweisung, `do` die die-Anweisung beendet.
*  Der Endpunkt der eingebetteten Anweisung ist erreichbar, und der boolesche Ausdruck weist nicht den konstanten Wert `true`auf.

### <a name="the-for-statement"></a>Die for-Anweisung

Die `for` -Anweisung wertet eine Sequenz von Initialisierungs Ausdrücken aus und führt dann, während eine Bedingung true ist, wiederholt eine eingebettete Anweisung aus und wertet eine Sequenz von Iterations Ausdrücken aus.

```antlr
for_statement
    : 'for' '(' for_initializer? ';' for_condition? ';' for_iterator? ')' embedded_statement
    ;

for_initializer
    : local_variable_declaration
    | statement_expression_list
    ;

for_condition
    : boolean_expression
    ;

for_iterator
    : statement_expression_list
    ;

statement_expression_list
    : statement_expression (',' statement_expression)*
    ;
```

*For_initializer*, falls vorhanden, besteht entweder aus einer *local_variable_declaration* ([lokale Variablen Deklarationen](statements.md#local-variable-declarations)) oder einer Liste von *statement_expression*s ([Ausdrucks Anweisungen](statements.md#expression-statements)), die durch Kommas getrennt sind. Der Gültigkeitsbereich einer lokalen Variablen, die von einem *for_initializer* deklariert wird, beginnt bei *local_variable_declarator* für die Variable und reicht bis zum Ende der eingebetteten Anweisung aus. Der Bereich umfasst *for_condition* und *for_iterator*.

Der *for_condition*muss, falls vorhanden, ein *Boolean_expression* ([boolescher Ausdruck](expressions.md#boolean-expressions)) sein.

Der *for_iterator*besteht, falls vorhanden, aus einer Liste von *statement_expression*s ([Ausdrucks Anweisungen](statements.md#expression-statements)), die durch Kommas getrennt sind.

Eine for-Anweisung wird wie folgt ausgeführt:

*  Wenn ein *for_initializer* vorhanden ist, werden die Variableninitialisierer oder Anweisungs Ausdrücke in der Reihenfolge ausgeführt, in der Sie geschrieben werden. Dieser Schritt wird nur einmal ausgeführt.
*  Wenn ein *for_condition* vorhanden ist, wird es ausgewertet.
*  Wenn *for_condition* nicht vorhanden ist oder die Auswertung `true` ergibt, wird die Steuerung an die eingebettete Anweisung übertragen. Wenn und wenn das Steuerelement den Endpunkt der eingebetteten Anweisung erreicht (möglicherweise aus der Ausführung einer `continue`-Anweisung), werden die Ausdrücke des *for_iterator*, sofern vorhanden, nacheinander ausgewertet. Anschließend wird eine weitere Iterationen ausgeführt, beginnend mit Auswertung des *for_condition* im obigen Schritt.
*  Wenn *for_condition* vorhanden ist und die Auswertung `false` ergibt, wird die Steuerung an den Endpunkt der `for`-Anweisung übertragen.

Innerhalb der eingebetteten Anweisung einer `for`-Anweisung kann eine `break`-Anweisung ([die Break-Anweisung](statements.md#the-break-statement)) verwendet werden, um die Steuerung an den Endpunkt der `for`-Anweisung (d. h. die Iterations Ende der eingebetteten Anweisung) und eine `continue`-Anweisung ([ Die Continue-Anweisung](statements.md#the-continue-statement)) kann verwendet werden, um die Steuerung an den Endpunkt der eingebetteten Anweisung zu übertragen (d. h., der *for_iterator* wird ausgeführt, und es wird eine weitere Iterations Anweisung der `for`-Anweisung ausgeführt, beginnend mit dem *for_condition*).

Die eingebettete Anweisung einer `for` -Anweisung ist erreichbar, wenn eine der folgenden Aussagen zutrifft:

*  Die `for`-Anweisung ist erreichbar, und es ist keine *for_condition* vorhanden.
*  Die `for`-Anweisung ist erreichbar, und ein *for_condition* ist vorhanden und weist nicht den konstanten Wert `false` auf.

Der Endpunkt einer `for` -Anweisung ist erreichbar, wenn mindestens einer der folgenden Punkte zutrifft:

*  Die `for` -Anweisung enthält eine `break` erreichbare Anweisung, `for` die die-Anweisung beendet.
*  Die `for`-Anweisung ist erreichbar, und ein *for_condition* ist vorhanden und weist nicht den konstanten Wert `true` auf.

### <a name="the-foreach-statement"></a>Die foreach-Anweisung

Die `foreach` -Anweisung zählt die Elemente einer Auflistung auf und führt eine eingebettete Anweisung für jedes Element der Auflistung aus.

```antlr
foreach_statement
    : 'foreach' '(' local_variable_type identifier 'in' expression ')' embedded_statement
    ;
```

Der *Typ* und der *Bezeichner* einer `foreach` -Anweisung deklarieren die ***iterations Variable*** der-Anweisung. Wenn der Bezeichner "`var`" als *local_variable_type*angegeben wird und kein Typ mit dem Namen `var` im Gültigkeitsbereich ist, wird die Iterations Variable als ***implizit typisierte Iterations Variable***bezeichnet, und ihr Typ wird als Elementtyp des `foreach` verwendet. -Anweisung, wie unten angegeben. Die Iterations Variable entspricht einer schreibgeschützten lokalen Variablen mit einem Bereich, der die eingebettete Anweisung erweitert. Während der Ausführung einer `foreach` -Anweisung stellt die Iterations Variable das Auflistungs Element dar, für das zurzeit eine Iterations Ausführung ausgeführt wird. Ein Kompilierzeitfehler tritt auf, wenn die eingebettete Anweisung versucht, die Iterations Variable (über die `++` - `--` und-Operatoren) zu ändern oder die `ref` Iterations `out` Variable als-oder-Parameter zu übergeben.

Im folgenden wird `IEnumerable`aus Gründen `IEnumerator` `System.Collections` `IEnumerable<T>` der Übersichtlichkeit,, `IEnumerator<T>` und auf die entsprechenden Typen in den-Namespaces `System.Collections.Generic`und verwiesen.

Die Kompilierzeit Verarbeitung einer foreach-Anweisung bestimmt zuerst den ***Auflistungstyp***, den ***Enumeratortyp*** und den ***Elementtyp*** des Ausdrucks. Diese Bestimmung verläuft wie folgt:

*  Wenn der Typ `X` des *Ausdrucks* ein Arraytyp ist, gibt es eine implizite Verweis Konvertierung `X` von in `IEnumerable` die-Schnitt `System.Array` Stelle (da diese Schnittstelle implementiert). Der ***Sammlungstyp*** ist `IEnumerable` die-Schnittstelle, der ***Enumeratortyp*** ist die `IEnumerator` -Schnittstelle, und der ***Elementtyp*** ist der `X`Elementtyp des Arraytyps.
*  Wenn der Typ `X` des *Ausdrucks* ist `dynamic` , gibt es eine implizite Konvertierung von einem *Ausdruck* in `IEnumerable` die-Schnittstelle ([implizite dynamische Konvertierungen](conversions.md#implicit-dynamic-conversions)). Der ***Sammlungstyp*** ist `IEnumerable` die-Schnittstelle, und der ***Enumeratortyp*** ist die `IEnumerator` -Schnittstelle. Wenn der Bezeichner "`var`" als *local_variable_type* angegeben wird, ist der ***Elementtyp*** `dynamic`; andernfalls ist er `object`.
*  Stellen Sie andernfalls fest, ob `X` der Typ über `GetEnumerator` eine geeignete Methode verfügt:
   * Führt eine Element Suche für den Typ `X` mit Bezeichnern `GetEnumerator` und ohne Typargumente durch. Wenn die Element Suche keine Entsprechung erzeugt oder eine Mehrdeutigkeit erzeugt oder eine Entsprechung erzeugt, die keine Methoden Gruppe ist, überprüfen Sie wie unten beschrieben, ob eine Aufzähl Bare-Schnittstelle vorliegt. Es wird empfohlen, dass eine Warnung ausgegeben wird, wenn die Element Suche nur eine Methoden Gruppe oder keine Entsprechung erzeugt.
   * Führt die Überladungs Auflösung mithilfe der resultierenden Methoden Gruppe und einer leeren Argumentliste durch. Wenn die Überladungs Auflösung keine anwendbaren Methoden zur Folge hat, zu einer Mehrdeutigkeit führt oder zu einer einzigen optimalen Methode führt, diese Methode jedoch entweder statisch oder nicht öffentlich ist, überprüfen Sie, wie unten beschrieben, eine Aufzähl Bare Schnittstelle. Es wird empfohlen, dass eine Warnung ausgegeben wird, wenn die Überladungs Auflösung nur eine eindeutige öffentliche Instanzmethode oder keine anwendbaren Methoden erzeugt.
   * Wenn der `E` Rückgabetyp `GetEnumerator` der Methode keine Klasse, Struktur oder Schnittstellentyp ist, wird ein Fehler erzeugt, und es werden keine weiteren Schritte ausgeführt.
   * Die Element Suche erfolgt `E` mit dem-Bezeichner `Current` und ohne Typargumente. Wenn die Member-Suche keine Entsprechung erzeugt, ist das Ergebnis ein Fehler, oder es handelt sich um ein anderes Ergebnis als eine öffentliche Instanzeigenschaft, die Lesevorgänge zulässt, es wird ein Fehler erzeugt, und es werden keine weiteren Schritte ausgeführt.
   * Die Element Suche erfolgt `E` mit dem-Bezeichner `MoveNext` und ohne Typargumente. Wenn die Member-Suche keine Entsprechung erzeugt, ist das Ergebnis ein Fehler, oder es handelt sich um ein anderes Ergebnis als eine Methoden Gruppe. es wird ein Fehler erzeugt, und es werden keine weiteren Schritte ausgeführt.
   * Die Überladungs Auflösung wird für die Methoden Gruppe mit einer leeren Argumentliste ausgeführt. Wenn die Überladungs Auflösung keine anwendbaren Methoden zur Folge hat, führt dies zu einer Mehrdeutigkeit, oder führt dies zu einer einzigen optimalen Methode, aber diese Methode ist entweder statisch oder nicht öffentlich `bool`, oder der Rückgabetyp ist nicht. es wird ein Fehler erzeugt, und es werden keine weiteren Schritte ausgeführt.
   * Der ***Auflistungstyp*** ist `X`, der ***Enumeratortyp*** ist `E`, `Current` und der ***Elementtyp*** ist der Typ der Eigenschaft.

*  Überprüfen Sie andernfalls eine Aufzähl Bare-Schnittstelle:
   * Wenn zwischen `Ti` allen Typen, für die eine implizite Konvertierung von `X` in `IEnumerable<Ti>`vorhanden ist, gibt es einen eindeutigen Typ `T` , der `T` nicht `dynamic` ist und für alle anderen `Ti` ein implizite Konvertierung von `IEnumerable<T>` in `IEnumerable<Ti>`, dann ist der Sammlungstyp die `IEnumerable<T>`-Schnittstelle, der ***Enumeratortyp*** ist die-Schnittstelle `IEnumerator<T>`, und der ***Elementtyp*** ist `T`.
   * Andernfalls wird ein Fehler erzeugt, wenn mehr als ein `T`solcher Typ vorhanden ist, und es werden keine weiteren Schritte ausgeführt.
   * Andernfalls `X` ist bei einer impliziten Konvertierung von in die `System.Collections.IEnumerable` -Schnittstelle der Sammlungstyp diese Schnittstelle, der ***Enumeratortyp*** ist die-Schnitt `System.Collections.IEnumerator`Stelle, und der ***Elementtyp*** ist. `object`.
   * Andernfalls wird ein Fehler erzeugt, und es werden keine weiteren Schritte ausgeführt.

Die oben genannten Schritte, wenn erfolgreich, führen zu einer eindeutigen `C`Generierung eines Auflistungs `E` Typs, enumeratortyps und Elementtyps `T`. Eine foreach-Anweisung des Formulars
```csharp
foreach (V v in x) embedded_statement
```
wird dann auf erweitert:
```csharp
{
    E e = ((C)(x)).GetEnumerator();
    try {
        while (e.MoveNext()) {
            V v = (V)(T)e.Current;
            embedded_statement
        }
    }
    finally {
        ... // Dispose e
    }
}
```

Die Variable `e` ist für den Ausdruck `x` oder die eingebettete Anweisung oder einen anderen Quellcode des Programms nicht sichtbar oder kann nicht darauf zugegriffen werden. Die Variable `v` ist in der eingebetteten Anweisung schreibgeschützt. Wenn keine explizite Konvertierung ([explizite Konvertierungen](conversions.md#explicit-conversions)) von `T` (dem Elementtyp) in `V` (der *local_variable_type* in der foreach-Anweisung) vorhanden ist, wird ein Fehler erzeugt, und es werden keine weiteren Schritte ausgeführt. Wenn `x` den Wert `null`hat, wird `System.NullReferenceException` zur Laufzeit eine ausgelöst.

Eine Implementierung darf eine bestimmte foreach-Anweisung anders implementieren, z. b. aus Leistungsgründen, solange das Verhalten mit der obigen Erweiterung konsistent ist.

Die Platzierung von `v` innerhalb der while-Schleife ist wichtig für die Erfassung durch eine anonyme Funktion, die in *embedded_statement*auftritt.

Zum Beispiel:
```csharp
int[] values = { 7, 9, 13 };
Action f = null;

foreach (var value in values)
{
    if (f == null) f = () => Console.WriteLine("First value: " + value);
}

f();
```
Wenn `v` außerhalb der while-Schleife deklariert wurde, wird Sie von allen Iterationen gemeinsam genutzt, und ihr Wert nach der for-Schleife wäre der endgültige `13`Wert,, der den Aufruf von `f` druckt. Da jede Iterations Variable über eine eigene Variable `v`verfügt, hält die, die von `f` in der ersten Iterationen aufgezeichnet wird, den Wert `7`, der gedruckt wird, weiterhin. (Hinweis: frühere Versionen von C# , `v` die außerhalb der while-Schleife deklariert wurden.)

Der Hauptteil des Blocks zum Schluss wird gemäß den folgenden Schritten erstellt:

*  Wenn eine implizite Konvertierung von `E` in die `System.IDisposable` -Schnittstelle erfolgt, dann
   *  Wenn `E` ein Werttyp ist, der keine NULL-Werte zulässt, wird die schließlich-Klausel auf die semantische Entsprechung von erweitert:

      ```csharp
      finally {
          ((System.IDisposable)e).Dispose();
      }
      ```

   *  Andernfalls wird die schließlich-Klausel auf die semantische Entsprechung von erweitert:

      ```csharp
      finally {
          if (e != null) ((System.IDisposable)e).Dispose();
      }
      ```

   mit der Ausnahme `E` , dass bei einem Werttyp oder einem Typparameter, der auf einen Werttyp instanziiert `e` wird `System.IDisposable` , die Umwandlung von in nicht dazu führt, dass Boxing auftritt.

*  Andernfalls, wenn `E` ein versiegelter Typ ist, wird die letzte-Klausel auf einen leeren Block erweitert:

   ```csharp
   finally {
   }
   ```

*  Andernfalls wird die schließlich-Klausel auf erweitert:

   ```csharp
   finally {
       System.IDisposable d = e as System.IDisposable;
       if (d != null) d.Dispose();
   }
   ```    

   Die lokale Variable `d` ist für keinen Benutzercode sichtbar oder nicht verfügbar. Dies hat insbesondere keinen Konflikt mit einer anderen Variablen, deren Bereich den letzten Block enthält.

Die Reihenfolge, `foreach` in der die Elemente eines Arrays durchlaufen werden, lautet wie folgt: Für eindimensionale Arrays werden Elemente in steigender Index Reihenfolge durchlaufen, beginnend mit `0` Index und endende `Length - 1`mit Index. Bei mehrdimensionalen Arrays werden Elemente so durchlaufen, dass die Indizes der äußersten rechten Dimension zuerst angehoben werden, dann die nächste linke Dimension usw.

Im folgenden Beispiel werden die einzelnen Werte in der Reihenfolge der Elemente in einem zweidimensionalen Array ausgegeben:
```csharp
using System;

class Test
{
    static void Main() {
        double[,] values = {
            {1.2, 2.3, 3.4, 4.5},
            {5.6, 6.7, 7.8, 8.9}
        };

        foreach (double elementValue in values)
            Console.Write("{0} ", elementValue);

        Console.WriteLine();
    }
}
```
Die erstellte Ausgabe lautet wie folgt:
```console
1.2 2.3 3.4 4.5 5.6 6.7 7.8 8.9
```

Im Beispiel
```csharp
int[] numbers = { 1, 3, 5, 7, 9 };
foreach (var n in numbers) Console.WriteLine(n);
```
der Typ von `n` wird als `int`, der Elementtyp von `numbers`, abgeleitet.

## <a name="jump-statements"></a>Sprunganweisungen

Jump-Anweisungen: Bedingungs Übertragung der Steuerung.

```antlr
jump_statement
    : break_statement
    | continue_statement
    | goto_statement
    | return_statement
    | throw_statement
    ;
```

Der Speicherort, an den eine Jump-Anweisung die Steuerung überträgt, wird als ***Ziel*** der Jump-Anweisung bezeichnet.

Wenn eine Jump-Anweisung innerhalb eines-Blocks auftritt und sich das Ziel dieser Jump-Anweisung außerhalb dieses Blocks befindet, wird die Jump-Anweisung zum ***Beenden*** des Blocks bezeichnet. Während eine Jump-Anweisung die Steuerung von einem-Block übertragen kann, kann Sie die Steuerung niemals in einen-Block übertragen.

Die Ausführung von Jump-Anweisungen wird durch das vorhanden sein `try` von dazwischenliegenden Anweisungen erschwert. Wenn solche `try` Anweisungen fehlen, überträgt eine Jump-Anweisung bedingungslos die Steuerung von der Jump-Anweisung an das Ziel. Wenn diese dazwischenliegenden `try` Anweisungen vorhanden sind, ist die Ausführung komplexer. Wenn die Jump-Anweisung einen oder mehrere `try` Blöcke mit zugeordneten `finally` Blöcken beendet, wird die Steuerung anfänglich an den `finally` Block der `try` innersten Anweisung übertragen. Wenn und wenn das Steuerelement den Endpunkt eines `finally` -Blocks erreicht, wird die Steuerung an den `finally` -Block der nächsten einschließenden `try` Anweisung übertragen. Dieser Vorgang wird wiederholt, `finally` bis die Blöcke aller `try` dazwischen liegenden Anweisungen ausgeführt wurden.

Im Beispiel
```csharp
using System;

class Test
{
    static void Main() {
        while (true) {
            try {
                try {
                    Console.WriteLine("Before break");
                    break;
                }
                finally {
                    Console.WriteLine("Innermost finally block");
                }
            }
            finally {
                Console.WriteLine("Outermost finally block");
            }
        }
        Console.WriteLine("After break");
    }
}
```
die `finally` Blöcke, die zwei `try` Anweisungen zugeordnet sind, werden ausgeführt, bevor die Steuerung an das Ziel der Jump-Anweisung übertragen wird.

Die erstellte Ausgabe lautet wie folgt:
```console
Before break
Innermost finally block
Outermost finally block
After break
```

### <a name="the-break-statement"></a>Die Break-Anweisung

Die `break` -Anweisung beendet die nächste `while`einschließende `do` `switch` `for`-,- `foreach` ,-,-oder-Anweisung.

```antlr
break_statement
    : 'break' ';'
    ;
```

Das `break` Ziel einer-Anweisung ist der Endpunkt der nächsten einschließenden `switch`, `while`, `do` `for`, oder `foreach` -Anweisung. Wenn eine `break` -Anweisung nicht durch eine `switch`-, `while` `do` `for`-,-, `foreach` -oder-Anweisung eingeschlossen ist, tritt ein Kompilierzeitfehler auf.

Wenn mehrere `switch`- `while` `foreach` `break` , `do`-,-,-oder-Anweisungen ineinander geschachtelt sind, gilt eine-Anweisung nur für die innerste-Anweisung. `for` Um die Steuerung auf mehrere Schachtelungs Ebenen `goto` zu übertragen, muss eine-Anweisung ([die GoTo-Anweisung](statements.md#the-goto-statement)) verwendet werden.

Eine `break` -Anweisung kann einen `finally` Block nicht beenden ([try-Anweisung](statements.md#the-try-statement)). Wenn eine `break` -Anweisung innerhalb eines `finally` -Blocks auftritt `break` , muss sich das Ziel der-Anweisung innerhalb `finally` desselben-Blocks befinden; andernfalls tritt ein Kompilierzeitfehler auf.

Eine `break` -Anweisung wird wie folgt ausgeführt:

*  Wenn die `break` Anweisung einen oder mehrere `try` Blöcke mit zugeordneten `finally` Blöcken beendet, wird die Steuerung anfänglich `finally` an den Block der innersten `try` Anweisung übertragen. Wenn und wenn das Steuerelement den Endpunkt eines `finally` -Blocks erreicht, wird die Steuerung an den `finally` -Block der nächsten einschließenden `try` Anweisung übertragen. Dieser Vorgang wird wiederholt, `finally` bis die Blöcke aller `try` dazwischen liegenden Anweisungen ausgeführt wurden.
*  Das Steuerelement wird an das Ziel der `break` -Anweisung übertragen.

Da eine `break` Anweisung die Steuerung an andere Stellen überträgt, ist der Endpunkt `break` einer Anweisung nie erreichbar.

### <a name="the-continue-statement"></a>Die Continue-Anweisung

Mit `continue` der-Anweisung wird eine neue Iterations Anweisung der nächsten `while` `do`einschließenden `for`- `foreach` ,-,-oder-Anweisung gestartet.

```antlr
continue_statement
    : 'continue' ';'
    ;
```

Das `continue` Ziel einer-Anweisung ist der Endpunkt der eingebetteten Anweisung der nächsten `do`einschließenden `while` `for`-,-,-oder `foreach` -Anweisung. Wenn eine `continue` -Anweisung nicht durch eine `while`-, `do` `for`-,- `foreach` oder-Anweisung eingeschlossen ist, tritt ein Kompilierzeitfehler auf.

Wenn mehrere `while`- `do` `foreach` ,-,-oder-Anweisungen ineinander geschachtelt sind `continue` , gilt eine-Anweisung nur für die innerste-Anweisung. `for` Um die Steuerung auf mehrere Schachtelungs Ebenen `goto` zu übertragen, muss eine-Anweisung ([die GoTo-Anweisung](statements.md#the-goto-statement)) verwendet werden.

Eine `continue` -Anweisung kann einen `finally` Block nicht beenden ([try-Anweisung](statements.md#the-try-statement)). Wenn eine `continue` -Anweisung innerhalb eines `finally` -Blocks auftritt `continue` , muss sich das Ziel der-Anweisung innerhalb `finally` desselben-Blocks befinden; andernfalls tritt ein Kompilierzeitfehler auf.

Eine `continue` -Anweisung wird wie folgt ausgeführt:

*  Wenn die `continue` Anweisung einen oder mehrere `try` Blöcke mit zugeordneten `finally` Blöcken beendet, wird die Steuerung anfänglich `finally` an den Block der innersten `try` Anweisung übertragen. Wenn und wenn das Steuerelement den Endpunkt eines `finally` -Blocks erreicht, wird die Steuerung an den `finally` -Block der nächsten einschließenden `try` Anweisung übertragen. Dieser Vorgang wird wiederholt, `finally` bis die Blöcke aller `try` dazwischen liegenden Anweisungen ausgeführt wurden.
*  Das Steuerelement wird an das Ziel der `continue` -Anweisung übertragen.

Da eine `continue` Anweisung die Steuerung an andere Stellen überträgt, ist der Endpunkt `continue` einer Anweisung nie erreichbar.

### <a name="the-goto-statement"></a>Die goto-Anweisung

Mit `goto` der-Anweisung wird die Steuerung an eine Anweisung übertragen, die durch eine Bezeichnung gekennzeichnet ist.

```antlr
goto_statement
    : 'goto' identifier ';'
    | 'goto' 'case' constant_expression ';'
    | 'goto' 'default' ';'
    ;
```

Das Ziel einer `goto` *bezeichneranweisung* ist die bezeichnete Anweisung mit der angegebenen Bezeichnung. Wenn eine Bezeichnung mit dem angegebenen Namen nicht im aktuellen Funktionsmember vorhanden ist oder die `goto` Anweisung nicht innerhalb des Bereichs der Bezeichnung liegt, tritt ein Kompilierzeitfehler auf. Diese Regel ermöglicht die Verwendung einer `goto` -Anweisung, um die Steuerung aus einem nicht in einen Bereich eingefügten Bereich zu übertragen, jedoch nicht in einen eingefügten Bereich. Im Beispiel
```csharp
using System;

class Test
{
    static void Main(string[] args) {
        string[,] table = {
            {"Red", "Blue", "Green"},
            {"Monday", "Wednesday", "Friday"}
        };

        foreach (string str in args) {
            int row, colm;
            for (row = 0; row <= 1; ++row)
                for (colm = 0; colm <= 2; ++colm)
                    if (str == table[row,colm])
                         goto done;

            Console.WriteLine("{0} not found", str);
            continue;
    done:
            Console.WriteLine("Found {0} at [{1}][{2}]", str, row, colm);
        }
    }
}
```
eine `goto` -Anweisung wird verwendet, um die Steuerung aus einem in einem Bereich befindlichen Bereich zu übertragen.

Das Ziel `goto case` einer-Anweisung ist die Anweisungs Liste in der unmittelbar `switch` einschließenden Anweisung ([Switch-Anweisung](statements.md#the-switch-statement)), die `case` eine Bezeichnung mit dem angegebenen konstanten Wert enthält. , Wenn die `goto case`-Anweisung nicht durch eine `switch`-Anweisung eingeschlossen ist,, wenn die *constant_expression* nicht implizit konvertierbar ist ([implizite Konvertierungen](conversions.md#implicit-conversions)), in den regierenden Typ der nächstgelegenen `switch`-Anweisung oder, wenn die nächste einschließende `switch`-Anweisung enthält keine `case`-Bezeichnung mit dem angegebenen konstanten Wert. ein Kompilierzeitfehler tritt auf.

Das Ziel `goto default` einer-Anweisung ist die Anweisungs Liste in der unmittelbar `switch` einschließenden Anweisung ([Switch-Anweisung](statements.md#the-switch-statement)), die `default` eine Bezeichnung enthält. Wenn die `goto default` Anweisung nicht durch eine `switch` -Anweisung eingeschlossen ist, oder wenn die nächste einschließende `switch` Anweisung keine `default` Bezeichnung enthält, tritt ein Kompilierzeitfehler auf.

Eine `goto` -Anweisung kann einen `finally` Block nicht beenden ([try-Anweisung](statements.md#the-try-statement)). Wenn eine `goto` -Anweisung innerhalb eines `finally` -Blocks auftritt `goto` , muss sich das Ziel der-Anweisung innerhalb `finally` desselben Blocks befinden, andernfalls tritt ein Kompilierzeitfehler auf.

Eine `goto` -Anweisung wird wie folgt ausgeführt:

*  Wenn die `goto` Anweisung einen oder mehrere `try` Blöcke mit zugeordneten `finally` Blöcken beendet, wird die Steuerung anfänglich `finally` an den Block der innersten `try` Anweisung übertragen. Wenn und wenn das Steuerelement den Endpunkt eines `finally` -Blocks erreicht, wird die Steuerung an den `finally` -Block der nächsten einschließenden `try` Anweisung übertragen. Dieser Vorgang wird wiederholt, `finally` bis die Blöcke aller `try` dazwischen liegenden Anweisungen ausgeführt wurden.
*  Das Steuerelement wird an das Ziel der `goto` -Anweisung übertragen.

Da eine `goto` Anweisung die Steuerung an andere Stellen überträgt, ist der Endpunkt `goto` einer Anweisung nie erreichbar.

### <a name="the-return-statement"></a>Return-Anweisung

Die `return` -Anweisung gibt die Steuerung an den aktuellen Aufrufer der Funktion `return` zurück, in der die-Anweisung angezeigt wird.

```antlr
return_statement
    : 'return' expression? ';'
    ;
```

Eine `return` -Anweisung ohne Ausdruck kann nur in einem Funktionsmember verwendet werden, der keinen Wert berechnet, d. h. eine Methode mit dem Ergebnistyp ([Methoden Text](classes.md#method-body)) `void`, `set` der-Accessor einer Eigenschaft oder eines Indexers, der `add` - `remove` und-Accessoren eines Ereignisses, eines Instanzkonstruktors, eines statischen Konstruktors oder eines Dekonstruktors.

Eine `return` -Anweisung mit einem-Ausdruck kann nur in einem Funktionsmember verwendet werden, der einen-Wert berechnet, d. h. eine Methode mit einem nicht leeren `get` Ergebnistyp, den-Accessor einer Eigenschaft oder einen Indexer oder einen benutzerdefinierten Operator. Eine implizite Konvertierung ([implizite Konvertierungen](conversions.md#implicit-conversions)) muss vom Typ des Ausdrucks bis zum Rückgabetyp des enthaltenden Funktionsmembers vorhanden sein.

Return-Anweisungen können auch im Text der anonymen Funktions Ausdrücke ([Anonyme Funktions](expressions.md#anonymous-function-expressions)Ausdrücke) verwendet werden und daran beteiligt sein, zu bestimmen, welche Konvertierungen für diese Funktionen vorhanden sind.

Es handelt sich um einen Kompilierzeitfehler `return` , damit eine-Anweisung `finally` in einem-Block ([der try-Anweisung](statements.md#the-try-statement)) angezeigt wird.

Eine `return` -Anweisung wird wie folgt ausgeführt:

*  Wenn die `return` Anweisung einen Ausdruck angibt, wird der Ausdruck ausgewertet, und der resultierende Wert wird durch eine implizite Konvertierung in den Rückgabetyp der enthaltenden Funktion konvertiert. Das Ergebnis der Konvertierung wird der Ergebniswert, der von der-Funktion erzeugt wird.
*  Wenn die `return` -Anweisung von einem oder mehreren `try` -oder `catch` -Blöcken mit `finally` zugeordneten-Blöcken eingeschlossen wird, wird `finally` die Steuerung anfänglich an `try` den-Block der innersten-Anweisung übertragen. Wenn und wenn das Steuerelement den Endpunkt eines `finally` -Blocks erreicht, wird die Steuerung an den `finally` -Block der nächsten einschließenden `try` Anweisung übertragen. Dieser Vorgang wird wiederholt, `finally` bis die Blöcke aller einschließenden `try` Anweisungen ausgeführt wurden.
*  Wenn die enthaltende Funktion keine Async-Funktion ist, wird die Steuerung an den Aufrufer der enthaltenden Funktion zusammen mit dem Ergebniswert zurückgegeben, falls vorhanden.
*  Wenn die enthaltende Funktion eine Async-Funktion ist, wird die Steuerung an den aktuellen Aufrufer zurückgegeben, und der Ergebniswert wird ggf. in der Rückgabe Aufgabe aufgezeichnet, wie in ([Enumeratorschnittstellen](classes.md#enumerator-interfaces)) beschrieben.

Da eine `return` Anweisung die Steuerung an andere Stellen überträgt, ist der Endpunkt `return` einer Anweisung nie erreichbar.

### <a name="the-throw-statement"></a>Die throw-Anweisung

Die `throw` -Anweisung löst eine Ausnahme aus.

```antlr
throw_statement
    : 'throw' expression? ';'
    ;
```

Eine `throw` -Anweisung mit einem Ausdruck löst den Wert aus, der durch Auswerten des Ausdrucks erzeugt wird. Der Ausdruck muss einen Wert des Klassen Typs `System.Exception`, eines Klassen Typs, der von `System.Exception` abgeleitet ist `System.Exception` , oder von einem typparametertyp mit (oder einer Unterklasse) als effektive Basisklasse bezeichnen. Wenn die Auswertung des Ausdrucks erzeugt `null`, wird `System.NullReferenceException` stattdessen eine ausgelöst.

Eine `throw` -Anweisung ohne Ausdruck kann nur in einem `catch` -Block verwendet werden. in diesem Fall löst die-Anweisung die Ausnahme erneut aus, die zurzeit von `catch` diesem Block behandelt wird.

Da eine `throw` Anweisung die Steuerung an andere Stellen überträgt, ist der Endpunkt `throw` einer Anweisung nie erreichbar.

Wenn eine Ausnahme ausgelöst wird, wird die Steuerung an die erste `catch` Klausel in einer einschließenden `try` Anweisung übertragen, die die Ausnahme behandeln kann. Der Prozess, der ab dem Zeitpunkt der Ausnahme ausgelöst wird, der zum Zeitpunkt der Übertragung der Steuerung an einen geeigneten Ausnahmehandler ausgelöst wird, wird als ***Ausnahme***Weitergabe bezeichnet. Die Weitergabe einer Ausnahme besteht aus dem wiederholten Auswerten der folgenden `catch` Schritte, bis eine Klausel gefunden wird, die der Ausnahme entspricht. In dieser Beschreibung ist der ***throw-Punkt*** anfänglich der Speicherort, an dem die Ausnahme ausgelöst wird.

*  Im aktuellen Funktionsmember wird jede `try` Anweisung, die den Throw-Punkt einschließt, untersucht. Für jede Anweisung `S`, beginnend mit der innersten `try` -Anweisung und mit der äußersten `try` -Anweisung, werden die folgenden Schritte ausgewertet:

   * Wenn der `try` -Block `S` von den Throw-Punkt einschließt und S über eine oder mehrere `catch` Klauseln verfügt, `catch` werden die Klauseln in der Reihenfolge der Darstellung untersucht, um nach einem geeigneten Handler für die Ausnahme zu suchen. Dies erfolgt gemäß den in Abschnitt [die try-Anweisung](statements.md#the-try-statement). Wenn eine überein `catch` stimmende Klausel gefunden wird, wird die Ausnahme Weitergabe durch übertragen der Steuerung an `catch` den Block dieser Klausel abgeschlossen.

   * Andernfalls wird die Steuerung `try` an den `S` `finally` `S` `catch` -`finally` Block übertragen, wenn der-Block oder ein-Block den Throw-Punkt einschließt und wenn über einen-Block verfügt. Wenn der `finally` -Block eine andere Ausnahme auslöst, wird die Verarbeitung der aktuellen Ausnahme beendet. Andernfalls wird die Verarbeitung der aktuellen Ausnahme fortgesetzt, `finally` wenn die Steuerung den Endpunkt des Blocks erreicht.

*  Wenn ein Ausnahmehandler im aktuellen Funktionsaufruf nicht gefunden wurde, wird der Funktionsaufruf beendet, und eine der folgenden Aktionen wird ausgeführt:

   * Wenn die aktuelle Funktion nicht Async ist, werden die obigen Schritte für den Aufrufer der Funktion mit einem Throw-Punkt wiederholt, der der Anweisung entspricht, aus der der Funktionsmember aufgerufen wurde.

   * Wenn die aktuelle Funktion Async ist und die Aufgabe zurückgibt, wird die Ausnahme in der Rückgabe Aufgabe aufgezeichnet, die in einen fehlerhaften oder abgebrochenen Zustand versetzt wird, wie in [Enumeratorschnittstellen](classes.md#enumerator-interfaces)beschrieben.

   * Wenn die aktuelle Funktion Async ist und void zurückgibt, wird der Synchronisierungs Kontext des aktuellen Threads wie in [Aufzähl baren Schnittstellen](classes.md#enumerable-interfaces)beschrieben benachrichtigt.

*  Wenn die Ausnahme Verarbeitung alle Funktionselement Aufrufe im aktuellen Thread beendet und angibt, dass der Thread keinen Handler für die Ausnahme aufweist, wird der Thread selbst beendet. Die Auswirkungen dieser Beendigung sind Implementierungs definiert.

## <a name="the-try-statement"></a>Die try-Anweisung

Die `try` -Anweisung stellt einen Mechanismus zum Abfangen von Ausnahmen bereit, die während der Ausführung eines-Blocks auftreten. Außerdem bietet die `try` -Anweisung die Möglichkeit, einen Codeblock anzugeben, der immer ausgeführt wird, wenn das Steuer `try` Element die-Anweisung verlässt.

```antlr
try_statement
    : 'try' block catch_clause+
    | 'try' block finally_clause
    | 'try' block catch_clause+ finally_clause
    ;

catch_clause
    : 'catch' exception_specifier? exception_filter?  block
    ;

exception_specifier
    : '(' type identifier? ')'
    ;

exception_filter
    : 'when' '(' expression ')'
    ;

finally_clause
    : 'finally' block
    ;
```

Es gibt drei mögliche Formen von `try` -Anweisungen:

*  Ein `try` -Block, gefolgt von einem `catch` oder mehreren-Blöcken.
*  Ein `try` -Block, gefolgt `finally` von einem-Block.
*  Ein `try` -Block, gefolgt von einem `catch` oder mehreren-Blöcken `finally` , gefolgt von einem-Block.

Wenn eine `catch`-Klausel ein *exception_specifier*-Element angibt, muss der Typ `System.Exception`, ein Typ, der von `System.Exception` abgeleitet ist, oder ein typparametertyp sein, der über `System.Exception` (oder eine Unterklasse) als effektive Basisklasse verfügt.

Wenn eine `catch`-Klausel sowohl eine *exception_specifier* mit einem *Bezeichner*angibt, wird eine ***Ausnahme Variable*** mit dem angegebenen Namen und Typ deklariert. Die Exception-Variable entspricht einer lokalen Variablen mit einem Bereich, der die `catch` -Klausel erweitert. Während der Ausführung des *exception_filter* und des *Blocks*stellt die Ausnahme Variable die derzeit behandelte Ausnahme dar. Zum Zweck der eindeutigen Zuweisungs Überprüfung wird die Ausnahme Variable als definitiv in Ihrem gesamten Bereich zugewiesen.

Es ist `catch` nicht möglich, auf das Ausnahme Objekt im Filter und `catch` Block zuzugreifen, es sei denn, eine Klausel enthält einen Ausnahme Variablennamen.

Eine `catch`-Klausel, die keine *exception_specifier* angibt, wird als allgemeine `catch`-Klausel bezeichnet.

Einige Programmiersprachen unterstützen möglicherweise Ausnahmen, die nicht als von `System.Exception`abgeleitete Objekt darstellbar sind, obwohl solche Ausnahmen nie durch C# Code generiert werden könnten. Eine allgemeine `catch` Klausel kann verwendet werden, um solche Ausnahmen abzufangen. Daher unterscheidet sich `catch` eine allgemeine Klausel semantisch von einer, die den Typ `System.Exception`angibt, da der erste auch Ausnahmen von anderen Sprachen abfängt.

Um einen Handler für eine Ausnahme zu finden, `catch` werden Klauseln in lexikalischer Reihenfolge untersucht. Wenn eine `catch` Klausel einen Typ, aber keinen Ausnahme Filter angibt, ist dies ein Kompilierzeitfehler für eine `catch` spätere Klausel in derselben `try` Anweisung, um einen Typ anzugeben, der dem Typ entspricht oder von diesem abgeleitet ist. Wenn eine `catch` Klausel keinen Typ und keinen Filter angibt, muss es sich um die `catch` letzte Klausel für `try` diese Anweisung handeln.

Innerhalb eines `catch` -Blocks kann `throw` eine-Anweisung ([die throw-Anweisung](statements.md#the-throw-statement)) ohne Ausdruck verwendet werden, um `catch` die vom-Block aufgefangene Ausnahme erneut auszulösen. Bei Zuweisungen zu einer Ausnahme Variablen wird die Ausnahme, die erneut ausgelöst wird, nicht geändert.

Im Beispiel
```csharp
using System;

class Test
{
    static void F() {
        try {
            G();
        }
        catch (Exception e) {
            Console.WriteLine("Exception in F: " + e.Message);
            e = new Exception("F");
            throw;                // re-throw
        }
    }

    static void G() {
        throw new Exception("G");
    }

    static void Main() {
        try {
            F();
        }
        catch (Exception e) {
            Console.WriteLine("Exception in Main: " + e.Message);
        }
    }
}
```
die Methode `F` fängt eine Ausnahme ab, schreibt einige Diagnoseinformationen in die Konsole, ändert die Ausnahme Variable und löst die Ausnahme erneut aus. Die Ausnahme, die erneut ausgelöst wird, ist die ursprüngliche Ausnahme, sodass die Ausgabe erzeugt wird:
```console
Exception in F: G
Exception in Main: G
```

Wenn der erste catch-Block `e` ausgelöst wurde und die aktuelle Ausnahme nicht erneut ausgelöst wurde, sieht die Ausgabe wie folgt aus:
```console
Exception in F: G
Exception in Main: F
```

Es handelt sich um einen Kompilierzeitfehler `break`für `continue`eine- `goto` ,-oder-Anweisung zum über `finally` tragen der Steuerung aus einem-Block. Wenn eine `break`- `continue`,- `goto` oder-Anweisung in `finally` einem-Block auftritt, muss sich das Ziel der-Anweisung `finally` innerhalb desselben Blocks befinden. andernfalls tritt ein Kompilierzeitfehler auf.

Es handelt sich um einen Kompilierzeitfehler `return` , damit eine-Anweisung `finally` in einem-Block auftritt.

Eine `try` -Anweisung wird wie folgt ausgeführt:

*  Das Steuerelement wird an `try` den-Block übertragen.
*  Wenn und wenn das Steuerelement den Endpunkt des `try` Blocks erreicht:
   *  Wenn die `try` Anweisung über einen `finally` -Block verfügt `finally` , wird der-Block ausgeführt.
   *  Das Steuerelement wird an den Endpunkt `try` der Anweisung übertragen.

*  Wenn eine Ausnahme während der Ausführung `try` `try` des Blocks an die Anweisung weitergegeben wird:
   *  Die `catch` Klauseln werden ggf. in der Reihenfolge ihrer Darstellung untersucht, um einen geeigneten Handler für die Ausnahme zu suchen. Wenn eine `catch` -Klausel keinen Typ angibt oder den Ausnahmetyp oder einen Basistyp des Ausnahme Typs angibt:
      *  Wenn die `catch` -Klausel eine Exception-Variable deklariert, wird das Ausnahme Objekt der Ausnahme Variablen zugewiesen.
      *  Wenn die `catch` -Klausel einen Ausnahme Filter deklariert, wird der Filter ausgewertet. Wenn ausgewertet `false`wird, ist die catch-Klausel keine Entsprechung, und die Suche wird durch alle nachfolg `catch` enden Klauseln für einen geeigneten Handler fortgesetzt.
      *  Andernfalls wird die `catch` -Klausel als Übereinstimmung angesehen, und die Steuerung wird an den `catch` entsprechenden-Block übertragen.
      *  Wenn und wenn das Steuerelement den Endpunkt des `catch` Blocks erreicht:
         * Wenn die `try` Anweisung über einen `finally` -Block verfügt `finally` , wird der-Block ausgeführt.
         * Das Steuerelement wird an den Endpunkt `try` der Anweisung übertragen.
      *  Wenn eine Ausnahme während der Ausführung `try` `catch` des Blocks an die Anweisung weitergegeben wird:
         *  Wenn die `try` Anweisung über einen `finally` -Block verfügt `finally` , wird der-Block ausgeführt.
         *  Die Ausnahme wird an die nächste einschließende `try` Anweisung weitergegeben.
   *  Wenn die `try` Anweisung keine `catch` Klauseln aufweist oder wenn keine `catch` Klausel mit der Ausnahme übereinstimmt:
      *  Wenn die `try` Anweisung über einen `finally` -Block verfügt `finally` , wird der-Block ausgeführt.
      *  Die Ausnahme wird an die nächste einschließende `try` Anweisung weitergegeben.

Die-Anweisungen eines `finally` -Blocks werden immer ausgeführt, wenn die `try` Steuerung eine-Anweisung verlässt. Dies gilt unabhängig davon, ob die Steuerung aufgrund der normalen Ausführung `break`aufgrund der Ausführung einer- `goto`, `continue`-,-oder `return` -Anweisung oder als Ergebnis der Weitergabe einer Ausnahme aus der `try` an.

Wenn während der Ausführung `finally` eines-Blocks eine Ausnahme ausgelöst wird und nicht innerhalb desselben letzten Blocks abgefangen wird, wird die Ausnahme an die nächste einschließende `try` Anweisung weitergegeben. Wenn eine andere Ausnahme gerade weitergegeben wurde, geht diese Ausnahme verloren. Der Prozess der Weitergabe einer Ausnahme wird weiter unten in der Beschreibung der `throw` Anweisung ([der throw-Anweisung](statements.md#the-throw-statement)) erläutert.

Der `try` -Block einer `try` -Anweisung ist erreichbar, `try` wenn die-Anweisung erreichbar ist.

Ein `catch` Block `try` einer-Anweisung ist erreichbar, wenn die-Anweisung erreichbar ist. `try`

Der `finally` -Block einer `try` -Anweisung ist erreichbar, `try` wenn die-Anweisung erreichbar ist.

Der Endpunkt einer `try` -Anweisung ist erreichbar, wenn beide der folgenden Punkte zutreffen:

*  Der Endpunkt des `try` Blocks ist erreichbar, oder der Endpunkt von mindestens einem `catch` Block ist erreichbar.
*  Wenn ein `finally` -Block vorhanden ist, ist der Endpunkt `finally` des-Blocks erreichbar.

## <a name="the-checked-and-unchecked-statements"></a>Die aktivierten und deaktivierten Anweisungen

Die `checked` - `unchecked` und-Anweisungen werden verwendet, um den ***Überlauf Überprüfungs Kontext*** für arithmetische Operationen im ganzzahligen Typ und Konvertierungen zu steuern.

```antlr
checked_statement
    : 'checked' block
    ;

unchecked_statement
    : 'unchecked' block
    ;
```

Die `checked` -Anweisung bewirkt, dass alle Ausdrücke im- *Block* in einem überprüften Kontext ausgewertet werden `unchecked` , und die-Anweisung bewirkt, dass alle Ausdrücke im *Block* in einem nicht überprüften Kontext ausgewertet werden.

Die `checked` - `unchecked` und-Anweisungen entsprechen genau den `checked` Operatoren und (die aktivierten und deaktivierten[Operatoren](expressions.md#the-checked-and-unchecked-operators)), mit dem Unterschied, dass Sie anstelle von Ausdrücken an- `unchecked` Blöcken arbeiten.

## <a name="the-lock-statement"></a>Die Lock-Anweisung

Die `lock` -Anweisung ruft die Sperre für den gegenseitigen Ausschluss für ein bestimmtes Objekt ab, führt eine-Anweisung aus und gibt dann die Sperre frei.

```antlr
lock_statement
    : 'lock' '(' expression ')' embedded_statement
    ;
```

Der Ausdruck einer `lock`-Anweisung muss einen Wert eines Typs bezeichnen, der bekanntermaßen *reference_type*ist. Für den Ausdruck einer `lock`-Anweisung wird nie eine implizite Boxing-Konvertierung ([Boxing-Konvertierungen](conversions.md#boxing-conversions)) ausgeführt, und daher ist es ein Kompilierzeitfehler, wenn der Ausdruck den Wert eines *value_type*angibt.

Eine `lock` -Anweisung der Form
```csharp
lock (x) ...
```
Wenn `x` ein Ausdruck eines *reference_type*ist, ist genau Äquivalent zu
```csharp
bool __lockWasTaken = false;
try {
    System.Threading.Monitor.Enter(x, ref __lockWasTaken);
    ...
}
finally {
    if (__lockWasTaken) System.Threading.Monitor.Exit(x);
}
```
außer dass `x` nur einmal überprüft wird.

Während eine gegenseitige Ausschluss Sperre aufrechterhalten wird, kann der Code, der im selben Ausführungs Thread ausgeführt wird, auch die Sperre abrufen und freigeben. Der Code, der in anderen Threads ausgeführt wird, wird jedoch blockiert, bis die Sperre aufgehoben wird.

Das `System.Type` Sperren von Objekten, um den Zugriff auf statische Daten zu synchronisieren, wird nicht empfohlen. Anderer Code kann denselben Typ sperren, was zu einem Deadlock führen kann. Ein besserer Ansatz besteht darin, den Zugriff auf statische Daten zu synchronisieren, indem ein privates statisches Objekt gesperrt wird. Zum Beispiel:
```csharp
class Cache
{
    private static readonly object synchronizationObject = new object();

    public static void Add(object x) {
        lock (Cache.synchronizationObject) {
            ...
        }
    }

    public static void Remove(object x) {
        lock (Cache.synchronizationObject) {
            ...
        }
    }
}
```

## <a name="the-using-statement"></a>Die using-Anweisung

Die `using` -Anweisung ruft eine oder mehrere Ressourcen ab, führt eine-Anweisung aus und verwirft dann die Ressource.

```antlr
using_statement
    : 'using' '(' resource_acquisition ')' embedded_statement
    ;

resource_acquisition
    : local_variable_declaration
    | expression
    ;
```

Eine ***Ressource*** ist eine Klasse oder Struktur, die `System.IDisposable`implementiert, die eine einzelne Parameter lose Methode namens `Dispose`enthält. Code, der eine Ressource verwendet, kann `Dispose` aufzurufen, um anzugeben, dass die Ressource nicht mehr benötigt wird. Wenn `Dispose` nicht aufgerufen wird, wird die automatische Entfernung schließlich aufgrund Garbage Collection ausgelöst.

Wenn die Form von *resource_acquisition* *local_variable_declaration* ist, muss der Typ des *local_variable_declaration* entweder `dynamic` oder ein Typ sein, der implizit in `System.IDisposable` konvertiert werden kann. Wenn die Form von *resource_acquisition* *Expression* ist, muss dieser Ausdruck implizit in `System.IDisposable` konvertiert werden können.

Lokale Variablen, die in einem *resource_acquisition* deklariert werden, sind schreibgeschützt und müssen einen Initialisierer enthalten. Ein Kompilierzeitfehler tritt auf, wenn die eingebettete Anweisung versucht, diese lokalen Variablen (über die `++` -und-Operatoren) zu ändern, die Adresse dieser Variablen zu `ref` über `out` nehmen oder Sie als-oder- `--` Parameter zu übergeben.

Eine `using` -Anweisung wird in drei Teile übersetzt: Beschaffung, Verwendung und Entsorgung. Die Verwendung der Ressource ist implizit in eine `try` -Anweisung eingeschlossen, die eine `finally` -Klausel einschließt. Diese `finally` Klausel verwirft die Ressource. Wenn eine `null` Ressource `Dispose` abgerufen wird, wird kein-Rückruf durchgeführt, und es wird keine Ausnahme ausgelöst. Wenn die Ressource vom Typ `dynamic` ist, wird Sie dynamisch durch eine implizite dynamische Konvertierung ([implizite dynamische](conversions.md#implicit-dynamic-conversions) `IDisposable` Konvertierungen) in in den Erwerb konvertiert, um sicherzustellen, dass die Konvertierung erfolgreich war, bevor die Verwendung und Ihnen.

Eine `using` -Anweisung der Form
```csharp
using (ResourceType resource = expression) statement
```
entspricht einer von drei möglichen Erweiterungen. Wenn `ResourceType` ein Werttyp ist, der keine NULL-Werte zulässt, ist die Erweiterung
```csharp
{
    ResourceType resource = expression;
    try {
        statement;
    }
    finally {
        ((IDisposable)resource).Dispose();
    }
}
```

Andernfalls: Wenn `ResourceType` ein Werte zulässt-Werttyp oder ein anderer Verweistyp `dynamic`als ist, wird die Erweiterung
```csharp
{
    ResourceType resource = expression;
    try {
        statement;
    }
    finally {
        if (resource != null) ((IDisposable)resource).Dispose();
    }
}
```

Andernfalls ist die `ResourceType` Erweiterung `dynamic`, wenn den Wert hat.
```csharp
{
    ResourceType resource = expression;
    IDisposable d = (IDisposable)resource;
    try {
        statement;
    }
    finally {
        if (d != null) d.Dispose();
    }
}
```

Bei beiden Erweiterungen ist die `resource` -Variable in der eingebetteten-Anweisung schreibgeschützt, `d` und die-Variable ist in der eingebetteten-Anweisung nicht verfügbar, und Sie ist nicht sichtbar.

Eine Implementierung darf eine angegebene using-Anweisung anders implementieren, z. b. aus Leistungsgründen, solange das Verhalten mit der obigen Erweiterung konsistent ist.

Eine `using` -Anweisung der Form
```csharp
using (expression) statement
```
hat dieselben drei möglichen Erweiterungen. In diesem Fall `ResourceType` ist implizit der Kompilier Zeittyp `expression`von, wenn er über einen verfügt. Andernfalls wird die `IDisposable` -Schnittstelle selbst `ResourceType`als verwendet. Auf `resource` die-Variable kann nicht zugegriffen werden, und die eingebettete-Anweisung ist unsichtbar.

Wenn ein *resource_acquisition* das Format eines *local_variable_declaration*annimmt, ist es möglich, mehrere Ressourcen eines bestimmten Typs zu erhalten. Eine `using` -Anweisung der Form
```csharp
using (ResourceType r1 = e1, r2 = e2, ..., rN = eN) statement
```
entspricht genau einer Sequenz von `using` -Anweisungen:
```csharp
using (ResourceType r1 = e1)
    using (ResourceType r2 = e2)
        ...
            using (ResourceType rN = eN)
                statement
```

Im folgenden Beispiel wird eine Datei mit `log.txt` dem Namen erstellt, und es werden zwei Textzeilen in die Datei geschrieben. Im Beispiel wird dann dieselbe Datei zum Lesen geöffnet, und die enthaltenen Textzeilen werden in die Konsole kopiert.
```csharp
using System;
using System.IO;

class Test
{
    static void Main() {
        using (TextWriter w = File.CreateText("log.txt")) {
            w.WriteLine("This is line one");
            w.WriteLine("This is line two");
        }

        using (TextReader r = File.OpenText("log.txt")) {
            string s;
            while ((s = r.ReadLine()) != null) {
                Console.WriteLine(s);
            }

        }
    }
}
```

Da die `TextWriter` Klassen `TextReader` und die `IDisposable` -Schnittstelle implementieren, kann im `using` Beispiel-Anweisungen verwendet werden, um sicherzustellen, dass die zugrunde liegende Datei nach den Schreib-oder Lesevorgängen ordnungsgemäß geschlossen wird

## <a name="the-yield-statement"></a>Die yield-Anweisung

Die `yield` -Anweisung wird in einem Iteratorblock ([Blocks](statements.md#blocks)) verwendet, um einen Wert für das Enumeratorobjekt ([Enumeratorobjekte](classes.md#enumerator-objects)) oder das Aufzähl Bare Objekt ([Aufzähl Bare Objekte](classes.md#enumerable-objects)) eines Iterators oder das Ende der Iterationen anzugeben.

```antlr
yield_statement
    : 'yield' 'return' expression ';'
    | 'yield' 'break' ';'
    ;
```

`yield`ist kein reserviertes Wort. Sie hat nur dann eine besondere Bedeutung, wenn Sie `return` direkt `break` vor einem Schlüsselwort oder verwendet wird. In anderen Kontexten `yield` kann als Bezeichner verwendet werden.

Es gibt mehrere Einschränkungen hinsichtlich des Orts `yield` , an dem eine-Anweisung angezeigt werden kann, wie im folgenden beschrieben.

*  Es handelt sich um einen Kompilierzeitfehler für eine `yield`-Anweisung (von beiden Formularen), die außerhalb von *method_body*, *operator_body* oder *accessor_body* angezeigt wird.
*  Es handelt sich um einen Kompilierzeitfehler `yield` für eine-Anweisung (von beiden Formularen), die in einer anonymen Funktion angezeigt wird.
*  Es handelt sich um einen Kompilierzeitfehler `yield` für eine-Anweisung (von beiden Formularen), `finally` die in der `try` -Klausel einer-Anweisung angezeigt wird.
*  Es ist ein Kompilierzeitfehler, wenn `yield return` eine-Anweisung an einer beliebigen `try` Stelle in einer- `catch` Anweisung angezeigt wird, die Klauseln enthält.

Das folgende Beispiel zeigt einige gültige und ungültige Verwendungen von `yield` -Anweisungen.

```csharp
delegate IEnumerable<int> D();

IEnumerator<int> GetEnumerator() {
    try {
        yield return 1;        // Ok
        yield break;           // Ok
    }
    finally {
        yield return 2;        // Error, yield in finally
        yield break;           // Error, yield in finally
    }

    try {
        yield return 3;        // Error, yield return in try...catch
        yield break;           // Ok
    }
    catch {
        yield return 4;        // Error, yield return in try...catch
        yield break;           // Ok
    }

    D d = delegate { 
        yield return 5;        // Error, yield in an anonymous function
    }; 
}

int MyMethod() {
    yield return 1;            // Error, wrong return type for an iterator block
}
```

Eine implizite Konvertierung ([implizite Konvertierungen](conversions.md#implicit-conversions)) muss vom Typ des Ausdrucks in der `yield return` -Anweisung bis zum Yield-Typ ([Yield-Typ](classes.md#yield-type)) des Iterators vorhanden sein.

Eine `yield return` -Anweisung wird wie folgt ausgeführt:

*  Der in der-Anweisung angegebene Ausdruck wird ausgewertet, implizit in den Yield-Typ konvertiert und der `Current` -Eigenschaft des Enumeratorobjekts zugewiesen.
*  Die Ausführung des Iteratorblocks wurde angehalten. Wenn sich `yield return` die-Anweisung innerhalb eines oder `try` mehrerer Blöcke befindet, `finally` werden die zugeordneten Blöcke zurzeit nicht ausgeführt.
*  Die `MoveNext` -Methode des Enumeratorobjekts wird `true` an den Aufrufer zurückgegeben und gibt an, dass das Enumeratorobjekt erfolgreich auf das nächste Element erweitert wurde.

Der nächste aufrufungs Vorgang der- `MoveNext` Methode des Enumeratorobjekts setzt die Ausführung des Iteratorblocks fort, von wo er zuletzt angehalten wurde.

Eine `yield break` -Anweisung wird wie folgt ausgeführt:

*  Wenn die `yield break` -Anweisung von einem oder mehreren `try` -Blöcken mit zugeordneten `finally` -Blöcken eingeschlossen wird, wird die `finally` Steuerung anfänglich an den `try` -Block der innersten-Anweisung übertragen. Wenn und wenn das Steuerelement den Endpunkt eines `finally` -Blocks erreicht, wird die Steuerung an den `finally` -Block der nächsten einschließenden `try` Anweisung übertragen. Dieser Vorgang wird wiederholt, `finally` bis die Blöcke aller einschließenden `try` Anweisungen ausgeführt wurden.
*  Das Steuerelement wird an den Aufrufer des Iteratorblocks zurückgegeben. Dies ist entweder die `MoveNext` -Methode `Dispose` oder die-Methode des Enumeratorobjekts.

Da eine `yield break` Anweisung die Steuerung an andere Stellen überträgt, ist der Endpunkt `yield break` einer Anweisung nie erreichbar.
