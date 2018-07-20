---
title: Souhrn kapitole 19. Zobrazení kolekcí
description: 'Vytváření mobilních aplikací s Xamarin.Forms: Souhrn kapitole 19. Zobrazení kolekcí'
ms.prod: xamarin
ms.technology: xamarin-forms
ms.assetid: 0AEC3A5C-586E-4D0F-9895-67E99A053A79
author: charlespetzold
ms.author: chape
ms.date: 07/18/2018
ms.openlocfilehash: 01985cf253c0f33c52128386b36c11af50381ee1
ms.sourcegitcommit: 8555a4dd1a579b2206f86c867125ee20fbc3d264
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/19/2018
ms.locfileid: "39156681"
---
# <a name="summary-of-chapter-19-collection-views"></a>Souhrn kapitole 19. Zobrazení kolekcí

> [!NOTE] 
> Poznámky na této stránce označit oblasti, kde se Xamarin.Forms se rozcházela z materiály uvedené v seznamu.

Xamarin.Forms definuje tři zobrazení, které udržují kolekce a zobrazit jejich prvky:

- [`Picker`](xref:Xamarin.Forms.Picker) je poměrně krátké seznam položek řetězec, který umožňuje uživateli vybrat jednu
- [`ListView`](xref:Xamarin.Forms.ListView) často je dlouhý seznam položek obvykle stejného typu a formátování, také umožňuje uživateli vybrat jednu
- [`TableView`](xref:Xamarin.Forms.TableView) je kolekce *buňky* (obvykle o různých typech videí a vzhled) k zobrazení dat nebo spravovat uživatelský vstup

Je běžné pro použití aplikacemi MVVM `ListView` zobrazíte Vybrat kolekci objektů.

## <a name="program-options-with-picker"></a>Některé z možností programu s výběr

[ `Picker` ](xref:Xamarin.Forms.Picker) Je dobrou volbou, když budete chtít umožnit uživateli vybrat možnost z poměrně krátké seznam `string` položky.

### <a name="the-picker-and-event-handling"></a>Výběr a zpracování událostí

[ **PickerDemo** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter19/PickerDemo) Ukázka předvádí, jak nastavit pomocí XAML `Picker` [ `Title` ](xref:Xamarin.Forms.Picker.Title) vlastnost a přidejte `string` položky, které chcete [ `Items` ](xref:Xamarin.Forms.Picker.Items) kolekce. Když uživatel vybere `Picker`, zobrazuje položky v `Items` kolekce v podobě závislého na platformě.

[ `SelectedIndexChanged` ](xref:Xamarin.Forms.Picker.SelectedIndexChanged) Událost označuje, kdy uživatel vybral položky. Založený na nule [ `SelectedIndex` ](xref:Xamarin.Forms.Picker.SelectedIndex) vlastnost pak označuje vybranou položku. Pokud není vybrána žádná položka `SelectedIndex` rovná &ndash;1.

Můžete také použít `SelectedIndex` inicializovat vybrané položky, ale musí být nastavena po `Items` naplní kolekci. V XAML, to znamená, že budete pravděpodobně používat prvek vlastnosti nastavit `SelectedIndex`.

### <a name="data-binding-the-picker"></a>Datové vazby při výběru

`SelectedIndex` Vlastnost je založená na vlastnost s vazbou, ale `Items` není, takže použití datových vazeb s `Picker` je obtížné. Jedním řešením je použít `Picker` v kombinaci s [ `ObjectToIndexConverter` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/ObjectToIndexConverter.cs) například podle návodu na [ **Xamarin.FormsBook.Toolkit** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit) knihovny. [ **PickerBinding** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter19/PickerBinding) ukazuje, jak to funguje.

> [!NOTE] 
> Xamarin.Forms `Picker` teď zahrnuje `ItemsSource` a `SelectedItem` vlastnosti, které podporují vytváření datových vazeb. Zobrazit [výběr](~/xamarin-forms/user-interface/picker/index.md).

