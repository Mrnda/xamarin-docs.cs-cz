---
title: Zvýraznění trasy na mapě
description: Tento článek vysvětluje, jak přidat překrytí lomené čáry na mapu. Překrytí lomené čáry je řada spojených čar segmenty, které se obvykle používají k zobrazení trasy na mapě nebo tvoří žádný obrazec, který je potřeba.
ms.prod: xamarin
ms.assetid: FBFDC715-1654-4188-82A0-FC522548BCFF
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 11/29/2017
ms.openlocfilehash: 786f050495d4682b719178f2723c482929544678
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/12/2018
ms.locfileid: "38998717"
---
# <a name="highlighting-a-route-on-a-map"></a>Zvýraznění trasy na mapě

_Tento článek vysvětluje, jak přidat překrytí lomené čáry na mapu. Překrytí lomené čáry je řada spojených čar segmenty, které se obvykle používají k zobrazení trasy na mapě nebo tvoří žádný obrazec, který je potřeba._

## <a name="overview"></a>Přehled

Překrytí je vrstvený grafiky na mapě. Překryvy podporují výkresu grafického obsahu, která se škáluje s mapou, jak je zvětšeno. Na následujících snímcích obrazovky zobrazit výsledek přidání překrytí lomené čáry mapy:

![](polyline-map-overlay-images/screenshots.png)

Když [ `Map` ](xref:Xamarin.Forms.Maps.Map) aplikací Xamarin.Forms v Iosu se vykreslí ovládací prvek `MapRenderer` je vytvořena instance třídy, které pak vytvoří instanci nativní `MKMapView` ovládacího prvku. Na platformu Android `MapRenderer` třídy vytvoří instanci nativní `MapView` ovládacího prvku. Na Universal Windows Platform (UWP), `MapRenderer` třídy vytvoří instanci nativní `MapControl`. Samotný proces vykreslování můžete třeba využít implementovat přizpůsobení specifické pro platformu mapování tak, že vytvoříte vlastní zobrazovací jednotky pro `Map` na jednotlivých platformách. Tento proces je následujícím způsobem:

1. [Vytvoření](#Creating_the_Custom_Map) vlastní mapa Xamarin.Forms.
1. [Využívání](#Consuming_the_Custom_Map) vlastní mapy z Xamarin.Forms.
1. [Přizpůsobení](#Customizing_the_Map) mapování tak, že vytvoříte vlastní zobrazovací jednotky pro mapování na jednotlivých platformách.

> [!NOTE]
> [`Xamarin.Forms.Maps`](xref:Xamarin.Forms.Maps) musí být inicializován a před použitím nakonfigurovat. Další informace najdete na webu [`Maps Control`](~/xamarin-forms/user-interface/map.md).

Informace o přizpůsobení mapy pomocí vlastní zobrazovací jednotky najdete v tématu [přizpůsobení špendlíku mapy](~/xamarin-forms/app-fundamentals/custom-renderer/map/customized-pin.md).

<a name="Creating_the_Custom_Map" />

### <a name="creating-the-custom-map"></a>Vytváří se vlastní mapa

Vytvořit podtřídu [ `Map` ](xref:Xamarin.Forms.Maps.Map) třída, která přidá `RouteCoordinates` vlastnost:

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

`RouteCoordinates` Vlastnost uloží kolekci souřadnic, které definují trasy, která má být zvýrazněn.

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
    customMap.RouteCoordinates.Add (new Position (37.785559, -122.396728));
    customMap.RouteCoordinates.Add (new Position (37.780624, -122.390541));
    customMap.RouteCoordinates.Add (new Position (37.777113, -122.394983));
    customMap.RouteCoordinates.Add (new Position (37.776831, -122.394627));

    customMap.MoveToRegion (MapSpan.FromCenterAndRadius (new Position (37.79752, -122.40183), Distance.FromMiles (1.0)));
  }
}
```

Tato inicializace určuje řadu souřadnice zeměpisné šířky a délky pro definování trasy na mapě zvýrazněny. Potom umístí zobrazení na mapě s [ `MoveToRegion` ](xref:Xamarin.Forms.Maps.Map.MoveToRegion*) metoda, která změní pozice a úroveň přiblížení mapy tak, že vytvoříte [ `MapSpan` ](xref:Xamarin.Forms.Maps.MapSpan) z [ `Position` ](xref:Xamarin.Forms.Maps.Position) a [ `Distance` ](xref:Xamarin.Forms.Maps.Distance).

<a name="Customizing_the_Map" />

### <a name="customizing-the-map"></a>Přizpůsobení mapy

Vlastní zobrazovací jednotky musí nyní přidán do každého projektu aplikace přidat do překrytí lomené čáry na mapě.

#### <a name="creating-the-custom-renderer-on-ios"></a>Vytvoření vlastní zobrazovací jednotky v systému iOS

Vytvořit podtřídu `MapRenderer` třídy a přepsat její `OnElementChanged` metoda pro přidání do překrytí lomené čáry:

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

Za předpokladu, že vlastní zobrazovací jednotky je připojen k nový prvek Xamarin.Forms, tato metoda provádí následující konfiguraci:

- `MKMapView.OverlayRenderer` Je nastavena na odpovídající delegáta.
- Kolekce zeměpisné šířky a délky se načítají z `CustomMap.RouteCoordinates` vlastnosti a uložené jako pole `CLLocationCoordinate2D` instancí.
- Lomenou čáru je vytvořen zavoláním statické `MKPolyline.FromCoordinates` metodu, která určuje zeměpisnou šířku a délku každého bodu.
- Lomenou čáru je přidán do mapování voláním `MKMapView.AddOverlay` metody.

Pak implementovat `GetOverlayRenderer` metodu za účelem přizpůsobení vykreslování překrytí:

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

#### <a name="creating-the-custom-renderer-on-android"></a>Vytvoření vlastního Rendereru v Androidu

Vytvořit podtřídu `MapRenderer` třídy a přepsat její `OnElementChanged` a `OnMapReady` metody pro přidání do překrytí lomené čáry:

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

`OnElementChanged` Metoda načte kolekci zeměpisné šířky a délky souřadnice z `CustomMap.RouteCoordinates` vlastnosti a ukládá je v členské proměnné. Poté zavolá `MapView.GetMapAsync` metodu, která získá základní `GoogleMap` , který se váže k zobrazení, za předpokladu, že vlastní zobrazovací jednotky je připojen k nový prvek Xamarin.Forms. Jednou `GoogleMap` instance je k dispozici, `OnMapReady` metoda bude volána, pokud je lomenou čáru vytvořené po vytvoření instance `PolylineOptions` určující zeměpisnou šířku a délku každého bodu. Lomenou čáru se pak přidá do mapy voláním `NativeMap.AddPolyline` metody.

#### <a name="creating-the-custom-renderer-on-the-universal-windows-platform"></a>Vytvoření vlastního Rendereru na Universal Windows Platform

Vytvořit podtřídu `MapRenderer` třídy a přepsat její `OnElementChanged` metoda pro přidání do překrytí lomené čáry:

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

Za předpokladu, že vlastní zobrazovací jednotky je připojen k nový prvek Xamarin.Forms, tato metoda provádí následující operace:

- Kolekce zeměpisné šířky a délky se načítají z `CustomMap.RouteCoordinates` vlastnost a převedené do `List` z `BasicGeoposition` souřadnice.
- Po vytvoření instance je vytvořena lomenou čáru `MapPolyline` objektu. `MapPolygon` Třída se používá k zobrazení řádku na mapě nastavením jeho `Path` vlastnost `Geopath` objekt, který obsahuje tento řádek souřadnice.
- Lomenou čáru se vykreslí na mapu tak, že přidáte tak, `MapControl.MapElements` kolekce.

## <a name="summary"></a>Souhrn

Tento článek vysvětlil, jak přidat překrytí lomené čáry na mapě zobrazit trasy na mapě nebo formuláře žádný obrazec, který je potřeba.


## <a name="related-links"></a>Související odkazy

- [Lomené čáry mapy Ovlerlay (ukázka)](https://developer.xamarin.com/samples/xamarin-forms/customrenderers/map/polyline/)
- [Přizpůsobení špendlíku mapy](~/xamarin-forms/app-fundamentals/custom-renderer/map/customized-pin.md)
- [Xamarin.Forms.Maps](xref:Xamarin.Forms.Maps)
