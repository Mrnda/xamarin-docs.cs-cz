---
title: "Okraj a odsazení"
description: "Okraj a odsazení vlastnosti řízení rozložení chování při vykreslení elementu v uživatelském rozhraní. Tento článek ukazuje rozdíl mezi dvě vlastnosti a postupu při jejich nastavení."
ms.topic: article
ms.prod: xamarin
ms.assetid: BEB096BB-51DF-410F-B0F1-D235287B0F4A
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 04/27/2016
ms.openlocfilehash: 7bab512ef11f8e0f553a00f0240d82f860fe2676
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 02/27/2018
---
# <a name="margin-and-padding"></a>Okraj a odsazení

_Okraj a odsazení vlastnosti řízení rozložení chování při vykreslení elementu v uživatelském rozhraní. Tento článek ukazuje rozdíl mezi dvě vlastnosti a postupu při jejich nastavení._

## <a name="overview"></a>Přehled

Okraj a odsazení jsou související rozložení koncepty:

- [ `Margin` ](https://developer.xamarin.com/api/property/Xamarin.Forms.View.Margin/) Vlastnost představuje vzdálenost mezi prvek a jeho přiléhající prvky a slouží k řízení umístění prvku vykreslování a vykreslování pozici své okolí. `Margin` hodnoty mohou být zadány na [rozložení](~/xamarin-forms/user-interface/controls/layouts.md) a [zobrazení](~/xamarin-forms/user-interface/controls/views.md) třídy.
- [ `Padding` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Layout.Padding/) Vlastnost představuje vzdálenost mezi elementem a její podřízené elementy a slouží k oddělení od vlastní obsah ovládacího prvku. `Padding` hodnoty mohou být zadány na [rozložení](~/xamarin-forms/user-interface/controls/layouts.md) třídy.

Následující diagram znázorňuje dvěma konceptů:

[![](margin-and-padding-images/margins-and-padding-sml.png "Okraje a odsazení koncepty")](margin-and-padding-images/margins-and-padding.png "okraje a odsazení koncepty")

Všimněte si, že [ `Margin` ](https://developer.xamarin.com/api/property/Xamarin.Forms.View.Margin/) hodnoty jsou aditivní. Proto pokud dva elementy přiléhající zadat okraj 20 pixelů, vzdálenost mezi elementy, bude 40 pixelů. Kromě toho jsou okraj a odsazení doplňkové obě použité v, že vzdálenost mezi elementem a veškerý obsah, bude okraj a odsazení.

## <a name="specifying-a-thickness"></a>Určení tloušťka

[ `Margin` ](https://developer.xamarin.com/api/property/Xamarin.Forms.View.Margin/) a [ `Padding` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Layout.Padding/) vlastnosti jsou obě typu [ `Thickness` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Thickness/). Existují tři možnosti při vytváření `Thickness` strukturu:

- Vytvoření [ `Thickness` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Thickness/) struktury definované uniform jednu hodnotu. Jednu hodnotu se použije na levé straně, horní, pravé a dolní strany elementu.
- Vytvoření [ `Thickness` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Thickness/) struktury definované hodnoty vodorovného a svislého. Vodorovné hodnota platí symetricky na levé a pravé straně elementu, svislé hodnotou symetricky aplikované na horní a dolní strana elementu.
- Vytvoření [ `Thickness` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Thickness/) struktury definované čtyři odlišné hodnoty, které se použijí na levé straně, horní, pravé a dolní strany elementu.

Následující příklad kódu XAML ukazuje všechny tři možnosti:

```xaml
<StackLayout Padding="0,20,0,0">
  <Label Text="Xamarin.Forms" Margin="20" />
  <Label Text="Xamarin.iOS" Margin="10, 15" />
  <Label Text="Xamarin.Android" Margin="0, 20, 15, 5" />
</StackLayout>
```

Ekvivalentní kódu C# je znázorněno v následujícím příkladu kódu:

```csharp
var stackLayout = new StackLayout {
  Padding = new Thickness(0,20,0,0),
  Children = {
    new Label { Text = "Xamarin.Forms", Margin = new Thickness (20) },
    new Label { Text = "Xamarin.iOS", Margin = new Thickness (10, 25) },
    new Label { Text = "Xamarin.Android", Margin = new Thickness (0, 20, 15, 5) }
  }
};
```

> [!NOTE]
> **Poznámka:**: `Thickness` hodnoty mohou být záporná, což obvykle klipy nebo overdraws obsah.

## <a name="summary"></a>Souhrn

Tento článek ukázán rozdíl mezi [ `Margin` ](https://developer.xamarin.com/api/property/Xamarin.Forms.View.Margin/) a [ `Padding` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Layout.Padding/) vlastnosti a postupu při jejich nastavení. Vlastnosti řízení rozložení chování při vykreslení elementu v uživatelském rozhraní.


## <a name="related-links"></a>Související odkazy

- [Okraj](https://developer.xamarin.com/api/property/Xamarin.Forms.View.Margin/)
- [Odsazení](https://developer.xamarin.com/api/property/Xamarin.Forms.Layout.Padding/)
- [Tloušťka](https://developer.xamarin.com/api/type/Xamarin.Forms.Thickness/)
