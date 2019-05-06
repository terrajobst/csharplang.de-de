---
ms.openlocfilehash: c9f8417dc68153f02ceb72bb1d51f3615f3c4961
ms.sourcegitcommit: 3fc033b6e98ed7ecdf46a85c79b00a3a3ddcf963
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/18/2019
ms.locfileid: "54272045"
---
# <a name="documentation-comments"></a>Kommentare zur Dokumentation

C# bietet einen Mechanismus für Programmierer, dokumentieren ihren Code mit einer besonderen Kommentarsyntax, die XML-Text enthält. In Quellcodedateien können Kommentare, die einem bestimmten Format aufweisen verwendet werden, leiten Sie ein Tool zum Erstellen von XML aus diesen Kommentaren und die Quellcodeelemente, die sie vor. Diese Syntax heißen-Kommentaren mit ***Dokumentationskommentare***. Sie müssen unmittelbar vor einem benutzerdefinierten Typ (z. B. eine Klasse, Delegat oder Schnittstelle) oder ein Element (z. B. ein Feld, Ereignis, Eigenschaft oder Methode) stehen. Das Tool zum Generieren von XML-wird aufgerufen, die ***Dokumentations-Generator***. (Dieses Generators könnte sein, aber muss nicht sein, die C#-Compiler selbst.) Die Ausgabe der Dokumentations-Generator wird aufgerufen, die ***Dokumentationsdatei***. Eine Dokumentationsdatei dient als Eingabe für eine ***Dokumentations-Viewer***ein Tool soll eine Art visuelle Darstellung der Typinformationen und die zugehörige Dokumentation zu erzeugen.

Diese Spezifikation empfiehlt einen Satz von Tags in Dokumentationskommentare, verwendet werden, aber mithilfe dieser Tags ist nicht erforderlich und andere Tags können verwendet werden, wenn gewünscht, wie lange die Regeln für wohlgeformte XML-eingehalten werden.

## <a name="introduction"></a>Einführung

