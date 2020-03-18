---
ms.openlocfilehash: 39fb0aab5e8bb0d422f25fd2e92ab3d8256d3f59
ms.sourcegitcommit: b8f1103eb686c5d82e294837c9386d9b667da292
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/29/2019
ms.locfileid: "79484021"
---
# <a name="readonly-references"></a>Schreibgeschützte Verweise

* [x] vorgeschlagen
* [x] Prototyp
* [x] Implementierung: gestartet
* [] Spezifikation: nicht gestartet

## <a name="summary"></a>Zusammenfassung
[summary]: #summary

Die Funktion "schreibgeschützte Verweise" ist tatsächlich eine Gruppe von Features, die die Effizienz der Weitergabe von Variablen als Verweis nutzen, ohne die Daten für Änderungen verfügbar zu machen:  
- `in` Parameter
- `ref readonly`-Rückgaben
- `readonly` Strukturen
- `ref`/`in`-Erweiterungs Methoden
- lokale `ref readonly`
- bedingte Ausdrücke `ref`

## <a name="passing-arguments-as-readonly-references"></a>Übergeben von Argumenten als schreibgeschützte Verweise.

Es gibt ein vorhandenes Angebot, das dieses Thema https://github.com/dotnet/roslyn/issues/115 als Sonderfall von schreibgeschützten Parametern, ohne in viele Details zu gehen.
An dieser Stelle möchte ich nur bestätigen, dass die Idee allein nicht ganz neu ist.

### <a name="motivation"></a>Motivation

