---
title: "Práce se zobrazeními tabulky"
description: "Tento článek se zabývá navrhování a práce s řadiče zobrazení tabulky a zobrazení tabulek uvnitř Xamarin.tvOS aplikace."
ms.topic: article
ms.prod: xamarin
ms.assetid: D8F80FA9-6400-4DB7-AFC9-A28A54AD04E8
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/16/2017
ms.openlocfilehash: 26a73c2536bf4b4959928bfb27958b1a10734bf5
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 02/27/2018
---
# <a name="working-with-table-views"></a>Práce se zobrazeními tabulky

_Tento článek se zabývá navrhování a práce s řadiče zobrazení tabulky a zobrazení tabulek uvnitř Xamarin.tvOS aplikace._

V tvOS zobrazí se jako jeden sloupec posouvání řádků, které lze volitelně můžete uspořádat do skupin nebo části zobrazení tabulky. Pokud je třeba zobrazit velké množství dat efektivně se uživateli na clear pochopit způsob, jak je třeba použít zobrazení tabulek.

Zobrazení tabulek se obvykle zobrazují v jedné straně [rozděleným zobrazením](~/ios/tvos/user-interface/split-views.md) jako navigační podrobné informace o této položce vybrané v opačné straně zobrazí:

[ ![](table-views-images/intro01.png "Ukázková tabulka zobrazení")](table-views-images/intro01.png)

<a name="About-Table-Views" />

## <a name="about-table-views"></a>O zobrazení tabulek

A `UITableView` zobrazí jeden sloupec posouvatelného řádků jako seznam hierarchické informace, které lze volitelně můžete uspořádat do skupin nebo částech: 

[ ![](table-views-images/table01.png "Vybranou položku")](table-views-images/table01.png)

Společnost Apple má následující návrhy pro práci s tabulkami:

- **Být šířka vědět o** -pokuste se najít správnou rovnováhu mezi u vaší šířek tabulky. Pokud je tabulka příliš široké, může být obtížné naskenovat z vzdálenosti a může trvat mimo oblast obsahu k dispozici. Pokud je tabulka příliš úzké, může to způsobit informace k oříznutí nebo wrap, znovu to může být pro uživatele ke čtení z v místnosti.
- **Zobrazit tabulku obsah rychle** – pro velké seznamy dat opožděné zatížení obsah a začne zobrazovat informace také v tabulce se zobrazí uživateli. Pokud v tabulce trvá dlouho na zatížení, uživatel může dojít ke ztrátě zájem o aplikaci nebo zvažte, které je zamčený nahoru.
- **Informujte uživatele o dlouho obsahu zatížení** – Pokud čas načítání dlouhé tabulky je nevyhnutelné použít, existuje [indikátor průběhu nebo ukazatel aktivity](~/ios/tvos/user-interface/progress-indicators.md) , aby věděli aplikace není uzamčen.

<a name="Table-Cell-Types" />

## <a name="table-view-cell-types"></a>Typy zobrazení buněk tabulky

A `UITableViewCell` se používá k reprezentování jednotlivé řádky dat v tabulce zobrazení. Apple definovala několik typů buněk tabulky výchozí:

- **Výchozí** – tento typ uvede možnost bitové kopie na levé straně buňky a Title vlevo zarovnaný na pravé straně. 
- **Subtitle** – tento typ uvede název vlevo zarovnaný na první řádek a menší zarovnaný doleva Subtitle na následujícím řádku.
- **Hodnota 1** – tento typ uvede název zarovnaný doleva s Subtitle zapalovače barevnou, vpravo zarovnaný na stejném řádku.
- **Hodnota 2** – tento typ uvede název zarovnaný doprava s Subtitle zapalovače barevnou, vlevo zarovnaný na stejném řádku.

Všechny typy zobrazení buněk tabulky výchozí také podporují grafické prvky, jako jsou například indikátory zpřístupnění nebo značky zaškrtnutí. 

Kromě toho můžete definovat **vlastní** zobrazení buněk tabulky a přítomné _prototypu buňky_, buď vytvořené v Návrháři rozhraní nebo prostřednictvím kódu.

Společnost Apple má následující návrhy pro práci s buněk tabulky zobrazení:

- **Vyhněte se Text výstřižek** -zachovat jednotlivé řádky textu krátké tak, že nemáte skončili oříznuta. Zkrácený slova nebo fráze se obtížně pro uživatele analyzovat z v místnosti.
- **Vezměte v úvahu stavu řádek Focused** – kvůli tomu řádek větší s více zaokrouhlené rozích v fokus, je potřeba provést testování vaší buňky vzhled v všechny stavy. Bitové kopie nebo text může být oříznut nebo si ve stavu Focused nesprávné.
- **Používejte opatrně upravitelné tabulky** -přesunutí nebo odstranění řádků v tabulce je více časově náročné na tvOS než iOS. Je třeba pečlivě rozhodnout, pokud bude tato funkce Přidat nebo rušil tvOS aplikace.
- **Vytvořit vlastní buňky typy kterém příslušné** – vestavěné typy buňku zobrazení tabulky jsou skvěle hodí k mnoha situacích, zvažte vytvoření vlastní typy buněk nestandardní informace poskytují větší kontrolu a lépe prezentovat informace uživatel.

<a name="Working-With-Table-Views" />

## <a name="working-with-table-views"></a>Práce se zobrazeními tabulky

Nejjednodušší způsob, jak pracovat s zobrazení tabulek v aplikaci Xamarin.tvOS je vytvořit a upravit jejich vzhled v Návrháři rozhraní.

Abyste mohli začít, postupujte takto:

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)
    
