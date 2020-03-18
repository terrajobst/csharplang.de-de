---
ms.openlocfilehash: 76065293f652979ab395e131d657e44899c5a65b
ms.sourcegitcommit: 94a3d151c438d34ede1d99de9eb4ebdc07ba4699
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/25/2019
ms.locfileid: "79483541"
---
# <a name="simplified-null-argument-checking"></a>Vereinfachte NULL-Argument Überprüfung

## <a name="summary"></a>Zusammenfassung
Dieser Vorschlag stellt eine vereinfachte Syntax für das Validieren von Methoden Argumenten dar, die nicht `null` sind und `ArgumentNullException` entsprechend auslöst.

## <a name="motivation"></a>Motivation
Die Arbeit beim Entwerfen von Verweis Typen, die NULL-Werte zulassen, hat dazu geführt, dass wir den Code untersuchen, der für die `null` Da sich NRT nicht auf die Codeausführung auswirkt, müssen Entwickler weiterhin `if (arg is null) throw`-Kessel Bausteine hinzufügen, auch in Projekten, die vollständig `null` bereinigt sind. Dadurch haben wir die Absicht, eine minimale Syntax für die Argument `null` Validierung in der Sprache zu untersuchen. 

Obwohl diese `null` Parameter-Validierungs Syntax häufig mit NRT gekoppelt werden soll, ist der Vorschlag vollständig unabhängig davon. Die Syntax kann unabhängig von `#nullable` Direktiven verwendet werden.

## <a name="detailed-design"></a>Detaillierter Entwurf 

### <a name="null-validation-parameter-syntax"></a>Syntax der NULL-Validierungs Parameter
Der Bang-Operator (`!`) kann nach einem Parameternamen in einer Parameterliste positioniert werden. Dies bewirkt, C# dass der Compiler Standard `null` Überprüfungs Code für diesen Parameter ausgibt. Dies wird als `null` Validierungs Parameter Syntax bezeichnet. Beispiel:

``` csharp
void M(string name!) {
    ...
}
```

Wird übersetzt in:

``` csharp
void M(string name) {
    if (name is null) {
        throw new ArgumentNullException(nameof(name));
    }
    ...
}
```

Die generierte `null` Prüfung erfolgt vor einem von Entwicklern erstellten Code in der-Methode. Wenn mehrere Parameter den `!` Operator enthalten, werden die Überprüfungen in derselben Reihenfolge ausgeführt, in der die Parameter deklariert werden.

``` csharp
void M(string p1, string p2) {
    if (p1 is null) {
        throw new ArgumentNullException(nameof(p1));
    }
    if (p2 is null) {
        throw new ArgumentNullException(nameof(p2));
    }
    ...
}
```

Die Überprüfung bezieht sich speziell auf Verweis Gleichheit auf `null`, ruft keine `==` oder benutzerdefinierten Operatoren auf. Dies bedeutet auch, dass der `!` Operator nur zu Parametern hinzugefügt werden kann, deren Typ auf Gleichheit mit `null`getestet werden kann. Dies bedeutet, dass Sie nicht für einen Parameter verwendet werden kann, dessen Typ bekanntermaßen ein Werttyp ist.

``` csharp
// Error: Cannot use ! on parameters who types derive from System.ValueType
void G<T>(T arg!) where T : struct {

}
```

Im Fall eines Konstruktors erfolgt die `null` Validierung vor jedem anderen Code im Konstruktor. Dies umfasst Folgendes: 

- Verkettung von anderen Konstruktoren mit `this` oder `base` 
- Feldinitialisierer, die implizit im Konstruktor auftreten

Beispiel:

``` csharp
class C {
    string field = GetString();
    C(string name!): this(name) {
        ...
    }
}
```

Wird ungefähr in Folgendes übersetzt:

``` csharp
class C {
    C(string name)
        if (name is null) {
            throw new ArgumentNullException(nameof(name));
        }
        field = GetString();
        :this(name);
        ...
}
```

Hinweis: Dies ist kein gültiger C# Code, sondern lediglich eine Näherung der Implementierung der Implementierung. 

