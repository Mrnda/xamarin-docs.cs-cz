---
title: Vzdálená oznámení s zasílání zpráv cloudu Google
description: Tento názorný postup obsahuje podrobné vysvětlení způsobu implementace vzdáleného oznámení (také nazývané nabízená oznámení) pomocí služby Google Cloud Messaging v aplikaci Xamarin.Android. Popisuje různé třídy, které je nutné implementovat pro komunikaci s zasílání zpráv cloudu Google (GCM), vysvětluje, jak nastavit oprávnění ve Android Manifest pro přístup do služby GCM a ukazuje, s testovací ukázka programu zasílání zpráv začátku do konce.
ms.prod: xamarin
ms.assetid: 4FC3C774-EF93-41B2-A81E-C6A08F32C09B
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 03/13/2018
ms.openlocfilehash: 969b1b36659ac52782d30a1840ba352524e5e3c6
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/04/2018
---
# <a name="remote-notifications-with-google-cloud-messaging"></a>Vzdálená oznámení s zasílání zpráv cloudu Google

_Tento názorný postup obsahuje podrobné vysvětlení způsobu implementace vzdáleného oznámení (také nazývané nabízená oznámení) pomocí služby Google Cloud Messaging v aplikaci Xamarin.Android. Popisuje různé třídy, které je nutné implementovat pro komunikaci s zasílání zpráv cloudu Google (GCM), vysvětluje, jak nastavit oprávnění ve Android Manifest pro přístup do služby GCM a ukazuje, s testovací ukázka programu zasílání zpráv začátku do konce._

## <a name="gcm-notifications-overview"></a>Přehled oznámení GCM

V tomto návodu vytvoříme aplikace Xamarin.Android, která používá zasílání zpráv cloudu Google (GCM) k implementaci vzdáleného oznámení (také označované jako *nabízená oznámení*). Jsme budete implementovat různé záměr a naslouchací proces služby, které používají GCM pro vzdálené zasílání zpráv a otestujeme naše implementace pomocí příkazového řádku programu, který simuluje aplikační server. 

Upozorňujeme, že zasílání zpráv cloudu Firebase (FCM) je novou verzi GCM &ndash; Google důrazně doporučuje použití FCM namísto GCM. Pokud aktuálně používáte GCM, upgrade na FCM je vhodné. Další informace o FCM najdete v tématu [Firebase Cloud Messaging](~/android/data-cloud/google-messaging/firebase-cloud-messaging.md). 

Než budete pokračovat v tomto průvodci, musíte získat potřebné pověření pro použití Google GCM serverů; Tento proces je vysvětleno v [Google Cloud Messaging](~/android/data-cloud/google-messaging/google-cloud-messaging.md). Zejména budete potřebovat *klíč rozhraní API* a *ID odesílatele* vložení do ukázkový kód uvedené v tomto návodu. 

K vytvoření aplikace Xamarin.Android povoleno GCM klienta použijeme následující kroky:

1.  Nainstalujte další balíčky požadované pro komunikaci se servery služby GCM.
2.  Konfigurace aplikace oprávnění pro přístup k serverům služby GCM.
3.  Implementaci kódu, které chcete zkontrolovat přítomnost služby Google Play. 
4.  Implementujte registrace záměrné služba, která vyjedná s GCM pro token registrace.
5.  Implementujte instance ID procesu naslouchání služby, která čeká na aktualizace tokenu registrace ze služby GCM.
6.  Implementace služby naslouchací proces služby GCM, která přijímá vzdálené zprávy ze serveru aplikace pomocí služby GCM.

