---
title: Souhrn kapitoly 8. Kód a XAML v souladu
ms.prod: xamarin
ms.technology: xamarin-forms
ms.assetid: 5970DEEB-1FC9-4F78-B4F6-D403E16D22ED
author: charlespetzold
ms.author: chape
ms.date: 11/07/2017
ms.openlocfilehash: ffd2d508e99508309ec07c6bc65c8d716427bdff
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/04/2018
---
# <a name="summary-of-chapter-8-code-and-xaml-in-harmony"></a>Souhrn kapitoly 8. Kód a XAML v souladu

Tato kapitola zde popsány XAML hlubšímu a zvláště kód XAML interakci a.

## <a name="passing-arguments"></a>Předání argumentů

V případě obecné třídu instanci v jazyce XAML, musí mít veřejný konstruktor; Výsledný objekt je inicializován prostřednictvím nastavení vlastností. Ale existují dva další způsoby, objekty, můžete vytvořit instance a inicializován.

I když jsou tyto pro obecné účely techniky, se nejčastěji používá ve spojení s modelem MVVM Zobrazit modely.

### <a name="constructors-with-arguments"></a>Konstruktory argumentů.

[ **ParameteredConstructorDemo** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter08/ParameteredConstructorDemo) příklad ukazuje způsob použití `x:Arguments` značky chcete zadat argumenty konstruktoru. Tyto argumenty musí být odděleny značky elementu označující typ argumentu. Pro základní datové typy .NET jsou k dispozici následující značky:

- `x:Object`
- `x:Boolean`
- `x:Byte`
- `x:Int16`
- `x:Int32`
- `x:Int64`
- `x:Single`
- `x:Double`
- `x:Decimal`
- `x:Char`
- `x:String`
- `x:TimeSpan`
- `x:Array`
- `x:DateTime`

### <a name="can-i-call-methods-from-xaml"></a>Můžete volat metody z XAML?

[ **FactoryMethodDemo** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter08/FactoryMethodDemo) příklad ukazuje způsob použití `x:FactoryMethod` elementu, který chcete zadat metoda factory, která je vyvolána k vytvoření objektu. Tato metoda factory musí být veřejné a statické, a musí vytvořit objekt typu, ve kterém je definovaný. (Například [ `Color.FromRgb` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Color.FromRgb/p/System.Double/System.Double/System.Double/)) metoda vyfiltrování, protože je veřejná a statické a vrátí hodnotu typu `Color`.) Jsou zadané argumenty, které se metoda objektu factory v rámci `x:Arguments` značky.

## <a name="the-xname-attribute"></a>X: Name – atribut

`x:Name` Atribut umožňuje vytvoření v jazyce XAML, které má být poskytnut název instance objektu. Pravidla pro tyto názvy jsou stejné jako názvy proměnných C#. Následující návrat `InitializeComponent` volání v konstruktoru, souboru kódu na pozadí se může vztahovat na tyto názvy pro přístup k odpovídající element XAML. Názvy jsou ve skutečnosti převést analyzátorem jazyka XAML do privátním polím v generované třídu.

[ **XamlClock** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter08/XamlClock) příklad ukazuje použití `x:Name` povolíte souboru kódu na pozadí udržovat dva `Label` prvky, které jsou definované v jazyce XAML aktualizovat s aktuálním datem a časem.

Stejný název nelze použít pro více prvků na stejné stránce. Pokud použijete tento konkrétní problém je `OnPlatform` k vytvoření paralelní s názvem objekty pro každou platformu. [ **PlatformSpecificLabele** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter08/PlatformSpecificLabels) ukázka představuje lepší způsob, jak dělat něco jako je například.

## <a name="custom-xaml-based-views"></a>Vlastní pohledy založených na XAML

Existuje několik způsobů, aby se zabránilo opakování značek v jazyce XAML. Jednou z běžných technik je vytvořte novou třídu založených na XAML, která je odvozena z [ `ContentView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentView/). Tento postup je znázorněn v [ **ColorViewList** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter08/ColorViewList) ukázka. `ColorView` Třída odvozená z `ContentView` Pokud chcete zobrazit konkrétní barvy a jeho název, při `ColorViewListPage` třída odvozená z `ContentPage` obvyklým a explicitně vytváří 17 instance `ColorView`.

Přístup k `ColorView` třídy v jazyce XAML vyžaduje jiný deklaraci oboru názvů XML, obvykle s názvem `local` pro třídy ve stejném sestavení.

## <a name="events-and-handlers"></a>Události a obslužné rutiny

Události lze přiřadit k obslužné rutiny událostí v jazyce XAML, ale samotný obslužné rutiny události musí být implementován v souboru kódu na pozadí. [ **XamlKeypad** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter08/XamlKeypad) ukazuje postup vytvoření uživatelského rozhraní v jazyce XAML klávesnici a k implementaci `Clicked` obslužné rutiny v souboru kódu na pozadí.

## <a name="tap-gestures"></a>Klepněte na gesta

Všechny `View` objekt můžete získat dotykové ovládání a generovat události z tento vstup. `View` Třída definuje [ `GestureRecognizers` ](https://developer.xamarin.com/api/property/Xamarin.Forms.View.GestureRecognizers/) vlastnost kolekce, která může obsahovat jednu nebo více instancí třídy, které jsou odvozeny od [ `GestureRecognizer` ](https://developer.xamarin.com/api/type/Xamarin.Forms.GestureRecognizer/).

[ `TapGestureRecognizer` ](https://developer.xamarin.com/api/type/Xamarin.Forms.TapGestureRecognizer/) Generuje [ `Tapped` ](https://developer.xamarin.com/api/event/Xamarin.Forms.TapGestureRecognizer.Tapped/) události. [ **MonkeyTap** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter08/MonkeyTap) program ukazuje, jak připojit `TapGestureRecognizer` objekty čtyři `BoxView` elementy vytvořit napodobeniny herní:

[![Trojitá snímek obrazovky klepněte na opic](images/ch08fg07-small.png "napodobení herní")](images/ch08fg07-large.png#lightbox "napodobení hra")

Ale **MonkeyTap** program opravdu potřebuje zvuku. (Viz [další kapitoly](chapter09.md).)



## <a name="related-links"></a>Související odkazy

- [Úplný text kapitoly 8 (PDF)](https://download.xamarin.com/developer/xamarin-forms-book/XamarinFormsBook-Ch08-Apr2016.pdf)
- [Ukázky kapitoly 8](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter08)
- [Ukázka kapitoly 8 F #](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter08/FS/XamlKeypad)
