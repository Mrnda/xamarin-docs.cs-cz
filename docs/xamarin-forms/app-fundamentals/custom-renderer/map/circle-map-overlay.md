---
title: "Zvýraznění cyklické oblast na mapu"
description: "Tento článek vysvětluje postup přidání cyklické překrytí na mapu, abyste měli na očích cyklické oblasti mapy."
ms.topic: article
ms.prod: xamarin
ms.assetid: 6FF8BD15-074E-4E6A-9522-F9E2BE32EF12
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 11/29/2017
ms.openlocfilehash: 0eef31c5b9a93154b1038ffa63ee560bd738fe6b
ms.sourcegitcommit: 61f5ecc5a2b5dcfbefdef91664d7460c0ee2f357
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 02/28/2018
---
# <a name="highlighting-a-circular-area-on-a-map"></a>Zvýraznění cyklické oblast na mapu

_Tento článek vysvětluje postup přidání cyklické překrytí na mapu, abyste měli na očích cyklické oblasti mapy._

## <a name="overview"></a>Přehled

Překrytí je vrstveného grafika na mapě. Překryvy podporovat kreslení grafické obsah, který škáluje s mapy, jako je možnosti. Na následujících snímcích obrazovky zobrazit výsledkem přidání cyklické překrytí na mapu:

![](circle-map-overlay-images/screenshots.png)

Když [ `Map` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Maps.Map/) vykreslení ovládacího prvku platformě Xamarin.Forms aplikací v iOS `MapRenderer` vytvoření instance třídy, které pak vytvoří nativní `MKMapView` ovládacího prvku. Na platformě Android `MapRenderer` třída vytvoří nativní `MapView` ovládacího prvku. Na univerzální platformu Windows (UWP), `MapRenderer` třída vytvoří nativní `MapControl`. Proces vykreslování můžete provedeny výhod implementovat přizpůsobení specifické pro platformu mapy tak, že vytvoříte vlastní zobrazovací jednotky pro `Map` na každou platformu. Proces pro to vypadá takto:

