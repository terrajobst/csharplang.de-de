---
ms.openlocfilehash: 63dfdfee9ea6c16e162f483aa1298feed297daef
ms.sourcegitcommit: 94a3d151c438d34ede1d99de9eb4ebdc07ba4699
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/25/2019
ms.locfileid: "79483667"
---
# <a name="conditional-ref-expressions"></a>Bedingte Verweis Ausdrücke

Das Muster, mit dem eine Verweis Variable an einen oder einen anderen Ausdruck bedingt gebunden wird, ist C#in derzeit nicht ausdrucksfähig.

Die typische Problem Umgehung besteht darin, eine Methode wie die folgende einzuführen:

```csharp
ref T Choice(bool condition, ref T consequence, ref T alternative)
{
    if (condition)
    {
         return ref consequence;
    }
    else
    {
         return ref alternative;
    }
}
```

Beachten Sie, dass dies keine genaue Ersetzung eines ternären ist, da alle Argumente an der aufrufssite ausgewertet werden müssen.

Folgendes funktioniert nicht wie erwartet:

```csharp
       // will crash with NRE because 'arr[0]' will be executed unconditionally
      ref var r = ref Choice(arr != null, ref arr[0], ref otherArr[0]);
```

Die vorgeschlagene Syntax sieht wie folgt aus:

```csharp
     <condition> ? ref <consequence> : ref <alternative>;
```

Der obige Versuch mit "Choice" kann mithilfe von "Ref ternäre" _Ordnungs_ gemäß geschrieben werden:

```csharp
     ref var r = ref (arr != null ? ref arr[0]: ref otherArr[0]);
```

Der Unterschied besteht darin, dass die Folge und Alternative Ausdrücke _auf eine ordnungs_ gemäß bedingte Weise aufgerufen werden. Daher wird kein Absturz angezeigt, wenn ```arr == null```

Der ternäre Verweis ist nur ein ternärer Verweis, bei dem Alternative und Folge Refs sind. Dies erfordert natürlich, dass die Folge/Alternative Operanden Lvalues sind. Außerdem ist es erforderlich, dass diese Folge und Alternative über Typen verfügen, die in einander als Identitätswechsel konvertierbar sind.

Der Typ des Ausdrucks wird ähnlich wie der für die reguläre ternäre berechnet. Das heißt, in einem Fall, wenn eine Folge und eine Alternative Identitäts konvertierbar, aber unterschiedliche Typen aufweisen, gelten die vorhandenen Regeln zum Zusammenführen von Typen.

"Safe-to-return" wird von den bedingten Operanden konservativ angenommen. Wenn eine der beiden Optionen unsicher ist, ist die Rückgabe unsicher.

Ref ternäre ist ein Lvalue und kann daher als Verweis erfolgreich/zugewiesen/zurückgegeben werden.

```csharp
     // pass by reference
     foo(ref (arr != null ? ref arr[0]: ref otherArr[0]));

     // return by reference
     return ref (arr != null ? ref arr[0]: ref otherArr[0]);
```

Dabei kann es sich um einen lvalue-Wert handeln, der ebenfalls zugewiesen werden kann. 

```csharp
    // assign to
    (arr != null ? ref arr[0]: ref otherArr[0]) = 1;
```

Ref ternäre kann auch in einem regulären (nicht Verweis-) Kontext verwendet werden. Obwohl es nicht üblich wäre, sollten Sie nur ein reguläres Ternäres verwenden.

```csharp
     int x = (arr != null ? ref arr[0]: ref otherArr[0]);
```


___

Implementierungs Hinweise: 

Die Komplexität der Implementierung wäre anscheinend die Größe einer mittelgroßen zu großen Fehlerbehebung. -I. E ist nicht sehr aufwendig.
Ich denke nicht, dass wir Änderungen an der Syntax oder der Verarbeitung benötigen.
Es gibt keine Auswirkung auf Metadaten oder Interop. Die Funktion ist vollständig Ausdrucks basiert.
Keine Auswirkung auf Debuggen/PDB
