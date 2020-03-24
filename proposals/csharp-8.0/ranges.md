---
ms.openlocfilehash: 50f2bd2d0a84064cfe35fe65b9e5c59c052d19ac
ms.sourcegitcommit: 1dbb8e82bed5012a58a3a035bf2c3737ed570d07
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/24/2020
ms.locfileid: "80122948"
---
# <a name="ranges"></a>Ranges

## <a name="summary"></a>Zusammenfassung

Diese Funktion dient zum Übernehmen von zwei neuen Operatoren, die das Erstellen von `System.Index`-und `System.Range` Objekten sowie deren Verwendung zum Indizieren/Slice von Auflistungen zur Laufzeit ermöglichen

## <a name="overview"></a>Übersicht

### <a name="well-known-types-and-members"></a>Bekannte Typen und Member

Um die neuen syntaktischen Formen für `System.Index` und `System.Range`zu verwenden, sind möglicherweise neue bekannte Typen und Member erforderlich, je nachdem, welche syntaktischen Formulare verwendet werden.

Um den "hat"-Operator (`^`) zu verwenden, ist Folgendes erforderlich:

```csharp
namespace System
{
    public readonly struct Index
    {
        public Index(int value, bool fromEnd);
    }
}
```

Um den `System.Index`-Typ als Argument in einem Array Element-Zugriff zu verwenden, ist der folgende Member erforderlich:

```csharp
int System.Index.GetOffset(int length);
```

Die `..` Syntax für `System.Range` erfordert den `System.Range` Typ sowie einen oder mehrere der folgenden Member:

```csharp
namespace System
{
    public readonly struct Range
    {
        public Range(System.Index start, System.Index end);
        public static Range StartAt(System.Index start);
        public static Range EndAt(System.Index end);
        public static Range All { get; }
    }
}
```

Die `..`-Syntax ermöglicht, dass sowohl als auch keines der Argumente vorhanden ist. Unabhängig von der Anzahl der Argumente ist der `Range`-Konstruktor für die Verwendung der `Range`-Syntax immer ausreichend. Wenn jedoch eines der anderen Member vorhanden ist, und mindestens ein `..` Argument fehlt, kann der entsprechende Member ersetzt werden.

Zum Schluss muss für einen Wert des Typs `System.Range` in einem Array Element-Zugriffs Ausdruck verwendet werden, der folgende Member muss vorhanden sein:

```csharp
namespace System.Runtime.CompilerServices
{
    public static class RuntimeHelpers
    {
        public static T[] GetSubArray<T>(T[] array, System.Range range);
    }
}
```

### <a name="systemindex"></a>System. Index

C#hat keine Möglichkeit, eine Auflistung vom Ende zu indizieren, aber die meisten Indexer verwenden die "from Start"-Idee oder einen "Length-i"-Ausdruck. Wir stellen einen neuen Index Ausdruck vor, der "vom Ende" bedeutet. Mit diesem Feature wird ein neuer unärer Prefix-Operator (hat) eingeführt. Der einzige Operand muss in `System.Int32`konvertierbar sein. Sie wird in den entsprechenden `System.Index` factorymethodenaufzurufen gesenkt.

Wir erweitern die Grammatik für *unary_expression* mit der folgenden zusätzlichen Syntax Form:

```antlr
unary_expression
    : '^' unary_expression
    ;
```

Dies wird als *Index des endoperators* bezeichnet. Der vordefinierte *Index von endoperatoren* lautet wie folgt:

```csharp
    System.Index operator ^(int fromEnd);
```

Das Verhalten dieses Operators ist nur für Eingabewerte, die größer oder gleich 0 (null) sind, definiert.

Beispiele:

```csharp
var thirdItem = list[2];                // list[2]
var lastItem = list[^1];                // list[Index.CreateFromEnd(1)]

var multiDimensional = list[3, ^2]      // list[3, Index.CreateFromEnd(2)]
```

#### <a name="systemrange"></a>System. Range

C#hat keine syntaktische Methode, um auf Bereiche oder Slices von Sammlungen zuzugreifen. Normalerweise sind Benutzer gezwungen, komplexe Strukturen zu implementieren, um Slices von Arbeitsspeicher zu filtern oder zu verarbeiten, oder auf LINQ-Methoden wie `list.Skip(5).Take(2)`zurückgreifen. Wenn `System.Span<T>` und andere ähnliche Typen hinzugefügt werden, ist es wichtiger, dass diese Art von Vorgang auf einer tieferen Ebene in der Sprache/Laufzeit unterstützt wird und die Schnittstelle vereinheitlicht ist.