Vor diesem Feature C# verfügte nicht über eine effiziente Möglichkeit, Struktur Variablen in Methodenaufrufe für schreibgeschützte Zwecke zu übergeben, ohne zu ändern. Die reguläre durch-Wert-Argument Übergabe impliziert das Kopieren, wodurch unnötige Kosten entstehen.  Dadurch werden Benutzer zur Verwendung der by-ref-Argument Übergabe und der Verwendung von Kommentaren/Dokumentationen aufgefordert, um anzugeben, dass die Daten nicht vom aufgerufenen mutiert werden sollen. Es ist aus vielen Gründen keine gute Lösung.  
Bei den Beispielen handelt es sich um zahlreiche Vektor-/matrixoperatoren in Grafik Bibliotheken, wie [XNA](https://msdn.microsoft.com/library/bb194944.aspx) bekanntermaßen nur aufgrund von Leistungs Überlegungen zu Referenz Operanden gehören. Der Roslyn-Compiler selbst enthält Code, der Strukturen verwendet, um Zuordnungen zu vermeiden, und Sie dann als Verweis übergibt, um das Kopieren von Kosten zu vermeiden.

### <a name="solution-in-parameters"></a>Lösung (`in` Parameter)

Ebenso wie die `out` Parameter werden `in` Parameter als verwaltete Verweise mit zusätzlichen Garantien vom aufgerufenen übergeben.  
Im Gegensatz zu `out`-Parametern, die vom aufgerufenen vor einer anderen Verwendung zugewiesen werden _müssen_ , können `in` Parameter überhaupt nicht vom aufgerufenen zugewiesen werden.

Daher können `in` Parameter die Effektivität der indirekten Argument Übergabe ermöglichen, ohne Argumente für Mutationen durch den aufgerufenen verfügbar zu machen.

### <a name="declaring-in-parameters"></a>Deklarieren eines `in`-Parameters

`in` Parameter werden mithilfe `in`-Schlüssel Worts als Modifizierer in der Parameter Signatur deklariert.

Für alle Zwecke wird der `in` Parameter als `readonly` Variable behandelt. Die meisten Einschränkungen in Bezug auf die Verwendung von `in`-Parametern in der-Methode sind identisch mit denen `readonly` Felder.

> Tatsächlich kann ein `in`-Parameter ein `readonly` Feld darstellen. Die Ähnlichkeit von Einschränkungen stellt keinen Zufall dar.

Beispielsweise werden Felder eines `in`-Parameters mit einem Strukturtyp rekursiv als `readonly` Variablen klassifiziert.

```csharp
static Vector3 Add (in Vector3 v1, in Vector3 v2)
{
    // not OK!!
    v1 = default(Vector3);

    // not OK!!
    v1.X = 0;

    // not OK!!
    foo(ref v1.X);

    // OK
    return new Vector3(v1.X + v2.X, v1.Y + v2.Y, v1.Z + v2.Z);
}
```

- `in` Parameter sind überall zulässig, wo gewöhnliche ByVal-Parameter zulässig sind. Dies schließt Indexer, Operatoren (einschließlich Konvertierungen), Delegaten, Lambdas, lokale Funktionen ein.

> ```csharp
>  (in int x) => x                                                     // lambda expression  
>  TValue this[in TKey index];                                         // indexer
>  public static Vector3 operator +(in Vector3 x, in Vector3 y) => ... // operator
>  ```

- `in` ist in Kombination mit `out` oder mit allem, was `out` nicht kombiniert, nicht zulässig.

- Es ist nicht zulässig, `ref`/zu überlasten `out`/`in` Unterschiede.

- Es ist zulässig, bei normalen ByVal-und `in` unterschieden überladen zu können.

- Für den Zweck von ohi (überladen, ausblenden, implementieren) verhält sich `in` ähnlich wie ein `out` Parameter.
Es gelten dieselben Regeln.
Beispielsweise muss die über schreibende Methode `in`-Parametern mit `in` Parametern eines Identitäts konvertierbaren Typs abgeglichen werden.

- Für den Zweck der Konvertierung von Delegaten/Lambda-/Methodengruppen verhält sich `in` ähnlich wie ein `out` Parameter.
Lambdas und entsprechende Methoden Gruppen Konvertierungs Kandidaten müssen `in` Parametern des Ziel Delegaten mit `in` Parametern eines Identitäts konvertierbaren Typs abgeglichen werden.

- Zum Zweck der generischen Varianz sind `in` Parameter nicht Variant.

> Hinweis: Es gibt keine Warnungen für `in` Parameter, die Verweis-oder primitive Typen aufweisen.
Im Allgemeinen ist es möglicherweise sinnlos, aber in einigen Fällen muss der Benutzer primitive als `in`übergeben. Beispiele: Überschreiben einer generischen Methode, wie z. b. `Method(in T param)`, wenn `T` `int`ersetzt hat, oder wenn Methoden wie `Volatile.Read(in int location)`
>
> Es ist denkbar, dass Sie über einen Analyzer verfügen, der im Fall einer ineffizienten Verwendung von `in`-Parametern gewarnt wird, aber die Regeln für diese Analyse wären zu unscharf, um Teil einer Sprachspezifikation zu sein.

### <a name="use-of-in-at-call-sites-in-arguments"></a>Verwendung von `in` an Aufrufsites. (`in` Argumente)

Es gibt zwei Möglichkeiten, Argumente an `in` Parameter zu übergeben.

#### <a name="in-arguments-can-match-in-parameters"></a>`in` Argumente können `in` Parametern entsprechen:

Ein Argument mit einem `in`-Modifizierer an der-aufrufssite kann `in` Parametern entsprechen.

```csharp
int x = 1;

void M1<T>(in T x)
{
  // . . .
}

var x = M1(in x);  // in argument to a method

class D
{
    public string this[in Guid index];
}

D dictionary = . . . ;
var y = dictionary[in Guid.Empty]; // in argument to an indexer
```

- `in` Argument muss ein _lesbarer_ lvalue (*) sein.
Beispiel: `M1(in 42)` ist ungültig.

> (*) Das Konzept von [Lvalue/Rvalue](https://en.wikipedia.org/wiki/Value_(computer_science)#lrvalue) variiert je nach Sprache.  
Hier ist nach lvalue ein Ausdruck gemeint, der einen Speicherort darstellt, auf den direkt verwiesen werden kann.
Und Rvalue bedeutet einen Ausdruck, der ein temporäres Ergebnis ergibt, das nicht eigenständig persistent gespeichert wird.  

- Insbesondere ist es zulässig, `readonly` Felder, `in` Parameter oder andere formal `readonly` Variablen als `in` Argumente zu übergeben.
Beispiel: `dictionary[in Guid.Empty]` ist zulässig. `Guid.Empty` ist ein statisches Schreib geschütztes Feld.

- `in` Argument muss eine _Typidentität_ aufweisen, die in den Typ des Parameters konvertiert werden muss.
Beispiel: `M1<object>(in Guid.Empty)` ist ungültig. `Guid.Empty` ist nicht _Identitäts_ Wechsel in `object`

Die Motivation für die oben genannten Regeln besteht darin, dass `in` Argumente das _Aliasing_ der Argument Variablen sicherstellen. Der aufgerufene erhält immer einen direkten Verweis auf denselben Speicherort, der durch das-Argument dargestellt wird.

- in seltenen Fällen, in denen `in` Argumente aufgrund von `await` Ausdrücken, die als Operanden desselben Aufrufes verwendet werden, Stapelüberlauf sein müssen, ist das Verhalten identisch mit dem `out` und `ref`-Argumenten. wenn die Variable nicht auf die Weise übertragen werden kann, wird ein Fehler gemeldet.

Beispiele:
1. `M1(in staticField, await SomethingAsync())` ist gültig.
`staticField` ist ein statisches Feld, auf das mehr als einmal ohne Observable-Nebeneffekte zugegriffen werden kann. Daher können die Reihenfolge von Nebeneffekten und Aliasing Anforderungen bereitgestellt werden.
2. `M1(in RefReturningMethod(), await SomethingAsync())` erzeugt einen Fehler.
`RefReturningMethod()` ist eine `ref` Rückgabe Methode. Ein Methodenaufruf kann beobachtbare Nebeneffekte haben und muss daher vor dem `SomethingAsync()`-Operanden ausgewertet werden. Das Ergebnis des aufforderes ist jedoch ein Verweis, der nicht über den `await` Unterbrechungs Punkt beibehalten werden kann, der die direkte Verweis Anforderung unmöglich macht.

> Hinweis: die Stapelüberlauf Fehler gelten als Implementierungs spezifische Einschränkungen. Daher haben Sie keinen Einfluss auf die Überladungs Auflösung oder den Lambda-Inference.

#### <a name="ordinary-byval-arguments-can-match-in-parameters"></a>Normale ByVal-Argumente können `in` Parametern entsprechen:

Reguläre Argumente ohne modifiziererer können `in`-Parametern entsprechen. In diesem Fall verfügen die Argumente über die gleichen gelockerten Einschränkungen wie normale ByVal-Argumente.

Die Motivation für dieses Szenario besteht darin, dass `in` Parameter in APIs zu Unannehmlichkeiten für den Benutzer führen können, wenn Argumente nicht als direkter Verweis (ex: Literale, berechnete oder `await`-bezogene Ergebnisse oder Argumente, die über spezifischere Typen verfügen,) weitergegeben werden können.  
Alle diese Fälle haben eine triviale Lösung, den Argument Wert in einem temporären lokalen des entsprechenden Typs zu speichern und diesen lokal als `in` Argument zu übergeben.  
Wenn `in` Modifizierer nicht an der aufrufssite vorhanden ist, kann der Code Compiler die gleiche Transformation durchführen, um die Notwendigkeit zu verringern.  

Außerdem gibt es in manchen Fällen, z. b. beim Aufrufen von Operatoren oder bei `in` Erweiterungs Methoden, keine syntaktische Methode, `in` überhaupt anzugeben. Das allein erfordert, dass Sie das Verhalten normaler ByVal-Argumente angeben, wenn Sie `in`-Parametern entsprechen.

Dies gilt insbesondere für:

- Es ist gültig, Rvalues zu übergeben.
Ein Verweis auf eine temporäre wird in diesem Fall übermittelt.
Beispiel:
```csharp
Print("hello");      // not an error.

void Print<T>(in T x)
{
  //. . .
}
```

- implizite Konvertierungen sind zulässig.

> Dies ist ein Sonderfall, wenn ein rvalue-Wert übergeben wird.  

In einem solchen Fall wird ein Verweis auf einen temporären, mit dem Wert konvertierten Wert übermittelt.
Beispiel:
```csharp
Print<int>(Short.MaxValue)     // not an error.
```

- bei einem Empfänger einer `in`-Erweiterungsmethode (im Gegensatz zu `ref` Erweiterungs Methoden) sind Rvalues oder implizite _this-Argument-Konvertierungen_ zulässig.
In einem solchen Fall wird ein Verweis auf einen temporären, mit dem Wert konvertierten Wert übermittelt.
Beispiel:
```csharp
public static IEnumerable<T> Concat<T>(in this (IEnumerable<T>, IEnumerable<T>) arg)  => . . .;

("aa", "bb").Concat<char>()    // not an error.
```
Weitere Informationen zu `ref`/`in` Erweiterungs Methoden finden Sie weiter unten in diesem Dokument.

- ein Argument Überlauf aufgrund von `await` Operanden könnte ggf. "by-Value" überlaufen.
In Szenarien, in denen die Bereitstellung eines direkten Verweises auf das-Argument aufgrund von dazwischen liegenden `await` nicht möglich ist, wird stattdessen eine Kopie des Argument Werts übertragen.  
Beispiel:
```csharp
M1(RefReturningMethod(), await SomethingAsync())   // not an error.
```
Da es sich bei dem Ergebnis eines Nebeneffekten um einen Verweis handelt, der nicht über `await` Unterbrechung hinweg beibehalten werden kann, wird stattdessen ein temporäres mit dem tatsächlichen Wert beibehalten (wie es in einem normalen ByVal-Parameter Fall der Fall wäre).

#### <a name="omitted-optional-arguments"></a>Nicht optionale Argumente ausgelassen

Es ist zulässig, dass ein `in`-Parameter einen Standardwert angibt. Dadurch wird das entsprechende Argument optional.

Wenn Sie das optionale Argument an der aufrufssite weglassen, wird der Standardwert über einen temporären übergeben.

```csharp
Print("hello");      // not an error, same as
Print("hello", c: Color.Black);

void Print(string s, in Color c = Color.Black)
{
    // . . .
}
```

### <a name="aliasing-behavior-in-general"></a>Aliasing-Verhalten im allgemeinen

Ebenso wie `ref` und `out` Variablen sind `in` Variablen Verweise/Aliase auf vorhandene Speicherorte.

Das Schreiben eines `in`-Parameters ist zwar nicht zulässig, kann jedoch unterschiedliche Werte als Nebeneffekte anderer Auswertungen beobachten.

Beispiel:

```C#
static Vector3 v = Vector3.UnitY;

static void Main()
{
    Test(v);
}

static void Test(in Vector3 v1)
{
    Debug.Assert(v1 == Vector3.UnitY);
    // changes v1 deterministically (no races required)
    ChangeV();
    Debug.Assert(v1 == Vector3.UnitX);
}

static void ChangeV()
{
    v = Vector3.UnitX;
}
```

### <a name="in-parameters-and-capturing-of-local-variables"></a>`in` Parameter und die Erfassung von lokalen Variablen.  
Zum Zweck der Lambda-/Async-Erfassung `in` Parameter identisch mit den Parametern `out` und `ref`.

- `in` Parameter können nicht in einem Abschluss aufgezeichnet werden.
- `in` Parameter sind in Iteratormethoden nicht zulässig.
- `in` Parameter sind in Async-Methoden nicht zulässig.

### <a name="temporary-variables"></a>Temporäre Variablen.  
Einige Verwendungsmöglichkeiten von `in` Parameter Übergabe erfordern möglicherweise die indirekte Verwendung einer temporären lokalen Variablen:  
- `in` Argumente werden immer als direkte Aliase weitergegeben, wenn die Aufrufsite `in`verwendet. Temporär wird in diesem Fall nie verwendet.
- `in` Argumente müssen keine direkten Aliase sein, wenn die Aufrufsite `in`nicht verwendet. Wenn das Argument kein lvalue ist, kann ein temporäres verwendet werden.
- `in` Parameter kann einen Standardwert aufweisen. Wenn das entsprechende Argument an der aufrufssite weggelassen wird, wird der Standardwert über einen temporären Wert übermittelt.
- `in` Argumente können implizite Konvertierungen aufweisen, einschließlich derjenigen, die die Identität nicht beibehalten. In diesen Fällen wird ein temporäres verwendet.
- Empfänger von gewöhnlichen Struktur aufrufen sind möglicherweise keine beschreibbaren Lvalues (**vorhandener Fall!** ). In diesen Fällen wird ein temporäres verwendet.

Die Lebensdauer der Argument temporare stimmt mit dem nächstgelegenen Bereich der Aufruf Site überein.

Die formale Lebensdauer temporärer Variablen ist in Szenarien mit escapeanalysen von Variablen, die als Verweis zurückgegeben werden, semantisch signifikant.

### <a name="metadata-representation-of-in-parameters"></a>Metadatendarstellung von `in`-Parametern.
Wenn `System.Runtime.CompilerServices.IsReadOnlyAttribute` auf einen ByRef-Parameter angewendet wird, bedeutet dies, dass der Parameter ein `in` Parameter ist.

Wenn die Methode *abstrakt* oder *virtuell*ist, muss die Signatur solcher Parameter (und nur solcher Parameter) über `modreq[System.Runtime.InteropServices.InAttribute]`verfügen.

**Motivation**: Dies geschieht, um sicherzustellen, dass bei der Überschreibung/Implementierung der `in` Parameter Übereinstimmung vorliegt.

Die gleichen Anforderungen gelten für `Invoke` Methoden in Delegaten.

**Motivation**: Dadurch wird sichergestellt, dass vorhandene Compiler beim Erstellen oder Zuweisen von Delegaten nicht einfach `readonly` ignorieren können.

## <a name="returning-by-readonly-reference"></a>Rückgabe durch einen schreibgeschützten Verweis.

### <a name="motivation"></a>Motivation
Die Motivation für diese Unterfunktion ist ungefähr symmetrisch zu den Gründen für die `in` Parameter-das Kopieren wird vermieden, aber auf der Rückgabe Seite. Vor dieser Funktion hatten eine Methode oder ein Indexer zwei Optionen: 1) Rückgabe als Verweis und verfügbar für mögliche Mutationen oder 2) Rückgabe nach Wert, der zum Kopieren führt.

