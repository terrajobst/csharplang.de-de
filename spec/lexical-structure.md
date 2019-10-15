---
ms.openlocfilehash: 4676bcd3f0a92260b4e5e20a0aa5b5ec00bf204e
ms.sourcegitcommit: 892af9016b3317a8fae12d195014dc38ba51cf16
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/01/2019
ms.locfileid: "71704072"
---
# <a name="lexical-structure"></a><span data-ttu-id="7f472-101">Lexikalische Struktur</span><span class="sxs-lookup"><span data-stu-id="7f472-101">Lexical structure</span></span>

## <a name="programs"></a><span data-ttu-id="7f472-102">Programs</span><span class="sxs-lookup"><span data-stu-id="7f472-102">Programs</span></span>

<span data-ttu-id="7f472-103">Ein C# ***Programm*** besteht aus mindestens einer ***Quelldatei***, die formal als ***Kompilierungs Einheiten*** ([Kompilierungs Einheiten](namespaces.md#compilation-units)) bekannt ist.</span><span class="sxs-lookup"><span data-stu-id="7f472-103">A C# ***program*** consists of one or more ***source files***, known formally as ***compilation units*** ([Compilation units](namespaces.md#compilation-units)).</span></span> <span data-ttu-id="7f472-104">Eine Quelldatei ist eine geordnete Sequenz von Unicode-Zeichen.</span><span class="sxs-lookup"><span data-stu-id="7f472-104">A source file is an ordered sequence of Unicode characters.</span></span> <span data-ttu-id="7f472-105">Quelldateien verfügen in der Regel über eine eins-zu-eins-Entsprechung zu Dateien in einem Dateisystem, diese Entsprechung ist jedoch nicht erforderlich.</span><span class="sxs-lookup"><span data-stu-id="7f472-105">Source files typically have a one-to-one correspondence with files in a file system, but this correspondence is not required.</span></span> <span data-ttu-id="7f472-106">Zur maximalen Portabilität wird empfohlen, dass Dateien in einem Dateisystem mit der UTF-8-Codierung codiert werden.</span><span class="sxs-lookup"><span data-stu-id="7f472-106">For maximal portability, it is recommended that files in a file system be encoded with the UTF-8 encoding.</span></span>

<span data-ttu-id="7f472-107">Konzeptionell wird ein Programm mithilfe von drei Schritten kompiliert:</span><span class="sxs-lookup"><span data-stu-id="7f472-107">Conceptually speaking, a program is compiled using three steps:</span></span>

1. <span data-ttu-id="7f472-108">Transformation, die eine Datei von einem bestimmten Zeichen-und Codierungsschema in eine Unicode-Zeichenfolge konvertiert.</span><span class="sxs-lookup"><span data-stu-id="7f472-108">Transformation, which converts a file from a particular character repertoire and encoding scheme into a sequence of Unicode characters.</span></span>
2. <span data-ttu-id="7f472-109">Lexikalische Analyse, die einen Stream von Unicode-Eingabezeichen in einen Datenstrom von Token übersetzt.</span><span class="sxs-lookup"><span data-stu-id="7f472-109">Lexical analysis, which translates a stream of Unicode input characters into a stream of tokens.</span></span>
3. <span data-ttu-id="7f472-110">Syntaktische Analyse, bei der der Datenstrom von Token in ausführbaren Code übersetzt wird.</span><span class="sxs-lookup"><span data-stu-id="7f472-110">Syntactic analysis, which translates the stream of tokens into executable code.</span></span>

## <a name="grammars"></a><span data-ttu-id="7f472-111">Grammatiken</span><span class="sxs-lookup"><span data-stu-id="7f472-111">Grammars</span></span>

<span data-ttu-id="7f472-112">Diese Spezifikation zeigt die Syntax der C# Programmiersprache mithilfe von zwei Grammatiken.</span><span class="sxs-lookup"><span data-stu-id="7f472-112">This specification presents the syntax of the C# programming language using two grammars.</span></span> <span data-ttu-id="7f472-113">Die ***lexikalische Grammatik*** ([lexikalische Grammatik](lexical-structure.md#lexical-grammar)) definiert, wie Unicode-Zeichen kombiniert werden, um Zeilen Abschluss Zeichen, Leerzeichen, Kommentare, Token und Vorverarbeitungs Direktiven zu bilden.</span><span class="sxs-lookup"><span data-stu-id="7f472-113">The ***lexical grammar*** ([Lexical grammar](lexical-structure.md#lexical-grammar)) defines how Unicode characters are combined to form line terminators, white space, comments, tokens, and pre-processing directives.</span></span> <span data-ttu-id="7f472-114">Die ***syntaktische Grammatik*** ([syntaktische Grammatik](lexical-structure.md#syntactic-grammar)) definiert, wie die aus der lexikalischen Grammatik resultierenden Token in Form C# von Programmen kombiniert werden.</span><span class="sxs-lookup"><span data-stu-id="7f472-114">The ***syntactic grammar*** ([Syntactic grammar](lexical-structure.md#syntactic-grammar)) defines how the tokens resulting from the lexical grammar are combined to form C# programs.</span></span>

### <a name="grammar-notation"></a><span data-ttu-id="7f472-115">Grammatik Notation</span><span class="sxs-lookup"><span data-stu-id="7f472-115">Grammar notation</span></span>

<span data-ttu-id="7f472-116">Die lexikalischen und syntaktischen Grammatiken werden im Backus-Naur-Formular mithilfe der Notation des antlr-Grammatik Tools dargestellt.</span><span class="sxs-lookup"><span data-stu-id="7f472-116">The lexical and syntactic grammars are presented in Backus-Naur form using the notation of the ANTLR grammar tool.</span></span>

### <a name="lexical-grammar"></a><span data-ttu-id="7f472-117">Lexikalische Grammatik</span><span class="sxs-lookup"><span data-stu-id="7f472-117">Lexical grammar</span></span>

<span data-ttu-id="7f472-118">Die lexikalische Grammatik von C# wird in [lexikalischen Analysen](lexical-structure.md#lexical-analysis), [Token](lexical-structure.md#tokens)und [Vorverarbeitungs Direktiven](lexical-structure.md#pre-processing-directives)dargestellt.</span><span class="sxs-lookup"><span data-stu-id="7f472-118">The lexical grammar of C# is presented in [Lexical analysis](lexical-structure.md#lexical-analysis), [Tokens](lexical-structure.md#tokens), and [Pre-processing directives](lexical-structure.md#pre-processing-directives).</span></span> <span data-ttu-id="7f472-119">Die Terminal Symbole der lexikalischen Grammatik sind die Zeichen des Unicode-Zeichensatzes, und die lexikalische Grammatik gibt an, wie Zeichen kombiniert werden, um Token ([Token](lexical-structure.md#tokens)), Leerzeichen ([Leerraum](lexical-structure.md#white-space)), Kommentare ([Kommentare](lexical-structure.md#comments)) und Vorverarbeitungs Direktiven ([Vorverarbeitungs Direktiven](lexical-structure.md#pre-processing-directives)).</span><span class="sxs-lookup"><span data-stu-id="7f472-119">The terminal symbols of the lexical grammar are the characters of the Unicode character set, and the lexical grammar specifies how characters are combined to form tokens ([Tokens](lexical-structure.md#tokens)), white space ([White space](lexical-structure.md#white-space)), comments ([Comments](lexical-structure.md#comments)), and pre-processing directives ([Pre-processing directives](lexical-structure.md#pre-processing-directives)).</span></span>

<span data-ttu-id="7f472-120">Jede Quelldatei in einem C# Programm muss der *Eingabe* Produktion der lexikalischen Grammatik ([lexikalische Analyse](lexical-structure.md#lexical-analysis)) entsprechen.</span><span class="sxs-lookup"><span data-stu-id="7f472-120">Every source file in a C# program must conform to the *input* production of the lexical grammar ([Lexical analysis](lexical-structure.md#lexical-analysis)).</span></span>

### <a name="syntactic-grammar"></a><span data-ttu-id="7f472-121">Syntaktische Grammatik</span><span class="sxs-lookup"><span data-stu-id="7f472-121">Syntactic grammar</span></span>

<span data-ttu-id="7f472-122">Die syntaktische Grammatik von C# wird in den Kapiteln und Anhänge dargestellt, die diesem Kapitel folgen.</span><span class="sxs-lookup"><span data-stu-id="7f472-122">The syntactic grammar of C# is presented in the chapters and appendices that follow this chapter.</span></span> <span data-ttu-id="7f472-123">Die Terminal Symbole der syntaktischen Grammatik sind die von der lexikalischen Grammatik definierten Token, und die syntaktische Grammatik gibt an, wie Token in Form C# von Programmen kombiniert werden.</span><span class="sxs-lookup"><span data-stu-id="7f472-123">The terminal symbols of the syntactic grammar are the tokens defined by the lexical grammar, and the syntactic grammar specifies how tokens are combined to form C# programs.</span></span>

<span data-ttu-id="7f472-124">Jede Quelldatei in einem C# Programm muss der *compilation_unit* -Produktion der syntaktischen Grammatik ([Kompilierungs Einheiten](namespaces.md#compilation-units)) entsprechen.</span><span class="sxs-lookup"><span data-stu-id="7f472-124">Every source file in a C# program must conform to the *compilation_unit* production of the syntactic grammar ([Compilation units](namespaces.md#compilation-units)).</span></span>

## <a name="lexical-analysis"></a><span data-ttu-id="7f472-125">Lexikalische Analyse</span><span class="sxs-lookup"><span data-stu-id="7f472-125">Lexical analysis</span></span>

<span data-ttu-id="7f472-126">Die *Eingabe* Produktion definiert die lexikalische Struktur einer C# Quelldatei.</span><span class="sxs-lookup"><span data-stu-id="7f472-126">The *input* production defines the lexical structure of a C# source file.</span></span> <span data-ttu-id="7f472-127">Jede Quelldatei in einem C# Programm muss mit dieser lexikalischen Grammatik-Produktion übereinstimmen.</span><span class="sxs-lookup"><span data-stu-id="7f472-127">Each source file in a C# program must conform to this lexical grammar production.</span></span>

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

<span data-ttu-id="7f472-128">Fünf grundlegende Elemente bilden die lexikalische Struktur einer C# Quelldatei: Zeilen Abschluss Zeichen ([Zeilen](lexical-structure.md#line-terminators)Abschluss Zeichen), Leerraum (leer[Zeichen](lexical-structure.md#white-space)), Kommentare ([Kommentare](lexical-structure.md#comments)), Token ([Token](lexical-structure.md#tokens)) und Vorverarbeitungs Direktiven ([Vorverarbeitungs Direktiven](lexical-structure.md#pre-processing-directives)).</span><span class="sxs-lookup"><span data-stu-id="7f472-128">Five basic elements make up the lexical structure of a C# source file: Line terminators ([Line terminators](lexical-structure.md#line-terminators)), white space ([White space](lexical-structure.md#white-space)), comments ([Comments](lexical-structure.md#comments)), tokens ([Tokens](lexical-structure.md#tokens)), and pre-processing directives ([Pre-processing directives](lexical-structure.md#pre-processing-directives)).</span></span> <span data-ttu-id="7f472-129">Von diesen grundlegenden Elementen sind nur Token in der syntaktischen Grammatik eines C# Programms ([syntaktische Grammatik](lexical-structure.md#syntactic-grammar)) von Bedeutung.</span><span class="sxs-lookup"><span data-stu-id="7f472-129">Of these basic elements, only tokens are significant in the syntactic grammar of a C# program ([Syntactic grammar](lexical-structure.md#syntactic-grammar)).</span></span>

<span data-ttu-id="7f472-130">Die lexikalische Verarbeitung einer C# Quelldatei besteht darin, die Datei in eine Folge von Token zu reduzieren, die zur Eingabe für die syntaktische Analyse wird.</span><span class="sxs-lookup"><span data-stu-id="7f472-130">The lexical processing of a C# source file consists of reducing the file into a sequence of tokens which becomes the input to the syntactic analysis.</span></span> <span data-ttu-id="7f472-131">Zeilen Abschluss Zeichen, Leerräume und Kommentare können für separate Token dienen, und Vorverarbeitungs Direktiven können dazu führen, dass Abschnitte der Quelldatei übersprungen werden. andernfalls haben diese lexikalischen Elemente keinerlei Auswirkung auf die syntaktische Struktur eines C# Programms.</span><span class="sxs-lookup"><span data-stu-id="7f472-131">Line terminators, white space, and comments can serve to separate tokens, and pre-processing directives can cause sections of the source file to be skipped, but otherwise these lexical elements have no impact on the syntactic structure of a C# program.</span></span>

