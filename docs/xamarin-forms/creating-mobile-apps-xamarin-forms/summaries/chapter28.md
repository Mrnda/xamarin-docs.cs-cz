---
title: Souhrn kapitola 28. Poloha a mapy
description: 'Vytváření mobilních aplikací s Xamarin.Forms: Souhrn kapitola 28. Poloha a mapy'
ms.prod: xamarin
ms.technology: xamarin-forms
ms.assetid: F6E20077-687C-45C4-A375-31D4F49BBFA4
author: charlespetzold
ms.author: chape
ms.date: 07/19/2018
ms.openlocfilehash: da8ce02a0185364c2b833238ee04ebc29e8d3bb2
ms.sourcegitcommit: 8555a4dd1a579b2206f86c867125ee20fbc3d264
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/19/2018
ms.locfileid: "39156610"
---
# <a name="summary-of-chapter-28-location-and-maps"></a>Souhrn kapitola 28. Poloha a mapy

> [!NOTE] 
> Poznámky na této stránce označit oblasti, kde se Xamarin.Forms se rozcházela z materiály uvedené v seznamu.

Podporuje Xamarin.Forms [ `Map` ](xref:Xamarin.Forms.Maps.Map) element, který je odvozen od `View`. Vzhledem k požadavkům na platformu speciální účastnící se pomocí mapy, jsou implementovány v samostatném sestavení **xamarin.Forms.Maps není úspěšný kvůli**a zahrnovat jiný obor názvů: `Xamarin.Forms.Maps`.

## <a name="the-geographic-coordinate-system"></a>Geografické souřadnicový systém

Geografické souřadnicový systém identifikuje pozice na kulovité (nebo skoro ke kulovému) objektu jako země. Souřadnice se skládá ze *zeměpisná šířka* a *délky* vyjádřeny úhlů.

Kruh velké volá se, `equator` je uprostřed mezi dvěma hole, pomocí kterých země na východozápadní ose osy koncepčně rozšiřuje.

### <a name="parallels-and-latitude"></a>Parallels zeměpisné šířky a délky

Úhel měřený severně nebo jižně od rovníku v centru značky Earth řádky stejné šířky, označované jako *parallels*. Jsou nejrůznější, od 0 stupňů na rovníku do 90 stupňů v Severní a Jižní hole. Podle konvence jsou zeměpisná šířka severně od rovníku kladné hodnoty a jižně od rovníku jsou záporné hodnoty.

### <a name="longitude-and-meridians"></a>Zeměpisná délka a poledníky

Polovinami kruhy velké od jižní pól jsou řádky stejné délky, také známé jako *poledníky*. Toto jsou relativní vzhledem k poledníku, který v Greenwiche v Anglii. Podle konvence, délka východ od poledníku, který je kladné hodnoty od 0 stupňů do 180 stupňů a délka západně od poledníku, který je záporné hodnoty od 0 stupňů &ndash;180 stupňů.

### <a name="the-equirectangular-projection"></a>Projekce equirectangular

Žádné bez stromové struktury mapy všech koutech světa zavádí narušení. Pokud všechny řádky zeměpisná šířka a délka jsou rovné, a pokud rovná rozdíly v zeměpisné šířky a délky úhly odpovídají stejnou vzdálenost na mapě, výsledek je *equirectangular projekce*. Toto mapování deformuje blíž oblasti, které hole vzhledem k tomu, že jsou vodorovně roztažená navýšení kapacity.

### <a name="the-mercator-projection"></a>Projekci Mercator

Oblíbené *projekci Mercator* pokusí kompenzovat vodorovné roztažení roztažením také tyto oblasti svisle. Výsledkem je Mapa umístění mnohem větší, než ve skutečnosti jsou to ale žádné místní oblasti skutečné odpovídá poměrně pečlivě oblastí blízko hole.

### <a name="map-services-and-tiles"></a>Mapa služeb a dlaždice

Mapa služby používat varianta projekci Mercator volá `Web Mercator`. Mapy služeb doručování rastrový obrázek dlaždice do klienta na základě umístění a přiblížení úrovně.

## <a name="getting-the-users-location"></a>Načítá se poloha uživatele

