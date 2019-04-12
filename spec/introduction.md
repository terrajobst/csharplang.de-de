---
ms.openlocfilehash: db10046af5d635b430951679a448e23680b18b87
ms.sourcegitcommit: a19fac74c01a6c3da67d38b2f79527145d4edcbc
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/09/2019
ms.locfileid: "59426811"
---
# <a name="introduction"></a>Einführung

C# (Aussprache „C Sharp“) ist eine einfache, moderne, objektorientierte und typsichere Programmiersprache. C# hat seine Wurzeln in der Sprachen C und C, C++ und Java-Programmierer sofort vertraut sein. C# ist durch ECMA-International als standardisiert die ***ECMA-334*** standard und ISO/IEC als die ***ISO/IEC 23270*** standard. Von Microsoft c#-Compiler für .NET Framework ist keine entsprechende Implementierung dieser beiden dieser Standards.

C# ist eine objektorientierte Sprache, umfasst allerdings auch Unterstützung für eine ***komponentenorientierte*** Programmierung. Die Softwareentwicklung von heute beruht zunehmend auf Softwarekomponenten in Form von eigenständigen und selbstbeschreibenden Funktionspaketen. Wichtig bei solchen Komponenten ist, dass sie für ein Programmiermodell mit Eigenschaften, Methoden und Ereignissen stehen. Sie verfügen über Attribute, die deklarative Informationen zur Komponente bereitstellen, und lassen sich in ihre eigene Dokumentation integrieren. C# bietet Sprachkonstrukte zur direkten Unterstützung für diese Konzepte, wodurch c# zu einer sehr natürlichen Sprache, in denen zum Erstellen und Verwenden der Software-Komponenten.

Mehrere C#-Funktionen helfen beim Entwickeln stabiler und dauerhafter Anwendungen: ***Die automatische speicherbereinigung*** von nicht verwendeten Objekte belegten Arbeitsspeicher automatisch freigibt ***Ausnahmebehandlung*** bietet einen strukturierten und erweiterbaren Ansatz für die fehlererkennung und Wiederherstellung und die ***typsichere*** Aufbau der Sprache macht es unmöglich, nicht initialisierte Variablen gelesen werden soll. Geben Sie zum Indizieren Umwandlungen Arrays über die Grenzen oder führen Sie deaktiviert.

C# verfügt über ein ***einheitliches Typsystem***. Alle C#-Typen, einschließlich primitiver Typen wie `int` und `double`, erben von einem einzelnen `object`-Stammtyp. Daher verwenden alle Typen einen Satz allgemeiner Vorgänge, und Werte eines beliebigen Typs können gespeichert, übertragen und konsistent ausgeführt werden. Darüber hinaus unterstützt C# benutzerdefinierte Verweistypen und Werttypen und ermöglicht so die dynamische Zuordnung von Objekten sowie die Inlinespeicherung einfacher Strukturen.

Um sicherzustellen, dass c#-Programmen und Bibliotheken im Laufe der Zeit kompatibel weiterentwickelt werden können, wurde viel Bedeutung für platziert ***versionsverwaltung*** # Entwurf. Viele Programmiersprachen schenken diesem Problem wenig Beachtung, und in dieser Sprache geschriebene Programme stürzen daher häufiger als notwendig ab, wenn neuere Versionen abhängiger Bibliotheken eingeführt werden. Aspekte der C#des Entwurfs, der direkt von Überlegungen bei der Versionskontrolle beeinflusst wurden, gehören die separaten `virtual` und `override` Modifizierer, die Regeln für die überladungsauflösung und die Unterstützung für explizite Schnittstellenmember-Deklarationen.

Im verbleibenden Teil dieses Kapitels wird beschrieben, die wesentlichen Funktionen von c#-Sprache. Obwohl es sich bei spätere Kapiteln detailorientiertheit und manchmal mathematischen Regeln und Ausnahmen zu beschreiben, wurde in diesem Kapitel für Klarheit und Übersichtlichkeit zu Lasten der Vollständigkeit. Ziel ist für eine Einführung in die Sprache des Readers bereit, die das Schreiben von frühen Programme und das Lesen der späteren Kapiteln erleichtern wird.

## <a name="hello-world"></a>Hello World

Das Programm „Hello, World“ wird für gewöhnlich zur Einführung einer Programmiersprache verwendet. Hier ist es in C#:

```csharp
using System;

class Hello
{
    static void Main() {
        Console.WriteLine("Hello, World");
    }
}
```

C#-Quelldateien weisen in der Regel die Dateierweiterung `.cs` auf. Unter der Annahme, dass das Programm "Hello, World" in der Datei gespeichert wird `hello.cs`, das Programm kompiliert werden kann, mit dem Microsoft C#-Compiler, die über die Befehlszeile
```
csc hello.cs
```
erzeugt eine ausführbare Assembly mit dem Namen `hello.exe`. Die Ausgabe von dieser Anwendung erstellt wird, wenn er ausgeführt wird
```
Hello, World
```

Das Programm „Hello, World“ wird mit einer `using`-Richtlinie gestartet, die auf den `System`-Namespace verweist. Namespaces bieten eine hierarchische Möglichkeit zum Organisieren von C#-Programmen und -Bibliotheken. Namespaces enthalten Typen und andere Namespaces. Beispiel: Der `System`-Namespace enthält eine Reihe von Typen, wie etwa die `Console`-Klasse, auf die im Programm verwiesen wird, und eine Reihe anderer Namespaces, wie etwa `IO` und `Collections`. Eine `using`-Richtlinie, die auf einen bestimmten Namespace verweist, ermöglicht die nicht qualifizierte Nutzung der Typen, die Member dieses Namespace sind. Aufgrund der `using`-Direktive kann das Programm `Console.WriteLine` als Abkürzung für `System.Console.WriteLine` verwenden.

Die `Hello`-Klasse, die vom Programm „Hello, World“ deklariert wird, verfügt über einen einzelnen Member: die `Main`-Methode. Die `Main` Methode wird deklariert, mit der `static` Modifizierer. Auch wenn Instanzmethoden mit dem Schlüsselwort `this` auf eine bestimmte einschließende Objektinstanz verweisen können, agieren statische Methoden ohne Verweis auf ein bestimmtes Objekt. Gemäß der Konvention fungiert eine statische Methode mit der Bezeichnung `Main` als Einstiegspunkt eines Programms.

Die Ausgabe des Programms wird anhand der `WriteLine`-Methode der `Console`-Klasse im `System`-Namespace generiert. Diese Klasse wird von der .NET Framework-Klassenbibliotheken bereitgestellt, die standardmäßig vom Microsoft C#-Compiler automatisch verwiesen wird. Beachten Sie, dass c# selbst keine separate Laufzeit-Bibliothek. Stattdessen ist .NET Framework die Runtime-Bibliothek von c#.

## <a name="program-structure"></a>Programmstruktur

Die organisatorischen Schlüsselkonzepte in C# sind: ***Programme***, ***Namespaces***, ***Typen***, ***Member*** und ***Assemblys***. C#-Programme bestehen aus mindestens einer Quelldatei. Programme deklarieren Typen, die Member enthalten, und können in Namespaces organisiert werden. Klassen und Schnittstellen sind Beispiele für Typen. Felder, Methoden, Eigenschaften und Ereignisse sind Beispiele für Member. Wenn C#-Programme kompiliert werden, werden sie physisch in Assemblys verpackt. Assemblys müssen in der Regel die Dateierweiterung `.exe` oder `.dll`, abhängig davon, ob sie implementieren ***Anwendungen*** oder ***Bibliotheken***.

Im Beispiel

```csharp
using System;

namespace Acme.Collections
{
    public class Stack
    {
        Entry top;

        public void Push(object data) {
            top = new Entry(top, data);
        }

        public object Pop() {
            if (top == null) throw new InvalidOperationException();
            object result = top.data;
            top = top.next;
            return result;
        }

        class Entry
        {
            public Entry next;
            public object data;
    
            public Entry(Entry next, object data) {
                this.next = next;
                this.data = data;
            }
        }
    }
}
```
deklariert eine Klasse namens `Stack` in einem Namespace namens `Acme.Collections`. Der vollqualifizierte Name dieser Klasse ist `Acme.Collections.Stack`. Die Klasse enthält mehrere Member: ein Feld mit dem Namen `top`, zwei Methoden mit dem Namen `Push` und `Pop` sowie eine geschachtelte Klasse mit dem Namen `Entry`. Die `Entry`-Klasse enthält weitere drei Member: ein Feld mit dem Namen `next`, ein Feld mit dem Namen `data` und einen Konstruktor. Vorausgesetzt, dass der Quellcode des Beispiels in der Datei `acme.cs` gespeichert wird, kompiliert die Befehlszeile

```
csc /t:library acme.cs
```
das Beispiel als Bibliothek (Code ohne `Main`-Einstiegspunkt) und erstellt eine Assembly mit dem Namen `acme.dll`.

Assemblys enthalten ausführbaren Code in Form von ***Intermediate Language*** (IL)-Anweisungen und symbolischen Informationen in Form von ***Metadaten***. Vor der Ausführung wird der IL-Code in einer Assembly automatisch durch den Just-in-Time-Compiler (JIT) der .NET Common Language Runtime in prozessorspezifischen Code konvertiert.

Da eine Assembly eine selbstbeschreibende Funktionseinheit mit Code und Metadaten ist, besteht in C# keine Notwendigkeit für `#include`-Direktiven und Headerdateien. Die öffentlichen Typen und Member, die in einer bestimmten Assembly enthalten sind, werden einfach durch Verweisen auf die Assembly beim Kompilieren des Programms in einem C#-Programm verfügbar gemacht. Dieses Programm verwendet z.B. die `Acme.Collections.Stack`-Klasse aus der `acme.dll`-Assembly:

```csharp
using System;
using Acme.Collections;

class Test
{
    static void Main() {
        Stack s = new Stack();
        s.Push(1);
        s.Push(10);
        s.Push(100);
        Console.WriteLine(s.Pop());
        Console.WriteLine(s.Pop());
        Console.WriteLine(s.Pop());
    }
}
```
Wenn das Programm in der Datei gespeichert ist `test.cs`bei `test.cs` kompiliert wurde, die `acme.dll` Assembly kann verwiesen werden, mithilfe des Compilers `/r` Option:

```
csc /r:acme.dll test.cs
```
So wird eine ausführbare Assembly mit dem Namen `test.exe` erstellt, die bei Ausführung folgende Ausgabe erzeugt:

```
100
10
1
```
In C# kann der Quelltext eines Programms in verschiedenen Quelldateien gespeichert werden. Bei der Kompilierung eines C#-Programms mit mehreren Dateien werden alle Quelldateien zusammen verarbeitet, und die Quelldateien können frei aufeinander verweisen – vom Konzept her ist es so, als seien alle Quelldateien vor der Verarbeitung in einer einzigen großen Datei verkettet worden. Vorwärtsdeklarationen sind in C# nie erforderlich, da die Reihenfolge der Deklaration mit wenigen Ausnahmen unbedeutend ist. C# beschränkt eine Quelldatei weder auf die Deklaration eines einzigen öffentlichen Typs, noch muss der Name der Quelldatei mit einem in der Quelldatei deklarierten Typ übereinstimmen.

## <a name="types-and-variables"></a>Typen und Variablen

Es gibt zwei Arten von Typen in C#: ***Werttypen*** und ***Verweistypen***. Variablen von Werttypen enthalten ihre Daten direkt, Variablen von Verweistypen speichern hingegen Verweise auf ihre Daten – letztere werden als Objekte bezeichnet. Mit Verweistypen können zwei Variablen auf das gleiche Objekt verweisen, und so können an einer Variablen durchgeführte Vorgänge das Objekt beeinflussen, auf das die andere Variable verweist. Bei Werttypen besitzt jede Variable eine eigene Kopie der Daten, und auf eine Variable angewendete Vorgänge können die andere Variable nicht beeinflussen (außer im Fall von `ref`- und `out`-Parametervariablen).

