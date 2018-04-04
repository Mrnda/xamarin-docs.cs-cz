---
title: Práce s indikátory průběhu
description: Tento článek se zabývá navrhování a práce s indikátory průběhu uvnitř Xamarin.tvOS aplikace.
ms.prod: xamarin
ms.assetid: 582B6D0C-1F16-4299-A9A6-5651E76009FE
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/16/2017
ms.openlocfilehash: 96fc3ea0aa802f62bd697b34f7bd504eb445a4f6
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/04/2018
---
# <a name="working-with-progress-indicators"></a>Práce s indikátory průběhu

_Tento článek se zabývá navrhování a práce s indikátory průběhu uvnitř Xamarin.tvOS aplikace._


Mohou nastat situace, když vaše aplikace Xamarin.tvOS musí načíst nový obsah nebo provést operaci zdlouhavé zpracování. Během této doby by měla představovat buď ukazatel aktivity nebo indikátor průběhu aby mohl uživatel vědět, že aplikace je stále spuštěná a dát jim některé údaj o délce úloha spuštěná.

[![](progress-indicators-images/intro01.png "Ukázka indikátory průběhu")](progress-indicators-images/intro01.png#lightbox)

<a name="About-Activity-Indicators" />

## <a name="about-activity-indicators"></a>O indikátory aktivity

Slouží jako ukazatel aktivity uvede jako roztočený ikonu vizuálně a se používá k reprezentování úlohu neurčená délky. Ukazatele se zobrazí, když úloha spustí a po dokončení úlohy zmizí.

Společnost Apple má následující návrhy pro práci s indikátory aktivity:

- **Kdykoli je to možné, použijte místo řádky průběh** – protože slouží jako ukazatel aktivity umožňuje uživateli žádné zpětnou vazbu, jak dlouho bude trvat proces spuštěn, vždy použijte indikátor průběhu, pokud je délka vědět (například kolik bajtů ke stažení v souboru).
- **Zachovat animovaný ukazatel** -uživatelé zablokované aplikace se týkají stojící ukazatel aktivity, byste měli mít vždy indikátoru animovaný při se zobrazily.
- **Popis úloh zpracovávaných** -právě zobrazení ukazatel aktivity sám o sobě není dost, uživatel musí být informováni o proces, že čekají na. Zahrnout smysluplný popisek (obvykle jednu, kompletní věta), který jasně definuje úlohu.

<a name="Summary" />

## <a name="about-progress-bars"></a>O indikátory průběhu

Indikátor průběhu uvede jako řádek, který vyplní barvou k označení stavu a délka časově náročný úkol. Indikátory průběhu by měl být použit při délka úlohy je vědět, nebo můžete vypočítat.

Společnost Apple má následující návrhy pro práci s indikátory průběhu:

- **Přesně sestavy průběhu** -indikátory průběhu by měla být vždy přesné reprezentace čas potřebný k dokončení úlohy. Nikdy poskytovat zavádějící informace o době k vytvoření aplikace zobrazí zaneprázdněný.
- **Použití dob trvání Well-Defined** -indikátor průběhu nesmí jenom zobrazit, že náročná úloha trvá umístit, ale poskytnout uživatele a údaj o kolik úlohy dokončení a odhad zbývající dobu.

<a name="Progress-Indicators-and-Storyboards" />

## <a name="progress-indicators-and-storyboards"></a>Indikátory průběhu a scénářů

Nejjednodušší způsob, jak pracovat s ukazatelem průběhu v aplikaci Xamarin.tvOS je chcete přidat do aplikace uživatelského rozhraní pomocí návrháře iOS.

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)
    
