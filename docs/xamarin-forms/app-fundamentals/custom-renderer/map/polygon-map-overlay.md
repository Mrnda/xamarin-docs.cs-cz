---
title: Zvýraznění Region na mapě
description: Tento článek vysvětluje postup přidání překrytí mnohoúhelníku na mapu, abyste měli na očích region na mapě. Mnohoúhelníky jsou uzavřený obrazec a jejich vnitřek vyplnili.
ms.prod: xamarin
ms.assetid: E79EB2CF-8DD6-44A8-B47D-5F0A94FB0A63
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 11/29/2017
ms.openlocfilehash: e0ffa1948bb7dd0996dd21793237df550a32aa70
ms.sourcegitcommit: d80d93957040a14b4638a91b0eac797cfaade840
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 06/07/2018
ms.locfileid: "34848327"
---
# <a name="highlighting-a-region-on-a-map"></a>Zvýraznění Region na mapě

_Tento článek vysvětluje postup přidání překrytí mnohoúhelníku na mapu, abyste měli na očích region na mapě. Mnohoúhelníky jsou uzavřený obrazec a jejich vnitřek vyplnili._

## <a name="overview"></a>Přehled

Překrytí je vrstveného grafika na mapě. Překryvy podporovat kreslení grafické obsah, který škáluje s mapy, jako je možnosti. Na následujících snímcích obrazovky zobrazit výsledkem přidání překrytí mnohoúhelníku na mapu:

![](polygon-map-overlay-images/screenshots.png)

Když [ `Map` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Maps.Map/) vykreslení ovládacího prvku platformě Xamarin.Forms aplikací v iOS `MapRenderer` vytvoření instance třídy, které pak vytvoří nativní `MKMapView` ovládacího prvku. Na platformě Android `MapRenderer` třída vytvoří nativní `MapView` ovládacího prvku. Na univerzální platformu Windows (UWP), `MapRenderer` třída vytvoří nativní `MapControl`. Proces vykreslování můžete provedeny výhod implementovat přizpůsobení specifické pro platformu mapy tak, že vytvoříte vlastní zobrazovací jednotky pro `Map` na každou platformu. Proces pro to vypadá takto:

