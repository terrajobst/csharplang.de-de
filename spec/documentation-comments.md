---
ms.openlocfilehash: adf81842e3c763c7bbdd3f10bb884dc1207b9099
ms.sourcegitcommit: 0489cb64b7dfb328813d757f4d447a15b85a5851
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 09/11/2019
ms.locfileid: "70912438"
---
# <a name="documentation-comments"></a><span data-ttu-id="10c52-101">Dokumentationskommentare</span><span class="sxs-lookup"><span data-stu-id="10c52-101">Documentation comments</span></span>

<span data-ttu-id="10c52-102">C#stellt einen Mechanismus bereit, mit dem Programmierer Ihren Code mithilfe einer speziellen Kommentar Syntax dokumentieren können, die XML-Text enthält.</span><span class="sxs-lookup"><span data-stu-id="10c52-102">C# provides a mechanism for programmers to document their code using a special comment syntax that contains XML text.</span></span> <span data-ttu-id="10c52-103">In Quell Code Dateien können Kommentare mit einem bestimmten Formular verwendet werden, um ein Tool zum Erstellen von XML aus diesen Kommentaren und den Quell Code Elementen zu leiten, denen Sie vorangestellt sind.</span><span class="sxs-lookup"><span data-stu-id="10c52-103">In source code files, comments having a certain form can be used to direct a tool to produce XML from those comments and the source code elements, which they precede.</span></span> <span data-ttu-id="10c52-104">Kommentare, die diese Syntax verwenden, werden als ***Dokumentations Kommentare***bezeichnet.</span><span class="sxs-lookup"><span data-stu-id="10c52-104">Comments using such syntax are called ***documentation comments***.</span></span> <span data-ttu-id="10c52-105">Sie müssen direkt einem benutzerdefinierten Typ (z. b. einer Klasse, einem Delegaten oder einer Schnittstelle) oder einem Member (z. b. einem Feld, einem Ereignis, einer Eigenschaft oder einer Methode) vorangestellt sein.</span><span class="sxs-lookup"><span data-stu-id="10c52-105">They must immediately precede a user-defined type (such as a class, delegate, or interface) or a member (such as a field, event, property, or method).</span></span> <span data-ttu-id="10c52-106">Das XML-Generierungs Tool wird als ***Dokumentations Generator***bezeichnet.</span><span class="sxs-lookup"><span data-stu-id="10c52-106">The XML generation tool is called the ***documentation generator***.</span></span> <span data-ttu-id="10c52-107">(Dieser Generator kann, aber nicht, der C# Compiler selbst sein.) Die vom Dokumentations Generator erstellte Ausgabe wird als ***Dokumentations Datei***bezeichnet.</span><span class="sxs-lookup"><span data-stu-id="10c52-107">(This generator could be, but need not be, the C# compiler itself.) The output produced by the documentation generator is called the ***documentation file***.</span></span> <span data-ttu-id="10c52-108">Eine Dokumentations Datei wird als Eingabe für einen ***Dokumentations-Viewer***verwendet. ein Tool, mit dem eine visuelle Darstellung der Typinformationen und der zugehörigen Dokumentation erzeugt werden soll.</span><span class="sxs-lookup"><span data-stu-id="10c52-108">A documentation file is used as input to a ***documentation viewer***; a tool intended to produce some sort of visual display of type information and its associated documentation.</span></span>

<span data-ttu-id="10c52-109">Diese Spezifikation schlägt vor, dass ein Satz von Tags in Dokumentations Kommentaren verwendet wird, aber die Verwendung dieser Tags ist nicht erforderlich, und andere Tags können bei Bedarf verwendet werden, solange die Regeln von wohl geformtem XML befolgt werden.</span><span class="sxs-lookup"><span data-stu-id="10c52-109">This specification suggests a set of tags to be used in documentation comments, but use of these tags is not required, and other tags may be used if desired, as long the rules of well-formed XML are followed.</span></span>

## <a name="introduction"></a><span data-ttu-id="10c52-110">Einführung</span><span class="sxs-lookup"><span data-stu-id="10c52-110">Introduction</span></span>

