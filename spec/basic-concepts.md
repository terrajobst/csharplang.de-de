# <a name="basic-concepts"></a>Grundlegende Konzepte

## <a name="application-startup"></a>Anwendungsstart

Eine Assembly, ein ***Einstiegspunkt*** heißt ein ***Anwendung***. Wenn eine Anwendung wird ausgeführt werden, ein neues ***Anwendungsdomäne*** erstellt wird. Mehrere verschiedene Instanziierungen von einer Anwendung möglicherweise gleichzeitig auf demselben Computer vorhanden, und jede hat eigene Anwendungsdomäne.

Eine Anwendungsdomäne kann Anwendungsisolation durch fungiert als Container für den Anwendungszustand. Eine Anwendungsdomäne fungiert als Container und Begrenzung der Typen, die in der Anwendung und die Klassenbibliotheken verwendeten definiert. In einer Anwendungsdomäne geladene Typen unterscheiden sich von den gleichen Typ, der in einer anderen Anwendungsdomäne geladen und Instanzen von Objekten zwischen Anwendungsdomänen nicht direkt freigegeben werden. Beispielsweise jede Anwendungsdomäne verfügt über eine eigene Kopie statischer Variablen für diese Typen und für einen Typ ein statischer Konstruktor wird höchstens einmal pro Anwendungsdomäne ausgeführt. Implementierungen können implementierungsspezifische Richtlinie oder Mechanismen für die Erstellung und Zerstörung von Anwendungsdomänen bereit.

***Starten der Anwendung*** tritt auf, wenn die ausführungsumgebung eine angegebene Methode aufruft, die als Einstiegspunkt der Anwendung bezeichnet wird. Diese Einstiegspunktmethode wird stets der Name `Main`, und kann einen der folgenden Signaturen aufweisen:

```csharp
static void Main() {...}

static void Main(string[] args) {...}

static int Main() {...}

static int Main(string[] args) {...}
```

