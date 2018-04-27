---
title: StackLayout
description: Pomocí StackLayout kolekce zobrazení k dispozici prostřednictvím jednu dimenzi.
ms.prod: xamarin
ms.assetid: 6A91EA70-268C-462C-AAAF-F8DA011403F8
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 11/25/2015
ms.openlocfilehash: c27f94302037e4e19c9d72131e7137c8a4004d5c
ms.sourcegitcommit: 1561c8022c3585655229a869d9ef3510bf83f00a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/27/2018
---
# <a name="stacklayout"></a>StackLayout

`StackLayout` umožňuje uspořádat zobrazení jednorozměrné řádku ("stack"), vodorovně nebo svisle. Zobrazení v `StackLayout` může mít velikost podle místa v rozložení pomocí možností rozložení. Umístění je určen podle pořadí, ve kterém zobrazení byly přidány do rozložení a rozložení možnosti zobrazení.

[![](stack-layout-images/layouts-sml.png "Rozložení Xamarin.Forms")](stack-layout-images/layouts.png#lightbox "Xamarin.Forms rozložení")

## <a name="purpose"></a>Účel

`StackLayout` je menší složitější než dalšími zobrazeními. Jednoduché rozhraní lineární lze vytvořit pouze přidáním zobrazení `StackLayout`a složitější rozhraní vytvořené vnoření je.

## <a name="usage--behavior"></a>Použití & chování

### <a name="spacing"></a>Mezery

Ve výchozím nastavení `StackLayout` přidá okraj 6px mezi zobrazení. To můžete řídit nebo nastavit tak, aby měl žádné okraj nastavením `Spacing` vlastnost StackLayout. Následující ukazuje, jak nastavit mezery a účinek různých mezery možnosti:

V jazyce XAML:

```xaml
<?xml version="1.0" encoding="UTF-8"?>
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
x:Class="LayoutSamples.StackLayoutDemo"
Title="StackLayout Demo">
  <ContentPage.Content>
    <StackLayout Spacing="10" x:Name="layout">
      <Button Text="StackLayout" VerticalOptions="Start"
        HorizontalOptions="FillAndExpand" />
      <BoxView Color="Yellow" VerticalOptions="FillAndExpand"
        HorizontalOptions="FillAndExpand" />
            <BoxView Color="Green" VerticalOptions="FillAndExpand"
        HorizontalOptions="FillAndExpand" />
            <BoxView HeightRequest="75" Color="Blue" VerticalOptions="End"
        HorizontalOptions="FillAndExpand" />
    </StackLayout>
    </ContentPage.Content>
</ContentPage>
```

V jazyce C#:

```csharp
public class StackLayoutCode : ContentPage
{
  public StackLayoutCode ()
  {
    var layout = new StackLayout ();
    var button = new Button { Text = "StackLayout", VerticalOptions = LayoutOptions.Start,
     HorizontalOptions = LayoutOptions.FillAndExpand };
    var yellowBox = new BoxView { Color = Color.Yellow, VerticalOptions = LayoutOptions.FillAndExpand, HorizontalOptions = LayoutOptions.FillAndExpand };
    var greenBox = new BoxView { Color = Color.Green, VerticalOptions = LayoutOptions.FillAndExpand, HorizontalOptions = LayoutOptions.FillAndExpand };
    var blueBox = new BoxView { Color = Color.Blue, VerticalOptions = LayoutOptions.FillAndExpand,
      HorizontalOptions = LayoutOptions.FillAndExpand, HeightRequest = 75 };

    layout.Children.Add(button);
    layout.Children.Add(yellowBox);
    layout.Children.Add(greenBox);
    layout.Children.Add(blueBox);
    layout.Spacing = 10;
    Content = layout;
  }
}
```

Mezer = 0:

![](stack-layout-images/spacing-zero.png "StackLayout s mezery = 0")

Mezery mezi 10:

![](stack-layout-images/spacing-ten.png "StackLayout s mezery = 10")

### <a name="sizing"></a>Nastavení velikosti

Velikost zobrazení v StackLayout závisí na výšky a šířky požadavky a možnosti rozložení. `StackLayout` Vynutí odsazení. Následující `LayoutOption`s způsobí, že zobrazení tak, aby zaplnily tolik místa je k dispozici z rozložení:

- **CenterAndExpand** &ndash; centra pro zobrazení v rámci rozložení a rozšíří tak, aby zaplnily rozložení získáte tolik místa.
- **EndAndExpand** &ndash; umístí zobrazení na konci rozložení (dolní nebo hranic nejvíce vpravo) a rozšíří tak, aby zaplnily rozložení získáte tolik místa.
- **FillAndExpand** &ndash; umístí zobrazení tak, aby se žádné odsazení a zabírají rozložení získáte tolik místa.
- **StartAndExpand** &ndash; umístí zobrazení na začátku rozložení a zabírají nadřazené získáte tolik místa.

Další informace najdete v tématu [rozšíření](~/xamarin-forms/user-interface/layouts/layout-options.md#expansion).

### <a name="positioning"></a>Umístění

Zobrazení StackLayout je možné umístit a velikosti pomocí `LayoutOptions`. Každé zobrazení je možné přidělit `VerticalOptions` a `HorizontalOptions`, definování jak zobrazení se samy stanovují relativní vzhledem k rozložení. Následující předdefinované `LayoutOptions` jsou k dispozici:

- **Center** &ndash; centra pro zobrazení v rámci rozložení.
- **End** &ndash; umístí zobrazení na konci rozložení (dolní nebo hranic nejvíce vpravo).
- **Vyplnění** &ndash; umístí zobrazení tak, aby měl odsazení.
- **Spustit** &ndash; umístí zobrazení na začátku rozložení.

Následující kód ukazuje možnosti rozložení nastavení:

V jazyce XAML:

```xaml
<?xml version="1.0" encoding="UTF-8"?>
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
x:Class="LayoutSamples.StackLayoutDemo"
Title="StackLayout Demo">
  <ContentPage.Content>
    <StackLayout x:Name="layout">
      <Button VerticalOptions="Start"
        HorizontalOptions="FillAndExpand" />
      <BoxView VerticalOptions="FillAndExpand"
        HorizontalOptions="FillAndExpand" />
            <BoxView VerticalOptions="FillAndExpand"
        HorizontalOptions="FillAndExpand" />
            <BoxView HeightRequest="75" VerticalOptions="End"
        HorizontalOptions="FillAndExpand" />
    </StackLayout>
    </ContentPage.Content>
</ContentPage>
```

V jazyce C#:

```csharp
public class StackLayoutCode : ContentPage
{
  public StackLayoutCode ()
  {
    var layout = new StackLayout ();
    var button = new Button { VerticalOptions = LayoutOptions.Start,
     HorizontalOptions = LayoutOptions.FillAndExpand };
    var oneBox = new BoxView { VerticalOptions = LayoutOptions.FillAndExpand, HorizontalOptions = LayoutOptions.FillAndExpand };
    var twoBox = new BoxView {  VerticalOptions = LayoutOptions.FillAndExpand, HorizontalOptions = LayoutOptions.FillAndExpand };
    var threeBox = new BoxView {  VerticalOptions = LayoutOptions.FillAndExpand, HorizontalOptions = LayoutOptions.FillAndExpand };

    layout.Children.Add(button);
    layout.Children.Add(oneBox);
    layout.Children.Add(twoBox);
    layout.Children.Add(threeBox);
    Content = layout;
  }
}
```

Další informace najdete v tématu [zarovnání](~/xamarin-forms/user-interface/layouts/layout-options.md#alignment).

## <a name="exploring-a-complex-layout"></a>Zkoumání komplexní rozložení

Každé rozložení mít silné a slabé stránky pro vytvoření konkrétní rozložení. Ukázkovou aplikaci byl vytvořen v rámci této série rozložení články s stejné rozložení stránky implementovaná pomocí tří různých rozložení.

Vezměte v úvahu následující XAML:

```xaml
<?xml version="1.0" encoding="UTF-8"?>
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
x:Class="TheBusinessTumble.StackLayoutPage"
BackgroundColor="Maroon"
Title="StackLayouts">
    <ContentPage.Content>
    <ScrollView>
      <StackLayout Spacing="0" Padding="0" BackgroundColor="Maroon">
        <BoxView HorizontalOptions="FillAndExpand" HeightRequest="100"
          VerticalOptions="Start" Color="Gray" />
        <Button BorderRadius="30" HeightRequest="60" WidthRequest="60"
          BackgroundColor="Red" HorizontalOptions="Center" VerticalOptions="Start" />
        <StackLayout HeightRequest="100" VerticalOptions="Start" HorizontalOptions="FillAndExpand" Spacing="20" BackgroundColor="Maroon">
          <Label Text="User Name" FontSize="28" HorizontalOptions="Center"
            VerticalOptions="Center" FontAttributes="Bold" />
          <Entry Text="Bio + Hashtags" TextColor="White"
            BackgroundColor="Maroon" HorizontalOptions="FillAndExpand" VerticalOptions="CenterAndExpand" />
        </StackLayout>
        <StackLayout Orientation="Horizontal" HeightRequest="50" BackgroundColor="White" Padding="5">
          <StackLayout Spacing="0" BackgroundColor="White" Orientation="Horizontal" HorizontalOptions="Start">
            <BoxView BackgroundColor="Black" WidthRequest="40" HeightRequest="40"  HorizontalOptions="StartAndExpand" VerticalOptions="Center" />
            <Label FontSize="14" TextColor="Black" Text="Accent Color" HorizontalOptions="StartAndExpand" VerticalOptions="Center" />
          </StackLayout>
          <StackLayout Spacing="0" BackgroundColor="White" Orientation="Horizontal" HorizontalOptions="EndAndExpand">
            <BoxView BackgroundColor="Maroon" WidthRequest="40" HeightRequest="40" HorizontalOptions="Start" VerticalOptions="Center" />
            <Label FontSize="14" TextColor="Black" Text="Primary Color" HorizontalOptions="StartAndExpand" VerticalOptions="Center" />
          </StackLayout>
        </StackLayout>
        <StackLayout Orientation="Horizontal">
          <Label FontSize="14" Text="Age:" TextColor="White" HorizontalOptions="Start" VerticalOptions="Center" WidthRequest="100" />
          <Entry HorizontalOptions="FillAndExpand" Text="35" TextColor="White" BackgroundColor="Maroon" />
        </StackLayout>
        <StackLayout Orientation="Horizontal">
          <Label FontSize="14" Text="Interests:" TextColor="White"
          HorizontalOptions="Start" VerticalOptions="Center" WidthRequest="100" />
          <Entry HorizontalOptions="FillAndExpand" Text="Xamarin.Forms" TextColor="White" BackgroundColor="Maroon" />
        </StackLayout>
        <StackLayout Orientation="Horizontal">
          <Label FontSize="14" Text="Ask me about:" TextColor="White"
          HorizontalOptions="Start" VerticalOptions="Center" WidthRequest="100"/>
          <Entry HorizontalOptions="FillAndExpand" Text="Xamarin, C#, .NET, Mono..." TextColor="White" BackgroundColor="Maroon" />
        </StackLayout>
      </StackLayout>
    </ScrollView>
    </ContentPage.Content>
</ContentPage>

```

Ve výše uvedeném kódu má za následek následující rozložení:

![](stack-layout-images/stack.png "Komplexní StackLayout")

Všimněte si, že `StackLayouts`s jsou vnořené, protože v některých případech vnoření rozložení může být snazší než nabízí všechny elementy v rámci stejné rozvržení. Také Všimněte si, že, protože `StackLayout` nepodporuje překrývající se položky stránky není některé z rozložení niceties našli na stránkách pro ostatní rozložení.



## <a name="related-links"></a>Související odkazy

- [LayoutOptions](~/xamarin-forms/user-interface/layouts/layout-options.md)
- [Rozložení (ukázka)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/Layout/)
- [Příklad BusinessTumble (ukázka)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/BusinessTumble/)
