---
title: Oznámení v Xamarin.iosu
description: Tato část ukazuje, jak implementovat místní oznámení v Xamarin.iosu. Bude popisují různé elementy uživatelského rozhraní z oznámení o iOS a diskutovat o rozhraní API na péči o vytváření a zobrazování oznámení.
ms.prod: xamarin
ms.assetid: 5BB76915-5DB0-48C7-A267-FA9F7C50793E
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 07/13/2018
ms.openlocfilehash: ef5ce97736e60ff3a03bc0d496eadcae8c6f7e61
ms.sourcegitcommit: cb80df345795989528e9df78eea8a5b45d45f308
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/14/2018
ms.locfileid: "39038531"
---
# <a name="notifications-in-xamarinios"></a>Oznámení v Xamarin.iosu

> [!IMPORTANT]
> Informace v této části se vztahují na iOS 9 a předchozí. IOS 10 a novější, najdete v tématu [uživatelská příručka k rozhraní framework oznámení](~/ios/platform/user-notifications/index.md).

iOS má tři způsoby, jak uživateli označit, že bylo přijato oznámení:

- **Zvuk nebo pronikavost** -iOS můžete přehrát zvuk oznámení uživatelům. Pokud zvuk je zakázaná, může být toto zařízení nakonfigurované vibrovat.
- **Výstrahy** – je možné zobrazit dialogové okno na obrazovce s informacemi o oznámení.
- **Odznáčky** – když se publikuje oznámení, je možné zobrazit číslo (označené jako) na ikonu aplikace.

poskytuje také iOS *centrum oznámení* všechna oznámení, místních i vzdálených, který se zobrazí uživateli. Uživatelé mohou získat přístup potažením dolů z horní části obrazovky:

![Centrum oznámení](local-notifications-in-ios-images/image13.png "Centrum oznámení")

## <a name="creating-local-notifications-in-ios"></a>Vytváření místních oznámení v iOS

