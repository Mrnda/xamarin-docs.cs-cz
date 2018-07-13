---
title: Přizpůsobení vzhledu ListView buňky
description: Tento článek popisuje možnosti pro zobrazení dat v aplikacích Xamarin.Forms výhod pohodlí ovládacího prvku ListView.
ms.prod: xamarin
ms.assetid: FD45CB91-1A8F-46FB-B432-6BC20492E456
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 12/07/2016
ms.openlocfilehash: 7a0f55b6d8a61f52f4ef137d83c56d86149bc3c9
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/12/2018
ms.locfileid: "38996253"
---
# <a name="customizing-listview-cell-appearance"></a>Přizpůsobení vzhledu ListView buňky

ListView uvede posuvný seznamy, které je možné přizpůsobit prostřednictvím `ViewCell`s. `ViewCells` můžete použít pro zobrazení textu a obrázků, označující stav pravda/nepravda a přijímají uživatelský vstup.

Získávání vzhled, který chcete z buněk ListView dvěma způsoby:

- **[Přizpůsobení předdefinovaných buňky](#Built_in_Cells)**  &ndash; jednodušší implementací a lepší výkon na úkor přizpůsobitelnost.
- **[Vytváření vlastních buňky](#customcells)**  &ndash; větší kontrolu nad konečný výsledek, ale mají potenciální problémy s výkonem, pokud není správně implementován.

<a name="Built_in_Cells" />

## <a name="built-in-cells"></a>Vytvořené v buňkách
Xamarin.Forms dodává s integrovanou buňky, které fungují pro spoustu aplikací na jednoduchý:

- **TextCell** &ndash; pro zobrazení textu
- **Funkce ImageCell** &ndash; pro zobrazování obrázku s textem.

Dvě další buňky [ `SwitchCell` ](~/xamarin-forms/user-interface/tableview.md#switchcell) a [ `EntryCell` ](~/xamarin-forms/user-interface/tableview.md#entrycell) jsou k dispozici, ale nejsou běžně používá s `ListView`. Zobrazit [ `TableView` ](~/xamarin-forms/user-interface/tableview.md) Další informace o těchto buněk.

<a name="TextCell" />

### <a name="textcell"></a>TextCell

[`TextCell`](xref:Xamarin.Forms.TextCell) je buňky pro zobrazení textu, volitelně i s druhý řádek jako text podrobností.

TextCells jsou vykresleny jako nativní ovládací prvky za běhu, takže je výkon velmi dobře ve srovnání s vlastní `ViewCell`. TextCells jsou přizpůsobitelné, což vám umožní nastavit:

- `Text` &ndash; text, který se zobrazí na prvním řádku v velkými písmeny.
- `Detail` &ndash; text, který se zobrazí pod první řádek v menším písmem.
- `TextColor` &ndash; Barva textu.
- `DetailColor` &ndash; Barva textu podrobností

![](customizing-cell-appearance-images/text-cell-default.png "Příklad TextCell výchozí")

![](customizing-cell-appearance-images/text-cell-custom.png "Příklad přizpůsobené TextCell")

<a name="ImageCell" />

### <a name="imagecell"></a>Funkce ImageCell

[`ImageCell`](xref:Xamarin.Forms.ImageCell), jako je `TextCell`, lze použít pro zobrazení textu a sekundární podrobností text a nabízí skvělý výkon pomocí nativní ovládací prvky jednotlivé platformy. `ImageCell` se liší od `TextCell` v tom, že se zobrazí obrázek nalevo od textu.

`ImageCell` je užitečné, když budete chtít zobrazit seznam dat pomocí vizuální stran, například seznam kontaktů nebo filmy. ImageCells jsou přizpůsobitelné, což vám umožní nastavit:

- `Text` &ndash; text, který se zobrazí na prvním řádku v velkými písmeny
- `Detail` &ndash; text, který se zobrazí pod první řádek v menším písmem
- `TextColor` &ndash; Barva textu
- `DetailColor` &ndash; Barva textu podrobností
- `ImageSource` &ndash; Obrázek se zobrazí vedle textu

![](customizing-cell-appearance-images/image-cell-default.png "Výchozí funkce ImageCell příklad")

![](customizing-cell-appearance-images/image-cell-custom.png "Příklad přizpůsobené funkce ImageCell")

<a name="customcells" />

## <a name="custom-cells"></a>Vlastní buňky
Když integrované buňky neposkytuje požadované rozložení, implementovat vlastní buňky požadované rozložení. Můžete například představovat buňky pomocí dvou popisků, které mají stejnou důležitost. A `TextCell` by být nedostatečné vzhledem k tomu, `TextCell` má jeden popisek, který je menší. Většina nastavení buňky přidání dalších dat jen pro čtení (například další popisky, obrázky nebo jiné informace o zobrazení).

Všechny vlastní buňky musí být odvozen od [ `ViewCell` ](xref:Xamarin.Forms.ViewCell), je stejná základní třída, že všechny buňky vestavěné typy použití.

Xamarin.Forms 2 zavedl nový [chování ukládání do mezipaměti](~/xamarin-forms/user-interface/listview/performance.md#cachingstrategy) na `ListView` ovládací prvek, který může být nastaven na zvýšení výkonu posunování pro některé typy vlastní buněk.

Toto je příklad vlastního buňky:

![](customizing-cell-appearance-images/custom-cell.png "Příklad vlastní buňky")

### <a name="xaml"></a>XAML
XAML vytvořit výše rozložení je nižší než:

```xaml
<?xml version="1.0" encoding="UTF-8"?>
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
x:Class="demoListView.ImageCellPage">
    <ContentPage.Content>
        <ListView  x:Name="listView">
            <ListView.ItemTemplate>
                <DataTemplate>
                    <ViewCell>
                        <StackLayout BackgroundColor="#eee"
                        Orientation="Vertical">
                            <StackLayout Orientation="Horizontal">
                                <Image Source="{Binding image}" />
                                <Label Text="{Binding title}"
                                TextColor="#f35e20" />
                                <Label Text="{Binding subtitle}"
                                HorizontalOptions="EndAndExpand"
                                TextColor="#503026" />
                            </StackLayout>
                        </StackLayout>
                    </ViewCell>
                </DataTemplate>
            </ListView.ItemTemplate>
        </ListView>
    </ContentPage.Content>
</ContentPage>
```

Výše uvedené XAML je to mnohem. To Pojďme rozdělit:

- Vlastní buňky je vnořená do `DataTemplate`, která se nachází uvnitř `ListView.ItemTemplate`. Toto je stejný postup jako při použití libovolnou buňku.
- `ViewCell` je typ vlastního buňky. Podřízený `DataTemplate` elementu musí být nebo odvozen od typu `ViewCell`.
- Všimněte si, že uvnitř `ViewCell`, spravuje rozložení `StackLayout`. Toto rozložení umožňuje přizpůsobit barvu pozadí. Všimněte si, že nějaká vlastnost `StackLayout` , který je s možností vazby může být vázaný vlastní buňce, i když, která tady není ukázaný.

### <a name="cnum"></a>C&num;

Zadání vlastní buňku v jazyce C# je trochu podrobnější než ekvivalentní XAML. Podívejme se na to:

Nejprve definujte třídu vlastního buňky s `ViewCell` jako základní třídy:

```csharp
public class CustomCell : ViewCell
    {
        public CustomCell()
        {
            //instantiate each of our views
            var image = new Image ();
            StackLayout cellWrapper = new StackLayout ();
            StackLayout horizontalLayout = new StackLayout ();
            Label left = new Label ();
            Label right = new Label ();

            //set bindings
            left.SetBinding (Label.TextProperty, "title");
            right.SetBinding (Label.TextProperty, "subtitle");
            image.SetBinding (Image.SourceProperty, "image");

            //Set properties for desired design
            cellWrapper.BackgroundColor = Color.FromHex ("#eee");
            horizontalLayout.Orientation = StackOrientation.Horizontal;
            right.HorizontalOptions = LayoutOptions.EndAndExpand;
            left.TextColor = Color.FromHex ("#f35e20");
            right.TextColor = Color.FromHex ("503026");

            //add views to the view hierarchy
            horizontalLayout.Children.Add (image);
            horizontalLayout.Children.Add (left);
            horizontalLayout.Children.Add (right);
            cellWrapper.Children.Add (horizontalLayout);
            View = cellWrapper;
        }
    }
```

Vaše konstruktoru pro stránku s `ListView`, nastavte ListView `ItemTemplate` vlastnost do nového `DataTemplate`:

```csharp
public partial class ImageCellPage : ContentPage
    {
        public ImageCellPage ()
        {
            InitializeComponent ();
            listView.ItemTemplate = new DataTemplate (typeof(CustomCell));
        }
    }
```

Všimněte si, že konstruktor pro `DataTemplate` trvá typu. Operátor typeof získá typ CLR pro `CustomCell`.

<a name="binding-context-changes" />

### <a name="binding-context-changes"></a>Změny kontextu vazby

Při vytváření vazby na vlastní typ [ `BindableProperty` ](xref:Xamarin.Forms.BindableProperty) instancí, ovládacích prvků uživatelského rozhraní zobrazení `BindableProperty` byste použít hodnoty [ `OnBindingContextChanged` ](xref:Xamarin.Forms.Cell.OnBindingContextChanged) přepsání nastavení data, která mají být zobrazeny v Každá buňka, spíše než konstruktoru buňky, jak je ukázáno v následujícím příkladu kódu:

```csharp
public class CustomCell : ViewCell
{
    Label nameLabel, ageLabel, locationLabel;

    public static readonly BindableProperty NameProperty =
        BindableProperty.Create ("Name", typeof(string), typeof(CustomCell), "Name");
    public static readonly BindableProperty AgeProperty =
        BindableProperty.Create ("Age", typeof(int), typeof(CustomCell), 0);
    public static readonly BindableProperty LocationProperty =
        BindableProperty.Create ("Location", typeof(string), typeof(CustomCell), "Location");

    public string Name {
        get { return(string)GetValue (NameProperty); }
        set { SetValue (NameProperty, value); }
    }

    public int Age {
        get { return(int)GetValue (AgeProperty); }
        set { SetValue (AgeProperty, value); }
    }

    public string Location {
        get { return(string)GetValue (LocationProperty); }
        set { SetValue (LocationProperty, value); }
    }
    ...

    protected override void OnBindingContextChanged ()
    {
        base.OnBindingContextChanged ();

        if (BindingContext != null) {
            nameLabel.Text = Name;
            ageLabel.Text = Age.ToString ();
            locationLabel.Text = Location;
        }
    }
}
```

[ `OnBindingContextChanged` ](xref:Xamarin.Forms.Cell.OnBindingContextChanged) Bude volat přepsání, pokud [ `BindingContextChanged` ](xref:Xamarin.Forms.BindableObject.BindingContextChanged) událost je aktivována v reakci na hodnotu [ `BindingContext` ](xref:Xamarin.Forms.BindableObject.BindingContext) změny vlastnosti. Proto, když `BindingContext` změní, ovládacích prvků uživatelského rozhraní zobrazení [ `BindableProperty` ](xref:Xamarin.Forms.BindableProperty) hodnot by měl nastavit jejich data. Všimněte si, že `BindingContext` zkontrolovat `null` hodnoty, jak to lze nastavit pomocí Xamarin.Forms pro uvolňování paměti, které pak bude mít za následek `OnBindingContextChanged` přepsat volána.

Alternativně lze svázat ovládací prvky uživatelského rozhraní [ `BindableProperty` ](xref:Xamarin.Forms.BindableProperty) instance zobrazíte jejich hodnoty, které eliminuje potřebu přepsat `OnBindingContextChanged` metody.

> [!NOTE]
> Při přepisování `OnBindingContextChanged`, ujistěte se, že základní třídy `OnBindingContextChanged` metoda je volána, tak, že registrovaní delegáti obdrží `BindingContextChanged` událostí.

V XAML vlastní typ vazby k datům lze dosáhnout jak je znázorněno v následujícím příkladu kódu:

```xaml
<ListView x:Name="listView">
    <ListView.ItemTemplate>
        <DataTemplate>
            <local:CustomCell Name="{Binding Name}" Age="{Binding Age}" Location="{Binding Location}" />
        </DataTemplate>
    </ListView.ItemTemplate>
</ListView>
```

Tím je vytvořena vazba `Name`, `Age`, a `Location` vlastnosti umožňující vazbu v `CustomCell` instance položky `Name`, `Age`, a `Location` vlastnosti jednotlivých objektů v zdrojové kolekce.

Ekvivalentní vazby v jazyce C# můžete vidět v následujícím příkladu kódu:

```csharp
var customCell = new DataTemplate (typeof(CustomCell));
customCell.SetBinding (CustomCell.NameProperty, "Name");
customCell.SetBinding (CustomCell.AgeProperty, "Age");
customCell.SetBinding (CustomCell.LocationProperty, "Location");

var listView = new ListView {
    ItemsSource = people,
    ItemTemplate = customCell
};
```

V Iosu a Androidu Pokud [ `ListView` ](xref:Xamarin.Forms.ListView) se recykluje elementy a vlastní buňky používá vlastní zobrazovací jednotky, vlastní zobrazovací jednotky musí správně implementace oznámení změn vlastností. Když údaje znovu použijí buňky hodnot vlastností se změní při aktualizaci kontextu vazby, který k dispozici buňky, s `PropertyChanged` událostí vyvolaných. Další informace najdete v tématu [přizpůsobení ViewCell](~/xamarin-forms/app-fundamentals/custom-renderer/viewcell.md). Další informace o recyklaci buňky, naleznete v tématu [strategie ukládání do mezipaměti](~/xamarin-forms/user-interface/listview/performance.md#cachingstrategy).

## <a name="related-links"></a>Související odkazy

- [Součástí buňky (ukázka)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/ListView/BuiltInCells)
- [Vlastní buňky (ukázka)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/ListView/CustomCells)
- [Vazba kontext změnit (ukázka)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/ListView/BindingContextChanged)
