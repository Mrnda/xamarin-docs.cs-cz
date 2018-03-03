---
title: "Úvod do jazyka C# pro vývojáře jazyka Objective-C"
description: "Xamarin.iOS umožňuje bez ohledu na platformu kód napsaný v jazyce C# být sdílen napříč platformami. Stávající aplikace pro iOS však vhodné využít jazyka Objective-C kód, který již existuje. Tento článek slouží jako krátký úvod do jazyka Objective-C vývojářům vyhledávání přesunout do Xamarin a jazyka C#."
ms.topic: article
ms.prod: xamarin
ms.assetid: 00285CBD-AE5E-4126-8F22-6B231B9467EA
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.openlocfilehash: bfc91ba92b2ed62e61d7ba99dec03784933295bd
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 02/27/2018
---
# <a name="c-primer-for-objective-c-developers"></a>Úvod do jazyka C# pro vývojáře jazyka Objective-C

_Xamarin.iOS umožňuje bez ohledu na platformu kód napsaný v jazyce C# být sdílen napříč platformami. Stávající aplikace pro iOS však vhodné využít jazyka Objective-C kód, který již existuje. Tento článek slouží jako krátký úvod do jazyka Objective-C vývojářům vyhledávání přesunout do Xamarin a jazyka C#._

iOS a OS X aplikace vyvinuté v Objective-C mohou těžit z výhod Xamarin s využitím C# na místech, kde není vyžadována, kód specifický pro platformu povolení takový kód, který se má použít na zařízeních bez Apple. Věci, jako webové služby, JSON a XML analýzy a vlastní algoritmy lze způsobem napříč platformami.

Pokud chcete využít výhod Xamarin při zachování existující prostředky jazyka Objective-C, první mohou být zpřístupněny C# v technologie z Xamarin známé jako vazeb, které surface kód jazyka Objective-C spravované, World C#. Také v případě potřeby kód lze přenést řádek po řádku jazyka C# také. Bez ohledu na to přístupu ale ať už být vazba nebo portování, některé znalost jazyka Objective-C a C# je nutné efektivně využívat stávající kód jazyka Objective-C s Xamarin.iOS.

## <a name="objective-c-interop"></a>Objective-C Interop

V současné době není žádné podporované mechanismus pro vytvoření knihovny v C# s použitím Xamarin.iOS, kterou lze volat z Objective-c Hlavním důvodem je že mono runtime je také nutný kromě vazby. Většina logika však můžete vytvořit v Objective-C, včetně uživatelského rozhraní. K tomuto účelu zabalení kód jazyka Objective-C v knihovně a vytvořte vazbu na ni. Xamarin.iOS je potřeba k navázání připojení aplikace (což znamená, musíte vytvořit `Main` vstupního bodu). Potom může být další logiku v Objective-C, vystavený C# prostřednictvím vazby (nebo P/Invoke). Tímto způsobem můžete zachovat určitou logiku platformy v Objective-C a vývoj platformy lhostejné částí v jazyce C#.

V tomto článku klade důraz některé klíče podobnosti, jakož i se liší od několika rozdíly v obou jazycích, která bude sloužit jako základy při přesunu na C# s použitím Xamarin.iOS, zda vytvoření vazby na existující kód jazyka Objective-C nebo portování jazyka C#.

Informace o vytváření vazeb naleznete v dalších dokumentů v [vazby Objective-C](~/ios/platform/binding-objective-c/index.md).

## <a name="language-comparison"></a>Porovnání jazyka

Jazyka Objective-C a C# jsou velmi různé jazyky syntakticky i z hlediska modulu runtime. Jazyka Objective-C je dynamická jazyk a používá předávání zpráv schématu, zatímco C# je staticky zadán. Jazyka Objective-C Syntax-Wise, je třeba Smalltalk, zatímco C# odvozuje většinu jeho základní syntaxe z prostředí Java, i když má vyspěla zahrnout mnoho schopnosti nad běžný Java v posledních letech.