In der Sprache wird ein neuer Bereichs Operator `x..y`eingeführt. Dabei handelt es sich um einen binären Infix-Operator, der zwei Ausdrücke akzeptiert. Beide Operanden können ausgelassen werden (Beispiele unten) und müssen in `System.Index`konvertiert werden. Der Vorgang wird auf den entsprechenden `System.Range` Factory-Methoden aufrufsvorgang herabgesetzt.

Wir ersetzen die C# Grammatikregeln für *multiplicative_expression* durch Folgendes (um eine neue Rang folgen Ebene einzuführen):

```antlr
range_expression
    : unary_expression
    | range_expression? '..' range_expression?
    ;

multiplicative_expression
    : range_expression
    | multiplicative_expression '*' range_expression
    | multiplicative_expression '/' range_expression
    | multiplicative_expression '%' range_expression
    ;
```

Alle Formen des *Bereichs Operators* haben die gleiche Rangfolge. Diese neue Rang Folge Gruppe ist niedriger als die *unären Operatoren* und höher als die *arithmetischen Operatoren mulitiplizierung*.

Wir nennen den `..` Operator den *Bereichs Operator*. Der integrierte Bereichs Operator kann ungefähr verstanden werden, um dem Aufruf eines integrierten Operators in dieser Form zu entsprechen:

```csharp
    System.Range operator ..(Index start = 0, Index end = ^0);
```

Beispiele:

```csharp
var slice1 = list[2..^3];               // list[Range.Create(2, Index.CreateFromEnd(3))]
var slice2 = list[..^3];                // list[Range.ToEnd(Index.CreateFromEnd(3))]
var slice3 = list[2..];                 // list[Range.FromStart(2)]
var slice4 = list[..];                  // list[Range.All]

var multiDimensional = list[1..2, ..]   // list[Range.Create(1, 2), Range.All]
```

Darüber hinaus sollten `System.Index` eine implizite Konvertierung von `System.Int32`haben, damit keine Überlastung erforderlich ist, um ganze Zahlen und Indizes über mehrdimensionale Signaturen zu mischen.

## <a name="adding-index-and-range-support-to-existing-library-types"></a>Hinzufügen von Index-und Bereichs Unterstützung zu vorhandenen Bibliothekstypen

### <a name="implicit-index-support"></a>Implizite Index Unterstützung

Die Sprache stellt einen instanzindexer-Member mit einem einzelnen Parameter vom Typ `Index` für Typen bereit, die die folgenden Kriterien erfüllen:

- Der Typ ist "zählbar".
- Der-Typ verfügt über einen barrierefreien instanzindexer, der einen einzelnen `int` als Argument annimmt.
- Der-Typ verfügt über keinen zugänglichen instanzindexer, der einen `Index` als ersten Parameter annimmt. Der `Index` muss der einzige Parameter sein, oder die restlichen Parameter müssen optional sein.

Ein Typ ist " ***zählbar*** ", wenn er über eine Eigenschaft mit dem Namen `Length` oder `Count` mit einem zugänglichen Getter und einem Rückgabetyp von `int`verfügt. Die Sprache kann diese Eigenschaft verwenden, um einen Ausdruck vom Typ `Index` in eine `int` an der Stelle des Ausdrucks zu konvertieren, ohne dass der Typ `Index` verwendet werden muss. Wenn sowohl `Length` als auch `Count` vorhanden sind, werden `Length` bevorzugt. Der Einfachheit halber wird in diesem Vorschlag der Name `Length` verwendet, um `Count` oder `Length`darzustellen.

Bei solchen Typen verhält sich die Sprache so, als ob ein Indexer-Member der Form `T this[Index index]` ist, wobei `T` der Rückgabetyp des `int` basierten Indexers ist, einschließlich aller `ref`-Stil Anmerkungen. Das neue Element hat denselben `get` und `set` Member mit überein stimmendem Zugriff als `int` Indexer. 

Der neue Indexer wird implementiert, indem das Argument vom Typ `Index` in eine `int` umgewandelt und ein Aufruf an den `int` basierten Indexer ausgegeben wird. Zu Diskussions Zwecken verwenden wir das Beispiel `receiver[expr]`. Die Konvertierung von `expr` in `int` erfolgt wie folgt:

- Wenn das-Argument das Formular `^expr2` und der Typ der `expr2` `int`ist, wird es in `receiver.Length - expr2`übersetzt.
- Andernfalls wird Sie als `expr.GetOffset(receiver.Length)`übersetzt.