1. V sadě Visual Studio pro Mac, vytvořte nový projekt aplikace tvOS a vyberte **tvOS** > **aplikace** > **jediné zobrazení aplikace** a klikněte na  **Další** tlačítko: 

    [ ![](table-views-images/table02.png "Vyberte aplikaci, jednoho zobrazení")](table-views-images/table02.png)
1. Zadejte **název** pro aplikaci a klikněte na tlačítko **Další**: 

    [ ![](table-views-images/table03.png "Zadejte název aplikace")](table-views-images/table03.png)
1. Buď upravit **název projektu** a **název řešení** nebo přijměte výchozí hodnoty a klikněte na **vytvořit** tlačítko vytvořte nové řešení: 

    [ ![](table-views-images/table04.png "Název projektu a název řešení")](table-views-images/table04.png)
1. V **Pad řešení**, dvakrát klikněte `Main.storyboard` soubor otevřete v iOS Designer: 

    [ ![](table-views-images/table05.png "Soubor Main.storyboard")](table-views-images/table05.png)
1. Vyberte a odstraňte **výchozí View Controller**: 

    [ ![](table-views-images/table06.png "Vyberte a odstraňte řadiče výchozí zobrazení")](table-views-images/table06.png)
1. Vyberte **rozdělení View Controller** z **sada nástrojů** a přetáhněte ji na návrhovou plochu.
1. Ve výchozím nastavení, získáte [rozděleným zobrazením](~/ios/tvos/user-interface/split-views.md) s **navigační View Controller** a **řadiče zobrazení tabulky** v na levé straně a **View Controller** v pravé straně. Toto je navržené využití zobrazení tabulky v tvOS společnosti Apple: 

    [ ![](table-views-images/table08.png "Přidání zobrazení rozdělení")](table-views-images/table08.png)
1. Budete muset vybrat každou část Tabulka zobrazení a přiřaďte ho vlastní **název třídy** v **pomůcky** kartě **Explorer vlastnosti** , aby přístup později v jazyce C# kód. Například **řadiče zobrazení tabulky**: 

    [ ![](table-views-images/table09.png "Přiřaďte název třídy")](table-views-images/table09.png)
1. Ujistěte se, že vytvoříte vlastní třída pro **řadiče zobrazení tabulky**, **zobrazení tabulky** a jakýkoli **prototypu buněk**. Visual Studio pro Mac přidá vlastní třídy do stromu projektu při jejich vytvoření: 

    [ ![](table-views-images/table10.png "Vlastní třídy ve stromu projektu")](table-views-images/table10.png)
1. V dalším kroku vyberte zobrazení tabulky v návrhové plochy a podle potřeby upravte její vlastnosti. Například počet **prototypu buněk** a **styl** (prostý nebo seskupené): 

    [ ![](table-views-images/table11.png "Na kartě pomůcky")](table-views-images/table11.png)
1. Pro každou **prototypu buňky**, vyberte ho a přiřadit jedinečný **identifikátor** v **pomůcky** kartě **Explorer vlastnosti**. Tento krok je _velmi důležité_ jako budete později potřebovat tento identifikátor při naplňování tabulky. Například `AttrCell`: 

    [ ![](table-views-images/table12.png "Na kartě pomůcky")](table-views-images/table12.png)