iOS je poměrně snadné k vytvoření a zpracování místního oznámení.
IOS 8 vyžaduje nejprve aplikace a požádejte o oprávnění uživatele zobrazíte oznámení. Přidejte následující kód do aplikace před pokusem o odeslání místní oznámení – [připojené ukázkové](https://developer.xamarin.com/samples/monotouch/LocalNotifications/) umístí jej do **AppDelegate**společnosti **FinishedLaunching** metody.

```csharp
var notificationSettings = UIUserNotificationSettings.GetSettingsForTypes(
    UIUserNotificationType.Alert | UIUserNotificationType.Badge | UIUserNotificationType.Sound, null
);
application.RegisterUserNotificationSettings(notificationSettings);
```

[![Potvrzují se možnost odeslat místního oznámení](local-notifications-in-ios-images/image0-sml.png "potvrzení umožňuje odeslat místního oznámení změny")](local-notifications-in-ios-images/image0.png#lightbox)

Naplánování místního oznámení vytvořit `UILocalNotification` objektu, nastaven `FireDate`a naplánovat jeho prostřednictvím `ScheduleLocalNotification` metodu na `UIApplication.SharedApplication` objektu. Následující fragment kódu ukazuje, jak naplánovat oznámení, že se aktivuje v budoucnu jedné minuty a zobrazí výstraha a zobrazí se zpráva:

```csharp
UILocalNotification notification = new UILocalNotification();
NSDate.FromTimeIntervalSinceNow(15);
//notification.AlertTitle = "Alert Title"; // required for Apple Watch notifications
notification.AlertAction = "View Alert";
notification.AlertBody = "Your 15 second alert has fired!";
UIApplication.SharedApplication.ScheduleLocalNotification(notification);
```

Následující snímek obrazovky ukazuje, jak vypadá této výstrahy:

[![](local-notifications-in-ios-images/image2-sml.png "Příklad upozornění")](local-notifications-in-ios-images/image2.png#lightbox)

Všimněte si, že pokud se uživatel rozhodl *nepovoluje* se zobrazí oznámení pak žádnou akci.

Pokud chcete použít Odznáček s číslem na ikonu aplikace, můžete nastavit ji jak je znázorněno v následující řádek kódu:

```csharp
notification.ApplicationIconBadgeNumber = 1;
```

V pořadí přehrát zvuk s ikonou, nastavte vlastnost SoundName na oznámení jak je znázorněno v následujícím fragmentu kódu:

```csharp
notification.SoundName = UILocalNotification.DefaultSoundName;
```

Pokud zvukové upozornění je delší než 30 sekund, iOS bude přehrávat výchozí zvuk místo.

> [!IMPORTANT]
> Je chyba v simulátoru iOS, která se aktivuje upozornění delegáta dvakrát. Tento problém by nemělo dojít při spuštění aplikace na zařízení.

## <a name="handling-notifications"></a>Zpracování oznámení

aplikace pro iOS zpracování vzdálené a místní oznámení v téměř přesně stejným způsobem. Když aplikace běží, `ReceivedLocalNotification` metoda nebo `ReceivedRemoteNotification` metodu na `AppDelegate` třídu bude volat a informace o oznámení se předá jako parametr.

Může aplikace řešit oznámení různými způsoby. Aplikace může například pouze zobrazit upozornění, abyste jim Připomeňte o některé události. Nebo oznámení může se použít k zobrazení oznámení pro uživatele, který proces dokončí, jako je například synchronizace souborů na serveru.

Následující kód ukazuje, jak zpracovat místního oznámení a zobrazit výstrahy a oznámení "BADGE" číslo resetuje na nulu:

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

Pokud není aplikace spuštěna, iOS bude přehrávat zvuk a/nebo aktualizovat ikonu oznámení "BADGE" podle potřeby. Když uživatel spustí aplikaci souvisejícího s výstrahou, spustí se aplikace a `FinishedLaunching` zavolá se metoda delegáta aplikace a informace o oznámení budou předávána ve prostřednictvím `launchOptions` parametru. Pokud možnosti slovník obsahuje klíč `UIApplication.LaunchOptionsLocalNotificationKey`, pak bude `AppDelegate` ví, že se spustila aplikace z místního oznámení. Následující fragment kódu ukazuje tento proces:

```csharp
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
```

Pro vzdálené oznamování `launchOptions` bude mít `LaunchOptionsRemoteNotificationKey` s přidruženou `NSDictionary` obsahující datové části Vzdálená oznámení. Můžete rozbalit datová část oznámení prostřednictvím `alert`, `badge`, a `sound` klíče. Následující fragment kódu ukazuje, jak získat Vzdálená oznámení:

```csharp
NSDictionary remoteNotification = options[UIApplication.LaunchOptionsRemoteNotificationKey];
if(remoteNotification != null)
{
    string alert = remoteNotification["alert"];
}
```

## <a name="summary"></a>Souhrn

Tato část vám ukázal, jak vytvářet a publikovat oznámení v Xamarin.iosu. Měl zobrazit, jak může aplikace reakce na oznámení tak, že přepíšete `ReceivedLocalNotification` metoda nebo `ReceivedRemoteNotification` metoda ve `AppDelegate`.

## <a name="related-links"></a>Související odkazy

- [Místní oznámení (ukázka)](https://developer.xamarin.com/samples/monotouch/LocalNotifications)
- [Místní a nabízenými oznámeními pro vývojáře](https://developer.apple.com/notifications/)
- [Místních a nabízených oznámení Průvodce programováním](https://developer.apple.com/library/prerelease/content/documentation/NetworkingInternet/Conceptual/RemoteNotificationsPG/)
- [UIApplication](http://iosapi.xamarin.com/?link=T%3aMonoTouch.UIKit.UIApplication)
- [UILocalNotification](http://iosapi.xamarin.com/?link=T%3aMonoTouch.UIKit.UILocalNotification)
