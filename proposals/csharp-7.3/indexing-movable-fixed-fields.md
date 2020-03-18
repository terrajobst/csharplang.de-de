---
ms.openlocfilehash: 36f3e6204d12c2569b3a55f3a47f58337e8a08e4
ms.sourcegitcommit: 94a3d151c438d34ede1d99de9eb4ebdc07ba4699
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/25/2019
ms.locfileid: "79483607"
---
# <a name="indexing-fixed-fields-should-not-require-pinning-regardless-of-the-movableunmovable-context"></a>Die Indizierung `fixed` Felder sollte nicht unabhängig vom verschiebbaren/nicht verschiebbaren Kontext fixiert werden. #

Die Änderung hat die Größe einer Fehlerbehebung. Sie kann sich in 7,3 befinden und steht nicht in Konflikt mit der von uns weiterführenden Richtung.
Diese Änderung besteht lediglich darin, dass das folgende Szenario funktioniert, auch wenn `s` in der Lage ist, dies zu ermöglichen. Sie ist bereits gültig, wenn `s` nicht verschoben werden kann. 

Hinweis: in beiden Fällen ist weiterhin `unsafe` Kontext erforderlich. Es ist möglich, nicht initialisierte Daten zu lesen oder sogar außerhalb des gültigen Bereichs. Dies ändert sich nicht.

```csharp
unsafe struct S
{
    public fixed int myFixedField[10];
}

class Program
{
    static S s;

    unsafe static void Main()
    {
        int p = s.myFixedField[5]; // indexing fixed-size array fields would be ok
    }
}
```

Die wichtigste "Herausforderung", die hier angezeigt wird, ist die Erläuterung der Entspannung in der Spezifikation. Dies ist insbesondere erforderlich, da das anhepup weiterhin erforderlich ist. (da `s` in der Lage ist, das Feld explizit als Zeiger zu verwenden)

```csharp
unsafe struct S
{
    public fixed int myFixedField[10];
}

class Program
{
    static S s;

    unsafe static void Main()
    {
        int* ptr = s.myFixedField; // taking a pointer explicitly still requires pinning.
        int p = ptr[5];
    }
}
```

Ein Grund für das Anheften des Ziels, wenn es verschoben werden muss, ist das Artefakt unserer Code Generierungs Strategie,-wir konvertieren immer in einen nicht verwalteten Zeiger und erzwingen somit, dass der Benutzer die PIN über `fixed` Anweisung anheftet. Die Konvertierung in nicht verwaltete ist jedoch bei der Indizierung nicht erforderlich. Die gleiche unsichere Zeiger Mathematik ist gleichermaßen anwendbar, wenn der Empfänger in Form eines verwalteten Zeigers vorhanden ist. Wenn dies der Fall ist, wird der zwischen Verweis verwaltet (GC-nachverfolgt), und das anheunen ist nicht erforderlich.

Der Änderungs https://github.com/dotnet/roslyn/pull/24966 ist ein Prototyp-PR, der diese Anforderung lockert.