Tato aplikace bude používat nové funkce služby GCM známé jako *zasílání zpráv tématu*. V tématu zasílání zpráv, na aplikační server odešle zprávu do tématu, nikoli na seznam jednotlivých zařízení. Zařízení, která přihlášení k odběru tohoto tématu můžete přijímat zprávy téma jako nabízené oznámení. Další informace o zasílání zpráv tématu GCM, najdete v části Google [implementace zasílání zpráv tématu](https://developers.google.com/cloud-messaging/topic-messaging). 

Když klient aplikace je připravená, jsme budete implementovat aplikace příkazového řádku jazyka C#, odešle nabízených oznámení do vaší aplikace klienta pomocí služby GCM. 

## <a name="walkthrough"></a>Návod

Pokud chcete začít, vytvoříme nové prázdné řešení názvem **RemoteNotifications**. Dále umožňuje přidat nový projekt Android k tomuto řešení, která je založená na **aplikace pro Android** šablony. Umožňuje volání tento projekt **ClientApp**. (Pokud nejste obeznámeni s vytváření projektů Xamarin.Android, přečtěte si téma [Hello, Android](~/android/get-started/hello-android/hello-android-quickstart.md).) **ClientApp** projektu bude obsahovat kód pro klientskou aplikaci Xamarin.Android, která bude přijímat vzdáleného oznámení pomocí služby GCM. 

### <a name="add-required-packages"></a>Přidat požadované balíčky

Před můžeme implementovat naše kód aplikace klienta, jsme musíte nainstalovat několik balíčků, které jsme budete používat pro komunikaci s GCM. Také jsme musíte přidat aplikace Google Play Storu naše zařízení, pokud již není nainstalována.

#### <a name="add-the-xamarin-google-play-services-gcm-package"></a>Přidejte balíček GCM Xamarin Google Play Services

Pro příjem zpráv z Google Cloud Messaging [služby Google Play](https://www.nuget.org/packages/Xamarin.GooglePlayServices.Gcm/) framework musí být na zařízení. Bez toto rozhraní nemůže aplikace platformy Android ze serverů GCM přijímat zprávy. Google Play Services běží na pozadí při je zapnutý zařízení s Androidem, tiše naslouchání zpráv ze služby GCM. Při obdržení těchto zpráv, převede zprávy na záměry služby Google Play a potom vysílá těchto tříd Intent k aplikacím, které jste zaregistrovali pro ně. 

V sadě Visual Studio, klikněte pravým tlačítkem na **odkazy > spravovat balíčky NuGet...** ; v sadě Visual Studio pro Mac, klikněte pravým tlačítkem na **balíčků > přidat balíčky...** . Vyhledejte **Xamarin Google Play Services - GCM** a instalaci tohoto balíčku do **ClientApp** projektu: 

[![Instalace služby Google Play](remote-notifications-with-gcm-images/1-google-play-services-sml.png)](remote-notifications-with-gcm-images/1-google-play-services.png#lightbox)

Při instalaci **Xamarin Google Play Services - GCM**, **Xamarin Google Play Services - základní** se automaticky nainstaluje. Pokud dojde k chybě, změňte projektu *minimální Android k cíli* nastavení na hodnotu než **zkompilovat pomocí sady SDK verze** a znovu zkuste spustit instalaci NuGet. 

Potom upravte **MainActivity.cs** a přidejte následující `using` příkazy:

```csharp
using Android.Gms.Common;
using Android.Util;
```

Díky tomu typy v Google Play Services g balíčku k dispozici pro naše kódu a přidá funkce protokolování, které budeme používat ke sledování našich transakcí s g. 

#### <a name="google-play-store"></a>Obchod Google Play

Pro příjem zpráv ze služby GCM, musí být aplikace Google Play Storu nainstalována v zařízení. (Vždy, když aplikace Google Play nainstalována na zařízení, Google Play Storu nainstaluje taky, takže je pravděpodobné, že je již nainstalována na testovací zařízení.) Bez Google Play nemůže aplikace platformy Android přijímat zprávy ze služby GCM. Pokud ještě nemáte nainstalovanou na zařízení s aplikací Google Play Storu, navštivte [Google Play](https://support.google.com/googleplay) webový server ke stažení a instalaci Google Play. 

Alternativně můžete použít emulátoru Androidu systémem Android 2.2 nebo vyšší místo testovací zařízení (nemáte instalace obchod Google Play v emulátoru Androidu). Pokud používáte emulátor, Wi-Fi je nutné použít pro připojení do služby GCM a jak je popsáno dále v tomto návodu, je nutné otevřít několik portů v bráně firewall sítě Wi-Fi. 

### <a name="set-the-package-name"></a>Nastavte název balíčku

V [Google Cloud Messaging](~/android/data-cloud/google-messaging/google-cloud-messaging.md), jsme zadali název balíčku pro naše aplikace s podporou služby GCM (Tento název balíčku slouží taky jako *ID aplikace* který je přidružen naše klíč rozhraní API a ID odesílatele). Umožňuje otevřít vlastnosti **ClientApp** projektu a nastavte název balíčku na tento řetězec. V tomto příkladu jsme nastavte název balíčku na `com.xamarin.gcmexample`:

[![Nastavení názvu balíčku](remote-notifications-with-gcm-images/2-package-name-sml.png)](remote-notifications-with-gcm-images/2-package-name.png#lightbox)

Klientská aplikace bude nelze přijmout registrační token ze služby GCM, pokud nemá tento název balíčku *přesně* shodovat s názvem balíčku, který jsme zadat v konzole pro vývojáře Google. 

### <a name="add-permissions-to-the-android-manifest"></a>Přidejte oprávnění do manifestu systému Android.

Aplikace platformy Android musí mít následující oprávnění, dřív než ji může přijímat oznámení z Google Cloud Messaging: 

-   `com.google.android.c2dm.permission.RECEIVE` &ndash; Uděluje oprávnění k naší aplikaci zaregistrovat a přijmout zprávy z Google Cloud Messaging. (Jaké `c2dm` znamená? To znamená _cloudu pro zasílání zpráv zařízení_, což je zastaralý předchůdce do služby GCM. 
    Dál používá GCM `c2dm` v mnoha řetězců jeho oprávnění.) 

-   `android.permission.WAKE_LOCK` &ndash; (Volitelné) Zařízení procesoru z brání přejít do režimu spánku při naslouchání zprávy. 

-   `android.permission.INTERNET` &ndash; Uděluje přístup k Internetu, tak klientské aplikace může komunikovat se službou GCM. 

-   *název_balíčku* `.permission.C2D_MESSAGE` &ndash; registruje aplikace Android a požádá o oprávnění k výhradně přijímat všechny C2D zprávy (cloudu do zařízení). *Název_balíčku* předpona je stejný jako vaše ID aplikace. 

Vytvoříme v manifestu systému Android tato oprávnění. Umožňuje upravit **AndroidManifest.xml** a nahraďte jeho obsah s následující kód XML: 

```xml
<?xml version="1.0" encoding="utf-8"?>
<manifest xmlns:android="http://schemas.android.com/apk/res/android" 
    package="YOUR_PACKAGE_NAME" 
    android:versionCode="1" 
    android:versionName="1.0" 
    android:installLocation="auto">
    <uses-permission android:name="com.google.android.c2dm.permission.RECEIVE" />
    <uses-permission android:name="android.permission.WAKE_LOCK" />
    <uses-permission android:name="android.permission.INTERNET" />
    <uses-permission android:name="YOUR_PACKAGE_NAME.permission.C2D_MESSAGE" />
    <permission android:name="YOUR_PACKAGE_NAME.permission.C2D_MESSAGE" 
                android:protectionLevel="signature" />
    <application android:label="ClientApp" android:icon="@drawable/Icon">
    </application>
</manifest>
```

Ve výše uvedené XML, změňte *YOUR_PACKAGE_NAME* na název balíčku pro projekt klientské aplikace. Například `com.xamarin.gcmexample`. 

### <a name="check-for-google-play-services"></a>Zkontrolujte služby Google Play

V tomto návodu vytváříme holou aplikace s jedním `TextView` v uživatelském rozhraní. Tato aplikace nebude znamenat přímo interakci s GCM. Místo toho jsme budete sledovat okno výstupu a zobrazit jak metodou handshake naše aplikace s GCM, a když dorazí zkontrolujeme panelu oznámení pro nová oznámení. 

Nejdříve vytvoříme rozložení pro oblast zprávy. Upravit **Resources.layout.Main.axml** a nahraďte jeho obsah s následující kód XML: 

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

Uložit **Main.axml** a zavřete ho.

Při spuštění klienta aplikace, chceme a ověřit, zda je k dispozici služby Google Play, než jsme pokusí kontaktovat GCM. Upravit **MainActivity.cs** a nahraďte ``count`` instance deklarace proměnné s následující deklarace proměnné instance: 

```csharp
TextView msgText;
```

Dál přidejte následující metodu do **MainActivity** třídy: 

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
            msgText.Text = "Sorry, this device is not supported";
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

Tento kód zkontroluje zařízení, zda je nainstalována APK služby Google Play. Pokud nainstalovaná není, zobrazí se zpráva v oblasti zpráva, která se dá pokyn uživateli APK stáhnout z obchodu Google Play (nebo povolit v nastavení systému zařízení). Vzhledem k tomu, že nám chcete spustit tuto kontrolu při spuštění klienta aplikace, přidáme volání této metody na konci `OnCreate`. 


Potom nahraďte `OnCreate` metoda následujícím kódem:

```csharp
protected override void OnCreate (Bundle bundle)
{
    base.OnCreate (bundle);

    SetContentView (Resource.Layout.Main);
    msgText = FindViewById<TextView> (Resource.Id.msgText);

    IsPlayServicesAvailable ();
}
```

Tento kód zkontroluje přítomnost APK služby Google Play a zapíše výsledek do oblasti zpráv. 

Pojďme zcela znovu sestavte a spusťte aplikaci. Měli byste vidět k obrazovce, která vypadá jako na následujícím snímku obrazovky: 

[![Je k dispozici služby Google Play](remote-notifications-with-gcm-images/3-first-screen-sml.png)](remote-notifications-with-gcm-images/3-first-screen.png#lightbox)

Pokud nejste s tímto výsledkem, ověřte, zda je APK služby Google Play nainstalovány v zařízení a zda **Xamarin Google Play Services - GCM** balíčku se přidá do vašeho **ClientApp** projektu jak je popsáno dříve. Pokud dojde k chybě sestavení, zkuste čištění řešení a znovu sestavit. 

V dalším kroku jsme budete psát kód kontaktovat GCM a získat zpět token registrace.

### <a name="register-with-gcm"></a>Registraci s GCM

Předtím, než aplikace může přijímat oznámení vzdálené ze serveru aplikace, musí registraci s GCM a získat zpět token registrace. Práci při registraci aplikace GCM používá `IntentService` , vytvoříme. Naše `IntentService` provede následující kroky: 

1.  Používá [Identifikátor InstanceID](https://developers.google.com/instance-id/) rozhraní API pro generování tokenů zabezpečení, které povolují naše klientskou aplikaci pro přístup k serveru aplikace. Naopak jsme získat zpět registrace token ze služby GCM.

2.  Předává registrační token na aplikační server (pokud to vyžaduje aplikačního serveru).

3.  Odběratel u jednoho nebo více kanálů oznámení tématu.

Po implementaci to `IntentService`, otestujeme zobrazit, pokud se nám získat zpět registrace token ze služby GCM.

Přidat nový soubor s názvem **RegistrationIntentService.cs** a nahraďte kód šablony následujícím kódem:


```csharp
using System;
using Android.App;
using Android.Content;
using Android.Util;
using Android.Gms.Gcm;
using Android.Gms.Gcm.Iid;

namespace ClientApp
{
    [Service(Exported = false)]
    class RegistrationIntentService : IntentService
    {
        static object locker = new object();

        public RegistrationIntentService() : base("RegistrationIntentService") { }

        protected override void OnHandleIntent (Intent intent)
        {
            try
            {
                Log.Info ("RegistrationIntentService", "Calling InstanceID.GetToken");
                lock (locker)
                {
                    var instanceID = InstanceID.GetInstance (this);
                    var token = instanceID.GetToken (
                        "YOUR_SENDER_ID", GoogleCloudMessaging.InstanceIdScope, null);

                    Log.Info ("RegistrationIntentService", "GCM Registration Token: " + token);
                    SendRegistrationToAppServer (token);
                    Subscribe (token);
                }
            }
            catch (Exception e)
            {
                Log.Debug("RegistrationIntentService", "Failed to get a registration token");
                return;
            }
        }

        void SendRegistrationToAppServer (string token)
        {
            // Add custom implementation here as needed.
        }

        void Subscribe (string token)
        {
            var pubSub = GcmPubSub.GetInstance(this);
            pubSub.Subscribe(token, "/topics/global", null);
        }
    }
}
```

Ve výše uvedený ukázkový kód, změňte *YOUR_SENDER_ID* číslo ID odesílatele pro projekt klientské aplikace. Pokud chcete získat ID odesílatele pro svůj projekt: 

1.  Přihlaste se [Google Cloud Console](https://console.cloud.google.com/) a vyberte název projektu vyžádání nabídku. V **projektu informace** podokně, který se zobrazí u projektu, klikněte na tlačítko **přejděte na nastavení projektu**:

    [![Výběr XamarinGCM projektu](remote-notifications-with-gcm-images/7-choose-project-sml.png)](remote-notifications-with-gcm-images/7-choose-project.png#lightbox)

2.  Na **nastavení** stránky, vyhledejte **číslo projektu** &ndash; jedná se o ID odesílatele pro svůj projekt:

    [![Zobrazí číslo projektu](remote-notifications-with-gcm-images/9-project-number-sml.png)](remote-notifications-with-gcm-images/9-project-number.png#lightbox)

My chceme spustit naše `RegistrationIntentService` při spuštění vaší aplikace. Upravit **MainActivity.cs** a upravovat `OnCreate` metoda tak, aby naše `RegistrationIntentService` spustí po jsme kontrolovat přítomnost služby Google Play: 

```csharp
protected override void OnCreate (Bundle bundle)
{
    base.OnCreate (bundle);

    SetContentView(Resource.Layout.Main);
    msgText = FindViewById<TextView> (Resource.Id.msgText);

    if (IsPlayServicesAvailable ())
    {
        var intent = new Intent (this, typeof (RegistrationIntentService));
        StartService (intent);
    }
}
```

Nyní Podívejme se na každá část `RegistrationIntentService` pochopit, jak to funguje. 

Nejdřív jsme anotaci naše `RegistrationIntentService` s následující atribut označuje, že naše služba tak, aby se mohl vytvořit jeho instanci systému: 

```csharp
[Service (Exported = false)]
```

`RegistrationIntentService` Konstruktor názvy pracovní vlákno *RegistrationIntentService* usnadnění ladění. 

```csharp
public RegistrationIntentService() : base ("RegistrationIntentService") { }
```

Základní funkce služby `RegistrationIntentService` se nachází v `OnHandleIntent` metoda. Projděme tento kód, který chcete zobrazit, jak registraci vaší aplikace do služby GCM.


##### <a name="request-a-registration-token"></a>Žádost o registraci tokenu

`OnHandleIntent` nejprve volá Google [InstanceID.GetToken](https://developers.google.com/android/reference/com/google/android/gms/iid/InstanceID.html#getToken&#40;java.lang.String,%20java.lang.String&#41;) metoda k vyžádání tokenu registrace ze služby GCM. Jsme zabalení tento kód v `lock` pro ochranu proti možnost více tříd Intent registrace neprobíhaly současně &ndash; `lock` zajistí, že jsou tyto záměry postupně zpracovány. Pokud se nám nepovedlo získat token registrace, je vyvolána výjimka, a jsme chybu do protokolu. Pokud registrace úspěšná, `token` je nastaven na registrační token My zpět ze služby GCM: 

```csharp
static object locker = new object ();
...
try
{
    lock (locker)
    {
        var instanceID = InstanceID.GetInstance (this);
        var token = instanceID.GetToken (
            "YOUR_SENDER_ID", GoogleCloudMessaging.InstanceIdScope, null);
        ...
    }
}
catch (Exception e)
{
    Log.Debug ...
```

##### <a name="forward-the-registration-token-to-the-app-server"></a>Předávání registrační Token na aplikační server

Pokud jsme registrační token (tedy bez byla vyvolána výjimka), se nazývá `SendRegistrationToAppServer` k přidružení uživatele registrace tokenu s účtem na straně serveru (pokud existuje) který je spravován v naší aplikaci. Protože tato implementace závisí na návrh na aplikační server, je tady uvedené prázdnou metodu: 

```csharp
void SendRegistrationToAppServer (string token)
{
    // Add custom implementation here as needed.
}
```

V některých případech se nemusí na aplikační server token registrace uživatele; Tato metoda je v takovém případě lze vynechat. Při odeslání token registrace na aplikační server `SendRegistrationToAppServer` musí udržovat logickou hodnotu označující, zda je token byl odeslán do serveru. Pokud tato logická hodnota NEPRAVDA, `SendRegistrationToAppServer` odešle token do aplikačního serveru &ndash; , jinak hodnota tokenu již odeslal na aplikační server v předchozí volání. 


##### <a name="subscribe-to-the-notification-topic"></a>Přihlášení k odběru oznámení téma

V dalším kroku říkáme naše `Subscribe` metoda udávajících do služby GCM, který chceme k odběru oznámení téma. V `Subscribe`, zavoláme [GcmPubSub.Subscribe](https://developers.google.com/android/reference/com/google/android/gms/gcm/GcmPubSub.html#subscribe&#40;java.lang.String,%20java.lang.String,%20android.os.Bundle&#41;) rozhraní API pro přihlášení k odběru naší klientskou aplikaci na všechny zprávy v části `/topics/global`:

```csharp
void Subscribe (string token)
{
    var pubSub = GcmPubSub.GetInstance(this);
    pubSub.Subscribe(token, "/topics/global", null);
}
```

Na aplikační server musí odeslání zpráv oznámení `/topics/global` Pokud jsme se k jejich odběru. Všimněte si, že název tématu v části `/topics` může být nic chcete, tak dlouho, dokud na tyto názvy shodnou na aplikační server a klientské aplikace. (Tady jsme zvolili název `global` indikující, že má být pro příjem zpráv u všech témat podporována serverem aplikace.) 

Informace o tématu GCM zasílání zpráv na straně serveru najdete v tématu Google [odeslat zasílání zpráv na témata](https://developers.google.com/cloud-messaging/topic-messaging). 


#### <a name="implement-an-instance-id-listener-service"></a>Implementace služby naslouchací proces ID Instance

Registrace tokenů jsou jedinečné a zabezpečené; ale klientská aplikace (nebo GCM) pravděpodobně nutné aktualizovat token registrace v případě přeinstalace aplikace nebo chybě zabezpečení. Z tohoto důvodu jsme musí implementovat `InstanceIdListenerService` který reaguje na požadavky tokenu aktualizace ze služby GCM. 

Přidat nový soubor s názvem **InstanceIdListenerService.cs** a nahraďte kód šablony následujícím kódem: 

```csharp
using Android.App;
using Android.Content;
using Android.Gms.Gcm.Iid;

namespace ClientApp
{
    [Service(Exported = false), IntentFilter(new[] { "com.google.android.gms.iid.InstanceID" })]
    class MyInstanceIDListenerService : InstanceIDListenerService
    {
        public override void OnTokenRefresh()
        {
            var intent = new Intent (this, typeof (RegistrationIntentService));
            StartService (intent);
        }
    }
}
```

Přidání poznámek ke `InstanceIdListenerService` s následující atribut znamená, zda je služba nechcete vytvořit instanci v systému a aby mohl přijímat tokenu registrace GCM (také nazývané *instance ID*) aktualizovat požadavky: 

```csharp
[Service(Exported = false), IntentFilter(new[] { "com.google.android.gms.iid.InstanceID" })]
```

`OnTokenRefresh` Metoda v naší službě spouští `RegistrationIntentService` tak, aby ji možné zachytit nový token registrace.


#### <a name="test-registration-with-gcm"></a>Test registraci s GCM

Pojďme zcela znovu sestavte a spusťte aplikaci. Pokud se zobrazí úspěšně registrační token ze služby GCM, registrační token má být zobrazena v okně výstupu. Příklad: 

```shell
D/Mono    ( 1934): Assembly Ref addref ClientApp[0xb4ac2400] -> Xamarin.GooglePlayServices.Gcm[0xb4ac2640]: 2
I/RegistrationIntentService( 1934): Calling InstanceID.GetToken
I/RegistrationIntentService( 1934): GCM Registration Token: f8LdveCvXig:APA91bFIsjUAbP-V8TPQdLR89qQbEJh1SYG38AcCbBUf34z5gSdUc5OsXrgs93YFiGcRSRafPfzkz23lf3-LvYV1CwrFheMjHgwPeFSh12MywnRIhz

```

### <a name="handle-downstream-messages"></a>Zpracování zpráv příjem dat 

Kód, který jsme byla implementována proto mnohem je pouze "nastavení" kód; zkontroluje, zda služby Google Play je nainstalován a vyjedná s GCM a aplikačního serveru pro přípravu naše klientskou aplikaci pro příjem oznámení vzdálené. Ale máme ještě pro implementaci kódu, který ve skutečnosti obdrží a zpracuje zprávy podřízené oznámení. Chcete-li to provést, musíme implementovat *naslouchací proces služby GCM*. Tato služba přijímá tématu zprávy ze serveru aplikace a místně je vysílá jako oznámení. Po implementaci této služby, vytvoříme program test k odesílání zpráv do služby GCM, jsme viděli, pokud naše implementace funguje správně. 


#### <a name="add-a-notification-icon"></a>Přidat ikonu oznámení

Přidejme nejprve malé ikony, které se zobrazí v oznamovací oblasti při spuštění naše oznámení. Můžete zkopírovat [ikonu](remote-notifications-with-gcm-images/ic-stat-ic-notification.png) do projektu nebo vytvořit vlastní vlastní ikonu. Soubor ikony jsme budete název **ic_stat_button_click.png** a zkopírujte ho do **prostředky/drawable** složky. Nezapomeňte použít **Přidat > existující položka...**  zahrnout tento soubor ikony ve vašem projektu.


#### <a name="implement-a-gcm-listener-service"></a>Implementace naslouchací proces služby GCM

Přidat nový soubor s názvem **GcmListenerService.cs** a nahraďte kód šablony následujícím kódem:

```csharp
using Android.App;
using Android.Content;
using Android.OS;
using Android.Gms.Gcm;
using Android.Util;

namespace ClientApp
{
    [Service (Exported = false), IntentFilter (new [] { "com.google.android.c2dm.intent.RECEIVE" })]
    public class MyGcmListenerService : GcmListenerService
    {
        public override void OnMessageReceived (string from, Bundle data)
        {
            var message = data.GetString ("message");
            Log.Debug ("MyGcmListenerService", "From:    " + from);
            Log.Debug ("MyGcmListenerService", "Message: " + message);
            SendNotification (message);
        }

        void SendNotification (string message)
        {
            var intent = new Intent (this, typeof(MainActivity));
            intent.AddFlags (ActivityFlags.ClearTop);
            var pendingIntent = PendingIntent.GetActivity (this, 0, intent, PendingIntentFlags.OneShot);

            var notificationBuilder = new Notification.Builder(this)
                .SetSmallIcon (Resource.Drawable.ic_stat_ic_notification)
                .SetContentTitle ("GCM Message")
                .SetContentText (message)
                .SetAutoCancel (true)
                .SetContentIntent (pendingIntent);

            var notificationManager = (NotificationManager)GetSystemService(Context.NotificationService);
            notificationManager.Notify (0, notificationBuilder.Build());
        }
    }
}
```

Podívejme se na každá část naší `GcmListenerService` pochopit, jak to funguje. 

Nejdřív jsme anotaci `GcmListenerService` s atributem znamená, že tato služba je tak, aby se mohl vytvořit jeho instanci systému a zahrnuta filtr záměrné indikující, že obdrží GCM zprávy: 

```csharp
[Service (Exported = false), IntentFilter (new [] { "com.google.android.c2dm.intent.RECEIVE" })]
```

Když `GcmListenerService` přijme zprávu ze služby GCM, `OnMessageReceived` metoda je volána. Tato metoda extrahuje obsah zprávy z předané `Bundle`, zaznamená obsah zprávy (proto jsme ji zobrazit v okně výstupu) a volání `SendNotification` spuštění místního oznámení s obsahem přijaté zprávy: 

```csharp
var message = data.GetString ("message");
Log.Debug ("MyGcmListenerService", "From:    " + from);
Log.Debug ("MyGcmListenerService", "Message: " + message);
SendNotification (message);
```

`SendNotification` Používá metoda `Notification.Builder` vytvořit oznámení a poté používá `NotificationManager` spustíte oznámení. Prakticky to převede vzdáleného oznámení do místního oznámení předkládané uživateli.
Další informace o používání `Notification.Builder` a `NotificationManager`, najdete v části [místního oznámení](~/android/app-fundamentals/notifications/local-notifications.md).


#### <a name="declare-the-receiver-in-the-manifest"></a>Deklarovat příjemce v manifestu

Před obdržíme zpráv ze služby GCM jsme musí deklarovat naslouchací proces GCM v Android manifest. Umožňuje upravit **AndroidManifest.xml** a nahraďte `<application>` oddíl s následující kód XML: 

```xml
<application android:label="RemoteNotifications" android:icon="@drawable/Icon">
    <receiver android:name="com.google.android.gms.gcm.GcmReceiver" 
              android:exported="true" 
              android:permission="com.google.android.c2dm.permission.SEND">
        <intent-filter>
            <action android:name="com.google.android.c2dm.intent.RECEIVE" />
            <action android:name="com.google.android.c2dm.intent.REGISTRATION" />
            <category android:name="YOUR_PACKAGE_NAME" />
        </intent-filter>
    </receiver>
</application>
```

Ve výše uvedené XML, změňte *YOUR_PACKAGE_NAME* na název balíčku pro projekt klientské aplikace. V našem příkladu návod název balíčku je `com.xamarin.gcmexample`. 

Podívejme se na jaké každé nastavení v této kódu XML:

|Nastavení|Popis|
|---|---|
|`com.google.android.gms.gcm.GcmReceiver`|Deklaruje, že naše aplikace implementuje GCM příjemce, který zachytí a zpracuje příchozí nabízené oznámení zprávy.|
|`com.google.android.c2dm.permission.SEND`|Deklaruje, že pouze servery, GCM mohou zasílat zprávy přímo do aplikace.|
|`com.google.android.c2dm.intent.RECEIVE`|Záměrné filtr inzerování, že naše aplikace zpracovává zprávy všesměrového vysílání ze služby GCM.|
|`com.google.android.c2dm.intent.REGISTRATION`|Záměrné filtru inzerování, že naše aplikace zpracovává nové registrace tříd Intent (to znamená, že jsme byla implementována naslouchací proces služby ID Instance).|

Alternativně můžete uspořádání `GcmListenerService` s tyto atributy, nikoli jejich zadáním v XML; zde jsme zadejte je v **AndroidManifest.xml** tak, aby se snadněji postupujte podle ukázky kódu. 


### <a name="create-a-message-sender-to-test-the-app"></a>Vytvoření odesílatele zprávy k testování aplikace

Umožňuje do řešení přidat C# plochy projektu konzolové aplikace a pojmenujte ji **MessageSender**. Tato Konzolová aplikace použijeme k simulaci aplikační server &ndash; odešle zprávy s oznámením pro **ClientApp** pomocí služby GCM. 


#### <a name="add-the-jsonnet-package"></a>Přidejte balíček Json.NET

V této aplikaci konzoly vytváříme datové části JSON, který obsahuje zprávu oznámení, které nám chcete poslat do klientské aplikace. Použijeme **Json.NET** balíčku v **MessageSender** usnadnění vytvoření vyžadovaným GCM objekt JSON. V sadě Visual Studio, klikněte pravým tlačítkem na **odkazy > spravovat balíčky NuGet...** ; v sadě Visual Studio pro Mac, klikněte pravým tlačítkem na **balíčků > přidat balíčky...** . 

Umožňuje hledat **Json.NET** balíček a nainstalujte ho do projektu: 

[![Instalace balíčku Json.NET](remote-notifications-with-gcm-images/4-add-json.net-sml.png)](remote-notifications-with-gcm-images/4-add-json.net.png#lightbox)


#### <a name="add-a-reference-to-systemnethttp"></a>Přidat odkaz na System.Net.Http

Můžeme také budete muset přidat odkaz na `System.Net.Http` tak, aby jsme můžete vytvořit instanci `HttpClient` pro odesílání naše testovací zpráva do služby GCM. V **MessageSender** projektu, klikněte pravým tlačítkem na **odkazy > Přidat odkaz na** a posuňte se dolů, dokud neuvidíte **System.Net.Http**. Uveďte zatržení vedle **System.Net.Http** a klikněte na tlačítko **OK**. 


#### <a name="implement-code-that-sends-a-test-message"></a>Implementace kód, který odešle zkušební zprávu

V **MessageSender**, upravit **Program.cs** a nahraďte jeho obsah následujícím kódem:

```csharp
using System;
using System.Net.Http;
using System.Net.Http.Headers;
using System.Text;
using System.Threading.Tasks;
using Newtonsoft.Json.Linq;

namespace MessageSender
{
    class MessageSender
    {
        public const string API_KEY = "YOUR_API_KEY";
        public const string MESSAGE = "Hello, Xamarin!";

        static void Main (string[] args)
        {
            var jGcmData = new JObject();
            var jData = new JObject();

            jData.Add ("message", MESSAGE);
            jGcmData.Add ("to", "/topics/global");
            jGcmData.Add ("data", jData);

            var url = new Uri ("https://gcm-http.googleapis.com/gcm/send");
            try
            {
                using (var client = new HttpClient())
                {
                    client.DefaultRequestHeaders.Accept.Add(
                        new MediaTypeWithQualityHeaderValue("application/json"));

                    client.DefaultRequestHeaders.TryAddWithoutValidation (
                        "Authorization", "key=" + API_KEY);

                    Task.WaitAll(client.PostAsync (url,
                        new StringContent(jGcmData.ToString(), Encoding.Default, "application/json"))
                            .ContinueWith(response =>
                            {
                                Console.WriteLine(response);
                                Console.WriteLine("Message sent: check the client device notification tray.");
                            }));
                }
            }
            catch (Exception e)
            {
                Console.WriteLine("Unable to send GCM message:");
                Console.Error.WriteLine(e.StackTrace);
            }
        }
    }
}
```

Ve výše uvedeném kódu změňte *YOUR_API_KEY* pro klíč rozhraní API pro projekt klientské aplikace. 

Tento testovací aplikace server odešle zprávu následující formátu JSON do služby GCM:

```csharp
{
  "to": "/topics/global",
  "data": {
    "message": "Hello, Xamarin!"
  }
}
```

GCM, pak předá tuto zprávu vaší klientské aplikace. Vytvořme **MessageSender** a otevřete okno konzoly, kde jsme můžete spustit z příkazového řádku.



### <a name="try-it"></a>Můžete je vyzkoušejte.

Nyní je připraven k testování vaší klientské aplikace. Pokud používáte emulátor nebo pokud vaše zařízení komunikuje s GCM přes Wi-Fi, musíte otevřít následující porty TCP na bráně firewall pro zprávy GCM získat prostřednictvím: 5228, 5229 a 5230.

Spusťte aplikaci klienta a podívejte se ve výstupním okně. Po `RegistrationIntentService` úspěšně obdržel registrace token ze služby GCM, ve výstupním okně by měla zobrazovat token protokolu výstup podobný tomuto do:

```shell
I/RegistrationIntentService(16103): GCM Registration Token: eX9ggabZV1Q:APA91bHjBnQXMUeBOT6JDiLpRt8m2YWtY ...
```

V tomto okamžiku je připraven přijmout zprávu vzdáleného oznámení klientské aplikace. Z příkazového řádku, spusťte **MessageSender.exe** programu, který chcete odeslat zprávu oznámení "Hello, Xamarin" do klientské aplikace.
Pokud ještě nejsou vytvořené **MessageSender** projektu, udělejte to teď.

Ke spuštění **MessageSender.exe** v sadě Visual Studio, otevřete příkazový řádek, změňte **MessageSender/bin/Debug** adresáře a spusťte příkaz přímo:

```cmd
MessageSender.exe
```

Ke spuštění **MessageSender.exe** v sadě Visual Studio pro Mac, otevřete relaci terminálu, změňte na **MessageSender/bin/Debug** do adresáře a použijte mono ke spuštění **MessageSender.exe** 

```bash
mono MessageSender.exe
```

To může trvat až na minutu pro zprávu, která se šířit přes GCM a zpět do vaší klientské aplikace. Pokud zpráva se úspěšně přijme, bychom měli vidět výstup podobný tomuto v okně výstupu: 

```shell
D/MyGcmListenerService(16103): From:    /topics/global
D/MyGcmListenerService(16103): Message: Hello, Xamarin!
```

Kromě toho by měl zjistíte, že má ikona oznámení nového zobrazovaly na hlavním panelu oznámení: 

[![U zařízení se zobrazí ikona Notiication](remote-notifications-with-gcm-images/5-icon-appears-sml.png)](remote-notifications-with-gcm-images/5-icon-appears.png#lightbox)

Při otevření panelu oznámení zobrazit oznámení, měli byste vidět naše vzdáleného oznámení:

[![Zobrazí se zpráva oznámení](remote-notifications-with-gcm-images/6-notification-in-tray-sml.png)](remote-notifications-with-gcm-images/6-notification-in-tray.png#lightbox)

Blahopřejeme, aplikace přijal jeho prvního vzdáleného oznámení.

Všimněte si, GCM zprávy budou přijímány už v Pokud je aplikace force zastavena. Obnovit oznámení po zarážku force, aplikace musí být restartovat ručně. Další informace o této zásady pro Android, najdete v části [spusťte ovládací prvky v aplikacích pro zastavený](https://developer.android.com/about/versions/android-3.1.html#launchcontrols) a tato [stack overflow post](http://stackoverflow.com/questions/5051687/broadcastreceiver-not-receiving-boot-completed/19856267#19856267). 

 
## <a name="summary"></a>Souhrn

Tento názorný postup podrobné kroky pro implementaci vzdáleného oznámení v aplikaci Xamarin.Android. Je popsaný postup instalace dalších balíčků, které jsou potřebné pro komunikaci služby GCM a ho vysvětlení najdete postup konfigurace aplikace oprávnění pro přístup k serverům služby GCM. Poskytovala ukázkový kód, který znázorňuje postup kontroly na přítomnost služby Google Play, jak implementovat registrace záměrné služba a služba naslouchací proces ID instance, která vyjedná s GCM pro registrační token a jak implementovat naslouchací proces GCM Služba, která obdrží a zpracuje zprávy vzdáleného oznámení. Nakonec implementovali jsme testů z příkazového řádku programu odeslat zkušební oznámení do vaší aplikace klienta pomocí služby GCM. 


## <a name="related-links"></a>Související odkazy

- [RemoteNotifications GCM (ukázka)](https://developer.xamarin.com/samples/monodroid/RemoteNotifications)
- [Google Cloud Messaging](~/android/data-cloud/google-messaging/google-cloud-messaging.md)
