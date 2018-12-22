# <a name="statements"></a>Anweisungen

C# bietet eine Vielzahl von Anweisungen. Die meisten dieser Anweisungen werden Entwicklern vertraut sein, die in C und C++ programmiert haben.

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

Die *Embedded_statement* nichtterminal wird verwendet, Anweisungen, die in anderen Anweisungen angezeigt werden. Die Verwendung von *Embedded_statement* statt *Anweisung* schließt die Verwendung von deklarationsanweisungen und Anweisungen mit Bezeichnung in diesen Kontexten. Im Beispiel
```csharp
void F(bool b) {
    if (b)
        int i = 44;
}
```
führt zu einem Kompilierzeitfehler, da ein `if` -Anweisung erfordert eine *Embedded_statement* anstelle eines *Anweisung* für die If Branch. Wenn dieser Code wurden zulässig, klicken Sie dann die Variable `i` deklariert werden würde, aber nie verwendet werden kann. Beachten Sie jedoch, indem Sie platzieren `i`der Deklaration in einem Block, der im Beispiel ist gültig.

## <a name="end-points-and-reachability"></a>Endpunkte und Erreichbarkeit

Jede Anweisung verfügt über eine ***Endpunkt***. Intuitiv ausgedrückt ist der Endpunkt einer Anweisung den Speicherort an, der die Anweisung unmittelbar folgt. Die Ausführungsregeln für zusammengesetzte Statements (Anweisungen, die eingebettete Anweisungen enthalten) Geben Sie die Aktion, die ausgeführt wird, wenn die Steuerung der Endpunkt, der eine eingebettete Anweisung erreicht. Bei der Kontrolle über den Endpunkt einer Anweisung in einem Block erreicht, wird z. B. die Steuerung an die nächste Anweisung im-Block übergeben.

Wenn eine Anweisung möglicherweise durch die Ausführung erreicht werden kann, die Anweisung gilt ***erreichbar***. Im Gegensatz dazu ist es nicht möglich, dass eine Anweisung ausgeführt wird, die Anweisung gilt ***nicht erreichbar***.

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
der zweite Aufruf von `Console.WriteLine` ist nicht erreichbar, da besteht kein Risiko, dass die Anweisung ausgeführt wird.

Eine Warnung wird ausgegeben, wenn der Compiler feststellt, dass eine Anweisung nicht erreichbar ist. Es ist insbesondere nicht um einen Fehler für eine Anweisung als nicht erreichbar.

