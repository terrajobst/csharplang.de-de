---
ms.openlocfilehash: 52b43abd2d8fb56088a68c7169289a63c43ce96f
ms.sourcegitcommit: 94a3d151c438d34ede1d99de9eb4ebdc07ba4699
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/25/2019
ms.locfileid: "79483487"
---
# <a name="suppress-emitting-of-localsinit-flag"></a>Unterdrückt die Ausgabe von `localsinit`-Flag.

* [x] vorgeschlagen
* [] Prototyp: nicht gestartet
* [] Implementierung: nicht gestartet
* [] Spezifikation: nicht gestartet

## <a name="summary"></a>Zusammenfassung
[summary]: #summary

Ermöglicht das Unterdrücken der Ausgabe von `localsinit`-Flag über `SkipLocalsInitAttribute` Attribut. 

## <a name="motivation"></a>Motivation
[motivation]: #motivation


### <a name="background"></a>Hintergrund
Pro CLR-Spezifikation lokale Variablen, die keine Verweise enthalten, werden von der VM/JIT nicht mit einem bestimmten Wert initialisiert. Das Lesen aus solchen Variablen ohne Initialisierung ist typsicher, aber andernfalls ist das Verhalten nicht definiert und Implementierungs spezifisch. Normalerweise enthalten nicht initialisierte lokale Variablen alle Werte, die im Arbeitsspeicher verbleiben, der nun vom Stapel Rahmen belegt wird. Dies kann zu nicht deterministischem Verhalten und schwer zu reproduzieren Fehlern führen. 

Es gibt zwei Möglichkeiten, eine lokale Variable zuzuweisen: 
- durch Speichern eines Werts oder 
- Wenn Sie `localsinit` Flag angeben, das alle zugeordneten Elemente aus dem lokalen Speicherpool erzwingt, die mit NULL initialisiert werden: Dies schließt sowohl lokale Variablen als auch `stackalloc` Daten ein.    

Die Verwendung von nicht initialisierten Daten wird nicht empfohlen und ist in überprüfbarem Code nicht zulässig. Obwohl es möglicherweise möglich ist, nachzuweisen, dass der Überprüfungs Algorithmus Durchfluss Analysen konservativ sein kann, ist es einfach, dass `localsinit` festgelegt ist.

In C# der Vergangenheit gibt der Compiler `localsinit` Flag für alle Methoden aus, die lokale deklarieren.

Wenngleich C# eine definitive Zuweisungs Analyse eingesetzt wird, die strenger ist als die CLRC# -Spezifikation, die die Verwendung von CLR-Spezifikationen erfordert (erfordert auch das Festlegen von lokalen Assemblys), ist es nicht zwingend sicherzustellen, dass der resultierende Code formal
- CLR und C# Regeln stimmen möglicherweise nicht überein, ob das Übergeben eines lokalen as `out`-Arguments eine `use`ist.
- CLR und C# Regeln stimmen möglicherweise nicht mit der Behandlung von bedingten Verzweigungen überein, wenn Bedingungen bekannt sind (Konstante Propagierung).
- CLR kann auch nur `localinits`erfordern, da dies zulässig ist.  

### <a name="problem"></a>Problem
Bei Hochleistungsanwendungen können die Kosten der erzwungenen NULL-Initialisierung erkennbar sein. Dies ist besonders bemerkbar, wenn `stackalloc` verwendet wird.

In einigen Fällen kann JIT die anfängliche NULL-Initialisierung einzelner lokaler Variablen entfernen, wenn diese Initialisierung durch nachfolgende Zuweisungen "abgebrochen" wird. Nicht alle JITs machen dies, und eine solche Optimierung weist Grenzen auf. Es hilft nicht bei der `stackalloc`.

