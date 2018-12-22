# <a name="arrays"></a>Arrays

Ein Array ist eine Datenstruktur, die eine Reihe von Variablen enthält, die über berechnete Indizes zugegriffen wird. In einem Array, das die Elemente des Arrays, so genannte enthaltenen Variablen sind alle vom selben Typ, und diese Art wird den Elementtyp des Arrays bezeichnet.

Ein Array verfügt über einen Rang, der die Anzahl der Indizes, die jedes Arrayelement zugeordnet bestimmt. Der Rang eines Arrays ist auch als Dimensionen des Arrays bezeichnet. Ein Array mit dem Rang eins wird aufgerufen, eine ***eindimensionales Array***. Ein Array mit einem Rang größer als eine aufgerufen wird eine ***mehrdimensionales Array***. Bestimmte Größe mehrdimensionale Arrays werden häufig als zweidimensionale Arrays, dreidimensionale Arrays usw. bezeichnet.

Jeder Dimension eines Arrays verfügt über eine zugeordnete Länge eine ganzzahlige Anzahl größer als oder gleich 0 (null) handelt. Die Längen der Dimension sind nicht Teil des Typs des Arrays, aber eingerichtet, wenn eine Instanz des Arraytyps zur Laufzeit erstellt wird. Die Länge einer Dimension bestimmt des gültigen Bereichs von Indizes für diese Dimension: Für eine Dimension der Länge `N`, Indizes Prioritätswerte liegen zwischen `0` zu `N - 1` inklusive. Die Gesamtanzahl der Elemente in einem Array ist das Produkt der Längen der in jeder Dimension im Array. Wenn eine oder mehrere der Dimensionen eines Arrays eine Länge von 0 (null) haben, hört das Array leer sein.

Ein Array kann einen beliebigen Elementtyp verwenden, einschließlich eines Arraytyps. 

## <a name="array-types"></a>Arraytypen

Array-Typ wird geschrieben, als eine *Non_array_type* gefolgt von einem oder mehreren *Rank_specifier*s:

```antlr
array_type
    : non_array_type rank_specifier+
    ;

non_array_type
    : type
    ;

rank_specifier
    : '[' dim_separator* ']'
    ;

dim_separator
    : ','
    ;
```

Ein *Non_array_type* ist "any" *Typ* , nicht selbst eine *Array_type*.

Der Rang eines Arraytyps wird angegeben, durch die am weitesten links stehende *Rank_specifier* in die *Array_type*: Ein *Rank_specifier* gibt an, dass das Array ein Array mit Rang eins plus die Anzahl der "`,`" Token in der *Rank_specifier*.

Der Elementtyp des Arraytyps ist der Typ, der ergibt, der am weitesten links stehende löschen *Rank_specifier*:

*  Array-Typ des Formulars `T[R]` ist ein Array mit Rang `R` und einen nicht-Array-Elementtyp `T`.
*  Array-Typ des Formulars `T[R][R1]...[Rn]` ist ein Array mit Rang `R` und eines Elementtyps `T[R1]...[Rn]`.

Faktisch sind die *Rank_specifier*s werden von links nach rechts vor dem letzten Element der nicht-Array-Typ gelesen. Der Typ `int[][,,][,]` ist ein eindimensionales Array von dreidimensionalen Arrays eines zweidimensionalen Arrays von `int`.

Zur Laufzeit, kann ein Wert, der ein Arraytyp sein `null` oder ein Verweis auf eine Instanz dieses Arraytyps.

### <a name="the-systemarray-type"></a>Des Typs System.Array

