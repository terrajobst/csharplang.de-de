---
ms.openlocfilehash: ac4c8760e3b6a0934f01ae634f666af60aa1c0fe
ms.sourcegitcommit: 94a3d151c438d34ede1d99de9eb4ebdc07ba4699
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/25/2019
ms.locfileid: "79483553"
---
# <a name="compiler-intrinsics"></a><span data-ttu-id="82236-101">Intrinsische Compilerfunktionen</span><span class="sxs-lookup"><span data-stu-id="82236-101">Compiler Intrinsics</span></span>

## <a name="summary"></a><span data-ttu-id="82236-102">Zusammenfassung</span><span class="sxs-lookup"><span data-stu-id="82236-102">Summary</span></span>

<span data-ttu-id="82236-103">Dieser Vorschlag bietet Sprachkonstrukte, die IL-Opcodes auf niedriger Ebene verfügbar machen, auf die zurzeit nicht effizient zugegriffen werden kann, oder überhaupt: `ldftn`, `ldvirtftn``ldtoken` und `calli`.</span><span class="sxs-lookup"><span data-stu-id="82236-103">This proposal provides language constructs that expose low level IL opcodes that cannot currently be accessed efficiently, or at all: `ldftn`, `ldvirtftn`, `ldtoken` and `calli`.</span></span> <span data-ttu-id="82236-104">Diese Opcodes auf niedriger Ebene können bei Hochleistungs Code wichtig sein, und Entwickler benötigen eine effiziente Zugriffsmethode.</span><span class="sxs-lookup"><span data-stu-id="82236-104">These low level opcodes can be important in high performance code and developers need an efficient way to access them.</span></span>

## <a name="motivation"></a><span data-ttu-id="82236-105">Motivation</span><span class="sxs-lookup"><span data-stu-id="82236-105">Motivation</span></span>

<span data-ttu-id="82236-106">Die Gründe und der Hintergrund für dieses Feature werden im folgenden Problem beschrieben (wie eine mögliche Implementierung des Features):</span><span class="sxs-lookup"><span data-stu-id="82236-106">The motivations and background for this feature are described in the following issue (as is a potential implementation of the feature):</span></span> 

https://github.com/dotnet/csharplang/issues/191

<span data-ttu-id="82236-107">Dieser Alternative Entwurfsvorschlag wird nach der Überprüfung der Prototypimplementierung des ursprünglichen Angebots durch @msjabby und der Verwendung in einer bedeutenden Codebasis angezeigt.</span><span class="sxs-lookup"><span data-stu-id="82236-107">This alternate design proposal comes after reviewing a prototype implementation of the original proposal by @msjabby as well as the use throughout a significant code base.</span></span> <span data-ttu-id="82236-108">Dieser Entwurf wurde mit erheblichen Eingaben aus @mjsabby, @tmat und @jkotasdurchgeführt.</span><span class="sxs-lookup"><span data-stu-id="82236-108">This design was done with significant input from @mjsabby, @tmat and @jkotas.</span></span>

## <a name="detailed-design"></a><span data-ttu-id="82236-109">Detaillierter Entwurf</span><span class="sxs-lookup"><span data-stu-id="82236-109">Detailed Design</span></span> 

### <a name="allow-address-of-to-target-methods"></a><span data-ttu-id="82236-110">Address-Methode für Ziel Methoden zulassen</span><span class="sxs-lookup"><span data-stu-id="82236-110">Allow address of to target methods</span></span>

<span data-ttu-id="82236-111">Methoden Gruppen werden nun als Argumente für einen Address-of-Ausdruck zugelassen.</span><span class="sxs-lookup"><span data-stu-id="82236-111">Method groups will now be allowed as arguments to an address-of expression.</span></span> <span data-ttu-id="82236-112">Der Typ eines solchen Ausdrucks wird `void*`.</span><span class="sxs-lookup"><span data-stu-id="82236-112">The type of such an expression will be `void*`.</span></span> 

