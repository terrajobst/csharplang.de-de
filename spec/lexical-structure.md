---
ms.openlocfilehash: 4676bcd3f0a92260b4e5e20a0aa5b5ec00bf204e
ms.sourcegitcommit: 892af9016b3317a8fae12d195014dc38ba51cf16
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/01/2019
ms.locfileid: "71704072"
---
# <a name="lexical-structure"></a>Lexikalische Struktur

## <a name="programs"></a>Programs

Ein C# ***Programm*** besteht aus mindestens einer ***Quelldatei***, die formal als ***Kompilierungs Einheiten*** ([Kompilierungs Einheiten](namespaces.md#compilation-units)) bekannt ist. Eine Quelldatei ist eine geordnete Sequenz von Unicode-Zeichen. Quelldateien verfügen in der Regel über eine eins-zu-eins-Entsprechung zu Dateien in einem Dateisystem, diese Entsprechung ist jedoch nicht erforderlich. Zur maximalen Portabilität wird empfohlen, dass Dateien in einem Dateisystem mit der UTF-8-Codierung codiert werden.

Konzeptionell wird ein Programm mithilfe von drei Schritten kompiliert:

1. Transformation, die eine Datei von einem bestimmten Zeichen-und Codierungsschema in eine Unicode-Zeichenfolge konvertiert.
2. Lexikalische Analyse, die einen Stream von Unicode-Eingabezeichen in einen Datenstrom von Token übersetzt.
3. Syntaktische Analyse, bei der der Datenstrom von Token in ausführbaren Code übersetzt wird.

## <a name="grammars"></a>Grammatiken

Diese Spezifikation zeigt die Syntax der C# Programmiersprache mithilfe von zwei Grammatiken. Die ***lexikalische Grammatik*** ([lexikalische Grammatik](lexical-structure.md#lexical-grammar)) definiert, wie Unicode-Zeichen kombiniert werden, um Zeilen Abschluss Zeichen, Leerzeichen, Kommentare, Token und Vorverarbeitungs Direktiven zu bilden. Die ***syntaktische Grammatik*** ([syntaktische Grammatik](lexical-structure.md#syntactic-grammar)) definiert, wie die aus der lexikalischen Grammatik resultierenden Token in Form C# von Programmen kombiniert werden.

### <a name="grammar-notation"></a>Grammatik Notation

Die lexikalischen und syntaktischen Grammatiken werden im Backus-Naur-Formular mithilfe der Notation des antlr-Grammatik Tools dargestellt.

### <a name="lexical-grammar"></a>Lexikalische Grammatik

Die lexikalische Grammatik von C# wird in [lexikalischen Analysen](lexical-structure.md#lexical-analysis), [Token](lexical-structure.md#tokens)und [Vorverarbeitungs Direktiven](lexical-structure.md#pre-processing-directives)dargestellt. Die Terminal Symbole der lexikalischen Grammatik sind die Zeichen des Unicode-Zeichensatzes, und die lexikalische Grammatik gibt an, wie Zeichen kombiniert werden, um Token ([Token](lexical-structure.md#tokens)), Leerzeichen ([Leerraum](lexical-structure.md#white-space)), Kommentare ([Kommentare](lexical-structure.md#comments)) und Vorverarbeitungs Direktiven ([Vorverarbeitungs Direktiven](lexical-structure.md#pre-processing-directives)).

Jede Quelldatei in einem C# Programm muss der *Eingabe* Produktion der lexikalischen Grammatik ([lexikalische Analyse](lexical-structure.md#lexical-analysis)) entsprechen.

### <a name="syntactic-grammar"></a>Syntaktische Grammatik

Die syntaktische Grammatik von C# wird in den Kapiteln und Anhänge dargestellt, die diesem Kapitel folgen. Die Terminal Symbole der syntaktischen Grammatik sind die von der lexikalischen Grammatik definierten Token, und die syntaktische Grammatik gibt an, wie Token in Form C# von Programmen kombiniert werden.

Jede Quelldatei in einem C# Programm muss der *compilation_unit* -Produktion der syntaktischen Grammatik ([Kompilierungs Einheiten](namespaces.md#compilation-units)) entsprechen.

## <a name="lexical-analysis"></a>Lexikalische Analyse

Die *Eingabe* Produktion definiert die lexikalische Struktur einer C# Quelldatei. Jede Quelldatei in einem C# Programm muss mit dieser lexikalischen Grammatik-Produktion übereinstimmen.

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

Fünf grundlegende Elemente bilden die lexikalische Struktur einer C# Quelldatei: Zeilen Abschluss Zeichen ([Zeilen](lexical-structure.md#line-terminators)Abschluss Zeichen), Leerraum (leer[Zeichen](lexical-structure.md#white-space)), Kommentare ([Kommentare](lexical-structure.md#comments)), Token ([Token](lexical-structure.md#tokens)) und Vorverarbeitungs Direktiven ([Vorverarbeitungs Direktiven](lexical-structure.md#pre-processing-directives)). Von diesen grundlegenden Elementen sind nur Token in der syntaktischen Grammatik eines C# Programms ([syntaktische Grammatik](lexical-structure.md#syntactic-grammar)) von Bedeutung.

Die lexikalische Verarbeitung einer C# Quelldatei besteht darin, die Datei in eine Folge von Token zu reduzieren, die zur Eingabe für die syntaktische Analyse wird. Zeilen Abschluss Zeichen, Leerräume und Kommentare können für separate Token dienen, und Vorverarbeitungs Direktiven können dazu führen, dass Abschnitte der Quelldatei übersprungen werden. andernfalls haben diese lexikalischen Elemente keinerlei Auswirkung auf die syntaktische Struktur eines C# Programms.

