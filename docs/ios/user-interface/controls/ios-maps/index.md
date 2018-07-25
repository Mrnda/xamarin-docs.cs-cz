---
title: Mapy v Xamarin.iosu
description: Tento dokument popisuje Mapkitu rámce iOS a jak se používá s Xamarin.iOS. Popisuje, jak přidat mapu, styl, posouvání a přiblížení, pracovat s umístění uživatele, přidávat poznámky, práci s popisky a Překryvné prvky a místní hledání.
ms.prod: xamarin
ms.assetid: 5DD8E56D-51C1-4AFA-B387-79B5734698ED
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/21/2017
ms.openlocfilehash: 5343f53b77319b08424263103834ffcf10e261a0
ms.sourcegitcommit: b56b3f906d2c05a3f1be219ef41be8b79e519b8e
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/25/2018
ms.locfileid: "39242063"
---
# <a name="maps-in-xamarinios"></a>Mapy v Xamarin.iosu

Mapování jsou běžnou funkcí v všechny moderní mobilní operační systémy. iOS nabízí podporu mapování nativně prostřednictvím rozhraní mapování Kit. Pomocí mapy Kit aplikace snadno přidat bohatých, interaktivních mapy. Tyto mapy se dají přizpůsobit mnoha různými způsoby, například přidávání poznámek k označení míst na mapě a Překryvné grafiky libovolného tvarů. Mapa Kit i obsahuje integrovanou podporu pro zobrazení aktuální polohu zařízení.

## <a name="adding-a-map"></a>Přidáním mapy

Přidáním mapy do aplikace lze provést přidáním `MKMapView` instance do zobrazení hierarchie, jak je znázorněno níže:

```csharp
// map is an MKMapView declared as a class variable
map = new MKMapView (UIScreen.MainScreen.Bounds);
View = map;
```

 `MKMapView` je `UIView` podtřídy zobrazující mapu. Pouhým přidáním mapy pomocí výše uvedený kód vytvoří interaktivní mapu:

 ![](images/00-map.png "Ukázková mapa")

## <a name="map-style"></a>Styl mapy

 `MKMapView` podporuje 3 různé styly mapy. Mapa stylu, stačí nastavit `MapType` vlastnost na hodnotu z `MKMapType` výčtu:
 ```
map.MapType = MKMapType.Standard; //road map
map.MapType = MKMapType.Satellite;
map.MapType = MKMapType.Hybrid;
 ```
  Na následujícím snímku obrazovky zobrazit jiné mapování styly, které jsou k dispozici:

 ![](images/01-mapstyles.png "Tento snímek obrazovky zobrazit jiné mapování styly, které jsou k dispozici")

## <a name="panning-and-zooming"></a>Posouvání a změna měřítka zobrazení

 `MKMapView` zahrnuje podporu pro mapování interaktivní funkce jako například:

-  Změna měřítka zobrazení pomocí gesta stažení
-  Posouvání pomocí gest pan


Tyto funkce můžete povolit nebo zakázat jednoduše nastavením `ZoomEnabled` a `ScrollEnabled` vlastnosti `MKMapView` instance, kde výchozí hodnota je true pro obojí. Například pokud chcete zobrazit statickou mapou, stačí nastavte příslušné vlastnosti na hodnotu false:

```csharp
map.ZoomEnabled = false;
map.ScrollEnabled = false;
```

## <a name="user-location"></a>Umístění uživatele

Kromě interakci s uživatelem `MKMapView` také obsahuje integrovanou podporu pro zobrazení umístění zařízení. Používá *Core umístění* rozhraní framework. Pro přístup k umístění uživatele, musí výzvu. K tomuto účelu vytvořte instanci `CLLocationManager` a volat `RequestWhenInUseAuthorization`.

```csharp
CLLocationManager locationManager = new CLLocationManager();
locationManager.RequestWhenInUseAuthorization();
//locationManager.RequestAlwaysAuthorization(); //requests permission for access to location data while running in the background
```

