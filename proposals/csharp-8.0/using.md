---
ms.openlocfilehash: 91afbc3e3412049cd183c36c8035f1862c520413
ms.sourcegitcommit: da1180f7eacdd5067b32d291a76e6764159e00fe
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/05/2019
ms.locfileid: "79483973"
---
# <a name="pattern-based-using-and-using-declarations"></a>"Musterbasiertes verwenden" und "Verwenden von Deklarationen"

## <a name="summary"></a>Zusammenfassung

Die Sprache fügt zwei neue Funktionen um die `using`-Anweisung hinzu, um die Ressourcenverwaltung zu vereinfachen: `using` sollten zusätzlich zu `IDisposable` ein verwerfbares Muster erkennen und der Sprache eine `using` Deklaration hinzufügen.

## <a name="motivation"></a>Motivation

Die `using`-Anweisung ist zurzeit ein effektives Tool für die Ressourcenverwaltung, erfordert aber eine ganze Stunde. Methoden mit einer Reihe von Ressourcen, die verwaltet werden müssen, können mit einer Reihe von `using`-Anweisungen syntaktisch blockiert werden. Diese Syntax Belastung ist ausreichend, dass die meisten Richtlinien zum Codierungsstil in diesem Szenario explizit mit geschweiften Klammern zu tun haben. 

Mit der `using` Deklaration wird C# ein Großteil der Zeremonie an dieser Stelle entfernt und mit anderen Sprachen, die Ressourcen Verwaltungs Blöcke einschließen, abgerufen. Außerdem ermöglicht die Musterbasierte `using` Entwicklern, den Satz von Typen zu erweitern, die an dieser Stelle teilnehmen können. In vielen Fällen ist es nicht mehr erforderlich, Wrapper Typen zu erstellen, die nur vorhanden sind, um Werte in einer `using` Anweisung zu verwenden. 

Diese Features ermöglichen es Entwicklern, die Szenarien, in denen `using` angewendet werden können, zu vereinfachen und zu erweitern.

## <a name="detailed-design"></a>Detaillierter Entwurf 

### <a name="using-declaration"></a>using-Deklaration

Die Sprache ermöglicht das Hinzufügen von `using` zu einer lokalen Variablen Deklaration. Eine solche Deklaration hat denselben Effekt wie das Deklarieren der Variablen in einer `using`-Anweisung am gleichen Speicherort.

```csharp
if (...) 
{ 
   using FileStream f = new FileStream(@"C:\users\jaredpar\using.md");
   // statements
}

// Equivalent to 
if (...) 
{ 
   using (FileStream f = new FileStream(@"C:\users\jaredpar\using.md")) 
   {
    // statements
   }
}
```

Die Lebensdauer eines `using` lokalen wird bis zum Ende des Bereichs erweitert, in dem er deklariert ist. Die `using` lokalen Variablen werden dann in umgekehrter Reihenfolge verworfen, in der Sie deklariert werden. 

```csharp
{ 
    using var f1 = new FileStream("...");
    using var f2 = new FileStream("..."), f3 = new FileStream("...");
    ...
    // Dispose f3
    // Dispose f2 
    // Dispose f1
}
```

Es gibt keine Einschränkungen in Bezug auf `goto`oder ein anderes Ablauf Steuerungs Konstrukt im Zusammenhang mit einer `using` Deklaration. Stattdessen verhält sich der Code genauso wie für die entsprechende `using`-Anweisung:

```csharp
{
    using var f1 = new FileStream("...");
  target:
    using var f2 = new FileStream("...");
    if (someCondition) 
    {
        // Causes f2 to be disposed but has no effect on f1
        goto target;
    }
}
```

Eine lokale Deklaration, die in einer `using` lokalen Deklaration deklariert wird, ist implizit schreibgeschützt. Dies entspricht dem Verhalten der in einer `using` Anweisung deklarierten lokalen Variablen. 

Die Sprachgrammatik für `using` Deklarationen lautet wie folgt:

