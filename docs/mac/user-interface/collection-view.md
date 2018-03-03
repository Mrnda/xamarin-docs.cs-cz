---
title: "Zobrazení kolekce"
description: "Tento článek popisuje práci s zobrazení kolekcí v aplikaci Xamarin.Mac. Vysvětluje vytváření a údržbu zobrazení kolekcí v Xcode a Tvůrce rozhraní a práce s nimi prostřednictvím kódu programu."
ms.topic: article
ms.prod: xamarin
ms.assetid: 6EE32256-5948-4AE4-8133-6D0B3F4173E8
ms.technology: xamarin-mac
author: bradumbaugh
ms.author: brumbaug
ms.date: 05/24/2017
ms.openlocfilehash: d8fa54f23dfea063fa25f6e26e2df2c2ed82101e
ms.sourcegitcommit: 61f5ecc5a2b5dcfbefdef91664d7460c0ee2f357
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 02/28/2018
---
# <a name="collection-views"></a>Zobrazení kolekce

_Tento článek popisuje práci s zobrazení kolekcí v aplikaci Xamarin.Mac. Vysvětluje vytváření a údržbu zobrazení kolekcí v Xcode a Tvůrce rozhraní a práce s nimi prostřednictvím kódu programu._

Při práci s C# a rozhraní .NET v aplikaci Xamarin.Mac, vývojář má přístup ke stejné zobrazení kolekce AppKit prvky, které vývojář práce *jazyka Objective-C* a *Xcode* nepodporuje. Protože Xamarin.Mac integruje přímo s Xcode, používá vývojář na Xcode _rozhraní tvůrce_ pro vytváření a údržbu zobrazení kolekcí.

A `NSCollectionView` zobrazí mřížku uspořádané pomocí dílčích zobrazení `NSCollectionViewLayout`. Je reprezentována každý dílčí zobrazení v mřížce `NSCollectionViewItem` které spravuje načítání zobrazení obsahu z `.xib` souboru.

[ ![Příklad aplikace spustit](collection-view-images/intro01.png)](collection-view-images/intro01.png)

