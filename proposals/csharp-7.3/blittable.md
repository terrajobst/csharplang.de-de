---
ms.openlocfilehash: 11e9d21bda9e69be692c5c5f5aee80c2da1894ab
ms.sourcegitcommit: 94a3d151c438d34ede1d99de9eb4ebdc07ba4699
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/25/2019
ms.locfileid: "79483601"
---
# <a name="unmanaged-type-constraint"></a>Nicht verwaltete Typeinschränkung

## <a name="summary"></a>Zusammenfassung
[summary]: #summary

Die nicht verwaltete Einschränkungs Funktion bietet eine sprach Erzwingung für die Klasse von Typen, die in der C# Sprachspezifikation als "nicht verwaltete Typen" bezeichnet werden.  Dies wird im Abschnitt 18,2 als Typ definiert, der kein Referenztyp ist und keine Verweistyp Felder auf einer beliebigen Schachtelungs Ebene enthält.  

## <a name="motivation"></a>Motivation
[motivation]: #motivation

Die Hauptmotivation besteht darin, das Erstellen von Interop-Code auf niedriger Ebene C#in zu vereinfachen. Nicht verwaltete Typen sind einer der Kern Bausteine für Interop-Code, aber aufgrund der fehlenden Unterstützung in Generika ist es unmöglich, wiederverwendbare Routinen für alle nicht verwalteten Typen zu erstellen. Stattdessen sind Entwickler gezwungen, den gleichen Code für die kesselplatte für jeden nicht verwalteten Typ in der Bibliothek zu erstellen:

```csharp
int Hash(Point point) { ... } 
int Hash(TimeSpan timeSpan) { ... } 
```

Um diese Art von Szenario zu ermöglichen, wird in der Sprache eine neue Einschränkung eingeführt: nicht verwaltet:

```csharp
void Hash<T>(T value) where T : unmanaged
{
    ...
}
```

Diese Einschränkung kann nur von Typen erfüllt werden, die in die nicht verwaltete Typdefinition in der C# Sprachspezifikation passen. Eine andere Möglichkeit der Betrachtung besteht darin, dass ein Typ das nicht verwaltete Einschränkungs-IFF erfüllt, das auch als Zeiger verwendet werden kann. 

```csharp
Hash(new Point()); // Okay 
Hash(42); // Okay
Hash("hello") // Error: Type string does not satisfy the unmanaged constraint
```

Typparameter mit der nicht verwalteten Einschränkung können alle Features verwenden, die für nicht verwaltete Typen verfügbar sind: Zeiger, korrigiert usw... 

```csharp
void Hash<T>(T value) where T : unmanaged
{
    // Okay
    fixed (T* p = &value) 
    { 
        ...
    }
}
```

Diese Einschränkung ermöglicht außerdem die effiziente Konvertierung zwischen strukturierten Daten und Streams von Bytes. Dies ist ein Vorgang, der in Netzwerk Stapeln und serialisierungsschichten üblich ist:

```csharp
Span<byte> Convert<T>(ref T value) where T : unmanaged 
{
    ...
}
```

Solche Routinen sind von Vorteil, da Sie zur Kompilierzeit und Zuordnungs Kosten sicher sind.  Interop-Autoren können dies nicht tun (auch wenn es sich um eine Ebene handelt, bei der Leistungs wichtig ist).  Stattdessen müssen Sie Routinen verwenden, die über teure Laufzeitüberprüfungen verfügen, um sicherzustellen, dass die Werte ordnungsgemäß nicht verwaltet werden.

## <a name="detailed-design"></a>Detaillierter Entwurf
[design]: #detailed-design

In der Sprache wird eine neue Einschränkung mit dem Namen `unmanaged`eingeführt. Um diese Einschränkung zu erfüllen, muss ein Typ eine Struktur sein, und alle Felder des Typs müssen in eine der folgenden Kategorien fallen:

- Sie haben den Typ `sbyte`, `byte`, `short`, `ushort`, `int`, `uint`, `long`, `ulong`, `char`, `float`, `double`, `decimal`, `bool`, `IntPtr` oder `UIntPtr`.
- Jeder `enum` Typ.
- Ist ein Zeigertyp.
- Dabei handelt es sich um eine benutzerdefinierte Struktur, die die `unmanaged` Einschränkung.

Vom Compiler generierte Instanzfelder, wie z. b. solche, die automatisch implementierte Eigenschaften unterstützen, müssen diese Einschränkungen ebenfalls erfüllen. 

