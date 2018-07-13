---
title: Zvýraznění oblasti na mapě
description: Tento článek vysvětluje, jak přidat překrytí mnohoúhelníku mapy, abyste měli na očích oblasti na mapě. Mnohoúhelníky se zavřeného tvaru a jejich vnitřek vyplněno.
ms.prod: xamarin
ms.assetid: E79EB2CF-8DD6-44A8-B47D-5F0A94FB0A63
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 11/29/2017
ms.openlocfilehash: 0a11e9c25922531727ad2fee3bbed9c8d4e2b80c
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/12/2018
ms.locfileid: "38998131"
---
# <a name="highlighting-a-region-on-a-map"></a>Zvýraznění oblasti na mapě

_Tento článek vysvětlil, jak přidat překrytí mnohoúhelníku mapy, abyste měli na očích oblasti na mapě. Mnohoúhelníky se zavřeného tvaru a jejich vnitřek vyplněno._

## <a name="overview"></a>Přehled

Překrytí je vrstvený grafiky na mapě. Překryvy podporují výkresu grafického obsahu, která se škáluje s mapou, jak je zvětšeno. Na následujících snímcích obrazovky zobrazit výsledek přidání překrytí mnohoúhelníku mapy:

![](polygon-map-overlay-images/screenshots.png)

Když [ `Map` ](xref:Xamarin.Forms.Maps.Map) aplikací Xamarin.Forms v Iosu se vykreslí ovládací prvek `MapRenderer` je vytvořena instance třídy, které pak vytvoří instanci nativní `MKMapView` ovládacího prvku. Na platformu Android `MapRenderer` třídy vytvoří instanci nativní `MapView` ovládacího prvku. Na Universal Windows Platform (UWP), `MapRenderer` třídy vytvoří instanci nativní `MapControl`. Samotný proces vykreslování můžete třeba využít implementovat přizpůsobení specifické pro platformu mapování tak, že vytvoříte vlastní zobrazovací jednotky pro `Map` na jednotlivých platformách. Tento proces je následujícím způsobem:

