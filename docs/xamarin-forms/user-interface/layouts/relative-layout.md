---
title: RelativeLayout
description: "RelativeLayout použijte k vytvoření uživatelská rozhraní, které škálovat podle jakékoli velikosti obrazovky."
ms.topic: article
ms.prod: xamarin
ms.assetid: 2530BCB8-01B8-4C4F-BF14-CA53659F1B5A
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 11/25/2015
ms.openlocfilehash: 3d915191e24b5238d5165237f6ede74635b31e08
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 03/09/2018
---
# <a name="relativelayout"></a>RelativeLayout

`RelativeLayout` slouží k pozice a velikosti zobrazení relativně k vlastnosti zobrazení rozložení nebo na stejné úrovni. Na rozdíl od `AbsoluteLayout`, `RelativeLayout` nemá koncept přesunutí ukotvení a nemá zařízení pro umístění prvků relativně k dolní a pravé hrany rozložení. `RelativeLayout` podporuje umísťovací elementy mimo svůj vlastní rozsah.

[![](relative-layout-images/layouts-sml.png "Rozložení Xamarin.Forms")](relative-layout-images/layouts.png#lightbox "Xamarin.Forms rozložení")

## <a name="purpose"></a>Účel

`RelativeLayout` slouží k umístění zobrazení na obrazovce relativně k celkové rozložení nebo dvěma dalšími zobrazeními.

![](relative-layout-images/flag.png "Zkoumání RelativeLayout")

## <a name="usage"></a>Použití

### <a name="understanding-constraints"></a>Pochopení omezení

Umístění a velikost zobrazení v rámci `RelativeLayout` se provádí s omezeními. Ve výrazu omezení může zahrnovat následující informace:

- **Typ** &ndash; jestli omezení je relativní vzhledem k nadřazené nebo do jiného zobrazení.
- **Vlastnost** &ndash; vlastnosti, které použít jako základ pro dané omezení.
- **Faktor** &ndash; faktor, který chcete použít pro hodnotu vlastnosti.
- **Konstantní** &ndash; hodnota, která má použít jako posun hodnoty.
- **Vlastnost ElementName** &ndash; název zobrazení, které je vzhledem k omezení.

V jazyce XAML, omezení jsou vyjádřené jako `ConstraintExpression`s. Podívejte se na následující příklad:

```xaml
<BoxView Color="Green" WidthRequest="50" HeightRequest="50"
    RelativeLayout.XConstraint =
      "{ConstraintExpression Type=RelativeToParent,
                             Property=Width,
                             Factor=0.5,
                             Constant=-100}"
    RelativeLayout.YConstraint =
      "{ConstraintExpression Type=RelativeToParent,
                             Property=Height,
                             Factor=0.5,
                             Constant=-100}" />
```

V jazyce C# omezení jsou vyjádřeny trochu jinak, pomocí funkce spíše než výrazy v zobrazení. Omezení jsou zadané jako argumenty, které mají na rozložení `Add` metoda:

```csharp
layout.Children.Add(box, Constraint.RelativeToParent((parent) =>
    {
      return (.5 * parent.Width) - 100;
    }),
    Constraint.RelativeToParent((parent) =>
    {
        return (.5 * parent.Height) - 100;
    }),
    Constraint.Constant(50), Constraint.Constant(50));
```

Všimněte si následujících charakteristik ve výše uvedené rozložení:

- `x` a `y` omezení nejsou zadány s vlastní omezení.
- V jazyce C# relativní omezení jsou definovány jako funkce. Koncepty, jako `Factor` nejsou existuje, ale může být implementováno ručně.
- Do pole `x` souřadnice je definován jako poloviční šířky nadřazené -100.
- Do pole `y` souřadnice je definován jako polovině výšky nadřazeného objektu, -100.

> [!NOTE]
> Kvůli způsobu, kterým jsou definovány omezení je možné, aby složitější rozložení v jazyce C# než lze určit XAML.

Obě výše uvedených příkladech definují omezení jako `RelativeToParent` &ndash; tedy jejich hodnoty jsou relativní vzhledem k nadřazeného elementu. Je také možné definovat omezení jako relativně k jiné zobrazení. To umožňuje intuitivnější (pro vývojáře) rozložení a provádět záměr kódu rozložení snadněji zřejmá.

Vezměte v úvahu rozložení, kde jeden prvek musí být nižší než jiné 20 pixelů. Pokud oba elementy jsou definovány s konstantní hodnoty, může mít nižší jeho `Y` omezení definovaný jako konstanta, která je větší než 20 pixelů `Y` omezení vyšší elementu. Tento přístup spadá krátký, pokud element vyšší je umístěna pomocí poměru, tak, aby velikost pixelů není znám. V takovém případě je chovaly element na základě pozice jiný element robustnější:

```xaml
<RelativeLayout>
    <BoxView Color="Red" x:Name="redBox"
        RelativeLayout.YConstraint="{ConstraintExpression Type=RelativeToParent,
            Property=Height,Factor=.15,Constant=0}"
        RelativeLayout.WidthConstraint="{ConstraintExpression
            Type=RelativeToParent,Property=Width,Factor=1,Constant=0}"
        RelativeLayout.HeightConstraint="{ConstraintExpression
            Type=RelativeToParent,Property=Height,Factor=.8,Constant=0}" />
    <BoxView Color="Blue"
        RelativeLayout.YConstraint="{ConstraintExpression Type=RelativeToView,
            ElementName=redBox,Property=Y,Factor=1,Constant=20}"
        RelativeLayout.XConstraint="{ConstraintExpression Type=RelativeToView,
            ElementName=redBox,Property=X,Factor=1,Constant=20}"
        RelativeLayout.WidthConstraint="{ConstraintExpression
            Type=RelativeToParent,Property=Width,Factor=.5,Constant=0}"
        RelativeLayout.HeightConstraint="{ConstraintExpression
            Type=RelativeToParent,Property=Height,Factor=.5,Constant=0}" />
</RelativeLayout>
```

K provedení se stejné rozvržení v jazyce C#:

```csharp
layout.Children.Add (redBox, Constraint.RelativeToParent ((parent) => {
        return parent.X;
    }), Constraint.RelativeToParent ((parent) => {
        return parent.Y * .15;
    }), Constraint.RelativeToParent((parent) => {
        return parent.Width;
    }), Constraint.RelativeToParent((parent) => {
        return parent.Height * .8;
    }));
layout.Children.Add (blueBox, Constraint.RelativeToView (redBox, (Parent, sibling) => {
        return sibling.X + 20;
    }), Constraint.RelativeToView (blueBox, (parent, sibling) => {
        return sibling.Y + 20;
    }), Constraint.RelativeToParent((parent) => {
        return parent.Width * .5;
    }), Constraint.RelativeToParent((parent) => {
        return parent.Height * .5;
    }));
```

To vytváří následující výstupní s pozice pole blue určit _relativní_ na pozici červeným rámečkem:

![](relative-layout-images/red-blue-box.png "RelativeLayout s BoxViews červená a modrá")

### <a name="sizing"></a>Nastavení velikosti

Zobrazení nastíněny `RelativeLayout` k dispozici dvě možnosti pro zadání jejich velikost:

- `HeightRequest & WidthRequest`
- `RelativeLayout.WidthConstraint` & `RelativeLayout.HeightConstraint`

`HeightRequest` a `WidthRequest` zadejte určený výška a Šířka zobrazení, ale může být přepsána rozložení, podle potřeby. `WidthConstraint` a `HeightConstraint` podporují nastavení výška a šířka jako hodnotu vzhledem k vlastnosti pro rozložení nebo jiné zobrazení, nebo jako konstantní hodnotu.

## <a name="exploring-a-complex-layout"></a>Zkoumání komplexní rozložení
Každé rozložení mít silné a slabé stránky pro vytvoření konkrétní rozložení. Ukázkovou aplikaci byl vytvořen v rámci této série rozložení články s stejné rozložení stránky implementovaná pomocí tří různých rozložení.

Vezměte v úvahu následující XAML:

```xaml
<?xml version="1.0" encoding="UTF-8"?>
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
x:Class="TheBusinessTumble.RelativeLayoutPage"
BackgroundColor="Maroon"
Title="RelativeLayout">
    <ContentPage.Content>
    <ScrollView>
      <RelativeLayout>
        <BoxView Color="Gray" HeightRequest="100"
            RelativeLayout.WidthConstraint="{ConstraintExpression Type=RelativeToParent, Property=Width, Factor=1}" />
        <Button BorderRadius="35" x:Name="imageCircleBack"
            BackgroundColor="Maroon" HeightRequest="70" WidthRequest="70" RelativeLayout.XConstraint="{ConstraintExpression Type=RelativeToParent,Property=Width, Factor=.5, Constant = -35}" RelativeLayout.YConstraint="{ConstraintExpression Type=RelativeToParent, Factor=0, Property=Y, Constant=70}" />
        <Button BorderRadius="30" BackgroundColor="Red" HeightRequest="60"
            WidthRequest="60" RelativeLayout.XConstraint="{ConstraintExpression Type=RelativeToView, ElementName=imageCircleBack, Property=X, Factor=1,Constant=5}" RelativeLayout.YConstraint="{ConstraintExpression Type=RelativeToParent, Factor=0, Property=Y, Constant=75}" />
        <Label Text="User Name" FontAttributes="Bold" FontSize="26"
            HorizontalTextAlignment="Center" RelativeLayout.YConstraint="{ConstraintExpression Type=RelativeToParent, Property=Y, Factor=0, Constant=140}" RelativeLayout.WidthConstraint="{ConstraintExpression Type=RelativeToParent, Property=Width, Factor=1}" />
        <Entry Text="Bio + Hashtags" TextColor="White" BackgroundColor="Maroon"
            RelativeLayout.YConstraint="{ConstraintExpression Type=RelativeToParent, Property=Y, Factor=0, Constant=180}" RelativeLayout.WidthConstraint="{ConstraintExpression Type=RelativeToParent, Property=Width, Factor=1}" />
        <RelativeLayout BackgroundColor="White" RelativeLayout.YConstraint="
            {ConstraintExpression Type=RelativeToParent, Property=Y, Factor=0, Constant=220}" HeightRequest="60" RelativeLayout.WidthConstraint="{ConstraintExpression Type=RelativeToParent, Property=Width, Factor=1}" >
            <BoxView BackgroundColor="Black" WidthRequest="50"
                HeightRequest="50" RelativeLayout.YConstraint="{ConstraintExpression Type=RelativeToParent, Property=Y, Factor=0, Constant=5}" RelativeLayout.XConstraint="{ConstraintExpression Type=RelativeToParent, Property=X, Factor=0, Constant=5}" />
            <BoxView BackgroundColor="Maroon" WidthRequest="50"
                HeightRequest="50" RelativeLayout.YConstraint="{ConstraintExpression Type=RelativeToParent, Property=Y, Factor=0, Constant=5}" RelativeLayout.XConstraint="{ConstraintExpression Type=RelativeToParent, Property=Width, Factor=0.5, Constant=}" />
            <Label FontSize="14" TextColor="Black" Text="Accent Color"
                RelativeLayout.YConstraint="{ConstraintExpression Type=RelativeToParent, Property=Y, Factor=0, Constant=20}" RelativeLayout.XConstraint="{ConstraintExpression Type=RelativeToParent, Property=X, Factor=0, Constant=60}" />
            <Label FontSize="14" TextColor="Black" Text="Primary Color"
                RelativeLayout.YConstraint="{ConstraintExpression Type=RelativeToParent, Property=Y, Factor=0, Constant=20}" RelativeLayout.XConstraint="{ConstraintExpression Type=RelativeToParent, Property=Width, Factor=0.5, Constant=55}" />
        </RelativeLayout>
        <RelativeLayout Padding="5,0,0,0">
          <Label FontSize="14" Text="Age:" TextColor="White"
            RelativeLayout.YConstraint="{ConstraintExpression Type=RelativeToParent, Property=Height, Factor=0,Constant=305}"
            RelativeLayout.XConstraint="{ConstraintExpression Type=RelativeToParent, Property=Width, Factor=0, Constant=10}"
            RelativeLayout.WidthConstraint="{ConstraintExpression Type=RelativeToParent, Property=Width,Factor=.25,Constant=0}"
            RelativeLayout.HeightConstraint="{ConstraintExpression Type=RelativeToParent, Property=Height, Factor=0,Constant=50}" />
          <Entry Text="35" TextColor="White" BackgroundColor="Maroon"
            RelativeLayout.YConstraint="{ConstraintExpression Type=RelativeToParent, Property=Height, Factor=0,Constant=280}"
            RelativeLayout.XConstraint="{ConstraintExpression Type=RelativeToParent, Property=Width, Factor=0.3, Constant=0}"
            RelativeLayout.WidthConstraint="{ConstraintExpression Type=RelativeToParent, Property=Width,Factor=0.75,Constant=0}"
            RelativeLayout.HeightConstraint="{ConstraintExpression Type=RelativeToParent, Property=Height, Factor=0,Constant=50}" />
        </RelativeLayout>
        <RelativeLayout  Padding="5,0,0,0">
          <Label FontSize="14" Text="Interests:" TextColor="White"
            RelativeLayout.YConstraint="{ConstraintExpression Type=RelativeToParent, Property=Height, Factor=0,Constant=345}"
            RelativeLayout.XConstraint="{ConstraintExpression Type=RelativeToParent, Property=Width, Factor=0, Constant=10}"
            RelativeLayout.WidthConstraint="{ConstraintExpression Type=RelativeToParent, Property=Width,Factor=.25,Constant=0}"
            RelativeLayout.HeightConstraint="{ConstraintExpression Type=RelativeToParent, Property=Height, Factor=0,Constant=50}" />
          <Entry Text="Xamarin.Forms" TextColor="White" BackgroundColor="Maroon"
            RelativeLayout.YConstraint="{ConstraintExpression Type=RelativeToParent, Property=Height, Factor=0,Constant=320}"
            RelativeLayout.XConstraint="{ConstraintExpression Type=RelativeToParent, Property=Width, Factor=0.3, Constant=0}"
            RelativeLayout.WidthConstraint="{ConstraintExpression Type=RelativeToParent, Property=Width,Factor=0.75,Constant=0}"
            RelativeLayout.HeightConstraint="{ConstraintExpression Type=RelativeToParent, Property=Height, Factor=0,Constant=50}" />
        </RelativeLayout>
        <RelativeLayout  Padding="5,0,0,0">
          <Label FontSize="14" Text="Ask me about:" TextColor="White"
            LineBreakMode="WordWrap"
            RelativeLayout.YConstraint="{ConstraintExpression Type=RelativeToParent, Property=Height, Factor=0,Constant=395}"
            RelativeLayout.XConstraint="{ConstraintExpression Type=RelativeToParent, Property=Width, Factor=0, Constant=10}"
            RelativeLayout.WidthConstraint="{ConstraintExpression Type=RelativeToParent, Property=Width,Factor=.25,Constant=0}"
            RelativeLayout.HeightConstraint="{ConstraintExpression Type=RelativeToParent, Property=Height, Factor=0,Constant=50}" />
          <Entry Text="Xamarin, C#, .NET, Mono" TextColor="White"
            BackgroundColor="Maroon"
            RelativeLayout.YConstraint="{ConstraintExpression Type=RelativeToParent, Property=Height, Factor=0,Constant=370}"
            RelativeLayout.XConstraint="{ConstraintExpression Type=RelativeToParent, Property=Width, Factor=0.3, Constant=0}"
            RelativeLayout.WidthConstraint="{ConstraintExpression Type=RelativeToParent, Property=Width,Factor=0.75,Constant=0}"
            RelativeLayout.HeightConstraint="{ConstraintExpression Type=RelativeToParent, Property=Height, Factor=0,Constant=50}" />
        </RelativeLayout>
      </RelativeLayout>
    </ScrollView>
  </ContentPage.Content>
</ContentPage>
```

Ve výše uvedeném kódu má za následek následující rozložení:

![](relative-layout-images/relative.png "Komplexní RelativeLayout")

Všimněte si, že kvůli rozdílu ve způsobu vykreslení tlačítka ve Windows Phone, některé kroužky byly nahrazeny boxviews na snímku obrazovky Windows Phone.

Všimněte si, že `RelativeLayouts`s jsou vnořené, protože v některých případech vnoření rozložení může být snazší než nabízí všechny elementy v rámci stejné rozvržení. Také Všimněte si, že některé prvky jsou `RelativeToView`, protože umožňuje jednodušší a intuitivnější rozložení při vztahy mezi zobrazení Průvodce umístění.


## <a name="related-links"></a>Související odkazy

- [Rozložení (ukázka)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/Layout/)
- [Příklad BusinessTumble (ukázka)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/BusinessTumble/)