Beispiel:

```csharp
// Unmanaged type
struct Point 
{ 
    int X;
    int Y {get; set;}
}

// Not an unmanaged type
struct Student 
{ 
    string FirstName;
    string LastName;
}
``` 

Die `unmanaged`-Einschränkung kann nicht mit `struct`, `class` oder `new()`kombiniert werden. Diese Einschränkung leitet sich von der Tatsache ab, dass `unmanaged` `struct` folglich sind die anderen Einschränkungen nicht sinnvoll.

Die `unmanaged` Einschränkung wird nicht von CLR erzwungen, sondern nur von der Sprache. Um die fehl Verwendung durch andere Sprachen zu verhindern, werden Methoden mit dieser Einschränkung durch eine mod-req geschützt. Dadurch wird verhindert, dass andere Sprachen Typargumente verwenden, bei denen es sich nicht um verwaltete Typen handelt.

Das in der Einschränkung `unmanaged` Token ist weder ein Schlüsselwort noch ein Kontext Schlüsselwort. Stattdessen ist es wie `var`, dass es an diesem Speicherort ausgewertet wird und eine der folgenden Aktionen durchführt:

- An benutzerdefinierten oder referenzierten Typ mit dem Namen "`unmanaged`" binden: Dies wird genauso behandelt, wie jede andere benannte Typeinschränkung behandelt wird. 
- Binden an keinen Typ: Dies wird als `unmanaged` Einschränkung interpretiert.

Wenn ein Typ mit dem Namen `unmanaged` vorhanden ist, der ohne Qualifizierung im aktuellen Kontext verfügbar ist, gibt es keine Möglichkeit, die `unmanaged`-Einschränkung zu verwenden. Dies entspricht den Regeln, die die Funktions `var` und benutzerdefinierten Typen desselben Namens betreffen. 

## <a name="drawbacks"></a>Nachteile
[drawbacks]: #drawbacks

Der Hauptnachteil dieses Features besteht darin, dass es eine kleine Anzahl von Entwicklern bietet: in der Regel auf der Basis von Bibliotheks Autoren oder-Frameworks.  Daher wird für eine kleine Anzahl von Entwicklern eine kostbare sprach Zeit aufgewendet. 

Diese Frameworks sind aber häufig die Grundlage für die Mehrzahl der .NET-Anwendungen.  Daher kann die Leistung/Richtigkeit auf dieser Ebene einen Ripple-Effekt auf das .NET-Ökosystem haben.  Dadurch wird das Feature auch mit eingeschränkter Zielgruppe in Erwägung gezogen.

## <a name="alternatives"></a>Alternativen
[alternatives]: #alternatives

Es gibt einige Alternativen zu berücksichtigende Aspekte:

- Der Status quo: das Feature ist nicht für seine eigenen Vorzüge gerechtfertigt, und Entwickler verwenden weiterhin das implizite Opt-in-Verhalten.

## <a name="questions"></a>Fragen
[quesions]: #questions

### <a name="metadata-representation"></a>Metadatendarstellung

Die F# -Sprache codiert die Einschränkung in der Signatur Datei. C# Dies bedeutet, dass ihre Darstellung nicht wieder verwendet werden kann. Für diese Einschränkung muss ein neues Attribut ausgewählt werden. Darüber hinaus muss eine Methode, die diese Einschränkung hat, durch einen mod-req geschützt werden.

### <a name="blittable-vs-unmanaged"></a>Blitfähige und nicht verwaltete
Die F# Sprache verfügt über ein sehr [ähnliches Feature](https://docs.microsoft.com/dotnet/articles/fsharp/language-reference/generics/constraints) , das das Schlüsselwort "nicht verwaltet" verwendet. Der blitfähige Name stammt aus der Verwendung in Midori.  Sie sollten sich hier als Rangfolge ansehen und nicht verwaltete verwenden. 

**Lösung** Die Sprache entscheidet sich für die Verwendung nicht verwalteter 

### <a name="verifier"></a>Verifier

Muss der Verifier/die Laufzeit aktualisiert werden, um die Verwendung von Zeigern auf generische Typparameter zu verstehen?  Oder kann es einfach unverändert funktionieren, ohne dass Änderungen vorgenommen werden?

**Lösung** Es sind keine Änderungen erforderlich. Alle Zeiger Typen sind einfach nicht verifizierbar. 

## <a name="design-meetings"></a>Treffen von Besprechungen

–
