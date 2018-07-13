---
title: Okraje a odsazení
description: Na okraj a odsazení vlastnosti řídit rozložení chování při vykreslení elementu v uživatelském rozhraní. Tento článek ukazuje rozdíl mezi dvěma vlastnostmi a jejich nastavení.
ms.prod: xamarin
ms.assetid: BEB096BB-51DF-410F-B0F1-D235287B0F4A
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 04/27/2016
ms.openlocfilehash: 595e673c59d23a45cbaf923a0d58faff2000c296
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/12/2018
ms.locfileid: "38996613"
---
# <a name="margin-and-padding"></a>Okraje a odsazení

_Na okraj a odsazení vlastnosti řídit rozložení chování při vykreslení elementu v uživatelském rozhraní. Tento článek ukazuje rozdíl mezi dvěma vlastnostmi a jejich nastavení._

## <a name="overview"></a>Přehled

Okraje a odsazení jsou související rozložení koncepty:

- [ `Margin` ](xref:Xamarin.Forms.View.Margin) Vlastnost představuje vzdálenost mezi prvkem a jeho sousedícími elementy a slouží k řízení pozici vykreslení tohoto prvku a jeho okolím pozici vykreslení. `Margin` je možné zadat hodnoty na [rozložení](~/xamarin-forms/user-interface/controls/layouts.md) a [zobrazení](~/xamarin-forms/user-interface/controls/views.md) třídy.
- [ `Padding` ](xref:Xamarin.Forms.Layout.Padding) Vlastnost představuje vzdálenost mezi prvkem a jeho podřízené prvky a slouží k oddělení ovládacího prvku z jeho vlastní obsah. `Padding` je možné zadat hodnoty na [rozložení](~/xamarin-forms/user-interface/controls/layouts.md) třídy.

Následující diagram znázorňuje dvě koncepty:

[![](margin-and-padding-images/margins-and-padding-sml.png "Okraje a odsazení koncepty")](margin-and-padding-images/margins-and-padding.png#lightbox "okraje a odsazení koncepty")

Všimněte si, že [ `Margin` ](xref:Xamarin.Forms.View.Margin) hodnoty jsou aditivní. Proto pokud dva sousedící prvky okraj 20 pixelů, bude vzdálenost mezi elementy 40 pixelů. Kromě toho jsou okraje a odsazení additive při použití i, v tom, že vzdálenost mezi prvkem a veškerý obsah bude okraj a odsazení.

## <a name="specifying-a-thickness"></a>Určení tloušťku ohraničení

[ `Margin` ](xref:Xamarin.Forms.View.Margin) a [ `Padding` ](xref:Xamarin.Forms.Layout.Padding) vlastnosti jsou typu [ `Thickness` ](xref:Xamarin.Forms.Thickness). Existují tři možnosti při vytváření `Thickness` struktury:

- Vytvoření [ `Thickness` ](xref:Xamarin.Forms.Thickness) struktury definované jednotné jednu hodnotu. Vlevo, horní, pravé a dolní strana elementu, který se použije jednu hodnotu.
- Vytvoření [ `Thickness` ](xref:Xamarin.Forms.Thickness) struktury definované hodnoty vodorovného a svislého. Vodorovné hodnotu je symetricky použít na levé a pravé straně elementu, svislé hodnotou symetricky zavádí horní a dolní strana elementu.
- Vytvoření [ `Thickness` ](xref:Xamarin.Forms.Thickness) struktury definované čtyři různé hodnoty, které se použijí na levé straně, horní, pravé a dolní strana elementu.

Následující příklad kódu XAML ukazuje všechny tři možnosti:

```xaml
<StackLayout Padding="0,20,0,0">
  <Label Text="Xamarin.Forms" Margin="20" />
  <Label Text="Xamarin.iOS" Margin="10, 15" />
  <Label Text="Xamarin.Android" Margin="0, 20, 15, 5" />
</StackLayout>
```

Ekvivalentní kód jazyka C# můžete vidět v následujícím příkladu kódu:

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
> `Thickness` hodnoty mohou být záporná, který se obvykle klipy nebo overdraws obsah.

## <a name="summary"></a>Souhrn

V tomto článku jsme vám ukázali rozdíl mezi [ `Margin` ](xref:Xamarin.Forms.View.Margin) a [ `Padding` ](xref:Xamarin.Forms.Layout.Padding) vlastností a jejich nastavení. Vlastnosti řídit rozložení chování při vykreslení elementu v uživatelském rozhraní.


## <a name="related-links"></a>Související odkazy

- [Okraj](xref:Xamarin.Forms.View.Margin)
- [Odsazení](xref:Xamarin.Forms.Layout.Padding)
- [Tloušťka](xref:Xamarin.Forms.Thickness)