### <a name="solution-ref-readonly-returns"></a>Lösung (`ref readonly` Returns)  
Die Funktion ermöglicht es einem Member, Variablen als Verweis zurückzugeben, ohne Sie für Mutationen verfügbar zu machen.

### <a name="declaring-ref-readonly-returning-members"></a>Deklarieren `ref readonly` zurück gebenden Membern

Eine Kombination von modifiziererelementen, die auf die Rückgabe Signatur `ref readonly`, wird verwendet, um anzugeben, dass der Member einen schreibgeschützten Verweis zurückgibt.

Für alle Zwecke wird ein `ref readonly` Member als `readonly` Variable behandelt, ähnlich wie `readonly` Feldern und `in`-Parametern.

Beispielsweise werden Felder mit `ref readonly` Member mit einem Strukturtyp alle als `readonly` Variablen klassifiziert. -Es ist zulässig, Sie als `in` Argumente zu übergeben, aber nicht als `ref` oder `out` Argumente.

```csharp
ref readonly Guid Method1()
{
}

Method2(in Method1()); // valid. Can pass as `in` argument.

Method3(ref Method1()); // not valid. Cannot pass as `ref` argument
```

- `ref readonly` Rückgaben sind an denselben Stellen zulässig, wenn `ref` Rückgabe zulässig sind.
Dies schließt Indexer, Delegaten, Lambdas, lokale Funktionen ein.

