---
title: Referenční dokumentace ovládacích prvků DataPages
description: Tento článek představuje ovládací prvky, které jsou k dispozici v balíčku Xamarin.Forms DataPages NuGet.
ms.prod: xamarin
ms.assetid: 891615D0-E8BD-4ACC-A7F0-4C3725FBCC31
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 12/01/2017
ms.openlocfilehash: c907d55f09d334e167c831a19f9d0edc4c97732f
ms.sourcegitcommit: 632955f8cdb80712abd8dcc30e046cb9c435b922
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2018
ms.locfileid: "38866519"
---
# <a name="datapages-controls-reference"></a>Referenční dokumentace ovládacích prvků DataPages

![](~/media/shared/preview.png "Toto rozhraní API je aktuálně ve verzi preview")

> [!IMPORTANT]
> Vyžaduje DataPages [Xamarin.Forms motiv](~/xamarin-forms/user-interface/themes/index.md) odkaz k vykreslení.


Xamarin.Forms DataPages Nuget obsahuje několik ovládacích prvků, které můžete využít výhod datového zdroje vazby.

V XAML používat tyto ovládací prvky, ujistěte se obor názvů byl součástí, například najdete v článku `xmlns:pages` deklarace níže:

```xaml
<ContentPage
    xmlns="http://xamarin.com/schemas/2014/forms"
    xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
    xmlns:pages="clr-namespace:Xamarin.Forms.Pages;assembly=Xamarin.Forms.Pages"
    x:Class="DataPagesDemo.Detail">
```