1. V **řešení Pad**, dvakrát klikněte `Main.storyboard` souborů a otevřete pro úpravy.
1. Přetáhněte **ukazatel aktivity** z **sada nástrojů** na zobrazení: 

    [![](progress-indicators-images/activity01.png "Ukazatel aktivity")](progress-indicators-images/activity01.png#lightbox)
1. V **pomůcky karta** z **vlastnosti Pad**, můžete upravit několik vlastností ukazatel aktivity, jako jeho **styl** a **chování**: 

    [![](progress-indicators-images/activity02.png "Na kartě pomůcky ")](progress-indicators-images/activity02.png#lightbox)
1. Přetáhněte **zobrazení průběhu** z **sada nástrojů** na zobrazení: 

    [![](progress-indicators-images/activity03.png "Zobrazení průběhu")](progress-indicators-images/activity03.png#lightbox)
1. V **pomůcky karta** z **vlastnost Explorer**, můžete upravit několik vlastností zobrazení průběhu jeho **styl** a **průběh**(dokončeno): 

    [![](progress-indicators-images/activity04.png "Na kartě pomůcky")](progress-indicators-images/activity04.png#lightbox)
1. Nakonec přiřadit **názvy** pro ovládací prvky, aby mohli odpovídat na ně v kódu jazyka C#. Příklad: 

    [![](progress-indicators-images/activity05.png "Přiřadit název")](progress-indicators-images/activity05.png#lightbox)
1. Uložte provedené změny.

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)
    
1. V **Průzkumníku řešení**, dvakrát klikněte `Main.storyboard` souborů a otevřete pro úpravy.
1. Přetáhněte **ukazatel aktivity** z **sada nástrojů** na zobrazení: 

    [![](progress-indicators-images/activity01-vs.png "Ukazatel aktivity")](progress-indicators-images/activity01-vs.png#lightbox)
1. V **pomůcky karta** z **Explorer vlastnosti**, můžete upravit několik vlastností ukazatel aktivity, jako jeho **styl** a **chování**: 

    [![](progress-indicators-images/activity02-vs.png "Na kartě pomůcky")](progress-indicators-images/activity02-vs.png#lightbox)
1. Přetáhněte **zobrazení průběhu** z **sada nástrojů** na zobrazení: 

    [![](progress-indicators-images/activity03-vs.png "Zobrazení průběhu")](progress-indicators-images/activity03-vs.png#lightbox)
1. V **pomůcky karta** z **vlastnost Explorer**, můžete upravit několik vlastností zobrazení průběhu jeho **styl** a **průběh**(dokončeno): 

    [![](progress-indicators-images/activity04-vs.png "Na kartě pomůcky")](progress-indicators-images/activity04-vs.png#lightbox)
1. Nakonec přiřadit **názvy** pro ovládací prvky, aby mohli odpovídat na ně v kódu jazyka C#. Příklad: 

    [![](progress-indicators-images/activity05-vs.png "Přiřadit název")](progress-indicators-images/activity05-vs.png#lightbox)
1. Uložte provedené změny.

-----

Další informace o práci s scénářů, najdete v tématu naše [Hello, tvOS úvodní příručce](~/ios/tvos/get-started/hello-tvos.md). 

<a name="Working-with-Activity-Indicators" />

## <a name="working-with-activity-indicators"></a>Práce s indikátory aktivity

Jak jsme uvedli výše, pokud vaše aplikace běží dlouho procesu, ale neznáte přesnou délku doba, kterou bude vyžadovat úlohy se mají indikátory aktivity.

Kdykoli se zobrazí, pokud se ukazatel aktivity běží jeho animace roztočený kontrolou `IsAnimating` vlastnost. Pokud `HidesWhenStopped` vlastnost je `true`, ukazatel aktivity bude skrytá automaticky při zastavení jeho animace.

Můžete použít následující kód pro spuštění animace: 

```csharp
ActivityIndicator.StartAnimating();
```

A následující zastaví animace:

```csharp
ActivityIndicator.StopAnimating();
```

<a name="Working-with-Progress-Bars" />

## <a name="working-with-progress-bars"></a>Práce s indikátory průběhu

Indikátor průběhu, třeba použít vždy, když vaše aplikace je prováděna dlouhotrvající úkol dobu trvání Přehled. 

`Progress` Vlastnost se používá k nastavení velikosti úloha, která byla dokončena z 0 % na 100 % (od 0,0 do 1,0). Použití `ProgressTintColor` vlastnost pro nastavení barvy panelu velikost dokončit a `TrackTintColor` vlastnost nastavující barvu pozadí (nedokončené velikost).

<a name="Summary" />

## <a name="summary"></a>Souhrn

Tento článek má zahrnutých navrhování a práce s indikátory průběhu uvnitř Xamarin.tvOS aplikace.



## <a name="related-links"></a>Související odkazy

- [Ukázky pro tvOS](https://developer.xamarin.com/samples/tvos/all/)
- [tvOS](https://developer.apple.com/tvos/)
- [tvOS lidské rozhraní příručky](https://developer.apple.com/tvos/human-interface-guidelines/)
- [Průvodce programováním aplikace pro tvOS](https://developer.apple.com/library/prerelease/tvos/documentation/General/Conceptual/AppleTV_PG/)
