---
title: Referenční dokumentace ovládacích prvků DataPages
ms.prod: xamarin
ms.assetid: 891615D0-E8BD-4ACC-A7F0-4C3725FBCC31
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 12/01/2017
ms.openlocfilehash: 912733dabd5a1baa00f1b88e59c5fe305b375605
ms.sourcegitcommit: d80d93957040a14b4638a91b0eac797cfaade840
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 06/07/2018
ms.locfileid: "34846816"
---
# <a name="datapages-controls-reference"></a>Referenční dokumentace ovládacích prvků DataPages

![](~/media/shared/preview.png "Toto rozhraní API je aktuálně ve verzi preview")

> [!IMPORTANT]
> Vyžaduje DataPages [Xamarin.Forms motiv](~/xamarin-forms/user-interface/themes/index.md) odkaz k vykreslení.


Xamarin.Forms DataPages Nuget obsahuje řadu ovládacích prvků, které můžete využít výhod datového zdroje vazby.

Pokud chcete použít tyto ovládací prvky v jazyce XAML, zkontrolujte, obor názvů byl součástí najdete například `xmlns:pages` deklarace níže:

```xaml
<ContentPage
    xmlns="http://xamarin.com/schemas/2014/forms"
    xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
    xmlns:pages="clr-namespace:Xamarin.Forms.Pages;assembly=Xamarin.Forms.Pages"
    x:Class="DataPagesDemo.Detail">
```

