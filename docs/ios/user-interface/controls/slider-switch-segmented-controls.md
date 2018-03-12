---
title: "Posuvníky, přepínače a Segmentovaným ovládací prvky"
ms.topic: article
ms.prod: xamarin
ms.assetid: 85BF0EC8-E581-49CD-B9E7-98BE4C5A0F6B
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/21/2017
ms.openlocfilehash: 1c24b1faf7b108466d6e93ffae8112d0dea6d844
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 02/27/2018
---
# <a name="sliders-switches-and-segmented-controls"></a>Posuvníky, přepínače a Segmentovaným ovládací prvky

<a name="Sliders" />


## <a name="sliders"></a>Posuvníky

V ovládacím prvku posuvník umožňuje jednoduché výběr číselnou hodnotu v rozsahu. Ovládací prvek výchozí hodnotu mezi 0 a 1, ale tato omezení lze přizpůsobit.

 [ ![](slider-switch-segmented-controls-images/image25a.png "Posuvník")](slider-switch-segmented-controls-images/image25a.png)

Následující snímek obrazovky ukazuje vlastnosti, které lze upravit v designeru:

 [ ![](slider-switch-segmented-controls-images/image26a.png "Vlastnosti posuvníku")](slider-switch-segmented-controls-images/image25a.png)

Tyto hodnoty lze nastavit v kódu, jak je uvedeno níže, včetně kabeláž až obslužnou rutinu pro aktuálně vybranou hodnotu v zobrazení `UILabel` ovládacího prvku:

```csharp
slider1.MinValue = -1;
slider1.MaxValue = 2;
slider1.Value = 0.5f; // the current value
slider1.ValueChanged += (sender,e) => label1.Text = ((UISlider)sender).Value.ToString ();
```

Můžete taky přizpůsobit vzhled dráze jezdce nastavením

```csharp
slider1.ThumbTintColor = UIColor.Blue;
slider1.MinimumTrackTintColor = UIColor.Gray;
slider1.MaximumTrackTintColor = UIColor.Green;
```

Přizpůsobené posuvníku vypadá takto:

 [ ![](slider-switch-segmented-controls-images/image27a.png "Vlastní posuvníku")](slider-switch-segmented-controls-images/image28a.png)

> [!IMPORTANT]
> V současné době není [chyb](http://stackoverflow.com/a/19496179) způsobuje `ThumbTint` k vykreslení není v době běhu podle očekávání. Můžete přidat následující řádek kódu **před** kód výše jako alternativní řešení. [[Zdroj](http://stackoverflow.com/a/21396794)]:
>
> `slider1.SetThumbImage(UIImage.FromBundle("thumb.png"),UIControlState.Normal);`
> 
> Můžete použít všechny bitové kopie, které bude přepsáno, ale zajistěte, aby je umístěn _v_ adresáře prostředků a je volána v kódu.

<a name="Switch" />

## <a name="switch"></a>přepínače

používá iOS `UISwitch` jako logická hodnota vstupu, který může být reprezentována přepínač na jiných platformách. Uživatele můžete upravit ovládacího prvku přesunutím *jezdec* mezi **zapnutí nebo vypnutí** pozic.

 [ ![](slider-switch-segmented-controls-images/image28a.png "přepínače")](slider-switch-segmented-controls-images/image28a.png)

Lze přizpůsobit vzhled přepínače v **vlastnosti Pad** návrháře, což vám umožní řídit výchozí stav, **zapnutí nebo vypnutí TINT –** barvy a **samostatnými Image**. To je znázorněno na obrázku níže:

 [ ![](slider-switch-segmented-controls-images/image29a.png "Vlastnosti přepínače")](slider-switch-segmented-controls-images/image29a.png)

V kódu můžete také nastavit vlastnosti přepínače, například následující kód zobrazí přepínač se na výchozí hodnotu `On`:

```csharp
switch1.On = true;
```

 <a name="Segmented_Controls" />


## <a name="segmented-controls"></a>Segmentovaným ovládací prvky

Segmentovaným řízení je lze snadno povolit uživatelům interakci s malý počet možností. Je vodorovně nastíněny a každý segment funkce jako samostatný tlačítka. Při použití návrháře, Segmentovaným řízení najdete v části **sada nástrojů > ovládací prvky**a by měl vypadat jako na následujícím obrázku:

 [ ![](slider-switch-segmented-controls-images/segmentedcontrol.png "Segmentovaným ovládací prvek")](slider-switch-segmented-controls-images/segmentedcontrol.png)

Jedinečné funkce návrháře umožňuje každý segment, který se na návrhovou plochu, vybrat jednotlivě, jak je uvedeno dále:

 [ ![](slider-switch-segmented-controls-images/segmentedcontrolselection.png "Segmentovaným ovládací prvek")](slider-switch-segmented-controls-images/segmentedcontrolselection.png)

To umožňuje panelu Vlastnosti pro které bude použito ke přesněji řídit vlastnosti každého segmentu. Můžete zjistit upravovat vlastnosti na tomto snímku obrazovky:

 [ ![](slider-switch-segmented-controls-images/segmentedcontrolproperties.png "Segmentovaným ovládací prvek")](slider-switch-segmented-controls-images/segmentedcontrolproperties.png)

Je potřeba poznamenat, že styl Segmentovaným ovládací prvek se již nepoužívá v systému IOS 7 a proto úpravě možnosti pro tento v ios7 nespustí aplikaci nebude mít žádný vliv.

## <a name="related-links"></a>Související odkazy

- [Ovládací prvky (ukázka)](https://developer.xamarin.com/samples/Controls/)
- [Výstrahy řadiče](https://developer.xamarin.com/recipes/ios/standard_controls/alertcontroller/)