1. [Vytvoření](#Creating_the_Custom_Map) Xamarin.Forms vlastní mapování.
1. [Využívat](#Consuming_the_Custom_Map) vlastní mapy z Xamarin.Forms.
1. [Přizpůsobení](#Customizing_the_Map) mapy tak, že vytvoříte vlastní zobrazovací jednotky pro mapu na každou platformu.

> [!NOTE]
> [`Xamarin.Forms.Maps`](https://developer.xamarin.com/api/namespace/Xamarin.Forms.Maps/") musí být inicializovaná a před použitím. Další informace najdete v tématu [`Maps Control`](~/xamarin-forms/user-interface/map.md)

Informace o přizpůsobení mapu pomocí vlastní zobrazovací jednotky najdete v tématu [přizpůsobení Map kódu Pin](~/xamarin-forms/app-fundamentals/custom-renderer/map/customized-pin.md).

<a name="Creating_the_Custom_Map" />

### <a name="creating-the-custom-map"></a>Vytváření vlastních mapy

Vytvoření `CustomCircle` třídu, která má `Position` a `Radius` vlastnosti:

```csharp
public class CustomCircle
{
  public Position Position { get; set; }
  public double Radius { get; set; }
}
```

Pak vytvořte podtřídu [ `Map` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Maps.Map/) třída, která přidává vlastnosti typu `CustomCircle`:

```csharp
public class CustomMap : Map
{
  public CustomCircle Circle { get; set; }
}
```

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

Přidá tento inicializace [ `Pin` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Maps.Pin/) a `CustomCircle` instance vlastní mapu a umisťuje zobrazení mapy na s [ `MoveToRegion` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Maps.Map.MoveToRegion(Xamarin.Forms.Maps.MapSpan)/) metoda, která mění pozice a přiblížení úroveň mapy tak, že vytvoříte [ `MapSpan` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Maps.MapSpan/) z [ `Position` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Maps.Position/) a [ `Distance` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Maps.Distance/).

<a name="Customizing_the_Map" />

### <a name="customizing-the-map"></a>Přizpůsobení mapy

Vlastní zobrazovací jednotky je nyní přidat na každý projekt aplikace pro přidání do mapy cyklické překrytí.

#### <a name="creating-the-custom-renderer-on-ios"></a>Vytváření vlastní zobrazovací jednotky v systému iOS

Vytvoření podtřídou třídy `MapRenderer` třídy a přepsat její `OnElementChanged` metody přidat cyklické překrytí:

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

Za předpokladu, že vlastní zobrazovací jednotky je připojen k nového elementu Xamarin.Forms, tato metoda provádí následující konfiguraci:

- `MKMapView.OverlayRenderer` Je nastavena na odpovídající delegáta.
- Kruhu je vytvořen nastavením statického `MKCircle` objekt, který určuje střed kruhu a poloměr kruhu v měřidla.
- Kruhu je přidán do mapy voláním `MKMapView.AddOverlay` metoda.

Potom implementovat `GetOverlayRenderer` metodu za účelem přizpůsobení vykreslování překrytí:

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

#### <a name="creating-the-custom-renderer-on-android"></a>Vytváření vlastní zobrazovací jednotky v systému Android

Vytvoření podtřídou třídy `MapRenderer` třídy a přepsat její `OnElementChanged` a `OnMapReady` metody pro přidání cyklické překrytí:

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

`OnElementChanged` Volání metod `MapView.GetMapAsync` metodu, která získá základní `GoogleMap` který je vázaný na zobrazení, za předpokladu, že vlastní zobrazovací jednotky je připojen k nového elementu Xamarin.Forms. Jednou `GoogleMap` instance je k dispozici, `OnMapReady` metoda bude vyvolán, kde se má vytvořit na kruh po vytvoření instance `CircleOptions` objekt, který určuje střed kruhu a poloměr kruhu v měřidla. Kruhu se pak přidá do mapy voláním `NativeMap.AddCircle` metoda.

#### <a name="creating-the-custom-renderer-on-the-universal-windows-platform"></a>Vytváření vlastní zobrazovací jednotky na univerzální platformu Windows

Vytvoření podtřídou třídy `MapRenderer` třídy a přepsat její `OnElementChanged` metody přidat cyklické překrytí:

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
        ...
    }
}
```

Tato metoda provede následující operace, za předpokladu, že vlastní zobrazovací jednotky je připojen k nového elementu Xamarin.Forms:

- Pozice kruh a radius jsou načteny z `CustomMap.Circle` vlastnost a předána `GenerateCircleCoordinates` metodu, která generuje zeměpisnou šířku a délku souřadnice hraniční kruh.
- Souřadnice hraniční kruh se převedou na `List` z `BasicGeoposition` souřadnice.
- Po vytvoření instance se vytvoří na kruh `MapPolygon` objektu. `MapPolygon` Třída se používá k nastavení zobrazit tvar více bodů na mapě jeho `Path` vlastnosti `Geopath` objekt, který obsahuje souřadnice tvaru.
- Vykreslení mnohoúhelníku na mapě přidáním do `MapControl.MapElements` kolekce.

## <a name="summary"></a>Souhrn

Tento článek vysvětluje postup přidání cyklické překrytí na mapu, abyste měli na očích cyklické oblasti mapy.


## <a name="related-links"></a>Související odkazy

- [Cyklické Ovlerlay mapy (ukázka)](https://developer.xamarin.com/samples/xamarin-forms/customrenderers/map/circle/)
- [Přizpůsobení Map kódu Pin](~/xamarin-forms/app-fundamentals/custom-renderer/map/customized-pin.md)
- [Xamarin.Forms.Maps](https://developer.xamarin.com/api/namespace/Xamarin.Forms.Maps/)