Všimněte si, že se v verze iOS starší než 8.0, pokusu o volání `RequestWhenInUseAuthorization` dojde chybě. Ujistěte se, že chcete zkontrolovat verzi iOS před provedením tohoto volání, pokud máte v úmyslu podporu verze starší než 8.

Přístup k umístění uživatele také vyžaduje změny **Info.plist**. Nastavte následující klíče týkající se dat o poloze:

- **NSLocationWhenInUseUsageDescription** – když přistupujete podle umístění uživatele, když jsou interakci s vaší aplikací.
- **NSLocationAlwaysUsageDescription** – když vaše aplikace nemá přístup k umístění uživatele na pozadí.

Tyto klíče můžete přidat tak, že otevřete **Info.plist** a vyberete *zdroj* v dolní části editoru.

Jakmile jste aktualizovali **Info.plist** a výzva uživateli pro oprávnění pro přístup k jejich umístění, podle umístění uživatele na mapě můžete zobrazit tak, že nastavíte `ShowsUserLocation` vlastnost na hodnotu true:

```csharp
map.ShowsUserLocation = true;
```

 ![](images/02-location-alert.png "Výstraha přístup povolit umístění")
 
## <a name="annotations"></a>Poznámky

 `MKMapView` podporuje také zobrazování obrázků, označované jako poznámky na mapě. Mohou to být buď vlastní Image, nebo systémem definované PIN kódy různých barev. Například následující snímek obrazovky ukazuje mapy s oba pin a vlastní image:

 ![](images/03-annotations.png "Tento snímek obrazovky ukazuje mapy s oba pin a vlastní image")

### <a name="adding-an-annotation"></a>Přidání komentáře

Poznámka samotný má dvě části:

-  `MKAnnotation` Objektu, který zahrnuje data model o poznámky, jako jsou název a umístění anotace.
-  `MKAnnotationView` , Který obsahuje bitovou kopii k zobrazení a volitelně popisek, který se zobrazí, když uživatel klepne na poznámku.


Mapa Kit používá model delegování iOS přidání poznámek k mapě, kde `Delegate` vlastnost `MKMapView` nastavena na instanci `MKMapViewDelegate`. Je implementace tento delegát, který je zodpovědný za vrácení `MKAnnotationView` pro poznámku.

Přidat poznámku, nejprve je poznámka přidána voláním `AddAnnotations` na `MKMapView` instance:

```csharp
// add an annotation
map.AddAnnotations (new MKPointAnnotation (){
    Title="MyAnnotation",
    Coordinate = new CLLocationCoordinate2D (42.364260, -71.120824)
});
```

Při umístění na poznámku se zobrazí na mapě, `MKMapView` bude volat jeho delegáta `GetViewForAnnotation` metodu k získání `MKAnnotationView` k zobrazení.

Například následující kód vrátí poskytovaných systémem `MKPinAnnotationView`:

```csharp
string pId = "PinAnnotation";

public override MKAnnotationView GetViewForAnnotation (MKMapView mapView, NSObject annotation)
{
    if (annotation is MKUserLocation)
        return null;

    // create pin annotation view
    MKAnnotationView pinView = (MKPinAnnotationView)mapView.DequeueReusableAnnotation (pId);

    if (pinView == null)
        pinView = new MKPinAnnotationView (annotation, pId);

    ((MKPinAnnotationView)pinView).PinColor = MKPinAnnotationColor.Red;
    pinView.CanShowCallout = true;

    return pinView;
}
```

### <a name="reusing-annotations"></a>Opětovné použití poznámek

Pro konzervaci paměti, `MKMapView` umožňuje zobrazení anotace uživatele do fondu pro opakované použití, podobným způsobem, jakým jsou opakovaně buňkami v tabulce. Získání zobrazení o poznámky z fondu se provádí pomocí volání `DequeueReusableAnnotation`:

```csharp
MKAnnotationView pinView = (MKPinAnnotationView)mapView.DequeueReusableAnnotation (pId);
```

#### <a name="showing-callouts"></a>Zobrazuje popisky

