---
title: Odesílání nabízených oznámení z Azure Mobile Apps
description: Azure Notification Hubs poskytuje infrastrukturu škálovatelné nabízených pro odesílání mobilní nabízená oznámení z jakéhokoli back-endu na libovolnou mobilní platformu, přičemž složitost back-end museli komunikovat s různých systémů oznámení platforem. Tento článek vysvětluje, jak používat Azure Notification Hubs k odesílání nabízených oznámení z instance Azure Mobile Apps na platformě Xamarin.Forms aplikaci.
ms.prod: xamarin
ms.assetid: A1EF400F-73F4-43E9-A0C3-1569A0F34A3B
ms.technology: xamarin-forms
ms.custom: xamu-video
author: davidbritch
ms.author: dabritch
ms.date: 11/02/2017
ms.openlocfilehash: cff0b85514d2e5995d09735d6ad99b7909bfacb4
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/04/2018
---
# <a name="sending-push-notifications-from-azure-mobile-apps"></a>Odesílání nabízených oznámení z Azure Mobile Apps

_Azure Notification Hubs poskytuje infrastrukturu škálovatelné nabízených pro odesílání mobilní nabízená oznámení z jakéhokoli back-endu na libovolnou mobilní platformu, přičemž složitost back-end museli komunikovat s různých systémů oznámení platforem. Tento článek vysvětluje, jak používat Azure Notification Hubs k odesílání nabízených oznámení z instance Azure Mobile Apps na platformě Xamarin.Forms aplikaci._