- Es ist nicht zulässig, `ref`/`ref readonly`/Unterschiede zu überlasten.

- Es ist zulässig, bei normalen ByVal-und `ref readonly` Rückschlag unterschieden zu überlasten.

- Für ohi (überladen, ausblenden, implementieren) ist `ref readonly` ähnlich, unterscheidet sich jedoch von `ref`.
Beispielsweise muss eine Methode, die `ref readonly` eins überschreibt, selbst `ref readonly` sein und über einen Identitäts konvertierbaren Typ verfügen.

- Für den Zweck der Konvertierung von Delegaten/Lambda-/Methodengruppen ist `ref readonly` ähnlich, unterscheidet sich jedoch von `ref`.
Lambdas und entsprechende Methoden Gruppen Konvertierungs Kandidaten müssen `ref readonly` Rückgabe des Ziel Delegaten mit `ref readonly` Rückgabe des Typs, der Identitäts konvertierbar ist, abgleichen.

- Zum Zweck der generischen Varianz sind `ref readonly`-Rückgaben nicht Variant.

> Hinweis: Es gibt keine Warnungen für `ref readonly`-Rückgaben, die Verweis-oder primitive Typen aufweisen.
Im Allgemeinen ist es möglicherweise sinnlos, aber in einigen Fällen muss der Benutzer primitive als `in`übergeben. Beispiele: Überschreiben einer generischen Methode wie `ref readonly T Method()`, wenn `T` als `int`ersetzt wurde.
>
>Es ist denkbar, eine Analyse zu verwenden, die im Falle einer ineffizienten Verwendung von `ref readonly`-Rückgabe gewarnt wird, aber die Regeln für diese Analyse wären zu unscharf, um Teil einer Sprachspezifikation zu sein.

### <a name="returning-from-ref-readonly-members"></a>Zurückgeben von `ref readonly` Membern
Innerhalb des Methoden Texts ist die Syntax identisch mit der regulären Ref-Rückgabe. Der `readonly` wird von der enthaltenden Methode abgeleitet.

Die Motivation besteht darin, dass `return ref readonly <expression>` unnötig lange ist und nur Konflikte im `readonly` Teil zulässt, die immer zu Fehlern führen.
Der `ref` ist jedoch aus Gründen der Konsistenz mit anderen Szenarien erforderlich, in denen etwas durch Strict Aliasing oder durch einen Wert übermittelt wird.

> Anders als bei `in` Parametern gibt `ref readonly` zurück, die nie über eine lokale Kopie zurückgegeben werden. Wenn Sie in Erwägung ziehen, dass die Kopie sofort nach der Rückgabe einer solchen Übung nicht mehr vorhanden ist, wäre es sinnlos und gefährlich. Daher sind `ref readonly`-Rückgabe immer direkte Verweise.

Beispiel:

```csharp
struct ImmutableArray<T>
{
    private readonly T[] array;

    public ref readonly T ItemRef(int i)
    {
        // returning a readonly reference to an array element
        return ref this.array[i];
    }
}

```

- Ein Argument von `return ref` muss ein Lvalue (**vorhandene Regel**) sein.
- Ein Argument `return ref` muss "sicher zur Rückgabe" (**vorhandene Regel**) sein.
- In einem `ref readonly` Member muss ein Argument `return ref` _nicht geschrieben werden können_ .
Beispielsweise kann ein solcher Member Ref-ein Schreib geschütztes Feld oder einen seiner `in` Parameter zurückgeben.

### <a name="safe-to-return-rules"></a>Sichere Rückgaberegeln.
Normale sicher zum Zurückgeben von Regeln für Verweise gelten auch für schreibgeschützte Verweise.

