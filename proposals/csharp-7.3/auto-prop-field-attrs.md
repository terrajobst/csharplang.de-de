---
ms.openlocfilehash: 2e054c629f71ae37b112300905c3106f9ce977e8
ms.sourcegitcommit: 94a3d151c438d34ede1d99de9eb4ebdc07ba4699
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/25/2019
ms.locfileid: "79483637"
---
# <a name="auto-implemented-property-field-targeted-attributes"></a>Automatisch implementierte Eigenschaften Feld-Ziel Attribute

## <a name="summary"></a>Zusammenfassung
[summary]: #summary

Diese Funktion soll es Entwicklern ermöglichen, Attribute direkt auf die Unterstützungs Felder von automatisch implementierten Eigenschaften anzuwenden.

## <a name="motivation"></a>Motivation
[motivation]: #motivation

Derzeit ist es nicht möglich, Attribute auf die Unterstützungs Felder von automatisch implementierten Eigenschaften anzuwenden.  In Fällen, in denen der Entwickler ein Attribut für die Feld Ausrichtung verwenden muss, wird er gezwungen, das Feld manuell zu deklarieren und die ausführlichere Eigenschaften Syntax zu verwenden.  Wenn die C# feldorientierten Attribute für das generierte Unterstützungs Feld für Ereignisse immer von unterstützt werden, ist es sinnvoll, die gleiche Funktionalität auf ihre Eigenschaften-Kin auszuweiten.

## <a name="detailed-design"></a>Detaillierter Entwurf
[design]: #detailed-design

Kurz gesagt, wäre Folgendes zulässig, C# und es wird keine Warnung ausgegeben:

```csharp
[Serializable]
public class Foo 
{
    [field: NonSerialized]
    public string MySecret { get; set; }
}
```

Dies würde dazu führen, dass die feldorientierten Attribute auf das vom Compiler generierte Unterstützungs Feld angewendet werden:

```csharp
[Serializable]
public class Foo 
{
    [NonSerialized]
    private string _mySecretBackingField;
    
    public string MySecret
    {
        get { return _mySecretBackingField; }
        set { _mySecretBackingField = value; }
    }
}
```

Wie bereits erwähnt, wird die Parität mit der Ereignis C# Syntax von 1,0 bewirkt, da Folgendes bereits zulässig ist und sich wie erwartet verhält:

```csharp
[Serializable]
public class Foo
{
    [field: NonSerialized]
    public event EventHandler MyEvent;
}
```

## <a name="drawbacks"></a>Nachteile
[drawbacks]: #drawbacks

Es gibt zwei mögliche Nachteile bei der Implementierung dieser Änderung:

1. Wenn Sie versuchen, ein Attribut auf das Feld einer automatisch implementierten Eigenschaft anzuwenden, wird eine Compilerwarnung erzeugt, dass die Attribute in diesem Block ignoriert werden.  Wenn der Compiler dahingehend geändert wurde, dass diese Attribute unterstützt werden, werden Sie auf das dahinter liegende Feld bei einer nachfolgenden Neukompilierung angewendet, wodurch das Verhalten des Programms zur Laufzeit geändert werden kann.
1. Der Compiler überprüft derzeit nicht die AttributeUsage-Ziele der Attribute, wenn versucht wird, diese auf das Feld der automatisch implementierten Eigenschaft anzuwenden.  Wenn der Compiler geändert wurde, um feldorientierte Attribute zu unterstützen, und das betreffende Attribut nicht auf ein Feld angewendet werden kann, würde der Compiler einen Fehler ausgeben, anstatt eine Warnung zu erstellen, die den Build unterbricht.

## <a name="alternatives"></a>Alternativen
[alternatives]: #alternatives

## <a name="unresolved-questions"></a>Nicht aufgelöste Fragen
[unresolved]: #unresolved-questions

## <a name="design-meetings"></a>Treffen von Besprechungen
