---
ms.openlocfilehash: 392d932459ff0a4cb0d6d32c1606f73f9b913c68
ms.sourcegitcommit: 94a3d151c438d34ede1d99de9eb4ebdc07ba4699
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/25/2019
ms.locfileid: "79483457"
---
# <a name="covariant-return-types"></a>kovariante Rückgabe Typen

* [x] vorgeschlagen
* [] Prototyp: nicht gestartet
* [] Implementierung: nicht gestartet
* [] Spezifikation: nicht gestartet

## <a name="summary"></a>Zusammenfassung
[summary]: #summary

Unterstützung _kovariant-Rückgabe Typen_. Insbesondere ermöglichen Sie einer über schreibenden Methode einen stärker abgeleiteten Verweistyp als die Methode, die Sie überschreibt.

## <a name="motivation"></a>Motivation
[motivation]: #motivation

Es ist ein gängiges Muster im Code, dass verschiedene Methodennamen erfunden werden müssen, um die sprach Einschränkung zu umgehen, dass über schreibungen denselben Typ zurückgeben müssen wie die überschriebene Methode. Im folgenden finden Sie ein Beispiel aus der Roslyn-Codebasis.

## <a name="detailed-design"></a>Detaillierter Entwurf
[design]: #detailed-design

Unterstützung _kovariant-Rückgabe Typen_. Insbesondere ermöglichen Sie einer über schreibenden Methode einen stärker abgeleiteten Verweistyp als die Methode, die Sie überschreibt. Dies gilt für Methoden und Eigenschaften und wird in Klassen und Schnittstellen unterstützt.

Dies wäre für das Factorymuster nützlich. In der Roslyn-Codebasis haben wir beispielsweise Folgendes:

``` cs
class Compilation ...
{
    virtual Compilation WithOptions(Options options)...
}
```

``` cs
class CSharpCompilation : Compilation
{
    override CSharpCompilation WithOptions(Options options)...
}
```

Die Implementierung dieser Methode besteht darin, dass der Compiler die über schreibende Methode als "neue" virtuelle Methode ausgibt, die die Basisklassen Methode blendet, zusammen mit einer _Bridge Methode_ , die die Basisklassen Methode mit einem aufzurufenden Befehl an die Methode der abgeleiteten Klasse implementiert.

## <a name="drawbacks"></a>Nachteile
[drawbacks]: #drawbacks

- [] Jede sprach Änderung muss für sich selbst bezahlen.
- [] Wir sollten sicherstellen, dass die Leistung angemessen ist, auch im Fall von tiefen Vererbungs Hierarchien.
- [] Wir sollten sicherstellen, dass sich Artefakte der Übersetzungsstrategie nicht auf die sprach Semantik auswirken, auch wenn Sie eine neue Il aus alten Compilern nutzen.

## <a name="alternatives"></a>Alternativen
[alternatives]: #alternatives

Wir könnten die Sprachregeln so lockern, dass Sie in der Quelle

```csharp
abstract class Cloneable
{
    public abstract Cloneable Clone();
}

class Digit : Cloneable
{
    public override Cloneable Clone()
    {
        return this.Clone();
    }

    public new Digit Clone() // Error: 'Digit' already defines a member called 'Clone' with the same parameter types
    {
        return this;
    }
}
```

## <a name="unresolved-questions"></a>Nicht aufgelöste Fragen
[unresolved]: #unresolved-questions

- [] Wie funktionieren APIs, die zur Verwendung dieses Features kompiliert wurden, in älteren Versionen der Sprache?

## <a name="design-meetings"></a>Treffen von Besprechungen

Noch keine. Bei <https://github.com/dotnet/roslyn/issues/357>wurden einige Diskussionen erläutert.