> [!VIDEO https://youtube.com/embed/le2lDY22xwM]

**Azure Push pomocí centra oznámení a Xamarin.Forms, [univerzity Xamarin](https://university.xamarin.com/)**

Nabízená oznámení se používá k poskytování informací, jako je například zprávu, ze systému back-end aplikace na mobilní zařízení zvýšit využití a zapojení aplikace. Oznámení můžete odeslat na kdykoli, i když se uživatel není aktivně používá cílové aplikace.

Back-end systémy odesílání nabízených oznámení do mobilních zařízení prostřednictvím platformy oznámení systémy (PNS), jak je znázorněno v následujícím diagramu:

[![](azure-images/pns.png "Systémy oznámení platforem")](azure-images/pns-large.png#lightbox "systémy oznámení platforem")

K odesílání nabízených oznámení, back-end systém kontaktuje systém PNS specifické pro platformu odeslat oznámení do instance aplikace klienta. To výrazně zvyšuje složitost back-end napříč platformami nabízená oznámení jsou povinné, protože back-end musíte použít každý specifické pro platformu rozhraní API systému PNS a protokolu.

Azure Notification Hubs eliminovat této složitost podle detailním různých systémů oznámení platforem, což napříč platformami oznámení k odeslání na základě jednoho volání rozhraní API, jak je znázorněno v následujícím diagramu:

[![](azure-images/notification-hub.png)](azure-images/notification-hub-large.png#lightbox)

K odesílání nabízených oznámení, back-end systému jenom kontakty do centra oznámení Azure, která dále komunikuje se různých systémů oznámení platforem, proto snížení složitosti back-end kódu této odesílá nabízená oznámení.

Azure Mobile Apps mít integrovanou podporu pro nabízená oznámení pomocí centra oznámení. Proces pro odesílání nabízených oznámení z instance Azure Mobile Apps k aplikaci Xamarin.Forms vypadá takto:

1. Zaregistruje aplikaci Xamarin.Forms se Správou, která vrátí popisovač.
1. Instance Azure Mobile Apps odešle oznámení do jeho centra oznámení Azure určující popisovač zařízení je možné cílit.
1. Centra oznámení Azure odešle oznámení do příslušné Správou zařízení.
1. Systém PNS odešle oznámení do zadaného zařízení.
1. Aplikaci Xamarin.Forms zpracovává oznámení a zobrazí.

Ukázkovou aplikaci ukazuje, jejichž data jsou uložena v instanci Azure Mobile Apps aplikaci seznamu úkolů. Pokaždé, když se k instanci Azure Mobile Apps při přidání nové položky, nabízených oznámení se posílá aplikaci Xamarin.Forms. Na následujících snímcích obrazovky zobrazit jednotlivé platformy zobrazení přijaté nabízených oznámení:

[![](azure-images/screenshots.png "Ukázková aplikace příjem nabízených oznámení")](azure-images/screenshots-large.png#lightbox "ukázkové aplikace příjem nabízených oznámení")

Další informace o Azure Notification Hubs najdete v tématu [Azure Notification Hubs](https://azure.microsoft.com/documentation/articles/notification-hubs-push-notification-overview/) a [přidat nabízená oznámení do vaší aplikace na platformě Xamarin.Forms](/azure/app-service-mobile/app-service-mobile-xamarin-forms-get-started-push/).

## <a name="azure-and-platform-notification-system-setup"></a>Azure a nastavení systému oznámení platformy

Proces pro integraci centra oznámení Azure do Azure Mobile Apps instance vypadá takto:

1. Vytvoření instance Azure Mobile Apps. Další informace najdete v tématu [využívání služby Azure Mobile Apps](~/xamarin-forms/data-cloud/consuming/azure.md).
1. Konfigurace centra oznámení. Další informace najdete v tématu [Konfigurace centra oznámení](/azure/app-service-mobile/app-service-mobile-xamarin-forms-get-started-push#create-hub).
1. Aktualizace instance Azure Mobile Apps k odesílání nabízených oznámení. Další informace najdete v tématu [aktualizovat serverový projekt k odesílání nabízených oznámení](/azure/app-service-mobile/app-service-mobile-xamarin-forms-get-started-push#update-the-server-project-to-send-push-notifications).
1. Zaregistrujte se na každý systém PNS.
1. Konfigurace centra oznámení pro komunikaci s každou systém PNS.

Následující části poskytují pokyny další nastavení pro každou platformu.

### <a name="ios"></a>iOS

Následující kroky se musí provést používat oznámení služby APNS (Apple Push) z centra oznámení Azure:

1. Vygenerujte certifikát Podepisování žádost o certifikát push pomocí nástroje přístup do řetězce klíčů. Další informace najdete v tématu [vytvořit soubor žádost Certificate Signing Request pro certifikát push](https://azure.microsoft.com/documentation/articles/app-service-mobile-xamarin-forms-get-started-push/#generate-the-certificate-signing-request-file-for-the-push-certificate) v centru dokumentace Azure.
1. Zaregistrujte aplikaci Xamarin.Forms pro podporu nabízených oznámení v Apple Developer Center. Další informace najdete v tématu [registrace aplikace pro nabízená oznámení](https://azure.microsoft.com/documentation/articles/app-service-mobile-xamarin-forms-get-started-push/#register-your-app-for-push-notifications) v centru dokumentace Azure.
1. Vytvořte nabízená oznámení povoleno profil pro zřizování pro aplikaci Xamarin.Forms v Centru pro vývojáře Apple. Další informace najdete v tématu [vytvořit profil pro zřizování pro aplikace](https://azure.microsoft.com/documentation/articles/app-service-mobile-xamarin-forms-get-started-push/#create-a-provisioning-profile-for-the-app) v centru dokumentace Azure.
1. Konfigurace centra oznámení pro komunikaci s služby APN. Další informace najdete v tématu [Konfigurace centra oznámení pro služby APN](https://azure.microsoft.com/documentation/articles/app-service-mobile-xamarin-forms-get-started-push/#configure-the-notification-hub-for-apns).
1. Nakonfigurujte aplikaci Xamarin.Forms, chcete-li použít nové ID aplikace a profil zřizování. Další informace najdete v tématu [konfigurace projektu pro iOS v Xamarin studiu](https://azure.microsoft.com/documentation/articles/app-service-mobile-xamarin-forms-get-started-push/#configuring-the-ios-project-in-xamarin-studio) nebo [konfigurace projektu pro iOS v sadě Visual Studio](https://azure.microsoft.com/documentation/articles/app-service-mobile-xamarin-forms-get-started-push/#configuring-the-ios-project-in-visual-studio) v centru dokumentace Azure.

### <a name="android"></a>Android

Následující kroky se musí provést používat Firebase cloudu zasílání zpráv (FCM) z centra oznámení Azure:

1. Registrace pro FCM. Klíč rozhraní API serveru a ID klienta jsou automaticky generovány a mnoha funkcemi `google-services.json` soubor, který byl stažen. Další informace najdete v tématu [povolit Firebase cloudu zasílání zpráv (FCM)](/azure/app-service-mobile/app-service-mobile-xamarin-forms-get-started-push#enable-firebase-cloud-messaging-fcm).
1. Konfigurace centra oznámení pro komunikaci s FCM. Další informace najdete v tématu [konfigurovat end Mobile Apps zpět k posílání požadavků nabízené pomocí FCM](/azure/app-service-mobile/app-service-mobile-xamarin-forms-get-started-push#configure-the-mobile-apps-back-end-to-send-push-requests-by-using-fcm).

### <a name="universal-windows-platform"></a>Univerzální platforma pro Windows

Použití služby oznámení Windows (WNS) z centra oznámení Azure musí být provedena následující kroky:

1. Registrace pro oznámení službu systému Windows (WNS). Další informace najdete v tématu [registraci aplikace pro Windows pro nabízená oznámení WNS](https://azure.microsoft.com/documentation/articles/app-service-mobile-xamarin-forms-get-started-push/#register-your-windows-app-for-push-notifications-with-wns) v centru dokumentace Azure.
1. Konfigurace centra oznámení pro komunikaci s WNS. Další informace najdete v tématu [Konfigurace centra oznámení pro WNS](https://azure.microsoft.com/documentation/articles/app-service-mobile-xamarin-forms-get-started-push/#configure-the-notification-hub-for-wns) v centru dokumentace Azure.

## <a name="adding-push-notification-support-to-the-xamarinforms-application"></a>Přidání podpory nabízených oznámení do aplikace Xamarin.Forms

Následující části popisují implementaci potřebné v každém projektu specifické pro platformu pro podporu nabízených oznámení.

### <a name="ios"></a>iOS

Proces pro implementaci podporu nabízených oznámení v aplikaci pro systém iOS vypadá takto:

1. Zaregistrovat se APNS Apple Push Notification Service () v `AppDelegate.FinishedLaunching` metoda. Další informace najdete v tématu [registraci pomocí Apple Push Notification systému](#ios_register).
1. Implementace `AppDelegate.RegisteredForRemoteNotifications` metodu ke zpracování odpovědi registrace. Další informace najdete v tématu [zpracování odpovědi registrace](#ios_registration_response).
1. Implementace `AppDelegate.DidReceiveRemoteNotification` metoda ke zpracování příchozích nabízená oznámení. Další informace najdete v tématu [zpracování příchozí nabízená oznámení](#ios_process_incoming).

<a name="ios_register" />

### <a name="registering-with-the-apple-push-notification-service"></a>Registrace Apple Push Notification Service

Předtím, než aplikace pro iOS můžete přijímat nabízená oznámení, zaregistrujte se nabízených oznámení APNS (Apple Service), tím se vygenerování tokenu jedinečný zařízení a vrátit se do aplikace. Registrace je volána v `FinishedLaunching` potlačení v `AppDelegate` třídy:

```csharp
public override bool FinishedLaunching(UIApplication app, NSDictionary options)
{
    ...
    var settings = UIUserNotificationSettings.GetSettingsForTypes(
        UIUserNotificationType.Alert, new NSSet());

    UIApplication.SharedApplication.RegisterUserNotificationSettings(settings);
    UIApplication.SharedApplication.RegisterForRemoteNotifications();
    ...
}
```

Když aplikace pro iOS zaregistruje u služby APN musí určit typy nabízená oznámení, které se chcete dostávat. `RegisterUserNotificationSettings` Metoda registruje typy oznámení, které aplikace může přijímat, pomocí `RegisterForRemoteNotifications` metoda registrace přijímat nabízená oznámení ze služby APN.

> [!NOTE]
> Nedaří se volání `RegisterUserNotificationSettings` metoda bude mít za následek nabízená oznámení se bezobslužně přijatých aplikací.

<a name="ios_registration_response" />

### <a name="handling-the-registration-response"></a>Zpracování odpovědi registrace

Žádost o registraci služby APN probíhá na pozadí. Po přijetí odpovědi bude volat iOS `RegisteredForRemoteNotifications` potlačení v `AppDelegate` třídy:

```csharp
public override void RegisteredForRemoteNotifications(UIApplication application, NSData deviceToken)
{
    const string templateBodyAPNS = "{\"aps\":{\"alert\":\"$(messageParam)\"}}";

    JObject templates = new JObject();
    templates["genericMessage"] = new JObject
    {
        {"body", templateBodyAPNS}
    };

    // Register for push with the Azure mobile app
    Push push = TodoItemManager.DefaultManager.CurrentClient.GetPush();
    push.RegisterAsync(deviceToken, templates);
}
```

Tato metoda vytvoří šablonu oznámení jednoduché zprávy jako JSON a zaregistruje zařízení přijímat šablony oznámení z centra oznámení.

> [!NOTE]
> `FailedToRegisterForRemoteNotifications` Přepsání by měla být implementována pro řešení situací jako chybějící síťové připojení. To je důležité, protože uživatelé mohou spustit aplikaci při offline.

<a name="ios_process_incoming" />

### <a name="processing-incoming-push-notifications"></a>Zpracování příchozích nabízená oznámení

`DidReceiveRemoteNotification` Potlačení v `AppDelegate` třída se používá ke zpracování příchozích nabízená oznámení, když aplikace běží a je volána, když je přijato oznámení:

```csharp
public override void DidReceiveRemoteNotification(
    UIApplication application, NSDictionary userInfo, Action<UIBackgroundFetchResult> completionHandler)
{
    NSDictionary aps = userInfo.ObjectForKey(new NSString("aps")) as NSDictionary;

    string alert = string.Empty;
    if (aps.ContainsKey(new NSString("alert")))
        alert = (aps[new NSString("alert")] as NSString).ToString();

    // Show alert
    if (!string.IsNullOrEmpty(alert))
    {
      var notificationAlert = UIAlertController.Create("Notification", alert, UIAlertControllerStyle.Alert);
      notificationAlert.AddAction(UIAlertAction.Create("OK", UIAlertActionStyle.Cancel, null));
      UIApplication.SharedApplication.KeyWindow.RootViewController.PresentViewController(notificationAlert, true, null);
    }
}
```

`userInfo` Slovník obsahuje `aps` klíč, jehož hodnota je `alert` slovníku pomocí zbývající data oznámení. Tento adresář se načítají, s `string` oznámení se zobrazí v dialogovém okně.

> [!NOTE]
> Pokud aplikace není spuštěn, když dorazí nabízených oznámení, spustí se aplikace ale `DidReceiveRemoteNotification` metoda nezpracuje oznámení. Místo toho získat datová část oznámení a reagují odpovídajícím způsobem z `WillFinishLaunching` nebo `FinishedLaunching` přepsání.

Další informace o APNS najdete v tématu [nabízených oznámení v iOS](~/ios/platform/user-notifications/deprecated/remote-notifications-in-ios.md).

### <a name="android"></a>Android

Proces pro implementaci podporu nabízených oznámení v aplikaci pro Android je následující:

1. Přidat [Xamarin.Firebase.Messaging](https://www.nuget.org/packages/Xamarin.Firebase.Messaging/) NuGet balíčku do projektu pro Android a nastavte verzi cílové aplikace pro Android 7.0 nebo vyšší.
1. Přidat `google-services.json` soubor stáhnout z konzoly Firebase do kořenového adresáře projektu pro Android a nastavit jeho procesu sestavení na **GoogleServicesJson**. Další informace najdete v tématu [přidejte soubor JSON služby Google](~/android/data-cloud/google-messaging/remote-notifications-with-fcm.md).
1. Registrace pomocí zasílání zpráv cloudu Firebase (FCM) deklarováním příjemce v manifestu systému Android soubor a tím implementace `FirebaseRegistrationService.OnTokenRefresh` metoda. Další informace najdete v tématu [registrace zasílání zpráv cloudu Firebase](#android_register_fcm).
1. Zaregistrujte se do centra oznámení Azure v `AzureNotificationHubService.RegisterAsync` metoda. Další informace najdete v tématu [registraci do centra oznámení Azure](#android_register_azure).
1. Implementace `FirebaseNotificationService.OnMessageReceived` metoda ke zpracování příchozích nabízená oznámení. Další informace najdete v tématu [zobrazení obsahu nabízená oznámení](#android_displaying_notification).

Další informace o zasílání zpráv cloudu Firebase najdete v tématu [zasílání zpráv cloudu Firebase](~/android/data-cloud/google-messaging/firebase-cloud-messaging.md) a [vzdáleného oznámení s zasílání zpráv cloudu Firebase](~/android/data-cloud/google-messaging/remote-notifications-with-fcm.md).

<a name="android_register_fcm" />

#### <a name="registering-with-firebase-cloud-messaging"></a>Registrace Firebase cloudu zasílání zpráv

Předtím, než aplikace platformy Android může přijímat nabízená oznámení, musíte zaregistrovat u FCM, který bude generovat registrační token a vrátit se do aplikace. Další informace o registraci tokeny najdete v tématu [registraci FCM](~/android/data-cloud/google-messaging/firebase-cloud-messaging.md#registration).

To lze provést:

- Deklarování příjemce v Android manifest. Další informace najdete v tématu [deklarace příjemce v Android Manifest](#declaring_a_receiver).
- Implementace služby ID Instance Firebase. Další informace najdete v tématu [implementace služby ID Instance Firebase](#implementing-firebase-instance-id-service).

<a name="declaring_a_receiver" />

##### <a name="declaring-the-receiver-in-the-android-manifest"></a>Deklarování příjemce v manifestu systému Android.

Upravit **AndroidManifest.xml** a vložte následující `<receiver>` elementy do `<application>` element:

```xml
<receiver android:name="com.google.firebase.iid.FirebaseInstanceIdInternalReceiver" android:exported="false" />
<receiver android:name="com.google.firebase.iid.FirebaseInstanceIdReceiver" android:exported="true" android:permission="com.google.android.c2dm.permission.SEND">
  <intent-filter>
    <action android:name="com.google.android.c2dm.intent.RECEIVE" />
    <action android:name="com.google.android.c2dm.intent.REGISTRATION" />
    <category android:name="${applicationId}" />
  </intent-filter>
</receiver>
```

Tato konfigurace XML provede následující operace:

- Deklaruje interní `FirebaseInstanceIdInternalReceiver` implementace, která se používá ke spuštění služby bezpečně.
- Deklaruje `FirebaseInstanceIdReceiver` implementace, která poskytuje jedinečný identifikátor pro každou instanci aplikace. Tento příjemce také ověří a autorizuje akce.

`FirebaseInstanceIdReceiver` Obdrží `FirebaseInstanceId` a `FirebaseMessaging` události a zajišťuje jejich na třídu, která je odvozena z `FirebasesInstanceIdService`.

<a name="implementing-firebase-instance-id-service" />

##### <a name="implementing-the-firebase-instance-id-service"></a>Implementace služby Firebase instanci ID

Registrace aplikace FCM je dosáhnout odvozením třídy od `FirebaseInstanceIdService` třídy. Tato třída je zodpovědný za generování tokenů zabezpečení, které klient aplikaci autorizovat pro přístup k FCM. V ukázkové aplikaci `FirebaseRegistrationService` třída odvozená z `FirebaseInstanceIdService` třídy a je znázorněno v následujícím příkladu kódu:

```csharp
[Service]
[IntentFilter(new[] { "com.google.firebase.INSTANCE_ID_EVENT" })]
public class FirebaseRegistrationService : FirebaseInstanceIdService
{
    const string TAG = "FirebaseRegistrationService";

    public override void OnTokenRefresh()
    {
        var refreshedToken = FirebaseInstanceId.Instance.Token;
        Log.Debug(TAG, "Refreshed token: " + refreshedToken);
        SendRegistrationTokenToAzureNotificationHub(refreshedToken);
    }

    void SendRegistrationTokenToAzureNotificationHub(string token)
    {
        // Update notification hub registration
        Task.Run(async () =>
        {
            await AzureNotificationHubService.RegisterAsync(TodoItemManager.DefaultManager.CurrentClient.GetPush(), token);
        });
    }
}
```

`OnTokenRefresh` Metoda je volána, když aplikace přijímá token registrace z FCM. Metoda načte tokenu z `FirebaseInstanceId.Instance.Token` vlastnost, která se asynchronně aktualizuje pomocí FCM. `OnTokenRefresh` Zřídka zavolání metody, protože token je aktualizovány pouze v případě, že aplikace je nainstalována nebo odinstalována, když uživatel odstraní data aplikací, když aplikace vymaže Instance ID, nebo když byl zabezpečení tokenu ohrožení zabezpečení. Kromě toho FCM Instance ID služby bude požadovat, aby aplikace aktualizuje jeho token pravidelně obvykle každých 6 měsíců.

`OnTokenRefresh` Také vyvolá metoda `SendRegistrationTokenToAzureNotificationHub` metodu, která se používá k přidružení tokenu registrace uživatele do centra oznámení Azure.

<a name="android_register_azure" />

#### <a name="registering-with-the-azure-notification-hub"></a>Registrace do centra oznámení Azure

`AzureNotificationHubService` Třída poskytuje `RegisterAsync` metodu, která přidruží token uživatele registraci do centra oznámení Azure. Následující příklad kódu ukazuje `RegisterAsync` metoda, který lze vyvolat pomocí `FirebaseRegistrationService` třídy při změně token registrace uživatele:

```csharp
public class AzureNotificationHubService
{
    const string TAG = "AzureNotificationHubService";

    public static async Task RegisterAsync(Push push, string token)
    {
        try
        {
            const string templateBody = "{\"data\":{\"message\":\"$(messageParam)\"}}";
            JObject templates = new JObject();
            templates["genericMessage"] = new JObject
            {
                {"body", templateBody}
            };

            await push.RegisterAsync(token, templates);
            Log.Info("Push Installation Id: ", push.InstallationId.ToString());
        }
        catch (Exception ex)
        {
            Log.Error(TAG, "Could not register with Notification Hub: " + ex.Message);
        }
    }
}
```

Tato metoda vytvoří šablonu oznámení jednoduché zprávy jako JSON a zaregistruje se pro příjem oznámení šablony z centra oznámení, pomocí tokenu registrace Firebase. Tím se zajistí, že zařízení reprezentována registrační token se zaměří na všechny oznámení odesílaná z centra oznámení Azure.

<a name="android_displaying_notification" />

#### <a name="displaying-the-contents-of-a-push-notification"></a>Zobrazení obsahu nabízených oznámení

Zobrazení obsahu nabízených oznámení je dosáhnout odvozením třídy od `FirebaseMessagingService` třídy. Tato třída obsahuje přepisovatelné `OnMessageReceived` metoda, která se vyvolá, když aplikace obdrží oznámení od FCM, zadat, že je aplikace spuštěná v popředí. V ukázkové aplikaci `FirebaseNotificationService` třída odvozená z `FirebaseMessagingService` třídy a je znázorněno v následujícím příkladu kódu:

```csharp
[Service]
[IntentFilter(new[] { "com.google.firebase.MESSAGING_EVENT" })]
public class FirebaseNotificationService : FirebaseMessagingService
{
    const string TAG = "FirebaseNotificationService";

    public override void OnMessageReceived(RemoteMessage message)
    {
        Log.Debug(TAG, "From: " + message.From);

        // Pull message body out of the template
        var messageBody = message.Data["message"];
        if (string.IsNullOrWhiteSpace(messageBody))
            return;

        Log.Debug(TAG, "Notification message body: " + messageBody);
        SendNotification(messageBody);
    }

    void SendNotification(string messageBody)
    {
        var intent = new Intent(this, typeof(MainActivity));
        intent.AddFlags(ActivityFlags.ClearTop);
        var pendingIntent = PendingIntent.GetActivity(this, 0, intent, PendingIntentFlags.OneShot);

        var notificationBuilder = new NotificationCompat.Builder(this)
            .SetSmallIcon(Resource.Drawable.ic_stat_ic_notification)
            .SetContentTitle("New Todo Item")
            .SetContentText(messageBody)
            .SetContentIntent(pendingIntent)
            .SetSound(RingtoneManager.GetDefaultUri(RingtoneType.Notification))
            .SetAutoCancel(true);

        var notificationManager = NotificationManager.FromContext(this);
        notificationManager.Notify(0, notificationBuilder.Build());
    }
}
```

Když aplikace obdrží oznámení od FCM, `OnMessageReceived` metoda extrahuje obsah zprávy a volá `SendNotification` metoda. Tato metoda převede obsah zprávy do místního oznámení, který se spustí, když aplikace běží, oznámení, které jsou uvedeny v oznamovací oblasti.

##### <a name="handling-notification-intents"></a>Zpracování oznámení záměry

Když uživatel klepnutím oznámení, je k dispozici v žádná data doplňujícími oznámení `Intent` funkce. Tato data mohou být extrahovány následujícím kódem:

```csharp
if (Intent.Extras != null)
{
  foreach (var key in Intent.Extras.KeySet())
  {
    var value = Intent.Extras.GetString(key);
    Log.Debug(TAG, "Key: {0} Value: {1}", key, value);
  }
}
```

Spouštěč aplikace `Intent` je aktivována, když uživatel klepnutím své zprávy oznámení, takže tento kód se přihlaste všechna související data `Intent` do okna výstupu.

### <a name="universal-windows-platform"></a>Univerzální platforma pro Windows

Před univerzální platformu Windows (UWP) aplikace může přijímat nabízená oznámení, které se musí zaregistrovat s oznámení služby Windows (WNS), který vrátí kanál oznámení. Registrace je vyvolané `InitNotificationsAsync` metoda v `App` třídy:

```csharp
private async Task InitNotificationsAsync()
{
    var channel = await PushNotificationChannelManager
          .CreatePushNotificationChannelForApplicationAsync();

    const string templateBodyWNS =
        "<toast><visual><binding template=\"ToastText01\"><text id=\"1\">$(messageParam)</text></binding></visual></toast>";

    JObject headers = new JObject();
    headers["X-WNS-Type"] = "wns/toast";

    JObject templates = new JObject();
    templates["genericMessage"] = new JObject
    {
        {"body", templateBodyWNS},
        {"headers", headers} // Needed for WNS.
    };

    await TodoItemManager.DefaultManager.CurrentClient.GetPush()
          .RegisterAsync(channel.Uri, templates);
}
```

Tato metoda získá kanál nabízená oznámení, vytvoří šablonu oznámení zprávu jako JSON a zaregistruje zařízení přijímat šablony oznámení z centra oznámení.

`InitNotificationsAsync` Metoda je volána z `OnLaunched` potlačení v `App` třídy:

```csharp
protected override async void OnLaunched(LaunchActivatedEventArgs e)
{
    ...
    await InitNotificationsAsync();
}
```

Tím se zajistí, že registrace nabízených oznámení je vytvořit nebo aktualizovat pokaždé, když je aplikace spuštěna, proto zajistíte, že kanál nabízené WNS je vždy aktivní.

Při přijetí nabízeného oznámení se bude zobrazovat automaticky jako *informační* – nemodálním okně obsahující zprávu.

## <a name="summary"></a>Souhrn

Tento článek ukázal, jak používat Azure Notification Hubs k odesílání nabízených oznámení z instance Azure Mobile Apps na platformě Xamarin.Forms aplikaci. Azure Notification Hubs poskytuje infrastrukturu škálovatelné nabízených pro odesílání mobilní nabízená oznámení z jakéhokoli back-endu na libovolnou mobilní platformu, přičemž složitost back-end museli komunikovat s různých systémů oznámení platforem.


## <a name="related-links"></a>Související odkazy

- [Použití Azure mobilní aplikace](~/xamarin-forms/data-cloud/consuming/azure.md)
- [Azure Notification Hubs](https://azure.microsoft.com/documentation/articles/notification-hubs-push-notification-overview/)
- [Přidání nabízených oznámení do vaší aplikace na platformě Xamarin.Forms](https://azure.microsoft.com/documentation/articles/app-service-mobile-xamarin-forms-get-started-push/)
- [Nabízená oznámení v iOS](~/ios/platform/user-notifications/deprecated/remote-notifications-in-ios.md)
- [Firebase Cloud Messaging](~/android/data-cloud/google-messaging/firebase-cloud-messaging.md)
- [TodoAzurePush (ukázka)](https://developer.xamarin.com/samples/xamarin-forms/WebServices/TodoAzurePush/)
- [Azure mobilního klienta SDK](https://www.nuget.org/packages/Microsoft.Azure.Mobile.Client/)
