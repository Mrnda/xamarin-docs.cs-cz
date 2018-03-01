---
title: "Poznámky a překryvy"
description: "Tento článek představuje podrobný návod znázorňující způsob práce s poznámky a překrytí funkce Kit mapy. Ukazuje, jak přidat mapu do aplikace, která se zobrazí poznámky a překrytí v umístění konference Xamarin momentální 2013."
ms.topic: article
ms.prod: xamarin
ms.assetid: 1BC4F7FC-AE3C-46D7-A4D3-18E142F55B8E
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/21/2017
ms.openlocfilehash: 7eea7231ec4300a368e4612cbed2ba4ebc044a26
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 02/27/2018
---
# <a name="annotations-and-overlays--walkthrough"></a>Poznámky a překryvy – návod

Aplikace, kterou vytvoříme sestavení v tomto návodu je zobrazena níže:

 [![](ios-maps-walkthrough-images/00-map-overlay.png "Příklad MapKit aplikace")](ios-maps-walkthrough-images/00-map-overlay.png)
 
Dokončený kód v najdete **MapsWalkthroughComplete** složky v rámci [mapy Demo-ukázka](https://developer.xamarin.com/samples/monotouch/MapDemo/).

Začněme vytvořením nového **iOS prázdný projekt**a pod názvem relevantní. Jsme budete Začněte přidáním kód do Kontroleru zobrazení pro zobrazení MapView a pak vytvořit nové třídy pro naše MapDelegate a vlastní poznámky. Použijte následující postup k dosažení tohoto cíle:

## <a name="viewcontroller"></a>ViewController


1. Přidejte následující obory názvů `ViewController`:

    ```csharp
    using MapKit;
    using CoreLocation;
    using UIKit
    using CoreGraphics
    ```

1. Přidat `MKMapView` instance proměnné k třídě, spolu s `MapDelegate` instance. Vytvoříme `MapDelegate` krátce:

    ```csharp
    public partial class ViewController : UIViewController
    {
        MKMapView map;
        MapDelegate mapDelegate;
        ...
    ```

1. V kontroleru `LoadView` metoda, přidejte `MKMapView` a nastavte ji na `View` vlastnost řadiče:

    ```csharp
    public override void LoadView ()
    {
        map = new MKMapView (UIScreen.MainScreen.Bounds);
        View = map;
    }
    ```

    Potom přidáme kód pro inicializaci mapy ve ' ViewDidLoad'' metoda.

1. V `ViewDidLoad` přidejte kód nastavit typ mapy, zobrazí se umístění uživatele a povolit přibližování a posouvání:

    ```csharp
    // change map type, show user location and allow zooming and panning
    map.MapType = MKMapType.Standard;
    map.ShowsUserLocation = true;
    map.ZoomEnabled = true;
    map.ScrollEnabled = true;
    
    ```

1. Dál přidejte kód center mapy a nastavit jeho oblasti:

    ```csharp
    double lat = 30.2652233534254;
    double lon = -97.73815460962083;
    CLLocationCoordinate2D mapCenter = new CLLocationCoordinate2D (lat, lon);
    MKCoordinateRegion mapRegion = MKCoordinateRegion.FromDistance (mapCenter, 100, 100);
    map.CenterCoordinate = mapCenter;
    map.Region = mapRegion;
    
    ```

1. Vytvořit novou instanci třídy `MapDelegate` a přiřadit ji ke `Delegate` z `MKMapView`. Znovu, jsme budete implcodeent `MapDelegate` krátce:

    ```csharp
    mapDelegate = new MapDelegate ();
    map.Delegate = mapDelegate;     
    ```

1. Od verze iOS 8 by měl být požaduje autorizaci z uživatelům používat jejich umístění, takže přidejme to pro naše ukázka. Nejprve definovat `CLLocationManager` proměnné na úrovni třídy:

    ```csharp
    CLLocationManager locationManager = new CLLocationManager();
    ```

1. V `ViewDidLoad` metoda, chceme zkontrolujte, zda zařízení spuštěný aplikace používá iOS 8, a pokud je jsme bude požadovat autorizace, když aplikace se používá:

    ```csharp
    if (UIDevice.CurrentDevice.CheckSystemVersion(8,0)){
                    locationManager.RequestWhenInUseAuthorization ();
                }
    ```

1. Nakonec je potřeba upravit **Info.plist** soubor poradit uživatelé důvod žádosti jejich umístění. V **zdroj** nabídky **Info.plist**, přidat následující klíč:
    
    `NSLocationWhenInUseUsageDescription` 
    
    a řetězec: 

    `Maps Demo`.


## <a name="conferenceannotationcs--a-class-for-custom-annotations"></a>ConferenceAnnotation.cs – třídu, pro vlastní poznámky


1. Vytvoříme používat vlastní třídy pro poznámku názvem `ConferenceAnnotation`. Do projektu přidejte následující třídy:

    ```csharp
    using System;
    using CoreLocation;
    using MapKit;
    
    namespace MapDemo
    {
        public class ConferenceAnnotation : MKAnnotation
        {
            string title;
            CLLocationCoordinate2D coord;
    
            public ConferenceAnnotation (string title,
            CLLocationCoordinate2D coord)
            {
                this.title = title;
                this.coord = coord;
            }
    
            public override string Title {
                get {
                    return title;
                }
            }
    
            public override CLLocationCoordinate2D Coordinate {
                get {
                    return coord;
                }
            }
        }
    }   
    ```

## <a name="viewcontroller---adding-the-annotation-and-overlay"></a>ViewController - přidávat poznámky a překrytí

1. Pomocí `ConferenceAnnotation` na místě můžeme ho přidat do mapy. Zpět v `ViewDidLoad` metodu `ViewController`, přidat anotace souřadnice mapy center:

    ```csharp
    map.AddAnnotations (new ConferenceAnnotation ("Evolve Conference", mapCenter)); 
    ```

1. Chceme také mít překrytí hotelů. Přidejte následující kód k vytvoření `MKPolygon` pomocí souřadnice pro hotelů zadaná a ji přidat do mapy voláním `AddOverlay`:

    ```csharp
    // add an overlay of the hotel
    MKPolygon hotelOverlay = MKPolygon.FromCoordinates(
        new CLLocationCoordinate2D[]{
        new CLLocationCoordinate2D(30.2649977168594, -97.73863627705),
        new CLLocationCoordinate2D(30.2648461170005, -97.7381627734755),
        new CLLocationCoordinate2D(30.2648355402574, -97.7381750192576),
        new CLLocationCoordinate2D(30.2647791309417, -97.7379872505988),
        new CLLocationCoordinate2D(30.2654525150319, -97.7377341711021),
        new CLLocationCoordinate2D(30.2654807195004, -97.7377994819399),
        new CLLocationCoordinate2D(30.2655089239607, -97.7377994819399),
        new CLLocationCoordinate2D(30.2656428950368, -97.738346460207),
        new CLLocationCoordinate2D(30.2650364981811, -97.7385709662122),
        new CLLocationCoordinate2D(30.2650470749025, -97.7386199493406)
    });
    
    map.AddOverlay (hotelOverlay);  
    ```
Tím dokončíte kód v `ViewDidLoad`. Teď musíme implementovat naše `MapDelegate` třídy ke zpracování, vytváření anotace a překrytí zobrazení v uvedeném pořadí.


## <a name="mapdelegate"></a>MapDelegate

1. Vytvoření třídy s názvem `MapDelegate` který dědí z `MKMapViewDelegate` a patří `annotationId` proměnné pro použití jako identifikátor opakované použití pro poznámku:

    ```csharp
    class MapDelegate : MKMapViewDelegate
    {
        static string annotationId = "ConferenceAnnotation";
        ...
    }
    ```
    Jsme mít pouze jeden poznámky zde tak opakované použití kódu není nezbytně nutné, ale je dobrým zvykem zahrnout.

1. Implementace `GetViewForAnnotation` metodu pro zobrazení pro návrat `ConferenceAnnotation` pomocí **conference.png** bitové kopie, které jsou zahrnuté v tomto průvodci:

    ```csharp
    public override MKAnnotationView GetViewForAnnotation (MKMapView mapView, NSObject annotation)
    {
        MKAnnotationView annotationView = null;
    
        if (annotation is MKUserLocation)
            return null; 
    
        if (annotation is ConferenceAnnotation) {
    
            // show conference annotation
            annotationView = mapView.DequeueReusableAnnotation (annotationId);
    
            if (annotationView == null)
                annotationView = new MKAnnotationView (annotation, annotationId);
        
            annotationView.Image = UIImage.FromFile ("images/conference.png");
            annotationView.CanShowCallout = true;
        } 
    
        return annotationView;
    }
    ```

1. Když uživatel klepnutím na anotace, chceme zobrazte obrázek zobrazující Austinu města. Přidejte následující proměnné na `MapDelegate` pro bitové kopie a zobrazení tak, aby ho:

    ```csharp
    UIImageView venueView;
    UIImage venueImage;
    ```

1. V dalším kroku zobrazíte bitovou kopii, když je stisknuté anotace, implementovat `DidSelectAnnotation` metoda následujícím způsobem:

    ```csharp
    public override void DidSelectAnnotationView (MKMapView mapView, MKAnnotationView view)
    {
        // show an image view when the conference annotation view is selected
        if (view.Annotation is ConferenceAnnotation) {
    
            venueView = new UIImageView ();
            venueView.ContentMode = UIViewContentMode.ScaleAspectFit;
            venueImage = UIImage.FromFile ("image/venue.png");
            venueView.Image = venueImage;
            view.AddSubview (venueView);
    
            UIView.Animate (0.4, () => {
            venueView.Frame = new CGRect (-75, -75, 200, 200); });
        }
    }
    ```

1. Skrýt bitovou kopii, když uživatel zruší výběr anotace klepnutím kdekoliv jinde na mapě, implementovat `DidSelectAnnotationView` metoda následujícím způsobem:

    ```csharp
    public override void DidDeselectAnnotationView (MKMapView mapView, MKAnnotationView view)
    {
        // remove the image view when the conference annotation is deselected
        if (view.Annotation is ConferenceAnnotation) {
    
            venueView.RemoveFromSuperview ();
            venueView.Dispose ();
            venueView = null;
        }
    }
    ```
    Teď máme kód pro poznámku na místě. Vše, co je ponechán je přidejte kód, který `MapDelegate` vytvořit zobrazení pro hotelů překrytí.

1. Přidejte následující implementace `GetViewForOverlay` k `MapDelegate`:

    ```csharp
    public override MKOverlayView GetViewForOverlay (MKMapView mapView, NSObject overlay)
    {
        // return a view for the polygon
        MKPolygon polygon = overlay as MKPolygon;
        MKPolygonView polygonView = new MKPolygonView (polygon);
        polygonView.FillColor = UIColor.Blue;
        polygonView.StrokeColor = UIColor.Red;
        return polygonView;
    }
    ```

Spusťte aplikaci. Nyní je k dispozici interaktivní mapu s vlastní poznámkou a překrytí! Klepněte na ni a bitové kopie Austinu se zobrazí, jak je uvedeno níže:

 [![](ios-maps-walkthrough-images/01-map-image.png "Klepněte na ni a bitové kopie Austinu se zobrazí.")](ios-maps-walkthrough-images/01-map-image.png)

## <a name="summary"></a>Souhrn

V tomto článku jsme se podívali na postup přidání poznámky do mapy a také jak přidat překrytí pro zadaný mnohoúhelníku. Také jsme ukázal, jak přidat podporu touch poznámky pro animaci image přes mapy.


## <a name="related-links"></a>Související odkazy

- [Ukázkový mapy (ukázka)](https://developer.xamarin.com/samples/monotouch/MapDemo/)
- [iOS mapy](~/ios/user-interface/controls/ios-maps/index.md)
