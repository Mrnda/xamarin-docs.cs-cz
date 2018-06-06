---
title: Události, protokoly a delegáti v Xamarin.iOS
description: Tento dokument popisuje, jak pracovat s událostmi, protokolů a deleguje v Xamarin.iOS. Tyto základní koncepty jsou všudypřítomný v vývoj na platformě Xamarin.iOS.
ms.prod: xamarin
ms.assetid: 7C07F0B7-9000-C540-0FC3-631C29610447
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/18/2017
ms.openlocfilehash: d0e4c23bffe689c9218da2f43b97d98f348513ad
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 06/05/2018
ms.locfileid: "34784007"
---
# <a name="events-protocols-and-delegates-in-xamarinios"></a>Události, protokoly a delegáti v Xamarin.iOS

Xamarin.iOS používá ovládací prvky ke zveřejnění události pro většinu interakce uživatele.
Aplikace Xamarin.iOS využívat tyto události mnohem stejným způsobem jako tradiční aplikace .NET. Například Xamarin.iOS UIButton třída má událost názvem TouchUpInside a odebírá Tato událost stejně, jako kdyby této třídy a události se aplikace .NET.

Kromě toho tento přístup .NET zpřístupní Xamarin.iOS jiný model, který lze použít pro složitější interakce a datové vazby. Tato metoda používá, jaké Apple volá Delegáti a protokoly. Delegáti jsou v principu podobná Delegáti v jazyce C#, ale místo definování a volání jedné metody, delegát v Objective C je celou třídu, která vyhovuje protokol. Protokol je podobná rozhraní v jazyce C#, s tím rozdílem, že její metody může být volitelné. Tak například k naplnění UITableView s daty, vytvořili byste delegát třídy, která implementuje metody definované v UITableViewDataSource protokol, který UITableView by volání k naplnění sám sebe.

V tomto článku se dozvíte o těchto tématech, která poskytuje sice solidní základ pro zpracování zpětného volání scénáře v Xamarin.iOS, včetně:

-  **Události** – události pomocí rozhraní .NET s ovládacími prvky UIKit.
-  **Protokoly** – učení co protokoly jsou a jak se používají a vytvoření příklad, který poskytuje data pro poznámky mapy.
-  **Delegáti** – zkušenosti o jazyka Objective-C delegáti podle rozšíření Příklad mapy do zpracovávají interakci s uživatelem, který obsahuje poznámky, pak učení rozdíl mezi silné a slabé Delegáti a při použití každé z nich.