1. Můžete také vybrat nabídne buňky jako jeden z [výchozí typy buněk tabulky zobrazení](#Table-View-Cell-Types) prostřednictvím **styl** rozevírací nebo ji nastavte na **vlastní** a použít na návrhovou plochu na rozložení buňky Přetažením v jiných pomůcky uživatelského rozhraní z **sada nástrojů**: 

    [ ![](table-views-images/table13.png "Rozložení buněk")](table-views-images/table13.png)
1. Přiřadit jedinečný **název** na každý element uživatelského rozhraní v návrhu prototypu buňky ve **pomůcky** kartě **vlastnosti Explorer** tak k dispozici novější v kódu jazyka C#: 

    [ ![](table-views-images/table14.png "Přiřadit název")](table-views-images/table14.png)
1. Opakujte předchozí krok pro všechny prototypu buněk v tabulce zobrazení.
1. V dalším kroku přiřadit vlastní třídy s ostatními návrh uživatelského rozhraní, rozložení zobrazení podrobností a přiřazení jedinečných **názvy** pro každý prvek uživatelského rozhraní v podrobnostech zobrazení tak, aby se dostanete v jazyce C# také. Příklad: 

    [ ![](table-views-images/table15.png "Rozložení uživatelského rozhraní")](table-views-images/table15.png)
1. Uložte změny do scénáře.
    
# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)
    
1. V sadě Visual Studio spusťte nový projekt aplikace tvOS a vyberte **tvOS** > **jediné zobrazení aplikace** a zadejte název pro vaši aplikaci. Klikněte **nevadí** tlačítko vytvořte nové řešení: 

    [ ![](table-views-images/table02-vs.png "Vyberte aplikaci, jednoho zobrazení")](table-views-images/table02-vs.png)
1. V **Průzkumníku řešení**, dvakrát klikněte `Main.storyboard` soubor otevřete v iOS Designer: 

    [ ![](table-views-images/table05-vs.png "Soubor Main.storyboard")](table-views-images/table05-vs.png)
1. Vyberte a odstraňte **výchozí View Controller**: 

    [ ![](table-views-images/table06-vs.png "Vyberte a odstraňte řadiče výchozí zobrazení")](table-views-images/table06-vs.png)
1. Vyberte **rozdělení View Controller** z **sada nástrojů** a přetáhněte ji na návrhovou plochu: 

    [ ![](table-views-images/table07-vs.png "Řadič zobrazení rozdělení")](table-views-images/table07-vs.png)
1. Ve výchozím nastavení, získáte [rozděleným zobrazením](~/ios/tvos/user-interface/split-views.md) s **navigační View Controller** a **řadiče zobrazení tabulky** v na levé straně a **View Controller** v pravé straně. Toto je navržené využití zobrazení tabulky v tvOS společnosti Apple: 

    [ ![](table-views-images/table08-vs.png "Rozložení uživatelského rozhraní")](table-views-images/table08-vs.png)
1. Budete muset vybrat každou část Tabulka zobrazení a přiřaďte ho vlastní **název třídy** v **pomůcky** kartě **Explorer vlastnosti** , aby přístup později v jazyce C# kód. Například **řadiče zobrazení tabulky**: 

    [ ![](table-views-images/table09-vs.png "Na kartě pomůcky")](table-views-images/table09-vs.png)
1. Ujistěte se, že vytvoříte vlastní třída pro **řadiče zobrazení tabulky**, **zobrazení tabulky** a jakýkoli **prototypu buněk**. Visual Studio pro Mac přidá vlastní třídy do stromu projektu při jejich vytvoření: 

    [ ![](table-views-images/table10-vs.png "Vlastní třídy ve stromu projektu")](table-views-images/table10-vs.png)
1. V dalším kroku vyberte zobrazení tabulky v návrhové plochy a podle potřeby upravte její vlastnosti. Například počet **prototypu buněk** a **styl** (prostý nebo seskupené): 

    [ ![](table-views-images/table11-vs.png "Na kartě pomůcky")](table-views-images/table11-vs.png)
1. Pro každou **prototypu buňky**, vyberte ho a přiřadit jedinečný **identifikátor** v **pomůcky** kartě **Explorer vlastnosti**. Tento krok je _velmi důležité_ jako budete později potřebovat tento identifikátor při naplňování tabulky. Například `AttrCell`: 

    [ ![](table-views-images/table12-vs.png "Přiřadit identifikátor")](table-views-images/table12-vs.png)
1. Můžete také vybrat nabídne buňky jako jeden z [výchozí typy buněk tabulky zobrazení](#Table-View-Cell-Types) prostřednictvím **styl** rozevírací nebo ji nastavte na **vlastní** a použít na návrhovou plochu na rozložení buňky Přetažením v jiných pomůcky uživatelského rozhraní z **sada nástrojů**: 

    [ ![](table-views-images/table13-vs.png "Styl rozevíracího seznamu")](table-views-images/table13-vs.png)
1. Přiřadit jedinečný **název** na každý element uživatelského rozhraní v návrhu prototypu buňky ve **pomůcky** kartě **vlastnosti Explorer** tak k dispozici novější v kódu jazyka C#: 

    [ ![](table-views-images/table14-vs.png "Na kartě pomůcky")](table-views-images/table14-vs.png)
1. Opakujte předchozí krok pro všechny prototypu buněk v tabulce zobrazení.
1. V dalším kroku přiřadit vlastní třídy s ostatními návrh uživatelského rozhraní, rozložení zobrazení podrobností a přiřazení jedinečných **názvy** pro každý prvek uživatelského rozhraní v podrobnostech zobrazení tak, aby se dostanete v jazyce C# také. Příklad: 

    [ ![](table-views-images/table15.png "Rozložení uživatelského rozhraní")](table-views-images/table15.png)
1. Uložte změny do scénáře.
    
-----

<a name="Designing-a-Data-Model" />

## <a name="designing-a-data-model"></a>Navržení modelu dat

Pro usnadnění práce s informacemi, které tabulka zobrazení zobrazí snadnější a k usnadnění nabízí podrobné informace (jako uživatel vybere nebo zvýrazní řádky v tabulce zobrazení), vytvořte vlastní třídu nebo třídy tak, aby fungoval jako datový Model pro informace uvedené .

Proveďte v příkladu aplikaci rezervace cesta, která obsahuje seznam **města**, každý obsahující jedinečný seznam **atrakce** , můžete vybrat uživatele. Uživatel bude moct označit přitažlivosti jako *Oblíbené*, vyberte, chcete-li získat *pokynů* k přitažlivosti a *sešit letu* k dané bydlišti.

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

K vytvoření datového modelu pro **přitažlivosti**, klikněte pravým tlačítkem na název projektu v **řešení Pad** a vyberte **přidat** > **nový soubor...** . Zadejte `AttractionInformation` pro **název** a klikněte na **nový** tlačítko: 

[ ![](table-views-images/data01.png "Zadejte pro název AttractionInformation")](table-views-images/data01.png)

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

K vytvoření datového modelu pro **přitažlivosti**, klikněte pravým tlačítkem na název projektu v **Průzkumníku řešení** a vyberte **přidat** > **novou položku ...** . Vyberte **třída** a zadejte `AttractionInformation` pro **název** a klikněte na tlačítko **přidat** tlačítko: 

[ ![](table-views-images/data01-vs.png "Vyberte třídu a jako název zadejte AttractionInformation")](table-views-images/data01-vs.png)

-----

Upravit `AttractionInformation.cs` souboru a nastavit jej vypadat třeba takto:

```csharp
using System;
using Foundation;

namespace tvTable
{
    public class AttractionInformation : NSObject
    {
        #region Computed Properties
        public CityInformation City { get; set;}
        public string Name { get; set;}
        public string Description { get; set;}
        public string ImageName { get; set;}
        public bool IsFavorite { get; set;}
        public bool AddDirections { get; set;}
        #endregion

        #region Constructors
        public AttractionInformation (string name, string description, string imageName)
        {
            // Initialize
            this.Name = name;
            this.Description = description;
            this.ImageName = imageName;
        }
        #endregion
    }
}
```

Tato třída poskytuje vlastností pro ukládání informací o danou **přitažlivosti**.

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

Pak klikněte pravým tlačítkem na název projektu v **řešení Pad** znovu a vyberte **přidat** > **nový soubor...** . Zadejte `CityInformation` pro **název** a klikněte na **nový** tlačítko: 

[ ![](table-views-images/data02.png "Zadejte pro název CityInformation")](table-views-images/data02.png)

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Pak klikněte pravým tlačítkem na název projektu v **Průzkumníku řešení** znovu a vyberte **přidat** > **novou položku...** . Zadejte `CityInformation` pro **název** a klikněte na **přidat** tlačítko: 

[ ![](table-views-images/data02-vs.png "Zadejte pro název CityInformation")](table-views-images/data02-vs.png)

-----

Upravit `CityInformation.cs` souboru a nastavit jej vypadat třeba takto:

```csharp
using System;
using System.Collections.Generic;
using Foundation;

namespace tvTable
{
    public class CityInformation : NSObject
    {
        #region Computed Properties
        public string Name { get; set; }
        public List<AttractionInformation> Attractions { get; set;}
        public bool FlightBooked { get; set;}
        #endregion

        #region Constructors
        public CityInformation (string name)
        {
            // Initialize
            this.Name = name;
            this.Attractions = new List<AttractionInformation> ();
        }
        #endregion

        #region Public Methods
        public void AddAttraction (AttractionInformation attraction)
        {
            // Mark as belonging to this city
            attraction.City = this;

            // Add to collection
            Attractions.Add (attraction);
        }

        public void AddAttraction (string name, string description, string imageName)
        {
            // Create attraction
            var attraction = new AttractionInformation (name, description, imageName);

            // Mark as belonging to this city
            attraction.City = this;

            // Add to collection
            Attractions.Add (attraction);
        }
        #endregion
    }
}
```

Tato třída obsahuje všechny informace o cíl **města**, kolekce **atrakce** tohoto města a nabízí dvě metody helper (`AddAttraction`) aby bylo snazší přidat atrakce k města.

<a name="The-Table-Data-Source" />

## <a name="the-table-view-data-source"></a>Zdroj dat zobrazení tabulky

Každé zobrazení tabulky vyžaduje zdroje dat (`UITableViewDataSource`) k poskytování dat pro tabulku a generovat potřebné řádky jako vyžadovanou tabulka zobrazení.

Například výše uvedené, klikněte pravým tlačítkem na název projektu v **Průzkumníku řešení**, vyberte **přidat** > **nový soubor...**  a pojmenujte ji `AttractionTableDatasource` a klikněte na tlačítko **nový** tlačítko vytvořit. Potom upravte `AttractionTableDatasource.cs` souboru a nastavit jej vypadat třeba takto:

```csharp
using System;
using System.Collections.Generic;
using UIKit;

namespace tvTable
{
    public class AttractionTableDatasource : UITableViewDataSource
    {
        #region Constants
        const string CellID = "AttrCell";
        #endregion

        #region Computed Properties
        public AttractionTableViewController Controller { get; set;}
        public List<CityInformation> Cities { get; set;}
        #endregion

        #region Constructors
        public AttractionTableDatasource (AttractionTableViewController controller)
        {
            // Initialize
            this.Controller = controller;
            this.Cities = new List<CityInformation> ();
            PopulateCities ();
        }
        #endregion

        #region Public Methods
        public void PopulateCities ()
        {
            // Clear existing
            Cities.Clear ();

            // Define cities and attractions
            var Paris = new CityInformation ("Paris");
            Paris.AddAttraction ("Eiffel Tower", "Is a wrought iron lattice tower on the Champ de Mars in Paris, France.", "EiffelTower");
            Paris.AddAttraction ("Musée du Louvre", "is one of the world's largest museums and a historic monument in Paris, France.", "Louvre");
            Paris.AddAttraction ("Moulin Rouge", "French for 'Red Mill', is a cabaret in Paris, France.", "MoulinRouge");
            Paris.AddAttraction ("La Seine", "Is a 777-kilometre long river and an important commercial waterway within the Paris Basin.", "RiverSeine");
            Cities.Add (Paris);

            var SanFran = new CityInformation ("San Francisco");
            SanFran.AddAttraction ("Alcatraz Island", "Is located in the San Francisco Bay, 1.25 miles (2.01 km) offshore from San Francisco.", "Alcatraz");
            SanFran.AddAttraction ("Golden Gate Bridge", "Is a suspension bridge spanning the Golden Gate strait between San Francisco Bay and the Pacific Ocean", "GoldenGateBridge");
            SanFran.AddAttraction ("San Francisco", "Is the cultural, commercial, and financial center of Northern California.", "SanFrancisco");
            SanFran.AddAttraction ("Telegraph Hill", "Is primarily a residential area, much quieter than adjoining North Beach.", "TelegraphHill");
            Cities.Add (SanFran);

            var Houston = new CityInformation ("Houston");
            Houston.AddAttraction ("City Hall", "It was constructed in 1938-1939, and is located in Downtown Houston.", "CityHall");
            Houston.AddAttraction ("Houston", "Is the most populous city in Texas and the fourth most populous city in the US.", "Houston");
            Houston.AddAttraction ("Texas Longhorn", "Is a breed of cattle known for its characteristic horns, which can extend to over 6 ft.", "LonghornCattle");
            Houston.AddAttraction ("Saturn V Rocket", "was an American human-rated expendable rocket used by NASA between 1966 and 1973.", "Rocket");
            Cities.Add (Houston);
        }
        #endregion

        #region Override Methods
        public override UITableViewCell GetCell (UITableView tableView, Foundation.NSIndexPath indexPath)
        {
            // Get cell
            var cell = tableView.DequeueReusableCell (CellID) as AttractionTableCell;

            // Populate cell
            cell.Attraction = Cities [indexPath.Section].Attractions [indexPath.Row];

            // Return new cell
            return cell;
        }

        public override nint NumberOfSections (UITableView tableView)
        {
            // Return number of cities
            return Cities.Count;
        }

        public override nint RowsInSection (UITableView tableView, nint section)
        {
            // Return the number of attractions in the given city
            return Cities [(int)section].Attractions.Count;
        }

        public override string TitleForHeader (UITableView tableView, nint section)
        {
            // Get the name of the current city
            return Cities [(int)section].Name;
        }
        #endregion
    }
}
```

Podívejme se na několik částí třídy podrobně.

Nejdřív jsme definované konstanty pro uložení jedinečný identifikátor prototypu buňky (Toto je stejný identifikátor přiřazený v Návrháři rozhraní výše), přidat zástupce zpět do řadiče zobrazení tabulky a vytvořili úložiště pro naše data:

```csharp
const string CellID = "AttrCell";
public AttractionTableViewController Controller { get; set;}
public List<CityInformation> Cities { get; set;}
```

V dalším kroku jsme uložit řadiče zobrazení tabulky, pak vytvořit a naplnit naše zdroje dat (s použitím modelů dat definované výše) při vytvoření třídy:

```csharp
public AttractionTableDatasource (AttractionTableViewController controller)
{
    // Initialize
    this.Controller = controller;
    this.Cities = new List<CityInformation> ();
    PopulateCities ();
}
```

Z důvodu příkladu `PopulateCities` metoda jednoduše vytvoří objekty datového modelu v paměti, ale tyto bylo možné snadno načíst z databáze nebo webové služby v reálné aplikaci:

```csharp
public void PopulateCities ()
{
    // Clear existing
    Cities.Clear ();

    // Define cities and attractions
    var Paris = new CityInformation ("Paris");
    Paris.AddAttraction ("Eiffel Tower", "Is a wrought iron lattice tower on the Champ de Mars in Paris, France.", "EiffelTower");
    ...
}
```

`NumberOfSections` Metoda vrátí počet oddílů v tabulce:

```csharp
public override nint NumberOfSections (UITableView tableView)
{
    // Return number of cities
    return Cities.Count;
}
```

Pro **prostý** ve zobrazení tabulek, vždy vrátí 1.

`RowsInSection` Metoda vrátí počet řádků v aktuálním oddílu:

```csharp
public override nint RowsInSection (UITableView tableView, nint section)
{
    // Return the number of attractions in the given city
    return Cities [(int)section].Attractions.Count;
}
```

Znovu, **prostý** zobrazení tabulek, vrátí celkový počet položek v datovém zdroji.

`TitleForHeader` Metoda vrátí název pro danou část:

```csharp
public override string TitleForHeader (UITableView tableView, nint section)
{
    // Get the name of the current city
    return Cities [(int)section].Name;
}
```

Pro **prostý** zobrazení tabulky zadejte, ponechejte pole název prázdné (`""`).

Nakonec v případě požadavku tabulka zobrazení, vytvořit a naplnit prototypu buňky pomocí `GetCell` metoda: 

```csharp
public override UITableViewCell GetCell (UITableView tableView, Foundation.NSIndexPath indexPath)
{
    // Get cell
    var cell = tableView.DequeueReusableCell (CellID) as AttractionTableCell;

    // Populate cell
    cell.Attraction = Cities [indexPath.Section].Attractions [indexPath.Row];

    // Return new cell
    return cell;
}
```

Další informace o práci s `UITableViewDatasource`, najdete v tématu společnosti Apple [UITableViewDatasource](https://developer.apple.com/library/prerelease/tvos/documentation/UIKit/Reference/UITableViewDataSource_Protocol/index.html#//apple_ref/doc/uid/TP40006941) dokumentaci.

<a name="The-Table-View-Delegate" />

## <a name="the-table-view-delegate"></a>Delegát zobrazení tabulky

Každé zobrazení tabulky vyžaduje delegáta (`UITableViewDelegate`) reagovat na interakci s uživatelem nebo jiné systémové události v tabulce.

Například výše uvedené, klikněte pravým tlačítkem na název projektu v **Průzkumníku řešení**, vyberte **přidat** > **nový soubor...**  a pojmenujte ji `AttractionTableDelegate` a klikněte na tlačítko **nový** tlačítko vytvořit. Potom upravte `AttractionTableDelegate.cs` souboru a nastavit jej vypadat třeba takto:

```csharp
using System;
using System.Collections.Generic;
using UIKit;

namespace tvTable
{
    public class AttractionTableDelegate : UITableViewDelegate
    {
        #region Computed Properties
        public AttractionTableViewController Controller { get; set;}
        #endregion

        #region Constructors
        public AttractionTableDelegate (AttractionTableViewController controller)
        {
            // Initializw
            this.Controller = controller;
        }
        #endregion

        #region Override Methods
        public override void RowSelected (UITableView tableView, Foundation.NSIndexPath indexPath)
        {
            var attraction = Controller.Datasource.Cities [indexPath.Section].Attractions [indexPath.Row];
            attraction.IsFavorite = (!attraction.IsFavorite);

            // Update UI
            Controller.TableView.ReloadData ();
        }

        public override bool CanFocusRow (UITableView tableView, Foundation.NSIndexPath indexPath)
        {
            // Inform caller of highlight change
            RaiseAttractionHighlighted (Controller.Datasource.Cities [indexPath.Section].Attractions [indexPath.Row]);
            return true;
        }
        #endregion

        #region Events
        public delegate void AttractionHighlightedDelegate (AttractionInformation attraction);
        public event AttractionHighlightedDelegate AttractionHighlighted;

        internal void RaiseAttractionHighlighted (AttractionInformation attraction)
        {
            // Inform caller
            if (this.AttractionHighlighted != null) this.AttractionHighlighted (attraction);
        }
        #endregion
    }
}
```

Podívejme se na několika oddílů této třídy v podrobnostech.

Nejdříve vytvoříme zástupce řadiče zobrazení tabulky při vytvoření třídy:

```csharp
public AttractionTableViewController Controller { get; set;}
...

public AttractionTableDelegate (AttractionTableViewController controller)
{
    // Initialize
    this.Controller = controller;
}
```

Pak, když je vybrán řádek (uživatel klikne na Touch povrchu vzdálené Apple) chceme označit **přitažlivosti** reprezentována jako oblíbené vybraného řádku:

```csharp
public override void RowSelected (UITableView tableView, Foundation.NSIndexPath indexPath)
{
    var attraction = Controller.Datasource.Cities [indexPath.Section].Attractions [indexPath.Row];
    attraction.IsFavorite = (!attraction.IsFavorite);

    // Update UI
    Controller.TableView.ReloadData ();
}
```

Další, když uživatel označuje řádek (tím, že se pomocí Apple vzdálené Touch povrchu) chceme podrobnosti k dispozici **přitažlivosti** reprezentována tento řádek v části Podrobnosti o našem rozdělení řadiče zobrazení:

```csharp
public override bool CanFocusRow (UITableView tableView, Foundation.NSIndexPath indexPath)
{
    // Inform caller of highlight change
    RaiseAttractionHighlighted (Controller.Datasource.Cities [indexPath.Section].Attractions [indexPath.Row]);
    return true;
}
...

public delegate void AttractionHighlightedDelegate (AttractionInformation attraction);
public event AttractionHighlightedDelegate AttractionHighlighted;

internal void RaiseAttractionHighlighted (AttractionInformation attraction)
{
    // Inform caller
    if (this.AttractionHighlighted != null) this.AttractionHighlighted (attraction);
}
```

`CanFocusRow` Metoda je volána pro každý řádek, který má získat fokus v tabulkovém zobrazení. Vrátí `true` Pokud řádek můžete získat fokus, jinak vrátí `false`. V případě tohoto příkladu jsme vytvořili vlastní `AttractionHighlighted` událost, která bude vyvolána na každém řádku při aktivaci.

Další informace o práci s `UITableViewDelegate`, najdete v tématu společnosti Apple [UITableViewDelegate](https://developer.apple.com/library/prerelease/tvos/documentation/UIKit/Reference/UITableViewDelegate_Protocol/index.html#//apple_ref/doc/uid/TP40006942) dokumentaci.

<a name="The-Table-View-Cell" />

## <a name="the-table-view-cell"></a>Na buňku zobrazení tabulky.

Pro každý prototypu buňku, která jste přidali do tabulky zobrazení v Návrháři rozhraní, také vytvořit vlastní instanci buňku zobrazení tabulky (`UITableViewCell`) a umožní vám k naplnění nové buňky (řádek), jako je vytvořen.

Příklad aplikace, dvakrát klikněte `AttractionTableCell.cs` soubor otevřete pro úpravy a jeho vypadat třeba takto:

```csharp
using System;
using Foundation;
using UIKit;

namespace tvTable
{
    public partial class AttractionTableCell : UITableViewCell
    {
        #region Private Variables
        private AttractionInformation _attraction = null;
        #endregion

        #region Computed Properties
        public AttractionInformation Attraction {
            get { return _attraction; }
            set {
                _attraction = value;
                UpdateUI ();
            }
        }
        #endregion

        #region Constructors
        public AttractionTableCell (IntPtr handle) : base (handle)
        {
        }
        #endregion

        #region Private Methods
        private void UpdateUI ()
        {
            // Trap all errors
            try {
                Title.Text = Attraction.Name;
                Favorite.Hidden = (!Attraction.IsFavorite);
            } catch {
                // Since the UI might not be fully loaded, ignore
                // all errors at this point
            }
        }
        #endregion
    }
}
```

Tato třída poskytuje úložiště pro objekt přitažlivosti datový Model (`AttractionInformation` definovaný výše) zobrazí v daném řádku:

```csharp
private AttractionInformation _attraction = null;
...

public AttractionInformation Attraction {
    get { return _attraction; }
    set {
        _attraction = value;
        UpdateUI ();
    }
}
```

`UpdateUI` Metoda naplní pomůcky uživatelského rozhraní (které byly přidány do buňky prototypu v Návrháři rozhraní) podle potřeby:

```csharp
private void UpdateUI ()
{
    // Trap all errors
    try {
        Title.Text = Attraction.Name;
        Favorite.Hidden = (!Attraction.IsFavorite);
    } catch {
        // Since the UI might not be fully loaded, ignore
        // all errors at this point
    }
}
```

Další informace o práci s `UITableViewCell`, najdete v tématu společnosti Apple [UITableViewCell](https://developer.apple.com/library/prerelease/tvos/documentation/UIKit/Reference/UITableViewCell_Class/index.html#//apple_ref/doc/uid/TP40006938) dokumentaci.

<a name="The-Table-View-Controller" />

## <a name="the-table-view-controller"></a>Řadiče zobrazení tabulky

Řadič zobrazení tabulky (`UITableViewController`) spravuje zobrazení tabulky, který byl přidán do scénáře přes rozhraní návrháře.

Příklad aplikace, dvakrát klikněte `AttractionTableViewController.cs` soubor otevřete pro úpravy a jeho vypadat třeba takto:

```csharp
using System;
using Foundation;
using UIKit;

namespace tvTable
{
    public partial class AttractionTableViewController : UITableViewController
    {
        #region Computed Properties
        public AttractionTableDatasource Datasource {
            get { return TableView.DataSource as AttractionTableDatasource; }
        }

        public AttractionTableDelegate TableDelegate {
            get { return TableView.Delegate as AttractionTableDelegate; }
        }
        #endregion

        #region Constructors
        public AttractionTableViewController (IntPtr handle) : base (handle)
        {
        }
        #endregion

        #region Override Methods
        public override void ViewDidLoad ()
        {
            base.ViewDidLoad ();

            // Setup table
            TableView.DataSource = new AttractionTableDatasource (this);
            TableView.Delegate = new AttractionTableDelegate (this);
            TableView.ReloadData ();
        }
        #endregion
    }
}
```

Podívejme bližší pohled na tuto třídu. Nejdřív jsme vytvořili zástupce, aby bylo snazší pro přístup k tabulce zobrazení `DataSource` a `TableDelegate`. Použijeme těch, které později ke komunikaci mezi zobrazení tabulky na levé straně rozdělení zobrazení a zobrazení podrobností v pravém.

Nakonec, pokud tabulka zobrazení je načten do paměti, vytvoříme instance `AttractionTableDatasource` a `AttractionTableDelegate` (obě vytvořili výše) a připojte je k zobrazení tabulky.

Další informace o práci s `UITableViewController`, najdete v tématu společnosti Apple [UITableViewController](https://developer.apple.com/library/prerelease/tvos/documentation/UIKit/Reference/UITableViewController_Class/index.html#//apple_ref/doc/uid/TP40007523) dokumentaci.

<a name="Pulling-it-All-Together" />

## <a name="pulling-it-all-together"></a>Stahování je všechny najednou

Jak jsme uvedli na začátku tohoto dokumentu, zobrazení tabulek se obvykle zobrazují v jedné straně [rozděleným zobrazením](~/ios/tvos/user-interface/split-views.md) jako navigační podrobné informace o této položce vybrané v opačné straně zobrazí. Příklad: 

[ ![](table-views-images/intro01.png "Spuštění ukázkové aplikace")](table-views-images/intro01.png)

Vzhledem k tomu, že je standardní vzor v tvOS, podíváme se na poslední postup všechno, co společně nabídnou a levé a pravé strany rozděleným zobrazením vzájemné interakce.

<a name="The-Detail-View" />

### <a name="the-detail-view"></a>Zobrazení podrobností

Například cesta aplikace uvedené výš, vlastní třídy (`AttractionViewController`) se definují pro standardní řadiče zobrazení zobrazí na pravé straně zobrazení rozdělení jako podrobné zobrazení:

```csharp
using System;
using Foundation;
using UIKit;

namespace tvTable
{
    public partial class AttractionViewController : UIViewController
    {
        #region Private Variables
        private AttractionInformation _attraction = null;
        #endregion

        #region Computed Properties
        public AttractionInformation Attraction {
            get { return _attraction; }
            set {
                _attraction = value;
                UpdateUI ();
            }
        }

        public MasterSplitView SplitView { get; set;}
        #endregion

        #region Constructors
        public AttractionViewController (IntPtr handle) : base (handle)
        {
        }
        #endregion

        #region Public Methods
        public void UpdateUI ()
        {
            // Trap all errors
            try {
                City.Text = Attraction.City.Name;
                Title.Text = Attraction.Name;
                SubTitle.Text = Attraction.Description;

                IsFlighBooked.Hidden = (!Attraction.City.FlightBooked);
                IsFavorite.Hidden = (!Attraction.IsFavorite);
                IsDirections.Hidden = (!Attraction.AddDirections);
                BackgroundImage.Image = UIImage.FromBundle (Attraction.ImageName);
                AttractionImage.Image = BackgroundImage.Image;
            } catch {
                // Since the UI might not be fully loaded, ignore
                // all errors at this point
            }
        }
        #endregion

        #region Override Methods
        public override void ViewWillAppear (bool animated)
        {
            base.ViewWillAppear (animated);

            // Ensure the UI Updates
            UpdateUI ();
        }
        #endregion

        #region Actions
        partial void BookFlight (NSObject sender)
        {
            // Ask user to book flight
            AlertViewController.PresentOKCancelAlert ("Book Flight",
                                                      string.Format ("Would you like to book a flight to {0}?", Attraction.City.Name),
                                                      this,
                                                      (ok) => {
                Attraction.City.FlightBooked = ok;
                IsFlighBooked.Hidden = (!Attraction.City.FlightBooked);
            });
        }

        partial void GetDirections (NSObject sender)
        {
            // Ask user to add directions
            AlertViewController.PresentOKCancelAlert ("Add Directions",
                                                     string.Format ("Would you like to add directions to {0} to you itinerary?", Attraction.Name),
                                                     this,
                                                     (ok) => {
                                                         Attraction.AddDirections = ok;
                                                         IsDirections.Hidden = (!Attraction.AddDirections);
                                                     });
        }

        partial void MarkFavorite (NSObject sender)
        {
            // Flip favorite state
            Attraction.IsFavorite = (!Attraction.IsFavorite);
            IsFavorite.Hidden = (!Attraction.IsFavorite);

            // Reload table
            SplitView.Master.TableController.TableView.ReloadData ();
        }
        #endregion
    }
}
```

Zde jsme zadali **přitažlivosti** (`AttractionInformation`) se zobrazí jako vlastnost a vytvořit `UpdateUI` metoda, která naplní pomůcky uživatelského rozhraní přidat zobrazení v Návrháři rozhraní.

Jsme definovali zástupce také zpět na zobrazení kontroler rozdělení (`SplitView`) budeme používat ke komunikaci změny zpět do tabulky zobrazení (`AcctractionTableView`).

Nakonec vlastní akce (událostí) byly přidány do tří `UIButton` instance vytvořené v Návrháři rozhraní, které umožňují uživatelům označit přitažlivosti jako _Oblíbené_, získat _pokynů_ do přitažlivosti a _sešit letu_ k dané bydlišti.

<a name="The-Navigation-View-Controller" />

### <a name="the-navigation-view-controller"></a>Řadiče zobrazení navigace

Protože řadiče zobrazení tabulky vnořen v řadič navigační zobrazení na levé straně rozděleným zobrazením, řadiče zobrazení navigační byl přiřazen vlastní třídy (`MasterNavigationController`) v Návrháři rozhraní a definovány takto:

```csharp
using System;
using Foundation;
using UIKit;

namespace tvTable
{
    public partial class MasterNavigationController : UINavigationController
    {
        #region Computed Properties
        public MasterSplitView SplitView { get; set;}
        public AttractionTableViewController TableController {
            get { return TopViewController as AttractionTableViewController; }
        }
        #endregion

        #region Constructors
        public MasterNavigationController (IntPtr handle) : base (handle)
        {
        }
        #endregion
    }
}
```

Tato třída definuje znovu, stačí pár zástupce usnadňují komunikují přes sociálními řadiče zobrazení rozdělení:

* `SplitView` -Je odkaz na kontroler zobrazení rozdělení (`MainSpiltViewController`) náležející do řadiče zobrazení navigace.
* `TableController` -Získá řadiče zobrazení tabulky (`AttractionTableViewController`), zobrazí se jako horní zobrazení v navigačním řadiče zobrazení.

<a name="The-Split-View-Controller" />

### <a name="the-split-view-controller"></a>Rozdělení řadiče zobrazení

Rozdělení řadiče zobrazení je základem naší aplikaci, a proto jsme vytvořili vlastní třídy (`MasterSplitViewController`) pro něj v Návrháři rozhraní a definovány takto:

```csharp
using System;
using Foundation;
using UIKit;

namespace tvTable
{
    public partial class MasterSplitView : UISplitViewController
    {
        #region Computed Properties
        public AttractionViewController Details {
            get { return ViewControllers [1] as AttractionViewController; }
        }

        public MasterNavigationController Master {
            get { return ViewControllers [0] as MasterNavigationController; }
        }
        #endregion

        #region Constructors
        public MasterSplitView (IntPtr handle) : base (handle)
        {
        }
        #endregion

        #region Override Methods
        public override void ViewDidLoad ()
        {
            base.ViewDidLoad ();

            // Initialize
            Master.SplitView = this;
            Details.SplitView = this;

            // Wire-up events
            Master.TableController.TableDelegate.AttractionHighlighted += (attraction) => {
                // Display new attraction
                Details.Attraction = attraction;
            };
        }
        #endregion
    }
}
```

Nejdříve vytvoříme zástupce **podrobnosti** strany rozděleným zobrazením (`AttractionViewController`) a **hlavní** straně (`MasterNavigationController`). Znovu to usnadňuje komunikaci mezi sociálními později.

V dalším kroku při zobrazení rozdělení je načten do paměti, jsme připojit řadiče zobrazení rozdělení na obou stranách zobrazení rozdělení a reagovat na uživatele zvýraznění přitažlivosti v zobrazení tabulky (`AttractionHighlighted`) zobrazením nové přitažlivosti v **podrobnosti**  postranní rozdělení zobrazení.

Podrobnosti najdete [tvTables](https://developer.xamarin.com/samples/monotouch/tvos/tvTable/) ukázková aplikace pro úplnou implementaci daného zobrazení tabulek uvnitř zobrazení rozdělení.

## <a name="table-views-in-detail"></a>Zobrazení tabulek podrobně

Vzhledem k tomu, že tvOS je založen na iOS, řadiče zobrazení tabulky a zobrazení tabulek jsou navrženy a chovají podobně. Podrobné informace o práci s zobrazení tabulky v aplikaci Xamarin, najdete v tématu naše iOS [práce s tabulkami a buněk](~/ios/user-interface/controls/tables/index.md) dokumentaci.

<a name="Summary" />

## <a name="summary"></a>Souhrn

Tento článek má zahrnutých navrhování a práce se zobrazeními tabulky uvnitř Xamarin.tvOS aplikace. A má uvedené příklady práci s zobrazení tabulky v zobrazení rozdělení, což je typické použití zobrazení tabulky v aplikaci tvOS.



## <a name="related-links"></a>Související odkazy

- [Ukázky tvOS](https://developer.xamarin.com/samples/tvos/all/)
- [UITableViewController](https://developer.apple.com/library/prerelease/tvos/documentation/UIKit/Reference/UITableViewController_Class/index.html#//apple_ref/doc/uid/TP40007523)
- [tvOS](https://developer.apple.com/tvos/)
- [tvOS lidské rozhraní příručky](https://developer.apple.com/tvos/human-interface-guidelines/)
- [Průvodce programováním aplikace pro tvOS](https://developer.apple.com/library/prerelease/tvos/documentation/General/Conceptual/AppleTV_PG/)
