---
title: Návod - umístění pozadí v Xamarin.iOS
description: Tento dokument poskytuje návod, jak používat informace o umístění backgrounded aplikace pro Xamarin.iOS. Popisuje potřebné instalační uživatelské rozhraní a stavy aplikace.
ms.prod: xamarin
ms.assetid: F8EEA0FD-5614-47FE-ADAC-80A5BCA6EB5F
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/18/2017
ms.openlocfilehash: aef39ef435bbbad6f643b2376832d8f8132d6a4c
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 06/05/2018
ms.locfileid: "34784091"
---
# <a name="walkthrough---background-location-in-xamarinios"></a>Návod - umístění pozadí v Xamarin.iOS

V tomto příkladu přidáme sestavit iOS umístění aplikaci, která se zobrazí informace o našich aktuální umístění: zeměpisné šířky, zeměpisnou délku a dalších parametrů na obrazovku. Tato aplikace bude ukazují, jak správně provádět aktualizace umístění, zatímco aplikace je aktivní nebo Backgrounded.

Tento návod popisuje některé klíč backgrounding koncepty, včetně registrace aplikace jako pozadí nezbytné aplikace, pozastavení aktualizace uživatelského rozhraní, když je aplikace backgrounded a práce `WillEnterBackground` a `WillEnterForeground` `AppDelegate` metody .

## <a name="application-set-up"></a>Nastavení aplikace


