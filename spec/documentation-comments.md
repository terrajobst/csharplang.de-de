---
ms.openlocfilehash: c9f8417dc68153f02ceb72bb1d51f3615f3c4961
ms.sourcegitcommit: 3fc033b6e98ed7ecdf46a85c79b00a3a3ddcf963
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/18/2019
ms.locfileid: "54272045"
---
# <a name="documentation-comments"></a><span data-ttu-id="4b96a-101">Kommentare zur Dokumentation</span><span class="sxs-lookup"><span data-stu-id="4b96a-101">Documentation comments</span></span>

<span data-ttu-id="4b96a-102">C# bietet einen Mechanismus für Programmierer, dokumentieren ihren Code mit einer besonderen Kommentarsyntax, die XML-Text enthält.</span><span class="sxs-lookup"><span data-stu-id="4b96a-102">C# provides a mechanism for programmers to document their code using a special comment syntax that contains XML text.</span></span> <span data-ttu-id="4b96a-103">In Quellcodedateien können Kommentare, die einem bestimmten Format aufweisen verwendet werden, leiten Sie ein Tool zum Erstellen von XML aus diesen Kommentaren und die Quellcodeelemente, die sie vor.</span><span class="sxs-lookup"><span data-stu-id="4b96a-103">In source code files, comments having a certain form can be used to direct a tool to produce XML from those comments and the source code elements, which they precede.</span></span> <span data-ttu-id="4b96a-104">Diese Syntax heißen-Kommentaren mit ***Dokumentationskommentare***.</span><span class="sxs-lookup"><span data-stu-id="4b96a-104">Comments using such syntax are called ***documentation comments***.</span></span> <span data-ttu-id="4b96a-105">Sie müssen unmittelbar vor einem benutzerdefinierten Typ (z. B. eine Klasse, Delegat oder Schnittstelle) oder ein Element (z. B. ein Feld, Ereignis, Eigenschaft oder Methode) stehen.</span><span class="sxs-lookup"><span data-stu-id="4b96a-105">They must immediately precede a user-defined type (such as a class, delegate, or interface) or a member (such as a field, event, property, or method).</span></span> <span data-ttu-id="4b96a-106">Das Tool zum Generieren von XML-wird aufgerufen, die ***Dokumentations-Generator***.</span><span class="sxs-lookup"><span data-stu-id="4b96a-106">The XML generation tool is called the ***documentation generator***.</span></span> <span data-ttu-id="4b96a-107">(Dieses Generators könnte sein, aber muss nicht sein, die C#-Compiler selbst.) Die Ausgabe der Dokumentations-Generator wird aufgerufen, die ***Dokumentationsdatei***.</span><span class="sxs-lookup"><span data-stu-id="4b96a-107">(This generator could be, but need not be, the C# compiler itself.) The output produced by the documentation generator is called the ***documentation file***.</span></span> <span data-ttu-id="4b96a-108">Eine Dokumentationsdatei dient als Eingabe für eine ***Dokumentations-Viewer***ein Tool soll eine Art visuelle Darstellung der Typinformationen und die zugehörige Dokumentation zu erzeugen.</span><span class="sxs-lookup"><span data-stu-id="4b96a-108">A documentation file is used as input to a ***documentation viewer***; a tool intended to produce some sort of visual display of type information and its associated documentation.</span></span>

<span data-ttu-id="4b96a-109">Diese Spezifikation empfiehlt einen Satz von Tags in Dokumentationskommentare, verwendet werden, aber mithilfe dieser Tags ist nicht erforderlich und andere Tags können verwendet werden, wenn gewünscht, wie lange die Regeln für wohlgeformte XML-eingehalten werden.</span><span class="sxs-lookup"><span data-stu-id="4b96a-109">This specification suggests a set of tags to be used in documentation comments, but use of these tags is not required, and other tags may be used if desired, as long the rules of well-formed XML are followed.</span></span>

## <a name="introduction"></a><span data-ttu-id="4b96a-110">Einführung</span><span class="sxs-lookup"><span data-stu-id="4b96a-110">Introduction</span></span>

