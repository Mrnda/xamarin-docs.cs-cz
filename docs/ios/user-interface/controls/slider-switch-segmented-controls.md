---
title: Posuvníky, přepínače a segmentované ovládací prvky v Xamarin.iosu
description: Tento dokument popisuje snímky, přepínače a segmentované ovládací prvky v Xamarin.iosu popisující, jak pracovat s nimi prostřednictvím kódu programu i v iOS designeru.
ms.prod: xamarin
ms.assetid: 85BF0EC8-E581-49CD-B9E7-98BE4C5A0F6B
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/21/2017
ms.openlocfilehash: 7df79cb6f225326dda6656fa9dfe9534e35f2457
ms.sourcegitcommit: b56b3f906d2c05a3f1be219ef41be8b79e519b8e
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/25/2018
ms.locfileid: "39241988"
---
# <a name="sliders-switches-and-segmented-controls-in-xamarinios"></a>Posuvníky, přepínače a segmentované ovládací prvky v Xamarin.iosu

<a name="Sliders" />

## <a name="sliders"></a>Posuvníky

V ovládacím prvku posuvník umožňuje jednoduchý výběr číselnou hodnotu v rozsahu. Ovládací prvek má výchozí hodnotu mezi 0 a 1, ale tato omezení se dají přizpůsobit.

 [![](slider-switch-segmented-controls-images/image25a.png "Posuvník")](slider-switch-segmented-controls-images/image25a.png#lightbox)

Následující snímek obrazovky ukazuje vlastnosti, které lze upravit v Návrháři:

 [![](slider-switch-segmented-controls-images/image26a.png "Vlastnosti posuvník")](slider-switch-segmented-controls-images/image25a.png#lightbox)

Tyto hodnoty lze nastavit v kódu, jak je znázorněno níže, včetně zapojení do obslužné rutiny pro aktuálně vybrané hodnoty v zobrazení `UILabel` ovládacího prvku:

```csharp
slider1.MinValue = -1;
slider1.MaxValue = 2;
slider1.Value = 0.5f; // the current value
slider1.ValueChanged += (sender,e) => label1.Text = ((UISlider)sender).Value.ToString ();
```

Můžete také upravit vzhled posuvníku nastavením

```csharp
slider1.ThumbTintColor = UIColor.Blue;
slider1.MinimumTrackTintColor = UIColor.Gray;
slider1.MaximumTrackTintColor = UIColor.Green;
```

Vlastní posuvník vypadá takto:

 [![](slider-switch-segmented-controls-images/image27a.png "Vlastní posuvník")](slider-switch-segmented-controls-images/image28a.png#lightbox)

> [!IMPORTANT]
> Aktuálně nejsou k dispozici [chyb](http://stackoverflow.com/a/19496179) příčinou `ThumbTint` vykreslení v době běhu podle očekávání. Můžete přidat následující řádek kódu **před** kódu nad jako alternativní řešení. [[Zdroj](http://stackoverflow.com/a/21396794)]:
>
> `slider1.SetThumbImage(UIImage.FromBundle("thumb.png"),UIControlState.Normal);`
> 
> Můžete použít libovolné obrázku, bude přepsána, ale ujistěte se, že je umístěn _v_ adresáře prostředků a je volána v kódu.

<a name="Switch" />

## <a name="switch"></a>Přepínač

používá iOS `UISwitch` jako vstup logickou hodnotu, která může být reprezentována přepínač na jiných platformách. Uživatel můžete manipulovat s prvkem přechodem *thumb* mezi **zapnuto/vypnuto** pozic.

 [![](slider-switch-segmented-controls-images/image28a.png "Přepínač")](slider-switch-segmented-controls-images/image28a.png#lightbox)

Lze přizpůsobit vzhled přepínač **oblasti vlastnosti** návrháře, který vám umožní řídit výchozí stav, **zapnuto/vypnuto odstín** barvy a **Image nebo vypnout**. To je znázorněno na následujícím obrázku:

 [![](slider-switch-segmented-controls-images/image29a.png "Vlastnosti přepínače")](slider-switch-segmented-controls-images/image29a.png#lightbox)

Vlastnosti přepínače lze také nastavit v kódu, například následující kód zobrazí přepínače s výchozí hodnotou `On`:

```csharp
switch1.On = true;
```

 <a name="Segmented_Controls" />


## <a name="segmented-controls"></a>Segmentované ovládací prvky

Segmentované ovládací prvek je uspořádaný způsob, jak povolit uživatelům interakci s malý počet možností. Je vodorovně rozloží a každý segment funkce, jako samostatné tlačítko. Při použití návrháře segmentované ovládací prvek najdete v části **nástrojů > ovládací prvky**a měl by vypadat jako na následujícím obrázku:

 [![](slider-switch-segmented-controls-images/segmentedcontrol.png "Segmentované ovládací prvek")](slider-switch-segmented-controls-images/segmentedcontrol.png#lightbox)

Jedinečné funkce návrháře umožňuje každý segment výběr jednotlivě na návrhové ploše, jak je znázorněno níže:

 [![](slider-switch-segmented-controls-images/segmentedcontrolselection.png "Segmentované ovládací prvek")](slider-switch-segmented-controls-images/segmentedcontrolselection.png#lightbox)

To umožňuje oblasti vlastnosti se použije k přesněji řídit vlastnosti každého segmentu. Zobrazí upravitelné vlastnosti v následujícím snímku obrazovky:

 [![](slider-switch-segmented-controls-images/segmentedcontrolproperties.png "Segmentované ovládací prvek")](slider-switch-segmented-controls-images/segmentedcontrolproperties.png#lightbox)

Je třeba poznamenat, že Segmentovaným stylu ovládacího prvku se v iOS7 nepoužívá, a proto nastavení možnosti v iOS7 aplikace nebude mít žádný efekt.

## <a name="related-links"></a>Související odkazy

- [Ovládací prvky (ukázka)](https://developer.xamarin.com/samples/Controls/)
- [Upozornění Kontroleru](https://github.com/xamarin/recipes/tree/master/Recipes/ios/standard_controls/alertcontroller)