C#die Werttypen sind weiter unterteilt in ***einfache Typen***, ***Enumerationstypen***, ***Strukturtypen***, und ***auf NULL festlegbare Typen***, und C#des Verweistypen sind weiter unterteilt in ***Klassentypen***, ***Schnittstellentypen***, ***Arraytypen***, und ***Delegattypen***.

Die folgende Tabelle enthält eine Übersicht über C#Typsystem.

| __Kategorie__    |                 | __Beschreibung__ |
|-----------------|-----------------|-----------------|
| Werttypen     | Einfache Typen    | Ganzzahlig mit Vorzeichen: `sbyte`, `short`, `int`, `long` |
|                 |                 | Ganzzahlig ohne Vorzeichen: `byte`, `ushort`, `uint`, `ulong` |
|                 |                 | Unicode-Zeichen: `char` |
|                 |                 | IEEE-Gleitkomma: `float`, `double` |
|                 |                 | Decimal mit hoher Präzision: `decimal` |
|                 |                 | Boolescher Wert: `bool` |
|                 | Enumerationstypen      | Benutzerdefinierte Typen des Formulars `enum E {...}` |
|                 | Strukturtypen    | Benutzerdefinierte Typen des Formulars `struct S {...}` |
|                 | Auf NULL festlegbare Typen  | Erweiterungen aller anderen Werttypen mit einem `null`-Wert |
| Verweistypen | Klassentypen     | Ultimative Basisklasse aller anderen Typen: `object` |
|                 |                 | Unicode-Zeichenfolgen: `string` |
|                 |                 | Benutzerdefinierte Typen des Formulars `class C {...}` |
|                 | Schnittstellentypen | Benutzerdefinierte Typen des Formulars `interface I {...}` |
|                 | Arraytypen     | Ein- und mehrdimensional, z. B. `int[]` und `int[,]` |
|                 | Delegattypen  | Benutzerdefinierte Typen im Format z. B. `delegate int  D(...)` |

Die acht Ganzzahltypen unterstützen 8-Bit-, 16-Bit, 32-Bit- und 64-Bit-Werte mit oder ohne Vorzeichen.

Zeigen Sie die beiden floating-Typen, `float` und `double`, werden mit der 32-Bit mit einfacher Genauigkeit und 64-Bit-Gleitkommazahl mit doppelter Genauigkeit IEEE 754-Formate dargestellt.

Der `decimal`-Typ ist ein für Finanz-und Währungsberechnungen geeigneter 128-Bit-Datentyp.

C#die `bool` Typ dient zur Darstellung boolescher Werte – Werte, die entweder `true` oder `false`.

Zur Zeichen- und Zeichenfolgenverarbeitung in C# wird die Unicode-Codierung verwendet. Der `char`-Typ stellt eine UTF-16-Codeeinheit dar und der `string`-Typ eine Folge von UTF-16-Codeeinheiten.

Die folgende Tabelle enthält C#der numerischen Typen.


| __Kategorie__      | __Bits__ | __Typ__  | __Bereich/der gleichen Genauigkeit__ |
|-------------------|----------|-----------|---------------------|
| Ganzzahlig mit Vorzeichen   | 8        | `sbyte`   | -128...127 |
|                   | 16       | `short`   | -32,768...32,767 |
|                   | 32       | `int`     | -2,147,483,648...2,147,483,647 |
|                   | 64       | `long`    | -9,223,372,036,854,775,808...9,223,372,036,854,775,807 |
| Ganzzahlig ohne Vorzeichen | 8        | `byte`    | 0...255 |
|                   | 16       | `ushort`  | 0...65,535 |
|                   | 32       | `uint`    | 0...4,294,967,295 |
|                   | 64       | `ulong`   | 0...18,446,744,073,709,551,615 |
| Gleitkomma    | 32       | `float`   | 1,5 × 10 ^-45 bis 3,4 × 10 ^ 38, Genauigkeit von 7 Stellen |
|                   | 64       | `double`  | 5,0 × 10 ^-324 bis 1,7 × 10 ^ 308, Genauigkeit von 15 Stellen |
| Decimal           | 128      | `decimal` | 1.0 × 10 ^-28 7,9 × 10 ^ 28, 28 Stellen Genauigkeit |

C#-Programme verwenden ***Typdeklarationen***, um neue Typen zu erstellen. Eine Typdeklaration gibt den Namen und die Member des neuen Typs an. Fünf C#der Kategorien von Typen sind Benutzerdefinierbar: Klasse Typen, Strukturtypen, Schnittstellentypen, Enumerationstypen und Delegattypen.

Ein Klassentyp definiert eine Datenstruktur, die Datenmember (Felder) und Funktionsmember (Methoden, Eigenschaften usw.) enthält. Klassentypen unterstützen einzelne Vererbung und Polymorphie. Dies sind Mechanismen, durch die abgeleitete Klassen erweitert und Basisklassen spezialisiert werden können.

Ein Strukturtyp ähnelt einem Klassentyp, da es sich um eine Struktur mit Datenmembern und Funktionsmembern darstellt. Im Gegensatz zu Klassen, Strukturen sind allerdings Werttypen und erfordern keine Heapzuordnung. Strukturtypen unterstützen keine benutzerdefinierte Vererbung, und alle Strukturtypen erben implizit vom Typ `object`.

Ein Schnittstellentyp definiert einen Vertrag als benannter Satz öffentlicher Funktionsmember. Eine Klasse oder Struktur, die eine Schnittstelle implementiert, muss Implementierungen der Funktionsmember der Schnittstelle bereitstellen. Eine Schnittstelle kann von mehreren Basisschnittstellen erben, und eine Klasse oder Struktur kann mehrere Schnittstellen implementieren.

Ein Delegattyp stellt Verweise auf Methoden mit einer bestimmten Parameterliste und dem Rückgabetyp dar. Delegate ermöglichen die Behandlung von Methoden als Entitäten, die Variablen zugewiesen und als Parameter übergeben werden können. Delegate ähneln dem Konzept von Funktionszeigern, die Sie in einigen anderen Sprachen finden. Im Gegensatz zu Funktionszeigern sind Delegate allerdings objektorientiert und typsicher.

Klasse, Struktur, Schnittstellen- und Delegattypen-Typen unterstützen generische Typen, wobei sie mit anderen Typen parametrisiert werden können.

Ein Enumerationstyp ist ein eigenständiger Typ mit benannter Konstanten. Jeder Enumerationstyp hat einen zugrunde liegenden Typ, der eine der acht ganzzahligen Typen sein muss. Der Satz von Werten eines Enumerationstyps ist der Satz von Werten des zugrunde liegenden Typs identisch.

C# unterstützt ein- und mehrdimensionale Arrays beliebigen Typs. Im Gegensatz zu den oben aufgeführten Typen müssen Arraytypen nicht deklariert werden, bevor sie verwendet werden können. Stattdessen werden Arraytypen erstellt, indem hinter einen Typnamen eckige Klammern gesetzt werden. Z. B. `int[]` ist ein eindimensionales Array von `int`, `int[,]` wird ein zweidimensionales Array von `int`, und `int[][]` ist ein eindimensionales Array des eindimensionalen Arrays von `int`.

Auf NULL festlegbare Typen auch keine deklariert werden, bevor sie verwendet werden können. Für jeden NULL-Werte `T` wird es ein entsprechenden nullable-Typ `T?`, die einen zusätzlichen Wert enthalten kann `null`. Z. B. `int?` ist ein Typ, der alle 32-Bit-Ganzzahlwert oder den Wert aufnehmen kann `null`.

C#Typsystem ist dahingehend vereinheitlicht, dass ein Wert eines beliebigen Typs als Objekt behandelt werden kann. Jeder Typ in C# ist direkt oder indirekt vom `object`-Klassentyp abgeleitet, und `object` ist die ultimative Basisklasse aller Typen. Werte von Verweistypen werden als Objekte behandelt, indem die Werte einfach als Typ `object` angezeigt werden. Werte von Werttypen werden als Objekte behandelt, indem Sie Ausführung ***Boxing*** und ***unboxing*** Vorgänge. Im folgenden Beispiel wird ein `int`-Wert in ein `object` und wieder in einen `int`-Wert konvertiert.

```csharp
using System;

class Test
{
    static void Main() {
        int i = 123;
        object o = i;          // Boxing
        int j = (int)o;        // Unboxing
    }
}
```
Wenn der Wert eines Werttyps in den Typ konvertiert wird `object`, eine Objektinstanz, die auch "Box" genannte wird zum Speichern des Werts zugeordnet, und der Wert wird in diese Box kopiert. Im Gegensatz dazu, wann ein `object` Verweis auf einen Werttyp umgewandelt wird, wird eine Überprüfung durchgeführt, die das referenzierte Objekt ein Feld des korrekten Datentyps ist und, wenn die Überprüfung erfolgreich ist, wird der Wert in das Feld kopiert.

C#der unified Type System bedeutet, dass Werttypen "bei Bedarf." Objekte werden können Aufgrund der Vereinheitlichung können Bibliotheken für allgemeine Zwecke, die den Typ `object` verwenden, sowohl mit Verweis- als auch Werttypen verwendet werden.

Es gibt mehrere Arten von ***Variablen*** in C#, einschließlich Feldern, Arrayelementen, lokalen Variablen und Parametern. Variablen stellen Speicherorte dar, und jede Variable hat einen Typ, der bestimmt, welche Werte können in der Variablen gespeichert wird, wie in der folgenden Tabelle gezeigt.


| __Variablentyp__    | __Mögliche Inhalt__ |
|-------------------------|-----------------------|
| Nicht auf NULL festlegbarer Werttyp | Ein Wert genau dieses Typs |
| Auf NULL festlegbarer Werttyp     | Ein null-Wert oder einen Wert genau dieses Typs |
| `object`                | Ein null-Verweis, einen Verweis auf ein Objekt von einem beliebigen Verweistyp oder ein Verweis auf einen geschachtelten Wert eines beliebigen Werttyps |
| Klassentyp              | Ein null-Verweis, einen Verweis auf eine Instanz dieses Klassentyps oder ein Verweis auf eine Instanz einer Klasse abgeleitet werden, von diesem Klassentyp |
| Schnittstellentyp          | Ein null-Verweis, einen Verweis auf eine Instanz eines Klassentyps, der diesen Schnittstellentyp implementiert oder einen Verweis auf einen geschachtelten Wert eines Werttyps, der diesen Schnittstellentyp implementiert |
| Arraytyp              | Ein null-Verweis, einen Verweis auf eine Instanz dieses Arraytyps oder ein Verweis auf eine Instanz von eines kompatiblen Arraytyps |
| Delegattyp           | Ein null-Verweis oder einen Verweis auf eine Instanz dieses Delegattyps |

## <a name="expressions"></a>Ausdrücke

***Ausdrücke*** bestehen aus ***Operanden*** und ***Operatoren***. Die Operatoren eines Ausdrucks geben an, welche Operationen auf die Operanden angewendet werden. Beispiele für Operatoren sind `+`, `-`, `*`, `/` und `new`. Beispiele für Operanden sind Literale, Felder, lokale Variablen und Ausdrücke.

Wenn ein Ausdruck mehrere Operatoren enthält, bestimmt die ***Rangfolge*** der Operatoren die Reihenfolge, in der die einzelnen Operatoren ausgewertet werden. Der Ausdruck `x + y * z` wird z.B. als `x + (y * z)` ausgewertet, da der `*`-Operator Vorrang vor dem `+`-Operator hat.

Die meisten Operatoren können ***überladen*** werden. Das Überladen von Operatoren ermöglicht die Angabe benutzerdefinierter Operatorimplementierungen für Vorgänge, in denen einer der Operanden oder beide einer benutzerdefinierten Klasse oder einem benutzerdefinierten Strukturtyp angehören.

Die folgende Tabelle enthält C#Operatoren, die Operatorkategorien gemäß der Rangfolge von der höchsten zur niedrigsten Ebene auflisten. Operatoren der gleichen Kategorie haben den gleichen Rang.