``` csharp
class Util { 
    public static void Log() { } 
}

// ldftn Util.Log
void* ptr = &Util.Log; 
```

<span data-ttu-id="82236-113">Da hier keine Delegatkonvertierung vorhanden ist, ist der einzige Mechanismus zum Filtern von Elementen in der Methoden Gruppe der statische/instanzzugriff.</span><span class="sxs-lookup"><span data-stu-id="82236-113">Given there is no delegate conversion here the only mechanism for filtering members in the method group is by static / instance access.</span></span> <span data-ttu-id="82236-114">Wenn die Elemente nicht unterschieden werden können, tritt ein Kompilierzeitfehler auf.</span><span class="sxs-lookup"><span data-stu-id="82236-114">If that cannot distinguish the members then a compile time error will occur.</span></span>

``` csharp
class Util { 
    public void Log() { } 
    public void Log(string p1) { } 
    public static void Log(int i) { };
}

unsafe {
    // Error: Method group Log has more than one applicable candidate.
    void* ptr1 = &Log; 

    // Okay: only one static member to consider here.
    void* ptr2 = &Util.Log;
}
```

<span data-ttu-id="82236-115">Der AddressOf-Ausdruck in diesem Kontext wird folgendermaßen implementiert:</span><span class="sxs-lookup"><span data-stu-id="82236-115">The addressof expression in this context will be implemented in the following manner:</span></span>

- <span data-ttu-id="82236-116">ldftn:, wenn die Methode nicht virtuell ist.</span><span class="sxs-lookup"><span data-stu-id="82236-116">ldftn: when the method is non-virtual.</span></span>
- <span data-ttu-id="82236-117">ldvirtftn: Wenn die Methode virtuell ist.</span><span class="sxs-lookup"><span data-stu-id="82236-117">ldvirtftn: when the method is virtual.</span></span>

<span data-ttu-id="82236-118">Einschränkungen dieses Features:</span><span class="sxs-lookup"><span data-stu-id="82236-118">Restrictions of this feature:</span></span>

- <span data-ttu-id="82236-119">Instanzmethoden können nur angegeben werden, wenn ein Aufruf Ausdruck für einen Wert verwendet wird.</span><span class="sxs-lookup"><span data-stu-id="82236-119">Instance methods can only be specified when using an invocation expression on a value</span></span>
- <span data-ttu-id="82236-120">Lokale Funktionen können nicht in `&`verwendet werden.</span><span class="sxs-lookup"><span data-stu-id="82236-120">Local functions cannot be used in `&`.</span></span> <span data-ttu-id="82236-121">Die Implementierungsdetails dieser Methoden werden absichtlich nicht in der Sprache angegeben.</span><span class="sxs-lookup"><span data-stu-id="82236-121">The implementation details of these methods are deliberately not specified by the language.</span></span> <span data-ttu-id="82236-122">Dies umfasst auch, ob es sich um statische oder Instanz handelt oder genau, mit welcher Signatur Sie ausgegeben werden.</span><span class="sxs-lookup"><span data-stu-id="82236-122">This includes whether they are static vs. instance or exactly what signature they are emitted with.</span></span>

### <a name="handleof"></a><span data-ttu-id="82236-123">Lenker</span><span class="sxs-lookup"><span data-stu-id="82236-123">handleof</span></span>

<span data-ttu-id="82236-124">Das kontextabhängige Schlüsselwort `handleof` übersetzt ein Feld, einen Member oder einen Typ mithilfe der `ldtoken`-Anweisung in den entsprechenden `RuntimeHandle` Typ.</span><span class="sxs-lookup"><span data-stu-id="82236-124">The `handleof` contextual keyword will translate a field, member or type into their equivalent `RuntimeHandle` type using the `ldtoken` instruction.</span></span> <span data-ttu-id="82236-125">Der genaue Typ des Ausdrucks hängt von der Art des Namens in `handleof`ab:</span><span class="sxs-lookup"><span data-stu-id="82236-125">The exact type of the expression will depend on the kind of the name in `handleof`:</span></span>

