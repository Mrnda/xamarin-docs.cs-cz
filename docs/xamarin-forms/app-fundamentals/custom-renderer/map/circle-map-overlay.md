---
title: Zvýraznění kruhové oblasti na mapě
description: Tento článek vysvětluje, jak přidat kruhové překrytí k mapě, abyste měli na očích kruhové oblasti na mapě. Zařízení s iOS a Android nabízejí rozhraní API pro přidání do kruhové překrytí do mapy, překrytí na UPW vykreslen jako mnohoúhelníku.
ms.prod: xamarin
ms.assetid: 6FF8BD15-074E-4E6A-9522-F9E2BE32EF12
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 11/29/2017
ms.openlocfilehash: 3064296d4c78a3342fb27afc971c37a029987e5e
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/12/2018
ms.locfileid: "38998554"
---
# <a name="highlighting-a-circular-area-on-a-map"></a>Zvýraznění kruhové oblasti na mapě

_Tento článek vysvětluje, jak přidat kruhové překrytí k mapě, abyste měli na očích kruhové oblasti na mapě._

## <a name="overview"></a>Přehled

Překrytí je vrstvený grafiky na mapě. Překryvy podporují výkresu grafického obsahu, která se škáluje s mapou, jak je zvětšeno. Na následujících snímcích obrazovky zobrazit výsledek přidání kruhové překrytí do mapy:

![](circle-map-overlay-images/screenshots.png)

Když [ `Map` ](xref:Xamarin.Forms.Maps.Map) aplikací Xamarin.Forms v Iosu se vykreslí ovládací prvek `MapRenderer` je vytvořena instance třídy, které pak vytvoří instanci nativní `MKMapView` ovládacího prvku. Na platformu Android `MapRenderer` třídy vytvoří instanci nativní `MapView` ovládacího prvku. Na Universal Windows Platform (UWP), `MapRenderer` třídy vytvoří instanci nativní `MapControl`. Samotný proces vykreslování můžete třeba využít implementovat přizpůsobení specifické pro platformu mapování tak, že vytvoříte vlastní zobrazovací jednotky pro `Map` na jednotlivých platformách. Tento proces je následujícím způsobem:

1. [Vytvoření](#Creating_the_Custom_Map) vlastní mapa Xamarin.Forms.
1. [Využívání](#Consuming_the_Custom_Map) vlastní mapy z Xamarin.Forms.
1. [Přizpůsobení](#Customizing_the_Map) mapování tak, že vytvoříte vlastní zobrazovací jednotky pro mapování na jednotlivých platformách.

> [!NOTE]
> [`Xamarin.Forms.Maps`](xref:Xamarin.Forms.Maps) musí být inicializován a před použitím nakonfigurovat. Další informace najdete v tématu [`Maps Control`](~/xamarin-forms/user-interface/map.md)

Informace o přizpůsobení mapy pomocí vlastní zobrazovací jednotky najdete v tématu [přizpůsobení špendlíku mapy](~/xamarin-forms/app-fundamentals/custom-renderer/map/customized-pin.md).

<a name="Creating_the_Custom_Map" />

### <a name="creating-the-custom-map"></a>Vytváří se vlastní mapa

Vytvoření `CustomCircle` třídu, která má `Position` a `Radius` vlastnosti:

```csharp
public class CustomCircle
{
  public Position Position { get; set; }
  public double Radius { get; set; }
}
```

Potom vytvořte podtřídu [ `Map` ](xref:Xamarin.Forms.Maps.Map) třída, která přidá vlastnost typu `CustomCircle`:

```csharp
public class CustomMap : Map
{
  public CustomCircle Circle { get; set; }
}
```

<a name="Consuming_the_Custom_Map" />

### <a name="consuming-the-custom-map"></a>Použití vlastních Map

Využívat `CustomMap` ovládací prvek deklarováním její instanci v instanci stránky XAML:

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

Můžete také využívat `CustomMap` ovládací prvek deklarováním její instanci v instanci stránky jazyka C#:

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

Inicializovat `CustomMap` ovládací prvek podle potřeby:

```csharp
public partial class MapPage : ContentPage
{
  public MapPage ()
  {
    ...
    var pin = new Pin {
      Type = PinType.Place,
      Position = new Position (37.79752, -122.40183),
      Label = "Xamarin San Francisco Office",
      Address = "394 Pacific Ave, San Francisco CA"
    };

    var position = new Position (37.79752, -122.40183);
    customMap.Circle = new CustomCircle {
      Position = position,
      Radius = 1000
    };

    customMap.Pins.Add (pin);
    customMap.MoveToRegion (MapSpan.FromCenterAndRadius (position, Distance.FromMiles (1.0)));
  }
}
```

Tato inicializace přidá [ `Pin` ](xref:Xamarin.Forms.Maps.Pin) a `CustomCircle` instance do vlastní mapy a umístí zobrazení na mapě s [ `MoveToRegion` ](xref:Xamarin.Forms.Maps.Map.MoveToRegion*) metoda, která změní pozice a přiblížení úrovně mapy tak, že vytvoříte [ `MapSpan` ](xref:Xamarin.Forms.Maps.MapSpan) z [ `Position` ](xref:Xamarin.Forms.Maps.Position) a [ `Distance` ](xref:Xamarin.Forms.Maps.Distance).

<a name="Customizing_the_Map" />

### <a name="customizing-the-map"></a>Přizpůsobení mapy

Vlastní zobrazovací jednotky musí nyní přidán do každého projektu aplikace pro přidání do kruhové překrytí do mapy.

#### <a name="creating-the-custom-renderer-on-ios"></a>Vytvoření vlastní zobrazovací jednotky v systému iOS

Vytvořit podtřídu `MapRenderer` třídy a přepsat její `OnElementChanged` metoda pro přidání do kruhové překrytí:

```csharp
[assembly: ExportRenderer(typeof(CustomMap), typeof(CustomMapRenderer))]
namespace MapOverlay.iOS
{
    public class CustomMapRenderer : MapRenderer
    {
        MKCircleRenderer circleRenderer;

        protected override void OnElementChanged(ElementChangedEventArgs<View> e)
        {
            base.OnElementChanged(e);

            if (e.OldElement != null) {
                var nativeMap = Control as MKMapView;
                if (nativeMap != null) {
                    nativeMap.RemoveOverlays(nativeMap.Overlays);
                    nativeMap.OverlayRenderer = null;
                    circleRenderer = null;
                }
            }

            if (e.NewElement != null) {
                var formsMap = (CustomMap)e.NewElement;
                var nativeMap = Control as MKMapView;
                var circle = formsMap.Circle;

                nativeMap.OverlayRenderer = GetOverlayRenderer;

                var circleOverlay = MKCircle.Circle(new CoreLocation.CLLocationCoordinate2D(circle.Position.Latitude, circle.Position.Longitude), circle.Radius);
                nativeMap.AddOverlay(circleOverlay);
            }
        }
        ...
    }
}

```

Za předpokladu, že vlastní zobrazovací jednotky je připojen k nový prvek Xamarin.Forms, tato metoda provádí následující konfiguraci:

- `MKMapView.OverlayRenderer` Je nastavena na odpovídající delegáta.
- Kruhu je vytvořen nastavením statickou `MKCircle` objekt, který určuje v metrech střed kruhu a poloměr kruhu.
- Kruh se přidá do mapy voláním `MKMapView.AddOverlay` metody.

Pak implementovat `GetOverlayRenderer` metodu za účelem přizpůsobení vykreslování překrytí:

```csharp
public class CustomMapRenderer : MapRenderer
{
  MKCircleRenderer circleRenderer;
  ...

  MKOverlayRenderer GetOverlayRenderer(MKMapView mapView, IMKOverlay overlayWrapper)
  {
      if (circleRenderer == null && !Equals(overlayWrapper, null)) {
          var overlay = Runtime.GetNSObject(overlayWrapper.Handle) as IMKOverlay;
          circleRenderer = new MKCircleRenderer(overlay as MKCircle) {
              FillColor = UIColor.Red,
              Alpha = 0.4f
          };
      }
      return circleRenderer;
  }
}
```

#### <a name="creating-the-custom-renderer-on-android"></a>Vytvoření vlastního Rendereru v Androidu

Vytvořit podtřídu `MapRenderer` třídy a přepsat její `OnElementChanged` a `OnMapReady` metody pro přidání do kruhové překrytí:

```csharp
[assembly: ExportRenderer(typeof(CustomMap), typeof(CustomMapRenderer))]
namespace MapOverlay.Droid
{
    public class CustomMapRenderer : MapRenderer
    {
        CustomCircle circle;

        public CustomMapRenderer(Context context) : base(context)
        {
        }

        protected override void OnElementChanged(Xamarin.Forms.Platform.Android.ElementChangedEventArgs<Xamarin.Forms.Maps.Map> e)
        {
            base.OnElementChanged(e);

            if (e.OldElement != null)
            {
                // Unsubscribe
            }

            if (e.NewElement != null)
            {
                var formsMap = (CustomMap)e.NewElement;
                circle = formsMap.Circle;
                Control.GetMapAsync(this);
            }
        }

        protected override void OnMapReady(Android.Gms.Maps.GoogleMap map)
        {
            base.OnMapReady(map);

            var circleOptions = new CircleOptions();
            circleOptions.InvokeCenter(new LatLng(circle.Position.Latitude, circle.Position.Longitude));
            circleOptions.InvokeRadius(circle.Radius);
            circleOptions.InvokeFillColor(0X66FF0000);
            circleOptions.InvokeStrokeColor(0X66FF0000);
            circleOptions.InvokeStrokeWidth(0);

            NativeMap.AddCircle(circleOptions);
        }
    }
}
```

`OnElementChanged` Volání metod `MapView.GetMapAsync` metodu, která získá základní `GoogleMap` , který se váže k zobrazení, za předpokladu, že vlastní zobrazovací jednotky je připojen k nový prvek Xamarin.Forms. Jednou `GoogleMap` instance je k dispozici, `OnMapReady` metoda bude volána, kde se vytvoří kruhu po vytvoření instance `CircleOptions` objekt, který určuje v metrech střed kruhu a poloměr kruhu. Kruh se pak přidá do mapy voláním `NativeMap.AddCircle` metody.

#### <a name="creating-the-custom-renderer-on-the-universal-windows-platform"></a>Vytvoření vlastního Rendereru na Universal Windows Platform

Vytvořit podtřídu `MapRenderer` třídy a přepsat její `OnElementChanged` metoda pro přidání do kruhové překrytí:

```csharp
[assembly: ExportRenderer(typeof(CustomMap), typeof(CustomMapRenderer))]
namespace MapOverlay.UWP
{
    public class CustomMapRenderer : MapRenderer
    {
        const int EarthRadiusInMeteres = 6371000;

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
                var circle = formsMap.Circle;

                var coordinates = new List<BasicGeoposition>();
                var positions = GenerateCircleCoordinates(circle.Position, circle.Radius);
                foreach (var position in positions)
                {
                    coordinates.Add(new BasicGeoposition { Latitude = position.Latitude, Longitude = position.Longitude });
                }

                var polygon = new MapPolygon();
                polygon.FillColor = Windows.UI.Color.FromArgb(128, 255, 0, 0);
                polygon.StrokeColor = Windows.UI.Color.FromArgb(128, 255, 0, 0);
                polygon.StrokeThickness = 5;
                polygon.Path = new Geopath(coordinates);
                nativeMap.MapElements.Add(polygon);
            }
        }
        // GenerateCircleCoordinates helper method (below)
    }
}
```

Za předpokladu, že vlastní zobrazovací jednotky je připojen k nový prvek Xamarin.Forms, tato metoda provádí následující operace:

- Pozice kruhu a protokolu radius se načítají z `CustomMap.Circle` vlastnost a předat `GenerateCircleCoordinates` metodu, která generuje zeměpisné šířky a délky souřadnice obvod kruhu. Kód pro tuto metodu helper najdete níž.
- Souřadnice obvod kruhu se převedou na `List` z `BasicGeoposition` souřadnice.
- Po vytvoření instance je vytvořena na kruh `MapPolygon` objektu. `MapPolygon` Třída se používá k zobrazení více bodů tvar na mapě nastavením jeho `Path` vlastnost `Geopath` objekt, který obsahuje souřadnice tvaru.
- Mnohoúhelník je vykreslen na mapě přidáním tak, `MapControl.MapElements` kolekce.


```
List<Position> GenerateCircleCoordinates(Position position, double radius)
{
    double latitude = position.Latitude.ToRadians();
    double longitude = position.Longitude.ToRadians();
    double distance = radius / EarthRadiusInMeteres;
    var positions = new List<Position>();

    for (int angle = 0; angle <=360; angle++)
    {
        double angleInRadians = ((double)angle).ToRadians();
        double latitudeInRadians = Math.Asin(Math.Sin(latitude) * Math.Cos(distance) + Math.Cos(latitude) * Math.Sin(distance) * Math.Cos(angleInRadians));
        double longitudeInRadians = longitude + Math.Atan2(Math.Sin(angleInRadians) * Math.Sin(distance) * Math.Cos(latitude), Math.Cos(distance) - Math.Sin(latitude) * Math.Sin(latitudeInRadians));

        var pos = new Position(latitudeInRadians.ToDegrees(), longitudeInRadians.ToDegrees());
        positions.Add(pos);
    }

    return positions;
}
```

## <a name="summary"></a>Souhrn

Tento článek vysvětlil, jak přidat kruhové překrytí k mapě, abyste měli na očích kruhové oblasti na mapě.


## <a name="related-links"></a>Související odkazy

- [Cyklické Ovlerlay mapy (ukázka)](https://developer.xamarin.com/samples/xamarin-forms/customrenderers/map/circle/)
- [Přizpůsobení špendlíku mapy](~/xamarin-forms/app-fundamentals/custom-renderer/map/customized-pin.md)
- [Xamarin.Forms.Maps](xref:Xamarin.Forms.Maps)
