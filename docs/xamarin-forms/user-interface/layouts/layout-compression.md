---
title: Komprese rozložení
description: Komprese rozložení odebere zadané rozložení z vizuálního stromu za účelem zvýšení výkonu vykreslování stránky. Tento článek vysvětluje, jak povolit kompresi rozložení a výhody, které mohou přinést.
ms.prod: xamarin
ms.assetid: da9e1b26-9d31-4762-94c3-4039f306b7f2
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 12/13/2017
ms.openlocfilehash: ba9be51daa32be1034e2bdfafafe80c45d00d83c
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/12/2018
ms.locfileid: "38995229"
---
# <a name="layout-compression"></a>Komprese rozložení

_Komprese rozložení odebere zadané rozložení z vizuálního stromu za účelem zvýšení výkonu vykreslování stránky. Tento článek vysvětluje, jak povolit kompresi rozložení a výhody, které mohou přinést._

## <a name="overview"></a>Přehled

Xamarin.Forms provádí rozložení pomocí dvou řad rekurzivní volání metody:

- Rozložení začíná v horní části stránky z vizuálního stromu se stránkou, a pokračuje přes všechny větve ve vizuálním stromu rozšiřovat a zahrnovat každou vizuální prvek na stránce. Prvky, které jsou rodičům, aby ostatní prvky zodpovídají za změně velikosti a umístění jejich podřízených vzhledem k sami.
- Je proces, pomocí kterého změna v elementu na stránce spustí nový cyklus rozložení. Prvky jsou považovány za neplatné, když už nebude mít správnou velikost nebo pozice. Každý prvek ve vizuálním stromu, který má podřízené položky se zobrazí upozornění vždy, když jedna z jejích podřízených změny velikosti. Změnit velikost elementu ve vizuálním stromu proto může způsobit změny, které ripple směrem nahoru.

Další informace o tom, jak Xamarin.Forms provádí rozložení najdete v tématu [vytvoření rozložení platného pro vlastní](~/xamarin-forms/user-interface/layouts/custom.md).

Výsledek procesu rozložení je hierarchie nativní ovládací prvky. Tato hierarchie však zahrnuje další kontejneru renderery a obálky pro platformu renderery, další nafouknutí zobrazit hierarchii vnoření. Čím hlouběji úrovní vnoření, tím větší množství práce, které Xamarin.Forms má provést pro zobrazení stránky. Pro složitá rozložení lze zobrazit hierarchii hluboké a široké, s více úrovní vnoření.

Představte si třeba následující tlačítko pro přihlášení k Facebooku v ukázkové aplikaci:

![](layout-compression-images/facebook-button.png "Tlačítko Facebooku")

Toto tlačítko je zadán jako vlastní ovládací prvek s následující zobrazení hierarchie XAML:

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

Výsledná vnořená zobrazení hierarchie se dají prozkoumat s [Xamarin Inspector](~/tools/inspector/index.md). V Androidu obsahuje hierarchii vnořená zobrazení 17 zobrazení:

![](layout-compression-images/no-compression.png "Zobrazit hierarchii pro tlačítko Facebooku")

Komprese rozložení, který je k dispozici pro aplikace Xamarin.Forms v Iosu a Androidu platformy, zaměřuje k vyrovnání zobrazení vnoření tak, že odeberete zadanou rozložení z vizuálního stromu, což může zlepšit výkon vykreslování části stránky. Výhody výkonu, která je dodávána se liší v závislosti na složitosti stránku, verze operačního systému se používají a zařízení, na kterém je aplikace spuštěna. Největší zvýšení výkonu se však projeví na starší zařízení.

> [!NOTE]
> Přestože tento článek se zaměřuje na výsledky použití komprese rozložení v Androidu, se vztahuje rovněž na iOS.

## <a name="layout-compression"></a>Komprese rozložení

V XAML, můžete jej povolit nastavením komprese rozložení `CompressedLayout.IsHeadless` připojené vlastnosti `true` rozložení třídy:

```xaml
<StackLayout CompressedLayout.IsHeadless="true">
  ...
</StackLayout>   
```

