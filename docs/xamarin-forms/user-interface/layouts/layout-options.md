---
title: Možnosti rozložení v Xamarin.Forms
description: Každé zobrazení Xamarin.Forms má HorizontalOptions a VerticalOptions vlastnosti typu LayoutOptions. Tento článek vysvětluje, každá hodnota LayoutOptions vliv na zarovnání a rozšíření zobrazení.
ms.prod: xamarin
ms.assetid: 7CAB5631-5153-4DEF-8AD7-C6011CE44307
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 02/10/2017
ms.openlocfilehash: 1ede5f75925a3dafa93062d147fa349ff91f07d2
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/12/2018
ms.locfileid: "38995307"
---
# <a name="layout-options-in-xamarinforms"></a>Možnosti rozložení v Xamarin.Forms

_Každé zobrazení Xamarin.Forms má HorizontalOptions a VerticalOptions vlastnosti typu LayoutOptions. Tento článek vysvětluje, každá hodnota LayoutOptions vliv na zarovnání a rozšíření zobrazení._

## <a name="overview"></a>Přehled

[ `LayoutOptions` ](xref:Xamarin.Forms.LayoutOptions) Struktura zapouzdřuje dvě předvolby rozložení:

- **Zarovnání** – zobrazení upřednostňovaného zarovnání, který udává jeho pozici a velikost v rámci své nadřazené rozložení.
- **Rozšíření** – používá se pouze [ `StackLayout` ](xref:Xamarin.Forms.StackLayout)a znamená, pokud zobrazení by měl používat místo navíc, pokud je k dispozici.

Tyto předvolby rozložení můžete použít u [ `View` ](xref:Xamarin.Forms.View), relativní k nadřazené úloze, tak, že nastavíte [ `HorizontalOptions` ](xref:Xamarin.Forms.View.HorizontalOptions) nebo [ `VerticalOptions` ](xref:Xamarin.Forms.View.VerticalOptions) vlastnost `View` k jednomu z veřejné pole z [ `LayoutOptions` ](xref:Xamarin.Forms.LayoutOptions) struktury. Veřejná pole jsou následující:

- [`Start`](xref:Xamarin.Forms.LayoutOptions.Start)
- [`Center`](xref:Xamarin.Forms.LayoutOptions.Center)
- [`End`](xref:Xamarin.Forms.LayoutOptions.End)
- [`Fill`](xref:Xamarin.Forms.LayoutOptions.Fill)
- [`StartAndExpand`](xref:Xamarin.Forms.LayoutOptions.StartAndExpand)
- [`CenterAndExpand`](xref:Xamarin.Forms.LayoutOptions.CenterAndExpand)
- [`EndAndExpand`](xref:Xamarin.Forms.LayoutOptions.EndAndExpand)
- [`FillAndExpand`](xref:Xamarin.Forms.LayoutOptions.FillAndExpand)

`Start`, `Center`, `End`, A `Fill` pole se používají k definování zarovnání zobrazení v rámci nadřazené rozložení:

- Vodorovné zarovnání [ `Start` ](xref:Xamarin.Forms.LayoutOptions.Start) pozice [ `View` ](xref:Xamarin.Forms.View) na levé straně nadřazené rozložení a svislé zarovnání, umístí `View` v horní části nadřazené rozložení.
- Pro vodorovného a svislého zarovnání [ `Center` ](xref:Xamarin.Forms.LayoutOptions.Center) vodorovně nebo svisle centra [ `View` ](xref:Xamarin.Forms.View).
- Vodorovné zarovnání [ `End` ](xref:Xamarin.Forms.LayoutOptions.End) pozice [ `View` ](xref:Xamarin.Forms.View) na pravé straně nadřazené rozložení a svislé zarovnání, umístí `View` v dolní části nadřazené rozložení.
- Vodorovné zarovnání [ `Fill` ](xref:Xamarin.Forms.LayoutOptions.Fill) zajišťuje, že [ `View` ](xref:Xamarin.Forms.View) vyplní šířku nadřazené rozložení a svislé zarovnání, zajišťuje, že `View` vyplní Výška nadřazené rozložení.

`StartAndExpand`, `CenterAndExpand`, `EndAndExpand`, A `FillAndExpand` hodnoty se používají k definování předvoleb zarovnání a určuje, zda zobrazení bude zabírat více místa, pokud je k dispozici v rámci nadřazené [ `StackLayout` ](xref:Xamarin.Forms.StackLayout).

