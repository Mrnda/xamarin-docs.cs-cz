---
title: Úvod do jazyka C# pro vývojáře v Objective-C
description: Tento dokument popisuje C# pro vývojáře v Objective-C. Porovná a dva jazyků se liší od zkoumáním protokolů a rozhraní, kategorie a rozšiřující metody, architektury a sestavení, selektory a pojmenované parametry a další.
ms.prod: xamarin
ms.assetid: 00285CBD-AE5E-4126-8F22-6B231B9467EA
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 06/05/2017
ms.openlocfilehash: 7fbc37a561b0a1c0b0d5a16fea2892e7faf1a86b
ms.sourcegitcommit: aa9b9b203ab4cd6a6b4fd51e27d865e2abf582c1
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/30/2018
ms.locfileid: "39351506"
---
# <a name="c-primer-for-objective-c-developers"></a>Úvod do jazyka C# pro vývojáře v Objective-C

_Xamarin.iOS umožňuje více platforem kódu napsaného v jazyce C# ke sdílení napříč platformami. Existujících aplikací pro iOS však může chtít využít kód Objective-C, které již byly vytvořeny. Tento článek slouží jako krátký přehled pro vývojáře v Objective-C uvažujete o přesunu do Xamarinu a jazyka C#._

iOS a OS X aplikace vyvinuté v jazyce Objective-C mohou těžit z Xamarin s využitím C# na místech, kde platformě závislého kódu se nevyžaduje, což takový kód, který se má použít na zařízeních bez Apple. Věci, jako webové služby, JSON a XML analýzy a vlastní algoritmy lze pak použít způsobem napříč platformami.

Xamarin využít při zachování stávajících aktiv Objective-C, Bývalé daly vystavit do jazyka C# v technologii xamarin označované jako vazby, které zařízení surface kód Objective-C pro spravované, C# celý svět. Také v případě potřeby kód může být přenesená řádek po řádku jazyka C# také. Bez ohledu na to, přístup však, zda se vazba nebo přenesení, základní znalost jazyka Objective-C a C# je potřeba efektivně využít existující kód Objective-C s Xamarin.iOS.

## <a name="objective-c-interop"></a>Zprostředkovatel komunikace s objekty jazyka Objective-C

V současné době není podporovaný žádný mechanismus pro vytvoření knihovny v jazyce C# s použitím Xamarin.iOS, která může být volána z Objective-C. Hlavním důvodem pro to je, že modul Mono runtime je také nutný kromě vazby. Většina svoji logiku však můžete vytvořit v Objective-C, včetně uživatelského rozhraní. K tomuto účelu zabalit kód Objective-C v knihovně a vytvořit vazbu k němu. Xamarin.iOS je potřeba ke spuštění aplikace (to znamená, se musí vytvořit `Main` vstupního bodu). Potom může být další logiku v jazyce Objective-C, vystaveni C# prostřednictvím vazby (nebo prostřednictvím P/Invoke). Díky tomu můžete zachovat logika specifická pro platformy v Objective-C a vývoj platformy bez částí v jazyce C#.

Tento článek některé klíčovým podobnostem zvýrazňuje, jakož i několik rozdílů v oba jazyky, která bude sloužit jako základy při přesunu do jazyka C# s Xamarin.iOS, zda vazba vytváří k existující kód Objective-C nebo přenos do jazyka C# se liší od.

Podrobnosti o vytváření vazeb naleznete v části Další dokumenty v [vazeb Objective-C](~/ios/platform/binding-objective-c/index.md).

## <a name="language-comparison"></a>Porovnání jazyků

Objective-C a C# jsou velmi různých jazycích syntakticky i z hlediska modulu runtime. Objective-C je dynamické jazyk a používá předávání zpráv schéma, zatímco C# je staticky zadán. Syntax-Wise Objective-C je jako Smalltalk, zatímco C# je odvozena velkou část jeho základní syntaxe z Javy, i když vyvstává zahrnout mnoho možnosti nad rámec běžné Java v posledních letech.