1. [Vytvoření](#Creating_the_Custom_Map) Xamarin.Forms vlastní mapování.
1. [Využívat](#Consuming_the_Custom_Map) vlastní mapy z Xamarin.Forms.
1. [Přizpůsobení](#Customizing_the_Map) mapy tak, že vytvoříte vlastní zobrazovací jednotky pro mapu na každou platformu.

> [!NOTE]
> [`Xamarin.Forms.Maps`](https://developer.xamarin.com/api/namespace/Xamarin.Forms.Maps/) musí být inicializovaná a před použitím. Další informace najdete na webu [`Maps Control`](~/xamarin-forms/user-interface/map.md).

Informace o přizpůsobení mapu pomocí vlastní zobrazovací jednotky najdete v tématu [přizpůsobení Map kódu Pin](~/xamarin-forms/app-fundamentals/custom-renderer/map/customized-pin.md).

<a name="Creating_the_Custom_Map" />

### <a name="creating-the-custom-map"></a>Vytváření vlastních mapy

Vytvoření podtřídou třídy [ `Map` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Maps.Map/) třída, která přidává `ShapeCoordinates` vlastnost:

```csharp
public class CustomMap : Map
{
  public List<Position> ShapeCoordinates { get; set; }

  public CustomMap ()
  {
    ShapeCoordinates = new List<Position> ();
  }
}
```

`ShapeCoordinates` Vlastnost uloží kolekce souřadnic, které definují oblasti, kterou chcete mít zvýrazněná.

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
    customMap.ShapeCoordinates.Add (new Position (37.797513, -122.402058));
    customMap.ShapeCoordinates.Add (new Position (37.798433, -122.402256));
    customMap.ShapeCoordinates.Add (new Position (37.798582, -122.401071));
    customMap.ShapeCoordinates.Add (new Position (37.797658, -122.400888));

    customMap.MoveToRegion (MapSpan.FromCenterAndRadius (new Position (37.79752, -122.40183), Distance.FromMiles (0.1)));
  }
}
```

Tato inicializace určuje řadu zeměpisnou šířku a délku souřadnice oblasti mapy chcete mít zvýrazněná definovat. Ji pak umisťuje zobrazení mapy na s [ `MoveToRegion` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Maps.Map.MoveToRegion(Xamarin.Forms.Maps.MapSpan)/) metoda, která mění pozice a úroveň přiblížení mapy vytvořením [ `MapSpan` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Maps.MapSpan/) z [ `Position` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Maps.Position/) a [ `Distance` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Maps.Distance/).

<a name="Customizing_the_Map" />

### <a name="customizing-the-map"></a>Přizpůsobení mapy

Vlastní zobrazovací jednotky je nyní přidat na každý projekt aplikace pro přidání do překrytí mnohoúhelníku do mapy.

#### <a name="creating-the-custom-renderer-on-ios"></a>Vytváření vlastní zobrazovací jednotky v systému iOS

Vytvoření podtřídou třídy `MapRenderer` třídy a přepsat její `OnElementChanged` metoda pro přidání do překrytí mnohoúhelníku:

```csharp
[assembly: ExportRenderer(typeof(CustomMap), typeof(CustomMapRenderer))]
namespace MapOverlay.iOS
{
    public class CustomMapRenderer : MapRenderer
    {
        MKPolygonRenderer polygonRenderer;

        protected override void OnElementChanged(ElementChangedEventArgs<View> e)
        {
            base.OnElementChanged(e);

            if (e.OldElement != null) {
                var nativeMap = Control as MKMapView;
                if (nativeMap != null) {
                    nativeMap.RemoveOverlays(nativeMap.Overlays);
                    nativeMap.OverlayRenderer = null;
                    polygonRenderer = null;
                }
            }

            if (e.NewElement != null) {
                var formsMap = (CustomMap)e.NewElement;
                var nativeMap = Control as MKMapView;

                nativeMap.OverlayRenderer = GetOverlayRenderer;

                CLLocationCoordinate2D[] coords = new CLLocationCoordinate2D[formsMap.ShapeCoordinates.Count];

                int index = 0;
                foreach (var position in formsMap.ShapeCoordinates)
                {
                    coords[index] = new CLLocationCoordinate2D(position.Latitude, position.Longitude);
                    index++;
                }

                var blockOverlay = MKPolygon.FromCoordinates(coords);
                nativeMap.AddOverlay(blockOverlay);
            }
        }
        ...
    }
}

```

Za předpokladu, že vlastní zobrazovací jednotky je připojen k nového elementu Xamarin.Forms, tato metoda provádí následující konfiguraci:

- `MKMapView.OverlayRenderer` Je nastavena na odpovídající delegáta.
- Kolekce zeměpisné šířky a délky jsou načteny z `CustomMap.ShapeCoordinates` vlastnost a uloží jako pole `CLLocationCoordinate2D` instance.
- Vytvoření mnohoúhelníku voláním statické `MKPolygon.FromCoordinates` metoda, která určuje zeměpisnou šířku a délku každého bodu.
- Mnohoúhelníku je přidán do mapy voláním `MKMapView.AddOverlay` metoda. Tato metoda automaticky zavře mnohoúhelníku ve kreslení čára spojující body první a poslední.

Potom implementovat `GetOverlayRenderer` metodu za účelem přizpůsobení vykreslování překrytí:

```csharp
public class CustomMapRenderer : MapRenderer
{
  MKPolygonRenderer polygonRenderer;
  ...

  MKOverlayRenderer GetOverlayRenderer(MKMapView mapView, IMKOverlay overlayWrapper)
  {
      if (polygonRenderer == null && !Equals(overlayWrapper, null)) {
          var overlay = Runtime.GetNSObject(overlayWrapper.Handle) as IMKOverlay;
          polygonRenderer = new MKPolygonRenderer(overlay as MKPolygon) {
              FillColor = UIColor.Red,
              StrokeColor = UIColor.Blue,
              Alpha = 0.4f,
              LineWidth = 9
          };
      }
      return polygonRenderer;
  }
}
```

#### <a name="creating-the-custom-renderer-on-android"></a>Vytváření vlastní zobrazovací jednotky v systému Android

Vytvoření podtřídou třídy `MapRenderer` třídy a přepsat její `OnElementChanged` a `OnMapReady` metody pro přidání do překrytí mnohoúhelníku:

```csharp
[assembly: ExportRenderer(typeof(CustomMap), typeof(CustomMapRenderer))]
namespace MapOverlay.Droid
{
    public class CustomMapRenderer : MapRenderer
    {
        List<Position> shapeCoordinates;

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
                shapeCoordinates = formsMap.ShapeCoordinates;
                Control.GetMapAsync(this);
            }
        }

        protected override void OnMapReady(Android.Gms.Maps.GoogleMap map)
        {
            base.OnMapReady(map);

            var polygonOptions = new PolygonOptions();
            polygonOptions.InvokeFillColor(0x66FF0000);
            polygonOptions.InvokeStrokeColor(0x660000FF);
            polygonOptions.InvokeStrokeWidth(30.0f);

            foreach (var position in shapeCoordinates)
            {
                polygonOptions.Add(new LatLng(position.Latitude, position.Longitude));
            }
            NativeMap.AddPolygon(polygonOptions);
        }
    }
}
```

`OnElementChanged` Metoda načte kolekci zeměpisných souřadnic ze `CustomMap.ShapeCoordinates` vlastnost a ukládá je v členské proměnné. Potom zavolá `MapView.GetMapAsync` metodu, která získá základní `GoogleMap` který je vázaný na zobrazení, za předpokladu, že vlastní zobrazovací jednotky je připojen k nového elementu Xamarin.Forms. Jednou `GoogleMap` instance je k dispozici, `OnMapReady` metoda bude vyvolán, kde se má vytvořit mnohoúhelníku po vytvoření instance `PolygonOptions` objekt, který určuje zeměpisnou šířku a délku každého bodu. Mnohoúhelníku se pak přidá do mapy voláním `NativeMap.AddPolygon` metoda. Tato metoda automaticky zavře mnohoúhelníku ve kreslení čára spojující body první a poslední.

#### <a name="creating-the-custom-renderer-on-the-universal-windows-platform"></a>Vytváření vlastní zobrazovací jednotky na univerzální platformu Windows

Vytvoření podtřídou třídy `MapRenderer` třídy a přepsat její `OnElementChanged` metoda pro přidání do překrytí mnohoúhelníku:

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
                foreach (var position in formsMap.ShapeCoordinates)
                {
                    coordinates.Add(new BasicGeoposition() { Latitude = position.Latitude, Longitude = position.Longitude });
                }

                var polygon = new MapPolygon();
                polygon.FillColor = Windows.UI.Color.FromArgb(128, 255, 0, 0);
                polygon.StrokeColor = Windows.UI.Color.FromArgb(128, 0, 0, 255);
                polygon.StrokeThickness = 5;
                polygon.Path = new Geopath(coordinates);
                nativeMap.MapElements.Add(polygon);
            }
        }
    }
}
```