Die Syntax der `null` Validierungs Parameter ist auch in Lambda-Parameterlisten gültig. Dies ist auch in der einzelnen Parameter Syntax gültig, bei der keine Parameter fehlen.

``` csharp
void G() {
    // An identity lambda which throws on a null input
    Func<string, string> s = x! => x;
}
```

Die Syntax ist auch für Parameter für Iteratormethoden gültig. Im Gegensatz zu anderem Code im Iterator wird `null` Validierung ausgeführt, wenn die Iteratormethode aufgerufen wird, nicht, wenn der zugrunde liegende Enumerator durchlaufen wird. Dies gilt für herkömmliche oder `async` Iteratoren.

``` csharp
class Iterators {
    IEnumerable<char> GetCharacters(string s!) {
        foreach (var c in s) {
            yield return c;
        }
    }

    void Use() {
        // The invocation of GetCharacters will throw
        IEnumerable<char> e = GetCharacters(null);
    }
}
```

Der `!`-Operator kann nur für Parameterlisten verwendet werden, die über einen zugeordneten Methoden Text verfügen. Dies bedeutet, dass Sie nicht in einer `abstract` Methode, `interface``delegate` oder `partial` Methoden Definition verwendet werden kann.

### <a name="extending-is-null"></a>Erweiterung ist NULL
Die Typen, für die der Ausdruck `is null` gültig ist, werden so erweitert, dass Sie nicht eingeschränkte Typparameter einschließen. Dadurch wird es ermöglicht, die `null` für alle Typen zu überprüfen, die eine `null` Prüfung gültig ist. Dabei handelt es sich insbesondere um Typen, bei denen es sich nicht definitiv um Werttypen handelt. Beispielsweise können Typparameter, die auf `struct` beschränkt sind, nicht mit dieser Syntax verwendet werden.

``` csharp
void NullCheck<T1, T2>(T1 p1, T2 p2) where T2 : struct {
    // Okay: T1 could be a class or struct here.
    if (p1 is null) {
        ...
    }

    // Error 
    if (p2 is null) { 
        ...
    }
}
```

Das Verhalten von `is null` für einen Typparameter ist derselbe wie `== null` heute. In Fällen, in denen der Typparameter als Werttyp instanziiert wird, wird der Code als `false`ausgewertet. In Fällen, in denen es sich um einen Verweistyp handelt, führt der Code eine ordnungsgemäße `is null` Überprüfung durch.

### <a name="intersection-with-nullable-reference-types"></a>Schnittmenge mit Verweis Typen mit Nullwert
Jeder Parameter, der einen `!`-Operator hat, der auf seinen Namen angewendet wird, beginnt mit dem Zustand, der NULL-Werte zulässt, nicht `null`. Dies gilt auch, wenn der Typ des Parameters selbst potenziell `null`ist. Dies kann bei einem explizit auf NULL festleg baren Typ vorkommen, z. b. `string?`, oder mit einem uneingeschränkten Typparameter. 

Wenn eine `!` Syntax für Parameter mit einem explizit auf NULL festleg baren Typ für den Parameter kombiniert wird, wird vom Compiler eine Warnung ausgegeben:

``` csharp
void WarnCase<T>(
    string? name!, // Warning: combining explicit null checking with a nullable type
    T value1 // Okay
)
```

## <a name="open-issues"></a>Offene Probleme
Keine

## <a name="considerations"></a>Überlegungen

### <a name="constructors"></a>Konstruktoren
Die Codegenerierung für Konstruktoren bedeutet, dass es ein kleines, aber beobachtbares Behavior Change gibt, wenn Sie die Standard `null` Validierung heute und die Syntax für `null` Validierungs Parameter (`!`) verschieben. Die `null` Prüfung in der Standard Validierung erfolgt nach den feldinitialisierern und allen `base`-oder `this` aufrufen. Dies bedeutet, dass ein Entwickler nicht notwendigerweise 100% der `null` Validierung zur neuen Syntax migrieren kann. Für Konstruktoren ist mindestens eine Prüfung erforderlich.

