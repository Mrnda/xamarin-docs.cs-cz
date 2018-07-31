---
title: Nové funkce v Mapkitu v Iosu 11
description: 'Tento dokument popisuje nové funkce Iosu 11 Mapkitu: seskupování značky, kompasu tlačítko, zobrazení škálování a sledování tlačítka pro uživatele.'
ms.prod: xamarin
ms.assetid: 304AE5A3-518F-422F-BE24-92D62CE30F34
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 08/30/2017
ms.openlocfilehash: c060a7bbc8d5968aeaca5f84743cdf22513dfbec
ms.sourcegitcommit: aa9b9b203ab4cd6a6b4fd51e27d865e2abf582c1
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/30/2018
ms.locfileid: "39350583"
---
# <a name="new-features-in-mapkit-on-ios-11"></a>Nové funkce v Mapkitu v Iosu 11

iOS 11 přidává následující nové funkce Mapkitu:

- [Poznámka Clustering](#clustering)
- [Compass tlačítko](#compass)
- [Měřítko zobrazení](#scale)
- [Tlačítka pro sledování uživatele](#user-tracking)

![Mapa zobrazující Clusterované značky a compass tlačítko](mapkit-images/cyclemap-heading.png)

<a name="clustering" />

## <a name="automatically-grouping-markers-while-zooming"></a>Automaticky grouping značky při zvětšování

Ukázka [Mapkitu ukázka "Tandm"](https://developer.xamarin.com/samples/monotouch/ios11/MapKitSample/) ukazuje, jak implementovat nová poznámka iOS 11, funkce clustering.

### <a name="1-create-an-mkpointannotation-subclass"></a>1. Vytvoření `MKPointAnnotation` podtřídy

Třída poznámek bodu představuje každý značky na mapě. Je možné je přidat pomocí jednotlivě `MapView.AddAnnotation()` nebo z pole pomocí `MapView.AddAnnotations()`.

Bod anotace třídy nemají vizuální reprezentaci, je vyžadováno pouze ke znázornění dat přidružené k popisovači (co je nejdůležitější, `Coordinate` vlastnost, která je jeho zeměpisné šířky a délky na mapě) a všechny vlastní vlastnosti:

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

### <a name="2-create-an-mkmarkerannotationview-subclass-for-single-markers"></a>2. Vytvoření `MKMarkerAnnotationView` podtřídu pro jedné značky

Zobrazit značky poznámek je vizuální znázornění jednotlivé poznámky a je navržen tak, jako například pomocí vlastnosti:

- **MarkerTintColor** – barva značky.
- **GlyphText** – zobrazení textu v značky.
- **GlyphImage** – nastaví obrázek, který se zobrazí v značky.
- **DisplayPriority** – určuje pořadí z-order (překrývání chování) při mapování zaplněný se značkami. Použijte jednu z `Required`, `DefaultHigh`, nebo `DefaultLow`.

Pro podporu automatického clustering, musíte taky nastavit:

- **ClusteringIdentifier** – tato volba určuje které značek získat clusterovaného společně. Můžete použít stejný identifikátor pro všechny značky nebo odlišné identifikátory můžete řídit způsob, jakým jsou seskupeny dohromady.

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

### <a name="3-create-an-mkannotationview-to-represent-clusters-of-markers"></a>3. Vytvoření `MKAnnotationView` představující clustery značek

Při zobrazení poznámky, které představuje cluster značek _může_ být jednoduché bitové kopie, uživatelé očekávají aplikaci poskytují vizuální informace o tom, kolik značky, byly seskupeny dohromady.

[Ukázkový kód](https://developer.xamarin.com/samples/monotouch/ios11/MapKitSample/) CoreGraphics používá k vykreslení počet značek v clusteru, jakož i kruh grafu reprezentace poměr každého typu značky.

Měli byste také nastavit:

- **DisplayPriority** – určuje pořadí z-order (překrývání chování) při mapování zaplněný se značkami. Použijte jednu z `Required`, `DefaultHigh`, nebo `DefaultLow`.
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

Při zobrazení mapový ovládací prvek se vytváří a přidá do zobrazení, registrace anotace zobrazit typy tak, aby povolovala automatické chování clusteringu mapy zvětšován dovnitř a ven:

```csharp
MapView.Register(typeof(BikeView), MKMapViewDefault.AnnotationViewReuseIdentifier);
MapView.Register(typeof(ClusterView), MKMapViewDefault.ClusterAnnotationViewReuseIdentifier);
```

### <a name="5-render-the-map"></a>5. Vykreslení mapy!

Při zobrazení na mapě se bude značky poznámek v clusteru nebo vykreslen v závislosti na úrovni zvětšení. Jak se změní úroveň zvětšení, značky animace do clusterů.

![Simulátor zobrazující Clusterované značky na mapě](mapkit-images/cyclemap-sml.png)

Odkazovat [mapuje části](~/ios/user-interface/controls/ios-maps/index.md) Další informace o zobrazení dat ovládacím prvkem Mapkitu.

<a name="compass" />

## <a name="compass-button"></a>Compass tlačítko

iOS 11 přidává možnost vyvolat přes pop compass mimo mapy a vykreslit ho jinde v zobrazení. Zobrazit [Tandm ukázkovou aplikaci](https://developer.xamarin.com/samples/monotouch/ios11/MapKitSample/) příklad.

Vytvoření tlačítka, která vypadá jako compass (včetně živých animace při změně orientace mapy) a vykreslí na jiný ovládací prvek.

![Kompasu tlačítko v navigačním panelu](mapkit-images/compass-sml.png)

Následující kód vytvoří kompasu tlačítko a vykreslí na navigačním panelu:

```csharp
var compass = MKCompassButton.FromMapView(MapView);
compass.CompassVisibility = MKFeatureVisibility.Visible;
NavigationItem.RightBarButtonItem = new UIBarButtonItem(compass);
MapView.ShowsCompass = false; // so we don't have two compasses!
```

`ShowsCompass` Vlastnost umožňuje řídit viditelnost compass výchozí uvnitř zobrazení mapy.

<a name="scale" />

## <a name="scale-view"></a>Měřítko zobrazení

Přidat škálovací jinde v zobrazení pomocí `MKScaleView.FromMapView()` metodu k získání instance měřítko zobrazení přidat jinde v hierarchii zobrazení.

![Měřítko zobrazení jako překryvný obrázek na mapě](mapkit-images/scale-sml.png)

```csharp
var scale = MKScaleView.FromMapView(MapView);
scale.LegendAlignment = MKScaleViewAlignment.Trailing;
scale.TranslatesAutoresizingMaskIntoConstraints = false;
View.AddSubview(scale); // constraints omitted for simplicity
MapView.ShowsScale = false; // so we don't have two scale displays!
```

`ShowsScale` Vlastnost umožňuje řídit viditelnost compass výchozí uvnitř zobrazení mapy.

<a name="user-tracking" />

## <a name="user-tracking-button"></a>Tlačítka pro sledování uživatele

Tlačítka pro sledování uživatele centra pro mapování na aktuální umístění uživatele. Použití `MKUserTrackingButton.FromMapView()` metodu k získání instance tlačítka, použít změny formátování a přidání jinde v hierarchii zobrazení.

![Tlačítka pro umístění uživatele jako překryvný obrázek na mapě](mapkit-images/user-location-sml.png)

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

- [Ukázka Mapkitu "Tandm.](https://developer.xamarin.com/samples/monotouch/ios11/MapKitSample/)
- [MKCompassButton](https://developer.apple.com/documentation/mapkit/mkcompassbutton)
- [Co je nového v Mapkitu (WWDC) (video)](https://developer.apple.com/videos/play/wwdc2017/237/)
