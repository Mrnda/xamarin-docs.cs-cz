---
title: Xamarin.Forms ScrollView
description: Tento článek vysvětluje, jak použít třídu Xamarin.Forms ScrollView prezentovat rozložení, který se nemůže vejít na právě jednu obrazovku a obsahem uvolnilo místo pro klávesnici.
ms.prod: xamarin
ms.assetid: 7B542872-B3D1-49B3-B15E-0E98F53C1F6E
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 07/10/2018
ms.openlocfilehash: 54fdec74a6e1d0aee71ec0ca6809a5b40680de9f
ms.sourcegitcommit: 0a1c392829454468dbe92f81d975e124a22b7014
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/31/2018
ms.locfileid: "39360809"
---
# <a name="xamarinforms-scrollview"></a>Xamarin.Forms ScrollView

[`ScrollView`](xref:Xamarin.Forms.ScrollView) obsahuje rozložení a umožňuje jim posuvníku mimo obrazovku. `ScrollView` slouží také povolit zobrazení automaticky přesunout do viditelnou část obrazovky při zobrazování klávesnice.

[![](scroll-view-images/layouts-sml.png "Rozložení Xamarin.Forms")](scroll-view-images/layouts.png#lightbox "rozložení Xamarin.Forms")

Tento článek se týká:

- **[Účel](#purpose)**  &ndash; účelu `ScrollView` a kdy se používá.
- **[Využití](#usage)**  &ndash; použití `ScrollView` v praxi.
- **[Vlastnosti](#properties)**  &ndash; veřejné vlastnosti, které může číst a upravovat.
- **[Metody](#methods)**  &ndash; veřejné metody, které lze volat pro posouvání zobrazení.
- **[Události](#events)**  &ndash; události, které můžete použít k naslouchání změnám v zobrazení stavů.

## <a name="purpose"></a>Účel

`ScrollView` je možné zajistit, že větší zobrazení dobře na telefonech menší. Například může být rozložení, který funguje na Iphonu 6s oříznut v zařízení iPhone 4s. Použití `ScrollView` by umožnilo zkrácený částí rozložení, které se zobrazí na obrazovce menší.

## <a name="usage"></a>Použití

> [!NOTE]
> `ScrollView`s by neměl být vnořený. Kromě toho `ScrollView`s by neměl být vnořena s jinými ovládacími prvky, které poskytují posouvání, jako je třeba `ListView` a `WebView`.

`ScrollView` Zpřístupňuje `Content` vlastnost, která můžete nastavit na jedno zobrazení nebo rozložení. Podívejte se například rozložení s velmi velké boxView, za nímž následuje `Entry`:

```xaml
<ContentPage.Content>
    <ScrollView>
        <StackLayout>
            <BoxView BackgroundColor="Red" HeightRequest="600" WidthRequest="150" />
            <Entry />
        </StackLayout>
    </ScrollView>
</ContentPage.Content>
```

V jazyce C#:

```csharp
var scroll = new ScrollView();
Content = scroll;
var stack = new StackLayout();
stack.Children.Add(new BoxView { BackgroundColor = Color.Red,    HeightRequest = 600, WidthRequest = 600 });
stack.Children.Add(new Entry());
```

Předtím, než uživatel posouvání, pouze `BoxView` je viditelná:

![](scroll-view-images/scroll-start.png "BoxView v ScrollView")

Všimněte si, že když uživatel začne zadávat text `Entry`, zobrazení se posune zůstat viditelné na obrazovce:

![](scroll-view-images/scroll-end.png "Položka v ScrollView")

## <a name="properties"></a>Vlastnosti

`ScrollView` definuje následující vlastnosti:

- [`ContentSize`](xref:Xamarin.Forms.ScrollView.ContentSizeProperty) Získá [ `Size` ](xref:Xamarin.Forms.Size) hodnotu, která představuje velikost obsahu.
- [`Orientation`](xref:Xamarin.Forms.ScrollView.OrientationProperty) Získá nebo nastaví [ `ScrollOrientation` ](xref:Xamarin.Forms.ScrollOrientation) hodnota výčtu, která představuje posouvání směr `ScrollView`.
- [`ScrollX`](xref:Xamarin.Forms.ScrollView.ScrollXProperty) Získá `double` , který představuje aktuální pozice posuvníku X.
- [`ScrollY`](xref:Xamarin.Forms.ScrollView.ScrollYProperty) Získá `double` , který představuje aktuální pozici posunutí Y.
- [`HorizontalScrollBarVisibility`](xref:Xamarin.Forms.ScrollView.HorizontalScrollBarVisibilityProperty) Získá nebo nastaví [ `ScrollBarVisibility` ](xref:Xamarin.Forms.ScrollBarVisibility) hodnotu, která představuje, když je viditelné vodorovný posuvník.
- [`VerticalScrollBarVisibility`](xref:Xamarin.Forms.ScrollView.VerticalScrollBarVisibilityProperty) Získá nebo nastaví [ `ScrollBarVisibility` ](xref:Xamarin.Forms.ScrollBarVisibility) hodnotu, která představuje, když je zobrazen panel svislý posuvník.

## <a name="methods"></a>Metody

`ScrollView` poskytuje `ScrollToAsync` metodu, která slouží k posunutí zobrazení pomocí souřadnic nebo zadáním určité zobrazení, která by měla být dostupná.

Při použití souřadnice, zadejte `x` a `y` souřadnice, spolu s logickou hodnotu označující, zda by měl být animován posouvání:

```csharp
scroll.ScrollToAsync(0, 150, true); //scrolls so that the position at 150px from the top is visible

scroll.ScrollToAsync(label, ScrollToPosition.Start, true); //scrolls so that the label is at the start of the list
```

Při posouvání na konkrétní element `ScrollToPosition` výčet Určuje, kde v zobrazení se zobrazí element:

- **System Center** &ndash; posune element na střed viditelnou část zobrazení.
- **End** &ndash; posune prvek na konec viditelnou část zobrazení.
- **MakeVisible** &ndash; posune element tak, aby byly viditelné v rámci zobrazení.
- **Spustit** &ndash; posune elementu na začátku viditelnou část zobrazení.

`IsAnimated` Vlastnost určuje, jak bude posunu zobrazení. Když nastavenou na hodnotu true, plynulou animaci se použije, nikoli okamžité Přesun obsahu do zobrazení.

## <a name="events"></a>Události

`ScrollView` Definuje jen jedna událost, `Scrolled`. `Scrolled` je aktivována po dokončení posouvání zobrazení. Obslužnou rutinu události pro `Scrolled` trvá `ScrolledEventArgs`, který má `ScrollX` a `ScrollY` vlastnosti. Následující ukazuje, jak aktualizovat aktuální pozici posunutí popisek `ScrollView`:

```csharp
Label label = new Label { Text = "Position: " };
ScrollView scroll = new ScrollView();
scroll.Scrolled += (object sender, ScrolledEventArgs e) => {
    label.Text = "Position: " + e.ScrollX + " x " + e.ScrollY;
};
```

Všimněte si, že pozice posuvníku, mohou být záporná, kvůli opuštění efekt při posouvání na konci seznamu.


## <a name="related-links"></a>Související odkazy

- [Rozložení (ukázka)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/Layout/)
- [Příklad BusinessTumble (ukázka)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/BusinessTumble/)
