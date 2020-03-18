---
ms.openlocfilehash: 340a1dc5a2eac653458d7d29f74551e5fe28b6d5
ms.sourcegitcommit: 94a3d151c438d34ede1d99de9eb4ebdc07ba4699
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/25/2019
ms.locfileid: "79483547"
---
# <a name="operators-should-be-exposed-for-systemintptr-and-systemuintptr"></a>Operatoren sollten für `System.IntPtr` und `System.UIntPtr` verfügbar gemacht werden.

* [x] vorgeschlagen
* [] Prototyp: nicht gestartet
* [] Implementierung: nicht gestartet
* [] Spezifikation: nicht gestartet

## <a name="summary"></a>Zusammenfassung
[summary]: #summary

Die CLR unterstützt eine Reihe von Operatoren für die `System.IntPtr`-und `System.UIntPtr` Typen (`native int`). Diese Operatoren sind in `III.1.5` der Common Language Infrastructure Spezifikation (`ECMA-335`) zu sehen. Diese Operatoren werden von C#jedoch nicht unterstützt.

Die Sprachunterstützung sollte für den gesamten Satz von Operatoren bereitgestellt werden, die von `System.IntPtr` und `System.UIntPtr`unterstützt werden. Diese Operatoren lauten wie folgt: `Add`, `Divide`, `Multiply`, `Remainder`, `Subtract`, `Negate`, `Equals`, `Compare`, `And`, `Not`, `Or`, `XOr`, `ShiftLeft`, `ShiftRight`.

## <a name="motivation"></a>Motivation
[motivation]: #motivation

Heute können Benutzer problemlos Anwendungen für C# mehrere Plattformen schreiben, indem Sie verschiedene Tools und Frameworks verwenden, wie z. b.: `Xamarin`, `.NET Core`, `Mono`usw...

Beim Schreiben von Platt Form übergreifendem Code ist es häufig erforderlich, Interop-Code zu schreiben, der mit einer bestimmten Zielplattform auf eine bestimmte Weise interagiert. Dies kann das Schreiben von Grafik Code, das Aufrufen einer System-API oder das interagieren mit einer vorhandenen nativen Bibliothek einschließen.

Dieser Interop-Code muss häufig Handles, nicht verwalteten Speicher oder sogar plattformspezifische Ganzzahlen verarbeiten.

Die Common Language Runtime unterstützt dies durch Definieren einer Reihe von Operatoren, die für die primitiven Typen `native int` (`System.IntPtr`) und `native unsigned int` (`System.UIntPtr`) verwendet werden können.

C#hat diese Operatoren nie unterstützt, sodass Benutzer das Problem umgehen müssen. Dadurch wird die Codekomplexität häufig erhöht und die Code Wartbarkeit verringert.

Daher sollte die Sprache damit beginnen, diese Operatoren zu unterstützen, um die Sprache zu unterstützen, um diese Anforderungen besser zu unterstützen.

## <a name="detailed-design"></a>Detaillierter Entwurf
[design]: #detailed-design

