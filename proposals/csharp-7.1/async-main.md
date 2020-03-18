---
ms.openlocfilehash: 405153448d0e3685d6f22725e00d75d9250b3e20
ms.sourcegitcommit: 94a3d151c438d34ede1d99de9eb4ebdc07ba4699
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/25/2019
ms.locfileid: "79483751"
---
# <a name="async-main"></a>Async-Haupt-

* [x] vorgeschlagen
* [] Prototyp
* []-Implementierung
* []-Spezifikation

## <a name="summary"></a>Zusammenfassung
[summary]: #summary

Ermöglicht die Verwendung von `await` in der Main/EntryPoint-Methode einer Anwendung, indem der EntryPoint `Task` / `Task<int>` zurückgeben und als `async`gekennzeichnet werden kann.

## <a name="motivation"></a>Motivation
[motivation]: #motivation

Beim Schreiben von konsolenbasierten Dienst C#Programmen und beim Schreiben von kleinen Test-apps, die `async` Methoden von Main `await` aufgerufen werden sollen, kommt es sehr häufig vor.  Heute fügen wir hier einen Komplexitäts Grad hinzu, indem wir das Erstellen einer solchen `await`"in einer separaten Async-Methode erzwingen, was dazu beiträgt, dass Entwickler wie folgt Code Bausteine schreiben müssen, um zu beginnen:

```csharp
public static void Main()
{
    MainAsync().GetAwaiter().GetResult();
}

private static async Task MainAsync()
{
    ... // Main body here
}
```

Wir können die Notwendigkeit dieses Bausteine entfernen und den Einstieg vereinfachen, indem wir einfach den eigentlichen `async` so lassen, dass `await`s darin verwendet werden kann.

## <a name="detailed-design"></a>Detaillierter Entwurf
[design]: #detailed-design

Die folgenden Signaturen sind zurzeit zulässige entryPoints:

```csharp
static void Main()
static void Main(string[])
static int Main()
static int Main(string[])
```

Wir erweitern die Liste der zulässigen entryPoints um Folgendes:

```csharp
static Task Main()
static Task<int> Main()
static Task Main(string[])
static Task<int> Main(string[])
```

Um Kompatibilitäts Risiken zu vermeiden, werden diese neuen Signaturen nur als gültige Einstiegspunkte betrachtet, wenn keine über Ladungen der vorherigen Menge vorhanden sind.
Die Sprache/der Compiler erfordert nicht, dass der EntryPoint als `async`gekennzeichnet wird, obwohl wir erwarten, dass die meisten Verwendungen als solche gekennzeichnet werden.

Wenn einer dieser Methoden als EntryPoint identifiziert wird, erstellt der Compiler eine tatsächliche EntryPoint-Methode, die eine dieser codierten Methoden aufruft:
- ```static Task Main()``` führt dazu, dass der Compiler das Äquivalent von ausgibt ```private static void $GeneratedMain() => Main().GetAwaiter().GetResult();```
- ```static Task Main(string[])``` führt dazu, dass der Compiler das Äquivalent von ausgibt ```private static void $GeneratedMain(string[] args) => Main(args).GetAwaiter().GetResult();```
- ```static Task<int> Main()``` führt dazu, dass der Compiler das Äquivalent von ausgibt ```private static int $GeneratedMain() => Main().GetAwaiter().GetResult();```
- ```static Task<int> Main(string[])``` führt dazu, dass der Compiler das Äquivalent von ausgibt ```private static int $GeneratedMain(string[] args) => Main(args).GetAwaiter().GetResult();```

Beispielsyntax:

```csharp
using System;
using System.Net.Http;

class Test
{
    static async Task Main(string[] args) =>
        Console.WriteLine(await new HttpClient().GetStringAsync(args[0]));
}
```

## <a name="drawbacks"></a>Nachteile
[drawbacks]: #drawbacks

Der Hauptnachteil ist einfach die zusätzliche Komplexität der Unterstützung zusätzlicher EntryPoint-Signaturen.

## <a name="alternatives"></a>Alternativen
[alternatives]: #alternatives

Weitere Varianten, die berücksichtigt werden:

Zulassen von `async void`.  Wir müssen die Semantik für Code, der Sie direkt aufruft, unverändert lassen, wodurch es für einen generierten EntryPoint schwierig wird, ihn aufzurufen (keine Aufgabe zurückgegeben).  Wir könnten dies beheben, indem wir zwei andere Methoden erstellen, z. b.

```csharp
public static async void Main()
{
   ... // await code
}
```

wird zu

```csharp
public static async void Main() => await $MainTask();

private static void $EntrypointMain() => Main().GetAwaiter().GetResult();

private static async Task $MainTask()
{
    ... // await code
}
```

Außerdem gibt es Bedenken hinsichtlich der Verwendung von `async void`.

Verwenden von "mainasync" anstelle von "Main" als Name.  Obwohl das Async-Suffix für Methoden zurückgegeben wird, die Methoden zurückgeben, liegt das in erster Linie in der Bibliotheks Funktionalität, bei der es sich nicht um den Hauptschlüssel handelt, und die Unterstützung zusätzlicher entrypointnamen über "Main" hinaus.

## <a name="unresolved-questions"></a>Nicht aufgelöste Fragen
[unresolved]: #unresolved-questions

–

## <a name="design-meetings"></a>Treffen von Besprechungen

–
