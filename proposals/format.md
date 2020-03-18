---
ms.openlocfilehash: 1457c1eb018e12af30ce5b38be704bf8851d4b25
ms.sourcegitcommit: 7f7fc6e9e195e51b7ff8229aeaa70aa9fbbb63cb
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 09/10/2019
ms.locfileid: "79483937"
---
# <a name="efficient-params-and-string-formatting"></a>Effiziente Parameter und Zeichen folgen Formatierung

## <a name="summary"></a>Zusammenfassung
Diese Kombination von Features erhöht die Effizienz der Formatierung `string` Werte und übergibt `params` Stil Argumente.

## <a name="motivation"></a>Motivation
Der Zuordnungs Aufwand für das Formatieren `string` Werte kann die Leistung vieler textbasierter Anwendungen dominieren: von der Boxing-Einbuße `struct` Typen, der `object[]` Zuordnung für `params` und der zwischen `string` Zuordnungen während `string.Format` aufrufen. Um die Effizienz aufrechtzuerhalten, müssen solche Anwendungen häufig Produktivitäts Features wie `params` und `string` Interpolations-und Wechsel zu nicht standardmäßigen Hand codierten Lösungen verwerfen. 

Nehmen Sie als Beispiel MSBuild an. Dies wird mit vielen modernen C# Features von Entwicklern geschrieben, die die Leistung bewusst sind. In einem Beispiel für einen repräsentativen Build generiert MSBuild jedoch mit minimaler Ausführlichkeit eine `string` Zuordnung mit einer Größe von 262mb. Von dieser 1/2 der Zuordnungen sind kurzlebige Zuordnungen innerhalb `string.Format`. Diese Features würden einen Großteil davon auf .net-Desktop entfernen und in .net Core auf fast null senken, da die Verfügbarkeit von `Span<T>`

Die hier beschriebenen sprach Features ermöglichen es Anwendungen, diese Features weiterhin zu verwenden, und zwar mit nur wenigen oder gar keinen Änderungen an ihrer Anwendungs Codebasis, während gleichzeitig der unbeabsichtigte Zuweisungs Aufwand in den meisten Fällen entfernt wird.

## <a name="detailed-design"></a>Detaillierter Entwurf 
Es gibt eine Reihe von Features, die hier verwendet werden, um diese Ergebnisse zu erzielen:

- Erweitern von `params`, um einen umfassenderen Satz von Sammlungs Typen zu unterstützen.
- Ermöglicht Entwicklern das Anpassen der Art `string` Interpolationen. 
- Ermöglicht das Binden von interpoliert `string` an effizientere `string.Format` Überladungen.

### <a name="extending-params"></a>Erweitern von Parametern
Die Sprache ermöglicht, dass `params` in einer Methoden Signatur die Typen `Span<T>`, `ReadOnlySpan<T>` und `IEnumerable<T>`aufweisen kann. Die gleichen Regeln für den Aufruf gelten für die folgenden neuen Typen, die für `params T[]`gelten:

- Kann nicht überladen, da der einzige Unterschied ein `params`-Schlüsselwort ist
- Kann aufrufen, indem eine Reihe von Argumenten übergeben wird, die implizit in `T` oder eine einzelne `Span<T>` / 
`ReadOnlySpan<T>` / `IEnumerable<T>` Argument konvertiert werden.
- Muss der letzte Parameter in einer Methoden Signatur sein.
- Usw.... 

Die `Span<T>`-und `ReadOnlySpan<T>` Varianten werden aus Gründen der Einfachheit als `Span<T>` unten bezeichnet. In Fällen, in denen sich das Verhalten von `ReadOnlySpan<T>` unterscheidet, wird es explizit als bezeichnet. 

Der Vorteil der `Span<T>` Varianten von `params` ist, dass der Compiler großartige Flexibilität bei der Zuordnung des Sicherungs Speichers für den `Span<T>` Wert bietet. Mit einem-`params T[]` muss der Compiler für jeden Aufruf einer `params`-Methode eine neue `T[]` zuordnen. Die Wiederverwendung ist nicht möglich, da Sie davon ausgehen muss, dass der aufgerufene den-Parameter speichert und wieder verwendet. Dies kann zu einer großen Ineffizienz bei Methoden mit vielen `params` aufrufen führen.

