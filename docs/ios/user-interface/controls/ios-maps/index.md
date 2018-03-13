---
title: Mapy
description: "iOS zahrnuje MapKit mapuje framework, který umožňuje snadno přidat k aplikaci. Pomocí sady mapy, aplikace pro iOS můžete přidat interaktivní mapy, které podporují funkce, jako je posouvání a přibližování, zobrazuje umístění uživatele a bohaté grafické rozvrstvení na mapě. Tento článek obsahuje podrobné několik součástí mapy Kit znázorňující mohli využít k vytváření funkcí geografické funkce do aplikace"
ms.topic: article
ms.prod: xamarin
ms.assetid: 5DD8E56D-51C1-4AFA-B387-79B5734698ED
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/21/2017
ms.openlocfilehash: 540a459be24296c8446c2136773ddde59f9d4dd7
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 03/09/2018
---
# <a name="maps"></a>Mapy

_iOS zahrnuje MapKit mapuje framework, který umožňuje snadno přidat k aplikaci. Pomocí sady mapy, aplikace pro iOS můžete přidat interaktivní mapy, které podporují funkce, jako je posouvání a přibližování, zobrazuje umístění uživatele a bohaté grafické rozvrstvení na mapě. Tento článek obsahuje podrobné několik součástí mapy Kit znázorňující mohli využít k vytváření funkcí geografické funkce do aplikace_

Mapování jsou běžnou funkcí ve všech operačních systémech mobilních moderní. iOS nabízí podporu mapování nativně prostřednictvím rozhraní Kit mapy. Pomocí mapy Kit aplikace snadno přidat bohatý a interaktivní mapy. Tyto mapy lze přizpůsobit různými způsoby, například přidání poznámek k označení umístění na mapě a Překrytí grafiky libovolný tvarů. Mapa Kit i má integrovanou podporu pro zobrazení aktuálního umístění zařízení.

## <a name="adding-a-map"></a>Přidáním mapy

Přidáním mapy do aplikace je možné udělat přidáním `MKMapView` instanci zobrazení hierarchie, jak je uvedeno níže:

```csharp
// map is an MKMapView declared as a class variable
map = new MKMapView (UIScreen.MainScreen.Bounds);
View = map;
```

 `MKMapView` je `UIView` podtřídami, který zobrazí mapu. Jednoduše přidáním mapy pomocí výše uvedený kód vytvoří interaktivní mapu:

 ![](images/00-map.png "Ukázkové mapování")

## <a name="map-style"></a>Map – styl

 `MKMapView` podporuje 3 různé styly služby maps. Chcete-li použít styl mapy, jednoduše nastavte `MapType` vlastnost na hodnotu z `MKMapType` výčtu:
 ```
map.MapType = MKMapType.Standard; //road map
map.MapType = MKMapType.Satellite;
map.MapType = MKMapType.Hybrid;
 ```
  Následující snímek obrazovky zobrazit styly jiné mapování, které jsou k dispozici:

 ![](images/01-mapstyles.png "Tento snímek obrazovky zobrazit styly jiné mapování, které jsou k dispozici")

## <a name="panning-and-zooming"></a>Posouvání a přibližování

 `MKMapView` obsahuje podporu pro funkce interaktivity mapy jako například:

-  Přiblížení a oddálení prostřednictvím gesto roztahováním
-  Posouvání prostřednictvím gesto pan


Tyto funkce můžete povolit nebo zakázat jednoduše nastavením `ZoomEnabled` a `ScrollEnabled` vlastnosti `MKMapView` instance, kde je výchozí hodnota pro obě na hodnotu true. Například pokud chcete zobrazit statickou mapou, jednoduše nastavte příslušné vlastnosti na hodnotu false:

```csharp
map.ZoomEnabled = false;
map.ScrollEnabled = false;
```

## <a name="user-location"></a>Umístění uživatele

Kromě interakci s uživatelem `MKMapView` také obsahuje integrovanou podporu pro zobrazení umístění zařízení. Dělá to pomocí *základní umístění* framework. Než se dostanete k umístění uživatele, musíte požádat uživatele. Chcete-li to provést, vytvořte instanci `CLLocationManager` a volání `RequestWhenInUseAuthorization`.

```csharp
CLLocationManager locationManager = new CLLocationManager();
locationManager.RequestWhenInUseAuthorization();
//locationManager.RequestAlwaysAuthorization(); //requests permission for access to location data while running in the background
```

Všimněte si, že se ve verzích systému iOS před 8.0, pokus o volání `RequestWhenInUseAuthorization` bude mít za následek chybu. Ujistěte se, že kontrola verze iOS před provedením tohoto volání, pokud máte v úmyslu podporovat verze starší než 8.

Přístup k umístění uživatele také vyžaduje změny **Info.plist**. Musí být nastavená týkající se data o umístění následující klíče:

- **NSLocationWhenInUseUsageDescription** – když přistupujete umístění uživatele, když jsou interakci s vaší aplikací.
- **NSLocationAlwaysUsageDescription** – když vaše aplikace přístup k umístění uživatele na pozadí.

Tyto klíče můžete přidat tak, že otevřete **Info.plist** a výběrem *zdroj* v dolní části editoru.

Jakmile aktualizujete **Info.plist** a výzva uživateli pro oprávnění k přístupu k jejich umístění, umístění uživatele můžete zobrazit na mapě nastavením `ShowsUserLocation` vlastnost na hodnotu true:

```csharp
map.ShowsUserLocation = true;
```

 ![](images/02-location-alert.png "Povolit výstrahy přístup k umístění")
 
## <a name="annotations"></a>Poznámky

 `MKMapView` podporuje také zobrazování obrázků, označuje jako poznámky na mapě. To mohou být buď vlastní Image nebo definovaná systémem PIN různých barev. Například následující snímek obrazovky ukazuje mapování se i kód pin a vlastní image:

 ![](images/03-annotations.png "Tento snímek obrazovky ukazuje mapování se i kód pin a vlastní image")

### <a name="adding-an-annotation"></a>Přidání poznámky

Poznámky samotné má dvě části:

-  `MKAnnotation` Objekt, který obsahuje data modelu o poznámky, jako je název a umístění anotace.
-  `MKAnnotationView` , Který obsahuje bitovou kopii k zobrazení a volitelně popisků, které se zobrazí, když uživatel klepnutím anotace.


Mapy Kit používá vzor delegování iOS k přidání poznámky do mapy, kde `Delegate` vlastnost `MKMapView` je nastaven na instanci `MKMapViewDelegate`. Je implementace tohoto delegáta, která je zodpovědná za vrácení `MKAnnotationView` pro poznámky.

Pokud chcete přidat poznámky, nejprve je poznámka přidána voláním `AddAnnotations` na `MKMapView` instance:

```csharp
// add an annotation
map.AddAnnotations (new MKPointAnnotation (){
    Title="MyAnnotation",
    Coordinate = new CLLocationCoordinate2D (42.364260, -71.120824)
});
```

Při umístění Poznámka se zobrazí na mapě, `MKMapView` bude volat jeho delegáta `GetViewForAnnotation` metoda získat `MKAnnotationView` k zobrazení.

Například následující kód vrátí poskytované systémem `MKPinAnnotationView`:

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

Pro konzervaci paměti, `MKMapView` umožňuje anotaci zobrazení je k být ve fondu pro opakované použití, podobně jako jsou opakovaně buněk tabulky. Získání zobrazení poznámky z fondu se provádí pomocí volání `DequeueReusableAnnotation`:

```csharp
MKAnnotationView pinView = (MKPinAnnotationView)mapView.DequeueReusableAnnotation (pId);
```

#### <a name="showing-callouts"></a>Zobrazuje popisky

Jak už bylo zmíněno dříve, můžete volitelně poznámky zobrazit popisku. Chcete-li zobrazit popisek jednoduše nastavte `CanShowCallout` na hodnotu true na `MKAnnotationView`. Výsledkem je název poznámky se zobrazí, když je stisknuté anotace, jak je znázorněno:

 ![](images/04-callout.png "Název poznámky se zobrazuje")

### <a name="customizing-the-callout"></a>Přizpůsobení popisku

Popisek lze upravit k zobrazení levé a pravé příslušenství zobrazení, jak je uvedeno níže:

```csharp
pinView.RightCalloutAccessoryView = UIButton.FromType (UIButtonType.DetailDisclosure);
pinView.LeftCalloutAccessoryView = new UIImageView(UIImage.FromFile ("monkey.png"));
```

Tento kód vrátí následující popisku:

 ![](images/05-callout-accessories.png "Popisek v podobě příklad")

Pro zpracování uživatel klepnutím správné příslušenství, jednoduše implementovat `CalloutAccessoryControlTapped` metoda v `MKMapViewDelegate`:

```csharp
public override void CalloutAccessoryControlTapped (MKMapView mapView, MKAnnotationView view, UIControl control)
{
    ...
}
```

### <a name="overlays"></a>Překryvy

Překryvy používá jiný způsob, jak grafiky vrstvy na mapě. Překryvy podporovat kreslení grafické obsah, který škáluje s mapy, jako je možnosti. iOS poskytuje podporu pro několik typů překryvy, včetně:

-  Mnohoúhelníky - často používá ke zvýraznění některé oblasti na mapě.
-  Čáru lomených - často vidět až trasu.
-  Kroužky – zvýraznění cyklické oblasti mapy.


Kromě toho lze vytvořit vlastní překryvy zobrazíte libovolný geometrie granulární, přizpůsobené kreslení kódem. Například počasí paprskového by být vhodným kandidátem na vlastní překrytí.

#### <a name="adding-an-overlay"></a>Přidání překrytí

Podobně jako u poznámky, přidání překrytí zahrnuje 2 částí:

-  Vytvoření objektu modelu pro překrytí a jejím přidáním do `MKMapView` .
-  Vytvoření zobrazení pro překrytí v `MKMapViewDelegate` .


Model pro překrytí může být libovolná `MKShape` podtřídy. Zahrnuje Xamarin.iOS `MKShape` podtřídy mnohoúhelníky, čáru lomených a kroužky, prostřednictvím `MKPolygon`, `MKPolyline` a `MKCircle` třídy v uvedeném pořadí.

Například následující kód se používá k přidání `MKCircle`:

```csharp
var circleOverlay = MKCircle.Circle (mapCenter, 1000);
map.AddOverlay (circleOverlay);
```

Zobrazení pro překrytí `MKOverlayView` instance, který je vrácen `GetViewForOverlay` v `MKMapViewDelegate`. Každý `MKShape` má odpovídající `MKOverlayView` který umí zobrazíte daného tvaru. Pro `MKPolygon` je `MKPolygonView`. Podobně `MKPolyline` odpovídá `MKPolylineView`a pro `MKCircle` je `MKCircleView`.

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

Zobrazí kruh na mapě znázorněné:

 ![](images/06-circle-overlay.png "Zobrazit na mapě kruh")

## <a name="local-search"></a>Místní vyhledávání

iOS obsahuje místní vyhledávání rozhraní API pomocí Kit mapu, která umožňuje asynchronní hledání bodů zájmu v zadané geografické oblasti.

Pokud chcete provést místní vyhledávání, musí aplikace postupujte takto:

1.  Vytvoření `MKLocalSearchRequest` objektu.
1.  Vytvoření `MKLocalSearch` objektu z `MKLocalSearchRequest` .
1.  Volání `Start` metodu `MKLocalSearch` objektu.
1.  Načtení `MKLocalSearchResponse` objekt v zpětné volání.


Místní vyhledávání rozhraní API, samotné poskytuje žádné uživatelské rozhraní. Nevyžaduje i mapu, která použije. Chcete-li praktická použití místní vyhledávání, však aplikace musí zajistit některé způsob, jak zadejte vyhledávací dotaz a zobrazit výsledky. Navíc vzhledem k tomu, že výsledky budou obsahovat data o umístění, ji budou často smysl je zobrazit na mapě.

<a name="Adding_a_Local_Search_UI"/>

### <a name="adding-a-local-search-ui"></a>Přidání místní vyhledávání uživatelského rozhraní

Jedním ze způsobů tak, aby přijímal vstup vyhledávání je s `UISearchBar`, který poskytl `UISearchController` a zobrazí výsledky v tabulce.

Následující kód přidá `UISearchController` (která má vlastnost panelu Hledat) v `ViewDidLoad` metodu `MapViewController`:

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

### <a name="updating-the-search-results"></a>Aktualizuje výsledky hledání

`SearchResultsUpdater` Funguje jako prostředník mezi `searchController`na panelu Hledat a výsledky hledání. 

V tomto příkladu máme nejprve vytvořit metodu search v `SearchResultsViewController`. K tomu musíte vytvořit jsme `MKLocalSearch` objektu a použijte ji pro vydání vyhledávání pro `MKLocalSearchRequest`, výsledky se načítají při zpětném volání předaný `Start` metodu `MKLocalSearch` objektu. Potom budou vráceny výsledky v `MKLocalSearchResponse` objekt obsahující pole `MKMapItem` objekty:

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

Potom v našich `MapViewController` vytvoříme vlastní implementaci `UISearchResultsUpdating`, kterému je přiřazen k `SearchResultsUpdater` vlastnost naše `searchController` v [přidání místního uživatelského rozhraní vyhledávání](#Adding_a_Local_Search_UI) části:

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

Implementace výše přidá poznámky mapy je vybranou položku z výsledků, jak je uvedeno níže:

 ![](images/08-search-results.png "Poznámky přidat do mapy, když je vybrána položka z výsledků")
 
 > [!IMPORTANT]
> **Poznámka:** `UISearchController` byl implementován v iOS 8. Pokud chcete podporovat zařízení dříve, než to, pak budete muset použít `UISearchDisplayController`.



## <a name="summary"></a>Souhrn

Tento článek zkontrolován *mapy* *Kit* framework pro iOS. Nejprve hledá o tom, jak `MKMapView` třída umožňuje interaktivní mapy mají být zahrnuty v aplikaci. Potom ji ukázal, jak lze přizpůsobit pomocí poznámky a překryvy mapy. Nakonec je zkontrolován možnosti místní vyhledávání, které byly přidány do mapy Kit s iOS 6.1, znázorňující způsob použití provádět dotazy na základě umístění pro vás zajímá a přidat je do mapy.

## <a name="related-links"></a>Související odkazy

- [SearchController](https://developer.xamarin.com/recipes/ios/content_controls/search-controller/)
- [MapDemo (ukázka)](https://developer.xamarin.com/samples/monotouch/MapDemo)