## <a name="rendering-data-with-listview"></a>Vykreslování dat pomocí ListView

[ `ListView` ](xref:Xamarin.Forms.ListView) Je jediná třída, která je odvozena z [ `ItemsView<TVisual>` ](xref:Xamarin.Forms.ItemsView`1) ze kterého se dědí [ `ItemsSource` ](xref:Xamarin.Forms.ItemsView`1.ItemsSource) a [ `ItemTemplate` ](xref:Xamarin.Forms.ItemsView`1.ItemTemplate) vlastnosti.

`ItemsSource` je typu `IEnumerable` je ale `null` ve výchozím nastavení a musí být explicitně inicializován nebo do kolekce (častěji) nastavit prostřednictvím datové vazby. Položky v této kolekci mohou být libovolného typu.

`ListView` definuje [ `SelectedItem` ](xref:Xamarin.Forms.ListView.SelectedItem) vlastnost, která je nastavena na jednu z položek v `ItemsSource` kolekce nebo `null` Pokud není vybrána žádná položka. `ListView` je aktivována [ `ItemSelected` ](xref:Xamarin.Forms.ListView.ItemSelected) událost v případě, že je vybraná nová položka.

### <a name="collections-and-selections"></a>Kolekce a výběr

[ **ListViewList** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter19/ListViewList) vzorek výplně `ListView` s 17 `Color` hodnoty v `List<Color>` kolekce. Položky jsou volitelné, ale ve výchozím nastavení se zobrazí s jejich zkuste `ToString` reprezentace. Několik příkladů v této kapitole ukazují, jak opravit, které zobrazují a nastavte ji jako atraktivní podle potřeby.

### <a name="the-row-separator"></a>Oddělovač řádků

V Iosu a Androidu zobrazí odděluje dynamického zajišťování řádku řádky. Můžete řídit pomocí [ `SeparatorVisibiliy` ](xref:Xamarin.Forms.ListView.SeparatorVisibility) a [ `SeparatorColor` ](xref:Xamarin.Forms.ListView.SeparatorColor) vlastnosti. `SeparatorVisibility` Vlastnost je typu [ `SeparatorVisbility` ](xref:Xamarin.Forms.SeparatorVisibility), výčet se dvěma členy:

- [`Default`](xref:Xamarin.Forms.SeparatorVisibility.Default), ve výchozím nastavení
- [`None`](xref:Xamarin.Forms.SeparatorVisibility.None)

### <a name="data-binding-the-selected-item"></a>Datové vazby na vybranou položku

`SelectedItem` Vlastnost tímto modulem stojí umožňujících vazbu vlastnosti, tak může být buď zdroj nebo cíl datovou vazbu. Výchozí `BindingMode` je `OneWayToSource`, ale obvykle je cílem obousměrný datové vazby, zejména ve scénářích MVVM. [ **ListViewArray** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter19/ListViewArray) ukázka demonstruje tento typ vazby.

### <a name="the-observablecollection-difference"></a>Rozdíl kolekci ObservableCollection

[ **ListViewLogger** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter19/ListViewLogger) ukázkové sady `ItemsSource` vlastnost `ListView` k `List<DateTime>` kolekce a pak postupně přidá nový `DateTime` do kolekce každou sekundu, které pomocí časovače.

Však `ListView` neaktualizuje automaticky sama vzhledem k tomu, `List<T>` kolekce nemá mechanismus oznámení označíte, když jsou položky přidán či odebrán z kolekce.

Je mnohem lepší třídu použít v těchto scénářích [ `ObservableCollection<T>` ](xref:System.Collections.ObjectModel.ObservableCollection`1) definované v `System.Collections.ObjectModel` oboru názvů. Tato třída implementuje [ `INotifyCollectionChanged` ](xref:System.Collections.Specialized.INotifyCollectionChanged) rozhraní a proto je aktivována [ `CollectionChanged` ](xref:System.Collections.ObjectModel.ObservableCollection`1.CollectionChanged) událost, když jsou položky přidán či odebrán z kolekce, nebo když jsou nahrazeny nebo přesunuta v rámci kolekce. Při `ListView` interně zjistí, že třída implementace `INotifyCollectionChanged` byla nastavena na jeho `ItemsSource` vlastnost, připojí obslužnou rutinu pro `CollectionChanged` událostí a aktualizuje jeho zobrazení, pokud se změní kolekce.

[ **ObservableLogger** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter19/ObservableLogger) ukázka demonstruje použití `ObservableCollection`.

### <a name="templates-and-cells"></a>Šablony a buňky

Ve výchozím nastavení `ListView` zobrazí položky v jeho kolekce pomocí každou položku `ToString` metody. Lepším řešením je potřeba definovat šablony pro zobrazení položek.

Můžete experimentovat s touto funkcí, můžete použít [ `NamedColor` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/NamedColor.cs) třídy v [ **Xamarin.FormsBook.Toolkit** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit) knihovny. Tato třída definuje statickou `All` vlastnost typu `IList<NamedColor>` , který obsahuje 141 `NamedColor` objekty odpovídající veřejné pole `Color` struktury.

[ **NaiveNamedColorList** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter19/NaiveNamedColorList) ukázkové sady `ItemsSource` z `ListView` této `NamedColor.All` vlastnost, ale pouze třídy plně kvalifikované názvy `NamedColor` jsou objekty zobrazí.

`ListView` potřebuje šablonu pro zobrazení těchto položek. V kódu, můžete nastavit [ `ItemTemplate` ](xref:Xamarin.Forms.ItemsView`1.ItemTemplate) vlastnosti určené `ItemsView<TVisual>` k [ `DataTemplate` ](xref:Xamarin.Forms.DataTemplate) pomocí [ `DataTemplate` konstruktor](xref:Xamarin.Forms.DataTemplate.%23ctor(System.Type)) , který odkazuje na jejich odvozené z [ `Cell` ](xref:Xamarin.Forms.Cell) třídy. `Cell` má pět prvků odvozených:

- [`TextCell`](xref:Xamarin.Forms.TextCell) &mdash; obsahuje dva `Label` zobrazení (koncepčně mluvený)
- [`ImageCell`](xref:Xamarin.Forms.ImageCell) &mdash; Přidá `Image` se můžete podívat na `TextCell`
- [`EntryCell`](xref:Xamarin.Forms.EntryCell) &mdash; obsahuje `Entry` zobrazit se `Label`
- [`SwitchCell`](xref:Xamarin.Forms.SwitchCell) &mdash; obsahuje `Switch` s `Label`
- [`ViewCell`](xref:Xamarin.Forms.ViewCell) &mdash; může být kterýkoli `View` (pravděpodobně s podřízených objektů)

Poté zavolejte [ `SetValue` ](xref:Xamarin.Forms.DataTemplate.SetValue(Xamarin.Forms.BindableProperty,System.Object)) a [ `SetBinding` ](xref:Xamarin.Forms.DataTemplate.SetBinding(Xamarin.Forms.BindableProperty,Xamarin.Forms.BindingBase)) na `DataTemplate` objektu pro přiřazení hodnoty `Cell` vlastnosti, nebo k nastavení datové vazby na `Cell` odkazování na vlastnosti položek ve vlastnosti `ItemsSource` kolekce. To je patrné [ **TextCellListCode** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter19/TextCellListCode) vzorku.

Protože každá položka je zobrazena ve `ListView`malé vizuálního stromu je vytvořen z šablony a datové vazby se vytvoří mezi položky a vlastnosti prvků v tomto vizuálním stromu. Můžete získat představu o tomto procesu nainstalováním obslužné rutiny pro [ `ItemAppearing` ](xref:Xamarin.Forms.ListView.ItemAppearing) a [ `ItemDisappearing` ](xref:Xamarin.Forms.ListView.ItemDisappearing) události `ListView`, nebo pomocí alternativu [ `DataTemplate`konstruktor](xref:Xamarin.Forms.DataTemplate.%23ctor(System.Func{System.Object})) , který používá funkce, která je volána pokaždé, když vizuálního stromu. položky se musí vytvořit.