1. [Vytvoření](#Creating_the_Custom_Map) vlastní mapa Xamarin.Forms.
1. [Využívání](#Consuming_the_Custom_Map) vlastní mapy z Xamarin.Forms.
1. [Přizpůsobení](#Customizing_the_Map) mapování tak, že vytvoříte vlastní zobrazovací jednotky pro mapování na jednotlivých platformách.

> [!NOTE]
> [`Xamarin.Forms.Maps`](xref:Xamarin.Forms.Maps) musí být inicializován a před použitím nakonfigurovat. Další informace najdete na webu [`Maps Control`](~/xamarin-forms/user-interface/map.md).

Informace o přizpůsobení mapy pomocí vlastní zobrazovací jednotky najdete v tématu [přizpůsobení špendlíku mapy](~/xamarin-forms/app-fundamentals/custom-renderer/map/customized-pin.md).

<a name="Creating_the_Custom_Map" />

### <a name="creating-the-custom-map"></a>Vytváří se vlastní mapa

Vytvořit podtřídu [ `Map` ](xref:Xamarin.Forms.Maps.Map) třída, která přidá `ShapeCoordinates` vlastnost:

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

`ShapeCoordinates` Vlastnost uloží kolekci souřadnic, které definují oblasti, kterou chcete být zvýrazněn.

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
    customMap.ShapeCoordinates.Add (new Position (37.797513, -122.402058));
    customMap.ShapeCoordinates.Add (new Position (37.798433, -122.402256));
    customMap.ShapeCoordinates.Add (new Position (37.798582, -122.401071));
    customMap.ShapeCoordinates.Add (new Position (37.797658, -122.400888));

    customMap.MoveToRegion (MapSpan.FromCenterAndRadius (new Position (37.79752, -122.40183), Distance.FromMiles (0.1)));
  }
}
```

Tato inicializace určuje řadu souřadnice zeměpisné šířky a délky na definují oblasti mapy, která má být zvýrazněn. Potom umístí zobrazení na mapě s [ `MoveToRegion` ](xref:Xamarin.Forms.Maps.Map.MoveToRegion*) metoda, která změní pozice a úroveň přiblížení mapy tak, že vytvoříte [ `MapSpan` ](xref:Xamarin.Forms.Maps.MapSpan) z [ `Position` ](xref:Xamarin.Forms.Maps.Position) a [ `Distance` ](xref:Xamarin.Forms.Maps.Distance).

<a name="Customizing_the_Map" />

### <a name="customizing-the-map"></a>Přizpůsobení mapy

Vlastní zobrazovací jednotky musí nyní přidán do každého projektu aplikace přidat do překrytí mnohoúhelníku mapy.

#### <a name="creating-the-custom-renderer-on-ios"></a>Vytvoření vlastní zobrazovací jednotky v systému iOS

Vytvořit podtřídu `MapRenderer` třídy a přepsat její `OnElementChanged` metoda pro přidání do překrytí mnohoúhelníku:

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

Za předpokladu, že vlastní zobrazovací jednotky je připojen k nový prvek Xamarin.Forms, tato metoda provádí následující konfiguraci:

- `MKMapView.OverlayRenderer` Je nastavena na odpovídající delegáta.
- Kolekce zeměpisné šířky a délky se načítají z `CustomMap.ShapeCoordinates` vlastnosti a uložené jako pole `CLLocationCoordinate2D` instancí.
- Mnohoúhelník je vytvořen zavoláním statické `MKPolygon.FromCoordinates` metodu, která určuje zeměpisnou šířku a délku každého bodu.
- Mnohoúhelník je přidán do mapování voláním `MKMapView.AddOverlay` metody. Tato metoda automaticky uzavře mnohoúhelník kreslením čára spojující body první a poslední.

Pak implementovat `GetOverlayRenderer` metodu za účelem přizpůsobení vykreslování překrytí:

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

#### <a name="creating-the-custom-renderer-on-android"></a>Vytvoření vlastního Rendereru v Androidu

Vytvořit podtřídu `MapRenderer` třídy a přepsat její `OnElementChanged` a `OnMapReady` metody pro přidání do překrytí mnohoúhelníku:

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

`OnElementChanged` Metoda načte kolekci zeměpisné šířky a délky souřadnice z `CustomMap.ShapeCoordinates` vlastnosti a ukládá je v členské proměnné. Poté zavolá `MapView.GetMapAsync` metodu, která získá základní `GoogleMap` , který se váže k zobrazení, za předpokladu, že vlastní zobrazovací jednotky je připojen k nový prvek Xamarin.Forms. Jednou `GoogleMap` instance je k dispozici, `OnMapReady` metoda bude volána, kde se vytvoří mnohoúhelník po vytvoření instance `PolygonOptions` určující zeměpisnou šířku a délku každého bodu. Mnohoúhelník se pak přidá do mapy voláním `NativeMap.AddPolygon` metody. Tato metoda automaticky uzavře mnohoúhelník kreslením čára spojující body první a poslední.

#### <a name="creating-the-custom-renderer-on-the-universal-windows-platform"></a>Vytvoření vlastního Rendereru na Universal Windows Platform

Vytvořit podtřídu `MapRenderer` třídy a přepsat její `OnElementChanged` metoda pro přidání do překrytí mnohoúhelníku:

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

Za předpokladu, že vlastní zobrazovací jednotky je připojen k nový prvek Xamarin.Forms, tato metoda provádí následující operace:

- Kolekce zeměpisné šířky a délky se načítají z `CustomMap.ShapeCoordinates` vlastnost a převedené do `List` z `BasicGeoposition` souřadnice.
- Po vytvoření instance je vytvořena mnohoúhelníku `MapPolygon` objektu. `MapPolygon` Třída se používá k zobrazení více bodů tvar na mapě nastavením jeho `Path` vlastnost `Geopath` objekt, který obsahuje souřadnice tvaru.
- Mnohoúhelník je vykreslen na mapě přidáním tak, `MapControl.MapElements` kolekce. Všimněte si, že mnohoúhelníku se automaticky zavřou kreslením čára spojující body první a poslední.

## <a name="summary"></a>Souhrn

Tento článek vysvětlil, jak přidat překrytí mnohoúhelníku mapy, abyste měli na očích oblast mapy. Mnohoúhelníky se zavřeného tvaru a jejich vnitřek vyplněno.


## <a name="related-links"></a>Související odkazy

- [Mnohoúhelník mapy překrytí (ukázka)](https://developer.xamarin.com/samples/xamarin-forms/customrenderers/map/polygon/)
- [Přizpůsobení špendlíku mapy](~/xamarin-forms/app-fundamentals/custom-renderer/map/customized-pin.md)
- [Xamarin.Forms.Maps](xref:Xamarin.Forms.Maps)