Kommentare müssen eine besondere Form können verwendet werden, um ein Tool zum Erstellen von XML aus diesen Kommentaren und die Quellcodeelemente, die sie vor weiterzuleiten. Diese Kommentare werden einzeilige Kommentare, die mit drei Schrägstrichen beginnen (`///`), oder Kommentare, die mit einem Schrägstrich und zwei Sterne beginnen getrennt (`/**`). Sie müssen unmittelbar voranstehen, einen benutzerdefinierten Typ (z. B. eine Klasse, Delegat oder Schnittstelle) oder ein Element (z. B. ein Feld, Ereignis, Eigenschaft oder Methode), die sie kommentieren. Attribut Abschnitte ([Attributspezifikation](attributes.md#attribute-specification)) gelten als Teil von Deklarationen, damit Kommentare zur Dokumentation auf einen Typ oder Member angewendete vor angegeben werden müssen.

__Syntax:__

```antlr
single_line_doc_comment
    : '///' input_character*
    ;

delimited_doc_comment
    : '/**' delimited_comment_section* asterisk+ '/'
    ;
```

In einer *Single_line_doc_comment*, wenn eine *Leerzeichen* folgende Zeichen der `///` Zeichen auf jedem der *Single_line_doc_comment*s angrenzende mit dem aktuellen *Single_line_doc_comment*, klicken Sie dann, *Leerzeichen* Zeichen ist nicht in der XML-Ausgabe enthalten.

In einem getrennten-Dok-Kommentar Wenn das erste nicht-Leerzeichen in der zweiten Zeile ein Sternchen und dasselbe Muster von optionalen Leerzeichen und ein Sternchen am Anfang der Zeile im getrennten-Dok-Kommentar jedes wiederholt, Klicken Sie dann sind die Zeichen, das wiederholte Muster nicht in der XML-Ausgabe enthalten. Das Muster kann Leerzeichen nach sowie vor, das Sternchenzeichen enthalten.

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

Der Text innerhalb von Dokumentationskommentaren muss wohlgeformt sein und gemäß den Regeln des XML-Codes (https://www.w3.org/TR/REC-xml). Wenn der XML-Code fehlerhaft formuliert ist, formatiert, wird eine Warnung generiert, und die Dokumentationsdatei enthält einen Kommentar, der besagt, dass ein Fehler aufgetreten ist.

Obwohl Entwickler können ihren eigenen Satz von Tags zu erstellen sind, ist eine empfohlene Sammlung in definiert [empfohlene Tags](documentation-comments.md#recommended-tags). Einige der empfohlenen Tags haben eine besondere Bedeutung:

*  Die `<param>` Tag wird verwendet, um Parameter zu beschreiben. Wenn diese einem Tag verwendet wird, muss der Dokumentations-Generator überprüfen, ob der angegebene Parameter vorhanden ist und alle Parameter in Dokumentationskommentaren beschrieben werden. Wenn eine solche Überprüfung fehlschlägt, gibt der Dokumentations-Generator eine Warnung aus.
*  Das `cref`-Attribut kann an jedes Tag angefügt werden, um einen Verweis auf ein Codeelement bereitzustellen. Der Dokumentations-Generator muss, ob dieses Codeelement vorhanden ist. Wenn die Überprüfung fehlschlägt, gibt der Dokumentations-Generator eine Warnung aus. Bei der Suche für ein Namen im beschrieben eine `cref` -Attribut muss der Dokumentations-Generator Namespace Sichtbarkeit gemäß respektieren `using` Anweisungen im Quellcode. Für Codeelemente, die generisch ist, sind die normale generische Syntax (d. h. "`List<T>`") kann nicht verwendet werden, da er ungültige XML-Daten erzeugt. Geschweifte Klammern können anstelle von eckigen Klammern verwendet werden (d. h. "`List{T}`"), oder die XML-Escape-Syntax verwendet werden kann (d. h. "`List&lt;T&gt;`").
*  Die `<summary>` richtet sich an Tag durch eine Dokumentations-Viewer verwendet werden, um weitere Informationen über einen Typ oder Member anzuzeigen.
*  Die `<include>` Tag enthält Informationen aus einer externen XML-Datei.

Sorgfältig Beachten Sie, dass die Dokumentationsdatei keine vollständigen Informationen über Typen und Membern (z. B. es enthält keine Typinformationen). Um solche Informationen über einen Typ oder Member zu erhalten, muss die Dokumentationsdatei zusammen mit Reflektion für den tatsächlichen Typ oder Member verwendet werden.

## <a name="recommended-tags"></a>Empfohlene tags

Der Dokumentations-Generator muss akzeptiert und verarbeitet alle Tags, die gemäß den Regeln der XML gültig ist. Die folgenden Tags stellen häufig verwendete Funktionen in der Dokumentation für die Benutzer bereit. (Natürlich sind andere Tags möglich).


| __Tag__          | __Bereich__                                            | __Zweck__                                            |
|------------------|--------------------------------------------------------|--------------------------------------------------------|
| `<c>`            | [`<c>`](documentation-comments.md#c)                   | Text in einer Code-ähnliche Schriftart festlegen                           | 
| `<code>`         | [`<code>`](documentation-comments.md#code)             | Legen Sie eine oder mehrere Zeilen von Code oder Programmausgabe-Quellausgabe |
| `<example>`      | [`<example>`](documentation-comments.md#example)       | Geben Sie ein Beispiel für                                    |
| `<exception>`    | [`<exception>`](documentation-comments.md#exception)   | Identifiziert die Ausnahmen, die eine Methode ausgelöst werden können           |
| `<include>`      | [`<include>`](documentation-comments.md#include)       | Enthält XML-Code aus einer externen Datei                     |
| `<list>`         | [`<list>`](documentation-comments.md#list)             | Erstellen Sie eine Liste oder Tabelle                                 |
| `<para>`         | [`<para>`](documentation-comments.md#para)             | Zulassen der Struktur, die Text hinzugefügt werden                   |
| `<param>`        | [`<param>`](documentation-comments.md#param)           | Beschreiben Sie die Parameter für eine Methode oder Konstruktor       |
| `<paramref>`     | [`<paramref>`](documentation-comments.md#paramref)     | Erkennen Sie, dass ein Wort ein Parametername ist               |
| `<permission>`   | [`<permission>`](documentation-comments.md#permission) | Dokumentieren Sie die Security Zugriff auf ein Element        |
| `<remark>`       | [`<remark>`](documentation-comments.md#remark)         | Weitere Informationen zu einem Typ beschreiben           |
| `<returns>`      | [`<returns>`](documentation-comments.md#returns)       | Der Rückgabewert einer Methode zu beschreiben                  |
| `<see>`          | [`<see>`](documentation-comments.md#see)               | Geben Sie einen link                                         |
| `<seealso>`      | [`<seealso>`](documentation-comments.md#seealso)       | Generieren Sie einen Eintrag auch finden Sie unter                              |
| `<summary>`      | [`<summary>`](documentation-comments.md#summary)       | Ein Typ oder Member eines Typs zu beschreiben                  |
| `<value>`        | [`<value>`](documentation-comments.md#value)           | Beschreiben einer Eigenschaft                                    |
| `<typeparam>`    |                                                        | Beschreiben Sie einen generischen Typparameter                      |
| `<typeparamref>` |                                                        | Erkennen Sie, dass ein Wort einen Parameternamen handelt          |

### `<c>`

Dieses Tag stellt einen Mechanismus, um anzugeben, dass ein Fragment von Text in einer Beschreibung in einer speziellen Schriftart wie z. B., verwendet für einen Codeblock festgelegt werden soll. Verwenden Sie für die Feldlinien des tatsächlichen Code, `<code>` ([`<code>`](documentation-comments.md#code)).

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

Dieses Tag wird verwendet, eine oder mehrere Zeilen von Code oder Programmausgabe Quellausgabe in einige besondere Schriftart festlegen. Verwenden Sie für kleine Codefragmente in die Geschichte hinter dem, `<c>` ([`<c>`](documentation-comments.md#c)).

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

Dieses Tag ermöglicht Beispielcode in einem Kommentar, um anzugeben, wie eine Methode oder eines anderen Bibliothekmembers verwendet werden kann. Normalerweise würde dies auch die Verwendung des Tags betreffen `<code>` ([`<code>`](documentation-comments.md#code)) ebenfalls.

__Syntax:__

```xml
<example>description</example>
```

__Beispiel:__

Finden Sie unter `<code>` ([`<code>`](documentation-comments.md#code)) ein Beispiel.

### `<exception>`

Dieses Tag bietet eine Möglichkeit, die Ausnahmen zu dokumentieren, die eine Methode ausgelöst werden kann.

__Syntax:__

```xml
<exception cref="member">description</exception>
```

wo

* `member` ist der Name eines Elements. Der Dokumentations-Generator überprüft, ob der angegebene Member vorhanden ist und übersetzt `member` auf den kanonischen Elementnamen in der Dokumentationsdatei.
* `description` ist eine Beschreibung der Umstände, in denen die Ausnahme ausgelöst wird.

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

Dieses Tag ermöglicht, einschließlich der Informationen aus einem XML-Dokument, das außerhalb der Quellcodedatei ist. Die externe Datei muss ein wohlgeformtes XML-Dokument sein, und ein XPath-Ausdruck an, welche XML-Code aus diesem Dokument sollen Geben Sie das Dokument angewendet wird. Die `<include>` Tag wird dann mit den ausgewählten XML-Code aus dem externen Dokument ersetzt.

__Syntax:__

```
<include file="filename" path="xpath" />
```

wo

* `filename` ist der Dateiname einer externen XML-Datei an. Der Dateiname ist relativ zur Datei interpretiert, die die Include-Tag enthält.
* `xpath` ist ein XPath-Ausdruck, der Teil der XML-Code in der externen XML-Datei auswählt.

__Beispiel:__

Wenn der Quellcode eine Deklaration wie enthalten:

```csharp
/// <include file="docs.xml" path='extradoc/class[@name="IntList"]/*' />
public class IntList { ... }
```

und die externe Datei "docs.xml" hatte den folgenden Inhalt:

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

Klicken Sie dann ist die gleiche Dokumentation Ausgabe aus, als ob der Quellcode enthalten:

```csharp
/// <summary>
///    Contains a list of integers.
/// </summary>
public class IntList { ... }
```

### `<list>`

Dieses Tag wird verwendet, um eine Liste oder Tabelle der Elemente zu erstellen. Sie enthält eventuell eine `<listheader>` Block, um die Überschriftenzeile einer Tabelle oder einer Definitionsliste zu definieren. (Beim Definieren einer Tabelle, die nur einen Eintrag für `term` in der Überschrift müssen angegeben werden.)

Jedes Element in der Liste wird angegeben, mit einem `<item>` Block. Beim Erstellen einer Definitionsliste sowohl `term` und `description` muss angegeben werden. Allerdings für eine Tabelle, Liste mit Aufzählungszeichen oder nummerierte Liste nur `description` muss angegeben werden.

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

* `term` ist der Begriff definiert, deren Definition befindet sich in `description`.
* `description` ist entweder ein Element in einer Aufzählung oder nummerierten Liste oder die Definition einer `term`.

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

Dieses Tag ist für die Verwendung in anderen Tags, wie z. B. `<summary>` ([`<remark>`](documentation-comments.md#remark)) oder `<returns>` ([`<returns>`](documentation-comments.md#returns)), und lässt die Struktur, die Text hinzugefügt werden.

__Syntax:__

```xml
<para>content</para>
```

wo `content` ist der Text des Absatzes.

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

Dieses Tag wird verwendet, um einen Parameter für eine Methode, einem Konstruktor oder einem Indexer zu beschreiben.

__Syntax:__

```xml
<param name="name">description</param>
```

wo

* `name` Ist der Name des Parameters.
* `description` ist eine Beschreibung des Parameters.

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

Dieses Tag wird verwendet, um anzugeben, dass ein Wort ein Parameter ist. Die Dokumentationsdatei kann verarbeitet werden, um diesen Parameter auf unterschiedliche Weise zu formatieren.

__Syntax:__

```xml
<paramref name="name"/>
```

wo `name` ist der Name des Parameters.

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

Dieses Tag ermöglicht den Sicherheitszugriff auf einen Member dokumentiert werden soll.

__Syntax:__

```xml
<permission cref="member">description</permission>
```

wo

* `member` ist der Name eines Elements. Der Dokumentations-Generator überprüft, ob das angegebene Codeelement vorhanden ist und übersetzt *Member* auf den kanonischen Elementnamen in der Dokumentationsdatei.
* `description` ist eine Beschreibung des Zugriffs auf das Element.

__Beispiel:__

```csharp
/// <permission cref="System.Security.PermissionSet">Everyone can
/// access this method.</permission>

public static void Test() {
    // ...
}
```

### `<remark>`

Dieses Tag wird verwendet, um zusätzliche Informationen über einen Typ anzugeben. (Verwenden `<summary>` ([`<summary>`](documentation-comments.md#summary)) um den Typ selbst und die Member eines Typs beschreiben.)

__Syntax:__

```xml
<remark>description</remark>
```

wo `description` ist der Text, der die Anmerkung.

__Beispiel:__

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

Dieses Tag wird verwendet, um den Rückgabewert einer Methode zu beschreiben.

__Syntax:__

```xml
<returns>description</returns>
```

wo `description` finden Sie eine Beschreibung des Rückgabewerts.

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

Dieses Tag ermöglicht einen Link im Text angegeben werden. Verwendung `<seealso>` ([`<seealso>`](documentation-comments.md#seealso)) um Text anzugeben, die in einem Abschnitt Siehe auch angezeigt werden soll.

__Syntax:__

```xml
<see cref="member"/>
```

wo `member` ist der Name eines Elements. Der Dokumentations-Generator überprüft, ob das angegebene Codeelement vorhanden ist und ändert *Member* an den Elementnamen in der der generierten Dokumentationsdatei.

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

Dieses Tag ermöglicht einen Eintrag im Abschnitt Siehe auch generiert werden soll. Verwendung `<see>` ([`<see>`](documentation-comments.md#see)), geben Sie einen Link im Text.

__Syntax:__

```xml
<seealso cref="member"/>
```

wo `member` ist der Name eines Elements. Der Dokumentations-Generator überprüft, ob das angegebene Codeelement vorhanden ist und ändert *Member* an den Elementnamen in der der generierten Dokumentationsdatei.

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

Dieses Tag kann verwendet werden, um einen Typ oder Member eines Typs beschreiben. Verwendung `<remark>` ([`<remark>`](documentation-comments.md#remark)) um den Typ selbst beschreiben.

__Syntax:__

```xml
<summary>description</summary>
```

wo `description` ist eine Zusammenfassung der den Typ oder Member.

__Beispiel:__

```csharp
/// <summary>This constructor initializes the new Point to (0,0).</summary>
public Point() : this(0,0) {
}
```

### `<value>`

Dieses Tag kann eine Eigenschaft, um die beschrieben werden.

__Syntax:__

```xml
<value>property description</value>
```

wo `property description` finden Sie eine Beschreibung für die Eigenschaft.

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

Dieses Tag wird verwendet, um einen generischen Typparameter für eine Klasse, Struktur, Schnittstelle, Delegat oder Methode zu beschreiben.

__Syntax:__

```xml
<typeparam name="name">description</typeparam>
```

wo `name` ist der Name des Typparameters, und `description` wird seine Beschreibung.

__Beispiel:__

```csharp
/// <summary>A generic list class.</summary>
/// <typeparam name="T">The type stored by the list.</typeparam>
public class MyList<T> {
    ...
}
```

### `<typeparamref>`

Dieses Tag wird verwendet, um anzugeben, dass ein Wort ein Typparameter ist. Die Dokumentationsdatei kann verarbeitet werden, um diesen Typparameter auf unterschiedliche Weise zu formatieren.

__Syntax:__

```xml
<typeparamref name="name"/>
```

wo `name` ist der Name des Typparameters.

__Beispiel:__

```csharp
/// <summary>This method fetches data and returns a list of <typeparamref name="T"/>.</summary>
/// <param name="query">query to execute</param>
public List<T> FetchData<T>(string query) {
    ...
}
```

## <a name="processing-the-documentation-file"></a>Verarbeiten der Dokumentationsdatei

Der Dokumentations-Generator generiert eine ID-Zeichenfolge für jedes Element im Quellcode, der mit einem Dokumentationskommentar gekennzeichnet ist. Diese ID-Zeichenfolge identifiziert eindeutig ein Quellelement. Eine ID-Zeichenfolge können ein Dokumentations-Viewer die entsprechenden Metadaten/Reflektionselement identifizieren, die Dokumentation gilt.

Die Dokumentationsdatei ist keine hierarchische Darstellung des Quellcodes; Es handelt sich vielmehr um eine flache Liste mit einer generierten ID-Zeichenfolge für jedes Element.

### <a name="id-string-format"></a>Zeichenfolge-ID-format

Der Dokumentations-Generator verwendet die folgenden Regeln beim Generieren der ID-Zeichenfolgen:

*  In der Zeichenfolge wird kein Leerraum platziert.

*  Der erste Teil der Zeichenfolge identifiziert die Art von Member dokumentiert wird, über ein einzelnes Zeichen, gefolgt von einem Doppelpunkt. Die folgenden Arten von Membern sind definiert:

   | __Zeichen__ | __Beschreibung__                                             |
   |---------------|-------------------------------------------------------------|
   | E             | event                                                       |
   | F             | Feld                                                       |
   | M             | Methode (einschließlich Konstruktoren, Destruktoren und Operatoren) |
   | N             | Namespace                                                   |
   | P             | Eigenschaft (einschließlich Indexer)                               |
   | T             | Geben Sie (z. B. Klasse, Delegat, Enumeration, Schnittstelle und Struktur) |
   | !             | Fehlermeldungs-Zeichenfolge; der Rest der Zeichenfolge enthält Informationen zum Fehler. Der Dokumentations-Generator generiert z. B. Fehlerinformationen für Links, die nicht aufgelöst werden kann. |

*  Der zweite Teil der Zeichenfolge ist der vollqualifizierte Name des Elements im Stammverzeichnis des Namespace ab. Der Name des Elements, dessen einschließenden Typen und Namespace sind durch Punkte getrennt. Wenn der Name des Elements selbst Punkte enthält, werden sie durch ersetzt `#(U+0023)` Zeichen. (Es wird vorausgesetzt, dass kein Element dieses Zeichen im Namen hat.)
*  Listen Sie für die Methoden und Eigenschaften mit Argumenten das Argument folgt in Klammern eingeschlossen. Für diejenigen ohne Argumente werden keine Klammern verwendet. Die Argumente werden durch Kommas voneinander getrennt. Die Codierung jedes Arguments ist identisch mit einer CLI-Signatur wie folgt:
   *  Argumente werden anhand des Namens Dokumentation dargestellt, basierend auf dem vollqualifizierten Namen, die wie folgt geändert:
      * Argumente, die generische Typen darstellen müssen ein angefügtes `` ` `` (Gravis)-Zeichens, gefolgt von der Anzahl von Typparametern
      * Argumente, die mit der `out` oder `ref` -Modifizierer aufweisen. eine `@` nach ihren Namen eingeben. Argumente zu übergeben, als Wert oder über `params` haben keine besondere Schreibweise.
      * Argumente, die Arrays werden als dargestellt `[lowerbound:size, ... , lowerbound:size]` , in dem die Anzahl von Kommas ist der Rang minus 1, und die unteren Grenzen und die Größe jeder Dimension, sofern bekannt, Dezimal dargestellt werden. Wenn die untere Grenze oder die Größe nicht angegeben ist, wird es weggelassen. Wenn die untere Grenze und die Größe für eine bestimmte Dimension ausgelassen werden, die `:` ebenfalls ausgelassen werden. Verzweigte Arrays werden von einem dargestellt `[]` pro Ebene.
      * Argumente, die Zeiger als "void" aufweisen, werden mit dargestellt eine `*` nach dem Typnamen. Ein void-Zeiger wird dargestellt, verwenden den Typnamen `System.Void`.
      * Argumente, die die generischen Typparametern definierten Typen finden Sie codiert werden, mithilfe der `` ` `` (Gravis)-Zeichens, gefolgt von der nullbasierte Index des Typparameters.
      * Generischen Typparametern in Methoden definiert mithilfe von Argumenten verwenden ein Double-Wert-Hochkomma ``` `` ``` statt der `` ` `` für Typen verwendet.
      * Argumente, die konstruierte generische Typen verweisen, codiert werden, verwenden den generischen Typ an, gefolgt von `{`, gefolgt von einer durch Trennzeichen getrennte Liste der Argumente des Typs, gefolgt von `}`.

### <a name="id-string-examples"></a>Beispiele für die ID-Zeichenfolge

Die folgenden Beispiele wird jeder zeigen ein Fragment des C#-Code zusammen mit der ID-Zeichenfolge, die von jedem Quellelement für einen Dokumentationskommentar erstellt:

*  Typen werden unter Verwendung des vollqualifizierten Namens, getragenen allgemeine Informationen dargestellt:

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

*  Felder werden durch ihre vollqualifizierten Namen dargestellt:

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

*  -Methoden.

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

*  Ereignisse.

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

   Der vollständige Satz von Funktionsnamen für unären Operator verwendet, lautet wie folgt: `op_UnaryPlus`, `op_UnaryNegation`, `op_LogicalNot`, `op_OnesComplement`, `op_Increment`, `op_Decrement`, `op_True`, und `op_False`.

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

   Der vollständige Satz von binären Operator Funktionsnamen verwendet, lautet wie folgt: `op_Addition`, `op_Subtraction`, `op_Multiply`, `op_Division`, `op_Modulus`, `op_BitwiseAnd`, `op_BitwiseOr`, `op_ExclusiveOr`, `op_LeftShift`, `op_RightShift`, `op_Equality`, `op_Inequality`, `op_LessThan`, `op_LessThanOrEqual`, `op_GreaterThan`, und `op_GreaterThanOrEqual`.

*  Konvertierungsoperatoren verfügen über kein nachfolgendes Zeichen "`~`" gefolgt von den Rückgabetyp.

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

### <a name="c-source-code"></a>C#-Quellcode

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

### <a name="resulting-xml"></a>XML-Ergebnis

Hier ist die Ausgabe einen Dokumentations-Generator, wenn den Quellcode für die Klasse `Point`, wie oben gezeigt:

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