| __Kategorie__                     | __Ausdruck__    | __Beschreibung__ |
|----------------------------------|-------------------|-----------------|
| Primär                          | `x.m`             | Memberzugriff |
|                                  | `x(...)`          | Methoden- und Delegataufruf |
|                                  | `x[...]`          | Array- und Indexerzugriff |
|                                  | `x++`             | Postinkrement |
|                                  | `x--`             | Postdekrement |
|                                  | `new T(...)`      | Objekt- und Delegaterstellung |
|                                  | `new T(...){...}` | Objekterstellung mit Initialisierer |
|                                  | `new {...}`       | Anonymer Objektinitialisierer |
|                                  | `new T[...]`      | Arrayerstellung |
|                                  | `typeof(T)`       | Abrufen `System.Type` für Objekt `T` |
|                                  | `checked(x)`      | Auswerten von Ausdrücken in geprüftem Kontext |
|                                  | `unchecked(x)`    | Auswerten von Ausdrücken in nicht geprüftem Kontext |
|                                  | `default(T)`      | Abrufen des Standardwerts vom Typ `T` |
|                                  | `delegate {...}`  | Anonyme Funktion (anonyme Methode) |
| Unär                            | `+x`              | Identität |
|                                  | `-x`              | Negation |
|                                  | `!x`              | Logische Negation |
|                                  | `~x`              | Bitweise Negation |
|                                  | `++x`             | Präinkrement |
|                                  | `--x`             | Prädekrement |
|                                  | `(T)x`            | Konvertieren Sie explizit `x` eingeben `T` |
|                                  | `await x`         | Asynchrones Warten auf den Abschluss von `x` |
| Multiplikativ                   | `x * y`           | Multiplikation |
|                                  | `x / y`           | Division |
|                                  | `x % y`           | Rest |
| Additiv                         | `x + y`           | Addition, Zeichenfolgenverkettung, Delegatkombination |
|                                  | `x - y`           | Subtraktion, Delegatentfernung |
| Shift                            | `x << y`          | Linksverschiebung |
|                                  | `x >> y`          | Rechtsverschiebung |
| Relational und Typtest      | `x < y`           | Kleiner als |
|                                  | `x > y`           | Größer als |
|                                  | `x <= y`          | Kleiner oder gleich |
|                                  | `x >= y`          | Größer oder gleich |
|                                  | `x is T`          | `true` zurückgeben, wenn `x` ein `T` ist, andernfalls `false` |
|                                  | `x as T`          | Zurückgeben `x` als `T`, oder `null` Wenn `x` kein `T` |
| Gleichheit                         | `x == y`          | Gleich      |
|                                  | `x != y`          | Ungleich |
| Logisches AND                      | `x & y`           | Ganzzahliges bitweises AND, boolesches logisches AND |
| Logisches XOR                      | `x ^ y`           | Ganzzahliges bitweises XOR, boolesches logisches XOR |
| Logisches OR                       | <code>x &#124; y</code> | Ganzzahliges bitweises OR, boolesches logisches OR |
| Bedingtes AND                  | `x && y`          | Wertet `y` nur, wenn `x` ist `true` |
| Bedingtes OR                   | <code>x &#124;&#124; y</code> | Wertet `y` nur, wenn `x` ist `false` |
| NULL-Sammeloperator                  | `x ?? y`          | Ergibt `y` Wenn `x` ist `null`zu `x` andernfalls |
| Bedingt                      | `x ? y : z`       | Wertet `y` Wenn `x` ist `true`, `z` Wenn `x` ist `false` |
| Zuweisung oder anonyme Funktion | `x = y`           | Zuweisung |
|                                  | `x op= y`         | Zusammengesetzte Zuweisung; unterstützte Operatoren sind `*=` `/=` `%=` `+=` `-=` `<<=` `>>=` `&=` `^=` <code>&#124;=</code> |
|                                  | `(T x) => y`      | Anonyme Funktion (Lambda-Ausdruck) |

## <a name="statements"></a>Anweisungen

Die Aktionen eines Programms werden mit ***Anweisungen*** ausgedrückt. C# unterstützt verschiedene Arten von Anweisungen, von denen ein Teil als eingebettete Anweisungen definiert ist.

Ein ***Block*** ermöglicht, mehrere Anweisungen in Kontexten zu schreiben, in denen eine einzelne Anweisung zulässig ist. Ein Block besteht aus einer Liste von Anweisungen, die zwischen den Trennzeichen `{` und `}` geschrieben sind.

***Deklarationsanweisungen*** werden verwendet, um lokale Variablen und Konstanten deklarieren.

***Ausdrucksanweisungen*** werden zum Auswerten von Ausdrücken verwendet. Ausdrücke, die als Anweisungen verwendet werden können, enthalten Methodenaufrufe, objektzuordnungen mit dem `new` Operator, Zuweisungen mit `=` und die Verbundzuweisungsoperatoren, inkrementier- und dekrementiervorgänge-Vorgänge, die unter Verwendung der `++`und `--` Operatoren und await-Ausdrücken.

***Auswahlanweisungen*** werden verwendet, um eine Anzahl von möglichen Anweisungen für die Ausführung anhand des Werts eines Ausdrucks auszuwählen. Zu dieser Gruppe gehören die `if`- und `switch`-Anweisungen.

***Iterationsanweisungen*** werden verwendet, um eine eingebettete Anweisung wiederholt auszuführen. Zu dieser Gruppe gehören die `while`-, `do`-, `for`- und `foreach`-Anweisungen.

***Sprunganweisungen*** werden verwendet, um die Steuerung zu übertragen. Zu dieser Gruppe gehören die `break`-, `continue`-, `goto`-, `throw`-, `return`- und `yield`-Anweisungen.

Mit der `try`... `catch`-Anweisung werden Ausnahmen abgefangen, die während der Ausführung eines Blocks auftreten, und mit der `try`... `finally`-Anweisung wird Finalisierungscode angegeben, der immer ausgeführt wird, unabhängig davon, ob eine Ausnahme aufgetreten ist oder nicht.

Die `checked` und `unchecked` Anweisungen werden verwendet, um den Kontext für arithmetische Operationen für ganzzahlige Typen und Konvertierungen für die überlaufüberprüfung zu steuern.

Die `lock`-Anweisung wird verwendet, um die Sperre für gegenseitigen Ausschluss für ein bestimmtes Objekt abzurufen, eine Anweisung auszuführen und die Sperre aufzuheben.

Die `using`-Anweisung wird verwendet, um eine Ressource abzurufen, eine Anweisung auszuführen und dann diese Ressource zu verwerfen.

Im folgenden finden Sie Beispiele für jede Art von Anweisung

__Deklarationen von lokalen Variablen__

```csharp
static void Main() {
   int a;
   int b = 2, c = 3;
   a = 1;
   Console.WriteLine(a + b + c);
}
```


__Deklaration von lokalen Konstanten__

```csharp
static void Main() {
    const float pi = 3.1415927f;
    const int r = 25;
    Console.WriteLine(pi * r * r);
}
```


__Ausdrucksanweisung__

```csharp
static void Main() {
    int i;
    i = 123;                // Expression statement
    Console.WriteLine(i);   // Expression statement
    i++;                    // Expression statement
    Console.WriteLine(i);   // Expression statement
}
```

__`if` Anweisung__

```csharp
static void Main(string[] args) {
    if (args.Length == 0) {
        Console.WriteLine("No arguments");
    }
    else {
        Console.WriteLine("One or more arguments");
    }
}
```


__`switch` Anweisung__

```csharp
static void Main(string[] args) {
    int n = args.Length;
    switch (n) {
        case 0:
            Console.WriteLine("No arguments");
            break;
        case 1:
            Console.WriteLine("One argument");
            break;
        default:
            Console.WriteLine("{0} arguments", n);
            break;
    }
}
```

__`while` Anweisung__

```csharp
static void Main(string[] args) {
    int i = 0;
    while (i < args.Length) {
        Console.WriteLine(args[i]);
        i++;
    }
}
```


__`do` Anweisung__

```csharp
static void Main() {
    string s;
    do {
        s = Console.ReadLine();
        if (s != null) Console.WriteLine(s);
    } while (s != null);
}
```

__`for` Anweisung__

```csharp
static void Main(string[] args) {
    for (int i = 0; i < args.Length; i++) {
        Console.WriteLine(args[i]);
    }
}
```

__`foreach` Anweisung__

```csharp
static void Main(string[] args) {
    foreach (string s in args) {
        Console.WriteLine(s);
    }
}
```

__`break` Anweisung__

```csharp
static void Main() {
    while (true) {
        string s = Console.ReadLine();
        if (s == null) break;
        Console.WriteLine(s);
    }
}
```

__`continue` Anweisung__

```csharp
static void Main(string[] args) {
    for (int i = 0; i < args.Length; i++) {
        if (args[i].StartsWith("/")) continue;
        Console.WriteLine(args[i]);
    }
}
```

__`goto` Anweisung__

```csharp
static void Main(string[] args) {
    int i = 0;
    goto check;
    loop:
    Console.WriteLine(args[i++]);
    check:
    if (i < args.Length) goto loop;
}
```

__`return` Anweisung__

```csharp
static int Add(int a, int b) {
    return a + b;
}

static void Main() {
    Console.WriteLine(Add(1, 2));
    return;
}
```

__`yield` Anweisung__

```csharp
static IEnumerable<int> Range(int from, int to) {
    for (int i = from; i < to; i++) {
        yield return i;
    }
    yield break;
}

static void Main() {
    foreach (int x in Range(-10,10)) {
        Console.WriteLine(x);
    }
}
```

__`throw` und `try` Anweisungen__

```csharp
static double Divide(double x, double y) {
    if (y == 0) throw new DivideByZeroException();
    return x / y;
}

static void Main(string[] args) {
    try {
        if (args.Length != 2) {
            throw new Exception("Two numbers required");
        }
        double x = double.Parse(args[0]);
        double y = double.Parse(args[1]);
        Console.WriteLine(Divide(x, y));
    }
    catch (Exception e) {
        Console.WriteLine(e.Message);
    }
    finally {
        Console.WriteLine("Good bye!");
    }
}
```

__`checked` und `unchecked` Anweisungen__

```csharp
static void Main() {
    int i = int.MaxValue;
    checked {
        Console.WriteLine(i + 1);        // Exception
    }
    unchecked {
        Console.WriteLine(i + 1);        // Overflow
    }
}
```

__`lock` Anweisung__

```csharp
class Account
{
    decimal balance;
    public void Withdraw(decimal amount) {
        lock (this) {
            if (amount > balance) {
                throw new Exception("Insufficient funds");
            }
            balance -= amount;
        }
    }
}
```

__`using` Anweisung__

```csharp
static void Main() {
    using (TextWriter w = File.CreateText("test.txt")) {
        w.WriteLine("Line one");
        w.WriteLine("Line two");
        w.WriteLine("Line three");
    }
}
```

## <a name="classes-and-objects"></a>Klassen und Objekte

***Klassen*** sind die grundlegendsten der C#-Typen. Eine Klasse ist eine Datenstruktur, die einen Zustand (Felder) und Aktionen (Methoden und andere Funktionsmember) in einer einzigen Einheit kombiniert. Eine Klasse stellt eine Definition für dynamisch erstellte ***Instanzen*** der Klasse, auch bekannt als ***Objekte*** bereit. Klassen unterstützen ***Vererbung*** und ***Polymorphie***. Dies sind Mechanismen, durch die ***abgeleitete Klassen*** erweitert und ***Basisklassen*** spezialisiert werden können.

Neue Klassen werden mithilfe von Klassendeklarationen erstellt. Eine Klassendeklaration beginnt mit einem Header, der die Attribute und Modifizierer der Klasse, den Namen der Klasse, die Basisklasse (sofern vorhanden) und die von der Klasse implementierten Schnittstellen angibt. Auf den Header folgt der Klassenkörper. Dieser besteht aus einer Liste der Memberdeklarationen, die zwischen den Trennzeichen `{` und `}` eingefügt werden.

