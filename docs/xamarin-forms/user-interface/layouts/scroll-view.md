---
title: ScrollView
description: Pomocí ScrollView rozložení, které se nemůže vejít na právě jednu obrazovku a obsah se uvolnil prostor pro klávesnice k dispozici.
ms.prod: xamarin
ms.assetid: 7B542872-B3D1-49B3-B15E-0E98F53C1F6E
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 04/22/2016
ms.openlocfilehash: 13f3d5c02b8451bcd52b355fb89f7931f50a0d39
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/04/2018
---
# <a name="scrollview"></a>ScrollView

[`ScrollView`](https://developer.xamarin.com/api/type/Xamarin.Forms.ScrollView/) obsahuje rozložení a umožňuje jim scroll mimo obrazovku. `ScrollView` také používá k povolení zobrazení automaticky přejít na viditelnou část obrazovky, když se zobrazuje na klávesnici.

[![](scroll-view-images/layouts-sml.png "Rozložení Xamarin.Forms")](scroll-view-images/layouts.png#lightbox "Xamarin.Forms rozložení")

Tento článek se zabývá:

- **[Účel](#Purpose)**  &ndash; účelu `ScrollView` a kdy se používá.
- **[Využití](#Usage)**  &ndash; použití `ScrollView` v praxi.
- **[Vlastnosti](#Properties)**  &ndash; veřejné vlastnosti, které může číst a upravovat.
- **[Metody](#Methods)**  &ndash; veřejné metody, které lze volat pro posuňte zobrazení.
- **[Události](#Events)**  &ndash; události, které lze použít pro naslouchání na změny ve stavu zobrazení.

## <a name="purpose"></a>Účel

`ScrollView` slouží k zajištění vyšší zobrazení zobrazení i na menší telefony. Například může být v zařízení iPhone 4s oříznut rozložení, který funguje na zařízení typu iPhone 6s. Použití `ScrollView` by umožnilo oříznutí části rozložení, který se má zobrazit na obrazovce menší.

## <a name="usage"></a>Použití

> [!NOTE]
> `ScrollView`s nesmí být vnořený. Kromě toho `ScrollView`s nesmí být vnořené s další ovládací prvky, které poskytují posouvání, jako je třeba `ListView` a `WebView`.

`ScrollView` zpřístupní `Content` vlastnost, která může být nastaven na jediné zobrazení nebo rozložení. Vezměme si jako příklad rozložení s velmi velké boxView, za nímž následuje `Entry`:

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
stack.Children.Add(new BoxView { BackgroundColor = Color.Red,   HeightRequest = 600, WidthRequest = 600 });
stack.Children.Add(new Entry());
```

Předtím, než uživatel posune stránku dolů, jenom `BoxView` se zobrazí:

![](scroll-view-images/scroll-start.png "BoxView v ScrollView")

Všimněte si, že když uživatel spustí zadání textu v `Entry`, zobrazení posune zachovat viditelný na obrazovce:

![](scroll-view-images/scroll-end.png "Položka v ScrollView")

## <a name="properties"></a>Vlastnosti

ScrollView má následující vlastnosti:

- **Obsahu** &ndash; získá nebo nastaví zobrazení v `ScrollView`.
- **[ContentSize](https://developer.xamarin.com/api/type/Xamarin.Forms.Size/)**  &ndash; jen pro čtení, získá velikost obsahu, který obsahuje komponentu šířky a výšky. Toto je vazbu vlastnost
- **[Orientace](https://developer.xamarin.com/api/type/Xamarin.Forms.ScrollOrientation/)**  &ndash; jde [ `ScrollOrientation` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ScrollOrientation/), které je výčet, který může být nastaven na `Horizontal`, `Vertical`, nebo `Both`.
- **ScrollX** &ndash; jen pro čtení, získá aktuální pozici posunutí v dimenzi X.
- **ScrollY** &ndash; jen pro čtení, získá aktuální pozici posunutí v dimenzi Y.

## <a name="methods"></a>Metody

`ScrollView` poskytuje `ScrollToAsync` metodu, která slouží k posunutí zobrazení pomocí souřadnic nebo pomocí zadání konkrétní zobrazení, které by měla být dostupná.

Pokud používáte souřadnice, zadejte `x` a `y` souřadnice, společně s logickou hodnotu udávající, zda by měl animovaný posouvání:

```csharp
scroll.ScrollToAsync(0, 150, true); //scrolls so that the position at 150px from the top is visible

scroll.ScrollToAsync(label, ScrollToPosition.Start, true); //scrolls so that the label is at the start of the list
```

Při posouvání konkrétní prvek, `ScrollToPosition` výčet Určuje, kde v zobrazení se zobrazí element:

- **Center** &ndash; posune elementu k centru viditelnou část zobrazení.
- **End** &ndash; posune prvek na konec viditelnou část zobrazení.
- **MakeVisible** &ndash; posune element tak, aby se viditelné v rámci zobrazení.
- **Spustit** &ndash; posune prvek na začátek viditelnou část zobrazení.

`IsAnimated` Vlastnost určuje, jak bude posunout zobrazení. Když nastaven na hodnotu true, technologie smooth animace se použije, místo okamžitě Přesun obsahu do zobrazení.

## <a name="events"></a>Události

`ScrollView` zpřístupní pouze jednu událost `Scrolled`. `Scrolled` je vyvolána po dokončení posouvání zobrazení. Obslužné rutiny události pro `Scrolled` trvá `ScrolledEventArgs`, který má `ScrollX` a `ScrollY` vlastnosti. Následující ukazuje, jak aktualizovat štítek s aktuální pozici posunutí `ScrollView`:

```csharp
Label label = new Label { Text = "Position: " };
ScrollView scroll = new ScrollView();
scroll.Scrolled += (object sender, ScrolledEventArgs e) => {
    label.Text = "Position: " + e.ScrollX + " x " + e.ScrollY;
};
```

Všimněte si, že posuňte pozic může být záporná, z důvodu vrátit odesílateli účinek při posouvání na konci tohoto seznamu.


## <a name="related-links"></a>Související odkazy

- [Rozložení (ukázka)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/Layout/)
- [Příklad BusinessTumble (ukázka)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/BusinessTumble/)