Beachten Sie, dass ein `ref readonly` aus einem regulären `ref` local/Parameter/Return abgerufen werden kann, aber nicht umgekehrt. Andernfalls wird die Sicherheit der `ref readonly` Rückgabe auf die gleiche Weise wie bei regulären `ref` Rückgaben abgeleitet.

Wenn Sie in Erwägung ziehen, dass Rvalues als `in` Parameter übergeben und als `ref readonly` zurückgegeben werden, benötigen wir eine weitere Regel- **Rvalues als Verweis nicht**als "sicher" zurückgegeben.

> Beachten Sie die Situation, in der ein rvalue über eine Kopie an einen `in` Parameter übergeben und dann in Form einer `ref readonly`zurückgegeben wird. Im Zusammenhang mit dem Aufrufer ist das Ergebnis eines solchen Aufrufers ein Verweis auf lokale Daten, sodass die Rückgabe unsicher ist.
> Wenn die Rückgabe von Rvalues nicht sicher ist, wird dieser Fall bereits von der vorhandenen Regel `#6` behandelt.

Beispiel:
```csharp
ref readonly Vector3 Test1()
{
    // can pass an RValue as "in" (via a temp copy)
    // but the result is not safe to return
    // because the RValue argument was not safe to return by reference
    return ref Test2(default(Vector3));
}

ref readonly Vector3 Test2(in Vector3 r)
{
    // this is ok, r is returnable
    return ref r;
}
```

Aktualisierte `safe to return` Regeln:

1.  **die Rückgabe von Refs auf Variablen im Heap ist sicher.**
2.  **ref/in-Parameter können sicher zurückgegeben werden,** 
`in` Parameter natürlich nur als schreibgeschützt zurückgegeben werden können.
3.  **out-Parameter können sicher zurückgegeben** werden (Sie müssen jedoch definitiv zugewiesen werden, wie dies bereits heute der Fall ist).
4.  **instanzstrukturfelder können sicher zurückgegeben werden, solange der Empfänger sicher zurückgegeben werden kann.**
5.  **"This" kann nicht sicher von Strukturmembern zurückgegeben werden.**
6.  **ein Verweis, der von einer anderen Methode zurückgegeben wurde, kann sicher zurückgegeben werden, wenn alle an diese Methode weiter gegebenen Refs/out-Werte, wie Sie sicher zurückgegeben werden konnten**
*insbesondere ist es unerheblich, ob der Empfänger sicher zurückgegeben werden kann, unabhängig davon, ob der Empfänger eine Struktur oder Klasse ist oder als generischer Typparameter typisiert ist.*
7.  **Rvalues können nicht als Verweis zurückgegeben werden.** 
*insbesondere Rvalues sind sicher als in den Parametern zu übergeben.*

> Hinweis: Es gibt weitere Regeln bezüglich der Sicherheit von zurückgegebenen Rückgaben, wenn Ref-like-Typen und ref-reassignments beteiligt sind.
> Die Regeln gelten gleichermaßen für `ref`-und `ref readonly`-Member und werden daher hier nicht erwähnt.

### <a name="aliasing-behavior"></a>Aliasing Verhalten.
`ref readonly` Member bieten das gleiche Aliasing Verhalten wie normale `ref` Member (mit Ausnahme von "schreibgeschützt").
Aus diesem Grund dienen die Erfassung in Lambdas, async, Iteratoren, Stapelüberlauf usw... Es gelten die gleichen Einschränkungen. d. aufgrund der Tatsache, dass die eigentlichen Verweise nicht erfasst werden können, sind solche Szenarios aufgrund der Nebenwirkungen der Mitglieder Auswertung nicht zulässig.

> Es ist zulässig und erforderlich, eine Kopie zu erstellen, wenn `ref readonly` Rückgabe ein Empfänger regulärer Struktur Methoden ist, die `this` als einen normalen beschreibbaren Verweis annehmen. In der Vergangenheit wird in allen Fällen, in denen solche Aufrufe auf die schreibgeschützte Variable angewendet werden, eine lokale Kopie erstellt.

### <a name="metadata-representation"></a>Metadatendarstellung.
Wenn `System.Runtime.CompilerServices.IsReadOnlyAttribute` auf die Rückgabe einer ByRef-Rückgabe Methode angewendet wird, bedeutet dies, dass die Methode einen schreibgeschützten Verweis zurückgibt.

Außerdem muss die Ergebnis Signatur solcher Methoden (und nur dieser Methoden) über `modreq[System.Runtime.CompilerServices.IsReadOnlyAttribute]`verfügen.

**Motivation**: Dadurch wird sichergestellt, dass vorhandene Compiler beim Aufrufen von Methoden mit `ref readonly`-Rückgabe nicht einfach `readonly` ignorieren können.

## <a name="readonly-structs"></a>Schreibgeschützte Strukturen
Kurz gesagt: eine Funktion, die `this`-Parameter aller Instanzmember einer Struktur, mit Ausnahme von Konstruktoren, als `in` Parameter definiert.

### <a name="motivation"></a>Motivation
Der Compiler muss davon ausgehen, dass die Instanz durch einen beliebigen Methoden aufrufin einer Struktur Instanz geändert werden kann. Tatsächlich wird ein Beschreib barer Verweis als `this`-Parameter an die Methode übergeben und dieses Verhalten vollständig aktiviert. Um solche Aufrufe für `readonly` Variablen zuzulassen, werden die Aufrufe auf temporäre Kopien angewendet. Das kann nicht intuitiv sein und manchmal dazu gezwungen, `readonly` aus Leistungsgründen zu verwerfen.  
Beispiel: https://codeblog.jonskeet.uk/2014/07/16/micro-optimization-the-surprising-inefficiency-of-readonly-fields/

Nach dem Hinzufügen der Unterstützung für `in` Parameter und `ref readonly` wird das Problem des defensiven Kopierens zurückgegeben, da schreibgeschützte Variablen häufiger verwendet werden.

