---
title: Vzhledu buněk
description: Prozkoumejte možnosti pro prezentaci dat s využitím pohodlím, které představuje ListView.
ms.prod: xamarin
ms.assetid: FD45CB91-1A8F-46FB-B432-6BC20492E456
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 12/07/2016
ms.openlocfilehash: 74f65021c23515e78e630f907a89ffde74de4da4
ms.sourcegitcommit: 1561c8022c3585655229a869d9ef3510bf83f00a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/27/2018
---
# <a name="cell-appearance"></a>Vzhledu buněk

ListView uvede posouvatelného seznamů, které lze přizpůsobit prostřednictvím `ViewCell`s. `ViewCells` můžete použít pro zobrazení textu a obrázků, uvádí stav true nebo false a přijetí vstup uživatele.

Získávání vzhledu, které chcete z buněk ListView dvěma způsoby:

- **[Přizpůsobení předdefinované buněk](#Built_in_Cells)**  &ndash; jednodušší implementací a lepší výkon za cenu možnosti přizpůsobení.
- **[Vytváření vlastních buněk](#customcells)**  &ndash; více ovládat konečný výsledek, ale mají potenciální problémy s výkonem, pokud není správně implementován.

<a name="Built_in_Cells" />

## <a name="built-in-cells"></a>Vytvořené v buňkách
Xamarin.Forms se dodává s integrovanou buněk, které pracují pro mnoho jednoduché aplikace:

- **TextCell** &ndash; pro zobrazení textu
- **Funkce ImageCell** &ndash; pro zobrazení bitovou kopii s textem.

Dvě další buňky, [ `SwitchCell` ](~/xamarin-forms/user-interface/tableview.md#switchcell) a [ `EntryCell` ](~/xamarin-forms/user-interface/tableview.md#entrycell) jsou k dispozici, ale nejsou obvykle používány s `ListView`. V tématu [ `TableView` ](~/xamarin-forms/user-interface/tableview.md) Další informace o těchto buněk.

<a name="TextCell" />

### <a name="textcell"></a>TextCell

[`TextCell`](http://developer.xamarin.com/api/type/Xamarin.Forms.TextCell/) je buňky pro zobrazení textu, volitelně i s druhou linii jako text podrobností.

TextCells se vykresluje jako nativní ovládací prvky v době běhu, výkon, je velmi dobré ve srovnání s vlastní `ViewCell`. TextCells lze přizpůsobit, abyste mohli nastavit:

- `Text` &ndash; text, který je zobrazený na prvním řádku velkými písmeny.
- `Detail` &ndash; text, který se zobrazí pod v prvním řádku, menší písmo.
- `TextColor` &ndash; Barva textu.
- `DetailColor` &ndash; Barva textu podrobností

![](customizing-cell-appearance-images/text-cell-default.png "Příklad TextCell výchozí")

![](customizing-cell-appearance-images/text-cell-custom.png "Příklad přizpůsobené TextCell")

<a name="ImageCell" />

### <a name="imagecell"></a>Funkce ImageCell

[`ImageCell`](http://developer.xamarin.com/api/type/Xamarin.Forms.ImageCell/), jako je `TextCell`, lze použít pro zobrazení textu a sekundární podrobností text a nabízí vysoký výkon pomocí nativní ovládací prvky každou platformu. `ImageCell` se liší od `TextCell` v, že se zobrazí obrázek vlevo od textu.

`ImageCell` je užitečné, když je třeba zobrazit seznam dat pomocí visual aspekt, jako je například seznam kontaktů nebo filmy. ImageCells lze přizpůsobit, abyste mohli nastavit:

- `Text` &ndash; text, který je zobrazený na prvním řádku velkými písmeny
- `Detail` &ndash; text, který se zobrazí pod v prvním řádku, menší písma
- `TextColor` &ndash; Barva textu
- `DetailColor` &ndash; Barva textu podrobností
- `ImageSource` &ndash; obrázek, který má zobrazit vedle textu

![](customizing-cell-appearance-images/image-cell-default.png "Výchozí funkce ImageCell příklad")

![](customizing-cell-appearance-images/image-cell-custom.png "Příklad přizpůsobené funkce ImageCell")

<a name="customcells" />

## <a name="custom-cells"></a>Vlastní buněk
Při integrované buněk neposkytují na požadované rozložení, implementovat vlastní buněk na požadované rozložení. Můžete například představovat buňku dva popisky, které mají stejnou váhu. A `LabelCell` by dostatek protože `LabelCell` má jeden štítek, který je menší. Většina přizpůsobení buněk přidat další data jen pro čtení (například další popisky, obrázky nebo jiné zobrazované informace).

Všechny vlastní buňky musí být odvozeny od [ `ViewCell` ](http://developer.xamarin.com/api/type/Xamarin.Forms.ViewCell/), že všechny buňky předdefinované typy použijte stejnou základní třídu.

Xamarin.Forms 2 zavedl nový [chování ukládání do mezipaměti](~/xamarin-forms/user-interface/listview/performance.md#cachingstrategy) na `ListView` řídit, která může být nastaven na posouvání výkon pro některé typy vlastní buněk.

Jedná se o příklad vlastní buňky:

![](customizing-cell-appearance-images/custom-cell.png "Příklad vlastní buňky")

### <a name="xaml"></a>XAML
Zde je XAML pro vytvoření rozložení výše:

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

Výše uvedené XAML provádí mnoho. Můžeme rozdělit ho:

- Je vnořit vlastní buňky `DataTemplate`, který je uvnitř `ListView.ItemTemplate`. Toto je stejný postup jako při použití jiných buňky.
- `ViewCell` je typ vlastní buňky. Podřízeným `DataTemplate` element musí být nebo odvozen z typu `ViewCell`.
- Všimněte si, že uvnitř `ViewCell`, spravuje rozložení `StackLayout`. Toto rozložení umožňuje přizpůsobit barvu pozadí. Všimněte si, že žádnou vlastnost `StackLayout` tedy vazbu může být vázána v buňce vlastní, i když, není zde zobrazený.

### <a name="cnum"></a>C&num;

Určení vlastních buňky v jazyce C# je trochu podrobnější než ekvivalentní XAML. Podívejme se:

Nejprve definovat třídu vlastní buňky s `ViewCell` jako základní třída:

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

Ve vašem konstruktor pro stránku s `ListView`, nastavte ListView `ItemTemplate` vlastnost na nový `DataTemplate`:

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

Všimněte si, že v konstruktoru pro `DataTemplate` trvá typu. Získá typ CLR pro operátor typeof `CustomCell`.

<a name="binding-context-changes" />

### <a name="binding-context-changes"></a>Vazba kontextové změny

Při vytváření vazby na vlastní buňky typ [ `BindableProperty` ](https://developer.xamarin.com/api/type/Xamarin.Forms.BindableProperty/) instance ovládacích prvků uživatelského rozhraní zobrazení `BindableProperty` hodnoty by měl používat [ `OnBindingContextChanged` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Cell.OnBindingContextChanged()/) přepsání nastavení dat, který se má zobrazit v Každá buňka, nikoli buňky konstruktoru, jak je ukázáno v následujícím příkladu kódu:

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

[ `OnBindingContextChanged` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Cell.OnBindingContextChanged()/) Přepsání bude volána při [ `BindingContextChanged` ](https://developer.xamarin.com/api/event/Xamarin.Forms.BindableObject.BindingContextChanged/) aktivuje událost v reakci na hodnotu [ `BindingContext` ](https://developer.xamarin.com/api/property/Xamarin.Forms.BindableObject.BindingContext/) změna vlastností. Proto když `BindingContext` změní, ovládacích prvků uživatelského rozhraní zobrazení [ `BindableProperty` ](https://developer.xamarin.com/api/type/Xamarin.Forms.BindableProperty/) hodnoty by měly nastavit svá data. Všimněte si, že `BindingContext` zkontrolovat `null` hodnota, protože to je možné nastavit pomocí Xamarin.Forms pro uvolňování paměti, který pak bude mít za následek `OnBindingContextChanged` přepsat volána.

Alternativně můžete vázat ovládacích prvků uživatelského rozhraní na [ `BindableProperty` ](https://developer.xamarin.com/api/type/Xamarin.Forms.BindableProperty/) instance, které chcete zobrazit jejich hodnoty, které eliminuje nutnost přepsat `OnBindingContextChanged` metoda.

> [!NOTE]
> Při přepsání `OnBindingContextChanged`, ujistěte se, že základní třídy `OnBindingContextChanged` metoda je volána, aby registrovaná delegáti `BindingContextChanged` událostí.

V jazyce XAML typ vlastní buňky vytvoření vazby na data lze dosáhnout jak je znázorněno v následujícím příkladu kódu:

```xaml
<ListView x:Name="listView">
    <ListView.ItemTemplate>
        <DataTemplate>
            <local:CustomCell Name="{Binding Name}" Age="{Binding Age}" Location="{Binding Location}" />
        </DataTemplate>
    </ListView.ItemTemplate>
</ListView>
```

Tím je vytvořena vazba `Name`, `Age`, a `Location` vazbu vlastnosti v `CustomCell` instance na `Name`, `Age`, a `Location` vlastnosti každého objektu v podkladové kolekci.

Ekvivalentní vazby v jazyce C# je znázorněno v následujícím příkladu kódu:

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

Na iOS a Android Pokud [ `ListView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/) je recyklace elementy a vlastní buňky používá vlastní zobrazovací jednotky, vlastní zobrazovací jednotky musí správně implementovat oznámení o změně vlastností. Když jsou opakovaně buněk jejich hodnoty vlastností se změní při aktualizaci kontextu vazby na k dispozici buňky, který se `PropertyChanged` událostí vyvolaných. Další informace najdete v tématu [přizpůsobení ViewCell](~/xamarin-forms/app-fundamentals/custom-renderer/viewcell.md). Další informace o buňky recyklace najdete v tématu [ukládání do mezipaměti strategie](~/xamarin-forms/user-interface/listview/performance.md#cachingstrategy).

## <a name="related-links"></a>Související odkazy

- [Vytvořené v buňkách (ukázka)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/ListView/BuiltInCells)
- [Vlastní buněk (ukázka)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/ListView/CustomCells)
- [Vazba kontext změnit (ukázka)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/ListView/BindingContextChanged)