Ale nutné dodat, že existuje několik funkcí jazyka Objective-C a C#, které jsou podobné funkce. Při vytváření vazeb Objective-C kódu z jazyka C#, nebo při přenosu je užitečné Objective-C jazyka C#, pochopení těchto podobnosti.

### <a name="protocols-vs-interfaces"></a>Protokoly vs. Rozhraní

Objective-C a C# jsou jednoduchá dědičnost jazyky. Ale oba jazyky mají podporu pro implementaci rozhraní více v dané třídy. V jazyce Objective-C se nazývají těchto logických rozhraní *protokoly* vzhledem k tomu v jazyce C# jsou volány *rozhraní*. Hlavní rozdíl mezi rozhraní jazyka C# a protokol Objective-C Implementation-Wise, je že ten nesmí být volitelné metody. Další informace najdete v článku [protokolů, delegáti a události](~/ios/app-fundamentals/delegates-protocols-and-events.md) článku.

### <a name="categories-vs-extension-methods"></a>Kategorie vs. Rozšiřující metody

Objective-C umožňuje metody mají být přidány do třídy, pro které nemáte provádění kódu pomocí *kategorie*. V jazyce C# je k dispozici prostřednictvím co se označuje jako podobný koncept *rozšiřující metody*.

Rozšiřující metody umožňují přidat statické metody do třídy, ve kterém jsou podobná metody třídy v jazyce Objective-C. statické metody v jazyce C# Například následující kód přidá metodu s názvem `ScrollToBottom` k `UITextView` třídy, který je zase spravované třídy, která je vázána na Objective-C `UITextView` třídy z UIKit:

```csharp
public static class UITextViewExtensions
{
    public static void ScrollToBottom (this UITextView textView)
    {
        // code to scroll textView
    }
}
```

Potom, pokud instance `UITextView` se vytvoří v kódu, metoda bude k dispozici v seznamu Automatické dokončování, jak je znázorněno níže:

 ![](primer-images/01-extensionmethodintellisense.png "K dispozici v automatické dokončování – metoda")

Při volání metody rozšíření instance je předán jako argument, `textView` v tomto příkladu.

### <a name="frameworks-vs-assemblies"></a>Rozhraní vs. Sestavení

Balíčky jazyka Objective-C související třídy v speciální adresáře označované jako rámce. V jazyce C# a .NET však sestavení se používají k poskytování opakovaně použitelné bits předkompilovaný kód. V prostředí mimo iOS sestavení obsahují kód intermediate language (IL), který je za běhu (JIT) kompilaci za běhu. Apple však neumožňuje JIT v aplikacích pro iOS. Proto kód jazyka C# cílících na iOS pomocí Xamarinu je předem zkompilovat (AOT), vytváření jeden spustitelný soubor systému Unix spolu se soubory metadat, které jsou zahrnuty do sady prostředků aplikace.

### <a name="selectors-vs-named-parameters"></a>Selektory vs. Pojmenované parametry

Objective-C metody ze své podstaty obsahovat názvy parametrů v selektorech ze své podstaty. Například výběr jako `AddCrayon:WithColor:` je zřejmé, co každý parametr znamená, že při použití v kódu. Volitelně podporuje jazyk C# také pojmenované argumenty.

Například by být podobný kód v C# pomocí pojmenované argumenty:

```csharp
AddCrayon (crayon: myCrayon, color: UIColor.Blue);
```

Ačkoli C# ve verzi 4.0 jazyka přidání této podpory, v praxi není použit příliš často. Pokud chcete explicitně v kódu, podporu, ale existuje.

### <a name="headers-and-namespaces"></a>Hlavičky a obory názvů

Je nadstavbou jazyka C, Objective-C používá hlavičky pro veřejné deklarace, které jsou oddělené od implementační soubor. C# nepoužívá soubory hlaviček. Na rozdíl od jazyka Objective-C kód jazyka C# je součástí oborů názvů. Pokud chcete zahrnout kód je k dispozici v některých oboru názvů, můžete přidat buď using – direktiva k hornímu okraji implementace souboru nebo se kvalifikaci typu s plnou oboru názvů.

