---
title: "Oznámení o službách"
description: "Tato příručka popisuje, jak může služby Android pomocí místního oznámení odeslat informace o uživateli."
ms.topic: article
ms.prod: xamarin
ms.assetid: 6C06FDE7-6385-40EF-AC7C-8EFB54E29F45
ms.technology: xamarin-android
author: topgenorth
ms.author: toopge
ms.date: 02/16/2018
ms.openlocfilehash: 3c06fca9c6d8c3cd91889007bd1879149771622b
ms.sourcegitcommit: 0fdb243b46cf21be47584900805cadcd077121bf
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 03/12/2018
---
# <a name="service-notifications"></a>Oznámení o službách

_Tato příručka popisuje, jak může služby Android pomocí místního oznámení odeslat informace o uživateli._


## <a name="service-notifications-overview"></a>Přehled služby oznámení

Oznámení o službách povolí aplikaci zobrazíte informace o uživateli, i když není aplikace pro Android v popředí. Je možné pro oznámení zadejte akce pro uživatele, například zobrazení aktivity z aplikace. Následující příklad kódu ukazuje, jak služba může odeslat oznámení uživateli:

```csharp
[Service]
public class MyService: Service 
{
    // A notification requires an id that is unique to the application.
    const int NOTIFICATION_ID = 9000;
    
    public override StartCommandResult OnStartCommand(Intent intent, StartCommandFlags flags, int startId)
    {
        // Code omitted for clarity - here is where the service would do something.
    
        // Work has finished, now dispatch anotification to let the user know.
        Notification.Builder notificationBuilder = new Notification.Builder(this)
            .SetSmallIcon(Resource.Drawable.ic_notification_small_icon)
            .SetContentTitle(Resources.GetString(Resource.String.notification_content_title))
            .SetContentText(Resources.GetString(Resource.String.notification_content_text));
        
        var notificationManager = (NotificationManager)GetSystemService(NotificationService);
        notificationManager.Notify(NOTIFICATION_ID, notificationBuilder.Build());
    }
}
```

Tomto snímku obrazovky je příklad oznámení, který se zobrazí:

[![Ikona oznámení zobrazí ve stavovém řádku](service-notifications-images/01-notification-sml.png)](service-notifications-images/01-notification.png#lightbox)

Pokud uživatel snímky dolů z horní části obrazovky oznámení, zobrazí se úplné oznámení:

![Notication zobrazí na panelu oznámení](service-notifications-images/02-fullnotification.png)


## <a name="updating-a-notification"></a>Aktualizace oznámení

Aktualizace oznámení, bude služba znovu publikovat oznámení pomocí stejné ID oznámení. Android zobrazíte nebo aktualizovat oznámení ve stavovém řádku podle potřeby.

```csharp 
void UpdateNotification(string content)
{
   var notification = GetNotification(content, pendingIntent);

   NotificationManager notificationManager = (NotificationManager)GetSystemService(Context.NotificationService);
   notificationManager.Notify(NOTIFICATION_ID, notification);
}

Notification GetNotification(string content, PendingIntent intent)
{
   return new Notification.Builder(this)
           .SetContentTitle(tag)
           .SetContentText(content)
           .SetSmallIcon(Resource.Drawable.NotifyLg)
           .SetLargeIcon(BitmapFactory.DecodeResource(Resources, Resource.Drawable.Icon))
           .SetContentIntent(intent).Build();
}
```

Další informace o oznámeních je k dispozici v [místního oznámení](~/android/app-fundamentals/notifications/local-notifications.md) části [oznámení systému Android](~/android/app-fundamentals/notifications/index.md) průvodce.


## <a name="related-links"></a>Související odkazy

- [Místní oznámení v Android](~/android/app-fundamentals/notifications/local-notifications.md)