Dies ermöglicht es Entwicklern, die `Index` Funktion für vorhandene Typen zu verwenden, ohne dass Änderungen erforderlich sind. Beispiel:

``` csharp
List<char> list = ...;
var value = list[^1]; 

// Gets translated to 
var value = list[list.Count - 1]; 
```

Die `receiver`-und `Length` Ausdrücke werden nach Bedarf überlaufen, um sicherzustellen, dass Nebeneffekte nur einmal ausgeführt werden. Beispiel:

``` csharp
class Collection {
    private int[] _array = new[] { 1, 2, 3 };

    int Length {
        get {
            Console.Write("Length ");
            return _array.Length;
        }
    }

    int this[int index] => _array[index];
}

class SideEffect {
    Collection Get() {
        Console.Write("Get ");
        return new Collection();
    }

    void Use() { 
        int i = Get()[^1];
        Console.WriteLine(i);
    }
}
```

Mit diesem Code wird "Get length 3" gedruckt.

### <a name="implicit-range-support"></a>Implizite Bereichs Unterstützung

Die Sprache stellt einen instanzindexer-Member mit einem einzelnen Parameter vom Typ `Range` für Typen bereit, die die folgenden Kriterien erfüllen:

- Der Typ ist "zählbar".
- Der-Typ verfügt über einen zugänglichen Member mit dem Namen `Slice`, der zwei Parameter vom Typ `int`hat.
- Der-Typ verfügt über keinen instanzindexer, der einen einzelnen `Range` als ersten Parameter annimmt. Der `Range` muss der einzige Parameter sein, oder die restlichen Parameter müssen optional sein.

Bei solchen Typen wird die Sprache so gebunden, als ob es einen Indexer-Member in der Form gibt `T this[Range range]` wobei `T` der Rückgabetyp der `Slice` Methode ist, einschließlich aller `ref`-Stil Anmerkungen. Der neue Member verfügt auch über übereinstimmende Barrierefreiheit mit `Slice`. 

Wenn der `Range` basierte Indexer an einen Ausdruck mit dem Namen `receiver`gebunden ist, wird er verringert, indem der `Range` Ausdruck in zwei Werte umgewandelt wird, die dann an die `Slice`-Methode weitergegeben werden. Zu Diskussions Zwecken verwenden wir das Beispiel `receiver[expr]`.

Das erste Argument `Slice` wird durch das Umrechnen des typisierten Bereichs Ausdrucks in folgender Weise erreicht:

- Wenn `expr` die Form `expr1..expr2` (wo `expr2` ausgelassen werden kann) und `expr1` den Typ `int`hat, wird es als `expr1`ausgegeben.
- Wenn `expr` das Formular `^expr1..expr2` (wobei `expr2` ausgelassen werden kann), wird es als `receiver.Length - expr1`ausgegeben.
- Wenn `expr` das Formular `..expr2` (wobei `expr2` ausgelassen werden kann), wird es als `0`ausgegeben.
- Andernfalls wird Sie als `expr.Start.GetOffset(receiver.Length)`ausgegeben.

Dieser Wert wird bei der Berechnung des zweiten `Slice` Arguments wieder verwendet. Wenn dies der Fall ist, wird es als `start`bezeichnet. Das zweite Argument von `Slice` wird durch die folgende typumrechnung des Bereichs typisierten Ausdrucks abgerufen:

- Wenn `expr` die Form `expr1..expr2` (wo `expr1` ausgelassen werden kann) und `expr2` den Typ `int`hat, wird es als `expr2 - start`ausgegeben.
- Wenn `expr` das Formular `expr1..^expr2` (wobei `expr1` ausgelassen werden kann), wird es als `(receiver.Length - expr2) - start`ausgegeben.
- Wenn `expr` das Formular `expr1..` (wobei `expr1` ausgelassen werden kann), wird es als `receiver.Length - start`ausgegeben.
- Andernfalls wird Sie als `expr.End.GetOffset(receiver.Length) - start`ausgegeben.

Die `receiver`-, `Length`-und `expr`-Ausdrücke werden nach Bedarf überlaufen, um sicherzustellen, dass Nebeneffekte nur einmal ausgeführt werden. Beispiel:

``` csharp
class Collection {
    private int[] _array = new[] { 1, 2, 3 };

    int Length {
        get {
            Console.Write("Length ");
            return _array.Length;
        }
    }

    int[] Slice(int start, int length) { 
        var slice = new int[length];
        Array.Copy(_array, start, slice, 0, length);
        return slice;
    }
}

class SideEffect {
    Collection Get() {
        Console.Write("Get ");
        return new Collection();
    }

    void Use() { 
        var array = Get()[0..2];
        Console.WriteLine(array.length);
    }
}
```

