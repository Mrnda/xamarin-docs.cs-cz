---
title: Komprese rozložení
description: Komprese rozložení Odebere zadaný rozložení vizuálním stromu ve snaze zvýšit výkon vykreslování stránky. Tento článek vysvětluje postup povolení komprese rozložení a výhody, které můžete zahrnout.
ms.prod: xamarin
ms.assetid: da9e1b26-9d31-4762-94c3-4039f306b7f2
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 12/13/2017
ms.openlocfilehash: 9c698d539ab671ee2a033ae5943a46e0cc870f76
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/04/2018
ms.locfileid: "30791123"
---
# <a name="layout-compression"></a>Komprese rozložení

_Komprese rozložení Odebere zadaný rozložení vizuálním stromu ve snaze zvýšit výkon vykreslování stránky. Tento článek vysvětluje postup povolení komprese rozložení a výhody, které můžete zahrnout._

## <a name="overview"></a>Přehled

Xamarin.Forms provádí rozložení pomocí dvou řad rekurzivní volání metod:

- Rozložení začne v horní části stromu visual se stránkou, a pokračuje prostřednictvím všechny větve vizuálním stromu zahrnovat každý visual element na stránce. Prvky, které jsou rodičů na další elementy jsou zodpovědní za změny velikosti a umístění jejich podřízené relativně k sami.
- Zneplatnění je proces, pomocí kterého změnu element na stránce spustí cyklus nové rozložení. Elementy jsou považovány za neplatné, když už nebude mít správnou velikost nebo pozice. Každý element v visual stromové struktury, která má podřízených prvků, je upozornění pokaždé, když jednu z jejích podřízených změny velikosti. Změnit velikost elementu ve vizuální strojové struktuře proto může způsobit změny, které ripple stromu.

Další informace o tom, jak Xamarin.Forms provede rozložení najdete v tématu [vytváření vlastní rozložení](~/xamarin-forms/user-interface/layouts/custom.md).

Výsledek procesu rozložení je hierarchie nativní ovládací prvky. Tato hierarchie však zahrnuje další kontejner pro vykreslování a obálky pro platformu pro vykreslování, další nafouknutí hierarchii zobrazení vnoření. Čím hlouběji úroveň vnoření, tím větší množství práce, který Xamarin.Forms má provést pro zobrazení stránky. Pro komplexní rozložení může být zobrazení hierarchie hlubších a rozsáhlé, s více úrovní vnoření.

Představte si třeba z ukázkové aplikace pro protokolování do Facebook na následující tlačítko:

![](layout-compression-images/facebook-button.png "Tlačítko Facebook")

Toto tlačítko je zadán jako vlastního ovládacího prvku s následující zobrazení hierarchie XAML:

```xaml
<ContentView ...>
    <StackLayout>
        <StackLayout ...>
            <AbsoluteLayout ...>
                <Button ... />    
                <Image ... />
                <Image ... />
                <BoxView ... />
                <Label ... />
                <Button ... />
            </AbsoluteLayout>
        </StackLayout>
        <Label ... />
    </StackLayout>    
</ContentView>
```

Výsledný vnořené zobrazení hierarchie může být prověřen s [Xamarin Inspector](~/tools/inspector/index.md). V systému Android obsahuje hierarchii vnořené zobrazení 17 zobrazení:

![](layout-compression-images/no-compression.png "Zobrazení hierarchie pro tlačítko Facebook")

Rozložení kompresi, která je k dispozici pro Xamarin.Forms aplikace v iOS a Android platformy, cílem je vyrovnání zobrazení vnoření odebráním zadaný rozložení ze stromu visual, což může zlepšit výkon vykreslení stránky. Výhody výkonu, která je dodávána se liší v závislosti na složitosti stránky, na verzi operačního systému používá a zařízení, na kterém je aplikace spuštěna. Ale největších zvýšení výkonu se zobrazí na starší zařízení.

> [!NOTE]
> Tento článek zaměřuje na výsledcích použití komprese rozložení v systému Android, je rovněž na iOS.

## <a name="layout-compression"></a>Komprese rozložení

V jazyce XAML, může být povolena komprese rozložení nastavením `CompressedLayout.IsHeadless` přidružená vlastnost k `true` rozložení třídy:

```xaml
<StackLayout CompressedLayout.IsHeadless="true">
  ...
</StackLayout>   
```