- <span data-ttu-id="82236-126">Feld: `RuntimeFieldHandle`</span><span class="sxs-lookup"><span data-stu-id="82236-126">field: `RuntimeFieldHandle`</span></span>
- <span data-ttu-id="82236-127">Typ: `RuntimeTypeHandle`</span><span class="sxs-lookup"><span data-stu-id="82236-127">type: `RuntimeTypeHandle`</span></span>
- <span data-ttu-id="82236-128">Methode: `RuntimeMethodHandle`</span><span class="sxs-lookup"><span data-stu-id="82236-128">method: `RuntimeMethodHandle`</span></span>

<span data-ttu-id="82236-129">Die Argumente für die `handleof` sind identisch mit `nameof`.</span><span class="sxs-lookup"><span data-stu-id="82236-129">The arguments to `handleof` are identical to `nameof`.</span></span> <span data-ttu-id="82236-130">Dabei muss es sich um einen einfachen Namen, einen qualifizierten Namen, einen Element Zugriff, den Basis Zugriff mit einem angegebenen Member oder um diesen Zugriff mit einem angegebenen Element handeln.</span><span class="sxs-lookup"><span data-stu-id="82236-130">It must be a simple name, qualified name, member access, base access with a specified member, or this access with a specified member.</span></span> <span data-ttu-id="82236-131">Der Argumentausdruck identifiziert eine Codedefinition, wird jedoch niemals ausgewertet.</span><span class="sxs-lookup"><span data-stu-id="82236-131">The argument expression identifies a code definition, but it is never evaluated.</span></span>

<span data-ttu-id="82236-132">Der `handleof` Ausdruck wird zur Laufzeit ausgewertet und hat den Rückgabetyp `RuntimeHandle`.</span><span class="sxs-lookup"><span data-stu-id="82236-132">The `handleof` expression is evaluated at runtime and has a return type of `RuntimeHandle`.</span></span> <span data-ttu-id="82236-133">Dies kann in sicherem Code und unsicher ausgeführt werden.</span><span class="sxs-lookup"><span data-stu-id="82236-133">This can be executed in safe code as well as unsafe.</span></span> 

``` 
RuntimeHandle stringHandle = handleof(string);
```

<span data-ttu-id="82236-134">Einschränkungen dieses Features:</span><span class="sxs-lookup"><span data-stu-id="82236-134">Restrictions of this feature:</span></span>

- <span data-ttu-id="82236-135">Eigenschaften können nicht in einem `handleof` Ausdruck verwendet werden.</span><span class="sxs-lookup"><span data-stu-id="82236-135">Properties cannot be used in a `handleof` expression.</span></span>
- <span data-ttu-id="82236-136">Der `handleof` Ausdruck kann nicht verwendet werden, wenn ein vorhandener `handleof` Name im Gültigkeitsbereich vorhanden ist.</span><span class="sxs-lookup"><span data-stu-id="82236-136">The `handleof` expression cannot be used when there is an existing `handleof` name in scope.</span></span> <span data-ttu-id="82236-137">Z. b. Typ, Namespace usw...</span><span class="sxs-lookup"><span data-stu-id="82236-137">For example a type, namespace, etc ...</span></span>

### <a name="calli"></a><span data-ttu-id="82236-138">Calli</span><span class="sxs-lookup"><span data-stu-id="82236-138">calli</span></span>

<span data-ttu-id="82236-139">Der Compiler fügt Unterstützung für einen neuen Typ von `extern` Funktion hinzu, der effizient in eine `.calli` Anweisung übersetzt wird.</span><span class="sxs-lookup"><span data-stu-id="82236-139">The compiler will add support for a new type of `extern` function that efficiently translates into a `.calli` instruction.</span></span> <span data-ttu-id="82236-140">Das extern-Attribut wird mit einem Attribut der folgenden Form gekennzeichnet:</span><span class="sxs-lookup"><span data-stu-id="82236-140">The extern attribute will be marked with an attribute of the following shape:</span></span>

