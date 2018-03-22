---
title: "Návod - použití místního oznámení v Xamarin.iOS"
description: "V této části projdeme použití místního oznámení aplikace pro Xamarin.iOS. Popisuje základní informace o vytváření a publikování oznámení, že se objeví se upozornění, když přijaté aplikací."
ms.topic: article
ms.prod: xamarin
ms.assetid: 32B9C6F0-2BB3-4295-99CB-A75418969A62
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/18/2017
ms.openlocfilehash: 846b292aed73a4980f4ce711ecefe4382fa7a321
ms.sourcegitcommit: 73bd0c7e5f237f0a1be70a6c1384309bb26609d5
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 03/22/2018
---
# <a name="walkthrough---using-local-notifications-in-xamarinios"></a>Návod - použití místního oznámení v Xamarin.iOS

_V této části projdeme použití místního oznámení aplikace pro Xamarin.iOS. Popisuje základní informace o vytváření a publikování oznámení, že se objeví se upozornění, když přijaté aplikací._

> [!IMPORTANT]
> Informace v této části se vztahují na iOS 9 a předchozí, ho byla ponechána zde k podpoře starší verze iOS. IOS 10 a novější, najdete v tématu [uživatele oznámení Framework – průvodce](~/ios/platform/user-notifications/index.md) pro podporu místní i vzdálené oznámení na zařízení s iOS.

## <a name="walkthrough"></a>Návod

Umožní vytvořit jednoduchou aplikaci, která se zobrazí místní oznámení v akci. Tato aplikace bude mít jedno tlačítko na něm. Když jsme kliknutím na tlačítko, vytvoří se místní oznámení. Po zadané časové období uplynutí, jsme se zobrazí oznámení se zobrazí.


1. V sadě Visual Studio pro Mac, vytvořte nové řešení iOS jednoho zobrazení a pojmenujte ji `Notifications`.
1. Otevřete `Main.storyboard` souboru a přetáhněte tlačítko na zobrazení. Název tlačítka **tlačítko**a zadejte jeho název **přidat oznámení**. Můžete také nastavit některé [omezení](~/ios/user-interface/designer/designer-auto-layout.md) na tlačítko v tomto okamžiku: 

    ![](local-notifications-in-ios-walkthrough-images/image3.png "Některá omezení na tlačítko Nastavení")
1. Upravit `ViewController` třídu a přidejte následující obslužné rutiny události ViewDidLoad metody:

    ```csharp
    button.TouchUpInside += (sender, e) =>
    {
        // create the notification
        var notification = new UILocalNotification();

        // set the fire date (the date time in which it will fire)
        notification.FireDate = NSDate.FromTimeIntervalSinceNow(60);

        // configure the alert
        notification.AlertAction = "View Alert";
        notification.AlertBody = "Your one minute alert has fired!";

        // modify the badge
        notification.ApplicationIconBadgeNumber = 1;

        // set the sound to be the default sound
        notification.SoundName = UILocalNotification.DefaultSoundName;

        // schedule it
        UIApplication.SharedApplication.ScheduleLocalNotification(notification);
    };
    ```

    Tento kód vytvoří oznámení, která používá zvuku, nastaví hodnotu ikonu oznámení na 1 a zobrazí uživateli výstrahu.

1. Potom upravte soubor `AppDelegate.cs`, nejprve přidejte následující kód, který `FinishedLaunching` metoda. Jsme jste zkontrolovali zobrazíte, pokud je na zařízení spuštěný iOS 8, pokud jsou proto jsme **požadované** a požádejte o oprávnění uživatele pro příjem oznámení:

    ```csharp
    if (UIDevice.CurrentDevice.CheckSystemVersion (8, 0)) {
            var notificationSettings = UIUserNotificationSettings.GetSettingsForTypes (
                UIUserNotificationType.Alert | UIUserNotificationType.Badge | UIUserNotificationType.Sound, null
            );

            application.RegisterUserNotificationSettings (notificationSettings);
        }
    ```

1. Pořád ještě v `AppDelegate.cs`, přidejte následující metodu, která bude volána při přijetí oznámení:

    ```csharp
    public override void ReceivedLocalNotification(UIApplication application, UILocalNotification notification)
            {
                // show an alert
                UIAlertController okayAlertController = UIAlertController.Create(notification.AlertAction, notification.AlertBody, UIAlertControllerStyle.Alert);
                okayAlertController.AddAction(UIAlertAction.Create("OK", UIAlertActionStyle.Default, null));

                Window.RootViewController.PresentViewController(okayAlertController, true, null);

                // reset our badge
                UIApplication.SharedApplication.ApplicationIconBadgeNumber = 0;
            }

    ```

1. Je zapotřebí zpracovat tento případ, kdy byla spuštěná oznámení z důvodu místního oznámení. Upravit metodu `FinishedLaunching` v `AppDelegate` zahrnout následující fragment kódu:


    ```csharp
    // check for a notification

    if (launchOptions != null)
    {
        // check for a local notification
        if (launchOptions.ContainsKey(UIApplication.LaunchOptionsLocalNotificationKey))
        {
            var localNotification = launchOptions[UIApplication.LaunchOptionsLocalNotificationKey] as UILocalNotification;
            if (localNotification != null)
            {
                UIAlertController okayAlertController = UIAlertController.Create(localNotification.AlertAction, localNotification.AlertBody, UIAlertControllerStyle.Alert);
                okayAlertController.AddAction(UIAlertAction.Create("OK", UIAlertActionStyle.Default, null));

                Window.RootViewController.PresentViewController(okayAlertController, true, null);

                // reset our badge
                UIApplication.SharedApplication.ApplicationIconBadgeNumber = 0;
            }
        }
    }

    ```

1. Nakonec spusťte aplikaci. V systému iOS 8, zobrazí se výzva k povolit oznámení. Klikněte na tlačítko **OK** a klikněte **přidat oznámení** tlačítko. Po krátkém čekání byste měli vidět dialogového okna výstrah, jak je vidět na následujících snímcích obrazovky:

    ![](local-notifications-in-ios-walkthrough-images/image0.png "Potvrzení schopnost posílání oznámení") ![ ] (local-notifications-in-ios-walkthrough-images/image1.png "oznámení přidat tlačítko") ![ ] (local-notifications-in-ios-walkthrough-images/image2.png "dialogového okna oznámení výstrah")

## <a name="summary"></a>Souhrn

Tento návod vám ukázal, jak používat různé rozhraní API pro vytváření a publikování oznámení v iOS. Také ukázal, jak aktualizovat ikona aplikace oznámení "BADGE" k poskytnutí zpětné vazby konkrétní některé aplikace pro uživatele.


## <a name="related-links"></a>Související odkazy

- [Místní oznámení (ukázka)](https://developer.xamarin.com/samples/monotouch/LocalNotifications)
- [Místní a Průvodce programováním nabízená oznámení](https://developer.apple.com/library/prerelease/content/documentation/NetworkingInternet/Conceptual/RemoteNotificationsPG/)
