---
title: "Shrnutí kapitoly 28. Umístění a mapy"
ms.topic: article
ms.prod: xamarin
ms.technology: xamarin-forms
author: charlespetzold
ms.author: chape
ms.date: 11/07/2017
ms.openlocfilehash: 7361f65fecfed9d61b9df7088f9021ffa0192ad8
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 02/27/2018
---
# <a name="summary-of-chapter-28-location-and-maps"></a>Shrnutí kapitoly 28. Umístění a mapy

Podporuje Xamarin.Forms [ `Map` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Maps.Map/) element, který je odvozen od `View`. Z důvodu speciální požadavky na platformu účastnící se pomocí map, jsou implementované v samostatném sestavení, **Xamarin.Forms.Maps**a zahrnovat jiný obor názvů: `Xamarin.Forms.Maps`.

## <a name="the-geographic-coordinate-system"></a>Zeměpisná souřadnicový systém

Zeměpisná souřadnicový systém identifikuje pozic na objekt kulovým (nebo téměř kulovým) jako zemském povrchu. Souřadnice se skládá z obou *zeměpisnou šířku* a *zeměpisné délky* vyjádřené v úhly.

Hlavní kružnice volat `equator` je uprostřed mezi dvěma hole, pomocí kterých zemském povrchu osy koncepčně rozšiřuje.

### <a name="parallels-and-latitude"></a>Parallels šířky a délky

Úhel měří severně nebo jižně od rovníku z centra značek Earth řádků stejné zeměpisné šířky známé jako *parallels*. Tyto rozsahu od 0 stupňů na rovníku do 90 stupňů v Severní a Jižní hole. Podle konvence zeměpisné šířky severně od rovníku jsou kladné hodnoty a ty jižně od rovníku záporné hodnoty.

### <a name="longitude-and-meridians"></a>Zeměpisná délka a poledníků zeměpisné

Polovina velké kroužky od Severního k pólu jsou řádky stejné zeměpisné délky, také známé jako *poledníků zeměpisné*. Toto jsou relativní vzhledem k základního poledníku v Greenwich v Anglii. Podle konvence, délka východ od základního poledníku jsou kladné hodnoty od 0 stupňů do 180 stupňů, a délka západně od základního poledníku záporné hodnoty od 0 stupňů & #x 2013; 180 stupňů.

### <a name="the-equirectangular-projection"></a>Projekce equirectangular

Všechny ploché mapy zemském povrchu zavádí narušení. Pokud jsou všechny řádky zeměpisné šířky a délky přímých, a pokud rovna rozdíly v zeměpisné šířky a délky úhly odpovídá na stejnou vzdálenost na mapě, výsledkem je *equirectangular projekce*. Tato mapa deformuje blíž oblasti, které hole, protože jsou vodorovně roztažení.

### <a name="the-mercator-projection"></a>Projekci Mercator

Oblíbených *projekci Mercator* pokusí kompenzovat vodorovné roztažení také roztáhnout těchto oblastem svisle. Výsledkem mapy, kde se zobrazí mnohem větší, než se opravdu jsou, ale žádné místní oblasti vyhovuje velmi úzce k oblasti, skutečný oblasti téměř hole.

### <a name="map-services-and-tiles"></a>Mapa služeb a dlaždice

Mapy služeb pomocí varianta projekci Mercator názvem `Web Mercator`. Mapy služeb doručování dlaždice bitové mapy do klienta na základě umístění a přiblížení úrovně.

## <a name="getting-the-users-location"></a>Získávání umístění uživatele

Platformě Xamarin.Forms `Map` třídy nezahrnují budovy získat geografické umístění uživatele, ale to je často žádoucí, pokud práce maps, takže závislostí služby musí zpracovávat ho.

### <a name="the-location-tracker-api"></a>Sledovací modul umístění rozhraní API

[ **Xamarin.FormsBook.Platform** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Platform) řešení obsahuje kód pro sledovací modul umístění rozhraní API. [ `GeographicLocation` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Platform/Xamarin.FormsBook.Platform/GeographicLocation.cs) Struktura zapouzdří zeměpisné šířky a délky. [ `ILocationTracker` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Platform/Xamarin.FormsBook.Platform/ILocationTracker.cs) Rozhraní definuje dvě metody při spuštění a pozastavení sledovací modul umístění a událost v případě, že je k dispozici nové umístění.

