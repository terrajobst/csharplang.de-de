---
ms.openlocfilehash: 4faef9a12bdff54fa59a55a0206fa72bda4ea585
ms.sourcegitcommit: 892af9016b3317a8fae12d195014dc38ba51cf16
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/01/2019
ms.locfileid: "71704062"
---
# <a name="unsafe-code"></a>Unsicherer Code

Die in C# den vorstehenden Kapiteln definierte Kernsprache unterscheidet sich insbesondere von C und C++ durch das Weglassen von Zeigern als Datentyp. Stattdessen C# bietet Verweise und die Möglichkeit, Objekte zu erstellen, die von einem Garbage Collector verwaltet werden. Dieser Entwurf stellt C# in Verbindung mit anderen Features eine viel sicherere Sprache als C oder C++dar. In der Kern C# Sprache ist es einfach nicht möglich, eine nicht initialisierte Variable, einen "Verb leibend"-Zeiger oder einen Ausdruck zu haben, der ein Array über seine Begrenzungen hinaus indiziert. Alle Fehlerkategorien, die routinemäßig C und C++ Programme Plagen, werden daher vermieden.

Obwohl praktisch jedes Zeigertyp Konstrukt in C C++ oder ein Verweistyp in der C#Entsprechung in ist, gibt es jedoch Situationen, in denen der Zugriff auf Zeiger Typen zu einer Notwendigkeit wird. Beispielsweise ist die Schnittstellen mit dem zugrunde liegenden Betriebssystem, der Zugriff auf ein Speicher Abbild oder die Implementierung eines zeitkritischen Algorithmus möglicherweise ohne Zugriff auf Zeiger nicht möglich oder praktikabel. Um diese Anforderung zu erfüllen C# , bietet die Möglichkeit, ***unsicheren Code***zu schreiben.

Im unsicheren Code ist es möglich, Zeiger zu deklarieren und zu verarbeiten, Konvertierungen zwischen Zeigern und ganzzahligen Typen auszuführen, die Adresse von Variablen zu übernehmen usw. In gewisser Weise ist das Schreiben von unsicherem Code ähnlich wie das Schreiben C# von C-Code in einem Programm.

Unsicherer Code ist tatsächlich eine "sichere" Funktion aus der Perspektive von Entwicklern und Benutzern. Unsicherer Code muss eindeutig mit dem-Modifizierer `unsafe` gekennzeichnet werden, sodass Entwickler nicht möglicherweise unsichere Funktionen versehentlich verwenden können, und die Ausführungs-Engine kann sicherstellen, dass unsicherer Code nicht in einer nicht vertrauenswürdigen Umgebung ausgeführt werden kann.

## <a name="unsafe-contexts"></a>Unsichere Kontexte

Die unsicheren Funktionen von C# sind nur in unsicheren Kontexten verfügbar. Ein unsicherer Kontext wird durch das Einschließen eines `unsafe`-Modifizierers in die Deklaration eines Typs oder Members oder durch die Verwendung eines *unsafe_statement*-Elements eingeführt:

*  Eine Deklaration einer Klasse, Struktur, Schnittstelle oder eines Delegaten kann einen `unsafe`-Modifizierer enthalten. in diesem Fall wird der gesamte Text Block dieser Typdeklaration (einschließlich des Texts der Klasse, Struktur oder Schnittstelle) als unsicherer Kontext betrachtet.
*  Eine Deklaration eines Felds, einer Methode, einer Eigenschaft, eines Ereignisses, eines Indexers, eines Operators, eines Instanzkonstruktors, eines Dekonstruktors oder eines statischen Konstruktors kann einen `unsafe`-Modifizierer enthalten. in diesem Fall wird der gesamte Text Block der Element Deklaration als unsicherer Kontext betrachtet.
*  Ein *unsafe_statement* ermöglicht die Verwendung eines unsicheren Kontexts innerhalb eines- *Blocks*. Der gesamte Text Block des zugeordneten *Blocks* wird als unsicherer Kontext betrachtet.

Die zugehörigen Grammatik Produktionen sind unten dargestellt.

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

der in der Struktur Deklaration angegebene `unsafe`-Modifizierer bewirkt, dass der gesamte Text Block der Struktur Deklaration zu einem unsicheren Kontext wird. Daher ist es möglich, die Felder "`Left`" und "`Right`" als Zeigertyp zu deklarieren. Das obige Beispiel könnte auch geschrieben werden.

```csharp
public struct Node
{
    public int Value;
    public unsafe Node* Left;
    public unsafe Node* Right;
}
```

Hier bewirken die `unsafe`-Modifizierer in den Feld Deklarationen, dass diese Deklarationen als unsichere Kontexte angesehen werden.

Abgesehen von der Einrichtung eines unsicheren Kontexts, der die Verwendung von Zeiger Typen ermöglicht, hat der `unsafe`-Modifizierer keine Auswirkung auf einen Typ oder einen Member. Im Beispiel

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

der `unsafe`-Modifizierer für die `F`-Methode in `A` bewirkt, dass der Text Block von `F` zu einem unsicheren Kontext wird, in dem die unsicheren Funktionen der Sprache verwendet werden können. Bei der außer Kraft Setzung von `F` in `B` ist es nicht notwendig, den `unsafe`-Modifizierer erneut anzugeben, es sei denn, die `F`-Methode in `B` selbst benötigt Zugriff auf unsichere Features.

Die Situation ist etwas anders, wenn ein Zeigertyp Teil der Methoden Signatur ist.

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

Da die Signatur von `F` einen Zeigertyp enthält, kann Sie nur in einem unsicheren Kontext geschrieben werden. Allerdings kann der unsichere Kontext eingeführt werden, indem entweder die gesamte Klasse unsicher gemacht wird, wie es bei `A` der Fall ist, oder indem ein `unsafe`-Modifizierer in die Methoden Deklaration eingeschlossen wird, wie es in `B` der Fall ist.

## <a name="pointer-types"></a>Zeigertypen