### <a name="solution"></a>Lösung
Lässt `readonly` Modifizierer für Struktur Deklarationen zu, was dazu führen würde, dass `this` als `in` Parameter für alle strukturinstanzmethoden mit Ausnahme von Konstruktoren behandelt werden.

```csharp
static void Test(in Vector3 v1)
{
    // no need to make a copy of v1 since Vector3 is a readonly struct
    System.Console.WriteLine(v1.ToString());
}

readonly struct Vector3
{
    . . .

    public override string ToString()
    {
        // not OK!!  `this` is an `in` parameter
        foo(ref this.X);

        // OK
        return $"X: {X}, Y: {Y}, Z: {Z}";
    }
}
```

### <a name="restrictions-on-members-of-readonly-struct"></a>Einschränkungen für Mitglieder der schreibgeschützten Struktur
- Instanzfelder einer schreibgeschützten Struktur müssen schreibgeschützt sein.  
**Motivation:** kann nur in extern, aber nicht über Member geschrieben werden.
- Instanzeigenschaften einer schreibgeschützten Struktur müssen nur abgerufen werden.  
**Motivation:** die Folge der Einschränkung für Instanzfelder.
- Die schreibgeschützte Struktur darf keine Feld ähnlichen Ereignisse deklarieren.  
**Motivation:** die Folge der Einschränkung für Instanzfelder.

### <a name="metadata-representation"></a>Metadatendarstellung.
Wenn `System.Runtime.CompilerServices.IsReadOnlyAttribute` auf einen Werttyp angewendet wird, bedeutet dies, dass der Typ ein `readonly struct`ist.

Dies gilt insbesondere für:
-  Die Identität des `IsReadOnlyAttribute` Typs ist unwichtig. Sie kann bei Bedarf von dem Compiler in die enthaltende Assembly eingebettet werden.

## <a name="refin-extension-methods"></a>`ref`/`in`-Erweiterungs Methoden
Es gibt tatsächlich einen vorhandenen Vorschlag (https://github.com/dotnet/roslyn/issues/165) und den entsprechenden Prototype-PR (https://github.com/dotnet/roslyn/pull/15650).
Ich möchte nur bestätigen, dass diese Idee nicht völlig neu ist. Dies ist jedoch hier von Bedeutung, da `ref readonly` das am häufigsten strittige Problem zu diesen Methoden entfernt, was mit Rvalue-Empfängern zu tun ist.

Die allgemeine Idee besteht darin, Erweiterungs Methoden zuzulassen, den `this` Parameter als Verweis zu verwenden, solange der Typ bekannt ist, dass es sich um einen Strukturtyp handelt.

```csharp
public static void Extension(ref this Guid self)
{
    // do something
}
```

Die Gründe für das Schreiben solcher Erweiterungs Methoden sind hauptsächlich:  
1.  Kopiervorgang vermeiden, wenn der Empfänger eine große Struktur ist
2.  Verändernde Erweiterungs Methoden für Strukturen zulassen

Die Gründe, warum wir dies nicht für Klassen zulassen möchten.  
1.  Dies wäre ein sehr eingeschränkter Zweck.
2.  Es würde eine lange bestehende invariante unterbrechen, dass ein Methodenaufruf einen nicht`null` Empfänger nicht umwandeln kann, um nach dem Aufruf `null` werden zu können.
> Tatsächlich kann eine nicht`null` Variable nicht `null` werden, es sei denn, Sie wird _explizit_ von `ref` oder `out`zugewiesen oder übermittelt.
> Dadurch wird die Lesbarkeit oder andere Formen von "kann dies eine NULL hier-Analyse sein" erheblich unterstützt.
3.  Es wäre schwierig, mit der Semantik "einmal auswerten" der Semantik von NULL-bedingten Zugriffen zu stimmen.
Beispiel: `obj.stringField?.RefExtension(...)` müssen eine Kopie von `stringField` erfassen, um die NULL-Überprüfung aussagekräftig zu machen, aber dann werden Zuweisungen zum `this` in refextension nicht wieder in das Feld übernommen.

Eine Möglichkeit zum Deklarieren von Erweiterungs Methoden für **Strukturen** , die das erste Argument als Verweis akzeptieren, war eine langfristige Anforderung. Eine der blockierenden Aspekte lautete: "Was geschieht, wenn der Empfänger kein lvalue ist?".

- Es gibt einen Präzedenzfall, dass jede Erweiterungsmethode auch als statische Methode aufgerufen werden kann (manchmal ist Sie die einzige Möglichkeit, die Mehrdeutigkeit aufzulösen). Es würde vorschreiben, dass Rvalue-Empfänger nicht zulässig sein sollten.
- Andererseits ist es ratsam, in ähnlichen Situationen, in denen strukturinstanzmethoden beteiligt sind, einen Aufruf auf eine Kopie vorzunehmen.

Der Grund, warum das implizite kopieren vorhanden ist, besteht darin, dass die Mehrzahl der Struktur Methoden nicht die Struktur tatsächlich ändert und nicht in der Lage ist, dies anzugeben. Die praktischste Lösung bestand daher darin, den Aufruf auf eine Kopie zu übernehmen, aber diese Vorgehensweise ist bekannt, um die Leistung zu beeinträchtigen und Fehler zu verursachen.

Mit der Verfügbarkeit von `in`-Parametern kann eine Erweiterung nun der Absicht signalisieren. Daher kann das Problem gelöst werden, indem `ref` Erweiterungen mit beschreibbaren Empfängern aufgerufen werden müssen, während `in` Erweiterungen implizites kopieren bei Bedarf zulassen.

```csharp
// this can be called on either RValue or an LValue
public static void Reader(in this Guid self)
{
    // do something nonmutating.
    WriteLine(self == default(Guid));
}

// this can be called only on an LValue
public static void Mutator(ref this Guid self)
{
    // can mutate self
    self = new Guid();
}
```

### <a name="in-extensions-and-generics"></a>`in` Erweiterungen und Generika.
Der Zweck `ref` Erweiterungs Methoden besteht darin, den Empfänger direkt oder durch Aufrufen von mutierenden Membern zu mutieren. Daher sind `ref this T` Erweiterungen zulässig, solange `T` auf eine Struktur beschränkt ist.

Auf der anderen Seite `in` Erweiterungs Methoden speziell vorhanden sind, um implizites kopieren zu verringern. Allerdings muss die Verwendung eines `in T`-Parameters über einen Schnittstellenmember erfolgen. Da alle Schnittstellenmember als muating angesehen werden, ist für jede solche Verwendung eine Kopie erforderlich. -Anstatt das Kopieren zu verringern, wäre der Effekt das Gegenteil. Daher ist `in this T` nicht zulässig, wenn `T` ein generischer Typparameter ist, unabhängig von Einschränkungen.

### <a name="valid-kinds-of-extension-methods-recap"></a>Gültige Arten von Erweiterungs Methoden (Recap):
Die folgenden Formen der `this` Deklaration in einer Erweiterungsmethode sind jetzt zulässig:
1) `this T arg` reguläre ByVal-Erweiterung. (**vorhandener Fall**)
- "T" kann ein beliebiger Typ sein, einschließlich Verweis Typen oder Typparametern.
Die Instanz ist nach dem-Befehl dieselbe Variable.
Lässt implizite Konvertierungen _dieser Art von Argument Konvertierungen_ zu.
Kann für Rvalues aufgerufen werden.