``` csharp
[AttributeUsage(AttributeTargets.Method)]
public sealed class CallIndirectAttribute : Attribute
{
    public CallingConvention CallingConvention { get; }
    public CallIndirectAttribute(CallingConvention callingConvention)
    {
        CallingConvention = callingConvention;
    }
}
```

<span data-ttu-id="82236-141">Dies ermöglicht Entwicklern das Definieren von Methoden in der folgenden Form:</span><span class="sxs-lookup"><span data-stu-id="82236-141">This allows developers to define methods in the following form:</span></span>

``` csharp
[CallIndirect(CallingConvention.Cdecl)]
static extern int MapValue(string s, void *ptr);

unsafe {
    var i = MapValue("42", &int.Parse);
    Console.WriteLine(i);
}
```

<span data-ttu-id="82236-142">Einschränkungen für die Methode, auf die das `CallIndirect`-Attribut angewendet wurde:</span><span class="sxs-lookup"><span data-stu-id="82236-142">Restrictions on the method which has the `CallIndirect` attribute applied:</span></span>

- <span data-ttu-id="82236-143">Ein `DllImport`-Attribut ist nicht möglich.</span><span class="sxs-lookup"><span data-stu-id="82236-143">Cannot have a `DllImport` attribute.</span></span>
- <span data-ttu-id="82236-144">Kann nicht generisch sein.</span><span class="sxs-lookup"><span data-stu-id="82236-144">Cannot be generic.</span></span>

## <a name="open-issues"></a><span data-ttu-id="82236-145">Offene Probleme</span><span class="sxs-lookup"><span data-stu-id="82236-145">Open Issues</span></span>

### <a name="callingconvention"></a><span data-ttu-id="82236-146">CallingConvention</span><span class="sxs-lookup"><span data-stu-id="82236-146">CallingConvention</span></span>

<span data-ttu-id="82236-147">Die `CallIndirectAttribute` wie entworfen verwendet die `CallingConvention` Enumeration, die einen Eintrag für verwaltete Aufruf Konventionen fehlt.</span><span class="sxs-lookup"><span data-stu-id="82236-147">The `CallIndirectAttribute` as designed uses the `CallingConvention` enum which lacks an entry for managed calling conventions.</span></span> <span data-ttu-id="82236-148">Die Aufzählung muss entweder so erweitert werden, dass diese Aufruf Konvention enthalten ist, oder das Attribut muss einen anderen Ansatz annehmen.</span><span class="sxs-lookup"><span data-stu-id="82236-148">The enum either needs to be extended to include this calling convention or the attribute needs to take a different approach.</span></span>

## <a name="considerations"></a><span data-ttu-id="82236-149">Überlegungen</span><span class="sxs-lookup"><span data-stu-id="82236-149">Considerations</span></span>

### <a name="disambiguating-method-groups"></a><span data-ttu-id="82236-150">Disambiguating-Methoden Gruppen</span><span class="sxs-lookup"><span data-stu-id="82236-150">Disambiguating method groups</span></span>

<span data-ttu-id="82236-151">Es gab einige Erörterung von Features, die es einfacher machen, Methoden Gruppen zu unterscheiden, die an einen Address-of-Ausdruck übergeben wurden.</span><span class="sxs-lookup"><span data-stu-id="82236-151">There was some discussion around features that would make it easier to disambiguate method groups passed to an address-of expression.</span></span> <span data-ttu-id="82236-152">Beispielsweise können der Syntax möglicherweise Signatur Elemente hinzugefügt werden:</span><span class="sxs-lookup"><span data-stu-id="82236-152">For instance potentially adding signature elements to the syntax:</span></span>

``` csharp
class Util {
    public static void Log() { ... }
    public static void Log(string) { ... }
}

unsafe {
    // Error: ambiguous Log
    void *ptr1 = &Util.Log;

    // Use Util.Log();
    void *ptr2 = &Util.Log();
}
```