In einem unsicheren Kontext kann ein *Typ* ([types](types.md)) ein *pointer_type* sowie ein *value_type* oder ein *reference_type*sein. Ein *pointer_type* kann jedoch auch in einem `typeof`-Ausdruck ([Anonyme Objekt Erstellungs Ausdrücke](expressions.md#anonymous-object-creation-expressions)) außerhalb eines unsicheren Kontexts verwendet werden, da diese Verwendung nicht unsicher ist.

```antlr
type_unsafe
    : pointer_type
    ;
```

Ein *pointer_type* -Wert wird als *unmanaged_type* oder das-Schlüsselwort `void`, gefolgt von einem `*`-Token geschrieben:

```antlr
pointer_type
    : unmanaged_type '*'
    | 'void' '*'
    ;

unmanaged_type
    : type
    ;
```

Der Typ, der vor dem `*` in einem Zeigertyp angegeben wird, wird als ***Verweistyp*** des Zeiger Typs bezeichnet. Sie stellt den Typ der Variablen dar, auf die ein Wert des Zeiger Typs zeigt.

Anders als Verweise (Werte von Verweis Typen) werden Zeiger nicht vom Garbage Collector nachverfolgt, und der Garbage Collector hat keine Informationen über Zeiger und die Daten, auf die Sie zeigen. Aus diesem Grund kann ein Zeiger nicht auf einen Verweis oder eine Struktur verweisen, die Verweise enthält, und der Verweistyp eines Zeigers muss ein *unmanaged_type*sein.

Ein *unmanaged_type* ist ein beliebiger Typ, der kein *reference_type* oder konstruierter Typ ist und keine *reference_type* -oder konstruierten Typfelder auf einer beliebigen Schachtelungs Ebene enthält. Mit anderen Worten: ein *unmanaged_type* -Wert ist einer der folgenden:

*  `sbyte`, `byte`, `short`, `ushort`, `int`, `uint`, `long`, `ulong`, `char`, `float`, 0, 1 oder 2.
*  Alle *enum_type*.
*  Alle *pointer_type*.
*  Alle benutzerdefinierten *struct_type* , bei denen es sich nicht um einen konstruierten Typ handelt und die nur die Felder *unmanaged_type*s enthalten.

Die intuitive Regel zum Mischen von Zeigern und verweisen ist, dass Verweise von verweisen (Objekten) Zeiger enthalten dürfen, aber Verweise von Zeigern dürfen keine Verweise enthalten.

In der folgenden Tabelle sind einige Beispiele für Zeiger Typen angegeben:

| __Beispiel__ | __Beschreibung__                               |
|-------------|-----------------------------------------------|
| `byte*`     | Zeiger auf `byte`                             |
| `char*`     | Zeiger auf `char`                             |
| `int**`     | Zeiger auf den Zeiger auf `int`                   |
| `int*[]`    | Eindimensionales Array von Zeigern auf `int` |
| `void*`     | Zeiger auf unbekannten Typ                       |

Für eine bestimmte Implementierung müssen alle Zeiger Typen die gleiche Größe und Darstellung aufweisen.

Im Gegensatz zu C++C und wird in C# der `*` zusammen mit dem zugrunde liegenden Typ und nicht als Präfix Satzzeichen für jeden Zeiger Namen geschrieben, wenn mehrere Zeiger in der gleichen Deklaration deklariert werden. Beispiel:

```csharp
int* pi, pj;    // NOT as int *pi, *pj;
```

Der Wert eines Zeigers mit dem Typ "`T*`" stellt die Adresse einer Variablen vom Typ "`T`" dar. Der Zeiger Dereferenzierungsoperator `*` ([Zeigerdereferenzierung](unsafe-code.md#pointer-indirection)) kann verwendet werden, um auf diese Variable zuzugreifen. Wenn z. b. eine Variable `P` vom Typ `int*` ist, kennzeichnet der Ausdruck `*P` die `int`-Variable, die sich an der in `P` enthaltenen Adresse befindet.

Ein Zeiger kann wie ein Objekt Verweis `null` sein. Das Anwenden des Dereferenzierungsoperators auf einen `null`-Zeiger führt zu einem von der Implementierung definierten Verhalten. Ein Zeiger mit dem Wert "`null`" wird durch "All-Bits-Zero" dargestellt.

Der `void*`-Typ stellt einen Zeiger auf einen unbekannten Typ dar. Da der Verweistyp unbekannt ist, kann der Dereferenzierungsoperator nicht auf einen Zeiger vom Typ "`void*`" angewendet werden, und es können keine Arithmetik für einen solchen Zeiger ausgeführt werden. Ein Zeiger vom Typ "`void*`" kann jedoch in einen anderen Zeigertyp (und umgekehrt) umgewandelt werden.

Zeiger Typen sind eine separate Kategorie von Typen. Anders als bei Verweis Typen und Werttypen erben Zeiger Typen nicht von `object` und es sind keine Konvertierungen zwischen Zeiger Typen und `object` vorhanden. Vor allem werden Boxing und Unboxing ([Boxing und Unboxing](types.md#boxing-and-unboxing)) für Zeiger nicht unterstützt. Allerdings sind Konvertierungen zwischen verschiedenen Zeiger Typen sowie zwischen Zeiger Typen und ganzzahligen Typen zulässig. Dies wird in [Zeiger Konvertierungen](unsafe-code.md#pointer-conversions)beschrieben.

Ein *pointer_type* kann nicht als Typargument ([konstruierte Typen](types.md#constructed-types)) verwendet werden, und der Typrückschluss ([Typrückschluss](expressions.md#type-inference)) schlägt bei generischen Methoden aufrufen fehl, die ein Typargument als Zeigertyp ableiten müssten.

Ein *pointer_type* kann als Typ eines flüchtigen Felds ([flüchtige Felder](classes.md#volatile-fields)) verwendet werden.

Obwohl Zeiger als `ref`-oder `out`-Parameter weitergegeben werden können, kann dies zu undefiniertem Verhalten führen, da der Zeiger möglicherweise so festgelegt wird, dass er auf eine lokale Variable verweist, die nicht mehr vorhanden ist, wenn die aufgerufene Methode zurückgibt, oder das fixierte Objekt, auf das verweist. , ist nicht mehr korrigiert. Zum Beispiel:

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

Eine Methode kann einen Wert eines Typs zurückgeben, und dieser Typ kann ein Zeiger sein. Wenn beispielsweise ein Zeiger auf eine zusammenhängende Sequenz von `int`s, der Element Anzahl der Sequenz und einem anderen `int`-Wert angegeben wird, gibt die folgende Methode die Adresse dieses Werts in dieser Sequenz zurück, wenn eine Entsprechung auftritt. Andernfalls wird `null` zurückgegeben:

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

In einem unsicheren Kontext sind mehrere-Konstrukte zum Ausführen von Zeigern verfügbar:

*  Der `*`-Operator kann verwendet werden, um eine Zeigerdereferenzierung auszuführen ([Zeigerdereferenzierung](unsafe-code.md#pointer-indirection)).
*  Der `->`-Operator kann verwendet werden, um auf einen Member einer Struktur über einen Zeiger ([Zeiger Element Zugriff](unsafe-code.md#pointer-member-access)) zuzugreifen.
*  Der `[]`-Operator kann verwendet werden, um einen Zeiger zu indizieren ([Zeiger Element Zugriff](unsafe-code.md#pointer-element-access)).
*  Der `&`-Operator kann verwendet werden, um die Adresse einer Variablen ([dem address-of-Operator](unsafe-code.md#the-address-of-operator)) abzurufen.
*  Mit den Operatoren "`++`" und "`--`" können Zeiger erhöht und dekrementiert werden ([Zeiger Inkrement und Dekrement](unsafe-code.md#pointer-increment-and-decrement)).
*  Die Operatoren "`+`" und "`-`" können verwendet werden, um Zeigerarithmetik ([Zeigerarithmetik](unsafe-code.md#pointer-arithmetic)) auszuführen.
*  Der `==`-, `!=`-, `<`-, `>`-, `<=`-und `=>`-Operatoren können verwendet werden, um Zeiger zu vergleichen ([Zeiger Vergleiche](unsafe-code.md#pointer-comparison)).
*  Der `stackalloc`-Operator kann verwendet werden, um Speicher aus der aufrufsstapel zuzuordnen ([Puffer fester Größe](unsafe-code.md#fixed-size-buffers)).
*  Die `fixed`-Anweisung kann verwendet werden, um eine Variable temporär zu korrigieren, sodass Ihre Adresse abgerufen werden kann ([die fixed-Anweisung](unsafe-code.md#the-fixed-statement)).

## <a name="fixed-and-moveable-variables"></a>Behobene und verschiebbare Variablen

Der Address-of-Operator ([der Address-of-Operator](unsafe-code.md#the-address-of-operator)) und die `fixed`-Anweisung ([die fixed-Anweisung](unsafe-code.md#the-fixed-statement)) dividieren Variablen in zwei Kategorien: ***Fixierte Variablen*** und ***verschiebbare Variablen***.

Fixierte Variablen befinden sich in Speicherorten, die von der Garbage Collector nicht betroffen sind. (Beispiele für fixierte Variablen sind lokale Variablen, Wert Parameter und Variablen, die durch dereferenzierende Zeiger erstellt werden.) Auf der anderen Seite befinden sich verschiebbare Variablen in Speicherorten, die von der Garbage Collector verlagert oder verfügbar gemacht werden. (Beispiele für verschiebbare Variablen sind Felder in Objekten und Elementen von Arrays.)

Der `&`-Operator ([der Address-of-Operator](unsafe-code.md#the-address-of-operator)) gestattet, dass die Adresse einer Variablen mit fester Größe ohne Einschränkungen abgerufen wird. Da eine verschiebbare Variable jedoch vom Garbage Collector verschoben oder aufgehoben werden kann, kann die Adresse einer verschiebbaren Variablen nur mit einer `fixed`-Anweisung ([fixed-Anweisung](unsafe-code.md#the-fixed-statement)) abgerufen werden, und diese Adresse bleibt nur für die die Dauer dieser `fixed`-Anweisung.

Eine Variable mit fester Genauigkeit ist eine der folgenden:

*  Eine Variable, die sich aus einem *Simple_name* ([simple names](expressions.md#simple-names)) ergibt, der auf eine lokale Variable oder einen value-Parameter verweist, es sei denn, die Variable wird von einer anonymen Funktion aufgezeichnet.
*  Eine Variable, die sich aus einem *member_access* ([Member Access](expressions.md#member-access)) der Form `V.I` ergibt, wobei "`V`" eine festgelegte Variable eines *struct_type*ist.
*  Eine Variable, die sich aus einer *pointer_indirection_expression* ([Zeigerdereferenzierung](unsafe-code.md#pointer-indirection)) der Form `*P`, eines *pointer_member_access* ([Zeiger Element Zugriffs](unsafe-code.md#pointer-member-access)) der Form `P->I` oder einer *pointer_element_access* ( [Zeiger Element Zugriff](unsafe-code.md#pointer-element-access)) der Form `P[E]`.

Alle anderen Variablen werden als verschiebbare Variablen klassifiziert.

Beachten Sie, dass ein statisches Feld als eine verschiebbare Variable klassifiziert ist. Beachten Sie außerdem, dass der Parameter "`ref`" oder "`out`" als eine verschiebbare Variable klassifiziert ist, auch wenn das für den-Parameter angegebene Argument eine Fixed-Variable ist. Beachten Sie schließlich, dass eine Variable, die durch dereferenzieren eines Zeigers erzeugt wird, immer als eine festgelegte Variable klassifiziert wird.

## <a name="pointer-conversions"></a>Zeigerkonvertierungen

In einem unsicheren Kontext wird der Satz der verfügbaren impliziten Konvertierungen ([implizite Konvertierungen](conversions.md#implicit-conversions)) um die folgenden impliziten Zeiger Konvertierungen erweitert:

*  Von allen *pointer_type* bis zum Typ `void*`.
*  Vom `null`-literalen zu beliebigen *pointer_type*.

Außerdem wird in einem unsicheren Kontext der Satz der verfügbaren expliziten Konvertierungen ([explizite Konvertierungen](conversions.md#explicit-conversions)) erweitert, um die folgenden expliziten Zeiger Konvertierungen einzubeziehen:

*  Von allen *pointer_type* bis hin zu anderen *pointer_type*.
*  Von `sbyte`, `byte`, `short`, `ushort`, `int`, `uint`, `long` oder `ulong` in beliebige *pointer_type*.
*  Von jeder *pointer_type* zu `sbyte`, `byte`, `short`, `ushort`, `int`, `uint`, `long` oder `ulong`.

Schließlich umfasst der Satz von impliziten Standard Konvertierungen ([standardmäßige implizite Konvertierungen](conversions.md#standard-implicit-conversions)) in einem unsicheren Kontext die folgende Zeiger Konvertierung:

*  Von allen *pointer_type* bis zum Typ `void*`.

Konvertierungen zwischen zwei Zeiger Typen ändern niemals den tatsächlichen Zeiger Wert. Anders ausgedrückt: eine Konvertierung von einem Zeigertyp in einen anderen hat keine Auswirkungen auf die zugrunde liegende Adresse, die durch den Zeiger angegeben wird.

Wenn ein Zeigertyp in einen anderen konvertiert wird, wenn der resultierende Zeiger nicht ordnungsgemäß für den Verweis auf den Typ ausgerichtet ist, ist das Verhalten nicht definiert, wenn das Ergebnis dereferenziert wird. Im Allgemeinen ist das Konzept "ordnungsgemäß ausgerichtet" transitiv: Wenn ein Zeiger auf den Typ "`A`" ordnungsgemäß für einen Zeiger auf den Typ "`B`" ausgerichtet ist, der wiederum ordnungsgemäß für einen Zeiger auf den Typ "`C`" ausgerichtet ist, wird ein Zeiger auf den Typ "`A`" ordnungsgemäß für einen Zeiger auf den Typ `C`.

Beachten Sie den folgenden Fall, in dem auf eine Variable mit einem Typ über einen Zeiger auf einen anderen Typ zugegriffen wird:

```csharp
char c = 'A';
char* pc = &c;
void* pv = pc;
int* pi = (int*)pv;
int i = *pi;         // undefined
*pi = 123456;        // undefined
```

Wenn ein Zeigertyp in einen Zeiger auf ein Byte konvertiert wird, zeigt das Ergebnis auf das niedrigste adressierte Byte der Variablen. Aufeinanderfolgende Inkremente des Ergebnisses bis zur Größe der Variablen ergeben Zeiger auf die restlichen Bytes dieser Variablen. Die folgende Methode zeigt beispielsweise alle acht Bytes in einem Double-Wert als Hexadezimalwert an:

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

Die erstellte Ausgabe hängt natürlich von der-Unterscheidung ab.

Zuordnungen zwischen Zeigern und ganzen Zahlen sind Implementierungs definiert. Allerdings Verhalten sich Konvertierungen von Zeigern auf oder von ganzzahligen Typen auf 32 *-und 64-Bit-CPU-Architekturen mit einem linearen Adressraum in der Regel genauso wie Konvertierungen von `uint`-oder `ulong`-Werten in bzw. aus diesen ganzzahligen Typen.

### <a name="pointer-arrays"></a>Zeiger Arrays

In einem unsicheren Kontext können Arrays von Zeigern erstellt werden. Nur einige der Konvertierungen, die für andere Array Typen gelten, sind in Zeiger Arrays zulässig:

*  Die implizite Verweis Konvertierung ([implizite Verweis Konvertierungen](conversions.md#implicit-reference-conversions)) von *array_type* in `System.Array` und die implementierten Schnittstellen gelten auch für Zeiger Arrays. Allerdings wird bei jedem Versuch, auf die Array Elemente über `System.Array` oder die von ihm implementierten Schnittstelle zuzugreifen, zur Laufzeit eine Ausnahme ausgelöst, da Zeiger Typen nicht in `object` konvertierbar sind.
*  Die impliziten und expliziten Verweis Konvertierungen ([implizite Verweis](conversions.md#implicit-reference-conversions)Konvertierungen, [explizite Verweis Konvertierungen](conversions.md#explicit-reference-conversions)) aus einem eindimensionalen Arraytyp `S[]` in `System.Collections.Generic.IList<T>` und seine generischen Basis Schnittstellen werden nie auf Zeiger Arrays angewendet. Zeiger Typen können nicht als Typargumente verwendet werden, und es sind keine Konvertierungen von Zeiger Typen zu nicht-Zeiger Typen vorhanden.
*  Die explizite Verweis Konvertierung ([explizite Verweis Konvertierungen](conversions.md#explicit-reference-conversions)) von `System.Array` und die Schnittstellen, die Sie für beliebige *array_type* implementiert, gelten für Zeiger Arrays.
*  Die expliziten Verweis Konvertierungen ([explizite Verweis Konvertierungen](conversions.md#explicit-reference-conversions)) von `System.Collections.Generic.IList<S>` und deren Basis Schnittstellen in einen eindimensionalen Arraytyp `T[]` gelten nie für Zeiger Arrays, da Zeiger Typen nicht als Typargumente verwendet werden können und es keine Konvertierungen von Zeiger Typen in nicht-Zeiger Typen.

Diese Einschränkungen bedeuten, dass die Erweiterung für die `foreach`-Anweisung über Arrays, die in [der foreach-Anweisung](statements.md#the-foreach-statement) beschrieben werden, nicht auf Zeiger Arrays angewendet werden kann. Stattdessen wird eine foreach-Anweisung der Form

```csharp
foreach (V v in x) embedded_statement
```

Wenn der `x`-Typ ein Arraytyp der Form ist `T[,,...,]`, `N` die Anzahl der Dimensionen minus 1 und `T` oder `V` ein Zeigertyp ist, wird wie folgt mithilfe von schsted for-Schleifen erweitert:

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

Die Variablen "`a`", "`i0`", "`i1`", "...", "`iN`" sind für `x` oder den *embedded_statement* oder einen beliebigen anderen Quellcode des Programms nicht sichtbar oder können darauf zugegriffen werden. Die Variable `v` ist in der eingebetteten Anweisung schreibgeschützt. Wenn keine explizite Konvertierung ([Zeiger Konvertierungen](unsafe-code.md#pointer-conversions)) von `T` (dem Elementtyp) zu `V` vorhanden ist, wird ein Fehler erzeugt, und es werden keine weiteren Schritte ausgeführt. Wenn `x` den Wert `null`hat, wird `System.NullReferenceException` zur Laufzeit eine ausgelöst.

## <a name="pointers-in-expressions"></a>Zeiger in Ausdrücken

In einem unsicheren Kontext kann ein Ausdruck das Ergebnis eines Zeiger Typs ergeben, aber außerhalb eines unsicheren Kontexts ist es ein Kompilierzeitfehler für einen Ausdruck, der einen Zeigertyp aufweisen soll. Genau gesagt: außerhalb eines unsicheren Kontexts tritt ein Kompilierzeitfehler auf, *Wenn Simple_name* ([simple names](expressions.md#simple-names)), *member_access* ([Member Access](expressions.md#member-access)), *invocation_expression* ([Aufruf Ausdrücke](expressions.md#invocation-expressions)) oder  *element_access* ([Element Zugriff](expressions.md#element-access)) ist ein Zeigertyp.

In einem unsicheren Kontext ermöglichen die *primary_no_array_creation_expression* ([Primary Expressions](expressions.md#primary-expressions)) und *unary_expression* ([unäre Operatoren](expressions.md#unary-operators)) die folgenden zusätzlichen Konstrukte:

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

Diese Konstrukte werden in den folgenden Abschnitten beschrieben. Die-Grammatik ist die Rangfolge und Assoziativität der unsicheren Operatoren.

### <a name="pointer-indirection"></a>Zeiger Dereferenzierung

Ein *pointer_indirection_expression* besteht aus einem Sternchen (`*`), gefolgt von einem *unary_expression*.

```antlr
pointer_indirection_expression
    : '*' unary_expression
    ;
```

Der unäre `*`-Operator bezeichnet die Zeiger Dereferenzierung und wird zum Abrufen der Variablen verwendet, auf die ein Zeiger zeigt. Das Ergebnis der Auswertung von "`*P`", wobei "`P`" ein Ausdruck eines Zeiger Typs `T*` ist, ist eine Variable vom Typ "`T`". Es handelt sich um einen Kompilierzeitfehler, mit dem der unäre `*`-Operator auf einen Ausdruck vom Typ `void*` oder auf einen Ausdruck angewendet wird, der kein Zeigertyp ist.

Die Auswirkung der Anwendung des unären `*`-Operators auf einen `null`-Zeiger ist Implementierungs definiert. Insbesondere gibt es keine Garantie, dass dieser Vorgang eine `System.NullReferenceException` auslöst.

Wenn dem Zeiger ein ungültiger Wert zugewiesen wurde, ist das Verhalten des unären `*`-Operators nicht definiert. Unter den ungültigen Werten für die Dereferenzierung eines Zeigers durch den unären `*`-Operator handelt es sich um eine Adresse, die für den Typ, auf den verwiesen wird (siehe Beispiel in [Zeiger Konvertierungen](unsafe-code.md#pointer-conversions)), und die Adresse einer Variablen nach dem Ende ihrer Lebensdauer nicht ordnungsgemäß ausgerichtet ist.

Zum Zweck der eindeutigen Zuweisungs Analyse wird eine Variable, die durch Auswerten eines Ausdrucks der Form `*P` erstellt wird, als anfänglich zugewiesen ([ursprünglich zugewiesene Variablen](variables.md#initially-assigned-variables)).

### <a name="pointer-member-access"></a>Zugriff auf Zeiger Elemente

Ein *pointer_member_access* besteht aus einem *primary_expression*, gefolgt von einem "`->`"-Token, gefolgt von einem *Bezeichner* und einem optionalen *type_argument_list*.

```antlr
pointer_member_access
    : primary_expression '->' identifier
    ;
```

In einem Zeiger Element Zugriff auf das Formular `P->I` muss `P` ein Ausdruck eines anderen Zeiger Typs als `void*` sein, und `I` muss einen zugänglichen Member des Typs angeben, zu dem `P`-Punkte gehören.

Ein Zeiger Element Zugriff auf das Formular `P->I` wird genau wie `(*P).I` ausgewertet. Eine Beschreibung des Zeigerdereferenzierungsoperators (`*`) finden Sie unter [Zeiger Dereferenzierung](unsafe-code.md#pointer-indirection). Eine Beschreibung des Mitglieds Zugriffs Operators (`.`) finden Sie unter [Member Access](expressions.md#member-access).

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

der `->`-Operator wird verwendet, um auf Felder zuzugreifen und eine Methode einer Struktur mithilfe eines Zeigers aufzurufen. Da der Vorgang `P->I` exakt dem `(*P).I` entspricht, könnte die `Main`-Methode gleichermaßen gut geschrieben worden sein:

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

### <a name="pointer-element-access"></a>Zeiger Element Zugriff

Ein *pointer_element_access* besteht aus einem *primary_no_array_creation_expression* , gefolgt von einem Ausdruck, der in "`[`" und "`]`" eingeschlossen ist.

```antlr
pointer_element_access
    : primary_no_array_creation_expression '[' expression ']'
    ;
```

Bei einem Zeiger Element Zugriff auf das Formular `P[E]` muss `P` ein Ausdruck eines anderen Zeiger Typs als `void*` sein, und `E` muss ein Ausdruck sein, der implizit in `int`, `uint`, `long` oder `ulong` konvertiert werden kann.

Ein Zeiger Element Zugriff auf das Formular `P[E]` wird genau wie `*(P + E)` ausgewertet. Eine Beschreibung des Zeigerdereferenzierungsoperators (`*`) finden Sie unter [Zeiger Dereferenzierung](unsafe-code.md#pointer-indirection). Eine Beschreibung des Zeiger Additions Operators (`+`) finden Sie unter [Zeigerarithmetik](unsafe-code.md#pointer-arithmetic).

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

Ein Zeiger Element Zugriff wird verwendet, um den Zeichen Puffer in einer `for`-Schleife zu initialisieren. Da der Vorgang `P[E]` genau mit `*(P + E)` übereinstimmt, könnte das Beispiel gleich gut geschrieben worden sein:

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

Der Zeiger Element-Zugriffs Operator prüft nicht auf Fehler aufgrund von Fehlern, und das Verhalten beim Zugriff auf ein Out-of-Bounds-Element ist nicht definiert. Dies entspricht dem C und C++.

### <a name="the-address-of-operator"></a>Der Address-of-Operator

Ein *addressof_expression* besteht aus einem kaufmännischen und-(`&`), gefolgt von einem *unary_expression*.

```antlr
addressof_expression
    : '&' unary_expression
    ;
```

Wenn ein Ausdruck `E` ist, der vom Typ "`T`" ist und als eine Variable mit fester Größe ([Fixed und verschiebbare Variablen](unsafe-code.md#fixed-and-moveable-variables)) klassifiziert ist, berechnet das Konstrukt "`&E`" die Adresse der Variablen, die von `E` angegeben wird. Der Ergebnistyp ist `T*` und wird als Wert klassifiziert. Ein Kompilierzeitfehler tritt auf, wenn `E` nicht als Variable klassifiziert ist, wenn `E` als schreibgeschützte lokale Variable klassifiziert ist, oder wenn `E` eine verschiebbare Variable bezeichnet. Im letzten Fall kann eine fixed-Anweisung ([die fixed-Anweisung](unsafe-code.md#the-fixed-statement)) verwendet werden, um die Variable temporär zu korrigieren, bevor Sie Ihre Adresse erhält. Wie in [Member Access](expressions.md#member-access)angegeben, wird das Feld außerhalb eines Instanzkonstruktors oder statischen Konstruktors für eine Struktur oder Klasse, die ein `readonly`-Feld definiert, als Wert, nicht als Variable angesehen. Daher kann die Adresse nicht übernommen werden. Ebenso kann die Adresse einer Konstante nicht übernommen werden.

Der `&`-Operator erfordert nicht, dass sein Argument definitiv zugewiesen wird, aber nach einem `&`-Vorgang wird die Variable, auf die der Operator angewendet wird, definitiv in dem Ausführungs Pfad zugewiesen, in dem der Vorgang stattfindet. Es liegt in der Verantwortung des Programmierers, sicherzustellen, dass die richtige Initialisierung der Variablen in dieser Situation tatsächlich stattfindet.

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

`i` wird nach dem `&i`-Vorgang, der zum Initialisieren von `p` verwendet wird, definitiv zugewiesen. Die Zuweisung zu "`*p`" initialisiert `i`, aber die Einbindung dieser Initialisierung liegt in der Verantwortung des Programmierers, und es tritt kein Kompilierzeitfehler auf, wenn die Zuweisung entfernt wurde.

Die Regeln der eindeutigen Zuweisung für den `&`-Operator sind so vorhanden, dass die redundante Initialisierung lokaler Variablen vermieden werden kann. Viele externe APIs nehmen z. b. einen Zeiger auf eine Struktur, die von der API ausgefüllt wird. Aufrufe dieser APIs übergeben in der Regel die Adresse einer lokalen Struktur Variablen, und ohne die Regel wäre eine redundante Initialisierung der Struktur Variablen erforderlich.

### <a name="pointer-increment-and-decrement"></a>Inkrementieren und Dekrementieren von Zeigern

In einem unsicheren Kontext können die Operatoren "`++`" und "`--`" ([postfix-Inkrement-und Dekrementoperatoren](expressions.md#postfix-increment-and-decrement-operators) und [Präfix Inkrement-und Dekrementoperatoren](expressions.md#prefix-increment-and-decrement-operators)) auf Zeiger Variablen aller Typen außer `void*` angewendet werden. Daher sind für jeden Zeigertyp `T*` die folgenden Operatoren implizit definiert:

```csharp
T* operator ++(T* x);
T* operator --(T* x);
```

Die Operatoren erzielen dieselben Ergebnisse wie `x + 1` und `x - 1` bzw. ([Zeigerarithmetik](unsafe-code.md#pointer-arithmetic)). Anders ausgedrückt: für eine Zeiger Variable vom Typ `T*` fügt der `++`-Operator der in der Variablen enthaltenen Adresse `sizeof(T)` hinzu, und der `--`-Operator subtrahiert `sizeof(T)` von der Adresse, die in der Variablen enthalten ist.

Wenn ein Zeiger Inkrement-oder Dekrement-Vorgang die Domäne des Zeiger Typs überschreitet, wird das Ergebnis durch die Implementierung definiert, es werden jedoch keine Ausnahmen erzeugt.

### <a name="pointer-arithmetic"></a>Zeigerarithmetik

In einem unsicheren Kontext können die Operatoren "`+`" und "`-`" (Additions[Operator](expressions.md#addition-operator) und [Subtraktions Operator](expressions.md#subtraction-operator)) auf Werte aller Zeiger Typen außer `void*` angewendet werden. Daher sind für jeden Zeigertyp `T*` die folgenden Operatoren implizit definiert:

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

Wenn ein Ausdruck `P` eines Zeiger Typs ist `T*` und ein Ausdruck `N` vom Typ `int`, `uint`, `long` oder `ulong`, berechnen die Ausdrücke `P + N` und `N + P` den Zeiger Wert des Typs `T*`, der sich aus dem Hinzufügen von 0 zur Adresse ergibt. wird von 1 angegeben. Entsprechend berechnet der Ausdruck `P - N` den Zeiger Wert des Typs `T*`, der sich aus der Subtraktion von `N * sizeof(T)` von der Adresse ergibt, die von `P` angegeben wird.

Wenn zwei Ausdrücke, `P` und `Q`, eines Zeiger Typs `T*`, berechnet der Ausdruck `P - Q` den Unterschied zwischen den Adressen, die von `P` und `Q` angegeben werden, und dividiert diese Differenz durch `sizeof(T)`. Der Ergebnistyp ist immer `long`. In der Tat wird `P - Q` als `((long)(P) - (long)(Q)) / sizeof(T)` berechnet.

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

der die Ausgabe erzeugt:

```console
p - q = -14
q - p = 14
```

Wenn eine Zeiger arithmetische Operation die Domäne des Zeiger Typs überschreitet, wird das Ergebnis in einer durch die Implementierung definierten Weise abgeschnitten, es werden jedoch keine Ausnahmen erzeugt.

### <a name="pointer-comparison"></a>Zeiger Vergleich

In einem unsicheren Kontext können die Operatoren "`==`", "`!=`", "`<`", "`>`", "`<=`" und "`=>`" ([relationale und Typtest Operatoren](expressions.md#relational-and-type-testing-operators)) auf Werte aller Zeiger Typen angewendet werden. Die Zeiger Vergleichs Operatoren lauten wie folgt:

```csharp
bool operator ==(void* x, void* y);
bool operator !=(void* x, void* y);
bool operator <(void* x, void* y);
bool operator >(void* x, void* y);
bool operator <=(void* x, void* y);
bool operator >=(void* x, void* y);
```

Da eine implizite Konvertierung von einem Zeigertyp in den `void*`-Typ vorhanden ist, können die Operanden beliebiger Zeigertyp mithilfe dieser Operatoren verglichen werden. Die Vergleichs Operatoren vergleichen die Adressen, die von den beiden Operanden angegeben werden, so, als wären Sie ganze Zahlen ohne Vorzeichen.

### <a name="the-sizeof-operator"></a>Der sizeof-Operator

Der `sizeof`-Operator gibt die Anzahl der Bytes zurück, die von einer Variablen eines bestimmten Typs belegt werden. Der als Operand angegebene Typ für `sizeof` muss ein *unmanaged_type* ([Zeiger Typen](unsafe-code.md#pointer-types)) sein.

```antlr
sizeof_expression
    : 'sizeof' '(' unmanaged_type ')'
    ;
```

Das Ergebnis des `sizeof`-Operators ist ein Wert vom Typ `int`. Für bestimmte vordefinierte Typen ergibt der `sizeof`-Operator einen konstanten Wert, wie in der folgenden Tabelle dargestellt.


| __expression__   | __Ergebnis__ |
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

Für alle anderen Typen ist das Ergebnis des `sizeof`-Operators Implementierungs definiert und wird als Wert, nicht als Konstante klassifiziert.

Die Reihenfolge, in der Elemente in einer Struktur verpackt werden, ist nicht angegeben.

Zu Ausrichtungs Zwecken gibt es möglicherweise unbenannte Auffüll Zeichen am Anfang einer Struktur, innerhalb einer Struktur und am Ende der Struktur. Der Inhalt der als Auffüllung verwendeten Bits ist unbestimmt.

Wenn ein Operand mit einem Strukturtyp angewendet wird, ist das Ergebnis die Gesamtzahl der Bytes in einer Variablen dieses Typs, einschließlich aller Auffüll Zeichen.

## <a name="the-fixed-statement"></a>Fixed-Anweisung

In einem unsicheren Kontext gestattet die *embedded_statement* ([Statements](statements.md)) Production ein zusätzliches Konstrukt, die `fixed`-Anweisung, die verwendet wird, um eine verschiebbare Variable zu "korrigieren", sodass die Adresse für die Dauer der Anweisung konstant bleibt. .

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

Jede *fixed_pointer_declarator* deklariert eine lokale Variable des angegebenen *pointer_type* und initialisiert diese lokale Variable mit der Adresse, die von der entsprechenden *fixed_pointer_initializer*berechnet wird. Auf eine lokale Variable, die in einer `fixed`-Anweisung deklariert ist, kann in allen *fixed_pointer_initializer*s, die rechts neben der Deklaration der Variablen auftreten, und in der *embedded_statement* -Anweisung der `fixed`-Anweisung zugegriffen werden. Eine lokale Variable, die von einer `fixed`-Anweisung deklariert wird, wird als schreibgeschützt betrachtet. Ein Kompilierzeitfehler tritt auf, wenn die eingebettete Anweisung versucht, diese lokale Variable (überzuweisung oder den `++`-und `--`-Operatoren) zu ändern oder Sie als `ref`-oder `out`-Parameter zu übergeben.

Ein *fixed_pointer_initializer* kann eines der folgenden sein:

*  Das Token "`&`" gefolgt von einem *variable_reference* ([genaue Regeln zum Bestimmen der eindeutigen Zuweisung](variables.md#precise-rules-for-determining-definite-assignment)) für eine verschiebbare Variable ([Feste und verschiebbare Variablen](unsafe-code.md#fixed-and-moveable-variables)) eines nicht verwalteten Typs `T`, sofern der Typ `T*` ist implizit in den Zeigertyp konvertierbar, der in der `fixed`-Anweisung angegeben ist. In diesem Fall berechnet der Initialisierer die Adresse der angegebenen Variablen, und die Variable bleibt für die Dauer der `fixed`-Anweisung garantiert an einer bestimmten Adresse.
*  Ein Ausdruck eines *array_type* mit Elementen eines nicht verwalteten Typs `T`, sofern der Typ `T*` implizit in den Zeigertyp konvertiert werden kann, der in der `fixed`-Anweisung angegeben ist. In diesem Fall berechnet der Initialisierer die Adresse des ersten Elements im Array, und das gesamte Array bleibt für die Dauer der `fixed`-Anweisung garantiert an einer Fixed-Adresse. Wenn der Array Ausdruck NULL ist oder das Array über keine Elemente verfügt, berechnet der Initialisierer eine Adresse, die gleich 0 (null) ist.
*  Ein Ausdruck vom Typ "`string`", wenn der Typ "`char*`" implizit in den Zeigertyp konvertiert werden kann, der in der `fixed`-Anweisung angegeben ist. In diesem Fall berechnet der Initialisierer die Adresse des ersten Zeichens in der Zeichenfolge, und die gesamte Zeichenfolge bleibt für die Dauer der `fixed`-Anweisung garantiert an einer Fixed-Adresse. Das Verhalten der `fixed`-Anweisung ist Implementierungs definiert, wenn der Zeichen folgen Ausdruck NULL ist.
*  Ein *Simple_name* -oder *member_access* -Element, das auf einen Puffer mit fester Größe einer verschiebbaren Variablen verweist, sofern der Typ des Puffer Elements mit fester Größe implizit in den Zeigertyp konvertiert werden kann, der in der `fixed`-Anweisung angegeben ist. In diesem Fall berechnet der Initialisierer einen Zeiger auf das erste Element des Puffers mit fester Größe ([Puffer fester Größe in Ausdrücken](unsafe-code.md#fixed-size-buffers-in-expressions)), und der Puffer mit fester Größe bleibt für die Dauer der `fixed`-Anweisung garantiert an einer Fixed-Adresse.

Für jede Adresse, die von einem *fixed_pointer_initializer* berechnet wird, stellt die `fixed`-Anweisung sicher, dass die Variable, auf die die Adresse verweist, nicht für die Dauer der `fixed`-Anweisung von der Garbage Collector verlagert oder nicht zur Verfügung gestellt wird. Wenn die von einem *fixed_pointer_initializer* berechnete Adresse beispielsweise auf ein Feld eines Objekts oder ein Element einer Array Instanz verweist, stellt die `fixed`-Anweisung sicher, dass die enthaltende Objektinstanz während des die Lebensdauer der Anweisung.

Es ist Aufgabe des Programmierers, sicherzustellen, dass die von `fixed`-Anweisungen erstellten Zeiger nicht über die Ausführung dieser Anweisungen hinausgehen. Wenn beispielsweise Zeiger, die von `fixed`-Anweisungen erstellt werden, an externe APIs übermittelt werden, liegt es in der Verantwortung des Programmierers sicherzustellen, dass die APIs keinen Arbeitsspeicher für diese Zeiger erhalten.

Fixed-Objekte können die Fragmentierung des Heaps verursachen (da Sie nicht verschoben werden können). Aus diesem Grund sollten Objekte nur dann korrigiert werden, wenn Sie unbedingt erforderlich sind, und dann nur für die kürzeste benötigte Zeit.

Das Beispiel

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

veranschaulicht verschiedene Verwendungen der `fixed`-Anweisung. Mit der ersten Anweisung wird die Adresse eines statischen Felds korrigiert und abgerufen, mit der zweiten Anweisung wird die Adresse eines Instanzfelds korrigiert und abgerufen, und die dritte Anweisung korrigiert und ruft die Adresse eines Array Elements ab. In jedem Fall wäre es ein Fehler, den regulären `&`-Operator zu verwenden, da die Variablen alle als verschiebbare Variablen klassifiziert werden.

Die vierte `fixed`-Anweisung im obigen Beispiel führt zu einem ähnlichen Ergebnis wie das dritte.

In diesem Beispiel für die `fixed`-Anweisung wird `string` verwendet:

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

In einem unsicheren Kontext Array werden Elemente von eindimensionalen Arrays in zunehmender Index Reihenfolge gespeichert, beginnend mit Index `0` und endende mit Index `Length - 1`. Bei mehrdimensionalen Arrays werden Array Elemente so gespeichert, dass die Indizes der äußersten rechten Dimension zuerst, dann die nächste linke Dimension usw. nach links angehoben werden. Innerhalb einer `fixed`-Anweisung, die einen Zeiger `p` auf eine Array Instanz `a` abruft, stellen die Zeiger Werte zwischen `p` und `p + a.Length - 1` Adressen der Elemente im Array dar. Ebenso stellen die Variablen, von `p[0]` bis `p[a.Length - 1]`, die eigentlichen Array Elemente dar. Angesichts der Art und Weise, in der Arrays gespeichert werden, können wir ein Array einer beliebigen Dimension so behandeln, als wäre es linear.

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

der die Ausgabe erzeugt:

```console
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

eine `fixed`-Anweisung wird verwendet, um ein Array zu korrigieren, sodass die Adresse an eine Methode, die einen Zeiger annimmt, übermittelt werden kann.

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

eine fixed-Anweisung wird verwendet, um einen Puffer fester Größe einer Struktur zu korrigieren, sodass die Adresse als Zeiger verwendet werden kann.

Ein `char*`-Wert, der durch das Reparieren einer Zeichen folgen Instanz erzeugt wird, verweist immer auf eine mit NULL endenden Zeichenfolge Innerhalb einer fixed-Anweisung, die einen Zeiger `p` auf eine Zeichen folgen Instanz `s` abruft, stellen die Zeiger Werte von `p` bis `p + s.Length - 1` Adressen der Zeichen in der Zeichenfolge dar, und der Zeiger Wert `p + s.Length` zeigt immer auf ein NULL-Zeichen (das Zeichen mit dem Wert `'\0'`).

Das Ändern von Objekten des verwalteten Typs durch Fixed-Zeiger kann zu undefiniertem Verhalten führen. Da z. b. Zeichen folgen unveränderlich sind, ist es die Aufgabe des Programmierers sicherzustellen, dass die Zeichen, auf die von einem Zeiger auf eine festgelegte Zeichenfolge verwiesen wird, nicht geändert werden.

Die automatische NULL-Beendigung von Zeichen folgen ist besonders praktisch, wenn externe APIs aufgerufen werden, die "C-Style"-Zeichen folgen erwarten. Beachten Sie jedoch, dass eine Zeichen folgen Instanz NULL-Zeichen enthalten darf. Wenn solche NULL-Zeichen vorhanden sind, wird die Zeichenfolge abgeschnitten, wenn Sie als NULL-terminierte `char*` behandelt wird.

## <a name="fixed-size-buffers"></a>Puffer fester Größe

Puffer fester Größe werden verwendet, um in-Line-Arrays im C-Format als Member von Strukturen zu deklarieren und sind hauptsächlich für die Schnittstellen mit nicht verwalteten APIs nützlich.

### <a name="fixed-size-buffer-declarations"></a>Puffer Deklarationen fester Größe

Ein ***Puffer fester Größe*** ist ein Member, der den Speicher für einen Puffer fester Länge von Variablen eines bestimmten Typs darstellt. Eine Puffer Deklaration mit fester Größe führt einen oder mehrere Puffer fester Größe eines bestimmten Elementtyps ein. Puffer fester Größe sind nur in Struktur Deklarationen zulässig und können nur in unsicheren Kontexten ([unsichere Kontexte](unsafe-code.md#unsafe-contexts)) vorkommen.

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

Eine Puffer Deklaration fester Größe kann einen Satz von Attributen ([Attribute](attributes.md)), einen `new`-Modifizierer ([Modifizierer](classes.md#modifiers)), eine gültige Kombination der vier Zugriffsmodifizierer ([Typparameter und Einschränkungen](classes.md#type-parameters-and-constraints)) und einen `unsafe`-Modifizierer (unsicher) enthalten.[ Kontexte](unsafe-code.md#unsafe-contexts)). Die Attribute und Modifizierer gelten für alle Member, die von der Puffer Deklaration mit fester Größe deklariert werden. Es ist ein Fehler, dass derselbe Modifizierer mehrmals in einer Puffer Deklaration fester Größe angezeigt wird.

Eine Puffer Deklaration mit fester Größe darf den `static`-Modifizierer nicht enthalten.

Der Puffer Elementtyp einer Puffer Deklaration mit fester Größe gibt den Elementtyp der Puffer an, die von der Deklaration eingeführt wurden. Beim Puffer Elementtyp muss es sich um einen der vordefinierten Typen `sbyte`, `byte`, `short`, `ushort`, `int`, `uint`, `long`, `ulong`, `char`, `float`, 0 oder 1 handeln.

Auf den Puffer Elementtyp folgt eine Liste von Puffer Deklaratoren fester Größe, von denen jeder einen neuen Member einführt. Ein Puffer mit fester Größe besteht aus einem Bezeichner, der den Member benennt, gefolgt von einem konstanten Ausdruck, der in `[`-und `]`-Token eingeschlossen ist. Der Konstante Ausdruck gibt die Anzahl der Elemente in dem Element an, das von diesem Puffer Deklarator mit fester Größe eingeführt wurde. Der Typ des konstanten Ausdrucks muss implizit in den Typ "`int`" konvertiert werden, und der Wert muss eine positive ganze Zahl ungleich NULL sein.

Es ist sichergestellt, dass die Elemente eines Puffers mit fester Größe sequenziell im Arbeitsspeicher angeordnet werden.

Eine Puffer Deklaration fester Größe, die mehrere Puffer fester Größe deklariert, entspricht mehreren Deklarationen einer einzelnen Puffer Deklaration mit fester Größe mit denselben Attributen und Elementtypen. Beispiel:

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

Die Member-Suche ([Operatoren](expressions.md#operators)) eines Puffer Elements fester Größe verläuft genau wie die Element Suche eines Felds.

Ein Puffer fester Größe kann in einem Ausdruck mithilfe eines *Simple_name* ([Typrückschlusses](expressions.md#type-inference)) oder eines *member_access* ([Kompilierzeit Überprüfung der dynamischen Überladungs Auflösung](expressions.md#compile-time-checking-of-dynamic-overload-resolution)) referenziert werden.

Wenn ein Puffer Element mit fester Größe als einfacher Name referenziert wird, entspricht der Effekt dem Element Zugriff im Formular `this.I`, wobei `I` das Puffer Element mit fester Größe ist.

Wenn ein Element Zugriff auf das Formular `E.I`, wenn `E` einen Strukturtyp und eine Element Suche von `I` in diesem Strukturtyp einen Member mit fester Größe identifiziert, wird `E.I` wie folgt als klassifiziert ausgewertet:

*  Wenn der Ausdruck `E.I` nicht in einem unsicheren Kontext auftritt, tritt ein Kompilierzeitfehler auf.
*  Wenn `E` als Wert klassifiziert wird, tritt ein Kompilierzeitfehler auf.
*  Andernfalls tritt ein Kompilierzeitfehler auf, wenn `E` eine verschiebbare Variable ([Fixed und verschiebbare Variablen](unsafe-code.md#fixed-and-moveable-variables)) und der Ausdruck `E.I` kein *fixed_pointer_initializer* ([die fixed-Anweisung](unsafe-code.md#the-fixed-statement)) ist.
*  Andernfalls verweist `E` auf eine Variable mit fester Größe, und das Ergebnis des Ausdrucks ist ein Zeiger auf das erste Element des Puffer Elements mit fester Größe `I` in `E`. Das Ergebnis ist vom Typ "`S*`", wobei "`S`" der Elementtyp "`I`" ist und als Wert klassifiziert wird.

Auf die nachfolgenden Elemente des Puffers mit fester Größe kann mithilfe von Zeiger Vorgängen aus dem ersten Element zugegriffen werden. Im Gegensatz zum Zugriff auf Arrays ist der Zugriff auf die Elemente eines Puffers fester Größe ein unsicherer Vorgang und ist nicht Bereichs geprüft.

Im folgenden Beispiel wird eine Struktur mit einem Puffer Element fester Größe deklariert und verwendet.

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

### <a name="definite-assignment-checking"></a>Definitive Zuweisungs Überprüfung

Puffer fester Größe unterliegen nicht der eindeutigen Zuweisungs Überprüfung ([definitive Zuweisung](variables.md#definite-assignment)), und Puffer Elemente fester Größe werden ignoriert, um eine definitive Zuweisungs Überprüfung von Strukturtyp Variablen zu bestimmen.

Wenn die äußerste enthaltende Struktur Variable eines Puffer Elements mit fester Größe eine statische Variable, eine Instanzvariable einer Klasseninstanz oder ein Array Element ist, werden die Elemente des Puffers fester Größe automatisch mit ihren Standardwerten initialisiert ([Standardeinstellung: Werte](variables.md#default-values)). In allen anderen Fällen ist der anfängliche Inhalt eines Puffers mit fester Größe nicht definiert.

## <a name="stack-allocation"></a>Stapel Zuordnung

In einem unsicheren Kontext kann eine lokale Variablen Deklaration ([Deklarationen von lokalen Variablen](statements.md#local-variable-declarations)) einen Stapel Zuordnungs Initialisierer enthalten, der Arbeitsspeicher aus der-aufrufsliste zuweist.

```antlr
local_variable_initializer_unsafe
    : stackalloc_initializer
    ;

stackalloc_initializer
    : 'stackalloc' unmanaged_type '[' expression ']'
    ;
```

Der *unmanaged_type* gibt den Typ der Elemente an, die im neu zugewiesenen Speicherort gespeichert werden, und der *Ausdruck* gibt die Anzahl dieser Elemente an. Diese geben die erforderliche Zuordnungs Größe an. Da die Größe einer Stapel Zuordnung nicht negativ sein kann, handelt es sich um einen Kompilierzeitfehler, der die Anzahl der Elemente als *constant_expression* angibt, die einen negativen Wert ergibt.

Ein Stapel Zuordnungs Initialisierer der Form `stackalloc T[E]` erfordert, dass `T` ein nicht verwalteter Typ ([Zeiger Typen](unsafe-code.md#pointer-types)) und `E` ein Ausdruck des Typs `int` ist. Das-Konstrukt ordnet `E * sizeof(T)` Bytes aus der-aufrufsstapel zu und gibt einen Zeiger vom Typ `T*` an den neu belegten Block zurück. Wenn `E` ein negativer Wert ist, ist das Verhalten nicht definiert. Wenn `E` 0 (null) ist, wird keine Zuweisung durchgeführt, und der zurückgegebene Zeiger ist Implementierungs definiert. Wenn nicht genügend Arbeitsspeicher verfügbar ist, um einen Block mit der angegebenen Größe zuzuordnen, wird eine `System.StackOverflowException` ausgelöst.

Der Inhalt des neu zugeordneten Speichers ist undefiniert.

Stapel Zuordnungs Initialisierer sind in `catch`-oder `finally`-Blöcken ([try-Anweisung](statements.md#the-try-statement)) nicht zulässig.

Es gibt keine Möglichkeit, Arbeitsspeicher, der mit `stackalloc` belegt wird, explizit freizugeben. Alle Stapel zugeordneten Speicherblöcke, die während der Ausführung eines Funktionsmembers erstellt werden, werden automatisch verworfen, wenn der Funktions Member zurückgegeben wird. Dies entspricht der Funktion "`alloca`", eine Erweiterung, die häufig in C++ C und Implementierungen gefunden wird.

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

ein `stackalloc`-Initialisierer wird in der `IntToString`-Methode verwendet, um einen Puffer mit 16 Zeichen im Stapel zuzuordnen. Der Puffer wird automatisch verworfen, wenn die Methode zurückgibt.

## <a name="dynamic-memory-allocation"></a>Dynamische Speicher Belegung

Mit Ausnahme des `stackalloc`-Operators C# werden von keine vordefinierten Konstrukte zum Verwalten von nicht Garbage Collection-Speicher bereitgestellt. Solche Dienste werden in der Regel durch Unterstützung von Klassenbibliotheken oder direkt aus dem zugrunde liegenden Betriebssystem bereitgestellt. Beispielsweise wird in der `Memory`-Klasse unten veranschaulicht, wie auf die Heap Funktionen eines zugrunde liegenden Betriebssystems C#zugegriffen werden kann:

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

Im folgenden finden Sie ein Beispiel, in dem die Klasse `Memory` verwendet wird:

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

Im Beispiel werden 256 Bytes Arbeitsspeicher über `Memory.Alloc` zugewiesen und der Speicherblock mit Werten, die von 0 bis 255 steigen, initialisiert. Anschließend ordnet es ein 256-Element-Bytearray zu und verwendet `Memory.Copy`, um den Inhalt des Speicherblocks in das Bytearray zu kopieren. Schließlich wird der Speicherblock mithilfe von `Memory.Free` freigegeben, und der Inhalt des Bytearrays wird in der Konsole ausgegeben.