> [!NOTE]
> Výchozí hodnota pro zobrazení [ `HorizontalOptions` ](xref:Xamarin.Forms.View.HorizontalOptions) a [ `VerticalOptions` ](xref:Xamarin.Forms.View.VerticalOptions) vlastnosti je [ `LayoutOptions.Fill` ](xref:Xamarin.Forms.LayoutOptions.Fill).

<a name="alignment" />

## <a name="alignment"></a>Zarovnání

Zarovnání řídí, jak zobrazení je umístěn v rámci své nadřazené rozložení při rozložení nadřazené obsahuje nevyužité místo (to znamená, nadřazené rozložení je větší než celková velikost všech jeho podřízených prvků).

A [ `StackLayout` ](xref:Xamarin.Forms.StackLayout) pouze respektuje `Start`, `Center`, `End`, a `Fill` [ `LayoutOptions` ](xref:Xamarin.Forms.LayoutOptions) polí na podřízené zobrazení, které jsou v opačném směru Chcete `StackLayout` orientace. Proto podřízené zobrazení v rámci svisle orientovaný `StackLayout` můžete nastavit jejich [ `HorizontalOptions` ](xref:Xamarin.Forms.View.HorizontalOptions) vlastností do jednoho ze `Start`, `Center`, `End`, nebo `Fill` pole. Obdobně podřízené zobrazení v rámci vodorovně orientovaného `StackLayout` můžete nastavit jejich [ `VerticalOptions` ](xref:Xamarin.Forms.View.VerticalOptions) vlastností do jednoho ze `Start`, `Center`, `End`, nebo `Fill` pole.

A [ `StackLayout` ](xref:Xamarin.Forms.StackLayout) nerespektuje `Start`, `Center`, `End`, a `Fill` [ `LayoutOptions` ](xref:Xamarin.Forms.LayoutOptions) polí na podřízené zobrazení, které jsou ve stejném směru jako `StackLayout` orientace. Proto svisle orientovaný `StackLayout` ignoruje `Start`, `Center`, `End`, nebo `Fill` pole v případě, že jsou nastavené na [ `VerticalOptions` ](xref:Xamarin.Forms.View.VerticalOptions) vlastnosti podřízené zobrazení. Podobně, vodorovně orientovaného `StackLayout` ignoruje `Start`, `Center`, `End`, nebo `Fill` pole v případě, že jsou nastavené na [ `HorizontalOptions` ](xref:Xamarin.Forms.View.HorizontalOptions) vlastnosti podřízené zobrazení.

> [!NOTE]
> [`LayoutOptions.Fill`](xref:Xamarin.Forms.LayoutOptions.Fill) obecně přepsání velikost požadavků určeny pomocí [ `HeightRequest` ](xref:Xamarin.Forms.VisualElement.HeightRequest) a [ `WidthRequest` ](xref:Xamarin.Forms.VisualElement.WidthRequest) vlastnosti.

Následující příklad kódu XAML ukazuje svisle orientovaný [ `StackLayout` ](xref:Xamarin.Forms.StackLayout) kde jednotlivých podřízených [ `Label` ](xref:Xamarin.Forms.Label) nastaví jeho [ `HorizontalOptions` ](xref:Xamarin.Forms.View.HorizontalOptions) vlastnost na jedno z polí čtyři zarovnání z [ `LayoutOptions` ](xref:Xamarin.Forms.LayoutOptions) struktury:

```xaml
<StackLayout Margin="0,20,0,0">
  ...
  <Label Text="Start" BackgroundColor="Gray" HorizontalOptions="Start" />
  <Label Text="Center" BackgroundColor="Gray" HorizontalOptions="Center" />
  <Label Text="End" BackgroundColor="Gray" HorizontalOptions="End" />
  <Label Text="Fill" BackgroundColor="Gray" HorizontalOptions="Fill" />
</StackLayout>
```

Ekvivalentní kód jazyka C# je uveden níže:

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

Kód za následek rozložení je znázorněno na následujících snímcích obrazovky:

[![](layout-options-images/alignment.png "Možnosti rozložení zarovnání")](layout-options-images/alignment-large.png#lightbox "možnosti rozložení zarovnání")

<a name="expansion" />

## <a name="expansion"></a>Rozšíření

Rozšíření určuje, zda zobrazení bude zabírat více místa, pokud je k dispozici v rámci [ `StackLayout` ](xref:Xamarin.Forms.StackLayout). Pokud `StackLayout` obsahuje nevyužité místo (to znamená, `StackLayout` je větší než kombinované velikosti všech podřízených), nevyužité místo je sdílet stejnou měrou všechny podřízené zobrazení, které požadují rozšíření tak, že nastavíte jejich [ `HorizontalOptions` ](xref:Xamarin.Forms.View.HorizontalOptions)nebo [ `VerticalOptions` ](xref:Xamarin.Forms.View.VerticalOptions) vlastnosti, které chcete [ `LayoutOptions` ](xref:Xamarin.Forms.LayoutOptions) pole, které používá `AndExpand` příponu. Všimněte si, že veškeré místo v `StackLayout` je používají, možnosti rozšíření nemají žádný vliv.

A [ `StackLayout` ](xref:Xamarin.Forms.StackLayout) můžete rozbalit jenom podřízené zobrazení ve směru orientace. Proto svisle orientovaný `StackLayout` můžete rozbalit podřízené zobrazení, které nastavit jejich [ `VerticalOptions` ](xref:Xamarin.Forms.View.VerticalOptions) vlastností do jednoho ze `StartAndExpand`, `CenterAndExpand`, `EndAndExpand`, nebo `FillAndExpand` polí, pokud `StackLayout` obsahuje nevyužité místo. Podobně, vodorovně orientovaného `StackLayout` můžete rozbalit podřízené zobrazení, které nastavit jejich [ `HorizontalOptions` ](xref:Xamarin.Forms.View.HorizontalOptions) vlastností do jednoho ze `StartAndExpand`, `CenterAndExpand`, `EndAndExpand`, nebo `FillAndExpand` polí, pokud `StackLayout` obsahuje nevyužité místo.

A [ `StackLayout` ](xref:Xamarin.Forms.StackLayout) nelze rozbalit podřízené zobrazení ve směru než jeho orientace. Proto se na svisle orientovaný `StackLayout`a nastavte [ `HorizontalOptions` ](xref:Xamarin.Forms.View.HorizontalOptions) vlastnost u podřízené zobrazení, které [ `StartAndExpand` ](xref:Xamarin.Forms.LayoutOptions.StartAndExpand) má stejný účinek jako nastavení vlastnosti na [ `Start`](xref:Xamarin.Forms.LayoutOptions.Start).

> [!NOTE]
> Všimněte si, že povolení rozšíření nemění velikost zobrazení pokud používá [ `LayoutOptions.FillAndExpand` ](xref:Xamarin.Forms.LayoutOptions.FillAndExpand).

Následující příklad kódu XAML ukazuje svisle orientovaný [ `StackLayout` ](xref:Xamarin.Forms.StackLayout) kde jednotlivých podřízených [ `Label` ](xref:Xamarin.Forms.Label) nastaví jeho [ `VerticalOptions` ](xref:Xamarin.Forms.View.VerticalOptions) vlastnost na jedno z polí čtyři rozšíření z [ `LayoutOptions` ](xref:Xamarin.Forms.LayoutOptions) struktury:

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

Ekvivalentní kód jazyka C# je uveden níže:

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

Kód za následek rozložení je znázorněno na následujících snímcích obrazovky:

[![](layout-options-images/expansion.png "Možnosti rozložení rozšíření")](layout-options-images/expansion-large.png#lightbox "rozšíření možnosti rozložení")

Každý [ `Label` ](xref:Xamarin.Forms.Label) zabírají stejné množství mezeru mezi kulaté [ `StackLayout` ](xref:Xamarin.Forms.StackLayout). Nicméně pouze poslední `Label`, který nastaví jeho [ `VerticalOptions` ](xref:Xamarin.Forms.View.VerticalOptions) vlastnost [ `FillAndExpand` ](xref:Xamarin.Forms.LayoutOptions.FillAndExpand) má jinou velikost. Kromě toho každý `Label` jsou oddělené oddělovačem malé red [ `BoxView` ](xref:Xamarin.Forms.BoxView), což umožňuje místo `Label` zabírá snadné prohlížení.

## <a name="summary"></a>Souhrn

Tento článek vysvětlil efekt, které každý [ `LayoutOptions` ](xref:Xamarin.Forms.LayoutOptions) struktura hodnota má na zarovnání a rozšíření zobrazení, relativně k nadřazenému. `Start`, `Center`, `End`, A `Fill` pole se používají k definování zarovnání zobrazení v rámci nadřazené rozložení a `StartAndExpand`, `CenterAndExpand`, `EndAndExpand`, a `FillAndExpand` pole se používají k definování Předvolby zarovnání a na zjištění, zda zobrazení bude zabírat více místa, pokud je k dispozici v rámci [ `StackLayout` ](xref:Xamarin.Forms.StackLayout).



## <a name="related-links"></a>Související odkazy

- [LayoutOptions (ukázka)](https://developer.xamarin.com/samples/xamarin-forms/userinterface/layoutoptions/)
- [LayoutOptions](xref:Xamarin.Forms.LayoutOptions)