```antlr
local-using-declaration:
  using type using-declarators

using-declarators:
  using-declarator
  using-declarators , using-declarator
  
using-declarator:
  identifier = expression
```

Einschränkungen bei der `using` Deklaration:

- Darf nicht direkt in einer `case` Bezeichnung vorkommen, sondern muss sich innerhalb eines Blocks innerhalb der `case` Bezeichnung befinden.
- Kann nicht als Teil einer `out` Variablen Deklaration angezeigt werden. 
- Für jeden Deklarator muss ein Initialisierer vorhanden sein.
- Der lokale Typ muss implizit konvertierbar sein, um das `using` Muster zu `IDisposable` oder zu erfüllen.

### <a name="pattern-based-using"></a>Muster basiert mithilfe von

Die Sprache fügt das Konzept eines verwerfbaren Musters hinzu: ein Typ, der über eine `Dispose` Instanzmethode verfügt, auf die zugegriffen werden kann. Typen, die das verwerfbare Muster erfüllen, können an einer `using` Anweisung oder Deklaration teilnehmen, ohne dass `IDisposable`implementiert werden muss. 

```csharp
class Resource
{ 
    public void Dispose() { ... }
}

using (var r = new Resource())
{
    // statements
}
```

Auf diese Weise können Entwickler `using` in einer Reihe neuer Szenarien nutzen:

- `ref struct`: diese Typen können heute keine Schnittstellen implementieren und können daher nicht an `using` Anweisungen teilnehmen.
- Erweiterungs Methoden ermöglichen Entwicklern das Erweitern von Typen in anderen Assemblys, sodass Sie an `using`-Anweisungen teilnehmen können.

In der Situation, in der ein Typ implizit in `IDisposable` konvertiert werden kann, und auch das verwerfbare Muster entspricht, wird `IDisposable` bevorzugt. Wenngleich dies der gegen übergesetzte Ansatz von `foreach` ist (Muster, das gegenüber Interface bevorzugt ist), ist es für Abwärtskompatibilität erforderlich.

Dies gilt auch für die gleichen Einschränkungen wie für eine herkömmliche `using`-Anweisung: lokale Variablen, die im `using` deklariert sind, sind schreibgeschützt, ein `null` Wert führt nicht dazu, dass eine Ausnahme ausgelöst wird usw... Die Codegenerierung unterscheidet sich nur darin, dass vor dem Aufrufen von "verwerfen" keine Umwandlung in `IDisposable` erfolgt:

```csharp
{
      Resource r = new Resource();
      try {
            // statements
      }
      finally {
            if (resource != null) resource.Dispose();
      }
}
```

Um dem verwerfbaren Muster gerecht zu werden, muss auf die `Dispose` Methode zugegriffen werden können, Parameter lose Parameter und haben einen `void` Rückgabetyp. Es gibt keine weiteren Einschränkungen. Dies bedeutet explizit, dass hier Erweiterungs Methoden verwendet werden können.

## <a name="considerations"></a>Überlegungen

### <a name="case-labels-without-blocks"></a>Case-Bezeichnungen ohne Blöcke

Eine `using declaration` ist aufgrund von Komplikationen bei der tatsächlichen Lebensdauer direkt innerhalb einer `case` Bezeichnung unzulässig. Eine mögliche Lösung besteht darin, die gleiche Lebensdauer wie eine `out var` am gleichen Speicherort anzugeben. Dies war die zusätzliche Komplexität der Featureimplementierung und die einfache Problem Umgehung (fügen Sie der `case` Bezeichnung einen Block hinzu) nicht rechtfertigen diese Route.

## <a name="future-expansions"></a>Zukünftige Erweiterungen

### <a name="fixed-locals"></a>lokale Variablen

Eine `fixed`-Anweisung verfügt über alle Eigenschaften von `using`-Anweisungen, die die Möglichkeit haben, `using` lokale zu haben. Beachten Sie, dass Sie dieses Feature auch auf `fixed` lokalen Erweiterungen erweitern müssen. Die Lebensdauer-und Bestell Regeln sollten für `using` und `fixed` hier gleichermaßen gut anwendbar sein.