<span data-ttu-id="82236-153">Dies wurde zurückgewiesen, weil ein Großbuchstabe nicht durchgeführt werden konnte und hier keine einfache Syntax gefunden werden konnte.</span><span class="sxs-lookup"><span data-stu-id="82236-153">This was rejected because a compelling case could not be made nor could a simple syntax be envisioned here.</span></span> <span data-ttu-id="82236-154">Außerdem gibt es eine recht geradlinige Aufgabe: einfach definieren Sie eine andere Methode, die eindeutig ist C# , und verwenden Sie Code, um die gewünschte Funktion aufzurufen.</span><span class="sxs-lookup"><span data-stu-id="82236-154">Also there is a fairly straight forward work around: simple define another method that is unambiguous and uses C# code to call into the desired function.</span></span> 

``` csharp
class Workaround {
    public static void LocalLog() => Util.Log();
}
unsafe { 
    void* ptr = &Workaround.LocalLog;
}
```

<span data-ttu-id="82236-155">Dies wird sogar noch einfacher, wenn `static` lokalen Funktionen die Sprache eingeben.</span><span class="sxs-lookup"><span data-stu-id="82236-155">This becomes even simpler if `static` local functions enter the language.</span></span> <span data-ttu-id="82236-156">Anschließend könnte die Problem Umgehung in der gleichen Funktion definiert werden, die den mehrdeutigen address-of-Vorgang verwendet hat:</span><span class="sxs-lookup"><span data-stu-id="82236-156">Then the work around could be defined in the same function that used the ambiguous address-of operation:</span></span>

``` csharp
unsafe { 
    static void LocalLog() => Util.Log();
    void* ptr = &Workaround.LocalLog;
}
```

### <a name="loadtypetokenint32"></a><span data-ttu-id="82236-157">LoadTypeTokenInt32</span><span class="sxs-lookup"><span data-stu-id="82236-157">LoadTypeTokenInt32</span></span>

<span data-ttu-id="82236-158">Der ursprüngliche Vorschlag, der zum Laden von Metadatentoken als `int` Werte zur Kompilierzeit zulässig ist.</span><span class="sxs-lookup"><span data-stu-id="82236-158">The original proposal allowed for metadata tokens to be loaded as `int` values at compile time.</span></span> <span data-ttu-id="82236-159">Verfügen im Wesentlichen über `tokenof`, die die gleichen Argumente wie `handleof` haben, aber zur Kompilierzeit auf eine `int` Konstante ausgewertet wird.</span><span class="sxs-lookup"><span data-stu-id="82236-159">Essentially have `tokenof` that has the same arguments as `handleof` but is evaluated at compile time to an `int` constant.</span></span> 

<span data-ttu-id="82236-160">Dies wurde abgelehnt, da dadurch ein erhebliches Problem bei Il-neuschreib Vorgängen verursacht wird (von denen .net viele umfasst).</span><span class="sxs-lookup"><span data-stu-id="82236-160">This was rejected as it causes significant problem for IL rewrites (of which .NET has many).</span></span> <span data-ttu-id="82236-161">Solche ReWriter manipulieren die Metadatentabellen oft so, dass diese Werte ungültig werden könnten.</span><span class="sxs-lookup"><span data-stu-id="82236-161">Such rewriters often manipulate the metadata tables in a way that could invalidate these values.</span></span> <span data-ttu-id="82236-162">Es gibt keine sinnvolle Möglichkeit für solche ReWriter, diese Werte zu aktualisieren, wenn Sie als einfache `int` Werte gespeichert werden.</span><span class="sxs-lookup"><span data-stu-id="82236-162">There is no reasonable way for such rewriters to update these values when they are stored as simple `int` values.</span></span>

<span data-ttu-id="82236-163">Die zugrundeliegende Idee, ein undurchsichtiges Handle für Metadateneinträge zu haben, wird weiterhin vom Lauf Zeit Team untersucht.</span><span class="sxs-lookup"><span data-stu-id="82236-163">The underlying idea of having an opaque handle for metadata entries will continue to be explored by the runtime team.</span></span> 