Xamarin.Forms `Map` třídy neobsahují zařízení, které chcete získat geografické umístění uživatele, ale to je často žádoucí při práci s mapy, takže závislostí služby musí ji zpracovat.

> [!NOTE]
> Místo toho můžete použít aplikace Xamarin.Forms [ `Geolocation` ](~/essentials/geolocation.md) třída součástí Xamarin.Essentials.

### <a name="the-location-tracker-api"></a>Sledovací modul umístění rozhraní API

[ **Xamarin.FormsBook.Platform** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Platform) řešení obsahuje kód pro sledování umístění rozhraní API. [ `GeographicLocation` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Platform/Xamarin.FormsBook.Platform/GeographicLocation.cs) Struktura zapouzdřuje zeměpisné šířky a délky. [ `ILocationTracker` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Platform/Xamarin.FormsBook.Platform/ILocationTracker.cs) Rozhraní definuje dvě metody pro spuštění a pozastavení sledovací modul umístění a událost, když je k dispozici nové umístění.

#### <a name="the-ios-location-manager"></a>Správce umístění iOS

Implementace iOS `ILocationTracker` je [ `LocationTracker` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Platform/Xamarin.FormsBook.Platform.iOS/LocationTracker.cs) třídu, která umožňuje používat zařízení se systémy IOS [ `CLLocationManager` ](https://developer.xamarin.com/api/type/CoreLocation.CLLocationManager/).

#### <a name="the-android-location-manager"></a>Správce Android umístění

Android provádění `ILocationTracker` je [ `LocationTracker` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Platform/Xamarin.FormsBook.Platform.Android/LocationTracker.cs) třídu, která využívá Android [ `LocationManager` ](https://developer.xamarin.com/api/type/Android.Locations.LocationManager/) třídy.

#### <a name="the-uwp-geo-locator"></a>Lokátor geograficky UPW

Univerzální platforma Windows implementace `ILocationTracker` je [ `LocationTracker` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Platform/Xamarin.FormsBook.Platform.WinRT/LocationTracker.cs) třídu, která využívá UPW [ `Geolocator` ](/uwp/api/Windows.Devices.Geolocation.Geolocator).

### <a name="display-the-phones-location"></a>V telefonu umístění zobrazení

[ **WhereAmI** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter28/WhereAmI) Ukázka používá sledovací modul umístění zobrazíte umístění v telefonu, text a na mapě equirectangular.

### <a name="the-required-overhead"></a>Vyžadováno režijní náklady

Je třeba použít režijní náklady **WhereAmI** použít sledovací modul umístění. První, všechny projekty v **WhereAmI** řešení musí mít odkazy na odpovídající projekty v **Xamarin.FormsBook.Platform**a každý **WhereAmI** projektu zavolat `Toolkit.Init` metody.

Vyžaduje se další specifické pro platformu režijní náklady, ve formě umístění oprávnění.

#### <a name="location-permission-for-ios"></a>Umístění oprávnění pro iOS

Pro iOS **info.plist** soubor musí obsahovat položky obsahující text dotaz s žádostí o umožňující získání jeho umístění.

#### <a name="location-permissions-for-android"></a>Umístění oprávnění pro Android

Aplikace pro Android, které získávají podle umístění uživatele musí mít oprávnění ACCESS_FILE_LOCATION v souboru AndroidManifest.xml.

#### <a name="location-permissions-for-the-uwp"></a>Umístění oprávnění pro UPW

Aplikace pro univerzální platformu Windows musí mít `location` funkce zařízení označená v Package.appxmanifest souboru.

## <a name="working-with-xamarinformsmaps"></a>Práce s xamarin.Forms.Maps není úspěšný kvůli

Několik požadavků se týká použití `Map` třídy.

### <a name="the-nuget-package"></a>Balíček NuGet.

**Xamarin.Forms.Maps není úspěšný kvůli** NuGet knihovny musí být přidán do řešení aplikace. Číslo verze musí být stejná jako **Xamarin.Forms** aktuálně nainstalovaný balíček.

### <a name="initializing-the-maps-package"></a>Inicializuje se balíček mapy

Projekty aplikace musí volat `Xamarin.FormsMaps.Init` metoda po provedení volání `Xamarin.Forms.Forms.Init`.

### <a name="enabling-map-services"></a>Povolení mapy služeb

Vzhledem k tomu, `Map` můžete získat podle umístění uživatele, aplikace musí získat oprávnění pro uživatele je popsáno výše v této kapitole způsobem:

#### <a name="enabling-ios-maps"></a>Povolení iOS mapy

Aplikace iOS s využitím `Map` vyžaduje dva řádky v souboru info.plist.

#### <a name="enabling-android-maps"></a>Povolení Androidu mapy

Autorizační klíč je vyžadován pro použití služby mapy Google. Tento klíč je vložen do **AndroidManifest.xml** souboru. Kromě toho **AndroidManifest.xml** vyžaduje soubor `manifest` značky při získávání umístění uživatele.

#### <a name="enabling-uwp-maps"></a>Povolení UPW mapy

Aplikace pro univerzální platformu Windows vyžaduje autorizačního klíče pro použití služby mapy Bing. Tento klíč je předán jako argument `Xamarin.FormsMaps.Init` metody. Aplikace musí být povolena také pro umístění služby.

### <a name="the-unadorned-map"></a>Prostý mapy

[ **MapDemos** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter28/MapDemos) vzorek se skládá z [MapsDemoHomePage.xaml](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter28/MapDemos/MapDemos/MapDemos/MapDemosHomePage.xaml) souboru a [MapsDemoHomePage.xaml.cs](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter28/MapDemos/MapDemos/MapDemos/MapDemosHomePage.xaml.cs) soubor kódu Umožňuje přechod na různé ukázkové programy.

[BasicMapPage.xaml](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter28/MapDemos/MapDemos/MapDemos/BasicMapPage.xaml) soubor ukazuje, jak zobrazit [ `Map` ](xref:Xamarin.Forms.Maps.Map) zobrazení. Ve výchozím nastavení zobrazí město Řím, ale uživatel můžete pracovat na mapě.

Chcete-li zakázat vodorovné a svislé posouvání, nastavte [ `HasScrollEnabled` ](xref:Xamarin.Forms.Maps.Map.HasScrollEnabled) vlastnost `false`. Chcete-li zakázat měřítka, nastavte [ `HasZoomEnabled` ](xref:Xamarin.Forms.Maps.Map.HasZoomEnabled) k `false`. Tyto vlastnosti nemusí fungovat na všech platformách.

### <a name="streets-and-terrain"></a>Ulice a terénu

Různé druhy maps můžete zobrazit tak, že nastavíte `Map` vlastnost [ `MapType` ](xref:Xamarin.Forms.Maps.Map.MapType) typu [ `MapType` ](xref:Xamarin.Forms.Maps.MapType), výčet pomocí tří členů:

- [`Street`](xref:Xamarin.Forms.Maps.MapType.Street), výchozí hodnota
- [`Satellite`](xref:Xamarin.Forms.Maps.MapType.Satellite)
- [`Hybrid`](xref:Xamarin.Forms.Maps.MapType.Hybrid)

[MapTypesPage.xaml](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter28/MapDemos/MapDemos/MapDemos/MapTypesPage.xaml) soubor ukazuje, jak pomocí přepínače vyberte typ mapy. Ji používá [ `RadioButtonManager` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/RadioButtonManager.cs) třídy v [ **Xamarin.FormsBook.Toolkit** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit) na základě knihovny a třídu [MapTypeRadioButton.xaml](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter28/MapDemos/MapDemos/MapDemos/MapTypeRadioButton.xaml) souboru.

### <a name="map-coordinates"></a>Souřadnicích mapy

Program můžete získat aktuální oblasti, která `Map` zobrazení prostřednictvím [ `VisibleRegion` ](xref:Xamarin.Forms.Maps.Map.VisibleRegion) vlastnost. Tato vlastnost je *není* se opírá o vlastnost podporující vazby, a neexistuje žádný mechanismus oznámení k označení, kdy byl změněn, takže program, který chce monitorovat vlastnost vhodné použít časovač pro tento účel.

`VisibleRegion` je typu [ `MapSpan` ](xref:Xamarin.Forms.Maps.MapSpan), třída s atributem čtyři vlastnosti jen pro čtení:

- [`Center`](xref:Xamarin.Forms.Maps.MapSpan.Center) typu [`Position`](xref:Xamarin.Forms.Maps.Position)
- [`LatitudeDegrees`](xref:Xamarin.Forms.Maps.MapSpan.LatitudeDegrees) typ `double`, označující výšku oblasti zobrazené mapy
- [`LongitudeDegrees`](xref:Xamarin.Forms.Maps.MapSpan.LongitudeDegrees) typ `double`, Long určující šířku zobrazených oblasti mapy
- [`Radius`](xref:Xamarin.Forms.Maps.MapSpan.Radius) typu [ `Distance` ](xref:Xamarin.Forms.Maps.Distance), určující velikost největší kruhové oblasti na mapě viditelné

`Position` a `Distance` jsou obě struktury. `Position` definuje dvě vlastnosti jen pro čtení nastavené přes [ `Position` konstruktor](xref:Xamarin.Forms.Maps.Position.%23ctor(System.Double,System.Double)):

- [`Latitude`](xref:Xamarin.Forms.Maps.Position.Latitude)
- [`Longitude`](xref:Xamarin.Forms.Maps.Position.Longitude)

`Distance` je určená k zajištění nezávislé na jednotku vzdálenosti převedením mezi metriky a angličtině jednotky. A `Distance` hodnotu lze vytvořit několika způsoby:

- [`Distance` Konstruktor](xref:Xamarin.Forms.Maps.Distance.%23ctor(System.Double)) vzdálenost v metrech
- [`Distance.FromMeters`](xref:Xamarin.Forms.Maps.Distance.FromMeters(System.Double)) statická metoda
- [`Distance.FromKilometers`](xref:Xamarin.Forms.Maps.Distance.FromKilometers(System.Double)) statická metoda
- [`Distance.FromMiles`](xref:Xamarin.Forms.Maps.Distance.FromMiles(System.Double)) statická metoda

Hodnota je k dispozici tři vlastnosti:

- [`Meters`](xref:Xamarin.Forms.Maps.Distance.Meters) typu `double`
- [`Kilometers`](xref:Xamarin.Forms.Maps.Distance.Kilometers) typu `double`
- [`Miles`](xref:Xamarin.Forms.Maps.Distance.Miles) typu `double`

[MapCoordinatesPage.xaml](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter28/MapDemos/MapDemos/MapDemos/MapCoordinatesPage.xaml) soubor obsahuje několik `Label` prvky pro zobrazení `MapSpan` informace. [MapCoordinatesPage.xaml.cs](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter28/MapDemos/MapDemos/MapDemos/MapCoordinatesPage.xaml.cs) soubor kódu na pozadí používá časovač Udržovat informace aktualizovat, protože uživatel manipuluje se na mapě.

### <a name="position-extensions"></a>Pozice rozšíření

Nová knihovna pro tento seznam s názvem [ **Xamarin.FormsBook.Toolkit.Maps** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit.Maps) obsahuje typy, specifické pro mapu, ale nezávislý na platformě. [ `PositionExtensions` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit.Maps/Xamarin.FormsBook.Toolkit.Maps/PositionExtensions.cs) Třída nemá `ToString` metodu `Position`a metodu pro výpočet vzdálenost mezi dvěma `Position` hodnoty.

### <a name="setting-an-initial-location"></a>Nastavení počáteční umístění

Můžete volat [ `MoveToRegion` ](xref:Xamarin.Forms.Maps.Map.MoveToRegion(Xamarin.Forms.Maps.MapSpan)) metoda `Map` prostřednictvím kódu programu nastavit úroveň umístění a přiblížení na mapě. Argument je typu `MapSpan`. Můžete vytvořit `MapSpan` pomocí některého z následujících:

- [`MapSpan` Konstruktor](xref:Xamarin.Forms.Maps.MapSpan.%23ctor(Xamarin.Forms.Maps.Position,System.Double,System.Double)) s `Position`a zeměpisné šířky a délky rozpětí
- [`MapSpan.FromCenterAndRadius`](xref:Xamarin.Forms.Maps.MapSpan.FromCenterAndRadius(Xamarin.Forms.Maps.Position,Xamarin.Forms.Maps.Distance)) s `Position` a protokolu radius

Je také možné vytvořit nový `MapSpan` z existující pomocí metod [ `ClampLatitude` ](xref:Xamarin.Forms.Maps.MapSpan.ClampLatitude(System.Double,System.Double)) nebo [ `WithZoom` ](xref:Xamarin.Forms.Maps.MapSpan.WithZoom(System.Double)).

[WyomingPage.xaml](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter28/MapDemos/MapDemos/MapDemos/WyomingPage.xaml) souboru a [WyomingPage.xaml.cs](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter28/MapDemos/MapDemos/MapDemos/WyomingPage.xaml.cs) použití modelu code-behind soubor popisuje způsob použití `MoveToRegion` metodu pro zobrazení stát Wyoming.

Můžete také použít [ `Map` konstruktor](xref:Xamarin.Forms.Maps.Map.%23ctor(Xamarin.Forms.Maps.MapSpan)) s `MapSpan` objekt inicializovat umístění na mapě. [XamarinHQPage.xaml](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter28/MapDemos/MapDemos/MapDemos/XamarinHQPage.xaml) soubor ukazuje, jak to provést jenom v XAML zobrazení Xamarinu pro ústředí v kalifornském San Franciscu.

### <a name="dynamic-zooming"></a>Dynamická změna měřítka zobrazení

Můžete použít `Slider` dynamicky Přiblížení mapy. [RadiusZoomPage.xaml](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter28/MapDemos/MapDemos/MapDemos/RadiusZoomPage.xaml) souboru a [RadiusZoomPage.xaml.cs](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter28/MapDemos/MapDemos/MapDemos/RadiusZoomPage.xaml.cs) soubor kódu ukazují, jak změnit pomocí protokolu radius na základě mapy `Slider` hodnotu.

[LongitudeZoomPage.xaml](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter28/MapDemos/MapDemos/MapDemos/LongitudeZoomPage.xaml) souboru a [LongitudeZoomPage.xaml.cs](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter28/MapDemos/MapDemos/MapDemos/LongitudeZoomPage.xaml.cs) použití modelu code-behind soubor zobrazit alternativním přístupem, který lépe funguje v systému Android, ale ani přístup funguje dobře pro Windows platformy.

### <a name="the-phones-location"></a>Umístění v telefonu

[ `IsShowingUser` ](xref:Xamarin.Forms.Maps.Map.IsShowingUser) Vlastnost `Map` funguje trochu jinak na třech platformách, jako [ShowLocationPage.xaml](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter28/MapDemos/MapDemos/MapDemos/ShowLocationPage.xaml) soubor ukazuje:

- V systémech iOS modrá tečka označuje umístění v telefonu, ale je nutné ručně přejít existuje
- V systému Android, zobrazí se ikona, že vloženo přesune na mapě do umístění v telefonu
- Na UPW je podobný s Iosem ale někdy automaticky přejde na umístění

**MapDemos** pokusí projekt tak, aby napodoboval Android přístup tak, že první definujete založené na ikonu tlačítko na základě [MyLocationButton.xaml](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter28/MapDemos/MapDemos/MapDemos/MyLocationButton.xaml) souboru a [MyLocationButton.xaml.cs](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter28/MapDemos/MapDemos/MapDemos/MyLocationButton.xaml.cs) soubor kódu na pozadí.

[GoToLocationPage.xaml](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter28/MapDemos/MapDemos/MapDemos/GoToLocationPage.xaml) souboru a [GoToLocationPage.xaml.cs](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter28/MapDemos/MapDemos/MapDemos/GoToLocationPage.xaml.cs) soubor kódu na pozadí pomocí tohoto tlačítka můžete přejít na umístění v telefonu.

### <a name="pins-and-science-museums"></a>Kódy PIN a muzea vědy

Nakonec `Map` definuje třídu [ `Pins` ](xref:Xamarin.Forms.Maps.Map.Pins) vlastnost typu `IList<Pin>`. [ `Pin` ](xref:Xamarin.Forms.Maps.Pin) Třída definuje čtyři vlastnosti:

- [`Label`](xref:Xamarin.Forms.Maps.Pin.Label) typ `string`, požadovaná vlastnost
- [`Address`](xref:Xamarin.Forms.Maps.Pin.Address) typ `string`, volitelné čitelné adresu
- [`Position`](xref:Xamarin.Forms.Maps.Pin.Position) typ `Position`, určující, kde se zobrazí kód pin na mapě
- [`Type`](xref:Xamarin.Forms.Maps.Pin.Type) typu [ `PinType` ](xref:Xamarin.Forms.Maps.PinType), výčtu, která není používána.

**MapDemos** projekt obsahuje soubor [ScienceMuseums.xml](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter28/MapDemos/MapDemos/MapDemos/Data/ScienceMuseums.xml), vypíše seznam vědy muzea ve Spojených státech a [ `Locations` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter28/MapDemos/MapDemos/MapDemos/Locations.cs) a [ `Site` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter28/MapDemos/MapDemos/MapDemos/Site.cs) třídy pro deserializaci tato data.

[ScienceMuseumsPage.xaml](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter28/MapDemos/MapDemos/MapDemos/ScienceMuseumsPage.xaml) souboru a [ScienceMuseumsPage.xaml.cs](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter28/MapDemos/MapDemos/MapDemos/ScienceMuseumsPage.xaml.cs) použití modelu code-behind soubor zobrazení špendlíky pro tyto vědy muzea v objektu map. Když uživatel klepne kód pin, zobrazí adresu a webu, muzejních.

### <a name="the-distance-between-two-points"></a>Vzdálenost mezi dvěma body

[ `PositionExtensions` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit.Maps/Xamarin.FormsBook.Toolkit.Maps/PositionExtensions.cs) Obsahuje třídy [ `DistanceTo` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit.Maps/Xamarin.FormsBook.Toolkit.Maps/PositionExtensions.cs#L88) metodu s zjednodušené výpočet vzdálenost mezi dvěma zeměpisné umístění.

Používá se v [LocalMuseumsPage.xaml](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter28/MapDemos/MapDemos/MapDemos/LocalMuseumsPage.xaml) souboru a [LocalMuseumsPage.xaml.cs](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter28/MapDemos/MapDemos/MapDemos/LocalMuseumsPage.xaml.cs) soubor kódu na pozadí, který se také zobrazí vzdálenost muzejních podle umístění uživatele:

[![Trojitá snímek obrazovky stránky místní muzea](images/ch28fg28-small.png "vzdálenost k místu")](images/ch28fg28-large.png#lightbox "vzdálenost k místu")

Program také ukazuje, jak dynamicky omezit počet kódů PIN, v závislosti na umístění na mapě.

## <a name="geocoding-and-back-again"></a>Geokódování a zpět

[ **Xamarin.Forms.Maps není úspěšný kvůli** ](xref:Xamarin.Forms.Maps) také obsahuje sestavení [ `Geocoder` ](xref:Xamarin.Forms.Maps.Geocoder) třídy s [ `GetPositionsForAddressAsync` ](xref:Xamarin.Forms.Maps.Geocoder.GetPositionsForAddressAsync(System.String)) metodu, která převede Adresa text do nula nebo více možné zeměpisné polohy a jinou metodu [ `GetAddressesForPositionAsync` ](xref:Xamarin.Forms.Maps.Geocoder.GetAddressesForPositionAsync(Xamarin.Forms.Maps.Position)) , která převede opačným směrem.

[GeocoderRoundTrip.xaml](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter28/MapDemos/MapDemos/MapDemos/GeocoderRoundTripPage.xaml) souboru a [GeocoderRoundTrip.xaml.cs](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter28/MapDemos/MapDemos/MapDemos/GeocoderRoundTripPage.xaml.cs) soubor kódu ukazují tuto funkci.



## <a name="related-links"></a>Související odkazy

- [Kapitola 28 textu v plném znění (PDF)](https://download.xamarin.com/developer/xamarin-forms-book/XamarinFormsBook-Ch28-Aug2016.pdf)
- [Ukázky kapitola 28](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter28)
- [Mapa Xamarin.Forms](~/xamarin-forms/user-interface/map.md)
