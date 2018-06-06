---
title: Ukázka životního cyklu aplikace pro Xamarin.iOS
description: Tento dokument prověří různé události životního cyklu zpracovávaných delegát aplikace v aplikaci iOS, ukázka, kdy a jak tyto události jsou zpracovávány.
ms.prod: xamarin
ms.assetid: 5C8AACA6-49F8-4C6D-99C3-5F443C01B230
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/18/2017
ms.openlocfilehash: 64c695065012e4bf796c219c260324d9b6278ca5
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 06/05/2018
ms.locfileid: "34783581"
---
# <a name="application-lifecycle-demo-for-xamarinios"></a>Ukázka životního cyklu aplikace pro Xamarin.iOS

V této části jsme se chystáte vyhledejte aplikaci, která popisuje čtyři stavy aplikace a role `AppDelegate` metody v upozornění o při získání stavy změně aplikace. Aplikace bude tisku aktualizace do konzoly vždy, když se změní stav aplikace:

 [![](application-lifecycle-demo-images/image3.png "Ukázková aplikace")](application-lifecycle-demo-images/image3.png#lightbox)

 [![](application-lifecycle-demo-images/image4.png "Aplikace bude tisku aktualizace konzoly, vždy, když aplikace změní stav")](application-lifecycle-demo-images/image4.png#lightbox)

## <a name="walkthrough"></a>Návod


  1. Otevřete _životního cyklu_ v projektu _LifecycleDemo_ řešení.
  1. Otevřete `AppDelegate` třídy. Všimněte si, že jsme přidali protokolování metody životního cyklu a dejte nám vědět, změny stavu aplikace:

            ```chsarp
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

  1. Spusťte aplikaci v simulátoru, nebo na zařízení. `OnActivated` bude volána při spuštění aplikace. Aplikace je nyní v _Active_ stavu.
  1. Stiskněte tlačítko Domů v simulátoru nebo zařízení, aby aplikace na pozadí. `OnResignActivation` a `DidEnterBackground` se říká aplikace přechodů ze `Active` k `Inactive` a do `Backgrounded` stavu. Vzhledem k tomu, že jsme nedali naše aplikace žádný kód pro spuštění na pozadí, aplikace se považuje za _pozastaveno_ v paměti.
  1. Přejděte zpět do aplikace a vrátí ji zpět do popředí. `WillEnterForeground` a `OnActivated` budou obě volání:

        ![](application-lifecycle-demo-images/image4.png "Změny stavu vytisknout do konzoly nástroje")

    Všimněte si, jsme přidali následující kód do kontroleru zobrazení nám upozornění, že aplikace přešla popředí z na pozadí:

        ```csharp
            UIApplication.Notifications.ObserveWillEnterForeground ((sender, args) => {
                    label.Text = "Welcome back!";
                });
        ```

1. Stiskněte **Domů** tlačítko uvést aplikaci na pozadí. Potom poklepáním **Domů** tlačítko zprovoznit přepínači aplikace:
    
    ![](application-lifecycle-demo-images/app-switcher-.png "Aplikace přepínači")
  
1. Vyhledejte aplikaci v aplikaci přepínači a potažením prstem přejděte na odebrat:
    
    ![](application-lifecycle-demo-images/app-switcher-swipe-.png "Potažením prstem přejděte na odebrat spuštěné aplikaci") 
    
iOS přeruší aplikaci. Mějte na paměti, `WillTerminate` není volat, protože jsme se ukončení aplikace, která je _pozastaveno_ na pozadí.

Teď, když chápeme iOS aplikace stavy a přechody, provedeme podívejte se na různé možnosti, které jsou k dispozici pro backgrounding v iOS.



## <a name="related-links"></a>Související odkazy

- [LifecycleDemo(Part2) (ukázka)](https://developer.xamarin.com/samples/monotouch/LifecycleDemo/)