Tato metoda provede následující operace, za předpokladu, že vlastní zobrazovací jednotky je připojen k nového elementu Xamarin.Forms:

- Kolekce zeměpisné šířky a délky jsou načteny z `CustomMap.ShapeCoordinates` vlastnost a převedený do `List` z `BasicGeoposition` souřadnice.
- Po vytvoření instance se vytvoří mnohoúhelníku `MapPolygon` objektu. `MapPolygon` Třída se používá k nastavení zobrazit tvar více bodů na mapě jeho `Path` vlastnosti `Geopath` objekt, který obsahuje souřadnice tvaru.
- Vykreslení mnohoúhelníku na mapě přidáním do `MapControl.MapElements` kolekce. Všimněte si, že mnohoúhelníku se automaticky uzavřou podle kreslení čára spojující body první a poslední.

## <a name="summary"></a>Souhrn

Tento článek vysvětluje postup přidání překrytí mnohoúhelníku na mapu, aby byly zvýrazněné oblasti mapy. Mnohoúhelníky jsou uzavřený obrazec a jejich vnitřek vyplnili.


## <a name="related-links"></a>Související odkazy

- [Mnohoúhelníku mapy překrytí (ukázka)](https://developer.xamarin.com/samples/xamarin-forms/customrenderers/map/polygon/)
- [Přizpůsobení špendlíku mapy](~/xamarin-forms/app-fundamentals/custom-renderer/map/customized-pin.md)
- [Xamarin.Forms.Maps](https://developer.xamarin.com/api/namespace/Xamarin.Forms.Maps/)
