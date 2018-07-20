---
title: Souhrn kapitoly 8. Kód a XAML v souladu
description: 'Vytváření mobilních aplikací s Xamarin.Forms: Souhrn kapitoly 8. Kód a XAML v souladu'
ms.prod: xamarin
ms.technology: xamarin-forms
ms.assetid: 5970DEEB-1FC9-4F78-B4F6-D403E16D22ED
author: charlespetzold
ms.author: chape
ms.date: 07/19/2018
ms.openlocfilehash: 1aa5226e1e6867f77eea4d7679650e8d62f5b981
ms.sourcegitcommit: 8555a4dd1a579b2206f86c867125ee20fbc3d264
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/19/2018
ms.locfileid: "39156987"
---
# <a name="summary-of-chapter-8-code-and-xaml-in-harmony"></a>Souhrn kapitoly 8. Kód a XAML v souladu

Tato kapitola popisuje XAML hlouběji a zejména kód a XAML interakci.

## <a name="passing-arguments"></a>Předávání argumentů

V tomto obecném případě třída vytvořena instance v XAML musí mít veřejný konstruktor bez parametrů; Výsledný objekt je inicializován pomocí nastavení vlastnosti. Existují však dvěma dalšími způsoby, objektů můžete vytvořit instanci a inicializován.

I když tyto techniky pro obecné účely, se nejčastěji používají v souvislosti s modelem MVVM Zobrazit modely.

### <a name="constructors-with-arguments"></a>Konstruktory s argumenty

[ **ParameteredConstructorDemo** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter08/ParameteredConstructorDemo) Ukázka předvádí, jak používat `x:Arguments` značka zadat argumenty konstruktoru. Tyto argumenty musí být odděleny značky elementů určující typ argumentu. Pro základní datové typy .NET jsou k dispozici následující značky:

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

### <a name="can-i-call-methods-from-xaml"></a>Můžete volat metody z XAML

[ **FactoryMethodDemo** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter08/FactoryMethodDemo) Ukázka předvádí, jak používat `x:FactoryMethod` element chcete zadat objekt pro vytváření metodu, která se vyvolá pro vytvoření objektu. Metoda factory musí být veřejná a statická a se musí vytvořit objekt typu, ve kterém je definována. (Například [ `Color.FromRgb` ](xref:Xamarin.Forms.Color.FromRgb(System.Double,System.Double,System.Double)) metoda kvalifikuje, protože není veřejná a statická a vrátí hodnotu typu `Color`.) Argumenty pro výrobní metoda jsou určené v rámci `x:Arguments` značky.

## <a name="the-xname-attribute"></a>Atribut x: Name

`x:Name` Atribut umožňuje vytvořit instanci v XAML název objektu. Pravidla pro tyto názvy jsou stejné jako názvy proměnných jazyka C#. Po návratu `InitializeComponent` volání v konstruktoru, soubor kódu na pozadí mohou odkazovat na tyto názvy pro přístup k odpovídající prvek XAML. Názvy jsou ve skutečnosti převedené pomocí analyzátoru XAML do privátní pole v vygenerovanou dílčí třídu.

[ **XamlClock** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter08/XamlClock) ukázka demonstruje použití `x:Name` umožňující použití modelu code-behind souboru se má zachovat dvě `Label` prvky definované v XAML aktualizovat s aktuálním datem a časem.

Se stejným názvem nelze použít pro několik elementů na stejné stránce. To je konkrétního problému, když používáte `OnPlatform` vytvoření paralelní s názvem objekty pro každou platformu. [ **PlatformSpecificLabele** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter08/PlatformSpecificLabels) ukázce lepší způsob, jak něco udělat tímto způsobem.

## <a name="custom-xaml-based-views"></a>Vlastní zobrazení XAML

Existuje několik způsobů, jak zabránit opakování kódu v XAML. Jednou z běžných metod je pro vytvoření nové třídy založené na XAML, který je odvozen z [ `ContentView` ](xref:Xamarin.Forms.ContentView). Tato technika je patrné [ **ColorViewList** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter08/ColorViewList) vzorku. `ColorView` Třída odvozena z `ContentView` k zobrazení určité barvy a jeho název, zatímco `ColorViewListPage` třída odvozena z `ContentPage` obvyklým způsobem a explicitně vytvoří 17 instancí `ColorView`.

Přístup k `ColorView` třídy v XAML vyžaduje jiné deklarace oboru názvů XML, obvykle s názvem `local` pro třídy ve stejném sestavení.

## <a name="events-and-handlers"></a>Události a obslužné rutiny

Události je možné přiřadit k obslužné rutiny událostí v XAML, ale samotná obslužná rutina události musí být implementován v souboru kódu na pozadí. [ **XamlKeypad** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter08/XamlKeypad) ukazuje, jak vytvořit uživatelské rozhraní klávesnice v XAML a jak implementovat `Clicked` obslužných rutin v souboru kódu na pozadí.

## <a name="tap-gestures"></a>Klepněte na gesta

Žádné `View` objektu můžete získat dotykové ovládání a generovat události z těchto informací. `View` Definuje třídu [ `GestureRecognizers` ](xref:Xamarin.Forms.View.GestureRecognizers) vlastnost kolekce, která může obsahovat jednu nebo víc instancí třídy, které jsou odvozeny z [ `GestureRecognizer` ](xref:Xamarin.Forms.GestureRecognizer).

[ `TapGestureRecognizer` ](xref:Xamarin.Forms.TapGestureRecognizer) Generuje [ `Tapped` ](xref:Xamarin.Forms.TapGestureRecognizer.Tapped) události. [ **MonkeyTap** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter08/MonkeyTap) program ukazuje, jak připojit `TapGestureRecognizer` objekty na čtyři `BoxView` prvky pro vytvoření imitace hry:

[![Trojitá snímek obrazovky klepněte na opic](images/ch08fg07-small.png "imitace hru")](images/ch08fg07-large.png#lightbox "imitace hry")

Ale **MonkeyTap** program potřebuje zvuk. (Viz [další kapitolu](chapter09.md).)

## <a name="related-links"></a>Související odkazy

- [8 kapitoly textu v plném znění (PDF)](https://download.xamarin.com/developer/xamarin-forms-book/XamarinFormsBook-Ch08-Apr2016.pdf)
- [Ukázky kapitoly 8](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter08)
- [Ukázka kapitoly 8 F #](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter08/FS/XamlKeypad)
- [Předávání argumentů v XAML](~/xamarin-forms/xaml/passing-arguments.md)
