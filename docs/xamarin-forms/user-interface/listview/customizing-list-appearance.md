---
title: Seznam vzhledu
description: Přizpůsobte ListViews pomocí záhlaví, zápatí, skupiny a proměnné výška buňky.
ms.prod: xamarin
ms.assetid: DC8009B0-4371-4D60-885A-5362FC7EE3E5
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 03/08/2016
ms.openlocfilehash: 76a5c96d0e7bb85f0e6b313e2dbc058b8c2aae6d
ms.sourcegitcommit: 6f7033a598407b3e77914a85a3f650544a4b6339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/06/2018
---
# <a name="list-appearance"></a>Seznam vzhledu

`ListView` možnosti pro řízení prezentace seznamu celkové kromě základní `ViewCell`s. Mezi možnosti patří:

- [**Seskupování** ](#Grouping) &ndash; seskupit položky v ListView usnadnil navigaci a lepší organizaci.
- [**Záhlaví a zápatí** ](#Headers_and_Footers) &ndash; zobrazení informací na začátku a konci zobrazení, které se posouvá společně s další položky.
- [**Řádek oddělovače** ](#Row_Separators) &ndash; zobrazit nebo skrýt oddělovacích čar mezi položkami.
- [**Proměnné výšky řádků** ](#Row_Heights) &ndash; ve výchozím nastavení jsou všechny řádky výšce stejné, ale to se dá změnit umožňující řádky s různými výšky, který se má zobrazit.

<a name="Grouping" />

## <a name="grouping"></a>Seskupování
Často se může stát nepraktické, když v seznamu nepřetržitě posouvání rozsáhlých datových sad. Povolení seskupení lze vylepšit činnost koncového uživatele v těchto případech lepší uspořádání obsahu a aktivace ovládacích prvků specifických pro platformy, které usnadňují navigační data.

Když je aktivován seskupení pro `ListView`, se přidá řádek záhlaví pro každou skupinu.

Chcete-li povolit seskupování:

- Vytvořte seznam seznamů (seznam skupin, každé skupiny se seznam prvků).
- Nastavte `ListView`na `ItemsSource` do tohoto seznamu.
- Nastavit `IsGroupingEnabled` na hodnotu true.
- Nastavit [ `GroupDisplayBinding` ](http://developer.xamarin.com/api/property/Xamarin.Forms.ListView.GroupDisplayBinding/) k vytvoření vazby na vlastnost skupiny, který se používá jako název skupiny.
- [Nepovinné] Nastavit [ `GroupShortNameBinding` ](http://developer.xamarin.com/api/property/Xamarin.Forms.ListView.GroupShortNameBinding/) k vytvoření vazby na vlastnost skupiny, který je používán jako krátký název pro skupinu. Krátký název se používá pro seznam odkazů (rigt straně sloupec v systému iOS, dlaždice mřížky na Windows Phone).

Začněte vytvořením třídy pro skupiny:

```csharp
public class PageTypeGroup : List<PageModel>
    {
        public string Title { get; set; }
        public string ShortName { get; set; } //will be used for jump lists
        public string Subtitle { get; set; }
        private PageTypeGroup(string title, string shortName)
        {
            Title = title;
            ShortName = shortName;
        }

        public static IList<PageTypeGroup> All { private set; get; }
    }
```

Ve výše uvedeném kódu `All` je seznam, který bude přiřazen k naší ListView jako zdroj vazby. `Title` a `ShortName` jsou vlastnosti, které se použije pro záhlaví skupiny.

V této fázi `All` je prázdný seznam. Přidání statického konstruktoru tak, aby v seznamu se zobrazí při spuštění programu:

```csharp
static PageTypeGroup()
{
    List<PageTypeGroup> Groups = new List<PageTypeGroup> {
            new PageTypeGroup ("Alfa", "A"){
                new PageModel("Amelia", "Cedar", new switchCellPage(),""),
                new PageModel("Alfie", "Spruce", new switchCellPage(), "grapefruit.jpg"),
                new PageModel("Ava", "Pine", new switchCellPage(), "grapefruit.jpg"),
                new PageModel("Archie", "Maple", new switchCellPage(), "grapefruit.jpg")
            },
            new PageTypeGroup ("Bravo", "B"){
                new PageModel("Brooke", "Lumia", new switchCellPage(),""),
                new PageModel("Bobby", "Xperia", new switchCellPage(), "grapefruit.jpg"),
                new PageModel("Bella", "Desire", new switchCellPage(), "grapefruit.jpg"),
                new PageModel("Ben", "Chocolate", new switchCellPage(), "grapefruit.jpg")
            }
        }
        All = Groups; //set the publicly accessible list
}
```

Ve výše uvedeném kódu můžete také říkáme `Add` na elementy `groups`, které jsou instancemi typu `PageTypeGroup`. To je možné, protože `PageTypeGroup` dědí z `List<PageModel>`. Toto je příklad seznam seznamů vzor uvedených výše.

Tady je XAML pro zobrazení seznamu seskupené:

```xaml
<?xml version="1.0" encoding="UTF-8"?>
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
x:Class="DemoListView.GroupingViewPage"
    <ContentPage.Content>
        <ListView  x:Name="GroupedView"
        GroupDisplayBinding="{Binding Title}"
        GroupShortNameBinding="{Binding ShortName}"
        IsGroupingEnabled="true">
            <ListView.ItemTemplate>
                <DataTemplate>
                    <TextCell Text="{Binding Title}"
                     Detail="{Binding Subtitle}" />
                </DataTemplate>
            </ListView.ItemTemplate>
        </ListView>
    </ContentPage.Content>
</ContentPage>
```

Výsledkem je následující:

![](customizing-list-appearance-images/grouping-depth.png "Příklad seskupení ListView")

Všimněte si, že máme:

- Nastavit `GroupShortNameBinding` k `ShortName` vlastnost definovanou v naší skupiny – třída
- Nastavit `GroupDisplayBinding` k `Title` vlastnost definovanou v naší skupiny – třída
- Nastavit `IsGroupingEnabled` na hodnotu true
- Změnit `ListView`na `ItemsSource` do seznamu seskupené

### <a name="customizing-grouping"></a>Přizpůsobení seskupení

Pokud bylo povoleno seskupení v seznamu, lze přizpůsobit záhlaví skupiny.

Podobným způsobem `ListView` má `ItemTemplate` pro definování zobrazení řádky, `ListView` má `GroupHeaderTemplate`. 

Zde je uveden příklad možností přizpůsobení záhlaví skupiny v jazyce XAML:

```xaml
<?xml version="1.0" encoding="UTF-8"?>
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
x:Class="DemoListView.GroupingViewPage">
    <ContentPage.Content>
        <ListView x:Name="GroupedView"
         GroupDisplayBinding="{Binding Title}"
         GroupShortNameBinding="{Binding ShortName}"
         IsGroupingEnabled="true">
            <ListView.ItemTemplate>
                <DataTemplate>
                    <TextCell Text="{Binding Title}"
                    Detail="{Binding Subtitle}"
                    TextColor="#f35e20"
                    DetailColor="#503026" />
                </DataTemplate>
            </ListView.ItemTemplate>
            <!-- Group Header Customization-->
            <ListView.GroupHeaderTemplate>
                <DataTemplate>
                    <TextCell Text="{Binding Title}"
                    Detail="{Binding ShortName}"
                    TextColor="#f35e20"
                    DetailColor="#503026" />
                </DataTemplate>
            </ListView.GroupHeaderTemplate>
            <!-- End Group Header Customization
        </ListView>
    </ContentPage.Content>
</ContentPage>
```

<a name="Headers_and_Footers" />

## <a name="headers-and-footers"></a>Záhlaví a zápatí
Je možné pro ListView nabídne záhlaví a zápatí stránky, které posunout s prvky v seznamu. Záhlaví a zápatí stránky může být řetězce textu nebo složitější rozložení. Všimněte si, že to je oddělený od [části skupiny](#Grouping).

Můžete nastavit `Header` nebo `Footer` na řetězec jednoduchého hodnotu, nebo můžete nastavit je složitější rozložení.
Existují také `HeaderTemplate` a `FooterTemplate` vlastnosti, které vám umožní vytvořit složitější rozložení pro záhlaví a zápatí této vazby dat podpory.

Pokud chcete vytvořit jednoduché záhlaví a zápatí, stačí nastavte vlastnosti záhlaví nebo zápatí stránky na text, který chcete zobrazit. V kódu:

```csharp
ListView HeaderList = new ListView() {
    Header = "Header",
    Footer = "Footer"
    };
```

V jazyce XAML:

```xaml
<ListView  x:Name="HeaderList"  Header="Header" Footer="Footer"></ListView>
```

![](customizing-list-appearance-images/header-default.png "ListView s záhlaví a zápatí")

Pokud chcete vytvořit vlastní záhlaví a zápatí, definujte zobrazení záhlaví a zápatí stránky:

```xaml
<ListView.Header>
    <StackLayout Orientation="Horizontal">
        <Label Text="Header"
        TextColor="Olive"
        BackgroundColor="Red" />
    </StackLayout>
</ListView.Header>
<ListView.Footer>
    <StackLayout Orientation="Horizontal">
        <Label Text="Footer"
        TextColor="Gray"
        BackgroundColor="Blue" />
    </StackLayout>
</ListView.Footer>
```

![](customizing-list-appearance-images/header-custom.png "ListView s vlastní záhlaví a zápatí")

<a name="Row_Separators" />

## <a name="row-separators"></a>Řádek oddělovačů
Se zobrazí čáry oddělovače mezi `ListView` elementy ve výchozím nastavení na iOS a Android. Windows Phone nepodporuje oddělovač řádků na této platformy pravidla pro uživatelské prostředí. Pokud chcete skrýt oddělovacích čar na iOS a Android, nastavte `SeparatorVisibility` vlastnost na vaše ListView. Možnosti pro `SeparatorVisibility` jsou:

* **Výchozí** -zobrazuje oddělovací čáry na iOS a Android.
* **Žádný** -skryje oddělovače na všech platformách.

Výchozí viditelnost:

C#:

```csharp
SepratorDemoListView.SeparatorVisibility = SeparatorVisibility.Default;
```

XAML:

```xaml
<ListView x:Name="SeparatorDemoListView" SeparatorVisibility="Default" />
```

![](customizing-list-appearance-images/separator-default.png "ListView s oddělovači výchozí řádek")

Žádné:

C#:

```csharp
SepratorDemoListView.SeparatorVisibility = SeparatorVisibility.None;
```

XAML:

```xaml
<ListView x:Name="SeparatorDemoListView" SeparatorVisibility="None" />
```

![](customizing-list-appearance-images/separator-none.png "ListView bez oddělovačů řádek")

Můžete také nastavit barvu čáry oddělovače prostřednictvím `SeparatorColor` vlastnost:

C#:

```csharp
SepratorDemoListView.SeparatorColor = Color.Green;
```

XAML:

```xaml
<ListView x:Name="SeparatorDemoListView" SeparatorColor="Green" />
```

![](customizing-list-appearance-images/separator-custom.png "ListView s oddělovači řádek zelená")

> [!NOTE]
> Některá z těchto vlastností nastavení v systému Android po načtení `ListView` způsobuje snížení výkonu velké.

<a name="Row_Heights" />

## <a name="row-heights"></a>Výšky řádků
Všechny řádky v prvku ListView mají stejnou výšku ve výchozím nastavení. ListView má dvě vlastnosti, které můžete použít ke změně tohoto chování:

- `HasUnevenRows` &ndash; `true`/`false` hodnota, řádky mají různé výšky Pokud nastavena na `true`. Použije se výchozí hodnota `false`.
- `RowHeight` &ndash; Nastaví výšku každý řádek, kdy `HasUnevenRows` je `false`.

Výška všechny řádky můžete nastavit pomocí nastavení `RowHeight` vlastnost `ListView`.

### <a name="custom-fixed-row-height"></a>Vlastní pevnou výšku řádku

C#:

```csharp
RowHeightDemoListView.RowHeight = 100;
```

XAML:

```xaml
<ListView x:Name="RowHeightDemoListView" RowHeight="100" />
```

![](customizing-list-appearance-images/height-custom.png "ListView s pevnou výšku řádku")


### <a name="uneven-rows"></a>Nevyrovnaná řádků

Pokud chcete jednotlivé řádky, které mají různé výšky, můžete nastavit `HasUnevenRows` vlastnost `true`.
Všimněte si, že výšky řádků není nutné ručně nastavit jednou `HasUnevenRows` byla nastavena na `true`, protože výšky automaticky vypočítá Xamarin.Forms.


C#:

```csharp
RowHeightDemoListView.HasUnevenRows = true;
```

XAML:

```xaml
<ListView x:Name="RowHeightDemoListView" HasUnevenRows="true" />
```

![](customizing-list-appearance-images/height-uneven.png "ListView s nerovnoměrné řádky")

### <a name="runtime-resizing-of-rows"></a>Modul runtime Změna velikosti řádků

Jednotlivé `ListView` řádky jde prostřednictvím kódu programu změnit za běhu, za předpokladu, že `HasUnevenRows` je nastavena na `true`. [ `Cell.ForceUpdateSize` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Cell.ForceUpdateSize()/) Metoda aktualizace velikost buňky, i když není aktuálně viditelná, jak je ukázáno v následujícím příkladu kódu:

```csharp
void OnImageTapped (object sender, EventArgs args)
{
    var image = sender as Image;
    var viewCell = image.Parent.Parent as ViewCell;

    if (image.HeightRequest < 250) {
        image.HeightRequest = image.Height + 100;
        viewCell.ForceUpdateSize ();
    }
}
```

`OnImageTapped` Obslužné rutiny události se spustí v reakci na [ `Image` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Image/) v buňce se stisknuté a zvětšuje velikost `Image` zobrazena v buňce, aby je snadno zobrazit.

![](customizing-list-appearance-images/dynamic-row-resizing.png "ListView s Změna velikosti řádku modulu Runtime")

Všimněte si, že silné možnost snížení výkonu, pokud je tato funkce je Nepromyšlené.



## <a name="related-links"></a>Související odkazy

- [Seskupování (ukázka)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/ListView/Grouping)
- [Vlastní vykreslení zobrazení (ukázka)](https://developer.xamarin.com/samples/xamarin-forms/WorkingWithListviewNative/)
- [Dynamická změna velikosti řádků (ukázka)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/ListView/DynamicUnevenListCells/)
- [Poznámky k verzi 1.4](http://forums.xamarin.com/discussion/35451/xamarin-forms-1-4-0-released/)
- [1.3 poznámky](http://forums.xamarin.com/discussion/29934/xamarin-forms-1-3-0-released/)
