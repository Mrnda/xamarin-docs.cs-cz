---
title: Vzdálená oznámení s Firebase cloudu zasílání zpráv
description: Tento názorný postup obsahuje podrobné vysvětlení způsobu implementace vzdáleného oznámení (také nazývané nabízená oznámení) pomocí služby zasílání zpráv cloudu Firebase v aplikaci Xamarin.Android. Ilustruje způsob implementace různých tříd, které jsou potřebné pro komunikaci s zasílání zpráv cloudu Firebase (FCM), obsahuje příklady, jak nakonfigurovat Android Manifest pro přístup k FCM a ukazuje podřízené zasílání zpráv pomocí Firebase Konzola.
ms.prod: xamarin
ms.assetid: 4D7C5F46-C997-49F6-AFDA-6763E68CDC90
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 04/12/2018
ms.openlocfilehash: e2f25504b971a0332dc51dc9b017c9c83222ec57
ms.sourcegitcommit: bc39d85b4585fcb291bd30b8004b3f7edcac4602
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/16/2018
ms.locfileid: "31044935"
---
# <a name="remote-notifications-with-firebase-cloud-messaging"></a>Vzdálená oznámení s Firebase cloudu zasílání zpráv

_Tento názorný postup obsahuje podrobné vysvětlení způsobu implementace vzdáleného oznámení (také nazývané nabízená oznámení) pomocí služby zasílání zpráv cloudu Firebase v aplikaci Xamarin.Android. Ilustruje způsob implementace různých tříd, které jsou potřebné pro komunikaci s zasílání zpráv cloudu Firebase (FCM), obsahuje příklady, jak nakonfigurovat Android Manifest pro přístup k FCM a ukazuje podřízené zasílání zpráv pomocí Firebase Konzola._

## <a name="fcm-notifications-overview"></a>Přehled FCM oznámení

V tomto návodu se nazývá základní aplikaci **FCMClient** se vytvoří pro ilustraci essentials FCM zasílání zpráv. **FCMClient** ověří přítomnost služby Google Play, přijímá registrace tokenů ze FCM, zobrazí vzdáleného oznámení, které odešlete z konzoly Firebase a jako odběratel u zpráv tématu:

[![Příklad snímek obrazovky aplikace](remote-notifications-with-fcm-images/00-app-example-sml.png)](remote-notifications-with-fcm-images/00-app-example.png#lightbox)

Bude prozkoumali se následujících témat:

1.  Pozadí oznámení

2.  Téma zprávy

3.  Oznámení popředí

V tomto návodu se budou postupně přidávat funkce **FCMClient** a spustit v emulátoru pochopit, jak pracuje s FCM nebo zařízení. Protokolování bude používat k určující transakce za provozu aplikace s FCM servery a zjistíte, jak generování oznámení z FCM zprávy, které zadáte do grafické uživatelské rozhraní Firebase konzoly oznámení. 

## <a name="requirements"></a>Požadavky

Než budete pokračovat v tomto průvodci, musíte získat potřebné pověření pro použití Google FCM serverů; Tento proces je vysvětleno v [Firebase Cloud Messaging](~/android/data-cloud/google-messaging/firebase-cloud-messaging.md). Konkrétně je nutné stáhnout **google services.json** souboru pro použití s ukázkový kód uvedené v tomto návodu. Pokud jste ještě nevytvořili projektu v konzole Firebase (nebo pokud jste dosud nestáhli **google services.json** soubor), najdete v části [Firebase Cloud Messaging](~/android/data-cloud/google-messaging/firebase-cloud-messaging.md). 

Příklad aplikace spustit, budete potřebovat Android testovací zařízení nebo emulátoru, která je s aktualizací s Firebase. Zasílání zpráv cloudu firebase podporuje klienty se systémem Android 2.3 (Perník) nebo vyšší, a tato zařízení musí mít taky nainstalovanou aplikací Google Play Storu (Google Play Services 9.2.1 nebo novější je požadovaná). Pokud ještě nemáte nainstalovanou na zařízení s aplikací Google Play Storu, navštivte [Google Play](https://support.google.com/googleplay) webu si můžete stáhnout a nainstalovat. Alternativně můžete použít sady SDK pro Android emulátoru, systémem Android 2.3 nebo novějším místo testovací zařízení (není nutné instalovat Google Play Storu, pokud používáte emulátor sady SDK pro Android). 

## <a name="start-an-app-project"></a>Spusťte projekt aplikace

Pokud chcete začít, vytvořit nový prázdný projekt Xamarin.Android s názvem **FCMClient**. Pokud nejste obeznámeni s vytváření projektů Xamarin.Android, přečtěte si téma [Hello, Android](~/android/get-started/hello-android/hello-android-quickstart.md). Po vytvoření nové aplikace je dalším krokem je nastavit název balíčku a instalaci více balíčků NuGet, které se použijí pro komunikaci s FCM. 

### <a name="set-the-package-name"></a>Nastavte název balíčku

V [Firebase Cloud Messaging](~/android/data-cloud/google-messaging/firebase-cloud-messaging.md), jste zadali název balíčku pro aplikaci podporující FCM. Tento název balíčku slouží taky jako *ID aplikace* který je přidružen klíč rozhraní API. Nakonfiguruje aplikaci, aby použít tento název balíčku:

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

1.  Otevřete vlastnosti **FCMClient** projektu. 

2.  V **Android Manifest** nastavte název balíčku. 

V následujícím příkladu je název balíčku nastavte na `com.xamarin.fcmexample`: 

[![Nastavení názvu balíčku](remote-notifications-with-fcm-images/01-package-name-vs-sml.png)](remote-notifications-with-fcm-images/01-package-name-vs.png#lightbox)

Při aktualizaci **Android Manifest**, taky zkontrolujte, zda `Internet` je povoleno oprávnění. 

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

1.  Otevřete vlastnosti **FCMClient** projektu. 

2.  V **aplikace pro Android** nastavte název balíčku. 

V následujícím příkladu je název balíčku nastavte na `com.xamarin.fcmexample`: 

[![Nastavení názvu balíčku](remote-notifications-with-fcm-images/01-package-name-xs-sml.png)](remote-notifications-with-fcm-images/01-package-name-xs.png#lightbox)

Při aktualizaci **Android Manifest**, taky zkontrolujte, zda `INTERNET` je povoleno oprávnění (v části **požadovaná oprávnění**). 

-----

Klientská aplikace bude nelze přijmout token registrace z FCM, pokud nemá tento název balíčku *přesně* shodovat s názvem balíčku, který byl zadán do konzoly Firebase. 

### <a name="add-the-xamarin-google-play-services-base-package"></a>Přidejte balíček základní Xamarin Google Play Services

Protože zasílání zpráv cloudu Firebase na služby Google Play, závisí [Xamarin Google Play Services - základní](https://www.nuget.org/packages/Xamarin.GooglePlayServices.Base/) balíček NuGet musí být přidán do projektu Xamarin.Android. Budete potřebovat verze 29.0.0.2 nebo novější.

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

1.  V sadě Visual Studio, klikněte pravým tlačítkem na **odkazy > spravovat balíčky NuGet...** . 

2.  Klikněte **Procházet** kartě a vyhledejte **Xamarin.GooglePlayServices.Base**. 

3.  Instalaci tohoto balíčku do **FCMClient** projektu: 

    [![Instalace základní služby Google Play](remote-notifications-with-fcm-images/02-google-play-services-vs-sml.png)](remote-notifications-with-fcm-images/02-google-play-services-vs.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

1.  V sadě Visual Studio pro Mac, klikněte pravým tlačítkem na **balíčků > přidat balíčky...** . 

2.  Vyhledejte **Xamarin.GooglePlayServices.Base**. 

3.  Instalaci tohoto balíčku do **FCMClient** projektu: 

    [![Instalace základní služby Google Play](remote-notifications-with-fcm-images/02-google-play-services-xs-sml.png)](remote-notifications-with-fcm-images/02-google-play-services-xs.png#lightbox)

-----

Pokud dojde k chybě během instalace NuGet, zavřete **FCMClient** projekt znovu otevřete a opakujte instalaci NuGet. 

Při instalaci **Xamarin.GooglePlayServices.Base**, všechny potřebné závislosti jsou také nainstalované. Upravit **MainActivity.cs** a přidejte následující `using` příkaz: 

```csharp
using Android.Gms.Common;
```

Tento příkaz vytvoří `GoogleApiAvailability` třídy v **Xamarin.GooglePlayServices.Base** k dispozici pro **FCMClient** kódu. 
`GoogleApiAvailability` slouží ke kontrole přítomnosti služby Google Play. 

### <a name="add-the-xamarin-firebase-messaging-package"></a>Přidat Xamarin Firebase balíčku pro zasílání zpráv

Pro příjem zpráv z FCM, [Xamarin Firebase - zasílání zpráv](https://www.nuget.org/packages/Xamarin.Firebase.Messaging/) balíček NuGet musí být přidán do projektu aplikace. Bez tohoto balíčku aplikace platformy Android dostávat zprávy ze serverů FCM.

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

1.  V sadě Visual Studio, klikněte pravým tlačítkem na **odkazy > spravovat balíčky NuGet...** . 

2.  Zkontrolujte **zahrnout předběžné verze** a vyhledejte **Xamarin.Firebase.Messaging**. 

3.  Instalaci tohoto balíčku do **FCMClient** projektu: 

    [![Nainstalovat Xamarin Firebase zasílání zpráv](remote-notifications-with-fcm-images/03-firebase-messaging-vs-sml.png)](remote-notifications-with-fcm-images/03-firebase-messaging-vs.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

1.  V sadě Visual Studio pro Mac, klikněte pravým tlačítkem na **balíčků > přidat balíčky...** . 

2.  Zkontrolujte **zobrazit předběžné verze balíčků** a vyhledejte **Xamarin.Firebase.Messaging**. 

3.  Instalaci tohoto balíčku do **FCMClient** projektu: 

    [![Nainstalovat Xamarin Firebase zasílání zpráv](remote-notifications-with-fcm-images/03-firebase-messaging-xs-sml.png)](remote-notifications-with-fcm-images/03-firebase-messaging-xs.png#lightbox)

-----
 
Při instalaci **Xamarin.Firebase.Messaging**, všechny potřebné závislosti jsou také nainstalované.

Potom upravte **MainActivity.cs** a přidejte následující `using` příkazy:

```csharp
using Firebase.Messaging;
using Firebase.Iid;
using Android.Util;
```

Ujistěte se, typy v první dva příkazy **Xamarin.Firebase.Messaging** balíček NuGet, které jsou k dispozici pro **FCMClient** kódu. **Android.Util** přidá funkce protokolování, který se použije k sledovat transakce s FMS. 


### <a name="add-the-google-services-json-file"></a>Přidejte soubor JSON služby Google

Dalším krokem je přidání **google services.json** souboru do kořenového adresáře projektu: 

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

1.  Kopírování **google services.json** ke složce projektu.

2.  Přidat **google services.json** do projektu aplikace (klikněte na tlačítko **zobrazit všechny soubory** v **Průzkumníku řešení**, klikněte pravým tlačítkem na **google-services.json**, pak vyberte **zahrnout do projektu**). 

3.  Vyberte **google services.json** v **Průzkumníku řešení** okno.

4.  V **vlastnosti** podokně nastavit **akce sestavení** k **GoogleServicesJson** (Pokud **GoogleServicesJson** akce sestavení není zobrazený, Uložte a zavřete řešení a poté ho znovu otevřít):

    [![Nastavení akce sestavení na GoogleServicesJson](remote-notifications-with-fcm-images/04-google-services-json-vs-sml.png)](remote-notifications-with-fcm-images/04-google-services-json-vs.png#lightbox)
 
# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

1.  Kopírování **google services.json** ke složce projektu.

2.  Přidat **google services.json** do projektu aplikace. 

3.  Right-click **google-services.json**.

4.  Nastavte **akce sestavení** k **GoogleServicesJson**: 

    [![Nastavení akce sestavení na GoogleServicesJson](remote-notifications-with-fcm-images/04-google-services-json-xs-sml.png)](remote-notifications-with-fcm-images/04-google-services-json-xs.png#lightbox)
 
-----
 

Když **google services.json** se přidá do projektu (a **GoogleServicesJson** akce sestavení je nastavena), procesu sestavení Extrahuje klíč rozhraní API a ID klienta a pak přidá tyto přihlašovací údaje sloučené nebo generované **AndroidManifest.xml** který se nachází v **obj/Debug/android/AndroidManifest.xml**. Tento proces sloučení automaticky přidá všechna oprávnění a další elementy FCM, které jsou potřebné pro připojení k serveru, FCM. 


## <a name="check-for-google-play-services"></a>Zkontrolujte služby Google Play

Nejprve bude vytvoření počátečního rozložení pro uživatelském rozhraní aplikace. Upravit **Resources/layout/Main.axml** a nahraďte jeho obsah následující kód XML: 

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

To `TextView` se použije k zobrazení zprávy, které označuje, zda je nainstalována služby Google Play. Uložit změny do **Main.axml**. Upravit **MainActivity.cs** a přidejte následující deklarace proměnné instance k `MainActivity` třídy: 

```csharp
TextView msgText;
```

Google doporučuje, že aplikace pro Android kontrolovat přítomnost APK služby Google Play před přístupem k funkcím služby Google Play (Další informace najdete v tématu [zkontrolujte služby Google Play](https://firebase.google.com/docs/cloud-messaging/android/client#sample-play)). V následujícím příkladu `OnCreate` metoda bude ověřte, zda služby Google Play je k dispozici předtím, než se aplikace pokusí použít FCM služby. Přidejte následující metodu do `MainActivity` třídy: 

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

Tento kód zkontroluje zařízení, zda je nainstalována APK služby Google Play. Pokud nainstalovaná není, zobrazí se zpráva v `TextBox` , dá pokyn, uživatel APK stáhnout z obchodu Google Play (nebo povolit v nastavení systému zařízení). 

Nahraďte `OnCreate` metoda následujícím kódem: 

```csharp
protected override void OnCreate (Bundle bundle)
{
    base.OnCreate (bundle);
    SetContentView (Resource.Layout.Main);
    msgText = FindViewById<TextView> (Resource.Id.msgText);
    IsPlayServicesAvailable ();
}
```

`IsPlayServicesAvailable` je volána na konci `OnCreate` tak, aby zkontrolujte služby Google Play spustí každý spuštění aplikace. Pokud má vaše aplikace `OnResume` metody by měly volat `IsPlayServicesAvailable` z `OnResume` také. Zcela znovu sestavte a spusťte aplikaci. Pokud všechny správně nakonfigurovaný, byste měli vidět k obrazovce, která vypadá jako na následujícím snímku obrazovky: 

[![Aplikace označuje, že je k dispozici služby Google Play](remote-notifications-with-fcm-images/05-gps-available-sml.png)](remote-notifications-with-fcm-images/05-gps-available.png#lightbox)

Pokud nejste s tímto výsledkem, ověřte, zda je na zařízení nainstalován APK služby Google Play (Další informace najdete v tématu [nastavení nahoru služby Google Play](https://developers.google.com/android/guides/setup)). Také ověřte, že jste přidali **Xamarin.Google.Play.Services.Base** balíčku do vaší **FCMClient** projektu jak je popsáno výše.


## <a name="add-the-instance-id-receiver"></a>Přidejte příjemce ID Instance

Dalším krokem je přidání služba, která rozšiřuje `FirebaseInstanceIdService` pro zpracování vytváření, otáčení a aktualizaci Firebase registrace tokenů. (Další informace o registraci tokeny najdete v tématu [registraci FCM](~/android/data-cloud/google-messaging/firebase-cloud-messaging.md).) `FirebaseInstanceIdService` Služby je vyžadována pro FCM mohli k odesílání zpráv do zařízení. Když `FirebaseInstanceIdService` služby se přidá do klientské aplikace, aplikace, se automaticky přijímat zprávy FCM a zobrazí jako oznámení pokaždé, když je backgrounded aplikace. 

### <a name="declare-the-receiver-in-the-android-manifest"></a>Deklarovat příjemce v manifestu systému Android.

Upravit **AndroidManifest.xml** a vložte následující `<receiver>` elementy do `<application>` části: 

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

-   Deklaruje `FirebaseInstanceIdReceiver` implementace, která poskytuje [jedinečný identifikátor](https://developers.google.com/instance-id/) pro každou instanci aplikace. Tento příjemce také ověří a autorizuje akce. 

-   Deklaruje interní `FirebaseInstanceIdInternalReceiver` implementace, která se používá ke spuštění služby bezpečně.

`FirebaseInstanceIdReceiver` Je `WakefulBroadcastReceiver` která přijme `FirebaseInstanceId` a `FirebaseMessaging` události a zajišťuje jejich do třídy, které pocházejí z `FirebaseInstanceIdService`. 

### <a name="implement-the-firebase-instance-id-service"></a>Implementace služby Firebase instanci ID

Práce s FCM registrace aplikace používá vlastní `FirebaseInstanceIdService` služba, která zadáte. 
`FirebaseInstanceIdService` provede následující kroky: 

1.  Používá [rozhraní API ID Instance](https://developers.google.com/android/reference/com/google/android/gms/iid/InstanceID) ke generování tokenů zabezpečení, které autorizovat aplikaci klienta pro přístup k FCM a aplikačního serveru. Naopak aplikace získá zpět registrace tokenu z FCM. 

2.  Předává registrační token na aplikační server, pokud to vyžaduje na aplikační server.

Přidat nový soubor s názvem **MyFirebaseIIDService.cs** a jeho kód šablony nahraďte následujícím kódem: 

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

Tato služba se implementuje `OnTokenRefresh` metoda, která je volána, když registrační token je původně vytvořené nebo změněné. Když `OnTokenRefresh` běží, načte nejnovější tokenu z `FirebaseInstanceId.Instance.Token` (který se aktualizuje asynchronně pomocí FCM). V tomto příkladu je zaznamenána aktualizovat tokenu, aby ho bylo možné zobrazit v okně výstupu: 

```csharp
var refreshedToken = FirebaseInstanceId.Instance.Token;
Log.Debug(TAG, "Refreshed token: " + refreshedToken);
```

`OnTokenRefresh` je volána zřídka: se používá k aktualizaci tokenu podle následujících okolností: 

-   Pokud aplikace se instaluje nebo odinstaluje.

-   Když uživatel odstraní data aplikací. 

-   Když aplikace vymaže ID Instance.

-   Pokud token zabezpečení je ohrožené zabezpečení.

Podle Google [Instance ID](https://developers.google.com/instance-id/guides/android-implementation) dokumentace, službu FCM Instance ID bude požadovat, aby aplikace aktualizovat jeho token pravidelně (obvykle každých 6 měsíců). 

`OnTokenRefresh` také voláním `SendRegistrationToAppServer` k přidružení uživatele registrace tokenu s účtem na straně serveru (pokud existuje), je možný díky aplikaci: 

```csharp
void SendRegistrationToAppServer (string token)
{
    // Add custom implementation here as needed.
}
```

Protože tato implementace závisí na návrh na aplikační server, textu empty – metoda je k dispozici v tomto příkladu. Pokud server aplikace vyžaduje FCM registrační informace, upravte `SendRegistrationToAppServer` k přidružení uživatele FCM instance ID tokenu účtem všechny serverové udržované vaší aplikace. (Všimněte si, že je token neprůhledného do klientské aplikace.) 

Při odeslání token na aplikační server `SendRegistrationToAppServer` musí udržovat logickou hodnotu označující, zda je token byl odeslán do serveru. Pokud tato logická hodnota NEPRAVDA, `SendRegistrationToAppServer` odešle token do aplikačního serveru &ndash; , jinak hodnota tokenu již odeslal na aplikační server v předchozí volání. V některých případech (jako je tato `FCMClient` příkladu), server aplikace nemusí token; proto tato metoda se nevyžaduje v tomto příkladu. 

## <a name="implement-client-app-code"></a>Kód implementace klientské aplikace

Teď, když příjemce služby jsou na místě, kód klienta aplikace lze zapsat chcete využít výhod těchto služeb. V následujících částech je tlačítko Přidat do uživatelského rozhraní k protokolování registrační token (také nazývané *Instance ID token*), a další kód je přidán do `MainActivity` zobrazíte `Intent` informace při spuštění aplikace z oznámení: 

[![Token tlačítko Přidat na obrazovku aplikace](remote-notifications-with-fcm-images/06-log-token-sml.png)](remote-notifications-with-fcm-images/06-log-token.png#lightbox)

### <a name="log-tokens"></a>Tokeny protokolu

Kódu přidaném v tomto kroku je určena pouze pro demonstrační účely &ndash; produkční klienta aplikace by měla mít není nutné se přihlásit registrace tokenů. Upravit **Resources/layout/Main.axml** a přidejte následující `Button` deklarace ihned po `TextView` element: 

```xml
<Button
  android:id="@+id/logTokenButton"
  android:layout_width="match_parent"
  android:layout_height="wrap_content"
  android:layout_gravity="center_horizontal"
  android:text="Log Token" />
```

Upravit **MainActivity.cs** a přidejte následující deklarace členů k `MainActivity` třídy:

```csharp
const string TAG = "MainActivity";
```

Přidejte následující kód do konce `OnCreate` metoda: 

```csharp
var logTokenButton = FindViewById<Button>(Resource.Id.logTokenButton);
logTokenButton.Click += delegate {
    Log.Debug(TAG, "InstanceID token: " + FirebaseInstanceId.Instance.Token);
};
```

Tento kód protokoly aktuální token do okna výstupu při **protokolu tokenu** je stisknuté tlačítko. 

### <a name="handle-notification-intents"></a>Zpracování oznámení záměry

Když uživatel klepnutím oznámení vystavené **FCMClient**, všechna data doplňujícími tohoto oznámení zprávy je k dispozici v `Intent` funkce. Upravit **MainActivity.cs** a přidejte následující kód do horní části `OnCreate` – metoda (před voláním `IsPlayServicesAvailable`): 

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

Spouštěč aplikace `Intent` je aktivována, když uživatel klepnutím své zprávy oznámení, takže tento kód se přihlaste všechna související data `Intent` do okna výstupu. Pokud jiné `Intent` musí být aktivováno, `click_action` pole oznámení musí být nastavené na který `Intent` (spouštěč `Intent` se používá při ne `click_action` je zadána). 


## <a name="background-notifications"></a>Pozadí oznámení

Sestavení a spuštění **FCMClient** aplikace. **Protokolu tokenu** je zobrazeno tlačítko:

[![Je zobrazeno tlačítko Token protokolu](remote-notifications-with-fcm-images/06-log-token-sml.png)](remote-notifications-with-fcm-images/06-log-token.png#lightbox)

Klepněte **protokolu tokenu** tlačítko. V okně výstupu IDE by se zobrazit zpráva takto: 

[![Token ID instance zobrazí v okně výstupu](remote-notifications-with-fcm-images/07-token-received-sml.png)](remote-notifications-with-fcm-images/07-token-received.png#lightbox)

Dlouhý řetězec označený verzí **tokenu** je token ID instance, který bude vložte do konzoly Firebase &ndash; vyberte a zkopírujte tento řetězec do schránky. Pokud nevidíte instance ID token, přidejte následující řádek na začátek `OnCreate` metodu k ověření, který **google services.json** byla správně analyzovat:

```csharp
Log.Debug(TAG, "google app id: " + GetString(Resource.String.google_app_id));
```

`google_app_id` Hodnota protokolují do okna výstupu by měl odpovídat `mobilesdk_app_id` hodnota zaznamenaná v **google services.json**. 

### <a name="send-a-message"></a>Odeslat zprávu

Přihlaste se k [Firebase konzoly](https://console.firebase.google.com)vyberte projektu, klikněte na tlačítko **oznámení**a klikněte na tlačítko **odeslat vaše první zprávu**: 

[![Vaše první zprávu tlačítko Odeslat](remote-notifications-with-fcm-images/08-first-notification-sml.png)](remote-notifications-with-fcm-images/08-first-notification.png#lightbox)

Na **zprávy vytvářené** stránky, zadejte text zprávy a vyberte **jedno zařízení**. Zkopírujte instance ID token v okně výstupu IDE a vložte ji do **FCM registrační token** pole Firebase konzoly: 

[![Vytvořit dialogové okno zprávy](remote-notifications-with-fcm-images/09-compose-message-sml.png)](remote-notifications-with-fcm-images/09-compose-message.png#lightbox)

Na Android zařízení (nebo emulátoru) na pozadí aplikace klepnutím Android **přehled** tlačítko a klepnou na domovskou obrazovku. Když je zařízení připraveno, klikněte na tlačítko **odeslat zprávu** v konzole Firebase: 

[![Zpráva tlačítko Odeslat](remote-notifications-with-fcm-images/10-send-message-sml.png)](remote-notifications-with-fcm-images/10-send-message.png#lightbox)

Když **Zkontrolujte zprávu** dialogové okno se zobrazí, klikněte na tlačítko **odeslat**.
Na ikonu v oznamovací objevit v oznamovací oblasti (emulátoru nebo zařízení): 

[![Ikona oznámení se zobrazí.](remote-notifications-with-fcm-images/11-notification-icon-sml.png)](remote-notifications-with-fcm-images/11-notification-icon.png#lightbox)

Otevřete na ikonu oznámení a zobrazení zprávy. Oznámení by mělo obsahovat přesně co zadaného do **text zprávy** pole Firebase konzoly: 

[![Oznámení se zobrazí na zařízení](remote-notifications-with-fcm-images/12-notification-sml.png)](remote-notifications-with-fcm-images/12-notification.png#lightbox)

Klepněte na ikonu oznámení se vrátíte do **FCMClient** aplikace. `Intent` Funkce posílá **FCMClient** jsou uvedeny v okně výstupu IDE: 

[![Seznamy záměrné funkce z klíče, ID zprávy a sbalit klíč](remote-notifications-with-fcm-images/13-intent-extras-sml.png)](remote-notifications-with-fcm-images/13-intent-extras.png#lightbox)

V tomto příkladu **z** klíč nastaven na číslo projektu Firebase aplikace (v tomto příkladu `41590732`) a **collapse_key** je nastaven na jeho název balíčku ( **com.xamarin.fcmexample**). Pokud neobdržíte zprávu, zkuste odstranit **FCMClient** aplikace na zařízení (nebo emulátoru) a opakujte výše uvedené kroky. 


> [!NOTE]
> Pokud jste force zavřít aplikaci, FCM se zastaví doručování oznámení. Android zabrání pozadí služby všesměrové nechtěně nebo zbytečně spuštění součásti zastaven aplikací. (Další informace o tomto chování najdete v tématu [spusťte ovládací prvky v aplikacích pro zastavený](https://developer.android.com/about/versions/android-3.1.html#launchcontrols).) Z tohoto důvodu je nutné ručně odinstalovat aplikaci pokaždé, když ji spustit a zastavit z relace ladění &ndash; vynutí FCM vygenerovat nový token, aby zpráv bude nadále přijímat.

### <a name="add-a-custom-default-notification-icon"></a>Přidat vlastní výchozí ikonu oznámení

V předchozím příkladu na ikonu v oznamovací nastavena na ikonu aplikace. Následující kód XML nakonfiguruje vlastní výchozí ikony pro oznámení. Android zobrazí tuto vlastní výchozí ikonu pro všechny zprávy oznámení, kde není explicitně nastavena na ikonu oznámení. 

Přidat vlastní výchozí ikonu oznámení, přidat vaše ikonu **prostředky/drawable** directory upravit **AndroidManifest.xml**a vložte následující `<meta-data>` element na `<application>`části: 

```xml
<meta-data
    android:name="com.google.firebase.messaging.default_notification_icon"
    android:resource="@drawable/ic_stat_ic_notification" />
```

V tomto příkladu na ikonu v oznamovací který se nachází v **prostředky/drawable/vnitropodnikové\_stat\_vnitropodnikové\_notification.png** se použije jako vlastní výchozí ikonu oznámení. Pokud není nakonfigurovaná vlastní výchozí ikonu **AndroidManifest.xml** a žádná ikona je nastavena v datová část oznámení, Android ikona aplikace používá jako ikonu oznámení (jak je vidět na tomto snímku ikonu oznámení výše). 

## <a name="handle-topic-messages"></a>Zpracování zpráv tématu

Kód napsaný doposud zpracovává registrace tokenů a přidá funkce vzdáleného oznámení do aplikace. Další příklad přidá kód, který čeká na *tématu zprávy* a předává je uživateli jako Vzdálená oznámení. Tématu zprávy jsou FCM zprávy, které se odesílají do jednoho nebo více zařízení, která se k příslušné téma odběru. Další informace o zprávách tématu najdete v tématu [zasílání zpráv tématu](~/android/data-cloud/google-messaging/firebase-cloud-messaging.md). 

### <a name="subscribe-to-a-topic"></a>Přihlášení k odběru do tématu

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

Přidá tato konfigurace XML **přihlášení k odběru oznámení do** tlačítko rozložení.
Upravit **MainActivity.cs** a přidejte následující kód do konce `OnCreate` metoda: 

```csharp
var subscribeButton = FindViewById<Button>(Resource.Id.subscribeButton);
subscribeButton.Click += delegate {
    FirebaseMessaging.Instance.SubscribeToTopic("news");
    Log.Debug(TAG, "Subscribed to remote notifications");
};
```

Tento kód vyhledá **přihlášení k odběru oznámení do** tlačítko v rozložení a přiřadí její obslužná rutina kliknutí na kód, který volá `FirebaseMessaging.Instance.SubscribeToTopic`, předejte v sekci odebírané _zprávy_. Když uživatel klepnutím **přihlásit k odběru** tlačítko aplikace přihlásí k odběru _zprávy_ tématu. V následující části _zprávy_ tématu zpráva bude odeslána z grafického uživatelského rozhraní Firebase konzoly oznámení. 

### <a name="send-a-topic-message"></a>Odeslat zprávu tématu

Odinstalovat aplikaci, jej vytvořte znovu a potom ho spusťte znovu. Klikněte **přihlášení k odběru oznámení** tlačítko:

[![Přihlášení k odběru na tlačítko oznámení](remote-notifications-with-fcm-images/14-subscribe-sml.png)](remote-notifications-with-fcm-images/14-subscribe.png#lightbox)

Pokud aplikace se úspěšně připojila, měli byste vidět **úspěšná synchronizace tématu** v prostředí IDE výstup okna: 

[![Výstup – okno zobrazí zprávu úspěšná synchronizace tématu](remote-notifications-with-fcm-images/15-topic-sync-sml.png)](remote-notifications-with-fcm-images/15-topic-sync.png#lightbox)

Použijte následující postup k odesílání zpráv tématu:

1.  V konzole Firebase klikněte na **novou zprávu**. 

2.  Na **zprávy vytvářené** stránky, zadejte text zprávy a vyberte **tématu**. 

3.  V **tématu** rozevírací nabídky vyberte předdefinované tématu **zprávy**: 

    [![Výběr sekci zprávy](remote-notifications-with-fcm-images/16-topic-message-sml.png)](remote-notifications-with-fcm-images/16-topic-message.png#lightbox)

4.  Na Android zařízení (nebo emulátoru) na pozadí aplikace klepnutím Android **přehled** tlačítko a klepnou na domovskou obrazovku. 

5.  Když je zařízení připraveno, klikněte na tlačítko **odeslat zprávu** v konzole Firebase. 

6.  Zkontrolujte okno IDE výstupu a zobrazit **/témata/zprávy** ve výstupu protokolu: 

    [![Se zobrazí zpráva od /topic/news](remote-notifications-with-fcm-images/17-message-arrived-sml.png)](remote-notifications-with-fcm-images/17-message-arrived.png#lightbox)

Když tato zpráva se zobrazí v okně výstupu, by také zobrazit na ikonu v oznamovací v oznamovací oblasti na zařízení s Androidem. Otevřete na ikonu oznámení a zobrazení zprávy tématu: 

[![Téma zpráva se zobrazí jako oznámení](remote-notifications-with-fcm-images/18-other-news-sml.png)](remote-notifications-with-fcm-images/18-other-news.png#lightbox)

Pokud neobdržíte zprávu, zkuste odstranit **FCMClient** aplikace na zařízení (nebo emulátoru) a opakujte výše uvedené kroky. 

## <a name="foreground-notifications"></a>Oznámení popředí

Pokud chcete dostávat oznámení v aplikacích foregrounded, je nutné implementovat `FirebaseMessagingService`. Tato služba je také nutný pro datové části ukládat příjem a odesílání nadřazenému zprávy. Následující příklady ukazují, jak implementovat služba, která rozšiřuje `FirebaseMessagingService` &ndash; výsledná aplikace budou moci zpracovat vzdáleného oznámení, když je spuštěná v popředí. 

### <a name="implement-firebasemessagingservice"></a>Implementace FirebaseMessagingService

`FirebaseMessagingService` Služba obsahuje `OnMessageReceived` metody, která zapisovat pro zpracování příchozí zprávy. Všimněte si, že `OnMessageReceived` je volána pro zprávy s oznámením *pouze* když je aplikace spuštěná v popředí. Když aplikace běží na pozadí, zobrazí se automaticky generované oznámení (jak je předvedeno dříve v tomto návodu). 

Přidat nový soubor s názvem **MyFirebaseMessagingService.cs** a jeho kód šablony nahraďte následujícím kódem: 

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

Všimněte si, že `MESSAGING_EVENT` záměrné filtru musí být deklarován tak, aby nové zprávy FCM směrovány na `MyFirebaseMessagingService`: 

```csharp
[IntentFilter(new[] { "com.google.firebase.MESSAGING_EVENT" })]
```

Když klientská aplikace obdrží zprávu od FCM, `OnMessageReceived` extrahuje obsah zprávy z předané `RemoteMessage` objekt voláním jeho `GetNotification` metoda. V dalším kroku protokolů obsah zprávy, aby ho bylo možné zobrazit v okně výstupu IDE: 

```csharp
Log.Debug(TAG, "From: " + message.From);
Log.Debug(TAG, "Notification Message Body: " + message.GetNotification().Body);
```

> [!NOTE]
> Pokud nastavte zarážky v `FirebaseMessagingService`, vaše ladicí relace může nebo nemusí stiskněte tlačítko tyto zarážky kvůli způsob FCM doručení zprávy.
 

### <a name="send-another-message"></a>Odeslat další zprávu

Odinstalovat aplikaci, jej vytvořte znovu, potom ho spusťte znovu a postupujte podle těchto kroků k odeslání další zprávu:

1.  V konzole Firebase klikněte na **novou zprávu**. 

2.  Na **zprávy vytvářené** stránky, zadejte text zprávy a vyberte **jedno zařízení**. 

3.  Zkopírujte řetězec tokenu z ve výstupním okně IDE a vložte ji do **FCM registrační token** pole konzoly Firebase jako předtím. 

4.  Ujistěte se, že je aplikace spuštěná v popředí a pak klikněte na **odeslat zprávu** v konzole Firebase: 

    [![Odesílání další zprávu z konzoly](remote-notifications-with-fcm-images/19-hello-again-sml.png)](remote-notifications-with-fcm-images/19-hello-again.png#lightbox)

5.  Když **Zkontrolujte zprávu** dialogové okno se zobrazí, klikněte na tlačítko **odeslat**.

6.  Do okna výstupu IDE je zaznamenána příchozí zpráva:

    [![Tělo zprávy vytisknout pro výstup – okno](remote-notifications-with-fcm-images/20-logged-message.png)](remote-notifications-with-fcm-images/20-logged-message.png#lightbox)


### <a name="add-a-local-notifications-sender"></a>Přidání odesílatele místní oznámení

V tomto příkladu zbývající příchozí zpráva FCM převede do místního oznámení, který se spustí, když je aplikace spuštěná v popředí. Upravit **MyFirebaseMessageService.cs** a přidejte následující `using` příkazy: 

```csharp
using FCMClient;
using System.Collections.Generic;
```

Přidejte následující metodu do `MyFirebaseMessagingService`: 

```csharp
void SendNotification(string messageBody, IDictionary<string, string> data)
{
    var intent = new Intent(this, typeof(MainActivity));
    intent.AddFlags(ActivityFlags.ClearTop);
    foreach (string key in data.Keys)
    {
        intent.PutExtra(key, data[key]);
    }
    var pendingIntent = PendingIntent.GetActivity(this, 0, intent, PendingIntentFlags.OneShot);

    var notificationBuilder = new Notification.Builder(this)
        .SetSmallIcon(Resource.Drawable.ic_stat_ic_notification)
        .SetContentTitle("FCM Message")
        .SetContentText(messageBody)
        .SetAutoCancel(true)
        .SetContentIntent(pendingIntent);

    var notificationManager = NotificationManager.FromContext(this);
    notificationManager.Notify(0, notificationBuilder.Build());
}
```

K rozlišení tohoto oznámení z pozadí oznámení, tento kód označí ikonou, která se liší od ikonu applicatiion oznámení. Přidat [vnitropodnikové\_stat\_vnitropodnikové\_notification.png](remote-notifications-with-fcm-images/ic-stat-ic-notification.png) k **prostředky/drawable** a zahrnout jej do **FCMClient** projektu. 

`SendNotification` Používá metoda `Notification.Builder` vytvořit oznámení, a `NotificationManager` je použitý ke spuštění oznámení. Obsahuje oznámení `PendingIntent` který vám umožní uživateli otevřete aplikaci a zobrazit obsah řetězce předaný do `messageBody`. V závislosti na verze Android, které chcete cílit s vaší aplikací, můžete chtít použít `NotificationCompat.Builder` místo. Další informace o `Notification.Builder`, najdete v části [místního oznámení](~/android/app-fundamentals/notifications/local-notifications.md). 

Volání `SendNotification` metoda na konci `OnMessageReceived` metoda: 

```csharp
SendNotification(message.GetNotification().Body, message.Data);
```

V důsledku těchto změn `SendNotification` se spustí pokaždé, když je přijato oznámení, zatímco aplikace je v popředí, a oznámení se zobrazí v oznamovací oblasti. Pokud je aplikace backgrounded, `SendNotification` se nespustí, a místo toho bude spuštěn pozadí oznámení (zobrazené dříve v tomto návodu). 

### <a name="send-the-last-message"></a>Odeslání poslední zprávy

Odinstalovat aplikaci, znovu ji vytvořit, potom ho spusťte znovu a pak použijte následující postup k odeslání poslední zprávy:

1.  V konzole Firebase klikněte na **novou zprávu**. 

2.  Na **zprávy vytvářené** stránky, zadejte text zprávy a vyberte **jedno zařízení**. 

3.  Zkopírujte řetězec tokenu z ve výstupním okně IDE a vložte ji do **FCM registrační token** pole konzoly Firebase jako předtím. 

4.  Ujistěte se, že je aplikace spuštěná v popředí a pak klikněte na **odeslat zprávu** v konzole Firebase: 

    [![Odeslání zprávy popředí](remote-notifications-with-fcm-images/21-console-fg-msg-sml.png)](remote-notifications-with-fcm-images/21-console-fg-msg.png#lightbox)

Tentokrát zprávu, která byla zaznamenána ve výstupním okně je také součástí nové oznámení &ndash; ikonu oznámení se zobrazí na hlavním panelu oznámení, když je aplikace spuštěná v popředí: 

[![Ikony v oznamovací zprávy popředí](remote-notifications-with-fcm-images/22-foreground-icon-sml.png)](remote-notifications-with-fcm-images/22-foreground-icon.png#lightbox)

Když otevřete oznámení, měli vidět poslední zprávu, která byla odeslána z grafického uživatelského rozhraní Firebase konzoly oznámení: 

[![Zobrazí se ikona popředí oznámení popředí](remote-notifications-with-fcm-images/23-foreground-msg-sml.png)](remote-notifications-with-fcm-images/23-foreground-msg.png#lightbox)


## <a name="disconnecting-from-fcm"></a>Odpojení od FCM

Chcete-li zrušit odběr tématu, volejte [UnsubscribeFromTopic](https://firebase.google.com/docs/reference/android/com/google/firebase/messaging/FirebaseMessaging.html#unsubscribeFromTopic%28java.lang.String%29) metodu [FirebaseMessaging](https://firebase.google.com/docs/reference/android/com/google/firebase/messaging/FirebaseMessaging) – třída. Například pro odhlášení odběru _zprávy_ tématu přihlásit k odběru dříve, **Unsubscribe** tlačítko nebylo možné přidat do rozložení s následující kód pro obslužnou rutinu:

```csharp
var unSubscribeButton = FindViewById<Button>(Resource.Id.unsubscribeButton);
unSubscribeButton.Click += delegate {
    FirebaseMessaging.Instance.UnsubscribeFromTopic("news");
    Log.Debug(TAG, "Unsubscribed from remote notifications");
};
```

Chcete-li zrušit registraci zařízení z FCM úplně, odstranit instance ID voláním [DeleteInstanceId](https://firebase.google.com/docs/reference/android/com/google/firebase/iid/FirebaseInstanceId.html#deleteInstanceId%28%29) metodu [FirebaseInstanceId](https://firebase.google.com/docs/reference/android/com/google/firebase/iid/FirebaseInstanceId) – třída. Příklad:

```csharp
FirebaseInstanceId.Instance.DeleteInstanceId();
```

Toto volání metody odstraní instance ID a data s ním spojená. V důsledku toho se zastavilo pravidelné odesílání dat FCM do zařízení.

 
## <a name="troubleshooting"></a>Poradce při potížích

Následující popisujících problémy a řešení, které mohou nastat při použití zasílání zpráv cloudu Firebase s Xamarin.Android.

### <a name="firebaseapp-is-not-initialized"></a>FirebaseApp není inicializován.

V některých případech může zobrazit tato chybová zpráva:

```shell
Java.Lang.IllegalStateException: Default FirebaseApp is not initialized in this process
Make sure to call FirebaseApp.initializeApp(Context) first.
```

Toto je známý problém, který můžete obejít tak, že čištění řešení a projekt znovu sestavit (**sestavení > Vyčistit řešení**, **sestavení > znovu sestavit řešení**). Další informace najdete v tématu to [diskuzi na fóru](https://forums.xamarin.com/discussion/96263/default-firebaseapp-is-not-initialized-in-this-process).


## <a name="summary"></a>Souhrn

Tento názorný postup podrobné kroky pro implementaci zasílání zpráv cloudu Firebase vzdáleného oznámení v aplikaci Xamarin.Android. Je popsaný postup instalace požadované balíčky, které jsou potřebné pro komunikaci FCM a jeho vysvětlení najdete postup konfigurace Android Manifest pro přístup k serverům FCM. Je k dispozici ukázkový kód, který ukazuje, jak chcete zkontrolovat přítomnost služby Google Play. Je prokázat, jak implementovat instance ID procesu naslouchání služby, který vyjedná s FCM pro registrační token a je popsáno, jak tento kód vytvoří pozadí oznámení, když je backgrounded aplikace. Ho popsané postupy k odběru zpráv tématu a jeho poskytuje příklad implementace naslouchací proces služby zpráva, která se používá k přijímat a zobrazovat vzdáleného oznámení, když je aplikace spuštěná v popředí. 


## <a name="related-links"></a>Související odkazy

- [FCMNotifications (ukázka)](https://developer.xamarin.com/samples/monodroid/Firebase/FCMNotifications)
- [Firebase Cloud Messaging](~/android/data-cloud/google-messaging/firebase-cloud-messaging.md)