Der vollständige Satz der unterstützten Operatoren wird in `III.1.5` der Common Language Infrastructure Spezifikation (`ECMA-335`) definiert. Die Spezifikation ist hier verfügbar: [https://www.ecma-international.org/publications/files/ECMA-ST/ECMA-335.pdf](https://www.ecma-international.org/publications/files/ECMA-ST/ECMA-335.pdf).

* Im folgenden finden Sie eine Zusammenfassung der Operatoren zur einfacheren Bereitstellung.
* Die nicht überprüfbaren Operatoren, die von der CLI-Spezifikation definiert werden, werden nicht aufgelistet und sind zurzeit nicht Teil dieses Vorschlags (auch wenn dies auch in Erwägung gezogen werden sollte).
* Das Bereitstellen eines Schlüssel Worts (z. b. `nint` und `nuint`) und das Bereitstellen einer Möglichkeit, Literale für `System.IntPtr` und `System.UIntPtr` (z. b. 0n) zu deklarieren, ist nicht Teil dieses Angebots (auch wenn dies auch in Erwägung gezogen werden sollte).

### <a name="unary-plus-operator"></a>Unärer Plus-Operator

```csharp
System.IntPtr operator +(System.IntPtr)
```

```csharp
System.UIntPtr operator +(System.UIntPtr)
```

### <a name="unary-minus-operator"></a>Unärer Minus-Operator

```csharp
System.IntPtr operator -(System.IntPtr)
```

### <a name="bitwise-complement-operator"></a>Bitweiser Komplement Operator

```csharp
System.IntPtr operator ~(System.IntPtr)
```

```csharp
System.UIntPtr operator ~(System.UIntPtr)
```

### <a name="cast-operators"></a>Umwandlungsoperatoren

```csharp
explicit operator sbyte(System.IntPtr)               // Truncate
explicit operator short(System.IntPtr)               // Truncate
explicit operator int(System.IntPtr)                 // Truncate
explicit operator long(System.IntPtr)                // Sign Extend

explicit operator byte(System.IntPtr)                // Truncate
explicit operator ushort(System.IntPtr)              // Truncate
explicit operator uint(System.IntPtr)                // Truncate
explicit operator ulong(System.IntPtr)               // Zero Extend

explicit operator System.IntPtr(int)                 // Sign Extend
explicit operator System.IntPtr(long)                // Truncate

explicit operator System.IntPtr(uint)                // Sign Extend
explicit operator System.IntPtr(ulong)               // Truncate

explicit operator System.IntPtr(System.IntPtr)
explicit operator System.IntPtr(System.UIntPtr)
```

```csharp
explicit operator sbyte(System.UIntPtr)               // Truncate
explicit operator short(System.UIntPtr)               // Truncate
explicit operator int(System.UIntPtr)                 // Truncate
explicit operator long(System.UIntPtr)                // Sign Extend

explicit operator byte(System.UIntPtr)                // Truncate
explicit operator ushort(System.UIntPtr)              // Truncate
explicit operator uint(System.UIntPtr)                // Truncate
explicit operator ulong(System.UIntPtr)               // Zero Extend

explicit operator System.UIntPtr(int)                 // Zero Extend
explicit operator System.UIntPtr(long)                // Truncate

explicit operator System.UIntPtr(uint)                // Zero Extend
explicit operator System.UIntPtr(ulong)               // Truncate

explicit operator System.UIntPtr(System.IntPtr)
explicit operator System.UIntPtr(System.UIntPtr)
```

### <a name="multiplication-operator"></a>Multiplikations Operator

```csharp
System.IntPtr operator *(int, System.IntPtr)
System.IntPtr operator *(System.IntPtr, int)
System.IntPtr operator *(System.IntPtr, System.IntPtr)
```

```csharp
System.UIntPtr operator *(uint, System.UIntPtr)
System.UIntPtr operator *(System.UIntPtr, uint)
System.UIntPtr operator *(System.UIntPtr, System.UIntPtr)
```

### <a name="division-operator"></a>Divisions Operator

```csharp
System.IntPtr operator /(int, System.IntPtr)
System.IntPtr operator /(System.IntPtr, int)
System.IntPtr operator /(System.IntPtr, System.IntPtr)
```

```csharp
System.UIntPtr operator /(uint, System.UIntPtr)
System.UIntPtr operator /(System.UIntPtr, uint)
System.UIntPtr operator /(System.UIntPtr, System.UIntPtr)
```

### <a name="remainder-operator"></a>Rest-Operator

```csharp
System.IntPtr operator %(int, System.IntPtr)
System.IntPtr operator %(System.IntPtr, int)
System.IntPtr operator %(System.IntPtr, System.IntPtr)
```

```csharp
System.UIntPtr operator %(uint, System.UIntPtr)
System.UIntPtr operator %(System.UIntPtr, uint)
System.UIntPtr operator %(System.UIntPtr, System.UIntPtr)
```

### <a name="addition-operator"></a>Additions Operator

```csharp
System.IntPtr operator +(int, System.IntPtr)
System.IntPtr operator +(System.IntPtr, int)
System.IntPtr operator +(System.IntPtr, System.IntPtr)
```

```csharp
System.UIntPtr operator +(uint, System.UIntPtr)
System.UIntPtr operator +(System.UIntPtr, uint)
System.UIntPtr operator +(System.UIntPtr, System.UIntPtr)
```

### <a name="subtraction-operator"></a>Subtraktions Operator

```csharp
System.IntPtr operator -(int, System.IntPtr)
System.IntPtr operator -(System.IntPtr, int)
System.IntPtr operator -(System.IntPtr, System.IntPtr)
```

```csharp
System.UIntPtr operator -(uint, System.UIntPtr)
System.UIntPtr operator -(System.UIntPtr, uint)
System.UIntPtr operator -(System.UIntPtr, System.UIntPtr)
```

### <a name="shift-operators"></a>Schiebeoperatoren

```csharp
System.IntPtr operator <<(System.IntPtr, int)
System.IntPtr operator >>(System.IntPtr, int)
```

```csharp
System.UIntPtr operator <<(System.UIntPtr, int)
System.UIntPtr operator >>(System.UIntPtr, int)
```

### <a name="integer-comparison-operators"></a>Integer-Vergleichs Operatoren

```csharp
bool operator ==(int, System.IntPtr)
bool operator ==(System.IntPtr, int)
bool operator ==(System.IntPtr, System.IntPtr)

bool operator !=(int, System.IntPtr)
bool operator !=(System.IntPtr, int)
bool operator !=(System.IntPtr, System.IntPtr)

bool operator  <(int, System.IntPtr)
bool operator  <(System.IntPtr, int)
bool operator  <(System.IntPtr, System.IntPtr)

bool operator  >(int, System.IntPtr)
bool operator  >(System.IntPtr, int)
bool operator  >(System.IntPtr, System.IntPtr)

bool operator <=(int, System.IntPtr)
bool operator <=(System.IntPtr, int)
bool operator <=(System.IntPtr, System.IntPtr)

bool operator >=(int, System.IntPtr)
bool operator >=(System.IntPtr, int)
bool operator >=(System.IntPtr, System.IntPtr)
```

```csharp
bool operator ==(uint, System.UIntPtr)
bool operator ==(System.UIntPtr, uint)
bool operator ==(System.UIntPtr, System.UIntPtr)

bool operator !=(uint, System.UIntPtr)
bool operator !=(System.UIntPtr, uint)
bool operator !=(System.UIntPtr, System.UIntPtr)

bool operator  <(uint, System.UIntPtr)
bool operator  <(System.UIntPtr, uint)
bool operator  <(System.UIntPtr, System.UIntPtr)

bool operator  >(uint, System.UIntPtr)
bool operator  >(System.UIntPtr, uint)
bool operator  >(System.UIntPtr, System.UIntPtr)

bool operator <=(uint, System.UIntPtr)
bool operator <=(System.UIntPtr, uint)
bool operator <=(System.UIntPtr, System.UIntPtr)

bool operator >=(uint, System.UIntPtr)
bool operator >=(System.UIntPtr, uint)
bool operator >=(System.UIntPtr, System.UIntPtr)
```

### <a name="integer-logical-operators"></a>Ganz Zahl logische Operatoren

```csharp
System.IntPtr operator &(int, System.IntPtr)
System.IntPtr operator &(System.IntPtr, int)
System.IntPtr operator &(System.IntPtr, System.IntPtr)

System.IntPtr operator |(int, System.IntPtr)
System.IntPtr operator |(System.IntPtr, int)
System.IntPtr operator |(System.IntPtr, System.IntPtr)

System.IntPtr operator ^(int, System.IntPtr)
System.IntPtr operator ^(System.IntPtr, int)
System.IntPtr operator ^(System.IntPtr, System.IntPtr)
```

```csharp
System.UIntPtr operator &(uint, System.UIntPtr)
System.UIntPtr operator &(System.UIntPtr, uint)
System.UIntPtr operator &(System.UIntPtr, System.UIntPtr)

System.UIntPtr operator |(uint, System.UIntPtr)
System.UIntPtr operator |(System.UIntPtr, uint)
System.UIntPtr operator |(System.UIntPtr, System.UIntPtr)

System.UIntPtr operator ^(uint, System.UIntPtr)
System.UIntPtr operator ^(System.UIntPtr, uint)
System.UIntPtr operator ^(System.UIntPtr, System.UIntPtr)
```

## <a name="drawbacks"></a>Nachteile
[drawbacks]: #drawbacks

Die tatsächliche Verwendung dieser Operatoren ist möglicherweise klein und auf Endbenutzer beschränkt, die Bibliotheken auf niedrigerer Ebene oder Interop-Code schreiben. Die meisten Endbenutzer nutzen wahrscheinlich diese Bibliotheken der untersten Ebene selbst, die die nativen Ganzzahlen, Handles und Interop-Code abstrahieren würden. Daher müssten Sie die Operatoren selbst nicht benötigen.

## <a name="alternatives"></a>Alternativen
[alternatives]: #alternatives

Lassen Sie das Framework die erforderlichen Operatoren implementieren, indem Sie Sie direkt in Il schreiben. Darüber hinaus kann die Laufzeit eine intrinsische Unterstützung für die vom Framework definierten Operatoren bereitstellen, um die Leistung zu optimieren.

## <a name="unresolved-questions"></a>Nicht aufgelöste Fragen
[unresolved]: #unresolved-questions

Welche Teile des Entwurfs sind noch TBD?

## <a name="design-meetings"></a>Treffen von Besprechungen

Verknüpfen Sie die Entwurfs Hinweise, die sich auf diesen Vorschlag auswirken, und beschreiben Sie in einem Satz für jede Änderungen, zu denen Sie geführt haben.