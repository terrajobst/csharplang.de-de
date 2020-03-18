---
ms.openlocfilehash: a8822137c85f449444ed675c6f2912315c041691
ms.sourcegitcommit: 94a3d151c438d34ede1d99de9eb4ebdc07ba4699
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/25/2019
ms.locfileid: "79483475"
---
# <a name="static-delegates"></a>Statische Delegaten

* [x] vorgeschlagen
* [] Prototyp: nicht gestartet
* [] Implementierung: nicht gestartet
* [] Spezifikation: nicht gestartet

## <a name="summary"></a>Zusammenfassung
[summary]: #summary

Stellen Sie eine allgemeine, leichte Rückruffunktion für die C# Sprache bereit.

## <a name="motivation"></a>Motivation
[motivation]: #motivation

Heute haben Benutzer die Möglichkeit, Rückrufe mithilfe des `System.Delegate` Typs zu erstellen. Diese sind jedoch recht schwer zu handhaben (z. b. Wenn eine Heap Zuordnung erforderlich ist und Sie immer mit dem Verketten von Rückrufe arbeiten).

Außerdem bietet `System.Delegate` keinen optimalen Interop mit nicht verwalteten Funktions Zeigern, d. h. aufgrund nicht blitfähiger und erforderlicher Marshalling, wenn die verwaltete/nicht verwaltete Grenze überschritten wird.

Mit einigen geringfügigen Anpassungen könnten wir einen neuen Typ von Delegaten bereitstellen, der einfach, universell und gut mit System eigenem Code ist.

## <a name="detailed-design"></a>Detaillierter Entwurf
[design]: #detailed-design

Zum einen deklarieren Sie einen statischen Delegaten über Folgendes:

```C#
static delegate int Func()
```

Sie können der Deklaration außerdem ein Attribut ähnlich `System.Runtime.InteropServices.UnmanagedFunctionPointer` zuweisen, sodass die Aufruf Konvention, das Marshalling von Zeichen folgen und das Festlegen des letzten Fehler Verhaltens gesteuert werden kann. Hinweis: die Verwendung von `System.Runtime.InteropServices.UnmanagedFunctionPointer` selbst funktioniert nicht, da Sie nur für Delegaten verwendet werden kann.

Die Deklaration würde vom Compiler in eine interne Darstellung übersetzt werden, die der folgenden ähnelt.

```C#
struct <Func>e__StaticDelegate
{
    IntPtr pFunction;

    static int WellKnownCompilerName();
}
```

Das heißt, Sie wird intern durch eine Struktur dargestellt, die über einen einzelnen Member vom Typ "`IntPtr`" verfügt (eine solche Struktur ist blitfähig und verursacht keine Heap Zuweisungen):
* Der Member enthält die Adresse der Funktion, die als Rückruf fungieren soll.
* Der-Typ deklariert eine Methode, die mit der Methoden Signatur des Rückrufs übereinstimmt.
* Der Name der Struktur sollte nicht vom Benutzer konstruiert werden können (wie bei anderen intern generierten Unterstützungsstrukturen).
 * Beispiel: Puffer fester Größe generieren eine Struktur mit einem Namen im Format `<name>e__FixedBuffer` (`<` und `>` sind Teil des Bezeichners und sorgen dafür, dass der Bezeichner nicht in C#konstruktierbar ist, aber trotzdem in Il verwendbar ist).
* Der Name der Methoden Deklaration sollte ein bekannter Name sein, der für alle statischen Delegattypen verwendet wird (Dies ermöglicht es dem Compiler, den Namen zu ermitteln, der beim Bestimmen der Signatur gesucht werden soll).

Der Wert des statischen Delegaten kann nur an eine statische Methode gebunden werden, die mit der Signatur des Rückrufs übereinstimmt.

Das Verketten von Rückrufe wird nicht unterstützt.

Der Aufruf des Rückrufs würde von der `calli` Anweisung implementiert werden.

## <a name="drawbacks"></a>Nachteile
[drawbacks]: #drawbacks

Statische Delegaten können nicht mit vorhandenen APIs verwendet werden, die reguläre Delegaten verwenden (eine muss den genannten statischen Delegaten in einem regulären Delegaten derselben Signatur einschließen).
* Da `System.Delegate` intern als Satz von `object`-und `IntPtr` Feldern (http://source.dot.net/#System.Private.CoreLib/src/System/Delegate.cs)) dargestellt wird, ist es möglich, die implizite Konvertierung eines statischen Delegaten in einen `System.Delegate` zuzulassen, der über eine übereinstimmende Methoden Signatur verfügt. Es wäre auch möglich, eine explizite Konvertierung in umgekehrter Richtung bereitzustellen, sofern die `System.Delegate` für alle Anforderungen eines statischen Delegaten konform ist.

Es ist zusätzliche Arbeit erforderlich, um den statischen Delegaten im Kern Framework problemlos verwendbar zu machen.

## <a name="alternatives"></a>Alternativen
[alternatives]: #alternatives

TBD

## <a name="unresolved-questions"></a>Nicht aufgelöste Fragen
[unresolved]: #unresolved-questions

Welche Teile des Entwurfs sind noch TBD?

## <a name="design-meetings"></a>Treffen von Besprechungen

Verknüpfen Sie die Entwurfs Hinweise, die sich auf diesen Vorschlag auswirken, und beschreiben Sie in einem Satz für jede Änderungen, zu denen Sie geführt haben.