#### <a name="the-ios-location-manager"></a>Správce umístění iOS

Implementace iOS `ILocationTracker` je [ `LocationTracker` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Platform/Xamarin.FormsBook.Platform.iOS/LocationTracker.cs) třídu, která umožňuje používat IOS [ `CLLocationManager` ](https://developer.xamarin.com/api/type/CoreLocation.CLLocationManager/).

#### <a name="the-android-location-manager"></a>Správce Android umístění

Android implementace `ILocationTracker` je [ `LocationTracker` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Platform/Xamarin.FormsBook.Platform.Android/LocationTracker.cs) třídu, která se využívá pro Android [ `LocationManager` ](https://developer.xamarin.com/api/type/Android.Locations.LocationManager/) třídy.

#### <a name="the-windows-runtime-geo-locator"></a>Lokátor geograficky prostředí Windows Runtime

Prostředí Windows Runtime implementace `ILocationTracker` je [ `LocationTracker` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Platform/Xamarin.FormsBook.Platform.WinRT/LocationTracker.cs) třídu, která se využívá UWP [ `Geolocator` ](https://msdn.microsoft.com/library/windows/apps/br225534).

### <a name="display-the-phones-location"></a>Zobrazí umístění telefonu

[ **WhereAmI** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter28/WhereAmI) ukázce se používá k zobrazení umístění v telefonu, v textu i na mapě equirectangular sledovací modul umístění.

### <a name="the-required-overhead"></a>Potřeba režijní náklady

Je vyžadována pro některé zatížení **WhereAmI** používat sledovací modul umístění. První, všechny projekty v **WhereAmI** řešení musí mít odkazy na odpovídající projekty v **Xamarin.FormsBook.Platform**a každou **WhereAmI** projektu musí volat `Toolkit.Init` metoda.

Některé další režii specifické pro platformu, ve formě umístění oprávnění je povinný.

#### <a name="location-permission-for-ios"></a>Umístění oprávnění pro iOS

Pro iOS **info.plist** soubor musí zahrnovat položky obsahující hledaný text dotaz s dotazem na uživatele pro povolení získání umístění tohoto uživatele.

#### <a name="location-permissions-for-android"></a>Umístění oprávnění pro Android

Aplikace pro Android, které získávají umístění uživatele musí mít oprávnění ACCESS_FILE_LOCATION v souboru AndroidManifest.xml.

#### <a name="location-permissions-for-the-windows-runtime"></a>Umístění oprávnění pro prostředí Windows Runtime

Musí mít aplikaci systému Windows nebo Windows Phone `location` funkce zařízení označit v souboru Package.appxmanifest.

## <a name="working-with-xamarinformsmaps"></a>Práce s Xamarin.Forms.Maps

Několik požadavků jsou součástí pomocí `Map` třídy.

### <a name="the-nuget-package"></a>Balíček NuGet

**Xamarin.Forms.Maps** NuGet knihovny musí být přidaný do řešení aplikace. Číslo verze musí být stejná jako **Xamarin.Forms** balíček nainstalován.

### <a name="initializing-the-maps-package"></a>Inicializace balíček mapy

Projekty aplikace musí volat `Xamarin.FormsMaps.Init` metoda po provedení volání `Xamarin.Forms.Forms.Init`.

### <a name="enabling-map-services"></a>Povolení služby mapy

Protože `Map` můžete získat umístění uživatele, aplikace musí získat oprávnění pro uživatele způsobem popsaným dříve v této kapitole:

#### <a name="enabling-ios-maps"></a>Povolení iOS mapy

Aplikace iOS pomocí `Map` vyžaduje dva řádky v souboru info.plist.

#### <a name="enabling-android-maps"></a>Povolení Android mapy

Autorizační klíč je požadované pro použití služby Google mapy. Tento klíč je vložen do **AndroidManifest.xml** souboru. Kromě toho **AndroidManifest.xml** soubor vyžaduje `manifest` značky účastnící se získat umístění uživatele.

#### <a name="enabling-windows-runtime-maps"></a>Povolení prostředí Windows Runtime mapy

Prostředí Windows Runtime aplikace potřebuje autorizační klíč pomocí mapy Bing. Tento klíč je předat jako argument k `Xamarin.FormsMaps.Init` metoda. Aplikace musí být rovněž povolena pro umístění služby.

### <a name="the-unadorned-map"></a>Prostým mapy

[ **MapDemos** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter28/MapDemos) vzorek se skládá z [MapsDemoHomePage.xaml](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter28/MapDemos/MapDemos/MapDemos/MapDemosHomePage.xaml) souboru a [MapsDemoHomePage.xaml.cs](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter28/MapDemos/MapDemos/MapDemos/MapDemosHomePage.xaml.cs) kódu souboru, který umožňuje, přejdete na různých ukázkový programů.

[BasicMapPage.xaml](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter28/MapDemos/MapDemos/MapDemos/BasicMapPage.xaml) souboru ukazuje způsob zobrazení [ `Map` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Maps.Map/) zobrazení. Ve výchozím nastavení zobrazuje Řím města, ale uživatel smí uživatel manipulovat mapy.

Chcete-li zakázat vodorovného a svislého posouvání, nastavte [ `HasScrollEnabled` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Maps.Map.HasScrollEnabled/) vlastnost `false`. Chcete-li zakázat měřítka, nastavte [ `HasZoomEnabled` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Maps.Map.HasZoomEnabled/) k `false`. Tyto vlastnosti nemusí fungovat na všech platformách.

### <a name="streets-and-terrain"></a>Ulice a geologické struktury

Je-li zobrazit různé typy map nastavením `Map` vlastnost [ `MapType` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Maps.Map.MapType/) typu [ `MapType` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Maps.MapType/), výčet se tři členy:

- [`Street`](https://developer.xamarin.com/api/field/Xamarin.Forms.Maps.MapType.Street/), výchozí
- [`Satellite`](https://developer.xamarin.com/api/field/Xamarin.Forms.Maps.MapType.Satellite/)
- [`Hybrid`](https://developer.xamarin.com/api/field/Xamarin.Forms.Maps.MapType.Hybrid/)

[MapTypesPage.xaml](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter28/MapDemos/MapDemos/MapDemos/MapTypesPage.xaml) souboru ukazuje, jak pomocí přepínače vyberte typ mapy. Ho využívá [ `RadioButtonManager` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/RadioButtonManager.cs) třídy v [ **Xamarin.FormsBook.Toolkit** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit) na základě knihovny a třídu [MapTypeRadioButton.xaml](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter28/MapDemos/MapDemos/MapDemos/MapTypeRadioButton.xaml) souboru.

### <a name="map-coordinates"></a>Souřadnicích mapy

Program můžete získat aktuální oblasti, která `Map` je zobrazení prostřednictvím [ `VisibleRegion` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Maps.Map.VisibleRegion/) vlastnost. Tato vlastnost je *není* zálohovaný pomocí vazbu vlastnosti, a neexistuje žádný mechanismus oznámení k označení, pokud se změnila, takže program, který chce monitorovat vlastnost vhodnější použít časovač k tomuto účelu.

`VisibleRegion` je typu [ `MapSpan` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Maps.MapSpan/), třídu s vlastnostmi čtyři jen pro čtení:

- [`Center`](https://developer.xamarin.com/api/property/Xamarin.Forms.Maps.MapSpan.Center/) typu [`Position`](https://developer.xamarin.com/api/type/Xamarin.Forms.Maps.Position/)
- [`LatitudeDegrees`](https://developer.xamarin.com/api/property/Xamarin.Forms.Maps.MapSpan.LatitudeDegrees/) typu `double`, která určuje výšku oblasti zobrazených mapy
- [`LongitudeDegrees`](https://developer.xamarin.com/api/property/Xamarin.Forms.Maps.MapSpan.LongitudeDegrees/) typu `double`, která určuje šířku oblasti zobrazených mapy
- [`Radius`](https://developer.xamarin.com/api/property/Xamarin.Forms.Maps.MapSpan.Radius/) typu [ `Distance` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Maps.Distance/), označující velikost největší cyklické oblasti viditelné na mapě

`Position` a `Distance` jsou obě struktury. `Position` definuje dvě vlastnosti jen pro čtení nastavených prostřednictvím [ `Position` konstruktor](https://developer.xamarin.com/api/constructor/Xamarin.Forms.Maps.Position.Position/p/System.Double/System.Double/):

- [`Latitude`](https://developer.xamarin.com/api/property/Xamarin.Forms.Maps.Position.Latitude/)
- [`Longitude`](https://developer.xamarin.com/api/property/Xamarin.Forms.Maps.Position.Longitude/)

`Distance` slouží jako nezávislé na jednotku vzdálenosti podle převod mezi metrika a anglické jednotky. A `Distance` hodnotu lze vytvořit několika způsoby:

- [`Distance` Konstruktor](https://developer.xamarin.com/api/constructor/Xamarin.Forms.Maps.Distance.Distance/p/System.Double/) s vzdálenost v měřidla
- [`Distance.FromMeters`](https://developer.xamarin.com/api/member/Xamarin.Forms.Maps.Distance.FromMeters/p/System.Double/) statickou metodu
- [`Distance.FromKilometers`](https://developer.xamarin.com/api/member/Xamarin.Forms.Maps.Distance.FromKilometers/p/System.Double/) statickou metodu
- [`Distance.FromMiles`](https://developer.xamarin.com/api/member/Xamarin.Forms.Maps.Distance.FromMiles/p/System.Double/) statickou metodu

Hodnota je k dispozici tři vlastnosti:

- [`Meters`](https://developer.xamarin.com/api/property/Xamarin.Forms.Maps.Distance.Meters/) typu `double`
- [`Kilometers`](https://developer.xamarin.com/api/property/Xamarin.Forms.Maps.Distance.Kilometers/) typu `double`
- [`Miles`](https://developer.xamarin.com/api/property/Xamarin.Forms.Maps.Distance.Miles/) typu `double`

[MapCoordinatesPage.xaml](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter28/MapDemos/MapDemos/MapDemos/MapCoordinatesPage.xaml) soubor obsahuje několik `Label` prvky pro zobrazení `MapSpan` informace. [MapCoordinatesPage.xaml.cs](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter28/MapDemos/MapDemos/MapDemos/MapCoordinatesPage.xaml.cs) souboru kódu na pozadí používá časovač pro zachování informací o aktualizovat, protože uživatel manipuluje mapy.

### <a name="position-extensions"></a>Pozice rozšíření

Nová knihovna pro tato kniha s názvem [ **Xamarin.FormsBook.Toolkit.Maps** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit.Maps) obsahuje typy specifické pro mapu, ale nezávislé na platformě. [ `PositionExtensions` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit.Maps/Xamarin.FormsBook.Toolkit.Maps/PositionExtensions.cs) Třída má `ToString` metodu pro `Position`a metodu pro výpočet vzdálenost mezi dvěma `Position` hodnoty.

### <a name="setting-an-initial-location"></a>Nastavení počáteční umístění

Můžete volat [ `MoveToRegion` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Maps.Map.MoveToRegion/p/Xamarin.Forms.Maps.MapSpan/) metodu `Map` prostřednictvím kódu programu nastavení úrovně umístění a přiblížení na mapě. Argument je typu `MapSpan`. Můžete vytvořit `MapSpan` pomocí následujících nastavení:

- [`MapSpan` Konstruktor](https://developer.xamarin.com/api/constructor/Xamarin.Forms.Maps.MapSpan.MapSpan/p/Xamarin.Forms.Maps.Position/System.Double/System.Double/) s `Position`a zeměpisnou šířku a délku rozpětí
- [`MapSpan.FromCenterAndRadius`](https://developer.xamarin.com/api/member/Xamarin.Forms.Maps.MapSpan.FromCenterAndRadius/p/Xamarin.Forms.Maps.Position/Xamarin.Forms.Maps.Distance/) s `Position` a protokolu radius

Je také možné vytvořit novou `MapSpan` z některého ze stávajících pomocí metody [ `ClampLatitude` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Maps.MapSpan.ClampLatitude/p/System.Double/System.Double/) nebo [ `WithZoom` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Maps.MapSpan.WithZoom/p/System.Double/).

[WyomingPage.xaml](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter28/MapDemos/MapDemos/MapDemos/WyomingPage.xaml) souboru a [WyomingPage.xaml.cs](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter28/MapDemos/MapDemos/MapDemos/WyomingPage.xaml.cs) souboru kódu ukazuje, jak používat `MoveToRegion` metodu pro zobrazení stavu Wyoming.

Případně můžete použít [ `Map` konstruktor](https://developer.xamarin.com/api/constructor/Xamarin.Forms.Maps.Map.Map/p/Xamarin.Forms.Maps.MapSpan/) s `MapSpan` objekt inicializovat umístění mapy. [XamarinHQPage.xaml](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter28/MapDemos/MapDemos/MapDemos/XamarinHQPage.xaml) souboru ukazuje, jak to provést zcela v jazyce XAML zobrazíte na Xamarin ústředí v San Franciscu.

### <a name="dynamic-zooming"></a>Dynamické měřítka

Můžete použít `Slider` pro dynamicky Přiblížení mapy. [RadiusZoomPage.xaml](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter28/MapDemos/MapDemos/MapDemos/RadiusZoomPage.xaml) souboru a [RadiusZoomPage.xaml.cs](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter28/MapDemos/MapDemos/MapDemos/RadiusZoomPage.xaml.cs) souboru kódu ukazují, jak změnit radius mapy na základě `Slider` hodnotu.

[LongitudeZoomPage.xaml](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter28/MapDemos/MapDemos/MapDemos/LongitudeZoomPage.xaml) souboru a [LongitudeZoomPage.xaml.cs](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter28/MapDemos/MapDemos/MapDemos/LongitudeZoomPage.xaml.cs) souboru kódu zobrazit alternativní způsob, který funguje lépe v systému Android, ale ani přístup funguje dobře v systému Windows platformy.

### <a name="the-phones-location"></a>Umístění telefonu

[ `IsShowingUser` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Maps.Map.IsShowingUser/) Vlastnost `Map` funguje trochu jinak na tři platformách, jako [ShowLocationPage.xaml](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter28/MapDemos/MapDemos/MapDemos/ShowLocationPage.xaml) souboru ukazuje:

- V systému iOS modrá tečka označuje umístění telefonu, ale je nutné ručně přejít existuje
- V systému Android, zobrazí se ikona který po nabídnutých přesune mapy do telefonu umístění
- UWP je podobná iOS ale někdy automaticky přejde na umístění

**MapDemos** projektu pokusí tak, aby napodoboval Android přístup tak, že první definujete založeno na tlačítko na základě [MyLocationButton.xaml](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter28/MapDemos/MapDemos/MapDemos/MyLocationButton.xaml) souboru a [MyLocationButton.xaml.cs](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter28/MapDemos/MapDemos/MapDemos/MyLocationButton.xaml.cs) souboru kódu na pozadí.

[GoToLocationPage.xaml](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter28/MapDemos/MapDemos/MapDemos/GoToLocationPage.xaml) souboru a [GoToLocationPage.xaml.cs](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter28/MapDemos/MapDemos/MapDemos/GoToLocationPage.xaml.cs) souboru kódu na pozadí toto tlačítko slouží k přejděte do umístění telefonu.

### <a name="pins-and-science-museums"></a>PIN kódy a vědecké účely muzea

Nakonec `Map` třída definuje [ `Pins` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Maps.Map.Pins/) vlastnost typu `IList<Pin>`. [ `Pin` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Maps.Pin/) Třída definuje čtyři vlastnosti:

- [`Label`](https://developer.xamarin.com/api/property/Xamarin.Forms.Maps.Pin.Label/) typu `string`je požadovaná vlastnost
- [`Address`](https://developer.xamarin.com/api/property/Xamarin.Forms.Maps.Pin.Address/) typu `string`, volitelné čitelná pro člověka adresu
- [`Position`](https://developer.xamarin.com/api/property/Xamarin.Forms.Maps.Pin.Position/) typu `Position`, která určuje, kde je kód pin zobrazit na mapě
- [`Type`](https://developer.xamarin.com/api/property/Xamarin.Forms.Maps.Pin.Type/) typu [ `PinType` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Maps.PinType/), výčet, který se nepoužívá

**MapDemos** projekt obsahuje soubor [ScienceMuseums.xml](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter28/MapDemos/MapDemos/MapDemos/Data/ScienceMuseums.xml), který uvádí muzea vědě ve Spojených státech amerických, a [ `Locations` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter28/MapDemos/MapDemos/MapDemos/Locations.cs) a [ `Site` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter28/MapDemos/MapDemos/MapDemos/Site.cs) třídy pro deserializaci tato data.

[ScienceMuseumsPage.xaml](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter28/MapDemos/MapDemos/MapDemos/ScienceMuseumsPage.xaml) souboru a [ScienceMuseumsPage.xaml.cs](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter28/MapDemos/MapDemos/MapDemos/ScienceMuseumsPage.xaml.cs) kódu PIN kódy zobrazení souboru pro tyto vědecké účely muzea v mapě. Když uživatel klepnutím kód pin, zobrazí adresu a web pro muzea.

### <a name="the-distance-between-two-points"></a>Vzdálenost mezi dva body.

[ `PositionExtensions` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit.Maps/Xamarin.FormsBook.Toolkit.Maps/PositionExtensions.cs) Třída obsahuje [ `DistanceTo` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit.Maps/Xamarin.FormsBook.Toolkit.Maps/PositionExtensions.cs#L88) metoda s zjednodušené výpočtu vzdálenosti mezi dvěma zeměpisné umístění.

To se používá v [LocalMuseumsPage.xaml](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter28/MapDemos/MapDemos/MapDemos/LocalMuseumsPage.xaml) souboru a [LocalMuseumsPage.xaml.cs](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter28/MapDemos/MapDemos/MapDemos/LocalMuseumsPage.xaml.cs) souboru kódu má také zobrazit vzdálenost muzea umístění uživatele:

[![Trojitá snímek obrazovky stránky místní muzea](images/ch28fg28-small.png "vzdálenost do umístění")](images/ch28fg28-large.png "vzdálenost do umístění")

Program také ukazuje, jak dynamicky omezit počet kódů PIN na základě umístění mapy.

## <a name="geocoding-and-back-again"></a>Geografické kódování a zpět znovu

[ **Xamarin.Forms.Maps** ](https://developer.xamarin.com/api/namespace/Xamarin.Forms.Maps/) sestavení také obsahuje [ `Geocoder` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Maps.Geocoder/) třídy s [ `GetPositionsForAddressAsync` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Maps.Geocoder.GetPositionsForAddressAsync/p/System.String/) metodu, která převede adresu text do nula nebo více možné zeměpisné polohy a jinou metodu [ `GetAddressesForPositionAsync` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Maps.Geocoder.GetAddressesForPositionAsync/p/Xamarin.Forms.Maps.Position/) , která převádí opačným směrem.

[GeocoderRoundTrip.xaml](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter28/MapDemos/MapDemos/MapDemos/GeocoderRoundTripPage.xaml) souboru a [GeocoderRoundTrip.xaml.cs](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter28/MapDemos/MapDemos/MapDemos/GeocoderRoundTripPage.xaml.cs) souboru kódu ukazují, pokud tuto funkci.



## <a name="related-links"></a>Související odkazy

- [Úplný text 28 kapitoly (PDF)](https://download.xamarin.com/developer/xamarin-forms-book/XamarinFormsBook-Ch28-Aug2016.pdf)
- [Ukázky kapitoly 28](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter28)
- [Mapový ovládací prvek](~/xamarin-forms/user-interface/map.md)
