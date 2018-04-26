---
title: Práce s indikátory průběhu
description: Tento článek se zabývá navrhování a práce s indikátory průběhu uvnitř Xamarin.tvOS aplikace.
ms.prod: xamarin
ms.assetid: 582B6D0C-1F16-4299-A9A6-5651E76009FE
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 04/25/2018
ms.openlocfilehash: d512dfddb3a6c81767f937272a4ffb1ab1a35372
ms.sourcegitcommit: dc882e9631b4ed52596b944a6fbbdde309346943
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/26/2018
---
# <a name="working-with-progress-indicators"></a>Práce s indikátory průběhu

_Tento článek se zabývá navrhování a práce s indikátory průběhu uvnitř Xamarin.tvOS aplikace._

Mohou nastat situace, když vaše aplikace Xamarin.tvOS musí načíst nový obsah nebo provést operaci zdlouhavé zpracování. Během této doby by měla představovat indikátorem aktivity nebo indikátor průběhu, aby mohl uživatel vědět, že aplikace je stále spuštěná a dát jim některé údaj o délce úloha spuštěná.

![Ukázkové indikátory průběhu](progress-indicators-images/intro01.png "ukázkové indikátory průběhu")

## <a name="about-activity-indicators"></a>O indikátory aktivity

Slouží jako ukazatel aktivity uvede jako roztočený ikonu a se používá k reprezentování úlohu neurčená délky. Ukazatele se zobrazí, když úloha spustí a po dokončení úlohy zmizí.

Společnost Apple má následující návrhy pro práci s indikátory aktivity:

- **Pokud je to možné, použijte indikátory průběhu místo** – vzhledem poskytuje indikátor aktivity uživatele žádné zpětnou vazbu, jak dlouhé proces spuštěn bude trvat, vždy používají indikátor průběhu, pokud je délka znám (například kolik bajtů ke stažení v souboru).
- **Zachovat indikátoru animovaný** -uživatelé týkají ukazatel stojící aktivity zablokované aplikace, takže by měla vždy animace indikátoru, když se zobrazí.
- **Popis úloh zpracovávaných** -právě zobrazení ukazatel aktivity sám o sobě nestačí, uživatel musí být informováni o proces, na kterém jsou čekání. Zahrnout smysluplný popisek (obvykle jednu, kompletní věta), který jasně definuje úlohu.

## <a name="about-progress-bars"></a>O indikátory průběhu

Indikátor průběhu uvede jako řádek, který vyplní barvou k označení stavu a délka časově náročný úkol. Indikátory průběhu by měl být použit při délka úlohy je známý nebo je můžete vypočítat.

Společnost Apple má následující návrhy pro práci s indikátory průběhu:

- **Přesně hlásit průběh** -indikátory průběhu měli vždy k dispozici ve správné podobě čas potřebný k dokončení úlohy. Nikdy poskytovat zavádějící informace o době k vytvoření aplikace zobrazí zaneprázdněný.
- **Použití pro dobře definované doby trvání** -umístit průběh řádky nesmí jenom zobrazit, že náročná úloha trvá, ale poskytnout uživatele a údaj o kolik úlohy dokončení a odhad zbývající dobu.

## <a name="progress-indicators-and-storyboards"></a>Indikátory průběhu a scénářů

Nejjednodušší způsob, jak pracovat s indikátor průběhu v aplikaci Xamarin.tvOS je přidat do aplikace uživatelského rozhraní pomocí návrháře iOS.

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)
    
1. V **řešení Pad**, dvakrát klikněte **Main.storyboard** souborů a otevřete pro úpravy.

2. Přetáhněte **ukazatel aktivity** z **sada nástrojů** na zobrazení: 

    ![Slouží jako ukazatel aktivity](progress-indicators-images/activity01.png "indikátorem aktivity")

3. V **pomůcky** kartě **vlastnosti Pad**, můžete upravit několik vlastností ukazatel aktivity, jako jeho **styl**, **chování**, a **název**: 

    ![Kartě pomůcky pro indikátorem aktivity](progress-indicators-images/activity02.png "The pomůcky karta slouží jako ukazatel aktivity")
    
    **Název** Určuje název vlastnosti, která představuje ukazatel aktivity v kódu jazyka C#.

