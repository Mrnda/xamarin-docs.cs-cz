---
title: Vzdálená oznámení pomocí služby Firebase Cloud Messaging
description: Tento názorný postup obsahuje podrobné vysvětlení, jak pomocí služby Firebase Cloud Messaging k implementaci Vzdálená oznámení (také nazývané nabízená oznámení) v aplikaci Xamarin.Android. Znázorňuje způsob implementace různých tříd, které jsou potřeba pro komunikaci pomocí služby Firebase Cloud Messaging (FCM), poskytuje příklady, jak nakonfigurovat Android Manifest pro přístup k FCM a ukazuje příjem zpráv pomocí Firebase Konzoly.
ms.prod: xamarin
ms.assetid: 4D7C5F46-C997-49F6-AFDA-6763E68CDC90
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 07/31/2018
ms.openlocfilehash: 36ac1be1274ff90d573aa53e5c86ae0a97709505
ms.sourcegitcommit: bf05041cc74fb05fd906746b8ca4d1403fc5cc7a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/04/2018
ms.locfileid: "39514424"
---
# <a name="remote-notifications-with-firebase-cloud-messaging"></a>Vzdálená oznámení pomocí služby Firebase Cloud Messaging

_Tento názorný postup obsahuje podrobné vysvětlení, jak pomocí služby Firebase Cloud Messaging k implementaci Vzdálená oznámení (také nazývané nabízená oznámení) v aplikaci Xamarin.Android. Znázorňuje způsob implementace různých tříd, které jsou potřeba pro komunikaci pomocí služby Firebase Cloud Messaging (FCM), poskytuje příklady, jak nakonfigurovat Android Manifest pro přístup k FCM a ukazuje příjem zpráv pomocí Firebase Konzoly._

## <a name="fcm-notifications-overview"></a>Přehled FCM oznámení

V tomto podrobném návodu se volá základní aplikaci **FCMClient** vytvoří se pro ilustraci essentials zasílání zpráv FCM. **FCMClient** zjišťuje přítomnost služby Google Play, obdrží registračních tokenů z FCM, zobrazí vzdálená oznámení, které odešlete v konzole Firebase a získá odběr zpráv tématu:

[![Příklad snímek obrazovky aplikace](remote-notifications-with-fcm-images/00-app-example-sml.png)](remote-notifications-with-fcm-images/00-app-example.png#lightbox)

Budeme se zabývat následujících témat:

1.  Oznámení na pozadí

2.  Zpráv tématu

3.  Popředí oznámení

V tomto návodu budete postupně přidávat funkce, které **FCMClient** a spustit ho v zařízení nebo emulátor, abyste pochopili, jak komunikuje se službou FCM. Pomocí protokolování se určující živé aplikace transakce s FCM servery a zjistíte, jak jsou oznámení vygenerovaná zpráv FCM, které jste zadali do konzole Firebase s oznámení grafického uživatelského rozhraní.

## <a name="requirements"></a>Požadavky


Může být vhodné se seznámit s [různé typy zpráv](https://firebase.google.com/docs/cloud-messaging/concept-options#notifications_and_data_messages) , který může odeslat pomocí služby Firebase Cloud Messaging. Datová část zprávy určí, jak budou klientské aplikace příjem a zpracování zprávy.

Než budete moct pokračovat s tímto názorným postupem, musíte získat potřebné přihlašovací údaje pro použití Googlu FCM serverů; Tento proces je popsán v [službu Firebase Cloud Messaging](~/android/data-cloud/google-messaging/firebase-cloud-messaging.md#setup_fcm).
Zejména musíte stáhnout **souboru google-services.json** soubor má být použit s ukázkovým kódem uvedené v tomto názorném postupu. Pokud jste ještě nevytvořili projekt v konzole Firebase (nebo pokud jste ještě nestáhli **souboru google-services.json** souboru), najdete v článku [službu Firebase Cloud Messaging](~/android/data-cloud/google-messaging/firebase-cloud-messaging.md).

Spuštění ukázkové aplikace, budete potřebovat test v Androidu zařízení nebo emulátor, který je s aktualizací pomocí služby Firebase. Firebase Cloud Messaging podporuje klienty se systémem Android 4.0 nebo vyšší a tato zařízení musí mít taky Google Play Store aplikace nainstalovaná (Google Play Services 9.2.1 nebo později se vyžaduje). Pokud ještě nemáte nainstalovanou na zařízení s aplikací Google Play Store, přejděte [Google Play](https://support.google.com/googleplay) webovou stránku pro stažení a instalaci. Alternativně můžete použít emulátor sady Android SDK pomocí služby Google Play nainstalovaný testovací zařízení (není nutné instalovat Google Play Store, pokud používáte emulátor sady Android SDK).

## <a name="start-an-app-project"></a>Spuštění projektu aplikace

Pokud chcete začít, vytvořte nový prázdný projekt Xamarin.Android s názvem **FCMClient**. Pokud nejste obeznámeni s vytvářením projekty Xamarin.Android, přečtěte si téma [Hello, Android](~/android/get-started/hello-android/hello-android-quickstart.md).
Po vytvoření nové aplikace je dalším krokem je nastavit název balíčku a nainstalujte několik balíčků NuGet, které se použije pro komunikace se službou FCM.

### <a name="set-the-package-name"></a>Nastavte název balíčku

V [službu Firebase Cloud Messaging](~/android/data-cloud/google-messaging/firebase-cloud-messaging.md), zadáte název balíčku pro aplikaci podporující FCM. Tento název balíčku slouží taky jako [ *ID aplikace* ](./firebase-cloud-messaging.md#fcm-in-action-app-id) , který je přidružený [klíč rozhraní API](firebase-cloud-messaging.md#fcm-in-action-api-key). Nakonfiguruje aplikaci, aby použijte tento název balíčku:

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

1.  Otevřete jeho vlastnosti **FCMClient** projektu.

2.  V **Manifest v Androidu** nastavte název balíčku.

V následujícím příkladu je název balíčku nastavený na `com.xamarin.fcmexample`:

[![Nastavení názvu balíčku](remote-notifications-with-fcm-images/01-package-name-vs-sml.png)](remote-notifications-with-fcm-images/01-package-name-vs.png#lightbox)

Když aktualizujete **Manifest v Androidu**, také zkontrolujte, ujistěte se, že `Internet` povoleno oprávnění.

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

1.  Otevřete jeho vlastnosti **FCMClient** projektu.

2.  V **aplikace pro Android** nastavte název balíčku.

V následujícím příkladu je název balíčku nastavený na `com.xamarin.fcmexample`:

[![Nastavení názvu balíčku](remote-notifications-with-fcm-images/01-package-name-xs-sml.png)](remote-notifications-with-fcm-images/01-package-name-xs.png#lightbox)

Když aktualizujete **Manifest v Androidu**, také zkontrolujte, ujistěte se, že `INTERNET` povoleno oprávnění (v části **požadovaná oprávnění**).

-----

> [!IMPORTANT]
> Klientská aplikace nebude moci přijímat registračního tokenu z FCM, pokud tento název balíčku není *přesně* shodovat s názvem balíčku, který jste zadali do konzole Firebase.

### <a name="add-the-xamarin-google-play-services-base-package"></a>Přidat balíček Xamarin Google Play Services Base

Protože službu Firebase Cloud Messaging, závisí na Google Play Services [Xamarin Google Play Services - Base](https://www.nuget.org/packages/Xamarin.GooglePlayServices.Base/) balíček NuGet musí být přidán do projektu Xamarin.Android. Budete potřebovat verze 29.0.0.2 nebo novější.

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

1.  V sadě Visual Studio, klikněte pravým tlačítkem na **odkazy > spravovat balíčky NuGet...** .

2.  Klikněte na tlačítko **Procházet** kartu a vyhledejte **Xamarin.GooglePlayServices.Base**.

3.  Instalaci tohoto balíčku do **FCMClient** projektu:

    [![Instalace základní služby Google Play](remote-notifications-with-fcm-images/02-google-play-services-vs-sml.png)](remote-notifications-with-fcm-images/02-google-play-services-vs.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

1.  V sadě Visual Studio pro Mac, klikněte pravým tlačítkem na **balíčků > přidat balíčky...** .

2.  Vyhledejte **Xamarin.GooglePlayServices.Base**.

3.  Instalaci tohoto balíčku do **FCMClient** projektu:

    [![Instalace základní služby Google Play](remote-notifications-with-fcm-images/02-google-play-services-xs-sml.png)](remote-notifications-with-fcm-images/02-google-play-services-xs.png#lightbox)

-----

Pokud dojde k chybě při instalaci balíčku NuGet, zavřete **FCMClient** projektu, otevřete znovu a znovu zkuste nainstalovat NuGet.

Při instalaci **Xamarin.GooglePlayServices.Base**, všechny potřebné závislé součásti jsou nainstalovány. Upravit **MainActivity.cs** a přidejte následující `using` – příkaz:

```csharp
using Android.Gms.Common;
```

Tento příkaz vytvoří `GoogleApiAvailability` třídy v **Xamarin.GooglePlayServices.Base** k dispozici **FCMClient** kódu.
`GoogleApiAvailability` se používá ke kontrole přítomnosti služby Google Play.

### <a name="add-the-xamarin-firebase-messaging-package"></a>Přidejte balíček zasílání zpráv služby Firebase Xamarin

Pro příjem zpráv z FCM, [Xamarin Firebase – zasílání zpráv](https://www.nuget.org/packages/Xamarin.Firebase.Messaging/) balíček NuGet musí být přidán do projektu aplikace. Bez tohoto balíčku aplikace pro Android nejde přijímá zprávy z FCM servery.

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

1.  V sadě Visual Studio, klikněte pravým tlačítkem na **odkazy > spravovat balíčky NuGet...** .

2. Vyhledejte **Xamarin.Firebase.Messaging**.

3.  Instalaci tohoto balíčku do **FCMClient** projektu:

    [![Instalace Xamarinu Firebase zasílání zpráv](remote-notifications-with-fcm-images/03-firebase-messaging-vs-sml.png)](remote-notifications-with-fcm-images/03-firebase-messaging-vs.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

1.  V sadě Visual Studio pro Mac, klikněte pravým tlačítkem na **balíčků > přidat balíčky...** .

2.  Zkontrolujte **zobrazit balíčky v předběžné verzi** a vyhledejte **Xamarin.Firebase.Messaging**.

3.  Instalaci tohoto balíčku do **FCMClient** projektu:

    [![Instalace Xamarinu Firebase zasílání zpráv](remote-notifications-with-fcm-images/03-firebase-messaging-xs-sml.png)](remote-notifications-with-fcm-images/03-firebase-messaging-xs.png#lightbox)

-----

Při instalaci **Xamarin.Firebase.Messaging**, všechny potřebné závislé součásti jsou nainstalovány.

Potom upravte **MainActivity.cs** a přidejte následující `using` příkazy:

```csharp
using Firebase.Messaging;
using Firebase.Iid;
using Android.Util;
```

Ujistěte se, typy v prvních dvou příkazů **Xamarin.Firebase.Messaging** balíčku NuGet, které jsou k dispozici **FCMClient** kódu. **Android.Util** přidá funkce protokolování, který se použije sledovat transakce s FMS.

### <a name="add-googleplayservices-json"></a>Přidání souboru JSON služby Google

Dalším krokem je přidání **souboru google-services.json** souboru do kořenového adresáře projektu:

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

1.  Kopírování **souboru google-services.json** do složky projektu.

2.  Přidat **souboru google-services.json** do projektu aplikace (klikněte na tlačítko **zobrazit všechny soubory** v **Průzkumníka řešení**, klikněte pravým tlačítkem na **souboru google-services.json**a pak vyberte **zahrnout do projektu**).

3.  Vyberte **souboru google-services.json** v **Průzkumníka řešení** okna.

4.  V **vlastnosti** podokno, nastavte **akce sestavení** k **GoogleServicesJson** (Pokud **GoogleServicesJson** akci sestavení není zobrazený, Uložte a zavřete řešení a potom ho znovu otevřít):

    [![Nastavení akce sestavení na GoogleServicesJson](remote-notifications-with-fcm-images/04-google-services-json-vs-sml.png)](remote-notifications-with-fcm-images/04-google-services-json-vs.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

1.  Kopírování **souboru google-services.json** do složky projektu.

2.  Přidat **souboru google-services.json** do projektu aplikace.

3.  Right-click **google-services.json**.

4.  Nastavte **akce sestavení** k **GoogleServicesJson**:

    [![Nastavení akce sestavení na GoogleServicesJson](remote-notifications-with-fcm-images/04-google-services-json-xs-sml.png)](remote-notifications-with-fcm-images/04-google-services-json-xs.png#lightbox)

-----

Když **souboru google-services.json** se přidá do projektu (a **GoogleServicesJson** nastavte akci sestavení), proces sestavení extrahuje ID klienta a [klíč rozhraní API](./firebase-cloud-messaging.md#fcm-in-action-api-key) a pak Přidá tyto přihlašovací údaje se sloučí/vygenerovat **AndroidManifest.xml** , který se nachází v **obj/Debug/android/AndroidManifest.xml**. Tento proces sloučení automaticky přidá žádná oprávnění a další prvky FCM, které jsou potřebné pro připojení k serverům FCM.


## <a name="check-for-google-play-services-and-create-a-notification-channel"></a>Zkontrolujte služby Google Play a vytvořit kanál oznámení

Doporučuje Google, že aplikace pro Android kontrolovat přítomnost APK služby Google Play před přístupem k funkcím služby Google Play (Další informace najdete v tématu [zkontrolujte služby Google Play](https://firebase.google.com/docs/cloud-messaging/android/client#sample-play)).

Nejprve se vytvoří počáteční rozložení pro Uživatelském rozhraní aplikace. Upravit **Resources/layout/Main.axml** a jeho obsah nahraďte následující kód XML:

```xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:orientation="vertical"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:padding="10dp">
    <TextView
        android:text=" "
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:id="@+id/msgText"
        android:textAppearance="?android:attr/textAppearanceMedium"
        android:padding="10dp" />
</LinearLayout>
```

To `TextView` se použije k zobrazení zpráv, které označují, jestli je nainstalovaná aplikace služby Google Play. Uložit změny do **Main.axml**.


Upravit **MainActivity.cs** a přidejte následující proměnné instance `MainActivity` třídy:

```csharp
public class MainActivity : AppCompatActivity
{
    static readonly string TAG = "MainActivity";

    internal static readonly string CHANNEL_ID = "my_notification_channel";
    internal static readonly int NOTIFICATION_ID = 100;

    TextView msgText;
```

Proměnné `CHANNEL_ID` a `NOTIFICATION_ID` se použije v metodě [ `CreateNotificationChannel` ](#create-notification-channel-code) , které se přidají do `MainActivity` později v tomto názorném postupu.


V následujícím příkladu `OnCreate` – metoda ověří, že služby Google Play je k dispozici, předtím, než se aplikace pokusí použít FCM služby.
Přidejte následující metodu do `MainActivity` třídy:

```csharp
public bool IsPlayServicesAvailable ()
{
    int resultCode = GoogleApiAvailability.Instance.IsGooglePlayServicesAvailable (this);
    if (resultCode != ConnectionResult.Success)
    {
        if (GoogleApiAvailability.Instance.IsUserResolvableError (resultCode))
            msgText.Text = GoogleApiAvailability.Instance.GetErrorString (resultCode);
        else
        {
            msgText.Text = "This device is not supported";
            Finish ();
        }
        return false;
    }
    else
    {
        msgText.Text = "Google Play Services is available.";
        return true;
    }
}
```

Tento kód zkontroluje zařízení zjistit, zda je nainstalována APK služby Google Play. Pokud není nainstalovaná, zobrazí se zpráva v `TextBox` , který dává pokyn uživateli můžete stáhnout balíček APK z Play Store Google (nebo ho chcete povolit v nastavení systému zařízení).

<a name="create-notification-channel-code"></a>Musíte vytvořit aplikace, které běží na Androidu 8.0 (úroveň rozhraní API 26) nebo vyšší [ _kanál oznámení_ ](~/android/app-fundamentals/notifications/local-notifications.md) pro publikování jejich oznámení.  Přidejte následující metodu do `MainActivity` třídy, který vytvoří kanál oznámení (v případě potřeby):

```csharp
void CreateNotificationChannel()
{
    if (Build.VERSION.SdkInt < BuildVersionCodes.O)
    {
        // Notification channels are new in API 26 (and not a part of the
        // support library). There is no need to create a notification
        // channel on older versions of Android.
        return;
    }

    var channel = new NotificationChannel(MyFirebaseMessagingService.CHANNEL_ID,
                                          "FCM Notifications",
                                          NotificationImportance.Default)
                  {

                      Description = "Firebase Cloud Messages appear in this channel"
                  };

    var notificationManager = (NotificationManager)GetSystemService(Android.Content.Context.NotificationService);
    notificationManager.CreateNotificationChannel(channel);
}
```

Nahradit `OnCreate` metodu s následujícím kódem:

```csharp
protected override void OnCreate (Bundle bundle)
{
    base.OnCreate (bundle);
    SetContentView (Resource.Layout.Main);
    msgText = FindViewById<TextView> (Resource.Id.msgText);

    IsPlayServicesAvailable ();

    CreateNotificationChannel();
}
```

`IsPlayServicesAvailable` je volána na konci `OnCreate` tak, aby zkontrolujte služby Google Play spuštění každého každém spuštění aplikace. Metoda `CreateNotificationChannel` volané zajistěte, aby existovala kanál oznámení pro zařízení s Androidem 8 nebo vyšší. Pokud má vaše aplikace `OnResume` metoda, měla by volat `IsPlayServicesAvailable` z `OnResume` také. Zcela znovu sestavte a spusťte aplikaci. Pokud je vše nakonfigurované správně, měli byste vidět obrazovku, která vypadá jako na následujícím snímku obrazovky:

[![Aplikace znamená, že je k dispozici aplikace služby Google Play](remote-notifications-with-fcm-images/05-gps-available-sml.png)](remote-notifications-with-fcm-images/05-gps-available.png#lightbox)

Pokud neobdržíte tohoto výsledku, ověřte, zda APK služby Google Play je nainstalován ve vašem zařízení (Další informace najdete v tématu [nastavení do Google Play Services](https://developers.google.com/android/guides/setup)).
Dál ověřte, že jste přidali **Xamarin.Google.Play.Services.Base** balíček do vaší **FCMClient** projektu jak je vysvětleno výše.


## <a name="add-the-instance-id-receiver"></a>Přidat příjemce ID instance

Dalším krokem je přidání služby, která rozšiřuje `FirebaseInstanceIdService` pro vytvoření a zpracování a aktualizace [Firebase registračních tokenů](~/android/data-cloud/google-messaging/firebase-cloud-messaging.md#fcm-in-action-registration-token). `FirebaseInstanceIdService` Služby, je třeba FCM bude moct posílat zprávy do zařízení. Když `FirebaseInstanceIdService` přidání služby do klientské aplikace, aplikace automaticky příjem zpráv FCM, který se zobrazí jako oznámení pokaždé, když je aplikace backgrounded.

### <a name="declare-the-receiver-in-the-android-manifest"></a>Deklarovat příjemce v manifestu Android

Upravit **AndroidManifest.xml** a vložte následující `<receiver>` prvky do `<application>` části:

```xml
<receiver
    android:name="com.google.firebase.iid.FirebaseInstanceIdInternalReceiver"
    android:exported="false" />
<receiver
    android:name="com.google.firebase.iid.FirebaseInstanceIdReceiver"
    android:exported="true"
    android:permission="com.google.android.c2dm.permission.SEND">
    <intent-filter>
        <action android:name="com.google.android.c2dm.intent.RECEIVE" />
        <action android:name="com.google.android.c2dm.intent.REGISTRATION" />
        <category android:name="${applicationId}" />
    </intent-filter>
</receiver>
```

Tato konfigurace XML provede následující akce:

-   Deklaruje `FirebaseInstanceIdReceiver` implementace, která poskytuje [jedinečný identifikátor](https://developers.google.com/instance-id/) pro každou instanci aplikace. Tento příjemce také ověřuje a autorizuje akce.

-   Deklaruje interní `FirebaseInstanceIdInternalReceiver` implementace, která se používá ke spuštění služby bezpečně.

-   [ID aplikace](./firebase-cloud-messaging.md#fcm-in-action-app-id) je uložen v **souboru google-services.json** soubor, který byl [přidány do projektu](#add-googleplayservices-json). Vazby Xamarin.Android Firebase nahradí token `${applicationId}` s ID aplikace; žádný další kód nevyžadovala klientskou aplikaci poskytnout ID aplikace.

`FirebaseInstanceIdReceiver` Je `WakefulBroadcastReceiver` , která obdrží `FirebaseInstanceId` a `FirebaseMessaging` události a poskytnout je třída, která jsou odvozeny z `FirebaseInstanceIdService`.

### <a name="implement-the-firebase-instance-id-service"></a>Implementace služby Firebase Instance ID

Práci při registraci aplikace s FCM zařizuje služba vlastní `FirebaseInstanceIdService` služba, která zadáte.
`FirebaseInstanceIdService` provede následující kroky:

1.  Používá [rozhraní Instance ID API](https://developers.google.com/android/reference/com/google/android/gms/iid/InstanceID) ke generování tokenů zabezpečení, které povolit klientskou aplikaci pro přístup k FCM a aplikačního serveru. Naopak aplikace získá zpět [registračního tokenu](~/android/data-cloud/google-messaging/firebase-cloud-messaging.md#fcm-in-action-registration-token) z FCM.

2.  Předává registrační token a aplikačním serverem, pokud to vyžaduje aplikačního serveru.

Přidat nový soubor s názvem **MyFirebaseIIDService.cs** a nahraďte jeho kód šablony následujícím kódem:

```csharp
using System;
using Android.App;
using Firebase.Iid;
using Android.Util;

namespace FCMClient
{
    [Service]
    [IntentFilter(new[] { "com.google.firebase.INSTANCE_ID_EVENT" })]
    public class MyFirebaseIIDService : FirebaseInstanceIdService
    {
        const string TAG = "MyFirebaseIIDService";
        public override void OnTokenRefresh()
        {
            var refreshedToken = FirebaseInstanceId.Instance.Token;
            Log.Debug(TAG, "Refreshed token: " + refreshedToken);
            SendRegistrationToServer(refreshedToken);
        }
        void SendRegistrationToServer(string token)
        {
            // Add custom implementation, as needed.
        }
    }
}
```

Tato služba implementuje `OnTokenRefresh` metodu, která je volána, když je původně vytvořil nebo změnil registrační token. Když `OnTokenRefresh` spustí, načte nejnovější token z `FirebaseInstanceId.Instance.Token` vlastnosti (která se aktualizuje asynchronně pomocí FCM). V tomto příkladu je zaznamenána aktualizovat token tak, že bude možné dokument zobrazit v okně výstupu:

```csharp
var refreshedToken = FirebaseInstanceId.Instance.Token;
Log.Debug(TAG, "Refreshed token: " + refreshedToken);
```

`OnTokenRefresh` je vyvolána zřídka: používá se k aktualizaci tokenu za následujících okolností:

-   Pokud aplikace se instaluje nebo odinstaluje.

-   Když uživatel odstraní data aplikace.

-   Pokud aplikace odstraní, ID Instance.

-   Při zabezpečení z tokenu došlo k napadení.

Podle společnosti Google [Instance ID](https://developers.google.com/instance-id/guides/android-implementation) dokumentace ke službě, službu FCM Instance ID bude vyžadovat, že aplikace aktualizovat svůj token pravidelně (obvykle každých 6 měsíců).

`OnTokenRefresh` volá také `SendRegistrationToAppServer` k přidružení uživatele registrační token s účtem na straně serveru (pokud existuje), který se spravuje pomocí aplikace:

```csharp
void SendRegistrationToAppServer (string token)
{
    // Add custom implementation here as needed.
}
```

Protože tato implementace závisí na návrhu aplikačního serveru, je k dispozici textu empty – metoda v tomto příkladu. Pokud váš server app vyžaduje informace o registraci FCM, upravte `SendRegistrationToAppServer` k přidružení tokenu ID instance FCM uživatele pomocí libovolného účtu na straně serveru udržuje vaše aplikace. (Všimněte si, že token, který je neprůhledný do klientské aplikace.)

Odeslání tokenu do aplikačního serveru `SendRegistrationToAppServer` vhodné ponechat logickou hodnotu označující, zda token, který se poslal na serveru. Pokud se tento datový typ boolean má hodnotu false, `SendRegistrationToAppServer` odešle token do aplikačního serveru &ndash; jinak token, který už se poslala na aplikační server v předchozí volání. V některých případech (například to `FCMClient` příkladu), server aplikace nemusí token; proto tato metoda není vyžadována pro tento příklad.

## <a name="implement-client-app-code"></a>Implementovat kód klienta aplikace

Teď, když příjemce služby jsou na místě, kód klienta aplikace lze zapsat využívat výhody těchto služeb. V následujících částech je přidáno tlačítko v uživatelském rozhraní pro přihlášení registračního tokenu (také nazývané *token Instance ID*), a další kód přidá `MainActivity` zobrazíte `Intent` informace při spuštění aplikace z oznámení:

[![Tlačítko Token protokolu přidán na obrazovku aplikace](remote-notifications-with-fcm-images/06-log-token-sml.png)](remote-notifications-with-fcm-images/06-log-token.png#lightbox)

### <a name="log-tokes"></a>Tokes protokolu

Kód přidaný v tomto kroku je určena pouze pro demonstrační účely &ndash; klientskou aplikaci v produkčním prostředí by nepotřebujete k protokolování registračních tokenů. Upravit **Resources/layout/Main.axml** a přidejte následující `Button` deklarace ihned po `TextView` element:

```xml
<Button
  android:id="@+id/logTokenButton"
  android:layout_width="match_parent"
  android:layout_height="wrap_content"
  android:layout_gravity="center_horizontal"
  android:text="Log Token" />
```

Přidejte následující kód do konce `MainActivity.OnCreate` metody:

```csharp
var logTokenButton = FindViewById<Button>(Resource.Id.logTokenButton);
logTokenButton.Click += delegate {
    Log.Debug(TAG, "InstanceID token: " + FirebaseInstanceId.Instance.Token);
};
```

Tento kód protokoluje aktuálního tokenu do okna výstup při **Token protokolu** klepnutí tlačítka.

### <a name="handle-notification-intents"></a>Zpracování oznámení záměrů

Když uživatel klepne oznámení vydávány **FCMClient**, všechna data doprovodném oznámení zpráva zpřístupněná v `Intent` funkce. Upravit **MainActivity.cs** a přidejte následující kód do horní části `OnCreate` – metoda (před voláním `IsPlayServicesAvailable`):

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

Spouštěč aplikace `Intent` je aktivována, když uživatel klepne jeho zpráva s oznámením, tento kód se přihlásit se všechna související data `Intent` v okně výstupu. Pokud jiný `Intent` musí být aktivována, `click_action` pole zprávy oznámení musí být nastavena na tento `Intent` (spouštěč `Intent` se používá, když ne `click_action` je zadána).


## <a name="background-notifications"></a>Oznámení na pozadí

Sestavit a spustit **FCMClient** aplikace. **Token protokolu** je zobrazeno tlačítko:

[![Je zobrazeno tlačítko Token protokolu](remote-notifications-with-fcm-images/06-log-token-sml.png)](remote-notifications-with-fcm-images/06-log-token.png#lightbox)

Klepněte **Token protokolu** tlačítko. V okně výstupu integrovaném vývojovém prostředí by měl zobrazí zpráva vypadat asi takto:

[![Token ID instance zobrazí v okně výstupu](remote-notifications-with-fcm-images/07-token-received-sml.png)](remote-notifications-with-fcm-images/07-token-received.png#lightbox)

Dlouhý řetězec označené **token** je token ID instance, který vložíte do konzole Firebase &ndash; vyberte a zkopírujte tento řetězec do schránky. Pokud nevidíte instance ID token, přidejte následující řádek do horní části `OnCreate` metodu k ověření, **souboru google-services.json** správně parsovat:

```csharp
Log.Debug(TAG, "google app id: " + GetString(Resource.String.google_app_id));
```

`google_app_id` Hodnota protokoluje do okna výstup by měl odpovídat `mobilesdk_app_id` zaznamenaná hodnota v **souboru google-services.json**.

### <a name="send-a-message"></a>Odeslání zprávy

Přihlaste se [konzole Firebase](https://console.firebase.google.com), vyberte projekt, klikněte na tlačítko **oznámení**a klikněte na tlačítko **odeslat svůj první zprávu**:

[![Vaše první zprávu tlačítko Odeslat](remote-notifications-with-fcm-images/08-first-notification-sml.png)](remote-notifications-with-fcm-images/08-first-notification.png#lightbox)

Na **psaní zpráv** stránky, zadejte text zprávy a vyberte **jedno zařízení**. Zkopírujte instance ID token z okna výstup integrovaného vývojového prostředí a vložte ho do **registračního tokenu FCM** konzole Firebase pole:

[![Vytvořit dialogové okno zprávy](remote-notifications-with-fcm-images/09-compose-message-sml.png)](remote-notifications-with-fcm-images/09-compose-message.png#lightbox)

Na Androidu zařízení (nebo emulátoru) na pozadí aplikace stačí klepnout Android **přehled** tlačítko a klepnou na domovské obrazovce. Když zařízení je připravené, klikněte na tlačítko **POSLAT zprávu** v konzole Firebase:

[![Odeslat zprávu tlačítko](remote-notifications-with-fcm-images/10-send-message-sml.png)](remote-notifications-with-fcm-images/10-send-message.png#lightbox)

Když **Zkontrolujte zprávu** dialogového okna se zobrazí, klikněte na **odeslat**.
By se zobrazit ikonu oznámení v oznamovací oblasti na zařízení (nebo emulátoru):

[![Ikona je zobrazena.](remote-notifications-with-fcm-images/11-notification-icon-sml.png)](remote-notifications-with-fcm-images/11-notification-icon.png#lightbox)

Otevřete na ikonu oznámení chcete zprávu zobrazit. Zprávy oznámení musí být přesně co byla zadána do **text zprávy** konzole Firebase pole:

[![Zobrazí se oznámení na zařízení](remote-notifications-with-fcm-images/12-notification-sml.png)](remote-notifications-with-fcm-images/12-notification.png#lightbox)

Klepněte na ikonu oznámení a spustit **FCMClient** aplikace. `Intent` Funkce odesílat **FCMClient** jsou uvedeny ve výstupním okně integrovaného vývojového prostředí:

[![Seznamy záměru extra od sbalit klíč, klíč a ID zprávy](remote-notifications-with-fcm-images/13-intent-extras-sml.png)](remote-notifications-with-fcm-images/13-intent-extras.png#lightbox)

V tomto příkladu **z** klíč nastavený na číslo projektu Firebase aplikace (v tomto příkladu `41590732`) a **collapse_key** je nastavena na jeho název balíčku ( **com.xamarin.fcmexample**).
Pokud se zobrazí zpráva, zkuste odstranit **FCMClient** aplikace na zařízení (nebo emulátoru) a opakujte předchozí postup.


> [!NOTE]
> Pokud vám platnost blízko aplikace, přestane FCM, doručování oznámení. Android brání vysílání služby na pozadí od nedopatřením nebo zbytečně spuštěné součástí zastavené aplikace. (Další informace o tomto chování najdete v tématu [spuštění ovládacích prvků na zastavené aplikace](https://developer.android.com/about/versions/android-3.1.html#launchcontrols).) Z tohoto důvodu je nutné ručně odinstalovat aplikaci pokaždé, když ji spustit a zastavit z relace ladění &ndash; To přinutí FCM tak, aby se zprávy budou i nadále přijímat vygenerovat nový token.

### <a name="add-a-custom-default-notification-icon"></a>Přidat vlastní výchozí ikona oznámení

V předchozím příkladu je nastavena na ikonu oznámení na ikonu aplikace. Následující kód XML nakonfiguruje vlastní výchozí ikony pro oznámení. Android zobrazí tuto vlastní výchozí ikonu pro všechny zprávy oznámení, pokud není explicitně nastavena na ikonu oznámení.

Chcete-li přidat vlastní výchozí ikona oznámení, přidejte vaší ikony **prostředky/drawable** adresáře, upravit **AndroidManifest.xml**a vložte následující `<meta-data>` elementu do `<application>`části:

```xml
<meta-data
    android:name="com.google.firebase.messaging.default_notification_icon"
    android:resource="@drawable/ic_stat_ic_notification" />
```

V tomto příkladu na ikonu oznámení, která se nachází v **prostředky/drawable/ic\_stat\_ic\_notification.png** se použije jako výchozí vlastní ikonu oznámení. Pokud vlastní výchozí ikona není nakonfigurovaná v **AndroidManifest.xml** a v datová část oznámení je nastavena žádná ikona, Android (jak je vidět ve výše uvedeném snímku obrazovky oznámení ikonu) používá ikony aplikace jako ikonu oznámení.

## <a name="handle-topic-messages"></a>Zpracování zpráv tématu

Kód napsaný doposud zpracovává registračních tokenů a přidá funkce vzdáleného oznámení do aplikace. Následující příklad přidá kód, který naslouchá *zpráv tématu* a předává je na uživatele jako Vzdálená oznámení. Téma zprávy jsou zpráv FCM, které se odesílají do jednoho nebo více zařízení, která přihlášení k odběru určitého tématu. Další informace o zpráv tématu najdete v tématu [zasílání zpráv tématu](~/android/data-cloud/google-messaging/firebase-cloud-messaging.md).

### <a name="subscribe-to-a-topic"></a>K odběru tématu

Upravit **Resources/layout/Main.axml** a přidejte následující `Button` deklarace ihned po předchozí `Button` element:

```xml
<Button
  android:id="@+id/subscribeButton"
  android:layout_width="match_parent"
  android:layout_height="wrap_content"
  android:layout_gravity="center_horizontal"
  android:layout_marginTop="20dp"
  android:text="Subscribe to Notifications" />
```

Přidá tato konfigurace XML **přihlášení odběru oznámení** tlačítko rozložení.
Upravit **MainActivity.cs** a přidejte následující kód do konce `OnCreate` metody:

```csharp
var subscribeButton = FindViewById<Button>(Resource.Id.subscribeButton);
subscribeButton.Click += delegate {
    FirebaseMessaging.Instance.SubscribeToTopic("news");
    Log.Debug(TAG, "Subscribed to remote notifications");
};
```

Tento kód najde **přihlášení odběru oznámení** tlačítko v rozložení a přiřadí její obslužná rutina kliknutí na kód, který volá `FirebaseMessaging.Instance.SubscribeToTopic`a předejte předplacenému tématu _zpráv_. Když uživatel klepne **přihlásit k odběru** tlačítko, aplikace se přihlásí k odběru _zpráv_ tématu. V následující části _zpráv_ se pošle zpráva téma z konzole Firebase s oznámení grafického uživatelského rozhraní.

### <a name="send-a-topic-message"></a>Odeslání zprávy tématu

Odinstalujte aplikaci, její opětovné sestavení a potom ho spusťte znovu. Klikněte na tlačítko **přihlásit k odběru oznámení** tlačítka:

[![Přihlaste se k oznámení tlačítko odběru](remote-notifications-with-fcm-images/14-subscribe-sml.png)](remote-notifications-with-fcm-images/14-subscribe.png#lightbox)

Pokud aplikace se úspěšně připojila, měli byste vidět **úspěšná synchronizace tématu** okno výstup v integrovaném vývojovém prostředí:

[![Okno výstup zobrazí zprávu tématu úspěšná synchronizace](remote-notifications-with-fcm-images/15-topic-sync-sml.png)](remote-notifications-with-fcm-images/15-topic-sync.png#lightbox)

Použijte následující postup k odeslání zprávy tématu:

1.  V konzole Firebase klikněte na **novou zprávu**.

2.  Na **psaní zpráv** stránky, zadejte text zprávy a vyberte **tématu**.

3.  V **tématu** rozevírací nabídky vyberte integrované téma **zpráv**:

    [![Výběr zpráv tématu](remote-notifications-with-fcm-images/16-topic-message-sml.png)](remote-notifications-with-fcm-images/16-topic-message.png#lightbox)

4.  Na Androidu zařízení (nebo emulátoru) na pozadí aplikace stačí klepnout Android **přehled** tlačítko a klepnou na domovské obrazovce.

5.  Když zařízení je připravené, klikněte na tlačítko **POSLAT zprávu** v konzole Firebase.

6.  Zkontrolujte okno prostředí IDE výstupu a zobrazit **/témata/zprávy** ve výstupu protokolu:

    [![Zpráva z /topic/news je zobrazena.](remote-notifications-with-fcm-images/17-message-arrived-sml.png)](remote-notifications-with-fcm-images/17-message-arrived.png#lightbox)

Pokud tato zpráva je zobrazena v okně výstupu, na ikonu oznámení by měl také zobrazí v oznamovací oblasti na zařízení s Androidem. Otevřete na ikonu oznámení k zobrazení zpráv tématu:

[![Zobrazí se zpráva tématu jako upozornění](remote-notifications-with-fcm-images/18-other-news-sml.png)](remote-notifications-with-fcm-images/18-other-news.png#lightbox)

Pokud se zobrazí zpráva, zkuste odstranit **FCMClient** aplikace na zařízení (nebo emulátoru) a opakujte předchozí postup.

## <a name="foreground-notifications"></a>Popředí oznámení

Pokud chcete dostávat oznámení v aplikacích foregrounded, je nutné implementovat `FirebaseMessagingService`. Tato služba je také nutný pro příjem přenášených dat a pro odesílání zpráv nadřazený. Následující příklady ukazují, jak implementovat službu, která rozšiřuje `FirebaseMessagingService` &ndash; výsledné aplikace bude schopná zpracovat Vzdálená oznámení, když je spuštěná v popředí.

### <a name="implement-firebasemessagingservice"></a>Implementace FirebaseMessagingService

`FirebaseMessagingService` Služba je zodpovědná za příjem a zpracování zpráv ze služby Firebase. Každá aplikace musí podtřídu tohoto typu a přepsání `OnMessageReceived` zpracovat příchozí zprávu. Když aplikace v popředí, `OnMessageReceived` zpětné volání bude vždy zpracovávat zprávy.

> [!NOTE]
> Aplikace mají jenom 10 sekund, ve kterých se mají zpracovat příchozí zprávy služby Firebase Cloud. Žádnou práci, kterou trvá déle, než to má být naplánováno pro spuštění na pozadí pomocí knihovny, jako [Android Plánovač úloh](~/android/platform/android-job-scheduler.md) nebo [dispečer úloh Firebase](~/android/platform/firebase-job-dispatcher.md).

Přidat nový soubor s názvem **MyFirebaseMessagingService.cs** a nahraďte jeho kód šablony následujícím kódem:

```csharp
using System;
using Android.App;
using Android.Content;
using Android.Media;
using Android.Util;
using Firebase.Messaging;

namespace FCMClient
{
    [Service]
    [IntentFilter(new[] { "com.google.firebase.MESSAGING_EVENT" })]
    public class MyFirebaseMessagingService : FirebaseMessagingService
    {
        const string TAG = "MyFirebaseMsgService";
        public override void OnMessageReceived(RemoteMessage message)
        {
            Log.Debug(TAG, "From: " + message.From);
            Log.Debug(TAG, "Notification Message Body: " + message.GetNotification().Body);
        }
    }
}
```

Všimněte si, že `MESSAGING_EVENT` záměru filtru musí být deklarována tak, aby nové zprávy FCM přesměrováni na `MyFirebaseMessagingService`:

```csharp
[IntentFilter(new[] { "com.google.firebase.MESSAGING_EVENT" })]
```

Když klientská aplikace přijme zprávu z FCM, `OnMessageReceived` extrahuje obsah zprávy z modulu předaný in `RemoteMessage` objektu voláním jeho `GetNotification` metoda. V dalším kroku zaznamená obsah zprávy tak, že bude možné dokument zobrazit v okně výstupu integrované vývojové prostředí:

```csharp
var body = message.GetNotification().Body;
Log.Debug(TAG, "Notification Message Body: " + body);
```

> [!NOTE]
> Pokud nastavení zarážek v `FirebaseMessagingService`, vaši relaci ladění může nebo nemusí přístupů na tyto body přerušení kvůli jak předává zprávy ostatním FCM.


### <a name="send-another-message"></a>Odeslat další zprávy

Odinstalovat aplikaci, její opětovné sestavení, spusťte znovu a odeslat další zprávy pomocí těchto kroků:

1.  V konzole Firebase klikněte na **novou zprávu**.

2.  Na **psaní zpráv** stránky, zadejte text zprávy a vyberte **jedno zařízení**.

3.  Zkopírujte řetězec tokenu z okna výstup integrovaného vývojového prostředí a vložte ho do **registračního tokenu FCM** pole konzole Firebase stejně jako předtím.

4.  Ujistěte se, že je aplikace spuštěná v popředí a potom klikněte na **POSLAT zprávu** v konzole Firebase:

    [![Odesílání další zprávu z konzoly](remote-notifications-with-fcm-images/19-hello-again-sml.png)](remote-notifications-with-fcm-images/19-hello-again.png#lightbox)

5.  Když **Zkontrolujte zprávu** dialogového okna se zobrazí, klikněte na **odeslat**.

6.  Příchozí zprávy se protokoluje do okna výstup integrované vývojové prostředí:

    [![Tělo zprávy zobrazeny v okně výstupu](remote-notifications-with-fcm-images/20-logged-message.png)](remote-notifications-with-fcm-images/20-logged-message.png#lightbox)


### <a name="add-a-local-notification-sender"></a>Přidání místního oznámení změny odesílatele

V tomto příkladu zbývající příchozích zpráv FCM se převede na místní oznámení, který se spustí, když je aplikace spuštěná v popředí. Upravit **MyFirebaseMessageService.cs** a přidejte následující `using` příkazy:

```csharp
using FCMClient;
using System.Collections.Generic;
```

Přidejte následující metodu do `MyFirebaseMessagingService`:

<a name="sendnotification-method"></a>
```csharp
void SendNotification(string messageBody, IDictionary<string, string> data)
{
    var intent = new Intent(this, typeof(MainActivity));
    intent.AddFlags(ActivityFlags.ClearTop);
    foreach (var key in data.Keys)
    {
        intent.PutExtra(key, data[key]);
    }

    var pendingIntent = PendingIntent.GetActivity(this,
                                                  MainActivity.NOTIFICATION_ID,
                                                  intent,
                                                  PendingIntentFlags.OneShot);

    var notificationBuilder = new  NotificationCompat.Builder(this, MainActivity.CHANNEL_ID)
                              .SetSmallIcon(Resource.Drawable.ic_stat_ic_notification)
                              .SetContentTitle("FCM Message")
                              .SetContentText(messageBody)
                              .SetAutoCancel(true)
                              .SetContentIntent(pendingIntent);

    var notificationManager = NotificationManagerCompat.From(this);
    notificationManager.Notify(MainActivity.NOTIFICATION_ID, notificationBuilder.Build());
}
```

K rozlišení tohoto oznámení z oznámení na pozadí, tento kód označuje oznámení ikonou, která se liší od ikony aplikace. Přidat soubor [ic\_stat\_ic\_notification.png](remote-notifications-with-fcm-images/ic-stat-ic-notification.png) k **prostředky/drawable** a zahrnout ho **FCMClient** projektu .

`SendNotification` Používá metoda ` NotificationCompat.Builder` k vytvoření oznámení, a `NotificationManagerCompat` se používá ke spuštění oznámení. Obsahuje oznámení `PendingIntent` , který vám umožní uživateli otevřete aplikaci a zobrazit obsah řetězce předané do `messageBody`. Další informace o `NotificationCompat.Builder`, naleznete v tématu [místní oznámení](~/android/app-fundamentals/notifications/local-notifications.md).

Volání `SendNotification` metoda na konci `OnMessageReceived` metody:

```csharp
public override void OnMessageReceived(RemoteMessage message)
{
    Log.Debug(TAG, "From: " + message.From);

    var body = message.GetNotification().Body;
    Log.Debug(TAG, "Notification Message Body: " + body);
    SendNotification(body, message.Data);
}
```

V důsledku těchto změn `SendNotification` se spustí pokaždé, když je přijato oznámení, když je aplikace v popředí a v oznamovací oblasti se zobrazí oznámení.

Když je aplikace na pozadí [datovou část zprávy](https://firebase.google.com/docs/cloud-messaging/concept-options#notifications_and_data_messages) určí, jak se zpracovává zpráva:

* **Oznámení** &ndash; zprávy se odešlou do **na hlavním panelu systému**. Místní oznámení, tam objeví. Aplikace se spustí, když uživatel klepne na oznámení.
* **Data** &ndash; zpráv bude zpracován adresou `OnMessageReceived`.
* **Obě** &ndash; zprávy, které mají datová část oznámení a data bude doručen do oznamovací oblasti. Při spuštění aplikace, datová část se zobrazí v `Extras` z `Intent` , který se používá ke spuštění aplikace.

V tomto příkladu, pokud je aplikace backgrounded `SendNotification` se spustí, pokud má zpráva datovou částí. V opačném případě se spustí na pozadí oznámení (uvedené dříve v tomto návodu).

### <a name="send-the-last-message"></a>Odeslání poslední zprávy

Odinstalujte aplikaci, její opětovné sestavení, spusťte znovu a poté pomocí následujících kroků k odeslání poslední zprávy:

1.  V konzole Firebase klikněte na **novou zprávu**.

2.  Na **psaní zpráv** stránky, zadejte text zprávy a vyberte **jedno zařízení**.

3.  Zkopírujte řetězec tokenu z okna výstup integrovaného vývojového prostředí a vložte ho do **registračního tokenu FCM** pole konzole Firebase stejně jako předtím.

4.  Ujistěte se, že je aplikace spuštěná v popředí a potom klikněte na **POSLAT zprávu** v konzole Firebase:

    [![Odeslání zprávy popředí](remote-notifications-with-fcm-images/21-console-fg-msg-sml.png)](remote-notifications-with-fcm-images/21-console-fg-msg.png#lightbox)

Tentokrát, zprávu, která se zaznamenala ve výstupním okně je také zabalen do nové oznámení &ndash; ikonu oznámení se zobrazí v oznamovací oblasti, zatímco je aplikace spuštěná v popředí:

[![Ikona oznámení pro zprávu popředí](remote-notifications-with-fcm-images/22-foreground-icon-sml.png)](remote-notifications-with-fcm-images/22-foreground-icon.png#lightbox)

Když otevřete oznámení, by se zobrazit zpráva poslední odeslané z grafického uživatelského rozhraní služby Firebase konzoly oznámení:

[![Popředí oznámení zobrazí s ikonou popředí](remote-notifications-with-fcm-images/23-foreground-msg-sml.png)](remote-notifications-with-fcm-images/23-foreground-msg.png#lightbox)


## <a name="disconnecting-from-fcm"></a>Odpojování od FCM

Chcete-li zrušit odběr tématu, zavolejte [UnsubscribeFromTopic](https://firebase.google.com/docs/reference/android/com/google/firebase/messaging/FirebaseMessaging.html#unsubscribeFromTopic%28java.lang.String%29) metodu na [FirebaseMessaging](https://firebase.google.com/docs/reference/android/com/google/firebase/messaging/FirebaseMessaging) třídy. Například chcete-li zrušit odběr _zpráv_ tématu dříve, k odběru **Unsubscribe** tlačítko může být přidán do rozložení s následujícím kódem obslužné rutiny:

```csharp
var unSubscribeButton = FindViewById<Button>(Resource.Id.unsubscribeButton);
unSubscribeButton.Click += delegate {
    FirebaseMessaging.Instance.UnsubscribeFromTopic("news");
    Log.Debug(TAG, "Unsubscribed from remote notifications");
};
```

Pokud chcete zrušit registraci zařízení z zcela FCM, odstraňte instance ID voláním [DeleteInstanceId](https://firebase.google.com/docs/reference/android/com/google/firebase/iid/FirebaseInstanceId.html#deleteInstanceId%28%29) metodu [FirebaseInstanceId](https://firebase.google.com/docs/reference/android/com/google/firebase/iid/FirebaseInstanceId) třídy. Příklad:

```csharp
FirebaseInstanceId.Instance.DeleteInstanceId();
```

Odstraní volání této metody instance ID a dat s ním spojená. V důsledku toho je zastaveno pravidelné odesílání FCM data na zařízení.


## <a name="troubleshooting"></a>Poradce při potížích

Následující describe problémy a řešení, které mohou nastat při používání služby Firebase Cloud Messaging s Xamarin.Android.

### <a name="firebaseapp-is-not-initialized"></a>FirebaseApp není inicializován.

V některých případech se může zobrazit tato chybová zpráva:

```shell
Java.Lang.IllegalStateException: Default FirebaseApp is not initialized in this process
Make sure to call FirebaseApp.initializeApp(Context) first.
```

Toto je známý problém, který můžete alternativně vyřešit pomocí čištění řešení a projekt znovu sestavit (**sestavení > Vyčistit řešení**, **sestavení > znovu sestavit řešení**). Další informace najdete v tomto [diskuzi na fóru](https://forums.xamarin.com/discussion/96263/default-firebaseapp-is-not-initialized-in-this-process).


## <a name="summary"></a>Souhrn

Tento návod podrobně popsané kroky pro implementaci služby Firebase Cloud Messaging Vzdálená oznámení v aplikaci Xamarin.Android. Popisuje postup instalace požadovaných balíčků, které jsou pro komunikaci FCM, a to bylo vysvětleno, jak konfigurovat Android Manifest pro přístup k serverům FCM. Je k dispozici ukázkový kód, který ukazuje, jak kontrolovat přítomnost služby Google Play. To jsme vám ukázali jak implementovat službu naslouchání ID instance, který vyjednává s FCM pro registrační token a je vysvětleno, jak tento kód vytvoří oznámení na pozadí, zatímco backgrounded aplikace. Vysvětlení přihlášení odběru zpráv tématu, a poskytuje příklad implementace služby naslouchání zpráv, který se používá pro příjem a zobrazit Vzdálená oznámení, když je aplikace spuštěná v popředí.


## <a name="related-links"></a>Související odkazy

- [FCMNotifications (ukázka)](https://developer.xamarin.com/samples/monodroid/Firebase/FCMNotifications)
- [Firebase Cloud Messaging](~/android/data-cloud/google-messaging/firebase-cloud-messaging.md)
- [O zpráv FCM](https://firebase.google.com/docs/cloud-messaging/concept-options)
