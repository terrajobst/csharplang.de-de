---
ms.openlocfilehash: e56caa7b2fabb4b5ade242ec43f4592689e8ba3d
ms.sourcegitcommit: 7f7fc6e9e195e51b7ff8229aeaa70aa9fbbb63cb
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 09/10/2019
ms.locfileid: "70876824"
---
<a name="c-language-specification"></a>C#-Programmiersprachenspezifikation
===========================

__Version 6__

Dabei handelt es sich um einen inoffiziellen Entwurf, der Ihnen zur Verfügung gestellt wird. Wenn wir einen 6,0 C# -spec-Vorschlag an ECMA übermitteln, wird dieser hier freigegeben.

<!--
(This document is also available for download: [csharp.pdf](CSharp%20Language%20Specification.pdf?raw=true) and [csharp.docx](CSharp%20Language%20Specification.docx?raw=true))
-->

* [Introduction (Einführung)](introduction.md)
    * [Hello World](introduction.md#hello-world)
    * [Programmstruktur](introduction.md#program-structure)
    * [Typen und Variablen](introduction.md#types-and-variables)
    * [Ausdrücke](introduction.md#expressions)
    * [Anweisungen](introduction.md#statements)
    * [Klassen und Objekte](introduction.md#classes-and-objects)
* [Lexical structure (Lexikalische Struktur)](lexical-structure.md)
    * [Programme](lexical-structure.md#programs)
    * [Grammars (Grammatik)](lexical-structure.md#grammars)
    * [Lexical analysis (Lexikalische Analyse)](lexical-structure.md#lexical-analysis)
    * [Tokens (Tokens)](lexical-structure.md#tokens)
    * [Pre-processing directives (Vorverarbeitungsanweisungen)](lexical-structure.md#pre-processing-directives)
* [Basic concepts (Grundlegende Konzepte)](basic-concepts.md)
    * [Application Startup (Starten von Anwendungen)](basic-concepts.md#application-startup)
    * [Application termination (Beenden von Anwendungen)](basic-concepts.md#application-termination)
    * [Deklarationen](basic-concepts.md#declarations)
    * [Mitglieder](basic-concepts.md#members)
    * [Member access (Memberzugriff)](basic-concepts.md#member-access)
    * [Signatures and overloading (Signaturen und Überladen)](basic-concepts.md#signatures-and-overloading)
    * [Scopes (Bereiche)](basic-concepts.md#scopes)
    * [Namespace and type names (Namespace- und Typnamen)](basic-concepts.md#namespace-and-type-names)
    * [Automatic Memory Management](basic-concepts.md#automatic-memory-management)
    * [Execution order (Ausführungsreihenfolge)](basic-concepts.md#execution-order)
* [Typen](types.md)
    * [Value types (Werttypen)](types.md#value-types)
    * [Reference types (Verweistypen)](types.md#reference-types)
    * [Boxing and unboxing (Boxing und Unboxing)](types.md#boxing-and-unboxing)
    * [Constructed types (Konstruierte Typen)](types.md#constructed-types)
    * [Type parameters (Typparameter)](types.md#type-parameters)
    * [Expression tree types (Ausdrucksbaumstrukturtypen)](types.md#expression-tree-types)
    * [The dynamic type (Der dynamische Typ)](types.md#the-dynamic-type)
* [Variablen](variables.md)
    * [Variable categories (Variable Kategorien)](variables.md#variable-categories)
    * [Default values (Standardwerte)](variables.md#default-values)
    * [Definite assignment (Festgelegte Zuweisung)](variables.md#definite-assignment)
    * [Variable references (Variable Verweise)](variables.md#variable-references)
    * [Atomicity of variable references (Unteilbarkeit variabler Verweise)](variables.md#atomicity-of-variable-references)
* [Conversions (Konvertierungen)](conversions.md)
    * [Implicit conversions (Implizite Konvertierungen)](conversions.md#implicit-conversions)
    * [Explicit conversions (Explizite Konvertierungen)](conversions.md#explicit-conversions)
    * [Standard conversions (Standardkonvertierungen)](conversions.md#standard-conversions)
    * [User-defined conversions (Benutzerdefinierte Konvertierungen)](conversions.md#user-defined-conversions)
    * [Anonymous function conversions (Konvertierung anonymer Funktionen)](conversions.md#anonymous-function-conversions)
    * [Method group conversions (Konvertierung von Methodengruppen)](conversions.md#method-group-conversions)
* [Ausdrücke](expressions.md)
    * [Expression classifications (Ausdrucksklassifizierungen)](expressions.md#expression-classification)
    * [Static and Dynamic Binding (Statische und dynamische Bindung)](expressions.md#static-and-dynamic-binding)
    * [Operatoren](expressions.md#operators)
    * [Member lookup (Membersuche)](expressions.md#member-lookup)
    * [Function members (Funktionselemente)](expressions.md#function-members)
    * [Primary expressions (Primäre Ausdrücke)](expressions.md#primary-expressions)
    * [Unary operators (Unäre Operatoren)](expressions.md#unary-operators)
    * [Arithmetic operators (Arithmetische Operatoren)](expressions.md#arithmetic-operators)
    * [Shift operators (Schiebeoperatoren)](expressions.md#shift-operators)
    * [Relational and type-testing operators (Relationale und Typtestoperatoren)](expressions.md#relational-and-type-testing-operators)
    * [Logical operators (Logische Operatoren)](expressions.md#logical-operators)
    * [Conditional logical operators (Bedingte logische Operatoren)](expressions.md#conditional-logical-operators)
    * [The null coalescing operator (Der NULL-Sammeloperator)](expressions.md#the-null-coalescing-operator)
    * [Conditional operator (Bedingte Operatoren)](expressions.md#conditional-operator)
    * [Anonymous function expressions (Anonyme Funktionsausdrücke)](expressions.md#anonymous-function-expressions)
    * [Query expressions (Abfrageausdrücke)](expressions.md#query-expressions)
    * [Assignment operators (Zuweisungsoperatoren)](expressions.md#assignment-operators)
    * [Expression (Ausdruck)](expressions.md#expression)
    * [Constant expressions (Konstante Ausdrücke)](expressions.md#constant-expressions)
    * [Boolean expressions (Boolesche Ausdrücke)](expressions.md#boolean-expressions)
* [Anweisungen](statements.md)
    * [End points and reachability (Endpunkte und Erreichbarkeit)](statements.md#end-points-and-reachability)
    * [Blöcke](statements.md#blocks)
    * [The empty statement (Die leere Anweisung)](statements.md#the-empty-statement)
    * [Labeled statements (Anweisungen mit Bezeichnung)](statements.md#labeled-statements)
    * [Declaration statements (Deklarationsanweisungen)](statements.md#declaration-statements)
    * [Expression statements (Ausdrucksanweisungen)](statements.md#expression-statements)
    * [Auswahlanweisungen](statements.md#selection-statements)
    * [Iterationsanweisungen](statements.md#iteration-statements)
    * [Sprunganweisungen](statements.md#jump-statements)
    * [The try statement (Die try-Anweisung)](statements.md#the-try-statement)
    * [The checked and unchecked statements (Geprüfte und nicht geprüfte Anweisungen)](statements.md#the-checked-and-unchecked-statements)
    * [The lock statement (Die lock-Anweisung)](statements.md#the-lock-statement)
    * [The using statement (Die Using-Anweisung)](statements.md#the-using-statement)
    * [The yield statement (Die yield-Anweisung)](statements.md#the-yield-statement)
* [Namespaces](namespaces.md)
    * [Compilation units (Kompilationseinheiten)](namespaces.md#compilation-units)
    * [Namespace declarations (Namespacedeklarationen)](namespaces.md#namespace-declarations)
    * [Extern aliases (Externe Aliase)](namespaces.md#extern-aliases)
    * [Using directives (Using-Direktiven)](namespaces.md#using-directives)
    * [Namespace members (Namespacemember)](namespaces.md#namespace-members)
    * [Type declarations (Typdeklarationen)](namespaces.md#type-declarations)
    * [Namespace alias qualifiers (Qualifizierer für Namespacealiase)](namespaces.md#namespace-alias-qualifiers)
* [Klassen](classes.md)
    * [Class declarations (Klassendeklarationen)](classes.md#class-declarations)
    * [Partial types (Partielle Typen)](classes.md#partial-types)
    * [Class members (Klassenmember)](classes.md#class-members)
    * [Konstanten](classes.md#constants)
    * [Felder](classes.md#fields)
    * [Methoden](classes.md#methods)
    * [Eigenschaften](classes.md#properties)
    * [Ereignisse](classes.md#events)
    * [Indexer](classes.md#indexers)
    * [Operatoren](classes.md#operators)
    * [Instance constructors (Instanzkonstruktoren)](classes.md#instance-constructors)
    * [Statische Konstruktoren](classes.md#static-constructors)
    * [Destruktoren](classes.md#destructors)
    * [Iteratoren](classes.md#iterators)
    * [Async-Funktionen](classes.md#async-functions)
* [Strukturen](structs.md)
    * [Struct declarations (Strukturdeklarationen)](structs.md#struct-declarations)
    * [Struct members (Strukturmember)](structs.md#struct-members)
    * [Class and struct differences (Klassen- und Strukturdifferenzen)](structs.md#class-and-struct-differences)
    * [Struct examples (Strukturbeispiele)](structs.md#struct-examples)
* [Arrays](arrays.md)
    * [Array types (Arraytypen)](arrays.md#array-types)
    * [Array creation (Arrayerstellung)](arrays.md#array-creation)
    * [Array element access (Zugriff auf Arrayelemente)](arrays.md#array-element-access)
    * [Array members (Arraymember)](arrays.md#array-members)
    * [Array covariance (Array-Kovarianz)](arrays.md#array-covariance)
    * [Array initializers (Arrayinitialisierer)](arrays.md#array-initializers)
* [Schnittstellen](interfaces.md)
    * [Interface declarations (Schnittstellendeklarationen)](interfaces.md#interface-declarations)
    * [Interface members (Schnittstellenmember)](interfaces.md#interface-members)
    * [Fully qualified interface member names (Vollqualifizierte Schnittstellenmembernamen)](interfaces.md#fully-qualified-interface-member-names)
    * [Interface implementations (Schnittstellenimplementierung)](interfaces.md#interface-implementations)
* [Enumerationen](enums.md)
    * [Enum declarations (Enum-Deklarationen)](enums.md#enum-declarations)
    * [Enum modifiers (Enumerationsmodifizierer)](enums.md#enum-modifiers)
    * [Enum members (Enumerationsmember)](enums.md#enum-members)
    * [The System.Enum type (Der System.Enum-Typ)](enums.md#the-systemenum-type)
    * [Enum values and operations (Enumerationswerte und -vorgänge)](enums.md#enum-values-and-operations)
* [Delegaten](delegates.md)
    * [Delegate declarations (Delegatdeklarationen)](delegates.md#delegate-declarations)
    * [Delegate compatibility (Delegatkompatibilität)](delegates.md#delegate-compatibility)
    * [Delegate instantiation (Delegatinstanziierung)](delegates.md#delegate-instantiation)
    * [Delegate invocation (Delegataufruf)](delegates.md#delegate-invocation)
* [Ausnahmen](exceptions.md)
    * [Causes of exceptions (Ursachen von Ausnahmen)](exceptions.md#causes-of-exceptions)
    * [The System.Exception class (Die System.Exception-Klasse)](exceptions.md#the-systemexception-class)
    * [How exceptions are handled (Wie Ausnahmen behandelt werden)](exceptions.md#how-exceptions-are-handled)
    * [Common Exception Classes (Allgemeine Ausnahmeklassen)](exceptions.md#common-exception-classes)
* [Attribute](attributes.md)
    * [Attribute classes (Attributklassen)](attributes.md#attribute-classes)
    * [Attribute specification (Attributspezifikation)](attributes.md#attribute-specification)
    * [Attribute instances (Attributinstanzen)](attributes.md#attribute-instances)
    * [Reserved attributes (Reservierte Attribute)](attributes.md#reserved-attributes)
    * [Attributes for Interoperation (Attribute zur Interoperation)](attributes.md#attributes-for-interoperation)
* [Unsicherer Code](unsafe-code.md)
    * [Unsafe contexts (Unsichere Kontexte)](unsafe-code.md#unsafe-contexts)
    * [Zeigertypen](unsafe-code.md#pointer-types)
    * [Fixed and moveable variables (Feste und verschiebbare Variablen)](unsafe-code.md#fixed-and-moveable-variables)
    * [Pointer conversions (Zeigerkonvertierungen)](unsafe-code.md#pointer-conversions)
    * [Pointers in expressions (Zeiger in Ausdrücken)](unsafe-code.md#pointers-in-expressions)
    * [The fixed statement (Die fixed-Anweisung)](unsafe-code.md#the-fixed-statement)
    * [Fixed size buffers (Puffer fester Größe)](unsafe-code.md#fixed-size-buffers)
    * [Stack allocation (Stapelreservierung)](unsafe-code.md#stack-allocation)
    * [Dynamic memory allocation (Dynamische Speicherbelegung)](unsafe-code.md#dynamic-memory-allocation)
* [Documentation comments (Dokumentationskommentare)](documentation-comments.md)
    * [Introduction (Einführung)](documentation-comments.md#introduction)
    * [Recommended tags (Empfohlene Tags)](documentation-comments.md#recommended-tags)
    * [Processing the documentation file (Verarbeiten der Dokumentationsdatei)](documentation-comments.md#processing-the-documentation-file)
    * [An example (Ein Beispiel)](documentation-comments.md#an-example)

<!--
* Grammar: [csharp.html](http://ljw1004.github.io/csharpspec/csharp.html). Or download in ANTLR format: [csharp.g4](csharp.g4?raw=true). 
-->
