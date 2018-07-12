---
title: Služby na popředí
ms.prod: xamarin
ms.assetid: C10FD999-7A91-4708-B642-0C1B0901BD24
ms.technology: xamarin-android
author: topgenorth
ms.author: toopge
ms.date: 03/19/2018
ms.openlocfilehash: 47e1eda2f701b654f81f664050847677fba8bcc5
ms.sourcegitcommit: be4da0cd7e1a915e3b8932a7e3d6bcd74c7055be
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/12/2018
ms.locfileid: "38986031"
---
# <a name="foreground-services"></a>Služby na popředí

Služba popředí je speciální typ vazby služby nebo spuštěná služba. V některých služeb provádět úlohy, které uživatelé musí být aktivně vědět, tyto služby jsou označovány jako _služby na popředí_. Příkladem popředí služby je aplikace, je tak uživatelům pokyny při zvyšování nebo procházení. I v případě, že aplikace je na pozadí, je důležité, že služba má dostatek prostředků fungovala správně a zda má uživatel přístup k aplikaci rychlý a užitečný způsob. Pro aplikace pro Android, to znamená, že popředí služba by měla přijímat vyšší prioritu než "pravidelné" a musí poskytovat službu popředí `Notification` , která Android se zobrazí za předpokladu, že služba běží.
 
Chcete-li spustit službu popředí, musí aplikace odeslání záměru, který dává pokyn s Androidem a spustit službu. Potom službu musí registrovat sama jako popředí službu s Androidem. Aplikace, které jsou spuštěny na Androidu 8.0 (nebo novějším) by měly používat `Context.StartForegroundService` metodu spustit službu, zatímco by měl používat aplikace, které jsou spuštěny na zařízení se starší verzí systému Android `Context.StartService`

Tato metoda rozšíření jazyka C# je příklad toho, jak spustit službu popředí. Na Androidu 8.0 a novější budou používat `StartForegroundService` metodu, jinak starší `StartService` bude použita metoda.  

```csharp
public static void StartForegroundServiceComapt<T>(this Context context, Bundle args = null) where T : Service
{
    var intent = new Intent(context, typeof(T));
    if (args != null) 
    {
        intent.PutExtras(args);
    }

    if (Android.OS.Build.VERSION.SdkInt >= Android.OS.BuildVersionCodes.O)
    {
        context.StartForegroundService(intent);
    }
    else
    {
        context.StartService(intent);
    }
}
```

## <a name="registering-as-a-foreground-service"></a>Registrace jako služba popředí

Po zahájení popředí služby se musí zaregistrovat samotné s Androidem vyvoláním [ `StartForeground` ](https://developer.xamarin.com/api/member/Android.App.Service.StartForeground/p/System.Int32/Android.App.Notification/). Pokud je služba spuštěna s `Service.StartForegroundService` metoda ale nezaregistruje sama, pak bude Android zastavte službu a označit aplikaci jako nereagující.

`StartForeground` přebírá dva parametry, které jsou povinné:
 
* Celočíselná hodnota, který je jedinečný v rámci aplikace k identifikaci služby.
* A `Notification` objekt, který Android se zobrazí ve stavovém řádku pro za předpokladu, že služba běží.

Android se zobrazí ve stavovém řádku pro oznámení, za předpokladu, že služba běží. Oznámení alespoň poskytne vizuální upozornění pro uživatele, na kterém běží služba. Oznámení v ideálním případě by měla poskytnout uživatele s místní aplikace nebo může být některá akce tlačítka ovládací prvek aplikace. Příkladem je hudební přehrávač &ndash; , který se zobrazí oznámení mohou mít tlačítka pro pozastavení/přehrávání hudby, zpět na předchozí skladby nebo přeskočit na další skladbu. 

Tento fragment kódu je příkladem registrace služby jako služba popředí:   

```csharp
// This is any integer value unique to the application.
public const int SERVICE_RUNNING_NOTIFICATION_ID = 10000;

public override StartCommandResult OnStartCommand(Intent intent, StartCommandFlags flags, int startId)
{
    // Code not directly related to publishing the notification has been omitted for clarity.
    // Normally, this method would hold the code to be run when the service is started.
    
    var notification = new Notification.Builder(this)
        .SetContentTitle(Resources.GetString(Resource.String.app_name))
        .SetContentText(Resources.GetString(Resource.String.notification_text))
        .SetSmallIcon(Resource.Drawable.ic_stat_name)
        .SetContentIntent(BuildIntentToShowMainActivity())
        .SetOngoing(true)
        .AddAction(BuildRestartTimerAction())
        .AddAction(BuildStopServiceAction())
        .Build();

    // Enlist this instance of the service as a foreground service
    StartForeground(SERVICE_RUNNING_NOTIFICATION_ID, notification);
}
```

Předchozí oznámení se zobrazí stav panel oznámení, který je podobný následujícímu:

![Obrázek znázorňující oznámení ve stavovém řádku](foreground-services-images/foreground-services-01.png "obrázek znázorňující oznámení ve stavovém řádku")

Tento snímek obrazovky ukazuje rozšířené oznámení v oznamovací oblasti se dvě akce, které uživateli umožňují řízení služby:

![Obrázek znázorňující rozšířené oznámení](foreground-services-images/foreground-services-02.png "obrázek znázorňující rozšířené oznámení.")

Další informace o oznámeních je k dispozici v [místní oznámení](~/android/app-fundamentals/notifications/local-notifications.md) část [oznámení systému Android](~/android/app-fundamentals/notifications/index.md) průvodce.

## <a name="unregistering-as-a-foreground-service"></a>Ruší se registrace jako služba popředí

Službu můžete zrušit seznamu sám jako služba popředí voláním metody `StopForeground`. `StopForeground` nebude zastavit službu, ale dojde k odstranění ikonu oznámení a signály Androidu, který tuto službu můžete vypnout v případě potřeby.

Upozornění na stavovém řádku, který se zobrazí může být odebrán také předáním `true` metody: 

```csharp
StopForeground(true);
```

Pokud služba je zastaveno voláním `StopSelf` nebo `StopService`, odeberou upozornění na stavovém řádku.

## <a name="related-links"></a>Související odkazy

- [Android.App.Service](https://developer.xamarin.com/api/type/Android.App.Service/)
- [Android.App.Service.StartForeground](https://developer.xamarin.com/api/member/Android.App.Service.StartForeground/p/System.Int32/Android.App.Notification/)
- [Místní oznámení](~/android/app-fundamentals/notifications/local-notifications.md)
- [ForegroundServiceDemo (ukázka)](https://developer.xamarin.com/samples/monodroid/ApplicationFundamentals/ServiceSamples/ForegroundServiceDemo/)
