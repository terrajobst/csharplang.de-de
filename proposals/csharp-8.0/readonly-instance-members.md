---
ms.openlocfilehash: d0bb80305ccc755a555cf47a1d010fc4cb9a7bcd
ms.sourcegitcommit: 5688b13e66dd77b224a1223338de9e3b1f66d7f0
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/13/2020
ms.locfileid: "79484369"
---
# <a name="readonly-instance-members"></a>Schreibgeschützte Instanzmember

Propagierte Problem: <https://github.com/dotnet/csharplang/issues/1710>

## <a name="summary"></a>Zusammenfassung
[summary]: #summary

Geben Sie an, wie die einzelnen Instanzmember in einer Struktur nicht geändert werden sollen, und zwar auf dieselbe Weise, in der `readonly struct` die den Status "ändern" der Instanzelemente angibt.

Beachten Sie, dass `readonly instance member`! = `pure instance member`. Ein `pure` Instanzmember gewährleistet, dass kein Zustand geändert wird. Ein `readonly` Instanzmember garantiert nur, dass der Instanzstatus nicht geändert wird.

Alle Instanzmember in einer `readonly struct` können als implizit `readonly instance members`betrachtet werden. Explizite `readonly instance members`, die für nicht schreibgeschützte Strukturen deklariert sind, Verhalten sich auf die gleiche Weise. Beispielsweise würden Sie weiterhin verborgene Kopien erstellen, wenn Sie einen Instanzmember (auf der aktuellen Instanz oder in einem Feld der Instanz) aufgerufen haben, der selbst nicht schreibgeschützt war.

## <a name="motivation"></a>Motivation
[motivation]: #motivation

Heute haben Benutzer die Möglichkeit, `readonly struct` Typen zu erstellen, die der Compiler erzwingt, dass alle Felder schreibgeschützt sind (und durch Erweiterung, dass keine Instanzmember den Zustand ändern). Es gibt jedoch einige Szenarien, in denen Sie über eine vorhandene API verfügen, die barrierefreie Felder verfügbar macht oder eine Mischung aus veränderlichen und nicht veränderlichen Membern aufweist. Unter diesen Umständen können Sie den Typ nicht als `readonly` markieren (Breaking Change).

