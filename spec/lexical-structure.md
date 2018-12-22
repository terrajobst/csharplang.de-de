# <a name="lexical-structure"></a>Lexikalische Struktur

## <a name="programs"></a>Programs

Eine C#- ***Programm*** besteht aus einem oder mehreren ***Quelldateien***, bekannt als formal ***Kompilierungseinheiten*** ([Kompilierungseinheiten](namespaces.md#compilation-units)). Eine Quelldatei ist eine geordnete Sequenz von Unicode-Zeichen. Quelldateien müssen in der Regel eine 1: 1-Entsprechung mit Dateien in einem Dateisystem, aber diese Zuordnung ist nicht erforderlich. Zur maximalen Portabilität wird empfohlen, die Dateien in einem Dateisystem mit UTF-8 codiert werden Codierung.

Konzeptuell gesehen, ist ein Programm kompiliert mit drei Schritten:

1. Diese Transformation, die eine Datei von einem bestimmten zeichenrepertoire und Codierungsschema in eine Folge von Unicode-Zeichen konvertiert.
2. Lexikalische Analyse, der einen Stream von Unicode-Zeichen-Eingabe in einen Stream von Token übersetzt.
3. Syntaktischen Analyse, der den Strom der Token in ausführbaren Code übersetzt.

## <a name="grammars"></a>Grammatiken

Diese Spezifikation enthält die Syntax der C#-Programmiersprache mit zwei Grammatiken. Die ***lexikalische Grammatik*** ([lexikalische Grammatik](lexical-structure.md#lexical-grammar)) definiert, wie Unicode-Zeichen kombiniert werden, um das Zeilenabschlusszeichen, Leerzeichen, Kommentare, Token und Präprozessordirektiven. Die ***syntaktischen Grammatik*** ([syntaktischen Grammatik](lexical-structure.md#syntactic-grammar)) definiert, wie die Token, die durch die lexikalische Grammatik auf C#-Programme kombiniert werden.

### <a name="grammar-notation"></a>Grammar notation

Der lexikalischen und syntaktischen Grammatik werden in Backus-Naur-Form, die mithilfe der Notation des ANTLR-Grammatik Tools angezeigt.

### <a name="lexical-grammar"></a>Lexikalische Grammatik

Die lexikalische Grammatik von c# werden im [Lexikalische Analyse](lexical-structure.md#lexical-analysis), [Token](lexical-structure.md#tokens), und [Präprozessordirektiven](lexical-structure.md#pre-processing-directives). Die terminal-Symbole der lexikalische Grammatik sind die Zeichen des Unicode-Zeichensatz aus, und die lexikalische Grammatik gibt an, wie Zeichen kombiniert werden, um Formular-Token ([Token](lexical-structure.md#tokens)), Leerzeichen ([Leerraum](lexical-structure.md#white-space)), Kommentare ([Kommentare](lexical-structure.md#comments)), und Präprozessordirektiven ([Präprozessordirektiven](lexical-structure.md#pre-processing-directives)).

Jede Quelldatei in einem C#-Programm entsprechen den *Eingabe* Produktion der lexikalische Grammatik ([Lexikalische Analyse](lexical-structure.md#lexical-analysis)).

### <a name="syntactic-grammar"></a>Syntaktischen Grammatik

Die syntaktische Grammatik der C# -Code wird angezeigt, in den Kapiteln und Anhänge, die diesem Kapitel nachvollziehen können. Die terminal-Symbole der syntaktischen Grammatik sind die Token, die durch die lexikalische Grammatik definiert, und die syntaktische Grammatik gibt an, wie Token C#-Programme kombiniert werden.

Jede Quelldatei in einen C# Programm entsprechen den *Compilation_unit* Produktion der syntaktischen Grammatik ([Kompilierungseinheiten](namespaces.md#compilation-units)).

## <a name="lexical-analysis"></a>Lexikalische Analyse

Die *Eingabe* Produktion definiert die lexikalische Struktur einer C#-Quelldatei. Jede Quelldatei in einem C#-Programm muss diese Produktion lexikalische Grammatik entsprechen.

```antlr
input
    : input_section?
    ;

input_section
    : input_section_part+
    ;

input_section_part
    : input_element* new_line
    | pp_directive
    ;

input_element
    : whitespace
    | comment
    | token
    ;
```

Die lexikalische Struktur besteht aus fünf grundlegenden Elementen eine C# Quelldatei: Zeilenabschlüssen ([Zeilenabschlüssen](lexical-structure.md#line-terminators)), Leerzeichen ([Leerraum](lexical-structure.md#white-space)), Kommentare ([Kommentare](lexical-structure.md#comments)), Token ([Token](lexical-structure.md#tokens)), und Präprozessordirektiven ([Präprozessordirektiven](lexical-structure.md#pre-processing-directives)). Diese grundlegenden Elementen, nur Token sind von Bedeutung, in der syntaktischen Grammatik eines C#-Programms ([syntaktischen Grammatik](lexical-structure.md#syntactic-grammar)).

Reduzieren Sie die Datei in eine Folge von Token, die die Eingabe für die syntaktischen Analyse wird, besteht die lexikalische Verarbeitung einer C#-Quelldatei aus. Zeilenabschlusszeichen, Leerzeichen und Kommentare dienen zum Trennen von Tokens, und Präprozessordirektiven können dazu führen, dass Teile der Quelldatei, die übersprungen werden, aber andernfalls diese lexikalischen Elemente haben keine Auswirkung auf die syntaktische Struktur eines C#-Programms.

