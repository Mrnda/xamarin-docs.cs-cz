---
title: Souhrn kapitoly 25. Variace stránek
description: 'Vytváření mobilních aplikací s Xamarin.Forms: Souhrn kapitoly 25. Variace stránek'
ms.prod: xamarin
ms.technology: xamarin-forms
ms.assetid: D1D348F2-6A44-4781-ADCE-A0B7BB9AEF89
author: charlespetzold
ms.author: chape
ms.date: 11/07/2017
ms.openlocfilehash: 148388b80137bd335bbb977ea230726da1f4a32d
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/12/2018
ms.locfileid: "38995882"
---
# <a name="summary-of-chapter-25-page-varieties"></a>Souhrn kapitoly 25. Variace stránek

Zatím jste se seznámili dvě třídy, které jsou odvozeny z `Page`: `ContentPage` a `NavigationPage`. Tato kapitola představuje dvěma dalšími:

- [`MasterDetailPage`](xref:Xamarin.Forms.MasterDetailPage) spravuje dvě stránky předlohy a podrobností
- [`TabbedPage`](xref:Xamarin.Forms.TabbedPage) spravuje více podřízené stránky přístupné prostřednictvím karet

Tyto stránky typy poskytují sofistikovanější Možnosti navigačního než `NavagationPage` podrobněji [kapitoly 24. Navigační stránka](~/xamarin-forms/creating-mobile-apps-xamarin-forms/summaries/chapter24.md).

## <a name="master-and-detail"></a>Seznam a podrobnosti

[ `MasterDetailPage` ](xref:Xamarin.Forms.MasterDetailPage) Definuje dvě vlastnosti typu `Page`: [ `Master` ](xref:Xamarin.Forms.MasterDetailPage.Master) a [ `Detail` ](xref:Xamarin.Forms.MasterDetailPage.Detail). Obecně je každá z těchto vlastností lze nastavit `ContentPage`. `MasterDetailPage` Zobrazí a přepne mezi těmito dvěma stránkami.

Existují dva základní způsoby, jak přepínat mezi těmito dvěma stránkami:

- *Rozdělit* kde seznam a podrobnosti se vedle sebe
- *popover* kde stránce s podrobnostmi pokrývá nebo částečně pokrývá hlavní stránky

Existuje několik variant *popover* přístup (*snímku*, *překrývat*, a *prohození*), ale toto jsou obecně platformy závislé. Můžete nastavit [ `MasterDetailBehavior` ](xref:Xamarin.Forms.MasterDetailPage.MasterBehavior) vlastnost `MasterDetailPage` členovi [ `MasterBehavior` ](xref:Xamarin.Forms.MasterBehavior) výčtu:

- [`Default`](xref:Xamarin.Forms.MasterBehavior.Default)
- [`Split`](xref:Xamarin.Forms.MasterBehavior.Split)
- [`SplitOnLandscape`](xref:Xamarin.Forms.MasterBehavior.SplitOnLandscape)
- [`SplitOnPortrait`](xref:Xamarin.Forms.MasterBehavior.SplitOnPortrait)
- [`Popover`](xref:Xamarin.Forms.MasterBehavior.Popover)

Ale tato vlastnost nemá žádný vliv na telefonech. Telefony mají vždy popover chování. Rozdělení chování může mít pouze tablety a klasické pracovní plochy windows.

### <a name="exploring-the-behaviors"></a>Zkoumání chování

[ **MasterDetailBehaviors** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter25/MasterDetailBehaviors) ukázky můžete experimentovat s výchozím chováním na různých zařízeních. Program obsahuje dva samostatné `ContentPage` odvozené pro seznam a podrobnosti (s `Title` nastavenou u obou) a jiné třídy, která je odvozena od `MasterDetailPage` , který kombinuje je. Je ohraničen stránce s podrobnostmi `NavigationPage` protože UPW program nebude fungovat bez něho.

Platformy Windows 8.1 a Windows Phone 8.1 vyžadují, aby rastrový obrázek nastavena na `Icon` vlastnost stránky předlohy.

### <a name="back-to-school"></a>Zpět do školních Lavic

[ **SchoolAndDetail** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter25/SchoolAndDetail) ukázka přijímá trochu jiný přístup k vytváření program, který se zobrazí studenti z [ **SchoolOfFineArt** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/SchoolOfFineArt) knihovny.

`Master` a `Detail` vlastnosti jsou definované s vizuální stromové struktury v [SchoolAndDetailPage.xaml](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter25/SchoolAndDetail/SchoolAndDetail/SchoolAndDetail/SchoolAndDetailPage.xaml) soubor, který je odvozen od `MasterDetailPage`. Toto uspořádání umožňuje datové vazby k nastavení mezi stránkami a podrobností.

Také nastaví souboru XAML [ `IsPresented` ](xref:Xamarin.Forms.MasterDetailPage.IsPresented) vlastnost `MasterDetailPage` k `True`. To způsobí, že stránky předlohy, který se má zobrazit při spuštění; ve výchozím nastavení se zobrazí na stránce podrobností. [SchoolAndDetailPage.xaml.cs](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter25/SchoolAndDetail/SchoolAndDetail/SchoolAndDetail/SchoolAndDetailPage.xaml.cs) souboru sady `IsPresented` k `false` při výběru položky z `ListView` na hlavní stránce. Pak se zobrazí stránka podrobností:

[![Trojitá snímek obrazovky podrobností a školy](images/ch25fg09-small.png "stránky podrobností MasterDetailPage")](images/ch25fg09-large.png#lightbox "stránky podrobností MasterDetailPage")

### <a name="your-own-user-interface"></a>Vlastní uživatelské rozhraní

I když Xamarin.Forms poskytuje uživatelské rozhraní pro přepínání mezi zobrazením seznamu a podrobností, můžete zadat vlastní. Postup:

- Nastavte [ `IsGestureEnabled` ](xref:Xamarin.Forms.MasterDetailPage.IsGestureEnabled) vlastnost `false` zakázat potažení prstem
- Přepsat [ `ShouldShowToolbarButton` ](xref:Xamarin.Forms.MasterDetailPage.ShouldShowToolbarButton) metodu a vrátí `false` ke skrytí tlačítka panelu nástrojů na Windows 8.1 a Windows Phone 8.1.

Poté musíte zadat způsob, jak přepnout mezi stránkami a podrobností, tak jak je ukázáno podle [ **ColorsDetail** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter25/ColorsDetails) vzorku.

[ **MasterDetailTaps** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter25/MasterDetailTaps) ukázka znázorňuje použití jiný přístup `TapGestureRecognizer` na stránkách a podrobností.

## <a name="tabbedpage"></a>TabbedPage

[ `TabbedPage` ](xref:Xamarin.Forms.TabbedPage) Je kolekce stránek, které se dají přepínat pomocí karet. Se odvozuje od `MultiPage<Page>` a definuje žádné veřejné vlastnosti nebo metody své vlastní. [`MultiPage<T>`](xref:Xamarin.Forms.MultiPage`1), ale definovat vlastnost:

- [`Children`](xref:Xamarin.Forms.MultiPage`1.Children) vlastnost typu `IList<T>`

Vyplněním tohoto `Children` kolekce s objekty na stránce.

Další možností můžete zadat `TabbedPage` podobně jako podřízené prvky `ListView` pomocí tyto dvě vlastnosti, které automaticky generují s kartami stránek:

- [`ItemsSource`](xref:Xamarin.Forms.MultiPage`1.ItemsSource) typu `IEnumerable`
- [`ItemTemplate`](xref:Xamarin.Forms.MultiPage`1.ItemTemplate) typu `DataTemplate`

Tento přístup však nebude fungovat dobře v Iosu když kolekce obsahuje více než několik položek.

`MultiPage<T>` definuje dvě další vlastnosti, které vám umožňují udržovat přehled o stránku, která je aktuálně zobrazit:

- [`CurrentPage`](xref:Xamarin.Forms.MultiPage`1.CurrentPage) typu `T`odkazující na stránce
- [`SelectedItem`](xref:Xamarin.Forms.MultiPage`1.SelectedItem) typu `Object`odkazující na objekt v `ItemsSource` kolekce

`MultiPage<T>` Definuje také dvě události:

- [`PagesChanged`](xref:Xamarin.Forms.MultiPage`1.PagesChanged) Když `ItemsSource` změny kolekce
- [`CurrentPageChanged`](xref:Xamarin.Forms.MultiPage`1.CurrentPageChanged) Když se změní zobrazenou stránku

### <a name="discrete-tab-pages"></a>Stránky karet diskrétní

[ **DiscreteTabbedColors** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter25/DiscreteTabbedColors) vzorek se skládá ze tří s kartami stránek, které zobrazují barvy třemi různými způsoby. Každá karta je `ContentPage` odvozená díla a pak `TabbedPage` odvozených děl na základě [DiscreteTabbedColorsPage.xaml](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter25/DiscreteTabbedColors/DiscreteTabbedColors/DiscreteTabbedColors/DiscreteTabbedColorsPage.xaml) kombinuje tři stránky.

Pro každou stránku, která se zobrazí `TabbedPage`, `Title` vlastnost je nutné zadat text v kartě a Apple Store vyžaduje také použita ikona proto `Icon` je nastavena pro iOS:

[![Trojitá snímek diskrétní barvy na kartách](images/ch25fg13-small.png "TabbedPage")](images/ch25fg13-large.png#lightbox "TabbedPage")

[ **StudentNotes** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter25/StudentNotes) ukázka má Domovská stránka, která obsahuje seznam všech studentů. Student je klepnutí, tím přejde `TabbedPage` jejich odvozené části [ `StudentNotesDataPage` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter25/StudentNotes/StudentNotes/StudentNotes/StudentNotesDataPage.xaml), která zahrnuje tři `ContentPage` objekty v jeho vizuálního stromu, z nichž jeden umožňuje zadat několik poznámek pro studenty, že.

### <a name="using-an-itemtemplate"></a>Pomocí šablona ItemTemplate

[ **MultiTabbedColor** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter25/MultiTabbedColors) ukázkový používá [ `NamedColor` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/NamedColor.cs) třídy v [ **Xamarin.FormsBook.Toolkit** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit) knihovny. [MultiTabbedColorsPage.xaml](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter25/MultiTabbedColors/MultiTabbedColors/MultiTabbedColors/MultiTabbedColorsPage.xaml) souboru nastaví `DataTemplate` vlastnost `TabbedPage` vizuální strom začátek s `ContentPage` vazby na vlastnosti, která obsahuje `NamedColor` (včetně vazbu na `Title` vlastnost).

Je to ale problém v systému iOS. Je možné zobrazit pouze několik položek a neexistuje žádný dobrý způsob, jak uživatelům umožnit ikony.



## <a name="related-links"></a>Související odkazy

- [Kapitola 25 textu v plném znění (PDF)](https://download.xamarin.com/developer/xamarin-forms-book/XamarinFormsBook-Ch25-Apr2016.pdf)
- [Ukázky kapitoly 25](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter25)
- [Stránka hlavní podrobnosti](~/xamarin-forms/app-fundamentals/navigation/master-detail-page.md)
- [Stránka s kartami](~/xamarin-forms/app-fundamentals/navigation/tabbed-page.md)