Jak už bylo zmíněno dříve, poznámky lze volitelně zobrazit popisek. Chcete-li zobrazit popisek jednoduše nastavte `CanShowCallout` na true `MKAnnotationView`. Výsledkem je název poznámky se zobrazí, když je poznámka klepnutí, jak je znázorněno:

 ![](images/04-callout.png "Název poznámky se zobrazí")

### <a name="customizing-the-callout"></a>Přizpůsobení popisek

Popisek se taky dají upravit pro zobrazení vlevo a vpravo příslušenství, jak je znázorněno níže:

```csharp
pinView.RightCalloutAccessoryView = UIButton.FromType (UIButtonType.DetailDisclosure);
pinView.LeftCalloutAccessoryView = new UIImageView(UIImage.FromFile ("monkey.png"));
```

Tento kód vrátí následující popisku:

 ![](images/05-callout-accessories.png "Popisek v podobě příklad")

Budou zpracovávat klepnutím na pravém příslušenství, stačí pouze implementovat `CalloutAccessoryControlTapped` metodu `MKMapViewDelegate`:

```csharp
public override void CalloutAccessoryControlTapped (MKMapView mapView, MKAnnotationView view, UIControl control)
{
    ...
}
```

### <a name="overlays"></a>Překrytí

Dalším způsobem, jak vrstva grafiky na mapě používá překrytí. Překryvy podporují výkresu grafického obsahu, která se škáluje s mapou, jak je zvětšeno. iOS poskytuje podporu pro několik typů překrytí, včetně:

-  Mnohoúhelníky - běžně používají, abyste měli na očích některé oblasti na mapě.
-  Lomené čáry – často viděli při zobrazování trasu.
-  Kroužky – zvýraznění kruhové oblasti mapě.


Navíc lze vytvořit vlastní překrytí zobrazíte libovolného geometrie pomocí podrobné, vlastní kód pro vykreslování. Například by této možnosti taky přemýšlíte o počasí vhodným kandidátem pro vlastní překrytí.

#### <a name="adding-an-overlay"></a>Přidání překrytí

Podobně jako u poznámky, přidání překrytí zahrnuje 2 částí:

-  Vytvoření modelu objektu pro překrytí a jejím přidáním na `MKMapView` .
-  Vytvoření zobrazení pro překrytí v `MKMapViewDelegate` .


Model pro překrytí může být kterýkoli `MKShape` podtřídy. Zahrnuje Xamarin.iOS `MKShape` podtřídy pro mnohoúhelníků a čar kruhy, prostřednictvím `MKPolygon`, `MKPolyline` a `MKCircle` třídy v uvedeném pořadí.

Například následující kód slouží k přidání `MKCircle`:

```csharp
var circleOverlay = MKCircle.Circle (mapCenter, 1000);
map.AddOverlay (circleOverlay);
```

Zobrazení pro překrytí `MKOverlayView` instanci, která je vrácena `GetViewForOverlay` v `MKMapViewDelegate`. Každý `MKShape` má odpovídající `MKOverlayView` , který umí zobrazit daného tvaru. Pro `MKPolygon` je `MKPolygonView`. Obdobně `MKPolyline` odpovídá `MKPolylineView`a pro `MKCircle` je `MKCircleView`.

Například následující kód vrátí `MKCircleView` pro `MKCircle`:

```csharp
public override MKOverlayView GetViewForOverlay (MKMapView mapView, NSObject overlay)
{
    var circleOverlay = overlay as MKCircle;
    var circleView = new MKCircleView (circleOverlay);
    circleView.FillColor = UIColor.Blue;
    return circleView;
}
```

To bude vypadat kruhu na mapě jako:

 ![](images/06-circle-overlay.png "Kruh zobrazená na mapě")

## <a name="local-search"></a>Místní hledání

iOS obsahuje místní hledání rozhraní API pomocí mapy Kit, která umožňuje asynchronní hledání bodů zájmu v určité zeměpisné oblasti.

K provedení místní hledání, musí aplikace postupujte takto:

1.  Vytvoření `MKLocalSearchRequest` objektu.
1.  Vytvoření `MKLocalSearch` objektu z `MKLocalSearchRequest` .
1.  Volání `Start` metodu `MKLocalSearch` objektu.
1.  Načíst `MKLocalSearchResponse` objektu ve zpětném volání.


Místní hledání samotné rozhraní API poskytuje žádné uživatelské rozhraní. Dokonce i nevyžaduje mapu, která bude používat. Aby praktické použití místní hledání, ale aplikace potřebuje poskytnout způsob, jak zadat dotaz vyhledávání a zobrazení výsledků. Navíc protože výsledky budou obsahovat data o poloze, to se často smysl zobrazit na mapě.

<a name="Adding_a_Local_Search_UI"/>

### <a name="adding-a-local-search-ui"></a>Přidání místní vyhledávání uživatelského rozhraní

Jedním ze způsobů tak, aby přijímal vstup vyhledávání se `UISearchBar`, které poskytuje `UISearchController` a zobrazí výsledky v tabulce.

Následující kód přidává `UISearchController` (který má vlastnost panelu hledání) v `ViewDidLoad` metoda `MapViewController`:

```csharp
//Creates an instance of a custom View Controller that holds the results
var searchResultsController = new SearchResultsViewController (map);

//Creates a search controller updater
var searchUpdater = new SearchResultsUpdator ();
searchUpdater.UpdateSearchResults += searchResultsController.Search;
            
//add the search controller
searchController = new UISearchController (searchResultsController) {
                SearchResultsUpdater = searchUpdater
            };

//format the search bar
searchController.SearchBar.SizeToFit ();
searchController.SearchBar.SearchBarStyle = UISearchBarStyle.Minimal;
searchController.SearchBar.Placeholder = "Enter a search query";

//the search bar is contained in the navigation bar, so it should be visible
searchController.HidesNavigationBarDuringPresentation = false;

//Ensure the searchResultsController is presented in the current View Controller 
DefinesPresentationContext = true;

//Set the search bar in the navigation bar
NavigationItem.TitleView = searchController.SearchBar;

```csharp
Note that you are responsible for incorporating the search bar object into the user interface. In this example, we assigned it to the TitleView of the navigation bar, but if you do not use a navigation controller in your application you will have to find another place to display it.

In this code snippet, we created another custom view controller – `searchResultsController` –  that displays the search results and then we used this object to create our search controller object. We also created a new search updater, which becomes active when the user interacts with the search bar. It receives notifications about searches with each keystroke and is responsible for updating the UI.
We will take a look at how to implement both the `searchResultsController` and the `searchResultsUpdater` later in this guide.

This results in a search bar displayed over the map as shown below:

 ![](images/07-searchbar.png "A search bar displayed over the map")
 


### Displaying the Search Results

To display search results, we need to create a custom View Controller; normally a `UITableViewController`. As shown above, the `searchResultsController` is passed to the constructor of the `searchController` when it is being created.
The following code is an example of how to create this custom View Controller:

```csharp
public class SearchResultsViewController : UITableViewController
{
    static readonly string mapItemCellId = "mapItemCellId";
    MKMapView map;

    public List<MKMapItem> MapItems { get; set; }

    public SearchResultsViewController (MKMapView map)
    {
        this.map = map;

        MapItems = new List<MKMapItem> ();
    }

    public override nint RowsInSection (UITableView tableView, nint section)
    {
        return MapItems.Count;
    }

    public override UITableViewCell GetCell (UITableView tableView, NSIndexPath indexPath)
    {
        var cell = tableView.DequeueReusableCell (mapItemCellId);

        if (cell == null)
            cell = new UITableViewCell ();

        cell.TextLabel.Text = MapItems [indexPath.Row].Name;
        return cell;
    }

    public override void RowSelected (UITableView tableView, NSIndexPath indexPath)
    {
        // add item to map
        CLLocationCoordinate2D coord = MapItems [indexPath.Row].Placemark.Location.Coordinate;
        map.AddAnnotations (new MKPointAnnotation () {
            Title = MapItems [indexPath.Row].Name,
            Coordinate = coord
        });

        map.SetCenterCoordinate (coord, true);

        DismissViewController (false, null);
    }

    public void Search (string forSearchString)
    {
        // create search request
        var searchRequest = new MKLocalSearchRequest ();
        searchRequest.NaturalLanguageQuery = forSearchString;
        searchRequest.Region = new MKCoordinateRegion (map.UserLocation.Coordinate, new MKCoordinateSpan (0.25, 0.25));

        // perform search
        var localSearch = new MKLocalSearch (searchRequest);

        localSearch.Start (delegate (MKLocalSearchResponse response, NSError error) {
            if (response != null && error == null) {
                this.MapItems = response.MapItems.ToList ();
                this.TableView.ReloadData ();
            } else {
                Console.WriteLine ("local search error: {0}", error);
            }
        });


    }
}
```

