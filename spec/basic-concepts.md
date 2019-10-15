---
ms.openlocfilehash: ff31585520c9090ad92893a930327112743c8e77
ms.sourcegitcommit: 892af9016b3317a8fae12d195014dc38ba51cf16
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/01/2019
ms.locfileid: "71704006"
---
# <a name="basic-concepts"></a>Grundlegende Konzepte

## <a name="application-startup"></a>Anwendungsstart

Eine Assembly, die über einen ***Einstiegspunkt*** verfügt, wird als ***Anwendung***bezeichnet. Wenn eine Anwendung ausgeführt wird, wird eine neue ***Anwendungsdomäne*** erstellt. Mehrere verschiedene Instanziierungen einer Anwendung können gleichzeitig auf demselben Computer vorhanden sein, und jede verfügt über eine eigene Anwendungsdomäne.

Eine Anwendungsdomäne ermöglicht die Anwendungs Isolation, indem Sie als Container für den Anwendungs Zustand fungiert. Eine Anwendungsdomäne fungiert als Container und Grenze für die Typen, die in der Anwendung definiert sind, und die Klassenbibliotheken, die Sie verwendet. Typen, die in eine Anwendungsdomäne geladen werden, unterscheiden sich vom gleichen Typ, der in eine andere Anwendungsdomäne geladen wurde, und Instanzen von Objekten werden nicht direkt zwischen Anwendungs Domänen freigegeben. Zum Beispiel verfügt jede Anwendungsdomäne über eine eigene Kopie statischer Variablen für diese Typen, und ein statischer Konstruktor für einen Typ wird höchstens einmal pro Anwendungsdomäne ausgeführt. Implementierungen können mit Implementierungs spezifischen Richtlinien oder Mechanismen für die Erstellung und Zerstörung von Anwendungs Domänen bereitgestellt werden.

Der ***Anwendungsstart*** erfolgt, wenn die Ausführungsumgebung eine bestimmte Methode aufruft, die als Einstiegspunkt der Anwendung bezeichnet wird. Diese Einstiegspunkt Methode heißt immer `Main` und kann eine der folgenden Signaturen aufweisen:

```csharp
static void Main() {...}

static void Main(string[] args) {...}

static int Main() {...}

static int Main(string[] args) {...}
```

