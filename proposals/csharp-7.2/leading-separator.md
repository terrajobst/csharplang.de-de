---
ms.openlocfilehash: 6ec94aaabb2c52393a87ee450dbae972b6a50bd5
ms.sourcegitcommit: 94a3d151c438d34ede1d99de9eb4ebdc07ba4699
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/25/2019
ms.locfileid: "79483679"
---
# <a name="allow-digit-separator-after-0b-or-0x"></a><span data-ttu-id="44144-101">Ziffern Trennzeichen nach 0B oder 0x zulassen</span><span class="sxs-lookup"><span data-stu-id="44144-101">Allow digit separator after 0b or 0x</span></span>

<span data-ttu-id="44144-102">In C# 7,2 erweitern wir den Satz von stellen, die Ziffern Trennzeichen (Unterstrich Zeichen) in ganzzahligen literalen enthalten können.</span><span class="sxs-lookup"><span data-stu-id="44144-102">In C# 7.2, we extend the set of places that digit separators (the underscore character) can appear in integral literals.</span></span> <span data-ttu-id="44144-103">[Ab C# 7,0 sind Trennzeichen zwischen den Ziffern eines Literals zulässig](../csharp-7.0/digit-separators.md).</span><span class="sxs-lookup"><span data-stu-id="44144-103">[Beginning in C# 7.0, separators are permitted between the digits of a literal](../csharp-7.0/digit-separators.md).</span></span> <span data-ttu-id="44144-104">Nun können wir C# in 7,2 auch Ziffern Trennzeichen vor der ersten signifikanten Ziffer eines binären oder hexadezimalen Literals nach dem Präfix zulassen.</span><span class="sxs-lookup"><span data-stu-id="44144-104">Now, in C# 7.2, we also permit digit separators before the first significant digit of a binary or hexadecimal literal, after the prefix.</span></span>

```csharp
    123      // permitted in C# 1.0 and later
    1_2_3    // permitted in C# 7.0 and later
    0x1_2_3  // permitted in C# 7.0 and later
    0b101    // binary literals added in C# 7.0
    0b1_0_1  // permitted in C# 7.0 and later

    // in C# 7.2, _ is permitted after the `0x` or `0b`
    0x_1_2   // permitted in C# 7.2 and later
    0b_1_0_1 // permitted in C# 7.2 and later
```

<span data-ttu-id="44144-105">Es ist nicht zulässig, dass ein dezimales Ganzzahlliteral einen führenden Unterstrich hat.</span><span class="sxs-lookup"><span data-stu-id="44144-105">We do not permit a decimal integer literal to have a leading underscore.</span></span> <span data-ttu-id="44144-106">Ein Token wie `_123` ist ein Bezeichner.</span><span class="sxs-lookup"><span data-stu-id="44144-106">A token such as `_123` is an identifier.</span></span>