<span data-ttu-id="4b96a-111">Kommentare müssen eine besondere Form können verwendet werden, um ein Tool zum Erstellen von XML aus diesen Kommentaren und die Quellcodeelemente, die sie vor weiterzuleiten.</span><span class="sxs-lookup"><span data-stu-id="4b96a-111">Comments having a special form can be used to direct a tool to produce XML from those comments and the source code elements, which they precede.</span></span> <span data-ttu-id="4b96a-112">Diese Kommentare werden einzeilige Kommentare, die mit drei Schrägstrichen beginnen (`///`), oder Kommentare, die mit einem Schrägstrich und zwei Sterne beginnen getrennt (`/**`).</span><span class="sxs-lookup"><span data-stu-id="4b96a-112">Such comments are single-line comments that start with three slashes (`///`), or delimited comments that start with a slash and two stars (`/**`).</span></span> <span data-ttu-id="4b96a-113">Sie müssen unmittelbar voranstehen, einen benutzerdefinierten Typ (z. B. eine Klasse, Delegat oder Schnittstelle) oder ein Element (z. B. ein Feld, Ereignis, Eigenschaft oder Methode), die sie kommentieren.</span><span class="sxs-lookup"><span data-stu-id="4b96a-113">They must immediately precede a user-defined type (such as a class, delegate, or interface) or a member (such as a field, event, property, or method) that they annotate.</span></span> <span data-ttu-id="4b96a-114">Attribut Abschnitte ([Attributspezifikation](attributes.md#attribute-specification)) gelten als Teil von Deklarationen, damit Kommentare zur Dokumentation auf einen Typ oder Member angewendete vor angegeben werden müssen.</span><span class="sxs-lookup"><span data-stu-id="4b96a-114">Attribute sections ([Attribute specification](attributes.md#attribute-specification)) are considered part of declarations, so documentation comments must precede attributes applied to a type or member.</span></span>

<span data-ttu-id="4b96a-115">__Syntax:__</span><span class="sxs-lookup"><span data-stu-id="4b96a-115">__Syntax:__</span></span>

```antlr
single_line_doc_comment
    : '///' input_character*
    ;

delimited_doc_comment
    : '/**' delimited_comment_section* asterisk+ '/'
    ;
```

<span data-ttu-id="4b96a-116">In einer *Single_line_doc_comment*, wenn eine *Leerzeichen* folgende Zeichen der `///` Zeichen auf jedem der *Single_line_doc_comment*s angrenzende mit dem aktuellen *Single_line_doc_comment*, klicken Sie dann, *Leerzeichen* Zeichen ist nicht in der XML-Ausgabe enthalten.</span><span class="sxs-lookup"><span data-stu-id="4b96a-116">In a *single_line_doc_comment*, if there is a *whitespace* character following the `///` characters on each of the *single_line_doc_comment*s adjacent to the current *single_line_doc_comment*, then that *whitespace* character is not included in the XML output.</span></span>

<span data-ttu-id="4b96a-117">In einem getrennten-Dok-Kommentar Wenn das erste nicht-Leerzeichen in der zweiten Zeile ein Sternchen und dasselbe Muster von optionalen Leerzeichen und ein Sternchen am Anfang der Zeile im getrennten-Dok-Kommentar jedes wiederholt, Klicken Sie dann sind die Zeichen, das wiederholte Muster nicht in der XML-Ausgabe enthalten.</span><span class="sxs-lookup"><span data-stu-id="4b96a-117">In a delimited-doc-comment, if the first non-whitespace character on the second line is an asterisk and the same pattern of optional whitespace characters and an asterisk character is repeated at the beginning of each of the line within the delimited-doc-comment, then the characters of the repeated pattern are not included in the XML output.</span></span> <span data-ttu-id="4b96a-118">Das Muster kann Leerzeichen nach sowie vor, das Sternchenzeichen enthalten.</span><span class="sxs-lookup"><span data-stu-id="4b96a-118">The pattern may include whitespace characters after, as well as before, the asterisk character.</span></span>

<span data-ttu-id="4b96a-119">__Beispiel:__</span><span class="sxs-lookup"><span data-stu-id="4b96a-119">__Example:__</span></span>

```csharp
/// <summary>Class <c>Point</c> models a point in a two-dimensional
/// plane.</summary>
///
public class Point 
{
    /// <summary>method <c>draw</c> renders the point.</summary>
    void draw() {...}
}
```

<span data-ttu-id="4b96a-120">Der Text innerhalb von Dokumentationskommentaren muss wohlgeformt sein und gemäß den Regeln des XML-Codes (https://www.w3.org/TR/REC-xml).</span><span class="sxs-lookup"><span data-stu-id="4b96a-120">The text within documentation comments must be well formed according to the rules of XML (https://www.w3.org/TR/REC-xml).</span></span> <span data-ttu-id="4b96a-121">Wenn der XML-Code fehlerhaft formuliert ist, formatiert, wird eine Warnung generiert, und die Dokumentationsdatei enthält einen Kommentar, der besagt, dass ein Fehler aufgetreten ist.</span><span class="sxs-lookup"><span data-stu-id="4b96a-121">If the XML is ill formed, a warning is generated and the documentation file will contain a comment saying that an error was encountered.</span></span>

<span data-ttu-id="4b96a-122">Obwohl Entwickler können ihren eigenen Satz von Tags zu erstellen sind, ist eine empfohlene Sammlung in definiert [empfohlene Tags](documentation-comments.md#recommended-tags).</span><span class="sxs-lookup"><span data-stu-id="4b96a-122">Although developers are free to create their own set of tags, a recommended set is defined in [Recommended tags](documentation-comments.md#recommended-tags).</span></span> <span data-ttu-id="4b96a-123">Einige der empfohlenen Tags haben eine besondere Bedeutung:</span><span class="sxs-lookup"><span data-stu-id="4b96a-123">Some of the recommended tags have special meanings:</span></span>

*  <span data-ttu-id="4b96a-124">Die `<param>` Tag wird verwendet, um Parameter zu beschreiben.</span><span class="sxs-lookup"><span data-stu-id="4b96a-124">The `<param>` tag is used to describe parameters.</span></span> <span data-ttu-id="4b96a-125">Wenn diese einem Tag verwendet wird, muss der Dokumentations-Generator überprüfen, ob der angegebene Parameter vorhanden ist und alle Parameter in Dokumentationskommentaren beschrieben werden.</span><span class="sxs-lookup"><span data-stu-id="4b96a-125">If such a tag is used, the documentation generator must verify that the specified parameter exists and that all parameters are described in documentation comments.</span></span> <span data-ttu-id="4b96a-126">Wenn eine solche Überprüfung fehlschlägt, gibt der Dokumentations-Generator eine Warnung aus.</span><span class="sxs-lookup"><span data-stu-id="4b96a-126">If such verification fails, the documentation generator issues a warning.</span></span>
*  <span data-ttu-id="4b96a-127">Das `cref`-Attribut kann an jedes Tag angefügt werden, um einen Verweis auf ein Codeelement bereitzustellen.</span><span class="sxs-lookup"><span data-stu-id="4b96a-127">The `cref` attribute can be attached to any tag to provide a reference to a code element.</span></span> <span data-ttu-id="4b96a-128">Der Dokumentations-Generator muss, ob dieses Codeelement vorhanden ist.</span><span class="sxs-lookup"><span data-stu-id="4b96a-128">The documentation generator must verify that this code element exists.</span></span> <span data-ttu-id="4b96a-129">Wenn die Überprüfung fehlschlägt, gibt der Dokumentations-Generator eine Warnung aus.</span><span class="sxs-lookup"><span data-stu-id="4b96a-129">If the verification fails, the documentation generator issues a warning.</span></span> <span data-ttu-id="4b96a-130">Bei der Suche für ein Namen im beschrieben eine `cref` -Attribut muss der Dokumentations-Generator Namespace Sichtbarkeit gemäß respektieren `using` Anweisungen im Quellcode.</span><span class="sxs-lookup"><span data-stu-id="4b96a-130">When looking for a name described in a `cref` attribute, the documentation generator must respect namespace visibility according to `using` statements appearing within the source code.</span></span> <span data-ttu-id="4b96a-131">Für Codeelemente, die generisch ist, sind die normale generische Syntax (d. h. "`List<T>`") kann nicht verwendet werden, da er ungültige XML-Daten erzeugt.</span><span class="sxs-lookup"><span data-stu-id="4b96a-131">For code elements that are generic, the normal generic syntax (that is, "`List<T>`") cannot be used because it produces invalid XML.</span></span> <span data-ttu-id="4b96a-132">Geschweifte Klammern können anstelle von eckigen Klammern verwendet werden (d. h. "`List{T}`"), oder die XML-Escape-Syntax verwendet werden kann (d. h. "`List&lt;T&gt;`").</span><span class="sxs-lookup"><span data-stu-id="4b96a-132">Braces can be used instead of brackets (that is, "`List{T}`"), or the XML escape syntax can be used (that is, "`List&lt;T&gt;`").</span></span>
*  <span data-ttu-id="4b96a-133">Die `<summary>` richtet sich an Tag durch eine Dokumentations-Viewer verwendet werden, um weitere Informationen über einen Typ oder Member anzuzeigen.</span><span class="sxs-lookup"><span data-stu-id="4b96a-133">The `<summary>` tag is intended to be used by a documentation viewer to display additional information about a type or member.</span></span>
*  <span data-ttu-id="4b96a-134">Die `<include>` Tag enthält Informationen aus einer externen XML-Datei.</span><span class="sxs-lookup"><span data-stu-id="4b96a-134">The `<include>` tag includes information from an external XML file.</span></span>

<span data-ttu-id="4b96a-135">Sorgfältig Beachten Sie, dass die Dokumentationsdatei keine vollständigen Informationen über Typen und Membern (z. B. es enthält keine Typinformationen).</span><span class="sxs-lookup"><span data-stu-id="4b96a-135">Note carefully that the documentation file does not provide full information about the type and members (for example, it does not contain any type information).</span></span> <span data-ttu-id="4b96a-136">Um solche Informationen über einen Typ oder Member zu erhalten, muss die Dokumentationsdatei zusammen mit Reflektion für den tatsächlichen Typ oder Member verwendet werden.</span><span class="sxs-lookup"><span data-stu-id="4b96a-136">To get such information about a type or member, the documentation file must be used in conjunction with reflection on the actual type or member.</span></span>

## <a name="recommended-tags"></a><span data-ttu-id="4b96a-137">Empfohlene tags</span><span class="sxs-lookup"><span data-stu-id="4b96a-137">Recommended tags</span></span>

<span data-ttu-id="4b96a-138">Der Dokumentations-Generator muss akzeptiert und verarbeitet alle Tags, die gemäß den Regeln der XML gültig ist.</span><span class="sxs-lookup"><span data-stu-id="4b96a-138">The documentation generator must accept and process any tag that is valid according to the rules of XML.</span></span> <span data-ttu-id="4b96a-139">Die folgenden Tags stellen häufig verwendete Funktionen in der Dokumentation für die Benutzer bereit.</span><span class="sxs-lookup"><span data-stu-id="4b96a-139">The following tags provide commonly used functionality in user documentation.</span></span> <span data-ttu-id="4b96a-140">(Natürlich sind andere Tags möglich).</span><span class="sxs-lookup"><span data-stu-id="4b96a-140">(Of course, other tags are possible.)</span></span>


| <span data-ttu-id="4b96a-141">__Tag__</span><span class="sxs-lookup"><span data-stu-id="4b96a-141">__Tag__</span></span>          | <span data-ttu-id="4b96a-142">__Bereich__</span><span class="sxs-lookup"><span data-stu-id="4b96a-142">__Section__</span></span>                                            | <span data-ttu-id="4b96a-143">__Zweck__</span><span class="sxs-lookup"><span data-stu-id="4b96a-143">__Purpose__</span></span>                                            |
|------------------|--------------------------------------------------------|--------------------------------------------------------|
| `<c>`            | [`<c>`](documentation-comments.md#c)                   | <span data-ttu-id="4b96a-144">Text in einer Code-ähnliche Schriftart festlegen</span><span class="sxs-lookup"><span data-stu-id="4b96a-144">Set text in a code-like font</span></span>                           | 
| `<code>`         | [`<code>`](documentation-comments.md#code)             | <span data-ttu-id="4b96a-145">Legen Sie eine oder mehrere Zeilen von Code oder Programmausgabe-Quellausgabe</span><span class="sxs-lookup"><span data-stu-id="4b96a-145">Set one or more lines of source code or program output</span></span> |
| `<example>`      | [`<example>`](documentation-comments.md#example)       | <span data-ttu-id="4b96a-146">Geben Sie ein Beispiel für</span><span class="sxs-lookup"><span data-stu-id="4b96a-146">Indicate an example</span></span>                                    |
| `<exception>`    | [`<exception>`](documentation-comments.md#exception)   | <span data-ttu-id="4b96a-147">Identifiziert die Ausnahmen, die eine Methode ausgelöst werden können</span><span class="sxs-lookup"><span data-stu-id="4b96a-147">Identifies the exceptions a method can throw</span></span>           |
| `<include>`      | [`<include>`](documentation-comments.md#include)       | <span data-ttu-id="4b96a-148">Enthält XML-Code aus einer externen Datei</span><span class="sxs-lookup"><span data-stu-id="4b96a-148">Includes XML from an external file</span></span>                     |
| `<list>`         | [`<list>`](documentation-comments.md#list)             | <span data-ttu-id="4b96a-149">Erstellen Sie eine Liste oder Tabelle</span><span class="sxs-lookup"><span data-stu-id="4b96a-149">Create a list or table</span></span>                                 |
| `<para>`         | [`<para>`](documentation-comments.md#para)             | <span data-ttu-id="4b96a-150">Zulassen der Struktur, die Text hinzugefügt werden</span><span class="sxs-lookup"><span data-stu-id="4b96a-150">Permit structure to be added to text</span></span>                   |
| `<param>`        | [`<param>`](documentation-comments.md#param)           | <span data-ttu-id="4b96a-151">Beschreiben Sie die Parameter für eine Methode oder Konstruktor</span><span class="sxs-lookup"><span data-stu-id="4b96a-151">Describe a parameter for a method or constructor</span></span>       |
| `<paramref>`     | [`<paramref>`](documentation-comments.md#paramref)     | <span data-ttu-id="4b96a-152">Erkennen Sie, dass ein Wort ein Parametername ist</span><span class="sxs-lookup"><span data-stu-id="4b96a-152">Identify that a word is a parameter name</span></span>               |
| `<permission>`   | [`<permission>`](documentation-comments.md#permission) | <span data-ttu-id="4b96a-153">Dokumentieren Sie die Security Zugriff auf ein Element</span><span class="sxs-lookup"><span data-stu-id="4b96a-153">Document the security accessibility of a member</span></span>        |
| `<remark>`       | [`<remark>`](documentation-comments.md#remark)         | <span data-ttu-id="4b96a-154">Weitere Informationen zu einem Typ beschreiben</span><span class="sxs-lookup"><span data-stu-id="4b96a-154">Describe additional information about a type</span></span>           |
| `<returns>`      | [`<returns>`](documentation-comments.md#returns)       | <span data-ttu-id="4b96a-155">Der Rückgabewert einer Methode zu beschreiben</span><span class="sxs-lookup"><span data-stu-id="4b96a-155">Describe the return value of a method</span></span>                  |
| `<see>`          | [`<see>`](documentation-comments.md#see)               | <span data-ttu-id="4b96a-156">Geben Sie einen link</span><span class="sxs-lookup"><span data-stu-id="4b96a-156">Specify a link</span></span>                                         |
| `<seealso>`      | [`<seealso>`](documentation-comments.md#seealso)       | <span data-ttu-id="4b96a-157">Generieren Sie einen Eintrag auch finden Sie unter</span><span class="sxs-lookup"><span data-stu-id="4b96a-157">Generate a See Also entry</span></span>                              |
| `<summary>`      | [`<summary>`](documentation-comments.md#summary)       | <span data-ttu-id="4b96a-158">Ein Typ oder Member eines Typs zu beschreiben</span><span class="sxs-lookup"><span data-stu-id="4b96a-158">Describe a type or a member of a type</span></span>                  |
| `<value>`        | [`<value>`](documentation-comments.md#value)           | <span data-ttu-id="4b96a-159">Beschreiben einer Eigenschaft</span><span class="sxs-lookup"><span data-stu-id="4b96a-159">Describe a property</span></span>                                    |
| `<typeparam>`    |                                                        | <span data-ttu-id="4b96a-160">Beschreiben Sie einen generischen Typparameter</span><span class="sxs-lookup"><span data-stu-id="4b96a-160">Describe a generic type parameter</span></span>                      |
| `<typeparamref>` |                                                        | <span data-ttu-id="4b96a-161">Erkennen Sie, dass ein Wort einen Parameternamen handelt</span><span class="sxs-lookup"><span data-stu-id="4b96a-161">Identify that a word is a type parameter name</span></span>          |

### `<c>`

<span data-ttu-id="4b96a-162">Dieses Tag stellt einen Mechanismus, um anzugeben, dass ein Fragment von Text in einer Beschreibung in einer speziellen Schriftart wie z. B., verwendet für einen Codeblock festgelegt werden soll.</span><span class="sxs-lookup"><span data-stu-id="4b96a-162">This tag provides a mechanism to indicate that a fragment of text within a description should be set in a special font such as that used for a block of code.</span></span> <span data-ttu-id="4b96a-163">Verwenden Sie für die Feldlinien des tatsächlichen Code, `<code>` ([`<code>`](documentation-comments.md#code)).</span><span class="sxs-lookup"><span data-stu-id="4b96a-163">For lines of actual code, use `<code>` ([`<code>`](documentation-comments.md#code)).</span></span>

<span data-ttu-id="4b96a-164">__Syntax:__</span><span class="sxs-lookup"><span data-stu-id="4b96a-164">__Syntax:__</span></span>

```xml
<c>text</c>
```

<span data-ttu-id="4b96a-165">__Beispiel:__</span><span class="sxs-lookup"><span data-stu-id="4b96a-165">__Example:__</span></span>

```csharp
/// <summary>Class <c>Point</c> models a point in a two-dimensional
/// plane.</summary>

public class Point 
{
    // ...
}
```

### `<code>`

<span data-ttu-id="4b96a-166">Dieses Tag wird verwendet, eine oder mehrere Zeilen von Code oder Programmausgabe Quellausgabe in einige besondere Schriftart festlegen.</span><span class="sxs-lookup"><span data-stu-id="4b96a-166">This tag is used to set one or more lines of source code or program output in some special font.</span></span> <span data-ttu-id="4b96a-167">Verwenden Sie für kleine Codefragmente in die Geschichte hinter dem, `<c>` ([`<c>`](documentation-comments.md#c)).</span><span class="sxs-lookup"><span data-stu-id="4b96a-167">For small code fragments in narrative, use `<c>` ([`<c>`](documentation-comments.md#c)).</span></span>

<span data-ttu-id="4b96a-168">__Syntax:__</span><span class="sxs-lookup"><span data-stu-id="4b96a-168">__Syntax:__</span></span>

```xml
<code>source code or program output</code>
```

<span data-ttu-id="4b96a-169">__Beispiel:__</span><span class="sxs-lookup"><span data-stu-id="4b96a-169">__Example:__</span></span>

```csharp
/// <summary>This method changes the point's location by
///    the given x- and y-offsets.
/// <example>For example:
/// <code>
///    Point p = new Point(3,5);
///    p.Translate(-1,3);
/// </code>
/// results in <c>p</c>'s having the value (2,8).
/// </example>
/// </summary>

public void Translate(int xor, int yor) {
    X += xor;
    Y += yor;
}   
```

### `<example>`

<span data-ttu-id="4b96a-170">Dieses Tag ermöglicht Beispielcode in einem Kommentar, um anzugeben, wie eine Methode oder eines anderen Bibliothekmembers verwendet werden kann.</span><span class="sxs-lookup"><span data-stu-id="4b96a-170">This tag allows example code within a comment, to specify how a method or other library member may be used.</span></span> <span data-ttu-id="4b96a-171">Normalerweise würde dies auch die Verwendung des Tags betreffen `<code>` ([`<code>`](documentation-comments.md#code)) ebenfalls.</span><span class="sxs-lookup"><span data-stu-id="4b96a-171">Ordinarily, this would also involve use of the tag `<code>` ([`<code>`](documentation-comments.md#code)) as well.</span></span>

<span data-ttu-id="4b96a-172">__Syntax:__</span><span class="sxs-lookup"><span data-stu-id="4b96a-172">__Syntax:__</span></span>

```xml
<example>description</example>
```

<span data-ttu-id="4b96a-173">__Beispiel:__</span><span class="sxs-lookup"><span data-stu-id="4b96a-173">__Example:__</span></span>

<span data-ttu-id="4b96a-174">Finden Sie unter `<code>` ([`<code>`](documentation-comments.md#code)) ein Beispiel.</span><span class="sxs-lookup"><span data-stu-id="4b96a-174">See `<code>` ([`<code>`](documentation-comments.md#code)) for an example.</span></span>

### `<exception>`

<span data-ttu-id="4b96a-175">Dieses Tag bietet eine Möglichkeit, die Ausnahmen zu dokumentieren, die eine Methode ausgelöst werden kann.</span><span class="sxs-lookup"><span data-stu-id="4b96a-175">This tag provides a way to document the exceptions a method can throw.</span></span>

<span data-ttu-id="4b96a-176">__Syntax:__</span><span class="sxs-lookup"><span data-stu-id="4b96a-176">__Syntax:__</span></span>

```xml
<exception cref="member">description</exception>
```

<span data-ttu-id="4b96a-177">wo</span><span class="sxs-lookup"><span data-stu-id="4b96a-177">where</span></span>

* <span data-ttu-id="4b96a-178">`member` ist der Name eines Elements.</span><span class="sxs-lookup"><span data-stu-id="4b96a-178">`member` is the name of a member.</span></span> <span data-ttu-id="4b96a-179">Der Dokumentations-Generator überprüft, ob der angegebene Member vorhanden ist und übersetzt `member` auf den kanonischen Elementnamen in der Dokumentationsdatei.</span><span class="sxs-lookup"><span data-stu-id="4b96a-179">The documentation generator checks that the given member exists and translates `member` to the canonical element name in the documentation file.</span></span>
* <span data-ttu-id="4b96a-180">`description` ist eine Beschreibung der Umstände, in denen die Ausnahme ausgelöst wird.</span><span class="sxs-lookup"><span data-stu-id="4b96a-180">`description` is a description of the circumstances in which the exception is thrown.</span></span>

<span data-ttu-id="4b96a-181">__Beispiel:__</span><span class="sxs-lookup"><span data-stu-id="4b96a-181">__Example:__</span></span>

```csharp
public class DataBaseOperations
{
    /// <exception cref="MasterFileFormatCorruptException"></exception>
    /// <exception cref="MasterFileLockedOpenException"></exception>
    public static void ReadRecord(int flag) {
        if (flag == 1)
            throw new MasterFileFormatCorruptException();
        else if (flag == 2)
            throw new MasterFileLockedOpenException();
        // ...
    } 
}
```

### `<include>`

<span data-ttu-id="4b96a-182">Dieses Tag ermöglicht, einschließlich der Informationen aus einem XML-Dokument, das außerhalb der Quellcodedatei ist.</span><span class="sxs-lookup"><span data-stu-id="4b96a-182">This tag allows including information from an XML document that is external to the source code file.</span></span> <span data-ttu-id="4b96a-183">Die externe Datei muss ein wohlgeformtes XML-Dokument sein, und ein XPath-Ausdruck an, welche XML-Code aus diesem Dokument sollen Geben Sie das Dokument angewendet wird.</span><span class="sxs-lookup"><span data-stu-id="4b96a-183">The external file must be a well-formed XML document, and an XPath expression is applied to that document to specify what XML from that document to include.</span></span> <span data-ttu-id="4b96a-184">Die `<include>` Tag wird dann mit den ausgewählten XML-Code aus dem externen Dokument ersetzt.</span><span class="sxs-lookup"><span data-stu-id="4b96a-184">The `<include>` tag is then replaced with the selected XML from the external document.</span></span>

<span data-ttu-id="4b96a-185">__Syntax:__</span><span class="sxs-lookup"><span data-stu-id="4b96a-185">__Syntax:__</span></span>

```
<include file="filename" path="xpath" />
```

<span data-ttu-id="4b96a-186">wo</span><span class="sxs-lookup"><span data-stu-id="4b96a-186">where</span></span>

* <span data-ttu-id="4b96a-187">`filename` ist der Dateiname einer externen XML-Datei an.</span><span class="sxs-lookup"><span data-stu-id="4b96a-187">`filename` is the file name of an external XML file.</span></span> <span data-ttu-id="4b96a-188">Der Dateiname ist relativ zur Datei interpretiert, die die Include-Tag enthält.</span><span class="sxs-lookup"><span data-stu-id="4b96a-188">The file name is interpreted relative to the file that contains the include tag.</span></span>
* <span data-ttu-id="4b96a-189">`xpath` ist ein XPath-Ausdruck, der Teil der XML-Code in der externen XML-Datei auswählt.</span><span class="sxs-lookup"><span data-stu-id="4b96a-189">`xpath` is an XPath expression that selects some of the XML in the external XML file.</span></span>

<span data-ttu-id="4b96a-190">__Beispiel:__</span><span class="sxs-lookup"><span data-stu-id="4b96a-190">__Example:__</span></span>

<span data-ttu-id="4b96a-191">Wenn der Quellcode eine Deklaration wie enthalten:</span><span class="sxs-lookup"><span data-stu-id="4b96a-191">If the source code contained a declaration like:</span></span>

```csharp
/// <include file="docs.xml" path='extradoc/class[@name="IntList"]/*' />
public class IntList { ... }
```

<span data-ttu-id="4b96a-192">und die externe Datei "docs.xml" hatte den folgenden Inhalt:</span><span class="sxs-lookup"><span data-stu-id="4b96a-192">and the external file "docs.xml" had the following contents:</span></span>

```xml
<?xml version="1.0"?>
<extradoc>
  <class name="IntList">
     <summary>
        Contains a list of integers.
     </summary>
  </class>
  <class name="StringList">
     <summary>
        Contains a list of integers.
     </summary>
  </class>
</extradoc>
```

<span data-ttu-id="4b96a-193">Klicken Sie dann ist die gleiche Dokumentation Ausgabe aus, als ob der Quellcode enthalten:</span><span class="sxs-lookup"><span data-stu-id="4b96a-193">then the same documentation is output as if the source code contained:</span></span>

```csharp
/// <summary>
///    Contains a list of integers.
/// </summary>
public class IntList { ... }
```

### `<list>`

<span data-ttu-id="4b96a-194">Dieses Tag wird verwendet, um eine Liste oder Tabelle der Elemente zu erstellen.</span><span class="sxs-lookup"><span data-stu-id="4b96a-194">This tag is used to create a list or table of items.</span></span> <span data-ttu-id="4b96a-195">Sie enthält eventuell eine `<listheader>` Block, um die Überschriftenzeile einer Tabelle oder einer Definitionsliste zu definieren.</span><span class="sxs-lookup"><span data-stu-id="4b96a-195">It may contain a `<listheader>` block to define the heading row of either a table or definition list.</span></span> <span data-ttu-id="4b96a-196">(Beim Definieren einer Tabelle, die nur einen Eintrag für `term` in der Überschrift müssen angegeben werden.)</span><span class="sxs-lookup"><span data-stu-id="4b96a-196">(When defining a table, only an entry for `term` in the heading need be supplied.)</span></span>

<span data-ttu-id="4b96a-197">Jedes Element in der Liste wird angegeben, mit einem `<item>` Block.</span><span class="sxs-lookup"><span data-stu-id="4b96a-197">Each item in the list is specified with an `<item>` block.</span></span> <span data-ttu-id="4b96a-198">Beim Erstellen einer Definitionsliste sowohl `term` und `description` muss angegeben werden.</span><span class="sxs-lookup"><span data-stu-id="4b96a-198">When creating a definition list, both `term` and `description` must be specified.</span></span> <span data-ttu-id="4b96a-199">Allerdings für eine Tabelle, Liste mit Aufzählungszeichen oder nummerierte Liste nur `description` muss angegeben werden.</span><span class="sxs-lookup"><span data-stu-id="4b96a-199">However, for a table, bulleted list, or numbered list, only `description` need be specified.</span></span>

<span data-ttu-id="4b96a-200">__Syntax:__</span><span class="sxs-lookup"><span data-stu-id="4b96a-200">__Syntax:__</span></span>

```xml
<list type="bullet" | "number" | "table">
   <listheader>
      <term>term</term>
      <description>*description*</description>
   </listheader>
   <item>
      <term>term</term>
      <description>*description*</description>
   </item>
    ...
   <item>
      <term>term</term>
      <description>description</description>
   </item>
</list>
```

<span data-ttu-id="4b96a-201">wo</span><span class="sxs-lookup"><span data-stu-id="4b96a-201">where</span></span>

* <span data-ttu-id="4b96a-202">`term` ist der Begriff definiert, deren Definition befindet sich in `description`.</span><span class="sxs-lookup"><span data-stu-id="4b96a-202">`term` is the term to define, whose definition is in `description`.</span></span>
* <span data-ttu-id="4b96a-203">`description` ist entweder ein Element in einer Aufzählung oder nummerierten Liste oder die Definition einer `term`.</span><span class="sxs-lookup"><span data-stu-id="4b96a-203">`description` is either an item in a bullet or numbered list, or the definition of a `term`.</span></span>

<span data-ttu-id="4b96a-204">__Beispiel:__</span><span class="sxs-lookup"><span data-stu-id="4b96a-204">__Example:__</span></span>

```csharp
public class MyClass
{
    /// <summary>Here is an example of a bulleted list:
    /// <list type="bullet">
    /// <item>
    /// <description>Item 1.</description>
    /// </item>
    /// <item>
    /// <description>Item 2.</description>
    /// </item>
    /// </list>
    /// </summary>
    public static void Main () {
        // ...
    }
}
```

### `<para>`

<span data-ttu-id="4b96a-205">Dieses Tag ist für die Verwendung in anderen Tags, wie z. B. `<summary>` ([`<remark>`](documentation-comments.md#remark)) oder `<returns>` ([`<returns>`](documentation-comments.md#returns)), und lässt die Struktur, die Text hinzugefügt werden.</span><span class="sxs-lookup"><span data-stu-id="4b96a-205">This tag is for use inside other tags, such as `<summary>` ([`<remark>`](documentation-comments.md#remark)) or `<returns>` ([`<returns>`](documentation-comments.md#returns)), and permits structure to be added to text.</span></span>

<span data-ttu-id="4b96a-206">__Syntax:__</span><span class="sxs-lookup"><span data-stu-id="4b96a-206">__Syntax:__</span></span>

```xml
<para>content</para>
```

<span data-ttu-id="4b96a-207">wo `content` ist der Text des Absatzes.</span><span class="sxs-lookup"><span data-stu-id="4b96a-207">where `content` is the text of the paragraph.</span></span>

<span data-ttu-id="4b96a-208">__Beispiel:__</span><span class="sxs-lookup"><span data-stu-id="4b96a-208">__Example:__</span></span>

```csharp
/// <summary>This is the entry point of the Point class testing program.
/// <para>This program tests each method and operator, and
/// is intended to be run after any non-trivial maintenance has
/// been performed on the Point class.</para></summary>
public static void Main() {
    // ...
}
```

### `<param>`

<span data-ttu-id="4b96a-209">Dieses Tag wird verwendet, um einen Parameter für eine Methode, einem Konstruktor oder einem Indexer zu beschreiben.</span><span class="sxs-lookup"><span data-stu-id="4b96a-209">This tag is used to describe a parameter for a method, constructor, or indexer.</span></span>

<span data-ttu-id="4b96a-210">__Syntax:__</span><span class="sxs-lookup"><span data-stu-id="4b96a-210">__Syntax:__</span></span>

```xml
<param name="name">description</param>
```

<span data-ttu-id="4b96a-211">wo</span><span class="sxs-lookup"><span data-stu-id="4b96a-211">where</span></span>

* <span data-ttu-id="4b96a-212">`name` Ist der Name des Parameters.</span><span class="sxs-lookup"><span data-stu-id="4b96a-212">`name` is the name of the parameter.</span></span>
* <span data-ttu-id="4b96a-213">`description` ist eine Beschreibung des Parameters.</span><span class="sxs-lookup"><span data-stu-id="4b96a-213">`description` is a description of the parameter.</span></span>

<span data-ttu-id="4b96a-214">__Beispiel:__</span><span class="sxs-lookup"><span data-stu-id="4b96a-214">__Example:__</span></span>

```csharp
/// <summary>This method changes the point's location to
///    the given coordinates.</summary>
/// <param name="xor">the new x-coordinate.</param>
/// <param name="yor">the new y-coordinate.</param>
public void Move(int xor, int yor) {
    X = xor;
    Y = yor;
}
```

### `<paramref>`

<span data-ttu-id="4b96a-215">Dieses Tag wird verwendet, um anzugeben, dass ein Wort ein Parameter ist.</span><span class="sxs-lookup"><span data-stu-id="4b96a-215">This tag is used to indicate that a word is a parameter.</span></span> <span data-ttu-id="4b96a-216">Die Dokumentationsdatei kann verarbeitet werden, um diesen Parameter auf unterschiedliche Weise zu formatieren.</span><span class="sxs-lookup"><span data-stu-id="4b96a-216">The documentation file can be processed to format this parameter in some distinct way.</span></span>

<span data-ttu-id="4b96a-217">__Syntax:__</span><span class="sxs-lookup"><span data-stu-id="4b96a-217">__Syntax:__</span></span>

```xml
<paramref name="name"/>
```

<span data-ttu-id="4b96a-218">wo `name` ist der Name des Parameters.</span><span class="sxs-lookup"><span data-stu-id="4b96a-218">where `name` is the name of the parameter.</span></span>

<span data-ttu-id="4b96a-219">__Beispiel:__</span><span class="sxs-lookup"><span data-stu-id="4b96a-219">__Example:__</span></span>

```csharp
/// <summary>This constructor initializes the new Point to
///    (<paramref name="xor"/>,<paramref name="yor"/>).</summary>
/// <param name="xor">the new Point's x-coordinate.</param>
/// <param name="yor">the new Point's y-coordinate.</param>

public Point(int xor, int yor) {
    X = xor;
    Y = yor;
}
```

### `<permission>`

<span data-ttu-id="4b96a-220">Dieses Tag ermöglicht den Sicherheitszugriff auf einen Member dokumentiert werden soll.</span><span class="sxs-lookup"><span data-stu-id="4b96a-220">This tag allows the security accessibility of a member to be documented.</span></span>

<span data-ttu-id="4b96a-221">__Syntax:__</span><span class="sxs-lookup"><span data-stu-id="4b96a-221">__Syntax:__</span></span>

```xml
<permission cref="member">description</permission>
```

<span data-ttu-id="4b96a-222">wo</span><span class="sxs-lookup"><span data-stu-id="4b96a-222">where</span></span>

* <span data-ttu-id="4b96a-223">`member` ist der Name eines Elements.</span><span class="sxs-lookup"><span data-stu-id="4b96a-223">`member` is the name of a member.</span></span> <span data-ttu-id="4b96a-224">Der Dokumentations-Generator überprüft, ob das angegebene Codeelement vorhanden ist und übersetzt *Member* auf den kanonischen Elementnamen in der Dokumentationsdatei.</span><span class="sxs-lookup"><span data-stu-id="4b96a-224">The documentation generator checks that the given code element exists and translates *member* to the canonical element name in the documentation file.</span></span>
* <span data-ttu-id="4b96a-225">`description` ist eine Beschreibung des Zugriffs auf das Element.</span><span class="sxs-lookup"><span data-stu-id="4b96a-225">`description` is a description of the access to the member.</span></span>

<span data-ttu-id="4b96a-226">__Beispiel:__</span><span class="sxs-lookup"><span data-stu-id="4b96a-226">__Example:__</span></span>

```csharp
/// <permission cref="System.Security.PermissionSet">Everyone can
/// access this method.</permission>

public static void Test() {
    // ...
}
```

### `<remark>`

<span data-ttu-id="4b96a-227">Dieses Tag wird verwendet, um zusätzliche Informationen über einen Typ anzugeben.</span><span class="sxs-lookup"><span data-stu-id="4b96a-227">This tag is used to specify extra information about a type.</span></span> <span data-ttu-id="4b96a-228">(Verwenden `<summary>` ([`<summary>`](documentation-comments.md#summary)) um den Typ selbst und die Member eines Typs beschreiben.)</span><span class="sxs-lookup"><span data-stu-id="4b96a-228">(Use `<summary>` ([`<summary>`](documentation-comments.md#summary)) to describe the type itself and the members of a type.)</span></span>

<span data-ttu-id="4b96a-229">__Syntax:__</span><span class="sxs-lookup"><span data-stu-id="4b96a-229">__Syntax:__</span></span>

```xml
<remark>description</remark>
```

<span data-ttu-id="4b96a-230">wo `description` ist der Text, der die Anmerkung.</span><span class="sxs-lookup"><span data-stu-id="4b96a-230">where `description` is the text of the remark.</span></span>

<span data-ttu-id="4b96a-231">__Beispiel:__</span><span class="sxs-lookup"><span data-stu-id="4b96a-231">__Example:__</span></span>

```csharp
/// <summary>Class <c>Point</c> models a point in a 
/// two-dimensional plane.</summary>
/// <remark>Uses polar coordinates</remark>
public class Point 
{
    // ...
}
```

### `<returns>`

<span data-ttu-id="4b96a-232">Dieses Tag wird verwendet, um den Rückgabewert einer Methode zu beschreiben.</span><span class="sxs-lookup"><span data-stu-id="4b96a-232">This tag is used to describe the return value of a method.</span></span>

<span data-ttu-id="4b96a-233">__Syntax:__</span><span class="sxs-lookup"><span data-stu-id="4b96a-233">__Syntax:__</span></span>

```xml
<returns>description</returns>
```

<span data-ttu-id="4b96a-234">wo `description` finden Sie eine Beschreibung des Rückgabewerts.</span><span class="sxs-lookup"><span data-stu-id="4b96a-234">where `description` is a description of the return value.</span></span>

<span data-ttu-id="4b96a-235">__Beispiel:__</span><span class="sxs-lookup"><span data-stu-id="4b96a-235">__Example:__</span></span>

```csharp
/// <summary>Report a point's location as a string.</summary>
/// <returns>A string representing a point's location, in the form (x,y),
///    without any leading, trailing, or embedded whitespace.</returns>
public override string ToString() {
    return "(" + X + "," + Y + ")";
}
```

### `<see>`

<span data-ttu-id="4b96a-236">Dieses Tag ermöglicht einen Link im Text angegeben werden.</span><span class="sxs-lookup"><span data-stu-id="4b96a-236">This tag allows a link to be specified within text.</span></span> <span data-ttu-id="4b96a-237">Verwendung `<seealso>` ([`<seealso>`](documentation-comments.md#seealso)) um Text anzugeben, die in einem Abschnitt Siehe auch angezeigt werden soll.</span><span class="sxs-lookup"><span data-stu-id="4b96a-237">Use `<seealso>` ([`<seealso>`](documentation-comments.md#seealso)) to indicate text that is to appear in a See Also section.</span></span>

<span data-ttu-id="4b96a-238">__Syntax:__</span><span class="sxs-lookup"><span data-stu-id="4b96a-238">__Syntax:__</span></span>

```xml
<see cref="member"/>
```

<span data-ttu-id="4b96a-239">wo `member` ist der Name eines Elements.</span><span class="sxs-lookup"><span data-stu-id="4b96a-239">where `member` is the name of a member.</span></span> <span data-ttu-id="4b96a-240">Der Dokumentations-Generator überprüft, ob das angegebene Codeelement vorhanden ist und ändert *Member* an den Elementnamen in der der generierten Dokumentationsdatei.</span><span class="sxs-lookup"><span data-stu-id="4b96a-240">The documentation generator checks that the given code element exists and changes *member* to the element name in the generated documentation file.</span></span>

<span data-ttu-id="4b96a-241">__Beispiel:__</span><span class="sxs-lookup"><span data-stu-id="4b96a-241">__Example:__</span></span>

```csharp
/// <summary>This method changes the point's location to
///    the given coordinates.</summary>
/// <see cref="Translate"/>
public void Move(int xor, int yor) {
    X = xor;
    Y = yor;
}

/// <summary>This method changes the point's location by
///    the given x- and y-offsets.
/// </summary>
/// <see cref="Move"/>
public void Translate(int xor, int yor) {
    X += xor;
    Y += yor;
}
```

### `<seealso>`

<span data-ttu-id="4b96a-242">Dieses Tag ermöglicht einen Eintrag im Abschnitt Siehe auch generiert werden soll.</span><span class="sxs-lookup"><span data-stu-id="4b96a-242">This tag allows an entry to be generated for the See Also section.</span></span> <span data-ttu-id="4b96a-243">Verwendung `<see>` ([`<see>`](documentation-comments.md#see)), geben Sie einen Link im Text.</span><span class="sxs-lookup"><span data-stu-id="4b96a-243">Use `<see>` ([`<see>`](documentation-comments.md#see)) to specify a link from within text.</span></span>

<span data-ttu-id="4b96a-244">__Syntax:__</span><span class="sxs-lookup"><span data-stu-id="4b96a-244">__Syntax:__</span></span>

```xml
<seealso cref="member"/>
```

<span data-ttu-id="4b96a-245">wo `member` ist der Name eines Elements.</span><span class="sxs-lookup"><span data-stu-id="4b96a-245">where `member` is the name of a member.</span></span> <span data-ttu-id="4b96a-246">Der Dokumentations-Generator überprüft, ob das angegebene Codeelement vorhanden ist und ändert *Member* an den Elementnamen in der der generierten Dokumentationsdatei.</span><span class="sxs-lookup"><span data-stu-id="4b96a-246">The documentation generator checks that the given code element exists and changes *member* to the element name in the generated documentation file.</span></span>

<span data-ttu-id="4b96a-247">__Beispiel:__</span><span class="sxs-lookup"><span data-stu-id="4b96a-247">__Example:__</span></span>

```csharp
/// <summary>This method determines whether two Points have the same
///    location.</summary>
/// <seealso cref="operator=="/>
/// <seealso cref="operator!="/>
public override bool Equals(object o) {
    // ...
}
```

### `<summary>`

Dieses Tag kann verwendet werden, um einen Typ oder Member eines Typs beschreiben. <span data-ttu-id="4b96a-249">Verwendung `<remark>` ([`<remark>`](documentation-comments.md#remark)) um den Typ selbst beschreiben.</span><span class="sxs-lookup"><span data-stu-id="4b96a-249">Use `<remark>` ([`<remark>`](documentation-comments.md#remark)) to describe the type itself.</span></span>

<span data-ttu-id="4b96a-250">__Syntax:__</span><span class="sxs-lookup"><span data-stu-id="4b96a-250">__Syntax:__</span></span>

```xml
<summary>description</summary>
```

<span data-ttu-id="4b96a-251">wo `description` ist eine Zusammenfassung der den Typ oder Member.</span><span class="sxs-lookup"><span data-stu-id="4b96a-251">where `description` is a summary of the type or member.</span></span>

<span data-ttu-id="4b96a-252">__Beispiel:__</span><span class="sxs-lookup"><span data-stu-id="4b96a-252">__Example:__</span></span>

```csharp
/// <summary>This constructor initializes the new Point to (0,0).</summary>
public Point() : this(0,0) {
}
```

### `<value>`

<span data-ttu-id="4b96a-253">Dieses Tag kann eine Eigenschaft, um die beschrieben werden.</span><span class="sxs-lookup"><span data-stu-id="4b96a-253">This tag allows a property to be described.</span></span>

<span data-ttu-id="4b96a-254">__Syntax:__</span><span class="sxs-lookup"><span data-stu-id="4b96a-254">__Syntax:__</span></span>

```xml
<value>property description</value>
```

<span data-ttu-id="4b96a-255">wo `property description` finden Sie eine Beschreibung für die Eigenschaft.</span><span class="sxs-lookup"><span data-stu-id="4b96a-255">where `property description` is a description for the property.</span></span>

<span data-ttu-id="4b96a-256">__Beispiel:__</span><span class="sxs-lookup"><span data-stu-id="4b96a-256">__Example:__</span></span>

```csharp
/// <value>Property <c>X</c> represents the point's x-coordinate.</value>
public int X
{
    get { return x; }
    set { x = value; }
}
```

### `<typeparam>`

<span data-ttu-id="4b96a-257">Dieses Tag wird verwendet, um einen generischen Typparameter für eine Klasse, Struktur, Schnittstelle, Delegat oder Methode zu beschreiben.</span><span class="sxs-lookup"><span data-stu-id="4b96a-257">This tag is used to describe a generic type parameter for a class, struct, interface, delegate, or method.</span></span>

<span data-ttu-id="4b96a-258">__Syntax:__</span><span class="sxs-lookup"><span data-stu-id="4b96a-258">__Syntax:__</span></span>

```xml
<typeparam name="name">description</typeparam>
```

<span data-ttu-id="4b96a-259">wo `name` ist der Name des Typparameters, und `description` wird seine Beschreibung.</span><span class="sxs-lookup"><span data-stu-id="4b96a-259">where `name` is the name of the type parameter, and `description` is its description.</span></span>

<span data-ttu-id="4b96a-260">__Beispiel:__</span><span class="sxs-lookup"><span data-stu-id="4b96a-260">__Example:__</span></span>

```csharp
/// <summary>A generic list class.</summary>
/// <typeparam name="T">The type stored by the list.</typeparam>
public class MyList<T> {
    ...
}
```

### `<typeparamref>`

<span data-ttu-id="4b96a-261">Dieses Tag wird verwendet, um anzugeben, dass ein Wort ein Typparameter ist.</span><span class="sxs-lookup"><span data-stu-id="4b96a-261">This tag is used to indicate that a word is a type parameter.</span></span> <span data-ttu-id="4b96a-262">Die Dokumentationsdatei kann verarbeitet werden, um diesen Typparameter auf unterschiedliche Weise zu formatieren.</span><span class="sxs-lookup"><span data-stu-id="4b96a-262">The documentation file can be processed to format this type parameter in some distinct way.</span></span>

<span data-ttu-id="4b96a-263">__Syntax:__</span><span class="sxs-lookup"><span data-stu-id="4b96a-263">__Syntax:__</span></span>

```xml
<typeparamref name="name"/>
```

<span data-ttu-id="4b96a-264">wo `name` ist der Name des Typparameters.</span><span class="sxs-lookup"><span data-stu-id="4b96a-264">where `name` is the name of the type parameter.</span></span>

<span data-ttu-id="4b96a-265">__Beispiel:__</span><span class="sxs-lookup"><span data-stu-id="4b96a-265">__Example:__</span></span>

```csharp
/// <summary>This method fetches data and returns a list of <typeparamref name="T"/>.</summary>
/// <param name="query">query to execute</param>
public List<T> FetchData<T>(string query) {
    ...
}
```

## <a name="processing-the-documentation-file"></a><span data-ttu-id="4b96a-266">Verarbeiten der Dokumentationsdatei</span><span class="sxs-lookup"><span data-stu-id="4b96a-266">Processing the documentation file</span></span>

<span data-ttu-id="4b96a-267">Der Dokumentations-Generator generiert eine ID-Zeichenfolge für jedes Element im Quellcode, der mit einem Dokumentationskommentar gekennzeichnet ist.</span><span class="sxs-lookup"><span data-stu-id="4b96a-267">The documentation generator generates an ID string for each element in the source code that is tagged with a documentation comment.</span></span> <span data-ttu-id="4b96a-268">Diese ID-Zeichenfolge identifiziert eindeutig ein Quellelement.</span><span class="sxs-lookup"><span data-stu-id="4b96a-268">This ID string uniquely identifies a source element.</span></span> <span data-ttu-id="4b96a-269">Eine ID-Zeichenfolge können ein Dokumentations-Viewer die entsprechenden Metadaten/Reflektionselement identifizieren, die Dokumentation gilt.</span><span class="sxs-lookup"><span data-stu-id="4b96a-269">A documentation viewer can use an ID string to identify the corresponding metadata/reflection item to which the documentation applies.</span></span>

<span data-ttu-id="4b96a-270">Die Dokumentationsdatei ist keine hierarchische Darstellung des Quellcodes; Es handelt sich vielmehr um eine flache Liste mit einer generierten ID-Zeichenfolge für jedes Element.</span><span class="sxs-lookup"><span data-stu-id="4b96a-270">The documentation file is not a hierarchical representation of the source code; rather, it is a flat list with a generated ID string for each element.</span></span>

### <a name="id-string-format"></a><span data-ttu-id="4b96a-271">Zeichenfolge-ID-format</span><span class="sxs-lookup"><span data-stu-id="4b96a-271">ID string format</span></span>

<span data-ttu-id="4b96a-272">Der Dokumentations-Generator verwendet die folgenden Regeln beim Generieren der ID-Zeichenfolgen:</span><span class="sxs-lookup"><span data-stu-id="4b96a-272">The documentation generator observes the following rules when it generates the ID strings:</span></span>

*  <span data-ttu-id="4b96a-273">In der Zeichenfolge wird kein Leerraum platziert.</span><span class="sxs-lookup"><span data-stu-id="4b96a-273">No white space is placed in the string.</span></span>

*  <span data-ttu-id="4b96a-274">Der erste Teil der Zeichenfolge identifiziert die Art von Member dokumentiert wird, über ein einzelnes Zeichen, gefolgt von einem Doppelpunkt.</span><span class="sxs-lookup"><span data-stu-id="4b96a-274">The first part of the string identifies the kind of member being documented, via a single character followed by a colon.</span></span> <span data-ttu-id="4b96a-275">Die folgenden Arten von Membern sind definiert:</span><span class="sxs-lookup"><span data-stu-id="4b96a-275">The following kinds of members are defined:</span></span>

   | <span data-ttu-id="4b96a-276">__Zeichen__</span><span class="sxs-lookup"><span data-stu-id="4b96a-276">__Character__</span></span> | <span data-ttu-id="4b96a-277">__Beschreibung__</span><span class="sxs-lookup"><span data-stu-id="4b96a-277">__Description__</span></span>                                             |
   |---------------|-------------------------------------------------------------|
   | <span data-ttu-id="4b96a-278">E</span><span class="sxs-lookup"><span data-stu-id="4b96a-278">E</span></span>             | <span data-ttu-id="4b96a-279">event</span><span class="sxs-lookup"><span data-stu-id="4b96a-279">Event</span></span>                                                       |
   | <span data-ttu-id="4b96a-280">F</span><span class="sxs-lookup"><span data-stu-id="4b96a-280">F</span></span>             | <span data-ttu-id="4b96a-281">Feld</span><span class="sxs-lookup"><span data-stu-id="4b96a-281">Field</span></span>                                                       |
   | <span data-ttu-id="4b96a-282">M</span><span class="sxs-lookup"><span data-stu-id="4b96a-282">M</span></span>             | <span data-ttu-id="4b96a-283">Methode (einschließlich Konstruktoren, Destruktoren und Operatoren)</span><span class="sxs-lookup"><span data-stu-id="4b96a-283">Method (including constructors, destructors, and operators)</span></span> |
   | <span data-ttu-id="4b96a-284">N</span><span class="sxs-lookup"><span data-stu-id="4b96a-284">N</span></span>             | <span data-ttu-id="4b96a-285">Namespace</span><span class="sxs-lookup"><span data-stu-id="4b96a-285">Namespace</span></span>                                                   |
   | <span data-ttu-id="4b96a-286">P</span><span class="sxs-lookup"><span data-stu-id="4b96a-286">P</span></span>             | <span data-ttu-id="4b96a-287">Eigenschaft (einschließlich Indexer)</span><span class="sxs-lookup"><span data-stu-id="4b96a-287">Property (including indexers)</span></span>                               |
   | <span data-ttu-id="4b96a-288">T</span><span class="sxs-lookup"><span data-stu-id="4b96a-288">T</span></span>             | <span data-ttu-id="4b96a-289">Geben Sie (z. B. Klasse, Delegat, Enumeration, Schnittstelle und Struktur)</span><span class="sxs-lookup"><span data-stu-id="4b96a-289">Type (such as class, delegate, enum, interface, and struct)</span></span> |
   | <span data-ttu-id="4b96a-290">!</span><span class="sxs-lookup"><span data-stu-id="4b96a-290">!</span></span>             | <span data-ttu-id="4b96a-291">Fehlermeldungs-Zeichenfolge; der Rest der Zeichenfolge enthält Informationen zum Fehler.</span><span class="sxs-lookup"><span data-stu-id="4b96a-291">Error string; the rest of the string provides information about the error.</span></span> <span data-ttu-id="4b96a-292">Der Dokumentations-Generator generiert z. B. Fehlerinformationen für Links, die nicht aufgelöst werden kann.</span><span class="sxs-lookup"><span data-stu-id="4b96a-292">For example, the documentation generator generates error information for links that cannot be resolved.</span></span> |

*  <span data-ttu-id="4b96a-293">Der zweite Teil der Zeichenfolge ist der vollqualifizierte Name des Elements im Stammverzeichnis des Namespace ab.</span><span class="sxs-lookup"><span data-stu-id="4b96a-293">The second part of the string is the fully qualified name of the element, starting at the root of the namespace.</span></span> <span data-ttu-id="4b96a-294">Der Name des Elements, dessen einschließenden Typen und Namespace sind durch Punkte getrennt.</span><span class="sxs-lookup"><span data-stu-id="4b96a-294">The name of the element, its enclosing type(s), and namespace are separated by periods.</span></span> <span data-ttu-id="4b96a-295">Wenn der Name des Elements selbst Punkte enthält, werden sie durch ersetzt `#(U+0023)` Zeichen.</span><span class="sxs-lookup"><span data-stu-id="4b96a-295">If the name of the item itself has periods, they are replaced by `#(U+0023)` characters.</span></span> <span data-ttu-id="4b96a-296">(Es wird vorausgesetzt, dass kein Element dieses Zeichen im Namen hat.)</span><span class="sxs-lookup"><span data-stu-id="4b96a-296">(It is assumed that no element has this character in its name.)</span></span>
*  <span data-ttu-id="4b96a-297">Listen Sie für die Methoden und Eigenschaften mit Argumenten das Argument folgt in Klammern eingeschlossen.</span><span class="sxs-lookup"><span data-stu-id="4b96a-297">For methods and properties with arguments, the argument list follows, enclosed in parentheses.</span></span> <span data-ttu-id="4b96a-298">Für diejenigen ohne Argumente werden keine Klammern verwendet.</span><span class="sxs-lookup"><span data-stu-id="4b96a-298">For those without arguments, the parentheses are omitted.</span></span> <span data-ttu-id="4b96a-299">Die Argumente werden durch Kommas voneinander getrennt.</span><span class="sxs-lookup"><span data-stu-id="4b96a-299">The arguments are separated by commas.</span></span> <span data-ttu-id="4b96a-300">Die Codierung jedes Arguments ist identisch mit einer CLI-Signatur wie folgt:</span><span class="sxs-lookup"><span data-stu-id="4b96a-300">The encoding of each argument is the same as a CLI signature, as follows:</span></span>
   *  <span data-ttu-id="4b96a-301">Argumente werden anhand des Namens Dokumentation dargestellt, basierend auf dem vollqualifizierten Namen, die wie folgt geändert:</span><span class="sxs-lookup"><span data-stu-id="4b96a-301">Arguments are represented by their documentation name, which is based on their fully qualified name, modified as follows:</span></span>
      * <span data-ttu-id="4b96a-302">Argumente, die generische Typen darstellen müssen ein angefügtes `` ` `` (Gravis)-Zeichens, gefolgt von der Anzahl von Typparametern</span><span class="sxs-lookup"><span data-stu-id="4b96a-302">Arguments that represent generic types have an appended `` ` `` (backtick) character followed by the number of type parameters</span></span>
      * <span data-ttu-id="4b96a-303">Argumente, die mit der `out` oder `ref` -Modifizierer aufweisen. eine `@` nach ihren Namen eingeben.</span><span class="sxs-lookup"><span data-stu-id="4b96a-303">Arguments having the `out` or `ref` modifier have an `@` following their type name.</span></span> <span data-ttu-id="4b96a-304">Argumente zu übergeben, als Wert oder über `params` haben keine besondere Schreibweise.</span><span class="sxs-lookup"><span data-stu-id="4b96a-304">Arguments passed by value or via `params` have no special notation.</span></span>
      * <span data-ttu-id="4b96a-305">Argumente, die Arrays werden als dargestellt `[lowerbound:size, ... , lowerbound:size]` , in dem die Anzahl von Kommas ist der Rang minus 1, und die unteren Grenzen und die Größe jeder Dimension, sofern bekannt, Dezimal dargestellt werden.</span><span class="sxs-lookup"><span data-stu-id="4b96a-305">Arguments that are arrays are represented as `[lowerbound:size, ... , lowerbound:size]` where the number of commas is the rank less one, and the lower bounds and size of each dimension, if known, are represented in decimal.</span></span> <span data-ttu-id="4b96a-306">Wenn die untere Grenze oder die Größe nicht angegeben ist, wird es weggelassen.</span><span class="sxs-lookup"><span data-stu-id="4b96a-306">If a lower bound or size is not specified, it is omitted.</span></span> <span data-ttu-id="4b96a-307">Wenn die untere Grenze und die Größe für eine bestimmte Dimension ausgelassen werden, die `:` ebenfalls ausgelassen werden.</span><span class="sxs-lookup"><span data-stu-id="4b96a-307">If the lower bound and size for a particular dimension are omitted, the `:` is omitted as well.</span></span> <span data-ttu-id="4b96a-308">Verzweigte Arrays werden von einem dargestellt `[]` pro Ebene.</span><span class="sxs-lookup"><span data-stu-id="4b96a-308">Jagged arrays are represented by one `[]` per level.</span></span>
      * <span data-ttu-id="4b96a-309">Argumente, die Zeiger als "void" aufweisen, werden mit dargestellt eine `*` nach dem Typnamen.</span><span class="sxs-lookup"><span data-stu-id="4b96a-309">Arguments that have pointer types other than void are represented using a `*` following the type name.</span></span> <span data-ttu-id="4b96a-310">Ein void-Zeiger wird dargestellt, verwenden den Typnamen `System.Void`.</span><span class="sxs-lookup"><span data-stu-id="4b96a-310">A void pointer is represented using a type name of `System.Void`.</span></span>
      * <span data-ttu-id="4b96a-311">Argumente, die die generischen Typparametern definierten Typen finden Sie codiert werden, mithilfe der `` ` `` (Gravis)-Zeichens, gefolgt von der nullbasierte Index des Typparameters.</span><span class="sxs-lookup"><span data-stu-id="4b96a-311">Arguments that refer to generic type parameters defined on types are encoded using the `` ` `` (backtick) character followed by the zero-based index of the type parameter.</span></span>
      * <span data-ttu-id="4b96a-312">Generischen Typparametern in Methoden definiert mithilfe von Argumenten verwenden ein Double-Wert-Hochkomma ``` `` ``` statt der `` ` `` für Typen verwendet.</span><span class="sxs-lookup"><span data-stu-id="4b96a-312">Arguments that use generic type parameters defined in methods use a double-backtick ``` `` ``` instead of the `` ` `` used for types.</span></span>
      * <span data-ttu-id="4b96a-313">Argumente, die konstruierte generische Typen verweisen, codiert werden, verwenden den generischen Typ an, gefolgt von `{`, gefolgt von einer durch Trennzeichen getrennte Liste der Argumente des Typs, gefolgt von `}`.</span><span class="sxs-lookup"><span data-stu-id="4b96a-313">Arguments that refer to constructed generic types are encoded using the generic type, followed by `{`, followed by a comma-separated list of type arguments, followed by `}`.</span></span>

### <a name="id-string-examples"></a><span data-ttu-id="4b96a-314">Beispiele für die ID-Zeichenfolge</span><span class="sxs-lookup"><span data-stu-id="4b96a-314">ID string examples</span></span>

<span data-ttu-id="4b96a-315">Die folgenden Beispiele wird jeder zeigen ein Fragment des C#-Code zusammen mit der ID-Zeichenfolge, die von jedem Quellelement für einen Dokumentationskommentar erstellt:</span><span class="sxs-lookup"><span data-stu-id="4b96a-315">The following examples each show a fragment of C# code, along with the ID string produced from each source element capable of having a documentation comment:</span></span>

*  <span data-ttu-id="4b96a-316">Typen werden unter Verwendung des vollqualifizierten Namens, getragenen allgemeine Informationen dargestellt:</span><span class="sxs-lookup"><span data-stu-id="4b96a-316">Types are represented using their fully qualified name, augmented with generic information:</span></span>

   ```csharp
   enum Color { Red, Blue, Green }

   namespace Acme
   {
       interface IProcess {...}

       struct ValueType {...}

       class Widget: IProcess
       {
           public class NestedClass {...}
           public interface IMenuItem {...}
           public delegate void Del(int i);
           public enum Direction { North, South, East, West }
       }

       class MyList<T>
       {
           class Helper<U,V> {...}
       }
   }

   "T:Color"
   "T:Acme.IProcess"
   "T:Acme.ValueType"
   "T:Acme.Widget"
   "T:Acme.Widget.NestedClass"
   "T:Acme.Widget.IMenuItem"
   "T:Acme.Widget.Del"
   "T:Acme.Widget.Direction"
   "T:Acme.MyList`1"
   "T:Acme.MyList`1.Helper`2"
   ```

*  <span data-ttu-id="4b96a-317">Felder werden durch ihre vollqualifizierten Namen dargestellt:</span><span class="sxs-lookup"><span data-stu-id="4b96a-317">Fields are represented by their fully qualified name:</span></span>

   ```csharp
   namespace Acme
   {
       struct ValueType
       {
           private int total;
       }
   
       class Widget: IProcess
       {
           public class NestedClass
           {
               private int value;
           }
   
           private string message;
           private static Color defaultColor;
           private const double PI = 3.14159;
           protected readonly double monthlyAverage;
           private long[] array1;
           private Widget[,] array2;
           private unsafe int *pCount;
           private unsafe float **ppValues;
       }
   }

   "F:Acme.ValueType.total"
   "F:Acme.Widget.NestedClass.value"
   "F:Acme.Widget.message"
   "F:Acme.Widget.defaultColor"
   "F:Acme.Widget.PI"
   "F:Acme.Widget.monthlyAverage"
   "F:Acme.Widget.array1"
   "F:Acme.Widget.array2"
   "F:Acme.Widget.pCount"
   "F:Acme.Widget.ppValues"
   ```

*  <span data-ttu-id="4b96a-318">Konstruktoren.</span><span class="sxs-lookup"><span data-stu-id="4b96a-318">Constructors.</span></span>

   ```csharp
   namespace Acme
   {
       class Widget: IProcess
       {
           static Widget() {...}
           public Widget() {...}
           public Widget(string s) {...}
       }
   }

   "M:Acme.Widget.#cctor"
   "M:Acme.Widget.#ctor"
   "M:Acme.Widget.#ctor(System.String)"
   ```

*  <span data-ttu-id="4b96a-319">Destruktoren.</span><span class="sxs-lookup"><span data-stu-id="4b96a-319">Destructors.</span></span>

   ```csharp
   namespace Acme
   {
       class Widget: IProcess
       {
           ~Widget() {...}
       }
   }
   
   "M:Acme.Widget.Finalize"
   ```

*  <span data-ttu-id="4b96a-320">-Methoden.</span><span class="sxs-lookup"><span data-stu-id="4b96a-320">Methods.</span></span>

   ```csharp
   namespace Acme
   {
       struct ValueType
       {
           public void M(int i) {...}
       }

       class Widget: IProcess
       {
           public class NestedClass
           {
               public void M(int i) {...}
           }

           public static void M0() {...}
           public void M1(char c, out float f, ref ValueType v) {...}
           public void M2(short[] x1, int[,] x2, long[][] x3) {...}
           public void M3(long[][] x3, Widget[][,,] x4) {...}
           public unsafe void M4(char *pc, Color **pf) {...}
           public unsafe void M5(void *pv, double *[][,] pd) {...}
           public void M6(int i, params object[] args) {...}
       }

       class MyList<T>
       {
           public void Test(T t) { }
       }

       class UseList
       {
           public void Process(MyList<int> list) { }
           public MyList<T> GetValues<T>(T inputValue) { return null; }
       }
   }

   "M:Acme.ValueType.M(System.Int32)"
   "M:Acme.Widget.NestedClass.M(System.Int32)"
   "M:Acme.Widget.M0"
   "M:Acme.Widget.M1(System.Char,System.Single@,Acme.ValueType@)"
   "M:Acme.Widget.M2(System.Int16[],System.Int32[0:,0:],System.Int64[][])"
   "M:Acme.Widget.M3(System.Int64[][],Acme.Widget[0:,0:,0:][])"
   "M:Acme.Widget.M4(System.Char*,Color**)"
   "M:Acme.Widget.M5(System.Void*,System.Double*[0:,0:][])"
   "M:Acme.Widget.M6(System.Int32,System.Object[])"
   "M:Acme.MyList`1.Test(`0)"
   "M:Acme.UseList.Process(Acme.MyList{System.Int32})"
   "M:Acme.UseList.GetValues``(``0)"
   ```

*  <span data-ttu-id="4b96a-321">Eigenschaften und Indexer.</span><span class="sxs-lookup"><span data-stu-id="4b96a-321">Properties and indexers.</span></span>

   ```csharp
   namespace Acme
   {
       class Widget: IProcess
       {
           public int Width { get {...} set {...} }
           public int this[int i] { get {...} set {...} }
           public int this[string s, int i] { get {...} set {...} }
       }
   }

   "P:Acme.Widget.Width"
   "P:Acme.Widget.Item(System.Int32)"
   "P:Acme.Widget.Item(System.String,System.Int32)"
   ```

*  <span data-ttu-id="4b96a-322">Ereignisse.</span><span class="sxs-lookup"><span data-stu-id="4b96a-322">Events.</span></span>

   ```csharp
   namespace Acme
   {
       class Widget: IProcess
       {
           public event Del AnEvent;
       }
   }

   "E:Acme.Widget.AnEvent"
   ```

*  <span data-ttu-id="4b96a-323">Unäre Operatoren.</span><span class="sxs-lookup"><span data-stu-id="4b96a-323">Unary operators.</span></span>

   ```csharp
   namespace Acme
   {
       class Widget: IProcess
       {
           public static Widget operator+(Widget x) {...}
       }
   }

   "M:Acme.Widget.op_UnaryPlus(Acme.Widget)"
   ```

   <span data-ttu-id="4b96a-324">Der vollständige Satz von Funktionsnamen für unären Operator verwendet, lautet wie folgt: `op_UnaryPlus`, `op_UnaryNegation`, `op_LogicalNot`, `op_OnesComplement`, `op_Increment`, `op_Decrement`, `op_True`, und `op_False`.</span><span class="sxs-lookup"><span data-stu-id="4b96a-324">The complete set of unary operator function names used is as follows: `op_UnaryPlus`, `op_UnaryNegation`, `op_LogicalNot`, `op_OnesComplement`, `op_Increment`, `op_Decrement`, `op_True`, and `op_False`.</span></span>

*  <span data-ttu-id="4b96a-325">Binäre Operatoren.</span><span class="sxs-lookup"><span data-stu-id="4b96a-325">Binary operators.</span></span>

   ```csharp
   namespace Acme
   {
       class Widget: IProcess
       {
           public static Widget operator+(Widget x1, Widget x2) {...}
       }
   }

   "M:Acme.Widget.op_Addition(Acme.Widget,Acme.Widget)"
   ```

   <span data-ttu-id="4b96a-326">Der vollständige Satz von binären Operator Funktionsnamen verwendet, lautet wie folgt: `op_Addition`, `op_Subtraction`, `op_Multiply`, `op_Division`, `op_Modulus`, `op_BitwiseAnd`, `op_BitwiseOr`, `op_ExclusiveOr`, `op_LeftShift`, `op_RightShift`, `op_Equality`, `op_Inequality`, `op_LessThan`, `op_LessThanOrEqual`, `op_GreaterThan`, und `op_GreaterThanOrEqual`.</span><span class="sxs-lookup"><span data-stu-id="4b96a-326">The complete set of binary operator function names used is as follows: `op_Addition`, `op_Subtraction`, `op_Multiply`, `op_Division`, `op_Modulus`, `op_BitwiseAnd`, `op_BitwiseOr`, `op_ExclusiveOr`, `op_LeftShift`, `op_RightShift`, `op_Equality`, `op_Inequality`, `op_LessThan`, `op_LessThanOrEqual`, `op_GreaterThan`, and `op_GreaterThanOrEqual`.</span></span>

*  <span data-ttu-id="4b96a-327">Konvertierungsoperatoren verfügen über kein nachfolgendes Zeichen "`~`" gefolgt von den Rückgabetyp.</span><span class="sxs-lookup"><span data-stu-id="4b96a-327">Conversion operators have a trailing "`~`" followed by the return type.</span></span>

   ```csharp
   namespace Acme
   {
       class Widget: IProcess
       {
           public static explicit operator int(Widget x) {...}
           public static implicit operator long(Widget x) {...}
       }
   }

   "M:Acme.Widget.op_Explicit(Acme.Widget)~System.Int32"
   "M:Acme.Widget.op_Implicit(Acme.Widget)~System.Int64"
   ```

## <a name="an-example"></a><span data-ttu-id="4b96a-328">Beispiel</span><span class="sxs-lookup"><span data-stu-id="4b96a-328">An example</span></span>

### <a name="c-source-code"></a><span data-ttu-id="4b96a-329">C#-Quellcode</span><span class="sxs-lookup"><span data-stu-id="4b96a-329">C# source code</span></span>

<span data-ttu-id="4b96a-330">Das folgende Beispiel zeigt den Quellcode einer `Point` Klasse:</span><span class="sxs-lookup"><span data-stu-id="4b96a-330">The following example shows the source code of a `Point` class:</span></span>

```csharp
namespace Graphics
{

/// <summary>Class <c>Point</c> models a point in a two-dimensional plane.
/// </summary>
public class Point 
{

    /// <summary>Instance variable <c>x</c> represents the point's
    ///    x-coordinate.</summary>
    private int x;

    /// <summary>Instance variable <c>y</c> represents the point's
    ///    y-coordinate.</summary>
    private int y;

    /// <value>Property <c>X</c> represents the point's x-coordinate.</value>
    public int X
    {
        get { return x; }
        set { x = value; }
    }

    /// <value>Property <c>Y</c> represents the point's y-coordinate.</value>
    public int Y
    {
        get { return y; }
        set { y = value; }
    }

    /// <summary>This constructor initializes the new Point to
    ///    (0,0).</summary>
    public Point() : this(0,0) {}

    /// <summary>This constructor initializes the new Point to
    ///    (<paramref name="xor"/>,<paramref name="yor"/>).</summary>
    /// <param><c>xor</c> is the new Point's x-coordinate.</param>
    /// <param><c>yor</c> is the new Point's y-coordinate.</param>
    public Point(int xor, int yor) {
        X = xor;
        Y = yor;
    }

    /// <summary>This method changes the point's location to
    ///    the given coordinates.</summary>
    /// <param><c>xor</c> is the new x-coordinate.</param>
    /// <param><c>yor</c> is the new y-coordinate.</param>
    /// <see cref="Translate"/>
    public void Move(int xor, int yor) {
        X = xor;
        Y = yor;
    }

    /// <summary>This method changes the point's location by
    ///    the given x- and y-offsets.
    /// <example>For example:
    /// <code>
    ///    Point p = new Point(3,5);
    ///    p.Translate(-1,3);
    /// </code>
    /// results in <c>p</c>'s having the value (2,8).
    /// </example>
    /// </summary>
    /// <param><c>xor</c> is the relative x-offset.</param>
    /// <param><c>yor</c> is the relative y-offset.</param>
    /// <see cref="Move"/>
    public void Translate(int xor, int yor) {
        X += xor;
        Y += yor;
    }

    /// <summary>This method determines whether two Points have the same
    ///    location.</summary>
    /// <param><c>o</c> is the object to be compared to the current object.
    /// </param>
    /// <returns>True if the Points have the same location and they have
    ///    the exact same type; otherwise, false.</returns>
    /// <seealso cref="operator=="/>
    /// <seealso cref="operator!="/>
    public override bool Equals(object o) {
        if (o == null) {
            return false;
        }

        if (this == o) {
            return true;
        }

        if (GetType() == o.GetType()) {
            Point p = (Point)o;
            return (X == p.X) && (Y == p.Y);
        }
        return false;
    }

    /// <summary>Report a point's location as a string.</summary>
    /// <returns>A string representing a point's location, in the form (x,y),
    ///    without any leading, training, or embedded whitespace.</returns>
    public override string ToString() {
        return "(" + X + "," + Y + ")";
    }

    /// <summary>This operator determines whether two Points have the same
    ///    location.</summary>
    /// <param><c>p1</c> is the first Point to be compared.</param>
    /// <param><c>p2</c> is the second Point to be compared.</param>
    /// <returns>True if the Points have the same location and they have
    ///    the exact same type; otherwise, false.</returns>
    /// <seealso cref="Equals"/>
    /// <seealso cref="operator!="/>
    public static bool operator==(Point p1, Point p2) {
        if ((object)p1 == null || (object)p2 == null) {
            return false;
        }

        if (p1.GetType() == p2.GetType()) {
            return (p1.X == p2.X) && (p1.Y == p2.Y);
        }

        return false;
    }

    /// <summary>This operator determines whether two Points have the same
    ///    location.</summary>
    /// <param><c>p1</c> is the first Point to be compared.</param>
    /// <param><c>p2</c> is the second Point to be compared.</param>
    /// <returns>True if the Points do not have the same location and the
    ///    exact same type; otherwise, false.</returns>
    /// <seealso cref="Equals"/>
    /// <seealso cref="operator=="/>
    public static bool operator!=(Point p1, Point p2) {
        return !(p1 == p2);
    }

    /// <summary>This is the entry point of the Point class testing
    /// program.
    /// <para>This program tests each method and operator, and
    /// is intended to be run after any non-trivial maintenance has
    /// been performed on the Point class.</para></summary>
    public static void Main() {
        // class test code goes here
    }
}
}
```

### <a name="resulting-xml"></a><span data-ttu-id="4b96a-331">XML-Ergebnis</span><span class="sxs-lookup"><span data-stu-id="4b96a-331">Resulting XML</span></span>

<span data-ttu-id="4b96a-332">Hier ist die Ausgabe einen Dokumentations-Generator, wenn den Quellcode für die Klasse `Point`, wie oben gezeigt:</span><span class="sxs-lookup"><span data-stu-id="4b96a-332">Here is the output produced by one documentation generator when given the source code for class `Point`, shown above:</span></span>

```xml
<?xml version="1.0"?>
<doc>
    <assembly>
        <name>Point</name>
    </assembly>
    <members>
        <member name="T:Graphics.Point">
            <summary>Class <c>Point</c> models a point in a two-dimensional
            plane.
            </summary>
        </member>

        <member name="F:Graphics.Point.x">
            <summary>Instance variable <c>x</c> represents the point's
            x-coordinate.</summary>
        </member>

        <member name="F:Graphics.Point.y">
            <summary>Instance variable <c>y</c> represents the point's
            y-coordinate.</summary>
        </member>

        <member name="M:Graphics.Point.#ctor">
            <summary>This constructor initializes the new Point to
        (0,0).</summary>
        </member>

        <member name="M:Graphics.Point.#ctor(System.Int32,System.Int32)">
            <summary>This constructor initializes the new Point to
            (<paramref name="xor"/>,<paramref name="yor"/>).</summary>
            <param><c>xor</c> is the new Point's x-coordinate.</param>
            <param><c>yor</c> is the new Point's y-coordinate.</param>
        </member>

        <member name="M:Graphics.Point.Move(System.Int32,System.Int32)">
            <summary>This method changes the point's location to
            the given coordinates.</summary>
            <param><c>xor</c> is the new x-coordinate.</param>
            <param><c>yor</c> is the new y-coordinate.</param>
            <see cref="M:Graphics.Point.Translate(System.Int32,System.Int32)"/>
        </member>

        <member
            name="M:Graphics.Point.Translate(System.Int32,System.Int32)">
            <summary>This method changes the point's location by
            the given x- and y-offsets.
            <example>For example:
            <code>
            Point p = new Point(3,5);
            p.Translate(-1,3);
            </code>
            results in <c>p</c>'s having the value (2,8).
            </example>
            </summary>
            <param><c>xor</c> is the relative x-offset.</param>
            <param><c>yor</c> is the relative y-offset.</param>
            <see cref="M:Graphics.Point.Move(System.Int32,System.Int32)"/>
        </member>

        <member name="M:Graphics.Point.Equals(System.Object)">
            <summary>This method determines whether two Points have the same
            location.</summary>
            <param><c>o</c> is the object to be compared to the current
            object.
            </param>
            <returns>True if the Points have the same location and they have
            the exact same type; otherwise, false.</returns>
            <seealso
      cref="M:Graphics.Point.op_Equality(Graphics.Point,Graphics.Point)"/>
            <seealso
      cref="M:Graphics.Point.op_Inequality(Graphics.Point,Graphics.Point)"/>
        </member>

        <member name="M:Graphics.Point.ToString">
            <summary>Report a point's location as a string.</summary>
            <returns>A string representing a point's location, in the form
            (x,y),
            without any leading, training, or embedded whitespace.</returns>
        </member>

        <member
       name="M:Graphics.Point.op_Equality(Graphics.Point,Graphics.Point)">
            <summary>This operator determines whether two Points have the
            same
            location.</summary>
            <param><c>p1</c> is the first Point to be compared.</param>
            <param><c>p2</c> is the second Point to be compared.</param>
            <returns>True if the Points have the same location and they have
            the exact same type; otherwise, false.</returns>
            <seealso cref="M:Graphics.Point.Equals(System.Object)"/>
            <seealso
     cref="M:Graphics.Point.op_Inequality(Graphics.Point,Graphics.Point)"/>
        </member>

        <member
      name="M:Graphics.Point.op_Inequality(Graphics.Point,Graphics.Point)">
            <summary>This operator determines whether two Points have the
            same
            location.</summary>
            <param><c>p1</c> is the first Point to be compared.</param>
            <param><c>p2</c> is the second Point to be compared.</param>
            <returns>True if the Points do not have the same location and
            the
            exact same type; otherwise, false.</returns>
            <seealso cref="M:Graphics.Point.Equals(System.Object)"/>
            <seealso
      cref="M:Graphics.Point.op_Equality(Graphics.Point,Graphics.Point)"/>
        </member>

        <member name="M:Graphics.Point.Main">
            <summary>This is the entry point of the Point class testing
            program.
            <para>This program tests each method and operator, and
            is intended to be run after any non-trivial maintenance has
            been performed on the Point class.</para></summary>
        </member>

        <member name="P:Graphics.Point.X">
            <value>Property <c>X</c> represents the point's
            x-coordinate.</value>
        </member>

        <member name="P:Graphics.Point.Y">
            <value>Property <c>Y</c> represents the point's
            y-coordinate.</value>
        </member>
    </members>
</doc>
```
