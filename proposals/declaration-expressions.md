---
ms.openlocfilehash: d2064ec1637e50962262c9380281abd5e1711402
ms.sourcegitcommit: 94a3d151c438d34ede1d99de9eb4ebdc07ba4699
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/25/2019
ms.locfileid: "79483565"
---
# <a name="declaration-expressions"></a>Deklarations Ausdrücke

Unterstützen von Deklarations Zuweisungen als Ausdrücke.

## <a name="motivation"></a>Motivation
[motivation]: #motivation

Ermöglicht die Initialisierung zum Zeitpunkt der Deklaration in mehreren Fällen, vereinfacht den Code und ermöglicht die Verwendung von `var`.

```csharp
SpecialType ReferenceType =>
    (var st = _type.SpecialType).IsValueType() ? SpecialType.None : st;
```

Lässt Deklarationen für `ref` Argumente zu, ähnlich wie `out var`.

```csharp
Convert(source, destination, ref List<Diagnostic> diagnostics = null);
```

## <a name="detailed-design"></a>Detaillierter Entwurf
[design]: #detailed-design

Ausdrücke werden so erweitert, dass Sie Deklarations Zuweisung einschließen. Die Rangfolge entspricht der Zuweisung.

```antlr
expression
    : non_assignment_expression
    | assignment
    | declaration_assignment_expression // new
    ;
declaration_assignment_expression // new
    : declaration_expression '=' local_variable_initializer
    ;
declaration_expression // C# 7.0
    | type variable_designation
    ;
```

Die Deklarations Zuweisung ist eine einzelne lokale.

Der Typ eines Deklarations Zuweisungs Ausdrucks ist der Typ der Deklaration.
Wenn der Typ `var`ist, ist der abherleitet Typ der Typ des Initialisierungs Ausdrucks. 

Der Deklarations Zuweisungs Ausdruck kann ein l-Wert sein, der insbesondere für `ref` Argument Werte gilt.

Wenn der Deklarations Zuweisungs Ausdruck einen Werttyp deklariert und der Ausdruck ein r-Wert ist, ist der Wert des Ausdrucks eine Kopie.

Der Deklarations Zuweisungs Ausdruck kann eine `ref` lokalen deklarieren.
Es besteht eine Mehrdeutigkeit, wenn `ref` für einen Deklarations Ausdruck in einem `ref`-Argument verwendet wird.
Der lokale Variableninitialisierer bestimmt, ob die Deklaration eine `ref` Local ist.

```csharp
F(ref int x = IntFunc());    // int x;
F(ref int y = RefIntFunc()); // ref int y;
```

Der Bereich der in Deklarations Zuweisungs Ausdrücken deklarierten lokalen Variablen entspricht dem Bereich der entsprechenden Deklarations Ausdrücke aus C # 7.0.

Es handelt sich um einen Kompilierzeitfehler, der auf eine lokale in Text vor dem Deklarations Ausdruck verweist.

## <a name="alternatives"></a>Alternativen
[alternatives]: #alternatives
Keine Änderung. Diese Funktion ist einfach syntaktische Kurzweile.

Allgemeinere Sequenz Ausdrücke: siehe [#377](https://github.com/dotnet/csharplang/issues/377).

Um die Verwendung von `var` in mehreren Fällen zuzulassen, lassen Sie eine separate Deklaration und Zuweisung `var` lokalen Variablen zu, und leiten Sie den Typ von Zuweisungen von allen Codepfade ab.

## <a name="see-also"></a>Siehe auch
[see-also]: #see-also
Siehe grundlegender Deklarations Ausdruck in [#595](https://github.com/dotnet/csharplang/issues/595).

Weitere Informationen finden Sie unter Dekonstruktions Deklaration im [Dekonstruktions](https://github.com/dotnet/roslyn/blob/master/docs/features/deconstruction.md) Feature.
