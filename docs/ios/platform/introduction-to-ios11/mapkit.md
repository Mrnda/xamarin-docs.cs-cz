---
title: Nové funkce v MapKit v systému iOS 11
description: 'Tento dokument popisuje nové funkce MapKit v iOS 11: seskupování značek, tlačítko kompasu, měřítka zobrazení a tlačítko Sledování uživatele.'
ms.prod: xamarin
ms.assetid: 304AE5A3-518F-422F-BE24-92D62CE30F34
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 08/30/2016
ms.openlocfilehash: f73078a2dcbaeefeb5608ce7ec1e2c12b261acad
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 06/05/2018
ms.locfileid: "34787403"
---
# <a name="new-features-in-mapkit-on-ios-11"></a>Nové funkce v MapKit v systému iOS 11

iOS 11 MapKit přidává následující nové funkce:

- [Poznámky Clustering](#clustering)
- [Kompas tlačítko](#compass)
- [Škálování zobrazení](#scale)
- [Tlačítko Sledování uživatele](#user-tracking)

![Mapování zobrazující Clusterové značek a kompas tlačítko](mapkit-images/cyclemap-heading.png)

<a name="clustering" />

## <a name="automatically-grouping-markers-while-zooming"></a>Automaticky seskupení značek při změně měřítka

Ukázka [MapKit ukázka "Tandm"](https://developer.xamarin.com/samples/monotouch/ios11/MapKitSample/) ukazuje, jak implementovat nový iOS 11 poznámky funkci clusteringu.

### <a name="1-create-an-mkpointannotation-subclass"></a>1. Vytvoření `MKPointAnnotation` podtřídy

Třída poznámky bodu reprezentuje každé značky na mapě. Mohou být přidány jednotlivě pomocí `MapView.AddAnnotation()` nebo z pole pomocí `MapView.AddAnnotations()`.

Bod poznámky třídy nemají vizuální znázornění, pouze musí představovat data související s značky (co je nejdůležitější `Coordinate` vlastnost, která je jeho zeměpisnou šířku a zeměpisnou délku na mapě) a jakékoli vlastní vlastnosti:

```csharp
public class Bike : MKPointAnnotation
{
  public BikeType Type { get; set; } = BikeType.Tricycle;
  public Bike(){}
  public Bike(NSNumber lat, NSNumber lgn, NSNumber type)
  {
    Coordinate = new CLLocationCoordinate2D(lat.NFloatValue, lgn.NFloatValue);
    switch(type.NUIntValue) {
      case 0:
        Type = BikeType.Unicycle;
        break;
      case 1:
        Type = BikeType.Tricycle;
        break;
    }
  }
}
```

### <a name="2-create-an-mkmarkerannotationview-subclass-for-single-markers"></a>2. Vytvoření `MKMarkerAnnotationView` podtřídami pro jeden značek

Zobrazení poznámky značky je vizuální reprezentace každý poznámky a je navržen tak, jako například pomocí vlastnosti:

- **MarkerTintColor** – barvu značky.
- **GlyphText** – zobrazení textu v značky.
- **GlyphImage** – nastavuje obrázek, který se zobrazí v značky.
- **DisplayPriority** – určuje pořadí z-order (skládání chování) Pokud je zobrazeno příliš mnoho značek mapy. Použijte jednu z `Required`, `DefaultHigh`, nebo `DefaultLow`.

Pro podporu automatického clusteringu, musíte taky nastavit:

- **ClusteringIdentifier** – tato volba určuje, které značek získat clusterovaný společně. Můžete použít stejný identifikátor pro všechny značky, nebo použít různé identifikátory k řízení způsobu, jakým jsou seskupeny dohromady.

```csharp
[Register("BikeView")]
public class BikeView : MKMarkerAnnotationView
{
  public static UIColor UnicycleColor = UIColor.FromRGB(254, 122, 36);
  public static UIColor TricycleColor = UIColor.FromRGB(153, 180, 44);
  public override IMKAnnotation Annotation
  {
    get {
      return base.Annotation;
    }
    set {
      base.Annotation = value;

      var bike = value as Bike;
      if (bike != null){
        ClusteringIdentifier = "bike";
        switch(bike.Type){
          case BikeType.Unicycle:
            MarkerTintColor = UnicycleColor;
            GlyphImage = UIImage.FromBundle("Unicycle");
            DisplayPriority = MKFeatureDisplayPriority.DefaultLow;
            break;
          case BikeType.Tricycle:
            MarkerTintColor = TricycleColor;
            GlyphImage = UIImage.FromBundle("Tricycle");
            DisplayPriority = MKFeatureDisplayPriority.DefaultHigh;
            break;
        }
      }
    }
  }
```

### <a name="3-create-an-mkannotationview-to-represent-clusters-of-markers"></a>3. Vytvoření `MKAnnotationView` představují clustery značek

Při zobrazení poznámky, které představuje cluster značek _může_ být jednoduché bitové kopie, uživatelé očekávají, že aplikaci poskytují vizuální upozornění o tom, kolik značek, byly seskupeny dohromady.

[Ukázkový kód](https://developer.xamarin.com/samples/monotouch/ios11/MapKitSample/) používá CoreGraphics k vykreslení počet značek v clusteru, jakož i kruh grafu reprezentace podíl každý typ značky.

Je třeba nastavit také:

- **DisplayPriority** – určuje pořadí z-order (skládání chování) Pokud je zobrazeno příliš mnoho značek mapy. Použijte jednu z `Required`, `DefaultHigh`, nebo `DefaultLow`.
- **CollisionMode** – `Circle` nebo `Rectangle`.

```csharp
[Register("ClusterView")]
public class ClusterView : MKAnnotationView
{
  public static UIColor ClusterColor = UIColor.FromRGB(202, 150, 38);
  public override IMKAnnotation Annotation
  {
    get {
      return base.Annotation;
    }
    set {
      base.Annotation = value;
      var cluster = MKAnnotationWrapperExtensions.UnwrapClusterAnnotation(value);
      if (cluster != null)
      {
        var renderer = new UIGraphicsImageRenderer(new CGSize(40, 40));
        var count = cluster.MemberAnnotations.Length;
        var unicycleCount = CountBikeType(cluster.MemberAnnotations, BikeType.Unicycle);

        Image = renderer.CreateImage((context) => {
          // Fill full circle with tricycle color
          BikeView.TricycleColor.SetFill();
          UIBezierPath.FromOval(new CGRect(0, 0, 40, 40)).Fill();
          // Fill pie with unicycle color
          BikeView.UnicycleColor.SetFill();
          var piePath = new UIBezierPath();
          piePath.AddArc(new CGPoint(20,20), 20, 0, (nfloat)(Math.PI * 2.0 * unicycleCount / count), true);
          piePath.AddLineTo(new CGPoint(20, 20));
          piePath.ClosePath();
          piePath.Fill();
          // Fill inner circle with white color
          UIColor.White.SetFill();
          UIBezierPath.FromOval(new CGRect(8, 8, 24, 24)).Fill();
          // Finally draw count text vertically and horizontally centered
          var attributes = new UIStringAttributes() {
            ForegroundColor = UIColor.Black,
            Font = UIFont.BoldSystemFontOfSize(20)
          };
          var text = new NSString($"{count}");
          var size = text.GetSizeUsingAttributes(attributes);
          var rect = new CGRect(20 - size.Width / 2, 20 - size.Height / 2, size.Width, size.Height);
          text.DrawString(rect, attributes);
        });
      }
    }
  }
  public ClusterView(){}
  public ClusterView(MKAnnotation annotation, string reuseIdentifier) : base(annotation, reuseIdentifier)
  {
    DisplayPriority = MKFeatureDisplayPriority.DefaultHigh;
    CollisionMode = MKAnnotationViewCollisionMode.Circle;
    // Offset center point to animate better with marker annotations
    CenterOffset = new CoreGraphics.CGPoint(0, -10);
  }
  private nuint CountBikeType(IMKAnnotation[] members, BikeType type) {
    nuint count = 0;
    foreach(Bike member in members){
      if (member.Type == type) ++count;
    }
    return count;
  }
}
```

### <a name="4-register-the-view-classes"></a>4. Registrace třídy zobrazení

Při zobrazení mapový ovládací prvek se vytváří a přidá do zobrazení, registrace typy zobrazení poznámky tak, aby povolovala automatické clustering chování mapy je možnosti a odhlášení:

```csharp
MapView.Register(typeof(BikeView), MKMapViewDefault.AnnotationViewReuseIdentifier);
MapView.Register(typeof(ClusterView), MKMapViewDefault.ClusterAnnotationViewReuseIdentifier);
```

### <a name="5-render-the-map"></a>5. Vykreslení mapy!

Po vykreslení mapy poznámky značek bude v clusteru nebo vykreslen v závislosti na aktuální úroveň přiblížení. Jak změní úroveň přiblížení, použije animaci značek a deaktivovat clustery.

![Simulátor zobrazující Clusterové značek na mapě](mapkit-images/cyclemap-sml.png)

Odkazovat [mapuje části](~/ios/user-interface/controls/ios-maps/index.md) Další informace o zobrazení dat s MapKit.

<a name="compass" />

## <a name="compass-button"></a>Kompas tlačítko

iOS 11 přidává možnost pop kompas mimo mapy a vykreslit ho jinde v zobrazení. Najdete v článku [Tandm ukázkovou aplikaci](https://developer.xamarin.com/samples/monotouch/ios11/MapKitSample/) příklad.

Vytvoření tlačítka, které vypadá jako kompas (při změně orientaci mapy, včetně provozu animace), a vykreslí na další ovládací prvek.

![Kompasu tlačítka na navigačním panelu](mapkit-images/compass-sml.png)

Následující kód vytvoří kompasu tlačítko a vykreslí na navigačním panelu:

```csharp
var compass = MKCompassButton.FromMapView(MapView);
compass.CompassVisibility = MKFeatureVisibility.Visible;
NavigationItem.RightBarButtonItem = new UIBarButtonItem(compass);
MapView.ShowsCompass = false; // so we don't have two compasses!
```

`ShowsCompass` Vlastnost lze použít k řízení viditelnost kompas výchozí uvnitř zobrazení mapy.

<a name="scale" />

## <a name="scale-view"></a>Škálování zobrazení

Přidat měřítka jinde v zobrazení pomocí `MKScaleView.FromMapView()` metody pro získání instance škálování zobrazení přidat jinde v hierarchii zobrazení.

![Škálování zobrazení jako překryvný obrázek na mapu](mapkit-images/scale-sml.png)

```csharp
var scale = MKScaleView.FromMapView(MapView);
scale.LegendAlignment = MKScaleViewAlignment.Trailing;
scale.TranslatesAutoresizingMaskIntoConstraints = false;
View.AddSubview(scale); // constraints omitted for simplicity
MapView.ShowsScale = false; // so we don't have two scale displays!
```

`ShowsScale` Vlastnost lze použít k řízení viditelnost kompas výchozí uvnitř zobrazení mapy.

<a name="user-tracking" />

## <a name="user-tracking-button"></a>Tlačítko Sledování uživatele

Tlačítko sledování uživatel centra mapy na aktuální umístění uživatele. Použití `MKUserTrackingButton.FromMapView()` metoda získat instanci tlačítko, použít změny formátování a přidat jinde v hierarchii zobrazení.

![Tlačítko umístění uživatele jako překryvný obrázek na mapu](mapkit-images/user-location-sml.png)

```csharp
var button = MKUserTrackingButton.FromMapView(MapView);
button.Layer.BackgroundColor = UIColor.FromRGBA(255,255,255,80).CGColor;
button.Layer.BorderColor = UIColor.White.CGColor;
button.Layer.BorderWidth = 1;
button.Layer.CornerRadius = 5;
button.TranslatesAutoresizingMaskIntoConstraints = false;
View.AddSubview(button); // constraints omitted for simplicity
```


## <a name="related-links"></a>Související odkazy

- [Ukázka MapKit 'Tandm.](https://developer.xamarin.com/samples/monotouch/ios11/MapKitSample/)
- [MKCompassButton](https://developer.apple.com/documentation/mapkit/mkcompassbutton)
- [Co je nového v MapKit (WWDC) (video)](https://developer.apple.com/videos/play/wwdc2017/237/)
