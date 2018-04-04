---
title: Umístění služby
description: Tento průvodce uvádí sledování umístění v aplikacích Android a ukazuje, jak získat umístění uživatele pomocí rozhraní API služby Google umístění rozhraní API systému Android umístění služby a také k dispozici roztaveného umístění poskytovatele.
ms.prod: xamarin
ms.assetid: 0008682B-6CEF-0C1D-3200-56ECF58F5D3C
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/16/2018
ms.openlocfilehash: 366c75db49a7e0f4f559b13c0871071dee2f08e3
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/04/2018
---
# <a name="location-services"></a>Umístění služby

_Tento průvodce uvádí sledování umístění v aplikacích Android a ukazuje, jak získat umístění uživatele pomocí rozhraní API služby Google umístění rozhraní API systému Android umístění služby a také k dispozici roztaveného umístění poskytovatele._

## <a name="location-services-overview"></a>Přehled služby umístění

Android poskytuje přístup k různé technologie umístění jako umístění věž buňky, Wi-Fi a GPS. Podrobnosti o každém umístění technologie jsou abstrahované prostřednictvím *umístění zprostředkovatelé*, povolíte aplikací se stejným způsobem, bez ohledu na to zprostředkovatel použitý získat umístění. Tento průvodce představuje roztaveného umístění poskytovatele, součástí Google Play Services, která inteligentně určuje nejlepší způsob, jak získat umístění zařízení na základě které poskytovatelé jsou k dispozici a jak zařízení používá. Rozhraní API systému Android umístění služby a ukazuje, jak ke komunikaci s systému umístění služby pomocí `LocationManager`. Druhá část průvodce jsou zde popsány rozhraní API systému Android umístění služeb pomocí `LocationManager`.
 
Jako obecné pravidlo aplikace by měla dávají přednost používání roztaveného umístění poskytovatele, návratem zpět starší umístění služby rozhraní API systému Android pouze v případě potřeby.

## <a name="location-fundamentals"></a>Základní informace o umístění

V systému Android jaké rozhraní API, které jste vybrali pro práci s daty umístění, není podstatné, několik konceptů zůstávají stejné. Tato část představuje umístění poskytovatelé a umístění souvisí oprávnění.

### <a name="location-providers"></a>Zprostředkovatelé umístění

Několik technologií se používá interně ke kotvícímu bodu umístění uživatele. Hardwaru používaného závisí na typu *umístění poskytovatele* vybrané úlohy shromažďování dat o. Android používá tři zprostředkovatelé umístění:

-   **Zprostředkovatel GPS** &ndash; GPS poskytuje nejaktuálnější umístění, používá nejvíce napájení a funguje nejlépe venku. Tento zprostředkovatel používá kombinaci GPS a odbornou GPS ([aGPS](http://en.wikipedia.org/wiki/Assisted_GPS)), která vrací GPS data shromažďovaná společností mobilní towers.

-   **Sítě zprostředkovatele** &ndash; poskytuje kombinaci Wi-Fi a mobilní data, včetně aGPS data shromažďovaná společností towers buňky. Používá menší výkon než poskytovatel GPS, ale vrátí data o umístění různých přesnosti.

-   **Pasivní zprostředkovatele** &ndash; "složená" možnost pomocí poskytovatelů požadoval jinými aplikacemi nebo službami pro generování umístění dat v aplikaci. To je méně spolehlivé ale úsporný možnost ideální pro aplikace, které nevyžadují konstantní umístění aktualizace pracovat.

Zprostředkovatelé umístění nejsou vždy k dispozici. Například jsme chtít používat GPS pro naši aplikaci, ale může být vypnuto GPS v nastavení nebo GPS zařízení nemusí mít vůbec. Pokud není k dispozici konkrétního poskytovatele, výběr tohoto zprostředkovatele může vrátit `null`.

### <a name="location-permissions"></a>Umístění oprávnění

Umístění aplikace potřebuje přístup senzorů hardwaru zařízení přijímat GPS, Wi-Fi a mobilní data. Přístup je řízen pomocí příslušná oprávnění v Android Manifest aplikace.
Nejsou k dispozici dvě oprávnění &ndash; v závislosti na požadavcích vaší aplikace a zvoleného rozhraní API, můžete povolit jeden:

-   `ACCESS_FINE_LOCATION` &ndash; Umožňuje aplikaci přístup k GPS.
    Vyžaduje se pro *GPS zprostředkovatele* a *pasivní zprostředkovatele* možnosti (*oprávnění k přístupu k GPS data shromážděná jiná aplikace nebo služby, musí pasivní zprostředkovatel*). Volitelné oprávnění *poskytovatele sítě*.

-   `ACCESS_COARSE_LOCATION` &ndash; Umožňuje aplikaci přístup k umístění, mobilní a Wi-Fi. Vyžaduje se pro *poskytovatele sítě* Pokud `ACCESS_FINE_LOCATION` není nastaven.

Pro aplikace, které cílí na rozhraní API verze 21 (typu Lupa Android 5.0) nebo vyšší, můžete povolit `ACCESS_FINE_LOCATION` a stále spustit na zařízení, které nemají GPS hardwaru. Pokud vaše aplikace vyžaduje GPS hardwaru, je nutné explicitně přidat `android.hardware.location.gps` `uses-feature` element pro Android Manifest. Další informace najdete v tématu Android [používá funkci](https://developer.android.com/guide/topics/manifest/uses-feature-element.html) odkaz na element.

Pokud chcete nastavit oprávnění, rozbalte položku **vlastnosti** složky v **řešení Pad** a dvakrát klikněte na **AndroidManifest.xml**. Oprávnění, zobrazí se v části **požadovaných oprávnění**:

[![Snímek obrazovky nastavení Android Manifest požadovaných oprávnění](location-images/location-01-xs.png)](location-images/location-01-xs.png#lightbox)

Nastavení některý z těchto oprávnění informuje Android, že aplikace potřebuje oprávnění od uživatele, aby měli přístup k umístění zprostředkovatele. Zařízení, která spustit úroveň rozhraní API 22 (Android 5.1) nebo nižší vyzve uživatele k udělení těchto oprávnění pokaždé, když je aplikace nainstalovaná. Na zařízení se systémem rozhraní API na úrovni 23 (Android 6.0) nebo vyšší, aplikace by měla provést kontrolu oprávnění pro spuštění před provedením požadavku umístění poskytovatele. 

> [!NOTE]
>Poznámka: Nastavení `ACCESS_FINE_LOCATION` znamená přístup k datům obou hrubým a dobře umístění. Nikdy byste měli mít nastavit obě oprávnění pouze *minimální* oprávnění aplikace vyžaduje k práci.

Příkladem jak zkontrolovat, že aplikace má oprávnění pro tento fragment kódu je `ACCESS_FINE_LOCATION` oprávnění:

```csharp
 if (ContextCompat.CheckSelfPermission(this, Manifest.Permission.AccessFineLocation) == Permission.Granted)
{
    StartRequestingLocationUpdates();
    isRequestingLocationUpdates = true;
}
else
{
    // The app does not have permission ACCESS_FINE_LOCATION 
}
```

Aplikace musí být odolný vůči chybám scénáře, kde uživatel nebude udělit oprávnění (nebo má odvolat oprávnění) a mají způsob, jak řádně řešení této situace. Najdete v tématu [oprávnění průvodce](~/android/app-fundamentals/permissions.md) další podrobnosti o implementaci oprávnění spuštění kontrol v Xamarin.Android.


## <a name="using-the-fused-location-provider"></a>Pomocí zprostředkovatele roztaveného umístění

Zprostředkovatel roztaveného umístění je upřednostňovaný způsob pro aplikace pro Android a získat aktualizace umístění ze zařízení, protože efektivně vybere umístění poskytovatele při běhu poskytnout informace o osvědčených umístění baterie efektivním způsobem. Například chodí po venkovní, kde uživatel získá čtení s GPS nejlepší umístění. Pokud uživatel potom provede ve vnitřním uzavřeném prostoru, kde GPS funguje špatně (pokud vůbec), Wi-Fi, která funguje ve vnitřním uzavřeném prostoru lepší může automaticky přepnout roztaveného umístění poskytovatele.
 
Zprostředkovatel roztaveného umístění rozhraní API poskytuje celou řadu dalších nástrojů na základě kterého umístění aplikace, včetně monitorování aktivit a monitorování geografických zón. V této části přidáme zaměřit se na základní informace o nastavení `LocationClient`, zřízením poskytovatelů a získávání umístění uživatele.

Zprostředkovatel roztaveného umístění je součástí [služby Google Play](http://developer.android.com/google/play-services/index.html). Balíček služby Google Play musí být nainstalovaná a správně nakonfigurovaná v aplikaci pro zprostředkovatele roztaveného umístění rozhraní API pro práci, a zařízení musí mít APK přehrání služby Google nainstalována.

Před Xamarin.Android aplikace můžete použít poskytovatele roztaveného umístění, je nutné přidat **Xamarin.GooglePlayServices.Maps** do projektu.

### <a name="checking-if-google-play-services-is-installed"></a>Kontrola, zda je nainstalován služby Google Play

By tomu bylo Xamarin.Android bude selhat, pokud se ji pokusí použít roztaveného umístění poskytovatele, pokud není nainstalován služby Google Play (nebo aktuální) pak výjimku modulu runtime.  Pokud není nainstalovaná služby Google Play, aplikace by měla vrátit zpět k Android služby umístění výše popsané. Pokud služby Google Play je zastaralý, potom aplikace může zobrazit zprávu pro uživatele, které je vyzývají k aktualizaci nainstalovaná verze služeb Google Play.

Tento fragment kódu je příklad, jak můžete Android aktivitu prostřednictvím kódu programu Kontrola instalace služby Google Play:

```csharp
bool IsGooglePlayServicesInstalled()
{
    var queryResult = GoogleApiAvailability.Instance.IsGooglePlayServicesAvailable(this);
    if (queryResult == ConnectionResult.Success)
    {
        Log.Info("MainActivity", "Google Play Services is installed on this device.");
        return true;
    }

    if (GoogleApiAvailability.Instance.IsUserResolvableError(queryResult))
    {
        // Check if there is a way the user can resolve the issue
        var errorString = GoogleApiAvailability.Instance.GetErrorString(queryResult);
        Log.Error("MainActivity", "There is a problem with Google Play Services on this device: {0} - {1}",
                  queryResult, errorString);
                  
        // Alternately, display the error to the user.
    }

    return false;
}
```

### <a name="fusedlocationproviderclient"></a>FusedLocationProviderClient

Komunikovat s poskytovatelem roztaveného umístění, musí mít aplikace pro Xamarin.Android instanci `FusedLocationProviderClient`. Tato třída poskytuje metody potřebné k přihlásit k odběru aktualizací umístění a k načítání poslední známé umístění zařízení.

`OnCreate` Metoda aktivity je vhodné místo, kde můžete získat odkaz na `FusedLocationProviderClient`, jak je znázorněno v následující fragment kódu:

```csharp
public class MainActivity: AppCompatActivity
{
    FusedLocationProviderClient fusedLocationProviderClient;

    protected override void OnCreate(Bundle bundle) 
    {
        fusedLocationProviderClient = LocationServices.GetFusedLocationProviderClient(this);
    }
}
```

### <a name="getting-the-last-known-location"></a>Získání poslední známé umístění

`FusedLocationProviderClient.GetLastLocationAsync()` Metoda poskytuje jednoduché a neblokující způsob pro aplikace pro Xamarin.Android se rychle získat poslední známé umístění zařízení s minimální kódování režie.

Tento fragment kódu ukazuje způsob použití `GetLastLocationAsync` metoda pro načtení umístění zařízení:

```csharp
async Task GetLastLocationFromDevice()
{
    // This method assumes that the necessary run-time permission checks have succeeded.
    getLastLocationButton.SetText(Resource.String.getting_last_location);
    Android.Locations.Location location = await fusedLocationProviderClient.GetLastLocationAsync();

    if (location == null)
    {
        // Seldom happens, but should code that handles this scenario
    }
    else
    {
        // Do something with the location 
        Log.Debug("Sample", "The latitude is " + location.Latitude);
    }
}
```

### <a name="subscribing-to-location-updates"></a>Přihlášení k odběru do umístění aktualizace

Aplikace pro Xamarin.Android můžete také přihlásit k odběru aktualizací umístění ze zprostředkovatele roztaveného umístění pomocí `FusedLocationProviderClient.RequestLocationUpdatesAsync` metoda, jak ukazuje tento fragment kódu:

```csharp
await fusedLocationProviderClient.RequestLocationUpdatesAsync(locationRequest, locationCallback);
```
Tato metoda přebírá dva parametry:

-   **`Android.Gms.Location.LocationRequest`** &ndash; A `LocationRequest` objekt je, jak aplikace pro Xamarin.Android předává parametry na fungování roztaveného umístění poskytovatele. `LocationRequest` Obsahuje informace takové také o tom, je třeba častými požadavky nebo jak důležité aktualizace přesné umístění by měly být. Například požadavek důležité umístění způsobí, že zařízení použít GPS a v důsledku toho více energie, při určování umístění. Tento fragment kódu ukazuje, jak vytvořit `LocationRequest` k umístění s vysokou přesnost, kontrola přibližně každých pět minut pro aktualizace umístění (ale ne dřív, než dvě minuty mezi požadavky). Zprostředkovatel roztaveného umístění použije `LocationRequest` jako pokyny pro umístění poskytovatele, kterého chcete použít při pokusu o k určení umístění zařízení:

    ```csharp
    LocationRequest locationRequest = new LocationRequest()
                                      .SetPriority(LocationRequest.PriorityHighAccuracy)
                                      .SetInterval(60 * 1000 * 5)
                                      .SetFastestInterval(60 * 1000 * 2);
    ```
                                          

-   **`Android.Gms.Location.LocationCallback`** &ndash; Aby bylo možné přijímat aktualizace umístění, musí aplikace pro Xamarin.Android podtřídami `LocationProvider` abstraktní třídy. Tato třída zveřejněné dvě metody, které může být volána zprostředkovatelem roztaveného umístění na aktualizaci aplikace s informacemi o umístění. To bude možné podrobněji popsána níže.

Oznámit aplikace pro Xamarin.Android aktualizace umístění, bude vyvolání roztaveného umístění poskytovatele `LocationCallBack.OnLocationResult(LocationResult result)`. `Android.Gms.Location.LocationResult` Parametr bude obsahovat informace o aktualizaci umístění.

Pokud zprostředkovatele roztaveného umístění zjistí změnu v dostupnosti data o umístění, zavolá `LocationProvider.OnLocationAvaibility(LocationAvailability
locationAvailability)` metoda. Pokud `LocationAvailability.IsLocationAvailable` vlastnost vrátí `true`, pak dá se předpokládat, že výsledky umístění zařízení hlášené `OnLocationResult` jako přesné a jako aktuální podle požadavků `LocationRequest`. Pokud `IsLocationAvailable` je nastavena hodnota false, bude návratový podle žádné výsledky umístění `OnLocationResult`.

Tento fragment kódu je ukázka implementace `LocationCallback` objektu:

```csharp
public class FusedLocationProviderCallback : LocationCallback
{
    readonly MainActivity activity;

    public FusedLocationProviderCallback(MainActivity activity)
    {
        this.activity = activity;
    }

    public override void OnLocationAvailability(LocationAvailability locationAvailability)
    {
        Log.Debug("FusedLocationProviderSample", "IsLocationAvailable: {0}",locationAvailability.IsLocationAvailable);
    }

    public override void OnLocationResult(LocationResult result)
    {
        if (result.Locations.Any())
        {
            var location = result.Locations.First();
            Log.Debug("Sample", "The latitude is :" + location.Latitude);
        }
        else
        {
            // No locations to work with.
        }
    }
}
```

## <a name="using-the-android-location-service-api"></a>Pomocí rozhraní API systému Android umístění služby

Android služby umístění je starší rozhraní API pro používání informace o umístění v systému Android. Data o umístění se shromažďují hardwaru senzory a shromažďují službou systému, který je přístupný v aplikaci s `LocationManager` třídy a `ILocationListener`.

Umístění služby je nejvhodnější pro aplikace, které musí spustit na zařízení, které nemají Google Play Services nainstalována.

Umístění služby je zvláštní druh [služby](http://developer.android.com/guide/components/services.html) spravuje systému. Systémová služba komunikuje s hardwarem zařízení a je vždy spuštěn. Klepnout na do umístění aktualizace v naší aplikaci, jsme se přihlásit k odběru umístění aktualizace z umístění služby pomocí systému `LocationManager` a `RequestLocationUpdates` volání.

Získat umístění uživatele pomocí Android umístění služby zahrnuje několik kroků:

1.  Získat odkaz na `LocationManager` služby.
2.  Implementace `ILocationListener` rozhraní a popisovač události při změně umístění.
3.  Použití `LocationManager` žádosti o umístění aktualizacemi pro zadaného zprostředkovatele. `ILocationListener` z předchozího kroku se použije pro příjem zpětná volání z `LocationManager`.
4.  Aktualizace umístění zastavte, když aplikace je už vhodné přijímat aktualizace.

### <a name="location-manager"></a>Správce umístění

Jsme mají přístup ke službě umístění systému s instancí `LocationManager` třídy. `LocationManager` je speciální třídu, která umožňuje nám interakci s umístění systému služby a v něm volat metody. Aplikaci můžete získat odkaz na `LocationManager` voláním `GetSystemService` a předávání v typ služby, jak je uvedeno níže:

```csharp
LocationManager locationManager = (LocationManager) GetSystemService(Context.LocationService);
```

`OnCreate` je vhodná k získání odkazu na `LocationManager`.
Je vhodné ponechat `LocationManager` jako proměnné třídy, takže jsme můžete volat v různých fázích životního cyklu aktivity.

### <a name="request-location-updates-from-the-locationmanager"></a>Aktualizace umístění požádat LocationManager

Jakmile aplikace obsahuje odkaz na `LocationManager`, je potřeba říct `LocationManager` jaký typ informace o umístění, které jsou požadované, a jak často se bude aktualizovat tyto informace. To provedete volání `RequestionLocationUpdates` na `LocationManager` objekt a předávání některých kritéria pro aktualizace a zpětné volání, které budou přijímat aktualizace umístění. Tato zpětné volání je typ, který musí implementovat `ILocationListener` rozhraní (podrobněji popsané v dál v této příručce).

`RequestionLocationUpdates` Metoda řekne systému umístění služby, aplikace se má začít přijímat aktualizace umístění. Tato metoda umožňuje zadat na zprostředkovatele, a také čas a vzdálenost prahové hodnoty pro řízení četnosti aktualizace. Například metoda níže níže žádosti o umístění aktualizace z umístění poskytovatele GPS každých milisekund 2000 a pouze při umístění změní více než 1 m:

```csharp
// For this example, this method is part of a class that implements ILocationListener, described below
locationManager.RequestLocationUpdates(LocationManager.GpsProvider, 2000, 1, this);
```

Aplikace by měla pouze tak často, podle potřeby pro aplikaci k provedení dobře vyžádat umístění aktualizace. To ponechá výdrž baterie a vytvoří lepší prostředí pro uživatele.

### <a name="responding-to-updates-from-the-locationmanager"></a>Neodpovídá na požadavky aktualizace z LocationManager

Jakmile aplikace vyžaduje aktualizace z `LocationManager`, pak může přijímat informace ze služby implementací [ `ILocationListener` ](https://developer.xamarin.com/api/type/Android.Locations.ILocationListener/) rozhraní. Toto rozhraní obsahuje čtyři metody pro naslouchání umístění služby a poskytovateli umístění `OnLocationChanged`. Systém bude volat `OnLocationChanged` až se změní umístění uživatele dost pro kvalifikaci jako umístění změnit podle kritérií zadaných žádosti o umístění aktualizace. 

Následující kód ukazuje metody v `ILocationListener` rozhraní:

```csharp
public class MainActivity : AppCompatActivity, ILocationListener
{
    TextView latitude;
    TextView longitude;
    
    public void OnLocationChanged (Location location)
    {
        // called when the location has been updated.
    }
    
    public OnProviderDisabled(string locationProvider)
    {
        // called when the user disables the provider
    }
    
    public OnProviderEnabled(string locationProvider)
    {
        // called when the user enables the provider
    }
    
    public OnStatusChanged(string locationProvider, Availability status, Bundle extras)
    {
        // called when the status of the provider changes (there are a variety of reasons for this)
    }
}
```

### <a name="unsubscribing-to-locationmanager-updates"></a>Odhlášení musí být LocationManager aktualizací

Chcete-li ušetřit prostředky systému, aplikace by měl co nejdříve odhlásit umístění aktualizací. `RemoveUpdates` Metoda informuje `LocationManager` zastavit odesílání aktualizací na naší aplikace.  Jako příklad může volat aktivitu `RemoveUpdates` v `OnPause` metoda tak, aby jsme dokážou krátkodobě, pokud aplikace potřebuje umístění aktualizace při jeho aktivity není na obrazovce:

```csharp
protected override void OnPause ()
{
    base.OnPause ();
    locationManager.RemoveUpdates (this);
}
```

Pokud aplikace potřebuje k získání aktualizací umístění v pozadí, budete chtít vytvořit vlastní služby, která si předplatí systému služby umístění. Odkazovat [Backgrounding Android službou](~/android/app-fundamentals/services/index.md) Průvodce pro další informace.

### <a name="determining-the-best-location-provider-for-the-locationmanager"></a>Určení nejlepší umístění poskytovatele LocationManager

Výše uvedené aplikace nastaví GPS jako umístění poskytovatele. Však GPS nemusí být k dispozici ve všech případech, například pokud je zařízení ve vnitřním uzavřeném prostoru nebo nemá GPS příjemce. Pokud je to tento případ, výsledkem je, `null` návratový pro zprostředkovatele.

Vaše aplikace fungovat, když není k dispozici GPS získáte pomocí `GetBestProvider` metoda požádat o poskytovateli nejlepší umístění k dispozici (zařízení podporované a uživatel povolen) při spuštění aplikace. Místo předávání konkrétního poskytovatele, se dá zjistit `GetBestProvider` požadavky pro zprostředkovatele – například přesnost a power - s [ `Criteria` objekt](https://developer.xamarin.com/api/type/Android.Locations.Criteria/). `GetBestProvider` Vrátí osvědčené zprostředkovatele pro daná kritéria.

Následující kód ukazuje, jak získat nejlépe dostupného poskytovatele a použít ho žádosti o umístění aktualizace:

```csharp
Criteria locationCriteria = new Criteria();   
locationCriteria.Accuracy = Accuracy.Coarse;
locationCriteria.PowerRequirement = Power.Medium;

locationProvider = locationManager.GetBestProvider(locationCriteria, true);

if(locationProvider != null)
{
    locationManager.RequestLocationUpdates (locationProvider, 2000, 1, this);
}
else
{
    Log.Info(tag, "No location providers available");
}
```

> [!NOTE]
>  Pokud je uživatel vypnul všechny umístění poskytovatele `GetBestProvider` vrátí `null`. Pokud chcete zobrazit, jak tento kód funguje na skutečné zařízení, je nutné povolit GPS, Wi-Fi a mobilní sítě v části **nastavení Google > umístění > režimu** jak je vidět na tomto snímku obrazovky:

[![Obrazovka nastavení režimu umístění s telefonem s Androidem](location-images/location-02.png)](location-images/location-02.png#lightbox)

Následující snímek obrazovky ukazuje umístění aplikace spuštěná pomocí `GetBestProvider`:

[![Aplikace GetBestProvider zobrazení, šířky a zprostředkovatele](location-images/location-03.png)](location-images/location-03.png#lightbox)

Mějte na paměti, `GetBestProvider` nezmění zprostředkovatele dynamicky. Místo toho určuje nejlépe dostupného poskytovatele jednou během životního cyklu aktivity. Pokud po byla nastavena, změní se stav poskytovatele, aplikace bude vyžadovat další kód v `ILocationListener` metody &ndash; `OnProviderEnabled`, `OnProviderDisabled`, a `OnStatusChanged` &ndash; pro každou možnost týkající se zpracování přepínač zprostředkovatele.

## <a name="summary"></a>Souhrn

Tato příručka zahrnutých získání umístění uživatele pomocí Android služby umístění a roztaveného umístění poskytovatele z rozhraní API služby Google umístění.


## <a name="related-links"></a>Související odkazy

- [Umístění (ukázka)](https://developer.xamarin.com/samples/Location/)
- [FusedLocationProvider (ukázka)](https://developer.xamarin.com/samples/FusedLocationProvider/)
- [Služeb Google Play.](http://developer.android.com/google/play-services/index.html)
- [Kritéria – třída](https://developer.xamarin.com/api/type/Android.Locations.Criteria/)
- [LocationManager – třída](https://developer.xamarin.com/api/type/Android.Locations.LocationManager/)
- [LocationListener – třída](https://developer.xamarin.com/api/type/Android.Locations.ILocationListener/)
- [LocationClient rozhraní API](http://developer.android.com/reference/com/google/android/gms/location/LocationClient.html)
- [LocationListener rozhraní API](http://developer.android.com/reference/com/google/android/gms/location/LocationListener.html)
- [LocationRequest rozhraní API](https://developer.android.com/reference/com/google/android/gms/location/LocationRequest.html)
