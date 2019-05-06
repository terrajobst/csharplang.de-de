---
ms.openlocfilehash: 75fcd5b00ea5cac218a9f7809c53b179df97825c
ms.sourcegitcommit: 3fc033b6e98ed7ecdf46a85c79b00a3a3ddcf963
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/18/2019
ms.locfileid: "47229656"
---
# <a name="exceptions"></a>Ausnahmen

Ausnahmen in C# bieten eine strukturierte, einheitliche und typsichere Möglichkeit, sowohl auf Systemebene und auf Anwendungsebene Behandeln von fehlerbedingungen. Der Ausnahmemechanismus in C# ist vergleichbar mit C++, mit einigen wichtigen Unterschieden:

*  In C# müssen alle Ausnahmen, die von einer Instanz eines Klassentyps, die von abgeleiteten dargestellt werden `System.Exception`. In C++ kann einen beliebigen Wert eines beliebigen Typs verwendet werden, um eine Ausnahme darzustellen.
*  In C# einen finally-block ([der Try-Anweisung](statements.md#the-try-statement)) kann verwendet werden, um die Beendigung Code zu schreiben, die in normalen Ausführung und außergewöhnliche Bedingungen, die ausgeführt wird. Solcher Code ist schwierig, die in C++ zu schreiben, ohne Code zu duplizieren.
*  In C# auf Systemebene Ausnahmen wie z. B. Überlauf, Division durch Null und Null-Dereferenzierungen Ausnahmeklassen verfügen über klar definierte und werden auf die gleiche Stufe von fehlerbedingungen auf Anwendungsebene.

## <a name="causes-of-exceptions"></a>Ursachen von Ausnahmen

Ausnahme kann auf zwei unterschiedliche Arten ausgelöst werden.

