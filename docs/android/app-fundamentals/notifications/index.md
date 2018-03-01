---
title: "Oznámení v Xamarin.Android"
ms.topic: article
ms.prod: xamarin
ms.assetid: 2E54F1D0-45F4-43A7-B3A3-4F483B7150CB
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/16/2018
ms.openlocfilehash: 0dbf8c32ca7b7565105c01cfaa077fe792b09b18
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 02/27/2018
---
# <a name="notifications-in-xamarinandroid"></a>Oznámení v Xamarin.Android

<a name="Overview" />

## <a name="overview"></a>Přehled

V této části ukazuje, jak implementovat oznámení v Xamarin.Android.
To vysvětluje různé prvky uživatelského rozhraní Android oznámení a popisuje rozhraní API je zahrnuta s vytváření a zobrazování oznámení.

<a name="Sections" />

## <a name="sections"></a>Oddíly

### <a name="local-notifications-in-androidlocal-notificationsmd"></a>[Místní oznámení v Android](local-notifications.md)

Tato část vysvětluje, jak implementovat místní oznámení v Xamarin.Android. Popisuje různé prvky uživatelského rozhraní Android oznámení a popisují rozhraní API je zahrnuta s vytváření a zobrazování oznámení. 

### <a name="walkthrough---using-local-notifications-in-xamarinandroidlocal-notifications-walkthroughmd"></a>[Návod - použití místního oznámení v Xamarin.Android](local-notifications-walkthrough.md)  
 
Tento návod popisuje postup použití místního oznámení v aplikaci Xamarin.Android. Ukazuje základní informace o vytváření a publikování oznámení. Když uživatel klikne na oznámení v panel oznámení spuštění druhá aktivita. 


## <a name="for-further-reading"></a>Pro další čtení

[Zasílání zpráv cloudu firebase](~/android/data-cloud/google-messaging/firebase-cloud-messaging.md) &ndash; zasílání zpráv cloudu Firebase (FCM) je služba, která usnadňuje zasílání zpráv mezi mobilní aplikace a aplikace serveru. Zasílání zpráv cloudu firebase lze použít k implementaci vzdáleného oznámení (také nazývané nabízená oznámení) v aplikacích Xamarin.Android.

[Oznámení](http://developer.android.com/guide/topics/ui/notifiers/notifications.html) &ndash; tento Android Developer téma je Průvodce spolehlivý pro oznámení systému Android. Zahrnuje návrh část aspekty, které vám pomůže navrhnout oznámení tak, že jsou v souladu s pokyny Android uživatelského rozhraní. Poskytuje podrobnější informace o preserviing navigační při spouštění aktivitu, a vysvětluje postup zobrazení průběhu oznámení a řízení přehrávání médií na zamykací obrazovce. 

[NotificationListenerService](https://developer.xamarin.com/api/type/Android.Service.Notification.NotificationListenerService/) &ndash; tento Android služby umožňuje vaší aplikace pro naslouchání na (a interakci s) všechna oznámení odeslat na zařízení Android, ne jenom oznámení, která je zaregistrován v aplikaci dostávat. Všimněte si, že uživatel musí explicitně udělit oprávnění k aplikaci pro něj být schopni čekat na oznámení na zařízení.





## <a name="related-links"></a>Související odkazy

- [Místní oznámení (ukázka)](https://developer.xamarin.com/samples/monodroid/LocalNotifications/)
- [Vzdálená oznámení (ukázka)](https://developer.xamarin.com/samples/monodroid/RemoteNotifications/)
