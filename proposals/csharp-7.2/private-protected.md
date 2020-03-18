---
ms.openlocfilehash: 6cf489595654236c18edee94c0af380e605c9571
ms.sourcegitcommit: f61a06970fa0562d2e40363fae3948eb168624ca
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/14/2020
ms.locfileid: "79484075"
---
# <a name="private-protected"></a>private protected

* [x] vorgeschlagen
* [x] Prototyp: [Fertig](https://github.com/dotnet/roslyn/blob/master/docs/features/private-protected.md) stellen
* [x] Implementierung: [Complete](https://github.com/dotnet/roslyn/blob/master/docs/features/private-protected.md)
* [x] Spezifikation: [Fertig](#detailed-design) stellen

## <a name="summary"></a>Zusammenfassung
[summary]: #summary

Machen Sie die CLR-`protectedAndInternal` Barrierefreiheits Stufe in C# als `private protected`verfügbar.

## <a name="motivation"></a>Motivation
[motivation]: #motivation

Es gibt viele Situationen, in denen eine API Member enthält, die nur für die Implementierung und Verwendung durch Unterklassen in der Assembly, die den Typ bereitstellt, verwendet werden sollen. Obwohl die CLR für diesen Zweck eine Barrierefreiheits Ebene bereitstellt, ist Sie in C#nicht verfügbar. Folglich müssen API-Besitzer entweder `internal` Schutz und Selbstdisziplin oder eine benutzerdefinierte Analyse verwenden oder `protected` mit zusätzlichen Dokumentationen verwenden, die erläutern, dass der Member, der in der öffentlichen Dokumentation für den Typ erscheint, nicht Teil der öffentlichen API sein sollte.  Beispiele der letzteren finden Sie unter Member von Roslyn-`CSharpCompilationOptions`, deren Namen mit `Common`beginnen.

Durch die direkte Unterstützung dieser Zugriffsebene C# in können diese Umstände natürlich in der Sprache ausgedrückt werden.

## <a name="detailed-design"></a>Detaillierter Entwurf
[design]: #detailed-design

### <a name="private-protected-access-modifier"></a>Modifizierer `private protected` Zugriffs

Es wird vorgeschlagen, eine neue zugriffsmodifiziererkombination hinzuzufügen `private protected` (die in beliebiger Reihenfolge unter den Modifizierern vorkommen kann). Dies wird dem CLR-Konzept von protectedandinternal zugeordnet und verwendet dieselbe Syntax, die zurzeit in [ C++/CLI](https://docs.microsoft.com/cpp/dotnet/how-to-define-and-consume-classes-and-structs-cpp-cli#BKMK_Member_visibility)verwendet wird.

Auf einen Member, der `private protected` deklariert ist, kann innerhalb einer Unterklasse seines Containers zugegriffen werden, wenn sich diese Unterklasse in derselben Assembly befindet wie der Member.

Die Sprachspezifikation wird wie folgt geändert (Additions Fett). Abschnitts Nummern werden unten nicht angezeigt, da Sie je nach Version der Spezifikation, in die Sie integriert ist, variieren können.

-----

> Die deklarierte Barrierefreiheit eines Members kann eine der folgenden sein:
- Public, das durch Einschließen eines öffentlichen Modifizierers in die Element Deklaration ausgewählt wird. Die intuitive Bedeutung von Public ist "Access not Limited".
- Geschützt, das durch Einschließen eines geschützten Modifizierers in die Element Deklaration ausgewählt wird. Die intuitive Bedeutung von Protected ist "der Zugriff ist auf die enthaltende Klasse oder auf Typen beschränkt, die von der enthaltenden Klasse abgeleitet sind".
- Intern, das durch Einschließen eines internen Modifizierers in die Element Deklaration ausgewählt wird. Die intuitive Bedeutung von Internal ist "Zugriff beschränkt auf diese Assembly".
- Geschützter interner, der durch Einschließen eines geschützten und eines internen Modifizierers in die Element Deklaration ausgewählt wird. Die intuitive Bedeutung geschützter interner ist "Zugriff innerhalb dieser Assembly und von Typen, die von der enthaltenden Klasse abgeleitet sind".
- **Privat geschützt, das durch Einschließen eines privaten und eines geschützten Modifizierers in die Element Deklaration ausgewählt wird. Die intuitive Bedeutung von private Protected ist "Zugriff innerhalb dieser Assembly nach Typen, die von der enthaltenden Klasse abgeleitet sind".**

-----

> Abhängig vom Kontext, in dem eine Element Deklaration stattfindet, sind nur bestimmte Typen von deklarierter Barrierefreiheit zulässig. Wenn eine Member-Deklaration keine Zugriffsmodifizierer enthält, bestimmt der Kontext, in dem die Deklaration stattfindet, die standardmäßige deklarierte Barrierefreiheit. 
- Namespaces verfügen implizit über eine Public deklarierte Barrierefreiheit. Für Namespace Deklarationen sind keine Zugriffsmodifizierer zulässig.
- Typen, die direkt in Kompilierungs Einheiten oder Namespaces deklariert werden (im Gegensatz zu anderen Typen), können über eine öffentliche oder intern deklarierte Barrierefreiheit verfügen, und Sie erhalten standardmäßig intern deklarierten Zugriff.
- Klassenmember können eine der fünf Arten von deklarierter Barrierefreiheit aufweisen und werden standardmäßig als private deklarierte Barrierefreiheit angezeigt. [Hinweis: ein Typ, der als Member einer Klasse deklariert ist, kann eine der fünf Arten der deklarierten Barrierefreiheit aufweisen, wohingegen ein als Member eines Namespaces deklarierter Typ nur über eine öffentliche oder intern deklarierte Barrierefreiheit verfügen kann. Hinweis Ende]
- Strukturmember können als "Public", "Internal" oder "private" deklariert werden und sind standardmäßig für die private deklarierte Barrierefreiheit, da Strukturen implizit versiegelt sind. Strukturmember, die in einer Struktur ~~eingeführt werden (~~ d. h. nicht von dieser Struktur geerbt), können keine geschützte *,* geschützte interne**oder private geschützte** Barrierefreiheit haben. [Hinweis: ein Typ, der als Member einer Struktur deklariert wurde, kann als "Public", "Internal" oder "private" deklariert werden, wohingegen ein Typ, der als Member eines Namespaces deklariert ist, nur über eine öffentliche oder intern deklarierte Barrierefreiheit verfügen kann. Hinweis Ende]
- Schnittstellenmember haben implizit öffentlich deklarierte Zugriffsmöglichkeiten. Zugriffsmodifizierer sind für Schnittstellenmember-Deklarationen unzulässig
- Enumerationsmember haben implizit öffentlich deklarierte Zugriffsmöglichkeiten. Es sind keine Zugriffsmodifizierer für Enumerationsmember zulässig.

-----

> Die Zugriffs Domäne eines geschachtelten Members M, der in einem Typ T innerhalb eines Programms P deklariert ist, wird wie folgt definiert (Beachten Sie, dass M selbst möglicherweise ein Typ sein kann):
- Wenn die deklarierte Barrierefreiheit von m öffentlich ist, ist die Zugriffs Domäne m die Zugriffs Domäne T.
- Wenn die deklarierte Zugriffsart von M als intern geschützt ist, lassen Sie D den Programmtext von P und den Programmtext jedes Typs, der von T abgeleitet ist, als außerhalb von P deklariert. Die Zugriffs Domäne M ist die Schnittmenge der Zugriffs Domäne T mit D.
- **Wenn die deklarierte Zugriffsart von M Privat geschützt ist, lassen Sie die Schnittmenge des Programm Texts von P und des Programm Texts eines beliebigen Typs, der von T abgeleitet wird, angegeben werden. Die Zugriffs Domäne M ist die Schnittmenge der Zugriffs Domäne T mit D.**
- Wenn die deklarierte Zugriffsart von M geschützt ist, lassen Sie D den Programmtext von t und den Programmtext eines beliebigen Typs, der von t abgeleitet ist, darstellen. Die Zugriffs Domäne M ist die Schnittmenge der Zugriffs Domäne T mit D.
- Wenn die deklarierte Barrierefreiheit von m intern ist, entspricht die Zugriffs Domäne m der Schnittmenge der Zugriffs Domäne T mit dem Programmtext P.
- Wenn die deklarierte Barrierefreiheit von m privat ist, ist die Zugriffs Domäne m der Programmtext von T.

-----

> Wenn außerhalb des Programm Texts der Klasse, in der Sie deklariert ist, auf einen geschützten **oder privaten** Member der geschützten Instanz zugegriffen wird, und wenn außerhalb des Programm Texts des Programms, in dem es deklariert ist, auf einen geschützten internen Instanzmember zugegriffen wird, erfolgt der Zugriff innerhalb einer Klassen Deklaration, die von der Klasse abgeleitet ist, in der Sie deklariert ist. Darüber hinaus muss der Zugriff durch eine Instanz dieses abgeleiteten Klassen Typs oder eines aus der Klasse erstellten Klassen Typs erfolgen. Diese Einschränkung verhindert, dass eine abgeleitete Klasse auf geschützte Member anderer abgeleiteter Klassen zugreift, auch wenn die Elemente von derselben Basisklasse geerbt werden.

-----

> Die zulässigen Zugriffsmodifizierer und der Standard Zugriff für eine Typdeklaration hängen vom Kontext ab, in dem die Deklaration stattfindet (9.5.2):
- In Kompilierungs Einheiten oder Namespaces deklarierte Typen können öffentlichen oder internen Zugriff haben. Der Standardwert ist "interner Zugriff".
- In Klassen deklarierte Typen können über einen öffentlichen, geschützten internen, **privaten**, geschützten, geschützten, internen oder privaten Zugriff verfügen. Der Standardwert ist privater Zugriff.
- In Strukturen deklarierte Typen können über öffentlichen, internen oder privaten Zugriff verfügen. Der Standardwert ist privater Zugriff.

-----

> Eine statische Klassen Deklaration unterliegt den folgenden Einschränkungen:
- Eine statische Klasse darf keinen versiegelten oder abstrakten Modifizierer enthalten. (Da eine statische Klasse jedoch nicht instanziiert oder von abgeleitet werden kann, verhält sie sich so, als ob Sie versiegelt und abstrakt wäre.)
- Eine statische Klasse darf keine Klassenbasis Spezifikation ("16.2.5") enthalten und kann weder eine Basisklasse noch eine Liste implementierter Schnittstellen explizit angeben. Eine statische Klasse erbt implizit vom Type-Objekt.
- Eine statische Klasse darf nur statische Member enthalten ("16.4.8"). [Hinweis: alle Konstanten und Typen von Typen werden als statische Member klassifiziert. Hinweis Ende]
- Eine statische Klasse darf keine Member mit geschützter **, privater** oder geschützter interner Barrierefreiheit haben.

> Es handelt sich um einen Kompilierzeitfehler, der gegen diese Einschränkungen verstößt. 

-----

> Eine Klassenmember-Deklaration kann eine der ~~fünf~~**möglichen Arten** von deklarierten Barrierefreiheit aufweisen ("9.5.2"): Public, **private Protected**, protected internal, protected, internal oder private. Mit Ausnahme der geschützten internen **und privaten geschützten** Kombi**Nationen**ist es ein Kompilierzeitfehler, wenn mehr als ein Zugriffsmodifizierer angegeben wird. Wenn eine Klassenmember-Deklaration keine Zugriffsmodifizierer enthält, wird als private angenommen.

-----

> Nicht-in-Typen können über öffentliche oder intern deklarierte Barrierefreiheit verfügen und standardmäßig über intern deklarierte Zugriffsmöglichkeiten verfügen. In Form von untergeordneten Typen können auch diese Formen der deklarierten Barrierefreiheit sowie eine oder mehrere zusätzliche Formen der deklarierten Barrierefreiheit enthalten sein, je nachdem, ob der enthaltende Typ eine Klasse oder Struktur ist:
- Ein in einer Klasse deklarierter, in einer Klasse deklarierter Typ kann eine beliebige von ~~fünf~~**Formen** deklarierter Barrierefreiheit aufweisen (Public, **private Protected**, protected internal, protected, internal oder private), und, wie bei anderen Klassenmembern, standardmäßig auf private deklarierte Barrierefreiheit festgelegt.
- Ein in einer Struktur deklarierter, in einer Struktur deklarierter Typ kann eine beliebige von drei Formen der deklarierten Barrierefreiheit (Public, internal oder private) aufweisen und, wie andere Strukturmember, standardmäßig auf private deklarierte Barrierefreiheit festgelegt werden.

-----

> Die Methode, die durch eine Überschreibungs Deklaration überschrieben wird, wird als die überschriebene Basis Methode für eine in einer Klasse C deklarierte Überschreibungs Methode M bezeichnet. die überschriebene Basis Methode wird bestimmt, indem jeder Basis Klassentyp von c untersucht wird, beginnend mit dem direkten Basis Klassentyp c und Wenn Sie mit jedem aufeinander folgenden direkten Basis Klassentyp fortfahren, bis in einem gegebenen Basis Klassentyp, wird mindestens eine barrierefreie Methode gefunden, die nach der Ersetzung von Typargumenten dieselbe Signatur wie M hat. Zum Ermitteln der überschriebenen Basis Methode gilt eine Methode als verfügbar, wenn Sie öffentlich ist, wenn Sie geschützt ist, wenn Sie geschützt ist, oder wenn Sie intern **oder privat geschützt** **ist und** im selben Programm wie C deklariert ist.

-----

> Die Verwendung der Accessormodifizierer unterliegt den folgenden Einschränkungen:
- Ein Accessor-Modifizierer darf nicht in einer Schnittstelle oder in einer expliziten Schnittstellenmember-Implementierung verwendet werden.
- Für eine Eigenschaft oder einen Indexer, der über keinen Überschreibungsmodifizierer verfügt, ist ein Accessor-Modifier nur zulässig, wenn die Eigenschaft oder der Indexer sowohl einen get-als auch einen Set-Accessor aufweist und dann nur für einen dieser Accessoren zulässig ist.
- Für eine Eigenschaft oder einen Indexer, der einen Überschreibungsmodifizierer enthält, muss ein Accessor mit dem Accessor-Modifier (sofern vorhanden) des Accessors identisch sein, der überschrieben wird.
- Der Accessor-Modifizierer muss eine Barrierefreiheit deklarieren, die strikt restriktiver ist als der deklarierte Zugriff auf die Eigenschaft oder den Indexer selbst. Genauer gesagt:
  - Wenn die Eigenschaft oder der Indexer einen deklarierten Zugriff auf Public hat, kann der Accessor-Modifizierer entweder **Privat geschützt**, geschützt, geschützt, intern, geschützt oder privat sein.
  - Wenn die Eigenschaft oder der Indexer über eine deklarierte Zugriffsmethode für protected internal verfügt, kann der Accessor-Modifizierer entweder **Privat geschützt**, intern, geschützt oder privat sein.
  - Wenn die Eigenschaft oder der Indexer eine deklarierte Zugriffsmethode (intern oder geschützt) aufweist, muss der Accessor-Modifier **entweder privat geschützt oder** privat sein.
  - **Wenn die Eigenschaft oder der Indexer über eine deklarierte Zugriffsmethode für private geschützt verfügt, muss der Accessor-Modifier privat sein.**
  - Wenn die Eigenschaft oder der Indexer einen deklarierten Zugriff auf private hat, kann kein Accessor-Modifizierer verwendet werden.

-----

> Da die Vererbung für Strukturen nicht unterstützt wird, kann die deklarierte Barrierefreiheit eines Strukturmembers nicht geschützt, **Privat geschützt**oder geschützt werden.

-----

## <a name="drawbacks"></a>Nachteile
[drawbacks]: #drawbacks

Wie bei allen Sprach Features müssen wir Fragen, ob die zusätzliche Komplexität der Sprache in der zusätzlichen Klarheit für den Text der C# Programme, die von der Funktion profitieren würden, zurückgegeben wird.

## <a name="alternatives"></a>Alternativen
[alternatives]: #alternatives

Eine Alternative wäre die Bereitstellung einer API, die ein Attribut und einen Analyzer kombiniert. Das-Attribut wird vom Programmierer für ein `internal`-Element platziert, um zu zeigen, dass der Member nur in Unterklassen verwendet werden soll, und der Analyzer prüft, ob diese Einschränkungen befolgt werden. 

## <a name="unresolved-questions"></a>Nicht aufgelöste Fragen
[unresolved]: #unresolved-questions

Die Implementierung ist größtenteils fertiggestellt. Das einzige geöffnete Arbeits Element ist das Entwerfen einer entsprechenden Spezifikation für VB.

## <a name="design-meetings"></a>Treffen von Besprechungen

TBD
