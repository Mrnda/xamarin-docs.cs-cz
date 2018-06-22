---
title: Vázaný služby v Xamarin.Android
description: Vázané služby jsou Android služby, které poskytují rozhraní klient server, který klienta (například aktivitu, Android) mohou komunikovat s. Tato příručka popisuje klíčové komponenty, které jsou spojené s vytvářením vázané služby a způsobu jeho použití v aplikaci Xamarin.Android.
ms.prod: xamarin
ms.assetid: 809ECE88-EF08-4E9A-B389-A2DC08C51A6E
ms.technology: xamarin-android
author: topgenorth
ms.author: toopge
ms.date: 05/04/2018
ms.openlocfilehash: f4fe1bd753260f05dedb452655572d290c0781d0
ms.sourcegitcommit: daa089d41cfe1ed0456d6de2f8134cf96ae072b1
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 05/07/2018
ms.locfileid: "33850571"
---
# <a name="bound-services-in-xamarinandroid"></a>Vázaný služby v Xamarin.Android

_Vázané služby jsou Android služby, které poskytují rozhraní klient server, který klienta (například aktivitu, Android) mohou komunikovat s. Tato příručka popisuje klíčové komponenty, které jsou spojené s vytvářením vázané služby a způsobu jeho použití v aplikaci Xamarin.Android._

## <a name="bound-services-overview"></a>Přehled vázané služby

