# <a name="unsafe-code"></a>Unsicherer Code

Der Kern von c#-Sprache unterscheidet sich gemäß der in den vorangegangenen Kapiteln, insbesondere von C- und C++ in Auslassen von Zeigern als Datentyp. Stattdessen bietet C#-Verweise und die Möglichkeit zum Erstellen von Objekten, die von einem Garbage Collector verwaltet werden. Aufgrund dieses Designs, zusammen mit anderen Funktionen sind c# eine viel sicherere Sprache als C- oder C++. In der Kernsprache c# ist es einfach nicht möglich, dass eine nicht initialisierte Variable, eine "verwaiste" Zeiger oder ein Ausdruck, der ein Array außerhalb ihrer Grenzen indiziert. Gesamte Kategorien von Fehlern kämpfen, routinemäßig C und C++-Programme werden somit entfernt.

Praktisch jeder Zeiger-Typ-Konstrukt in C oder C++ eine Verweis-Typ-Entsprechung in C# geschrieben wurde, sind jedoch trotzdem, gibt es Situationen, in denen Zugriff auf Zeigertypen notwendig wird. Z. B. eine Schnittstelle mit dem zugrunde liegenden Betriebssystem, den Zugriff auf ein Gerät mit zugewiesenem Speicher oder einen zeitkritischen Algorithmus implementieren möglich oder praktikabel ist, ohne Zugriff auf die Zeiger möglicherweise nicht. Um diesem Erfordernis entgegenzukommen, c# bietet die Möglichkeit zum Schreiben ***unsicheren Code***.

In unsicherem Code ist es möglich, deklarieren und Arbeiten mit Zeigern, die zum Durchführen von Konvertierungen zwischen Zeigern und ganzzahligen Typen, für die Adresse der Variablen, und so weiter. In gewisser Hinsicht ähnelt das Schreiben von unsicherem Code Schreiben von C#-Code in einem C#-Programm.

Unsicherer Code ist in der Tat ein "sicher" Feature aus Sicht der Entwickler und Benutzer. Unsicherer Code muss eindeutig gekennzeichnet werden, mit dem Modifizierer `unsafe`, sodass Entwickler können keine möglicherweise unsichere Funktionen versehentlich, und die ausführungs-Engine funktioniert, um sicherzustellen, dass die unsicherer Code in einer nicht vertrauenswürdigen Umgebung ausgeführt werden kann.

## <a name="unsafe-contexts"></a>Nicht sicheren Kontexten

Die unsichere Funktionen von c# sind nur in einem unsicheren Kontext verfügbar. Ein unsicherer Kontext wird eingeführt, durch Einschließen einer `unsafe` Modifizierer in der Deklaration eines Typs oder Members oder durch den Einsatz einer *Unsafe_statement*:

*  Eine Deklaration einer Klasse, Struktur, Schnittstelle oder Delegaten eventuell eine `unsafe` Modifizierer, die in der Groß-und Kleinschreibung der gesamte Text Wertebereich diese Typdeklaration (einschließlich des Texts der Klasse, Struktur oder Schnittstelle) einen unsicheren Kontext berücksichtigt wird.
*  Eine Deklaration eines Felds, Methode, Eigenschaft, Ereignis, Indexer, Operator, Instanzenkonstruktor, Destruktor oder statischen Konstruktor enthalten möglicherweise eine `unsafe` Modifizierer verwenden, in dem Fall der gesamte Text Wertebereich, Memberdeklaration eine unsichere gilt Kontext.
*  Ein *Unsafe_statement* ermöglicht die Verwendung von einem unsicheren Kontext innerhalb einer *Block*. Das Ausmaß der gesamte Text des zugeordneten *Block* einen unsicheren Kontext gilt.

Die zugeordneten Grammatikproduktionen werden unten angezeigt.

```antlr
class_modifier_unsafe
    : 'unsafe'
    ;

struct_modifier_unsafe
    : 'unsafe'
    ;

interface_modifier_unsafe
    : 'unsafe'
    ;

delegate_modifier_unsafe
    : 'unsafe'
    ;

field_modifier_unsafe
    : 'unsafe'
    ;

method_modifier_unsafe
    : 'unsafe'
    ;

property_modifier_unsafe
    : 'unsafe'
    ;

event_modifier_unsafe
    : 'unsafe'
    ;

indexer_modifier_unsafe
    : 'unsafe'
    ;

operator_modifier_unsafe
    : 'unsafe'
    ;

constructor_modifier_unsafe
    : 'unsafe'
    ;

destructor_declaration_unsafe
    : attributes? 'extern'? 'unsafe'? '~' identifier '(' ')' destructor_body
    | attributes? 'unsafe'? 'extern'? '~' identifier '(' ')' destructor_body
    ;

static_constructor_modifiers_unsafe
    : 'extern'? 'unsafe'? 'static'
    | 'unsafe'? 'extern'? 'static'
    | 'extern'? 'static' 'unsafe'?
    | 'unsafe'? 'static' 'extern'?
    | 'static' 'extern'? 'unsafe'?
    | 'static' 'unsafe'? 'extern'?
    ;

embedded_statement_unsafe
    : unsafe_statement
    | fixed_statement
    ;

unsafe_statement
    : 'unsafe' block
    ;
```

Im Beispiel

```csharp
public unsafe struct Node
{
    public int Value;
    public Node* Left;
    public Node* Right;
}
```

die `unsafe` Modifizierer, die in der Strukturdeklaration angegebenen bewirkt, dass den gesamte Text Wertebereich die Strukturdeklaration zu einem unsicheren Kontext. Daher ist es möglich, deklarieren die `Left` und `Right` Felder eines Zeigertyps sein. Das obige Beispiel könnte auch geschrieben werden

```csharp
public struct Node
{
    public int Value;
    public unsafe Node* Left;
    public unsafe Node* Right;
}
```

Hier ist die `unsafe` Modifizierer in den Felddeklarationen dazu führen, dass diese Deklarationen nicht sicheren Kontexten berücksichtigt werden.

Im Gegensatz zum Einrichten eines unsicheren Kontexts, also eine ermöglicht die Verwendung von Zeigertypen, die `unsafe` Modifizierer hat keine Auswirkungen auf einen Typ oder Member. Im Beispiel

```csharp
public class A
{
    public unsafe virtual void F() {
        char* p;
        ...
    }
}

public class B: A
{
    public override void F() {
        base.F();
        ...
    }
}
```

die `unsafe` Modifizierer für den `F` -Methode in der `A` einfach führt dazu, dass der Text der `F` zu einem unsicheren Kontext, in dem die unsicheren Funktionen der Sprache verwendet werden können. Überschreiben `F` in `B`, besteht keine Notwendigkeit, geben Sie erneut die `unsafe` Modifizierer – es sei denn, natürlich die `F` -Methode in der `B` selbst benötigt Zugriff auf unsichere Funktionen.

Die Situation ist etwas anders, wenn ein Zeigertyp Teil der Signatur der Methode ist.

```csharp
public unsafe class A
{
    public virtual void F(char* p) {...}
}

public class B: A
{
    public unsafe override void F(char* p) {...}
}
```

Hier ist da `F`Signatur enthält einen Zeigertyp, es kann nur geschrieben werden in einem unsicheren Kontext. Allerdings kann der unsichere Kontext eingeführt werden, indem entweder die gesamte Klasse unsicher, wie in der Fall ist `A`, oder ein `unsafe` -Modifizierer in der Deklaration der Methode, wie in der Fall ist `B`.

## <a name="pointer-types"></a>Zeigertypen

