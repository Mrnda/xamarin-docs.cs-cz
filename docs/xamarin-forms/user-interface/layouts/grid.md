---
title: Mřížka
description: Existuje zobrazení mřížky.
ms.prod: xamarin
ms.assetid: 762B1802-D185-494C-B643-74EED55882FE
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 10/26/2017
ms.openlocfilehash: 4d1699f95d39fcd43a4d8da8404beac8cbf0158e
ms.sourcegitcommit: a7febc19102209b21e0696256c324f366faa444e
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 06/04/2018
ms.locfileid: "34733019"
---
# <a name="grid"></a>Mřížka

[`Grid`](https://developer.xamarin.com/api/type/Xamarin.Forms.Grid/) podporuje uspořádání zobrazení do řádků a sloupců. Řádků a sloupců můžete nastavit tak, aby měl přímo úměrná velikosti nebo absolutní velikosti. `Grid` Rozložení by neměly zaměňovat s tradiční tabulky a není určena pro tabulková data k dispozici. `Grid` Koncept řádek, sloupec nebo buňka formátování není k dispozici. Na rozdíl od tabulek HTML `Grid` je určený výhradně pro vytvoření rozložení obsahu.

[![](grid-images/layouts-sml.png "Rozložení Xamarin.Forms")](grid-images/layouts.png#lightbox "Xamarin.Forms rozložení")

Tento článek se zabývá:

- **[Účel](#Purpose)**  &ndash; běžné používá pro `Grid`.
- **[Využití](#Usage)**  &ndash; použití `Grid` k dosažení požadované návrhu.
  - **[Řádků a sloupců](#Rows_and_Columns)**  &ndash; zadejte řádků a sloupců pro `Grid`.
  - **[Umístění zobrazení](#Placing_Views)**  &ndash; přidat zobrazení do mřížky v určitých řádků a sloupců.
  - **[Mezer](#Spacing)**  &ndash; konfigurace mezery mezi řádky a sloupce.
  - **[Rozsahy](#Spans)**  &ndash; konfigurace elementů span napříč více řádků nebo sloupců.

![](grid-images/grid.png "Zkoumání mřížky")

## <a name="purpose"></a>Účel

`Grid` slouží k uspořádání zobrazení v mřížce. To je užitečné v mnoha případech:

- Uspořádání tlačítek v aplikaci kalkulačky
- Uspořádáním tlačítka nebo volby v mřížce, jako je iOS nebo Android domovské obrazovky
- Uspořádání zobrazení, aby byly stejné velikosti ve jednu dimenzi (jako v některé panely nástrojů)

## <a name="usage"></a>Použití

Na rozdíl od tradičních tabulky `Grid` nelze odvodit počtu a velikosti řádků a sloupců z obsahu. Místo toho `Grid` má `RowDefinitions` a `ColumnDefinitions` kolekce. Tyto obsahovat definice kolik řádků a sloupců budou rozloženy. Zobrazení jsou přidány do `Grid` zadaný řádek a indexy sloupců, které identifikovat, které řádků a sloupců zobrazení musí být umístěny v.

<a name="Rows_and_Columns" />

### <a name="rows-and-columns"></a>Řádků a sloupců

Řádků a sloupců informace jsou uloženy v `Grid`na `RowDefinitions`  &  `ColumnDefinitions` vlastnosti, které jsou každé kolekce z [ `RowDefinition` ](https://developer.xamarin.com/api/type/Xamarin.Forms.RowDefinition/) a [ `ColumnDefinition` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ColumnDefinition/)objekty, v uvedeném pořadí. `RowDefinition` má jednu vlastnost, `Height`, a `ColumnDefinition` má jednu vlastnost, `Width`. Možnosti pro výška a šířka jsou následující:

- **Automatické** &ndash; automaticky velikosti podle obsahu sloupce nebo řádku. Zadaný jako [ `GridUnitType.Auto` ](https://developer.xamarin.com/api/type/Xamarin.Forms.GridUnitType/) v jazyce C# nebo jako `Auto` v jazyce XAML.
- **Proportional(*)** &ndash; velikosti řádků a sloupců jako podíl zbývající místo. Stanovená jako hodnota a `GridUnitType.Star` v jazyce C# a jako `#*` v jazyce XAML, s `#` se požadovanou hodnotu. Určení jeden řádek nebo sloupec s `*` způsobí, že vyplňování dostupného místa.
- **Absolutní** &ndash; velikosti sloupce a řádky s konkrétní, pevné hodnoty výškou a šířkou. Stanovená jako hodnota a `GridUnitType.Absolute` v jazyce C# a jako `#` v jazyce XAML, s `#` se požadovanou hodnotu.

> [!NOTE]
> Šířka hodnoty pro sloupce jsou nastavené jako "*" ve výchozím nastavení v Xamarin.Forms, což zajistí, že sloupec bude vyplňování dostupného místa.

Zvažte aplikaci, která potřebuje tři řádky a dva sloupce. Dolní řádek musí být přesně 200 px výšku a na začátek řádku musí být dvakrát tak vysoký jako střední řádek. V levém sloupci musí být dost široké, a přizpůsobit obsah a pravém sloupci potřebuje k vyplnění zbývající místo.

V jazyce XAML:

```xaml
<Grid>
  <Grid.RowDefinitions>
    <RowDefinition Height="2*" />
    <RowDefinition Height="*" />
    <RowDefinition Height="200" />
  </Grid.RowDefinitions>
  <Grid.ColumnDefinitions>
    <ColumnDefinition Width="Auto" />
    <ColumnDefinition Width="*" />
  </Grid.ColumnDefinitions>
</Grid>
```

V jazyce C#:

```csharp
var grid = new Grid();
grid.RowDefinitions.Add (new RowDefinition { Height = new GridLength(2, GridUnitType.Star) });
grid.RowDefinitions.Add (new RowDefinition { Height = new GridLength (1, GridUnitType.Star) });
grid.RowDefinitions.Add (new RowDefinition { Height = new GridLength(200)});
grid.ColumnDefinitions.Add (new ColumnDefinition{ Width = new GridLength (200) });
```

<a name="Placing_Views" />

### <a name="placing-views-in-a-grid"></a>Umístění zobrazení v mřížce

Zobrazení v umístit `Grid` budete muset přidat jako podřízené objekty do mřížky a potom zadejte, které řádků a sloupců patří v.

V jazyce XAML, použijte `Grid.Row` a `Grid.Column` na každé jednotlivé zobrazení k určení umístění. Všimněte si, že `Grid.Row` a `Grid.Column` zadejte umístění, které jsou založené na nule seznam řádků a sloupců. To znamená, že v mřížce 4 x 4, buňku vlevo nahoře je (0,0) a není pravé dolní buňky (3,3).

`Grid` Vidět níže obsahuje čtyři buňky:

![](grid-images/label-grid.png "Mřížka s čtyři zobrazení")

V jazyce XAML:

```xaml
<Grid>
  <Grid.RowDefinitions>
    <RowDefinition Height="*" />
    <RowDefinition Height="*" />
  </Grid.RowDefinitions>
  <Grid.ColumnDefinitions>
    <ColumnDefinition Width="*" />
    <ColumnDefinition Width="*" />
  </Grid.ColumnDefinitions>
  <Label Text="Top Left" Grid.Row="0" Grid.Column="0" />
  <Label Text="Top Right" Grid.Row="0" Grid.Column="1" />
  <Label Text="Bottom Left" Grid.Row="1" Grid.Column="0" />
  <Label Text="Bottom Right" Grid.Row="1" Grid.Column="1" />
</Grid>
```

V jazyce C#:

```csharp
var grid = new Grid();

grid.RowDefinitions.Add(new RowDefinition { Height = new GridLength(1, GridUnitType.Star)});
grid.RowDefinitions.Add(new RowDefinition { Height = new GridLength(1, GridUnitType.Star)});
grid.ColumnDefinitions.Add(new ColumnDefinition { Width = new GridLength(1, GridUnitType.Star)});
grid.ColumnDefinitions.Add(new ColumnDefinition { Width = new GridLength(1, GridUnitType.Star)});

var topLeft = new Label { Text = "Top Left" };
var topRight = new Label { Text = "Top Right" };
var bottomLeft = new Label { Text = "Bottom Left" };
var bottomRight = new Label { Text = "Bottom Right" };

grid.Children.Add(topLeft, 0, 0);
grid.Children.Add(topRight, 1, 0);
grid.Children.Add(bottomLeft, 0, 1);
grid.Children.Add(bottomRight, 1, 1);
```

Výše uvedený kód vytvoří čtyři popisky, dva sloupce a dva řádky mřížky. Všimněte si, že každý popisek bude mít stejnou velikost a že bude používat všechny dostupné místo rozbalte řádky.

V předchozím příkladu se zobrazení přidat do [ `Grid.Children` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Grid.Children/) kolekce používá [ `Add` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Grid+IGridList%3CT%3E.Add/p/Xamarin.Forms.View/System.Int32/System.Int32/) přetížení, které určuje levého a horního argumenty. Při použití [ `Add` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Grid+IGridList%3CT%3E.Add/p/Xamarin.Forms.View/System.Int32/System.Int32/System.Int32/System.Int32/) přetížení, které určuje vlevo, vpravo, horní a dolní argumenty během doleva a horní argumenty se odkazují na buňky v rámci [ `Grid` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Grid/), pravé a může se zobrazit dolní argumenty k odkazování na buněk, které jsou mimo `Grid`. Je to proto pravý argument musí být vždy větší než levý argument a argument dolní vždy musí být větší než top argument. Následující příklad ukazuje ekvivalentní kódu pomocí obou `Add` přetížení:

```csharp
// left, top
grid.Children.Add(topLeft, 0, 0);
grid.Children.Add(topRight, 1, 0);
grid.Children.Add(bottomLeft, 0, 1);
grid.Children.Add(bottomRight, 1, 1);

// left, right, top, bottom
grid.Children.Add(topLeft, 0, 1, 0, 1);
grid.Children.Add(topRight, 1, 2, 0, 1);
grid.Children.Add(bottomLeft, 0, 1, 1, 2);
grid.Children.Add(bottomRight, 1, 2, 1, 2);
```

### <a name="spacing"></a>Mezery

`Grid` má vlastnosti k řízení mezery mezi řádky a sloupce.  Následující vlastnosti jsou k dispozici pro přizpůsobení `Grid`:

- **ColumnSpacing** &ndash; množství mezeru mezi sloupci.
- **RowSpacing** &ndash; velikost mezery mezi řádky.

Určuje následující XAML `Grid` s dva sloupce, jeden řádek a 5 px mezery mezi sloupci:

```xaml
<Grid ColumnSpacing="5">
  <Grid.ColumnDefinitions>
    <ColumnDefinitions Width="*" />
    <ColumnDefinitions Width="*" />
  </Grid.ColumnDefinitions>
</Grid>
```

V jazyce C#:

```csharp
var grid = new Grid { ColumnSpacing = 5 };
grid.ColumnDefnitions.Add(new ColumnDefinition { Width = new GridLength (1, GridUnitType.Star)});
grid.ColumnDefnitions.Add(new ColumnDefinition { Width = new GridLength (1, GridUnitType.Star)});
```

### <a name="spans"></a>Rozpětí

Často při práci s mřížka, není element, který by měla zabírat více než jeden sloupce či řádku. Vezměte v úvahu aplikace pro jednoduché výpočet:

![](grid-images/calculator.png "Calulator aplikace")

Všimněte si, že tlačítko 0 zahrnuje dva sloupce, stejně jako na vestavěné kalkulačky pro každou platformu. Toho dosahuje pomocí `ColumnSpan` vlastnosti, která určuje, kolik sloupců na element by měla zabírat. XAML pro toto tlačítko:

```xaml
<Button Text = "0" Grid.Row="4" Grid.Column="0" Grid.ColumnSpan="2" />
```

A v jazyce C#:

```csharp
Button zeroButton = new Button { Text = "0" };
controlGrid.Children.Add (zeroButton, 0, 4);
Grid.SetColumnSpan (zeroButton, 2);
```

Všimněte si, že v kódu statických metod `Grid` třída slouží k provádění umísťovací změny, včetně změny `ColumnSpan` a `RowSpan`. Všimněte si, že na rozdíl od jiných vlastností, které lze nastavit kdykoli, vlastnosti nastavit pomocí statických metod musí již být také v mřížce předtím, než jsou změněna.

Dokončení XAML pro výše uvedené aplikaci kalkulačky vypadá takto:

```xaml
<?xml version="1.0" encoding="UTF-8"?>
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
x:Class="LayoutSamples.CalculatorGridXAML"
Title = "Calculator - XAML"
BackgroundColor="#404040">
    <ContentPage.Resources>
        <ResourceDictionary>
            <Style x:Key="plainButton" TargetType="Button">
                <Setter Property="BackgroundColor" Value="#eee"/>
                <Setter Property="TextColor" Value="Black" />
                <Setter Property="BorderRadius" Value="0"/>
                <Setter Property="FontSize" Value="40" />
            </Style>
            <Style x:Key="darkerButton" TargetType="Button">
                <Setter Property="BackgroundColor" Value="#ddd"/>
                <Setter Property="TextColor" Value="Black" />
                <Setter Property="BorderRadius" Value="0"/>
                <Setter Property="FontSize" Value="40" />
            </Style>
            <Style x:Key="orangeButton" TargetType="Button">
                <Setter Property="BackgroundColor" Value="#E8AD00"/>
                <Setter Property="TextColor" Value="White" />
                <Setter Property="BorderRadius" Value="0"/>
                <Setter Property="FontSize" Value="40" />
            </Style>
        </ResourceDictionary>
    </ContentPage.Resources>
    <ContentPage.Content>
        <Grid x:Name="controlGrid" RowSpacing="1" ColumnSpacing="1">
            <Grid.RowDefinitions>
                <RowDefinition Height="150" />
                <RowDefinition Height="*" />
                <RowDefinition Height="*" />
                <RowDefinition Height="*" />
                <RowDefinition Height="*" />
                <RowDefinition Height="*" />
            </Grid.RowDefinitions>
            <Grid.ColumnDefinitions>
                <ColumnDefinition Width="*" />
                <ColumnDefinition Width="*" />
                <ColumnDefinition Width="*" />
                <ColumnDefinition Width="*" />
            </Grid.ColumnDefinitions>
            <Label Text="0" Grid.Row="0" HorizontalTextAlignment="End" VerticalTextAlignment="End" TextColor="White"
        FontSize="60" Grid.ColumnSpan="4" />
            <Button Text = "C" Grid.Row="1" Grid.Column="0"
        Style="{StaticResource darkerButton}" />
            <Button Text = "+/-" Grid.Row="1" Grid.Column="1"
        Style="{StaticResource darkerButton}" />
            <Button Text = "%" Grid.Row="1" Grid.Column="2"
        Style="{StaticResource darkerButton}" />
            <Button Text = "div" Grid.Row="1" Grid.Column="3"
        Style="{StaticResource orangeButton}" />
            <Button Text = "7" Grid.Row="2" Grid.Column="0"
        Style="{StaticResource plainButton}" />
            <Button Text = "8" Grid.Row="2" Grid.Column="1"
        Style="{StaticResource plainButton}" />
            <Button Text = "9" Grid.Row="2" Grid.Column="2"
        Style="{StaticResource plainButton}" />
            <Button Text = "X" Grid.Row="2" Grid.Column="3"
        Style="{StaticResource orangeButton}" />
            <Button Text = "4" Grid.Row="3" Grid.Column="0"
        Style="{StaticResource plainButton}" />
            <Button Text = "5" Grid.Row="3" Grid.Column="1"
        Style="{StaticResource plainButton}" />
            <Button Text = "6" Grid.Row="3" Grid.Column="2"
        Style="{StaticResource plainButton}" />
            <Button Text = "-" Grid.Row="3" Grid.Column="3"
        Style="{StaticResource orangeButton}" />
            <Button Text = "1" Grid.Row="4" Grid.Column="0"
        Style="{StaticResource plainButton}" />
            <Button Text = "2" Grid.Row="4" Grid.Column="1"
        Style="{StaticResource plainButton}" />
            <Button Text = "3" Grid.Row="4" Grid.Column="2"
        Style="{StaticResource plainButton}" />
            <Button Text = "+" Grid.Row="4" Grid.Column="3"
        Style="{StaticResource orangeButton}" />
            <Button Text = "0" Grid.ColumnSpan="2"
        Grid.Row="5" Grid.Column="0" Style="{StaticResource plainButton}" />
            <Button Text = "." Grid.Row="5" Grid.Column="2"
        Style="{StaticResource plainButton}" />
            <Button Text = "=" Grid.Row="5" Grid.Column="3"
        Style="{StaticResource orangeButton}" />
        </Grid>
    </ContentPage.Content>
</ContentPage>
```

Všimněte si, že jsou oba popisek v horní části mřížky a tlačítko nula occuping více než jeden sloupec. I když lze dosáhnout podobné rozložení pomocí vnořené mřížky `ColumnSpan`  &  `RowSpan` přístup je jednodušší.

C# implementace:

```csharp
public CalculatorGridCode ()
{
  Title = "Calculator - C#";
  BackgroundColor = Color.FromHex ("#404040");

  var plainButton = new Style (typeof(Button)) {
    Setters = {
      new Setter { Property = Button.BackgroundColorProperty, Value = Color.FromHex ("#eee") },
      new Setter { Property = Button.TextColorProperty, Value = Color.Black },
      new Setter { Property = Button.BorderRadiusProperty, Value = 0 },
      new Setter { Property = Button.FontSizeProperty, Value = 40 }
    }
  };
  var darkerButton = new Style (typeof(Button)) {
    Setters = {
      new Setter { Property = Button.BackgroundColorProperty, Value = Color.FromHex ("#ddd") },
      new Setter { Property = Button.TextColorProperty, Value = Color.Black },
      new Setter { Property = Button.BorderRadiusProperty, Value = 0 },
      new Setter { Property = Button.FontSizeProperty, Value = 40 }
    }
  };
  var orangeButton = new Style (typeof(Button)) {
    Setters = {
      new Setter { Property = Button.BackgroundColorProperty, Value = Color.FromHex ("#E8AD00") },
      new Setter { Property = Button.TextColorProperty, Value = Color.White },
      new Setter { Property = Button.BorderRadiusProperty, Value = 0 },
      new Setter { Property = Button.FontSizeProperty, Value = 40 }
    }
  };

  var controlGrid = new Grid { RowSpacing = 1, ColumnSpacing = 1 };
  controlGrid.RowDefinitions.Add (new RowDefinition { Height = new GridLength (150) });
  controlGrid.RowDefinitions.Add (new RowDefinition { Height = new GridLength (1, GridUnitType.Star) });
  controlGrid.RowDefinitions.Add (new RowDefinition { Height = new GridLength (1, GridUnitType.Star) });
  controlGrid.RowDefinitions.Add (new RowDefinition { Height = new GridLength (1, GridUnitType.Star) });
  controlGrid.RowDefinitions.Add (new RowDefinition { Height = new GridLength (1, GridUnitType.Star) });
  controlGrid.RowDefinitions.Add (new RowDefinition { Height = new GridLength (1, GridUnitType.Star) });

  controlGrid.ColumnDefinitions.Add (new ColumnDefinition { Width = new GridLength (1, GridUnitType.Star) });
  controlGrid.ColumnDefinitions.Add (new ColumnDefinition { Width = new GridLength (1, GridUnitType.Star) });
  controlGrid.ColumnDefinitions.Add (new ColumnDefinition { Width = new GridLength (1, GridUnitType.Star) });
  controlGrid.ColumnDefinitions.Add (new ColumnDefinition { Width = new GridLength (1, GridUnitType.Star) });

  var label = new Label {
    Text = "0",
    HorizontalTextAlignment = TextAlignment.End,
    VerticalTextAlignment = TextAlignment.End,
    TextColor = Color.White,
    FontSize = 60
  };
  controlGrid.Children.Add (label, 0, 0);

  Grid.SetColumnSpan (label, 4);

  controlGrid.Children.Add (new Button { Text = "C", Style = darkerButton }, 0, 1);
  controlGrid.Children.Add (new Button { Text = "+/-", Style = darkerButton }, 1, 1);
  controlGrid.Children.Add (new Button { Text = "%", Style = darkerButton }, 2, 1);
  controlGrid.Children.Add (new Button { Text = "div", Style = orangeButton }, 3, 1);
  controlGrid.Children.Add (new Button { Text = "7", Style = plainButton }, 0, 2);
  controlGrid.Children.Add (new Button { Text = "8", Style = plainButton }, 1, 2);
  controlGrid.Children.Add (new Button { Text = "9", Style = plainButton }, 2, 2);
  controlGrid.Children.Add (new Button { Text = "X", Style = orangeButton }, 3, 2);
  controlGrid.Children.Add (new Button { Text = "4", Style = plainButton }, 0, 3);
  controlGrid.Children.Add (new Button { Text = "5", Style = plainButton }, 1, 3);
  controlGrid.Children.Add (new Button { Text = "6", Style = plainButton }, 2, 3);
  controlGrid.Children.Add (new Button { Text = "-", Style = orangeButton }, 3, 3);
  controlGrid.Children.Add (new Button { Text = "1", Style = plainButton }, 0, 4);
  controlGrid.Children.Add (new Button { Text = "2", Style = plainButton }, 1, 4);
  controlGrid.Children.Add (new Button { Text = "3", Style = plainButton }, 2, 4);
  controlGrid.Children.Add (new Button { Text = "+", Style = orangeButton }, 3, 4);
  controlGrid.Children.Add (new Button { Text = ".", Style = plainButton }, 2, 5);
  controlGrid.Children.Add (new Button { Text = "=", Style = orangeButton }, 3, 5);

  var zeroButton = new Button { Text = "0", Style = plainButton };
  controlGrid.Children.Add (zeroButton, 0, 5);
  Grid.SetColumnSpan (zeroButton, 2);

  Content = controlGrid;
}
```


## <a name="related-links"></a>Související odkazy

- [Vytváření mobilních aplikací s Xamarin.Forms, kapitoly 17](https://developer.xamarin.com/r/xamarin-forms/book/chapter17.pdf)
- [Mřížka](https://developer.xamarin.com/api/type/Xamarin.Forms.Grid/)
- [Rozložení (ukázka)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/Layout/)
- [Příklad BusinessTumble (ukázka)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/BusinessTumble/)