- `in this T self` - `in` Erweiterung.
"T" muss ein tatsächlicher Strukturtyp sein.
Die Instanz ist nach dem-Befehl dieselbe Variable.
Lässt implizite Konvertierungen _dieser Art von Argument Konvertierungen_ zu.
Kann für Rvalues aufgerufen werden (kann bei Bedarf für eine Temp aufgerufen werden).

- `ref this T self` - `ref` Erweiterung.
T muss ein Strukturtyp oder ein generischer Typparameter sein, der als Struktur eingeschränkt ist.
Die-Instanz kann durch den Aufruf von geschrieben werden.
Lässt nur Identitäts Konvertierungen zu.
Muss für beschreibbaren lvalue aufgerufen werden. (nie über eine Temp aufgerufen).

## <a name="readonly-ref-locals"></a>Schreibgeschützte lokale Ref-Variablen.

### <a name="motivation"></a>Ationen.
Nachdem `ref readonly` Mitglieder eingeführt wurden, war es klar, dass Sie mit der passenden Art von lokalem paar kombiniert werden müssen. Durch die Auswertung eines Members können Nebeneffekte erzeugt oder beobachtet werden. wenn das Ergebnis mehrmals verwendet werden muss, muss es daher gespeichert werden. Normale `ref` lokale unterstützen hier nicht, da Ihnen keine `readonly` Referenz zugewiesen werden kann.   

### <a name="solution"></a>Not.
Ermöglicht das Deklarieren von `ref readonly` lokalen Dabei handelt es sich um eine neue Art von `ref` lokalen Variablen, die nicht geschrieben werden können. Daher können `ref readonly` lokale Verweise auf schreibgeschützte Variablen akzeptieren, ohne diese Variablen für Schreibvorgänge verfügbar zu machen.

### <a name="declaring-and-using-ref-readonly-locals"></a>Deklarieren und Verwenden von `ref readonly` lokalen Variablen.

Die Syntax dieser lokalen Variablen verwendet `ref readonly` Modifizierer an der Deklarations Site (in dieser bestimmten Reihenfolge). Ähnlich wie bei den normalen `ref` lokalen Variablen müssen `ref readonly` lokalen Variablen bei der Deklaration als ref-initialisiert werden. Im Gegensatz zu regulären `ref` lokalen Variablen können `ref readonly` lokal auf `readonly` Lvalues verweisen, wie `in` Parameter, `readonly` Felder und `ref readonly` Methoden.

Für alle Zwecke wird ein `ref readonly` local als `readonly` Variable behandelt. Die meisten Einschränkungen in Bezug auf die Verwendung sind identisch mit der Verwendung von `readonly` Feldern oder `in` Parametern.

Beispielsweise werden Felder eines `in`-Parameters mit einem Strukturtyp rekursiv als `readonly` Variablen klassifiziert.   

```csharp
static readonly ref Vector3 M1() => . . .

static readonly ref Vector3 M1_Trace()
{
    // OK
    ref readonly var r1 = ref M1();

    // Not valid. Need an LValue
    ref readonly Vector3 r2 = ref default(Vector3);

    // Not valid. r1 is readonly.
    Mutate(ref r1);

    // OK.
    Print(in r1);

    // OK.
    return ref r1;
}
```

### <a name="restrictions-on-use-of-ref-readonly-locals"></a>Einschränkungen bei der Verwendung von `ref readonly` lokalen Variablen
Mit Ausnahme der `readonly` Natur Verhalten sich `ref readonly` lokal wie normale `ref` lokale und unterliegen exakt denselben Einschränkungen.  
Beispielsweise gelten Einschränkungen im Zusammenhang mit der Erfassung von Abschlüssen, das Deklarieren in `async` Methoden oder die `safe-to-return` Analyse gleichermaßen für `ref readonly` lokale.

## <a name="ternary-ref-expressions-aka-conditional-lvalues"></a>TERNÄRE `ref` Ausdrücke. (Alias "Conditional Lvalues")

### <a name="motivation"></a>Motivation
Die Verwendung von `ref` und `ref readonly` lokalen Variablen gab an, dass solche lokalen Variablen auf der Grundlage einer Bedingung mit einer oder einer anderen Zielvariablen ref-initialisiert werden müssen.

