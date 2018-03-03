---
title: "Poskytnutí zpětné vazby hmatová"
description: "Tento článek se zabývá nové typy hmatová zpětnou vazbu k dispozici v iOS 10 a jejich implementaci v Xamarin.iOS."
ms.topic: article
ms.prod: xamarin
ms.assetid: 888106D1-58F4-453F-BACC-91D51FA39C80
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/16/2017
ms.openlocfilehash: 9f4eb989fe0c91471c9473c512c4befd36e4ace2
ms.sourcegitcommit: 61f5ecc5a2b5dcfbefdef91664d7460c0ee2f357
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 02/28/2018
---
# <a name="providing-haptic-feedback"></a>Poskytnutí zpětné vazby hmatová

_Tento článek se zabývá nové typy hmatová zpětnou vazbu k dispozici v iOS 10 a jejich implementaci v Xamarin.iOS._

<a name="Overview" />

## <a name="overview"></a>Přehled

Na zařízení iPhone 7 a iPhone 7 Plus Apple je k dispozici nové hmatová odpovědi, které poskytují další způsoby, jak fyzicky zaujmout uživatele. Hmatová zpětnou vazbu (často označované jako Haptics jednoduše) používá představu o touch (prostřednictvím force, vibrací nebo pohybu) v uživatelském rozhraní návrhu. Použijte tyto nové možnosti taktilní zpětnou vazbu a získat pozornost uživatele a posilovat jejich akce.

Podrobně se budeme v následujících tématech:

- [O hmatová zpětné vazby](#About-Haptic-Feedback)
- [UIImpactFeedbackGenerator](#UIImpactFeedbackGenerator)
- [UINotificationFeedbackGenerator](#UINotificationFeedbackGenerator)
- [UISelectionFeedbackGenerator](#UISelectionFeedbackGenerator)

<a name="About-Haptic-Feedback" />

## <a name="about-haptic-feedback"></a>O hmatová zpětné vazby

Několik předdefinovaných prvky uživatelského rozhraní již svůj názor hmatová například ovládacích prvků výběr, přepínače a posuvníky. iOS 10 teď navíc umožňuje prostřednictvím kódu programu aktivovat haptics pomocí konkrétní podtřídou třídy `UIFeedbackGenerator` třídy.

Vývojář můžete použít jednu z následujících `UIFeedbackGenerator` podtřídy na prostřednictvím kódu programu hmatová připomínky aktivační události:

- `UIImpactFeedbackGenerator` -Použijte tento generátor zpětnou vazbu k doplnění akci nebo úlohu, například prezentace "thud", při zobrazení snímky do místní, nebo pokud dojít ke konfliktu dvou objektů na obrazovce.
- `UINotificationFeedbackGenerator` -Použijte tento generátor zpětnou vazbu pro oznámení, jako je například typ akce dokončuje, chybě nebo jakýchkoli jiných upozornění.
- `UISelectionFeedbackGenerator` -Použijte tento generátor zpětnou vazbu pro výběr aktivně změna například výběr položky ze seznamu.

<a name="UIImpactFeedbackGenerator" />

### <a name="uiimpactfeedbackgenerator"></a>UIImpactFeedbackGenerator

Pomocí tohoto generátoru zpětnou vazbu doplňují akci nebo úlohu, například prezentace "thud", při zobrazení snímky do místní, nebo pokud dojít ke konfliktu dvou objektů na obrazovce.

Použijte následující kód na připomínky dopad aktivační události:

```csharp
using UIKit;
...

// Initialize feedback
var impact = new UIImpactFeedbackGenerator (UIImpactFeedbackStyle.Heavy);
impact.Prepare ();

// Trigger feedback
impact.ImpactOccurred ();
```

Když vývojář vytvoří novou instanci třídy `UIImpactFeedbackGenerator` třída poskytují `UIImpactFeedbackStyle` určení síly připomínek jako:

- `Heavy`
- `Medium`
- `Light`

`Prepare` Metodu `UIImpactFeedbackGenerator` je volána k informování systému, aby hmatová zpětná vazba je dojde tak, aby ho zajištění minimální latence.

`ImpactOccurred` Metoda pak spustí hmatová zpětnou vazbu.

<a name="UINotificationFeedbackGenerator" />

### <a name="uinotificationfeedbackgenerator"></a>UINotificationFeedbackGenerator

Pomocí tohoto generátoru zpětnou vazbu pro oznámení, jako je například typ akce dokončuje, chybě nebo jakýchkoli jiných upozornění.

Použijte následující kód na připomínky oznámení aktivační události:

```csharp
using UIKit;
...

// Initialize feedback
var notification = new UINotificationFeedbackGenerator ();
notification.Prepare ();

// Trigger feedback
notification.NotificationOccurred (UINotificationFeedbackType.Error);
```

Novou instanci třídy `UINotificationFeedbackGenerator` vytvořit třídu a jeho `Prepare` metoda je volána k informování systému, aby hmatová zpětná vazba je dojde tak, aby ho zajištění minimální latence.

`NotificationOccurred` Je volána k aktivaci hmatová zpětná danou `UINotificationFeedbackType` z:

- `Success`
- `Warning`
- `Error`

<a name="UISelectionFeedbackGenerator" />

### <a name="uiselectionfeedbackgenerator"></a>UISelectionFeedbackGenerator

Pomocí tohoto generátoru zpětnou vazbu pro výběr aktivně změna například výběr položky ze seznamu.

Použijte následující kód na připomínky výběr aktivační události:

```csharp
using UIKit;
...

// Initialize feedback
var selection = new UISelectionFeedbackGenerator ();
selection.Prepare ();

// Trigger feedback
selection.SelectionChanged ();
```

Novou instanci třídy `UISelectionFeedbackGenerator` vytvořit třídu a jeho `Prepare` metoda je volána k informování systému, aby hmatová zpětná vazba je dojde tak, aby ho zajištění minimální latence.

`SelectionChanged` Metoda pak spustí hmatová zpětnou vazbu.

## <a name="summary"></a>Souhrn

Tento článek má zahrnutých nové typy hmatová zpětnou vazbu k dispozici v iOS 10 a jejich implementaci v Xamarin.iOS.

## <a name="related-links"></a>Související odkazy

- [iOS 10 – ukázky](https://developer.xamarin.com/samples/ios/iOS10/)