Dies hat normalerweise keine großen Auswirkungen, außer im Fall von `in`-Parametern. Mit `in`-Parametern für nicht schreibgeschützte Strukturen erstellt der Compiler eine Kopie des-Parameters für jeden Aufruf des Instanzmembers, da er nicht gewährleisten kann, dass der interne Zustand durch den Aufruf nicht geändert wird. Dies kann zu einer Vielzahl von Kopien und einer schlechteren Gesamtleistung führen, als wenn Sie die Struktur einfach direkt als Wert übermittelt hätten. Ein Beispiel finden Sie in diesem Code auf [sharplab](https://sharplab.io/#v2:CYLg1APgAgDABFAjAbgLACgNQMxwM4AuATgK4DGBcAagKYUD2RATBgN4ZycK4BmANvQCGlAB5p0XbnH5DKAT3GSOXHNIHC4AGRoA7AOYEAFgGUAjiUFEawZZ3YTJXPTQK3H9x54QB2OAAoROAAqOBEASjgwNy8YvzlguDkwxS8AXzd09EysXCgmOABhOA8VXnVKAFk/AEsdajoCRnyAN0E+EhoIks8oX1b2mgA6bX0jMwsrYEi4fo7h3QMTc0trFM5M1KA==) .

Einige andere Szenarien, in denen ausgeblendete Kopien auftreten können, sind `static readonly fields` und `literals`. Wenn Sie zu einem späteren Zeitpunkt unterstützt werden, werden `blittable constants` am gleichen Boot. Das heißt, dass alle zurzeit eine vollständige Kopie erfordern (bei einem Instanzmember-Aufruf), wenn die Struktur nicht als `readonly`gekennzeichnet ist.

## <a name="design"></a>Entwurf
[design]: #design

Ermöglicht es Benutzern, anzugeben, dass ein Instanzmember selbst `readonly` ist, und ändert nicht den Status der Instanz (natürlich mit der gesamten entsprechenden Überprüfung des Compilers). Beispiel:

```csharp
public struct Vector2
{
    public float x;
    public float y;

    public readonly float GetLengthReadonly()
    {
        return MathF.Sqrt(LengthSquared);
    }

    public float GetLength()
    {
        return MathF.Sqrt(LengthSquared);
    }

    public readonly float GetLengthIllegal()
    {
        var tmp = MathF.Sqrt(LengthSquared);

        x = tmp;    // Compiler error, cannot write x
        y = tmp;    // Compiler error, cannot write y

        return tmp;
    }

    public float LengthSquared
    {
        readonly get
        {
            return (x * x) +
                   (y * y);
        }
    }
}

public static class MyClass
{
    public static float ExistingBehavior(in Vector2 vector)
    {
        // This code causes a hidden copy, the compiler effectively emits:
        //    var tmpVector = vector;
        //    return tmpVector.GetLength();
        //
        // This is done because the compiler doesn't know that `GetLength()`
        // won't mutate `vector`.

        return vector.GetLength();
    }

    public static float ReadonlyBehavior(in Vector2 vector)
    {
        // This code is emitted exactly as listed. There are no hidden
        // copies as the `readonly` modifier indicates that the method
        // won't mutate `vector`.

        return vector.GetLengthReadonly();
    }
}
```

"Schreibgeschützt" kann auf Eigenschaftenaccessoren angewendet werden, um anzugeben, dass `this` im Accessor nicht mutiert werden. Die folgenden Beispiele verfügen über schreibgeschützte Setter, da diese Accessoren den Zustand des Element Felds ändern, aber nicht den Wert dieses Member-Felds ändern.

```csharp
public int Prop1
{
    readonly get
    {
        return this._store["Prop1"];
    }
    readonly set
    {
        this._store["Prop1"] = value;
    }
}
```

Wenn `readonly` auf die Eigenschaften Syntax angewendet wird, bedeutet dies, dass alle Accessoren `readonly`werden.

```csharp
public readonly int Prop2
{
    get
    {
        return this._store["Prop2"];
    }
    set
    {
        this._store["Prop2"] = value;
    }
}
```

"Schreibgeschützt" kann nur auf Accessoren angewendet werden, die den enthaltenden Typ nicht mutieren.

```csharp
public int Prop3
{
    readonly get
    {
        return this._prop3;
    }
    set
    {
        this._prop3 = value;
    }
}
```

"Schreibgeschützt" kann auf einige automatisch implementierte Eigenschaften angewendet werden, hat jedoch keinen sinnvollen Effekt. Der Compiler behandelt alle automatisch implementierten Getter als schreibgeschützt, unabhängig davon, ob das `readonly`-Schlüsselwort vorhanden ist.

```csharp
// Allowed
public readonly int Prop4 { get; }
public int Prop5 { readonly get; }
public int Prop6 { readonly get; set; }

// Not allowed
public readonly int Prop7 { get; set; }
public int Prop8 { get; readonly set; }
```

"Schreibgeschützt" kann auf manuell implementierte Ereignisse angewendet werden, aber nicht auf Feld ähnliche Ereignisse. "Schreibgeschützt" kann nicht auf einzelne Ereignisaccessoren (hinzufügen/entfernen) angewendet werden.

```csharp
// Allowed
public readonly event Action<EventArgs> Event1
{
    add { }
    remove { }
}

// Not allowed
public readonly event Action<EventArgs> Event2;
public event Action<EventArgs> Event3
{
    readonly add { }
    readonly remove { }
}
public static readonly event Event4
{
    add { }
    remove { }
}
```

Einige weitere Syntax Beispiele:

* Ausdruckskörpermember: `public readonly float ExpressionBodiedMember => (x * x) + (y * y);`
* Generische Einschränkungen: `public static readonly void GenericMethod<T>(T value) where T : struct { }`

Der Compiler gibt den Instanzmember wie üblich aus und gibt darüber hinaus ein vom Compiler erkanntes Attribut aus, das angibt, dass der Instanzmember den Zustand nicht ändert. Dies bewirkt, dass der ausgeblendete `this`-Parameter anstelle `ref T``in T` wird.

Dadurch kann der Benutzer die genannte Instanzmethode sicher aufzurufen, ohne dass der Compiler eine Kopie erstellen muss.

Die Einschränkungen umfassen Folgendes:

* Der `readonly` Modifizierer kann nicht auf statische Methoden, Konstruktoren oder Dekonstruktoren angewendet werden.
* Der `readonly` Modifizierer kann nicht auf Delegaten angewendet werden.
* Der `readonly` Modifizierer kann nicht auf Member der Klasse oder Schnittstelle angewendet werden.

## <a name="drawbacks"></a>Nachteile
[drawbacks]: #drawbacks

Die gleichen Nachteile wie bei den `readonly struct` Methoden sind heute vorhanden. Bestimmter Code verursacht möglicherweise weiterhin verborgene Kopien.

## <a name="notes"></a>Hinweise
[notes]: #notes

Die Verwendung eines Attributs oder eines anderen Schlüssel Worts ist möglicherweise ebenfalls möglich.

Dieser Vorschlag steht in gewisser Weise im Zusammenhang mit (aber eine Teilmenge) `functional purity` und/oder `constant expressions`, von denen beide bereits über einige Vorschläge verfügen.
