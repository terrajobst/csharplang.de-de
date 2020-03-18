---
ms.openlocfilehash: b8d975a8fc95af6980feaae6be35160646fe2cd2
ms.sourcegitcommit: 32abf01f2e43be29114bfcf8f8ed1cc4e3eaded2
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 12/07/2019
ms.locfileid: "79484045"
---
# <a name="permit-stackalloc-in-nested-contexts"></a>`stackalloc` in einem Kontext in der Liste zulassen

### <a name="stack-allocation"></a>Stapel Zuordnung

Wir ändern den Abschnitt [*Stapel Zuordnung*](https://github.com/dotnet/csharplang/blob/master/spec/unsafe-code.md#stack-allocation) der C# Sprachspezifikation, um die Stellen zu lockern, wenn ein `stackalloc` Ausdruck möglicherweise angezeigt wird. Wir löschen

``` antlr
local_variable_initializer_unsafe
    : stackalloc_initializer
    ;

stackalloc_initializer
    : 'stackalloc' unmanaged_type '[' expression ']'
    ;
```

und ersetzen Sie Sie durch

``` antlr
primary_no_array_creation_expression
    : stackalloc_initializer
    ;

stackalloc_initializer
    : 'stackalloc' unmanaged_type '[' expression? ']' array_initializer?
    | 'stackalloc' '[' expression? ']' array_initializer
    ;
```

Beachten Sie, dass das Hinzufügen einer *array_initializer* zu *stackalloc_initializer* (und der Index Ausdruck optional) eine [Erweiterung in C# 7,3](https://github.com/dotnet/csharplang/blob/master/proposals/csharp-7.3/stackalloc-array-initializers.md) war und hier nicht beschrieben wird.

Der *Elementtyp* des `stackalloc` Ausdrucks ist die *unmanaged_type* mit dem Namen im stackzuzuordnen-Ausdruck, sofern vorhanden, oder der gemeinsame Typ unter den Elementen der *array_initializer* andernfalls.

Der Typ der *stackalloc_initializer* mit dem *Elementtyp* `K` hängt von seinem syntaktischen Kontext ab:
- Wenn die *stackalloc_initializer* direkt als *local_variable_initializer* einer *local_variable_declaration* -Anweisung oder einer *for_initializer*angezeigt wird, ist Ihr Typ `K*`.
- Andernfalls ist der Typ `System.Span<K>`.

### <a name="stackalloc-conversion"></a>Stackzuweisung-Konvertierung

Die *Stack-Zuweisung* ist eine neue integrierte implizite Konvertierung von Ausdruck. Wenn der Typ eines *stackalloc_initializer* `K*`ist, gibt es eine implizite *stackzugewiesene-Konvertierung* vom *stackalloc_initializer* in den Typ `System.Span<K>`.