Eine typische Problem Umgehung besteht darin, eine Methode wie die folgende einzuführen:

```csharp
ref T Choice(bool condition, ref T consequence, ref T alternative)
{
    if (condition)
    {
         return ref consequence;
    }
    else
    {
         return ref alternative;
    }
}
```

Beachten Sie, dass `Choice` keine genaue Ersetzung eines ternären ist, da _alle_ Argumente an der aufrufssite ausgewertet werden müssen, was zu nicht intuitivem Verhalten und Fehlern geführt hat.

Folgendes funktioniert nicht wie erwartet:

```csharp
    // will crash with NRE because 'arr[0]' will be executed unconditionally
    ref var r = ref Choice(arr != null, ref arr[0], ref otherArr[0]);
```

### <a name="solution"></a>Lösung
Hiermit wird eine spezielle Art von bedingtem Ausdruck zugelassen, die basierend auf einer Bedingung zu einem Verweis auf ein Lvalue-Argument ausgewertet wird.

### <a name="using-ref-ternary-expression"></a>Verwenden `ref` ternären Ausdrucks.

Die Syntax für die `ref`-Konfiguration eines bedingten Ausdrucks ist `<condition> ? ref <consequence> : ref <alternative>;`

Genau wie bei dem normalen bedingten Ausdruck nur `<consequence>` oder `<alternative>` wird in Abhängigkeit vom Ergebnis des Ausdrucks der booleschen Bedingung ausgewertet.

Im Gegensatz zum normalen bedingten Ausdruck `ref` Bedingungs Ausdruck:
- erfordert, dass `<consequence>` und `<alternative>` Lvalues sind.
- `ref` bedingte Ausdruck selbst ist ein Lvalue und
- `ref` bedingte Ausdruck ist beschreibbar, wenn sowohl `<consequence>` als auch `<alternative>` beschreibbare Lvalues sind.

Beispiele:  
`ref` ternäre ist ein Lvalue und kann daher übermittelt/zugewiesen/als Verweis zurückgegeben werden.
```csharp
     // pass by reference
     foo(ref (arr != null ? ref arr[0]: ref otherArr[0]));

     // return by reference
     return ref (arr != null ? ref arr[0]: ref otherArr[0]);
```

Dabei kann es sich um einen lvalue-Wert handeln, der ebenfalls zugewiesen werden kann.
```csharp
     // assign to
     (arr != null ? ref arr[0]: ref otherArr[0]) = 1;

     // error. readOnlyField is readonly and thus conditional expression is readonly
     (arr != null ? ref arr[0]: ref obj.readOnlyField) = 1;
```

Kann als Empfänger eines Methoden Aufrufes verwendet werden und das Kopieren bei Bedarf überspringen.
```csharp
     // no copies
     (arr != null ? ref arr[0]: ref otherArr[0]).StructMethod();

     // invoked on a copy.
     // The receiver is `readonly` because readOnlyField is readonly.
     (arr != null ? ref arr[0]: ref obj.readOnlyField).StructMethod();

     // no copies. `ReadonlyStructMethod` is a method on a `readonly` struct
     // and can be invoked directly on a readonly receiver
     (arr != null ? ref arr[0]: ref obj.readOnlyField).ReadonlyStructMethod();
```

`ref` ternäre kann auch in einem regulären (not Ref) Kontext verwendet werden.
```csharp
     // only an example
     // a regular ternary could work here just the same
     int x = (arr != null ? ref arr[0]: ref otherArr[0]);
```

### <a name="drawbacks"></a>Nachteile
[drawbacks]: #drawbacks

Ich kann zwei wichtige Argumente für erweiterte Unterstützung für Verweise und schreibgeschützte Verweise sehen:

1) Die hier gelösten Probleme sind sehr alt. Warum lösen Sie diese jetzt plötzlich, besonders weil Sie vorhandenen Code nicht unterstützen würde?

Wie wir sehen C# , und .net in neuen Domänen verwendet wird, werden einige Probleme deutlicher.  
Als Beispiele für Umgebungen, die kritischer als der Durchschnitt bei Berechnungs überschreitungs Werten sind, kann ich

* Cloud-/Datacenter-Szenarios, in denen die Berechnung abgerechnet wird und die Reaktionsfähigkeit einen Wettbewerbsvorteil ist.
* Spiele/VR/AR mit Soft-Echtzeitanforderungen bei Latenzen     

Mit diesem Feature werden keine der vorhandenen stärken, wie z. b. Typsicherheit, geopfert, während einige gängige Szenarien einen geringeren Aufwand ermöglichen.

2) Können wir in angemessener Weise sicherstellen, dass der aufgerufene die Regeln wieder gibt, wenn er sich für `readonly` Verträge entscheidet?

Bei der Verwendung `out`haben wir eine vergleichbare Vertrauensstellung. Eine falsche Implementierung von `out` kann ein nicht bestimmtes Verhalten verursachen, aber in der Realität kommt es selten vor.  

Wenn die formalen Überprüfungs Regeln, die mit `ref readonly` vertraut sind, das Vertrauensstellungs Problem weiter verringern.

### <a name="alternatives"></a>Alternativen
[alternatives]: #alternatives

Der wichtigste konkurrierende Entwurf ist wirklich "Do Nothing".

### <a name="unresolved-questions"></a>Nicht aufgelöste Fragen
[unresolved]: #unresolved-questions

### <a name="design-meetings"></a>Treffen von Besprechungen

https://github.com/dotnet/csharplang/blob/master/meetings/2017/LDM-2017-02-22.md https://github.com/dotnet/csharplang/blob/master/meetings/2017/LDM-2017-03-01.md https://github.com/dotnet/csharplang/blob/master/meetings/2017/LDM-2017-08-28.md https://github.com/dotnet/csharplang/blob/master/meetings/2017/LDM-2017-09-25.md https://github.com/dotnet/csharplang/blob/master/meetings/2017/LDM-2017-09-27.md