Im Fall von interpolierten Zeichenfolgenliterale ([interpolierte Zeichenfolgenliterale](lexical-structure.md#interpolated-string-literals)) ein einzelnes Token wird anfänglich von der lexikalischen Analyse erstellt, aber in mehrere Eingabeelemente, die wiederholt bei der lexikalischen Analyse unterzogen werden unterteilt ist bis alle interpolierte Zeichenfolgenliterale gelöst wurden. Die daraus resultierende Token dienen dann als Eingabe für die syntaktischen Analyse.

Wenn mehrere lexikalische Grammatikproduktionen übereinstimmt, der eine Folge von Zeichen in einer Quelldatei, bildet der lexikalischen Verarbeitung immer die am längsten mögliche lexikalische Element angezeigt. Z. B. die Zeichensequenz `//` wird die als Anfang eines einzeiligen Kommentars verarbeitet werden, da dieses lexikalische Element mehr als eine einzelne ist `/` token.

### <a name="line-terminators"></a>Zeilenabschlusszeichen

Zeilenabschlusszeichen unterteilen Sie die Zeichen einer C#-Quelldatei in Zeilen.

```antlr
new_line
    : '<Carriage return character (U+000D)>'
    | '<Line feed character (U+000A)>'
    | '<Carriage return character (U+000D) followed by line feed character (U+000A)>'
    | '<Next line character (U+0085)>'
    | '<Line separator character (U+2028)>'
    | '<Paragraph separator character (U+2029)>'
    ;
```

Für Kompatibilität mit Source code Bearbeitungstools, die End-of-File-Marker hinzufügen, und zum Aktivieren einer Datenquelle Datei als Sequenz angezeigt werden, ordnungsgemäß Zeilen beendet, werden die folgenden Transformationen angewendet, in der Reihenfolge jede Quelldatei in einem C#-Programm:

*  Wenn das letzte Zeichen der Quelldatei ein STRG + Z-Zeichen ist (`U+001A`), wird dieses Zeichen gelöscht.
*  Ein Wagenrücklaufzeichen (`U+000D`) wird am Ende der Quelldatei hinzugefügt, wenn der Quelldatei nicht leer ist und das letzte Zeichen der Quelldatei kein Wagenrücklauf (`U+000D`), ein Zeilenvorschub (`U+000A`), ein Zeilentrennzeichen (`U+2028`), oder einen Absatzseparator (`U+2029`).

### <a name="comments"></a>Kommentare

Zwei Arten von Kommentaren werden unterstützt: einzeilige Kommentare und Kommentare mit Trennzeichen. ***Einzeilige Kommentare*** beginnen mit den Zeichen `//` und am Ende der Quellzeile erweitern. ***Als Trennzeichen Kommentare*** beginnen mit den Zeichen `/*` und enden mit den Zeichen `*/`. Kommentare mit Trennzeichen können es sich um mehrere Zeilen erstrecken.

```antlr
comment
    : single_line_comment
    | delimited_comment
    ;

single_line_comment
    : '//' input_character*
    ;

input_character
    : '<Any Unicode character except a new_line_character>'
    ;

new_line_character
    : '<Carriage return character (U+000D)>'
    | '<Line feed character (U+000A)>'
    | '<Next line character (U+0085)>'
    | '<Line separator character (U+2028)>'
    | '<Paragraph separator character (U+2029)>'
    ;

delimited_comment
    : '/*' delimited_comment_section* asterisk* '/'
    ;

delimited_comment_section
    : '/'
    | asterisk* not_slash_or_asterisk
    ;

asterisk
    : '*'
    ;

not_slash_or_asterisk
    : '<Any Unicode character except / or *>'
    ;
```

Kommentare schachteln nicht. Die Zeichensequenzen `/*` und `*/` haben keine spezielle Bedeutung in einer `//` Kommentar und die Zeichensequenzen `//` und `/*` haben keine spezielle Bedeutung in einer durch Trennzeichen getrennten Kommentar.

Kommentare werden in Zeichen- und Zeichenfolgenliterale nicht verarbeitet werden.

Im Beispiel
```csharp
/* Hello, world program
   This program writes "hello, world" to the console
*/
class Hello
{
    static void Main() {
        System.Console.WriteLine("hello, world");
    }
}
```
enthält einen durch Trennzeichen getrennten Kommentar.

Im Beispiel
```csharp
// Hello, world program
// This program writes "hello, world" to the console
//
class Hello // any name will do for this class
{
    static void Main() { // this method must be named "Main"
        System.Console.WriteLine("hello, world");
    }
}
```
Zeigt mehrere einzeilige Kommentare.

### <a name="white-space"></a>Leerraum

Leerraum wird definiert, als beliebiges Zeichen mit Unicode-Klasse Zs (einschließlich Leerzeichen) als auch das horizontale Tabulatorzeichen, die vertikales Tabstoppzeichen und das Formular Zeichen Feed.

```antlr
whitespace
    : '<Any character with Unicode class Zs>'
    | '<Horizontal tab character (U+0009)>'
    | '<Vertical tab character (U+000B)>'
    | '<Form feed character (U+000C)>'
    ;
```

## <a name="tokens"></a>tokens

Es gibt mehrere Arten von Token: Bezeichner, Schlüsselwörter, Literale, Operatoren und Markierungszeichen. Leerzeichen und Kommentare sind nicht-Token, obwohl sie als Trennzeichen für Token verwendet werden.

```antlr
token
    : identifier
    | keyword
    | integer_literal
    | real_literal
    | character_literal
    | string_literal
    | interpolated_string_literal
    | operator_or_punctuator
    ;
```

### <a name="unicode-character-escape-sequences"></a>Unicode-Escapesequenzen für Steuerzeichen

Eine Unicode-Zeichen-Escapesequenz stellt ein Unicode-Zeichen dar. Escapesequenzen für Unicode-Zeichen in Bezeichnern verarbeitet werden ([Bezeichner](lexical-structure.md#identifiers)), Zeichenliterale ([Zeichenliterale](lexical-structure.md#character-literals)), und Reguläre Zeichenfolgenliterale ([Zeichenfolgenliterale](lexical-structure.md#string-literals)). Die Escapesequenz für ein Unicode-Zeichen wird nicht in einem anderen Speicherort (z. B. um einen Operator, Markierungszeichen oder Schlüsselwort bilden) verarbeitet.

```antlr
unicode_escape_sequence
    : '\\u' hex_digit hex_digit hex_digit hex_digit
    | '\\U' hex_digit hex_digit hex_digit hex_digit hex_digit hex_digit hex_digit hex_digit
    ;
```

Eine Unicode-Escape-Sequenz darstellt, das einzelne Unicode-Zeichen gebildet, indem die hexadezimale Zahl nach der "`\u`"oder"`\U`" Zeichen. Da c# eine 16-Bit-Codierung von Unicode-Codepunkte in Zeichen und Zeichenfolgenwerte verwendet, wird ein Unicode-Zeichen im Bereich U + 10000 bis U + 10FFFF ist nicht in einem Zeichenliteral zulässig und wird mithilfe eines Unicode-Ersatzzeichenpaars in einem Zeichenfolgenliteral dargestellt. Mit den Codepunkten oben 0x10FFFF Unicode-Zeichen werden nicht unterstützt.

Mehrere Übersetzungen werden nicht ausgeführt. Z. B. das Zeichenfolgenliteral "`\u005Cu005C`"ist gleich"`\u005C`"statt"`\`". Der Unicode-Wert `\u005C` ist das Zeichen "`\`".

Im Beispiel
```csharp
class Class1
{
    static void Test(bool \u0066) {
        char c = '\u0066';
        if (\u0066)
            System.Console.WriteLine(c.ToString());
    }        
}
```
zeigt verschiedene Verwendungsmöglichkeiten des `\u0066`, d.h., dass die Escapesequenz für den Buchstaben "`f`". Das Programm ist gleich
```csharp
class Class1
{
    static void Test(bool f) {
        char c = 'f';
        if (f)
            System.Console.WriteLine(c.ToString());
    }        
}
```

### <a name="identifiers"></a>Bezeichner

Die Regeln für Bezeichner, die in diesem Abschnitt angegebene entsprechen exakt mit den empfohlenen durch den Unicode Standard Annex 31, mit dem Unterschied, dass Unterstrich als erste Zeichen zulässig ist (wie in der Programmiersprache C herkömmliche ist), Unicode-Escapesequenzen, die Sequenzen sind in Bezeichnern zulässig und der "`@`" Zeichen ist zulässig, als Präfix zu Schlüsselwörtern als Bezeichner verwendet werden soll.

```antlr
identifier
    : available_identifier
    | '@' identifier_or_keyword
    ;

available_identifier
    : '<An identifier_or_keyword that is not a keyword>'
    ;

identifier_or_keyword
    : identifier_start_character identifier_part_character*
    ;

identifier_start_character
    : letter_character
    | '_'
    ;

identifier_part_character
    : letter_character
    | decimal_digit_character
    | connecting_character
    | combining_character
    | formatting_character
    ;

letter_character
    : '<A Unicode character of classes Lu, Ll, Lt, Lm, Lo, or Nl>'
    | '<A unicode_escape_sequence representing a character of classes Lu, Ll, Lt, Lm, Lo, or Nl>'
    ;

combining_character
    : '<A Unicode character of classes Mn or Mc>'
    | '<A unicode_escape_sequence representing a character of classes Mn or Mc>'
    ;

decimal_digit_character
    : '<A Unicode character of the class Nd>'
    | '<A unicode_escape_sequence representing a character of the class Nd>'
    ;

connecting_character
    : '<A Unicode character of the class Pc>'
    | '<A unicode_escape_sequence representing a character of the class Pc>'
    ;

formatting_character
    : '<A Unicode character of the class Cf>'
    | '<A unicode_escape_sequence representing a character of the class Cf>'
    ;
```

Informationen auf die oben genannten Unicode-Zeichenklassen finden Sie im Unicode-Standard, Version 3.0, Abschnitt 4.5.

Beispiele für gültige Bezeichner sind "`identifier1`","`_identifier2`", und "`@if`".

Gemäß Unicode Standard Annex 15, muss ein Bezeichner in einem entsprechenden Programm im kanonischen Format durch Unicode-Normalisierungsform C, definiert sein. Das Verhalten, wenn einen Bezeichner nicht in der Normalisierungsform C auftreten wird die Implementierung definiert; eine Diagnose ist jedoch nicht erforderlich.

Das Präfix "`@`" ermöglicht die Verwendung von Schlüsselwörtern als Bezeichner, was nützlich ist, für die Kommunikation mit anderen Programmiersprachen. Das Zeichen `@` ist nicht Bestandteil des Bezeichners, damit der Bezeichner in anderen Sprachen als normale Bezeichner, ohne das Präfix angezeigt wird. Ein Bezeichner mit einem `@` Präfix wird aufgerufen, eine ***ausführliche Bezeichner***. Verwenden der `@` Präfix für Bezeichner entsprechen, die keine Schlüsselwörter sind zulässig, jedoch nicht empfohlen, als eine Frage des Stils.

Beispiel:
```csharp
class @class
{
    public static void @static(bool @bool) {
        if (@bool)
            System.Console.WriteLine("true");
        else
            System.Console.WriteLine("false");
    }    
}

class Class1
{
    static void M() {
        cl\u0061ss.st\u0061tic(true);
    }
}
```
definiert eine Klasse namens "`class`"mit einer statischen Methode, die mit dem Namen"`static`", akzeptiert einen Parameter namens"`bool`". Beachten Sie, die seit der Unicode-Escapesequenzen in Schlüsselwörter, die das Token nicht zulässig sind "`cl\u0061ss`"ist ein Bezeichner, und der gleiche Bezeichner wie ist"`@class`".

Zwei Bezeichner gelten als identisch, wenn sie identisch sind, nachdem Sie die folgenden Transformationen in der Reihenfolge angewendet werden:

*  Das Präfix "`@`", wenn verwendet, wird entfernt.
*  Jede *Unicode_escape_sequence* wird umgewandelt in das entsprechende Unicode-Zeichen.
*  Alle *Formatting_character*s werden entfernt.

Bezeichner mit zwei aufeinander folgende Unterstriche (`U+005F`) für die Verwendung durch die Implementierung reserviert sind. Beispielsweise kann eine Implementierung erweiterter Schlüsselwörter bereitstellen, die mit zwei unterstrichen beginnen.

### <a name="keywords"></a>Schlüsselwörter

Ein ***Schlüsselwort*** ist eine Bezeichner-ähnliche Folge von Zeichen, das ist reserviert und kann nicht als Bezeichner außer bei vorangestelltem verwendet werden die `@` Zeichen.

```antlr
keyword
    : 'abstract' | 'as'       | 'base'       | 'bool'      | 'break'
    | 'byte'     | 'case'     | 'catch'      | 'char'      | 'checked'
    | 'class'    | 'const'    | 'continue'   | 'decimal'   | 'default'
    | 'delegate' | 'do'       | 'double'     | 'else'      | 'enum'
    | 'event'    | 'explicit' | 'extern'     | 'false'     | 'finally'
    | 'fixed'    | 'float'    | 'for'        | 'foreach'   | 'goto'
    | 'if'       | 'implicit' | 'in'         | 'int'       | 'interface'
    | 'internal' | 'is'       | 'lock'       | 'long'      | 'namespace'
    | 'new'      | 'null'     | 'object'     | 'operator'  | 'out'
    | 'override' | 'params'   | 'private'    | 'protected' | 'public'
    | 'readonly' | 'ref'      | 'return'     | 'sbyte'     | 'sealed'
    | 'short'    | 'sizeof'   | 'stackalloc' | 'static'    | 'string'
    | 'struct'   | 'switch'   | 'this'       | 'throw'     | 'true'
    | 'try'      | 'typeof'   | 'uint'       | 'ulong'     | 'unchecked'
    | 'unsafe'   | 'ushort'   | 'using'      | 'virtual'   | 'void'
    | 'volatile' | 'while'
    ;
```

An einigen Stellen in der Grammatik spezifische Bezeichner haben eine besondere Bedeutung, aber sind keine Schlüsselwörter. Solche Bezeichner werden manchmal als "kontextabhängige Schlüsselwörter" bezeichnet. Z. B. in einer Eigenschaftendeklaration die "`get`"und"`set`" Bezeichner haben eine besondere Bedeutung ([Accessoren](classes.md#accessors)). Andere Bezeichner als `get` oder `set` ist nie in diesen Speicherorten zulässig, sodass diese Verwendung nicht zu Konflikten mit der Verwendung der diese Wörter nicht als Bezeichner. In anderen Fällen, z. B. wie Sie mit der ID "`var`" in Deklarationen von implizit typisierten lokalen Variablen ([lokale Variablendeklarationen](statements.md#local-variable-declarations)), ein kontextbezogenes Schlüsselwort kann es zu Konflikten mit deklarierten Namen. In solchen Fällen hat der deklarierte Name Vorrang vor der Verwendung des Bezeichners als ein kontextbezogenes Schlüsselwort.

### <a name="literals"></a>Literale

Ein ***literal*** ist eine Source-Code-Darstellung eines Werts.

```antlr
literal
    : boolean_literal
    | integer_literal
    | real_literal
    | character_literal
    | string_literal
    | null_literal
    ;
```

#### <a name="boolean-literals"></a>Boolesche Literale

Es gibt zwei boolesche literale Werte: `true` und `false`.

```antlr
boolean_literal
    : 'true'
    | 'false'
    ;
```

Der Typ des eine *Boolean_literal* ist `bool`.

#### <a name="integer-literals"></a>Ganzzahlenliteral

Integer-Literale werden verwendet, um die Werte der Typen schreiben `int`, `uint`, `long`, und `ulong`. Integer-Literale haben zwei Formen: dezimalen und hexadezimalen.

```antlr
integer_literal
    : decimal_integer_literal
    | hexadecimal_integer_literal
    ;

decimal_integer_literal
    : decimal_digit+ integer_type_suffix?
    ;

decimal_digit
    : '0' | '1' | '2' | '3' | '4' | '5' | '6' | '7' | '8' | '9'
    ;

integer_type_suffix
    : 'U' | 'u' | 'L' | 'l' | 'UL' | 'Ul' | 'uL' | 'ul' | 'LU' | 'Lu' | 'lU' | 'lu'
    ;

hexadecimal_integer_literal
    : '0x' hex_digit+ integer_type_suffix?
    | '0X' hex_digit+ integer_type_suffix?
    ;

hex_digit
    : '0' | '1' | '2' | '3' | '4' | '5' | '6' | '7' | '8' | '9'
    | 'A' | 'B' | 'C' | 'D' | 'E' | 'F' | 'a' | 'b' | 'c' | 'd' | 'e' | 'f';
```

Der Typ, der ein Integer-literal wird wie folgt bestimmt:

*  Wenn das Literal kein Suffix besitzt, hat die erste dieser Typen, die in dem sein Wert dargestellt werden kann: `int`, `uint`, `long`, `ulong`.
*  Wenn das Literal durch angehängt ist. `U` oder `u`, hat die erste dieser Typen, die in dem sein Wert dargestellt werden kann: `uint`, `ulong`.
*  Wenn das Literal durch angehängt ist. `L` oder `l`, hat die erste dieser Typen, die in dem sein Wert dargestellt werden kann: `long`, `ulong`.
*  Wenn das Literal durch angehängt ist. `UL`, `Ul`, `uL`, `ul`, `LU`, `Lu`, `lU`, oder `lu`, ist vom Typ `ulong`.

Der durch ein Ganzzahlliteral dargestellte Wert liegt außerhalb des Bereichs von der `ulong` eingeben, ein Fehler während der Kompilierung auftritt.

Als eine Frage des Stils, es wird empfohlen, die "`L`"werden verwendet anstelle von"`l`" beim Schreiben von literalen Typs `long`, da sie leicht zu verwechseln Sie den Buchstaben "`l`"mit der Ziffer"`1`".

Um den kleinsten möglichen ermöglichen `int` und `long` Werte geschrieben werden, als ganze Dezimalzahl, die Literale, die folgenden zwei Regeln vorhanden sind:

* Wenn eine *Decimal_integer_literal* mit dem Wert 2147483648 (2 ^ 31) und es wird kein *Integer_type_suffix* angezeigt wird, wie das Token, die ein unäres minus Operatortoken unmittelbar nach ([unäres minus Operator](expressions.md#unary-minus-operator)), das Ergebnis ist eine Konstante des Typs `int` mit dem Wert von – 2147483648 (-2 ^ 31). In allen anderen Fällen solche eine *Decimal_integer_literal* ist vom Typ `uint`.
* Wenn eine *Decimal_integer_literal* mit dem Wert 9223372036854775808 (2 ^ 63) und es wird kein *Integer_type_suffix* oder *Integer_type_suffix* `L` oder `l` angezeigt wird, wie das Token direkt nach der ein unäres minus Operatortoken ([unäre Operator minus](expressions.md#unary-minus-operator)), das Ergebnis ist eine Konstante des Typs `long` mit dem Wert-9223372036854775808 (-2 ^ 63). In allen anderen Fällen solche eine *Decimal_integer_literal* ist vom Typ `ulong`.

#### <a name="real-literals"></a>Real-Literale

Echte Literale werden verwendet, um die Werte der Typen schreiben `float`, `double`, und `decimal`.

```antlr
real_literal
    : decimal_digit+ '.' decimal_digit+ exponent_part? real_type_suffix?
    | '.' decimal_digit+ exponent_part? real_type_suffix?
    | decimal_digit+ exponent_part real_type_suffix?
    | decimal_digit+ real_type_suffix
    ;

exponent_part
    : 'e' sign? decimal_digit+
    | 'E' sign? decimal_digit+
    ;

sign
    : '+'
    | '-'
    ;

real_type_suffix
    : 'F' | 'f' | 'D' | 'd' | 'M' | 'm'
    ;
```

Wenn kein *Real_type_suffix* angegeben wird, wird der Typ des Literals echte `double`. Das Suffix realer Typ bestimmt den Typ des Literals real, andernfalls wie folgt:

*  Ein reelles Literal `F` oder `f` ist vom Typ `float`. Z. B. die Literale `1f`, `1.5f`, `1e10f`, und `123.456F` sind "all" des Typs `float`.
*  Ein reelles Literal `D` oder `d` ist vom Typ `double`. Z. B. die Literale `1d`, `1.5d`, `1e10d`, und `123.456D` sind "all" des Typs `double`.
*  Ein reelles Literal `M` oder `m` ist vom Typ `decimal`. Z. B. die Literale `1m`, `1.5m`, `1e10m`, und `123.456M` sind "all" des Typs `decimal`. Dieses Literal wird konvertiert, um eine `decimal` Wert durch das der genaue Wert, und bei Bedarf zu runden, auf den nächsten darstellbaren Wert mit Banker rounding ([Dezimaltyps](types.md#the-decimal-type)). Jeder Größe, die aus dem Literal wird beibehalten, es sei denn, der Wert wird gerundet, oder der Wert 0 (null ist) (in diesem zweiten Fall die Anmeldung und die Dezimalstellen gleich 0 ist). Daher das Literal `2.900m` wird analysiert, um die Dezimalzahl mit Vorzeichen bilden `0`, Koeffizient `2900`, und skalieren `3`.

Wenn das angegebene Literal in den angegebenen Typ dargestellt werden kann, tritt ein Fehler während der Kompilierung.

Der Wert des ein reales Literal vom Typ `float` oder `double` richtet sich nach der Verwendung des IEEE "auf den nächsten Wert runden" Modus.

Beachten Sie, dass ein reales Literal, die Dezimalstellen müssen immer nach dem Dezimaltrennzeichen an. Z. B. `1.3F` ist eine Literal für reelle Zahlen aber `1.F` nicht.

#### <a name="character-literals"></a>Zeichenliterale

Ein Zeichenfolgenliteral, ein einzelnes Zeichen darstellt, und in der Regel besteht aus einem Zeichen in Anführungszeichen ein, wie in `'a'`.

Hinweis: Die ANTLR Grammar Notation macht die folgenden verwirrend. Im ANTLR, beim Schreiben von `\'` er steht für ein einfaches Anführungszeichen `'`. Und beim Erstellen von `\\` er steht für ein einzelner umgekehrter Schrägstrich `\`. Aus diesem Grund bedeutet die erste Regel für ein Zeichenliteral an, dass er mit einem einfachen Anführungszeichen und ein einfaches Anführungszeichen, dann ein Zeichen beginnt. Die elf möglich einfache Escapesequenzen sind `\'`, `\"`, `\\`, `\0`, `\a`, `\b`, `\f`, `\n`, `\r`, `\t`, `\v`.

```antlr
character_literal
    : '\'' character '\''
    ;

character
    : single_character
    | simple_escape_sequence
    | hexadecimal_escape_sequence
    | unicode_escape_sequence
    ;

single_character
    : '<Any character except \' (U+0027), \\ (U+005C), and new_line_character>'
    ;

simple_escape_sequence
    : '\\\'' | '\\"' | '\\\\' | '\\0' | '\\a' | '\\b' | '\\f' | '\\n' | '\\r' | '\\t' | '\\v'
    ;

hexadecimal_escape_sequence
    : '\\x' hex_digit hex_digit? hex_digit? hex_digit?;
```

Ein Zeichen, das einen umgekehrten Schrägstrich folgt (`\`) in einem *Zeichen* muss eines der folgenden Zeichen: `'`, `"`, `\`, `0`, `a`, `b` , `f`, `n`, `r`, `t`, `u`, `U`, `x`, `v`. Andernfalls tritt ein Kompilierungsfehler auf.

Eine hexadezimale Escapesequenz stellt ein einzelnes Unicode-Zeichen dar, mit dem Wert, der durch die hexadezimale Zahl nach gebildet "`\x`".

Wenn der Wert, der durch ein Zeichenliteral dargestellt größer ist `U+FFFF`, ein Fehler während der Kompilierung auftritt.

Eine Unicode-Zeichen-Escapesequenz ([Unicode-Escape-Zeichensequenzen](lexical-structure.md#unicode-character-escape-sequences)) in einem Zeichenliteral muss im Bereich von `U+0000` zu `U+FFFF`.

Eine einfache Escapesequenz dar, der eine Unicode-zeichencodierung, in der folgenden Tabelle beschrieben.


| __Escape-Sequenz__ | __Zeichenname__ | __Unicode-Codierung__ |
|---------------------|--------------------|----------------------|
| `\'`                | Einfaches Anführungszeichen       | `0x0027`             | 
| `\"`                | Doppeltes Anführungszeichen       | `0x0022`             | 
| `\\`                | Umgekehrter Schrägstrich          | `0x005C`             | 
| `\0`                | Null               | `0x0000`             | 
| `\a`                | Warnung              | `0x0007`             | 
| `\b`                | Rückschritt          | `0x0008`             | 
| `\f`                | Seitenvorschub          | `0x000C`             | 
| `\n`                | Zeilenwechsel           | `0x000A`             | 
| `\r`                | Wagenrücklauf    | `0x000D`             | 
| `\t`                | Horizontaler Tabulator     | `0x0009`             | 
| `\v`                | Vertikaler Tabulator       | `0x000B`             | 

Der Typ des eine *Character_literal* ist `char`.

#### <a name="string-literals"></a>Zeichenfolgenliterale

C# unterstützt zwei Arten von Zeichenfolgenliteralen: ***reguläre zeichenfolgeliterale*** und ***zeichenfolgeliterale***.

Ein regulären Zeichenfolgenliterals besteht aus null oder mehr Zeichen, die in doppelte Anführungszeichen eingeschlossen werden, wie in `"hello"`, und enthält möglicherweise sowohl einfache Escapesequenzen (wie z. B. `\t` für Tabstopps), und hexadezimaler Schreibweise und im Unicode-Escapesequenzen.

Ein ausführliches Zeichenfolgenliteral besteht aus einem `@` Zeichens, gefolgt von NULL oder mehr Zeichen, ein doppeltes Anführungszeichen und eine schließende Anführungszeichen. Ein einfaches Beispiel ist `@"hello"`. In der ein ausführliches Zeichenfolgenliteral, werden die Zeichen zwischen den Trennzeichen wörtlich interpretiert die einzige Ausnahme werden eine *Quote_escape_sequence*. Insbesondere werden einfache Escapesequenzen und hexadezimal- und Unicode-Escapesequenzen in wörtliche Zeichenfolgenliterale nicht verarbeitet. Ein ausführliches Zeichenfolgenliteral kann mehrere Zeilen erstrecken.

```antlr
string_literal
    : regular_string_literal
    | verbatim_string_literal
    ;

regular_string_literal
    : '"' regular_string_literal_character* '"'
    ;

regular_string_literal_character
    : single_regular_string_literal_character
    | simple_escape_sequence
    | hexadecimal_escape_sequence
    | unicode_escape_sequence
    ;

single_regular_string_literal_character
    : '<Any character except " (U+0022), \\ (U+005C), and new_line_character>'
    ;

verbatim_string_literal
    : '@"' verbatim_string_literal_character* '"'
    ;

verbatim_string_literal_character
    : single_verbatim_string_literal_character
    | quote_escape_sequence
    ;

single_verbatim_string_literal_character
    : '<any character except ">'
    ;

quote_escape_sequence
    : '""'
    ;
```

Ein Zeichen, das einen umgekehrten Schrägstrich folgt (`\`) in einem *Regular_string_literal_character* muss eines der folgenden Zeichen: `'`, `"`, `\`, `0`, `a` , `b`, `f`, `n`, `r`, `t`, `u`, `U`, `x`, `v`. Andernfalls tritt ein Kompilierungsfehler auf.

Im Beispiel
```csharp
string a = "hello, world";                   // hello, world
string b = @"hello, world";                  // hello, world

string c = "hello \t world";                 // hello      world
string d = @"hello \t world";                // hello \t world

string e = "Joe said \"Hello\" to me";       // Joe said "Hello" to me
string f = @"Joe said ""Hello"" to me";      // Joe said "Hello" to me

string g = "\\\\server\\share\\file.txt";    // \\server\share\file.txt
string h = @"\\server\share\file.txt";       // \\server\share\file.txt

string i = "one\r\ntwo\r\nthree";
string j = @"one
two
three";
```
Zeigt eine Vielzahl von Zeichenfolgenliteralen. Die letzte Zeichenfolge Zeichenliteral `j`, ist ein ausführliches Zeichenfolgenliteral, die mehrere Zeilen umfasst. Die Zeichen zwischen den Anführungszeichen, einschließlich Leerzeichen, z. B. neue-Zeile-Zeichen werden wörtlich beibehalten.

Da eine hexadezimale Escapesequenz eine Variable Anzahl von hexadezimalen Ziffern, das Zeichenfolgenliteral aufweisen kann `"\x123"` enthält ein einzelnes Zeichen mit hexadezimaler Wert 123. Um eine Zeichenfolge, enthält das Zeichen mit hexadezimaler Wert 12 gefolgt vom Zeichen 3 zu erstellen, könnten eine `"\x00123"` oder `"\x12" + "3"` stattdessen.

Der Typ des eine *String_literal* ist `string`.

Jeder String-literal führt nicht unbedingt in eine neue Zeichenfolgeninstanz. Wenn zwei oder mehr Zeichenfolgenliterale, die entsprechen gemäß den Zeichenfolge-Equality-Operator ([Zeichenfolge Gleichheitsoperatoren](expressions.md#string-equality-operators)) angezeigt werden im selben Programm, und diese Zeichenfolgenliterale verweisen, auf die gleiche Zeichenfolgeninstanz. Z. B. die Ausgabe von
```csharp
class Test
{
    static void Main() {
        object a = "hello";
        object b = "hello";
        System.Console.WriteLine(a == b);
    }
}
```
ist `True` da die beiden Literale auf die gleiche Zeichenfolgeninstanz verweisen.

#### <a name="interpolated-string-literals"></a>Interpolierte Zeichenfolgenliterale

Interpolierte Zeichenfolgenliterale sind vergleichbar mit Zeichenfolgenliteralen, aber Löcher aufweisen, getrennt durch `{` und `}`, bei dem Ausdrücke auftreten können. Zur Laufzeit werden die Ausdrücke ausgewertet, Ziel müssen ihre Text-Formate ersetzt, die in der Zeichenfolge an der Stelle, wo die Lücke auftritt. Die Syntax und Semantik der zeichenfolgeninterpolation werden im Abschnitt beschrieben ([interpolierte Zeichenfolgen](expressions.md#interpolated-strings)).

Wie Zeichenfolgenliterale können es sich bei interpolierte Zeichenfolgenliterale reguläre oder ausführliche sein. Interpolierte reguläre zeichenfolgeliterale werden gesetzte `$"` und `"`, und durch die interpolierte zeichenfolgeliterale getrennt sind `$@"` und `"`.

Wie andere Literale führt die lexikalische Analyse einer interpolierten Zeichenfolge literal zunächst in einem einzelnen Token, gemäß der folgenden Grammatik. Allerdings vor der syntaktischen Analyse, die einzelne Token einer interpolierten Zeichenfolge literal wird in mehrere Token für die Teile der Zeichenfolge, die die Löcher einschließenden unterteilt, und der Eingabeelemente, die in die Löcher auftreten werden lexikalisch erneut analysiert. Dies kann wiederum führen mehr interpolierte Zeichenfolgenliterale verarbeitet werden, jedoch, wenn lexikalisch korrigieren, letztendlich dazu führen, dass eine Folge von Token für die syntaktischen Analyse verarbeitet.

```antlr
interpolated_string_literal
    : '$' interpolated_regular_string_literal
    | '$' interpolated_verbatim_string_literal
    ;

interpolated_regular_string_literal
    : interpolated_regular_string_whole
    | interpolated_regular_string_start  interpolated_regular_string_literal_body interpolated_regular_string_end
    ;

interpolated_regular_string_literal_body
    : regular_balanced_text
    | interpolated_regular_string_literal_body interpolated_regular_string_mid regular_balanced_text
    ;

interpolated_regular_string_whole
    : '"' interpolated_regular_string_character* '"'
    ;

interpolated_regular_string_start
    : '"' interpolated_regular_string_character* '{'
    ;

interpolated_regular_string_mid
    : interpolation_format? '}' interpolated_regular_string_characters_after_brace? '{'
    ;

interpolated_regular_string_end
    : interpolation_format? '}' interpolated_regular_string_characters_after_brace? '"'
    ;

interpolated_regular_string_characters_after_brace
    : interpolated_regular_string_character_no_brace
    | interpolated_regular_string_characters_after_brace interpolated_regular_string_character
    ;

interpolated_regular_string_character
    : single_interpolated_regular_string_character
    | simple_escape_sequence
    | hexadecimal_escape_sequence
    | unicode_escape_sequence
    | open_brace_escape_sequence
    | close_brace_escape_sequence
    ;

interpolated_regular_string_character_no_brace
    : '<Any interpolated_regular_string_character except close_brace_escape_sequence and any hexadecimal_escape_sequence or unicode_escape_sequence designating } (U+007D)>'
    ;

single_interpolated_regular_string_character
    : '<Any character except \" (U+0022), \\ (U+005C), { (U+007B), } (U+007D), and new_line_character>'
    ;

open_brace_escape_sequence
    : '{{'
    ;

    close_brace_escape_sequence
    : '}}'
    ;
    
regular_balanced_text
    : regular_balanced_text_part+
    ;

regular_balanced_text_part
    : single_regular_balanced_text_character
    | delimited_comment
    | '@' identifier_or_keyword
    | string_literal
    | interpolated_string_literal
    | '(' regular_balanced_text ')'
    | '[' regular_balanced_text ']'
    | '{' regular_balanced_text '}'
    ;
    
single_regular_balanced_text_character
    : '<Any character except / (U+002F), @ (U+0040), \" (U+0022), $ (U+0024), ( (U+0028), ) (U+0029), [ (U+005B), ] (U+005D), { (U+007B), } (U+007D) and new_line_character>'
    | '</ (U+002F), if not directly followed by / (U+002F) or * (U+002A)>'
    ;
    
interpolation_format
    : interpolation_format_character+
    ;
    
interpolation_format_character
    : '<Any character except \" (U+0022), : (U+003A), { (U+007B) and } (U+007D)>'
    ;
    
interpolated_verbatim_string_literal
    : interpolated_verbatim_string_whole
    | interpolated_verbatim_string_start interpolated_verbatim_string_literal_body interpolated_verbatim_string_end
    ;

interpolated_verbatim_string_literal_body
    : verbatim_balanced_text
    | interpolated_verbatim_string_literal_body interpolated_verbatim_string_mid verbatim_balanced_text
    ;
    
interpolated_verbatim_string_whole
    : '@"' interpolated_verbatim_string_character* '"'
    ;
    
interpolated_verbatim_string_start
    : '@"' interpolated_verbatim_string_character* '{'
    ;
    
interpolated_verbatim_string_mid
    : interpolation_format? '}' interpolated_verbatim_string_characters_after_brace? '{'
    ;
    
interpolated_verbatim_string_end
    : interpolation_format? '}' interpolated_verbatim_string_characters_after_brace? '"'
    ;
    
interpolated_verbatim_string_characters_after_brace
    : interpolated_verbatim_string_character_no_brace
    | interpolated_verbatim_string_characters_after_brace interpolated_verbatim_string_character
    ;
    
interpolated_verbatim_string_character
    : single_interpolated_verbatim_string_character
    | quote_escape_sequence
    | open_brace_escape_sequence
    | close_brace_escape_sequence
    ;
    
interpolated_verbatim_string_character_no_brace
    : '<Any interpolated_verbatim_string_character except close_brace_escape_sequence>'
    ;
    
single_interpolated_verbatim_string_character
    : '<Any character except \" (U+0022), { (U+007B) and } (U+007D)>'
    ;
    
verbatim_balanced_text
    : verbatim_balanced_text_part+
    ;

verbatim_balanced_text_part
    : single_verbatim_balanced_text_character
    | comment
    | '@' identifier_or_keyword
    | string_literal
    | interpolated_string_literal
    | '(' verbatim_balanced_text ')'
    | '[' verbatim_balanced_text ']'
    | '{' verbatim_balanced_text '}'
    ;
    
single_verbatim_balanced_text_character
    : '<Any character except / (U+002F), @ (U+0040), \" (U+0022), $ (U+0024), ( (U+0028), ) (U+0029), [ (U+005B), ] (U+005D), { (U+007B) and } (U+007D)>'
    | '</ (U+002F), if not directly followed by / (U+002F) or * (U+002A)>'
    ;
```

Ein *Interpolated_string_literal* Token ist neu interpretiert, als mehrere Token und andere Elemente wie folgt in der Reihenfolge des Auftretens im Eingabe der *Interpolated_string_literal*:

* Vorkommen der folgenden werden neu interpretiert, als separate einzelner Token: das führende `$` Sign *Interpolated_regular_string_whole*, *Interpolated_regular_string_start*, *Interpolated_regular_string_mid*, *Interpolated_regular_string_end*, *Interpolated_verbatim_string_whole*,  *Interpolated_verbatim_string_start*, *Interpolated_verbatim_string_mid* und *Interpolated_verbatim_string_end*.
* Vorkommen des *Regular_balanced_text* und *Verbatim_balanced_text* zwischen diesen als verarbeitet eine *Input_section* ([Lexikalische Analyse ](lexical-structure.md#lexical-analysis)) und werden neu interpretiert, als der resultierenden Sequenz von Eingabeelementen. Diesen zählen möglicherweise wiederum die interpolierte Zeichenfolge literaltoken neu interpretiert werden.

Syntaktische Analyse werden die Token in zusammenzusetzen ein *Interpolated_string_expression* ([interpolierte Zeichenfolgen](expressions.md#interpolated-strings)).

TODO-Beispiele


#### <a name="the-null-literal"></a>Das null-literal

```antlr
null_literal
    : 'null'
    ;
```

Die *Null_literal* implizit in einem Referenztyp oder nullable-Typ konvertiert werden kann.

### <a name="operators-and-punctuators"></a>Operatoren und Markierungszeichen

Es gibt verschiedene Arten von Operatoren und Markierungszeichen. Operatoren sind in Ausdrücken verwendet, um Vorgänge im Zusammenhang mit einem oder mehreren Operanden zu beschreiben. Beispiel: der Ausdruck `a + b` verwendet die `+` Operator, um die beiden Operanden hinzufügen `a` und `b`. Markierungszeichen sind für das Gruppieren und zu trennen.

```antlr
operator_or_punctuator
    : '{'  | '}'  | '['  | ']'  | '('   | ')'  | '.'  | ','  | ':'  | ';'
    | '+'  | '-'  | '*'  | '/'  | '%'   | '&'  | '|'  | '^'  | '!'  | '~'
    | '='  | '<'  | '>'  | '?'  | '??'  | '::' | '++' | '--' | '&&' | '||'
    | '->' | '==' | '!=' | '<=' | '>='  | '+=' | '-=' | '*=' | '/=' | '%='
    | '&=' | '|=' | '^=' | '<<' | '<<=' | '=>'
    ;

right_shift
    : '>>'
    ;

right_shift_assignment
    : '>>='
    ;
```

Der senkrechte Strich in der *Right_shift* und *Right_shift_assignment* Produktionen werden verwendet, um anzugeben, dass, im Gegensatz zu anderen Produktionen in der syntaktischen Grammatik, die keine Zeichen jeglicher Art (nicht einmal Leerzeichen) zwischen den Token zulässig sind. Diese Produktionen werden behandelt, besonders, um die richtige Verarbeitung von aktivieren *Type_parameter_list*s ([Typparameter](classes.md#type-parameters)).

## <a name="pre-processing-directives"></a>Präprozessordirektiven

Die Präprozessordirektiven bieten die Möglichkeit, bedingte Abschnitte von Quelldateien, Fehler und Warnungen hin, überspringen und skizziert werden verschiedene Bereiche des Quellcodes. Der Begriff "Präprozessordirektiven" wird nur für Konsistenz mit den Programmiersprachen C und C++ verwendet. In C# geschrieben ist es keine separate vorbearbeitung von; Präprozessordirektiven werden als Teil der lexikalischen Analyse verarbeitet.

```antlr
pp_directive
    : pp_declaration
    | pp_conditional
    | pp_line
    | pp_diagnostic
    | pp_region
    | pp_pragma
    ;
```

Die folgenden Anweisungen für die vorverarbeitung sind verfügbar:

*  `#define` und `#undef`, die werden verwendet, um zu definieren und Symboldefinitionen aufheben, bzw. für die bedingte Kompilierung ([Deklaration Direktiven](lexical-structure.md#declaration-directives)).
*  `#if`, `#elif`, `#else`, und `#endif`, die werden verwendet, um bedingt Abschnitte des Quellcodes überspringen ([Bedingte Kompilierungsdirektiven](lexical-structure.md#conditional-compilation-directives)).
*  `#line`, dient zum Steuern von Zeilennummern für Fehler und Warnungen ausgegeben ([Zeile Direktiven](lexical-structure.md#line-directives)).
*  `#error` und `#warning`, die zum Fehler und Warnungen, geben Sie jeweils verwendet werden ([diagnostische Direktiven](lexical-structure.md#diagnostic-directives)).
*  `#region` und `#endregion`, die werden verwendet, um Abschnitte des Quellcodes explizit zu kennzeichnen ([Bereichsdirektiven](lexical-structure.md#region-directives)).
*  `#pragma`, das verwendet, um optionale Kontextinformationen für den Compiler anzugeben ([Pragma-Direktiven](lexical-structure.md#pragma-directives)).

Eine Präprozessordirektive nimmt immer auf eine separate Zeile des Quellcodes und beginnt immer mit einem `#` Zeichen- und ein vorab verarbeitetes Direktivenname. Leerzeichen vor dem Auftreten der `#` Zeichen und zwischen den `#` Zeichen und den Namen der Standarddirektive.

Zeile für eine Datenquelle mit einem `#define`, `#undef`, `#if`, `#elif`, `#else`, `#endif`, `#line`, oder `#endregion` Richtlinie möglicherweise ein einzeiliger Kommentar endet. Kommentare getrennt (die `/* */` Stil von Kommentaren) dürfen nicht auf Quellzeilen, die Präprozessordirektiven enthält.

Präprozessordirektiven sind keine Token und sind nicht Teil der syntaktischen Grammatik von c#. Allerdings Präprozessordirektiven können ein-oder Ausschließen von Sequenzen von Token verwendet werden und können auf diese Weise beeinträchtigen die Bedeutung eines C#-Programms. Beispielsweise wird bei der Kompilierung kann des Programms:
```csharp
#define A
#undef B

class C
{
#if A
    void F() {}
#else
    void G() {}
#endif

#if B
    void H() {}
#else
    void I() {}
#endif
}
```
die Ergebnisse in genau dieselbe Sequenz von Token wie das Programm:
```csharp
class C
{
    void F() {}
    void I() {}
}
```

Daher während lexikalisch, die beiden Programme syntaktisch ganz unterschiedlich sind, sind sie identisch.

### <a name="conditional-compilation-symbols"></a>Symbole für bedingte Kompilierung

Die Funktionalität für die bedingte Kompilierung von der `#if`, `#elif`, `#else`, und `#endif` Direktiven wird über die vorverarbeitung Ausdrücke gesteuert ([Präprozessorausdrücke](lexical-structure.md#pre-processing-expressions)) und Symbole für bedingte Kompilierung.

```antlr
conditional_symbol
    : '<Any identifier_or_keyword except true or false>'
    ;
```

Ein Symbol für bedingte Kompilierung hat zwei mögliche Zustände: ***definiert*** oder ***undefiniert***. Am Anfang der lexikalischen Verarbeitung einer Quelldatei ist ein Symbol für bedingte Kompilierung nicht definiert, es sei denn, sie durch einen externen Mechanismus (z. B. eine Befehlszeilen-Compiler-Option) explizit definiert wurde. Wenn eine `#define` Richtlinie verarbeitet wird, wird das Symbol für bedingte Kompilierung, die mit dem Namen, die in der Richtlinie wird definiert, in der Quelldatei. Es bleibt das Symbol definiert, bis ein `#undef` die Richtlinie für das gleiche Symbol verarbeitet wird oder bis das Ende der Quelldatei erreicht ist. Eine davon wird `#define` und `#undef` Anweisungen in einer Quelldatei haben keine Auswirkungen auf andere Quelldateien, die im selben Programm.

Wenn in einem vorab verarbeiteten Ausdruck verwiesen wird, ist ein Symbol definiert, für die bedingte Kompilierung den booleschen Wert `true`, und ein Symbol nicht definiert, für die bedingte Kompilierung hat den booleschen Wert `false`. Es ist nicht erforderlich, werden Symbole für bedingte Kompilierung explizit deklariert werden, bevor sie in Ausdrücken für die vorverarbeitung referenziert werden. Stattdessen nicht deklarierte Symbole sind einfach nicht definiert, und daher haben den Wert `false`.

Der Namespace für die Symbole für bedingte Kompilierung ist verschieden und getrennt von anderen benannten Entitäten in einem C#-Programm. Symbole für bedingte Kompilierung darf nur verwiesen werden, `#define` und `#undef` Anweisungen und Ausdrücke vorab zu verarbeiten.

### <a name="pre-processing-expressions"></a>Vorab verarbeitete Ausdrücke

Ausdrücke für die vorverarbeitung kann in auftreten `#if` und `#elif` Anweisungen. Die Operatoren `!`, `==`, `!=`, `&&` und `||` sind in Ausdrücken für die vorverarbeitung zulässig und Klammern zum Gruppieren verwendet werden können.

```antlr
pp_expression
    : whitespace? pp_or_expression whitespace?
    ;

pp_or_expression
    : pp_and_expression
    | pp_or_expression whitespace? '||' whitespace? pp_and_expression
    ;

pp_and_expression
    : pp_equality_expression
    | pp_and_expression whitespace? '&&' whitespace? pp_equality_expression
    ;

pp_equality_expression
    : pp_unary_expression
    | pp_equality_expression whitespace? '==' whitespace? pp_unary_expression
    | pp_equality_expression whitespace? '!=' whitespace? pp_unary_expression
    ;

pp_unary_expression
    : pp_primary_expression
    | '!' whitespace? pp_unary_expression
    ;

pp_primary_expression
    : 'true'
    | 'false'
    | conditional_symbol
    | '(' whitespace? pp_expression whitespace? ')'
    ;
```

Wenn in einem vorab verarbeiteten Ausdruck verwiesen wird, ist ein Symbol definiert, für die bedingte Kompilierung den booleschen Wert `true`, und ein Symbol nicht definiert, für die bedingte Kompilierung hat den booleschen Wert `false`.

Auswertung immer ein vorab verarbeitetes Ausdrucks ergibt einen booleschen Wert. Die Regeln für die Auswertung für einen vorab verarbeitete Ausdruck sind identisch mit denen für einen konstanten Ausdruck ([Konstante Ausdrücke](expressions.md#constant-expressions)), außer dass die einzigen benutzerdefinierte Entitäten, auf die verwiesen werden können, Symbole für bedingte Kompilierung sind .

### <a name="declaration-directives"></a>Deklaration-Direktiven

Die Deklaration-Anweisungen werden verwendet, um zu definieren bzw. dessen Definition aufzuheben Symbole für bedingte Kompilierung.

```antlr
pp_declaration
    : whitespace? '#' whitespace? 'define' whitespace conditional_symbol pp_new_line
    | whitespace? '#' whitespace? 'undef' whitespace conditional_symbol pp_new_line
    ;

pp_new_line
    : whitespace? single_line_comment? new_line
    ;
```

Die Verarbeitung einer `#define` Richtlinie bewirkt, dass die angegebenen bedingtes Kompilierungssymbol definiert wird, beginnend mit der Quellzeile, die die Direktive folgt. Ebenso wird die Verarbeitung von einer `#undef` Richtlinie bewirkt, dass das angegebene konditionale kompiliersymbol aufgehoben wird, beginnend mit der Quellzeile, die die Direktive folgt.

Alle `#define` und `#undef` Anweisungen in einer Quelldatei müssen vor dem ersten Auftreten *token* ([Token](lexical-structure.md#tokens)) in der Quelldatei; andernfalls ein Fehler während der Kompilierung auftritt. Intuitiv ausgedrückt `#define` und `#undef` Anweisungen müssen vor jeder Code"real" in der Quelldatei stehen.

Beispiel:
```csharp
#define Enterprise

#if Professional || Enterprise
    #define Advanced
#endif

namespace Megacorp.Data
{
    #if Advanced
    class PivotTable {...}
    #endif
}
```
gültig ist da die `#define` Anweisungen vorausgehen, das erste Token (das `namespace` Schlüsselwort) in der Quelldatei.

Im folgende Beispiel führt zu einem Fehler während der Kompilierung auf, da eine `#define` echten Code folgt:
```csharp
#define A
namespace N
{
    #define B
    #if B
    class Class1 {}
    #endif
}
```

Ein `#define` kann ein Symbol für bedingte Kompilierung, die bereits definiert ist, ohne dass alle dazwischen liegenden vorhanden definieren `#undef` für das Symbol. Das folgende Beispiel definiert ein Symbol für bedingte Kompilierung `A` und definiert es dann noch mal.
```csharp
#define A
#define A
```

Ein `#undef` möglicherweise "Symboldefinitionen" für die bedingte Kompilierung, die nicht definiert ist. Das folgende Beispiel definiert ein Symbol für bedingte Kompilierung `A` , und klicken Sie dann auch zweimal Aufhebungen der zweiten `#undef` hat keine Auswirkungen, es ist noch gültig ist.
```csharp
#define A
#undef A
#undef A
```

### <a name="conditional-compilation-directives"></a>Anweisungen für bedingte Kompilierung

Anweisungen für die bedingte Kompilierung werden verwendet, um bedingten ein- bzw. Ausschließen von Teilen einer Quelldatei.

```antlr
pp_conditional
    : pp_if_section pp_elif_section* pp_else_section? pp_endif
    ;

pp_if_section
    : whitespace? '#' whitespace? 'if' whitespace pp_expression pp_new_line conditional_section?
    ;

pp_elif_section
    : whitespace? '#' whitespace? 'elif' whitespace pp_expression pp_new_line conditional_section?
    ;

pp_else_section:
    | whitespace? '#' whitespace? 'else' pp_new_line conditional_section?
    ;

pp_endif
    : whitespace? '#' whitespace? 'endif' pp_new_line
    ;

conditional_section
    : input_section
    | skipped_section
    ;

skipped_section
    : skipped_section_part+
    ;

skipped_section_part
    : skipped_characters? new_line
    | pp_directive
    ;

skipped_characters
    : whitespace? not_number_sign input_character*
    ;

not_number_sign
    : '<Any input_character except #>'
    ;
```

Wie durch die Syntax angegeben wird, müssen Bedingte Kompilierungsdirektiven geschrieben werden, als Sätze aus, in der Reihenfolge, eine `#if` Richtlinie, die NULL oder mehr `#elif` -Anweisungen, 0 (null) oder einen `#else` Richtlinie, und ein `#endif` Richtlinie. Sind Sie zwischen den Anweisungen konditionalen Abschnitte von Quellcode aus. Jeder Abschnitt wird durch die vorhergehende Richtlinie gesteuert. Ein bedingtes Codeabschnitts kann selbst geschachtelte Bedingte Kompilierungsdirektiven enthalten, sofern diese Direktiven auf komplette Sätze bilden.

Ein *Pp_conditional* wählt Sie mindestens eine der enthaltenen *Conditional_section*s für die normale lexikalische Verarbeitung:

*  Die *Pp_expression*s der `#if` und `#elif` Direktiven werden in der Reihenfolge ausgewertet, bis eine ergibt `true`. Wenn ein Ausdruck ergibt `true`, *Conditional_section* der entsprechenden Richtlinie aktiviert ist.
*  Wenn alle *Pp_expression*s "yield" `false`, und wenn ein `#else` Richtlinie vorhanden ist, die *Conditional_section* von der `#else` Richtlinie aktiviert ist.
*  Anderenfalls, Nein *Conditional_section* ausgewählt ist.

Die ausgewählte *Conditional_section*, sofern vorhanden, als ein normaler verarbeitet *Input_section*: die lexikalische Grammatik entsprechen der im Abschnitt enthaltene Quellcode; Token aus der Datenquelle generiert werden Code im Abschnitt und Präprozessordirektiven im Abschnitt über die vorgeschriebenen Auswirkungen.

Die verbleibenden *Conditional_section*s, sofern vorhanden, werden als verarbeitet *Skipped_section*s: mit Ausnahme von Präprozessordirektiven, der Quellcode im Abschnitt muss nicht einhalten der lexikalische Grammatik; Nein Token werden aus dem Quellcode im Abschnitt generiert. und Präprozessordirektiven im Abschnitt müssen lexikalisch korrekt sein, aber andernfalls nicht verarbeitet werden. Innerhalb einer *Conditional_section* , ist als verarbeitet eine *Skipped_section*, alle geschachtelten *Conditional_section*s (enthalten in geschachtelten `#if`... `#endif` und `#region`... `#endregion` erstellt) werden als auch verarbeitet *Skipped_section*s.

Das folgende Beispiel veranschaulicht die Funktionsweise des bedingten Kompilierungsdirektiven geschachtelt werden können:
```csharp
#define Debug       // Debugging on
#undef Trace        // Tracing off

class PurchaseTransaction
{
    void Commit() {
        #if Debug
            CheckConsistency();
            #if Trace
                WriteToLog(this.ToString());
            #endif
        #endif
        CommitHelper();
    }
}
```

Mit Ausnahme von Präprozessordirektiven ist die übersprungenen Quellcode nicht unterliegt einer lexikalischen Analyse. Beispielsweise ist Folgendes gültig, trotz der nicht abgeschlossener Kommentar in der `#else` Abschnitt:
```csharp
#define Debug        // Debugging on

class PurchaseTransaction
{
    void Commit() {
        #if Debug
            CheckConsistency();
        #else
            /* Do something else
        #endif
    }
}
```

Beachten Sie jedoch, Präprozessordirektiven sind erforderlich, auch in der übersprungenen Abschnitte des Quellcodes lexikalisch korrekt zu sein.

Präprozessordirektiven werden nicht verarbeitet werden, wenn sie innerhalb der Eingabeelemente mit mehreren Zeilen angezeigt werden. Um beispielsweise das Programm:
```csharp
class Hello
{
    static void Main() {
        System.Console.WriteLine(@"hello, 
#if Debug
        world
#else
        Nebraska
#endif
        ");
    }
}
```
die Ergebnisse in der Ausgabe:
```
hello,
#if Debug
        world
#else
        Nebraska
#endif
```

In speziellen Fällen der Satz von Präprozessordirektiven, die verarbeitet wird bei der Auswertung der abhängen kann die *Pp_expression*. Beispiel:
```csharp
#if X
    /*
#else
    /* */ class Q { }
#endif
```
erzeugt immer den gleiche token-Stream (`class` `Q` `{` `}`), unabhängig davon, ob `X` definiert ist. Wenn `X` wird definiert, die nur verarbeiteten Direktiven sind `#if` und `#endif`, da der mehrzeiligen Kommentar. Wenn `X` ist nicht definiert ist, klicken Sie dann die folgenden drei Anweisungen (`#if`, `#else`, `#endif`) sind Teil der Anweisung.

### <a name="diagnostic-directives"></a>Diagnose-Direktiven

Die Diagnosen-Anweisungen werden verwendet, um explizit zu generieren, Fehler- und Warnmeldungen, die auf die gleiche Weise wie andere Kompilierungsfehler und Warnungen gemeldet werden.

```antlr
pp_diagnostic
    : whitespace? '#' whitespace? 'error' pp_message
    | whitespace? '#' whitespace? 'warning' pp_message
    ;

pp_message
    : new_line
    | whitespace input_character* new_line
    ;
```

Beispiel:
```csharp
#warning Code review needed before check-in

#if Debug && Retail
    #error A build can't be both debug and retail
#endif

class Test {...}
```
immer generiert eine Warnung ("codereview erforderlich sind, vor dem Einchecken"), und erzeugt einen Fehler während der Kompilierung ("ein Builds nicht sowohl Debug-als auch im Einzelhandel") Wenn die Symbole für bedingte `Debug` und `Retail` werden beide definiert. Beachten Sie, dass eine *Pp_message* kann beliebigen Text enthalten; insbesondere sie müssen keine wohlgeformte Token, wie durch das einfache Anführungszeichen in das Wort `can't`.

### <a name="region-directives"></a>Region-Direktiven

Die Regionsdirektiven werden verwendet, um Bereiche des Quellcodes explizit zu markieren.

```antlr
pp_region
    : pp_start_region conditional_section? pp_end_region
    ;

pp_start_region
    : whitespace? '#' whitespace? 'region' pp_message
    ;

pp_end_region
    : whitespace? '#' whitespace? 'endregion' pp_message
    ;
```

Keine semantische Bedeutung haben, wird in einer Region angefügt; Regionen für die Verwendung durch den Programmierer oder durch automatisierte Tools sollen einen Abschnitt mit dem Quellcode zu markieren. Die angegebene Fehlermeldung eine `#region` oder `#endregion` Richtlinie hat keine semantische Bedeutung, ebenso; es dient lediglich zur Identifizierung der Region. Übereinstimmende `#region` und `#endregion` Anweisungen möglicherweise verschiedene *Pp_message*s.

Die lexikalische Verarbeitung einer Region:
```csharp
#region
...
#endregion
```
entspricht genau der lexikalischen Verarbeitung einer Direktive für bedingte Kompilierung des Formulars:
```csharp
#if true
...
#endif
```

### <a name="line-directives"></a>Line-Anweisungen

Line-Anweisungen können verwendet werden, um die Zeilennummern und den Quelldateinamen, die vom Compiler in der Ausgabe, z. B. Warnungen und Fehler gemeldet werden, und vom Aufrufer-Informationsattribute verwendet werden ([Aufrufer-Informationsattribute](attributes.md#caller-info-attributes)).

Line-Anweisungen werden am häufigsten in Metaprogrammierung Tools verwendet, die C#-Quellcode aus einer anderen Texteingabe generieren.

```antlr
pp_line
    : whitespace? '#' whitespace? 'line' whitespace line_indicator pp_new_line
    ;

line_indicator
    : decimal_digit+ whitespace file_name
    | decimal_digit+
    | 'default'
    | 'hidden'
    ;

file_name
    : '"' file_name_character+ '"'
    ;

file_name_character
    : '<Any input_character except ">'
    ;
```

Wenn kein `#line` Anweisungen vorhanden sind, die der Compiler gibt "true" Zeilennummern und Quelldateinamen in die Ausgabe. Bei der Verarbeitung einer `#line` Richtlinie, die enthält eine *Line_indicator* , die sich nicht `default`, behandelt der Compiler die Zeile nach der Richtlinie mit der angegebenen Zeilennummer (und den Dateinamen ein, wenn angegeben).

Ein `#line default` Anweisung kehrt den Effekt der alle vorherigen #line-Direktiven. Der Compiler gibt "true" Zeileninformationen für nachfolgende Zeilen, genau als ob keine `#line` Anweisungen hatte verarbeitet wurde.

Ein `#line hidden` Richtlinie hat keine Auswirkungen auf die Datei und Zeilennummern gemeldeten Fehler Nachrichten wirkt sich jedoch Debuggen. Beim Debuggen alle Zeilen zwischen einer `#line hidden` Richtlinie und den nachfolgenden `#line` Richtlinie (das ist nicht `#line hidden`) haben keine Informationen zur Zeilennummer. Beim Code im Debugger zu durchlaufen, werden diese Zeilen komplett übersprungen.

Beachten Sie, dass eine *File_name* eines regulären Zeichenfolgenliterals besteht darin, dass Escapesequenzen nicht verarbeitet werden; die "`\`" Zeichen bezeichnet einfach einen normalen Schrägstrich innerhalb einer *File_name*.

### <a name="pragma-directives"></a>Pragma-Anweisungen

Die `#pragma` -Präprozessordirektive verwendet, um optionale Kontextinformationen für den Compiler anzugeben. Die Informationen bereitgestellt, einem `#pragma` Richtlinie ändert sich nie auf die Semantik der Anwendung.

```antlr
pp_pragma
    : whitespace? '#' whitespace? 'pragma' whitespace pragma_body pp_new_line
    ;

pragma_body
    : pragma_warning_body
    ;
```

C# bietet `#pragma` Direktiven zum Compiler-Warnungen zu steuern. Zukünftige Versionen der Sprache ist eventuell zusätzliche `#pragma` Anweisungen. Um Interoperabilität mit anderen c#-Compiler zu gewährleisten, stellt Microsoft c#-Compiler keine Kompilierungsfehler für unbekannt aus `#pragma` Anweisungen, während solche Anweisungen führen jedoch Warnungen generiert werden.

#### <a name="pragma-warning"></a>Pragma-Warnung

Die `#pragma warning` Richtlinie deaktivieren oder Wiederherstellen des gesamten verwendet wird, oder ein bestimmter Satz von Warnung Nachrichten während der Kompilierung des nachfolgenden Programm Texts.

```antlr
pragma_warning_body
    : 'warning' whitespace warning_action
    | 'warning' whitespace warning_action whitespace warning_list
    ;

warning_action
    : 'disable'
    | 'restore'
    ;

warning_list
    : decimal_digit+ (whitespace? ',' whitespace? decimal_digit+)*
    ;
```

Ein `#pragma warning` -Direktive, die die Liste der Warnungen unterdrückt, wirkt sich auf alle Warnungen. Ein `#pragma warning` Richtlinie die enthält eine Liste der Warnungen, wirkt sich auf nur diese Warnungen, die in der Liste angegeben sind.

Ein `#pragma warning disable` Anweisung deaktiviert alle oder den angegebenen Satz von Warnungen.

Ein `#pragma warning restore` -Direktive Wiederherstellungen, die alle oder den angegebenen Satz von Warnungen, um den Status, die am Anfang der Kompilierungseinheit gültig war. Beachten Sie, dass eine bestimmte Warnung extern deaktiviert wurde, wird eine `#pragma warning restore` (angibt, ob für alle oder die betreffende Warnung), dass die Warnung nicht erneut aktiviert wird.

Das folgende Beispiel zeigt die Verwendung von `#pragma warning` gemeldet, vorübergehend deaktivieren, die Warnung Member verwiesen wird, verwenden die Warnungsnummer aus dem Microsoft C#-Compiler, wenn "ist veraltet.
```csharp
using System;

class Program
{
    [Obsolete]
    static void Foo() {}

    static void Main() {
#pragma warning disable 612
    Foo();
#pragma warning restore 612
    }
}
```
