# <a name="attributes"></a>Attribute

Ein Großteil der C#-Sprache ermöglicht es den Programmierer, geben Sie die deklarative Informationen zu den Entitäten, die im Programm definiert. Z. B. der Zugriff auf eine Methode in einer Klasse angegeben ist, werden, indem diese mit der *Method_modifier*s `public`, `protected`, `internal`, und `private`.

C# ermöglicht es Programmierern, neue Arten von deklarativer Informationen, so genannte erfinden ***Attribute***. Programmierer können klicken Sie dann Attribute an verschiedene anfügen, und rufen Informationen über Bildattribute in einer Laufzeitumgebung. Beispielsweise kann ein Framework definieren eine `HelpAttribute` -Attribut, das für bestimmte Programmelemente (z. B. Klassen und Methoden) platziert werden kann, um eine Zuordnung von diese Programmelemente in der entsprechenden Dokumentation bereitzustellen.

Attribute werden durch die Deklaration von Attributklassen definiert ([Attributklassen](attributes.md#attribute-classes)), die möglicherweise mit Feldern fester Breite und benannte Parameter ([Feldern fester Breite und benannte Parameter](attributes.md#positional-and-named-parameters)). Attribute werden angefügt, um Entitäten in einem C#-Programm mit Attributspezifikationen ([Attributspezifikation](attributes.md#attribute-specification)), und zur Laufzeit abgerufen werden kann, um als Attributinstanzen ([Attributinstanzen](attributes.md#attribute-instances)).

## <a name="attribute-classes"></a>Attributklassen

Eine von der abstrakten Klasse abgeleitete Klasse `System.Attribute`, entweder direkt oder indirekt ist ein ***Attributklasse***. Die Deklaration einer Attributklasse definiert eine neue Art von ***Attribut*** dar, die in einer Deklaration platziert werden kann. Attributklassen heißen gemäß der Konvention, mit dem Suffix `Attribute`. Ein Attribut können entweder ein- oder lassen Sie das Suffix.

### <a name="attribute-usage"></a>Attributverwendung

Das Attribut `AttributeUsage` ([das AttributeUsage-Attribut](attributes.md#the-attributeusage-attribute)) wird verwendet, um zu beschreiben, wie eine Attributklasse verwendet werden kann.

`AttributeUsage` verfügt über einen Parameter mit Feldern fester Breite ([Feldern fester Breite und benannte Parameter](attributes.md#positional-and-named-parameters)), ermöglicht eine Attributklasse an die Arten von Deklarationen, auf dem sie verwendet werden kann. Im Beispiel

```csharp
using System;

[AttributeUsage(AttributeTargets.Class | AttributeTargets.Interface)]
public class SimpleAttribute: Attribute 
{
    ...
}
```

definiert eine Attributklasse, die mit dem Namen `SimpleAttribute` , platziert werden können, auf *Class_declaration*s und *Interface_declaration*nur. Im Beispiel

```csharp
[Simple] class Class1 {...}

[Simple] interface Interface1 {...}
```

Zeigt, verwendet der `Simple` Attribut. Obwohl dieses Attribut, mit dem Namen definiert ist `SimpleAttribute`, wenn dieses Attribut verwendet wird, die `Attribute` Suffix kann ausgelassen werden, was den kurzen Namen `Simple`. Folglich ist das obige Beispiel semantisch äquivalent zu folgendem:

```csharp
[SimpleAttribute] class Class1 {...}

[SimpleAttribute] interface Interface1 {...}
```

`AttributeUsage` verfügt über einen benannten Parameter ([Feldern fester Breite und benannte Parameter](attributes.md#positional-and-named-parameters)) namens `AllowMultiple`, der angibt, ob das Attribut für eine bestimmte Entität mehrmals angegeben werden kann. Wenn `AllowMultiple` Klasse für ein Attribut ist "true", und klicken Sie dann diese Attributklasse ist eine ***dienenden Attributklasse***, und kann für eine Entität mehrmals angegeben werden. Wenn `AllowMultiple` Klasse für ein Attribut ist "false" oder ist nicht angegeben ist, lautet die Attributklasse eine ***einmaligen Verwendung Attributklasse***, und kann für eine Entität höchstens einmal angegeben werden.

Im Beispiel

```csharp
using System;

[AttributeUsage(AttributeTargets.Class, AllowMultiple = true)]
public class AuthorAttribute: Attribute
{
    private string name;

    public AuthorAttribute(string name) {
        this.name = name;
    }

    public string Name {
        get { return name; }
    }
}
```

definiert eine Multi-Use-Attribut-Klasse, die mit dem Namen `AuthorAttribute`. Im Beispiel

```csharp
[Author("Brian Kernighan"), Author("Dennis Ritchie")] 
class Class1
{
    ...
}
```

Zeigt eine Klassendeklaration mit zwei Verwendungen von der `Author` Attribut.

`AttributeUsage` verfügt über eine andere benannte Parameter namens `Inherited`, der angibt, ob das Attribut, wenn Sie in einer Basisklasse angegeben auch von Klassen geerbt wird, die von dieser Basisklasse abgeleitet werden. Wenn `Inherited` Klasse für ein Attribut ist "true", und klicken Sie dann dieses Attribut geerbt. Wenn `Inherited` Klasse für ein Attribut ist "false", und klicken Sie dann dieses Attribut wird nicht geerbt. Wenn dies nicht angegeben ist, ist der Standardwert "true".

Eine Attributklasse `X` ohne eine `AttributeUsage` -Attribut zugewiesen ist, wie in

```csharp
using System;

class X: Attribute {...}
```

Ist äquivalent zu folgendem:

```csharp
using System;

[AttributeUsage(
    AttributeTargets.All,
    AllowMultiple = false,
    Inherited = true)
]
class X: Attribute {...}
```

### <a name="positional-and-named-parameters"></a>Positionelle und benannte Parameter

Attributklassen haben ***Positionsparameter*** und ***benannte Parameter***. Jeder öffentlichen Instanzkonstruktor für eine Attributklasse definiert eine gültige Sequenz an positionelle Parameter für diese Attributklasse. Jede nicht statische öffentliche Lese-/ Schreibfeld und die Eigenschaft für eine Attributklasse definiert einen benannten Parameter für die Attributklasse.

Im Beispiel

```csharp
using System;

[AttributeUsage(AttributeTargets.Class)]
public class HelpAttribute: Attribute
{
    public HelpAttribute(string url) {        // Positional parameter
        ...
    }

    public string Topic {                     // Named parameter
        get {...}
        set {...}
    }

    public string Url {
        get {...}
    }
}
```

definiert eine Attributklasse, die mit dem Namen `HelpAttribute` einen positionelle Parameter, dessen `url`, und ein benannter Parameter, `Topic`. Obwohl es nicht statisch und öffentlich und die Eigenschaft ist `Url` definiert keinen benannten Parameter aus, da er nicht über Lese-/ Schreibzugriff ist.

Diese Attributklasse kann wie folgt verwendet werden:

```csharp
[Help("http://www.mycompany.com/.../Class1.htm")]
class Class1
{
    ...
}

[Help("http://www.mycompany.com/.../Misc.htm", Topic = "Class2")]
class Class2
{
    ...
}
```

### <a name="attribute-parameter-types"></a>Attributparametertypen

Die Typen der positionelle und benannte Parameter für eine Attributklasse sind auf die ***Parameter Attributtypen***, welche sind:

*  Einer der folgenden Typen: `bool`, `byte`, `char`, `double`, `float`, `int`, `long`, `sbyte`, `short`, `string`, `uint`, `ulong`, `ushort`.
*  Typ `object`.
*  Typ `System.Type`.
*  Ein Enumerationstyp bereitgestellt, sie öffentlichen Zugriff hat und die Typen, die in der geschachtelt ist (sofern vorhanden) werden auch öffentliche zugreifbarkeit besitzen ([Attributspezifikation](attributes.md#attribute-specification)).
*  Eindimensionale Arrays der oben genannten Typen.
*  Ein Konstruktorargument oder ein öffentliches Feld, der nicht einem dieser Typen, werden nicht verwendet als Positionsparameter oder benannte Parameter in einer Attributspezifikation.

## <a name="attribute-specification"></a>Attributspezifikation

***Attributspezifikation*** ist die Anwendung von einem zuvor definierten Attribut auf eine Deklaration. Ein Attribut ist ein Stück von zusätzlichen deklarativen Informationen, der für eine Deklaration angegeben wird. Attribute können im globalen Gültigkeitsbereich (zur Angabe von Attributen auf die enthaltende Assembly oder ein Modul) angegeben werden und für *Type_declaration*s ([Typdeklarationen](namespaces.md#type-declarations)), *Class_member_declaration* s ([Geben Sie die Einschränkungen für Typparameter](classes.md#type-parameter-constraints)), *Interface_member_declaration*s ([Schnittstellenmember](interfaces.md#interface-members)), *Struct_member _declaration*s ([Strukturmember](structs.md#struct-members)), *Enum_member_declaration*s ([Enumerationsmember](enums.md#enum-members)), *Accessor_declarations*  ([Accessoren](classes.md#accessors)), *Event_accessor_declarations* ([Feldähnliche Ereignisse](classes.md#field-like-events)), und *Formal_parameter_list*s ([Methodenparameter](classes.md#method-parameters)).

Attribute werden in angegeben ***Attribut Abschnitte***. Ein Attributabschnitt besteht aus einem Paar eckiger Klammern, die eine durch Trennzeichen getrennte Liste von einem oder mehreren Attributen zu umschließen. Die Reihenfolge, in der Attribute in einer solchen Liste angegeben werden, und die Reihenfolge, in der Abschnitte auf dieselbe Anwendung Entität angefügt, werden angeordnet, spielt keine. Z. B. die Attributspezifikationen `[A][B]`, `[B][A]`, `[A,B]`, und `[B,A]` entsprechen.

```antlr
global_attributes
    : global_attribute_section+
    ;

global_attribute_section
    : '[' global_attribute_target_specifier attribute_list ']'
    | '[' global_attribute_target_specifier attribute_list ',' ']'
    ;

global_attribute_target_specifier
    : global_attribute_target ':'
    ;

global_attribute_target
    : 'assembly'
    | 'module'
    ;

attributes
    : attribute_section+
    ;

attribute_section
    : '[' attribute_target_specifier? attribute_list ']'
    | '[' attribute_target_specifier? attribute_list ',' ']'
    ;

attribute_target_specifier
    : attribute_target ':'
    ;

attribute_target
    : 'field'
    | 'event'
    | 'method'
    | 'param'
    | 'property'
    | 'return'
    | 'type'
    ;

attribute_list
    : attribute (',' attribute)*
    ;

attribute
    : attribute_name attribute_arguments?
    ;

attribute_name
    : type_name
    ;

attribute_arguments
    : '(' positional_argument_list? ')'
    | '(' positional_argument_list ',' named_argument_list ')'
    | '(' named_argument_list ')'
    ;

positional_argument_list
    : positional_argument (',' positional_argument)*
    ;

positional_argument
    : attribute_argument_expression
    ;

named_argument_list
    : named_argument (','  named_argument)*
    ;

named_argument
    : identifier '=' attribute_argument_expression
    ;

attribute_argument_expression
    : expression
    ;
```

Ein Attribut besteht aus einem *Attribute_name* und eine optionale Liste von benannten und Positionsargumenten. Die positionellen Argumente (sofern vorhanden) vorangestellt werden die benannten Argumente. Ein Positionsargument besteht aus einer *Attribute_argument_expression*; ein benanntes Argument besteht aus einem Namen, gefolgt von einem Gleichheitszeichen, gefolgt von einem *Attribute_argument_expression*, das zusammen , werden durch dieselben Regeln wie für einfache Zuweisung eingeschränkt. Die Reihenfolge der benannten Argumente ist nicht wichtig.

Die *Attribute_name* identifiziert eine Attributklasse. Wenn die Form der *Attribute_name* ist *Type_name* wird dieser Name muss mit einer Attributklasse verweisen. Andernfalls tritt ein Kompilierungsfehler auf. Im Beispiel

```csharp
class Class1 {}

[Class1] class Class2 {}    // Error
```

Führt einen Kompilierzeitfehler, da versucht wird, verwenden Sie `Class1` als Attribut Klasse an, wenn `Class1` ist keine Attributklasse.

Bestimmte Kontexte ermöglichen die Angabe eines Attributs auf mehr als ein Ziel. Ein Programm kann explizit Geben Sie das Ziel durch Einschließen einer *Attribute_target_specifier*. Wenn ein Attribut auf globaler Ebene platziert wird eine *Global_attribute_target_specifier* ist erforderlich. In allen anderen Standorten, ein geeigneten Standardwert angewendet wird, aber ein *Attribute_target_specifier* kann verwendet werden, zu bestätigen, oder die Standardeinstellung in bestimmten Fällen mehrdeutigen überschreiben (oder nur der Standardwert in nicht-mehrdeutigen Fällen bestätigen). Daher, in der Regel *Attribute_target_specifier*s kann nur auf globaler Ebene ausgelassen werden. Die potenziell mehrdeutigen Kontexte sind wie folgt aufgelöst:

*  Ein Attribut im globalen Bereich angegeben kann es sich um die Zielassembly oder das Zielmodul angewendet. Kein Standardwert vorhanden ist für diesen Kontext, also eine *Attribute_target_specifier* ist in diesem Kontext immer erforderlich. Das Vorhandensein der `assembly` *Attribute_target_specifier* gibt an, dass das Attribut für das Ziel gilt Assembly; das Vorhandensein der `module` *Attribute_target_specifier* Gibt an, dass das Attribut auf das Zielmodul angewendet wird.
*  Ein Attribut in einer Delegatdeklaration angegeben kann entweder an den Delegaten deklariert wird oder ihren Rückgabewert anwenden. In Ermangelung einer *Attribute_target_specifier*, das Attribut gilt für den Delegaten. Das Vorhandensein der `type` *Attribute_target_specifier* gibt an, dass das Attribut für den Delegaten, gilt das Vorhandensein der `return` *Attribute_target_specifier* Gibt an, dass das Attribut auf den Rückgabewert angewendet wird.
*  Ein Attribut in einer Methodendeklaration angegeben kann entweder an die Methode deklariert wird oder ihren Rückgabewert anwenden. In Ermangelung einer *Attribute_target_specifier*, das Attribut gilt für die Methode. Das Vorhandensein der `method` *Attribute_target_specifier* gibt an, dass das Attribut für die Methode gilt das Vorhandensein der `return` *Attribute_target_specifier* angibt Das Attribut auf den Rückgabewert angewendet werden soll.
*  Ein Attribut auf einen Operatordeklaration angegebenen kann entweder an den Operator deklariert wird oder ihren Rückgabewert anwenden. In Ermangelung einer *Attribute_target_specifier*, das Attribut gilt für den Operator. Das Vorhandensein der `method` *Attribute_target_specifier* gibt an, dass das Attribut für den Operator, gilt das Vorhandensein der `return` *Attribute_target_specifier* Gibt an, dass das Attribut auf den Rückgabewert angewendet wird.
*  Ein Attribut in einer Ereignisdeklaration, die von Ereignisaccessoren ausgelassen angegeben kann angewendet werden, auf das Ereignis deklariert wird, auf das zugeordnete Feld (wenn das Ereignis nicht abstrakt ist) oder den zugehörigen hinzufügen und entfernen-Methoden. In Ermangelung einer *Attribute_target_specifier*, das Attribut gilt für das Ereignis. Das Vorhandensein der `event` *Attribute_target_specifier* gibt an, dass das Attribut für das Ereignis gilt das Vorhandensein der `field` *Attribute_target_specifier* angibt Das Attribut auf das Feld angewendet werden; und das Vorhandensein der `method` *Attribute_target_specifier* gibt an, dass das Attribut für die Methoden gilt.
*  Ein Attribut auf eine Get-Zugriffsmethoden-Deklaration für eine Eigenschaft oder der Indexer-Deklaration angegebenen kann entweder auf die zugehörige Methode oder ihren Rückgabewert anwenden. In Ermangelung einer *Attribute_target_specifier*, das Attribut gilt für die Methode. Das Vorhandensein der `method` *Attribute_target_specifier* gibt an, dass das Attribut für die Methode gilt das Vorhandensein der `return` *Attribute_target_specifier* angibt Das Attribut auf den Rückgabewert angewendet werden soll.
*  Ein Attribut für einen Set-Accessor für eine Eigenschaft oder der Indexer-Deklaration angegeben kann entweder auf die zugehörige Methode oder als einzelne implizite Parameter angewendet werden. In Ermangelung einer *Attribute_target_specifier*, das Attribut gilt für die Methode. Das Vorhandensein der `method` *Attribute_target_specifier* gibt an, dass das Attribut für die Methode gilt das Vorhandensein der `param` *Attribute_target_specifier* angibt Das Attribut für den Parameter angewendet werden; das Vorhandensein der `return` *Attribute_target_specifier* gibt an, dass das Attribut auf den Rückgabewert angewendet wird.
*  Ein Attribut auf ein Add- oder Remove-Zugriffsmethoden-Deklaration angegeben werden, für eine Ereignisdeklaration entweder auf die zugehörige Methode oder als einzelne Parameter angewendet werden kann. In Ermangelung einer *Attribute_target_specifier*, das Attribut gilt für die Methode. Das Vorhandensein der `method` *Attribute_target_specifier* gibt an, dass das Attribut für die Methode gilt das Vorhandensein der `param` *Attribute_target_specifier* angibt Das Attribut für den Parameter angewendet werden; das Vorhandensein der `return` *Attribute_target_specifier* gibt an, dass das Attribut auf den Rückgabewert angewendet wird.

In anderen Kontexten Einbeziehung einer *Attribute_target_specifier* ist zulässig, aber nicht erforderlich. Z. B. eine Klassendeklaration kann entweder eingeschlossen oder weglassen den Spezifizierer `type`:

```csharp
[type: Author("Brian Kernighan")]
class Class1 {}

[Author("Dennis Ritchie")]
class Class2 {}
```

Es ist ein Fehler an eine ungültige *Attribute_target_specifier*. Z. B. der Spezifizierer `param` kann nicht in einer Klassendeklaration verwendet werden:

```csharp
[param: Author("Brian Kernighan")]        // Error
class Class1 {}
```

Attributklassen heißen gemäß der Konvention, mit dem Suffix `Attribute`. Ein *Attribute_name* des Formulars *Type_name* kann entweder ein- oder lassen Sie das Suffix. Wenn eine Attributklasse mit und ohne das Suffix gefunden wird, eine Mehrdeutigkeit vorliegt, und führt ein Fehler während der Kompilierung. Wenn der *Attribute_name* geschrieben ist, dass der äußersten *Bezeichner* wird als ausführlicher Bezeichner ([Bezeichner](lexical-structure.md#identifiers)), und klicken Sie dann nur ein Attribut ohne Suffix übereinstimmt, sodass solche Mehrdeutigkeiten aufgelöst werden. Im Beispiel

```csharp
using System;

[AttributeUsage(AttributeTargets.All)]
public class X: Attribute
{}

[AttributeUsage(AttributeTargets.All)]
public class XAttribute: Attribute
{}

[X]                     // Error: ambiguity
class Class1 {}

[XAttribute]            // Refers to XAttribute
class Class2 {}

[@X]                    // Refers to X
class Class3 {}

[@XAttribute]           // Refers to XAttribute
class Class4 {}
```

zeigt zwei Attributklassen, die mit dem Namen `X` und `XAttribute`. Das Attribut `[X]` ist mehrdeutig, da er entweder verweisen könnte `X` oder `XAttribute`. Als ausführlichen Bezeichner können die genaue Absicht in diesen seltenen Fällen angegeben werden. Das Attribut `[XAttribute]` ist nicht mehrdeutig (obwohl es der Fall wäre, wenn es eine Attributklasse, die mit dem Namen wurde `XAttributeAttribute`!). Wenn die Deklaration für Klasse `X` wird entfernt, und klicken Sie dann beide Attribute auf die Attributklasse, die mit dem Namen verweisen `XAttribute`wie folgt:

```csharp
using System;

[AttributeUsage(AttributeTargets.All)]
public class XAttribute: Attribute
{}

[X]                     // Refers to XAttribute
class Class1 {}

[XAttribute]            // Refers to XAttribute
class Class2 {}

[@X]                    // Error: no attribute named "X"
class Class3 {}
```

Es ist ein Fehler während der Kompilierung, um eine Single-Use-Attribut mehr als einmal auf die gleiche Entität verwendet werden. Im Beispiel

```csharp
using System;

[AttributeUsage(AttributeTargets.Class)]
public class HelpStringAttribute: Attribute
{
    string value;

    public HelpStringAttribute(string value) {
        this.value = value;
    }

    public string Value {
        get {...}
    }
}

[HelpString("Description of Class1")]
[HelpString("Another description of Class1")]
public class Class1 {}
```

Führt einen Kompilierzeitfehler, da versucht wird, verwenden Sie `HelpString`, d.h. eine Single-Use-Attribut-Klasse, mehr als einmal in der Deklaration der `Class1`.

Ein Ausdruck `E` ist ein *Attribute_argument_expression* Wenn alle der folgenden Aussagen zutreffen:

*  Der Typ des `E` ein Attributparametertyps ist ([Parameter Attributtypen](attributes.md#attribute-parameter-types)).
*  Zum Zeitpunkt der Kompilierung, den Wert der `E` aufgelöst werden kann, um einen der folgenden:
   * Ein konstanter Wert.
   * Ein `System.Type`-Objekt.
   * Ein eindimensionales Array von *Attribute_argument_expression*s.

Zum Beispiel:

```csharp
using System;

[AttributeUsage(AttributeTargets.Class)]
public class TestAttribute: Attribute
{
    public int P1 {
        get {...}
        set {...}
    }

    public Type P2 {
        get {...}
        set {...}
    }

    public object P3 {
        get {...}
        set {...}
    }
}

[Test(P1 = 1234, P3 = new int[] {1, 3, 5}, P2 = typeof(float))]
class MyClass {}
```

Ein *Typeof_expression* ([der Typeof-Operator](expressions.md#the-typeof-operator)) verwendet, wie ein Argumentausdruck Attribut kann ein nicht generischer Typ, ein geschlossener konstruierter Typ oder einen ungebundenen generischen Typ verweisen, aber sie kann keine verweisen eine Öffnen Sie die geben. Dadurch wird sichergestellt, dass der Ausdruck kann, während der Kompilierung aufgelöst werden.

```csharp
class A: Attribute
{
    public A(Type t) {...}
}

class G<T>
{
    [A(typeof(T))] T t;                  // Error, open type in attribute
}

class X
{
    [A(typeof(List<int>))] int x;        // Ok, closed constructed type
    [A(typeof(List<>))] int y;           // Ok, unbound generic type
}
```

## <a name="attribute-instances"></a>Attributinstanzen

Ein ***Attributinstanz*** ist eine Instanz, die ein Attribut zur Laufzeit darstellt. Ein Attribut mit positionellen Argumenten eine Attributklasse definiert und benannte Argumente. Eine Attributinstanz ist eine Instanz der Attributklasse, die mit benannten und Positionsargumenten Argumenten initialisiert wird.

Abrufen einer Instanz des umfasst sowohl während der Kompilierung und Laufzeit-Verarbeitung, wie in den folgenden Abschnitten beschrieben.

### <a name="compilation-of-an-attribute"></a>Ein Attribut für die Quellcodekompilierung

Die Kompilierung von einer *Attribut* mit Attributklasse `T`, *Positional_argument_list* `P` und *Named_argument_list* `N`, umfasst die folgenden Schritte aus:

*  Führen Sie die während der Kompilierung Verarbeitungsschritte für die Kompilierung einer *Object_creation_expression* des Formulars `new T(P)`. Diese Schritte führen zu einem Fehler während der Kompilierung oder bestimmen Instanzkonstruktor `C` auf `T` , die zur Laufzeit aufgerufen werden kann.
*  Wenn `C` keinen öffentlichen Zugriff und dann ein Fehler während der Kompilierung auftritt.
*  Für jede *Named_argument* `Arg` in `N`:
   * Lassen Sie `Name` werden die *Bezeichner* von der *Named_argument* `Arg`.
   * `Name` muss das Identifizieren von nicht statischen Lese-/ Schreibzugriff öffentliche Felder oder Eigenschaften auf `T`. Wenn `T` keine solche Feld oder einer Eigenschaft hat, und klicken Sie dann ein Fehler während der Kompilierung auftritt.
*  Behalten Sie die folgende Informationen für die Laufzeitinstanziierung des Attributs: die Attributklasse `T`, den Instanzkonstruktor `C` auf `T`, *Positional_argument_list* `P` und die *Named_argument_list* `N`.

### <a name="run-time-retrieval-of-an-attribute-instance"></a>Abrufen der Laufzeit eine Instanz des

Kompilierung von einer *Attribut* führt zu eine Attributklasse `T`, Instanzkonstruktor `C` auf `T`, *Positional_argument_list* `P`, und ein *Named_argument_list* `N`. Mit diesen Informationen kann zur Laufzeit mit den folgenden Schritten eine Attributinstanz abgerufen werden:

*  Führen Sie die laufzeitverarbeitung Schritte für die Ausführung einer *Object_creation_expression* des Formulars `new T(P)`, mit dem Instanzkonstruktor `C` , die zum Zeitpunkt der Kompilierung festgelegt. Diese Schritte wird eine Ausnahme ausgelöst oder erzeugen eine Instanz `O` von `T`.
*  Für jede *Named_argument* `Arg` in `N`, in der Reihenfolge:
   * Lassen Sie `Name` werden die *Bezeichner* von der *Named_argument* `Arg`. Wenn `Name` identifiziert keine nicht statischen öffentlichen Lese-/ Schreibzugriff Felder oder Eigenschaften auf `O`, und klicken Sie dann eine Ausnahme ausgelöst wird.
   * Lassen Sie `Value` werden das Ergebnis der Auswertung der *Attribute_argument_expression* von `Arg`.
   * Wenn `Name` gibt ein Feld auf `O`, legen Sie dieses Feld auf `Value`.
   * Andernfalls `Name` gibt eine Eigenschaft auf `O`. Legen Sie diese Eigenschaft auf `Value`.
   * Das Ergebnis ist `O`, eine Instanz der Attributklasse `T` , wurde mit initialisiert die *Positional_argument_list* `P` und *Named_argument_list* `N`.

## <a name="reserved-attributes"></a>Reservierte Attribute

Eine kleine Anzahl von Attributen Auswirkungen auf die Sprache in irgendeiner Form. Diese Attribute enthalten:

*  `System.AttributeUsageAttribute` ([Das AttributeUsage-Attribut](attributes.md#the-attributeusage-attribute)), womit die Möglichkeiten beschrieben, in denen eine Attributklasse verwendet werden kann.
*  `System.Diagnostics.ConditionalAttribute` ([Das Conditional-Attribut](attributes.md#the-conditional-attribute)), womit bedingte Methoden definieren.
*  `System.ObsoleteAttribute` ([Das Obsolete-Attribut](attributes.md#the-obsolete-attribute)), die wird verwendet, um ein Element als veraltet zu markieren.
*  `System.Runtime.CompilerServices.CallerLineNumberAttribute`, `System.Runtime.CompilerServices.CallerFilePathAttribute` und `System.Runtime.CompilerServices.CallerMemberNameAttribute` ([Aufrufer-Informationsattribute](attributes.md#caller-info-attributes)), die zum Bereitstellen von Informationen über den aufrufenden Kontext auf optionale Parameter verwendet werden.

### <a name="the-attributeusage-attribute"></a>Das AttributeUsage-Attribut

Das Attribut `AttributeUsage` wird verwendet, um die Art und Weise zu beschreiben, in denen die Attributklasse verwendet werden kann.

Eine Klasse, die mit versehen ist die `AttributeUsage` Attribut ableiten muss `System.Attribute`, entweder direkt oder indirekt. Andernfalls tritt ein Kompilierungsfehler auf.

```csharp
namespace System
{
    [AttributeUsage(AttributeTargets.Class)]
    public class AttributeUsageAttribute: Attribute
    {
        public AttributeUsageAttribute(AttributeTargets validOn) {...}
        public virtual bool AllowMultiple { get {...} set {...} }
        public virtual bool Inherited { get {...} set {...} }
        public virtual AttributeTargets ValidOn { get {...} }
    }

    public enum AttributeTargets
    {
        Assembly     = 0x0001,
        Module       = 0x0002,
        Class        = 0x0004,
        Struct       = 0x0008,
        Enum         = 0x0010,
        Constructor  = 0x0020,
        Method       = 0x0040,
        Property     = 0x0080,
        Field        = 0x0100,
        Event        = 0x0200,
        Interface    = 0x0400,
        Parameter    = 0x0800,
        Delegate     = 0x1000,
        ReturnValue  = 0x2000,

        All = Assembly | Module | Class | Struct | Enum | Constructor | 
            Method | Property | Field | Event | Interface | Parameter | 
            Delegate | ReturnValue
    }
}
```

### <a name="the-conditional-attribute"></a>Das Conditional-Attribut

Das Attribut `Conditional` ermöglicht die Definition von ***bedingte Methoden*** und ***conditional-Attribut von Klassen***.

```csharp
namespace System.Diagnostics
{
    [AttributeUsage(AttributeTargets.Method | AttributeTargets.Class, AllowMultiple = true)]
    public class ConditionalAttribute: Attribute
    {
        public ConditionalAttribute(string conditionString) {...}
        public string ConditionString { get {...} }
    }
}
```

#### <a name="conditional-methods"></a>Bedingte Methoden

Eine Methode mit ergänzt die `Conditional` -Attribut ist eine bedingte Methode. Die `Conditional` -Attribut gibt eine Bedingung an, durch ein Symbol für bedingte Kompilierung zu testen. Aufrufe an eine bedingte Methode entweder eingeschlossen oder ausgelassen wird, je nachdem, ob dieses Symbol, zum Zeitpunkt des Aufrufs definiert ist. Wenn das Symbol definiert ist, wird der Aufruf einbezogen; Andernfalls wird der Aufruf (einschließlich der Auswertung des Empfängers und Parameter des Aufrufs) weggelassen.

Eine bedingte Methode ist jedoch mit folgenden Einschränkungen:

*  Die bedingte Methode muss eine Methode in einer *Class_declaration* oder *Struct_declaration*. Ein Fehler während der Kompilierung tritt auf, wenn die `Conditional` -Attribut für eine Methode in einer Schnittstellendeklaration angegeben ist.
*  Die bedingte Methode muss einen Rückgabetyp verfügen `void`.
*  Die bedingte Methode muss nicht mit markiert werden die `override` Modifizierer. Eine bedingte Methode markiert werden kann, mit der `virtual` Modifizierer jedoch. Außerkraftsetzungen einer solchen Methode sind implizit bedingt und müssen nicht explizit markiert werden mit einem `Conditional` Attribut.
*  Die bedingte Methode muss eine Implementierung einer Schnittstellenmethode nicht. Andernfalls tritt ein Kompilierungsfehler auf.

Darüber hinaus tritt ein Kompilierungsfehler auf, wenn eine bedingte Methode, in verwendet wird einem *Delegate_creation_expression*. Im Beispiel

```csharp
#define DEBUG

using System;
using System.Diagnostics;

class Class1 
{
    [Conditional("DEBUG")]
    public static void M() {
        Console.WriteLine("Executed Class1.M");
    }
}

class Class2
{
    public static void Test() {
        Class1.M();
    }
}
```

deklariert `Class1.M` als eine bedingte Methode. `Class2`die `Test` -Methode ruft diese Methode. Da das konditionale kompiliersymbol `DEBUG` definiert ist, wenn `Class2.Test` wird aufgerufen, ruft sie `M`. Wenn das Symbol `DEBUG` hatte nicht definiert wurde, klicken Sie dann `Class2.Test` nicht aufruft `Class1.M`.

Es ist wichtig zu beachten, dass der Einschluss oder Ausschluss eines Aufrufs einer bedingten Methode durch die Symbole für bedingte Kompilierung zum Zeitpunkt des Aufrufs gesteuert wird. Im Beispiel

Datei `class1.cs`:

```csharp
using System.Diagnostics;

class Class1 
{
    [Conditional("DEBUG")]
    public static void F() {
        Console.WriteLine("Executed Class1.F");
    }
}
```

Datei `class2.cs`:

```csharp
#define DEBUG

class Class2
{
    public static void G() {
        Class1.F();                // F is called
    }
}
```

Datei `class3.cs`:

```csharp
#undef DEBUG

class Class3
{
    public static void H() {
        Class1.F();                // F is not called
    }
}
```

die Klassen `Class2` und `Class3` jeder enthält Aufrufe der bedingten Methode `Class1.F`, die bedingte basiert auf angibt, ob `DEBUG` definiert ist. Da dieses Symbol, im Kontext des definiert ist `Class2` , nicht jedoch `Class3`, den Aufruf von `F` in `Class2` enthalten ist, während der Aufruf von `F` in `Class3` weggelassen wird.

Die Verwendung von bedingten Methoden in einer Vererbungskette kann verwirrend sein. Aufrufe an eine bedingte Methode über `base`, des Formulars `base.M`, gelten die Regeln der normale bedingte Methode aufrufen. Im Beispiel

Datei `class1.cs`:

```csharp
using System;
using System.Diagnostics;

class Class1 
{
    [Conditional("DEBUG")]
    public virtual void M() {
        Console.WriteLine("Class1.M executed");
    }
}
```

Datei `class2.cs`:

```csharp
using System;

class Class2: Class1
{
    public override void M() {
        Console.WriteLine("Class2.M executed");
        base.M();                        // base.M is not called!
    }
}
```

Datei `class3.cs`:

```csharp
#define DEBUG

using System;

class Class3
{
    public static void Test() {
        Class2 c = new Class2();
        c.M();                            // M is called
    }
}
```

`Class2` enthält einen Aufruf an die `M` in seiner Basisklasse definiert. Dieser Aufruf wird ausgelassen, da die Basismethode bedingte basierend auf dem Vorhandensein des Symbols wird `DEBUG`, dies ist nicht definiert. Daher wird die Methode an die Konsole schreibt "`Class2.M executed`" nur. Umsichtiger Verwendung von *Pp_declaration*s kann derartige Probleme beseitigen.

#### <a name="conditional-attribute-classes"></a>Conditional-Attribut von Klassen

Eine Attributklasse ([Attributklassen](attributes.md#attribute-classes)) mit einem oder mehreren ergänzt `Conditional` Attribute ist ein ***conditional-Attribut-Klasse***. Eine conditional-Attribut-Klasse ist mit der Symbole für bedingte Kompilierung in deklarierten daher zugeordneten seine `Conditional` Attribute. In diesem Beispiel:

```csharp
using System;
using System.Diagnostics;
[Conditional("ALPHA")]
[Conditional("BETA")]
public class TestAttribute : Attribute {}
```

deklariert `TestAttribute` als ein conditional-Attribut-Klasse, die die Symbole für bedingte Kompilierungen zugeordneten `ALPHA` und `BETA`.

Attribut-Spezifikationen ([Attributspezifikation](attributes.md#attribute-specification)) des ein conditional-Attribut sind enthalten, wenn mindestens eines der zugeordneten konditionale Kompilierungssymbole andernfalls zum Zeitpunkt der-Spezifikation definiert ist das Attribut Spezifikation wird weggelassen.

Es ist wichtig zu beachten, dass der Einschluss oder Ausschluss eine Attributspezifikation einer Klasse conditional-Attribut durch die Symbole für bedingte Kompilierung zum Zeitpunkt der Spezifikation gesteuert wird. Im Beispiel

Datei `test.cs`:

```csharp
using System;
using System.Diagnostics;

[Conditional("DEBUG")]

public class TestAttribute : Attribute {}
```

Datei `class1.cs`:

```csharp
#define DEBUG

[Test]                // TestAttribute is specified

class Class1 {}
```

Datei `class2.cs`:

```csharp
#undef DEBUG

[Test]                 // TestAttribute is not specified

class Class2 {}
```

die Klassen `Class1` und `Class2` sind jeweils mit-Attribut versehen `Test`, die bedingte basiert auf angibt, ob `DEBUG` definiert ist. Da im Rahmen dieses Symbol definiert ist `Class1` aber nicht `Class2`, die Spezifikation des der `Test` -Attribut `Class1` enthalten ist, während die Spezifikation des der `Test` -Attribut `Class2` weggelassen wird.

### <a name="the-obsolete-attribute"></a>Das Obsolete-Attribut

Das Attribut `Obsolete` wird verwendet, um die Typen und Member von Typen, die nicht mehr verwendet werden soll.

```csharp
namespace System
{
    [AttributeUsage(
        AttributeTargets.Class | 
        AttributeTargets.Struct |
        AttributeTargets.Enum | 
        AttributeTargets.Interface | 
        AttributeTargets.Delegate |
        AttributeTargets.Method | 
        AttributeTargets.Constructor |
        AttributeTargets.Property | 
        AttributeTargets.Field |
        AttributeTargets.Event,
        Inherited = false)
    ]
    public class ObsoleteAttribute: Attribute
    {
        public ObsoleteAttribute() {...}
        public ObsoleteAttribute(string message) {...}
        public ObsoleteAttribute(string message, bool error) {...}
        public string Message { get {...} }
        public bool IsError { get {...} }
    }
}
```

Wenn ein Programm verwendet werden soll, einen Typ oder Member, die mit ergänzt wird die `Obsolete` -Attribut, gibt der Compiler eine Warnung oder einen Fehler. Insbesondere gibt der Compiler eine Warnung, wenn keine Fehlerparameter angegeben wird, oder wenn der Fehlerparameter angegeben wird, und hat den Wert `false`. Der Compiler gibt einen Fehler aus, wenn der Fehlerparameter angegeben ist, und den Wert hat `true`.

Im Beispiel

```csharp
[Obsolete("This class is obsolete; use class B instead")]
class A
{
    public void F() {}
}

class B
{
    public void F() {}
}

class Test
{
    static void Main() {
        A a = new A();         // Warning
        a.F();
    }
}
```

die Klasse `A` ergänzt wird, mit der `Obsolete` Attribut. Jede Verwendung von `A` in `Main` löst eine Warnung, die die angegebene Meldung enthält, "Diese Klasse ist veraltet; Stattdessen Sie Klasse B."

### <a name="caller-info-attributes"></a>Aufruferinformationsattribute

Für Zwecke, z. B. Protokollierung und berichterstellung ist es manchmal sinnvoll für ein Funktionselement zum Abrufen bestimmter Informationen während der Kompilierung zum aufrufenden Code. Die Attribute "callerinfo" bieten eine Möglichkeit, solche Informationen transparent zu übergeben.

Wenn ein optionaler Parameter mit einem der Attribute "callerinfo" kommentiert wird, bewirkt das Auslassen von des entsprechenden Arguments in einem Aufruf nicht, unbedingt den standardmäßige Parameterwert ersetzt werden. Wenn die angegebene Informationen zu den aufrufenden Kontext verfügbar ist, wird diese Informationen stattdessen als Wert des Arguments übergeben werden.

Zum Beispiel:

```csharp
using System.Runtime.CompilerServices

...

public void Log(
    [CallerLineNumber] int line = -1,
    [CallerFilePath]   string path = null,
    [CallerMemberName] string name = null
)
{
    Console.WriteLine((line < 0) ? "No line" : "Line "+ line);
    Console.WriteLine((path == null) ? "No file path" : path);
    Console.WriteLine((name == null) ? "No member name" : name);
}
```

Ein Aufruf von `Log()` ohne Argumente wird drucken, die Zeile Anzahl und den Dateipfad des Aufrufs sowie der Name des Elements in dem der Aufruf aufgetreten ist.

Attribute "callerinfo" können auf eine beliebige Stelle, optionale Parameter auftreten, einschließlich Delegaten verwendet. Allerdings müssen die Attribute "callerinfo" bestimmte Einschränkungen für die Typen der Parameter, die sie Attribut können, damit es wird immer eine implizite Konvertierung von einem ersetzten Werts auf den Parametertyp vorhanden sein.

Es ist ein Fehler, wenn das gleiche Aufrufer Info-Attribut für einen Parameter sowohl die zum Definieren und Implementieren eines Teils der Deklaration einer partiellen Methode. Nur Attribute "callerinfo" in der definierenden Teil werden angewendet, während die Aufrufer-Informationsattribute, die nur in der implementierenden Teil auftreten ignoriert werden.

Aufruferinformationen hat keine Auswirkungen auf die überladungsauflösung. Wie die attributierte optionalen Parameter noch aus dem Quellcode des Aufrufers ausgelassen werden, ignoriert Auflösung von funktionsüberladungen dieser Parameter auf die gleiche Weise, die sie andere ausgelassenen optionale Parameter ignoriert ([Überladungsauflösung](expressions.md#overload-resolution)).

Aufruferinformationen wird nur ersetzt, wenn eine Funktion im Quellcode explizit aufgerufen wird. Implizite Aufrufe wie z. B. Konstruktoraufrufe implizite übergeordnete Element haben keine Quellort und Aufruferinformationen nicht ersetzt werden. Darüber hinaus werden Aufrufe, die dynamisch gebunden sind keine Aufruferinformationen ersetzen. Wenn ein Aufrufer-Informationsattribute, die in solchen Fällen attributierten Parameter ausgelassen wird, wird stattdessen der angegebene Standardwert des Parameters verwendet.

Eine Ausnahme ist die Abfrage-Ausdrücken. Gelten diese syntaktische Erweiterungen, und wenn die Aufrufe sie erweitert, um optionale Parameter mit der Attribute "callerinfo" weglassen, wird die Aufruferinformationen ersetzt werden. Der Speicherort ist der Speicherort der Abfrageklausel, die der Aufruf von generiert wurde.

Wenn mehr als einem Aufrufer Info-Attribut auf einen bestimmten Parameter angegeben wird, werden sie in der folgenden Reihenfolge bevorzugt: `CallerLineNumber`, `CallerFilePath`, `CallerMemberName`.

#### <a name="the-callerlinenumber-attribute"></a>Das Attribut CallerLineNumber

Die `System.Runtime.CompilerServices.CallerLineNumberAttribute` ist für optionale Parameter zulässig, wenn eine implizite standardkonvertierung besteht ([Standard implizite Konvertierungen](conversions.md#standard-implicit-conversions)) aus den konstanten Wert `int.MaxValue` auf den Typ des Parameters. Dadurch wird sichergestellt, dass eine positive Zeilennummer bis zu diesem Wert kann, ohne Fehler übergeben werden.

Wenn ein Funktionsaufruf über einen Ort im Quellcode mit einen optionalen Parameter lässt die `CallerLineNumberAttribute`, und klicken Sie dann ein numerisches Literal, das die Nummer der Zeile des Speicherorts darstellt, die als Argument für den Aufruf statt des Standardwerts für den Parameter verwendet wird.

Wenn der Aufruf mehrere Zeilen erstreckt, ist die ausgewählte Zeile implementierungsabhängig.

Beachten Sie, die die Nummer der Zeile betroffen werden möglicherweise `#line` Direktiven ([Zeile Direktiven](lexical-structure.md#line-directives)).

#### <a name="the-callerfilepath-attribute"></a>Das Attribut CallerFilePath

Die `System.Runtime.CompilerServices.CallerFilePathAttribute` ist für optionale Parameter zulässig, wenn eine implizite standardkonvertierung besteht ([Standard implizite Konvertierungen](conversions.md#standard-implicit-conversions)) von `string` auf den Typ des Parameters.

Wenn ein Funktionsaufruf über einen Ort im Quellcode mit einen optionalen Parameter lässt die `CallerFilePathAttribute`, und klicken Sie dann ein Zeichenfolgenliteral, des Speicherorts Dateipfad darstellt, die als Argument für den Aufruf statt des Standardwerts für den Parameter verwendet wird.

Das Format des Dateipfads ist implementierungsabhängig.

Beachten Sie, die der Dateipfad von betroffen sein könnten `#line` Direktiven ([Zeile Direktiven](lexical-structure.md#line-directives)).

#### <a name="the-callermembername-attribute"></a>Das CallerMemberName-Attribut

Die `System.Runtime.CompilerServices.CallerMemberNameAttribute` ist für optionale Parameter zulässig, wenn eine implizite standardkonvertierung besteht ([Standard implizite Konvertierungen](conversions.md#standard-implicit-conversions)) von `string` auf den Typ des Parameters.

Wenn das Funktionselement selbst oder der Rückgabetyp, Parameter oder Typparameter in ein Funktionsaufruf in einen Speicherort innerhalb des Texts ein Funktionsmember oder innerhalb eines Attributs angewendet lässt Quellcode einen optionalen Parameter mit der `CallerMemberNameAttribute`, ein Zeichenfolgenliteral, die den Namen dieses Elements wird als Argument für den Aufruf statt des Standardwerts für den Parameter verwendet.

Aufrufe, die innerhalb generischer Methoden auftreten, wird nur der Methodennamen selbst, ohne die Liste der Typparameter verwendet.

Aufrufe, die in der expliziten Implementierungen eines Schnittstellenmembers auftreten, wird nur der Methodennamen selbst, ohne die Kennzeichnung durch einen vorherigen Schnittstelle verwendet.

Für Aufrufe, die in Eigenschaften- oder Ereignisaccessoren auftreten, ist der Elementname verwendet, die der Eigenschaft oder Ereignis selbst.

Aufrufe, die in Indexeraccessoren auftreten, der Namen des Members verwendet wird, angegeben durch eine `IndexerNameAttribute` ([das IndexerName-Attribut](attributes.md#the-indexername-attribute)) auf das Indexer-Element, sofern vorhanden, oder den Standardnamen `Item` andernfalls.

Aufrufe, die auftreten, in den Deklarationen von Instanzkonstruktoren, statischen Konstruktoren, Destruktoren und Operatoren den Member ist Name abhängig von der Implementierung.

## <a name="attributes-for-interoperation"></a>Attribute für die Interoperation

Hinweis: Dieser Abschnitt gilt nur für die Microsoft .NET Implementierung C#.

### <a name="interoperation-with-com-and-win32-components"></a>Interoperation mit COM- und Win32-Komponenten

Die Laufzeit .NET bietet eine große Anzahl von Attributen, die C#-Programme für die Zusammenarbeit mit Komponenten geschrieben wurden, mithilfe von COM und Win32-DLLs zu ermöglichen. Z. B. die `DllImport` Attribut kann verwendet werden, auf eine `static extern` Methode, um anzugeben, dass die Implementierung der Methode in einer Win32-DLL gefunden werden. Diese Attribute finden Sie in der `System.Runtime.InteropServices` -Namespace, und eine detaillierte Dokumentation für diese Attribute in der Dokumentation von .NET Common Language Runtime befindet.

### <a name="interoperation-with-other-net-languages"></a>Interoperation mit anderen .NET-Sprachen

#### <a name="the-indexername-attribute"></a>Das IndexerName-Attribut

Indexer sind in .NET unter Verwendung von indizierten Eigenschaften implementiert, und einen Namen in den Metadaten für .NET. Wenn kein `IndexerName` Attribut ist für einen Indexer, und klicken Sie dann auf den Namen `Item` wird standardmäßig verwendet. Die `IndexerName` -Attribut ermöglicht dem Entwickler diesen Standardwert überschreiben und einen anderen Namen angeben.

```csharp
namespace System.Runtime.CompilerServices.CSharp
{
    [AttributeUsage(AttributeTargets.Property)]
    public class IndexerNameAttribute: Attribute
    {
        public IndexerNameAttribute(string indexerName) {...}
        public string Value { get {...} } 
    }
}
```
