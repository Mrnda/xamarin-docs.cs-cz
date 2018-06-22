---
title: Služby popředí
ms.prod: xamarin
ms.assetid: C10FD999-7A91-4708-B642-0C1B0901BD24
ms.technology: xamarin-android
author: topgenorth
ms.author: toopge
ms.date: 03/19/2018
ms.openlocfilehash: 3088fa4b5cfa21ac57533ef331ffcc15414e14b4
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/04/2018
ms.locfileid: "30763745"
---
# <a name="foreground-services"></a>Služby popředí

Služba popředí je speciální typu vázané služby nebo spuštěná služba. Příležitostně služby bude provádět úlohy, které uživatelé musí být aktivně vědět, tyto služby se označují jako _popředí služby_. Příklad služby popředí je aplikace, která poskytuje uživatele pokynů při řízení nebo procházení. I v případě, že aplikace je na pozadí, je však důležité, zda má služba dostatek prostředků ke fungovat správně a zda má uživatel rychlý a snadný způsob, jak přístup k aplikaci. Pro aplikace pro Android, to znamená, že popředí služby by měly dostávat vyšší prioritu než "Regulérní" služba a služba popředí musíte zadat `Notification` který Android se zobrazí, dokud je služba spuštěná.
 
Chcete-li spustit službu popředí, musí aplikace odesílání záměrem s informacemi o Android se spustit službu. Potom služba musí se registrovat jako služba popředí s Android. Aplikace, které jsou spuštěny na Android 8.0 (nebo vyšší) by měl používat `Context.StartForegroundService` se spustit službu, při by měl používat aplikace, které jsou spuštěny na zařízení se starší verzí systému Android – metoda `Context.StartService`

Tato metoda rozšíření C# je příklad toho, jak spustit službu popředí. Na Android 8.0 a vyšší se bude používat `StartForegroundService` metoda, jinak hodnota starší `StartService` metoda se použije.  

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

Po zahájení popředí služby, se musí se registrovat s Android vyvoláním [ `StartForeground` ](https://developer.xamarin.com/api/member/Android.App.Service.StartForeground/p/System.Int32/Android.App.Notification/). Pokud je služba spuštěna s `Service.StartForegroundService` metoda ale nezaregistruje samostatně, pak bude Android zastavit službu a příznak aplikace jako přestane reagovat.

`StartForeground` přebírá dva parametry, které jsou povinné:
 
* Celočíselná hodnota, která je jedinečná v rámci aplikace k identifikaci služby.
* A `Notification` objekt, který Android se zobrazí ve stavovém řádku pro tak dlouho, dokud je služba spuštěná.

Android se zobrazí oznámení ve stavovém řádku pro tak dlouho, dokud je služba spuštěná. Oznámení, minimálně, bude poskytovat vizuální upozornění uživatele, který je služba spuštěná. Oznámení v ideálním případě by mělo poskytovat uživatele s zástupce aplikace nebo může být některá akce tlačítka pro řízení aplikace. Příkladem je hudební přehrávač &ndash; oznámení, které se zobrazí může mít tlačítka na pozastavení nebo play Hudba, rewind předchozí skladbu, nebo přejděte na další skladbu. 

Tento fragment kódu je příklad registrace služby jako služba popředí:   

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

Předchozí oznámení se zobrazí oznámení panelu Stav, který je podobný následujícímu:

![Obrázek znázorňující oznámení ve stavovém řádku](foreground-services-images/foreground-services-01.png "obrázek znázorňující oznámení ve stavovém řádku")

Tento snímek obrazovky ukazuje rozšířené oznámení na hlavním panelu oznámení s dvě akce, které umožňují uživateli řídit službu:

![Obrázek znázorňující rozšířené oznámení](foreground-services-images/foreground-services-02.png "obrázek znázorňující rozšířené oznámení.")

Další informace o oznámeních je k dispozici v [místního oznámení](~/android/app-fundamentals/notifications/local-notifications.md) části [oznámení systému Android](~/android/app-fundamentals/notifications/index.md) průvodce.

## <a name="unregistering-as-a-foreground-service"></a>Zrušení registrace jako služba popředí

Službu můžete zrušte seznamu sám sebe jako služba popředí voláním metody `StopForeground`. `StopForeground` nebude zastavit službu, ale dojde k odebrání ikonu oznámení a signály Android, která tuto službu můžete vypnout v případě potřeby.

Oznámení o panelu stavu, který se zobrazí může být odebrán také předáním `true` metody: 

```csharp
StopForeground(true);
```

Pokud je službu zastavit pomocí volání `StopSelf` nebo `StopService`, odeberou se panelu oznámení o stavu.

## <a name="related-links"></a>Související odkazy

- [Android.App.Service](https://developer.xamarin.com/api/type/Android.App.Service/)
- [Android.App.Service.StartForegrond](https://developer.xamarin.com/api/member/Android.App.Service.StartForeground/p/System.Int32/Android.App.Notification/)
- [Místní oznámení](~/android/app-fundamentals/notifications/local-notifications.md)
- [ForegroundServiceDemo (ukázka)](https://developer.xamarin.com/samples/monodroid/ApplicationFundamentals/ServiceSamples/ForegroundServiceDemo/)
