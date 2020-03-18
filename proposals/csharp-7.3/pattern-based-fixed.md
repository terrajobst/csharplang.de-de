---
ms.openlocfilehash: 2070cf3b3269585055791adc3427cbd134df444d
ms.sourcegitcommit: 94a3d151c438d34ede1d99de9eb4ebdc07ba4699
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/25/2019
ms.locfileid: "79483631"
---
# <a name="pattern-based-fixed-statement"></a>Musterbasierte `fixed`-Anweisung

## <a name="summary"></a>Zusammenfassung
[summary]: #summary

Stellen Sie ein Muster vor, das es Typen ermöglicht, an `fixed`-Anweisungen teilzunehmen. 

## <a name="motivation"></a>Motivation
[motivation]: #motivation

Die Sprache bietet einen Mechanismus zum Fixieren von verwalteten Daten und zum Abrufen eines systemeigenen Zeigers auf den zugrunde liegenden Puffer.

```csharp
fixed(byte* ptr = byteArray)
{
   // ptr is a native pointer to the first element of the array
   // byteArray is protected from being moved/collected by the GC for the duration of this block 
}

```

Der Satz von Typen, die an `fixed` teilnehmen können, ist hart codiert und auf Arrays und `System.String`beschränkt. Das hart codieren von "speziellen" Typen wird nicht skaliert, wenn neue Primitive wie `ImmutableArray<T>`, `Span<T>``Utf8String` eingeführt werden. 

Darüber hinaus basiert die aktuelle Lösung für `System.String` auf einer recht starren API. Die Form der API impliziert, dass `System.String` ein zusammenhängendes Objekt ist, das UTF16 codierte Daten an einem festgelegten Offset aus dem Objekt Header einbettet. Diese Vorgehensweise wurde in mehreren Vorschlägen als problematisch eingestuft, die Änderungen am zugrunde liegenden Layout erfordern könnten. Es wäre wünschenswert, zu einem flexibleren wechseln zu können, das `System.String` Objekt von seiner internen Darstellung zum Zweck der nicht verwalteten Interop entkoppelt. 

## <a name="detailed-design"></a>Detaillierter Entwurf
[design]: #detailed-design

## <a name="pattern"></a>*Muster* ##
Ein brauchbares, auf Mustern basierendes "Festes" muss Folgendes sein:
-   Stellen Sie die verwalteten Verweise bereit, um die Instanz anzuheften und den Zeiger zu initialisieren (vorzugsweise ist dies derselbe Verweis).
-   Vermitteln Sie eindeutig den Typ des nicht verwalteten Elements (d. h. "char" für "String")
-   Weisen Sie das Verhalten im "leeren" Fall an, wenn keine Verweise vorhanden sind. 
-   Sollten API-Autoren nicht in Bezug auf Entwurfsentscheidungen pushen, die die Verwendung des Typs außerhalb `fixed`beeinträchtigen.

Ich bin der Ansicht, dass die oben genannten Elemente durch das Erkennen eines speziell benannten Ref-zurück gebenden Members erfüllt werden können: `ref [readonly] T GetPinnableReference()`.

Die folgenden Bedingungen müssen erfüllt sein, damit Sie von der `fixed`-Anweisung verwendet werden können:

1. Es ist nur ein solcher Member für einen Typ angegeben.
1. Wird von `ref` oder `ref readonly`zurückgegeben. (`readonly` ist zulässig, damit Autoren von unveränderlichen/schreibgeschützten Typen das Muster implementieren können, ohne eine beschreibbare API hinzuzufügen, die in sicherem Code verwendet werden kann.)
1. T ist ein nicht verwalteter Typ.
(da `T*` der Zeigertyp wird. Die Einschränkung wird auf natürliche Weise erweitert, wenn das Konzept "nicht verwaltet" erweitert wird.
1. Gibt verwaltete `nullptr` zurück, wenn keine Daten zum Anheften vorhanden sind – wahrscheinlich die günstigste Methode zum vermitteln von leergaben.
(Beachten Sie, dass die Zeichenfolge "" einen Verweis auf "\ 0" zurückgibt, da Zeichen folgen auf Null enden).

Alternativ `#3` dazu können wir das Ergebnis in leeren Fällen als nicht definiert oder Implementierungs spezifisch zulassen. Dies kann jedoch die API gefährlicher machen und anfällig für Missbrauch und unbeabsichtigte Kompatibilitäts Belastungen werden. 

## <a name="translation"></a>*Übersetzung* ##

```csharp
fixed(byte* ptr = thing)
{ 
    // <BODY>
}
```

wird zum folgenden Pseudo Code (nicht alles Ausdrucks in C#)

```csharp
byte* ptr;
// specially decorated "pinned" IL local slot, not visible to user code.
pinned ref byte _pinned;

try
{
    // NOTE: null check is omitted for value types 
    // NOTE: `thing` is evaluated only once (temporary is introduced if necessary) 
    if (thing != null)
    {
        // obtain and "pin" the reference
        _pinned = ref thing.GetPinnableReference();

        // unsafe cast in IL
        ptr = (byte*)_pinned;
    }
    else
    {
        ptr = default(byte*);
    }

    // <BODY> 
}
finally   // finally can be omitted when not observable
{
    // "unpin" the object
    _pinned = nullptr;
}
```

## <a name="drawbacks"></a>Nachteile
[drawbacks]: #drawbacks

- Getpinnablereferenzierungsweise ist nur in `fixed`gedacht, aber nichts hindert die Verwendung in sicherem Code, weshalb dies von implemenor berücksichtigt werden muss.

## <a name="alternatives"></a>Alternativen
[alternatives]: #alternatives

Benutzer können getpinablereferenzierungsart oder ähnliches Mitglied einführen und als
 
```csharp
fixed(byte* ptr = thing.GetPinnableReference())
{ 
    // <BODY>
}
```

Es gibt keine Lösung für `System.String`, wenn eine alternative Lösung gewünscht ist.

## <a name="unresolved-questions"></a>Nicht aufgelöste Fragen
[unresolved]: #unresolved-questions

- [] Verhalten im "leeren" Zustand. - `nullptr` oder `undefined`? 
- [] Sollten die Erweiterungs Methoden berücksichtigt werden? 
- [] Wenn ein Muster auf `System.String`erkannt wird, sollte es gewinnen? 

## <a name="design-meetings"></a>Treffen von Besprechungen

Noch keine. 
