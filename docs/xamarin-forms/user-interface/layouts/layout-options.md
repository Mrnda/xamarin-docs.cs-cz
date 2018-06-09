---
title: Možnosti rozložení v Xamarin.Forms
description: Každé zobrazení Xamarin.Forms má HorizontalOptions a VerticalOptions vlastnosti typu LayoutOptions. Tento článek vysvětluje o tom, že každá hodnota LayoutOptions má na zarovnání a rozšíření zobrazení.
ms.prod: xamarin
ms.assetid: 7CAB5631-5153-4DEF-8AD7-C6011CE44307
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 02/10/2017
ms.openlocfilehash: dc15c05bf3633ef2ae5f71754290a7bd768dc836
ms.sourcegitcommit: 66682dd8e93c0e4f5dee69f32b5fc5a96443e307
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 06/08/2018
ms.locfileid: "35245687"
---
# <a name="layout-options-in-xamarinforms"></a>Možnosti rozložení v Xamarin.Forms

_Každé zobrazení Xamarin.Forms má HorizontalOptions a VerticalOptions vlastnosti typu LayoutOptions. Tento článek vysvětluje o tom, že každá hodnota LayoutOptions má na zarovnání a rozšíření zobrazení._

## <a name="overview"></a>Přehled

[ `LayoutOptions` ](https://developer.xamarin.com/api/type/Xamarin.Forms.LayoutOptions/) Struktura zapouzdří dvě předvolby rozložení:

- **Zarovnání** – zobrazení preferovaný zarovnání, což určuje jeho pozice a velikosti v rámci své nadřazené rozložení.
- **Rozšíření** – používá se pouze [ `StackLayout` ](https://developer.xamarin.com/api/type/Xamarin.Forms.StackLayout/)a určuje, pokud zobrazení by měl použít místo navíc, pokud je k dispozici.

Lze použít tyto předvolby rozložení [ `View` ](https://developer.xamarin.com/api/type/Xamarin.Forms.View/), relativně k jeho nadřazeným prvkem, a to nastavením [ `HorizontalOptions` ](https://developer.xamarin.com/api/property/Xamarin.Forms.View.HorizontalOptions/) nebo [ `VerticalOptions` ](https://developer.xamarin.com/api/property/Xamarin.Forms.View.VerticalOptions/) vlastnost `View` na jednu z veřejná pole z [ `LayoutOptions` ](https://developer.xamarin.com/api/type/Xamarin.Forms.LayoutOptions/) struktury. Veřejná pole jsou následující:

- [`Start`](https://developer.xamarin.com/api/field/Xamarin.Forms.LayoutOptions.Start/)
- [`Center`](https://developer.xamarin.com/api/field/Xamarin.Forms.LayoutOptions.Center/)
- [`End`](https://developer.xamarin.com/api/field/Xamarin.Forms.LayoutOptions.End/)
- [`Fill`](https://developer.xamarin.com/api/field/Xamarin.Forms.LayoutOptions.Fill/)
- [`StartAndExpand`](https://developer.xamarin.com/api/field/Xamarin.Forms.LayoutOptions.StartAndExpand/)
- [`CenterAndExpand`](https://developer.xamarin.com/api/field/Xamarin.Forms.LayoutOptions.CenterAndExpand/)
- [`EndAndExpand`](https://developer.xamarin.com/api/field/Xamarin.Forms.LayoutOptions.EndAndExpand/)
- [`FillAndExpand`](https://developer.xamarin.com/api/field/Xamarin.Forms.LayoutOptions.FillAndExpand/)

`Start`, `Center`, `End`, A `Fill` pole slouží k určení zarovnání zobrazení v rámci nadřazené rozložení:

- Pro vodorovné zarovnání [ `Start` ](https://developer.xamarin.com/api/field/Xamarin.Forms.LayoutOptions.Start/) pozice [ `View` ](https://developer.xamarin.com/api/type/Xamarin.Forms.View/) na na levé straně na nadřazené rozložení a pro svislé zarovnání, umisťuje `View` v horní části nadřazené rozložení.
- Pro vodorovné nebo svislé zarovnání [ `Center` ](https://developer.xamarin.com/api/field/Xamarin.Forms.LayoutOptions.Center/) vodorovně nebo svisle soustředí [ `View` ](https://developer.xamarin.com/api/type/Xamarin.Forms.View/).
- Pro vodorovné zarovnání [ `End` ](https://developer.xamarin.com/api/field/Xamarin.Forms.LayoutOptions.End/) pozice [ `View` ](https://developer.xamarin.com/api/type/Xamarin.Forms.View/) na pravé straně nadřazené rozložení a pro svislé zarovnání, umisťuje `View` v dolní části nadřazené rozložení.
- Pro vodorovné zarovnání [ `Fill` ](https://developer.xamarin.com/api/field/Xamarin.Forms.LayoutOptions.Fill/) zajistí, že [ `View` ](https://developer.xamarin.com/api/type/Xamarin.Forms.View/) vyplní celé šířce nadřazeného rozložení a pro svislé zarovnání, zajišťuje, aby `View` vyplní celé Výška nadřazené rozložení.

`StartAndExpand`, `CenterAndExpand`, `EndAndExpand`, A `FillAndExpand` hodnoty slouží k určení zarovnání předvoleb, a zda zobrazení bude zabírat více místa, pokud je k dispozici v rámci nadřazené [ `StackLayout` ](https://developer.xamarin.com/api/type/Xamarin.Forms.StackLayout/).

> [!NOTE]
> Výchozí hodnota pro zobrazení [ `HorizontalOptions` ](https://developer.xamarin.com/api/property/Xamarin.Forms.View.HorizontalOptions/) a [ `VerticalOptions` ](https://developer.xamarin.com/api/property/Xamarin.Forms.View.VerticalOptions/) vlastnosti je [ `LayoutOptions.Fill` ](https://developer.xamarin.com/api/field/Xamarin.Forms.LayoutOptions.Fill/).

<a name="alignment" />

## <a name="alignment"></a>Zarovnání

Zarovnání řídí, jak zobrazení je umístěn v rámci své nadřazené rozložení při rozložení nadřazené obsahuje nevyužívaného místa (nadřazené rozložení je větší než celková velikost všech jeho podřízených položek).

A [ `StackLayout` ](https://developer.xamarin.com/api/type/Xamarin.Forms.StackLayout/) pouze respektuje `Start`, `Center`, `End`, a `Fill` [ `LayoutOptions` ](https://developer.xamarin.com/api/type/Xamarin.Forms.LayoutOptions/) pole na podřízené zobrazení, které jsou v opačném směru na `StackLayout` orientace. Proto podřízené zobrazení v rámci svisle orientované `StackLayout` můžete nastavit jejich [ `HorizontalOptions` ](https://developer.xamarin.com/api/property/Xamarin.Forms.View.HorizontalOptions/) vlastností do jednoho ze `Start`, `Center`, `End`, nebo `Fill` pole. Podobně podřízené zobrazení v rámci vodorovně orientované `StackLayout` můžete nastavit jejich [ `VerticalOptions` ](https://developer.xamarin.com/api/property/Xamarin.Forms.View.VerticalOptions/) vlastností do jednoho ze `Start`, `Center`, `End`, nebo `Fill` pole.

A [ `StackLayout` ](https://developer.xamarin.com/api/type/Xamarin.Forms.StackLayout/) nerespektuje `Start`, `Center`, `End`, a `Fill` [ `LayoutOptions` ](https://developer.xamarin.com/api/type/Xamarin.Forms.LayoutOptions/) pole na podřízené zobrazení, které jsou ve stejném směru jako `StackLayout` orientace. Proto svisle orientované `StackLayout` ignoruje `Start`, `Center`, `End`, nebo `Fill` pole, pokud jsou nastavená na [ `VerticalOptions` ](https://developer.xamarin.com/api/property/Xamarin.Forms.View.VerticalOptions/) vlastnosti podřízené zobrazení. Podobně, vodorovně orientované `StackLayout` ignoruje `Start`, `Center`, `End`, nebo `Fill` pole, pokud jsou nastavená na [ `HorizontalOptions` ](https://developer.xamarin.com/api/property/Xamarin.Forms.View.HorizontalOptions/) vlastnosti podřízené zobrazení.

> [!NOTE]
> [`LayoutOptions.Fill`](https://developer.xamarin.com/api/field/Xamarin.Forms.LayoutOptions.Fill/) obecně přepsání velikost zadán pomocí požadavků [ `HeightRequest` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.HeightRequest/) a [ `WidthRequest` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.WidthRequest/) vlastnosti.

Následující příklad kódu XAML ukazuje svisle orientované [ `StackLayout` ](https://developer.xamarin.com/api/type/Xamarin.Forms.StackLayout/) kde jednotlivých podřízených [ `Label` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Label/) nastaví jeho [ `HorizontalOptions` ](https://developer.xamarin.com/api/property/Xamarin.Forms.View.HorizontalOptions/) vlastnost na jedno z polí čtyři zarovnání z [ `LayoutOptions` ](https://developer.xamarin.com/api/type/Xamarin.Forms.LayoutOptions/) strukturu:

```xaml
<StackLayout Margin="0,20,0,0">
  ...
  <Label Text="Start" BackgroundColor="Gray" HorizontalOptions="Start" />
  <Label Text="Center" BackgroundColor="Gray" HorizontalOptions="Center" />
  <Label Text="End" BackgroundColor="Gray" HorizontalOptions="End" />
  <Label Text="Fill" BackgroundColor="Gray" HorizontalOptions="Fill" />
</StackLayout>
```

Ekvivalentní kódu C# je zobrazena níže:

```csharp
Content = new StackLayout
{
  Margin = new Thickness(0, 20, 0, 0),
  Children = {
    ...
    new Label { Text = "Start", BackgroundColor = Color.Gray, HorizontalOptions = LayoutOptions.Start },
    new Label { Text = "Center", BackgroundColor = Color.Gray, HorizontalOptions = LayoutOptions.Center },
    new Label { Text = "End", BackgroundColor = Color.Gray, HorizontalOptions = LayoutOptions.End },
    new Label { Text = "Fill", BackgroundColor = Color.Gray, HorizontalOptions = LayoutOptions.Fill }
  }
};
```

Kód výsledkem rozložení vidět na následujících snímcích obrazovky:

[![](layout-options-images/alignment.png "Možnosti zarovnání rozložení")](layout-options-images/alignment-large.png#lightbox "možnosti zarovnání rozložení")

<a name="expansion" />

## <a name="expansion"></a>Rozšíření

Rozšíření řídí, zda zobrazení bude zabírat více místa, pokud je k dispozici v rámci [ `StackLayout` ](https://developer.xamarin.com/api/type/Xamarin.Forms.StackLayout/). Pokud `StackLayout` obsahuje nevyužívaného místa (tedy `StackLayout` je větší než celková velikost všech jeho podřízených položek), nevyužité místo sdílí stejnou měrou všechny podřízené zobrazení, která požádat o rozšíření nastavením jejich [ `HorizontalOptions` ](https://developer.xamarin.com/api/property/Xamarin.Forms.View.HorizontalOptions/)nebo [ `VerticalOptions` ](https://developer.xamarin.com/api/property/Xamarin.Forms.View.VerticalOptions/) vlastností [ `LayoutOptions` ](https://developer.xamarin.com/api/type/Xamarin.Forms.LayoutOptions/) pole, které používá `AndExpand` příponu. Všimněte si, že když veškeré místo v `StackLayout` se používá, možnosti rozšíření nemají žádný vliv.

A [ `StackLayout` ](https://developer.xamarin.com/api/type/Xamarin.Forms.StackLayout/) můžete rozšířit pouze podřízené zobrazení ve směru orientace. Proto svisle orientované `StackLayout` můžete rozbalit podřízené zobrazení, které nastavit jejich [ `VerticalOptions` ](https://developer.xamarin.com/api/property/Xamarin.Forms.View.VerticalOptions/) vlastností do jednoho ze `StartAndExpand`, `CenterAndExpand`, `EndAndExpand`, nebo `FillAndExpand` pole, pokud `StackLayout` obsahuje nevyužívaného místa. Podobně, vodorovně orientované `StackLayout` můžete rozbalit podřízené zobrazení, které nastavit jejich [ `HorizontalOptions` ](https://developer.xamarin.com/api/property/Xamarin.Forms.View.HorizontalOptions/) vlastností do jednoho ze `StartAndExpand`, `CenterAndExpand`, `EndAndExpand`, nebo `FillAndExpand` pole, pokud `StackLayout` obsahuje nevyužívaného místa.

A [ `StackLayout` ](https://developer.xamarin.com/api/type/Xamarin.Forms.StackLayout/) nelze rozbalit podřízené zobrazení, v opačném než její orientace. Proto na svisle orientované `StackLayout`, nastavení [ `HorizontalOptions` ](https://developer.xamarin.com/api/property/Xamarin.Forms.View.HorizontalOptions/) vlastnost podřízené zobrazení [ `StartAndExpand` ](https://developer.xamarin.com/api/field/Xamarin.Forms.LayoutOptions.StartAndExpand/) má stejný účinek jako nastavení vlastnosti na [ `Start`](https://developer.xamarin.com/api/field/Xamarin.Forms.LayoutOptions.Start/).

> [!NOTE]
> Všimněte si, že povolení rozšíření nemění velikost zobrazení pokud používá [ `LayoutOptions.FillAndExpand` ](https://developer.xamarin.com/api/field/Xamarin.Forms.LayoutOptions.FillAndExpand/).

Následující příklad kódu XAML ukazuje svisle orientované [ `StackLayout` ](https://developer.xamarin.com/api/type/Xamarin.Forms.StackLayout/) kde jednotlivých podřízených [ `Label` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Label/) nastaví jeho [ `VerticalOptions` ](https://developer.xamarin.com/api/property/Xamarin.Forms.View.VerticalOptions/) vlastnost na jedno z polí čtyři rozšíření z [ `LayoutOptions` ](https://developer.xamarin.com/api/type/Xamarin.Forms.LayoutOptions/) strukturu:

```xaml
<StackLayout Margin="0,20,0,0">
  ...
  <BoxView BackgroundColor="Red" HeightRequest="1" />
  <Label Text="Start" BackgroundColor="Gray" VerticalOptions="StartAndExpand" />
  <BoxView BackgroundColor="Red" HeightRequest="1" />
  <Label Text="Center" BackgroundColor="Gray" VerticalOptions="CenterAndExpand" />
  <BoxView BackgroundColor="Red" HeightRequest="1" />
  <Label Text="End" BackgroundColor="Gray" VerticalOptions="EndAndExpand" />
  <BoxView BackgroundColor="Red" HeightRequest="1" />
  <Label Text="Fill" BackgroundColor="Gray" VerticalOptions="FillAndExpand" />
  <BoxView BackgroundColor="Red" HeightRequest="1" />
</StackLayout>
```

Ekvivalentní kódu C# je zobrazena níže:

```csharp
Content = new StackLayout
{
  Margin = new Thickness(0, 20, 0, 0),
  Children = {
    ...
    new BoxView { BackgroundColor = Color.Red, HeightRequest = 1 },
    new Label { Text = "StartAndExpand", BackgroundColor = Color.Gray, VerticalOptions = LayoutOptions.StartAndExpand },
    new BoxView { BackgroundColor = Color.Red, HeightRequest = 1 },
    new Label { Text = "CenterAndExpand", BackgroundColor = Color.Gray, VerticalOptions = LayoutOptions.CenterAndExpand },
    new BoxView { BackgroundColor = Color.Red, HeightRequest = 1 },
    new Label { Text = "EndAndExpand", BackgroundColor = Color.Gray, VerticalOptions = LayoutOptions.EndAndExpand },
    new BoxView { BackgroundColor = Color.Red, HeightRequest = 1 },
    new Label { Text = "FillAndExpand", BackgroundColor = Color.Gray, VerticalOptions = LayoutOptions.FillAndExpand },
    new BoxView { BackgroundColor = Color.Red, HeightRequest = 1 }
  }
};
```

Kód výsledkem rozložení vidět na následujících snímcích obrazovky:

[![](layout-options-images/expansion.png "Možnosti rozložení rozšíření")](layout-options-images/expansion-large.png#lightbox "možnosti rozšíření rozložení")

Každý [ `Label` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Label/) zabírá stejné množství místa v rámci [ `StackLayout` ](https://developer.xamarin.com/api/type/Xamarin.Forms.StackLayout/). Však pouze konečné `Label`, která nastaví jeho [ `VerticalOptions` ](https://developer.xamarin.com/api/property/Xamarin.Forms.View.VerticalOptions/) vlastnost [ `FillAndExpand` ](https://developer.xamarin.com/api/field/Xamarin.Forms.LayoutOptions.FillAndExpand/) má různou velikost. Kromě toho každý `Label` jsou oddělené oddělovačem malé red [ `BoxView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.BoxView/), což umožňuje prostor `Label` zabírá lze snadno zobrazit.

## <a name="summary"></a>Souhrn

Tento článek vysvětlené účinek že každý [ `LayoutOptions` ](https://developer.xamarin.com/api/type/Xamarin.Forms.LayoutOptions/) hodnoty strukturu má na zarovnání a rozšíření zobrazení, relativně k nadřazenému. `Start`, `Center`, `End`, A `Fill` pole slouží k určení zarovnání zobrazení v rámci nadřazené rozložení a `StartAndExpand`, `CenterAndExpand`, `EndAndExpand`, a `FillAndExpand` pole slouží k určení Zarovnání předvoleb a určení, zda zobrazení bude zabírat více místa, pokud je k dispozici v rámci [ `StackLayout` ](https://developer.xamarin.com/api/type/Xamarin.Forms.StackLayout/).



## <a name="related-links"></a>Související odkazy

- [LayoutOptions (ukázka)](https://developer.xamarin.com/samples/xamarin-forms/userinterface/layoutoptions/)
- [LayoutOptions](https://developer.xamarin.com/api/type/Xamarin.Forms.LayoutOptions/)