Nachfolgend sehen Sie eine Deklaration einer einfachen Klasse namens `Point`:

```csharp
public class Point
{
    public int x, y;

    public Point(int x, int y) {
        this.x = x;
        this.y = y;
    }
}
```
Instanzen von Klassen werden mit dem `new`-Operator erstellt. Dieser reserviert Speicher für eine neue Instanz, ruft einen Konstruktor zum Initialisieren der Instanz auf und gibt einen Verweis auf die Instanz zurück. Die folgenden Anweisungen erstellen Sie zwei `Point` Objekte und Verweise auf diese Objekte in zwei Variablen zu speichern:

```
Point p1 = new Point(0, 0);
Point p2 = new Point(10, 20);
```
Der von einem Objekt belegte Arbeitsspeicher wird automatisch freigegeben, wenn das Objekt nicht mehr verwendet wird. Es ist weder erforderlich noch möglich, die Zuweisung von Objekten in C# explizit aufzuheben.

### <a name="members"></a>Member

Die Member einer Klasse sind entweder ***statische Member*** oder ***Instanzmember***. Statische Member gehören zu Klassen, Instanzmember gehören zu Objekten (Instanzen von Klassen).

Die folgende Tabelle enthält eine Übersicht über die Arten von Membern, die eine Klasse enthalten kann.


| __Member__   | __Beschreibung__ |
|------------  |-----------------|
| Konstanten    | Konstante Werte, die der Klasse zugeordnet sind |
| Felder       | Variablen der Klasse |
| Methoden      | Berechnungen und Aktionen, die von der Klasse ausgeführt werden |
| Eigenschaften   | Aktionen im Zusammenhang mit dem Lesen und Schreiben von benannten Eigenschaften der Klasse |
| Indexer     | Aktionen im Zusammenhang mit dem Indizieren von Instanzen der Klasse, z.B. einem Array |
| Ereignisse       | Benachrichtigungen, die von der Klasse generiert werden können |
| Operatoren    | Operatoren für Konvertierungen und Ausdrücke, die von der Klasse unterstützt werden |
| Konstruktoren | Aktionen, die zum Initialisieren von Instanzen der Klasse oder der Klasse selbst benötigt werden |
| Destruktoren  | Aktionen, die ausgeführt werden, bevor Instanzen der Klasse dauerhaft verworfen werden |
| Typen        | Geschachtelte Typen, die von der Klasse deklariert werden |

### <a name="accessibility"></a>Zugriff

Jeder Member einer Klasse ist mit einem Zugriff verknüpft, der die Regionen des Programmtexts steuert, die auf den Member zugreifen können. Es gibt fünf mögliche Formen des Zugriffs. Diese werden in der folgenden Tabelle zusammengefasst.


| __Zugriff__    | __Bedeutung__ |
|----------------------|-----------------|
| `public`             | Der Zugriff ist nicht eingeschränkt. |
| `protected`          | Der Zugriff ist auf diese Klasse oder auf von dieser Klasse abgeleitete Klassen beschränkt. |
| `internal`           | Der Zugriff ist auf dieses Programm beschränkt. |
| `protected internal` | Der Zugriff ist auf dieses Programm oder auf von dieser Klasse abgeleitete Klassen beschränkt. |
| `private`            | Der Zugriff ist auf diese Klasse beschränkt. |

### <a name="type-parameters"></a>Typparameter

Eine Klassendefinition kann einen Satz an Typparametern angeben, indem eine Liste der Typparameternamen in spitzen Klammern an den Klassennamen angehängt wird. Der Typparameter können die im Text der Klassendeklarationen verwendet werden, um der Member der Klasse zu definieren. Im folgenden Beispiel lauten die Typparameter von `Pair` `TFirst` und `TSecond`:

```csharp
public class Pair<TFirst,TSecond>
{
    public TFirst First;
    public TSecond Second;
}
```
Ein Klassentyp, der zum Akzeptieren von Typparametern deklariert ist, wird einen Typ generische Klasse aufgerufen. Struktur-, Schnittstellen- und Delegattypen können auch generisch sein.

Wenn die generische Klasse verwendet wird, müssen für jeden der Typparameter Typargumente angegeben werden:

```csharp
Pair<int,string> pair = new Pair<int,string> { First = 1, Second = "two" };
int i = pair.First;     // TFirst is int
string s = pair.Second; // TSecond is string
```
Ein generischer Typ mit den Typargumente angegeben wurden, wie z. B. `Pair<int,string>
    ` oben einen konstruierten Typ aufgerufen wird.

### <a name="base-classes"></a>Basisklassen

Eine Klassendeklaration kann eine Basisklasse angeben, indem ein Doppelpunkt und der Name der Basisklasse an den Klassennamen und die Typparameter angehängt wird. Das Auslassen einer Basisklassenspezifikation ist dasselbe wie eine Ableitung vom Typ `object`. Im folgenden Beispiel ist `Point` die Basisklasse von `Point3D`, und die Basisklasse von `Point` ist `object`:

```csharp
public class Point
{
    public int x, y;

    public Point(int x, int y) {
        this.x = x;
        this.y = y;
    }
}

public class Point3D: Point
{
    public int z;

    public Point3D(int x, int y, int z): base(x, y) {
        this.z = z;
    }
}
```
Eine Klasse erbt die Member der zugehörigen Basisklasse. Vererbung bedeutet, dass eine Klasse implizit alle Member der Basisklasse, mit Ausnahme von der Instanz und statische Konstruktoren und Destruktoren der Basisklasse enthält. Eine abgeleitete Klasse kann den geerbten Membern neue Member hinzufügen, aber die Definition eines geerbten Members kann nicht entfernt werden. Im vorherigen Beispiel erbt `Point3D` die Felder `x` und `y` von `Point`, und jede `Point3D`-Instanz enthält drei Felder: `x`, `y` und `z`.

Ein Klassentyp kann implizit in einen beliebigen zugehörigen Basisklassentyp konvertiert werden. Deshalb kann eine Variable eines Klassentyps auf eine Instanz dieser Klasse oder auf eine Instanz einer beliebigen abgeleiteten Klasse verweisen. Beispielsweise kann in den vorherigen Klassendeklarationen eine Variable vom Typ `Point` entweder auf `Point` oder auf `Point3D` verweisen:

```csharp
Point a = new Point(10, 20);
Point b = new Point3D(10, 20, 30);
```

### <a name="fields"></a>Felder

Ein Feld ist eine Variable, die mit einer Klasse oder eine Instanz einer Klasse zugeordnet ist.

Ein Feld deklariert, mit der `static` -Modifizierer definiert eine ***statisches Feld***. Ein statisches Feld identifiziert genau einen Speicherort. Unabhängig davon, wie viele Instanzen einer Klasse erstellt werden, gibt es nur eine Kopie eines statischen Felds.

Ein Feld deklariert, ohne die `static` -Modifizierer definiert eine ***Instanzenfeld***. Jede Instanz einer Klasse enthält eine separate Kopie aller Instanzfelder dieser Klasse.

Im folgenden Beispiel weist jede Instanz der `Color`-Klasse eine separate Kopie der Instanzfelder `r`, `g` und `b` auf, aber es gibt nur eine Kopie der statischen Felder `Black`, `White`, `Red`, `Green` und `Blue`:

```csharp
public class Color
{
    public static readonly Color Black = new Color(0, 0, 0);
    public static readonly Color White = new Color(255, 255, 255);
    public static readonly Color Red = new Color(255, 0, 0);
    public static readonly Color Green = new Color(0, 255, 0);
    public static readonly Color Blue = new Color(0, 0, 255);
    private byte r, g, b;

    public Color(byte r, byte g, byte b) {
        this.r = r;
        this.g = g;
        this.b = b;
    }
}
```
Wie im vorherigen Beispiel gezeigt, können ***schreibgeschützte Felder*** mit einem `readonly`-Modifizierer deklariert werden. Zuweisung zu einem `readonly` Feld kann nur als Teil einer Deklaration oder in einem Konstruktor derselben Klasse auftreten.

### <a name="methods"></a>Methoden

Eine ***Methode*** ist ein Member, das eine Berechnung oder eine Aktion implementiert, die durch ein Objekt oder eine Klasse durchgeführt werden kann. Auf ***statische Methoden*** wird über die Klasse zugegriffen. Auf ***Instanzmethoden*** wird über Instanzen der Klasse zugegriffen.

Methoden verfügen über eine (möglicherweise leere) Liste von ***Parameter***, die darstellen, Werte oder Variablenverweise an die Methode übergeben, und ein ***Rückgabetyp***, der angibt, dass der Typ des Werts berechnet und zurückgegeben, indem die Methode. Der Rückgabetyp einer Methode ist `void` , wenn sie keinen Wert zurückgibt.

Ebenso wie Typen können Methoden einen Satz an Typparametern aufweisen, für den beim Aufruf der Methode Typargumente angegeben werden müssen. Im Gegensatz zu Typen können die Typargumente häufig aus den Argumenten eines Methodenaufrufs abgeleitet werden und müssen nicht explizit angegeben werden.

Die ***Signatur*** einer Methode muss innerhalb der Klasse eindeutig sein, in der die Methode deklariert ist. Die Signatur einer Methode besteht aus dem Namen der Methode, der Anzahl von Typparametern und der Anzahl, den Modifizierern und den Typen der zugehörigen Parameter. Die Signatur einer Methode umfasst nicht den Rückgabetyp.

#### <a name="parameters"></a>Parameter

Parameter werden dazu verwendet, Werte oder Variablenverweise an Methoden zu übergeben. Die Parameter einer Methode erhalten ihre tatsächlichen Werte über ***Argumente***, die angegeben werden, wenn die Methode aufgerufen wird. Es gibt vier Arten von Parametern: Wertparameter, Verweisparameter, Ausgabeparameter und Parameterarrays.

Ein ***Wertparameter*** wird zum Übergeben von Eingabeparametern verwendet. Ein Wertparameter entspricht einer lokalen Variablen, die ihren Anfangswert von dem Argument erhält, das für den Parameter übergeben wurde. Änderungen an einem Wertparameter wirken sich nicht auf das Argument aus, das für den Parameter übergeben wurde.

Wertparameter können optional sein, indem ein Standardwert festgelegt wird, damit die zugehörigen Argumente weggelassen werden können.

Ein ***Verweisparameter*** wird sowohl für die Übergabe von Eingabe- als auch Ausgabeparametern verwendet. Das für einen Verweisparameter übergebene Argument muss eine Variable sein, und während der Ausführung der Methode repräsentiert der Verweisparameter denselben Speicherort wie die Argumentvariable. Ein Verweisparameter wird mit dem `ref`-Modifizierer deklariert. Das folgende Beispiel veranschaulicht die Verwendung des `ref`-Parameters.

```csharp
using System;

class Test
{
    static void Swap(ref int x, ref int y) {
        int temp = x;
        x = y;
        y = temp;
    }

    static void Main() {
        int i = 1, j = 2;
        Swap(ref i, ref j);
        Console.WriteLine("{0} {1}", i, j);            // Outputs "2 1"
    }
}
```
Ein ***Ausgabeparameter*** wird zum Übergeben von Ausgabeparametern verwendet. Ein Ausgabeparameter ähnelt einem Verweisparameter, mit dem Unterschied, dass der Anfangswert des vom Aufrufer bereitgestellten Arguments nicht von Bedeutung ist. Ein Ausgabeparameter wird mit dem `out`-Modifizierer deklariert. Das folgende Beispiel veranschaulicht die Verwendung des `out`-Parameters.

