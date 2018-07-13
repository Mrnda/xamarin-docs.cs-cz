---
title: Xamarin.Forms BoxView
description: Tento článek vysvětluje, jak používat barevný obdélník pro dekoraci, grafiku a interakce aplikace Xamarin.Forms.
ms.prod: xamarin
ms.assetid: 4CBF703D-84A0-4CDF-A433-5926B587782A
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 12/07/2017
ms.openlocfilehash: 813a913c2c2fb27456c9a489c73b16d5892c4b8d
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/12/2018
ms.locfileid: "38997049"
---
# <a name="xamarinforms-boxview"></a>Xamarin.Forms BoxView

[`BoxView`](xref:Xamarin.Forms.BoxView) vykreslí jednoduchý obdélník nastavená šířka, výška a barvu. Můžete použít `BoxView` pro dekoraci, základní grafiky a pro interakci s uživatelem prostřednictvím dotykové ovládání.

Protože Xamarin.Forms nemá žádné předdefinované vektorové grafiky systému, `BoxView` pomáhá odpovídajícím způsobem upravit. Některé z ukázkové aplikace popsané v tomto článku využívají `BoxView` pro vykreslování grafiky. `BoxView` Můžete velikost tak, aby připomínaly řádku s konkrétní šířkou a tloušťku a potom otočit o jakékoli úhel pomocí `Rotation` vlastnost.

I když `BoxView` mohou napodobovat jednoduché grafiky, můžete chtít prozkoumat [pomocí ve Skiasharpu v Xamarin.Forms](~/xamarin-forms/user-interface/graphics/skiasharp/index.md) pro sofistikovanější grafiky požadavky.

Tento článek popisuje v následujících tématech:

- **[Nastavení BoxView barvu a velikost](#colorandsize)**  &ndash; nastavit `BoxView` vlastnosti.
- **[Dekorace textu vykreslování](#textdecorations)**  &ndash; použít `BoxView` pro vykreslení čáry.
- **[Výpis barvy s BoxView](#listingcolors)**  &ndash; Zobrazit vše systémových barev v `ListView`.
- **[Přehrávání hru život tak vytváření podtříd BoxView](#subclassing)**  &ndash; implementovat slavných automaton mobilní sítě.
- **[Vytvoření digitální hodiny](#digitalclock)**  &ndash; simulovat jehličkové zobrazení.
- **[Vytvoření obdobu jmenovek hodiny](#analogclock)**  &ndash; transformaci a animovat `BoxView` elementy.

<a name="colorandsize" />

## <a name="setting-boxview-color-and-size"></a>Nastavení BoxView barvu a velikost

Velmi často nastavíte následující tři vlastnosti `BoxView`:

- [`Color`](xref:Xamarin.Forms.BoxView.Color) Chcete-li nastavit její barvu.
- [`WidthRequest`](xref:Xamarin.Forms.VisualElement.WidthRequest) Chcete-li nastavit šířku `BoxView` v jednotkách nezávislých na zařízení.
- [`HeightRequest`](xref:Xamarin.Forms.VisualElement.HeightRequest) Chcete-li nastavit výšku `BoxView`.

`Color` Vlastnost je typu `Color`; vlastnost lze nastavit na libovolnou `Color` hodnotu, včetně 141 statické pole jen pro čtení z pojmenované barvy podle abecedy od `AliceBlue` k `YellowGreen`.

`WidthRequest` a `HeightRequest` vlastnosti pouze hrají roli, pokud `BoxView` je *neomezeným* v rozložení. To je případ, kdy kontejner rozložení potřebuje vědět, podřízené uživatele upravit velikost, například když `BoxView` je podřízeným prvkem Automatická velikost buňky v `Grid` rozložení. A `BoxView` je také bez omezení při jeho `HorizontalOptions` a `VerticalOptions` vlastnosti jsou nastaveny na hodnoty jiné než `LayoutOptions.Fill`. Pokud `BoxView` je bez omezení, ale `WidthRequest` a `HeightRequest` nejsou nastaveny vlastnosti a pak šířky nebo výšky jsou nastaveny na výchozí hodnoty 40 jednotek nebo přibližně 1/4 palce na mobilních zařízeních.

`WidthRequest` a `HeightRequest` vlastnosti jsou ignorovány, pokud `BoxView` je *omezené* v rozložení, ve kterém případ kontejner rozložení ukládá své vlastní velikosti `BoxView`.

A `BoxView` můžete omezené v jedné dimenzi a vstupy bez omezení v jiném. Například pokud `BoxView` je podřízeným prvkem a jsou odděleny svislou `StackLayout`, svislé dimenze `BoxView` je vstupy bez omezení a jeho vodorovný rozměr je obvykle omezené. Ale existují výjimky pro tuto dimenzi vodorovné: Pokud `BoxView` má jeho `HorizontalOptions` nastavenou na něco jiného než `LayoutOptions.Fill`, vodorovný rozměr je také bez omezení. Je také možné, `StackLayout` samotný s neomezeným vodorovný rozměr, v takovém případě `BoxView` bude také vodorovně bez omezení.

[ **BasicBoxView** ](https://developer.xamarin.com/samples/xamarin-forms/BoxView/BasicBoxView) ukázka zobrazí jeden – palec – čtverec neomezeného `BoxView` ve středu jeho stránky:

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

Pokud `VerticalOptions` a `HorizontalOptions` odebráním vlastnosti odeberete z `BoxView` označení nebo se nastaví `Fill`, pak bude `BoxView` stane omezeny velikost stránky a roztáhne a vyplní stránky.

A `BoxView` může také být podřízenou `AbsoluteLayout`. V takovém případě umístění a velikost `BoxView` jsou `LayoutBounds` přidružená vlastnost podporující vazby. `AbsoluteLayout` Je popsán v článku [ **AbsoluteLayout**](~/xamarin-forms/user-interface/layouts/absolute-layout.md).

Zobrazí se vám všechny tyto případy v ukázkové programy, které následují příklady.

<a name="textdecorations" />

## <a name="rendering-text-decorations"></a>Vykreslování dekorace textu

Můžete použít `BoxView` přidat některé jednoduché dekorace na stránkách v podobě vodorovné a svislé čáry. [ **Textdecoration –** ](https://developer.xamarin.com/samples/xamarin-forms/BoxView/TextDecoration) příklad ukazuje to. Všechny vizuály programu jsou definovány v **MainPage.xaml** soubor, který obsahuje několik `Label` a `BoxView` prvky `StackLayout` je vidět tady:

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

Všechny značky, který následuje jsou podřízené `StackLayout`. Tato značka se skládá z několika typů dekorativní `BoxView` prvků, které slouží se `Label` element:

[![Textové dekorace](boxview-images/textdecoration-small.png "textové dekorace")](boxview-images/textdecoration-large.png#lightbox "dekorace textu")

Stylový záhlaví v horní části stránky se dosahuje prostřednictvím `AbsoluteLayout` jehož potomci jsou čtyři `BoxView` elementy a `Label`, všechny které jsou přiřazeny konkrétní umístění a velikosti:

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

Textový řetězec můžete podtržení uzavřením i `Label` a `BoxView` v `StackLayout` , který má jeho `HorizontalOptions` hodnota nastavená na něco jiného než `Fill`. Šířka `StackLayout` se pak řídí šířku `Label`, která ukládá tuto šířku `BoxView`. `BoxView` Je přiřazená pouze explicitní height:

```xaml
<StackLayout HorizontalOptions="Center">
    <Label Text="Underlined Text"
           FontSize="24" />
    <BoxView HeightRequest="2" />
</StackLayout>
```

Tímto způsobem nelze underline jednotlivých slov v rámci delší textové řetězce nebo odstavce.

Je také možné použít `BoxView` tak, aby připomínaly HTML `hr` – element (vodorovná čára). Jednoduše nechat šířku `BoxView` měli určit podle jeho nadřazeného kontejneru, který je v tomto případě `StackLayout`:

```xaml
<BoxView HeightRequest="3" />
```

Nakonec můžete nakreslit svislá čára na jedné straně odstavce textu uzavřením i `BoxView` a `Label` koleček vodorovnou `StackLayout`. V tomto případě výšku `BoxView` je stejný jako výška `StackLayout`, které se vztahují výšku `Label`:

```xaml
<StackLayout Orientation="Horizontal">
    <BoxView WidthRequest="4"
             Margin="0, 0, 10, 0" />
    <Label>

        ···

    </Label>
</StackLayout>
```
<a name="listingcolors" />

## <a name="listing-colors-with-boxview"></a>Výpis barvy s BoxView

`BoxView` Je vhodné pro zobrazování barvy. Používá tento program `ListView` k výpisu všech veřejných statických jen pro čtení pole Xamarin.Forms `Color` struktury:

[![Barvy ListView](boxview-images/listviewcolors-small.png "ListView barvy")](boxview-images/listviewcolors-large.png#lightbox "barvy ListView")

[ **ListViewColors** ](https://developer.xamarin.com/samples/xamarin-forms/BoxView/ListViewColors/) program obsahuje třídu s názvem `NamedColor`. Statický konstruktor používá reflexi pro přístup k všechna pole `Color` struktury a vytvořit `NamedColor` objekt pro každý z nich. Tyto jsou uložené ve statické `All` vlastnost:

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

Vizuální prvky programu jsou popsány v souboru XAML. `ItemsSource` Vlastnost `ListView` je nastavena na statickou `NamedColor.All` vlastnost, která znamená, že `ListView` zobrazí všechna individuální `NamedColor` objekty:

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

`NamedColor` Objekty se formátují podle `ViewCell` objekt, který je nastaven jako šablonu data `ListView`. Tato šablona obsahuje `BoxView` jehož `Color` vlastnost je vázána na `Color` vlastnost `NamedColor` objektu.

<a name="subclassing" />

## <a name="playing-the-game-of-life-by-subclassing-boxview"></a>Hraní her ve vytváření podtříd BoxView životnosti

Hra životnost je mobilní automaton vymysleli podle matematikovi Jan Conwayův a který na stránkách *vědecké American* v 1970s. Poskytuje vhodným úvodem článku na wikipedii [Conwayův hru životnosti](https://en.wikipedia.org/wiki/Conway%27s_Game_of_Life).

Xamarin.Forms [ **GameOfLife** ](https://developer.xamarin.com/samples/xamarin-forms/BoxView/GameOfLife/) program definuje třídu s názvem `LifeCell` , která je odvozena z `BoxView`. Tato třída zapouzdří logiku jednotlivé buňky v životě hry:

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

`LifeCell` Přidá tři další vlastnosti `BoxView`: `Col` a `Row` vlastnosti ukládání pozice buňku v mřížce a `IsAlive` vlastnost indikuje její stav. `IsAlive` Vlastnost nastaví `Color` vlastnost `BoxView` na černou, pokud buňka je aktivní a bílé, pokud buňka není aktivní.

`LifeCell` jde nainstalovat také `TapGestureRecognizer` uživatel k přepnutí stavu buněk je klepnutím. Třída překládá `Tapped` událost do jeho vlastní nástroj pro rozpoznávání gest `Tapped` událostí.

**GameOfLife** program také zahrnuje `LifeGrid` třídy, který ukrývá většinu funkcí logika hry, a `MainPage` třídu, která zpracovává programu vizuály. Patří mezi ně překrytí, který popisuje pravidla hry. Tady je program v akci ukazující několik set `LifeCell` objekty na stránce:

[![Hra životnosti](boxview-images/gameoflife-small.png "hru životnosti")](boxview-images/gameoflife-large.png#lightbox "hru životnosti")

<a name="digitalclock" />

## <a name="creating-a-digital-clock"></a>Vytvoření digitální hodiny

[ **DotMatrixClock** ](https://developer.xamarin.com/samples/xamarin-forms/BoxView/DotMatrixClock/) program vytvoří 210 `BoxView` prvky pro simulaci tečky zastaralý zobrazení jehličkové 5 7. Můžete si přečíst čas v režimu na výšku nebo šířku, ale je větší orientovaný na šířku:

[![Hodiny jehličkové](boxview-images/dotmatrixclock-small.png "jehličkové hodiny")](boxview-images/dotmatrixclock-large.png#lightbox "jehličkové hodiny")

Soubor XAML trochu více než vytvoření instance `AbsoluteLayout` používá pro hodiny:

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

Všechno ostatní vyvolá se v souboru kódu na pozadí. Definice několik polí, která popisují tečky odpovídající každé z 10 číslic a dvojtečky výrazně zjednodušuje logiku jehličkové zobrazení:

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

Tato pole zakončen trojrozměrného pole `BoxView` prvky pro ukládání tečkou vzory pro šest číslic.

Konstruktor vytvoří všechny `BoxView` prvky číslice a dvojtečky a také inicializuje `Color` vlastnost `BoxView` prvky pro dvojtečka:

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

Tento program používá relativní umístění a velikosti funkce `AbsoluteLayout`. Šířku a výšku každého `BoxView` jsou nastaveny na desetinné hodnoty, konkrétně 85 % 1, vydělí počtem vodorovné a svislé tečky. Pozice jsou nastaveny také pro desetinné hodnoty.

Protože pozice a velikosti jsou relativní vzhledem k celkové velikosti `AbsoluteLayout`, `SizeChanged` obslužné rutiny pro stránky třeba nastavit pouze `HeightRequest` z `AbsoluteLayout`:

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

Šířka `AbsoluteLayout` je automaticky nastavit, protože se roztáhne na celou šířku stránky.

Konečný kód `MainPage` třída zpracovává časovače zpětného volání a barvy tečky každou číslici. Definice vícerozměrných polí na začátku souboru kódu na pozadí jednodušeji tuto logiku nejjednodušší část programu:

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

## <a name="creating-an-analog-clock"></a>Vytváření analogové hodiny

Hodiny jehličkové zdát zřejmé žádost `BoxView`, ale `BoxView` prvky jsou také schopná porozumění analogové hodiny:

[![Hodiny BoxView](boxview-images/boxviewclock-small.png "BoxView hodiny")](boxview-images/boxviewclock-large.png#lightbox "BoxView hodiny")

Všechny vizuály [ **BoxViewClock** ](https://developer.xamarin.com/samples/xamarin-forms/BoxView/BoxViewClock/) programu jsou podřízené `AbsoluteLayout`. Tyto prvky jsou velikosti pomocí `LayoutBounds` přidružená vlastnost a otočit pomocí `Rotation` vlastnost.

Tři `BoxView` prvky pro rukou hodiny nejsou vytvořena instance v souboru XAML, ale umístěné nebo velikosti:

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

Konstruktor soubor kódu na pozadí vytvoří instanci 60 `BoxView` prvky pro značky kolem obvodu hodiny:

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

Velikost a umístění všech `BoxView` prvky probíhá `SizeChanged` obslužné rutiny pro `AbsoluteLayout`. Trochu struktura vnitřní třídu volá `HandParams` popisuje velikost každé tři praktické vzhledem k celkové velikosti hodiny:

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

`SizeChanged` Obslužné rutiny určuje System center a poloměr `AbsoluteLayout`a potom velikosti a umístí 60 `BoxView` prvků, které slouží jako osové značky. `for` Smyčky končí tím, že nastavíte `Rotation` vlastnost každý z těchto `BoxView` elementy. Na konci `SizeChanged` obslužné rutiny, `LayoutHand` metoda je volána k určení velikosti a umístění tři praktické hodiny:

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

`LayoutHand` Metoda velikosti a umístění jednotlivých ručně tak, aby odkazovala přímo do pozice 12:00. Na konci metody `AnchorY` je nastavena na pozici odpovídající center hodin. To znamená střed otáčení.

Do rukou jsou otočeny ve funkci zpětného volání časovače:

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

Druhé straně zpracovává trochu jinak: animace funkce uvolnění se použije provést přesun zdá se, že mechanickým spíše než hladký průběh. Na jednotlivé takty druhé straně si vyžádá zpět trochu a pak overshoots svůj cíl. Nepatrné kód přidá mnohem realitu pohybu.

## <a name="conclusion"></a>Závěr

`BoxView` Zdát, že jednoduchá na první, ale jako vy jste viděli, může být poměrně univerzální a můžete téměř reprodukci vizuály, které jsou obvykle je to možné, pouze s vektorové grafiky. Pro složitější grafiky, najdete [pomocí ve Skiasharpu v Xamarin.Forms](~/xamarin-forms/user-interface/graphics/skiasharp/index.md).


## <a name="related-links"></a>Související odkazy

- [Základní BoxView (ukázka)](https://developer.xamarin.com/samples/xamarin-forms/BoxView/BasicBoxView)
- [Textové dekorace (ukázka)](https://developer.xamarin.com/samples/xamarin-forms/BoxView/TextDecoration)
- [Barva ListBox (ukázka)](https://developer.xamarin.com/samples/xamarin-forms/BoxView/ColorListBox)
- [Hra života (ukázka)](https://developer.xamarin.com/samples/xamarin-forms/BoxView/GameOfLife)
- [Maticové hodin (ukázka)](https://developer.xamarin.com/samples/xamarin-forms/BoxView/DotMatrixClock)
- [Hodiny BoxView (ukázka)](https://developer.xamarin.com/samples/xamarin-forms/BoxView/BoxViewClock)
- [BoxView](xref:Xamarin.Forms.BoxView)
