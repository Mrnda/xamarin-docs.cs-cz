---
title: Zkontrolujte žádost o aplikaci
description: Tento článek se zabývá metodu RequestReview této Apple přidat iOS 10 a jak implementovat v Xamarin.iOS.
ms.prod: xamarin
ms.assetid: 6408e707-b7dc-4557-b931-16a4d79b8930
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/29/2017
ms.openlocfilehash: 2fff227581d6eeca69d7a770308d9793a4831baf
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/04/2018
---
# <a name="request-app-review"></a>Zkontrolujte žádost o aplikaci

_Tento článek se zabývá metodu RequestReview této Apple přidat iOS 10 a jak implementovat v Xamarin.iOS._

Nový iOS 10.3, `RequestReview()` metoda umožní aplikaci pro iOS požádat uživatele o hodnocení nebo zkontrolujte ho. Když tato metoda je volána v přesouvání aplikaci, která uživatel nainstaloval z obchodu s aplikacemi, budou iOS 10 zpracovávat celý hodnocení a proces zkontrolovat pro vývojáře. Protože tento proces se řídí zásadami obchodu s aplikacemi, výstraha může nebo nemusí být zobrazeny.

![](request-app-review-images/review01.png "Ukázku Kontrola požadavků oznámení")

## <a name="requesting-a-rating-or-review"></a>Požaduje hodnocení nebo revize

Při `RequestReview()` statickou metodu `SKStoreReviewController` třída může být volána v libovolném bodě, kde má smysl v činnost koncového uživatele, je proces kontroly řídí a zpracovává zásady obchodu s aplikacemi. V důsledku toho tato metoda může nebo nemusí být zobrazeny výstrahy a nikdy by měla být volána v reakci na akci uživatele, jako je například klepnutím na tlačítko.

Aplikace může například žádostí, zkontrolujte po jejím spuštění zadaného počtu opakování nebo hru si mohou vyžádat kontrolu po dokončení přehrávač úrovní.

Na požadavky kontrolu také aplikace Xamarin.iOS dokončení spouštění, proveďte následující změny na `AppDelegate.cs` souboru:

```csharp
using Foundation;
using StoreKit;
using UIKit;

namespace iOSTenThree
{
    [Register ("AppDelegate")]
    public class AppDelegate : UIApplicationDelegate
    {
        ...

        public override bool FinishedLaunching (UIApplication application, NSDictionary launchOptions)
        {
            // Request a review from the user
            SKStoreReviewController.RequestReview ();

            return true;
        }
        
        ...
        
    }
}
```

> [!NOTE]
> Volání metody `RequestReview()` v snížení vývoj aplikace bude vždy zobrazovat hodnocení a zkontrolujte dialogové okno, může být testována. To neplatí pro aplikace, které byly distribuovány prostřednictvím TestFlight, kde volání metody, které budou ignorovány.

Když `RequestReview()` metoda je volána v přesouvání aplikaci, která uživatel nainstaloval z webu App Store, iOS 10 zpracuje celý proces hodnocení a zkontrolujte pro vývojáře. Znovu protože tento proces se řídí zásadami obchodu s aplikacemi, může výstrahu nebo se nemusí zobrazit.

## <a name="linking-to-an-app-store-product-page"></a>Propojení na stránku produktu App Storu 

Kromě nové `RequestReview` metody vývojář může stále poskytovat přímý odkaz na stránku produktu aplikace v úložišti aplikací z aplikace. Připojením `action=write-review` na konec adresy URL stránky produktu otevře se stránka kde uživatel může zapisovat kontrolu aplikace automaticky. 

## <a name="summary"></a>Souhrn

Tento článek má zahrnutých metodu RequestReview této Apple přidat iOS 10 a jak implementovat v Xamarin.iOS.



## <a name="related-links"></a>Související odkazy

- [iOSTenThree vzorku](https://developer.xamarin.com/samples/ios/iOS10/iOSTenThree)