4. Přetáhněte **zobrazení průběhu** z **sada nástrojů** na zobrazení: 

    ![Zobrazení průběhu](progress-indicators-images/activity03.png "zobrazení průběhu")

5. V **pomůcky** kartě **vlastnost Explorer**, můžete upravit několik vlastností zobrazení průběhu jeho **styl**, **průběh**(dokončeno), a **název**: 

    ![Na kartě pomůcky pro zobrazení průběhu](progress-indicators-images/activity04.png "kartu The pomůcky pro zobrazení průběhu")
    
    **Název** Určuje název vlastnosti, který představuje zobrazení průběhu v kódu jazyka C#.

6. Uložte provedené změny.

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)
    
1. V **Průzkumníku řešení**, dvakrát klikněte **Main.storyboard** souborů a otevřete pro úpravy.

2. Přetáhněte **ukazatel aktivity** z **sada nástrojů** na zobrazení: 

    ![Slouží jako ukazatel aktivity](progress-indicators-images/activity01-vs.png
    "indikátorem aktivity")

3. V **pomůcky** kartě **Explorer vlastnosti**, můžete upravit několik vlastností ukazatel aktivity, jako jeho **styl**, **chování**, a **název**: 

    ![Kartě pomůcky pro indikátorem aktivity](progress-indicators-images/activity02-vs.png "The pomůcky karta slouží jako ukazatel aktivity")

    **Název** Určuje název vlastnosti, která představuje ukazatel aktivity v kódu jazyka C#.

4. Přetáhněte **zobrazení průběhu** z **sada nástrojů** na zobrazení: 

   ![Zobrazení průběhu](progress-indicators-images/activity03-vs.png "zobrazení průběhu")

5. V **pomůcky** kartě **vlastnost Explorer**, můžete upravit několik vlastností zobrazení průběhu jeho **styl**, **průběh**(dokončeno), a **název**: 

    ![Na kartě pomůcky pro zobrazení průběhu](progress-indicators-images/activity04-vs.png "kartu The pomůcky pro zobrazení průběhu")
    
    **Název** Určuje název vlastnosti, který představuje zobrazení průběhu v kódu jazyka C#.

6. Uložte provedené změny.

-----

Další informace o práci s scénářů, najdete v tématu naše [Hello, tvOS úvodní příručce](~/ios/tvos/get-started/hello-tvos.md). 

## <a name="working-with-activity-indicators"></a>Práce s indikátory aktivity

Jak jsme uvedli výše, pokud vaše aplikace běží dlouho proces neurčitém délka se mají indikátory aktivity.

Kdykoli, uvidíte, pokud je indikátor aktivity animace kontrolou jeho `IsAnimating` vlastnost. Pokud `HidesWhenStopped` vlastnost je `true`, ukazatel aktivity bude skrytá automaticky při zastavení jeho animace.

Můžete použít následující kód pro spuštění animace: 

```csharp
ActivityIndicator.StartAnimating();
```

A následující zastaví animace:

```csharp
ActivityIndicator.StopAnimating();
```

> [!NOTE]
> Tyto fragmenty kódu předpokládat, že ukazatel aktivity **název** byla nastavena na **ActivityIndicator** v **pomůcky** kartě IOS Designer.

## <a name="working-with-progress-bars"></a>Práce s indikátory průběhu

Indikátor průběhu znovu, třeba použít vždy, když vaše aplikace je prováděna dlouho spuštěná úloha známé hodnotě DURATION. 

`Progress` Vlastnost se používá k nastavení velikosti úloha, která byla dokončena z 0 % na 100 % (od 0,0 do 1,0). Použití `ProgressTintColor` vlastnost pro nastavení barvy panelu velikost dokončit a `TrackTintColor` vlastnost nastavující barvu pozadí (nedokončené velikost).

## <a name="summary"></a>Souhrn

Tento článek má zahrnutých navrhování a práce s indikátory průběhu uvnitř Xamarin.tvOS aplikace.

## <a name="related-links"></a>Související odkazy

- [Ukázky pro tvOS](https://developer.xamarin.com/samples/tvos/all/)
- [tvOS](https://developer.apple.com/tvos/)
- [tvOS lidské rozhraní příručky](https://developer.apple.com/tvos/human-interface-guidelines/)
- [Průvodce programováním aplikace pro tvOS](https://developer.apple.com/library/prerelease/tvos/documentation/General/Conceptual/AppleTV_PG/)
