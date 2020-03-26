---
ms.openlocfilehash: 54ae4ffabde6dca49b7e6bfb626d65837eabc8f5
ms.sourcegitcommit: 1e1c7c72b156e2fbc54d6d6ac8d21bca9934d8d2
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/26/2020
ms.locfileid: "80281943"
---
# <a name="simple-programs"></a>Einfache Programme

* [x] vorgeschlagen
* [x] Prototyp: gestartet
* [] Implementierung: nicht gestartet
* [] Spezifikation: nicht gestartet

## <a name="summary"></a>Zusammenfassung
[summary]: #summary

Ermöglicht das Auftreten einer Sequenz von- *Anweisungen* direkt vor den *namespace_member_declaration*s einer *compilation_unit* (d. h. Quelldatei).

Die Semantik besteht darin, dass die folgende Typdeklaration, wenn eine solche Sequenz von *Anweisungen* vorhanden ist, den tatsächlichen Typnamen und den Methodennamen Modulo ergibt:

``` c#
static class Program
{
    static async Task Main()
    {
        // statements
    }
}
```

Siehe auch https://github.com/dotnet/csharplang/issues/3117.

## <a name="motivation"></a>Motivation
[motivation]: #motivation

Aufgrund der Notwendigkeit einer expliziten `Main` Methode gibt es auch eine bestimmte Anzahl von Code Steinen, die auch die einfachsten Programme umgeben. Dies scheint das Erlernen von Sprache und Programm Klarheit zu erreichen. Das Hauptziel des Features besteht daher darin, Programme ohne C# unnötige Bausteine für die Lernprogramme und die Klarheit des Codes zuzulassen.

## <a name="detailed-design"></a>Detaillierter Entwurf
[design]: #detailed-design

### <a name="syntax"></a>Syntax

Die einzige zusätzliche Syntax besteht darin, eine Sequenz *von-* Anweisungen in einer Kompilierungseinheit direkt vor den *namespace_member_declaration*s zuzulassen:

``` antlr
compilation_unit
    : extern_alias_directive* using_directive* global_attributes? statement* namespace_member_declaration*
    ;
```

Nur ein *compilation_unit* darf *Anweisung*s enthalten. 

Beispiel:

``` c#
if (System.Environment.CommandLine.Length == 0
    || !int.TryParse(System.Environment.CommandLine, out int n)
    || n < 0) return;
Console.WriteLine(Fib(n).curr);

(int curr, int prev) Fib(int i)
{
    if (i == 0) return (1, 0);
    var (curr, prev) = Fib(i - 1);
    return (curr + prev, curr);
}
```

### <a name="semantics"></a>Semantik

Wenn alle Anweisungen der obersten Ebene in einer Kompilierungseinheit des Programms vorhanden sind, ist die Bedeutung so, als ob Sie im Block Körper einer `Main` Methode einer `Program` Klasse im globalen Namespace kombiniert wurden, wie im folgenden dargestellt:

``` c#
static class Program
{
    static async Task Main()
    {
        // statements
    }
}
```

Beachten Sie, dass die Namen "Program" und "Main" nur zu Illustrations Zwecken verwendet werden, dass die vom Compiler verwendeten Namen von der Implementierung abhängen und weder der Typ noch die Methode anhand des Namens aus dem Quellcode referenziert werden kann.

Die-Methode wird als Einstiegspunkt des Programms bezeichnet. Explizit deklarierte Methoden, die gemäß der Konvention als Einstiegspunkt Kandidaten angesehen werden können, werden ignoriert. Wenn dies der Fall ist, wird eine Warnung ausgegeben. Es ist ein Fehler, `-main:<type>` Compilerschalter anzugeben, wenn die Anweisungen der obersten Ebene vorhanden sind.

Asynchrone Vorgänge sind in den Anweisungen der obersten Ebene zulässig, bis zu dem Grad, den Sie in-Anweisungen innerhalb einer regulären Methode für asynchrone Einstiegspunkte haben. Sie sind jedoch nicht erforderlich. wenn `await` Ausdrücke und andere asynchrone Vorgänge ausgelassen werden, wird keine Warnung erzeugt. Stattdessen entspricht die Signatur der generierten Einstiegspunkt Methode 
``` c#
    static void Main()
```

Das obige Beispiel ergibt die folgende `$Main` Methoden Deklaration:

``` c#
static class $Program
{
    static void $Main()
    {
        if (System.Environment.CommandLine.Length == 0
            || !int.TryParse(System.Environment.CommandLine, out int n)
            || n < 0) return;
        Console.WriteLine(Fib(n).curr);
        
        (int curr, int prev) Fib(int i)
        {
            if (i == 0) return (1, 0);
            var (curr, prev) = Fib(i - 1);
            return (curr + prev, curr);
        }
    }
}
```

Gleichzeitig ein Beispiel wie das folgende:
``` c#
await System.Threading.Tasks.Task.Delay(1000);
System.Console.WriteLine("Hi!");
```

würde ergeben:
``` c#
static class $Program
{
    static async Task $Main()
    {
        await System.Threading.Tasks.Task.Delay(1000);
        System.Console.WriteLine("Hi!");
    }
}
```

### <a name="scope-of-top-level-local-variables-and-local-functions"></a>Bereich von lokalen Variablen der obersten Ebene und lokalen Funktionen

Obwohl lokale Variablen und Funktionen der obersten Ebene in der generierten Einstiegspunkt Methode "umfänden" sind, sollten Sie im gesamten Programm in jeder Kompilierungseinheit weiterhin im Gültigkeitsbereich sein.
Für den Zweck der Auswertung mit einfachem Namen wird der globale Namespace erreicht:
- Zuerst wird versucht, den Namen innerhalb der generierten Einstiegspunkt Methode auszuwerten, und zwar nur, wenn dieser Versuch fehlschlägt. 
- Die "reguläre" Auswertung innerhalb der globalen Namespace Deklaration wird ausgeführt. 

Dies kann zu einem namens shadodown von Namespaces und Typen führen, die innerhalb des globalen Namespace deklariert sind, sowie zum überschatten importierter Namen.

Wenn die einfache namens Auswertung außerhalb der Anweisungen der obersten Ebene stattfindet und die Auswertung eine lokale Variable oder Funktion der obersten Ebene ergibt, sollte dies zu einem Fehler führen.

Auf diese Weise schützen wir unsere zukünftige Fähigkeit zur besseren Adressierung von "Funktionen der obersten Ebene" (Szenario 2 in https://github.com/dotnet/csharplang/issues/3117)und können Benutzern nützliche Diagnosen zur Verfügung stellen.

