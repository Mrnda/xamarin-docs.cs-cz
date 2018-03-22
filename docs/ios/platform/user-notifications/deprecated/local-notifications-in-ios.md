---
title: "Oznámení v Xamarin.iOS"
description: "V této části ukazuje, jak implementovat místní oznámení v Xamarin.iOS. Bude popisují různé prvky uživatelského rozhraní oznámení iOS a popisují rozhraní API je zahrnuta s vytváření a zobrazování oznámení."
ms.topic: article
ms.prod: xamarin
ms.assetid: 5BB76915-5DB0-48C7-A267-FA9F7C50793E
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/18/2017
ms.openlocfilehash: f1b36d3ba8601d125d0a17173efb12c249224e78
ms.sourcegitcommit: 73bd0c7e5f237f0a1be70a6c1384309bb26609d5
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 03/22/2018
---
# <a name="notifications-in-xamarinios"></a>Oznámení v Xamarin.iOS

_V této části ukazuje, jak implementovat místní oznámení v Xamarin.iOS. Bude popisují různé prvky uživatelského rozhraní oznámení iOS a popisují rozhraní API je zahrnuta s vytváření a zobrazování oznámení._

> [!IMPORTANT]
> Informace v této části se vztahují na iOS 9 a předchozí, ho byla ponechána zde k podpoře starší verze iOS. IOS 10 a novější, najdete v tématu [uživatele oznámení Framework – průvodce](~/ios/platform/user-notifications/index.md) pro podporu místní i vzdálené oznámení na zařízení s iOS.

iOS má tři způsoby, jak oznámení uživateli, aby bylo přijato oznámení:

-  **Zvukové nebo vibrací** -iOS můžete přehrát zvuk, který má upozornit uživatele. Pokud zvuk je zakázaná, může být toto zařízení nakonfigurované na zavibrovat.
-  **Výstrahy** -je možné zobrazit dialogové okno na obrazovce s informacemi o oznámení.
-  **Odznaky** – při publikování oznámení číslo lze zobrazit (služebním označením) na ikonu aplikace.


také poskytuje iOS *centru oznámení* všechna oznámení, místních i vzdálených, který se zobrazí uživateli. Uživatelé mohou přístup k: potažením dolů z horní části obrazovky:

 ![](local-notifications-in-ios-images/image13.png "Centrum oznámení")

## <a name="creating-local-notifications-in-ios"></a>Vytvoření místního oznámení v iOS

iOS je poměrně snadné k vytvoření a zpracování místního oznámení.
Nejprve iOS 8 vyžaduje aplikace a požádejte o oprávnění k zobrazení oznámení. Přidejte následující kód do aplikace před pokusem o odeslání oznámení místní – připojené ukázka umístí jej do **AppDelegate**na **FinishedLaunching** metoda.

