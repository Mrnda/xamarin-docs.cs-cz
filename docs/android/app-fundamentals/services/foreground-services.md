---
title: "Služby popředí"
ms.topic: article
ms.prod: xamarin
ms.assetid: C10FD999-7A91-4708-B642-0C1B0901BD24
ms.technology: xamarin-android
author: topgenorth
ms.author: toopge
ms.date: 03/09/2018
ms.openlocfilehash: 96e8d1a3658a515b6b1d37cf0fdd93157954c01d
ms.sourcegitcommit: 0fdb243b46cf21be47584900805cadcd077121bf
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 03/12/2018
---
# <a name="foreground-services"></a>Služby popředí

Některé služby fungují některé úlohy, které jsou uživatelé aktivně vědět, tyto služby se označují jako _popředí služby_. Příklad služby popředí je aplikace, která poskytuje uživatele pokynů při řízení nebo procházení. I v případě, že aplikace je na pozadí, je však důležité, zda má služba dostatek prostředků ke fungovat správně a zda má uživatel rychlý a snadný způsob, jak přístup k aplikaci. Pro aplikace pro Android, to znamená, že popředí služby by měly dostávat vyšší prioritu než "Regulérní" služba a služba popředí musíte zadat `Notification` který Android se zobrazí, dokud je služba spuštěná.
 
Popředí služby je vytvořena a spuštěna stejně jako jakoukoli jinou službu. Při spuštění služby, bude sám zaregistruje v Android jako služba popředí.
 
Tato příručka popisuje další kroky, které musí být přijata zaregistrovat službu popředí a zastavit službu, pokud se provádí.

## <a name="registering-as-a-foreground-service"></a>Registrace jako služba popředí

Služba popředí je speciální typu vázané služby nebo spuštěná služba. Služby, pokud již bylo spuštěno, zavolá [ `StartForeground` ](https://developer.xamarin.com/api/member/Android.App.Service.StartForeground/p/System.Int32/Android.App.Notification/) metoda k registraci v Android jako služba popředí.   

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

Pokud je službu zastavit pomocí volání `StopSelf` nebo `StopService`, pak oznámení panelu Stav se taky odeberou.


## <a name="related-links"></a>Související odkazy

- [Android.App.Service](https://developer.xamarin.com/api/type/Android.App.Service/)
- [Android.App.Service.StartForegrond](https://developer.xamarin.com/api/member/Android.App.Service.StartForeground/p/System.Int32/Android.App.Notification/)
- [Místní oznámení](~/android/app-fundamentals/notifications/local-notifications.md)
- [ForegroundServiceDemo (ukázka)](https://developer.xamarin.com/samples/monodroid/ApplicationFundamentals/ServiceSamples/ForegroundServiceDemo/)
