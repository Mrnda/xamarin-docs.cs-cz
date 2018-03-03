---
title: mapy
description: "Xamarin.Forms používá nativní mapy rozhraní API na každou platformu."
ms.topic: article
ms.prod: xamarin
ms.assetid: 22288ABF-57BE-47A9-ACC3-AC604D787C46
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 04/27/2016
ms.openlocfilehash: 98e6cade952e656046c6c0981a0b73ff0894c9d6
ms.sourcegitcommit: 61f5ecc5a2b5dcfbefdef91664d7460c0ee2f357
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 02/28/2018
---
# <a name="map"></a>mapy

_Xamarin.Forms používá nativní mapy rozhraní API na každou platformu._

Xamarin.Forms.Maps používá nativní mapy rozhraní API na každou platformu. To nabízí rychlý, známé mapy prostředí pro uživatele, ale znamená, že některé věci řídit specifické požadavky rozhraní API jednotlivých platforem.
Jednou nakonfigurovaná, `Map` řízení funguje stejně jako jakýkoli jiný Xamarin.Forms element společný kód.

* [Mapuje inicializace](#Maps_Initialization) – pomocí `Map` vyžaduje další inicializační kód při spuštění.
* [Konfigurace platformy](#Platform_Configuration) -každou platformu vyžaduje konfiguraci pro službu maps pracovat.
* [V jazyce C# pomocí map](#Using_Maps) -zobrazení mapy a PIN kódy pomocí jazyka C#.
* [Pomocí map v jazyce XAML](#Using_Xaml) -zobrazení mapy s XAML.

Používá se mapový ovládací prvek v [MapsSample](https://developer.xamarin.com/samples/WorkingWithMaps/) vzorku, který je uveden níže.

 [ ![Mapy v ukázce MobileCRM](map-images/maps-zoom-sml.png "Příklad mapy ovládacího prvku")](map-images/maps-zoom.png "Příklad mapy ovládacího prvku")

Mapování funkce může dále zvýšit tak, že vytvoříte [namapovat vlastní zobrazovací jednotky](~/xamarin-forms/app-fundamentals/custom-renderer/map/index.md).

<a name="Maps_Initialization" />

## <a name="maps-initialization"></a>Inicializace mapy

Při přidávání mapování pro aplikaci Xamarin.Forms, **Xamarin.Forms.Maps** je samostatný balíček NuGet, měli byste přidat na všechny projekty v řešení.
V systému Android to je také závislý na GooglePlayServices (jiné NuGet), kterou si můžete stáhnout automaticky při přidání Xamarin.Forms.Maps.

Po instalaci balíčku NuGet, některé inicializace kód není nutný v každém projektu aplikace *po* `Xamarin.Forms.Forms.Init` volání metody. Pro iOS použijte následující kód:

```csharp
Xamarin.FormsMaps.Init();
```

V systému Android musí projít stejné parametry jako `Forms.Init`:

```csharp
Xamarin.FormsMaps.Init(this, bundle);
```

Pro prostředí Windows Runtime (WinRT) a univerzální platformu Windows (UWP) použijte následující kód:

```csharp
Xamarin.FormsMaps.Init("INSERT_AUTHENTICATION_TOKEN_HERE");
```

Přidejte toto volání v následujících souborech pro každou platformu:

-  **iOS** -AppDelegate.cs v souboru `FinishedLaunching` metoda.
-  **Android** -MainActivity.cs v souboru `OnCreate` metoda.
-  **WinRT a UWP** -souboru MainPage.xaml.cs, `MainPage` konstruktor.

Jakmile se přidal balíček NuGet a inicializační metoda volána v rámci každé applcation `Xamarin.Forms.Maps` rozhraní API mohou být používány společný kód PCL nebo sdílený projekt.

<a name="Platform_Configuration" />

## <a name="platform-configuration"></a>Konfigurace platformy

Na některých platformách jsou zapotřebí další konfigurační kroky, než se zobrazí mapy.

### <a name="ios"></a>iOS

V systému iOS 7 mapy řízení "jenom funguje", za předpokladu jako `FormsMaps.Init()` bylo provedeno volání.

Pro iOS 8 dva klíče musí být přidán do **Info.plist** souboru: [ `NSLocationAlwaysUsageDescription` ](https://developer.apple.com/library/ios/documentation/General/Reference/InfoPlistKeyReference/Articles/CocoaKeys.html#//apple_ref/doc/uid/TP40009251-SW18) a [ `NSLocationWhenInUseUsageDescription` ](https://developer.apple.com/library/ios/documentation/General/Reference/InfoPlistKeyReference/Articles/CocoaKeys.html#//apple_ref/doc/uid/TP40009251-SW26). Reprezentace XML je zobrazena níže – by měl aktualizovat `string` hodnoty tak, aby odrážela, jak vaše aplikace používá informace o umístění:

```xml
<key>NSLocationAlwaysUsageDescription</key>
    <string>Can we use your location</string>
<key>NSLocationWhenInUseUsageDescription</key>
    <string>We are using your location</string>
```

**Info.plist** položky můžete přidat i v **zdroj** zobrazení při úpravách **Info.plist** souboru:

![Info.plist pro iOS 8](map-images/ios8-map-permissions.png "položky Info.plist vyžaduje iOS 8")


### <a name="android"></a>Android

Použít [Google Maps API v2](https://developers.google.com/maps/documentation/android/) v systému Android musí vygenerovat klíč rozhraní API a přidejte ji do vašeho projektu Android.
Postupujte podle pokynů v dokumentu Xamarin [získání klíč Google Maps API v2](~/android/platform/maps-and-location/maps/obtaining-a-google-maps-api-key.md).
Až projdete tyto pokyny, vložte klíč rozhraní API v **Properties/AndroidManifest.xml** souboru (zobrazení zdroje a najít nebo aktualizovat následující element):

```xml
<meta-data android:name="com.google.android.maps.v2.API_KEY"
            android:value="AbCdEfGhIjKlMnOpQrStUvWValueGoesHere" />
```

Bez platný klíč rozhraní API ovládacího prvku mapy zobrazí jako šedé pole v systému Android.

> [!NOTE]
> **Poznámka:**: nezapomeňte generovat jiný klíč pomocí souboru úložiště klíčů, který se používá k podepisování prodejní verze nástroje jakékoli aplikace, která je načtený do obchodu Google Play. Klíč vygenerujete pro vývoj a ladění nebude fungovat a bude obsahovat aplikaci stáhnout z webu Google Play nefunkční zobrazení mapy. Nezapomeňte také znovu vygenerovat klíče Pokud aplikace **název balíčku** změny.

Musíte také povolit příslušná oprávnění kliknutím pravým tlačítkem myši na projekt pro Android a výběrem **možnosti > sestavení > aplikace pro Android** a tikání následující:

* `AccessCoarseLocation`
* `AccessFineLocation`
* `AccessLocationExtraCommands`
* `AccessMockLocation`
* `AccessNetworkState`
* `AccessWifiState`
* `Internet`

Na snímku obrazovky níže jsou uvedeny některé z těchto:

![Požadovaná oprávnění pro Android](map-images/android-map-permissions.png "požadovaných oprávnění pro Android")

Poslední dva jsou požadovány, protože aplikace vyžaduje síťové připojení ke stahování dat mapy. Přečtěte si informace o Android [oprávnění](http://developer.android.com/reference/android/Manifest.permission.html) Další informace.

### <a name="windows-runtime-and-universal-windows-platform"></a>Prostředí Windows Runtime a univerzální platformu Windows

Chcete-li použít mapy v prostředí Windows Runtime a univerzální platformu Windows musíte vygenerovat autorizační token. Další informace najdete v tématu [žádostí ověřovací klíč mapy](https://msdn.microsoft.com/library/windows/apps/mt219694.aspx) na webu MSDN.

Ověřovací token musí být zadán pak v `FormsMaps.Init("AUTHORIZATION_TOKEN")` volání metody k ověření aplikace pomocí mapy Bing.

<a name="Using_Maps" />

## <a name="using-maps"></a>Pomocí map

Najdete v článku [MapPage.cs](https://github.com/xamarin/xamarin-forms-samples/blob/master/MobileCRM/MobileCRM.Shared/Pages/MapPage.cs) v ukázce MobileCRM příklad použití mapový ovládací prvek v kódu. Jednoduchý `MapPage` třída může vypadat například tohoto – upozornění, nový `MapSpan` se vytvoří na pozici zobrazení mapy na:

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

### <a name="map-type"></a>Typ mapy

Mapa obsahu může také změnit nastavení `MapType` vlastnost, k zobrazení regulární silniční mapa (výchozí), satelitních snímků nebo jejich kombinaci.

```csharp
map.MapType == MapType.Street;
```

Platný `MapType` hodnoty jsou:

-  Hybridní
-  Satelitní
-  Ulice (výchozí)


### <a name="map-region-and-mapspan"></a>Oblasti map a MapSpan

Jak je vidět ve výše uvedeném fragmentu kódu, zadávání `MapSpan` instance do konstruktoru mapy nastaví počáteční zobrazení (center bodu a úroveň přiblížení) mapy, když je načten. `MoveToRegion` Metoda u třídy map pak lze změnit úroveň pozici nebo Přiblížení mapy. Existují dva způsoby, jak vytvořit nový `MapSpan` instance:

-  **MapSpan.FromCenterAndRadius()** -statickou metodu pro vytvoření rozsahu ze `Position` a zadání `Distance` .
-  **nové (MapSpan)** – konstruktor, který používá `Position` a degress zeměpisné šířky a délky k zobrazení.


Pokud chcete změnit úroveň přiblížení mapy beze změny umístění, vytvořte novou `MapSpan` pomocí aktuálního umístění, ze `VisibleRegion.Center` vlastnost mapový ovládací prvek. A `Slider` může použít k řízení Přiblížení mapy takto (ale přiblížení a oddálení přímo v mapový ovládací prvek nemůže aktualizovat aktuálně hodnotu jezdce):

```csharp
var slider = new Slider (1, 18, 1);
slider.ValueChanged += (sender, e) => {
    var zoomLevel = e.NewValue; // between 1 and 18
    var latlongdegrees = 360 / (Math.Pow(2, zoomLevel));
    map.MoveToRegion(new MapSpan (map.VisibleRegion.Center, latlongdegrees, latlongdegrees));
};
```

 [ ![Mapy s přiblížení](map-images/maps-zoom-sml.png "Přiblížení mapy ovládacího prvku")](map-images/maps-zoom.png "Přiblížení mapy ovládacího prvku")

### <a name="map-pins"></a>Mapování kódů PIN

Umístění může být označen na mapě s `Pin` objekty.

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

 `PinType` může být nastavené na jednu z následujících hodnot, které mohou ovlivnit způsob kódu pin v vykreslení (v závislosti na platformě):

-  Obecné
-  Místní
-  SavedPin
-  SearchResult


<a name="Using_Xaml" />

## <a name="using-xaml"></a>Použitím jazyka Xaml

Mapy mohou být umístěny v Xaml rozložení také, jak ukazuje tento fragment kódu.

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

`MapRegion` a `Pins` lze nastavit v kódu pomocí `MyMap` odkaz (nebo ať mapy jmenuje). Všimněte si, že další `xmlns` tak, aby odkazovaly ovládací prvky Xamarin.Forms.Maps je vyžadována definice oboru názvů.

```csharp
MyMap.MoveToRegion(
    MapSpan.FromCenterAndRadius(
        new Position(37,-122), Distance.FromMiles(1)));
```

<a name="Summary" />

## <a name="summary"></a>Souhrn

Xamarin.Forms.Maps je samostatný NuGet, který musí být přidaný do jednotlivých projektů v řešení Xamarin.Forms. Je požadován, jako i některé kroky konfigurace pro iOS, Android, WinRT a UWP další inicializační kód.

Jednou nakonfigurovaných rozhraní API map můžete použít k vykreslení maps se značkami PIN kód v několika řádků kódu. Mapy lze dále rozšířit o [vlastní zobrazovací jednotky](~/xamarin-forms/app-fundamentals/custom-renderer/map/index.md).


## <a name="related-links"></a>Související odkazy

- [MapsSample](https://developer.xamarin.com/samples/WorkingWithMaps/)
- [Vlastní zobrazovací jednotky mapy](~/xamarin-forms/app-fundamentals/custom-renderer/map/index.md)
- [Ukázky Xamarin.Forms](https://developer.xamarin.com/samples/xamarin-forms/all/)