*  Ein `throw` Anweisung ([die Throw-Anweisung](statements.md#the-throw-statement)) löst eine Ausnahme aus, sofort und ohne Bedingung. Steuerung niemals erreicht, die Anweisung sofort nach der `throw`.
*  Bestimmte außergewöhnlichen Bedingungen, die während der Verarbeitung von C#-Anweisungen und Ausdrücke entstehen unter bestimmten Umständen eine Ausnahme ausgelöst, wenn der Vorgang normal abgeschlossen werden kann. Z. B. eine ganze Zahl Division ([Divisionsoperator](expressions.md#division-operator)) löst eine `System.DivideByZeroException` Wenn der Nenner 0 (null) ist. Finden Sie unter [Common Exception Classes](exceptions.md#common-exception-classes) eine Liste der verschiedenen Ausnahmen, die auf diese Weise auftreten können.

## <a name="the-systemexception-class"></a>Die System.Exception-Klasse

Die `System.Exception` Klasse ist der Basistyp aller Ausnahmen. Diese Klasse verfügt über einige wichtige Eigenschaften, die alle Ausnahmen, die gemeinsam nutzen:

*  `Message` ist eine nur-Lese Eigenschaft des Typs `string` , der eine lesbare Beschreibung des Grunds für die Ausnahme enthält.
*  `InnerException` ist eine nur-Lese Eigenschaft des Typs `Exception`. Wenn der Wert ungleich Null ist, wird es bezieht sich auf die Ausnahme, die die aktuelle Ausnahme verursacht hat –, also die aktuelle Ausnahme ausgelöst wurde, in einem Catch-Block behandeln die `InnerException`. Andernfalls ist der Wert null ist, gibt an, dass diese Ausnahme nicht durch eine andere Ausnahme ausgelöst wurde. Die Anzahl von Ausnahmeobjekten miteinander verkettet werden, auf diese Weise kann beliebige sein.

Der Wert dieser Eigenschaften kann angegeben werden, in Aufrufen an den Instanzkonstruktor für `System.Exception`.

## <a name="how-exceptions-are-handled"></a>Behandlung von Ausnahmen

Ausnahmen werden behandelt, indem eine `try` Anweisung ([der Try-Anweisung](statements.md#the-try-statement)).

Wenn eine Ausnahme auftritt, das System sucht das nächste `catch` -Klausel, die die Ausnahme behandeln kann, wie durch den Laufzeit-Typ der Ausnahme bestimmt. Zunächst durchsucht die aktuelle Methode für eine lexikalisch einschließenden `try` -Anweisung und die zugehörigen Catch-Klauseln der Try-Anweisung in der Reihenfolge betrachtet werden. Schlägt dies fehl, wird die Methode, die die aktuelle Methode aufgerufen für eine lexikalisch einschließenden durchsucht `try` -Anweisung, die den Punkt des Aufrufs an die aktuelle Methode einschließt. Bei dieser Suche wird fortgesetzt, bis eine `catch` Klausel wurde gefunden, die können die aktuelle Ausnahme behandeln, indem Sie benennen eine Ausnahmeklasse, die von derselben Klasse oder eine Basisklasse, von dem Laufzeittyp der ausgelösten Ausnahme ist. Ein `catch` -Klausel, die eine Ausnahme Klassenname nicht kann eine beliebige Ausnahme behandeln.

Nach eine übereinstimmenden Catch-Klausel gefunden wird, wird das System die Steuerung an die Catch-Klausel der ersten Anweisung übertragen vorbereitet. Vor dem Beginn der Ausführung des Catch-Klausel das System zuerst ausgeführt wird, nacheinander alle `finally` Klauseln, die Try-Anweisungen, die weitere zugeordnet waren geschachtelt sind, als die, die die Ausnahme abgefangen.

Wenn keine übereinstimmenden Catch-Klausel gefunden wird, geschieht zweierlei:

*  Wenn die Suche nach einer übereinstimmenden Catch-Klausel einen statischen Konstruktor erreicht ([statische Konstruktoren](classes.md#static-constructors)) oder statischen Feldinitialisierer, und klicken Sie dann eine `System.TypeInitializationException` wird ausgelöst, an dem Punkt, der den Aufruf der statischen Konstruktor ausgelöst. Die innere Ausnahme von der `System.TypeInitializationException` enthält die Ausnahme, die ursprünglich ausgelöst wurde.
*  Wenn die Suche nach übereinstimmenden Catch-Klauseln im Code, der Anfangs den Thread gestartet hat erreicht, wird die Ausführung des Threads beendet. Die Auswirkungen des Abbruchs ist implementierungsdefiniert.

Ausnahmen, die auftreten, während der Destruktorausführung sind muss erwähnt werden. Wenn eine Ausnahme tritt auf, während der Destruktorausführung diese Ausnahme nicht abgefangen und die Ausführung von diesem Destruktor wird beendet, und der Destruktor der Basisklasse (sofern vorhanden) aufgerufen. Wenn keine Basisklasse vorhanden ist (wie im Fall von der `object` Typs) oder wenn es keine Basisklassen-Destruktor, wird die Ausnahme verworfen.

## <a name="common-exception-classes"></a>Allgemeine Ausnahmeklassen

Die folgenden Ausnahmen werden von bestimmten C#-Vorgängen ausgelöst.

|                                      |                |
|--------------------------------------|----------------|
| `System.ArithmeticException`         | Eine Basisklasse für Ausnahmen (z.B. `System.DivideByZeroException` und `System.OverflowException`), die während arithmetischer Operationen auftreten. | 
| `System.ArrayTypeMismatchException`  | Ausgelöst, wenn Sie ein Speicher in ein Array, das schlägt fehl, da der tatsächliche Typ des gespeicherten Elements mit dem tatsächlichen Typ des Arrays inkompatibel ist. | 
| `System.DivideByZeroException`       | Ausgelöst, wenn der Versuch, einen Integralwert durch Null zu teilen. | 
| `System.IndexOutOfRangeException`    | Wird ausgelöst, wenn ein Versuch, ein Array mit einem Index zu indizieren, die kleiner als 0 (null) oder außerhalb der Grenzen des Arrays ist. | 
| `System.InvalidCastException`        | Wird ausgelöst, wenn eine explizite Konvertierung von einer Basisklasse oder Schnittstelle in einem abgeleiteten Typ zur Laufzeit ein Fehler auftritt. | 
| `System.NullReferenceException`      | Wird ausgelöst, wenn eine `null` -Verweis wird verwendet, in einer Weise, die bewirkt, dass das referenzierte Objekt erforderlich sein. | 
| `System.OutOfMemoryException`        | Wird ausgelöst, wenn ein Versuch zur Belegung von Arbeitsspeicher (über `new`) ein Fehler auftritt. | 
| `System.OverflowException`           | Wird ausgelöst, wenn eine arithmetische Operation im Kontext `checked` überläuft. | 
| `System.StackOverflowException`      | Wird ausgelöst, wenn der Ausführungsstapel durch zu viele ausstehende Methodenaufrufe ausgeschöpft ist; in der Regel ein Zeichen für sehr tiefe oder unbegrenzte Rekursion. | 
| `System.TypeInitializationException` | Wird ausgelöst, wenn ein statischer Konstruktor eine Ausnahme aus, und keine auslöst `catch` Klauseln vorhanden ist, die abgefangen werden. | 