Ale nutné dodat, že existuje několik funkcí jazyka Objective-C a C#, které jsou podobné ve funkci. Při vytváření vazby ke kódu jazyka Objective-C z jazyka C#, nebo když portování jazyka Objective-C jazyka C#, vysvětlení těchto podobnosti je užitečné.

### <a name="protocols-vs-interfaces"></a>Protokoly vs. Rozhraní

Jazyka Objective-C a C# se jedna dědičnost jazyků. Však obě jazyky je podpora implementace více rozhraní v dané třídě. V Objective-C se nazývají tyto logické rozhraní *protokoly* zatímco v jazyce C#, označují se jako *rozhraní*. Hlavní rozdíl mezi rozhraní jazyka C# a protokol jazyka Objective-C Implementation-Wise, je, že k tomu může mít volitelné metody. Další informace najdete v článku [protokoly, delegáti a události](~/ios/app-fundamentals/delegates-protocols-and-events.md) článku.

### <a name="categories-vs-extension-methods"></a>Kategorie vs. Metody rozšíření

Jazyka Objective-C umožňuje metody, které chcete přidat do třídy, pro které nemáte implementaci kódu pomocí *kategorie*. V jazyce C# je k dispozici prostřednictvím, která se označuje jako podobný koncept *rozšiřující metody*.

Rozšiřující metody umožňují přidat statické metody do třídy, kde jsou obdobou metody třídy v Objective-c statické metody v jazyce C# Například následující kód přidá metodu s názvem `ScrollToBottom` k `UITextView` třídy, který je spravovaný třídu, která je vázána na jazyka Objective-C `UITextView` třídy z UIKit:

```csharp
public static class UITextViewExtensions
{
    public static void ScrollToBottom (this UITextView textView)
    {
        // code to scroll textView
    }
}
```

Poté, pokud instance `UITextView` se vytvoří v kódu metodu bude k dispozici v seznamu automatického dokončování, jak je uvedeno níže:

 ![](primer-images/01-extensionmethodintellisense.png "Metoda dostupná ve automatického dokončování")

Při volání metody rozšíření instance předaný argument, například `textView` v tomto příkladu.

### <a name="frameworks-vs-assemblies"></a>Rozhraní vs. Sestavení

Související třídy jazyka Objective-C balíčky v speciální adresáře označuje jako rozhraní. V C# a rozhraní .NET však sestavení se používají k poskytování opakovaně použitelné bits předkompilovaných kódu. V prostředích mimo iOS sestavení obsahovat kód intermediate language (IL), který je právě v čase (JIT) kompilované za běhu. Apple však neumožňuje JIT v aplikacích pro iOS. Proto je kód C# cílení na iOS s Xamarinem předem zkompilovat (AOT), který vytvořil jeden spustitelný soubor Unix společně se soubory metadat, které jsou součástí sady aplikací.

### <a name="selectors-vs-named-parameters"></a>Selektory vs. Pojmenované parametry

Metody jazyka Objective-C ze své podstaty zahrnout názvy parametrů selektory ze své podstaty. Například selektor jako `AddCrayon:WithColor:` jasně ukazuje, co každý parametr znamená, když se používá v kódu. C# volitelně podporuje také pojmenované argumenty.

Například by být podobné kódu v C# pomocí pojmenované argumenty:

```csharp
AddCrayon (crayon: myCrayon, color: UIColor.Blue);
```

Přestože C# přidá tato podpora ve verzi 4.0 jazyk, v praxi se nepoužívá velmi často. Ale pokud budete chtít lze odvodit přímo v kódu, podporu pro něj existuje.

### <a name="headers-and-namespaces"></a>Záhlaví a obory názvů

Je nadmnožinou C, Objective-C používá hlavičky pro veřejné deklarace, které jsou oddělené od soubor implementace. C# nepoužívá soubory hlaviček. Na rozdíl od jazyka Objective-C C# – kód je obsažený v oborech názvů. Pokud chcete zahrnout kód je k dispozici v některé oboru názvů, můžete přidat buď pomocí – direktiva na začátek souboru implementace, nebo můžete kvalifikaci typu se úplný obor názvů.