Tento článek popisuje základní informace o práci s zobrazení kolekcí v aplikaci Xamarin.Mac. Vysoce navržený na spolupracovat [Hello, Mac](~/mac/get-started/hello-mac.md) článek nejprve, konkrétně [Úvod do Xcode a rozhraní tvůrce](~/mac/get-started/hello-mac.md#Introduction_to_Xcode_and_Interface_Builder) a [výstupy a akce](~/mac/get-started/hello-mac.md#Outlets_and_Actions) oddíly, jak se popisuje klíčové koncepty a techniky, které se používají v tomto článku.

Můžete chtít podívejte se na [třídy vystavení jazyka C# nebo metody jazyka Objective-C](~/mac/internals/how-it-works.md) části [Xamarin.Mac Internals](~/mac/internals/how-it-works.md) dokumentu taky vysvětluje `Register` a `Export` příkazy používá k navázání tříd jazyka C# jazyka Objective-C objekty a prvky uživatelského rozhraní.

<a name="About_Collection_Views"/>

## <a name="about-collection-views"></a>O zobrazení kolekce

Hlavním cílem zobrazení kolekce (`NSCollectionView`) je vizuálně uspořádat skupiny objektů uspořádány způsobem pomocí rozložení zobrazení kolekce (`NSCollectionViewLayout`), s každého jednotlivého objektu (`NSCollectionViewItem`) v kolekci větší získávání vlastní zobrazení. Zobrazení kolekce fungovat prostřednictvím datové vazby a klíč-hodnota kódování technik a jako takový, měli byste si přečíst [datové vazby a klíč-hodnota kódování](~/mac/app-fundamentals/databinding.md) dokumentace před pokračováním v tomto článku.

Zobrazení kolekce má žádné standardní, předdefinované kolekce položky zobrazení (jako jsou nemá na obrys nebo zobrazení tabulky), takže vývojáři zodpovídá za navrhování a implementace _prototypu zobrazení_ pomocí jiných ovládacích prvků AppKit například pole bitové kopie , Textových polí, popisky, atd. Toto zobrazení prototypu se použije k zobrazení a pracovat s každou položku je spravováno nástrojem zobrazení kolekce a je uložen ve `.xib` souboru.

Vzhledem k tomu, že vývojář je zodpovědná za vzhledu a chování položky kolekce zobrazení, zobrazení kolekce má žádné integrovanou podporu pro zvýraznění vybrané položky v mřížce. Implementace této funkce se budeme v tomto článku.

<a name="Defining_your_Data_Model"/>

## <a name="defining-the-data-model"></a>Definování datový Model

Před datové vazby zobrazení kolekce v Tvůrci rozhraní, klíč-hodnota kódování (KVC) / kompatibilní třída sledování klíč-hodnota (KVO) musí být definován v aplikaci Xamarin.Mac tak, aby fungoval jako _datový Model_ pro vazbu. Datový Model obsahuje všechna data, která se zobrazí v kolekci a přijímá veškeré úpravy data, která uživatel provede v uživatelském rozhraní při spuštění aplikace.

Proveďte v příkladu aplikaci, která spravuje skupinu zaměstnanců, následující třídy může definují Model dat:

```csharp
using System;
using Foundation;
using AppKit;

namespace MacDatabinding
{
    [Register("PersonModel")]
    public class PersonModel : NSObject
    {
        #region Private Variables
        private string _name = "";
        private string _occupation = "";
        private bool _isManager = false;
        private NSMutableArray _people = new NSMutableArray();
        #endregion

        #region Computed Properties
        [Export("Name")]
        public string Name {
            get { return _name; }
            set {
                WillChangeValue ("Name");
                _name = value;
                DidChangeValue ("Name");
            }
        }

        [Export("Occupation")]
        public string Occupation {
            get { return _occupation; }
            set {
                WillChangeValue ("Occupation");
                _occupation = value;
                DidChangeValue ("Occupation");
            }
        }

        [Export("isManager")]
        public bool isManager {
            get { return _isManager; }
            set {
                WillChangeValue ("isManager");
                WillChangeValue ("Icon");
                _isManager = value;
                DidChangeValue ("isManager");
                DidChangeValue ("Icon");
            }
        }

        [Export("isEmployee")]
        public bool isEmployee {
            get { return (NumberOfEmployees == 0); }
        }

        [Export("Icon")]
        public NSImage Icon
        {
            get
            {
                if (isManager)
                {
                    return NSImage.ImageNamed("IconGroup");
                }
                else
                {
                    return NSImage.ImageNamed("IconUser");
                }
            }
        }

        [Export("personModelArray")]
        public NSArray People {
            get { return _people; }
        }

        [Export("NumberOfEmployees")]
        public nint NumberOfEmployees {
            get { return (nint)_people.Count; }
        }
        #endregion

        #region Constructors
        public PersonModel ()
        {
        }

        public PersonModel (string name, string occupation)
        {
            // Initialize
            this.Name = name;
            this.Occupation = occupation;
        }

        public PersonModel (string name, string occupation, bool manager)
        {
            // Initialize
            this.Name = name;
            this.Occupation = occupation;
            this.isManager = manager;
        }
        #endregion

        #region Array Controller Methods
        [Export("addObject:")]
        public void AddPerson(PersonModel person) {
            WillChangeValue ("personModelArray");
            isManager = true;
            _people.Add (person);
            DidChangeValue ("personModelArray");
        }

        [Export("insertObject:inPersonModelArrayAtIndex:")]
        public void InsertPerson(PersonModel person, nint index) {
            WillChangeValue ("personModelArray");
            _people.Insert (person, index);
            DidChangeValue ("personModelArray");
        }

        [Export("removeObjectFromPersonModelArrayAtIndex:")]
        public void RemovePerson(nint index) {
            WillChangeValue ("personModelArray");
            _people.RemoveObject (index);
            DidChangeValue ("personModelArray");
        }

        [Export("setPersonModelArray:")]
        public void SetPeople(NSMutableArray array) {
            WillChangeValue ("personModelArray");
            _people = array;
            DidChangeValue ("personModelArray");
        }
        #endregion
    }
}
```

`PersonModel` Datového modelu se použije po celý zbytek tohoto článku.

<a name="Working_with_a_Collection_View"/>

## <a name="working-with-a-collection-view"></a>Práce s zobrazení kolekce

Datová vazba s zobrazení kolekce je hodně like vazbu pomocí zobrazení tabulky, jako `NSCollectionViewDataSource` slouží k poskytování dat pro kolekci. Vzhledem k tomu, že zobrazení kolekce nemá formát přednastavené zobrazení, další práci a je nutné k poskytnutí zpětné vazby interakce uživatelů ke sledování výběr uživatele.

<a name="Creating-the-Cell-Prototype"/>

### <a name="creating-the-cell-prototype"></a>Vytváření prototypů buňky

Protože zobrazení kolekce nezahrnuje prototyp výchozí buňky, bude nutné přidat jeden nebo víc Vývojář `.xib` soubory do aplikace Xamarin.Mac zadat rozložení a obsahu jednotlivých buněk.

Postupujte takto:

1. V **Průzkumníku řešení**, klikněte pravým tlačítkem na název projektu a vyberte **přidat** > **nový soubor...**
2. Vyberte **Mac** > **View Controller**, zadejte jeho název (například `EmployeeItem` v tomto příkladu) a klikněte na tlačítko **nový** tlačítko vytvořte: 

    ![Přidávání nového řadiče zobrazení](collection-view-images/proto01.png)

    Bude přidáno `EmployeeItem.cs`, `EmployeeItemController.cs` a `EmployeeItemController.xib` souboru projektu řešení.
3. Dvakrát klikněte `EmployeeItemController.xib` soubor otevřete pro úpravy v Tvůrci rozhraní pro Xcode.
4. Přidat `NSBox`, `NSImageView` a dvě `NSLabel` ovládacích prvků do zobrazení a rozložení je následujícím způsobem:

    ![Navrhování rozložení buněk prototypu](collection-view-images/proto02.png)
5. Otevřete **pomocníka Editor** a vytvoření **výstupu** pro `NSBox` , aby se může použít k označení stavu výběr buňky:

    ![Vystavení NSBox v výstupu](collection-view-images/proto03.png)
6. Vraťte se do **Standard Editor** a vyberte zobrazení bitové kopie.
7. V **vazby Inspector**, vyberte **vytvořit vazbu k** > **vlastník souboru** a zadejte **cesta ke klíči modelu** z `self.Person.Icon`:

    ![Vytvoření vazby na ikonu](collection-view-images/proto04.png)
8. Vyberte první popisek a v **vazby Inspector**, vyberte **vytvořit vazbu k** > **vlastník souboru** a zadejte **cesta ke klíči modelu**z `self.Person.Name`:

    ![Vazba název](collection-view-images/proto05.png)
9. Vyberte štítek, druhý a v **vazby Inspector**, vyberte **vytvořit vazbu k** > **vlastník souboru** a zadejte **cesta ke klíči modelu**z `self.Person.Occupation`:

    ![Vazba povolání](collection-view-images/proto06.png)
10. Uložit změny `.xib` souboru a vrátíte se k sadě Visual Studio k synchronizaci změn.

Upravit `EmployeeItemController.cs` souboru a nastavit jej vypadat třeba takto:

```csharp
using System;
using System.Collections.Generic;
using System.Linq;
using Foundation;
using AppKit;

namespace MacCollectionNew
{
    /// <summary>
    /// The Employee item controller handles the display of the individual items that will
    /// be displayed in the collection view as defined in the associated .XIB file.
    /// </summary>
    public partial class EmployeeItemController : NSCollectionViewItem
    {
        #region Private Variables
        /// <summary>
        /// The person that will be displayed.
        /// </summary>
        private PersonModel _person;
        #endregion

        #region Computed Properties
        // strongly typed view accessor
        public new EmployeeItem View
        {
            get
            {
                return (EmployeeItem)base.View;
            }
        }

        /// <summary>
        /// Gets or sets the person.
        /// </summary>
        /// <value>The person that this item belongs to.</value>
        [Export("Person")]
        public PersonModel Person
        {
            get { return _person; }
            set
            {
                WillChangeValue("Person");
                _person = value;
                DidChangeValue("Person");
            }
        }

        /// <summary>
        /// Gets or sets the color of the background for the item.
        /// </summary>
        /// <value>The color of the background.</value>
        public NSColor BackgroundColor {
            get { return Background.FillColor; }
            set { Background.FillColor = value; }
        }

        /// <summary>
        /// Gets or sets a value indicating whether this <see cref="T:MacCollectionNew.EmployeeItemController"/> is selected.
        /// </summary>
        /// <value><c>true</c> if selected; otherwise, <c>false</c>.</value>
        /// <remarks>This also changes the background color based on the selected state
        /// of the item.</remarks>
        public override bool Selected
        {
            get
            {
                return base.Selected;
            }
            set
            {
                base.Selected = value;

                // Set background color based on the selection state
                if (value) {
                    BackgroundColor = NSColor.DarkGray;
                } else {
                    BackgroundColor = NSColor.LightGray;
                }
            }
        }
        #endregion

        #region Constructors
        // Called when created from unmanaged code
        public EmployeeItemController(IntPtr handle) : base(handle)
        {
            Initialize();
        }

        // Called when created directly from a XIB file
        [Export("initWithCoder:")]
        public EmployeeItemController(NSCoder coder) : base(coder)
        {
            Initialize();
        }

        // Call to load from the XIB/NIB file
        public EmployeeItemController() : base("EmployeeItem", NSBundle.MainBundle)
        {
            Initialize();
        }

        // Added to support loading from XIB/NIB
        public EmployeeItemController(string nibName, NSBundle nibBundle) : base(nibName, nibBundle) {

            Initialize();
        }

        // Shared initialization code
        void Initialize()
        {
        }
        #endregion
    }
}
```

Tento kód podrobně prohlížení, třídy dědí z `NSCollectionViewItem` , takže může fungovat jako prototyp pro buňku zobrazení kolekce. `Person` Vlastnost zpřístupní třídu, která byla použita k vazbě dat na zobrazení bitové kopie a popisky v Xcode. Toto je instance `PersonModel` vytvořili výše.

`BackgroundColor` Vlastnost je zástupce `NSBox` ovládacího prvku `FillColor` který se použije k zobrazení stavu výběr buňky. Přepsáním `Selected` vlastnost `NSCollectionViewItem`, následující kód Zapne nebo vypne tento stav výběru:

```csharp
public override bool Selected
{
    get
    {
        return base.Selected;
    }
    set
    {
        base.Selected = value;

        // Set background color based on the selection state
        if (value) {
            BackgroundColor = NSColor.DarkGray;
        } else {
            BackgroundColor = NSColor.LightGray;
        }
    }
}
```

<a name="Creating-the-Collection-View-Data-Source"/>

### <a name="creating-the-collection-view-data-source"></a>Vytváření zdroje dat zobrazení kolekce

Zdroj dat zobrazení kolekce (`NSCollectionViewDataSource`) poskytuje všechna data pro zobrazení kolekce a vytvoří a naplní buňku zobrazení kolekce (pomocí `.xib` prototypu) podle potřeby pro každou položku v kolekci.

Přidejte novou třídu projektu, volání `CollectionViewDataSource` vypadá takto:

```csharp
using System;
using System.Collections.Generic;
using AppKit;
using Foundation;

namespace MacCollectionNew
{
    /// <summary>
    /// Collection view data source provides the data for the collection view.
    /// </summary>
    public class CollectionViewDataSource : NSCollectionViewDataSource
    {
        #region Computed Properties
        /// <summary>
        /// Gets or sets the parent collection view.
        /// </summary>
        /// <value>The parent collection view.</value>
        public NSCollectionView ParentCollectionView { get; set; }

        /// <summary>
        /// Gets or sets the data that will be displayed in the collection.
        /// </summary>
        /// <value>A collection of PersonModel objects.</value>
        public List<PersonModel> Data { get; set; } = new List<PersonModel>();
        #endregion

        #region Constructors
        /// <summary>
        /// Initializes a new instance of the <see cref="T:MacCollectionNew.CollectionViewDataSource"/> class.
        /// </summary>
        /// <param name="parent">The parent collection that this datasource will provide data for.</param>
        public CollectionViewDataSource(NSCollectionView parent)
        {
            // Initialize
            ParentCollectionView = parent;

            // Attach to collection view
            parent.DataSource = this;

        }
        #endregion

        #region Override Methods
        /// <summary>
        /// Gets the number of sections.
        /// </summary>
        /// <returns>The number of sections.</returns>
        /// <param name="collectionView">The parent Collection view.</param>
        public override nint GetNumberOfSections(NSCollectionView collectionView)
        {
            // There is only one section in this view
            return 1;
        }

        /// <summary>
        /// Gets the number of items in the given section.
        /// </summary>
        /// <returns>The number of items.</returns>
        /// <param name="collectionView">The parent Collection view.</param>
        /// <param name="section">The Section number to count items for.</param>
        public override nint GetNumberofItems(NSCollectionView collectionView, nint section)
        {
            // Return the number of items
            return Data.Count;
        }

        /// <summary>
        /// Gets the item for the give section and item index.
        /// </summary>
        /// <returns>The item.</returns>
        /// <param name="collectionView">The parent Collection view.</param>
        /// <param name="indexPath">Index path specifying the section and index.</param>
        public override NSCollectionViewItem GetItem(NSCollectionView collectionView, NSIndexPath indexPath)
        {
            var item = collectionView.MakeItem("EmployeeCell", indexPath) as EmployeeItemController;
            item.Person = Data[(int)indexPath.Item];

            return item;
        }
        #endregion
    }
}
```

Tento kód podrobně prohlížení, třídy dědí z `NSCollectionViewDataSource` a zveřejňuje seznam `PersonModel` instance prostřednictvím jeho `Data` vlastnost.

Protože tato kolekce má jenom jeden oddíl, přepíše kód `GetNumberOfSections` metoda a vždy vrátí `1`. Kromě toho `GetNumberofItems` je metoda potlačena v vrátí počet položek v `Data` seznam vlastností.

`GetItem` Metoda je volána, když nové buňky je povinná a vypadá takto:

```csharp
public override NSCollectionViewItem GetItem(NSCollectionView collectionView, NSIndexPath indexPath)
{
    var item = collectionView.MakeItem("EmployeeCell", indexPath) as EmployeeItemController;
    item.Person = Data[(int)indexPath.Item];

    return item;
}
```

`MakeItem` Je volána metoda zobrazení kolekce k vytvoření nebo vrátit opakovaně použitelné instanci `EmployeeItemController` a jeho `Person` vlastnost je nastavena na položky se zobrazuje v požadované buňky. 

`EmployeeItemController` Musí být registrovaný pomocí řadiče zobrazení kolekce předem pomocí následujícího kódu:

```csharp
EmployeeCollection.RegisterClassForItem(typeof(EmployeeItemController), "EmployeeCell");
``` 

**Identifikátor** (`EmployeeCell`) používán `MakeItem` volání _musí_ odpovídal názvu řadiče zobrazení, který byl zaregistrován pomocí zobrazení kolekce. Tento krok se budeme rozpis naleznete níže.

<a name="Handling-Item-Selection"/>

### <a name="handling-item-selection"></a>Výběr položek zpracování

Pro zpracování výběr a deselection položky v kolekci, `NSCollectionViewDelegate` se bude vyžadovat. Vzhledem k tomu, že v tomto příkladu budete používat předdefinovaných v `NSCollectionViewFlowLayout` typ rozložení `NSCollectionViewDelegateFlowLayout` se bude vyžadovat určité verze tohoto delegáta.

Přidejte novou třídu do projektu, volání `CollectionViewDelegate` vypadá takto:

```csharp
using System;
using Foundation;
using AppKit;

namespace MacCollectionNew
{
    /// <summary>
    /// Collection view delegate handles user interaction with the elements of the 
    /// collection view for the Flow-Based layout type.
    /// </summary>
    public class CollectionViewDelegate : NSCollectionViewDelegateFlowLayout
    {
        #region Computed Properties
        /// <summary>
        /// Gets or sets the parent view controller.
        /// </summary>
        /// <value>The parent view controller.</value>
        public ViewController ParentViewController { get; set; }
        #endregion

        #region Constructors
        /// <summary>
        /// Initializes a new instance of the <see cref="T:MacCollectionNew.CollectionViewDelegate"/> class.
        /// </summary>
        /// <param name="parentViewController">Parent view controller.</param>
        public CollectionViewDelegate(ViewController parentViewController)
        {
            // Initialize
            ParentViewController = parentViewController;
        }
        #endregion

        #region Override Methods
        /// <summary>
        /// Handles one or more items being selected.
        /// </summary>
        /// <param name="collectionView">The parent Collection view.</param>
        /// <param name="indexPaths">The Index paths of the items being selected.</param>
        public override void ItemsSelected(NSCollectionView collectionView, NSSet indexPaths)
        {
            // Dereference path
            var paths = indexPaths.ToArray<NSIndexPath>();
            var index = (int)paths[0].Item;

            // Save the selected item
            ParentViewController.PersonSelected = ParentViewController.Datasource.Data[index];

        }

        /// <summary>
        /// Handles one or more items being deselected.
        /// </summary>
        /// <param name="collectionView">The parent Collection view.</param>
        /// <param name="indexPaths">The Index paths of the items being deselected.</param>
        public override void ItemsDeselected(NSCollectionView collectionView, NSSet indexPaths)
        {
            // Dereference path
            var paths = indexPaths.ToArray<NSIndexPath>();
            var index = paths[0].Item;

            // Clear selection
            ParentViewController.PersonSelected = null;
        }
        #endregion
    }
}
``` 

`ItemsSelected` a `ItemsDeselected` metody jsou přepsat a použít k nastavení nebo vymazání `PersonSelected` vlastnost řadiče zobrazení, který je zpracování zobrazení kolekce, když uživatel vybere nebo zruší výběr položky. To bude zobrazen v níže uvedené podrobnosti.

<a name="Creating-the-Collection-View-in-Interface-Builder"/>

### <a name="creating-the-collection-view-in-interface-builder"></a>Vytváření zobrazení kolekce v Tvůrci rozhraní

Se všemi požadované podpůrné vytvořené v místní hlavní storyboard lze upravovat a zobrazení kolekce přidána.

Postupujte takto:

1. Dvakrát klikněte `Main.Storyboard` souboru v **Průzkumníku řešení** otevřete pro úpravy v Xcode je rozhraní tvůrce.
2. Přetáhněte zobrazení kolekce do hlavní zobrazení a změnit tak, aby vyplnil zobrazení velikost:

    ![Přidání zobrazení kolekce k rozložení](collection-view-images/collection01.png)
3. Pomocí zobrazení kolekce vybraná připnout na zobrazení při změně velikosti pomocí editoru omezení:

    ![Přidání omezení](collection-view-images/collection02.png)
4. Zkontrolujte, že je vybraný zobrazení kolekce v **návrhová plocha** (a ne **obrysy označující posuňte zobrazení** nebo **klip zobrazení** který ji obsahuje), přepnout na  **Pomocník pro Editor** a vytvoření **výstupu** pro zobrazení kolekce:

    ![Přidání omezení](collection-view-images/collection03.png)
5. Uložte změny a vrátit k sadě Visual Studio k synchronizaci.

<a name="Bringing-it-all-Together"/>

## <a name="bringing-it-all-together"></a>Převedení všechny společně

Všechny podpůrné částí teď byla umístili do místní s třídou tak, aby fungoval jako datový model (`PersonModel`), `NSCollectionViewDataSource` byla přidána k poskytování dat, `NSCollectionViewDelegateFlowLayout` byla vytvořena pro zpracování výběr položek a `NSCollectionView` byla přidána do Main Storyboard a zveřejněné jako výstupu (`EmployeeCollection`).

Posledním krokem je upravit řadiče zobrazení, která obsahuje zobrazení kolekce a převeďte všechny části společně a naplňte kolekce a zpracování výběr položek.

Upravit `ViewController.cs` souboru a nastavit jej vypadat třeba takto:

```csharp
using System;
using AppKit;
using Foundation;
using CoreGraphics;

namespace MacCollectionNew
{
    /// <summary>
    /// The View controller controls the main view that houses the Collection View.
    /// </summary>
    public partial class ViewController : NSViewController
    {
        #region Private Variables
        private PersonModel _personSelected;
        private bool shouldEdit = true;
        #endregion

        #region Computed Properties
        /// <summary>
        /// Gets or sets the datasource that provides the data to display in the 
        /// Collection View.
        /// </summary>
        /// <value>The datasource.</value>
        public CollectionViewDataSource Datasource { get; set; }

        /// <summary>
        /// Gets or sets the person currently selected in the collection view.
        /// </summary>
        /// <value>The person selected or <c>null</c> if no person is selected.</value>
        [Export("PersonSelected")]
        public PersonModel PersonSelected
        {
            get { return _personSelected; }
            set
            {
                WillChangeValue("PersonSelected");
                _personSelected = value;
                DidChangeValue("PersonSelected");
                RaiseSelectionChanged();
            }
        }
        #endregion

        #region Constructors
        /// <summary>
        /// Initializes a new instance of the <see cref="T:MacCollectionNew.ViewController"/> class.
        /// </summary>
        /// <param name="handle">Handle.</param>
        public ViewController(IntPtr handle) : base(handle)
        {
        }
        #endregion

        #region Override Methods
        /// <summary>
        /// Called after the view has finished loading from the Storyboard to allow it to
        /// be configured before displaying to the user.
        /// </summary>
        public override void ViewDidLoad()
        {
            base.ViewDidLoad();

            // Initialize Collection View
            ConfigureCollectionView();
            PopulateWithData();
        }
        #endregion

        #region Private Methods
        /// <summary>
        /// Configures the collection view.
        /// </summary>
        private void ConfigureCollectionView()
        {
            EmployeeCollection.RegisterClassForItem(typeof(EmployeeItemController), "EmployeeCell");

            // Create a flow layout
            var flowLayout = new NSCollectionViewFlowLayout()
            {
                ItemSize = new CGSize(150, 150),
                SectionInset = new NSEdgeInsets(10, 10, 10, 20),
                MinimumInteritemSpacing = 10,
                MinimumLineSpacing = 10
            };
            EmployeeCollection.WantsLayer = true;

            // Setup collection view
            EmployeeCollection.CollectionViewLayout = flowLayout;
            EmployeeCollection.Delegate = new CollectionViewDelegate(this);

        }

        /// <summary>
        /// Populates the Datasource with data and attaches it to the collection view.
        /// </summary>
        private void PopulateWithData()
        {
            // Make datasource
            Datasource = new CollectionViewDataSource(EmployeeCollection);

            // Build list of employees
            Datasource.Data.Add(new PersonModel("Craig Dunn", "Documentation Manager", true));
            Datasource.Data.Add(new PersonModel("Amy Burns", "Technical Writer"));
            Datasource.Data.Add(new PersonModel("Joel Martinez", "Web & Infrastructure"));
            Datasource.Data.Add(new PersonModel("Kevin Mullins", "Technical Writer"));
            Datasource.Data.Add(new PersonModel("Mark McLemore", "Technical Writer"));
            Datasource.Data.Add(new PersonModel("Tom Opgenorth", "Technical Writer"));
            Datasource.Data.Add(new PersonModel("Larry O'Brien", "API Docs Manager", true));
            Datasource.Data.Add(new PersonModel("Mike Norman", "API Documentor"));

            // Populate collection view
            EmployeeCollection.ReloadData();
        }
        #endregion

        #region Events
        /// <summary>
        /// Selection changed delegate.
        /// </summary>
        public delegate void SelectionChangedDelegate();

        /// <summary>
        /// Occurs when selection changed.
        /// </summary>
        public event SelectionChangedDelegate SelectionChanged;

        /// <summary>
        /// Raises the selection changed event.
        /// </summary>
        internal void RaiseSelectionChanged() {
            // Inform caller
            if (this.SelectionChanged != null) SelectionChanged();
        }
        #endregion
    }
}
```

Pořízení podrobně, podívejte se na tento kód `Datasource` vlastnost definovaná pro uložení instance `CollectionViewDataSource` které zajistí data pro zobrazení kolekce. A `PersonSelected` vlastnost definovaná pro uložení `PersonModel` představující aktuálně vybrané položky v kolekci zobrazení. Tato vlastnost také vyvolá `SelectionChanged` událost v případě změny výběru.

`ConfigureCollectionView` Třída se používá k registraci řadiče zobrazení, který funguje jako prototyp buňky s kolekce zobrazení pomocí následující řádek:

```csharp
EmployeeCollection.RegisterClassForItem(typeof(EmployeeItemController), "EmployeeCell");
```

Všimněte si, že **identifikátor** (`EmployeeCell`) používá k registraci na prototypu odpovídá byl volán v `GetItem` metodu `CollectionViewDataSource` definované výše:

```csharp
var item = collectionView.MakeItem("EmployeeCell", indexPath) as EmployeeItemController;
...
```

Kromě toho typ řadiče zobrazení **musí** shodovat s názvem `.xib` soubor, který definuje prototyp **přesně**. V případě tohoto příkladu `EmployeeItemController` a `EmployeeItemController.xib`.

Skutečné rozložení položek v kolekci zobrazení řídí třídu kolekce zobrazení rozložení a může změnit dynamicky za běhu přiřazení nové instance do `CollectionViewLayout` vlastnost. Změna této vlastnosti aktualizuje vzhled zobrazení kolekce bez animace změnu.

Apple dodává dva typy předdefinované rozložení s zobrazení kolekce, který bude zpracovávat nejobvyklejším používá: `NSCollectionViewFlowLayout` a `NSCollectionViewGridLayout`. Vývojář podle potřeby vlastní formát, například položky odhlašování v kruh, kterým se uživatel může vytvořit vlastní instanci `NSCollectionViewLayout` a přepsat požadované metody pro dosažení požadovaného účinku.

Tento příklad používá výchozí rozložení toku, vytvoří instanci `NSCollectionViewFlowLayout` třídy a nakonfiguruje následovně:

```csharp
var flowLayout = new NSCollectionViewFlowLayout()
{
    ItemSize = new CGSize(150, 150),
    SectionInset = new NSEdgeInsets(10, 10, 10, 20),
    MinimumInteritemSpacing = 10,
    MinimumLineSpacing = 10
};
```

`ItemSize` Vlastnost definuje velikost každé jednotlivé buňky v kolekci. `SectionInset` Vlastnost definuje vsazení od levého okraje buňky nastíněny v kolekce. `MinimumInteritemSpacing` definuje minimální mezera mezi položkami a `MinimumLineSpacing` definuje minimální mezery mezi řádky v kolekci.

Rozložení je přiřazena k zobrazení kolekce a instance `CollectionViewDelegate` je připojen k zpracování výběr položek:

```csharp
// Setup collection view
EmployeeCollection.CollectionViewLayout = flowLayout;
EmployeeCollection.Delegate = new CollectionViewDelegate(this);
```

`PopulateWithData` Metoda vytvoří novou instanci třídy `CollectionViewDataSource`, naplní s daty, připojí k zobrazení kolekce a volání `ReloadData` metodu pro zobrazení položek:

```csharp
private void PopulateWithData()
{
    // Make datasource
    Datasource = new CollectionViewDataSource(EmployeeCollection);

    // Build list of employees
    Datasource.Data.Add(new PersonModel("Craig Dunn", "Documentation Manager", true));
    ...

    // Populate collection view
    EmployeeCollection.ReloadData();
}
```

`ViewDidLoad` Metoda přepsána a volání `ConfigureCollectionView` a `PopulateWithData` způsoby, jak zobrazit poslední zobrazení kolekce pro uživatele:

```csharp
public override void ViewDidLoad()
{
    base.ViewDidLoad();

    // Initialize Collection View
    ConfigureCollectionView();
    PopulateWithData();
}
```

<a name="Summary"/>

## <a name="summary"></a>Souhrn

Tento článek podrobně prohlédnout práce se zobrazeními kolekce v aplikaci Xamarin.Mac trvá. Nejprve hledá v vystavení třída C# pro Objective-C pomocí kódování klíč-hodnota (KVC) a sledování klíč-hodnota (KVO). V dalším kroku se vám ukázal, jak používat třídu kompatibilní KVO a navázat jej Data zobrazení kolekcí v Xcode na rozhraní tvůrce. Nakonec se vám ukázal, jak pracovat s zobrazení kolekcí v kódu jazyka C#.

## <a name="related-links"></a>Související odkazy

- [MacCollectionNew (ukázka)](https://developer.xamarin.com/samples/mac/MacCollectionNew/)
- [Hello, Mac](~/mac/get-started/hello-mac.md)
- [Vazba dat a kódování párů klíč-hodnota](~/mac/app-fundamentals/databinding.md)
- [NSCollectionView](https://developer.apple.com/reference/appkit/nscollectionview)
- [Pokyny pro rozhraní lidské OS X](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/)
