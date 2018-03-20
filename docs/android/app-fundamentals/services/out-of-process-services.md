---
title: "Spuštění Android služeb ve vzdálených procesů"
description: "Obecně platí všechny součásti v aplikaci pro Android se spustí v rámci jednoho procesu. Android služby se upozorňují na důležité výjimkou, že mohou být nakonfigurovány na spouštění v vlastní procesy a sdílet s jinými aplikacemi, včetně těch, které z jiných vývojáře v systému Android. Tato příručka popisuje postup vytvoření a používání Android vzdálené služby, pomocí Xamarin."
ms.topic: article
ms.prod: xamarin
ms.assetid: 27A2E972-A690-480B-B31D-5EF1F74F673C
ms.technology: xamarin-android
author: topgenorth
ms.author: toopge
ms.date: 02/16/2018
ms.openlocfilehash: ebbb4b527b27b87bb6357723978e730304658720
ms.sourcegitcommit: cc38757f56aab53bce200e40f873eb8d0e5393c3
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 03/20/2018
---
# <a name="running-android-services-in-remote-processes"></a>Spuštění Android služeb ve vzdálených procesů

_Obecně platí všechny součásti v aplikaci pro Android se spustí v rámci jednoho procesu. Android služby se upozorňují na důležité výjimkou, že mohou být nakonfigurovány na spouštění v vlastní procesy a sdílet s jinými aplikacemi, včetně těch, které z jiných vývojáře v systému Android. Tato příručka popisuje postup vytvoření a používání Android vzdálené služby, pomocí Xamarin._

## <a name="out-of-process-services-overview"></a>Mimo přehled procesu služby

Při spuštění aplikace, vytvoří Android procesu, ve kterém ke spuštění aplikace. Obvykle se všechny součásti aplikace poběží v tento jeden proces. Android služby se upozorňují na důležité výjimkou, že mohou být nakonfigurovány na spouštění v vlastní procesy a sdílet s jinými aplikacemi, včetně těch, které z jiných vývojáře v systému Android. Tyto typy služeb se označují jako _vzdálené_ nebo _mimo proces služby_. Kód pro tyto služby bude obsahovat stejné APK jako hlavní aplikace; ale pokud je služba spuštěná Android se vytvoří nový proces právě dané služby. Naproti tomu služba, která běží v rámci jednoho procesu jako ostatní aplikace se někdy označuje jako _místní služba_.

Obecně platí není nutné pro aplikaci k implementaci vzdálené služby. Místní služba je dostatečná (a žádoucí) pro požadavky na aplikace v mnoha případech. Out-of-process má vlastní paměti prostor, který se musí spravovat přes Android. I když to zavést další režii celkové aplikace, je několik scénářů, kde může být výhodné spuštění služby ve svém vlastním procesu:

1. **Sdílení funkce** &ndash; někteří vývojáři aplikace může mít víc aplikací a funkce, která je sdílena mezi těmito aplikacemi. Balení, které tuto funkci v službě Android, které běží ve svém vlastním procesu může zjednodušit Údržba aplikace. Je také možné balíček služby v jeho vlastní samostatné APK a nasaďte ho odděleně od zbytku aplikace.
2. **Vylepšení uživatelské prostředí** &ndash; existují dva způsoby možné, že spuštění aplikace můžete vylepšit služby mimo proces. První způsob se zabývá Správa paměti. Při uvolnění paměti (GC) dojde k cyklus, Android se pozastaví všechny aktivity v procesu, dokud se nedokončí globální Katalog. Uživatel může vnímat tento pozastavení jako "přerušovaný" nebo "jank". Když je služba spuštěná v je vlastní proces, je služby procesu, který je pozastaven, není proces aplikace. Tato pozastavení bude mnohem méně znatelný pro uživatele, protože není pozastavená proces aplikace (a proto uživatelské rozhraní).

    Za druhé Pokud požadavky na paměť procesu příliš velká, může Android ukončení tohoto procesu tím se uvolní prostředky pro zařízení. Pokud služba nemá nároků velké paměti a je spuštěna v rámci jednoho procesu jako uživatelské rozhraní, pak při Android vynuceně získá tyto prostředky uživatelského rozhraní se vypne, vynucení uživatele a spusťte aplikaci. Když služby, spuštěné ve svém vlastním procesu vypnutí Android je proces uživatelského rozhraní zůstane neovlivní. Uživatelského rozhraní můžete vytvořit vazbu (a restartovat) službu pro uživatele a obnovit normální fungování transparentní.

3. **Zvýšení výkonu aplikací** &ndash; proces uživatelského rozhraní může být ukončeno, nebo vypnout bez ohledu na proces služby. Přesunutím zdlouhavé spuštění úlohy do služby mimo proces je možné, čas spuštění uživatelského rozhraní možná vylepšené (za předpokladu, že proces služby je udržováno zachování připojení mezi doby spuštění uživatelského rozhraní).

V mnoha směrech vazby do služby spuštěné v jiném procesu je stejný jako [připojení k místní službě](~/android/app-fundamentals/services/creating-a-service/bound-services.md). Klient bude vyvolání `BindService` bind (a spustit v případě potřeby) službu. `Android.OS.IServiceConnection` Objekt se vytvoří ke správě připojení mezi klientem a služby. Pokud klient úspěšně váže ke službě, pak Android vrátí objekt prostřednictvím `IServiceConnection` který slouží k vyvolání metody pro službu. Klient pak komunikuje s pomocí tohoto objektu. Chcete-li zkontrolovat, tady jsou kroky k vytvoření vazby služby:

* **Vytvoření záměrem** &ndash; explicitní záměrem je nutné použít pro vazbu ke službě.
* **Implementace a vytvořit instance `IServiceConnection` objekt** &ndash; `IServiceConnection` objektu slouží jako propojení mezi klientem a služby.  Je zodpovědná za monitorování připojení mezi klientem a serverem.
* **Vyvolání `BindService` metoda** &ndash; volání `BindService` bude dispatch záměr a připojení k službě vytvořené v předchozí kroky pro Android, která se postará o spouštění služby a navázání komunikace mezi klientem a službou.

Potřeba mezi hranice procesu zavést další složitosti: komunikace je jednosměrná (klienta k serveru) a klient nemůže přímo vyvolat metody pro třídu služby. Odvolat, že když služba běží stejný postup jako klient, Android poskytuje `IBinder` objekt, který může povolit pro obousměrnou komunikaci. Toto není případu se služby spuštěné ve svém vlastním procesu. Klient komunikuje s vzdálené služby s pomocí `Android.OS.Messenger` třídy.

Když klient požádá o má být svázána s vzdálené služby, bude vyvolání Android `Service.OnBind` životního cyklu metodu, která vrátí interní `IBinder` objekt zapouzdřenou `Messenger`. `Messenger` Je dynamické obálku nad speciální `IBinder` implementace, které poskytuje SDK pro Android. `Messenger` Má na starosti komunikaci mezi dvěma různými procesy. Vývojář je unconcerned s detaily o serializaci zprávu, zařazování zprávy přes hranice procesu a pak ho deserializaci na straně klienta. Tento pracovní jsou zpracována `Messenger` objektu. Tento diagram zobrazuje Android klientské komponenty, které se podílejí, když klient zahájí vazbu na službu mimo proces:

![Diagram, který zobrazuje kroky a součásti pro klienta vazbu ke službě](out-of-process-services-images/ipc-01.png "Diagram, který zobrazuje kroky a součásti pro vytvoření vazby na služby klienta.")

`Service` Třídy v vzdálený proces se absolvovat stejný životní cyklus zpětná volání, které bude projít vázané služby v místní proces a řadu rozhraní API související se situací jsou stejné. `Service.OnCreate` slouží k inicializaci `Handler` a vložit, který do `Messenger` objektu. Podobně `OnBind` je přepsaného, ale místo vrácení `IBinder` objekt, vrátí službu `Messenger`.  Tento diagram znázorňuje, co se stane ve službě, když se navazuje klienta:

![Diagram, který zobrazuje kroky a součásti služby prochází při byla vázána vzdáleného klienta](out-of-process-services-images/ipc-02.png "prochází Diagram, který zobrazuje kroky a součásti služby, pokud byla vázána vzdáleného klienta.")

Když `Message` přijetí službou, je zpracován v instanci `Android.OS.Handler`. Služba bude implementovat vlastní `Handler` podtřídami, který musí přepsat `HandleMessage` metoda. Tato metoda je volána pomocí `Messenger` a přijímá `Message` jako parametr. `Handler` Bude kontrolovat `Message` metadata a tyto informace používat k vyvolání metody pro službu.

Jednosměrná komunikace nastane, když klient vytvoří `Message` objektu a odešle ji do služby pomocí `Messenger.Send` metoda. `Messenger.Send` bude serializovat `Message` a straně, který serializovat dat vypnout, Android, která bude směrovat zprávy přes hranice procesu a ke službě.  `Messenger` Který je hostitelem služby se vytvoří `Message` objekt z příchozí data. To `Message` je umístěn do fronty, kde jsou zprávy odeslané jeden po druhém k `Handler`. `Handler` Bude kontrolovat meta-data obsažená v `Message` a vyvolání metody odpovídající na `Service`. Následující diagram znázorňuje tyto různé koncepty v akci:

![Diagram znázorňující, jak jsou zprávy předávají mezi procesy](out-of-process-services-images/ipc-03.png "Diagram znázorňující, jak jsou zprávy předávají mezi procesy.")

Tato příručka popisuje podrobnosti implementace služby mimo proces. Bude popisují, jak implementovat služba, která je určená ke spuštění ve svém vlastním procesu a jak může klient komunikovat s použitím této služby `Messenger` framework. Je také krátce zabývat obousměrné komunikace: klient při odesílání zprávy na službu a službu odesílání zprávy zpět do klienta. Protože služby lze sdílet mezi různými aplikacemi, tento průvodce také popisuje jeden postup pro omezení přístupu klientů ke službě pomocí Android oprávnění.