Wie gezeigt, kann optional der Einstiegspunkt Zurückgeben einer `int` Wert. Dadurch zurückgegeben Wert wird verwendet, Beenden der Anwendung ([Beenden der Anwendung](basic-concepts.md#application-termination)).

Der Einstiegspunkt kann optional einen formalen Parameter aufweisen. Der Parameter kann einen beliebigen Namen aufweisen, aber der Typ des Parameters muss `string[]`. Wenn der formale Parameter vorhanden ist, wird die ausführungsumgebung erstellt und übergibt eine `string[]` Argument mit den Befehlszeilenargumenten, die angegeben, wenn die Anwendung gestartet wurde. Die `string[]` Argument ist nie null, aber es möglicherweise eine Länge von 0 (null) auf, wenn keine Befehlszeilenargumente angegeben wurden.

Da c# das Überladen von Methoden unterstützt, kann eine Klasse oder Struktur mehrere Definitionen einer Methode enthalten, jeweils eine andere Signatur ist. Mit einem einzelnen Programm, keine Klasse oder Struktur kann jedoch enthalten mehr als eine Methode mit dem Namen `Main` , deren Definition kann es als Einstiegspunkt für die Anwendung verwendet werden kann. Andere überladenen Versionen der `Main` sind zulässig, aber sie haben mehr als einen Parameter oder ihr einziger Parameter ist als Typ `string[]`.

Eine Anwendung kann aus mehreren Klassen oder Strukturen bestehen. Es ist möglich, mehr als eine dieser Klassen oder Strukturen, die eine Methode namens enthalten `Main` , deren Definition kann es als Einstiegspunkt für die Anwendung verwendet werden kann. In solchen Fällen muss ein externer Mechanismus (z. B. eine Befehlszeilen-Compiler-Option) verwendet werden, wählen Sie eines der folgenden `Main` Methoden als Einstiegspunkt.

In c# muss jede Methode als Member einer Klasse oder Struktur definiert werden. Normalerweise die deklarierte Zugriffsart ([deklariert Barrierefreiheit](basic-concepts.md#declared-accessibility)) einer Methode richtet sich nach den Zugriffsmodifizierern ([Zugriffsmodifizierer](classes.md#access-modifiers)) in der Deklaration verwendet werden soll, und ebenso der deklarierte angegeben Zugriff auf einen Typ richtet sich nach der Zugriffsmodifizierer, die in der Deklaration angegeben. In der Reihenfolge für eine bestimmte Methode eines angegebenen Typs aufgerufen werden können müssen sowohl den Typ und das Element zugegriffen werden. Allerdings ist der Einstiegspunkt der Anwendung ein besonderer Fall. Insbesondere kann die ausführungsumgebung Einstiegspunkt der Anwendung, unabhängig von die deklarierte Zugriffsart und unabhängig von die deklarierte Zugriffsart von seiner einschließenden Typdeklarationen zugreifen.

Die Einstiegspunktmethode für die Anwendung möglicherweise nicht in der Deklaration einer generischen Klasse.

In jeder anderen Hinsicht Verhalten sich die einstiegspunktmethoden aus, wie diejenigen, die keine Eingangspunkte sind.

## <a name="application-termination"></a>Beenden der Anwendung

***Beenden der Anwendung*** übergibt die Steuerung an die ausführungsumgebung.

Wenn der Rückgabetyp der Anwendung ***Einstiegspunkt*** Methode `int`, der zurückgegebene Wert dient als der Anwendung ***Beendigungsstatuscode***. Dieser Code dient zum Zulassen der Kommunikation von Erfolg oder Fehler für die ausführungsumgebung.

Ist der Rückgabetyp der der Einstiegspunktmethode `void`, erreichen die rechte geschweifte Klammer (`}`) der beendet werden, die Methode oder Ausführen einer `return` -Anweisung, die keine Ausdruck hat einen Statuscode der Beendigung des führt `0`.

Vor der Beendigung einer Anwendung, Destruktoren für alle Objekte, die noch nicht über Garbage Collection bereinigt wurden aufgerufen werden, wenn eine solche Bereinigung unterdrückt wurde (durch einen Aufruf an die Bibliotheksmethode `GC.SuppressFinalize`, z. B.).

## <a name="declarations"></a>Deklarationen

Deklarationen in einem C#-Programm definieren Sie die einzelnen Elemente des Programms. C#-Programme werden mithilfe von Namespaces organisiert ([Namespaces](namespaces.md)), Typ enthalten kann Deklarationen und geschachtelten Namespace-Deklarationen. Typdeklarationen ([Typdeklarationen](namespaces.md#type-declarations)) werden verwendet, um Klassen zu definieren ([Klassen](classes.md)), Strukturen ([Strukturen](structs.md)), Schnittstellen ([Schnittstellen](interfaces.md) ), Enumerationen ([Enumerationen](enums.md)), und Delegaten ([Delegaten](delegates.md)). Die Arten von Membern, die in einer Typdeklaration zulässig, abhängig von der Form der Deklaration des ab. Klassendeklarationen können z. B. Deklarationen, die für Konstanten enthalten ([Konstanten](classes.md#constants)), Felder ([Felder](classes.md#fields)), Methoden ([Methoden](classes.md#methods)), Eigenschaften ([ Eigenschaften](classes.md#properties)), Ereignisse ([Ereignisse](classes.md#events)), Indexer ([Indexer](classes.md#indexers)), Operatoren ([Operatoren](classes.md#operators)), Instanzkonstruktoren ([ Instanzkonstruktoren](classes.md#instance-constructors)), statische Konstruktoren ([statische Konstruktoren](classes.md#static-constructors)), Destruktoren ([Destruktoren](classes.md#destructors)), und geschachtelte Typen ([geschachtelte Typen](classes.md#nested-types)).

Eine Deklaration definiert einen Namen in der ***Deklarationsabschnitt*** , der die Deklaration angehört. Mit Ausnahme von überladenen Membern ([Signaturen und überladen](basic-concepts.md#signatures-and-overloading)), es ist ein Fehler während der Kompilierung, um zwei oder mehr Deklarationen zu erhalten, die Member mit demselben Namen in einem Deklarationsabschnitt einführen. Es ist nicht möglich, dass eine Deklarationsabschnitt auf verschiedene Arten von Membern mit dem gleichen Namen enthalten. Beispielsweise kann ein Deklarationsabschnitt nie ein Feld und eine Methode mit dem gleichen Namen enthalten.

Es gibt verschiedene Arten von Deklaration Leerzeichen, wie im folgenden beschrieben.

*  In allen Quelldateien von einem Programm *Namespace_member_declaration*mit ohne einschließenden *Namespace_declaration* sind Mitglieder einer einzelnen kombinierten Deklarationsabschnitts wird aufgerufen, die ***globale Deklarationsabschnitt***.
*  In allen Quelldateien von einem Programm *Namespace_member_declaration*e in *Namespace_declaration*s, die den gleichen Namen für den vollqualifizierten Namespace haben sind Mitglieder einer einzelnen kombinierten Deklaration Speicherplatz.
*  Jede Klasse, Struktur oder Schnittstellendeklaration erstellt einen neuen Deklarationsabschnitt. Namen werden in diesen Deklarationsabschnitt über eingeführt *Class_member_declaration*s, *Struct_member_declaration*s, *Interface_member_declaration*s, oder *Type_parameter*s. Mit Ausnahme von überladenen Instanzkonstruktor darf keine Deklarationen und statischen Konstruktor Deklarationen, eine Klasse oder Struktur eine Memberdeklaration mit dem gleichen Namen wie die Klasse oder Struktur enthalten. Eine Klasse, Struktur oder Schnittstelle ermöglicht die Deklaration der überladenen Methoden und Indexer. Darüber hinaus ermöglicht einer Klasse oder Struktur, die Deklaration der der überladenen Instanzkonstruktoren und Operatoren. Z. B. eine Klasse, Struktur oder Schnittstelle darf mehrere Methodendeklarationen mit demselben Namen, sofern diese Methodendeklarationen in deren Signatur unterscheiden ([Signaturen und überladen](basic-concepts.md#signatures-and-overloading)). Beachten Sie, dass Basisklassen tragen nicht zum Deklarationsbereich einer Klasse Basisschnittstellen tragen nicht zum Deklarationsbereich einer Schnittstelle. Daher ist eine abgeleitete Klasse oder Schnittstelle zulässig, um ein Element mit dem gleichen Namen wie für einen geerbten Member zu deklarieren. Solcher Member gilt als ***ausblenden*** geerbten Members.
*  Jede Delegatdeklaration erstellt einen neuen Deklarationsabschnitt. Namen werden durch formale Parameter in diesen eingeführt (*Fixed_parameter*s und *Parameter_array*s) und *Type_parameter*s.
*  Alle Enumerationsdeklarationen erstellt einen neuen Deklarationsabschnitt. Namen werden in diesen Deklarationsabschnitt über eingeführt *Enum_member_declarations*.
*  Jede Deklaration der Methode, Indexerdeklaration, Operatordeklaration, Instanz Konstruktordeklaration verwendet und anonyme Funktion erstellt einen neuen Deklarationsabschnitt wird aufgerufen, eine ***Deklaration lokaler Variablen Speicherplatz***. Namen werden durch formale Parameter in diesen eingeführt (*Fixed_parameter*s und *Parameter_array*s) und *Type_parameter*s. Der Text der Funktionsmember der Member oder eine anonyme Funktion, sofern vorhanden, gilt in der Deklaration lokaler Variablen Speicherplatz geschachtelt werden. Es ist ein Fehler für eine Deklaration lokaler Variablen und einer geschachtelten lokalen Variablendeklaration Speicherplatz Elemente mit dem gleichen Namen enthalten. Daher ist es in einer geschachtelten Deklarationsabschnitt nicht möglich, eine lokale Variable oder Konstante mit dem gleichen Namen wie eine lokale Variable oder Konstante in einem einschließenden Deklarationsbereich deklarieren. Es ist möglich, dass zwei Deklaration Leerzeichen Elemente mit dem gleichen Namen enthalten, solange weder Deklarationsabschnitt der anderen enthält.
*  Jede *Block* oder *Switch_block* , sowie ein *für*, *Foreach* und *mit* -Anweisung erstellt eine Deklaration lokaler Variablen den Speicherplatz für lokale Variablen und lokalen Konstanten. Namen werden in diesen Deklarationsabschnitt über eingeführt *Local_variable_declaration*s und *Local_constant_declaration*s. Beachten Sie, dass es sich bei der Deklaration lokaler Variablen Speicherplatz, die von diesen Funktionen für ihre Parameter deklariert Blöcke, die auftreten, als "oder" innerhalb des Texts einer Funktionsmember der Member oder eine anonyme Funktion geschachtelt sind. Daher ist es ein Fehler, z. B. eine Methode mit einer lokalen Variablen und Parameter mit dem gleichen Namen haben.
*  Jede *Block* oder *Switch_block* erstellt einen separaten Deklarationsabschnitt für Bezeichnungen. Namen werden in diesen Deklarationsabschnitt über eingeführt *Labeled_statement*s und die Namen sind auf die verwiesen wird durch *Goto_statement*s. Die ***Bezeichnung Deklarationsabschnitt*** eines Blocks enthält keine geschachtelten Blöcke. Daher ist es in einem geschachtelten Block nicht möglich, eine Bezeichnung mit dem gleichen Namen wie eine Bezeichnung in einem einschließenden Block deklarieren.

Die Reihenfolge im Text in der Namen deklariert werden, ist im Allgemeinen ohne Bedeutung. Insbesondere ist die Reihenfolge im Text nicht für die Deklaration und Verwendung von Namespaces, Konstanten, Methoden, Eigenschaften, Ereignisse, Indexer, Operatoren, Instanzkonstruktoren, Destruktoren, statische Konstruktoren und Typen von großer Bedeutung. Reihenfolge der Deklaration ist wichtig, es gibt folgende Möglichkeiten:

*  Die Reihenfolge der Deklaration für die Steuerelementfeld-Deklarationen und Deklarationen von lokalen Variablen bestimmt die Reihenfolge, in der ihre Initialisierer (sofern vorhanden) ausgeführt werden.
*  Lokale Variablen müssen definiert werden, bevor sie verwendet werden ([Bereiche](basic-concepts.md#scopes)).
*  Die Reihenfolge der Deklaration für Enum-Deklarationen ([Enumerationsmember](enums.md#enum-members)) ist wichtig, wenn *Constant_expression* -Werte weggelassen werden.

Deklarationsbereich eines Namespace ist "ohne feste Begrenzung", und zwei Deklarationen mit demselben vollqualifizierten Namen zum selben Deklarationsabschnitt beitragen. Beispiel:
```csharp
namespace Megacorp.Data
{
    class Customer
    {
        ...
    }
}

namespace Megacorp.Data
{
    class Order
    {
        ...
    }
}
```

die beiden oben genannten Namespacedeklarationen beitragen zum selben Deklarationsabschnitt, deklarieren zwei Klassen mit den vollqualifizierten Namen in diesem Fall `Megacorp.Data.Customer` und `Megacorp.Data.Order`. Da die beiden Deklarationen zum selben Deklarationsabschnitt beitragen möchten, haben sie würde ein Kompilierungsfehler verursacht, wenn jeweils eine Deklaration einer Klasse mit dem gleichen Namen enthalten.

Wie oben angegeben ist schließt der Deklaration eines Blocks keine geschachtelten Blöcke. Im folgenden Beispiel die `F` und `G` Methoden führen zu einem Fehler während der Kompilierung, da der Name `i` im äußeren-Block deklariert wird und nicht im inneren Block erneut deklariert werden. Allerdings die `H` und `I` Methoden sind gültig, da die beiden `i`des werden in separaten nicht geschachtelten Blöcken deklariert.

```csharp
class A
{
    void F() {
        int i = 0;
        if (true) {
            int i = 1;            
        }
    }

    void G() {
        if (true) {
            int i = 0;
        }
        int i = 1;                
    }

    void H() {
        if (true) {
            int i = 0;
        }
        if (true) {
            int i = 1;
        }
    }

    void I() {
        for (int i = 0; i < 10; i++)
            H();
        for (int i = 0; i < 10; i++)
            H();
    }
}
```

## <a name="members"></a>Member

Namespaces und Typen haben ***Mitglieder***. Die Elemente einer Entität sind allgemein verfügbar ist, durch die Verwendung von einem qualifizierten Namen, die beginnt mit einem Verweis auf die Entität, gefolgt von einem "`.`" token, gefolgt vom Namen des Members.

Member eines Typs deklariert werden entweder in der Typdeklaration oder ***geerbt*** von der Basisklasse des Typs. Wenn ein Typ von einer Basisklasse erbt, werden alle Member der Basisklasse, mit Ausnahme der Instanzkonstruktoren, Destruktoren und statischen Konstruktoren, die Member des abgeleiteten Typs. Die deklarierte Zugriffsart eines Members der Basisklasse nicht gesteuert, ob das Element geerbt wird, Vererbung erstreckt sich auch auf ein Element, das einen Instanzenkonstruktor, statischen Konstruktor oder Destruktor ist nicht. Allerdings ein geerbter Member kann nicht zugegriffen werden in einem abgeleiteten Typ, entweder aufgrund von die deklarierte Zugriffsart ([deklariert Barrierefreiheit](basic-concepts.md#declared-accessibility)) oder weil es mit einer Deklaration in den Typ selbst ausgeblendet wird ([durch Ausblenden Vererbung](basic-concepts.md#hiding-through-inheritance)).

### <a name="namespace-members"></a>Namespace-Elemente

Namespaces und Typen, die ohne einschließenden Namespace verfügen, sind Sie Mitglied der ***globalen Namespace***. Dies entspricht direkt den Namen, die im Bereich globale Deklaration deklariert.

Namespaces und Typen, die innerhalb eines Namespace deklariert sind Member dieses Namespace. Dies entspricht direkt den Namen, die im Deklarationsbereich des Namespaces deklariert.

Namespaces haben uneingeschränkten Zugriff. Es ist nicht möglich, privat, geschützt oder intern Namespaces zu deklarieren und Namespace-Namen sind immer öffentlich zugegriffen werden kann.

### <a name="struct-members"></a>Strukturmember

Die Member einer Struktur werden die Elemente, die in der Struktur deklariert und die von der Struktur der direkten Basisklasse geerbten Member `System.ValueType` und indirekte Basisklasse `object`.

Die Elemente eines einfachen Typs entsprechen direkt auf die Member der Struktur Typ mit einem Alias versehen vom einfachen Typ auf:

*  Die Elemente der `sbyte` sind die Mitglieder der `System.SByte` Struktur.
*  Die Elemente der `byte` sind die Mitglieder der `System.Byte` Struktur.
*  Die Elemente der `short` sind die Mitglieder der `System.Int16` Struktur.
*  Die Elemente der `ushort` sind die Mitglieder der `System.UInt16` Struktur.
*  Die Elemente der `int` sind die Mitglieder der `System.Int32` Struktur.
*  Die Elemente der `uint` sind die Mitglieder der `System.UInt32` Struktur.
*  Die Elemente der `long` sind die Mitglieder der `System.Int64` Struktur.
*  Die Elemente der `ulong` sind die Mitglieder der `System.UInt64` Struktur.
*  Die Elemente der `char` sind die Mitglieder der `System.Char` Struktur.
*  Die Elemente der `float` sind die Mitglieder der `System.Single` Struktur.
*  Die Elemente der `double` sind die Mitglieder der `System.Double` Struktur.
*  Die Elemente der `decimal` sind die Mitglieder der `System.Decimal` Struktur.
*  Die Elemente der `bool` sind die Mitglieder der `System.Boolean` Struktur.

### <a name="enumeration-members"></a>Enumerationsmember

Die Member einer Enumeration sind die Konstanten, die in der Enumeration und die von der Enumeration direkte Basisklasse geerbten Member `System.Enum` und der indirekten Basisklassen `System.ValueType` und `object`.

### <a name="class-members"></a>Klassenmember

Die Member einer Klasse sind Member, die in der Klasse deklariert und die von der Basisklasse geerbten Member (mit Ausnahme der Klasse `object` der hat keine Basisklasse). Die von der Basisklasse geerbten Member enthalten, die Konstanten, Felder, Methoden, Eigenschaften, Ereignisse, Indexer, Operatoren und Typen von der Basisklasse, jedoch nicht die Instanzkonstruktoren, Destruktoren und statische Konstruktoren der Basisklasse. Unabhängig von ihrem Zugriff werden die Member der Basisklasse geerbt.

Eine Klassendeklaration kann Deklarationen von Konstanten, Felder, Methoden, Eigenschaften, Ereignisse, Indexer, Operatoren, Instanzkonstruktoren, Destruktoren, statische Konstruktoren und Typen enthalten.

Die Elemente der `object` und `string` entsprechen direkt der Member der Klassentypen sie alias:

*  Die Elemente der `object` sind die Mitglieder der `System.Object` Klasse.
*  Die Elemente der `string` sind die Mitglieder der `System.String` Klasse.

### <a name="interface-members"></a>Schnittstellenmember

Die Member einer Schnittstelle sind die Elemente in der Benutzeroberfläche und in der Schnittstelle alle Basisschnittstellen deklariert. Die Elemente in der Klasse `object` sind nicht streng genommen, Mitglied einer beliebigen Schnittstelle ([Schnittstellenmember](interfaces.md#interface-members)). Allerdings die Elemente in der Klasse `object` stehen über die Suche nach Membern in einen Schnittstellentyp ([Membersuche](expressions.md#member-lookup)).

### <a name="array-members"></a>Array-Elemente

Die Elemente eines Arrays sind die Elemente, die von der Klasse geerbt `System.Array`.

### <a name="delegate-members"></a>Mitglieder von Delegaten

Die Member eines Delegaten sind die Elemente, die von der Klasse geerbt `System.Delegate`.

## <a name="member-access"></a>Memberzugriff

Deklarationen von Elementen ermöglichen die Steuerung des Zugriffs für Member. Der Zugriff auf ein Element wird hergestellt, indem die deklarierte Zugriffsart ([deklariert Barrierefreiheit](basic-concepts.md#declared-accessibility)) des Elements zusammen mit den Zugriff des direkt enthaltenden Typs, sofern vorhanden.

Wenn Zugriff auf einen bestimmten Member zulässig ist, das Element wird als ***zugänglich***. Im Gegensatz dazu beim Zugriff auf einen bestimmten Member nicht zulässig, der Member gilt als ***kann nicht zugegriffen werden***. Zugriff auf einen Member ist zulässig, wenn die Textposition, in denen der Zugriff erfolgt, in der Zugriffsdomäne enthalten ist ([Barrierefreiheit Domänen](basic-concepts.md#accessibility-domains)) des Elements.

### <a name="declared-accessibility"></a>Deklarierter Zugriff

Die ***deklariert Barrierefreiheit*** eines Elements kann einen der folgenden sein:

*  Öffentlich, die dazu ausgewählt ist eine `public` Modifizierer in der Memberdeklaration. Die intuitive Bedeutung von `public` ist "Zugriff nicht eingeschränkt".
*  Geschützt, die dazu ausgewählt ist eine `protected` Modifizierer in der Memberdeklaration. Die intuitive Bedeutung von `protected` "den Zugriff auf die enthaltende Klasse oder auf Typen beschränkt stammt von der enthaltenden Klasse".
*  Intern fest, die dazu ausgewählt wird ein `internal` Modifizierer in der Memberdeklaration. Die intuitive Bedeutung von `internal` "Zugriff auf dieses Programm beschränkt" ist.
*  Geschützte interne (das heißt geschützt oder intern), die ausgewählt ist, indem auch ein `protected` und ein `internal` Modifizierer in der Memberdeklaration. Die intuitive Bedeutung von `protected internal` ist "den Zugriff auf dieses Programm beschränkt oder von der enthaltenden Klasse abgeleiteten Typen".
*  Private, die dazu ausgewählt ist eine `private` Modifizierer in der Memberdeklaration. Die intuitive Bedeutung von `private` ist "Zugriff auf den enthaltenden Typ eingeschränkt".

Platzieren Sie je nach Kontext, in dem Deklaration eines Elements verwendet, nur bestimmte Arten von deklarierte Zugriffsart sind zulässig. Darüber hinaus bei der Deklaration eines Elements keine Zugriffsmodifizierer enthalten ist, bestimmt der Kontext, in dem die Deklaration erfolgt, die Barrierefreiheit deklariert.

*  Namespaces verfügen implizit über `public` Barrierefreiheit deklariert. Keine Zugriffsmodifizierer sind für Namespace-Deklarationen zulässig.
*  Typen in Namespaces oder einer Kompilierungseinheiten deklariert haben `public` oder `internal` deklariert, Barrierefreiheit und sind standardmäßig `internal` Barrierefreiheit deklariert.
*  Klassenmember können mindestens einer der fünf Arten von deklarierte Zugriffsart und standardmäßig `private` Barrierefreiheit deklariert. (Beachten Sie, dass ein Typ deklariert, wie ein Member einer Klasse, die eines der fünf Arten von deklarierte Zugriffsart verfügen kann, während ein Typ deklarierter ein Member eines Namespaces kann nur über verfügen `public` oder `internal` Barrierefreiheit deklariert.)
*  Strukturmember können haben `public`, `internal`, oder `private` deklariert, Barrierefreiheit und sind standardmäßig `private` Barrierefreiheit deklariert, da Strukturen implizit versiegelt sind. Strukturmember in einer Struktur (die durch diese Struktur nicht vererbt wird) eingeführt wurden keine `protected` oder `protected internal` Barrierefreiheit deklariert. (Beachten Sie, dass ein Typ deklariert, wie ein Member einer Struktur kann `public`, `internal`, oder `private` Barrierefreiheit, deklariert, während ein Typ deklarierter ein Member eines Namespaces kann nur über verfügen `public` oder `internal` Barrierefreiheit deklariert.)
*  Schnittstellenmember verfügen implizit über `public` Barrierefreiheit deklariert. Auf Schnittstellenmember-Deklarationen sind keine Zugriffsmodifizierer zulässig.
*  Enumerationsmember verfügen implizit über `public` Barrierefreiheit deklariert. Deklaration von Enumerationsmembern sind keine Zugriffsmodifizierer zulässig.

### <a name="accessibility-domains"></a>Barrierefreiheit-Domänen

Die ***Zugriffsdomäne*** eines Elements enthält (die möglicherweise nicht zusammenhängende) Abschnitte des Programmtexts, in dem Zugriff auf das Element ist zulässig. Rahmen, die Zugriffsdomäne eines Members zu definieren, ein Member gilt als ***auf oberster Ebene*** wenn er nicht innerhalb eines Typs deklariert ist, und ein Member gilt als ***geschachtelte*** wenn er in einem anderen Typ deklariert wird. Darüber hinaus die ***Programm Text*** eines Programms ist wie alle Text des Programms alle Quelldateien im Programm und der Programmtext von einem Typ definiert ist, wie alle Text im Programm definiert die *Type_declaration*s dieses Typs (einschließlich, möglicherweise Typen, die innerhalb des Typs geschachtelt sind).

Die Zugriffsdomäne eines vordefinierten Typs (z. B. `object`, `int`, oder `double`) gibt es keine Einschränkungen.

Die Zugriffsdomäne von oberster Ebene von nicht gebundenen Typ `T` ([gebunden und Typen von nicht gebundenen](types.md#bound-and-unbound-types)) in einem Programm deklariert wird `P` ist wie folgt definiert:

*  Wenn die deklarierte Zugriffsart von `T` ist `public`, die Zugriffsdomäne von `T` ist der Programmtext von `P` und jedes Programm, das Verweise `P`.
*  Wenn die deklarierte Zugriffsart von `T` den Wert `internal` hat, entspricht die Zugriffsdomäne von `T` dem Programmtext von `P`.

Aus diesen Definitionen folgt, dass die Zugriffsdomäne eines Typs der obersten Ebene nicht gebundenen immer mindestens der Programmtext des Programms in der eingeben, die deklariert wird.

Die Zugriffsdomäne eines konstruierten Typs `T<A1, ..., An>` der Schnittmenge zwischen der Zugriffsdomäne des ungebundenen generischen Typs `T` und den auf Zugriffsdomänen der Typargumente `A1, ..., An`.

Die Zugriffsdomäne eines geschachtelten Members `M` in einem Typ deklariert `T` innerhalb eines Programms `P` ist wie folgt definiert (wobei `M` selbst ein Typ sein kann):

*  Wenn die deklarierte Zugriffsart von `M` den Wert `public` hat, entspricht die Zugriffsdomäne von `M` der von `T`.
*  Wenn die deklarierte Zugriffsart von `M` ist `protected internal`ermöglichen `D` werden die Vereinigung der Programmtext des `P` und der Programmtext jedes Typs abgeleitete `T`, der deklariert wird, außerhalb `P`. Die Zugriffsdomäne von `M` der Schnittmenge zwischen der Zugriffsdomäne von `T` mit `D`.
*  Wenn die deklarierte Zugriffsart von `M` ist `protected`ermöglichen `D` werden die Vereinigung der Programmtext des `T` und der Programmtext jedes Typs abgeleitete `T`. Die Zugriffsdomäne von `M` der Schnittmenge zwischen der Zugriffsdomäne von `T` mit `D`.
*  Wenn die deklarierte Zugriffsart von `M` den Wert `internal` hat, entspricht die Zugriffsdomäne von `M` der Schnittmenge zwischen der Zugriffsdomäne von `T` und dem Programmtext von `P`.
*  Wenn die deklarierte Zugriffsart von `M` den Wert `private` hat, entspricht die Zugriffsdomäne von `M` dem Programmtext von `T`.

Aus diesen Definitionen folgt, dass die Zugriffsdomäne eines geschachtelten Members immer mindestens der Programmtext des Typs in der das Element deklariert wird. Darüber hinaus ergibt sich, dass es sich bei die Zugriffsdomäne eines Members nie mehr als die Zugriffsdomäne des Typs ist in der das Element deklariert wird.

Intuitiv ausgedrückt Wenn ein Typ oder Member `M` wird zugegriffen, die folgenden Schritte werden ausgewertet, um sicherzustellen, dass der Zugriff zulässig ist:

*  Wenn zunächst `M` ist innerhalb eines Typs deklariert (im Unterschied zu einer Kompilierungseinheit oder einen Namespace), ein Fehler während der Kompilierung tritt auf, wenn dieses Typs nicht zugänglich ist.
*  Wenn danach `M` ist `public`, der Zugriff zulässig ist.
*  Andernfalls gilt: Wenn `M` ist `protected internal`, der Zugriff zulässig ist, tritt innerhalb des Programms in der `M` deklariert ist, oder wenn er innerhalb einer Klasse erfolgt, abgeleitete Klasse, in der `M` wird deklariert und erfolgt über die abgeleitete Klassentyp ([z. B. den Zugriff auf Member für geschützte](basic-concepts.md#protected-access-for-instance-members)).
*  Andernfalls gilt: Wenn `M` ist `protected`, der Zugriff zulässig ist, tritt in der Klasse in der `M` deklariert ist, oder wenn er innerhalb einer Klasse erfolgt, abgeleitete Klasse, in der `M` wird deklariert und erfolgt über die abgeleitete Klassentyp ([z. B. den Zugriff auf Member für geschützte](basic-concepts.md#protected-access-for-instance-members)).
*  Andernfalls gilt: Wenn `M` ist `internal`, der Zugriff zulässig ist, tritt innerhalb des Programms in der `M` deklariert wird.
*  Andernfalls gilt: Wenn `M` ist `private`, der Zugriff zulässig ist, tritt innerhalb des Typs in der `M` deklariert wird.
*  Hingegen den Typ oder Member kann nicht zugegriffen werden, und ein Fehler während der Kompilierung auftritt.

Im Beispiel
```csharp
public class A
{
    public static int X;
    internal static int Y;
    private static int Z;
}

internal class B
{
    public static int X;
    internal static int Y;
    private static int Z;

    public class C
    {
        public static int X;
        internal static int Y;
        private static int Z;
    }

    private class D
    {
        public static int X;
        internal static int Y;
        private static int Z;
    }
}
```
die Klassen und Member haben die folgenden Eingabehilfen-Domänen:

*  Die Zugriffsdomäne von `A` und `A.X` ist unbegrenzt.
*  Die Zugriffsdomäne von `A.Y`, `B`, `B.X`, `B.Y`, `B.C`, `B.C.X`, und `B.C.Y` ist der Programmtext des Programms enthält.
*  Die Zugriffsdomäne von `A.Z` ist der Programmtext von `A`.
*  Die Zugriffsdomäne von `B.Z` und `B.D` ist der Programmtext von `B`, einschließlich den Programmtext von `B.C` und `B.D`.
*  Die Zugriffsdomäne von `B.C.Z` ist der Programmtext von `B.C`.
*  Die Zugriffsdomäne von `B.D.X` und `B.D.Y` ist der Programmtext von `B`, einschließlich den Programmtext von `B.C` und `B.D`.
*  Die Zugriffsdomäne von `B.D.Z` ist der Programmtext von `B.D`.

Wie im Beispiel wird veranschaulicht, ist die Zugriffsdomäne eines Members nie größer als die des enthaltenden Typs. Beispielsweise, obwohl alle `X` Mitglieder haben Zugriff public deklariert, gut `A.X` haben Sie auf Zugriffsdomänen, die von einem enthaltenden Typ eingeschränkt werden.

Siehe [Mitglieder](basic-concepts.md#members), alle Member einer Basisklasse, mit Ausnahme von z. B. Konstruktoren, Destruktoren und statische Konstruktoren von abgeleiteten Typen vererbt werden. Dies schließt auch Private Member einer Basisklasse. Die Zugriffsdomäne eines privaten Members enthält jedoch nur der Programmtext des Typs in der das Element deklariert wird. Im Beispiel
```csharp
class A
{
    int x;

    static void F(B b) {
        b.x = 1;        // Ok
    }
}

class B: A
{
    static void F(B b) {
        b.x = 1;        // Error, x not accessible
    }
}
```
die `B` -Klasse erbt die privaten Member `x` aus der `A` Klasse. Da der Member privat ist, ist es nur zugegriffen werden kann die *Class_body* von `A`. Daher den Zugriff auf `b.x` erfolgreich die `A.F` -Methode, aber nicht erfolgreich. die `B.F` Methode.

### <a name="protected-access-for-instance-members"></a>Geschützter Zugriff auf Instanzmember

Bei der ein `protected` Instanzmember erfolgt außerhalb von dem Programmtext von der Klasse, in dem sie deklariert ist, und wann eine `protected internal` Instanzmember erfolgt außerhalb der Programmtext des Programms in der sie deklariert ist, der Zugriff erfolgen muss innerhalb einer Deklaration der Klasse, die von der Klasse abgeleitet ist, in dem sie deklariert ist. Darüber hinaus muss der Zugriff durch eine Instanz der abgeleiteten Typ oder ein Klassentyp, der daraus erstellte stattfinden. Diese Einschränkung wird verhindert, dass eine abgeleitete Klasse den Zugriff auf geschützte Member der andere abgeleiteten Klassen, selbst wenn die Elemente, die von derselben Basisklasse geerbt werden.

Lassen Sie `B` eine Basisklasse, die eine geschützte Instanzmember deklariert sein `M`, und lassen Sie `D` werden eine abgeleitete Klasse `B`. In der *Class_body* von `D`, den Zugriff auf `M` kann einen der folgenden Formen annehmen:

*  Einen nicht qualifizierten *Type_name* oder *Primary_expression* des Formulars `M`.
*  Ein *Primary_expression* des Formulars `E.M`, den Typ des bereitgestellten `E` ist `T` oder eine abgeleitete Klasse `T`, wobei `T` ist der Klassentyp `D`, oder ein Klassentyp erstellt von `D`
*  Ein *Primary_expression* des Formulars `base.M`.

Zusätzlich zu diese Formen des Zugriffs, kann eine abgeleitete Klasse zugreifen, einen geschützte Instanzenkonstruktor einer Basisklasse in eine *Constructor_initializer* ([Konstruktorinitialisierer](classes.md#constructor-initializers)).

Im Beispiel
```csharp
public class A
{
    protected int x;

    static void F(A a, B b) {
        a.x = 1;        // Ok
        b.x = 1;        // Ok
    }
}

public class B: A
{
    static void F(A a, B b) {
        a.x = 1;        // Error, must access through instance of B
        b.x = 1;        // Ok
    }
}
```
in `A`, es ist möglich, für den Zugriff auf `x` durch Instanzen der `A` und `B`, da in beiden Fällen der Zugriff durch eine Instanz der stattfindet `A` oder eine abgeleitete Klasse `A`. Allerdings in `B`, es ist nicht möglich, den Zugriff auf `x` durch eine Instanz der `A`, da `A` nicht von abgeleitet `B`.

Im Beispiel
```csharp
class C<T>
{
    protected T x;
}

class D<T>: C<T>
{
    static void F() {
        D<T> dt = new D<T>();
        D<int> di = new D<int>();
        D<string> ds = new D<string>();
        dt.x = default(T);
        di.x = 123;
        ds.x = "test";
    }
}
```
die drei Zuweisungen zu `x` sind zulässig, da sie alle über Instanzen von Klassentypen, die über den generischen Typ erstellt erfolgen.

### <a name="accessibility-constraints"></a>Barrierefreiheit-Einschränkungen

Verschiedene Konstrukte in c# müssen einen Typ ist ***mindestens dieselben zugriffsmöglichkeiten wie*** ein Element oder einen anderen Typ. Ein Typ `T` gilt mindestens dieselben zugriffsmöglichkeiten wie ein Member oder Typ `M` Wenn die Zugriffsdomäne von `T` ist eine Obermenge der Zugriffsdomäne von `M`. Das heißt, `T` ist mindestens dieselben zugriffsmöglichkeiten wie `M` Wenn `T` zugegriffen werden in allen Kontexten, in dem `M` zugegriffen werden kann.

Die folgenden Einschränkungen für die Barrierefreiheit vorhanden sein:

*  Die direkte Basisklasse eines Klassentyps muss mindesten dieselben Zugriffsmöglichkeiten wie der Klassentyp selbst bieten.
*  Die explizite Basisschnittstelle eines Schnittstellentyps muss mindesten dieselben Zugriffsmöglichkeiten bieten wie der Schnittstellentyp selbst.
*  Die Rückgabe- und Parametertypen eines Delegattyps müssen mindestens dieselben Zugriffsmöglichkeiten wie der Delegattyp selbst bieten.
*  Der Typ einer Konstante muss mindestens dieselben Zugriffsmöglichkeiten wie die Konstante selbst bieten.
*  Der Typ eines Felds muss mindestens dieselben Zugriffsmöglichkeiten bieten wie das Feld selbst.
*  Die Rückgabe- und Parametertypen einer Methode müssen mindestens dieselben Zugriffsmöglichkeiten bieten wie die Methode selbst.
*  Der Typ einer Eigenschaft muss mindestens dieselben Zugriffsmöglichkeiten bieten wie die Eigenschaft selbst.
*  Der Typ eines Ereignisses muss mindestens dieselben Zugriffsmöglichkeiten bieten wie das Ereignis selbst.
*  Der Typ und die Parametertypen eines Indexers müssen mindestens dieselben Zugriffsmöglichkeiten bieten wie der Indexer selbst.
*  Die Rückgabe- und Parametertypen eines Operators müssen mindestens dieselben Zugriffsmöglichkeiten bieten wie der Operator selbst.
*  Die Parametertypen eines Instanzkonstruktors müssen mindestens dieselben zugriffsmöglichkeiten bieten wie der Instanzkonstruktor selbst sein.

Im Beispiel
```csharp
class A {...}

public class B: A {...}
```
die `B` Klasse führt zu einem Fehler während der Kompilierung auf, da `A` ist nicht mindestens dieselben zugriffsmöglichkeiten wie `B`.

Ebenso im Beispiel
```csharp
class A {...}

public class B
{
    A F() {...}

    internal A G() {...}

    public A H() {...}
}
```
die `H` -Methode in der `B` führt zu einem Fehler während der Kompilierung, weil der Rückgabetyp `A` ist nicht mindestens dieselben zugriffsmöglichkeiten bieten wie die Methode.

## <a name="signatures-and-overloading"></a>Signaturen und überladen

Methoden, Instanzkonstruktoren, Indexer und Operatoren sind gekennzeichnet durch die ***Signaturen***:

*  Die Signatur einer Methode besteht aus den Namen der Methode, die Anzahl von Typparametern und der Typ und die Art (Wert, Verweis oder Ausgabe) aller seiner formalen Parameter, die in der Reihenfolge von links nach rechts betrachtet. Für diese Zwecke wird einen Typparameter der Methode, die in den Typ eines formalen Parameters auftritt, nicht mit dem Namen, aber anhand ihrer Ordnungsposition in der Liste der Typargumente der Methode identifiziert. Die Signatur einer Methode beinhalten insbesondere nicht den Rückgabetyp der `params` Modifizierer, der für den äußeren rechten Parameter noch optionale Einschränkungen für Typparameter angegeben werden kann.
*  Die Signatur eines Instanzkonstruktors besteht aus den Typ und die Art (Wert, Verweis oder Ausgabe) aller seiner formalen Parameter, die in der Reihenfolge von links nach rechts betrachtet. Die Signatur eines Instanzkonstruktors schließt insbesondere nicht die `params` Modifizierer, der für den äußersten-Parameter angegeben werden kann.
*  Die Signatur eines Indexers besteht aus den Typ der einzelnen formalen Parameter, in der Reihenfolge von links nach rechts betrachtet. Die Signatur eines Indexers speziell enthält keinen Typ des Elements, und umfasst auch die `params` Modifizierer, der für den äußersten-Parameter angegeben werden kann.
*  Die Signatur eines Operators besteht aus den Namen des Operators und den Typ der einzelnen formalen Parameter, in der Reihenfolge von links nach rechts betrachtet. Die Signatur eines Operators schließt insbesondere nicht den Ergebnistyp.

Signaturen sind der Aktivierung Mechanismus zum ***überladen*** von Elementen in Klassen, Strukturen und Schnittstellen:

*  Das Überladen von Methoden ermöglicht, Klasse, Struktur oder Schnittstelle deklariert, dass mehrere Methoden mit dem gleichen Namen, ihre Signaturen bereitgestellten innerhalb dieser Klasse, Struktur oder Schnittstelle eindeutig sind.
*  Das Überladen von Instanzkonstruktoren ermöglicht einer Klasse oder Struktur, um mehrere Instanzkonstruktoren, zu deklarieren, vorausgesetzt, dass die Signaturen in der Klasse oder Struktur eindeutig sind.
*  Eine Klasse, Struktur oder Schnittstelle, um mehrere Indexer deklarieren, das Überladen der Indexer ermöglicht werden, vorausgesetzt, dass die Signaturen in dieser Klasse, Struktur oder Schnittstelle eindeutig sind.
*  Das Überladen von Operatoren ermöglicht, eine Klasse oder Struktur deklariert, dass mehrere Operatoren mit dem gleichen Namen, ihre Signaturen bereitgestellten eindeutig innerhalb der Klasse oder Struktur sind.

Obwohl `out` und `ref` Parametermodifizierern gelten als Teil einer Signatur, die in einem einzelnen Typ deklarierten Member dürfen sich nicht unterscheiden in Signatur über `ref` und `out`. Ein Fehler während der Kompilierung tritt auf, wenn zwei Member deklariert werden in der gleichen Art mit Signaturen, die dieselbe, wenn alle Parameter in den beiden Methoden mit `out` Modifizierer wurden geändert, um `ref` Modifizierer. Für andere Zwecke signaturabstimmung (z. B. zum Ausblenden oder überschreiben), `ref` und `out` gelten als Teil der Signatur und passen nicht zueinander. (Diese Einschränkung ist, können Sie C#-Programme leicht übersetzt werden, führen Sie auf der Common Language Infrastructure (CLI), die keine bietet eine Möglichkeit zum Definieren von Methoden, die ausschließlich in unterscheiden `ref` und `out`.)

Im Rahmen der Signaturen, die Typen `object` und `dynamic` werden als identisch betrachtet. In einem einzelnen Typ deklarierten Member können aus diesem Grund unterscheiden sich nicht in der Signatur über `object` und `dynamic`.

Das folgende Beispiel zeigt eine Reihe von Deklarationen der überladenen Methode zusammen mit ihren Signaturen.
```csharp
interface ITest
{
    void F();                        // F()

    void F(int x);                   // F(int)

    void F(ref int x);               // F(ref int)

    void F(out int x);               // F(out int)      error

    void F(int x, int y);            // F(int, int)

    int F(string s);                 // F(string)

    int F(int x);                    // F(int)          error

    void F(string[] a);              // F(string[])

    void F(params string[] a);       // F(string[])     error
}
```

Beachten Sie, dass jede `ref` und `out` Parametermodifizierern ([Methodenparameter](classes.md#method-parameters)) sind Teil einer Signatur. Daher `F(int)` und `F(ref int)` sind eindeutige Signaturen. Allerdings `F(ref int)` und `F(out int)` kann nicht innerhalb der gleichen Schnittstelle deklariert werden, da ihre Signaturen, ausschließlich vom unterscheiden `ref` und `out`. Beachten Sie auch, die den Rückgabetyp und die `params` Modifizierer sind nicht Teil der Signatur, daher ist es nicht möglich, ausschließlich basierend auf den Rückgabetyp oder Einschluss oder Ausschluss von Überladen der `params` Modifizierer. Als solche, die Deklarationen der Methoden `F(int)` und `F(params string[])` das oben genannte Ergebnis zu einem Fehler während der Kompilierung.

## <a name="scopes"></a>Bereiche

Die ***Bereich*** mit einem Namen, ist die Region des Programmtexts, in dem es möglich ist, auf die Entität deklariert, die anhand des Namens ohne Qualifizierung des Namens verweisen. Bereiche möglich ***geschachtelte***, und einem inneren Gültigkeitsbereich möglicherweise erneut die Bedeutung eines Namens außerhalb des Gültigkeitsbereichs deklariert werden (Dies wird jedoch nicht, entfernt die Einschränkung seitens [Deklarationen](basic-concepts.md#declarations) , dass es in einem geschachtelten Block nicht möglich, eine lokale Variable mit dem gleichen Namen wie eine lokale Variable in einem einschließenden Block deklarieren). Der Name aus dem äußeren Bereich gilt dann als ***ausgeblendeten*** in der Region des Programms Text behandelt, durch den inneren Bereich aus, und Zugriff auf den äußeren Namen ist nur möglich, indem Sie die Namen qualifizieren.

*  Der Gültigkeitsbereich eines Namespace deklariert werden, indem eine *Namespace_member_declaration* ([Namespace Member](namespaces.md#namespace-members)) mit ohne einschließenden *Namespace_declaration* wird das gesamte Programm Text.
*  Der Gültigkeitsbereich eines Namespace deklariert werden, indem eine *Namespace_member_declaration* innerhalb einer *Namespace_declaration* ist, dessen vollqualifizierter Name `N` ist die *Namespace_body*  von jedem *Namespace_declaration* ist, dessen vollqualifizierter Name `N` oder beginnt mit `N`, gefolgt von einem Punkt.
*  Den Bereich der Namen von definiert eine *Extern_alias_directive* erstreckt sich über die *Using_directive*s, *Global_attributes* und *Namespace_member_ Deklaration*s des direkt enthaltenden Kompilierung oder Text. Ein *Extern_alias_directive* trägt sich nicht auf alle neuen Member zu den zugrunde liegenden Deklarationsabschnitt. Das heißt, eine *Extern_alias_directive* ist nicht transitiv, sondern nur die Kompilierung Einheit oder Namespacetext in dem er auftritt, beeinflusst.
*  Bereich mit einem Namen definiert oder importiert, indem eine *Using_directive* ([Using-Direktiven](namespaces.md#using-directives)) erstreckt sich über die *Namespace_member_declaration*s der  *Compilation_unit* oder *Namespace_body* in dem die *Using_directive* auftritt. Ein *Using_directive* möglicherweise Verfügbarmachen von 0 (null) oder mehrere Namen von Namespace, Typ oder Member innerhalb eines bestimmten *Compilation_unit* oder *Namespace_body*, jedoch nicht tragen Sie alle neuen Member zu den zugrunde liegenden Deklarationsabschnitt. Das heißt, eine *Using_directive* ist nicht transitiv, sondern beeinflusst nur den *Compilation_unit* oder *Namespace_body* in dem sie verwendet wird.
*  Der Bereich, der einen Typparameter deklariert, indem eine *Type_parameter_list* auf eine *Class_declaration* ([Klasse Deklarationen](classes.md#class-declarations)) ist die *Class_base*, *Type_parameter_constraints_clause*s, und *Class_body* , *Class_declaration*.
*  Der Bereich, der einen Typparameter deklariert, indem eine *Type_parameter_list* auf eine *Struct_declaration* ([Strukturdeklarationen](structs.md#struct-declarations)) ist die *Struct_interfaces* , *Type_parameter_constraints_clause*s, und *Struct_body* , *Struct_declaration*.
*  Der Bereich, der einen Typparameter deklariert, indem eine *Type_parameter_list* auf ein *Interface_declaration* ([Schnittstellendeklarationen](interfaces.md#interface-declarations)) ist die *Interface_base* , *Type_parameter_constraints_clause*s, und *Interface_body* , *Interface_declaration*.
*  Der Bereich der einen Typparameter deklariert, indem eine *Type_parameter_list* auf eine *Delegate_declaration* ([delegieren Deklarationen](delegates.md#delegate-declarations)) ist die *Return_type*, *Formal_parameter_list*, und *Type_parameter_constraints_clause*s, *Delegate_declaration*.
*  Der Bereich eines Elements deklariert wird, indem eine *Class_member_declaration* ([Klasse Text](classes.md#class-body)) ist die *Class_body* in dem die Deklaration erfolgt. Darüber hinaus der Bereich eines Klassenmembers erweitert, um die *Class_body* davon abgeleiteten Klassen, die in der Zugriffsdomäne enthalten sind ([Barrierefreiheit Domänen](basic-concepts.md#accessibility-domains)) des Elements.
*  Der Bereich eines Elements deklariert wird, indem eine *Struct_member_declaration* ([Strukturmember](structs.md#struct-members)) ist die *Struct_body* in dem die Deklaration erfolgt.
*  Der Bereich eines Elements deklariert wird, indem ein *Enum_member_declaration* ([Enumerationsmember](enums.md#enum-members)) ist der *Enum_body* in dem die Deklaration erfolgt.
*  Der Gültigkeitsbereich eines Parameters in deklarierten eine *Method_declaration* ([Methoden](classes.md#methods)) ist die *Method_body* , *Method_declaration*.
*  Der Gültigkeitsbereich eines Parameters in deklarierten ein *Indexer_declaration* ([Indexer](classes.md#indexers)) ist die *Accessor_declarations* , *Indexer_declaration*.
*  Der Gültigkeitsbereich eines Parameters in deklarierten ein *Operator_declaration* ([Operatoren](classes.md#operators)) ist die *Block* , *Operator_declaration*.
*  Der Gültigkeitsbereich eines Parameters in deklarierten eine *Constructor_declaration* ([Instanzkonstruktoren](classes.md#instance-constructors)) ist die *Constructor_initializer* und *Block* , *Constructor_declaration*.
*  Der Gültigkeitsbereich eines Parameters in deklarierten eine *Lambda_expression* ([anonyme Funktionsausdrücke](expressions.md#anonymous-function-expressions)) ist die *Anonymous_function_body* , *Lambda_ Ausdruck*
*  Der Gültigkeitsbereich eines Parameters in deklarierten ein *Anonymous_method_expression* ([anonyme Funktionsausdrücke](expressions.md#anonymous-function-expressions)) ist die *Block* , *Anonymous_method _ausdruck*.
*  Der Umfang einer Bezeichnung in deklariert eine *Labeled_statement* ([Anweisungen mit der Bezeichnung](statements.md#labeled-statements)) ist die *Block* in dem die Deklaration erfolgt.
*  Eine lokale Variable deklariert, die im Rahmen einer *Local_variable_declaration* ([lokale Variablendeklarationen](statements.md#local-variable-declarations)) wird der Block in dem die Deklaration erfolgt.
*  Eine lokale Variable deklariert, die im Rahmen einer *Switch_block* von eine `switch` Anweisung ([der Switch-Anweisung](statements.md#the-switch-statement)) ist der *Switch_block*.
*  Eine lokale Variable deklariert, die im Rahmen eine *For_initializer* von eine `for` Anweisung ([der für die Anweisung](statements.md#the-for-statement)) ist der *For_initializer*, wird die  *For_condition*, *For_iterator*, und den enthaltenen *Anweisung* von der `for` Anweisung.
*  Der Bereich, der eine lokale Konstante deklariert wird, eine *Local_constant_declaration* ([Deklaration von lokalen Konstanten](statements.md#local-constant-declarations)) wird der Block in dem die Deklaration erfolgt. Es ist ein Fehler während der Kompilierung zu verweisen auf eine lokale Konstante in der Lage, Text vor dessen *Constant_declarator*.
*  Der Gültigkeitsbereich einer Variablen deklariert werden, als Teil einer *Foreach_statement*, *Using_statement*, *Lock_statement* oder *Query_expression* ist durch die Erweiterung des angegebenen Konstrukts bestimmt.

Innerhalb des Bereichs eines Elements Namespace, Klasse, Struktur oder eines Enumerationswerts ist es möglich, auf das Element in einer Textposition verweisen, die vor der Deklaration des Members. Beispiel:
```csharp
class A
{
    void F() {
        i = 1;
    }

    int i = 0;
}
```
Hier ist es für `F` zum Verweisen auf `i` bevor sie deklariert ist.

Innerhalb des Bereichs einer lokalen Variablen, ist es ein Fehler während der Kompilierung zu verweisen auf die lokale Variable in der Lage, Text vor dem *Local_variable_declarator* der lokalen Variablen. Beispiel:
```csharp
class A
{
    int i = 0;

    void F() {
        i = 1;                  // Error, use precedes declaration
        int i;
        i = 2;
    }

    void G() {
        int j = (j = 1);        // Valid
    }

    void H() {
        int a = 1, b = ++a;    // Valid
    }
}
```

In der `F` oben genannte Methode, die erste Zuweisung zu `i` insbesondere verweist nicht auf das Feld im äußeren Bereich deklariert. Stattdessen wird dies bezieht sich auf die lokale Variable, und dies führt zu einem Fehler während der Kompilierung, da die Deklaration der Variablen textlich vorausgehenden. In der `G` -Methode, die Verwendung von `j` im Initialisierer für die Deklaration von `j` ist gültig, da die Verwendung nicht vorausgeht, wird die *Local_variable_declarator*. In der `H` -Methode, die einen nachfolgenden *Local_variable_declarator* ordnungsgemäß verweist auf eine lokale Variable deklariert, die in einem früheren Zeitpunkt *Local_variable_declarator* innerhalb desselben  *Local_variable_declaration*.

Um sicherzustellen, dass die Bedeutung des einen Namen, die in einem Ausdruckskontext verwendet immer innerhalb eines Blocks identisch ist, ist die Bereichsregeln für lokale Variablen dienen. Würde der Rahmen einer lokalen Variablen nur aus ihrer Deklaration bis zum Ende des Blocks zu erweitern, klicken Sie dann im obigen Beispiel würden die erste Zuweisung an die Instanzvariable zuweisen, und die zweite Zuweisung würden die lokalen Variablen, was möglicherweise zu zuweisen Fehler während der Kompilierung, wäre die Anweisungen des Blocks neu angeordnet werden weiter unten.

Die Bedeutung eines Namens innerhalb eines Blocks kann basierend auf den Kontext unterscheiden, in dem der Name verwendet wird. Im Beispiel
```csharp
using System;

class A {}

class Test
{
    static void Main() {
        string A = "hello, world";
        string s = A;                            // expression context

        Type t = typeof(A);                      // type context

        Console.WriteLine(s);                    // writes "hello, world"
        Console.WriteLine(t);                    // writes "A"
    }
}
```
der Name `A` in einem Ausdruckskontext verwendet wird, die zum Verweisen auf die lokale Variable `A` und in einem Typenkontext zum Verweisen auf die Klasse `A`.

### <a name="name-hiding"></a>Ausblenden von Namen

Der Bereich einer Entität umfasst in der Regel mehr Programmtext im Deklarationsabschnitt der Entität. Insbesondere kann der Bereich einer Entität Deklarationen enthalten, die Einführung neuer Deklaration Leerzeichen, die Entitäten mit demselben Namen enthält. Derartige Deklarationen dazu führen, dass die ursprüngliche Entität werden ***ausgeblendeten***. Im Gegensatz dazu eine Entität gilt als ***sichtbar*** Wenn dieser nicht ausgeblendet.

Ausblenden von Namen tritt auf, wenn Bereiche überschneiden, durch die Schachtelung und wenn die Bereiche durch Vererbung überlappen. Die Eigenschaften der beiden Typen von ausblenden, werden in den folgenden Abschnitten beschrieben.

#### <a name="hiding-through-nesting"></a>Durch die Schachtelung ausblenden

Ausblenden des Namens durch die Schachtelung kann als Ergebnis das Schachteln von Namespaces oder Typen in Namespaces, die als Ergebnis das Schachteln von Typen in Klassen oder Strukturen und als Ergebnis der Parameter und der lokale Variablendeklarationen auftreten.

Im Beispiel
```csharp
class A
{
    int i = 0;

    void F() {
        int i = 1;
    }

    void G() {
        i = 1;
    }
}
```
in der `F` -Methode, die Instanzvariable `i` ist ausgeblendet, die lokale Variable `i`, aber noch innerhalb der `G` -Methode, `i` weiterhin auf die Instanzvariable verweist.

Wenn Sie ein Namen in einem inneren Gültigkeitsbereich auf einen Namen in einem äußeren Gültigkeitsbereich verdeckt, wird er alle überladene Vorkommen mit diesem Namen ausgeblendet. Im Beispiel
```csharp
class Outer
{
    static void F(int i) {}

    static void F(string s) {}

    class Inner
    {
        void G() {
            F(1);              // Invokes Outer.Inner.F
            F("Hello");        // Error
        }

        static void F(long l) {}
    }
}
```
der Aufruf `F(1)` Ruft die `F` in deklarierten `Inner` da alle äußeren Vorkommen von `F` werden durch die innere Deklaration ausgeblendet. Aus demselben Grund, den Aufruf `F("Hello")` führt zu einem Fehler während der Kompilierung.

#### <a name="hiding-through-inheritance"></a>Durch Vererbung ausblenden

Ausblenden des Namens durch Vererbung tritt auf, wenn Sie Klassen oder Strukturen Namen erneut deklariert werden, die von Klassen geerbt wurden. Diese Art von Ausblenden von Namen akzeptiert einen der folgenden Formate:

*  Eine Konstante, ein Feld, Eigenschaft, Ereignis oder in einer Klasse oder Struktur eingeführt Typ Blendet alle Member der Basisklasse mit dem gleichen Namen.
*  Eine Methode, die in einer Klasse oder Struktur eingeführt Blendet alle nicht-Method-Member der Basisklasse mit dem gleichen Namen, und alle Basisklassenmethoden mit der gleichen Signatur (Methodenname und Parameteranzahl, Modifizierer und Typen).
*  Ein Indexer in einer Klasse oder Struktur eingeführt Blendet alle Basisklassenindexer mit der gleichen Signatur (Anzahl und Typen).

Die Regeln für die Operatordeklarationen ([Operatoren](classes.md#operators)) möglicherweise nicht für eine abgeleitete Klasse einen Operator mit der gleichen Signatur als-Operator in einer Basisklasse deklariert. Operatoren ausblenden daher nie miteinander.

Im Gegensatz zur Ausblenden von einem Namen aus einem äußeren Bereich aus, führt dazu, dass eine barrierefreie Name aus einem geerbten Gültigkeitsbereich ausblenden eine Warnung gemeldet werden. Im Beispiel
```csharp
class Base
{
    public void F() {}
}

class Derived: Base
{
    public void F() {}        // Warning, hiding an inherited name
}
```
die Deklaration von `F` in `Derived` bewirkt, dass eine Warnung gemeldet werden. Ausblenden einer geerbten Namens ist insbesondere keinen Fehler, da, die separate Entwicklung von Basisklassen ausgeschlossen würden. Z. B. die oben genannten Situation möglicherweise haben vorkommen, da eine höhere Version von `Base` eingeführt, eine `F` Methode, die nicht in einer früheren Version der Klasse vorhanden war. Die oben genannten Situation ein Fehler gewesen, könnte jede Änderung an einer Basisklasse in einer separaten Versionen Klassenbibliothek potenziell abgeleitete Klassen, ungültig werden, bewirken.

Durch das Ausblenden einer geerbten namens ausgelöste Warnung kann behoben werden, durch die Verwendung von der `new` Modifizierer:
```csharp
class Base
{
    public void F() {}
}

class Derived: Base
{
    new public void F() {}
}
```

Die `new` Modifizierer gibt an, dass die `F` in `Derived` ist "Neu", und dass sie tatsächlich dafür konzipiert ist, die geerbten Member auszublenden.

Eine Deklaration eines neuen Members Blendet einen geerbten Member nur innerhalb des Bereichs des neuen Mitglieds.

```csharp
class Base
{
    public static void F() {}
}

class Derived: Base
{
    new private static void F() {}    // Hides Base.F in Derived only
}

class MoreDerived: Derived
{
    static void G() { F(); }          // Invokes Base.F
}
```

Im obigen Beispiel ist die Deklaration von `F` in `Derived` Blendet Sie aus der `F` , vererbt wurde `Base`, aber das neue `F` in `Derived` privaten Zugriff hat der Bereich erstreckt sich nicht um `MoreDerived` . Daher den Aufruf `F()` in `MoreDerived.G` gültig ist und ruft `Base.F`.

## <a name="namespace-and-type-names"></a>Namespace und Typnamen

Mehrere Kontexte in einem C#-Programm erfordert eine *Namespace_name* oder *Type_name* angegeben werden.

```antlr
namespace_name
    : namespace_or_type_name
    ;

type_name
    : namespace_or_type_name
    ;

namespace_or_type_name
    : identifier type_argument_list?
    | namespace_or_type_name '.' identifier type_argument_list?
    | qualified_alias_member
    ;
```

Ein *Namespace_name* ist eine *Namespace_or_type_name* , der auf einen Namespace verweist. Auflösung befolgen, wie unten beschrieben, die *Namespace_or_type_name* von einer *Namespace_name* muss einem Namespace verweisen oder andernfalls tritt ein Fehler während der Kompilierung auf. Keine Typargumente ([Typargumente](types.md#type-arguments)) in vorhanden sein können eine *Namespace_name* (nur Typen über Typargumente verfügen können).

Ein *Type_name* ist eine *Namespace_or_type_name* , der auf einen Typ verweist. Auflösung befolgen, wie unten beschrieben, die *Namespace_or_type_name* von einer *Type_name* muss auf einen Typ verweisen oder andernfalls tritt ein Fehler während der Kompilierung auf.

Wenn die *Namespace_or_type_name* ist ein qualifizierter-Alias-Element seine Bedeutung ist, wie in beschrieben [Namespace Alias Qualifiers](namespaces.md#namespace-alias-qualifiers). Andernfalls ein *Namespace_or_type_name* verfügt über einen von vier Formaten:

*  `I`
*  `I<A1, ..., Ak>`
*  `N.I`
*  `N.I<A1, ..., Ak>`

in denen `I` ist ein einzelner Bezeichner, `N` ist eine *Namespace_or_type_name* und `<A1, ..., Ak>` ist eine optionale *Type_argument_list*. Wenn kein *Type_argument_list* wird angegeben, sollten Sie `k` 0 (null).

Die Bedeutung des einen *Namespace_or_type_name* wird wie folgt bestimmt:

*   Wenn die *Namespace_or_type_name* hat das Format `I` oder des Formulars `I<A1, ..., Ak>`:
    * Wenn `K` ist 0 (null) und die *Namespace_or_type_name* innerhalb der Deklaration einer generischen Methode angezeigt wird ([Methoden](classes.md#methods)) und wenn dieser Deklaration einen Typparameter enthält ([Typ Parameter](classes.md#type-parameters)) mit dem Namen `I`, und klicken Sie dann die *Namespace_or_type_name* bezieht sich auf den Typparameter.
    * Andernfalls gilt: Wenn die *Namespace_or_type_name* angezeigt wird, in einer Typdeklaration aus, und klicken Sie dann für jeden Instanztyp `T` ([den Instanztyp](classes.md#the-instance-type)) mit dem Instanztyp dieses Typs ab. Erklärung und weiterhin mit den Instanztyp einer Deklaration für jeden einschließenden Klasse oder Struktur (falls vorhanden):
        * Wenn `K` ist 0 (null) und die Deklaration von `T` enthält einen Typparameter mit dem Namen `I`, und klicken Sie dann die *Namespace_or_type_name* bezieht sich auf den Typparameter.
        * Andernfalls gilt: Wenn die *Namespace_or_type_name* angezeigt wird, im Hauptteil der Deklaration des Typs und `T` oder seinen Basistypen enthalten einen geschachtelten zugegriffen werden Typ, der mit Namen `I` und `K` Typparameter , und klicken Sie dann die *Namespace_or_type_name* bezieht sich auf diesen Typ mit den Argumenten angegebenen Typs erstellt. Wenn mehr als ein solcher Typ vorhanden ist, wird die in den stärker abgeleiteten Typ deklarierte Typ ausgewählt. Beachten Sie, dass Nichttyp-Member (Konstanten, Felder, Methoden, Eigenschaften, Indexer, Operatoren, Instanzkonstruktoren, Destruktoren und statische Konstruktoren) und Typmember mit einer anderen Anzahl von Typparametern ignoriert werden, wenn bestimmt wird die Bedeutung des das *Namespace_or_type_name*.
    * Wenn die vorherigen Schritte aus, klicken Sie dann für jeden Namespace nicht erfolgreich ausgeführt wurden `N`, beginnend mit dem Namespace, in dem die *Namespace_or_type_name* auftritt, die Sie den Vorgang fortsetzen, wobei für jeden einschließenden Namespace (sofern vorhanden) und endet mit dem Globaler Namespace die folgenden Schritte werden ausgewertet, bis eine Entität befindet:
        * Wenn `K` ist 0 (null) und `I` ist der Name eines Namespace im `N`, klicken Sie dann:
            * Wenn der Speicherort, in dem die *Namespace_or_type_name* tritt auf, steht eine Namespace-Deklaration für `N` und die Namespacedeklaration enthält eine *Extern_alias_directive* oder *Using_alias_directive* , die den Namen zuordnet `I` mit einem Namespace oder Typ, der *Namespace_or_type_name* ist mehrdeutig und ein Fehler während der Kompilierung auftritt.
            * Andernfalls die *Namespace_or_type_name* verweist auf den Namespace mit dem Namen `I` in `N`.
        * Andernfalls gilt: Wenn `N` enthält einen zugreifbarer Typ, der mit Namen `I` und `K` Typparameter, klicken Sie dann:
            * Wenn `K` ist 0 (null) und den Speicherort, in denen die *Namespace_or_type_name* tritt auf, wird durch eine Namespacedeklaration zur eingeschlossen `N` und die Namespacedeklaration enthält eine *Extern_alias_directive*  oder *Using_alias_directive* , die den Namen verknüpft `I` mit einem Namespace oder Typ, und klicken Sie dann die *Namespace_or_type_name* mehrdeutig und durch einen während der Kompilierung Fehler tritt auf.
            * Andernfalls die *Namespace_or_type_name* verweist auf den Typ mit den Argumenten angegebenen Typs erstellt.
        * Andernfalls gilt: Wenn der Speicherort, in dem die *Namespace_or_type_name* tritt auf, wird durch eine Namespacedeklaration zur eingeschlossen `N`:
            * Wenn `K` ist 0 (null) und die Namespacedeklaration enthält eine *Extern_alias_directive* oder *Using_alias_directive* , die den Namen verknüpft `I` mit einem importierten Namespace oder Der Typ, und klicken Sie dann die *Namespace_or_type_name* bezieht sich auf diesen Namespace oder Typ.
            * Andernfalls, wenn die Deklarationen für Namespaces und importiert die *Using_namespace_directive*s und *Using_alias_directive*s der Namespacedeklaration enthalten genau ein zugreifbarer Typ mit Namen `I` und `K` Typparameter, und klicken Sie dann die *Namespace_or_type_name* bezieht sich auf diesen Typ mit den Argumenten angegebenen Typs erstellt.
            * Andernfalls, wenn die Deklarationen für Namespaces und importiert die *Using_namespace_directive*s und *Using_alias_directive*s der Namespacedeklaration enthalten mehr als ein zugreifbarer Typ mit Namen `I` und `K` Typparameter, und klicken Sie dann die *Namespace_or_type_name* ist mehrdeutig und ein Fehler auftritt.
    * Andernfalls die *Namespace_or_type_name* ist nicht definiert und ein Fehler während der Kompilierung auftritt.
*  Andernfalls die *Namespace_or_type_name* hat das Format `N.I` oder des Formulars `N.I<A1, ..., Ak>`. `N` wird zuerst aufgelöst, als eine *Namespace_or_type_name*. Wenn die Auflösung der `N` nicht erfolgreich ist, tritt ein Fehler während der Kompilierung. Andernfalls `N.I` oder `N.I<A1, ..., Ak>` wird wie folgt aufgelöst:
    * Wenn `K` ist 0 (null) und `N` bezieht sich auf einen Namespace und `N` enthält einen geschachtelten Namespace mit dem Namen `I`, und klicken Sie dann die *Namespace_or_type_name* bezieht sich auf diesen geschachtelten Namespace.
    * Andernfalls gilt: Wenn `N` bezieht sich auf einen Namespace und `N` enthält einen zugreifbarer Typ, der mit Namen `I` und `K` Typparameter, und klicken Sie dann die *Namespace_or_type_name* bezieht sich auf diesen Typ mit den Argumenten angegebenen Typs erstellt.
    * Andernfalls gilt: Wenn `N` bezieht sich auf einen (möglicherweise konstruierten) Klasse oder Struktur-Typ und `N` oder der zugehörigen Basisklassen enthält einen geschachtelte zugreifbarer Typ mit Namen `I` und `K` Typparameter, und klicken Sie dann die *Namespace _or_type_name* bezieht sich auf diesen Typ mit den Argumenten angegebenen Typs erstellt. Wenn mehr als ein solcher Typ vorhanden ist, wird die in den stärker abgeleiteten Typ deklarierte Typ ausgewählt. Beachten Sie, wenn die Bedeutung der `N.I` als Teil des Abschlusses der Basisklasse-Spezifikation, der bestimmt wird `N` klicken Sie dann die direkte Basisklasse von `N` Objekt gilt ([Basisklassen](classes.md#base-classes)).
    * Andernfalls `N.I` ist ein ungültiger *Namespace_or_type_name*, und ein Fehler während der Kompilierung auftritt.

Ein *Namespace_or_type_name* ist zulässig, eine statische Klasse verweisen ([statische Klassen](classes.md#static-classes)) nur, wenn

*  Die *Namespace_or_type_name* ist die `T` in einem *Namespace_or_type_name* des Formulars `T.I`, oder
*  Die *Namespace_or_type_name* ist die `T` in einem *Typeof_expression* ([Argumentlisten](expressions.md#argument-lists)1) des Formulars `typeof(T)`.

### <a name="fully-qualified-names"></a>Vollqualifizierte Namen

Jeder Namespace und Typ verfügt über eine ***voll gekennzeichneten Namen***, den Namespace oder Typ unter anderem alle eindeutig identifiziert. Der vollqualifizierte Name eines Namespace oder Typ `N` wird wie folgt bestimmt:

*  Wenn `N` ist Mitglied des globalen Namespaces, lautet der vollqualifizierte Name ist `N`.
*  Andernfalls lautet der vollqualifizierte Name ist `S.N`, wobei `S` ist der vollqualifizierte Name des Namespace oder Typ, in dem `N` deklariert wird.

Das heißt, der vollqualifizierte Name des `N` enthält den vollständigen hierarchische Pfad von Bezeichnern, die zu führen `N`ab, aus dem globalen Namespace. Da jedes Mitglied eines Namespaces oder Typs auf einen eindeutigen Namen aufweisen muss, ergibt sich, dass es sich bei der vollqualifizierte Namen eines Namespaces oder der Typ immer eindeutig ist.

Das folgende Beispiel zeigt mehrere Deklarationen von Namespace und Typ sowie den zugehörigen voll qualifizierten Namen.
```csharp
class A {}                // A

namespace X               // X
{
    class B               // X.B
    {
        class C {}        // X.B.C
    }

    namespace Y           // X.Y
    {
        class D {}        // X.Y.D
    }
}

namespace X.Y             // X.Y
{
    class E {}            // X.Y.E
}
```

## <a name="automatic-memory-management"></a>Automatische Speicherverwaltung

C# -Code setzt automatische Speicherverwaltung, die Entwickler von manuell zuordnen und Freigeben von den von Objekten belegten Arbeitsspeicher freigibt. Richtlinien für die automatische arbeitsspeicherverwaltung werden implementiert, indem eine ***Garbage Collector***. Der Speicher-Management-Lebenszyklus eines Objekts lautet wie folgt aus:

1. Wenn das Objekt erstellt wird, Arbeitsspeicher dafür zugeordnet wird, wird der Konstruktor ausgeführt und das Objekt wird als aktiv angesehen.
2. Wenn das Objekt oder einen Teil der Seite, indem alle möglichen Fortsetzung der Ausführung nicht zugegriffen werden kann, als die Ausführung von Destruktoren, das Objekt gilt als nicht mehr verwendet, und es zerstört wird. Der C#-Compiler und der Garbage Collector können zum Analysieren von Code, um festzustellen, welche Verweise auf ein Objekt in der Zukunft verwendet werden können. Beispielsweise wenn eine lokale Variable, die im Bereich ist die einzige vorhandene Verweis auf ein Objekt, aber die lokale Variable wird nie in alle möglichen Fortsetzung der Ausführung der aktuellen Ausführung zeigen Sie im Verfahren bezeichnet, der Garbage Collector kann (aber nicht erforderlich, um) behandeln Sie das Objekt als nicht mehr verwendet.
3. Sobald das Objekt zerstört werden kann, Zeit Sie einige später angegeben den Destruktor ([Destruktoren](classes.md#destructors)) (sofern vorhanden) für das Objekt ausgeführt wird. Unter normalen Umständen wird der Destruktor für das Objekt nur einmal ausgeführt, wenn dieses Verhalten außer Kraft gesetzt werden möglicherweise die implementierungsspezifischen APIs zulässt.
4. Wenn der Destruktor für ein Objekt ausgeführt wird, wenn das Objekt oder einen Teil der Seite, von jedem möglichen Fortsetzung der Ausführung, einschließlich der Ausführung von Destruktoren, zugegriffen werden kann, gilt als das Objekt kann nicht zugegriffen werden, und das Objekt wird Garbage Collection.
5. Schließlich nach der Garbage Collection, das Objekt nicht mehr frei zu einem Zeitpunkt der Garbage Collector den Speicher mit diesem Objekt verknüpft ist.

Der Garbage Collector verwaltet Informationen über die Verwendung des Objekts und verwendet diese Informationen zum Arbeitsspeicher Entscheidungen Management, z. B. Where, im Arbeitsspeicher, um ein neu erstelltes Objekt, zu suchen, wenn ein Objekt, und wenn ein Objekt verschieben nicht mehr verwendet oder ist nicht möglich ist.

Wie andere Sprachen, die das Vorhandensein eines Garbage Collectors wird davon ausgegangen wird, ist C# -Code so konzipiert, dass der Garbage Collector kann eine Vielzahl von Richtlinien für die arbeitsspeicherverwaltung implementieren. Z. B. erfordert c# keine Destruktoren ausgeführt werden oder, dass Objekte gesammelt werden, sobald sie berechtigt sind, oder dass Destruktoren in einer bestimmten Reihenfolge oder in einem bestimmten Thread ausgeführt werden.

Das Verhalten des Garbage Collectors kann gesteuert werden, und zu einem gewissen Grad über statische Methoden für die Klasse `System.GC`. Diese Klasse kann verwendet werden, um die Anforderung einer Auflistung, die auftreten, Destruktoren führen (oder nicht ausgeführt werden), und so weiter.

Da der Garbage Collector entscheiden, wann Objekte sammeln, und führen die Destruktoren zulässig ist, kann keine entsprechende Implementierung Ergebnis liefern, die unterscheidet sich von dem durch den folgenden Code gezeigt. Das Programm
```csharp
using System;

class A
{
    ~A() {
        Console.WriteLine("Destruct instance of A");
    }
}

class B
{
    object Ref;

    public B(object o) {
        Ref = o;
    }

    ~B() {
        Console.WriteLine("Destruct instance of B");
    }
}

class Test
{
    static void Main() {
        B b = new B(new A());
        b = null;
        GC.Collect();
        GC.WaitForPendingFinalizers();
    }
}
```
erstellt eine Instanz der Klasse `A` und eine Instanz dieser Klasse `B`. Diese Objekte werden für die Garbagecollection bei der die Variable `b` wird der Wert zugewiesen `null`, da nach dieser Zeit es für jeden Benutzer erstellter Code für den Zugriff darauf nicht möglich ist. Die Ausgabe kann entweder sein.
```
Destruct instance of A
Destruct instance of B
```
oder
```
Destruct instance of B
Destruct instance of A
```
Da die Sprache keine Einschränkungen hinsichtlich der Reihenfolge erzwungen, in dem Objekte die Garbage Collection bereinigt werden.

In Fällen feine kann der Unterschied zwischen "für die Zerstörung geeignet" und "Garbage Collection" wichtig sein. Ein auf ein Objekt angewendeter
```csharp
using System;

class A
{
    ~A() {
        Console.WriteLine("Destruct instance of A");
    }

    public void F() {
        Console.WriteLine("A.F");
        Test.RefA = this;
    }
}

class B
{
    public A Ref;

    ~B() {
        Console.WriteLine("Destruct instance of B");
        Ref.F();
    }
}

class Test
{
    public static A RefA;
    public static B RefB;

    static void Main() {
        RefB = new B();
        RefA = new A();
        RefB.Ref = RefA;
        RefB = null;
        RefA = null;

        // A and B now eligible for destruction
        GC.Collect();
        GC.WaitForPendingFinalizers();

        // B now eligible for collection, but A is not
        if (RefA != null)
            Console.WriteLine("RefA is not null");
    }
}
```

Im oben genannten-Programm, wenn der Garbage Collector auswählt, den Destruktor der auszuführenden `A` vor dem Destruktor der `B`, und klicken Sie dann die Ausgabe dieses Programms sein könnte:
```
Destruct instance of A
Destruct instance of B
A.F
RefA is not null
```

Beachten Sie, dass, obwohl die Instanz von `A` war nicht verwendet und `A`der Destruktor ausgeführt wurde, kann aber dennoch für Methoden des `A` (in diesem Fall `F`), die von einem anderen Destruktor aufgerufen werden. Beachten Sie, dass die Ausführung von einem Destruktor ein Objekt, aus dem hauptbranch Programm wieder verwendbar führen kann. In diesem Fall ist die Ausführung von `B`der Destruktor aufgrund einer Instanz von `A` , wurde zuvor nicht in Gebrauch von der aktiven Verweis zugegriffen werden kann `Test.RefA`. Nach dem Aufruf von `WaitForPendingFinalizers`, die Instanz von `B` ist für die Sammlung, doch die Instanz von geeignet `A` ist aufgrund der Verweis nicht, `Test.RefA`.

Um Verwirrung und unerwartetes Verhalten zu vermeiden, ist es im Allgemeinen eine gute Idee für Destruktoren, nur auszuführen, Cleanup für Daten in die Felder des Objekts und nicht für alle Aktionen auf Objekte verwiesen wird, oder statische Felder.

Eine Alternative zum Verwenden von Destruktoren-besteht darin, dass eine Klasse implementiert die `System.IDisposable` Schnittstelle. Dies ermöglicht dem Client des Objekts zu bestimmen, wann die Ressourcen des Objekts, in der Regel freigegeben wird, durch den Zugriff auf das Objekt als Ressource in einem `using` Anweisung ([die using-Anweisung](statements.md#the-using-statement)).

## <a name="execution-order"></a>Ausführungsreihenfolge

Ausführung eines C#-Programms wird fortgesetzt, die Nebeneffekte für jeden ausgeführten Thread an Ausführungspunkten, kritische beibehalten werden. Ein ***Nebeneffekt*** ist definiert als ein Lese- oder Schreibvorgang eines flüchtigen Felds, das Schreiben in einen nicht flüchtigen Variablen, die das Schreiben in eine externe Ressource, und das Auslösen einer Ausnahme. Die kritische Ausführungspunkte, an dem die Reihenfolge der diese Nebenwirkungen beibehalten werden muss, werden Verweise auf flüchtige Felder ([flüchtige Felder](classes.md#volatile-fields)), `lock` Anweisungen ([die sperranweisung](statements.md#the-lock-statement)), und Threaderstellung und-Beendung. Die ausführungsumgebung kann so ändern Sie die Reihenfolge der Ausführung eines C#-Programms, gelten die folgenden Einschränkungen:

*  Abhängigkeit von Daten wird in einem Thread der Ausführung beibehalten. D.h., wird der Wert der einzelnen Variablen berechnet, als ob alle Anweisungen im Thread in der ursprünglichen programmreihenfolge ausgeführt wurden.
*  Initialisierung Sortierung Regeln bleiben erhalten ([Feld Initialisierung](classes.md#field-initialization) und [Variableninitialisierern](classes.md#variable-initializers)).
*  Die Sortierung von Nebenwirkungen in Bezug auf flüchtige Lese- und Schreibvorgänge beibehalten wird ([flüchtige Felder](classes.md#volatile-fields)). Darüber hinaus muss die ausführungsumgebung nicht Teil des Ausdrucks ausgewertet, wenn es ableiten kann, dass der Wert des Ausdrucks nicht verwendet wird und keine erforderlichen Nebeneffekte erstellt werden (einschließlich durch Aufrufen einer Methode oder den Zugriff auf ein flüchtiges Feld verursacht). Wenn die Ausführung des Programms durch ein asynchrones Ereignis (z. B. eine von einem anderen Thread ausgelöste Ausnahme) unterbrochen wird, ist nicht gewährleistet, dass die wahrnehmbaren Nebeneffekte in der ursprünglichen Reihenfolge der Anwendung angezeigt werden.