Pro ilustraci protokoly a delegáti, využijeme jednoduché mapy aplikace, která přidá poznámky na mapu, jak je vidět tady:

 [![](delegates-protocols-and-events-images/01-map.png "Příklad jednoduchého mapy aplikace, které přidá poznámky mapy") ](delegates-protocols-and-events-images/01-map.png#lightbox) [ ![ ] (delegates-protocols-and-events-images/04-annotation-with-callout.png "příklad poznámky přidat na mapu")](delegates-protocols-and-events-images/04-annotation-with-callout.png#lightbox)

Před boji se této aplikace, můžeme začít pohledem na události UIKit .NET.

<a name=".NET_Events_with_UIKit" />

## <a name="net-events-with-uikit"></a>Události rozhraní .NET s UIKit

Xamarin.iOS zpřístupní události .NET na UIKit ovládací prvky. Například UIButton má TouchUpInside událost, která zpracovává jako za normálních okolností byste v rozhraní .NET, jak je znázorněno v následujícím kódu, který používá výrazu lambda C#:

```csharp
aButton.TouchUpInside += (o,s) => {
    Console.WriteLine("button touched");
};
```
To může implementovat pomocí jazyka C# 2.0 stylu anonymní metody podobné následujícímu:

```csharp
aButton.TouchUpInside += delegate {
    Console.WriteLine ("button touched");
};
```

Předchozí kód je drátové nahoru v metodě ViewDidLoad UIViewContoller. Proměnná aButton odkazuje tlačítko, které můžete přidat v iOS Designer nebo s kódem. Následující obrázek znázorňuje toto tlačítko, který je přidán v iOS Designer převzat ze vzorku, který doprovází v tomto článku:

 [![](delegates-protocols-and-events-images/02-interface-builder-outlet.png "Tlačítko Přidat v iOS návrháře")](delegates-protocols-and-events-images/02-interface-builder-outlet.png#lightbox)

Xamarin.iOS taky podporuje připojení kódu k interakci, ke kterému dochází s ovládacím prvkem styl cíl akce. Pokud chcete vytvořit cíl akce pro tlačítko Hello, na ni dvakrát kliknete na iOS Designer. Zobrazí souboru kódu UIViewController a vývojář se výzva k vyberte umístění, kam chcete vložit připojování metoda:

 [![](delegates-protocols-and-events-images/03-interface-builder-action.png "Soubor UIViewControllers kódu")](delegates-protocols-and-events-images/03-interface-builder-action.png#lightbox)

Po výběru umístění se nová metoda je vytvořen a drátové až do ovládacího prvku. V následujícím příkladu zprávy se zapíšou do konzole při kliknutí na tlačítko:

 [![](delegates-protocols-and-events-images/05-interface-builder-action.png "Zprávu se zapíšou do konzole při kliknutí na tlačítko")](delegates-protocols-and-events-images/05-interface-builder-action.png#lightbox)

Další podrobnosti o vzoru cíl akce iOS, najdete v části cíl-Action " [základní možnosti aplikací pro iOS](http://developer.apple.com/library/ios/#DOCUMENTATION/General/Conceptual/Devpedia-CocoaApp/TargetAction.html)" v společnosti Apple iOS Developer Library.

Další informace o tom, jak používat iOS Návrhář s Xamarin.iOS najdete v tématu " [iOS Návrhář přehled](~/ios/user-interface/designer/index.md)" dokumentaci.

 <a name="Events" />


## <a name="events"></a>Události

Pokud chcete zachytit události z UIControl, máte celou řadu možností: pomocí jazyka C# lambdas a delegáta k pomocí nízké úrovně rozhraní API jazyka Objective-C.

Následující část popisuje, jak by zachycení událostí TouchDown na tlačítko, v závislosti na tom, kolik ovládací prvek, je nutné.

 <a name="C#_Style" />


## <a name="c-style"></a>Styl C#

Pomocí syntaxe delegáta:

```csharp
UIButton button = MakeTheButton ();
button.TouchDown += delegate {    
    Console.WriteLine ("Touched");
};
```

Pokud chcete místo toho lambdas:

```csharp
button.TouchDown += () => {
   Console.WriteLine ("Touched");
};
```

Pokud budete chtít mít více tlačítek pomocí stejné rutiny sdílet stejný kód:

```csharp
void handler (object sender, EventArgs args)
{
   if (sender == button1)
      Console.WriteLine ("button1");
   else
      Console.WriteLine ("some other button");
}

button1.TouchDown += handler;
button2.TouchDown += handler;
```

<a name="Monitoring_more_than_one_kind_of_Event" />


## <a name="monitoring-more-than-one-kind-of-event"></a>Monitorování více než jeden typ události

Události C# pro UIControlEvent příznaky obsahují mapování 1: 1 pro jednotlivé příznaky. Pokud chcete mít stejnou část kódu zpracování dvou nebo více událostí, použijte `UIControl.AddTarget` metoda:

```csharp
button.AddTarget (handler, UIControlEvent.TouchDown | UIControlEvent.TouchCancel);
```

Pomocí syntaxe lambda:

```csharp
button.AddTarget ((sender, event)=> Console.WriteLine ("An event happened"), UIControlEvent.TouchDown | UIControlEvent.TouchCancel);
```

Pokud potřebujete použít nízké úrovně funkce jazyka Objective-C, jako je zapojování do instance určitého objektu a vyvolání konkrétní selektor:

```csharp
[Export ("MySelector")]
void MyObjectiveCHandler ()
{
    Console.WriteLine ("Hello!");
}

// In some other place:

button.AddTarget (this, new Selector ("MySelector"), UIControlEvent.TouchDown);
```

Upozorňujeme, pokud budete implementovat metodu instance v zděděné základní třídu, musí být veřejná metoda.

 <a name="Protocols" />


## <a name="protocols"></a>protokoly

Protokol je funkce jazyka Objective-C, která obsahuje seznam metoda deklarace. Tom, že se protokol může mít volitelné metody funguje podobně jako účel k rozhraní v jazyce C#, hlavní rozdíl. Volitelné metody nejsou volána, pokud je třída, která přijímá protokol neimplementuje. Jednu třídu v Objective-C navíc můžete implementovat více protokolů, stejně jako třída C# můžete implementovat více rozhraní.

Apple používá k definování kontraktů pro třídy přijmout, zatímco poskytuje abstrakci rychle implementující třídu volající, stejně jako rozhraní jazyka C# proto operační protokoly v rámci iOS. Se používají protokoly i v jiných delegáta scénáře (například s `MKAnnotation` ukazuje následující příklad) a s delegáti (jak je uvedené dál v tomto dokumentu v části delegáti).

 <a name="Protocols_with_Monotouch" />


### <a name="protocols-with-xamarinios"></a>Protokoly s Xamarin.ios

Podívejme se na příklad protokol jazyka Objective-C z Xamarin.iOS. V tomto příkladu použijeme `MKAnnotation` protokol, který je součástí služby `MapKit` framework. `MKAnnotation` je protokol, který umožňuje libovolný objekt, který přijme ji, aby poskytovala informace o anotaci, který jde přidat do mapy. Například k objektu implementace `MKAnnotation` poskytuje umístění anotace a název s ním spojená.

Tímto způsobem `MKAnnotation` protokol slouží k poskytování relevantním údajem, který doprovází poznámky. Skutečné zobrazení pro poznámku samotné vychází z dat v objektu, která přijme `MKAnnotation` protokolu. Například v textu popisku, který se zobrazí, když uživatel klepnutím na poznámku (jak je znázorněno na tomto snímku obrazovky) pochází z `Title` vlastnost ve třídě, která implementuje protokol:

 [![](delegates-protocols-and-events-images/04-annotation-with-callout.png "Příklad text popisku, když uživatel klepnutím na ni")](delegates-protocols-and-events-images/04-annotation-with-callout.png#lightbox)

Jak je popsáno v části Další podrobné informace protokoly, Xamarin.iOS připojí protokoly na abstraktní třídy. Pro `MKAnnotation` protokol, název vázané třída C# `MKAnnotation` tak, aby napodoboval název protokolu ale je podtřídou třídy `NSObject`, kořenová základní třída pro CocoaTouch. Protokol vyžaduje metody getter a setter k implementaci pro souřadnice; Nadpis a podnadpis však jsou volitelné. Proto v `MKAnnotation` třídy, `Coordinate` vlastnost je *abstraktní*, nutnosti jeho implementaci a `Title` a `Subtitle` vlastnosti jsou označeny *virtuální* , přitom volitelné, jak je uvedeno níže:

```csharp
[Register ("MKAnnotation"), Model ]
public abstract class MKAnnotation : NSObject
{
    public abstract CLLocationCoordinate2D Coordinate
    {
        [Export ("coordinate")]
        get;
        [Export ("setCoordinate:")]
        set;
    }

    public virtual string Title
    {
        [Export ("title")]
        get
        {
            throw new ModelNotImplementedException ();
        }
    }

    public virtual string Subtitle
    {
        [Export ("subtitle")]
        get
        {
            throw new ModelNotImplementedException ();
        }
    }
...
}
```

Jakákoli třída může poskytovat data poznámky jednoduše odvozené z `MKAnnotation`, tak dlouho, dokud alespoň `Coordinate` vlastnost je implementována. Například zde je ukázka třídu, která přebírá souřadnice v konstruktoru a vrátí řetězec pro nadpis:

```csharp
/// <summary>
/// Annotation class that subclasses MKAnnotation abstract class
/// MKAnnotation is bound by Xamarin.iOS to the MKAnnotation protocol
/// </summary>
public class SampleMapAnnotation : MKAnnotation
{
    string _title;

    public SampleMapAnnotation (CLLocationCoordinate2D coordinate)
    {
        Coordinate = coordinate;
        _title = "Sample";
    }

    public override CLLocationCoordinate2D Coordinate { get; set; }

    public override string Title {
        get {
            return _title;
        }
    }
}
```

Přes protokol je vázána na jakékoli třídy, která je podtřídou `MKAnnotation` může poskytnout relevantní data, která budou používána mapy při vytváření zobrazení poznámky. Přidání poznámky do mapy, stačí zavolat `AddAnnotation` metodu `MKMapView` instance, jak je znázorněno v následujícím kódu:

```csharp
//an arbitrary coordinate used for demonstration here
var sampleCoordinate =
new CLLocationCoordinate2D (42.3467512, -71.0969456);

//create an annotation and add it to the map
map.AddAnnotation (new SampleMapAnnotation (sampleCoordinate));
```

Proměnnou mapy je instance `MKMapView`, což je třída, která reprezentuje sám sebe. `MKMapView` Použije `Coordinate` údaje získané z `SampleMapAnnotation` instance na pozici poznámky zobrazit na mapě.

`MKAnnotation` Protokol poskytuje se známou sadou funkcí mezi objekty, které implementaci bez příjemce (map v tomto případě) museli vědět o podrobnosti implementace. To zjednodušuje přidání celou řadu možných poznámky na mapu.

 <a name="Protocols_Deep_Dive" />


### <a name="protocols-deep-dive"></a>Podrobné informace protokoly

Vzhledem k tomu, že rozhraní jazyka C# nepodporuje metody volitelné, Xamarin.iOS mapuje protokoly na abstraktní třídy. Proto přijetí protokolu v Objective-C provádí v Xamarin.iOS odvozování z abstraktní třídy, který je vázaný na protokol a implementace požadované metody. Tyto metody k dispozici jako abstraktní metody ve třídě. Volitelné metody z protokolu bude vázán k virtuální metody třídy jazyka C#.

Zde je část například `UITableViewDataSource` protokol jako vázané v Xamarin.iOS:

```csharp
public abstract class UITableViewDataSource : NSObject
{
    [Export ("tableView:cellForRowAtIndexPath:")]
    public abstract UITableViewCell GetCell (UITableView tableView, NSIndexPath indexPath);
    [Export ("numberOfSectionsInTableView:")]
    public virtual int NumberOfSections (UITableView tableView){...}
...
}
```

Všimněte si, že třída je abstraktní. Díky Xamarin.iOS je třída abstraktní pro podporu volitelné nebo požadované metody v protokolech.
Na rozdíl od jazyka Objective-C protokolů (nebo rozhraní jazyka C#), ale třídy jazyka C# nepodporuje vícenásobná dědičnost. Tato akce ovlivní návrh kód C#, která používá protokoly a obvykle vede k vnořené třídy. Více o tomto problému je popsané dál v tomto dokumentu v části delegáti.

 `GetCell(…)` je abstraktní metodu vázána na jazyka Objective-C *selektor*, `tableView:cellForRowAtIndexPath:`, což je požadovaná metoda `UITableViewDataSource` protokolu. Selektor je termín jazyka Objective-C pro název metody. Pokud chcete vynutit metodu podle potřeby, deklaruje Xamarin.iOS ji jako abstraktní. Jiné metody `NumberOfSections(…)`, je vázán k `numberOfSectionsInTableview:`. Tato metoda je v protokolu je volitelné, takže Xamarin.iOS deklaruje jej jako virtuální, takže je volitelné k přepsání v jazyce C#.

Xamarin.iOS postará všechny vazby iOS za vás. Ale pokud byste někdy potřebovali pro vazbu protokolu z jazyka Objective-C ručně, můžete tak učinit pomocí architekturu třídu s `ExportAttribute`. Toto je stejnou metodu používá Xamarin.iOS sám sebe.

Další informace o tom, jak vytvořit vazbu typy jazyka Objective-C v Xamarin.iOS, najdete v článku [vazby typy jazyka Objective-C](~/ios/platform/binding-objective-c/index.md).

Jsme ještě nejsou prostřednictvím s protokoly, ale. Se rovněž používají v iOS jako základ pro delegáti jazyka Objective-C, které je v tématu v další části.

 <a name="Delegates" />


## <a name="delegates"></a>Delegáty

iOS používá jazyka Objective-C delegátů k implementaci delegování vzor, ve kterém jeden objekt předá pracovní do jiného. Objekt provádění práce je delegát je první objekt. Objekt informuje jeho delegáta pro práci odesláním hodnoty hash že zpráv, po určité umět poradit. Odesílání zprávy jako to v Objective C je funkčně srovnatelný volání metody v jazyce C#. Delegát implementuje metody v reakci na těchto volání a tak poskytuje funkce, které aplikace.

Delegáti umožňují rozšířit chování třídy bez nutnosti vytvoření podtřídy. Aplikace v iOS často používají delegáti, když jedna třída volá zpět do jiného potom, co dojde důležité akce. Například `MKMapView` třídy volání zpět na jeho delegáta, když uživatel klepnutím poznámky na mapě, udělíte Autor delegát třídy příležitost se v aplikaci. Můžete pracovat prostřednictvím příklad tohoto typu použití delegáta později v tomto článku, například pomocí delegáta s Xamarin.iOS.

V tuto chvíli asi vás zajímá, jak třídu Určuje, jaké metody volat jeho delegáta. Toto je jiné místo, kdy používáte protokoly. Dostupné metody delegáta obvykle pochází z protokolů, které přijmou.

 <a name="How_Protocols_are_used_with_Delegates" />


### <a name="how-protocols-are-used-with-delegates"></a>Jak se používají protokoly s delegáti

Jsme viděli dříve jak se používají protokoly pro podporu přidání poznámky do mapy.
Protokoly se taky používají k poskytují známé sadu metody pro třídy, které má být volána po určité události vzniknout, například jako po odposlouchávání uživatele poznámky na mapě nebo vybere buňku v tabulce. Třídy, které implementují tyto metody jsou označovány jako delegáti tříd, které je volání.

Třídy, které podporují delegování tomu vystavení delegáta vlastnost, ke kterému je přiřazena třídu implementace delegát. Metody, které můžete implementovat pro delegáta bude záviset na protokol, který přijme konkrétního delegáta. Pro `UITableView` implementovat metodu, `UITableViewDelegate` protokol, pro `UIAccelerometer` metody by implementovat `UIAccelerometerDelegate`, a tak dále, pro jiné třídy v rámci iOS, pro který by chcete vystavit delegáta.

`MKMapView` Jsme viděli v našem příkladu starší třída také obsahuje vlastnost s názvem delegáta, který bude volat po různé události. Delegát pro `MKMapView` je typu `MKMapViewDelegate`.
Budete používat to za chvíli v příklad reagovat na anotace po vybrání, ale první budeme zabývat rozdíl mezi silné a slabé delegáti.

 <a name="Strong_Delegates_vs._Weak_Delegates" />


### <a name="strong-delegates-vs-weak-delegates"></a>Delegáti silným vs. Slabé delegáti

Delegáti, které jsme jste již hledali na, pokud jsou silné delegáti, což znamená, že jsou silného typu. Vazby Xamarin.iOS dodávají spolu s třída silného typu, pro každý protokol delegáta v iOS. IOS má však také koncepci slabé delegáta. Místo vytvoření podtřídy třídu vázaný na protokol jazyka Objective-C u konkrétního delegáta, iOS také umožňuje vybrat pro vazbu protokolu metody ve třídě se vám líbí, která je odvozena z NSObject, architekturu vaší metody s ExportAttribute a potom poskytnutí odpovídající selektorů.
Pokud tuto metodu, přiřadíte instanci třídě vlastnost WeakDelegate místo pro vlastnost delegáta. Slabé delegáta nabízí flexibilitu provést třídě delegáta hierarchii dědičnosti různých níže. Podívejme se na příklad Xamarin.iOS, který používá silné a slabé delegáti.

 <a name="Example_using_a_Delegate_with_Xamarin.iOS" />


### <a name="example-using-a-delegate-with-xamarinios"></a>Příklad použití delegáta s Xamarin.iOS

Ke spouštění kódu v reakci na uživatele klepnutím poznámky v našem příkladu, můžeme podtřídami `MKMapViewDelegate` a přiřaďte instanci do `MKMapView`na `Delegate` vlastnost. `MKMapViewDelegate` Protokol obsahuje jenom volitelné metody.
Proto všechny metody jsou virtuální, je vázána na tento protokol v Xamarin.iOS `MKMapViewDelegate` třídy. Když uživatel vybere poznámky, `MKMapView` instance odešle `mapView:didSelectAnnotationView:` zprávu, která se jeho delegáta. Pro toto zpracování v Xamarin.iOS, musíme přepsat `DidSelectAnnotationView (MKMapView mapView, MKAnnotationView annotationView)` metoda v podtřídy MKMapViewDelegate takto:

```csharp
public class SampleMapDelegate : MKMapViewDelegate
{
    public override void DidSelectAnnotationView (
        MKMapView mapView, MKAnnotationView annotationView)
    {
        var sampleAnnotation =
            annotationView.Annotation as SampleMapAnnotation;

        if (sampleAnnotation != null) {

            //demo accessing the coordinate of the selected annotation to
            //zoom in on it
            mapView.Region = MKCoordinateRegion.FromDistance(
                sampleAnnotation.Coordinate, 500, 500);

            //demo accessing the title of the selected annotation
            Console.WriteLine ("{0} was tapped", sampleAnnotation.Title);
        }
    }
}
```

Třída SampleMapDelegate uvedené výše je implementovaný jako vnořené třídy v kontroleru, který obsahuje `MKMapView` instance. V Objective-C zobrazí se často řadičem přijímat více protokoly přímo v rámci třídy. Ale vzhledem k tomu, že protokoly jsou vázaná na třídy v Xamarin.iOS třídy, které implementují silného typu delegáti mají obvykle jako vnořené třídy.

S implementací třídy delegáta na místě, potřebujete jenom vytvoří instanci delegáta v kontroleru a přiřadit ji ke `MKMapView`na `Delegate` vlastnost, jak je vidět tady:

```csharp
public partial class Protocols_Delegates_EventsViewController : UIViewController
{
    SampleMapDelegate _mapDelegate;
    ...
    public override void ViewDidLoad ()
    {
        base.ViewDidLoad ();

        //set the map's delegate
        _mapDelegate = new SampleMapDelegate ();
        map.Delegate = _mapDelegate;
        ...
    }
    class SampleMapDelegate : MKMapViewDelegate
    {
        ...
    }
}
```

Chcete-li totéž provést pomocí slabé delegáta, budete muset vazby metodu do třídy, která je odvozena z `NSObject` a přiřadit ji ke `WeakDelegate` vlastnost `MKMapView`. Vzhledem k tomu, `UIViewController` třída nakonec odvozena z `NSObject` (např. Každá třída jazyka Objective-C v CocoaTouch), můžeme jednoduše implementovat metodu vázána na `mapView:didSelectAnnotationView:` přímo v kontroleru a přiřaďte tento řadič na `MKMapView`na `WeakDelegate`, aniž by potřeboval velmi vnořené třídy. Následující kód ukazuje tento přístup:

```csharp
public partial class Protocols_Delegates_EventsViewController : UIViewController
{
    ...
    public override void ViewDidLoad ()
    {
        base.ViewDidLoad ();
        //assign the controller directly to the weak delegate
        map.WeakDelegate = this;
    }
    //bind to the Objective-C selector mapView:didSelectAnnotationView:
    [Export("mapView:didSelectAnnotationView:")]
    public void DidSelectAnnotationView (MKMapView mapView,
        MKAnnotationView annotationView)
    {
        ...
    }
}
```

Když spustíte tento kód, aplikace se chová stejně jako při spuštění verzi silného typu delegáta. Výhodou z tohoto kódu je, že slabé delegát nevyžaduje vytvoření další třídy, která byla vytvořena, když jsme použili silného typu delegáta. Ale jde to za cenu zabezpečení typů. Pokud jste udělali v selektoru, který byl předán `ExportAttribute`, nebude zjistit dokud modulu runtime.

 <a name="Events_and_Delegates" />


### <a name="events-and-delegates"></a>Události a delegáti

Delegáti se používají pro zpětná volání v iOS podobně způsob .NET používá události. Chcete-li iOS rozhraní API a způsob jejich používání delegátů jazyka Objective-C zdá se, že více jako .NET, Xamarin.iOS zpřístupní události .NET na mnoha místech použití delegátů v iOS.

Například starší implementace kde `MKMapViewDelegate` odpověděl vybrané poznámky lze také implementovat v Xamarin.iOS pomocí rozhraní .NET událostí. V takovém případě události by být definována v `MKMapView` s názvem `DidSelectAnnotationView`. Měl by mít `EventArgs` podtřídou třídy typu `MKMapViewAnnotationEventsArgs`. `View` Vlastnost `MKMapViewAnnotationEventsArgs` , získáte odkaz na zobrazení poznámky, ze kterého může pokračovat v stejnou implementaci vám, že dříve, jako ilustrované tady:

```csharp
map.DidSelectAnnotationView += (s,e) => {
    var sampleAnnotation = e.View.Annotation as SampleMapAnnotation;
    if (sampleAnnotation != null) {
        //demo accessing the coordinate of the selected annotation to
        //zoom in on it
        mapView.Region = MKCoordinateRegion.FromDistance (
            sampleAnnotation.Coordinate, 500, 500);

        //demo accessing the title of the selected annotation
        Console.WriteLine ("{0} was tapped", sampleAnnotation.Title);
    }
};
```

 <a name="Summary" />


## <a name="summary"></a>Souhrn

Tento článek zahrnutých použití události, protokoly a delegáti v Xamarin.iOS. Jsme viděli, jak Xamarin.iOS zpřístupní normální .NET styl události pro ovládací prvky.
Jsme získali další informace o protokoly jazyka Objective-C, včetně jak se liší od rozhraní jazyka C# a jak je používá Xamarin.iOS. Nakonec jsme se zaměřili delegáti jazyka Objective-C z hlediska Xamarin.iOS. Jsme viděli, jak Xamarin.iOS podporuje obě důrazně a slabě typovaná Delegáti a události .NET delegovat metody navázat.


## <a name="related-links"></a>Související odkazy

- [Protokoly, delegáti a události (ukázka)](https://developer.xamarin.com/samples/Protocols_Delegates_Events/)
- [Hello, iOS](~/ios/get-started/hello-ios/index.md)
- [Vazby typů jazyka Objective-C](~/ios/platform/binding-objective-c/index.md)
- [Programovacího jazyka Objective-C](http://developer.apple.com/library/ios/#documentation/Cocoa/Conceptual/ObjectiveC/Introduction/introObjectiveC.html)
- [Návrh uživatelského rozhraní v Xcode 4](http://developer.apple.com/library/ios/#documentation/IDEs/Conceptual/Xcode4TransitionGuide/InterfaceBuilder/InterfaceBuilder.html)
- [Základní možnosti aplikací pro iOS](http://developer.apple.com/library/ios/#DOCUMENTATION/General/Conceptual/Devpedia-CocoaApp/TargetAction.html)
