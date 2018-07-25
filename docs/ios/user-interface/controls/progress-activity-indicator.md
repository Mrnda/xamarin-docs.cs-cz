---
title: Indikátory průběhu a aktivit v Xamarin.iosu
description: Tento dokument popisuje způsob použití indikátory průběhu a aktivit v Xamarin.iOS. Popisuje jejich použití, jak prostřednictvím kódu programu a scénáře.
ms.prod: xamarin
ms.assetid: 7AA887E4-51F7-4867-82C5-A8D2EA48AE07
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 07/11/2017
ms.openlocfilehash: 24ad47bd37693eccf38033159acef72a9d4942be
ms.sourcegitcommit: b56b3f906d2c05a3f1be219ef41be8b79e519b8e
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/25/2018
ms.locfileid: "39242176"
---
# <a name="progress-and-activity-indicators-in-xamarinios"></a>Indikátory průběhu a aktivit v Xamarin.iosu

Je pravděpodobné, že vaše aplikace bude mít provádět dlouhodobě spuštěných úloh, jako je například načítání nebo zpracování dat a že toto zpoždění může způsobit zpoždění při aktualizaci uživatelského rozhraní. Během této doby vždy používejte indikátor průběhu jistotu uživatele, že systém je zaneprázdněn prováděním práce. Díky tomu uživatelský ovládací prvek, který aplikace pracuje na své žádosti, který nečeká na svůj vstup a může poskytnout způsob s podrobnostmi o mají přesně jak dlouho čekat.

poskytuje dva hlavní způsoby, zadejte tento údaj průběhu v aplikaci pro iOS: aktivita ukazatele (včetně konkrétní _sítě_ indikátor aktivity) a indikátory průběhu.

## <a name="activity-indicator"></a>Indikátor aktivity

Aktivita indikátory by měl zobrazený v případě, že vaše aplikace běží dlouho procesu, ale nevíte přesně délka dobu, po kterou bude vyžadovat úkolu.

Apple má následující návrhy pro práci s indikátory aktivity:

- **Kdykoli je to možné, použijte průběh pruhy** – protože indikátorem aktivity umožňuje uživateli žádná zpětná vazba, jak dlouho bude trvat proces spuštěn, vždy používejte indikátor průběhu, pokud je délka znáte (například kolik bajtů se stáhnout do souboru).
- **Zachovat animovaný ukazatel** -uživatelů týkají ukazatel setrvá aktivity pozastavily aplikace tak, indikátor animovat, zatímco se zobrazily byste měli mít vždy.
- **Popsat úkol zpracovává** – jenom zobrazení indikátorem aktivity sám o sobě není k dispozici dostatek, uživatel musí být informována o čekání na proces. Zahrnout smysluplný popisek (obvykle jednu, kompletní věta), který jasně definuje úlohy.

### <a name="implementing-an-activity-indicator"></a>Implementace indikátorem aktivity

Indikátor aktivity se implementuje pomocí [ `UIActivityIndictorView` ](https://developer.xamarin.com/api/type/UIKit.UIActivityIndicatorView/) třída, která označuje, že `UIActivity` probíhat.

### <a name="activity-indicators-and-storyboards"></a>Aktivita ukazatele a scénářů

Používáte-li vytvořit uživatelské rozhraní v iOS designeru, indikátor aktivity můžete přidat do rozložení z panelu nástrojů. Z panelu Vlastnosti je možné upravit následující vlastnosti:

![Panel Vlastnosti](progress-activity-indicator-images/progress-indicator1.png)

### <a name="managing-activity-indicator-behavior"></a>Správa chování indikátor aktivity

Použití `StartAnimating()` a `StopAnimating()` metod ke spuštění a zastavení animaci indikátor činnosti.

Nastavte `HidesWhenStopped` vlastnost `true` aby indikátor aktivity mohou zmizet po `StopAnimating()` byla volána. Je nastavené na `true` ve výchozím nastavení. Kdykoli můžete zobrazit, pokud indikátor aktivity běží tak, že zkontrolujete jeho pokryjte animace `IsAnimating` vlastnost. 


### <a name="managing-activity-indicator-appearances"></a>Správa aktivit vzhled ukazatele

`UIActivityIndicatorViewStyle` Výčtu lze předat jako parametr při vytváření instance indikátorem aktivity. Můžete nastavit vizuální styl `Gray`, `White`, nebo `WhiteLarge`, například:

```csharp
activitySpinner = new UIActivityIndicatorView(UIActivityIndicatorViewStyle.WhiteLarge);
```

Můžete přepsat barvu poskytované `UIActivityIndicatorViewStyle` nastavením `Color` vlastnost.

## <a name="progress-bar"></a>Indikátor průběhu

Indikátor průběhu prezentuje jako řádek, který vyplní barvou pro označení toho, stavu a délka časově náročný úkol. Indikátory průběhu by měl být používá vždy, když délka úlohy je vědět, nebo mohou být vypočítány.

Apple má následující návrhy pro práci s indikátory průběhu:

- **Přesné informace o průběhu** -indikátory průběhu by vždycky měla být přesnou reprezentací čas potřebný k dokončení úkolu. Nikdy poskytovat zavádějící informace o čas, aby aplikace zobrazí zaneprázdněn.
- **Použití pro dobu trvání Well-Defined** – indikátor průběhu by neměl jenom zobrazit, že trvá dlouhou úlohu umístit, ale poskytne uživatele a údaj o jaká úloha je dokončena a odhad zbývající dobu.

### <a name="implementing-an-progress-bar"></a>Implementace indikátor průběhu

Indikátor průběhu proběhne po vytvoření instance [`UIProgressView`](https://developer.xamarin.com/api/type/UIKit.UIProgressView/)

### <a name="progress-bars-and-storyboards"></a>Indikátory průběhu a scénářů

Indikátor průběhu můžete také přidat do vašeho uživatelského rozhraní při použití v iOS designeru. Vyhledejte **zobrazení průběhu** v **nástrojů** a přetáhněte jej do zobrazení.

Na panelu Vlastnosti je možné upravit následující vlastnosti:

![Panel Vlastnosti](progress-activity-indicator-images/progress-indicator3.png)


### <a name="managing-progress-bar-behavior"></a>Správa chování indikátor průběhu

Průběh na panelu můžete nejdřív nastavit pomocí `Progress` vlastnost:

```csharp
ProgressBar.Progress = 0f;
```

Průběh můžete upravit pomocí `SetProgress` metoda a předáním logickou hodnotu deklarace, pokud chcete změnit animovat, nebo ne.

```csharp
ProgressBar.SetProgress(1.0f, true);
```

Další informace o použití indikátor průběhu najdete [vykazování průběhu](https://github.com/xamarin/recipes/tree/master/Recipes/cross-platform/networking/download_progress) předpisu a [ukázky pro tvOS UICatalog](https://developer.xamarin.com/samples/monotouch/tvos/UICatalog/).

### <a name="managing-progress-bar-appearance"></a>Správa vzhled indikátor průběhu

Podobně jako indikátor činnosti `UIProgressViewStyle` lze předat výčet jako parametr při vytváření instance indikátor průběhu.

Průběh a sledovat Image a barevný nádech lze upravit pomocí následujících vlastností:

```csharp
progressBar = new UIProgressView(UIProgressViewStyle.Default)
            {
                ProgressImage = UIImage.FromBundle("TrackImage"),
                ProgressTintColor = UIColor.Cyan,
                TrackImage = UIImage.FromBundle("TrackImage"),
                TrackTintColor = UIColor.Magenta
            }; 
```