```csharp
using System;

class Test
{
    static void Divide(int x, int y, out int result, out int remainder) {
        result = x / y;
        remainder = x % y;
    }

    static void Main() {
        int res, rem;
        Divide(10, 3, out res, out rem);
        Console.WriteLine("{0} {1}", res, rem);    // Outputs "3 1"
    }
}
```
Ein ***Parameterarray*** ermöglicht es, eine variable Anzahl von Argumenten an eine Methode zu übergeben. Ein Parameterarray wird mit dem `params`-Modifizierer deklariert. Nur der letzte Parameter einer Methode kann ein Parameterarray sein, und es muss sich um ein eindimensionales Parameterarray handeln. Die `Write` und `WriteLine` Methoden der `System.Console` -Klasse sind gute Beispiele für die Verwendung eines Parameterarrays. Sie werden folgendermaßen deklariert.

```csharp
public class Console
{
    public static void Write(string fmt, params object[] args) {...}
    public static void WriteLine(string fmt, params object[] args) {...}
    ...
}
```
Innerhalb einer Methode mit einem Parameterarray verhält sich das Parameterarray wie ein regulärer Parameter des Arraytyps. Beim Aufruf einer Methode mit einem Parameterarray ist es jedoch möglich, entweder ein einzelnes Argument des Parameterarraytyps oder eine beliebige Anzahl von Argumenten des Elementtyps des Parameterarrays zu übergeben. Im letzteren Fall wird automatisch eine Arrayinstanz erstellt und mit den vorgegebenen Argumenten initialisiert. Dieses Beispiel:

```csharp
Console.WriteLine("x={0} y={1} z={2}", x, y, z);
```
...entspricht dem folgenden Code:

```csharp
string s = "x={0} y={1} z={2}";
object[] args = new object[3];
args[0] = x;
args[1] = y;
args[2] = z;
Console.WriteLine(s, args);
```

#### <a name="method-body-and-local-variables"></a>Methodenkörper und lokale Variablen

Der Methodenkörper gibt an, die Anweisungen ausgeführt werden, wenn die Methode aufgerufen wird.

Ein Methodenkörper kann Variablen deklarieren, die für den Aufruf der Methode spezifisch sind. Diese Variable werden ***lokale Variablen*** genannt. Die Deklaration einer lokalen Variable gibt einen Typnamen, einen Variablennamen und eventuell einen Anfangswert an. Im folgenden Beispiel wird eine lokale Variable `i` mit einem Anfangswert von 0 und einer lokalen Variablen `j` ohne Anfangswert deklariert.

```csharp
using System;

class Squares
{
    static void Main() {
        int i = 0;
        int j;
        while (i < 10) {
            j = i * i;
            Console.WriteLine("{0} x {0} = {1}", i, j);
            i = i + 1;
        }
    }
}
```
In C# muss eine lokale Variable ***definitiv zugewiesen*** sein, bevor ihr Wert abgerufen werden kann. Wenn beispielsweise die vorherige Deklaration von `i` keinen Anfangswert enthielte, würde der Compiler bei der nachfolgenden Verwendung von `i` einen Fehler melden, weil `i` zu diesen Zeitpunkten im Programm nicht definitiv zugewiesen wäre.

Eine Methode kann `return`-Anweisungen verwenden, um die Steuerung an den zugehörigen Aufrufer zurückzugeben. In einer Methode, die `void` zurückgibt, können `return`-Anweisungen keinen Ausdruck angeben. In einer Methode zurückgeben von nicht -`void`, `return` -Anweisungen müssen einen Ausdruck, der den Rückgabewert berechnet enthalten.

#### <a name="static-and-instance-methods"></a>Statische Methoden und Instanzmethoden

Eine Methode deklariert, mit einem `static` Modifizierer ist ein ***statische Methode***. Eine statische Methode führt keine Vorgänge für eine spezifische Instanz aus und kann nur direkt auf statische Member zugreifen.

Eine Methode deklariert werden, ohne eine `static` Modifizierer ist ein ***Instanzmethode***. Eine Instanzmethode führt Vorgänge für eine spezifische Instanz aus und kann sowohl auf statische Member als auch auf Instanzmember zugreifen. Auf die Instanz, für die eine Instanzmethode aufgerufen wurde, kann explizit als `this` zugegriffen werden. Es ist ein Fehler, in einer statischen Methode auf `this` zu verweisen.

Die folgende `Entity`-Klasse umfasst sowohl statische Member als auch Instanzmember.

```csharp
class Entity
{
    static int nextSerialNo;
    int serialNo;

    public Entity() {
        serialNo = nextSerialNo++;
    }

    public int GetSerialNo() {
        return serialNo;
    }

    public static int GetNextSerialNo() {
        return nextSerialNo;
    }

    public static void SetNextSerialNo(int value) {
        nextSerialNo = value;
    }
}
```
Jede `Entity`-Instanz enthält eine Seriennummer (und vermutlich weitere Informationen, die hier nicht angezeigt werden). Der `Entity`-Konstruktor (der einer Instanzmethode ähnelt) initialisiert die neue Instanz mit der nächsten verfügbaren Seriennummer. Da der Konstruktor ein Instanzmember ist, kann er sowohl auf das `serialNo`-Instanzfeld als auch auf das statische `nextSerialNo`-Feld zugreifen.

Die statischen Methoden `GetNextSerialNo` und `SetNextSerialNo` können auf das statische Feld `nextSerialNo` zugreifen, aber es wäre ein Fehler, über diese Methoden direkt auf das Instanzfeld `serialNo` zuzugreifen.

Das folgende Beispiel zeigt die Verwendung der `Entity` Klasse.

```csharp
using System;

class Test
{
    static void Main() {
        Entity.SetNextSerialNo(1000);
        Entity e1 = new Entity();
        Entity e2 = new Entity();
        Console.WriteLine(e1.GetSerialNo());           // Outputs "1000"
        Console.WriteLine(e2.GetSerialNo());           // Outputs "1001"
        Console.WriteLine(Entity.GetNextSerialNo());   // Outputs "1002"
    }
}
```
Beachten Sie, dass die statischen Methoden `SetNextSerialNo` und `GetNextSerialNo` für die Klasse aufgerufen werden, während die `GetSerialNo`-Instanzmethode für Instanzen der Klasse aufgerufen wird.

#### <a name="virtual-override-and-abstract-methods"></a>Virtuelle, überschriebene und abstrakte Methoden

Wenn die Deklaration einer Instanzmethode einen `virtual`-Modifizierer enthält, wird die Methode als ***virtuelle Methode*** bezeichnet. Wenn kein `virtual` Modifizierer vorhanden ist, wird die Methode gilt eine ***nicht virtuellen Methode***.

Beim Aufruf einer virtuellen Methode bestimmt der ***Laufzeittyp*** der Instanz, für die der Aufruf erfolgt, die tatsächlich aufzurufende Methodenimplementierung. Beim Aufruf einer nicht virtuellen Methode ist der ***Kompilierzeittyp*** der bestimmende Faktor.

Eine virtuelle Methode kann in einer abgeleiteten Klasse ***überschrieben*** werden. Wenn eine Instanzmethodendeklaration enthält ein `override` Modifizierer, die Methode überschreibt eine geerbte virtuelle Methode mit der gleichen Signatur. Während eine Deklaration einer virtuellen Methode eine neue Methode einführt, spezialisiert eine Deklaration einer überschriebenen Methode eine vorhandene geerbte virtuelle Methode, indem eine neue Implementierung dieser Methode bereitgestellt wird.

Ein ***abstrakte*** Methode ist eine virtuelle Methode ohne Implementierung. Eine abstrakte Methode ist deklariert, mit der `abstract` Modifizierer und darf nur in einer Klasse, die auch deklariert wird `abstract`. Eine abstrakte Methode muss in jeder nicht abstrakten abgeleiteten Klasse überschrieben werden.

