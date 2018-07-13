---
title: Xamarin.Forms AbsoluteLayout
description: Tento článek vysvětluje, jak použít třídu Xamarin.Forms AbsoluteLayout pro vytváření uživatelského rozhraní perfektním vzhledem. Tato třída pozice a velikosti podřízené prvky přímo úměrná velikosti a pozice nebo absolutní hodnoty.
ms.prod: xamarin
ms.assetid: 01A5CCE0-AD45-4806-84FD-72C007005B38
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 11/25/2015
ms.openlocfilehash: 0d49b8c50db08ad07952425492591ee246de4f8b
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/12/2018
ms.locfileid: "38998345"
---
# <a name="xamarinforms-absolutelayout"></a>Xamarin.Forms AbsoluteLayout

[`AbsoluteLayout`](xref:Xamarin.Forms.AbsoluteLayout) umístění a velikosti podřízené prvky přímo úměrná velikosti a pozice nebo absolutní hodnoty. Podřízené zobrazení mohou být umístěné a velikosti pomocí proporcionální hodnoty nebo statickými hodnotami a proporcionální a statické hodnoty lze kombinovat.

[![](absolute-layout-images/layouts-sml.png "Rozložení Xamarin.Forms")](absolute-layout-images/layouts.png#lightbox "rozložení Xamarin.Forms")

Tento článek se zabývá:

- **[Účel](#Purpose)**  &ndash; běžné použití pro `AbsoluteLayout`.
- **[Využití](#Usage)**  &ndash; použití `AbsoluteLayout` k dosažení požadované návrhu.
  - **[Proporcionální rozložení](#Proportional_Layouts)**  &ndash; pochopit, jak poměrné hodnoty fungují `AbsoluteLayout`.
  - **[Zadání hodnot](#Specifying_Values)**  &ndash; pochopit, jak zadat proporcionálně a absolutní hodnoty.
  - **[Proporcionální hodnoty](#Proportional_Values)**  &ndash; pochopit, jak poměrné hodnoty fungovat.
    - **[Absolutní hodnoty](#Absolute_Values)**  &ndash; pochopit, jak fungují absolutní hodnoty.

<a name="Purpose" />

## <a name="purpose"></a>Účel

Z důvodu umístění model `AbsoluteLayout`, rozložení je poměrně přímočarý do umístění prvků tak, aby byly flush všechny strany rozložení, nebo na střed. S přímo úměrná velikosti a pozice prvků v `AbsoluteLayout` může automaticky škálovat na libovolnou velikost zobrazení. Pro položky, ve kterém by se měly škálovat pouze na pozici, ale nikoli velikost absolutní a proporcionální hodnoty lze kombinovat.

`AbsoluteLayout` může být použít všude, kde prvky musí být umístěn v rámci zobrazení a je zvláště užitečné, když zarovnání prvků, které mají hran.

<a name="Usage" />

## <a name="usage"></a>Použití

<a name="Proportional_Layouts" />

### <a name="proportional-layouts"></a>Proporcionální rozložení

`AbsoluteLayout` má jedinečné ukotvení model, kterým ukotvení elementu, který je umístěn vzhledem k jeho element jako prvek umístěn vzhledem k rozložení proporcionální umístění se používá. Při použití absolutní pozici ukotvení je na (0,0) v rámci zobrazení. To má dvě důležité důsledky:

- Elementy nelze umístit, obrazovky pomocí proporcionální hodnot.
- Prvky může být spolehlivě umístěné společně na žádné straně rozložení nebo v centru bez ohledu na velikost rozložení nebo zařízení.

`AbsoluteLayout`, jako je `RelativeLayout`, je možné umístit prvky tak, aby se překrývají.

Poznámka: na následujícím snímku obrazovky ukotvení pole je bílé tečku. Všimněte si, že vztah mezi ukotvení a do pole při jejich přesunu prostřednictvím rozložení:

![](absolute-layout-images/anchor-start.png "Ukotvení na začátku")
![](absolute-layout-images/anchor-center.png "ukotvení v centru")
![](absolute-layout-images/anchor-end.png "ukotvení na konci")

<a name="Specifying_Values" />

### <a name="specifying-values"></a>Zadání hodnot

Zobrazení v rámci `AbsoluteLayout` jsou umístěny pomocí čtyři hodnoty:

- **X** &ndash; (vodorovně) x pozice ukotvení zobrazení
- **Y** &ndash; pozice y (svislé) zobrazení ukotvení
- **Šířka** &ndash; Šířka zobrazení
- **Výška** &ndash; výšku zobrazení

Každá z těchto hodnot je možné nastavit jako [proporcionální](#Proportional_Values) hodnotu nebo [absolutní](#Absolute_Values) hodnotu.

Hodnoty jsou specifikované jako kombinace omezení a příznak. `LayoutBounds` je [ `Rectangle` ](xref:Xamarin.Forms.Rectangle) sestávající ze čtyř hodnot: `x`, `y`, `width`, `height`.

### <a name="absolutelayoutflags"></a>AbsoluteLayoutFlags

[`AbsoluteLayoutFlags`](xref:Xamarin.Forms.AbsoluteLayoutFlags) Určuje, jak bude interpretovat hodnoty a obsahuje následující předdefinované možnosti:

- **Žádný** &ndash; interpretuje všechny hodnoty jako absolutní. Toto je výchozí hodnota, pokud nejsou zadány žádné příznaky rozložení.
- **Všechny** &ndash; interpretuje jako proporcionální všechny hodnoty.
- **WidthProportional** &ndash; interpretuje `Width` hodnotu jako proporcionálně a všechny ostatní hodnoty jako absolutní.
- **HeightProportional** &ndash; interpretuje pouze hodnota výšky jako proporcionální se všemi ostatními hodnotami absolutní.
- **XProportional** &ndash; interpretuje `X` hodnotu jako proporcionální, nakládá všechny ostatní hodnoty jako absolutní.
- **YProportional** &ndash; interpretuje `Y` hodnotu jako proporcionální, nakládá všechny ostatní hodnoty jako absolutní.
- **PositionProportional** &ndash; interpretuje `X` a `Y` hodnoty jako proporcionální, zatímco hodnoty velikosti se interpretují jako absolutní.
- **SizeProportional** &ndash; interpretuje `Width` a `Height` podle proporcionální hodnoty jsou absolutní hodnoty pozice.

V XAML, hranice a příznaky jsou nastaveny jako součást definice zobrazení v rámci rozložení, pomocí `AbsoluteLayout.LayoutBounds` vlastnost. Hranice jsou nastavené jako čárkou oddělený seznam hodnot, `X`, `Y`, `Width`, a `Height`v tomto pořadí. Příznaky jsou také uvedené v deklaraci zobrazení v rozložení pomocí `AbsoluteLayout.LayoutFlags` vlastnost. Všimněte si, že je možné kombinovat příznaky v XAML pomocí seznamu čárkami. Vezměte v úvahu v následujícím příkladu:

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

- Popisek ve středu je umístěn pomocí absolutní hodnoty velikost a umístění. Z důvodu, zobrazí se na střed na iPhone 4S a nižší, ale není zarovnaný na větší zařízení.
- Text v dolní části rozložení je umístěn pomocí přímo úměrná velikosti a pozice hodnot. Vždy se zobrazí v dolní části centru rozložení, ale jeho velikost se zvýší o větší velikosti rozložení.
- Tři barevné `BoxView`s jsou umístěny v horní, levý a pravý okraji obrazovky pomocí proporcionální pozici a velikost absolutní.

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

### <a name="proportional-values"></a>Proporcionální hodnoty

Proporcionální hodnoty definovat vztah mezi rozložení a zobrazení. Tento vztah definuje jako podíl odpovídající hodnota rozložení nadřazené umístění nebo hodnota měřítka podřízené zobrazení. Tyto hodnoty jsou vyjádřeny jako `double`s hodnotami mezi 0 a 1.

Proporcionální hodnoty se používají k pozici a velikost zobrazení v rámci rozložení. Tedy Šířka zobrazení nastavena jako část, hodnota výsledná šířky při poměr vynásobené `AbsoluteLayout`na šířku. Například `AbsoluteLayout` šířky `500` a zobrazení nastavení mít přímo úměrná šířku.5, šířku vykreslené zobrazení bude 250 (500 ×.5).

Chcete-li použít přímo úměrná hodnoty, nastavte `LayoutBounds` pomocí (x, y) rozměry a přímo úměrná velikosti, nastavte `LayoutFlags` k `All`.

V XAML:

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

Absolutní hodnoty explicitně definovat, kde zobrazení by měl být umístěn v rámci rozložení. Na rozdíl od přímo úměrná hodnoty jsou schopné umístění a velikosti zobrazení, které se nevejde do hranice rozložení absolutní hodnoty.

Použití absolutní hodnoty pro umístění může být nebezpečné, pokud není známý velikost rozložení. Při použití absolutní umístění, ve středu obrazovky na velikosti elementu může posun v jakékoli velikosti. Je potřeba testovat aplikace pro různé velikosti obrazovky podporovaných zařízení.

Chcete-li použít absolutní rozložení hodnoty, nastavte `LayoutBounds` pomocí (x, y) souřadnic a explicitní velikost nastavte `LayoutFlags` k `None`.

V XAML:

```xaml
<Label Text="I'm centered on iPhone 4 but no other device."
  AbsoluteLayout.LayoutBounds="115,150,100,100"  />
```

V jazyce C#:

```csharp
var label = new Label {Text = "I'm centered on iPhone 4 but no other device."};
AbsoluteLayout.SetLayoutBounds(label, new Rectangle(115,150,100,100));
```

## <a name="exploring-a-complex-layout"></a>Zkoumání složitá rozložení

Každá z rozložení mít silné a slabé stránky pro vytvoření konkrétního rozložení. V celé této sérii článků rozložení byla vytvořena ukázkovou aplikaci s stejné rozložení stránky implementované pomocí tři různá rozložení.

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

Výše uvedený kód za následek následující rozložení:

![](absolute-layout-images/abs.png "Komplexní AbsoluteLayout")

Všimněte si, že `AbsoluteLayout`s jsou vnořené, protože v některých případech vnoření rozložení může být jednodušší, než nabízí ten samý všechny prvky v rámci stejné rozložení.

## <a name="related-links"></a>Související odkazy

- [Vytváření mobilních aplikací s Xamarin.Forms, kapitola 14](https://developer.xamarin.com/r/xamarin-forms/book/chapter14.pdf)
- [AbsoluteLayout](xref:Xamarin.Forms.AbsoluteLayout)
- [Rozložení (ukázka)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/Layout/)
- [Příklad BusinessTumble (ukázka)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/BusinessTumble/)
