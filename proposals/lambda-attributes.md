---
ms.openlocfilehash: 9647fff40a1e45bef917f140612ae4e91abea958
ms.sourcegitcommit: 94a3d151c438d34ede1d99de9eb4ebdc07ba4699
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/25/2019
ms.locfileid: "79483559"
---
# <a name="lambda-attributes"></a>Lambda-Attribute

* [x] vorgeschlagen
* [] Prototyp
* []-Implementierung
* []-Spezifikation

## <a name="summary"></a>Zusammenfassung
[summary]: #summary

Ermöglicht das Anwenden von Attributen auf Lambdas (und anonyme Methoden) und auf Parameter der Lambda-/anonymen Methode, da Sie auf reguläre Methoden angewendet werden können.

## <a name="motivation"></a>Motivation
[motivation]: #motivation

Zwei Hauptgründe:

1. Zum Bereitstellen von Metadaten, die Analysen zur Kompilierzeit sichtbar sind.
2. Zum Bereitstellen von Metadaten, die für die Reflektion und die Tools zur Laufzeit sichtbar sind.

Als Beispiel für (1): bei Leistungs sensiblem Code ist es hilfreich, wenn Sie über einen Analyzer verfügen, der die Zuordnung von Abschlüssen und Delegaten für Lambdas zuweist, die den Zustand überschreiten.  Häufig geht ein Entwickler dieses Codes von seiner Methode aus, um die Erfassung eines Zustands zu vermeiden, sodass der Compiler eine statische Methode und einen zwischen speicherbaren Delegaten für die Methode generieren kann, oder der Entwickler stellt sicher, dass der einzige Zustand, der geschlossen wird, `this`ist, sodass der Compiler zumindest die Zuordnung eines Closure-Objekts vermeiden kann.  Ohne Sprachunterstützung für das Einschränken der erfassten Elemente ist es jedoch nicht einfach, versehentlich den Zustand zu schließen.  Es wäre hilfreich, wenn ein Entwickler Lambdas mit Attributen kommentieren könnte, um anzugeben, in welchem Zustand Sie geschlossen werden dürfen, z. b.:

```csharp
[CaptureNone] // can't close over any instance state
[CaptureThis] // can only capture `this` and no other instance state
[CaptureAny] // can close over any instance state
```

Anschließend kann ein Analyzer geschrieben werden, um zu markieren, wenn der Zustand falsch aufgezeichnet wird, z. b.:

```csharp
var results = collection.Select([CaptureNone](i) => Process(item)); // Analyzer error: [CaptureNone] lambdas captures `this`
...
private U Process(T item) { ... }
```

## <a name="detailed-design"></a>Detaillierter Entwurf
[design]: #detailed-design

- Wenn Sie dieselbe Attribut Syntax wie bei normalen Methoden verwenden, können Attribute am Anfang einer Lambda-oder anonymen Methode angewendet werden, z. b.:

```csharp
[SomeAttribute(...)] () => { ... }
[SomeAttribute(...)] delegate (int i) { ... }
```

- Um Mehrdeutigkeiten zu vermeiden, ob ein Attribut für die Lambda-Methode oder eines der Argumente gilt, dürfen Attribute nur verwendet werden, wenn Klammern um Argumente verwendet werden, z. b.:

```csharp
[SomeAttribute] i => { ... } // ERROR
[SomeAttribute] (i) => { ... } // Ok
[SomeAttribute] (int i) => { ... } // Ok
```

- Bei anonymen Methoden werden keine Parser benötigt, um ein Attribut auf die Methode vor dem `delegate`-Schlüsselwort anzuwenden, z. b.:

```csharp
[SomeAttribute] delegate { ... } // Ok
[SomeAttribute] delegate (int i) => { ... } // Ok
```

- Mehrere Attribute können entweder über eine standardmäßige, durch Trennzeichen getrennte Syntax oder über eine vollständige Attribut Syntax angewendet werden, z. b.:

```csharp
[FirstAttribute, SecondAttribute] (i) => { ... } // Ok
[FirstAttribute] [SecondAttribute] (i) => { .... } // Ok
```

- Attribute können auf die Parameter einer anonymen Methode oder eines Lambda-Ausdrucks angewendet werden, jedoch nur, wenn Klammern um Argumente herum verwendet werden, z. b.:

```csharp
[SomeAttribute] i => { ... } // ERROR
([SomeAttribute] i) => { .... } // Ok
([SomeAttribute] int i) => { ... } // Ok
([SomeAttribute] i, [SomeOtherAttribute] j) => { ... } // Ok
```

- Mehrere Attribute können auf die Parameter einer anonymen Methode oder eines Lambda-Ausdrucks angewendet werden, indem entweder die durch Kommas getrennte oder vollständige Attribut Syntax verwendet wird, z. b.:

```csharp
([FirstAttribute, SecondAttribute] i) => { ... } // Ok
([FirstAttribute] [SecondAttribute] i) => { ... } // Ok
```

- `return`Ziel Attribute können auch in Lambda-Ausdrücken verwendet werden, z. b.:

```csharp
([return: SomeAttribute] (i) => { ... }) // Ok
```

- Der Compiler gibt die Attribute für die generierte Methode und die Argumente für diese Methoden wie für jede andere Methode aus.

## <a name="drawbacks"></a>Nachteile
[drawbacks]: #drawbacks

–

## <a name="alternatives"></a>Alternativen
[alternatives]: #alternatives

–

## <a name="unresolved-questions"></a>Nicht aufgelöste Fragen
[unresolved]: #unresolved-questions

–

## <a name="design-meetings"></a>Treffen von Besprechungen

–