Alternativně může být povoleno zadáním rozložení instance jako první argument v jazyce C# `CompressedLayout.SetIsHeadless` metoda:

```csharp
CompressedLayout.SetIsHeadless(stackLayout, true);
```

> [!IMPORTANT]
> Vzhledem k tomu, že komprese rozložení a rozložení odebere vizuálním stromu, není vhodný pro rozložení, které mají vzhled nebo který získat dotykové ovládání. Proto se rozložení, nastavte [ `VisualElement` ](https://developer.xamarin.com/api/type/Xamarin.Forms.VisualElement/) vlastnosti (například [ `BackgroundColor` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.BackgroundColor/), [ `IsVisible` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.IsVisible/), [ `Rotation` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Rotation/), [ `Scale` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Scale/), [ `TranslationX` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.TranslationX/) a [ `TranslationY` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.TranslationY/)) nebo která gesta přijmout, nejsou kandidáty pro rozložení komprese. Však povolení komprese rozložení na rozložení, který nastaví vlastnosti vzhled nebo který přijímá gesta, nebude mít za následek chyby sestavení nebo modul runtime. Místo toho komprese rozložení se použijí a vlastnosti vzhled a gesto rozpoznávání bez upozornění selže.

Pro tlačítko Facebook rozložení komprese může být povolena na tři rozložení třídy:

```xaml
<StackLayout CompressedLayout.IsHeadless="true">
    <StackLayout CompressedLayout.IsHeadless="true" ...>
        <AbsoluteLayout CompressedLayout.IsHeadless="true" ...>
            ...
        </AbsoluteLayout>
    </StackLayout>
    ...
</StackLayout>  
```

V systému Android výsledkem vnořené zobrazení hierarchie 14 zobrazení:

![](layout-compression-images/layout-compression.png "Zobrazení hierarchie pro Facebook tlačítko s kompresí rozložení")

Porovnání s původní vnořené zobrazení hierarchie 17 zobrazení, reprezentuje snížení počtu zobrazení % 17. A toto snížení mohou být zobrazeny zanedbatelný, může být větších snížení zobrazení přes celou stránku.

### <a name="fast-renderers"></a>Pro rychlé vykreslování

Rychlé nástroji pro vykreslování snížit inflace a náklady vykreslování ovládacích prvků Xamarin.Forms v systému Android pomocí sloučení výsledná hierarchie nativní zobrazení. Tato další zlepšuje výkon vytvořením menší počet objektů, které pak výsledků v méně složitých vizuálním stromu a menší využití paměti. Další informace o nástroji pro vykreslování rychlé najdete v tématu [rychlé nástroji pro vykreslování](~/xamarin-forms/internals/fast-renderers.md).

Pro tlačítko sítě Facebook v ukázkové aplikaci vytváří kombinace rozložení komprese a rychlé nástroji pro vykreslování vnořených zobrazení hierarchie 8 zobrazení:

![](layout-compression-images/layout-compression-with-fast-renderers.png "Zobrazení hierarchie pro Facebook tlačítko s rozložení komprese a rychlé nástroji pro vykreslování")

Porovnání s původní vnořené zobrazení hierarchie 17 zobrazení, reprezentuje snížení 52 %.

Ukázková aplikace obsahuje stránku extrahoval z reálné aplikaci. Bez komprese rozložení a rychlé nástroji pro vykreslování vytvoří stránky hierarchii vnořené zobrazení 130 zobrazení v systému Android. Povolení rychlého nástroji pro vykreslování a rozložení kompresi na příslušná rozložení třídy snižuje hierarchii vnořené zobrazení do 70 zobrazení, snížení 46 %.

## <a name="summary"></a>Souhrn

Komprese rozložení Odebere zadaný rozložení vizuálním stromu ve snaze zvýšit výkon vykreslování stránky. Výhody výkonu, který to doručí se liší v závislosti na složitosti stránky, na verzi operačního systému používá a zařízení, na kterém je aplikace spuštěna. Ale největších zvýšení výkonu se zobrazí na starší zařízení.


## <a name="related-links"></a>Související odkazy

- [Vytvoření vlastního rozložení](~/xamarin-forms/user-interface/layouts/custom.md)
- [Rychlé renderery](~/xamarin-forms/internals/fast-renderers.md)
- [LayoutCompression (ukázka)](https://developer.xamarin.com/samples/xamarin-forms/userinterface/layoutcompression/)
