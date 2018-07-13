---
title: Úvod do efekty
description: Účinky Povolit nativní ovládací prvky na jednotlivých platformách přizpůsobit a jsou obvykle používány pro používání stylů pro malé změny. Tento článek obsahuje úvod do efekty, popisuje hranici mezi efekty a vlastní renderery a popisuje třídy PlatformEffect.
ms.prod: xamarin
ms.assetid: 30CB8615-8F39-4762-BDB7-333D2B57D112
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 03/08/2016
ms.openlocfilehash: d3fa958e999a10832d5fa15e4190077955b0e6df
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/12/2018
ms.locfileid: "38997374"
---
# <a name="introduction-to-effects"></a>Úvod do efekty

_Účinky Povolit nativní ovládací prvky na jednotlivých platformách přizpůsobit a jsou obvykle používány pro používání stylů pro malé změny. Tento článek obsahuje úvod do efekty, popisuje hranici mezi efekty a vlastní renderery a popisuje třídy PlatformEffect._

Xamarin.Forms [rozložení stránky a ovládací prvky](~/xamarin-forms/user-interface/controls/index.md) představuje společné rozhraní API popsat mobilních multiplatformních uživatelských rozhraní. Jednotlivé stránky, rozložení a ovládací prvek je vykreslen jinak na každé platformě pomocí `Renderer` třídu, která se pak vytvoří ovládací prvek nativní (odpovídající reprezentaci Xamarin.Forms), uspořádá na obrazovce a přidá chování určené ve sdílené kód.

Vývojáři můžou implementovat vlastní vlastní `Renderer` třídy k přizpůsobení vzhledu a chování ovládacího prvku. Implementující třída vlastního rendereru provádět přizpůsobení jednoduchých ovládacího prvku je však často odpověď náročné. Účinky zjednodušení tohoto procesu, což nativní ovládací prvky na jednotlivých platformách snadno přizpůsobit.

Efekty jsou vytvářeny v projektech pro konkrétní platformu vytváření podtříd `PlatformEffect` ovládací prvek a potom účinky se spotřebovávají připojení na odpovídající ovládací prvek v Xamarin.Forms .NET Standard knihovny nebo sdílené knihovny projektu.

## <a name="why-use-an-effect-over-a-custom-renderer"></a>Proč používat efektu přes vlastní zobrazovací jednotky?

Efekty zjednodušit vlastní nastavení ovládacího prvku, jsou opakovaně použitelné a může být parametrizován abyste dále zvýšili opakované použití.

Cokoli, co lze nastavit pomocí efektu lze také nastavit pomocí vlastní zobrazovací jednotky. Vlastní renderery ale nabízí větší flexibilitu a možnosti přizpůsobení než účinky. Podle následujících pokynů zobrazte seznam okolnosti, ve kterých si dá vybírat efektu přes vlastní zobrazovací jednotky:

- Efekt se doporučuje, pokud změna vlastností ovládacího prvku specifické pro platformu bude docílili požadovaného výsledku.
- Vlastní zobrazovací jednotky se vyžaduje, když je potřeba přepsat metody specifické pro platformu ovládacího prvku.
- Vyžaduje se vlastní zobrazovací jednotky, když je potřeba nahradit specifické pro platformu ovládací prvek, který implementuje ovládací prvek Xamarin.Forms.

## <a name="subclassing-the-platformeffect-class"></a>Vytvoření podtřídy třídy PlatformEffect

V následující tabulce jsou uvedeny obor názvů pro `PlatformEffect` třídy na každou platformu a typů vlastností:

|Platforma|Obor názvů|Kontejner|Ovládací prvek|
|--- |--- |--- |--- |
|iOS|Xamarin.Forms.Platform.iOS|UIView|UIView|
|Android|Xamarin.Forms.Platform.Android|Skupinu ViewGroup|Zobrazit|
|Univerzální platforma Windows (UPW)|Xamarin.Forms.Platform.UWP|FrameworkElement|FrameworkElement|

Každý specifické pro platformu `PlatformEffect` třída zveřejňuje následující vlastnosti:

- `Container` – odkazuje na konkrétní platformu ovládací prvek se použije k implementaci rozložení.
- `Control` – odkazuje na ovládací prvek specifické pro platformu používaný k implementaci ovládacího prvku Xamarin.Forms.
- `Element` – odkazuje na ovládací prvek, vykreslované Xamarin.Forms.

Účinky nemají typ informace o kontejneru, ovládacího prvku nebo prvku, které jsou připojeny k vzhledem k tomu je možné připojit k libovolnému prvku. Proto případě efektu na prvek, který nepodporuje ji by měl řádná degradace nebo vyvolat výjimku. Ale `Container`, `Control`, a `Element` vlastnosti lze převést na jejich implementujícího typu. Pro další informace o těchto typů viz [Renderer základní třídy a nativní ovládací prvky](~/xamarin-forms/app-fundamentals/custom-renderer/renderers.md).

Každý specifické pro platformu `PlatformEffect` třída zveřejňuje následující metody, které musí být potlačena za účelem implementace efektu:

- [`OnAttached`](xref:Xamarin.Forms.Effect.OnAttached) – volána, když efekt je připojen k ovládacímu prvku Xamarin.Forms. Přepsané verzi této metody v každé třídě specifické pro platformy efekt je místem, kde můžete provádět přizpůsobení ovládacího prvku, spolu s zpracování výjimek v případě, že efekt nelze použít pro zadaný ovládací prvek Xamarin.Forms.
- [`OnDetached`](xref:Xamarin.Forms.Effect.OnDetached) – volána, když server efektu byl odpojen z ovládacího prvku Xamarin.Forms. Přepsané verzi této metody v každé třídě specifické pro platformu efekt je místem, kde můžete vyčistěte efekt například zrušení registrace obslužné rutiny události.

Kromě toho `PlatformEffect` zpřístupňuje [ `OnElementPropertyChanged` ](xref:Xamarin.Forms.PlatformEffect`2.OnElementPropertyChanged(System.ComponentModel.PropertyChangedEventArgs)) metodu, která lze také přepsat. Tato metoda je volána, když je změněna vlastnost elementu. Přepsané verzi této metody v každé třídě specifické pro platformu efekt je místem, kde můžete reagovat na změny umožňujících vazbu vlastnosti na ovládacím prvku Xamarin.Forms. Kontrola změněná vlastnost by měla vždy provedena, protože toto přepsání je možné vyvolat v mnoha případech.


## <a name="related-links"></a>Související odkazy

- [Vlastní renderery](~/xamarin-forms/app-fundamentals/custom-renderer/index.md)