Der Typ `System.Array` ist der abstrakte Basistyp aller Typen von Arrays. Implizite verweiskonvertierung ([implizite Verweis-](conversions.md#implicit-reference-conversions)) vorhanden ist, über einen anderen Arraytyp aufweisen, `System.Array`, und eine explizite Konvertierung ([explizite Konvertierungen](conversions.md#explicit-reference-conversions)) vorhanden ist von `System.Array` auf einen anderen Arraytyp aufweisen. Beachten Sie, dass `System.Array` ist nicht selbst eine *Array_type*. Es handelt sich vielmehr eine *Class_type* von der alle *Array_type*s abgeleitet werden.

Zur Laufzeit, ein Wert vom Typ `System.Array` kann `null` oder ein Verweis auf eine Instanz eines beliebigen Arraytyps.

### <a name="arrays-and-the-generic-ilist-interface"></a>Arrays und die generische IList-Schnittstelle

Ein eindimensionales Array `T[]` implementiert die Schnittstelle `System.Collections.Generic.IList<T>` (`IList<T>` kurz) und die Basis-Schnittstellen. Daher besteht eine implizite Konvertierung von `T[]` zu `IList<T>` und die Basis-Schnittstellen. Darüber hinaus liegt eine implizite verweiskonvertierung von `S` zu `T` dann `S[]` implementiert `IList<T>` und es gibt eine implizite verweiskonvertierung von `S[]` zu `IList<T>` und Basis-Schnittstellen ( [Implizite Verweis-](conversions.md#implicit-reference-conversions)). Es ist eine explizite Konvertierung von `S` zu `T` dann gibt es eine explizite Konvertierung von `S[]` zu `IList<T>` und die Basisschnittstellen ([explizite Konvertierungen](conversions.md#explicit-reference-conversions)). Zum Beispiel:
```csharp
using System.Collections.Generic;

class Test
{
    static void Main() {
        string[] sa = new string[5];
        object[] oa1 = new object[5];
        object[] oa2 = sa;

        IList<string> lst1 = sa;                    // Ok
        IList<string> lst2 = oa1;                   // Error, cast needed
        IList<object> lst3 = sa;                    // Ok
        IList<object> lst4 = oa1;                   // Ok

        IList<string> lst5 = (IList<string>)oa1;    // Exception
        IList<string> lst6 = (IList<string>)oa2;    // Ok
    }
}
```

Die Zuweisung `lst2 = oa1` generiert einen Fehler während der Kompilierung, da die Konvertierung von `object[]` zu `IList<string>` ist eine explizite Konvertierung, die nicht implizit. Die Umwandlung `(IList<string>)oa1` bewirkt, dass eine Ausnahme zur Laufzeit seit werden `oa1` Verweise ein `object[]` und keine `string[]`. Jedoch die Umwandlung `(IList<string>)oa2` führt nicht dazu, dass eine Ausnahme ausgelöst werden, da `oa2` Verweise ein `string[]`.

Jedes Mal, wenn es gibt eine implizite oder explizite verweiskonvertierung von `S[]` zu `IList<T>`, es gibt auch eine explizite Konvertierung von `IList<T>` und Basis-Schnittstellen für `S[]` ([expliziten Verweis Konvertierungen](conversions.md#explicit-reference-conversions)).

Wenn ein Arraytyp `S[]` implementiert `IList<T>`, einige der Member der implementierten Schnittstelle kann Ausnahmen auslösen. Das genaue Verhalten der Implementierung der Schnittstelle ist nicht Gegenstand dieser Spezifikation.

## <a name="array-creation"></a>Arrayerstellung

Arrayinstanzen werden erstellt, indem *Array_creation_expression*s ([Array-Ausdrücke für die Arrayerstellung](expressions.md#array-creation-expressions)) oder durch das Feld oder eine lokale Variablendeklarationen, die enthalten eine *Array_initializer*([Array Initializers](arrays.md#array-initializers)).

Wenn eine Arrayinstanz erstellt wird, ändern Sie den Rang und die-Länge der einzelnen Dimensionen werden festgelegt, und Sie dann konstant bleiben, während der gesamten Lebensdauer der Instanz. Das heißt, es ist nicht möglich, den Rang einer vorhandenen Instanz des Arrays zu ändern, noch ist es möglich, die Größe seiner Dimensionen ändern.

Es ist immer eine Arrayinstanz eines Arraytyps. Die `System.Array` Typ ist ein abstrakter Typ, der nicht instanziiert werden kann.

Elemente des Arrays, die von erstellten *Array_creation_expression*s werden immer auf ihren Standardwert initialisiert ([Standardwerte](variables.md#default-values)).

## <a name="array-element-access"></a>Array Element access

Elemente des Arrays erfolgt mit *Element_access* Ausdrücke ([Array Zugriff](expressions.md#array-access)) des Formulars `A[I1, I2, ..., In]`, wobei `A` ist ein Ausdruck vom Array-Typ und den einzelnen `Ix` ist ein Ein Ausdruck vom Typ `int`, `uint`, `long`, `ulong`, oder an eine oder mehrere der folgenden Typen implizit konvertiert werden kann. Das Ergebnis ein Array Element Access ist eine Variable, nämlich dem Array-Element, das von den Indizes ausgewählt.

Die Elemente eines Arrays aufgelistet werden können, mithilfe einer `foreach` Anweisung ([der Foreach-Anweisung](statements.md#the-foreach-statement)).

## <a name="array-members"></a>Array-Elemente

Jedes Array-Typ erbt die Member deklariert, indem die `System.Array` Typ.

## <a name="array-covariance"></a>Array-Kovarianz

Für zwei *Reference_type*s `A` und `B`, wenn implizite verweiskonvertierung ([implizite Verweis-](conversions.md#implicit-reference-conversions)) oder explizite Konvertierung ([ Explizite Konvertierungen](conversions.md#explicit-reference-conversions)) vorhanden ist, von `A` zu `B`, und klicken Sie dann auch die gleichen verweiskonvertierung aus dem Arraytyp vorhanden ist `A[R]` in den Arraytyp `B[R]`, wobei `R` ist "any" bestimmte *Rank_specifier* (aber sowohl für Arraytypen). Diese Beziehung wird als bezeichnet ***Array-Kovarianz***. Array-Kovarianz vor allem bedeutet, dass einen Wert eines Arraytyps `A[R]` möglicherweise tatsächlich ein Verweis auf eine Instanz des Arraytyps `B[R]`, sofern eine implizite verweiskonvertierung von vorhanden `B` zu `A`.

Aufgrund der Array-Kovarianz enthalten Zuweisungen auf Elemente des Verweistyparrays eine laufzeitüberprüfung wird sichergestellt, dass der Wert zugewiesen wird, auf das Arrayelement tatsächlich einen zulässigen Typ aufweist ([einfache Zuweisung](expressions.md#simple-assignment)). Zum Beispiel:
```csharp
class Test
{
    static void Fill(object[] array, int index, int count, object value) {
        for (int i = index; i < index + count; i++) array[i] = value;
    }

    static void Main() {
        string[] strings = new string[100];
        Fill(strings, 0, 100, "Undefined");
        Fill(strings, 0, 10, null);
        Fill(strings, 90, 10, 0);
    }
}
```

Die Zuweisung zu `array[i]` in die `Fill` Methode enthält implizit eine laufzeitüberprüfung das Objekt, das auf stellt sicher, dass `value` ist entweder `null` oder eine Instanz, die kompatibel mit dem tatsächlichen Element `array`. In `Main`, die ersten beiden Aufrufe der `Fill` erfolgreich, aber der dritte Aufruf bewirkt, dass eine `System.ArrayTypeMismatchException` ausgelöst wird, bei der Ausführung der ersten Zuweisung zu `array[i]`. Die Ausnahme tritt auf, weil eine geschachtelte `int` kann nicht gespeichert werden, einem `string` Array.

Array-Kovarianz speziell erstreckt sich nicht um Arrays von *Value_type*s. Beispielsweise keine Konvertierung möglich ist, ermöglicht eine `int[]` behandelt werden soll, als ein `object[]`.

## <a name="array-initializers"></a>Arrayinitialisierer

Arrayinitialisierer können angegeben werden, in die Steuerelementfeld-Deklarationen ([Felder](classes.md#fields)), lokale Variablendeklarationen ([lokale Variablendeklarationen](statements.md#local-variable-declarations)), und Erstellen von Ausdrücken ([Array erstellen Ausdrücke](expressions.md#array-creation-expressions)):

```antlr
array_initializer
    : '{' variable_initializer_list? '}'
    | '{' variable_initializer_list ',' '}'
    ;

variable_initializer_list
    : variable_initializer (',' variable_initializer)*
    ;

variable_initializer
    : expression
    | array_initializer
    ;
```

Ein Arrayinitialisierer besteht aus einer Sequenz von Variableninitialisierern, eingeschlossen durch "`{`"und"`}`"Token und getrennt durch"`,`" Token. Jede Variableninitialisierer ist ein Ausdruck oder, im Fall eines mehrdimensionalen Arrays, ein geschachtelter Arrayinitialisierer.

Der Kontext, in dem ein Arrayinitialisierer verwendet wird, bestimmt den Typ des Arrays initialisiert wird. In einem Arrayerstellungsausdruck das Array vom Typ sofort steht die Initialisierung oder von die Ausdrücke im Arrayinitialisierer für abgeleitet wird. In einem Feld oder die Deklaration von Variablen ist der Arraytyp den Typ des Felds oder der Variable deklariert wird. Wenn ein Arrayinitialisierer in einem Feld oder die Deklaration von Variablen, wie z. B. verwendet wird:
```csharp
int[] a = {0, 2, 4, 6, 8};
```
Es ist einfach die Kurzschreibweise für einen entsprechenden Ausdruck für die Arrayerstellung:
```csharp
int[] a = new int[] {0, 2, 4, 6, 8};
```

Für ein eindimensionales Array sind muss des Arrayinitialisierers einer Sequenz von Ausdrücken bestehen, die Zuweisung, die mit dem Elementtyp des Arrays kompatibel sind. Die Ausdrücke in aufsteigender Reihenfolge, beginnend mit dem Element am Index 0 (null) Elemente des Arrays initialisiert werden. Die Anzahl der Ausdrücke im Arrayinitialisierer für bestimmt die Länge der Arrayinstanz erstellt wird. Die oben genannten Arrayinitialisierer erstellt z. B. eine `int[]` Instanz der Länge 5, und klicken Sie dann initialisiert die Instanz mit den folgenden Werten:
```csharp
a[0] = 0; a[1] = 2; a[2] = 4; a[3] = 6; a[4] = 8;
```

Für ein mehrdimensionales Array ist müssen die Arrayinitialisierer beliebig viele Ebenen von geschachtelt werden, wenn die Dimensionen im Array vorhanden sind. Die äußerste Schachtelungsebene der am weitesten links stehende Dimension entspricht, und die innerste Schachtelungsebene entspricht die Dimension ganz rechts. Die Länge der einzelnen Dimensionen des Arrays wird durch die Anzahl der Elemente auf der entsprechenden Schachtelungsebene im Arrayinitialisierer bestimmt. Für jede geschachtelter Arrayinitialisierer muss die Anzahl von Elementen identisch mit den anderen Arrayinitialisierer auf derselben Ebene. Beispiel:
```csharp
int[,] b = {{0, 1}, {2, 3}, {4, 5}, {6, 7}, {8, 9}};
```
erstellt ein zweidimensionales Array mit einer Länge von fünf der am weitesten links stehende Dimension und einer Länge von zwei für die Dimension ganz rechts:
```csharp
int[,] b = new int[5, 2];
```
und initialisiert dann das Array Instanz mit den folgenden Werten:
```csharp
b[0, 0] = 0; b[0, 1] = 1;
b[1, 0] = 2; b[1, 1] = 3;
b[2, 0] = 4; b[2, 1] = 5;
b[3, 0] = 6; b[3, 1] = 7;
b[4, 0] = 8; b[4, 1] = 9;
```

Wenn eine andere Dimension als am weitesten rechts mit der Länge 0 (null) angegeben ist, werden die nachfolgenden Dimensionen als auch die Länge 0 (null) haben. Beispiel:
```csharp
int[,] c = {};
```
erstellt ein zweidimensionales Array mit einer Länge von 0 (null), die am weitesten links stehende sowohl für die Dimension ganz rechts:
```csharp
int[,] c = new int[0, 0];
```

Wenn einem Arrayerstellungsausdruck sowohl die Längen explizite Dimension als auch ein Arrayinitialisierer enthält, die Längen müssen Konstante Ausdrücke sein, und die Anzahl der Elemente auf jeder Schachtelungsebene muss die entsprechende Dimensionslänge entsprechen. Hier einige Beispiele:
```csharp
int i = 3;
int[] x = new int[3] {0, 1, 2};        // OK
int[] y = new int[i] {0, 1, 2};        // Error, i not a constant
int[] z = new int[3] {0, 1, 2, 3};     // Error, length/initializer mismatch
```

Hier können die Initialisierung für `y` führt zu einem Kompilierzeitfehler, da es sich bei der Dimension Länge-Ausdruck ist eine Konstante und den Initialisierer für `z` führt zu einem Kompilierzeitfehler, da die Länge und die Anzahl der Elemente in der Initialisierer stimmen nicht überein.