Um zu bestimmen, ob eine bestimmte Anweisung oder der Endpunkt erreichbar ist, führt der Compiler Flussanalyse gemäß den Erreichbarkeitsregeln für jede Anweisung an. Die Flussanalyse berücksichtigt die Werte der Konstanten Ausdrücke ([Konstante Ausdrücke](expressions.md#constant-expressions)), die das Verhalten der Anweisungen steuern, aber die möglichen Werte nicht Konstante Ausdrücke werden nicht berücksichtigt. Das heißt, für Zwecke der Ablaufsteuerungsanalyse, ein nicht konstanter Ausdruck eines bestimmten Typs gilt alle möglichen Werte dieses Typs haben.

Im Beispiel
```csharp
void F() {
    const int i = 1;
    if (i == 2) Console.WriteLine("unreachable");
}
```
der boolesche Ausdruck, der die `if` -Anweisung ist ein konstanter Ausdruck, weil beide Operanden aus der `==` Operator Konstanten sind. Wie der Konstante Ausdruck zur Kompilierzeit ausgewertet wird, den Wert erzeugt `false`, `Console.WriteLine` Aufruf gilt als nicht erreichbar. Aber wenn `i` wird geändert, um eine lokale Variable.
```csharp
void F() {
    int i = 1;
    if (i == 2) Console.WriteLine("reachable");
}
```
die `Console.WriteLine` Aufruf gilt als erreichbar sind, auch wenn sich in der Praxis es nie ausgeführt wird.

Die *Block* einer Funktion Element immer als erreichbar. Durch die Auswertung der nacheinander der Erreichbarkeitsregeln für jede Anweisung in einem Block, kann die Erreichbarkeit des jede angegebene Anweisung ermittelt werden.

Im Beispiel
```csharp
void F(int x) {
    Console.WriteLine("start");
    if (x < 0) Console.WriteLine("negative");
}
```
die Erreichbarkeit des zweiten `Console.WriteLine` wird wie folgt bestimmt:

*  Die erste `Console.WriteLine` Ausdrucksanweisung erreichbar ist. da der Block der `F` Methode erreichbar ist.
*  Der Endpunkt des ersten `Console.WriteLine` Ausdrucksanweisung ist erreichbar, da die Anweisung erreichbar ist.
*  Die `if` -Anweisung erreichbar ist, daran, dass das Ende des ersten `Console.WriteLine` Ausdrucksanweisung ist erreichbar.
*  Die zweite `Console.WriteLine` Ausdrucksanweisung erreichbar ist. da der boolesche Ausdruck, der die `if` Anweisung verfügt nicht über den konstanten Wert `false`.

Es gibt zwei Situationen, in denen ein Fehler während der Kompilierung für den Endpunkt einer Anweisung erreichbar ist:

*  Da die `switch` Anweisung ist einen Switch-Abschnitt auf "Fortfahren" mit dem nächsten Switch-Abschnitt nicht zulässig, es ist ein Fehler während der Kompilierung für den Endpunkt der Anweisungsliste einen Switch-Abschnitt erreichbar sein. Wenn dieser Fehler auftritt, ist es in der Regel ein Hinweis auf, die eine `break` Anweisung ist nicht vorhanden.
*  Es ist ein Fehler während der Kompilierung für den Endpunkt des Codeblocks an ein Funktionsmember, der berechnet einen Wert als erreichbar sein. Wenn dieser Fehler auftritt, ist es in der Regel ein Hinweis auf, die eine `return` Anweisung ist nicht vorhanden.

## <a name="blocks"></a>Blöcke

Ein *Block* ermöglicht, mehrere Anweisungen in Kontexten zu schreiben, in denen eine einzelne Anweisung zulässig ist.

```antlr
block
    : '{' statement_list? '}'
    ;
```

Ein *Block* besteht aus einem optionalen *Statement_list* ([Anweisung listet](statements.md#statement-lists)) in geschweiften Klammern. Wenn die Anweisungsliste ausgelassen wird, wird der Block als leer sein. bezeichnet.

Ein Block kann deklarationsanweisungen enthalten ([deklarationsanweisungen](statements.md#declaration-statements)). Im Rahmen einer lokalen Variable oder Konstante wird in einem Block deklariert der Block.

Ein Block ist wie folgt ausgeführt:

*  Wenn der Block leer ist, wird die Steuerung an den Endpunkt des Blocks übergeben.
*  Wenn der Block nicht leer ist, wird die Steuerung an die Anweisungsliste übergeben. Wenn und Steuerung am Ende der Anweisungsliste erreicht, wird die Steuerung an den Endpunkt des Blocks übergeben.

Die Anweisungsliste eines Blocks ist erreichbar, wenn der Block erreicht werden kann.

Der Endpunkt eines Blocks ist erreichbar, wenn der Block leer ist oder wenn der Endpunkt der Anweisungsliste erreichbar ist.

Ein *Block* , enthält eine oder mehrere `yield` Anweisungen ([die Yield-Anweisung](statements.md#the-yield-statement)) kein Iteratorblock aufgerufen wird. Iteratorblöcke werden verwendet, um Funktionsmember als Iteratoren implementiert ([Iteratoren](classes.md#iterators)). Es gelten zusätzlichen Einschränkungen gelten für Iteratorblöcke:

*  Es ist ein Fehler während der Kompilierung für eine `return` -Anweisung in einem Iteratorblock angezeigt (aber `yield return` -Anweisungen sind zulässig).
*  Es ist ein Fehler während der Kompilierung für die kein Iteratorblock einen unsicheren Kontext enthalten ([nicht sicheren Kontexten](unsafe-code.md#unsafe-contexts)). Kein Iteratorblock definiert immer einen sicheren Kontext ein, auch wenn der Deklaration in einem unsicheren Kontext geschachtelt sind.

### <a name="statement-lists"></a>Anweisungslisten

Ein ***Anweisungsliste*** besteht aus mindestens einer Anweisung, die in der Sequenz geschrieben wurden. Anweisungslisten kommen *Block*s ([Blöcke](statements.md#blocks)) und im *Switch_block*s ([der Switch-Anweisung](statements.md#the-switch-statement)).

```antlr
statement_list
    : statement+
    ;
```

Eine Anweisungsliste wird durch Übergabe der Steuerung an die erste Anweisung ausgeführt. Wenn und Kontrolle über den Endpunkt einer Anweisung erreicht, wird die Steuerung an die nächste Anweisung übergeben. Wenn und Steuerung der Endpunkt der letzten Anweisung erreicht, wird die Steuerung an den Endpunkt der Anweisungsliste übergeben.

Eine Anweisung in eine Anweisungsliste ist erreichbar, wenn mindestens eine der folgenden "true" ist:

*  Die Anweisung ist die erste Anweisung aus, und die Anweisungsliste selbst erreichbar ist.
*  Der Endpunkt der vorherigen Anweisung ist erreichbar.
*  Die Anweisung ist eine Anweisung mit Bezeichnung und die Bezeichnung verweist auf einen erreichbaren `goto` Anweisung.

Der Endpunkt eine Anweisungsliste ist erreichbar, wenn der Endpunkt, der die letzte Anweisung in der Liste erreicht werden kann.

## <a name="the-empty-statement"></a>Die leere Anweisung

Ein *Empty_statement* hat keine Auswirkungen.

```antlr
empty_statement
    : ';'
    ;
```

Eine leere Anweisung wird verwendet, wenn es keine Vorgänge in einem Kontext ausgeführt werden, wenn eine Anweisung erforderlich ist.

Ausführung eine leere Anweisung wird einfach die Steuerung an den Endpunkt der Anweisung. Daher ist der Endpunkt, der eine leere Anweisung erreichbar, wenn die leere Anweisung erreichbar ist.

Eine leere Anweisung kann verwendet werden, wenn das Schreiben einer `while` -Anweisung mit einem null-Text:
```csharp
bool ProcessMessage() {...}

void ProcessMessages() {
    while (ProcessMessage())
        ;
}
```

Darüber hinaus kann eine leere Anweisung verwendet werden, deklarieren Sie eine Bezeichnung direkt vor dem abschließenden "`}`" eines Blocks:
```csharp
void F() {
    ...
    if (done) goto exit;
    ...
    exit: ;
}
```

## <a name="labeled-statements"></a>Anweisungen mit Bezeichnung

Ein *Labeled_statement* ermöglicht eine Anweisung eine Bezeichnung vorangestellt werden. Anweisungen mit Bezeichnung sind in Blöcken zulässig, aber Sie sind als eingebettete Anweisungen nicht zulässig.

```antlr
labeled_statement
    : identifier ':' statement
    ;
```

Eine bezeichnete Anweisung deklariert eine Bezeichnung mit dem Namen durch die *Bezeichner*. Der Bereich einer Bezeichnung ist auf den ganzen Block in der die Bezeichnung deklariert ist, einschließlich aller geschachtelten Blöcke. Es ist ein Fehler während der Kompilierung für zwei Bezeichnungen mit dem gleichen Namen, um überlappende Bereiche zu erhalten.

Eine Bezeichnung aus verwiesen werden kann `goto` Anweisungen ([Goto-Anweisung](statements.md#the-goto-statement)) innerhalb des Bereichs der Bezeichnung. Dies bedeutet, dass `goto` Anweisungen können Steuerelement innerhalb von Blöcken und Out-of-Blöcken, aber nie in Blöcke übertragen.

Bezeichnungen müssen ihre eigenen Deklarationsabschnitt und verursachen keine Konflikte mit anderen Bezeichnern. Im Beispiel
```csharp
int F(int x) {
    if (x >= 0) goto x;
    x = -x;
    x: return x;
}
```
gültig ist und verwendet den Namen des `x` als Parameter und eine Bezeichnung.

Ausführung einer Anweisung mit Bezeichnung entspricht der Ausführung der Anweisung nach der Bezeichnung.

Zusätzlich zu der Erreichbarkeit von normalen ablaufsteuerung bereitgestellt wird, ist eine Anweisung mit Bezeichnung erreichbar, wenn die Bezeichnung einen erreichbaren verweist `goto` Anweisung. (Ausnahme: Wenn eine `goto` -Anweisung ist innerhalb einer `try` , enthält eine `finally` Block und der Anweisung mit Bezeichnung befindet sich außerhalb der `try`, und den Endpunkt des der `finally` Block ist nicht erreichbar, und klicken Sie dann die Anweisung mit Bezeichnung ist nicht erreichbar `goto` Anweisung.)

## <a name="declaration-statements"></a>Deklarationsanweisungen

Ein *Declaration_statement* deklariert eine lokale Variable oder Konstante. Deklarationsanweisungen sind in Blöcken zulässig, aber Sie sind als eingebettete Anweisungen nicht zulässig.

```antlr
declaration_statement
    : local_variable_declaration ';'
    | local_constant_declaration ';'
    ;
```

### <a name="local-variable-declarations"></a>Deklarationen von lokalen Variablen

Ein *Local_variable_declaration* deklariert eine oder mehrere lokale Variablen.

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

Die *Local_variable_type* von einem *Local_variable_declaration* direkt gibt den Typ der Variablen durch die Deklaration eingeführt wurden, oder gibt an, mit dem Bezeichner `var` , Der Typ sollte basierend auf einem Initialisierer abgeleitet werden. Der Typ folgt eine Liste der *Local_variable_declarator*s, von denen jede eine neue Variable führt. Ein *Local_variable_declarator* besteht aus einer *Bezeichner* mit dem Namen der Variablen, optional gefolgt von einer "`=`" token und einem *Local_variable_initializer* , durch den Anfangswert der Variablen erhalten.

Im Kontext der Deklaration einer lokalen Variablen, Var Bezeichner fungiert, als ein kontextbezogenes Schlüsselwort ([Schlüsselwörter](lexical-structure.md#keywords)). Bei der *Local_variable_type* angegeben ist, als `var` und kein Typ mit dem Namen `var` ist im Gültigkeitsbereich die Deklaration ist eine ***implizit typisierten Deklaration lokalen Variablen***, dessen Typ abgeleitet aus dem Typ des initialisiererausdrucks zugeordnet. Implizit typisierte lokale Variable Deklarationen sind jedoch mit folgenden Einschränkungen:

*  Die *Local_variable_declaration* kann nicht mehrere enthalten *Local_variable_declarator*s.
*  Die *Local_variable_declarator* müssen eine *Local_variable_initializer*.
*  Die *Local_variable_initializer* muss ein *Ausdruck*.
*  Der Initialisierer *Ausdruck* muss eine während der Kompilierung aufweisen.
*  Der Initialisierer *Ausdruck* kann nicht auf die deklarierte Variable sich selbst verweisen

Es folgen Beispiele für falsche implizit typisierte lokale Variable Deklarationen:

```csharp
var x;               // Error, no initializer to infer type from
var y = {1, 2, 3};   // Error, array initializer not permitted
var z = null;        // Error, null does not have a type
var u = x => x + 1;  // Error, anonymous functions do not have a type
var v = v++;         // Error, initializer cannot refer to variable itself
```

Der Wert einer lokalen Variablen wird abgerufen, in einem Ausdruck mit einer *Simple_name* ([einfache Namen](expressions.md#simple-names)), sowie der Wert einer lokalen Variablen mit geändert wird ein *Zuweisung* ( [Zuweisungsoperatoren](expressions.md#assignment-operators)). Eine lokale Variable muss definitiv zugewiesen werden ([definitive Zuweisung](variables.md#definite-assignment)) an jedem Standort, bei dem der Wert abgerufen wird.

Eine lokale Variable deklariert, die im Rahmen einer *Local_variable_declaration* ist der Block in dem die Deklaration erfolgt. Es ist ein Fehler auf Sie verweisen auf eine lokale Variable in der Lage, Text vor dem *Local_variable_declarator* der lokalen Variablen. Innerhalb des Bereichs einer lokalen Variablen ist es ein Fehler während der Kompilierung eine andere lokale Variable oder Konstante mit dem gleichen Namen deklariert.

Die Deklaration eine lokale Variable, die mehrere Variablen deklariert entspricht mehreren Deklarationen der einzelnen Variablen mit dem gleichen Typ. Darüber hinaus entspricht einem Variableninitialisierer in einer lokalen Variablendeklaration genau einer zuweisungsanweisung, die sofort nach der Deklaration eingefügt wird.

Im Beispiel
```csharp
void F() {
    int x = 1, y, z = x * 2;
}
```
genau entspricht
```csharp
void F() {
    int x; x = 1;
    int y;
    int z; z = x * 2;
}
```

In einer implizit typisierten lokalen Variablendeklaration wird der Typ der deklarierten lokalen Variablen erstellt, um den Typ des Ausdrucks verwendet, um die Variable initialisieren identisch sein. Zum Beispiel:
```csharp
var i = 5;
var s = "Hello";
var d = 1.0;
var numbers = new int[] {1, 2, 3};
var orders = new Dictionary<int,Order>();
```

Implizit typisierten lokalen Variablendeklarationen oben sind die folgenden explizit typisierten Deklarationen genaue Entsprechung:
```csharp
int i = 5;
string s = "Hello";
double d = 1.0;
int[] numbers = new int[] {1, 2, 3};
Dictionary<int,Order> orders = new Dictionary<int,Order>();
```

### <a name="local-constant-declarations"></a>Deklaration von lokalen Konstanten

Ein *Local_constant_declaration* eine oder mehrere lokale Konstanten deklariert.

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

Die *Typ* von einem *Local_constant_declaration* gibt den Typ der Konstanten, die durch die Deklaration eingeführt. Der Typ folgt eine Liste der *Constant_declarator*s, von denen jede eine neue Konstante führt. Ein *Constant_declarator* besteht aus einer *Bezeichner* , Namen der Konstanten gefolgt von einer "`=`" token, gefolgt von einer *Constant_expression* ([ Konstante Ausdrücke](expressions.md#constant-expressions)), die den Wert der Konstanten bietet.

Die *Typ* und *Constant_expression* der Deklaration von lokalen Konstanten muss, gelten dieselben Regeln wie für eine Deklaration einer Konstante Member ([Konstanten](classes.md#constants)).

Der Wert einer lokalen Konstanten wird ermittelt, in einem Ausdruck mit einem *Simple_name* ([einfache Namen](expressions.md#simple-names)).

Der Gültigkeitsbereich der lokale Konstante ist, den Block in dem die Deklaration erfolgt. Es ist ein Fehler auf Sie verweisen auf eine lokale Konstante in der Lage, Text vor dessen *Constant_declarator*. Innerhalb des Bereichs einer lokalen Konstanten ist es ein Fehler während der Kompilierung eine andere lokale Variable oder Konstante mit dem gleichen Namen deklariert.

Deklaration von lokale Konstante, die mehrere Konstanten deklariert entspricht mehreren Deklarationen der einzelnen Konstanten desselben Typs.

## <a name="expression-statements"></a>Ausdrucksanweisungen

Ein *Expression_statement* wertet einen angegebenen Ausdruck. Der Wert berechnet werden, durch den Ausdruck wird ggf. verworfen.

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

Nicht alle Ausdrücke werden als Anweisungen zulässig. Insbesondere Ausdrücke wie z. B. `x + y` und `x == 1` , die lediglich einen Wert (die werden verworfen) berechnen, werden als Anweisungen nicht zulässig.

Ausführung einer *Expression_statement* der enthaltenen Ausdruck ausgewertet, und klicken Sie dann überträgt die Steuerung an den Endpunkt, der die *Expression_statement*. Der Endpunkt, der eine *Expression_statement* ist erreichbar. wenn das *Expression_statement* erreichbar ist.

## <a name="selection-statements"></a>Auswahlanweisungen

Auswahlanweisungen wählen Sie eine Anzahl von möglichen Anweisungen für die Ausführung anhand des Werts eines Ausdrucks.

```antlr
selection_statement
    : if_statement
    | switch_statement
    ;
```

### <a name="the-if-statement"></a>Die bei Anweisung

Die `if` -Anweisung wählt eine Anweisung für die Ausführung anhand des Werts eines booleschen Ausdrucks.

```antlr
if_statement
    : 'if' '(' boolean_expression ')' embedded_statement
    | 'if' '(' boolean_expression ')' embedded_statement 'else' embedded_statement
    ;
```

Ein `else` Teil bezieht sich auf dem lexikalisch nächsten vorausgehenden `if` , durch die Syntax zulässig ist. Daher eine `if` -Anweisung der Form
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

Ein `if` -Anweisung wird wie folgt ausgeführt:

*  Die *Boolean_expression* ([boolesche Ausdrücke](expressions.md#boolean-expressions)) ausgewertet wird.
*  Wenn der boolesche Ausdruck ergibt `true`, wird die Steuerung an die erste eingebettete Anweisung. Wenn und Steuerung der Endpunkt, der diese Anweisung erreicht, wird die Steuerung an den Endpunkt der `if` Anweisung.
*  Wenn der boolesche Ausdruck ergibt `false` und, wenn ein `else` Teil vorhanden ist, wird die Steuerung an die zweite, eingebettete Anweisung. Wenn und Steuerung der Endpunkt, der diese Anweisung erreicht, wird die Steuerung an den Endpunkt der `if` Anweisung.
*  Wenn der boolesche Ausdruck ergibt `false` und, wenn ein `else` Teil ist nicht vorhanden, wird die Steuerung an den Endpunkt, der die `if` Anweisung.

Die erste eingebettete Anweisung, der ein `if` Anweisung erreichbar ist. wenn die `if` -Anweisung erreichbar ist und der boolesche Ausdruck verfügt nicht über den konstanten Wert `false`.

Die zweite eingebettete Anweisung, die von einer `if` -Anweisung, falls vorhanden, ist erreichbar Wenn die `if` -Anweisung erreichbar ist und der boolesche Ausdruck verfügt nicht über den konstanten Wert `true`.

Der Endpunkt, der eine `if` -Anweisung ist erreichbar, wenn der Endpunkt, der mindestens eines der eingebetteten Anweisungen erreichbar ist. Darüber hinaus zeigen Sie das Ende des ein `if` Anweisung ohne `else` Teil erreichbar ist. wenn die `if` -Anweisung erreichbar ist und der boolesche Ausdruck verfügt nicht über den konstanten Wert `true`.

### <a name="the-switch-statement"></a>Der Switch-Anweisung

Die Switch-Anweisung wählt eine Anweisungsliste mit einer zugeordneten Switch-Bezeichnung, die den Wert des Switch-Ausdrucks entspricht, für die Ausführung.

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

Ein *Switch_statement* besteht aus dem Schlüsselwort `switch`, gefolgt von einem Ausdruck in Klammern (den Switch-Ausdruck bezeichnet), gefolgt von einem *Switch_block*. Die *Switch_block* besteht aus 0 (null) oder mehreren *Switch_section*s, die in geschweifte Klammern eingeschlossen. Jede *Switch_section* besteht aus einem oder mehreren *Switch_label*s gefolgt von einem *Statement_list* ([Anweisung listet](statements.md#statement-lists)).

Die ***für Typ*** von einem `switch` Anweisung wird hergestellt, indem der Switch-Ausdruck.

*  Wenn der Typ des Switch-Ausdrucks ist `sbyte`, `byte`, `short`, `ushort`, `int`, `uint`, `long`, `ulong`, `bool`, `char`, `string`, oder ein *Enum_type*, oder wenn es für einen dieser Typen einen nullable-Typ ist, dann wird die Steuerung zu geben, der die `switch` Anweisung.
*  Andernfalls genau eine benutzerdefinierte implizite Konvertierung ([benutzerdefinierte Konvertierungen](conversions.md#user-defined-conversions)) muss vorhanden sein, von dem Typ des Switch-Ausdrucks auf einen der folgenden möglichen Typen, die steuern: `sbyte`, `byte`, `short` , `ushort`, `int`, `uint`, `long`, `ulong`, `char`, `string`, oder einen nullable-Typ, der einer dieser Typen entspricht.
*  Andernfalls tritt auf, wenn solch eine implizite Konvertierung vorhanden ist oder wenn mehr als eine implizite Konvertierung vorhanden ist, ein Fehler während der Kompilierung.

Der Konstante Ausdruck jeder `case` Bezeichnung muss einen Wert, der implizit konvertiert werden deuten ([implizite Konvertierungen](conversions.md#implicit-conversions)) auf der vorherrschende Datentyp der `switch` Anweisung. Ein Fehler während der Kompilierung tritt auf, wenn zwei oder mehr `case` Bezeichnungen in der gleichen `switch` Anweisung geben Sie den konstanten Wert.

Es kann höchstens eine `default` Bezeichnung in einer Switch-Anweisung.

Ein `switch` -Anweisung wird wie folgt ausgeführt:

*  Der Switch-Ausdruck wird ausgewertet und in der vorherrschende Datentyp konvertiert.
*  Wenn eine der Konstanten in angegeben ein `case` Bezeichnung in der gleichen `switch` -Anweisung ist gleich dem Wert des Switch-Ausdrucks, die Steuerung an die Anweisungsliste, die nach der entsprechenden `case` Bezeichnung.
*  Wenn keine der Konstanten in angegebenen `case` Bezeichnungen in der gleichen `switch` -Anweisung ist gleich dem Wert des Switch-Ausdrucks, und wenn ein `default` Bezeichnung vorhanden ist, wird die Steuerung an die Anweisung Liste nach der `default` Bezeichnung.
*  Wenn keine der Konstanten in angegebenen `case` Bezeichnungen in der gleichen `switch` -Anweisung ist gleich dem Wert des Switch-Ausdrucks, und wenn keine `default` Bezeichnung vorhanden ist, wird die Steuerung an den Endpunkt der `switch` Anweisung.

Wenn der Endpunkt der Anweisungsliste einen Switch-Abschnitt erreichbar ist, tritt ein Fehler während der Kompilierung. Dies wird wie in der Regel "keine fortfahren" bezeichnet. Im Beispiel
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
ist gültig, da keine Switch-Abschnitt auf einen erreichbaren Endpunkt enthält. Im Gegensatz zu C und C++ Ausführung ein Switch-Abschnitt darf nicht "Fortfahren" zu den nächsten Switch-Abschnitt im Beispiel
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
führt zu einem Fehler während der Kompilierung. Bei Ausführung des ein Switch-Abschnitt ist durch die Ausführung von einem anderen Switch-Abschnitt, eine explizite befolgt werden `goto case` oder `goto default` -Anweisung verwendet werden muss:
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

Mehrere Bezeichnungen dürfen sich einem *Switch_section*. Im Beispiel
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
ist gültig. Im Beispiel wird die Regel "keine fortfahren" nicht verletzt, da die Bezeichnungen `case 2:` und `default:` sind Teil derselben *Switch_section*.

Die Regel "keine fortfahren" wird verhindert, dass eine allgemeine Klasse von Fehlern, die in C und C++ auftreten, wenn `break` Anweisungen versehentlich ausgelassen werden. Darüber hinaus aufgrund dieser Regel, die Switch-Abschnitte von einem `switch` Anweisung willkürlich neu angeordnet werden kann ohne Auswirkungen auf das Verhalten der Anweisung. Z. B. die Abschnitte der `switch` obigen Anweisung ohne Auswirkungen auf das Verhalten der Anweisung rückgängig gemacht werden kann:
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

In der Regel die Anweisungsliste, der ein Switch-Abschnitt endet, in einem `break`, `goto case`, oder `goto default` -Anweisung, jedoch Konstrukt, das am Ende der Anweisungsliste unerreichbar ist zulässig. Z. B. eine `while` Anweisung, die von der boolesche Ausdruck gesteuert `true` nie Reichweite seinen Endpunkt bekannt ist. Ebenso eine `throw` oder `return` -Anweisung immer überträgt die Steuerung an anderer Stelle und seinen Endpunkt nicht erreicht. Im folgende Beispiel ist daher gültig:
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

Der vorherrschende Datentyp eine `switch` Anweisung kann es sich um den Typ `string`. Zum Beispiel:
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

Wie die Gleichheitsoperatoren ([Zeichenfolge Gleichheitsoperatoren](expressions.md#string-equality-operators)), wird die `switch` -Anweisung wird die Groß-/Kleinschreibung beachtet und einen bestimmten Switch-Abschnitt nur ausgeführt, wenn die Zeichenfolge des Switch-Ausdrucks identisch eine `case` Bezeichnung Konstante.

Wenn der Typ den leitenden eine `switch` -Anweisung ist `string`, den Wert `null` als Konstante Case-Bezeichnung zulässig ist.

Die *Statement_list*s eine *Switch_block* darf deklarationsanweisungen ([deklarationsanweisungen](statements.md#declaration-statements)). Im Rahmen einer lokalen Variable oder Konstante wird, die in einer Switch-Block deklariert der Switch-Block.

Die Anweisungsliste, der einem bestimmten Switch-Abschnitt ist erreichbar. wenn die `switch` -Anweisung erreichbar ist und mindestens eine der folgenden gilt:

*  Der Switch-Ausdruck ist ein nicht konstanter Wert.
*  Der Switch-Ausdruck ist ein konstanter Wert, der entspricht einem `case` Bezeichnung im Switch-Abschnitt.
*  Der Switch-Ausdruck ist ein konstanter Wert, der bundesstaatscode keiner entspricht `case` Bezeichnung und Switch-Abschnitt enthält die `default` Bezeichnung.
*  Eine Switch-Bezeichnung der Switch-Abschnitt verweist auf einen erreichbaren `goto case` oder `goto default` Anweisung.

Der Endpunkt, der eine `switch` -Anweisung ist erreichbar, wenn mindestens eine der folgenden "true" ist:

*  Die `switch` -Anweisung enthält einen erreichbaren `break` -Anweisung, die beendet die `switch` Anweisung.
*  Die `switch` -Anweisung erreichbar ist, der Switch-Ausdruck ist ein nicht konstanter Wert und keine `default` -Bezeichnung ist vorhanden.
*  Die `switch` -Anweisung erreichbar ist, der Switch-Ausdruck ist ein konstanter Wert, der bundesstaatscode keiner entspricht `case` Bezeichnung und ohne `default` -Bezeichnung ist vorhanden.

## <a name="iteration-statements"></a>Iterationsanweisungen

Iterationsanweisungen hat eine eingebettete Anweisung wiederholt ausführen.

```antlr
iteration_statement
    : while_statement
    | do_statement
    | for_statement
    | foreach_statement
    ;
```

### <a name="the-while-statement"></a>Die while-Anweisung

Die `while` Anweisung führt bedingt eine eingebettete Anweisung nullmal oder häufiger abgeglichen.

```antlr
while_statement
    : 'while' '(' boolean_expression ')' embedded_statement
    ;
```

Ein `while` -Anweisung wird wie folgt ausgeführt:

*  Die *Boolean_expression* ([boolesche Ausdrücke](expressions.md#boolean-expressions)) ausgewertet wird.
*  Wenn der boolesche Ausdruck ergibt `true`, wird die Steuerung an die eingebettete Anweisung. Wenn und die Steuerung der Endpunkt, der die eingebettete Anweisung erreicht (möglicherweise durch Ausführen von einer `continue` Anweisung), wird die Steuerung an den Anfang der `while` Anweisung.
*  Wenn der boolesche Ausdruck ergibt `false`, die Steuerung an den Endpunkt der `while` Anweisung.

In der die eingebettete Anweisung eine `while` -Anweisung eine `break` Anweisung ([die Break-Anweisung](statements.md#the-break-statement)) kann verwendet werden, die Steuerung an den Endpunkt übertragen die `while` (also mit der Endung Iteration, der die eingebettete Anweisung -Anweisung), und ein `continue` Anweisung ([die Continue-Anweisung](statements.md#the-continue-statement)) kann verwendet werden, um die Steuerung an den Endpunkt, der die eingebettete Anweisung zu übertragen (daher Ausführen einer anderen Iteration von der `while` Anweisung).

Die eingebettete Anweisung, der eine `while` Anweisung erreichbar ist. wenn die `while` -Anweisung erreichbar ist und der boolesche Ausdruck verfügt nicht über den konstanten Wert `false`.

Der Endpunkt, der eine `while` -Anweisung ist erreichbar, wenn mindestens eine der folgenden "true" ist:

*  Die `while` -Anweisung enthält einen erreichbaren `break` -Anweisung, die beendet die `while` Anweisung.
*  Die `while` -Anweisung erreichbar ist und der boolesche Ausdruck verfügt nicht über den konstanten Wert `true`.

### <a name="the-do-statement"></a>Die Do-Anweisung

Die `do` Anweisung führt bedingt eine eingebettete Anweisung einmal oder mehrmals.

```antlr
do_statement
    : 'do' embedded_statement 'while' '(' boolean_expression ')' ';'
    ;
```

Ein `do` -Anweisung wird wie folgt ausgeführt:

*  Steuerung wird an die eingebettete Anweisung übergeben.
*  Wenn und die Steuerung der Endpunkt, der die eingebettete Anweisung erreicht (möglicherweise durch Ausführen von einer `continue` Anweisung), wird die *Boolean_expression* ([boolesche Ausdrücke](expressions.md#boolean-expressions)) ausgewertet wird. Wenn der boolesche Ausdruck ergibt `true`, wird die Steuerung an den Anfang der `do` Anweisung. Andernfalls wird die Steuerung an den Endpunkt, der die `do` Anweisung.

In der die eingebettete Anweisung eine `do` -Anweisung eine `break` Anweisung ([die Break-Anweisung](statements.md#the-break-statement)) kann verwendet werden, die Steuerung an den Endpunkt übertragen die `do` (also mit der Endung Iteration, der die eingebettete Anweisung -Anweisung), und ein `continue` Anweisung ([die Continue-Anweisung](statements.md#the-continue-statement)) kann verwendet werden, um die Steuerung an den Endpunkt, der die eingebettete Anweisung zu übertragen.

Die eingebettete Anweisung des eine `do` Anweisung erreichbar ist. wenn die `do` -Anweisung erreichbar ist.

Der Endpunkt, der eine `do` -Anweisung ist erreichbar, wenn mindestens eine der folgenden "true" ist:

*  Die `do` -Anweisung enthält einen erreichbaren `break` -Anweisung, die beendet die `do` Anweisung.
*  Der Endpunkt, der die eingebettete Anweisung erreichbar ist und der boolesche Ausdruck verfügt nicht über den konstanten Wert `true`.

### <a name="the-for-statement"></a>Die für die Anweisung

Die `for` Anweisung wertet eine Reihe von Initialisierungsausdrücken und dann, solange eine Bedingung "true" wird wiederholt führt eine eingebettete Anweisung und eine Sequenz von Ausdrücken für Iteration ausgewertet.

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

Die *For_initializer*, falls vorhanden, besteht entweder aus einem *Local_variable_declaration* ([lokale Variablendeklarationen](statements.md#local-variable-declarations)) oder eine Liste von *Statement_ Ausdruck*s ([ausdrucksanweisungen](statements.md#expression-statements)) durch Kommas getrennt. Der Bereich, der eine lokale Variable deklariert, indem eine *For_initializer* beginnt bei der *Local_variable_declarator* für die Variable und reicht bis zum Ende der embedded-Anweisung. Der Gültigkeitsbereich umfasst die *For_condition* und *For_iterator*.

Die *For_condition*, falls vorhanden, muss ein *Boolean_expression* ([boolesche Ausdrücke](expressions.md#boolean-expressions)).

Die *For_iterator*, falls vorhanden, besteht aus einer Liste von *Statement_expression*s ([ausdrucksanweisungen](statements.md#expression-statements)) durch Kommas getrennt.

Eine for-Anweisung wird wie folgt ausgeführt:

*  Wenn eine *For_initializer* ist vorhanden, wird die Variable Initialisierer oder Anweisungsausdrücke ausgeführt werden, in der Reihenfolge, die sie geschrieben werden. Dieser Schritt ist nur einmal ausgeführt.
*  Wenn eine *For_condition* vorhanden ist, wird ausgewertet.
*  Wenn die *For_condition* nicht vorhanden ist oder wenn das Ergebnis der Auswertung `true`, wird die Steuerung an die eingebettete Anweisung. Wenn und die Steuerung der Endpunkt, der die eingebettete Anweisung erreicht (möglicherweise durch Ausführen von eine `continue` Anweisung), die Ausdrücke der *For_iterator*, sofern vorhanden, werden in der Reihenfolge ausgewertet, und klicken Sie dann eine andere Iteration ist durchgeführt, beginnend mit der Auswertung der *For_condition* im vorherigen Schritt.
*  Wenn die *For_condition* vorhanden ist und die Auswertung ergibt `false`, die Steuerung an den Endpunkt der `for` Anweisung.

In der die eingebettete Anweisung eine `for` -Anweisung eine `break` Anweisung ([die Break-Anweisung](statements.md#the-break-statement)) kann verwendet werden, die Steuerung an den Endpunkt übertragen die `for` (also mit der Endung Iteration, der die eingebettete Anweisung -Anweisung), und ein `continue` Anweisung ([die Continue-Anweisung](statements.md#the-continue-statement)) kann verwendet werden, um die Steuerung an den Endpunkt, der die eingebettete Anweisung zu übertragen (somit Ausführen der *For_iterator* und Ausführen einer anderen Iteration, der die `for` -Anweisung, beginnend mit der *For_condition*).

Die eingebettete Anweisung von einem `for` -Anweisung ist erreichbar, wenn eine der folgenden Aussagen zutrifft:

*  Die `for` -Anweisung erreichbar ist und es wird kein *For_condition* vorhanden ist.
*  Die `for` -Anweisung erreichbar ist und ein *For_condition* vorhanden ist und keine den konstanten Wert `false`.

Der Endpunkt, der eine `for` -Anweisung ist erreichbar, wenn mindestens eine der folgenden "true" ist:

*  Die `for` -Anweisung enthält einen erreichbaren `break` -Anweisung, die beendet die `for` Anweisung.
*  Die `for` -Anweisung erreichbar ist und ein *For_condition* vorhanden ist und keine den konstanten Wert `true`.

### <a name="the-foreach-statement"></a>Die Foreach-Anweisung

Die `foreach` Anweisung listet die Elemente einer Auflistung und führt eine eingebettete Anweisung für jedes Element der Auflistung.

```antlr
foreach_statement
    : 'foreach' '(' local_variable_type identifier 'in' expression ')' embedded_statement
    ;
```

Die *Typ* und *Bezeichner* von einer `foreach` Anweisung deklariert den ***Iterationsvariable*** der Anweisung. Wenn die `var` Bezeichner angegeben wird, als die *Local_variable_type*, und kein Typ mit dem Namen `var` ist im Gültigkeitsbereich der Iterationsvariablen gilt eine ***implizit typisierte Iterationsvariable***, und sein Typ wird erstellt, um den Elementtyp des werden die `foreach` Anweisung, wie unten beschrieben. Die Iterationsvariable entspricht einer schreibgeschützten lokalen Variablen mit einem Bereich, der über die eingebettete Anweisung erweitert. Während der Ausführung einer `foreach` -Anweisung, die Iterationsvariable stellt das Auflistungselement für die Iteration gegenwärtig ausgeführt wird wird. Ein Fehler während der Kompilierung tritt auf, wenn die eingebettete Anweisung versucht, so ändern Sie die Iterationsvariable (per Zuweisung oder `++` und `--` Operatoren) oder übergeben Sie die Iterationsvariable als einen `ref` oder `out` Parameter.

Im folgenden wird aus Gründen der Übersichtlichkeit `IEnumerable`, `IEnumerator`, `IEnumerable<T>` und `IEnumerator<T>` finden Sie in die entsprechenden Typen in den Namespaces `System.Collections` und `System.Collections.Generic`.

Die Verarbeitung während der Kompilierung einer Foreach-Anweisung zuerst bestimmt das ***Auflistungstyp***, ***Enumeratortyp*** und ***Elementtyp*** des Ausdrucks. Diese Ermittlung wird wie folgt aus:

*  Wenn der Typ `X` von *Ausdruck* Arraytyp ist, dann gibt es eine implizite verweiskonvertierung von `X` auf die `IEnumerable` Schnittstelle (da `System.Array` implementiert diese Schnittstelle). Die ***Auflistungstyp*** ist die `IEnumerable` -Schnittstelle, die ***Enumeratortyp*** ist die `IEnumerator` Schnittstelle und die ***Elementtyp*** ist der Elementtyp des der Arraytyp `X`.
*  Wenn der Typ `X` von *Ausdruck* ist `dynamic` dann gibt es eine implizite Konvertierung von *Ausdruck* auf die `IEnumerable` Schnittstelle ([implizite Dynamic Konvertierungen](conversions.md#implicit-dynamic-conversions)). Die ***Auflistungstyp*** ist die `IEnumerable` Schnittstelle und die ***Enumeratortyp*** ist die `IEnumerator` Schnittstelle. Wenn der `var` Bezeichner angegeben wird, als die *Local_variable_type* die ***Elementtyp*** ist `dynamic`, andernfalls ist es `object`.
*  Andernfalls bestimmt, ob der Typ `X` verfügt über eine entsprechende `GetEnumerator` Methode:
   * Führen Sie die Suche nach Membern des Typs `X` mit dem Bezeichner `GetEnumerator` und keine Typargumente. Wenn die Membersuche keine Übereinstimmung ergibt oder eine Mehrdeutigkeit erzeugt oder erzeugt eine Übereinstimmung, die keine Methodengruppe ist, überprüfen Sie für eine enumerable-Schnittstelle, wie unten beschrieben. Es wird empfohlen, dass eine Warnung ausgegeben werden, wenn die Suche nach Membern erzeugt alles außer einer Methodengruppe oder keine Übereinstimmung.
   * Führen Sie die Auflösung von funktionsüberladungen mit der resultierenden Methodengruppe und einer leeren Argumentliste. Wenn keine entsprechenden Methoden Auflösung von funktionsüberladungen ergibt, führt zu einer Mehrdeutigkeit und führt zu einer einzelnen bewährte Methode, aber diese Methode entweder statisch oder nicht öffentlich ist, überprüfen Sie für eine enumerable-Schnittstelle, ist wie unten beschrieben. Es wird empfohlen, dass eine Warnung ausgegeben werden, wenn die Auflösung von funktionsüberladungen alles mit Ausnahme von eine eindeutige öffentliche Instanzmethode oder keine anwendbaren Methoden erzeugt.
   * Wenn der Rückgabetyp `E` von der `GetEnumerator` Methode ist keine Klasse, Struktur oder Schnittstelle-Typ, ein Fehler erzeugt wird und keine weiteren Schritte ausgeführt.
   * Suche nach Membern erfolgt auf `E` mit dem Bezeichner `Current` und keine Typargumente. Wenn die Suche nach Membern keine Übereinstimmung erzeugt, das Ergebnis ein Fehler ist oder das Ergebnis ist nichts außer eine öffentliche Instanz-Eigenschaft, die beim Lesen zu können, ein Fehler erzeugt wird und keine weiteren Schritte ausgeführt.
   * Suche nach Membern erfolgt auf `E` mit dem Bezeichner `MoveNext` und keine Typargumente. Wenn die Suche nach Membern keine Übereinstimmung erzeugt, das Ergebnis ein Fehler ist oder das Ergebnis alles außer einer Methodengruppe ist, ein Fehler erzeugt wird und keine weiteren Schritte ausgeführt.
   * Überladungsauflösung erfolgt auf die Methodengruppe mit einer leeren Argumentliste. Wenn Überladung Lösung führt keine entsprechenden Methoden, führt zu einer Mehrdeutigkeit oder Ergebnisse in einer einzelnen bewährte Methode, aber diese Methode entweder statisch oder nicht öffentlich ist oder der Rückgabetyp nicht ist `bool`, ein Fehler erzeugt wird und keine weiteren Schritte ausgeführt.
   * Die ***Auflistungstyp*** ist `X`, ***Enumeratortyp*** ist `E`, und die ***Elementtyp*** ist der Typ des der `Current` Eigenschaft.

*  Überprüfen Sie andernfalls für eine enumerable-Schnittstelle:
   * If für alle Typen `Ti` für den es eine implizite Konvertierung von `X` zu `IEnumerable<Ti>`, es ist ein eindeutiger Typ `T` so, dass `T` ist nicht `dynamic` und für alle anderen `Ti` gibt es ein implizite Konvertierung von `IEnumerable<T>` zu `IEnumerable<Ti>`, und klicken Sie dann die ***Auflistungstyp*** ist die Schnittstelle `IEnumerable<T>`, ***Enumeratortyp*** ist die Schnittstelle `IEnumerator<T>`, und die ***Elementtyp*** ist `T`.
   * Wenn mehr als ein solcher Typ vorhanden ist, andernfalls `T`, klicken Sie dann ein Fehler erzeugt wird und keine weiteren Schritte ausgeführt.
   * Andernfalls, wenn es eine implizite Konvertierung von `X` auf die `System.Collections.IEnumerable` -Schnittstelle, die ***Auflistungstyp*** ist diese Schnittstelle, die ***Enumeratortyp*** ist die Schnittstelle `System.Collections.IEnumerator`, und die ***Elementtyp*** ist `object`.
   * Andernfalls wird ein Fehler wird ausgelöst, und keine weiteren Schritte ausgeführt.

Die oben genannten Schritte, wenn erfolgreich, eindeutig zu erzeugen einen Auflistungstyp `C`, Enumeratortyp `E` und Elementtyp `T`. Eine Foreach-Anweisung der form
```csharp
foreach (V v in x) embedded_statement
```
Klicken Sie dann auf Erweitert:
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

Die Variable `e` ist nicht sichtbar bzw. zugegriffen werden kann, auf den Ausdruck `x` oder die eingebettete Anweisung oder jeden anderen Quellcode des Programms. Die Variable `v` in die eingebettete Anweisung schreibgeschützt ist. Wenn keine explizite Konvertierung vorhanden ist ([explizite Konvertierungen](conversions.md#explicit-conversions)) von `T` (der Typ des Elements), `V` (die *Local_variable_type* in der Foreach-Anweisung), ein Fehler wird ausgelöst, und es werden keine weiteren Schritte ausgeführt. Wenn `x` hat den Wert `null`, `System.NullReferenceException` wird zur Laufzeit ausgelöst.

Eine Implementierung ist auf eine gegebenen Foreach-Anweisung auf andere Weise, z. B. zur Verbesserung der Leistung, implementiert zulässig, solange das Verhalten konsistent mit der Erweiterung der oben genannten ist.

Die Platzierung von `v` innerhalb der While-Schleife ist wichtig, dass Sie wie sie eine anonyme Funktion, die im erfasst wird die *Embedded_statement*.

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
Wenn `v` deklariert wurde, außerhalb der While-Schleife wird für alle Iterationen und dessen Wert nach gemeinsam genutzt werden die für Schleife den endgültigen Wert wäre `13`, d.h. welche der Aufruf von `f` druckt. Stattdessen, da jede Iteration eine eigene Variable weist `v`, die von erfasst `f` in der ersten Iteration weiterhin den Wert enthalten soll `7`, dies ist, was gedruckt werden. (Hinweis: frühere Versionen von c# deklariert `v` außerhalb der While-Schleife.)

Der Hauptteil der Block wird schließlich anhand der folgenden Schritte erstellt:

*  Es ist eine implizite Konvertierung von `E` auf die `System.IDisposable` Schnittstelle ist, klicken Sie dann
   *  Wenn `E` ist ein NULL-Werte wird die Klausel wird schließlich erweitert, um die semantischen Darstellung:

      ```csharp
      finally {
          ((System.IDisposable)e).Dispose();
      }
      ```

   *  Andernfalls die Klausel wird schließlich erweitert, um die semantischen Darstellung:

      ```csharp
      finally {
          if (e != null) ((System.IDisposable)e).Dispose();
      }
      ```

   mit der Ausnahme, dass bei `E` ist ein Werttyp oder ein Typparameter instanziiert, um einen Werttyp handelt, und klicken Sie dann auf die Umwandlung von `e` zu `System.IDisposable` verursacht keine Boxing auftreten.

*  Andernfalls gilt: Wenn `E` einen versiegelten Typ, der zum Schluss wird die Klausel zu einem leeren Block erweitert:

   ```csharp
   finally {
   }
   ```

*  Andernfalls, die schließlich-Klausel erweitert:

   ```csharp
   finally {
       System.IDisposable d = e as System.IDisposable;
       if (d != null) d.Dispose();
   }
   ```    

   Die lokale Variable `d` ist nicht sichtbar bzw. Benutzercode zugegriffen werden kann. Es vor allem keine Konflikte mit einer anderen Variablen, deren Bereich umfasst, die finally-block.

Die Reihenfolge, in der `foreach` durchläuft die Elemente eines Arrays, lautet wie folgt: Für eindimensionale Arrays von Elementen in aufsteigender Indexreihenfolge durchlaufen werden, beginnend mit dem Index `0` und endend mit Index `Length - 1`. Für mehrdimensionale Arrays werden Elemente durchlaufen, sodass die Indizes von die Dimension ganz rechts erhöhte zuerst, dann weiter linken Dimension, und so weiter auf der linken Seite sind.

Das folgende Beispiel gibt jeden Wert in ein zweidimensionales Array, in der Reihenfolge der Elemente:
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
Die Ausgabe lautet wie folgt aus:
```csharp
1.2 2.3 3.4 4.5 5.6 6.7 7.8 8.9
```

Im Beispiel
```csharp
int[] numbers = { 1, 3, 5, 7, 9 };
foreach (var n in numbers) Console.WriteLine(n);
```
Der Typ des `n` wird davon ausgegangen werden `int`, den Elementtyp des `numbers`.

## <a name="jump-statements"></a>Sprunganweisungen

Sprunganweisungen übertragen der Steuerung bedingungslos.

```antlr
jump_statement
    : break_statement
    | continue_statement
    | goto_statement
    | return_statement
    | throw_statement
    ;
```

Der Speicherort, der eine Jump-Anweisung überträgt die Steuerung, wird aufgerufen, die ***Ziel*** der Jump-Anweisung.

Wenn eine Jump-Anweisung innerhalb eines Blocks auftritt und das Ziel dieser Jump-Anweisung außerhalb dieses Blocks ist, der Jump-Anweisung gilt als ***beenden*** des Blocks. Während eine sprunganweisung die Steuerung einen Block übertragen, können sie nie Steuerelement in einem Block übertragen.

Sprunganweisungen Ausführung wird durch das Vorhandensein der dazwischen liegenden verkompliziert `try` Anweisungen. In Ermangelung eines solchen `try` Anweisungen, die eine Jump-Anweisung überträgt die Steuerung bedingungslos aus der Jump-Anweisung an das Ziel. Bei solchen dazwischen liegenden `try` -Anweisungen, die Ausführung ist komplexer. Wenn der Jump-Anweisung beendet, eine oder mehrere wird `try` Blöcke mit verknüpften `finally` Blöcken Steuerelement anfänglich auf übertragen wird die `finally` Block des innersten `try` Anweisung. Wenn und die Steuerung über den Endpunkt erreicht eine `finally` auf Steuerelement-Block übertragen wird die `finally` Block der nächsten einschließenden `try` Anweisung. Dieser Prozess wird wiederholt, bis die `finally` Blöcke aller beteiligten `try` Anweisungen ausgeführt wurden.

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
die `finally` zwei zugeordneten Blöcken `try` -Anweisungen werden ausgeführt, bevor die Steuerung an das Ziel der Jump-Anweisung übertragen wird.

Die Ausgabe lautet wie folgt aus:
```
Before break
Innermost finally block
Outermost finally block
After break
```

### <a name="the-break-statement"></a>Die Break-Anweisung

Die `break` -Anweisung beendet der nächsten einschließenden `switch`, `while`, `do`, `for`, oder `foreach` Anweisung.

```antlr
break_statement
    : 'break' ';'
    ;
```

Das Ziel einer `break` Anweisung ist der Endpunkt der nächsten einschließenden `switch`, `while`, `do`, `for`, oder `foreach` Anweisung. Wenn eine `break` -Anweisung ist nicht eingeschlossen, indem eine `switch`, `while`, `do`, `for`, oder `foreach` -Anweisung ein Fehler während der Kompilierung auftritt.

Wenn mehrere `switch`, `while`, `do`, `for`, oder `foreach` Anweisungen geschachtelt sind, eine `break` Aussage gilt nur für die innerste-Anweisung. Um das Steuerelement über mehrere Verschachtelungsebenen übertragen einer `goto` Anweisung ([Goto-Anweisung](statements.md#the-goto-statement)) muss verwendet werden.

Ein `break` Anweisung kann nicht beendet eine `finally` Block ([der Try-Anweisung](statements.md#the-try-statement)). Wenn eine `break` -Anweisung befindet sich in eine `finally` blockieren, das Ziel des der `break` -Anweisung muss innerhalb eines Abonnements sein `finally` blockieren; andernfalls ein Kompilierungsfehler tritt auf.

Ein `break` -Anweisung wird wie folgt ausgeführt:

*  Wenn die `break` -Anweisung beendet eine oder mehrere `try` Blöcke mit verknüpften `finally` Blöcken Steuerelement anfänglich auf übertragen wird die `finally` Block des innersten `try` Anweisung. Wenn und die Steuerung über den Endpunkt erreicht eine `finally` auf Steuerelement-Block übertragen wird die `finally` Block der nächsten einschließenden `try` Anweisung. Dieser Prozess wird wiederholt, bis die `finally` Blöcke aller beteiligten `try` Anweisungen ausgeführt wurden.
*  Die Steuerung an das Ziel der `break` Anweisung.

Da eine `break` Anweisung bedingungslos überträgt die Steuerung an anderen Positionen wird der Endpunkt, der eine `break` -Anweisung ist nicht erreichbar.

### <a name="the-continue-statement"></a>Die Continue-Anweisung

Die `continue` Anweisung startet eine neue Iteration der nächsten einschließenden `while`, `do`, `for`, oder `foreach` Anweisung.

```antlr
continue_statement
    : 'continue' ';'
    ;
```

Das Ziel einer `continue` -Anweisung ist der Endpunkt, der die eingebettete Anweisung der nächsten einschließenden `while`, `do`, `for`, oder `foreach` Anweisung. Wenn eine `continue` -Anweisung ist nicht eingeschlossen, indem eine `while`, `do`, `for`, oder `foreach` -Anweisung ein Fehler während der Kompilierung auftritt.

Wenn mehrere `while`, `do`, `for`, oder `foreach` Anweisungen geschachtelt sind, eine `continue` Aussage gilt nur für die innerste-Anweisung. Um das Steuerelement über mehrere Verschachtelungsebenen übertragen einer `goto` Anweisung ([Goto-Anweisung](statements.md#the-goto-statement)) muss verwendet werden.

Ein `continue` Anweisung kann nicht beendet eine `finally` Block ([der Try-Anweisung](statements.md#the-try-statement)). Beim ein `continue` -Anweisung befindet sich in eine `finally` blockieren, das Ziel der `continue` -Anweisung muss innerhalb eines Abonnements sein `finally` blockieren; andernfalls tritt ein Fehler während der Kompilierung auf.

Ein `continue` -Anweisung wird wie folgt ausgeführt:

*  Wenn die `continue` -Anweisung beendet eine oder mehrere `try` Blöcke mit verknüpften `finally` Blöcken Steuerelement anfänglich auf übertragen wird die `finally` Block des innersten `try` Anweisung. Wenn und die Steuerung über den Endpunkt erreicht eine `finally` auf Steuerelement-Block übertragen wird die `finally` Block der nächsten einschließenden `try` Anweisung. Dieser Prozess wird wiederholt, bis die `finally` Blöcke aller beteiligten `try` Anweisungen ausgeführt wurden.
*  Die Steuerung an das Ziel der `continue` Anweisung.

Da eine `continue` Anweisung bedingungslos überträgt die Steuerung an anderen Positionen wird der Endpunkt, der eine `continue` -Anweisung ist nicht erreichbar.

### <a name="the-goto-statement"></a>Die goto-Anweisung

Die `goto` -Anweisung überträgt die Steuerung an eine Anweisung, die von einer Bezeichnung markiert ist.

```antlr
goto_statement
    : 'goto' identifier ';'
    | 'goto' 'case' constant_expression ';'
    | 'goto' 'default' ';'
    ;
```

Das Ziel einer `goto` *Bezeichner* ist die bezeichnete Anweisung mit der angegebenen Bezeichnung. Wenn eine Bezeichnung mit dem angegebenen Namen in das aktuelle Funktionselement nicht vorhanden ist, oder wenn die `goto` Anweisung ist nicht innerhalb des Bereichs für die Beschriftung, ein Fehler während der Kompilierung auftritt. Diese Regel ermöglicht die Verwendung von einem `goto` Anweisung, um das Steuerelement aus einem geschachtelten Bereich, jedoch nicht in einem geschachtelten Bereich übertragen. Im Beispiel
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
eine `goto` -Anweisung verwendet, um das Steuerelement aus einem geschachtelten Bereich zu übertragen.

Das Ziel einer `goto case` -Anweisung ist der Anweisungsliste im unmittelbar einschließenden `switch` Anweisung ([der Switch-Anweisung](statements.md#the-switch-statement)), enthält eine `case` Bezeichnung mit dem angegebenen konstanten Wert. Wenn die `goto case` -Anweisung ist nicht eingeschlossen, indem eine `switch` -Anweisung, wenn die *Constant_expression* kann nicht implizit konvertiert werden ([implizite Konvertierungen](conversions.md#implicit-conversions)) auf der vorherrschende Datentyp der nächste einschließende `switch` -Anweisung, oder wenn der nächsten einschließenden `switch` Anweisung enthält keine `case` Bezeichnung mit dem angegebenen konstanten Wert ein Fehler während der Kompilierung auftritt.

Das Ziel einer `goto default` -Anweisung ist der Anweisungsliste im unmittelbar einschließenden `switch` Anweisung ([der Switch-Anweisung](statements.md#the-switch-statement)), enthält eine `default` Bezeichnung. Wenn die `goto default` -Anweisung ist nicht eingeschlossen, indem eine `switch` -Anweisung, oder, wenn der nächsten einschließenden `switch` Anweisung enthält keine `default` beschriften, tritt ein Fehler während der Kompilierung.

Ein `goto` Anweisung kann nicht beendet eine `finally` Block ([der Try-Anweisung](statements.md#the-try-statement)). Beim eine `goto` -Anweisung befindet sich in eine `finally` blockieren, das Ziel der `goto` -Anweisung muss innerhalb eines Abonnements sein `finally` Block, oder andernfalls ein Kompilierungsfehler tritt auf.

Ein `goto` -Anweisung wird wie folgt ausgeführt:

*  Wenn die `goto` -Anweisung beendet eine oder mehrere `try` Blöcke mit verknüpften `finally` Blöcken Steuerelement anfänglich auf übertragen wird die `finally` Block des innersten `try` Anweisung. Wenn und die Steuerung über den Endpunkt erreicht eine `finally` auf Steuerelement-Block übertragen wird die `finally` Block der nächsten einschließenden `try` Anweisung. Dieser Prozess wird wiederholt, bis die `finally` Blöcke aller beteiligten `try` Anweisungen ausgeführt wurden.
*  Die Steuerung an das Ziel der `goto` Anweisung.

Da eine `goto` Anweisung bedingungslos überträgt die Steuerung an anderen Positionen wird der Endpunkt, der eine `goto` -Anweisung ist nicht erreichbar.

### <a name="the-return-statement"></a>Die return-Anweisung

Die `return` -Anweisung übergibt die Kontrolle an den aktuellen Aufrufer der Funktion in der die `return` -Anweisung angezeigt wird.

```antlr
return_statement
    : 'return' expression? ';'
    ;
```

Ein `return` Anweisung ohne Ausdruck kann verwendet werden, nur in ein Funktionsmember, die keinen Wert, d. h. eine Methode mit dem Ergebnistyp berechnet ([Methodentext](classes.md#method-body)) `void`, `set` Accessor einer Eigenschaft oder Indexer der `add` und `remove` Accessoren der ein Ereignis, ein Instanzkonstruktor, einen statischen Konstruktor oder Destruktor.

Ein `return` -Anweisung mit einem Ausdruck kann nur verwendet werden, in ein Funktionsmember, die einen Wert, d. h. eine Methode mit einem nicht-Void-Ergebnistyp, berechnet die `get` Accessor einer Eigenschaft oder der Indexer oder ein benutzerdefinierter Operator. Eine implizite Konvertierung ([implizite Konvertierungen](conversions.md#implicit-conversions)) muss vorhanden sein, von dem Typ des Ausdrucks in den Rückgabetyp des enthaltenden Elements Funktion.

Zurückgeben, Anweisungen können auch verwendet werden, im Text der anonyme Funktionsausdrücke ([anonyme Funktionsausdrücke](expressions.md#anonymous-function-expressions)), und daran teilnehmen, die bestimmen, welche Konvertierungen für diese Funktionen vorhanden sind.

Es ist ein Fehler während der Kompilierung für eine `return` -Anweisung auftreten einer `finally` Block ([der Try-Anweisung](statements.md#the-try-statement)).

Ein `return` -Anweisung wird wie folgt ausgeführt:

*  Wenn die `return` -Anweisung gibt einen Ausdruck, der Ausdruck wird ausgewertet, und der resultierende Wert wird durch eine implizite Konvertierung in den Rückgabetyp der enthaltenden Funktion konvertiert. Das Ergebnis der Konvertierung wird der Ergebniswert, der von der Funktion generiert wurde.
*  Wenn die `return` -Anweisung wird durch eine oder mehrere eingeschlossen `try` oder `catch` Blöcke mit verknüpften `finally` Blöcken Steuerelement anfänglich auf übertragen wird die `finally` Block des innersten `try` Anweisung. Wenn und die Steuerung über den Endpunkt erreicht eine `finally` auf Steuerelement-Block übertragen wird die `finally` Block der nächsten einschließenden `try` Anweisung. Dieser Prozess wird wiederholt, bis die `finally` Blöcke alle einschließenden `try` Anweisungen ausgeführt wurden.
*  Wenn die enthaltende-Funktion keine Async-Funktion ist, wird die Steuerung an den Aufrufer der enthaltenden Funktion zusammen mit dem Ergebniswert ggf. zurückgegeben.
*  Wenn die enthaltende-Funktion eine asynchrone Funktion ist, die Steuerung wieder an den aktuellen Aufrufer und der Ergebniswert, sofern vorhanden, in der return-Aufgabe aufgezeichnet wird wie in beschrieben ([Enumeratorschnittstellen](classes.md#enumerator-interfaces)).

Da eine `return` Anweisung bedingungslos überträgt die Steuerung an anderen Positionen wird der Endpunkt, der eine `return` -Anweisung ist nicht erreichbar.

### <a name="the-throw-statement"></a>Die Throw-Anweisung

Die `throw` Anweisung löst eine Ausnahme aus.

```antlr
throw_statement
    : 'throw' expression? ';'
    ;
```

Ein `throw` -Anweisung mit einem Ausdruck löst aus, der von der Auswertung des Ausdrucks erzeugte Wert. Der Ausdruck muss einen Wert des Klassentyps deuten `System.Exception`, eines Klassentyps, die von abgeleitet `System.Exception` oder von einem Typ der Type-Parameter, die `System.Exception` (oder eine Unterklasse davon) als effektive Basisklasse. Wenn die Auswertung des Ausdrucks erzeugt `null`, `System.NullReferenceException` sondern ausgelöst.

Ein `throw` Anweisung ohne Ausdruck kann nur in verwendet werden eine `catch` blockieren, die in diesem Fall erneut die Anweisung wird die Ausnahme ausgelöst, die derzeit von diesem behandelt wird `catch` Block.

Da eine `throw` Anweisung bedingungslos überträgt die Steuerung an anderen Positionen wird der Endpunkt, der eine `throw` -Anweisung ist nicht erreichbar.

Wenn eine Ausnahme ausgelöst wird, wird die Steuerung an die erste `catch` -Klausel in einer einschließenden `try` -Anweisung, die die Ausnahme behandeln kann. Der Prozess, der ab dem Zeitpunkt der ausgelösten Ausnahme zum Zeitpunkt des Übertragens der Steuerung an einen geeigneten Ausnahmehandler stattfindet, wird als bezeichnet ***ausnahmeweitergabe***. Weitergabe einer Ausnahme besteht aus der Auswertung wiederholt die folgenden Schritte aus, bis eine `catch` -Klausel, die die Ausnahme entspricht gefunden wird. In dieser Beschreibung die ***Punkt auslösen*** beträgt anfänglich der Speicherort, an dem die Ausnahme wird ausgelöst.

*  In das aktuelle Funktionselement jede `try` -Anweisung, die den Throw einschließt, wird untersucht. Für jede Anweisung `S`, beginnend mit der innersten `try` -Anweisung und bis hin zu den äußersten `try` -Anweisung werden ausgewertet, die folgenden Schritte aus:

   * Wenn die `try` -Block `S` umschließt der auslösepunkt und S eine oder mehrere `catch` -Klauseln, die `catch` -Klauseln nacheinander überprüft werden, in der Reihenfolge ihres Auftretens einen geeigneten Handler für die Ausnahme, gemäß den Regeln, die im angegebenen gefunden Abschnitt [der Try-Anweisung](statements.md#the-try-statement). Wenn kein übereinstimmendes `catch` Klausel befindet, die ausnahmeweitergabe erfolgt durch Übergabe der Steuerung an den Block, `catch` Klausel.

   * Andernfalls gilt: Wenn die `try` Block oder ein `catch` -Block `S` umschließt der auslösepunkt und, wenn `S` verfügt über eine `finally` auf Steuerelement-Block übertragen wird die `finally` Block. Wenn die `finally` Block eine weitere Ausnahme auslöst, die Verarbeitung der aktuellen Ausnahme beendet. Wenn die Steuerung über den Endpunkt erreicht, andernfalls die `finally` Block Verarbeitung der aktuellen Ausnahme wird fortgesetzt.

*  Wenn ein Ausnahmehandler in den aktuellen Funktionsaufruf nicht gefunden wurde, der Funktionsaufruf wird beendet und eine der folgenden Bedingungen zutrifft:

   * Wenn die aktuelle Funktion nicht asynchronen ist, werden die oben genannten Schritte wiederholt für den Aufrufer der Funktion mit einer Throw-Punkt für die Anweisung, die aus der das Funktionselement aufgerufen wurde.

   * Ist die aktuelle Funktion Async "und" Aufgabe zurückgibt, wird die Ausnahme in der Task zurückgeben, die in einem fehlerhaften oder abgebrochenen Zustand versetzt wird, wie in beschrieben aufgezeichnet [Enumeratorschnittstellen](classes.md#enumerator-interfaces).

   * Wenn die aktuelle Funktion Async "und" void "zurückgebende ist, benachrichtigt der Synchronisierungskontext des aktuellen Threads wie in beschrieben [Enumerable-Schnittstellen](classes.md#enumerable-interfaces).

*  Wenn die ausnahmeverarbeitung alle Member für Funktionsaufrufe im aktuellen Thread beendet wird, ist, der angibt, dass der Thread kein Handler für die Ausnahme, hat der Thread selbst beendet. Die Auswirkungen des Abbruchs ist implementierungsdefiniert.

## <a name="the-try-statement"></a>Der Try-Anweisung

Die `try` Anweisung bietet einen Mechanismus zum Abfangen von Ausnahmen, die während der Ausführung eines Blocks auftreten. Darüber hinaus die `try` Anweisung bietet die Möglichkeit, einen Codeblock angeben, die immer ausgeführt wird, wenn das Steuerelement verlässt die `try` Anweisung.

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

Es gibt drei mögliche Formen von `try` Anweisungen:

*  Ein `try` -Block gefolgt von einer oder mehreren `catch` Blöcke.
*  Ein `try` -Block gefolgt von einem `finally` Block.
*  Ein `try` -Block gefolgt von einer oder mehreren `catch` Blöcke gefolgt von einem `finally` Block.

Wenn eine `catch` -Klausel gibt ein *Exception_specifier*, aufweisen muss `System.Exception`, ein Typ, der von abgeleitet ist `System.Exception` oder einen Typ-Parametertyp, die `System.Exception` (oder eine Unterklasse davon) als der effektive Basis-Klasse.

Wenn eine `catch` -Klausel gibt sowohl eine *Exception_specifier* mit eine *Bezeichner*, eine ***Ausnahmevariable*** mit dem angegebenen Namen und Typ deklariert wird. Die Ausnahmevariable entspricht einer lokalen Variablen mit einem Bereich, der über erweitert die `catch` Klausel. Während der Ausführung der *Exception_filter* und *Block*, die Ausnahmevariable stellt die Ausnahme, die gerade verarbeitet wird. Für Zwecke der definitive Zuweisungen zu überprüfen gilt die Ausnahmevariable im gesamten Bereich definitiv zugewiesen.

Es sei denn, eine `catch` -Klausel enthält einen Variablennamen der Ausnahme, es ist nicht möglich, auf das Ausnahmeobjekt im Filter und `catch` Block.

Ein `catch` -Klausel nicht angegeben ist, die eine *Exception_specifier* wird aufgerufen, eine allgemeine `catch` Klausel.

Einige Programmiersprachen unterstützen möglicherweise Ausnahmen, die nicht darstellbar sind, wie ein Objekt abgeleitet `System.Exception`, obwohl solche Ausnahmen niemals von c#-Code generiert werden können. Eine allgemeine `catch` -Klausel kann verwendet werden, um solche Ausnahmen abzufangen. Daher eine allgemeine `catch` -Klausel ist semantisch, die den Typ angibt, `System.Exception`, die erste auch aus anderen Sprachen Abfangen von Ausnahmen kann.

Um einen Handler für eine Ausnahme aus, suchen `catch` -Klauseln nacheinander überprüft werden, in der lexikalischen Reihenfolge. Wenn eine `catch` -Klausel gibt an, ein Typ, aber keine Ausnahmefilter, es ist ein Fehler während der Kompilierung für eine höhere `catch` Klausel in der gleichen `try` -Anweisung geben Sie einen Typ, die entspricht, oder wird abgeleitet, die eingeben. Wenn eine `catch` -Klausel gibt an, kein Typ und kein Filter, er muss die letzte `catch` -Klausel, die für die `try` Anweisung.

Innerhalb einer `catch` Block eine `throw` Anweisung ([die Throw-Anweisung](statements.md#the-throw-statement)) kein Ausdruck kann verwendet werden, um die Ausnahme erneut auszulösen, die von abgefangen wurde die `catch` Block. Zuweisungen für eine Ausnahmevariable ändern nicht die Ausnahme, die erneut ausgelöst wird.

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
die Methode `F` wird eine Ausnahme abgefangen, Diagnoseinformationen an die Konsole schreibt, ändert die Ausnahmevariable und löst die Ausnahme erneut aus. Die Ausnahme, die erneut ausgelöst wird ist die ursprüngliche Ausnahme, ist die Ausgabe:
```
Exception in F: G
Exception in Main: G
```

Wenn die erste Catch-Block ausgelöst hatte `e` anstatt erneut die aktuelle Ausnahme ausgelöst, würde die Ausgabe wie folgt lauten:
```csharp
Exception in F: G
Exception in Main: F
```

Es ist ein Fehler während der Kompilierung für eine `break`, `continue`, oder `goto` Anweisung, um das Übergeben der Steuerung aus einer `finally` Block. Wenn eine `break`, `continue`, oder `goto` -Anweisung befindet sich eine `finally` Block, der das Ziel der Anweisung muss im selben `finally` Block oder andernfalls ein Kompilierungsfehler tritt auf.

Es ist ein Fehler während der Kompilierung für eine `return` -Anweisung auftreten, in eine `finally` Block.

Ein `try` -Anweisung wird wie folgt ausgeführt:

*  Die Steuerung an die `try` Block.
*  Wenn und die Steuerung über den Endpunkt erreicht die `try` blockieren:
   *  Wenn die `try` Anweisung verfügt über eine `finally` Block, der `finally` Block wird ausgeführt.
   *  Die Steuerung an den Endpunkt der `try` Anweisung.

*  Wenn eine Ausnahme, um weitergegeben wird die `try` Anweisung während der Ausführung der `try` blockieren:
   *  Die `catch` -Klauseln, sofern vorhanden, werden in der Reihenfolge ihres Auftretens einen geeigneten Handler für die Ausnahme gefunden überprüft. Wenn eine `catch` -Klausel kein Typ angegeben ist, oder gibt an, der Typ der Ausnahme oder ein Basistyp des Ausnahmetyps:
      *  Wenn die `catch` -Klausel deklariert eine Ausnahmevariable, die Ausnahmevariable das Ausnahmeobjekt zugewiesen wird.
      *  Wenn die `catch` -Klausel deklariert einen Ausnahmefilter, der Filter ausgewertet wird. Ergibt die Auswertung `false`die Catch-Klausel ist keine Übereinstimmung und die Suche wird fortgesetzt, bis alle nachfolgenden `catch` Klauseln für einen geeigneten Handler.
      *  Andernfalls die `catch` Klausel wird als Übereinstimmung angesehen, und die Steuerung an den entsprechenden `catch` Block.
      *  Wenn und die Steuerung über den Endpunkt erreicht die `catch` blockieren:
         * Wenn die `try` Anweisung verfügt über eine `finally` Block, der `finally` Block wird ausgeführt.
         * Die Steuerung an den Endpunkt der `try` Anweisung.
      *  Wenn eine Ausnahme, um weitergegeben wird die `try` Anweisung während der Ausführung der `catch` blockieren:
         *  Wenn die `try` Anweisung verfügt über eine `finally` Block, der `finally` Block wird ausgeführt.
         *  Die Ausnahme wird an der nächsten einschließenden weitergegeben `try` Anweisung.
   *  Wenn die `try` Anweisung hat keine `catch` Klauseln oder wenn keine `catch` Klausel entspricht, der die Ausnahme:
      *  Wenn die `try` Anweisung verfügt über eine `finally` Block, der `finally` Block wird ausgeführt.
      *  Die Ausnahme wird an der nächsten einschließenden weitergegeben `try` Anweisung.

Die Anweisungen des eine `finally` Block werden immer ausgeführt, wenn das Steuerelement verlässt eine `try` Anweisung. Dies ist "true" gibt an, ob die Steuerelement-Übertragung infolge einer normalen Ausführung, die als Ergebnis der Ausführung erfolgt eine `break`, `continue`, `goto`, oder `return` -Anweisung oder als Ergebnis eine Ausnahme von der `try` -Anweisung.

Wenn eine Ausnahme, während der Ausführung ausgelöst wird einer `finally` blockieren, und nicht abgefangen innerhalb desselben finally-Block, der die Ausnahme an der nächsten einschließenden weitergegeben wird `try` Anweisung. Wenn eine andere Ausnahme war gerade weitergegeben wird, geht diese Ausnahme verloren. Erläutert der Prozess der Weitergabe einer Ausnahme in der Beschreibung der weiteren der `throw` Anweisung ([die Throw-Anweisung](statements.md#the-throw-statement)).

Die `try` -Block eine `try` Anweisung erreichbar ist. wenn die `try` -Anweisung erreichbar ist.

Ein `catch` -Block eine `try` Anweisung erreichbar ist. wenn die `try` -Anweisung erreichbar ist.

Die `finally` -Block eine `try` Anweisung erreichbar ist. wenn die `try` -Anweisung erreichbar ist.

Der Endpunkt, der eine `try` -Anweisung ist erreichbar, wenn die beiden folgenden Bedingungen erfüllt sind:

*  Der Endpunkt des der `try` Block erreichbar ist oder das Ende der mindestens eine `catch` Block erreichbar ist.
*  Wenn eine `finally` Block vorhanden ist, den Endpunkt der `finally` Block erreichbar ist.

## <a name="the-checked-and-unchecked-statements"></a>Die Anweisungen checked und unchecked

Die `checked` und `unchecked` Anweisungen werden verwendet, um zu steuern die ***Kontext der überlaufprüfung*** für arithmetische Operationen für ganzzahlige Typen und Konvertierungen.

```antlr
checked_statement
    : 'checked' block
    ;

unchecked_statement
    : 'unchecked' block
    ;
```

Die `checked` Anweisung bewirkt, dass alle Ausdrücke in der *Block* in einem überprüften Kontext ausgewertet werden und die `unchecked` Anweisung bewirkt, dass alle Ausdrücke in der *Block* in ausgewertet werden ein nicht geprüften Kontext.

Die `checked` und `unchecked` -Anweisungen sind genaue Entsprechung der `checked` und `unchecked` Operatoren ([checked und unchecked Operatoren](expressions.md#the-checked-and-unchecked-operators)), außer dass sie Blöcke anstelle von Ausdrücken ausgeführt werden .

## <a name="the-lock-statement"></a>Die Lock-Anweisung

Die `lock` -Anweisung ruft die Sperre für gegenseitigen Ausschluss für ein bestimmtes Objekt, führt eine Anweisung aus und gibt dann die Sperre frei.

```antlr
lock_statement
    : 'lock' '(' expression ')' embedded_statement
    ;
```

Der Ausdruck eine `lock` Anweisung muss einen Wert eines Typs, die bekanntermaßen kennzeichnen einer *Reference_type*. Kein implizites Boxing-Konvertierung ([Boxingkonvertierungen](conversions.md#boxing-conversions)) erfolgt einmal für den Ausdruck, der eine `lock` -Anweisung, und daher ist ein Fehler während der Kompilierung für den Ausdruck zur Bezeichnung eines Werts von einer *Value_type*.

Ein `lock` -Anweisung der Form
```csharp
lock (x) ...
```
wo `x` ist ein Ausdruck, der eine *Reference_type*, entspricht exakt dem
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

Während eine Sperre für gegenseitigen Ausschluss gehalten wird, kann Ausführung von Code in der gleichen Ausführungsthread auch zu erhalten und die Sperre freigibt. Allerdings wird die Ausführung von Code in anderen Threads blockiert, die Sperre abrufen, bis die Sperre aufgehoben wird.

Sperren `System.Type` Objekte zum Synchronisieren des Zugriffs auf statische Daten wird nicht empfohlen. Anderer Code kann auf dem gleichen Typ, sperren, was zu Deadlocks führen kann. Ein besserer Ansatz ist, den Zugriff auf statische Daten zu synchronisieren, indem Sie ein privates statisches Objekt zu sperren. Zum Beispiel:
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

Die `using` -Anweisung ruft eine oder mehrere Ressourcen, führt eine Anweisung aus und klicken Sie dann die Ressource frei.

```antlr
using_statement
    : 'using' '(' resource_acquisition ')' embedded_statement
    ;

resource_acquisition
    : local_variable_declaration
    | expression
    ;
```

Ein ***Ressource*** ist eine Klasse oder Struktur, die implementiert `System.IDisposable`, einschließlich eine einzelne parameterlose Methode, die mit dem Namen `Dispose`. Erreichen von Code, der eine Ressource verwendet `Dispose` um anzugeben, dass die Ressource nicht mehr benötigt wird. Wenn `Dispose` nicht aufgerufen wird, wird automatische Löschung letztendlich als Folge der Garbagecollection durchgeführt.

Wenn die Form der *Resource_acquisition* ist *Local_variable_declaration* der Typ des der *Local_variable_declaration* muss `dynamic` oder einen Typ der implizit in konvertiert werden kann `System.IDisposable`. Wenn die Form der *Resource_acquisition* ist *Ausdruck* dieser Ausdruck muss implizit in sein `System.IDisposable`.

Lokale Variablen, die in einem *Resource_acquisition* sind schreibgeschützt und muss einen Initialisierer enthalten. Ein Fehler während der Kompilierung tritt auf, wenn die eingebettete Anweisung versucht, diese lokalen Variablen zu ändern (per Zuweisung oder `++` und `--` Operatoren), nehmen sie die Adresse, oder übergeben Sie sie als `ref` oder `out` Parameter.

Ein `using` -Anweisung übersetzt in drei Teile: Abruf, Verwendung und Freigabe. Bei Verwendung der Ressource steht implizit in einen `try` -Anweisung mit einer `finally` Klausel. Dies `finally` Klausel gibt die Ressource frei. Wenn eine `null` Ressource wird abgerufen, dann kein Aufruf von `Dispose` vorgenommen wird, und es wird keine Ausnahme ausgelöst. Wenn die Ressource vom Typ `dynamic` wird dynamisch durch eine implizite Konvertierung für die dynamische Konvertierung ([implizite dynamische Konvertierungen](conversions.md#implicit-dynamic-conversions)) zu `IDisposable` während der Übernahme, um sicherzustellen, dass die Konvertierung ist vor der Nutzung und die Freigabe erfolgreich.

Ein `using` -Anweisung der Form
```csharp
using (ResourceType resource = expression) statement
```
entspricht einem der drei möglichen Erweiterungen. Wenn `ResourceType` ist ein Typ NULL-Werte, die Erweiterung ist
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

Andernfalls gilt bei `ResourceType` ist ein Werttyp oder ein Verweistyp als `dynamic`, ist die Erweiterung
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

Andernfalls gilt bei `ResourceType` ist `dynamic`, ist die Erweiterung
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

In beiden Erweiterung der `resource` Variable ist schreibgeschützt und in die eingebettete Anweisung, und die `d` Variable ist nicht sichtbar und kann nicht zugegriffen werden, in an, die eingebettete Anweisung.

Eine Implementierung ist auf eine bestimmte using-Anweisung auf andere Weise, z. B. zur Verbesserung der Leistung, implementiert zulässig, solange das Verhalten aufgrund der oben genannten steigenden konsistent ist.

Ein `using` -Anweisung der Form
```csharp
using (expression) statement
```
verfügt über die gleichen drei möglichen Erweiterungen. In diesem Fall `ResourceType` ist implizit der Kompilierzeit-Typ, der die `expression`, sofern vorhanden. Andernfalls die Schnittstelle `IDisposable` selbst dient als die `ResourceType`. Die `resource` Variable ist nicht sichtbar und kann nicht zugegriffen werden, in an, die eingebettete Anweisung.

Wenn eine *Resource_acquisition* nimmt die Form einer *Local_variable_declaration*, es ist möglich, mehrere Ressourcen eines bestimmten Typs abrufen. Ein `using` -Anweisung der Form
```csharp
using (ResourceType r1 = e1, r2 = e2, ..., rN = eN) statement
```
genau entspricht einer Sequenz von geschachtelt `using` Anweisungen:
```csharp
using (ResourceType r1 = e1)
    using (ResourceType r2 = e2)
        ...
            using (ResourceType rN = eN)
                statement
```

Das folgende Beispiel erstellt eine Datei namens `log.txt` und zwei Textzeilen in die Datei geschrieben. Im Beispiel wird dann der gleichen Datei zum Lesen geöffnet und die enthaltenen Textzeilen in der Konsole kopiert.
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

Da die `TextWriter` und `TextReader` -Klassen implementieren die `IDisposable` -Schnittstelle, im Beispiel können `using` stellen Sie sicher, dass die zugrunde liegende Datei ordnungsgemäß geschlossen wird nach dem Schreiben oder Lesen Sie die Operations-Anweisungen.

## <a name="the-yield-statement"></a>Die Yield-Anweisung

Die `yield` Anweisung wird in einem Iteratorblock verwendet ([Blöcke](statements.md#blocks)) ergibt einen Wert auf das Enumeratorobjekt ([Enumeratorobjekte](classes.md#enumerator-objects)) oder ein aufzählbares Objekt ([aufzählbare Objekte](classes.md#enumerable-objects)) ein Iterator oder das Ende der Iteration zu signalisieren.

```antlr
yield_statement
    : 'yield' 'return' expression ';'
    | 'yield' 'break' ';'
    ;
```

`yield` ist kein reserviertes Wort. Es hat eine besondere Bedeutung, die erst bei der unmittelbar vor einem `return` oder `break` Schlüsselwort. In anderen Kontexten `yield` als Bezeichner verwendet werden können.

Es gibt mehrere Einschränkungen dazu, wo ein `yield` Anweisung angezeigt werden kann, wie im folgenden beschrieben.

*  Es ist ein Fehler während der Kompilierung für eine `yield` -Anweisung (entweder Form) außerhalb angezeigt werden. eine *Method_body*, *Operator_body* oder *Accessor_body*
*  Es ist ein Fehler während der Kompilierung für eine `yield` -Anweisung (entweder Form) innerhalb einer anonymen Funktion angezeigt werden.
*  Es ist ein Fehler während der Kompilierung für eine `yield` -Anweisung (entweder Form) angezeigt werden die `finally` -Klausel einer `try` Anweisung.
*  Es ist ein Fehler während der Kompilierung für eine `yield return` Anweisung in eine beliebige Stelle angezeigt werden eine `try` -Anweisung, die enthält `catch` Klauseln.

Das folgende Beispiel zeigt einige gültigen und ungültigen Verwendungen von `yield` Anweisungen.

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

Eine implizite Konvertierung ([implizite Konvertierungen](conversions.md#implicit-conversions)) muss vorhanden sein, von dem Typ des Ausdrucks in der `yield return` Anweisung, um den Typ "yield" ([Yield-Typ](classes.md#yield-type)) des Iterators.

Ein `yield return` -Anweisung wird wie folgt ausgeführt:

*  Die in der Anweisung angegebene Ausdruck wird ausgewertet, implizit in den Typ "yield" konvertiert und zugewiesen der `Current` Eigenschaft des Enumeratorobjekts.
*  Die Ausführung des Iteratorblocks wird angehalten. Wenn die `yield return` -Anweisung ist in einem oder mehreren `try` blockiert wird, zugeordneten `finally` Blöcke werden zu diesem Zeitpunkt nicht ausgeführt.
*  Die `MoveNext` gibt die Methode des Enumeratorobjekts `true` an den Aufrufer, der angibt, dass das Enumeratorobjekt erfolgreich auf das nächste Element gesetzt wurde.

Der nächste Aufruf auf des Enumeratorobjekts `MoveNext` -Methode setzt die Ausführung des Iteratorblocks aus, in dem es zuletzt angehalten wurde.

Ein `yield break` -Anweisung wird wie folgt ausgeführt:

*  Wenn die `yield break` -Anweisung wird durch eine oder mehrere eingeschlossen `try` Blöcke mit verknüpften `finally` Blöcken Steuerelement anfänglich auf übertragen wird die `finally` Block des innersten `try` Anweisung. Wenn und die Steuerung über den Endpunkt erreicht eine `finally` auf Steuerelement-Block übertragen wird die `finally` Block der nächsten einschließenden `try` Anweisung. Dieser Prozess wird wiederholt, bis die `finally` Blöcke alle einschließenden `try` Anweisungen ausgeführt wurden.
*  Steuerelement wird an den Aufrufer des Iteratorblocks zurückgegeben. Dies ist entweder der `MoveNext` Methode oder `Dispose` Methode des Enumeratorobjekts.

Da eine `yield break` Anweisung bedingungslos überträgt die Steuerung an anderen Positionen wird der Endpunkt, der eine `yield break` -Anweisung ist nicht erreichbar.
