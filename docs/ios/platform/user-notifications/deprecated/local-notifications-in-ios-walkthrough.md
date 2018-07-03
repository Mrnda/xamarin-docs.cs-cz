---
title: Návod – používání místních oznámení v Xamarin.iosu
description: V této části budeme zabývat použití místního oznámení aplikace pro Xamarin.iOS. To vám ukáže základy vytváření a publikování oznámení, objeví se upozornění, když přijatý aplikací.
ms.prod: xamarin
ms.assetid: 32B9C6F0-2BB3-4295-99CB-A75418969A62
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/18/2017
ms.openlocfilehash: cf1e44ba4176922234fc1b6b9bfe5c463611cc7b
ms.sourcegitcommit: 081a2d094774c6f75437d28b71d22607e33aae71
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/03/2018
ms.locfileid: "37403426"
---
# <a name="walkthrough---using-local-notifications-in-xamarinios"></a>Návod – používání místních oznámení v Xamarin.iosu

_V této části budeme zabývat použití místního oznámení aplikace pro Xamarin.iOS. To vám ukáže základy vytváření a publikování oznámení, objeví se upozornění, když přijatý aplikací._

> [!IMPORTANT]
> Informace v této části se vztahují na iOS 9 a předchozí, to byla ponechána zde pro podporu staršími verzemi Iosu. IOS 10 a novější, najdete v tématu [uživatel oznámení Framework – průvodce](~/ios/platform/user-notifications/index.md) pro podporu místní a Vzdálená oznámení na zařízení s Iosem.

## <a name="walkthrough"></a>Názorný postup

Umožní vytvořit jednoduchou aplikaci, která se zobrazí místní oznámení v akci. Tato aplikace bude obsahovat jedno tlačítko. Když jsme klikněte na tlačítko, vytvoří místní oznámení. Po zadanou dobu, uvidíme se zobrazí oznámení.


1. V sadě Visual Studio pro Mac, vytvořte nové řešení jedno zobrazení s Iosem a jeho volání `Notifications`.
1. Otevřít `Main.storyboard` souboru a přetáhněte tlačítko na zobrazení. Pojmenujte tlačítko **tlačítko**a přiřaďte jí název **přidat oznámení**. Můžete také nastavit některá [omezení](~/ios/user-interface/designer/designer-auto-layout.md) tlačítko v tomto okamžiku: 

    ![](local-notifications-in-ios-walkthrough-images/image3.png "Nastavení některých omezení na tlačítko")
1. Upravit `ViewController` třídu a přidejte následující obslužnou rutinu události do metody ViewDidLoad:

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

    Tento kód vytvoří oznámení, která používá zvuk, nastaví hodnotu Odznáček ikony na hodnotu 1 a zobrazí uživateli výstrahu.

1. Potom upravte soubor `AppDelegate.cs`, nejprve přidejte následující kód, který `FinishedLaunching` metody. Jsme kontrole zobrazíte, pokud zařízení běží systém iOS 8, pokud se tak nám **požadované** žádat o oprávnění uživatele pro příjem oznámení:

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

                UIApplication.SharedApplication.KeyWindow.RootViewController.PresentViewController(okayAlertController, true, null);

                // reset our badge
                UIApplication.SharedApplication.ApplicationIconBadgeNumber = 0;
            }

    ```

1. Je potřeba zpracovávat tento případ, ve kterém byl spuštěn oznámení z důvodu místního oznámení. Upravit metodu `FinishedLaunching` v `AppDelegate` zahrnout následující fragment kódu:


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

1. Nakonec spusťte aplikaci. V systému iOS 8 budete vyzváni k povolit oznámení. Klikněte na tlačítko **OK** a potom klikněte na tlačítko **přidat oznámení** tlačítko. Po krátkém čekání byste měli vidět dialogového okna výstrah, jak je znázorněno na následujících snímcích obrazovky:

    ![](local-notifications-in-ios-walkthrough-images/image0.png "Potvrzují se možnost Odeslat oznámení") ![ ] (local-notifications-in-ios-walkthrough-images/image1.png "tlačítko pro přidání oznámení") ![ ] (local-notifications-in-ios-walkthrough-images/image2.png "dialogového okna oznámení výstrah")

## <a name="summary"></a>Souhrn

Tento průvodce vám ukázal, jak pomocí různých rozhraní API pro vytváření a publikování oznámení v iOS. To jsme si rovněž ukázali jak aktualizovat ikonu aplikace s odznáčkem k poskytnutí zpětné vazby konkrétní některé aplikace pro uživatele.


## <a name="related-links"></a>Související odkazy

- [Místní oznámení (ukázka)](https://developer.xamarin.com/samples/monotouch/LocalNotifications)
- [Místních a nabízených oznámení Průvodce programováním](https://developer.apple.com/library/prerelease/content/documentation/NetworkingInternet/Conceptual/RemoteNotificationsPG/)
