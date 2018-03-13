---
title: "Vzhled rozhraní API"
description: "iOS vám umožňuje použít nastavení visual vlastností na úrovni statická třída a nikoli na jednotlivé objekty tak, aby tato změna se vztahuje na všechny instance tohoto ovládacího prvku v aplikaci."
ms.topic: article
ms.prod: xamarin
ms.assetid: C1727F0C-82B1-D085-D46F-C6383FF04B16
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/22/2017
ms.openlocfilehash: f35256529d6d72a3f5e563dc88b9d5883a9724d4
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 03/09/2018
---
# <a name="appearance-api"></a>Vzhled rozhraní API

_iOS vám umožňuje použít nastavení visual vlastností na úrovni statická třída a nikoli na jednotlivé objekty tak, aby tato změna se vztahuje na všechny instance tohoto ovládacího prvku v aplikaci._

Tato funkce je zveřejněna v Xamarin.iOS prostřednictvím statického `Appearance` vlastnost u všech UIKit ovládacích prvků, které ji podporují. Vzhled (vlastnosti, například jako obrázek odstín barvy a pozadí) můžete proto snadno přizpůsobit aplikaci poskytnout jednotný vzhled. Rozhraní API vzhled byla zavedena v systému iOS 5 a při některé části jsou zastaralé v iOS 9 je stále vhodný způsob, jak dosáhnout některé stylů a motivů efekty při aplikace Xamarin.iOS.

## <a name="overview"></a>Přehled

iOS umožňuje že přizpůsobení vzhledu ovládacího prvku mnoho UIKit standardní ovládací prvky odpovídat branding, který chcete použít pro vaši aplikaci.

Použít vlastní vzhled dvěma různými způsoby:

- **Přímo na instanci ovládacího prvku** – včetně panely nástrojů, navigační panely, tlačítka a posuvníky mnoho ovládacích prvků můžete nastavit odstín barvy, obrázek na pozadí a pozice názvu (a také některé další atributy).

- **Nastavit výchozí hodnoty na statickou vlastnost vzhled** – přizpůsobitelné atributy pro každý ovládací prvek jsou zveřejňovány prostřednictvím `Appearance` statickou vlastnost. Veškerá přizpůsobení, která můžete použít pro tyto vlastnosti se použije jako výchozí pro libovolný ovládací prvek tohoto typu, které se vytvoří, jakmile je vlastnost nastavena.

