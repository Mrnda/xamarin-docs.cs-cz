---
title: Práce se zobrazeními kolekce
description: Tento článek se zabývá navrhování a práce se zobrazeními kolekce uvnitř Xamarin.tvOS aplikace.
ms.prod: xamarin
ms.assetid: 5125C4C7-2DDF-4C19-A362-17BB2B079178
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/16/2017
ms.openlocfilehash: 7fa38aa81e5929bdc88ceebd153d86cfcd92f20e
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/04/2018
---
# <a name="working-with-collection-views"></a>Práce se zobrazeními kolekce

_Tento článek se zabývá navrhování a práce se zobrazeními kolekce uvnitř Xamarin.tvOS aplikace._

Zobrazení kolekce povolit pro skupinu obsahu, který se má zobrazit pomocí libovolného rozložení. Pomocí integrovanou podporu, umožňují pro snadné vytváření rozložení mřížky nebo lineární zároveň také podporuje vlastní rozložení.

[![](collection-views-images/collection01.png "Ukázkové zobrazení kolekce")](collection-views-images/collection01.png#lightbox)

Zobrazení kolekce udržuje kolekce položek použití delegáta a zdroj dat k zajištění interakce s uživatelem a obsah, kolekce. Vzhledem k tomu, že zobrazení kolekce je založena na subsystému rozložení, která je nezávislá samotném zobrazení, poskytuje rozložení můžete snadno změnit prezentaci zobrazení kolekce dat na průběžně.

<a name="About-Collection-Views" />

## <a name="about-collection-views"></a>O zobrazení kolekce

Jak jsme uvedli výše, zobrazení kolekce (`UICollectionView`) spravuje uspořádanou kolekci položek a nabídne tyto položky přizpůsobitelné rozložení. Zobrazení kolekce pracovních podobným způsobem k zobrazení tabulek (`UITableView`), s výjimkou používají rozložení k dispozici položky ve více než jen jeden sloupec.

Pokud používáte zobrazení kolekce v tvOS, vaše aplikace zodpovídá za poskytování data přidružená k kolekce s použitím zdroje dat (`UICollectionViewDataSource`). Zobrazení dat kolekce můžete volitelně uspořádány a uvedené do různých skupin (oddíly).

Zobrazení kolekce uvede jednotlivé položky na obrazovce pomocí buňku (`UICollectionViewCell`), který poskytuje prezentaci danou část informace z kolekce (např. obrázku a jeho název).

Volitelně můžete doplňující zobrazení přidat do kolekce zobrazení prezentace tak, aby fungoval jako záhlaví a zápatí pro oddíly a buněk. Zobrazení kolekce rozložení je zodpovědná za definování umístění těchto zobrazení spolu s jednotlivých buněk.

Zobrazení kolekce může reagovat na interakci s uživatelem použití delegáta (`UICollectionViewDelegate`). Tento delegát je také zodpovědná za určení, jestli dané buňky můžete získat fokus, pokud byla zdůraznily buňku, nebo pokud byla vybrána. V některých případech delegát Určuje velikost jednotlivých buněk.

<a name="Collection-View-Layouts" />

## <a name="collection-view-layouts"></a>Rozložení zobrazení kolekce

Klíčovou funkcí zobrazení kolekce je jeho oddělení od data, která je prezentace a jeho rozložení. Rozložení zobrazení kolekce (`UICollectionViewLayout`) zodpovídá za poskytování organizace a umístění buněk (a všechny doplňující zobrazení) se v zobrazení kolekce prezentace na obrazovce.

Jednotlivých buněk jsou vytvořené pomocí zobrazení kolekce z jeho připojené zdroje dat jsou uspořádané a zobrazí rozložení zobrazení dané kolekce.

Rozložení zobrazení kolekce obvykle je k dispozici při vytváření zobrazení kolekce. Ale můžete kdykoli změnit rozložení zobrazení kolekce a na obrazovce prezentace zobrazení kolekce dat se automaticky aktualizují pomocí nového rozložení zadat.

Rozložení zobrazení kolekce poskytuje několik metod, které slouží k animace přechodu mezi dvě různé rozložení (ve výchozím nastavení, které se provádí žádná animace). Kromě toho rozložení zobrazení kolekce můžete pracovat se nástroje pro rozpoznávání gesto pro další animaci interakci s uživatelem, který vede ke změně rozložení.

<a name="Creating-Cells-and-Supplementary-Views" />

## <a name="creating-cells-and-supplementary-views"></a>Probíhá vytváření buněk a doplňující zobrazení

Zdroj dat zobrazení kolekce není pouze data zálohování kolekce položek, ale také zajišťuje buněk, které se používají k zobrazení obsahu.

Protože zobrazení kolekce byly navrženy pro zpracování rozsáhlých kolekcí položek, může být jednotlivých buněk vyjmutou a znovu použít k udržování z překročení omezení paměti. Existují dvě různé metody pro dequeueing zobrazení:

- `DequeueReusableCell` -Vytvoří nebo vrátí buňku daného typu (jak je uvedeno v Storyboard aplikace).
- `DequeueReusableSupplementaryView` -Vytvoří nebo vrátí doplňující zobrazení daného typu (jak je uvedeno v Storyboard aplikace).

Před voláním žádnou z těchto metod, je nutné zaregistrovat třídu, Storyboard nebo `.xib` soubor použitý k vytvoření na buňku zobrazení s zobrazení kolekce. Příklad:

```csharp
public CityCollectionView (IntPtr handle) : base (handle)
{
    // Initialize
    RegisterClassForCell (typeof(CityCollectionViewCell), CityViewDatasource.CardCellId);
    ...
}
```

Kde `typeof(CityCollectionViewCell)` poskytuje třídu, která podporuje zobrazení a `CityViewDatasource.CardCellId` poskytuje ID použité při buňky (nebo zobrazení) je vyřazeno z fronty.

Jakmile je vyjmutou buňky, nakonfigurujete s daty pro položku, kterou je představující a vrátit do zobrazení kolekce pro zobrazení.

<a name="About-Collection-View-Controllers" />

## <a name="about-collection-view-controllers"></a>O řadiče zobrazení kolekce

Řadič zobrazení kolekce (`UICollectionViewController`) je specializovaná View Controller (`UIViewController`), který poskytuje následující chování:

- Zodpovídá za načítání zobrazení kolekce z jeho Storyboard nebo `.xib` souboru a vytváření instancí zobrazení. Pokud je vytvořen v kódu, automaticky vytvoří nové a nenakonfigurovaného zobrazení kolekce.
- Po zobrazení kolekce je načtena, kontroler, pokusí se načíst její zdroj dat a delegáta scénáře nebo `.xib` souboru. Pokud žádný není k dispozici, nastaví se jako zdroj obou.
- Zajišťuje, že data načtení předtím, než se naplní zobrazení kolekce na první zobrazí a znovu načte a zrušte výběr na každém následném zobrazení.

Kromě toho řadiče zobrazení kolekce poskytuje přepisovatelné metody, které lze použít ke správě životního cyklu zobrazení kolekce, jako `AwakeFromNib` a `ViewWillDisplay`.

<a name="Collection-Views-and-Storyboards" />

## <a name="collection-views-and-storyboards"></a>Zobrazení kolekcí a scénářů

Nejjednodušší způsob, jak pracovat s zobrazení kolekce v aplikaci Xamarin.tvOS je přidat do jeho scénáře. Jako rychlou příkladu přidáme k vytvoření ukázkové aplikace, která uvede bitovou kopii, název a vyberte tlačítko. Pokud uživatel kliknutím na tlačítko vybrat, zobrazení kolekce se zobrazí umožňující uživateli vybrat novou bitovou kopii. Při výběru image zobrazení kolekce se zavře a zobrazí se nová bitová kopie a název.

Pojďme postupujte takto:

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

    
1. Spusťte novou **tvOS jediné zobrazení aplikace** v sadě Visual Studio for Mac.
1. V **Průzkumníku řešení**, dvakrát klikněte `Main.storyboard` souboru a otevře ji v iOS Designer.
1. Přidání zobrazení bitové kopie, štítek a tlačítko pro existující zobrazit a nakonfigurovat je pro vypadat následovně: 

    [![](collection-views-images/collection02.png "Ukázka rozložení")](collection-views-images/collection02.png#lightbox)
1. Přiřazení **název** k zobrazení bitové kopie a název v **pomůcky karta** z **Explorer vlastnosti**. Příklad: 

    [![](collection-views-images/collection03.png "Název nastavení")](collection-views-images/collection03.png#lightbox)
1. V dalším kroku přetažením řadič zobrazení kolekce na scénáři: 

    [![](collection-views-images/collection04.png "Řadič zobrazení kolekce")](collection-views-images/collection04.png#lightbox)
1. Ovládací prvek přetažení z tlačítka řadiče zobrazení kolekcí a vyberte **Push** z místní nabídce: 

    [![](collection-views-images/collection05.png "Vyberte nabízené automaticky otevřeném okně.")](collection-views-images/collection05.png#lightbox)
1. Když se aplikace spustí, bude zobrazení kolekce se zobrazí pokaždé, když uživatel klikne na tlačítko.
1. Vyberte zobrazení kolekce a zadejte následující hodnoty v **kartu rozložení** z **Explorer vlastnosti**: 

    [![](collection-views-images/collection06.png "Průzkumník vlastnosti")](collection-views-images/collection06.png#lightbox)
1. Tato volba určuje velikost jednotlivých buněk a mezi buněk a zobrazení kolekce vnějšího okraje ohraničení.
1. Vyberte řadiče zobrazení kolekce a nastavte své třídy na `CityCollectionViewController` v **pomůcky karta**: 

    [![](collection-views-images/collection07.png "Nastavit třídy na CityCollectionViewController")](collection-views-images/collection07.png#lightbox)
1. Vyberte zobrazení kolekce a nastavte své třídy na `CityCollectionView` v **pomůcky karta**: 

    [![](collection-views-images/collection08.png "Nastavit třídy na CityCollectionView")](collection-views-images/collection08.png#lightbox)
1. Vyberte buňku zobrazení kolekce a nastavte své třídy na `CityCollectionViewCell` v **pomůcky karta**: 

    [![](collection-views-images/collection09.png "Nastavit třídy na CityCollectionViewCell")](collection-views-images/collection09.png#lightbox)
1. V **pomůcky karta** Ujistěte se, že **rozložení** je `Flow` a **směr posouvání** je `Vertical` pro zobrazení kolekce: 

    [![](collection-views-images/collection10.png "Na kartě pomůcky")](collection-views-images/collection10.png#lightbox)
1. Vyberte buňku zobrazení kolekce a nastavte její **Identity** k `CityCell` v **pomůcky karta**: 

    [![](collection-views-images/collection11.png "Nastavení Identity pro CityCell")](collection-views-images/collection11.png#lightbox)
1. Uložte provedené změny.
    

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

    
1. Spusťte novou **tvOS jediné zobrazení aplikace** v sadě Visual Studio.
1. V **Průzkumníku řešení**, dvakrát klikněte `Main.storyboard` souboru a otevře ji v iOS Designer.
1. Přidání zobrazení bitové kopie, štítek a tlačítko pro existující zobrazit a nakonfigurovat je pro vypadat následovně: 

    [![](collection-views-images/collection02vs.png "Konfigurovat jeho rozložení")](collection-views-images/collection02vs.png#lightbox)
1. Přiřazení **název** k zobrazení bitové kopie a název v **pomůcky karta** z **Explorer vlastnosti**. Příklad: 

    [![](collection-views-images/collection03vs.png "Průzkumník vlastnosti")](collection-views-images/collection03vs.png#lightbox)
1. V dalším kroku přetažením řadič zobrazení kolekce na scénáři: 

    [![](collection-views-images/collection04vs.png "Řadič zobrazení kolekce")](collection-views-images/collection04vs.png#lightbox)
1. Ovládací prvek přetažení z tlačítka řadiče zobrazení kolekcí a vyberte **Push** z místní nabídce: 

    [![](collection-views-images/collection05vs.png "Vyberte nabízené automaticky otevřeném okně.")](collection-views-images/collection05vs.png#lightbox)
1. Když se aplikace spustí, bude zobrazení kolekce se zobrazí pokaždé, když uživatel klikne na tlačítko.
1. Vyberte zobrazení, kolekce a v **kartu rozložení** z **Explorer vlastnosti** zadejte **šířka** jako _361_ a  **Výška** jako _256_ 
1. Tato volba určuje velikost jednotlivých buněk a mezi buněk a zobrazení kolekce vnějšího okraje ohraničení.
1. Vyberte řadiče zobrazení kolekce a nastavte své třídy na `CityCollectionViewController` v **pomůcky karta**: 

    [![](collection-views-images/collection07vs.png "Nastavit třídy na CityCollectionViewController")](collection-views-images/collection07vs.png#lightbox)
1. Vyberte zobrazení kolekce a nastavte své třídy na `CityCollectionView` v **pomůcky karta**: 

    [![](collection-views-images/collection08vs.png "Nastavit třídy na CityCollectionView")](collection-views-images/collection08vs.png#lightbox)
1. Vyberte buňku zobrazení kolekce a nastavte své třídy na `CityCollectionViewCell` v **pomůcky karta**: 

    [![](collection-views-images/collection09vs.png "Nastavit třídy na CityCollectionViewCell")](collection-views-images/collection09vs.png#lightbox)
1. V **pomůcky karta** Ujistěte se, že **rozložení** je `Flow` a **směr posouvání** je `Vertical` pro zobrazení kolekce: 

    [![](collection-views-images/collection10vs.png "Karta Tnelze pomůcky")](collection-views-images/collection10vs.png#lightbox)
1. Vyberte buňku zobrazení kolekce a nastavte její **Identity** k `CityCell` v **pomůcky karta**: 

    [![](collection-views-images/collection11vs.png "Nastavení Identity pro CityCell")](collection-views-images/collection11vs.png#lightbox)
1. Uložte provedené změny.
    

-----

Pokud by byla vybrána `Custom` pro zobrazení kolekce **rozložení**, jsme může zadali vlastní zobrazení. Apple poskytuje integrované `UICollectionViewFlowLayout` a `UICollectionViewDelegateFlowLayout` , můžete snadno v rozložení mřížky na základě dat k dispozici (ty jsou používány `flow` stylu rozložení). 

Další informace o práci s scénářů, najdete v tématu naše [Hello, tvOS úvodní příručce](~/ios/tvos/get-started/hello-tvos.md).

<a name="Providing-Data-for-the-Collection-View" />

## <a name="providing-data-for-the-collection-view"></a>Poskytuje Data pro zobrazení kolekce

Teď, když máme naše zobrazení kolekce (a kolekce View Controller), přidá do našich Storyboard, je potřeba poskytnout data pro kolekci. 

<a name="The-Data-Model" />

### <a name="the-data-model"></a>Datový Model

Nejprve přidáme pro vytvoření modelu pro naše data, která obsahuje název souboru pro bitovou kopii k zobrazení, názvu a příznak pro povolení města, aby byl vybrán.

Vytvoření `CityInfo` třídy a nastavit jej vypadat třeba takto:

```csharp
using System;

namespace tvCollection
{
    public class CityInfo
    {
        #region Computed Properties
        public string ImageFilename { get; set; }
        public string Title { get; set; }
        public bool CanSelect{ get; set; }
        #endregion

        #region Constructors
        public CityInfo (string filename, string title, bool canSelect)
        {
            // Initialize
            this.ImageFilename = filename;
            this.Title = title;
            this.CanSelect = canSelect;
        }
        #endregion
    }
}
```

### <a name="the-collection-view-cell"></a>Buňku zobrazení kolekce

Teď je potřeba definovat, jak bude uvedeno data pro každou buňku. Upravit `CityCollectionViewCell.cs` souboru (pro vás automaticky vytvořen z souboru Storyboard) a nastavit jej vypadat třeba takto:

```csharp
using System;
using Foundation;
using UIKit;
using CoreGraphics;

namespace tvCollection
{
    public partial class CityCollectionViewCell : UICollectionViewCell
    {
        #region Private Variables
        private CityInfo _city;
        #endregion

        #region Computed Properties
        public UIImageView CityView { get ; set; }
        public UILabel CityTitle { get; set; }

        public CityInfo City {
            get { return _city; }
            set {
                _city = value;
                CityView.Image = UIImage.FromFile (City.ImageFilename);
                CityView.Alpha = (City.CanSelect) ? 1.0f : 0.5f;
                CityTitle.Text = City.Title;
            }
        }
        #endregion

        #region Constructors
        public CityCollectionViewCell (IntPtr handle) : base (handle)
        {
            // Initialize
            CityView = new UIImageView(new CGRect(22, 19, 320, 171));
            CityView.AdjustsImageWhenAncestorFocused = true;
            AddSubview (CityView);

            CityTitle = new UILabel (new CGRect (22, 209, 320, 21)) {
                TextAlignment = UITextAlignment.Center,
                TextColor = UIColor.White,
                Alpha = 0.0f
            };
            AddSubview (CityTitle);
        }
        #endregion
    

    }
}
```

Pro naše aplikace tvOS jsme se zobrazení bitovou kopii a volitelný název. Pokud není k dispozici daného města, jsme jsou ztmavení zobrazení bitové kopie pomocí následujícího kódu:

```csharp
CityView.Alpha = (City.CanSelect) ? 1.0f : 0.5f;
```

Když na buňku obsahující bitovou kopii je uvést do režimu vybraný uživatelem, chceme používat integrované paralaxy vliv na něm být nastavení následující vlastnosti:

```csharp
CityView.AdjustsImageWhenAncestorFocused = true;
```

Další informace o navigaci a fokus, najdete v tématu naše [práce s navigační a výběr](~/ios/tvos/app-fundamentals/navigation-focus.md) a [Siri vzdálené a Bluetooth řadiče](~/ios/tvos/platform/remote-bluetooth.md) dokumentaci.


<a name="The-Collection-View-Data-Provider" />

### <a name="the-collection-view-data-provider"></a>Zprostředkovatel dat zobrazení kolekce

Pomocí našeho datového modelu, vytvořit a naše rozložení buněk definované vytvoříme zdroj dat pro naše zobrazení kolekce. Zdroj dat bude nejen zajišťuje zálohování dat, ale také dequeueing buněk k zobrazení jednotlivých buněk na obrazovce.

Vytvoření `CityViewDatasource` třídy a nastavit jej vypadat třeba takto:

```csharp
using System;
using System.Collections.Generic;
using UIKit;
using Foundation;
using CoreGraphics;
using ObjCRuntime;

namespace tvCollection
{
    public class CityViewDatasource : UICollectionViewDataSource
    {
        #region Application Access
        public static AppDelegate App {
            get { return (AppDelegate)UIApplication.SharedApplication.Delegate; }
        }
        #endregion

        #region Static Constants
        public static NSString CardCellId = new NSString ("CityCell");
        #endregion

        #region Computed Properties
        public List<CityInfo> Cities { get; set; } = new List<CityInfo>();
        public CityCollectionView ViewController { get; set; }
        #endregion

        #region Constructors
        public CityViewDatasource (CityCollectionView controller)
        {
            // Initialize
            this.ViewController = controller;
            PopulateCities ();
        }
        #endregion

        #region Public Methods
        public void PopulateCities() {

            // Clear existing cities
            Cities.Clear();

            // Add new cities
            Cities.Add(new CityInfo("City01.jpg", "Houses by Water", false));
            Cities.Add(new CityInfo("City02.jpg", "Turning Circle", true));
            Cities.Add(new CityInfo("City03.jpg", "Skyline at Night", true));
            Cities.Add(new CityInfo("City04.jpg", "Golden Gate Bridge", true));
            Cities.Add(new CityInfo("City05.jpg", "Roads by Night", true));
            Cities.Add(new CityInfo("City06.jpg", "Church Domes", true));
            Cities.Add(new CityInfo("City07.jpg", "Mountain Lights", true));
            Cities.Add(new CityInfo("City08.jpg", "City Scene", false));
            Cities.Add(new CityInfo("City09.jpg", "House in Winter", true));
            Cities.Add(new CityInfo("City10.jpg", "By the Lake", true));
            Cities.Add(new CityInfo("City11.jpg", "At the Dome", true));
            Cities.Add(new CityInfo("City12.jpg", "Cityscape", true));
            Cities.Add(new CityInfo("City13.jpg", "Model City", true));
            Cities.Add(new CityInfo("City14.jpg", "Taxi, Taxi!", true));
            Cities.Add(new CityInfo("City15.jpg", "On the Sidewalk", true));
            Cities.Add(new CityInfo("City16.jpg", "Midnight Walk", true));
            Cities.Add(new CityInfo("City17.jpg", "Lunchtime Cafe", true));
            Cities.Add(new CityInfo("City18.jpg", "Coffee Shop", true));
            Cities.Add(new CityInfo("City19.jpg", "Rustic Tavern", true));
        }
        #endregion

        #region Override Methods
        public override nint NumberOfSections (UICollectionView collectionView)
        {
            return 1;
        }

        public override nint GetItemsCount (UICollectionView collectionView, nint section)
        {
            return Cities.Count;
        }

        public override UICollectionViewCell GetCell (UICollectionView collectionView, NSIndexPath indexPath)
        {
            var cityCell = (CityCollectionViewCell)collectionView.DequeueReusableCell (CardCellId, indexPath);
            var city = Cities [indexPath.Row];

            // Initialize city
            cityCell.City = city;

            return cityCell;
        }
        #endregion
    }
}
```

Umožní, podívejte se na této třídy v podrobností. Nejdřív jsme dědí `UICollectionViewDataSource` a poskytují zástupce buněk ID (kterou jsme přiřazen v Návrháři iOS):

```csharp
public static NSString CardCellId = new NSString ("CityCell");
```

V dalším poskytování úložiště pro naše data kolekce a zadejte třídu k naplnění dat:

```csharp
public List<CityInfo> Cities { get; set; } = new List<CityInfo>();
...

public void PopulateCities() {

    // Clear existing cities
    Cities.Clear();

    // Add new cities
    Cities.Add(new CityInfo("City01.jpg", "Houses by Water", false));
    Cities.Add(new CityInfo("City02.jpg", "Turning Circle", true));
    ...
}
```

Potom jsme přepsat `NumberOfSections` metodu a vrátí počet oddíly (skupiny položek), které chcete zobrazit naše kolekce. V takovém případě je jen jeden:

```csharp
public override nint NumberOfSections (UICollectionView collectionView)
{
    return 1;
}
```

V dalším kroku se vrací počet položek v našem kolekci pomocí následujícího kódu:

```csharp
public override nint GetItemsCount (UICollectionView collectionView, nint section)
{
    return Cities.Count;
}
```

Nakonec jsme dequeue opakovaně použitelné buňku, při zobrazení kolekce žádosti s následujícím kódem:

```csharp
public override UICollectionViewCell GetCell (UICollectionView collectionView, NSIndexPath indexPath)
{
    var cityCell = (CityCollectionViewCell)collectionView.DequeueReusableCell (CardCellId, indexPath);
    var city = Cities [indexPath.Row];

    // Initialize city
    cityCell.City = city;

    return cityCell;
}
```

Poté, co se nám získat buňku zobrazení kolekce z našich `CityCollectionViewCell` typu, jsme jeho naplnění daná položka.

<a name="Responding-to-User-Events" />

## <a name="responding-to-user-events"></a>Reagování na události uživatele

Protože jsme má uživatel bude moci vybrat položku z našich kolekce, potřebujeme zajistit kolekce zobrazení delegáta pro zpracování tato interakce. A je potřeba poskytnout způsob, jak umožnit naše volání zobrazení vědět, jaké položky uživatel vybral.

<a name="The-App-Delegate" />

### <a name="the-app-delegate"></a>Delegát aplikace

Potřebujeme způsob, jak se týkají aktuálně vybrané položky z kolekce zobrazení zpět do volání zobrazení. Budeme používat vlastní vlastnost na našem `AppDelegate`. Upravit `AppDelegate.cs` souboru a přidejte následující kód:

```csharp
public CityInfo SelectedCity { get; set;} = new CityInfo("City02.jpg", "Turning Circle", true);
```

To definuje vlastnost a nastaví výchozí města, který se původně zobrazovat. Později jsme budete využívat tuto vlastnost zobrazit výběr uživatele a povolit select se musí změnit.

<a name="The-Collection-View-Delegate" />

### <a name="the-collection-view-delegate"></a>Delegát zobrazení kolekce

V dalším kroku přidejte nový `CityViewDelegate` třídu do projektu a nastavit jej vypadat třeba takto:


```csharp
using System;
using System.Collections.Generic;
using UIKit;
using Foundation;
using CoreGraphics;

namespace tvCollection
{
    public class CityViewDelegate : UICollectionViewDelegateFlowLayout
    {
        #region Application Access
        public static AppDelegate App {
            get { return (AppDelegate)UIApplication.SharedApplication.Delegate; }
        }
        #endregion

        #region Constructors
        public CityViewDelegate ()
        {
        }
        #endregion

        #region Override Methods
        public override CGSize GetSizeForItem (UICollectionView collectionView, UICollectionViewLayout layout, NSIndexPath indexPath)
        {
            return new CGSize (361, 256);
        }

        public override bool CanFocusItem (UICollectionView collectionView, NSIndexPath indexPath)
        {
            if (indexPath == null) {
                return false;
            } else {
                var controller = collectionView as CityCollectionView;
                return controller.Source.Cities[indexPath.Row].CanSelect;
            }
        }

        public override void ItemSelected (UICollectionView collectionView, NSIndexPath indexPath)
        {
            var controller = collectionView as CityCollectionView;
            App.SelectedCity = controller.Source.Cities [indexPath.Row];

            // Close Collection
            controller.ParentController.DismissViewController(true,null);
        }
        #endregion
    }
}
```

Podívejme bližší pohled na tuto třídu. Nejdřív jsme dědí `UICollectionViewDelegateFlowLayout`. Z důvodu, kterou jsme dědí z této třídy a ne `UICollectionViewDelegate` je, že používáme integrované `UICollectionViewFlowLayout` nabídne naše položky a není typu vlastní rozložení.

V dalším kroku vrátíme velikost jednotlivých položek pomocí tohoto kódu:

```csharp
public override CGSize GetSizeForItem (UICollectionView collectionView, UICollectionViewLayout layout, NSIndexPath indexPath)
{
    return new CGSize (361, 256);
}
```

Potom jsme rozhodněte, pokud daná buňka můžete získat fokus pomocí následujícího kódu: 

```csharp
public override bool CanFocusItem (UICollectionView collectionView, NSIndexPath indexPath)
{
    if (indexPath == null) {
        return false;
    } else {
        var controller = collectionView as CityCollectionView;
        return controller.Source.Cities[indexPath.Row].CanSelect;
    }
}
```

Jsme zkontrolujte, zda má určitou část zálohování dat jeho `CanSelect` příznak nastaven na hodnotu `true` a vrátit tuto hodnotu. Další informace o navigaci a fokus, najdete v tématu naše [práce s navigační a výběr](~/ios/tvos/app-fundamentals/navigation-focus.md) a [Siri vzdálené a Bluetooth řadiče](~/ios/tvos/platform/remote-bluetooth.md) dokumentaci.

Nakonec jsme reagovat na uživatele, vyberte položku s následujícím kódem:

```csharp
public override void ItemSelected (UICollectionView collectionView, NSIndexPath indexPath)
{
    var controller = collectionView as CityCollectionView;
    App.SelectedCity = controller.Source.Cities [indexPath.Row];

    // Close Collection
    controller.ParentController.DismissViewController(true,null);
}
```

Zde jsme nastavit `SelectedCity` vlastnost naše `AppDelegate` na položku, vyberte uživatele a My zavřete řadiče zobrazení kolekce, vrátíte k zobrazení, která nám volána. Nebyly definovaného `ParentController` vlastnost naše zobrazení kolekce ještě, provedeme to další.

<a name="Configuring-the-Collection-View" />

## <a name="configuring-the-collection-view"></a>Konfigurace zobrazení kolekce

Teď musíme upravit naše zobrazení kolekce a přiřazení našeho zdroj dat a delegáta. Upravit `CityCollectionView.cs` souboru (pro nás automaticky vytvořen z našich Storyboard) a nastavit jej vypadat třeba takto:

```csharp
using System;
using Foundation;
using UIKit;

namespace tvCollection
{
    public partial class CityCollectionView : UICollectionView
    {
        #region Application Access
        public static AppDelegate App {
            get { return (AppDelegate)UIApplication.SharedApplication.Delegate; }
        }
        #endregion

        #region Computed Properties
        public CityViewDatasource Source {
            get { return DataSource as CityViewDatasource;}
        }

        public CityCollectionViewController ParentController { get; set;}
        #endregion

        #region Constructors
        public CityCollectionView (IntPtr handle) : base (handle)
        {
            // Initialize
            RegisterClassForCell (typeof(CityCollectionViewCell), CityViewDatasource.CardCellId);
            DataSource = new CityViewDatasource (this);
            Delegate = new CityViewDelegate ();
        }
        #endregion

        #region Override Methods
        public override nint NumberOfSections ()
        {
            return 1;
        }

        public override void DidUpdateFocus (UIFocusUpdateContext context, UIFocusAnimationCoordinator coordinator)
        {
            var previousItem = context.PreviouslyFocusedView as CityCollectionViewCell;
            if (previousItem != null) {
                Animate (0.2, () => {
                    previousItem.CityTitle.Alpha = 0.0f;
                });
            }

            var nextItem = context.NextFocusedView as CityCollectionViewCell;
            if (nextItem != null) {
                Animate (0.2, () => {
                    nextItem.CityTitle.Alpha = 1.0f;
                });
            }
        }
        #endregion
    }
}
```

Nejprve poskytujeme zástupce pro přístup k naší `AppDelegate`: 

```csharp
public static AppDelegate App {
    get { return (AppDelegate)UIApplication.SharedApplication.Delegate; }
}
```

V dalším kroku poskytujeme zástupce pro zdroj dat zobrazení kolekce a vlastnost přístup k řadiče zobrazení kolekce (používané naše delegáta výše zavřete kolekce, když uživatel provede výběr):

```csharp
public CityViewDatasource Source {
    get { return DataSource as CityViewDatasource;}
}

public CityCollectionViewController ParentController { get; set;}
```

Pak můžeme použít následující kód k inicializovat zobrazení kolekce a přiřazení našeho třída buňky, zdroj dat a delegáta:

```csharp
public CityCollectionView (IntPtr handle) : base (handle)
{
    // Initialize
    RegisterClassForCell (typeof(CityCollectionViewCell), CityViewDatasource.CardCellId);
    DataSource = new CityViewDatasource (this);
    Delegate = new CityViewDelegate ();
}
```

Nakonec chcete nadpis pod obrázek, který má je zobrazen, jen když má uživatel, se zvýrazněnou (vybraný). Provedeme to pomocí následujícího kódu:

```csharp
public override void DidUpdateFocus (UIFocusUpdateContext context, UIFocusAnimationCoordinator coordinator)
{
    var previousItem = context.PreviouslyFocusedView as CityCollectionViewCell;
    if (previousItem != null) {
        Animate (0.2, () => {
            previousItem.CityTitle.Alpha = 0.0f;
        });
    }

    var nextItem = context.NextFocusedView as CityCollectionViewCell;
    if (nextItem != null) {
        Animate (0.2, () => {
            nextItem.CityTitle.Alpha = 1.0f;
        });
    }
}
```

Nastaví transparentnost předchozí položce ztráty fokus na nulu (0) a transparentnost na další položku získat fokus na 100 %. Tyto přechod získat animované také.


## <a name="configuring-the-collection-view-controller"></a>Konfigurace řadiče zobrazení kolekce

Teď je potřeba udělat finální konfiguraci na našem zobrazení kolekce a umožňují řadiči set pro vlastnost, která jsme definovali tak zobrazení kolekce je možné uzavřít, až uživatel provede výběr.

Upravit `CityCollectionViewController.cs` souboru (automaticky vytvořen z našich Storyboard) a nastavit jej vypadat třeba takto:

```csharp
// This file has been autogenerated from a class added in the UI designer.

using System;

using Foundation;
using UIKit;

namespace tvCollection
{
    public partial class CityCollectionViewController : UICollectionViewController
    {
        #region Computed Properties
        public CityCollectionView Collection {
            get { return CollectionView as CityCollectionView; }
        }
        #endregion

        #region Constructors
        public CityCollectionViewController (IntPtr handle) : base (handle)
        {
        }
        #endregion

        #region Override Methods
        public override void AwakeFromNib ()
        {
            base.AwakeFromNib ();

            // Save link to controller
            Collection.ParentController = this;
        }
        #endregion
    }
}

```

## <a name="putting-it-all-together"></a>Jeho uvedení společně všechny 

Teď, když máme všech částí připravili k naplnění a řízení naše zobrazení kolekce musíme mít poslední úpravy sdružujícího všechno, co naše hlavního zobrazení.

Upravit `ViewController.cs` souboru (automaticky vytvořen z našich Storyboard) a nastavit jej vypadat třeba takto:

```csharp
using System;
using Foundation;
using UIKit;
using tvCollection;

namespace MySingleView
{
    public partial class ViewController : UIViewController
    {
        #region Application Access
        public static AppDelegate App {
            get { return (AppDelegate)UIApplication.SharedApplication.Delegate; }
        }
        #endregion

        #region Constructors
        public ViewController (IntPtr handle) : base (handle)
        {
        }
        #endregion

        #region Override Methods
        public override void ViewDidLoad ()
        {
            base.ViewDidLoad ();
            // Perform any additional setup after loading the view, typically from a nib.
        }

        public override void ViewWillAppear (bool animated)
        {
            base.ViewWillAppear (animated);

            // Update image with the currently selected one
            CityView.Image = UIImage.FromFile(App.SelectedCity.ImageFilename);
            BackgroundView.Image = CityView.Image;
            CityTitle.Text = App.SelectedCity.Title;
        }

        public override void DidReceiveMemoryWarning ()
        {
            base.DidReceiveMemoryWarning ();
            // Release any cached data, images, etc that aren't in use.
        }
        #endregion
    }
}
```

Následující kód po jeho otevření zobrazí vybrané položky ze `SelectedCity` vlastnost `AppDelegate` a znovu se zobrazí, když uživatel provede výběr z pohledu kolekce:

```csharp
public override void ViewWillAppear (bool animated)
{
    base.ViewWillAppear (animated);

    // Update image with the currently selected one
    CityView.Image = UIImage.FromFile(App.SelectedCity.ImageFilename);
    BackgroundView.Image = CityView.Image;
    CityTitle.Text = App.SelectedCity.Title;
}
```

<a name="Testing-the-app" />

## <a name="testing-the-app"></a>Testování aplikace

S vše na místě, pokud je sestavení a spuštění aplikace, hlavní je zobrazeno zobrazení s města výchozí:

[![](collection-views-images/run01.png "Hlavní obrazovky")](collection-views-images/run01.png#lightbox)

Pokud uživatel kliknutím na **vyberte zobrazení** tlačítko zobrazení kolekce se zobrazí:

[![](collection-views-images/run02.png "Zobrazení kolekce")](collection-views-images/run02.png#lightbox)

Všechny města, který má jeho `CanSelect` vlastnost nastavena na hodnotu `false` se zobrazí neaktivní a uživatel nebude moci zaměřit se na ni. Když uživatel označuje položku (ujistěte se, je vybraný) se zobrazuje název a používají paralaxy efekt subtlety náklon bitovou kopii v 3D.

Když uživatel klikne vyberte bitovou kopii, zobrazení kolekce se zavře a hlavního zobrazení se zobrazí znovu s novou bitovou kopii:

[![](collection-views-images/run03.png "Novou bitovou kopii na domovské obrazovce")](collection-views-images/run03.png#lightbox)

<a name="Creating-Custom-Layout-and-Reordering-Items" />

## <a name="creating-custom-layout-and-reordering-items"></a>Vytvoření vlastního rozložení a změna uspořádání položek

Jedním z klíčových funkcích služby pomocí zobrazení kolekce je možnost vytvářet vlastní rozložení. Vzhledem k tomu, že tvOS dědí z iOS, proces pro vytvoření vlastního rozložení je stejný. Najdete v tématu naše [Úvod do zobrazení kolekce](~/ios/user-interface/controls/uicollectionview.md) Další informace naleznete v dokumentaci.

Nedávno přidané do zobrazení kolekcí pro iOS 9 byla možnost snadno povolit změny pořadí položek v kolekci. Znovu, protože tvOS 9 je podmnožinou iOS 9, to se provádí je stejným způsobem. Najdete v tématu naše [zobrazení změn v kolekci](~/ios/user-interface/controls/uicollectionview.md) dokumentu další podrobnosti.


<a name="Summary" />

## <a name="summary"></a>Souhrn

Tento článek má zahrnutých navrhování a práce se zobrazeními kolekce uvnitř Xamarin.tvOS aplikace. Nejprve popsané všechny elementy, které tvoří zobrazení kolekce. V dalším kroku se vám ukázal, jak navrhnout a implementovat zobrazení kolekce pomocí scénáře. Nakonec se poskytuje odkazy na informace o vytvoření vlastní rozložení a změna uspořádání položek.



## <a name="related-links"></a>Související odkazy

- [Ukázky pro tvOS](https://developer.xamarin.com/samples/tvos/all/)
- [tvOS](https://developer.apple.com/tvos/)
- [tvOS lidské rozhraní příručky](https://developer.apple.com/tvos/human-interface-guidelines/)
- [Průvodce programováním aplikace pro tvOS](https://developer.apple.com/library/prerelease/tvos/documentation/General/Conceptual/AppleTV_PG/)