Bei interpoliert-Zeichen folgen Literalen ([interpoliert Zeichen folgen Literale](lexical-structure.md#interpolated-string-literals)) wird zunächst ein einzelnes Token von der lexikalischen Analyse erstellt, aber in mehrere Eingabeelemente unterteilt, die wiederholt der lexikalischen Analyse unterzogen werden, bis alle interpoliert werden. Zeichenfolgenliterale wurden aufgelöst. Die resultierenden Token dienen dann als Eingabe für die syntaktische Analyse.

Wenn mehrere lexikalische Grammatik Produktionen einer Sequenz von Zeichen in einer Quelldatei entsprechen, bildet die lexikalische Verarbeitung immer das längstmögliche lexikalische Element. Beispielsweise wird die Zeichenfolge `//` als Anfang eines einzeiligen Kommentars verarbeitet, da dieses lexikalische Element länger als ein einzelnes `/` Token ist.

### <a name="line-terminators"></a>Zeilen Abschluss Zeichen

Zeilen Abschluss Zeichen teilen die Zeichen einer C# Quelldatei in Zeilen auf.

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

Aus Kompatibilitätsgründen mit Tools zur Quell Code Bearbeitung, die Dateiendemarker hinzufügen und eine Quelldatei als Sequenz von ordnungsgemäß beendeten Zeilen angezeigt werden können, werden die folgenden Transformationen in der entsprechenden Reihenfolge auf jede Quelldatei in einem C# Programm angewendet:

*  Wenn das letzte Zeichen der Quelldatei ein Control-Z-Zeichen (`U+001A`) ist, wird dieses Zeichen gelöscht.
*  Ein Wagen Rücklauf Zeichen (`U+000D`) wird am Ende der Quelldatei hinzugefügt, wenn diese Quelldatei nicht leer ist, und wenn das letzte Zeichen der Quelldatei kein Wagen Rücklauf Zeichen (`U+000D`)`U+000A`, ein Zeilenvorschub (), ein Zeilen Trennzeichen`U+2028`() oder ein Absatz Trennzeichen (`U+2029`).

### <a name="comments"></a>Kommentare

Zwei Arten von Kommentaren werden unterstützt: einzeilige Kommentare und durch Trennzeichen getrennte Kommentare. ***Einzeilige Kommentare*** beginnen mit den Zeichen `//` und werden bis zum Ende der Quellzeile erweitert. Durch Trennzeichen getrennte ***Kommentare*** beginnen mit `/*` den Zeichen und enden mit `*/`den Zeichen. Begrenzungs Kommentare können sich über mehrere Zeilen erstrecken.

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
    : '/*' delimited_comment_section* asterisk+ '/'
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

Kommentare werden nicht geschachtelt. Die Zeichen folgen `/*` und `*/` haben keine besondere Bedeutung innerhalb eines `//` Kommentars, und die Zeichen `//` folgen `/*` und haben keine besondere Bedeutung in einem durch Trennzeichen getrennten Kommentar.

Kommentare werden nicht innerhalb von Zeichen-und Zeichen folgen literalen verarbeitet.

Das Beispiel
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

Das Beispiel
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
zeigt mehrere einzeilige Kommentare an.

### <a name="white-space"></a>Leerraum

Leerraum ist als beliebiges Zeichen mit Unicode-Klassen-ZS (einschließlich Leerzeichen) sowie dem horizontalen Tabstopp Zeichen, dem vertikalen Tabstopp Zeichen und dem Formular Vorschub Zeichen definiert.

```antlr
whitespace
    : '<Any character with Unicode class Zs>'
    | '<Horizontal tab character (U+0009)>'
    | '<Vertical tab character (U+000B)>'
    | '<Form feed character (U+000C)>'
    ;
```

## <a name="tokens"></a>tokens

Es gibt mehrere Arten von Token: Bezeichner, Schlüsselwörter, Literale, Operatoren und Satzzeichen. Leerzeichen und Kommentare sind keine Token, obwohl Sie als Trennzeichen für Token fungieren.

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

### <a name="unicode-character-escape-sequences"></a>Unicode-Escapesequenzen

Eine Unicode-Escapesequenz stellt ein Unicode-Zeichen dar. Escapesequenzen von Unicode-Zeichen werden in bezeichern ([bezeichern](lexical-structure.md#identifiers)), Zeichen Literalen ([Zeichen literalen](lexical-structure.md#character-literals)) und regulären Zeichen folgen Literalen ([Zeichen folgen Literale](lexical-structure.md#string-literals)) verarbeitet. Eine Unicode-Escapesequenz wird an keinem anderen Speicherort verarbeitet (z. b. zum bilden eines Operators, eines interpunterators oder eines Schlüssel Worts).

```antlr
unicode_escape_sequence
    : '\\u' hex_digit hex_digit hex_digit hex_digit
    | '\\U' hex_digit hex_digit hex_digit hex_digit hex_digit hex_digit hex_digit hex_digit
    ;
```

Eine Unicode-Escapesequenz stellt das einzelne Unicode-Zeichen dar, das durch die hexadezimal`\U`Zahl nach den Zeichen "`\u`" oder "" gebildet wird. Da C# eine 16-Bit-Codierung von Unicode-Code Punkten in Zeichen und Zeichen folgen Werten verwendet, ist ein Unicode-Zeichen im Bereich u + 10000 bis U + 10FFFF in einem Zeichenliterals nicht zulässig und wird mit einem Unicode-Ersatz Zeichenpaar in einem Zeichenfolgenliteralzeichen dargestellt. Unicode-Zeichen mit Code Punkten oberhalb von 0x10FFFF werden nicht unterstützt.

Es werden nicht mehrere Übersetzungen ausgeführt. Beispielsweise entspricht`\u005Cu005C``\u005C`das Zeichen folgen Literalzeichen "" anstelle von "`\`". Der Unicode- `\u005C` Wert ist das Zeichen`\`"".

Das Beispiel
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
zeigt verschiedene Verwendungen `\u0066`von, d. h. die Escapesequenz für den Buchstaben "`f`". Das Programm entspricht
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

Die Regeln für Bezeichner, die in diesem Abschnitt angegeben sind, entsprechen genau den Regeln, die vom Unicode-Standard Anhang 31 empfohlen werden, mit dem Unterschied, dass Unterstriche als Ausgangs Zeichen zulässig sind (wie es in der C-Programmiersprache üblich ist). zulässig in bezeichlern, und das Zeichen`@`"" ist als Präfix zulässig, damit Schlüsselwörter als Bezeichner verwendet werden können.

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

Informationen zu den oben erwähnten Unicode-Zeichenklassen finden Sie im Unicode-Standard, Version 3,0, Abschnitt 4,5.

Beispiele für gültige Bezeichner sind "`identifier1`", "`_identifier2`" und "`@if`".

Ein Bezeichner in einem übereinstimmenden Programm muss das kanonische Format aufweisen, das durch die Unicode-normalisierungs Form C definiert ist, wie im Unicode-Standard Anhang 15 definiert Das Verhalten beim Auffinden eines Bezeichners, der nicht in der normalisierungs Form C vorliegt, ist Implementierungs definiert. eine Diagnose ist jedoch nicht erforderlich.

Das-Präfix "`@`" ermöglicht die Verwendung von Schlüsselwörtern als Bezeichner, was bei der Schnittstellen mit anderen Programmiersprachen nützlich ist. Das Zeichen `@` ist nicht Teil des Bezeichners, daher kann der Bezeichner in anderen Sprachen als normaler Bezeichner ohne das Präfix angezeigt werden. Ein Bezeichner mit `@` einem Präfix wird als ***ausführlicher Bezeichner***bezeichnet. Die `@` Verwendung des Präfix für Bezeichner, bei denen es sich nicht um Schlüsselwörter handelt, ist zulässig, aber es wird dringend davon abgeraten.

Das Beispiel:
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
definiert eine Klasse`class`mit dem Namen "" mit einer statischen Methode`static`mit dem Namen "", die`bool`einen Parameter mit dem Namen "" annimmt. Beachten Sie, dass das Token "`cl\u0061ss`" ein Bezeichner ist, da Unicode-Escapezeichen in Schlüsselwörtern nicht zulässig sind und denselben Bezeichner wie "`@class`" haben.

Zwei Bezeichner werden als identisch angesehen, wenn Sie nach dem Anwenden der folgenden Transformationen identisch sind:

*  Wenn Sie verwendet`@`wird, wird das Präfix "" entfernt.
*  Jede *unicode_escape_sequence* wird in das entsprechende Unicode-Zeichen transformiert.
*  Alle *formatting_character*s werden entfernt.

Bezeichner, die zwei aufeinander folgende unter`U+005F`Striche () enthalten, sind für die Verwendung durch die-Implementierung reserviert. Beispielsweise kann eine-Implementierung erweiterte Schlüsselwörter bereitstellen, die mit zwei unterstrichen beginnen.

### <a name="keywords"></a>Stichwörter

Ein ***Schlüsselwort*** ist eine bezeichnerartige Zeichenfolge, die reserviert ist und nicht als Bezeichner verwendet werden kann, es sei denn, `@` das Zeichen wird vorangestellt.

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

An manchen Stellen in der Grammatik haben bestimmte Bezeichner eine besondere Bedeutung, aber keine Schlüsselwörter. Solche Bezeichner werden manchmal als "kontextabhängige Schlüsselwörter" bezeichnet. In einer Eigenschafts Deklaration haben die Bezeichner`get`"" und`set`"" z. b. eine besondere Bedeutung ([Accessoren](classes.md#accessors)). Ein anderer Bezeichner `get` als `set` oder ist in diesen Speicherorten nie zulässig, sodass diese Verwendung nicht mit der Verwendung dieser Wörter als Bezeichner in Konflikt steht. In anderen Fällen, z. b. mit dem`var`Bezeichner "" in implizit typisierten lokalen Variablen Deklarationen ([lokale Variablen Deklarationen](statements.md#local-variable-declarations)), kann ein Kontext Schlüsselwort mit deklarierten Namen in Konflikt stehen. In solchen Fällen hat der deklarierte Name Vorrang vor der Verwendung des Bezeichners als kontextbezogenes Schlüsselwort.

### <a name="literals"></a>Literale

Ein ***Literalwert*** ist eine Quell Code Darstellung eines Werts.

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

Es gibt zwei boolesche Literalwerte `true` : `false`und.

```antlr
boolean_literal
    : 'true'
    | 'false'
    ;
```

Der Typ eines *boolean_literal* ist `bool`.

#### <a name="integer-literals"></a>Ganzzahlenliteral

Ganzzahlige Literale werden verwendet, um Werte `int`der `uint`Typen `long`,, `ulong`und zu schreiben. Ganzzahlige Literale haben zwei mögliche Formen: decimal und hexadezimal.

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

Der Typ eines ganzzahligen Literals wird wie folgt bestimmt:

*  Wenn das Literale kein Suffix aufweist, verfügt es über den ersten dieser Typen, in dem sein Wert dargestellt werden `int`kann `uint`: `long`, `ulong`,,.
*  `U` Wenn das Literale von oder `u`als Suffix versehen wird, verfügt es über den ersten dieser Typen, in dem sein Wert dargestellt werden `ulong`kann: `uint`,.
*  `L` Wenn das Literale von oder `l`als Suffix versehen wird, verfügt es über den ersten dieser Typen, in dem sein Wert dargestellt werden `ulong`kann: `long`,.
*  Wenn das Literale `UL`von, `LU` `ulong`,,,, `Lu` ,`lU`oder suffixtwird,istesvomTyp`lu`. `ul` `uL` `Ul`

Wenn der durch ein `ulong` Ganzzahlliteral dargestellte Wert außerhalb des Bereichs des Typs liegt, tritt ein Kompilierzeitfehler auf.

Es wird empfohlen, beim Schreiben von literalen des Typs`L` `long`"" anstelle von "`l`" zu verwenden, da es einfach ist, den Buchstaben "`l`" mit der Ziffer "`1`" zu verwechseln.

Um zuzulassen, dass die `int` kleinsten `long` möglichen Werte und als Dezimale ganzzahlige Literale geschrieben werden, sind die folgenden zwei Regeln vorhanden:

* Wenn ein *decimal_integer_literal* mit dem Wert 2147483648 (2 ^ 31) und No *integer_type_suffix* als Token unmittelbar nach einem unären Minus Operator Token ([unärer Minus Operator](expressions.md#unary-minus-operator)) angezeigt wird, ist das Ergebnis eine Konstante vom Typ `int` mit dem Wert-2147483648 (-2 ^ 31). In allen anderen Fällen ist ein solcher *decimal_integer_literal* vom Typ `uint`.
* Wenn ein *decimal_integer_literal* mit dem Wert 9.223.372.036.854.775.808 (2 ^ 63) und No *integer_type_suffix* oder *integer_type_suffix* `L` oder `l` als Token unmittelbar nach einem unären Minus Operator Token angezeigt wird ([ Unärer Minus-Operator](expressions.md#unary-minus-operator)), das Ergebnis ist eine Konstante vom Typ `long` mit dem Wert-9.223.372.036.854.775.808 (-2 ^ 63). In allen anderen Fällen ist ein solcher *decimal_integer_literal* vom Typ `ulong`.

#### <a name="real-literals"></a>Echte Literale

Echte Literale werden verwendet, um Werte der Typen `float`, `double`und `decimal`zu schreiben.

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

Wenn *real_type_suffix* nicht angegeben wird, ist der Typ des realen Literals `double`. Andernfalls bestimmt das echte Typsuffix den Typ des echten Literals wie folgt:

*  Ein echtes LiteralSuffix, `F` das `f` von oder ist `float`, ist vom Typ. Die `1f` `1.5f`Literale `float`,, und `123.456F` sind z. b. vom Typ. `1e10f`
*  Ein echtes LiteralSuffix, `D` das `d` von oder ist `double`, ist vom Typ. Die `1d` `1.5d`Literale `double`,, und `123.456D` sind z. b. vom Typ. `1e10d`
*  Ein echtes LiteralSuffix, `M` das `m` von oder ist `decimal`, ist vom Typ. Die `1m` `1.5m`Literale `decimal`,, und `123.456M` sind z. b. vom Typ. `1e10m` Diese Literale werden in einen `decimal` -Wert konvertiert, indem der genaue Wert verwendet wird, und, falls erforderlich, auf den nächstgelegenen darstellbaren Wert mit der-Rundung ([dem Decimal-Typ](types.md#the-decimal-type)) gerundet. Alle in der Literale sichtbaren Skalierungen bleiben erhalten, es sei denn, der Wert ist gerundet, oder der Wert ist NULL (in letzterem Fall ist das Vorzeichen und die Dezimalstellen 0). Daher wird das Literale `2.900m` analysiert, um das Dezimaltrennzeichen `0`, den Koeffizienten `2900`und die Skala `3`zu bilden.

Wenn das angegebene Literale nicht im angegebenen Typ dargestellt werden kann, tritt ein Kompilierzeitfehler auf.

Der Wert eines echten Literals vom Typ `float` oder `double` wird mit dem IEEE-Modus "Round to Next" bestimmt.

Beachten Sie, dass in einem echten Literalzeichen nach dem Dezimaltrennzeichen immer Dezimalstellen erforderlich sind. Beispielsweise ist `1.3F` ein echtes Literalzeichen `1.F` , aber nicht.

#### <a name="character-literals"></a>Zeichenliterale

Ein Zeichenliteral stellt ein einzelnes Zeichen dar und besteht normalerweise aus einem Zeichen in Anführungs `'a'`Zeichen, wie in.

Hinweis: Die Grammatik-Notation von antlr macht folgendes verwirrend! Wenn Sie in antlr schreiben `\'` , steht es für ein einzelnes Anführungs `'`Zeichen. Und wenn Sie schreiben `\\` , steht ein einzelner umgekehrter schräg `\`Strich. Daher bedeutet die erste Regel für ein Zeichenliteral, dass Sie mit einem einfachen Anführungszeichen, einem Zeichen und einem einfachen Anführungszeichen beginnt. Und die elf möglichen einfachen Escapesequenzen `\"`sind `\\` `\'`, `\0`, `\a`, `\b`, `\f`, `\n`, `\r`, `\t`,, ,`\v`.

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

Ein Zeichen, das einem umgekehrten Schrägstrich (`\`) in einem *Zeichen* folgt, muss eines der folgenden Zeichen sein `'`: `"`, `\`, `0`, `a`, `b`, `f` , , `n`, `r`, `t`, `u`, `U`, `x`, `v`. Andernfalls tritt ein Kompilierungsfehler auf.

Eine hexadezimale Escapesequenz stellt ein einzelnes Unicode-Zeichen dar, wobei der Wert durch die hexadezimale Zahl nach "`\x`" gebildet wird.

Wenn der Wert, der von einem Zeichenliterals `U+FFFF`dargestellt wird, größer als ist, tritt ein Kompilierzeitfehler auf.

Eine Unicode-Escapesequenz (Escapesequenzen für[Unicode-Zeichen](lexical-structure.md#unicode-character-escape-sequences)) in einem Zeichenliterals muss im Bereich `U+0000` von `U+FFFF`liegen.

Eine einfache Escapesequenz stellt eine Unicode-Zeichencodierung dar, wie in der folgenden Tabelle beschrieben.


| __Escapesequenz__ | __Zeichen Name__ | __Unicode-Codierung__ |
|---------------------|--------------------|----------------------|
| `\'`                | Einfaches Anführungszeichen       | `0x0027`             | 
| `\"`                | Doppeltes Anführungszeichen       | `0x0022`             | 
| `\\`                | Umgekehrter Schrägstrich          | `0x005C`             | 
| `\0`                | NULL               | `0x0000`             | 
| `\a`                | Warnung              | `0x0007`             | 
| `\b`                | Rückschritt          | `0x0008`             | 
| `\f`                | Seitenvorschub          | `0x000C`             | 
| `\n`                | Zeilenwechsel           | `0x000A`             | 
| `\r`                | Wagenrücklauf    | `0x000D`             | 
| `\t`                | Horizontaler Tabulator     | `0x0009`             | 
| `\v`                | Vertikaler Tabulator       | `0x000B`             | 

Der Typ eines *character_literal* ist `char`.

#### <a name="string-literals"></a>Zeichenfolgenliterale

C#unterstützt zwei Formen von Zeichenfolgenliteralen: ***reguläre Zeichen folgen Literale*** und ***ausführliche Zeichen folgen Literale***.

Ein reguläres Zeichenfolgenliterale besteht aus null oder mehr Zeichen, die `"hello"`in doppelten Anführungszeichen eingeschlossen sind, wie z. b `\t` ., und kann sowohl einfache Escapesequenzen (z. b. für das Tabstopp Zeichen) als auch hexadezimale und

Ein ausführlichen Zeichenfolgenliterals besteht aus einem `@` Zeichen, gefolgt von einem doppelten Anführungszeichen, NULL oder mehr Zeichen und einem schließenden doppelten Anführungszeichen. Ein einfaches Beispiel ist `@"hello"`. In einem ausführlichen zeichenfolgenliteralliteralen werden die Zeichen zwischen den Trennzeichen wörtlich interpretiert, die einzige Ausnahme ist eine *quote_escape_sequence*. Insbesondere einfache Escapesequenzen und hexadezimale und Unicode-Escapesequenzen werden in ausführlichen Zeichenfolgenliteralen nicht verarbeitet. Ein ausführlicher zeichenfolgenliteralvorgang kann mehrere Zeilen umfassen.

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

Ein Zeichen, das auf einen umgekehrten Schrägstrich (`\`) in einem *regular_string_literal_character* folgt, muss eines der folgenden Zeichen sein: `'`, `"`, `\`, `0`, `a`, `b`, `f`, `n`, 0, 1, @no__ t-12, 3, 4, 5. Andernfalls tritt ein Kompilierungsfehler auf.

Das Beispiel
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
zeigt eine Vielzahl von Zeichenfolgenliteralen an. Das letzte Zeichenfolgenliterale, `j`, ist ein ausführlicher Zeichenfolgenliteral, das mehrere Zeilen Die Zeichen zwischen den Anführungszeichen, einschließlich Leerzeichen, z. b. neue Zeilenzeichen, werden wörtlich beibehalten.

Da eine hexadezimale Escapesequenz eine Variable Anzahl von hexadezimalen Ziffern `"\x123"` aufweisen kann, enthält das Zeichenfolgenliterale ein einzelnes Zeichen 123 mit dem hexadezi Um eine Zeichenfolge zu erstellen, die das Zeichen mit dem Hexadezimalwert 12 gefolgt vom `"\x00123"` Zeichen `"\x12" + "3"` 3 enthält, könnte ein oder stattdessen geschrieben werden.

Der Typ eines *string_literal* ist `string`.

Jedes Zeichenfolgenliterale führt nicht unbedingt zu einer neuen Zeichen folgen Instanz. Wenn zwei oder mehr Zeichen folgen Literale, die gemäß dem Zeichen folgen Gleichheits Operator ([Zeichen folgen Gleichheits Operatoren](expressions.md#string-equality-operators)) gleichwertig sind, im selben Programm vorkommen, verweisen diese Zeichen folgen Literale auf dieselbe Zeichen folgen Instanz. Beispielsweise die Ausgabe, die von
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
liegt `True` daran, dass die beiden Literale auf dieselbe Zeichen folgen Instanz verweisen.

#### <a name="interpolated-string-literals"></a>Interpoliert Zeichen folgen Literale

Interinterpolierte Zeichen folgen Literale ähneln Zeichen folgen Literalen, enthalten jedoch Lücken, die `{` durch `}`und getrennt sind, wobei Ausdrücke auftreten können. Zur Laufzeit werden die Ausdrücke so ausgewertet, dass Ihre Text Formulare in der Zeichenfolge an der Stelle, an der die Lücke auftritt, ersetzt werden. Die Syntax und die Semantik der Zeichen folgen Interpolationen werden im Abschnitt ([interpoliert](expressions.md#interpolated-strings)Zeichen folgen) beschrieben.

Wie bei Zeichenfolgenliteralen können interpoliert Zeichen folgen Literale entweder regulär oder wörtlich sein. Interpoliert reguläre Zeichenfolgenliterale werden `$"` durch `"`und getrennt, und interpoliert, ausführliche Zeichen folgen Literale `$@"` werden `"`durch und getrennt.

Wie bei anderen literalen ergibt die lexikalische Analyse eines interpoliert für interpoliert zunächst ein einzelnes Token, wie gemäß der Grammatik unten. Vor der syntaktischen Analyse wird jedoch das einzelne Token eines interpoliert-Zeichenfolgenliterals in mehrere Token für die Teile der Zeichenfolge aufgeteilt, die die Löcher einschließen, und die in den Löchern auftretenden Eingabeelemente werden lexikalisch erneut analysiert. Dies kann wiederum dazu führen, dass mehr interinterpolierte Zeichenfolgenliterale verarbeitet werden, die verarbeitet werden können, aber wenn lexikalisch korrekt ist, führt letztendlich zu einer Sequenz von Tokens, damit die syntaktische Analyse verarbeitet wird.

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

Ein *interpolated_string_literal* -Token wird wie folgt als mehrere Token und andere Eingabeelemente in der Reihenfolge des Auftretens in *interpolated_string_literal*neu interpretiert:

* Vorkommen der folgenden werden als separate einzelne Token neu interpretiert: das führende `$`-Vorzeichen, *interpolated_regular_string_whole*, *interpolated_regular_string_start*, *interpolated_regular_string_mid*,  *interpolated_regular_string_end*, *interpolated_verbatim_string_whole*, *interpolated_verbatim_string_start*, *interpolated_verbatim_string_mid* und *interpolated_verbatim_string_end*.
* Vorkommen von *regular_balanced_text* und *verbatim_balanced_text* dazwischen werden als *input_section* ([lexikalische Analyse](lexical-structure.md#lexical-analysis)) neu verarbeitet und als resultierende Sequenz von Eingabe Elementen interpretiert. Diese können wiederum interinterpolierte zeichenfolgenliteraltoken enthalten, die neu interpretiert werden.

Die syntaktische Analyse kombiniert die Token in einer *interpolated_string_expression* ([interpoliert](expressions.md#interpolated-strings)) neu.

Beispiele TODO


#### <a name="the-null-literal"></a>Das NULL-Literale

```antlr
null_literal
    : 'null'
    ;
```

*Null_literal* kann implizit in einen Verweistyp oder einen Typ konvertiert werden, der NULL-Werte zulässt.

### <a name="operators-and-punctuators"></a>Operatoren und Satzzeichen

Es gibt mehrere Arten von Operatoren und Satzzeichen. Operatoren werden in Ausdrücken verwendet, um Vorgänge mit einem oder mehreren Operanden zu beschreiben. Der-Ausdruck `a + b` verwendet beispielsweise den `+` -Operator, um die beiden Operanden `b` `a` und hinzuzufügen. Satzzeichen sind zum Gruppieren und trennen vorgesehen.

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

Der senkrechte Strich in den *right_shift* -und *right_shift_assignment* -Produktionen wird verwendet, um anzugeben, dass im Gegensatz zu anderen Produktionen in der syntaktischen Grammatik keine Zeichen jeglicher Art (nicht sogar Leerzeichen) zwischen den Token zulässig sind. Diese Produktionen werden speziell behandelt, um die korrekte Handhabung von *type_parameter_list*s ([Typparameter](classes.md#type-parameters)) zu ermöglichen.

## <a name="pre-processing-directives"></a>Vorverarbeitungs Direktiven

Die Vorverarbeitungs Direktiven bieten die Möglichkeit, Abschnitte von Quelldateien bedingt zu überspringen, Fehler-und Warnungs Bedingungen zu melden und verschiedene Bereiche des Quellcodes zu gliedern. Der Begriff "Vorverarbeitungs Direktiven" wird nur aus Gründen der Konsistenz mit C- C++ und Programmiersprachen verwendet. In C#gibt es keinen separaten Schritt vor der Verarbeitung. Vorverarbeitungs Direktiven werden im Rahmen der lexikalischen Analysephase verarbeitet.

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

Die folgenden Vorverarbeitungs Direktiven sind verfügbar:

*  `#define`und `#undef`, die zum definieren bzw. Aufheben der Definition von Symbolen für bedingte Kompilierung ([Deklarations Anweisungen](lexical-structure.md#declaration-directives)) verwendet werden.
*  `#if`, `#elif`, `#else` und`#endif`, die verwendet werden, um Abschnitte des Quellcodes bedingt zu überspringen ([bedingte Kompilierungs Direktiven](lexical-structure.md#conditional-compilation-directives)).
*  `#line`, der verwendet wird, um Zeilennummern zu steuern, die für Fehler und Warnungen ausgegeben werden ([Zeilen Anweisungen](lexical-structure.md#line-directives)).
*  `#error`und `#warning`, die verwendet werden, um Fehler und Warnungen bzw. ([Diagnose Direktiven](lexical-structure.md#diagnostic-directives)) auszugeben.
*  `#region`und `#endregion`, die verwendet werden, um Abschnitte des Quellcodes ([Regions Direktiven](lexical-structure.md#region-directives)) explizit zu markieren.
*  `#pragma`, der verwendet wird, um optionale Kontextinformationen für den Compiler anzugeben ([pragma-Direktiven](lexical-structure.md#pragma-directives)).

Eine Vorverarbeitungs Direktive belegt immer eine separate Zeile des Quellcodes und beginnt immer mit einem `#` Zeichen und einem Vorverarbeitungs-Direktivennamen. Leerräume können vor dem `#` Zeichen und zwischen dem `#` Zeichen und dem Direktivennamen auftreten.

Eine Quellzeile `#define` `#elif` `#line` `#endif` `#undef` `#else` ,die`#endregion` eine-,-,-,-,-,-oder-Direktive enthält, kannmiteinemeinzeiligenKommentarenden.`#if` Durch Trennzeichen getrennte Kommentare `/* */` (der Stil von Kommentaren) sind in Quellzeilen mit Vorverarbeitungs Direktiven nicht zulässig.

Vorverarbeitungs Direktiven sind keine Token und sind nicht Teil der syntaktischen Grammatik von C#. Allerdings können Vorverarbeitungs Direktiven verwendet werden, um Sequenzen von Token einzuschließen oder auszuschließen, und sich auf diese Weise auf die C# Bedeutung eines Programms auswirken. Bei der Kompilierung wird das Programm z. b.:
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
ergibt genau dieselbe Sequenz von Token wie das Programm:
```csharp
class C
{
    void F() {}
    void I() {}
}
```

Im Gegensatz dazu sind die beiden Programme ganz anders, syntaktisch, Sie sind jedoch identisch.

### <a name="conditional-compilation-symbols"></a>Symbole für bedingte Kompilierung

Die Funktionen für die bedingte Kompilierung `#if`, die `#else`von den `#endif` Direktiven,, und bereitgestellt werden, `#elif`werden durch Vorverarbeitungs Ausdrücke ([Vorverarbeitungs Ausdrücke](lexical-structure.md#pre-processing-expressions)) und bedingt gesteuert. Kompilierungs Symbole.

```antlr
conditional_symbol
    : '<Any identifier_or_keyword except true or false>'
    ;
```

Ein Symbol für die bedingte Kompilierung hat zwei mögliche Zustände: ***definiert*** oder nicht ***definiert***. Am Anfang der lexikalischen Verarbeitung einer Quelldatei ist ein Symbol für die bedingte Kompilierung nicht definiert, es sei denn, es wurde explizit durch einen externen Mechanismus definiert (z. b. eine Befehlszeilen-Compileroption). Wenn eine `#define` Direktive verarbeitet wird, wird das in dieser Direktive benannte bedingte Kompilierungs Symbol in dieser Quelldatei definiert. Das Symbol bleibt so lange definiert `#undef` , bis eine-Direktive für das gleiche Symbol verarbeitet wird oder bis das Ende der Quelldatei erreicht wird. Dies bedeutet, dass `#define` die-und- `#undef` Direktiven in einer Quelldatei keine Auswirkung auf andere Quelldateien im gleichen Programm haben.

Wenn in einem Vorverarbeitungs Ausdruck darauf verwiesen wird, hat ein definiertes bedingtes Kompilierungs Symbol den `true`booleschen Wert, und ein nicht definiertes bedingtes Kompilierungs Symbol weist den booleschen Wert `false`auf. Es ist nicht erforderlich, dass bedingte Kompilierungs Symbole explizit deklariert werden, bevor in Vorverarbeitungs Ausdrücken auf Sie verwiesen wird. Stattdessen sind nicht deklarierte Symbole einfach undefiniert und verfügen daher `false`über den Wert.

Der Namespace für Symbole für die bedingte Kompilierung ist eindeutig und getrennt von allen anderen benannten C# Entitäten in einem Programm. Auf Symbole für die bedingte Kompilierung kann `#define` nur `#undef` in-und-Direktiven und in Vorverarbeitungs Ausdrücken verwiesen werden.

### <a name="pre-processing-expressions"></a>Vorverarbeiten von Ausdrücken

Vorverarbeitungs Ausdrücke können in `#if` -und `#elif` -Direktiven auftreten. `!`Die Operatoren `==`, `!=`, `&&` und sindinVorverarbeitungsAusdrückenzulässig,undfürdieGruppierungkönnenKlammernverwendetwerden.`||`

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

Wenn in einem Vorverarbeitungs Ausdruck darauf verwiesen wird, hat ein definiertes bedingtes Kompilierungs Symbol den `true`booleschen Wert, und ein nicht definiertes bedingtes Kompilierungs Symbol weist den booleschen Wert `false`auf.

Bei der Auswertung eines Vorverarbeitungs Ausdrucks ergibt sich immer ein boolescher Wert. Die Regeln für die Auswertung eines Vorverarbeitungs Ausdrucks sind identisch mit denen für einen konstanten Ausdruck ([Konstante Ausdrücke](expressions.md#constant-expressions)), mit dem Unterschied, dass nur benutzerdefinierte Entitäten, auf die verwiesen werden kann, Symbole für die bedingte Kompilierung sind.

### <a name="declaration-directives"></a>Deklarations Direktiven

Die Deklarations Anweisungen werden verwendet, um Symbole für die bedingte Kompilierung zu definieren oder zu deaktivieren.

```antlr
pp_declaration
    : whitespace? '#' whitespace? 'define' whitespace conditional_symbol pp_new_line
    | whitespace? '#' whitespace? 'undef' whitespace conditional_symbol pp_new_line
    ;

pp_new_line
    : whitespace? single_line_comment? new_line
    ;
```

Die Verarbeitung einer `#define` -Direktive bewirkt, dass das angegebene bedingte Kompilierungs Symbol definiert wird, beginnend mit der Quellzeile, die auf die-Direktive folgt. Entsprechend bewirkt die Verarbeitung einer- `#undef` Direktive, dass das angegebene bedingte Kompilierungs Symbol undefiniert wird, beginnend mit der Quellzeile, die auf die-Direktive folgt.

Alle `#define` - `#undef` und-Direktiven in einer Quelldatei müssen vor dem ersten *Token* ([Tokens](lexical-structure.md#tokens)) in der Quelldatei auftreten; andernfalls tritt ein Kompilierzeitfehler auf. In intuitiver Hinsicht `#define` müssen `#undef` -und-Direktiven allen "echten Code" in der Quelldatei vorangestellt sein.

Das Beispiel:
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
ist gültig, da `#define` die-Anweisungen dem ersten Token (dem `namespace` -Schlüsselwort) in der Quelldatei vorangestellt sind.

Das folgende Beispiel führt zu einem Kompilierzeitfehler, da `#define` ein echter Code befolgt:
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

Ein `#define` kann ein Symbol für die bedingte Kompilierung definieren, das bereits definiert ist, ohne `#undef` dass ein Eingreifen für dieses Symbol vorliegt. Im folgenden Beispiel wird ein Symbol `A` für die bedingte Kompilierung definiert und dann wieder definiert.
```csharp
#define A
#define A
```

Ein `#undef` kann ein bedingtes Kompilierungs Symbol, das nicht definiert ist, nicht definieren. Im folgenden Beispiel wird ein Symbol `A` für die bedingte Kompilierung definiert und dann zweimal wieder definiert. das zweite `#undef` hat jedoch keine Auswirkung, aber es ist noch gültig.
```csharp
#define A
#undef A
#undef A
```

### <a name="conditional-compilation-directives"></a>Bedingte Kompilierungs Direktiven

Die Direktiven für die bedingte Kompilierung werden verwendet, um Teile einer Quelldatei bedingt einzuschließen oder auszuschließen.

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

Wie durch die-Syntax angegeben, müssen bedingte Kompilierungs Direktiven als Sätze geschrieben werden, die aus, `#if` in Reihenfolge, einer `#elif` -Direktive, NULL `#else` oder mehr-Direktiven, keiner oder einer-Direktive und einer `#endif` -Direktive bestehen Zwischen den Direktiven bestehen bedingte Abschnitte des Quellcodes. Jeder Abschnitt wird von der unmittelbar vorangehenden-Direktive gesteuert. Ein bedingter Abschnitt kann selbst ggf. ggf. ggf

Ein *pp_conditional* wählt höchstens eine der enthaltenen *conditional_section*s für die normale lexikalische Verarbeitung aus:

*  Die *pp_expression*s der Anweisungen `#if` und `#elif` werden in der Reihenfolge ausgewertet, bis eine `true` ergibt. Wenn ein Ausdruck `true` ergibt, wird der *conditional_section* der entsprechenden Direktive ausgewählt.
*  Wenn alle *pp_expression*s `false` ergeben und eine `#else`-Direktive vorhanden ist, wird der *conditional_section* -Wert der `#else`-Direktive ausgewählt.
*  Andernfalls wird kein *conditional_section* ausgewählt.

Das ausgewählte *conditional_section*(sofern vorhanden) wird als normales *input_section*verarbeitet: der im Abschnitt enthaltene Quellcode muss der lexikalischen Grammatik entsprechen. Token werden aus dem Quellcode im-Abschnitt generiert. und Vorverarbeitungs Direktiven im-Abschnitt haben die vorgeschriebenen Auswirkungen.

Die verbleibenden *conditional_section*s werden, sofern vorhanden, als *skipped_section*s verarbeitet: mit Ausnahme von Vorverarbeitungs Direktiven muss der Quellcode im-Abschnitt nicht an die lexikalische Grammatik heran bestehen. aus dem Quellcode im-Abschnitt werden keine Token generiert. und Vorverarbeitungs Direktiven im Abschnitt müssen lexikalisch korrigiert werden, werden jedoch nicht anderweitig verarbeitet. Innerhalb einer *conditional_section* , die als *skipped_section*verarbeitet wird, werden alle geschachtelten *conditional_section*s (die in geschachtelten `#if`... `#endif`-und `#region`... `#endregion`-Konstrukten enthalten sind) ebenfalls als skipped_ verarbeitet.  *Abschnitt*s.

Im folgenden Beispiel wird veranschaulicht, wie bedingte Kompilierungs Direktiven schachteln können:
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

Mit Ausnahme von Vorverarbeitungs Direktiven unterliegt der übersprungene Quellcode nicht der lexikalischen Analyse. Beispielsweise ist Folgendes gültig, obwohl der nicht abgeschlossener Kommentar im `#else` Abschnitt lautet:
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

Beachten Sie jedoch, dass Vorverarbeitungs Direktiven auch in übersprungenen Abschnitten des Quellcodes lexikalisch korrigiert werden müssen.

Vorverarbeitungs Direktiven werden nicht verarbeitet, wenn Sie in mehrzeiligen Eingabe Elementen angezeigt werden. Das Programm lautet z. b.:
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
Ergebnisse in der Ausgabe:
```console
hello,
#if Debug
        world
#else
        Nebraska
#endif
```

In besonderen Fällen hängt der Satz der verarbeiteten Vorverarbeitungs Direktiven möglicherweise von der Auswertung des *pp_expression*ab. Das Beispiel:
```csharp
#if X
    /*
#else
    /* */ class Q { }
#endif
```
erzeugt immer denselben Tokenstream (`class` `}` `Q` `{` ), unabhängig davon, ob definiert `X` ist oder nicht. Wenn `X` definiert ist, sind `#if` die einzigen verarbeiteten Direktiven `#endif`und, aufgrund des mehrzeiligen Kommentars. Wenn `X` nicht definiert ist, sind drei-`#if`Direktiven `#endif`(, `#else`,) Teil der direktivenmenge.

### <a name="diagnostic-directives"></a>Diagnose Direktiven

Die Diagnose Direktiven werden verwendet, um Fehler-und Warnmeldungen explizit zu generieren, die auf die gleiche Weise wie andere Kompilierzeitfehler und-Warnungen gemeldet werden.

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

Das Beispiel:
```csharp
#warning Code review needed before check-in

#if Debug && Retail
    #error A build can't be both debug and retail
#endif

class Test {...}
```
erzeugt immer eine Warnung ("Code Review ist vor dem Einchecken erforderlich") und erzeugt einen Kompilierzeitfehler ("ein Build kann nicht gleichzeitig Debuggen und Einzelhandel sein") `Debug` , `Retail` wenn die bedingten Symbole und definiert sind. Beachten Sie, dass ein *pp_message* beliebigen Text enthalten kann. insbesondere muss Sie keine wohlgeformten Token enthalten, wie durch das einfache Anführungszeichen im Wort `can't` gezeigt.

### <a name="region-directives"></a>Regions Direktiven

Die Regions Direktiven werden verwendet, um Bereiche des Quellcodes explizit zu markieren.

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

An einen Bereich ist keine semantische Bedeutung angefügt. Regionen sind für die Verwendung durch den Programmierer oder automatisierte Tools vorgesehen, um einen Abschnitt des Quellcodes zu markieren. Die in einer `#region` -oder `#endregion` -Direktive angegebene Meldung hat ebenfalls keine semantische Bedeutung; Sie dient lediglich zur Identifizierung der Region. Übereinstimmende `#region`-und `#endregion`-Direktiven haben möglicherweise unterschiedliche *pp_message*s.

Die lexikalische Verarbeitung einer Region:
```csharp
#region
...
#endregion
```
entspricht genau der lexikalischen Verarbeitung einer Direktive für die bedingte Kompilierung in der Form:
```csharp
#if true
...
#endif
```

### <a name="line-directives"></a>Line-Direktiven

Zeilen Anweisungen können verwendet werden, um die Zeilennummern und Quell Dateinamen zu ändern, die vom Compiler in Ausgabe wie Warnungen und Fehlern gemeldet werden, und die von Aufrufer-Informations Attributen ([aufruferinformationsattribute](attributes.md#caller-info-attributes)) verwendet werden.

Zeilen Direktiven werden am häufigsten in metaprogrammierungs Tools verwendet, C# die Quellcode aus einer anderen Texteingabe generieren.

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

Wenn keine `#line` -Direktiven vorhanden sind, meldet der Compiler echte Zeilennummern und Quell Dateinamen in der Ausgabe. Bei der Verarbeitung einer `#line`-Direktive, die eine *line_indicator* enthält, die nicht `default` ist, behandelt der Compiler die Zeile nach der Direktive mit der angegebenen Zeilennummer (und dem Dateinamen, falls angegeben).

Eine `#line default` -Direktive kehrt die Auswirkungen aller vorangehenden #line Direktiven um. Der Compiler meldet echte Zeilen Informationen für nachfolgende Zeilen, genau so, `#line` als ob keine Direktiven verarbeitet wurden.

Eine `#line hidden` -Direktive hat keine Auswirkung auf die Datei-und Zeilennummern, die in Fehlermeldungen gemeldet werden, wirkt sich aber auf das Debugging auf Quell Ebene aus Beim Debuggen verfügen alle Zeilen `#line hidden` zwischen einer-Direktive und der nachfolgenden `#line hidden` `#line` Direktive (das nicht) über keine Zeilennummern Informationen. Wenn Sie Code im Debugger schrittweise durchlaufen, werden diese Zeilen vollständig übersprungen.

Beachten Sie, dass ein *file_name* -Wert von einem regulären Zeichenfolgenliteralwert abweicht, da Escapezeichen das Zeichen "`\`" bezeichnet einfach einen normalen umgekehrten Schrägstrich innerhalb eines *file_name*.

### <a name="pragma-directives"></a>Pragma-Direktiven

Die `#pragma` Vorverarbeitungs Direktive wird verwendet, um dem Compiler optionale Kontextinformationen anzugeben. Die in einer `#pragma` -Direktive angegebenen Informationen ändern die Programm Semantik nie.

```antlr
pp_pragma
    : whitespace? '#' whitespace? 'pragma' whitespace pragma_body pp_new_line
    ;

pragma_body
    : pragma_warning_body
    ;
```

C#stellt `#pragma` Anweisungen zum Steuern von Compilerwarnungen bereit. Zukünftige Versionen der Sprache können zusätzliche `#pragma` Anweisungen enthalten. Um die Interoperabilität mit anderen C# Compilern sicherzustellen, C# gibt der Microsoft-Compiler keine Kompilierungs `#pragma` Fehler für unbekannte Direktiven aus. solche Direktiven generieren jedoch Warnungen.

#### <a name="pragma-warning"></a>pragma-Warnung

Die `#pragma warning` -Anweisung wird verwendet, um alle oder einen bestimmten Satz von Warnmeldungen während der Kompilierung des nachfolgenden Programm Texts zu deaktivieren oder wiederherzustellen.

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

Eine `#pragma warning` Anweisung, die die Warnungs Liste auslässt, wirkt sich auf alle Warnungen aus. Eine `#pragma warning` Anweisung, die eine Warnungs Liste enthält, wirkt sich nur auf die Warnungen aus, die in der Liste angegeben sind.

Eine `#pragma warning disable` -Direktive deaktiviert alle oder den angegebenen Satz von Warnungen.

Eine `#pragma warning restore` -Direktive stellt alle oder den angegebenen Satz von Warnungen in dem Zustand wieder her, der am Anfang der Kompilierungseinheit wirksam war. Beachten Sie Folgendes: Wenn eine bestimmte Warnung extern deaktiviert wurde `#pragma warning restore` , wird diese Warnung von a (ob für alle oder eine bestimmte Warnung) nicht erneut aktiviert.

Das folgende Beispiel zeigt die Verwendung `#pragma warning` von, um die Warnung, die bei referenzierten Membern verwiesen wird, vorübergehend zu deaktivieren. dabei wird C# die Warnungs Nummer des Microsoft-Compilers verwendet.
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
