---
title: Mapa Xamarin.Forms
description: Tento článek vysvětluje, jak použít třídu mapy Xamarin.Forms pomocí nativní mapování rozhraní API na každou platformu zajistit, že se že známým mapuje prostředí pro uživatele.
ms.prod: xamarin
ms.assetid: 59CD1344-8248-406C-9144-0C8A67141E5B
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 04/27/2016
ms.openlocfilehash: d74ad52a2926fb30a528aeba29156259390c3edf
ms.sourcegitcommit: 632955f8cdb80712abd8dcc30e046cb9c435b922
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/10/2018
ms.locfileid: "37947241"
---
# <a name="xamarinforms-map"></a>Mapa Xamarin.Forms

_Xamarin.Forms využívá nativní mapování rozhraní API na každou platformu._

Xamarin.Forms.Maps není úspěšný kvůli využívá nativní mapování rozhraní API na každou platformu. To poskytuje rychlý a dobře známý mapování prostředí pro uživatele, ale znamená, že některé kroky konfigurace je třeba splnit požadavky jednotlivých platforem konkrétní rozhraní API.
Po nakonfigurování `Map` ovládací prvek funguje stejně jako jakýkoli jiný Xamarin.Forms element společný kód.

* [Mapuje inicializace](#Maps_Initialization) – pomocí `Map` vyžaduje další inicializační kód při spuštění.
* [Konfigurace platformy](#Platform_Configuration) – jednotlivé platformy, vyžaduje určitou konfiguraci pro mapování pro práci.
* [Použití mapy v jazyce C#](#Using_Maps) – zobrazení mapy a připíná pomocí jazyka C#.
* [Použití mapy v XAML](#Using_Xaml) – zobrazení mapy s XAML.

Se použil mapový ovládací prvek [MapsSample](https://developer.xamarin.com/samples/WorkingWithMaps/) vzorku, který je uveden níže.

 [![Mapy v ukázce MobileCRM](map-images/maps-zoom-sml.png "příklad ovládací prvek mapy")](map-images/maps-zoom.png#lightbox "příklad ovládacího prvku mapy")

Můžete tak, že vytvoříte další rozšířené funkce mapy [namapovat vlastní zobrazovací jednotky](~/xamarin-forms/app-fundamentals/custom-renderer/map/index.md).

<a name="Maps_Initialization" />

## <a name="maps-initialization"></a>Inicializace mapy

Při přidávání mapy aplikace Xamarin.Forms **xamarin.Forms.Maps není úspěšný kvůli** je samostatném balíčku NuGet, měli byste přidat do jakéhokoliv projektu v řešení.
V Androidu to je také závislý na GooglePlayServices (jiné NuGet), kterou si můžete stáhnout automaticky při přidání xamarin.Forms.Maps není úspěšný kvůli.

Po instalaci balíčku NuGet, se vyžaduje kód inicializace v každém projektu aplikace *po* `Xamarin.Forms.Forms.Init` volání metody. Pro iOS pomocí následujícího kódu:

```csharp
Xamarin.FormsMaps.Init();
```

V systému Android je nutné předat stejné parametry jako `Forms.Init`:

```csharp
Xamarin.FormsMaps.Init(this, bundle);
```

Pro univerzální platformu Windows (UPW) pomocí následujícího kódu:

```csharp
Xamarin.FormsMaps.Init("INSERT_AUTHENTICATION_TOKEN_HERE");
```

Přidáte toto volání v následujících souborech pro každou platformu:

-  **iOS** -souboru AppDelegate.cs, `FinishedLaunching` metody.
-  **Android** -MainActivity.cs souboru `OnCreate` metody.
-  **UPW** -souboru MainPage.xaml.cs, `MainPage` konstruktoru.

Jakmile se přidal balíček NuGet a inicializační metoda volána v rámci každé applcation `Xamarin.Forms.Maps` slouží rozhraní API v běžných projekt knihovny .NET Standard nebo sdíleného projektu kódu.

<a name="Platform_Configuration" />

## <a name="platform-configuration"></a>Konfigurace platformy

Předtím, než se zobrazí na mapě, vyžadují se další konfigurační kroky na některých platformách.

### <a name="ios"></a>iOS

Pro přístup k umístění služby v systému iOS, je nutné nastavit následující klíče v **Info.plist**:

- iOS 11
    - [`NSLocationWhenInUseUsageDescription`](https://developer.apple.com/library/ios/documentation/General/Reference/InfoPlistKeyReference/Articles/CocoaKeys.html#//apple_ref/doc/uid/TP40009251-SW26) – pro pomocí zjišťování polohy, když se aplikace používá
    - [`NSLocationAlwaysAndWhenInUseUsageDescription`](https://developer.apple.com/documentation/corelocation/choosing_the_authorization_level_for_location_services/requesting_always_authorization?language=objc) – používání polohy po celou dobu
- iOS 10 a starší
    - [`NSLocationWhenInUseUsageDescription`](https://developer.apple.com/library/ios/documentation/General/Reference/InfoPlistKeyReference/Articles/CocoaKeys.html#//apple_ref/doc/uid/TP40009251-SW26) – pro pomocí zjišťování polohy, když se aplikace používá
    - [`NSLocationAlwaysUsageDescription`](https://developer.apple.com/library/ios/documentation/General/Reference/InfoPlistKeyReference/Articles/CocoaKeys.html#//apple_ref/doc/uid/TP40009251-SW18) – používání polohy po celou dobu    

Pro podporu iOS 11 a starší, může zahrnovat všechny tři klíče: `NSLocationWhenInUseUsageDescription`, `NSLocationAlwaysAndWhenInUseUsageDescription`, a `NSLocationAlwaysUsageDescription`.

Reprezentace XML pro tyto klíče v **Info.plist** je uveden níže. Měli byste aktualizovat `string` hodnoty tak, aby odrážely, jak vaše aplikace používá informace o poloze:

```xml
<key>NSLocationAlwaysUsageDescription</key>
<string>Can we use your location at all times?</string>
<key>NSLocationWhenInUseUsageDescription</key>
<string>Can we use your location when your app is being used?</string>
<key>NSLocationAlwaysAndWhenInUseUsageDescription</key>
<string>Can we use your location at all times?</string>
```

**Info.plist** položky lze přidat také v **zdroj** zobrazení při úpravách **Info.plist** souboru:

![Soubor info.plist pro iOS 8](map-images/ios8-map-permissions.png "iOS 8 požadovaných položek souboru Info.plist")


### <a name="android"></a>Android

Použít [Google Maps API v2](https://developers.google.com/maps/documentation/android/) na Androidu musí vygenerovat klíč rozhraní API a přidejte ji do vašeho projektu Android.
Postupujte podle pokynů v dokumentu Xamarin na [získání klíče rozhraní API služby mapy Google v2](~/android/platform/maps-and-location/maps/obtaining-a-google-maps-api-key.md).
Po provedení těchto pokynů, vložte klíč rozhraní API v **Properties/AndroidManifest.xml** soubor (zdroj zobrazení a hledání nebo aktualizovat následující element):

```xml
<application ...>
    <meta-data android:name="com.google.android.maps.v2.API_KEY" android:value="YOUR_API_KEY" />
</application>
```

Bez platný klíč rozhraní API se zobrazí ovládací prvek mapy jako šedou pole v Androidu.

> [!NOTE]
> Mějte na paměti, aby vaše soubory APK k mapy Google, je nutné zahrnout otisky prstů SHA-1 a balíček názvy pro každé úložiště klíčů (debug a release), který se používá k podepisování vaší APK. Například pokud použijete jeden počítač pro ladění a další počítače ke generování vydání APK, měli byste zahrnout otisk SHA-1 certifikátu z úložiště klíčů ladění prvního počítače a otisk SHA-1 certifikátu z úložiště klíčů verzi nástroje druhý počítač. Nezapomeňte také upravit klíč přihlašovací údaje, pokud aplikace **název balíčku** změny. Zobrazit [získání klíče rozhraní API služby mapy Google v2](~/android/platform/maps-and-location/maps/obtaining-a-google-maps-api-key.md).

Bude také nutné povolit příslušné oprávnění tak, že kliknete pravým tlačítkem na projekt pro Android a vyberete **možnosti > sestavení > aplikace pro Android** a zaškrtnutím příslušného políčka následující:

* `AccessCoarseLocation`
* `AccessFineLocation`
* `AccessLocationExtraCommands`
* `AccessMockLocation`
* `AccessNetworkState`
* `AccessWifiState`
* `Internet`

Na snímku obrazovky níže jsou uvedené některé z těchto:

![Požadovaná oprávnění pro Android](map-images/android-map-permissions.png "požadovaná oprávnění pro Android")

Poslední dva jsou povinné, protože aplikace vyžadují připojení k síti pro stahování dat na mapě. Přečtěte si o aplikaci Android [oprávnění](http://developer.android.com/reference/android/Manifest.permission.html) Další informace.

### <a name="universal-windows-platform"></a>Univerzální platforma pro Windows

Použití mapy na univerzální platformu Windows musí generovat autorizační token. Další informace najdete v tématu [požádat o ověřovací klíč map](https://msdn.microsoft.com/library/windows/apps/mt219694.aspx) na webové stránce MSDN.

Ověřovací token musí být potom zadán v `FormsMaps.Init("AUTHORIZATION_TOKEN")` volání metody k ověření aplikace mapách služby mapy Bing.

<a name="Using_Maps" />

## <a name="using-maps"></a>Pomocí mapy

Zobrazit [MapPage.cs](https://github.com/xamarin/xamarin-forms-samples/blob/master/MobileCRM/MobileCRM.Shared/Pages/MapPage.cs) v ukázce MobileCRM příklad použití mapového ovládacího prvku v kódu. Jednoduchý `MapPage` třídy může vypadat toto – oznámení, který nový `MapSpan` se vytvoří na pozici na mapě zobrazit:

```csharp
public class MapPage : ContentPage {
    public MapPage() {
        var map = new Map(
            MapSpan.FromCenterAndRadius(
                    new Position(37,-122), Distance.FromMiles(0.3))) {
                IsShowingUser = true,
                HeightRequest = 100,
                WidthRequest = 960,
                VerticalOptions = LayoutOptions.FillAndExpand
            };
        var stack = new StackLayout { Spacing = 0 };
        stack.Children.Add(map);
        Content = stack;
    }
}
```

### <a name="map-type"></a>Typ mapování

Obsah mapy lze také změnit tak, že nastavíte `MapType` vlastnost zobrazíte regulární ulice mapu (výchozí), satelitních snímků nebo kombinaci obojího.

```csharp
map.MapType == MapType.Street;
```

Platný `MapType` hodnoty jsou:

-  Hybridní
-  Satelit
-  Ulice (výchozí)


### <a name="map-region-and-mapspan"></a>Mapa oblastí a MapSpan

Jak je uvedeno ve výše uvedeném fragmentu kódu, zadávání `MapSpan` instance konstruktoru mapy nastaví počáteční zobrazení (na střed bodu a úroveň zvětšení) mapy při spuštění. `MoveToRegion` Metody ve třídě mapy je pak možné změnit úroveň pozice nebo Přiblížení mapy. Existují dva způsoby, jak vytvořit novou `MapSpan` instance:

-  **MapSpan.FromCenterAndRadius()** -statickou metodu pro vytvoření rozsahu ze `Position` a zadáte `Distance` .
-  **(nové MapSpan)** – konstruktor, který se používá `Position` a degress zeměpisné šířky a délky pro zobrazení.


Chcete-li změnit úroveň přiblížení mapy beze změny umístění, vytvořte nový `MapSpan` pomocí aktuální umístění, ze `VisibleRegion.Center` vlastnost mapového ovládacího prvku. A `Slider` může použít k řízení Přiblížení mapy takto (ale přiblížit mapový ovládací prvek nemůže aktualizovat aktuálně hodnotu posuvníku):

```csharp
var slider = new Slider (1, 18, 1);
slider.ValueChanged += (sender, e) => {
    var zoomLevel = e.NewValue; // between 1 and 18
    var latlongdegrees = 360 / (Math.Pow(2, zoomLevel));
    map.MoveToRegion(new MapSpan (map.VisibleRegion.Center, latlongdegrees, latlongdegrees));
};
```

 [![Mapy s přiblížení](map-images/maps-zoom-sml.png "Přiblížení mapy ovládacího prvku")](map-images/maps-zoom.png#lightbox "Přiblížení mapy ovládacího prvku")

### <a name="map-pins"></a>Špendlíky

Umístění je možné označit na mapě s `Pin` objekty.

```csharp
var position = new Position(37,-122); // Latitude, Longitude
var pin = new Pin {
            Type = PinType.Place,
            Position = position,
            Label = "custom pin",
            Address = "custom detail info"
        };
map.Pins.Add(pin);
```

 `PinType` můžete nastavit na jednu z následujících hodnot, které může mít vliv na způsob, jak kód pin v vykreslení (v závislosti na platformě):

-  Obecné
-  Místo
-  SavedPin
-  SearchResult


<a name="Using_Xaml" />

## <a name="using-xaml"></a>Pomocí jazyka Xaml

Maps může také umístěné v rozložení Xaml, jak je znázorněno v tomto fragmentu kódu.

```xaml
<?xml version="1.0" encoding="UTF-8" ?>
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
    xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
    xmlns:maps="clr-namespace:Xamarin.Forms.Maps;assembly=Xamarin.Forms.Maps"
    x:Class="MapDemo.MapPage">
    <StackLayout VerticalOptions="StartAndExpand" Padding="30">
        <maps:Map WidthRequest="320" HeightRequest="200"
            x:Name="MyMap"
            IsShowingUser="true"
            MapType="Hybrid"
        />
    </StackLayout>
</ContentPage>
```

`MapRegion` a `Pins` je možné nastavit v kódu pomocí `MyMap` odkaz (nebo cokoli, co má název mapy). Všimněte si, že další `xmlns` definice oboru názvů je vyžadovaný pro odkaz na ovládací prvky xamarin.Forms.Maps není úspěšný kvůli.

```csharp
MyMap.MoveToRegion(
    MapSpan.FromCenterAndRadius(
        new Position(37,-122), Distance.FromMiles(1)));
```

<a name="Summary" />

## <a name="summary"></a>Souhrn

Xamarin.Forms.Maps není úspěšný kvůli je samostatný NuGet, který musí být přidané do jednotlivých projektů v řešení Xamarin.Forms. Další inicializační kód je potřeba, a některé kroky konfigurace pro iOS, Android a UPW.

Jednou nakonfigurovaných rozhraní API pro mapy můžete použít k vykreslení mapy se značkami PIN kód v několika řádků kódu. Maps můžete dále rozšířit o další [vlastního rendereru](~/xamarin-forms/app-fundamentals/custom-renderer/map/index.md).


## <a name="related-links"></a>Související odkazy

- [MapsSample](https://developer.xamarin.com/samples/WorkingWithMaps/)
- [Mapování vlastního Rendereru](~/xamarin-forms/app-fundamentals/custom-renderer/map/index.md)
- [Ukázky Xamarin.Forms](https://developer.xamarin.com/samples/xamarin-forms/all/)
