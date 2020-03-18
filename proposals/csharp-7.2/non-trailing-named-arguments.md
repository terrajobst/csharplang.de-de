---
ms.openlocfilehash: ac2b233eb703b5eea3bd2dfdbeeadd7494b0c695
ms.sourcegitcommit: 94a3d151c438d34ede1d99de9eb4ebdc07ba4699
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/25/2019
ms.locfileid: "79483643"
---
# <a name="non-trailing-named-arguments"></a>Nicht schließende benannte Argumente

## <a name="summary"></a>Zusammenfassung
[summary]: #summary
Zulassen, dass benannte Argumente an einer nicht nachfolgenden Position verwendet werden, solange Sie an der richtigen Position verwendet werden. Beispiel: `DoSomething(isEmployed:true, name, age);`.

## <a name="motivation"></a>Motivation
[motivation]: #motivation

Die Hauptmotivation besteht darin, redundante Informationen zu vermeiden. Es ist üblich, ein Argument, bei dem es sich um eine Literale handelt (z. b. `null``true`), für die Verdeutlichung des Codes zu benennen, anstatt Argumente außerhalb der Reihenfolge zu übergeben.
Dies ist zurzeit nicht zulässig (`CS1738`), es sei denn, die folgenden Argumente werden ebenfalls benannt.

```csharp
DoSomething(isEmployed:true, name, age); // currently disallowed, even though all arguments are in position
// CS1738 "Named argument specifications must appear after all fixed arguments have been specified"
```

Einige zusätzliche Beispiele:
```csharp
public void DoSomething(bool isEmployed, string personName, int personAge) { ... }

DoSomething(isEmployed:true, name, age); // currently CS1738, but would become legal
DoSomething(true, personName:name, age); // currently CS1738, but would become legal
DoSomething(name, isEmployed:true, age); // remains illegal
DoSomething(name, age, isEmployed:true); // remains illegal
DoSomething(true, personAge:age, personName:name); // already legal
```

Dies würde auch mit Parametern funktionieren:
```csharp
public class Task
{
    public static Task When(TaskStatus all, TaskStatus any, params Task[] tasks);
}
Task.When(all: TaskStatus.RanToCompletion, any: TaskStatus.Faulted, task1, task2)
```

## <a name="detailed-design"></a>Detaillierter Entwurf
[design]: #detailed-design

In "7.5.1" (Argument Listen) gibt die Spezifikation derzeit Folgendes an:
> Ein *Argument* mit einem *Argument Namen* wird als __benanntes Argument__bezeichnet, wohingegen ein *Argument* ohne *Argument Name* ein __Positions Argument__ist. Es ist ein Fehler für ein Positions Argument, das nach einem benannten Argument in einer *Argumentliste*angezeigt wird.

Der Vorschlag besteht darin, diesen Fehler zu entfernen und die Regeln für die Suche nach dem entsprechenden Parameter für ein Argument ("7.5.1.1") zu aktualisieren:

Argumente in der Argumentliste von Instanzkonstruktoren, Methoden, Indexern und Delegaten:
- [vorhandene Regeln]
- Ein unbenanntes Argument entspricht keinem Parameter, wenn es sich nach einem benannten Argument außerhalb der Position oder einem benannten params-Argument befindet.

Dies verhindert insbesondere das Aufrufen von `void M(bool a = true, bool b = true, bool c = true, );` mit `M(c: false, valueB);`. Das erste Argument wird außerhalb der Position verwendet (das Argument wird an der ersten Position verwendet, aber der Parameter mit dem Namen "c" befindet sich an der dritten Position). Daher sollten die folgenden Argumente benannt werden.

Anders ausgedrückt: nicht nachfolgende benannte Argumente sind nur zulässig, wenn der Name und die Position dazu führen, den gleichen entsprechenden Parameter zu suchen.

## <a name="drawbacks"></a>Nachteile
[drawbacks]: #drawbacks

Dieser Vorschlag verschärft vorhandene Feinheiten mit benannten Argumenten in der Überladungs Auflösung. Beispiel:

```csharp
void M(int x, int y) { }
void M<T>(T y, int x) { }

void M2()
{
    M(3, 4);
    M(y: 3, x: 4); // Invokes M(int, int)
    M(y: 3, 4); // Invokes M<T>(T, int)
}
```

Sie können diese Situation heute erzielen, indem Sie die Parameter austauschen:

```csharp
void M(int y, int x) { }
void M<T>(int x, T y) { }

void M2()
{
    M(3, 4);
    M(x: 3, y: 4); // Invokes M(int, int)
    M(3, y: 4); // Invokes M<T>(int, T)
}
```

Wenn Sie zwei Methoden `void M(int a, int b)` und `void M(int x, string y)`haben, erzeugt der falsche Aufruf `M(x: 1, 2)` eine Diagnose auf der Grundlage der zweiten Überladung ("kann nicht von" int "in" String "konvertiert werden). Dieses Problem ist bereits vorhanden, wenn das benannte Argument an einer nachfolgenden Position verwendet wird.

## <a name="alternatives"></a>Alternativen
[alternatives]: #alternatives

Es gibt einige Alternativen zu berücksichtigende Aspekte:

- Der Status quo
- Bereitstellen von IDE-Unterstützung zum Ausfüllen aller Namen von nachfolgenden Argumenten, wenn Sie einen bestimmten Namen in der Mitte eingeben.

Beide sind von ausführlicheren Ausführlichkeit, da Sie mehrere benannte Argumente einführen, auch wenn Sie nur einen Namen eines Literals am Anfang der Argumentliste benötigen.

## <a name="unresolved-questions"></a>Nicht aufgelöste Fragen
[unresolved]: #unresolved-questions

## <a name="design-meetings"></a>Treffen von Besprechungen
[ldm]: #ldm
Die Funktion wurde kurz am 16. Mai 2017 in LDM erläutert, mit Genehmigung im Prinzip ("OK", um zu "Vorschlag/Prototyp" zu wechseln). Es wurde auch kurz am 28. Juni 2017 erläutert.

Bezieht sich auf die anfängliche Erörterung https://github.com/dotnet/csharplang/issues/518 die sich auf das Problem https://github.com/dotnet/csharplang/issues/570