<span data-ttu-id="10c52-111">Kommentare mit einem speziellen Formular können verwendet werden, um ein Tool zum Erstellen von XML aus diesen Kommentaren und den Quell Code Elementen zu leiten, denen Sie vorangestellt sind.</span><span class="sxs-lookup"><span data-stu-id="10c52-111">Comments having a special form can be used to direct a tool to produce XML from those comments and the source code elements, which they precede.</span></span> <span data-ttu-id="10c52-112">Solche Kommentare sind einzeilige Kommentare, die mit drei Schrägstrichen (`///`) oder durch getrennte Kommentare beginnen, die mit einem Schrägstrich und zwei Sternen`/**`beginnen ().</span><span class="sxs-lookup"><span data-stu-id="10c52-112">Such comments are single-line comments that start with three slashes (`///`), or delimited comments that start with a slash and two stars (`/**`).</span></span> <span data-ttu-id="10c52-113">Sie müssen unmittelbar vor einem benutzerdefinierten Typ (z. b. einer Klasse, einem Delegaten oder einer Schnittstelle) oder einem Member (z. b. einem Feld, einem Ereignis, einer Eigenschaft oder einer Methode), dem Sie kommentiert werden, voranstehen.</span><span class="sxs-lookup"><span data-stu-id="10c52-113">They must immediately precede a user-defined type (such as a class, delegate, or interface) or a member (such as a field, event, property, or method) that they annotate.</span></span> <span data-ttu-id="10c52-114">Attribut Abschnitte ([Attribut Spezifikation](attributes.md#attribute-specification)) werden als Teil von Deklarationen betrachtet. Daher müssen Dokumentations Kommentare den Attributen vorangestellt werden, die auf einen Typ oder Member angewendet werden.</span><span class="sxs-lookup"><span data-stu-id="10c52-114">Attribute sections ([Attribute specification](attributes.md#attribute-specification)) are considered part of declarations, so documentation comments must precede attributes applied to a type or member.</span></span>

<span data-ttu-id="10c52-115">__Syntax:__</span><span class="sxs-lookup"><span data-stu-id="10c52-115">__Syntax:__</span></span>

```antlr
single_line_doc_comment
    : '///' input_character*
    ;

delimited_doc_comment
    : '/**' delimited_comment_section* asterisk+ '/'
    ;
```

<span data-ttu-id="10c52-116">Wenn in *einem single_line_doc_comment*ein *leer* Zeichen vorhanden ist, das auf `///` die Zeichen auf jeder der *single_line_doc_comment*s neben dem aktuellen *single_line_doc_comment*folgt, dann  *ein leer* Zeichen ist nicht in der XML-Ausgabe enthalten.</span><span class="sxs-lookup"><span data-stu-id="10c52-116">In a *single_line_doc_comment*, if there is a *whitespace* character following the `///` characters on each of the *single_line_doc_comment*s adjacent to the current *single_line_doc_comment*, then that *whitespace* character is not included in the XML output.</span></span>

<span data-ttu-id="10c52-117">Wenn in einem durch Trennzeichen getrennten doc-Kommentar das erste Zeichen in der zweiten Zeile, das kein Leerzeichen ist, ein Sternchen und das gleiche Muster optionaler leer Raum Zeichen und ein Sternchen am Anfang jeder Zeile innerhalb des durch Trennzeichen getrennten-doc-Kommentars wiederholt wird. dann sind die Zeichen des wiederholten Musters nicht in der XML-Ausgabe enthalten.</span><span class="sxs-lookup"><span data-stu-id="10c52-117">In a delimited-doc-comment, if the first non-whitespace character on the second line is an asterisk and the same pattern of optional whitespace characters and an asterisk character is repeated at the beginning of each of the line within the delimited-doc-comment, then the characters of the repeated pattern are not included in the XML output.</span></span> <span data-ttu-id="10c52-118">Das Muster kann Leerzeichen nach und vor das Sternchen Zeichen enthalten.</span><span class="sxs-lookup"><span data-stu-id="10c52-118">The pattern may include whitespace characters after, as well as before, the asterisk character.</span></span>

<span data-ttu-id="10c52-119">__Beispiel:__</span><span class="sxs-lookup"><span data-stu-id="10c52-119">__Example:__</span></span>

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

<span data-ttu-id="10c52-120">Der Text in den Dokumentations Kommentaren muss gemäß den Regeln von XML (https://www.w3.org/TR/REC-xml) ) wohl geformt sein.</span><span class="sxs-lookup"><span data-stu-id="10c52-120">The text within documentation comments must be well formed according to the rules of XML (https://www.w3.org/TR/REC-xml).</span></span> <span data-ttu-id="10c52-121">Wenn das XML nicht ordnungsgemäß formatiert ist, wird eine Warnung generiert, und die Dokumentations Datei enthält einen Kommentar, der besagt, dass ein Fehler aufgetreten ist.</span><span class="sxs-lookup"><span data-stu-id="10c52-121">If the XML is ill formed, a warning is generated and the documentation file will contain a comment saying that an error was encountered.</span></span>

<span data-ttu-id="10c52-122">Obwohl Entwickler kostenlos einen eigenen Satz von Tags erstellen können, wird ein empfohlener Satz in [empfohlenen Tags](documentation-comments.md#recommended-tags)definiert.</span><span class="sxs-lookup"><span data-stu-id="10c52-122">Although developers are free to create their own set of tags, a recommended set is defined in [Recommended tags](documentation-comments.md#recommended-tags).</span></span> <span data-ttu-id="10c52-123">Einige der empfohlenen Tags haben eine besondere Bedeutung:</span><span class="sxs-lookup"><span data-stu-id="10c52-123">Some of the recommended tags have special meanings:</span></span>

*  <span data-ttu-id="10c52-124">Das `<param>` -Tag wird verwendet, um Parameter zu beschreiben.</span><span class="sxs-lookup"><span data-stu-id="10c52-124">The `<param>` tag is used to describe parameters.</span></span> <span data-ttu-id="10c52-125">Wenn ein solches Tag verwendet wird, muss der Dokumentations Generator überprüfen, ob der angegebene Parameter vorhanden ist und dass alle Parameter in den Dokumentations Kommentaren beschrieben werden.</span><span class="sxs-lookup"><span data-stu-id="10c52-125">If such a tag is used, the documentation generator must verify that the specified parameter exists and that all parameters are described in documentation comments.</span></span> <span data-ttu-id="10c52-126">Wenn diese Überprüfung fehlschlägt, gibt der Dokumentations Generator eine Warnung aus.</span><span class="sxs-lookup"><span data-stu-id="10c52-126">If such verification fails, the documentation generator issues a warning.</span></span>
*  <span data-ttu-id="10c52-127">Das `cref`-Attribut kann an jedes Tag angefügt werden, um einen Verweis auf ein Codeelement bereitzustellen.</span><span class="sxs-lookup"><span data-stu-id="10c52-127">The `cref` attribute can be attached to any tag to provide a reference to a code element.</span></span> <span data-ttu-id="10c52-128">Der Dokumentations Generator muss überprüfen, ob dieses Code Element vorhanden ist.</span><span class="sxs-lookup"><span data-stu-id="10c52-128">The documentation generator must verify that this code element exists.</span></span> <span data-ttu-id="10c52-129">Wenn die Überprüfung fehlschlägt, gibt der Dokumentations Generator eine Warnung aus.</span><span class="sxs-lookup"><span data-stu-id="10c52-129">If the verification fails, the documentation generator issues a warning.</span></span> <span data-ttu-id="10c52-130">Bei der Suche nach einem Namen, der `cref` in einem-Attribut beschrieben wird, muss der Dokumentations-Generator `using` die Sichtbarkeit von Namespaces gemäß den Anweisungen im Quellcode berücksichtigen.</span><span class="sxs-lookup"><span data-stu-id="10c52-130">When looking for a name described in a `cref` attribute, the documentation generator must respect namespace visibility according to `using` statements appearing within the source code.</span></span> <span data-ttu-id="10c52-131">Für Code Elemente, die generisch sind, kann die normale generische Syntax (d`List<T>`. h. "") nicht verwendet werden, da Sie ungültiges XML erzeugt.</span><span class="sxs-lookup"><span data-stu-id="10c52-131">For code elements that are generic, the normal generic syntax (that is, "`List<T>`") cannot be used because it produces invalid XML.</span></span> <span data-ttu-id="10c52-132">Geschweifte Klammern können anstelle von Klammern (`List{T}`d. h. "") verwendet werden, oder die XML-Escapesyntax kann verwendet werden (d. h. "`List&lt;T&gt;`").</span><span class="sxs-lookup"><span data-stu-id="10c52-132">Braces can be used instead of brackets (that is, "`List{T}`"), or the XML escape syntax can be used (that is, "`List&lt;T&gt;`").</span></span>
*  <span data-ttu-id="10c52-133">Das `<summary>` -Tag soll von einem Dokumentations-Viewer verwendet werden, um zusätzliche Informationen zu einem Typ oder Member anzuzeigen.</span><span class="sxs-lookup"><span data-stu-id="10c52-133">The `<summary>` tag is intended to be used by a documentation viewer to display additional information about a type or member.</span></span>
*  <span data-ttu-id="10c52-134">Das `<include>` -Tag enthält Informationen aus einer externen XML-Datei.</span><span class="sxs-lookup"><span data-stu-id="10c52-134">The `<include>` tag includes information from an external XML file.</span></span>

<span data-ttu-id="10c52-135">Beachten Sie, dass die Dokumentations Datei keine vollständigen Informationen über den Typ und die Member bereitstellt (z. b. enthält Sie keine Typinformationen).</span><span class="sxs-lookup"><span data-stu-id="10c52-135">Note carefully that the documentation file does not provide full information about the type and members (for example, it does not contain any type information).</span></span> <span data-ttu-id="10c52-136">Um solche Informationen zu einem Typ oder Member zu erhalten, muss die Dokumentations Datei in Verbindung mit der Reflektion für den tatsächlichen Typ oder Member verwendet werden.</span><span class="sxs-lookup"><span data-stu-id="10c52-136">To get such information about a type or member, the documentation file must be used in conjunction with reflection on the actual type or member.</span></span>

## <a name="recommended-tags"></a><span data-ttu-id="10c52-137">Empfohlene Tags</span><span class="sxs-lookup"><span data-stu-id="10c52-137">Recommended tags</span></span>

<span data-ttu-id="10c52-138">Der Dokumentations-Generator muss alle Tags akzeptieren und verarbeiten, die gemäß den XML-Regeln gültig sind.</span><span class="sxs-lookup"><span data-stu-id="10c52-138">The documentation generator must accept and process any tag that is valid according to the rules of XML.</span></span> <span data-ttu-id="10c52-139">Die folgenden Tags stellen häufig verwendete Funktionen in der Benutzerdokumentation bereit.</span><span class="sxs-lookup"><span data-stu-id="10c52-139">The following tags provide commonly used functionality in user documentation.</span></span> <span data-ttu-id="10c52-140">(Natürlich sind andere Tags möglich.)</span><span class="sxs-lookup"><span data-stu-id="10c52-140">(Of course, other tags are possible.)</span></span>


| <span data-ttu-id="10c52-141">__Tag__</span><span class="sxs-lookup"><span data-stu-id="10c52-141">__Tag__</span></span>          | <span data-ttu-id="10c52-142">__Bereich__</span><span class="sxs-lookup"><span data-stu-id="10c52-142">__Section__</span></span>                                            | <span data-ttu-id="10c52-143">__Darin__</span><span class="sxs-lookup"><span data-stu-id="10c52-143">__Purpose__</span></span>                                            |
|------------------|--------------------------------------------------------|--------------------------------------------------------|
| `<c>`            | [`<c>`](documentation-comments.md#c)                   | <span data-ttu-id="10c52-144">Festlegen von Text in einer Code ähnlichen Schriftart</span><span class="sxs-lookup"><span data-stu-id="10c52-144">Set text in a code-like font</span></span>                           | 
| `<code>`         | [`<code>`](documentation-comments.md#code)             | <span data-ttu-id="10c52-145">Mindestens eine Zeile des Quellcodes oder der Programmausgabe festlegen</span><span class="sxs-lookup"><span data-stu-id="10c52-145">Set one or more lines of source code or program output</span></span> |
| `<example>`      | [`<example>`](documentation-comments.md#example)       | <span data-ttu-id="10c52-146">Anzeigen eines Beispiels</span><span class="sxs-lookup"><span data-stu-id="10c52-146">Indicate an example</span></span>                                    |
| `<exception>`    | [`<exception>`](documentation-comments.md#exception)   | <span data-ttu-id="10c52-147">Identifiziert die Ausnahmen, die von einer Methode ausgelöst werden können.</span><span class="sxs-lookup"><span data-stu-id="10c52-147">Identifies the exceptions a method can throw</span></span>           |
| `<include>`      | [`<include>`](documentation-comments.md#include)       | <span data-ttu-id="10c52-148">Enthält XML aus einer externen Datei.</span><span class="sxs-lookup"><span data-stu-id="10c52-148">Includes XML from an external file</span></span>                     |
| `<list>`         | [`<list>`](documentation-comments.md#list)             | <span data-ttu-id="10c52-149">Erstellen einer Liste oder Tabelle</span><span class="sxs-lookup"><span data-stu-id="10c52-149">Create a list or table</span></span>                                 |
| `<para>`         | [`<para>`](documentation-comments.md#para)             | <span data-ttu-id="10c52-150">Struktur zum Hinzufügen von Text zulassen</span><span class="sxs-lookup"><span data-stu-id="10c52-150">Permit structure to be added to text</span></span>                   |
| `<param>`        | [`<param>`](documentation-comments.md#param)           | <span data-ttu-id="10c52-151">Beschreibt einen Parameter für eine Methode oder einen Konstruktor.</span><span class="sxs-lookup"><span data-stu-id="10c52-151">Describe a parameter for a method or constructor</span></span>       |
| `<paramref>`     | [`<paramref>`](documentation-comments.md#paramref)     | <span data-ttu-id="10c52-152">Erkennen, dass ein Wort ein Parameter Name ist</span><span class="sxs-lookup"><span data-stu-id="10c52-152">Identify that a word is a parameter name</span></span>               |
| `<permission>`   | [`<permission>`](documentation-comments.md#permission) | <span data-ttu-id="10c52-153">Dokumentieren der Sicherheits Barrierefreiheit eines Mitglieds</span><span class="sxs-lookup"><span data-stu-id="10c52-153">Document the security accessibility of a member</span></span>        |
| `<remarks>`      | [`<remarks>`](documentation-comments.md#remarks)       | <span data-ttu-id="10c52-154">Beschreiben zusätzlicher Informationen zu einem Typ</span><span class="sxs-lookup"><span data-stu-id="10c52-154">Describe additional information about a type</span></span>           |
| `<returns>`      | [`<returns>`](documentation-comments.md#returns)       | <span data-ttu-id="10c52-155">Beschreiben des Rückgabewerts einer Methode</span><span class="sxs-lookup"><span data-stu-id="10c52-155">Describe the return value of a method</span></span>                  |
| `<see>`          | [`<see>`](documentation-comments.md#see)               | <span data-ttu-id="10c52-156">Link angeben</span><span class="sxs-lookup"><span data-stu-id="10c52-156">Specify a link</span></span>                                         |
| `<seealso>`      | [`<seealso>`](documentation-comments.md#seealso)       | <span data-ttu-id="10c52-157">Einen Eintrag "Siehe auch" generieren</span><span class="sxs-lookup"><span data-stu-id="10c52-157">Generate a See Also entry</span></span>                              |
| `<summary>`      | [`<summary>`](documentation-comments.md#summary)       | <span data-ttu-id="10c52-158">Beschreibt einen Typ oder einen Member eines Typs.</span><span class="sxs-lookup"><span data-stu-id="10c52-158">Describe a type or a member of a type</span></span>                  |
| `<value>`        | [`<value>`](documentation-comments.md#value)           | <span data-ttu-id="10c52-159">Beschreiben einer Eigenschaft</span><span class="sxs-lookup"><span data-stu-id="10c52-159">Describe a property</span></span>                                    |
| `<typeparam>`    |                                                        | <span data-ttu-id="10c52-160">Beschreiben eines generischen Typparameters</span><span class="sxs-lookup"><span data-stu-id="10c52-160">Describe a generic type parameter</span></span>                      |
| `<typeparamref>` |                                                        | <span data-ttu-id="10c52-161">Erkennen, dass ein Wort ein Typparameter Name ist</span><span class="sxs-lookup"><span data-stu-id="10c52-161">Identify that a word is a type parameter name</span></span>          |

### `<c>`

<span data-ttu-id="10c52-162">Dieses Tag stellt einen Mechanismus bereit, mit dem angegeben wird, dass ein Textfragment innerhalb einer Beschreibung in einer speziellen Schriftart, z. b. der für einen Codeblock verwendeten, festgelegt werden soll.</span><span class="sxs-lookup"><span data-stu-id="10c52-162">This tag provides a mechanism to indicate that a fragment of text within a description should be set in a special font such as that used for a block of code.</span></span> <span data-ttu-id="10c52-163">Verwenden `<code>` Sie für Zeilen des tatsächlichen Codes ([`<code>`](documentation-comments.md#code)).</span><span class="sxs-lookup"><span data-stu-id="10c52-163">For lines of actual code, use `<code>` ([`<code>`](documentation-comments.md#code)).</span></span>

<span data-ttu-id="10c52-164">__Syntax:__</span><span class="sxs-lookup"><span data-stu-id="10c52-164">__Syntax:__</span></span>

```xml
<c>text</c>
```

<span data-ttu-id="10c52-165">__Beispiel:__</span><span class="sxs-lookup"><span data-stu-id="10c52-165">__Example:__</span></span>

```csharp
/// <summary>Class <c>Point</c> models a point in a two-dimensional
/// plane.</summary>

public class Point 
{
    // ...
}
```

### `<code>`

<span data-ttu-id="10c52-166">Dieses Tag wird verwendet, um eine oder mehrere Zeilen des Quellcodes oder der Programmausgabe in einer speziellen Schriftart festzulegen.</span><span class="sxs-lookup"><span data-stu-id="10c52-166">This tag is used to set one or more lines of source code or program output in some special font.</span></span> <span data-ttu-id="10c52-167">Verwenden `<c>` Sie bei kleinen Code Fragmenten in der Erzählung[`<c>`](documentation-comments.md#c)().</span><span class="sxs-lookup"><span data-stu-id="10c52-167">For small code fragments in narrative, use `<c>` ([`<c>`](documentation-comments.md#c)).</span></span>

<span data-ttu-id="10c52-168">__Syntax:__</span><span class="sxs-lookup"><span data-stu-id="10c52-168">__Syntax:__</span></span>

```xml
<code>source code or program output</code>
```

<span data-ttu-id="10c52-169">__Beispiel:__</span><span class="sxs-lookup"><span data-stu-id="10c52-169">__Example:__</span></span>

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

<span data-ttu-id="10c52-170">Dieses Tag ermöglicht Beispielcode in einem Kommentar, um anzugeben, wie eine Methode oder ein anderes Bibliothekselement verwendet werden kann.</span><span class="sxs-lookup"><span data-stu-id="10c52-170">This tag allows example code within a comment, to specify how a method or other library member may be used.</span></span> <span data-ttu-id="10c52-171">Normalerweise würde dies auch die Verwendung des-Tags `<code>` ([`<code>`](documentation-comments.md#code)) beinhalten.</span><span class="sxs-lookup"><span data-stu-id="10c52-171">Ordinarily, this would also involve use of the tag `<code>` ([`<code>`](documentation-comments.md#code)) as well.</span></span>

<span data-ttu-id="10c52-172">__Syntax:__</span><span class="sxs-lookup"><span data-stu-id="10c52-172">__Syntax:__</span></span>

```xml
<example>description</example>
```

<span data-ttu-id="10c52-173">__Beispiel:__</span><span class="sxs-lookup"><span data-stu-id="10c52-173">__Example:__</span></span>

<span data-ttu-id="10c52-174">Ein `<code>` Beispiel[`<code>`](documentation-comments.md#code)finden Sie unter ().</span><span class="sxs-lookup"><span data-stu-id="10c52-174">See `<code>` ([`<code>`](documentation-comments.md#code)) for an example.</span></span>

### `<exception>`

<span data-ttu-id="10c52-175">Dieses Tag bietet eine Möglichkeit, die Ausnahmen zu dokumentieren, die eine Methode auslösen kann.</span><span class="sxs-lookup"><span data-stu-id="10c52-175">This tag provides a way to document the exceptions a method can throw.</span></span>

<span data-ttu-id="10c52-176">__Syntax:__</span><span class="sxs-lookup"><span data-stu-id="10c52-176">__Syntax:__</span></span>

```xml
<exception cref="member">description</exception>
```

<span data-ttu-id="10c52-177">wo</span><span class="sxs-lookup"><span data-stu-id="10c52-177">where</span></span>

* <span data-ttu-id="10c52-178">`member`der Name eines Members.</span><span class="sxs-lookup"><span data-stu-id="10c52-178">`member` is the name of a member.</span></span> <span data-ttu-id="10c52-179">Der Dokumentations Generator überprüft, ob der angegebene Member vorhanden ist `member` , und übersetzt ihn in der Dokumentations Datei in den kanonischen Elementnamen.</span><span class="sxs-lookup"><span data-stu-id="10c52-179">The documentation generator checks that the given member exists and translates `member` to the canonical element name in the documentation file.</span></span>
* <span data-ttu-id="10c52-180">`description`eine Beschreibung der Umstände, in denen die Ausnahme ausgelöst wird.</span><span class="sxs-lookup"><span data-stu-id="10c52-180">`description` is a description of the circumstances in which the exception is thrown.</span></span>

<span data-ttu-id="10c52-181">__Beispiel:__</span><span class="sxs-lookup"><span data-stu-id="10c52-181">__Example:__</span></span>

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

<span data-ttu-id="10c52-182">Dieses Tag ermöglicht das Einschließen von Informationen aus einem XML-Dokument, das für die Quell Code Datei extern ist.</span><span class="sxs-lookup"><span data-stu-id="10c52-182">This tag allows including information from an XML document that is external to the source code file.</span></span> <span data-ttu-id="10c52-183">Die externe Datei muss ein wohl geformtes XML-Dokument sein, und es wird ein XPath-Ausdruck auf das Dokument angewendet, um anzugeben, welches XML-Dokument in diesem Dokument enthalten sein soll.</span><span class="sxs-lookup"><span data-stu-id="10c52-183">The external file must be a well-formed XML document, and an XPath expression is applied to that document to specify what XML from that document to include.</span></span> <span data-ttu-id="10c52-184">Das `<include>` Tag wird dann durch den ausgewählten XML-Code aus dem externen Dokument ersetzt.</span><span class="sxs-lookup"><span data-stu-id="10c52-184">The `<include>` tag is then replaced with the selected XML from the external document.</span></span>

<span data-ttu-id="10c52-185">__Syntax:__</span><span class="sxs-lookup"><span data-stu-id="10c52-185">__Syntax:__</span></span>

```
<include file="filename" path="xpath" />
```

<span data-ttu-id="10c52-186">wo</span><span class="sxs-lookup"><span data-stu-id="10c52-186">where</span></span>

* <span data-ttu-id="10c52-187">`filename`der Dateiname einer externen XML-Datei.</span><span class="sxs-lookup"><span data-stu-id="10c52-187">`filename` is the file name of an external XML file.</span></span> <span data-ttu-id="10c52-188">Der Dateiname wird relativ zur Datei interpretiert, die das include-Tag enthält.</span><span class="sxs-lookup"><span data-stu-id="10c52-188">The file name is interpreted relative to the file that contains the include tag.</span></span>
* <span data-ttu-id="10c52-189">`xpath`ein XPath-Ausdruck, der einen Teil des XML-Codes in der externen XML-Datei auswählt.</span><span class="sxs-lookup"><span data-stu-id="10c52-189">`xpath` is an XPath expression that selects some of the XML in the external XML file.</span></span>

<span data-ttu-id="10c52-190">__Beispiel:__</span><span class="sxs-lookup"><span data-stu-id="10c52-190">__Example:__</span></span>

<span data-ttu-id="10c52-191">Wenn der Quellcode eine Deklaration wie z. b. enthält:</span><span class="sxs-lookup"><span data-stu-id="10c52-191">If the source code contained a declaration like:</span></span>

```csharp
/// <include file="docs.xml" path='extradoc/class[@name="IntList"]/*' />
public class IntList { ... }
```

<span data-ttu-id="10c52-192">die externe Datei "docs. xml" wies den folgenden Inhalt auf:</span><span class="sxs-lookup"><span data-stu-id="10c52-192">and the external file "docs.xml" had the following contents:</span></span>

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

<span data-ttu-id="10c52-193">dann wird dieselbe Dokumentation ausgegeben, als wäre der Quellcode wie folgt:</span><span class="sxs-lookup"><span data-stu-id="10c52-193">then the same documentation is output as if the source code contained:</span></span>

```csharp
/// <summary>
///    Contains a list of integers.
/// </summary>
public class IntList { ... }
```

### `<list>`

<span data-ttu-id="10c52-194">Dieses Tag wird verwendet, um eine Liste oder eine Tabelle mit Elementen zu erstellen.</span><span class="sxs-lookup"><span data-stu-id="10c52-194">This tag is used to create a list or table of items.</span></span> <span data-ttu-id="10c52-195">Sie kann einen `<listheader>` -Block enthalten, um die Überschriften Zeile einer Tabelle oder einer Definitionsliste zu definieren.</span><span class="sxs-lookup"><span data-stu-id="10c52-195">It may contain a `<listheader>` block to define the heading row of either a table or definition list.</span></span> <span data-ttu-id="10c52-196">(Beim Definieren einer Tabelle muss nur ein Eintrag für `term` in der Überschrift angegeben werden.)</span><span class="sxs-lookup"><span data-stu-id="10c52-196">(When defining a table, only an entry for `term` in the heading need be supplied.)</span></span>

<span data-ttu-id="10c52-197">Jedes Element in der Liste wird mit einem `<item>` -Block angegeben.</span><span class="sxs-lookup"><span data-stu-id="10c52-197">Each item in the list is specified with an `<item>` block.</span></span> <span data-ttu-id="10c52-198">Beim Erstellen einer Definitionsliste müssen sowohl `term` als `description` auch angegeben werden.</span><span class="sxs-lookup"><span data-stu-id="10c52-198">When creating a definition list, both `term` and `description` must be specified.</span></span> <span data-ttu-id="10c52-199">Allerdings muss für eine Tabelle, eine aufzählige Liste oder eine nummerierte Liste nur `description` angegeben werden.</span><span class="sxs-lookup"><span data-stu-id="10c52-199">However, for a table, bulleted list, or numbered list, only `description` need be specified.</span></span>

<span data-ttu-id="10c52-200">__Syntax:__</span><span class="sxs-lookup"><span data-stu-id="10c52-200">__Syntax:__</span></span>

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

<span data-ttu-id="10c52-201">wo</span><span class="sxs-lookup"><span data-stu-id="10c52-201">where</span></span>

* <span data-ttu-id="10c52-202">`term`der Begriff, der definiert werden soll, dessen Definition `description`in ist.</span><span class="sxs-lookup"><span data-stu-id="10c52-202">`term` is the term to define, whose definition is in `description`.</span></span>
* <span data-ttu-id="10c52-203">`description`ist entweder ein Element in einer Aufzählungs Liste oder nummerierte Liste oder die `term`Definition einer.</span><span class="sxs-lookup"><span data-stu-id="10c52-203">`description` is either an item in a bullet or numbered list, or the definition of a `term`.</span></span>

<span data-ttu-id="10c52-204">__Beispiel:__</span><span class="sxs-lookup"><span data-stu-id="10c52-204">__Example:__</span></span>

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

<span data-ttu-id="10c52-205">Dieses Tag dient zur Verwendung innerhalb anderer Tags, z `<summary>` . b. ( `<returns>` [`<remarks>`](documentation-comments.md#remarks))[`<returns>`](documentation-comments.md#returns)oder (), und lässt zu, dass die Struktur dem Text hinzugefügt wird.</span><span class="sxs-lookup"><span data-stu-id="10c52-205">This tag is for use inside other tags, such as `<summary>` ([`<remarks>`](documentation-comments.md#remarks)) or `<returns>` ([`<returns>`](documentation-comments.md#returns)), and permits structure to be added to text.</span></span>

<span data-ttu-id="10c52-206">__Syntax:__</span><span class="sxs-lookup"><span data-stu-id="10c52-206">__Syntax:__</span></span>

```xml
<para>content</para>
```

<span data-ttu-id="10c52-207">dabei `content` ist der Text des Absatzes.</span><span class="sxs-lookup"><span data-stu-id="10c52-207">where `content` is the text of the paragraph.</span></span>

<span data-ttu-id="10c52-208">__Beispiel:__</span><span class="sxs-lookup"><span data-stu-id="10c52-208">__Example:__</span></span>

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

<span data-ttu-id="10c52-209">Dieses Tag wird verwendet, um einen Parameter für eine Methode, einen Konstruktor oder einen Indexer zu beschreiben.</span><span class="sxs-lookup"><span data-stu-id="10c52-209">This tag is used to describe a parameter for a method, constructor, or indexer.</span></span>

<span data-ttu-id="10c52-210">__Syntax:__</span><span class="sxs-lookup"><span data-stu-id="10c52-210">__Syntax:__</span></span>

```xml
<param name="name">description</param>
```

<span data-ttu-id="10c52-211">wo</span><span class="sxs-lookup"><span data-stu-id="10c52-211">where</span></span>

* <span data-ttu-id="10c52-212">`name`der Name des Parameters.</span><span class="sxs-lookup"><span data-stu-id="10c52-212">`name` is the name of the parameter.</span></span>
* <span data-ttu-id="10c52-213">`description`eine Beschreibung des Parameters.</span><span class="sxs-lookup"><span data-stu-id="10c52-213">`description` is a description of the parameter.</span></span>

<span data-ttu-id="10c52-214">__Beispiel:__</span><span class="sxs-lookup"><span data-stu-id="10c52-214">__Example:__</span></span>

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

<span data-ttu-id="10c52-215">Dieses Tag wird verwendet, um anzugeben, dass ein Wort ein Parameter ist.</span><span class="sxs-lookup"><span data-stu-id="10c52-215">This tag is used to indicate that a word is a parameter.</span></span> <span data-ttu-id="10c52-216">Die Dokumentations Datei kann verarbeitet werden, um diesen Parameter auf unterschiedliche Weise zu formatieren.</span><span class="sxs-lookup"><span data-stu-id="10c52-216">The documentation file can be processed to format this parameter in some distinct way.</span></span>

<span data-ttu-id="10c52-217">__Syntax:__</span><span class="sxs-lookup"><span data-stu-id="10c52-217">__Syntax:__</span></span>

```xml
<paramref name="name"/>
```

<span data-ttu-id="10c52-218">dabei `name` ist der Name des Parameters.</span><span class="sxs-lookup"><span data-stu-id="10c52-218">where `name` is the name of the parameter.</span></span>

<span data-ttu-id="10c52-219">__Beispiel:__</span><span class="sxs-lookup"><span data-stu-id="10c52-219">__Example:__</span></span>

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

<span data-ttu-id="10c52-220">Mit diesem Tag kann die Sicherheits Barrierefreiheit eines Members dokumentiert werden.</span><span class="sxs-lookup"><span data-stu-id="10c52-220">This tag allows the security accessibility of a member to be documented.</span></span>

<span data-ttu-id="10c52-221">__Syntax:__</span><span class="sxs-lookup"><span data-stu-id="10c52-221">__Syntax:__</span></span>

```xml
<permission cref="member">description</permission>
```

<span data-ttu-id="10c52-222">wo</span><span class="sxs-lookup"><span data-stu-id="10c52-222">where</span></span>

* <span data-ttu-id="10c52-223">`member`der Name eines Members.</span><span class="sxs-lookup"><span data-stu-id="10c52-223">`member` is the name of a member.</span></span> <span data-ttu-id="10c52-224">Der Dokumentations Generator überprüft, ob das angegebene Code Element vorhanden ist, und übersetzt den *Member* in den kanonischen Elementnamen in der Dokumentations Datei.</span><span class="sxs-lookup"><span data-stu-id="10c52-224">The documentation generator checks that the given code element exists and translates *member* to the canonical element name in the documentation file.</span></span>
* <span data-ttu-id="10c52-225">`description`eine Beschreibung des Zugriffs auf den Member.</span><span class="sxs-lookup"><span data-stu-id="10c52-225">`description` is a description of the access to the member.</span></span>

<span data-ttu-id="10c52-226">__Beispiel:__</span><span class="sxs-lookup"><span data-stu-id="10c52-226">__Example:__</span></span>

```csharp
/// <permission cref="System.Security.PermissionSet">Everyone can
/// access this method.</permission>

public static void Test() {
    // ...
}
```

### `<remarks>`

<span data-ttu-id="10c52-227">Dieses Tag wird verwendet, um zusätzliche Informationen zu einem Typ anzugeben.</span><span class="sxs-lookup"><span data-stu-id="10c52-227">This tag is used to specify extra information about a type.</span></span> <span data-ttu-id="10c52-228">(Verwenden `<summary>` Sie[`<summary>`](documentation-comments.md#summary)(), um den Typ selbst und die Member eines Typs zu beschreiben.)</span><span class="sxs-lookup"><span data-stu-id="10c52-228">(Use `<summary>` ([`<summary>`](documentation-comments.md#summary)) to describe the type itself and the members of a type.)</span></span>

<span data-ttu-id="10c52-229">__Syntax:__</span><span class="sxs-lookup"><span data-stu-id="10c52-229">__Syntax:__</span></span>

```xml
<remarks>description</remarks>
```

<span data-ttu-id="10c52-230">dabei `description` steht für den Text der Anmerkung.</span><span class="sxs-lookup"><span data-stu-id="10c52-230">where `description` is the text of the remark.</span></span>

<span data-ttu-id="10c52-231">__Beispiel:__</span><span class="sxs-lookup"><span data-stu-id="10c52-231">__Example:__</span></span>

```csharp
/// <summary>Class <c>Point</c> models a point in a 
/// two-dimensional plane.</summary>
/// <remarks>Uses polar coordinates</remarks>
public class Point 
{
    // ...
}
```

### `<returns>`

<span data-ttu-id="10c52-232">Dieses Tag wird verwendet, um den Rückgabewert einer Methode zu beschreiben.</span><span class="sxs-lookup"><span data-stu-id="10c52-232">This tag is used to describe the return value of a method.</span></span>

<span data-ttu-id="10c52-233">__Syntax:__</span><span class="sxs-lookup"><span data-stu-id="10c52-233">__Syntax:__</span></span>

```xml
<returns>description</returns>
```

<span data-ttu-id="10c52-234">dabei `description` ist eine Beschreibung des Rückgabewerts.</span><span class="sxs-lookup"><span data-stu-id="10c52-234">where `description` is a description of the return value.</span></span>

<span data-ttu-id="10c52-235">__Beispiel:__</span><span class="sxs-lookup"><span data-stu-id="10c52-235">__Example:__</span></span>

```csharp
/// <summary>Report a point's location as a string.</summary>
/// <returns>A string representing a point's location, in the form (x,y),
///    without any leading, trailing, or embedded whitespace.</returns>
public override string ToString() {
    return "(" + X + "," + Y + ")";
}
```

### `<see>`

<span data-ttu-id="10c52-236">Dieses Tag ermöglicht das Angeben eines Links in Text.</span><span class="sxs-lookup"><span data-stu-id="10c52-236">This tag allows a link to be specified within text.</span></span> <span data-ttu-id="10c52-237">Verwenden `<seealso>` Sie[`<seealso>`](documentation-comments.md#seealso)(), um Text anzugeben, der in einem Abschnitt Siehe auch angezeigt werden soll.</span><span class="sxs-lookup"><span data-stu-id="10c52-237">Use `<seealso>` ([`<seealso>`](documentation-comments.md#seealso)) to indicate text that is to appear in a See Also section.</span></span>

<span data-ttu-id="10c52-238">__Syntax:__</span><span class="sxs-lookup"><span data-stu-id="10c52-238">__Syntax:__</span></span>

```xml
<see cref="member"/>
```

<span data-ttu-id="10c52-239">dabei `member` ist der Name eines Members.</span><span class="sxs-lookup"><span data-stu-id="10c52-239">where `member` is the name of a member.</span></span> <span data-ttu-id="10c52-240">Der Dokumentations Generator überprüft, ob das angegebene Code Element vorhanden ist, und ändert den *Member* in der generierten Dokumentations Datei in den Elementnamen.</span><span class="sxs-lookup"><span data-stu-id="10c52-240">The documentation generator checks that the given code element exists and changes *member* to the element name in the generated documentation file.</span></span>

<span data-ttu-id="10c52-241">__Beispiel:__</span><span class="sxs-lookup"><span data-stu-id="10c52-241">__Example:__</span></span>

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

<span data-ttu-id="10c52-242">Dieses Tag ermöglicht das Generieren eines Eintrags für den Abschnitt "Siehe auch".</span><span class="sxs-lookup"><span data-stu-id="10c52-242">This tag allows an entry to be generated for the See Also section.</span></span> <span data-ttu-id="10c52-243">Verwenden `<see>` Sie[`<see>`](documentation-comments.md#see)(), um einen Link aus Text anzugeben.</span><span class="sxs-lookup"><span data-stu-id="10c52-243">Use `<see>` ([`<see>`](documentation-comments.md#see)) to specify a link from within text.</span></span>

<span data-ttu-id="10c52-244">__Syntax:__</span><span class="sxs-lookup"><span data-stu-id="10c52-244">__Syntax:__</span></span>

```xml
<seealso cref="member"/>
```

<span data-ttu-id="10c52-245">dabei `member` ist der Name eines Members.</span><span class="sxs-lookup"><span data-stu-id="10c52-245">where `member` is the name of a member.</span></span> <span data-ttu-id="10c52-246">Der Dokumentations Generator überprüft, ob das angegebene Code Element vorhanden ist, und ändert den *Member* in der generierten Dokumentations Datei in den Elementnamen.</span><span class="sxs-lookup"><span data-stu-id="10c52-246">The documentation generator checks that the given code element exists and changes *member* to the element name in the generated documentation file.</span></span>

<span data-ttu-id="10c52-247">__Beispiel:__</span><span class="sxs-lookup"><span data-stu-id="10c52-247">__Example:__</span></span>

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

Dieses Tag kann verwendet werden, um einen Typ oder einen Member eines Typs zu beschreiben. <span data-ttu-id="10c52-249">Verwenden `<remarks>` Sie[`<remarks>`](documentation-comments.md#remarks)(), um den Typ selbst zu beschreiben.</span><span class="sxs-lookup"><span data-stu-id="10c52-249">Use `<remarks>` ([`<remarks>`](documentation-comments.md#remarks)) to describe the type itself.</span></span>

<span data-ttu-id="10c52-250">__Syntax:__</span><span class="sxs-lookup"><span data-stu-id="10c52-250">__Syntax:__</span></span>

```xml
<summary>description</summary>
```

<span data-ttu-id="10c52-251">dabei `description` ist eine Zusammenfassung des Typs oder Members.</span><span class="sxs-lookup"><span data-stu-id="10c52-251">where `description` is a summary of the type or member.</span></span>

<span data-ttu-id="10c52-252">__Beispiel:__</span><span class="sxs-lookup"><span data-stu-id="10c52-252">__Example:__</span></span>

```csharp
/// <summary>This constructor initializes the new Point to (0,0).</summary>
public Point() : this(0,0) {
}
```

### `<value>`

<span data-ttu-id="10c52-253">Dieses Tag ermöglicht die Beschreibung einer Eigenschaft.</span><span class="sxs-lookup"><span data-stu-id="10c52-253">This tag allows a property to be described.</span></span>

<span data-ttu-id="10c52-254">__Syntax:__</span><span class="sxs-lookup"><span data-stu-id="10c52-254">__Syntax:__</span></span>

```xml
<value>property description</value>
```

<span data-ttu-id="10c52-255">dabei `property description` ist eine Beschreibung der Eigenschaft.</span><span class="sxs-lookup"><span data-stu-id="10c52-255">where `property description` is a description for the property.</span></span>

<span data-ttu-id="10c52-256">__Beispiel:__</span><span class="sxs-lookup"><span data-stu-id="10c52-256">__Example:__</span></span>

```csharp
/// <value>Property <c>X</c> represents the point's x-coordinate.</value>
public int X
{
    get { return x; }
    set { x = value; }
}
```

### `<typeparam>`

<span data-ttu-id="10c52-257">Dieses Tag wird verwendet, um einen generischen Typparameter für eine Klasse, Struktur, Schnittstelle, einen Delegaten oder eine Methode zu beschreiben.</span><span class="sxs-lookup"><span data-stu-id="10c52-257">This tag is used to describe a generic type parameter for a class, struct, interface, delegate, or method.</span></span>

<span data-ttu-id="10c52-258">__Syntax:__</span><span class="sxs-lookup"><span data-stu-id="10c52-258">__Syntax:__</span></span>

```xml
<typeparam name="name">description</typeparam>
```

<span data-ttu-id="10c52-259">Dabei ist der Name des Typparameters, und `description` ist seine Beschreibung. `name`</span><span class="sxs-lookup"><span data-stu-id="10c52-259">where `name` is the name of the type parameter, and `description` is its description.</span></span>

<span data-ttu-id="10c52-260">__Beispiel:__</span><span class="sxs-lookup"><span data-stu-id="10c52-260">__Example:__</span></span>

```csharp
/// <summary>A generic list class.</summary>
/// <typeparam name="T">The type stored by the list.</typeparam>
public class MyList<T> {
    ...
}
```

### `<typeparamref>`

<span data-ttu-id="10c52-261">Dieses Tag wird verwendet, um anzugeben, dass ein Wort ein Typparameter ist.</span><span class="sxs-lookup"><span data-stu-id="10c52-261">This tag is used to indicate that a word is a type parameter.</span></span> <span data-ttu-id="10c52-262">Die Dokumentations Datei kann verarbeitet werden, um diesen Typparameter auf unterschiedliche Weise zu formatieren.</span><span class="sxs-lookup"><span data-stu-id="10c52-262">The documentation file can be processed to format this type parameter in some distinct way.</span></span>

<span data-ttu-id="10c52-263">__Syntax:__</span><span class="sxs-lookup"><span data-stu-id="10c52-263">__Syntax:__</span></span>

```xml
<typeparamref name="name"/>
```

<span data-ttu-id="10c52-264">dabei `name` ist der Name des Typparameters.</span><span class="sxs-lookup"><span data-stu-id="10c52-264">where `name` is the name of the type parameter.</span></span>

<span data-ttu-id="10c52-265">__Beispiel:__</span><span class="sxs-lookup"><span data-stu-id="10c52-265">__Example:__</span></span>

```csharp
/// <summary>This method fetches data and returns a list of <typeparamref name="T"/>.</summary>
/// <param name="query">query to execute</param>
public List<T> FetchData<T>(string query) {
    ...
}
```

## <a name="processing-the-documentation-file"></a><span data-ttu-id="10c52-266">Die Dokumentations Datei wird verarbeitet.</span><span class="sxs-lookup"><span data-stu-id="10c52-266">Processing the documentation file</span></span>

<span data-ttu-id="10c52-267">Der Dokumentations Generator generiert eine ID-Zeichenfolge für jedes Element im Quellcode, das mit einem Dokumentations Kommentar gekennzeichnet ist.</span><span class="sxs-lookup"><span data-stu-id="10c52-267">The documentation generator generates an ID string for each element in the source code that is tagged with a documentation comment.</span></span> <span data-ttu-id="10c52-268">Diese ID-Zeichenfolge identifiziert eindeutig ein Quell Element.</span><span class="sxs-lookup"><span data-stu-id="10c52-268">This ID string uniquely identifies a source element.</span></span> <span data-ttu-id="10c52-269">In einem Dokumentations-Viewer kann eine ID-Zeichenfolge verwendet werden, um die entsprechenden Metadaten/reflektionselementelemente zu identifizieren</span><span class="sxs-lookup"><span data-stu-id="10c52-269">A documentation viewer can use an ID string to identify the corresponding metadata/reflection item to which the documentation applies.</span></span>

<span data-ttu-id="10c52-270">Die Dokumentations Datei ist keine hierarchische Darstellung des Quellcodes. Vielmehr handelt es sich um eine flache Liste mit einer generierten ID-Zeichenfolge für jedes Element.</span><span class="sxs-lookup"><span data-stu-id="10c52-270">The documentation file is not a hierarchical representation of the source code; rather, it is a flat list with a generated ID string for each element.</span></span>

### <a name="id-string-format"></a><span data-ttu-id="10c52-271">ID-Zeichen folgen Format</span><span class="sxs-lookup"><span data-stu-id="10c52-271">ID string format</span></span>

<span data-ttu-id="10c52-272">Beim Generieren der ID-Zeichen folgen werden die folgenden Regeln vom Dokumentations-Generator beachtet:</span><span class="sxs-lookup"><span data-stu-id="10c52-272">The documentation generator observes the following rules when it generates the ID strings:</span></span>

*  <span data-ttu-id="10c52-273">In der Zeichenfolge wird kein Leerraum platziert.</span><span class="sxs-lookup"><span data-stu-id="10c52-273">No white space is placed in the string.</span></span>

*  <span data-ttu-id="10c52-274">Der erste Teil der Zeichenfolge identifiziert die Art des dokumentierten Members, über ein einzelnes Zeichen gefolgt von einem Doppelpunkt.</span><span class="sxs-lookup"><span data-stu-id="10c52-274">The first part of the string identifies the kind of member being documented, via a single character followed by a colon.</span></span> <span data-ttu-id="10c52-275">Die folgenden Arten von Membern sind definiert:</span><span class="sxs-lookup"><span data-stu-id="10c52-275">The following kinds of members are defined:</span></span>

   | <span data-ttu-id="10c52-276">__Zeichen__</span><span class="sxs-lookup"><span data-stu-id="10c52-276">__Character__</span></span> | <span data-ttu-id="10c52-277">__Beschreibung__</span><span class="sxs-lookup"><span data-stu-id="10c52-277">__Description__</span></span>                                             |
   |---------------|-------------------------------------------------------------|
   | <span data-ttu-id="10c52-278">E</span><span class="sxs-lookup"><span data-stu-id="10c52-278">E</span></span>             | <span data-ttu-id="10c52-279">event</span><span class="sxs-lookup"><span data-stu-id="10c52-279">Event</span></span>                                                       |
   | <span data-ttu-id="10c52-280">F</span><span class="sxs-lookup"><span data-stu-id="10c52-280">F</span></span>             | <span data-ttu-id="10c52-281">Feld</span><span class="sxs-lookup"><span data-stu-id="10c52-281">Field</span></span>                                                       |
   | <span data-ttu-id="10c52-282">M</span><span class="sxs-lookup"><span data-stu-id="10c52-282">M</span></span>             | <span data-ttu-id="10c52-283">Methode (einschließlich Konstruktoren, Dekonstruktoren und Operatoren)</span><span class="sxs-lookup"><span data-stu-id="10c52-283">Method (including constructors, destructors, and operators)</span></span> |
   | <span data-ttu-id="10c52-284">N</span><span class="sxs-lookup"><span data-stu-id="10c52-284">N</span></span>             | <span data-ttu-id="10c52-285">Namespace</span><span class="sxs-lookup"><span data-stu-id="10c52-285">Namespace</span></span>                                                   |
   | <span data-ttu-id="10c52-286">P</span><span class="sxs-lookup"><span data-stu-id="10c52-286">P</span></span>             | <span data-ttu-id="10c52-287">Eigenschaft (einschließlich Indexer)</span><span class="sxs-lookup"><span data-stu-id="10c52-287">Property (including indexers)</span></span>                               |
   | <span data-ttu-id="10c52-288">T</span><span class="sxs-lookup"><span data-stu-id="10c52-288">T</span></span>             | <span data-ttu-id="10c52-289">Type (z. b. Klasse, Delegat, Enumeration, Schnittstelle und Struktur)</span><span class="sxs-lookup"><span data-stu-id="10c52-289">Type (such as class, delegate, enum, interface, and struct)</span></span> |
   | <span data-ttu-id="10c52-290">!</span><span class="sxs-lookup"><span data-stu-id="10c52-290">!</span></span>             | <span data-ttu-id="10c52-291">Fehler Zeichenfolge; der Rest der Zeichenfolge enthält Informationen zum Fehler.</span><span class="sxs-lookup"><span data-stu-id="10c52-291">Error string; the rest of the string provides information about the error.</span></span> <span data-ttu-id="10c52-292">Der Dokumentations Generator generiert beispielsweise Fehlerinformationen für Links, die nicht aufgelöst werden können.</span><span class="sxs-lookup"><span data-stu-id="10c52-292">For example, the documentation generator generates error information for links that cannot be resolved.</span></span> |

*  <span data-ttu-id="10c52-293">Der zweite Teil der Zeichenfolge ist der voll qualifizierte Name des Elements, beginnend mit dem Stamm des Namespace.</span><span class="sxs-lookup"><span data-stu-id="10c52-293">The second part of the string is the fully qualified name of the element, starting at the root of the namespace.</span></span> <span data-ttu-id="10c52-294">Der Name des Elements, der einschließende Typ (en) und der Namespace werden durch Punkte getrennt.</span><span class="sxs-lookup"><span data-stu-id="10c52-294">The name of the element, its enclosing type(s), and namespace are separated by periods.</span></span> <span data-ttu-id="10c52-295">Wenn der Name des Elements selbst über Zeiträume verfügt, werden Sie durch `#(U+0023)` Zeichen ersetzt.</span><span class="sxs-lookup"><span data-stu-id="10c52-295">If the name of the item itself has periods, they are replaced by `#(U+0023)` characters.</span></span> <span data-ttu-id="10c52-296">(Es wird angenommen, dass kein Element über dieses Zeichen im Namen verfügt.)</span><span class="sxs-lookup"><span data-stu-id="10c52-296">(It is assumed that no element has this character in its name.)</span></span>
*  <span data-ttu-id="10c52-297">Bei Methoden und Eigenschaften mit Argumenten folgt die Argumentliste in Klammern.</span><span class="sxs-lookup"><span data-stu-id="10c52-297">For methods and properties with arguments, the argument list follows, enclosed in parentheses.</span></span> <span data-ttu-id="10c52-298">Für diejenigen ohne Argumente werden die Klammern ausgelassen.</span><span class="sxs-lookup"><span data-stu-id="10c52-298">For those without arguments, the parentheses are omitted.</span></span> <span data-ttu-id="10c52-299">Die Argumente werden durch Kommas voneinander getrennt.</span><span class="sxs-lookup"><span data-stu-id="10c52-299">The arguments are separated by commas.</span></span> <span data-ttu-id="10c52-300">Die Codierung jedes Arguments ist wie folgt mit einer CLI-Signatur identisch:</span><span class="sxs-lookup"><span data-stu-id="10c52-300">The encoding of each argument is the same as a CLI signature, as follows:</span></span>
   *  <span data-ttu-id="10c52-301">Argumente werden durch ihren Dokumentations Namen dargestellt, der auf dem voll qualifizierten Namen basiert, wie folgt geändert:</span><span class="sxs-lookup"><span data-stu-id="10c52-301">Arguments are represented by their documentation name, which is based on their fully qualified name, modified as follows:</span></span>
      * <span data-ttu-id="10c52-302">Argumente, die generische Typen darstellen, haben `` ` `` ein angefügtes Zeichen (Backtick), gefolgt von der Anzahl der Typparameter.</span><span class="sxs-lookup"><span data-stu-id="10c52-302">Arguments that represent generic types have an appended `` ` `` (backtick) character followed by the number of type parameters</span></span>
      * <span data-ttu-id="10c52-303">Argumente, die `out` den `ref` -Modifizierer `@` oder den-Modifizierer haben, haben einen folgenden</span><span class="sxs-lookup"><span data-stu-id="10c52-303">Arguments having the `out` or `ref` modifier have an `@` following their type name.</span></span> <span data-ttu-id="10c52-304">Argumente, die nach Wert oder `params` via übermittelt werden, haben keine besondere Notation.</span><span class="sxs-lookup"><span data-stu-id="10c52-304">Arguments passed by value or via `params` have no special notation.</span></span>
      * <span data-ttu-id="10c52-305">Argumente, bei denen es sich um `[lowerbound:size, ... , lowerbound:size]` Arrays handelt, werden als dargestellt, wobei die Anzahl der Kommas der Rang kleiner ist, und die unteren Begrenzungen und die Größe der einzelnen Dimensionen, sofern bekannt, in Dezimalform dargestellt werden.</span><span class="sxs-lookup"><span data-stu-id="10c52-305">Arguments that are arrays are represented as `[lowerbound:size, ... , lowerbound:size]` where the number of commas is the rank less one, and the lower bounds and size of each dimension, if known, are represented in decimal.</span></span> <span data-ttu-id="10c52-306">Wenn eine Untergrenze oder Größe nicht angegeben ist, wird Sie ausgelassen.</span><span class="sxs-lookup"><span data-stu-id="10c52-306">If a lower bound or size is not specified, it is omitted.</span></span> <span data-ttu-id="10c52-307">Wenn die Untergrenze und die Größe für eine bestimmte Dimension ausgelassen werden, `:` wird auch der weggelassen.</span><span class="sxs-lookup"><span data-stu-id="10c52-307">If the lower bound and size for a particular dimension are omitted, the `:` is omitted as well.</span></span> <span data-ttu-id="10c52-308">Verzweigte Arrays werden durch eine `[]` pro Ebene dargestellt.</span><span class="sxs-lookup"><span data-stu-id="10c52-308">Jagged arrays are represented by one `[]` per level.</span></span>
      * <span data-ttu-id="10c52-309">Argumente, die andere Zeiger Typen als void aufweisen, werden mithilfe `*` eines nach dem Typnamen dargestellt.</span><span class="sxs-lookup"><span data-stu-id="10c52-309">Arguments that have pointer types other than void are represented using a `*` following the type name.</span></span> <span data-ttu-id="10c52-310">Ein void-Zeiger wird mithilfe eines Typnamens `System.Void`dargestellt.</span><span class="sxs-lookup"><span data-stu-id="10c52-310">A void pointer is represented using a type name of `System.Void`.</span></span>
      * <span data-ttu-id="10c52-311">Argumente, die auf generische Typparameter verweisen, die für-Typen definiert `` ` `` sind, werden mit dem Zeichen (Backtick) codiert, gefolgt vom Null basierten Index des Typparameters.</span><span class="sxs-lookup"><span data-stu-id="10c52-311">Arguments that refer to generic type parameters defined on types are encoded using the `` ` `` (backtick) character followed by the zero-based index of the type parameter.</span></span>
      * <span data-ttu-id="10c52-312">Argumente, die generische Typparameter verwenden, die in-Methoden definiert sind, ``` `` ``` verwenden einen doppelten `` ` `` Backtick anstelle der für-Typen verwendeten.</span><span class="sxs-lookup"><span data-stu-id="10c52-312">Arguments that use generic type parameters defined in methods use a double-backtick ``` `` ``` instead of the `` ` `` used for types.</span></span>
      * <span data-ttu-id="10c52-313">Argumente, die auf konstruierte generische Typen verweisen, werden mit dem generischen Typ gefolgt `{`von codiert, gefolgt von einer durch Trennzeichen getrennten Liste von Typargumenten, gefolgt von. `}`</span><span class="sxs-lookup"><span data-stu-id="10c52-313">Arguments that refer to constructed generic types are encoded using the generic type, followed by `{`, followed by a comma-separated list of type arguments, followed by `}`.</span></span>

### <a name="id-string-examples"></a><span data-ttu-id="10c52-314">ID-Zeichen folgen Beispiele</span><span class="sxs-lookup"><span data-stu-id="10c52-314">ID string examples</span></span>

<span data-ttu-id="10c52-315">In den folgenden Beispielen wird jeweils ein C# Code Fragment zusammen mit der ID-Zeichenfolge angezeigt, die aus jedem Quell Element erstellt wurde, das einen Dokumentations Kommentar aufweisen kann:</span><span class="sxs-lookup"><span data-stu-id="10c52-315">The following examples each show a fragment of C# code, along with the ID string produced from each source element capable of having a documentation comment:</span></span>

*  <span data-ttu-id="10c52-316">Typen werden mit dem voll qualifizierten Namen dargestellt, der auf generische Informationen erweitert ist:</span><span class="sxs-lookup"><span data-stu-id="10c52-316">Types are represented using their fully qualified name, augmented with generic information:</span></span>

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

*  <span data-ttu-id="10c52-317">Felder werden durch ihren voll qualifizierten Namen dargestellt:</span><span class="sxs-lookup"><span data-stu-id="10c52-317">Fields are represented by their fully qualified name:</span></span>

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

*  <span data-ttu-id="10c52-318">Konstruktoren.</span><span class="sxs-lookup"><span data-stu-id="10c52-318">Constructors.</span></span>

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

*  <span data-ttu-id="10c52-319">Destruktoren.</span><span class="sxs-lookup"><span data-stu-id="10c52-319">Destructors.</span></span>

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

*  <span data-ttu-id="10c52-320">Anzuwenden.</span><span class="sxs-lookup"><span data-stu-id="10c52-320">Methods.</span></span>

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

*  <span data-ttu-id="10c52-321">Eigenschaften und Indexer.</span><span class="sxs-lookup"><span data-stu-id="10c52-321">Properties and indexers.</span></span>

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

*  <span data-ttu-id="10c52-322">Fall.</span><span class="sxs-lookup"><span data-stu-id="10c52-322">Events.</span></span>

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

*  <span data-ttu-id="10c52-323">Unäre Operatoren.</span><span class="sxs-lookup"><span data-stu-id="10c52-323">Unary operators.</span></span>

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

   <span data-ttu-id="10c52-324">Der gesamte `op_UnaryPlus`Satz unärer Operator Funktionsnamen wird wie folgt verwendet:, `op_UnaryNegation`, `op_LogicalNot`, `op_OnesComplement`, `op_Increment`, `op_Decrement`, `op_True`und `op_False`.</span><span class="sxs-lookup"><span data-stu-id="10c52-324">The complete set of unary operator function names used is as follows: `op_UnaryPlus`, `op_UnaryNegation`, `op_LogicalNot`, `op_OnesComplement`, `op_Increment`, `op_Decrement`, `op_True`, and `op_False`.</span></span>

*  <span data-ttu-id="10c52-325">Binäre Operatoren.</span><span class="sxs-lookup"><span data-stu-id="10c52-325">Binary operators.</span></span>

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

   <span data-ttu-id="10c52-326">Der gesamte verwendete Satz von Funktionsnamen für binäre Operatoren lautet wie `op_Addition`folgt `op_Subtraction`: `op_Multiply`, `op_Division`, `op_Modulus`, `op_BitwiseAnd`, `op_BitwiseOr`, `op_ExclusiveOr`, `op_LeftShift`, `op_RightShift`,,, `op_Equality`, ,,`op_LessThan`, und`op_GreaterThan`. `op_LessThanOrEqual` `op_Inequality` `op_GreaterThanOrEqual`</span><span class="sxs-lookup"><span data-stu-id="10c52-326">The complete set of binary operator function names used is as follows: `op_Addition`, `op_Subtraction`, `op_Multiply`, `op_Division`, `op_Modulus`, `op_BitwiseAnd`, `op_BitwiseOr`, `op_ExclusiveOr`, `op_LeftShift`, `op_RightShift`, `op_Equality`, `op_Inequality`, `op_LessThan`, `op_LessThanOrEqual`, `op_GreaterThan`, and `op_GreaterThanOrEqual`.</span></span>

*  <span data-ttu-id="10c52-327">Konvertierungs Operatoren verfügen über`~`eine nachfolgende "", gefolgt vom Rückgabetyp.</span><span class="sxs-lookup"><span data-stu-id="10c52-327">Conversion operators have a trailing "`~`" followed by the return type.</span></span>

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

## <a name="an-example"></a><span data-ttu-id="10c52-328">Beispiel</span><span class="sxs-lookup"><span data-stu-id="10c52-328">An example</span></span>

### <a name="c-source-code"></a><span data-ttu-id="10c52-329">C#Quellcode</span><span class="sxs-lookup"><span data-stu-id="10c52-329">C# source code</span></span>

<span data-ttu-id="10c52-330">Das folgende Beispiel zeigt den Quellcode einer `Point` Klasse:</span><span class="sxs-lookup"><span data-stu-id="10c52-330">The following example shows the source code of a `Point` class:</span></span>

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

### <a name="resulting-xml"></a><span data-ttu-id="10c52-331">Resultierende XML</span><span class="sxs-lookup"><span data-stu-id="10c52-331">Resulting XML</span></span>

<span data-ttu-id="10c52-332">Hier sehen Sie die Ausgabe, die von einem Dokumentations Generator erzeugt wird, wenn der `Point`Quellcode für die-Klasse angegeben wird, wie oben gezeigt:</span><span class="sxs-lookup"><span data-stu-id="10c52-332">Here is the output produced by one documentation generator when given the source code for class `Point`, shown above:</span></span>

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