In einem unsicheren Kontext einen *Typ* ([Typen](types.md)) möglicherweise ein *Pointer_type* als auch ein *Value_type* oder *Reference_type* . Allerdings eine *Pointer_type* kann ebenfalls verwendet werden, eine `typeof` Ausdruck ([anonyme Erstellung Objektausdrücke](expressions.md#anonymous-object-creation-expressions)) außerhalb eines unsicheren Kontexts als solche Verwendung ist nicht unsicher.

```antlr
type_unsafe
    : pointer_type
    ;
```

Ein *Pointer_type* wird geschrieben, als ein *Unmanaged_type* oder das Schlüsselwort `void`, gefolgt von einem `*` token:

```antlr
pointer_type
    : unmanaged_type '*'
    | 'void' '*'
    ;

unmanaged_type
    : type
    ;
```

Vor dem angegebenen Typ der `*` in einen Zeiger Typ wird aufgerufen, die ***Referent Typ*** des Zeigertyps. Es stellt den Typ der Variablen, die auf den Wert der Zeigertyp zeigt dar.

Im Gegensatz zu verweisen (Werte von Verweistypen) Zeigern werden nicht vom Garbage Collector verfolgt, denn der Garbage Collector hat keine Kenntnis von Zeigern und die Daten, die auf die sie verweisen. Aus diesem Grund ein Zeiger ist nicht zulässig, um auf einen Verweis zu verweisen oder auf eine Struktur, die Verweise enthält, und der Referent Typ eines Zeigers muss ein *Unmanaged_type*.

Ein *Unmanaged_type* ist jeder Typ, die keine *Reference_type* oder konstruierter Typ und enthält keine *Reference_type* oder auf einer beliebigen Ebene-Feldern erstellt schachteln. Das heißt, eine *Unmanaged_type* ist eine der folgenden:

*  `sbyte`, `byte`, `short`, `ushort`, `int`, `uint`, `long`, `ulong`, `char`, `float`, `double`, `decimal`, oder `bool`.
*  Alle *Enum_type*.
*  Alle *Pointer_type*.
*  Eine benutzerdefinierte *Struct_type* , keinen konstruierten Typ und enthält Felder *Unmanaged_type*nur.

Die Faustregel für das Mischen von Zeiger und Verweise ist, dass Referenten von verweisen (Objekte) sind zulässig, die Zeiger enthalten, aber Referenten von Zeigern nicht zulässig sind, um die Verweise enthalten.

Einige Beispiele für Zeigertypen werden in der folgenden Tabelle angegeben:

| __Beispiel__ | __Beschreibung__                               |
|-------------|-----------------------------------------------|
| `byte*`     | Zeiger auf `byte`                             |
| `char*`     | Zeiger auf `char`                             |
| `int**`     | Zeiger auf Zeiger auf `int`                   |
| `int*[]`    | Eindimensionales Array von Zeigern auf `int` |
| `void*`     | Zeiger auf unbekannten Typ                       |

Für eine bestimmte Implementierung müssen alle Zeigertypen gleicher Größe und Darstellung.

Im Gegensatz zu C und C++, wenn mehrere Zeiger, in der gleichen Deklaration in c# deklariert werden die `*` wird zusammen mit dem zugrunde liegenden Typ, nicht als ein Präfix Punctuator für jeden Zeigernamen geschrieben. Beispiel:

```csharp
int* pi, pj;    // NOT as int *pi, *pj;
```

Der Wert eines Zeigers, der mit einem Typ `T*` stellt die Adresse einer Variablen vom Typ `T`. Der Zeiger-Dereferenzierungsoperator `*` ([Zeigerdereferenzierung](unsafe-code.md#pointer-indirection)) kann verwendet werden, um diese Variable zugreifen. Angenommen, eine Variable `P` des Typs `int*`, den Ausdruck `*P` kennzeichnet die `int` Variablen finden Sie unter der Adresse, die in enthaltenen `P`.

Wie einen Objektverweis, möglicherweise ein Zeiger `null`. Anwendung des Dereferenzierungsoperators auf einen `null` Zeiger führt die Implementierung definiertes Verhalten. Ein Zeiger mit dem Wert `null` wird durch alle Bits auf 0 dargestellt.

Die `void*` Typ stellt einen Zeiger auf einen unbekannten Typ. Da der Referent-Typ unbekannt ist, kann der Dereferenzierungsoperator kann nicht in einen Zeiger vom Typ angewendet werden `void*`, noch kann für einen solchen Zeiger eine arithmetische Operation ausgeführt werden. Allerdings einen Zeiger vom Typ `void*` umgewandelt werden kann, um einen anderen Zeigertyp (und umgekehrt).

Zeigertypen sind eine eigene Kategorie von Typen. Im Gegensatz zu Verweistypen und Werttypen, Zeigertypen erben nicht von `object` und keine Konvertierung zwischen Zeigertypen und `object`. Insbesondere das boxing und unboxing ([Boxing und unboxing](types.md#boxing-and-unboxing)) werden für Zeiger nicht unterstützt. Allerdings sind Konvertierungen zwischen verschiedenen Zeigertypen sowie zwischen Zeigertypen und ganzzahligen Typen zulässig. Finden Sie im [zeigerkonvertierungen](unsafe-code.md#pointer-conversions).

Ein *Pointer_type* kann nicht als Typargument verwendet werden ([Typen konstruiert](types.md#constructed-types)), und den Typrückschluss ([Typrückschluss](expressions.md#type-inference)) ein Fehler auftritt, auf generische Methodenaufrufen, die abgeleitet haben würde ein Geben Sie ein Argument für ein Zeigertyp sein.

Ein *Pointer_type* als den Typ eines flüchtigen Felds verwendet werden kann ([flüchtige Felder](classes.md#volatile-fields)).

Obwohl Zeiger können, als übergeben werden `ref` oder `out` Parameter nicht definiertem Verhalten führen kann, da der Zeiger gut kann auch festgelegt werden, zeigen Sie auf eine lokale Variable, die nicht mehr vorhanden ist, wenn die aufgerufene Methode zurückgibt, oder das feste Objekt auf das Sie verweisen, ist nicht mehr festgelegt. Zum Beispiel:

```csharp
using System;

class Test
{
    static int value = 20;

    unsafe static void F(out int* pi1, ref int* pi2) {
        int i = 10;
        pi1 = &i;

        fixed (int* pj = &value) {
            // ...
            pi2 = pj;
        }
    }

    static void Main() {
        int i = 10;
        unsafe {
            int* px1;
            int* px2 = &i;

            F(out px1, ref px2);

            Console.WriteLine("*px1 = {0}, *px2 = {1}",
                *px1, *px2);    // undefined behavior
        }
    }
}
```

Eine Methode kann einen Wert eines bestimmten Typs zurückgeben, und dieser Typ kann ein Zeiger sein. Beispielsweise, wenn ein Zeiger auf eine fortlaufende Folge von angegeben `int`s, diese Sequenz die Elementanzahl und einige andere `int` Wert, die folgende Methode gibt die Adresse des entsprechenden Wertes in dieser Reihenfolge, zurück, wenn eine Übereinstimmung vorliegt; andernfalls `null`:

```csharp
unsafe static int* Find(int* pi, int size, int value) {
    for (int i = 0; i < size; ++i) {
        if (*pi == value) 
            return pi;
        ++pi;
    }
    return null;
}
```

Verschiedene Konstrukte stehen in einem unsicheren Kontext für den Betrieb für Zeiger ist:

*  Die `*` Operator kann verwendet werden, um Zeigerdereferenzierung ([Zeigerdereferenzierung](unsafe-code.md#pointer-indirection)).
*  Die `->` Operator über einen Zeiger auf einen Member einer Struktur verwendet werden kann ([Zeigermemberzugriff](unsafe-code.md#pointer-member-access)).
*  Die `[]` Operator kann verwendet werden, um einen Zeiger zu indizieren ([Zeigerelementzugriff auf](unsafe-code.md#pointer-element-access)).
*  Die `&` Operator kann verwendet werden, um die Adresse einer Variablen zu erhalten ([Address-of-Operators](unsafe-code.md#the-address-of-operator)).
*  Die `++` und `--` Operatoren können verwendet werden, um Inkrementieren und Dekrementieren von Zeigern ([Zeiger Inkrementieren und Dekrementieren](unsafe-code.md#pointer-increment-and-decrement)).
*  Die `+` und `-` Operatoren können verwendet werden, um das Ausführen von Zeigerarithmetik ([Zeigerarithmetik](unsafe-code.md#pointer-arithmetic)).
*  Die `==`, `!=`, `<`, `>`, `<=`, und `=>` Operatoren verwendet werden können, Vergleichen von Zeigern ([zeigervergleich](unsafe-code.md#pointer-comparison)).
*  Die `stackalloc` Operator kann verwendet werden, um die speicherbelegung in der Aufrufliste ([Puffer fester Größe](unsafe-code.md#fixed-size-buffers)).
*  Die `fixed` Anweisung kann verwendet werden, um eine Variable vorübergehend zu beheben, damit seine Adresse abgerufen werden kann ([die fixed-Anweisung](unsafe-code.md#the-fixed-statement)).

## <a name="fixed-and-moveable-variables"></a>Feste und verschiebbare Variablen

Der Address-of-Operator ([Address-of-Operators](unsafe-code.md#the-address-of-operator)) und die `fixed` Anweisung ([die fixed-Anweisung](unsafe-code.md#the-fixed-statement)) Variablen in zwei Kategorien unterteilen: ***Variablen fester*** und ***verschiebbare Variablen***.

Feste Variablen befinden sich in die Speicherorte, die von den Vorgang der Garbage Collection nicht betroffen sind. (Beispiele für feste Variablen sind lokale Variablen, Parameter und Variablen, die durch die Dereferenzierung von Zeigern erstellt.) Befinden auf der anderen Seite verschiebbare Variablen an externen Speicherorten, die vom Garbage Collector verschoben oder verworfen werden. (Beispiele für bewegliche Variablen sind die Felder in Objekte und Elemente des Arrays an.)

Die `&` Operator ([Address-of-Operators](unsafe-code.md#the-address-of-operator)) ermöglicht die Adresse des eine feste Variable ohne Einschränkungen abgerufen werden sollen. Aber da verschoben oder verworfen, die vom Garbage Collector eine bewegliche Variable ist, die Adresse des eine bewegliche Variable nur abgerufen werden kann mithilfe einer `fixed` Anweisung ([die fixed-Anweisung](unsafe-code.md#the-fixed-statement)), und diese Adresse bleibt gültig, nur für die Dauer dieser `fixed` Anweisung.

Eine feste Variable ist präzise ausgedrückt eine der folgenden:

*  Eine Variable aufgrund einer *Simple_name* ([einfache Namen](expressions.md#simple-names)), die auf eine lokale Variable oder ein Value-Parameter, es sei denn, die Variable eine anonyme Funktion erfasst wird.
*  Eine Variable aufgrund einer *Member_access* ([Memberzugriff](expressions.md#member-access)) des Formulars `V.I`, wobei `V` ist eine feste Variable von einem *Struct_type*.
*  Eine Variable aufgrund einer *Pointer_indirection_expression* ([Zeigerdereferenzierung](unsafe-code.md#pointer-indirection)) des Formulars `*P`, *Pointer_member_access* ([Zeigermemberzugriff](unsafe-code.md#pointer-member-access)) des Formulars `P->I`, oder ein *Pointer_element_access* ([Zeigerelementzugriff auf](unsafe-code.md#pointer-element-access)) des Formulars `P[E]`.

Alle anderen Variablen werden als bewegliche Variablen klassifiziert.

Beachten Sie, dass ein statisches Feld als bewegliche Variable klassifiziert wurde. Beachten Sie, dass eine `ref` oder `out` Parameter wird als eine bewegliche Variable klassifiziert, selbst wenn das Argument für den Parameter erhält eine feste Variable ist. Beachten Sie außerdem, dass eine Variable, die durch einen Zeiger dereferenziert erzeugt immer als eine feste Variable klassifiziert wurde.

## <a name="pointer-conversions"></a>Zeigerkonvertierungen

In einem unsicheren Kontext, den Satz von verfügbaren impliziten Konvertierungen ([implizite Konvertierungen](conversions.md#implicit-conversions)) wird erweitert, um die folgenden implizite zeigerkonvertierungen einzuschließen:

*  Von jedem *Pointer_type* in den Typ `void*`.
*  Von der `null` in einen literalen *Pointer_type*.

Darüber hinaus in einem unsicheren Kontext, den Satz von verfügbaren explizite Konvertierungen ([explizite Konvertierungen](conversions.md#explicit-conversions)) wurde erweitert und die folgende explizite zeigerkonvertierungen enthalten:

*  Von jedem *Pointer_type* auch einer beliebigen anderen *Pointer_type*.
*  Von `sbyte`, `byte`, `short`, `ushort`, `int`, `uint`, `long`, oder `ulong` auf *Pointer_type*.
*  Von jedem *Pointer_type* zu `sbyte`, `byte`, `short`, `ushort`, `int`, `uint`, `long`, oder `ulong`.

Schließlich wird in einem unsicheren Kontext, den Satz von standardmäßigen implizite Konvertierungen ([Standard implizite Konvertierungen](conversions.md#standard-implicit-conversions)) enthält die folgende zeigerkonvertierung:

*  Von jedem *Pointer_type* in den Typ `void*`.

Konvertierungen zwischen zwei Typen von Funktionszeigern ändern sich nie den eigentlichen Zeigerwert. Das heißt, hat eine Konvertierung von einem Zeigertyp in einen anderen keine Auswirkungen auf die zugrunde liegenden Adresse des Zeigers.

Wenn ein Zeigertyp in eine andere konvertiert wird, wenn der resultierende Zeiger für den Typ auf nicht korrekt ausgerichtet ist, ist das Verhalten undefiniert, wenn das Ergebnis dereferenziert wird. Im Allgemeinen ist das Konzept "korrekt ausgerichtet" transitiv: Wenn ein Zeiger auf den Typ `A` wird für einen Zeiger auf den Typ richtig ausgerichtet `B`, das wiederum wird für einen Zeiger auf den Typ richtig ausgerichtet `C`, klicken Sie dann einen Zeiger auf Typ `A`wird für einen Zeiger auf den Typ richtig ausgerichtet `C`.

Betrachten Sie den folgenden Fall, in dem eine Variable eines bestimmten Typs, über einen Zeiger auf einen anderen Typ zugegriffen wird, aus:

```csharp
char c = 'A';
char* pc = &c;
void* pv = pc;
int* pi = (int*)pv;
int i = *pi;         // undefined
*pi = 123456;        // undefined
```

Wenn ein Zeigertyp in einen Zeiger auf Byte, zeigt das Ergebnis, das niedrigste adressierte Byte der Variablen konvertiert. Aufeinander folgenden Schritten des Ergebnisses wird bis zur Größe der Variablen, ergeben die Zeiger auf die verbleibenden Bytes der Variablen. Beispielsweise zeigt die folgende Methode jedes Byte acht in einen Double-Wert als hexadezimaler Wert:

```csharp
using System;

class Test
{
    unsafe static void Main() {
      double d = 123.456e23;
        unsafe {
           byte* pb = (byte*)&d;
            for (int i = 0; i < sizeof(double); ++i)
               Console.Write("{0:X2} ", *pb++);
            Console.WriteLine();
        }
    }
}
```

Natürlich hängt die Ausgabe von Bytereihenfolge.

Zuordnungen zwischen Zeigern und ganze Zahlen werden in der Implementierung definiert. Jedoch auf 32 * und 64-Bit-CPU-Architekturen mit einem linearen Adressraums, Konvertierungen von Zeigern zu oder von ganzzahligen Typen in der Regel Verhalten sich genau wie Konvertierungen von `uint` oder `ulong` -Werte an oder von diesen ganzzahligen Typen.

### <a name="pointer-arrays"></a>Zeiger-arrays

In einem unsicheren Kontext können Arrays von Zeigern erstellt werden. Nur einige der Konvertierungen, die für andere Arraytypen gelten, sind für Zeiger Arrays zulässig:

*  Die implizite verweiskonvertierung ([implizite Verweis-](conversions.md#implicit-reference-conversions)) aus allen *Array_type* zu `System.Array` und die Schnittstellen, die es implementiert auch gilt, für Zeiger-Arrays. Jedoch jeder Versuch, auf die Elemente des Arrays durch `System.Array` oder die Schnittstellen implementiert führt zur Laufzeit eine Ausnahme, da Zeigertypen nicht konvertierbar sind `object`.
*  Verweisen Sie die implizite und explizite Konvertierungen ([implizite Verweis-](conversions.md#implicit-reference-conversions), [explizite Konvertierungen](conversions.md#explicit-reference-conversions)) aus einem eindimensionalen Array-Typ `S[]` zu `System.Collections.Generic.IList<T>` und die generische Basisschnittstellen gelten nicht für Zeiger-Arrays, auf, da Zeigertypen nicht als Typargumente verwendet werden, und es keine Konvertierungen von Zeigertypen auf Nichtzeiger-Typen gibt.
*  Die explizite Konvertierung ([explizite Konvertierungen](conversions.md#explicit-reference-conversions)) von `System.Array` und die Schnittstellen implementiert, alle *Array_type* gelten für Zeiger-Arrays.
*  Die explizite Konvertierungen ([explizite Konvertierungen](conversions.md#explicit-reference-conversions)) von `System.Collections.Generic.IList<S>` und Basis Schnittstellen verwenden, um einen eindimensionalen Arraytyp `T[]` wird nicht für Zeiger-Arrays, da Zeigertypen sein darf nicht verwendet als Typargumente ein, und es gibt keine Konvertierungen von Zeigertypen auf Nichtzeiger-Typen.

Diese Einschränkungen bedeuten, dass die Erweiterung für die `foreach` Anweisung über Arrays in beschriebenen [der Foreach-Anweisung](statements.md#the-foreach-statement) nicht auf Zeiger-Arrays angewendet werden. Stattdessen eine Foreach-Anweisung der Form

```csharp
foreach (V v in x) embedded_statement
```

in denen der Typ des `x` ist ein Arraytyp des Formulars `T[,,...,]`, `N` ist die Anzahl der Dimensionen minus 1 und `T` oder `V` ist ein Typ, mit geschachtelten for-Schleifen wie folgt erweitert:

```csharp
{
    T[,,...,] a = x;
    for (int i0 = a.GetLowerBound(0); i0 <= a.GetUpperBound(0); i0++)
    for (int i1 = a.GetLowerBound(1); i1 <= a.GetUpperBound(1); i1++)
    ...
    for (int iN = a.GetLowerBound(N); iN <= a.GetUpperBound(N); iN++) {
        V v = (V)a.GetValue(i0,i1,...,iN);
        embedded_statement
    }
}
```

Die Variablen `a`, `i0`, `i1`,..., `iN` sind nicht sichtbar und zugänglich `x` oder *Embedded_statement* oder jeden anderen Quellcode des Programms. Die Variable `v` in die eingebettete Anweisung schreibgeschützt ist. Wenn eine explizite Konvertierung nicht vorhanden ist ([zeigerkonvertierungen](unsafe-code.md#pointer-conversions)) von `T` (der Typ des Elements), `V`, ein Fehler erzeugt wird und keine weiteren Schritte ausgeführt. Wenn `x` hat den Wert `null`, `System.NullReferenceException` wird zur Laufzeit ausgelöst.

## <a name="pointers-in-expressions"></a>Zeiger in Ausdrücken

Klicken Sie in einem unsicheren Kontext ein Ausdruck kann ein Ergebnis von einem Zeigertyp, aber außerhalb eines unsicheren Kontexts, die es ist ein Fehler während der Kompilierung für einen Ausdruck, der ein Zeigertyp sein. Präzise ausgedrückt, außerhalb eines unsicheren Kontexts ein Kompilierungsfehler tritt auf, wenn alle *Simple_name* ([einfache Namen](expressions.md#simple-names)), *Member_access* ([Memberzugriff ](expressions.md#member-access)), *Invocation_expression* ([Aufrufausdrücke](expressions.md#invocation-expressions)), oder *Element_access* ([Elementzugriff](expressions.md#element-access)) ist ein Zeigertyp.

In einem unsicheren Kontext der *Primary_no_array_creation_expression* ([primärausdrücke](expressions.md#primary-expressions)) und *Unary_expression* ([unäre Operatoren](expressions.md#unary-operators)) Produktionen ermöglichen die folgenden zusätzliche Konstrukte:

```antlr
primary_no_array_creation_expression_unsafe
    : pointer_member_access
    | pointer_element_access
    | sizeof_expression
    ;

unary_expression_unsafe
    : pointer_indirection_expression
    | addressof_expression
    ;
```

Diese Konstrukte werden in den folgenden Abschnitten beschrieben. Die Rangfolge und Assoziativität der unsicheren Operatoren wird von der Grammatik impliziert.

### <a name="pointer-indirection"></a>Zeigerdereferenzierung

Ein *Pointer_indirection_expression* besteht aus einem Sternchen (`*`) gefolgt von einem *Unary_expression*.

```antlr
pointer_indirection_expression
    : '*' unary_expression
    ;
```

Der unäre `*` Operator Zeigerdereferenzierung bezeichnet und wird verwendet, um die Variable zu ermitteln, zu dem ein Zeiger zeigt. Das Ergebnis der Auswertung `*P`, wobei `P` ist ein Ausdruck eines Zeigertyps `T*`, eine Variable vom Typ `T`. Es ist ein Fehler während der Kompilierung der unäre anzuwendende `*` Operator, um ein Ausdruck vom Typ `void*` oder auf einen Ausdruck, der einen Zeigertyp ist.

Der Effekt der Anwendung der unäre `*` Operator, um eine `null` Zeiger wird durch die Implementierung definiert. Insbesondere besteht keine Garantie, die dieser Vorgang löst einen `System.NullReferenceException`.

Wenn ein ungültiger Wert auf den Zeiger, die das Verhalten der unären zugewiesen wurde `*` Operator nicht definiert ist. Auf die ungültige Werte für einen Zeiger dereferenziert, indem Sie den unären `*` Operator werden eine Adresse, die nicht ordnungsgemäß ausgerichtet werden, für der Typ, zeigt (siehe Beispiel in [zeigerkonvertierungen](unsafe-code.md#pointer-conversions)), und die Adresse einer Variablen nach der Ende ihrer Lebensdauer.

Für Zwecke der definite Assignment Analyse, eine Variablen, die durch die Auswertung eines Ausdrucks des Formulars `*P` gilt als ursprünglich zugewiesen ([anfänglich zugewiesene Variablen](variables.md#initially-assigned-variables)).

### <a name="pointer-member-access"></a>Zeigermemberzugriff

Ein *Pointer_member_access* besteht aus einer *Primary_expression*, gefolgt von einer "`->`" token, gefolgt von einer *Bezeichner* und eine optionale *Type_argument_list*.

```antlr
pointer_member_access
    : primary_expression '->' identifier
    ;
```

In einem Zeigermemberzugriff des Formulars `P->I`, `P` muss ein Ausdruck eines Zeigertyps außer `void*`, und `I` müssen einen verfügbaren Member des Typs, der dem kennzeichnen `P` Punkte.

Ein Zeigermemberzugriff des Formulars `P->I` wird ausgewertet, genau wie `(*P).I`. Eine Beschreibung der Zeiger-Dereferenzierungsoperator (`*`), finden Sie unter [Zeigerdereferenzierung](unsafe-code.md#pointer-indirection). Eine Beschreibung der den Memberzugriffsoperator (`.`), finden Sie unter [Memberzugriff](expressions.md#member-access).

Im Beispiel

```csharp
using System;

struct Point
{
    public int x;
    public int y;

    public override string ToString() {
        return "(" + x + "," + y + ")";
    }
}

class Test
{
    static void Main() {
        Point point;
        unsafe {
            Point* p = &point;
            p->x = 10;
            p->y = 20;
            Console.WriteLine(p->ToString());
        }
    }
}
```

die `->` Operator wird verwendet, um den Zugriff auf Felder und Aufrufen einer Methode, einer Struktur über einen Zeiger. Da der Vorgang `P->I` entspricht exakt dem `(*P).I`, `Main` Methode kann ebenso gut geschrieben wurden:

```csharp
class Test
{
    static void Main() {
        Point point;
        unsafe {
            Point* p = &point;
            (*p).x = 10;
            (*p).y = 20;
            Console.WriteLine((*p).ToString());
        }
    }
}
```

### <a name="pointer-element-access"></a>Zeigerelementzugriff auf

Ein *Pointer_element_access* besteht aus einem *Primary_no_array_creation_expression* gefolgt von einem Ausdruck, der eingeschlossen in "`[`"und"`]`".

```antlr
pointer_element_access
    : primary_no_array_creation_expression '[' expression ']'
    ;
```

In einem Zeigerelementzugriff des Formulars `P[E]`, `P` muss ein Ausdruck eines Zeigertyps außer `void*`, und `E` muss ein Ausdruck, der implizit in konvertiert werden kann `int`, `uint`, `long`, oder `ulong`.

Ein Zeiger auf der Form `P[E]` wird ausgewertet, genau wie `*(P + E)`. Eine Beschreibung der Zeiger-Dereferenzierungsoperator (`*`), finden Sie unter [Zeigerdereferenzierung](unsafe-code.md#pointer-indirection). Eine Beschreibung des Zeigeradditionsoperators (`+`), finden Sie unter [Zeigerarithmetik](unsafe-code.md#pointer-arithmetic).

Im Beispiel

```csharp
class Test
{
    static void Main() {
        unsafe {
            char* p = stackalloc char[256];
            for (int i = 0; i < 256; i++) p[i] = (char)i;
        }
    }
}
```

Ein Zeiger auf wird verwendet, um den Zeichenpuffer in zu initialisieren. eine `for` Schleife. Da der Vorgang `P[E]` entspricht exakt dem `*(P + E)`, im Beispiel kann ebenso gut geschrieben wurden:

```csharp
class Test
{
    static void Main() {
        unsafe {
            char* p = stackalloc char[256];
            for (int i = 0; i < 256; i++) *(p + i) = (char)i;
        }
    }
}
```

Der Zeiger Element Access-Operator nicht außerhalb des gültigen Bereichs nach Fehlern und das Verhalten beim Zugriff auf ein Element außerhalb des gültigen Bereichs ist nicht definiert. Dies ist identisch mit C- und C++.

### <a name="the-address-of-operator"></a>Der Address-of-operator

Ein *Addressof_expression* besteht aus einem kaufmännischen und-Zeichen (`&`) gefolgt von einem *Unary_expression*.

```antlr
addressof_expression
    : '&' unary_expression
    ;
```

Erhält einen Ausdruck `E` die ist ein Typ `T` und wird als eine feste Variable klassifiziert ([für feste und verschiebbare Variablen](unsafe-code.md#fixed-and-moveable-variables)), das Konstrukt `&E` berechnet die Adresse der Variablen vom `E`. Der Typ des Ergebnisses ist `T*` und wird als Wert klassifiziert. Ein Fehler während der Kompilierung tritt auf, wenn `E` wird nicht als Variable klassifiziert, wenn `E` wird als schreibgeschützte lokale Variable klassifiziert oder, wenn `E` eine bewegliche Variable bezeichnet. Im letzten Fall einer fixed-Anweisung ([die fixed-Anweisung](unsafe-code.md#the-fixed-statement)) kann verwendet werden, um vorübergehend "die Variable beheben", bevor Sie ihre Adresse abzurufen. Wie in der angegeben [Memberzugriff](expressions.md#member-access), außerhalb eines Instanzkonstruktors oder einen statischen Konstruktor für eine Struktur oder Klasse, die definiert eine `readonly` Feld, in dieses Feld gilt einen Wert, nicht auf eine Variable. Daher kann nicht die Adresse nicht ausgeführt werden. Die Adresse einer Konstanten können auf ähnliche Weise kann nicht ausgeführt werden.

Die `&` Operator erfordert nicht das Argument in der definitiv zugewiesen werden, jedoch folgende eine `&` -Operation, die Variable, die auf die der Operator angewendet wird gilt als definitiv zugewiesen, in den Ausführungspfad, in dem der Vorgang auftritt. Es handelt sich um die Verantwortung des Programmierers sicherzustellen, dass die richtige Initialisierung der Variable tatsächlich Stelle in diesem Fall übernimmt.

Im Beispiel

```csharp
using System;

class Test
{
    static void Main() {
        int i;
        unsafe {
            int* p = &i;
            *p = 123;
        }
        Console.WriteLine(i);
    }
}
```

`i` gilt als definitiv zugewiesen, nach der `&i` Vorgang, der zum Initialisieren verwendet `p`. Die Zuweisung zu `*p` faktisch initialisiert `i`, aber der Einbindung dieser Initialisierung liegt in der Verantwortung des Programmierers und keine Kompilierungsfehler tritt auf, wenn die Zuweisung entfernt wurde.

Die Regeln für definitive Zuweisungen für die `&` Operator vorhanden sein, dass redundante Initialisierung von lokalen Variablen kann vermieden werden. Viele externe APIs werden z. B. einen Zeiger auf eine Struktur, die von der API angegeben ist. Aufrufe dieser APIs in der Regel übergeben Sie die Adresse einer Struktur von lokalen Variablen und ohne die Regel, redundante Initialisierung der Strukturvariable wären erforderlich.

### <a name="pointer-increment-and-decrement"></a>Zeiger Inkrementieren und Dekrementieren

In einem unsicheren Kontext der `++` und `--` Operatoren ([Postfix-Inkrement und Dekrement-Operatoren](expressions.md#postfix-increment-and-decrement-operators) und [Präfix-Inkrement und Dekrement-Operatoren](expressions.md#prefix-increment-and-decrement-operators)) können auf Zeiger angewendet werden Variablen aller Typen mit Ausnahme von `void*`. Daher ist es bei jedem Zeigertyp `T*`, die folgenden Operatoren werden implizit definiert:

```csharp
T* operator ++(T* x);
T* operator --(T* x);
```

Die Operatoren erzeugen die gleichen Ergebnisse wie `x + 1` und `x - 1`bzw. ([Zeigerarithmetik](unsafe-code.md#pointer-arithmetic)). In anderen Worten: für eine Zeigervariable des Typs `T*`, `++` -Operator fügt hinzu, `sizeof(T)` an die Adresse, die in der Variablen enthalten und die `--` Operator subtrahiert `sizeof(T)` von der Adresse, die in der Variablen enthalten.

Wenn Sie eine Zeiger-Inkrement oder Dekrement Operation die Domänengrenzen des Zeigertyps, das Ergebnis ist die Implementierung definiert, aber keine Ausnahmen erstellt werden.

### <a name="pointer-arithmetic"></a>Zeigerarithmetik

In einem unsicheren Kontext der `+` und `-` Operatoren ([Additionsoperator](expressions.md#addition-operator) und [Subtraktionsoperator](expressions.md#subtraction-operator)) auf Werte mit Ausnahme von alle Zeigertypen angewendet werden können `void*`. Daher ist es bei jedem Zeigertyp `T*`, die folgenden Operatoren werden implizit definiert:

```csharp
T* operator +(T* x, int y);
T* operator +(T* x, uint y);
T* operator +(T* x, long y);
T* operator +(T* x, ulong y);

T* operator +(int x, T* y);
T* operator +(uint x, T* y);
T* operator +(long x, T* y);
T* operator +(ulong x, T* y);

T* operator -(T* x, int y);
T* operator -(T* x, uint y);
T* operator -(T* x, long y);
T* operator -(T* x, ulong y);

long operator -(T* x, T* y);
```

Erhält einen Ausdruck `P` eines Zeigertyps `T*` und einen Ausdruck `N` des Typs `int`, `uint`, `long`, oder `ulong`, die Ausdrücke `P + N` und `N + P` Berechnen der Zeigerwert des Typs `T*` , die sich aus der Addition ergibt `N * sizeof(T)` an die Adresse, die vom `P`. Entsprechend dem Ausdruck `P - N` berechnet den Wert des Zeigers des Typs `T*` , die sich aus der Subtraktion ergibt `N * sizeof(T)` von der Adresse, die vom `P`.

Mit den Ausdrücken `P` und `Q`, eines Zeigertyps `T*`, den Ausdruck `P - Q` berechnet die Differenz zwischen den Adressen, die vom `P` und `Q` und klicken Sie dann diesen Unterschied von `sizeof(T)`. Der Typ des Ergebnisses ist immer `long`. Tatsächlich `P - Q` wird berechnet als `((long)(P) - (long)(Q)) / sizeof(T)`.

Zum Beispiel:

```csharp
using System;

class Test
{
    static void Main() {
        unsafe {
            int* values = stackalloc int[20];
            int* p = &values[1];
            int* q = &values[15];
            Console.WriteLine("p - q = {0}", p - q);
            Console.WriteLine("q - p = {0}", q - p);
        }
    }
}
```

die Ausgabe erzeugt:

```
p - q = -14
q - p = 14
```

Wenn eine arithmetische Operation der Zeiger auf die Domäne des Zeigertyps überläuft, das Ergebnis wird auf eine Weise implementierungsdefinierte abgeschnitten, aber keine Ausnahmen erstellt werden.

### <a name="pointer-comparison"></a>Zeigervergleich

In einem unsicheren Kontext der `==`, `!=`, `<`, `>`, `<=`, und `=>` Operatoren ([Relational und Typtest Operatoren](expressions.md#relational-and-type-testing-operators)) kann auf alle Werte angewendet werden Zeigertypen. Die Vergleichsoperatoren für Zeiger sind:

```csharp
bool operator ==(void* x, void* y);
bool operator !=(void* x, void* y);
bool operator <(void* x, void* y);
bool operator >(void* x, void* y);
bool operator <=(void* x, void* y);
bool operator >=(void* x, void* y);
```

Da eine implizite Konvertierung in einen Zeigertyp, vorhanden ist. die `void*` Typ Operanden vom Zeigertyp mithilfe dieser Operatoren verglichen werden kann. Die Vergleichsoperatoren vergleichen die Adressen, die durch die beiden Operanden angegeben wird, als wären sie ganze Zahlen ohne Vorzeichen.

### <a name="the-sizeof-operator"></a>Der Operator sizeof

Die `sizeof` -Operator gibt die Anzahl der Bytes, die durch eine Variable eines bestimmten Typs belegt wird. Der Typ als Operand für `sizeof` muss ein *Unmanaged_type* ([Zeigertypen](unsafe-code.md#pointer-types)).

```antlr
sizeof_expression
    : 'sizeof' '(' unmanaged_type ')'
    ;
```

Das Ergebnis der `sizeof` Operator ist ein Wert vom Typ `int`. Für bestimmte vordefinierte Typen, die `sizeof` -Operator liefert einen konstanten Wert an, wie in der folgenden Tabelle gezeigt.


| __Expression (Ausdruck)__   | __Ergebnis__ |
|------------------|------------|
| `sizeof(sbyte)`  | `1`        |
| `sizeof(byte)`   | `1`        |
| `sizeof(short)`  | `2`        |
| `sizeof(ushort)` | `2`        |
| `sizeof(int)`    | `4`        |
| `sizeof(uint)`   | `4`        |
| `sizeof(long)`   | `8`        |
| `sizeof(ulong)`  | `8`        |
| `sizeof(char)`   | `2`        |
| `sizeof(float)`  | `4`        |
| `sizeof(double)` | `8`        |
| `sizeof(bool)`   | `1`        |

Für alle anderen Dateitypen, das Ergebnis der `sizeof` Operator wird durch die Implementierung definiert und wird als Wert und kein Konstante klassifiziert.

Die Reihenfolge, in der Elemente in einer Struktur gepackt werden, ist nicht angegeben.

Für die Ausrichtung möglicherweise benannte Füllzeichen am Anfang einer Struktur, in eine Struktur, und am Ende der Struktur. Der Inhalt der Bits, die als Abstand verwendet ist unbestimmt.

Wenn auf einen Operanden angewendet, die Struktur verfügt, ist das Ergebnis die Gesamtzahl der Bytes in einer Variablen des Typs, einschließlich der Abstände an.

## <a name="the-fixed-statement"></a>Die fixed-Anweisung

In einem unsicheren Kontext der *Embedded_statement* ([Anweisungen](statements.md)) Produktion ermöglicht ein zusätzliches Konstrukt, das `fixed` -Anweisung, die verwendet wird, um "eine bewegliche Variable korrigieren" so, dass die Adresse bleibt unverändert, für die Dauer der Anweisung.

```antlr
fixed_statement
    : 'fixed' '(' pointer_type fixed_pointer_declarators ')' embedded_statement
    ;

fixed_pointer_declarators
    : fixed_pointer_declarator (','  fixed_pointer_declarator)*
    ;

fixed_pointer_declarator
    : identifier '=' fixed_pointer_initializer
    ;

fixed_pointer_initializer
    : '&' variable_reference
    | expression
    ;
```

Jede *Fixed_pointer_declarator* deklariert eine lokale Variable mit der angegebenen *Pointer_type* und initialisiert die lokale Variable mit der Adresse berechnet, indem Sie die entsprechende *Fixed_ Pointer_initializer*. Eine lokale Variable deklariert, die eine `fixed` -Anweisung ist in jedem zugänglich *Fixed_pointer_initializer*s auf der rechten Seite der Deklaration dieser Variablen die, und in der *Embedded_statement* von der `fixed` Anweisung. Eine lokale Variable deklariert, indem eine `fixed` Anweisung ist schreibgeschützt. Ein Fehler während der Kompilierung tritt auf, wenn die eingebettete Anweisung versucht, diese lokale Variable zu ändern (per Zuweisung oder `++` und `--` Operatoren) oder übergeben Sie sie als eine `ref` oder `out` Parameter.

Ein *Fixed_pointer_initializer* kann einen der folgenden sein:

*  Das Token "`&`" gefolgt von einem *Variable_reference* ([präzise Regeln für definitive Zuweisungen bestimmen](variables.md#precise-rules-for-determining-definite-assignment)) auf eine bewegliche Variable ([für feste und verschiebbare Variablen](unsafe-code.md#fixed-and-moveable-variables)) eines nicht verwalteten Typs `T`, der Typ `T*` wird implizit in den Zeigertyp den `fixed` Anweisung. In diesem Fall berechnet die Initialisierung der Adresse der angegebenen Variablen und die Variable ist garantiert an fester Adresse für die Dauer der `fixed` Anweisung.
*  Ein Ausdruck, der eine *Array_type* mit Elementen eines nicht verwalteten Typs `T`, der Typ `T*` wird implizit in den Zeigertyp die `fixed` Anweisung. In diesem Fall berechnet die Initialisierung der Adresse des ersten Elements im Array und das gesamte Array ist garantiert an fester Adresse für die Dauer der `fixed` Anweisung. Wenn der Ausdruck null ist oder wenn das Array 0 (null) Elemente verfügt, berechnet der Initialisierer einer Adresse gleich 0 (null).
*  Ein Ausdruck vom Typ `string`, der Typ `char*` wird implizit in den Zeigertyp den `fixed` Anweisung. In diesem Fall berechnet die Adresse des ersten Zeichens in der Zeichenfolge für die Initialisierung und die gesamte Zeichenfolge ist garantiert an fester Adresse für die Dauer der `fixed` Anweisung. Das Verhalten der `fixed` Anweisung ist Implementierung definiert, wenn der Ausdruck null ist.
*  Ein *Simple_name* oder *Member_access* , die verweist auf einen Member fester Größe Puffer eine bewegliche Variable, sofern der Typ des Members Puffer fester Größe implizit in den Zeigertyp ist. in der `fixed` Anweisung. In diesem Fall berechnet die Initialisierung für einen Zeiger auf das erste Element des Puffers fester Größe ([Puffer fester Größe in Ausdrücken](unsafe-code.md#fixed-size-buffers-in-expressions)), und der Puffer mit fester Größe ist garantiert an fester Adresse für die Dauer der `fixed`Anweisung.

Für jede Adresse berechnet, indem eine *Fixed_pointer_initializer* der `fixed` Anweisung wird sichergestellt, dass die Variable verwiesen wird, durch die Adresse nicht verschoben oder verworfen, die vom Garbage Collector für die Dauer der `fixed` Anweisung. Wenn die Adresse berechnet, indem Sie z. B. eine *Fixed_pointer_initializer* verweist auf ein Feld eines Objekts oder ein Element einer Instanz des Arrays, die `fixed` -Anweisung wird sichergestellt, dass die enthaltende Objektinstanz nicht verschoben wird oder während der Lebensdauer der Anweisung verworfen.

Es ist der Programmierer dafür verantwortlich, um sicherzustellen, dass der Zeiger von erstellt `fixed` Anweisungen bleiben nach Ausführung dieser Anweisungen nicht. Z. B. Zeiger der Erstellung von `fixed` Anweisungen an externen APIs übergeben werden, es ist der Programmierer dafür verantwortlich, um sicherzustellen, dass die APIs keinen Speicher für diese Zeiger beibehalten.

Feste Objekte möglicherweise die Fragmentierung des Heaps (da sie nicht verschoben werden können). Aus diesem Grund sollten Objekte nur wenn unbedingt nötig behoben werden und dann nur für kürzestmöglicher Zeit möglich.

Im Beispiel

```csharp
class Test
{
    static int x;
    int y;

    unsafe static void F(int* p) {
        *p = 1;
    }

    static void Main() {
        Test t = new Test();
        int[] a = new int[10];
        unsafe {
            fixed (int* p = &x) F(p);
            fixed (int* p = &t.y) F(p);
            fixed (int* p = &a[0]) F(p);
            fixed (int* p = a) F(p);
        }
    }
}
```

veranschaulicht verschiedene Verwendungen von der `fixed` Anweisung. Die erste Anweisung korrigiert und ruft die Adresse eines statischen Felds ab, die zweite Anweisung korrigiert und die Adresse eines Instanzenfelds und die dritte Anweisung korrigiert und ruft die Adresse eines Arrayelements ab. In jedem Fall hätte ein Fehler mit der normalen `&` Operator, da die Variablen als bewegliche Variablen klassifiziert werden.

Der vierte `fixed` -Anweisung im obigen Beispiel erzeugt ein ähnliches Ergebnis an den dritten.

Dieses Beispiel die `fixed` -Anweisung verwendet `string`:

```csharp
class Test
{
    static string name = "xx";

    unsafe static void F(char* p) {
        for (int i = 0; p[i] != '\0'; ++i)
            Console.WriteLine(p[i]);
    }

    static void Main() {
        unsafe {
            fixed (char* p = name) F(p);
            fixed (char* p = "xx") F(p);
        }
    }
}
```

In einem unsicheren Kontext werden Elemente des Arrays von eindimensionalen Arrays in aufsteigender Indexreihenfolge, beginnend mit dem Index gespeichert `0` und endend mit Index `Length - 1`. Für mehrdimensionale Arrays, Arrays, die Elemente gespeichert werden, dass die Indizes von die Dimension ganz rechts zunächst erhöht werden dann von der nächsten links Dimension, und so weiter auf der linken Seite. Innerhalb einer `fixed` -Anweisung, die einen Zeiger erhält `p` auf eine Arrayinstanz `a`, die Zeigerwerte von `p` zu `p + a.Length - 1` Adressen der Elemente im Array darstellen. Ebenso die Variablen, die im Bereich von `p[0]` zu `p[a.Length - 1]` die tatsächlichen Arrayelemente darstellen. Wenn die Möglichkeit, die in der Arrays gespeichert sind, können wir ein Array von einer beliebigen Dimension, behandeln, als wäre es linear.

Zum Beispiel:

```csharp
using System;

class Test
{
    static void Main() {
        int[,,] a = new int[2,3,4];
        unsafe {
            fixed (int* p = a) {
                for (int i = 0; i < a.Length; ++i)    // treat as linear
                    p[i] = i;
            }
        }

        for (int i = 0; i < 2; ++i)
            for (int j = 0; j < 3; ++j) {
                for (int k = 0; k < 4; ++k)
                    Console.Write("[{0},{1},{2}] = {3,2} ", i, j, k, a[i,j,k]);
                Console.WriteLine();
            }
    }
}
```

die Ausgabe erzeugt:

```
[0,0,0] =  0 [0,0,1] =  1 [0,0,2] =  2 [0,0,3] =  3
[0,1,0] =  4 [0,1,1] =  5 [0,1,2] =  6 [0,1,3] =  7
[0,2,0] =  8 [0,2,1] =  9 [0,2,2] = 10 [0,2,3] = 11
[1,0,0] = 12 [1,0,1] = 13 [1,0,2] = 14 [1,0,3] = 15
[1,1,0] = 16 [1,1,1] = 17 [1,1,2] = 18 [1,1,3] = 19
[1,2,0] = 20 [1,2,1] = 21 [1,2,2] = 22 [1,2,3] = 23
```

Im Beispiel

```csharp
class Test
{
    unsafe static void Fill(int* p, int count, int value) {
        for (; count != 0; count--) *p++ = value;
    }

    static void Main() {
        int[] a = new int[100];
        unsafe {
            fixed (int* p = a) Fill(p, 100, -1);
        }
    }
}
```

eine `fixed` -Anweisung verwendet, um ein Array zu beheben, damit seine Adresse an eine Methode übergeben werden kann, die einen Zeiger akzeptiert.

Im folgenden Beispiel

```csharp
unsafe struct Font
{
    public int size;
    public fixed char name[32];
}

class Test
{
    unsafe static void PutString(string s, char* buffer, int bufSize) {
        int len = s.Length;
        if (len > bufSize) len = bufSize;
        for (int i = 0; i < len; i++) buffer[i] = s[i];
        for (int i = len; i < bufSize; i++) buffer[i] = (char)0;
    }

    Font f;

    unsafe static void Main()
    {
        Test test = new Test();
        test.f.size = 10;
        fixed (char* p = test.f.name) {
            PutString("Times New Roman", p, 32);
        }
    }
}
```

eine fixed-Anweisung wird verwendet, um Puffers mit fester Größe einer Struktur zu beheben, damit seine Adresse als Zeiger verwendet werden kann.

Ein `char*` Wert erzeugt, indem Sie die Fixierung einer Zeichenfolgeninstanz immer zeigt auf eine Null-terminierte Zeichenfolge. In einer fixed-Anweisung, die einen Zeiger erhält `p` zu einer Zeichenfolgeninstanz `s`, die Zeigerwerte von `p` zu `p + s.Length - 1` Adressen Zeichen in der Zeichenfolge und der Zeigerwert darstellen `p + s.Length` immer verweist auf ein Null-Zeichen (das Zeichen, mit dem Wert `'\0'`).

Ändern Objekte vom verwalteten Typ über feste Zeiger können die Ergebnisse in einem nicht definierten Verhalten. Da Zeichenfolgen unveränderlich sind, ist es z. B. der Programmierer dafür verantwortlich, um sicherzustellen, dass die Zeichen, die auf die verwiesen wird durch einen Zeiger auf eine feste Zeichenfolge nicht geändert werden.

Die automatischen Null-Terminierung von Zeichenfolgen ist besonders praktisch, wenn es sich bei externen APIs aufrufen, die voraussichtlich von Zeichenfolgen im "C-Stil". Beachten Sie jedoch, dass eine Zeichenfolgeninstanz zulässig ist, Null-Zeichen enthalten. Wenn diese Null-Zeichen vorhanden sind, die Zeichenfolge wird angezeigt, abgeschnittene als eine Null-terminierte behandelt `char*`.

## <a name="fixed-size-buffers"></a>Puffer fester Größe

Puffer fester Größe werden verwendet, um "C-Style" Inline-Arrays als Member von Strukturen deklariert werden, und eignen sich in erster Linie für die Interaktion mit nicht verwalteten APIs.

### <a name="fixed-size-buffer-declarations"></a>Puffer fester Größe-Deklarationen

Ein ***fester Größe Puffer*** angehört, der Speicher für einen Puffer fester Länge mit einer Variablen eines bestimmten Typs darstellt. Puffer fester Größe Deklaration führt eine oder mehrere Puffer mit fester Größe eines bestimmten Elementtyps. Puffer fester Größe dürfen nur in Strukturdeklarationen von und kann nur in nicht sicheren Kontexten auftreten ([nicht sicheren Kontexten](unsafe-code.md#unsafe-contexts)).

```antlr
struct_member_declaration_unsafe
    : fixed_size_buffer_declaration
    ;

fixed_size_buffer_declaration
    : attributes? fixed_size_buffer_modifier* 'fixed' buffer_element_type fixed_size_buffer_declarator+ ';'
    ;

fixed_size_buffer_modifier
    : 'new'
    | 'public'
    | 'protected'
    | 'internal'
    | 'private'
    | 'unsafe'
    ;

buffer_element_type
    : type
    ;

fixed_size_buffer_declarator
    : identifier '[' constant_expression ']'
    ;
```

Deklaration Puffer fester Größe kann einen Satz von Attributen enthalten ([Attribute](attributes.md)), ein `new` Modifizierer ([Modifizierer](classes.md#modifiers)), eine gültige Kombination der vier Zugriffsmodifizierer ([Typ Parameter und Einschränkungen](classes.md#type-parameters-and-constraints)) und ein `unsafe` Modifizierer ([nicht sicheren Kontexten](unsafe-code.md#unsafe-contexts)). Die Attribute und Modifizierer gelten für alle Member, die der Puffer fester Größe-Deklaration deklariert. Es ist ein Fehler für den gleichen Modifizierer für mehrere Male in einer Puffer fester Größe angezeigt werden.

Deklaration Puffer fester Größe ist nicht zulässig, sollen die `static` Modifizierer.

Der Puffer-Elementtyp, der eine feste Größe Puffer Deklaration gibt an, den Typ des Elements, der die Puffer durch die Deklaration eingeführt wurden. Der Elementtyp der Puffer muss eine der vordefinierten Typen `sbyte`, `byte`, `short`, `ushort`, `int`, `uint`, `long`, `ulong`, `char`, `float`, `double`, oder `bool`.

Der Puffer-Elementtyp folgt eine Liste von Deklaratoren verwenden Puffer fester Größe, von denen jede einen neuen Member einführen. Puffer fester Größe Deklaration besteht aus einem Bezeichner, der den Elementnamen gefolgt von einem konstanten Ausdruck, der eingeschlossen in `[` und `]` Token. Der Konstante Ausdruck gibt an, die Anzahl der Elemente in das Element, das durch diese feste Größe Puffer Deklarator eingeführt wird. Der Typ des konstanten Ausdrucks muss implizit in den Typ `int`, und der Wert muss eine ganze Zahl ungleich NULL sein.

Die Elemente des Puffers mit fester Größe werden garantiert sequenziell im Arbeitsspeicher angeordnet werden.

Erklärung Puffer fester Größe, die mehrere Puffer mit fester Größe deklariert entspricht mehreren Deklarationen für eine Deklaration einer Puffer fester Größe mit denselben Attributen und Elementtypen. Beispiel:

```csharp
unsafe struct A
{
   public fixed int x[5], y[10], z[100];
}
```

für die folgende Syntax:

```csharp
unsafe struct A
{
   public fixed int x[5];
   public fixed int y[10];
   public fixed int z[100];
}
```

### <a name="fixed-size-buffers-in-expressions"></a>Puffer fester Größe in Ausdrücken

Suche nach Membern ([Operatoren](expressions.md#operators)) eine feste Größe Puffer Member wird fortgesetzt, genau wie die Suche nach Membern eines Felds.

Puffers mit fester Größe kann verwiesen werden, in einem Ausdruck mit einer *Simple_name* ([Typrückschluss](expressions.md#type-inference)) oder ein *Member_access* ([Überprüfungen zur Kompilierzeit der Dynamische überladungsauflösung](expressions.md#compile-time-checking-of-dynamic-overload-resolution)).

Wenn Mitglied Puffer fester Größe als einen einfachen Namen verwiesen wird, der Effekt ist derselbe wie ein Memberzugriff des Formulars `this.I`, wobei `I` Members Puffer fester Größe ist.

In einem Memberzugriff des Formulars `E.I`, wenn `E` ist ein Strukturtyp und eine Suche nach Membern der `I` , Typ "Struktur" einen Member mit fester Größe, bezeichnet, die dann `E.I` ist eine klassifizierte wie folgt ausgewertet:

*  Wenn der Ausdruck `E.I` erfolgt nicht in einem unsicheren Kontext, ein Fehler während der Kompilierung auftritt.
*  Wenn `E` wird als Wert klassifiziert, ein Fehler während der Kompilierung auftritt.
*  Andernfalls gilt: Wenn `E` ist eine bewegliche Variable ([für feste und verschiebbare Variablen](unsafe-code.md#fixed-and-moveable-variables)) und der Ausdruck `E.I` keine *Fixed_pointer_initializer* ([festen Anweisung](unsafe-code.md#the-fixed-statement)), ein Fehler während der Kompilierung auftritt.
*  Andernfalls `E` verweist auf eine feste Variable und das Ergebnis des Ausdrucks ist ein Zeiger auf das erste Element des Elements Puffer fester Größe `I` in `E`. Das Ergebnis ist vom Typ `S*`, wobei `S` ist der Elementtyp der `I`, und wird als Wert klassifiziert.

Die nachfolgenden Elemente des Puffers fester Größe können mithilfe von Zeigeroperationen vom ersten Element zugegriffen werden. Im Gegensatz zu den Zugriff auf Arrays Zugriff auf die Elemente eines Puffers mit fester Größe ist ein unsicherer Vorgang und ist kein Bereich überprüft.

Das folgende Beispiel deklariert und verwendet eine Struktur mit einem Member Puffer mit fester Größe.

```csharp
unsafe struct Font
{
    public int size;
    public fixed char name[32];
}

class Test
{
    unsafe static void PutString(string s, char* buffer, int bufSize) {
        int len = s.Length;
        if (len > bufSize) len = bufSize;
        for (int i = 0; i < len; i++) buffer[i] = s[i];
        for (int i = len; i < bufSize; i++) buffer[i] = (char)0;
    }

    unsafe static void Main()
    {
        Font f;
        f.size = 10;
        PutString("Times New Roman", f.name, 32);
    }
}
```

### <a name="definite-assignment-checking"></a>Definitive Zuweisung überprüfen

Puffer fester Größe werden nicht definitive Zuweisung überprüfen ([definitive Zuweisung](variables.md#definite-assignment)), und fester Größe Puffer Elemente werden ignoriert, zum Zweck der definitive Zuweisung, die Überprüfung der Struktur Typvariablen.

Wenn die äußersten enthaltenden Strukturvariable eines Elements Puffer fester Größe eine statische Variable, eine Instanzvariable für eine Instanz der Klasse oder eines Arrayelements ist, werden die Elemente des Puffers fester Größe automatisch auf ihre Standardwerte initialisiert ([Standardwerte](variables.md#default-values)). In allen anderen Fällen ist der ursprüngliche Inhalt des Puffers mit fester Größe nicht definiert.

## <a name="stack-allocation"></a>Stapelreservierung

In einem unsicheren Kontext, einer lokalen Variablendeklaration ([lokale Variablendeklarationen](statements.md#local-variable-declarations)) kann einen Stack Allocation-Initialisierer dem Speicher zugeordnet werden, in der Aufrufliste enthalten.

```antlr
local_variable_initializer_unsafe
    : stackalloc_initializer
    ;

stackalloc_initializer
    : 'stackalloc' unmanaged_type '[' expression ']'
    ;
```

Die *Unmanaged_type* gibt den Typ der Elemente, die in den neu zugewiesenen Standort gespeichert werden und die *Ausdruck* gibt die Anzahl dieser Elemente. Zusammen geben diese die erforderlichen Zuordnungsgröße. Da die Größe des Stack-gesamtzuordnung darf nicht negativ sein, es ist ein Fehler während der Kompilierung an die Anzahl der Elemente als eine *Constant_expression* , die auf einen negativen Wert ausgewertet wird.

Ein Stack Allocation-Initialisierer des Formulars `stackalloc T[E]` erfordert `T` einen nicht verwalteten Typ sein ([Zeigertypen](unsafe-code.md#pointer-types)) und `E` als Ausdruck vom Typ `int`. Das Konstrukt weist `E * sizeof(T)` Bytes aus dem Aufruf Stapel und gibt einen Zeiger vom Typ `T*`, auf den neu belegten Block. Wenn `E` ein negativer Wert ist, und klicken Sie dann das Verhalten nicht definiert ist. Wenn `E` ist 0 (null), und klicken Sie dann keine Zuordnung erfolgt, und des zurückgegebene Zeigers Implementierung definiert. Wenn nicht genügend Arbeitsspeicher verfügbar, um einen Block, der angegebenen Größe ein `System.StackOverflowException` ausgelöst.

Der Inhalt des neu belegten Speichers ist nicht definiert.

Stack Allocation-Initialisierer sind nicht zulässig `catch` oder `finally` Blöcke ([der Try-Anweisung](statements.md#the-try-statement)).

Es gibt keine Möglichkeit, explizit freizugeben, speicherbelegung, die mit `stackalloc`. Alle stapelzugeordneten Speicherblöcke, die während der Ausführung ein Funktionsmember erstellt werden automatisch verworfen werden, wenn diese Funktionselement zurückgibt. Dies entspricht der `alloca` -Funktion, eine Erweiterung, die häufig in C und C++-Implementierungen gefunden.

Im Beispiel

```csharp
using System;

class Test
{
    static string IntToString(int value) {
        int n = value >= 0? value: -value;
        unsafe {
            char* buffer = stackalloc char[16];
            char* p = buffer + 16;
            do {
                *--p = (char)(n % 10 + '0');
                n /= 10;
            } while (n != 0);
            if (value < 0) *--p = '-';
            return new string(p, 0, (int)(buffer + 16 - p));
        }
    }

    static void Main() {
        Console.WriteLine(IntToString(12345));
        Console.WriteLine(IntToString(-999));
    }
}
```

eine `stackalloc` Initialisierer werden in der `IntToString` Methode, um einen Puffer von 16 Zeichen auf dem Stapel zu reservieren. Der Puffer wird automatisch verworfen, wenn die Methode zurückgegeben.

## <a name="dynamic-memory-allocation"></a>Dynamische speicherbelegung

Mit Ausnahme der `stackalloc` Operator c# bietet keine vordefinierten Konstrukte für die Verwaltung von erfassten nicht-Garbage-Speicher. Diese Dienste werden in der Regel durch die Unterstützung von Klassenbibliotheken bereitgestellt oder direkt aus dem zugrunde liegenden Betriebssystem importiert. Z. B. die `Memory` nachstehenden Klasse veranschaulicht, wie die Heapfunktionen eines zugrunde liegenden Betriebssystems aus c# zugegriffen werden können:

```csharp
using System;
using System.Runtime.InteropServices;

public unsafe class Memory
{
    // Handle for the process heap. This handle is used in all calls to the
    // HeapXXX APIs in the methods below.
    static int ph = GetProcessHeap();

    // Private instance constructor to prevent instantiation.
    private Memory() {}

    // Allocates a memory block of the given size. The allocated memory is
    // automatically initialized to zero.
    public static void* Alloc(int size) {
        void* result = HeapAlloc(ph, HEAP_ZERO_MEMORY, size);
        if (result == null) throw new OutOfMemoryException();
        return result;
    }

    // Copies count bytes from src to dst. The source and destination
    // blocks are permitted to overlap.
    public static void Copy(void* src, void* dst, int count) {
        byte* ps = (byte*)src;
        byte* pd = (byte*)dst;
        if (ps > pd) {
            for (; count != 0; count--) *pd++ = *ps++;
        }
        else if (ps < pd) {
            for (ps += count, pd += count; count != 0; count--) *--pd = *--ps;
        }
    }

    // Frees a memory block.
    public static void Free(void* block) {
        if (!HeapFree(ph, 0, block)) throw new InvalidOperationException();
    }

    // Re-allocates a memory block. If the reallocation request is for a
    // larger size, the additional region of memory is automatically
    // initialized to zero.
    public static void* ReAlloc(void* block, int size) {
        void* result = HeapReAlloc(ph, HEAP_ZERO_MEMORY, block, size);
        if (result == null) throw new OutOfMemoryException();
        return result;
    }

    // Returns the size of a memory block.
    public static int SizeOf(void* block) {
        int result = HeapSize(ph, 0, block);
        if (result == -1) throw new InvalidOperationException();
        return result;
    }

    // Heap API flags
    const int HEAP_ZERO_MEMORY = 0x00000008;

    // Heap API functions
    [DllImport("kernel32")]
    static extern int GetProcessHeap();

    [DllImport("kernel32")]
    static extern void* HeapAlloc(int hHeap, int flags, int size);

    [DllImport("kernel32")]
    static extern bool HeapFree(int hHeap, int flags, void* block);

    [DllImport("kernel32")]
    static extern void* HeapReAlloc(int hHeap, int flags, void* block, int size);

    [DllImport("kernel32")]
    static extern int HeapSize(int hHeap, int flags, void* block);
}
```

Ein Beispiel, verwendet der `Memory` Klasse sind unten angegeben:

```csharp
class Test
{
    static void Main() {
        unsafe {
            byte* buffer = (byte*)Memory.Alloc(256);
            try {
                for (int i = 0; i < 256; i++) buffer[i] = (byte)i;
                byte[] array = new byte[256];
                fixed (byte* p = array) Memory.Copy(buffer, p, 256); 
            }
            finally {
                Memory.Free(buffer);
            }
            for (int i = 0; i < 256; i++) Console.WriteLine(array[i]);
        }
    }
}
```

Das Beispiel ordnet 256 Bytes an Arbeitsspeicher über `Memory.Alloc` und initialisiert den Speicherblock mit Werten von 0 bis 255. Klicken Sie dann ein Element mit 256-Byte-Array zugeordnet und verwendet `Memory.Copy` um den Inhalt des Speicherblocks in der Byte-Array zu kopieren. Schließlich wird der Speicherblock mit freigegeben `Memory.Free` und der Inhalt des Byte-Arrays auf der Konsole ausgegeben.
