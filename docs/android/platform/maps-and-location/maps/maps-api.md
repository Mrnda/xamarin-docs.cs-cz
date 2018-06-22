---
title: Rozhraní API map
ms.prod: xamarin
ms.assetid: C0589878-2D04-180E-A5B9-BB41D5AF6E02
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/16/2018
ms.openlocfilehash: fc16178a4068b2dcf22fc19047e0ef403e83633f
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/04/2018
ms.locfileid: "30773521"
---
# <a name="maps-api"></a>Rozhraní API map

Pomocí aplikace mapy je skvělé, ale někdy, do které chcete zahrnout mapy přímo v aplikaci. Kromě integrovaných mapuje aplikace, Google, poskytuje taky [nativní mapování rozhraní API pro Android](https://developers.google.com/maps/documentation/android/).
Rozhraní API map je vhodná pro případy, kdy chcete zachovat větší kontrolu nad prostředí mapování. Věcí, které je možné pomocí rozhraní API map, patří:

-  Změna prostřednictvím kódu programu hlediska mapy.
-  Přidání a přizpůsobení značek.
-  Zadávání poznámek k mapa s překryvy.

Na rozdíl od zastaralý Google Maps Android API v1, v2 Google Maps Android API je součástí [služby Google Play](http://developer.android.com/google/play-services/index.html).
Proto je potřeba splnit některé povinné požadavky, než bude možné použít rozhraní API systému Android mapy Google v aplikaci Xamarin.Android.


## <a name="google-maps-api-prerequisites"></a>Mapuje požadavky rozhraní API Google

Několik položek je potřeba nakonfigurovat před použitím rozhraní API map, včetně:

-  Nainstalujte sadu SDK služby Google Play
-  Vytvoření emulátor pomocí rozhraní API Google
-  Získat klíč rozhraní API map
-  Zadejte požadovaná oprávnění



### <a name="install-the-google-play-services-sdk"></a>Nainstalujte sadu SDK služby Google Play

Google Play Services je technologie z Google, která umožňuje využít výhod různých Google funkcí, jako je například Google +, v aplikaci fakturace a mapy Android aplikací. Tyto funkce jsou dostupné na zařízeních s Androidem jako pozadí služby, které jsou součástí [Google Play Services APK](https://play.google.com/store/apps/details?id=com.google.android.gms&hl=en).

Aplikace pro Android komunikovat s služby Google Play pomocí klientské knihovny služby Google Play. Tato knihovna obsahuje rozhraní a třídy pro jednotlivé služby, jako je například mapy. Následující diagram znázorňuje vztah mezi aplikace pro Android a Google Play Services:

![Diagram ilustrující Google Play Storu aktualizace APK služby Google Play](maps-api-images/play-services-diagram.png)

Rozhraní API systému Android mapy je k dispozici jako součást služby Google Play.
Aplikace pro Xamarin.Android mohli používat rozhraní API map, musí být nainstalovaná a vázán Google Play Services SDK. Google Play Services SDK je nainstalován pomocí nástroje Android SDK Manager. Následující snímek obrazovky ukazuje, kde v Android SDK Manager, klient služby Google Play najdete:

![Služby Google Play se zobrazí v části funkce v Android SDK Manager](maps-api-images/image01.png)

> [!NOTE]
> Služby Google Play APK je licencovaného produktu, které nemusí být k dispozici na všech zařízeních. Pokud není nainstalována, službu mapy Google nebude fungovat v zařízení.


#### <a name="binding-google-play-services"></a>Vazba webu Google Play Services

Po instalaci služby Google Play klientské knihovny, musí být vázána knihovnu vazby Xamarin.Android Java. Chcete-li to provést dvěma způsoby:

-  **Použijte balíček NuGet mapy Google Play Services** -Toto je nejjednodušší přístup (k dispozici pouze v Xamarin.Android 4.8 nebo vyšší).
   Nainstalujte [Xamarin Google Play Services mapy NuGet](https://www.nuget.org/packages/Xamarin.GooglePlayServices.Maps); také to nainstaluje všechny balíčky závislostí služby Google Play.
   Zbytek tohoto průvodce se zaměřuje na tento přístup.

-  **Ručně vytvořit vazbu klientské knihovny služby Google Play** – to je složitější přístup a je jedinou možností pro Xamarin.Android 4.4 nebo Xamarin.Android 4.6, pro vazbu webu Google Play Services SDK.
   Ručně vazby služby Google Play klientské knihovny je nad rámec tohoto dokumentu, ale najdete příklad toho, jak to udělat v [Maps a umístění ukázku v3 ukázka](https://github.com/xamarin/monodroid-samples/tree/master/MapsAndLocationDemo_v3) na Githubu.


#### <a name="adding-the-google-play-services-map-package"></a>Probíhá přidávání balíčku mapy služby Google Play

Chcete-li přidat balíček Google Play Services mapy, klikněte pravým tlačítkem **odkazy** složku projekt v Průzkumníku řešení a klikněte na tlačítko **spravovat balíčky NuGet...** :

![Zobrazuje Průzkumníka řešení položky kontextové nabídky spravovat balíčky NuGet v rámci odkazy](maps-api-images/image02.png)

Tím se otevře **Správce balíčků NuGet**. Klikněte na tlačítko **Procházet** a zadejte **Xamarin Google Play Services Maps** do pole hledání. Vyberte **Xamarin.GooglePlayServices.Maps** a klikněte na tlačítko **nainstalovat**. (Pokud tento balíček měl dřív nainstalované, klikněte na tlačítko **aktualizace**.):

[![Správce balíčků NuGet s balíčkem Xamarin.GooglePlayServices.Maps vybrané](maps-api-images/image03-sml.png)](maps-api-images/image03.png#lightbox)

Všimněte si, že jsou nainstalovány také následující balíčky závislost:

-   **Xamarin.GooglePlayServices.Base**
-   **Xamarin.GooglePlayServices.Basement**
-   **Xamarin.GooglePlayServices.Tasks**



### <a name="create-an-emulator-with-google-apis"></a>Vytvoření emulátor pomocí rozhraní API Google

I když to nedoporučujeme, je možné nastavit emulátor pro podporu rozhraní API systému Android mapy. Emulátor serveru musí být nakonfigurována na cílové 17 úrovní rozhraní API Google (Android 4.2.2) nebo vyšší. Na následujícím snímku obrazovky je obrázek emulátoru nakonfigurované pro rozhraní API úrovně 19: 

![Správce emulátoru Android s AVD nakonfigurované pro rozhraní API úrovně 19](maps-api-images/image04.png)



### <a name="obtain-a-google-maps-api-key"></a>Získat klíč Google Maps API

Posledním krokem je získat klíč Google Maps API (Všimněte si, že nelze znovu použít klíč rozhraní API ze starší verze v1 Google Maps). Informace o tom, jak získat a použít klíč rozhraní API s Xamarin.Android najdete v tématu [získání A Google klíč rozhraní API map](~/android/platform/maps-and-location/maps/obtaining-a-google-maps-api-key.md).
 


### <a name="specify-the-required-permissions"></a>Zadejte požadovaná oprávnění

Je třeba zadat následující oprávnění v **AndroidManifest.XML** pro rozhraní API systému Android mapy Google:

-  **Přístup k síti stav** &ndash; rozhraní API map musí být schopna provést kontrolu, pokud ho můžete stáhnout dlaždice map.

-  **Přístup k Internetu** &ndash; přístup k Internetu je nutné stáhnout dlaždice map a komunikují se servery Google Play pro přístup k rozhraní API.

-  **OpenGL ES v2** &ndash; aplikace, musí deklarovat požadavek OpenGL ES v2.

-  **Klíč rozhraní API Google Maps** &ndash; klíč rozhraní API slouží k potvrzení, že aplikace je zaregistrován a oprávnění k používání služby Google Play. V tématu [získání klíč Google Maps API](~/android/platform/maps-and-location/maps/obtaining-a-google-maps-api-key.md) podrobnosti o tomto klíči.

-  **Zápis do externího úložiště** &ndash; rozhraní Android API Google Maps bude ukládat do mezipaměti stažené dlaždice na externí úložiště.

-  **Přístup ke službám založené na webu Google** &ndash; aplikace potřebuje oprávnění pro přístup k webové služby Google, které zpět rozhraní API systému Android mapy.

-  **Oprávnění pro Google Play Services oznámení** &ndash; aplikace musí udělit oprávnění ke vzdálené oznámení dostávat, služby Google Play.

-  **Přístup k umístění zprostředkovatelé** &ndash; jedná se o volitelné oprávnění.
   Bude umožňují `GoogleMap` třída pro zobrazení umístění zařízení na mapě.


Následující fragment kódu je příkladem nastavení, která musí být přidaný do **AndroidManifest.XML**:

```xml
<?xml version="1.0" encoding="utf-8"?>
<manifest xmlns:android="http://schemas.android.com/apk/res/android" android:versionName="4.5" package="com.xamarin.docs.android.mapsandlocationdemo2" android:versionCode="6">
    <uses-sdk android:minSdkVersion="14" android:targetSdkVersion="17" />

    <!-- Google Maps for Android v2 requires OpenGL ES v2 -->
    <uses-feature android:glEsVersion="0x00020000" android:required="true" />

    <!-- We need to be able to download map tiles and access Google Play Services-->
    <uses-permission android:name="android.permission.INTERNET" />

    <!-- Allow the application to access Google web-based services. -->
    <uses-permission android:name="com.google.android.providers.gsf.permission.READ_GSERVICES" />

    <!-- Google Maps for Android v2 will cache map tiles on external storage -->
    <uses-permission android:name="android.permission.WRITE_EXTERNAL_STORAGE" />

    <!-- Google Maps for Android v2 needs this permission so that it may check the connection state as it must download data -->
    <uses-permission android:name="android.permission.ACCESS_NETWORK_STATE" />

    <!-- Permission to receive remote notifications from Google Play Services -->
    <!-- Notice here that we have the package name of our application as a prefix on the permissions. -->
    <uses-permission android:name="<PACKAGE NAME>.permission.MAPS_RECEIVE" />
    <permission android:name="<PACKAGE NAME>.permission.MAPS_RECEIVE" android:protectionLevel="signature" />

    <!-- These are optional, but recommended. They will allow Maps to use the My Location provider. -->
    <uses-permission android:name="android.permission.ACCESS_COARSE_LOCATION" />
    <uses-permission android:name="android.permission.ACCESS_FINE_LOCATION" />


    <application android:label="@string/app_name">
        <!-- Put your Google Maps V2 API Key here. -->
        <meta-data android:name="com.google.android.maps.v2.API_KEY" android:value="YOUR_API_KEY" />
        <meta-data android:name="com.google.android.gms.version" android:value="@integer/google_play_services_version" />
    </application>
</manifest>
```


## <a name="the-googlemap-class"></a>GoogleMap – třída

Jakmile požadavky bylo postaráno, je čas začít vyvíjet aplikace a používat rozhraní API systému Android mapy. [GoogleMap](https://developers.google.com/maps/documentation/android/reference/com/google/android/gms/maps/GoogleMap) třída je hlavním rozhraní API, které aplikace pro Xamarin.Android se použije k zobrazení a interakci s Google Maps pro Android. Tato třída obsahuje následující zodpovědnosti:

-  Interakci s služby Google Play, abyste autorizovat aplikaci s webovou službou Google.

-  Stahování a ukládání do mezipaměti a zobrazení dlaždice map.

-  Zobrazení ovládacích prvků uživatelského rozhraní, jako posouvání a přiblížení uživateli.

-  Kreslení značek a geometrické obrazce v maps.

`GoogleMap` Je přidána do aktivity v jedné ze dvou způsobů:

-  **MapFragment** – [MapFragment](http://developer.android.com/reference/com/google/android/gms/maps/MapFragment.html) je specializované Fragment, který funguje jako hostitel pro `GoogleMap` objektu. `MapFragment` Vyžaduje úroveň rozhraní API systému Android 12 nebo vyšší.
   Můžete použít starší verze Android [SupportMapFragment](http://developer.android.com/reference/com/google/android/gms/maps/SupportMapFragment.html).

-  **MapView** – [MapView](https://developer.xamarin.com/api/type/Android.GoogleMaps.MapView/) je podtřídou specializovaná zobrazení, které může fungovat jako hostitel pro `GoogleMap` objektu. Uživatelé této třídy musí předávání všech metod životního cyklu aktivity, která `MapView` třídy.

Každý z těchto kontejnerů vystavit `Map` vlastnost, která vrací instanci třídy `GoogleMap`. Měli dává [MapFragment](http://developer.android.com/reference/com/google/android/gms/maps/MapFragment.html) třídy, protože je jednodušší rozhraní API, která snižuje velikost často používaný kód, který vývojář musí implementovat ručně.


### <a name="adding-a-mapfragment-to-an-activity"></a>Přidání MapFragment do aktivity

Na následujícím snímku obrazovky je příklad velmi jednoduchý `MapFragment`:

[![Snímek obrazovky zobrazení mapy fragment zařízení](maps-api-images/image05-sml.png)](maps-api-images/image05.png#lightbox)

Podobně jako ostatní třídy Fragment, existují dva způsoby, jak přidat to `MapFragment` do aktivity:

-   **Deklarativně** – `MapFragment` jde přidat prostřednictvím rozložení souboru XML pro aktivitu. Následující fragment kódu XML ukazuje příklad použití `fragment` element:

    ```xml
    <?xml version="1.0" encoding="utf-8"?>
    <fragment xmlns:android="http://schemas.android.com/apk/res/android"
              android:id="@+id/map"
              android:layout_width="match_parent"
              android:layout_height="match_parent"
              class="com.google.android.gms.maps.MapFragment" />
    ```

-   **Programově** – `MapFragment` lze přidat programově, jak je popsáno dále.

K přidávání prostřednictvím kódu programu `MapFragment`, musí implementovat vaši aktivitu `IOnMapReadyCallback` rozhraní. Protože inicializace `GoogleMap` objekt může trvat nějakou dobu dokončit, protože rozhraní API komunikuje se službou Google Play, je nutné zadat zpětné volání, které upozorní aplikace při `GoogleMap` je připraven.

Nejprve přidejte `IOnMapReadyCallback` k `Activity` deklarace třídy.
Příklad:

```csharp
public class MapWithMarkersActivity : Activity, IOnMapReadyCallback
```

Vedle `OnCreate` metoda, přidejte `MapFragment` jak je znázorněno v následujícím příkladu kódu ( `GoogleMapOptions` třídy se vysvětluje dále v této příručce):

```csharp
_mapFragment = FragmentManager.FindFragmentByTag("map") as MapFragment;
if (_mapFragment == null)
{
    GoogleMapOptions mapOptions = new GoogleMapOptions()
        .InvokeMapType(GoogleMap.MapTypeSatellite)
        .InvokeZoomControlsEnabled(false)
        .InvokeCompassEnabled(true);

    FragmentTransaction fragTx = FragmentManager.BeginTransaction();
    _mapFragment = MapFragment.NewInstance(mapOptions);
    fragTx.Add(Resource.Id.map, _mapFragment, "map");
    fragTx.Commit();
}
_mapFragment.GetMapAsync(this);
```

A `GoogleMap` musí být získány pomocí `GetMapAsync`, jak je znázorněno na konci v předchozím příkladu kódu &ndash; to automaticky inicializuje systému mapy a zobrazení. (Všimněte si, že tato metoda nepoužívá `await` / `async` sémantiku &ndash; `Async` chování je implementováno modulem Android.) Když `GoogleMap` objektu je připraven, Android volá vaší aplikace `OnMapReady` – metoda (který musí implementovat jako součást `IOnMapReadyCallback` rozhraní). Příklad:

```csharp
public void OnMapReady (GoogleMap map)
{
    _map = map;
}
```

Ve výše uvedeném příkladu kódu `OnMapReady` inicializuje zpětného volání `_map` proměnnou vytvořený `GoogleMap` objektu.

Jako příklad toho, jak používat tento výsledek když `OnResume` je volána, může zkontrolovat, zda `_map` hodnotu Null. Pokud `_map` je nastaven na `GoogleMap` objekt, `OnResume` můžete volat metody pro přidání značek a jeho fotoaparát přesunuty zadané zeměpisné šířky a délky. Úplný příklad, naleznete v tématu [SimpleMapDemo](https://github.com/xamarin/monodroid-samples/tree/master/MapsAndLocationDemo_v3/SimpleMapDemo).



### <a name="map-types"></a>Mapování typů

Nejsou k dispozici z rozhraní API Google mapuje pět různých typů map:

-  **Normální** – jedná se o výchozí typ mapy. Zobrazuje cest a důležitých přirozené funkcí společně s některé syntetická body zájmu (například budovy a mostů).

-  **Satelitní** – Tato mapa znázorňuje satelitní fotografie.

-  **Hybridní** – Tato mapa znázorňuje satelitní fotografie a silniční mapy.

-  **Geologické struktury** -především ukazuje topografické funkce s některé cest.

-  **Žádný** – Tato mapa nenačte všechny dlaždice, je vykreslen jako prázdnou mřížku.


Následující obrázek ukazuje tři různé typy map, z zleva doprava (normální hybridní, geologické struktury):

[![Tři mapování snímky obrazovky příklad: Normální, hybridního i geologické struktury](maps-api-images/map-types-sml.png)](maps-api-images/map-types.png#lightbox)

`GoogleMap.MapType` Vlastnost se používá k nastavení nebo změna, jaký typ mapy se zobrazí. Následující fragment kódu ukazuje, jak zobrazit mapu satelit.

```csharp
MapFragment mapFrag = (MapFragment) FragmentManager.FindFragmentById(Resource.Id.my_mapfragment_container);
mapFrag.GetMapAsync(this);
...
if (_map != null) {
    _map.MapType = GoogleMap.MapTypeSatellite;
}
```


### <a name="googlemap-properties"></a>Vlastnosti GoogleMap

`GoogleMap` definuje několik vlastností, které můžete ovládat funkce a vzhled mapy. Jeden ze způsobů, jak nakonfigurovat počáteční stav `GoogleMap` je předat [GoogleMapOptions](http://developer.android.com/reference/com/google/android/gms/maps/GoogleMapOptions.html) objektu při vytváření `MapFragment`. Následující fragment kódu je jedním z příkladů použití `GoogleMapOptions` objektu při vytváření `MapFragment`:

```csharp
GoogleMapOptions mapOptions = new GoogleMapOptions()
    .InvokeMapType(GoogleMap.MapTypeSatellite)
    .InvokeZoomControlsEnabled(false)
    .InvokeCompassEnabled(true);

FragmentTransaction fragTx = FragmentManager.BeginTransaction();
_mapFragment = MapFragment.NewInstance(mapOptions);
fragTx.Add(Resource.Id.map, _mapFragment, "map");
fragTx.Commit();
```

Další způsob konfigurace `GoogleMap` objektu je nastavením hodnoty [UiSettings](http://developer.android.com/reference/com/google/android/gms/maps/UiSettings.html) vlastnost objektu mapy. Další příklad ukazuje, jak nakonfigurovat `GoogleMap` zobrazíte ovládací prvky přiblížení a kompas:

```csharp
MapFragment mapFrag = (MapFragment) FragmentManager.FindFragmentById(Resource.Id.my_mapfragment_container);
mapFrag.GetMapAsync(this);
...
if (_map != null) {
    _map.UiSettings.ZoomControlsEnabled = true;
    _map.UiSettings.CompassEnabled = true;
}
```


## <a name="interacting-with-the-map"></a>Interakci s mapy

Rozhraní API systému Android mapy poskytuje rozhraní API umožňující aktivitu změnit hlediska, přidat značky, umístěte vlastní překryvy nebo kreslení geometrické obrazce. V této části se popisují postup provést některé z těchto úloh v Xamarin.Android.

### <a name="changing-the-viewpoint"></a>Změna hlediska

Mapy jsou modelována jako plochý roviny na obrazovce, v závislosti na projekci Mercator. Zobrazení mapy, je *fotoaparát* vypadající rovnou dolů na této umělé. Pozice kamery lze řídit změnou umístění, přiblížení, Náklon a vliv. [CameraUpdate](https://developers.google.com/maps/documentation/android/reference/com/google/android/gms/maps/CameraUpdate) třída se používá k přesunu umístění fotoaparát. `CameraUpdate` objekty nejsou přímo vytvořeny instance, místo toho poskytuje rozhraní API map [CameraUpdateFactory](http://developer.android.com/reference/com/google/android/gms/maps/CameraUpdateFactory.html) třídy.

Jednou `CameraUpdate` objekt byl vytvořen, jako parametr je předán do buď [GoogleMap.MoveCamera](https://developers.google.com/maps/documentation/android/reference/com/google/android/gms/maps/GoogleMap.html#moveCamera(com.google.maps.CameraUpdate)) nebo [GoogleMap.AnimateCamera](https://developers.google.com/maps/documentation/android/reference/com/google/android/gms/maps/GoogleMap.html#animateCamera(com.google.maps.CameraUpdate)) metody. `MoveCamera` Metoda aktualizace mapy okamžitě při `AnimateCamera` metoda poskytuje smooth, animovaný přechodu.

Tento fragment kódu je jednoduchý příklad použití `CameraUpdateFactory` k vytvoření `CameraUpdate` , se zvýší o jednu úroveň přiblížení mapy:

```csharp
MapFragment mapFrag = (MapFragment) FragmentManager.FindFragmentById(Resource.Id.my_mapfragment_container);
mapFrag.GetMapAsync(this);
...
if (_map != null) {
    _map.MoveCamera(CameraUpdateFactory.ZoomIn());
}
```

Poskytuje rozhraní API map [CameraPosition](http://developer.android.com/reference/com/google/android/gms/maps/model/CameraPosition.html) která bude agregovat všech možných hodnot pro pozice fotoaparát. Instance této třídy lze zadat do [CameraUpdateFactory.NewCameraPosition](https://developers.google.com/maps/documentation/android/reference/com/google/android/gms/maps/CameraUpdateFactory#newCameraPosition(com.google.android.gms.maps.model.CameraPosition)) metodu, která vrátí `CameraUpdate` objektu. Rozhraní API map také zahrnuje [CameraPosition.Builder](http://developer.android.com/reference/com/google/android/gms/maps/model/CameraPosition.Builder.html) třídu, která poskytuje rozhraní fluent API pro vytváření `CameraPosition` objekty.
Následující fragment kódu ukazuje příklad vytvoření `CameraUpdate` z `CameraPosition` a použití, chcete-li změnit na pozici fotoaparát `GoogleMap`:

```csharp
LatLng location = new LatLng(50.897778, 3.013333);
CameraPosition.Builder builder = CameraPosition.InvokeBuilder();
builder.Target(location);
builder.Zoom(18);
builder.Bearing(155);
builder.Tilt(65);
CameraPosition cameraPosition = builder.Build();
CameraUpdate cameraUpdate = CameraUpdateFactory.NewCameraPosition(cameraPosition);

MapFragment mapFrag = (MapFragment) FragmentManager.FindFragmentById(Resource.Id.my_mapfragment_container);
mapFrag.GetMapAsync(this);
...
if (_map != null) {
    _map.MoveCamera(cameraUpdate);
}
```

V předchozím fragmentu kódu je reprezentována určitého umístění na mapě [LatLng](https://developers.google.com/maps/documentation/android/reference/com/google/android/gms/maps/model/LatLng) třídy. Úroveň zvětšení je nastavena na 18. Vliv, který je po směru hodinových ručiček severní kompasu měření. Určuje náklon vlastnost ovládací prvky zobrazení úhel a je úhel 25 o od svislice. Následující snímek obrazovky ukazuje `GoogleMap` po provedení předchozí kód:

[![Příklad mapování Google zobrazující určitého umístění s nakloněné zobrazení úhlu](maps-api-images/image06-sml.png)](maps-api-images/image06.png#lightbox)


### <a name="drawing-on-the-map"></a>Kreslení na mapě

Rozhraní API systému Android mapy poskytuje rozhraní API pro kreslení na mapě následující položky:

-  **Značky** – jedná se o zvláštní ikony, které se používají k identifikaci jednoho místa na mapě.

-  **Překryvy** – to je obrázek, který můžete použít k identifikaci kolekci umístění nebo oblast na mapě.

-  **Řádky, mnohoúhelníky a kroužky** – jedná se rozhraní API umožňující aktivity a přidávání obrazců do mapy.


#### <a name="markers"></a>Značky

Poskytuje rozhraní API map [značky](https://developers.google.com/maps/documentation/android/reference/com/google/android/gms/maps/model/Marker) třídy, který zapouzdřuje všechna data o jedno umístění na mapě. Ve výchozím nastavení používají standardní ikonu poskytované Google Maps. Je možné přizpůsobit vzhled značku a reakce na kliknutí uživatele.


##### <a name="adding-a-marker"></a>Přidání značka

Chcete-li přidat značku k mapě, je nutné vytvořit nový [MarkerOptions](https://developers.google.com/android/reference/com/google/android/gms/maps/model/MarkerOptions) objektu a pak zavolají [AddMarker](http://developer.android.com/reference/com/google/android/gms/maps/GoogleMap.html#addMarker(com.google.android.gms.maps.model.MarkerOptions)) metodu `GoogleMap` instance. Tato metoda vrátí [značky](https://developers.google.com/maps/documentation/android/reference/com/google/android/gms/maps/model/Marker) objektu.

```csharp
MapFragment mapFrag = (MapFragment) FragmentManager.FindFragmentById(Resource.Id.my_mapfragment_container);
mapFrag.GetMapAsync(this);
...
if (_map != null) {
    MarkerOptions markerOpt1 = new MarkerOptions();
    markerOpt1.SetPosition(new LatLng(50.379444, 2.773611));
    markerOpt1.SetTitle("Vimy Ridge");
    _map.AddMarker(marker1);
}
```

Zobrazí se název značky v *informační okno* když uživatel klepnutím na značky. Následující snímek obrazovky ukazuje, jak vypadá této značky:

[![Příklad mapování Google s značku a okno s informací pro Vimy Ridge](maps-api-images/image07-sml.png)](maps-api-images/image07.png#lightbox)


##### <a name="customizing-a-marker"></a>Přizpůsobení značku

Je možné přizpůsobit na ikonu používané značky voláním `MarkerOptions.InvokeIcon` metoda při přidávání značky do mapy.
Tato metoda přebírá [BitmapDescriptor](http://developer.android.com/reference/com/google/android/gms/maps/model/BitmapDescriptor.html) objekt obsahující data potřebná k vykreslení ikonu. [BitmapDescriptorFactory](https://developer.android.com/reference/com/google/android/gms/maps/model/BitmapDescriptorFactory.html) třída poskytuje některé pomocné metody pro zjednodušení vytváření `BitmapDescriptor`. Následující seznam uvádí některé z těchto metod:

-   `DefaultMarker(float colour)` &ndash; Použít výchozí značky mapy Google, ale změnit barvy.

-   `FromAsset(string assetName)` &ndash; Použijte vlastní ikonou ze zadaného souboru ve složce prostředky.

-   `FromBitmap(Bitmap image)` &ndash; Zadaný rastrový obrázek použijte jako ikona.

-   `FromFile(string fileName` &ndash; Vytvořte vlastní ikonou ze souboru v zadané cestě.

-   `FromResource(int resourceId)` &ndash; Vytvořte vlastní ikonou z zadaný prostředek.

Následující fragment kódu ukazuje příklad vytvoření azurové barevném výchozí značky:

```csharp
mapFrag.GetMapAsync(this);
...
if (_map != null)
{
    MarkerOptions markerOpt1 = new MarkerOptions();
    markerOpt1.SetPosition(new LatLng(50.379444, 2.773611));
    markerOpt1.SetTitle("Vimy Ridge");
    markerOpt1.InvokeIcon(BitmapDescriptorFactory.DefaultMarker (BitmapDescriptorFactory.HueCyan));
    _map.AddMarker(marker1);
}
```


#### <a name="info-windows"></a>Informace o systému Windows

*Informace o windows* jsou speciální windows tento místní zobrazíte informace pro uživatele, když se klepnutím na konkrétní značku. Ve výchozím nastavení v okně informace se zobrazí obsah název značky. Název nebyl přiřazen, se zobrazí okno žádné informace. Pouze jeden informační okno se může zobrazit najednou.

Je možné přizpůsobit okno informace o implementaci [GoogleMap.IInfoWindowAdapter](https://developers.google.com/maps/documentation/android/reference/com/google/android/gms/maps/GoogleMap.InfoWindowAdapter) rozhraní. Na tomto rozhraní existují dvě důležité metody:

-  `public View GetInfoWindow(Marker marker)` &ndash; Tato metoda je volána potřebujete okno vlastní informace pro značku. Vrátí-li `null` , pak vykreslování oken ve výchozím nastavení se použije. Pokud tato metoda vrací zobrazení, bude tento zobrazení umístěn uvnitř rámce okna informace.

-  `public View GetInfoContents(Marker marker)` &ndash; Tato metoda bude volat pouze v případě GetInfoWindow vrátí `null` . Tato metoda může vrátit `null` hodnotu, pokud má být použita výchozí vykreslování obsah okna informace. Tuto metodu, jinak by měl vrátit zobrazení s obsah okně informace.

Okno s informací není za provozu zobrazení – místo toho bude Android převést zobrazení do statického rastrového obrázku a zobrazit, na bitovou kopii. To znamená, že informační okno nemůže reagovat na všechny události touch nebo gesta ani ji automaticky aktualizuje sám sebe. Chcete-li aktualizovat okno s informací, je nutné volat [GoogleMap.ShowInfoWindow](http://developer.android.com/reference/com/google/android/gms/maps/model/Marker.html#showInfoWindow()) metoda.

Následující obrázek ukazuje některé příklady některých vlastní informace o systému windows. Bitová kopie na levé straně, je jeho obsah přizpůsobili, bitovou kopii na pravé straně se okno jeho a přizpůsobit obsah:

![Příklad značky windows pro Melbourne, včetně ikonu a naplnění. Pravém má zaokrouhlené rozích.](maps-api-images/marker-infowindows.png)


#### <a name="ground-overlays"></a>Překryvy základů

Na rozdíl od značek, které identifikovat určitého umístění na mapě, [GroundOverlay](http://developer.android.com/reference/com/google/android/gms/maps/model/GroundOverlay.html) je obrázek, který se používá k identifikaci kolekci umístění nebo oblast na mapě.


##### <a name="adding-a-groundoverlay"></a>Přidání GroundOverlay

Přidání základů překrytí na mapu je velmi podobný jako při přidávání značku na mapu. První, [GroundOverlayOptions](http://developer.android.com/reference/com/google/android/gms/maps/model/GroundOverlayOptions.html) je vytvořen objekt. Tento objekt se potom předá jako parametr, který se `GoogleMap.AddGroundOverlay` metoda, která se vrátit `GroundOverlay` objektu. Tento fragment kódu je příklad přidávání základů překrytí na mapu:

```csharp
BitmapDescriptor image = BitmapDescriptorFactory.FromResource(Resource.Drawable.polarbear);
GroundOverlayOptions groundOverlayOptions = new GroundOverlayOptions()
    .Position(position, 150, 200)
    .InvokeImage(image);
GroundOverlay myOverlay = _map.AddGroundOverlay(groundOverlayOptions);
```

Následující snímek obrazovky ukazuje tento překrytí na mapě:

[![Příklad mapování s buňka image polárního opatřeny](maps-api-images/image09-sml.png)](maps-api-images/image09.png#lightbox)


#### <a name="lines-circles-and-polygons"></a>Řádky, kružnice a mnohoúhelníky

Existují tři typy jednoduché geometrickou obrázků, které mohou být přidány do mapy:

-  **Lomenou čáru** -Toto je řadu připojené úsečky. Dokáže označit cestu na mapě nebo vytvoří libovolný tvar vyžaduje.

-  **Mnohoúhelníku** -Toto je uzavřený obrazec pro označení oblasti na mapě.

-  **Kruh** – to bude nakreslit kruh na mapě.



##### <a name="polylines"></a>Čáru lomených

A [lomenou čáru](https://developers.google.com/maps/documentation/android/reference/com/google/android/gms/maps/model/Polyline) je seznam po sobě jdoucích `LatLng` objekty, které určují vrcholy každého řádku segmentu. Lomenou čáru vytvoření nejprve vytvoří `PolylineOptions` objekt a přidání body k němu. `PolylineOption` Je následně předán objekt `GoogleMap` objekt voláním `AddPolyline` metoda.

```csharp
PolylineOption rectOptions = new PolylineOption();
rectOptions.Add(new LatLng(37.35, -122.0));
rectOptions.Add(new LatLng(37.45, -122.0));
rectOptions.Add(new LatLng(37.45, -122.2));
rectOptions.Add(new LatLng(37.35, -122.2));
rectOptions.Add(new LatLng(37.35, -122.0)); // close the polyline - this makes a rectangle.
myMap.AddPolyline(rectOptions);
```


##### <a name="polygons"></a>Mnohoúhelníky

`Polygon`s jsou velmi podobné `Polyline`s, ale nejsou otevřené skončila. `Polygon`s jsou uzavřené smyčky a mít jejich interior vyplněna.
`Polygon`s jsou vytvořené v přesný stejným způsobem jako `Polyline`, s výjimkou [GoogleMap.AddPolygon](http://developer.android.com/reference/com/google/android/gms/maps/GoogleMap.html#addPolygon(com.google.android.gms.maps.model.PolygonOptions)) volaná metoda.

Na rozdíl od `Polyline`, `Polygon` samoobslužné ukončuje. Když `AddPolygon` je volána metoda bude automaticky zavřete vypnout mnohoúhelníku ve kreslení řádek, který připojí bodech první a poslední. Následující fragment kódu vytvoří plného obdélníku přes stejné oblasti jako v předchozím fragmentu kódu `Polyline` příklad.

```csharp
PolygonOptions rectOptions = new PolygonOptions();
rectOptions.Add(new LatLng(37.35, -122.0));
rectOptions.Add(new LatLng(37.45, -122.0));
rectOptions.Add(new LatLng(37.45, -122.2));
rectOptions.Add(new LatLng(37.35, -122.2));
// notice we don't need to close off the polygon
myMap.AddPolygon(rectOptions);
```


##### <a name="circles"></a>Kroužky

Kroužky jsou vytvořeny po prvním vytvoření instance [CircleOption](https://developers.google.com/maps/documentation/android/reference/com/google/android/gms/maps/model/CircleOptions) objekt, který specifikujete ve m centru a radius kruhu. Kruhu vykreslením na mapě voláním [GoogleMap.AddCircle](https://developers.google.com/maps/documentation/android/reference/com/google/android/gms/maps/GoogleMap#addCircle(com.google.android.gms.maps.model.CircleOptions)).
Následující fragment kódu ukazuje, jak k vykreslení kruh:

```csharp
CircleOptions circleOptions = new CircleOptions ();
circleOptions.InvokeCenter (new LatLng(37.4, -122.1));
circleOptions.InvokeRadius (1000);
_map.AddCircle (CircleOptions);
```


## <a name="responding-to-events"></a>Reagování na události

Existují tři typy interakcí, které uživatel může mít s mapou:

-  **Klikněte na značku** -uživatel klikne na značku.

-  **Přetáhněte značky** -uživatel dlouho klepne na mparger

-  **Klikněte na okno informace o** -uživatel klikne na informační okno.

Každá z těchto událostí bude možné podrobněji popsána níže.


### <a name="marker-click-events"></a>Značky události kliknutí

Když uživatel klikne na značku `MarkerClick` událost se vyvolá a `GoogleMap.MarkerClickEventArgs` předaná. Tato třída obsahuje dvě vlastnosti:

-  `GoogleMap.MarkerClickEventArgs.Handled` &ndash; Tato vlastnost by měla být nastavená na `true` indikující, že obslužné rutiny události spotřebovala události. Pokud je nastavena v `false` pak výchozí chování dojde kromě vlastní chování obslužné rutiny události.

-  `P0` &ndash; Tento parametr nedostatečně název je odkaz na značky, který vyvolal `MarkerClick` událostí.


Tento fragment kódu ukazuje příklad `MarkerClick` , do nového umístění na mapě změní pozici fotoaparát:

```csharp
private void MapOnMarkerClick(object sender, GoogleMap.MarkerClickEventArgs markerClickEventArgs)
{
    markerClickEventArgs.Handled = true;
    Marker marker = markerClickEventArgs.P0;
    if (marker.Id.Equals(MyMarkerId)) // The ID of a specific marker the user clicked on.
    {
        _map.AnimateCamera(CameraUpdateFactory.NewLatLngZoom(new LatLng(20.72110, -156.44776), 13));
    }
    else
    {
        Toast.MakeText(this, String.Format("You clicked on Marker ID {0}", marker.Id), ToastLength.Short).Show();
    }
}
```


### <a name="marker-drag-events"></a>Přetáhněte událostech

Tato událost se vyvolá, když uživatel chce přetáhněte značky. Ve výchozím nastavení nejsou značek přetahovatelným. Značku lze nastavit jako přetahovatelným nastavením `Marker.Draggable` vlastnost `true` nebo vyvoláním `MarkerOptions.Draggable` metoda s `true` jako parametr.

Nejdřív přetáhněte značky, uživatel musí dlouho, klikněte na jeho a zachovat jejich prstem na mapě. Při přetažení jejich prstem po obrazovce, se přesune značky. Pokud uživatele prstem zruší z obrazovky, bude značky zůstávají na svém místě.

Následující seznam popisuje různé události, které bude vyvolána pro přetahovatelným značku:

-   `GoogleMap.MarkerDragStart(object sender, GoogleMap.MarkerDragStartEventArgs e)` &ndash; Tato událost se vyvolá, když uživatel nastavuje tažením nejprve značky.

-   `GoogleMap.MarkerDrag(object sender, GoogleMap.MarkerDragEventArgs e)` &ndash; Tato událost je aktivována, jako je právě přetáhli značky.

-   `GoogleMap.MarkerDragEnd(object sender, GoogleMap.MarkerDragEndEventArgs e)` &ndash; Tato událost se vyvolá, když tím, kdy uživatel dokončení přetahování značky.

Každý z `EventArgs` obsahuje jednu vlastnost s názvem `P0` tedy odkaz na `Marker` objektu přetažen.


### <a name="info-window-click-events"></a>Okno informace o události kliknutí

Pouze jeden informační okno lze zobrazit najednou. Když uživatel klikne na okno s informací na mapě, bude vyvolána objekt map `InfoWindowClick` událostí. Následující fragment kódu ukazuje, jak propojit se obslužná rutina události:

```csharp
private bool SetupMapIfNeeded()
{
    if (_map == null)
    {
        _map = _mapFragment.Map;
        if (_map != null)
        {
            _map.InfoWindowClick += MapOnInfoWindowClick;
            return true;
        }
        return false;
    }
    return true;
}

private void MapOnInfoWindowClick (object sender, GoogleMap.InfoWindowClickEventArgs e)
{
    Marker myMarker = e.P0;
    // Do something with marker.
}
```

Odvolat, zda je informační okno statického `View` které je vykresleno jako bitovou kopii na mapě. Všechny pomůcky, jako jsou tlačítka, zaškrtávací políčka nebo zobrazení textu, které jsou umístěny v okně informace budou už netečný a nemůže reagovat na všechny svoje události integrální uživatele.



## <a name="related-links"></a>Související odkazy

- [Služeb Google Play.](http://developer.android.com/google/play-services/index.html)
- [Mapy Google Android API v2](https://developers.google.com/maps/documentation/android/)
- [Google Play Services APK](https://play.google.com/store/apps/details?id=com.google.android.gms&hl=en)
- [Získat klíč Google Maps API](~/android/platform/maps-and-location/maps/obtaining-a-google-maps-api-key.md)
- [Problém 57880: Aktualizovat ale AVD není služby Google Play](https://code.google.com/p/android/issues/detail?id=57880)
