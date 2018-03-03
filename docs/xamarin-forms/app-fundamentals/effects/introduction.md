---
title: "Úvod do efekty"
description: "Účinky Povolit nativní ovládací prvky na každou platformu, která lze přizpůsobit a jsou obvykle používány pro malé stylů změny. Tento článek obsahuje úvod do důsledky, popisuje hranice mezi efekty a vlastní nástroji pro vykreslování a popisuje třídy PlatformEffect."
ms.topic: article
ms.prod: xamarin
ms.assetid: 30CB8615-8F39-4762-BDB7-333D2B57D112
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 03/08/2016
ms.openlocfilehash: b66f47ecb8f955f6558df6fff18af92a7a8b97cf
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 02/27/2018
---
# <a name="introduction-to-effects"></a>Úvod do efekty

_Účinky Povolit nativní ovládací prvky na každou platformu, která lze přizpůsobit a jsou obvykle používány pro malé stylů změny. Tento článek obsahuje úvod do důsledky, popisuje hranice mezi efekty a vlastní nástroji pro vykreslování a popisuje třídy PlatformEffect._

Xamarin.Forms [rozložení stránky a ovládací prvky](~/xamarin-forms/user-interface/controls/index.md) uvede společné rozhraní API k popisu napříč platformami mobilních uživatelská rozhraní. Každé stránky, rozložení a řízení vykreslením jinak na každé platformě pomocí `Renderer` třídu, která zase vytvoří nativní ovládací prvek (odpovídající Xamarin.Forms zastupování), uspořádá na obrazovce a přidá zadané v sdílený chování kód.

Vývojářům můžete implementovat vlastní vlastní `Renderer` třídy k přizpůsobení vzhledu a chování ovládacího prvku. Implementace třídy vlastní zobrazovací jednotky k přizpůsobení jednoduchý ovládací prvek je však často zobrazené – odpověď. Tento proces, což nativní ovládací prvky na každou platformu, která lze snadno přizpůsobit zjednodušit účinky.

Účinky jsou vytvořené v projektech specifické pro platformu vytváření podtříd `PlatformEffect` řízení a důsledky jsou spotřebovávají jejich připojením k vhodný ovládací prvek v projektu Xamarin.Forms přenosných třída knihovny PCL () nebo sdílené knihovny.

## <a name="why-use-an-effect-over-a-custom-renderer"></a>Proč používat vliv přes vlastní zobrazovací jednotky?

Účinky zjednodušit přizpůsobení ovládacího prvku, jsou opakovaně použitelné a lze nastavit parametry dále zvýšit opakované použití.

Všechno, co můžete dosáhnout s vliv lze také dosáhnout s vlastní zobrazovací jednotky. Vlastní nástroji pro vykreslování však nabízí větší flexibilitu a přizpůsobení než účinky. Následující pokyny seznamu v případech, ve kterém se má zvolte efekt přes vlastní zobrazovací jednotky:

- Efekt se doporučuje změna vlastností ovládacího prvku specifické pro platformu bude dosáhnout požadovaného výsledku.
- Vlastní zobrazovací jednotky je potřeba při je zapotřebí k přepsání metody ovládacího prvku specifické pro platformu.
- Vlastní zobrazovací jednotky je vyžadován, pokud je potřeba nahradit specifické pro platformu ovládací prvek, který implementuje ovládacího prvku Xamarin.Forms.

## <a name="subclassing-the-platformeffect-class"></a>Vytvoření podtřídy PlatformEffect – třída

Následující tabulka uvádí obor názvů pro `PlatformEffect` třídy pro každou platformu a typy jeho vlastnosti:

<table>
 <thead>
   <tr>
     <td><strong>Platforma</strong></td>
     <td><strong>Namespace</strong></td>
     <td><strong>kontejner</strong></td>
     <td><strong>Ovládací prvek</strong></td>
   </tr>
 </thead>
 <tbody>
   <tr>
     <td>iOS</a></td>
     <td>Xamarin.Forms.Platform.iOS</td>
     <td>UIView</td>
     <td>UIView</td>
   </tr>
   <tr>
     <td>Android</a></td>
     <td>Xamarin.Forms.Platform.Android</td>
     <td>Skupinu ViewGroup</td>
     <td>Zobrazit</td>
   </tr>
   <tr>
     <td>Windows Phone 8,1</a></td>
     <td>Xamarin.Forms.Platform.WinRT</td>
     <td>FrameworkElement</td>
     <td>FrameworkElement</td>
   </tr>
   <tr>
     <td>Univerzální platforma Windows (UPW)</a></td>
     <td>Xamarin.Forms.Platform.UWP</td>
     <td>FrameworkElement</td>
     <td>FrameworkElement</td>
   </tr>
 </tbody>
</table>

Každý specifické pro platformu `PlatformEffect` třída zpřístupňuje následující vlastnosti:

- `Container` – ovládací prvek specifických pro platformy se použije k implementaci rozložení odkazuje.
- `Control` – ovládací prvek specifických pro platformy se použije k implementaci ovládacího prvku Xamarin.Forms odkazuje.
- `Element` – odkazuje na platformě Xamarin.Forms ovládací prvek, který je vykreslované.

Účinky nemají typ informace o kontejneru, řízení nebo elementu, které jsou připojené k, protože může být připojené k libovolný element. Proto když efekt je připojen k element, který nepodporuje ho by měl bez nebo výjimku. Ale `Container`, `Control`, a `Element` vlastnosti lze převést na jejich implementující typů. Pro další informace o těchto typů najdete [zobrazovací jednotky základní třídy a nativní ovládací prvky](~/xamarin-forms/app-fundamentals/custom-renderer/renderers.md).

Každý specifické pro platformu `PlatformEffect` třída poskytuje následující metody, které musí být potlačena za účelem implementace vliv:

- [`OnAttached`](https://developer.xamarin.com/api/member/Xamarin.Forms.Effect.OnAttached()/) – voláno, pokud je připojen vliv do ovládacího prvku Xamarin.Forms. Přepsané verzi tuto metodu v třídě každý čip konkrétní vliv je místo, kde provádět přizpůsobení ovládacího prvku, společně s zpracování výjimek v případě, že účinek nelze použít pro daný ovládací prvek Xamarin.Forms.
- [`OnDetached`](https://developer.xamarin.com/api/member/Xamarin.Forms.Effect.OnDetached()/) – voláno, pokud vliv odpojený od ovládacího prvku Xamarin.Forms. Přepsané verzi této metody v každé třídě vliv specifických pro platformy je místo, kde vyčistit všechny vliv například zrušte registraci obslužné rutiny události.

Kromě toho `PlatformEffect` zpřístupní [ `OnElementPropertyChanged` ](https://developer.xamarin.com/api/member/Xamarin.Forms.PlatformEffect%3CTContainer,TControl%3E.OnElementPropertyChanged/p/System.ComponentModel.PropertyChangedEventArgs/) metodu, která je také možné přepsat. Tato metoda je volána, když došlo ke změně vlastností elementu. Přepsané verzi této metody v každé třídě vliv specifických pro platformy je na místě reagovat na změny vazbu vlastnosti na platformě Xamarin.Forms ovládací prvek. Kontrolu pro vlastnost, která se změnila měli vždy provedena, protože toto přepsání lze volat vícekrát.


## <a name="related-links"></a>Související odkazy

- [Vlastní renderery](~/xamarin-forms/app-fundamentals/custom-renderer/index.md)
