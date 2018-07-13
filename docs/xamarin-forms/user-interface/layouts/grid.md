---
title: Mřížka Xamarin.Forms
description: Tento článek vysvětluje, jak pomocí třídy Xamarin.Forms mřížky v tabulkách, které mají řádků a sloupců vyjádřit.
ms.prod: xamarin
ms.assetid: 762B1802-D185-494C-B643-74EED55882FE
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 10/26/2017
ms.openlocfilehash: 01dd59d5e94b473316b03f9035d38305fad42880
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/12/2018
ms.locfileid: "38994499"
---
# <a name="xamarinforms-grid"></a>Mřížka Xamarin.Forms

[`Grid`](xref:Xamarin.Forms.Grid) podporuje uspořádání zobrazení do řádků a sloupců. Řádky a sloupce můžete nastavit přímo úměrná velikosti nebo absolutní velikosti. `Grid` Rozložení by neměly být zaměněny s tradiční tabulkami a není určena pro tabulková data k dispozici. `Grid` nemá koncept řádek, sloupec nebo buňku formátování. Na rozdíl od tabulek HTML `Grid` čistě slouží k vytváření rozložení obsahu.

[![](grid-images/layouts-sml.png "Rozložení Xamarin.Forms")](grid-images/layouts.png#lightbox "rozložení Xamarin.Forms")

Tento článek se zabývá:

- **[Účel](#Purpose)**  &ndash; běžné použití pro `Grid`.
- **[Využití](#Usage)**  &ndash; použití `Grid` k dosažení požadované návrhu.
  - **[Řádky a sloupce](#Rows_and_Columns)**  &ndash; zadejte řádků a sloupců `Grid`.
  - **[Umístění zobrazení](#Placing_Views)**  &ndash; přidat zobrazení do mřížky v určitých řádků a sloupců.
  - **[Mezery](#Spacing)**  &ndash; konfigurace mezery mezi řádky a sloupce.
  - **[Rozsahy](#Spans)**  &ndash; konfigurace prvků, které mají pokrývat více řádcích nebo sloupcích.

![](grid-images/grid.png "Zkoumání mřížky")

## <a name="purpose"></a>Účel

`Grid` umožňuje uspořádat zobrazení do mřížky. To je užitečné v mnoha případech:

- Uspořádání tlačítek v kalkulačce aplikace
- Uspořádáním tlačítka/volby v mřížce, jako je iOS nebo Android domovské obrazovky
- Uspořádání zobrazení tak, aby byly stejné velikosti v jedné dimenzi (jako v některé panely nástrojů)

## <a name="usage"></a>Použití

Na rozdíl od tradičních tabulky `Grid` nelze odvodit, počtu a velikosti řádků a sloupců z obsahu. Místo toho `Grid` má `RowDefinitions` a `ColumnDefinitions` kolekce. Tyto definice o tom, kolik řádků a sloupců se rozloží uchování. Zobrazení jsou přidána do `Grid` zadaný řádek a indexy sloupců, které identifikovat, kterému řádku a sloupce zobrazení musí být umístěné ve.

<a name="Rows_and_Columns" />

### <a name="rows-and-columns"></a>Řádků a sloupců

Řádek a sloupec informace jsou uloženy v `Grid`společnosti `RowDefinitions`  &  `ColumnDefinitions` vlastnosti, které jsou každé kolekce z [ `RowDefinition` ](xref:Xamarin.Forms.RowDefinition) a [ `ColumnDefinition` ](xref:Xamarin.Forms.ColumnDefinition)objekty v uvedeném pořadí. `RowDefinition` má jednu vlastnost `Height`, a `ColumnDefinition` má jednu vlastnost `Width`. Možnosti pro výška a šířka jsou následující:

- **Automatické** &ndash; automaticky velikosti podle obsahu v řádku nebo sloupce. Zadaný jako [ `GridUnitType.Auto` ](xref:Xamarin.Forms.GridUnitType) v jazyce C# nebo jako `Auto` v XAML.
- **Proportional(*)** &ndash; jako podíl zbývající prostor o velikosti sloupců a řádků. Stanovená jako hodnota a `GridUnitType.Star` v jazyce C# a jako `#*` v XAML, s `#` se požadovanou hodnotu. Zadáním jednoho řádku/sloupce s `*` způsobí tak, aby vyplnil dostupné místo.
- **Absolutní** &ndash; velikosti sloupců a řádků s hodnotami konkrétní, pevné výšky a šířky. Stanovená jako hodnota a `GridUnitType.Absolute` v jazyce C# a jako `#` v XAML, s `#` se požadovanou hodnotu.

> [!NOTE]
> Šířka hodnoty pro sloupce jsou nastavené jako "*" ve výchozím nastavení v Xamarin.Forms, které zajišťuje, že se sloupci vyplnil dostupné místo.

Vezměte v úvahu aplikaci, která potřebuje tři řádky a dva sloupce. Dolní řádek musí být přesně 200 px výšku a do horního řádku musí být dvakrát tak vysoký jako prostředním. Do levého sloupce musí být dostatečně široké a nevejdou se obsah a v pravém sloupci je potřeba vyplnit zbývající prostor.

V XAML:

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

### <a name="placing-views-in-a-grid"></a>Zobrazení umístění v mřížce

Umístí zobrazení `Grid` je potřeba je přidat jako podřízené objekty k mřížce a potom zadejte, kterému řádku a sloupci patří v.

V XAML, použijte `Grid.Row` a `Grid.Column` na každé jednotlivé zobrazení k určení umístění. Všimněte si, že `Grid.Row` a `Grid.Column` zadejte umístění podle založený na nule seznam řádků a sloupců. To znamená, že do mřížky 4 x 4 levá horní buňka je (0; 0) a pravá dolní buňka je (3,3).

`Grid` Uvedené níže obsahuje čtyři pole:

![](grid-images/label-grid.png "Mřížka s čtyři zobrazení")

V XAML:

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

Výše uvedený kód vytvoří mřížku s čtyři popisky, dvěma sloupci a dva řádky. Mějte na paměti, že každému popisku bude mít stejnou velikost a že řádky se rozbalí a použít všechny dostupné místo.

V předchozím příkladu zobrazení jsou přidána do [ `Grid.Children` ](xref:Xamarin.Forms.Grid.Children) pomocí kolekce [ `Add` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Grid+IGridList%3CT%3E.Add/p/Xamarin.Forms.View/System.Int32/System.Int32/) přetížení, která určuje argumenty nahoře a vlevo. Při použití [ `Add` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Grid+IGridList%3CT%3E.Add/p/Xamarin.Forms.View/System.Int32/System.Int32/System.Int32/System.Int32/) přetížení, které určuje vlevo, vpravo, horní a dolní argumenty, zatímco nalevo a hlavní argumenty bude vždy odkazovat buněk v rámci [ `Grid` ](xref:Xamarin.Forms.Grid), pravé a dolní argumenty pravděpodobně neodkazuje na buňky, které jsou mimo `Grid`. Je to proto, že pravý argument musí být větší než levý argument a dolní argument vždy musí být větší než top argument. Následující příklad ukazuje odpovídající kód pomocí `Add` přetížení:

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

`Grid` obsahuje vlastnosti pro řízení mezery mezi řádky a sloupce.  Následující vlastnosti jsou k dispozici pro přizpůsobení `Grid`:

- **ColumnSpacing** &ndash; velikost místa mezi sloupci.
- **RowSpacing** &ndash; velikost místa mezi řádky.

Určuje následující XAML `Grid` dva sloupce, jeden řádek a 5 px mezery mezi sloupci:

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

Při práci s mřížky, často je element, který by měla zabírat více než jeden řádek nebo sloupec. Vezměte v úvahu aplikace s jednoduchou kalkulačku:

![](grid-images/calculator.png "Calulator aplikace")

Všimněte si, že tlačítko 0 zahrnuje dva sloupce, stejně jako na integrované kalkulačky pro každou platformu. Využívá se při něm `ColumnSpan` vlastnost, která určuje, kolik sloupců, kterým je element by měla zabírat. XAML pro toto tlačítko:

```xaml
<Button Text = "0" Grid.Row="4" Grid.Column="0" Grid.ColumnSpan="2" />
```

A v jazyce C#:

```csharp
Button zeroButton = new Button { Text = "0" };
controlGrid.Children.Add (zeroButton, 0, 4);
Grid.SetColumnSpan (zeroButton, 2);
```

Všimněte si, že v kódu statických metod `Grid` třídy jsou používány k provádění umístění změn včetně změny `ColumnSpan` a `RowSpan`. Všimněte si, že na rozdíl od dalších vlastností, které lze nastavit v okamžiku, vlastnosti nastavené pomocí statické metody, už musí být také v mřížce předtím, než se změní.

Kompletní XAML pro výše uvedené Kalkulačka aplikace vypadá takto:

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

Všimněte si, že popisek v horní části stránky mřížky i na nulovou tlačítko occuping více než jeden sloupec. I když rozložení lze dosáhnout pomocí vnořených mřížky `ColumnSpan`  &  `RowSpan` přístup je jednodušší.

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

- [Vytváření mobilních aplikací s Xamarin.Forms, kapitole 17](https://developer.xamarin.com/r/xamarin-forms/book/chapter17.pdf)
- [Mřížka](xref:Xamarin.Forms.Grid)
- [Rozložení (ukázka)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/Layout/)
- [Příklad BusinessTumble (ukázka)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/BusinessTumble/)