> [!IMPORTANT]
> [Bugzilla 51940 - služeb izolovaných procesech a vlastní třídy aplikace se nepodaří vyřešit přetížení správně](https://bugzilla.xamarin.com/show_bug.cgi?id=51940) sestavy, které služba Xamarin.Android nebude správně spustit až když `IsolatedProcess` je nastaven na `true`. Tato příručka slouží pro odkaz. Aplikace pro Xamarin.Android by měla být stále moci komunikovat se službou mimo proces, který je napsán v jazyce Java.

## <a name="requirements"></a>Požadavky

Tato příručka předpokládá znalost vytváření služeb.

Přestože je možné používat implicitní záměry s aplikacemi, že cíl starší rozhraní Android API, tato příručka se soustředí výhradně na použití explicitní záměry. Aplikace cílené na Android 5.0 (API úrovně 21) nebo vyšší musí použít explicitní záměrem má být svázána s služby; Toto je postup, který je popsána v této příručce...

## <a name="create-a-service-that-runs-in-a-separate-process"></a>Vytvoření služby, který běží v samostatném procesu

Jak je popsáno výše, skutečnost, že služba běží ve svém vlastním procesu znamená, že jsou některé jiné rozhraní API související se situací. Jako rychlý přehled Zde jsou kroky pro vytvoření vazby s a využívat vzdálené služby:  

* **Vytvořte `Service` podtřídami** &ndash; podtřídami `Service` zadejte a implementovat metodu životního cyklu vázané služby. Také je nutné nastavit metadata, která bude informovat Android, která služba je ke spuštění ve svém vlastním procesu.
* **Implementace `Handler`**  &ndash; `Handler` zodpovídá za analýza žádosti klienta, extrahování žádné parametry, které byly předány z klienta a volání metody odpovídající na službu.
* **Vytváření instancí `Messenger`**  &ndash; jak bylo popsáno výše, každý `Service` musí zachovat instanci `Messenger` třídu, která bude směrovat požadavky na `Handler` který byl vytvořen v předchozím kroku.

Služba, která je určená ke spuštění ve svém vlastním procesu je, od základu, stále vázané služby. Třída služby bude rozšíření základní `Service` třídy a je upraven pomocí `ServiceAttribute` obsahující meta-data, která musí sady v Android manifest Android. Chcete-li začít s následující vlastnosti `ServiceAttribute` , které jsou důležité služby mimo proces:

1. `Exported` &ndash; Tato vlastnost musí být nastavená na `true` umožňující interakci se službou dalších aplikací. Výchozí hodnota této vlastnosti je `false`.
2. `Process` &ndash; Tato vlastnost musí být nastavena. Slouží k zadání názvu procesu, který služba bude spuštěna v.
3. `IsolatedProcess` &ndash; Tato vlastnost vám umožní vyšší bezpečnost, že ke spuštění služby v izolované izolovaného prostoru s minimální oprávnění k iteract se zbytkem systému Android. V tématu [Bugzilla 51940 - Services s izolovaných procesech a vlastní třídy aplikace nepodaří vyřešit přetížení správně](https://bugzilla.xamarin.com/show_bug.cgi?id=51940).
4. `Permission` &ndash; Je možné k řízení přístupu klientů ke službě zadáním oprávnění, které klienti musí požádat (a udělit).

Ke spuštění služby vlastní proces, `Process` vlastnost `ServiceAttribute` musí být nastavena na název služby. Pro interakci s mimo aplikace `Exported` vlastnost by měla být nastavená na `true`. Pokud `Exported` je `false`, pak budou moci komunikovat se službou pouze klienty ve stejné APK (tj. stejné aplikace) a spuštěna v rámci jednoho procesu.

Jaký druh služba bude spuštěna v procesu závisí na hodnotu `Process` vlastnost. Android identifikuje tři různé typy procesů:

-   **Proces privátní** &ndash; privátní proces je ten, který je k dispozici pro aplikaci, která je spuštěna. K identifikaci procesu jako privátní, jeho název musí začínat znakem **:** (středníkem). Služby, které popsané v předchozím fragmentu kódu a snímek je privátní proces. Následující fragment kódu je příkladem `ServiceAttribute`:

    ```csharp
    [Service(Name = "com.xamarin.TimestampService",
             Process=":timestampservice_process",
             Exported=true)]
    ```

-   **Globální proces** &ndash; služba, která běží v globální proces je dostupný pro všechny aplikace spuštěné na zařízení. Globální procesu musí být plně kvalifikovaný název, který začíná malé písmeno.
    (Pokud opatření k zabezpečení služby, ostatních aplikací může vytvořit vazbu a pracovat s ním. Zabezpečení služby před neoprávněným použitím probereme v této příručce.)

    ```csharp
    [Service(Name = "com.xamarin.TimestampService",
             Process="com.xamarin.xample.messengerservice.timestampservice_process",
             Exported=true)]
    ```

-   **Samostatný proces** &ndash; izolovaných procesech je proces, který běží ve vlastní izolovaného prostoru, izolovaně od zbytku systému a s žádná zvláštní oprávnění své vlastní. Chcete-li spustit službu v izolovaném procesu, `IsolatedProcess` vlastnost `ServiceAttribute` je nastaven na `true` jak ukazuje tento fragment kódu:
    
    ```csharp
    [Service(Name = "com.xamarin.TimestampService",
             IsolatedProcess= true,
             Process="com.xamarin.xample.messengerservice.timestampservice_process",
             Exported=true)]
    ```

> [!IMPORTANT]
> V tématu [Bugzilla 51940 - Services s izolovaných procesech a vlastní třídy aplikace nepodaří správně vyřešit přetížení](https://bugzilla.xamarin.com/show_bug.cgi?id=51940)

Izolované služby je jednoduchý způsob, jak zabezpečit aplikace a zařízení s nedůvěryhodnými. Aplikace může například stažení a spuštění skriptu z webu. V tomto případě to provádění v izolovaných procesech poskytuje další vrstvu zabezpečení před nedůvěryhodnými ohrožení zařízení s Androidem.

> [!IMPORTANT]
> Po vyexportování služby by neměl změnit název služby. Změna názvu služby je možné ukončit jiné aplikace, které používají službu.

Pokud chcete vidět, `Process` vlastnost má, následující snímek obrazovky ukazuje služby spuštěné v jeho vlastní privátní procesu:

![Snímek obrazovky zobrazující služby spuštěné v privátní procesu](out-of-process-services-images/ipc-04.png "snímek obrazovky zobrazující služby spuštěné v privátní procesu.")

Tato další snímek obrazovky ukazuje `Process="com.xamarin.xample.messengerservice.timestampservice_process"` a služba spuštěna v globální procesu:

![Snímek obrazovky služby spuštěné v globální procesu](out-of-process-services-images/ipc-05.png "snímek služby spuštěné v globální procesu.")

Jednou `ServiceAttribute` byla nastavena, je nutné službu implementovat `Handler`.

### <a name="implementing-a-handler"></a>Implementace obslužné rutiny

Ke zpracování požadavků klientů, musíte službu implementovat `Handler` a přepsat `HandleMessage` methodThis je metoda trvá `Message` instance, které, který zapouzdřuje volání metody, které z klienta a znamená, že volání do některé akce nebo úloha, která provede službu. `Message` Objekt zpřístupňuje vlastnost s názvem `What` tedy celočíselná hodnota, význam jsou sdílena mezi klientem a službu a má vztah k některé úlohu, která služba je k provedení pro klienta.

Následující fragment kódu z ukázkové aplikace zobrazuje příkladem `HandleMessage`. V tomto příkladu jsou dvě akce, které může klient požadovat služby:

* Je první akcí _Hello, World_ zpráva, klient odeslal zprávu jednoduché ke službě.
* Druhou akci se vyvolat metodu na službu a načíst řetězec, v tomto případě je řetězec zprávu, která vrací jaké čas služba spuštěna a jak dlouho byla spuštěná:

```csharp
public class TimestampRequestHandler : Android.OS.Handler
{
    // other code omitted for clarity

    public override void HandleMessage(Message msg)
    {
        int messageType = msg.What;
        Log.Debug(TAG, $"Message type: {messageType}.");

        switch (messageType)
        {
            case Constants.SAY_HELLO_TO_TIMESTAMP_SERVICE:
                // The client as sent a simple Hello, say in the Android Log.
                break;

            case Constants.GET_UTC_TIMESTAMP:
                // Call methods on the service to retrive a timestamp message.
                break;
            default:
                Log.Warn(TAG, $"Unknown messageType, ignoring the value {messageType}.");
                base.HandleMessage(msg);
                break;
        }
    }
}
```

Je také možné parametry balíček služby `Message`. To bude probírat později v této příručce. Vytváří další téma vzít v úvahu `Messenger` objekt pro zpracování příchozí `Message`s.

### <a name="instantiating-the-messenger"></a>Vytváření instancí Messenger

Dřív popsané deserializaci `Message` objekt a vyvolání `Handler.HandleMessage` je odpovědného z `Messenger` objektu. `Messenger` Třída rovněž poskytuje `IBinder` objektu, že bude klient používat k odesílání zpráv do služby.  

Když se služba spustí, vytvoří instanci `Messenger` a vložit `Handler`. Je vhodná k provedení této inicializace na `OnCreate` metoda služby. Tento fragment kódu je jedním z příkladů služby, která inicializuje vlastní `Handler` a `Messenger`:

```csharp
private Messenger messenger; // Instance variable for the Messenger

public override void OnCreate()
{
    base.OnCreate();
    messenger = new Messenger(new TimestampRequestHandler(this));
    Log.Info(TAG, $"TimestampService is running in process id {Android.OS.Process.MyPid()}.");
}
```

V tomto okamžiku je posledním krokem pro `Service` přepsat `OnBind`.

### <a name="implementing-serviceonbind"></a>Implementace Service.OnBind

Všechny vázané služby, zda by spuštěny ve vlastním procesu, nebo Ne, musí implementovat `OnBind` metoda. Návratová hodnota tato metoda je některé objekt, který může klient použít pro interakci se službou. Přesně co tento objekt je závisí, zda je služba místní služba nebo vzdálené služby. Při místní služba vrátí vlastní `IBinder` implementace vrátí vzdálené služby `IBinder` , je zapouzdřený ale `Messenger` který byl vytvořen v předchozí části:

```csharp
public override IBinder OnBind(Intent intent)
{
    Log.Debug(TAG, "OnBind");
    return messenger.Binder;
}
```

Jakmile jsou tyto tři kroky provést, vzdálené služby můžete být považováno za dokončené.

## <a name="consuming-the-service"></a>Využívání služby

Všichni klienti musí implementovat nějaký kód, abyste mohli vytvořit vazbu a využívat vzdálené služby. Koncepčně z hlediska klienta, jsou jen několik rozdíly mezi vazbu na místní služba nebo vzdálené služby. Klient vyvolá `BindService` metodu předáním explicitní záměrem k identifikaci služby a `IServiceConnection` , pomáhá spravovat připojení mezi klientem a služby.

Tento fragment kódu je příklad toho, jak vytvořit **explicitní záměr** vazba vzdálené služby. Záměr musíte určit balíček, který obsahuje službu a název služby. Je možné nastavit tyto informace používat `Android.Content.ComponentName` objektu a k poskytování který záměr. Tento fragment kódu je jedním z příkladů:  

```csharp
// This is the package name of the APK, set in the Android manifest
const string REMOTE_SERVICE_COMPONENT_NAME = "com.xamarin.TimestampService";
// This is the name of the service, according the value of ServiceAttribute.Name
const string REMOTE_SERVICE_PACKAGE_NAME   = "com.xamarin.xample.messengerservice";

// Provide the package name and the name of the service with a ComponentName object.
ComponentName cn = new ComponentName(REMOTE_SERVICE_PACKAGE_NAME, REMOTE_SERVICE_COMPONENT_NAME);
Intent serviceToStart = new Intent();
serviceToStart.SetComponent(cn);
```

Když je vázána služby, `IServiceConnection.OnServiceConnected` metoda je volána a poskytuje `IBinder` ke klientovi. Nicméně, klient nebude používat přímo `IBinder`. Místo toho vytvoří instanci `Messenger` objekt od `IBinder`. Toto je `Messenger` , bude klient používat k interakci se vzdálené služby.

Tady je příklad velmi základní `IServiceConnection` implementace, která ukazuje, jak by klient zpracovávat k připojení a odpojení od služby. Všimněte si, že `OnServiceConnected` metoda obdrží a `IBinder`, a vytvoří klienta `Messenger` od `IBinder`:

```csharp
public class TimestampServiceConnection : Java.Lang.Object, IServiceConnection
{
    static readonly string TAG = typeof(TimestampServiceConnection).FullName;

    MainActivity mainActivity;
    Messenger messenger;

    public TimestampServiceConnection(MainActivity activity)
    {
        IsConnected = false;
        mainActivity = activity;
    }

    public bool IsConnected { get; private set; }
    public Messenger Messenger { get; private set; }

    public void OnServiceConnected(ComponentName name, IBinder service)
    {
        Log.Debug(TAG, $"OnServiceConnected {name.ClassName}");

        IsConnected = service != null
        Messenger = new Messenger(service);

        if (IsConnected)
        {
            // things to do when the connection is successful. perhaps notify the client? enable UI features?
        }
        else
        {
            // things to do when the connection isn't successful.
        }
    }

    public void OnServiceDisconnected(ComponentName name)
    {
        Log.Debug(TAG, $"OnServiceDisconnected {name.ClassName}");
        IsConnected = false;
        Messenger = null;

        // Things to do when the service disconnects. perhaps notify the client? disable UI features?

    }
}
```

Po vytvoření připojení k službě a záměr je možné klienta k volání `BindService` a zahájit proces vytváření vazby:

```csharp
IServiceConnection serviceConnection = new TimestampServiceConnection(this);
BindActivity(serviceToStart, serviceConnection, Bind.AutoCreate);
```

Jakmile má klient úspěšně svázaná se službou a `Messenger` je k dispozici, je možné klienta k odesílání `Messages` ke službě.

## <a name="sending-messages-to-the-service"></a>Odesílání zpráv do služby

Jakmile klient připojený a má `Messenger` objekt, je možné komunikovat se službou odesláním `Message` objekty prostřednictvím `Messenger`. Tato komunikace je jednosměrná, klient odešle zprávu, ale není žádná návratový ze služby do klienta. V tomto ohledu `Message` mechanismus ještě efektivněji a zapomněli.

Upřednostňovaný způsob, jak vytvořit `Message` objekt, je použít [ `Message.Obtain` ](https://developer.xamarin.com/api/type/Android.OS.Message/#Public%20Methods) metoda factory. Tato metoda načte `Message` objekt z globálního fondu, který je spravován Android. `Message.Obtain` také některé přetížil metody, které umožňují `Message` objekt, který má být inicializován s hodnotami a parametry, které služba vyžaduje.  Jednou `Message` se vytvořit instanci, je odeslána ke službě voláním `Messenger.Send`. Tento fragment kódu je jeden příklad vytvoření a odeslání `Message` pro proces služby:

```csharp
Message msg = Message.Obtain(null, Constants.SAY_HELLO_TO_TIMESTAMP_SERVICE);
try
{
    serviceConnection.Messenger.Send(msg);
}
catch (RemoteException ex)
{
    Log.Error(TAG, ex, "There was a error trying to send the message.");
}
```

Existuje několik různých způsobů `Message.Obtain` metoda. Předchozí příklad používá [ `Message.Obtain(Handler h, Int32 what)` ](https://developer.xamarin.com/api/member/Android.OS.Message.Obtain/p/Android.OS.Handler/System.Int32/). Protože se jedná o požadavek asynchronní službou mimo proces. budou existovat žádná odpověď ze služby, proto `Handler` je nastaven na `null`. Druhý parametr `Int32 what`, se budou ukládat do `.What` vlastnost `Message` objektu. `.What` Vlastnost se používá v kódu v procesu služby k vyvolání metody pro službu.

`Message` Třída taky zpřístupňuje dva další vlastnosti, které může být použita recipent: `Arg1` a `Arg2`. Tyto dvě vlastnosti jsou celočíselné hodnoty, které můžou mít některé speciální dohodnutých hodnoty, které mají význam mezi klientem a služby. Například `Arg1` může obsahovat ID zákazníka a `Arg2` může obsahovat číslo objednávky tohoto zákazníka. [ `Method.Obtain(Handler h, Int32 what, Int32 arg1, Int32 arg2)` ](https://developer.xamarin.com/api/member/Android.OS.Message.Obtain/p/Android.OS.Handler/System.Int32/System.Int32/System.Int32/) Můžete použít k nastavení dvě vlastnosti při `Message` je vytvořena. Další způsob naplnění tyto dvě hodnoty je nastavit `.Arg` a `.Arg2` vlastnosti přímo na `Message` objektu po jeho vytvoření.

### <a name="passing-additional-values-to-the-service"></a>Další hodnoty předávání do služby

Je možné předat složitější data do služby pomocí `Bundle`. V takovém případě doplňující hodnoty mohou být umístěny v `Bundle` a odeslané spolu s `Message` nastavením [ `.Data` vlastnost](https://developer.xamarin.com/api/property/Android.OS.Message.Data/) vlastnost před odesláním.

```csharp
Bundle serviceParameters = new Bundle();
serviceParameters.

var msg = Message.Obtain(null, Constants.SERVICE_TASK_TO_PERFORM);
msg.Data = serviceParameters;

messenger.Send(msg);
```


> [!NOTE]
> Obecně platí `Message` by neměl mít datovou část větší než 1 MB. Limit velikosti může lišit podle verze Android a na všechny vlastní změny jste zadali dodavatele pro jejich implementaci z Android otevřít zdroj projektu (AOSP) zahrnutý v systému zařízení.

## <a name="returning-values-from-the-service"></a>Vrácení hodnoty ze služby

Zasílání zpráv architekturu, která má k tomuto bodu popsány jednosměrné, klient odešle zprávu do služby. Pokud je nutné pro službu, kterou chcete ke klientovi vrátit hodnotu pak všechno, co se má k tomuto bodu popsány je vrátit zpět. Musíte vytvořit službu `Message`, zabalené všechny návratové hodnoty a odesílání `Message` prostřednictvím `Messenger` klientovi. Však služba nevytváří vlastní `Messenger`; místo toho se spoléhá na klientovi, vytváření instancí a balíčku `Messenger` jako součást počáteční požadavku. Služba bude `Send` zprávy pomocí zadaný tento klient `Messenger`.  

Posloupnost událostí pro obousměrnou komunikaci jsou tyto:

1. Klient vytvoří vazbu ke službě. Když služba a klient připojit, `IServiceConnection` který je spravován klient bude obsahovat odkaz na `Messenger` objekt, který se používá k přenosu `Message`s ke službě. Pokud chcete předejít nejasnostem, to bude označována jako _Kurýrní služba_.
2. Vytvoří klienta `Handler` (označují jako _obslužné rutiny klienta_) a použije ho k inicializaci vlastní `Messenger` ( _klienta Messenger_). Upozorňujeme, že služba Messenger a Messenger klienta jsou dva různé objekty, které zpracovává přenosy ve dvou různých směrů. Kurýrní služba zpracovává zprávy z klienta ke službě Messenger klienta bude zpracovávat zpráv ze služby do klienta.
3. Klient vytvoří `Message` objekt a nastaví `ReplyTo` vlastnost s Messenger klienta. Zpráva bude odeslaný do služby pomocí Kurýrní služba.
4. Služba obdrží zprávu od klienta a provede požadovanou práci.
5. Pokud nastane čas pro službu pro odeslání odpovědi klientovi, použije `Message.Obtain` pro vytvoření nového `Message` objektu.
6. Odeslat tuto zprávu do klienta, bude služba extrahovat Messenger klienta z `.ReplyTo` vlastnost klienta zprávy a použít jej k `.Send` `Message` zpět do klienta.
7. Odpověď odeslaná klientem, má svou vlastní `Handler` bude zpracovávat `Message` zkontrolováním `.What` vlastnost (a v případě potřeby extrahování žádné parametry obsažených `Message`).

Tento ukázkový kód ukazuje, jak klient vytvoří instanci `Message` a balíčku `Messenger` , služba by měla použít pro odpovědi:

```csharp
Handler clientHandler = new ActivityHandler();
Messenger clientMessenger = new Messenger(activityHandler);

Message msg = Message.Obtain(null, Constants.GET_UTC_TIMESTAMP);
msg.ReplyTo = clientMessenger;

try
{
    serviceConnection.Messenger.Send(msg);
}
catch (RemoteException ex)
{
    Log.Error(TAG, ex, "There was a problem sending the message.");
}
```

Služba musí udělat některé změny, které vlastní `Handler` k extrakci `Messenger` a použít jej k odeslání odpovědi klientovi. Tento fragment kódu je příklad toho, jak služby `Handler` by vytvořit `Message` a odeslat zpět klientovi:  

```csharp
// This is the message that the service will send to the client.
Message responseMessage = Message.Obtain(null, Constants.RESPONSE_TO_SERVICE);
Bundle dataToReturn = new Bundle();
dataToReturn.PutString(Constants.RESPONSE_MESSAGE_KEY, "This is the result from the service.");
responseMessage.Data = dataToReturn;

// The msg object here is the message that was received by the service. The service will not instantiate a client,
// It will use the client that is encapsulated by the message from the client.
Messenger clientMessenger = msg.ReplyTo;
if (clientMessenger!= null)
{
    try
    {
        clientMessenger.Send(responseMessage);
    }
    catch (Exception ex)
    {
        Log.Error(TAG, ex, "There was a problem sending the message.");
    }
}
```

Všimněte si, že se ve výše uvedené, ukázky kódu `Messenger` instance, který je vytvořen pomocí klienta je *není* stejný objekt, který je služba přijímá. Toto jsou dva různé `Messenger` objekty, které jsou spuštěné v dva samostatné procesy, které představují komunikační kanál.

## <a name="securing-the-service-with-android-permissions"></a>Zabezpečení služby s Androidem oprávnění

Služba, která běží v globální procesu je přístupný pro všechny aplikace spuštěné na daném zařízení Android. V některých situacích tento průhlednost a dostupnosti není žádoucí, a je potřeba zabezpečit DOS proti přístupu z neoprávněným klientů. Jeden způsob, jak omezit přístup k vzdálené služby je použití Android oprávnění.

Oprávnění lze identifikovat podle `Permission` vlastnost `ServiceAttribute` , upraví `Service` podtřídou třídy. To bude název oprávnění, které klient musí mít udělen při vytváření vazby ke službě. Pokud klient nemá příslušná oprávnění, pak Android vyvolá výjimku `Java.Lang.SecurityException` když se klient pokusí vytvořit vazbu ke službě.

Existují čtyři úrovně oprávnění, které poskytuje Android:

* **Normální** &ndash; Toto je výchozí úroveň oprávnění. Slouží k identifikaci nízkým rizikem oprávnění, která může automaticky Android na klienty, kteří ji. Uživatel nemá explicitně udělit oprávnění, ale oprávnění lze zobrazit v nastavení aplikace.
* **podpis** &ndash; Toto je speciální kategorie oprávnění, která bude udělen automaticky Android k aplikacím, které jsou podepsané stejným certifikátem. Toto oprávnění je navržená tak, aby ji snadno pro vývojáře aplikací sdílení součásti nebo dat mezi svoje aplikace bez bothering uživatele pro konstantní schválení.
* **signatureOrSystem** &ndash; to je velmi podobná **podpis** oprávnění popsané výše. Kromě toho automaticky uděleno k aplikacím, které jsou podepsány stejným certifikátem, tato se také udělit oprávnění aplikací, které jsou podepsané stejný certifikát, který se použil k podepsání aplikace nainstalované s bitovou kopii systému Android. Toto oprávnění je obvykle pouze umožňuje vývojáři Android ROM své aplikace pro práci s aplikací třetí strany. Běžně nepoužívá aplikace, které jsou určené obecné distribuce pro široké veřejnosti.
* **nebezpečná** &ndash; nebezpečná oprávnění jsou ty, které by mohla způsobovat problémy pro uživatele. Z tohoto důvodu **nebezpečná** oprávnění musí být explicitně schválení uživatelem.

Protože `signature` a `normal` automaticky oprávnění nainstalované během Android, je zásadní, protože nainstalované APK hostující službu **před** APK obsahující klienta. Pokud nejprve instalace klienta, nebude Android udělit oprávnění. V takovém případě bude nutné odinstalovat klienta APK, nainstalovat službu APK a poté ji znovu nainstalujte klienta APK.

Existují dvě běžné způsoby zabezpečení služby s Androidem oprávnění:

1.  **Implementace zabezpečení na úrovni podpisu** &ndash; zabezpečení na úrovni podpisu znamená, že oprávnění je automaticky uděleno pro tyto aplikace, které jsou podepsané stejným klíčem, který se použil k podepsání APK, která uchovává službu. Toto je jednoduchý způsob pro vývojáře k zabezpečení svých služby ještě zachovat je přístupná ze svých vlastních aplikacích. Oprávnění na úrovni podpisu jsou deklarovány nastavením `Permission` vlastnost `ServiceAttribute` k `signature`:

    ```csharp
    [Service(Name = "com.xamarin.TimestampService",
             Process="com.xamarin.TimestampService.timestampservice_process",
             Permission="signature")]
    public class TimestampService : Service
    {
    }
    ```

2.  **Vytvořit vlastní oprávnění** &ndash; je možné pro vývojáře služby, chcete-li vytvořit vlastní oprávnění pro službu. Toto je nejvhodnější pro když chce sdílet své služby s aplikací od jiných vývojářů vývojář. Vlastní oprávnění vyžaduje trochu další úsilí implementovat, bude uvedena níže.

Zjednodušený příklad vytvoření vlastní `normal` oprávnění budou popsané v další části. Další informace o Android oprávnění, přečtěte si dokumentaci Google [osvědčené postupy a zabezpečení](https://developer.android.com/training/articles/security-tips.html). Další informace o oprávněních Android, najdete v článku [oprávnění části](https://developer.android.com/guide/topics/manifest/manifest-intro.html#perms) Android dokumentace pro další informace o oprávněních Android manifest aplikace.

> [!NOTE]
> Obecně platí [Google nedoporučuje použití vlastní oprávnění](https://developer.android.com/training/articles/security-tips.html#RequestingPermissions) jako jejich může být pro uživatele matoucí.

### <a name="creating-a-custom-permission"></a>Vytváření vlastní oprávnění

Pokud chcete používat vlastní oprávnění, ho není deklarovaná služby při klientovi výslovně požaduje tato oprávnění.

K vytvoření oprávnění v rámci služby APK, `permission` prvek přidán `manifest` element v **AndroidManifest.xml**. Musí mít toto oprávnění `name`, `protectionLevel`, a `label` sady atributů. `name` Musí být nastaven na řetězec, který jednoznačně identifikuje oprávnění. Název se zobrazí v **informace o aplikaci** zobrazení **nastavení Androidu** (jak je znázorněno v následujícím oddílu).

`protectionLevel` Musí být nastaven na jednu z čtyři řetězcové hodnoty, které bylo popsáno výše.  `label` a `description` musí odkazovat na prostředky řetězce a používají se pro uživatelsky přívětivý název a popis pro uživatele.

Tento fragment kódu je příkladem deklarace vlastní `permission` atribut **AndroidManifest.xml** z APK, který obsahuje službu:

```xml
<manifest xmlns:android="http://schemas.android.com/apk/res/android"
          android:versionCode="1"
          android:versionName="1.0"
          package="com.xamarin.xample.messengerservice">

    <uses-sdk android:minSdkVersion="21" />

    <permission android:name="com.xamarin.xample.messengerservice.REQUEST_TIMESTAMP"
                android:protectionLevel="signature"
                android:label="@string/permission_label"
                android:description="@string/permission_description"
                />

    <application android:allowBackup="true"
            android:icon="@mipmap/icon"
            android:label="@string/app_name"
            android:theme="@style/AppTheme">

    </application>
</manifest>
```

Potom, **AndroidManifest.xml** klienta musí explicitně APK požádat toto nové oprávnění. To se provádí přidáním `users-permission` atribut **AndroidManifest.xml**:

```xml
<?xml version="1.0" encoding="utf-8"?>
<manifest xmlns:android="http://schemas.android.com/apk/res/android"
          android:versionCode="1"
          android:versionName="1.0"
          package="com.xamarin.xample.messengerclient">

    <uses-sdk android:minSdkVersion="21" />

    <uses-permission android:name="com.xamarin.xample.messengerservice.REQUEST_TIMESTAMP" />

    <application
            android:allowBackup="true"
            android:icon="@mipmap/icon"
            android:label="@string/app_name"
            android:theme="@style/AppTheme">
    </application>
    </manifest>
```

### <a name="view-the-permissions-granted-to-an-app"></a>Zobrazit oprávnění udělená aplikaci

Chcete-li zobrazit oprávnění, která byla udělena aplikace, otevřete aplikaci Android nastavení a vyberte **aplikace**. Najděte a vyberte aplikaci v seznamu. Z **informace o aplikaci** klepněte na **oprávnění** které se otevře zobrazení, které obsahuje všechna oprávnění udělená aplikaci:

[![Snímky obrazovky ze zařízení se systémem Android znázorňující postup nalezení oprávnění udělená aplikaci](out-of-process-services-images/ipc-06-sml.png)](out-of-process-services-images/ipc-06.png#lightbox)

## <a name="summary"></a>Souhrn

Tato příručka se má rozšířené diskuse o tom, jak spustit Android službu v vzdálený proces. Rozdíly mezi místní a vzdálené služby byl vysvětlené společně s některé z důvodů, proč může být užitečné při stability a výkonu aplikace pro Android vzdálené služby. Po vysvětlením, jak implementovat vzdálené služby a jak klient může komunikovat se službou, v Průvodci se zajistit jeden způsob, jak omezit přístup ke službě z pouze autorizovaní klienti.


## <a name="related-links"></a>Související odkazy

- [obslužné rutiny](https://developer.xamarin.com/api/type/Android.OS.Handler/)
- [Zpráva](https://developer.xamarin.com/api/type/Android.OS.Message/)
- [Messenger](https://developer.xamarin.com/api/type/Android.OS.Messenger/)
- [ServiceAttribute](https://developer.xamarin.com/api/type/Android.App.ServiceAttribute)
- [Atribut exportované](https://developer.android.com/guide/topics/manifest/service-element.html#exported)
- [Služeb izolovaných procesech a vlastní třídy aplikace se nepodaří správně vyřešit přetížení](https://bugzilla.xamarin.com/show_bug.cgi?id=51940)
- [Procesy a vlákna](https://developer.android.com/guide/components/processes-and-threads.html)
- [Android Manifest - oprávnění](https://developer.android.com/guide/topics/manifest/manifest-intro.html#perms)
- [Tipy pro zabezpečení](https://developer.android.com/training/articles/security-tips.html)
- [MessengerServiceDemo (sample)](https://developer.xamarin.com/samples/monodroid/ApplicationFundamentals/ServiceSamples/MessengerServiceDemo/)
