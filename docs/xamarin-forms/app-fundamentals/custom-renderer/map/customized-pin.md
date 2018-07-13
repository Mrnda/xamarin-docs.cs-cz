---
title: Přizpůsobení špendlíku mapy
description: Tento článek ukazuje, jak vytvořit vlastního rendereru pro mapový ovládací prvek, který zobrazuje nativní mapa s vlastní kód pin a vlastní zobrazení dat PIN kód na jednotlivých platformách.
ms.prod: xamarin
ms.assetid: C5481D86-80E9-4E3D-9FB6-57B0F93711A6
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 11/29/2017
ms.openlocfilehash: 4fee67f08e86c40709aa226c40c0f7721dc26800
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/12/2018
ms.locfileid: "38998297"
---
# <a name="customizing-a-map-pin"></a>Přizpůsobení špendlíku mapy

_Tento článek ukazuje, jak vytvořit vlastního rendereru pro mapový ovládací prvek, který zobrazuje nativní mapa s vlastní kód pin a vlastní zobrazení dat PIN kód na jednotlivých platformách._

Každé zobrazení Xamarin.Forms má související zobrazovací jednotky pro každou platformu, která vytvoří instanci nativní ovládacího prvku. Když [ `Map` ](xref:Xamarin.Forms.Maps.Map) je vykreslen metodou aplikace Xamarin.Forms v iOS, `MapRenderer` je vytvořena instance třídy, které pak vytvoří instanci nativní `MKMapView` ovládacího prvku. Na platformu Android `MapRenderer` třídy vytvoří instanci nativní `MapView` ovládacího prvku. Na Universal Windows Platform (UWP), `MapRenderer` třídy vytvoří instanci nativní `MapControl`. Další informace o nástroj pro vykreslování a nativní ovládací prvek třídy, které ovládací prvky Xamarin.Forms namapovat na najdete v tématu [Renderer základní třídy a nativní ovládací prvky](~/xamarin-forms/app-fundamentals/custom-renderer/renderers.md).

Následující diagram znázorňuje vztah mezi [ `Map` ](xref:Xamarin.Forms.Maps.Map) a odpovídající nativní ovládací prvky, které je implementují:

![](customized-pin-images/map-classes.png "Vztah mezi ovládací prvek mapa a implementující nativní ovládací prvky")

Samotný proces vykreslování je možné implementovat přizpůsobení konkrétní platformy tak, že vytvoříte vlastní zobrazovací jednotky pro [ `Map` ](xref:Xamarin.Forms.Maps.Map) na jednotlivých platformách. Tento proces je následujícím způsobem:

1. [Vytvoření](#Creating_the_Custom_Map) vlastní mapa Xamarin.Forms.
1. [Využívání](#Consuming_the_Custom_Map) vlastní mapy z Xamarin.Forms.
1. [Vytvoření](#Creating_the_Custom_Renderer_on_each_Platform) vlastní zobrazovací jednotky pro mapování na jednotlivých platformách.

Každá položka nyní probereme zase k implementaci `CustomMap` renderer, který se zobrazí nativní mapa s vlastní kód pin a vlastní zobrazení dat PIN kód na jednotlivých platformách.

> [!NOTE]
> [`Xamarin.Forms.Maps`](xref:Xamarin.Forms.Maps) musí být inicializován a před použitím nakonfigurovat. Další informace najdete na webu [`Maps Control`](~/xamarin-forms/user-interface/map.md).

<a name="Creating_the_Custom_Map" />

## <a name="creating-the-custom-map"></a>Vytváří se vlastní mapa

Vytváření podtříd můžete vytvořit vlastní mapový ovládací prvek [ `Map` ](xref:Xamarin.Forms.Maps.Map) třídy, jak je znázorněno v následujícím příkladu kódu:

```csharp
public class CustomMap : Map
{
  public List<CustomPin> CustomPins { get; set; }
}
```

`CustomMap` Ovládacího prvku je vytvořen v projektu knihovny .NET Standard a definuje rozhraní API pro vlastní mapy. Vlastní mapa zpřístupňuje `CustomPins` vlastnost, která představuje kolekci `CustomPin` objekty, které se zobrazí nativní mapový ovládací prvek na jednotlivých platformách. `CustomPin` Třídy je znázorněno v následujícím příkladu kódu:

```csharp
public class CustomPin : Pin
{
  public string Url { get; set; }
}
```

Tato třída definuje `CustomPin` jako dědění vlastností [ `Pin` ](xref:Xamarin.Forms.Maps.Pin) třídy a přidání `Url` vlastnost.

<a name="Consuming_the_Custom_Map" />

## <a name="consuming-the-custom-map"></a>Použití vlastních Map

`CustomMap` Ovládací prvek může být odkazováno v XAML v projektu knihovny .NET Standard deklarace oboru názvů pro jeho umístění a použitím předponu oboru názvů na vlastní mapový ovládací prvek. Následující příklad kódu ukazuje jak `CustomMap` ovládacího prvku mohou být spotřebovány stránky XAML:

```xaml
<ContentPage ...
             xmlns:local="clr-namespace:CustomRenderer;assembly=CustomRenderer">
    <ContentPage.Content>
        <local:CustomMap x:Name="myMap" MapType="Street"
          WidthRequest="{x:Static local:App.ScreenWidth}"
          HeightRequest="{x:Static local:App.ScreenHeight}" />
    </ContentPage.Content>
</ContentPage>
```

`local` Předponu oboru názvů může být název cokoli. Ale `clr-namespace` a `assembly` hodnoty musí odpovídat podrobnosti o vlastní mapy. Jakmile je deklarován obor názvů, je předpona, která slouží k odkazování vlastní mapy.

Následující příklad kódu ukazuje jak `CustomMap` ovládacího prvku mohou být spotřebovány stránky jazyka C#:

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

`CustomMap` Instance se použije k zobrazení nativní mapování na jednotlivých platformách. Má [ `MapType` ](xref:Xamarin.Forms.Maps.Map.MapType) vlastnost nastaví styl zobrazení [ `Map` ](xref:Xamarin.Forms.Maps.Map), s možné hodnoty jsou popsány v [ `MapType` ](xref:Xamarin.Forms.Maps.MapType) výčtu. Pro iOS a Android, šířka a výška ohraničení mapy je nastavena prostřednictvím vlastnosti `App` třídu, která jsou inicializovány v projektech pro konkrétní platformu.

Umístění na mapě a kolíky obsahuje, jsou inicializovány, jak je znázorněno v následujícím příkladu kódu:

```csharp
public MapPage ()
{
  ...
  var pin = new CustomPin {
    Type = PinType.Place,
    Position = new Position (37.79752, -122.40183),
    Label = "Xamarin San Francisco Office",
    Address = "394 Pacific Ave, San Francisco CA",
    Id = "Xamarin",
    Url = "http://xamarin.com/about/"
  };

  customMap.CustomPins = new List<CustomPin> { pin };
  customMap.Pins.Add (pin);
  customMap.MoveToRegion (MapSpan.FromCenterAndRadius (
    new Position (37.79752, -122.40183), Distance.FromMiles (1.0)));
}
```

Tato inicializace přidá vlastní kód pin a umístí zobrazení na mapě s [ `MoveToRegion` ](xref:Xamarin.Forms.Maps.Map.MoveToRegion*) metoda, která změní pozice a úroveň přiblížení mapy tak, že vytvoříte [ `MapSpan` ](xref:Xamarin.Forms.Maps.MapSpan) z [ `Position` ](xref:Xamarin.Forms.Maps.Position) a [ `Distance` ](xref:Xamarin.Forms.Maps.Distance).

Vlastní zobrazovací jednotky je nyní přidat do každého projektu aplikace k přizpůsobení ovládacích prvků nativní mapování.

<a name="Creating_the_Custom_Renderer_on_each_Platform" />

## <a name="creating-the-custom-renderer-on-each-platform"></a>Vytvoření vlastního Rendereru na jednotlivých platformách

Proces pro vytvoření vlastního rendereru třídy vypadá takto:

1. Vytvořit podtřídu `MapRenderer` třídu, která vykreslí vlastní mapy.
1. Přepsat `OnElementChanged` metody, která vykreslí vlastní mapy a psaní logiky ji přizpůsobit. Tato metoda je volána, když se vytvoří odpovídající vlastní mapa Xamarin.Forms.
1. Přidat `ExportRenderer` atribut pro třídu vlastního rendereru určíte, že bude používat k vykreslení vlastní mapa Xamarin.Forms. Tento atribut slouží k registraci vlastního rendereru s Xamarin.Forms.

> [!NOTE]
> Zadání je volitelné poskytnout vlastní zobrazovací jednotky v každém projektu platformy. Pokud není zaregistrovaný vlastní zobrazovací jednotky, se používá výchozí zobrazovací jednotky pro základní třídu ovládacího prvku.

Následující diagram znázorňuje odpovědnosti každý projekt v ukázkové aplikaci, spolu s jejich vzájemné vztahy:

![](customized-pin-images/solution-structure.png "CustomMap vlastní zobrazovací jednotky projektu odpovědnosti")

`CustomMap` Ovládací prvek je vykreslen metodou renderer specifické pro platformu, odvozené třídy `MapRenderer` třídy pro každou platformu. Výsledkem je každý `CustomMap` řídit vykreslované s ovládacími prvky pro konkrétní platformu, jak je znázorněno na následujících snímcích obrazovky:

![](customized-pin-images/screenshots.png "CustomMap na jednotlivých platformách")

`MapRenderer` Třídy zpřístupňuje `OnElementChanged` metodu, která je volána, když se vytvoří vlastní mapa Xamarin.Forms pro vykreslení odpovídající nativní ovládací prvek. Tato metoda přebírá `ElementChangedEventArgs` parametr, který obsahuje `OldElement` a `NewElement` vlastnosti. Tyto vlastnosti představují elementu Xamarin.Forms, která zobrazovací jednotky *byl* připojené a Xamarin.Forms element, která zobrazovací jednotky *je* připojené položky v uvedeném pořadí. V ukázkové aplikaci `OldElement` bude mít vlastnost `null` a `NewElement` vlastnost bude obsahovat odkaz na `CustomMap` instance.

Přepsané verzi `OnElementChanged` metoda v každé třídě renderer specifické pro platformu je místem, kde můžete provádět přizpůsobení nativní ovládacího prvku. Zadaný odkaz na nativní ovládací prvek se používají na platformě je přístupná prostřednictvím `Control` vlastnost. Kromě toho lze získat odkaz na ovládací prvek Xamarin.Forms, která se vykresluje přes `Element` vlastnost.

Musíte věnovat pozornost při přihlášení k odběru do obslužné rutiny událostí v `OnElementChanged` způsob, jak je ukázáno v následujícím příkladu kódu:

```csharp
protected override void OnElementChanged (ElementChangedEventArgs<Xamarin.Forms.ListView> e)
{
  base.OnElementChanged (e);

  if (e.OldElement != null) {
    // Unsubscribe from event handlers
  }

  if (e.NewElement != null) {
    // Configure the native control and subscribe to event handlers
  }
}
```

Nativní ovládací prvek by měl být nakonfigurovaný a obslužných rutin událostí k odběru pouze v případě, že vlastní zobrazovací jednotky je připojen k nový prvek Xamarin.Forms. Podobně by měla být všechny obslužné rutiny, které byly k odběru Odhlášený pouze, když změní elementu, který je přiřazena zobrazovací jednotky. Přijmout tento přístup vám pomůže vytvořit vlastní zobrazovací jednotky, který není trpí nevracení paměti.

Každá třída vlastní zobrazovací jednotky je doplněn `ExportRenderer` atribut, který registruje vykreslovaný pomocí Xamarin.Forms. Atribut přebírá dva parametry – název typu vlastního ovládacího prvku Xamarin.Forms vykreslované a název typu vlastní zobrazovací jednotky. `assembly` Předpona, která atribut určuje, že platí atribut pro celé sestavení.

Následující části popisují implementaci každou třídu vlastního rendereru pro konkrétní platformu.

### <a name="creating-the-custom-renderer-on-ios"></a>Vytvoření vlastní zobrazovací jednotky v systému iOS

Na následujících snímcích obrazovky zobrazit na mapě před a po přizpůsobení:

![](customized-pin-images/map-layout-ios.png "Mapový ovládací prvek před a po přizpůsobení")

V Iosu se volá kód pin *anotace*, a může být buď vlastní image nebo systémem definovaná pin různých barev. Můžete volitelně zobrazit poznámky *popisek*, který se zobrazí v reakci na výběru Poznámka uživatelem. Popisek zobrazí `Label` a `Address` vlastnosti `Pin` instance volitelné vlevo a vpravo příslušenství zobrazení. Na snímku obrazovky výše je z levého zobrazení příslušenství image opic, s přímo příslušenství zobrazení se *informace* tlačítko.

Následující příklad kódu ukazuje vlastní zobrazovací jednotky pro platformu iOS:

```csharp
[assembly: ExportRenderer(typeof(CustomMap), typeof(CustomMapRenderer))]
namespace CustomRenderer.iOS
{
    public class CustomMapRenderer : MapRenderer
    {
        UIView customPinView;
        List<CustomPin> customPins;

        protected override void OnElementChanged(ElementChangedEventArgs<View> e)
        {
            base.OnElementChanged(e);

            if (e.OldElement != null) {
                var nativeMap = Control as MKMapView;
                if (nativeMap != null) {
                    nativeMap.RemoveAnnotations(nativeMap.Annotations);
                    nativeMap.GetViewForAnnotation = null;
                    nativeMap.CalloutAccessoryControlTapped -= OnCalloutAccessoryControlTapped;
                    nativeMap.DidSelectAnnotationView -= OnDidSelectAnnotationView;
                    nativeMap.DidDeselectAnnotationView -= OnDidDeselectAnnotationView;
                }
            }

            if (e.NewElement != null) {
                var formsMap = (CustomMap)e.NewElement;
                var nativeMap = Control as MKMapView;
                customPins = formsMap.CustomPins;

                nativeMap.GetViewForAnnotation = GetViewForAnnotation;
                nativeMap.CalloutAccessoryControlTapped += OnCalloutAccessoryControlTapped;
                nativeMap.DidSelectAnnotationView += OnDidSelectAnnotationView;
                nativeMap.DidDeselectAnnotationView += OnDidDeselectAnnotationView;
            }
        }
        ...
    }
}
```

`OnElementChanged` Metoda provádí následující konfiguraci [ `MKMapView` ](https://developer.xamarin.com/api/type/MapKit.MKMapView/) instance, za předpokladu, že vlastní zobrazovací jednotky je připojen k nový prvek Xamarin.Forms:

- [ `GetViewForAnnotation` ](https://developer.xamarin.com/api/property/MapKit.MKMapView.GetViewForAnnotation/) Je nastavena na `GetViewForAnnotation` metody. Tato metoda je volána, když [umístění ze Poznámka se zobrazí na mapě](#Displaying_the_Annotation)a slouží k přizpůsobení předchozího poznámky k zobrazení.
- Obslužné rutiny událostí pro `CalloutAccessoryControlTapped`, `DidSelectAnnotationView`, a `DidDeselectAnnotationView` události jsou registrované. Vyvolat tyto události, kdy uživatel [klepne správné příslušenství v popisku](#Tapping_on_the_Right_Callout_Accessory_View)a když uživatel [vybere](#Selecting_the_Annotation) a [zrušit výběr](#Deselecting_the_Annotation) poznámku, v uvedeném pořadí. Události se zrušila, pouze v případě, že element zobrazovací jednotky je připojený k změny.

<a name="Displaying_the_Annotation" />

#### <a name="displaying-the-annotation"></a>Zobrazení anotace

`GetViewForAnnotation` Metoda je volána při umístění na poznámku se zobrazí na mapě a slouží k přizpůsobení předchozího poznámky k zobrazení. Poznámka má dvě části:

- `MkAnnotation` – obsahuje nadpis, podnadpis a umístění anotace.
- `MkAnnotationView` – obsahuje bitovou kopii k reprezentaci anotace a volitelně popisek, který se zobrazí, když uživatel klepne na poznámku.

`GetViewForAnnotation` Metoda přijímá `IMKAnnotation` , který obsahuje data, poznámky a vrátí `MKAnnotationView` pro zobrazení na mapě a je znázorněno v následujícím příkladu kódu:

```csharp
MKAnnotationView GetViewForAnnotation(MKMapView mapView, IMKAnnotation annotation)
{
    MKAnnotationView annotationView = null;

    if (annotation is MKUserLocation)
        return null;

    var customPin = GetCustomPin(annotation as MKPointAnnotation);
    if (customPin == null) {
        throw new Exception("Custom pin not found");
    }

    annotationView = mapView.DequeueReusableAnnotation(customPin.Id.ToString());
    if (annotationView == null) {
        annotationView = new CustomMKAnnotationView(annotation, customPin.Id.ToString());
        annotationView.Image = UIImage.FromFile("pin.png");
        annotationView.CalloutOffset = new CGPoint(0, 0);
        annotationView.LeftCalloutAccessoryView = new UIImageView(UIImage.FromFile("monkey.png"));
        annotationView.RightCalloutAccessoryView = UIButton.FromType(UIButtonType.DetailDisclosure);
        ((CustomMKAnnotationView)annotationView).Id = customPin.Id.ToString();
        ((CustomMKAnnotationView)annotationView).Url = customPin.Url;
    }
    annotationView.CanShowCallout = true;

    return annotationView;
}
```

Tato metoda zajišťuje, že poznámka se zobrazí jako vlastní image, nikoli jako definovaných systémem PIN kód a že klepnutí Poznámka popisku se zobrazí, který zahrnuje další obsah vlevo a vpravo od názvu poznámky a adresa . To lze provést následujícím způsobem:

1. `GetCustomPin` Metoda je volána k vrácení dat vlastní kód pin pro poznámku.
1. Pro konzervaci paměti, zobrazit poznámky je ve fondu pro další použití volání [ `DequeueReusableAnnotation` ](https://developer.xamarin.com/api/member/MapKit.MKMapView.DequeueReusableAnnotation/(System.String)/).
1. `CustomMKAnnotationView` Třída rozšiřuje `MKAnnotationView` třídy s `Id` a `Url` vlastnostmi, které odpovídají na stejné vlastnosti v `CustomPin` instance. Novou instanci třídy `CustomMKAnnotationView` se vytvoří, za předpokladu, že je Poznámka `null`:
  - `CustomMKAnnotationView.Image` Je nastavena na bitovou kopii, která bude představovat poznámky na mapě.
  - `CustomMKAnnotationView.CalloutOffset` Je nastavena na `CGPoint` , která určuje, že bude popisek nad Poznámka na střed.
  - `CustomMKAnnotationView.LeftCalloutAccessoryView` Je nastavena na obrázek opic, který se zobrazí nalevo od názvu poznámky a adresu.
  - `CustomMKAnnotationView.RightCalloutAccessoryView` Je nastavena na *informace* tlačítko, které se zobrazí napravo od názvu poznámky a adresu.
  - `CustomMKAnnotationView.Id` Je nastavena na `CustomPin.Id` vlastnosti vrácené `GetCustomPin` metody. To umožňuje anotaci musí identifikovat tak, aby byly [popisek se dají dál přizpůsobit](#Selecting_the_Annotation), v případě potřeby.
  - `CustomMKAnnotationView.Url` Je nastavena na `CustomPin.Url` vlastnosti vrácené `GetCustomPin` metody. Adresu URL se přejde poté, kdy uživatel [klepne na tlačítko v pravém popisek příslušenství zobrazení](#Tapping_on_the_Right_Callout_Accessory_View).
1. [ `MKAnnotationView.CanShowCallout` ](https://developer.xamarin.com/api/property/MapKit.MKAnnotationView.CanShowCallout/) Je nastavena na `true` tak, aby popisek se zobrazí po klepnutí na poznámku.
1. Poznámka se vrátí k zobrazení na mapě.

<a name="Selecting_the_Annotation" />

#### <a name="selecting-the-annotation"></a>Vybráním poznámky

Když uživatel klepne na poznámku, `DidSelectAnnotationView` dojde k aktivaci události, které se spouštějí zase `OnDidSelectAnnotationView` metody:

```csharp
void OnDidSelectAnnotationView (object sender, MKAnnotationViewEventArgs e)
{
  var customView = e.View as CustomMKAnnotationView;
  customPinView = new UIView ();

  if (customView.Id == "Xamarin") {
    customPinView.Frame = new CGRect (0, 0, 200, 84);
    var image = new UIImageView (new CGRect (0, 0, 200, 84));
    image.Image = UIImage.FromFile ("xamarin.png");
    customPinView.AddSubview (image);
    customPinView.Center = new CGPoint (0, -(e.View.Frame.Height + 75));
    e.View.AddSubview (customPinView);
  }
}
```

Tato metoda rozšiřuje existující popisek (který obsahuje zobrazení vlevo a vpravo příslušenství) tak, že přidáte `UIView` instance k němu, který obsahuje obrázek loga Xamarin, za předpokladu, že vybrané poznámky jeho `Id` nastavenou na `Xamarin`. To umožňuje scénáře, kde lze zobrazit různé popisky pro různé poznámky. `UIView` Instance se zobrazí na střed nad existující popisek.

<a name="Tapping_on_the_Right_Callout_Accessory_View" />

#### <a name="tapping-on-the-right-callout-accessory-view"></a>Klepnutím na zobrazení správné popisek příslušenství

Když uživatel klepne na *informace* tlačítko v pravém popisek příslušenství zobrazení, `CalloutAccessoryControlTapped` dojde k aktivaci události, které následně provede `OnCalloutAccessoryControlTapped` metody:

```csharp
void OnCalloutAccessoryControlTapped (object sender, MKMapViewAccessoryTappedEventArgs e)
{
  var customView = e.View as CustomMKAnnotationView;
  if (!string.IsNullOrWhiteSpace (customView.Url)) {
    UIApplication.SharedApplication.OpenUrl (new Foundation.NSUrl (customView.Url));
  }
}
```

Tato metoda se otevře webový prohlížeč a přejde na adresu uloženou v `CustomMKAnnotationView.Url` vlastnost. Všimněte si, že adresa byla definována při vytváření `CustomPin` kolekce v projektu knihovny .NET Standard.

<a name="Deselecting_the_Annotation" />

#### <a name="deselecting-the-annotation"></a>Zrušením výběru anotace

Když uživatel klepne na mapě a poznámky se zobrazí `DidDeselectAnnotationView` dojde k aktivaci události, které následně provede `OnDidDeselectAnnotationView` metoda:

```csharp
void OnDidDeselectAnnotationView (object sender, MKAnnotationViewEventArgs e)
{
  if (!e.View.Selected) {
    customPinView.RemoveFromSuperview ();
    customPinView.Dispose ();
    customPinView = null;
  }
}
```

Tato metoda zajišťuje, že při existující popisek není vybrána, rozšířené součástí popisek (obrázek loga Xamarin) se také zastaví se zobrazí a její prostředky budou vydané.

Další informace o přizpůsobení `MKMapView` instance najdete v tématu [iOS mapy](~/ios/user-interface/controls/ios-maps/index.md).

### <a name="creating-the-custom-renderer-on-android"></a>Vytvoření vlastního Rendereru v Androidu

Na následujících snímcích obrazovky zobrazit na mapě před a po přizpůsobení:

![](customized-pin-images/map-layout-android.png "Mapový ovládací prvek před a po přizpůsobení")

V systému Android je volána kód pin *značky*, a může být buď vlastní image nebo systémem definovaná značky různých barev. Můžete zobrazit značky *informační okno*, který se zobrazí odpověď na uživatelský, klepnutím na značku. V okně se informace zobrazí `Label` a `Address` vlastnosti `Pin` instanci a je možné přizpůsobit zahrnout další obsah. Pouze jeden informační okno však lze zobrazit najednou.

Následující příklad kódu ukazuje vlastní zobrazovací jednotky pro platformu Android:

```csharp
[assembly: ExportRenderer(typeof(CustomMap), typeof(CustomMapRenderer))]
namespace CustomRenderer.Droid
{
    public class CustomMapRenderer : MapRenderer, GoogleMap.IInfoWindowAdapter
    {
        List<CustomPin> customPins;

        public CustomMapRenderer(Context context) : base(context)
        {
        }

        protected override void OnElementChanged(Xamarin.Forms.Platform.Android.ElementChangedEventArgs<Map> e)
        {
            base.OnElementChanged(e);

            if (e.OldElement != null)
            {
                NativeMap.InfoWindowClick -= OnInfoWindowClick;
            }

            if (e.NewElement != null)
            {
                var formsMap = (CustomMap)e.NewElement;
                customPins = formsMap.CustomPins;
                Control.GetMapAsync(this);
            }
        }

        protected override void OnMapReady(GoogleMap map)
        {
            base.OnMapReady(map);

            NativeMap.InfoWindowClick += OnInfoWindowClick;
            NativeMap.SetInfoWindowAdapter(this);
        }
        ...
    }
}
```

Za předpokladu, že vlastní zobrazovací jednotky je připojen k nový prvek Xamarin.Forms `OnElementChanged` volání metod `MapView.GetMapAsync` metodu, která získá základní `GoogleMap` , který se váže k zobrazení. Jednou `GoogleMap` instance je k dispozici, `OnMapReady` přepsání, který bude vyvolán. Metoda registruje obslužná rutina události `InfoWindowClick` událost, která se vyvolá, když [informační okno dojde ke kliknutí na](#Clicking_on_the_Info_Window)a je jenom v případě, že element zobrazovací jednotky je připojený k změny zrušili odběr. `OnMapReady` Přepsat také volání `SetInfoWindowAdapter` metoda určit, že `CustomMapRenderer` instance třídy se poskytují metody pro přizpůsobení informační okno.

`CustomMapRenderer` Implementuje třída `GoogleMap.IInfoWindowAdapter` rozhraní při [přizpůsobit informační okno](#Customizing_the_Info_Window). Toto rozhraní určuje, že musí být implementované následujících metod:

- `public Android.Views.View GetInfoWindow(Marker marker)` – Tato metoda je volána k vrácení informací o vlastní okno pro značku. Vrátí-li `null`, pak bude použita výchozí vykreslování oken. Vrátí-li `View`, pak, která `View` budou umístěny uvnitř okna rámce informace.
- `public Android.Views.View GetInfoContents(Marker marker)` – Tato metoda je volána k vrácení `View` s obsahem v okně informací a bude volat pouze pokud `GetInfoWindow` vrátí metoda `null`. Vrátí-li `null`, pak bude použita výchozí vykreslování obsahu okna informace.

V ukázkové aplikaci je přizpůsobený pouze informace o obsahu okna a proto `GetInfoWindow` vrátí metoda `null` aby to bylo.

#### <a name="customizing-the-marker"></a>Přizpůsobení značky

Ikona použitá pro znázornění značku je možné přizpůsobit pomocí volání `MarkerOptions.SetIcon` metody. Toho můžete docílit tak, že přepíšete `CreateMarker` metoda, která se vyvolá pro každou `Pin` , který je přidán do mapování:

```csharp
protected override MarkerOptions CreateMarker(Pin pin)
{
    var marker = new MarkerOptions();
    marker.SetPosition(new LatLng(pin.Position.Latitude, pin.Position.Longitude));
    marker.SetTitle(pin.Label);
    marker.SetSnippet(pin.Address);
    marker.SetIcon(BitmapDescriptorFactory.FromResource(Resource.Drawable.pin));
    return marker;
}
```

Tato metoda vytvoří nový `MarkerOption` instance pro každý `Pin` instance. Po nastavení pozice, popisek a adresu značky, jeho ikonu nastavená `SetIcon` metody. Tato metoda přebírá `BitmapDescriptor` objekt, který obsahuje data potřebná k vykreslení na ikonu s `BitmapDescriptorFactory` třída poskytující pomocné metody pro zjednodušení vytváření `BitmapDescriptor`.

Další informace o používání `BitmapDescriptorFactory` třídy upravit značku, přečtěte si téma [přizpůsobení značku](~/android/platform/maps-and-location/maps/maps-api.md).

<a name="Customizing_the_Info_Window" />

#### <a name="customizing-the-info-window"></a>Přizpůsobení informační okno

Když uživatel klepne na značku, `GetInfoContents` metoda provádí, za předpokladu, že `GetInfoWindow` vrátí metoda `null`. Následující příklad kódu ukazuje `GetInfoContents` metody:

```csharp
public Android.Views.View GetInfoContents (Marker marker)
{
  var inflater = Android.App.Application.Context.GetSystemService (Context.LayoutInflaterService) as Android.Views.LayoutInflater;
  if (inflater != null) {
    Android.Views.View view;

    var customPin = GetCustomPin (marker);
    if (customPin == null) {
      throw new Exception ("Custom pin not found");
    }

    if (customPin.Id.ToString() == "Xamarin") {
      view = inflater.Inflate (Resource.Layout.XamarinMapInfoWindow, null);
    } else {
      view = inflater.Inflate (Resource.Layout.MapInfoWindow, null);
    }

    var infoTitle = view.FindViewById<TextView> (Resource.Id.InfoWindowTitle);
    var infoSubtitle = view.FindViewById<TextView> (Resource.Id.InfoWindowSubtitle);

    if (infoTitle != null) {
      infoTitle.Text = marker.Title;
    }
    if (infoSubtitle != null) {
      infoSubtitle.Text = marker.Snippet;
    }

    return view;
  }
  return null;
}
```

Tato metoda vrátí hodnotu `View` obsahující informační okno. To lze provést následujícím způsobem:

- A `LayoutInflater` načíst instanci. Tento parametr slouží k vytvoření instance XML soubor rozložení do odpovídající `View`.
- `GetCustomPin` Metoda je volána k vrácení dat vlastní kód pin pro informační okno.
- `XamarinMapInfoWindow` Rozložení je zvětšený, pokud `CustomPin.Id` rovná vlastnost `Xamarin`. V opačném případě `MapInfoWindow` zvětšený rozložení. To umožňuje scénáře, kde můžou zobrazit různé informace o rozložení oken různých značek.
- `InfoWindowTitle` a `InfoWindowSubtitle` zdroje se načítají z zvýšeným rozložení a jejich `Text` vlastnosti jsou nastavené na odpovídající data z `Marker` instance, za předpokladu, že prostředky nejsou `null`.
- `View` Instance se vrátí k zobrazení na mapě.

> [!NOTE]
> Informační okno není živé `View`. Místo toho se Android převede `View` na statickou rastrového obrázku a zobrazit je jako obrázek. To znamená, které při informační okno můžou reagovat na událost click, nemůže reagovat na gesta ani dotyků a jednotlivých ovládacích prvků v okně informace nemůže reagovat na své vlastní události kliknutí.

<a name="Clicking_on_the_Info_Window" />

#### <a name="clicking-on-the-info-window"></a>Kliknutím na informační okno

Když uživatel klikne na informační okno `InfoWindowClick` dojde k aktivaci události, které následně provede `OnInfoWindowClick` metoda:

```csharp
void OnInfoWindowClick (object sender, GoogleMap.InfoWindowClickEventArgs e)
{
  var customPin = GetCustomPin (e.Marker);
  if (customPin == null) {
    throw new Exception ("Custom pin not found");
  }

  if (!string.IsNullOrWhiteSpace (customPin.Url)) {
    var url = Android.Net.Uri.Parse (customPin.Url);
    var intent = new Intent (Intent.ActionView, url);
    intent.AddFlags (ActivityFlags.NewTask);
    Android.App.Application.Context.StartActivity (intent);
  }
}
```

Tato metoda se otevře webový prohlížeč a přejde na adresu uloženou v `Url` vlastnost načtené `CustomPin` instanci `Marker`. Všimněte si, že adresa byla definována při vytváření `CustomPin` kolekce v projektu knihovny .NET Standard.

Další informace o přizpůsobení `MapView` instance najdete v tématu [rozhraní API map](~/android/platform/maps-and-location/maps/maps-api.md).

### <a name="creating-the-custom-renderer-on-the-universal-windows-platform"></a>Vytvoření vlastního Rendereru na Universal Windows Platform

Na následujících snímcích obrazovky zobrazit na mapě před a po přizpůsobení:

![](customized-pin-images/map-layout-uwp.png "Mapový ovládací prvek před a po přizpůsobení")

Na UPW je volána kód pin *ikona mapa*, a může být buď vlastní image nebo systémem definovaná výchozí image. Může zobrazit ikona mapa `UserControl`, který se zobrazí v reakci na uživatele, klepněte na ikonu mapy. `UserControl` Můžete zobrazit žádný obsah, včetně `Label` a `Address` vlastnosti `Pin` instance.

Následující příklad kódu ukazuje vlastní zobrazovací jednotky pro UPW:

```csharp
[assembly: ExportRenderer(typeof(CustomMap), typeof(CustomMapRenderer))]
namespace CustomRenderer.UWP
{
    public class CustomMapRenderer : MapRenderer
    {
        MapControl nativeMap;
        List<CustomPin> customPins;
        XamarinMapOverlay mapOverlay;
        bool xamarinOverlayShown = false;

        protected override void OnElementChanged(ElementChangedEventArgs<Map> e)
        {
            base.OnElementChanged(e);

            if (e.OldElement != null)
            {
                nativeMap.MapElementClick -= OnMapElementClick;
                nativeMap.Children.Clear();
                mapOverlay = null;
                nativeMap = null;
            }

            if (e.NewElement != null)
            {
                var formsMap = (CustomMap)e.NewElement;
                nativeMap = Control as MapControl;
                customPins = formsMap.CustomPins;

                nativeMap.Children.Clear();
                nativeMap.MapElementClick += OnMapElementClick;

                foreach (var pin in customPins)
                {
                    var snPosition = new BasicGeoposition { Latitude = pin.Pin.Position.Latitude, Longitude = pin.Pin.Position.Longitude };
                    var snPoint = new Geopoint(snPosition);

                    var mapIcon = new MapIcon();
                    mapIcon.Image = RandomAccessStreamReference.CreateFromUri(new Uri("ms-appx:///pin.png"));
                    mapIcon.CollisionBehaviorDesired = MapElementCollisionBehavior.RemainVisible;
                    mapIcon.Location = snPoint;
                    mapIcon.NormalizedAnchorPoint = new Windows.Foundation.Point(0.5, 1.0);

                    nativeMap.MapElements.Add(mapIcon);                    
                }
            }
        }
        ...
    }
}
```

`OnElementChanged` Metoda provádí následující operace, za předpokladu, že vlastní zobrazovací jednotky je připojen k nový prvek Xamarin.Forms:

- Vymaže `MapControl.Children` kolekce odeberte všechny existující prvky uživatelského rozhraní z mapy, před registrací obslužná rutina události `MapElementClick` událostí. Tato událost se aktivuje, když uživatel klepne nebo klikne na `MapElement` na `MapControl`a je jenom v případě, že element zobrazovací jednotky je připojený k změny zrušili odběr.
- Každý kód pin v `customPins` kolekce se zobrazí ve správném geografické umístění na mapě následujícím způsobem:
  - Umístění pro kód pin je vytvořen jako `Geopoint` instance.
  - A `MapIcon` představující kód pin je vytvořena instance.
  - Obrázek použitý k reprezentaci `MapIcon` je určený nastavením `MapIcon.Image` vlastnost. Ale mapy obraz bitové kopie není vždy zaručeně ukazuje, jak mohou být skryty pomocí jiných prvků na mapě. Proto mapy ikony `CollisionBehaviorDesired` je nastavena na `MapElementCollisionBehavior.RemainVisible`, aby bylo zajištěno, že zůstává viditelná.
  - Umístění `MapIcon` je určený nastavením `MapIcon.Location` vlastnost.
  - `MapIcon.NormalizedAnchorPoint` Je nastavena na Přibližná poloha ukazatele na obrázku. Pokud tuto vlastnost zachová svou výchozí hodnotu (0; 0), který představuje levý horní roh obrázku, změny v úroveň přiblížení mapy nemusí se bitové kopie odkazující na jiné místo.
  - `MapIcon` Instance je přidána do `MapControl.MapElements` kolekce. Výsledkem je ikona mapa na `MapControl`.

> [!NOTE]
> Při použití stejnou bitovou kopii pro více mapy ikony `RandomAccessStreamReference` by měly být deklarovány instance na úrovni stránky nebo aplikace pro zajištění nejlepšího výkonu.

#### <a name="displaying-the-usercontrol"></a>Zobrazení uživatelský ovládací prvek

Po klepnutí na ikonu mapování `OnMapElementClick` provedení metody. Následující příklad kódu ukazuje tuto metodu:

```csharp
private void OnMapElementClick(MapControl sender, MapElementClickEventArgs args)
{
    var mapIcon = args.MapElements.FirstOrDefault(x => x is MapIcon) as MapIcon;
    if (mapIcon != null)
    {
        if (!xamarinOverlayShown)
        {
            var customPin = GetCustomPin(mapIcon.Location.Position);
            if (customPin == null)
            {
                throw new Exception("Custom pin not found");
            }

            if (customPin.Id.ToString() == "Xamarin")
            {
                if (mapOverlay == null)
                {
                    mapOverlay = new XamarinMapOverlay(customPin);
                }

                var snPosition = new BasicGeoposition { Latitude = customPin.Pin.Position.Latitude, Longitude = customPin.Pin.Position.Longitude };
                var snPoint = new Geopoint(snPosition);

                nativeMap.Children.Add(mapOverlay);
                MapControl.SetLocation(mapOverlay, snPoint);
                MapControl.SetNormalizedAnchorPoint(mapOverlay, new Windows.Foundation.Point(0.5, 1.0));
                xamarinOverlayShown = true;
            }
        }
        else
        {
            nativeMap.Children.Remove(mapOverlay);
            xamarinOverlayShown = false;
        }
    }
}
```

Tato metoda vytvoří `UserControl` instanci, která zobrazí informace o kódu pin. To lze provést následujícím způsobem:

- `MapIcon` Načíst instanci.
- `GetCustomPin` Metoda je volána k vrácení dat vlastní kód pin, který se zobrazí.
- A `XamarinMapOverlay` pro zobrazení údajů o vlastní kód pin je vytvořena instance. Tato třída je uživatelský ovládací prvek.
- Zeměpisné umístění, ve kterém se má zobrazit `XamarinMapOverlay` instance na `MapControl` je vytvořen jako `Geopoint` instance.
- `XamarinMapOverlay` Instance je přidána do `MapControl.Children` kolekce. Tato kolekce obsahuje prvky uživatelského rozhraní XAML, které se zobrazí na mapě.
- Zeměpisné umístění `XamarinMapOverlay` instance na mapě je nastavena pomocí volání `SetLocation` metoda.
- Relativní umístění v `XamarinMapOverlay` instance, která odpovídá zadané umístění, je nastavit pomocí volání `SetNormalizedAnchorPoint` metody. Tím se zajistí, která se změní úroveň přiblížení mapy výsledku `XamarinMapOverlay` instance vždy zobrazí ve správném umístění.

Případně, pokud už se zobrazí informace o PIN kód na mapě, klepnete na mapě odebere `XamarinMapOverlay` z instance `MapControl.Children` kolekce.

#### <a name="tapping-on-the-information-button"></a>Klepnutím na tlačítko informace

Když uživatel klepne na *informace* tlačítko `XamarinMapOverlay` uživatelský ovládací prvek, `Tapped` dojde k aktivaci události, které se spouštějí zase `OnInfoButtonTapped` – metoda:

```csharp
private async void OnInfoButtonTapped(object sender, TappedRoutedEventArgs e)
{
    await Launcher.LaunchUriAsync(new Uri(customPin.Url));
}
```

Tato metoda se otevře webový prohlížeč a přejde na adresu uloženou v `Url` vlastnost `CustomPin` instance. Všimněte si, že adresa byla definována při vytváření `CustomPin` kolekce v projektu knihovny .NET Standard.

Další informace o přizpůsobení `MapControl` instance najdete v tématu [mapy a přehled umístění](https://msdn.microsoft.com/library/windows/apps/mt219699.aspx) na webové stránce MSDN.

## <a name="summary"></a>Souhrn

V tomto článku jsme vám ukázali jak vytvořit vlastního rendereru pro `Map` ovládacího prvku, umožňuje vývojářům přepsat výchozí nativní vykreslení s jejich přizpůsobováním specifické pro platformu. Xamarin.Forms.Maps není úspěšný kvůli poskytuje abstrakce napříč platformami pro zobrazení mapy, které používají nativní mapování rozhraní API na každou platformu zajistit rychlé a důvěrně známé mapování prostředí pro uživatele.


## <a name="related-links"></a>Související odkazy

- [Ovládací prvek mapy](~/xamarin-forms/user-interface/map.md)
- [iOS mapy](~/ios/user-interface/controls/ios-maps/index.md)
- [Rozhraní API služby Map](~/android/platform/maps-and-location/maps/maps-api.md)
- [Vlastní kód Pin (ukázka)](https://developer.xamarin.com/samples/xamarin-forms/customrenderers/map/pin/)