[ **TextCellListXaml** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter19/TextCellListXaml) znázorňuje funkčně stejný jako program zcela v XAML. A `DataTemplate` značka je nastavená na `ItemTemplate` vlastnost `ListView`a pak `TextCell` je nastavena na `DataTemplate`. Vazby na vlastnosti položek v kolekci jsou nastavena přímo [ `Text` ](xref:Xamarin.Forms.TextCell.Text) a [ `Detail` ](xref:Xamarin.Forms.TextCell.Detail) vlastnosti `TextCell`.

### <a name="custom-cells"></a>Vlastní buňky

V XAML je možné nastavit [ `ViewCell` ](xref:Xamarin.Forms.ViewCell) k `DataTemplate` a pak definovat vlastní vizuálního stromu, jako [ `View` ](xref:Xamarin.Forms.ViewCell.View) vlastnost `ViewCell`. (`View` je vlastnost content `ViewCell` proto `ViewCell.View` značky nejsou povinné.) [ **CustomNamedColorList** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter19/CustomNamedColorList) ukázka demonstruje tento postup:

[![Trojitá snímek obrazovky seznam s názvem barev vlastní](images/ch19fg11-small.png "vlastní seznam barev s názvem")](images/ch19fg11-large.png#lightbox "vlastní s názvem seznam barev")

Získání nastavení velikosti pro všechny platformy může být velmi obtížné. [ `RowHeight` ](xref:Xamarin.Forms.ListView.RowHeight) Vlastnost je užitečná, ale v některých případech budete chtít uchýlíte k [ `HasUnevenRows` ](xref:Xamarin.Forms.ListView.HasUnevenRows) vlastnost, která je méně efektivní, ale sil `ListView` pro nastavení velikosti řádků. Pro iOS a Android musíte použít jednu z těchto dvou vlastností se získat nastavení velikosti řádku správné.

### <a name="grouping-the-listview-items"></a>Seskupení položek ListView

`ListView` podporuje seskupování položek a procházení mezi těchto skupin. `ItemsSource` Do kolekce kolekcí musí být nastavena vlastnost: objekt, který `ItemsSource` je nastavena na musí implementovat `IEnumerable`, a každá položka v kolekci musí také implementovat `IEnumerable`. Každá skupina by měla obsahovat dvě vlastnosti: textový popis skupiny a dvoupísmennou zkratku.

[ `NamedColorGroup` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/NamedColorGroup.cs) Třídy v [ **Xamarin.FormsBook.Toolkit** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit) knihovny vytvoří sedm skupiny `NamedColor` objekty. [ **ColorGroupList** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter19/ColorGroupList) příklad ukazuje, jak používat tyto skupiny se [ `IsGroupingEnabled` ](xref:Xamarin.Forms.ListView.IsGroupingEnabled) vlastnost `ListView` nastavena na `true`a [ `GroupDisplayBinding` ](xref:Xamarin.Forms.ListView.GroupDisplayBinding) a [ `GroupShortNameBinding` ](xref:Xamarin.Forms.ListView.GroupShortNameBinding) vlastnosti vázané na vlastnosti v každé skupině.

### <a name="custom-group-headers"></a>Vlastní skupiny záhlaví

Je možné vytvořit vlastní hlavičky pro `ListView` skupiny tak, že nahradíte `GroupDisplayBinding` vlastnost s [ `GroupHeaderTemplate` ](xref:Xamarin.Forms.ListView.GroupHeaderTemplate) definice šablony pro záhlaví.

### <a name="listview-and-interactivity"></a>ListView a interaktivita

Obecná aplikace získá interakce uživatele s `ListView` připojením obslužná rutina `ItemSelected` nebo [ `ItemTapped` ](xref:Xamarin.Forms.ListView.ItemTapped) události, nebo nastavením datové vazby na `SelectedItem` vlastnost. Ale některé typy buňky (`EntryCell` a `SwitchCell`) umožňují interakci s uživatelem, a je také možné vytvořit vlastní buňky, že sami komunikovat s uživatelem. [ **InteractiveListView** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter19/InteractiveListView) vytvoří 100 instancí [ `ColorViewModel` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/ColorViewModel.cs) a umožňuje uživatelům změnit každá barva pomocí trojice `Slider` elementy. Program také využívá [ `ColorToContrastColorConverter` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/ColorToContrastColorConverter.cs) v [ **Xamarin.FormsBook.Toolkit**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit).

## <a name="listview-and-mvvm"></a>ListView a rozhraní MVVM

`ListView` hraje důležitou roli ve scénářích MVVM. Pokaždé, když `IEnumerable` kolekce existuje v ViewModel, často je vázán na `ListView`. Také implementovat položek v kolekci často `INotifyPropertyChanged` má být svázána s vlastností v šabloně.

### <a name="a-collection-of-viewmodels"></a>Kolekce modely ViewModels

K prozkoumání, [ **SchoolOfFineArts** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/SchoolOfFineArt) knihovny vytvoří několik tříd na základě [datový soubor XML](http://xamarin.github.io/xamarin-forms-book-samples/SchoolOfFineArt/students.xml) a obrázky fiktivní studentům, kteří tuto fiktivní školu.

[ `Student` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/SchoolOfFineArt/SchoolOfFineArt/Student.cs) Třída odvozena z [ `ViewModelBase` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/SchoolOfFineArt/SchoolOfFineArt/ViewModelBase.cs). [ `StudentBody` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/SchoolOfFineArt/SchoolOfFineArt/StudentBody.cs) Třídy je kolekce `Student` objekty a také se odvozuje od `ViewModelBase`. [ `SchoolViewModel` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/SchoolOfFineArt/SchoolOfFineArt/SchoolViewModel.cs) Stáhne soubor XML a sestavuje všechny objekty.

[ **StudentList** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter19/StudentList) program používá `ImageCell` zobrazíte na studentů a jejich obrázků v `ListView`:

[![Trojitá snímek Student](images/ch19fg18-small.png "seznamu studentů")](images/ch19fg18-large.png#lightbox "seznamu studentů")

[ **ListViewHeader** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter19/ListViewHeader) ukázka přidá [ `Header` ](xref:Xamarin.Forms.ListView.Header) vlastností, ale zobrazí jen v systému Android.

### <a name="selection-and-the-binding-context"></a>Výběr a kontextu vazby

[ **SelectedStudentDetail** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter19/SelectedStudentDetail) program vytvoří vazbu `BindingContext` z `StackLayout` k `SelectedItem` vlastnost `ListView`. To umožňuje aplikaci zobrazíte podrobné informace o vybrané studentů.

### <a name="context-menus"></a>Kontextové nabídky

Buňky můžete definovat kontextové nabídky, která je implementována v podobě specifické pro platformu. Chcete-li vytvořit tuto nabídku, přidejte [ `MenuItem` ](xref:Xamarin.Forms.MenuItem) objektů [ `ContextActions` ](xref:Xamarin.Forms.Cell.ContextActions) vlastnost `Cell`.

`MenuItem` definuje vlastnosti pět:

- [`Text`](xref:Xamarin.Forms.MenuItem.Text) typu `string`
- [`Icon`](xref:Xamarin.Forms.MenuItem.Icon) typu `FileImageSource`
- [`IsDestructive`](xref:Xamarin.Forms.MenuItem.IsDestructive) typu `bool`
- [`Command`](xref:Xamarin.Forms.MenuItem.Command) typu `ICommand`
- [`CommandParameter`](xref:Xamarin.Forms.MenuItem.CommandParameter) typu `object`

`Command` a `CommandParameter` vlastnosti znamenají, že ViewModel pro každou položku obsahuje metody k provedení požadované příkazy. Ve scénářích bez MVVM `MenuItem` také definuje [ `Clicked` ](xref:Xamarin.Forms.MenuItem.Clicked) událostí.

[ **CellContextMenu** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter19/CellContextMenu) ukazuje tento postup. `Command` Vlastnosti každého `MenuItem` je vázán na vlastnost typu `ICommand` v `Student` třídy. Nastavte `IsDestructive` vlastnost `true` pro `MenuItem` , odebere nebo odstraní vybraný objekt.

### <a name="varying-the-visuals"></a>Různé vizuály

Někdy je vhodné malé odchylky v vizuály položek `ListView` na základě vlastnosti. Například když student získal na podnikové úrovni – průměr klesne pod 2.0, [ **ColorCodedStudents** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter19/ColorCodedStudents) ukázka zobrazí název tohoto studenta červeně.
To lze provést pomocí převaděč hodnot vazby [ `ThresholdToObjectConverter` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/ThresholdToObjectConverter.cs)v [ **Xamarin.FormsBook.Toolkit** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit) knihovny.

### <a name="refreshing-the-content"></a>Aktualizace obsahu

`ListView` Podporuje rozevírací gesta pro aktualizaci svá data. Program musí nastavit [ `IsPullToRefresh` ](xref:Xamarin.Forms.ListView.IsPullToRefreshEnabled) vlastnost `true` aby to bylo. `ListView` Reaguje na gesta rozevírací nastavením jeho [ `IsRefreshing` ](xref:Xamarin.Forms.ListView.IsRefreshing) vlastnost `true`a zobrazením [ `Refreshing` ](xref:Xamarin.Forms.ListView.Refreshing) události a (pro scénáře MVVM) volání `Execute` metodu jeho [ `RefreshCommand` ](xref:Xamarin.Forms.ListView.RefreshCommand) vlastnost.

Kód zpracování `Refresh` události nebo `RefreshCommand` pravděpodobně aktualizace dat zobrazených ve `ListView` a nastaví `IsRefreshing` zpět `false`.

[ **RssFeed** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter19/RssFeed) ukázka znázorňuje použití [ `RssFeedViewModel` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter19/RssFeed/RssFeed/RssFeed/RssFeedViewModel.cs) , který implementuje `RefreshCommand` a `IsRefreshing` vlastnosti pro datovou vazbu.

## <a name="the-tableview-and-its-intents"></a>Zobrazení Tabulka a její záměrů

Zatímco `ListView` obecně zobrazí více instancí stejného typu, [ `TableView` ](xref:Xamarin.Forms.TableView) se obvykle zaměřuje na poskytování uživatelského rozhraní pro více vlastností různých typů. Každá položka je přidružena k vlastní [ `Cell` ](xref:Xamarin.Forms.Cell) odvozené pro zobrazení vlastnosti nebo poskytnutí uživatelského rozhraní na ni.

### <a name="properties-and-hierarchies"></a>Vlastnosti a hierarchie

`TableView` definuje pouze čtyři vlastnosti:

- [`Intent`](xref:Xamarin.Forms.TableView.Intent) typu [ `TableIntent` ](xref:Xamarin.Forms.TableIntent), výčet
- [`Root`](xref:Xamarin.Forms.TableView.Root) typu [ `TableRoot` ](xref:Xamarin.Forms.TableRoot), vlastnost obsahu `TableView`
- [`RowHeight`](xref:Xamarin.Forms.TableView.RowHeight) typu `int`
- [`HasUnevenRows`](xref:Xamarin.Forms.TableView.HasUnevenRows) typu `bool`

`TableIntent` Výčet Určuje, jak máte v úmyslu použít `TableView`:

- [`Data`](xref:Xamarin.Forms.TableIntent.Data)
- [`Form`](xref:Xamarin.Forms.TableIntent.Form)
- [`Settings`](xref:Xamarin.Forms.TableIntent.Settings)
- [`Menu`](xref:Xamarin.Forms.TableIntent.Menu)

Tito členové také navrhnout využijete například `TableView`.

Několik jiných tříd jsou součástí definice tabulky:

- [`TableSectionBase`](xref:Xamarin.Forms.TableSectionBase) je abstraktní třída, která je odvozena z `BindableObject` a definuje [ `Title` ](xref:Xamarin.Forms.TableSectionBase.Title) vlastnost

- [`TableSectionBase<T>`](xref:Xamarin.Forms.TableSectionBase`1) je abstraktní třída, která je odvozena z `TableSectionBase` a implementuje `IList<T>` a `INotifyCollectionChanged`

- [`TableSection`](xref:Xamarin.Forms.TableSection) je odvozen od `TableSectionBase<Cell>`

- [`TableRoot`](xref:Xamarin.Forms.TableRoot) je odvozen od `TableSectionBase<TableSection>`

Stručně řečeno `TableView` má `Root` vlastnost, která nastavíte, a `TableRoot` objekt, což je kolekce z `TableSection` objektů, z nichž každý je kolekce `Cell` objekty. Má tabulka více oddílů a každý oddíl obsahuje více buněk. Samotnou tabulku může mít název, a každý oddíl může obsahovat název. I když `TableView` využívá `Cell` vy, neprovede využívání `DataTemplate`.

### <a name="a-prosaic-form"></a>Prosaic formuláře

[ **EntryForm** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter19/EntryForm) ukázka definuje [ `PersonalInformation` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter19/EntryForm/EntryForm/EntryForm/PersonalInformation.cs) model zobrazení, bude instance z nich `BindingContext` z `TableView`. Každý `Cell` odvozená díla v jeho `TableSection` potom může mít vazby na vlastnosti `PersonalInformation` třídy.

### <a name="custom-cells"></a>Vlastní buňky

[ **ConditionalCells** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter19/ConditionalCells) ukázka rozšiřuje **EntryForm**. [ `ProgrammerInformation` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter19/EntryForm/EntryForm/EntryForm/PersonalInformation.cs) Třída zahrnuje logická vlastnost, která řídí použitelnosti dvě další vlastnosti. Pro tyto dvě další vlastnosti, program používá vlastní `PickerCell` na základě [PickerCell.xaml](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/PickerCell.xaml) a [PickerCell.xaml.cs](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/PickerCell.xaml.cs) v [  **Xamarin.FormsBook.Toolkit** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit) knihovny.

I když `IsEnabled` vlastnosti z nich `PickerCell` prvky, které jsou vázány na logickou vlastnost `ProgrammerInformation`, tento postup nefunguje pro práci, který vyzve bude další vzorek.

### <a name="conditional-sections"></a>Podmíněné sekce

[ **ConditionalSection** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter19/ConditionalSection) ukázka vloží dvě položky, které jsou podmíněná na výběr logická položky v samostatném `TableSection`. Soubor kódu na pozadí odebere z této části `TableView` nebo přidá na základě zpět na vlastnost typu Boolean.

### <a name="a-tableview-menu"></a>Nabídka zobrazení Tabulka

Další používání `TableView` je nabídka. [ **MenuCommands** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter19/MenuCommands) ukázce nabídky, která umožňuje přechod trochu `BoxView` po obrazovce.



## <a name="related-links"></a>Související odkazy

- [Kapitola 19 textu v plném znění (PDF)](https://download.xamarin.com/developer/xamarin-forms-book/XamarinFormsBook-Ch19-Apr2016.pdf)
- [Ukázky kapitole 19](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter19)
- [Výběr](~/xamarin-forms/user-interface/picker/index.md)
- [ListView](~/xamarin-forms/user-interface/listview/index.md)
- [TableView](~/xamarin-forms/user-interface/tableview.md)
