---
title: Přizpůsobení Map kódu Pin
description: Tento článek ukazuje, jak vytvořit vlastní zobrazovací jednotky pro mapový ovládací prvek, který zobrazí nativní mapa s vlastní kód pin a vlastní zobrazení dat PIN kódu na jednotlivých platformách.
ms.prod: xamarin
ms.assetid: C5481D86-80E9-4E3D-9FB6-57B0F93711A6
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 11/29/2017
ms.openlocfilehash: 029dbf073f61e3a07ec01da4f877bf997af57d98
ms.sourcegitcommit: d80d93957040a14b4638a91b0eac797cfaade840
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 06/07/2018
ms.locfileid: "34848548"
---
# <a name="customizing-a-map-pin"></a>Přizpůsobení Map kódu Pin

_Tento článek ukazuje, jak vytvořit vlastní zobrazovací jednotky pro mapový ovládací prvek, který zobrazí nativní mapa s vlastní kód pin a vlastní zobrazení dat PIN kódu na jednotlivých platformách._

Každé zobrazení Xamarin.Forms má doprovodné zobrazovací jednotky pro každou platformu, která vytvoří instanci nativní ovládacího prvku. Když [ `Map` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Maps.Map/) je vykreslen metodou Xamarin.Forms aplikace v iOS, `MapRenderer` vytvoření instance třídy, které pak vytvoří nativní `MKMapView` ovládacího prvku. Na platformě Android `MapRenderer` třída vytvoří nativní `MapView` ovládacího prvku. Na univerzální platformu Windows (UWP), `MapRenderer` třída vytvoří nativní `MapControl`. Další informace o zobrazovací jednotky a třídy nativní ovládacích prvků, které ovládací prvky Xamarin.Forms mapování na najdete v tématu [zobrazovací jednotky základní třídy a nativní ovládací prvky](~/xamarin-forms/app-fundamentals/custom-renderer/renderers.md).

Následující diagram znázorňuje vztah mezi [ `Map` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Maps.Map/) a odpovídající nativní ovládací prvky, které implementují ho:

![](customized-pin-images/map-classes.png "Vztah mezi mapový ovládací prvek a implementuje nativní ovládací prvky")

Proces vykreslování lze použít k implementaci přizpůsobení specifické pro platformu tak, že vytvoříte vlastní zobrazovací jednotky pro [ `Map` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Maps.Map/) na každou platformu. Proces pro to vypadá takto:

1. [Vytvoření](#Creating_the_Custom_Map) Xamarin.Forms vlastní mapování.
1. [Využívat](#Consuming_the_Custom_Map) vlastní mapy z Xamarin.Forms.
1. [Vytvoření](#Creating_the_Custom_Renderer_on_each_Platform) vlastní zobrazovací jednotky pro mapu na každou platformu.

Každá položka teď probereme pak implementovat `CustomMap` zobrazovací jednotky, která zobrazuje nativní mapa s vlastní kód pin a vlastní zobrazení dat PIN kódu na jednotlivých platformách.

> [!NOTE]
> [`Xamarin.Forms.Maps`](https://developer.xamarin.com/api/namespace/Xamarin.Forms.Maps/) musí být inicializovaná a před použitím. Další informace najdete na webu [`Maps Control`](~/xamarin-forms/user-interface/map.md).

<a name="Creating_the_Custom_Map" />

## <a name="creating-the-custom-map"></a>Vytváření vlastních mapy

Vytváření podtříd můžete vytvořit vlastní mapový ovládací prvek [ `Map` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Maps.Map/) třídy, jak je znázorněno v následujícím příkladu kódu:

```csharp
public class CustomMap : Map
{
  public List<CustomPin> CustomPins { get; set; }
}
```

`CustomMap` Řízení je vytvořen v rozhraní .NET standardní projektu knihovny a definuje rozhraní API pro vlastní mapy. Zpřístupňuje vlastní mapy `CustomPins` vlastnost, která představuje kolekci `CustomPin` objekty, které bude vykreslen pomocí nativní mapový ovládací prvek na každou platformu. `CustomPin` Třída je znázorněno v následujícím příkladu kódu:

```csharp
public class CustomPin : Pin
{
  public string Url { get; set; }
}
```

Tato třída definuje `CustomPin` jako dědí vlastnosti [ `Pin` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Maps.Pin/) třídy a přidávání `Url` vlastnost.

<a name="Consuming_the_Custom_Map" />

## <a name="consuming-the-custom-map"></a>Použití vlastní mapy

`CustomMap` Řízení může odkazovat v jazyce XAML v rozhraní .NET standardní projektu knihovny deklarace oboru názvů pro umístění a použití Předpona oboru názvů na vlastní mapový ovládací prvek. Následující příklad kódu ukazuje jak `CustomMap` řízení mohou být spotřebovávána stránky XAML:

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

`local` Předponu oboru názvů můžete pojmenovat. Ale `clr-namespace` a `assembly` hodnoty musí odpovídat podrobnosti vlastní mapy. Jakmile je deklarován obor názvů, je předpona, která slouží k odkazování vlastní mapy.

Následující příklad kódu ukazuje jak `CustomMap` řízení mohou být spotřebovávána stránky C#:

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

`CustomMap` Instance bude sloužit k zobrazení nativní mapy na každou platformu. Má [ `MapType` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Maps.Map.MapType/) vlastnost nastaví styl zobrazení [ `Map` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Maps.Map/), s možné hodnoty, které definujete ve [ `MapType` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Maps.MapType/) výčtu. Pro iOS a Android, šířku a výšku mapy nastavit pomocí vlastnosti `App` třídu, která jsou inicializovány v projektech specifické pro platformu.

Umístění mapy a kódy PIN obsahuje, se inicializují, jak je znázorněno v následujícím příkladu kódu:

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

Tato inicializace přidá vlastní kód pin a umisťuje zobrazení mapy na s [ `MoveToRegion` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Maps.Map.MoveToRegion(Xamarin.Forms.Maps.MapSpan)/) metoda, která mění pozice a úroveň přiblížení mapy vytvořením [ `MapSpan` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Maps.MapSpan/) z [ `Position` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Maps.Position/) a [ `Distance` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Maps.Distance/).

Vlastní zobrazovací jednotky lze nyní přidat na každý projekt aplikace k přizpůsobení ovládací prvky nativní mapy.

<a name="Creating_the_Custom_Renderer_on_each_Platform" />

## <a name="creating-the-custom-renderer-on-each-platform"></a>Vytváření vlastní zobrazovací jednotky na jednotlivých platformách

Proces pro vytvoření třídy vlastní zobrazovací jednotky vypadá takto:

1. Vytvoření podtřídou třídy `MapRenderer` třídu, která vykreslí vlastní mapy.
1. Přepsání `OnElementChanged` metoda, která vykreslí mapy a logiku zápisu a přizpůsobit. Tato metoda je volána, když je vytvořen odpovídající Xamarin.Forms vlastní mapy.
1. Přidat `ExportRenderer` atributu na vlastní zobrazovací jednotky třídu k určení, že bude použit k vykreslení Xamarin.Forms vlastní mapy. Tento atribut slouží k registraci vlastní zobrazovací jednotky s Xamarin.Forms.

> [!NOTE]
> Zadání je volitelné poskytnout vlastní zobrazovací jednotky v každém projektu platformy. Pokud vlastní zobrazovací jednotky není registrované, bude použit výchozí zobrazovací jednotky pro základní třídu ovládacího prvku.

Následující diagram znázorňuje odpovědnosti jednotlivých projektů v ukázkové aplikace, spolu s jejich vzájemných vztahů:

![](customized-pin-images/solution-structure.png "CustomMap vlastní zobrazovací jednotky projektu odpovědnosti")

`CustomMap` Vykreslení ovládacího prvku renderer specifické pro platformu třídy, odvozených od `MapRenderer` třídu pro každou platformu. Výsledkem je každý `CustomMap` řízení vykreslované s ovládacími prvky specifické pro platformu, jak je vidět na následujících snímcích obrazovky:

![](customized-pin-images/screenshots.png "CustomMap na jednotlivých platformách")

`MapRenderer` Třídy zpřístupňuje `OnElementChanged` metodu, která je volána, když vlastní mapy Xamarin.Forms se vytvoří pro vykreslení odpovídající nativní ovládacího prvku. Tato metoda přebírá `ElementChangedEventArgs` parametr, který obsahuje `OldElement` a `NewElement` vlastnosti. Tyto vlastnosti představují Xamarin.Forms element, zobrazovací jednotky *byla* připojené a Xamarin.Forms element, zobrazovací jednotky *je* připojené k, v uvedeném pořadí. V ukázkové aplikaci `OldElement` , bude mít vlastnost `null` a `NewElement` vlastnost bude obsahovat odkaz na `CustomMap` instance.

Přepsané verzi `OnElementChanged` metoda v každé třídě renderer specifických pro platformy je místo, kde provádět přizpůsobení nativní ovládací prvek. Zadaný odkaz na nativní ovládací prvek používá na platformě je možné přistupovat prostřednictvím `Control` vlastnost. Kromě toho nelze získat odkaz na platformě Xamarin.Forms ovládací prvek, který je vykreslované prostřednictvím `Element` vlastnost.

Musí dát pozor, když se přihlásíte k obslužné rutiny událostí v odběru `OnElementChanged` metoda, jak je ukázáno v následujícím příkladu kódu:

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

Nativní ovládacího prvku by měl být nakonfigurovaný a jenom v případě, že vlastní zobrazovací jednotky je připojen k nového elementu Xamarin.Forms přihlásit k odběru obslužné rutiny událostí. Podobně všechny obslužné rutiny, které byly přihlásit k odběru musí být v po elementu, který zobrazovací jednotky je připojena k pouze změny odhlásit. Přijetí tento přístup vám pomůže vytvořit vlastní zobrazovací jednotky, která není trpí nevracení paměti.

Každá třída vlastní zobrazovací jednotky je upraven pomocí `ExportRenderer` atribut, který registruje zobrazovací jednotky s Xamarin.Forms. Atribut přebírá dva parametry – název typu vlastního ovládacího prvku Xamarin.Forms vykreslované a název typu vlastní zobrazovací jednotky. `assembly` Předpona, která má atribut určuje atribut, které se vztahují na celou sestavení.

Následující části popisují implementaci třídy každý vlastní zobrazovací jednotky specifické pro platformu.

### <a name="creating-the-custom-renderer-on-ios"></a>Vytváření vlastní zobrazovací jednotky v systému iOS

Na následujících snímcích obrazovky zobrazit na mapě před a po přizpůsobení:

![](customized-pin-images/map-layout-ios.png "Mapový ovládací prvek před a po přizpůsobení")

V systému iOS se nazývá kódu pin *poznámky*, a může být buď vlastní image nebo PIN kód definovaná systémem různých barev. Volitelně můžete zobrazit poznámky *popisku*, který se zobrazí v reakci na uživatele vybráním poznámky. Zobrazí popisek `Label` a `Address` vlastnosti `Pin` instance s volitelné v levém horním a pravém příslušenství zobrazení. Na snímku obrazovky výše, je z levého zobrazení příslušenství bitové kopie opic s právo příslušenství zobrazení se *informace* tlačítko.

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

`OnElementChanged` Metoda provádí následující konfigurace [ `MKMapView` ](https://developer.xamarin.com/api/type/MapKit.MKMapView/) instance, za předpokladu, že vlastní zobrazovací jednotky je připojen k nového elementu Xamarin.Forms:

- [ `GetViewForAnnotation` ](https://developer.xamarin.com/api/property/MapKit.MKMapView.GetViewForAnnotation/) Je nastavena na `GetViewForAnnotation` metoda. Tato metoda je volána, když [umístění Poznámka se zobrazí na mapě](#Displaying_the_Annotation)a slouží k přizpůsobení před poznámky k zobrazení.
- Obslužné rutiny události pro `CalloutAccessoryControlTapped`, `DidSelectAnnotationView`, a `DidDeselectAnnotationView` události jsou registrované. Provést tyto události, kdy uživatel [klepne na pravém příslušenství v popisku](#Tapping_on_the_Right_Callout_Accessory_View)a když uživatel [vybere](#Selecting_the_Annotation) a [zruší výběr](#Deselecting_the_Annotation) poznámky, v uvedeném pořadí. Události jsou v odhlásit pouze v případě, že element zobrazovací jednotky je připojen k změny.

<a name="Displaying_the_Annotation" />

#### <a name="displaying-the-annotation"></a>Zobrazení anotace

`GetViewForAnnotation` Metoda je volána, když umístění Poznámka se zobrazí na mapě a slouží k přizpůsobení před poznámky k zobrazení. Poznámky má dvě části:

- `MkAnnotation` – obsahuje název, subtitle a umístění anotace.
- `MkAnnotationView` – obsahuje bitovou kopii k reprezentaci anotace a volitelně popisků, které se zobrazí, když uživatel klepnutím anotace.

`GetViewForAnnotation` Metoda přijímá `IMKAnnotation` který obsahuje data, poznámky a vrátí `MKAnnotationView` pro zobrazení na mapě a je znázorněno v následujícím příkladu kódu:

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

Tato metoda zajišťuje, že poznámka se zobrazí jako vlastní image, nikoli jako definované v systému a kódu pin, který po Poznámka je stisknuté popisku se zobrazí obsahující další obsah vlevo a vpravo od poznámky název a adresu . To lze provést následujícím způsobem:

1. `GetCustomPin` Metoda je volána pro vrácení dat vlastní kód pin pro poznámku.
1. Pro konzervaci paměti, poznámky zobrazení je ve fondu pro opakované použití s volání [ `DequeueReusableAnnotation` ](https://developer.xamarin.com/api/member/MapKit.MKMapView.DequeueReusableAnnotation/(System.String)/).
1. `CustomMKAnnotationView` Třída rozšiřuje `MKAnnotationView` třídy s `Id` a `Url` vlastnosti, které odpovídají na stejné vlastnosti v `CustomPin` instance. Novou instanci třídy `CustomMKAnnotationView` je vytvořen, za předpokladu, že je Poznámka `null`:
  - `CustomMKAnnotationView.Image` Je nastavena na bitovou kopii, která bude představovat poznámky na mapě.
  - `CustomMKAnnotationView.CalloutOffset` Je nastavena na `CGPoint` který určuje, že popisek bude vycházet výše anotace.
  - `CustomMKAnnotationView.LeftCalloutAccessoryView` Je nastavena na obrázek opic, který se zobrazí vlevo od poznámky název a adresu.
  - `CustomMKAnnotationView.RightCalloutAccessoryView` Je nastavena na *informace* tlačítko, které se zobrazí vpravo od poznámky název a adresu.
  - `CustomMKAnnotationView.Id` Je nastavena na `CustomPin.Id` vlastnost vrácený `GetCustomPin` metoda. To umožňuje anotaci identifikaci tak, aby měl [popisku mohou být dále uzpůsobeny](#Selecting_the_Annotation), v případě potřeby.
  - `CustomMKAnnotationView.Url` Je nastavena na `CustomPin.Url` vlastnost vrácený `GetCustomPin` metoda. Adresu URL se přejde poté, kdy uživatel [klepnutím na tlačítko se zobrazí v pravém popisku příslušenství zobrazení](#Tapping_on_the_Right_Callout_Accessory_View).
1. [ `MKAnnotationView.CanShowCallout` ](https://developer.xamarin.com/api/property/MapKit.MKAnnotationView.CanShowCallout/) Je nastavena na `true` tak, aby popisku se zobrazí, když je stisknuté anotace.
1. Poznámka vrátí k zobrazení na mapě.

<a name="Selecting_the_Annotation" />

#### <a name="selecting-the-annotation"></a>Výběr anotace

Když uživatel klepnutím na poznámku, `DidSelectAnnotationView` aktivuje událost, které pak provede `OnDidSelectAnnotationView` metoda:

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

Tato metoda rozšiřuje existující popisek (která obsahuje zobrazení levé a pravé příslušenství) přidáním `UIView` instance k němu, který obsahuje obrázek loga Xamarin, za předpokladu, že vybrané poznámky jeho `Id` vlastnost nastavena na hodnotu `Xamarin`. To umožňuje scénáře, kde lze zobrazit různé popisky pro různé poznámky. `UIView` Instance se zobrazí na střed výše existující popisku.

<a name="Tapping_on_the_Right_Callout_Accessory_View" />

#### <a name="tapping-on-the-right-callout-accessory-view"></a>Klepnutím na zobrazení správné příslušenství popisku

Když uživatel klepnutím na *informace* tlačítko v pravém popisku příslušenství zobrazení `CalloutAccessoryControlTapped` aktivuje událost, které pak provede `OnCalloutAccessoryControlTapped` metoda:

```csharp
void OnCalloutAccessoryControlTapped (object sender, MKMapViewAccessoryTappedEventArgs e)
{
  var customView = e.View as CustomMKAnnotationView;
  if (!string.IsNullOrWhiteSpace (customView.Url)) {
    UIApplication.SharedApplication.OpenUrl (new Foundation.NSUrl (customView.Url));
  }
}
```

Tato metoda otevře webový prohlížeč a přejde na adresu uložené v `CustomMKAnnotationView.Url` vlastnost. Všimněte si, že adresa byla definována při vytváření `CustomPin` kolekce v rozhraní .NET standardní projektu knihovny.

<a name="Deselecting_the_Annotation" />

#### <a name="deselecting-the-annotation"></a>Ať anotace

Když Poznámka se zobrazí a uživatel klepnutím na mapě, `DidDeselectAnnotationView` aktivuje událost, které pak provede `OnDidDeselectAnnotationView` metoda:

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

Tato metoda zajišťuje, že pokud není vybrána existující popisku, rozšířené součástí popisku (obrázek loga Xamarin) také zastaví se zobrazuje, a její prostředky, které budou vydané.

Další informace o přizpůsobení `MKMapView` instance najdete v tématu [iOS mapy](~/ios/user-interface/controls/ios-maps/index.md).

### <a name="creating-the-custom-renderer-on-android"></a>Vytváření vlastní zobrazovací jednotky v systému Android

Na následujících snímcích obrazovky zobrazit na mapě před a po přizpůsobení:

![](customized-pin-images/map-layout-android.png "Mapový ovládací prvek před a po přizpůsobení")

V systému Android se nazývá PIN kód *značky*, a může být buď vlastní image nebo značku definovaná systémem různých barev. Můžete zobrazit značek *informační okno*, který se zobrazí v reakci na uživatel klepnutím na značky. V okně informace se zobrazí `Label` a `Address` vlastnosti `Pin` instance a můžete upravit, aby obsahoval jiný obsah. Pouze jeden informační okno však lze zobrazit najednou.

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

Za předpokladu, že vlastní zobrazovací jednotky je připojen k nového elementu Xamarin.Forms, `OnElementChanged` volání metod `MapView.GetMapAsync` metodu, která získá základní `GoogleMap` který je vázaný na zobrazení. Jednou `GoogleMap` instance je k dispozici, `OnMapReady` přepsání bude volána. Metoda registruje obslužné rutiny události pro `InfoWindowClick` událost, která aktivuje, když [po kliknutí na informační okno](#Clicking_on_the_Info_Window)a je odhlásil z jenom v případě, že element zobrazovací jednotky je připojen k změny. `OnMapReady` Přepsat také volání `SetInfoWindowAdapter` metoda určíte, že `CustomMapRenderer` instance třídy bude poskytovat metody k přizpůsobení okně informace.

`CustomMapRenderer` Třída implementuje `GoogleMap.IInfoWindowAdapter` rozhraní k [přizpůsobit informační okno](#Customizing_the_Info_Window). Toto rozhraní určuje, že musí být implementována následujících metod:

- `public Android.Views.View GetInfoWindow(Marker marker)` – Tato metoda je volána vrátit okna vlastní informace pro značku. Vrátí-li `null`, pak vykreslování oken ve výchozím nastavení se použije. Vrátí-li `View`, pak který `View` budou umístěny uvnitř rámce okna informace.
- `public Android.Views.View GetInfoContents(Marker marker)` – Tato metoda je volána vrátit `View` s obsahem okna informací a bude volat pouze pokud `GetInfoWindow` metoda vrátí `null`. Vrátí-li `null`, pak se použije výchozí vykreslování obsahu okna informace.

V ukázkové aplikaci pouze obsah, okno informace upravíte a proto `GetInfoWindow` metoda vrátí `null` Chcete-li povolit tuto funkci.

#### <a name="customizing-the-marker"></a>Přizpůsobení značky

Ikona použitá pro znázornění značku lze přizpůsobit pomocí volání `MarkerOptions.SetIcon` metoda. Toho lze dosáhnout přepsáním `CreateMarker` metoda, která se vyvolá pro každou `Pin` , se přidá do mapy:

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

Tato metoda vytvoří novou `MarkerOption` instance pro každý `Pin` instance. Po nastavení pozice, popisek a adresy značky, jeho ikonu nastavena `SetIcon` metoda. Tato metoda přebírá `BitmapDescriptor` objekt obsahující data potřebná k vykreslení na ikonu s `BitmapDescriptorFactory` třída poskytuje pomocné metody pro zjednodušení vytváření `BitmapDescriptor`.

Další informace o používání `BitmapDescriptorFactory` třída přizpůsobit značku, najdete v článku [přizpůsobení značku](~/android/platform/maps-and-location/maps/maps-api.md).

<a name="Customizing_the_Info_Window" />

#### <a name="customizing-the-info-window"></a>Okno informace o přizpůsobení

Když uživatel klepnutím na značky, `GetInfoContents` metoda proveden, za předpokladu, že `GetInfoWindow` metoda vrátí `null`. Následující příklad kódu ukazuje `GetInfoContents` metoda:

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

Tato metoda vrátí hodnotu `View` obsahující obsah okně informace. To lze provést následujícím způsobem:

- A `LayoutInflater` instance je načíst. Slouží k vytváření instancí rozložení souboru XML do její odpovídající `View`.
- `GetCustomPin` Metoda je volána pro vrácení dat vlastní kód pin pro informační okno.
- `XamarinMapInfoWindow` Rozložení je zvětšený, pokud `CustomPin.Id` vlastnost rovná `Xamarin`. Jinak `MapInfoWindow` je zvětšený rozložení. To umožňuje scénáře, kde lze zobrazit různé informace o rozložení oken pro jiné značky.
- `InfoWindowTitle` a `InfoWindowSubtitle` prostředky jsou načteny z zvýšeným rozložení a jejich `Text` vlastnosti jsou nastaveny na odpovídající data z `Marker` instance, za předpokladu, že prostředky nejsou `null`.
- `View` Instance vrátí k zobrazení na mapě.

> [!NOTE]
> Okno s informací není živé `View`. Místo toho Android převede `View` do statického bitmap a které zobrazit jako obrázek. To znamená, které při okno s informací mohou reagovat na událost click, nemůže reagovat na všechny události touch nebo gesta a jednotlivých ovládacích prvků v okně informace o nemůže reagovat na vlastních události kliknutí.

<a name="Clicking_on_the_Info_Window" />

#### <a name="clicking-on-the-info-window"></a>Kliknutím na informační okno

Když uživatel klikne v okně informace `InfoWindowClick` aktivuje událost, které pak provede `OnInfoWindowClick` metoda:

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

Tato metoda otevře webový prohlížeč a přejde na adresu uložené v `Url` vlastnost načtené `CustomPin` instanci pro `Marker`. Všimněte si, že adresa byla definována při vytváření `CustomPin` kolekce v rozhraní .NET standardní projektu knihovny.

Další informace o přizpůsobení `MapView` instance najdete v tématu [rozhraní API map](~/android/platform/maps-and-location/maps/maps-api.md).

### <a name="creating-the-custom-renderer-on-the-universal-windows-platform"></a>Vytváření vlastní zobrazovací jednotky na univerzální platformu Windows

Na následujících snímcích obrazovky zobrazit na mapě před a po přizpůsobení:

![](customized-pin-images/map-layout-uwp.png "Mapový ovládací prvek před a po přizpůsobení")

Na UWP se nazývá PIN kód *ikonu mapy*, a může být buď vlastní image nebo definovaná systémem výchozí image. Můžete zobrazit ikonu mapy `UserControl`, který se zobrazí v reakci na uživatel klepnutím na ikonu mapy. `UserControl` Můžete zobrazit všechny obsah, včetně `Label` a `Address` vlastnosti `Pin` instance.

Následující příklad kódu ukazuje UWP vlastní zobrazovací jednotky:

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

`OnElementChanged` Metoda provede následující operace, za předpokladu, že vlastní zobrazovací jednotky je připojen k nového elementu Xamarin.Forms:

- Vypne `MapControl.Children` kolekce k odebrání všech existujících prvky uživatelského rozhraní mapy, před registrací obslužné rutiny události pro `MapElementClick` událostí. Tato událost se aktivuje, když uživatel klepnutím nebo klikne na `MapElement` na `MapControl`a je odhlásil z jenom v případě, že element zobrazovací jednotky je připojen k změny.
- Každý kód pin v `customPins` kolekce se zobrazí ve správném geografické umístění na mapě následujícím způsobem:
  - Umístění pro PIN kód je vytvořen jako `Geopoint` instance.
  - A `MapIcon` představují PIN kód se vytvoří instance.
  - Bitovou kopii používá k reprezentování `MapIcon` je určený nastavením `MapIcon.Image` vlastnost. Obrázek ikony mapy není však vždy zaručena zobrazený, jako mohou být skryty jinými prvky na mapě. Proto mapy ikony `CollisionBehaviorDesired` je nastavena na `MapElementCollisionBehavior.RemainVisible`, aby bylo zajištěno, že zůstává viditelná.
  - Umístění `MapIcon` je určený nastavením `MapIcon.Location` vlastnost.
  - `MapIcon.NormalizedAnchorPoint` Je nastavena na přibližnou polohu ukazatele na bitovou kopii. Pokud tuto vlastnost zachová výchozí hodnota (0,0), který představuje levém horním rohu bitovou kopii, změny v úroveň přiblížení mapy může vést k bitové kopie odkazující na jiné umístění.
  - `MapIcon` Instance je přidán do `MapControl.MapElements` kolekce. Výsledkem je na mapu ikonu se zobrazí na `MapControl`.

> [!NOTE]
> Při použití stejnou bitovou kopii pro více mapy ikony `RandomAccessStreamReference` instance musí být deklarován na úrovni stránky nebo aplikace pro nejlepší výkon.

#### <a name="displaying-the-usercontrol"></a>Zobrazení UserControl

Když uživatel klepnutím na ikonu mapy `OnMapElementClick` metoda spuštěna. Následující příklad kódu ukazuje této metody:

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

Tato metoda vytvoří `UserControl` instance, který zobrazí informace o kódu pin. To lze provést následujícím způsobem:

- `MapIcon` Instance je načíst.
- `GetCustomPin` Metoda je volána vrátit data vlastní pin, který se zobrazí.
- A `XamarinMapOverlay` je vytvořena instance pro zobrazení dat vlastní kód pin. Tato třída je uživatelský ovládací prvek.
- Zeměpisné umístění, kam chcete zobrazit `XamarinMapOverlay` instance na `MapControl` je vytvořen jako `Geopoint` instance.
- `XamarinMapOverlay` Instance je přidán do `MapControl.Children` kolekce. Tato kolekce obsahuje prvky uživatelského rozhraní jazyka XAML, které se zobrazí na mapě.
- Geografické umístění `XamarinMapOverlay` instance na mapě nastavit tak, že zavoláte `SetLocation` metoda.
- Relativní umístění na `XamarinMapOverlay` instanci, která odpovídá do zadaného umístění, je nastavit, že zavoláte `SetNormalizedAnchorPoint` metoda. To zajistí, která se změní úroveň přiblížení mapy výsledek v `XamarinMapOverlay` instance vždy zobrazují ve správném umístění.

Případně, pokud už se zobrazuje informace o kódu pin na mapě, klepnutím na na mapě odebere `XamarinMapOverlay` instanci z `MapControl.Children` kolekce.

#### <a name="tapping-on-the-information-button"></a>Klepnutím na tlačítko informace

Když uživatel klepnutím na *informace* tlačítko v `XamarinMapOverlay` uživatelský ovládací prvek, `Tapped` aktivuje událost, které pak provede `OnInfoButtonTapped` metoda:

```csharp
private async void OnInfoButtonTapped(object sender, TappedRoutedEventArgs e)
{
    await Launcher.LaunchUriAsync(new Uri(customPin.Url));
}
```

Tato metoda otevře webový prohlížeč a přejde na adresu uložené v `Url` vlastnost `CustomPin` instance. Všimněte si, že adresa byla definována při vytváření `CustomPin` kolekce v rozhraní .NET standardní projektu knihovny.

Další informace o přizpůsobení `MapControl` instance najdete v tématu [Maps a umístění přehled](https://msdn.microsoft.com/library/windows/apps/mt219699.aspx) na webu MSDN.

## <a name="summary"></a>Souhrn

Tento článek ukázal, jak vytvořit vlastní zobrazovací jednotky pro `Map` řízení, umožňuje vývojářům přepsat výchozí nativní vykreslování s vlastní přizpůsobení specifické pro platformu. Xamarin.Forms.Maps poskytuje abstrakci a platformy pro zobrazení mapy, které používají nativní mapy rozhraní API na jednotlivých platformách zajistit mapu rychlého a známým prostředí pro uživatele.


## <a name="related-links"></a>Související odkazy

- [Ovládací prvek map](~/xamarin-forms/user-interface/map.md)
- [iOS mapy](~/ios/user-interface/controls/ios-maps/index.md)
- [Rozhraní API služby Map](~/android/platform/maps-and-location/maps/maps-api.md)
- [Vlastní kód Pin (ukázka)](https://developer.xamarin.com/samples/xamarin-forms/customrenderers/map/pin/)