Wie gezeigt, kann der Einstiegspunkt optional einen `int`-Wert zurückgeben. Dieser Rückgabewert wird beim Beenden der Anwendung ([Beendigung der Anwendung](basic-concepts.md#application-termination)) verwendet.

Der Einstiegspunkt kann optional einen formalen Parameter aufweisen. Der-Parameter kann einen beliebigen Namen haben, aber der Typ des Parameters muss `string[]` sein. Wenn der formale Parameter vorhanden ist, erstellt und übergibt die Ausführungsumgebung ein `string[]`-Argument, das die Befehlszeilenargumente enthält, die beim Starten der Anwendung angegeben wurden. Das `string[]`-Argument ist nie NULL, kann jedoch eine Länge von 0 (null) aufweisen, wenn keine Befehlszeilenargumente angegeben wurden.

Da C# das Überladen von Methoden unterstützt, kann eine Klasse oder Struktur mehrere Definitionen einer Methode enthalten, vorausgesetzt, jede hat eine andere Signatur. Allerdings kann in einem einzelnen Programm keine Klasse oder Struktur mehr als eine Methode mit dem Namen "`Main`" enthalten, deren Definition die Verwendung als Anwendungs Einstiegspunkt qualifiziert. Andere überladene Versionen von `Main` sind jedoch zulässig, vorausgesetzt, Sie verfügen über mehr als einen Parameter, oder der einzige Parameter ist ein anderer Parameter als der Typ `string[]`.

Eine Anwendung kann aus mehreren Klassen oder Strukturen bestehen. Es ist möglich, dass mehr als eine dieser Klassen oder Strukturen eine Methode namens "`Main`" enthalten, deren Definition die Verwendung als Anwendungs Einstiegspunkt qualifiziert. In solchen Fällen muss ein externer Mechanismus (z. b. eine Befehlszeilen-Compileroption) verwendet werden, um eine dieser `Main`-Methoden als Einstiegspunkt auszuwählen.

In C#muss jede Methode als Member einer Klasse oder Struktur definiert werden. Normalerweise wird die deklarierte Barrierefreiheit (der[deklarierte](basic-concepts.md#declared-accessibility)Zugriff) einer Methode durch die Zugriffsmodifizierer ([Zugriffsmodifizierer](classes.md#access-modifiers)) bestimmt, die in der Deklaration angegeben sind, und auf ähnliche Weise wird die deklarierte Barrierefreiheit eines Typs in der Deklaration angegebene Zugriffsmodifizierer Damit eine bestimmte Methode eines bestimmten Typs aufgerufen werden kann, muss der Zugriff auf den Typ und den Member möglich sein. Der Einstiegspunkt der Anwendung ist jedoch ein Sonderfall. Insbesondere kann die Ausführungsumgebung auf den Einstiegspunkt der Anwendung zugreifen, unabhängig von der deklarierten Barrierefreiheit und unabhängig von der deklarierten Barrierefreiheit ihrer einschließenden Typdeklarationen.

Die Einstiegspunkt Methode der Anwendung darf sich nicht in einer generischen Klassen Deklaration befinden.

In allen anderen Punkten verhalten sich Einstiegspunkt Methoden wie solche, die keine Einstiegspunkte sind.

## <a name="application-termination"></a>Beenden der Anwendung

Beim Beenden der ***Anwendung*** wird die Steuerung an die Ausführungsumgebung zurückgegeben.

Wenn der Rückgabetyp der ***Einstiegspunkt*** Methode der Anwendung `int` ist, fungiert der zurückgegebene Wert als Beendigungs ***Statuscode***der Anwendung. Der Zweck dieses Codes besteht darin, die Kommunikation über Erfolg oder Misserfolg der Ausführungsumgebung zuzulassen.

Wenn der Rückgabetyp der Einstiegspunkt Methode `void` ist und die Rechte geschweifter Klammer (`}`) erreicht wird, die diese Methode beendet oder eine `return`-Anweisung ausführt, die keinen Ausdruck aufweist, wird der Beendigungs Statuscode `0` angezeigt.

Vor dem Beenden einer Anwendung werden debugtoren für alle Objekte, die noch keine Garbage Collection durchgeführt haben, aufgerufen, es sei denn, eine solche Bereinigung wurde unterdrückt (z. b. durch einen Aufruf der Bibliotheks Methode `GC.SuppressFinalize`).

## <a name="declarations"></a>Deklarationen

Deklarationen in C# einem Programm definieren die Bestandteile des Programms. C#Programme werden mithilfe von Namespaces ([Namespaces](namespaces.md)) organisiert, die Typdeklarationen und schsted Namespace Deklarationen enthalten können. Typdeklarationen ([Typdeklarationen](namespaces.md#type-declarations)) werden verwendet, um Klassen ([Klassen](classes.md)), Strukturen ([Strukturen](structs.md)), Schnittstellen ([Schnittstellen](interfaces.md)), Enumerationen ([Enumerationen](enums.md)) und Delegaten ([Delegaten)](delegates.md) zu definieren. Die Arten von Membern, die in einer Typdeklaration zulässig sind, hängen von der Form der Typdeklaration ab. Klassen Deklarationen können z. a. Deklarationen für Konstanten ([Konstanten](classes.md#constants)), Felder ([Felder](classes.md#fields)), Methoden ([Methoden](classes.md#methods)), Eigenschaften ([Eigenschaften](classes.md#properties)), Ereignisse ([Ereignisse](classes.md#events)), Indexer ([Indexer](classes.md#indexers)) enthalten, Operatoren[(Operatoren](classes.md#operators)), Instanzkonstruktoren ([Instanzkonstruktoren](classes.md#instance-constructors)), statische Konstruktoren ([statische Konstruktoren](classes.md#static-constructors)), Dekonstruktoren ([Dekonstruktoren](classes.md#destructors)) und geclusterte Typen ([geclusterte Typen](classes.md#nested-types)).

Eine Deklaration definiert einen Namen im ***Deklarations Bereich*** , zu dem die Deklaration gehört. Mit Ausnahme überladener Elemente ([Signaturen und überladen](basic-concepts.md#signatures-and-overloading)) handelt es sich um einen Kompilierzeitfehler, der zwei oder mehr Deklarationen mit demselben Namen in einem Deklarations Raum einführt. Es ist nie möglich, dass ein Deklarations Bereich unterschiedliche Arten von Membern mit dem gleichen Namen enthält. Beispielsweise kann ein Deklarations Raum nie ein Feld und eine Methode mit demselben Namen enthalten.

Es gibt mehrere verschiedene Typen von Deklarations Bereichen, die im folgenden beschrieben werden.

*  Innerhalb aller Quelldateien eines Programms sind *namespace_member_declaration*s ohne einschließende *namespace_declaration* Member eines einzelnen kombinierten Deklarations Raums, der als ***globaler Deklarations Bereich***bezeichnet wird.
*  Innerhalb aller Quelldateien eines Programms sind *namespace_member_declaration*s innerhalb von *namespace_declaration*s, die denselben voll qualifizierten Namespace Namen aufweisen, Member eines einzelnen kombinierten Deklarations Raums.
*  Jede Klassen-, Struktur-oder Schnittstellen Deklaration erstellt einen neuen Deklarations Bereich. Namen werden in diesem Deklarations Bereich durch *class_member_declaration*s, *struct_member_declaration*s, *interface_member_declaration*s oder *type_parameter*s eingeführt. Mit Ausnahme von überladenen Instanzkonstruktordeklarationen und statischen Konstruktordeklarationen kann eine Klasse oder Struktur keine Element Deklaration mit dem gleichen Namen wie die Klasse oder Struktur enthalten. Eine Klasse, Struktur oder Schnittstelle ermöglicht die Deklaration überladener Methoden und Indexer. Außerdem ermöglicht eine Klasse oder Struktur die Deklaration überladener Instanzkonstruktoren und Operatoren. Eine Klasse, Struktur oder Schnittstelle kann z. b. mehrere Methoden Deklarationen mit demselben Namen enthalten, sofern sich diese Methoden Deklarationen in Ihrer Signatur ([Signaturen und überladen](basic-concepts.md#signatures-and-overloading)) unterscheiden. Beachten Sie, dass Basisklassen nicht zum Deklarations Bereich einer Klasse beitragen und Basis Schnittstellen nicht zum Deklarations Bereich einer Schnittstelle beitragen. Daher kann eine abgeleitete Klasse oder Schnittstelle einen Member mit demselben Namen wie ein geerbten Member deklarieren. Ein solcher Member gibt an, dass der geerbte Member ***ausgeblendet*** wird.
*  Jede Delegatdeklaration erstellt einen neuen Deklarations Bereich. Namen werden in diesem Deklarations Raum durch formale Parameter (*fixed_parameter*s und *parameter_array*s) und *type_parameter*s eingeführt.
*  Jede Enumerationsdeklaration erstellt einen neuen Deklarations Bereich. Namen werden in diesem Deklarations Raum über *enum_member_declarations*eingeführt.
*  Jede Methoden Deklaration, Indexer-Deklaration, Operator Deklaration, Instanzkonstruktordeklaration und anonyme Funktion erstellt einen neuen Deklarations Raum, der als ***lokaler Variablen Deklarations Bereich***bezeichnet wird Namen werden in diesem Deklarations Raum durch formale Parameter (*fixed_parameter*s und *parameter_array*s) und *type_parameter*s eingeführt. Der Text des Funktionsmembers oder der anonymen Funktion wird, falls vorhanden, als geschachtelt im lokalen Variablen Deklarations Bereich betrachtet. Es ist ein Fehler bei einem Deklarations Raum für lokale Variablen und einem in der Tabelle enthaltenen Deklarations Bereich der lokalen Variablen, der Elemente mit dem gleichen Namen enthalten soll. Daher ist es in einem geschachtelten Deklarations Bereich nicht möglich, eine lokale Variable oder Konstante mit dem gleichen Namen wie eine lokale Variable oder Konstante in einem einschließenden Deklarations Bereich zu deklarieren. Es ist möglich, dass zwei Deklarations Bereiche Elemente mit dem gleichen Namen enthalten, solange kein Deklarations Raum den anderen enthält.
*  Jede *Block* -oder *switch_block* -Anweisung sowie eine *for*-, *foreach* -und *using* -Anweisung erstellt einen lokalen Variablen Deklarations Raum für lokale Variablen und lokale Konstanten. Namen werden in diesem Deklarations Bereich durch *local_variable_declaration*s und *local_constant_declaration*s eingeführt. Beachten Sie, dass Blöcke, die als oder innerhalb des Texts eines Funktionsmembers oder einer anonymen Funktion auftreten, innerhalb des Deklarations Raums für lokale Variablen geschachtelt sind, der von diesen Funktionen für Ihre Parameter deklariert wird. Daher ist es ein Fehler, z. b. eine Methode mit einer lokalen Variablen und einem Parameter mit dem gleichen Namen zu haben.
*  Jeder *Block* oder *switch_block* erstellt einen separaten Deklarations Raum für Bezeichnungen. Namen werden mit *labeled_statement*s in diesen Deklarations Bereich eingefügt, und auf die Namen wird über *goto_statement*s verwiesen. Der ***Zeichenbereich*** der Bezeichnungs Deklaration eines-Blocks enthält alle in der Liste enthaltenen Blöcke. Daher ist es in einem geschachtelten Block nicht möglich, eine Bezeichnung mit demselben Namen wie eine Bezeichnung in einem einschließenden Block zu deklarieren.

Die Text Reihenfolge, in der die Namen deklariert werden, ist im Allgemeinen nicht von Bedeutung. Insbesondere ist die Text Reihenfolge für die Deklaration und Verwendung von Namespaces, Konstanten, Methoden, Eigenschaften, Ereignissen, Indexern, Operatoren, Instanzkonstruktoren, Dekonstruktoren, statischen Konstruktoren und Typen nicht signifikant. Die Deklarations Reihenfolge ist wie folgt signifikant:

*  Die Deklarations Reihenfolge für Feld Deklarationen und lokale Variablen Deklarationen bestimmt die Reihenfolge, in der Ihre Initialisierer (sofern vorhanden) ausgeführt werden.
*  Lokale Variablen müssen definiert werden, bevor Sie verwendet werden ([Bereiche](basic-concepts.md#scopes)).
*  Die Deklarations Reihenfolge für Enumerationmember-Deklarationen ([Enumeration](enums.md#enum-members)-Member) ist signifikant, wenn *constant_expression* -Werte ausgelassen

Der Deklarations Bereich eines Namespaces ist "Open End", und zwei Namespace Deklarationen mit demselben voll qualifizierten Namen tragen zum selben Deklarations Bereich bei. Beispiel:
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

Die beiden oben genannten Namespace Deklarationen tragen zum selben Deklarations Bereich bei, in diesem Fall werden zwei Klassen mit den voll qualifizierten Namen `Megacorp.Data.Customer` und `Megacorp.Data.Order` deklariert. Da die beiden Deklarationen zum gleichen Deklarations Bereich beitragen, hätte Sie einen Kompilierzeitfehler verursacht, wenn jeder eine Deklaration einer Klasse mit dem gleichen Namen enthielt.

Wie oben angegeben, enthält der Deklarations Bereich eines-Blocks alle in der Liste enthaltenen Blöcke. Im folgenden Beispiel führen die Methoden `F` und `G` zu einem Kompilierzeitfehler, da der Name `i` im äußeren Block deklariert wird und nicht im Inneren Block erneut deklariert werden kann. Allerdings sind die Methoden `H` und `I` gültig, da die beiden `i` in separaten nicht in-Blöcken deklarierten Blöcken deklariert werden.

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

Namespaces und Typen verfügen über ***Mitglieder***. Die Elemente einer Entität sind in der Regel über einen qualifizierten Namen verfügbar, der mit einem Verweis auf die Entität beginnt, gefolgt von einem "`.`"-Token, gefolgt vom Namen des Members.

Member eines Typs werden entweder in der Typdeklaration deklariert oder von der Basisklasse des Typs ***geerbt*** . Wenn ein Typ von einer Basisklasse erbt, werden alle Member der Basisklasse, ausgenommen Instanzkonstruktoren, destrukturtoren und statische Konstruktoren, zu Membern des abgeleiteten Typs. Der deklarierte Zugriff eines Basisklassenmembers steuert nicht, ob der Member geerbt wird – die Vererbung wird auf einen Member ausgedehnt, der kein Instanzkonstruktor, statischer Konstruktor oder Dekonstruktor ist. Es ist jedoch möglich, dass auf einen geerbten Member nicht in einem abgeleiteten Typ zugegriffen werden kann, entweder aufgrund seiner deklarierten Barrierefreiheit ([deklariert Barrierefreiheit](basic-concepts.md#declared-accessibility)) oder weil er durch eine Deklaration im Typ selbst ausgeblendet wird ([durch Vererbung verbergen](basic-concepts.md#hiding-through-inheritance)).

### <a name="namespace-members"></a>Namespace-Member

Namespaces und Typen, die keinen einschließenden Namespace aufweisen, sind Member des ***globalen Namespace***. Dies entspricht direkt den im globalen Deklarations Bereich deklarierten Namen.

Namespaces und Typen, die in einem Namespace deklariert werden, sind Member dieses Namespace. Dies entspricht direkt den Namen, die im Deklarations Raum des-Namespace deklariert werden.

Namespaces haben uneingeschränkten Zugriff. Es ist nicht möglich, private, geschützte oder interne Namespaces zu deklarieren, und Namespace Namen sind immer öffentlich zugänglich.

### <a name="struct-members"></a>Strukturmember

Die Member einer Struktur sind die Member, die in der Struktur deklariert sind, und die Member, die von der direkten Basisklasse der Struktur geerbt wurden `System.ValueType` und die indirekte Basisklasse `object`.

Die Member eines einfachen Typs entsprechen direkt den Membern des Struktur Typs, der durch den einfachen Typ Alias:

*  Die Member von `sbyte` sind die Elemente der `System.SByte`-Struktur.
*  Die Member von `byte` sind die Elemente der `System.Byte`-Struktur.
*  Die Member von `short` sind die Elemente der `System.Int16`-Struktur.
*  Die Member von `ushort` sind die Elemente der `System.UInt16`-Struktur.
*  Die Member von `int` sind die Elemente der `System.Int32`-Struktur.
*  Die Member von `uint` sind die Elemente der `System.UInt32`-Struktur.
*  Die Member von `long` sind die Elemente der `System.Int64`-Struktur.
*  Die Member von `ulong` sind die Elemente der `System.UInt64`-Struktur.
*  Die Member von `char` sind die Elemente der `System.Char`-Struktur.
*  Die Member von `float` sind die Elemente der `System.Single`-Struktur.
*  Die Member von `double` sind die Elemente der `System.Double`-Struktur.
*  Die Member von `decimal` sind die Elemente der `System.Decimal`-Struktur.
*  Die Member von `bool` sind die Elemente der `System.Boolean`-Struktur.

### <a name="enumeration-members"></a>Enumerationsmember

Die Member einer Enumeration sind die in der-Enumeration deklarierten Konstanten und die Member, die von der direkten Basisklasse der-Enumeration geerbt werden `System.Enum` und die indirekten Basisklassen `System.ValueType` und `object`.

### <a name="class-members"></a>Klassenmember

Die Member einer Klasse sind die Member, die in der Klasse deklariert werden, und die Member, die von der Basisklasse geerbt wurden (mit Ausnahme der Klasse `object`, die keine Basisklasse aufweist). Die von der Basisklasse geerbten Member enthalten die Konstanten, Felder, Methoden, Eigenschaften, Ereignisse, Indexer, Operatoren und Typen der Basisklasse, nicht jedoch die Instanzkonstruktoren, destrukturtoren und statischen Konstruktoren der Basisklasse. Basisklassenmember werden ohne Rücksicht auf ihre Barrierefreiheit geerbt.

Eine Klassen Deklaration kann Deklarationen von Konstanten, Feldern, Methoden, Eigenschaften, Ereignissen, Indexern, Operatoren, Instanzkonstruktoren, Debuggern, statischen Konstruktoren und Typen enthalten.

Die Member von `object` und `string` entsprechen direkt den Membern der Klassentypen, die Sie Alias:

*  Die Member von `object` sind die Member der `System.Object`-Klasse.
*  Die Member von `string` sind die Member der `System.String`-Klasse.

### <a name="interface-members"></a>Schnittstellenmember

Die Member einer Schnittstelle sind die Member, die in der-Schnittstelle und in allen Basis Schnittstellen der-Schnittstelle deklariert werden. Die Member in der Klasse `object` sind nicht, streng genommen Member einer beliebigen Schnittstelle ([Schnittstellenmember](interfaces.md#interface-members)). Allerdings sind die Member in der Klasse `object` über die Element Suche in einem beliebigen Schnittstellentyp ([Member-Suche](expressions.md#member-lookup)) verfügbar.

### <a name="array-members"></a>Array Elemente

Die Member eines Arrays sind die Member, die von der Klasse `System.Array` geerbt werden.

### <a name="delegate-members"></a>Delegatmember

Die Member eines Delegaten sind die Member, die von der Klasse `System.Delegate` geerbt werden.

## <a name="member-access"></a>Memberzugriff

Deklarationen von Membern ermöglichen die Steuerung des Member-Zugriffs. Der Zugriff auf einen Member wird durch die deklarierte Barrierefreiheit (als[Barrierefreiheit deklariert](basic-concepts.md#declared-accessibility)) des Members in Kombination mit dem Zugriff auf den direkt enthaltenden Typ hergestellt, sofern vorhanden.

Wenn der Zugriff auf ein bestimmtes Element zulässig ist, wird der Zugriff auf den Member als ***verfügbar***bezeichnet. Wenn der Zugriff auf ein bestimmtes Element nicht zulässig ist ***, wird der***Zugriff auf den Member als nicht zulässig bezeichnet. Der Zugriff auf ein Mitglied ist zulässig, wenn der Text Speicherort, in dem der Zugriff erfolgt, in der Zugriffs Domäne ([Barrierefreiheits Domänen](basic-concepts.md#accessibility-domains)) des Mitglieds enthalten ist.

### <a name="declared-accessibility"></a>Deklarierter Zugriff

Die ***deklarierte Barrierefreiheit*** eines Members kann eine der folgenden sein:

*  Public, das durch Einschließen eines `public`-Modifizierers in die Element Deklaration ausgewählt wird. Die intuitive Bedeutung von `public` ist "Zugriff nicht beschränkt".
*  Geschützt, das durch Einschließen eines `protected`-Modifizierers in die Element Deklaration ausgewählt wird. Die intuitive Bedeutung von `protected` ist "der Zugriff ist auf die enthaltende Klasse oder auf Typen beschränkt, die von der enthaltenden Klasse abgeleitet sind".
*  Intern, das durch Einschließen eines `internal`-Modifizierers in die Element Deklaration ausgewählt wird. Die intuitive Bedeutung von `internal` ist "Zugriff beschränkt auf dieses Programm".
*  Geschützter interner (d.h. geschützt oder intern), der durch das Einschließen eines `protected`-und eines `internal`-Modifizierers in die Element Deklaration ausgewählt wird. Die intuitive Bedeutung von `protected internal` ist "der Zugriff ist auf dieses Programm oder auf Typen beschränkt, die von der enthaltenden Klasse abgeleitet sind".
*  Privat, das durch Einschließen eines `private`-Modifizierers in die Element Deklaration ausgewählt wird. Die intuitive Bedeutung von `private` ist "der Zugriff ist auf den enthaltenden Typ beschränkt".

Abhängig vom Kontext, in dem eine Element Deklaration stattfindet, sind nur bestimmte Typen von deklarierter Barrierefreiheit zulässig. Wenn eine Member-Deklaration keine Zugriffsmodifizierer enthält, bestimmt der Kontext, in dem die Deklaration stattfindet, die standardmäßige deklarierte Barrierefreiheit.

*  Namespaces verfügen implizit über `public` deklarierte Zugriffsmöglichkeiten. Für Namespace Deklarationen sind keine Zugriffsmodifizierer zulässig.
*  Typen, die in Kompilierungs Einheiten oder Namespaces deklariert sind, können `public` oder `internal` als Barrierefreiheit deklariert haben und standardmäßig `internal` deklarierten Barrierefreiheit aufweisen.
*  Klassenmember können eine der fünf Arten von deklarierten zugreif barkeit aufweisen und standardmäßig auf `private` deklarierte Barrierefreiheit. (Beachten Sie, dass ein Typ, der als Member einer Klasse deklariert wird, eine der fünf Arten von deklarierten zugreif barkeit haben kann, wohingegen ein als Member eines Namespace deklarierter Typ nur `public` oder `internal` als Barrierefreiheit deklariert hat.)
*  Strukturmember können `public`, `internal` oder `private` als Barrierefreiheit deklariert haben und standardmäßig `private` deklarierten Barrierefreiheit aufweisen, da Strukturen implizit versiegelt sind. Strukturmember, die in einer Struktur eingeführt werden (d. h. nicht von dieser Struktur geerbt), dürfen nicht `protected`-oder `protected internal`-Barrierefreiheit haben. (Beachten Sie, dass ein Typ, der als Member einer Struktur deklariert ist, `public`, `internal` oder `private` als Barrierefreiheit deklariert hat. ein als Member eines Namespace deklarierter Typ kann jedoch nur `public` oder `internal` als Barrierefreiheit deklarierte Zugriffsmöglichkeiten aufweisen.)
*  Schnittstellenmember verfügen implizit über `public` deklarierte Zugriffsmöglichkeiten. Zugriffsmodifizierer sind für Schnittstellenmember-Deklarationen unzulässig
*  Enumerationsmember haben implizit `public` deklarierte Zugriffsmöglichkeiten. Es sind keine Zugriffsmodifizierer für Enumerationsmember zulässig.

### <a name="accessibility-domains"></a>Barrierefreiheits Domänen

Die Zugriffs ***Domäne*** eines Members besteht aus den (möglicherweise zusammenhängenden) Abschnitten von Programmtext, in dem der Zugriff auf den Member zulässig ist. Zum Definieren der Zugriffs Domäne eines Members wird ein Member als ***oberste Ebene*** bezeichnet, wenn er nicht innerhalb eines Typs deklariert ist, und ein Member wird als geschachtelt bezeichnet, wenn er in einem anderen Typ deklariert wird. Außerdem wird der ***Programmtext*** eines Programms als sämtlicher Programmtext definiert, der in allen Quelldateien des Programms enthalten ist, und der Programmtext eines Typs wird als sämtlicher Programmtext definiert, der in den *type_declaration*s dieses Typs enthalten ist (einschließlich). Möglicherweise sind Typen, die innerhalb des Typs geschachtelt sind.

Die Zugriffs Domäne eines vordefinierten Typs (z. b. `object`, `int` oder `double`) ist unbegrenzt.

Die Zugriffs Domäne eines ungebundenen Typs der obersten Ebene `T` ([gebundene und ungebundene Typen](types.md#bound-and-unbound-types)), die in einem Programm `P` deklariert ist, wird wie folgt definiert:

*  Wenn die deklarierte Barrierefreiheit von `T` `public` ist, ist die Zugriffs Domäne von `T` der Programmtext von `P` und jedes Programm, das auf `P` verweist.
*  Wenn die deklarierte Zugriffsart von `T` den Wert `internal` hat, entspricht die Zugriffsdomäne von `T` dem Programmtext von `P`.

Aus diesen Definitionen folgt, dass die Zugriffs Domäne eines ungebundenen Typs der obersten Ebene immer mindestens dem Programmtext des Programms entspricht, in dem der Typ deklariert ist.

Die Barrierefreiheits Domäne für einen konstruierten Typ `T<A1, ..., An>` ist die Schnittmenge der Barrierefreiheits Domäne des ungebundenen generischen Typs `T` und der Barrierefreiheits Domänen der Typargumente `A1, ..., An`.

Die Zugriffs Domäne @no__t eines geschachtelten Members, der in einem Typ `T` in einem Programm `P` deklariert ist, wird wie folgt definiert (Beachten Sie, dass `M` selbst möglicherweise ein Typ sein kann):

*  Wenn die deklarierte Zugriffsart von `M` den Wert `public` hat, entspricht die Zugriffsdomäne von `M` der von `T`.
*  Wenn die deklarierte Barrierefreiheit von `M` `protected internal` ist, lassen Sie `D` die Vereinigung des Programm Texts von `P` und den Programmtext eines beliebigen Typs sein, der von `T` abgeleitet ist, der außerhalb von `P` deklariert ist. Die Zugriffs Domäne `M` ist die Schnittmenge der Zugriffs Domäne `T` mit `D`.
*  Wenn die deklarierte Barrierefreiheit von `M` `protected` ist, lassen Sie `D` die Vereinigung des Programm Texts von `T` und den Programmtext jedes Typs, der von `T` abgeleitet ist, sein. Die Zugriffs Domäne `M` ist die Schnittmenge der Zugriffs Domäne `T` mit `D`.
*  Wenn die deklarierte Zugriffsart von `M` den Wert `internal` hat, entspricht die Zugriffsdomäne von `M` der Schnittmenge zwischen der Zugriffsdomäne von `T` und dem Programmtext von `P`.
*  Wenn die deklarierte Zugriffsart von `M` den Wert `private` hat, entspricht die Zugriffsdomäne von `M` dem Programmtext von `T`.

Aus diesen Definitionen folgt, dass die Barrierefreiheits Domäne eines in einem Bereich eingefügten Members immer mindestens dem Programmtext des Typs entspricht, in dem der Member deklariert ist. Ferner folgt, dass die Zugriffs Domäne eines Members nie inklusiver ist als die Zugriffs Domäne des Typs, in dem der Member deklariert ist.

Wenn auf einen Typ oder Member `M` zugegriffen wird, werden die folgenden Schritte in intuitiver Hinsicht ausgewertet, um sicherzustellen, dass der Zugriff zulässig ist:

*  Wenn `M` innerhalb eines Typs deklariert ist (im Gegensatz zu einer Kompilierungseinheit oder einem Namespace), tritt zunächst ein Kompilierzeitfehler auf, wenn auf diesen Typ nicht zugegriffen werden kann.
*  Wenn `M` `public` ist, ist der Zugriff zulässig.
*  Andernfalls, wenn `M` `protected internal` ist, ist der Zugriff zulässig, wenn er innerhalb des Programms auftritt, in dem `M` deklariert ist, oder wenn es in einer Klasse auftritt, die von der Klasse abgeleitet ist, in der `M` deklariert ist, und durch den abgeleiteten Klassentyp (geschützt) erfolgt.[ Zugriff für Instanzmember](basic-concepts.md#protected-access-for-instance-members)).
*  Andernfalls, wenn `M` `protected` ist, ist der Zugriff zulässig, wenn er innerhalb der Klasse auftritt, in der `M` deklariert ist, oder wenn er in einer Klasse auftritt, die von der Klasse abgeleitet ist, in der `M` deklariert ist und durch den Typ der abgeleiteten Klasse erfolgt ([geschützt Zugriff für Instanzmember](basic-concepts.md#protected-access-for-instance-members)).
*  Andernfalls ist der Zugriff zulässig, wenn `M` `internal` ist, wenn er innerhalb des Programms auftritt, in dem `M` deklariert ist.
*  Andernfalls ist der Zugriff zulässig, wenn `M` `private` ist, wenn er innerhalb des Typs auftritt, in dem `M` deklariert ist.
*  Andernfalls ist der Typ oder Member nicht zugänglich, und es tritt ein Kompilierzeitfehler auf.

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
die Klassen und Member haben die folgenden Barrierefreiheits Domänen:

*  Die Zugriffs Domäne `A` und `A.X` ist unbegrenzt.
*  Die Zugriffs Domäne `A.Y`, `B`, `B.X`, `B.Y`, `B.C`, `B.C.X` und `B.C.Y` ist der Programmtext des enthaltenden Programms.
*  Die Zugriffs Domäne `A.Z` ist der Programmtext von `A`.
*  Die Zugriffs Domäne `B.Z` und `B.D` ist der Programmtext von `B`, einschließlich des Programm Texts von `B.C` und `B.D`.
*  Die Zugriffs Domäne `B.C.Z` ist der Programmtext von `B.C`.
*  Die Zugriffs Domäne `B.D.X` und `B.D.Y` ist der Programmtext von `B`, einschließlich des Programm Texts von `B.C` und `B.D`.
*  Die Zugriffs Domäne `B.D.Z` ist der Programmtext von `B.D`.

Wie das Beispiel zeigt, ist die Zugriffs Domäne eines Members nie größer als die eines enthaltenden Typs. Wenn z. b. alle `X`-Member öffentlich deklarierte Barrierefreiheit haben, verfügen alle, aber `A.X` über Barrierefreiheits Domänen, die durch einen enthaltenden Typ eingeschränkt werden.

Wie in [Members](basic-concepts.md#members)beschrieben, werden alle Member einer Basisklasse, mit Ausnahme von Instanzkonstruktoren, destrukturiertoren und statischen Konstruktoren, von abgeleiteten Typen geerbt. Dies schließt sogar private Member einer Basisklasse ein. Die Zugriffs Domäne eines privaten Members enthält jedoch nur den Programmtext des Typs, in dem der Member deklariert ist. Im Beispiel
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
die `B`-Klasse erbt den privaten Member `x` von der `A`-Klasse. Da der Member privat ist, kann er nur innerhalb des *class_body* von `A` aufgerufen werden. Folglich ist der Zugriff auf `b.x` in der `A.F`-Methode erfolgreich, schlägt jedoch in der `B.F`-Methode fehl.

### <a name="protected-access-for-instance-members"></a>Geschützter Zugriff für Instanzmember

Wenn auf einen `protected`-Instanzmember außerhalb des Programm Texts der Klasse zugegriffen wird, in der er deklariert ist, und wenn auf einen `protected internal`-Instanzmember außerhalb des Programm Texts des Programms zugegriffen wird, in dem es deklariert ist, muss der Zugriff innerhalb einer Klasse erfolgen. Deklaration, die von der Klasse abgeleitet ist, in der Sie deklariert ist. Darüber hinaus muss der Zugriff durch eine Instanz dieses abgeleiteten Klassen Typs oder eines aus der Klasse erstellten Klassen Typs erfolgen. Diese Einschränkung verhindert, dass eine abgeleitete Klasse auf geschützte Member anderer abgeleiteter Klassen zugreift, auch wenn die Elemente von derselben Basisklasse geerbt werden.

Let `B` ist eine Basisklasse, die einen geschützten Instanzmember `M` deklariert und `D` eine Klasse sein kann, die von `B` abgeleitet ist. Innerhalb des *class_body* -`D` kann der Zugriff auf `M` eine der folgenden Formen annehmen:

*  Ein nicht qualifizierter *TYPE_NAME* oder *primary_expression* der Form `M`.
*  Ein *primary_expression* im Format `E.M`, wenn der Typ von `E` `T` ist, oder eine Klasse, die von `T` abgeleitet ist, wobei `T` der Klassentyp `D` oder ein Klassentyp ist, der aus `D` erstellt wurde.
*  Ein *primary_expression* -Format `base.M`.

Zusätzlich zu diesen Zugriffs Formen kann eine abgeleitete Klasse auf einen geschützten Instanzkonstruktor einer Basisklasse in einem *constructor_initializer* ([Konstruktorinitialisierer](classes.md#constructor-initializers)) zugreifen.

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
in `A` ist es möglich, über Instanzen von `A` und `B` auf `x` zuzugreifen, da der Zugriff in beiden Fällen durch eine Instanz von `A` oder eine von `A` abgeleitete Klasse erfolgt. In `B` ist es jedoch nicht möglich, auf `x` über eine Instanz von `A` zuzugreifen, da `A` nicht von `B` abgeleitet ist.

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
die drei Zuweisungen zu `x` sind zulässig, da Sie alle über Instanzen von Klassentypen erfolgen, die aus dem generischen Typ erstellt werden.

### <a name="accessibility-constraints"></a>Barrierefreiheits Einschränkungen

Mehrere Konstrukte in C# der Sprache erfordern, dass ein Typ mindestens ***so zugänglich ist wie*** ein Member oder ein anderer Typ. Ein Typ `T` ist zumindest so, dass er mindestens so zugänglich ist wie ein Member oder Typ `M`, wenn die Barrierefreiheits Domäne von `T` eine supermenge der Barrierefreiheits Domäne `M` ist. Anders ausgedrückt: `T` ist zumindest so verfügbar wie `M`, wenn `T` in allen Kontexten zugänglich ist, in denen `M` zugänglich ist.

Die folgenden Barrierefreiheits Einschränkungen sind vorhanden:

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
*  Die Parametertypen eines Instanzkonstruktors müssen mindestens so zugänglich sein wie der Instanzkonstruktor selbst.

Im Beispiel
```csharp
class A {...}

public class B: A {...}
```
die `B`-Klasse führt zu einem Kompilierzeitfehler, da `A` nicht mindestens so zugänglich ist wie `B`.

Gleiches gilt für das Beispiel
```csharp
class A {...}

public class B
{
    A F() {...}

    internal A G() {...}

    public A H() {...}
}
```
die `H`-Methode in `B` führt zu einem Kompilierzeitfehler, da der Rückgabetyp `A` nicht mindestens so zugänglich ist wie die-Methode.

## <a name="signatures-and-overloading"></a>Signaturen und überladen

Methoden, Instanzkonstruktoren, Indexer und Operatoren sind durch ihre ***Signaturen***gekennzeichnet:

*  Die Signatur einer Methode besteht aus dem Namen der Methode, der Anzahl der Typparameter und dem Typ und der Art (Wert, Verweis oder Ausgabe) der einzelnen formalen Parameter, die in der Reihenfolge von links nach rechts berücksichtigt werden. Zu diesem Zweck wird jeder Typparameter der Methode, die im Typ eines formalen Parameters vorkommt, nicht anhand seines Namens identifiziert, sondern anhand seiner Ordinalposition in der Typargument Liste der Methode. Die Signatur einer Methode enthält nicht den Rückgabetyp, den `params`-Modifizierer, der für den ganz rechts Esten Parameter angegeben werden kann, oder die optionalen Typparameter Einschränkungen.
*  Die Signatur eines Instanzkonstruktors besteht aus Typ und Art (Wert, Verweis oder Ausgabe) der einzelnen formalen Parameter, die in der Reihenfolge von links nach rechts berücksichtigt werden. Die Signatur eines Instanzkonstruktors schließt speziell den `params`-Modifizierer nicht ein, der für den ganz rechts Esten Parameter angegeben werden kann.
*  Die Signatur eines Indexers besteht aus dem Typ der einzelnen formalen Parameter, die in der Reihenfolge von links nach rechts berücksichtigt werden. Die Signatur eines Indexers enthält weder den Elementtyp noch den `params`-Modifizierer, der für den ganz rechts Esten Parameter angegeben werden kann.
*  Die Signatur eines Operators besteht aus dem Namen des Operators und dem Typ der einzelnen formalen Parameter, die in der Reihenfolge von links nach rechts betrachtet werden. Die Signatur eines Operators schließt den Ergebnistyp nicht ein.

Signaturen sind der aktivierende Mechanismus für das ***überladen*** von Membern in Klassen, Strukturen und Schnittstellen:

*  Das Überladen von Methoden ermöglicht einer Klasse, Struktur oder Schnittstelle das Deklarieren mehrerer Methoden mit demselben Namen, vorausgesetzt, ihre Signaturen sind innerhalb dieser Klasse, Struktur oder Schnittstelle eindeutig.
*  Das Überladen von Instanzkonstruktoren ermöglicht einer Klasse oder Struktur das Deklarieren mehrerer Instanzkonstruktoren, vorausgesetzt, ihre Signaturen sind innerhalb dieser Klasse oder Struktur eindeutig.
*  Das Überladen von indexatoren ermöglicht es einer Klasse, Struktur oder Schnittstelle, mehrere Indexer zu deklarieren, vorausgesetzt, ihre Signaturen sind innerhalb dieser Klasse, Struktur oder Schnittstelle eindeutig.
*  Das Überladen von Operatoren ermöglicht einer Klasse oder Struktur das Deklarieren mehrerer Operatoren mit demselben Namen, vorausgesetzt, ihre Signaturen sind innerhalb dieser Klasse oder Struktur eindeutig.

Obwohl `out`-und `ref`-Parametermodifizierer als Teil einer Signatur angesehen werden, können in einem einzelnen Typ deklarierte Member nicht in der Signatur ausschließlich von `ref` und `out` abweichen. Ein Kompilierzeitfehler tritt auf, wenn zwei Member im gleichen Typ mit Signaturen deklariert werden, die identisch wären, wenn alle Parameter in beiden Methoden mit `out`-modifiziererelementen in `ref`-modifiziererer geändert wurden. Für andere Zwecke des Signatur Abgleich (z. b. ausblenden oder überschreiben) werden die `ref` und `out` als Teil der Signatur angesehen und stimmen nicht überein. (Diese Einschränkung besteht darin, C# dass Programme auf einfache Weise für die Common Language Infrastructure (CLI) übersetzt werden können, die keine Möglichkeit bietet, Methoden zu definieren, die sich nur in `ref` und `out` unterscheiden.)

Für Signaturen werden die Typen `object` und `dynamic` als identisch betrachtet. Member, die in einem einzelnen Typ deklariert werden, können sich daher nicht nur durch `object` und `dynamic` in der Signatur unterscheiden.

Das folgende Beispiel zeigt eine Reihe von überladenen Methoden Deklarationen zusammen mit ihren Signaturen.
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

Beachten Sie, dass die Parametermodifizierer "`ref`" und "`out`" ([Methoden Parameter](classes.md#method-parameters)) Teil einer Signatur sind. Daher sind `F(int)` und `F(ref int)` eindeutige Signaturen. Allerdings können `F(ref int)` und `F(out int)` nicht innerhalb derselben Schnittstelle deklariert werden, da sich Ihre Signaturen ausschließlich durch `ref` und `out` unterscheiden. Beachten Sie außerdem, dass der Rückgabetyp und der `params`-Modifizierer nicht Teil einer Signatur sind. Daher ist es nicht möglich, nur auf der Grundlage des Rückgabe Typs oder auf dem einschließen oder ausschließen des `params`-Modifizierers zu überladen. Daher führen die Deklarationen der oben identifizierten Methoden `F(int)` und `F(params string[])` zu einem Kompilierzeitfehler.

## <a name="scopes"></a>Bereiche

Der Gültigkeitsbereich eines Namens ist der ***Bereich*** des Programm Texts, in dem auf die durch den Namen deklarierte Entität ohne Qualifikation des Namens verwiesen werden kann. Bereiche können ***geschachtelt***werden, und ein innerer Bereich kann die Bedeutung eines Namens aus einem äußeren Gültigkeitsbereich neu deklarieren (Dies bedeutet jedoch nicht, dass die Einschränkung, die von [Deklarationen aufgrund von Deklarationen](basic-concepts.md#declarations) in einem geschachtelten Block festgelegt wird, nicht möglich ist, eine lokale Variable mit dem gleicher Name wie eine lokale Variable in einem einschließenden-Block). Der Name aus dem äußeren Gültigkeitsbereich wird dann in der vom inneren Bereich behandelten Programm Textzeile ***ausgeblendet*** , und der Zugriff auf den äußeren Namen ist nur durch Qualifizierung des Namens möglich.

*  Der Gültigkeitsbereich eines Namespace Members, der von einem *namespace_member_declaration* ([Namespace](namespaces.md#namespace-members)-Member) ohne einschließendes *namespace_declaration* deklariert wird, ist der gesamte Programmtext.
*  Der Gültigkeitsbereich eines Namespace Members, der von einem *namespace_member_declaration* innerhalb eines *namespace_declaration* deklariert wird, dessen voll qualifizierter Name `N` ist, ist die *namespace_body* jedes *namespace_declaration* , dessen vollständig der qualifizierte Name ist `N` oder beginnt mit `N`, gefolgt von einem bestimmten Zeitraum.
*  Der durch ein *extern_alias_directive* definierte Bereich des Namens erstreckt sich über die *using_directive*s, *global_attributes* und *namespace_member_declaration*s der unmittelbar enthaltenden Kompilierungseinheit oder des Namespace Texts. Ein *extern_alias_directive* führt keine neuen Member zum zugrunde liegenden Deklarations Bereich ein. Anders ausgedrückt: ein *extern_alias_directive* -Wert ist nicht transitiv, sondern wirkt sich nur auf die Kompilierungseinheit oder den Namespace Körper aus, in dem er auftritt.
*  Der Bereich eines Namens, der von einem *using_directive* (using-[Direktiven](namespaces.md#using-directives)) definiert oder importiert wird, erstreckt sich über die *namespace_member_declaration*s von *compilation_unit* oder *namespace_body* , in denen das *using_directive* tritt auf. Ein *using_directive* kann NULL oder mehr Namespace-, Typ-oder Elementnamen innerhalb eines bestimmten *compilation_unit* oder *namespace_body*zur Verfügung stellen, führt jedoch keine neuen Member zum zugrunde liegenden Deklarations Bereich. Anders ausgedrückt: ein *using_directive* -Wert ist nicht transitiv, sondern wirkt sich nur auf den *compilation_unit* oder *namespace_body* aus, in dem er auftritt.
*  Der Gültigkeitsbereich eines Typparameters, der von einem *type_parameter_list* -Wert in einem *class_declaration* ([Klassen Deklarationen](classes.md#class-declarations)) deklariert wird, sind *class_base*, *type_parameter_constraints_clause*s und *class_body* der  *class_declaration*.
*  Der Gültigkeitsbereich eines Typparameters, der durch eine *type_parameter_list* -Deklaration in einem *struct_declaration* ([Struktur Deklarationen](structs.md#struct-declarations)) deklariert wird, sind *struct_interfaces*, *type_parameter_constraints_clause*s und *struct_body* von Diese *struct_declaration*.
*  Der Gültigkeitsbereich eines Typparameters, der durch eine *type_parameter_list* -Deklaration in einer *interface_declaration* ([Schnittstellen Deklaration](interfaces.md#interface-declarations)) deklariert wird, sind *interface_base*, *type_parameter_constraints_clause*s und *interface_body* dieses *interface_declaration*.
*  Der Gültigkeitsbereich eines Typparameters, der von einem *type_parameter_list* -Objekt in einem *delegate_declaration* ([Delegatdeklarationen](delegates.md#delegate-declarations)) deklariert wird, sind *return_type*, *formal_parameter_list*und *type_parameter_constraints_clause.* s dieses *delegate_declaration*.
*  Der Gültigkeitsbereich eines Members, der von einem *class_member_declaration* ([Klassen Text](classes.md#class-body)) deklariert wird, ist die *class_body* , in der die Deklaration auftritt. Außerdem erstreckt sich der Gültigkeitsbereich eines Klassenmembers auf den *class_body* der abgeleiteten Klassen, die in der Zugriffs Domäne ([Barrierefreiheits Domänen](basic-concepts.md#accessibility-domains)) des Members enthalten sind.
*  Der Gültigkeitsbereich eines Members, der von einem *struct_member_declaration* ([Strukturmember](structs.md#struct-members)) deklariert wird, ist die *struct_body* , in der die Deklaration erfolgt.
*  Der Gültigkeitsbereich eines Members, der von einem *enum_member_declaration* ([enum](enums.md#enum-members)-Member) deklariert wird, ist die *enum_body* , in der die Deklaration erfolgt.
*  Der Gültigkeitsbereich eines in *method_declaration* ([Methoden](classes.md#methods)) deklarierten Parameters ist der *method_body* dieses *method_declaration*.
*  Der Gültigkeitsbereich eines in einem *indexer_declaration* ([Indexer](classes.md#indexers)) deklarierten Parameters ist der *accessor_declarations* dieses *indexer_declaration*.
*  Der Gültigkeitsbereich eines in *operator_declaration* ([Operatoren](classes.md#operators)) deklarierten Parameters ist der *Block* dieser *operator_declaration*.
*  Der Gültigkeitsbereich eines in einem *constructor_declaration* ([Instanzkonstruktors](classes.md#instance-constructors)) deklarierten Parameters ist der *constructor_initializer* und *Block* dieses *constructor_declaration*.
*  Der Gültigkeitsbereich eines in einem *lambda_expression* ([anonymen Funktions Ausdruck](expressions.md#anonymous-function-expressions)) deklarierten Parameters ist *anonymous_function_body* dieser *lambda_expression*
*  Der Gültigkeitsbereich eines in einem *anonymous_method_expression* ([anonymen Funktions Ausdruck](expressions.md#anonymous-function-expressions)) deklarierten Parameters ist der *Block* dieser *anonymous_method_expression*.
*  Der Gültigkeitsbereich einer Bezeichnung, die in einer *labeled_statement* -[Anweisung (bezeichnet](statements.md#labeled-statements)) deklariert ist, ist der *Block* , in dem die Deklaration auftritt.
*  Der Gültigkeitsbereich einer lokalen Variablen, die in einem *local_variable_declaration* ([lokale Variablen Deklarationen](statements.md#local-variable-declarations)) deklariert ist, ist der Block, in dem die Deklaration auftritt.
*  Der Gültigkeitsbereich einer lokalen Variablen, die in einer *switch_block* -Anweisung einer `switch`-Anweisung ([der Switch-Anweisung](statements.md#the-switch-statement)) deklariert ist, ist *switch_block*.
*  Der Gültigkeitsbereich einer lokalen Variablen, die in einer *for_initializer* -Anweisung einer `for`-Anweisung deklariert ist ([for-Anweisung](statements.md#the-for-statement)), ist *for_initializer*, *for_condition*, *for_iterator*und die enthaltene *Anweisung* von `for`-Anweisung.
*  Der Gültigkeitsbereich einer lokalen Konstante, die in *local_constant_declaration* ([lokale Konstante Deklarationen](statements.md#local-constant-declarations)) deklariert ist, ist der Block, in dem die Deklaration auftritt. Es handelt sich um einen Kompilierzeitfehler, der auf eine lokale Konstante in einer Textposition verweist, die dem *constant_declarator*vorangestellt ist.
*  Der Gültigkeitsbereich einer Variablen, die als Teil von *foreach_statement*, *using_statement*, *lock_statement* oder *query_expression* deklariert ist, wird durch die Erweiterung des angegebenen Konstrukts bestimmt.

Innerhalb des Gültigkeits Bereichs eines Namespace, einer Klasse, einer Struktur oder eines Enumerationsmembers kann auf das Element in einer Textposition verwiesen werden, die der Deklaration des Members vorangestellt ist. Beispiel:
```csharp
class A
{
    void F() {
        i = 1;
    }

    int i = 0;
}
```
Hier ist es gültig, dass `F` auf `i` verweist, bevor es deklariert wird.

Innerhalb des Gültigkeits Bereichs einer lokalen Variablen ist dies ein Kompilierzeitfehler, der auf die lokale Variable in einer Textposition verweist, die dem *local_variable_declarator* der lokalen Variablen vorangestellt ist. Beispiel:
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

In der obigen Methode `F` verweist die erste Zuweisung an `i` nicht auf das im äußeren Gültigkeitsbereich deklarierte Feld. Stattdessen verweist Sie auf die lokale Variable und führt zu einem Kompilierzeitfehler, da Sie sich textumal der Deklaration der Variablen vorangestellt ist. In der `G`-Methode ist die Verwendung von `j` im Initialisierer für die Deklaration von `j` gültig, da die Verwendung dem *local_variable_declarator*nicht vorangestellt ist. In der `H`-Methode verweist eine nachfolgende *local_variable_declarator* ordnungsgemäß auf eine lokale Variable, die in einem früheren *local_variable_declarator* innerhalb desselben *local_variable_declaration*deklariert wurde.

Die Bereichs Regeln für lokale Variablen dienen dazu, sicherzustellen, dass die Bedeutung eines Namens, der in einem Ausdrucks Kontext verwendet wird, innerhalb eines-Blocks immer identisch ist. Wenn der Gültigkeitsbereich einer lokalen Variablen nur von der Deklaration bis zum Ende des Blocks erweitert werden soll, wird im obigen Beispiel die erste Zuweisung der Instanzvariablen zugewiesen, und die zweite Zuweisung würde der lokalen Variablen zugewiesen, was möglicherweise zu Kompilierzeitfehler, wenn die Anweisungen des Blocks später neu angeordnet wurden.

Die Bedeutung eines Namens innerhalb eines-Blocks kann sich je nach Kontext unterscheiden, in dem der Name verwendet wird. Im Beispiel
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
der Name `A` wird in einem Ausdrucks Kontext verwendet, um auf die lokale Variable `A` und in einem typkontext zu verweisen, um auf die-Klasse `A` zu verweisen.

### <a name="name-hiding"></a>Namens ausblenden

Der Gültigkeitsbereich einer Entität umfasst in der Regel mehr Programmtext als der Deklarations Bereich der Entität. Der Gültigkeitsbereich einer Entität kann insbesondere Deklarationen enthalten, die neue Deklarations Bereiche mit Entitäten mit demselben Namen enthalten. Diese Deklarationen bewirken, dass die ursprüngliche Entität ***ausgeblendet***wird. Umgekehrt wird eine Entität als ***sichtbar*** angezeigt, wenn Sie nicht ausgeblendet ist.

Das Ausblenden von Namen tritt auf, wenn sich Bereiche überlappen Die Merkmale der beiden Arten von ausblenden werden in den folgenden Abschnitten beschrieben.

#### <a name="hiding-through-nesting"></a>Ausblenden durch Schachtelung

Der Name, der durch Schachtelung ausgeblendet wird, kann als Ergebnis der Schachtelung von Namespaces oder Typen innerhalb von Namespaces auftreten, als Ergebnis der Schachtelung von Typen in Klassen oder Strukturen, und als Ergebnis von Parameter-und lokalen Variablen Deklarationen.

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
innerhalb der `F`-Methode wird die Instanzvariable `i` von der lokalen Variablen `i` ausgeblendet, aber innerhalb der `G`-Methode verweist `i` immer noch auf die Instanzvariable.

Wenn ein Name in einem inneren Gültigkeitsbereich einen Namen in einem äußeren Gültigkeitsbereich verbirgt, werden alle überladenen Vorkommen dieses Namens ausgeblendet. Im Beispiel
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
der Aufruf `F(1)` ruft die in `Inner` deklarierte `F` auf, da alle äußeren Vorkommen von `F` durch die innere Deklaration ausgeblendet werden. Aus demselben Grund führt der Aufruf `F("Hello")` zu einem Kompilierzeitfehler.

#### <a name="hiding-through-inheritance"></a>Ausblenden durch Vererbung

Das Ausblenden des Namens durch Vererbung tritt auf, wenn Klassen oder Strukturen Namen, die von Basisklassen geerbt wurden, neu deklarieren. Diese Art von namens ausblenden hat eine der folgenden Formen:

*  Eine Konstante, ein Feld, eine Eigenschaft, ein Ereignis oder ein Typ, die in einer Klasse oder Struktur eingeführt wurde, verbergen alle Basisklassenmember mit demselben Namen.
*  Eine Methode, die in einer Klasse oder Struktur eingeführt wurde, verbirgt alle nicht-Method-Basisklassenmember mit demselben Namen und alle Basisklassen Methoden mit der gleichen Signatur (Methodenname und Parameter Anzahl, Modifizierer und Typen).
*  Ein Indexer, der in einer Klasse oder Struktur eingeführt wurde, verbirgt alle Basisklassenindexer mit der gleichen Signatur (Parameter Anzahl und-Typen).

Die Regeln für Operator Deklarationen ([Operatoren](classes.md#operators)) machen es für eine abgeleitete Klasse unmöglich, einen Operator mit derselben Signatur wie ein Operator in einer Basisklasse zu deklarieren. Folglich verbergen Operatoren nie eine andere.

Im Gegensatz zum Ausblenden eines Namens aus einem äußeren Gültigkeitsbereich bewirkt das Ausblenden eines zugänglichen namens aus einem geerbten Bereich, dass eine Warnung ausgegeben wird. Im Beispiel
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
die Deklaration von "`F`" in `Derived` bewirkt, dass eine Warnung gemeldet wird. Das Ausblenden eines geerbten Namens ist kein Fehler, da dies die getrennte Weiterentwicklung von Basisklassen ausschließen würde. Die obige Situation könnte z. b. eintreten, weil eine neuere Version von `Base` eine `F`-Methode eingeführt hat, die in einer früheren Version der Klasse nicht vorhanden war. Hätte die obige Situation einen Fehler verursacht, kann jede Änderung, die an einer Basisklasse in einer separat versionierten Klassenbibliothek vorgenommen wurde, möglicherweise dazu führen, dass abgeleitete Klassen ungültig werden.

Die Warnung, die durch das Ausblenden eines geerbten Namens verursacht wurde, kann durch die Verwendung des `new`-Modifizierers gelöscht werden:
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

Der `new`-Modifizierer gibt an, dass der `F` in `Derived` "New" ist, und dass er tatsächlich dazu gedacht ist, den geerbten Member auszublenden.

Eine Deklaration eines neuen Members verbirgt einen geerbten Member nur innerhalb des Gültigkeits Bereichs des neuen Members.

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

Im obigen Beispiel blendet die Deklaration von `F` in `Derived` die `F` aus, die von `Base` geerbt wurde. da der neue `F` in `Derived` jedoch über privaten Zugriff verfügt, wird sein Bereich nicht auf `MoreDerived` erweitert. Daher ist der Aufruf `F()` in `MoreDerived.G` gültig und wird `Base.F` aufgerufen.

## <a name="namespace-and-type-names"></a>Namespace-und Typnamen

Mehrere Kontexte in einem C# Programm erfordern, dass ein *namespace_name* oder ein *TYPE_NAME* angegeben wird.

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

Ein *namespace_name* ist eine *namespace_or_type_name* , die auf einen Namespace verweist. Gemäß der unten beschriebenen Auflösung muss *namespace_or_type_name* eines *namespace_name* auf einen Namespace verweisen, andernfalls tritt ein Kompilierzeitfehler auf. In einem *namespace_name* -Typ können keine Typargumente ([Typargumente](types.md#type-arguments)) vorhanden sein (nur Typen können Typargumente aufweisen).

Ein *TYPE_NAME* ist ein *namespace_or_type_name* , das auf einen Typ verweist. Gemäß der unten beschriebenen Auflösung muss *namespace_or_type_name* eines *TYPE_NAME* auf einen Typ verweisen, andernfalls tritt ein Kompilierzeitfehler auf.

Wenn *namespace_or_type_name* ein Qualified-Alias-Member ist, wird die Bedeutung in [Namespacealias-Qualifizierern](namespaces.md#namespace-alias-qualifiers)beschrieben. Andernfalls hat eine *namespace_or_type_name* eine von vier Formularen:

*  `I`
*  `I<A1, ..., Ak>`
*  `N.I`
*  `N.I<A1, ..., Ak>`

Wenn `I` ein einzelner Bezeichner ist, ist `N` ein *namespace_or_type_name* und `<A1, ..., Ak>` ein optionales *type_argument_list*. Wenn *type_argument_list* nicht angegeben ist, sollten Sie `k` NULL sein.

Die Bedeutung eines *namespace_or_type_name* wird wie folgt bestimmt:

*   Wenn der *namespace_or_type_name* die Form `I` oder der Form `I<A1, ..., Ak>` hat:
    * Wenn `K` 0 (null) ist und die *namespace_or_type_name* innerhalb einer generischen Methoden Deklaration ([Methoden](classes.md#methods)) enthalten ist und diese Deklaration einen Typparameter ([Typparameter](classes.md#type-parameters)) mit dem Namen @ no__t-4 enthält, wird *namespace_or_type_ Name* verweist auf diesen Typparameter.
    * Andernfalls, wenn *namespace_or_type_name* innerhalb einer Typdeklaration angezeigt wird, dann für jeden Instanztyp @ no__t-1 ([der Instanztyp](classes.md#the-instance-type)), beginnend mit dem Instanztyp dieser Typdeklaration und fortsetzen mit dem Instanztyp der einzelnen umschließende Klassen-oder Struktur Deklaration (sofern vorhanden):
        * Wenn `K` 0 (null) ist und die Deklaration von `T` einen Typparameter mit dem Namen @ no__t-2 enthält, verweist *namespace_or_type_name* auf diesen Typparameter.
        * Andernfalls, wenn *namespace_or_type_name* innerhalb des Texts der Typdeklaration angezeigt wird und `T` oder einer der zugehörigen Basis Typen einen geschachtelten zugänglichen Typ mit den Parametern Name @ no__t-2 und `K` @ no__t-4type enthält, dann wird *namespace_or_type _name* verweist auf diesen Typ, der mit den angegebenen Typargumenten erstellt wurde. Wenn mehr als ein solcher Typ vorhanden ist, wird der in einem stärker abgeleiteten Typ deklarierte Typ ausgewählt. Beachten Sie, dass nicht-Typmember (Konstanten, Felder, Methoden, Eigenschaften, Indexer, Operatoren, Instanzkonstruktoren, Dekonstruktoren und statische Konstruktoren) und Typmember mit einer anderen Anzahl von Typparametern ignoriert werden, wenn die Bedeutung des *namespace_or_type_name*.
    * Wenn die vorherigen Schritte nicht erfolgreich waren, dann für jeden Namespace @ no__t-0, beginnend mit dem Namespace, in dem der *namespace_or_type_name* auftritt, den Vorgang mit jedem einschließenden Namespace (sofern vorhanden) und Ende mit dem globalen Namespace: Folgendes die Schritte werden ausgewertet, bis eine Entität gefunden wird:
        * Wenn `K` 0 (null) und `I` der Name eines Namespace in @ no__t-2 ist, dann gilt Folgendes:
            * Wenn der Speicherort, an dem der *namespace_or_type_name* auftritt, von einer Namespace Deklaration für `N` eingeschlossen wird und die Namespace Deklaration ein *extern_alias_directive* oder *using_alias_directive* enthält, das den Namen @ No zuordnet. __t-4 mit einem Namespace oder Typ, dann ist *namespace_or_type_name* mehrdeutig, und ein Kompilierzeitfehler tritt auf.
            * Andernfalls verweist *namespace_or_type_name* auf den Namespace mit dem Namen "`I`" in `N`.
        * Wenn `N` einen zugänglichen Typ mit den Parametern "Name @ no__t-1" und "`K` @ no__t-3type" enthält, wird Folgendes angezeigt:
            * Wenn `K` 0 (null) ist und die Position, an der das *namespace_or_type_name* auftritt, von einer Namespace Deklaration für `N` eingeschlossen wird und die Namespace Deklaration ein *extern_alias_directive* oder *using_alias_directive* enthält, das ordnet den Namen @ no__t-5 einem Namespace oder Typ zu, dann ist *namespace_or_type_name* mehrdeutig, und ein Kompilierzeitfehler tritt auf.
            * Andernfalls verweist der *namespace_or_type_name* auf den Typ, der mit den angegebenen Typargumenten erstellt wurde.
        * Andernfalls, wenn der Speicherort, an dem der *namespace_or_type_name* auftritt, von einer Namespace Deklaration für `N` eingeschlossen wird:
            * Wenn `K` 0 (null) ist und die Namespace Deklaration ein *extern_alias_directive* -oder *using_alias_directive* -Wert enthält, der den Namen @ no__t-3 einem importierten Namespace oder Typ zuordnet, bezieht sich *namespace_or_type_name* auf das Namespace oder Typ.
            * Andernfalls, wenn die von den *using_namespace_directive*s und *using_alias_directive*s der Namespace Deklaration importierten Namespaces und Typdeklarationen genau einen zugänglichen Typ mit dem Namen @ no__t-2 und `K` @ no__t-4type enthalten. Parameter, dann verweist das *namespace_or_type_name* auf diesen Typ, der mit den angegebenen Typargumenten erstellt wurde.
            * Andernfalls, wenn die Namespaces und Typdeklarationen, die von den *using_namespace_directive*s und *using_alias_directive*s der Namespace Deklaration importiert werden, mehr als einen zugänglichen Typ mit dem Namen @ no__t-2 und `K` @ no__t-4type enthalten. Parameter, dann ist *namespace_or_type_name* mehrdeutig, und es tritt ein Fehler auf.
    * Andernfalls ist *namespace_or_type_name* nicht definiert, und es tritt ein Kompilierzeitfehler auf.
*  Andernfalls hat das *namespace_or_type_name* -Format den Wert `N.I` oder das Formular `N.I<A1, ..., Ak>`. `N` wird zuerst als *namespace_or_type_name*aufgelöst. Wenn die Auflösung von `N` nicht erfolgreich ist, tritt ein Kompilierzeitfehler auf. Andernfalls wird `N.I` oder `N.I<A1, ..., Ak>` wie folgt aufgelöst:
    * Wenn `K` 0 (null) ist und `N` auf einen Namespace verweist und `N` einen schsted Namespace mit dem Namen "`I`" enthält, verweist der *namespace_or_type_name* auf diesen schsted Namespace.
    * Wenn `N` auf einen Namespace verweist und `N` einen zugreif baren Typ mit dem Namen @ no__t-2 und `K` @ no__t-4type-Parameter enthält, verweist *namespace_or_type_name* auf diesen Typ, der mit den angegebenen Typargumenten erstellt wurde.
    * Wenn sich `N` auf eine (möglicherweise konstruierte) Klasse oder einen Strukturtyp bezieht und `N` oder eine der zugehörigen Basisklassen einen für den nsted Typ zugänglichen Typ mit den Parametern Name @ no__t-2 und `K` @ no__t-4type enthält, verweist *namespace_or_type_name* auf der Typ, der mit den angegebenen Typargumenten erstellt wurde. Wenn mehr als ein solcher Typ vorhanden ist, wird der in einem stärker abgeleiteten Typ deklarierte Typ ausgewählt. Beachten Sie Folgendes: Wenn die Bedeutung von `N.I` als Teil der Auflösung der Basisklassen Spezifikation von `N` festgelegt wird, wird die direkte Basisklasse von `N` als Object ([Basisklassen](classes.md#base-classes)) betrachtet.
    * Andernfalls ist `N.I` ein ungültiges *namespace_or_type_name*, und es tritt ein Kompilierzeitfehler auf.

Ein *namespace_or_type_name* -Wert darf nur dann auf eine statische Klasse ([statische Klassen](classes.md#static-classes)) verweisen, wenn

*  *Namespace_or_type_name* ist der `T` in einem *namespace_or_type_name* -Format `T.I` oder
*  *Namespace_or_type_name* ist der `T` in einer *typeof_expression* ([Argument Liste](expressions.md#argument-lists)1) der Form `typeof(T)`.

### <a name="fully-qualified-names"></a>Vollqualifizierte Namen

Jeder Namespace und Typ verfügt über einen ***voll qualifizierten Namen***, der den Namespace oder den Typ unter allen anderen eindeutig identifiziert. Der voll qualifizierte Name eines Namespace oder Typs `N` wird wie folgt bestimmt:

*  Wenn `N` ein Member des globalen Namespace ist, lautet der voll qualifizierte Name `N`.
*  Andernfalls ist der voll qualifizierte Name `S.N`, wobei `S` der voll qualifizierte Name des Namespace oder Typs ist, in dem `N` deklariert ist.

Anders ausgedrückt: der voll qualifizierte Name `N` ist der vollständige hierarchische Pfad der Bezeichner, die zu `N` führen, beginnend beim globalen Namespace. Da jeder Member eines Namespaces oder Typs einen eindeutigen Namen haben muss, folgt der voll qualifizierte Name eines Namespace oder Typs immer eindeutig.

Das folgende Beispiel zeigt mehrere Namespace-und Typdeklarationen zusammen mit den zugehörigen voll qualifizierten Namen.
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

C#verwendet die automatische Speicherverwaltung, mit der Entwickler den von-Objekten belegten Arbeitsspeicher manuell zuordnen und freigeben können. Automatische Speicher Verwaltungsrichtlinien werden von einem ***Garbage Collector***implementiert. Der Lebenszyklus der Speicherverwaltung eines Objekts lautet wie folgt:

1. Wenn das Objekt erstellt wird, wird Arbeitsspeicher zugeordnet, der Konstruktor wird ausgeführt, und das Objekt wird als Live-Objekt betrachtet.
2. Wenn auf das Objekt oder einen Teil davon nicht durch eine mögliche Fortsetzung der Ausführung zugegriffen werden kann, abgesehen von der Ausführung von dedededededektoren, wird das Objekt als nicht mehr verwendet und ist für die Zerstörung infrage. Der C# Compiler und der Garbage Collector können Code analysieren, um zu bestimmen, welche Verweise auf ein Objekt in Zukunft verwendet werden können. Wenn beispielsweise eine lokale Variable, die sich im Gültigkeitsbereich befindet, der einzige vorhandene Verweis auf ein Objekt ist, aber auf diese lokale Variable in keiner möglichen Fortsetzung der Ausführung vom aktuellen Ausführungs Punkt in der Prozedur verwiesen wird, wird der Garbage Collector möglicherweise (aber nicht erforderlich für) behandeln Sie das Objekt als nicht mehr verwendende.
3. Sobald das Objekt für die Zerstörung geeignet ist, wird zu einem späteren Zeitpunkt der Dekonstruktor ([Dekonstruktoren](classes.md#destructors)) für das Objekt ausgeführt. Unter normalen Umständen wird der debugtor für das Objekt nur einmal ausgeführt, obwohl Implementierungs spezifische APIs das Überschreiben dieses Verhaltens zulassen können.
4. Sobald der Dekonstruktor für ein Objekt ausgeführt wird, und der Zugriff auf das Objekt oder einen Teil davon durch eine mögliche Fortsetzung der Ausführung (einschließlich der Ausführung von Dekonstruktoren) nicht möglich ist, wird das Objekt als nicht zugänglich angesehen, und das Objekt wird für die Auflistung qualifiziert.
5. Schließlich gibt der Garbage Collector zu einem späteren Zeitpunkt, nachdem das Objekt für die Auflistung infrage kommt, den diesem Objekt zugeordneten Arbeitsspeicher frei.

Der Garbage Collector verwaltet Informationen zur Objekt Verwendung und verwendet diese Informationen, um Entscheidungen hinsichtlich der Speicherverwaltung zu treffen, z. b. wo im Arbeitsspeicher ein neu erstelltes Objekt zu finden ist, wann ein Objekt verschoben werden soll und wann ein Objekt nicht mehr verwendet wird oder nicht.

Wie andere Sprachen, die voraussetzen, dass eine Garbage Collector C# vorhanden ist, ist so konzipiert, dass die Garbage Collector eine Vielzahl von Speicher Verwaltungsrichtlinien implementieren kann. C# Beispielsweise ist nicht erforderlich, dass Dekonstruktoren ausgeführt werden oder dass Objekte gesammelt werden, sobald Sie qualifiziert sind oder dass Dekonstruktoren in einer bestimmten Reihenfolge oder in einem bestimmten Thread ausgeführt werden.

Das Verhalten des Garbage Collector kann in gewissem Maße über statische Methoden für die Klasse `System.GC` gesteuert werden. Diese Klasse kann verwendet werden, um eine Auflistung anzufordern, Dekonstruktoren auszuführen (oder nicht ausgeführt) usw.

Da die Garbage Collector den Breitengrad der Entscheidung, wann Objekte gesammelt werden sollen, und die Ausführung von Debuggern unterstützt, kann eine konforme Implementierung eine Ausgabe ergeben, die sich von der im folgenden Code gezeigten unterscheidet. Das Programm
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
erstellt eine Instanz der-Klasse `A` und eine Instanz der-Klasse `B`. Diese Objekte sind für Garbage Collection qualifiziert, wenn der Wert `b` der Wert `null` zugewiesen wird, da nach diesem Zeitpunkt kein Benutzer geschriebener Code mehr darauf zugreifen kann. Die Ausgabe kann entweder

```console
Destruct instance of A
Destruct instance of B
```
oder
```console
Destruct instance of B
Destruct instance of A
```
Da in der Sprache keine Einschränkungen für die Reihenfolge auferlegt werden, in der Objekte in die Garbage Collection aufgenommen werden.

In einigen Fällen kann es wichtig sein, den Unterschied zwischen "berechtigte für Zerstörung" und "berechtigte Sammlung" zu unterscheiden. Ein auf ein Objekt angewendeter
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

Wenn im obigen Programm der Garbage Collector den debugtor von `A` vor dem debugtor von `B` ausführen möchte, könnte die Ausgabe dieses Programms wie folgt lauten:
```console
Destruct instance of A
Destruct instance of B
A.F
RefA is not null
```

Beachten Sie, dass zwar die Instanz von `A` nicht verwendet wurde und der Dekonstruktor von `A` ausgeführt wurde, aber es ist weiterhin möglich, dass die Methoden von `A` (in diesem Fall `F`) von einem anderen Dekonstruktor aufgerufen werden. Beachten Sie außerdem, dass das Ausführen eines Dekonstruktors dazu führen kann, dass ein Objekt erneut aus dem Haupt-Programm verwendet werden kann. In diesem Fall hat die Ausführung des Dekonstruktors von `B` bewirkt, dass eine Instanz von `A`, die zuvor nicht verwendet wurde, über den Live Verweis `Test.RefA` zugänglich ist. Nach dem `WaitForPendingFinalizers`-Auflistungs Wert ist die Instanz von `B` für die Auflistung qualifiziert, aber die Instanz von `A` ist aufgrund des Verweises `Test.RefA` nicht verfügbar.

Um Verwirrung und unerwartetes Verhalten zu vermeiden, ist es im Allgemeinen eine gute Idee, dass debugtoren nur die Bereinigung für Daten ausführen, die in den eigenen Feldern Ihres Objekts gespeichert sind, und keine Aktionen für referenzierte Objekte oder statische Felder durchführen.

Eine Alternative zur Verwendung von Dekonstruktoren besteht darin, dass eine Klasse die `System.IDisposable`-Schnittstelle implementiert. Dies ermöglicht es dem Client des-Objekts zu bestimmen, wann die Ressourcen des Objekts freigegeben werden sollen. Dies geschieht in der Regel durch den Zugriff auf das-Objekt als Ressource in einer `using`-Anweisung ([using-Anweisung](statements.md#the-using-statement)).

## <a name="execution-order"></a>Ausführungsreihenfolge

Die Ausführung eines C# Programms wird so fortgesetzt, dass die Nebeneffekte der einzelnen ausführenden Threads an kritischen Ausführungs Punkten beibehalten werden. Ein ***Nebeneffekt*** wird als Lese-oder Schreibvorgang eines flüchtigen Felds, eines Schreibzugriffs auf eine nicht flüchtige Variable, eines Schreibzugriffs auf eine externe Ressource und das Auslösen einer Ausnahme definiert. Die kritischen Ausführungs Punkte, bei denen die Reihenfolge dieser Nebeneffekte beibehalten werden muss, sind Verweise auf flüchtige Felder ([flüchtige Felder](classes.md#volatile-fields)), `lock`-Anweisungen ([lock-Anweisung](statements.md#the-lock-statement)) und Thread Erstellung und-Beendigung. In der Ausführungsumgebung kann die Reihenfolge der Ausführung eines C# Programms geändert werden, wobei die folgenden Einschränkungen gelten:

*  Die Daten Abhängigkeit wird innerhalb eines Ausführungs Threads beibehalten. Das heißt, der Wert jeder Variablen wird berechnet, als ob alle Anweisungen im Thread in der ursprünglichen Programm Reihenfolge ausgeführt wurden.
*  Die Regeln für die Initialisierungs Reihenfolge werden beibehalten ([Feld Initialisierung](classes.md#field-initialization) und [Variableninitialisierer](classes.md#variable-initializers)).
*  Die Reihenfolge von Nebeneffekten wird in Bezug auf flüchtige Lese-und Schreibvorgänge ([flüchtige Felder](classes.md#volatile-fields)) beibehalten. Darüber hinaus muss die Ausführungsumgebung einen Teil eines Ausdrucks nicht auswerten, wenn er ableiten kann, dass der Wert des Ausdrucks nicht verwendet wird und keine erforderlichen Nebeneffekte erzeugt werden (einschließlich der durch Aufrufen einer Methode oder zugreifen auf ein flüchtiges Feld verursachten). Wenn die Programmausführung durch ein asynchrones Ereignis (z. b. eine von einem anderen Thread ausgelöste Ausnahme) unterbrochen wird, ist es nicht sichergestellt, dass die wahrnehmbaren Nebeneffekte in der ursprünglichen Programm Reihenfolge sichtbar sind.