Im folgenden Beispiel wird die abstrakte Klasse `Expression` deklariert, die einen Ausdrucksbaumstrukturknoten sowie drei abgeleitete Klassen repräsentiert: `Constant`, `VariableReference` und `Operation`. Diese implementieren Ausdrucksbaumstrukturknoten für Konstanten, variable Verweise und arithmetische Operationen. (Dieser Vorgang ähnelt, aber nicht zu verwechseln mit die ausdrucksbaumstrukturtypen, das in eingeführte [ausdrucksbaumstrukturtypen](types.md#expression-tree-types)).

```csharp
using System;
using System.Collections;

public abstract class Expression
{
    public abstract double Evaluate(Hashtable vars);
}

public class Constant: Expression
{
    double value;

    public Constant(double value) {
        this.value = value;
    }

    public override double Evaluate(Hashtable vars) {
        return value;
    }
}

public class VariableReference: Expression
{
    string name;

    public VariableReference(string name) {
        this.name = name;
    }

    public override double Evaluate(Hashtable vars) {
        object value = vars[name];
        if (value == null) {
            throw new Exception("Unknown variable: " + name);
        }
        return Convert.ToDouble(value);
    }
}

public class Operation: Expression
{
    Expression left;
    char op;
    Expression right;

    public Operation(Expression left, char op, Expression right) {
        this.left = left;
        this.op = op;
        this.right = right;
    }

    public override double Evaluate(Hashtable vars) {
        double x = left.Evaluate(vars);
        double y = right.Evaluate(vars);
        switch (op) {
            case '+': return x + y;
            case '-': return x - y;
            case '*': return x * y;
            case '/': return x / y;
        }
        throw new Exception("Unknown operator");
    }
}
```
Die vorherigen vier Klassen können zum Modellieren arithmetischer Ausdrücke verwendet werden. Beispielsweise kann mithilfe von Instanzen dieser Klassen der Ausdruck `x + 3` folgendermaßen dargestellt werden.

```csharp
Expression e = new Operation(
    new VariableReference("x"),
    '+',
    new Constant(3));
```
Die `Evaluate`-Methode einer `Expression`-Instanz wird aufgerufen, um den vorgegebenen Ausdruck auszuwerten und einen `double`-Wert zu generieren. Die Methode, die als Argument akzeptiert eine `Hashtable` , Variablennamen (als Schlüssel der Einträge) und Werte (als Werte der Einträge) enthält. Die `Evaluate` Methode ist eine virtuelle abstrakte Methode, was bedeutet, dass nicht abstrakte abgeleitete Klassen überschreiben müssen, um eine tatsächliche Implementierung bereitzustellen.

Eine Implementierung von `Constant` für `Evaluate` gibt lediglich die gespeicherte Konstante zurück. Ein `VariableReference`die Implementierung sieht nach dem Variablennamen in der Hashtabelle und gibt den Ergebniswert zurück. Eine Implementierung von `Operation` wertet zunächst (durch einen rekursiven Aufruf der zugehörigen `Evaluate`-Methoden) den linken und rechten Operanden aus und führt dann die vorgegebene arithmetische Operation aus.

Das folgende Programm verwendet die `Expression`-Klassen zum Auswerten des Ausdrucks `x * (y + 2)` für verschiedene Werte von `x` und `y`.

```csharp
using System;
using System.Collections;

class Test
{
    static void Main() {
        Expression e = new Operation(
            new VariableReference("x"),
            '*',
            new Operation(
                new VariableReference("y"),
                '+',
                new Constant(2)
            )
        );
        Hashtable vars = new Hashtable();
        vars["x"] = 3;
        vars["y"] = 5;
        Console.WriteLine(e.Evaluate(vars));        // Outputs "21"
        vars["x"] = 1.5;
        vars["y"] = 9;
        Console.WriteLine(e.Evaluate(vars));        // Outputs "16.5"
    }
}
```

#### <a name="method-overloading"></a>Methodenüberladung

Das ***Überladen*** von Methoden macht es möglich, dass mehrere Methoden in derselben Klasse denselben Namen verwenden, solange sie eindeutige Signaturen aufweisen. Beim Kompilieren des Aufrufs einer überladenen Methode verwendet der Compiler die ***Überladungsauflösung***, um die spezifische Methode zu ermitteln, die aufgerufen werden soll. Mithilfe der Überladungsauflösung wird die Methode ermittelt, die den Argumenten am besten entspricht, bzw. es wird ein Fehler ausgegeben, wenn keine passende Methode gefunden wird. Das folgende Beispiel zeigt die Verwendung der Überladungsauflösung. Der Kommentar für jeden Aufruf in der `Main`-Methode zeigt, welche Methode tatsächlich aufgerufen wird.

```csharp
class Test
{
    static void F() {
        Console.WriteLine("F()");
    }

    static void F(object x) {
        Console.WriteLine("F(object)");
    }

    static void F(int x) {
        Console.WriteLine("F(int)");
    }

    static void F(double x) {
        Console.WriteLine("F(double)");
    }

    static void F<T>(T x) {
        Console.WriteLine("F<T>(T)");
    }

    static void F(double x, double y) {
        Console.WriteLine("F(double, double)");
    }

    static void Main() {
        F();                 // Invokes F()
        F(1);                // Invokes F(int)
        F(1.0);              // Invokes F(double)
        F("abc");            // Invokes F(object)
        F((double)1);        // Invokes F(double)
        F((object)1);        // Invokes F(object)
        F<int>(1);           // Invokes F<T>(T)
        F(1, 1);             // Invokes F(double, double)
    }
}
```
Wie im Beispiel gezeigt, kann eine bestimmte Methode immer ausgewählt werden, indem die Argumente explizit in die passenden Parametertypen konvertiert und/oder explizit Typargumente angegeben werden.

### <a name="other-function-members"></a>Andere Funktionsmember

Member, die ausführbaren Code enthalten, werden als ***Funktionsmember*** einer Klasse bezeichnet. In den vorangegangenen Abschnitten wurden die Methoden beschrieben, die wichtigste Form der Funktionsmember. In diesem Abschnitt wird beschrieben, die andere Arten von Funktionsmembern von c# unterstützt: Konstruktoren, Eigenschaften, Indexer, Ereignisse, Operatoren und Destruktoren.

Der folgende Code zeigt eine generische Klasse namens `List<T>`, die eine wachsende Liste von Objekten implementiert. Die Klasse enthält verschiedene Beispiele der gängigsten Arten von Funktionsmembern.


```csharp
public class List<T> {
    // Constant...
    const int defaultCapacity = 4;

    // Fields...
    T[] items;
    int count;

    // Constructors...
    public List(int capacity = defaultCapacity) {
        items = new T[capacity];
    }

    // Properties...
    public int Count {
        get { return count; }
    }
    public int Capacity {
        get {
            return items.Length;
        }
        set {
            if (value < count) value = count;
            if (value != items.Length) {
                T[] newItems = new T[value];
                Array.Copy(items, 0, newItems, 0, count);
                items = newItems;
            }
        }
    }

    // Indexer...
    public T this[int index] {
        get {
            return items[index];
        }
        set {
            items[index] = value;
            OnChanged();
        }
    }

    // Methods...
    public void Add(T item) {
        if (count == Capacity) Capacity = count * 2;
        items[count] = item;
        count++;
        OnChanged();
    }
    protected virtual void OnChanged() {
        if (Changed != null) Changed(this, EventArgs.Empty);
    }
    public override bool Equals(object other) {
        return Equals(this, other as List<T>);
    }
    static bool Equals(List<T> a, List<T> b) {
        if (a == null) return b == null;
        if (b == null || a.count != b.count) return false;
        for (int i = 0; i < a.count; i++) {
            if (!object.Equals(a.items[i], b.items[i])) {
                return false;
            }
        }
        return true;
    }

    // Event...
    public event EventHandler Changed;

    // Operators...
    public static bool operator ==(List<T> a, List<T> b) {
        return Equals(a, b);
    }
    public static bool operator !=(List<T> a, List<T> b) {
        return !Equals(a, b);
    }
}
```

#### <a name="constructors"></a>Konstruktoren

C# unterstützt sowohl Instanzkonstruktoren als auch statische Konstruktoren. Ein ***Instanzkonstruktor*** ist ein Member, der die erforderlichen Aktionen zum Initialisieren einer Instanz einer Klasse implementiert. Ein ***statischer Konstruktor*** ist ein Member, der die zum Initialisieren einer Klasse erforderlichen Aktionen implementiert, um die Klasse beim ersten Laden selbst zu initialisieren.

Ein Konstruktor wird wie eine Methode ohne Rückgabetyp und mit demselben Namen wie die enthaltende Klasse deklariert. Wenn eine Konstruktordeklaration verwendet enthält eine `static` Modifizierer verwenden, wird einen statischen Konstruktor deklariert. Andernfalls wird ein Instanzkonstruktor deklariert.

Instanzkonstruktoren können überladen werden. Beispielsweise deklariert die `List<T>
`-Klasse zwei Instanzkonstruktoren, einen ohne Parameter und einen weiteren mit einem `int`-Parameter. Instanzkonstruktoren werden über den `new`-Operator aufgerufen. Die folgenden Anweisungen weisen zwei `List<string>
` Instanzen, indem Sie die Konstruktoren der `List` Klasse.

```csharp
List<string> list1 = new List<string>();
List<string> list2 = new List<string>(10);
```
Im Gegensatz zu anderen Members werden Instanzkonstruktoren nicht geerbt, und eine Klasse weist keine anderen Instanzkonstruktoren auf als diejenigen, die tatsächlich in der Klasse deklariert wurden. Wenn kein Instanzkonstruktor für eine Klasse angegeben ist, wird automatisch ein leerer Instanzkonstruktor ohne Parameter bereitgestellt.

#### <a name="properties"></a>Eigenschaften

***Eigenschaften*** sind eine natürliche Erweiterung der Felder. Beide sind benannte Member mit zugeordneten Typen, und für den Zugriff auf Felder und Eigenschaften wird dieselbe Syntax verwendet. Im Gegensatz zu Feldern bezeichnen Eigenschaften jedoch keine Speicherorte. Stattdessen verfügen Eigenschaften über ***Accessors*** zum Angeben der Anweisungen, die beim Lesen oder Schreiben ihrer Werte ausgeführt werden sollen.

Eine Eigenschaft wird wie ein Feld deklariert, außer dass die Deklaration mit endet eine `get` Accessor und/oder einen `set` Accessor, die zwischen den Trennzeichen `{` und `}` statt einem Semikolon endet. Eine Eigenschaft, die beides aufweist eine `get` Accessor und einen `set` -Accessor ist eine ***Lese-/ Schreibzugriff-Eigenschaft***, eine Eigenschaft, die nur eine `get` -Accessor ist eine ***schreibgeschützte Eigenschaft***, und ein Eigenschaft, die nur eine `set` -Accessor ist eine ***lesegeschützte Eigenschaft***.

Ein `get` -Accessor entspricht einer Methode ohne Parameter mit einem Rückgabewert des Eigenschaftstyps. Eine Eigenschaft in einem Ausdruck verwiesen wird, wird als Ziel einer Zuweisung, außer die `get` -Accessor der Eigenschaft wird aufgerufen, um den Wert der Eigenschaft zu berechnen.

Ein `set` Accessor entspricht einer Methode mit einem einzigen Parameter, die mit dem Namen `value` und keinen Rückgabewert. Wenn eine Eigenschaft verwiesen wird, als das Ziel einer Zuweisung oder als Operand `++` oder `--`, `set` -Accessor wird aufgerufen, mit der ein Argument, das den neuen Wert bereitstellt.

Die `List<T>
`-Klasse deklariert die beiden Eigenschaften „`Count`“ und „`Capacity`“, von denen die eine schreibgeschützt ist und die andere Lese- und Schreibzugriff besitzt. Es folgt ein Beispiel zur Verwendung dieser Eigenschaften.

```csharp
List<string> names = new List<string>();
names.Capacity = 100;            // Invokes set accessor
int i = names.Count;             // Invokes get accessor
int j = names.Capacity;          // Invokes get accessor
```
Ähnlich wie bei Feldern und Methoden unterstützt C# sowohl Instanzeigenschaften als auch statische Eigenschaften. Statische Eigenschaften werden deklariert, mit der `static` Modifizierer und Instanzeigenschaften werden ohne ihn deklariert.

Die Accessors einer Eigenschaft können virtuell sein. Wenn eine Eigenschaftendeklaration einen `virtual`-, `abstract`- oder `override`-Modifizierer enthält, wird dieser auf den Accessor der Eigenschaft angewendet.

#### <a name="indexers"></a>Indexer

Ein ***Indexer*** ist ein Member, mit dem Objekte wie ein Array indiziert werden können. Ein Indexer wird wie eine Eigenschaft deklariert, mit dem Unterschied, dass der Name des Members `this` gefolgt von einer Parameterliste, die zwischen den Trennzeichen `[` und `]`. Die Parameter stehen im Accessor des Indexers zur Verfügung. Ähnlich wie Eigenschaften können Indexer Lese-/Schreibzugriff besitzen, schreibgeschützt und lesegeschützt sein und virtuelle Accessors verwenden.

Die `List`-Klasse deklariert einen einzigen Indexer mit Lese-/Schreibzugriff, der einen `int`-Parameter akzeptiert. Der Indexer ermöglicht es, Instanzen von `List` mit `int`-Werten zu indizieren. Beispiel:

```csharp
List<string> names = new List<string>();
names.Add("Liz");
names.Add("Martha");
names.Add("Beth");
for (int i = 0; i < names.Count; i++) {
    string s = names[i];
    names[i] = s.ToUpper();
}
```
Indexer können überladen werden, d.h. eine Klasse kann mehrere Indexer deklarieren, solange sich die Anzahl oder Typen ihrer Parameter unterscheiden.

#### <a name="events"></a>Ereignisse

Ein ***Ereignis*** ist ein Member, der es einer Klasse oder einem Objekt ermöglicht, Benachrichtigungen bereitzustellen. Ein Ereignis wird wie ein Feld deklariert, mit dem Unterschied, dass die Deklaration enthält eine `event` -Schlüsselwort und der Typ müssen ein Delegattyp sein.

Innerhalb einer Klasse, die einen Ereignismember deklariert, verhält sich das Ereignis wie ein Feld des Delegattyps (vorausgesetzt, das Ereignis ist nicht abstrakt und deklariert keine Accessors). Das Feld speichert einen Verweis auf einen Delegaten, der die Ereignishandler repräsentiert, die dem Ereignis hinzugefügt wurden. Wenn keine Handles für ein Ereignis vorhanden sind, wird das Feld `null`.

Die `List<T>
`-Klasse deklariert einen einzigen Ereignismember namens `Changed`, der angibt, dass der Liste ein neues Element hinzugefügt wurde. Die `Changed` Ereignis wird ausgelöst, durch die `OnChanged` virtuelle Methode, die zunächst prüft, ob das Ereignis `null` (d. h., dass keine Handler vorhanden sind). Das Auslösen eines Ereignisses entspricht exakt dem Aufrufen des Delegaten, der durch das Ereignis repräsentiert wird, es gibt deshalb keine besonderen Sprachkonstrukte zum Auslösen von Ereignissen.

Clients reagieren über ***Ereignishandler*** auf Ereignisse. Ereignishandler werden unter Verwendung des `+=`-Operators angefügt und mit dem `-=`-Operator entfernt. Im folgenden Beispiel wird dem `Changed`-Ereignis von `List<string>
` ein Ereignishandler hinzugefügt.

```csharp
using System;

class Test
{
    static int changeCount;

    static void ListChanged(object sender, EventArgs e) {
        changeCount++;
    }

    static void Main() {
        List<string> names = new List<string>();
        names.Changed += new EventHandler(ListChanged);
        names.Add("Liz");
        names.Add("Martha");
        names.Add("Beth");
        Console.WriteLine(changeCount);        // Outputs "3"
    }
}
```
In komplexeren Szenarien, in denen die zugrunde liegende Speicherung eines Ereignisses gesteuert werden soll, können in einer Ereignisdeklaration explizit die `add`- und `remove`-Accessors bereitgestellt werden. Diese ähneln in gewisser Weise dem `set`-Accessor einer Eigenschaft.

#### <a name="operators"></a>Operatoren

Ein ***Operator*** ist ein Member, der die Bedeutung der Anwendung eines bestimmten Ausdrucksoperators auf Instanzen einer Klasse definiert. Es können drei Arten von Operatoren definiert werden: unäre Operatoren, binäre Operatoren und Konvertierungsoperatoren. Alle Operatoren müssen als `public` und `static` deklariert werden.

Die `List<T>
`-Klasse deklariert zwei Operatoren, `operator==` und `operator!=`, und verleiht so Ausdrücken, die diese Operatoren auf `List`-Instanzen anwenden, eine neue Bedeutung. Insbesondere die Operatoren definieren die Gleichheit von zwei `List<T>
` als Vergleich aller enthaltenen Objekte mithilfe von Instanzen ihrer `Equals` Methoden. Im folgenden Beispiel wird der `==`-Operator verwendet, um zwei Instanzen von `List<int>
` zu vergleichen.

```csharp
using System;

class Test
{
    static void Main() {
        List<int> a = new List<int>();
        a.Add(1);
        a.Add(2);
        List<int> b = new List<int>();
        b.Add(1);
        b.Add(2);
        Console.WriteLine(a == b);        // Outputs "True"
        b.Add(3);
        Console.WriteLine(a == b);        // Outputs "False"
    }
}
```

Die erste Methode `Console.WriteLine` gibt `True` aus, weil die zwei Listen dieselbe Anzahl von Objekten mit denselben Werten in derselben Reihenfolge enthalten. Wenn `List<T>
` nicht `operator==` definieren würde, würde die Ausgabe der ersten `Console.WriteLine`-Methode `False` lauten, weil `a` und `b` auf unterschiedliche `List<int>
`-Instanzen verweisen.

#### <a name="destructors"></a>Destruktoren

Ein ***Destruktor*** ist ein Element, das die erforderlichen Aktionen zum zerstört einer Instanz einer Klasse implementiert. Destruktoren können keine Parameter haben, sie können keine Zugriffsmodifizierer aufweisen und sie können nicht explizit aufgerufen werden. Der Destruktor für eine Instanz wird automatisch während der automatischen speicherbereinigung aufgerufen.

Der Garbage Collector kann entscheiden, wann Objekte sammeln und Destruktoren ausführen. Insbesondere der Zeitpunkt der Destruktor aufrufen, ist nicht deterministisch und Destruktoren können in jedem Thread ausgeführt werden. Für diesen und anderen Gründen sollten Klassen mit Destruktoren implementieren, nur, wenn keine andere Lösung möglich ist.

Die `using`-Anweisung bietet einen besseren Ansatz für die Objektzerstörung.

## <a name="structs"></a>Strukturen

Wie Klassen sind ***Strukturen*** Datenstrukturen, die Datenmember und Funktionsmember enthalten können, aber im Gegensatz zu Klassen sind Strukturen Werttypen und erfordern keine Heapzuordnung. Eine Variable eines Strukturtyps speichert die Daten der Struktur direkt, während eine Variable eines Klassentyps einen Verweis auf ein dynamisch zugeordnetes Objekt speichert. Strukturtypen unterstützen keine benutzerdefinierte Vererbung, und alle Strukturtypen erben implizit vom Typ `object`.

Strukturen sind besonders nützlich für kleine Datenstrukturen, die über Wertsemantik verfügen. Komplexe Zahlen, Punkte in einem Koordinatensystem oder Schlüssel-Wert-Paare im Wörterbuch sind gute Beispiele für Strukturen. Die Verwendung von Strukturen statt Klassen für kleine Datenstrukturen kann bei der Anzahl der Speicherbelegungen, die eine Anwendung durchführt, einen großen Unterschied ausmachen. Das folgende Programm erstellt und initialisiert z.B. ein Array aus 100 Punkten. Mit `Point` als implementierter Klasse werden 101 separate Objekte instanziiert – eines für das Array und jeweils eines für jedes der 100 Elemente.

```csharp
class Point
{
    public int x, y;

    public Point(int x, int y) {
        this.x = x;
        this.y = y;
    }
}

class Test
{
    static void Main() {
        Point[] points = new Point[100];
        for (int i = 0; i < 100; i++) points[i] = new Point(i, i);
    }
}
```
Eine Alternative ist, `Point` eine Struktur.

```csharp
struct Point
{
    public int x, y;

    public Point(int x, int y) {
        this.x = x;
        this.y = y;
    }
}
```
Jetzt wird nur ein Objekt instanziiert – für das Array – und die `Point`-Instanzen werden inline im Array gespeichert.

Strukturkonstruktoren werden mit dem neuen Operator `new` aufgerufen, doch das bedeutet nicht, dass der Arbeitsspeicher belegt wird. Statt ein Objekt dynamisch zuzuordnen und einen Verweis darauf zurückzugeben, gibt ein Strukturkonstruktor einfach den Strukturwert selbst zurück (in der Regel in einen temporären Speicherort auf dem Stapel), und dieser Wert wird dann nach Bedarf kopiert.

Mit Klassen können zwei Variablen auf das gleiche Objekt verweisen, und so können an einer Variablen durchgeführte Vorgänge das Objekt beeinflussen, auf das die andere Variable verweist. Mit Strukturen besitzt jede Variable eine eigene Kopie der Daten, und es ist nicht möglich, dass an einer Variablen durchgeführte Vorgänge die andere beeinflussen. Zum Beispiel die Ausgabe durch das folgende Codefragment davon abhängig, ob `Point` ist eine Klasse oder Struktur.

```csharp
Point a = new Point(10, 10);
Point b = a;
a.x = 20;
Console.WriteLine(b.x);
```
Wenn `Point` ist eine Klasse, die Ausgabe ist `20` da `a` und `b` auf das gleiche Objekt verweisen. Wenn `Point` ist eine Struktur, die Ausgabe ist `10` da die Zuweisung von `a` zu `b` erstellt eine Kopie des Werts, und diese Kopie ist nicht betroffen von der nachfolgenden Zuweisung zu `a.x`.

Im vorherigen Beispiel werden zwei der Einschränkungen von Strukturen hervorgehoben. Erstens ist das Kopieren einer gesamten Struktur in der Regel weniger effizient als das Kopieren eines Objektverweises, sodass Zuweisung und Wertparameterübergabe mit Strukturen aufwändiger sein kann als mit Verweistypen. Zweitens ist es mit Ausnahme der `ref`- und `out`-Parameter nicht möglich, Verweise auf Strukturen zu erstellen, was ihre Verwendung in einer Reihe von Situationen ausschließt.

## <a name="arrays"></a>Arrays

Ein ***Array*** ist eine Datenstruktur, die eine Anzahl von Variablen enthält, auf die über berechnete Indizes zugegriffen wird. Die im Array enthaltenen Variablen, auch ***Elemente*** des Arrays genannt, weisen alle denselben Typ auf. Dieser Typ wird als ***Elementtyp*** des Arrays bezeichnet.

Arraytypen sind Verweistypen, und die Deklaration einer Arrayvariablen reserviert Speicher für einen Verweis auf eine Arrayinstanz. Die tatsächlichen Arrayinstanzen werden dynamisch erstellt, zur Laufzeit mithilfe der `new` Operator. Die `new` Vorgang gibt die ***Länge*** der neuen Arrayinstanz, die dann für die Lebensdauer der Instanz behoben wird. Die Indizes der Arrayelemente reichen von `0` bis `Length - 1`. Der `new`-Operator initialisiert die Elemente eines Arrays automatisch mit ihren Standardwerten. Dieser lautet z.B. für alle numerischen Typen 0 und für alle Verweistypen `null`.

Im folgenden Beispiel wird ein Array aus `int`-Elementen erstellt. Anschließend wird das Array initialisiert und die Inhalte des Arrays werden gedruckt.

```csharp
using System;

class Test
{
    static void Main() {
        int[] a = new int[10];
        for (int i = 0; i < a.Length; i++) {
            a[i] = i * i;
        }
        for (int i = 0; i < a.Length; i++) {
            Console.WriteLine("a[{0}] = {1}", i, a[i]);
        }
    }
}
```
Mit diesem Beispiel wird ein ***eindimensionales Array*** erstellt und verwendet. C# unterstützt auch ***mehrdimensionale Arrays***. Die Anzahl von Dimensionen eines Arraytyps, auch als ***Rang*** des Arraytyps bezeichnet, ist 1 plus die Anzahl von Kommas, die innerhalb der eckigen Klammern des Arraytyps angegeben ist. Im folgende Beispiel weist ein eindimensionales, ein zweidimensionales und ein dreidimensionales Array.

```csharp
int[] a1 = new int[10];
int[,] a2 = new int[10, 5];
int[,,] a3 = new int[10, 5, 2];
```
Das `a1`-Array enthält 10 Elemente, das `a2`-Array umfasst 50 (10 × 5) Elemente, und das `a3`-Array enthält 100 (10 × 5 × 2) Elemente.

Ein Array kann einen beliebigen Elementtyp verwenden, einschließlich eines Arraytyps.  Ein Array mit Elementen eines Arraytyps wird auch als ***verzweigtes Array*** bezeichnet, weil die Länge der Elementarrays nicht identisch sein muss. Im folgenden Beispiel wird ein Array aus `int`-Arrays zugewiesen:

```csharp
int[][] a = new int[3][];
a[0] = new int[10];
a[1] = new int[5];
a[2] = new int[20];
```
In der ersten Zeile wird ein Array mit drei Elementen erstellt, das jeweils den Typ `int[]` und einen Anfangswert von `null` aufweist. In den folgenden Zeilen werden die drei Elemente mit Verweisen auf einzelne Arrayinstanzen unterschiedlicher Länge initialisiert.

Die `new` Operator erlaubt es, die Anfangswerte der Arrayelemente unter Verwendung einer ***Arrayinitialisierer***, dies ist eine Liste von Ausdrücken, die zwischen den Trennzeichen `{` und `}`. Mit dem folgenden Beispiel wird ein `int[]` mit drei Elementen zugewiesen und initialisiert.

```csharp
int[] a = new int[] {1, 2, 3};
```
Beachten Sie, dass die Länge des Arrays, von der Anzahl von Ausdrücken zwischen abgeleitet wird `{` und `}`. Deklarationen lokaler Variablen und Felder können weiter verkürzt werden, sodass der Arraytyp nicht erneut aufgeführt werden muss.

```csharp
int[] a = {1, 2, 3};
```
Die zwei vorherigen Beispiele entsprechen dem folgenden:

```csharp
int[] t = new int[3];
t[0] = 1;
t[1] = 2;
t[2] = 3;
int[] a = t;
```
## <a name="interfaces"></a>Schnittstellen

Eine ***Schnittstelle*** definiert einen Vertrag, der von Klassen und Strukturen implementiert werden kann. Eine Schnittstelle kann Methoden, Eigenschaften, Ereignisse und Indexer enthalten. Eine Schnittstelle stellt keine Implementierungen der von ihr definierten Member bereit. Sie gibt lediglich die Member an, die von Klassen oder Strukturen bereitgestellt werden müssen, die die Schnittstelle implementieren.

Schnittstellen können ***Mehrfachvererbung*** einsetzen. Im folgenden Beispiel erbt die Schnittstelle `IComboBox` sowohl von `ITextBox` als auch `IListBox`.

```csharp
interface IControl
{
    void Paint();
}

interface ITextBox: IControl
{
    void SetText(string text);
}

interface IListBox: IControl
{
    void SetItems(string[] items);
}

interface IComboBox: ITextBox, IListBox {}
```
Klassen und Strukturen können mehrere Schnittstellen implementieren. Im folgenden Beispiel implementiert die Klasse `EditBox` sowohl `IControl` als auch `IDataBound`.

```csharp
interface IDataBound
{
    void Bind(Binder b);
}

public class EditBox: IControl, IDataBound
{
    public void Paint() {...}
    public void Bind(Binder b) {...}
}
```
Wenn eine Klasse oder Struktur eine bestimmte Schnittstelle implementiert, können Instanzen dieser Klasse oder Struktur implizit in diesen Schnittstellentyp konvertiert werden. Beispiel:

```csharp
EditBox editBox = new EditBox();
IControl control = editBox;
IDataBound dataBound = editBox;
```
In Fällen, in denen nicht bekannt ist, dass eine Instanz eine bestimmte Schnittstelle implementiert, können dynamische Typumwandlungen verwendet werden. Beispielsweise verwenden die folgenden Anweisungen dynamische Typumwandlungen zum Abrufen eines Objekts `IControl` und `IDataBound` schnittstellenimplementierungen. Da der tatsächliche Typ des Objekts ist `EditBox`, die Umwandlungen erfolgreich.

```csharp
object obj = new EditBox();
IControl control = (IControl)obj;
IDataBound dataBound = (IDataBound)obj;
```
In der vorherigen `EditBox` -Klasse, die `Paint` Methode aus der `IControl` Schnittstelle und die `Bind` Methode aus der `IDataBound` Schnittstelle implementiert werden, mithilfe von `public` Member. C# unterstützt außerdem ***explizite Implementierungen eines Schnittstellenmembers***, verwenden die Klasse oder Struktur kann vermeiden, dass die Mitglieder `public`. Eine explizite Implementierung eines Schnittstellenmembers wird mit dem vollqualifizierten Namen des Schnittstellenmembers geschrieben. Die `EditBox`-Klasse könnte z.B. die `IControl.Paint`- und `IDataBound.Bind`-Methode wie folgt über explizite Implementierungen eines Schnittstellenmembers implementieren.

```csharp
public class EditBox: IControl, IDataBound
{
    void IControl.Paint() {...}
    void IDataBound.Bind(Binder b) {...}
}
```
Der Zugriff auf explizite Schnittstellenmember kann nur über den Schnittstellentyp erfolgen. Z. B. die Implementierung der `IControl.Paint` bereitgestellt, die von der vorherigen `EditBox` Klasse kann nur aufgerufen werden, indem zunächst die `EditBox` Verweis auf die `IControl` Schnittstellentyp.

```csharp
EditBox editBox = new EditBox();
editBox.Paint();                        // Error, no such method
IControl control = editBox;
control.Paint();                        // Ok
```

## <a name="enums"></a>Enumerationen

Ein ***Enumerationstyp*** ist ein eindeutiger Werttyp mit einem Satz benannter Konstanten. Das folgende Beispiel deklariert und verwendet einen Enumerationstyp, der mit dem Namen `Color` mit drei Konstanten Werten `Red`, `Green`, und `Blue`.

```csharp
using System;

enum Color
{
    Red,
    Green,
    Blue
}

class Test
{
    static void PrintColor(Color color) {
        switch (color) {
            case Color.Red:
                Console.WriteLine("Red");
                break;
            case Color.Green:
                Console.WriteLine("Green");
                break;
            case Color.Blue:
                Console.WriteLine("Blue");
                break;
            default:
                Console.WriteLine("Unknown color");
                break;
        }
    }

    static void Main() {
        Color c = Color.Red;
        PrintColor(c);
        PrintColor(Color.Blue);
    }
}
```
Jeder Enumerationstyp hat einen entsprechenden ganzzahligen Typ, der Namen der ***zugrunde liegender Typ*** des Enum-Typs. Ein Enumerationstyp, der nicht explizit einen zugrunde liegenden Typ deklariert, hat einen zugrunde liegenden Typ `int`. Speicherformat und den Bereich der möglichen Werte eines Enumerationstyps werden durch die zugrunde liegenden Typ bestimmt. Der Satz von Werten, die ein Enum-Typ übernehmen kann, ist nicht durch die Enumerationsmember beschränkt. Insbesondere ein Wert für den zugrunde liegenden Typ einer Enumeration in Enum-Typs umgewandelt werden kann und ein eindeutiger gültiger Wert dieses Enum-Typs ist.

Das folgende Beispiel deklariert einen Enum-Typ, der mit dem Namen `Alignment` mit einem zugrunde liegenden Typs `sbyte`.

```csharp
enum Alignment: sbyte
{
    Left = -1,
    Center = 0,
    Right = 1
}
```
Wie im vorherigen Beispiel gezeigt wird, zählen eine Enum-Element-Deklaration einen konstanten Ausdruck, der den Wert des Members angibt. Der Konstante Wert für jede Enum-Element muss im Bereich von der zugrunde liegende Typ der Enumeration. Wenn eine Enum-Memberdeklaration nicht explizit einen Wert angeben, erhält das Element den Wert 0 (null), (wenn es das erste Element in der Enumerationstyp ist) oder den Wert des textlich vorausgehenden Enum-Element plus 1.

Enum-Werte können in ganzzahlige Werte und umgekehrt mit Typumwandlungen werden. Beispiel:

```csharp
int i = (int)Color.Blue;        // int i = 2;
Color c = (Color)2;             // Color c = Color.Blue;
```
Der Standardwert eines beliebigen Enumerationstyps ist der ganzzahlige Wert 0 (null), die in den Enumerationstyp konvertiert. In Fällen, in denen Variablen automatisch auf einen Standardwert initialisiert werden, ist dies der Wert, der Variablen von Enum-Typen. In der Reihenfolge für den Standardwert eines Enumerationstyps leicht verfügbar ist, ist das Literal `0` implizit in einen beliebigen Enumerationstyp konvertiert. Daher ist Folgendes zugelassen.

```csharp
Color c = 0;
```

## <a name="delegates"></a>Delegaten

Ein ***Delegattyp*** stellt Verweise auf Methoden mit einer bestimmten Parameterliste und dem Rückgabetyp dar. Delegate ermöglichen die Behandlung von Methoden als Entitäten, die Variablen zugewiesen und als Parameter übergeben werden können. Delegate ähneln dem Konzept von Funktionszeigern, die Sie in einigen anderen Sprachen finden. Im Gegensatz zu Funktionszeigern sind Delegate allerdings objektorientiert und typsicher.

Im folgenden Beispiel wird ein Delegattyp namens `Function` deklariert und verwendet.

```csharp
using System;

delegate double Function(double x);

class Multiplier
{
    double factor;

    public Multiplier(double factor) {
        this.factor = factor;
    }

    public double Multiply(double x) {
        return x * factor;
    }
}

class Test
{
    static double Square(double x) {
        return x * x;
    }

    static double[] Apply(double[] a, Function f) {
        double[] result = new double[a.Length];
        for (int i = 0; i < a.Length; i++) result[i] = f(a[i]);
        return result;
    }

    static void Main() {
        double[] a = {0.0, 0.5, 1.0};
        double[] squares = Apply(a, Square);
        double[] sines = Apply(a, Math.Sin);
        Multiplier m = new Multiplier(2.0);
        double[] doubles =  Apply(a, m.Multiply);
    }
}
```
Eine Instanz des Delegattyps `Function` kann auf jede Methode verweisen, die ein `double`-Argument und einen `double`-Wert akzeptiert. Die `Apply` Methode gilt eine angegebenen `Function` für die Elemente der ein `double[]`, zurückgeben eine `double[]` mit den Ergebnissen. In der `Main`-Methode wird `Apply` verwendet, um drei verschiedene Funktionen auf ein `double[]` anzuwenden.

Ein Delegat kann entweder auf eine statische Methode verweisen (z.B. `Square` oder `Math.Sin` im vorherigen Beispiel) oder eine Instanzmethode (z.B. `m.Multiply` im vorherigen Beispiel). Ein Delegat, der auf eine Instanzmethode verweist, verweist auch auf ein bestimmtes Objekt, und wenn die Instanzmethode durch den Delegaten aufgerufen wird, wird das Objekt `this` im Aufruf.

Delegaten können auch mit anonymen Funktionen erstellt werden, die dynamisch erstellte „Inlinemethoden“ sind. Anonyme Funktionen können die lokalen Variablen der umgebenden Methoden sehen. Daher das obige multiplikatorbeispiel geschrieben werden kann leichter ohne Verwendung einer `Multiplier` Klasse:

```csharp
double[] doubles =  Apply(a, (double x) => x * 2.0);
```
Eine interessante und nützliche Eigenschaft eines Delegaten ist, dass er die Klasse der Methode, auf die er verweist, nicht kennt oder sie ignoriert; wichtig ist nur, dass die referenzierte Methode die gleichen Parameter und den gleichen Rückgabetyp hat wie der Delegat.

## <a name="attributes"></a>Attribute

Typen, Member und andere Entitäten in einem C#-Programm unterstützen Modifizierer, die bestimmte Aspekte ihres Verhaltens steuern. Der Zugriff auf eine Methode wird beispielsweise mithilfe der Modifizierer `public`, `protected`, `internal` und `private` kontrolliert. C# generalisiert diese Funktionalität, indem benutzerdefinierte Typen deklarativer Informationen an eine Programmentität angefügt und zur Laufzeit abgerufen werden können. Programm geben diese zusätzlichen deklarativen Informationen durch das Definieren und Verwenden von ***Attributen*** an.

Im folgenden Beispiel wird ein `HelpAttribute`-Attribut deklariert, dass in Programmentitäten platziert werden kann, um Links zur zugehörigen Dokumentation bereitzustellen.

```csharp
using System;

public class HelpAttribute: Attribute
{
    string url;
    string topic;

    public HelpAttribute(string url) {
        this.url = url;
    }

    public string Url {
        get { return url; }
    }

    public string Topic {
        get { return topic; }
        set { topic = value; }
    }
}
```
Alle Attributklassen leiten Sie von der `System.Attribute` Basisklasse, die von .NET Framework bereitgestellt. Attribute können durch Angabe ihres Namens angewendet werden, zusammen mit beliebigen Argumenten. Diese müssen in eckigen Klammern genau vor der zugehörigen Deklaration eingefügt werden. Wenn Sie den Namen eines Attributs endet `Attribute`, dieser Teil des Namens kann ausgelassen werden, wenn das Attribut verwiesen wird. Beispielsweise kann das `HelpAttribute`-Attribut wie folgt verwendet werden.

```csharp
[Help("http://msdn.microsoft.com/.../MyClass.htm")]
public class Widget
{
    [Help("http://msdn.microsoft.com/.../MyClass.htm", Topic = "Display")]
    public void Display(string text) {}
}
```
In diesem Beispiel fügt eine `HelpAttribute` auf die `Widget` -Klasse und eine weitere `HelpAttribute` auf die `Display` Methode in der Klasse. Die öffentlichen Konstruktoren einer Attributklasse steuern die Informationen, die beim Anfügen des Attributs an eine Programmentität angegeben werden müssen. Zusätzliche Informationen können angegeben werden, indem auf öffentliche Eigenschaften mit Lese-/Schreibzugriff der Attributklasse verwiesen wird (z.B. wie der obige Verweis auf die `Topic`-Eigenschaft).

Das folgende Beispiel zeigt, wie Informationen über Bildattribute für ein bestimmtes Programmentität zur Laufzeit abgerufen werden kann mithilfe von Reflektion.

```csharp
using System;
using System.Reflection;

class Test
{
    static void ShowHelp(MemberInfo member) {
        HelpAttribute a = Attribute.GetCustomAttribute(member,
            typeof(HelpAttribute)) as HelpAttribute;
        if (a == null) {
            Console.WriteLine("No help for {0}", member);
        }
        else {
            Console.WriteLine("Help for {0}:", member);
            Console.WriteLine("  Url={0}, Topic={1}", a.Url, a.Topic);
        }
    }

    static void Main() {
        ShowHelp(typeof(Widget));
        ShowHelp(typeof(Widget).GetMethod("Display"));
    }
}
```
Wenn per Reflektion ein bestimmtes Attribut angefordert wird, wird der Konstruktor für die Attributklasse mit den in der Programmquelle angegebenen Informationen aufgerufen, und die resultierende Attributinstanz wird zurückgegeben. Wenn zusätzliche Informationen über Eigenschaften bereitgestellt wurden, werden diese Eigenschaften auf die vorgegebenen Werte festgelegt, bevor die Attributinstanz zurückgegeben wird.
