---
title: "Shrnutí kapitoly 19. Zobrazení kolekce"
ms.topic: article
ms.prod: xamarin
ms.technology: xamarin-forms
author: charlespetzold
ms.author: chape
ms.date: 11/07/2017
ms.openlocfilehash: 37afa3a54fd20745a65312fb5a24d958c8ec405f
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: HT
ms.contentlocale: cs-CZ
ms.lasthandoff: 02/27/2018
---
# <a name="summary-of-chapter-19-collection-views"></a>Shrnutí kapitoly 19. Zobrazení kolekce

Xamarin.Forms definuje tři zobrazení, které udržují kolekce a zobrazit jejich elementů:

- [`Picker`](https://developer.xamarin.com/api/type/Xamarin.Forms.Picker/) je poměrně krátké seznam položek řetězec, který umožňuje uživateli vybrat jednu
- [`ListView`](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/) často je dlouhý seznam položek obvykle stejného typu a formátování, také umožňuje uživateli vybrat jednu
- [`TableView`](https://developer.xamarin.com/api/type/Xamarin.Forms.TableView/) je kolekce *buněk* (obvykle z různých typů a vzhled) k zobrazení dat nebo spravovat uživatelský vstup

Je běžné pro použití aplikacemi modelem MVVM `ListView` zobrazíte kolekci objektů, které lze vybírat.

## <a name="program-options-with-picker"></a>Možnosti programu s výběr.

[ `Picker` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Picker/) Je vhodné použít, když potřebujete umožnit uživatelům zvolit možnost některé z těchto poměrně krátké seznam `string` položky.

### <a name="the-picker-and-event-handling"></a>Výběr a zpracování událostí

[ **PickerDemo** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter19/PickerDemo) ukázka ukazuje, jak můžete nastavit pomocí XAML `Picker` [ `Title` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Picker.Title/) vlastnost a přidejte `string` položek [ `Items` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Picker.Items/) kolekce. Když uživatel vybere `Picker`, se zobrazí položky v `Items` kolekce způsobem závislé na platformu.

[ `SelectedIndexChanged` ](https://developer.xamarin.com/api/event/Xamarin.Forms.Picker.SelectedIndexChanged/) Událost označuje, když má uživatel vybrané položky. Nule [ `SelectedIndex` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Picker.SelectedIndex/) vlastnost pak označuje vybranou položku. Pokud není vybrána žádná položka, `SelectedIndex` rovná &#x2013; 1.

Můžete také použít `SelectedIndex` k chybě při inicializaci vybranou položku, ale musí být nastavena po `Items` naplní kolekce. V jazyce XAML, to znamená, že budete pravděpodobně používat vlastnost element nastavit `SelectedIndex`.

### <a name="data-binding-the-picker"></a>Datové vazby nástroje pro výběr

`SelectedIndex` Vlastnost je zálohovaný díky vazbu vlastnosti ale `Items` není, takže použití datových vazeb s `Picker` je obtížné. Jeden řešením je použití `Picker` v kombinaci s [ `ObjectToIndexConverter` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/ObjectToIndexConverter.cs) například ve [ **Xamarin.FormsBook.Toolkit** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit) knihovny. [ **PickerBinding** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter19/PickerBinding) ukazuje, jak to funguje.

## <a name="rendering-data-with-listview"></a>Generování dat s ListView

[ `ListView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/) Je jediná třída, která je odvozena z [ `ItemsView<TVisual>` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ItemsView%3CTVisual%3E/) z který dědí [ `ItemsSource` ](https://developer.xamarin.com/api/property/Xamarin.Forms.ItemsView%3CTVisual%3E.ItemsSource/) a [ `ItemTemplate` ](https://developer.xamarin.com/api/property/Xamarin.Forms.ItemsView%3CTVisual%3E.ItemTemplate/) vlastnosti.

`ItemsSource` je typu `IEnumerable` , ale je `null` ve výchozím nastavení a musí být explicitně inicializována nebo (běžně) nastavit do kolekce pomocí datová vazba. Položky v této kolekci můžou být jakéhokoli typu.

`ListView` definuje [ `SelectedItem` ](https://developer.xamarin.com/api/property/Xamarin.Forms.ListView.SelectedItem/) vlastnost, která je buď nastavte na některou z položek v `ItemsSource` kolekce nebo `null` Pokud není vybrána žádná položka. `ListView` Aktivuje se [ `ItemSelected` ](https://developer.xamarin.com/api/event/Xamarin.Forms.ListView.ItemSelected/) událost, pokud je vybrána novou položku.

### <a name="collections-and-selections"></a>Kolekce a výběry

[ **ListViewList** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter19/ListViewList) ukázkové výplněmi `ListView` s 17 `Color` hodnoty ve `List<Color>` kolekce. Položky jsou volitelný, ale ve výchozím nastavení jsou zobrazeny s jejich rozdělení nežádoucí `ToString` reprezentace. Několik příkladů v této kapitole ukazují, jak opravit zobrazované a nastavit jej jako atraktivní podle potřeby.

### <a name="the-row-separator"></a>Oddělovač řádků

Na iOS a Android zobrazí odděluje dynamické řádku řádky. Můžete řídit o [ `SeparatorVisibiliy` ](https://developer.xamarin.com/api/property/Xamarin.Forms.ListView.SeparatorVisibility/) a [ `SeparatorColor` ](https://developer.xamarin.com/api/property/Xamarin.Forms.ListView.SeparatorColor/) vlastnosti. `SeparatorVisibility` Vlastnost je typu [ `SeparatorVisbility` ](https://developer.xamarin.com/api/type/Xamarin.Forms.SeparatorVisibility/), výčet s dva členy:

- [`Default`](https://developer.xamarin.com/api/field/Xamarin.Forms.SeparatorVisibility.Default/), výchozí nastavení
- [`None`](https://developer.xamarin.com/api/field/Xamarin.Forms.SeparatorVisibility.None/)

### <a name="data-binding-the-selected-item"></a>Datové vazby vybrané položky

`SelectedItem` Vlastnost je zálohovaný díky vazbu vlastnosti, což může být buď zdroj nebo cíl datová vazba. Jeho výchozí `BindingMode` je `OneWayToSource`, ale obecně je cílem obousměrný datovou vazbu, zejména ve scénářích rozhraní MVVM. [ **ListViewArray** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter19/ListViewArray) příklad znázorňuje tento typ vazby.

### <a name="the-observablecollection-difference"></a>Rozdíl ObservableCollection

[ **ListViewLogger** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter19/ListViewLogger) ukázkové sady `ItemsSource` vlastnost `ListView` k `List<DateTime>` kolekci a pak postupně přidá nový `DateTime` objektu do kolekce každé druhé pomocí časovač.

Ale `ListView` nebude aktualizovat automaticky protože `List<T>` kolekce nemá vhodný mechanismus oznámení označíte, když se položky přidají do nebo z kolekce odebrán.

Je mnohem lepší třídu pro použití v takových scénářů [ `ObservableCollection<T>` ](https://developer.xamarin.com/api/type/System.Collections.ObjectModel.ObservableCollection%3CT%3E/) definovaný v `System.Collections.ObjectModel` oboru názvů. Tato třída implementuje [ `INotifyCollectionChanged` ](https://developer.xamarin.com/api/type/System.Collections.Specialized.INotifyCollectionChanged/) rozhraní a proto je aktivována [ `CollectionChanged` ](https://developer.xamarin.com/api/event/System.Collections.ObjectModel.ObservableCollection%3CT%3E.CollectionChanged/) událost v případě, že jsou položky Přidat nebo odebrat z kolekce, nebo pokud jsou nahrazena nebo přesunout v rámci kolekce. Když `ListView` interně rozpozná, že třída implementace `INotifyCollectionChanged` byla nastavena na jeho `ItemsSource` vlastnost přiloží obslužnou rutinu pro `CollectionChanged` událostí a aktualizuje jeho zobrazení, když se změní kolekce.

[ **ObservableLogger** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter19/ObservableLogger) příklad ukazuje použití `ObservableCollection`.

### <a name="templates-and-cells"></a>Šablony a buněk

Ve výchozím nastavení `ListView` zobrazí položky v jeho kolekce pomocí každou položku `ToString` metoda. Je vhodnější zahrnuje definice šablony a zobrazit seznam položek.

Pokud chcete vyzkoušet tuto funkci, můžete použít [ `NamedColor` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/NamedColor.cs) třídy v [ **Xamarin.FormsBook.Toolkit** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit) knihovny. Tato třída definuje statického `All` vlastnost typu `IList<NamedColor>` obsahující 141 `NamedColor` objekty odpovídající veřejný pole `Color` struktura.

[ **NaiveNamedColorList** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter19/NaiveNamedColorList) ukázkové sady `ItemsSource` z `ListView` k tomuto `NamedColor.All` vlastnost, ale pouze plně kvalifikovaný třída názvy `NamedColor` jsou objekty zobrazí.

`ListView` musí šablonu zobrazíte tyto položky. V kódu, můžete nastavit [ `ItemTemplate` ](https://developer.xamarin.com/api/property/Xamarin.Forms.ItemsView%3CTVisual%3E.ItemTemplate/) vlastnosti definované `ItemsView<TVisual>` k [ `DataTemplate` ](https://developer.xamarin.com/api/type/Xamarin.Forms.DataTemplate/) pomocí [ `DataTemplate` konstruktor](https://developer.xamarin.com/api/constructor/Xamarin.Forms.DataTemplate.DataTemplate/p/System.Type/) , odkazuje na odvozený ze [ `Cell` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Cell/) třídy. `Cell` má pět odvozené konfigurace:

- [`TextCell`](https://developer.xamarin.com/api/type/Xamarin.Forms.TextCell/) & #x 2014; obsahuje dva `Label` zobrazení (koncepčně mluvení)
- [`ImageCell`](https://developer.xamarin.com/api/type/Xamarin.Forms.ImageCell/) &#x2014; Přidá `Image` zobrazení `TextCell`
- [`EntryCell`](https://developer.xamarin.com/api/type/Xamarin.Forms.EntryCell/) &#x2014; obsahuje `Entry` zobrazení s `Label`
- [`SwitchCell`](https://developer.xamarin.com/api/type/Xamarin.Forms.SwitchCell/) &#x2014; obsahuje `Switch` s `Label`
- [`ViewCell`](https://developer.xamarin.com/api/type/Xamarin.Forms.ViewCell/) &#x2014; může být libovolná `View` (pravděpodobně s podřízenými položkami)

Potom zavolejte [ `SetValue` ](https://developer.xamarin.com/api/member/Xamarin.Forms.DataTemplate.SetValue/p/Xamarin.Forms.BindableProperty/System.Object/) a [ `SetBinding` ](https://developer.xamarin.com/api/member/Xamarin.Forms.DataTemplate.SetBinding/p/Xamarin.Forms.BindableProperty/Xamarin.Forms.BindingBase/) na `DataTemplate` objekt, který chcete přidružit hodnoty se `Cell` vlastnosti, nebo nastavit vazby dat na `Cell` odkazování na vlastnosti položek v části vlastnosti `ItemsSource` kolekce. Tento postup je znázorněn v [ **TextCellListCode** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter19/TextCellListCode) ukázka.

Jak každou položku se zobrazí `ListView`, malé vizuálním stromu je vytvořený z šablony a vazby dat jsou určeny mezi položkou a vlastnosti elementů v této vizuálním stromu. Můžete získat představu o tento proces obslužné rutiny pro instalaci [ `ItemAppearing` ](https://developer.xamarin.com/api/event/Xamarin.Forms.ListView.ItemAppearing/) a [ `ItemDisappearing` ](https://developer.xamarin.com/api/event/Xamarin.Forms.ListView.ItemDisappearing/) události `ListView`, nebo pomocí alternativu [ `DataTemplate`konstruktor](https://developer.xamarin.com/api/constructor/Xamarin.Forms.DataTemplate.DataTemplate/p/System.Func%7BSystem.Object%7D/) používající funkci, která je volána pokaždé, když je nutné vytvořit položky vizuálním stromu.

[ **TextCellListXaml** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter19/TextCellListXaml) ukazuje funkčně identické program zcela v jazyce XAML. A `DataTemplate` značky je nastaven na `ItemTemplate` vlastnost `ListView`a potom `TextCell` je nastaven na `DataTemplate`. Vazby na vlastnosti položek v kolekci jsou přímo na nastavené [ `Text` ](https://developer.xamarin.com/api/property/Xamarin.Forms.TextCell.Text/) a [ `Detail` ](https://developer.xamarin.com/api/property/Xamarin.Forms.TextCell.Detail/) vlastnosti `TextCell`.

### <a name="custom-cells"></a>Vlastní buněk

V jazyce XAML je možné nastavit [ `ViewCell` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ViewCell/) k `DataTemplate` a pak definovat vlastní vizuálním stromu jako [ `View` ](https://developer.xamarin.com/api/property/Xamarin.Forms.ViewCell.View/) vlastnost `ViewCell`. (`View` je vlastnost obsahu `ViewCell` proto `ViewCell.View` nejsou požadované značky.) [ **CustomNamedColorList** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter19/CustomNamedColorList) příklad znázorňuje tento postup:

[![Trojitá snímek obrazovky seznam pojmenovaná barva vlastní](images/ch19fg11-small.png "seznam pojmenovaná barva vlastní")](images/ch19fg11-large.png "seznam pojmenovaná barva vlastní")

Získávání nastavení velikosti pro všechny platformy může být složité. [ `RowHeight` ](https://developer.xamarin.com/api/property/Xamarin.Forms.ListView.RowHeight/) Vlastnost je užitečná, ale v některých případech budete chtít použít [ `HasUnevenRows` ](https://developer.xamarin.com/api/property/Xamarin.Forms.ListView.HasUnevenRows/) vlastnosti, která sice méně efektivní, ale vynutí `ListView` na velikost řádky. Pro iOS a Android musíte použít jednu z těchto dvou vlastností získat nastavení velikosti řádku správné.

### <a name="grouping-the-listview-items"></a>Seskupení položek ListView

`ListView` podporuje seskupení položek a procházení mezi těchto skupin. `ItemsSource` Musí být nastavena na kolekce kolekcí: objekt, `ItemsSource` je nastaven na musí implementovat `IEnumerable`, a každou položku v kolekci musí také implementovat `IEnumerable`. Každá skupina by měla obsahovat dvě vlastnosti: textový popis skupiny a zkratka tří písmen.

[ `NamedColorGroup` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/NamedColorGroup.cs) Třídy v [ **Xamarin.FormsBook.Toolkit** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit) knihovny vytvoří sedm skupiny `NamedColor` objekty. [ **ColorGroupList** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter19/ColorGroupList) ukázka ukazuje, jak použít tyto skupiny se [ `IsGroupingEnabled` ](https://developer.xamarin.com/api/property/Xamarin.Forms.ListView.IsGroupingEnabled/) vlastnost `ListView` nastavena na `true`a [ `GroupDisplayBinding` ](https://developer.xamarin.com/api/property/Xamarin.Forms.ListView.GroupDisplayBinding/) a [ `GroupShortNameBinding` ](https://developer.xamarin.com/api/property/Xamarin.Forms.ListView.GroupShortNameBinding/) vlastnosti vázána na vlastnosti v každé skupině.

### <a name="custom-group-headers"></a>Vlastní skupiny hlavičky

Je možné vytvořit vlastní hlavičky pro `ListView` skupiny tak, že nahradíte `GroupDisplayBinding` vlastnost s [ `GroupHeaderTemplate` ](https://developer.xamarin.com/api/property/Xamarin.Forms.ListView.GroupHeaderTemplate/) definice šablony pro hlavičky.

### <a name="listview-and-interactivity"></a>ListView a interakce

Obecně aplikace získá interakce uživatele s `ListView` připojením obslužné rutiny na `ItemSelected` nebo [ `ItemTapped` ](https://developer.xamarin.com/api/event/Xamarin.Forms.ListView.ItemTapped/) události, nebo nastavením datová vazba na `SelectedItem` vlastnost. Ale některé typy buňky (`EntryCell` a `SwitchCell`) umožňují interakci s uživatelem, a je také možné vytvořit vlastní buněk, že sami komunikovat s uživatelem. [ **InteractiveListView** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter19/InteractiveListView) vytváří 100 instance [ `ColorViewModel` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/ColorViewModel.cs) a umožňuje uživatelům změnit jednotlivých barev pomocí trojice `Slider` elementy. Program také využívá [ `ColorToContrastColorConverter` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/ColorToContrastColorConverter.cs) v [ **Xamarin.FormsBook.Toolkit**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit).

## <a name="listview-and-mvvm"></a>ListView a rozhraní MVVM

`ListView` ve scénářích rozhraní MVVM hraje důležitou roli. Vždy, když `IEnumerable` kolekce existuje v ViewModel, často je vázána na `ListView`. Také implementovat položky v kolekci často `INotifyPropertyChanged` pro vazbu s vlastnostmi v šabloně.

### <a name="a-collection-of-viewmodels"></a>Kolekce ViewModels

K prozkoumání, [ **SchoolOfFineArts** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/SchoolOfFineArt) knihovny vytvoří několik tříd, na základě [datového souboru XML](http://xamarin.github.io/xamarin-forms-book-samples/SchoolOfFineArt/students.xml) a bitové kopie fiktivní studenty na tento fiktivní škole.

[ `Student` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/SchoolOfFineArt/SchoolOfFineArt/Student.cs) Třída odvozená z [ `ViewModelBase` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/SchoolOfFineArt/SchoolOfFineArt/ViewModelBase.cs). [ `StudentBody` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/SchoolOfFineArt/SchoolOfFineArt/StudentBody.cs) Třída je kolekce `Student` objekty a také je odvozena z `ViewModelBase`. [ `SchoolViewModel` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/SchoolOfFineArt/SchoolOfFineArt/SchoolViewModel.cs) Stáhne soubor XML a sestaví všechny objekty.

[ **StudentList** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter19/StudentList) program používá `ImageCell` zobrazíte studenty a jejich obrázků v `ListView`:

[![Trojitá snímek obrazovky seznamu Student](images/ch19fg18-small.png "Student seznamu")](images/ch19fg18-large.png "Student seznamu")

[ **ListViewHeader** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter19/ListViewHeader) ukázka přidá [ `Header` ](https://developer.xamarin.com/api/property/Xamarin.Forms.ListView.Header/) vlastnost ale zobrazuje pouze v systému Android.

### <a name="selection-and-the-binding-context"></a>Výběr a kontextu vazby

[ **SelectedStudentDetail** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter19/SelectedStudentDetail) programu vazby `BindingContext` z `StackLayout` k `SelectedItem` vlastnost `ListView`. To umožňuje program, který chcete zobrazit podrobné informace o vybrané student.

### <a name="context-menus"></a>Kontextové nabídky

Buňku můžete definovat z kontextové nabídky, která je implementována způsobem specifické pro platformu. Chcete-li vytvořit tuto nabídku, přidejte [ `MenuItem` ](https://developer.xamarin.com/api/type/Xamarin.Forms.MenuItem/) objekty ke [ `ContextActions` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Cell.ContextActions/) vlastnost `Cell`.

`MenuItem` Definuje pěti vlastností:

- [`Text`](https://developer.xamarin.com/api/property/Xamarin.Forms.MenuItem.Text/) typu `string`
- [`Icon`](https://developer.xamarin.com/api/property/Xamarin.Forms.MenuItem.Icon/) typu `FileImageSource`
- [`IsDestructive`](https://developer.xamarin.com/api/property/Xamarin.Forms.MenuItem.IsDestructive/) typu `bool`
- [`Command`](https://developer.xamarin.com/api/property/Xamarin.Forms.MenuItem.Command/) typu `ICommand`
- [`CommandParameter`](https://developer.xamarin.com/api/property/Xamarin.Forms.MenuItem.CommandParameter/) typu `object`

`Command` a `CommandParameter` vlastnosti neznamená, že ViewModel pro každou položku obsahuje metody pro příkazy požadované nabídek. Ve scénářích modelem MVVM `MenuItem` také definuje [ `Clicked` ](https://developer.xamarin.com/api/event/Xamarin.Forms.MenuItem.Clicked/) událostí.

[ **CellContextMenu** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter19/CellContextMenu) ukazuje tento postup. `Command` Vlastnost jednotlivých `MenuItem` je vázána na vlastnost typu `ICommand` v `Student` třídy. Nastavte `IsDestructive` vlastnost `true` pro `MenuItem` který odebere nebo odstraní vybraný objekt.

### <a name="varying-the-visuals"></a>Různých vizuálech

V některých případech budete muset drobné rozdíly v vizuálech položek v `ListView` na základě vlastnosti. Například když Studentova úrovni bodu průměr klesne pod 2.0, [ **ColorCodedStudents** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter19/ColorCodedStudents) ukázka zobrazuje název této Studentova červeně.
To se provádí prostřednictvím použití převaděč hodnoty vazby, [ `ThresholdToObjectConverter` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/ThresholdToObjectConverter.cs)v [ **Xamarin.FormsBook.Toolkit** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit) knihovny.

### <a name="refreshing-the-content"></a>Aktualizace obsahu

`ListView` Podporuje gesto rozevírací pro aktualizaci jeho data. Program musí nastavit [ `IsPullToRefresh` ](https://developer.xamarin.com/api/property/Xamarin.Forms.ListView.IsPullToRefreshEnabled/) vlastnost `true` Chcete-li povolit tuto funkci. `ListView` Odpoví na gesto rozevírací nastavením jeho [ `IsRefreshing` ](https://developer.xamarin.com/api/property/Xamarin.Forms.ListView.IsRefreshing/) vlastnost `true`a zobrazením [ `Refreshing` ](https://developer.xamarin.com/api/event/Xamarin.Forms.ListView.Refreshing/) událostí a (pro scénáře s modelem MVVM) volání `Execute` metodu jeho [ `RefreshCommand` ](https://developer.xamarin.com/api/property/Xamarin.Forms.ListView.RefreshCommand/) vlastnost.

Zpracování kódu `Refresh` události nebo `RefreshCommand` pravděpodobně aktualizace dat zobrazených ve `ListView` a nastaví `IsRefreshing` zpět na `false`.

[ **RssFeed** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter19/RssFeed) příklad ukazuje použití [ `RssFeedViewModel` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter19/RssFeed/RssFeed/RssFeed/RssFeedViewModel.cs) , která implementuje `RefreshCommand` a `IsRefreshing` vlastnosti pro datovou vazbu.

## <a name="the-tableview-and-its-intents"></a>Zobrazení Tabulka a její záměry

Při `ListView` obecně zobrazí více instancí stejného typu, [ `TableView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.TableView/) se obecně zaměřuje na zajištění uživatelské rozhraní pro více vlastností různých typů. Každá položka souvisí s vlastním [ `Cell` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Cell/) odvozené pro zobrazení vlastnost nebo poskytuje uživatelské rozhraní k němu.

### <a name="properties-and-hierarchies"></a>Vlastnosti a hierarchií

`TableView` definuje pouze čtyři vlastnosti:

- [`Intent`](https://developer.xamarin.com/api/property/Xamarin.Forms.TableView.Intent/) typu [ `TableIntent` ](https://developer.xamarin.com/api/type/Xamarin.Forms.TableIntent/), výčet
- [`Root`](https://developer.xamarin.com/api/property/Xamarin.Forms.TableView.Root/) typu [ `TableRoot` ](https://developer.xamarin.com/api/type/Xamarin.Forms.TableRoot/), vlastnost obsahu `TableView`
- [`RowHeight`](https://developer.xamarin.com/api/property/Xamarin.Forms.TableView.RowHeight/) typu `int`
- [`HasUnevenRows`](https://developer.xamarin.com/api/property/Xamarin.Forms.TableView.HasUnevenRows/) typu `bool`

`TableIntent` Výčet Určuje, jak máte v úmyslu použít `TableView`:

- [`Data`](https://developer.xamarin.com/api/field/Xamarin.Forms.TableIntent.Data/)
- [`Form`](https://developer.xamarin.com/api/field/Xamarin.Forms.TableIntent.Form/)
- [`Settings`](https://developer.xamarin.com/api/field/Xamarin.Forms.TableIntent.Settings/)
- [`Menu`](https://developer.xamarin.com/api/field/Xamarin.Forms.TableIntent.Menu/)

Tito členové také navrhnout některé používá `TableView`.

Několik ostatní třídy jsou součástí definice tabulky:

- [`TableSectionBase`](https://developer.xamarin.com/api/type/Xamarin.Forms.TableSectionBase/) je abstraktní třída, která je odvozena z `BindableObject` a definuje [ `Title` ](https://developer.xamarin.com/api/property/Xamarin.Forms.TableSectionBase.Title/) vlastnost

- [`TableSectionBase<T>`](https://developer.xamarin.com/api/type/Xamarin.Forms.TableSectionBase%3CT%3E/) je abstraktní třída, která je odvozena z `TableSectionBase` a implementuje `IList<T>` a `INotifyCollectionChanged`

- [`TableSection`](https://developer.xamarin.com/api/type/Xamarin.Forms.TableSection/) odvozená z `TableSectionBase<Cell>`

- [`TableRoot`](https://developer.xamarin.com/api/type/Xamarin.Forms.TableRoot/) odvozená z `TableSectionBase<TableSection>`

Stručně řečeno `TableView` má `Root` vlastnost, která můžete nastavit na hodnotu `TableRoot` objekt, který je kolekce z `TableSection` objekty, z nichž každý je kolekce `Cell` objekty. Tabulka obsahuje více oddílů, přičemž každý oddíl má více buněk. Samotné tabulky může mít název, a každé části může mít název. I když `TableView` využívá `Cell` odvozené konfigurace, neprovede použití `DataTemplate`.

### <a name="a-prosaic-form"></a>Prosaic formuláře

[ **EntryForm** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter19/EntryForm) definuje ukázková [ `PersonalInformation` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter19/EntryForm/EntryForm/EntryForm/PersonalInformation.cs) modelu zobrazení, stane se o instanci `BindingContext` z `TableView`. Každý `Cell` odvozené v jeho `TableSection` potom může mít vazby na vlastnosti `PersonalInformation` třídy.

### <a name="custom-cells"></a>Vlastní buněk

[ **ConditionalCells** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter19/ConditionalCells) ukázka rozšíří na **EntryForm**. [ `ProgrammerInformation` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter19/EntryForm/EntryForm/EntryForm/PersonalInformation.cs) Třída zahrnuje vlastnost typu Boolean, která řídí použitelnost dva další vlastnosti. Pro tyto dva další vlastnosti, program používá vlastní `PickerCell` na základě [PickerCell.xaml](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/PickerCell.xaml) a [PickerCell.xaml.cs](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/PickerCell.xaml.cs) v [  **Xamarin.FormsBook.Toolkit** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit) knihovny.

I když `IsEnabled` vlastnosti dvou `PickerCell` elementy je vázána na vlastnost typu Boolean v `ProgrammerInformation`, tato technika zdá se, že chcete pracovat, který zobrazí výzvu bude další vzorek.

### <a name="conditional-sections"></a>Podmíněné sekce

[ **ConditionalSection** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter19/ConditionalSection) ukázka vloží dvě položky, které jsou podmíněné na výběr Boolean položky v samostatné `TableSection`. Odebere z této části souboru kódu `TableView` nebo ho přidá na základě zpět na vlastnost typu Boolean.

### <a name="a-tableview-menu"></a>Zobrazení Tabulka nabídky

Další používání `TableView` je nabídky. [ **MenuCommands** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter19/MenuCommands) příklad ukazuje nabídky, která umožňuje přesunout trochu `BoxView` po obrazovce.



## <a name="related-links"></a>Související odkazy

- [Úplný text 19 kapitoly (PDF)](https://download.xamarin.com/developer/xamarin-forms-book/XamarinFormsBook-Ch19-Apr2016.pdf)
- [Ukázky kapitoly 19](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter19)
- [ListView](~/xamarin-forms/user-interface/listview/index.md)
- [TableView](~/xamarin-forms/user-interface/tableview.md)