Následující příklady zahrnují `DynamicResource` odkazy, které by musela existovat ve slovníku prostředků v projektu pro práci. Také je příklad toho, jak sestavit [vlastního ovládacího prvku](#custom)

## <a name="built-in-controls"></a>Integrovaných ovládacích prvků

* [HeroImage](#heroimage)
* [ListItem](#listitem)

<a name="heroimage" />

### <a name="heroimage"></a>HeroImage

`HeroImage` Ovládací prvek má čtyři vlastnosti:

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

![](controls-images/heroimage-light-android.png "Ovládací prvek HeroImage v Androidu") ![ ] (controls-images/heroimage-dark-android.png "HeroImage ovládacího prvku v Androidu")

**iOS**

![](controls-images/heroimage-light-ios.png "Ovládací prvek HeroImage v Iosu") ![ ] (controls-images/heroimage-dark-ios.png "HeroImage ovládacího prvku v Iosu")


<a name="listitem" />

### <a name="listitem"></a>ListItem

`ListItem` Rozložení ovládacího prvku je podobný nativní aplikace pro iOS a Android seznam nebo tabulku řádků, ale může také sloužit jako regulární zobrazení. V příkladu se zobrazí kód pod ní hostované uvnitř `StackLayout`, ale můžete použít také v ovládacích prvcích seznam scolling vázané na data.

Existuje pět vlastnosti:

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

Tyto snímky obrazovky ukazují `ListItem` na platformy iOS a Android pomocí světlý a tmavý motivy:

**Android**

![](controls-images/listitem-light-android.png "Ovládacího prvku ListItem v Androidu") ![ ] (controls-images/listitem-dark-android.png "ovládacího prvku ListItem v Androidu")

**iOS**

![](controls-images/listitem-light-ios.png "Ovládacího prvku ListItem v Iosu") ![ ] (controls-images/listitem-dark-ios.png "ovládacího prvku ListItem v Iosu")


## <a name="custom-control-example"></a>Příklad vlastního ovládacího prvku

Cílem tuto vlastní `CardView` tak, aby připomínaly nativní Android CardView je ovládací prvek.

Bude obsahovat tři vlastnosti:

* Text
* Podrobnosti
* ImageSource

Cílem je vlastní ovládací prvek, který bude vypadat jako následující kód (Všimněte si, že vlastní `xmlns:local` je povinný, která odkazuje na aktuální sestavení):

```xaml
<local:CardView
  ImageSource="{ DynamicResource CardViewImage }"
  Text="CardView Text"
  Detail="CardView Detail"
/>
```

By měl vypadat jako na snímcích obrazovky níže použití barev odpovídající předdefinované motivy tmavý a světlý motiv:

**Android**

![](controls-images/cardview-light-android.png "CardView vlastní ovládací prvek na Androidu") ![ ] (controls-images/cardview-dark-android.png "CardView vlastní ovládací prvek v Androidu")

**iOS**

![](controls-images/cardview-light-ios.png "CardView vlastní ovládací prvek v Iosu") ![ ] (controls-images/cardview-dark-ios.png "CardView vlastní ovládací prvek v Iosu")

<a name="custom" />

### <a name="building-the-custom-cardview"></a>Vytváření vlastních CardView

1. [Podtřídy třídy DataView](#1)
2. [Zadejte písmo, rozložení a okraje](#2)
3. [Vytvoření styly pro podřízené položky ovládacího prvku](#3)
4. [Vytvořit šablonu rozložení ovládacího prvku](#4)
5. [Přidat prostředky specifické pro konkrétní motiv](#5)
6. [Nastavte element ControlTemplate pro třídu CardView](#6)
7. [Přidání ovládacího prvku na stránku](#7)

<a name="1" />

#### <a name="1-dataview-subclass"></a>1. Podtřídy třídy DataView

Jazyce C# podtřídu `DataView` definuje vlastnosti umožňující vazbu ovládacího prvku.

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

#### <a name="2-define-font-layout-and-margins"></a>2. Zadejte písmo, rozložení a okraje

Návrhář ovládacího prvku by zjistit tyto hodnoty jako součást návrhu uživatelského rozhraní vlastního ovládacího prvku. Pokud jsou povinné, specifikace specifické pro platformu `OnPlatform` element se používá.

Všimněte si, že některé hodnoty se vztahují `StaticResource`s – ty budou určené v [kroku 5](#5).

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

#### <a name="3-create-styles-for-the-controls-children"></a>3. Vytvoření styly pro podřízené položky ovládacího prvku

Odkazovat na všechny prvky definice vytvoření podřízené položky, které se použijí v vlastního ovládacího prvku:

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

Vizuální návrh vlastního ovládacího prvku je explicitně deklarována v šabloně ovládacího prvku pomocí prostředky definované výše:

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

#### <a name="5-add-the-theme-specific-resources"></a>5. Přidat prostředky specifické pro konkrétní motiv

Protože se jedná vlastní ovládací prvek, přidejte prostředky, které odpovídají motiv používáte slovník prostředků:

##### <a name="light-theme-colors"></a>Barvy světlý motiv

```xaml
<Color x:Key="iOSCardViewBackgroundColor">#FFFFFF</Color>
<Color x:Key="AndroidCardViewBackgroundColor">#FFFFFF</Color>

<Color x:Key="AndroidCardViewTextTextColor">#030303</Color>
<Color x:Key="iOSCardViewTextTextColor">#030303</Color>

<Color x:Key="AndroidCardViewDetailTextColor">#8F8E94</Color>
<Color x:Key="iOSCardViewDetailTextColor">#8F8E94</Color>
```

##### <a name="dark-theme-colors"></a>Motiv tmavé barvy

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

#### <a name="6-set-the-controltemplate-for-the-cardview-class"></a>6. Nastavte element ControlTemplate pro třídu CardView

Nakonec se ujistěte, třída jazyka C# vytvořené v [kroku 1](#1) používá definované v šabloně ovládacího prvku [kroku 4](#4) pomocí `Style` `Setter` – element

```xml
<Style TargetType="local:CardView">
    <Setter Property="ControlTemplate" Value="{ StaticResource CardViewControlControlTemplate }" />
  ... some custom styling omitted
  <Setter Property="BackgroundColor" Value="{ StaticResource CardViewBackgroundColor }" />
</Style>
```

<a name="7" />

#### <a name="7-add-the-control-to-a-page"></a>7. Přidání ovládacího prvku na stránku

`CardView` Ovládací prvek je nyní možné přidat na stránku. V následujícím příkladu je hostované v `StackLayout`:

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