Mit diesem Code wird "Get length 2" gedruckt.

In der Sprache werden die folgenden bekannten Typen als Sonderzeichen verwendet: 

- `string`: die Methode `Substring` wird anstelle von `Slice`verwendet.
- `array`: die Methode `System.Reflection.CompilerServices.GetSubArray` wird anstelle von `Slice`verwendet.

## <a name="alternatives"></a>Alternativen

Die neuen Operatoren (`^` und `..`) sind syntaktische Sugar. Die Funktionalität kann durch explizite Aufrufe von `System.Index`-und `System.Range` Factorymethoden implementiert werden, führt jedoch zu viel mehr Code Bausteinen, und die Benutzer Funktionalität ist nicht intuitiv.

## <a name="il-representation"></a>Il-Darstellung

Diese beiden Operatoren werden auf reguläre Indexer-/Methodenaufrufe herabgesetzt, ohne Änderungen in nachfolgenden compilerschichten.

## <a name="runtime-behavior"></a>Laufzeitverhalten

- Der Compiler kann Indexer für integrierte Typen wie Arrays und Zeichen folgen optimieren und die Indizierung auf die entsprechenden vorhandenen Methoden verringern.
- `System.Index` wird ausgelöst, wenn ein Wert mit einem negativen Wert erstellt wird.
- `^0` löst nicht aus, aber es wird in die Länge der Auflistung bzw. der Enumerable übersetzt, für die es bereitgestellt wird.
- `Range.All` ist semantisch äquivalent zu `0..^0`und kann für diese Indizes deaktiviert werden.

## <a name="considerations"></a>Überlegungen

### <a name="detect-indexable-based-on-icollection"></a>Erkennen von indexbaren auf der Grundlage von ICollection

Die Inspiration für dieses Verhalten waren Auflistungsinitialisierer. Verwenden der Struktur eines Typs, um zu vermitteln, dass Sie sich für eine Funktion entschieden hat. Bei Auflistungsinitialisierern können Typen die Funktion aktivieren, indem Sie die-Schnittstelle implementieren `IEnumerable` (nicht generisch).

Dieser Vorschlag erforderte zunächst, dass die Typen `ICollection` implementieren, um sich als indexbar zu qualifizieren. Dies erforderte jedoch eine Reihe von besonderen Fällen:

- `ref struct`: Diese können keine Schnittstellen implementieren, aber Typen wie `Span<T>` eignen sich ideal für Index-und Bereichs Unterstützung. 
- `string`: implementiert keine `ICollection`, und das Hinzufügen dieser `interface` hat große Kosten.

Dies bedeutet, dass die Groß-/Kleinschreibung von Schlüsseltypen bereits erforderlich ist. Die Groß-/Kleinschreibung von `string` ist weniger interessant, da die Sprache dies in anderen Bereichen (`foreach` Herabsetzung, Konstanten usw.) tut. Die Groß-/Kleinschreibung von `ref struct` ist eher darauf zu achten, dass eine ganze Klasse von Typen eine besondere Schreibweise ist. Sie werden als Indexer bezeichnet, wenn Sie einfach über eine Eigenschaft mit dem Namen "`Count`" mit dem Rückgabetyp "`int`" verfügen. 

Nach der Überlegung wurde der Entwurf normalisiert, um zu sagen, dass jeder Typ, der über eine Eigenschaft `Count` / `Length` mit dem Rückgabetyp `int` ist, indexbar ist. Dadurch werden alle Sonderzeichen entfernt, auch für `string` und Arrays.

### <a name="detect-just-count"></a>Nur Anzahl erkennen

Das Erkennen von Eigenschaften Namen `Count` oder `Length` erschwert das Design etwas. Wenn Sie nur eine für die Standardisierung auswählen, reicht dies nicht aus, da Sie am Ende eine große Anzahl von Typen ausschließt:

- Verwenden Sie `Length`: schließt in System. Collections und untergeordneten Namespaces sehr viele Sammlungen aus. Diese sind tendenziell von `ICollection` abgeleitet und bevorzugen daher `Count` über Länge.
- `Count`verwenden: schließt `string`, Arrays, `Span<T>` und die meisten `ref struct` basierten Typen aus.

Die zusätzliche Komplikation bei der anfänglichen Erkennung von indexbaren Typen wird durch die Vereinfachung in anderen Aspekten überwogen.

