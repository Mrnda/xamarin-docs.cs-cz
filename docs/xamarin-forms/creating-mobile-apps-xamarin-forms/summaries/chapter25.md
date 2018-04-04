---
title: Shrnutí kapitoly 25. Stránka typy
ms.prod: xamarin
ms.technology: xamarin-forms
ms.assetid: D1D348F2-6A44-4781-ADCE-A0B7BB9AEF89
author: charlespetzold
ms.author: chape
ms.date: 11/07/2017
ms.openlocfilehash: 951ae41763d8338d5adf73fb46ebc6defa64f8f8
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/04/2018
---
# <a name="summary-of-chapter-25-page-varieties"></a>Shrnutí kapitoly 25. Stránka typy

Pokud jste se seznámili s dvě třídy, které jsou odvozeny od `Page`: `ContentPage` a `NavigationPage`. Tato kapitola uvede dvěma dalšími:

- [`MasterDetailPage`](https://developer.xamarin.com/api/type/Xamarin.Forms.MasterDetailPage/) spravuje dvě stránky, hlavní a podrobností
- [`TabbedPage`](https://developer.xamarin.com/api/type/Xamarin.Forms.TabbedPage/) spravuje více stránek podřízené přístupné prostřednictvím karet

Tyto typy stránka poskytuje sofistikovanější navigační možnosti, než `NavagationPage` popsané v [kapitoly 24. Stránka navigační](~/xamarin-forms/creating-mobile-apps-xamarin-forms/summaries/chapter24.md).

## <a name="master-and-detail"></a>Seznam a podrobnosti

[ `MasterDetailPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.MasterDetailPage/) Definuje dvě vlastnosti typu `Page`: [ `Master` ](https://developer.xamarin.com/api/property/Xamarin.Forms.MasterDetailPage.Master/) a [ `Detail` ](https://developer.xamarin.com/api/property/Xamarin.Forms.MasterDetailPage.Detail/). Obecně nastavíte každý z těchto vlastností `ContentPage`. `MasterDetailPage` Zobrazí a přepíná mezi těmito dvěma stránkami.

Existují dva základní způsoby, jak přepínat mezi těmito dvěma stránkami:

- *rozdělení* kde seznamu a podrobností jsou umístěny vedle sebe
- *popover* kde stránce podrobností popisuje nebo částečně popisuje hlavní stránky

Existuje několik variant *popover* přístup (*snímku*, *překrývat*, a *swap*), ale toto jsou obecně platformy závislé. Můžete nastavit [ `MasterDetailBehavior` ](https://developer.xamarin.com/api/property/Xamarin.Forms.MasterDetailPage.MasterBehavior/) vlastnost `MasterDetailPage` k členem [ `MasterBehavior` ](https://developer.xamarin.com/api/type/Xamarin.Forms.MasterBehavior/) výčtu:

- [`Default`](https://developer.xamarin.com/api/field/Xamarin.Forms.MasterBehavior.Default/)
- [`Split`](https://developer.xamarin.com/api/field/Xamarin.Forms.MasterBehavior.Split/)
- [`SplitOnLandscape`](https://developer.xamarin.com/api/field/Xamarin.Forms.MasterBehavior.SplitOnLandscape/)
- [`SplitOnPortrait`](https://developer.xamarin.com/api/field/Xamarin.Forms.MasterBehavior.SplitOnPortrait/)
- [`Popover`](https://developer.xamarin.com/api/field/Xamarin.Forms.MasterBehavior.Popover/)

Tuto vlastnost však nemá žádný vliv na telefonech. Telefony mít vždy popover chování. Rozdělení chování mohou obsahovat pouze tablety a plochy windows.

### <a name="exploring-the-behaviors"></a>Zkoumání chování

[ **MasterDetailBehaviors** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter25/MasterDetailBehaviors) ukázka můžete experimentovat s výchozí chování v různých zařízeních. Program obsahuje dva samostatné `ContentPage` odvozené konfigurace pro seznam a podrobnosti (s `Title` vlastnost nastavte u obou) a jiné třídy, která je odvozena z `MasterDetailPage` kombinuje je. Ohraničeno stránce podrobností `NavigationPage` protože UWP program nebude fungovat bez ní.

Platformy Windows 8.1 a Windows Phone 8.1 vyžadují, aby rastrový obrázek nastavena na `Icon` vlastnost stránky předlohy.

### <a name="back-to-school"></a>Zpět do školních Lavic

[ **SchoolAndDetail** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter25/SchoolAndDetail) Ukázka vytváření program, který chcete zobrazit studenty z trvat poněkud jiný přístup [ **SchoolOfFineArt** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/SchoolOfFineArt) knihovny.

`Master` a `Detail` vlastnosti jsou definované pomocí visual stromy v [SchoolAndDetailPage.xaml](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter25/SchoolAndDetail/SchoolAndDetail/SchoolAndDetail/SchoolAndDetailPage.xaml) souboru, který je odvozen od `MasterDetailPage`. Toto uspořádání umožňuje vazby dat mezi stránky seznamu a podrobností.

Také nastaví souboru XAML [ `IsPresented` ](https://developer.xamarin.com/api/property/Xamarin.Forms.MasterDetailPage.IsPresented/) vlastnost `MasterDetailPage` k `True`. To způsobí, že hlavní stránky, který se má zobrazit při spuštění; ve výchozím nastavení se zobrazí stránka podrobností. [SchoolAndDetailPage.xaml.cs](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter25/SchoolAndDetail/SchoolAndDetail/SchoolAndDetail/SchoolAndDetailPage.xaml.cs) souboru nastaví `IsPresented` k `false` když se vybere položka ze `ListView` na hlavní stránce. Stránka Podrobnosti se následně zobrazí:

[![Trojitá snímek obrazovky a podrobností školní](images/ch25fg09-small.png "stránku s podrobnostmi od MasterDetailPage")](images/ch25fg09-large.png#lightbox "stránku s podrobnostmi od MasterDetailPage")

### <a name="your-own-user-interface"></a>Vlastní uživatelské rozhraní

I když Xamarin.Forms poskytuje uživatelské rozhraní pro přepínání mezi zobrazení seznamu a podrobností, můžete zadat vlastní. Postupujte následovně:

- Nastavte [ `IsGestureEnabled` ](https://developer.xamarin.com/api/property/Xamarin.Forms.MasterDetailPage.IsGestureEnabled/) vlastnost `false` zakázat k načtení
- Přepsání [ `ShouldShowToolbarButton` ](https://developer.xamarin.com/api/member/Xamarin.Forms.MasterDetailPage.ShouldShowToolbarButton()/) metodu a vrátí `false` ke skrytí tlačítka panelu nástrojů na Windows 8.1 a Windows Phone 8.1.

Poté musíte zadat znamená přepnout mezi stránkami seznamu a podrobností, tak jak je předvedeno pomocí [ **ColorsDetail** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter25/ColorsDetails) ukázka.

[ **MasterDetailTaps** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter25/MasterDetailTaps) příklad znázorňuje jiný přístup pomocí `TapGestureRecognizer` na stránkách seznamu a podrobností.

## <a name="tabbedpage"></a>TabbedPage

[ `TabbedPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.TabbedPage/) Je kolekce stránek, které můžete přepnout mezi pomocí karty. Je odvozena z `MultiPage<Page>` a definuje žádné veřejné vlastnosti nebo metody své vlastní. [`MultiPage<T>`](https://developer.xamarin.com/api/type/Xamarin.Forms.MultiPage%3CT%3E/), ale taky definovat vlastnost:

- [`Children`](https://developer.xamarin.com/api/property/Xamarin.Forms.MultiPage%3CT%3E.Children/) vlastnost typu `IList<T>`

Vyplněním tohoto `Children` kolekce s objekty stránky.

Jiná možnost umožňuje definovat `TabbedPage` podobně jako podřízené objekty `ListView` pomocí tyto dvě vlastnosti, které automaticky generují stránky s kartami:

- [`ItemsSource`](https://developer.xamarin.com/api/property/Xamarin.Forms.MultiPage%3CT%3E.ItemsSource/) typu `IEnumerable`
- [`ItemTemplate`](https://developer.xamarin.com/api/property/Xamarin.Forms.MultiPage%3CT%3E.ItemTemplate/) typu `DataTemplate`

Tento přístup však nebude fungovat i na iOS když kolekce obsahuje větší počet položek.

`MultiPage<T>` definuje dva další vlastnosti, které umožňují udržování přehledu o stránku, která je aktuálně zobrazit:

- [`CurrentPage`](https://developer.xamarin.com/api/property/Xamarin.Forms.MultiPage%3CT%3E.CurrentPage/) typu `T`odkazující na stránce
- [`SelectedItem`](https://developer.xamarin.com/api/property/Xamarin.Forms.MultiPage%3CT%3E.SelectedItem/) typu `Object`odkazující na objekt v `ItemsSource` kolekce

`MultiPage<T>` také definuje dvě události:

- [`PagesChanged`](https://developer.xamarin.com/api/event/Xamarin.Forms.MultiPage%3CT%3E.PagesChanged/) Když `ItemsSource` změny kolekce
- [`CurrentPageChanged`](https://developer.xamarin.com/api/event/Xamarin.Forms.MultiPage%3CT%3E.CurrentPageChanged/) Když se změní zobrazenou stránku

### <a name="discrete-tab-pages"></a>Stránky karet diskrétní

[ **DiscreteTabbedColors** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter25/DiscreteTabbedColors) ukázka se skládá ze tří záložkách stránek, které zobrazují barvy třemi různými způsoby. Je každé kartě `ContentPage` odvozených a potom `TabbedPage` odvozených [DiscreteTabbedColorsPage.xaml](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter25/DiscreteTabbedColors/DiscreteTabbedColors/DiscreteTabbedColors/DiscreteTabbedColorsPage.xaml) kombinuje tři stránky.

Pro jednotlivé stránky, které se zobrazí v `TabbedPage`, `Title` vlastnost je nutný zadejte text, na kartě, a Apple Store taky použít ikonu proto `Icon` je nastavena pro iOS:

[![Trojitá snímek obrazovky diskrétní barvy – záložkami](images/ch25fg13-small.png "TabbedPage")](images/ch25fg13-large.png#lightbox "TabbedPage")

[ **StudentNotes** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter25/StudentNotes) ukázka má domovskou stránku, která obsahuje seznam všech studentů. Po klepnutí student přejde na `TabbedPage` odvozených, [ `StudentNotesDataPage` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter25/StudentNotes/StudentNotes/StudentNotes/StudentNotesDataPage.xaml), který zahrnuje tři `ContentPage` objekty v jeho vizuálním stromu, díky čemuž zadání tohoto student některé poznámky.

### <a name="using-an-itemtemplate"></a>Pomocí ItemTemplate

[ **MultiTabbedColor** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter25/MultiTabbedColors) ukázkové používá [ `NamedColor` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/NamedColor.cs) třídy v [ **Xamarin.FormsBook.Toolkit** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit) knihovny. [MultiTabbedColorsPage.xaml](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter25/MultiTabbedColors/MultiTabbedColors/MultiTabbedColors/MultiTabbedColorsPage.xaml) souboru nastaví `DataTemplate` vlastnost `TabbedPage` začátku vizuálním stromu s `ContentPage` vazby na vlastnosti, která obsahuje `NamedColor` (včetně vazbu ke `Title` vlastnost).

To je však problematické v systému iOS. Zobrazit lze pouze několik položek a neexistuje žádný funkční způsob jim dát ikony.



## <a name="related-links"></a>Související odkazy

- [Úplný text kapitoly 25 (PDF)](https://download.xamarin.com/developer/xamarin-forms-book/XamarinFormsBook-Ch25-Apr2016.pdf)
- [Ukázky kapitoly 25](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter25)
- [Stránka podrobností](~/xamarin-forms/app-fundamentals/navigation/master-detail-page.md)
- [Na kartách stránky](~/xamarin-forms/app-fundamentals/navigation/tabbed-page.md)