Nach der Erörterung wurde beschlossen, dass es sehr unwahrscheinlich ist, dass dies zu erheblichen Akzeptanz Problemen führt. Es ist logischer, dass die `null` Prüfung ausgeführt wird, bevor eine Logik im Konstruktor funktioniert. Kann erneut auftreten, wenn signifikante kompatibleme Probleme erkannt werden.

### <a name="warning-when-mixing--and-"></a>Warnung beim Mischen? immer!
Es wurde ausführlich diskutiert, ob eine Warnung ausgegeben werden soll, wenn die `!`-Syntax auf einen Parameter angewendet wird, der explizit in einen Werte zulässt-Typ eingegeben wird. Auf der Oberfläche scheint es eine unsinnige Deklaration durch den Entwickler zu sein, aber es gibt Fälle, in denen Typhierarchien Entwicklern eine solche Situation erzwingen könnten. 

Beachten Sie die folgende Klassenhierarchie für eine Reihe von Assemblys (vorausgesetzt, alle werden mit aktivierter `null` Prüfung kompiliert):

``` csharp
// Assembly1
abstract class C1 {
    protected abstract void M(object o); 
}

// Assembly2
abstract class C2 : C1 {

}

// Assembly3
abstract class C3 : C2 { 
    protected override void M(object o!) {
        ...
    }
}
```

Hier hat der Autor von `C3` beschlossen, dem Parameter `o``null` Validierung hinzuzufügen. Dies ist vollständig mit der Verwendungsweise des Features in Einklang.

Stellen Sie sich jetzt vor, zu einem späteren Zeitpunkt beschließt der Autor von Assembly2, die folgende außer Kraft Setzung hinzuzufügen:

``` csharp
// Assembly2
abstract class C2 : C1 {
   protected override void M(object? o) { 
       ...
   }
}
```

Dies ist für Verweis Typen zulässig, die auf NULL festgelegt werden können, da es zulässig ist, den Vertrag für Eingabe Positionen flexibler zu gestalten. Die NRT-Funktion im allgemeinen ermöglicht angemessene Co/kontra Varianz in der NULL-Zulässigkeit von Parametern/Rückgabe. Die Sprache führt jedoch die Co/kontra Varianz Überprüfung basierend auf der spezifischsten außer Kraft Setzung und nicht mit der ursprünglichen Deklaration durch. Dies bedeutet, dass der Autor von "Assembly3" eine Warnung über den Typ der `o`, die nicht übereinstimmt, erhält und die Signatur wie folgt ändern muss, um dies auszuschließen: 

``` csharp
// Assembly3
abstract class C3 : C2 { 
    protected override void M(object? o!) {
        ...
    }
}
```

An diesem Punkt hat der Autor von Assembly3 einige Möglichkeiten:

- Sie können die Warnung über `object?` und `object` Konflikt annehmen/unterdrücken.
- Sie können die Warnung über `object?` und `!` Konflikt annehmen/unterdrücken.
- Sie können einfach die `null` Überprüfungs Prüfung entfernen (Lösch `!` und explizite Überprüfung ausführen).

Dabei handelt es sich um ein echtes Szenario, aber vorerst besteht die Idee darin, mit der Warnung fortzufahren. Wenn sich herausstellt, dass die Warnung häufiger als erwartet auftritt, können wir Sie später entfernen (das Gegenteil ist nicht der Fall).

### <a name="implicit-property-setter-arguments"></a>Implizite Eigenschaften Setter-Argumente
Das `value`-Argument eines-Parameters ist implizit und wird nicht in einer Parameterliste angezeigt. Dies bedeutet, dass es sich nicht um ein Ziel dieses Features handeln kann. Die Eigenschaften Setter-Syntax kann so erweitert werden, dass Sie eine Parameterliste enthält, um die Anwendung des `!` Operators zuzulassen. Das ist jedoch gegen die Idee dieses Features, `null` Validierung zu vereinfachen. Daher funktioniert das implizite `value` Argument nur mit diesem Feature.

## <a name="future-considerations"></a>Überlegungen für die Zukunft
Keine