Angegebene `Span<T>` Varianten sind `ref struct` der aufgerufene das Argument nicht speichern kann. Daher kann der Compiler die CallSites optimieren, indem er Aktionen wie das erneute Verwenden des Arguments vornimmt. Dies kann dazu führen, dass wiederholte Aufrufe im Vergleich zu `T[]`sehr effizient sind. In der Sprache werden jedoch keine besonderen Garantien hinsichtlich der Optimierung solcher Aufruf Sites gegeben. Beachten Sie, dass der Compiler beim Aufrufen einer `params Span<T>` Methode andere Werte als `T[]` verwenden kann. 

Eine mögliche Implementierung ist die folgende: Beachten Sie, dass alle `params` Aufrufe in einem Methoden Text berücksichtigt werden. Der Compiler kann ein Array zuordnen, das eine Größe gleich dem größten `params` Aufruf hat, und dieses für alle Aufrufe verwenden, indem es `Span<T>` Instanzen mit entsprechender Größe über das Array erstellt. Beispiel:

``` csharp
static class OneAllocation {
    static void Use(params Span<string> spans) {
        ...
    }

    static void Go() {
        Use("jaredpar");
        Use("hello", "world");
        Use("a", "longer", "set");
    }
}
```

Der Compiler könnte den Text der `Go` wie folgt ausgeben:

``` csharp
    static void Go() {
        var args = new string[3];
        args[0] = "jaredpar";
        Use(new Span<string>(args, start: 0, length: 1));

        args[0] = "hello";
        args[1] = "world";
        Use(new Span<string>(args, start: 0, length: 2));

        args[0] = "a";
        args[1] = "longer";
        args[2] = "set";
        Use(new Span<string>(args, start: 0, length: 3));
   }
```

Dadurch kann die Anzahl der in einer Anwendung zugewiesenen Arrays erheblich reduziert werden. Zuweisungen können sogar noch weiter reduziert werden, wenn die Laufzeit Hilfsprogramme für eine intelligentere Stapel Zuordnung von Arrays bereitstellt.

Diese Optimierung kann jedoch nicht immer angewendet werden. Obwohl der aufgerufene das `params` Argument nicht erfassen kann, kann es dennoch im Aufrufer aufgezeichnet werden, wenn ein `ref` oder ein `out / ref` Parameter vorhanden ist, der selbst ein `ref struct`-Typ ist. 

``` csharp
static class SneakyCapture {
    static ref int M(params Span<T> span) => ref span[0];

    static void Oops() {
        // This now holds onto the memory backing the Span<T> 
        ref int r = ref M(42);
    }
}
```

Diese Fälle sind jedoch statisch erkennbar. Sie tritt möglicherweise auf, wenn eine `ref` Rückgabe oder ein `ref struct` Parameter von `out` oder `ref`übergeben wird. In einem solchen Fall muss der Compiler für jeden Aufruf eine neue `T[]` zuordnen. 

Am Ende dieses Dokuments werden einige andere mögliche Optimierungsstrategien erläutert.

Der `IEnumerable<T>` Variant ist eine einfache Überladung. Dies ist in Szenarios nützlich, in denen häufig `IEnumerable<T>` verwendet werden, aber auch viele `params` Verwendung. Wenn Sie in `T` Argument Form aufgerufen wird, wird der Sicherungs Speicher als `T[]` wie `params T[]` zugeordnet.

### <a name="params-overload-resolution-changes"></a>Änderungen der Überladungs Auflösung von Parametern
Dieses Angebot bedeutet, dass die Sprache nun vier Varianten von `params` enthält, in denen Sie sich vor einem solchen Dienst befand. Es ist sinnvoll, dass Methoden über Ladungen von Methoden definieren, die sich nur vom Typ einer `params` Deklarationen unterscheiden. 

Beachten Sie, dass `StringBuilder.AppendFormat` zusätzlich zum `params object[]`eine `params ReadOnlySpan<object>` Überladung hinzufügen würde. Dadurch kann die Leistung erheblich verbessert werden, indem Sammlungs Zuordnungen reduziert werden, ohne dass Änderungen am aufrufenden Code erforderlich sind. 

Um dies zu vereinfachen, führt die Sprache die folgende Regel zur Trennung der Überladungs Auflösung ein. Wenn sich die Kandidaten Methoden nur durch den `params`-Parameter unterscheiden, werden die Kandidaten in der folgenden Reihenfolge bevorzugt:

1. `ReadOnlySpan<T>`
1. `Span<T>`
1. `T[]`
1. `IEnumerable<T>`

Diese Reihenfolge ist der am wenigsten effiziente für den allgemeinen Fall.