<span data-ttu-id="7f472-132">Bei interpoliert-Zeichen folgen Literalen ([interpoliert Zeichen folgen Literale](lexical-structure.md#interpolated-string-literals)) wird zunächst ein einzelnes Token von der lexikalischen Analyse erstellt, aber in mehrere Eingabeelemente unterteilt, die wiederholt der lexikalischen Analyse unterzogen werden, bis alle interpoliert werden. Zeichenfolgenliterale wurden aufgelöst.</span><span class="sxs-lookup"><span data-stu-id="7f472-132">In the case of interpolated string literals ([Interpolated string literals](lexical-structure.md#interpolated-string-literals)) a single token is initially produced by lexical analysis, but is broken up into several input elements which are repeatedly subjected to lexical analysis until all interpolated string literals have been resolved.</span></span> <span data-ttu-id="7f472-133">Die resultierenden Token dienen dann als Eingabe für die syntaktische Analyse.</span><span class="sxs-lookup"><span data-stu-id="7f472-133">The resulting tokens then serve as input to the syntactic analysis.</span></span>

<span data-ttu-id="7f472-134">Wenn mehrere lexikalische Grammatik Produktionen einer Sequenz von Zeichen in einer Quelldatei entsprechen, bildet die lexikalische Verarbeitung immer das längstmögliche lexikalische Element.</span><span class="sxs-lookup"><span data-stu-id="7f472-134">When several lexical grammar productions match a sequence of characters in a source file, the lexical processing always forms the longest possible lexical element.</span></span> <span data-ttu-id="7f472-135">Beispielsweise wird die Zeichenfolge `//` als Anfang eines einzeiligen Kommentars verarbeitet, da dieses lexikalische Element länger als ein einzelnes `/` Token ist.</span><span class="sxs-lookup"><span data-stu-id="7f472-135">For example, the character sequence `//` is processed as the beginning of a single-line comment because that lexical element is longer than a single `/` token.</span></span>

### <a name="line-terminators"></a><span data-ttu-id="7f472-136">Zeilen Abschluss Zeichen</span><span class="sxs-lookup"><span data-stu-id="7f472-136">Line terminators</span></span>

<span data-ttu-id="7f472-137">Zeilen Abschluss Zeichen teilen die Zeichen einer C# Quelldatei in Zeilen auf.</span><span class="sxs-lookup"><span data-stu-id="7f472-137">Line terminators divide the characters of a C# source file into lines.</span></span>

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

<span data-ttu-id="7f472-138">Aus Kompatibilitätsgründen mit Tools zur Quell Code Bearbeitung, die Dateiendemarker hinzufügen und eine Quelldatei als Sequenz von ordnungsgemäß beendeten Zeilen angezeigt werden können, werden die folgenden Transformationen in der entsprechenden Reihenfolge auf jede Quelldatei in einem C# Programm angewendet:</span><span class="sxs-lookup"><span data-stu-id="7f472-138">For compatibility with source code editing tools that add end-of-file markers, and to enable a source file to be viewed as a sequence of properly terminated lines, the following transformations are applied, in order, to every source file in a C# program:</span></span>

*  <span data-ttu-id="7f472-139">Wenn das letzte Zeichen der Quelldatei ein Control-Z-Zeichen (`U+001A`) ist, wird dieses Zeichen gelöscht.</span><span class="sxs-lookup"><span data-stu-id="7f472-139">If the last character of the source file is a Control-Z character (`U+001A`), this character is deleted.</span></span>
*  <span data-ttu-id="7f472-140">Ein Wagen Rücklauf Zeichen (`U+000D`) wird am Ende der Quelldatei hinzugefügt, wenn diese Quelldatei nicht leer ist, und wenn das letzte Zeichen der Quelldatei kein Wagen Rücklauf Zeichen (`U+000D`)`U+000A`, ein Zeilenvorschub (), ein Zeilen Trennzeichen`U+2028`() oder ein Absatz Trennzeichen (`U+2029`).</span><span class="sxs-lookup"><span data-stu-id="7f472-140">A carriage-return character (`U+000D`) is added to the end of the source file if that source file is non-empty and if the last character of the source file is not a carriage return (`U+000D`), a line feed (`U+000A`), a line separator (`U+2028`), or a paragraph separator (`U+2029`).</span></span>

### <a name="comments"></a><span data-ttu-id="7f472-141">Kommentare</span><span class="sxs-lookup"><span data-stu-id="7f472-141">Comments</span></span>

<span data-ttu-id="7f472-142">Zwei Arten von Kommentaren werden unterstützt: einzeilige Kommentare und durch Trennzeichen getrennte Kommentare.</span><span class="sxs-lookup"><span data-stu-id="7f472-142">Two forms of comments are supported: single-line comments and delimited comments.</span></span> <span data-ttu-id="7f472-143">***Einzeilige Kommentare*** beginnen mit den Zeichen `//` und werden bis zum Ende der Quellzeile erweitert.</span><span class="sxs-lookup"><span data-stu-id="7f472-143">***Single-line comments*** start with the characters `//` and extend to the end of the source line.</span></span> <span data-ttu-id="7f472-144">Durch Trennzeichen getrennte ***Kommentare*** beginnen mit `/*` den Zeichen und enden mit `*/`den Zeichen.</span><span class="sxs-lookup"><span data-stu-id="7f472-144">***Delimited comments*** start with the characters `/*` and end with the characters `*/`.</span></span> <span data-ttu-id="7f472-145">Begrenzungs Kommentare können sich über mehrere Zeilen erstrecken.</span><span class="sxs-lookup"><span data-stu-id="7f472-145">Delimited comments may span multiple lines.</span></span>

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

<span data-ttu-id="7f472-146">Kommentare werden nicht geschachtelt.</span><span class="sxs-lookup"><span data-stu-id="7f472-146">Comments do not nest.</span></span> <span data-ttu-id="7f472-147">Die Zeichen folgen `/*` und `*/` haben keine besondere Bedeutung innerhalb eines `//` Kommentars, und die Zeichen `//` folgen `/*` und haben keine besondere Bedeutung in einem durch Trennzeichen getrennten Kommentar.</span><span class="sxs-lookup"><span data-stu-id="7f472-147">The character sequences `/*` and `*/` have no special meaning within a `//` comment, and the character sequences `//` and `/*` have no special meaning within a delimited comment.</span></span>

<span data-ttu-id="7f472-148">Kommentare werden nicht innerhalb von Zeichen-und Zeichen folgen literalen verarbeitet.</span><span class="sxs-lookup"><span data-stu-id="7f472-148">Comments are not processed within character and string literals.</span></span>

<span data-ttu-id="7f472-149">Das Beispiel</span><span class="sxs-lookup"><span data-stu-id="7f472-149">The example</span></span>
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
<span data-ttu-id="7f472-150">enthält einen durch Trennzeichen getrennten Kommentar.</span><span class="sxs-lookup"><span data-stu-id="7f472-150">includes a delimited comment.</span></span>

<span data-ttu-id="7f472-151">Das Beispiel</span><span class="sxs-lookup"><span data-stu-id="7f472-151">The example</span></span>
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
<span data-ttu-id="7f472-152">zeigt mehrere einzeilige Kommentare an.</span><span class="sxs-lookup"><span data-stu-id="7f472-152">shows several single-line comments.</span></span>

### <a name="white-space"></a><span data-ttu-id="7f472-153">Leerraum</span><span class="sxs-lookup"><span data-stu-id="7f472-153">White space</span></span>

<span data-ttu-id="7f472-154">Leerraum ist als beliebiges Zeichen mit Unicode-Klassen-ZS (einschließlich Leerzeichen) sowie dem horizontalen Tabstopp Zeichen, dem vertikalen Tabstopp Zeichen und dem Formular Vorschub Zeichen definiert.</span><span class="sxs-lookup"><span data-stu-id="7f472-154">White space is defined as any character with Unicode class Zs (which includes the space character) as well as the horizontal tab character, the vertical tab character, and the form feed character.</span></span>

```antlr
whitespace
    : '<Any character with Unicode class Zs>'
    | '<Horizontal tab character (U+0009)>'
    | '<Vertical tab character (U+000B)>'
    | '<Form feed character (U+000C)>'
    ;
```

## <a name="tokens"></a><span data-ttu-id="7f472-155">tokens</span><span class="sxs-lookup"><span data-stu-id="7f472-155">Tokens</span></span>

<span data-ttu-id="7f472-156">Es gibt mehrere Arten von Token: Bezeichner, Schlüsselwörter, Literale, Operatoren und Satzzeichen.</span><span class="sxs-lookup"><span data-stu-id="7f472-156">There are several kinds of tokens: identifiers, keywords, literals, operators, and punctuators.</span></span> <span data-ttu-id="7f472-157">Leerzeichen und Kommentare sind keine Token, obwohl Sie als Trennzeichen für Token fungieren.</span><span class="sxs-lookup"><span data-stu-id="7f472-157">White space and comments are not tokens, though they act as separators for tokens.</span></span>

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

### <a name="unicode-character-escape-sequences"></a><span data-ttu-id="7f472-158">Unicode-Escapesequenzen</span><span class="sxs-lookup"><span data-stu-id="7f472-158">Unicode character escape sequences</span></span>

<span data-ttu-id="7f472-159">Eine Unicode-Escapesequenz stellt ein Unicode-Zeichen dar.</span><span class="sxs-lookup"><span data-stu-id="7f472-159">A Unicode character escape sequence represents a Unicode character.</span></span> <span data-ttu-id="7f472-160">Escapesequenzen von Unicode-Zeichen werden in bezeichern ([bezeichern](lexical-structure.md#identifiers)), Zeichen Literalen ([Zeichen literalen](lexical-structure.md#character-literals)) und regulären Zeichen folgen Literalen ([Zeichen folgen Literale](lexical-structure.md#string-literals)) verarbeitet.</span><span class="sxs-lookup"><span data-stu-id="7f472-160">Unicode character escape sequences are processed in identifiers ([Identifiers](lexical-structure.md#identifiers)), character literals ([Character literals](lexical-structure.md#character-literals)), and regular string literals ([String literals](lexical-structure.md#string-literals)).</span></span> <span data-ttu-id="7f472-161">Eine Unicode-Escapesequenz wird an keinem anderen Speicherort verarbeitet (z. b. zum bilden eines Operators, eines interpunterators oder eines Schlüssel Worts).</span><span class="sxs-lookup"><span data-stu-id="7f472-161">A Unicode character escape is not processed in any other location (for example, to form an operator, punctuator, or keyword).</span></span>

```antlr
unicode_escape_sequence
    : '\\u' hex_digit hex_digit hex_digit hex_digit
    | '\\U' hex_digit hex_digit hex_digit hex_digit hex_digit hex_digit hex_digit hex_digit
    ;
```

<span data-ttu-id="7f472-162">Eine Unicode-Escapesequenz stellt das einzelne Unicode-Zeichen dar, das durch die hexadezimal`\U`Zahl nach den Zeichen "`\u`" oder "" gebildet wird.</span><span class="sxs-lookup"><span data-stu-id="7f472-162">A Unicode escape sequence represents the single Unicode character formed by the hexadecimal number following the "`\u`" or "`\U`" characters.</span></span> <span data-ttu-id="7f472-163">Da C# eine 16-Bit-Codierung von Unicode-Code Punkten in Zeichen und Zeichen folgen Werten verwendet, ist ein Unicode-Zeichen im Bereich u + 10000 bis U + 10FFFF in einem Zeichenliterals nicht zulässig und wird mit einem Unicode-Ersatz Zeichenpaar in einem Zeichenfolgenliteralzeichen dargestellt.</span><span class="sxs-lookup"><span data-stu-id="7f472-163">Since C# uses a 16-bit encoding of Unicode code points in characters and string values, a Unicode character in the range U+10000 to U+10FFFF is not permitted in a character literal and is represented using a Unicode surrogate pair in a string literal.</span></span> <span data-ttu-id="7f472-164">Unicode-Zeichen mit Code Punkten oberhalb von 0x10FFFF werden nicht unterstützt.</span><span class="sxs-lookup"><span data-stu-id="7f472-164">Unicode characters with code points above 0x10FFFF are not supported.</span></span>

<span data-ttu-id="7f472-165">Es werden nicht mehrere Übersetzungen ausgeführt.</span><span class="sxs-lookup"><span data-stu-id="7f472-165">Multiple translations are not performed.</span></span> <span data-ttu-id="7f472-166">Beispielsweise entspricht`\u005Cu005C``\u005C`das Zeichen folgen Literalzeichen "" anstelle von "`\`".</span><span class="sxs-lookup"><span data-stu-id="7f472-166">For instance, the string literal "`\u005Cu005C`" is equivalent to "`\u005C`" rather than "`\`".</span></span> <span data-ttu-id="7f472-167">Der Unicode- `\u005C` Wert ist das Zeichen`\`"".</span><span class="sxs-lookup"><span data-stu-id="7f472-167">The Unicode value `\u005C` is the character "`\`".</span></span>

<span data-ttu-id="7f472-168">Das Beispiel</span><span class="sxs-lookup"><span data-stu-id="7f472-168">The example</span></span>
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
<span data-ttu-id="7f472-169">zeigt verschiedene Verwendungen `\u0066`von, d. h. die Escapesequenz für den Buchstaben "`f`".</span><span class="sxs-lookup"><span data-stu-id="7f472-169">shows several uses of `\u0066`, which is the escape sequence for the letter "`f`".</span></span> <span data-ttu-id="7f472-170">Das Programm entspricht</span><span class="sxs-lookup"><span data-stu-id="7f472-170">The program is equivalent to</span></span>
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

### <a name="identifiers"></a><span data-ttu-id="7f472-171">Bezeichner</span><span class="sxs-lookup"><span data-stu-id="7f472-171">Identifiers</span></span>

<span data-ttu-id="7f472-172">Die Regeln für Bezeichner, die in diesem Abschnitt angegeben sind, entsprechen genau den Regeln, die vom Unicode-Standard Anhang 31 empfohlen werden, mit dem Unterschied, dass Unterstriche als Ausgangs Zeichen zulässig sind (wie es in der C-Programmiersprache üblich ist). zulässig in bezeichlern, und das Zeichen`@`"" ist als Präfix zulässig, damit Schlüsselwörter als Bezeichner verwendet werden können.</span><span class="sxs-lookup"><span data-stu-id="7f472-172">The rules for identifiers given in this section correspond exactly to those recommended by the Unicode Standard Annex 31, except that underscore is allowed as an initial character (as is traditional in the C programming language), Unicode escape sequences are permitted in identifiers, and the "`@`" character is allowed as a prefix to enable keywords to be used as identifiers.</span></span>

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

<span data-ttu-id="7f472-173">Informationen zu den oben erwähnten Unicode-Zeichenklassen finden Sie im Unicode-Standard, Version 3,0, Abschnitt 4,5.</span><span class="sxs-lookup"><span data-stu-id="7f472-173">For information on the Unicode character classes mentioned above, see The Unicode Standard, Version 3.0, section 4.5.</span></span>

<span data-ttu-id="7f472-174">Beispiele für gültige Bezeichner sind "`identifier1`", "`_identifier2`" und "`@if`".</span><span class="sxs-lookup"><span data-stu-id="7f472-174">Examples of valid identifiers include "`identifier1`", "`_identifier2`", and "`@if`".</span></span>

<span data-ttu-id="7f472-175">Ein Bezeichner in einem übereinstimmenden Programm muss das kanonische Format aufweisen, das durch die Unicode-normalisierungs Form C definiert ist, wie im Unicode-Standard Anhang 15 definiert</span><span class="sxs-lookup"><span data-stu-id="7f472-175">An identifier in a conforming program must be in the canonical format defined by Unicode Normalization Form C, as defined by Unicode Standard Annex 15.</span></span> <span data-ttu-id="7f472-176">Das Verhalten beim Auffinden eines Bezeichners, der nicht in der normalisierungs Form C vorliegt, ist Implementierungs definiert. eine Diagnose ist jedoch nicht erforderlich.</span><span class="sxs-lookup"><span data-stu-id="7f472-176">The behavior when encountering an identifier not in Normalization Form C is implementation-defined; however, a diagnostic is not required.</span></span>

<span data-ttu-id="7f472-177">Das-Präfix "`@`" ermöglicht die Verwendung von Schlüsselwörtern als Bezeichner, was bei der Schnittstellen mit anderen Programmiersprachen nützlich ist.</span><span class="sxs-lookup"><span data-stu-id="7f472-177">The prefix "`@`" enables the use of keywords as identifiers, which is useful when interfacing with other programming languages.</span></span> <span data-ttu-id="7f472-178">Das Zeichen `@` ist nicht Teil des Bezeichners, daher kann der Bezeichner in anderen Sprachen als normaler Bezeichner ohne das Präfix angezeigt werden.</span><span class="sxs-lookup"><span data-stu-id="7f472-178">The character `@` is not actually part of the identifier, so the identifier might be seen in other languages as a normal identifier, without the prefix.</span></span> <span data-ttu-id="7f472-179">Ein Bezeichner mit `@` einem Präfix wird als ***ausführlicher Bezeichner***bezeichnet.</span><span class="sxs-lookup"><span data-stu-id="7f472-179">An identifier with an `@` prefix is called a ***verbatim identifier***.</span></span> <span data-ttu-id="7f472-180">Die `@` Verwendung des Präfix für Bezeichner, bei denen es sich nicht um Schlüsselwörter handelt, ist zulässig, aber es wird dringend davon abgeraten.</span><span class="sxs-lookup"><span data-stu-id="7f472-180">Use of the `@` prefix for identifiers that are not keywords is permitted, but strongly discouraged as a matter of style.</span></span>

<span data-ttu-id="7f472-181">Das Beispiel:</span><span class="sxs-lookup"><span data-stu-id="7f472-181">The example:</span></span>
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
<span data-ttu-id="7f472-182">definiert eine Klasse`class`mit dem Namen "" mit einer statischen Methode`static`mit dem Namen "", die`bool`einen Parameter mit dem Namen "" annimmt.</span><span class="sxs-lookup"><span data-stu-id="7f472-182">defines a class named "`class`" with a static method named "`static`" that takes a parameter named "`bool`".</span></span> <span data-ttu-id="7f472-183">Beachten Sie, dass das Token "`cl\u0061ss`" ein Bezeichner ist, da Unicode-Escapezeichen in Schlüsselwörtern nicht zulässig sind und denselben Bezeichner wie "`@class`" haben.</span><span class="sxs-lookup"><span data-stu-id="7f472-183">Note that since Unicode escapes are not permitted in keywords, the token "`cl\u0061ss`" is an identifier, and is the same identifier as "`@class`".</span></span>

<span data-ttu-id="7f472-184">Zwei Bezeichner werden als identisch angesehen, wenn Sie nach dem Anwenden der folgenden Transformationen identisch sind:</span><span class="sxs-lookup"><span data-stu-id="7f472-184">Two identifiers are considered the same if they are identical after the following transformations are applied, in order:</span></span>

*  <span data-ttu-id="7f472-185">Wenn Sie verwendet`@`wird, wird das Präfix "" entfernt.</span><span class="sxs-lookup"><span data-stu-id="7f472-185">The prefix "`@`", if used, is removed.</span></span>
*  <span data-ttu-id="7f472-186">Jede *unicode_escape_sequence* wird in das entsprechende Unicode-Zeichen transformiert.</span><span class="sxs-lookup"><span data-stu-id="7f472-186">Each *unicode_escape_sequence* is transformed into its corresponding Unicode character.</span></span>
*  <span data-ttu-id="7f472-187">Alle *formatting_character*s werden entfernt.</span><span class="sxs-lookup"><span data-stu-id="7f472-187">Any *formatting_character*s are removed.</span></span>

<span data-ttu-id="7f472-188">Bezeichner, die zwei aufeinander folgende unter`U+005F`Striche () enthalten, sind für die Verwendung durch die-Implementierung reserviert.</span><span class="sxs-lookup"><span data-stu-id="7f472-188">Identifiers containing two consecutive underscore characters (`U+005F`) are reserved for use by the implementation.</span></span> <span data-ttu-id="7f472-189">Beispielsweise kann eine-Implementierung erweiterte Schlüsselwörter bereitstellen, die mit zwei unterstrichen beginnen.</span><span class="sxs-lookup"><span data-stu-id="7f472-189">For example, an implementation might provide extended keywords that begin with two underscores.</span></span>

### <a name="keywords"></a><span data-ttu-id="7f472-190">Stichwörter</span><span class="sxs-lookup"><span data-stu-id="7f472-190">Keywords</span></span>

<span data-ttu-id="7f472-191">Ein ***Schlüsselwort*** ist eine bezeichnerartige Zeichenfolge, die reserviert ist und nicht als Bezeichner verwendet werden kann, es sei denn, `@` das Zeichen wird vorangestellt.</span><span class="sxs-lookup"><span data-stu-id="7f472-191">A ***keyword*** is an identifier-like sequence of characters that is reserved, and cannot be used as an identifier except when prefaced by the `@` character.</span></span>

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

<span data-ttu-id="7f472-192">An manchen Stellen in der Grammatik haben bestimmte Bezeichner eine besondere Bedeutung, aber keine Schlüsselwörter.</span><span class="sxs-lookup"><span data-stu-id="7f472-192">In some places in the grammar, specific identifiers have special meaning, but are not keywords.</span></span> <span data-ttu-id="7f472-193">Solche Bezeichner werden manchmal als "kontextabhängige Schlüsselwörter" bezeichnet.</span><span class="sxs-lookup"><span data-stu-id="7f472-193">Such identifiers are sometimes referred to as "contextual keywords".</span></span> <span data-ttu-id="7f472-194">In einer Eigenschafts Deklaration haben die Bezeichner`get`"" und`set`"" z. b. eine besondere Bedeutung ([Accessoren](classes.md#accessors)).</span><span class="sxs-lookup"><span data-stu-id="7f472-194">For example, within a property declaration, the "`get`" and "`set`" identifiers have special meaning ([Accessors](classes.md#accessors)).</span></span> <span data-ttu-id="7f472-195">Ein anderer Bezeichner `get` als `set` oder ist in diesen Speicherorten nie zulässig, sodass diese Verwendung nicht mit der Verwendung dieser Wörter als Bezeichner in Konflikt steht.</span><span class="sxs-lookup"><span data-stu-id="7f472-195">An identifier other than `get` or `set` is never permitted in these locations, so this use does not conflict with a use of these words as identifiers.</span></span> <span data-ttu-id="7f472-196">In anderen Fällen, z. b. mit dem`var`Bezeichner "" in implizit typisierten lokalen Variablen Deklarationen ([lokale Variablen Deklarationen](statements.md#local-variable-declarations)), kann ein Kontext Schlüsselwort mit deklarierten Namen in Konflikt stehen.</span><span class="sxs-lookup"><span data-stu-id="7f472-196">In other cases, such as with the identifier "`var`" in implicitly typed local variable declarations ([Local variable declarations](statements.md#local-variable-declarations)), a contextual keyword can conflict with declared names.</span></span> <span data-ttu-id="7f472-197">In solchen Fällen hat der deklarierte Name Vorrang vor der Verwendung des Bezeichners als kontextbezogenes Schlüsselwort.</span><span class="sxs-lookup"><span data-stu-id="7f472-197">In such cases, the declared name takes precedence over the use of the identifier as a contextual keyword.</span></span>

### <a name="literals"></a><span data-ttu-id="7f472-198">Literale</span><span class="sxs-lookup"><span data-stu-id="7f472-198">Literals</span></span>

<span data-ttu-id="7f472-199">Ein ***Literalwert*** ist eine Quell Code Darstellung eines Werts.</span><span class="sxs-lookup"><span data-stu-id="7f472-199">A ***literal*** is a source code representation of a value.</span></span>

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

#### <a name="boolean-literals"></a><span data-ttu-id="7f472-200">Boolesche Literale</span><span class="sxs-lookup"><span data-stu-id="7f472-200">Boolean literals</span></span>

<span data-ttu-id="7f472-201">Es gibt zwei boolesche Literalwerte `true` : `false`und.</span><span class="sxs-lookup"><span data-stu-id="7f472-201">There are two boolean literal values: `true` and `false`.</span></span>

```antlr
boolean_literal
    : 'true'
    | 'false'
    ;
```

<span data-ttu-id="7f472-202">Der Typ eines *boolean_literal* ist `bool`.</span><span class="sxs-lookup"><span data-stu-id="7f472-202">The type of a *boolean_literal* is `bool`.</span></span>

#### <a name="integer-literals"></a><span data-ttu-id="7f472-203">Ganzzahlenliteral</span><span class="sxs-lookup"><span data-stu-id="7f472-203">Integer literals</span></span>

<span data-ttu-id="7f472-204">Ganzzahlige Literale werden verwendet, um Werte `int`der `uint`Typen `long`,, `ulong`und zu schreiben.</span><span class="sxs-lookup"><span data-stu-id="7f472-204">Integer literals are used to write values of types `int`, `uint`, `long`, and `ulong`.</span></span> <span data-ttu-id="7f472-205">Ganzzahlige Literale haben zwei mögliche Formen: decimal und hexadezimal.</span><span class="sxs-lookup"><span data-stu-id="7f472-205">Integer literals have two possible forms: decimal and hexadecimal.</span></span>

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

<span data-ttu-id="7f472-206">Der Typ eines ganzzahligen Literals wird wie folgt bestimmt:</span><span class="sxs-lookup"><span data-stu-id="7f472-206">The type of an integer literal is determined as follows:</span></span>

*  <span data-ttu-id="7f472-207">Wenn das Literale kein Suffix aufweist, verfügt es über den ersten dieser Typen, in dem sein Wert dargestellt werden `int`kann `uint`: `long`, `ulong`,,.</span><span class="sxs-lookup"><span data-stu-id="7f472-207">If the literal has no suffix, it has the first of these types in which its value can be represented: `int`, `uint`, `long`, `ulong`.</span></span>
*  <span data-ttu-id="7f472-208">`U` Wenn das Literale von oder `u`als Suffix versehen wird, verfügt es über den ersten dieser Typen, in dem sein Wert dargestellt werden `ulong`kann: `uint`,.</span><span class="sxs-lookup"><span data-stu-id="7f472-208">If the literal is suffixed by `U` or `u`, it has the first of these types in which its value can be represented: `uint`, `ulong`.</span></span>
*  <span data-ttu-id="7f472-209">`L` Wenn das Literale von oder `l`als Suffix versehen wird, verfügt es über den ersten dieser Typen, in dem sein Wert dargestellt werden `ulong`kann: `long`,.</span><span class="sxs-lookup"><span data-stu-id="7f472-209">If the literal is suffixed by `L` or `l`, it has the first of these types in which its value can be represented: `long`, `ulong`.</span></span>
*  <span data-ttu-id="7f472-210">Wenn das Literale `UL`von, `LU` `ulong`,,,, `Lu` ,`lU`oder suffixtwird,istesvomTyp`lu`. `ul` `uL` `Ul`</span><span class="sxs-lookup"><span data-stu-id="7f472-210">If the literal is suffixed by `UL`, `Ul`, `uL`, `ul`, `LU`, `Lu`, `lU`, or `lu`, it is of type `ulong`.</span></span>

<span data-ttu-id="7f472-211">Wenn der durch ein `ulong` Ganzzahlliteral dargestellte Wert außerhalb des Bereichs des Typs liegt, tritt ein Kompilierzeitfehler auf.</span><span class="sxs-lookup"><span data-stu-id="7f472-211">If the value represented by an integer literal is outside the range of the `ulong` type, a compile-time error occurs.</span></span>

<span data-ttu-id="7f472-212">Es wird empfohlen, beim Schreiben von literalen des Typs`L` `long`"" anstelle von "`l`" zu verwenden, da es einfach ist, den Buchstaben "`l`" mit der Ziffer "`1`" zu verwechseln.</span><span class="sxs-lookup"><span data-stu-id="7f472-212">As a matter of style, it is suggested that "`L`" be used instead of "`l`" when writing literals of type `long`, since it is easy to confuse the letter "`l`" with the digit "`1`".</span></span>

<span data-ttu-id="7f472-213">Um zuzulassen, dass die `int` kleinsten `long` möglichen Werte und als Dezimale ganzzahlige Literale geschrieben werden, sind die folgenden zwei Regeln vorhanden:</span><span class="sxs-lookup"><span data-stu-id="7f472-213">To permit the smallest possible `int` and `long` values to be written as decimal integer literals, the following two rules exist:</span></span>

* <span data-ttu-id="7f472-214">Wenn ein *decimal_integer_literal* mit dem Wert 2147483648 (2 ^ 31) und No *integer_type_suffix* als Token unmittelbar nach einem unären Minus Operator Token ([unärer Minus Operator](expressions.md#unary-minus-operator)) angezeigt wird, ist das Ergebnis eine Konstante vom Typ `int` mit dem Wert-2147483648 (-2 ^ 31).</span><span class="sxs-lookup"><span data-stu-id="7f472-214">When a *decimal_integer_literal* with the value 2147483648 (2^31) and no *integer_type_suffix* appears as the token immediately following a unary minus operator token ([Unary minus operator](expressions.md#unary-minus-operator)), the result is a constant of type `int` with the value -2147483648 (-2^31).</span></span> <span data-ttu-id="7f472-215">In allen anderen Fällen ist ein solcher *decimal_integer_literal* vom Typ `uint`.</span><span class="sxs-lookup"><span data-stu-id="7f472-215">In all other situations, such a *decimal_integer_literal* is of type `uint`.</span></span>
* <span data-ttu-id="7f472-216">Wenn ein *decimal_integer_literal* mit dem Wert 9.223.372.036.854.775.808 (2 ^ 63) und No *integer_type_suffix* oder *integer_type_suffix* `L` oder `l` als Token unmittelbar nach einem unären Minus Operator Token angezeigt wird ([ Unärer Minus-Operator](expressions.md#unary-minus-operator)), das Ergebnis ist eine Konstante vom Typ `long` mit dem Wert-9.223.372.036.854.775.808 (-2 ^ 63).</span><span class="sxs-lookup"><span data-stu-id="7f472-216">When a *decimal_integer_literal* with the value 9223372036854775808 (2^63) and no *integer_type_suffix* or the *integer_type_suffix* `L` or `l` appears as the token immediately following a unary minus operator token ([Unary minus operator](expressions.md#unary-minus-operator)), the result is a constant of type `long` with the value -9223372036854775808 (-2^63).</span></span> <span data-ttu-id="7f472-217">In allen anderen Fällen ist ein solcher *decimal_integer_literal* vom Typ `ulong`.</span><span class="sxs-lookup"><span data-stu-id="7f472-217">In all other situations, such a *decimal_integer_literal* is of type `ulong`.</span></span>

#### <a name="real-literals"></a><span data-ttu-id="7f472-218">Echte Literale</span><span class="sxs-lookup"><span data-stu-id="7f472-218">Real literals</span></span>

<span data-ttu-id="7f472-219">Echte Literale werden verwendet, um Werte der Typen `float`, `double`und `decimal`zu schreiben.</span><span class="sxs-lookup"><span data-stu-id="7f472-219">Real literals are used to write values of types `float`, `double`, and `decimal`.</span></span>

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

<span data-ttu-id="7f472-220">Wenn *real_type_suffix* nicht angegeben wird, ist der Typ des realen Literals `double`.</span><span class="sxs-lookup"><span data-stu-id="7f472-220">If no *real_type_suffix* is specified, the type of the real literal is `double`.</span></span> <span data-ttu-id="7f472-221">Andernfalls bestimmt das echte Typsuffix den Typ des echten Literals wie folgt:</span><span class="sxs-lookup"><span data-stu-id="7f472-221">Otherwise, the real type suffix determines the type of the real literal, as follows:</span></span>

*  <span data-ttu-id="7f472-222">Ein echtes LiteralSuffix, `F` das `f` von oder ist `float`, ist vom Typ.</span><span class="sxs-lookup"><span data-stu-id="7f472-222">A real literal suffixed by `F` or `f` is of type `float`.</span></span> <span data-ttu-id="7f472-223">Die `1f` `1.5f`Literale `float`,, und `123.456F` sind z. b. vom Typ. `1e10f`</span><span class="sxs-lookup"><span data-stu-id="7f472-223">For example, the literals `1f`, `1.5f`, `1e10f`, and `123.456F` are all of type `float`.</span></span>
*  <span data-ttu-id="7f472-224">Ein echtes LiteralSuffix, `D` das `d` von oder ist `double`, ist vom Typ.</span><span class="sxs-lookup"><span data-stu-id="7f472-224">A real literal suffixed by `D` or `d` is of type `double`.</span></span> <span data-ttu-id="7f472-225">Die `1d` `1.5d`Literale `double`,, und `123.456D` sind z. b. vom Typ. `1e10d`</span><span class="sxs-lookup"><span data-stu-id="7f472-225">For example, the literals `1d`, `1.5d`, `1e10d`, and `123.456D` are all of type `double`.</span></span>
*  <span data-ttu-id="7f472-226">Ein echtes LiteralSuffix, `M` das `m` von oder ist `decimal`, ist vom Typ.</span><span class="sxs-lookup"><span data-stu-id="7f472-226">A real literal suffixed by `M` or `m` is of type `decimal`.</span></span> <span data-ttu-id="7f472-227">Die `1m` `1.5m`Literale `decimal`,, und `123.456M` sind z. b. vom Typ. `1e10m`</span><span class="sxs-lookup"><span data-stu-id="7f472-227">For example, the literals `1m`, `1.5m`, `1e10m`, and `123.456M` are all of type `decimal`.</span></span> <span data-ttu-id="7f472-228">Diese Literale werden in einen `decimal` -Wert konvertiert, indem der genaue Wert verwendet wird, und, falls erforderlich, auf den nächstgelegenen darstellbaren Wert mit der-Rundung ([dem Decimal-Typ](types.md#the-decimal-type)) gerundet.</span><span class="sxs-lookup"><span data-stu-id="7f472-228">This literal is converted to a `decimal` value by taking the exact value, and, if necessary, rounding to the nearest representable value using banker's rounding ([The decimal type](types.md#the-decimal-type)).</span></span> <span data-ttu-id="7f472-229">Alle in der Literale sichtbaren Skalierungen bleiben erhalten, es sei denn, der Wert ist gerundet, oder der Wert ist NULL (in letzterem Fall ist das Vorzeichen und die Dezimalstellen 0).</span><span class="sxs-lookup"><span data-stu-id="7f472-229">Any scale apparent in the literal is preserved unless the value is rounded or the value is zero (in which latter case the sign and scale will be 0).</span></span> <span data-ttu-id="7f472-230">Daher wird das Literale `2.900m` analysiert, um das Dezimaltrennzeichen `0`, den Koeffizienten `2900`und die Skala `3`zu bilden.</span><span class="sxs-lookup"><span data-stu-id="7f472-230">Hence, the literal `2.900m` will be parsed to form the decimal with sign `0`, coefficient `2900`, and scale `3`.</span></span>

<span data-ttu-id="7f472-231">Wenn das angegebene Literale nicht im angegebenen Typ dargestellt werden kann, tritt ein Kompilierzeitfehler auf.</span><span class="sxs-lookup"><span data-stu-id="7f472-231">If the specified literal cannot be represented in the indicated type, a compile-time error occurs.</span></span>

<span data-ttu-id="7f472-232">Der Wert eines echten Literals vom Typ `float` oder `double` wird mit dem IEEE-Modus "Round to Next" bestimmt.</span><span class="sxs-lookup"><span data-stu-id="7f472-232">The value of a real literal of type `float` or `double` is determined by using the IEEE "round to nearest" mode.</span></span>

<span data-ttu-id="7f472-233">Beachten Sie, dass in einem echten Literalzeichen nach dem Dezimaltrennzeichen immer Dezimalstellen erforderlich sind.</span><span class="sxs-lookup"><span data-stu-id="7f472-233">Note that in a real literal, decimal digits are always required after the decimal point.</span></span> <span data-ttu-id="7f472-234">Beispielsweise ist `1.3F` ein echtes Literalzeichen `1.F` , aber nicht.</span><span class="sxs-lookup"><span data-stu-id="7f472-234">For example, `1.3F` is a real literal but `1.F` is not.</span></span>

#### <a name="character-literals"></a><span data-ttu-id="7f472-235">Zeichenliterale</span><span class="sxs-lookup"><span data-stu-id="7f472-235">Character literals</span></span>

<span data-ttu-id="7f472-236">Ein Zeichenliteral stellt ein einzelnes Zeichen dar und besteht normalerweise aus einem Zeichen in Anführungs `'a'`Zeichen, wie in.</span><span class="sxs-lookup"><span data-stu-id="7f472-236">A character literal represents a single character, and usually consists of a character in quotes, as in `'a'`.</span></span>

<span data-ttu-id="7f472-237">Hinweis: Die Grammatik-Notation von antlr macht folgendes verwirrend!</span><span class="sxs-lookup"><span data-stu-id="7f472-237">Note: The ANTLR grammar notation makes the following confusing!</span></span> <span data-ttu-id="7f472-238">Wenn Sie in antlr schreiben `\'` , steht es für ein einzelnes Anführungs `'`Zeichen.</span><span class="sxs-lookup"><span data-stu-id="7f472-238">In ANTLR, when you write `\'` it stands for a single quote `'`.</span></span> <span data-ttu-id="7f472-239">Und wenn Sie schreiben `\\` , steht ein einzelner umgekehrter schräg `\`Strich.</span><span class="sxs-lookup"><span data-stu-id="7f472-239">And when you write `\\` it stands for a single backslash `\`.</span></span> <span data-ttu-id="7f472-240">Daher bedeutet die erste Regel für ein Zeichenliteral, dass Sie mit einem einfachen Anführungszeichen, einem Zeichen und einem einfachen Anführungszeichen beginnt.</span><span class="sxs-lookup"><span data-stu-id="7f472-240">Therefore the first rule for a character literal means it starts with a single quote, then a character, then a single quote.</span></span> <span data-ttu-id="7f472-241">Und die elf möglichen einfachen Escapesequenzen `\"`sind `\\` `\'`, `\0`, `\a`, `\b`, `\f`, `\n`, `\r`, `\t`,, ,`\v`.</span><span class="sxs-lookup"><span data-stu-id="7f472-241">And the eleven possible simple escape sequences are `\'`, `\"`, `\\`, `\0`, `\a`, `\b`, `\f`, `\n`, `\r`, `\t`, `\v`.</span></span>

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

<span data-ttu-id="7f472-242">Ein Zeichen, das einem umgekehrten Schrägstrich (`\`) in einem *Zeichen* folgt, muss eines der folgenden Zeichen sein `'`: `"`, `\`, `0`, `a`, `b`, `f` , , `n`, `r`, `t`, `u`, `U`, `x`, `v`.</span><span class="sxs-lookup"><span data-stu-id="7f472-242">A character that follows a backslash character (`\`) in a *character* must be one of the following characters: `'`, `"`, `\`, `0`, `a`, `b`, `f`, `n`, `r`, `t`, `u`, `U`, `x`, `v`.</span></span> <span data-ttu-id="7f472-243">Andernfalls tritt ein Kompilierungsfehler auf.</span><span class="sxs-lookup"><span data-stu-id="7f472-243">Otherwise, a compile-time error occurs.</span></span>

<span data-ttu-id="7f472-244">Eine hexadezimale Escapesequenz stellt ein einzelnes Unicode-Zeichen dar, wobei der Wert durch die hexadezimale Zahl nach "`\x`" gebildet wird.</span><span class="sxs-lookup"><span data-stu-id="7f472-244">A hexadecimal escape sequence represents a single Unicode character, with the value formed by the hexadecimal number following "`\x`".</span></span>

<span data-ttu-id="7f472-245">Wenn der Wert, der von einem Zeichenliterals `U+FFFF`dargestellt wird, größer als ist, tritt ein Kompilierzeitfehler auf.</span><span class="sxs-lookup"><span data-stu-id="7f472-245">If the value represented by a character literal is greater than `U+FFFF`, a compile-time error occurs.</span></span>

<span data-ttu-id="7f472-246">Eine Unicode-Escapesequenz (Escapesequenzen für[Unicode-Zeichen](lexical-structure.md#unicode-character-escape-sequences)) in einem Zeichenliterals muss im Bereich `U+0000` von `U+FFFF`liegen.</span><span class="sxs-lookup"><span data-stu-id="7f472-246">A Unicode character escape sequence ([Unicode character escape sequences](lexical-structure.md#unicode-character-escape-sequences)) in a character literal must be in the range `U+0000` to `U+FFFF`.</span></span>

<span data-ttu-id="7f472-247">Eine einfache Escapesequenz stellt eine Unicode-Zeichencodierung dar, wie in der folgenden Tabelle beschrieben.</span><span class="sxs-lookup"><span data-stu-id="7f472-247">A simple escape sequence represents a Unicode character encoding, as described in the table below.</span></span>


| <span data-ttu-id="7f472-248">__Escapesequenz__</span><span class="sxs-lookup"><span data-stu-id="7f472-248">__Escape sequence__</span></span> | <span data-ttu-id="7f472-249">__Zeichen Name__</span><span class="sxs-lookup"><span data-stu-id="7f472-249">__Character name__</span></span> | <span data-ttu-id="7f472-250">__Unicode-Codierung__</span><span class="sxs-lookup"><span data-stu-id="7f472-250">__Unicode encoding__</span></span> |
|---------------------|--------------------|----------------------|
| `\'`                | <span data-ttu-id="7f472-251">Einfaches Anführungszeichen</span><span class="sxs-lookup"><span data-stu-id="7f472-251">Single quote</span></span>       | `0x0027`             | 
| `\"`                | <span data-ttu-id="7f472-252">Doppeltes Anführungszeichen</span><span class="sxs-lookup"><span data-stu-id="7f472-252">Double quote</span></span>       | `0x0022`             | 
| `\\`                | <span data-ttu-id="7f472-253">Umgekehrter Schrägstrich</span><span class="sxs-lookup"><span data-stu-id="7f472-253">Backslash</span></span>          | `0x005C`             | 
| `\0`                | <span data-ttu-id="7f472-254">NULL</span><span class="sxs-lookup"><span data-stu-id="7f472-254">Null</span></span>               | `0x0000`             | 
| `\a`                | <span data-ttu-id="7f472-255">Warnung</span><span class="sxs-lookup"><span data-stu-id="7f472-255">Alert</span></span>              | `0x0007`             | 
| `\b`                | <span data-ttu-id="7f472-256">Rückschritt</span><span class="sxs-lookup"><span data-stu-id="7f472-256">Backspace</span></span>          | `0x0008`             | 
| `\f`                | <span data-ttu-id="7f472-257">Seitenvorschub</span><span class="sxs-lookup"><span data-stu-id="7f472-257">Form feed</span></span>          | `0x000C`             | 
| `\n`                | <span data-ttu-id="7f472-258">Zeilenwechsel</span><span class="sxs-lookup"><span data-stu-id="7f472-258">New line</span></span>           | `0x000A`             | 
| `\r`                | <span data-ttu-id="7f472-259">Wagenrücklauf</span><span class="sxs-lookup"><span data-stu-id="7f472-259">Carriage return</span></span>    | `0x000D`             | 
| `\t`                | <span data-ttu-id="7f472-260">Horizontaler Tabulator</span><span class="sxs-lookup"><span data-stu-id="7f472-260">Horizontal tab</span></span>     | `0x0009`             | 
| `\v`                | <span data-ttu-id="7f472-261">Vertikaler Tabulator</span><span class="sxs-lookup"><span data-stu-id="7f472-261">Vertical tab</span></span>       | `0x000B`             | 

<span data-ttu-id="7f472-262">Der Typ eines *character_literal* ist `char`.</span><span class="sxs-lookup"><span data-stu-id="7f472-262">The type of a *character_literal* is `char`.</span></span>

#### <a name="string-literals"></a><span data-ttu-id="7f472-263">Zeichenfolgenliterale</span><span class="sxs-lookup"><span data-stu-id="7f472-263">String literals</span></span>

<span data-ttu-id="7f472-264">C#unterstützt zwei Formen von Zeichenfolgenliteralen: ***reguläre Zeichen folgen Literale*** und ***ausführliche Zeichen folgen Literale***.</span><span class="sxs-lookup"><span data-stu-id="7f472-264">C# supports two forms of string literals: ***regular string literals*** and ***verbatim string literals***.</span></span>

<span data-ttu-id="7f472-265">Ein reguläres Zeichenfolgenliterale besteht aus null oder mehr Zeichen, die `"hello"`in doppelten Anführungszeichen eingeschlossen sind, wie z. b `\t` ., und kann sowohl einfache Escapesequenzen (z. b. für das Tabstopp Zeichen) als auch hexadezimale und</span><span class="sxs-lookup"><span data-stu-id="7f472-265">A regular string literal consists of zero or more characters enclosed in double quotes, as in `"hello"`, and may include both simple escape sequences (such as `\t` for the tab character), and hexadecimal and Unicode escape sequences.</span></span>

<span data-ttu-id="7f472-266">Ein ausführlichen Zeichenfolgenliterals besteht aus einem `@` Zeichen, gefolgt von einem doppelten Anführungszeichen, NULL oder mehr Zeichen und einem schließenden doppelten Anführungszeichen.</span><span class="sxs-lookup"><span data-stu-id="7f472-266">A verbatim string literal consists of an `@` character followed by a double-quote character, zero or more characters, and a closing double-quote character.</span></span> <span data-ttu-id="7f472-267">Ein einfaches Beispiel ist `@"hello"`.</span><span class="sxs-lookup"><span data-stu-id="7f472-267">A simple example is `@"hello"`.</span></span> <span data-ttu-id="7f472-268">In einem ausführlichen zeichenfolgenliteralliteralen werden die Zeichen zwischen den Trennzeichen wörtlich interpretiert, die einzige Ausnahme ist eine *quote_escape_sequence*.</span><span class="sxs-lookup"><span data-stu-id="7f472-268">In a verbatim string literal, the characters between the delimiters are interpreted verbatim, the only exception being a *quote_escape_sequence*.</span></span> <span data-ttu-id="7f472-269">Insbesondere einfache Escapesequenzen und hexadezimale und Unicode-Escapesequenzen werden in ausführlichen Zeichenfolgenliteralen nicht verarbeitet.</span><span class="sxs-lookup"><span data-stu-id="7f472-269">In particular, simple escape sequences, and hexadecimal and Unicode escape sequences are not processed in verbatim string literals.</span></span> <span data-ttu-id="7f472-270">Ein ausführlicher zeichenfolgenliteralvorgang kann mehrere Zeilen umfassen.</span><span class="sxs-lookup"><span data-stu-id="7f472-270">A verbatim string literal may span multiple lines.</span></span>

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

<span data-ttu-id="7f472-271">Ein Zeichen, das auf einen umgekehrten Schrägstrich (`\`) in einem *regular_string_literal_character* folgt, muss eines der folgenden Zeichen sein: `'`, `"`, `\`, `0`, `a`, `b`, `f`, `n`, 0, 1, @no__ t-12, 3, 4, 5.</span><span class="sxs-lookup"><span data-stu-id="7f472-271">A character that follows a backslash character (`\`) in a *regular_string_literal_character* must be one of the following characters: `'`, `"`, `\`, `0`, `a`, `b`, `f`, `n`, `r`, `t`, `u`, `U`, `x`, `v`.</span></span> <span data-ttu-id="7f472-272">Andernfalls tritt ein Kompilierungsfehler auf.</span><span class="sxs-lookup"><span data-stu-id="7f472-272">Otherwise, a compile-time error occurs.</span></span>

<span data-ttu-id="7f472-273">Das Beispiel</span><span class="sxs-lookup"><span data-stu-id="7f472-273">The example</span></span>
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
<span data-ttu-id="7f472-274">zeigt eine Vielzahl von Zeichenfolgenliteralen an.</span><span class="sxs-lookup"><span data-stu-id="7f472-274">shows a variety of string literals.</span></span> <span data-ttu-id="7f472-275">Das letzte Zeichenfolgenliterale, `j`, ist ein ausführlicher Zeichenfolgenliteral, das mehrere Zeilen</span><span class="sxs-lookup"><span data-stu-id="7f472-275">The last string literal, `j`, is a verbatim string literal that spans multiple lines.</span></span> <span data-ttu-id="7f472-276">Die Zeichen zwischen den Anführungszeichen, einschließlich Leerzeichen, z. b. neue Zeilenzeichen, werden wörtlich beibehalten.</span><span class="sxs-lookup"><span data-stu-id="7f472-276">The characters between the quotation marks, including white space such as new line characters, are preserved verbatim.</span></span>

<span data-ttu-id="7f472-277">Da eine hexadezimale Escapesequenz eine Variable Anzahl von hexadezimalen Ziffern `"\x123"` aufweisen kann, enthält das Zeichenfolgenliterale ein einzelnes Zeichen 123 mit dem hexadezi</span><span class="sxs-lookup"><span data-stu-id="7f472-277">Since a hexadecimal escape sequence can have a variable number of hex digits, the string literal `"\x123"` contains a single character with hex value 123.</span></span> <span data-ttu-id="7f472-278">Um eine Zeichenfolge zu erstellen, die das Zeichen mit dem Hexadezimalwert 12 gefolgt vom `"\x00123"` Zeichen `"\x12" + "3"` 3 enthält, könnte ein oder stattdessen geschrieben werden.</span><span class="sxs-lookup"><span data-stu-id="7f472-278">To create a string containing the character with hex value 12 followed by the character 3, one could write `"\x00123"` or `"\x12" + "3"` instead.</span></span>

<span data-ttu-id="7f472-279">Der Typ eines *string_literal* ist `string`.</span><span class="sxs-lookup"><span data-stu-id="7f472-279">The type of a *string_literal* is `string`.</span></span>

<span data-ttu-id="7f472-280">Jedes Zeichenfolgenliterale führt nicht unbedingt zu einer neuen Zeichen folgen Instanz.</span><span class="sxs-lookup"><span data-stu-id="7f472-280">Each string literal does not necessarily result in a new string instance.</span></span> <span data-ttu-id="7f472-281">Wenn zwei oder mehr Zeichen folgen Literale, die gemäß dem Zeichen folgen Gleichheits Operator ([Zeichen folgen Gleichheits Operatoren](expressions.md#string-equality-operators)) gleichwertig sind, im selben Programm vorkommen, verweisen diese Zeichen folgen Literale auf dieselbe Zeichen folgen Instanz.</span><span class="sxs-lookup"><span data-stu-id="7f472-281">When two or more string literals that are equivalent according to the string equality operator ([String equality operators](expressions.md#string-equality-operators)) appear in the same program, these string literals refer to the same string instance.</span></span> <span data-ttu-id="7f472-282">Beispielsweise die Ausgabe, die von</span><span class="sxs-lookup"><span data-stu-id="7f472-282">For instance, the output produced by</span></span>
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
<span data-ttu-id="7f472-283">liegt `True` daran, dass die beiden Literale auf dieselbe Zeichen folgen Instanz verweisen.</span><span class="sxs-lookup"><span data-stu-id="7f472-283">is `True` because the two literals refer to the same string instance.</span></span>

#### <a name="interpolated-string-literals"></a><span data-ttu-id="7f472-284">Interpoliert Zeichen folgen Literale</span><span class="sxs-lookup"><span data-stu-id="7f472-284">Interpolated string literals</span></span>

<span data-ttu-id="7f472-285">Interinterpolierte Zeichen folgen Literale ähneln Zeichen folgen Literalen, enthalten jedoch Lücken, die `{` durch `}`und getrennt sind, wobei Ausdrücke auftreten können.</span><span class="sxs-lookup"><span data-stu-id="7f472-285">Interpolated string literals are similar to string literals, but contain holes delimited by `{` and `}`, wherein expressions can occur.</span></span> <span data-ttu-id="7f472-286">Zur Laufzeit werden die Ausdrücke so ausgewertet, dass Ihre Text Formulare in der Zeichenfolge an der Stelle, an der die Lücke auftritt, ersetzt werden.</span><span class="sxs-lookup"><span data-stu-id="7f472-286">At runtime, the expressions are evaluated with the purpose of having their textual forms substituted into the string at the place where the hole occurs.</span></span> <span data-ttu-id="7f472-287">Die Syntax und die Semantik der Zeichen folgen Interpolationen werden im Abschnitt ([interpoliert](expressions.md#interpolated-strings)Zeichen folgen) beschrieben.</span><span class="sxs-lookup"><span data-stu-id="7f472-287">The syntax and semantics of string interpolation are described in section ([Interpolated strings](expressions.md#interpolated-strings)).</span></span>

<span data-ttu-id="7f472-288">Wie bei Zeichenfolgenliteralen können interpoliert Zeichen folgen Literale entweder regulär oder wörtlich sein.</span><span class="sxs-lookup"><span data-stu-id="7f472-288">Like string literals, interpolated string literals can be either regular or verbatim.</span></span> <span data-ttu-id="7f472-289">Interpoliert reguläre Zeichenfolgenliterale werden `$"` durch `"`und getrennt, und interpoliert, ausführliche Zeichen folgen Literale `$@"` werden `"`durch und getrennt.</span><span class="sxs-lookup"><span data-stu-id="7f472-289">Interpolated regular string literals are delimited by `$"` and `"`, and interpolated verbatim string literals are delimited by `$@"` and `"`.</span></span>

<span data-ttu-id="7f472-290">Wie bei anderen literalen ergibt die lexikalische Analyse eines interpoliert für interpoliert zunächst ein einzelnes Token, wie gemäß der Grammatik unten.</span><span class="sxs-lookup"><span data-stu-id="7f472-290">Like other literals, lexical analysis of an interpolated string literal initially results in a single token, as per the grammar below.</span></span> <span data-ttu-id="7f472-291">Vor der syntaktischen Analyse wird jedoch das einzelne Token eines interpoliert-Zeichenfolgenliterals in mehrere Token für die Teile der Zeichenfolge aufgeteilt, die die Löcher einschließen, und die in den Löchern auftretenden Eingabeelemente werden lexikalisch erneut analysiert.</span><span class="sxs-lookup"><span data-stu-id="7f472-291">However, before syntactic analysis, the single token of an interpolated string literal is broken into several tokens for the parts of the string enclosing the holes, and the input elements occurring in the holes are lexically analysed again.</span></span> <span data-ttu-id="7f472-292">Dies kann wiederum dazu führen, dass mehr interinterpolierte Zeichenfolgenliterale verarbeitet werden, die verarbeitet werden können, aber wenn lexikalisch korrekt ist, führt letztendlich zu einer Sequenz von Tokens, damit die syntaktische Analyse verarbeitet wird.</span><span class="sxs-lookup"><span data-stu-id="7f472-292">This may in turn produce more interpolated string literals to be processed, but, if lexically correct, will eventually lead to a sequence of tokens for syntactic analysis to process.</span></span>

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

<span data-ttu-id="7f472-293">Ein *interpolated_string_literal* -Token wird wie folgt als mehrere Token und andere Eingabeelemente in der Reihenfolge des Auftretens in *interpolated_string_literal*neu interpretiert:</span><span class="sxs-lookup"><span data-stu-id="7f472-293">An *interpolated_string_literal* token is reinterpreted as multiple tokens and other input elements as follows, in order of occurrence in the *interpolated_string_literal*:</span></span>

* <span data-ttu-id="7f472-294">Vorkommen der folgenden werden als separate einzelne Token neu interpretiert: das führende `$`-Vorzeichen, *interpolated_regular_string_whole*, *interpolated_regular_string_start*, *interpolated_regular_string_mid*,  *interpolated_regular_string_end*, *interpolated_verbatim_string_whole*, *interpolated_verbatim_string_start*, *interpolated_verbatim_string_mid* und *interpolated_verbatim_string_end*.</span><span class="sxs-lookup"><span data-stu-id="7f472-294">Occurrences of the following are reinterpreted as separate individual tokens: the leading `$` sign, *interpolated_regular_string_whole*, *interpolated_regular_string_start*, *interpolated_regular_string_mid*, *interpolated_regular_string_end*, *interpolated_verbatim_string_whole*, *interpolated_verbatim_string_start*, *interpolated_verbatim_string_mid* and *interpolated_verbatim_string_end*.</span></span>
* <span data-ttu-id="7f472-295">Vorkommen von *regular_balanced_text* und *verbatim_balanced_text* dazwischen werden als *input_section* ([lexikalische Analyse](lexical-structure.md#lexical-analysis)) neu verarbeitet und als resultierende Sequenz von Eingabe Elementen interpretiert.</span><span class="sxs-lookup"><span data-stu-id="7f472-295">Occurrences of *regular_balanced_text* and *verbatim_balanced_text* between these are reprocessed as an *input_section* ([Lexical analysis](lexical-structure.md#lexical-analysis)) and are reinterpreted as the resulting sequence of input elements.</span></span> <span data-ttu-id="7f472-296">Diese können wiederum interinterpolierte zeichenfolgenliteraltoken enthalten, die neu interpretiert werden.</span><span class="sxs-lookup"><span data-stu-id="7f472-296">These may in turn include interpolated string literal tokens to be reinterpreted.</span></span>

<span data-ttu-id="7f472-297">Die syntaktische Analyse kombiniert die Token in einer *interpolated_string_expression* ([interpoliert](expressions.md#interpolated-strings)) neu.</span><span class="sxs-lookup"><span data-stu-id="7f472-297">Syntactic analysis will recombine the tokens into an *interpolated_string_expression* ([Interpolated strings](expressions.md#interpolated-strings)).</span></span>

<span data-ttu-id="7f472-298">Beispiele TODO</span><span class="sxs-lookup"><span data-stu-id="7f472-298">Examples TODO</span></span>


#### <a name="the-null-literal"></a><span data-ttu-id="7f472-299">Das NULL-Literale</span><span class="sxs-lookup"><span data-stu-id="7f472-299">The null literal</span></span>

```antlr
null_literal
    : 'null'
    ;
```

<span data-ttu-id="7f472-300">*Null_literal* kann implizit in einen Verweistyp oder einen Typ konvertiert werden, der NULL-Werte zulässt.</span><span class="sxs-lookup"><span data-stu-id="7f472-300">The  *null_literal* can be implicitly converted to a reference type or nullable type.</span></span>

### <a name="operators-and-punctuators"></a><span data-ttu-id="7f472-301">Operatoren und Satzzeichen</span><span class="sxs-lookup"><span data-stu-id="7f472-301">Operators and punctuators</span></span>

<span data-ttu-id="7f472-302">Es gibt mehrere Arten von Operatoren und Satzzeichen.</span><span class="sxs-lookup"><span data-stu-id="7f472-302">There are several kinds of operators and punctuators.</span></span> <span data-ttu-id="7f472-303">Operatoren werden in Ausdrücken verwendet, um Vorgänge mit einem oder mehreren Operanden zu beschreiben.</span><span class="sxs-lookup"><span data-stu-id="7f472-303">Operators are used in expressions to describe operations involving one or more operands.</span></span> <span data-ttu-id="7f472-304">Der-Ausdruck `a + b` verwendet beispielsweise den `+` -Operator, um die beiden Operanden `b` `a` und hinzuzufügen.</span><span class="sxs-lookup"><span data-stu-id="7f472-304">For example, the expression `a + b` uses the `+` operator to add the two operands `a` and `b`.</span></span> <span data-ttu-id="7f472-305">Satzzeichen sind zum Gruppieren und trennen vorgesehen.</span><span class="sxs-lookup"><span data-stu-id="7f472-305">Punctuators are for grouping and separating.</span></span>

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

<span data-ttu-id="7f472-306">Der senkrechte Strich in den *right_shift* -und *right_shift_assignment* -Produktionen wird verwendet, um anzugeben, dass im Gegensatz zu anderen Produktionen in der syntaktischen Grammatik keine Zeichen jeglicher Art (nicht sogar Leerzeichen) zwischen den Token zulässig sind.</span><span class="sxs-lookup"><span data-stu-id="7f472-306">The vertical bar in the *right_shift* and *right_shift_assignment* productions are used to indicate that, unlike other productions in the syntactic grammar, no characters of any kind (not even whitespace) are allowed between the tokens.</span></span> <span data-ttu-id="7f472-307">Diese Produktionen werden speziell behandelt, um die korrekte Handhabung von *type_parameter_list*s ([Typparameter](classes.md#type-parameters)) zu ermöglichen.</span><span class="sxs-lookup"><span data-stu-id="7f472-307">These productions are treated specially in order to enable the correct  handling of *type_parameter_list*s ([Type parameters](classes.md#type-parameters)).</span></span>

## <a name="pre-processing-directives"></a><span data-ttu-id="7f472-308">Vorverarbeitungs Direktiven</span><span class="sxs-lookup"><span data-stu-id="7f472-308">Pre-processing directives</span></span>

<span data-ttu-id="7f472-309">Die Vorverarbeitungs Direktiven bieten die Möglichkeit, Abschnitte von Quelldateien bedingt zu überspringen, Fehler-und Warnungs Bedingungen zu melden und verschiedene Bereiche des Quellcodes zu gliedern.</span><span class="sxs-lookup"><span data-stu-id="7f472-309">The pre-processing directives provide the ability to conditionally skip sections of source files, to report error and warning conditions, and to delineate distinct regions of source code.</span></span> <span data-ttu-id="7f472-310">Der Begriff "Vorverarbeitungs Direktiven" wird nur aus Gründen der Konsistenz mit C- C++ und Programmiersprachen verwendet.</span><span class="sxs-lookup"><span data-stu-id="7f472-310">The term "pre-processing directives" is used only for consistency with the C and C++ programming languages.</span></span> <span data-ttu-id="7f472-311">In C#gibt es keinen separaten Schritt vor der Verarbeitung. Vorverarbeitungs Direktiven werden im Rahmen der lexikalischen Analysephase verarbeitet.</span><span class="sxs-lookup"><span data-stu-id="7f472-311">In C#, there is no separate pre-processing step; pre-processing directives are processed as part of the lexical analysis phase.</span></span>

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

<span data-ttu-id="7f472-312">Die folgenden Vorverarbeitungs Direktiven sind verfügbar:</span><span class="sxs-lookup"><span data-stu-id="7f472-312">The following pre-processing directives are available:</span></span>

*  <span data-ttu-id="7f472-313">`#define`und `#undef`, die zum definieren bzw. Aufheben der Definition von Symbolen für bedingte Kompilierung ([Deklarations Anweisungen](lexical-structure.md#declaration-directives)) verwendet werden.</span><span class="sxs-lookup"><span data-stu-id="7f472-313">`#define` and `#undef`, which are used to define and undefine, respectively, conditional compilation symbols ([Declaration directives](lexical-structure.md#declaration-directives)).</span></span>
*  <span data-ttu-id="7f472-314">`#if`, `#elif`, `#else` und`#endif`, die verwendet werden, um Abschnitte des Quellcodes bedingt zu überspringen ([bedingte Kompilierungs Direktiven](lexical-structure.md#conditional-compilation-directives)).</span><span class="sxs-lookup"><span data-stu-id="7f472-314">`#if`, `#elif`, `#else`, and `#endif`, which are used to conditionally skip sections of source code ([Conditional compilation directives](lexical-structure.md#conditional-compilation-directives)).</span></span>
*  <span data-ttu-id="7f472-315">`#line`, der verwendet wird, um Zeilennummern zu steuern, die für Fehler und Warnungen ausgegeben werden ([Zeilen Anweisungen](lexical-structure.md#line-directives)).</span><span class="sxs-lookup"><span data-stu-id="7f472-315">`#line`, which is used to control line numbers emitted for errors and warnings ([Line directives](lexical-structure.md#line-directives)).</span></span>
*  <span data-ttu-id="7f472-316">`#error`und `#warning`, die verwendet werden, um Fehler und Warnungen bzw. ([Diagnose Direktiven](lexical-structure.md#diagnostic-directives)) auszugeben.</span><span class="sxs-lookup"><span data-stu-id="7f472-316">`#error` and `#warning`, which are used to issue errors and warnings, respectively ([Diagnostic directives](lexical-structure.md#diagnostic-directives)).</span></span>
*  <span data-ttu-id="7f472-317">`#region`und `#endregion`, die verwendet werden, um Abschnitte des Quellcodes ([Regions Direktiven](lexical-structure.md#region-directives)) explizit zu markieren.</span><span class="sxs-lookup"><span data-stu-id="7f472-317">`#region` and `#endregion`, which are used to explicitly mark sections of source code ([Region directives](lexical-structure.md#region-directives)).</span></span>
*  <span data-ttu-id="7f472-318">`#pragma`, der verwendet wird, um optionale Kontextinformationen für den Compiler anzugeben ([pragma-Direktiven](lexical-structure.md#pragma-directives)).</span><span class="sxs-lookup"><span data-stu-id="7f472-318">`#pragma`, which is used to specify optional contextual information to the compiler ([Pragma directives](lexical-structure.md#pragma-directives)).</span></span>

<span data-ttu-id="7f472-319">Eine Vorverarbeitungs Direktive belegt immer eine separate Zeile des Quellcodes und beginnt immer mit einem `#` Zeichen und einem Vorverarbeitungs-Direktivennamen.</span><span class="sxs-lookup"><span data-stu-id="7f472-319">A pre-processing directive always occupies a separate line of source code and always begins with a `#` character and a pre-processing directive name.</span></span> <span data-ttu-id="7f472-320">Leerräume können vor dem `#` Zeichen und zwischen dem `#` Zeichen und dem Direktivennamen auftreten.</span><span class="sxs-lookup"><span data-stu-id="7f472-320">White space may occur before the `#` character and between the `#` character and the directive name.</span></span>

<span data-ttu-id="7f472-321">Eine Quellzeile `#define` `#elif` `#line` `#endif` `#undef` `#else` ,die`#endregion` eine-,-,-,-,-,-oder-Direktive enthält, kannmiteinemeinzeiligenKommentarenden.`#if`</span><span class="sxs-lookup"><span data-stu-id="7f472-321">A source line containing a `#define`, `#undef`, `#if`, `#elif`, `#else`, `#endif`, `#line`, or `#endregion` directive may end with a single-line comment.</span></span> <span data-ttu-id="7f472-322">Durch Trennzeichen getrennte Kommentare `/* */` (der Stil von Kommentaren) sind in Quellzeilen mit Vorverarbeitungs Direktiven nicht zulässig.</span><span class="sxs-lookup"><span data-stu-id="7f472-322">Delimited comments (the `/* */` style of comments) are not permitted on source lines containing pre-processing directives.</span></span>

<span data-ttu-id="7f472-323">Vorverarbeitungs Direktiven sind keine Token und sind nicht Teil der syntaktischen Grammatik von C#.</span><span class="sxs-lookup"><span data-stu-id="7f472-323">Pre-processing directives are not tokens and are not part of the syntactic grammar of C#.</span></span> <span data-ttu-id="7f472-324">Allerdings können Vorverarbeitungs Direktiven verwendet werden, um Sequenzen von Token einzuschließen oder auszuschließen, und sich auf diese Weise auf die C# Bedeutung eines Programms auswirken.</span><span class="sxs-lookup"><span data-stu-id="7f472-324">However, pre-processing directives can be used to include or exclude sequences of tokens and can in that way affect the meaning of a C# program.</span></span> <span data-ttu-id="7f472-325">Bei der Kompilierung wird das Programm z. b.:</span><span class="sxs-lookup"><span data-stu-id="7f472-325">For example, when compiled, the program:</span></span>
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
<span data-ttu-id="7f472-326">ergibt genau dieselbe Sequenz von Token wie das Programm:</span><span class="sxs-lookup"><span data-stu-id="7f472-326">results in the exact same sequence of tokens as the program:</span></span>
```csharp
class C
{
    void F() {}
    void I() {}
}
```

<span data-ttu-id="7f472-327">Im Gegensatz dazu sind die beiden Programme ganz anders, syntaktisch, Sie sind jedoch identisch.</span><span class="sxs-lookup"><span data-stu-id="7f472-327">Thus, whereas lexically, the two programs are quite different, syntactically, they are identical.</span></span>

### <a name="conditional-compilation-symbols"></a><span data-ttu-id="7f472-328">Symbole für bedingte Kompilierung</span><span class="sxs-lookup"><span data-stu-id="7f472-328">Conditional compilation symbols</span></span>

<span data-ttu-id="7f472-329">Die Funktionen für die bedingte Kompilierung `#if`, die `#else`von den `#endif` Direktiven,, und bereitgestellt werden, `#elif`werden durch Vorverarbeitungs Ausdrücke ([Vorverarbeitungs Ausdrücke](lexical-structure.md#pre-processing-expressions)) und bedingt gesteuert. Kompilierungs Symbole.</span><span class="sxs-lookup"><span data-stu-id="7f472-329">The conditional compilation functionality provided by the `#if`, `#elif`, `#else`, and `#endif` directives is controlled through pre-processing expressions ([Pre-processing expressions](lexical-structure.md#pre-processing-expressions)) and conditional compilation symbols.</span></span>

```antlr
conditional_symbol
    : '<Any identifier_or_keyword except true or false>'
    ;
```

<span data-ttu-id="7f472-330">Ein Symbol für die bedingte Kompilierung hat zwei mögliche Zustände: ***definiert*** oder nicht ***definiert***.</span><span class="sxs-lookup"><span data-stu-id="7f472-330">A conditional compilation symbol has two possible states: ***defined*** or ***undefined***.</span></span> <span data-ttu-id="7f472-331">Am Anfang der lexikalischen Verarbeitung einer Quelldatei ist ein Symbol für die bedingte Kompilierung nicht definiert, es sei denn, es wurde explizit durch einen externen Mechanismus definiert (z. b. eine Befehlszeilen-Compileroption).</span><span class="sxs-lookup"><span data-stu-id="7f472-331">At the beginning of the lexical processing of a source file, a conditional compilation symbol is undefined unless it has been explicitly defined by an external mechanism (such as a command-line compiler option).</span></span> <span data-ttu-id="7f472-332">Wenn eine `#define` Direktive verarbeitet wird, wird das in dieser Direktive benannte bedingte Kompilierungs Symbol in dieser Quelldatei definiert.</span><span class="sxs-lookup"><span data-stu-id="7f472-332">When a `#define` directive is processed, the conditional compilation symbol named in that directive becomes defined in that source file.</span></span> <span data-ttu-id="7f472-333">Das Symbol bleibt so lange definiert `#undef` , bis eine-Direktive für das gleiche Symbol verarbeitet wird oder bis das Ende der Quelldatei erreicht wird.</span><span class="sxs-lookup"><span data-stu-id="7f472-333">The symbol remains defined until an `#undef` directive for that same symbol is processed, or until the end of the source file is reached.</span></span> <span data-ttu-id="7f472-334">Dies bedeutet, dass `#define` die-und- `#undef` Direktiven in einer Quelldatei keine Auswirkung auf andere Quelldateien im gleichen Programm haben.</span><span class="sxs-lookup"><span data-stu-id="7f472-334">An implication of this is that `#define` and `#undef` directives in one source file have no effect on other source files in the same program.</span></span>

<span data-ttu-id="7f472-335">Wenn in einem Vorverarbeitungs Ausdruck darauf verwiesen wird, hat ein definiertes bedingtes Kompilierungs Symbol den `true`booleschen Wert, und ein nicht definiertes bedingtes Kompilierungs Symbol weist den booleschen Wert `false`auf.</span><span class="sxs-lookup"><span data-stu-id="7f472-335">When referenced in a pre-processing expression, a defined conditional compilation symbol has the boolean value `true`, and an undefined conditional compilation symbol has the boolean value `false`.</span></span> <span data-ttu-id="7f472-336">Es ist nicht erforderlich, dass bedingte Kompilierungs Symbole explizit deklariert werden, bevor in Vorverarbeitungs Ausdrücken auf Sie verwiesen wird.</span><span class="sxs-lookup"><span data-stu-id="7f472-336">There is no requirement that conditional compilation symbols be explicitly declared before they are referenced in pre-processing expressions.</span></span> <span data-ttu-id="7f472-337">Stattdessen sind nicht deklarierte Symbole einfach undefiniert und verfügen daher `false`über den Wert.</span><span class="sxs-lookup"><span data-stu-id="7f472-337">Instead, undeclared symbols are simply undefined and thus have the value `false`.</span></span>

<span data-ttu-id="7f472-338">Der Namespace für Symbole für die bedingte Kompilierung ist eindeutig und getrennt von allen anderen benannten C# Entitäten in einem Programm.</span><span class="sxs-lookup"><span data-stu-id="7f472-338">The name space for conditional compilation symbols is distinct and separate from all other named entities in a C# program.</span></span> <span data-ttu-id="7f472-339">Auf Symbole für die bedingte Kompilierung kann `#define` nur `#undef` in-und-Direktiven und in Vorverarbeitungs Ausdrücken verwiesen werden.</span><span class="sxs-lookup"><span data-stu-id="7f472-339">Conditional compilation symbols can only be referenced in `#define` and `#undef` directives and in pre-processing expressions.</span></span>

### <a name="pre-processing-expressions"></a><span data-ttu-id="7f472-340">Vorverarbeiten von Ausdrücken</span><span class="sxs-lookup"><span data-stu-id="7f472-340">Pre-processing expressions</span></span>

<span data-ttu-id="7f472-341">Vorverarbeitungs Ausdrücke können in `#if` -und `#elif` -Direktiven auftreten.</span><span class="sxs-lookup"><span data-stu-id="7f472-341">Pre-processing expressions can occur in `#if` and `#elif` directives.</span></span> <span data-ttu-id="7f472-342">`!`Die Operatoren `==`, `!=`, `&&` und sindinVorverarbeitungsAusdrückenzulässig,undfürdieGruppierungkönnenKlammernverwendetwerden.`||`</span><span class="sxs-lookup"><span data-stu-id="7f472-342">The operators `!`, `==`, `!=`, `&&` and `||` are permitted in pre-processing expressions, and parentheses may be used for grouping.</span></span>

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

<span data-ttu-id="7f472-343">Wenn in einem Vorverarbeitungs Ausdruck darauf verwiesen wird, hat ein definiertes bedingtes Kompilierungs Symbol den `true`booleschen Wert, und ein nicht definiertes bedingtes Kompilierungs Symbol weist den booleschen Wert `false`auf.</span><span class="sxs-lookup"><span data-stu-id="7f472-343">When referenced in a pre-processing expression, a defined conditional compilation symbol has the boolean value `true`, and an undefined conditional compilation symbol has the boolean value `false`.</span></span>

<span data-ttu-id="7f472-344">Bei der Auswertung eines Vorverarbeitungs Ausdrucks ergibt sich immer ein boolescher Wert.</span><span class="sxs-lookup"><span data-stu-id="7f472-344">Evaluation of a pre-processing expression always yields a boolean value.</span></span> <span data-ttu-id="7f472-345">Die Regeln für die Auswertung eines Vorverarbeitungs Ausdrucks sind identisch mit denen für einen konstanten Ausdruck ([Konstante Ausdrücke](expressions.md#constant-expressions)), mit dem Unterschied, dass nur benutzerdefinierte Entitäten, auf die verwiesen werden kann, Symbole für die bedingte Kompilierung sind.</span><span class="sxs-lookup"><span data-stu-id="7f472-345">The rules of evaluation for a pre-processing expression are the same as those for a constant expression ([Constant expressions](expressions.md#constant-expressions)), except that the only user-defined entities that can be referenced are conditional compilation symbols.</span></span>

### <a name="declaration-directives"></a><span data-ttu-id="7f472-346">Deklarations Direktiven</span><span class="sxs-lookup"><span data-stu-id="7f472-346">Declaration directives</span></span>

<span data-ttu-id="7f472-347">Die Deklarations Anweisungen werden verwendet, um Symbole für die bedingte Kompilierung zu definieren oder zu deaktivieren.</span><span class="sxs-lookup"><span data-stu-id="7f472-347">The declaration directives are used to define or undefine conditional compilation symbols.</span></span>

```antlr
pp_declaration
    : whitespace? '#' whitespace? 'define' whitespace conditional_symbol pp_new_line
    | whitespace? '#' whitespace? 'undef' whitespace conditional_symbol pp_new_line
    ;

pp_new_line
    : whitespace? single_line_comment? new_line
    ;
```

<span data-ttu-id="7f472-348">Die Verarbeitung einer `#define` -Direktive bewirkt, dass das angegebene bedingte Kompilierungs Symbol definiert wird, beginnend mit der Quellzeile, die auf die-Direktive folgt.</span><span class="sxs-lookup"><span data-stu-id="7f472-348">The processing of a `#define` directive causes the given conditional compilation symbol to become defined, starting with the source line that follows the directive.</span></span> <span data-ttu-id="7f472-349">Entsprechend bewirkt die Verarbeitung einer- `#undef` Direktive, dass das angegebene bedingte Kompilierungs Symbol undefiniert wird, beginnend mit der Quellzeile, die auf die-Direktive folgt.</span><span class="sxs-lookup"><span data-stu-id="7f472-349">Likewise, the processing of an `#undef` directive causes the given conditional compilation symbol to become undefined, starting with the source line that follows the directive.</span></span>

<span data-ttu-id="7f472-350">Alle `#define` - `#undef` und-Direktiven in einer Quelldatei müssen vor dem ersten *Token* ([Tokens](lexical-structure.md#tokens)) in der Quelldatei auftreten; andernfalls tritt ein Kompilierzeitfehler auf.</span><span class="sxs-lookup"><span data-stu-id="7f472-350">Any `#define` and `#undef` directives in a source file must occur before the first *token* ([Tokens](lexical-structure.md#tokens)) in the source file; otherwise a compile-time error occurs.</span></span> <span data-ttu-id="7f472-351">In intuitiver Hinsicht `#define` müssen `#undef` -und-Direktiven allen "echten Code" in der Quelldatei vorangestellt sein.</span><span class="sxs-lookup"><span data-stu-id="7f472-351">In intuitive terms, `#define` and `#undef` directives must precede any "real code" in the source file.</span></span>

<span data-ttu-id="7f472-352">Das Beispiel:</span><span class="sxs-lookup"><span data-stu-id="7f472-352">The example:</span></span>
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
<span data-ttu-id="7f472-353">ist gültig, da `#define` die-Anweisungen dem ersten Token (dem `namespace` -Schlüsselwort) in der Quelldatei vorangestellt sind.</span><span class="sxs-lookup"><span data-stu-id="7f472-353">is valid because the `#define` directives precede the first token (the `namespace` keyword) in the source file.</span></span>

<span data-ttu-id="7f472-354">Das folgende Beispiel führt zu einem Kompilierzeitfehler, da `#define` ein echter Code befolgt:</span><span class="sxs-lookup"><span data-stu-id="7f472-354">The following example results in a compile-time error because a `#define` follows real code:</span></span>
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

<span data-ttu-id="7f472-355">Ein `#define` kann ein Symbol für die bedingte Kompilierung definieren, das bereits definiert ist, ohne `#undef` dass ein Eingreifen für dieses Symbol vorliegt.</span><span class="sxs-lookup"><span data-stu-id="7f472-355">A `#define` may define a conditional compilation symbol that is already defined, without there being any intervening `#undef` for that symbol.</span></span> <span data-ttu-id="7f472-356">Im folgenden Beispiel wird ein Symbol `A` für die bedingte Kompilierung definiert und dann wieder definiert.</span><span class="sxs-lookup"><span data-stu-id="7f472-356">The example below defines a conditional compilation symbol `A` and then defines it again.</span></span>
```csharp
#define A
#define A
```

<span data-ttu-id="7f472-357">Ein `#undef` kann ein bedingtes Kompilierungs Symbol, das nicht definiert ist, nicht definieren.</span><span class="sxs-lookup"><span data-stu-id="7f472-357">A `#undef` may "undefine" a conditional compilation symbol that is not defined.</span></span> <span data-ttu-id="7f472-358">Im folgenden Beispiel wird ein Symbol `A` für die bedingte Kompilierung definiert und dann zweimal wieder definiert. das zweite `#undef` hat jedoch keine Auswirkung, aber es ist noch gültig.</span><span class="sxs-lookup"><span data-stu-id="7f472-358">The example below defines a conditional compilation symbol `A` and then undefines it twice; although the second `#undef` has no effect, it is still valid.</span></span>
```csharp
#define A
#undef A
#undef A
```

### <a name="conditional-compilation-directives"></a><span data-ttu-id="7f472-359">Bedingte Kompilierungs Direktiven</span><span class="sxs-lookup"><span data-stu-id="7f472-359">Conditional compilation directives</span></span>

<span data-ttu-id="7f472-360">Die Direktiven für die bedingte Kompilierung werden verwendet, um Teile einer Quelldatei bedingt einzuschließen oder auszuschließen.</span><span class="sxs-lookup"><span data-stu-id="7f472-360">The conditional compilation directives are used to conditionally include or exclude portions of a source file.</span></span>

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

<span data-ttu-id="7f472-361">Wie durch die-Syntax angegeben, müssen bedingte Kompilierungs Direktiven als Sätze geschrieben werden, die aus, `#if` in Reihenfolge, einer `#elif` -Direktive, NULL `#else` oder mehr-Direktiven, keiner oder einer-Direktive und einer `#endif` -Direktive bestehen</span><span class="sxs-lookup"><span data-stu-id="7f472-361">As indicated by the syntax, conditional compilation directives must be written as sets consisting of, in order, an `#if` directive, zero or more `#elif` directives, zero or one `#else` directive, and an `#endif` directive.</span></span> <span data-ttu-id="7f472-362">Zwischen den Direktiven bestehen bedingte Abschnitte des Quellcodes.</span><span class="sxs-lookup"><span data-stu-id="7f472-362">Between the directives are conditional sections of source code.</span></span> <span data-ttu-id="7f472-363">Jeder Abschnitt wird von der unmittelbar vorangehenden-Direktive gesteuert.</span><span class="sxs-lookup"><span data-stu-id="7f472-363">Each section is controlled by the immediately preceding directive.</span></span> <span data-ttu-id="7f472-364">Ein bedingter Abschnitt kann selbst ggf. ggf. ggf</span><span class="sxs-lookup"><span data-stu-id="7f472-364">A conditional section may itself contain nested conditional compilation directives provided these directives form complete sets.</span></span>

<span data-ttu-id="7f472-365">Ein *pp_conditional* wählt höchstens eine der enthaltenen *conditional_section*s für die normale lexikalische Verarbeitung aus:</span><span class="sxs-lookup"><span data-stu-id="7f472-365">A *pp_conditional* selects at most one of the contained *conditional_section*s for normal lexical processing:</span></span>

*  <span data-ttu-id="7f472-366">Die *pp_expression*s der Anweisungen `#if` und `#elif` werden in der Reihenfolge ausgewertet, bis eine `true` ergibt.</span><span class="sxs-lookup"><span data-stu-id="7f472-366">The *pp_expression*s of the `#if` and `#elif` directives are evaluated in order until one yields `true`.</span></span> <span data-ttu-id="7f472-367">Wenn ein Ausdruck `true` ergibt, wird der *conditional_section* der entsprechenden Direktive ausgewählt.</span><span class="sxs-lookup"><span data-stu-id="7f472-367">If an expression yields `true`, the *conditional_section* of the corresponding directive is selected.</span></span>
*  <span data-ttu-id="7f472-368">Wenn alle *pp_expression*s `false` ergeben und eine `#else`-Direktive vorhanden ist, wird der *conditional_section* -Wert der `#else`-Direktive ausgewählt.</span><span class="sxs-lookup"><span data-stu-id="7f472-368">If all *pp_expression*s yield `false`, and if an `#else` directive is present, the *conditional_section* of the `#else` directive is selected.</span></span>
*  <span data-ttu-id="7f472-369">Andernfalls wird kein *conditional_section* ausgewählt.</span><span class="sxs-lookup"><span data-stu-id="7f472-369">Otherwise, no *conditional_section* is selected.</span></span>

<span data-ttu-id="7f472-370">Das ausgewählte *conditional_section*(sofern vorhanden) wird als normales *input_section*verarbeitet: der im Abschnitt enthaltene Quellcode muss der lexikalischen Grammatik entsprechen. Token werden aus dem Quellcode im-Abschnitt generiert. und Vorverarbeitungs Direktiven im-Abschnitt haben die vorgeschriebenen Auswirkungen.</span><span class="sxs-lookup"><span data-stu-id="7f472-370">The selected *conditional_section*, if any, is processed as a normal *input_section*: the source code contained in the section must adhere to the lexical grammar; tokens are generated from the source code in the section; and pre-processing directives in the section have the prescribed effects.</span></span>

<span data-ttu-id="7f472-371">Die verbleibenden *conditional_section*s werden, sofern vorhanden, als *skipped_section*s verarbeitet: mit Ausnahme von Vorverarbeitungs Direktiven muss der Quellcode im-Abschnitt nicht an die lexikalische Grammatik heran bestehen. aus dem Quellcode im-Abschnitt werden keine Token generiert. und Vorverarbeitungs Direktiven im Abschnitt müssen lexikalisch korrigiert werden, werden jedoch nicht anderweitig verarbeitet.</span><span class="sxs-lookup"><span data-stu-id="7f472-371">The remaining *conditional_section*s, if any, are processed as *skipped_section*s: except for pre-processing directives, the source code in the section need not adhere to the lexical grammar; no tokens are generated from the source code in the section; and pre-processing directives in the section must be lexically correct but are not otherwise processed.</span></span> <span data-ttu-id="7f472-372">Innerhalb einer *conditional_section* , die als *skipped_section*verarbeitet wird, werden alle geschachtelten *conditional_section*s (die in geschachtelten `#if`... `#endif`-und `#region`... `#endregion`-Konstrukten enthalten sind) ebenfalls als skipped_ verarbeitet.  *Abschnitt*s.</span><span class="sxs-lookup"><span data-stu-id="7f472-372">Within a *conditional_section* that is being processed as a *skipped_section*, any nested *conditional_section*s (contained in nested `#if`...`#endif` and `#region`...`#endregion` constructs) are also processed as *skipped_section*s.</span></span>

<span data-ttu-id="7f472-373">Im folgenden Beispiel wird veranschaulicht, wie bedingte Kompilierungs Direktiven schachteln können:</span><span class="sxs-lookup"><span data-stu-id="7f472-373">The following example illustrates how conditional compilation directives can nest:</span></span>
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

<span data-ttu-id="7f472-374">Mit Ausnahme von Vorverarbeitungs Direktiven unterliegt der übersprungene Quellcode nicht der lexikalischen Analyse.</span><span class="sxs-lookup"><span data-stu-id="7f472-374">Except for pre-processing directives, skipped source code is not subject to lexical analysis.</span></span> <span data-ttu-id="7f472-375">Beispielsweise ist Folgendes gültig, obwohl der nicht abgeschlossener Kommentar im `#else` Abschnitt lautet:</span><span class="sxs-lookup"><span data-stu-id="7f472-375">For example, the following is valid despite the unterminated comment in the `#else` section:</span></span>
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

<span data-ttu-id="7f472-376">Beachten Sie jedoch, dass Vorverarbeitungs Direktiven auch in übersprungenen Abschnitten des Quellcodes lexikalisch korrigiert werden müssen.</span><span class="sxs-lookup"><span data-stu-id="7f472-376">Note, however, that pre-processing directives are required to be lexically correct even in skipped sections of source code.</span></span>

<span data-ttu-id="7f472-377">Vorverarbeitungs Direktiven werden nicht verarbeitet, wenn Sie in mehrzeiligen Eingabe Elementen angezeigt werden.</span><span class="sxs-lookup"><span data-stu-id="7f472-377">Pre-processing directives are not processed when they appear inside multi-line input elements.</span></span> <span data-ttu-id="7f472-378">Das Programm lautet z. b.:</span><span class="sxs-lookup"><span data-stu-id="7f472-378">For example, the program:</span></span>
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
<span data-ttu-id="7f472-379">Ergebnisse in der Ausgabe:</span><span class="sxs-lookup"><span data-stu-id="7f472-379">results in the output:</span></span>
```console
hello,
#if Debug
        world
#else
        Nebraska
#endif
```

<span data-ttu-id="7f472-380">In besonderen Fällen hängt der Satz der verarbeiteten Vorverarbeitungs Direktiven möglicherweise von der Auswertung des *pp_expression*ab.</span><span class="sxs-lookup"><span data-stu-id="7f472-380">In peculiar cases, the set of pre-processing directives that is processed might depend on the evaluation of the *pp_expression*.</span></span> <span data-ttu-id="7f472-381">Das Beispiel:</span><span class="sxs-lookup"><span data-stu-id="7f472-381">The example:</span></span>
```csharp
#if X
    /*
#else
    /* */ class Q { }
#endif
```
<span data-ttu-id="7f472-382">erzeugt immer denselben Tokenstream (`class` `}` `Q` `{` ), unabhängig davon, ob definiert `X` ist oder nicht.</span><span class="sxs-lookup"><span data-stu-id="7f472-382">always produces the same token stream (`class` `Q` `{` `}`), regardless of whether or not `X` is defined.</span></span> <span data-ttu-id="7f472-383">Wenn `X` definiert ist, sind `#if` die einzigen verarbeiteten Direktiven `#endif`und, aufgrund des mehrzeiligen Kommentars.</span><span class="sxs-lookup"><span data-stu-id="7f472-383">If `X` is defined, the only processed directives are `#if` and `#endif`, due to the multi-line comment.</span></span> <span data-ttu-id="7f472-384">Wenn `X` nicht definiert ist, sind drei-`#if`Direktiven `#endif`(, `#else`,) Teil der direktivenmenge.</span><span class="sxs-lookup"><span data-stu-id="7f472-384">If `X` is undefined, then three directives (`#if`, `#else`, `#endif`) are part of the directive set.</span></span>

### <a name="diagnostic-directives"></a><span data-ttu-id="7f472-385">Diagnose Direktiven</span><span class="sxs-lookup"><span data-stu-id="7f472-385">Diagnostic directives</span></span>

<span data-ttu-id="7f472-386">Die Diagnose Direktiven werden verwendet, um Fehler-und Warnmeldungen explizit zu generieren, die auf die gleiche Weise wie andere Kompilierzeitfehler und-Warnungen gemeldet werden.</span><span class="sxs-lookup"><span data-stu-id="7f472-386">The diagnostic directives are used to explicitly generate error and warning messages that are reported in the same way as other compile-time errors and warnings.</span></span>

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

<span data-ttu-id="7f472-387">Das Beispiel:</span><span class="sxs-lookup"><span data-stu-id="7f472-387">The example:</span></span>
```csharp
#warning Code review needed before check-in

#if Debug && Retail
    #error A build can't be both debug and retail
#endif

class Test {...}
```
<span data-ttu-id="7f472-388">erzeugt immer eine Warnung ("Code Review ist vor dem Einchecken erforderlich") und erzeugt einen Kompilierzeitfehler ("ein Build kann nicht gleichzeitig Debuggen und Einzelhandel sein") `Debug` , `Retail` wenn die bedingten Symbole und definiert sind.</span><span class="sxs-lookup"><span data-stu-id="7f472-388">always produces a warning ("Code review needed before check-in"), and produces a compile-time error ("A build can't be both debug and retail") if the conditional symbols `Debug` and `Retail` are both defined.</span></span> <span data-ttu-id="7f472-389">Beachten Sie, dass ein *pp_message* beliebigen Text enthalten kann. insbesondere muss Sie keine wohlgeformten Token enthalten, wie durch das einfache Anführungszeichen im Wort `can't` gezeigt.</span><span class="sxs-lookup"><span data-stu-id="7f472-389">Note that a *pp_message* can contain arbitrary text; specifically, it need not contain well-formed tokens, as shown by the single quote in the word `can't`.</span></span>

### <a name="region-directives"></a><span data-ttu-id="7f472-390">Regions Direktiven</span><span class="sxs-lookup"><span data-stu-id="7f472-390">Region directives</span></span>

<span data-ttu-id="7f472-391">Die Regions Direktiven werden verwendet, um Bereiche des Quellcodes explizit zu markieren.</span><span class="sxs-lookup"><span data-stu-id="7f472-391">The region directives are used to explicitly mark regions of source code.</span></span>

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

<span data-ttu-id="7f472-392">An einen Bereich ist keine semantische Bedeutung angefügt. Regionen sind für die Verwendung durch den Programmierer oder automatisierte Tools vorgesehen, um einen Abschnitt des Quellcodes zu markieren.</span><span class="sxs-lookup"><span data-stu-id="7f472-392">No semantic meaning is attached to a region; regions are intended for use by the programmer or by automated tools to mark a section of source code.</span></span> <span data-ttu-id="7f472-393">Die in einer `#region` -oder `#endregion` -Direktive angegebene Meldung hat ebenfalls keine semantische Bedeutung; Sie dient lediglich zur Identifizierung der Region.</span><span class="sxs-lookup"><span data-stu-id="7f472-393">The message specified in a `#region` or `#endregion` directive likewise has no semantic meaning; it merely serves to identify the region.</span></span> <span data-ttu-id="7f472-394">Übereinstimmende `#region`-und `#endregion`-Direktiven haben möglicherweise unterschiedliche *pp_message*s.</span><span class="sxs-lookup"><span data-stu-id="7f472-394">Matching `#region` and `#endregion` directives may have different *pp_message*s.</span></span>

<span data-ttu-id="7f472-395">Die lexikalische Verarbeitung einer Region:</span><span class="sxs-lookup"><span data-stu-id="7f472-395">The lexical processing of a region:</span></span>
```csharp
#region
...
#endregion
```
<span data-ttu-id="7f472-396">entspricht genau der lexikalischen Verarbeitung einer Direktive für die bedingte Kompilierung in der Form:</span><span class="sxs-lookup"><span data-stu-id="7f472-396">corresponds exactly to the lexical processing of a conditional compilation directive of the form:</span></span>
```csharp
#if true
...
#endif
```

### <a name="line-directives"></a><span data-ttu-id="7f472-397">Line-Direktiven</span><span class="sxs-lookup"><span data-stu-id="7f472-397">Line directives</span></span>

<span data-ttu-id="7f472-398">Zeilen Anweisungen können verwendet werden, um die Zeilennummern und Quell Dateinamen zu ändern, die vom Compiler in Ausgabe wie Warnungen und Fehlern gemeldet werden, und die von Aufrufer-Informations Attributen ([aufruferinformationsattribute](attributes.md#caller-info-attributes)) verwendet werden.</span><span class="sxs-lookup"><span data-stu-id="7f472-398">Line directives may be used to alter the line numbers and source file names that are reported by the compiler in output such as warnings and errors, and that are used by caller info attributes ([Caller info attributes](attributes.md#caller-info-attributes)).</span></span>

<span data-ttu-id="7f472-399">Zeilen Direktiven werden am häufigsten in metaprogrammierungs Tools verwendet, C# die Quellcode aus einer anderen Texteingabe generieren.</span><span class="sxs-lookup"><span data-stu-id="7f472-399">Line directives are most commonly used in meta-programming tools that generate C# source code from some other text input.</span></span>

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

<span data-ttu-id="7f472-400">Wenn keine `#line` -Direktiven vorhanden sind, meldet der Compiler echte Zeilennummern und Quell Dateinamen in der Ausgabe.</span><span class="sxs-lookup"><span data-stu-id="7f472-400">When no `#line` directives are present, the compiler reports true line numbers and source file names in its output.</span></span> <span data-ttu-id="7f472-401">Bei der Verarbeitung einer `#line`-Direktive, die eine *line_indicator* enthält, die nicht `default` ist, behandelt der Compiler die Zeile nach der Direktive mit der angegebenen Zeilennummer (und dem Dateinamen, falls angegeben).</span><span class="sxs-lookup"><span data-stu-id="7f472-401">When processing a `#line` directive that includes a *line_indicator* that is not `default`, the compiler treats the line after the directive as having the given line number (and file name, if specified).</span></span>

<span data-ttu-id="7f472-402">Eine `#line default` -Direktive kehrt die Auswirkungen aller vorangehenden #line Direktiven um.</span><span class="sxs-lookup"><span data-stu-id="7f472-402">A `#line default` directive reverses the effect of all preceding #line directives.</span></span> <span data-ttu-id="7f472-403">Der Compiler meldet echte Zeilen Informationen für nachfolgende Zeilen, genau so, `#line` als ob keine Direktiven verarbeitet wurden.</span><span class="sxs-lookup"><span data-stu-id="7f472-403">The compiler reports true line information for subsequent lines, precisely as if no `#line` directives had been processed.</span></span>

<span data-ttu-id="7f472-404">Eine `#line hidden` -Direktive hat keine Auswirkung auf die Datei-und Zeilennummern, die in Fehlermeldungen gemeldet werden, wirkt sich aber auf das Debugging auf Quell Ebene aus</span><span class="sxs-lookup"><span data-stu-id="7f472-404">A `#line hidden` directive has no effect on the file and line numbers reported in error messages, but does affect source level debugging.</span></span> <span data-ttu-id="7f472-405">Beim Debuggen verfügen alle Zeilen `#line hidden` zwischen einer-Direktive und der nachfolgenden `#line hidden` `#line` Direktive (das nicht) über keine Zeilennummern Informationen.</span><span class="sxs-lookup"><span data-stu-id="7f472-405">When debugging, all lines between a `#line hidden` directive and the subsequent `#line` directive (that is not `#line hidden`) have no line number information.</span></span> <span data-ttu-id="7f472-406">Wenn Sie Code im Debugger schrittweise durchlaufen, werden diese Zeilen vollständig übersprungen.</span><span class="sxs-lookup"><span data-stu-id="7f472-406">When stepping through code in the debugger, these lines will be skipped entirely.</span></span>

<span data-ttu-id="7f472-407">Beachten Sie, dass ein *file_name* -Wert von einem regulären Zeichenfolgenliteralwert abweicht, da Escapezeichen das Zeichen "`\`" bezeichnet einfach einen normalen umgekehrten Schrägstrich innerhalb eines *file_name*.</span><span class="sxs-lookup"><span data-stu-id="7f472-407">Note that a *file_name* differs from a regular string literal in that escape characters are not processed; the "`\`" character simply designates an ordinary backslash character within a *file_name*.</span></span>

### <a name="pragma-directives"></a><span data-ttu-id="7f472-408">Pragma-Direktiven</span><span class="sxs-lookup"><span data-stu-id="7f472-408">Pragma directives</span></span>

<span data-ttu-id="7f472-409">Die `#pragma` Vorverarbeitungs Direktive wird verwendet, um dem Compiler optionale Kontextinformationen anzugeben.</span><span class="sxs-lookup"><span data-stu-id="7f472-409">The `#pragma` preprocessing directive is used to specify optional contextual information to the compiler.</span></span> <span data-ttu-id="7f472-410">Die in einer `#pragma` -Direktive angegebenen Informationen ändern die Programm Semantik nie.</span><span class="sxs-lookup"><span data-stu-id="7f472-410">The information supplied in a `#pragma` directive will never change program semantics.</span></span>

```antlr
pp_pragma
    : whitespace? '#' whitespace? 'pragma' whitespace pragma_body pp_new_line
    ;

pragma_body
    : pragma_warning_body
    ;
```

<span data-ttu-id="7f472-411">C#stellt `#pragma` Anweisungen zum Steuern von Compilerwarnungen bereit.</span><span class="sxs-lookup"><span data-stu-id="7f472-411">C# provides `#pragma` directives to control compiler warnings.</span></span> <span data-ttu-id="7f472-412">Zukünftige Versionen der Sprache können zusätzliche `#pragma` Anweisungen enthalten.</span><span class="sxs-lookup"><span data-stu-id="7f472-412">Future versions of the language may include additional `#pragma` directives.</span></span> <span data-ttu-id="7f472-413">Um die Interoperabilität mit anderen C# Compilern sicherzustellen, C# gibt der Microsoft-Compiler keine Kompilierungs `#pragma` Fehler für unbekannte Direktiven aus. solche Direktiven generieren jedoch Warnungen.</span><span class="sxs-lookup"><span data-stu-id="7f472-413">To ensure interoperability with other C# compilers, the Microsoft C# compiler does not issue compilation errors for unknown `#pragma` directives; such directives do however generate warnings.</span></span>

#### <a name="pragma-warning"></a><span data-ttu-id="7f472-414">pragma-Warnung</span><span class="sxs-lookup"><span data-stu-id="7f472-414">Pragma warning</span></span>

<span data-ttu-id="7f472-415">Die `#pragma warning` -Anweisung wird verwendet, um alle oder einen bestimmten Satz von Warnmeldungen während der Kompilierung des nachfolgenden Programm Texts zu deaktivieren oder wiederherzustellen.</span><span class="sxs-lookup"><span data-stu-id="7f472-415">The `#pragma warning` directive is used to disable or restore all or a particular set of warning messages during compilation of the subsequent program text.</span></span>

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

<span data-ttu-id="7f472-416">Eine `#pragma warning` Anweisung, die die Warnungs Liste auslässt, wirkt sich auf alle Warnungen aus.</span><span class="sxs-lookup"><span data-stu-id="7f472-416">A `#pragma warning` directive that omits the warning list affects all warnings.</span></span> <span data-ttu-id="7f472-417">Eine `#pragma warning` Anweisung, die eine Warnungs Liste enthält, wirkt sich nur auf die Warnungen aus, die in der Liste angegeben sind.</span><span class="sxs-lookup"><span data-stu-id="7f472-417">A `#pragma warning` directive that includes a warning list affects only those warnings that are specified in the list.</span></span>

<span data-ttu-id="7f472-418">Eine `#pragma warning disable` -Direktive deaktiviert alle oder den angegebenen Satz von Warnungen.</span><span class="sxs-lookup"><span data-stu-id="7f472-418">A `#pragma warning disable` directive disables all or the given set of warnings.</span></span>

<span data-ttu-id="7f472-419">Eine `#pragma warning restore` -Direktive stellt alle oder den angegebenen Satz von Warnungen in dem Zustand wieder her, der am Anfang der Kompilierungseinheit wirksam war.</span><span class="sxs-lookup"><span data-stu-id="7f472-419">A `#pragma warning restore` directive restores all or the given set of warnings to the state that was in effect at the beginning of the compilation unit.</span></span> <span data-ttu-id="7f472-420">Beachten Sie Folgendes: Wenn eine bestimmte Warnung extern deaktiviert wurde `#pragma warning restore` , wird diese Warnung von a (ob für alle oder eine bestimmte Warnung) nicht erneut aktiviert.</span><span class="sxs-lookup"><span data-stu-id="7f472-420">Note that if a particular warning was disabled externally, a `#pragma warning restore` (whether for all or the specific warning) will not re-enable that warning.</span></span>

<span data-ttu-id="7f472-421">Das folgende Beispiel zeigt die Verwendung `#pragma warning` von, um die Warnung, die bei referenzierten Membern verwiesen wird, vorübergehend zu deaktivieren. dabei wird C# die Warnungs Nummer des Microsoft-Compilers verwendet.</span><span class="sxs-lookup"><span data-stu-id="7f472-421">The following example shows use of `#pragma warning` to temporarily disable the warning reported when obsoleted members are referenced, using the warning number from the Microsoft C# compiler.</span></span>
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
