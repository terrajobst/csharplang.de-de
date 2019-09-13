---
ms.openlocfilehash: adf81842e3c763c7bbdd3f10bb884dc1207b9099
ms.sourcegitcommit: 0489cb64b7dfb328813d757f4d447a15b85a5851
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 09/11/2019
ms.locfileid: "70912438"
---
# <a name="documentation-comments"></a>Dokumentationskommentare

C#stellt einen Mechanismus bereit, mit dem Programmierer Ihren Code mithilfe einer speziellen Kommentar Syntax dokumentieren können, die XML-Text enthält. In Quell Code Dateien können Kommentare mit einem bestimmten Formular verwendet werden, um ein Tool zum Erstellen von XML aus diesen Kommentaren und den Quell Code Elementen zu leiten, denen Sie vorangestellt sind. Kommentare, die diese Syntax verwenden, werden als ***Dokumentations Kommentare***bezeichnet. Sie müssen direkt einem benutzerdefinierten Typ (z. b. einer Klasse, einem Delegaten oder einer Schnittstelle) oder einem Member (z. b. einem Feld, einem Ereignis, einer Eigenschaft oder einer Methode) vorangestellt sein. Das XML-Generierungs Tool wird als ***Dokumentations Generator***bezeichnet. (Dieser Generator kann, aber nicht, der C# Compiler selbst sein.) Die vom Dokumentations Generator erstellte Ausgabe wird als ***Dokumentations Datei***bezeichnet. Eine Dokumentations Datei wird als Eingabe für einen ***Dokumentations-Viewer***verwendet. ein Tool, mit dem eine visuelle Darstellung der Typinformationen und der zugehörigen Dokumentation erzeugt werden soll.

Diese Spezifikation schlägt vor, dass ein Satz von Tags in Dokumentations Kommentaren verwendet wird, aber die Verwendung dieser Tags ist nicht erforderlich, und andere Tags können bei Bedarf verwendet werden, solange die Regeln von wohl geformtem XML befolgt werden.

## <a name="introduction"></a>Einführung