```csharp
var settings = UIUserNotificationSettings.GetSettingsForTypes(
  UIUserNotificationType.Alert | UIUserNotificationType.Badge | UIUserNotificationType.Sound
  , null);
UIApplication.SharedApplication.RegisterUserNotificationSettings (settings);
```

  [![](local-notifications-in-ios-images/image0-sml.png "Potvrzení schopnost posílání místního oznámení")](local-notifications-in-ios-images/image0.png#lightbox)

Při plánování místního oznámení vytvoříte `UILocalNotification` objektu, nastavte `FireDate`a naplánovat pomocí `ScheduleLocalNotification` metodu `UIApplication.SharedApplication` objektu. Následující fragment kódu se ukazují, jak naplánovat oznámení, že bude fire v budoucnu jednu minutu a zobrazit výstrahu o zprávou:

```csharp
UILocalNotification notification = new UILocalNotification();
NSDate.FromTimeIntervalSinceNow(15);
//notification.AlertTitle = "Alert Title"; // required for Apple Watch notifications
notification.AlertAction = "View Alert";
notification.AlertBody = "Your 15 second alert has fired!";
UIApplication.SharedApplication.ScheduleLocalNotification(notification);
```

Následující snímek obrazovky zobrazit tuto výstrahu, která bude vypadat takto:

  [![](local-notifications-in-ios-images/image2-sml.png "Příklad výstrahy")](local-notifications-in-ios-images/image2.png#lightbox)

Všimněte si, že pokud se uživatel rozhodl *nepovolíte* oznámení a nic se zobrazí.

Pokud chcete použít oznámení "BADGE" na ikonu aplikace s číslem, můžete ho jak je znázorněno v následujícím řádku kódu nastavit:

```csharp
notification.ApplicationIconBadgeNumber = 1;
```

V play pořadí zvuku s ikonou, nastavte vlastnost SoundName na oznámení jak je znázorněno v následující fragment kódu:

```csharp
notification.SoundName = UILocalNotification.DefaultSoundName;
```

Podle dokumentu Apple Human Interface Guidelines Pokud oznámení přehrát zvuk, ho měli také doprovázet Odznáček nebo uživatelům pomoci identifikovat aplikace, který výstrahu vyvolal výstrahu. Navíc pokud zvuk je delší než 30 sekund, iOS bude přehrát výchozí zvuk místo.

> [!IMPORTANT]
> Je chyba v simulátoru iOS, které bude platit oznámení delegáta dvakrát. Tento problém by nemělo dojít při spuštění aplikace na zařízení.

## <a name="handling-notifications"></a>Zpracování oznámení

aplikace pro iOS zpracování vzdálené a místní oznámení v téměř přesně stejným způsobem. Když aplikace běží, `ReceivedLocalNotification` metoda nebo `ReceivedRemoteNotification` volání metody pro třídu AppDelegate a oznámení o se předá jako parametr.

Aplikace může zpracovávat oznámení různými způsoby. Například aplikace může zobrazit právě výstrahu k upozornění uživatele o některé události. Nebo oznámení může použít k zobrazení výstrahy uživateli, který tento proces se dokončí, jako jsou například synchronizace soubory na server.

Následující kód ukazuje, jak zpracovat místního oznámení a zobrazit výstrahu a číslo oznámení "BADGE" nastaven na hodnotu nula:

```csharp
 public override void ReceivedLocalNotification(UIApplication application, UILocalNotification notification)
        {
            // show an alert
            UIAlertController okayAlertController = UIAlertController.Create (notification.AlertAction, notification.AlertBody, UIAlertControllerStyle.Alert);
            okayAlertController.AddAction (UIAlertAction.Create ("OK", UIAlertActionStyle.Default, null));
            viewController.PresentViewController (okayAlertController, true, null);

            // reset our badge
            UIApplication.SharedApplication.ApplicationIconBadgeNumber = 0;
        }
```

Pokud aplikace není spuštěný, bude iOS přehrát zvuk nebo aktualizovat ikonu oznámení "BADGE" podle vhodnosti. Když uživatel spustí aplikaci spojených s výstrahou, se spustí aplikace a bude volána metoda FinishedLaunching na delegáta aplikace a informace oznámení budou předávána ve prostřednictvím parametr možnosti. Pokud možnosti slovník obsahuje klíč `UIApplication.LaunchOptionsLocalNotificationKey`, pak AppDelegate ví, že aplikace byla spuštěna z místního oznámení. Následující fragment kódu ukazuje tento proces:

```csharp
// check for a local notification
if (options.ContainsKey(UIApplication.LaunchOptionsLocalNotificationKey))
 {
     var localNotification = options[UIApplication.LaunchOptionsLocalNotificationKey] as UILocalNotification;
     if (localNotification != null)
     {
        UIAlertController okayAlertController = UIAlertController.Create (localNotification.AlertAction, localNotification.AlertBody, UIAlertControllerStyle.Alert);
        okayAlertController.AddAction (UIAlertAction.Create ("OK", UIAlertActionStyle.Default, null));
        viewController.PresentViewController (okayAlertController, true, null);

         // reset our badge
         UIApplication.SharedApplication.ApplicationIconBadgeNumber = 0;
     }
 }
```

Pokud je Vzdálená oznámení slovníku možnosti místo toho by obsahovala `LaunchOptionsRemoteNotificationKey`, a výslednou hodnotu pro tento klíč by `NSDictionary` objekt se datová část vzdáleného oznámení. Datová část oznámení prostřednictvím výstrahy, oznámení a zvukových klíčů můžete rozbalit. Následující fragment kódu ukazuje, jak získat vzdáleného oznámení:

```csharp
NSDictionary remoteNotification = options[UIApplication.LaunchOptionsRemoteNotificationKey];
if(remoteNotification != null)
{
    string alert = remoteNotification["alert"];
}
```

## <a name="summary"></a>Souhrn

V této části vám ukázal, jak vytvářet a publikovat oznámení v Xamarin.iOS. Ji zobrazit, jak může aplikace reagovat na oznámení přepsáním `ReceivedLocalNotification` metoda nebo `ReceivedRemoteNotification` metoda v `AppDelegate`.


## <a name="related-links"></a>Související odkazy

- [Místní oznámení (ukázka)](https://developer.xamarin.com/samples/monotouch/LocalNotifications)
- [Místní a nabízená oznámení pro vývojáře](https://developer.apple.com/notifications/)
- [Místních a nabízených oznámení Průvodce programováním](https://developer.apple.com/library/prerelease/content/documentation/NetworkingInternet/Conceptual/RemoteNotificationsPG/)
- [UIApplication](http://iosapi.xamarin.com/?link=T%3aMonoTouch.UIKit.UIApplication)
- [UILocalNotification](http://iosapi.xamarin.com/?link=T%3aMonoTouch.UIKit.UILocalNotification)