## <a name="future-considerations"></a><span data-ttu-id="82236-164">Überlegungen für die Zukunft</span><span class="sxs-lookup"><span data-stu-id="82236-164">Future Considerations</span></span>

### <a name="static-local-functions"></a><span data-ttu-id="82236-165">statische lokale Funktionen</span><span class="sxs-lookup"><span data-stu-id="82236-165">static local functions</span></span>

<span data-ttu-id="82236-166">Dies bezieht sich auf [den Vorschlag](https://github.com/dotnet/csharplang/issues/1565) , den `static` Modifizierer für lokale Funktionen zuzulassen.</span><span class="sxs-lookup"><span data-stu-id="82236-166">This refers to [the proposal](https://github.com/dotnet/csharplang/issues/1565) to allow the `static` modifier on local functions.</span></span> <span data-ttu-id="82236-167">Eine solche Funktion wird garantiert als `static` und mit der exakten Signatur ausgegeben, die im Quellcode angegeben ist.</span><span class="sxs-lookup"><span data-stu-id="82236-167">Such a function would be guaranteed to be emitted as `static` and with the exact signature specified in source code.</span></span> <span data-ttu-id="82236-168">Eine solche Funktion sollte ein gültiges Argument für `&` sein, da Sie keines der Probleme aufweist, die lokale Funktionen heute aufweisen.</span><span class="sxs-lookup"><span data-stu-id="82236-168">Such a function should be a valid argument to `&` as it contains none of the problems local functions have today.</span></span>

### <a name="nativecallableattribute"></a><span data-ttu-id="82236-169">Nativecallableattribute</span><span class="sxs-lookup"><span data-stu-id="82236-169">NativeCallableAttribute</span></span>

<span data-ttu-id="82236-170">Die CLR verfügt über eine Funktion, mit der verwaltete Methoden so ausgegeben werden können, dass Sie direkt aus nativem Code aufgerufen werden können.</span><span class="sxs-lookup"><span data-stu-id="82236-170">The CLR has a feature that allows for managed methods to be emitted in such a way that they are directly callable from native code.</span></span> <span data-ttu-id="82236-171">Dies erfolgt durch Hinzufügen des `NativeCallableAttribute` zu Methoden.</span><span class="sxs-lookup"><span data-stu-id="82236-171">This is done by adding the `NativeCallableAttribute` to methods.</span></span> <span data-ttu-id="82236-172">Eine solche Methode kann nur aus nativem Code aufgerufen werden und muss daher nur blitfähige Typen in der Signatur enthalten.</span><span class="sxs-lookup"><span data-stu-id="82236-172">Such a method is only callable from native code and hence must contain only blittable types in the signature.</span></span> <span data-ttu-id="82236-173">Der Aufruf von verwaltetem Code führt zu einem Laufzeitfehler.</span><span class="sxs-lookup"><span data-stu-id="82236-173">Calling from managed code results in a runtime error.</span></span> 

<span data-ttu-id="82236-174">Diese Funktion ist mit diesem Vorschlag gut wie möglich:</span><span class="sxs-lookup"><span data-stu-id="82236-174">This feature would pattern well with this proposal as it would allow:</span></span>

- <span data-ttu-id="82236-175">Übergeben einer in verwaltetem Code definierten Funktion an systemeigenen Code als Funktionszeiger (über Address-of) ohne mehr Aufwand in verwaltetem oder nativem Code.</span><span class="sxs-lookup"><span data-stu-id="82236-175">Passing a function defined in managed code to native code as a function pointer (via address-of) with no overhead in managed or native code.</span></span> 
- <span data-ttu-id="82236-176">Runtime kann Verwendungs Site Fehler für solche Funktionen in verwaltetem Code einführen, um zu verhindern, dass Sie zur Kompilierzeit aufgerufen werden.</span><span class="sxs-lookup"><span data-stu-id="82236-176">Runtime can introduce use site errors for such functions in managed code to prevent them from being invoked at compile time.</span></span>