### <a name="variant"></a>Variant
Corefx ist ein Prototyp eines neuen verwalteten Typs namens [Variant](https://github.com/dotnet/corefxlab/pull/2595). Dieser Typ ist für die Verwendung in APIs vorgesehen, die heterogene Werte erwarten, aber nicht den mehr Aufwand nutzen möchten, indem `object` als Parameter verwendet wird. Der `Variant` Typ stellt universellen Speicher bereit, vermeidet aber die Boxing-Zuordnung für die am häufigsten verwendeten Typen. Durch die Verwendung dieses Typs in APIs wie `string.Format` kann der boxingaufwand in den meisten Fällen vermieden werden.

Dieser Typ selbst ist nicht notwendigerweise speziell für die Sprache. Sie wird in diesem Dokument separat in dieses Dokument eingefügt, da es zu einem Implementierungsdetail anderer Teile des Angebots wird. 

### <a name="efficient-interpolated-strings"></a>Effiziente interinterpolierte Zeichen folgen
Interinterpolierte Zeichen folgen sind eine beliebte, aber C#ineffiziente Funktion in. Die häufigste Syntax, die eine interinterpolierte `string` als `string`verwendet, wird in einen `string.Format(string, params object[])`-Befehl übersetzt. Dies verursacht boxingzuordnungen für alle Werttypen, zwischen `string` Zuordnungen, da die Implementierung größtenteils `object.ToString` für die Formatierung und die Zuordnung von Arrays verwendet, wenn die Anzahl der Argumente die Anzahl der Parameter für die "schnellen" über Ladungen von `string.Format`überschreitet. 

Die Sprache ändert die Interpolations Herabsetzung, um alternative über Ladungen von `string.Format`in Erwägung zu gezogen. Dabei werden alle Formen von `string.Format(string, params)` berücksichtigt und die "beste" Überladung ausgewählt, die den Argument Typen entspricht.
Die "beste" `params` Überladung wird durch die oben beschriebenen Regeln bestimmt. Dies bedeutet, dass interpoliert `string` jetzt an sehr effiziente über Ladungen wie `string.Format(string format, params ReadOnlySpan<Variant> args)`gebunden werden kann. In vielen Fällen werden dadurch alle zwischen Zuordnungen entfernt.

### <a name="customizable-interpolated-strings"></a>Anpassbare interinterpolierte Zeichen folgen
Entwickler können das Verhalten interpoliert Zeichen folgen mit `FormattableString`anpassen. Diese Datei enthält die Daten, die in eine interinterpolierte Zeichenfolge fließen: das Format `string` und die Argumente als Array. Dies hat jedoch immer noch die Zuordnung von Boxing-und Argument Arrays sowie die Zuordnung für `FormattableString` (`abstract class`). Daher ist es von geringem Verwendungs Aufwand für Anwendungen, die die `string` Formatierung stark belegen.

Um das Formatieren von interpoliert Zeichen folgen effizient zu gestalten, erkennt die Sprache einen neuen Typ: `System.ValueFormattableString`. Alle interpoliert-Zeichen folgen weisen eine Zieltyp Konvertierung in diesen Typ auf. Dies wird implementiert, indem die interinterpolierte Zeichenfolge in den-Rückruf übersetzt wird `ValueFormattableString.Create` genau wie für `FormattableString.Create` heute. Die Sprache unterstützt alle `params` Optionen, die in diesem Dokument beschrieben werden, wenn Sie nach der am besten geeigneten `ValueFormattableString.Create` Methode suchen. 

``` csharp
readonly struct ValueFormattableString {
    public static ValueFormattableString Create(Variant v) { ... } 
    public static ValueFormattableString Create(string s) { ... } 
    public static ValueFormattableString Create(string s, params ReadOnlySpan<Variant> collection) { ... } 
}

class ConsoleEx { 
    static void Write(ValueFormattableString f) { ... }
}

class Program { 
    static void Main() { 
        ConsoleEx.Write(42);
        ConsoleEx.Write($"hello {DateTime.UtcNow}");

        // Translates into 
        ConsoleEx.Write(ValueFormattableString.Create((Variant)42));
        ConsoleEx.Write(ValueFormattableString.Create(
            "hello {0}", 
            new Variant(DateTime.UtcNow));
    }
}
```

Regeln zur Überladungs Auflösung werden geändert, um `ValueFormattableString` über `string` vorzuziehen, wenn das Argument eine interpoliert Zeichenfolge ist. Dies bedeutet, dass es für über Ladungen nützlich sein wird, die sich nur bei `string` und `ValueFormattableString`unterscheiden. Eine solche Überladung ist heute mit `FormattableString` nicht wertvoll, da der Compiler immer die `string` Version bevorzugen wird (es sei denn, der Entwickler verwendet eine explizite Umwandlung). 

## <a name="open-issues"></a>Offene Probleme

### <a name="valueformattablestring-breaking-change"></a>Valueformattablestring-Breaking Change
Die Änderung, die bei der Überladungs Auflösung bei `string` `ValueFormattableString` bevorzugen, ist eine Breaking Change. Es ist möglich, dass ein Entwickler heute einen Typ namens "`ValueFormattableString`" definiert und ihn in Methoden Überladungen mit `string`verwendet. Diese vorgeschlagene Änderung würde bewirken, dass der Compiler eine andere Überladung ausgewählt hat, nachdem dieser Satz von Features implementiert wurde. 

Die Möglichkeit, dies zu tun, ist relativ gering. Der-Typ benötigt den vollständigen Namen `System.ValueFormattableString` und muss `static` Methoden mit dem Namen `Create`haben. Da Entwickler dringend davon ausgehen, dass Sie keinen Typ im `System` Namespace definieren, scheint diese Unterbrechung eine sinnvolle Gefährdung zu sein.

### <a name="expanding-to-more-types"></a>Erweitern auf weitere Typen
Da wir uns in diesem Bereich befinden, sollten wir `IList<T>`, `ICollection<T>` und `IReadOnlyList<T>` der Sammlung von Sammlungen hinzufügen, für die `params` unterstützt wird. Im Hinblick auf die Implementierung kostet die andere Arbeit hier eine kleine Menge.

LDM muss entscheiden, ob die Komplikation für die Sprache den Wert hat. Durch das Hinzufügen von `IEnumerable<T>` wird ein sehr spezifischer Reibungspunkt entfernt. Wenn diese `params` Lösung fehlt, waren viele Kunden gezwungen, `T[]` aus einem `IEnumerable<T>` zuzuweisen, wenn eine `params`-Methode aufgerufen wurde. Durch das Hinzufügen von `IEnumerable<T>` wird dies behoben. Es gibt keinen bestimmten Reibungspunkt, der von den anderen Schnittstellen behoben wird. 

## <a name="considerations"></a>Überlegungen

### <a name="variant2-and-variant3"></a>Variant2 und Variant3
Das corefx-Team verfügt auch über einen nicht Zuweisungs Satz von Speichertypen für bis zu drei `Variant` Argumente. Dabei handelt es sich um eine einzelne `Variant`, `Variant2` und `Variant3`. Alle verfügen über ein paar Methoden, um eine Zuordnung frei `Span<Variant>` von Ihnen zu erhalten: `CreateSpan` und `KeepAlive`. Dies bedeutet, dass für eine `params Span<Variant>` von bis zu drei Argumenten die Website für die Website vollständig freigegeben werden kann.

``` csharp
static class ZeroAllocation {
    static void Use(params Span<Variant> spans) {
        ...
    }

    static void Go() {
        Use("hello", "world");
    }
}
```

Die `Go`-Methode kann auf Folgendes herabgesetzt werden:

``` csharp
static class ZeroAllocation {
    static void Go() {
        Variant2 _v;
        _v.Variant1 = new Variant("hello");
        _v.Variant2 = new Variant("word");
        Use(_v.CreateSpan());
        _v.KeepAlive();
    }
}
```

Dies erfordert sehr wenig Aufwand, um `T[]` zwischen `params Span<T>` aufrufen wiederzuverwenden. Der Compiler muss bereits einen temporären pro-Rückruf verwalten und eine Bereinigungs Arbeit nach ausführen (auch wenn er in einem Fall nur eine interne Temp als frei markiert). 

Hinweis: die `KeepAlive`-Funktion ist nur auf dem Desktop erforderlich. In .net Core ist die Methode nicht verfügbar, daher gibt der Compiler keinen aufzurufenden Befehl aus.

### <a name="clr-stack-allocation-helpers"></a>Hilfe zur CLR-Stapel Zuordnung
Die CLR bietet nur [localloc](https://docs.microsoft.com/en-us/dotnet/api/system.reflection.emit.opcodes.localloc?redirectedfrom=MSDN&view=netframework-4.7.2) für die Stapel Zuordnung von zusammenhängenden Arbeitsspeicher. Diese Anweisung ist darauf beschränkt, dass Sie nur für `unmanaged` Typen funktioniert. Dies bedeutet, dass Sie nicht als universelle Lösung für die effiziente Zuordnung des Sicherungs Speichers für `params 
 Span<T>`verwendet werden kann. 

Diese Einschränkung ist jedoch nicht eine grundlegende Einschränkung, sondern vielmehr ein Element des Verlaufs. Die CLR könnte neue op-Codes/intrinsie-Funktionen hinzufügen, die eine universelle Stapel Zuordnung bereitstellen. Diese können dann verwendet werden, um den Sicherungs Speicher für die meisten `params Span<T>` Aufrufe zuzuordnen.

``` csharp
static class BetterAllocation {
    static void Use(params Span<string> spans) {
        ...
    }

    static void Go() {
        Use("hello", "world");
    }
}
```

Die `Go`-Methode kann auf Folgendes herabgesetzt werden:

``` csharp
static class ZeroAllocation {
    static void Go() {
        Span<T> span = RuntimeIntrinsic.StackAlloc<string>(length: 2);
        span[0] = "hello";
        span[1] = "world";
        Use(span);
    }
}
```

Obwohl dieser Ansatz sehr Heap effizient ist, führt dies zu einer zusätzlichen Stapel Auslastung. In einem Algorithmus, der über einen tiefen Stapel und viele `params` Verwendung verfügt, kann dies dazu führen, dass eine `StackOverflowException` generiert wird, wenn eine einfache `T[]` Zuordnung erfolgreich ist. 

Leider C# ist nicht für den Typ der Analyse übergreifende Analyse eingerichtet, bei der es eine fundierte Entscheidung treffen könnte, ob die Stapel-oder Heap Zuordnung von `params`verwendet werden soll. Die einzelnen Aufrufe können nur für Sie selbst in Erwägung gezogen werden.

Die CLR ist die beste Einrichtung, um diese Art von Bestimmung zur Laufzeit zu treffen. Daher ist es wahrscheinlich, dass die Laufzeit zwei Methoden für die universelle Stapel Zuweisung bereitstellt:

1. `Span<T> StackAlloc<T>(int length)`: Dies hat das gleiche Verhalten und die gleichen Einschränkungen wie `localloc`, außer dass es für beliebige Typen `T`funktionieren kann. 
1. `Span<T> MaybeStackAlloc<T>(int length)`: Diese Laufzeit kann diese Option implementieren, indem Sie eine Stapel-oder Heap Zuordnung durchführen. Die Laufzeit kann dann den Ausführungs Kontext verwenden, in dem Sie aufgerufen wird, um zu bestimmen, wie die `Span<T>` zugeordnet wird. Der Aufrufer behandelt ihn jedoch immer so, als ob er Stapel zugeordnet wäre.

Für sehr einfache Fälle, wie z. b. ein bis C# zwei Argumente, könnte der Compiler immer `StackAlloc<T>` Variant verwenden. In den meisten Fällen ist es unwahrscheinlich, dass die Stapel Auslastung maßgeblich ist. In anderen Fällen könnte der Compiler entscheiden, `MaybeStackAlloc<T>` stattdessen zu verwenden und die Laufzeit den-Befehl zu übernehmen.

Wie wir uns entscheiden, erfordert wahrscheinlich eine tiefere Untersuchung und Untersuchung von realen apps. Wenn diese neuen systeminternen Funktionen verfügbar sind, erhalten wir diese Art von Flexibilität.

### <a name="why-not-varargs"></a>Warum nicht varargs? 
Die vorhandene [varargs](https://docs.microsoft.com/en-us/cpp/windows/variable-argument-lists-dot-dot-dot-cpp-cli?view=vs-2017) -Funktion wurde hier als mögliche Lösung betrachtet. Diese Funktion ist jedoch in erster Linie C++für/CLI-Szenarien gedacht und verfügt über bekannte Lücken für andere Szenarien. Außerdem entstehen beträchtliche Kosten bei der Portierung auf UNIX. Daher wurde sie nicht als eine funktionierende Lösung angesehen.

## <a name="related-issues"></a>Verwandte Probleme
Diese Spezifikation bezieht sich auf die folgenden Probleme: 

- https://github.com/dotnet/csharplang/issues/1757
- https://github.com/dotnet/csharplang/issues/179
- https://github.com/dotnet/corefxlab/pull/2595

