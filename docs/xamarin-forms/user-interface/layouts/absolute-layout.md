---
title: AbsoluteLayout
description: "AbsoluteLayout použijte k vytvoření uživatelská dokonalou pixelů."
ms.topic: article
ms.prod: xamarin
ms.assetid: 01A5CCE0-AD45-4806-84FD-72C007005B38
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 11/25/2015
ms.openlocfilehash: 227a636db370b2dd3d132c49ad7c39ef42c5f30c
ms.sourcegitcommit: 61f5ecc5a2b5dcfbefdef91664d7460c0ee2f357
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 02/28/2018
---
# <a name="absolutelayout"></a>AbsoluteLayout

[`AbsoluteLayout`](https://developer.xamarin.com/api/type/Xamarin.Forms.AbsoluteLayout/) umisťuje a velikosti podřízené elementy přímo úměrná vlastní velikosti a pozice nebo absolutní hodnoty. Podřízené zobrazení může být umístěné a velikosti pomocí přímo úměrná hodnoty nebo statické hodnoty a přímo úměrná a statické hodnoty můžete kombinovat.

[ ![](absolute-layout-images/layouts-sml.png "Rozložení Xamarin.Forms")](absolute-layout-images/layouts.png "Xamarin.Forms rozložení")

Tento článek se zabývá:

- **[Účel](#Purpose)**  &ndash; běžné používá pro `AbsoluteLayout`.
- **[Využití](#Usage)**  &ndash; použití `AbsoluteLayout` k dosažení požadované návrhu.
  - **[Proporční rozložení](#Proportional_Layouts)**  &ndash; pochopit, jak přímo úměrná hodnoty pracovní v `AbsoluteLayout`.
  - **[Zadání hodnoty](#Specifying_Values)**  &ndash; pochopit, jak jsou určené proporční a absolutní hodnoty.
  - **[Proporční hodnoty](#Proportional_Values)**  &ndash; pochopit, jak přímo úměrná hodnoty fungovat.
    - **[Absolutní hodnoty](#Absolute_Values)**  &ndash; pochopili, jak funguje absolutní hodnoty.

<a name="Purpose" />

## <a name="purpose"></a>Účel

Z důvodu umísťovací model `AbsoluteLayout`, rozložení díky je poměrně přehledné na pozici elementy, aby byly roviny s žádné straně rozložení nebo na střed. S přímo úměrná velikosti a pozice, elementů v `AbsoluteLayout` lze automaticky škálovat pro libovolnou velikost zobrazení. Pro položky, kde by měl škálovat jenom pozici, ale ne podle velikosti můžete ve smíšeném absolutní a přímo úměrná hodnoty.

`AbsoluteLayout` bylo možné použít kdekoli elementy musí být umístěn v rámci zobrazení a je obzvláště užitečná při zarovnání prvky hrany.

<a name="Usage" />

## <a name="usage"></a>Použití

<a name="Proportional_Layouts" />

### <a name="proportional-layouts"></a>Proporční rozložení

`AbsoluteLayout` má jedinečný ukotvení modelu, při němž ukotvení elementu umístěný relativně k jeho element jako element nachází relativně k rozložení přímo úměrná umístění se používá. Pokud absolutní umístění se používá, je ukotvení na (0,0) v rámci zobrazení. To má dvě důležité důsledky:

- Elementy nelze umístit mimo obrazovku pomocí přímo úměrná hodnot.
- Elementy lze spolehlivě umístit na žádné straně rozložení nebo v centru bez ohledu na velikost rozložení nebo zařízení.

`AbsoluteLayout`, jako je `RelativeLayout`, je možné umístit elementy, aby se překrývala.

Poznámka: na následujícím snímku obrazovky ukotvení pole je bílé tečku. Všimněte si vztah mezi ukotvení a do pole, při jejich přesunu prostřednictvím rozložení:

![](absolute-layout-images/anchor-start.png "Ukotvení při spuštění")
![](absolute-layout-images/anchor-center.png "ukotvení v centru")
![](absolute-layout-images/anchor-end.png "ukotvení na konci")

<a name="Specifying_Values" />

### <a name="specifying-values"></a>Zadání hodnoty

Zobrazení v rámci `AbsoluteLayout` jsou umístěny pomocí čtyř hodnot:

- **X** &ndash; (vodorovně) x pozice ukotvení zobrazení
- **Y** &ndash; y (svisle) pozici ukotvení zobrazení
- **Šířka** &ndash; Šířka zobrazení
- **Výška** &ndash; výšku zobrazení

Každý z těchto hodnot můžete nastavit jako [přímo úměrná](#Proportional_Values) hodnotu nebo [absolutní](#Absolute_Values) hodnotu.

Hodnoty jsou specifikované jako kombinace hranice a příznak. `LayoutBounds` je [ `Rectangle` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Rectangle/) sestávající ze čtyř hodnot: `x`, `y`, `width`, `height`.

### <a name="absolutelayoutflags"></a>AbsoluteLayoutFlags

[`AbsoluteLayoutFlags`](https://developer.xamarin.com/api/type/Xamarin.Forms.AbsoluteLayoutFlags/) Určuje, jak se interpretují hodnoty a obsahuje následující předdefinované možnosti:

- **Žádný** &ndash; interpretuje jako absolutní všechny hodnoty. Toto je výchozí hodnota, pokud nejsou zadány žádné příznaky rozložení.
- **Všechny** &ndash; interpretuje jako úměrná všechny hodnoty.
- **WidthProportional** &ndash; interpretuje `Width` hodnotu jako proporční a všechny ostatní hodnoty jako absolutní.
- **HeightProportional** &ndash; interpretuje pouze hodnota height jako úměrná s všechny ostatní hodnoty absolutní.
- **XProportional** &ndash; interpretuje `X` hodnotu jako úměrná, při práce jako absolutní všechny ostatní hodnoty.
- **YProportional** &ndash; interpretuje `Y` hodnotu jako úměrná, při práce jako absolutní všechny ostatní hodnoty.
- **PositionProportional** &ndash; interpretuje `X` a `Y` hodnoty jako úměrná, zatímco hodnoty velikosti se interpretují jako absolutní.
- **SizeProportional** &ndash; interpretuje `Width` a `Height` hodnoty jako úměrná, pokud jsou absolutní hodnoty pozice.

V jazyce XAML, rozsah a příznaky jsou nastavené jako součást definice zobrazení v rozložení, pomocí `AbsoluteLayout.LayoutBounds` vlastnost. Hranice jsou nastavené jako textový soubor s oddělovači seznam hodnot, `X`, `Y`, `Width`, a `Height`, v tomto pořadí. Příznaky jsou také zadaný v deklaraci zobrazení v rozložení pomocí `AbsoluteLayout.LayoutFlags` vlastnost. Všimněte si, že příznaky mohou být kombinovány v jazyce XAML pomocí seznam oddělený čárkami. Podívejte se na následující příklad:

```xaml
<?xml version="1.0" encoding="UTF-8"?>
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
x:Class="LayoutSamples.AbsoluteLayoutExploration"
Title="Absolute Layout Exploration">
    <ContentPage.Content>
        <AbsoluteLayout>
            <Label Text="I'm centered on iPhone 4 but no other device"
                AbsoluteLayout.LayoutBounds="115,150,100,100" LineBreakMode="WordWrap"  />
            <Label Text="I'm bottom center on every device."
                AbsoluteLayout.LayoutBounds=".5,1,.5,.1" AbsoluteLayout.LayoutFlags="All"
                LineBreakMode="WordWrap"  />
            <BoxView Color="Olive"  AbsoluteLayout.LayoutBounds="1,.5, 25, 100"
                AbsoluteLayout.LayoutFlags="PositionProportional" />
            <BoxView Color="Red" AbsoluteLayout.LayoutBounds="0,.5,25,100"
                AbsoluteLayout.LayoutFlags="PositionProportional" />
            <BoxView Color="Blue" AbsoluteLayout.LayoutBounds=".5,0,100,25"
                AbsoluteLayout.LayoutFlags="PositionProportional" />
        </AbsoluteLayout>
    </ContentPage.Content>
</ContentPage>
```

![](absolute-layout-images/exploration.png "Příklady AbsoluteLayout")

Vezměte na vědomí následující:

- Popisek v centru umístěn pomocí absolutní hodnoty velikosti a pozice. Z tohoto důvodu zobrazí se na zařízení iPhone 4S na střed a nižší, ale není zarovnaný na větší zařízení.
- Text v dolní části rozložení je nastavený pomocí přímo úměrná velikosti a pozice hodnot. Vždy zobrazí v centru dolní rozložení, ale s větší velikostí rozložení se zvýší jeho velikost.
- Tři stejné barvy `BoxView`s jsou umístěny na horní, levé a pravé hrany obrazovky pomocí přímo úměrná pozice a velikosti absolutní.

Následující dosahuje se stejné rozvržení v jazyce C#:

```csharp
public class AbsoluteLayoutExplorationCode : ContentPage
{
    public AbsoluteLayoutExplorationCode ()
    {
        Title = "Absolute Layout Exploration - Code";
        var layout = new AbsoluteLayout();

        var centerLabel = new Label {
        Text = "I'm centered on iPhone 4 but no other device.",
        LineBreakMode = LineBreakMode.WordWrap};

        AbsoluteLayout.SetLayoutBounds (centerLabel, new Rectangle (115, 159, 100, 100));
        // No need to set layout flags, absolute positioning is the default

        var bottomLabel = new Label { Text = "I'm bottom center on every device.", LineBreakMode = LineBreakMode.WordWrap };
        AbsoluteLayout.SetLayoutBounds (bottomLabel, new Rectangle (.5, 1, .5, .1));
        AbsoluteLayout.SetLayoutFlags (bottomLabel, AbsoluteLayoutFlags.All);

        var rightBox = new BoxView{ Color = Color.Olive };
        AbsoluteLayout.SetLayoutBounds (rightBox, new Rectangle (1, .5, 25, 100));
        AbsoluteLayout.SetLayoutFlags (rightBox, AbsoluteLayoutFlags.PositionProportional);

        var leftBox = new BoxView{ Color = Color.Red };
        AbsoluteLayout.SetLayoutBounds (leftBox, new Rectangle (0, .5, 25, 100));
        AbsoluteLayout.SetLayoutFlags (leftBox, AbsoluteLayoutFlags.PositionProportional);

        var topBox = new BoxView{ Color = Color.Blue };
        AbsoluteLayout.SetLayoutBounds (topBox, new Rectangle (.5, 0, 100, 25));
        AbsoluteLayout.SetLayoutFlags (topBox, AbsoluteLayoutFlags.PositionProportional);

        layout.Children.Add (bottomLabel);
        layout.Children.Add (centerLabel);
        layout.Children.Add (rightBox);
        layout.Children.Add (leftBox);
        layout.Children.Add (topBox);

        Content = layout;
    }
}
```
<a name="Proportional_Values" />

### <a name="proportional-values"></a>Proporční hodnoty

Proporční hodnoty definovat vztah mezi rozložení a zobrazení. Tento vztah definuje jako podíl s odpovídající hodnotou rozložení nadřazené podřízené zobrazení pozici nebo hodnota měřítka. Tyto hodnoty jsou vyjádřené jako `double`s hodnotami mezi 0 a 1.

Proporční hodnoty se používají k pozice a velikosti zobrazení v rozložení. Ano, Šířka zobrazení je nastaven jako část, hodnota výsledné width při poměr násobí hodnotou `AbsoluteLayout`na šířku. Například s `AbsoluteLayout` šířky `500` a zobrazení nastavit tak, aby měl přímo úměrná šířku.5, šířku vykreslené zobrazení bude 250 (500 ×.5).

Chcete-li použít přímo úměrná hodnoty, nastavte `LayoutBounds` pomocí (x, y) proporce a přímo úměrná velikosti, nastavte `LayoutFlags` k `All`.

V jazyce XAML:

```xaml
<Label Text="I'm bottom center on every device."
  AbsoluteLayout.LayoutBounds=".5,1,.5,.1" AbsoluteLayout.LayoutFlags="All"  />
```

V jazyce C#:

```csharp
var label = new Label {Text = "I'm bottom center on every device."};
AbsoluteLayout.SetLayoutBounds(label, new Rectangle(.5,1,.5,.1));
AbsoluteLayout.SetLayoutFlags(label, AbsoluteLayoutFlags.All);
```

<a name="Absolute_Values" />

### <a name="absolute-values"></a>Absolutní hodnoty

Absolutní hodnoty explicitně definujte, kde má být umístěn zobrazení v rámci rozložení. Na rozdíl od přímo úměrná hodnoty jsou schopná využívat umístění a velikost zobrazení, který se nevejde do hranice rozložení absolutní hodnoty.

Pomocí absolutní hodnoty pro umístění může být nebezpečné, když velikost rozložení není znám. Pokud používáte absolutní umístění, element ve středu obrazovky na velikosti může posunu v jiné velikosti. Je důležité k testování aplikace napříč různými velikost obrazovky zařízení podporované.

Chcete-li použít absolutní rozložení hodnoty, nastavte `LayoutBounds` pomocí (x, y) souřadnice explicitní velikost a poté nastavte `LayoutFlags` k `None`.

V jazyce XAML:

```xaml
<Label Text="I'm centered on iPhone 4 but no other device."
  AbsoluteLayout.LayoutBounds="115,150,100,100"  />
```

V jazyce C#:

```csharp
var label = new Label {Text = "I'm centered on iPhone 4 but no other device."};
AbsoluteLayout.SetLayoutBounds(label, new Rectangle(115,150,100,100));
```

## <a name="exploring-a-complex-layout"></a>Zkoumání komplexní rozložení

Každé rozložení mít silné a slabé stránky pro vytvoření konkrétní rozložení. Ukázkovou aplikaci byl vytvořen v rámci této série rozložení články s stejné rozložení stránky implementovaná pomocí tří různých rozložení.

Vezměte v úvahu následující XAML:

```xaml
<?xml version="1.0" encoding="UTF-8"?>
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
x:Class="TheBusinessTumble.AbsoluteLayoutPage"
Title="AbsoluteLayout">
    <ContentPage.ToolbarItems>
        <ToolbarItem Text="Save" />
    </ContentPage.ToolbarItems>
    <ContentPage.Content>
        <ScrollView>
            <AbsoluteLayout BackgroundColor="Maroon">
                <BoxView BackgroundColor="Gray" AbsoluteLayout.LayoutBounds="0
                    0,1,100" AbsoluteLayout.LayoutFlags="XProportional,YProportional,WidthProportional" />
                <Button BackgroundColor="Maroon"
                    AbsoluteLayout.LayoutBounds=".5,55,70,70" AbsoluteLayout.LayoutFlags="XProportional" BorderRadius="35" />
                <Button BackgroundColor="Red" AbsoluteLayout.LayoutBounds=".5
                    60,60,60" AbsoluteLayout.LayoutFlags="XProportional" BorderRadius="30" />
                <Label Text="User Name" FontAttributes="Bold" FontSize="26"
                    TextColor="Black" HorizontalTextAlignment="Center" AbsoluteLayout.LayoutBounds=".5,140,1,40" AbsoluteLayout.LayoutFlags="XProportional,WidthProportional" />
                <Entry Text="Bio + Hashtags" TextColor="White"
                    BackgroundColor="Maroon" AbsoluteLayout.LayoutBounds=".5,180,1,40" AbsoluteLayout.LayoutFlags="XProportional,WidthProportional" />
                <AbsoluteLayout BackgroundColor="White"
                        AbsoluteLayout.LayoutBounds="0, 220, 1, 50" AbsoluteLayout.LayoutFlags="XProportional,WidthProportional">
                    <AbsoluteLayout AbsoluteLayout.LayoutBounds="0,0,.5,1" AbsoluteLayout.LayoutFlags="WidthProportional,HeightProportional">
                        <Button BackgroundColor="Black" BorderRadius="20"
                            AbsoluteLayout.LayoutBounds="5,.5,40,40" AbsoluteLayout.LayoutFlags="YProportional" />
                        <Label Text="Accent Color" TextColor="Black"
                            AbsoluteLayout.LayoutBounds="50,.55,1,25" AbsoluteLayout.LayoutFlags="YProportional,WidthProportional" />
                    </AbsoluteLayout>
                    <AbsoluteLayout AbsoluteLayout.LayoutBounds="1,0,.5,1" AbsoluteLayout.LayoutFlags="WidthProportional,HeightProportional,XProportional">
                        <Button BackgroundColor="Maroon" BorderRadius="20"
                            AbsoluteLayout.LayoutBounds="5,.5,40,40" AbsoluteLayout.LayoutFlags="YProportional" />
                        <Label Text="Primary Color" TextColor="Black"
                            AbsoluteLayout.LayoutBounds="50,.55,1,25" AbsoluteLayout.LayoutFlags="YProportional,WidthProportional" />
                    </AbsoluteLayout>
                </AbsoluteLayout>
                <AbsoluteLayout AbsoluteLayout.LayoutBounds="0,270,1,50" AbsoluteLayout.LayoutFlags="WidthProportional" Padding="5,0,0,0">
                    <Label Text="Age:" TextColor="White"
                        AbsoluteLayout.LayoutBounds="0,25,.25,50" AbsoluteLayout.LayoutFlags="WidthProportional" />
                    <Entry Text="35" TextColor="White" BackgroundColor="Maroon"
                        AbsoluteLayout.LayoutBounds="1,10,.75,50" AbsoluteLayout.LayoutFlags="XProportional,WidthProportional" />
                </AbsoluteLayout>
                <AbsoluteLayout AbsoluteLayout.LayoutBounds="0,320,1,50" AbsoluteLayout.LayoutFlags="WidthProportional" Padding="5,0,0,0">
                    <Label Text="Interests:" TextColor="White"
                        AbsoluteLayout.LayoutBounds="0,25,.25,50" AbsoluteLayout.LayoutFlags="WidthProportional" />
                    <Entry Text="Xamarin.Forms" TextColor="White"
                        BackgroundColor="Maroon" AbsoluteLayout.LayoutBounds="1,10,.75,50" AbsoluteLayout.LayoutFlags="XProportional,WidthProportional" />
                </AbsoluteLayout>
                <AbsoluteLayout AbsoluteLayout.LayoutBounds="0,370,1,50" AbsoluteLayout.LayoutFlags="WidthProportional" Padding="5,0,0,0">
                    <Label Text="Ask me about:" TextColor="White"
                        AbsoluteLayout.LayoutBounds="0,25,.25,50" AbsoluteLayout.LayoutFlags="WidthProportional" />
                    <Entry Text="Xamarin, C#, .NET, Mono" TextColor="White"
                        BackgroundColor="Maroon" AbsoluteLayout.LayoutBounds="1,10,.75,50" AbsoluteLayout.LayoutFlags="XProportional,WidthProportional" />
                </AbsoluteLayout>
            </AbsoluteLayout>
        </ScrollView>
    </ContentPage.Content>
</ContentPage>
```

Ve výše uvedeném kódu má za následek následující rozložení:

![](absolute-layout-images/abs.png "Komplexní AbsoluteLayout")

Všimněte si, že kvůli rozdílu ve způsobu vykreslení tlačítka ve Windows Phone, některé kroužky byly nahrazeny boxviews na snímku obrazovky Windows Phone.

Všimněte si, že `AbsoluteLayout`s jsou vnořené, protože v některých případech vnoření rozložení může být snazší než nabízí všechny elementy v rámci stejné rozvržení.



## <a name="related-links"></a>Související odkazy

- [Vytváření mobilních aplikací s Xamarin.Forms, kapitoly 14](https://developer.xamarin.com/r/xamarin-forms/book/chapter14.pdf)
- [AbsoluteLayout](https://developer.xamarin.com/api/type/Xamarin.Forms.AbsoluteLayout/)
- [Rozložení (ukázka)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/Layout/)
- [Příklad BusinessTumble (ukázka)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/BusinessTumble/)