Um zu verdeutlichen, dass es sich um ein echtes Problem handelt, gibt es einen bekannten Fehler, bei dem eine Methode, die keine `IL` lokalen Variablen enthält, nicht `localsinit` Flag hätte. Der Fehler wird bereits von Benutzern ausgenutzt, indem `stackalloc` absichtlich in derartige Methoden versetzt wird, um die Initialisierungs Kosten zu vermeiden. Dies liegt daran, dass das Fehlen von `IL` lokalen Variablen eine instabile Metrik ist und abhängig von Änderungen in der Codegen-Strategie variieren kann. Der Fehler sollte behoben werden, und Benutzer sollten eine dokumentierte und zuverlässigere Methode zum Unterdrücken des Flags erhalten. 

## <a name="detailed-design"></a>Detaillierter Entwurf

Ermöglicht das Angeben von `System.Runtime.CompilerServices.SkipLocalsInitAttribute`, um den Compiler anzuweisen, `localsinit` Flag nicht auszugeben.
 
Das Endergebnis besteht darin, dass die lokalen Variablen möglicherweise nicht von der JIT initialisiert werden, was in den meisten Fällen nicht Observable in C#ist.  
Außerdem werden `stackalloc` Daten nicht mit NULL initialisiert. Das ist definitiv Observable, aber auch das am meisten motivierte Szenario.

Zulässige und erkannte Attribut Ziele sind: `Method`, `Property`, `Module`, `Class`, `Struct`, `Interface`, `Constructor`. Der Compiler verlangt jedoch nicht, dass dieses Attribut mit den aufgelisteten Zielen definiert ist, und es wird nicht berücksichtigt, in welcher Assembly das Attribut definiert ist. 

Wenn Attribute für einen Container (`class``module`mit der Methode für eine geschachtelte Methode (...) angegeben wird, wirkt sich das Flag auf alle im Container enthaltenen Methoden aus.

Synthetisierte Methoden "Erben" das Flag vom logischen Container/Besitzer. 

Das Flag wirkt sich nur auf die CodeGen-Strategie für den eigentlichen Methoden Körper aus. Das heißt, das Flag hat keine Auswirkung auf abstrakte Methoden und wird nicht an Überschreibungs-/Implementierungsmethoden weitergegeben.

Dies ist explizit eine **_Compilerfunktion_** und **_keine Sprachfunktion_** .  
Ebenso wie die compilerbefehlszeilenschalter steuert die Funktion die Implementierungsdetails einer bestimmten CodeGen-Strategie und muss von der C# Spezifikation nicht benötigt werden.

## <a name="drawbacks"></a>Nachteile
[drawbacks]: #drawbacks

- Alte oder andere Compiler dürfen das-Attribut nicht berücksichtigen.
Das Ignorieren des Attributs ist ein kompatibles Verhalten. Dies kann nur zu einer geringfügigen Leistungseinbußen führen.

- Der Code ohne `localinits`-Flag löst möglicherweise Überprüfungs Fehler aus.
Benutzer, die diese Funktion anfordern, sind in der Regel nicht mit der Verifizierbarkeit beschäftigt. 
 
- Das Anwenden des-Attributs auf höheren Ebenen als eine einzelne Methode hat einen nicht lokalen Effekt, der beobachtet werden kann, wenn `stackalloc` verwendet wird. Dies ist jedoch das am häufigsten angeforderte Szenario.

## <a name="alternatives"></a>Alternativen
[alternatives]: #alternatives

- lassen Sie `localinits` Flag aus, wenn die Methode in `unsafe` Kontext deklariert wird. Dies könnte bei `stackalloc`zu stillen und gefährlichen Behavior Change von deterministisch zu nicht deterministisch führen.

- lassen Sie `localinits` Flag immer aus.
Noch schlimmer als oben.

- lassen Sie `localinits` Flag aus, es sei denn, im Methoden Text wird `stackalloc` verwendet.
Adressiert nicht das am häufigsten angeforderte Szenario und kann Code nicht überprüfbar machen, ohne dass diese wieder hergestellt werden kann.

## <a name="unresolved-questions"></a>Nicht aufgelöste Fragen
[unresolved]: #unresolved-questions

- Soll das Attribut tatsächlich an Metadaten ausgegeben werden? 

## <a name="design-meetings"></a>Treffen von Besprechungen

Noch keine. 