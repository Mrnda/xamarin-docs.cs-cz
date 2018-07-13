---
title: Přizpůsobení vzhledu ListView
description: Tento článek vysvětluje, jak přizpůsobit zobrazení v aplikacích Xamarin.Forms pomocí záhlaví, zápatí, skupiny a proměnné výška buňky.
ms.prod: xamarin
ms.assetid: DC8009B0-4371-4D60-885A-5362FC7EE3E5
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 03/08/2016
ms.openlocfilehash: 1326a1326b4a88459e4e0a01ef590e770e3a88c0
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/12/2018
ms.locfileid: "38997345"
---
# <a name="customizing-listview-appearance"></a>Přizpůsobení vzhledu ListView

`ListView` obsahuje možnosti pro řízení prezentace celkový přehled, kromě základního `ViewCell`s. Mezi možnosti patří:

- [**Seskupení** ](#Grouping) &ndash; seskupit položky v ListView pro snadnější navigaci a lepší organizaci.
- [**Záhlaví a zápatí** ](#Headers_and_Footers) &ndash; zobrazit informace na začátku a konci zobrazení, které se posouvá společně s další položky.
- [**Řádek oddělovače** ](#Row_Separators) &ndash; zobrazit nebo skrýt oddělovače řádků mezi položkami.
- [**Proměnnou výšku řádků** ](#Row_Heights) &ndash; ve výchozím nastavení všechny řádky se stejnou výškou, ale to se dá změnit umožňující řádky s různými výšky, který se má zobrazit.

<a name="Grouping" />

## <a name="grouping"></a>Seskupování
Velkých sad dat často, může být nepraktický, při zápisu do průběžně posuvný seznam. Povolení seskupení lze vylepšit uživatelské prostředí v těchto případech lepší uspořádání obsahu a aktivace ovládacích prvků pro konkrétní platformu, které usnadňují navigační data.

Když se aktivuje seskupení pro `ListView`, přidat řádek záhlaví pro každou skupinu.

Pokud chcete povolit seskupování:

- Vytvoření seznamů (seznam skupin, každá skupina se seznam prvků).
- Nastavte `ListView`společnosti `ItemsSource` do tohoto seznamu.
- Nastavte `IsGroupingEnabled` na hodnotu true.
- Nastavte [ `GroupDisplayBinding` ](xref:Xamarin.Forms.ListView.GroupDisplayBinding) vytvořit vazbu na vlastnost skupiny, která se používá jako název skupiny.
- [Volitelné] Nastavte [ `GroupShortNameBinding` ](xref:Xamarin.Forms.ListView.GroupShortNameBinding) vytvořit vazbu k vlastnosti skupiny, který je používán jako krátký název pro skupinu. Krátký název se používá pro seznamy odkazů (sloupec vpravo v Iosu).

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

Ve výše uvedeném kódu `All` je seznam, který bude předán naše ListView jako zdroj vazby. `Title` a `ShortName` jsou vlastnosti, které se použijí pro skupinu záhlaví.

V této fázi `All` je prázdný seznam. Přidání statického konstruktoru tak, aby naplní se seznam při spuštění programu:

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

Ve výše uvedeném kódu lze také říkáme `Add` na prvcích `groups`, které jsou instancemi typu `PageTypeGroup`. To je možné, protože `PageTypeGroup` dědí z `List<PageModel>`. Toto je příklad seznamu seznamy vzor, jak je uvedeno nahoře.

Tady je XAML pro zobrazení seskupeného seznamu:

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

Všimněte si, že máme k dispozici:

- Nastavte `GroupShortNameBinding` k `ShortName` vlastnosti definované v našich group – třída
- Nastavte `GroupDisplayBinding` k `Title` vlastnosti definované v našich group – třída
- Nastavte `IsGroupingEnabled` na hodnotu true
- Změnit `ListView`společnosti `ItemsSource` do seskupeného seznamu

### <a name="customizing-grouping"></a>Přizpůsobení seskupování

Pokud v seznamu se povolila seskupení, záhlaví skupiny se taky dají upravit.

Podobným způsobem `ListView` má `ItemTemplate` pro definování, jak je zobrazeno řádků, `ListView` má `GroupHeaderTemplate`.

Příklad přizpůsobení záhlaví skupiny v XAML je znázorněna zde:

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
            <!-- End Group Header Customization -->
        </ListView>
    </ContentPage.Content>
</ContentPage>
```

<a name="Headers_and_Footers" />

## <a name="headers-and-footers"></a>Záhlaví a zápatí
Je možné, ListView prezentovat záhlaví a zápatí, které umožňují s prvky v seznamu. Záhlaví a zápatí může být řetězce textu nebo složitější rozložení. Všimněte si, že se odděleně od [části skupiny](#Grouping).

Můžete nastavit `Header` a/nebo `Footer` k jednoduchým řetězcem hodnotu, nebo můžete je nastavit na složitější rozložení.
Existují také `HeaderTemplate` a `FooterTemplate` vlastnosti, které vám umožní vytvořit složitější rozložení pro záhlaví a zápatí této podpory datovou vazbu.

Pokud chcete vytvořit jednoduchou záhlaví/zápatí, stačí nastavte vlastnosti záhlaví nebo zápatí stránky na text, který chcete zobrazit. V kódu:

```csharp
ListView HeaderList = new ListView() {
    Header = "Header",
    Footer = "Footer"
    };
```

V XAML:

```xaml
<ListView  x:Name="HeaderList"  Header="Header" Footer="Footer"></ListView>
```

![](customizing-list-appearance-images/header-default.png "ListView s záhlaví a zápatí")

Chcete-li vytvořit vlastní záhlaví a zápatí, definujte zobrazení záhlaví a zápatí:

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

## <a name="row-separators"></a>Oddělovače řádků
Oddělovač řádků zobrazených mezi `ListView` elementy ve výchozím nastavení v Iosu a Androidu. Pokud chcete skrýt oddělovače řádků v Iosu a Androidu, nastavte `SeparatorVisibility` vlastnost na vaše ListView. Možnosti pro `SeparatorVisibility` jsou:

* **Výchozí** -ukazuje oddělovací čáry v Iosu a Androidu.
* **Žádný** -skryje oddělovač na všech platformách.

Výchozí viditelnost:

C#:

```csharp
SepratorDemoListView.SeparatorVisibility = SeparatorVisibility.Default;
```

XAML:

```xaml
<ListView x:Name="SeparatorDemoListView" SeparatorVisibility="Default" />
```

![](customizing-list-appearance-images/separator-default.png "ListView s výchozím řádkem oddělovače")

Žádné:

C#:

```csharp
SepratorDemoListView.SeparatorVisibility = SeparatorVisibility.None;
```

XAML:

```xaml
<ListView x:Name="SeparatorDemoListView" SeparatorVisibility="None" />
```

![](customizing-list-appearance-images/separator-none.png "ListView bez oddělovačů řádků")

Můžete také nastavit barvu oddělovací čáry prostřednictvím `SeparatorColor` vlastnost:

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
> Některé z těchto vlastností nastavení pro Android po načtení `ListView` způsobuje snížení výkonu velké.

<a name="Row_Heights" />

## <a name="row-heights"></a>Výšku řádků
Všechny řádky ListView mají stejnou výškou ve výchozím nastavení. ListView má dvě vlastnosti, které můžete použít ke změně tohoto chování:

- `HasUnevenRows` &ndash; `true`/`false` hodnota, řádky mají různé výšky-li nastavena na `true`. Výchozí hodnota je `false`.
- `RowHeight` &ndash; Nastaví výšku každého řádku, při `HasUnevenRows` je `false`.

Můžete nastavit tak, že nastavíte výšky všech řádků `RowHeight` vlastnost `ListView`.

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


### <a name="uneven-rows"></a>Nerovnoměrné řádků

Pokud chcete mít různé výšky jednotlivých řádků, můžete nastavit `HasUnevenRows` vlastnost `true`.
Všimněte si, že není nutné ručně nastavit jednou výšku řádků `HasUnevenRows` byla nastavena na `true`, protože výšky automaticky vypočítá Xamarin.Forms.


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

Jednotlivé `ListView` řádky dá programově změnit za běhu, za předpokladu, že `HasUnevenRows` je nastavena na `true`. [ `Cell.ForceUpdateSize` ](xref:Xamarin.Forms.Cell.ForceUpdateSize) Metoda aktualizuje velikost buňky, i když není aktuálně viditelné, jak je ukázáno v následujícím příkladu kódu:

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

`OnImageTapped` Obslužná rutina události je provést v reakci na [ `Image` ](xref:Xamarin.Forms.Image) v buňce se klepnutí a dojde ke zvětšení `Image` v buňce zobrazí tak, aby se snadno zobrazit.

![](customizing-list-appearance-images/dynamic-row-resizing.png "ListView s Změna velikosti řádků modulu Runtime")

Mějte na paměti, že pokud je tato funkce je Nepromyšlené neexistuje silné možnost snížení výkonu.



## <a name="related-links"></a>Související odkazy

- [Seskupení (ukázka)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/ListView/Grouping)
- [Vlastní nástroj pro vykreslování zobrazení (ukázka)](https://developer.xamarin.com/samples/xamarin-forms/WorkingWithListviewNative/)
- [Dynamická změna velikosti řádků (ukázka)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/ListView/DynamicUnevenListCells/)
- [zpráva k vydání verze 1.4](http://forums.xamarin.com/discussion/35451/xamarin-forms-1-4-0-released/)
- [Poznámky k verzi 1.3](http://forums.xamarin.com/discussion/29934/xamarin-forms-1-3-0-released/)