### <a name="choice-of-slice-as-a-name"></a>Auswahl des Slice als Name

Der Name `Slice` wurde als de-facto-Standardname für Slice-Vorgänge in .net ausgewählt. Ab netcoreapp 2.1 verwenden alle Spannen Stiltypen den Namen `Slice` für aufteilen-Vorgänge. Vor netcoreapp 2.1 gibt es keine Beispiele für das Aufteilen von aufteilen, um ein Beispiel zu finden. Typen wie `List<T>`, `ArraySegment<T>``SortedList<T>` wären ideal für die slizierung, aber das Konzept war nicht vorhanden, als Typen hinzugefügt wurden. 

Daher ist `Slice` das einzige Beispiel, dass es als Name ausgewählt wurde.

### <a name="index-target-type-conversion"></a>Konvertierung des Index Ziel Typs

Eine andere Möglichkeit, die `Index` Transformation in einem Indexer-Ausdruck anzuzeigen, ist die Konvertierung des Zieltyps. Anstatt so zu binden, als ob ein Member der Form `return_type this[Index]`ist, weist die Sprache stattdessen eine typisierte Typisierung zu `int`zu. 

Dieses Konzept kann für alle Element Zugriffe auf zählbare Typen generalisiert werden. Jedes Mal, wenn ein Ausdruck mit dem Typ `Index` als Argument für einen instanzelementaufruf verwendet wird und der Empfänger zählbar ist, verfügt der Ausdruck über eine Zieltyp Konvertierung in `int`. Die für diese Konvertierung anwendbaren Element Aufrufe umfassen Methoden, Indexer, Eigenschaften, Erweiterungs Methoden usw... Nur Konstruktoren werden ausgeschlossen, da Sie über keinen Empfänger verfügen. 

Die Zieltyp Konvertierung wird für jeden Ausdruck, der einen `Index`Typ aufweist, wie folgt implementiert. Für Diskussions Zwecke können Sie das Beispiel `receiver[expr]`verwenden:

- Wenn `expr` das Formular `^expr2` und der Typ der `expr2` `int`ist, wird er in `receiver.Length - expr2`übersetzt.
- Andernfalls wird Sie als `expr.GetOffset(receiver.Length)`übersetzt.

Die `receiver`-und `Length` Ausdrücke werden nach Bedarf überlaufen, um sicherzustellen, dass Nebeneffekte nur einmal ausgeführt werden. Beispiel:

``` csharp
class Collection {
    private int[] _array = new[] { 1, 2, 3 };

    int Length {
        get {
            Console.Write("Length ");
            return _array.Length;
        }
    }

    int GetAt(int index) => _array[index];
}

class SideEffect {
    Collection Get() {
        Console.Write("Get ");
        return new Collection();
    }

    void Use() { 
        int i = Get().GetAt(^1);
        Console.WriteLine(i);
    }
}
```

Mit diesem Code wird "Get length 3" gedruckt. 

Diese Funktion ist nützlich für alle Member, die über einen Parameter verfügen, der einen Index repräsentiert. Beispiel: `List<T>.InsertAt`. Dies kann auch Verwirrung verursachen, da die Sprache keine Hinweise dazu gibt, ob ein Ausdruck für die Indizierung vorgesehen ist. Dabei können alle `Index` Ausdrücke in `int` konvertiert werden, wenn ein Member für einen zählbaren Typ aufgerufen wird. 

Begrenzungen

- Diese Konvertierung ist nur anwendbar, wenn der Ausdruck mit dem Typ `Index` direkt ein Argument für den Member ist. Dies gilt nicht für andere Ausdrücke.

## <a name="decisions-made-during-implementation"></a>Entscheidungen bei der Implementierung

- Alle Elemente im Muster müssen Instanzmember sein.
- Wenn eine Length-Methode gefunden wird, jedoch den falschen Rückgabetyp aufweist, sollten Sie die Anzahl weiterhin suchen.
- Der Indexer, der für das Index Muster verwendet wird, muss genau einen int-Parameter aufweisen.
- Die für das Bereichs Muster verwendete Slice-Methode muss genau zwei int-Parameter aufweisen.
- Bei der Suche nach den mustermembern suchen wir nach ursprünglichen Definitionen, nicht nach konstruierten Membern.

## <a name="design-meetings"></a>Treffen von Besprechungen

- https://github.com/dotnet/csharplang/blob/master/meetings/2019/LDM-2019-04-01.md

## <a name="questions"></a>Fragen