Kommentare mit einem speziellen Formular können verwendet werden, um ein Tool zum Erstellen von XML aus diesen Kommentaren und den Quell Code Elementen zu leiten, denen Sie vorangestellt sind. Solche Kommentare sind einzeilige Kommentare, die mit drei Schrägstrichen (`///`) oder durch getrennte Kommentare beginnen, die mit einem Schrägstrich und zwei Sternen`/**`beginnen (). Sie müssen unmittelbar vor einem benutzerdefinierten Typ (z. b. einer Klasse, einem Delegaten oder einer Schnittstelle) oder einem Member (z. b. einem Feld, einem Ereignis, einer Eigenschaft oder einer Methode), dem Sie kommentiert werden, voranstehen. Attribut Abschnitte ([Attribut Spezifikation](attributes.md#attribute-specification)) werden als Teil von Deklarationen betrachtet. Daher müssen Dokumentations Kommentare den Attributen vorangestellt werden, die auf einen Typ oder Member angewendet werden.

__Syntax:__

```antlr
single_line_doc_comment
    : '///' input_character*
    ;

delimited_doc_comment
    : '/**' delimited_comment_section* asterisk+ '/'
    ;
```

Wenn in *einem single_line_doc_comment*ein *leer* Zeichen vorhanden ist, das auf `///` die Zeichen auf jeder der *single_line_doc_comment*s neben dem aktuellen *single_line_doc_comment*folgt, dann  *ein leer* Zeichen ist nicht in der XML-Ausgabe enthalten.

Wenn in einem durch Trennzeichen getrennten doc-Kommentar das erste Zeichen in der zweiten Zeile, das kein Leerzeichen ist, ein Sternchen und das gleiche Muster optionaler leer Raum Zeichen und ein Sternchen am Anfang jeder Zeile innerhalb des durch Trennzeichen getrennten-doc-Kommentars wiederholt wird. dann sind die Zeichen des wiederholten Musters nicht in der XML-Ausgabe enthalten. Das Muster kann Leerzeichen nach und vor das Sternchen Zeichen enthalten.

__Beispiel:__

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

Der Text in den Dokumentations Kommentaren muss gemäß den Regeln von XML (https://www.w3.org/TR/REC-xml) ) wohl geformt sein. Wenn das XML nicht ordnungsgemäß formatiert ist, wird eine Warnung generiert, und die Dokumentations Datei enthält einen Kommentar, der besagt, dass ein Fehler aufgetreten ist.

Obwohl Entwickler kostenlos einen eigenen Satz von Tags erstellen können, wird ein empfohlener Satz in [empfohlenen Tags](documentation-comments.md#recommended-tags)definiert. Einige der empfohlenen Tags haben eine besondere Bedeutung:

*  Das `<param>` -Tag wird verwendet, um Parameter zu beschreiben. Wenn ein solches Tag verwendet wird, muss der Dokumentations Generator überprüfen, ob der angegebene Parameter vorhanden ist und dass alle Parameter in den Dokumentations Kommentaren beschrieben werden. Wenn diese Überprüfung fehlschlägt, gibt der Dokumentations Generator eine Warnung aus.
*  Das `cref`-Attribut kann an jedes Tag angefügt werden, um einen Verweis auf ein Codeelement bereitzustellen. Der Dokumentations Generator muss überprüfen, ob dieses Code Element vorhanden ist. Wenn die Überprüfung fehlschlägt, gibt der Dokumentations Generator eine Warnung aus. Bei der Suche nach einem Namen, der `cref` in einem-Attribut beschrieben wird, muss der Dokumentations-Generator `using` die Sichtbarkeit von Namespaces gemäß den Anweisungen im Quellcode berücksichtigen. Für Code Elemente, die generisch sind, kann die normale generische Syntax (d`List<T>`. h. "") nicht verwendet werden, da Sie ungültiges XML erzeugt. Geschweifte Klammern können anstelle von Klammern (`List{T}`d. h. "") verwendet werden, oder die XML-Escapesyntax kann verwendet werden (d. h. "`List&lt;T&gt;`").
*  Das `<summary>` -Tag soll von einem Dokumentations-Viewer verwendet werden, um zusätzliche Informationen zu einem Typ oder Member anzuzeigen.
*  Das `<include>` -Tag enthält Informationen aus einer externen XML-Datei.

Beachten Sie, dass die Dokumentations Datei keine vollständigen Informationen über den Typ und die Member bereitstellt (z. b. enthält Sie keine Typinformationen). Um solche Informationen zu einem Typ oder Member zu erhalten, muss die Dokumentations Datei in Verbindung mit der Reflektion für den tatsächlichen Typ oder Member verwendet werden.

## <a name="recommended-tags"></a>Empfohlene Tags

Der Dokumentations-Generator muss alle Tags akzeptieren und verarbeiten, die gemäß den XML-Regeln gültig sind. Die folgenden Tags stellen häufig verwendete Funktionen in der Benutzerdokumentation bereit. (Natürlich sind andere Tags möglich.)


| __Tag__          | __Bereich__                                            | __Darin__                                            |
|------------------|--------------------------------------------------------|--------------------------------------------------------|
| `<c>`            | [`<c>`](documentation-comments.md#c)                   | Festlegen von Text in einer Code ähnlichen Schriftart                           | 
| `<code>`         | [`<code>`](documentation-comments.md#code)             | Mindestens eine Zeile des Quellcodes oder der Programmausgabe festlegen |
| `<example>`      | [`<example>`](documentation-comments.md#example)       | Anzeigen eines Beispiels                                    |
| `<exception>`    | [`<exception>`](documentation-comments.md#exception)   | Identifiziert die Ausnahmen, die von einer Methode ausgelöst werden können.           |
| `<include>`      | [`<include>`](documentation-comments.md#include)       | Enthält XML aus einer externen Datei.                     |
| `<list>`         | [`<list>`](documentation-comments.md#list)             | Erstellen einer Liste oder Tabelle                                 |
| `<para>`         | [`<para>`](documentation-comments.md#para)             | Struktur zum Hinzufügen von Text zulassen                   |
| `<param>`        | [`<param>`](documentation-comments.md#param)           | Beschreibt einen Parameter für eine Methode oder einen Konstruktor.       |
| `<paramref>`     | [`<paramref>`](documentation-comments.md#paramref)     | Erkennen, dass ein Wort ein Parameter Name ist               |
| `<permission>`   | [`<permission>`](documentation-comments.md#permission) | Dokumentieren der Sicherheits Barrierefreiheit eines Mitglieds        |
| `<remarks>`      | [`<remarks>`](documentation-comments.md#remarks)       | Beschreiben zusätzlicher Informationen zu einem Typ           |
| `<returns>`      | [`<returns>`](documentation-comments.md#returns)       | Beschreiben des Rückgabewerts einer Methode                  |
| `<see>`          | [`<see>`](documentation-comments.md#see)               | Link angeben                                         |
| `<seealso>`      | [`<seealso>`](documentation-comments.md#seealso)       | Einen Eintrag "Siehe auch" generieren                              |
| `<summary>`      | [`<summary>`](documentation-comments.md#summary)       | Beschreibt einen Typ oder einen Member eines Typs.                  |
| `<value>`        | [`<value>`](documentation-comments.md#value)           | Beschreiben einer Eigenschaft                                    |
| `<typeparam>`    |                                                        | Beschreiben eines generischen Typparameters                      |
| `<typeparamref>` |                                                        | Erkennen, dass ein Wort ein Typparameter Name ist          |

### `<c>`

Dieses Tag stellt einen Mechanismus bereit, mit dem angegeben wird, dass ein Textfragment innerhalb einer Beschreibung in einer speziellen Schriftart, z. b. der für einen Codeblock verwendeten, festgelegt werden soll. Verwenden `<code>` Sie für Zeilen des tatsächlichen Codes ([`<code>`](documentation-comments.md#code)).

__Syntax:__

```xml
<c>text</c>
```

__Beispiel:__

```csharp
/// <summary>Class <c>Point</c> models a point in a two-dimensional
/// plane.</summary>

public class Point 
{
    // ...
}
```

### `<code>`

Dieses Tag wird verwendet, um eine oder mehrere Zeilen des Quellcodes oder der Programmausgabe in einer speziellen Schriftart festzulegen. Verwenden `<c>` Sie bei kleinen Code Fragmenten in der Erzählung[`<c>`](documentation-comments.md#c)().

__Syntax:__

```xml
<code>source code or program output</code>
```

__Beispiel:__

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

Dieses Tag ermöglicht Beispielcode in einem Kommentar, um anzugeben, wie eine Methode oder ein anderes Bibliothekselement verwendet werden kann. Normalerweise würde dies auch die Verwendung des-Tags `<code>` ([`<code>`](documentation-comments.md#code)) beinhalten.

__Syntax:__

```xml
<example>description</example>
```

__Beispiel:__

Ein `<code>` Beispiel[`<code>`](documentation-comments.md#code)finden Sie unter ().

### `<exception>`

Dieses Tag bietet eine Möglichkeit, die Ausnahmen zu dokumentieren, die eine Methode auslösen kann.

__Syntax:__

```xml
<exception cref="member">description</exception>
```

wo

* `member`der Name eines Members. Der Dokumentations Generator überprüft, ob der angegebene Member vorhanden ist `member` , und übersetzt ihn in der Dokumentations Datei in den kanonischen Elementnamen.
* `description`eine Beschreibung der Umstände, in denen die Ausnahme ausgelöst wird.

__Beispiel:__

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

Dieses Tag ermöglicht das Einschließen von Informationen aus einem XML-Dokument, das für die Quell Code Datei extern ist. Die externe Datei muss ein wohl geformtes XML-Dokument sein, und es wird ein XPath-Ausdruck auf das Dokument angewendet, um anzugeben, welches XML-Dokument in diesem Dokument enthalten sein soll. Das `<include>` Tag wird dann durch den ausgewählten XML-Code aus dem externen Dokument ersetzt.

__Syntax:__

```
<include file="filename" path="xpath" />
```

wo

* `filename`der Dateiname einer externen XML-Datei. Der Dateiname wird relativ zur Datei interpretiert, die das include-Tag enthält.
* `xpath`ein XPath-Ausdruck, der einen Teil des XML-Codes in der externen XML-Datei auswählt.

__Beispiel:__

Wenn der Quellcode eine Deklaration wie z. b. enthält:

```csharp
/// <include file="docs.xml" path='extradoc/class[@name="IntList"]/*' />
public class IntList { ... }
```

die externe Datei "docs. xml" wies den folgenden Inhalt auf:

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

dann wird dieselbe Dokumentation ausgegeben, als wäre der Quellcode wie folgt:

```csharp
/// <summary>
///    Contains a list of integers.
/// </summary>
public class IntList { ... }
```

### `<list>`

Dieses Tag wird verwendet, um eine Liste oder eine Tabelle mit Elementen zu erstellen. Sie kann einen `<listheader>` -Block enthalten, um die Überschriften Zeile einer Tabelle oder einer Definitionsliste zu definieren. (Beim Definieren einer Tabelle muss nur ein Eintrag für `term` in der Überschrift angegeben werden.)

Jedes Element in der Liste wird mit einem `<item>` -Block angegeben. Beim Erstellen einer Definitionsliste müssen sowohl `term` als `description` auch angegeben werden. Allerdings muss für eine Tabelle, eine aufzählige Liste oder eine nummerierte Liste nur `description` angegeben werden.

__Syntax:__

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

wo

* `term`der Begriff, der definiert werden soll, dessen Definition `description`in ist.
* `description`ist entweder ein Element in einer Aufzählungs Liste oder nummerierte Liste oder die `term`Definition einer.

__Beispiel:__

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

Dieses Tag dient zur Verwendung innerhalb anderer Tags, z `<summary>` . b. ( `<returns>` [`<remarks>`](documentation-comments.md#remarks))[`<returns>`](documentation-comments.md#returns)oder (), und lässt zu, dass die Struktur dem Text hinzugefügt wird.

__Syntax:__

```xml
<para>content</para>
```

dabei `content` ist der Text des Absatzes.

__Beispiel:__

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

Dieses Tag wird verwendet, um einen Parameter für eine Methode, einen Konstruktor oder einen Indexer zu beschreiben.

__Syntax:__

```xml
<param name="name">description</param>
```

wo

* `name`der Name des Parameters.
* `description`eine Beschreibung des Parameters.

__Beispiel:__

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

Dieses Tag wird verwendet, um anzugeben, dass ein Wort ein Parameter ist. Die Dokumentations Datei kann verarbeitet werden, um diesen Parameter auf unterschiedliche Weise zu formatieren.

__Syntax:__

```xml
<paramref name="name"/>
```

dabei `name` ist der Name des Parameters.

__Beispiel:__

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

Mit diesem Tag kann die Sicherheits Barrierefreiheit eines Members dokumentiert werden.

__Syntax:__

```xml
<permission cref="member">description</permission>
```

wo

* `member`der Name eines Members. Der Dokumentations Generator überprüft, ob das angegebene Code Element vorhanden ist, und übersetzt den *Member* in den kanonischen Elementnamen in der Dokumentations Datei.
* `description`eine Beschreibung des Zugriffs auf den Member.

__Beispiel:__

```csharp
/// <permission cref="System.Security.PermissionSet">Everyone can
/// access this method.</permission>

public static void Test() {
    // ...
}
```

### `<remarks>`

Dieses Tag wird verwendet, um zusätzliche Informationen zu einem Typ anzugeben. (Verwenden `<summary>` Sie[`<summary>`](documentation-comments.md#summary)(), um den Typ selbst und die Member eines Typs zu beschreiben.)

__Syntax:__

```xml
<remarks>description</remarks>
```

dabei `description` steht für den Text der Anmerkung.

__Beispiel:__

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

Dieses Tag wird verwendet, um den Rückgabewert einer Methode zu beschreiben.

__Syntax:__

```xml
<returns>description</returns>
```

dabei `description` ist eine Beschreibung des Rückgabewerts.

__Beispiel:__

```csharp
/// <summary>Report a point's location as a string.</summary>
/// <returns>A string representing a point's location, in the form (x,y),
///    without any leading, trailing, or embedded whitespace.</returns>
public override string ToString() {
    return "(" + X + "," + Y + ")";
}
```

### `<see>`

Dieses Tag ermöglicht das Angeben eines Links in Text. Verwenden `<seealso>` Sie[`<seealso>`](documentation-comments.md#seealso)(), um Text anzugeben, der in einem Abschnitt Siehe auch angezeigt werden soll.

__Syntax:__

```xml
<see cref="member"/>
```

dabei `member` ist der Name eines Members. Der Dokumentations Generator überprüft, ob das angegebene Code Element vorhanden ist, und ändert den *Member* in der generierten Dokumentations Datei in den Elementnamen.

__Beispiel:__

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

Dieses Tag ermöglicht das Generieren eines Eintrags für den Abschnitt "Siehe auch". Verwenden `<see>` Sie[`<see>`](documentation-comments.md#see)(), um einen Link aus Text anzugeben.

__Syntax:__

```xml
<seealso cref="member"/>
```

dabei `member` ist der Name eines Members. Der Dokumentations Generator überprüft, ob das angegebene Code Element vorhanden ist, und ändert den *Member* in der generierten Dokumentations Datei in den Elementnamen.

__Beispiel:__

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

Dieses Tag kann verwendet werden, um einen Typ oder einen Member eines Typs zu beschreiben. Verwenden `<remarks>` Sie[`<remarks>`](documentation-comments.md#remarks)(), um den Typ selbst zu beschreiben.

__Syntax:__

```xml
<summary>description</summary>
```

dabei `description` ist eine Zusammenfassung des Typs oder Members.

__Beispiel:__

```csharp
/// <summary>This constructor initializes the new Point to (0,0).</summary>
public Point() : this(0,0) {
}
```

### `<value>`

Dieses Tag ermöglicht die Beschreibung einer Eigenschaft.

__Syntax:__

```xml
<value>property description</value>
```

dabei `property description` ist eine Beschreibung der Eigenschaft.

__Beispiel:__

```csharp
/// <value>Property <c>X</c> represents the point's x-coordinate.</value>
public int X
{
    get { return x; }
    set { x = value; }
}
```

### `<typeparam>`

Dieses Tag wird verwendet, um einen generischen Typparameter für eine Klasse, Struktur, Schnittstelle, einen Delegaten oder eine Methode zu beschreiben.

__Syntax:__

```xml
<typeparam name="name">description</typeparam>
```

Dabei ist der Name des Typparameters, und `description` ist seine Beschreibung. `name`

__Beispiel:__

```csharp
/// <summary>A generic list class.</summary>
/// <typeparam name="T">The type stored by the list.</typeparam>
public class MyList<T> {
    ...
}
```

### `<typeparamref>`

Dieses Tag wird verwendet, um anzugeben, dass ein Wort ein Typparameter ist. Die Dokumentations Datei kann verarbeitet werden, um diesen Typparameter auf unterschiedliche Weise zu formatieren.

__Syntax:__

```xml
<typeparamref name="name"/>
```

dabei `name` ist der Name des Typparameters.

__Beispiel:__

```csharp
/// <summary>This method fetches data and returns a list of <typeparamref name="T"/>.</summary>
/// <param name="query">query to execute</param>
public List<T> FetchData<T>(string query) {
    ...
}
```

## <a name="processing-the-documentation-file"></a>Die Dokumentations Datei wird verarbeitet.

Der Dokumentations Generator generiert eine ID-Zeichenfolge für jedes Element im Quellcode, das mit einem Dokumentations Kommentar gekennzeichnet ist. Diese ID-Zeichenfolge identifiziert eindeutig ein Quell Element. In einem Dokumentations-Viewer kann eine ID-Zeichenfolge verwendet werden, um die entsprechenden Metadaten/reflektionselementelemente zu identifizieren

Die Dokumentations Datei ist keine hierarchische Darstellung des Quellcodes. Vielmehr handelt es sich um eine flache Liste mit einer generierten ID-Zeichenfolge für jedes Element.

### <a name="id-string-format"></a>ID-Zeichen folgen Format

Beim Generieren der ID-Zeichen folgen werden die folgenden Regeln vom Dokumentations-Generator beachtet:

*  In der Zeichenfolge wird kein Leerraum platziert.

*  Der erste Teil der Zeichenfolge identifiziert die Art des dokumentierten Members, über ein einzelnes Zeichen gefolgt von einem Doppelpunkt. Die folgenden Arten von Membern sind definiert:

   | __Zeichen__ | __Beschreibung__                                             |
   |---------------|-------------------------------------------------------------|
   | E             | event                                                       |
   | F             | Feld                                                       |
   | M             | Methode (einschließlich Konstruktoren, Dekonstruktoren und Operatoren) |
   | N             | Namespace                                                   |
   | P             | Eigenschaft (einschließlich Indexer)                               |
   | T             | Type (z. b. Klasse, Delegat, Enumeration, Schnittstelle und Struktur) |
   | !             | Fehler Zeichenfolge; der Rest der Zeichenfolge enthält Informationen zum Fehler. Der Dokumentations Generator generiert beispielsweise Fehlerinformationen für Links, die nicht aufgelöst werden können. |

*  Der zweite Teil der Zeichenfolge ist der voll qualifizierte Name des Elements, beginnend mit dem Stamm des Namespace. Der Name des Elements, der einschließende Typ (en) und der Namespace werden durch Punkte getrennt. Wenn der Name des Elements selbst über Zeiträume verfügt, werden Sie durch `#(U+0023)` Zeichen ersetzt. (Es wird angenommen, dass kein Element über dieses Zeichen im Namen verfügt.)
*  Bei Methoden und Eigenschaften mit Argumenten folgt die Argumentliste in Klammern. Für diejenigen ohne Argumente werden die Klammern ausgelassen. Die Argumente werden durch Kommas voneinander getrennt. Die Codierung jedes Arguments ist wie folgt mit einer CLI-Signatur identisch:
   *  Argumente werden durch ihren Dokumentations Namen dargestellt, der auf dem voll qualifizierten Namen basiert, wie folgt geändert:
      * Argumente, die generische Typen darstellen, haben `` ` `` ein angefügtes Zeichen (Backtick), gefolgt von der Anzahl der Typparameter.
      * Argumente, die `out` den `ref` -Modifizierer `@` oder den-Modifizierer haben, haben einen folgenden Argumente, die nach Wert oder `params` via übermittelt werden, haben keine besondere Notation.
      * Argumente, bei denen es sich um `[lowerbound:size, ... , lowerbound:size]` Arrays handelt, werden als dargestellt, wobei die Anzahl der Kommas der Rang kleiner ist, und die unteren Begrenzungen und die Größe der einzelnen Dimensionen, sofern bekannt, in Dezimalform dargestellt werden. Wenn eine Untergrenze oder Größe nicht angegeben ist, wird Sie ausgelassen. Wenn die Untergrenze und die Größe für eine bestimmte Dimension ausgelassen werden, `:` wird auch der weggelassen. Verzweigte Arrays werden durch eine `[]` pro Ebene dargestellt.
      * Argumente, die andere Zeiger Typen als void aufweisen, werden mithilfe `*` eines nach dem Typnamen dargestellt. Ein void-Zeiger wird mithilfe eines Typnamens `System.Void`dargestellt.
      * Argumente, die auf generische Typparameter verweisen, die für-Typen definiert `` ` `` sind, werden mit dem Zeichen (Backtick) codiert, gefolgt vom Null basierten Index des Typparameters.
      * Argumente, die generische Typparameter verwenden, die in-Methoden definiert sind, ``` `` ``` verwenden einen doppelten `` ` `` Backtick anstelle der für-Typen verwendeten.
      * Argumente, die auf konstruierte generische Typen verweisen, werden mit dem generischen Typ gefolgt `{`von codiert, gefolgt von einer durch Trennzeichen getrennten Liste von Typargumenten, gefolgt von. `}`

### <a name="id-string-examples"></a>ID-Zeichen folgen Beispiele

In den folgenden Beispielen wird jeweils ein C# Code Fragment zusammen mit der ID-Zeichenfolge angezeigt, die aus jedem Quell Element erstellt wurde, das einen Dokumentations Kommentar aufweisen kann:

*  Typen werden mit dem voll qualifizierten Namen dargestellt, der auf generische Informationen erweitert ist:

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

*  Felder werden durch ihren voll qualifizierten Namen dargestellt:

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

*  Konstruktoren.

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

*  Destruktoren.

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

*  Anzuwenden.

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

*  Eigenschaften und Indexer.

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

*  Fall.

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

*  Unäre Operatoren.

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

   Der gesamte `op_UnaryPlus`Satz unärer Operator Funktionsnamen wird wie folgt verwendet:, `op_UnaryNegation`, `op_LogicalNot`, `op_OnesComplement`, `op_Increment`, `op_Decrement`, `op_True`und `op_False`.

*  Binäre Operatoren.

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

   Der gesamte verwendete Satz von Funktionsnamen für binäre Operatoren lautet wie `op_Addition`folgt `op_Subtraction`: `op_Multiply`, `op_Division`, `op_Modulus`, `op_BitwiseAnd`, `op_BitwiseOr`, `op_ExclusiveOr`, `op_LeftShift`, `op_RightShift`,,, `op_Equality`, ,,`op_LessThan`, und`op_GreaterThan`. `op_LessThanOrEqual` `op_Inequality` `op_GreaterThanOrEqual`

*  Konvertierungs Operatoren verfügen über`~`eine nachfolgende "", gefolgt vom Rückgabetyp.

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

## <a name="an-example"></a>Beispiel

### <a name="c-source-code"></a>C#Quellcode

Das folgende Beispiel zeigt den Quellcode einer `Point` Klasse:

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

### <a name="resulting-xml"></a>Resultierende XML

Hier sehen Sie die Ausgabe, die von einem Dokumentations Generator erzeugt wird, wenn der `Point`Quellcode für die-Klasse angegeben wird, wie oben gezeigt:

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
