---
title: Zvýraznění trasu na mapě
description: Tento článek vysvětluje postup přidání překrytí lomenou čáru na mapu. Překrytí lomenou čáru je řada spojených čar segmentů, které jsou obvykle používány trasu zobrazit na mapě nebo formuláře libovolného tvaru, které je nutné.
ms.prod: xamarin
ms.assetid: FBFDC715-1654-4188-82A0-FC522548BCFF
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 11/29/2017
ms.openlocfilehash: f781a472a63d97c8859aff36b28e0fd4fa0c7756
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/04/2018
---
# <a name="highlighting-a-route-on-a-map"></a>Zvýraznění trasu na mapě

_Tento článek vysvětluje postup přidání překrytí lomenou čáru na mapu. Překrytí lomenou čáru je řada spojených čar segmentů, které jsou obvykle používány trasu zobrazit na mapě nebo formuláře libovolného tvaru, které je nutné._

## <a name="overview"></a>Přehled

Překrytí je vrstveného grafika na mapě. Překryvy podporovat kreslení grafické obsah, který škáluje s mapy, jako je možnosti. Na následujících snímcích obrazovky zobrazit výsledkem přidání překrytí lomenou čáru na mapu:

![](polyline-map-overlay-images/screenshots.png)

Když [ `Map` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Maps.Map/) vykreslení ovládacího prvku platformě Xamarin.Forms aplikací v iOS `MapRenderer` vytvoření instance třídy, které pak vytvoří nativní `MKMapView` ovládacího prvku. Na platformě Android `MapRenderer` třída vytvoří nativní `MapView` ovládacího prvku. Na univerzální platformu Windows (UWP), `MapRenderer` třída vytvoří nativní `MapControl`. Proces vykreslování můžete provedeny výhod implementovat přizpůsobení specifické pro platformu mapy tak, že vytvoříte vlastní zobrazovací jednotky pro `Map` na každou platformu. Proces pro to vypadá takto:

1. [Vytvoření](#Creating_the_Custom_Map) Xamarin.Forms vlastní mapování.
1. [Využívat](#Consuming_the_Custom_Map) vlastní mapy z Xamarin.Forms.
1. [Přizpůsobení](#Customizing_the_Map) mapy tak, že vytvoříte vlastní zobrazovací jednotky pro mapu na každou platformu.

> [!NOTE]
> [`Xamarin.Forms.Maps`](https://developer.xamarin.com/api/namespace/Xamarin.Forms.Maps/) musí být inicializovaná a před použitím. Další informace najdete na webu [`Maps Control`](~/xamarin-forms/user-interface/map.md).

Informace o přizpůsobení mapu pomocí vlastní zobrazovací jednotky najdete v tématu [přizpůsobení Map kódu Pin](~/xamarin-forms/app-fundamentals/custom-renderer/map/customized-pin.md).

<a name="Creating_the_Custom_Map" />

### <a name="creating-the-custom-map"></a>Vytváření vlastních mapy

Vytvoření podtřídou třídy [ `Map` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Maps.Map/) třída, která přidává `RouteCoordinates` vlastnost:

```csharp
public class CustomMap : Map
{
  public List<Position> RouteCoordinates { get; set; }

  public CustomMap ()
  {
    RouteCoordinates = new List<Position> ();
  }
}
```

`RouteCoordinates` Vlastnost uloží kolekce souřadnic, které definují trasy, která má mít zvýrazněná.

<a name="Consuming_the_Custom_Map" />

### <a name="consuming-the-custom-map"></a>Použití vlastní mapy

Využívat `CustomMap` řízení deklarováním její instanci v instanci stránky XAML:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:local="clr-namespace:MapOverlay;assembly=MapOverlay"
             x:Class="MapOverlay.MapPage">
    <ContentPage.Content>
        <local:CustomMap x:Name="customMap" MapType="Street" WidthRequest="{x:Static local:App.ScreenWidth}" HeightRequest="{x:Static local:App.ScreenHeight}" />
    </ContentPage.Content>
</ContentPage>
```

Alternativně využívat `CustomMap` řízení deklarováním její instanci v instanci stránky C#:

```csharp
public class MapPageCS : ContentPage
{
    public MapPageCS ()
    {
        var customMap = new CustomMap {
            MapType = MapType.Street,
            WidthRequest = App.ScreenWidth,
            HeightRequest = App.ScreenHeight
        };
        ...
        Content = customMap;
    }
}
```

Inicializace `CustomMap` řízení podle potřeby:

```csharp
public partial class MapPage : ContentPage
{
  public MapPage ()
  {
    ...
    customMap.RouteCoordinates.Add (new Position (37.785559, -122.396728));
    customMap.RouteCoordinates.Add (new Position (37.780624, -122.390541));
    customMap.RouteCoordinates.Add (new Position (37.777113, -122.394983));
    customMap.RouteCoordinates.Add (new Position (37.776831, -122.394627));

    customMap.MoveToRegion (MapSpan.FromCenterAndRadius (new Position (37.79752, -122.40183), Distance.FromMiles (1.0)));
  }
}
```

Tato inicializace určuje řadu zeměpisnou šířku a délku souřadnice k definování trasy na mapu, která bude mít zvýrazněná. Ji pak umisťuje zobrazení mapy na s [ `MoveToRegion` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Maps.Map.MoveToRegion(Xamarin.Forms.Maps.MapSpan)/) metoda, která mění pozice a úroveň přiblížení mapy vytvořením [ `MapSpan` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Maps.MapSpan/) z [ `Position` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Maps.Position/) a [ `Distance` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Maps.Distance/).

<a name="Customizing_the_Map" />

### <a name="customizing-the-map"></a>Přizpůsobení mapy

Vlastní zobrazovací jednotky je nyní přidat na každý projekt aplikace pro přidání do překrytí lomenou čáru na mapě.

#### <a name="creating-the-custom-renderer-on-ios"></a>Vytváření vlastní zobrazovací jednotky v systému iOS

Vytvoření podtřídou třídy `MapRenderer` třídy a přepsat její `OnElementChanged` metoda pro přidání do překrytí lomenou čáru:

```csharp
[assembly: ExportRenderer(typeof(CustomMap), typeof(CustomMapRenderer))]
namespace MapOverlay.iOS
{
    public class CustomMapRenderer : MapRenderer
    {
        MKPolylineRenderer polylineRenderer;

        protected override void OnElementChanged(ElementChangedEventArgs<View> e)
        {
            base.OnElementChanged(e);

            if (e.OldElement != null) {
                var nativeMap = Control as MKMapView;
                if (nativeMap != null) {
                    nativeMap.RemoveOverlays(nativeMap.Overlays);
                    nativeMap.OverlayRenderer = null;
                    polylineRenderer = null;
                }
            }

            if (e.NewElement != null) {
                var formsMap = (CustomMap)e.NewElement;
                var nativeMap = Control as MKMapView;
                nativeMap.OverlayRenderer = GetOverlayRenderer;

                CLLocationCoordinate2D[] coords = new CLLocationCoordinate2D[formsMap.RouteCoordinates.Count];
                int index = 0;
                foreach (var position in formsMap.RouteCoordinates)
                {
                    coords[index] = new CLLocationCoordinate2D(position.Latitude, position.Longitude);
                    index++;
                }

                var routeOverlay = MKPolyline.FromCoordinates(coords);
                nativeMap.AddOverlay(routeOverlay);
            }
        }
        ...
    }
}

```

Za předpokladu, že vlastní zobrazovací jednotky je připojen k nového elementu Xamarin.Forms, tato metoda provádí následující konfiguraci:

- `MKMapView.OverlayRenderer` Je nastavena na odpovídající delegáta.
- Kolekce zeměpisné šířky a délky jsou načteny z `CustomMap.RouteCoordinates` vlastnost a uloží jako pole `CLLocationCoordinate2D` instance.
- Vytvoření lomenou čáru voláním statické `MKPolyline.FromCoordinates` metoda, která určuje zeměpisnou šířku a délku každého bodu.
- Lomenou čáru je přidán do mapy voláním `MKMapView.AddOverlay` metoda.

Potom implementovat `GetOverlayRenderer` metodu za účelem přizpůsobení vykreslování překrytí:

```csharp
public class CustomMapRenderer : MapRenderer
{
  MKPolylineRenderer polylineRenderer;
  ...

  MKOverlayRenderer GetOverlayRenderer(MKMapView mapView, IMKOverlay overlayWrapper)
  {
      if (polylineRenderer == null && !Equals(overlayWrapper, null)) {
          var overlay = Runtime.GetNSObject(overlayWrapper.Handle) as IMKOverlay;
          polylineRenderer = new MKPolylineRenderer(overlay as MKPolyline) {
              FillColor = UIColor.Blue,
              StrokeColor = UIColor.Red,
              LineWidth = 3,
              Alpha = 0.4f
          };
      }
      return polylineRenderer;
  }
}
```

#### <a name="creating-the-custom-renderer-on-android"></a>Vytváření vlastní zobrazovací jednotky v systému Android

Vytvoření podtřídou třídy `MapRenderer` třídy a přepsat její `OnElementChanged` a `OnMapReady` metody pro přidání do překrytí lomenou čáru:

```csharp
[assembly: ExportRenderer(typeof(CustomMap), typeof(CustomMapRenderer))]
namespace MapOverlay.Droid
{
    public class CustomMapRenderer : MapRenderer
    {
        List<Position> routeCoordinates;

        public CustomMapRenderer(Context context) : base(context)
        {
        }

        protected override void OnElementChanged(Xamarin.Forms.Platform.Android.ElementChangedEventArgs<Map> e)
        {
            base.OnElementChanged(e);

            if (e.OldElement != null)
            {
                // Unsubscribe
            }

            if (e.NewElement != null)
            {
                var formsMap = (CustomMap)e.NewElement;
                routeCoordinates = formsMap.RouteCoordinates;
                Control.GetMapAsync(this);
            }
        }

        protected override void OnMapReady(Android.Gms.Maps.GoogleMap map)
        {
            base.OnMapReady(map);

            var polylineOptions = new PolylineOptions();
            polylineOptions.InvokeColor(0x66FF0000);

            foreach (var position in routeCoordinates)
            {
                polylineOptions.Add(new LatLng(position.Latitude, position.Longitude));
            }

            NativeMap.AddPolyline(polylineOptions);
        }
    }
}
```

`OnElementChanged` Metoda načte kolekci zeměpisných souřadnic ze `CustomMap.RouteCoordinates` vlastnost a ukládá je v členské proměnné. Potom zavolá `MapView.GetMapAsync` metodu, která získá základní `GoogleMap` který je vázaný na zobrazení, za předpokladu, že vlastní zobrazovací jednotky je připojen k nového elementu Xamarin.Forms. Jednou `GoogleMap` instance je k dispozici, `OnMapReady` metoda bude vyvolán, kde se má vytvořit čáru po vytvoření instance `PolylineOptions` objekt, který určuje zeměpisnou šířku a délku každého bodu. Čáru se pak přidá do mapy voláním `NativeMap.AddPolyline` metoda.

#### <a name="creating-the-custom-renderer-on-the-universal-windows-platform"></a>Vytváření vlastní zobrazovací jednotky na univerzální platformu Windows

Vytvoření podtřídou třídy `MapRenderer` třídy a přepsat její `OnElementChanged` metoda pro přidání do překrytí lomenou čáru:

```csharp
[assembly: ExportRenderer(typeof(CustomMap), typeof(CustomMapRenderer))]
namespace MapOverlay.UWP
{
    public class CustomMapRenderer : MapRenderer
    {
        protected override void OnElementChanged(ElementChangedEventArgs<Map> e)
        {
            base.OnElementChanged(e);

            if (e.OldElement != null)
            {
                // Unsubscribe
            }

            if (e.NewElement != null)
            {
                var formsMap = (CustomMap)e.NewElement;
                var nativeMap = Control as MapControl;

                var coordinates = new List<BasicGeoposition>();
                foreach (var position in formsMap.RouteCoordinates)
                {
                    coordinates.Add(new BasicGeoposition() { Latitude = position.Latitude, Longitude = position.Longitude });
                }

                var polyline = new MapPolyline();
                polyline.StrokeColor = Windows.UI.Color.FromArgb(128, 255, 0, 0);
                polyline.StrokeThickness = 5;
                polyline.Path = new Geopath(coordinates);
                nativeMap.MapElements.Add(polyline);
            }
        }
    }
}
```

Tato metoda provede následující operace, za předpokladu, že vlastní zobrazovací jednotky je připojen k nového elementu Xamarin.Forms:

- Kolekce zeměpisné šířky a délky jsou načteny z `CustomMap.RouteCoordinates` vlastnost a převedený do `List` z `BasicGeoposition` souřadnice.
- Po vytvoření instance se vytvoří lomenou čáru `MapPolyline` objektu. `MapPolygon` Třída se používá pro zobrazení průběhu na mapě nastavením jeho `Path` vlastnosti `Geopath` objekt, který obsahuje souřadnice řádku.
- Vykreslení lomenou čáru na mapě přidáním jeho `MapControl.MapElements` kolekce.

## <a name="summary"></a>Souhrn

Tento článek vysvětluje postup přidání překrytí lomenou čáru na mapu, můžete zobrazit na mapě trasu nebo formuláři libovolného tvaru, které je nutné.


## <a name="related-links"></a>Související odkazy

- [Mapa Ovlerlay lomenou čáru (ukázka)](https://developer.xamarin.com/samples/xamarin-forms/customrenderers/map/polyline/)
- [Přizpůsobení špendlíku mapy](~/xamarin-forms/app-fundamentals/custom-renderer/map/customized-pin.md)
- [Xamarin.Forms.Maps](https://developer.xamarin.com/api/namespace/Xamarin.Forms.Maps/)