### <a name="updating-the-search-results"></a>Výsledky hledání se aktualizují

`SearchResultsUpdater` Funguje jako prostředník mezi `searchController`na panelu hledání a výsledky hledání. 

V tomto příkladu máme nejprve vytvořit metodu vyhledávání v `SearchResultsViewController`. K tomu musíte vytvořit jsme `MKLocalSearch` objektu a použijte ji pro vydání hledání `MKLocalSearchRequest`, se načtou výsledky zpětné volání předané `Start` metodu `MKLocalSearch` objektu. Výsledky jsou vráceny v `MKLocalSearchResponse` objekt, který obsahuje celou řadu `MKMapItem` objekty:

```csharp
public void Search (string forSearchString)
{
    // create search request
    var searchRequest = new MKLocalSearchRequest ();
    searchRequest.NaturalLanguageQuery = forSearchString;
    searchRequest.Region = new MKCoordinateRegion (map.UserLocation.Coordinate, new MKCoordinateSpan (0.25, 0.25));

    // perform search
    var localSearch = new MKLocalSearch (searchRequest);

    localSearch.Start (delegate (MKLocalSearchResponse response, NSError error) {
        if (response != null && error == null) {
            this.MapItems = response.MapItems.ToList ();
            this.TableView.ReloadData ();
        } else {
            Console.WriteLine ("local search error: {0}", error);
        }
    });


}
```

Potom v našich `MapViewController` vytvoříme vlastní implementaci `UISearchResultsUpdating`, přiřazená do `SearchResultsUpdater` vlastnost naše `searchController` v [přidání místního uživatelského rozhraní vyhledávání](#Adding_a_Local_Search_UI) části:

```csharp
public class SearchResultsUpdator : UISearchResultsUpdating
{
    public event Action<string> UpdateSearchResults = delegate {};

    public override void UpdateSearchResultsForSearchController (UISearchController searchController)
    {
        this.UpdateSearchResults (searchController.SearchBar.Text);
    }
}
```

Implementace výše přidá poznámku do mapy při výběru položky z výsledků, jak je znázorněno níže:

 ![](images/08-search-results.png "Poznámka přidána do mapy při výběru položky z výsledků")
 
> [!IMPORTANT]
> `UISearchController` bylo implementováno v iOS 8. Pokud chcete podporovat zařízení starší než toto, pak budete muset použít `UISearchDisplayController`.



## <a name="summary"></a>Souhrn

Tento článek prozkoumat *mapy* *Kit* rozhraní pro iOS. Nejprve hledá na to, jak `MKMapView` třída umožňuje interaktivní mapy mají být zahrnuty v aplikaci. Potom se ukázal, jak lze dále přizpůsobit pomocí poznámky a Překryvné prvky mapy. Nakonec prověřit místní vyhledávací funkce, které byly přidány k sadě mapy se systémem iOS 6.1, ukazuje, jak používat provádět dotazy na základě umístění pro body zájmu a přidat je do mapy.

## <a name="related-links"></a>Související odkazy

- [SearchController](https://github.com/xamarin/recipes/tree/master/Recipes/ios/content_controls/search-controller)
- [MapDemo (ukázka)](https://developer.xamarin.com/samples/monotouch/MapDemo)
