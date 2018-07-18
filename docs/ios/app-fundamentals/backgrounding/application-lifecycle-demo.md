---
title: Ukázka životního cyklu aplikace pro Xamarin.iOS
description: Tento dokument zkoumá různé události životního cyklu, které jsou zpracovány delegáta aplikace v aplikaci iOS představením toho, kdy a jak tyto události jsou zpracovávány.
ms.prod: xamarin
ms.assetid: 5C8AACA6-49F8-4C6D-99C3-5F443C01B230
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 07/17/2018
ms.openlocfilehash: 53ae6947cf1483fabe415d6f6521d9384bddb46f
ms.sourcegitcommit: e98a9ce8b716796f15de7cec8c9465c4b6bb2997
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/18/2018
ms.locfileid: "39111170"
---
# <a name="application-lifecycle-demo-for-xamarinios"></a>Ukázka životního cyklu aplikace pro Xamarin.iOS

V tomto článku a [ukázkový kód](https://developer.xamarin.com/samples/monotouch/LifecycleDemo/) ukazuje stavy čtyři aplikace v Iosu a role `AppDelegate` metody v oznámení použití získat změny stavů. Aplikace vypíše aktualizace do konzoly pokaždé, když se změní stav aplikace:

[![](application-lifecycle-demo-images/image3-sml.png "Ukázková aplikace")](application-lifecycle-demo-images/image3.png#lightbox)

[![](application-lifecycle-demo-images/image4.png "Aplikace vytiskne aktualizace do konzoly pokaždé, když se změní stav aplikace")](application-lifecycle-demo-images/image4.png#lightbox)

## <a name="walkthrough"></a>Názorný postup

1. Otevřít **životního cyklu** projekt **LifecycleDemo** řešení.
1. Otevřete `AppDelegate` třídy. Protokolování je přidaný do životního cyklu metody k označení, změny stavu aplikace:

    ```csharp
    public override void OnActivated(UIApplication application)
    {
        Console.WriteLine("OnActivated called, App is active.");
    }
    public override void WillEnterForeground(UIApplication application)
    {
        Console.WriteLine("App will enter foreground");
    }
    public override void OnResignActivation(UIApplication application)
    {
        Console.WriteLine("OnResignActivation called, App moving to inactive state.");
    }
    public override void DidEnterBackground(UIApplication application)
    {
        Console.WriteLine("App entering background state.");
    }
    // not guaranteed that this will run
    public override void WillTerminate(UIApplication application)
    {
        Console.WriteLine("App is terminating.");
    }
    ```

1. Spusťte aplikaci v simulátoru, nebo na zařízení. `OnActivated` bude volána při spuštění aplikace. Aplikace je nyní v _aktivní_ stavu.
1. Klikněte na tlačítko Domů na simulátor nebo zařízení, aby aplikace na pozadí. `OnResignActivation` a `DidEnterBackground` bude volána jako aplikace přechody ze `Active` k `Inactive` a do `Backgrounded` stavu. Protože neexistuje žádná sada kódu aplikace ke spuštění na pozadí, aplikace se považuje za _pozastaveno_ v paměti.
1. Přejděte zpět do aplikace ji vrací do stavu v popředí. `WillEnterForeground` a `OnActivated` obě, zavolá se:

    ![](application-lifecycle-demo-images/image4.png "Vytisknout na konzole změny stavu")

    Následující řádek kódu v kontroleru zobrazení se spustí, až aplikace se dostala na popředí z na pozadí a změní text zobrazený na obrazovce:

    ```csharp
    UIApplication.Notifications.ObserveWillEnterForeground ((sender, args) => {
        label.Text = "Welcome back!";
    });
    ```

1. Stisknutím klávesy **Domů** tlačítko Vložit aplikaci na pozadí. Potom dvojitého klepnutí **Domů** tlačítko zobrazíte přepínačem aplikace. Na zařízeních iPhone X potažením prstem přejděte do dolní části obrazovky:

    [![Přepínání aplikace](application-lifecycle-demo-images/app-switcher-sml.png "přepínání aplikace")](application-lifecycle-demo-images/app-switcher.png#lightbox)
  
1. V přepínači aplikací vyhledejte aplikaci a potažením prstem přejděte na odebrat (v iOS 11, dlouhé stisknutí dokud zobrazí červený ikony v horním):

    [![Potáhnutí prstem až po odebrání spuštěné aplikaci](application-lifecycle-demo-images/app-switcher-swipe-sml.png "potáhnutí prstem až po odebrání spuštěné aplikace")](application-lifecycle-demo-images/app-switcher-swipe.png#lightbox)

iOS ukončíte aplikaci. Všimněte si, že `WillTerminate` nevolá, protože aplikace je již _pozastaveno_ na pozadí.

## <a name="related-links"></a>Související odkazy

- [LifecycleDemo (ukázka)](https://developer.xamarin.com/samples/monotouch/LifecycleDemo/)