Ukázkovou aplikaci vzhled ukazuje všechny tři způsoby, jak je znázorněno v tyto snímky obrazovky:

 [![](introduction-to-the-appearance-api-images/appearance01.png "Ukázkovou aplikaci vzhled ukazuje všechny tři metody")](introduction-to-the-appearance-api-images/appearance01.png#lightbox)

Od verze iOS 8 proxy vzhled rozšířilo a TraitCollections.
 `AppearanceForTraitCollection` slouží k nastavení vzhledu výchozí kolekce konkrétní znak. Další informace najdete v [Úvod do scénářů](~/ios/user-interface/storyboards/unified-storyboards.md) průvodce.


## <a name="setting-appearance-properties"></a>Nastavení vlastností vzhledu

Na první obrazovce statická třída vzhled slouží k úpravě stylu tlačítek a prvků žlutý/oranžová takto:

```csharp
// Set the default appearance values
UIButton.Appearance.TintColor = UIColor.LightGray;
UIButton.Appearance.SetTitleColor(UIColor.FromRGB(0,127,14), UIControlState.Normal);

UISlider.Appearance.ThumbTintColor = UIColor.Red;
UISlider.Appearance.MinimumTrackTintColor = UIColor.Orange;
UISlider.Appearance.MaximumTrackTintColor = UIColor.Yellow;

UIProgressView.Appearance.ProgressTintColor = UIColor.Yellow;
UIProgressView.Appearance.TrackTintColor = UIColor.Orange;
```

Styly zelená element jsou nastaveny v tímto způsobem, `ViewDidLoad` metoda, která přepisuje výchozí hodnoty a *vzhled* statická třída:

```csharp
slider2.ThumbTintColor = UIColor.FromRGB (0,127,70); // dark green
slider2.MinimumTrackTintColor = UIColor.FromRGB (66,255,63);
slider2.MaximumTrackTintColor = UIColor.FromRGB (197,255,132);
```

```csharp
progress2.ProgressTintColor = UIColor.FromRGB (66,255,63);
progress2.TrackTintColor = UIColor.FromRGB (197,255,132);
```

## <a name="using-uiappearance-in-xamarinforms"></a>Použití UIAppearance v Xamarin.Forms

Rozhraní API vzhled může být užitečné, když [styly aplikaci pro iOS](~/xamarin-forms/platform/ios/theme.md#uiappearance) v řešení Xamarin.Forms. Po zadání několika řádků v `AppDelegate` třída může pomoci implementovat barevné schéma bez nutnosti vytvářet [vlastní zobrazovací jednotky](~/xamarin-forms/app-fundamentals/custom-renderer/index.md).


### <a name="custom-themes-and-uiappearance"></a>Vlastní motivy a UIAppearance

iOS – ovládací prvky jako "motivu" použití rozhraní umožňuje mnoho visual atributů uživatele *UIAppearance* rozhraní API pro všechny instance konkrétní ovládacího prvku tak, aby měl stejné vzhled vynutit. To je zpřístupněná jako vlastnost vzhled mnoho třídy ovládacích prvků uživatelského rozhraní, nikoli jednotlivých instancí ovládacího prvku. Nastavení vlastnosti zobrazení na statické `Appearance` vlastnost ovlivňuje všechny ovládací prvky daného typu v aplikaci.

Chcete-li lépe porozumět koncepci, zvažte příklad.

Chcete-li změnit konkrétní `UISegmentedControl` tak, aby měl fialové odstín, jsme by odkazovat na konkrétní ovládací prvek v našem obrazovku podobnou v `ViewDidLoad`:

```csharp
sg1.TintColor = UIColor.Magenta;
```

Alternativně nastavte hodnotu v poli vlastnosti pro návrháře: 

[![](introduction-to-the-appearance-api-images/propertiespadtint.png "TINT – vlastnosti odsazení")](introduction-to-the-appearance-api-images/propertiespadtint.png#lightbox)

Následující obrázek ukazuje, že nastaví tato odstín barvy na pouze ovládací prvek s názvem 'sz1'.

 [![](introduction-to-the-appearance-api-images/image53.png "Nastavení TINT – jednotlivé ovládací prvek")](introduction-to-the-appearance-api-images/image53.png#lightbox)

Nastavení mnoho ovládacích prvků tímto způsobem by bylo úplně neefektivní, takže jsme místo toho můžete nastavit statickou `Appearance` vlastnost na vlastní třídy. To je ukázáno v následujícím kódu:

```csharp
UISegmentedControl.Appearance.TintColor = UIColor.Magenta;
```

Následující obrázek nyní znázorňuje obou segmentovaným ovládací prvky s vzhled nastavit fialově:

 [![](introduction-to-the-appearance-api-images/image54.png "Nastavení odstín barvy vzhledu ovládacího prvku")](introduction-to-the-appearance-api-images/image54.png#lightbox)

`Appearance` vlastnosti musí být nastaveno již v rané fázi v průběhu životního cyklu aplikací, například v AppDelegate `FinishedLaunching` události, nebo v ViewController před ovlivněných ovládací prvky jsou zobrazeny.


Odkazovat [Úvod do rozhraní API vzhled](~/ios/user-interface/ios-ui/introduction-to-the-appearance-api.md) podrobnější informace.


## <a name="related-links"></a>Související odkazy

- [Vzhled (ukázka)](https://developer.xamarin.com/samples/monotouch/IntroToAppearance/)
- [Referenční informace o protokolu UIAppearance](https://developer.apple.com/library/ios/documentation/UIKit/Reference/UIAppearance_Protocol/)
- [Vzhled v Xamarin.Forms](~/xamarin-forms/platform/ios/theme.md#uiappearance)