Služby, které poskytují rozhraní klient server pro klienty přímo komunikovat se službou se označují jako _vázaný služby_.  Může existovat více klientů připojených do jediné instance služby ve stejnou dobu. Vázané službu a klienta jsou od sebe navzájem oddělené. Místo toho Android poskytuje řadu mezilehlých objektů, které spravují stav připojení mezi těmito dvěma. Tento stav je možný díky objekt, který implementuje [ `Android.Content.IServiceConnection` ](https://developer.xamarin.com/api/type/Android.Content.IServiceConnection/) rozhraní.  Tento objekt se vytvoří klientem a předá jako parametr, který se [ `BindService` ](https://developer.xamarin.com/api/member/Android.Content.Context.BindService/) metoda. `BindService` Je k dispozici na všech [ `Android.Content.Context` ](https://developer.xamarin.com/api/type/Android.Content.Context) objektu (například aktivitu). Je požadavek na operační systém Android ke spuštění služby a vytvořit vazbu klienta. Existují tři způsoby, jak klient může vytvořit vazbu na službu pomocí `BindService` metoda:

* **Vazač služby** &ndash; vazač služby je třída, která implementuje [ `Android.OS.IBinder` ](https://developer.xamarin.com/api/type/Android.OS.IBinder) rozhraní. Většina aplikací nebude toto rozhraní implementovat přímo, namísto toho bude rozšíření [ `Android.OS.Binder` ](https://developer.xamarin.com/api/type/Android.OS.Binder) třídy. Toto je nejběžnější přístup a je vhodný pro při službu a klienta existovat v rámci jednoho procesu.
* **Použití Messenger** &ndash; tento postup je vhodný pro když tato služba může existovat jako samostatný proces. Místo toho jsou mezi klientem a službou prostřednictvím zařazeno žádosti o služby [ `Android.OS.Messenger` ](https://developer.xamarin.com/api/type/Android.OS.Messenger). [ `Android.OS.Handler` ](https://developer.xamarin.com/api/type/Android.OS.Handler) Se vytvoří ve službě, která bude zpracovávat `Messenger` požadavky. To se budeme v jiné příručce.
* **Pomocí AIDL** &ndash; to narazilo tchyba v srdcem většina vývojáře v systému Android a nebude součástí této příručky.

Jakmile klient byla svázána se služby, je komunikace mezi těmito dvěma dojde k prostřednictvím `Android.OS.IBinder` objektu.  Tento objekt je zodpovědná za rozhraní, které vám umožní klient komunikovat se službou. Není nutné pro každou aplikaci Xamarin.Android implementace tohoto rozhraní od začátku, Android SDK poskytuje [ `Android.OS.Binder` ](https://developer.xamarin.com/api/type/Android.OS.Binder) třídy, který má na starosti většinu vyžaduje s zařazování objektu mezi kód Klient a služba.

Pokud klienta se provádí se službou, jeho musí odpojení od ho voláním `UnbindService` metoda. Jakmile má klient poslední nevázaných ze služby, Android zastaví a dispose vázané služby.

Tento diagram znázorňuje, jak aktivity, připojení služby, vazač a služba vzájemně souvisí:

![Diagram znázorňující, jak součásti služby vztahují k sobě navzájem](bound-services-images/bound-services-02.png "diagram znázorňující, jak součásti služby vztahují k sobě navzájem.")

Tato příručka popisuje postup rozšíření `Service` třídu pro implementaci vázané služby. Také se týkají implementace `IServiceConnection` a rozšíření `Binder` umožnit klientům pro komunikaci se službou. Ukázkovou aplikaci doprovází této příručce, které obsahují řešení s jeden projekt Xamarin.Android názvem **[BoundServiceDemo](https://github.com/xamarin/monodroid-samples/tree/master/ApplicationFundamentals/ServiceSamples/BoundServiceDemo)** . Toto je velmi základní aplikaci, která ukazuje, jak implementovat služby a jak vytvořit vazbu aktivitu. Vázané služba má velmi jednoduché rozhraní API s pouze jednu metodu `GetFormattedTimestamp`, která vrací řetězec, který uživateli říká, když služba spuštěna a jak dlouho po spuštění. Aplikace také umožňuje uživateli ručně zrušit vazbu a vytvořit vazbu ke službě.

[![Snímek obrazovky aplikace běžící na telefon se systémem Android](bound-services-images/bound-services-03-sml.png)](bound-services-images/bound-services-03.png#lightbox)

## <a name="implementing-and-consuming-a-bound-service"></a>Implementace a využívání vázané služby

Existují tři součásti, které musí být implementován v pořadí pro aplikace pro Android využívat vázané služby:

1. **Rozšíření `Service` třídy a metody zpětného volání životního cyklu implementovat** &ndash; tuto třídu bude obsahovat kód, který provede práci, kterou bude vyžádána služby. To bude možné podrobněji níže.
2. **Vytvořte třídu, že implementuje `IServiceConnection`**  &ndash; toto rozhraní, poskytuje metody zpětného volání, bude vyvolána Android o upozornění klienta při připojení ke službě došlo ke změně, má tj klienta připojení nebo odpojení na Služba. Připojení služby bude také zadejte odkaz na objekt, který může klient použít přímo komunikovat se službou. Tento odkaz se označuje jako _vazač_.
3. **Vytvořte třídu, že implementuje `IBinder`**  &ndash; A _vazač_ implementace poskytuje rozhraní API, které klient používá ke komunikaci se službou. Vazač můžete buď zadat odkaz na službu vázané umožňuje metody, které má být volána přímo nebo vazač poskytnout klienta rozhraní API, který zapouzdřuje a skryje vázané službu z aplikace. `IBinder` Musíte zadat kód potřebné pro volání vzdálené procedury. Není nutné (nebo doporučené) k implementaci `IBinder` rozhraní přímo. Místo toho by měla aplikace rozšířit `Binder` typ, který obsahuje většinu základní funkce vyžadují `IBinder`.
4. **Spuštění a vytvoření vazby na služby** &ndash; po vytvoření připojení služby, vazač a služba aplikace pro Android je zodpovědný za spouštění služby a vytvoření vazby na ni.

Každý z těchto kroků bude popsané v následující části obsahují další podrobnosti.

### <a name="extend-the-service-class"></a>Rozšíření `Service` – třída

Vytvoření služby pomocí Xamarin.Android, je potřeba podtřídami `Service` a adorn třídě s [ `ServiceAttribute` ](https://developer.xamarin.com/api/type/Android.App.ServiceAttribute). Atribut se používá nástroje pro sestavení Xamarin.Android správně zaregistrovat službu do aplikace **AndroidManifest.xml** souboru podobně jako aktivity, vázané služba má vlastní životního cyklu a zpětného volání metody přidružené důležité události v jeho průběhu životního cyklu. V následujícím seznamu je příklad některých běžných metody zpětného volání, které bude implementace služby:

* `OnCreate` &ndash; Tato metoda je volána Android, jak ho je vytvoření instance služby. Slouží k chybě při inicializaci všechny proměnné nebo objekty, které vyžaduje služba během celé jeho životnosti. Tato metoda je volitelné.
* `OnBind` &ndash; Tato metoda musí být implementované všechny vázané služby. Vyvolání při první klient se pokusí připojit ke službě. Vrátí instanci `IBinder` tak, že klient může komunikovat se službou. Tak dlouho, dokud je služba spuštěná, `IBinder` objektu se použije ke splnění budoucí požadavky pro vazbu ke službě.
* `OnUnbind` &ndash; Tato metoda je volána, když mají nevázaných všechny vázané klienty. Vrácením `true` z této metody bude služba později volat `OnRebind` se záměrem předaný `OnUnbind` při nových klientů svázat s ní. Když služba dál spuštěný po jejím nevázaný byste to provedli. To by mohlo dojít, pokud byly nedávno nevázaný služby taky spuštěná služba a `StopService` nebo `StopSelf` kdyby byla volána. V takové situaci `OnRebind` umožňuje záměr mají být načteny. Vrátí výchozí `false` , který neprovede žádnou akci. Volitelné.
* `OnDestroy` &ndash; Tato metoda je volána, když je Android zničení službu. Všechny nezbytné čištění, jako je například uvolnění prostředků, je třeba provést v této metodě. Volitelné.

Události životního cyklu u klíče vázané služby jsou uvedeny v tomto diagramu:

![Diagram zobrazující pořadí, ve kterém jsou volány metody životního cyklu](bound-services-images/bound-services-01.png "diagram zobrazující pořadí, ve kterém jsou volány metody životního cyklu.")

Následující fragment kódu z doprovodné aplikace, který doprovází tato příručka ukazuje, jak implementovat vázané služby v Xamarin.Android:

```csharp
using Android.App;
using Android.Util;
using Android.Content;
using Android.OS;

namespace BoundServiceDemo
{
    [Service(Name="com.xamarin.ServicesDemo1")]
    public class TimestampService : Service, IGetTimestamp
    {
        static readonly string TAG = typeof(TimestampService).FullName;
        IGetTimestamp timestamper;

        public IBinder Binder { get; private set; }

        public override void OnCreate()
        {
            // This method is optional to implement
            base.OnCreate();
            Log.Debug(TAG, "OnCreate");
            timestamper = new UtcTimestamper();
        }

        public override IBinder OnBind(Intent intent)
        {
            // This method must always be implemented
            Log.Debug(TAG, "OnBind");
            this.Binder = new TimestampBinder(this);
            return this.Binder;
        }

        public override bool OnUnbind(Intent intent)
        {
            // This method is optional to implement
            Log.Debug(TAG, "OnUnbind");
            return base.OnUnbind(intent);
        }

        public override void OnDestroy()
        {
            // This method is optional to implement
            Log.Debug(TAG, "OnDestroy");
            Binder = null;
            timestamper = null;
            base.OnDestroy();
        }

        /// <summary>
        /// This method will return a formatted timestamp to the client.
        /// </summary>
        /// <returns>A string that details what time the service started and how long it has been running.</returns>
        public string GetFormattedTimestamp()
        {
            return timestamper?.GetFormattedTimestamp();
        }
    }
}
```

V ukázce `OnCreate` metoda inicializuje objekt, který obsahuje logiku pro načítání a formátování časové razítko, které by požadované klientem. Při první klient se pokusí vytvořit vazbu na službu, bude vyvolání Android `OnBind` metoda. Tato služba vytvoří instanci `TimestampServiceBinder` objekt, který vám umožní klientům přístup k této instanci spuštěné služby. `TimestampServiceBinder` Třída je popsané v další části.

### <a name="implementing-ibinder"></a>Implementace IBinder

Jak je uvedeno, `IBinder` objekt poskytuje komunikační kanál mezi klientem a služby. Aplikace pro Android by neměly implementovat `IBinder` rozhraní, [ `Android.OS.Binder` ](https://developer.xamarin.com/api/type/Android.OS.Binder/) rozšířeno. `Binder` Třída poskytuje mnohem potřebnou infrastrukturu, která je potřebná zařazování objekt vazače ze služby (která může být spuštěná jako samostatný proces) do klienta. Ve většině případů `Binder` podtřídami je jenom pár řádků kódu a zabalí odkaz na službu. V tomto příkladu `TimestampBinder` má vlastnost, která zveřejňuje `TimestampService` klientovi:

```csharp
public class TimestampBinder : Binder
{
    public TimestampBinder(TimestampService service)
    {
        this.Service = service;
    }

    public TimestampService Service { get; private set; }
}
```

To `Binder` umožňuje vyvolání veřejné metody ve službě; například:

```csharp
string currentTimestamp = serviceConnection.Binder.Service.GetFormattedTimestamp()
```

Jednou `Binder` bylo rozšířeno, je nutné implementovat `IServiceConnection` všechno připojovat najednou k.

### <a name="creating-the-service-connection"></a>Vytvoření připojení k službě

`IServiceConnection` Nabídne | zavést | vystavit | připojit `Binder` objektu do klienta. Kromě implementace `IServiceConnection` rozhraní, musíte rozšířit třídu `Java.Lang.Object`. Připojení služby by měl poskytovat také nějakým způsobem, který má klient přístup `Binder` (a proto komunikovat se službou vázané).

Tento kód je z doprovodných ukázkový projekt je jeden možný způsob, jak implementovat `IServiceConnection`:

```csharp
using Android.Util;
using Android.OS;
using Android.Content;

namespace BoundServiceDemo
{
    public class TimestampServiceConnection : Java.Lang.Object, IServiceConnection, IGetTimestamp
    {
        static readonly string TAG = typeof(TimestampServiceConnection).FullName;

        MainActivity mainActivity;
        public TimestampServiceConnection(MainActivity activity)
        {
            IsConnected = false;
            Binder = null;
            mainActivity = activity;
        }

        public bool IsConnected { get; private set; }
        public TimestampBinder Binder { get; private set; }

        public void OnServiceConnected(ComponentName name, IBinder service)
        {
            Binder = service as TimestampBinder;
            IsConnected = this.Binder != null;

            string message = "onServiceConnected - ";
            Log.Debug(TAG, $"OnServiceConnected {name.ClassName}");

            if (IsConnected)
            {
                message = message + " bound to service " + name.ClassName;
                mainActivity.UpdateUiForBoundService();
            }
            else
            {
                message = message + " not bound to service " + name.ClassName;
                mainActivity.UpdateUiForUnboundService();
            }

            Log.Info(TAG, message);
            mainActivity.timestampMessageTextView.Text = message;

        }

        public void OnServiceDisconnected(ComponentName name)
        {
            Log.Debug(TAG, $"OnServiceDisconnected {name.ClassName}");
            IsConnected = false;
            Binder = null;
            mainActivity.UpdateUiForUnboundService();
        }

        public string GetFormattedTimestamp()
        {
            if (!IsConnected)
            {
                return null;
            }

            return Binder?.GetFormattedTimestamp();
        }
    }
}

```

Jako součást procesu vázání, bude vyvolání Android `OnServiceConnected` metoda, poskytuje `name` služby, který je právě vázaný a `binder` který obsahuje odkaz na službu samotnou. Připojení služby v tomto příkladu má dvě vlastnosti, který obsahuje odkaz na vazač a logický příznak pro, pokud je klient připojen ke službě nebo ne.

`OnServiceDisconnected` Metoda je volána, pouze když je připojení mezi klientem a služba neočekávaně ztráty nebo poškozený. Tato metoda umožňuje klientovi prvního reagovat na narušení chodu služby.  

## <a name="starting-and-binding-to-a-service-with-an-explicit-intent"></a>Spuštění a připojení k službě se záměrem explicitní

Pomocí vázané služby, musí klient (například aktivitu) vytvořit instanci objektu, který implementuje `Android.Content.IServiceConnection` a vyvolání `BindService` metoda.` BindService` Vrátí `true` Pokud je vázána služby, `false` Pokud není. `BindService` Metoda přijímá tři parametry:

* **`Intent`**  &ndash; The záměr měli explicitně identifikovat služby, která pro připojení k.
* **`IServiceConnection` Objekt** &ndash; tento objekt je prostředník, který poskytuje metody zpětného volání o upozornění klienta při spuštění a zastavení vázané služby.
* **[`Android.Content.Bind`](https://developer.xamarin.com/api/type/Android.Content.Bind/) výčet** &ndash; tento parametr je sada příznaky jsou používány v systému pro při vázání objektu. Nejčastěji používané hodnota je [ `Bind.AutoCreate` ](https://developer.xamarin.com/api/field/Android.Content.Bind.AutoCreate/), který se automaticky spustí službu, pokud ještě není spuštěná.

Následující fragment kódu je příklad toho, jak spustit v aktivitě pomocí explicitní záměrem vázané službu:

```csharp
protected override void OnStart ()
{
    base.OnStart ();

    if (serviceConnection == null)
    {
        this.serviceConnection = new TimestampServiceConnection(this);
    }

    Intent serviceToStart = new Intent(this, typeof(TimestampService));
    BindService(serviceToStart, this.serviceConnection, Bind.AutoCreate);

}
```

> [!IMPORTANT]
> Od verze Android 5.0 (API úrovně 21) ho je možné pouze při vytváření vazby ke službě se záměrem explicitní.

## <a name="architectural-notes-about-the-service-connection-and-the-binder"></a>Architektury poznámky o připojení služby a vazače.

Některé purists mimoprocesová aplikace VOLÁNA může dohlížitele odmítnout předchozí implementace `TimestampBinder` třídy, jako je porušení [zákonem Demeter](https://en.wikipedia.org/wiki/Law_of_Demeter) , který v nejjednodušší podobě stavy "nemáte komunikují s neznámých osob; pouze obraťte se na vaši přátelé". Tato konkrétní implementace zpřístupní konkrétní `TimestampService` třída pro všechny klienty.

Přesněji řečeno, není nutné pro klienta upozornit `TimestampService` a vystavení konkrétní třídy klientům mohl provádět aplikace více křehká a těžší udržovat po jeho životnosti. Alternativní přístup je použití rozhraní, která zpřístupňuje `GetFormattedTimestamp()` metoda a proxy volání služby pomocí `Binder` (nebo možné třídě připojení služby):  

```csharp
public class TimestampBinder : Binder, IGetTimestamp
{
    TimestampService service;
    public TimestampBinder(TimestampService service)
    {
        this.service = service;
    }

    public string GetFormattedTimestamp()
    {
        return service?.GetFormattedTimestamp();
    }
}
```

V tomto konkrétním příkladu povolit aktivity k vyvolání metody na samotnou službu:

```csharp
// In this example the Activity is only talking to a friend, i.e. the IGetTimestamp interface provided by the Binder.
string currentTimestamp = serviceConnection.Binder.GetFormattedTimestamp()
```


## <a name="related-links"></a>Související odkazy

- [Android.App.Service](https://developer.xamarin.com/api/type/Android.App.Service/)
- [Android.Content.Bind](https://developer.xamarin.com/api/type/Android.Content.Bind/)
- [Android.Content.Context](https://developer.xamarin.com/api/type/Android.Content.Context/)
- [Android.Content.IServiceConnection](https://developer.xamarin.com/api/type/Android.Content.IServiceConnection/)
- [Android.OS.Binder](https://developer.xamarin.com/api/type/Android.OS.Binder/)
- [Android.OS.IBinder](https://developer.xamarin.com/api/type/Android.OS.IBinder)
- [BoundServiceDemo (ukázka)](https://developer.xamarin.com/samples/monodroid/ApplicationFundamentals/ServiceSamples/BoundServiceDemo/)
