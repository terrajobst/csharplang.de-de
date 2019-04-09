---
ms.openlocfilehash: af7af574814dc04ee3ece0396b7ae5f86b3ec8eb
ms.sourcegitcommit: 3fc033b6e98ed7ecdf46a85c79b00a3a3ddcf963
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/18/2019
ms.locfileid: "47229644"
---
# <a name="classes"></a>Klassen

Eine Klasse ist eine Datenstruktur, die Daten Mitglieder (Konstanten und Feldern), Funktionsmember (Methoden, Eigenschaften, Ereignisse, Indexer, Operatoren, Instanzkonstruktoren, Destruktoren und statische Konstruktoren) und geschachtelte Typen enthalten kann. Klassentypen unterstützen die Vererbung, einen Mechanismus, bei dem eine abgeleitete Klasse erweitern und spezialisiert eine Basisklasse kann.

## <a name="class-declarations"></a>Klassendeklarationen

Ein *Class_declaration* ist eine *Type_declaration* ([Typdeklarationen](namespaces.md#type-declarations)), die eine neue Klasse deklariert.

```antlr
class_declaration
    : attributes? class_modifier* 'partial'? 'class' identifier type_parameter_list?
      class_base? type_parameter_constraints_clause* class_body ';'?
    ;
```

Ein *Class_declaration* besteht aus einer optionalen Gruppe von *Attribute* ([Attribute](attributes.md)), gefolgt von einer optionalen Gruppe von *Class_modifier*s ([Klasse Modifizierer](classes.md#class-modifiers)), gefolgt von einem optionalen `partial` Modifizierer, gefolgt vom Schlüsselwort `class` und ein *Bezeichner* mit dem Namen der Klasse, gefolgt von einem optionale *Type_parameter_list* ([Typparameter](classes.md#type-parameters)), gefolgt von einem optionalen *Class_base* Spezifikation ([Klasse Basis Spezifikation](classes.md#class-base-specification)), gefolgt von einer optionalen Gruppe von *Type_parameter_constraints_clause*s ([Geben Sie die Einschränkungen für Typparameter](classes.md#type-parameter-constraints)), gefolgt von einem *Class_body*  ([Klasse Text](classes.md#class-body)), optional gefolgt durch ein Semikolon.

Eine Klassendeklaration kann nicht bereitstellen *Type_parameter_constraints_clause*s es sei denn, sie verfügt auch über eine *Type_parameter_list*.

Eine Klassendeklaration, die bereitstellt eine *Type_parameter_list* ist eine ***generischen Klassendeklaration***. Darüber hinaus ist jeder Klasse, die in der Deklaration einer generischen Klasse oder einer generischen Strukturdeklaration geschachtelt selbst die Deklaration einer generischen Klasse, da der Typparameter für den enthaltenden Typ angegeben werden müssen, um einen konstruierten Typ zu erstellen.

### <a name="class-modifiers"></a>Klassenmodifizierer

Ein *Class_declaration* kann optional eine Sequenz von Klassenmodifizierer enthalten:

```antlr
class_modifier
    : 'new'
    | 'public'
    | 'protected'
    | 'internal'
    | 'private'
    | 'abstract'
    | 'sealed'
    | 'static'
    | class_modifier_unsafe
    ;
```

Es ist ein Fehler während der Kompilierung für den gleichen Modifizierer für mehrere Male in einer Klassendeklaration angezeigt werden.

Die `new` -Modifizierer ist für geschachtelte Klassen zulässig. Es gibt an, dass die Klasse mit dem gleichen Namen, Blendet einen geerbten Member aus wie in beschrieben [der new-Modifizierer](classes.md#the-new-modifier). Es ist ein Fehler während der Kompilierung für die `new` Modifizierer, um in einer Klassendeklaration angezeigt werden, die nicht auf eine Deklaration der geschachtelten Klasse ist.

Die `public`, `protected`, `internal`, und `private` Modifizierern steuern den Zugriff auf die Klasse. Je nach Kontext, in der Deklaration der Klasse steht, einige dieser Modifizierer kann nicht gestattet ([deklariert Barrierefreiheit](basic-concepts.md#declared-accessibility)).

Die `abstract`, `sealed` und `static` Modifizierer werden in den folgenden Abschnitten erläutert.

#### <a name="abstract-classes"></a>Abstrakte Klassen

Die `abstract` Modifizierer wird verwendet, um anzugeben, dass eine Klasse unvollständig ist und es wird nur als Basisklasse verwendet werden soll. Eine abstrakte Klasse unterscheidet sich von einer nicht abstrakten Klasse gibt folgenden Möglichkeiten:

*  Eine abstrakte Klasse kann nicht direkt instanziiert werden, und es ist ein Fehler während der Kompilierung mit der `new` Operator für eine abstrakte Klasse. Es ist, zwar möglich, Variablen und Werte, deren Typen während der Kompilierung abstrakt sind diesen Variablen und Werten handelt es sich entweder unbedingt `null` oder enthält Verweise auf Instanzen von nicht abstrakte Klassen, die von der abstrakten Typen abgeleitet.
*  Eine abstrakte Klasse ist zulässig (jedoch nicht erforderlich) abstrakte Member enthalten.
*  Eine abstrakte Klasse kann nicht versiegelt werden.

Wenn eine nicht abstrakte Klasse von einer abstrakten Klasse abgeleitet ist, muss der nicht abstrakten Klasse Implementierungen aller geerbten abstrakten Member, und überschreiben die abstrakte Member enthalten. Im Beispiel
```csharp
abstract class A
{
    public abstract void F();
}

abstract class B: A
{
    public void G() {}
}

class C: B
{
    public override void F() {
        // actual implementation of F
    }
}
```
die abstrakte Klasse `A` stellt eine abstrakte Methode `F`. Klasse `B` führt eine zusätzliche Methode `G`, da es eine Implementierung bereitstellen, nicht jedoch `F`, `B` muss auch als abstrakt deklariert werden. Klasse `C` überschreibt `F` und eine tatsächliche Implementierung bereitstellt. Da es sich um keine abstrakten Member in `C`, `C` ist zulässig (jedoch nicht erforderlich), der nicht abstrakt ist.

#### <a name="sealed-classes"></a>Versiegelte Klassen

Die `sealed` Modifizierer wird verwendet, um zu verhindern, dass bei der Ableitung von einer Klasse. Ein Fehler während der Kompilierung tritt auf, wenn eine versiegelte Klasse als Basisklasse einer anderen Klasse angegeben wird.

Eine versiegelte Klasse darf nicht auch eine abstrakte Klasse sein.

Die `sealed` Modifizierer wird in erster Linie zum Verhindern von unbeabsichtigten ableitungen, aber sie können zudem bestimmte Optimierungen für die Laufzeit. Insbesondere, da eine versiegelte Klasse alle abgeleiteten Klassen nie damit bekannt ist, ist es möglich, eine virtuelle Funktion, die von Memberaufrufen für versiegelte Klasse-Instanzen in nicht-virtuelle Aufrufe zu transformieren.

#### <a name="static-classes"></a>Statische Klassen

Die `static` Modifizierer wird verwendet, um die Klasse deklariert wird, als Markieren einer ***statische Klasse***. Eine statische Klasse kann nicht instanziiert werden, kann nicht als Typ verwendet werden und kann nur statische Member enthalten. Nur eine statische Klasse Deklarationen von Erweiterungsmethoden enthalten kann ([Erweiterungsmethoden](classes.md#extension-methods)).

Eine statische Klasse-Deklaration ist jedoch mit folgenden Einschränkungen:

*  Eine statische Klasse enthält möglicherweise keine `sealed` oder `abstract` Modifizierer. Beachten Sie jedoch, da eine statische Klasse kann nicht instanziiert oder abgeleitet werden, verhält sich als wäre es versiegelt und abstrakt.
*  Eine statische Klasse enthält möglicherweise keine *Class_base* Spezifikation ([Klasse basisspezifizierung](classes.md#class-base-specification)) und nicht explizit angeben, eine Basisklasse oder eine Liste der implementierten Schnittstellen. Eine statische Klasse erbt implizit vom Typ `object`.
*  Eine statische Klasse kann nur statische Member enthalten ([statische und Instanzmember](classes.md#static-and-instance-members)). Beachten Sie, dass Konstanten und geschachtelte Typen als statische Member klassifiziert sind.
*  Eine statische Klasse sind keine Elemente mit `protected` oder `protected internal` Barrierefreiheit deklariert.

Es ist ein Fehler während der Kompilierung, diese Einschränkungen zu verletzen.

Eine statische Klasse verfügt über keine Instanzkonstruktoren. Es ist nicht möglich, Deklaration eines Instanzkonstruktors in einer statischen Klasse und kein Standardkonstruktor für die Instanz ([Standardkonstruktoren](classes.md#default-constructors)) wird bereitgestellt, um eine statische Klasse.

Die Member einer statischen Klasse sind nicht automatisch statisch, und die Memberdeklarationen müssen explizit eine `static` Modifizierer (mit Ausnahme von Konstanten und geschachtelte Typen). Wenn eine Klasse in einer statischen äußeren Klasse geschachtelt ist, die geschachtelte Klasse ist keine statische Klasse, sofern es explizit enthält eine `static` Modifizierer.

__Statische Klasse Verweistypen__

Ein *Namespace_or_type_name* ([Namespace und Typnamen](basic-concepts.md#namespace-and-type-names)) ist auf eine statische Klasse verweisen, wenn zulässig

*  Die *Namespace_or_type_name* ist die `T` in einem *Namespace_or_type_name* des Formulars `T.I`, oder
*  Die *Namespace_or_type_name* ist die `T` in einem *Typeof_expression* ([Argumentlisten](expressions.md#argument-lists)1) des Formulars `typeof(T)`.

Ein *Primary_expression* ([Funktionsmember](expressions.md#function-members)) ist auf eine statische Klasse verweisen, wenn zulässig

*  Die *Primary_expression* ist die `E` in einem *Member_access* ([Überprüfungen zur Kompilierzeit der dynamischen überladungsauflösung](expressions.md#compile-time-checking-of-dynamic-overload-resolution)) des Formulars `E.I`.

In einem anderen Kontext ist es ein Fehler während der Kompilierung, um auf eine statische Klasse verweisen. Es ist z. B. einen Fehler für eine statische Klasse als eine Basisklasse, die einen einzelnen Typ verwendet werden soll ([geschachtelte Typen](classes.md#nested-types)) ein Element, ein generisches Typargument oder eine Einschränkung für einen Parameter. Ebenso eine statische Klasse kann nicht in einen Arraytyp, ein Zeigertyp verwendet werden kann eine `new` Ausdruck, der ein Cast-Ausdruck, ein `is` Ausdruck eine `as` Ausdruck eine `sizeof` Ausdruck oder eine Default-Wertausdruck.

### <a name="partial-modifier"></a>Ein partial-Modifizierer

Die `partial` Modifizierer verwendet, um anzugeben, das von diesem *Class_declaration* ist eine Deklaration der partiellen Typ. Mehrere partielle Typdeklarationen mit dem gleichen Namen in einer einschließenden Namespace oder Typ Deklaration kombiniert werden, um eine Typdeklaration Form, in angegebenen gemäß den Regeln [partielle Typen](classes.md#partial-types).

Müssen die Deklaration einer Klasse, die über separate Segmente des Programmtexts verteilt kann nützlich sein, wenn diese Segmente erstellt oder in unterschiedlichen Kontexten verwaltet werden. Z. B. möglicherweise ein Teil einer Klassendeklaration generiert, Computer, während die andere manuell erstellt wird. Text Trennung zwischen den beiden verhindert, dass Updates von einem von in Konflikt stehende mit Updates von anderen.

### <a name="type-parameters"></a>Typparameter

Ein Typparameter ist ein einfacher Bezeichner, der einen Platzhalter für ein Typargument, das zum Erstellen eines konstruierten Typs bezeichnet. Ein Typparameter ist eine formale Platzhalter für einen Typ, der später angegeben werden, wird. Im Gegensatz dazu ein Typargument ([Typargumente](types.md#type-arguments)) ist der tatsächliche Typ, der für den Typparameter ersetzt wird, wenn ein konstruierter Typ erstellt wird.

```antlr
type_parameter_list
    : '<' type_parameters '>'
    ;

type_parameters
    : attributes? type_parameter
    | type_parameters ',' attributes? type_parameter
    ;

type_parameter
    : identifier
    ;
```

Jeden Typparameter in einer Klassendeklaration definiert einen Namen im Deklarationsbereich ([Deklarationen](basic-concepts.md#declarations)) dieser Klasse. Also, es kann nicht den gleichen Namen wie ein anderer Typparameter aufweisen oder ein Member, die in dieser Klasse deklariert. Ein Typparameter kann nicht den gleichen Namen wie der Typ selbst aufweisen.

### <a name="class-base-specification"></a>Die Basisspezifikationen Klasse

Eine Klassendeklaration enthalten möglicherweise eine *Class_base* -Spezifikation, die die direkte Basisklasse der Klasse und die Schnittstellen definiert ([Schnittstellen](interfaces.md)) direkt von der Klasse implementiert.

```antlr
class_base
    : ':' class_type
    | ':' interface_type_list
    | ':' class_type ',' interface_type_list
    ;

interface_type_list
    : interface_type (',' interface_type)*
    ;
```

Die Basisklasse, die in einer Klassendeklaration angegeben, kann ein konstruierten Klasse-Typ sein ([Typen konstruiert](types.md#constructed-types)). Eine Basisklasse darf nicht Typparameter in ihren eigenen sein, obwohl er die Typparameter umfassen kann, die im Gültigkeitsbereich befinden.

```csharp
class Extend<V>: V {}            // Error, type parameter used as base class
```

#### <a name="base-classes"></a>Basisklassen

Wenn eine *Class_type* befindet sich auf die *Class_base*, wird die direkte Basisklasse der Klasse deklariert wird. Wenn eine Deklaration der Klasse keine *Class_base*, oder wenn die *Class_base* Listen nur Schnittstellentypen, die direkte Basisklasse wird als `object`. Eine Klasse erbt von der direkten Basisklasse, Member, wie in beschrieben [Vererbung](classes.md#inheritance).

Im Beispiel
```csharp
class A {}

class B: A {}
```
Klasse `A` heißt es, die direkte Basisklasse von `B`, und `B` gilt als abgeleitet werden `A`. Da `A` ist keine direkte Basisklasse nicht explizit angeben, die direkte Basisklasse ist implizit `object`.

Für einen konstruierten Klasse, wenn eine Basisklasse in der generischen Klassendeklaration angegeben ist die Basisklasse des konstruierten Typs wird abgerufen, indem ersetzen, für die einzelnen *Type_parameter* in der entsprechenden Basisklassendeklaration *Type_argument* des konstruierten Typs. Erhalten die generische Klassendeklarationen
```csharp
class B<U,V> {...}

class G<T>: B<string,T[]> {...}
```
die Basisklasse des konstruierten Typs `G<int>` wäre `B<string,int[]>`.

Die direkte Basisklasse eines Klassentyps muss mindestens dieselben zugriffsmöglichkeiten wie der Klassentyp selbst bieten ([Barrierefreiheit Domänen](basic-concepts.md#accessibility-domains)). Es ist z. B. ein Fehler während der Kompilierung für eine `public` Klasse abgeleitet eine `private` oder `internal` Klasse.

Die direkte Basisklasse eines Klassentyps muss keine der folgenden Typen sein: `System.Array`, `System.Delegate`, `System.MulticastDelegate`, `System.Enum`, oder `System.ValueType`. Darüber hinaus Deklaration einer generischen Klasse können keine `System.Attribute` als eine direkte oder indirekte Basisklasse.

Beim Bestimmen der Bedeutung der direkten Basisklasse-Spezifikation `A` einer Klasse `B`, die direkte Basisklasse von `B` wird vorübergehend als `object`. Intuitiv Dadurch wird sichergestellt, dass die Bedeutung einer basisklassenspezifikation rekursiv kann nicht auf sich selbst abhängig sind. Beispiel:
```csharp
class A<T> {
   public class B {}
}

class C : A<C.B> {}
```
ist fehlerhaft. seit in der Basisklasse-Spezifikation `A<C.B>` die direkte Basisklasse von `C` gilt `object`, und somit (durch die Regeln zur [Namespace und Typnamen](basic-concepts.md#namespace-and-type-names)) `C` nicht gilt kein Element `B`.

Die Basisklassen eines Klassentyps sind die direkte Basisklasse und ihre Basisklassen. Das heißt, ist die Reihe von Basisklassen den transitiven Abschluss von der direkten basisklassenbeziehung. Wie im Beispiel oben die Basisklassen `B` sind `A` und `object`. Im Beispiel
```csharp
class A {...}

class B<T>: A {...}

class C<T>: B<IComparable<T>> {...}

class D<T>: C<T[]> {...}
```
die Basisklassen `D<int>` sind `C<int[]>`, `B<IComparable<int[]>>`, `A`, und `object`.

Mit Ausnahme der Klasse `object`, jeder Klassentyp verfügt über genau eine direkte Basisklasse. Die `object` Klasse hat keine direkte Basisklasse und ist die ultimative Basisklasse aller anderen Klassen.

Wenn eine Klasse `B` leitet sich von einer Klasse `A`, es ist ein Fehler während der Kompilierung für `A` hängt `B`. Eine Klasse ***hängt direkt von*** seine direkte Basisklasse (sofern vorhanden) und ***hängt direkt von*** der Klasse, in dem es sofort geschachtelt ist (sofern vorhanden). Dank dieser der vollständige Satz von Klassen, die von dem eine Klasse abhängig ist, wird den reflexiv und transitiv Abschluss von der ***hängt direkt von*** Beziehung.

Im Beispiel
```csharp
class A: A {}
```
ist fehlerhaft, da die Klasse von sich selbst abhängig ist. Ebenso, Beispiel
```csharp
class A: B {}
class B: C {}
class C: A {}
```
ist fehlerhaft, da die Klassen zirkulär voneinander abhängig. Zum Schluss das Beispiel
```csharp
class A: B.C {}

class B: A
{
    public class C {}
}
```
führt zu einem Kompilierzeitfehler, da `A` hängt `B.C` (seine direkte Basisklasse), abhängig vom `B` (der unmittelbar einschließenden Klasse), zirkulär abhängig vom `A`.

Beachten Sie, dass eine Klasse nicht von den Klassen abhängt, die darin geschachtelt sind. Im Beispiel
```csharp
class A
{
    class B: A {}
}
```
`B` hängt von `A` (da `A` ist seine direkte Basisklasse und die Sie unmittelbar umschließende Klasse), aber `A` hängt nicht `B` (da `B` ist weder eine Basisklasse als auch von einer einschließenden Klasse `A` ). Im Beispiel ist daher gültig.

Es ist nicht möglich, für die Ableitung einer `sealed` Klasse. Im Beispiel
```csharp
sealed class A {}

class B: A {}            // Error, cannot derive from a sealed class
```
Klasse `B` ist falsch, weil er versucht, für die Ableitung der `sealed` Klasse `A`.

#### <a name="interface-implementations"></a>Schnittstellenimplementierungen

Ein *Class_base* möglicherweise eine Spezifikation enthalten eine Liste der Schnittstellentypen, in dem Fall die Klasse wird als die angegebene Schnittstelle-Typen direkt zu implementieren. Schnittstellenimplementierungen finden Sie weiter unten in [Schnittstellenimplementierungen](interfaces.md#interface-implementations).

### <a name="type-parameter-constraints"></a>Einschränkungen für Typparameter

Generische Typ- und Methodendeklarationen können optional die Einschränkungen für Typparameter angeben, indem einschließlich *Type_parameter_constraints_clause*s.

```antlr
type_parameter_constraints_clause
    : 'where' type_parameter ':' type_parameter_constraints
    ;

type_parameter_constraints
    : primary_constraint
    | secondary_constraints
    | constructor_constraint
    | primary_constraint ',' secondary_constraints
    | primary_constraint ',' constructor_constraint
    | secondary_constraints ',' constructor_constraint
    | primary_constraint ',' secondary_constraints ',' constructor_constraint
    ;

primary_constraint
    : class_type
    | 'class'
    | 'struct'
    ;

secondary_constraints
    : interface_type
    | type_parameter
    | secondary_constraints ',' interface_type
    | secondary_constraints ',' type_parameter
    ;

constructor_constraint
    : 'new' '(' ')'
    ;
```

Jede *Type_parameter_constraints_clause* besteht aus dem Token `where`, gefolgt vom Namen eines Typparameters, gefolgt von einem Doppelpunkt und die Liste der Einschränkungen für den Typparameter. Es kann höchstens eine `where` -Klausel für jeden Typparameter, und die `where` Klauseln können in beliebiger Reihenfolge aufgeführt werden. Wie die `get` und `set` Token in einem Eigenschaftenaccessor der `where` Token ist kein Schlüsselwort.

Die Liste der Einschränkungen, die im angegebenen ein `where` -Klausel kann eine der folgenden Komponenten, in der angegebenen Reihenfolge enthalten: eine einzelne primary-Einschränkung, einer oder mehreren sekundären Einschränkungen und die Konstruktoreinschränkung `new()`.

Eine primary-Einschränkung kann ein Klassentyp sein oder die ***verweisen auf eine Einschränkung*** `class` oder ***Wert typeinschränkung*** `struct`. Eine sekundäre Einschränkung möglich einen *Type_parameter* oder *Interface_type*.

Der verweistypeinschränkung gibt an, dass ein Typargument verwendet, für den Typparameter ein Verweistyp sein muss. Alle Klassentypen, Schnittstellentypen, Delegattypen, Arraytypen und Parameter vom Typ bekannt, dass ein Verweistyp (wie unten definiert) werden diese Einschränkung zu erfüllen.

Der werttypeinschränkung gibt an, dass ein Typargument verwendet, für den Parameter ein NULL-Wert sein muss. Alle NULL-Strukturtypen, Enumerationstypen und Parameter vom Typ der werttypeinschränkung müssen diese Einschränkung zu erfüllen. Beachten Sie, dass, obwohl Sie als ein Werttyp, der einen nullable-Typ klassifiziert ([auf NULL festlegbare Typen](types.md#nullable-types)) erfüllt nicht der werttypeinschränkung. Ein Typparameter mit der werttypeinschränkung keine auch die *Constructor_constraint*.

Zeigertypen dürfen keine Typargumente werden und gelten nicht als entweder die Referenz Typ oder Wert typeinschränkungen.

Wenn eine Einschränkung eines Klassentyps, einen Schnittstellentyp aufweisen oder ein Typparameter ist, gibt dieses Typs ein minimaler "Basistyp", die jedes Typargument, das für den Typparameter verwendete unterstützen muss. Wenn es sich bei einem konstruierten Typ oder eine generische Methode verwendet wird, wird das Typargument für die Einschränkungen für den Typparameter zum Zeitpunkt der Kompilierung überprüft. Das Typargument muss die in beschriebenen Bedingungen erfüllen, [Einschränkungen erfüllen](types.md#satisfying-constraints).

Ein *Class_type* Einschränkung muss die folgenden Regeln entsprechen:

*  Der Typ muss ein Klassentyp sein.
*  Der Typ muss nicht `sealed`.
*  Der Typ muss nicht einer der folgenden Typen sein: `System.Array`, `System.Delegate`, `System.Enum`, oder `System.ValueType`.
*  Der Typ muss nicht `object`. Da alle Typen abgeleitet `object`, eine solche Einschränkung hätte keine Auswirkung, wenn sie zulässig wären.
*  Maximal eine Einschränkung für einen bestimmten Typ-Parameter kann ein Klassentyp sein.

Ein Typ, der als ein *Interface_type* Einschränkung muss die folgenden Regeln entsprechen:

*  Der Typ muss ein Schnittstellentyp sein.
*  Ein Typ nicht angegeben werden mehr als einmal einer angegebenen `where` Klausel.

In beiden Fällen die Einschränkung kann einer der Typparameter des zugeordneten Typs oder der Deklaration der Methode als Teil eines konstruierten Typs umfassen, und kann den deklarierenden Typ umfassen.

Jede Klasse oder Schnittstelle-Typ angegeben wird, eine Einschränkung für einen Parameter muss mindestens dieselben Zugriffstypen unterstützt werden ([Barrierefreiheit Einschränkungen](basic-concepts.md#accessibility-constraints)) als generischen Typ oder Methode, die deklariert wird.

Ein Typ, der als ein *Type_parameter* Einschränkung muss die folgenden Regeln entsprechen:

*  Der Typ muss es sich um ein Typparameter sein.
*  Ein Typ nicht angegeben werden mehr als einmal einer angegebenen `where` Klausel.

Darüber hinaus muss gibt es keine Zyklen im Abhängigkeitsdiagramm an Typparametern, in dem Abhängigkeit eine transitive Beziehung durch definiert ist:

*  Wenn ein Typparameter `T` dient als Einschränkung für den Typparameter `S` dann `S` ***hängt*** `T`.
*  Wenn ein Typparameter `S` hängt von einem Typparameter `T` und `T` hängt von einem Typparameter `U` dann `S` ***hängt von*** `U`.

Wenn diese Beziehung ist, ist es ein Fehler während der Kompilierung für einen Typparameter auf sich selbst (direkt oder indirekt) abhängig sein.

Alle Einschränkungen müssen zwischen abhängigen Typparameter konsistent sein. Wenn der Typparameter `S` hängt von den Typparameter `T` dann:

*  `T` darf nicht der werttypeinschränkung sein. Andernfalls `T` ist so effektiv versiegelt `S` möchten, müssen Sie den gleichen Typ wie werden `T`, beseitigt die Notwendigkeit zwei Typparametern.
*  Wenn `S` hat der werttypeinschränkung `T` darf keine *Class_type* Einschränkung.
*  Wenn `S` verfügt über eine *Class_type* Einschränkung `A` und `T` verfügt über eine *Class_type* Einschränkung `B` darf eine identitätskonvertierung werden oder implizit Verweisen auf die Konvertierung von `A` zu `B` oder eine implizite verweiskonvertierung von `B` zu `A`.
*  Wenn `S` hängt auch von Typparameter `U` und `U` verfügt über eine *Class_type* Einschränkung `A` und `T` verfügt über eine *Class_type* Einschränkung `B` dann vorhanden, eine identitätskonvertierung oder implizite verweiskonvertierung von sein müssen `A` zu `B` oder eine implizite verweiskonvertierung von `B` zu `A`.

Es gilt für `S` der werttypeinschränkung haben und `T` der verweistypeinschränkung haben. Effektiv begrenzt `T` zu den Typen `System.Object`, `System.ValueType`, `System.Enum`, und einen beliebigen anderen Schnittstellentyp.

Wenn die `where` -Klausel für einen Typparameter enthält eine Konstruktoreinschränkung (die weist das Format `new()`), es ist möglich, verwenden Sie die `new` Operator, um Instanzen des Typs zu erstellen ([Objektausdrücke bei der Erstellung](expressions.md#object-creation-expressions)). Jeder Typ verwendete Argument für ein Typparameter eine Konstruktoreinschränkung für einen öffentlichen parameterlosen Konstruktor aufweisen muss (von diesem Konstruktor implizit für jeden Werttyp vorhanden) oder ein Typparameter mit der werttypeinschränkung oder Konstruktoreinschränkung (Siehe werden [Geben Sie die Einschränkungen für Typparameter](classes.md#type-parameter-constraints) Einzelheiten).

Es folgen Beispiele für Einschränkungen:
```csharp
interface IPrintable
{
    void Print();
}

interface IComparable<T>
{
    int CompareTo(T value);
}

interface IKeyProvider<T>
{
    T GetKey();
}

class Printer<T> where T: IPrintable {...}

class SortedList<T> where T: IComparable<T> {...}

class Dictionary<K,V>
    where K: IComparable<K>
    where V: IPrintable, IKeyProvider<K>, new()
{
    ...
}
```

Im folgende Beispiel ist fehlerhaft, da es eine Zirkularität bewirkt, in dem Abhängigkeitsdiagramm der Typparameter dass:
```csharp
class Circular<S,T>
    where S: T
    where T: S                // Error, circularity in dependency graph
{
    ...
}
```

Die folgenden Beispiele veranschaulichen weitere ungültige Situationen:
```csharp
class Sealed<S,T>
    where S: T
    where T: struct        // Error, T is sealed
{
    ...
}

class A {...}

class B {...}

class Incompat<S,T>
    where S: A, T
    where T: B                // Error, incompatible class-type constraints
{
    ...
}

class StructWithClass<S,T,U>
    where S: struct, T
    where T: U
    where U: A                // Error, A incompatible with struct
{
    ...
}
```

Die ***effektive Basisklasse*** eines Typparameters `T` ist wie folgt definiert:

*  Wenn `T` besitzt keine primary-Einschränkungen oder Einschränkungen für Typparameter, effektive Basisklasse `object`.
*  Wenn `T` hat der werttypeinschränkung, ist der effektive Basisklasse `System.ValueType`.
*  Wenn `T` verfügt über eine *Class_type* Einschränkung `C` aber kein *Type_parameter* Einschränkungen, die effektive Basisklasse ist `C`.
*  Wenn `T` hat keine *Class_type* Einschränkung weist jedoch mindestens eine *Type_parameter* Einschränkungen, die effektive Basisklasse ist der am stärksten umfasste ([Konvertierung aufgehoben Operatoren](conversions.md#lifted-conversion-operators)) in der Menge der effektiven Basisklassen der *Type_parameter* Einschränkungen. Die Konsistenzregeln stellen Sie sicher, dass diese am stärksten umfasste ein Typ vorhanden ist.
*  Wenn `T` weist sowohl ein *Class_type* Einschränkung und eine oder mehrere *Type_parameter* Einschränkungen, die effektive Basisklasse ist der am stärksten umfasste ([Konvertierung aufgehoben Operatoren](conversions.md#lifted-conversion-operators)) im Satz bestehend aus dem *Class_type* Einschränkung `T` und die effektive Basisklassen seine *Type_parameter* Einschränkungen. Die Konsistenzregeln stellen Sie sicher, dass diese am stärksten umfasste ein Typ vorhanden ist.
*  Wenn `T` der verweistypeinschränkung hat, aber keine *Class_type* Einschränkungen, die effektive Basisklasse ist `object`.

Für diese Regeln, wenn T eine Einschränkung wurde `V` d. h. eine *Value_type*, verwenden Sie stattdessen den spezifischsten Basistyp `V` , eine *Class_type*. Dies kann in einem explizit angegebenen Einschränkung nie auftreten, aber kann auftreten, wenn die Einschränkungen einer generischen Methode implizit durch einen überschreibenden Deklaration der Methode oder eine explizite Implementierung einer Schnittstellenmethode geerbt werden.

Diese Regeln stellen sicher, dass die effektive Basisklasse immer ist eine *Class_type*.

Die ***effektive Satz*** eines Typparameters `T` ist wie folgt definiert:

*  Wenn `T` hat keine *Secondary_constraints*, dessen effektive Schnittstelle-Satz ist leer.
*  Wenn `T` hat *Interface_type* Einschränkungen, aber keine *Type_parameter* Einschränkungen, die effektive Satz ist die Sammlung von *Interface_type* Einschränkungen.
*  Wenn `T` hat keine *Interface_type* weist Einschränkungen jedoch *Type_parameter* Einschränkungen, die effektive Satz ist die Vereinigung der effektive Schnittstelle Sätze von dessen *Type_ Parameter* Einschränkungen.
*  Wenn `T` weist sowohl *Interface_type* Einschränkungen und *Type_parameter* Einschränkungen, die effektive Satz ist die Vereinigung der einen Satz von *Interface_type* Einschränkungen und die effektive Schnittstelle Sätze von dessen *Type_parameter* Einschränkungen.

Ein Typparameter ist ***bekannt, dass ein Verweistyp sein*** ist der verweistypeinschränkung hat oder ihre effektiven Basisklasse nicht `object` oder `System.ValueType`.

Werte einen eingeschränkten Typ Parametertyp können verwendet werden, auf die Instanz-Elemente, die durch die Einschränkungen impliziert. Im Beispiel
```csharp
interface IPrintable
{
    void Print();
}

class Printer<T> where T: IPrintable
{
    void PrintOne(T x) {
        x.Print();
    }
}
```
die Methoden der `IPrintable` kann aufgerufen werden, direkt auf `x` da `T` immer implementieren `IPrintable`.

### <a name="class-body"></a>Text der Klasse

Die *Class_body* einer Klasse definiert die Elemente dieser Klasse.

```antlr
class_body
    : '{' class_member_declaration* '}'
    ;
```

## <a name="partial-types"></a>Partielle Typen

Eine Typdeklaration aufgeteilt werden kann, über mehrere ***partielle Typdeklarationen***. Die Deklaration des Typs wird aus seiner Teile erstellt, wird entsprechend den Regeln in diesem Abschnitt, wonach es als eine einzelne Deklaration während der Rest der Verarbeitung während der Kompilierung und Laufzeit des Programms behandelt wird.

Ein *Class_declaration*, *Struct_declaration* oder *Interface_declaration* eine Deklaration der partiellen Typ darstellt, sofern er enthält eine `partial` Modifizierer. `partial` ist kein Schlüsselwort und dient nur als ein Modifizierer, wenn es angezeigt, unmittelbar vor der eines der Schlüsselwörter wird `class`, `struct` oder `interface` in einer Typdeklaration oder vor dem Typ `void` in einer Methodendeklaration. In anderen Kontexten können sie als ein normaler Bezeichner verwendet werden.

Jeder Teil einer Deklaration der partiellen Typs muss enthalten eine `partial` Modifizierer. Sie müssen den gleichen Namen aufweisen und in den gleichen Namespace oder die Typdeklaration als die anderen Teile deklariert werden. Die `partial` Modifizierer gibt an, dass zusätzliche Teile der Typdeklaration möglicherweise an anderer Stelle vorhanden, aber das Vorhandensein von zusätzlichen Komponenten nicht erforderlich ist; es gilt für einen Typ mit einer einzigen Deklaration sollen die `partial` Modifizierer.

Alle Teile eines partiellen Typs müssen zusammen kompiliert werden, sodass Teile zum Zeitpunkt der Kompilierung in eine einzelne Typdeklaration zusammengeführt werden können. Partielle Typen lassen nicht speziell bereits kompilierte Typen erweitert werden soll.

Geschachtelte Typen können aus mehreren Teilen deklariert werden, mithilfe der `partial` Modifizierer. In der Regel wird der enthaltende Typ mit deklariert `partial` gut, und jeder Teil des geschachtelten Typs in einem anderen Teil des enthaltenden Typs deklariert ist.

Die `partial` Modifizierer ist für Delegat- oder Enum-Deklarationen nicht zulässig.

### <a name="attributes"></a>Attribute

Die Attribute eines partiellen Typs werden durch die Kombination von können Sie in einer nicht vorgegebenen Reihenfolge, die Attribute der einzelnen Teile bestimmt. Wenn ein Attribut auf mehrere Teile platziert wird, entspricht das Attribut mehrmals für den Typ angegeben. Beispielsweise die beiden Teile:

```csharp
[Attr1, Attr2("hello")]
partial class A {}

[Attr3, Attr2("goodbye")]
partial class A {}
```
gibt z. B. eine Deklaration entspricht:
```csharp
[Attr1, Attr2("hello"), Attr3, Attr2("goodbye")]
class A {}
```

Attribute für Typparameter auf ähnliche Weise zu kombinieren.

### <a name="modifiers"></a>Modifizierer

Wenn eine Deklaration einer partiellen Typ enthält eine Spezifikation für die Barrierefreiheit (die `public`, `protected`, `internal`, und `private` Modifizierer) sie müssen zustimmen, andere Komponenten, die eine Spezifikation für die Barrierefreiheit enthalten. Wenn kein Teil eines partiellen Typs eine Spezifikation für die Barrierefreiheit enthält, wird der Typ der entsprechenden Standardzugriff angegeben ([deklariert Barrierefreiheit](basic-concepts.md#declared-accessibility)).

Wenn ein oder mehr partielle Deklarationen eines geschachtelten Typs enthalten eine `new` Modifizierer keine Warnung wird ausgegeben, wenn der geschachtelte Typ einen geerbten Member Blendet ([ausblenden, die durch Vererbung von](basic-concepts.md#hiding-through-inheritance)).

Wenn eine oder mehrere partielle Deklarationen, die von einer Klasse umfassen eine `abstract` Modifizierer die Klasse gilt als abstrakt ([abstrakte Klassen](classes.md#abstract-classes)). Andernfalls gilt die Klasse abstrakt.

Wenn eine oder mehrere partielle Deklarationen, die von einer Klasse enthalten eine `sealed` Modifizierer die Klasse wird als versiegelt betrachtet ([versiegelte Klassen](classes.md#sealed-classes)). Andernfalls gilt die Klasse nicht versiegelten.

Beachten Sie, dass eine Klasse nicht gleichzeitig abstrakt und versiegelt sein kann.

Wenn die `unsafe` Modifizierer ist für eine partielle Typdeklaration verwendet, nur diesen bestimmte Teil als unsicherer Kontext angesehen wird ([nicht sicheren Kontexten](unsafe-code.md#unsafe-contexts)).

### <a name="type-parameters-and-constraints"></a>Typparameter und Einschränkungen

Wenn in mehreren Teilen ein generisches Typs deklariert wird, muss jeder Teil die Typparameter angeben. Jeder Teil muss die gleiche Anzahl von Typparametern und den gleichen Namen für jeden Typparameter in der Reihenfolge haben.

Wenn eine partielle generischen Typdeklaration enthält Einschränkungen (`where` Klauseln), die Einschränkungen müssen zustimmen, andere Komponenten, die Einschränkungen enthalten. Insbesondere jeder Teil, der Einschränkungen enthält, müssen Einschränkungen für den gleichen Satz an Typparametern und für jeden Typparameter der Sätze von primären, sekundären und konstruktoreinschränkungen müssen äquivalent sein. Zwei Sätze von Integritätsregeln sind äquivalent, wenn sie dieselben Elemente enthalten. Wenn kein Teil eines partiellen Typs für die generische Einschränkungen für Typparameter angegeben wird, ist kein Hindernis für der Typ, Parameter berücksichtigt werden.

Im Beispiel
```csharp
partial class Dictionary<K,V>
    where K: IComparable<K>
    where V: IKeyProvider<K>, IPersistable
{
    ...
}

partial class Dictionary<K,V>
    where V: IPersistable, IKeyProvider<K>
    where K: IComparable<K>
{
    ...
}

partial class Dictionary<K,V>
{
    ...
}
```
ist richtig, da die Teile, die Einschränkungen (die ersten beiden) effektiv enthalten die gleichen Satz von primären, sekundären und konstruktoreinschränkungen für den gleichen Satz an Typparametern angeben.

### <a name="base-class"></a>Basisklasse

Wenn eine Deklaration der partiellen Klasse eine basisklassenspezifikation enthält müssen sie alle anderen Teile zustimmen, die eine basisklassenspezifikation enthalten. Wenn kein Teil einer partiellen Klasse eine basisklassenspezifikation enthält, wird die Basisklasse `System.Object` ([Basisklassen](classes.md#base-classes)).

### <a name="base-interfaces"></a>Basisschnittstellen

Der Satz von Schnittstellen für einen Typ, der aus mehreren Teilen deklariert ist die Union der Basisschnittstellen für jede Komponente angegeben. Eine bestimmte Basis-Schnittstelle kann nur einmal für jede Komponente namens werden, aber es ist zulässig, für mehrere Teile, benennen Sie die gleichen basisschnitstelle. Es muss nur eine Implementierung der Elemente einer angegebenen Basis-Schnittstelle vorhanden sein.

Im Beispiel
```csharp
partial class C: IA, IB {...}

partial class C: IC {...}

partial class C: IA, IB {...}
```
der Satz von Schnittstellen für die Klasse `C` ist `IA`, `IB`, und `IC`.

In der Regel stellt jeder Teil eine Implementierung der Schnittstellen, die für diesen Teil deklariert; Dies ist jedoch keine Voraussetzung. Ein Teil kann die Implementierung einer Schnittstelle deklariert, die auf einem anderen Teil bereitstellen:
```csharp
partial class X
{
    int IComparable.CompareTo(object o) {...}
}

partial class X: IComparable
{
    ...
}
```

### <a name="members"></a>Member

Mit Ausnahme von partiellen Methoden ([partielle Methoden](classes.md#partial-methods)), die Menge der Elemente eines Typs, die in mehreren Teilen deklariert ist, einfach die Vereinigung der Menge der Elemente in jedem Teil deklariert. Die Texte der alle Teile der Typdeklaration Freigabe im selben Deklarationsabschnitt ([Deklarationen](basic-concepts.md#declarations)), und der Bereich für jedes Element ([Bereiche](basic-concepts.md#scopes)) erstreckt sich auch auf die Texte der alle Teile. Die Zugriffsdomäne eines Members enthält immer alle Teile des einschließenden Typs; eine `private` in einem Bereich deklarierten Member aus einem anderen Teil frei zugänglich ist. Es ist ein Fehler während der Kompilierung, um dasselbe Element in mehr als ein Teil des Typs zu deklarieren, es sei denn, dieser Member ein Typ mit ist der `partial` Modifizierer.

```csharp
partial class A
{
    int x;                     // Error, cannot declare x more than once

    partial class Inner        // Ok, Inner is a partial type
    {
        int y;
    }
}

partial class A
{
    int x;                     // Error, cannot declare x more than once

    partial class Inner        // Ok, Inner is a partial type
    {
        int z;
    }
}
```

Die Reihenfolge der Elemente in einem Typ wird nur selten relevant, für die C#-Code, aber kann erheblich sein, wenn für die Kommunikation mit anderen Sprachen und Umgebungen. In diesen Fällen ist es nicht definiert, die Reihenfolge der Member innerhalb eines Typs, die in mehreren Teilen deklariert.

### <a name="partial-methods"></a>Partielle Methoden

Partielle Methoden in einem Teil einer Typdeklaration definiert, und in einem anderen implementiert werden können. Die Implementierung ist optional. Wenn kein Teil die partielle Methode implementiert, werden die Deklaration der partiellen Methode und alle Aufrufe aus die Typdeklaration, die durch die Kombination aus den Teilen entfernt.

Partielle Methoden können keine Zugriffsmodifizierer definieren, jedoch handelt es sich implizit `private`. Ihren Rückgabetyp muss `void`, und ihre Parameter darf keine der `out` Modifizierer. Der Bezeichner `partial` wird als eine spezielle Schlüsselwort in einer Methodendeklaration erkannt, nur, wenn Sie anscheinend direkt vor der `void` geben; andernfalls kann es als ein normaler Bezeichner verwendet werden. Eine partielle Methode kann keine Schnittstellenmethoden explizit implementieren.

Es gibt zwei Arten von Deklarationen der partiellen Methode ein: Wenn der Hauptteil der Deklaration der Methode ein Semikolon ist, die Deklaration wird als eine ***definierende Deklaration der partiellen Methode***. Wenn Text, als angegeben wird eine *Block*, die Deklaration wird als ein ***implementierende Deklaration der partiellen Methode***. Für die Teile einer Typdeklaration es kann nur eine definierende Deklaration der partiellen Methode mit einem angegebenen Signatur-, und es kann nur eine implementierende Deklaration der partiellen Methode mit einer bestimmten Signatur. Wenn eine implementierende Deklaration der partiellen Methode angegeben, eine entsprechende definierende Deklaration der partiellen Methode vorhanden sein muss und müssen die Deklarationen als übereinstimmen, die in der folgenden angegeben werden:

* Die Deklarationen müssen die gleichen Modifizierer (obwohl nicht unbedingt in der gleichen Reihenfolge), Methodennamen, der Anzahl von Typparametern und der Anzahl von Parametern.
* Entsprechende Parameter in den Deklarationen müssen die gleichen Modifizierer (obwohl nicht unbedingt in der gleichen Reihenfolge) und den gleichen Typ (modulo Unterschiede bei Typparameternamen).
* Entsprechende Typparameter in den Deklarationen müssen dieselben Einschränkungen (modulo Unterschiede bei Typparameternamen).

Eine implementierende Deklaration der partiellen Methode kann in den gleichen Teil als entsprechende definierende Deklaration der partiellen Methode erscheinen.

Nur eine definierende partielle Methode, die an der überladungsauflösung beteiligt ist. Also, und zwar unabhängig davon, ob eine implementierende Deklaration angegeben ist, können auf Aufrufe der partiellen Methode Aufrufausdrücke beheben. Da eine partielle Methode immer zurückgibt `void`, solche Aufrufausdrücke werden sich immer auf die Tabellenausdruck-Anweisungen. Darüber hinaus, da eine partielle Methode implizit wird `private`, solche Anweisungen treten immer innerhalb einer der Bestandteile der Typdeklaration, die in dem die partielle Methode deklariert ist.

Wenn kein Teil einer Deklaration der partiellen Typ eine implementierende Deklaration für eine bestimmte partielle Methode enthält, wird Ausdrucksanweisung Aufrufen aus der kombinierten Typdeklaration einfach entfernt. Daher ist der Aufrufausdruck, einschließlich der enthaltenen Ausdrücke, hat keine Auswirkungen zur Laufzeit. Die partielle Methode selbst wird wird ebenfalls entfernt und nicht Mitglied der kombinierten Typdeklaration.

Wenn die implementierende Deklaration für eine bestimmte partielle Methode vorhanden sind, werden die Aufrufe der partiellen Methoden beibehalten. Die partielle Methode führt zu einer Methodendeklaration, die die implementierende Deklaration der partiellen Methode mit Ausnahme der folgenden ähnelt:

* Die `partial` Modifizierer nicht enthalten ist.
* Die Attribute in der resultierenden Methodendeklaration sind die kombinierten Attribute definieren und die implementierende Deklaration der partiellen Methode Erweiterungsvalidierungsmethoden ohne spezifische Reihenfolge. Duplikate werden nicht entfernt.
* Die Attribute für die Parameter, der sich ergebenden Methodendeklaration sind der Kombination der Eigenschaften der entsprechenden Parameter definieren und die implementierende Deklaration der partiellen Methode in einer nicht vorgegebenen Reihenfolge. Duplikate werden nicht entfernt.

Wenn eine definierende Deklaration jedoch keine implementierende Deklaration für eine partielle Methode M gegeben wird, gelten die folgenden Einschränkungen:

* Es ist ein Fehler während der Kompilierung zum Erstellen eines Delegaten-Methode ([delegieren erstellen Ausdrücke](expressions.md#delegate-creation-expressions)).
* Es ist ein Fehler während der Kompilierung zum Verweisen auf `M` innerhalb einer anonymen Funktion, die in einen Typ für die Ausdrucksbaumstruktur konvertiert wird ([Auswertung der anonymen Funktion-Konvertierungen in ausdrucksbaumstrukturtypen](conversions.md#evaluation-of-anonymous-function-conversions-to-expression-tree-types)).
* Ausdrücke, die auftreten, die als Teil eines Aufrufs der `M` wirken sich nicht auf den Zustand für definitive Zuweisungen ([definitive Zuweisung](variables.md#definite-assignment)), dies kann potenziell zu Kompilierungsfehlern führen.
* `M` der Einstiegspunkt für eine Anwendung nicht möglich ([Anwendungsstart](basic-concepts.md#application-startup)).

Partielle Methoden sind hilfreich für das Zulassen von einem Teil einer Typdeklaration, um das Verhalten von einem anderen Teil, z. B. eine anzupassen, die von einem Tool generiert wird. Beachten Sie die folgende Deklaration der partiellen Klasse ein:
```csharp
partial class Customer
{
    string name;

    public string Name {
        get { return name; }
        set {
            OnNameChanging(value);
            name = value;
            OnNameChanged();
        }

    }

    partial void OnNameChanging(string newName);

    partial void OnNameChanged();
}
```

Wenn diese Klasse, ohne auf andere Teile kompiliert wird, die definierende Deklarationen der partiellen Methode und ihre Aufrufe werden entfernt, und die resultierende kombinierten Klassendeklaration werden äquivalent zu folgendem:
```csharp
class Customer
{
    string name;

    public string Name {
        get { return name; }
        set { name = value; }
    }
}
```

Nehmen Sie an, dass ein anderer Teil, jedoch verfügt die implementierende Deklarationen der partiellen Methoden bereitstellt:
```csharp
partial class Customer
{
    partial void OnNameChanging(string newName)
    {
        Console.WriteLine("Changing " + name + " to " + newName);
    }

    partial void OnNameChanged()
    {
        Console.WriteLine("Changed to " + name);
    }
}
```

Anschließend werden die resultierenden kombinierten Klassendeklaration äquivalent zu folgendem:
```csharp
class Customer
{
    string name;

    public string Name {
        get { return name; }
        set {
            OnNameChanging(value);
            name = value;
            OnNameChanged();
        }

    }

    void OnNameChanging(string newName)
    {
        Console.WriteLine("Changing " + name + " to " + newName);
    }

    void OnNameChanged()
    {
        Console.WriteLine("Changed to " + name);
    }
}
```

### <a name="name-binding"></a>-Bindungen

Obwohl jeder Teil eine erweiterbare Typ in den gleichen Namespace deklariert werden muss, werden die Teile in der Regel in den anderen Namespace-Deklarationen geschrieben. Daher verschiedene `using` Directives ([Using-Direktiven](namespaces.md#using-directives)) kann für die einzelnen Teile vorhanden sein. Beim Interpretieren von einfachen Namen ([Typrückschluss](expressions.md#type-inference)) in nur ein Teil der `using` gelten als Anweisungen von der dieses Teils einschließenden Namespace zurückzusenden. Dies kann dazu führen, dass den gleichen Bezeichner müssen verschiedene Bedeutungen in verschiedenen Teilen:
```csharp
namespace N
{
    using List = System.Collections.ArrayList;

    partial class A
    {
        List x;                // x has type System.Collections.ArrayList
    }
}

namespace N
{
    using List = Widgets.LinkedList;

    partial class A
    {
        List y;                // y has type Widgets.LinkedList
    }
}
```

## <a name="class-members"></a>Klassenmember

Die Member einer Klasse bestehen aus der Elemente eingeführt, durch die *Class_member_declaration*s und die Elemente, die von der direkten Basisklasse geerbt.

```antlr
class_member_declaration
    : constant_declaration
    | field_declaration
    | method_declaration
    | property_declaration
    | event_declaration
    | indexer_declaration
    | operator_declaration
    | constructor_declaration
    | destructor_declaration
    | static_constructor_declaration
    | type_declaration
    ;
```

Die Member eines Klassentyps sind in folgenden Kategorien unterteilt:

*  Konstanten, die Konstanten Werte, die der Klasse zugeordneten darstellen ([Konstanten](classes.md#constants)).
*  Felder, die die Variablen der Klasse ([Felder](classes.md#fields)).
*  Methoden, die Implementierung der Berechnungen und Aktionen, die von der Klasse ausgeführt werden können ([Methoden](classes.md#methods)).
*  Eigenschaften, die definieren, mit dem Namen Merkmale und der Aktionen in Verbindung mit dem Lesen und Schreiben diese Eigenschaften ([Eigenschaften](classes.md#properties)).
*  Ereignisse, die Benachrichtigungen zu definieren, die von der Klasse generiert werden kann ([Ereignisse](classes.md#events)).
*  Indexer, die zulassen von Instanzen der Klasse, die auf die gleiche Weise indiziert werden als Arrays (syntaktisch) ([Indexer](classes.md#indexers)).
*  Operatoren, die die Operatoren für Ausdrücke zu definieren, die auf Instanzen der Klasse angewendet werden können ([Operatoren](classes.md#operators)).
*  Instanzkonstruktoren, die die erforderlichen Aktionen zum Initialisieren von Instanzen der Klasse zu implementieren ([Instanzkonstruktoren](classes.md#instance-constructors))
*  Destruktoren, die die Aktionen, die ausgeführt werden, bevor Instanzen der Klasse dauerhaft verworfen werden implementieren ([Destruktoren](classes.md#destructors)).
*  Statische Konstruktoren, die die erforderlichen Aktionen zum Initialisieren der Klasse selbst zu implementieren ([statische Konstruktoren](classes.md#static-constructors)).
*  Typen, die die Typen darstellen, die lokal auf die Klasse sind ([geschachtelte Typen](classes.md#nested-types)).

Elemente, die ausführbaren Code enthalten können, werden zusammen als bezeichnet die *Funktionsmember* des Klassentyps. Die Funktionsmember eines Klassentyps sind die Methoden, Eigenschaften, Ereignisse, Indexer, Operatoren, Instanzkonstruktoren, Destruktoren und statischen Konstruktoren des Klassentyps.

Ein *Class_declaration* erstellt einen neuen Deklarationsabschnitt ([Deklarationen](basic-concepts.md#declarations)), und die *Class_member_declaration*s sofort enthalten die *Klasse _declaration* neue Elemente in diesen Deklarationsabschnitt einführen. Die folgenden Regeln gelten für *Class_member_declaration*s:

*  Instanzkonstruktoren, Destruktoren und statische Konstruktoren müssen den gleichen Namen wie die Sie unmittelbar umschließende Klasse aufweisen. Alle anderen Elemente müssen Namen haben, die von den Namen der unmittelbar einschließenden Klasse abweichen.
*  Der Name der eine Konstante, ein Feld, Eigenschaft, Ereignis oder Typ muss die Namen aller anderen Elemente, die in der gleichen Klasse deklariert unterscheiden.
*  Der Name einer Methode muss die Namen aller anderen nicht--Methoden, in der gleichen Klasse deklariert unterscheiden. Außerdem wird die Signatur ([Signaturen und überladen](basic-concepts.md#signatures-and-overloading)) von eine Methode muss die Signaturen aller anderen in derselben Klasse deklarierten Methoden unterscheiden, und zwei Methoden, die in der gleichen Klasse deklariert dürfen keine Signaturen, die ausschließlich unterscheiden. durch `ref` und `out`.
*  Die Signatur eines Instanzkonstruktors muss von den Signaturen aller anderen in derselben Klasse deklarierten Instanzkonstruktoren unterscheiden, und zwei Konstruktoren, die in der gleichen Klasse deklariert dürfen keine Signaturen, die ausschließlich vom unterscheiden `ref` und `out`.
*  Die Signatur eines Indexers muss die Signaturen aller anderen in derselben Klasse deklarierten Indexer unterscheiden.
*  Die Signatur eines Operators muss die Signaturen aller anderen Operatoren, die in der gleichen Klasse deklariert unterscheiden.

Die geerbten Member eines Klassentyps ([Vererbung](classes.md#inheritance)) sind nicht Teil des Speicherplatzes Deklaration einer Klasse. Daher kann eine abgeleitete Klasse ein Element mit demselben Namen bzw. derselben Signatur als einen geerbten Member zu deklarieren (d. Blendet die geerbten Member in Kraft).

### <a name="the-instance-type"></a>Der Instanztyp

Jede Deklaration der Klasse verfügt über einen zugeordneten gebundenen Typ ([gebunden und Typen von nicht gebundenen](types.md#bound-and-unbound-types)), wird die ***Instanztyp***. Für die Deklaration einer generischen Klasse, wird der Instanztyp durch das Erstellen eines konstruierten Typs gebildet ([Typen konstruiert](types.md#constructed-types)) aus der Deklaration des Typs mit den angegebenen Typ Argumente werden den entsprechenden Typparameter. Da der Instanztyp die Typparameter verwendet, können sie nur, in dem sich die Typparameter im Gültigkeitsbereich befinden verwendet werden. d. h. in der Klassendeklaration. Der Instanztyp ist der Typ der `this` für Code in der Klassendeklaration geschrieben wurde. Für nicht generische Klassen ist der Instanztyp einfach die deklarierte-Klasse. Das folgende Beispiel zeigt mehrere Klassendeklarationen zusammen mit ihren Instanztypen aus: 
```csharp
class A<T>                           // instance type: A<T>
{
    class B {}                       // instance type: A<T>.B
    class C<U> {}                    // instance type: A<T>.C<U>
}

class D {}                           // instance type: D
```

### <a name="members-of-constructed-types"></a>Mitglieder der konstruierte Typen

Die nicht geerbte Member eines konstruierten Typs erhalten Sie durch Ersetzen der für die einzelnen *Type_parameter* in der Element-Deklaration wird das entsprechende *Type_argument* des konstruierten Typs. Die Ersetzung Prozess basiert auf die semantische Bedeutung der Deklarationen von Typen und ist nicht einfach Text ersetzen.

Wenn z. B. die generische Klasse-Deklaration
```csharp
class Gen<T,U>
{
    public T[,] a;
    public void G(int i, T t, Gen<U,T> gt) {...}
    public U Prop { get {...} set {...} }
    public int H(double d) {...}
}
```
Der konstruierte Typ `Gen<int[],IComparable<string>>` hat die folgenden Elemente:
```csharp
public int[,][] a;
public void G(int i, int[] t, Gen<IComparable<string>,int[]> gt) {...}
public IComparable<string> Prop { get {...} set {...} }
public int H(double d) {...}
```

Der Typ des Members `a` in der generischen Klassendeklaration `Gen` ist "zweidimensionales Array von `T`", sodass der Typ des Members `a` im oben erstellten Typ ist "zweidimensionales Array von eindimensionales Array von `int`", oder `int[,][]`.

Innerhalb der Funktion Instanzmember, der Typ des `this` ist der Instanztyp ([den Instanztyp](classes.md#the-instance-type)) der Deklaration enthält.

Alle Member einer generischen Klasse können Typparameter von einer einschließenden Klasse, entweder direkt oder als Teil eines konstruierten Typs verwenden. Beim Schließen von einer bestimmtes konstruierten Typ ([offene und geschlossene Typen](types.md#open-and-closed-types)) wird verwendet, zur Laufzeit, wird jede Verwendung von einem Typparameter durch die tatsächliche, für den konstruierten Typ angegebenes Typargument ersetzt. Zum Beispiel:
```csharp
class C<V>
{
    public V f1;
    public C<V> f2 = null;

    public C(V x) {
        this.f1 = x;
        this.f2 = this;
    }
}

class Application
{
    static void Main() {
        C<int> x1 = new C<int>(1);
        Console.WriteLine(x1.f1);        // Prints 1

        C<double> x2 = new C<double>(3.1415);
        Console.WriteLine(x2.f1);        // Prints 3.1415
    }
}
```

### <a name="inheritance"></a>Vererbung

Eine Klasse ***erbt*** die Member dieses Typs direkte Basisklasse. Vererbung bedeutet, dass eine Klasse implizit alle Member seines Typs direkte Basisklasse mit Ausnahme der Instanzkonstruktoren, Destruktoren und statische Konstruktoren der Basisklasse enthält. Einige wichtige Aspekte bei der Vererbung sind:

*  Vererbung ist transitiv. Wenn `C` ergibt sich aus `B`, und `B` ergibt sich aus `A`, klicken Sie dann `C` im deklarierten Member erbt `B` sowie im deklarierten Member `A`.
*  Eine abgeleitete Klasse erweitert seine direkte Basisklasse. Eine abgeleitete Klasse kann den geerbten Membern neue Member hinzufügen, aber die Definition eines geerbten Members kann nicht entfernt werden.
*  Instanzkonstruktoren, Destruktoren und statischen Konstruktoren nicht geerbt werden, aber alle anderen Elemente sind, unabhängig von deren deklarierte Zugriffsart ([Memberzugriff](basic-concepts.md#member-access)). Allerdings können je nach ihrem deklarierten Zugriff geerbte Member in einer abgeleiteten Klasse nicht zugreifen.
*  Kann eine abgeleitete Klasse ***ausblenden*** ([ausblenden, die durch Vererbung von](basic-concepts.md#hiding-through-inheritance)) geerbte Member durch neue Member mit demselben Namen bzw. derselben Signatur deklarieren. Beachten Sie jedoch, die durch das Ausblenden eines geerbten Members dieses Element nicht entfernt wird, lediglich erleichtert diesen Member nicht direkt über der abgeleiteten Klasse zugegriffen werden.
*  Eine Instanz einer Klasse enthält einen Satz von alle Instanzenfelder in der Klasse und ihre Basisklassen und eine implizite Konvertierung deklariert ([implizite Verweis-](conversions.md#implicit-reference-conversions)) von einem abgeleiteten Typ vorhanden ist, um eine der zugehörigen Basisklassentyp konvertiert. Daher kann ein Verweis auf eine Instanz einer abgeleiteten Klasse als Verweis auf eine Instanz einer seiner Basisklassen behandelt werden.
*  Eine Klasse kann virtuelle Methoden, Eigenschaften und Indexer deklarieren, und abgeleitete Klassen können die Implementierung dieser Funktion Member überschreiben. Dies ermöglicht es sich um Klassen, weisen polymorphes Verhalten, bei dem die Aktionen, die durch einen Aufruf der Funktion Mitglied variiert je nach den Laufzeittyp der Instanz über den diese Funktionsmember der Member aufgerufen wird.

Der geerbte Member eines Typs konstruierten Klasse sind die Member des Typs direkten Basisklasse ([Basisklassen](classes.md#base-classes)), die wird ermittelt, indem Sie die Typargumente für jedes Vorkommen des entsprechenden Typs des konstruierten Typs ersetzen Parameter in der *Class_base* Spezifikation. Diese Elemente werden wiederum durch Ersetzen der für die einzelnen transformiert *Type_parameter* in der Element-Deklaration wird das entsprechende *Type_argument* von der *Class_base* Spezifikation.

```csharp
class B<U>
{
    public U F(long index) {...}
}

class D<T>: B<T[]>
{
    public T G(string s) {...}
}
```

Im obigen Beispiel den konstruierten Typ `D<int>` verfügt über einen nicht geerbte Member `public int G(string s)` abgerufen, indem Sie das Typargument ersetzt `int` für den Typparameter `T`. `D<int>` verfügt auch über einen geerbten Member aus der Klassendeklaration `B`. Diese geerbte Member wird bestimmt, indem er zuerst ermittelt den Basisklassentyp `B<int[]>` von `D<int>` ersetzen `int` für `T` in der Basisklasse-Spezifikation `B<T[]>`. Klicken Sie dann als Typargument für `B`, `int[]` durch ersetzt, `U` in `public U F(long index)`, stellt des geerbten Members `public int[] F(long index)`.

### <a name="the-new-modifier"></a>Das new-Modifizierer

Ein *Class_member_declaration* ist zulässig, einen Member mit demselben Namen bzw. derselben Signatur als einen geerbten Member deklarieren. In diesem Fall Members der abgeleiteten Klasse gilt als ***ausblenden*** Member der Basisklasse. Durch das Ausblenden eines geerbten Members ist kein Fehler betrachtet, aber es führt dazu, dass des Compilers eine Warnung ausgegeben. Um die Warnung zu unterdrücken, zählen die Deklaration des Members der abgeleiteten Klasse eine `new` Modifizierer, um anzugeben, dass der abgeleitete Member die Member den Basisklassen ausblenden vorgesehen ist. In diesem Thema wird erläutert. im weiteren [ausblenden, die durch Vererbung von](basic-concepts.md#hiding-through-inheritance).

Wenn eine `new` Modifizierer in einer Deklaration, der einen geerbten Member auszublenden, nicht enthalten ist, eine Warnung zu diesem Zweck wird ausgegeben. Diese Warnung unterdrückt wird, durch das Entfernen der `new` Modifizierer.

### <a name="access-modifiers"></a>Zugriffsmodifizierer

Ein *Class_member_declaration* haben eines der fünf Arten von deklarierte Zugriffsart ([deklariert Barrierefreiheit](basic-concepts.md#declared-accessibility)): `public`, `protected internal`, `protected`, `internal` , oder `private`. Mit Ausnahme der `protected internal` Kombination, es ist ein Fehler während der Kompilierung mehr als eine Zugriffsmodifizierer angeben. Wenn eine *Class_member_declaration* enthält keinen Zugriffsmodifizierer, `private` wird angenommen.

### <a name="constituent-types"></a>Enthaltenen Typen

Typen, die in der Deklaration eines Elements verwendet werden, sind die einzelnen Typen von diesen Member aufgerufen. Mögliche enthaltenen Typen werden der Typ, der eine Konstante, Feld, Eigenschaft, Ereignis oder des Indexers, der Rückgabetyp einer Methode oder einen Operator und die Parametertypen einer Methode, Indexer, Operator oder Instanzenkonstruktor. Die einzelnen Typen eines Elements müssen mindestens dieselben zugriffsmöglichkeiten bieten wie dieser Member selbst sein ([Barrierefreiheit Einschränkungen](basic-concepts.md#accessibility-constraints)).

### <a name="static-and-instance-members"></a>Statische und Mitglieder

Member einer Klasse sind entweder ***statische Member*** oder ***Instanzmember***. Im Allgemeinen ist es nützlich, um statische Member als Klassentypen und Instanzmember als von Objekten (Instanzen von Klassentypen) gehören vorstellen.

Wenn eine Deklaration Feld, Methode, Eigenschaft, Ereignis, Operator oder Konstruktor enthält einen `static` Modifizierer verwenden, wird einen statischen Member deklariert. Darüber hinaus deklariert die Deklaration einer Konstante oder der Typ implizit einen statischen Member. Statische Member weisen folgende Merkmale auf:

*  Wenn ein statischer Member `M` verwiesen wird, einem *Member_access* ([Memberzugriff](expressions.md#member-access)) des Formulars `E.M`, `E` müssen kennzeichnen ein Typ mit `M`. Es ist ein Fehler während der Kompilierung für `E` um eine Instanz zu kennzeichnen.
*  Ein statisches Feld identifiziert genau einen Speicherort für alle Instanzen des angegebenen Klassentyps geschlossene freigegeben werden. Unabhängig davon, wie viele Instanzen eines bestimmten geschlossene Klassentyps erstellt werden müssen Sie immer nur eine Kopie eines statischen Felds vorhanden ist.
*  Ein statischer Funktionsmember (Methode, Eigenschaft, Ereignis, Operator oder Konstruktor) kann nicht in einer bestimmten Instanz ausgeführt werden, und es ist ein Fehler während der Kompilierung zum Verweisen auf `this` in diese ein Funktionsmember.

Wenn eine Feld, Methode, Eigenschaft, Ereignis, Indexer, Konstruktor oder Destruktor Deklaration enthält keine `static` Modifizierer verwenden, wird einen Instanzmember deklariert. (Ein Instanzmember ist einen nicht statisches Member bezeichnet.) Instanzmember weisen folgende Merkmale auf:

*  Wenn ein Instanzmember `M` verwiesen wird, eine *Member_access* ([Memberzugriff](expressions.md#member-access)) des Formulars `E.M`, `E` muss eine Instanz eines Typs mit deuten`M`. Es ist ein Fehler während der Bindung für `E` um einen Typ anzugeben.
*  Jede Instanz einer Klasse enthält einen separaten Satz aller Instanzfelder der Klasse.
*  Ein Instanzmember-Funktion (Methode, Eigenschaft, Indexer, Instance-Konstruktor oder Destruktor), die für eine bestimmte Instanz der Klasse ausgeführt wird, und diese Instanz zugegriffen werden kann, als `this` ([diesen Zugriff](expressions.md#this-access)).

Das folgende Beispiel veranschaulicht die Regeln für den Zugriff auf statische und Instanzmember:
```csharp
class Test
{
    int x;
    static int y;

    void F() {
        x = 1;            // Ok, same as this.x = 1
        y = 1;            // Ok, same as Test.y = 1
    }

    static void G() {
        x = 1;            // Error, cannot access this.x
        y = 1;            // Ok, same as Test.y = 1
    }

    static void Main() {
        Test t = new Test();
        t.x = 1;          // Ok
        t.y = 1;          // Error, cannot access static member through instance
        Test.x = 1;       // Error, cannot access instance member through type
        Test.y = 1;       // Ok
    }
}
```

Die `F` Methode zeigt, dass in der Funktion einen Instanzmember einer *Simple_name* ([einfache Namen](expressions.md#simple-names)) Zugriff auf Instanzmember und statische Member verwendet werden können. Die `G` Methode zeigt, in einem statischen Funktionsmember, ein Fehler während der Kompilierung einen Instanzmember über den Zugriff auf eine *Simple_name*. Die `Main` wird verdeutlicht, dass in einem *Member_access* ([Memberzugriff](expressions.md#member-access)) Instanzmember über Instanzen zugegriffen werden müssen und statische Member über Typen zugegriffen werden müssen.

### <a name="nested-types"></a>Geschachtelte Typen

Ein Typ, in die Deklaration einer Klasse oder Struktur deklariert wird aufgerufen, eine ***geschachtelter Typ***. Ein Typ, der innerhalb einer Kompilierungseinheit oder dem Namespace deklariert wird aufgerufen, wird eine ***nicht geschachtelten Typ***.

Im Beispiel
```csharp
using System;

class A
{
    class B
    {
        static void F() {
            Console.WriteLine("A.B.F");
        }
    }
}
```
Klasse `B` ein geschachtelter Typ ist, da es in der Klasse deklariert ist `A`, und die Klasse `A` ein nicht geschachtelter Typ ist, da er innerhalb einer Kompilierungseinheit deklariert ist.

#### <a name="fully-qualified-name"></a>Vollqualifizierte Namen

Der vollqualifizierte Name ([vollständig qualifizierten Namen](basic-concepts.md#fully-qualified-names)) für ein geschachtelter Typ ist `S.N` , in denen `S` ist der vollqualifizierte Name des Typs in der `N` deklariert wird.

#### <a name="declared-accessibility"></a>Deklarierter Zugriff

Nicht geschachtelten Typen haben `public` oder `internal` deklariert Barrierefreiheit und `internal` Barrierefreiheit werden standardmäßig deklariert. Geschachtelte Typen haben diese Formen des deklarierte Zugriffsart, sowie eine oder mehrere weitere Formen der deklarierte Zugriffsart, je nachdem, ob der enthaltende Typ eine Klasse oder Struktur:

*  Ein geschachtelter Typ, der in einer Klasse deklariert ist, kann jeden der fünf Arten von deklarierte Zugriffsart aufweisen (`public`, `protected internal`, `protected`, `internal`, oder `private`), und wie andere Klassenmember standardmäßig `private` deklariert Barrierefreiheit.
*  Ein geschachtelter Typ, der in einer Struktur deklariert ist, kann jeden der deklarierte Zugriffsart in drei Formen aufweisen (`public`, `internal`, oder `private`), und wie andere Strukturmember standardmäßig `private` Barrierefreiheit deklariert.

Im Beispiel
```csharp
public class List
{
    // Private data structure
    private class Node
    { 
        public object Data;
        public Node Next;

        public Node(object data, Node next) {
            this.Data = data;
            this.Next = next;
        }
    }

    private Node first = null;
    private Node last = null;

    // Public interface
    public void AddToFront(object o) {...}
    public void AddToBack(object o) {...}
    public object RemoveFromFront() {...}
    public object RemoveFromBack() {...}
    public int Count { get {...} }
}
```
deklariert eine private geschachtelte Klasse `Node`.

#### <a name="hiding"></a>Durch das Ausblenden

Ein geschachtelter Typ kann ausblenden ([Ausblenden von Namen](basic-concepts.md#name-hiding)) ein Member der Basisklasse. Die `new` Modifizierer ist in Deklarationen von geschachtelten Typen zulässig, sodass ausblenden explizit ausgedrückt werden kann. Im Beispiel
```csharp
using System;

class Base
{
    public static void M() {
        Console.WriteLine("Base.M");
    }
}

class Derived: Base 
{
    new public class M 
    {
        public static void F() {
            Console.WriteLine("Derived.M.F");
        }
    }
}

class Test 
{
    static void Main() {
        Derived.M.F();
    }
}
```
Zeigt eine geschachtelte Klasse `M` , die die Methode blendet `M` in definierten `Base`.

#### <a name="this-access"></a>Dieser Zugriff

Ein geschachtelter Typ und seinem enthaltenden Typ haben eine besondere Beziehung in Bezug auf nicht *This_access* ([diesen Zugriff](expressions.md#this-access)). Insbesondere `this` innerhalb eines geschachtelten Typs kann nicht verwendet werden, um auf Instanzmember des enthaltenden Typs verweisen. In Fällen, in denen ein geschachtelter Typ Zugriff auf die Instanzmember des enthaltenden Typs benötigt, Zugriff bereitgestellt werden kann, durch die Bereitstellung der `this` für die Instanz des enthaltenden Typs als Konstruktorargument für den geschachtelten Typ. Im folgenden Beispiel wird
```csharp
using System;

class C
{
    int i = 123;

    public void F() {
        Nested n = new Nested(this);
        n.G();
    }

    public class Nested
    {
        C this_c;

        public Nested(C c) {
            this_c = c;
        }

        public void G() {
            Console.WriteLine(this_c.i);
        }
    }
}

class Test
{
    static void Main() {
        C c = new C();
        c.F();
    }
}
```
Dieses Verfahren veranschaulicht. Eine Instanz von `C` erstellt eine Instanz des `Nested` und übergibt Sie eine eigene `this` zu `Nested`Konstruktor, um den Zugriff auf bereitzustellen `C`des Instanzmember.

#### <a name="access-to-private-and-protected-members-of-the-containing-type"></a>Zugriff auf private und geschützte Member des enthaltenden Typs

Ein geschachtelter Typ hat Zugriff auf alle Elemente, die auf seinem enthaltenden Typ, einschließlich, die Mitglieder des enthaltenden Typs zugegriffen werden `private` und `protected` Barrierefreiheit deklariert. Im Beispiel
```csharp
using System;

class C 
{
    private static void F() {
        Console.WriteLine("C.F");
    }

    public class Nested 
    {
        public static void G() {
            F();
        }
    }
}

class Test 
{
    static void Main() {
        C.Nested.G();
    }
}
```
Zeigt eine Klasse `C` , enthält eine geschachtelte Klasse `Nested`. In `Nested`, die Methode `G` Ruft die statische Methode `F` in definierten `C`, und `F` wurde deklariert privaten Zugriff.

Ein geschachtelter Typ kann auch geschützte Member, die in einem Basistyp des enthaltenden Typs definiert zugreifen. Im Beispiel
```csharp
using System;

class Base 
{
    protected void F() {
        Console.WriteLine("Base.F");
    }
}

class Derived: Base 
{
    public class Nested 
    {
        public void G() {
            Derived d = new Derived();
            d.F();        // ok
        }
    }
}

class Test 
{
    static void Main() {
        Derived.Nested n = new Derived.Nested();
        n.G();
    }
}
```
die geschachtelte Klasse `Derived.Nested` greift auf die geschützte Methode `F` in definierten `Derived`Basisklasse, `Base`, von Aufrufen durch eine Instanz der `Derived`.

#### <a name="nested-types-in-generic-classes"></a>Geschachtelte Typen in generische Klassen

Deklaration einer generischen Klasse kann über geschachtelte Typdeklarationen enthalten. Die Typparameter der einschließenden Klasse können innerhalb der geschachtelten Typen verwendet werden. Eine geschachtelten Typdeklaration kann weitere Typparameter enthalten, die nur für den geschachtelten Typ gelten.

Jede Typdeklaration, die innerhalb der Deklaration einer generischen Klasse ist implizit die Deklaration eines generischen Typs. Schreiben einen Verweis auf einen Typ in einem generischen Typ geschachtelt werden, muss mit konstruierten Typs, einschließlich seiner Typargumente benannt werden. Allerdings kann von innerhalb der äußeren Klasse der geschachtelte Typ ohne Qualifikation verwendet werden; der Instanztyp der äußeren Klasse kann implizit verwendet werden, beim Erstellen des geschachtelten Typs. Das folgende Beispiel zeigt drei richtige Möglichkeiten zum Verweisen auf einen konstruierten Typ aus erstellt `Inner`; die ersten beiden sind gleichwertig:
```csharp
class Outer<T>
{
    class Inner<U>
    {
        public static void F(T t, U u) {...}
    }

    static void F(T t) {
        Outer<T>.Inner<string>.F(t, "abc");      // These two statements have
        Inner<string>.F(t, "abc");               // the same effect

        Outer<int>.Inner<string>.F(3, "abc");    // This type is different

        Outer.Inner<string>.F(t, "abc");         // Error, Outer needs type arg
    }
}
```

Obwohl das Gegenteil kann Mitglied Programmierstil, ein Typparameter in einem geschachtelten Typ oder ausblenden Typparameter des äußeren Typs deklariert:
```csharp
class Outer<T>
{
    class Inner<T>        // Valid, hides Outer's T
    {
        public T t;       // Refers to Inner's T
    }
}
```

### <a name="reserved-member-names"></a>Reservierte Namen

Um die zugrunde liegenden C#-Laufzeit-Implementierung für jede Quelle-Memberdeklaration zu ermöglichen, die eine Eigenschaft, Ereignis oder Indexer muss die Implementierung zwei Methodensignaturen, die je nach der Art der Memberdeklaration, den Namen und den Typ reservieren. Es ist ein Fehler während der Kompilierung für ein Programm, auch wenn die zugrunde liegende Implementierung für die Laufzeit keine vornimmt deklariert ein Element, dessen Signatur, eines der folgenden übereinstimmt, reservierten Signaturen, verwenden Sie diese Reservierungen.

Die reservierten Namen keine Deklarationen einführen, daher beim Nachschlagen von Membern nicht beteiligt. Eine Deklaration zugeordneten jedoch reservierte Methode Signaturen bei der Vererbung teilnehmen ([Vererbung](classes.md#inheritance)), und können ausgeblendet werden, mit der `new` Modifizierer ([der new-Modifizierer](classes.md#the-new-modifier)).

Die Reservierung dieser Namen erfüllt drei Zwecke:

*  Um die zugrunde liegende Implementierung einen normalen Bezeichner als Methodenname für Get verwenden, oder legen Sie den Zugriff auf die C#-Sprachfunktion zu ermöglichen.
*  Um andere Sprachen zusammenarbeiten, verwenden einen normalen Bezeichner als Methodenname für Get oder legen Sie den Zugriff auf die C#-Sprachfunktion zu ermöglichen.
*  Um sicherzustellen, dass die Quelle von einem konformen Compiler akzeptiert werden durch einen anderen, akzeptiert, dazu die Einzelheiten der reservierten Member Namen konsistent über alle C#-Implementierungen.

Die Deklaration eines Destruktors ([Destruktoren](classes.md#destructors)) bewirkt auch, dass eine Signatur reserviert werden sollen ([für Destruktoren Membernamen](classes.md#member-names-reserved-for-destructors)).

#### <a name="member-names-reserved-for-properties"></a>Elementnamen, die für Eigenschaften reserviert

Für eine Eigenschaft `P` ([Eigenschaften](classes.md#properties)) vom Typ `T`, die folgenden Signaturen sind reserviert:

```csharp
T get_P();
void set_P(T value);
```

Beide Signaturen sind reserviert, auch wenn die Eigenschaft schreibgeschützt oder lesegeschützt ist.

Im Beispiel
```csharp
using System;

class A
{
    public int P {
        get { return 123; }
    }
}

class B: A
{
    new public int get_P() {
        return 456;
    }

    new public void set_P(int value) {
    }
}

class Test
{
    static void Main() {
        B b = new B();
        A a = b;
        Console.WriteLine(a.P);
        Console.WriteLine(b.P);
        Console.WriteLine(b.get_P());
    }
}
```
eine Klasse `A` definiert eine schreibgeschützte Eigenschaft `P`, also Reservieren von Signaturen für `get_P` und `set_P` Methoden. Eine Klasse `B` leitet sich von `A` und blendet Sie aus dieser reservierte Signaturen. Das Beispiel erzeugt die Ausgabe:
```
123
123
456
```

#### <a name="member-names-reserved-for-events"></a>Elementnamen, die für Ereignisse reserviert

Für ein Ereignis `E` ([Ereignisse](classes.md#events)) eines Delegattyps `T`, die folgenden Signaturen sind reserviert:
```csharp
void add_E(T handler);
void remove_E(T handler);
```

#### <a name="member-names-reserved-for-indexers"></a>Für Indexer reservierte Elementnamen

Für einen Indexer ([Indexer](classes.md#indexers)) vom Typ `T` mit Parameter-List `L`, die folgenden Signaturen sind reserviert:
```csharp
T get_Item(L);
void set_Item(L, T value);
```

Beide Signaturen sind reserviert, selbst wenn der Indexer schreibgeschützt oder lesegeschützt ist.

Außerdem den Namen des Members `Item` ist reserviert.

#### <a name="member-names-reserved-for-destructors"></a>Elementnamen, die für Destruktoren reserviert

Für eine Klasse, die einen Destruktor enthält ([Destruktoren](classes.md#destructors)), die folgende Signatur ist reserviert:
```csharp
void Finalize();
```

## <a name="constants"></a>Konstanten

Ein ***Konstanten*** ist ein Klassenmember, der einen konstanten Wert darstellt: ein Wert, der zur Kompilierungszeit berechnet werden kann. Ein *Constant_declaration* führt eine oder mehrere Konstanten eines bestimmten Typs.

```antlr
constant_declaration
    : attributes? constant_modifier* 'const' type constant_declarators ';'
    ;

constant_modifier
    : 'new'
    | 'public'
    | 'protected'
    | 'internal'
    | 'private'
    ;

constant_declarators
    : constant_declarator (',' constant_declarator)*
    ;

constant_declarator
    : identifier '=' constant_expression
    ;
```

Ein *Constant_declaration* eventuell einen Satz von *Attribute* ([Attribute](attributes.md)), ein `new` Modifizierer ([der new-Modifizierer](classes.md#the-new-modifier)), und eine gültige Kombination der vier Zugriffsmodifizierer ([Zugriffsmodifizierer](classes.md#access-modifiers)). Die Attribute und Modifizierer, gelten für alle Member deklariert, indem die *Constant_declaration*. Obwohl Konstanten statische Member, gelten eine *Constant_declaration* erfordert und keine ermöglicht eine `static` Modifizierer. Es ist ein Fehler für den gleichen Modifizierer für mehrere Male in einer Konstantendeklaration angezeigt werden.

Die *Typ* von einem *Constant_declaration* gibt den Typ der Elemente, die durch die Deklaration eingeführt. Der Typ folgt eine Liste der *Constant_declarator*s, von denen jede einen neuen Member führt. Ein *Constant_declarator* besteht aus einer *Bezeichner* mit dem das Element, gefolgt vom Namen einer "`=`" token, gefolgt von einer *Constant_expression* ([ Konstante Ausdrücke](expressions.md#constant-expressions)), die den Wert des Elements bietet.

Die *Typ* in eine Konstante angegeben Deklaration muss `sbyte`, `byte`, `short`, `ushort`, `int`, `uint`, `long`, `ulong`, `char`, `float`, `double`, `decimal`, `bool`, `string`, *Enum_type*, oder ein *Reference_type*. Jede *Constant_expression* muss einen Wert des Zieltyps oder eines Typs, der in den Zieltyp konvertiert werden kann, indem eine implizite Konvertierung werden liefern ([implizite Konvertierungen](conversions.md#implicit-conversions)).

Die *Typ* einer Konstante muss mindestens dieselben zugriffsmöglichkeiten bieten wie die Konstante selbst ([Barrierefreiheit Einschränkungen](basic-concepts.md#accessibility-constraints)).

Der Wert einer Konstante wird ermittelt, in ein Ausdruck mit einem *Simple_name* ([einfache Namen](expressions.md#simple-names)) oder ein *Member_access* ([Memberzugriff](expressions.md#member-access)).

Eine Konstante selbst teilnehmen kann eine *Constant_expression*. Daher kann eine Konstante verwendet werden, in einem beliebigen Konstrukt, das erfordert eine *Constant_expression*. Beispiele für solche Konstrukte `case` Bezeichnungen `goto case` Anweisungen `enum` Memberdeklarationen, Attribute und andere Konstanten Deklarationen.

Siehe [Konstante Ausdrücke](expressions.md#constant-expressions), *Constant_expression* ist ein Ausdruck, der zum Zeitpunkt der Kompilierung vollständig ausgewertet werden kann. Da die einzige Möglichkeit zum Erstellen eines nicht-Null-Wert, der eine *Reference_type* außer `string` angewendet werden soll die `new` -Operator, und da die `new` Operator ist nicht zulässig, eine *Constant_ Ausdruck*, der einzig mögliche Wert für Konstanten von *Reference_type*s außer `string` ist `null`.

Wenn ein symbolische Namen für einen konstanten Wert gewünscht ist, aber wenn der Typ des Werts in einer Konstantendeklaration nicht zulässig ist oder wenn der Wert nicht zum Zeitpunkt der Kompilierung von berechnet werden kann eine *Constant_expression*, `readonly` (Feld [Schreibgeschützte Felder](classes.md#readonly-fields)) kann stattdessen verwendet werden.

Deklaration eine Konstante, die mehrere Konstanten deklariert entspricht mehreren Deklarationen der einzelnen Konstanten, mit der gleichen Attribute, Modifizierer, und geben. Beispiel:
```csharp
class A
{
    public const double X = 1.0, Y = 2.0, Z = 3.0;
}
```
für die folgende Syntax:
```csharp
class A
{
    public const double X = 1.0;
    public const double Y = 2.0;
    public const double Z = 3.0;
}
```

Konstanten sind andere Konstanten innerhalb desselben Programms abhängig ist, solange die Abhängigkeiten keine zirkuläre Natur sind zulässig. Der Compiler ordnet automatisch die Konstanten Deklarationen in der richtigen Reihenfolge ausgewertet. Im Beispiel
```csharp
class A
{
    public const int X = B.Z + 1;
    public const int Y = 10;
}

class B
{
    public const int Z = A.Y + 1;
}
```
der Compiler wertet zunächst `A.Y`, wertet dann `B.Z`, und schließlich wertet `A.X`, die Werte erzeugen `10`, `11`, und `12`. Konstante Deklarationen können auf Konstanten aus anderen Programmen abhängig sind, aber solche Abhängigkeiten sind nur in einer Richtung möglich. Im obigen Beispiel auf, wenn `A` und `B` deklariert wurden in verschiedenen Programmen, es wäre möglich, dass `A.X` hängt von `B.Z`, aber `B.Z` können nicht gleichzeitig klicken Sie dann abhängig `A.Y`.

## <a name="fields"></a>Felder

Ein ***Feld*** ist ein Member, der eine Variable zugeordnet, ein Objekt oder eine Klasse darstellt. Ein *Field_declaration* führt eine oder mehrere Felder eines angegebenen Typs.

```antlr
field_declaration
    : attributes? field_modifier* type variable_declarators ';'
    ;

field_modifier
    : 'new'
    | 'public'
    | 'protected'
    | 'internal'
    | 'private'
    | 'static'
    | 'readonly'
    | 'volatile'
    | field_modifier_unsafe
    ;

variable_declarators
    : variable_declarator (',' variable_declarator)*
    ;

variable_declarator
    : identifier ('=' variable_initializer)?
    ;

variable_initializer
    : expression
    | array_initializer
    ;
```

Ein *Field_declaration* eventuell einen Satz von *Attribute* ([Attribute](attributes.md)), ein `new` Modifizierer ([der new-Modifizierer](classes.md#the-new-modifier)), gültige Kombination der vier Zugriffsmodifizierer ([Zugriffsmodifizierer](classes.md#access-modifiers)), und ein `static` Modifizierer ([statische Felder und Instanzfelder](classes.md#static-and-instance-fields)). Darüber hinaus eine *Field_declaration* eventuell eine `readonly` Modifizierer ([schreibgeschützte Felder](classes.md#readonly-fields)) oder ein `volatile` Modifizierer ([flüchtige Felder](classes.md#volatile-fields)) jedoch nicht beides. Die Attribute und Modifizierer, gelten für alle Member deklariert, indem die *Field_declaration*. Es ist ein Fehler für den gleichen Modifizierer für mehrere Male in einer Felddeklaration angezeigt werden.

Die *Typ* von einem *Field_declaration* gibt den Typ der Elemente, die durch die Deklaration eingeführt. Der Typ folgt eine Liste der *Variable_declarator*s, von denen jede einen neuen Member führt. Ein *Variable_declarator* besteht aus einer *Bezeichner* mit dem Namen dieses Members, optional gefolgt von einer "`=`" token und einem *Variable_initializer* ([ Variableninitialisierern](classes.md#variable-initializers)), die ermöglicht, dass des Anfangswert dieses Members.

Die *Typ* eines Felds muss mindestens dieselben zugriffsmöglichkeiten bieten wie das Feld selbst ([Barrierefreiheit Einschränkungen](basic-concepts.md#accessibility-constraints)).

Der Wert eines Felds wird ermittelt, in einem Ausdruck mit einer *Simple_name* ([einfache Namen](expressions.md#simple-names)) oder ein *Member_access* ([Memberzugriff](expressions.md#member-access)). Der Wert eines Felds nicht schreibgeschützte geändert wird, mithilfe einer *Zuweisung* ([Zuweisungsoperatoren](expressions.md#assignment-operators)). Der Wert eines Felds nicht schreibgeschützte kann sein, abgerufen und das Bearbeiten mit postfix-Inkrement und Dekrement-Operatoren ([Postfix-Inkrement und Dekrement-Operatoren](expressions.md#postfix-increment-and-decrement-operators)) und Präfix-Inkrement und Dekrement-Operatoren ([Präfix Inkrement-und Dekrementoperatoren](expressions.md#prefix-increment-and-decrement-operators)).

Eine Felddeklaration, die mehrere Felder deklariert entspricht mehreren Deklarationen der einzelnen Felder mit der gleichen Attribute, Modifizierer, und geben. Beispiel:
```csharp
class A
{
    public static int X = 1, Y, Z = 100;
}
```
für die folgende Syntax:
```csharp
class A
{
    public static int X = 1;
    public static int Y;
    public static int Z = 100;
}
```

### <a name="static-and-instance-fields"></a>Statische und Felder

Wenn eine Felddeklaration enthält eine `static` Modifizierer, die Felder, die durch die Deklaration eingeführt sind ***statische Felder***. Wenn kein `static` Modifizierer vorhanden ist, wird die Felder, die durch die Deklaration eingeführt sind ***Instanzfelder***. Statische Felder und Instanzfelder sind zwei verschiedene Arten von Variablen ([Variablen](variables.md)) von C# unterstützt, und gelegentlich werden sie bezeichnet als ***statische Variablen*** und ***Instanzvariablen*** bzw.

Ein statisches Feld gehört nicht zu einer bestimmten Instanz. Stattdessen es für alle Instanzen eines Typs geschlossene genutzt ([offene und geschlossene Typen](types.md#open-and-closed-types)). Unabhängig davon, wie viele Instanzen eines geschlossenen Klassentyps erstellt werden ist es immer nur eine Kopie eines statischen Felds für die Domäne der zugehörigen Anwendung.

Zum Beispiel:
```csharp
class C<V>
{
    static int count = 0;

    public C() {
        count++;
    }

    public static int Count {
        get { return count; }
    }
}

class Application
{
    static void Main() {
        C<int> x1 = new C<int>();
        Console.WriteLine(C<int>.Count);        // Prints 1

        C<double> x2 = new C<double>();
        Console.WriteLine(C<int>.Count);        // Prints 1

        C<int> x3 = new C<int>();
        Console.WriteLine(C<int>.Count);        // Prints 2
    }
}
```

Ein Instanzenfeld gehört zu einer Instanz. Insbesondere enthält jede Instanz einer Klasse einen separaten Satz von aller Instanzfelder dieser Klasse.

Wenn ein Feld verwiesen wird, einem *Member_access* ([Memberzugriff](expressions.md#member-access)) des Formulars `E.M`, wenn `M` ist ein statisches Feld `E` müssen kennzeichnen ein Typ mit `M` , und wenn `M` ist ein Instanzfeld E muss eine Instanz von einem Typ mit deuten `M`.

Die Unterschiede zwischen statischen und Instanzmember finden Sie weiter unten in [statische und Instanzmember](classes.md#static-and-instance-members).

### <a name="readonly-fields"></a>Schreibgeschützte Felder

Wenn eine *Field_declaration* enthält eine `readonly` Modifizierer, die Felder, die durch die Deklaration eingeführt sind ***schreibgeschützte Felder***. Direkte Zuweisungen an schreibgeschützte Felder können nur als Teil dieser Deklaration oder in einem Instanzenkonstruktor oder einen statischen Konstruktor in derselben Klasse auftreten. (Ein schreibgeschütztes Feld kann mehrere Male in diesen Kontexten zugewiesen werden.) Insbesondere direkten Zuweisungen für eine `readonly` Feld sind nur in den folgenden Kontexten zulässig:

*  In der *Variable_declarator* wird das Feld (mit einem *Variable_initializer* in der Deklaration).
*  Für ein Instanzenfeld in den Instanzkonstruktoren der Klasse, die die Felddeklaration enthält. für ein statisches Feld im statischen Konstruktor der Klasse, die die Felddeklaration enthält. Hierbei handelt es sich auch um die einzigen Fälle, in dem Übergeben einer `readonly` Feld als ein `out` oder `ref` Parameter.

Beim Zuweisen einer `readonly` Feld ein, oder übergeben Sie sie als ein `out` oder `ref` Parameter in einem anderen Kontext ist ein Fehler während der Kompilierung.

#### <a name="using-static-readonly-fields-for-constants"></a>Verwenden für Konstanten statische schreibgeschützte Felder

Ein `static readonly` Feld ist hilfreich, wenn ein symbolische Namen für einen konstanten Wert gewünscht ist, aber wenn der Typ des Werts in nicht zulässig ist eine `const` Deklaration, oder wenn der Wert nicht zur Kompilierzeit berechnet werden kann. Im Beispiel
```csharp
public class Color
{
    public static readonly Color Black = new Color(0, 0, 0);
    public static readonly Color White = new Color(255, 255, 255);
    public static readonly Color Red = new Color(255, 0, 0);
    public static readonly Color Green = new Color(0, 255, 0);
    public static readonly Color Blue = new Color(0, 0, 255);

    private byte red, green, blue;

    public Color(byte r, byte g, byte b) {
        red = r;
        green = g;
        blue = b;
    }
}
```
die `Black`, `White`, `Red`, `Green`, und `Blue` Member können nicht deklariert werden, als `const` Member, da ihre Werte nicht zur Kompilierungszeit berechnet werden können. Deklarieren Sie sie jedoch `static readonly` stattdessen nahezu die gleiche Wirkung hat.

#### <a name="versioning-of-constants-and-static-readonly-fields"></a>Versionsverwaltung von Konstanten und statische schreibgeschützte Felder

Konstanten und schreibgeschützte Felder haben eine unterschiedliche binäre versionsverwaltung-Semantik. Wenn eine Konstante in ein Ausdruck verwiesen wird, wird der Wert der Konstanten zur Kompilierzeit abgerufen, aber wenn ein Ausdruck ein schreibgeschütztes Feld verwiesen wird, wird der Wert des Felds nicht bis zur Laufzeit abgerufen. Betrachten Sie eine Anwendung, die aus zwei verschiedenen Programmen besteht aus:
```csharp
using System;

namespace Program1
{
    public class Utils
    {
        public static readonly int X = 1;
    }
}

namespace Program2
{
    class Test
    {
        static void Main() {
            Console.WriteLine(Program1.Utils.X);
        }
    }
}
```

Die `Program1` und `Program2` Namespaces kennzeichnen die beiden Programme, die separat kompiliert werden. Da `Program1.Utils.X` ist als ein statisches schreibgeschütztes Feld, die Ausgabe des Werts von deklariert die `Console.WriteLine` Anweisung zur Kompilierzeit nicht bekannt, aber stattdessen zur Laufzeit abgerufen wird. Also wenn der Wert des `X` geändert wird und `Program1` erneut kompiliert wird, die `Console.WriteLine` Anweisung gibt der neue Wert, selbst wenn `Program2` ist nicht neu kompiliert. Hatte jedoch `X` wurde eine Konstante, die den Wert der `X` würde abgerufen haben, wurden zum Zeitpunkt `Program2` kompiliert wurde, und würde nicht betroffen von Änderungen in `Program1` bis `Program2` neu kompiliert wird.

### <a name="volatile-fields"></a>Flüchtige Felder

Wenn eine *Field_declaration* enthält eine `volatile` Modifizierer, die Felder, der durch diese Deklaration sind ***flüchtige Felder***.

Für nicht flüchtige Felder Optimierungstechniken, die Anweisungen neu anordnet können zu unerwarteten und unvorhersehbaren Ergebnissen führen in Multithreaded-Programmen, die Zugriff auf Felder ohne Synchronisierung, z. B. über die *Lock_statement*  ([Die sperranweisung](statements.md#the-lock-statement)). Diese Optimierungen können durch den Compiler, durch die Laufzeit-System oder von der Hardware ausgeführt werden. Für flüchtige Felder sind solche Neuanordnen von Optimierungen beschränkt:

*  Ein Lesevorgang eines flüchtigen Felds wird aufgerufen, eine ***Volatile lesen***. Ein flüchtiger Lesevorgang wurde "acquire-Semantik;" Es ist garantiert, also bevor alle Verweise auf den Speicher auftreten, die auftreten, nachdem sie in der anweisungsabfolge.
*  Ein Schreibvorgang eines flüchtigen Felds wird aufgerufen, eine ***flüchtiger Schreibvorgang***. Ein flüchtiger Schreibvorgang hat "release Semantik"; Es ist garantiert, also nach Verweise Arbeitsspeicher vor der Write-Anweisung in der anweisungsabfolge ausgeführt.

Diese Einschränkungen stellen sicher, dass alle Threads volatile-Schreibvorgänge, die von einem anderen Thread ausgeführt werden, in der Reihenfolge ihrer Ausführung berücksichtigen. Keine entsprechende Implementierung ist nicht erforderlich, zu der eine einzelne gesamte Sortierung von flüchtigen Schreibvorgänge von allen Threads der Ausführung sehen. Der Typ eines flüchtigen Felds muss es sich um eine der folgenden sein:

*  Ein *Reference_type*.
*  Der Typ `byte`, `sbyte`, `short`, `ushort`, `int`, `uint`, `char`, `float`, `bool`, `System.IntPtr`, oder` System.UIntPtr`.
*  Ein *Enum_type* müssen einen Enum-Basistyp des `byte`, `sbyte`, `short`, `ushort`, `int`, oder `uint`.

Im Beispiel
```csharp
using System;
using System.Threading;

class Test
{
    public static int result;   
    public static volatile bool finished;

    static void Thread2() {
        result = 143;    
        finished = true; 
    }

    static void Main() {
        finished = false;

        // Run Thread2() in a new thread
        new Thread(new ThreadStart(Thread2)).Start();

        // Wait for Thread2 to signal that it has a result by setting
        // finished to true.
        for (;;) {
            if (finished) {
                Console.WriteLine("result = {0}", result);
                return;
            }
        }
    }
}
```
erzeugt die Ausgabe:
```
result = 143
```

In diesem Beispiel ist die Methode `Main` startet einen neuen Thread, der die Methode ausgeführt wird `Thread2`. Diese Methode speichert einen Wert in ein nicht flüchtiges Feld namens `result`, dann speichert `true` im flüchtigen Felds `finished`. Der Hauptthread wartet auf das Feld `finished` festgelegt werden, um `true`, liest dann das Feld `result`. Da `finished` deklariert wurde, `volatile`, der Haupt-Thread muss den Wert lesen `143` aus dem Feld `result`. Wenn das Feld `finished` nicht deklariert wurde `volatile`, es wäre für den Store, um zulässige `result` an den primären Thread nach dem Speichervorgang für sichtbar `finished`, und somit für den Hauptthread, lesen den Wert `0` aus der Feld `result`. Deklarieren von `finished` als eine `volatile` Feld verhindert, dass solche Inkonsistenzen.

### <a name="field-initialization"></a>Feld-Initialisierung

Der Anfangswert eines Felds, ob es ein statisches Feld oder ein Instanzenfeld, ist der Standardwert ([Standardwerte](variables.md#default-values)) der Typ des Felds. Es ist nicht möglich, beachten Sie den Wert eines Felds aus, bevor diese standardinitialisierung aufgetreten, und ein Feld daher nie ist "nicht initialisiert". Im Beispiel
```csharp
using System;

class Test
{
    static bool b;
    int i;

    static void Main() {
        Test t = new Test();
        Console.WriteLine("b = {0}, i = {1}", b, t.i);
    }
}
```
erzeugt die Ausgabe
```
b = False, i = 0
```
Da `b` und `i` sind beide automatisch mit Standardwerten initialisiert.

### <a name="variable-initializers"></a>Variableninitialisierern

Herausgeberkontos ausgewiesenen Form Felddeklarationen *Variable_initializer*s. Für statische Felder entsprechen Variableninitialisierern zuweisungsanweisungen, die während der klasseninitialisierung ausgeführt werden. Beispielsweise entsprechen Feldern Variableninitialisierern zuweisungsanweisungen, die ausgeführt werden, wenn eine Instanz der Klasse erstellt wird.

Im Beispiel
```csharp
using System;

class Test
{
    static double x = Math.Sqrt(2.0);
    int i = 100;
    string s = "Hello";

    static void Main() {
        Test a = new Test();
        Console.WriteLine("x = {0}, i = {1}, s = {2}", x, a.i, a.s);
    }
}
```
erzeugt die Ausgabe
```
x = 1.4142135623731, i = 100, s = Hello
```
Da eine Zuweisung zu `x` tritt auf, wenn statischen Feldinitialisierer ausführen und Zuweisungen zu `i` und `s` auftreten, wenn die Instanzenfeldinitialisierer ausführen.

Die standardinitialisierung-Wert in beschriebenen [Feld Initialisierung](classes.md#field-initialization) tritt für alle Felder einschließlich der Felder, die Variable initialisiert werden. Wenn eine Klasse initialisiert wird, daher alle statischen Felder in dieser Klasse werden zuerst auf ihre Standardwerte initialisiert, und klicken Sie dann die statischen Feldinitialisierer in der Reihenfolge im Text ausgeführt werden. Wenn eine Instanz einer Klasse erstellt wird, ebenso alle Instanzenfelder in dieser Instanz werden zuerst auf ihre Standardwerte initialisiert, und klicken Sie dann die Instanzenfeldinitialisierer in der Reihenfolge im Text ausgeführt werden.

Es ist möglich, für statische Felder mit Variableninitialisierern an, die im Standardzustand Wert beachtet werden. Dies ist jedoch dringend davon abgeraten, als eine Frage des Stils. Im Beispiel
```csharp
using System;

class Test
{
    static int a = b + 1;
    static int b = a + 1;

    static void Main() {
        Console.WriteLine("a = {0}, b = {1}", a, b);
    }
}
```
Dieses Verhalten dargestellt. Trotz der zirkulären Definitionen von einer und b, das Programm ist ungültig. Dies führt in der Ausgabe
```
a = 1, b = 2
```
Da die statischen Felder `a` und `b` werden initialisiert, um `0` (der Standardwert für `int`) vor der Ausführung sind. Bei den Initialisierer für `a` ausgeführt wird, wird den Wert des `b` ist 0 (null), und daher `a` wird initialisiert `1`. Bei den Initialisierer für `b` ausgeführt wird, wird den Wert des `a` ist bereits `1`, und daher `b` wird initialisiert `2`.

#### <a name="static-field-initialization"></a>Initialisierung statischer Felder

Die statische Variable Feldinitialisierer einer Klasse entsprechen einer Sequenz von Zuweisungen, die in der Reihenfolge im Text ausgeführt werden, in denen sie in der Klassendeklaration angezeigt werden. Wenn ein statischer Konstruktor ([statische Konstruktoren](classes.md#static-constructors)) vorhanden ist in der Klasse, von der statischen Feldinitialisierer ausgeführt direkt vor dem Ausführen dieser statischen Konstruktors. Andernfalls werden die statischen Feldinitialisierer zu einem Zeitpunkt abhängig von der Implementierung vor der ersten Verwendung eines statischen Felds dieser Klasse ausgeführt. Im Beispiel
```csharp
using System;

class Test 
{ 
    static void Main() {
        Console.WriteLine("{0} {1}", B.Y, A.X);
    }

    public static int F(string s) {
        Console.WriteLine(s);
        return 1;
    }
}

class A
{
    public static int X = Test.F("Init A");
}

class B
{
    public static int Y = Test.F("Init B");
}
```
kann entweder die Ausgabe erzeugt:
```
Init A
Init B
1 1
```
oder der Ausgabe:
```
Init B
Init A
1 1
```
Da die Ausführung von `X`Initialisierer und `Y`Initialisierer konnte in beliebiger Reihenfolge auftreten; diese sind nur eingeschränkt, die auftreten, bevor Sie die Verweise auf diese Felder. Aber im Beispiel:
```csharp
using System;

class Test
{
    static void Main() {
        Console.WriteLine("{0} {1}", B.Y, A.X);
    }

    public static int F(string s) {
        Console.WriteLine(s);
        return 1;
    }
}

class A
{
    static A() {}

    public static int X = Test.F("Init A");
}

class B
{
    static B() {}

    public static int Y = Test.F("Init B");
}
```
die Ausgabe muss sein:
```
Init B
Init A
1 1
```
Da die Regeln für die beim Ausführen der statischer Konstruktors (gemäß [statische Konstruktoren](classes.md#static-constructors)) bereitstellen, die `B`des statischen Konstruktor (und somit `B`des statischen Feldinitialisierer) müssen ausführen, bevor `A`des statischen Konstruktor und Feldinitialisierer.

#### <a name="instance-field-initialization"></a>Initialisierung von Instanzfeldern

Die Variable Instanzenfeld einer Klasse entsprechen einer Sequenz von Zuweisungen, die beim Einstieg in eines der Instanzkonstruktoren sofort ausgeführt werden ([Konstruktorinitialisierer](classes.md#constructor-initializers)) dieser Klasse. Die Variable Initialisierer werden in der Reihenfolge im Text ausgeführt in denen sie in der Klassendeklaration vorkommen. Die Klasse erstellen und initialisieren Prozess ist unter [Instanzkonstruktoren](classes.md#instance-constructors).

Einem Variableninitialisierer für ein Instanzfeld kann nicht zu erstellende Instanz verweisen. Daher wird ein Kompilierzeitfehler auf `this` in einem Variableninitialisierer, als es ist ein Fehler während der Kompilierung für eine Variable Initialisierer auf beliebiger Instanzmember über eine *Simple_name*. Im Beispiel
```csharp
class A
{
    int x = 1;
    int y = x + 1;        // Error, reference to instance member of this
}
```
die Variable Initialisierer für `y` führt zu einem Fehler während der Kompilierung, da sie ein Mitglied der zu erstellenden Instanz verweist.

## <a name="methods"></a>Methoden

Eine ***Methode*** ist ein Member, das eine Berechnung oder eine Aktion implementiert, die durch ein Objekt oder eine Klasse durchgeführt werden kann. Methoden deklariert werden, mithilfe von *Method_declaration*s:

```antlr
method_declaration
    : method_header method_body
    ;

method_header
    : attributes? method_modifier* 'partial'? return_type member_name type_parameter_list?
      '(' formal_parameter_list? ')' type_parameter_constraints_clause*
    ;

method_modifier
    : 'new'
    | 'public'
    | 'protected'
    | 'internal'
    | 'private'
    | 'static'
    | 'virtual'
    | 'sealed'
    | 'override'
    | 'abstract'
    | 'extern'
    | 'async'
    | method_modifier_unsafe
    ;

return_type
    : type
    | 'void'
    ;

member_name
    : identifier
    | interface_type '.' identifier
    ;

method_body
    : block
    | '=>' expression ';'
    | ';'
    ;
```

Ein *Method_declaration* eventuell einen Satz von *Attribute* ([Attribute](attributes.md)) und eine gültige Kombination der vier Zugriffsmodifizierer ([Zugriffsmodifizierer ](classes.md#access-modifiers)), wird die `new` ([der new-Modifizierer](classes.md#the-new-modifier)), `static` ([statische und Instanzmethoden](classes.md#static-and-instance-methods)), `virtual` ([virtuelle Methoden](classes.md#virtual-methods)), `override` ([Methoden außer Kraft setzen](classes.md#override-methods)), `sealed` ([Versiegelte Methoden](classes.md#sealed-methods)), `abstract` ([abstrakten Methoden](classes.md#abstract-methods)), und `extern` ([Externe Methoden](classes.md#external-methods)) Modifizierer.

Eine Deklaration verfügt über eine gültige Kombination von Modifizierern aus, wenn alle der folgenden Bedingungen erfüllt sind:

*  Die Deklaration enthält eine gültige Kombination von Zugriffsmodifizierer ([Zugriffsmodifizierer](classes.md#access-modifiers)).
*  Die Deklaration ist nicht derselben Modifizierer mehrmals enthalten.
*  Die Deklaration enthält mindestens eine der folgenden Modifizierer: `static`, `virtual`, und `override`.
*  Die Deklaration enthält mindestens eine der folgenden Modifizierer: `new` und `override`.
*  Wenn die Deklaration enthält die `abstract` Modifizierer, und klicken Sie dann auf die Deklaration umfasst keine der folgenden Modifizierer: `static`, `virtual`, `sealed` oder `extern`.
*  Wenn die Deklaration enthält die `private` Modifizierer, und klicken Sie dann auf die Deklaration umfasst keine der folgenden Modifizierer: `virtual`, `override`, oder `abstract`.
*  Wenn die Deklaration enthält die `sealed` Modifizierer, und klicken Sie dann auf die Deklaration enthält auch die `override` Modifizierer.
*  Wenn die Deklaration enthält die `partial` Modifizierer, dann umfasst keine der folgenden Modifizierer: `new`, `public`, `protected`, `internal`, `private`, `virtual`, `sealed`, `override` , `abstract`, oder `extern`.

Eine Methode mit dem `async` -Modifizierer ist eine asynchrone Funktion und die in beschriebenen Regeln folgt [asynchrone Funktionen](classes.md#async-functions).

Die *Return_type* Deklaration gibt den Typ des Werts berechnet und zurückgegeben wird, von der Methode einer Methode an. Die *Return_type* ist `void` , wenn die Methode keinen Wert zurückgibt. Wenn die Deklaration enthält die `partial` Modifizierer, und klicken Sie dann auf der Rückgabetyp muss `void`.

Die *Member_name* gibt den Namen der Methode. Wenn die Methode eine explizite Schnittstellenmember-Implementierung ist ([explizite Implementierungen eines Schnittstellenmembers](interfaces.md#explicit-interface-member-implementations)), wird die *Member_name* ist einfach ein *Bezeichner*. Für eine explizite Schnittstellenmember-Implementierung die *Member_name* besteht aus einer *Interface_type* gefolgt von einem "`.`" und ein *Bezeichner*.

Der optionale *Type_parameter_list* gibt an, die Typparameter der Methode ([Typparameter](classes.md#type-parameters)). Wenn eine *Type_parameter_list* angegeben ist, wird die Methode eine ***generische Methode***. Wenn die Methode verfügt über eine `extern` Modifizierer eine *Type_parameter_list* kann nicht angegeben werden.

Der optionale *Formal_parameter_list* gibt die Parameter der Methode ([Methodenparameter](classes.md#method-parameters)).

Der optionale *Type_parameter_constraints_clause*s angeben von Einschränkungen für einzelne Typparameter ([Geben Sie die Einschränkungen für Typparameter](classes.md#type-parameter-constraints)) und kann nur angegeben werden, wenn eine *Type_parameter_ Liste* ebenfalls angegeben wird, und die Methode verfügt nicht über eine `override` Modifizierer.

Die *Return_type* und alle referenzierten Typen der *Formal_parameter_list* einer Methode muss mindestens dieselben zugriffsmöglichkeiten bieten wie die Methode selbst ([Barrierefreiheit Einschränkungen](basic-concepts.md#accessibility-constraints)).

Die *Method_body* ist entweder ein Semikolon, ein ***Anweisungstext*** oder ***ausdruckskörper***. Eine-Anweisungstext besteht aus einem *Block*, der angibt, dass der Anweisungen ausgeführt werden, wenn die Methode aufgerufen wird. Ein ausdruckskörper besteht aus `=>` gefolgt von einem *Ausdruck* und ein Semikolon, und gibt einen einzelnen Ausdruck ausführen, wenn die Methode aufgerufen wird. 

Für `abstract` und `extern` Methoden, die *Method_body* besteht einfach aus einem Semikolon. Für `partial` Methoden der *Method_body* kann entweder ein Semikolon, einem Blocktext oder einen ausdruckskörper bestehen. Für alle anderen Methoden die *Method_body* ist ein Blocktext oder einen ausdruckskörper.

Wenn die *Method_body* besteht aus einem Semikolon, und klicken Sie dann die Deklaration möglicherweise nicht enthalten. die `async` Modifizierer.

Der Name, der Liste der Typparameter und die Liste der formalen Parameter einer Methode definieren, die Signatur ([Signaturen und überladen](basic-concepts.md#signatures-and-overloading)) der Methode. Insbesondere besteht aus die Signatur einer Methode den Namen, die Anzahl der Parameter vom Typ und die Anzahl, Modifizierer, und Typen der formalen Parameter. Für diese Zwecke wird einen Typparameter der Methode, die in den Typ eines formalen Parameters auftritt, nicht mit dem Namen, aber anhand ihrer Ordnungsposition in der Liste der Typargumente der Methode identifiziert. Der Rückgabetyp ist nicht Teil der Signatur einer Methode, noch werden die Namen der Typparameter oder die formalen Parameter.

Der Name einer Methode muss die Namen aller anderen nicht--Methoden, in der gleichen Klasse deklariert unterscheiden. Darüber hinaus die Signatur einer Methode muss von den Signaturen aller anderen in derselben Klasse deklarierten Methoden unterscheiden, und zwei in der gleichen Klasse deklarierte Methoden dürfen keine Signaturen, die ausschließlich vom unterscheiden `ref` und `out`.

Der Methode *Type_parameter*sind im Gültigkeitsbereich der *Method_declaration*, und können verwendet werden, um die von Formulartypen in diesem Bereich in der gesamten *Return_type*, *Method_body*, und *Type_parameter_constraints_clause*s jedoch nicht in *Attribute*.

Alle formalen Parametern und Typparametern müssen unterschiedliche Namen aufweisen.

### <a name="method-parameters"></a>Methodenparameter

Die Parameter einer Methode, sofern vorhanden, werden von der Methode deklariert *Formal_parameter_list*.

```antlr
formal_parameter_list
    : fixed_parameters
    | fixed_parameters ',' parameter_array
    | parameter_array
    ;

fixed_parameters
    : fixed_parameter (',' fixed_parameter)*
    ;

fixed_parameter
    : attributes? parameter_modifier? type identifier default_argument?
    ;

default_argument
    : '=' expression
    ;

parameter_modifier
    : 'ref'
    | 'out'
    | 'this'
    ;

parameter_array
    : attributes? 'params' array_type identifier
    ;
```

Liste der formalen Parameter besteht aus einen oder mehrere durch Trennzeichen getrennte Parameter von denen nur die letzte möglicherweise eine *Parameter_array*.

Ein *Fixed_parameter* besteht aus einer optionalen Gruppe von *Attribute* ([Attribute](attributes.md)), ein optionales `ref`, `out` oder `this` Modifizierer ein *Typ*, *Bezeichner* und einem optionalen *Default_argument*. Jede *Fixed_parameter* deklariert einen Parameter des angegebenen Typs mit dem angegebenen Namen. Die `this` kennzeichnet die Methode als eine Erweiterungsmethode und ist nur für den ersten Parameter einer statischen Methode zulässig. Erweiterungsmethoden werden ausführlicher beschrieben [Erweiterungsmethoden](classes.md#extension-methods).

Ein *Fixed_parameter* mit einer *Default_argument* heißt ein ***Optionaler Parameter***, während eine *Fixed_parameter* ohne eine *Default_argument* ist eine ***Erforderlicher Parameter***. Ein erforderlicher Parameter möglicherweise nicht angezeigt, nachdem ein optionaler Parameter in einer *Formal_parameter_list*.

Ein `ref` oder `out` sind keine Parameter ein *Default_argument*. Die *Ausdruck* in einem *Default_argument* muss eine der folgenden sein:

*  a *constant_expression*
*  Ein Ausdruck der Form `new S()` , in denen `S` ist ein Werttyp.
*  Ein Ausdruck der Form `default(S)` , in denen `S` ist ein Werttyp.

Die *Ausdruck* muss implizit von einer Identität oder ein NULL-Werte zulassen Konvertierung in den Typ des Parameters konvertiert werden.

Wenn optionale Parameter in eine implementierende Deklaration der partiellen Methode auftreten ([partielle Methoden](classes.md#partial-methods)), eine explizite Schnittstellenmember-Implementierung ([explizite Implementierungen eines Schnittstellenmembers](interfaces.md#explicit-interface-member-implementations)) oder in einem einzigen Parameter Indexerdeklaration ([Indexer](classes.md#indexers)) der Compiler erhalten eine Warnung aus, da diese Member nicht auf eine Weise können, die Argumente aufgerufen werden, die ausgelassen werden können.

Ein *Parameter_array* besteht aus einer optionalen Gruppe von *Attribute* ([Attribute](attributes.md)), ein `params` Modifizierer eine *Array_type*, und ein *Bezeichner*. Ein Parameterarray deklariert einen einzelnen Parameter des Typs angegebenen Array mit dem angegebenen Namen. Die *Array_type* eines Parameters Arrays ein eindimensionales Array sein muss ([Arraytypen](arrays.md#array-types)). In einem Methodenaufruf einem Parameterarray können entweder ein einzelnes Argument des Typs angegebenen Array angegeben werden, oder er lässt NULL oder mehr Argumente den Elementtyp des Arrays angegeben werden. Parameter-Arrays werden ausführlich in [Parameterarrays](classes.md#parameter-arrays).

Ein *Parameter_array* nach einem optionalen Parameter auftreten können, jedoch keinen Standardwert – die Auslassung der Argumente für eine *Parameter_array* würde stattdessen bei der Erstellung eines leeren Arrays.

Das folgende Beispiel veranschaulicht verschiedene Arten von Parametern:
```csharp
public void M(
    ref int      i,
    decimal      d,
    bool         b = false,
    bool?        n = false,
    string       s = "Hello",
    object       o = null,
    T            t = default(T),
    params int[] a
) { }
```

In der *Formal_parameter_list* für `M`, `i` ist ein erforderliche Ref-Parameter, `d` Parameter ist ein erforderlicher Wert, `b`, `s`, `o` und `t` Optionale Parameter sind und `a` ein Parameterarray.

Deklaration einer Methode erstellt einen separaten Deklarationsabschnitt für Parameter "," Parameter vom Typ "und" lokale Variablen. Namen werden in diesen Deklarationsabschnitt eingeführt, durch die Liste der Typparameter und die Liste der formalen Parameter der Methode und Deklarationen von lokalen Variablen in der *Block* der Methode. Es ist ein Fehler bei zwei Mitgliedern eines Deklarationsabschnitts der Methode, die den gleichen Namen haben. Es ist ein Fehler für die Methode Deklaration und der Deklaration lokaler Variablen Speicherplatz eines geschachtelten Deklarationsabschnitts Elemente mit dem gleichen Namen enthalten.

Aufruf einer Methode ([Methodenaufrufe](expressions.md#method-invocations)) erstellt eine Kopie, die für diesen Aufruf spezifischen, weist der formalen Parameter und lokalen Variablen der Methode und der Argumentliste des Aufrufs, Werte oder Variablenverweise an das neu erstellte formalen Parameter. In der *Block* einer Methode, formalen Parameter verwiesen werden können, durch die IDs in *Simple_name* Ausdrücke ([einfache Namen](expressions.md#simple-names)).

Es gibt vier Arten der formalen Parameter:

*  Parameter, die ohne Modifizierer deklariert werden.
*  Verweisparameter, die mit deklariert werden die `ref` Modifizierer.
*  Output-Parameter, die mit deklariert werden die `out` Modifizierer.
*  Parameterarrays, die mit deklariert werden die `params` Modifizierer.

Siehe [Signaturen und überladen](basic-concepts.md#signatures-and-overloading), `ref` und `out` Modifizierer sind Teil der Signatur einer Methode, aber die `params` -Modifizierer ist nicht.

#### <a name="value-parameters"></a>Wert-Parametern

Ein Parameter mit dem keine Modifizierer deklariert ist eine Value-Parameter. Ein Wertparameter entspricht einer lokalen Variablen, die den Anfangswert von das entsprechende Argument im Aufruf Methode angegebenen erhält.

Wenn ein formaler Parameter ein Werteparameter ist, muss das entsprechende Argument in einen Methodenaufruf ein Ausdruck, der implizit konvertiert werden kann ([implizite Konvertierungen](conversions.md#implicit-conversions)) in den formalen Parametertyp.

Eine Methode ist zulässig, einen Wertparameter neue Werte zuweisen. Diese Zuweisungen wirken sich nur auf den Speicherort der lokalen Speicherressource, dargestellt durch den Wertparameter – sie haben keine Auswirkungen auf das tatsächliche Argument im Methodenaufruf angegeben.

#### <a name="reference-parameters"></a>Verweisparameter

Ein Parameter deklariert, mit einem `ref` Modifizierer ist ein Verweisparameter. Im Gegensatz zu einem Value-Parameter erstellt ein Verweisparameter nicht auf einen neuen Speicherort zu. Stattdessen stellt ein Verweisparameter am gleichen Speicherort wie die Variable als Argument im Methodenaufruf angegeben.

Wenn ein formaler Parameter einen Verweisparameter handelt, muss das entsprechende Argument in einen Methodenaufruf des Schlüsselworts bestehen `ref` gefolgt von einem *Variable_reference* ([präzise Regeln für die Bestimmung definitive Zuweisung](variables.md#precise-rules-for-determining-definite-assignment)) des gleichen Typs wie der formale Parameter. Eine Variable muss definitiv zugewiesen werden, bevor es als Verweisparameter übergeben werden kann.

Innerhalb einer Methode gilt ein Verweisparameter immer als definitiv zugewiesen.

Eine Methode, die als ein Iterator deklariert ([Iteratoren](classes.md#iterators)) keine Verweisparameter angeben.

Im Beispiel
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
        Console.WriteLine("i = {0}, j = {1}", i, j);
    }
}
```
erzeugt die Ausgabe
```
i = 2, j = 1
```

Für den Aufruf von `Swap` in `Main`, `x` stellt `i` und `y` stellt `j`. Der Aufruf ist daher die Auswirkungen der Tauschen der Werte der `i` und `j`.

In einer Methode, die mehrere Namen am gleichen Speicherort darstellen kann, Verweisparameter akzeptiert. Im Beispiel
```csharp
class A
{
    string s;

    void F(ref string a, ref string b) {
        s = "One";
        a = "Two";
        b = "Three";
    }

    void G() {
        F(ref s, ref s);
    }
}
```
der Aufruf von `F` in `G` übergibt einen Verweis auf `s` für beide `a` und `b`. Daher ist es bei diesen Aufruf, der die Namen `s`, `a`, und `b` verweisen alle auf den gleichen Speicherort aus, und die drei alle Zuweisungen Ändern des Instanzfelds `s`.

#### <a name="output-parameters"></a>Output-Parameter

Ein Parameter deklariert, mit einem `out` Modifizierer ist ein Output-Parameter. Ein Verweisparameter ähnlich, ist ein Output-Parameter keinen neuen Speicherort erstellt werden. Stattdessen stellt ein Output-Parameter am gleichen Speicherort wie die Variable als Argument im Methodenaufruf angegeben.

Wenn ein formaler Parameter ein Ausgabeparameter ist, muss das entsprechende Argument in einen Methodenaufruf des Schlüsselworts bestehen `out` gefolgt von einem *Variable_reference* ([präzise Regeln für die Bestimmung definitive Zuweisung](variables.md#precise-rules-for-determining-definite-assignment)) des gleichen Typs wie der formale Parameter. Eine Variable muss nicht definitiv zugewiesen werden, bevor es als Output-Parameter übergeben werden kann, aber nach einem Aufruf, in dem eine Variable als Output-Parameter übergeben wurde, wird die Variable definitiv zugewiesen betrachtet.

Innerhalb einer Methode, so wie eine lokale Variable, ein Output-Parameter zunächst gilt als nicht zugewiesene und müssen definitiv zugewiesen werden, bevor der Wert verwendet wird.

Alle Ausgabeparameter einer Methode muss definitiv zugewiesen werden, bevor die Methode zurückgibt.

Eine Methode, die als partielle Methode deklariert ([partielle Methoden](classes.md#partial-methods)) oder eines Iterators ([Iteratoren](classes.md#iterators)) können keine Ausgabeparameter enthalten haben.

Output-Parameter werden in der Regel in Methoden verwendet, die Rückgabe mehrerer Werte zu erzeugen. Zum Beispiel:
```csharp
using System;

class Test
{
    static void SplitPath(string path, out string dir, out string name) {
        int i = path.Length;
        while (i > 0) {
            char ch = path[i - 1];
            if (ch == '\\' || ch == '/' || ch == ':') break;
            i--;
        }
        dir = path.Substring(0, i);
        name = path.Substring(i);
    }

    static void Main() {
        string dir, name;
        SplitPath("c:\\Windows\\System\\hello.txt", out dir, out name);
        Console.WriteLine(dir);
        Console.WriteLine(name);
    }
}
```

Das Beispiel erzeugt die Ausgabe:
```
c:\Windows\System\
hello.txt
```

Beachten Sie, dass die `dir` und `name` Variablen können nicht zugewiesen werden, vor der Übergabe an `SplitPath`, und dass sie als definitiv zugewiesen, nach dem Aufruf angesehen werden.

#### <a name="parameter-arrays"></a>Parameterarrays

Ein Parameter deklariert, mit einem `params` Modifizierer ist ein Parameterarray. Wenn eine Liste formaler Parameter ein Parameterarray enthält, muss der letzte Parameter in der Liste sein, und es muss ein eindimensionales Array-Typ aufweisen. Beispielsweise die Typen `string[]` und `string[][]` kann verwendet werden, wie der Typ eines Parameterarrays, aber der Typ `string[,]` nicht können. Es ist nicht möglich, zum Kombinieren der `params` Modifizierer mit den Modifizierern `ref` und `out`.

Ein Parameterarray kann die Argumente in eine von zwei Arten in einem Methodenaufruf angegeben werden:

*  Das Argument angegeben wird, für ein Parameterarray kann ein einzelner Ausdruck sein, die implizit konvertiert werden kann ([implizite Konvertierungen](conversions.md#implicit-conversions)) in den Parameter-Arraytyp. In diesem Fall verhält sich das Parameterarray genau wie ein Value-Parameter.
*  Der Aufruf kann auch angeben, NULL oder mehr Argumente für das Parameterarray, wobei jedes Argument ist ein Ausdruck, der implizit konvertiert werden kann ([implizite Konvertierungen](conversions.md#implicit-conversions)), die den Elementtyp des Parameterarrays. In diesem Fall der Aufruf erstellt eine Instanz des Parameterarraytyps mit einer Länge, die Anzahl von Argumenten entspricht, initialisiert die Elemente der Arrayinstanz mit den Werten des angegebenen Arguments und verwendet die neu erstellte Array-Instanz als die tatsächliche Argument.

Mit Ausnahme von ermöglichen, dass eine Variable Anzahl von Argumenten in einen Aufruf, ein Parameterarray entspricht exakt dem Value-Parameter ([-Wertparameter](classes.md#value-parameters)) des gleichen Typs.

Im Beispiel
```csharp
using System;

class Test
{
    static void F(params int[] args) {
        Console.Write("Array contains {0} elements:", args.Length);
        foreach (int i in args) 
            Console.Write(" {0}", i);
        Console.WriteLine();
    }

    static void Main() {
        int[] arr = {1, 2, 3};
        F(arr);
        F(10, 20, 30, 40);
        F();
    }
}
```
erzeugt die Ausgabe
```
Array contains 3 elements: 1 2 3
Array contains 4 elements: 10 20 30 40
Array contains 0 elements:
```

Der erste Aufruf der `F` einfach das Array übergibt `a` als ein Value-Parameter. Der zweite Aufruf von `F` erstellt automatisch eine vier-Elemente `int[]` mit dem angegebenen Elementwerte und übergibt, die array-Instanz als ein Value-Parameter. Entsprechend der dritte Aufruf von `F` wird ein NULL-Element erstellt `int[]` und übergibt diese Instanz als ein Value-Parameter. Die zweite und dritte Aufrufe sind wie folgt:
```csharp
F(new int[] {10, 20, 30, 40});
F(new int[] {});
```

Bei der Auflösung von funktionsüberladungen durchführen zu können, eine Methode mit einem Parameterarray möglicherweise auf, in seiner normalen Form oder in der erweiterten Form ([Anwendbarer Funktionsmember](expressions.md#applicable-function-member)). Die erweiterte Form von einer Methode ist nur dann, wenn die normale Form der Methode nicht anwendbar ist, und nur dann, wenn eine entsprechende Methode mit der gleichen Signatur wie die erweiterte Form noch nicht in denselben Typ deklariert ist verfügbar.

Im Beispiel
```csharp
using System;

class Test
{
    static void F(params object[] a) {
        Console.WriteLine("F(object[])");
    }

    static void F() {
        Console.WriteLine("F()");
    }

    static void F(object a0, object a1) {
        Console.WriteLine("F(object,object)");
    }

    static void Main() {
        F();
        F(1);
        F(1, 2);
        F(1, 2, 3);
        F(1, 2, 3, 4);
    }
}
```
erzeugt die Ausgabe
```
F();
F(object[]);
F(object,object);
F(object[]);
F(object[]);
```

Im Beispiel sind zwei der möglichen erweiterten Formulare der Methode mit einem Parameterarray bereits in der Klasse als reguläre Methoden enthalten. Diese erweiterten datenformen sind aus diesem Grund nicht berücksichtigt, wenn die Auflösung von funktionsüberladungen ausführen, und wählen Sie die erste und dritte Methodenaufrufe daher die regulären Methoden. Wenn eine Klasse eine Methode mit einem Parameterarray deklariert, ist es nicht ungewöhnlich, dass auch einige der erweiterten Formen als reguläre Methoden eingeschlossen werden sollen. -Instanz, die bei der erweiterten Zustand einer Methode mit einem Parameterarray tritt auf, wird aufgerufen, wodurch es möglich ist, die die Zuordnung eines Arrays zu vermeiden.

Wenn der Typ, der ein Parameterarray ist `object[]`, eine mögliche Mehrdeutigkeit zwischen die normale Form der Methode und der erweiterten Form für ein einzelnes entsteht `object` Parameter. Der Grund für die Mehrdeutigkeit ist, dass ein `object[]` ist selbst in den Typ implizit `object`. Die Mehrdeutigkeit stellt kein Problem, jedoch ein, da er aufgelöst werden kann, indem Sie eine Umwandlung einfügen, bei Bedarf.

Im Beispiel
```csharp
using System;

class Test
{
    static void F(params object[] args) {
        foreach (object o in args) {
            Console.Write(o.GetType().FullName);
            Console.Write(" ");
        }
        Console.WriteLine();
    }

    static void Main() {
        object[] a = {1, "Hello", 123.456};
        object o = a;
        F(a);
        F((object)a);
        F(o);
        F((object[])o);
    }
}
```
erzeugt die Ausgabe
```
System.Int32 System.String System.Double
System.Object[]
System.Object[]
System.Int32 System.String System.Double
```

In der ersten und letzten Aufrufen von `F`, die normale Form von `F` ist anwendbar, da eine implizite Konvertierung des Argumenttyps in den Parametertyp vorhanden ist (beide sind vom Typ `object[]`). Daher wählt die überladungsauflösung die normale Form von `F`, und das Argument wird als reguläre Werteparameter übergeben. In der zweiten und dritten aufrufen, die normale Form von `F` ist nicht anwendbar, da keine implizite Konvertierung des Argumenttyps in den Parametertyp vorhanden ist (Typ `object` kann nicht implizit konvertiert Typ `object[]`). Allerdings die erweiterte Form von `F` gilt, damit es von der Auflösung von funktionsüberladungen aktiviert ist. Als Ergebnis einem Element `object[]` wird durch den Aufruf erstellt und das einzige Element des Arrays mit dem angegebenen Argumentwert initialisiert wird (die selbst ist ein Verweis auf ein `object[]`).

### <a name="static-and-instance-methods"></a>Statische Methoden und Instanzmethoden

Wenn die Deklaration einer Methode enthält einen `static` -Modifizierer ist, wird diese Methode wird als eine statische Methode sein. Wenn kein `static` Modifizierer vorhanden ist, wird die Methode wird als eine Instanzmethode sein.

Eine statische Methode in einer bestimmten Instanz kann nicht ausgeführt werden, und es ist ein Fehler während der Kompilierung zum Verweisen auf `this` in einer statischen Methode.

Eine Instanzmethode funktioniert für eine bestimmte Instanz einer Klasse, und diese Instanz zugegriffen werden kann, als `this` ([diesen Zugriff](expressions.md#this-access)).

Wenn eine Methode verwiesen wird, einem *Member_access* ([Memberzugriff](expressions.md#member-access)) des Formulars `E.M`, wenn `M` ist eine statische Methode, `E` muss einen Typ mit deuten`M`, und wenn `M` ist eine Instanzmethode `E` muss eine Instanz von einem Typ mit deuten `M`.

Die Unterschiede zwischen statischen und Instanzmember finden Sie weiter unten in [statische und Instanzmember](classes.md#static-and-instance-members).

### <a name="virtual-methods"></a>Virtuelle Methoden

Wenn eine Instanzmethodendeklaration enthält eine `virtual` -Modifizierer ist, wird diese Methode wird als eine virtuelle Methode sein. Wenn kein `virtual` Modifizierer vorhanden ist, wird die Methode ist eine nicht virtuelle Methode bezeichnet.

Die Implementierung einer nicht virtuellen Methode ist unveränderlich: Die Implementierung ist identisch, ob die Methode, auf einer Instanz der Klasse in aufgerufen wird der sie deklariert ist, oder eine Instanz einer abgeleiteten Klasse. Im Gegensatz dazu kann die Implementierung einer virtuellen Methode durch abgeleitete Klassen abgelöst werden. Der Prozess der Implementierung einer geerbten virtuellen Methode heißt ***überschreiben*** diese Methode ([Methoden außer Kraft setzen](classes.md#override-methods)).

Im Aufruf einer virtuellen Methode die ***Laufzeittyp*** der Instanz, die diesen Aufruf benötigt, direkten legt fest, die Implementierung der tatsächlichen Methode aufrufen. Im Aufruf einer nicht virtuellen Methode die ***Kompilierzeittyp*** der Instanz ist der bestimmende Faktor. Genau bedeutet dies, wenn eine Methode namens `N` wird aufgerufen, mit einer Argumentliste `A` auf einer Instanz mit einem Typ während der Kompilierung `C` und einen Laufzeittyp `R` (, in dem `R` ist entweder `C` oder eine abgeleitete Klasse von `C`), der Aufruf wird wie folgt verarbeitet:

*  Zunächst wird die Auflösung von funktionsüberladungen auf angewendet `C`, `N`, und `A`, auswählen eine bestimmte Methode `M` aus dem Satz von Methoden deklariert "und" geerbt `C`. Finden Sie im [Methodenaufrufe](expressions.md#method-invocations).
*  Wenn danach `M` ist eine nicht virtuelle Methode, `M` aufgerufen wird.
*  Andernfalls `M` ist eine virtuelle Methode und der am weitesten abgeleiteten Implementierung der `M` in Bezug auf `R` aufgerufen wird.

Für jede virtuelle Methode von einer Klasse deklariert oder geerbt, vorhanden ist eine ***am weitesten abgeleiteten Implementierung*** der Methode in Bezug auf die Klasse. Die am weitesten abgeleiteten Implementierung einer virtuellen Methode `M` in Bezug auf eine Klasse `R` wird wie folgt bestimmt:

*  Wenn `R` enthält, die Einführung von `virtual` Deklaration `M`, ist dies der am weitesten abgeleiteten Implementierung der `M`.
*  Andernfalls gilt: Wenn `R` enthält ein `override` von `M`, ist dies der am weitesten abgeleiteten Implementierung der `M`.
*  Andernfalls am meisten Implementierung der abgeleiteten der `M` in Bezug auf `R` ist identisch mit den am weitesten abgeleiteten Implementierung der `M` in Bezug auf die direkte Basisklasse von `R`.

Im folgende Beispiel werden die Unterschiede zwischen virtuellen und nicht virtuelle Methoden veranschaulicht:
```csharp
using System;

class A
{
    public void F() { Console.WriteLine("A.F"); }

    public virtual void G() { Console.WriteLine("A.G"); }
}

class B: A
{
    new public void F() { Console.WriteLine("B.F"); }

    public override void G() { Console.WriteLine("B.G"); }
}

class Test
{
    static void Main() {
        B b = new B();
        A a = b;
        a.F();
        b.F();
        a.G();
        b.G();
    }
}
```

Im Beispiel `A` führt eine nicht virtuelle Methode `F` und eine virtuelle Methode `G`. Die Klasse `B` führt eine neue nicht virtuelle Methode `F`, daher durch das Ausblenden der geerbten `F`, und außerdem überschreibt die geerbte Methode `G`. Das Beispiel erzeugt die Ausgabe:
```
A.F
B.F
B.G
B.G
```

Beachten Sie, dass die Anweisung `a.G()` ruft `B.G`, nicht `A.G`. Dies ist, da der Laufzeittyp der Instanz (d.h. `B`), nicht der Kompilierzeit-Typ der Instanz (d.h. `A`), bestimmt die Implementierung der tatsächlichen Methode zum Aufrufen.

Da die Methoden zum Ausblenden von geerbter Methoden zulässig sind, ist es möglich, eine Klasse kann mehrere virtuelle Methoden mit derselben Signatur enthalten. Dies ist keiner Mehrdeutigkeiten, da alle bis auf die am häufigsten abgerufene Methode ausgeblendet sind. Im Beispiel
```csharp
using System;

class A
{
    public virtual void F() { Console.WriteLine("A.F"); }
}

class B: A
{
    public override void F() { Console.WriteLine("B.F"); }
}

class C: B
{
    new public virtual void F() { Console.WriteLine("C.F"); }
}

class D: C
{
    public override void F() { Console.WriteLine("D.F"); }
}

class Test
{
    static void Main() {
        D d = new D();
        A a = d;
        B b = d;
        C c = d;
        a.F();
        b.F();
        c.F();
        d.F();
    }
}
```
die `C` und `D` Klassen enthalten, zwei virtuelle Methoden, mit der gleichen Signatur: Der eingeschleuste `A` und der eingeschleuste `C`. Die Methode, die von eingeführte `C` Blendet die geerbte Methode `A`. Daher die außer Kraft setzen-Deklaration in `D` überschreibt die Methode, die von eingeführt `C`, und es ist nicht möglich, dass `D` , die Methode eingeführt, durch Überschreiben `A`. Das Beispiel erzeugt die Ausgabe:
```
B.F
B.F
D.F
D.F
```

Beachten Sie, dass es möglich ist, rufen Sie die virtuelle Methode die ausgeblendete durch den Zugriff auf eine Instanz von `D` über einen weniger abgeleiteten Typ, die in der die Methode nicht ausgeblendet ist.

### <a name="override-methods"></a>Methoden außer Kraft setzen

Wenn eine Instanzmethodendeklaration enthält ein `override` Modifizierer, die Methode gilt eine ***Überschreibungsmethode***. Eine Überschreibungsmethode überschreibt eine geerbte virtuelle Methode mit der gleichen Signatur. Während eine Deklaration einer virtuellen Methode eine neue Methode einführt, spezialisiert eine Deklaration einer überschriebenen Methode eine vorhandene geerbte virtuelle Methode, indem eine neue Implementierung dieser Methode bereitgestellt wird.

Die Methode überschrieben, indem ein `override` Deklaration wird als bezeichnet die ***Basismethode überschreiben***. Für eine Überschreibungsmethode `M` in einer Klasse deklarierten `C`, die überschriebene Basismethode richtet sich nach der Untersuchung jeder Typ von der Basisklasse `C`, beginnend mit dem direkten Basisklasse `C` und mit jedem aufeinander folgenden direkte Basisklasse-Typ, bis in einem angegebenen Basisklassentyp mindestens eine zugängliche Methode ist die befindet, hat die gleiche Signatur wie `M` nach dem Ersetzen der Typargumente. Im Rahmen, suchen die überschriebene Basismethode, eine Methode wird als verfügbar betrachtet, ist dies `public`, sofern sie `protected`, sofern sie `protected internal`, oder es ist `internal` und im selben Programm als deklariert `C`.

Ein Fehler während der Kompilierung tritt auf, es sei denn, alle der folgenden "true" für eine Deklaration außer Kraft setzen:

*  Eine überschriebene Basismethode kann gefunden werden, wie oben beschrieben werden.
*  Es gibt genau einen solchen überschriebene Basismethode. Diese Einschränkung wirkt sich nur, wenn Sie der Basisklassentyp ein konstruierter Typ ist, macht die Ersetzung der Argumente des Typs der Signatur der beiden Methoden identisch.
*  Die überschriebene Basismethode ist einer virtuell, abstrakt oder Überschreiben der Methode. Die überschriebene Basismethode darf nicht in anderen Worten: statisch oder nicht virtuell sein.
*  Die überschriebene Basismethode ist keine versiegelten Methode.
*  Die Methode zum Überschreiben und die überschriebene Basismethode haben den gleichen Rückgabetyp auf.
*  Die Deklaration außer Kraft setzen und die überschriebene Basismethode haben die gleiche deklarierte Zugriffsart auf. Eine außer Kraft setzen-Deklaration kann nicht in anderen Worten, den Zugriff auf die virtuelle Methode ändern. Allerdings muss, wenn die überschriebene Basismethode wird intern geschützt, und sie in einer anderen Assembly deklariert wird, als die Assembly mit der Methode außer Kraft setzen und dann der Überschreibungsmethode deklariert Zugriff geschützt werden.
*  Die außer Kraft setzen-Deklaration ist nicht mit Typ-Parameter-Einschränkungen-Klauseln angegeben. Stattdessen werden die Einschränkungen von die überschriebene Basismethode geerbt. Beachten Sie, dass die Einschränkungen, die Typparameter in der überschriebenen Methode sind in der geerbten Einschränkung durch Typargumente ersetzt werden können. Dies kann zu Einschränkungen führen, die nicht gültig, wenn explizit angegeben, z. B. Werttypen oder versiegelte Typen sind.

Das folgende Beispiel zeigt, wie die überschreibenden Regeln für generische Klassen funktionieren:
```csharp
abstract class C<T>
{
    public virtual T F() {...}
    public virtual C<T> G() {...}
    public virtual void H(C<T> x) {...}
}

class D: C<string>
{
    public override string F() {...}            // Ok
    public override C<string> G() {...}         // Ok
    public override void H(C<T> x) {...}        // Error, should be C<string>
}

class E<T,U>: C<U>
{
    public override U F() {...}                 // Ok
    public override C<U> G() {...}              // Ok
    public override void H(C<T> x) {...}        // Error, should be C<U>
}
```

Eine Deklaration für die Außerkraftsetzung stehen die überschriebene Basismethode mithilfe einer *Base_access* ([Base Access](expressions.md#base-access)). Im Beispiel
```csharp
class A
{
    int x;

    public virtual void PrintFields() {
        Console.WriteLine("x = {0}", x);
    }
}

class B: A
{
    int y;

    public override void PrintFields() {
        base.PrintFields();
        Console.WriteLine("y = {0}", y);
    }
}
```
die `base.PrintFields()` Aufruf im `B` Ruft die `PrintFields` Methode deklariert `A`. Ein *Base_access* den virtuellen Aufruf-Mechanismus deaktiviert und einfach die Basismethode behandelt, als eine nicht virtuelle Methode. Hatte den Aufruf im `B` wurde geschrieben `((A)this).PrintFields()`, wäre Sie rekursiv Aufrufen der `PrintFields` Methode deklariert werden, `B`, nicht zu derjenigen, die in deklariert `A`, da `PrintFields` ist virtuell und den Laufzeittyp der `((A)this)` ist `B`.

Nur durch Einfügen einer `override` Modifizierer kann eine Methode eine andere Methode zu überschreiben. In allen anderen Fällen wird eine Methode mit der gleichen Signatur wie eine geerbte Methode einfach die geerbte Methode ausgeblendet. Im Beispiel
```csharp
class A
{
    public virtual void F() {}
}

class B: A
{
    public virtual void F() {}        // Warning, hiding inherited F()
}
```
die `F` -Methode in der `B` enthält kein `override` Modifizierer und wird daher nicht überschrieben der `F` -Methode in der `A`. Vielmehr die `F` -Methode in der `B` Blendet die Methode in `A`, und eine Warnung wird ausgegeben, da die Deklaration keine umfasst eine `new` Modifizierer.

Im Beispiel
```csharp
class A
{
    public virtual void F() {}
}

class B: A
{
    new private void F() {}        // Hides A.F within body of B
}

class C: B
{
    public override void F() {}    // Ok, overrides A.F
}
```
die `F` -Methode in der `B` verbirgt den virtuellen `F` Methode geerbt von `A`. Das neue `F` in `B` privaten Zugriff, der Bereich enthält nur die Klassendefinition von `B` und erstreckt sich nicht um `C`. Daher ist die Deklaration der `F` in `C` ist zulässig, außer Kraft setzen der `F` vererbt `A`.

### <a name="sealed-methods"></a>Versiegelte Methoden

Wenn eine Instanzmethodendeklaration enthält eine `sealed` -Modifizierer ist, wird diese Methode wird als bezeichnet ein ***versiegelt Methode***. Wenn eine Instanzmethodendeklaration enthält die `sealed` Modifizierer verwenden, muss auch enthalten die `override` Modifizierer. Verwenden der `sealed` -Modifizierer wird verhindert, dass eine abgeleitete Klasse weiter durch Überschreiben der Methode.

Im Beispiel
```csharp
using System;

class A
{
    public virtual void F() {
        Console.WriteLine("A.F");
    }

    public virtual void G() {
        Console.WriteLine("A.G");
    }
}

class B: A
{
    sealed override public void F() {
        Console.WriteLine("B.F");
    } 

    override public void G() {
        Console.WriteLine("B.G");
    } 
}

class C: B
{
    override public void G() {
        Console.WriteLine("C.G");
    } 
}
```
die Klasse `B` bietet zwei Methoden außer Kraft setzen: eine `F` Methode, die `sealed` Modifizierer und eine `G` -Methode, die nicht der Fall ist. `B`die Nutzung von der versiegelten `modifier` wird verhindert, dass `C` weiter überschreiben `F`.

### <a name="abstract-methods"></a>Abstrakte Methoden

Wenn eine Instanzmethodendeklaration enthält ein `abstract` -Modifizierer ist, wird diese Methode wird als bezeichnet ein ***abstrakte Methode***. Obwohl eine abstrakte Methode implizit auch eine virtuelle Methode ist, können keine Modifizierer besitzen `virtual`.

Eine abstrakte Methodendeklaration führt eine neue virtuelle Methode, aber es bietet eine Implementierung dieser Methode keine. Stattdessen müssen nicht abstrakte abgeleitete Klassen ihre eigene Implementierung bereitstellen, indem Sie diese Methode überschreiben. Da eine abstrakte Methode keine Implementierungen, bietet der *Method_body* einer abstrakten Methode einfach besteht aus einem Semikolon.

Abstrakte Methodendeklarationen sind nur in abstrakten Klassen zulässig ([abstrakte Klassen](classes.md#abstract-classes)).

Im Beispiel
```csharp
public abstract class Shape
{
    public abstract void Paint(Graphics g, Rectangle r);
}

public class Ellipse: Shape
{
    public override void Paint(Graphics g, Rectangle r) {
        g.DrawEllipse(r);
    }
}

public class Box: Shape
{
    public override void Paint(Graphics g, Rectangle r) {
        g.DrawRect(r);
    }
}
```
die `Shape` -Klasse definiert die abstrakte Vorstellung einer geometrischen Formobjekt, das sich selbst zu zeichnen kann. Die `Paint` Methode ist abstrakt, da es keine sinnvolle Standardimplementierung ist. Die `Ellipse` und `Box` Klassen sind konkrete `Shape` Implementierungen. Da diese Klassen abstrakt sind, müssen sie außer Kraft setzen der `Paint` Methode und eine tatsächliche Implementierung bereitstellen.

Es ist ein Fehler während der Kompilierung für eine *Base_access* ([Base Access](expressions.md#base-access)) auf eine abstrakte Methode zu verweisen. Im Beispiel
```csharp
abstract class A
{
    public abstract void F();
}

class B: A
{
    public override void F() {
        base.F();                        // Error, base.F is abstract
    }
}
```
ein Kompilierung ein Fehler wird gemeldet, für die `base.F()` aufrufen, da es sich um eine abstrakte Methode verweist.

Eine abstrakte Methodendeklaration ist zulässig, um eine virtuelle Methode zu überschreiben. Dadurch können eine abstrakte Klasse, die erneute Implementierung der Methode in abgeleiteten Klassen zu erzwingen, und die ursprüngliche Implementierung der Methode nicht verfügbar. Im Beispiel
```csharp
using System;

class A
{
    public virtual void F() {
        Console.WriteLine("A.F");
    }
}

abstract class B: A
{
    public abstract override void F();
}

class C: B
{
    public override void F() {
        Console.WriteLine("C.F");
    }
}
```
Klasse `A` deklariert eine virtuelle Methode, Klasse `B` überschreibt diese Methode mit dem eine abstrakte Methode, und die Klasse `C` überschreibt die abstrakte Methode, um eine eigene Implementierung bereitzustellen.

### <a name="external-methods"></a>Externe Methoden

Wenn eine Methodendeklaration enthält ein `extern` -Modifizierer ist, wird diese Methode wird als bezeichnet ein ***externe Methode***. Externe Methoden werden extern implementiert, in der Regel mit einer anderen Sprache als C# -Code. Da eine externe Methodendeklaration keine Implementierungen, bietet der *Method_body* besteht aus einer externen Methode einfach aus einem Semikolon. Eine externe Methode kann nicht generisch sein.

Die `extern` Modifizierer wird normalerweise verwendet, in Verbindung mit einer `DllImport` Attribut ([Interoperation mit COM- und Win32-Komponenten](attributes.md#interoperation-with-com-and-win32-components)), können externe Methoden von DLLs (Dynamic Link Libraries) implementiert werden. Die ausführungsumgebung möglicherweise andere Mechanismen unterstützt durch Implementierungen der externe Methoden bereitgestellt werden kann.

Wenn eine externe Methode enthält einen `DllImport` -Attribut, die Deklaration der Methode muss auch enthalten eine `static` Modifizierer. Dieses Beispiel veranschaulicht die Verwendung von der `extern` Modifizierer und der `DllImport` Attribut:
```csharp
using System.Text;
using System.Security.Permissions;
using System.Runtime.InteropServices;

class Path
{
    [DllImport("kernel32", SetLastError=true)]
    static extern bool CreateDirectory(string name, SecurityAttribute sa);

    [DllImport("kernel32", SetLastError=true)]
    static extern bool RemoveDirectory(string name);

    [DllImport("kernel32", SetLastError=true)]
    static extern int GetCurrentDirectory(int bufSize, StringBuilder buf);

    [DllImport("kernel32", SetLastError=true)]
    static extern bool SetCurrentDirectory(string name);
}
```

### <a name="partial-methods-recap"></a>Partielle Methoden (Zusammenfassung)

Wenn die Deklaration einer Methode enthält einen `partial` -Modifizierer ist, wird diese Methode wird als bezeichnet ein ***partielle Methode***. Partielle Methoden können nur Mitglieder der partiellen Typen deklariert werden ([partielle Typen](classes.md#partial-types)), und eine Reihe von Einschränkungen unterliegen. Partielle Methoden ausführlicher beschrieben werden [partielle Methoden](classes.md#partial-methods).

### <a name="extension-methods"></a>Erweiterungsmethoden

Wenn der erste Parameter einer Methode enthält die `this` -Modifizierer ist, wird diese Methode wird als bezeichnet ein ***Erweiterungsmethode***. Erweiterungsmethoden können nur in nicht-generische, nicht geschachtelten statischen Klassen deklariert werden. Der erste Parameter einer Erweiterungsmethode haben keine Modifizierer außer `this`, und der Parametertyp kann kein Zeigertyp sein.

Im folgenden finden ein Beispiel für eine statische Klasse, die zwei Erweiterungsmethoden deklariert:
```csharp
public static class Extensions
{
    public static int ToInt32(this string s) {
        return Int32.Parse(s);
    }

    public static T[] Slice<T>(this T[] source, int index, int count) {
        if (index < 0 || count < 0 || source.Length - index < count)
            throw new ArgumentException();
        T[] result = new T[count];
        Array.Copy(source, index, result, 0, count);
        return result;
    }
}
```

Eine Erweiterungsmethode ist eine normale statische Methode. Darüber hinaus im Gültigkeitsbereich der einschließenden statische Klasse ist, eine Erweiterungsmethode kann aufgerufen werden mithilfe einer Instanzmethodensyntax Aufruf ([Erweiterung Methodenaufrufe](expressions.md#extension-method-invocations)), mit dem Empfänger-Ausdruck als erstes Argument.

Das folgende Programm verwendet die Erweiterungsmethoden, die oben deklariert:
```csharp
static class Program
{
    static void Main() {
        string[] strings = { "1", "22", "333", "4444" };
        foreach (string s in strings.Slice(1, 2)) {
            Console.WriteLine(s.ToInt32());
        }
    }
}
```

Die `Slice` Methode steht für die `string[]`, und die `ToInt32` Methode steht für `string`, da sie als Erweiterungsmethoden deklariert wurden. Die Bedeutung des Programms ist identisch mit den folgenden, wobei normale statische Methodenaufrufen:
```csharp
static class Program
{
    static void Main() {
        string[] strings = { "1", "22", "333", "4444" };
        foreach (string s in Extensions.Slice(strings, 1, 2)) {
            Console.WriteLine(Extensions.ToInt32(s));
        }
    }
}
```

### <a name="method-body"></a>Methodentext

Die *Method_body* Deklaration einer Methode besteht aus einer Blocktext, einen ausdruckskörper oder ein Semikolon.

Die ***Ergebnistyp*** einer Methode ist `void` ist der Rückgabetyp `void`, oder wenn die Methode asynchron ist und der Rückgabetyp ist `System.Threading.Tasks.Task`. Andernfalls ist der Ergebnistyp einer nicht-Async-Methode, der Rückgabetyp und dem Rückgabetyp einer Async-Methode mit Rückgabetyp `System.Threading.Tasks.Task<T>` ist `T`.

Wenn eine Methode verfügt über eine `void` dazu führen, Typ und einem Blocktext `return` Anweisungen ([die return-Anweisung](statements.md#the-return-statement)) im-Block dürfen nicht auf einen Ausdruck angeben. Wenn die Ausführung des Blocks einer "void" Methode normal abgeschlossen wird (d. h. Kontrollflüsse am Ende des Methodentexts), dass die Methode einfach an den aktuellen Aufrufer zurückgibt.
    
Wenn eine Methode verfügt über eine `void` Ergebnis und einen ausdruckskörper, den Ausdruck `E` muss eine *Statement_expression*, und der Text entspricht genau einen Blocktext des Formulars `{ E; }`.
    
Wenn eine Methode einen für nicht-Void-Ergebnistyp und einem Block Anforderungstext, jede `return` -Anweisung in den Block muss einen Ausdruck, der implizit in den Ergebnistyp konvertiert werden angeben. Der Endpunkt eines Block-Texts, der eine Methode einen Wert zurückgibt, darf nicht erreichbar sein. Das heißt, in einer Methode Wertrückgabe mit einem Blocktext von Steuerelement darf nicht über das Ende des Methodentexts hinausgehen.
    
Wenn eine Methode ein für nicht-Void-Ergebnistyp und einen ausdruckskörper hat, der Ausdruck implizit in den Ergebnistyp konvertiert werden muss und der Text genau einen Blocktext des Formulars entspricht `{ return E; }`.
    
Im Beispiel
```csharp
class A
{
    public int F() {}            // Error, return value required

    public int G() {
        return 1;
    }

    public int H(bool b) {
        if (b) {
            return 1;
        }
        else {
            return 0;
        }
    }

    public int I(bool b) => b ? 1 : 0;
}
```
die Wertrückgabe `F` Methode führt ein Fehler während der Kompilierung, da das Ende des Methodentexts ablaufsteuerung können. Die `G` und `H` Methoden richtig sind, da sämtliche möglichen ausführungswege eine return-Anweisung enden, der angibt, einen Wert zurückgegeben. Die `I` Methode korrekt ist, da Text einen Anweisungsblock mit nur eine einzelne rückgabeanweisung darin entspricht.

### <a name="method-overloading"></a>Methodenüberladung

Die Regeln der überladungsauflösung Methode werden in beschrieben [Typrückschluss](expressions.md#type-inference).

## <a name="properties"></a>Eigenschaften

Ein ***Eigenschaft*** ist ein Element, das Zugriff auf ein Merkmal eines Objekts oder einer Klasse bereitstellt. Beispiele für Eigenschaften enthalten die Länge einer Zeichenfolge, die Größe einer Schriftart, die Titelleiste eines Fensters, den Namen eines Kunden und so weiter. Eigenschaften sind eine natürliche Erweiterung von Feldern, beide sind benannte Member mit zugeordneten Typen, und die Syntax für den Zugriff auf Felder und Eigenschaften entspricht. Im Gegensatz zu Feldern bezeichnen Eigenschaften jedoch keine Speicherorte. Stattdessen verfügen Eigenschaften über ***Accessors*** zum Angeben der Anweisungen, die beim Lesen oder Schreiben ihrer Werte ausgeführt werden sollen. Eigenschaften stellen somit einen Mechanismus zum Zuordnen von Aktionen mit dem Lesen und Schreiben der Attribute eines Objekts bereit; Außerdem ermöglichen sie solche Attribute berechnet werden soll.

Eigenschaften werden deklariert, die mit *Property_declaration*s:

```antlr
property_declaration
    : attributes? property_modifier* type member_name property_body
    ;

property_modifier
    : 'new'
    | 'public'
    | 'protected'
    | 'internal'
    | 'private'
    | 'static'
    | 'virtual'
    | 'sealed'
    | 'override'
    | 'abstract'
    | 'extern'
    | property_modifier_unsafe
    ;

property_body
    : '{' accessor_declarations '}' property_initializer?
    | '=>' expression ';'
    ;

property_initializer
    : '=' variable_initializer ';'
    ;
```

Ein *Property_declaration* eventuell einen Satz von *Attribute* ([Attribute](attributes.md)) und eine gültige Kombination der vier Zugriffsmodifizierer ([Zugriffsmodifizierer ](classes.md#access-modifiers)), wird die `new` ([der new-Modifizierer](classes.md#the-new-modifier)), `static` ([statische und Instanzmethoden](classes.md#static-and-instance-methods)), `virtual` ([virtuelle Methoden](classes.md#virtual-methods)), `override` ([Methoden außer Kraft setzen](classes.md#override-methods)), `sealed` ([Versiegelte Methoden](classes.md#sealed-methods)), `abstract` ([abstrakten Methoden](classes.md#abstract-methods)), und `extern` ([Externe Methoden](classes.md#external-methods)) Modifizierer.

Eigenschaftendeklarationen gelten dieselben Regeln wie Methodendeklarationen ([Methoden](classes.md#methods)) im Hinblick auf die gültigen Kombinationen der Modifizierer.

Die *Typ* Deklaration gibt den Typ der Eigenschaft, der durch die Deklaration einer Eigenschaft an und *Member_name* gibt den Namen der Eigenschaft. Wenn die Eigenschaft eine explizite Schnittstellenmember-Implementierung, ist die *Member_name* ist einfach ein *Bezeichner*. Für eine explizite Schnittstellenmember-Implementierung ([explizite Implementierungen eines Schnittstellenmembers](interfaces.md#explicit-interface-member-implementations)), wird die *Member_name* besteht aus einer *Interface_type* gefolgt von einer " `.`"und ein *Bezeichner*.

Die *Typ* einer Eigenschaft muss mindestens dieselben zugriffsmöglichkeiten bieten wie die Eigenschaft selbst ([Barrierefreiheit Einschränkungen](basic-concepts.md#accessibility-constraints)).

Ein *Property_body* entweder besteht möglicherweise aus einem ***Accessor-Body*** oder ***ausdruckskörper***. In einem Accessortext *Accessor_declarations*, die eingeschlossen werden müssen, "`{`"und"`}`" Token, deklarieren die Accessoren ([Accessoren](classes.md#accessors)) der Eigenschaft. Die Accessoren angeben, die ausführbaren Anweisungen lesen und schreiben die Eigenschaft zugeordnet wird.

Ein ausdruckskörper bestehend aus `=>` gefolgt von einer *Ausdruck* `E` und ein Semikolon entspricht genau der Anweisungstext `{ get { return E; } }`, und kann daher nur nur-Getter an verwendet werden Eigenschaften, in dem das Ergebnis der getter-Methode von einem einzelnen Ausdruck angegeben ist.

Ein *Property_initializer* kann nur für eine automatisch implementierte Eigenschaft zugewiesen werden ([automatisch implementierten Eigenschaften](classes.md#automatically-implemented-properties)), und bewirkt, dass die Initialisierung des zugrunde liegenden Felds solcher Eigenschaften mit dem angegebenen Wert der *Ausdruck*.

Obwohl die Syntax für den Zugriff auf eine Eigenschaft, die für ein Feld übereinstimmt, wird eine Eigenschaft nicht als Variable klassifiziert. Es ist daher nicht möglich, eine Eigenschaft als übergeben eine `ref` oder `out` Argument.

Wenn eine Eigenschaftendeklaration enthält ein `extern` Modifizierer, die Eigenschaft gilt eine ***Eigenschaft "external"***. Da eine Deklaration der Eigenschaft "external" keine Implementierung, jede bietet die *Accessor_declarations* besteht aus einem Semikolon.

### <a name="static-and-instance-properties"></a>Statische und Eigenschaften

Wenn eine Eigenschaftendeklaration enthält eine `static` Modifizierer, die Eigenschaft gilt eine ***statische Eigenschaft***. Wenn kein `static` Modifizierer vorhanden ist, wird die Eigenschaft gilt eine ***-Instanzeigenschaft***.

Eine statische Eigenschaft ist nicht mit einer bestimmten Instanz verknüpft, und es ist ein Fehler während der Kompilierung zum Verweisen auf `this` in den Accessoren für eine statische Eigenschaft.

Eine Instance-Eigenschaft ist eine bestimmte Instanz einer Klasse zugeordnet, und diese Instanz zugegriffen werden kann, als `this` ([diesen Zugriff](expressions.md#this-access)) in den Accessoren der Eigenschaft.

Wenn eine Eigenschaft verwiesen wird, einem *Member_access* ([Memberzugriff](expressions.md#member-access)) des Formulars `E.M`, wenn `M` ist eine statische Eigenschaft, `E` muss einen Typ mit deuten`M`, und wenn `M` ist eine Instanzeigenschaft E muss eine Instanz von einem Typ mit deuten `M`.

Die Unterschiede zwischen statischen und Instanzmember finden Sie weiter unten in [statische und Instanzmember](classes.md#static-and-instance-members).

### <a name="accessors"></a>Zugriffsmethoden

Die *Accessor_declarations* Geben Sie eine Eigenschaft der ausführbaren Anweisungen lesen und Schreiben von dieser Eigenschaft zugeordnet.

```antlr
accessor_declarations
    : get_accessor_declaration set_accessor_declaration?
    | set_accessor_declaration get_accessor_declaration?
    ;

get_accessor_declaration
    : attributes? accessor_modifier? 'get' accessor_body
    ;

set_accessor_declaration
    : attributes? accessor_modifier? 'set' accessor_body
    ;

accessor_modifier
    : 'protected'
    | 'internal'
    | 'private'
    | 'protected' 'internal'
    | 'internal' 'protected'
    ;

accessor_body
    : block
    | ';'
    ;
```

Die Accessordeklarationen bestehen aus einer *Get_accessor_declaration*, *Set_accessor_declaration*, oder beides. Jede Zugriffsmethoden-Deklaration besteht aus dem Token `get` oder `set` gefolgt von einem optionalen *Accessor_modifier* und *Accessor_body*.

Die Verwendung von *Accessor_modifier*s unterliegt folgenden Einschränkungen:

*  Ein *Accessor_modifier* kann nicht in einer Schnittstelle oder in eine explizite Schnittstellenmember-Implementierung verwendet werden.
*  Für eine Eigenschaft oder einen Indexer, der keine `override` Modifizierer, ein *Accessor_modifier* ist zulässig, nur, wenn die Eigenschaft oder der Indexer sowohl einen `get` und `set` -Accessor, und klicken Sie dann nur für eine solche darf Accessoren.
*  Für eine Eigenschaft oder einen Indexer, die enthält ein `override` Modifizierer, ein Accessor muss übereinstimmen. die *Accessor_modifier*, falls vorhanden, von der Zugriffsmethode, die überschrieben wird.
*  Die *Accessor_modifier* muss einen Zugriff, die streng restriktiver ist als die deklarierte Zugriffsart von Eigenschaft oder des Indexers selbst deklariert werden. Um genau zu sein:
   * Wenn die Eigenschaft oder des Indexers eine deklarierte Zugriffsart von ist `public`, *Accessor_modifier* entweder `protected internal`, `internal`, `protected`, oder `private`.
   * Wenn die Eigenschaft oder des Indexers eine deklarierte Zugriffsart von ist `protected internal`, *Accessor_modifier* entweder `internal`, `protected`, oder `private`.
   * Wenn die Eigenschaft oder des Indexers eine deklarierte Zugriffsart von ist `internal` oder `protected`, *Accessor_modifier* muss `private`.
   * Wenn die Eigenschaft oder des Indexers eine deklarierte Zugriffsart von ist `private`, keine *Accessor_modifier* kann verwendet werden.

Für `abstract` und `extern` Eigenschaften, die *Accessor_body* für jeden Accessor angegeben einfach ein Semikolon ist. Eine nicht abstrakte, nicht Extern Eigenschaft möglicherweise jede *Accessor_body* können Sie ein Semikolon, in diesem Fall ist es ein ***automatisch implementierte Eigenschaft*** ([automatisch implementierte Eigenschaften ](classes.md#automatically-implemented-properties)). Eine automatisch implementierte Eigenschaft müssen mindestens einen Get-Accessor. Für den Accessoren für eine andere nicht abstrakten, nicht Extern Eigenschaft die *Accessor_body* ist eine *Block* der gibt an, die Anweisungen ausgeführt werden, wenn der entsprechende-Accessor aufgerufen wird.

Ein `get` -Accessor entspricht einer Methode ohne Parameter mit einem Rückgabewert des Eigenschaftstyps. Mit Ausnahme der als Ziel einer Zuweisung, wenn eine Eigenschaft in einem Ausdruck verwiesen wird die `get` -Accessor der Eigenschaft wird aufgerufen, um den Wert der Eigenschaft zu berechnen ([Werte Ausdrücke](expressions.md#values-of-expressions)). Der Text, der eine `get` Accessor entsprechen den Regeln für die Wertrückgabe beschriebenen Methoden [Methodentext](classes.md#method-body). Insbesondere alle `return` Anweisungen im Text einer `get` Accessor muss einen Ausdruck implizit in den Eigenschaftentyp angeben. Darüber hinaus den Endpunkt des eine `get` Accessor nicht erreicht werden kann.

Ein `set` Accessor entspricht einer Methode mit einem einzelnen Werteparameter des Eigenschaftstyps und `void` Typ zurückgeben. Der implizite Parameter eines eine `set` Accessor wird stets der Name `value`. Wenn eine Eigenschaft verwiesen wird, als Ziel einer Zuweisung ([Zuweisungsoperatoren](expressions.md#assignment-operators)), oder als Operand `++` oder `--` ([Postfix-Inkrement und Dekrement-Operatoren](expressions.md#postfix-increment-and-decrement-operators), [ Präfix-Inkrement und Dekrement-Operatoren](expressions.md#prefix-increment-and-decrement-operators)), wird die `set` -Accessor wird aufgerufen, mit einem Argument (, deren Wert ist, die von der rechten Seite der Zuweisung oder der Operand des der `++` oder `--` Operator), Gibt den neuen Wert ([einfache Zuweisung](expressions.md#simple-assignment)). Der Text, der eine `set` Accessor entsprechen den Regeln für `void` beschriebenen Methoden [Methodentext](classes.md#method-body). Insbesondere `return` Anweisungen in der `set` Accessor-Body sind nicht zulässig, einen Ausdruck angeben. Da eine `set` Accessor weist implizit einen Parameter namens `value`, es ist ein Fehler während der Kompilierung für die Deklaration einer lokalen Variable oder Konstante in einen `set` Accessor diesen Namen vor.

Basierend auf das Vorhandensein oder fehlen von der `get` und `set` Accessoren, eine Eigenschaft wird wie folgt klassifiziert:

*  Eine Eigenschaft, die beides umfasst eine `get` Accessor und einen `set` Accessor gilt eine ***Lese-/ Schreibzugriff*** Eigenschaft.
*  Eine Eigenschaft, die nur eine `get` Accessor gilt eine ***schreibgeschützte*** Eigenschaft. Es ist ein Fehler während der Kompilierung für eine schreibgeschützte Eigenschaft, die das Ziel einer Zuweisung sein.
*  Eine Eigenschaft, die nur eine `set` Accessor gilt eine ***lesegeschützte*** Eigenschaft. Mit der Ausnahme als Ziel einer Zuweisung, es einen Fehler während der Kompilierung, um eine Nur-Schreiben-Eigenschaft in einem Ausdruck zu verweisen ist.

Im Beispiel
```csharp
public class Button: Control
{
    private string caption;

    public string Caption {
        get {
            return caption;
        }
        set {
            if (caption != value) {
                caption = value;
                Repaint();
            }
        }
    }

    public override void Paint(Graphics g, Rectangle r) {
        // Painting code goes here
    }
}
```
die `Button` Steuerelement deklariert ein öffentliches `Caption` Eigenschaft. Die `get` Accessor die `Caption` Eigenschaft gibt die Zeichenfolge, die in der privaten gespeicherten `caption` Feld. Die `set` Accessor überprüft, ob der neue Wert unterscheidet sich von den aktuellen Wert aus, und wenn dies der Fall ist, es den neuen Wert speichert und wird neu aufgebaut, das Steuerelement. Eigenschaften führen Sie oft auf das oben gezeigte Muster aus: Die `get` Accessor gibt einfach einen Wert, der in einem privaten Feld gespeichert und die `set` Accessor ändert das private Feld und führt dann zusätzlichen Aktionen erforderlich, um den Zustand des Objekts vollständig zu aktualisieren.

Erhält die `Button` oben gezeigte Klasse, die folgenden ist ein Beispiel mit der `Caption` Eigenschaft:
```csharp
Button okButton = new Button();
okButton.Caption = "OK";            // Invokes set accessor
string s = okButton.Caption;        // Invokes get accessor
```

Hier ist die `set` -Accessor wird aufgerufen, durch das Zuweisen eines Werts der Eigenschaft und die `get` Accessor wird aufgerufen, indem Sie auf die Eigenschaft in einem Ausdruck verweisen.

Die `get` und `set` Accessor einer Eigenschaft sind nicht unterschiedliche Elemente aus, und es ist nicht möglich, um die Accessoren der Eigenschaft separat zu deklarieren. Daher ist es nicht möglich, für die beiden Accessoren Schreib-Lese-Eigenschaft auf unterschiedlichen Zugriff haben. Im Beispiel
```csharp
class A
{
    private string name;

    public string Name {                // Error, duplicate member name
        get { return name; }
    }

    public string Name {                // Error, duplicate member name
        set { name = value; }
    }
}
```
eine einzelne Lese-/ Schreibzugriff-Eigenschaft ist nicht deklariert werden. Stattdessen deklariert zwei Eigenschaften mit dem gleichen Namen, eine schreibgeschützte und lesegeschützte. Da zwei Elemente, die in der gleichen Klasse deklariert den gleichen Namen haben können, wird im Beispiel wird einen Fehler während der Kompilierung auftreten.

Wenn eine abgeleitete Klasse eine Eigenschaft mit dem gleichen Namen wie eine geerbte Eigenschaft deklariert, blendet die abgeleitete Eigenschaft die geerbte Eigenschaft in Bezug auf das Lesen und schreiben. Im Beispiel
```csharp
class A
{
    public int P {
        set {...}
    }
}

class B: A
{
    new public int P {
        get {...}
    }
}
```
die `P` -Eigenschaft in `B` Blendet die `P` -Eigenschaft in `A` in Bezug auf das Lesen und schreiben. Daher in den Anweisungen
```csharp
B b = new B();
b.P = 1;          // Error, B.P is read-only
((A)b).P = 1;     // Ok, reference to A.P
```
die Zuweisung zu `b.P` verursacht einen Fehler während der Kompilierung gemeldet werden, da die schreibgeschützte `P` -Eigenschaft in `B` Blendet Sie aus, die nur-schreiben- `P` -Eigenschaft in `A`. Beachten Sie jedoch, dass eine Umwandlung verwendet werden kann, auf die ausgeblendeten `P` Eigenschaft.

Geben Sie im Gegensatz zu öffentlichen Feldern Eigenschaften eine Trennung zwischen internen Zustand eines Objekts und dessen öffentliche Schnittstelle. Betrachten Sie das Beispiel aus:
```csharp
class Label
{
    private int x, y;
    private string caption;

    public Label(int x, int y, string caption) {
        this.x = x;
        this.y = y;
        this.caption = caption;
    }

    public int X {
        get { return x; }
    }

    public int Y {
        get { return y; }
    }

    public Point Location {
        get { return new Point(x, y); }
    }

    public string Caption {
        get { return caption; }
    }
}
```

Hier ist die `Label` -Klasse verwendet zwei `int` Felder `x` und `y`, um den Speicherort zu speichern. Der Speicherort ist öffentlich verfügbar gemacht, als ein `X` und `Y` Eigenschaft und als eine `Location` Eigenschaft vom Typ `Point`. Im Rahmen einer zukünftigen Version von `Label`, wird es einfacher, den Ort als eine `Point` intern für die Änderung vorgenommen werden kann, ohne Auswirkungen auf die öffentliche Schnittstelle der Klasse:
```csharp
class Label
{
    private Point location;
    private string caption;

    public Label(int x, int y, string caption) {
        this.location = new Point(x, y);
        this.caption = caption;
    }

    public int X {
        get { return location.x; }
    }

    public int Y {
        get { return location.y; }
    }

    public Point Location {
        get { return location; }
    }

    public string Caption {
        get { return caption; }
    }
}
```

Hatte `x` und `y` wurde stattdessen `public readonly` Felder, war es unmöglich, solche eine Änderung an der `Label` Klasse.

Zustand durch Eigenschaften verfügbar zu machen, ist nicht unbedingt weniger effizient als Felder direkt verfügbar zu machen. Insbesondere, wenn eine Eigenschaft nicht virtuell ist und nur eine kleine Menge an Code enthält, kann die ausführungsumgebung Aufrufe von Accessoren mit den eigentlichen Code der Accessoren ersetzen. Dieser Prozess wird als bezeichnet ***inlining***, und es wird der Zugriff auf Eigenschaften so effizient wie Feldzugriff, jedoch behält die höhere Flexibilität von Eigenschaften.

Seit dem Aufrufen einer `get` Accessor Konzept entspricht dem Lesen des Werts eines Felds, gilt dies Unzulässiger Programmierstil für `get` Accessoren, um sichtbare Nebeneffekte haben. Im Beispiel
```csharp
class Counter
{
    private int next;

    public int Next {
        get { return next++; }
    }
}
```
Der Wert des der `Next` Eigenschaft hängt die Anzahl der Male, die die Eigenschaft zuvor zugegriffen wurde. Klicken Sie daher Zugriff auf die Eigenschaft einen Observable Nebeneffekt erzeugt, und die Eigenschaft sollte stattdessen als eine Methode implementiert werden.

Die "keine Nebeneffekte" Konvention für `get` Accessoren nicht bedeuten, dass `get` Accessoren sollte stets so verfasst werden einfach in Feldern gespeicherte Werte zurückgeben. In der Tat `get` Accessoren häufig berechnen Sie den Wert einer Eigenschaft, indem Sie den Zugriff auf mehrere Felder oder Methoden aufrufen. Allerdings ein ordnungsgemäß entwickeltes `get` Accessor führt keine Aktionen, die dazu führen, wahrnehmbare Änderungen in den Zustand des Objekts dass.

Eigenschaften können verwendet werden, um die Initialisierung einer Ressource bis zum Moment zu verzögern, die es erstmalig verwiesen wird. Zum Beispiel:
```csharp
using System.IO;

public class Console
{
    private static TextReader reader;
    private static TextWriter writer;
    private static TextWriter error;

    public static TextReader In {
        get {
            if (reader == null) {
                reader = new StreamReader(Console.OpenStandardInput());
            }
            return reader;
        }
    }

    public static TextWriter Out {
        get {
            if (writer == null) {
                writer = new StreamWriter(Console.OpenStandardOutput());
            }
            return writer;
        }
    }

    public static TextWriter Error {
        get {
            if (error == null) {
                error = new StreamWriter(Console.OpenStandardError());
            }
            return error;
        }
    }
}
```

Die `Console` -Klasse enthält drei Eigenschaften `In`, `Out`, und `Error`, die Eingabe, Ausgabe und Fehler Geräten bzw. darstellen. Durch diese Member als Eigenschaften verfügbar macht, die `Console` Klasse kann die Initialisierung verzögert, bis sie tatsächlich verwendet werden. Beispiel: nach dem ersten Verweis auf die `Out` Eigenschaft, wie in
```csharp
Console.Out.WriteLine("hello, world");
```
die zugrunde liegende `TextWriter` Ausgabegerät erstellt wird. Aber wenn die Anwendung keinen Verweis auf die `In` und `Error` Eigenschaften, keine Objekte für diese Geräte erstellt werden.

### <a name="automatically-implemented-properties"></a>Automatisch implementierte Eigenschaften

Eine automatisch implementierte Eigenschaft (oder ***Auto-Eigenschaft*** kurz), ist eine nicht abstrakte nicht Extern-Eigenschaft mit dem Text von Accessoren für reine durch Semikolons. Auto-Eigenschaften können müssen einen Get-Accessor und optional auch einen Set-Accessor.

Wenn eine Eigenschaft als eine automatisch implementierte Eigenschaft angegeben wird, ein ausgeblendetes dahinter liegendes Feld wird automatisch für die Eigenschaft zur Verfügung, und die Accessoren implementiert, um Lese- und Schreibzugriff auf dieses dahinter liegende Feld. Wenn die automatische Eigenschaft keine Set-Accessor verfügt, gilt das dahinter liegende Feld `readonly` ([schreibgeschützte Felder](classes.md#readonly-fields)). Genau wie ein `readonly` Feld eine nur-Getter-Auto-Eigenschaft kann auch zugewiesen werden im Text eines Konstruktors der einschließenden Klasse. Eine solche Zuweisung wird direkt das Readonly dahinter liegende Feld der Eigenschaft zugewiesen.

Eine Auto-Eigenschaft möglicherweise optional ein *Property_initializer*, das angewendet wird, direkt auf das dahinter liegende Feld als ein *Variable_initializer* ([Variableninitialisierern](classes.md#variable-initializers)) .

Im folgenden Beispiel:
```csharp
public class Point {
    public int X { get; set; } = 0;
    public int Y { get; set; } = 0;
}
```
entspricht die folgende Deklaration:
```csharp
public class Point {
    private int __x = 0;
    private int __y = 0;
    public int X { get { return __x; } set { __x = value; } }
    public int Y { get { return __y; } set { __y = value; } }
}
```

Im folgenden Beispiel:
```csharp
public class ReadOnlyPoint
{
    public int X { get; }
    public int Y { get; }
    public ReadOnlyPoint(int x, int y) { X = x; Y = y; }
}
```
entspricht die folgende Deklaration:
```csharp
public class ReadOnlyPoint
{
    private readonly int __x;
    private readonly int __y;
    public int X { get { return __x; } }
    public int Y { get { return __y; } }
    public ReadOnlyPoint(int x, int y) { __x = x; __y = y; }
}
```

Beachten Sie, dass die Zuweisungen für das schreibgeschützte Feld zulässig, da sie innerhalb des Konstruktors auftreten.


### <a name="accessibility"></a>Zugriff

Besitzt ein Accessor einer *Accessor_modifier*, die Zugriffsdomäne ([Barrierefreiheit Domänen](basic-concepts.md#accessibility-domains)) des Accessors bestimmt ist, verwenden die deklarierte Zugriffsart von der *Accessor_modifier* . Wenn ein Accessor kein *Accessor_modifier*, die Zugriffsdomäne des Accessors wird über die deklarierte Zugriffsart von Eigenschaft oder des Indexers bestimmt.

Das Vorhandensein einer *Accessor_modifier* wirkt sich nicht auf die Suche nach Membern ([Operatoren](expressions.md#operators)) oder der überladungsauflösung ([Überladungsauflösung](expressions.md#overload-resolution)). Der Modifizierer für die Eigenschaft oder der Indexer zu immer bestimmen, welche Eigenschaft oder des Indexers, unabhängig vom Kontext des Zugriffs gebunden ist.

Nachdem eine bestimmte Eigenschaft oder einen Indexer ausgewählt wurde, werden die Barrierefreiheit Domänen der beteiligten bestimmten Accessoren verwendet, um festzustellen, ob diese Nutzung gültig ist:

*  Ist die Verwendung als Wert ([Werte Ausdrücke](expressions.md#values-of-expressions)), wird die `get` -Accessor vorhanden und zugänglich sein muss.
*  Wenn die Verwendung als Ziel für eine einfache Zuweisung ([einfache Zuweisung](expressions.md#simple-assignment)), wird die `set` -Accessor vorhanden und zugänglich sein muss.
*  Wenn die Auslastung als Ziel für zusammengesetzte Zuweisung ist ([Verbundzuweisung](expressions.md#compound-assignment)), oder als Ziel für die `++` oder `--` Operatoren ([Funktionsmember](expressions.md#function-members).9, [ Aufrufausdrücke](expressions.md#invocation-expressions)), werden beide die `get` Accessoren und `set` -Accessor vorhanden und zugänglich sein muss.

Im folgenden Beispiel die Eigenschaft `A.Text` ist ausgeblendet, die Eigenschaft `B.Text`, dies gilt auch in Kontexten, in dem nur die `set` Accessor wird aufgerufen. Im Gegensatz dazu sind die Eigenschaft `B.Count` kann nicht zugegriffen werden, um die Klasse `M`, sodass die Eigenschaft zugegriffen werden kann `A.Count` wird stattdessen verwendet.

```csharp
class A
{
    public string Text {
        get { return "hello"; }
        set { }
    }

    public int Count {
        get { return 5; }
        set { }
    }
}

class B: A
{
    private string text = "goodbye"; 
    private int count = 0;

    new public string Text {
        get { return text; }
        protected set { text = value; }
    }

    new protected int Count { 
        get { return count; }
        set { count = value; }
    }
}

class M
{
    static void Main() {
        B b = new B();
        b.Count = 12;             // Calls A.Count set accessor
        int i = b.Count;          // Calls A.Count get accessor
        b.Text = "howdy";         // Error, B.Text set accessor not accessible
        string s = b.Text;        // Calls B.Text get accessor
    }
}
```

Ein Accessor, der verwendet wird, um eine Schnittstelle implementieren, möglicherweise kein *Accessor_modifier*. Wenn nur ein Accessor verwendet wird, eine Schnittstelle implementieren, kann der andere Accessor deklariert werden, mit einem *Accessor_modifier*:
```csharp
public interface I
{
    string Prop { get; }
}

public class C: I
{
    public Prop {
        get { return "April"; }       // Must not have a modifier here
        internal set {...}            // Ok, because I.Prop has no set accessor
    }
}
```

### <a name="virtual-sealed-override-and-abstract-property-accessors"></a>Virtuelle, versiegelt, überschriebene und abstrakte eigenschaftenzugriffsmethoden

Ein `virtual` -Eigenschaftendeklaration gibt an, dass die Accessoren der Eigenschaft virtuell sind. Die `virtual` Modifizierer gilt für beide Accessoren der Schreib-Lese-Eigenschaft – es ist nicht möglich, dass nur ein Accessor einer Eigenschaft Lese-/ Schreibzugriff auf nicht virtuell sein.

Ein `abstract` Eigenschaftendeklaration gibt an, dass die Accessoren der Eigenschaft sind virtuell, jedoch bietet keine tatsächliche Implementierung der Accessoren. Stattdessen müssen nicht abstrakte abgeleitete Klassen ihre eigene Implementierung für die Accessoren durch Überschreiben der Eigenschaft bereitstellen. Da Sie ein Accessor für eine abstrakte Eigenschaftendeklaration keine Implementierungen, bietet die *Accessor_body* besteht einfach aus einem Semikolon.

Eine Eigenschaftendeklaration, die beide enthält die `abstract` und `override` Modifizierer gibt an, dass die Eigenschaft ist abstrakt und überschreibt eine Basiseigenschaft. Accessoren für diese Eigenschaft ist auch abstrakt sind.

Abstrakte Eigenschaftendeklarationen sind nur in abstrakten Klassen zulässig ([abstrakte Klassen](classes.md#abstract-classes)). Die Zugriffsmethoden einer geerbten virtuellen-Eigenschaft können in einer abgeleiteten Klasse überschrieben werden, durch eine Eigenschaftendeklaration, der angibt, ein `override` Richtlinie. Dies bezeichnet man als ein ***überschreiben Eigenschaftendeklaration***. Eine überschreibende Eigenschaftsdeklaration ist eine neue Eigenschaft nicht deklarieren. Stattdessen ist einfach die Implementierungen der Accessoren einer vorhandenen virtuellen Eigenschaft spezialisiert.

Eine überschreibende Eigenschaftsdeklaration muss genau gleichen Zugriffsmodifizierer, Typ und Namen als die geerbte Eigenschaft angeben. Wenn die geerbte Eigenschaft hat nur einen einzelnen Accessor (d. h., wenn die geerbte Eigenschaft schreibgeschützt oder lesegeschützt ist), das die überschreibende Eigenschaft muss nur diesen Accessor enthalten. Wenn die geerbte Eigenschaft enthält beide Accessoren (d. h., wenn die geerbte Eigenschaft Lese-/ Schreibzugriff ist), die überschreibende Eigenschaft kann entweder eines einzelnen Accessors oder beide Accessoren enthalten.

Eine überschreibende Eigenschaftsdeklaration herausgeberkontos ausgewiesenen Form der `sealed` Modifizierer. Verwendung dieser Modifizierer wird verhindert, dass eine abgeleitete Klasse weiter Überschreiben der Eigenschaft. Die Zugriffsmethoden einer versiegelten Eigenschaft sind ebenfalls versiegelt.

Verhalten Syntax, virtuellen, "sealed" außer Kraft setzen und abstrakten Accessor genau wie virtuelle, "sealed" Override "und" abstrakte Methoden, mit Ausnahme der Unterschiede in der Deklarations- und Aufrufsyntax. Insbesondere die Regeln, die in beschriebenen [virtuelle Methoden](classes.md#virtual-methods), [Methoden außer Kraft setzen](classes.md#override-methods), [Versiegelte Methoden](classes.md#sealed-methods), und [abstrakten Methoden](classes.md#abstract-methods) gelten wie Accessoren wurden die Methoden der entsprechenden:

*  Ein `get` -Accessor entspricht einer Methode ohne Parameter mit einem Rückgabewert von den Eigenschaftentyp und den gleichen Modifizierer wie die Eigenschaft.
*  Ein `set` Accessor entspricht einer Methode mit einem Einzelwert-Parameter, der den Typ der Eigenschaft, ein `void` Typ und die gleichen Modifizierer wie die Eigenschaft zurück.

Im Beispiel
```csharp
abstract class A
{
    int y;

    public virtual int X {
        get { return 0; }
    }

    public virtual int Y {
        get { return y; }
        set { y = value; }
    }

    public abstract int Z { get; set; }
}
```
`X` ist eine virtuelle nur-Lese Eigenschaft, `Y` ist eine virtuelle Lese-/ Schreibzugriff-Eigenschaft, und `Z` ist eine abstrakte Eigenschaft mit Lese-/ Schreibzugriff. Da `Z` ist eine abstrakte Methode der enthaltenden Klasse `A` muss auch als abstrakt deklariert werden.

Eine abgeleitete Klasse `A` wird unten gezeigt:
```csharp
class B: A
{
    int z;

    public override int X {
        get { return base.X + 1; }
    }

    public override int Y {
        set { base.Y = value < 0? 0: value; }
    }

    public override int Z {
        get { return z; }
        set { z = value; }
    }
}
```

Hier wird die Deklarationen der `X`, `Y`, und `Z` Eigenschaftendeklarationen überschreiben. Jede Eigenschaftendeklaration entspricht genau der Zugriffsmodifizierer, Typ und Namen der entsprechenden geerbte Eigenschaften. Die `get` Accessor `X` und `set` Accessor `Y` verwenden die `base` Schlüsselwort, um die geerbten Accessoren. Die Deklaration von `Z` beide Accessoren abstrakte überschreibungen – es gibt also keine ausstehenden abstrakte Funktionsmember in `B`, und `B` ist zulässig, eine nicht abstrakte Klasse sein.

Wenn eine Eigenschaft deklariert ist, als ein `override`, alle überschriebenen Accessoren müssen für den überschreibenden Code zugänglich sein. Darüber hinaus muss die deklarierte Zugriffsart von sowohl die Eigenschaft oder den Indexer und Accessoren, mit dem überschriebenen Member und den Accessoren übereinstimmen. Zum Beispiel:
```csharp
public class B
{
    public virtual int P {
        protected set {...}
        get {...}
    }
}

public class D: B
{
    public override int P {
        protected set {...}            // Must specify protected here
        get {...}                      // Must not have a modifier here
    }
}
```

## <a name="events"></a>Ereignisse

Ein ***Ereignis*** ist ein Element, das ein Objekt oder eine Klasse, um Benachrichtigungen zu senden zu können. Clients können ausführbaren Code für Ereignisse anfügen, indem Sie die Angabe ***Ereignishandler***.

Ereignisse werden mit deklariert *Event_declaration*s:

```antlr
event_declaration
    : attributes? event_modifier* 'event' type variable_declarators ';'
    | attributes? event_modifier* 'event' type member_name '{' event_accessor_declarations '}'
    ;

event_modifier
    : 'new'
    | 'public'
    | 'protected'
    | 'internal'
    | 'private'
    | 'static'
    | 'virtual'
    | 'sealed'
    | 'override'
    | 'abstract'
    | 'extern'
    | event_modifier_unsafe
    ;

event_accessor_declarations
    : add_accessor_declaration remove_accessor_declaration
    | remove_accessor_declaration add_accessor_declaration
    ;

add_accessor_declaration
    : attributes? 'add' block
    ;

remove_accessor_declaration
    : attributes? 'remove' block
    ;
```

Ein *Event_declaration* eventuell einen Satz von *Attribute* ([Attribute](attributes.md)) und eine gültige Kombination der vier Zugriffsmodifizierer ([Zugriffsmodifizierer ](classes.md#access-modifiers)), wird die `new` ([der new-Modifizierer](classes.md#the-new-modifier)), `static` ([statische und Instanzmethoden](classes.md#static-and-instance-methods)), `virtual` ([virtuelle Methoden](classes.md#virtual-methods)), `override` ([Methoden außer Kraft setzen](classes.md#override-methods)), `sealed` ([Versiegelte Methoden](classes.md#sealed-methods)), `abstract` ([abstrakten Methoden](classes.md#abstract-methods)), und `extern` ([Externe Methoden](classes.md#external-methods)) Modifizierer.

Ereignisdeklarationen gelten dieselben Regeln wie Methodendeklarationen ([Methoden](classes.md#methods)) im Hinblick auf die gültigen Kombinationen der Modifizierer.

Die *Typ* eines Ereignisses muss die Deklaration einer *Delegate_type* ([Verweistypen](types.md#reference-types)), und dass *Delegate_type* muss mindestens als wie das Ereignis selbst zugegriffen werden kann ([Barrierefreiheit Einschränkungen](basic-concepts.md#accessibility-constraints)).

Eine Ereignisdeklaration herausgeberkontos ausgewiesenen Form *Event_accessor_declarations*. Jedoch wenn, für nicht-Extern, nicht abstrakte Ereignisse nicht der Compiler sie automatisch bereitgestellt ([Feldähnliche Ereignisse](classes.md#field-like-events)); für "extern"-Ereignisse, die Accessoren extern bereitgestellt werden.

Ein Event-Deklaration, die ausgelassen *Event_accessor_declarations* definiert eine oder mehrere Ereignisse – eine für jede der *Variable_declarator*s. Die Attribute und Modifizierer, gelten für alle Member deklariert, indem z. B. eine *Event_declaration*.

Es ist ein Fehler während der Kompilierung für eine *Event_declaration* zu beiden enthalten die `abstract` Modifizierer und geschweifte Klammern getrennter *Event_accessor_declarations*.

Wenn eine Ereignisdeklaration enthält ein `extern` Modifizierer verwenden, das Ereignis wird als ein ***externes Ereignis***. Da eine externes Ereignisdeklaration keine tatsächliche Implementierung bereitstellt, ist ein Fehler für sie zu beiden enthalten die `extern` Modifizierer und *Event_accessor_declarations*.

Es ist ein Fehler während der Kompilierung für eine *Variable_declarator* einer Event-Deklaration mit einer `abstract` oder `external` Modifizierer enthalten eine *Variable_initializer*.

Ein Ereignis kann verwendet werden, wie der linke Operand der `+=` und `-=` Operatoren ([Ereignis Zuweisung](expressions.md#event-assignment)). Diese Operatoren werden, bzw. zum Anfügen von Ereignishandlern zu oder Entfernen von-Ereignishandler von einem Ereignis, und der Zugriffsmodifizierer des Ereignisses steuern die Kontexte, in denen solche Vorgänge zulässig sind.

Da `+=` und `-=` sind die einzigen Operationen, die auf ein Ereignis außerhalb des Typs, der deklariert zulässig sind, das Ereignis, den externen Code können hinzufügen und entfernen-Handler für ein Ereignis, jedoch kann nicht auf andere Weise zu erhalten oder ändern die zugrunde liegenden Liste des Ereignisses Handler.

In einem Vorgang des Formulars `x += y` oder `x -= y`Wenn `x` ist ein Ereignis aus, und der Verweis erfolgt außerhalb des Typs, die die Deklaration enthält `x`, weist das Ergebnis des Vorgangs den Typ `void` (im Gegensatz zur Verwendung Der Typ des `x`, mit dem Wert des `x` nach der Zuweisung). Mit dieser Regel wird verhindert, dass externen Code indirekt überprüft den zugrunde liegenden Delegaten eines Ereignisses.

Das folgende Beispiel zeigt, wie Ereignishandler angefügt werden, um Instanzen von der `Button` Klasse:
```csharp
public delegate void EventHandler(object sender, EventArgs e);

public class Button: Control
{
    public event EventHandler Click;
}

public class LoginDialog: Form
{
    Button OkButton;
    Button CancelButton;

    public LoginDialog() {
        OkButton = new Button(...);
        OkButton.Click += new EventHandler(OkButtonClick);
        CancelButton = new Button(...);
        CancelButton.Click += new EventHandler(CancelButtonClick);
    }

    void OkButtonClick(object sender, EventArgs e) {
        // Handle OkButton.Click event
    }

    void CancelButtonClick(object sender, EventArgs e) {
        // Handle CancelButton.Click event
    }
}
```

Hier die `LoginDialog` Instanzkonstruktor erstellt zwei `Button` Instanzen und fügt der Ereignishandler die `Click` Ereignisse.

### <a name="field-like-events"></a>Feldähnliche Ereignisse

In den Programmtext der Klasse oder Struktur, die die Deklaration eines Ereignisses enthält, können bestimmte Ereignisse wie Felder verwendet werden. Auf diese Weise verwendet werden soll, ein Ereignis darf nicht sein `abstract` oder `extern`, und müssen nicht explizit *Event_accessor_declarations*. Derartiges Ereignis kann in jedem Kontext verwendet werden, die ein Feld zulässt. Das Feld enthält einen Delegaten ([Delegaten](delegates.md)) die bezieht sich auf die Liste der Ereignishandler, die das Ereignis hinzugefügt wurden. Wenn keine Ereignishandler hinzugefügt wurden, enthält das Feld `null`.

Im Beispiel
```csharp
public delegate void EventHandler(object sender, EventArgs e);

public class Button: Control
{
    public event EventHandler Click;

    protected void OnClick(EventArgs e) {
        if (Click != null) Click(this, e);
    }

    public void Reset() {
        Click = null;
    }
}
```
`Click` Dient als ein Feld innerhalb der `Button` Klasse. Wie im Beispiel wird veranschaulicht, kann das Feld werden überprüft, geändert und in den Delegaten aufrufen von Ausdrücken verwendet. Die `OnClick` -Methode in der die `Button` "löst" die `Click` Ereignis. Das Auslösen eines Ereignisses entspricht exakt dem Aufrufen des Delegaten, der durch das Ereignis repräsentiert wird, es gibt deshalb keine besonderen Sprachkonstrukte zum Auslösen von Ereignissen. Beachten Sie, dass der Delegataufruf eine Überprüfung vorangestellt ist, die sicherstellt, dass der Delegat ungleich Null ist.

Außerhalb der Variablendeklaration der `Button` -Klasse, die `Click` Member kann nur verwendet werden, auf der linken Seite von der `+=` und `-=` Operatoren, wie in
```csharp
b.Click += new EventHandler(...);
```
Ein Delegat der Aufrufliste von fügt die `Click` Ereignis und
```csharp
b.Click -= new EventHandler(...);
```
das einen Delegaten aus der Aufrufliste entfernt die `Click` Ereignis.

Beim Kompilieren ein feldähnliches Ereignis der Compiler erstellt automatisch Speicher zum Speichern von Delegaten, und Zugriffsmethoden für das Ereignis, die hinzufügen oder Entfernen von Ereignishandlern auf das Delegatfeld erstellt. Die Vorgänge hinzufügen und entfernen sind threadsicher, und möglicherweise (jedoch nicht erforderlich, um) Fertig Zeit belegen die Sperre ([die Lock-Anweisung](statements.md#the-lock-statement)) auf das enthaltende Objekt für ein Instanzereignis oder die Typ-Objekt ([anonym Objektausdrücke Erstellung](expressions.md#anonymous-object-creation-expressions)) für ein statisches Ereignis.

Daher eine Instanzereignisdeklaration des Formulars:
```csharp
class X
{
    public event D Ev;
}
```
wird in etwa gleich kompiliert werden:
```csharp
class X
{
    private D __Ev;  // field to hold the delegate

    public event D Ev {
        add {
            /* add the delegate in a thread safe way */
        }

        remove {
            /* remove the delegate in a thread safe way */
        }
    }
}
```
In der Klasse `X`, Verweise auf `Ev` auf der linken Seite von der `+=` und `-=` Operatoren dazu führen, dass das Hinzufügen und remove-Accessoren besitzen, die aufgerufen werden. Alle anderen Verweise auf `Ev` werden kompiliert, um das ausgeblendete Feld verweisen `__Ev` stattdessen ([Memberzugriff](expressions.md#member-access)). Der Name "`__Ev`" ist ein beliebiger; das ausgeblendete Feld möglicherweise eine beliebige oder gar kein Name überhaupt.

### <a name="event-accessors"></a>Ereignisaccessoren

Ereignisdeklarationen in der Regel auslassen *Event_accessor_declarations*, z. B. die `Button` Beispiel oben. Eine Situation dafür umfasst also den Fall, in dem die Speicherkosten, die ein Feld pro Ereignis nicht zulässig ist. In solchen Fällen kann eine Klasse enthalten *Event_accessor_declarations* und einen privaten Mechanismus zum Speichern der Liste der Ereignishandler verwenden.

Die *Event_accessor_declarations* eines Ereignisses Geben Sie die ausführbare Anweisungen, die mit dem Hinzufügen und Entfernen von Ereignishandlern verknüpft ist.

Die Accessordeklarationen bestehen aus einer *Add_accessor_declaration* und *Remove_accessor_declaration*. Jede Zugriffsmethoden-Deklaration besteht aus dem Token `add` oder `remove` gefolgt von einem *Block*. Die *Block* zugeordneten ein *Add_accessor_declaration* gibt an, die Anweisungen, die ausgeführt werden, wenn ein Ereignishandler hinzugefügt wird, und die *Block* eine zugeordnet*Remove_accessor_declaration* gibt an, die Anweisungen, die ausgeführt werden, wenn ein Ereignishandler entfernt wird.

Jede *Add_accessor_declaration* und *Remove_accessor_declaration* entspricht einer Methode mit einem einzelnen Werteparameter des Ereignistyps und `void` Typ zurückgeben. Den Namen der impliziten Parameters mit der ein Ereignisaccessor `value`. Wenn ein Ereignis in einer Ereignis-Zuweisung verwendet wird, wird der entsprechende Ereignisaccessor verwendet. Insbesondere ist der Zuweisungsoperator `+=` der Add-Accessor verwendet wird, und wenn der Zuweisungsoperator `-=` der Remove-Accessor verwendet wird. In beiden Fällen wird der Rechte Operand des Zuweisungsoperators als Argument für den Ereignisaccessor verwendet. Der Block eine *Add_accessor_declaration* oder *Remove_accessor_declaration* entsprechen den Regeln für `void` beschriebenen Methoden [Methodentext](classes.md#method-body). Insbesondere `return` Anweisungen in einem solchen Block sind nicht zulässig, einen Ausdruck angeben.

Da ein Ereignisaccessor implizit einen Parameter namens weist `value`, es ist ein Fehler während der Kompilierung für eine lokale Variable oder Konstante in einem Ereignisaccessor auf diesen Namen nicht bereits deklariert.

Im Beispiel
```csharp
class Control: Component
{
    // Unique keys for events
    static readonly object mouseDownEventKey = new object();
    static readonly object mouseUpEventKey = new object();

    // Return event handler associated with key
    protected Delegate GetEventHandler(object key) {...}

    // Add event handler associated with key
    protected void AddEventHandler(object key, Delegate handler) {...}

    // Remove event handler associated with key
    protected void RemoveEventHandler(object key, Delegate handler) {...}

    // MouseDown event
    public event MouseEventHandler MouseDown {
        add { AddEventHandler(mouseDownEventKey, value); }
        remove { RemoveEventHandler(mouseDownEventKey, value); }
    }

    // MouseUp event
    public event MouseEventHandler MouseUp {
        add { AddEventHandler(mouseUpEventKey, value); }
        remove { RemoveEventHandler(mouseUpEventKey, value); }
    }

    // Invoke the MouseUp event
    protected void OnMouseUp(MouseEventArgs args) {
        MouseEventHandler handler; 
        handler = (MouseEventHandler)GetEventHandler(mouseUpEventKey);
        if (handler != null)
            handler(this, args);
    }
}
```
die `Control` -Klasse implementiert einen internen Speichermechanismus für Ereignisse. Die `AddEventHandler` Methode ordnet einen Delegatwert mit einem Schlüssel, der `GetEventHandler` Methodenrückgabe der Delegat, der derzeit mit einem Schlüssel zugeordnet sind und die `RemoveEventHandler` Methode entfernt einen Delegaten als Ereignishandler für das angegebene Ereignis. Es empfiehlt sich, der zugrunde liegenden Speichermechanismus ist so entworfen, dass es keine Kosten für das Zuordnen fallen einer `null` delegieren Wert mit einem Schlüssel und somit Ereignisse ohne Behandlung keinen Speicher nutzen.

### <a name="static-and-instance-events"></a>Statische und Ereignisse

Wenn eine Ereignisdeklaration enthält eine `static` Modifizierer verwenden, das Ereignis gilt eine ***statisches Ereignis***. Wenn kein `static` Modifizierer vorhanden ist, wird das Ereignis wird als ein ***Instanzereignisses***.

Ein statisches Ereignis ist nicht mit einer bestimmten Instanz verknüpft, und es ist ein Fehler während der Kompilierung zum Verweisen auf `this` in den Accessoren für ein statisches Ereignis.

Ein Instanzereignis eine bestimmte Instanz einer Klasse zugeordnet ist, und diese Instanz zugegriffen werden kann, als `this` ([diesen Zugriff](expressions.md#this-access)) in den Zugriffsmethoden des Ereignisses.

Wenn ein Ereignis verwiesen wird, einem *Member_access* ([Memberzugriff](expressions.md#member-access)) des Formulars `E.M`, wenn `M` ist ein statisches Ereignis, `E` muss einen Typ mit deuten`M`, und wenn `M` ist ein Instanzereignis E muss eine Instanz von einem Typ mit deuten `M`.

Die Unterschiede zwischen statischen und Instanzmember finden Sie weiter unten in [statische und Instanzmember](classes.md#static-and-instance-members).

### <a name="virtual-sealed-override-and-abstract-event-accessors"></a>Virtuelle, versiegelt, überschriebene und abstrakte ereigniszugriffsmethoden

Ein `virtual` gibt an, dass die Accessoren dieses Ereignisses virtuell sind. Die `virtual` Modifizierer gilt für beide Accessoren eines Ereignisses.

Ein `abstract` Ereignisdeklaration gibt an, dass die Accessoren des Ereignisses sind virtuell, jedoch bietet keine tatsächliche Implementierung der Accessoren. Stattdessen müssen nicht abstrakte abgeleitete Klassen ihre eigene Implementierung für die Accessoren angeben, durch Überschreiben des Ereignisses. Da eine abstrakte Ereignisdeklaration keine tatsächliche Implementierung bereitstellt, kann keine es bereitstellen, geschweifte Klammern getrennter *Event_accessor_declarations*.

Eine Ereignisdeklaration, das beide enthält die `abstract` und `override` Modifizierer gibt an, dass das Ereignis ist abstrakt und überschreibt eine grundlegende-Ereignis. Die Accessoren eines solchen Ereignisses sind auch abstrakt.

Abstraktes Ereignis-Deklarationen sind nur in abstrakten Klassen zulässig ([abstrakte Klassen](classes.md#abstract-classes)).

Die zugriffmethoden von einer geerbten virtuellen Veranstaltung können in einer abgeleiteten Klasse überschrieben werden, durch eine Ereignisdeklaration, der angibt, einschließlich einer `override` Modifizierer. Dies bezeichnet man als ein ***überschreiben Ereignisdeklaration***. Eine überschreibende Ereignisdeklaration wird ein neues Ereignis nicht deklarieren. Stattdessen ist einfach die Implementierungen der Accessoren für ein vorhandenes virtuelles Ereignis spezialisiert.

Eine überschreibende Ereignisdeklaration muss dem genau gleichen Zugriffsmodifizierer, Typ und Namen als das außer Kraft gesetzte Ereignis angeben.

Eine überschreibende Event-Deklaration enthalten möglicherweise die `sealed` Modifizierer. Verwendung dieser Modifizierer wird verhindert, dass eine abgeleitete Klasse weiter Überschreiben des Ereignisses. Die Accessoren der versiegelten Ereignisses sind ebenfalls versiegelt.

Es ist ein Fehler während der Kompilierung für eine überschreibende Ereignisdeklaration sollen eine `new` Modifizierer.

Verhalten Syntax, virtuellen, "sealed" außer Kraft setzen und abstrakten Accessor genau wie virtuelle, "sealed" Override "und" abstrakte Methoden, mit Ausnahme der Unterschiede in der Deklarations- und Aufrufsyntax. Insbesondere die Regeln, die in beschriebenen [virtuelle Methoden](classes.md#virtual-methods), [Methoden außer Kraft setzen](classes.md#override-methods), [Versiegelte Methoden](classes.md#sealed-methods), und [abstrakten Methoden](classes.md#abstract-methods) gelten wie Accessoren wurden die Methoden der entsprechenden. Jeder Accessor entspricht einer Methode mit einem Einzelwert-Parameter, der den Ereignistyp vorgesehen, eine `void` Typ und die gleichen Modifizierer wie das enthaltende Ereignis zurück.

## <a name="indexers"></a>Indexer

Ein ***Indexer*** ist ein Element, das ein Objekt, auf die gleiche Weise wie ein Array indiziert werden kann. Indexer sind mit deklariert *Indexer_declaration*s:

```antlr
indexer_declaration
    : attributes? indexer_modifier* indexer_declarator indexer_body
    ;

indexer_modifier
    : 'new'
    | 'public'
    | 'protected'
    | 'internal'
    | 'private'
    | 'virtual'
    | 'sealed'
    | 'override'
    | 'abstract'
    | 'extern'
    | indexer_modifier_unsafe
    ;

indexer_declarator
    : type 'this' '[' formal_parameter_list ']'
    | type interface_type '.' 'this' '[' formal_parameter_list ']'
    ;

indexer_body
    : '{' accessor_declarations '}' 
    | '=>' expression ';'
    ;
```

Ein *Indexer_declaration* eventuell einen Satz von *Attribute* ([Attribute](attributes.md)) und eine gültige Kombination der vier Zugriffsmodifizierer ([Zugriffsmodifizierer ](classes.md#access-modifiers)), wird die `new` ([der new-Modifizierer](classes.md#the-new-modifier)), `virtual` ([virtuelle Methoden](classes.md#virtual-methods)), `override` ([Methoden außer Kraft setzen](classes.md#override-methods) ), `sealed` ([Versiegelte Methoden](classes.md#sealed-methods)), `abstract` ([abstrakten Methoden](classes.md#abstract-methods)), und `extern` ([externe Methoden](classes.md#external-methods)) Modifizierer.

Indexerdeklarationen sind, gelten dieselben Regeln wie Methodendeklarationen ([Methoden](classes.md#methods)) im Hinblick auf die gültigen Kombinationen von Modifizierern, mit der eine Ausnahme, die den static-Modifizierer darf nicht auf eine Indexerdeklaration.

Die Modifizierer `virtual`, `override`, und `abstract` gegenseitig außer in einem Fall. Die `abstract` und `override` Modifizierer können zusammen verwendet werden, sodass abstrakte Indexer einen virtuellen überschreiben kann.

Die *Typ* Deklaration gibt den Typ des Elements des Indexers, der durch die Deklaration eines Indexers an. Wenn der Indexer eine explizite Schnittstellenmember-Implementierung, ist die *Typ* folgt das Schlüsselwort `this`. Für eine explizite Schnittstellenmember-Implementierung die *Typ* gefolgt von einer *Interface_type*, eine "`.`", und das Schlüsselwort `this`. Im Gegensatz zu anderen Membern haben Indexer keine benutzerdefinierten Namen.

Die *Formal_parameter_list* gibt die Parameter des Indexers. Die Liste der formalen Parameter eines Indexers entspricht einer Methode ([Methodenparameter](classes.md#method-parameters)), mit dem Unterschied, dass mindestens ein Parameter angegeben werden muss, und dass die `ref` und `out` Parametermodifizierern sind nicht zulässig. .

Die *Typ* einen Indexer und alle Typen verwiesen wird, der *Formal_parameter_list* muss mindestens dieselben zugriffsmöglichkeiten bieten wie der Indexer selbst ([Barrierefreiheit Einschränkungen](basic-concepts.md#accessibility-constraints)).

Ein *Indexer_body* entweder besteht möglicherweise aus einem ***Accessor-Body*** oder ***ausdruckskörper***. In einem Accessortext *Accessor_declarations*, die eingeschlossen werden müssen, "`{`"und"`}`" Token, deklarieren die Accessoren ([Accessoren](classes.md#accessors)) der Eigenschaft. Die Accessoren angeben, die ausführbaren Anweisungen lesen und schreiben die Eigenschaft zugeordnet wird.

Ein ausdruckskörper bestehend aus "`=>`" gefolgt von einem Ausdruck `E` und ein Semikolon entspricht genau der Anweisungstext `{ get { return E; } }`, und kann daher nur verwendet werden nur-Getter-Indexer angeben, wo ist das Ergebnis der getter-Methode durch einen einzelnen Ausdruck angegeben.

Obwohl die Syntax für den Zugriff auf eine Indexer-Element identisch, die für ein Arrayelement ist, wird eine Indexer-Element nicht als Variable klassifiziert. Folglich ist es nicht möglich, übergeben einen Indexer-Element als ein `ref` oder `out` Argument.

Die Liste der formalen Parameter eines Indexers definiert die Signatur ([Signaturen und überladen](basic-concepts.md#signatures-and-overloading)) des Indexers. Die Signatur eines Indexers besteht insbesondere die Anzahl und Typen der formalen Parameter. Der Elementtyp und den Namen der formalen Parameter sind nicht Teil der Signatur eines Indexers.

Die Signatur eines Indexers muss die Signaturen aller anderen in derselben Klasse deklarierten Indexer unterscheiden.

Indexer und Eigenschaften sind konzeptionell sehr ähnlich, unterscheiden sich jedoch auf folgende Weise:

*  Eine Eigenschaft wird mit seinem Namen identifiziert, während von der Signatur ein Indexers identifiziert wird.
*  Eine Eigenschaft erfolgt über eine *Simple_name* ([einfache Namen](expressions.md#simple-names)) oder ein *Member_access* ([Memberzugriff](expressions.md#member-access)), während eines Indexers Element erfolgt über eine *Element_access* ([Indexerzugriff](expressions.md#indexer-access)).
*  Eine Eigenschaft kann sein, eine `static` Member, während ein Indexers immer ein Instanzmember ist.
*  Ein `get` Accessor einer Eigenschaft entspricht einer Methode ohne Parameter, während eine `get` Accessor eines Indexers entspricht einer Methode mit dem dieselbe Liste formaler Parameter wie der Indexer.
*  Ein `set` Accessor einer Eigenschaft entspricht einer Methode mit einem einzigen Parameter, die mit dem Namen `value`, während eine `set` Accessor eines Indexers entspricht einer Methode mit dem dieselbe Liste formaler Parameter wie der Indexer sowie einen zusätzlichen Parameter mit dem Namen `value`.
*  Es ist ein Fehler während der Kompilierung für einen Indexeraccessor für eine lokale Variable mit dem gleichen Namen wie ein Indexerparameter zu deklarieren.
*  In eine überschreibende Eigenschaftsdeklaration die geerbte Eigenschaft erfolgt mithilfe der Syntax `base.P`, wobei `P` ist der Eigenschaftenname. In der Indexerdeklaration einer überschreibenden der geerbte Indexer erfolgt mithilfe der Syntax `base[E]`, wobei `E` ist eine durch Trennzeichen getrennte Liste von Ausdrücken.
*  Es gibt kein Konzept für "automatisch implementierte Indexer". Es ist ein Fehler auf einen nicht abstrakten, nicht extern-Indexer mit Semikolon-Accessoren aufweisen.

Abgesehen von diesen unterschieden werden alle Regeln in definierten [Accessoren](classes.md#accessors) und [automatisch implementierten Eigenschaften](classes.md#automatically-implemented-properties) als auch für Eigenschaftenaccessoren angewendet.

Wenn eine Indexerdeklaration enthält ein `extern` Modifizierer, der Indexer gilt eine ***externe Indexer***. Da eine externe Indexerdeklaration keine Implementierung, jede bietet die *Accessor_declarations* besteht aus einem Semikolon.

Das folgende Beispiel deklariert eine `BitArray` -Klasse, die einen Indexer für den Zugriff auf den einzelnen Bits im BitArray implementiert.
```csharp
using System;

class BitArray
{
    int[] bits;
    int length;

    public BitArray(int length) {
        if (length < 0) throw new ArgumentException();
        bits = new int[((length - 1) >> 5) + 1];
        this.length = length;
    }

    public int Length {
        get { return length; }
    }

    public bool this[int index] {
        get {
            if (index < 0 || index >= length) {
                throw new IndexOutOfRangeException();
            }
            return (bits[index >> 5] & 1 << index) != 0;
        }
        set {
            if (index < 0 || index >= length) {
                throw new IndexOutOfRangeException();
            }
            if (value) {
                bits[index >> 5] |= 1 << index;
            }
            else {
                bits[index >> 5] &= ~(1 << index);
            }
        }
    }
}
```

Einer Instanz von der `BitArray` Klasse erheblich weniger Arbeitsspeicher belegt als eine entsprechende `bool[]` (da jeder Wert der ersten nur ein Bit anstelle von belegt Letzteres des ein Byte), aber es ermöglicht die gleichen Vorgänge wie eine `bool[]`.

Die folgenden `CountPrimes` -Klasse verwendet ein `BitArray` und den klassischen "Sieb"-Algorithmus berechnet die Anzahl der Primzahlen wie folgt zwischen 1 und eine festgelegte maximale:
```csharp
class CountPrimes
{
    static int Count(int max) {
        BitArray flags = new BitArray(max + 1);
        int count = 1;
        for (int i = 2; i <= max; i++) {
            if (!flags[i]) {
                for (int j = i * 2; j <= max; j += i) flags[j] = true;
                count++;
            }
        }
        return count;
    }

    static void Main(string[] args) {
        int max = int.Parse(args[0]);
        int count = Count(max);
        Console.WriteLine("Found {0} primes between 1 and {1}", count, max);
    }
}
```

Beachten Sie, dass die Syntax für den Zugriff auf Elemente der `BitArray` ist genau dieselbe wie für eine `bool[]`.

Das folgende Beispiel zeigt eine Raster mit 26 * 10-Klasse, die einen Indexer mit zwei Parametern verfügt. Der erste Parameter ist erforderlich, um ein Groß- oder Kleinbuchstaben im Bereich von A bis Z sein, und die zweite ist erforderlich, um eine ganze Zahl im Bereich von 0 bis 9 sein.

```csharp
using System;

class Grid
{
    const int NumRows = 26;
    const int NumCols = 10;

    int[,] cells = new int[NumRows, NumCols];

    public int this[char c, int col] {
        get {
            c = Char.ToUpper(c);
            if (c < 'A' || c > 'Z') {
                throw new ArgumentException();
            }
            if (col < 0 || col >= NumCols) {
                throw new IndexOutOfRangeException();
            }
            return cells[c - 'A', col];
        }

        set {
            c = Char.ToUpper(c);
            if (c < 'A' || c > 'Z') {
                throw new ArgumentException();
            }
            if (col < 0 || col >= NumCols) {
                throw new IndexOutOfRangeException();
            }
            cells[c - 'A', col] = value;
        }
    }
}
```

### <a name="indexer-overloading"></a>Indexer überladen

Die Regeln der überladungsauflösung Indexer werden im beschrieben [Typrückschluss](expressions.md#type-inference).

## <a name="operators"></a>Operatoren

Ein ***Operator*** ist ein Member, die Bedeutung eines Operators Ausdruck definiert, die auf Instanzen der Klasse angewendet werden können. Operatoren werden mit deklariert *Operator_declaration*s:

```antlr
operator_declaration
    : attributes? operator_modifier+ operator_declarator operator_body
    ;

operator_modifier
    : 'public'
    | 'static'
    | 'extern'
    | operator_modifier_unsafe
    ;

operator_declarator
    : unary_operator_declarator
    | binary_operator_declarator
    | conversion_operator_declarator
    ;

unary_operator_declarator
    : type 'operator' overloadable_unary_operator '(' type identifier ')'
    ;

overloadable_unary_operator
    : '+' | '-' | '!' | '~' | '++' | '--' | 'true' | 'false'
    ;

binary_operator_declarator
    : type 'operator' overloadable_binary_operator '(' type identifier ',' type identifier ')'
    ;

overloadable_binary_operator
    : '+'   | '-'   | '*'   | '/'   | '%'   | '&'   | '|'   | '^'   | '<<'
    | right_shift | '=='  | '!='  | '>'   | '<'   | '>='  | '<='
    ;

conversion_operator_declarator
    : 'implicit' 'operator' type '(' type identifier ')'
    | 'explicit' 'operator' type '(' type identifier ')'
    ;

operator_body
    : block
    | '=>' expression ';'
    | ';'
    ;
```

Es gibt drei Kategorien von Überladbare Operatoren: Unäre Operatoren ([unäre Operatoren](classes.md#unary-operators)), binäre Operatoren ([binären Operatoren](classes.md#binary-operators)), und Konvertierungsoperatoren ([Konvertierungsoperatoren](classes.md#conversion-operators)).

Die *Operator_body* ist entweder ein Semikolon, ein ***Anweisungstext*** oder ***ausdruckskörper***. Eine-Anweisungstext besteht aus einem *Block*, die angibt, dass die Anweisungen, die ausgeführt werden, wenn der Operator aufgerufen wird. Die *Block* entsprechen den Regeln für die Wertrückgabe beschriebenen Methoden [Methodentext](classes.md#method-body). Ein ausdruckskörper besteht aus `=>` gefolgt von einem Ausdruck und ein Semikolon, und gibt einen einzelnen Ausdruck ausführen, wenn der Operator aufgerufen wird.

Für `extern` Operatoren, die *Operator_body* besteht einfach aus einem Semikolon. Für alle anderen Operatoren wird die *Operator_body* ist ein Blocktext oder einen ausdruckskörper.

Die folgenden Regeln gelten für alle Operatordeklarationen:

*  Eine Operatordeklaration muss enthalten beide eine `public` und `static` Modifizierer.
*  Die Parameter eines Operators muss den-Parameter ([-Wertparameter](variables.md#value-parameters)). Es ist ein Fehler während der Kompilierung für eine Operatordeklaration an `ref` oder `out` Parameter.
*  Die Signatur eines Operators ([unäre Operatoren](classes.md#unary-operators), [binären Operatoren](classes.md#binary-operators), [Konvertierungsoperatoren](classes.md#conversion-operators)) muss sich von den Signaturen aller anderen Operatoren, die im deklarierten unterscheiden die derselben Klasse.
*  Alle Typen, die auf die verwiesen wird in der Operatordeklaration eines müssen mindestens dieselben zugriffsmöglichkeiten bieten wie der Operator selbst sein ([Barrierefreiheit Einschränkungen](basic-concepts.md#accessibility-constraints)).
*  Es ist ein Fehler für den gleichen Modifizierer für mehrere Male in einem Operatordeklaration angezeigt werden.

Jeder Operatorkategorie schränkt, wie in den folgenden Abschnitten beschrieben.

Wie andere Member werden die Operatoren, die in einer Basisklasse von abgeleiteten Klassen geerbt. Da Operatordeklarationen immer erforderlich, die Klasse oder Struktur, die in der der Operator, zur Teilnahme an der Signatur des Operators deklariert wird, ist es nicht möglich, für einen Operator in einer abgeleiteten Klasse deklariert, um einen Operator in einer Basisklasse deklariert auszublenden. Daher die `new` Modifizierer ist nicht erforderlich, und daher nicht zulässig, in der Operatordeklaration eines.

Weitere Informationen zur unären und binären Operatoren finden Sie im [Operatoren](expressions.md#operators).

Weitere Informationen zu Konvertierungsoperatoren befinden sich [benutzerdefinierte Konvertierungen](conversions.md#user-defined-conversions).

### <a name="unary-operators"></a>Unäre Operatoren

Die folgenden Regeln gelten für unären Operator-Deklarationen, wobei `T` bezeichnet den Instanztyp der Klasse oder Struktur, die sich die Operatordeklaration enthält:

*  Ein unäres `+`, `-`, `!`, oder `~` Operator muss einen einzelnen Parameter vom Typ akzeptieren `T` oder `T?` und können jeden Typ zurückgeben.
*  Ein unäres `++` oder `--` Operator muss einen einzelnen Parameter vom Typ akzeptieren `T` oder `T?` und denselben Typ oder einen davon Typ abgeleiteten zurückgeben müssen.
*  Ein unäres `true` oder `false` Operator muss einen einzelnen Parameter vom Typ akzeptieren `T` oder `T?` und müssen Rückgabetyp `bool`.

Die Signatur eines unären Operators besteht aus den Operatortoken (`+`, `-`, `!`, `~`, `++`, `--`, `true`, oder `false`) und den Typ des den einzelnen formalen Parameter. Der Rückgabetyp ist nicht Teil der Signatur für einen unäroperator, noch ist der Name des formalen Parameters.

Die `true` und `false` unäre Operatoren müssen paarweise Deklaration. Ein Fehler während der Kompilierung tritt auf, wenn eine Klasse einer dieser Operatoren deklariert, ohne auch die andere zu deklarieren. Die `true` und `false` Operatoren werden ausführlich in [bedingten logischen Operatoren eine benutzerdefinierte](expressions.md#user-defined-conditional-logical-operators) und [boolesche Ausdrücke](expressions.md#boolean-expressions).

Das folgende Beispiel zeigt eine Implementierung und die anschließende Verwendung von `operator ++` für eine ganze Zahl Vector-Klasse:
```csharp
public class IntVector
{
    public IntVector(int length) {...}

    public int Length {...}                 // read-only property

    public int this[int index] {...}        // read-write indexer

    public static IntVector operator ++(IntVector iv) {
        IntVector temp = new IntVector(iv.Length);
        for (int i = 0; i < iv.Length; i++)
            temp[i] = iv[i] + 1;
        return temp;
    }
}

class Test
{
    static void Main() {
        IntVector iv1 = new IntVector(4);    // vector of 4 x 0
        IntVector iv2;

        iv2 = iv1++;    // iv2 contains 4 x 0, iv1 contains 4 x 1
        iv2 = ++iv1;    // iv2 contains 4 x 2, iv1 contains 4 x 2
    }
}
```

Beachten Sie, wie der Operator den Wert 1 mit dem Operanden, wie das Postfixinkrement zu addieren erzeugten Methodenrückgabe und Dekrement-Operatoren ([Postfix-Inkrement und Dekrement-Operatoren](expressions.md#postfix-increment-and-decrement-operators)), und die Präfix-Inkrement und Dekrement Operatoren ([Präfix-Inkrement und Dekrement-Operatoren](expressions.md#prefix-increment-and-decrement-operators)). Im Gegensatz zu muss in C++ wird diese Methode nicht den Wert des Operanden direkt ändern. Ändern den Wert des Operanden würde in der Tat die Standardsemantik des Postfixinkrement-Operators verletzt werden.

### <a name="binary-operators"></a>Binäre Operatoren

Die folgenden Regeln gelten für binären Operator-Deklarationen, wobei `T` bezeichnet den Instanztyp der Klasse oder Struktur, die sich die Operatordeklaration enthält:

*  Ein binärer nicht Shift-Operator muss zwei Parameter, die mindestens eine der Typ aufweisen einen muss annehmen `T` oder `T?`, und Sie können jeden Typ zurückgeben.
*  Ein binäres `<<` oder `>>` -Operator muss zwei Parameter, die das erste Argument Typ aufweisen einen muss annehmen `T` oder `T?` und dem zweiten muss einen Typ aufweisen `int` oder `int?`, und Sie können jeden Typ zurückgeben.

Die Signatur eines binären Operators besteht aus den Operatortoken (`+`, `-`, `*`, `/`, `%`, `&`, `|`, `^`, `<<`, `>>`, `==`, `!=`, `>`, `<`, `>=`, oder `<=`) und die Typen der formalen Parameter zwei. Der Rückgabetyp und die Namen der formalen Parameter sind nicht Teil eines binären Operators-Signatur.

Bestimmte binären Operatoren müssen paarweise Deklaration. Für jede Deklaration entweder ein paar-Operators muss eine entsprechende Deklaration des anderen Operators des Paars vorhanden sein. Zwei Operatordeklarationen Abgleich aus, wenn sie den gleichen Rückgabetyp und den gleichen Typ für jeden Parameter aufweisen. Die folgenden Operatoren müssen paarweise Deklaration:

*  `operator ==` und `operator !=`
*  `operator >` und `operator <`
*  `operator >=` und `operator <=`

### <a name="conversion-operators"></a>Konvertierungsoperatoren

Eine Konvertierung Operator-Deklaration führt eine ***benutzerdefinierte Konvertierung*** ([benutzerdefinierte Konvertierungen](conversions.md#user-defined-conversions)) das Standardfehlerprotokoll erweitert, der vordefinierten impliziten und expliziten Konvertierungen.

Eine Konvertierung Operator-Deklaration, enthält die `implicit` -Schlüsselwort Führt eine implizite Konvertierung von benutzerdefinierten. Implizite Konvertierungen können in einer Vielzahl von Situationen, wie z.B. Memberaufrufen für die Funktion Cast-Ausdrücke und Zuweisungen auftreten. Dies wird weiter in [implizite Konvertierungen](conversions.md#implicit-conversions).

Eine Konvertierung Operator-Deklaration, enthält die `explicit` -Schlüsselwort Führt eine benutzerdefinierte explizite Konvertierung. Explizite Konvertierungen können in der Cast-Ausdrücke auftreten, und werden ausführlich in [explizite Konvertierungen](conversions.md#explicit-conversions).

Ein Konvertierungsoperator konvertiert aus einer Datenquelle, die von der Parametertyp des Konvertierungsoperators in einen Zieltyp, angegeben durch den Rückgabetyp der Operator für die Konvertierung angegeben.

Für einen bestimmten Quell- `S` und einen Zieltyp `T`, wenn `S` oder `T` werden auf NULL festlegbare Typen können `S0` und `T0` finden Sie in die zugrunde liegende Typen, andernfalls `S0` und `T0` sind gleich `S` und `T` bzw. Eine Klasse oder Struktur ist zulässig, deklarieren Sie eine Konvertierung aus einer Datenquelle `S` in einen Zieltyp `T` nur dann, wenn alle der folgenden Bedingungen erfüllt sind:

*  `S0` und `T0` gibt verschiedene Typen.
*  Entweder `S0` oder `T0` ist der Typ Klasse oder Struktur, die in der die Operatordeklaration erfolgt.
*  Weder `S0` noch `T0` ist ein *Interface_type*.
*  Mit Ausnahme von benutzerdefinierten Konvertierungen, eine Konvertierung ist nicht vom `S` zu `T` oder `T` zu `S`.

Im Rahmen dieser Regeln werden alle zugeordneten Parameter geben `S` oder `T` gelten als eindeutige Typen, die keine vererbungsbeziehung mit anderen Typen, und alle Einschränkungen für diesen Typ mit dem Parameter werden ignoriert.

Im Beispiel
```csharp
class C<T> {...}

class D<T>: C<T>
{
    public static implicit operator C<int>(D<T> value) {...}      // Ok
    public static implicit operator C<string>(D<T> value) {...}   // Ok
    public static implicit operator C<T>(D<T> value) {...}        // Error
}
```
die ersten zwei Operatordeklarationen sind zulässig, da für die Zwecke [Indexer](classes.md#indexers).3 und `T` und `int` und `string` gelten jeweils eindeutige Typen ohne Beziehung. Der dritte Operator ist jedoch ein Fehler, da `C<T>` ist die Basisklasse der `D<T>`.

Ein Konvertierungsoperator muss aus der zweiten Regel ergibt sich, dass konvertieren, auf ein oder von der Klasse oder Struktur-Typ, in dem der Operator deklariert ist. Beispielsweise ist es möglich, dass eine Klasse oder Struktur-Typ `C` definieren Sie eine Konvertierung von `C` zu `int` und `int` zu `C`, jedoch nicht von `int` zu `bool`.

Es ist nicht möglich, um eine vordefinierte Konvertierung direkt neu zu definieren. Konvertierungsoperatoren sind daher nicht zulässig, Konvertieren von oder zu `object` Da implizite und explizite Konvertierungen zwischen bereits vorhanden sind `object` und alle anderen Typen. Ebenso können weder Quelle noch die Zieltypen einer Konvertierung sein, ein Basistyp des anderen, da eine Konvertierung, klicken Sie dann noch vorhanden wäre.

Allerdings ist es möglich, die Operatoren in generischen Typen deklarieren, die bei bestimmten Typargumenten, geben Sie als vordefinierte Konvertierungen Konvertierungen, die bereits vorhanden. Im Beispiel
```csharp
struct Convertible<T>
{
    public static implicit operator Convertible<T>(T value) {...}
    public static explicit operator T(Convertible<T> value) {...}
}
```
Geben Sie bei `object` angegeben ist, als Typargument für `T`, der zweite Operator deklariert eine Konvertierung, die bereits vorhanden ist (eine implizite, und daher auch eine explizite Konvertierung von jedem Typ vorhanden ist `object`).

In Fällen, in dem eine vordefinierte Konvertierung zwischen zwei Typen vorhanden ist, werden keine benutzerdefinierten Konvertierungen zwischen diesen Typen ignoriert. Dies gilt insbesondere in folgenden Fällen:

*  Wenn eine vordefinierte implizite Konvertierung ([implizite Konvertierungen](conversions.md#implicit-conversions)) vom Typ `S` eingeben `T`, alle benutzerdefinierten Konvertierungen (implizit oder explizit) von `S` zu `T` werden ignoriert.
*  Wenn die vordefinierten explizite Konvertierung ([explizite Konvertierungen](conversions.md#explicit-conversions)) vom Typ `S` eingeben `T`, eine benutzerdefinierte, explizite Konvertierung von `S` zu `T` werden ignoriert. Darüber hinaus:

Wenn `T` ist ein Schnittstellentyp, der den benutzerdefinierten impliziten Konvertierungen von `S` zu `T` werden ignoriert.

Andernfalls, eine benutzerdefinierte impliziten Konvertierungen von `S` zu `T` gelten weiterhin.

Für alle Typen jedoch `object`, die Operatoren deklariert, indem die `Convertible<T>` oben genannten Typ nicht in Konflikt mit vordefinierten Konvertierungen. Zum Beispiel:
```csharp
void F(int i, Convertible<int> n) {
    i = n;                          // Error
    i = (int)n;                     // User-defined explicit conversion
    n = i;                          // User-defined implicit conversion
    n = (Convertible<int>)i;        // User-defined implicit conversion
}
```

Für den Typ jedoch `object`, vordefinierte Konvertierungen ausblenden, die eine benutzerdefinierte Konvertierungen in allen Fällen jedoch eine:

```csharp
void F(object o, Convertible<object> n) {
    o = n;                         // Pre-defined boxing conversion
    o = (object)n;                 // Pre-defined boxing conversion
    n = o;                         // User-defined implicit conversion
    n = (Convertible<object>)o;    // Pre-defined unboxing conversion
}
```

Benutzerdefinierte Konvertierungen sind nicht zulässig, Konvertieren von oder zu *Interface_type*s. Insbesondere diese Beschränkung wird sichergestellt, dass keine benutzerdefinierten Transformationen erfolgen, bei der Konvertierung in eine *Interface_type*, und dass eine Konvertierung in eine *Interface_type* nur erfolgreich, wenn das Objekt konvertierte tatsächlich implementiert den angegebenen *Interface_type*.

Die Signatur eines Konvertierungsoperators besteht aus den Quelltyp und der Zieltyp. (Beachten Sie, dass dies die einzige Form des Elements ist der Rückgabetyp für die in der Signatur beteiligt ist.) Die `implicit` oder `explicit` Klassifizierung eines Konvertierungsoperators ist nicht Teil der Signatur des Operators. Daher eine Klasse oder Struktur kann nicht deklariert sowohl eine `implicit` und `explicit` Konvertierungsoperator mit den gleichen Typen von Quelle und Ziel.

Im Allgemeinen sollten implizite Konvertierungen benutzerdefinierte entworfen werden, keine Ausnahmen auslösen und keine Informationen verlieren. Wenn eine benutzerdefinierte Konvertierung Anstieg auf Ausnahmen erhalten (z. B. weil das Quellargument außerhalb des gültigen Bereichs ist) oder Verlust von Informationen (z. B. das höherwertige Bits werden verworfen), und klicken Sie dann auf diese Konvertierung als eine explizite Konvertierung definiert werden sollte.

Im Beispiel
```csharp
using System;

public struct Digit
{
    byte value;

    public Digit(byte value) {
        if (value < 0 || value > 9) throw new ArgumentException();
        this.value = value;
    }

    public static implicit operator byte(Digit d) {
        return d.value;
    }

    public static explicit operator Digit(byte b) {
        return new Digit(b);
    }
}
```
die Konvertierung von `Digit` zu `byte` ist implizit, da sie nie Ausnahmen auslöst oder Informationen, aber die Konvertierung von verliert `byte` zu `Digit` ist seit explizit `Digit` können nur eine Teilmenge der möglichen darstellen Werte von einem `byte`.

## <a name="instance-constructors"></a>Instanzkonstruktoren

Ein ***Instanzkonstruktor*** ist ein Member, der die erforderlichen Aktionen zum Initialisieren einer Instanz einer Klasse implementiert. Instanzkonstruktoren werden mit deklariert *Constructor_declaration*s:

```antlr
constructor_declaration
    : attributes? constructor_modifier* constructor_declarator constructor_body
    ;

constructor_modifier
    : 'public'
    | 'protected'
    | 'internal'
    | 'private'
    | 'extern'
    | constructor_modifier_unsafe
    ;

constructor_declarator
    : identifier '(' formal_parameter_list? ')' constructor_initializer?
    ;

constructor_initializer
    : ':' 'base' '(' argument_list? ')'
    | ':' 'this' '(' argument_list? ')'
    ;

constructor_body
    : block
    | ';'
    ;
```

Ein *Constructor_declaration* eventuell einen Satz von *Attribute* ([Attribute](attributes.md)), eine gültige Kombination der vier Zugriffsmodifizierer ([Zugriffsmodifizierer ](classes.md#access-modifiers)), und ein `extern` ([externe Methoden](classes.md#external-methods)) Modifizierer. Eine Konstruktordeklaration verwendet darf nicht den gleichen Modifizierer mehrfach enthalten.

Die *Bezeichner* von einem *Constructor_declarator* müssen den Namen der Klasse, die in der die Instanzkonstruktor deklariert wird. Wenn ein anderen Namen angegeben wird, tritt ein Fehler während der Kompilierung.

Der optionale *Formal_parameter_list* einer Instanz unterliegt Konstruktor dieselben Regeln wie für die *Formal_parameter_list* einer Methode ([Methoden](classes.md#methods)). Liste der formalen Parameter definiert die Signatur ([Signaturen und überladen](basic-concepts.md#signatures-and-overloading)) eines Instanzkonstruktors und steuert den Prozess, bei dem der überladungsauflösung ([Typrückschluss](expressions.md#type-inference)) Wählt eine bestimmte Bei einem Aufruf Instance-Konstruktor.

Alle Typen verwiesen wird, der *Formal_parameter_list* einer Instanz-Konstruktor muss mindestens dieselben zugriffsmöglichkeiten bieten wie der Konstruktor selbst sein ([Barrierefreiheit Einschränkungen](basic-concepts.md#accessibility-constraints)).

Der optionale *Constructor_initializer* gibt an, eine andere Instanzkonstruktor aufrufen, die vor dem Ausführen der Anweisungen, die im angegebenen die *Constructor_body* dieses Konstruktors Instanz. Dies wird weiter in [Konstruktorinitialisierer](classes.md#constructor-initializers).

Wenn eine Konstruktordeklaration verwendet enthält ein `extern` Modifizierer, der Konstruktor wird als ein ***externer Konstruktor***. Da eine externe Konstruktordeklaration keine Implementierungen, bietet die *Constructor_body* besteht aus einem Semikolon. Für alle übrigen Konstruktoren die *Constructor_body* besteht aus einem *Block* der gibt an, die Anweisungen, um eine neue Instanz der Klasse zu initialisieren. Dies entspricht genau dem der *Block* einer Instanzmethode mit einem `void` Rückgabetyp ([Methodentext](classes.md#method-body)).

Instanzkonstruktoren werden nicht geerbt. Daher verfügt eine Klasse keine Instanzkonstruktoren auf als die tatsächlich in der Klasse deklariert wurden. Wenn eine Klasse keine Instanz-Deklarationen enthält, wird ein Standardkonstruktor für die Instanz wird automatisch bereitgestellt ([Standardkonstruktoren](classes.md#default-constructors)).

Instanzkonstruktoren werden aufgerufen, indem *Object_creation_expression*s ([Erstellung Objektausdrücke](expressions.md#object-creation-expressions)) und über *Constructor_initializer*s.

### <a name="constructor-initializers"></a>Konstruktorinitialisierer

Alle Instanzkonstruktoren (mit Ausnahme für die Klasse `object`) sind implizit einen Aufruf des Instanzkonstruktors für eine andere unmittelbar vor der *Constructor_body*. Der Konstruktor, der implizit aufgerufen richtet sich nach der *Constructor_initializer*:

*  Ein Instanz Konstruktorinitialisierer des Formulars `base(argument_list)` oder `base()` bewirkt, dass ein Instanzkonstruktor aus der direkten Basisklasse aufgerufen werden. Dieser Konstruktor wird mit ausgewählt *Argument_list* wenn vorhanden und die Regeln der überladungsauflösung des [Überladungsauflösung](expressions.md#overload-resolution). Der Satz von Kandidaten Instanzkonstruktoren besteht aus allen verfügbaren Instanzkonstruktoren, die in der direkten Basisklasse enthalten oder die Standard-Konstruktor ([Standardkonstruktoren](classes.md#default-constructors)), wenn keine Instanzkonstruktoren, in deklariert sind der direkte Basisklasse. Wenn dieser Satz leer ist, oder ein einzelnen bewährte Instanzenkonstruktor nicht identifiziert werden kann, tritt ein Fehler während der Kompilierung.
*  Ein Instanz Konstruktorinitialisierer des Formulars `this(argument-list)` oder `this()` bewirkt, dass ein Instanzkonstruktor aus der Klasse selbst aufgerufen werden. Der Konstruktor ausgewählt ist, mithilfe von *Argument_list* wenn vorhanden und die Regeln der überladungsauflösung des [Überladungsauflösung](expressions.md#overload-resolution). Der Satz von Kandidaten Instanzkonstruktoren besteht aus allen verfügbaren Instanzkonstruktoren, die in der Klasse selbst deklariert. Wenn dieser Satz leer ist, oder ein einzelnen bewährte Instanzenkonstruktor nicht identifiziert werden kann, tritt ein Fehler während der Kompilierung. Wenn eine Instanzkonstruktordeklaration einem Konstruktorinitialisierer, die den Konstruktor selbst aufruft enthält, tritt ein Fehler während der Kompilierung.

Verfügt ein Instanzkonstruktor ohne Konstruktorinitialisierer, einem Konstruktorinitialisierer des Formulars `base()` wird impliziert bereitgestellt. Daher eine Instanzkonstruktordeklaration des Formulars
```csharp
C(...) {...}
```
ist genau gleich
```csharp
C(...): base() {...}
```

Der Bereich der angegebenen Parameter die *Formal_parameter_list* eines Instanzkonstruktors Deklaration konstruktorinitialisierers diese Deklaration enthält. Daher ist einem Konstruktorinitialisierer auf die Parameter des Konstruktors zulässig. Zum Beispiel:
```csharp
class A
{
    public A(int x, int y) {}
}

class B: A
{
    public B(int x, int y): base(x + y, x - y) {}
}
```

Ein Konstruktorinitialisierer Instanz kann nicht zu erstellende Instanz zugreifen. Daher ist es ein Fehler während der Kompilierung auf `this` in ein Argumentausdruck des konstruktorinitialisierers, wie ist es ein Fehler während der Kompilierung ein Argumentausdruck auf beliebiger Instanzmember über eine *Simple_name*.

### <a name="instance-variable-initializers"></a>Instanz Variableninitialisierern

Wenn ein Instanzkonstruktor hat keinen Konstruktor/Initialisierer aufweisen oder einem Konstruktorinitialisierer des Formulars kann `base(...)`, dieser Konstruktor führt implizit die Initialisierungen, die gemäß der *Variable_initializer*s die Instanzfelder werden in der Klasse deklariert. Dies entspricht einer Sequenz von Zuweisungen, die bei der Eingabe an den Konstruktor und vor den impliziten Aufruf des Konstruktors der direkten Basisklasse sofort ausgeführt werden. Die Variable Initialisierer werden in der Reihenfolge im Text ausgeführt in denen sie in der Klassendeklaration vorkommen.

### <a name="constructor-execution"></a>Konstruktor-Ausführung

Variableninitialisierern in zuweisungsanweisungen transformiert werden, und diese Anweisungen werden ausgeführt, bevor Sie den Aufruf des Konstruktors der Basisklasse-Instanz. Diese Reihenfolge wird sichergestellt, dass alle Instanzenfelder von ihren Variableninitialisierern initialisiert werden, bevor alle Anweisungen, die Zugriff auf diese Instanz ausgeführt werden.

Im Beispiel angegeben
```csharp
using System;

class A
{
    public A() {
        PrintFields();
    }

    public virtual void PrintFields() {}
}

class B: A
{
    int x = 1;
    int y;

    public B() {
        y = -1;
    }

    public override void PrintFields() {
        Console.WriteLine("x = {0}, y = {1}", x, y);
    }
}
```
Wenn `new B()` dient zum Erstellen einer Instanz von `B`, wird die folgende Ausgabe generiert:
```
x = 1, y = 0
```

Der Wert des `x` ist 1, da die Variableninitialisierer ausgeführt wird, bevor der Konstruktor der Basisklasse-Instanz aufgerufen wird. Jedoch den Wert der `y` ist 0 (standardmäßig den Wert ein `int`) da die Zuweisung zu `y` wird erst ausgeführt, nachdem Konstruktor der Basisklasse zurückgegeben.

Es ist hilfreich, die Instanz Variableninitialisierern und Konstruktorinitialisierer wie Anweisungen vorstellen, die automatisch, bevor eingefügt werden die *Constructor_body*. Im Beispiel
```csharp
using System;
using System.Collections;

class A
{
    int x = 1, y = -1, count;

    public A() {
        count = 0;
    }

    public A(int n) {
        count = n;
    }
}

class B: A
{
    double sqrt2 = Math.Sqrt(2.0);
    ArrayList items = new ArrayList(100);
    int max;

    public B(): this(100) {
        items.Add("default");
    }

    public B(int n): base(n - 1) {
        max = n;
    }
}
```
enthält mehrere Variableninitialisierern; Es enthält auch eine Konstruktor-Initialisierungen der beiden Formen (`base` und `this`). Das Beispiel entspricht dem folgenden Code, wobei jeder Kommentar eine automatisch eingefügte Anweisung gibt an, (die Syntax für die Konstruktoraufrufe automatisch eingefügte ist nicht gültig, aber dient lediglich zur Veranschaulichung des Mechanismus).

```csharp
using System.Collections;

class A
{
    int x, y, count;

    public A() {
        x = 1;                       // Variable initializer
        y = -1;                      // Variable initializer
        object();                    // Invoke object() constructor
        count = 0;
    }

    public A(int n) {
        x = 1;                       // Variable initializer
        y = -1;                      // Variable initializer
        object();                    // Invoke object() constructor
        count = n;
    }
}

class B: A
{
    double sqrt2;
    ArrayList items;
    int max;

    public B(): this(100) {
        B(100);                      // Invoke B(int) constructor
        items.Add("default");
    }

    public B(int n): base(n - 1) {
        sqrt2 = Math.Sqrt(2.0);      // Variable initializer
        items = new ArrayList(100);  // Variable initializer
        A(n - 1);                    // Invoke A(int) constructor
        max = n;
    }
}
```

### <a name="default-constructors"></a>Standardkonstruktoren

Wenn eine Klasse keine Instanz-Deklarationen enthält, wird automatisch ein Standardkonstruktor für die Instanz bereitgestellt. Diese Standard-Konstruktor ruft einfach den parameterlosen Konstruktor der direkten Basisklasse. Wenn die Klasse abstrakt ist, ist dann die deklarierte Zugriffsart für den Standardkonstruktor geschützt. Andernfalls ist die deklarierte Zugriffsart für den Standardkonstruktor öffentlich. Ist daher der Standardkonstruktor immer im Format

```csharp
protected C(): base() {}
```
oder
```csharp
public C(): base() {}
```
wo `C` ist der Name der Klasse. Wenn die Auflösung von funktionsüberladungen kann nicht auf einen eindeutige besten Kandidaten für die Basisklasse konstruktorinitialisierers zu bestimmen, und klicken Sie dann ein Fehler während der Kompilierung auftritt.

Im Beispiel
```csharp
class Message
{
    object sender;
    string text;
}
```
ein Standardkonstruktor bereitgestellt, da die Klasse keine Instanz-Deklarationen enthält. Daher ist das Beispiel genaue Entsprechung
```csharp
class Message
{
    object sender;
    string text;

    public Message(): base() {}
}
```

### <a name="private-constructors"></a>Private Konstruktoren

Wenn eine Klasse `T` nur private Instanzkonstruktoren deklariert, ist nicht möglich, dass Klassen außerhalb der Programmtext des `T` für die Ableitung `T` oder direkt erstellen von Instanzen von `T`. Daher, wenn eine Klasse nur statische Member enthält und wird nicht instanziiert werden soll, Hinzufügen einer leeren, privaten Instanzkonstruktor Instanziierung verhindert. Zum Beispiel:
```csharp
public class Trig
{
    private Trig() {}        // Prevent instantiation

    public const double PI = 3.14159265358979323846;

    public static double Sin(double x) {...}
    public static double Cos(double x) {...}
    public static double Tan(double x) {...}
}
```

Die `Trig` Klasse Gruppen von verwandten Methoden und Konstanten, aber nicht instanziiert werden soll. Aus diesem Grund deklariert einen leeren privaten Instanzkonstruktor. Mindestens ein Instanzkonstruktor muss deklariert werden, um die automatische Generierung des Standardkonstruktors zu unterdrücken.

### <a name="optional-instance-constructor-parameters"></a>Optionale Instanz Konstruktorparameter

Die `this(...)` Form der Konstruktorinitialisierer wird meist in Verbindung mit der Überladung um optionale Instanz Konstruktorparameter zu implementieren. Im Beispiel
```csharp
class Text
{
    public Text(): this(0, 0, null) {}

    public Text(int x, int y): this(x, y, null) {}

    public Text(int x, int y, string s) {
        // Actual constructor implementation
    }
}
```
die ersten zwei Instanzkonstruktoren geben lediglich die Standardwerte für die fehlenden Argumente an. Beide verwenden eine `this(...)` Konstruktor/Initialisierer aufweisen, den dritten Instanzkonstruktor aufrufen, sodass die Arbeit wird die neue Instanz initialisiert wird. Der Effekt ist der optionale Konstruktorparameter:
```csharp
Text t1 = new Text();                    // Same as Text(0, 0, null)
Text t2 = new Text(5, 10);               // Same as Text(5, 10, null)
Text t3 = new Text(5, 20, "Hello");
```

## <a name="static-constructors"></a>Statische Konstruktoren

Ein ***statischen Konstruktor*** ist ein Element, das die erforderlichen Aktionen zum Initialisieren eines geschlossenen Klassentyps implementiert. Statische Konstruktoren deklariert werden, mithilfe von *Static_constructor_declaration*s:

```antlr
static_constructor_declaration
    : attributes? static_constructor_modifiers identifier '(' ')' static_constructor_body
    ;

static_constructor_modifiers
    : 'extern'? 'static'
    | 'static' 'extern'?
    | static_constructor_modifiers_unsafe
    ;

static_constructor_body
    : block
    | ';'
    ;
```

Ein *Static_constructor_declaration* eventuell einen Satz von *Attribute* ([Attribute](attributes.md)) und ein `extern` Modifizierer ([externe Methoden](classes.md#external-methods)).

Die *Bezeichner* von einem *Static_constructor_declaration* müssen den Namen der Klasse, die in der der statische Konstruktor deklariert wird. Wenn ein anderen Namen angegeben wird, tritt ein Fehler während der Kompilierung.

Wenn eine statische Destruktordeklaration enthält ein `extern` Modifizierer, der statische Konstruktor gilt eine ***externen statischen Konstruktor***. Da eine externe statische Destruktordeklaration keine Implementierungen, bietet die *Static_constructor_body* besteht aus einem Semikolon. Für alle anderen statischen Konstruktor-Deklarationen die *Static_constructor_body* besteht aus einem *Block* der gibt an, die Anweisungen ausführen, um die Klasse zu initialisieren. Dies entspricht genau der *Method_body* einer statischen Methode mit einer `void` Rückgabetyp ([Methodentext](classes.md#method-body)).

Statische Konstruktoren nicht vererbt werden und nicht direkt aufgerufen werden.

Der statische Konstruktor für einen geschlossenen Klassentyp wird höchstens einmal in einer bestimmten Anwendungsdomäne ausgeführt. Die Ausführung eines statischen Konstruktors wird durch das erste der folgenden Ereignisse innerhalb einer Anwendungsdomäne ausgelöst:

*  Eine Instanz des Klassentyps wird erstellt.
*  Einer der statischen Member des Klassentyps sind auf die verwiesen wird.

Wenn eine Klasse enthält die `Main` Methode ([Anwendungsstart](basic-concepts.md#application-startup)) in die die Ausführung beginnt, den statischen Konstruktor für diese Klasse vor ausgeführt wird die `Main` Methode wird aufgerufen.

Zum Initialisieren Sie eines neuen geschlossene Klassentyps, zunächst eines neuen Satz von statischen Feldern ([statische Felder und Instanzfelder](classes.md#static-and-instance-fields)) für den gewählten geschlossen wurde. Jedes der statische Felder, die auf den Standardwert initialisiert wird ([Standardwerte](variables.md#default-values)). Anschließend wird die statischen Feldinitialisierer ([Initialisierung statischer Felder](classes.md#static-field-initialization)) für die statische Felder ausgeführt werden. Schließlich wird der statische Konstruktor ausgeführt.

Im Beispiel
```csharp
using System;

class Test
{
    static void Main() {
        A.F();
        B.F();
    }
}

class A
{
    static A() {
        Console.WriteLine("Init A");
    }
    public static void F() {
        Console.WriteLine("A.F");
    }
}

class B
{
    static B() {
        Console.WriteLine("Init B");
    }
    public static void F() {
        Console.WriteLine("B.F");
    }
}
```
muss die Ausgabe erzeugen:
```
Init A
A.F
Init B
B.F
```
Da die Ausführung von `A`des statischer Konstruktor wird ausgelöst, durch den Aufruf von `A.F`, und die Ausführung des `B`des statischer Konstruktor wird ausgelöst, durch den Aufruf von `B.F`.

Es ist möglich, die ringabhängigkeiten erstellen, mit denen statische Felder mit Variableninitialisierern an, die im Standardzustand Wert beachtet werden.

Im Beispiel
```csharp
using System;

class A
{
    public static int X;

    static A() {
        X = B.Y + 1;
    }
}

class B
{
    public static int Y = A.X + 1;

    static B() {}

    static void Main() {
        Console.WriteLine("X = {0}, Y = {1}", A.X, B.Y);
    }
}
```
erzeugt die Ausgabe
```
X = 1, Y = 2
```

Zum Ausführen der `Main` -Methode, führt das System zunächst den Initialisierer für `B.Y`vor der Klasse `B`des statischen Konstruktor. `Y`die führt dazu, dass der Initialisierer `A`des statischen Konstruktor verfügt, ausgeführt werden, da der Wert des `A.X` verwiesen wird. Der statische Konstruktor des `A` wiederum geht, der Berechnung des Werts der `X`, und dies der Fall ist, ruft des Standardwerts von `Y`, d.h. 0 (null). `A.X` 1 wird so initialisiert werden. Der Prozess zur Ausführung `A`des statischen Feldinitialisierer und statischen Konstruktor, und klicken Sie dann abgeschlossen ist, und die Berechnung des Anfangswerts der `Y`, deren Ergebnis ist 2.

Da der statische Konstruktor genau einmal ausgeführt wird, für jede erstellte Klassentyp geschlossen, es ist eine bequeme Möglichkeit, Überprüfungen zur Laufzeit für den Typparameter zu erzwingen, die zum Zeitpunkt der Kompilierung über die Einschränkungen nicht überprüft werden kann ([Typparameter Einschränkungen](classes.md#type-parameter-constraints)). Beispielsweise verwendet der folgende Typ einen statischen Konstruktor erzwingen, dass das Typargument eine Enumeration ist:
```csharp
class Gen<T> where T: struct
{
    static Gen() {
        if (!typeof(T).IsEnum) {
            throw new ArgumentException("T must be an enum");
        }
    }
}
```

## <a name="destructors"></a>Destruktoren

Ein ***Destruktor*** ist ein Element, das die erforderlichen Aktionen zum zerstört einer Instanz einer Klasse implementiert. Ein Destruktor deklariert wird, mithilfe einer *Destructor_declaration*:

```antlr
destructor_declaration
    : attributes? 'extern'? '~' identifier '(' ')' destructor_body
    | destructor_declaration_unsafe
    ;

destructor_body
    : block
    | ';'
    ;
```

Ein *Destructor_declaration* eventuell einen Satz von *Attribute* ([Attribute](attributes.md)).

Die *Bezeichner* von einem *Destructor_declaration* müssen den Namen der Klasse, die in der der Destruktor deklariert wird. Wenn ein anderen Namen angegeben wird, tritt ein Fehler während der Kompilierung.

Wenn eine Destruktordeklaration enthält eine `extern` Modifizierer, der Destruktor gilt eine ***externe Destruktor***. Da eine externe Destruktordeklaration keine Implementierungen, bietet die *Destructor_body* besteht aus einem Semikolon. Für alle anderen Destruktoren der *Destructor_body* besteht aus einem *Block* der gibt an, die Anweisungen ausführen, um eine Instanz der Klasse zerstört. Ein *Destructor_body* entspricht genau der *Method_body* einer Instanzmethode mit einer `void` Rückgabetyp ([Methodentext](classes.md#method-body)).

Destruktoren werden nicht geerbt. Daher verfügt über eine Klasse keine Destruktoren, die in dieser Klasse deklariert werden kann.

Da Sie ein Destruktor ohne Parameter erforderlich ist, darf nicht überladene, sein, damit eine Klasse, höchstens einen Destruktor verfügen kann.

Destruktoren werden automatisch aufgerufen und können nicht explizit aufgerufen werden. Eine Instanz wird zerstört, wenn er nicht mehr möglich, dass jeder Code, verwenden Sie diese Instanz ist. Der Destruktor für die Instanz kann auftreten, zu einem beliebigen Zeitpunkt nach dem die Instanz für die Zerstörung freigegeben wird. Wenn eine Instanz zerstört wird, die Destruktoren in diese Instanz die Vererbungskette werden aufgerufen, in der Reihenfolge, die meisten abgeleitet, um am wenigsten abgeleiteten. Ein Destruktor kann auf jedem Thread ausgeführt werden. Weitere Informationen zu den Regeln, die steuern, wann und wie ein Destruktor ausgeführt wird, finden Sie unter [automatische Speicherverwaltung](basic-concepts.md#automatic-memory-management).

Die Ausgabe des Beispiels
```csharp
using System;

class A
{
    ~A() {
        Console.WriteLine("A's destructor");
    }
}

class B: A
{
    ~B() {
        Console.WriteLine("B's destructor");
    }
}

class Test
{
   static void Main() {
        B b = new B();
        b = null;
        GC.Collect();
        GC.WaitForPendingFinalizers();
   }
}
```
echt
```
B's destructor
A's destructor
```
da Destruktoren in einer Vererbungskette nacheinander aufgerufen werden die meisten abgeleitet, um am wenigsten abgeleiteten.

Destruktoren werden durch Überschreiben der virtuellen Methode implementiert `Finalize` auf `System.Object`. C#-Programme sind nicht zulässig, diese Methode überschreiben, oder rufen sie (oder überschreibt es) direkt. Beispielsweise das Programm
```csharp
class A 
{
    override protected void Finalize() {}    // error

    public void F() {
        this.Finalize();                     // error
    }
}
```
enthält zwei Fehler an.

Der Compiler verhält sich wie diese Methode, und klicken Sie auf Außerkraftsetzungen, überhaupt nicht vorhanden sind. Daher ist dieses Programm:
```csharp
class A 
{
    void Finalize() {}                            // permitted
}
```
ist gültig, und die Methode blendet `System.Object`des `Finalize` Methode.

Eine Erläuterung des Verhaltens bei der von einem Destruktor eine Ausnahme ausgelöst wird, finden Sie unter [Behandlung von Ausnahmen](exceptions.md#how-exceptions-are-handled).

## <a name="iterators"></a>Iterators

Ein Funktionsmember ([Funktionsmember](expressions.md#function-members)) mit einem Iteratorblock implementiert ([Blöcke](statements.md#blocks)) wird aufgerufen, eine ***Iterator***.

Kein Iteratorblock kann als Text ein Funktionsmember verwendet werden, solange der Rückgabetyp des entsprechenden Elements Funktion eine der Enumeratorschnittstellen ([Enumeratorschnittstellen](classes.md#enumerator-interfaces)) mindestens die enumerable-Schnittstellen ([Enumerable-Schnittstellen](classes.md#enumerable-interfaces)). Es ist möglich, als eine *Method_body*, *Operator_body* oder *Accessor_body*, während Ereignisse, Instanzkonstruktoren, statische Konstruktoren und Destruktoren nicht möglich als Iteratoren implementiert.

Wenn ein Funktionsmember mit Iteratorblock implementiert ist, es ist ein Fehler während der Kompilierung für die Liste der formalen Parameter, der das Funktionselement zum angeben `ref` oder `out` Parameter.

### <a name="enumerator-interfaces"></a>Enumerator-Schnittstellen

Die ***Enumeratorschnittstellen*** sind die nicht generische Schnittstelle `System.Collections.IEnumerator` und alle Instanziierungen an der die generische Schnittstelle `System.Collections.Generic.IEnumerator<T>`. Aus Gründen der Übersichtlichkeit sind in diesem Kapitel diese Schnittstellen werden auf die verwiesen wird als `IEnumerator` und `IEnumerator<T>`bzw.

### <a name="enumerable-interfaces"></a>Aufzählbare Schnittstellen

Die ***enumerable-Schnittstellen*** sind die nicht generische Schnittstelle `System.Collections.IEnumerable` und alle Instanziierungen an der die generische Schnittstelle `System.Collections.Generic.IEnumerable<T>`. Aus Gründen der Übersichtlichkeit sind in diesem Kapitel diese Schnittstellen werden auf die verwiesen wird als `IEnumerable` und `IEnumerable<T>`bzw.

### <a name="yield-type"></a>Yield-Typ

Ein Iterator erzeugt eine Sequenz von Werten, alle den gleichen Typ aufweisen. Dieser Typ wird aufgerufen, die ***yield-Typ*** des Iterators.

*  Die Yield-Typ, der ein Iterator, der gibt `IEnumerator` oder `IEnumerable` ist `object`.
*  Die Yield-Typ, der ein Iterator, der gibt `IEnumerator<T>` oder `IEnumerable<T>` ist `T`.

### <a name="enumerator-objects"></a>Enumerator-Objekte

Wenn ein Funktionsmember ein Enumerator zurückgegeben, der den Typ mit einem Iteratorblock implementiert ist, wird das Funktionselement Aufrufen nicht sofort den Code im Iteratorblock ausgeführt. Stattdessen eine ***Enumeratorobjekt*** erstellt und zurückgegeben. Dieses Objekt kapselt den Code im Iteratorblock angegeben und Ausführung des Codes im Iteratorblock tritt auf, wenn des Enumeratorobjekts `MoveNext` -Methode wird aufgerufen. Ein Enumeratorobjekt weist folgende Merkmale auf:

*  Es implementiert `IEnumerator` und `IEnumerator<T>`, wobei `T` ist der Yield-Typ des Iterators.
*  Sie implementiert `System.IDisposable`.
*  Initialisiert mit einer Kopie die Argumentwerte gelten (sofern vorhanden) und Instanzwert an das Funktionselement übergeben.
*  Hat vier mögliche Zustände, ***vor***, ***ausführen***, ***angehalten***, und ***nach***, und befindet sich zunächst im der ***vor***  Zustand.

Ein Enumeratorobjekt ist in der Regel eine Instanz einer vom Compiler generierter Enumerator-Klasse, die den Code im Iteratorblock kapselt und die Enumeratorschnittstellen implementiert, aber andere Methoden der Implementierung sind möglich. Wenn eine Enumeratorklasse, die vom Compiler generiert wird, wird diese Klasse geschachtelt werden, direkt oder indirekt in der Klasse, die mit das Funktionselement muss private zugriffsmöglichkeiten und muss einen Namen für die Verwendung durch den Compiler reserviert ([Bezeichner ](lexical-structure.md#identifiers)).

Ein Enumeratorobjekt kann mehrere Schnittstellen als die oben angegebenen implementieren.

Die folgenden Abschnitte beschreiben das genaue Verhalten der der `MoveNext`, `Current`, und `Dispose` Mitglied der `IEnumerable` und `IEnumerable<T>` schnittstellenimplementierungen, die von einem Enumeratorobjekt bereitgestellt.

Beachten Sie, dass der Enumerator-Objekte nicht unterstützen die `IEnumerator.Reset` Methode. Das Aufrufen dieser Methode bewirkt, dass eine `System.NotSupportedException` ausgelöst wird.

#### <a name="the-movenext-method"></a>Die MoveNext-Methode

Die `MoveNext` Methode eines Enumerators-Objekts kapselt den Code der Iteratorblock. Aufrufen der `MoveNext` Methode führt Code in der Iteratorblock und legt die `Current` Eigenschaft des Enumeratorobjekts nach Bedarf. Der genaue vom durchgeführten Aktion `MoveNext` hängt vom Zustand des Enumeratorobjekts beim `MoveNext` aufgerufen:

*  Wenn der Status des Enumeratorobjekts ist ***vor***, wird durch Aufrufen `MoveNext`:
   * Ändert den Zustand in ***ausführen***.
   * Die Parameter initialisiert (einschließlich `this`) des Iteratorblocks auf den Argumentwerten und Instanzwert gespeichert, wenn der Enumerator-Objekt initialisiert wurde.
   * Führt Iteratorblock von Anfang an, bis die Ausführung unterbrochen wird (siehe Beschreibung unten).
*  Wenn der Status des Enumeratorobjekts ist ***ausführen***, das Ergebnis des Aufrufs `MoveNext` ist nicht angegeben.
*  Wenn der Status des Enumeratorobjekts ist ***angehalten***, wird durch Aufrufen `MoveNext`:
   * Ändert den Zustand in ***ausführen***.
   * Stellt die Werte aller lokalen Variablen und Parameter, die (einschließlich dieses) wieder her Werte, die bei der Ausführung des Iteratorblocks zuletzt angehalten wurde. Beachten Sie, dass der Inhalt der alle Objekte, die auf diese Variablen verweist seit dem vorherigen Aufruf von MoveNext geändert haben können.
   * Setzt die Ausführung der Iteratorblock unmittelbar nach der `yield return` -Anweisung, verursacht das Anhalten der Ausführung und wird fortgesetzt, bis die Ausführung unterbrochen wird (siehe Beschreibung unten).
*  Wenn der Status des Enumeratorobjekts ist ***nach***, wird durch Aufrufen `MoveNext` gibt `false`.


Wenn `MoveNext` Iteratorblock führt auf vier Arten unterbrochen werden können: Durch eine `yield return` Anweisung, durch eine `yield break` Anweisung, durch das Ende der Iteratorblocks auftreten und eine Ausnahme ausgelöst und aus dem Iteratorblock übertragen wird.

*  Wenn eine `yield return` -Anweisung gefunden ([die Yield-Anweisung](statements.md#the-yield-statement)):
   * Die in der Anweisung angegebene Ausdruck wird ausgewertet, implizit in den Typ "yield" konvertiert und zugewiesen der `Current` Eigenschaft des Enumeratorobjekts.
   * Die Ausführung des iteratortexts wird angehalten. Die Werte aller lokalen Variablen und Parameter (einschließlich `this`) gespeichert sind, wie der Speicherort dieser `yield return` Anweisung. Wenn die `yield return` -Anweisung ist in einem oder mehreren `try` blockiert wird, zugeordneten `finally` Blöcke werden zu diesem Zeitpunkt nicht ausgeführt.
   * Der Status des Enumeratorobjekts wird geändert, um ***angehalten***.
   * Die `MoveNext` Methodenrückgabe `true` an den Aufrufer, der angibt, dass die Iteration erfolgreich auf den nächsten Wert gesetzt.
*  Wenn eine `yield break` -Anweisung gefunden ([die Yield-Anweisung](statements.md#the-yield-statement)):
   * Wenn die `yield break` -Anweisung ist in einem oder mehreren `try` blockiert wird, zugeordneten `finally` -blocke ausgeführt werden.
   * Der Status des Enumeratorobjekts wird geändert, um ***nach***.
   * Die `MoveNext` Methodenrückgabe `false` an den Aufrufer, der angibt, dass die Iteration abgeschlossen ist.
*  Wenn das Ende des iteratortexts erreicht wird:
   * Der Status des Enumeratorobjekts wird geändert, um ***nach***.
   * Die `MoveNext` Methodenrückgabe `false` an den Aufrufer, der angibt, dass die Iteration abgeschlossen ist.
*  Wenn eine Ausnahme ausgelöst und aus dem Iteratorblock übertragen:
   * Entsprechende `finally` Blöcke im des iteratortexts werden durch die ausnahmeweitergabe ausgeführt wurden.
   * Der Status des Enumeratorobjekts wird geändert, um ***nach***.
   * Die ausnahmeweitergabe wird fortgesetzt, an den Aufrufer von der `MoveNext` Methode.

#### <a name="the-current-property"></a>Die Current-Eigenschaft

Ein Enumeratorobjekt `Current` Eigenschaft betroffen `yield return` Anweisungen im Iteratorblock.

Wenn ein Enumeratorobjekt ist der ***angehalten*** Zustand, den Wert der `Current` ist der Wert festlegen, indem dem vorherigen Aufruf von `MoveNext`. Wenn ein Enumeratorobjekt ist der ***vor***, ***ausgeführt***, oder ***nach*** angibt, das Ergebnis des Zugriffs auf `Current` ist nicht angegeben.

Geben Sie für einen Iterator mit einem "yield" als `object`, das Ergebnis des Zugriffs auf `Current` über des Enumeratorobjekts `IEnumerable` Implementierung entspricht, für den Zugriff auf `Current` über des Enumeratorobjekts `IEnumerator<T>` Implementierung und das Ergebnis umwandeln `object`.

#### <a name="the-dispose-method"></a>Die Dispose-Methode

Die `Dispose` Methode wird verwendet, um die Iteration zu bereinigen, indem Sie direkt die Enumerator-Objekt an die ***nach*** Zustand.

*  Wenn der Status des Enumeratorobjekts ist ***vor***, wird durch Aufrufen `Dispose` ändert den Zustand in ***nach***.
*  Wenn der Status des Enumeratorobjekts ist ***ausführen***, das Ergebnis des Aufrufs `Dispose` ist nicht angegeben.
*  Wenn der Status des Enumeratorobjekts ist ***angehalten***, wird durch Aufrufen `Dispose`:
   * Ändert den Zustand in ***ausführen***.
   * Führt alle schließlich Blöcke, als ob die zuletzt ausgeführte `yield return` Anweisung wurden eine `yield break` Anweisung. Wenn dies bewirkt, eine Ausnahme ausgelöst und aus dem Iterator-Text übertragen werden dass, ist der Status des Enumeratorobjekts auf festgelegt ***nach*** und die Ausnahme wird an den Aufrufer der übertragen die `Dispose` Methode.
   * Ändert den Zustand in ***nach***.
*  Wenn der Status des Enumeratorobjekts ist ***nach***, wird durch Aufrufen `Dispose` hat keine Auswirkungen.

### <a name="enumerable-objects"></a>Aufzählbare Objekte

Wenn ein Funktionsmember einen enumerable Schnittstellentyp zurückgeben mit einem Iteratorblock implementiert ist, wird das Funktionselement Aufrufen nicht sofort den Code im Iteratorblock ausgeführt. Stattdessen eine ***aufzählbares Objekt*** erstellt und zurückgegeben. Des aufzählbaren Objekts `GetEnumerator` Methode gibt ein Enumeratorobjekt, das den Code kapselt im Iteratorblock angegeben und Ausführung des Codes im Iteratorblock tritt auf, wenn des Enumeratorobjekts `MoveNext` -Methode wird aufgerufen. Ein aufzählbares Objekt weist folgende Merkmale auf:

*  Es implementiert `IEnumerable` und `IEnumerable<T>`, wobei `T` ist der Yield-Typ des Iterators.
*  Initialisiert mit einer Kopie die Argumentwerte gelten (sofern vorhanden) und Instanzwert an das Funktionselement übergeben.

Ein aufzählbares Objekt ist in der Regel eine Instanz einer vom Compiler generierter enumerable-Klasse, die den Code im Iteratorblock kapselt und die enumerable-Schnittstellen implementiert, aber andere Methoden der Implementierung sind möglich. Wenn eine enumerable-Klasse, die vom Compiler generiert wird, wird diese Klasse geschachtelt werden, direkt oder indirekt in der Klasse, die mit das Funktionselement muss private zugriffsmöglichkeiten und muss einen Namen für die Verwendung durch den Compiler reserviert ([Bezeichner ](lexical-structure.md#identifiers)).

Ein aufzählbares Objekt möglicherweise mehr als die oben angegebenen Schnittstellen. Insbesondere kann ein aufzählbares Objekt auch implementieren `IEnumerator` und `IEnumerator<T>`, er ermöglicht, sowohl als ein aufzählbares Element einen Enumerator dient. In dieser Art der Implementierung, die beim ersten Öffnen eines aufzählbaren Objekts `GetEnumerator` Methode wird aufgerufen, das aufzählbare Objekt selbst wird zurückgegeben. Nachfolgende Aufrufe von des aufzählbaren Objekts `GetEnumerator`, sofern vorhanden, die eine Kopie des aufzählbaren Objekts zurückgeben. Daher zurückgegeben jeder Enumerator hat seinen eigenen Status und Änderungen in einem Enumerator hat keinen Einfluss auf einen anderen.

#### <a name="the-getenumerator-method"></a>Der GetEnumerator-Methode

Ein aufzählbares Objekt stellt eine Implementierung der `GetEnumerator` Methoden der `IEnumerable` und `IEnumerable<T>` Schnittstellen. Die beiden `GetEnumerator` Methoden gemeinsam nutzen, eine gängige Implementierung, die reserviert, und gibt ein verfügbaren Enumeratorobjekt zurück. Das Enumeratorobjekt mit den Argumentwerten initialisiert wird und der Instanz ein Wert gespeichert, wenn das aufzählbare Objekt initialisiert, aber ansonsten wurde die Enumerator-Objekt-Funktionen wie in beschrieben [Enumeratorobjekte](classes.md#enumerator-objects).

### <a name="implementation-example"></a>Beispiel für die Implementierung

Dieser Abschnitt beschreibt eine mögliche Implementierung der Iteratoren in Bezug auf die standardmäßige C#-Konstrukte. Die hier beschriebene Implementierung basiert darauf, dass die gleichen Prinzipien, die vom Microsoft C#-Compiler verwendet, aber es ist keine beauftragten Implementierung oder das einzig mögliche.

Die folgenden `Stack<T>` -Klasse implementiert die `GetEnumerator` Methode, die mithilfe eines Iterators. Der Iterator Listet die Elemente des Stapels in von oben nach unten.

```csharp
using System;
using System.Collections;
using System.Collections.Generic;

class Stack<T>: IEnumerable<T>
{
    T[] items;
    int count;

    public void Push(T item) {
        if (items == null) {
            items = new T[4];
        }
        else if (items.Length == count) {
            T[] newItems = new T[count * 2];
            Array.Copy(items, 0, newItems, 0, count);
            items = newItems;
        }
        items[count++] = item;
    }

    public T Pop() {
        T result = items[--count];
        items[count] = default(T);
        return result;
    }

    public IEnumerator<T> GetEnumerator() {
        for (int i = count - 1; i >= 0; --i) yield return items[i];
    }
}
```

Die `GetEnumerator` Methode kann in eine Instanziierung einer vom Compiler generierter Enumerator-Klasse, die den Code im Iteratorblock kapselt übersetzt werden, wie im folgenden dargestellt.

```csharp
class Stack<T>: IEnumerable<T>
{
    ...

    public IEnumerator<T> GetEnumerator() {
        return new __Enumerator1(this);
    }

    class __Enumerator1: IEnumerator<T>, IEnumerator
    {
        int __state;
        T __current;
        Stack<T> __this;
        int i;

        public __Enumerator1(Stack<T> __this) {
            this.__this = __this;
        }

        public T Current {
            get { return __current; }
        }

        object IEnumerator.Current {
            get { return __current; }
        }

        public bool MoveNext() {
            switch (__state) {
                case 1: goto __state1;
                case 2: goto __state2;
            }
            i = __this.count - 1;
        __loop:
            if (i < 0) goto __state2;
            __current = __this.items[i];
            __state = 1;
            return true;
        __state1:
            --i;
            goto __loop;
        __state2:
            __state = 2;
            return false;
        }

        public void Dispose() {
            __state = 2;
        }

        void IEnumerator.Reset() {
            throw new NotSupportedException();
        }
    }
}
```

Der Code im Iteratorblock in einen Zustandsautomaten aktiviert ist, und die in der vorherigen Übersetzung, platziert Sie der `MoveNext` -Methode der Enumerator-Klasse. Darüber hinaus die lokale Variable `i` in ein Feld im Enumerator-Objekt aktiviert ist, damit er fortfahren kann, über Aufrufe der vorhanden sein, `MoveNext`.

Im folgende Beispiel wird eine einfache Tabelle der Multiplikation von ganzen Zahlen 1 bis 10 gedruckt. Die `FromTo` -Methode im Beispiel gibt ein aufzählbares Objekt zurück und wird mithilfe eines Iterators implementiert.

```csharp
using System;
using System.Collections.Generic;

class Test
{
    static IEnumerable<int> FromTo(int from, int to) {
        while (from <= to) yield return from++;
    }

    static void Main() {
        IEnumerable<int> e = FromTo(1, 10);
        foreach (int x in e) {
            foreach (int y in e) {
                Console.Write("{0,3} ", x * y);
            }
            Console.WriteLine();
        }
    }
}
```

Die `FromTo` Methode kann in eine Instanziierung einer vom Compiler generierter enumerable-Klasse, die den Code im Iteratorblock kapselt übersetzt werden, wie im folgenden dargestellt.

```csharp
using System;
using System.Threading;
using System.Collections;
using System.Collections.Generic;

class Test
{
    ...

    static IEnumerable<int> FromTo(int from, int to) {
        return new __Enumerable1(from, to);
    }

    class __Enumerable1:
        IEnumerable<int>, IEnumerable,
        IEnumerator<int>, IEnumerator
    {
        int __state;
        int __current;
        int __from;
        int from;
        int to;
        int i;

        public __Enumerable1(int __from, int to) {
            this.__from = __from;
            this.to = to;
        }

        public IEnumerator<int> GetEnumerator() {
            __Enumerable1 result = this;
            if (Interlocked.CompareExchange(ref __state, 1, 0) != 0) {
                result = new __Enumerable1(__from, to);
                result.__state = 1;
            }
            result.from = result.__from;
            return result;
        }

        IEnumerator IEnumerable.GetEnumerator() {
            return (IEnumerator)GetEnumerator();
        }

        public int Current {
            get { return __current; }
        }

        object IEnumerator.Current {
            get { return __current; }
        }

        public bool MoveNext() {
            switch (__state) {
            case 1:
                if (from > to) goto case 2;
                __current = from++;
                __state = 1;
                return true;
            case 2:
                __state = 2;
                return false;
            default:
                throw new InvalidOperationException();
            }
        }

        public void Dispose() {
            __state = 2;
        }

        void IEnumerator.Reset() {
            throw new NotSupportedException();
        }
    }
}
```

Die enumerable-Klasse implementiert sowohl die enumerable-Schnittstellen als auch die Enumeratorschnittstellen, die er ermöglicht, sowohl als ein aufzählbares Element einen Enumerator dient. Beim ersten die `GetEnumerator` Methode wird aufgerufen, das aufzählbare Objekt selbst wird zurückgegeben. Nachfolgende Aufrufe von des aufzählbaren Objekts `GetEnumerator`, sofern vorhanden, die eine Kopie des aufzählbaren Objekts zurückgeben. Daher zurückgegeben jeder Enumerator hat seinen eigenen Status und Änderungen in einem Enumerator hat keinen Einfluss auf einen anderen. Die `Interlocked.CompareExchange` Methode wird verwendet, um die Thread-sichere Betrieb sicherzustellen.

Die `from` und `to` Parameter werden in die Felder in der enumerable-Klasse umgewandelt. Da `from` geändert wird, im Iteratorblock, eine zusätzliche `__from` Feld wird eingeführt, um den ersten Wert speichern `from` in foreach-Enumerator.

Die `MoveNext` -Methode löst eine `InvalidOperationException` , wenn er, wenn aufgerufen wird `__state` ist `0`. Dies schützt vor der Verwendung des aufzählbaren Objekts als ein Enumeratorobjekt erst nach Aufrufen von `GetEnumerator`.

Das folgende Beispiel zeigt eine einfache Struktur-Klasse. Die `Tree<T>` -Klasse implementiert die `GetEnumerator` Methode, die mithilfe eines Iterators. Der Iterator Listet die Elemente der Struktur in Infix-Reihenfolge.

```csharp
using System;
using System.Collections.Generic;

class Tree<T>: IEnumerable<T>
{
    T value;
    Tree<T> left;
    Tree<T> right;

    public Tree(T value, Tree<T> left, Tree<T> right) {
        this.value = value;
        this.left = left;
        this.right = right;
    }

    public IEnumerator<T> GetEnumerator() {
        if (left != null) foreach (T x in left) yield x;
        yield value;
        if (right != null) foreach (T x in right) yield x;
    }
}

class Program
{
    static Tree<T> MakeTree<T>(T[] items, int left, int right) {
        if (left > right) return null;
        int i = (left + right) / 2;
        return new Tree<T>(items[i], 
            MakeTree(items, left, i - 1),
            MakeTree(items, i + 1, right));
    }

    static Tree<T> MakeTree<T>(params T[] items) {
        return MakeTree(items, 0, items.Length - 1);
    }

    // The output of the program is:
    // 1 2 3 4 5 6 7 8 9
    // Mon Tue Wed Thu Fri Sat Sun

    static void Main() {
        Tree<int> ints = MakeTree(1, 2, 3, 4, 5, 6, 7, 8, 9);
        foreach (int i in ints) Console.Write("{0} ", i);
        Console.WriteLine();

        Tree<string> strings = MakeTree(
            "Mon", "Tue", "Wed", "Thu", "Fri", "Sat", "Sun");
        foreach (string s in strings) Console.Write("{0} ", s);
        Console.WriteLine();
    }
}
```

Die `GetEnumerator` Methode kann in eine Instanziierung einer vom Compiler generierter Enumerator-Klasse, die den Code im Iteratorblock kapselt übersetzt werden, wie im folgenden dargestellt.

```csharp
class Tree<T>: IEnumerable<T>
{
    ...

    public IEnumerator<T> GetEnumerator() {
        return new __Enumerator1(this);
    }

    class __Enumerator1 : IEnumerator<T>, IEnumerator
    {
        Node<T> __this;
        IEnumerator<T> __left, __right;
        int __state;
        T __current;

        public __Enumerator1(Node<T> __this) {
            this.__this = __this;
        }

        public T Current {
            get { return __current; }
        }

        object IEnumerator.Current {
            get { return __current; }
        }

        public bool MoveNext() {
            try {
                switch (__state) {

                case 0:
                    __state = -1;
                    if (__this.left == null) goto __yield_value;
                    __left = __this.left.GetEnumerator();
                    goto case 1;

                case 1:
                    __state = -2;
                    if (!__left.MoveNext()) goto __left_dispose;
                    __current = __left.Current;
                    __state = 1;
                    return true;

                __left_dispose:
                    __state = -1;
                    __left.Dispose();

                __yield_value:
                    __current = __this.value;
                    __state = 2;
                    return true;

                case 2:
                    __state = -1;
                    if (__this.right == null) goto __end;
                    __right = __this.right.GetEnumerator();
                    goto case 3;

                case 3:
                    __state = -3;
                    if (!__right.MoveNext()) goto __right_dispose;
                    __current = __right.Current;
                    __state = 3;
                    return true;

                __right_dispose:
                    __state = -1;
                    __right.Dispose();

                __end:
                    __state = 4;
                    break;

                }
            }
            finally {
                if (__state < 0) Dispose();
            }
            return false;
        }

        public void Dispose() {
            try {
                switch (__state) {

                case 1:
                case -2:
                    __left.Dispose();
                    break;

                case 3:
                case -3:
                    __right.Dispose();
                    break;

                }
            }
            finally {
                __state = 4;
            }
        }

        void IEnumerator.Reset() {
            throw new NotSupportedException();
        }
    }
}
```

Die vom Compiler generierten temporären Dateien, die in verwendet die `foreach` Anweisungen werden herausgehoben, in der `__left` und `__right` Felder des Enumeratorobjekts. Die `__state` Feld des Enumeratorobjekts sorgfältig aktualisiert, damit die richtige `Dispose()` Methode wird ordnungsgemäß aufgerufen werden, wenn eine Ausnahme ausgelöst wird. Beachten Sie, dass es nicht möglich, schreiben den übersetzten Code mit einfachen `foreach` Anweisungen.

## <a name="async-functions"></a>Asynchrone Funktionen

Eine Methode ([Methoden](classes.md#methods)) oder eine anonyme Funktion ([anonyme Funktionsausdrücke](expressions.md#anonymous-function-expressions)) mit der `async` Modifizierer wird aufgerufen, eine ***Async-Funktion***. Im Allgemeinen den Begriff ***Async*** wird verwendet, um jede Art von Funktion beschreiben, das die `async` Modifizierer.

Es ist ein Fehler während der Kompilierung für die Liste der formalen Parameter zum Angeben einer Async-Funktion `ref` oder `out` Parameter.

Die *Return_type* von Async-Methode muss entweder `void` oder ***Aufgabentyp***. Die Tasktypen sind `System.Threading.Tasks.Task` und Typen aus `System.Threading.Tasks.Task<T>`. Aus Gründen der Übersichtlichkeit sind in diesem Kapitel diese Typen verwiesen wird als `Task` und `Task<T>`bzw. Eine asynchrone Methode zurückgeben von Typ "Task" wird als Task zurückgeben bezeichnet.

Die genaue Definition der Tasktypen ist die Implementierung definiert, aber aus Sicht der Sprache wird Typ "Task" in einem der Zustände nicht abgeschlossen wurde, erfolgreich war oder fehlgeschlagen ist. Eine fehlgeschlagene Aufgabe zeichnet eine wichtige Ausnahme. Eine erfolgreiche `Task<T>` zeichnet ein Ergebnis vom Typ `T`. Tasktypen sind "awaitable", und können daher die Operanden des await-Ausdrücken ([Await-Ausdrücken](expressions.md#await-expressions)).

Eine Async-Funktionsaufruf bietet die Möglichkeit, die Auswertung Anhalten von await-Ausdrücken ([Await-Ausdrücken](expressions.md#await-expressions)) in ihrem Text. Auswertung kann später fortgesetzt werden, zum Zeitpunkt der anhalten await-Ausdruck von einem ***Wiederaufnahmedelegat***. Der Wiederaufnahmedelegat ist vom Typ `System.Action`, und wenn er aufgerufen wird, wird Auswertung der Async-Funktionsaufruf aus der Await-Ausdruck, der an der Stelle fortgesetzt. Die ***aktuellen Aufrufer*** einer asynchronen Funktion Aufruf ist des ursprünglichen Aufrufers, wenn der Funktionsaufruf nie angehalten wurde, oder der letzte Aufrufer der Wiederaufnahmedelegat andernfalls.

### <a name="evaluation-of-a-task-returning-async-function"></a>Eine Aufgabe zurückgibt Async-Funktion

Aufruf einer Aufgabe zurückgibt Async-Funktion bewirkt, dass eine Instanz des Typs die zurückgegebene Aufgabe generiert werden soll. Dies wird aufgerufen, die ***Task zurückgeben*** der Async-Funktion. Der Task ist anfangs in einem unvollständigen Status.

Der Hauptteil der Async-Funktion wird dann ausgewertet bis sie (durch erreichen einen Await-Ausdruck) entweder angehalten oder beendet wird, an dem Verwaltungspunkt-Steuerungs an den Aufrufer, zusammen mit dem Rückgabewert Task zurückgegeben.

Wenn der Hauptteil der Async-Funktion beendet wird, wird die return-Aufgabe beendet den unvollständigen Zustand verschoben:

*  Wenn der Hauptteil der Funktion als Ergebnis erreicht wird, eine return-Anweisung oder das Ende des Texts beendet wird, wird alle Ergebniswert in der Aufgabe zurückgeben aufgezeichnet, die in einem Zustand "erfolgreich" versetzt wird.
*  Wenn der Hauptteil der Funktion als Ergebnis einer nicht abgefangenen Ausnahme beendet wird ([die Throw-Anweisung](statements.md#the-throw-statement)) wird die Ausnahme in der return-Aufgabe das in einem fehlerhaften Zustand versetzt wird aufgezeichnet.

### <a name="evaluation-of-a-void-returning-async-function"></a>Auswertung einer "void" zurückgebende asynchrone-Funktion

Wenn der Rückgabetyp der asynchronen Funktion `void`, Auswertung unterscheidet sich von den oben genannten auf folgende Weise: Da keine Aufgabe zurückgegeben wird, kommuniziert die Funktion Vervollständigung und Ausnahmen für des aktuellen Threads stattdessen ***Synchronisierungskontext***. Die genaue Definition von Synchronisierungskontext ist implementierungsabhängig, aber es ist eine Darstellung des "where" der aktuelle Thread ausgeführt wird. Der Synchronisierungskontext wird benachrichtigt, wenn es sich bei Auswertung einer "void" zurückgebende asynchrone Funktion beginnt, wird erfolgreich abgeschlossen oder führt dazu, dass eine nicht abgefangene Ausnahme ausgelöst wird.

Dadurch wird den Kontext, behalten Sie verfolgen, wie viele "void" zurückgebende asynchrone Funktionen, darunter ausgeführt werden, und klicken Sie, wie Ausnahmen, die sie stammenden weitergegeben werden soll.