1. Nejprve vytvořte novou **iOS > aplikace > jediné zobrazení aplikace (C#)**. Volání _umístění_ a ujistěte se, že byly vybrány iPad a iPhone.

1. Umístění aplikace se považují za pozadí potřeby aplikace v iOS. Zaregistrovat aplikaci jako umístění aplikace úpravou **Info.plist** soubor projektu.

    V Průzkumníku řešení klikněte dvakrát klikněte na **Info.plist** soubor otevřít, a přejděte do dolní části seznamu. Zaškrtněte oběma **povolit režimy pozadí** a **aktualizace umístění** zaškrtávací políčka.

    V sadě Visual Studio pro Mac bude vypadat přibližně takto:

    [![](location-walkthrough-images/image7.png "Toto políčko zaškrtněte režimy pozadí povolit i umístění aktualizace zaškrtávací políčka")](location-walkthrough-images/image7.png#lightbox)

    V sadě Visual Studio **Info.plist** je nutné ručně aktualizovat přidáním následující dvojice klíč/hodnota:

    ```xml
    <key>UIBackgroundModes</key>
    <array>
      <string>location</string>
    </array>
    ```

1. Teď, když je zaregistrován aplikace, ho můžete získat data o umístění ze zařízení. V iOS `CLLocationManager` třída se používá pro přístup k informace o umístění a může vygenerovat události, které poskytují aktualizace umístění.

1. V kódu, vytvořte novou třídu s názvem `LocationManager` poskytuje jednotné místo pro různé obrazovky a kód přihlásit k odběru aktualizací umístění. V `LocationManager` třídy, ujistěte se o instanci `CLLocationManager` názvem `LocMgr`:

    ```csharp
    public class LocationManager
    {
        protected CLLocationManager locMgr;

        public LocationManager () {
            this.locMgr = new CLLocationManager();
            this.locMgr.PausesLocationUpdatesAutomatically = false;

            // iOS 8 has additional permissions requirements
            if (UIDevice.CurrentDevice.CheckSystemVersion (8, 0)) {
                locMgr.RequestAlwaysAuthorization (); // works in background
                //locMgr.RequestWhenInUseAuthorization (); // only in foreground
            }

            if (UIDevice.CurrentDevice.CheckSystemVersion (9, 0)) {
                locMgr.AllowsBackgroundLocationUpdates = true;
            }
        }

        public CLLocationManager LocMgr {
            get { return this.locMgr; }
        }
    }
    ```

    Výše uvedený kód nastaví počet vlastností a oprávnění [CLLocationManager](https://developer.xamarin.com/api/type/CoreLocation.CLLocationManager/) třídy:

    - `PausesLocationUpdatesAutomatically` – To je logická hodnota, která můžete nastavit v závislosti na tom, jestli je dovoleno systému pozastavit aktualizace umístění. Na některých zařízeních je standardně `true`, což může způsobit zařízení zastavit získávání pozadí aktualizace umístění přibližně za 15 minut.
    - `RequestAlwaysAuthorization` -By měla předávat tato metoda poskytuje aplikaci uživateli možnost Povolit umístění přístup na pozadí. `RequestWhenInUseAuthorization` Můžete také předat Pokud chcete uživateli přidělit možnost Povolit umístění přístup jenom v případě, že aplikace je v popředí.
    - `AllowsBackgroundLocationUpdates` – To je vlastnost typu Boolean, zavedená v iOS 9, která může být nastaven na povolit aplikaci dostávat aktualizace umístění, pokud byla pozastavena.

    > [!IMPORTANT]
    > iOS 8 (a vyšší) taky vyžaduje položku v **Info.plist** souboru se uživateli zobrazí jako součást požadavek ověřování.

1. Přidá klíč `NSLocationAlwaysUsageDescription` nebo `NSLocationWhenInUseUsageDescription` řetězcem, který se zobrazí se uživateli na výstrahy, které vyžadují přístup k datům umístění.

1. iOS 9 vyžaduje, aby při použití `AllowsBackgroundLocationUpdates` **Info.plist** zahrnuje klíč `UIBackgroundModes` s hodnotou `location`. Pokud jste dokončili krok 2 tohoto návodu, to by měl již byla v souboru Info.plist.


1. Uvnitř `LocationManager` třídy, vytvořte metodu s názvem `StartLocationUpdates` následujícím kódem. Tento kód ukazuje na začít přijímat aktualizace umístění z `CLLocationManager`:

    ```csharp
    if (CLLocationManager.LocationServicesEnabled) {
        //set the desired accuracy, in meters
        LocMgr.DesiredAccuracy = 1;
        LocMgr.LocationsUpdated += (object sender, CLLocationsUpdatedEventArgs e) =>
        {
            // fire our custom Location Updated event
            LocationUpdated (this, new LocationUpdatedEventArgs (e.Locations [e.Locations.Length - 1]));
        };
        LocMgr.StartUpdatingLocation();
    }
    ```

    Existuje několik důležitých věcí děje u této metody. Nejprve můžeme provést kontrola ověří, zda má aplikace přístup k umístění dat na zařízení. Jsme to ověřit pomocí volání `LocationServicesEnabled` na `CLLocationManager`. Tato metoda vrátí **false** Pokud uživatel má odepřené aplikaci přístup k informace o umístění.

1. Dále určete do správce k umístění, jak často k aktualizaci. `CLLocationManager` poskytuje mnoho možností pro filtrování a konfigurace data o umístění, včetně četnosti aktualizací. V tomto příkladu nastavený `DesiredAccuracy` aktualizovat vždy, když se změní umístění měřidla. Další informace o konfiguraci frekvence aktualizace umístění a další předvolby, najdete v části [referenci třídy CLLocationManager](http://developer.apple.com/library/ios/#documentation/CoreLocation/Reference/CLLocationManager_Class/CLLocationManager/CLLocationManager.html) v dokumentaci Apple.

1. Nakonec volání `StartUpdatingLocation` na `CLLocationManager` instance. Tato hodnota informuje správce umístění, abyste získali počáteční opravu na aktuální umístění a zahájit odesílání aktualizací

Pokud správce umístění má byly vytvořeny, konfigurovány s typů dat chceme přijímat, a bylo zjištěno počáteční umístění. Teď je potřeba kód k vykreslení musí data o umístění do uživatelského rozhraní. Jsme to lze provést pomocí vlastní události, která přijímá `CLLocation` jako argument:

```csharp
// event for the location changing
public event EventHandler<LocationUpdatedEventArgs>LocationUpdated = delegate { };
```

V dalším kroku se přihlásit k odběru aktualizací umístění z `CLLocationManager`a vygenerovat vlastní `LocationUpdated` událost v případě, že nová umístění data k dispozici, předávání v umístění jako argument. Chcete-li to provést, vytvořte novou třídu **LocationUpdateEventArgs.cs**. Tento kód je přístupný v hlavní aplikaci a vrátí umístění zařízení, pokud je vyvolána událost:

```csharp
public class LocationUpdatedEventArgs : EventArgs
{
    CLLocation location;

    public LocationUpdatedEventArgs(CLLocation location)
    {
       this.location = location;
    }

    public CLLocation Location
    {
       get { return location; }
    }
}
```

## <a name="user-interface"></a>Uživatelské rozhraní

1. Návrhář iOS použijte k vytvoření obrazovky, který se zobrazí informace o umístění. Dvakrát klikněte na **Main.storyboard** soubor k zahájení.

    Ve scénáři přetáhněte několik popisky na obrazovce tak, aby fungoval jako zástupné symboly pro informace o umístění. V tomto příkladu jsou popisky pro, šířky, výšky, kurzu a rychlost.

    Rozložení by měl vypadat takto:

    ![](location-walkthrough-images/image8.png "Příklad rozvržení uživatelského rozhraní v iOS návrháře")

1. V panelu pro řešení, dvakrát klikněte na `ViewController.cs` soubor a upravit ho vytvořit novou instanci třídy LocationManager a volání `StartLocationUpdates`na něm.
  Změňte kód, který vypadat následovně:

    ```csharp
    #region Computed Properties
    public static bool UserInterfaceIdiomIsPhone {
                get { return UIDevice.CurrentDevice.UserInterfaceIdiom == UIUserInterfaceIdiom.Phone; }
            }

    public static LocationManager Manager { get; set;}
    #endregion

    #region Constructors
    public ViewController (IntPtr handle) : base (handle)
    {
    // As soon as the app is done launching, begin generating location updates in the location manager
        Manager = new LocationManager();
        Manager.StartLocationUpdates();
    }

    #endregion
    ```

    Aktualizace umístění tím se spustí při spuštění aplikace, i když se nezobrazí žádná data.

1. Teď, když se aktualizace umístění přijme, aktualizujte na obrazovce s informace o umístění. Následující metoda získá umístění z našich `LocationUpdated` událostí a zobrazí v uživatelském rozhraní:

    ```csharp
    #region Public Methods
    public void HandleLocationChanged (object sender, LocationUpdatedEventArgs e)
    {
        // Handle foreground updates
        CLLocation location = e.Location;

        LblAltitude.Text = location.Altitude + " meters";
        LblLongitude.Text = location.Coordinate.Longitude.ToString ();
        LblLatitude.Text = location.Coordinate.Latitude.ToString ();
        LblCourse.Text = location.Course.ToString ();
        LblSpeed.Text = location.Speed.ToString ();

        Console.WriteLine ("foreground updated");
    }
    #endregion
    ```

Musíme přihlášení k odběru `LocationUpdated` událost v našich AppDelegate a volání nové metody aktualizace uživatelského rozhraní. Přidejte následující kód v `ViewDidLoad,` hned po `StartLocationUpdates` volání:

```csharp
public override void ViewDidLoad ()
{
    base.ViewDidLoad ();

    // It is better to handle this with notifications, so that the UI updates
    // resume when the application re-enters the foreground!
    Manager.LocationUpdated += HandleLocationChanged;

}
```


Teď když se aplikace spustí, by měla vypadat nějak takto:

[![](location-walkthrough-images/image5.png "Příklad aplikace spustit")](location-walkthrough-images/image5.png#lightbox)

## <a name="handling-active-and-background-states"></a>Zpracování stavů aktivní a pozadí

1. Aplikace je výstup vytvořeného aktualizace umístění, i když je aktivní a aktivní. K předvedení, co se stane, když aplikace přejde na pozadí, přepsat `AppDelegate` metody, které sledují aplikace změny stavu, takže aplikace se zapíše do konzoly při přechází mezi popředí a na pozadí:

    ```csharp
    public override void DidEnterBackground (UIApplication application)
    {
        Console.WriteLine ("App entering background state.");
    }

    public override void WillEnterForeground (UIApplication application)
    {
        Console.WriteLine ("App will enter foreground");
    }
    ```

    Přidejte následující kód v `LocationManager` nepřetržitě tisku aktualizované umístění data na výstupní aplikace, pro ověření informací o umístění je stále k dispozici na pozadí:

    ```csharp
    public class LocationManager
    {
        public LocationManager ()
        {
        ...
        LocationUpdated += PrintLocation;
        }
        ...

        //This will keep going in the background and the foreground
        public void PrintLocation (object sender, LocationUpdatedEventArgs e) {
        CLLocation location = e.Location;
        Console.WriteLine ("Altitude: " + location.Altitude + " meters");
        Console.WriteLine ("Longitude: " + location.Coordinate.Longitude);
        Console.WriteLine ("Latitude: " + location.Coordinate.Latitude);
        Console.WriteLine ("Course: " + location.Course);
        Console.WriteLine ("Speed: " + location.Speed);
        }
    }
    ```

1. Existuje jedna zbývající problém s kódem: Pokus o aktualizaci uživatelského rozhraní, když je aplikace backgrounded bude příčina iOS bude ho ukončit. Pokud aplikace přejde do na pozadí, musí kód odhlásit z umístění aktualizace a přestanou aktualizovat uživatelského rozhraní.

    iOS poskytuje nám oznámení, pokud je aplikace o přecházení mezi jinou aplikaci stavy. V takovém případě jsme mohou přihlásit k odběru `ObserveDidEnterBackground` oznámení.

    Následující fragment kódu ukazuje, jak pokud chcete, aby zobrazení vědět, kdy zastavit aktualizace uživatelského rozhraní použijte oznámení. To bude přejděte `ViewDidLoad`:

    ```csharp
    UIApplication.Notifications.ObserveDidEnterBackground ((sender, args) => {
        Manager.LocationUpdated -= HandleLocationChanged;
    });
    ```

    Když aplikace běží, výstup bude vypadat přibližně takto:

    ![](location-walkthrough-images/image6.png "Příklad výstupu umístění v konzole")

1. Aplikace vytiskne umístění aktualizace obrazovky při fungování v popředí a pokračuje k vytištění data do okna výstupu aplikace při fungování v pozadí.

Pouze jeden nezpracovaných problém zůstane: na obrazovce spustí aktualizace uživatelského rozhraní při prvním načtení aplikace, ale nemá žádný způsob, jak zjistit, když aplikace znovu přešla popředí. Je-li aplikace backgrounded zpět do popředí, uživatelského rozhraní aktualizace nebude pokračovat.

Chcete-li odstranit tento problém, vnořit volání spuštění uživatelského rozhraní aktualizace uvnitř jiné oznámení, které bude platit, jakmile se zobrazí aktivním stavu:

```csharp
UIApplication.Notifications.ObserveDidBecomeActive ((sender, args) => {
  Manager.LocationUpdated += HandleLocationChanged;
});
```

Uživatelské rozhraní teď bude zahájena aktualizace při prvním spuštění aplikace a obnovit, aplikace se aktualizuje vždy, když zpátky do popředí.

V tomto návodu jsme vytvořili aplikaci dobře behaved, pozadí podporující iOS, která zobrazí data o umístění na obrazovce a ve výstupním okně aplikace.


## <a name="related-links"></a>Související odkazy

- [Umístění (část 4) (ukázka)](https://developer.xamarin.com/samples/monotouch/Location/)
- [Základní umístění Framework – referenční informace](https://developer.apple.com/library/ios/documentation/CoreLocation/Reference/CoreLocation_Framework/_index.html)