Následující příklady zahrnují `DynamicResource` odkazy, které by bylo potřeba existovat ve slovníku prostředky projektu pro práci. Je také příklad toho, jak vytvořit [vlastního ovládacího prvku](#custom)

## <a name="built-in-controls"></a>Integrované ovládací prvky

* [HeroImage](#heroimage)
* [ListItem](#listitem)

<a name="heroimage" />

### <a name="heroimage"></a>HeroImage

`HeroImage` Řízení má čtyři vlastnosti:

* Text
* Podrobnosti
* ImageSource
* Aspekt

```xaml
<pages:HeroImage
    ImageSource="{ DynamicResource HeroImageImage }"
    Text="Keith Ballinger"
    Detail="Xamarin"
/>
```

**Android**

![](controls-images/heroimage-light-android.png "Ovládací prvek HeroImage v systému Android") ![ ] (controls-images/heroimage-dark-android.png "HeroImage ovládací prvek v systému Android")

**iOS**

![](controls-images/heroimage-light-ios.png "Ovládací prvek HeroImage v iOS") ![ ] (controls-images/heroimage-dark-ios.png "HeroImage ovládací prvek v iOS")


<a name="listitem" />

### <a name="listitem"></a>ListItem

`ListItem` Rozložení ovládacího prvku je podobný nativní aplikace pro iOS a Android seznamu nebo tabulky řádků, ale může taky sloužit jako regulární zobrazení. V příkladu se zobrazí kód pod ním hostované uvnitř `StackLayout`, ale můžou používat i v ovládacích prvcích seznam scolling vázané na data.

Nejsou k dispozici pět vlastnosti:

* Název
* Podrobnosti
* ImageSource
* PlaceholdImageSource
* Aspekt

```xaml
<StackLayout Spacing="0">
    <pages:ListItemControl
        Detail="Xamarin"
        ImageSource="{ DynamicResource UserImage }"
        Title="Miguel de Icaza"
        PlaceholdImageSource="{ DynamicResource IconImage }"
    />
```

Tyto snímky obrazovky ukazují `ListItem` na iOS a Android platforem a jak tmavý a světlý motivů:

**Android**

![](controls-images/listitem-light-android.png "Ovládacího prvku ListItem v systému Android") ![ ] (controls-images/listitem-dark-android.png "ovládacího prvku ListItem v systému Android")

**iOS**

![](controls-images/listitem-light-ios.png "Ovládacího prvku ListItem v iOS") ![ ] (controls-images/listitem-dark-ios.png "ovládacího prvku ListItem v iOS")


## <a name="custom-control-example"></a>Příklad vlastního ovládacího prvku

Cílem tento vlastní `CardView` ovládací prvek je tak, aby připomínaly nativní Android zobrazení karty aplikace.

Bude obsahovat tři vlastnosti:

* Text
* Podrobnosti
* ImageSource

Cílem je vlastní ovládací prvek, který bude vypadat podobně jako následující kód (Všimněte si, že vlastní `xmlns:local` je vyžadován, odkazuje na aktuální sestavení):

```xaml
<local:CardView
  ImageSource="{ DynamicResource CardViewImage }"
  Text="CardView Text"
  Detail="CardView Detail"
/>
```

By měl vypadat jako na snímcích obrazovky níže pomocí barev odpovídající předdefinované motivy tmavý a světlý:

**Android**

![](controls-images/cardview-light-android.png "Zobrazení karty aplikace vlastní ovládací prvek v systému Android") ![ ] (controls-images/cardview-dark-android.png "zobrazení karty aplikace vlastní ovládací prvek v systému Android")

**iOS**

![](controls-images/cardview-light-ios.png "Zobrazení karty aplikace vlastní ovládací prvek v systému iOS") ![ ] (controls-images/cardview-dark-ios.png "zobrazení karty aplikace vlastní ovládací prvek v systému iOS")

<a name="custom" />

### <a name="building-the-custom-cardview"></a>Vytváření vlastních zobrazení karty aplikace

1. [Podtřída zobrazení dat](#1)
2. [Zadejte písma, rozložení a okraje](#2)
3. [Vytvoření stylů pro podřízené položky ovládacího prvku](#3)
4. [Vytvořit šablonu rozložení ovládacího prvku](#4)
5. [Přidat prostředky specifické pro motiv](#5)
6. [Nastavit ControlTemplate pro zobrazení karty aplikace – třída](#6)
7. [Přidání ovládacího prvku na stránku](#7)

<a name="1" />

#### <a name="1-dataview-subclass"></a>1. Podtřída zobrazení dat

Podtřídami C# třídy `DataView` definuje vazbu vlastnosti pro ovládací prvek.

```csharp
public class CardView : DataView
{
    public static readonly BindableProperty TextProperty =
        BindableProperty.Create ("Text", typeof (string), typeof (CardView), null, BindingMode.TwoWay);

    public string Text
    {
        get { return (string)GetValue (TextProperty); }
        set { SetValue (TextProperty, value); }
    }

    public static readonly BindableProperty DetailProperty =
        BindableProperty.Create ("Detail", typeof (string), typeof (CardView), null, BindingMode.TwoWay);

    public string Detail
    {
        get { return (string)GetValue (DetailProperty); }
        set { SetValue (DetailProperty, value); }
    }

    public static readonly BindableProperty ImageSourceProperty =
        BindableProperty.Create ("ImageSource", typeof (ImageSource), typeof (CardView), null, BindingMode.TwoWay);

    public ImageSource ImageSource
    {
        get { return (ImageSource)GetValue (ImageSourceProperty); }
        set { SetValue (ImageSourceProperty, value); }
    }

    public CardView()
    {
    }
}
```

<a name="2" />

#### <a name="2-define-font-layout-and-margins"></a>2. Zadejte písma, rozložení a okraje

Návrhář ovládacího prvku by rozmyslete si tyto hodnoty jako součást návrh uživatelského rozhraní pro vlastní ovládací prvek. Tam, kde jsou povinné, specifické pro platformu specifikace `OnPlatform` element se používá.

Všimněte si, že některé hodnoty se vztahují `StaticResource`s – ty budou určené v [krok 5](#5).

```xml
<!-- CARDVIEW FONT SIZES -->
<OnPlatform x:TypeArguments="x:Double" x:Key="CardViewTextFontSize">
        <On Platform="iOS, Android" Value="15" />
</OnPlatform>

<OnPlatform x:TypeArguments="x:Double" x:Key="CardViewDetailFontSize">
        <On Platform="iOS, Android" Value="13" />
</OnPlatform>

<OnPlatform x:TypeArguments="Color"    x:Key="CardViewTextTextColor">
        <On Platform="iOS" Value="{StaticResource iOSCardViewTextTextColor}" />
        <On Platform="Android" Value="{StaticResource AndroidCardViewTextTextColor}" />
</OnPlatform>

<OnPlatform x:TypeArguments="Thickness"    x:Key="CardViewTextlMargin">
        <On Platform="iOS" Value="12,10,12,4" />
        <On Platform="Android" Value="20,0,20,5" />
</OnPlatform>

<OnPlatform x:TypeArguments="Color"    x:Key="CardViewDetailTextColor">
        <On Platform="iOS" Value="{StaticResource iOSCardViewDetailTextColor}" />
        <On Platform="Android" Value="{StaticResource AndroidCardViewDetailTextColor}" />
</OnPlatform>

<OnPlatform x:TypeArguments="Thickness"    x:Key="CardViewDetailMargin">
        <On Platform="iOS" Value="12,0,10,12" />
        <On Platform="Android" Value="20,0,20,20" />
</OnPlatform>

<OnPlatform x:TypeArguments="Color"    x:Key="CardViewBackgroundColor">
        <On Platform="iOS" Value="{StaticResource iOSCardViewBackgroundColor}" />
        <On Platform="Android" Value="{StaticResource AndroidCardViewBackgroundColor}" />
</OnPlatform>

<OnPlatform x:TypeArguments="x:Double" x:Key="CardViewShadowSize">
        <On Platform="iOS" Value="2" />
        <On Platform="Android" Value="5" />
</OnPlatform>

<OnPlatform x:TypeArguments="x:Double" x:Key="CardViewCornerRadius">
        <On Platform="iOS" Value="0" />
        <On Platform="Android" Value="4" />
</OnPlatform>

<OnPlatform x:TypeArguments="Color"    x:Key="CardViewShadowColor">
        <On Platform="iOS, Android" Value="#CDCDD1" />
</OnPlatform>
```

<a name="3" />

#### <a name="3-create-styles-for-the-controls-children"></a>3. Vytvoření stylů pro podřízené položky ovládacího prvku

Odkazy na všechny prvky, které jsou definované Chystáte se vytvořit podřízené objekty, které se použijí v vlastního ovládacího prvku:

```xml
<!-- EXPLICIT STYLES (will be Classes) -->
<Style TargetType="Label" x:Key="CardViewTextStyle">
    <Setter Property="FontSize" Value="{ StaticResource CardViewTextFontSize }" />
    <Setter Property="TextColor" Value="{ StaticResource CardViewTextTextColor }" />
    <Setter Property="HorizontalOptions" Value="Start" />
    <Setter Property="Margin" Value="{ StaticResource CardViewTextlMargin }" />
    <Setter Property="HorizontalTextAlignment" Value="Start" />
</Style>

<Style TargetType="Label" x:Key="CardViewDetailStyle">
    <Setter Property="HorizontalTextAlignment" Value="Start" />
    <Setter Property="TextColor" Value="{ StaticResource CardViewDetailTextColor }" />
    <Setter Property="FontSize" Value="{ StaticResource CardViewDetailFontSize }" />
    <Setter Property="HorizontalOptions" Value="Start" />
    <Setter Property="Margin" Value="{ StaticResource CardViewDetailMargin }" />
</Style>

<Style TargetType="Image" x:Key="CardViewImageImageStyle">
    <Setter Property="HorizontalOptions" Value="Center" />
    <Setter Property="VerticalOptions" Value="Center" />
    <Setter Property="WidthRequest" Value="220"/>
    <Setter Property="HeightRequest" Value="165"/>
</Style>
```

<a name="4" />

#### <a name="4-create-the-control-layout-template"></a>4. Vytvořit šablonu rozložení ovládacího prvku

Vizuální návrh vlastní ovládací prvek je explicitně deklarován v šabloně ovládacího prvku pomocí prostředky definované výše:

```xml
<!--- CARDVIEW -->
<ControlTemplate x:Key="CardViewControlControlTemplate">
  <StackLayout
    Spacing="0"
    BackgroundColor="{ TemplateBinding BackgroundColor }"
  >

    <!-- CARDVIEW IMAGE -->
    <Image
      Source="{ TemplateBinding ImageSource }"
      HorizontalOptions="FillAndExpand"
      VerticalOptions="StartAndExpand"
      Aspect="AspectFill"
      Style="{ StaticResource CardViewImageImageStyle }"
    />

    <!-- CARDVIEW TEXT -->
    <Label
      Text="{ TemplateBinding Text }"
      LineBreakMode="WordWrap"
      VerticalOptions="End"
      Style="{ StaticResource CardViewTextStyle }"
    />


    <!-- CARDVIEW DETAIL -->
    <Label
      Text="{ TemplateBinding Detail }"
      LineBreakMode="WordWrap"
      VerticalOptions="End"
      Style="{ StaticResource CardViewDetailStyle }" />

  </StackLayout>

</ControlTemplate>
```

<a name="5" />

#### <a name="5-add-the-theme-specific-resources"></a>5. Přidat prostředky specifické pro motiv

Protože se jedná vlastní ovládací prvek, přidejte prostředky, které splňují motiv používáte slovník prostředků:

##### <a name="light-theme-colors"></a>Motiv světlý barvy

```xaml
<Color x:Key="iOSCardViewBackgroundColor">#FFFFFF</Color>
<Color x:Key="AndroidCardViewBackgroundColor">#FFFFFF</Color>

<Color x:Key="AndroidCardViewTextTextColor">#030303</Color>
<Color x:Key="iOSCardViewTextTextColor">#030303</Color>

<Color x:Key="AndroidCardViewDetailTextColor">#8F8E94</Color>
<Color x:Key="iOSCardViewDetailTextColor">#8F8E94</Color>
```

##### <a name="dark-theme-colors"></a>Tmavý motiv barvy

```xaml
<!-- CARD VIEW COLORS -->
            <Color x:Key="iOSCardViewBackgroundColor">#404040</Color>
            <Color x:Key="AndroidCardViewBackgroundColor">#404040</Color>

            <Color x:Key="AndroidCardViewTextTextColor">#FFFFFF</Color>
            <Color x:Key="iOSCardViewTextTextColor">#FFFFFF</Color>

            <Color x:Key="AndroidCardViewDetailTextColor">#B5B4B9</Color>
            <Color x:Key="iOSCardViewDetailTextColor">#B5B4B9</Color>
```

<a name="6" />

#### <a name="6-set-the-controltemplate-for-the-cardview-class"></a>6. Nastavit ControlTemplate pro zobrazení karty aplikace – třída

Nakonec se ujistěte, třída C# vytvořené v [krok 1](#1) používá šablonu ovládacího prvku definované v [krok 4](#4) pomocí `Style` `Setter` – element

```xml
<Style TargetType="local:CardView">
    <Setter Property="ControlTemplate" Value="{ StaticResource CardViewControlControlTemplate }" />
  ... some custom styling omitted
  <Setter Property="BackgroundColor" Value="{ StaticResource CardViewBackgroundColor }" />
</Style>
```

<a name="7" />

#### <a name="7-add-the-control-to-a-page"></a>7. Přidání ovládacího prvku na stránku

`CardView` Ovládací prvek je nyní možné přidat na stránku. Následující příklad ukazuje ho hostované v `StackLayout`:

```xaml
<StackLayout Spacing="0">
  <local:CardView
    Margin="12,6"
    ImageSource="{ DynamicResource CardViewImage }"
    Text="CardView Text"
    Detail="CardView Detail"
  />
</StackLayout>

```
