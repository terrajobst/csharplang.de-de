---
ms.openlocfilehash: 0c8bc2b5072ea7f86189b41a1cdbf2a449661b05
ms.sourcegitcommit: 33a60a1db1d42d21d959acfeb127e647150173aa
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/18/2020
ms.locfileid: "79509492"
---
# <a name="attributes-on-local-functions"></a>Attribute für lokale Funktionen

## <a name="attributes"></a>Attribute

Lokale Funktions Deklarationen dürfen jetzt über [Attribute](../spec/attributes.md)verfügen. Parameter und Typparameter für lokale Funktionen dürfen auch über Attribute verfügen.

Attribute mit einer bestimmten Bedeutung, wenn Sie auf eine Methode, deren Parameter oder die Typparameter angewendet werden, haben die gleiche Bedeutung, wenn Sie auf eine lokale Funktion, ihre Parameter oder die zugehörigen Typparameter angewendet werden.

Eine lokale Funktion kann in demselben Sinne wie eine [bedingte Methode](../spec/attributes.md#the-conditional-attribute) bedingt erstellt werden, indem Sie mit einem `[ConditionalAttribute]`versehen wird. Eine bedingte lokale Funktion muss ebenfalls `static`werden. Alle Einschränkungen für bedingte Methoden gelten auch für bedingte lokale Funktionen, einschließlich, dass der Rückgabetyp `void`werden muss.

## <a name="extern"></a>Extern

Der `extern` Modifizierer ist nun für lokale Funktionen zulässig. Dadurch wird die lokale Funktion extern im gleichen Sinne wie eine [externe Methode](../spec/classes.md#external-methods).

Ähnlich wie bei einer externen Methode muss der *lokale Funktions Text* einer externen lokalen Funktion ein Semikolon sein. Ein Semikolon " *local-Function-Body* " ist nur für eine externe lokale Funktion zulässig. 

Eine externe lokale Funktion muss ebenfalls `static`werden.

## <a name="syntax"></a>Syntax

Die [Grammatik der lokalen Funktionen](csharp-7.0/local-functions.md#syntax-grammar) wird wie folgt geändert:
```
local-function-header
    : attributes? local-function-modifiers? return-type identifier type-parameter-list?
        ( formal-parameter-list? ) type-parameter-constraints-clauses
    ;

local-function-modifiers
    : (async | unsafe | static | extern)*
    ;

local-function-body
    : block
    | arrow-expression-body
    | ';'
    ;
```