Například následující kód obsahuje `UIKit` obor názvů, zpřístupnění každá třída v daném oboru názvů pro implementaci:

```csharp
using UIKit
namespace MyAppNamespace
{
    // implementation of classes
}
```

Klíčové slovo oboru názvů ve výše uvedeném kódu také nastaví obor názvů používá pro samotný soubor implementace. Pokud více souborů implementace sdílet stejný obor názvů, je není potřeba zahrnovat obor názvů v pomocí – direktiva taky, jak je implicitní.

### <a name="properties"></a>Vlastnosti

Jazyka Objective-C a C# mají koncept zajistit vysokou úroveň abstrakce kolem přístupových metod vlastností. V Objective C @property kompilátoru je použita pro efektivní generování přístupových metod. Naproti tomu C# zahrnuje podporu pro vlastnosti v rámci jazyk sám sebe. Vlastnost C# lze implementovat pomocí a delší styl, který přistupuje k zálohování pole nebo pomocí kratší, automatické vlastnost syntaxe, jak je znázorněno v následujících příkladech:

```csharp
// automatic property syntax
public string Name { get; set; }

// property implemented with a backing field
string address;
public string Address {
    get {
        // could add additional code here
        return address;
    }
    set {
        address = value;
    }
}
```

### <a name="static-keyword"></a>Static – klíčové slovo

*Statické* – klíčové slovo má velmi jiný význam mezi jazyka Objective-C a C#. V Objective-C statické funkce slouží k omezení oboru funkce, která se aktuální soubor. V jazyce C# však oboru se udržují prostřednictvím *veřejné*, *privátní* a *interní* klíčová slova.

Při použití proměnné v Objective-C static – klíčové slovo, proměnná udržuje svou hodnotu mezi volání funkcí.

C# má také static – klíčové slovo. Při použití metody, tak efektivně činí samé `+` modifikátor nemá v Objective-c Konkrétně vytvoří se metoda třídy. Podobně při použití jiných objektů, jako je například pole, vlastností a událostí, umožňuje těchto součástí typu, které jsou deklarovány v rámci a nikoli s jakoukoli instanci daného typu. Můžete provést také statická třída, ve kterém všechny metody definovaný ve třídě, musí být statické také.

### <a name="nsarray-vs-list-initialization"></a>NSArray vs. Inicializace seznamu

Jazyka Objective-C nyní zahrnuje literálu syntaxe pro použití s `NSArray`, což usnadňuje inicializaci. C# má ale bohatší typu s názvem `List` tedy *Obecné*, což znamená typ seznamu obsahuje, můžete zadat kód, který vytvoří seznam (například šablony v jazyce C++). Seznamy navíc podporují Automatická inicializace syntaxe, jak je uvedeno níže:

```csharp
MyClass object1 = new MyClass ();
MyClass object2 = new MyClass ();
List<MyClass> myList = new List<MyClass>{ object1, object2 };
```

### <a name="blocks-vs-lambda-expressions"></a>Bloky vs. Výrazy lambda

Používá jazyka Objective-C *bloky* vytvořit uzavření, kde můžete vytvořit vložené funkce, které můžete provést pomocí stavu, kde je uzavřen. C# má podobný koncept prostřednictvím výrazy lambda. V jazyce C# lambda výrazy vytvořené mají `=>` operátor, jak je uvedeno níže:

```csharp
(args) => {
    //  implementation code
};
```

Další informace o výrazy lambda, najdete v článku společnosti Microsoft [Průvodce programováním v C#](http://msdn.microsoft.com/en-us/library/vstudio/bb397687.aspx).

## <a name="summary"></a>Souhrn

V tomto článku byly rozdíl celou řadu funkcí jazyka od aktualizovaného mezi jazyka Objective-C a C#. V některých případech se volá se podobá funkce, které existují mezi oba jazyky, jako je bloky výrazy lambda a kategorie, které metody rozšíření. Kromě toho je rozdíl od aktualizovaného míst, kde jazyky odchýlit, například stejně jako u oborů názvů v jazyce C# a význam static – klíčové slovo.