Například následující kód obsahuje `UIKit` obor názvů, zpřístupnění každá třída v tomto oboru názvů pro implementaci:

```csharp
using UIKit
namespace MyAppNamespace
{
    // implementation of classes
}
```

Obor názvů – klíčové slovo v kódu uvedeného výše se nastaví také, oboru názvů použitého pro samotný soubor implementace. Pokud více souborů implementace sdílejí stejný obor názvů, není nutné zahrnout oboru názvů using – direktiva stejně, jako je vyjádřena.

### <a name="properties"></a>Vlastnosti

Objective-C a C# využívá koncepce vlastností, které budou poskytovat vysokou úroveň abstrakce kolem přístupové metody. V jazyce Objective-C @property direktivy kompilátoru slouží k efektivní generovat přístupové metody. Naproti tomu C# obsahuje podporu pro vlastnosti v rámci samotný jazyk. Vlastnost jazyka C# je možné implementovat pomocí a delší styl, který přistupuje k pomocné pole nebo pomocí kratší, automatickou vlastnost syntaxe, jak je znázorněno v následujícím příkladu:

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

*Statické* – klíčové slovo má velmi odlišný význam mezi Objective-C a C#. V jazyce Objective-C statické funkce umožňují omezit obor funkce, která se aktuální soubor. V jazyce C# však oboru se udržují prostřednictvím *veřejné*, *privátní* a *interní* klíčová slova.

Při static – klíčové slovo použití proměnné v jazyce Objective-C, proměnnou udržuje jeho hodnota ve volání funkce.

C# obsahuje také static – klíčové slovo. Při použití metody, je efektivně provede stejnou akci, která `+` modifikátor nemá v jazyce Objective-C. Konkrétně vytvoří metodu třídy. Podobně při použití jiných objektů, jako je například pole, vlastnosti a události, díky těchto součástí typu, které jsou deklarovány v rámci, nikoli s jakoukoli instanci daného typu. Můžete provést také statické třídy, ve kterém všechny metody, které jsou definovány ve třídě, musí být statické i.

### <a name="nsarray-vs-list-initialization"></a>NSArray vs. Inicializace seznamu

Objective-C teď obsahuje syntaxi literálu pro použití s `NSArray`, což usnadňuje inicializovat. C# však má širší typ s názvem `List` tedy *obecný*, což znamená, typ obsahuje seznamu lze zadat kód, který vytvoří seznam (jako jsou šablony v jazyce C++). Seznamy navíc podporují syntaxe inicializace automaticky, jak je znázorněno níže:

```csharp
MyClass object1 = new MyClass ();
MyClass object2 = new MyClass ();
List<MyClass> myList = new List<MyClass>{ object1, object2 };
```

### <a name="blocks-vs-lambda-expressions"></a>Bloky vs. Výrazy lambda

Objective-C používá *bloky* vytvoříte uzávěry kde si můžete vytvořit vložené funkce, které můžete provést pomocí stavu, ve kterém je uzavřen. C# používá podobný koncept pomocí výrazů lambda. V jazyce C# lambda výrazy jsou vytvořeny pomocí `=>` operátor, jak je znázorněno níže:

```csharp
(args) => {
    //  implementation code
};
```

Další informace o výrazech lambda naleznete v tématu Microsoftu [Průvodce programováním v C#](http://msdn.microsoft.com/library/vstudio/bb397687.aspx).

## <a name="summary"></a>Souhrn

V tomto článku byly širokou škálu funkcí jazyka rozdíly mezi Objective-C a C#. V některých případech se volá na obdobné funkce, které existují mezi oba jazyky, jako je například bloky pro výrazy lambda a kategorie, které metody rozšíření. Kromě toho rozdíly místa, kde jazyky odchýlit, například stejně jako u oborů názvů v jazyce C# a význam static – klíčové slovo.
