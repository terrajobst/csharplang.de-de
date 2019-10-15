---
ms.openlocfilehash: e0def754174ab8646f9b849abe86d2c375c958b6
ms.sourcegitcommit: 892af9016b3317a8fae12d195014dc38ba51cf16
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/01/2019
ms.locfileid: "71703982"
---
# <a name="classes"></a>Klassen

Eine-Klasse ist eine Datenstruktur, die Datenmember (Konstanten und Felder), Funktionsmember (Methoden, Eigenschaften, Ereignisse, Indexer, Operatoren, Instanzkonstruktoren, Dekonstruktoren und statische Konstruktoren) und die in der Struktur enthaltenen Typen enthalten kann. Klassentypen unterstützen Vererbung, einen Mechanismus, mit dem eine abgeleitete Klasse eine Basisklasse erweitern und spezialisieren kann.

## <a name="class-declarations"></a>Klassen Deklarationen

Ein *class_declaration* ist ein *type_declaration* ([Typdeklarationen](namespaces.md#type-declarations)), der eine neue Klasse deklariert.

```antlr
class_declaration
    : attributes? class_modifier* 'partial'? 'class' identifier type_parameter_list?
      class_base? type_parameter_constraints_clause* class_body ';'?
    ;
```

Ein *class_declaration* besteht aus einem optionalen Satz von *Attributen* ([Attributen](attributes.md)), gefolgt von einem optionalen Satz von *class_modifier*s ([Klassenmodifizierer](classes.md#class-modifiers)), gefolgt von einem optionalen `partial`-Modifizierer, gefolgt vom-Schlüsselwort. `class` und ein *Bezeichner* , der die-Klasse benennt, gefolgt von einem optionalen *type_parameter_list* ([Typparameter](classes.md#type-parameters)), gefolgt von einer optionalen *class_base* -Spezifikation ([Klassenbasis Spezifikation](classes.md#class-base-specification)), gefolgt von ein optionaler Satz von *type_parameter_constraints_clause*s ([Typparameter Einschränkungen](classes.md#type-parameter-constraints)), gefolgt von einem *class_body* ([Klassen Text](classes.md#class-body)), optional gefolgt von einem Semikolon.

Eine Klassen Deklaration kann keine *type_parameter_constraints_clause*s bereitstellen, es sei denn, Sie stellt auch ein *type_parameter_list*

Eine Klassen Deklaration, die ein *type_parameter_list* bereitstellt, ist eine ***generische Klassen Deklaration***. Außerdem ist jede Klasse, die in einer generischen Klassen Deklaration oder generischen Struktur Deklaration geschachtelt ist, selbst eine generische Klassen Deklaration, da Typparameter für den enthaltenden Typ angegeben werden müssen, um einen konstruierten Typ zu erstellen.

### <a name="class-modifiers"></a>Klassenmodifizierer

Ein *class_declaration* kann optional eine Sequenz von Klassenmodifizierer einschließen:

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

Es ist ein Kompilierzeitfehler, damit derselbe Modifizierer mehrmals in einer Klassen Deklaration angezeigt wird.

Der `new` -Modifizierer ist für-Klassen zulässig. Er gibt an, dass die Klasse einen geerbten Member mit demselben Namen verbirgt, wie im [neuen Modifizierer](classes.md#the-new-modifier)beschrieben. Es ist ein Kompilierzeitfehler, damit `new` der-Modifizierer in einer Klassen Deklaration angezeigt wird, die keine Klassen Deklaration ist.

Die `public`Modifizierer `protected`, `private` , `internal`und Steuern den Zugriff auf die-Klasse. Abhängig vom Kontext, in dem die Klassen Deklaration auftritt, sind einige dieser Modifizierer möglicherweise nicht zulässig (als[Barrierefreiheit deklariert](basic-concepts.md#declared-accessibility)).

Die `abstract`modifiziererer, `sealed` und `static` werden in den folgenden Abschnitten erläutert.

#### <a name="abstract-classes"></a>Abstrakte Klassen

Der `abstract` -Modifizierer wird verwendet, um anzugeben, dass eine Klasse unvollständig ist und nur als Basisklasse verwendet werden soll. Eine abstrakte Klasse unterscheidet sich wie folgt von einer nicht abstrakten Klasse:

*  Eine abstrakte Klasse kann nicht direkt instanziiert werden, und es handelt sich um einen Kompilierzeitfehler `new` , wenn der Operator für eine abstrakte Klasse verwendet werden soll. Obwohl es möglich ist, Variablen und Werte zu haben, deren Kompilier Zeittypen abstrakt sind, sind diese Variablen und Werte `null` notwendigerweise entweder ein oder enthalten Verweise auf Instanzen von nicht abstrakten Klassen, die von den abstrakten Typen abgeleitet sind.
*  Eine abstrakte Klasse ist zulässig (jedoch nicht erforderlich), um abstrakte Member zu enthalten.
*  Eine abstrakte Klasse kann nicht versiegelt werden.

Wenn eine nicht abstrakte Klasse von einer abstrakten Klasse abgeleitet wird, muss die nicht abstrakte Klasse tatsächliche Implementierungen aller geerbten abstrakten Member enthalten, wodurch diese abstrakten Member überschrieben werden. Im Beispiel
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
die abstrakte- `A` Klasse führt eine abstrakte `F`Methode ein. Die `B` -Klasse führt eine `G`zusätzliche-Methode ein, aber da Sie keine `F`Implementierung `B` von bereitstellt, muss auch als abstrakt deklariert werden. Die `C` Klasse über `F` schreibt und stellt eine tatsächliche Implementierung bereit. Da keine abstrakten Member in `C`vorhanden sind, `C` ist zulässig (jedoch nicht erforderlich), um nicht abstrakt zu sein.

#### <a name="sealed-classes"></a>Versiegelte Klassen

Der `sealed` -Modifizierer wird verwendet, um die Ableitung von einer Klasse zu verhindern. Ein Kompilierzeitfehler tritt auf, wenn eine versiegelte Klasse als Basisklasse einer anderen Klasse angegeben wird.

Eine versiegelte Klasse kann nicht auch eine abstrakte Klasse sein.

Der `sealed` -Modifizierer wird hauptsächlich verwendet, um eine unbeabsichtigte Ableitung zu verhindern, er ermöglicht aber auch bestimmte Lauf Zeit Optimierungen. Insbesondere weil eine versiegelte Klasse bekanntermaßen keine abgeleiteten Klassen hat, ist es möglich, die Aufrufe virtueller Funktionsmember für versiegelte Klassen Instanzen in nicht virtuelle Aufrufe umzuwandeln.

#### <a name="static-classes"></a>Statische Klassen

Der `static` -Modifizierer wird verwendet, um die Klasse zu markieren, die als ***statische Klasse***deklariert wird. Eine statische Klasse kann nicht instanziiert werden, kann nicht als Typ verwendet werden und darf nur statische Member enthalten. Nur eine statische Klasse kann Deklarationen von Erweiterungs Methoden ([Erweiterungs Methoden](classes.md#extension-methods)) enthalten.

Eine statische Klassen Deklaration unterliegt den folgenden Einschränkungen:

*  Eine statische Klasse darf keinen-Modifizierer `sealed` oder `abstract` -Modifizierer enthalten. Beachten Sie jedoch, dass eine statische Klasse, die nicht von instanziiert oder abgeleitet werden kann, so verhält, als ob Sie sowohl versiegelt als auch abstrakt wäre.
*  Eine statische Klasse darf keine *class_base* Specification ([Klassenbasis Spezifikation](classes.md#class-base-specification)) enthalten und kann weder eine Basisklasse noch eine Liste implementierter Schnittstellen explizit angeben. Eine statische Klasse erbt implizit vom Typ `object`.
*  Eine statische Klasse kann nur statische Member ([statische Member und Instanzmember](classes.md#static-and-instance-members)) enthalten. Beachten Sie, dass Konstanten und Untertypen als statische Member klassifiziert werden.
*  Eine statische Klasse kann keine Member mit `protected` oder `protected internal` deklarierter Barrierefreiheit haben.

Es handelt sich um einen Kompilierzeitfehler, der gegen diese Einschränkungen verstößt.

Eine statische Klasse hat keine Instanzkonstruktoren. Es ist nicht möglich, einen Instanzkonstruktor in einer statischen Klasse zu deklarieren, und für eine statische Klasse wird kein Standardinstanzkonstruktor ([Standardkonstruktoren](classes.md#default-constructors)) bereitgestellt.

Die Member einer statischen Klasse sind nicht automatisch statisch, und die Element Deklarationen müssen explizit einen `static` Modifizierer einschließen (mit Ausnahme von Konstanten und Typen). Wenn eine Klasse in einer statischen äußeren Klasse geschachtelt ist, ist die geschachtelte Klasse keine statische Klasse, es sei denn `static` , Sie enthält explizit einen Modifizierer.

__Verweisen auf statische Klassentypen__

Ein *namespace_or_type_name* ([Namespace und Typnamen](basic-concepts.md#namespace-and-type-names)) darf auf eine statische Klasse verweisen, wenn

*  *Namespace_or_type_name* ist der `T` in einem *namespace_or_type_name* -Format `T.I` oder
*  *Namespace_or_type_name* ist der `T` in einer *typeof_expression* ([Argument Liste](expressions.md#argument-lists)1) der Form `typeof(T)`.

Ein *primary_expression* ([Funktionsmember](expressions.md#function-members)) darf auf eine statische Klasse verweisen, wenn

*  *Primary_expression* ist der `E` in einer *member_access* ([Kompilierzeit Überprüfung der dynamischen Überladungs Auflösung](expressions.md#compile-time-checking-of-dynamic-overload-resolution)) der Form `E.I`.

In jedem anderen Kontext ist es ein Kompilierzeitfehler, um auf eine statische Klasse zu verweisen. Es ist z. b. ein Fehler für eine statische Klasse, die als Basisklasse, als konstituierender Typ (in Form von[Typen](classes.md#nested-types)) eines Members, als generisches Typargument oder als Typparameter Einschränkung verwendet werden soll. Ebenso kann eine statische Klasse nicht in einem Arraytyp, einem Zeigertyp, einem `new` Ausdruck, einem Umwandlungs Ausdruck, `is` einem Ausdruck, `as` einem Ausdruck, `sizeof` einem Ausdruck oder einem Standardwert Ausdruck verwendet werden.

### <a name="partial-modifier"></a>Partieller Modifizierer

Der `partial`-Modifizierer wird verwendet, um anzugeben, dass dieses *class_declaration* eine partielle Typdeklaration ist. Mehrere partielle Typdeklarationen mit demselben Namen innerhalb eines einschließenden Namespace oder einer Typdeklaration kombinieren eine Typdeklaration, die den in [partiellen Typen](classes.md#partial-types)angegebenen Regeln folgt.

Die Deklaration einer Klasse, die über separate Segmente von Programmtext verteilt ist, kann nützlich sein, wenn diese Segmente in verschiedenen Kontexten erstellt oder verwaltet werden. Beispielsweise kann ein Teil einer Klassen Deklaration maschinell generiert werden, während der andere manuell erstellt wird. Die Text Trennung der beiden verhindert, dass Updates durch eine in Konflikt mit Updates durch die andere verursacht werden.

### <a name="type-parameters"></a>Typparameter

Ein Typparameter ist ein einfacher Bezeichner, der einen Platzhalter für ein Typargument angibt, das zum Erstellen eines konstruierten Typs bereitgestellt wird. Ein Typparameter ist ein formaler Platzhalter für einen Typ, der später bereitgestellt wird. Im Gegensatz dazu ist ein[Typargument (Typargumente](types.md#type-arguments)) der tatsächliche Typ, der beim Erstellen eines konstruierten Typs den Typparameter ersetzt.

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

Jeder Typparameter in einer Klassen Deklaration definiert einen Namen im Deklarations Raum ([Deklarationen](basic-concepts.md#declarations)) dieser Klasse. Daher kann er nicht denselben Namen wie ein anderer Typparameter oder ein Member haben, der in dieser Klasse deklariert ist. Ein Typparameter kann nicht den gleichen Namen haben wie der Typ selbst.

### <a name="class-base-specification"></a>Klassenbasis Spezifikation

Eine Klassen Deklaration kann eine *class_base* -Spezifikation enthalten, die die direkte Basisklasse der Klasse und die Schnittstellen ([Schnittstellen](interfaces.md)) definiert, die von der-Klasse direkt implementiert werden.

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

Die in einer Klassen Deklaration angegebene Basisklasse kann ein konstruierter Klassentyp ([konstruierte Typen](types.md#constructed-types)) sein. Eine Basisklasse kann nicht eigenständig ein Typparameter sein, Sie kann jedoch die Typparameter enthalten, die sich im Gültigkeitsbereich befinden.

```csharp
class Extend<V>: V {}            // Error, type parameter used as base class
```

#### <a name="base-classes"></a>Basisklassen

Wenn ein *class_type* in der *class_base*enthalten ist, gibt es die direkte Basisklasse der Klasse an, die deklariert wird. Wenn eine Klassen Deklaration keine *class_base*hat oder wenn die *class_base* nur Schnittstellentypen auflistet, wird davon ausgegangen, dass die direkte Basisklasse `object` ist. Eine Klasse erbt Member von ihrer direkten Basisklasse, wie in [Vererbung](classes.md#inheritance)beschrieben.

Im Beispiel
```csharp
class A {}

class B: A {}
```
die `A` Klasse ist die direkte Basisklasse von `B`, und `B` wird als abgeleitet `A`bezeichnet. Da `A` nicht explizit eine direkte Basisklasse angibt, ist die direkte Basisklasse implizit `object`.

Wenn eine Basisklasse für einen konstruierten Klassentyp in der Deklaration der generischen Klasse angegeben wird, wird die Basisklasse des konstruierten Typs abgerufen, indem für jede *type_parameter* in der Basisklassen Deklaration der entsprechende *type_argument* des konstruierten Typs. Bei Angabe der generischen Klassen Deklarationen
```csharp
class B<U,V> {...}

class G<T>: B<string,T[]> {...}
```
die Basisklasse des konstruierten Typs `G<int>` `B<string,int[]>`wäre.

Die direkte Basisklasse eines Klassen Typs muss mindestens so zugänglich sein wie der Klassentyp selbst ([Barrierefreiheits Domänen](basic-concepts.md#accessibility-domains)). Beispielsweise ist es ein Kompilierzeitfehler für eine `public` Klasse, die von einer `private` -Klasse `internal` oder-Klasse abgeleitet werden soll.

Die direkte Basisklasse eines Klassen Typs darf keinem der folgenden Typen sein: `System.Array`, `System.Delegate`, `System.MulticastDelegate`, `System.Enum`oder `System.ValueType`. Darüber hinaus kann eine generische Klassen Deklaration `System.Attribute` nicht als direkte oder indirekte Basisklasse verwenden.

Bei der Bestimmung der Bedeutung der direkten Basisklassen Spezifikation `A` einer Klasse `B`wird die direkte Basisklasse von `B` vorübergehend `object`als festgelegt. Intuitiv wird dadurch sichergestellt, dass die Bedeutung einer Basisklassen Spezifikation nicht rekursiv von sich selbst abhängig ist. Das Beispiel:
```csharp
class A<T> {
   public class B {}
}

class C : A<C.B> {}
```
`A<C.B>` ist fehlerhaft `object`, da in der Basisklassen Spezifikation die direkte Basisklasse `C` von als betrachtet wird und daher (durch die Regeln von [Namespace-und Typnamen](basic-concepts.md#namespace-and-type-names)) `C` nicht als Member `B`angesehenwird.

Die Basisklassen eines Klassen Typs sind die direkte Basisklasse und deren Basisklassen. Mit anderen Worten: der Satz von Basisklassen ist der transitiv Abschluss der direkten Basisklassen Beziehung. Im obigen Beispiel sind `B` `A` die Basisklassen von und `object`. Im Beispiel
```csharp
class A {...}

class B<T>: A {...}

class C<T>: B<IComparable<T>> {...}

class D<T>: C<T[]> {...}
```
die Basisklassen von `D<int>` sind `C<int[]>`, `B<IComparable<int[]>>`, `A`und. `object`

Mit Ausnahme von `object`Class hat jeder Klassentyp genau eine direkte Basisklasse. Die `object` Klasse verfügt über keine direkte Basisklasse und ist die ultimative Basisklasse aller anderen Klassen.

Wenn eine Klasse `B` von einer Klasse `A`abgeleitet ist, ist dies ein `A` Kompilier `B`Zeitfehler, von dem abhängig ist. Eine Klasse ***hängt direkt*** von ihrer direkten Basisklasse (sofern vorhanden) ab und ***hängt direkt*** von der Klasse ab, in der Sie sofort geschachtelt ist (sofern vorhanden). Bei dieser Definition ist der gesamte Satz von Klassen, von dem eine Klasse abhängt, die reflexive und transitiv Schließung der ***direkt*** von Beziehung abhängig.

Das Beispiel
```csharp
class A: A {}
```
ist fehlerhaft, da die-Klasse von sich selbst abhängt. Ebenso ist das Beispiel
```csharp
class A: B {}
class B: C {}
class C: A {}
```
ist fehlerhaft, da die Klassen zirkulär von sich selbst abhängen. Abschließend wird das Beispiel
```csharp
class A: B.C {}

class B: A
{
    public class C {}
}
```
`A` führt zu `B.C` einem Kompilierzeitfehler, da von (seiner direkten Basisklasse `B` ) abhängt, das von (seiner unmittelbar einschließenden `A`Klasse) abhängt, von dem zirkulär abhängig ist.

Beachten Sie, dass eine Klasse nicht von den Klassen abhängig ist, die darin geschachtelt sind. Im Beispiel
```csharp
class A
{
    class B: A {}
}
```
`B``B` `B` `A` hängt von ab `A`(da sowohl die direkte Basisklasse als auch die unmittelbar einschließende Klasse ist), aber nicht von abhängt (da weder eine Basisklasse noch eine einschließende Klasse von ist). `A` `A`). Daher ist das Beispiel gültig.

Es ist nicht möglich, von einer `sealed` Klasse abzuleiten. Im Beispiel
```csharp
sealed class A {}

class B: A {}            // Error, cannot derive from a sealed class
```
die `B` Klasse ist fehlerhaft, da Sie versucht, von `sealed` der `A`-Klasse abzuleiten.

#### <a name="interface-implementations"></a>Schnittstellenimplementierungen

Eine *class_base* -Spezifikation kann eine Liste von Schnittstellentypen enthalten. in diesem Fall wird die Klasse so genannte, dass die angegebenen Schnittstellentypen direkt implementiert werden. Schnittstellen Implementierungen werden in [Schnittstellen Implementierungen](interfaces.md#interface-implementations)ausführlicher erläutert.

### <a name="type-parameter-constraints"></a>Typparameter Einschränkungen

Generische Typ-und Methoden Deklarationen können optional Typparameter Einschränkungen durch Einschließen von *type_parameter_constraints_clause*s angeben.

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

Jede *type_parameter_constraints_clause* besteht aus dem Token `where`, gefolgt vom Namen eines Typparameters, gefolgt von einem Doppelpunkt und der Liste der Einschränkungen für diesen Typparameter. Für jeden Typparameter kann höchstens `where` eine Klausel vorhanden sein, und die `where` Klauseln können in beliebiger Reihenfolge aufgelistet werden. Wie das `get` - `set` Token und das-Token in einem Eigenschaften `where` Accessor ist das Token kein Schlüsselwort.

Die Liste der Einschränkungen, die in `where` einer-Klausel angegeben werden, kann eine der folgenden Komponenten in dieser Reihenfolge enthalten: eine einzelne primäre Einschränkung, eine oder mehrere sekundäre Einschränkungen und die `new()`Konstruktoreinschränkung.

Eine primäre Einschränkung kann ein Klassentyp oder eine ***Verweistyp Einschränkung*** `class` oder die ***Werttyp Einschränkung*** `struct`sein. Eine sekundäre Einschränkung kann ein *type_parameter* oder *INTERFACE_TYPE*sein.

Die Verweistyp Einschränkung gibt an, dass ein für den Typparameter verwendetes Typargument ein Verweistyp sein muss. Alle Klassentypen, Schnittstellentypen, Delegattypen, Array Typen und Typparameter, die als Verweistyp bekannt sind (wie unten definiert), erfüllen diese Einschränkung.

Die Werttyp Einschränkung gibt an, dass das für den Typparameter verwendete Typargument ein Werttyp sein muss, der keine NULL-Werte zulässt. Alle Strukturtypen, Enumerationstypen und Typparameter, die keine NULL-Werte zulassen und die Werttyp Einschränkung aufweisen, erfüllen diese Einschränkung. Beachten Sie, dass ein Typ, der NULL-Werte zulässt (Typen, die[null](types.md#nullable-types)-Werte zulassen), nicht der Werttyp Einschränkung entspricht. Ein Typparameter mit der Werttyp Einschränkung kann nicht auch über *constructor_constraint*verfügen.

Zeiger Typen dürfen nicht als Typargumente eingestuft werden und werden nicht berücksichtigt, um entweder den Verweistyp oder die Werttyp Einschränkungen zu erfüllen.

Wenn es sich bei einer Einschränkung um einen Klassentyp, einen Schnittstellentyp oder einen Typparameter handelt, gibt dieser Typ einen minimalen "Basistyp" an, den jedes Typargument für diesen Typparameter unterstützen muss. Wenn ein konstruierter Typ oder eine generische Methode verwendet wird, wird das Typargument zur Kompilierzeit mit den Einschränkungen für den Typparameter verglichen. Das angegebene Typargument muss die unter " [erfüllen von Einschränkungen](types.md#satisfying-constraints)" beschriebenen Bedingungen erfüllen.

Eine *class_type* -Einschränkung muss die folgenden Regeln erfüllen:

*  Der Typ muss ein Klassentyp sein.
*  Der Typ darf nicht sein `sealed`.
*  Der Typ darf keinem der folgenden Typen sein `System.Array`:, `System.Delegate`, `System.Enum`oder `System.ValueType`.
*  Der Typ darf nicht sein `object`. Da alle Typen von `object`abgeleitet sind, hätte eine solche Einschränkung keine Auswirkung, wenn Sie zulässig wäre.
*  Höchstens eine Einschränkung für einen angegebenen Typparameter kann ein Klassentyp sein.

Ein Typ, der als *INTERFACE_TYPE* -Einschränkung angegeben ist, muss die folgenden Regeln erfüllen:

*  Der Typ muss ein Schnittstellentyp sein.
*  Ein Typ darf in einer gegebenen `where` Klausel nicht mehrmals angegeben werden.

In beiden Fällen kann die Einschränkung einen der Typparameter der zugeordneten Typ-oder Methoden Deklaration als Teil eines konstruierten Typs einschließen und den Typ einschließen, der deklariert wird.

Jeder Klassen-oder Schnittstellentyp, der als Typparameter Einschränkung angegeben ist, muss mindestens so zugänglich sein (Barrierefreiheits[Einschränkungen](basic-concepts.md#accessibility-constraints)) wie der generische Typ oder die Methode, der deklariert wird.

Ein Typ, der als *type_parameter* -Einschränkung angegeben ist, muss die folgenden Regeln erfüllen:

*  Der Typ muss ein Typparameter sein.
*  Ein Typ darf in einer gegebenen `where` Klausel nicht mehrmals angegeben werden.

Außerdem dürfen im Abhängigkeits Diagramm der Typparameter keine Zyklen vorhanden sein, bei denen die Abhängigkeit eine transitiv Beziehung ist, die durch definiert wird:

*  Wenn ein Typparameter `T` als Einschränkung für den `S` Typparameter `S` verwendet wird, ***hängt von ab*** `T`.
*  Wenn ein Typparameter `S` von einem Typparameter `T` abhängt und `T` von einem Typparameter `U` `U` `S` abhängt, hängt von ***ab*** .

Bei dieser Beziehung handelt es sich um einen Kompilierzeitfehler für einen Typparameter, der direkt oder indirekt von sich selbst abhängig ist.

Alle Einschränkungen müssen zwischen abhängigen Typparametern einheitlich sein. Wenn der Typparameter `S` vom Typparameter `T` abhängt, dann:

*  `T`darf nicht über die Werttyp Einschränkung verfügen. Andernfalls ist tatsächlich versiegelt, sodass `S` gezwungen wird, denselben Typ wie `T`zu haben, sodass zwei Typparameter nicht mehr benötigt werden. `T`
*  Wenn `S` die Werttyp Einschränkung aufweist, darf `T` keine *class_type* -Einschränkung aufweisen.
*  Wenn `S` über eine *class_type* -Einschränkung verfügt `A` und `T` eine *class_type* -Einschränkung `B`, muss eine Identitäts Konvertierung oder eine implizite Verweis Konvertierung von `A` in `B` oder eine implizite Verweis Konvertierung von `B` auf `A`.
*  Wenn `S` auch vom Typparameter `U` und `U` über eine *class_type* -Einschränkung verfügt `A` und `T` eine *class_type* -Einschränkung `B`, muss eine Identitäts Konvertierung oder eine implizite Verweis Konvertierung von `A` erfolgen. zum `B` oder eine implizite Verweis Konvertierung von 0 in 1.

Es ist zulässig, `S` dass die Werttyp Einschränkung und `T` die Verweistyp Einschränkung aufweisen. Dies schränkt `T` praktisch die Typen `System.Object`, `System.ValueType`, `System.Enum`und alle Schnittstellentypen ein.

Wenn die `where` -Klausel für einen Typparameter eine Konstruktoreinschränkung (die `new()`das-Format aufweist) enthält, kann der `new` -Operator verwendet werden, um Instanzen des-Typs ([Objekt Erstellungs Ausdrücke](expressions.md#object-creation-expressions)) zu erstellen. Alle Typargumente, die für einen Typparameter mit einer Konstruktoreinschränkung verwendet werden, müssen über einen öffentlichen Parameter losen Konstruktor verfügen (dieser Konstruktor ist implizit für jeden Werttyp vorhanden), oder es handelt sich um einen Typparameter mit der Werttyp Einschränkung oder Konstruktoreinschränkung (siehe [Typparameter Einschränkungen](classes.md#type-parameter-constraints) für Details).

Im folgenden finden Sie Beispiele für Einschränkungen:
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

Das folgende Beispiel ist fehlerhaft, da es eine Zirkularität im Abhängigkeits Diagramm der Typparameter verursacht:
```csharp
class Circular<S,T>
    where S: T
    where T: S                // Error, circularity in dependency graph
{
    ...
}
```

In den folgenden Beispielen werden zusätzliche ungültige Situationen veranschaulicht:
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

Die ***effektive Basisklasse*** eines Typparameters `T` wird wie folgt definiert:

*  Wenn `T` keine Primary-Einschränkungen oder Typparameter Einschränkungen aufweist, ist `object`die effektive Basisklasse.
*  Wenn `T` die Werttyp Einschränkung aufweist, ist `System.ValueType`die effektive Basisklasse.
*  Wenn `T` über eine *class_type* -Einschränkung `C`, aber keine *type_parameter* -Einschränkungen verfügt, ist die effektive Basisklasse `C`.
*  Wenn `T` keine *class_type* -Einschränkung hat, aber mindestens eine *type_parameter* -Einschränkung aufweist, handelt es sich bei der effektiven Basisklasse um den am häufigsten als Typ ([gesteigerte Konvertierungs Operator](conversions.md#lifted-conversion-operators)) im Satz effektiver Basisklassen der *Type_ Parameter* Einschränkungen. Durch die Konsistenzregeln wird sichergestellt, dass ein solcher Typ vorhanden ist.
*  Wenn `T` sowohl eine *class_type* -Einschränkung als auch eine oder mehrere *type_parameter* -Einschränkungen aufweist, ist die effektive Basisklasse der am meisten eingeschlossenen Typ ([gesteigerte Konvertierungs Operatoren](conversions.md#lifted-conversion-operators)) in der Menge, die aus dem *class_type* besteht. Einschränkung von `T` und den effektiven Basisklassen der *type_parameter* -Einschränkungen. Durch die Konsistenzregeln wird sichergestellt, dass ein solcher Typ vorhanden ist.
*  Wenn `T` die Verweistyp Einschränkung, aber keine *class_type* -Einschränkungen aufweist, ist die effektive Basisklasse `object`.

Verwenden Sie für diese Regeln stattdessen den spezifischsten Basistyp von `V`, bei dem es sich um eine Einschränkung `V` handelt, bei der es sich um eine *value_type* *handelt.* Dies kann in einer explizit angegebenen Einschränkung nie vorkommen, kann jedoch auftreten, wenn die Einschränkungen einer generischen Methode implizit von einer über schreibenden Methoden Deklaration oder einer expliziten Implementierung einer Schnittstellen Methode geerbt werden.

Diese Regeln stellen sicher, dass die effektive Basisklasse immer ein *class_type*ist.

Der ***effektive Schnittstellen Satz*** eines Typparameters `T` wird wie folgt definiert:

*  Wenn `T` keinen *secondary_constraints*hat, ist der effektive Schnittstellen Satz leer.
*  Wenn `T` *INTERFACE_TYPE* -Einschränkungen, aber keine *type_parameter* -Einschränkungen aufweist, ist der effektive Schnittstellen Satz der Satz von *INTERFACE_TYPE* -Einschränkungen.
*  Wenn `T` keine *INTERFACE_TYPE* -Einschränkungen aufweist, aber über *type_parameter* -Einschränkungen verfügt, ist der effektive Schnittstellen Satz die Vereinigung der effektiven Schnittstellen Sätze der *type_parameter* -Einschränkungen.
*  Wenn `T` sowohl *INTERFACE_TYPE* -Einschränkungen als auch *type_parameter* -Einschränkungen aufweist, ist der effektive Schnittstellen Satz die Kombination aus dem Satz von *INTERFACE_TYPE* -Einschränkungen und den effektiven Schnittstellen Sätzen seiner *type_parameter* Auflagen.

Ein Typparameter ist ***bekanntermaßen ein Referenztyp,*** wenn er über die Verweistyp Einschränkung oder seine effektive Basisklasse nicht `object` oder `System.ValueType`ist.

Werte eines eingeschränkten Typparameter Typs können für den Zugriff auf die Instanzmember verwendet werden, die durch die Einschränkungen impliziert sind. Im Beispiel
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
die-Methoden `IPrintable` von können direkt auf `x` aufgerufen werden `T` , da eingeschränkt ist, `IPrintable`um immer zu implementieren.

### <a name="class-body"></a>Klassen Text

Der *class_body* einer Klasse definiert die Member dieser Klasse.

```antlr
class_body
    : '{' class_member_declaration* '}'
    ;
```

## <a name="partial-types"></a>Partial types (Partielle Typen)

Eine Typdeklaration kann über mehrere ***partielle Typdeklarationen***hinweg aufgeteilt werden. Die Typdeklaration wird anhand der in diesem Abschnitt aufgeführten Regeln erstellt, woraufhin Sie im Rest der Kompilierzeit-und Lauf Zeit Verarbeitung des Programms als eine einzige Deklaration behandelt wird.

Eine *class_declaration*, *struct_declaration* oder *interface_declaration* stellt eine partielle Typdeklaration dar, wenn Sie einen `partial`-Modifizierer enthält. `partial`ist kein Schlüsselwort und fungiert nur als Modifizierer, wenn er unmittelbar vor `class`einem der Schlüsselwörter `struct` oder `interface` in einer Typdeklaration oder vor dem Typ `void` in einer Methoden Deklaration angezeigt wird. In anderen Kontexten kann es als normaler Bezeichner verwendet werden.

Jeder Teil einer partiellen Typdeklaration muss einen `partial` Modifizierer enthalten. Er muss denselben Namen haben und in derselben Namespace-oder Typdeklaration wie die anderen Teile deklariert werden. Der `partial` -Modifizierer gibt an, dass zusätzliche Teile der Typdeklaration an anderer Stelle vorhanden sein können, aber das vorhanden sein solcher zusätzlicher Teile ist nicht erforderlich. es ist für einen Typ mit einer `partial` einzelnen Deklaration gültig, den Modifizierer einzuschließen.

Alle Teile eines partiellen Typs müssen zusammen kompiliert werden, sodass die Teile zur Kompilierzeit in eine einzelne Typdeklaration zusammengeführt werden können. Bei partiellen Typen können nicht bereits kompilierte Typen erweitert werden.

Mit dem-Modifizierer können mit dem `partial` -Modifizierer in mehreren Teilen deklarierte Typen deklariert werden. In der Regel wird der enthaltende Typ `partial` auch mit deklariert, und jeder Teil des untergeordneten Typs wird in einem anderen Teil des enthaltenden Typs deklariert.

Der `partial` -Modifizierer ist in Delegaten-oder Enumerationsdeklarationen unzulässig.

### <a name="attributes"></a>Attribute

Die Attribute eines partiellen Typs werden festgelegt, indem die Attribute der einzelnen Teile in einer nicht angegebenen Reihenfolge kombiniert werden. Wenn ein Attribut in mehreren Teilen platziert wird, entspricht es dem mehrfachen angeben des Attributs für den Typ. Die beiden Teile sind z. b.:

```csharp
[Attr1, Attr2("hello")]
partial class A {}

[Attr3, Attr2("goodbye")]
partial class A {}
```
Äquivalent zu einer Deklaration, z. b.:
```csharp
[Attr1, Attr2("hello"), Attr3, Attr2("goodbye")]
class A {}
```

Attribute für Typparameter werden auf ähnliche Weise kombiniert.

### <a name="modifiers"></a>Modifizierer

Wenn eine partielle Typdeklaration eine Barrierefreiheits Spezifikation `public`( `protected`die `internal`Modifizierer,, und `private` ) enthält, muss Sie mit allen anderen Teilen übereinstimmen, die eine Barrierefreiheits Spezifikation enthalten. Wenn kein Teil eines partiellen Typs eine Barrierefreiheits Spezifikation enthält, erhält der Typ die entsprechende Standard Barrierefreiheit (als[Barrierefreiheit deklariert](basic-concepts.md#declared-accessibility)).

Wenn eine oder mehrere partielle Deklarationen eines in einem Typ geänderten Typs einen `new` Modifizierer enthalten, wird keine Warnung ausgegeben, wenn der Typ eines geerbten Members ([durch Vererbung](basic-concepts.md#hiding-through-inheritance)ausblenden) ausgeblendet wird.

Wenn eine oder mehrere partielle Deklarationen einer Klasse einen `abstract` Modifizierer enthalten, gilt die Klasse als abstrakt ([abstrakte Klassen](classes.md#abstract-classes)). Andernfalls gilt die Klasse als nicht abstrakt.

Wenn mindestens eine partielle Deklaration einer Klasse einen `sealed` Modifizierer enthält, gilt die Klasse als versiegelt ([versiegelte Klassen](classes.md#sealed-classes)). Andernfalls wird die Klasse als nicht versiegelt angesehen.

Beachten Sie, dass eine Klasse nicht gleichzeitig abstrakt und versiegelt sein kann.

Wenn der `unsafe` -Modifizierer für eine partielle Typdeklaration verwendet wird, wird nur dieser bestimmte Teil als unsicherer Kontext ([unsichere Kontexte](unsafe-code.md#unsafe-contexts)) betrachtet.

### <a name="type-parameters-and-constraints"></a>Typparameter und Einschränkungen

Wenn ein generischer Typ in mehreren Teilen deklariert ist, muss jeder Teil die Typparameter angeben. Jeder Teil muss die gleiche Anzahl von Typparametern und den gleichen Namen für jeden Typparameter in der richtigen Reihenfolge aufweisen.

Wenn eine partielle generische Typdeklaration Einschränkungen`where` (Klauseln) enthält, müssen die Einschränkungen allen anderen Teilen, die Einschränkungen einschließen, zustimmen. Insbesondere müssen alle Teile, die Einschränkungen enthalten, Einschränkungen für denselben Satz von Typparametern aufweisen, und für jeden Typparameter müssen die Sätze der primären, sekundären und Konstruktoreinschränkungen gleichwertig sein. Zwei Sätze von Einschränkungen sind äquivalent, wenn Sie dieselben Member enthalten. Wenn kein Teil eines partiellen generischen Typs Typparameter Einschränkungen angibt, gelten die Typparameter als nicht eingeschränkt.

Das Beispiel
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
ist richtig, da diese Teile, die Einschränkungen enthalten (die ersten beiden), effektiv denselben Satz von primären, sekundären und Konstruktoreinschränkungen für denselben Satz von Typparametern angeben.

### <a name="base-class"></a>Basisklasse

Wenn eine partielle Klassen Deklaration eine Basisklassen Spezifikation enthält, muss Sie mit allen anderen Teilen übereinstimmen, die eine Basisklassen Spezifikation enthalten. Wenn kein Teil einer partiellen Klasse eine Basisklassen Spezifikation enthält, wird die Basisklasse `System.Object` zu ([Basisklassen](classes.md#base-classes)).

### <a name="base-interfaces"></a>Basis Schnittstellen

Der Satz von Basis Schnittstellen für einen in mehreren Teilen deklarierten Typ ist die Vereinigung der Basis Schnittstellen, die für jeden Teil angegeben werden. Eine bestimmte Basisschnittstelle kann nur einmal pro Teil benannt werden, es ist jedoch zulässig, dass mehrere Teile die gleichen Basis Schnittstellen benennen. Es darf nur eine Implementierung der Member einer bestimmten Basisschnittstelle vorhanden sein.

Im Beispiel
```csharp
partial class C: IA, IB {...}

partial class C: IC {...}

partial class C: IA, IB {...}
```
der Satz von Basis Schnittstellen für die `C` - `IA`Klasse `IB`ist, `IC`und.

In der Regel stellt jeder Teil eine Implementierung der Schnittstellen bereit, die für diesen Teil deklariert werden. Dies ist jedoch keine Voraussetzung. Ein Teil kann die Implementierung für eine Schnittstelle bereitstellen, die für einen anderen Teil deklariert wurde:
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

Mit Ausnahme von partiellen Methoden ([partielle Methoden](classes.md#partial-methods)) ist der Satz von Membern eines Typs, der in mehreren Teilen deklariert wurde, einfach die Vereinigung der Menge von Membern, die in jedem Teil deklariert werden. Die Texte aller Teile der Typdeklaration verwenden denselben Deklarations Bereich ([Deklarationen](basic-concepts.md#declarations)), und der[Gültigkeits](basic-concepts.md#scopes)Bereich der einzelnen Member (Bereiche) erstreckt sich auf den Text aller Teile. Die Zugriffs Domäne eines beliebigen Members enthält immer alle Teile des einschließenden Typs. ein `private` Member, der in einem Teil deklariert ist, kann aus einem anderen Teil frei zugänglich sein. Es handelt sich um einen Kompilierzeitfehler, um denselben Member in mehr als einem Teil des Typs zu deklarieren, es sei denn, dieser `partial` Member ist ein Typ mit dem-Modifizierer.

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

Die Reihenfolge von Membern innerhalb eines Typs ist nur C# selten wichtig für Code, kann jedoch bei der Schnittstelle mit anderen Sprachen und Umgebungen von Bedeutung sein. In diesen Fällen ist die Reihenfolge von Membern innerhalb eines in mehreren Teilen deklarierten Typs nicht definiert.

### <a name="partial-methods"></a>Partielle Methoden

Partielle Methoden können in einem Teil einer Typdeklaration definiert und in einem anderen implementiert werden. Die Implementierung ist optional. Wenn kein Teil die partielle Methode implementiert, werden die Deklaration der partiellen Methode und alle Aufrufe an die Deklaration aus der Typdeklaration entfernt, die sich aus der Kombination der Teile ergibt.

Partielle Methoden können keine Zugriffsmodifizierer definieren, sondern implizit `private`. Der Rückgabetyp `void`muss sein, und ihre Parameter dürfen `out` nicht den-Modifizierer aufweisen. Der Bezeichner `partial` wird in einer Methoden Deklaration nur dann als sonderschlüsselwort erkannt, wenn `void` er direkt vor dem Typ angezeigt wird. andernfalls kann er als normaler Bezeichner verwendet werden. Eine partielle Methode kann Schnittstellen Methoden nicht explizit implementieren.

Es gibt zwei Arten von partiellen Methoden Deklarationen: Wenn der Text der Methoden Deklaration ein Semikolon ist, wird die Deklaration als eine ***definierende partielle Methoden Deklaration***bezeichnet. Wenn der Text als- *Block*angegeben wird, wird die Deklaration als eine ***implementierende partielle Methoden Deklaration***bezeichnet. In den Teilen einer Typdeklaration darf nur eine partielle Methoden Deklaration mit einer bestimmten Signatur definiert werden, und es kann nur eine partielle Methoden Deklaration mit einer bestimmten Signatur implementiert werden. Wenn eine implementierende partielle Methoden Deklaration angegeben wird, muss eine entsprechende definierende partielle Methoden Deklaration vorhanden sein, und die Deklarationen müssen übereinstimmen, wie im folgenden angegeben:

* Die Deklarationen müssen die gleichen modifiziererer (auch nicht unbedingt in derselben Reihenfolge), den Methodennamen, die Anzahl der Typparameter und die Anzahl von Parametern aufweisen.
* Die entsprechenden Parameter in den Deklarationen müssen dieselben Modifizierer aufweisen (obwohl Sie nicht notwendigerweise in derselben Reihenfolge sind) und dieselben Typen (Modulo-Unterschiede in Typparameter Namen).
* Die entsprechenden Typparameter in den Deklarationen müssen dieselben Einschränkungen aufweisen (Modulo-Unterschiede in Typparameter Namen).

Eine implementierende partielle Methoden Deklaration kann im gleichen Teil wie die entsprechende definierende partielle Methoden Deklaration vorkommen.

Nur eine definierende partielle Methode ist an der Überladungs Auflösung beteiligt. Unabhängig davon, ob eine implementierende Deklaration angegeben wird, können Aufruf Ausdrücke in Aufrufe der partiellen Methode aufgelöst werden. Da eine partielle Methode immer `void`zurückgibt, sind solche Aufruf Ausdrücke immer Ausdrucks Anweisungen. Da eine partielle Methode implizit `private`ist, treten diese Anweisungen immer innerhalb eines der Teile der Typdeklaration auf, in der die partielle Methode deklariert ist.

Wenn kein Teil einer partiellen Typdeklaration eine implementierende Deklaration für eine bestimmte partielle Methode enthält, wird jede Ausdrucks Anweisung, die Sie aufruft, einfach aus der kombinierten Typdeklaration entfernt. Folglich hat der Aufruf Ausdruck, einschließlich aller konstituierender Ausdrücke, keine Auswirkung zur Laufzeit. Die partielle Methode selbst wird ebenfalls entfernt und ist kein Member der kombinierten Typdeklaration.

Wenn eine implementierende Deklaration für eine bestimmte partielle Methode vorhanden ist, werden die Aufrufe der partiellen Methoden beibehalten. Die partielle Methode führt zu einer Methoden Deklaration, die der Implementierung der partiellen Methoden Deklaration ähnelt, mit Ausnahme folgender:

* Der `partial` -Modifizierer ist nicht eingeschlossen.
* Die Attribute in der resultierenden Methoden Deklaration sind die kombinierten Attribute der definierenden und der implementierenden partiellen Methoden Deklaration in einer nicht angegebenen Reihenfolge. Duplikate werden nicht entfernt.
* Die Attribute für die Parameter der resultierenden Methoden Deklaration sind die kombinierten Attribute der entsprechenden Parameter der definierenden und der implementierenden partiellen Methoden Deklaration in einer nicht angegebenen Reihenfolge. Duplikate werden nicht entfernt.

Wenn eine definierende Deklaration, aber keine implementierende Deklaration für eine partielle Methode M angegeben wird, gelten die folgenden Einschränkungen:

* Es handelt sich um einen Kompilierzeitfehler zum Erstellen eines Delegaten für die Methode ([delegaterstellungs-Ausdrücke](expressions.md#delegate-creation-expressions)).
* Es handelt sich um einen Kompilierzeitfehler, `M` auf den in einer anonymen Funktion verwiesen wird, die in einen Ausdrucks Strukturtyp konvertiert wird ([Auswertung anonymer Funktions Konvertierungen in Ausdrucks Baumstruktur Typen](conversions.md#evaluation-of-anonymous-function-conversions-to-expression-tree-types)).
* Ausdrücke, die als Teil des aufzurufenden von auftreten, wirken sich nicht auf den eindeutigen Zuweisungs Zustand ( `M` [definitive Zuweisung](variables.md#definite-assignment)) aus, der potenziell zu Kompilier Zeitfehlern führen kann.
* `M`kann nicht der Einstiegspunkt für eine Anwendung ([Anwendungsstart](basic-concepts.md#application-startup)) sein.

Partielle Methoden sind hilfreich, um einem Teil einer Typdeklaration das Anpassen des Verhaltens eines anderen Teils zu ermöglichen, z. b. eines, das von einem Tool generiert wird. Beachten Sie die folgende Deklaration der partiellen Klasse:
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

Wenn diese Klasse ohne andere Teile kompiliert wird, werden die definierenden partiellen Methoden Deklarationen und deren Aufrufe entfernt, und die resultierende kombinierte Klassen Deklaration entspricht Folgendem:
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

Angenommen, es wird jedoch ein anderer Teil angegeben, der die Implementierung von Deklarationen der partiellen Methoden bereitstellt:
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

Dann entspricht die resultierende kombinierte Klassen Deklaration folgendem:
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

### <a name="name-binding"></a>Namens Bindung

Obwohl jeder Teil eines erweiterbaren Typs innerhalb desselben Namespace deklariert werden muss, werden die Teile in der Regel in verschiedenen Namespace Deklarationen geschrieben. Daher können für `using` jeden Teil verschiedene Direktiven ([using-Direktiven](namespaces.md#using-directives)) vorhanden sein. Bei der Interpretation von einfachen Namen ([Typrückschluss](expressions.md#type-inference)) innerhalb eines Teils `using` werden nur die Direktiven der Namespace Deklaration (en) berücksichtigt, die diesen Teil einschließen. Dies kann dazu führen, dass derselbe Bezeichner in unterschiedlichen Teilen mit unterschiedlichen Bedeutungen übereinstimmen:
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

Die Member einer Klasse bestehen aus den Membern, die von den *class_member_declaration*s eingeführt wurden, und den Membern, die von der direkten Basisklasse geerbt wurden.

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

Die Member eines Klassen Typs sind in die folgenden Kategorien unterteilt:

*  Konstanten, die Konstante Werte darstellen, die der-Klasse ([Konstanten](classes.md#constants)) zugeordnet sind.
*  Felder, bei denen es sich um die Variablen der-Klasse ([Felder](classes.md#fields)) handelt.
*  -Methoden, die die Berechnungen und Aktionen implementieren, die von der-Klasse ausgeführt werden können (-[Methoden](classes.md#methods)).
*  Eigenschaften, die benannte Merkmale und die Aktionen definieren, die mit dem Lesen und Schreiben dieser Eigenschaften verknüpft sind ([Eigenschaften](classes.md#properties)).
*  Ereignisse, die Benachrichtigungen definieren, die von der-Klasse ([Ereignissen](classes.md#events)) generiert werden können.
*  Indexer, die es ermöglichen, Instanzen der Klasse auf die gleiche Weise (syntaktisch) als Arrays ([Indexer](classes.md#indexers)) zu indizieren.
*  Operatoren, die die Ausdrucks Operatoren definieren, die auf Instanzen der-Klasse ([Operatoren](classes.md#operators)) angewendet werden können.
*  Instanzkonstruktoren, die die zum Initialisieren von Instanzen der-Klasse erforderlichen Aktionen implementieren ([Instanzkonstruktoren](classes.md#instance-constructors)).
*  Dekonstruktoren, die die auszuführenden Aktionen implementieren, bevor Instanzen der Klasse dauerhaft verworfen werden ([Dekonstruktoren](classes.md#destructors)).
*  Statische Konstruktoren, die die zum Initialisieren der Klasse selbst erforderlichen Aktionen implementieren ([statische Konstruktoren](classes.md#static-constructors)).
*  Typen, die die Typen darstellen, die für die-Klasse lokal sind (unter[Typen](classes.md#nested-types)).

Member, die ausführbaren Code enthalten können, werden zusammen mit den Funktionsmembern des Klassen Typs bezeichnet. Die Funktionsmember eines Klassen Typs sind die Methoden, Eigenschaften, Ereignisse, Indexer, Operatoren, Instanzkonstruktoren, destrukturtoren und statischen Konstruktoren dieses Klassen Typs.

Ein *class_declaration* erstellt einen neuen Deklarations Raum ([Deklarationen](basic-concepts.md#declarations)), und die *class_member_declaration*-Elemente, die direkt in der *class_declaration* enthalten sind, stellen neue Member in diesen Deklarations Bereich ein. Die folgenden Regeln gelten für *class_member_declaration*s:

*  Instanzkonstruktoren, Dekonstruktoren und statische Konstruktoren müssen den gleichen Namen wie die unmittelbar einschließende Klasse haben. Alle anderen Member müssen Namen haben, die sich von dem Namen der unmittelbar einschließenden Klasse unterscheiden.
*  Der Name einer Konstante, eines Felds, einer Eigenschaft, eines Ereignisses oder eines Typs muss sich von den Namen aller anderen Member unterscheiden, die in derselben Klasse deklariert sind.
*  Der Name einer Methode muss sich von den Namen aller anderen nicht-Methoden unterscheiden, die in derselben Klasse deklariert sind. Außerdem müssen sich die Signatur ([Signaturen und überladen](basic-concepts.md#signatures-and-overloading)) einer Methode von den Signaturen aller anderen Methoden unterscheiden, die in derselben Klasse deklariert sind, und zwei Methoden, die in derselben Klasse deklariert sind, dürfen keine Signaturen aufweisen, `ref` die sich ausschließlich durch und unterscheiden. `out`.
*  Die Signatur eines Instanzkonstruktors muss sich von den Signaturen aller anderen Instanzkonstruktoren unterscheiden, die in derselben Klasse deklariert sind, und zwei in derselben Klasse deklarierte Konstruktoren verfügen möglicherweise nicht `ref` über `out`Signaturen, die sich ausschließlich durch und unterscheiden.
*  Die Signatur eines Indexer muss sich von den Signaturen aller anderen Indexer unterscheiden, die in derselben Klasse deklariert sind.
*  Die Signatur eines Operators muss sich von den Signaturen aller anderen Operatoren unterscheiden, die in derselben Klasse deklariert sind.

Die geerbten Member eines Klassen Typs ([Vererbung](classes.md#inheritance)) sind nicht Teil des Deklarations Raums einer Klasse. Folglich kann eine abgeleitete Klasse einen Member mit demselben Namen oder derselben Signatur wie ein geerbten Member deklarieren (wodurch der geerbte Member in der Tat ausgeblendet wird).

### <a name="the-instance-type"></a>Der Instanztyp.

Jede Klassen Deklaration verfügt über einen zugeordneten gebundenen Typ ([gebundene und ungebundene Typen](types.md#bound-and-unbound-types)), den ***Instanztyp***. Bei einer generischen Klassen Deklaration wird der Instanztyp durch Erstellen eines konstruierten Typs ([konstruierte Typen](types.md#constructed-types)) aus der Typdeklaration gebildet, wobei jedes der angegebenen Typargumente der entsprechende Typparameter ist. Da der Instanztyp die Typparameter verwendet, kann er nur dort verwendet werden, wo sich die Typparameter im Gültigkeitsbereich befinden. Das heißt, innerhalb der Klassen Deklaration. Der Instanztyp ist der Typ `this` von für Code, der innerhalb der Klassen Deklaration geschrieben wurde. Bei nicht generischen Klassen ist der Instanztyp einfach die deklarierte Klasse. Das folgende Beispiel zeigt mehrere Klassen Deklarationen zusammen mit ihren Instanztypen: 
```csharp
class A<T>                           // instance type: A<T>
{
    class B {}                       // instance type: A<T>.B
    class C<U> {}                    // instance type: A<T>.C<U>
}

class D {}                           // instance type: D
```

### <a name="members-of-constructed-types"></a>Member konstruierter Typen

Die nicht geerbten Member eines konstruierten Typs werden abgerufen, indem für jede *type_parameter* in der Element Deklaration der entsprechende *type_argument* des konstruierten Typs ersetzt wird. Der Ersetzungs Vorgang basiert auf der semantischen Bedeutung von Typdeklarationen und ist nicht einfach die Text Ersetzung.

Beispielsweise bei der Deklaration der generischen Klasse
```csharp
class Gen<T,U>
{
    public T[,] a;
    public void G(int i, T t, Gen<U,T> gt) {...}
    public U Prop { get {...} set {...} }
    public int H(double d) {...}
}
```
der konstruierte Typ `Gen<int[],IComparable<string>>` verfügt über die folgenden Member:
```csharp
public int[,][] a;
public void G(int i, int[] t, Gen<IComparable<string>,int[]> gt) {...}
public IComparable<string> Prop { get {...} set {...} }
public int H(double d) {...}
```

Der Typ des `a` Members in der generischen Klassen Deklaration `Gen` ist das zweidimensionale Array von `T`, sodass der Typ des `a` Members im konstruierten Typ oben "zweidimensionales Array eines eindimensionalen Arrays aus "ist.`int`", oder `int[,][]`.

Innerhalb von instanzfunktionsmembern `this` ist der Typ von der Instanztyp ([der Instanztyp](classes.md#the-instance-type)) der enthaltenden Deklaration.

Alle Member einer generischen Klasse können Typparameter aus einer beliebigen einschließenden Klasse verwenden, entweder direkt oder als Teil eines konstruierten Typs. Wenn ein bestimmter, von einem Typ geschlossenes konstruierter Typ ([Open-und Closed-Typen](types.md#open-and-closed-types)) zur Laufzeit verwendet wird, wird jede Verwendung eines Typparameters durch das tatsächliche Typargument ersetzt, das für den konstruierten Typ angegeben wird. Zum Beispiel:
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

Eine Klasse ***erbt*** die Member ihres direkten Basisklassen Typs. Vererbung bedeutet, dass eine Klasse implizit alle Member ihres direkten Basisklassen Typs enthält, mit Ausnahme der Instanzkonstruktoren, Dekonstruktoren und statischer Konstruktoren der Basisklasse. Einige wichtige Aspekte der Vererbung sind:

*  Vererbung ist transitiv. Wenn `C` von `C` `A` `B` abgeleitet ist und `B` von`A`abgeleitet ist, erbt die in deklarierten Member sowie die in deklarierten Member. `B`
*  Eine abgeleitete Klasse erweitert die direkte Basisklasse. Eine abgeleitete Klasse kann den geerbten Membern neue Member hinzufügen, aber die Definition eines geerbten Members kann nicht entfernt werden.
*  Instanzkonstruktoren, destrukturtoren und statische Konstruktoren werden nicht geerbt, aber alle anderen Member sind, unabhängig von ihrer deklarierten Barrierefreiheit ([Member Access](basic-concepts.md#member-access)). Abhängig von der deklarierten Barrierefreiheit sind vererbte Member jedoch möglicherweise in einer abgeleiteten Klasse nicht zugänglich.
*  Eine abgeleitete Klasse kann vererbte Member ausblenden ([durch Vererbung](basic-concepts.md#hiding-through-inheritance) ***Ausblenden*** ), indem neue Member mit demselben Namen oder derselben Signatur deklariert werden. Beachten Sie jedoch, dass beim Ausblenden eines geerbten Members dieser Member nicht entfernt wird – er macht diesen Member lediglich direkt über die abgeleitete Klasse zugänglich.
*  Eine Instanz einer Klasse enthält einen Satz aller Instanzfelder, die in der-Klasse und den zugehörigen Basisklassen deklariert sind, und eine implizite Konvertierung ([implizite Verweis Konvertierungen](conversions.md#implicit-reference-conversions)) ist von einem abgeleiteten Klassentyp zu einem der zugehörigen Basisklassen Typen vorhanden. Folglich kann ein Verweis auf eine Instanz einer abgeleiteten Klasse als Verweis auf eine Instanz der zugehörigen Basisklassen behandelt werden.
*  Eine Klasse kann virtuelle Methoden, Eigenschaften und Indexer deklarieren, und abgeleitete Klassen können die Implementierung dieser Funktionsmember überschreiben. Dadurch können Klassen polymorphes Verhalten darstellen, wobei die von einem Funktionselement Aufruf ausgeführten Aktionen je nach Lauf Zeittyp der Instanz variieren, durch die der Funktions Member aufgerufen wird.

Der geerbte Member eines konstruierten Klassen Typs sind die Member des unmittelbaren Basisklassen Typs ([Basisklassen](classes.md#base-classes)), die gefunden werden, indem die Typargumente des konstruierten Typs für jedes Vorkommen der entsprechenden Typparameter im  *class_base* -Spezifikation. Diese Member werden wiederum transformiert, indem Sie für jede *type_parameter* in der Element Deklaration den entsprechenden *type_argument* der *class_base* -Spezifikation ersetzen.

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

Im obigen Beispiel hat der konstruierte `D<int>` Typ einen nicht geerbten Member `public int G(string s)` , der durch das Typargument `int` für den Typparameter `T`abgerufen wurde. `D<int>`verfügt auch über einen geerbten Member aus der `B`Klassen Deklaration. Dieser geerbte Member wird bestimmt, indem zuerst `B<int[]>` der Basis Klassentyp `D<int>` bestimmt wird `int` , `T` indem in der Basisklassen `B<T[]>`Spezifikation ersetzt wird. Anschließend `int[]` wird als Typargument `B`für `U` in `public U F(long index)`durch ersetzt, wodurch der geerbte Member `public int[] F(long index)`bereitstellt wird.

### <a name="the-new-modifier"></a>Der New-Modifizierer.

Ein *class_member_declaration* -Element darf einen Member mit demselben Namen oder derselben Signatur wie ein geerbte Member deklarieren. In diesem Fall wird der Member der abgeleiteten Klasse zum ***Ausblenden*** des Basisklassenmembers bezeichnet. Das Ausblenden eines geerbten Members wird nicht als Fehler betrachtet, bewirkt jedoch, dass der Compiler eine Warnung ausgibt. Um die Warnung zu unterdrücken, kann die Deklaration des Members der abgeleiteten Klasse einen `new` -Modifizierer einschließen, um anzugeben, dass der abgeleitete Member den Basismember ausblenden soll. Dieses Thema wird unter Ausblenden [durch Vererbung](basic-concepts.md#hiding-through-inheritance)ausführlicher erläutert.

Wenn ein `new` Modifizierer in einer Deklaration enthalten ist, die einen geerbten Member nicht verbirgt, wird eine Warnung für diesen Effekt ausgegeben. Diese Warnung wird durch Entfernen des `new` -Modifizierers unterdrückt.

### <a name="access-modifiers"></a>Modifizierer für Zugriffe

Ein *class_member_declaration* kann eine der fünf möglichen Arten von deklarierten Barrierefreiheits ([deklarierter Barrierefreiheit](basic-concepts.md#declared-accessibility)) haben: `public`, `protected internal`, `protected`, `internal` oder `private`. Mit Ausnahme der `protected internal` Kombination ist es ein Kompilierzeitfehler, mehr als einen Zugriffsmodifizierer anzugeben. Wenn ein *class_member_declaration* keine Zugriffsmodifizierer enthält, wird `private` angenommen.

### <a name="constituent-types"></a>Konstituierende Typen

Typen, die in der Deklaration eines Members verwendet werden, werden als konstituierende Typen dieses Members bezeichnet. Mögliche Typen sind der Typ einer Konstante, eines Felds, einer Eigenschaft, eines Ereignisses oder eines Indexers, der Rückgabetyp einer Methode oder eines Operators und die Parametertypen einer Methode, eines Indexers, eines Operators oder eines Instanzkonstruktors. Die einzelnen Typen eines Members müssen mindestens so zugänglich sein, wie der Member selbst (Barrierefreiheits[Einschränkungen](basic-concepts.md#accessibility-constraints)).

### <a name="static-and-instance-members"></a>Statische Member und Instanzmember

Member einer Klasse sind entweder ***statische*** Member oder ***Instanzmember***. Im Allgemeinen ist es sinnvoll, statische Member als zu den Klassentypen und Instanzmembern gehörend zu betrachten (Instanzen von Klassentypen).

Wenn ein Feld, eine Methode, eine Eigenschaft, eine Ereignis-, Operator-oder Konstruktordeklaration einen `static` -Modifizierer enthält, wird ein statischer Member deklariert. Außerdem deklariert eine Konstante oder Typdeklaration implizit einen statischen Member. Statische Member haben die folgenden Merkmale:

*  Wenn auf einen statischen Member `M` in einem *member_access* -Element ([Member Access](expressions.md#member-access)) der Form `E.M` verwiesen wird, muss `E` einen Typ mit `M` bezeichnen. Es handelt sich um einen Kompilierzeitfehler `E` , mit dem eine Instanz bezeichnet wird.
*  Ein statisches Feld identifiziert genau einen Speicherort, der von allen Instanzen eines gegebenen geschlossenen Klassen Typs freigegeben werden soll. Unabhängig davon, wie viele Instanzen eines bestimmten geschlossenen Klassen Typs erstellt werden, gibt es nur eine Kopie eines statischen Felds.
*  Ein statisches Funktionsmember (Methode, Eigenschaft, Ereignis, Operator oder Konstruktor) funktioniert nicht für eine bestimmte Instanz, und es handelt sich um einen Kompilierzeitfehler, `this` auf den in einem derartigen Funktionsmember verwiesen wird.

Wenn ein Feld, eine Methode, eine Eigenschaft, ein Ereignis, ein Indexer, ein Konstruktor oder eine `static` destrukturerdeklaration keinen Modifizierer enthält, wird ein Instanzmember deklariert. (Ein Instanzmember wird manchmal als nicht statischer Member bezeichnet.) Instanzmember haben die folgenden Merkmale:

*  Wenn auf einen Instanzmember `M` in einem *member_access* ([Member Access](expressions.md#member-access)) der Form `E.M` verwiesen wird, muss `E` eine Instanz eines Typs mit `M` bezeichnen. Es handelt sich um einen Bindungs Zeit Fehler `E` , mit dem ein Typ bezeichnet wird.
*  Jede Instanz einer Klasse enthält einen separaten Satz aller Instanzfelder der Klasse.
*  Ein Instanzfunktionsmember (Methode, Eigenschaft, Indexer, Instanzkonstruktor oder Dekonstruktor) arbeitet auf einer bestimmten Instanz der Klasse, und auf diese Instanz kann `this` als ([dieser Zugriff](expressions.md#this-access)) zugegriffen werden.

Das folgende Beispiel veranschaulicht die Regeln für den Zugriff auf statische Member und Instanzmember:
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

Die `F`-Methode zeigt, dass in einem Instanzfunktionsmember ein *Simple_name* ([simple names](expressions.md#simple-names)) für den Zugriff auf Instanzmember und statische Member verwendet werden kann. Die `G`-Methode zeigt, dass es sich bei einem statischen Funktionsmember um einen Kompilierzeitfehler handelt, der über eine *Simple_name*auf einen Instanzmember zugreifen kann. Die `Main`-Methode zeigt, dass in einem *member_access* ([Member Access](expressions.md#member-access)) auf Instanzmember über-Instanzen zugegriffen werden muss und auf statische Member über-Typen zugegriffen werden muss.

### <a name="nested-types"></a>Geschachtelte Typen

Ein Typ, der innerhalb einer Klassen-oder Struktur Deklaration deklariert wird, wird als geschachtelter ***Typ***bezeichnet. Ein Typ, der in einer Kompilierungseinheit oder einem Namespace deklariert wird, wird als ***nicht geschachtelter Typ***bezeichnet.

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
die `B` Klasse ist ein geschachtelter Typ, da Sie innerhalb `A`der Klasse deklariert `A` wird und die Klasse ein nicht geschachtelter Typ ist, da Sie innerhalb einer Kompilierungseinheit deklariert wird.

#### <a name="fully-qualified-name"></a>Voll qualifizierter Name

Der voll qualifizierte Name ([voll](basic-concepts.md#fully-qualified-names)gekennzeichnete Namen) für einen Typ ist `S.N` , wobei `S` der voll qualifizierte Name des Typs ist, in dem der `N` Typ deklariert ist.

#### <a name="declared-accessibility"></a>Deklarierter Zugriff

Nicht--nicht--- `public` - `internal` ---typtypen können Barrierefreiheit haben oder deklarieren und sind `internal` standardmäßig als In Form von untergeordneten Typen können auch diese Formen der deklarierten Barrierefreiheit sowie eine oder mehrere zusätzliche Formen der deklarierten Barrierefreiheit enthalten sein, je nachdem, ob der enthaltende Typ eine Klasse oder Struktur ist:

*  Ein in einer Klasse deklarierter`public` `protected`, in einer Klasse deklarierter Typ kann eine beliebige von fünf Formen der deklarierten Barrierefreiheit `internal`aufweisen ( `private`, `protected internal`,, `private` oder), und wie bei anderen Klassenmembern werden standardmäßig deklarierte Barrierefreiheit.
*  Ein in einer Struktur deklarierter, in einer Struktur deklarierter Typ kann über eine von drei Formen der`public`deklarierten Barrierefreiheit `private`(, `internal`oder) verfügen, und wie bei anderen `private` Strukturmembern werden standardmäßig deklarierte Zugriffsmöglichkeiten verwendet.

Das Beispiel
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
deklariert eine private, in einer `Node`Klasse.

#### <a name="hiding"></a>Zieher

Ein-Typ kann einen Basismember ausblenden ([Name](basic-concepts.md#name-hiding)ausblenden). Der `new` -Modifizierer ist für Klassentyp Deklarationen zulässig, damit das Ausblenden explizit ausgedrückt werden kann. Das Beispiel
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
zeigt eine in der Liste `M` definierte Klasse, die `M` die in `Base`definierte Methode verbirgt.

#### <a name="this-access"></a>Dieser Zugriff

Ein-Typ und der enthaltende Typ haben keine besondere Beziehung hinsichtlich *this_access* ([dieser Zugriff](expressions.md#this-access)). `this` Insbesondere innerhalb eines geschachtelten Typs kann nicht verwendet werden, um auf Instanzmember des enthaltenden Typs zu verweisen. In Fällen, in denen ein geclusterter Typ Zugriff auf die Instanzmember seines enthaltenden Typs benötigt, kann der `this` Zugriff bereitgestellt werden, indem der für die Instanz des enthaltenden Typs als Konstruktorargument für den schsted Type bereitgestellt wird. Im folgenden Beispiel
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
zeigt diese Methode. Eine Instanz von `C` erstellt eine Instanz von `Nested` und übergibt ihren eigenen `this` an `Nested`den-Konstruktor, um den nachfolgenden Zugriff `C`auf die Instanzmember bereitzustellen.

#### <a name="access-to-private-and-protected-members-of-the-containing-type"></a>Zugriff auf private und geschützte Member des enthaltenden Typs

Ein Typ, der einen Typ aufweist, kann auf alle Member zugreifen, auf die der enthaltende Typ zugreifen kann, einschließlich der Member des `private` enthaltenden Typs, die über die Berechtigung "Barrierefreiheit" verfügen `protected` . Das Beispiel
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
zeigt eine Klasse `C` an, die eine `Nested`-Klasse enthält. In `Nested`Ruft die- `G` Methode die statische Methode `F` auf, `C`die in `F` definiert ist, und verfügt über eine private deklarierte Barrierefreiheit.

Ein-Typ kann auch auf geschützte Member zugreifen, die in einem Basistyp seines enthaltenden Typs definiert sind. Im Beispiel
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
die schsted Class `Derived.Nested` greift auf die geschützte `F` Methode zu `Derived`, die in der `Base`Basisklasse von definiert ist, indem `Derived`Sie durch eine Instanz von aufruft.

#### <a name="nested-types-in-generic-classes"></a>Typen in generischen Klassen in generischen Klassen

Eine generische Klassen Deklaration kann eine Typdeklaration für einen Typ enthalten. Die Typparameter der einschließenden Klasse können innerhalb der geschachtelten Typen verwendet werden. Eine Typdeklaration in einem Typ kann zusätzliche Typparameter enthalten, die nur für den Typ "nsted" gelten.

Jede in einer generischen Klassen Deklaration enthaltene Typdeklaration ist implizit eine generische Typdeklaration. Beim Schreiben eines Verweises auf einen Typ, der in einem generischen Typ geschachtelt ist, muss der enthaltende konstruierte Typ, einschließlich der Typargumente, benannt werden. Allerdings kann der geschachtelte Typ innerhalb der äußeren Klasse ohne Qualifizierung verwendet werden. der Instanztyp der äußeren Klasse kann beim Konstruieren des-Typs implizit verwendet werden. Im folgenden Beispiel werden drei verschiedene korrekte Möglichkeiten zum Verweisen auf einen konstruierten Typ gezeigt `Inner`, der aus erstellt wurde. die ersten beiden sind äquivalent:
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

Obwohl es sich um einen ungültigen Programmierstil handelt, kann ein Typparameter in einem Schraffurtyp einen Member oder Typparameter ausblenden, der im äußeren Typ deklariert ist:
```csharp
class Outer<T>
{
    class Inner<T>        // Valid, hides Outer's T
    {
        public T t;       // Refers to Inner's T
    }
}
```

### <a name="reserved-member-names"></a>Reservierte Elementnamen

Um die zugrunde liegende C# Lauf Zeit Implementierung zu vereinfachen, muss die Implementierung für jede Deklaration des Quell Members, bei der es sich um eine Eigenschaft, ein Ereignis oder einen Indexer handelt, zwei Methoden Signaturen reservieren, basierend auf der Art der Element Deklaration, dem Namen und dem Typ. Es ist ein Kompilierzeitfehler für ein Programm, einen Member zu deklarieren, dessen Signatur mit einer der reservierten Signaturen übereinstimmt, auch wenn die zugrunde liegende Lauf Zeit Implementierung diese Reservierungen nicht verwendet.

Die reservierten Namen führen keine Deklarationen ein, sodass Sie nicht an der Member-Suche beteiligt sind. Die zugeordneten reservierten Methoden Signaturen einer Deklaration nehmen jedoch an der Vererbung ([Vererbung](classes.md#inheritance)) Teil und können mit `new` dem Modifizierer ([dem neuen Modifizierer](classes.md#the-new-modifier)) ausgeblendet werden.

Die Reservierung dieser Namen dient drei Zwecken:

*  , Damit die zugrunde liegende Implementierung einen normalen Bezeichner als Methodennamen zum Abrufen oder Festlegen des Zugriffs auf die C# Sprachfunktion verwendet.
*  , Damit andere Sprachen mithilfe eines normalen Bezeichners als Methodenname interagieren können, um den Zugriff auf die C# Sprachfunktion zu erhalten oder festzulegen.
*  Um sicherzustellen, dass die von einem konformen Compiler akzeptierte Quelle von einer anderen akzeptiert wird, indem die Besonderheiten von reservierten Elementnamen für C# alle Implementierungen konsistent gemacht werden.

Die Deklaration eines destrukturtors[(](classes.md#destructors)destrukturatoren) bewirkt auch, dass eine Signatur reserviert wird ([für destrukturtoren reservierte Elementnamen](classes.md#member-names-reserved-for-destructors)).

#### <a name="member-names-reserved-for-properties"></a>Für Eigenschaften reservierte Elementnamen

Für eine Eigenschaft `P` ([Eigenschaften](classes.md#properties)) vom Typ `T`sind die folgenden Signaturen reserviert:

```csharp
T get_P();
void set_P(T value);
```

Beide Signaturen sind reserviert, auch wenn die Eigenschaft schreibgeschützt oder schreibgeschützt ist.

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
eine Klasse `A` definiert eine schreibgeschützte Eigenschaft `P`und reserviert somit Signaturen für `get_P` -und- `set_P` Methoden. Eine- `B` Klasse wird `A` von abgeleitet und verbirgt beide reservierten Signaturen. Das Beispiel erzeugt die Ausgabe:
```console
123
123
456
```

#### <a name="member-names-reserved-for-events"></a>Für Ereignisse reservierte Elementnamen

Für ein Ereignis `E` ([Ereignisse](classes.md#events)) des Delegattyps `T`sind die folgenden Signaturen reserviert:
```csharp
void add_E(T handler);
void remove_E(T handler);
```

#### <a name="member-names-reserved-for-indexers"></a>Für Indexer reservierte Elementnamen

Für einen Indexer ([Indexer](classes.md#indexers)) vom Typ `T` mit Parameter-List `L`sind die folgenden Signaturen reserviert:
```csharp
T get_Item(L);
void set_Item(L, T value);
```

Beide Signaturen sind reserviert, auch wenn der Indexer schreibgeschützt oder schreibgeschützt ist.

Außerdem ist der Element `Item` Name reserviert.

#### <a name="member-names-reserved-for-destructors"></a>Für destrukturtoren reservierte Elementnamen

Für eine Klasse, die einen Dekonstruktor ([Dekonstruktoren](classes.md#destructors)) enthält, ist die folgende Signatur reserviert:
```csharp
void Finalize();
```

## <a name="constants"></a>Konstanten

Eine ***Konstante*** ist ein Klassenmember, der einen konstanten Wert darstellt: einen Wert, der zur Kompilierzeit berechnet werden kann. Ein *constant_declaration* führt eine oder mehrere Konstanten eines bestimmten Typs ein.

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

Ein *constant_declaration* kann einen Satz von *Attributen* ([Attribute](attributes.md)), einen `new`-Modifizierer ([den neuen Modifizierer](classes.md#the-new-modifier)) und eine gültige Kombination der vier Zugriffsmodifizierer ([Zugriffsmodifizierer](classes.md#access-modifiers)) enthalten. Die Attribute und Modifizierer gelten für alle Member, die von *constant_declaration*deklariert werden. Obwohl Konstanten als statische Member angesehen werden, ist ein *constant_declaration* weder erforderlich noch ein `static`-Modifizierer zulässig. Es ist ein Fehler, dass derselbe Modifizierer mehrmals in einer Konstanten Deklaration angezeigt wird.

Der *Typ* eines *constant_declaration* gibt den Typ der Member an, die von der Deklaration eingeführt wurden. Auf den Typ folgt eine Liste von *constant_declarator*s, von denen jeder einen neuen Member einführt. Ein *constant_declarator* besteht aus einem *Bezeichner* , der den Member benennt, gefolgt von einem "`=`"-Token, gefolgt von einem *constant_expression* ([Konstantenausdrücken](expressions.md#constant-expressions)), das den Wert des Members liefert.

Der in einer Konstanten Deklaration angegebene *Typ* muss `sbyte`, `byte`, `short`, `ushort`, `int`, `uint`, `long`, `ulong`, `char`, 0, 1, 2, 3, 4, ein *enum_type*oder ein *reference_ Geben*Sie ein. Jede *constant_expression* muss einen Wert des Zieltyps oder eines Typs liefern, der durch eine implizite Konvertierung ([implizite Konvertierungen](conversions.md#implicit-conversions)) in den Zieltyp konvertiert werden kann.

Der *Typ* einer Konstanten muss mindestens so zugänglich sein, wie die Konstante selbst (Barrierefreiheits[Einschränkungen](basic-concepts.md#accessibility-constraints)).

Der Wert einer Konstanten wird in einem Ausdruck mithilfe eines *Simple_name* ([simple names](expressions.md#simple-names)) oder eines *member_access* ([Member Access](expressions.md#member-access)) abgerufen.

Eine Konstante kann selbst an einem *constant_expression*teilnehmen. Daher kann eine Konstante in jedem Konstrukt verwendet werden, das ein *constant_expression*erfordert. Beispiele für derartige Konstrukte `case` sind Bezeichnungen `goto case` , Anweisungen `enum` , Element Deklarationen, Attribute und andere Konstante Deklarationen.

Wie in [konstanten Ausdrücken](expressions.md#constant-expressions)beschrieben, ist ein *constant_expression* ein Ausdruck, der zur Kompilierzeit vollständig ausgewertet werden kann. Da die einzige Möglichkeit zum Erstellen eines nicht-NULL-Werts eines anderen *reference_type* als `string` ist, den `new`-Operator anzuwenden, und da der `new`-Operator in einem *constant_expression*-Operator nicht zulässig ist, ist der einzige mögliche Wert für Konstanten von *. reference_type*s (außer `string`) ist `null`.

Wenn ein symbolischer Name für einen konstanten Wert gewünscht ist, aber wenn der Typ dieses Werts in einer Konstanten Deklaration nicht zulässig ist oder wenn der Wert nicht zur Kompilierzeit von einem *constant_expression*berechnet werden kann, kann ein `readonly`-Feld (schreibgeschützte[Felder](classes.md#readonly-fields)) Verwenden Sie stattdessen.

Eine Konstante Deklaration, die mehrere Konstanten deklariert, entspricht mehreren Deklarationen von einzelnen Konstanten mit denselben Attributen, modifizierertypen und Typen. Beispiel:
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

Konstanten dürfen von anderen Konstanten innerhalb desselben Programms abhängen, solange die Abhängigkeiten nicht aus Zirkel Natur bestehen. Der Compiler ordnet automatisch an, die Konstanten Deklarationen in der entsprechenden Reihenfolge auszuwerten. Im Beispiel
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
der Compiler `A.Y`wertet zunächst die Werte `B.Z` `A.X` `10`, `11`und ausundwertetSieanschließendaus.`12` Konstante Deklarationen können von Konstanten anderer Programme abhängen, aber solche Abhängigkeiten sind nur in einer Richtung möglich. `A` Wenn und `B.Z` `A.X` `A.Y` `B.Z` in separaten Programmen in Bezug auf das obige Beispiel deklariert wurden, kann es möglich sein, von abhängig zu sein, aber es kann nicht gleichzeitig von abhängen. `B`

## <a name="fields"></a>Felder

Ein ***Feld*** ist ein Member, der eine Variable darstellt, die einem Objekt oder einer Klasse zugeordnet ist. Ein *field_declaration* führt ein oder mehrere Felder eines bestimmten Typs ein.

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

Ein *field_declaration* kann einen Satz von *Attributen* ([Attribute](attributes.md)), einen `new`-Modifizierer ([den neuen Modifizierer](classes.md#the-new-modifier)), eine gültige Kombination der vier Zugriffsmodifizierer ([Zugriffsmodifizierer](classes.md#access-modifiers)) und einen `static`-Modifizierer ([ Statische Felder und Instanzfelder](classes.md#static-and-instance-fields)). Außerdem kann ein *field_declaration* einen `readonly`-Modifizierer (schreibgeschützte[Felder](classes.md#readonly-fields)) oder einen `volatile`-Modifizierer ([flüchtige Felder](classes.md#volatile-fields)) enthalten, aber nicht beides. Die Attribute und Modifizierer gelten für alle Member, die von *field_declaration*deklariert werden. Es ist ein Fehler, dass derselbe Modifizierer mehrmals in einer Feld Deklaration angezeigt wird.

Der *Typ* eines *field_declaration* gibt den Typ der Member an, die von der Deklaration eingeführt wurden. Auf den Typ folgt eine Liste von *variable_declarator*s, von denen jeder einen neuen Member einführt. Ein *variable_declarator* besteht aus einem *Bezeichner* , der diesen Member benennt, optional gefolgt von einem "`=`"-Token und einem *variable_initializer* ([Variableninitialisierer](classes.md#variable-initializers)), der den Anfangswert dieses Members enthält.

Der *Typ* eines Felds muss mindestens so zugänglich sein wie das Feld selbst (Barrierefreiheits[Einschränkungen](basic-concepts.md#accessibility-constraints)).

Der Wert eines Felds wird in einem Ausdruck mithilfe eines *Simple_name* ([simple names](expressions.md#simple-names)) oder eines *member_access* ([Member Access](expressions.md#member-access)) abgerufen. Der Wert eines nicht schreibgeschützten Felds wird mithilfe einer *Zuweisung* ([Zuweisungs Operatoren](expressions.md#assignment-operators)) geändert. Der Wert eines nicht schreibgeschützten Felds kann mit Postfix-Inkrement-und Dekrementoperatoren ([postfix-Inkrement-und Dekrementoperatoren](expressions.md#postfix-increment-and-decrement-operators)) und Präfix Inkrement-und Dekrementoperatoren ([Präfix Inkrement und Dekrement) abgerufen und geändert werden. Operatoren](expressions.md#prefix-increment-and-decrement-operators)).

Eine Feld Deklaration, die mehrere Felder deklariert, entspricht mehreren Deklarationen von einzelnen Feldern mit denselben Attributen, modifizierertypen und Typen. Beispiel:
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

### <a name="static-and-instance-fields"></a>Statische Felder und Instanzfelder

Wenn eine Feld Deklaration einen `static` Modifizierer enthält, sind die von der Deklaration eingeführten Felder ***statische Felder***. Wenn kein `static` Modifizierer vorhanden ist, sind die von der Deklaration eingeführten Felder ***Instanzfelder***. Statische Felder und Instanzfelder sind zwei der verschiedenen Arten von Variablen ([Variablen](variables.md)), die C#von unterstützt werden. manchmal werden Sie als ***statische Variablen*** bzw. ***Instanzvariablen***bezeichnet.

Ein statisches Feld ist nicht Teil einer bestimmten Instanz. Stattdessen wird Sie von allen Instanzen eines geschlossenen Typs ([offene und geschlossene Typen](types.md#open-and-closed-types)) gemeinsam verwendet. Unabhängig davon, wie viele Instanzen eines geschlossenen Klassen Typs erstellt werden, gibt es nur eine Kopie eines statischen Felds für die zugehörige Anwendungsdomäne.

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

Ein Instanzfeld gehört zu einer Instanz. Insbesondere enthält jede Instanz einer Klasse einen separaten Satz aller Instanzfelder dieser Klasse.

Wenn auf ein Feld in einem *member_access* -Element ([Member Access](expressions.md#member-access)) der Form `E.M` verwiesen wird, wenn `M` ein statisches Feld ist, muss `E` einen Typ mit `M` angeben. wenn `M` ein Instanzfeld ist, muss E eine Instanz eines Typs angeben, der enthält. `M`.

Die Unterschiede zwischen statischen und Instanzmembern werden in [statischen und Instanzmembern](classes.md#static-and-instance-members)ausführlicher erläutert.

### <a name="readonly-fields"></a>Schreibgeschützte Felder

Wenn ein *field_declaration* einen `readonly`-Modifizierer enthält, sind die von der Deklaration eingeführten Felder schreibgeschützte ***Felder***. Direkte Zuweisungen zu schreibgeschützten Feldern können nur als Teil der Deklaration oder in einem Instanzkonstruktor oder statischen Konstruktor in derselben Klasse erfolgen. (Ein Schreib geschütztes Feld kann in diesen Kontexten mehrmals zugewiesen werden.) Direkte Zuweisungen zu einem `readonly` Feld sind nur in den folgenden Kontexten zulässig:

*  In der *variable_declarator* , die das Feld einführt (durch Einschließen eines *variable_initializer* in die Deklaration).
*  Für ein Instanzfeld in den Instanzkonstruktoren der-Klasse, die die Feld Deklaration enthält; für ein statisches Feld im statischen Konstruktor der Klasse, die die Feld Deklaration enthält. Dies sind auch die einzigen Kontexte, in denen es zulässig ist, ein `readonly` Feld `out` als-oder `ref` -Parameter zu übergeben.

Der Versuch, einem `readonly` Feld zuzuweisen oder es `out` als-oder `ref` -Parameter in einem anderen Kontext zu übergeben, ist ein Kompilierzeitfehler.

#### <a name="using-static-readonly-fields-for-constants"></a>Verwenden statischer Schreib geschützter Felder für Konstanten

Ein `static readonly` Feld ist nützlich, wenn ein symbolischer Name für einen konstanten Wert erwünscht ist, aber wenn der Typ des Werts nicht in einer `const` Deklaration zulässig ist oder wenn der Wert nicht zur Kompilierzeit berechnet werden kann. Im Beispiel
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
die `Black`Member `White`,, `Red`, undkönnen`Blue` nicht als`const` Member deklariert werden, da ihre Werte zur Kompilierzeit nicht berechnet werden können. `Green` Das deklarieren `static readonly` der Dateien hat jedoch die gleiche Wirkung.

#### <a name="versioning-of-constants-and-static-readonly-fields"></a>Versionierung von Konstanten und statischen schreibgeschützten Feldern

Konstanten und schreibgeschützte Felder haben unterschiedliche Semantik für die binäre Versionsverwaltung. Wenn ein Ausdruck auf eine Konstante verweist, wird der Wert der Konstante zur Kompilierzeit abgerufen. Wenn jedoch ein Ausdruck auf ein Schreib geschütztes Feld verweist, wird der Wert des Felds bis zur Laufzeit nicht abgerufen. Stellen Sie sich eine Anwendung vor, die aus zwei separaten Programmen besteht:
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

Die `Program1` Namespaces und `Program2` bezeichnen zwei Programme, die separat kompiliert werden. Da `Program1.Utils.X` als statisches Schreib geschütztes Feld deklariert ist, ist der Wert, der `Console.WriteLine` von der-Anweisung ausgegeben wird, zur Kompilierzeit nicht bekannt, sondern wird zur Laufzeit abgerufen. Wenn also `X` der Wert von geändert und `Program1` neu kompiliert wird, gibt die `Console.WriteLine` Anweisung den neuen Wert auch dann aus, wenn `Program2` nicht neu kompiliert wird. `Program2` `X` `Program2` `Program1` War jedoch eine Konstante. der Wert von wurde zum Zeitpunkt der Kompilierung abgerufen und bleibt von Änderungen in bis zur erneuten Kompilierung nicht beeinträchtigt. `X`

### <a name="volatile-fields"></a>Flüchtige Felder

Wenn ein *field_declaration* einen `volatile`-Modifizierer enthält, sind die von dieser Deklaration eingeführten Felder ***flüchtige Felder***.

Bei nicht flüchtigen Feldern können Optimierungstechniken, die Anweisungen neu anordnen, zu unerwarteten und unvorhersehbaren Ergebnissen in Multithreadprogrammen führen, die auf Felder ohne Synchronisierung zugreifen, wie z. b. die von *lock_statement* bereitgestellte ([die Lock-Anweisung](statements.md#the-lock-statement)). Diese Optimierungen können vom Compiler, vom Laufzeitsystem oder von der Hardware ausgeführt werden. Für flüchtige Felder sind solche neubestellungs Optimierungen eingeschränkt:

*  Ein Lesevorgang eines flüchtigen Felds wird als ***flüchtiger Lese***Vorgang bezeichnet. Ein flüchtiger Lesevorgang hat "Semantik abrufen". Das heißt, es wird sichergestellt, dass es vor allen in der Anweisungs Sequenz auftretenden verweisen auf den Speicher auftritt.
*  Ein Schreibvorgang eines flüchtigen Felds wird als ***flüchtiges schreiben***bezeichnet. Ein flüchtiger Schreibvorgang weist die Freigabe Semantik auf. Das heißt, es wird sichergestellt, dass es nach allen Speicher verweisen vor der Schreib Anweisung in der Anweisungs Sequenz erfolgt.

Diese Einschränkungen stellen sicher, dass alle Threads volatile-Schreibvorgänge, die von einem anderen Thread ausgeführt werden, in der Reihenfolge ihrer Ausführung berücksichtigen. Eine konforme Implementierung ist nicht erforderlich, um eine einzelne Gesamt Reihenfolge von flüchtigen Schreibvorgängen zu gewährleisten, die von allen Ausführungs Threads erkannt werden. Der Typ eines flüchtigen Felds muss einer der folgenden sein:

*  Ein *reference_type*.
*  Der Typ `byte`, `sbyte`, `short`, ,`ushort`, ,`float`,,, oder`System.IntPtr`. `char` `uint` `int` `bool` `System.UIntPtr`
*  Ein *enum_type* mit dem Aufzählungs Basistyp `byte`, `sbyte`, `short`, `ushort`, `int` oder `uint`.

Das Beispiel
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
```console
result = 143
```

In diesem Beispiel startet die- `Main` Methode einen neuen Thread, der die- `Thread2`Methode ausführt. Diese Methode speichert einen Wert in ein nicht flüchtiges Feld mit `result`dem Namen und `true` speichert dann im flüchtigen `finished`Feld. Der Haupt Thread wartet darauf, dass `finished` das Feld auf `true`festgelegt wird, und liest `result`dann das Feld. Da `finished` `143` `result`deklariert `volatile`wurde, muss der Haupt Thread den Wert aus dem Feld lesen. Wenn das Feld `finished` nicht deklariert `volatile`wurde, ist es `result` zulässig, dass der Speicher für den Haupt Thread nach dem Speichern in `finished`sichtbar ist, sodass der Haupt Thread den Wert `0` aus dem ein `result`. Das `finished` deklarieren `volatile` als Feld verhindert jegliche Inkonsistenz.

### <a name="field-initialization"></a>Feld Initialisierung

Der Anfangswert eines Felds, unabhängig davon, ob es sich um ein statisches Feld oder ein Instanzfeld handelt, ist der Standardwert ([Standardwerte](variables.md#default-values)) des Feldtyps. Es ist nicht möglich, den Wert eines Felds zu beobachten, bevor diese Standard Initialisierung erfolgt ist, und ein Feld wird daher nie "nicht initialisiert". Das Beispiel
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
```console
b = False, i = 0
```
, `b` da `i` und automatisch mit den Standardwerten initialisiert werden.

### <a name="variable-initializers"></a>Variableninitialisierer

Feld Deklarationen können *variable_initializer*s enthalten. Bei statischen Feldern entsprechen Variableninitialisierern Zuweisungs Anweisungen, die während der Initialisierung der Klasse ausgeführt werden. Bei Instanzfeldern entsprechen Variableninitialisierern Zuweisungs Anweisungen, die ausgeführt werden, wenn eine Instanz der Klasse erstellt wird.

Das Beispiel
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
```console
x = 1.4142135623731, i = 100, s = Hello
```
eine Zuweisung zu erfolgt `x` , wenn statische Feldinitialisierer ausgeführt werden `i` und `s` Zuweisungen zu und auftreten, wenn die instanzfeldinitialisierer ausgeführt werden.

Die in der [Feld Initialisierung](classes.md#field-initialization) beschriebene Standardwert Initialisierung tritt für alle Felder auf, einschließlich der Felder, die über Variableninitialisierer verfügen. Wenn eine Klasse initialisiert wird, werden daher alle statischen Felder in dieser Klasse zuerst mit ihren Standardwerten initialisiert, und anschließend werden die statischen Feldinitialisierer in der Textfolge ausgeführt. Wenn eine Instanz einer Klasse erstellt wird, werden alle Instanzfelder in dieser Instanz zuerst mit ihren Standardwerten initialisiert, und anschließend werden die instanzfeldinitialisierer in der Textfolge ausgeführt.

Es ist möglich, dass statische Felder mit Variableninitialisierern in ihrem Standardwert Zustand beobachtet werden. Dies wird jedoch dringend von einem Stil abgeraten. Das Beispiel
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
weist dieses Verhalten auf. Trotz der zirkulären Definitionen von a und b ist das Programm gültig. Dadurch wird die Ausgabe ausgegeben.
```console
a = 1, b = 2
```
Da die statischen Felder `a` und `b` auf (den Standard `0` Wert für `int`) initialisiert werden, bevor Ihre Initialisierer ausgeführt werden. Wenn der Initialisierer für `a` ausgeführt wird, ist der `b` Wert von 0 (NULL `a` ) und wird daher `1`mit initialisiert. Wenn der Initialisierer für `b` ausgeführt wird, ist der `a` Wert von `1`bereits, und `b` wird daher mit `2`initialisiert.

#### <a name="static-field-initialization"></a>Initialisierung statischer Felder

Die statischen feldvariableninitialisierer einer Klasse entsprechen einer Sequenz von Zuweisungen, die in der Text Reihenfolge ausgeführt werden, in der Sie in der Klassen Deklaration angezeigt werden. Wenn ein statischer Konstruktor ([statischer Konstruktor](classes.md#static-constructors)) in der Klasse vorhanden ist, erfolgt die Ausführung der statischen Feldinitialisierer unmittelbar vor der Ausführung dieses statischen Konstruktors. Andernfalls werden die statischen Feldinitialisierer zu einem Implementierungs abhängigen Zeitpunkt vor der ersten Verwendung eines statischen Felds dieser Klasse ausgeführt. Das Beispiel
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
erzeugt möglicherweise die folgende Ausgabe:
```console
Init A
Init B
1 1
```
oder die Ausgabe:
```console
Init B
Init A
1 1
```
Da die Ausführung des `X`Initialisierers und `Y`des Initialisierers in einer der beiden Reihenfolge auftreten kann, sind Sie nur so eingeschränkt, dass Sie vor den verweisen auf diese Felder auftreten. Im Beispiel:
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
die Ausgabe muss wie folgt lauten:
```console
Init B
Init A
1 1
```
Da die Regeln für den Fall, dass statische Konstruktoren ausgeführt werden (wie in [statischen Konstruktoren](classes.md#static-constructors)definiert), den statischen Konstruktor `B`(und somit die statischen Feldinitialisierer) bereitstellen, `B`müssen vor `A`der statischen ausgeführt werden. Konstruktor und Feldinitialisierer.

#### <a name="instance-field-initialization"></a>Instanzfeldinitialisierung

Die instanzfeldvariableninitialisierer einer Klasse entsprechen einer Sequenz von Zuweisungen, die unmittelbar nach dem Eintrag für einen der Instanzkonstruktoren ([Konstruktorinitialisierer](classes.md#constructor-initializers)) dieser Klasse ausgeführt werden. Die Variableninitialisierer werden in der Text Reihenfolge ausgeführt, in der Sie in der Klassen Deklaration angezeigt werden. Die Erstellung und Initialisierung der Klasseninstanz wird in [Instanzkonstruktoren](classes.md#instance-constructors)ausführlicher beschrieben.

Ein Variableninitialisierer für ein Instanzfeld kann nicht auf die erstellte Instanz verweisen. Daher ist es ein Kompilierzeitfehler, auf `this` in einem Variableninitialisierer zu verweisen, da es sich um einen Kompilierzeitfehler für einen Variableninitialisierer handelt, der auf ein beliebiges Instanzmember über eine *Simple_name*verweist. Im Beispiel
```csharp
class A
{
    int x = 1;
    int y = x + 1;        // Error, reference to instance member of this
}
```
der Variableninitialisierer `y` für führt zu einem Kompilierzeitfehler, da er auf einen Member der Instanz verweist, die erstellt wird.

## <a name="methods"></a>Methoden

Eine ***Methode*** ist ein Member, das eine Berechnung oder eine Aktion implementiert, die durch ein Objekt oder eine Klasse durchgeführt werden kann. Methoden werden mit *method_declaration*s deklariert:

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

Ein *method_declaration* kann einen Satz von *Attributen* ([Attribute](attributes.md)) und eine gültige Kombination der vier Zugriffsmodifizierer ([Zugriffsmodifizierer](classes.md#access-modifiers)), den `new` ([den neuen Modifizierer](classes.md#the-new-modifier)), `static` ([statisch und Instanz) enthalten. Methoden](classes.md#static-and-instance-methods)), `virtual` ([virtuelle Methoden](classes.md#virtual-methods)), `override` ([Überschreibungs Methoden](classes.md#override-methods)), `sealed` ([versiegelte Methoden](classes.md#sealed-methods)), `abstract` ([abstrakte Methoden](classes.md#abstract-methods)) und `extern`-Modifizierer ([Externe Methoden](classes.md#external-methods)).

Eine-Deklaration hat eine gültige Kombination von Modifizierer, wenn Folgendes zutrifft:

*  Die-Deklaration enthält eine gültige Kombination von [zugriffsmodifizierermodifizierer](classes.md#access-modifiers)
*  Die-Deklaration enthält nicht denselben Modifizierer mehrmals.
*  Die-Deklaration enthält höchstens einen der folgenden Modifizierer `static`: `virtual`, und `override`.
*  Die-Deklaration enthält höchstens einen der folgenden Modifizierer `new` : `override`und.
*  Wenn die Deklaration den `abstract` -Modifizierer enthält, enthält die Deklaration keinen der folgenden Modifizierer `static`: `virtual`, `sealed` oder `extern`.
*  Wenn die Deklaration den `private` -Modifizierer enthält, enthält die Deklaration keinen der folgenden Modifizierer `virtual`: `override`, oder `abstract`.
*  Wenn die Deklaration den `sealed` -Modifizierer enthält, enthält die Deklaration auch den `override` -Modifizierer.
*  Wenn die Deklaration den `partial` -Modifizierer enthält, enthält Sie keinen der folgenden Modifizierer: `new`, `public`, `protected`, `internal`, `private`, `virtual`, `sealed`, `override` , `abstract`oder .`extern`

Eine Methode, die über `async` den-Modifizierer verfügt, ist eine Async-Funktion und befolgt die in [Async-Funktionen](classes.md#async-functions)beschriebenen Regeln.

Der *return_type* einer Methoden Deklaration gibt den Typ des Werts an, der von der-Methode berechnet und zurückgegeben wird. Der *return_type* -Wert ist `void`, wenn die Methode keinen Wert zurückgibt. Wenn die Deklaration den `partial` -Modifizierer enthält, muss der Rückgabetyp sein. `void`

*MEMBER_NAME* gibt den Namen der Methode an. Wenn die Methode keine explizite Schnittstellenmember-Implementierung ist ([explizite Schnittstellenmember-Implementierungen](interfaces.md#explicit-interface-member-implementations)), ist *MEMBER_NAME* einfach ein *Bezeichner*. Bei einer expliziten Schnittstellenmember-Implementierung besteht das *MEMBER_NAME* aus einem *INTERFACE_TYPE* , gefolgt von einem "`.`" und einem *Bezeichner*.

Der optionale *type_parameter_list* -Parameter gibt die Typparameter der Methode ([Typparameter](classes.md#type-parameters)) an. Wenn *type_parameter_list* angegeben wird, handelt es sich bei der Methode um eine ***generische Methode***. Wenn die Methode einen `extern`-Modifizierer aufweist, kann kein *type_parameter_list* angegeben werden.

Mit dem optionalen *formal_parameter_list* werden die Parameter der Methode ([Methoden Parameter](classes.md#method-parameters)) angegeben.

Die optionalen *type_parameter_constraints_clause*s geben Einschränkungen für einzelne Typparameter an ([Typparameter Einschränkungen](classes.md#type-parameter-constraints)) und können nur angegeben werden, wenn eine *type_parameter_list* ebenfalls bereitgestellt wird, und die Methode hat keine `override`-Modifizierer.

Die *return_type* und alle Typen, auf die im *formal_parameter_list* einer Methode verwiesen wird, müssen mindestens so zugänglich sein wie die Methode selbst ([Barrierefreiheits Einschränkungen](basic-concepts.md#accessibility-constraints)).

Der *method_body* ist entweder ein Semikolon, ein ***Anweisungs Text*** oder ein ***Ausdrucks Körper***. Ein Anweisungs Text besteht aus einem- *Block*, der die auszuführenden Anweisungen angibt, wenn die-Methode aufgerufen wird. Ein Ausdrucks Körper besteht aus `=>` , gefolgt von einem *Ausdruck* und einem Semikolon und deutet auf einen einzelnen Ausdruck hin, der beim Aufrufen der Methode ausgeführt werden soll. 

Bei `abstract`-und `extern`-Methode besteht das *method_body* einfach aus einem Semikolon. Bei `partial`-Methoden kann der *method_body* entweder aus einem Semikolon, einem Block Text oder einem Ausdrucks Körper bestehen. Für alle anderen Methoden ist *method_body* entweder ein Block Körper oder ein Ausdrucks Text.

Wenn *method_body* aus einem Semikolon besteht, enthält die Deklaration möglicherweise nicht den `async`-Modifizierer.

Der Name, die Typparameter Liste und die Liste der formalen Parameter einer Methode definieren die Signatur ([Signaturen und überladen](basic-concepts.md#signatures-and-overloading)) der Methode. Insbesondere besteht die Signatur einer Methode aus dem Namen, der Anzahl der Typparameter und der Anzahl, den modifizierertypen und den Typen der formalen Parameter. Zu diesem Zweck wird jeder Typparameter der Methode, die im Typ eines formalen Parameters vorkommt, nicht anhand seines Namens identifiziert, sondern anhand seiner Ordinalposition in der Typargument Liste der Methode. Der Rückgabetyp ist weder Teil der Signatur einer Methode noch die Namen der Typparameter oder der formalen Parameter.

Der Name einer Methode muss sich von den Namen aller anderen nicht-Methoden unterscheiden, die in derselben Klasse deklariert sind. Außerdem muss sich die Signatur einer Methode von den Signaturen aller anderen Methoden unterscheiden, die in derselben Klasse deklariert sind, und zwei Methoden, die in derselben Klasse deklariert werden, dürfen keine Signaturen aufweisen, `ref` die `out`sich ausschließlich durch und unterscheiden.

Die *type_parameter*s der Methode befinden sich im Gültigkeitsbereich des *method_declaration*und können zum bilden von Typen in diesem Bereich in *return_type*, *method_body*und *type_parameter_constraints_clause*s verwendet werden, aber nicht in *Attribute*.

Alle formalen Parameter und Typparameter müssen unterschiedliche Namen aufweisen.

### <a name="method-parameters"></a>Methoden Parameter

Die Parameter einer Methode, sofern vorhanden, werden vom *formal_parameter_list*der Methode deklariert.

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

Die Liste der formalen Parameter besteht aus mindestens einem durch Trennzeichen getrennten Parameter, bei dem nur der letzte ein *parameter_array*sein kann.

Ein *fixed_parameter* besteht aus einem optionalen Satz von *Attributen* ([Attributen](attributes.md)), einem optionalen `ref`-, `out`-oder `this`-Modifizierer, einem *Typ*, einem *Bezeichner* und einem optionalen *default_argument*. Jede *fixed_parameter* deklariert einen Parameter des angegebenen Typs mit dem angegebenen Namen. Der `this` -Modifizierer legt die-Methode als Erweiterungsmethode fest und ist nur für den ersten Parameter einer statischen Methode zulässig. Erweiterungs Methoden werden weiter unten in den [Erweiterungs Methoden](classes.md#extension-methods)beschrieben.

Ein *fixed_parameter* mit einem *default_argument* -Wert wird als ***optionaler Parameter***bezeichnet, wohingegen ein *fixed_parameter* ohne *default_argument* ein ***erforderlicher Parameter***ist. Ein erforderlicher Parameter darf nicht nach einem optionalen Parameter in einer *formal_parameter_list*angezeigt werden.

Ein `ref`-oder `out`-Parameter darf keinen *default_argument*aufweisen. Der *Ausdruck* in einem *default_argument* muss einer der folgenden sein:

*  eine *constant_expression*
*  ein Ausdruck der Form `new S()` , in `S` der ein Werttyp ist.
*  ein Ausdruck der Form `default(S)` , in `S` der ein Werttyp ist.

Der *Ausdruck* muss implizit durch eine Identität oder eine Konvertierung in den Typ des Parameters konvertiert werden können.

Wenn optionale Parameter in einer implementierenden partiellen Methoden Deklaration ([partielle Methoden](classes.md#partial-methods)) auftreten, eine explizite Schnittstellenmember-Implementierung ([explizite Schnittstellenmember-Implementierungen](interfaces.md#explicit-interface-member-implementations)) oder in einer Indexer-Deklaration mit einem einzelnen Parameter ([ Indexer](classes.md#indexers)) der Compiler sollte eine Warnung angeben, da diese Member niemals auf eine Weise aufgerufen werden können, die das Weglassen von Argumenten zulässt.

Ein *parameter_array* besteht aus einem optionalen Satz von *Attributen* ([Attributen](attributes.md)), einem `params`-Modifizierer, einem *array_type*und einem *Bezeichner*. Ein Parameter Array deklariert einen einzelnen Parameter des angegebenen Array Typs mit dem angegebenen Namen. Der *array_type* eines Parameter Arrays muss ein eindimensionaler Arraytyp ([Array Typen](arrays.md#array-types)) sein. Bei einem Methodenaufruf ermöglicht ein Parameter Array, dass entweder ein einzelnes Argument des angegebenen Array Typs angegeben wird, oder es lässt NULL oder mehr Argumente des Array Elementtyps angegeben werden. Parameter Arrays werden in [Parameter Arrays](classes.md#parameter-arrays)ausführlicher beschrieben.

Ein *parameter_array* kann nach einem optionalen Parameter auftreten, kann aber keinen Standardwert aufweisen. das Weglassen von Argumenten für eine *parameter_array* würde stattdessen zur Erstellung eines leeren Arrays führen.

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

In *formal_parameter_list* für `M` ist `i` ein erforderlicher ref-Parameter. `d` ist ein erforderlicher Wert Parameter, `b`, `s`, `o` und `t` sind optionale Wert Parameter und `a` ein Parameter Array.

Eine Methoden Deklaration erstellt einen separaten Deklarations Bereich für Parameter, Typparameter und lokale Variablen. Namen werden in diesem Deklarations Bereich durch die Typparameter Liste und die Liste formaler Parameter der-Methode und durch lokale Variablen Deklarationen im- *Block* der-Methode eingeführt. Es ist ein Fehler, wenn zwei Member eines Methoden Deklarations Raums denselben Namen haben. Es ist ein Fehler für den Deklarations Bereich der Methode und der Deklarations Bereich der lokalen Variablen eines in der Tabelle enthaltenen Deklarations Raums, der Elemente mit dem gleichen Namen enthalten soll.

Ein Methodenaufruf ([Methodenaufrufe](expressions.md#method-invocations)) erstellt eine Kopie, die für diesen Aufruf spezifisch ist, der formalen Parameter und lokalen Variablen der Methode, und die Argumentliste des Aufrufs weist Werte oder Variablen Verweise auf den neu erstellten formalen Metern. Innerhalb des- *Blocks* einer Methode kann von ihren bezeichtern in *Simple_name* -Ausdrücken ([einfachen Namen](expressions.md#simple-names)) auf formale Parameter verwiesen werden.

Es gibt vier Arten von formalen Parametern:

*  Wert Parameter, die ohne modifiziererer deklariert werden.
*  Verweis Parameter, die mit dem `ref` -Modifizierer deklariert werden.
*  Ausgabeparameter, die mit dem `out` -Modifizierer deklariert werden.
*  Parameter Arrays, die mit dem `params` -Modifizierer deklariert werden.

Wie in [Signaturen und überladen](basic-concepts.md#signatures-and-overloading)beschrieben, sind `ref` die `out` Modifizierer und Teil der Signatur einer Methode, der `params` -Modifizierer ist jedoch nicht.

#### <a name="value-parameters"></a>Wert Parameter

Ein Parameter, der ohne Modifizierer deklariert wird, ist ein value-Parameter. Ein value-Parameter entspricht einer lokalen Variablen, die den Anfangswert aus dem entsprechenden Argument abruft, das im Methodenaufruf angegeben wird.

Wenn ein formaler Parameter ein value-Parameter ist, muss das entsprechende Argument in einem Methodenaufruf ein Ausdruck sein, der implizit konvertierbar ist ([implizite Konvertierungen](conversions.md#implicit-conversions)), in den formalen Parametertyp.

Eine Methode darf einem Wert Parameter neue Werte zuweisen. Solche Zuweisungen wirken sich nur auf den lokalen Speicherort aus, der durch den value-Parameter dargestellt wird – Sie haben keine Auswirkung auf das tatsächliche Argument, das im Methodenaufruf angegeben wurde.

#### <a name="reference-parameters"></a>Verweisparameter

Ein Parameter, der mit `ref` einem-Modifizierer deklariert wird, ist ein Verweis Parameter. Anders als bei einem value-Parameter erstellt ein Verweis Parameter keinen neuen Speicherort. Stattdessen stellt ein Verweis Parameter denselben Speicherort wie die Variable dar, die im Methodenaufruf als Argument angegeben wurde.

Wenn ein formaler Parameter ein Verweis Parameter ist, muss das entsprechende Argument in einem Methodenaufruf aus dem Schlüsselwort `ref` gefolgt von einem *variable_reference* ([exakte Regeln zum Bestimmen der eindeutigen Zuweisung](variables.md#precise-rules-for-determining-definite-assignment)) desselben Typs bestehen wie der formale Parameter. Eine Variable muss definitiv zugewiesen werden, bevor Sie als Verweis Parameter übergeben werden kann.

Innerhalb einer Methode wird ein Verweis Parameter immer als definitiv zugewiesen betrachtet.

Eine Methode, die als Iterator ([Iteratoren](classes.md#iterators)) deklariert ist, darf keine Verweis Parameter aufweisen.

Das Beispiel
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
```console
i = 2, j = 1
```

Für den `Swap` Aufruf von `Main`in stellt `i`und dar`y` .`j` `x` Folglich hat der Aufruf die Auswirkungen, die Werte von `i` und `j`auszutauschen.

In einer Methode, die Verweis Parameter annimmt, ist es möglich, dass mehrere Namen denselben Speicherort darstellen. Im Beispiel
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
der `F` Aufruf von `b`in `G` übergibt einen Verweis auf `s` sowohl `a` für als auch für. Daher beziehen sich bei `s`diesem Aufruf die Namen `a`, und `b` alle auf denselben Speicherort, und die drei Zuweisungen ändern das Instanzfeld `s`.

#### <a name="output-parameters"></a>Ausgabeparameter

Ein mit einem `out` -Modifizierer deklarierter Parameter ist ein Output-Parameter. Ähnlich wie ein Verweis Parameter wird von einem Output-Parameter kein neuer Speicherort erstellt. Stattdessen stellt ein Ausgabeparameter denselben Speicherort wie die Variable dar, die im Methodenaufruf als Argument angegeben wurde.

Wenn ein formaler Parameter ein Ausgabeparameter ist, muss das entsprechende Argument in einem Methodenaufruf aus dem Schlüsselwort `out` gefolgt von einem *variable_reference* ([exakte Regeln zum Bestimmen der eindeutigen Zuweisung](variables.md#precise-rules-for-determining-definite-assignment)) desselben Typs wie dem bestehen. formaler Parameter. Eine Variable muss nicht definitiv zugewiesen werden, bevor Sie als Output-Parameter übergeben werden kann. nach einem Aufruf, bei dem eine Variable als Output-Parameter übergeben wurde, wird die Variable als definitiv zugewiesen betrachtet.

Innerhalb einer Methode wird ein Ausgabeparameter, wie eine lokale Variable, anfänglich als nicht zugewiesen betrachtet und muss definitiv zugewiesen werden, bevor der Wert verwendet wird.

Jeder Ausgabeparameter einer Methode muss definitiv zugewiesen werden, bevor die Methode zurückgegeben wird.

Eine Methode, die als partielle Methode ([partielle Methoden](classes.md#partial-methods)) oder Iterator ([Iteratoren](classes.md#iterators)) deklariert ist, darf keine Ausgabeparameter aufweisen.

Ausgabeparameter werden in der Regel in Methoden verwendet, die mehrere Rückgabewerte liefern. Zum Beispiel:
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
```console
c:\Windows\System\
hello.txt
```

Beachten Sie, `dir` dass `name` die Zuweisung der-Variable und der-Variablen aufgeh `SplitPath`oben werden kann, bevor Sie an übermittelt werden, und dass Sie nach dem-Befehl definitiv zugewiesen werden

#### <a name="parameter-arrays"></a>Parameterarrays

Ein Parameter, der mit `params` einem-Modifizierer deklariert wird, ist ein Parameter Array. Wenn eine Liste formaler Parameter ein Parameter Array enthält, muss es sich um den letzten Parameter in der Liste handeln, und es muss sich um einen eindimensionalen Arraytyp handeln. Beispielsweise können die- `string[]` Typen `string[][]` und als Typ eines Parameter Arrays verwendet werden, aber der-Typ `string[,]` ist nicht möglich. Es ist nicht möglich, den `params` -Modifizierer mit den Modifizierern `ref` und `out`zu kombinieren.

Ein Parameter Array ermöglicht das Angeben von Argumenten in einem Methodenaufruf auf zwei Arten:

*  Das für ein Parameter Array angegebene Argument kann ein einzelner Ausdruck sein, der implizit konvertierbar ist ([implizite Konvertierungen](conversions.md#implicit-conversions)), in den Typ des Parameter Arrays. In diesem Fall verhält sich das Parameter Array genau wie ein value-Parameter.
*  Alternativ kann der Aufruf NULL oder mehr Argumente für das Parameter Array angeben, wobei jedes Argument ein Ausdruck ist, der implizit konvertierbar ist ([implizite Konvertierungen](conversions.md#implicit-conversions)), in den Elementtyp des Parameter Arrays. In diesem Fall erstellt der Aufruf eine Instanz des Parameter Array Typs mit einer Länge, die der Anzahl der Argumente entspricht, initialisiert die Elemente der Array Instanz mit den angegebenen Argument Werten und verwendet die neu erstellte Array Instanz als die tatsächliche gestritten.

Mit der Ausnahme, dass eine Variable Anzahl von Argumenten in einem Aufruf zugelassen wird, entspricht ein Parameter Array exakt einem Wert Parameter ([value-Parameter](classes.md#value-parameters)) desselben Typs.

Das Beispiel
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
```console
Array contains 3 elements: 1 2 3
Array contains 4 elements: 10 20 30 40
Array contains 0 elements:
```

Beim ersten Aufruf von `F` wird das Array `a` einfach als value-Parameter übergeben. Der zweite Aufruf von `F` erstellt automatisch ein vier-Element `int[]` mit den angegebenen Element Werten und übergibt diese Array Instanz als Wert Parameter. Ebenso erstellt der dritte Aufruf von `F` ein NULL-Element `int[]` und übergibt diese Instanz als value-Parameter. Der zweite und der dritte Aufruf entsprechen genau dem Schreiben:
```csharp
F(new int[] {10, 20, 30, 40});
F(new int[] {});
```

Beim Durchführen der Überladungs Auflösung kann eine Methode mit einem Parameter Array entweder in der normalen Form oder in der erweiterten Form ([anwendbares Funktionsmember](expressions.md#applicable-function-member)) anwendbar sein. Die erweiterte Form einer Methode ist nur verfügbar, wenn die normale Form der Methode nicht anwendbar ist und nur, wenn eine anwendbare Methode mit derselben Signatur wie das erweiterte Formular nicht bereits im selben Typ deklariert ist.

Das Beispiel
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
```console
F();
F(object[]);
F(object,object);
F(object[]);
F(object[]);
```

Im Beispiel sind zwei der möglichen erweiterten Formen der-Methode mit einem Parameter Array bereits als reguläre Methoden in der-Klasse enthalten. Diese erweiterten Formulare werden daher bei der Überladungs Auflösung nicht berücksichtigt, und der erste und dritte Methodenaufruf wählen daher die regulären Methoden aus. Wenn eine Klasse eine Methode mit einem Parameter Array deklariert, ist es nicht ungewöhnlich, dass auch einige der erweiterten Formulare als reguläre Methoden enthalten sind. Auf diese Weise ist es möglich, die Zuordnung einer Array Instanz zu vermeiden, die auftritt, wenn eine erweiterte Form einer Methode mit einem Parameter Array aufgerufen wird.

Wenn der Typ eines Parameter Arrays ist `object[]`, entsteht eine potenzielle Mehrdeutigkeit zwischen der normalen Form der-Methode und dem aufgewendet-Formular für einen einzelnen `object` Parameter. Der Grund für die Mehrdeutigkeit besteht darin `object[]` , dass eine selbst implizit in `object`den Typ konvertiert werden kann. Die Mehrdeutigkeit stellt jedoch kein Problem dar, da Sie durch Einfügen einer Umwandlung bei Bedarf aufgelöst werden kann.

Das Beispiel
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
```console
System.Int32 System.String System.Double
System.Object[]
System.Object[]
System.Int32 System.String System.Double
```

Im ersten und letzten Aufruf von `F`ist die normale Form von `F` anwendbar, da eine implizite Konvertierung vom Argumenttyp in den Parametertyp besteht (beide sind vom Typ `object[]`). Folglich wählt die Überladungs Auflösung die normale Form `F`von aus, und das-Argument wird als regulärer Wert Parameter übergeben. Im zweiten und dritten Aufruf ist die normale Form von `F` nicht anwendbar, weil keine implizite Konvertierung vom Argumenttyp zum Parametertyp vorhanden ist (der Typ `object` kann nicht implizit in den Typ `object[]`konvertiert werden). Allerdings ist die erweiterte Form `F` von anwendbar, sodass Sie durch Überladungs Auflösung ausgewählt wird. Folglich wird ein One-Element `object[]` durch den Aufruf erstellt, und das einzelne Element des Arrays wird mit dem angegebenen Argument Wert initialisiert (der selbst ein Verweis auf ein `object[]`ist).

### <a name="static-and-instance-methods"></a>Statische Methoden und Instanzmethoden

Wenn eine Methoden Deklaration einen `static` -Modifizierer enthält, wird diese Methode als statische Methode bezeichnet. Wenn kein `static` Modifizierer vorhanden ist, wird die Methode als Instanzmethode bezeichnet.

Eine statische Methode funktioniert nicht für eine bestimmte Instanz, und Sie ist ein Kompilierzeitfehler, auf den `this` in einer statischen Methode verwiesen wird.

Eine Instanzmethode arbeitet auf einer bestimmten Instanz einer Klasse, und auf diese Instanz kann als `this` ([dieser Zugriff](expressions.md#this-access)) zugegriffen werden.

Wenn auf eine Methode in einem *member_access* -Element ([Member Access](expressions.md#member-access)) der Form `E.M` verwiesen wird, wenn `M` eine statische Methode ist, muss `E` einen Typ angeben, der `M` enthält, und wenn `M` eine Instanzmethode ist, muss `E` eine Instanz eines Typs bezeichnen. mit `M`.

Die Unterschiede zwischen statischen und Instanzmembern werden in [statischen und Instanzmembern](classes.md#static-and-instance-members)ausführlicher erläutert.

### <a name="virtual-methods"></a>Virtuelle Methoden

Wenn eine Instanzmethodendeklaration einen `virtual` -Modifizierer enthält, wird diese Methode als virtuelle Methode bezeichnet. Wenn kein `virtual` Modifizierer vorhanden ist, wird die Methode als nicht virtuelle Methode bezeichnet.

Die Implementierung einer nicht virtuellen Methode ist unveränderlich: Die-Implementierung ist identisch, unabhängig davon, ob die-Methode für eine Instanz der-Klasse, in der Sie deklariert ist, oder für eine Instanz einer abgeleiteten Klasse aufgerufen wird. Im Gegensatz dazu kann die Implementierung einer virtuellen Methode durch abgeleitete Klassen abgelöst werden. Der Prozess der Ersetzung der Implementierung einer geerbten virtuellen Methode ***wird als über*** Schreiben dieser Methode bezeichnet ([Überschreibungs Methoden](classes.md#override-methods)).

Beim Aufruf einer virtuellen Methode bestimmt der ***Lauf Zeittyp*** der Instanz, für die dieser Aufruf erfolgt, die tatsächliche Methoden Implementierung, die aufgerufen werden soll. Bei einem Aufruf der nicht virtuellen Methode ist der ***Kompilier Zeittyp*** der Instanz der bestimmende Faktor. Genauer gesagt, wenn eine Methode mit dem `N` Namen mit einer Argumentliste `A` für eine Instanz mit einem Kompilier Zeittyp `C` und einem Lauf Zeittyp `R` aufgerufen wird `R` (wobei `C` entweder oder eine von abgeleitete Klasse ist). von `C`) wird der Aufruf wie folgt verarbeitet:

*  Zuerst wird die Überladungs Auflösung auf `C`, `N`und `A`angewendet, um eine bestimmte Methode `M` aus dem Satz von Methoden auszuwählen, der in deklariert und `C`von geerbt wurde. Dies wird unter [Methodenaufrufe](expressions.md#method-invocations)beschrieben.
*  Wenn `M` dann eine nicht virtuelle Methode ist, `M` wird aufgerufen.
*  Andernfalls ist eine virtuelle Methode, und die am meisten abgeleitete Implementierung `M` von in Bezug `R` auf wird aufgerufen. `M`

Für jede virtuelle Methode, die in der Klasse deklariert oder von einer Klasse geerbt wurde, gibt es in Bezug auf diese Klasse eine ***am meisten abgeleitete Implementierung*** der-Methode. Die am weitesten abgeleitete Implementierung einer virtuellen `M` Methode in Bezug auf eine `R` Klasse wird wie folgt bestimmt:

*  Wenn `R` die Introducing `virtual` -Deklaration `M`von enthält, dann handelt es sich hierbei um `M`die am meisten abgeleitete Implementierung von.
*  Wenn `R` andernfalls einen `override` von `M`enthält, ist dies die am meisten abgeleitete Implementierung von `M`.
*  Andernfalls ist die am meisten abgeleitete `M` Implementierung von in `R` Bezug auf die gleiche wie die am meisten abgeleitete Implementierung von `M` in Bezug auf die direkte `R`Basisklasse von.

Im folgenden Beispiel werden die Unterschiede zwischen virtuellen und nicht virtuellen Methoden veranschaulicht:
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

Im Beispiel wird `A` eine nicht virtuelle Methode `F` und eine virtuelle Methode `G`eingeführt. Die- `B` Klasse führt eine neue nicht virtuelle Methode `F`ein, wodurch die geerbte `F`-Methode ausgeblendet wird, und über `G`schreibt auch die geerbte-Methode. Das Beispiel erzeugt die Ausgabe:
```console
A.F
B.F
B.G
B.G
```

Beachten Sie `B.G`, dass `a.G()` die-Anweisung `A.G`aufruft, nicht. Dies liegt daran, dass der Lauf Zeittyp der-Instanz ( `B`d. h.), nicht der Kompilier Zeittyp der `A`-Instanz (d. h.), die tatsächliche Methoden Implementierung bestimmt, die aufgerufen werden soll.

Da Methoden vererbte Methoden ausblenden dürfen, kann eine Klasse mehrere virtuelle Methoden mit der gleichen Signatur enthalten. Dies stellt kein mehrdeutigkeitsproblem dar, da alle außer der am weitesten abgeleiteten Methode ausgeblendet sind. Im Beispiel
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
die `C` Klassen `D` und enthalten zwei virtuelle Methoden mit der gleichen Signatur: Der von `A` eingeführte und der von `C`eingeführte. Die durch `C` eingeführte Methode blendet die von geerbte Methode aus `A`. Folglich überschreibt die Überschreibungs `D` Deklaration in die von `C`eingeführte Methode, und `D` es ist nicht möglich, die durch `A`eingeführte Methode zu überschreiben. Das Beispiel erzeugt die Ausgabe:
```console
B.F
B.F
D.F
D.F
```

Beachten Sie, dass es möglich ist, die verborgene virtuelle Methode aufzurufen, indem `D` Sie auf eine Instanz von über einen weniger abgeleiteten Typ zugreifen, in dem die Methode nicht ausgeblendet ist.

### <a name="override-methods"></a>Überschreibungs Methoden

Wenn eine Instanzmethodendeklaration einen `override` -Modifizierer enthält, wird die-Methode als ***Überschreibungs Methode***bezeichnet. Eine Überschreibungs Methode überschreibt eine geerbte virtuelle Methode mit derselben Signatur. Während eine Deklaration einer virtuellen Methode eine neue Methode einführt, spezialisiert eine Deklaration einer überschriebenen Methode eine vorhandene geerbte virtuelle Methode, indem eine neue Implementierung dieser Methode bereitgestellt wird.

Die Methode, die durch eine `override` -Deklaration überschrieben wird, wird als die ***überschriebene Basis Methode***bezeichnet. Bei einer in einer `M` Klasse `C`deklarierten Überschreibungs Methode wird die überschriebene Basis Methode bestimmt, indem jeder Basis Klassentyp von `C`untersucht wird, beginnend mit `C` dem direkten Basis Klassentyp von und mit jeder nachfolgenden direkter Basis Klassentyp, bis in einem gegebenen Basis Klassentyp mindestens eine barrierefreie Methode gefunden wird, die die gleiche Signatur `M` wie nach der Ersetzung von Typargumenten aufweist. Zum Ermitteln der überschriebenen Basis Methode wird eine Methode `public`als verfügbar `protected` `protected internal`betrachtet, wenn dies der Fall ist, wenn dies der Fall ist, oder wenn Sie in demselben `internal` Programm wie `C`deklariert und deklariert wird.

Ein Kompilierzeitfehler tritt auf, es sei denn, für eine Überschreibungs Deklaration gilt Folgendes:

*  Eine überschriebene Basis Methode kann wie oben beschrieben gefunden werden.
*  Es gibt genau eine solche überschriebene Basis Methode. Diese Einschränkung hat nur Auswirkungen, wenn der Basis Klassentyp ein konstruierter Typ ist, bei dem durch die Ersetzung von Typargumenten die Signatur zweier Methoden identisch ist.
*  Die überschriebene Basis Methode ist eine virtuelle Methode, eine abstrakte Methode oder eine Überschreibungs Methode. Anders ausgedrückt: die überschriebene Basis Methode kann nicht statisch oder nicht virtuell sein.
*  Die überschriebene Basis Methode ist keine versiegelte Methode.
*  Die Überschreibungs Methode und die überschriebene Basis Methode haben denselben Rückgabetyp.
*  Die Überschreibungs Deklaration und die überschriebene Basis Methode haben dieselbe deklarierte Zugriffsmethode. Anders ausgedrückt: eine Überschreibungs Deklaration kann nicht den Zugriff auf die virtuelle Methode ändern. Wenn die überschriebene Basis Methode jedoch intern geschützt ist und in einer anderen Assembly als der Assembly deklariert ist, die die Überschreibungs Methode enthält, muss die deklarierte Barrierefreiheit der Überschreibungs Methode geschützt werden.
*  Die Überschreibungs Deklaration gibt keine Type-Parameter-Einschränkungs Klauseln an. Stattdessen werden die Einschränkungen von der überschriebenen Basis Methode geerbt. Beachten Sie, dass Einschränkungen, die Typparameter in der überschriebenen Methode sind, durch Typargumente in der geerbten Einschränkung ersetzt werden können. Dies kann zu Einschränkungen führen, die nicht zulässig sind, wenn Sie explizit angegeben werden, z. b. Werttypen oder versiegelte Typen.

Im folgenden Beispiel wird veranschaulicht, wie die über schreibenden Regeln für generische Klassen funktionieren:
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

Eine Überschreibungs Deklaration kann mithilfe eines *base_access* ([Basis Zugriffs](expressions.md#base-access)) auf die überschriebene Basis Methode zugreifen. Im Beispiel
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
der `base.PrintFields()` Aufruf in `B` Ruft die `PrintFields` in `A`deklarierte Methode auf. Ein *base_access* deaktiviert den virtuellen Aufruf Mechanismus und behandelt die Basis Methode einfach als nicht virtuelle Methode. Wenn `B` der Aufruf von geschrieben `((A)this).PrintFields()`wurde, würde er die `PrintFields` in `B`deklarierte Methode rekursiv aufrufen, nicht die in `A`deklarierte Methode `PrintFields` , da der virtuelle und der Lauf Zeittyp von ist.`((A)this)` ist .`B`

Nur durch das einschließen `override` eines-Modifizierers kann eine Methode eine andere Methode überschreiben. In allen anderen Fällen verbirgt eine Methode, die dieselbe Signatur wie eine geerbte Methode hat, einfach die geerbte Methode. Im Beispiel
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
die `F` -Methode `B` in `override` enthält keinen-Modifizierer und überschreibt daher nicht `F` die- `A`Methode in. Stattdessen verbirgt die `F` -Methode `B` in die-Methode `A`in, und es wird eine Warnung ausgegeben, da die Deklaration `new` keinen Modifizierer enthält.

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
die `F` -Methode `B` in blendet `F` die von geerbte virtuelle Methode aus `A`. Da das neue `F` in `B` privaten Zugriff hat, umfasst sein Bereich nur den Klassen Text von `B` und wird nicht auf `C`erweitert. Daher ist es zulässig, `F` die `C` Deklaration von in zu `F` überschreiben `A`, das von geerbt wurde.

### <a name="sealed-methods"></a>Versiegelte Methoden

Wenn eine Instanzmethodendeklaration einen `sealed` -Modifizierer enthält, wird diese Methode als ***versiegelte Methode***bezeichnet. Wenn eine Instanzmethodendeklaration den `sealed` -Modifizierer enthält, muss Sie auch den `override` -Modifizierer einschließen. Die Verwendung des `sealed` -Modifizierers verhindert, dass eine abgeleitete Klasse die-Methode weiter überschreibt.

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
die- `B` Klasse stellt zwei Überschreibungs Methoden `F` bereit: eine Methode `sealed` , die über den `G` -Modifizierer und eine-Methode verfügt, die nicht. `B`die Verwendung des versiegelten `modifier` verhindert `C` , dass weiter `F`überschrieben wird.

### <a name="abstract-methods"></a>Abstrakte Methoden

Wenn eine Instanzmethodendeklaration einen `abstract` -Modifizierer enthält, wird diese Methode als ***abstrakte Methode***bezeichnet. Obwohl eine abstrakte Methode implizit auch eine virtuelle Methode ist, kann Sie nicht den-Modifizierer `virtual`aufweisen.

Eine abstrakte Methoden Deklaration führt eine neue virtuelle Methode ein, stellt jedoch keine Implementierung dieser Methode bereit. Stattdessen sind nicht abstrakte abgeleitete Klassen erforderlich, um eine eigene Implementierung bereitzustellen, indem diese Methode überschrieben wird. Da eine abstrakte Methode keine tatsächliche Implementierung bereitstellt, besteht das *method_body* einer abstrakten Methode einfach aus einem Semikolon.

Abstrakte Methoden Deklarationen sind nur in abstrakten Klassen zulässig ([abstrakte Klassen](classes.md#abstract-classes)).

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
die `Shape` -Klasse definiert das abstrakte Konzept eines geometrischen Shape-Objekts, das sich selbst zeichnen kann. Die `Paint` Methode ist abstrakt, da keine sinnvolle Standard Implementierung vorhanden ist. Die `Ellipse` Klassen `Box` und sind konkrete `Shape` Implementierungen. Da diese Klassen nicht abstrakt sind, müssen Sie die `Paint` -Methode überschreiben und eine tatsächliche Implementierung bereitstellen.

Es handelt sich um einen Kompilierzeitfehler für ein *base_access* ([Base Access](expressions.md#base-access)), der auf eine abstrakte Methode verweist. Im Beispiel
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
für den `base.F()` Aufruf wird ein Kompilierzeitfehler gemeldet, weil er auf eine abstrakte Methode verweist.

Eine abstrakte Methoden Deklaration darf eine virtuelle Methode überschreiben. Dadurch kann eine abstrakte Klasse die erneute Implementierung der Methode in abgeleiteten Klassen erzwingen und die ursprüngliche Implementierung der Methode nicht verfügbar machen. Im Beispiel
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
die `A` -Klasse deklariert eine virtuelle Methode `B` , die-Klasse überschreibt diese Methode mit einer abstrakten `C` -Methode, und die-Klasse überschreibt die abstrakte-Methode, um eine eigene Implementierung bereitzustellen.

### <a name="external-methods"></a>Externe Methoden

Wenn eine Methoden Deklaration einen `extern` -Modifizierer enthält, wird diese Methode als ***externe Methode***bezeichnet. Externe Methoden werden extern implementiert, in der Regel mit einer anderen C#Sprache als. Da eine externe Methoden Deklaration keine tatsächliche Implementierung bereitstellt, besteht das *method_body* einer externen Methode einfach aus einem Semikolon. Eine externe Methode darf nicht generisch sein.

Der `extern` -Modifizierer wird in der Regel in `DllImport` Verbindung mit einem-Attribut ([Interoperation mit com-und Win32-Komponenten](attributes.md#interoperation-with-com-and-win32-components)) verwendet, sodass externe Methoden durch DLLs (Dynamic Link Libraries) implementiert werden können. Die Ausführungsumgebung unterstützt möglicherweise andere Mechanismen, bei denen Implementierungen externer Methoden bereitgestellt werden können.

Wenn eine externe Methode ein `DllImport` -Attribut enthält, muss die Methoden Deklaration auch einen `static` -Modifizierer enthalten. Dieses Beispiel veranschaulicht die Verwendung des `extern` -Modifizierers und des `DllImport` -Attributs:
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

### <a name="partial-methods-recap"></a>Partielle Methoden (Recap)

Wenn eine Methoden Deklaration einen `partial` -Modifizierer enthält, wird diese Methode als ***partielle Methode***bezeichnet. Partielle Methoden können nur als Member von partiellen Typen ([partielle Typen](classes.md#partial-types)) deklariert werden und unterliegen einer Reihe von Einschränkungen. Partielle Methoden werden weiter unten in [partiellen Methoden](classes.md#partial-methods)beschrieben.

### <a name="extension-methods"></a>Erweiterungsmethoden

Wenn der erste Parameter einer Methode den `this` -Modifizierer enthält, wird diese Methode als ***Erweiterungsmethode***bezeichnet. Erweiterungs Methoden können nur in nicht generischen, nicht in der Tabelle statischen Klassen deklariert werden. Der erste Parameter einer Erweiterungsmethode kann keine anderen Modifizierer als `this`aufweisen, und der Parametertyp darf kein Zeigertyp sein.

Im folgenden finden Sie ein Beispiel für eine statische Klasse, die zwei Erweiterungs Methoden deklariert:
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

Eine Erweiterungsmethode ist eine reguläre statische Methode. Außerdem kann eine Erweiterungsmethode mit instanzmethodenaufruf-Syntax ([Erweiterungs Methodenaufrufe](expressions.md#extension-method-invocations)) mithilfe des Empfänger Ausdrucks als erstes Argument aufgerufen werden, wenn sich Ihre einschließende statische Klasse im Gültigkeitsbereich befindet.

Im folgenden Programm werden die oben deklarierten Erweiterungs Methoden verwendet:
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

Die `Slice` -Methode ist `string[]`in verfügbar, und die `ToInt32` -Methode ist in `string`verfügbar, da Sie als Erweiterungs Methoden deklariert wurden. Die Bedeutung des Programms ist mit den folgenden statischen Methoden aufrufen identisch:
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

### <a name="method-body"></a>Methoden Text

Der *method_body* einer Methoden Deklaration besteht aus einem Block Körper, einem Ausdrucks Körper oder einem Semikolon.

Der ***Ergebnistyp*** einer Methode ist `void` , wenn der Rückgabetyp ist `void`, oder wenn die Methode Async ist und der `System.Threading.Tasks.Task`Rückgabetyp ist. Andernfalls ist der Ergebnistyp einer nicht Async-Methode der Rückgabetyp, und der Ergebnistyp einer Async-Methode mit `System.Threading.Tasks.Task<T>` dem `T`Rückgabetyp ist.

Wenn eine Methode über einen `void` Ergebnistyp und einen Block Text `return` verfügt, ist es nicht zulässig, Anweisungen ([die Return-Anweisung](statements.md#the-return-statement)) im-Block anzugeben. Wenn die Ausführung des Blocks einer void-Methode normal abgeschlossen ist (d. h., der Ablauf der Steuerelemente wird vom Ende des Methoden Texts entfernt), kehrt diese Methode einfach an den aktuellen Aufrufer zurück.
    
Wenn eine Methode über ein `void`-Ergebnis und einen Ausdrucks Text verfügt, muss der Ausdruck `E` ein *statement_expression*sein, und der Text entspricht genau einem Block Text in der Form `{ E; }`.
    
Wenn eine Methode einen nicht leeren Ergebnistyp und einen Block Text hat, muss `return` jede Anweisung im Block einen Ausdruck angeben, der implizit in den Ergebnistyp konvertiert werden kann. Der Endpunkt eines Block Texts einer Wert Rückgabe Methode darf nicht erreichbar sein. Anders ausgedrückt: in einer Rückgabe Methode mit einem Block Text ist das Steuerelement nicht berechtigt, das Ende des Methoden Texts zu übertragen.
    
Wenn eine Methode einen nicht leeren Ergebnistyp und einen Ausdrucks Körper aufweist, muss der Ausdruck implizit in den Ergebnistyp konvertiert werden, und der Text entspricht genau dem Block Text des Formulars `{ return E; }`.
    
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
die Wert Rückgabe `F` Methode führt zu einem Kompilierzeitfehler, da das Steuerelement vom Ende des Methoden Texts entfernt werden kann. Die `G` - `H` Methode und die-Methode sind korrekt, da alle möglichen Ausführungs Pfade in einer Return-Anweisung enden, die einen Rückgabewert angibt. Die `I` Methode ist korrekt, da Ihr Text einem Anweisungsblock mit nur einer einzigen return-Anweisung entspricht.

### <a name="method-overloading"></a>Methodenüberladung

Die Regeln zur Auflösung von Methoden Überladungen werden unter [Typrückschluss](expressions.md#type-inference)beschrieben.

## <a name="properties"></a>Eigenschaften

Eine ***Eigenschaft*** ist ein Member, der den Zugriff auf ein Merkmal eines Objekts oder einer Klasse ermöglicht. Beispiele für Eigenschaften sind die Länge einer Zeichenfolge, die Größe einer Schriftart, die Beschriftung eines Fensters, der Name eines Kunden usw. Eigenschaften sind eine natürliche Erweiterung von Feldern – beide sind benannte Member mit zugeordneten Typen, und die Syntax für den Zugriff auf Felder und Eigenschaften ist identisch. Im Gegensatz zu Feldern bezeichnen Eigenschaften jedoch keine Speicherorte. Stattdessen verfügen Eigenschaften über ***Accessors*** zum Angeben der Anweisungen, die beim Lesen oder Schreiben ihrer Werte ausgeführt werden sollen. Eigenschaften bieten somit einen Mechanismus zum Zuordnen von Aktionen zum Lesen und Schreiben der Attribute eines Objekts. Außerdem können solche Attribute berechnet werden.

Eigenschaften werden mit *property_declaration*s deklariert:

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

Ein *property_declaration* kann einen Satz von *Attributen* ([Attribute](attributes.md)) und eine gültige Kombination der vier Zugriffsmodifizierer ([Zugriffsmodifizierer](classes.md#access-modifiers)), den `new` ([den neuen Modifizierer](classes.md#the-new-modifier)), `static` ([statisch und Instanz) enthalten. Methoden](classes.md#static-and-instance-methods)), `virtual` ([virtuelle Methoden](classes.md#virtual-methods)), `override` ([Überschreibungs Methoden](classes.md#override-methods)), `sealed` ([versiegelte Methoden](classes.md#sealed-methods)), `abstract` ([abstrakte Methoden](classes.md#abstract-methods)) und `extern`-Modifizierer ([Externe Methoden](classes.md#external-methods)).

Eigenschafts Deklarationen unterliegen den gleichen Regeln wie Methoden Deklarationen ([Methoden](classes.md#methods)) in Bezug auf gültige Kombinationen von modifiziererobjekten.

Der *Typ* einer Eigenschafts Deklaration gibt den Typ der Eigenschaft an, die von der Deklaration eingeführt wurde, und *MEMBER_NAME* gibt den Namen der Eigenschaft an. Wenn die Eigenschaft keine explizite Schnittstellenmember-Implementierung ist, ist *MEMBER_NAME* einfach ein *Bezeichner*. Bei einer expliziten Schnittstellenmember-Implementierung ([explizite Schnittstellenmember-Implementierungen](interfaces.md#explicit-interface-member-implementations)) besteht das *MEMBER_NAME* aus einem *INTERFACE_TYPE* , gefolgt von einem "`.`" und einem *Bezeichner*.

Der *Typ* einer Eigenschaft muss mindestens so zugänglich sein wie die Eigenschaft selbst (Barrierefreiheits[Einschränkungen](basic-concepts.md#accessibility-constraints)).

Ein *property_body* kann entweder aus einem ***Accessor-Text*** oder einem ***Ausdrucks Körper***bestehen. In einem Accessor-Text muss *accessor_declarations*, der in "`{`" und "`}`"-Token eingeschlossen werden muss, die Accessoren ([Accessoren](classes.md#accessors)) der Eigenschaft deklarieren. Die Accessoren geben die ausführbaren Anweisungen an, die dem Lesen und Schreiben der Eigenschaft zugeordnet sind.

Ein Ausdrucks Text, der `=>` aus gefolgt von einem *Ausdruck* `E` und einem Semikolon besteht, entspricht exakt dem `{ get { return E; } }`Anweisungs Text und kann daher nur zum Angeben von nur-Getter-Eigenschaften verwendet werden, bei denen das Ergebnis von der Getter wird durch einen einzelnen Ausdruck angegeben.

Ein *property_initializer* kann nur für eine automatisch implementierte Eigenschaft ([automatisch implementierte Eigenschaften](classes.md#automatically-implemented-properties)) angegeben werden und bewirkt die Initialisierung des zugrunde liegenden Felds dieser Eigenschaften mit dem Wert, der vom *Ausdruck angegeben wird.* .

Obwohl die Syntax für den Zugriff auf eine Eigenschaft mit der Syntax für ein Feld identisch ist, wird eine Eigenschaft nicht als Variable klassifiziert. Daher ist es nicht möglich, eine Eigenschaft als `ref` -oder `out` -Argument zu übergeben.

Wenn eine Eigenschafts Deklaration `extern` einen-Modifizierer enthält, wird die-Eigenschaft als ***externe Eigenschaft***bezeichnet. Da eine externe Eigenschaften Deklaration keine tatsächliche Implementierung bereitstellt, besteht jede Ihrer *accessor_declarations* aus einem Semikolon.

### <a name="static-and-instance-properties"></a>Statische Eigenschaften und Instanzeigenschaften

Wenn eine Eigenschafts Deklaration `static` einen-Modifizierer enthält, wird die-Eigenschaft als ***statische Eigenschaft***bezeichnet. Wenn kein `static` Modifizierer vorhanden ist, wird die Eigenschaft als ***Instanzeigenschaft***bezeichnet.

Eine statische Eigenschaft ist keiner bestimmten Instanz zugeordnet, und es handelt sich um einen Kompilierzeitfehler, auf den `this` in den Accessoren einer statischen Eigenschaft verwiesen wird.

Eine Instanzeigenschaft ist einer bestimmten Instanz einer Klasse zugeordnet, und auf diese Instanz kann als `this` ([dieser Zugriff](expressions.md#this-access)) in den Accessoren dieser Eigenschaft zugegriffen werden.

Wenn auf eine Eigenschaft in einem *member_access* ([Member Access](expressions.md#member-access)) der Form `E.M` verwiesen wird, wenn `M` eine statische Eigenschaft ist, muss `E` einen Typ angeben, der `M` enthält, und wenn `M` eine Instanzeigenschaft ist, muss E eine Instanz eines Typs angeben. mit `M`.

Die Unterschiede zwischen statischen und Instanzmembern werden in [statischen und Instanzmembern](classes.md#static-and-instance-members)ausführlicher erläutert.

### <a name="accessors"></a>Accessoren

Der *accessor_declarations* -Wert einer Eigenschaft gibt die ausführbaren Anweisungen an, die dem Lesen und Schreiben dieser Eigenschaft zugeordnet sind.

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

Die Accessordeklarationen bestehen aus *get_accessor_declaration*, *set_accessor_declaration*oder beidem. Jede Accessor-Deklaration besteht aus dem Token `get` oder `set`, gefolgt von einem optionalen *accessor_modifier* und einem *accessor_body*.

Die Verwendung von *accessor_modifier*s unterliegt den folgenden Einschränkungen:

*  Ein *accessor_modifier* kann nicht in einer Schnittstelle oder in einer expliziten Schnittstellenmember-Implementierung verwendet werden.
*  Für eine Eigenschaft oder einen Indexer, der über keinen `override`-Modifizierer verfügt, ist ein *accessor_modifier* nur zulässig, wenn die Eigenschaft oder der Indexer sowohl einen `get`-als auch einen `set`-Accessor hat und dann nur für einen dieser Accessoren zulässig ist.
*  Für eine Eigenschaft oder einen Indexer, die einen `override`-Modifizierer enthält, muss ein Accessor mit dem *accessor_modifier*(sofern vorhanden) der über schreibenden Zugriffsmethode identisch sein.
*  *Accessor_modifier* muss eine Barrierefreiheit deklarieren, die strikt restriktiver ist als der deklarierte Zugriff auf die Eigenschaft oder den Indexer selbst. Genauer gesagt:
   * Wenn die Eigenschaft oder der Indexer über eine deklarierte Zugriffsmöglichkeit für `public` verfügt, kann *accessor_modifier* entweder `protected internal`, `internal`, `protected` oder `private` sein.
   * Wenn die Eigenschaft oder der Indexer über eine deklarierte Zugriffsmöglichkeit für `protected internal` verfügt, kann *accessor_modifier* entweder `internal`, `protected` oder `private` sein.
   * Wenn die Eigenschaft oder der Indexer eine deklarierte Zugriffsmöglichkeit für `internal` oder `protected` aufweist, muss *accessor_modifier* `private` sein.
   * Wenn die Eigenschaft oder der Indexer über eine deklarierte Zugriffsmöglichkeit für `private` verfügt, darf kein *accessor_modifier* verwendet werden.

Bei `abstract`-und `extern`-Eigenschaften ist die *accessor_body* für jeden angegebenen Accessor einfach ein Semikolon. Eine nicht abstrakte, nicht externe Eigenschaft kann jede *accessor_body* ein Semikolon sein. in diesem Fall handelt es sich um eine ***automatisch implementierte Eigenschaft*** ([automatisch implementierte Eigenschaften](classes.md#automatically-implemented-properties)). Eine automatisch implementierte Eigenschaft muss mindestens über einen get-Accessor verfügen. Für die Accessoren anderer nicht abstrakter, nicht externer Eigenschaften ist *accessor_body* ein- *Block* , der die auszuführenden Anweisungen angibt, wenn der entsprechende-Accessor aufgerufen wird.

Ein `get` -Accessor entspricht einer Parameter losen Methode mit einem Rückgabewert des Eigenschaftentyps. Wenn in einem Ausdruck `get` auf eine Eigenschaft verwiesen wird, wird der-Accessor der Eigenschaft aufgerufen, um den Wert der-Eigenschaft ([Werte von Ausdrücken](expressions.md#values-of-expressions)) zu berechnen, außer als Ziel einer Zuweisung. Der Text einer `get` -Zugriffsmethode muss den Regeln für die Rückgabe von Werten entsprechen, die im [Methoden Text](classes.md#method-body)beschrieben werden. Insbesondere müssen alle `return` Anweisungen im Textkörper einer `get` -Zugriffsmethode einen Ausdruck angeben, der implizit in den Eigenschaftentyp konvertiert werden kann. Außerdem darf der Endpunkt einer `get` -Zugriffsmethode nicht erreichbar sein.

Ein `set` -Accessor entspricht einer Methode mit einem einzelnen Wert Parameter des Eigenschaftentyps `void` und einem Rückgabetyp. Der implizite Parameter einer `set` -Zugriffsmethode wird immer benannt. `value` Wenn auf eine Eigenschaft als Ziel einer Zuweisung ([Zuweisungs Operatoren](expressions.md#assignment-operators)) oder `++` als Operand von oder `--` ([postfix Inkrement-und Dekrementoperatoren](expressions.md#postfix-increment-and-decrement-operators), [Präfix Inkrement-und Dekrementoperatoren](expressions.md#prefix-increment-and-decrement-operators)) verwiesen wird, wird der der Accessor wird mit einem Argument aufgerufen (dessen Wert dem der rechten Seite der Zuweisung oder dem Operanden `++` des or `--` -Operators entspricht), der den neuen Wert ([einfache Zuweisung](expressions.md#simple-assignment)) bereitstellt. `set` Der Text einer `set` -Zugriffsmethode muss den Regeln für `void` Methoden entsprechen, die im [Methoden Text](classes.md#method-body)beschrieben werden. Insbesondere können `return` Anweisungen `set` im Accessor-Text keinen Ausdruck angeben. Da ein `set` -Accessor implizit einen Parameter mit `value`dem Namen aufweist, handelt es sich um einen Kompilierzeitfehler für eine lokale Variable `set` oder eine Konstante Deklaration in einem-Accessor, der über diesen Namen verfügt.

Basierend auf dem vorhanden sein oder fehlen `get` der-und- `set` Accessoren wird eine Eigenschaft wie folgt klassifiziert:

*  Eine Eigenschaft, die sowohl einen `get` -Accessor als `set` auch einen-Accessor enthält, wird als ***Lese-/Schreib-Eigenschaft*** bezeichnet.
*  Eine Eigenschaft, die nur einen `get` -Accessor aufweist, wird als Schreib ***geschützte Eigenschaft bezeichnet*** . Es handelt sich um einen Kompilierzeitfehler für eine schreibgeschützte Eigenschaft, die das Ziel einer Zuweisung ist.
*  Eine Eigenschaft, die nur einen `set` -Accessor aufweist, wird als ***Schreib*** geschützte Eigenschaft bezeichnet. Außer als Ziel einer Zuweisung ist es ein Kompilierzeitfehler, der auf eine schreibgeschützte Eigenschaft in einem Ausdruck verweist.

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
Das `Button` -Steuerelement deklariert `Caption` eine öffentliche Eigenschaft. Der `get` -Accessor `Caption` der-Eigenschaft gibt die Zeichenfolge zurück, `caption` die im privaten Feld gespeichert ist. Der `set` -Accessor überprüft, ob sich der neue Wert vom aktuellen Wert unterscheidet. wenn dies der Fall ist, wird der neue Wert gespeichert und das Steuerelement neu gezeichnet. Eigenschaften folgen häufig dem oben gezeigten Muster: Der `get` -Accessor gibt einfach einen in einem privaten Feld gespeicherten Wert zurück, `set` und der-Accessor ändert dieses private Feld und führt dann alle zusätzlichen Aktionen aus, die erforderlich sind, um den Status des Objekts vollständig zu aktualisieren.

Bei der `Button` obigen Klasse ist Folgendes ein Beispiel für die `Caption` Verwendung der-Eigenschaft:
```csharp
Button okButton = new Button();
okButton.Caption = "OK";            // Invokes set accessor
string s = okButton.Caption;        // Invokes get accessor
```

Hier wird der `set` -Accessor aufgerufen, indem der-Eigenschaft ein Wert zugewiesen wird, `get` und der-Accessor wird aufgerufen, indem auf die-Eigenschaft in einem Ausdruck verwiesen wird.

Die `get` - `set` und-Accessoren einer Eigenschaft sind keine unterschiedlichen Member, und es ist nicht möglich, die Accessoren einer Eigenschaft separat zu deklarieren. Daher ist es nicht möglich, dass die beiden Accessoren einer Eigenschaft mit Lese-/Schreibzugriff über unterschiedliche Zugriffsberechtigungen verfügen. Das Beispiel
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
deklariert keine einzige Lese-/Schreibeigenschaft. Stattdessen werden zwei Eigenschaften mit demselben Namen deklariert, ein Schreib geschützter und ein Schreib geschützter. Da zwei Member, die in derselben Klasse deklariert werden, nicht denselben Namen haben können, bewirkt das Beispiel, dass ein Kompilierzeitfehler auftritt.

Wenn eine abgeleitete Klasse eine Eigenschaft mit demselben Namen wie eine geerbte Eigenschaft deklariert, verbirgt die abgeleitete Eigenschaft die geerbte Eigenschaft in Bezug auf Lese-und Schreibvorgänge. Im Beispiel
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
die `P` -Eigenschaft `B` in blendet die `A` -Eigenschaft in in Bezug auf das `P` lesen und schreiben aus. Folglich in den Anweisungen
```csharp
B b = new B();
b.P = 1;          // Error, B.P is read-only
((A)b).P = 1;     // Ok, reference to A.P
```
die Zuweisung von `b.P` bewirkt, dass ein Kompilierzeitfehler gemeldet wird, da die `P` schreibgeschützte Eigenschaft in `B` die Schreib `P` geschützte Eigenschaft in `A`ausblendet. Beachten Sie jedoch, dass eine Umwandlung verwendet werden kann, um auf die `P` Hidden-Eigenschaft zuzugreifen.

Im Gegensatz zu öffentlichen Feldern stellen Eigenschaften eine Trennung zwischen dem internen Zustand eines Objekts und seiner öffentlichen Schnittstelle dar. Sehen Sie sich das Beispiel an:
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

Hier verwendet die `Label` -Klasse zwei `int` Felder, `x` und `y`, um ihren Speicherort zu speichern. `X` Der Speicherort wird sowohl als als `Y` auch `Location` als Eigenschaft des Typs `Point`öffentlich verfügbar gemacht. Wenn es in einer zukünftigen Version von `Label`bequemer wird, den Speicherort `Point` als intern zu speichern, kann die Änderung vorgenommen werden, ohne dass sich dies auf die öffentliche Schnittstelle der Klasse auswirkt:
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

Waren `x` und `y` stattdessen `Label` Felder, wäre es unmöglich, eine solche Änderung an der-Klasse vorzunehmen. `public readonly`

Das verfügbar machen des Zustands durch Eigenschaften ist nicht notwendigerweise weniger effizient, als Felder direkt verfügbar zu machen. Insbesondere wenn eine Eigenschaft nicht virtuell ist und nur eine kleine Menge an Code enthält, kann die Ausführungsumgebung Aufrufe von Accessoren durch den tatsächlichen Code der Accessoren ersetzen. Dieser Prozess wird als ***Inlining***bezeichnet und macht den Eigenschafts Zugriff so effizient wie der Feld Zugriff, behält jedoch die höhere Flexibilität von Eigenschaften bei.

Da das Aufrufen `get` eines Accessoren konzeptionell Äquivalent zum Lesen des Werts eines Felds ist, gilt es als ungültiges Programmier Format `get` für Accessoren, um Observable-Nebeneffekte zu haben. Im Beispiel
```csharp
class Counter
{
    private int next;

    public int Next {
        get { return next++; }
    }
}
```
der Wert `Next` der-Eigenschaft hängt von der Häufigkeit ab, mit der zuvor auf die Eigenschaft zugegriffen wurde. Folglich erzeugt der Zugriff auf die-Eigenschaft einen beobachtbaren Nebeneffekt, und die-Eigenschaft sollte stattdessen als Methode implementiert werden.

Die "No Side-Effects"-Konvention `get` für Accessoren bedeutet nicht `get` , dass Accessoren immer so geschrieben werden müssen, dass Sie einfach in Feldern gespeicherte Werte zurückgeben. Accessoren `get` berechnen häufig den Wert einer Eigenschaft, indem Sie auf mehrere Felder zugreifen oder Methoden aufrufen. Ein ordnungsgemäß entworfener `get` Accessor führt jedoch keine Aktionen aus, die Observable-Änderungen im Status des Objekts bewirken.

Eigenschaften können verwendet werden, um die Initialisierung einer Ressource zu verzögern, bis zu dem Zeitpunkt, zu dem Sie erstmals referenziert wird. Zum Beispiel:
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

Die `Console` -Klasse enthält die drei `In`Eigenschaften `Out`,, `Error`und, die jeweils die standardmäßigen Eingabe-, Ausgabe-und Fehler Geräte darstellen. Wenn diese Member als Eigenschaften verfügbar gemacht werden `Console` , kann die-Klasse Ihre Initialisierung verzögern, bis Sie tatsächlich verwendet werden. Beispielsweise nach dem ersten Verweis auf die `Out` -Eigenschaft, wie in
```csharp
Console.Out.WriteLine("hello, world");
```
der zugrunde `TextWriter` liegende für das Ausgabegerät wird erstellt. Wenn die Anwendung jedoch keinen Verweis auf die- `In` Eigenschaft `Error` und die-Eigenschaft durchführt, werden für diese Geräte keine Objekte erstellt.

### <a name="automatically-implemented-properties"></a>Automatisch implementierte Eigenschaften

Eine automatisch implementierte Eigenschaft (oder ***Auto-Eigenschaft*** für Short) ist eine nicht abstrakte nicht-externe Eigenschaft mit nur Semikolon-Zugriffsmethoden. Auto-Eigenschaften müssen über einen get-Accessor verfügen und optional über einen Set-Accessor verfügen.

Wenn eine Eigenschaft als automatisch implementierte Eigenschaft angegeben wird, ist ein verborgenes dahinter liegendes Feld für die Eigenschaft automatisch verfügbar, und die Accessoren werden implementiert, um aus dem dahinter liegenden Feld zu lesen und in dieses zu schreiben. Wenn die Auto-Eigenschaft keinen Set-Accessor aufweist, wird das Unterstützungs Feld `readonly` als (schreibgeschützte[Felder](classes.md#readonly-fields)) betrachtet. Genau wie ein `readonly` Feld kann auch eine nur-Getter-Auto-Eigenschaft im Text eines Konstruktors der einschließenden Klasse zugewiesen werden. Eine solche Zuweisung wird direkt dem schreibgeschützten Unterstützungs Feld der-Eigenschaft zugewiesen.

Eine Auto-Eigenschaft kann optional über ein *property_initializer*verfügen, das direkt auf das Unterstützungs Feld als *variable_initializer* ([Variableninitialisierer](classes.md#variable-initializers)) angewendet wird.

Im Beispiel unten geschieht Folgendes:
```csharp
public class Point {
    public int X { get; set; } = 0;
    public int Y { get; set; } = 0;
}
```
entspricht der folgenden Deklaration:
```csharp
public class Point {
    private int __x = 0;
    private int __y = 0;
    public int X { get { return __x; } set { __x = value; } }
    public int Y { get { return __y; } set { __y = value; } }
}
```

Im Beispiel unten geschieht Folgendes:
```csharp
public class ReadOnlyPoint
{
    public int X { get; }
    public int Y { get; }
    public ReadOnlyPoint(int x, int y) { X = x; Y = y; }
}
```
entspricht der folgenden Deklaration:
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

Beachten Sie, dass die Zuweisungen zum schreibgeschützten Feld zulässig sind, da Sie im Konstruktor auftreten.


### <a name="accessibility"></a>Zugriff

Wenn ein Accessor über eine *accessor_modifier*verfügt, wird die Zugriffs Domäne ([Barrierefreiheits Domänen](basic-concepts.md#accessibility-domains)) der Zugriffsmethode mithilfe des deklarierten zugriffszugriffs auf die *accessor_modifier*festgelegt. Wenn ein Accessor nicht über eine *accessor_modifier*verfügt, wird die Zugriffs Domäne des Accessors anhand der deklarierten Zugriffsmethode der Eigenschaft oder des Indexers bestimmt.

Das vorhanden sein eines *accessor_modifier* hat niemals Auswirkungen auf die Suche nach Membern ([Operatoren](expressions.md#operators)) oder Überladungs Auflösung ([Überladungs Auflösung](expressions.md#overload-resolution)). Die Modifizierer für die Eigenschaft oder den Indexer bestimmen unabhängig vom Kontext des Zugriffs immer, an welche Eigenschaft oder welcher Indexer gebunden ist.

Nachdem eine bestimmte Eigenschaft oder ein Indexer ausgewählt wurde, werden die Barrierefreiheits Domänen der beteiligten spezifischen Accessoren verwendet, um zu bestimmen, ob diese Verwendung gültig ist:

*  Wenn die Verwendung als Wert ([Werte von Ausdrücken](expressions.md#values-of-expressions)) verwendet wird, muss `get` der Accessor vorhanden und zugänglich sein.
*  Wenn die Verwendung als Ziel einer einfachen Zuweisung ([einfache Zuweisung](expressions.md#simple-assignment)) verwendet wird, muss der `set` -Accessor vorhanden und zugänglich sein.
*  Wenn die Verwendung als Ziel der Verbund Zuweisung ([Verbund Zuweisung](expressions.md#compound-assignment)) oder `++` als Ziel der Operatoren oder `--` ([Funktionsmember](expressions.md#function-members)0,9, [Aufruf Ausdrücke](expressions.md#invocation-expressions)) verwendet wird, werden sowohl die `get` Accessoren als auch der `set` -Accessor muss vorhanden und zugänglich sein.

Im folgenden Beispiel wird die-Eigenschaft `A.Text` durch die-Eigenschaft `B.Text`ausgeblendet, auch in Kontexten, in denen `set` nur der-Accessor aufgerufen wird. Im Gegensatz dazu kann die `B.Count` -Eigenschaft nicht von der `M`-Klasse aufgerufen werden, `A.Count` sodass stattdessen die barrierefreie Eigenschaft verwendet wird.

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

Ein Accessor, der zur Implementierung einer Schnittstelle verwendet wird, verfügt möglicherweise nicht über eine *accessor_modifier*. Wenn nur ein Accessor verwendet wird, um eine Schnittstelle zu implementieren, kann der andere Accessor mit einem *accessor_modifier*deklariert werden:
```csharp
public interface I
{
    string Prop { get; }
}

public class C: I
{
    public string Prop {
        get { return "April"; }       // Must not have a modifier here
        internal set {...}            // Ok, because I.Prop has no set accessor
    }
}
```

### <a name="virtual-sealed-override-and-abstract-property-accessors"></a>Virtual-, sealed-, override-und Abstract-Eigenschaftenaccessoren

Eine `virtual` Eigenschafts Deklaration gibt an, dass die Accessoren der Eigenschaft virtuell sind. Der `virtual` -Modifizierer gilt für beide Accessoren einer Lese-/Schreibeigenschaft – es ist nicht möglich, dass nur ein Accessor einer Eigenschaft mit Lese-/Schreibzugriff virtuell ist.

Eine `abstract` Eigenschafts Deklaration gibt an, dass die Accessoren der Eigenschaft virtuell sind, aber keine tatsächliche Implementierung der Accessoren bereitstellt. Stattdessen sind nicht abstrakte abgeleitete Klassen erforderlich, um eine eigene Implementierung für die Accessoren bereitzustellen, indem die-Eigenschaft überschrieben wird. Da ein Accessor für eine abstrakte Eigenschaften Deklaration keine tatsächliche Implementierung bereitstellt, besteht seine *accessor_body* einfach aus einem Semikolon.

Eine Eigenschafts Deklaration, die `abstract` den `override` -Modifizierer und den-Modifizierer enthält, gibt an, dass die Eigenschaft abstrakt ist, und überschreibt Die Accessoren einer solchen Eigenschaft sind ebenfalls abstrakt.

Abstrakte Eigenschafts Deklarationen sind nur in abstrakten Klassen zulässig ([abstrakte Klassen](classes.md#abstract-classes)). Die Accessoren einer geerbten virtuellen Eigenschaft können in einer abgeleiteten Klasse überschrieben werden, indem Sie eine Eigenschaften Deklaration `override` einschließen, die eine-Direktive angibt. Dies wird als über schreibende ***Eigenschaften Deklaration***bezeichnet. Eine über schreibende Eigenschaften Deklaration deklariert keine neue Eigenschaft. Stattdessen werden lediglich die Implementierungen der Accessoren einer vorhandenen virtuellen Eigenschaft spezialisiert.

Eine über schreibende Eigenschaften Deklaration muss genau dieselben Zugriffsmodifizierer, denselben Typ und denselben Namen wie die geerbte Eigenschaft angeben. Wenn die geerbte Eigenschaft nur über einen einzigen Accessor verfügt (d. h., wenn die geerbte Eigenschaft schreibgeschützt oder schreibgeschützt ist), muss die über schreibende Eigenschaft nur den Accessor enthalten. Wenn die geerbte Eigenschaft beide Accessoren enthält (d. h., wenn die geerbte Eigenschaft Lese-/Schreibzugriff hat), kann die über schreibende Eigenschaft entweder einen einzelnen Accessor oder beide Accessoren einschließen.

Eine über schreibende Eigenschaften Deklaration `sealed` kann den-Modifizierer enthalten. Durch die Verwendung dieses Modifizierers wird verhindert, dass eine abgeleitete Klasse die Eigenschaft weiter überschreibt. Die Accessoren einer versiegelten Eigenschaft sind ebenfalls versiegelt.

Mit Ausnahme der Unterschiede in der Deklaration und der Aufruf Syntax Verhalten sich virtuelle, versiegelte, Überschreibungs-und abstrakte Accessoren genauso wie virtuelle, versiegelte, Überschreibungs-und abstrakte Methoden. Insbesondere gelten die in [virtuellen Methoden](classes.md#virtual-methods), [Methoden](classes.md#override-methods)zum überschreiben, [versiegelten Methoden](classes.md#sealed-methods)und [abstrakten Methoden](classes.md#abstract-methods) beschriebenen Regeln so, als wären Accessoren Methoden einer entsprechenden Form:

*  Ein `get` -Accessor entspricht einer Parameter losen Methode mit einem Rückgabewert des Eigenschaftentyps und denselben Modifiziererwerten wie die enthaltende Eigenschaft.
*  Ein `set` -Accessor entspricht einer Methode mit einem einzelnen Wert Parameter des Eigenschaftentyps `void` , einem Rückgabetyp und denselben Modifiziererwerten wie die enthaltende Eigenschaft.

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
`X`ist eine virtuelle schreibgeschützte Eigenschaft, `Y` ist eine virtuelle Lese-/Schreibeigenschaft und `Z` eine abstrakte Lese-/Schreibeigenschaft. Da `Z` abstrakt ist, muss die enthaltende Klasse `A` auch als abstrakt deklariert werden.

Eine Klasse, die von `A` abgeleitet wird, wird unten angezeigt:
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

Hier überschreiben die Deklarationen `Y`von `X`, `Z` und Eigenschafts Deklarationen. Jede Eigenschaften Deklaration stimmt genau mit den zugriffsmodifizierertypen und dem Namen der entsprechenden geerbten Eigenschaft überein. Der `get` -Accessor `X` von und `set` der-Accessor von `base` `Y` verwenden das-Schlüsselwort, um auf die geerbten Accessoren zuzugreifen. Die Deklaration `Z` von überschreibt beide abstrakten Accessoren – folglich gibt es keine ausstehenden abstrakten Funktionsmember `B`in, `B` und es darf sich um eine nicht abstrakte Klasse handeln.

Wenn eine Eigenschaft als `override`deklariert wird, müssen alle überschriebenen Accessoren für den über schreibenden Code zugänglich sein. Außerdem muss der deklarierte Zugriff sowohl für die Eigenschaft oder den Indexer selbst als auch für die Accessoren mit der der überschriebenen Member und Accessoren identisch sein. Zum Beispiel:
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

Ein ***Ereignis*** ist ein Member, mit dem ein Objekt oder eine Klasse Benachrichtigungen bereitstellen kann. Clients können ausführbaren Code durch Bereitstellen von ***Ereignis Handlern***für Ereignisse anfügen.

Ereignisse werden mit *event_declaration*s deklariert:

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

Ein *event_declaration* kann einen Satz von *Attributen* ([Attribute](attributes.md)) und eine gültige Kombination der vier Zugriffsmodifizierer ([Zugriffsmodifizierer](classes.md#access-modifiers)), den `new` ([den neuen Modifizierer](classes.md#the-new-modifier)), `static` ([statisch und Instanz) enthalten. Methoden](classes.md#static-and-instance-methods)), `virtual` ([virtuelle Methoden](classes.md#virtual-methods)), `override` ([Überschreibungs Methoden](classes.md#override-methods)), `sealed` ([versiegelte Methoden](classes.md#sealed-methods)), `abstract` ([abstrakte Methoden](classes.md#abstract-methods)) und `extern`-Modifizierer ([Externe Methoden](classes.md#external-methods)).

Ereignis Deklarationen unterliegen den gleichen Regeln wie Methoden Deklarationen ([Methoden](classes.md#methods)) in Bezug auf gültige Kombinationen von modifiziererereignissen.

Der *Typ* einer Ereignis Deklaration muss ein *delegate_type* ([Verweis Typen](types.md#reference-types)) sein, und *delegate_type* muss mindestens so zugänglich sein wie das Ereignis selbst ([Barrierefreiheits Einschränkungen](basic-concepts.md#accessibility-constraints)).

Eine Ereignis Deklaration kann *event_accessor_declarations*enthalten. Wenn dies jedoch nicht der Fall ist, stellt der Compiler Sie für nicht-externe, nicht abstrakte Ereignisse automatisch bereit ([Feld ähnliche Ereignisse](classes.md#field-like-events)). bei externen Ereignissen werden die Accessoren extern bereitgestellt.

Eine Ereignis Deklaration, die *event_accessor_declarations* auslässt, definiert mindestens ein Ereignis – eines für jede *variable_declarator*s. Die Attribute und Modifizierer gelten für alle Member, die von einem solchen *event_declaration*deklariert werden.

Es handelt sich um einen Kompilierzeitfehler für ein *event_declaration* -Objekt, das sowohl den `abstract`-Modifizierer als auch das mit geschweiften Klammern getrennte *event_accessor_declarations*einschließt.

Wenn eine Ereignis Deklaration einen `extern` Modifizierer enthält, wird das Ereignis als ***externes Ereignis***bezeichnet. Da eine externe Ereignis Deklaration keine tatsächliche Implementierung bereitstellt, ist es ein Fehler, dass Sie sowohl den `extern`-Modifizierer als auch *event_accessor_declarations*einschließen.

Es handelt sich um einen Kompilierzeitfehler für eine *variable_declarator* einer Ereignis Deklaration mit einem `abstract`-oder `external`-Modifizierer, um ein *variable_initializer*-Zeichen einzuschließen.

Ein Ereignis kann als Linker Operand des `+=` Operators und `-=` ([Ereignis Zuweisung](expressions.md#event-assignment)) verwendet werden. Diese Operatoren werden zum Anfügen von Ereignis Handlern an oder zum Entfernen von Ereignis Handlern aus einem Ereignis verwendet, und die Zugriffsmodifizierer des Ereignisses steuern die Kontexte, in denen solche Vorgänge zulässig sind.

Da `+=` und`-=` die einzigen Vorgänge sind, die für ein Ereignis außerhalb des Typs zulässig sind, der das Ereignis deklariert, kann externer Code Handler für ein Ereignis hinzufügen und entfernen, aber nicht auf andere Weise die zugrunde liegende Ereignisliste abrufen oder ändern. Handler.

Bei einem Vorgang des Formulars `x += y` oder `x -= y`, wenn `x` ein Ereignis und der Verweis außerhalb des Typs, der die Deklaration von `x`enthält, erfolgt, hat das Ergebnis des Vorgangs den Typ `void` (im Gegensatz zu der Typ von `x`mit dem Wert von `x` nach der Zuweisung). Diese Regel verhindert, dass externer Code indirekt den zugrunde liegenden Delegaten eines Ereignisses untersucht.

Das folgende Beispiel zeigt, wie Ereignishandler an Instanzen der `Button` -Klasse angefügt werden:
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

Hier erstellt der `LoginDialog` Instanzkonstruktor zwei `Button` -Instanzen und fügt Ereignishandler an die `Click` -Ereignisse an.

### <a name="field-like-events"></a>Feld ähnliche Ereignisse

Im Programmtext der Klasse oder Struktur, die die Deklaration eines Ereignisses enthält, können bestimmte Ereignisse wie Felder verwendet werden. Um auf diese Weise verwendet zu werden, darf ein Ereignis nicht `abstract` oder `extern` sein und darf *event_accessor_declarations*nicht explizit enthalten. Ein derartiges Ereignis kann in jedem Kontext verwendet werden, der ein Feld zulässt. Das-Feld enthält einen Delegaten[(](delegates.md)Delegaten), der auf die Liste der Ereignishandler verweist, die dem-Ereignis hinzugefügt wurden. Wenn keine Ereignishandler hinzugefügt wurden, enthält `null`das Feld.

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
`Click`wird als Feld in der `Button` -Klasse verwendet. Wie das Beispiel zeigt, kann das-Feld überprüft, geändert und in Delegataufrufausdrücken verwendet werden. Die `OnClick` -Methode in `Button` der-Klasse löst das `Click` -Ereignis aus. Das Auslösen eines Ereignisses entspricht exakt dem Aufrufen des Delegaten, der durch das Ereignis repräsentiert wird, es gibt deshalb keine besonderen Sprachkonstrukte zum Auslösen von Ereignissen. Beachten Sie, dass dem Delegataufruf eine Prüfung vorangestellt ist, die sicherstellt, dass der Delegat nicht NULL ist.

Außerhalb der Deklaration `Button` der `+=` -Klasse kann `Click` der Member nur auf der linken Seite der Operatoren und `-=` verwendet werden, wie in
```csharp
b.Click += new EventHandler(...);
```
, der einen Delegaten an die Aufruf Liste des `Click` Ereignisses anfügt, und
```csharp
b.Click -= new EventHandler(...);
```
entfernt einen Delegaten aus der Aufruf Liste des `Click` Ereignisses.

Beim Kompilieren eines Feld ähnlichen Ereignisses erstellt der Compiler automatisch Speicher, um den Delegaten aufzunehmen, und erstellt Accessoren für das Ereignis, mit dem Ereignishandler zum Delegatfeld hinzugefügt oder daraus entfernt werden. Die hinzu Füge-und Entfernungs Vorgänge sind Thread sicher und können (jedoch nicht erforderlich) ausgeführt werden, während die Sperre ([lock-Anweisung](statements.md#the-lock-statement)) für das enthaltende Objekt für ein Instanzereignis oder das Typobjekt ([Anonyme Objekt Erstellungs Ausdrücke](expressions.md#anonymous-object-creation-expressions)) aufrechterhalten werden. für ein statisches Ereignis.

Daher ist eine Instanzereignisdeklaration der folgenden Form:
```csharp
class X
{
    public event D Ev;
}
```
wird in eine entsprechende kompiliert:
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
In der `+=` - `X`Klasse bewirken Verweise `Ev` auf auf der linken Seite der Operatoren und `-=` , dass die Add-und remove-Accessoren aufgerufen werden. Alle anderen Verweise auf `Ev` werden kompiliert, um stattdessen auf das `__Ev` ausgeblendete Feld zu verweisen ([Member Access](expressions.md#member-access)). Der Name "`__Ev`" ist willkürlich; das ausgeblendete Feld kann einen beliebigen Namen oder keinen Namen haben.

### <a name="event-accessors"></a>Ereignisaccessoren

Ereignis Deklarationen lassen *event_accessor_declarations*üblicherweise aus, wie im obigen Beispiel `Button`. Eine Situation hierfür ist der Fall, in dem die Speicherkosten eines Felds pro Ereignis nicht zulässig sind. In solchen Fällen kann eine Klasse *event_accessor_declarations* einschließen und einen privaten Mechanismus zum Speichern der Liste von Ereignis Handlern verwenden.

Der *event_accessor_declarations* eines Ereignisses gibt die ausführbaren Anweisungen an, die dem Hinzufügen und Entfernen von Ereignis Handlern zugeordnet sind.

Die Accessordeklarationen bestehen aus einem *add_accessor_declaration* -und einem *remove_accessor_declaration*-Operator. Jede Accessordeklaration besteht aus dem `add` Token `remove` oder gefolgt von einem- *Block*. Der einem *add_accessor_declaration* zugeordnete *Block* gibt die Anweisungen an, die beim Hinzufügen eines Ereignis Handlers ausgeführt werden sollen, und der mit einem *remove_accessor_declaration* -Block verknüpfte *Block* gibt die auszuführenden Anweisungen an. Wenn ein Ereignishandler entfernt wird.

Jede *add_accessor_declaration* und *remove_accessor_declaration* entspricht einer Methode mit einem einzelnen Wert Parameter des Ereignis Typs und einem `void`-Rückgabetyp. Der implizite Parameter eines Ereignis Accessors heißt `value`. Wenn ein Ereignis in einer Ereignis Zuweisung verwendet wird, wird der entsprechende Ereignis Accessor verwendet. Insbesondere, wenn der Zuweisungs Operator `+=` ist, wird der Add-Accessor verwendet, und wenn der Zuweisungs Operator ist `-=` , wird der remove-Accessor verwendet. In beiden Fällen wird der rechte Operand des Zuweisungs Operators als Argument für den Ereignis Accessor verwendet. Der-Block eines *add_accessor_declaration* -oder *remove_accessor_declaration* -muss den Regeln für `void`-Methoden entsprechen, die im [Methoden Text](classes.md#method-body)beschrieben werden. Insbesondere `return` -Anweisungen in einem solchen Block dürfen keinen Ausdruck angeben.

Da ein Ereignis Accessor implizit einen Parameter mit dem `value`Namen aufweist, handelt es sich um einen Kompilierzeitfehler für eine lokale Variable oder Konstante, die in einem Ereignis Accessor deklariert wurde, um diesen Namen zu haben.

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
die `Control` -Klasse implementiert einen internen Speichermechanismus für-Ereignisse. Die `AddEventHandler` -Methode ordnet einem Schlüssel einen Delegatwert zu `GetEventHandler` , die Methode gibt den derzeit einem Schlüssel zugeordneten Delegaten `RemoveEventHandler` zurück, und die Methode entfernt einen Delegaten als Ereignishandler für das angegebene Ereignis. Vermutlich ist der zugrunde liegende Speichermechanismus so konzipiert, dass es keine Kosten für das Zuordnen eines `null` Delegatwerts zu einem Schlüssel gibt. daher verbrauchen nicht behandelte Ereignisse keinen Speicherplatz.

### <a name="static-and-instance-events"></a>Statische Ereignisse und Instanzereignisse

Wenn eine Ereignis Deklaration einen `static` Modifizierer enthält, wird das Ereignis als ***statisches Ereignis***bezeichnet. Wenn kein `static` Modifizierer vorhanden ist, wird das Ereignis als ***Instanzereignis***bezeichnet.

Ein statisches Ereignis ist nicht mit einer bestimmten Instanz verknüpft, und es ist ein Kompilierzeitfehler, auf `this` den in den Accessoren eines statischen Ereignisses verwiesen wird.

Ein Instanzereignis ist einer bestimmten Instanz einer Klasse zugeordnet, und auf diese Instanz kann in den Accessoren dieses Ereignisses als `this` ([dieser Zugriff](expressions.md#this-access)) zugegriffen werden.

Wenn auf ein Ereignis in einem *member_access* -Element ([Member Access](expressions.md#member-access)) der Form `E.M` verwiesen wird, muss, wenn `M` ein statisches Ereignis ist, `E` einen Typ angeben, der `M` enthält, und wenn `M` ein Instanzereignis ist, muss E eine Instanz eines Typs angeben, der enthält. `M`.

Die Unterschiede zwischen statischen und Instanzmembern werden in [statischen und Instanzmembern](classes.md#static-and-instance-members)ausführlicher erläutert.

### <a name="virtual-sealed-override-and-abstract-event-accessors"></a>Virtual-, sealed-, override-und Abstract-Ereignisaccessoren

Eine `virtual` -Ereignis Deklaration gibt an, dass die Accessoren dieses Ereignisses virtuell sind. Der `virtual` -Modifizierer gilt für beide Accessoren eines Ereignisses.

Eine `abstract` Ereignis Deklaration gibt an, dass die Accessoren des Ereignisses virtuell sind, aber keine tatsächliche Implementierung der Accessoren bereitstellt. Stattdessen sind nicht abstrakte abgeleitete Klassen erforderlich, um eine eigene Implementierung für die Accessoren bereitzustellen, indem das-Ereignis überschrieben wird. Da eine abstrakte Ereignis Deklaration keine tatsächliche Implementierung bereitstellt, kann Sie keine durch Klammern getrennte *event_accessor_declarations*bereitstellen.

Eine Ereignis Deklaration, die den `abstract` - `override` Modifizierer und den-Modifizierer enthält, gibt an, dass das Ereignis abstrakt ist, und überschreibt ein Die Accessoren eines solchen Ereignisses sind ebenfalls abstrakt.

Abstrakte Ereignis Deklarationen sind nur in abstrakten Klassen zulässig ([abstrakte Klassen](classes.md#abstract-classes)).

Die Accessoren eines geerbten virtuellen Ereignisses können durch Einschließen einer Ereignis Deklaration, die einen `override` Modifizierer angibt, in einer abgeleiteten Klasse überschrieben werden. Dies wird als über schreibende ***Ereignis Deklaration***bezeichnet. Eine über schreibende Ereignis Deklaration deklariert kein neues Ereignis. Stattdessen werden lediglich die Implementierungen der Accessoren eines vorhandenen virtuellen Ereignisses spezialisiert.

Eine über schreibende Ereignis Deklaration muss genau dieselben Zugriffsmodifizierer, denselben Typ und denselben Namen wie das überschriebene Ereignis angeben.

Eine über schreibende Ereignis Deklaration `sealed` kann den-Modifizierer enthalten. Durch die Verwendung dieses Modifizierers wird verhindert, dass eine abgeleitete Klasse das Ereignis weiter überschreibt. Die Accessoren eines versiegelten Ereignisses werden ebenfalls versiegelt.

Es ist ein Kompilierzeitfehler, wenn eine über schreibende Ereignis Deklaration einen `new` -Modifizierer einschließt.

Mit Ausnahme der Unterschiede in der Deklaration und der Aufruf Syntax Verhalten sich virtuelle, versiegelte, Überschreibungs-und abstrakte Accessoren genauso wie virtuelle, versiegelte, Überschreibungs-und abstrakte Methoden. Insbesondere gelten die in [Virtual Methods](classes.md#virtual-methods), [override Methods](classes.md#override-methods), [sealed Methods](classes.md#sealed-methods)und [abstract Methods](classes.md#abstract-methods) beschriebenen Regeln so, als wären Accessoren Methoden einer entsprechenden Form. Jeder-Accessor entspricht einer Methode mit einem einzelnen value-Parameter des Ereignis Typs, einem `void` Rückgabetyp und denselben Modifiziererwerten wie das enthaltende Ereignis.

## <a name="indexers"></a>Indexer

Ein ***Indexer*** ist ein Member, mit dem ein Objekt auf die gleiche Weise wie ein Array indiziert werden kann. Indexer werden mithilfe von *indexer_declaration*s deklariert:

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

Ein *indexer_declaration* kann einen Satz von *Attributen* ([Attribute](attributes.md)) und eine gültige Kombination der vier Zugriffsmodifizierer ([Zugriffsmodifizierer](classes.md#access-modifiers)), den `new` ([den neuen Modifizierer](classes.md#the-new-modifier)), `virtual` ([virtuelle Methoden) enthalten. ](classes.md#virtual-methods)), `override` ([Überschreibungs Methoden](classes.md#override-methods)), `sealed` ([versiegelte Methoden](classes.md#sealed-methods)), `abstract` ([abstrakte Methoden](classes.md#abstract-methods)) und `extern` ([Externe Methoden](classes.md#external-methods))-Modifizierer.

Indexer-Deklarationen unterliegen den gleichen Regeln wie Methoden Deklarationen ([Methoden](classes.md#methods)) in Bezug auf gültige Kombinationen von Modifizierern, wobei die einzige Ausnahme darin besteht, dass der statische Modifizierer in einer Indexer-Deklaration nicht zulässig ist.

Die `virtual`modifiziererer `override`, und `abstract` schließen sich gegenseitig aus, außer in einem Fall. Die `abstract` Modifizierer und `override` können zusammen verwendet werden, damit ein abstrakter Indexer einen virtuellen überschreiben kann.

Der *Typ* einer Indexer-Deklaration gibt den Elementtyp des Indexers an, der von der Deklaration eingeführt wird. Es *sei denn* , der Indexer ist eine explizite Schnittstellenmember-Implementierung, gefolgt vom `this`-Schlüsselwort. Für eine explizite Implementierung des Schnittstellenmembers folgt der *Typ* *INTERFACE_TYPE*, a "`.`" und das Schlüsselwort `this`. Im Gegensatz zu anderen Membern haben Indexer keine benutzerdefinierten Namen.

*Formal_parameter_list* gibt die Parameter des Indexers an. Die Liste formaler Parameter eines Indexers entspricht der einer Methode ([Methoden Parameter](classes.md#method-parameters)), mit dem Unterschied, dass mindestens ein Parameter angegeben werden muss und dass die `ref` - `out` und-Parametermodifizierer nicht zulässig sind.

Der *Typ* eines Indexers und alle Typen, auf die in *formal_parameter_list* verwiesen wird, müssen mindestens so zugänglich sein wie der Indexer selbst ([Barrierefreiheits Einschränkungen](basic-concepts.md#accessibility-constraints)).

Ein *indexer_body* kann entweder aus einem ***Accessor-Text*** oder einem ***Ausdrucks Körper***bestehen. In einem Accessor-Text muss *accessor_declarations*, der in "`{`" und "`}`"-Token eingeschlossen werden muss, die Accessoren ([Accessoren](classes.md#accessors)) der Eigenschaft deklarieren. Die Accessoren geben die ausführbaren Anweisungen an, die dem Lesen und Schreiben der Eigenschaft zugeordnet sind.

Ein Ausdrucks Text, der aus`=>`"" gefolgt von einem `E` Ausdruck und einem Semikolon besteht, entspricht exakt dem `{ get { return E; } }`Anweisungs Text und kann daher nur zum Angeben von nur-Getter-indexatoren verwendet werden, bei denen das Ergebnis des Getters wird von einem einzelnen Ausdruck angegeben.

Obwohl die Syntax für den Zugriff auf ein Indexer-Element mit der Syntax für ein Array Element identisch ist, wird ein Indexer-Element nicht als Variable klassifiziert. Folglich ist es nicht möglich, ein Indexer-Element als `ref` -oder `out` -Argument zu übergeben.

Die Liste der formalen Parameter eines Indexers definiert die Signatur ([Signaturen und überladen](basic-concepts.md#signatures-and-overloading)) des Indexers. Insbesondere besteht die Signatur eines Indexers aus der Anzahl und den Typen der formalen Parameter. Der Elementtyp und die Namen der formalen Parameter sind nicht Teil der Signatur eines Indexers.

Die Signatur eines Indexer muss sich von den Signaturen aller anderen Indexer unterscheiden, die in derselben Klasse deklariert sind.

Indexer und Eigenschaften sind in der Konzeption sehr ähnlich, unterscheiden sich jedoch wie folgt:

*  Eine Eigenschaft wird anhand ihres Namens identifiziert, während ein Indexer durch die Signatur identifiziert wird.
*  Der Zugriff auf eine Eigenschaft erfolgt über einen *Simple_name* ([simple names](expressions.md#simple-names)) oder einen *member_access* ([Member Access](expressions.md#member-access)), während auf ein Indexer-Element über einen *element_access* ([Indexer-Zugriff](expressions.md#indexer-access)) zugegriffen wird.
*  Eine Eigenschaft kann ein `static` Member sein, während ein Indexer immer ein Instanzmember ist.
*  Ein `get` -Accessor einer Eigenschaft entspricht einer Methode ohne Parameter, während ein `get` -Accessor eines Indexers einer Methode mit derselben formalen Parameterliste wie der Indexer entspricht.
*  Ein `set` -Accessor einer Eigenschaft entspricht einer Methode mit einem einzelnen Parameter mit dem `value`Namen, während `set` ein Accessor eines Indexers einer Methode mit derselben formalen Parameterliste wie der Indexer entspricht, zuzüglich eines zusätzlichen Parameters. mit `value`dem Namen.
*  Es ist ein Kompilierzeitfehler für einen Indexer-Accessor, eine lokale Variable mit dem gleichen Namen wie ein Indexer-Parameter zu deklarieren.
*  In einer über schreibenden Eigenschaften Deklaration wird auf die geerbte Eigenschaft `base.P`mithilfe der `P` -Syntax zugegriffen, wobei der Eigenschaftsname ist. In einer über schreibenden Indexer-Deklaration wird auf den geerbten Indexer mithilfe `E` der-Syntax `base[E]`zugegriffen, wobei eine durch Kommas getrennte Liste von Ausdrücken ist.
*  Es gibt kein Konzept für einen "automatisch implementierten Indexer". Es ist ein Fehler, wenn ein nicht abstrakter, nicht externer Indexer mit Semikolon-Accessoren vorliegt.

Abgesehen von diesen Unterschieden gelten alle in [Accessoren](classes.md#accessors) und [automatisch implementierten Eigenschaften](classes.md#automatically-implemented-properties) definierten Regeln sowohl für Indexer-Accessoren als auch für Eigenschaftenaccessoren.

Wenn eine Indexer-Deklaration `extern` einen Modifizierer enthält, wird der Indexer als ***externer Indexer***bezeichnet. Da eine externe Indexer-Deklaration keine tatsächliche Implementierung bereitstellt, besteht jede Ihrer *accessor_declarations* aus einem Semikolon.

Im folgenden Beispiel wird eine `BitArray` Klasse deklariert, die einen Indexer für den Zugriff auf die einzelnen Bits im BitArray implementiert.
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

Eine Instanz der `BitArray` -Klasse beansprucht wesentlich weniger Arbeitsspeicher als eine `bool[]` entsprechende (da jeder Wert des ersten-Werts nur ein Bit und nicht das zweite Byte) beansprucht, aber die gleichen Vorgänge wie eine `bool[]`zulässt.

Die folgende `CountPrimes` Klasse verwendet einen `BitArray` und den klassischen "Sieve"-Algorithmus, um die Anzahl von PRIMES zwischen 1 und einem angegebenen Maximum zu berechnen:
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

Beachten Sie, dass die Syntax für den Zugriff `BitArray` auf Elemente von exakt der gleiche ist `bool[]`wie für ein.

Im folgenden Beispiel wird eine 26 * 10-Raster Klasse gezeigt, die über einen Indexer mit zwei Parametern verfügt. Der erste Parameter muss ein groß-oder Kleinbuchstabe im Bereich A-Z sein, und der zweite Parameter muss eine ganze Zahl im Bereich 0-9 sein.

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

### <a name="indexer-overloading"></a>Überladen von Indexern

Die Regeln der Indexer-Überladungs Auflösung werden unter [Typrückschluss](expressions.md#type-inference)beschrieben.

## <a name="operators"></a>Operatoren

Ein ***Operator*** ist ein Member, der die Bedeutung eines Ausdrucks Operators definiert, der auf Instanzen der Klasse angewendet werden kann. Operatoren werden mit *operator_declaration*s deklariert:

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

Es gibt drei Kategorien von über ladbaren Operatoren: Unäre Operatoren ([unäre Operatoren](classes.md#unary-operators)), binäre Operatoren ([binäre Operatoren](classes.md#binary-operators)) und Konvertierungs Operatoren ([Konvertierungs Operatoren](classes.md#conversion-operators)).

Der *operator_body* ist entweder ein Semikolon, ein ***Anweisungs Text*** oder ein ***Ausdrucks Körper***. Ein Anweisungs Text besteht aus einem- *Block*, der die auszuführenden Anweisungen angibt, wenn der Operator aufgerufen wird. Der- *Block* muss den Regeln für Rückgabe Methoden entsprechen, die im [Methoden Text](classes.md#method-body)beschrieben werden. Ein Ausdrucks Körper besteht aus `=>` , gefolgt von einem Ausdruck und einem Semikolon und deutet auf einen einzelnen Ausdruck hin, der beim Aufrufen des Operators ausgeführt werden soll.

Bei `extern`-Operatoren besteht das *operator_body* einfach aus einem Semikolon. Für alle anderen Operatoren ist *operator_body* entweder ein Block Körper oder ein Ausdrucks Körper.

Die folgenden Regeln gelten für alle Operator Deklarationen:

*  Eine Operator Deklaration muss sowohl einen `public` -als `static` auch einen-Modifizierer enthalten.
*  Die Parameter eines Operators müssen Wert Parameter ([value](variables.md#value-parameters)-Parameter) sein. Es handelt sich um einen Kompilierzeitfehler für eine Operator Deklaration `out` zum angeben `ref` von-oder-Parametern.
*  Die Signatur eines Operators ([unäre Operatoren](classes.md#unary-operators), [binäre Operatoren](classes.md#binary-operators)und [Konvertierungs Operatoren](classes.md#conversion-operators)) muss sich von den Signaturen aller anderen Operatoren unterscheiden, die in derselben Klasse deklariert sind.
*  Alle Typen, auf die in einer Operator Deklaration verwiesen wird, müssen mindestens so zugänglich sein wie der Operator selbst ([Barrierefreiheits Einschränkungen](basic-concepts.md#accessibility-constraints)).
*  Es ist ein Fehler, dass derselbe Modifizierer mehrmals in einer Operator Deklaration angezeigt wird.

Jede Operator Kategorie erzwingt zusätzliche Einschränkungen, wie in den folgenden Abschnitten beschrieben.

Wie bei anderen Membern werden Operatoren, die in einer Basisklasse deklariert werden, von abgeleiteten Klassen geerbt. Da Operator Deklarationen immer die Klasse oder Struktur erfordern, in der der Operator deklariert ist, um an der Signatur des Operators teilzunehmen, ist es nicht möglich, dass ein Operator, der in einer abgeleiteten Klasse deklariert ist, einen in einer Basisklasse deklarierten Operator ausblenden kann. Daher ist der `new` -Modifizierer in einer Operator Deklaration nie erforderlich und daher nie zulässig.

Weitere Informationen zu unären und binären Operatoren finden Sie unter [Operatoren](expressions.md#operators).

Weitere Informationen zu Konvertierungs Operatoren finden Sie unter [benutzerdefinierte Konvertierungen](conversions.md#user-defined-conversions).

### <a name="unary-operators"></a>Unäre Operatoren

Die folgenden Regeln gelten für unäre Operator Deklarationen, `T` wobei den Instanztyp der Klasse oder Struktur bezeichnet, die die Operator Deklaration enthält:

*  Ein unärer `+`Operator `-`, `!`, oder `~` muss einen einzelnen Parameter vom Typ `T` oder `T?` annehmen und kann jeden beliebigen Typ zurückgeben.
*  Ein unärer `++` or `--` -Operator muss einen einzelnen Parameter vom Typ `T` oder `T?` annehmen, und er muss denselben Typ oder einen von ihm abgeleiteten Typ zurückgeben.
*  Ein unärer `true` or `false` -Operator muss einen einzelnen Parameter vom Typ `T` oder `T?` annehmen und muss den `bool`Typ zurückgeben.

Die Signatur eines unären Operators besteht aus dem Operator Token (`+`, `-`, `!`, `~`, `++`, `--`, `true`oder `false`) und dem Typ des einzelnen formalen Parameters. Der Rückgabetyp ist weder Teil der Signatur eines unären Operators noch der Name des formalen Parameters.

Die `true` unären Operatoren und `false` erfordern eine paarweise Deklaration. Ein Kompilierzeitfehler tritt auf, wenn eine Klasse einen dieser Operatoren deklariert, ohne auch den anderen zu deklarieren. Die `true` Operatoren und `false` werden weiter unten in [benutzerdefinierten bedingten logischen Operatoren](expressions.md#user-defined-conditional-logical-operators) und [booleschen Ausdrücken](expressions.md#boolean-expressions)beschrieben.

Das folgende Beispiel zeigt eine-Implementierung und die nachfolg `operator ++` Ende Verwendung von für eine ganzzahlige Vektor Klasse:
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

Beachten Sie, dass die Operator-Methode den Wert zurückgibt, der durch das Hinzufügen von 1 zum Operanden erzeugt wird, genau wie die Postfix-Inkrement-und Dekrementoperatoren ([postfix-Inkrement-und Dekrementoperatoren](expressions.md#postfix-increment-and-decrement-operators)) und die Präfix-Inkrement-[ Inkrement-und Dekrementoperatoren](expressions.md#prefix-increment-and-decrement-operators)). Anders als C++in muss diese Methode den Wert des Operanden nicht direkt ändern. Tatsächlich würde das Ändern des Operanden-Werts gegen die Standard Semantik des Postfix-Inkrementoperators verstoßen.

### <a name="binary-operators"></a>Binäre Operatoren

Die folgenden Regeln gelten für binäre Operator Deklarationen, `T` wobei den Instanztyp der Klasse oder Struktur bezeichnet, die die Operator Deklaration enthält:

*  Ein binärer nicht Verschiebungs Operator muss zwei Parameter annehmen, von denen mindestens eine den Typ `T` oder `T?`aufweisen muss und jeden beliebigen Typ zurückgeben kann.
*  Ein `<<` binärer `>>` or `T?` -Operator muss zwei Parameter annehmen. der erste muss den- `T` Typ aufweisen, und der zweite muss den- `int` Typ `int?`oder aufweisen und jeden beliebigen Typ zurückgeben.

Die Signatur`+`eines binären Operators besteht aus dem Operator Token (, `<<` `|` `*` `&` `/` `-`,,, `%`,,, `^`,, `>>`, `==`, ,,`>`, oder`>=`) unddieTypenderbeidenformalenParameter`<=`. `!=` `<` Der Rückgabetyp und die Namen der formalen Parameter sind nicht Teil der Signatur eines binären Operators.

Bestimmte binäre Operatoren erfordern eine paarweise Deklaration. Für jede Deklaration eines der beiden Operatoren eines Paares muss eine entsprechende Deklaration des anderen Operators des Paars vorhanden sein. Zwei Operator Deklarationen stimmen überein, wenn Sie den gleichen Rückgabetyp und denselben Typ für jeden Parameter aufweisen. Die folgenden Operatoren erfordern eine paarweise Deklaration:

*  `operator ==` und `operator !=`
*  `operator >` und `operator <`
*  `operator >=` und `operator <=`

### <a name="conversion-operators"></a>Konvertierungsoperatoren

Eine Konvertierungs Operator Deklaration führt eine ***benutzerdefinierte Konvertierung*** ([benutzerdefinierte Konvertierungen](conversions.md#user-defined-conversions)) ein, mit der die vordefinierten und expliziten Konvertierungen erweitert werden.

Eine Konvertierungs Operator Deklaration, `implicit` die das-Schlüsselwort enthält, führt eine benutzerdefinierte implizite Konvertierung ein. Implizite Konvertierungen können in einer Vielzahl von Situationen auftreten, einschließlich Funktionsmember-Aufrufe, Umwandlungs Ausdrücke und Zuweisungen. Dies wird in [implizite Konvertierungen](conversions.md#implicit-conversions)beschrieben.

Eine Konvertierungs Operator Deklaration, `explicit` die das-Schlüsselwort enthält, führt eine benutzerdefinierte explizite Konvertierung ein. Explizite Konvertierungen können in Umwandlungs Ausdrücken auftreten und werden weiter unten in [expliziten Konvertierungen](conversions.md#explicit-conversions)beschrieben.

Ein Konvertierungs Operator konvertiert von einem Quelltyp, der durch den Parametertyp des Konvertierungs Operators angegeben ist, in einen Zieltyp, der durch den Rückgabetyp des Konvertierungs Operators angegeben wird.

Verwenden Sie für einen angegebenen `S` Quelltyp und `T`Zieltyp `S` , `T` wenn oder NULL-Werte zulassen `S0` `T0` , `T0` die zugrunde liegenden Typen, und verweisen `S0` Sie andernfalls auf. `S` gleich`T` bzw. Eine Klasse oder Struktur darf nur dann eine Konvertierung von einem Quelltyp `S` in einen Zieltyp `T` deklarieren, wenn Folgendes zutrifft:

*  `S0`und `T0` sind unterschiedliche Typen.
*  Entweder `S0` oder`T0` ist der Klassen-oder Strukturtyp, in dem die Operator Deklaration stattfindet.
*  Weder `S0` noch `T0` ist ein *INTERFACE_TYPE*-Wert.
*  Ohne Benutzer `S` definierte Konvertierungen ist eine Konvertierung von zu `T` oder von `T` zu `S`nicht vorhanden.

Bei diesen Regeln werden alle Typparameter, die mit `S` oder `T` verknüpft sind, als eindeutige Typen betrachtet, die keine Vererbungs Beziehung mit anderen Typen aufweisen, und Einschränkungen für diese Typparameter werden ignoriert.

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
die ersten beiden Operator Deklarationen sind zulässig, da für `T` [Indexer-Indexer](classes.md#indexers), und `int` `string` bzw. als eindeutige Typen ohne Beziehung angesehen werden. Der dritte Operator ist jedoch ein Fehler, da `C<T>` die Basisklasse von `D<T>`ist.

Aus der zweiten Regel folgt, dass ein Konvertierungs Operator entweder in oder aus dem Klassen-oder Strukturtyp konvertieren muss, in dem der Operator deklariert ist. Beispielsweise ist es möglich, dass ein Klassen- `C` oder Strukturtyp eine Konvertierung von `C` in `int` und von `int` in `C`, aber nicht von `int` in `bool`definiert.

Es ist nicht möglich, eine vordefinierte Konvertierung direkt neu zu definieren. Folglich ist es nicht zulässig, Konvertierungs Operatoren von oder `object` in zu konvertieren, da zwischen `object` und allen anderen Typen bereits implizite und explizite Konvertierungen vorhanden sind. Ebenso kann weder die Quelle noch die Zieltypen einer Konvertierung ein Basistyp der anderen sein, da eine Konvertierung dann bereits vorhanden wäre.

Es ist jedoch möglich, Operatoren für generische Typen zu deklarieren, die für bestimmte Typargumente Konvertierungen angeben, die bereits als vordefinierte Konvertierungen vorhanden sind. Im Beispiel
```csharp
struct Convertible<T>
{
    public static implicit operator Convertible<T>(T value) {...}
    public static explicit operator T(Convertible<T> value) {...}
}
```
Wenn Type `object` als Typargument für `T`angegeben wird, deklariert der zweite Operator eine Konvertierung, die bereits vorhanden ist (ein implizites und somit auch eine explizite Konvertierung von einem beliebigen `object`Typ in den Typ).

In Fällen, in denen eine vordefinierte Konvertierung zwischen zwei Typen vorhanden ist, werden alle benutzerdefinierten Konvertierungen zwischen diesen Typen ignoriert. Dies gilt insbesondere in folgenden Fällen:

*  Wenn eine vordefinierte implizite Konvertierung ([implizite Konvertierungen](conversions.md#implicit-conversions) `S` ) vom Typ in den Typ `T`vorhanden ist, werden alle benutzerdefinierten Konvertierungen (implizit oder explizit `T` ) von `S` zu ignoriert.
*  Wenn eine vordefinierte explizite Konvertierung ([explizite Konvertierungen](conversions.md#explicit-conversions) `S` ) vom Typ in den Typ `T`vorhanden ist, werden alle benutzerdefinierten expliziten `T` Konvertierungen von `S` in ignoriert. Weiter

Wenn `T` ein Schnittstellentyp ist, werden benutzerdefinierte implizite Konvertierungen `T` von `S` in ignoriert.

Andernfalls werden benutzerdefinierte implizite Konvertierungen von `S` in `T` immer noch berücksichtigt.

Für alle Typen `object`, jedoch verursachen die `Convertible<T>` vom oben genannten Typ deklarierten Operatoren keinen Konflikt mit vordefinierten Konvertierungen. Zum Beispiel:
```csharp
void F(int i, Convertible<int> n) {
    i = n;                          // Error
    i = (int)n;                     // User-defined explicit conversion
    n = i;                          // User-defined implicit conversion
    n = (Convertible<int>)i;        // User-defined implicit conversion
}
```

Für den Typ `object`verbergen vordefinierte Konvertierungen jedoch die benutzerdefinierten Konvertierungen in allen Fällen, aber eine:

```csharp
void F(object o, Convertible<object> n) {
    o = n;                         // Pre-defined boxing conversion
    o = (object)n;                 // Pre-defined boxing conversion
    n = o;                         // User-defined implicit conversion
    n = (Convertible<object>)o;    // Pre-defined unboxing conversion
}
```

Benutzerdefinierte Konvertierungen dürfen nicht von oder in *INTERFACE_TYPE*s konvertiert werden. Diese Einschränkung stellt insbesondere sicher, dass keine benutzerdefinierten Transformationen beim Konvertieren in ein *INTERFACE_TYPE*auftreten und dass eine Konvertierung in ein *INTERFACE_TYPE* nur erfolgreich ist, wenn das konvertierte Objekt tatsächlich das angegebene *INTERFACE_TYPE*.

Die Signatur eines Konvertierungs Operators besteht aus dem Quelltyp und dem Zieltyp. (Beachten Sie, dass dies die einzige Form der Member ist, für die der Rückgabetyp an der Signatur teilnimmt.) Die `implicit` - `explicit` oder-Klassifizierung eines Konvertierungs Operators ist nicht Teil der Signatur des Operators. Daher kann eine Klasse oder Struktur nicht sowohl einen `implicit` -als auch einen `explicit` -Konvertierungs Operator mit denselben Quell-und Zieltypen deklarieren.

Im Allgemeinen sollten benutzerdefinierte implizite Konvertierungen so entworfen werden, dass Sie niemals Ausnahmen auslösen und nie Informationen verlieren. Wenn eine benutzerdefinierte Konvertierung Ausnahmen auslösen kann (z. b. weil das Quell Argument außerhalb des gültigen Bereichs liegt) oder Informationen verloren geht (z. b. das Verwerfen von großen Bits), sollte diese Konvertierung als explizite Konvertierung definiert werden.

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
die Konvertierung von `Digit` in `byte` ist implizit, weil Sie niemals Ausnahmen auslöst oder Informationen verliert, aber die Konvertierung `byte` von `Digit` in ist explizit `Digit` , da nur eine Teilmenge der möglichen Werte einer `byte`.

## <a name="instance-constructors"></a>Instanzkonstruktoren

Ein ***Instanzkonstruktor*** ist ein Member, der die erforderlichen Aktionen zum Initialisieren einer Instanz einer Klasse implementiert. Instanzkonstruktoren werden mit *constructor_declaration*s deklariert:

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

Ein *constructor_declaration* kann einen Satz von *Attributen* ([Attribute](attributes.md)), eine gültige Kombination der vier Zugriffsmodifizierer ([Zugriffsmodifizierer](classes.md#access-modifiers)) und einen `extern`-Modifizierer ([Externe Methoden](classes.md#external-methods)) enthalten. Eine Konstruktordeklaration darf nicht denselben Modifizierer mehrmals einschließen.

Der *Bezeichner* eines *constructor_declarator* muss der Klasse, in der der Instanzkonstruktor deklariert ist, einen Namen benennen. Wenn ein anderer Name angegeben wird, tritt ein Kompilierzeitfehler auf.

Der optionale *formal_parameter_list* eines Instanzkonstruktors unterliegt den gleichen Regeln wie das *formal_parameter_list* einer Methode ([Methoden](classes.md#methods)). Die Liste formaler Parameter definiert die Signatur ([Signaturen und überladen](basic-concepts.md#signatures-and-overloading)) eines Instanzkonstruktors und steuert den Prozess, bei dem die Überladungs Auflösung ([Typrückschluss](expressions.md#type-inference)) einen bestimmten Instanzkonstruktor in einem Aufruf auswählt.

Auf jeden der Typen, auf die in der *formal_parameter_list* eines Instanzkonstruktors verwiesen wird, muss mindestens der Zugriff auf den Konstruktor selbst (Barrierefreiheits[Einschränkungen](basic-concepts.md#accessibility-constraints)) möglich sein.

Der optionale *constructor_initializer* -Konstruktor gibt einen anderen Instanzkonstruktor an, der vor der Ausführung der Anweisungen im *constructor_body* dieses Instanzkonstruktors aufgerufen werden soll. Dies wird in [konstruktorinitialisierern](classes.md#constructor-initializers)ausführlicher beschrieben.

Wenn eine Konstruktordeklaration einen `extern` Modifizierer enthält, wird der Konstruktor als ***externer Konstruktor***bezeichnet. Da eine externe Konstruktordeklaration keine tatsächliche Implementierung bereitstellt, besteht deren *constructor_body* aus einem Semikolon. Für alle anderen Konstruktoren besteht der *constructor_body* aus einem- *Block* , der die Anweisungen zum Initialisieren einer neuen Instanz der-Klasse angibt. Dies entspricht exakt dem *Block* einer Instanzmethode mit einem `void` Rückgabetyp ([Methoden Text](classes.md#method-body)).

Instanzkonstruktoren werden nicht geerbt. Folglich hat eine Klasse keine Instanzkonstruktoren, die nicht tatsächlich in der Klasse deklariert sind. Wenn eine Klasse keine Instanzkonstruktordeklarationen enthält, wird automatisch ein Standardinstanzkonstruktor bereitgestellt ([Standardkonstruktoren](classes.md#default-constructors)).

Instanzkonstruktoren werden von *object_creation_expression*s ([Objekt Erstellungs Ausdrücke](expressions.md#object-creation-expressions)) und bis *constructor_initializer*s aufgerufen.

### <a name="constructor-initializers"></a>Konstruktorinitialisierer

Alle Instanzkonstruktoren (außer den Klassen `object`) enthalten implizit einen Aufruf eines anderen Instanzkonstruktors direkt vor dem *constructor_body*-Element. Der implizit aufzurufende Konstruktor wird durch die *constructor_initializer*bestimmt:

*  Ein instanzkonstruktorinitialisierer des Formulars `base(argument_list)` oder `base()` bewirkt, dass ein Instanzkonstruktor von der direkten Basisklasse aufgerufen wird. Dieser Konstruktor wird mit *argument_list* , sofern vorhanden, und den Regeln zur Überladungs Auflösung der [Überladungs Auflösung](expressions.md#overload-resolution)ausgewählt. Der Satz von Kandidaten Instanzkonstruktoren besteht aus allen zugänglichen Instanzkonstruktoren, die in der direkten Basisklasse enthalten sind, oder dem Standardkonstruktor ([Standardkonstruktoren](classes.md#default-constructors)), wenn in der direkten Basisklasse keine Instanzkonstruktoren deklariert werden. Wenn dieser Satz leer ist oder ein einzelner Konstruktor mit der besten Instanz nicht identifiziert werden kann, tritt ein Kompilierzeitfehler auf.
*  Ein instanzkonstruktorinitialisierer des Formulars `this(argument-list)` oder `this()` bewirkt, dass ein Instanzkonstruktor von der Klasse selbst aufgerufen wird. Der Konstruktor wird mit *argument_list* , sofern vorhanden, und den Regeln zur Überladungs Auflösung der [Überladungs Auflösung](expressions.md#overload-resolution)ausgewählt. Der Satz von Kandidaten Instanzkonstruktoren besteht aus allen zugänglichen Instanzkonstruktoren, die in der Klasse selbst deklariert sind. Wenn dieser Satz leer ist oder ein einzelner Konstruktor mit der besten Instanz nicht identifiziert werden kann, tritt ein Kompilierzeitfehler auf. Wenn eine Instanzkonstruktordeklaration einen Konstruktorinitialisierer enthält, der den Konstruktor selbst aufruft, tritt ein Kompilierzeitfehler auf.

Wenn ein Instanzkonstruktor über keinen Konstruktorinitialisierer verfügt, wird ein Konstruktorinitialisierer `base()` des Formulars implizit bereitgestellt. Folglich ist eine Instanzkonstruktordeklaration der Form
```csharp
C(...) {...}
```
ist genau Äquivalent zu
```csharp
C(...): base() {...}
```

Der Gültigkeitsbereich der Parameter, die vom *formal_parameter_list* einer Instanzkonstruktordeklaration angegeben werden, beinhaltet den Konstruktorinitialisierer dieser Deklaration. Daher ist es einem Konstruktorinitialisierer gestattet, auf die Parameter des Konstruktors zuzugreifen. Zum Beispiel:
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

Ein instanzkonstruktorinitialisierer kann nicht auf die Instanz zugreifen, die erstellt wird. Daher ist es ein Kompilierzeitfehler, der auf `this` in einem Argument Ausdruck des konstruktorinitialisierers verweist, ebenso wie es ein Kompilierzeitfehler für einen Argument Ausdruck ist, der auf ein Instanzmember über eine *Simple_name*verweist.

### <a name="instance-variable-initializers"></a>Instanzvariableninitialisierer

Wenn ein Instanzkonstruktor über keinen Konstruktorinitialisierer oder einen Konstruktorinitialisierer der Form `base(...)` verfügt, führt dieser Konstruktor implizit die Initialisierungen aus, die von den *variable_initializer*s der Instanzfelder angegeben werden, die in deklariert werden. die Klasse. Dies entspricht einer Sequenz von Zuweisungen, die unmittelbar nach dem Einstieg in den Konstruktor und vor dem impliziten Aufruf des direkten Basisklassenkonstruktors ausgeführt werden. Die Variableninitialisierer werden in der Text Reihenfolge ausgeführt, in der Sie in der Klassen Deklaration angezeigt werden.

### <a name="constructor-execution"></a>Konstruktorausführung

Variableninitialisierer werden in Zuweisungs Anweisungen transformiert, und diese Zuweisungs Anweisungen werden vor dem Aufruf des Basisklasseninstanzkonstruktors ausgeführt. Diese Reihenfolge stellt sicher, dass alle Instanzfelder durch ihre Variableninitialisierer initialisiert werden, bevor Anweisungen ausgeführt werden, die auf diese Instanz zugreifen können.

Im Beispiel
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
Wenn `new B()` verwendet wird, um eine Instanz von `B`zu erstellen, wird die folgende Ausgabe erzeugt:
```console
x = 1, y = 0
```

Der Wert von `x` ist 1, da der Variableninitialisierer ausgeführt wird, bevor der basisklasseninstanzkonstruktor aufgerufen wird. Der Wert von `y` ist jedoch 0 (der Standardwert `int`von), da die Zuweisung zu `y` erst ausgeführt wird, nachdem der Basisklassenkonstruktor zurückgegeben wurde.

Es ist hilfreich, instanzvariableninitialisierer und Konstruktorinitialisierer als Anweisungen zu betrachten, die vor dem *constructor_body*automatisch eingefügt werden. Das Beispiel
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
enthält mehrere Variableninitialisierer. Sie enthält auch Konstruktorinitialisierer von beiden Formularen (`base` und `this`). Das Beispiel entspricht dem unten gezeigten Code, wobei jeder Kommentar eine automatisch eingefügte Anweisung angibt (die Syntax, die für die automatisch eingefügten Konstruktoraufrufe verwendet wird, ist nicht gültig, dient lediglich zur Veranschaulichung des Mechanismus).

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

Wenn eine Klasse keine Instanzkonstruktordeklarationen enthält, wird automatisch ein Standardinstanzkonstruktor bereitgestellt. Dieser Standardkonstruktor ruft einfach den Parameter losen Konstruktor der direkten Basisklasse auf. Wenn die Klasse abstrakt ist, wird die deklarierte Barrierefreiheit für den Standardkonstruktor geschützt. Andernfalls ist die deklarierte Barrierefreiheit für den Standardkonstruktor öffentlich. Daher ist der Standardkonstruktor immer das Formular.

```csharp
protected C(): base() {}
```
oder
```csharp
public C(): base() {}
```
dabei `C` ist der Name der Klasse. Wenn die Überladungs Auflösung keinen eindeutigen besten Kandidaten für den basisklassenkonstruktorinitialisierer ermitteln kann, tritt ein Kompilierzeitfehler auf.

Im Beispiel
```csharp
class Message
{
    object sender;
    string text;
}
```
ein Standardkonstruktor wird bereitgestellt, da die-Klasse keine Instanzkonstruktordeklarationen enthält. Folglich entspricht das Beispiel genau dem
```csharp
class Message
{
    object sender;
    string text;

    public Message(): base() {}
}
```

### <a name="private-constructors"></a>Private Konstruktoren

Wenn eine Klasse `T` nur private Instanzkonstruktoren deklariert, ist es nicht möglich, dass Klassen außerhalb des Programm `T` Texts von `T` oder direkt Instanzen von `T`erstellen. Wenn eine Klasse nur statische Member enthält und nicht instanziiert werden soll, wird durch das Hinzufügen eines leeren privaten Instanzkonstruktors eine Instanziierung verhindert. Zum Beispiel:
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

Die `Trig` -Klasse gruppiert verwandte Methoden und Konstanten, sollte jedoch nicht instanziiert werden. Daher wird ein einzelner leerer privater Instanzkonstruktor deklariert. Mindestens ein Instanzkonstruktor muss deklariert werden, um die automatische Generierung eines Standardkonstruktors zu unterdrücken.

### <a name="optional-instance-constructor-parameters"></a>Optionale instanzkonstruktorparameter

Die `this(...)` Form des konstruktorinitialisierers wird häufig in Verbindung mit überladen verwendet, um optionale instanzkonstruktorparameter zu implementieren. Im Beispiel
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
die ersten beiden Instanzkonstruktoren stellen lediglich die Standardwerte für die fehlenden Argumente bereit. Beide verwenden einen `this(...)` Konstruktorinitialisierer, um den dritten Instanzkonstruktor aufzurufen, der tatsächlich die Initialisierung der neuen Instanz bewirkt. Der Effekt besteht aus den optionalen Konstruktorparametern:
```csharp
Text t1 = new Text();                    // Same as Text(0, 0, null)
Text t2 = new Text(5, 10);               // Same as Text(5, 10, null)
Text t3 = new Text(5, 20, "Hello");
```

## <a name="static-constructors"></a>Statische Konstruktoren

Ein ***statischer Konstruktor*** ist ein Member, der die erforderlichen Aktionen zum Initialisieren eines geschlossenen Klassen Typs implementiert. Statische Konstruktoren werden mit *static_constructor_declaration*s deklariert:

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

Ein *static_constructor_declaration* kann einen Satz von *Attributen* ([Attribute](attributes.md)) und einen `extern`-Modifizierer ([Externe Methoden](classes.md#external-methods)) enthalten.

Der *Bezeichner* eines *static_constructor_declaration* muss der Klasse, in der der statische Konstruktor deklariert ist, einen Namen benennen. Wenn ein anderer Name angegeben wird, tritt ein Kompilierzeitfehler auf.

Wenn eine statische Konstruktordeklaration einen `extern` Modifizierer enthält, wird als statischer Konstruktor ein ***externer statischer Konstruktor***bezeichnet. Da eine externe statische Konstruktordeklaration keine tatsächliche Implementierung bereitstellt, besteht deren *static_constructor_body* aus einem Semikolon. Für alle anderen statischen Konstruktordeklarationen besteht der *static_constructor_body* aus einem- *Block* , der die auszuführenden Anweisungen angibt, um die-Klasse zu initialisieren. Dies entspricht exakt dem *method_body* einer statischen Methode mit einem `void`-Rückgabetyp ([Methoden Text](classes.md#method-body)).

Statische Konstruktoren werden nicht geerbt und können nicht direkt aufgerufen werden.

Der statische Konstruktor für einen geschlossenen Klassentyp wird höchstens einmal in einer bestimmten Anwendungsdomäne ausgeführt. Die Ausführung eines statischen Konstruktors wird ausgelöst, wenn das erste der folgenden Ereignisse in einer Anwendungsdomäne auftritt:

*  Eine Instanz des-Klassen Typs wird erstellt.
*  Auf alle statischen Member des Klassen Typs wird verwiesen.

Wenn eine Klasse die `Main` Methode ([Anwendungsstart](basic-concepts.md#application-startup)) enthält, in der die Ausführung beginnt, wird der statische Konstruktor für diese Klasse `Main` ausgeführt, bevor die-Methode aufgerufen wird.

Um einen neuen geschlossenen Klassentyp zu initialisieren, wird zuerst ein neuer Satz statischer Felder ([statische Felder und Instanzfelder](classes.md#static-and-instance-fields)) für diesen bestimmten geschlossenen Typ erstellt. Jedes der statischen Felder wird mit dem Standardwert initialisiert ([Standardwerte](variables.md#default-values)). Als nächstes werden die statischen Feldinitialisierer ([statische Feld Initialisierung](classes.md#static-field-initialization)) für diese statischen Felder ausgeführt. Schließlich wird der statische Konstruktor ausgeführt.

Das Beispiel
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
die Ausgabe muss erzeugt werden:
```console
Init A
A.F
Init B
B.F
```
, da der statische `A`Konstruktor der Ausführung durch den `B`- `B.F`aufzurufenden aufgerufen wird und die Ausführung des statischen Konstruktors durch den-Befehl ausgelöst wird. `A.F`

Es ist möglich, zirkuläre Abhängigkeiten zu erstellen, die es ermöglichen, dass statische Felder mit Variableninitialisierern im Standardwert Zustand beobachtet werden.

Das Beispiel
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
```console
X = 1, Y = 2
```

Zum Ausführen der `Main` -Methode führt das System zuerst den Initialisierer für `B.Y`aus, bevor der `B`statische Konstruktor der Klasse ausgeführt wird. `Y`der Initialisierer bewirkt `A`, dass der statische Konstruktor ausgeführt wird, da auf `A.X` den Wert von verwiesen wird. Der statische Konstruktor von `A` führt wiederum den Wert von `X`aus und ruft dabei den Standardwert von `Y`ab, der 0 (null) ist. `A.X`wird daher mit 1 initialisiert. Der Prozess der Ausführung `A`der statischen Feldinitialisierer und des statischen Konstruktors wird dann abgeschlossen, wobei die Berechnung des Anfangs `Y`Werts von zurückgegeben wird. Dadurch wird das Ergebnis 2.

Da der statische Konstruktor für jeden geschlossenen konstruierten Klassentyp genau einmal ausgeführt wird, können Sie Laufzeitüberprüfungen für den Typparameter erzwingen, der zur Kompilierzeit nicht über Einschränkungen ([Typparameter Einschränkungen](classes.md#type-parameter-constraints)) überprüft werden kann. . Der folgende Typ verwendet beispielsweise einen statischen Konstruktor, um zu erzwingen, dass das Typargument eine-Enum ist:
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

Ein ***destrukturtor*** ist ein Member, der die erforderlichen Aktionen zum Zerstörung einer Instanz einer Klasse implementiert. Ein Dekonstruktor wird mit einem *destructor_declaration*deklariert:

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

Ein *destructor_declaration* kann einen Satz von *Attributen* ([Attribute](attributes.md)) enthalten.

Der *Bezeichner* eines *destructor_declaration* muss der Klasse, in der der Dekonstruktor deklariert ist, einen Namen benennen. Wenn ein anderer Name angegeben wird, tritt ein Kompilierzeitfehler auf.

Wenn eine dekonstruktordeklaration `extern` einen Modifizierer enthält, wird der Dekonstruktor als ***externer Dekonstruktor***bezeichnet. Da eine externe dekonstruktordeklaration keine tatsächliche Implementierung bereitstellt, besteht deren *destructor_body* aus einem Semikolon. Für alle anderen destrukturtoren besteht der *destructor_body* aus einem- *Block* , der die auszuführenden Anweisungen angibt, um eine Instanz der-Klasse zu Zerstörung. Ein *destructor_body* entspricht exakt dem *method_body* einer Instanzmethode mit einem `void`-Rückgabetyp ([Methoden Text](classes.md#method-body)).

Deerdektoren werden nicht geerbt. Folglich hat eine Klasse keine Dekonstruktoren, die nicht die Dekonstruktoren aufweisen, die in dieser Klasse deklariert werden können.

Da für einen Dekonstruktor keine Parameter erforderlich sind, kann er nicht überladen werden, sodass eine Klasse höchstens einen Dekonstruktor aufweisen kann.

Dededektoren werden automatisch aufgerufen und können nicht explizit aufgerufen werden. Eine Instanz ist für die Zerstörung infrage, wenn es für keinen Code mehr möglich ist, diese Instanz zu verwenden. Die Ausführung des Dekonstruktors für die Instanz kann zu einem beliebigen Zeitpunkt erfolgen, nachdem die Instanz für eine Zerstörung berechtigt ist. Wenn eine Instanz von zerstört wird, werden die Dekonstruktoren in der Vererbungs Kette dieser Instanz in der richtigen Reihenfolge von der am wenigsten abgeleiteten zum geringsten abgeleiteten aufgerufen. Ein Dekonstruktor kann in jedem Thread ausgeführt werden. Weitere Informationen zu den Regeln, die bestimmen, wann und wie ein Dekonstruktor ausgeführt wird, finden Sie unter [Automatische Speicherverwaltung](basic-concepts.md#automatic-memory-management).

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
Da deerdektoren in einer Vererbungs Kette in der Reihenfolge von den meisten abgeleiteten zu den am wenigsten abgeleiteten

Dededektoren werden implementiert, indem die `Finalize` virtuelle `System.Object`Methode für überschrieben wird. C#Es ist nicht zulässig, dass Programme diese Methode außer Kraft setzen oder Sie direkt (bzw. über schreibungen) direkt aufzurufen. Beispielsweise wird das Programm
```csharp
class A 
{
    override protected void Finalize() {}    // error

    public void F() {
        this.Finalize();                     // error
    }
}
```
enthält zwei Fehler.

Der Compiler verhält sich so, als ob diese Methode und über schreibungen davon überhaupt nicht vorhanden sind. Dieses Programm ist also:
```csharp
class A 
{
    void Finalize() {}                            // permitted
}
```
ist gültig, und die `System.Object`-Methode hat die- `Finalize` Methode ausgeblendet.

Eine Erläuterung des Verhaltens, wenn eine Ausnahme von einem debugtor ausgelöst wird, finden Sie unter [so werden Ausnahmen behandelt](exceptions.md#how-exceptions-are-handled).

## <a name="iterators"></a>Iterators

Ein Funktionsmember ([Funktionsmember](expressions.md#function-members)), der mit einem Iteratorblock ([Blocks](statements.md#blocks)) implementiert wird, wird als ***Iterator***bezeichnet.

Ein Iteratorblock kann als Text eines Funktionsmembers verwendet werden, solange der Rückgabetyp des entsprechenden Funktionsmembers eine der Enumeratorschnittstellen ([Enumeratorschnittstellen](classes.md#enumerator-interfaces)) oder eine der Aufzähl Bare-Schnittstellen ([Aufzähl Bare Schnitt](classes.md#enumerable-interfaces)stellen) ist. . Sie kann als *method_body*, *operator_body* oder *accessor_body*auftreten, wohingegen Ereignisse, Instanzkonstruktoren, statische Konstruktoren und Dekonstruktoren nicht als Iteratoren implementiert werden können.

Wenn ein Funktionsmember mit einem Iteratorblock implementiert wird, ist dies ein Kompilierzeitfehler für die Liste formaler Parameter des Funktionsmembers, um `ref` beliebige `out` -oder-Parameter anzugeben.

### <a name="enumerator-interfaces"></a>Enumeratorschnittstellen

Die ***Enumeratorschnittstellen*** sind die nicht generische Schnittstelle `System.Collections.IEnumerator` und alle Instanziierungen der generischen- `System.Collections.Generic.IEnumerator<T>`Schnittstelle. Aus Gründen der Kürze werden diese Schnittstellen in diesem Kapitel als `IEnumerator` `IEnumerator<T>`bzw. referenziert.

### <a name="enumerable-interfaces"></a>Enumerable-Schnittstellen

Die ***Aufzähl Bare-Schnittstellen*** sind die nicht generische `System.Collections.IEnumerable` Schnittstelle und alle Instanziierungen der generischen-Schnittstelle. `System.Collections.Generic.IEnumerable<T>` Aus Gründen der Kürze werden diese Schnittstellen in diesem Kapitel als `IEnumerable` `IEnumerable<T>`bzw. referenziert.

### <a name="yield-type"></a>Yield-Typ

Ein Iterator erzeugt eine Sequenz von Werten, die alle denselben Typ haben. Dieser Typ wird als ***Yield-Typ*** des Iterators bezeichnet.

*  Der Yield-Typ eines Iterators, der `IEnumerator` oder `IEnumerable` zurück `object`gibt, ist.
*  Der Yield-Typ eines Iterators, der `IEnumerator<T>` oder `IEnumerable<T>` zurück `T`gibt, ist.

### <a name="enumerator-objects"></a>Enumeratorobjekte

Wenn ein Funktionsmember, der einen enumeratorschnittstellentyp zurückgibt, mit einem Iteratorblock implementiert wird, führt der Aufruf des Funktionsmembers den Code nicht sofort im Iteratorblock aus. Stattdessen wird ein ***Enumeratorobjekt*** erstellt und zurückgegeben. Dieses Objekt kapselt den im Iteratorblock angegebenen Code, und die Ausführung des Codes im Iteratorblock tritt auf, wenn die- `MoveNext` Methode des Enumeratorobjekts aufgerufen wird. Ein Enumeratorobjekt weist die folgenden Eigenschaften auf:

*  Es implementiert `IEnumerator` und `IEnumerator<T>`, wobei `T` der Yield-Typ des Iterators ist.
*  Sie implementiert `System.IDisposable`.
*  Sie wird mit einer Kopie der Argument Werte (sofern vorhanden) und dem Instanzwert initialisiert, der an das Funktionsmember übermittelt wurde.
*  Sie verfügt über vier mögliche Zustände: ***vor***, wird ***ausgeführt*** ***, angeh***alten und ***nach***, und befindet sich anfänglich im Zustand " ***vor*** ".

Bei einem Enumeratorobjekt handelt es sich in der Regel um eine Instanz einer vom Compiler generierten Enumeratorklasse, die den Code im Iteratorblock kapselt und die Enumeratorschnittstellen implementiert, aber andere Implementierungs Methoden sind möglich. Wenn eine Enumeratorklasse vom Compiler generiert wird, wird diese Klasse direkt oder indirekt in der Klasse, die den Funktionsmember enthält, in die-Klasse eingefügt, Sie verfügt über private zugreif barkeit und hat einen Namen, der fürdie Verwendung durch den Compiler reserviert ist ([Identifier](lexical-structure.md#identifiers)).

Ein Enumeratorobjekt kann mehr Schnittstellen implementieren, als die oben genannten.

In den folgenden `MoveNext`Abschnitten wird das genaue Verhalten der `IEnumerable` - `Current`,- `Dispose` und-Member der `IEnumerable<T>` -und-Schnittstellen Implementierungen beschrieben, die von einem Enumeratorobjekt bereitgestellt werden.

Beachten Sie, dass Enumeratorobjekte die `IEnumerator.Reset` -Methode nicht unterstützen. Das Aufrufen dieser Methode bewirkt `System.NotSupportedException` , dass eine ausgelöst wird.

#### <a name="the-movenext-method"></a>Die Methode "Design ext"

Die `MoveNext` -Methode eines Enumeratorobjekts kapselt den Code eines Iteratorblocks. Durch Aufrufen `MoveNext` der-Methode wird Code im Iteratorblock ausgeführt, `Current` und die-Eigenschaft des Enumeratorobjekts wird entsprechend festgelegt. Die genaue Aktion, die `MoveNext` von ausgeführt wird, hängt vom Status des Enumeratorobjekts ab, wenn `MoveNext` aufgerufen wird:

*  Wenn der Status des Enumeratorobjekts ***vor***ist, wird aufgerufen `MoveNext`:
   * Ändert den Status in wird ***ausgeführt***.
   * Initialisiert die Parameter (einschließlich `this`) des Iteratorblocks mit den Argument Werten und dem Instanzwert, die beim Initialisieren des Enumeratorobjekts gespeichert wurden.
   * Führt den Iteratorblock von dem Anfang aus, bis die Ausführung unterbrochen wird (wie unten beschrieben).
*  Wenn der Status des Enumeratorobjekts ***ausgeführt***wird, `MoveNext` ist das Ergebnis des Aufrufs nicht angegeben.
*  Wenn der Status des Enumeratorobjekts angehalten ***wird,*** wird `MoveNext`aufgerufen:
   * Ändert den Status in wird ***ausgeführt***.
   * Stellt die Werte aller lokalen Variablen und Parameter (einschließlich dieser) für die Werte wieder her, die bei der letzten Ausführung des Iteratorblocks gespeichert wurden. Beachten Sie, dass sich der Inhalt aller Objekte, auf die von diesen Variablen verwiesen wird, seit dem vorherigen-Befehl von "muvenext" geändert hat.
   * Nimmt die Ausführung des Iteratorblocks unmittelbar nach der `yield return` Anweisung an, die die Unterbrechung der Ausführung verursacht hat, und wird fortgesetzt, bis die Ausführung unterbrochen wurde (wie unten beschrieben).
*  Wenn der Zustand des Enumeratorobjekts ***nach***ist, wird der `MoveNext` Aufruf `false`von zurückgegeben.


Wenn `MoveNext` den Iteratorblock ausführt, kann die Ausführung auf vier Arten unterbrochen werden: Durch eine `yield return` -Anweisung durch eine `yield break` -Anweisung, durch die das Ende des Iteratorblocks und durch eine Ausnahme ausgelöst und aus dem Iteratorblock weitergegeben wurde.

*  Wenn eine `yield return` -Anweisung gefunden wird ([die yield-Anweisung](statements.md#the-yield-statement)):
   * Der in der-Anweisung angegebene Ausdruck wird ausgewertet, implizit in den Yield-Typ konvertiert und der `Current` -Eigenschaft des Enumeratorobjekts zugewiesen.
   * Die Ausführung des iteratortexts wurde angehalten. Die Werte aller lokalen Variablen und Parameter (einschließlich `this`) werden gespeichert, ebenso wie der Speicherort dieser `yield return` Anweisung. Wenn sich `yield return` die-Anweisung innerhalb eines oder `try` mehrerer Blöcke befindet, `finally` werden die zugeordneten Blöcke zurzeit nicht ausgeführt.
   * Der Status des Enumeratorobjekts ***wird in "*** angehalten" geändert.
   * Die `MoveNext` -Methode `true` kehrt an ihren Aufrufer zurück und gibt an, dass die Iterationen erfolgreich auf den nächsten Wert erweitert wurden.
*  Wenn eine `yield break` -Anweisung gefunden wird ([die yield-Anweisung](statements.md#the-yield-statement)):
   * Wenn die `yield break` Anweisung innerhalb eines oder mehrerer `try` Blöcke liegt, werden die `finally` zugeordneten Blöcke ausgeführt.
   * Der Status des Enumeratorobjekts wird in ***nach***geändert.
   * Die `MoveNext` Methode kehrt `false` an ihren Aufrufer zurück und gibt an, dass die Iterations Funktion vollständig ist.
*  Wenn das Ende des iteratortexts erreicht ist:
   * Der Status des Enumeratorobjekts wird in ***nach***geändert.
   * Die `MoveNext` Methode kehrt `false` an ihren Aufrufer zurück und gibt an, dass die Iterations Funktion vollständig ist.
*  Wenn eine Ausnahme ausgelöst und aus dem Iteratorblock weitergegeben wird:
   * Die `finally` entsprechenden Blöcke im iteratortext werden von der Ausnahme Weitergabe ausgeführt.
   * Der Status des Enumeratorobjekts wird in ***nach***geändert.
   * Die Ausnahme Weitergabe wird an den Aufrufer der `MoveNext` -Methode weitergegeben.

#### <a name="the-current-property"></a>Die aktuelle Eigenschaft

Die-Eigenschaft eines Enumeratorobjekts wirkt sich `yield return` auf- `Current` Anweisungen im Iteratorblock aus.

Wenn sich ein Enumeratorobjekt im angehaltenen Zustand befindet, ist `Current` der Wert von der Wert, der durch `MoveNext`den vorherigen-Befehl festgelegt wurde. Wenn sich ein Enumeratorobjekt in den Zuständen " ***before***", " ***Running***" oder " ***after*** " `Current` befindet, ist das Ergebnis des Zugriffs nicht angegeben.

Bei einem Iterator mit einem anderen Yield-Typ `object`als entspricht das Ergebnis des `Current` Zugriffs auf die- `IEnumerable` Implementierung des Enumeratorobjekts dem `Current` Zugriff über das- `IEnumerator<T>` Enumeratorobjekt. Implementierung und Umwandeln des Ergebnisses in `object`.

#### <a name="the-dispose-method"></a>Die verwerfen-Methode

Die `Dispose` -Methode wird verwendet, um die Iterationen zu bereinigen, indem das Enumeratorobjekt in den ***after*** -Zustand versetzt wird.

*  Wenn der Status des Enumeratorobjekts ***vorher***ist, ändert der `Dispose` Aufruf von den Zustand in ***nach***.
*  Wenn der Status des Enumeratorobjekts ***ausgeführt***wird, `Dispose` ist das Ergebnis des Aufrufs nicht angegeben.
*  Wenn der Status des Enumeratorobjekts angehalten ***wird,*** wird `Dispose`aufgerufen:
   * Ändert den Status in wird ***ausgeführt***.
   * Führt beliebige letzte Blöcke aus, als wäre die `yield return` Letzte ausgeführte `yield break` Anweisung eine-Anweisung. Wenn dies bewirkt, dass eine Ausnahme ausgelöst und aus dem iteratortext weitergegeben wird, wird der Status des Enumeratorobjekts auf ***after*** festgelegt, und die Ausnahme wird an den Aufrufer der `Dispose` Methode weitergegeben.
   * Ändert den Zustand in ***nach***.
*  Wenn der Zustand des Enumeratorobjekts ***nach***ist, hat der `Dispose` Aufruf von keine Auswirkungen.

### <a name="enumerable-objects"></a>Enumerable-Objekte

Wenn ein Funktionsmember, der einen Aufzähl baren Schnittstellentyp zurückgibt, mit einem Iteratorblock implementiert wird, führt der Aufruf des Funktionsmembers den Code nicht sofort im Iteratorblock aus. Stattdessen wird ein ***Aufzähl bares Objekt*** erstellt und zurückgegeben. Die- `GetEnumerator` Methode des Aufzähl Bare-Objekts gibt ein Enumeratorobjekt zurück, das den im Iteratorblock angegebenen Code kapselt, und die Ausführung des Codes im Iteratorblock tritt auf, wenn die- `MoveNext` Methode des Enumeratorobjekts aufgerufen wird. Ein Aufzähl Bare-Objekt hat die folgenden Eigenschaften:

*  Es implementiert `IEnumerable` und `IEnumerable<T>`, wobei `T` der Yield-Typ des Iterators ist.
*  Sie wird mit einer Kopie der Argument Werte (sofern vorhanden) und dem Instanzwert initialisiert, der an das Funktionsmember übermittelt wurde.

Ein Aufzähl Bare-Objekt ist in der Regel eine Instanz einer vom Compiler generierten Aufzähl Bare-Klasse, die den Code im Iteratorblock kapselt und die Aufzähl Bare-Schnittstellen implementiert, aber andere Implementierungs Methoden sind möglich. Wenn eine Aufzähl Bare-Klasse vom Compiler generiert wird, wird diese Klasse direkt oder indirekt in der-Klasse, die den Funktionsmember enthält, in eine private Barrierefreiheit eingefügt, und Sie erhält einen Namen, der für die Verwendung durch den[Compiler (](lexical-structure.md#identifiers)Bezeichner) reserviert ist.

Ein Aufzähl Bare-Objekt kann mehr Schnittstellen implementieren, als die oben genannten. Insbesondere kann ein Aufzähl Bare-Objekt auch und `IEnumerator` `IEnumerator<T>`implementieren, sodass es sowohl als Aufzähl Bare-als auch als Enumerator fungieren kann. Bei diesem Implementierungstyp wird das Aufzähl Bare Objekt selbst zurückgegeben `GetEnumerator` , wenn die-Methode eines Aufzähl Bare-Objekts zum ersten Mal aufgerufen wird. Nachfolgende Aufrufe des Aufzähl baren Objekts `GetEnumerator`geben, falls vorhanden, eine Kopie des Aufzähl Bare-Objekts zurück. Folglich hat jeder zurückgegebene Enumerator seinen eigenen Zustand, und Änderungen in einem Enumerator haben keine Auswirkung auf einen anderen Enumerator.

#### <a name="the-getenumerator-method"></a>Die getenreerator-Methode

Ein Aufzähl Bare `IEnumerable` -Objekt stellt eine Implementierung der `GetEnumerator` Methoden der-Schnitt `IEnumerable<T>` Stelle und der-Schnittstelle bereit. Die beiden `GetEnumerator` Methoden verwenden eine gemeinsame-Implementierung, die ein verfügbares Enumeratorobjekt abruft und zurückgibt. Das Enumeratorobjekt wird mit den Argument Werten und dem Instanzwert initialisiert, die gespeichert wurden, als das Aufzähl Bare Objekt initialisiert wurde. andernfalls ist das Enumeratorobjekt wie in [enumeratorobjekten](classes.md#enumerator-objects)beschrieben funktionsfähig.

### <a name="implementation-example"></a>Implementierungs Beispiel

In diesem Abschnitt wird eine mögliche Implementierung von Iteratoren in Bezug auf C# standardkonstrukte beschrieben. Die hier beschriebene Implementierung basiert auf denselben Prinzipien, die vom Microsoft C# -Compiler verwendet werden. Dies bedeutet jedoch nicht, dass es sich um eine vorgeschriebene Implementierung oder die einzige Möglichkeit handelt.

Die folgende `Stack<T>` Klasse `GetEnumerator` implementiert die-Methode mit einem Iterator. Der Iterator listet die Elemente des Stapels in der Reihenfolge von oben nach unten auf.

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

Die `GetEnumerator` -Methode kann in eine Instanziierung einer vom Compiler generierten Enumeratorklasse übersetzt werden, die den Code im Iteratorblock kapselt, wie im folgenden gezeigt.

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

In der obigen Übersetzung wird der Code im Iteratorblock in einen Zustands Automat umgewandelt und in die `MoveNext` -Methode der Enumeratorklasse eingefügt. Außerdem wird die lokale Variable `i` in ein Feld im Enumeratorobjekt umgewandelt, sodass Sie weiterhin über Aufrufe von `MoveNext`vorhanden sein kann.

Im folgenden Beispiel wird eine einfache Multiplikationstabelle der ganzen Zahlen 1 bis 10 gedruckt. Die `FromTo` -Methode im Beispiel gibt ein Aufzähl bares Objekt zurück und wird mit einem Iterator implementiert.

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

Die `FromTo` -Methode kann in eine Instanziierung einer vom Compiler generierten Aufzähl Bare-Klasse übersetzt werden, die den Code im Iteratorblock kapselt, wie im folgenden gezeigt.

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

Die Aufzähl Bare-Klasse implementiert sowohl die Aufzähl Bare-Schnittstelle als auch die Enumeratorschnittstellen, sodass Sie sowohl als Aufzähl Bare-als auch als Enumerator fungieren kann. Wenn die- `GetEnumerator` Methode zum ersten Mal aufgerufen wird, wird das Aufzähl Bare-Objekt selbst zurückgegeben. Nachfolgende Aufrufe des Aufzähl baren Objekts `GetEnumerator`geben, falls vorhanden, eine Kopie des Aufzähl Bare-Objekts zurück. Folglich hat jeder zurückgegebene Enumerator seinen eigenen Zustand, und Änderungen in einem Enumerator haben keine Auswirkung auf einen anderen Enumerator. Die `Interlocked.CompareExchange` -Methode wird verwendet, um einen Thread sicheren Vorgang sicherzustellen.

Der `from` - `to` Parameter und der-Parameter werden in Felder in der Aufzähl Bare-Klasse umgewandelt. Da `from` im Iteratorblock geändert wird, wird ein zusätzliches `__from` Feld eingeführt, das den Anfangswert enthält, der `from` für jeden Enumerator angegeben wird.

Die `MoveNext` Methode löst eine `InvalidOperationException` aus, wenn `0`Sie aufgerufen `__state` wird, wenn den Wert hat. Dadurch wird verhindert, dass das Aufzähl Bare Objekt als Enumeratorobjekt verwendet wird, ohne `GetEnumerator`dass zuerst aufgerufen wird.

Das folgende Beispiel zeigt eine einfache Strukturklasse. Die `Tree<T>` -Klasse `GetEnumerator` implementiert die-Methode mit einem Iterator. Der Iterator listet die Elemente der Struktur in der Infix-Reihenfolge auf.

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

Die `GetEnumerator` -Methode kann in eine Instanziierung einer vom Compiler generierten Enumeratorklasse übersetzt werden, die den Code im Iteratorblock kapselt, wie im folgenden gezeigt.

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

Die vom Compiler generierten temporare, die `foreach` in den-Anweisungen verwendet `__left` werden `__right` , werden in die Felder und des Enumeratorobjekts gehoben. Das `__state` -Feld des Enumeratorobjekts wird sorgfältig aktualisiert, sodass die richtige `Dispose()` Methode ordnungsgemäß aufgerufen wird, wenn eine Ausnahme ausgelöst wird. Beachten Sie, dass es nicht möglich ist, den übersetzten Code `foreach` mit einfachen Anweisungen zu schreiben.

## <a name="async-functions"></a>Async-Funktionen

Eine Methode ([Methoden](classes.md#methods)) oder eine anonyme Funktion ([Anonyme Funktions Ausdrücke](expressions.md#anonymous-function-expressions)) mit `async` dem-Modifizierer wird als ***Async-Funktion***bezeichnet. Im Allgemeinen wird der Begriff ***Async*** verwendet, um jede Art von Funktion zu beschreiben, die `async` über den-Modifizierer verfügt.

Es handelt sich um einen Kompilierzeitfehler für die Liste formaler Parameter einer Async-Funktion, `ref` um `out` beliebige-oder-Parameter anzugeben.

Der *return_type* einer Async-Methode muss entweder `void` oder ein ***Tasktyp***sein. Die Aufgaben Typen sind `System.Threading.Tasks.Task` -und-Typen `System.Threading.Tasks.Task<T>`, die aus erstellt werden. Der Kürze halber wird in diesem Kapitel auf diese Typen als `Task` `Task<T>`bzw. verwiesen. Eine Async-Methode, die einen Tasktyp zurückgibt, wird als Aufgaben Rückgabe bezeichnet.

Die genaue Definition der Aufgaben Typen ist implementiert, aber aus der Sicht der Sprache befindet sich ein Aufgabentyp in einem der Zustände unvollständig, erfolgreich oder fehlerhaft. Eine fehlerhafte Aufgabe zeichnet eine relevante Ausnahme auf. Ein erfolgreicher Datensatz `T` zeichneteinErgebnis`Task<T>` des Typs auf. Aufgaben Typen sind möglich und können daher die Operanden von Erwartungs Ausdrücken (Erwartungs[Ausdrücke](expressions.md#await-expressions)) sein.

Ein Async-Funktionsaufruf bietet die Möglichkeit zum Aussetzen der Auswertung mithilfe von Erwartungs Ausdrücken (Erwartungs[Ausdrücke](expressions.md#await-expressions)) im Textkörper. Die Auswertung kann später an dem Punkt fortgesetzt werden, an dem der aufrufende Ausdruck mithilfe eines ***Wiederaufnahme***Delegaten fortgesetzt wird. Der Wiederaufnahme Delegat ist `System.Action`vom Typ, und wenn er aufgerufen wird, wird die Auswertung des asynchronen Funktions aufrutens aus dem Erwartungs Ausdruck fortgesetzt, an dem er unterbrochen wurde. Der ***aktuelle*** Aufrufer eines Async-Funktions Aufrufers ist der ursprüngliche Aufrufer, wenn der Funktionsaufruf nie angehalten wurde, andernfalls der letzte Aufrufer des Wiederaufnahme Delegaten.

### <a name="evaluation-of-a-task-returning-async-function"></a>Auswertung einer Aufgaben Rückgabe Async-Funktion

Durch den Aufruf einer Aufgaben Rückgabe Async-Funktion wird eine Instanz des zurückgegebenen Aufgaben Typs generiert. Dies wird als ***Rückgabe Task*** der Async-Funktion bezeichnet. Der Task befindet sich anfänglich in einem unvollständigen Zustand.

Der asynchrone Funktions Text wird dann ausgewertet, bis er entweder angehalten wird (indem ein Erwartungs Ausdruck erreicht wird) oder beendet wird, an dem die Punkt Steuerung zusammen mit der Rückgabe Aufgabe an den Aufrufer zurückgegeben wird.

Wenn der Text der Async-Funktion beendet wird, wird die Rückgabe Aufgabe aus dem unvollständigen Zustand verschoben:

*  Wenn der Funktions Rumpf durch das Erreichen einer Return-Anweisung oder des Endes des Texts beendet wird, werden alle Ergebnis Werte in der Rückgabe Aufgabe aufgezeichnet, die in den Status "erfolgreich" versetzt wird.
*  Wenn der Funktions Text als Ergebnis einer nicht abgefangenen Ausnahme ([der throw-Anweisung](statements.md#the-throw-statement)) beendet wird, wird die Ausnahme in der Rückgabe Aufgabe aufgezeichnet, die in einen fehlerhaften Zustand versetzt wird.

### <a name="evaluation-of-a-void-returning-async-function"></a>Auswertung einer "void"-Rückgabe Async-Funktion

Wenn der Rückgabetyp der Async- `void`Funktion ist, weicht die Auswertung von der obigen auf folgende Weise ab: Da keine Aufgabe zurückgegeben wird, kommuniziert die Funktion stattdessen mit Abschluss und Ausnahmen mit dem ***Synchronisierungs Kontext***des aktuellen Threads. Die genaue Definition des Synchronisierungs Kontexts ist implementierungsabhängig, ist jedoch eine Darstellung von "Where", in der der aktuelle Thread ausgeführt wird. Der Synchronisierungs Kontext wird benachrichtigt, wenn die Auswertung einer Async-Funktion mit void-Rückgabe beginnt, erfolgreich abgeschlossen wird oder eine nicht abgefangene Ausnahme ausgelöst wird.

Dies ermöglicht es dem Kontext nachzuverfolgen, wie viele void-zurückgegebene asynchrone Funktionen darunter ausgeführt werden, und um zu entscheiden, wie Ausnahmen weitergegeben werden sollen, die aus ihnen stammen.
