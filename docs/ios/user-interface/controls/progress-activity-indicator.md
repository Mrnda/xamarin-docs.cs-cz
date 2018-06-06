---
title: Průběh a aktivity indikátory v Xamarin.iOS
description: Tento dokument popisuje, jak používat indikátory průběhu a aktivity v Xamarin.iOS. Popisuje jejich použití prostřednictvím kódu programu i s scénáře.
ms.prod: xamarin
ms.assetid: 7AA887E4-51F7-4867-82C5-A8D2EA48AE07
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 07/11/2017
ms.openlocfilehash: 27ee788ec40bfd158dbc0d9926245b166e2954a9
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 06/05/2018
ms.locfileid: "34790052"
---
# <a name="progress-and-activity-indicators-in-xamarinios"></a>Průběh a aktivity indikátory v Xamarin.iOS

Je pravděpodobné, že vaše aplikace bude mít k provádění dlouho spuštěných úloh, jako je například načítání nebo zpracování dat a aby toto zpoždění může způsobit zpoždění při aktualizaci vašeho uživatelského rozhraní. Během této doby byste měli vždycky používat indikátor průběhu uživatel ujistit, že systém je zaneprázdněn prováděním pracovní. Díky tomu uživatelského ovládacího prvku, zda aplikace pracuje na jejich požadavek, který není čekání na jejich zadání a způsob s podrobnostmi o přesně jak dlouho budou muset počkat, než může poskytnout.

iOS poskytuje dva hlavní způsoby, jak poskytnout tento údaj průběh ve vaší aplikaci: indikátory aktivitu (včetně konkrétní _sítě_ ukazatel aktivity) a indikátory průběhu.

## <a name="activity-indicator"></a>Ukazatel aktivity

Když aplikace běží dlouho procesu, ale neznáte přesnou délku doba, kterou bude vyžadovat úlohy se mají indikátory aktivity.

Společnost Apple má následující návrhy pro práci s indikátory aktivity:

- **Kdykoli je to možné, použijte místo řádky průběh** – protože slouží jako ukazatel aktivity umožňuje uživateli žádné zpětnou vazbu, jak dlouho bude trvat proces spuštěn, vždy použijte indikátor průběhu, pokud je délka vědět (například kolik bajtů ke stažení v souboru).
- **Zachovat animovaný ukazatel** -uživatelé zablokované aplikace se týkají stojící ukazatel aktivity, byste měli mít vždy indikátoru animovaný při se zobrazily.
- **Popis úloh zpracovávaných** -právě zobrazení ukazatel aktivity sám o sobě není dost, uživatel musí být informováni o proces, že čekají na. Zahrnout smysluplný popisek (obvykle jednu, kompletní věta), který jasně definuje úlohu.

### <a name="implementing-an-activity-indicator"></a>Implementace indikátorem aktivity

Slouží jako ukazatel aktivity se implementuje pomocí [ `UIActivityIndictorView` ](https://developer.xamarin.com/api/type/UIKit.UIActivityIndicatorView/) třída, která označuje, že `UIActivity` probíhá.

### <a name="activity-indicators-and-storyboards"></a>Indikátory aktivity a scénářů

IOS návrháře používají k vytvoření vašeho uživatelského rozhraní, ukazatel aktivity mohou být přidány do vaší rozložení z panelu nástrojů. Z panelu pro vlastnosti lze upravit následující vlastnosti:

![Vlastnosti odsazení](progress-activity-indicator-images/progress-indicator1.png)

### <a name="managing-activity-indicator-behavior"></a>Správa chování aktivity indikátoru

Použití `StartAnimating()` a `StopAnimating()` metody spuštění a ukončení animace indikátorem aktivity.

Nastavte `HidesWhenStopped` vlastnost `true` aby ukazatel aktivity mohou zmizet po `StopAnimating()` byla volána. To je nastaven na `true` ve výchozím nastavení. Kdykoli se zobrazí, pokud se ukazatel aktivity běží jeho animace roztočený kontrolou `IsAnimating` vlastnost. 


### <a name="managing-activity-indicator-appearances"></a>Správa aktivity vzhled ukazatele

`UIActivityIndicatorViewStyle` Výčet může být předána jako parametr při vytváření instance ukazatel aktivity. To můžete nastavit vizuální styl `Gray`, `White`, nebo `WhiteLarge`, například:

```csharp
activitySpinner = new UIActivityIndicatorView(UIActivityIndicatorViewStyle.WhiteLarge);
```

Můžete přepsat barvu poskytované `UIActivityIndicatorViewStyle` nastavením `Color` vlastnost.

## <a name="progress-bar"></a>Indikátor průběhu

Indikátor průběhu uvede jako řádek, který vyplní barvou k označení stavu a délka časově náročný úkol. Indikátory průběhu by měl být použit při délka úlohy je vědět, nebo můžete vypočítat.

Společnost Apple má následující návrhy pro práci s indikátory průběhu:

- **Přesně sestavy průběhu** -indikátory průběhu by měla být vždy přesné reprezentace čas potřebný k dokončení úlohy. Nikdy poskytovat zavádějící informace o době k vytvoření aplikace zobrazí zaneprázdněný.
- **Použití dob trvání Well-Defined** -indikátor průběhu nesmí jenom zobrazit, že náročná úloha trvá umístit, ale poskytnout uživatele a údaj o kolik úlohy dokončení a odhad zbývající dobu.

### <a name="implementing-an-progress-bar"></a>Implementace indikátor průběhu

Indikátor průběhu je vytvořen po vytvoření instance [`UIProgressView`](https://developer.xamarin.com/api/type/UIKit.UIProgressView/)

### <a name="progress-bars-and-storyboards"></a>Indikátory průběhu a scénářů

Indikátor průběhu můžete také přidat do vašeho uživatelského rozhraní při použití iOS Designer. Vyhledejte **zobrazení průběhu** v **sada nástrojů** a přetáhněte jej do zobrazení.

Na panelu pro vlastnosti lze upravit následující vlastnosti:

![Vlastnosti odsazení](progress-activity-indicator-images/progress-indicator3.png)


### <a name="managing-progress-bar-behavior"></a>Správa chování indikátor průběhu

Průběh panelu můžete nejdřív nastavit pomocí `Progress` vlastnost:

```csharp
ProgressBar.Progress = 0f;
```

Průběh lze upravit pomocí `SetProgress` metoda a předání logickou hodnotu deklarace, pokud chcete změnit animované nebo ne.

```csharp
ProgressBar.SetProgress(1.0f, true);
```

Další informace o použití indikátor průběhu, najdete v části [Reporting průběh](https://developer.xamarin.com/recipes/cross-platform/networking/download_progress/#Reporting_Progress_in_iOS) recepturách a [UICatalog tvOS ukázka](https://developer.xamarin.com/samples/monotouch/tvos/UICatalog/).

### <a name="managing-progress-bar-appearance"></a>Správa vzhled indikátor průběhu

Slouží jako ukazatel aktivity, podobně jako `UIProgressViewStyle` výčet může být předána jako parametr při vytváření instance indikátor průběhu.

Průběh a sledovat Image a odstín barvy lze upravit pomocí následujících vlastností:

```csharp
progressBar = new UIProgressView(UIProgressViewStyle.Default)
            {
                ProgressImage = UIImage.FromBundle("TrackImage"),
                ProgressTintColor = UIColor.Cyan,
                TrackImage = UIImage.FromBundle("TrackImage"),
                TrackTintColor = UIColor.Magenta
            }; 
```



