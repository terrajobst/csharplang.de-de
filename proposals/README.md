---
ms.openlocfilehash: aa0c233e7739ae7a0a7752251aae6f89df18ba93
ms.sourcegitcommit: 94a3d151c438d34ede1d99de9eb4ebdc07ba4699
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/25/2019
ms.locfileid: "79483511"
---
# <a name="c-language-proposals"></a>C#Sprach Vorschläge

Bei sprach Vorschlägen handelt es sich um aktive Dokumente, in denen die aktuelle Betrachtung eines bestimmten Sprach Features beschrieben wird.

Vorschläge können *aktiv*, *inaktiv*, *abgelehnt* oder *abgeschlossen*sein. *Aktive* Angebote werden direkt im Ordner "Vorschläge" gespeichert, *inaktive* und *Abgelehnte* Vorschläge werden in den Unterordnern " [inaktiv](proposals/inactive) " und " [abgelehnt](proposals/rejected) " gespeichert *, und die* abgeschlossenen Angebote werden in einem Ordner archiviert, der der Sprachversion entspricht, zu der Sie gehören.

## <a name="lifetime-of-a-proposal"></a>Lebensdauer eines Angebots

Ein Vorschlag beginnt seine Lebensdauer, wenn das sprach Entwurfs Team entscheidet, dass es eine gute Ergänzung der Sprache an einem Tag machen kann. In der Regel wird es als *aktiv*gestartet, aber wenn wir eine Idee erfassen möchten, ohne gerade daran arbeiten zu wollen, kann ein Vorschlag auch im *inaktiven* Unterordner beginnen. Vorschläge können sogar direkt in den *abgelehnten* Status geraten, wenn wir einen Datensatz erstellen möchten, der nicht beabsichtigt ist. Wenn beispielsweise eine beliebte und wiederkehrende Anforderung nicht implementiert werden kann, können wir dies als abgelehnten Vorschlag erfassen.

Der Vorschlag könnte als Idee in einem [Diskussions Problem](https://github.com/dotnet/csharplang/labels/Discussion)auftreten, oder er kann von Diskussionen in der sprach Entwurfs Besprechung oder von vielen anderen Fronten ausgehen. Das wichtigste ist, dass das Entwurfs Team glaubt, dass es zu tun ist, und dass es jemanden gibt, der bereit ist, ihn zu schreiben. In der Regel weist ein Mitglied des sprach Entwurfs Teams sich selbst als [ein Champion für](https://github.com/dotnet/csharplang/labels/Proposal%20champion)das Feature zu, das nachverfolgt wird. Der Champion ist dafür verantwortlich, den Vorschlag durch den Entwurfsprozess zu verschieben.

Ein Vorschlag ist *aktiv* , wenn er sich über den Entwurf und die Implementierung in Bezug auf eine bevorstehende Version vorwärts bewegt. Wenn die Implementierung vollständig *abgeschlossen*ist, d. h., eine-Implementierung wurde in eine Version zusammengeführt, und die-Funktion wurde angegeben. Sie wird in ein Unterverzeichnis verschoben, das der zugehörigen Version entspricht.

Wenn eine Funktion sich herausstellt, dass Sie überhaupt nicht in die Sprache passt, weil Sie z. b. nicht realisierbar ist, scheint nicht ausreichend Wert hinzuzufügen, oder Sie ist nicht richtig für die Sprache, Sie wird [zurückgewiesen](proposals/rejected). Wenn eine Funktion eine angemessene Zusage hat, aber derzeit nicht priorisiert wird, um mit zu arbeiten, wird Sie möglicherweise als [inaktiv](proposals/inactive) deklariert, um das Gruppieren des Haupt Ordners zu vermeiden. Es ist in Ordnung, dass die Arbeit bei inaktiven oder abgelehnten Vorschlägen erfolgt und dass Sie später wieder auferstanden werden. Die Kategorien sind vorhanden, um die aktuelle Entwurfs Absicht widerzuspiegeln.

## <a name="nature-of-a-proposal"></a>Art eines Angebots

Ein Vorschlag sollte der [Angebots Vorlage](proposal-template.md)folgen. Ein guter Vorschlag sollte Folgendes sein:

- Passen Sie den allgemeinen Geist und die Ästhetik der Sprache an.
- Führen Sie keine fein alternative Syntax für vorhandene Funktionen ein.
- Fügen Sie für eine klare Gruppe von Benutzern einen großen Wert hinzu.
- Fügen Sie der Komplexität der Sprache nicht wesentlich hinzu, insbesondere für neue Benutzer.  

## <a name="discussion-of-proposals"></a>Erörterung von Vorschlägen

Feedback und Diskussionen treten in [Diskussionsthemen](https://github.com/dotnet/csharplang/labels/Discussion)auf. Wenn dem Ordner "Vorschläge" ein neuer Vorschlag hinzugefügt wird, sollte er in einem Diskussions Problem des Erstellers oder des Vorschlags Erstellers angekündigt werden. 

 