Alternativně může být povoleno zadáním instance rozložení jako první argument v jazyce C# `CompressedLayout.SetIsHeadless` metody:

```csharp
CompressedLayout.SetIsHeadless(stackLayout, true);
```

> [!IMPORTANT]
> Protože komprese rozložení odebere rozložení z vizuálního stromu, není vhodný pro rozložení, které mají vizuálního vzhledu, nebo který získat dotykové ovládání. Proto se rozložení, který nastavte [ `VisualElement` ](xref:Xamarin.Forms.VisualElement) vlastnosti (například [ `BackgroundColor` ](xref:Xamarin.Forms.VisualElement.BackgroundColor), [ `IsVisible` ](xref:Xamarin.Forms.VisualElement.IsVisible), [ `Rotation` ](xref:Xamarin.Forms.VisualElement.Rotation), [ `Scale` ](xref:Xamarin.Forms.VisualElement.Scale), [ `TranslationX` ](xref:Xamarin.Forms.VisualElement.TranslationX) a [ `TranslationY` ](xref:Xamarin.Forms.VisualElement.TranslationY) nebo, který přijímá gesta, nejsou kandidáty pro rozložení komprese. Ale povolení komprese rozložení na rozložení, který nastavuje vlastnosti vzhled nebo, který přijímá gesta, nebude mít za následek chybu sestavení nebo modul runtime. Místo toho budou použity komprese rozložení a vzhled vlastnosti a rozpoznání gest se bez upozornění nepodaří.

Tlačítko Facebook může být povolena komprese rozložení na tři rozložení třídy:

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

V Androidu výsledkem je vnořená zobrazení hierarchie 14 zobrazení:

![](layout-compression-images/layout-compression.png "Zobrazit hierarchii pro Facebook tlačítko Komprese rozložení")

Porovnání s původní vnořené zobrazení hierarchie 17 zobrazení, to představuje snížení počtu zobrazení 17 %. A toto snížení mohou být zobrazeny nevýznamné, může být mnohem závažnější snížení zobrazení přes celou stránku.

### <a name="fast-renderers"></a>Rychlé Renderery

Rychlé renderery snížit inflaci a náklady na vykreslení ovládacích prvků Xamarin.Forms v Androidu linearizovat výsledný nativní zobrazit hierarchii. To dále zvyšuje výkon tím, že vytvoříte méně objektů, což zase vede v méně složitých vizuální strom a nižší využití paměti. Další informace o rychlé renderery, naleznete v tématu [rychlé Renderery](~/xamarin-forms/internals/fast-renderers.md).

Pro tlačítko sítě Facebook v ukázkové aplikaci vytvoří spojení komprese rozložení a rychlé renderery vnořené zobrazení hierarchie 8 zobrazení:

![](layout-compression-images/layout-compression-with-fast-renderers.png "Zobrazit hierarchii pro Facebook tlačítko s komprese rozložení a rychlé Renderery")

Porovnání s původní vnořené zobrazení hierarchie 17 zobrazení, to představuje snížení 52 %.

Ukázková aplikace obsahuje stránku extrahují z aplikace skutečný. Na stránce bez komprese rozložení a rychlé renderery, vytvoří vnořené zobrazení hierarchie 130 zobrazení v Androidu. Vnořené zobrazení hierarchie umožňující rychlé renderery a komprese rozložení na odpovídající rozložení třídy snižuje 70 zobrazeními, snížení 46 %.

## <a name="summary"></a>Souhrn

Komprese rozložení odebere zadané rozložení z vizuálního stromu za účelem zvýšení výkonu vykreslování stránky. Zlepšuje výkon, který to poskytuje se liší v závislosti na složitosti stránku, verze operačního systému se používají a zařízení, na kterém je aplikace spuštěna. Největší zvýšení výkonu se však projeví na starší zařízení.


## <a name="related-links"></a>Související odkazy

- [Vytvoření vlastního rozložení](~/xamarin-forms/user-interface/layouts/custom.md)
- [Rychlé renderery](~/xamarin-forms/internals/fast-renderers.md)
- [LayoutCompression (ukázka)](https://developer.xamarin.com/samples/xamarin-forms/userinterface/layoutcompression/)
