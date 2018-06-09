---
title: Xamarin.Forms BoxView
description: Tento článek vysvětluje, jak používat barevného obdélníku pro dekorace, grafiky a interakce v aplikaci Xamarin.Forms.
ms.prod: xamarin
ms.assetid: 4CBF703D-84A0-4CDF-A433-5926B587782A
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 12/07/2017
ms.openlocfilehash: edb2785362f2cc7377d9adb0c1a89a6fa2b08111
ms.sourcegitcommit: 66682dd8e93c0e4f5dee69f32b5fc5a96443e307
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 06/08/2018
ms.locfileid: "35244312"
---
# <a name="xamarinforms-boxview"></a>Xamarin.Forms BoxView

[`BoxView`](https://developer.xamarin.com/api/type/Xamarin.Forms.BoxView/) vykreslí jednoduché obdélník zadaný šířky, výšky a barvy. Můžete použít `BoxView` pro dekorace, elementární grafiky a pro interakci s uživatelem prostřednictvím dotykového ovládání.

Protože Xamarin.Forms nemá systému předdefinované vektorové grafiky `BoxView` pomáhá odpovídajícím způsobem. Některé programy ukázka popsané v tomto článku `BoxView` pro vykreslování grafiky. `BoxView` Můžete velikost tak, aby připomínaly řádku konkrétní šířky a tloušťka a pak otáčet o jakékoli úhel pomocí `Rotation` vlastnost.

I když `BoxView` mohou napodobovat jednoduché grafiky, můžete chtít prozkoumat [pomocí SkiaSharp v Xamarin.Forms](~/xamarin-forms/user-interface/graphics/skiasharp/index.md) pro sofistikovanější grafiky požadavky.

Tento článek popisuje v následujících tématech:

- **[Nastavení barvy BoxView a velikost](#colorandsize)**  &ndash; nastavit `BoxView` vlastnosti.
- **[Vykreslování textové dekorace](#textdecorations)**  &ndash; používat `BoxView` pro vykreslování řádky.
- **[Výpis barvy s BoxView](#listingcolors)**  &ndash; zobrazení všech systému barvy v `ListView`.
- **[Přehrávání herní dobu životnosti ve vytváření podtříd BoxView](#subclassing)**  &ndash; implementovat famous mobilní automaton.
- **[Vytváření digitální hodiny](#digitalclock)**  &ndash; simulovat maticové zobrazení.
- **[Vytváření analogovým hodiny](#analogclock)**  &ndash; transformace a použije animaci `BoxView` elementy.

<a name="colorandsize" />

## <a name="setting-boxview-color-and-size"></a>Nastavení BoxView barvy a velikosti

Velmi často budete nastavíte následující tři vlastnosti `BoxView`:

- [`Color`](https://developer.xamarin.com/api/property/Xamarin.Forms.BoxView.Color/) Chcete-li nastavit jeho barev.
- [`WidthRequest`](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.WidthRequest/) Chcete-li nastavit šířku `BoxView` v jednotkách nezávislé na zařízení.
- [`HeightRequest`](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.HeightRequest/) Chcete-li nastavit výšku `BoxView`.

`Color` Vlastnost je typu `Color`; vlastnost můžete nastavit na libovolnou `Color` hodnotu, včetně 141 statické jen pro čtení pole s názvem barvy abecedně od `AliceBlue` k `YellowGreen`.

`WidthRequest` a `HeightRequest` vlastnosti jenom hrají roli, pokud `BoxView` je *neomezeným* v rozložení. To je případ, kdy kontejner rozložení musí znát, podřízená je upravit velikost, například když `BoxView` je podřízená Automatická velikost buňky v `Grid` rozložení. A `BoxView` je také neomezeným při jeho `HorizontalOptions` a `VerticalOptions` vlastnosti jsou nastaveny na hodnoty jiné než `LayoutOptions.Fill`. Pokud `BoxView` neomezeným, ale `WidthRequest` a `HeightRequest` nejsou nastaveny vlastnosti a potom šířky nebo výšky jsou nastaveny na výchozí hodnoty 40 jednotky nebo o 1/4 palce na mobilních zařízeních.

`WidthRequest` a `HeightRequest` vlastnosti jsou ignorovány, pokud `BoxView` je *omezené* v rozložení, ve kterém případ kontejneru rozložení ukládá na vlastní velikost `BoxView`.

A `BoxView` může být omezené v jednou dimenzí a neomezeného v dalších. Například pokud `BoxView` je podřízená svislého `StackLayout`, svislé dimenzi `BoxView` je neomezeného a jeho vodorovné dimenze je obvykle omezené. Existují však výjimky pro daná vodorovném dimenze: Pokud `BoxView` má jeho `HorizontalOptions` vlastnost nastavena na jinou hodnotu než `LayoutOptions.Fill`, pak vodorovné dimenze je také neomezeným. Je také možné, `StackLayout` samotné tak, aby měl neomezeným vodorovné dimenze, v takovém případě `BoxView` bude také vodorovně neomezeným.

[ **BasicBoxView** ](https://developer.xamarin.com/samples/xamarin-forms/BoxView/BasicBoxView) ukázka zobrazuje jeden palec čtverce neomezeného `BoxView` v centru jeho stránky:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:local="clr-namespace:BasicBoxView"
             x:Class="BasicBoxView.MainPage">

    <BoxView Color="CornflowerBlue"
             WidthRequest="160"
             HeightRequest="160"
             VerticalOptions="Center"
             HorizontalOptions="Center" />

</ContentPage>
```

Tady je výsledek:

[![Základní BoxView](boxview-images/basicboxview-small.png "základní BoxView")](boxview-images/basicboxview-large.png#lightbox "BasicBoxView")

Pokud `VerticalOptions` a `HorizontalOptions` vlastnosti jsou odebrány z `BoxView` značka nebo se nastaví na `Fill`, pak se `BoxView` stane omezené velikost stránky a rozbalí k zaplnění stránky.

A `BoxView` může být také podřízenou `AbsoluteLayout`. V takovém případě umístění a velikost `BoxView` jsou nastavené pomocí `LayoutBounds` připojené vazbu vlastnosti. `AbsoluteLayout` Je popsána v článku [ **AbsoluteLayout**](~/xamarin-forms/user-interface/layouts/absolute-layout.md).

Zobrazí se všechny tyto případy v ukázkové aplikace, které následují příklady.

<a name="textdecorations" />

## <a name="rendering-text-decorations"></a>Dekorace vykreslování textu

Můžete použít `BoxView` přidat některé jednoduché dekorace na vaše stránky ve formě vodorovného a svislého řádky. [ **TextDecoration** ](https://developer.xamarin.com/samples/xamarin-forms/BoxView/TextDecoration) příklad znázorňuje to. Všechny vizuály programu jsou definovány v **MainPage.xaml** soubor, který obsahuje několik `Label` a `BoxView` elementů v `StackLayout` znázorněno zde:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:local="clr-namespace:TextDecoration"
             x:Class="TextDecoration.MainPage">
    <ContentPage.Padding>
        <OnPlatform x:TypeArguments="Thickness">
            <On Platform="iOS" Value="0, 20, 0, 0" />
        </OnPlatform>
    </ContentPage.Padding>

    <ContentPage.Resources>
        <ResourceDictionary>
            <Style TargetType="BoxView">
                <Setter Property="Color" Value="Black" />
            </Style>
        </ResourceDictionary>
    </ContentPage.Resources>

    <ScrollView Margin="15">
        <StackLayout>

            ···

        </StackLayout>
    </ScrollView>
</ContentPage>
```

Všechny značky následující podřízené objekty jsou `StackLayout`. Tento kód se skládá z několika typů dekorativní `BoxView` prvky používané s `Label` element:

[![Textové dekorace](boxview-images/textdecoration-small.png "textové dekorace")](boxview-images/textdecoration-large.png#lightbox "textové dekorace")

Stylových hlavičky v horní části stránky se dosáhne s `AbsoluteLayout` jehož podřízené objekty jsou čtyři `BoxView` elementy a `Label`, všechny z které jsou přiřazeny konkrétní umístění a velikost:

```xaml
<AbsoluteLayout>
    <BoxView AbsoluteLayout.LayoutBounds="0, 10, 200, 5" />
    <BoxView AbsoluteLayout.LayoutBounds="0, 20, 200, 5" />
    <BoxView AbsoluteLayout.LayoutBounds="10, 0, 5, 65" />
    <BoxView AbsoluteLayout.LayoutBounds="20, 0, 5, 65" />
    <Label Text="Stylish Header"
           FontSize="24"
           AbsoluteLayout.LayoutBounds="30, 25, AutoSize, AutoSize"/>
</AbsoluteLayout>
```

V souboru XAML `AbsoluteLayout` následuje `Label` s formátovaný text, který popisuje `AbsoluteLayout`.

Textový řetězec můžete underline uzavřením i `Label` a `BoxView` v `StackLayout` s jeho `HorizontalOptions` hodnota nastavena na jinou hodnotu než `Fill`. Šířka `StackLayout` se pak řídí šířku `Label`, který pak ukládá na tuto šířku `BoxView`. `BoxView` Je přiřazena pouze explicitní výška:

```xaml
<StackLayout HorizontalOptions="Center">
    <Label Text="Underlined Text"
           FontSize="24" />
    <BoxView HeightRequest="2" />
</StackLayout>
```

Tímto způsobem nelze underline jednotlivých slov v textové řetězce delší nebo odstavec.

Je také možné použít `BoxView` tak, aby připomínaly HTML `hr` – element (vodorovné pravítko). Jednoduše umožní šířku `BoxView` určí na základě jeho nadřazený kontejner, který v tomto případě `StackLayout`:

```xaml
<BoxView HeightRequest="3" />
```

Nakonec kreslení svislé čáry na jedné straně odstavec textu uzavřením i `BoxView` a `Label` vodorovně `StackLayout`. V tomto případě výšku `BoxView` je stejný jako výšku `StackLayout`, které se řídí výšku `Label`:

```xaml
<StackLayout Orientation="Horizontal">
    <BoxView WidthRequest="4"
             Margin="0, 0, 10, 0" />
    <Label>

        ···

    </Label>
/StackLayout>
```
<a name="listingcolors" />

## <a name="listing-colors-with-boxview"></a>Výpis barvy s BoxView

`BoxView` Je vhodné pro zobrazení barev. Tento program používá `ListView` seznam všech veřejných statických jen pro čtení pole platformě Xamarin.Forms `Color` strukturu:

[![Barvy ListView](boxview-images/listviewcolors-small.png "ListView barvy")](boxview-images/listviewcolors-large.png#lightbox "ListView barvy")

[ **ListViewColors** ](https://developer.xamarin.com/samples/xamarin-forms/BoxView/ListViewColors/) program zahrnuje třídy s názvem `NamedColor`. Statický konstruktor reflexe používá pro přístup k všechna pole `Color` struktury a vytvořte `NamedColor` objekt pro každé z nich. Tyto jsou uložené v statických `All` vlastnost:

```csharp
public class NamedColor
{
    // Instance members.
    private NamedColor()
    {
    }

    public string Name { private set; get; }

    public string FriendlyName { private set; get; }

    public Color Color { private set; get; }

    public string RgbDisplay { private set; get; }

    // Static members.
    static NamedColor()
    {
        List<NamedColor> all = new List<NamedColor>();
        StringBuilder stringBuilder = new StringBuilder();

        // Loop through the public static fields of the Color structure.
        foreach (FieldInfo fieldInfo in typeof(Color).GetRuntimeFields ())
        {
            if (fieldInfo.IsPublic &&
                fieldInfo.IsStatic &&
                fieldInfo.FieldType == typeof (Color))
            {
                // Convert the name to a friendly name.
                string name = fieldInfo.Name;
                stringBuilder.Clear();
                int index = 0;

                foreach (char ch in name)
                {
                    if (index != 0 && Char.IsUpper(ch))
                    {
                        stringBuilder.Append(' ');
                    }
                    stringBuilder.Append(ch);
                    index++;
                }

                // Instantiate a NamedColor object.
                Color color = (Color)fieldInfo.GetValue(null);

                NamedColor namedColor = new NamedColor
                {
                    Name = name,
                    FriendlyName = stringBuilder.ToString(),
                    Color = color,
                    RgbDisplay = String.Format("{0:X2}-{1:X2}-{2:X2}",
                                               (int)(255 * color.R),
                                               (int)(255 * color.G),
                                               (int)(255 * color.B))
                };

                // Add it to the collection.
                all.Add(namedColor);
            }
        }
        all.TrimExcess();
        All = all;
    }

    public static IList<NamedColor> All { private set; get; }
}
```

Vizuály programu jsou popsané v souboru XAML. `ItemsSource` Vlastnost `ListView` nastavena na statickou `NamedColor.All` vlastnost, což znamená, že `ListView` zobrazuje všechny jednotlivý `NamedColor` objekty:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:local="clr-namespace:ListViewColors"
             x:Class="ListViewColors.MainPage">
    <ContentPage.Padding>
        <OnPlatform x:TypeArguments="Thickness">
            <On Platform="iOS" Value="10, 20, 10, 0" />
            <On Platform="Android, UWP" Value="10, 0" />
        </OnPlatform>
    </ContentPage.Padding>

    <ListView SeparatorVisibility="None"
              ItemsSource="{x:Static local:NamedColor.All}">
        <ListView.RowHeight>
            <OnPlatform x:TypeArguments="x:Int32">
                <On Platform="iOS, Android" Value="80" />
                <On Platform="UWP" Value="90" />
            </OnPlatform>
        </ListView.RowHeight>

        <ListView.ItemTemplate>
            <DataTemplate>
                <ViewCell>
                    <ContentView Padding="5">
                        <Frame OutlineColor="Accent"
                               Padding="10">
                            <StackLayout Orientation="Horizontal">
                                <BoxView Color="{Binding Color}"
                                         WidthRequest="50"
                                         HeightRequest="50" />
                                <StackLayout>
                                    <Label Text="{Binding FriendlyName}"
                                           FontSize="22"
                                           VerticalOptions="StartAndExpand" />
                                    <Label Text="{Binding RgbDisplay, StringFormat='RGB = {0}'}"
                                           FontSize="16"
                                           VerticalOptions="CenterAndExpand" />
                                </StackLayout>
                            </StackLayout>
                        </Frame>
                    </ContentView>
                </ViewCell>
            </DataTemplate>
        </ListView.ItemTemplate>
    </ListView>
</ContentPage>
```

`NamedColor` Objekty jsou formátovány podle `ViewCell` objekt, který je nastaven jako šablonu dat z `ListView`. Tato šablona zahrnuje `BoxView` jejichž `Color` vlastnost je vázána na `Color` vlastnost `NamedColor` objektu.

<a name="subclassing" />

## <a name="playing-the-game-of-life-by-subclassing-boxview"></a>Hru životnosti ve vytváření podtříd BoxView

Herní životnosti je mobilní automaton vyvinuta firmou matematikovi Conwayovu Jan a popularized na stránkách *Scientific American* v 1970s. Dobrý Úvod je uveden článek Wikipedia [na Conwayovu herní životnosti](https://en.wikipedia.org/wiki/Conway%27s_Game_of_Life).

Platformě Xamarin.Forms [ **GameOfLife** ](https://developer.xamarin.com/samples/xamarin-forms/BoxView/GameOfLife/) program definuje třídu s názvem `LifeCell` která je odvozena od `BoxView`. Tato třída zapouzdří logiku jednotlivých buněk v herním životnosti:

```csharp
class LifeCell : BoxView
{
    bool isAlive;

    public event EventHandler Tapped;

    public LifeCell()
    {
        BackgroundColor = Color.White;

        TapGestureRecognizer tapGesture = new TapGestureRecognizer();
        tapGesture.Tapped += (sender, args) =>
        {
            Tapped?.Invoke(this, EventArgs.Empty);
        };
        GestureRecognizers.Add(tapGesture);
    }

    public int Col { set; get; }

    public int Row { set; get; }

    public bool IsAlive
    {
        set
        {
            if (isAlive != value)
            {
                isAlive = value;
                BackgroundColor = isAlive ? Color.Black : Color.White;
            }
        }
        get
        {
            return isAlive;
        }
    }
}
```

`LifeCell` Přidá tři další vlastnosti pro `BoxView`: `Col` a `Row` vlastnosti ukládání pozici buňky v mřížce a `IsAlive` vlastnost označuje jeho stav. `IsAlive` Také nastaví vlastnost `Color` vlastnost `BoxView` do černé, je-li buňka aktivní a bílé, pokud buňka není aktivní.

`LifeCell` nainstaluje taky `TapGestureRecognizer` chcete umožnit uživatelům přepnutí stavu buněk je klepnutím. Třída přeloží `Tapped` událost z rozpoznávání rukopisu gesto do vlastní `Tapped` událostí.

**GameOfLife** program také zahrnuje `LifeGrid` třídy, který zapouzdřuje velkou část logiky ve hře, a `MainPage` třída, která zpracovává vizuály programu. Mezi ně patří překrytí, který popisuje pravidla hry. Tady je program v akci zobrazující několik set `LifeCell` objekty na stránce:

[![Herní života](boxview-images/gameoflife-small.png "herní životní")](boxview-images/gameoflife-large.png#lightbox "herní životnosti")

<a name="digitalclock" />

## <a name="creating-a-digital-clock"></a>Vytváření digitální hodiny

[ **DotMatrixClock** ](https://developer.xamarin.com/samples/xamarin-forms/BoxView/DotMatrixClock/) program vytvoří 210 `BoxView` elementy k simulaci tečky stejné 5 7 maticové zobrazení. Můžete si přečíst čas v režimu na výšku nebo na šířku, ale je větší v na šířku:

[![Maticové hodiny](boxview-images/dotmatrixclock-small.png "maticové hodiny")](boxview-images/dotmatrixclock-large.png#lightbox "maticové hodiny")

Vytvoření souboru XAML trochu více než instance `AbsoluteLayout` používá pro hodiny:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:local="clr-namespace:DotMatrixClock"
             x:Class="DotMatrixClock.MainPage"
             Padding="10"
             SizeChanged="OnPageSizeChanged">

    <AbsoluteLayout x:Name="absoluteLayout"
                    VerticalOptions="Center" />
</ContentPage>
```

Všem ostatním dojde v souboru kódu na pozadí. Maticové logiku zobrazení je výrazně jednodušší podle definice několik polí, které popisují tečky odpovídající každé z 10 číslic a dvojtečky:

```csharp
public partial class MainPage : ContentPage
{
    // Total dots horizontally and vertically.
    const int horzDots = 41;
    const int vertDots = 7;

    // 5 x 7 dot matrix patterns for 0 through 9.
    static readonly int[, ,] numberPatterns = new int[10, 7, 5]
    {
        {
            { 0, 1, 1, 1, 0}, { 1, 0, 0, 0, 1}, { 1, 0, 0, 1, 1}, { 1, 0, 1, 0, 1},
            { 1, 1, 0, 0, 1}, { 1, 0, 0, 0, 1}, { 0, 1, 1, 1, 0}
        },
        {
            { 0, 0, 1, 0, 0}, { 0, 1, 1, 0, 0}, { 0, 0, 1, 0, 0}, { 0, 0, 1, 0, 0},
            { 0, 0, 1, 0, 0}, { 0, 0, 1, 0, 0}, { 0, 1, 1, 1, 0}
        },
        {
            { 0, 1, 1, 1, 0}, { 1, 0, 0, 0, 1}, { 0, 0, 0, 0, 1}, { 0, 0, 0, 1, 0},
            { 0, 0, 1, 0, 0}, { 0, 1, 0, 0, 0}, { 1, 1, 1, 1, 1}
        },
        {
            { 1, 1, 1, 1, 1}, { 0, 0, 0, 1, 0}, { 0, 0, 1, 0, 0}, { 0, 0, 0, 1, 0},
            { 0, 0, 0, 0, 1}, { 1, 0, 0, 0, 1}, { 0, 1, 1, 1, 0}
        },
        {
            { 0, 0, 0, 1, 0}, { 0, 0, 1, 1, 0}, { 0, 1, 0, 1, 0}, { 1, 0, 0, 1, 0},
            { 1, 1, 1, 1, 1}, { 0, 0, 0, 1, 0}, { 0, 0, 0, 1, 0}
        },
        {
            { 1, 1, 1, 1, 1}, { 1, 0, 0, 0, 0}, { 1, 1, 1, 1, 0}, { 0, 0, 0, 0, 1},
            { 0, 0, 0, 0, 1}, { 1, 0, 0, 0, 1}, { 0, 1, 1, 1, 0}
        },
        {
            { 0, 0, 1, 1, 0}, { 0, 1, 0, 0, 0}, { 1, 0, 0, 0, 0}, { 1, 1, 1, 1, 0},
            { 1, 0, 0, 0, 1}, { 1, 0, 0, 0, 1}, { 0, 1, 1, 1, 0}
        },
        {
            { 1, 1, 1, 1, 1}, { 0, 0, 0, 0, 1}, { 0, 0, 0, 1, 0}, { 0, 0, 1, 0, 0},
            { 0, 1, 0, 0, 0}, { 0, 1, 0, 0, 0}, { 0, 1, 0, 0, 0}
        },
        {
            { 0, 1, 1, 1, 0}, { 1, 0, 0, 0, 1}, { 1, 0, 0, 0, 1}, { 0, 1, 1, 1, 0},
            { 1, 0, 0, 0, 1}, { 1, 0, 0, 0, 1}, { 0, 1, 1, 1, 0}
        },
        {
            { 0, 1, 1, 1, 0}, { 1, 0, 0, 0, 1}, { 1, 0, 0, 0, 1}, { 0, 1, 1, 1, 1},
            { 0, 0, 0, 0, 1}, { 0, 0, 0, 1, 0}, { 0, 1, 1, 0, 0}
        },
    };

    // Dot matrix pattern for a colon.
    static readonly int[,] colonPattern = new int[7, 2]
    {
        { 0, 0 }, { 1, 1 }, { 1, 1 }, { 0, 0 }, { 1, 1 }, { 1, 1 }, { 0, 0 }
    };

    // BoxView colors for on and off.
    static readonly Color colorOn = Color.Red;
    static readonly Color colorOff = new Color(0.5, 0.5, 0.5, 0.25);

    // Box views for 6 digits, 7 rows, 5 columns.
    BoxView[, ,] digitBoxViews = new BoxView[6, 7, 5];

    ···

}
```

Tato pole uzavřít s trojrozměrné z `BoxView` prvky pro ukládání tečkou vzory pro šesti číslic.

V konstruktoru vytvoří všechny `BoxView` elementy pro číslice a dvojtečky a také inicializuje `Color` vlastnost `BoxView` prvky pro dvojtečkou:

```csharp
public partial class MainPage : ContentPage
{

    ···

    public MainPage()
    {
        InitializeComponent();

        // BoxView dot dimensions.
        double height = 0.85 / vertDots;
        double width = 0.85 / horzDots;

        // Create and assemble the BoxViews.
        double xIncrement = 1.0 / (horzDots - 1);
        double yIncrement = 1.0 / (vertDots - 1);
        double x = 0;

        for (int digit = 0; digit < 6; digit++)
        {
            for (int col = 0; col < 5; col++)
            {
                double y = 0;

                for (int row = 0; row < 7; row++)
                {
                    // Create the digit BoxView and add to layout.
                    BoxView boxView = new BoxView();
                    digitBoxViews[digit, row, col] = boxView;
                    absoluteLayout.Children.Add(boxView,
                                                new Rectangle(x, y, width, height),
                                                AbsoluteLayoutFlags.All);
                    y += yIncrement;
                }
                x += xIncrement;
            }
            x += xIncrement;

            // Colons between the hours, minutes, and seconds.
            if (digit == 1 || digit == 3)
            {
                int colon = digit / 2;

                for (int col = 0; col < 2; col++)
                {
                    double y = 0;

                    for (int row = 0; row < 7; row++)
                    {
                        // Create the BoxView and set the color.
                        BoxView boxView = new BoxView
                            {
                                Color = colonPattern[row, col] == 1 ?
                                            colorOn : colorOff
                            };
                        absoluteLayout.Children.Add(boxView,
                                                    new Rectangle(x, y, width, height),
                                                    AbsoluteLayoutFlags.All);
                        y += yIncrement;
                    }
                    x += xIncrement;
                }
                x += xIncrement;
            }
        }

        // Set the timer and initialize with a manual call.
        Device.StartTimer(TimeSpan.FromSeconds(1), OnTimer);
        OnTimer();
    }

    ···

}
```

Tento program používá relativní umístění a velikost funkce `AbsoluteLayout`. Šířka a výška jednotlivých `BoxView` jsou nastaveny na desetinné číslo, konkrétně 85 % 1 rozdělené podle počtu vodorovného a svislého tečky. Pozice jsou nastaveny také pro desetinné číslo.

Vzhledem k tomu, že pozice a velikosti jsou relativní vzhledem k celková velikost `AbsoluteLayout`, `SizeChanged` obslužné rutiny pro stránky třeba nastavit pouze `HeightRequest` z `AbsoluteLayout`:

```csharp
public partial class MainPage : ContentPage
{

    ···

    void OnPageSizeChanged(object sender, EventArgs args)
    {
        // No chance a display will have an aspect ratio > 41:7
        absoluteLayout.HeightRequest = vertDots * Width / horzDots;
    }

    ···

}
```

Šířka `AbsoluteLayout` bude automaticky nastavena, protože ji roztahovány na celou šířku stránky.

Poslední kód `MainPage` třída zpracovává zpětné volání časovače a barvy tečky každý číslice. Definice vícerozměrných polí na začátku souboru kódu na pozadí pomáhá zkontrolujte tuto logiku nejjednodušší součást programu:

```csharp
public partial class MainPage : ContentPage
{

    ···

    bool OnTimer()
    {
        DateTime dateTime = DateTime.Now;

        // Convert 24-hour clock to 12-hour clock.
        int hour = (dateTime.Hour + 11) % 12 + 1;

        // Set the dot colors for each digit separately.
        SetDotMatrix(0, hour / 10);
        SetDotMatrix(1, hour % 10);
        SetDotMatrix(2, dateTime.Minute / 10);
        SetDotMatrix(3, dateTime.Minute % 10);
        SetDotMatrix(4, dateTime.Second / 10);
        SetDotMatrix(5, dateTime.Second % 10);
        return true;
    }

    void SetDotMatrix(int index, int digit)
    {
        for (int row = 0; row < 7; row++)
            for (int col = 0; col < 5; col++)
            {
                bool isOn = numberPatterns[digit, row, col] == 1;
                Color color = isOn ? colorOn : colorOff;
                digitBoxViews[index, row, col].Color = color;
            }
    }
}
```
<a name="analogclock" />

## <a name="creating-an-analog-clock"></a>Vytváření analogovým hodiny

Maticové hodiny zdát zřejmé aplikace `BoxView`, ale `BoxView` prvky jsou také schopná porozumění analogovým hodiny:

[![Hodiny BoxView](boxview-images/boxviewclock-small.png "BoxView hodiny")](boxview-images/boxviewclock-large.png#lightbox "BoxView hodiny")

Všech vizuálů na [ **BoxViewClock** ](https://developer.xamarin.com/samples/xamarin-forms/BoxView/BoxViewClock/) programu jsou podřízené `AbsoluteLayout`. Tyto prvky jsou dimenzované pomocí `LayoutBounds` přidružená vlastnost a otáčet pomocí `Rotation` vlastnost.

Tří `BoxView` prvky pro do nesprávných rukou hodiny nejsou vytvořena instance v souboru XAML, ale umístěný nebo velikost:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:local="clr-namespace:BoxViewClock"
             x:Class="BoxViewClock.MainPage">
    <ContentPage.Padding>
        <OnPlatform x:TypeArguments="Thickness">
            <On Platform="iOS" Value="0, 20, 0, 0" />
        </OnPlatform>
    </ContentPage.Padding>

    <AbsoluteLayout x:Name="absoluteLayout"
                    SizeChanged="OnAbsoluteLayoutSizeChanged">

        <BoxView x:Name="hourHand"
                 Color="Black" />

        <BoxView x:Name="minuteHand"
                 Color="Black" />

        <BoxView x:Name="secondHand"
                 Color="Black" />
    </AbsoluteLayout>
</ContentPage>
```

Vytvoří konstruktoru souboru kódu 60 `BoxView` elementy značek kolem obvodu hodin:

```csharp
public partial class MainPage : ContentPage
{

    ···

    BoxView[] tickMarks = new BoxView[60];

    public MainPage()
    {
        InitializeComponent();

        // Create the tick marks (to be sized and positioned later).
        for (int i = 0; i < tickMarks.Length; i++)
        {
            tickMarks[i] = new BoxView { Color = Color.Black };
            absoluteLayout.Children.Add(tickMarks[i]);
        }

        Device.StartTimer(TimeSpan.FromSeconds(1.0 / 60), OnTimerTick);
    }

    ···

}
```

Velikost a umístění všech `BoxView` elementy v dojde `SizeChanged` obslužné rutiny pro `AbsoluteLayout`. Trochu struktura interní k třídě volá `HandParams` popisuje velikost každého ze tří rukou relativně k celková velikost hodiny:

```csharp
public partial class MainPage : ContentPage
{
    // Structure for storing information about the three hands.
    struct HandParams
    {
        public HandParams(double width, double height, double offset) : this()
        {
            Width = width;
            Height = height;
            Offset = offset;
        }

        public double Width { private set; get; }   // fraction of radius
        public double Height { private set; get; }  // ditto
        public double Offset { private set; get; }  // relative to center pivot
    }

    static readonly HandParams secondParams = new HandParams(0.02, 1.1, 0.85);
    static readonly HandParams minuteParams = new HandParams(0.05, 0.8, 0.9);
    static readonly HandParams hourParams = new HandParams(0.125, 0.65, 0.9);

    ···

 }
```

`SizeChanged` Obslužná rutina určuje center a úhlu `AbsoluteLayout`a pak velikosti a umisťuje 60 `BoxView` prvky používané jako osové značky. `for` Smyčky ukončí nastavení `Rotation` vlastnost každé z nich `BoxView` elementy. Na konci `SizeChanged` obslužnou rutinu, `LayoutHand` metoda je volána velikosti a umístění do tří rukou hodiny:

```csharp
public partial class MainPage : ContentPage
{

    ···

    void OnAbsoluteLayoutSizeChanged(object sender, EventArgs args)
    {
        // Get the center and radius of the AbsoluteLayout.
        Point center = new Point(absoluteLayout.Width / 2, absoluteLayout.Height / 2);
        double radius = 0.45 * Math.Min(absoluteLayout.Width, absoluteLayout.Height);

        // Position, size, and rotate the 60 tick marks.
        for (int index = 0; index < tickMarks.Length; index++)
        {
            double size = radius / (index % 5 == 0 ? 15 : 30);
            double radians = index * 2 * Math.PI / tickMarks.Length;
            double x = center.X + radius * Math.Sin(radians) - size / 2;
            double y = center.Y - radius * Math.Cos(radians) - size / 2;
            AbsoluteLayout.SetLayoutBounds(tickMarks[index], new Rectangle(x, y, size, size));
            tickMarks[index].Rotation = 180 * radians / Math.PI;
        }

        // Position and size the three hands.
        LayoutHand(secondHand, secondParams, center, radius);
        LayoutHand(minuteHand, minuteParams, center, radius);
        LayoutHand(hourHand, hourParams, center, radius);
    }

    void LayoutHand(BoxView boxView, HandParams handParams, Point center, double radius)
    {
        double width = handParams.Width * radius;
        double height = handParams.Height * radius;
        double offset = handParams.Offset;

        AbsoluteLayout.SetLayoutBounds(boxView,
            new Rectangle(center.X - 0.5 * width,
                          center.Y - offset * height,
                          width, height));

        // Set the AnchorY property for rotations.
        boxView.AnchorY = handParams.Offset;
    }

    ···

}
```

`LayoutHand` Metoda velikosti a umisťuje každé straně tak, aby odkazoval rovnou až do pozice, 12:00. Na konci metody `AnchorY` je nastavena na poloze odpovídající středu hodiny. To znamená středu otočení.

Do rukou otáčejí ve funkci zpětného volání časovače:

```csharp
public partial class MainPage : ContentPage
{

    ···

    bool OnTimerTick()
    {
        // Set rotation angles for hour and minute hands.
        DateTime dateTime = DateTime.Now;
        hourHand.Rotation = 30 * (dateTime.Hour % 12) + 0.5 * dateTime.Minute;
        minuteHand.Rotation = 6 * dateTime.Minute + 0.1 * dateTime.Second;

        // Do an animation for the second hand.
        double t = dateTime.Millisecond / 1000.0;

        if (t < 0.5)
        {
            t = 0.5 * Easing.SpringIn.Ease(t / 0.5);
        }
        else
        {
            t = 0.5 * (1 + Easing.SpringOut.Ease((t - 0.5) / 0.5));
        }

        secondHand.Rotation = 6 * (dateTime.Second + t);
        return true;
    }
}
```

Druhé straně považuje se může lišit: usnadnění funkce animace aby zdá se, že pohyb mechanických spíše než smooth. Na každé značky druhé straně vrátí trochu a pak overshoots svého cíle. Tato trocha kód přidá mnoho realism pohybu.

## <a name="conclusion"></a>Závěr

`BoxView` Zdát jednoduché na první, ale jako jste viděli, může být velmi flexibilní a můžete téměř reprodukovatelnými vizuály, které jsou obvykle možné jenom s vektorové grafiky. Složitější grafiky naleznete [pomocí SkiaSharp v Xamarin.Forms](~/xamarin-forms/user-interface/graphics/skiasharp/index.md).


## <a name="related-links"></a>Související odkazy

- [Základní BoxView (ukázka)](https://developer.xamarin.com/samples/xamarin-forms/BoxView/BasicBoxView)
- [Textové dekorace (ukázka)](https://developer.xamarin.com/samples/xamarin-forms/BoxView/TextDecoration)
- [Barva ListBox (ukázka)](https://developer.xamarin.com/samples/xamarin-forms/BoxView/ColorListBox)
- [Herní životnosti (ukázka)](https://developer.xamarin.com/samples/xamarin-forms/BoxView/GameOfLife)
- [Maticové hodin (ukázka)](https://developer.xamarin.com/samples/xamarin-forms/BoxView/DotMatrixClock)
- [Hodiny BoxView (ukázka)](https://developer.xamarin.com/samples/xamarin-forms/BoxView/BoxViewClock)
- [BoxView](https://developer.xamarin.com/api/type/Xamarin.Forms.BoxView/)
