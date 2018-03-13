---
title: "Práce s textem a vyhledávacích polí"
description: "Tento článek se zabývá navrhování a práci s textem a vyhledávacích polí uvnitř Xamarin.tvOS aplikace."
ms.topic: article
ms.prod: xamarin
ms.assetid: 9EE63CA6-2F31-4EE0-AAE5-82E18CFAC06C
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/16/2017
ms.openlocfilehash: 7d58c30e745e26d1076e75470e527cbe95e85eb6
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 03/09/2018
---
# <a name="working-with-text-and-search-fields"></a>Práce s textem a vyhledávacích polí

_Tento článek se zabývá navrhování a práci s textem a vyhledávacích polí uvnitř Xamarin.tvOS aplikace._



Pokud jsou povinné, Xamarin.tvOS aplikace může požádat o svislý text od uživatele (například ID uživatele a hesla) pomocí textové pole a program klávesnice na obrazovce:

[![](text-fields-and-search-images/intro01.png "Ukázka pole hledání")](text-fields-and-search-images/intro01.png#lightbox)

Volitelně můžete zadat možnost – klíčové slovo vyhledávání obsahu aplikace pomocí pole hledání:

[![](text-fields-and-search-images/intro02.png "Ukázkové výsledky hledání")](text-fields-and-search-images/intro02.png#lightbox)

Tento dokument obsahuje podrobnosti o práci s textem a vyhledávacích polí v aplikaci Xamarin.tvOS.

<a name="About-Text-and-Search-Fields" />

## <a name="about-text-and-search-fields"></a>O Text a pole hledání

Jak jsme uvedli výše, pokud je to nutné, vaše Xamarin.tvOS může být jeden nebo více textová pole ke shromažďování malé množství textu z uživatele s využitím obrazovce (nebo volitelné bluetooth klávesnice v závislosti na verzi tvOS uživatel nainstaloval). 

Kromě toho pokud vaše aplikace zobrazí velké množství obsahu pro uživatele (například hudba, videa nebo kolekci obrázek), můžete zahrnout pole hledání, který umožňuje uživateli zadat malé množství text pro filtrování seznamu položky k dispozici.

<a name="Text-Fields" />

## <a name="text-fields"></a>Textová pole

V tvOS, zobrazí se textové pole jako pole pro zadání-height, zaokrouhlí rohu, které se otevře klávesnice na obrazovce, když uživatel klikne na něm:

[![](text-fields-and-search-images/text01.png "Textové pole v tvOS")](text-fields-and-search-images/text01.png#lightbox)

Když se uživatel přesune [fokus](~/ios/tvos/app-fundamentals/navigation-focus.md) pro dané pole Text, se růst větší a zobrazí hloubkové stínové. Musíte mějte na paměti při navrhování vaší uživatelské rozhraní, jako textová pole může dojít k překrytí další prvky uživatelského rozhraní, když vybraný.

Společnost Apple má následující návrhy pro práci s textová pole:

- **Text položky animací** – vzhledem k povaze program klávesnice na obrazovce, zadáním dlouho části textu nebo vyplníte více textových polí je zdlouhavé pro uživatele. Lepší řešení je omezit množství zadávání textu s použitím seznamech výběru nebo [tlačítka](~/ios/tvos/user-interface/buttons.md).
- **Použít pomocné parametry komunikovat účel** – textové pole můžete zobrazit zástupný text "Nápověda", když je prázdný. Případně můžete pomocí odkazů na servery popište účel vaše textové pole, místo samostatných štítek.
- **Vyberte odpovídající typ klávesnice výchozí** -tvOS poskytuje několik různých, účel vytvořené klávesnice typů, které můžete zadat pro textové pole. Například můžete e-mailovou adresu klávesnice usnadňují položka tím, že se uživateli vybrat ze seznamu nedávno zadané adresy.
- **Podle potřeby použijte zabezpečené textových polí** – textové pole zabezpečení A uvede znaků zadané jako tečky (namísto skutečné písmena). Při shromažďování citlivé informace, jako jsou hesla vždy použijte zabezpečené textové pole.

<a name="Keyboards" />

## <a name="keyboards"></a>Klávesnice

Vždy, když uživatel klikne na textové pole v uživatelském rozhraní, lineární na obrazovce zobrazená klávesnice. Uživatel používá prostor Touch [Siri vzdálené](~/ios/tvos/platform/remote-bluetooth.md#The-Siri-Remote) vyberte jednotlivé písmena z klávesnice a zadejte požadované informace:

[![](text-fields-and-search-images/keyboard01.png "Siri vzdálené klávesnice")](text-fields-and-search-images/keyboard01.png#lightbox)

Pokud je více než jedno pole Text na aktuální zobrazení **Další** tlačítko se zobrazí automaticky pro uživatele další textové pole. A **provádí** tlačítko se zobrazí pole poslední textu, který bude končit zadávání textu a vracet uživateli na předchozí obrazovku. 

Kdykoli můžete také stisknout uživatele **nabídky** tlačítko na vzdáleném Siri zadávání textu ukončit a znovu vrátit na předchozí obrazovku.

Společnost Apple má následující návrhy pro práci s obrazovce klávesnice:

- **Vyberte odpovídající typ klávesnice výchozí** -tvOS poskytuje několik různých, účel vytvořené klávesnice typů, které můžete zadat pro textové pole. Například můžete e-mailovou adresu klávesnice usnadňují položka tím, že se uživateli vybrat ze seznamu nedávno zadané adresy.
- **Podle potřeby použijte zobrazení příslušenství klávesnice** – kromě standardní informace, které jsou vždy zobrazených, volitelné příslušenství zobrazení (například obrázky nebo popisky) mohou být přidány do klávesnice na obrazovce vysvětluje účel text položky nebo položky pomůže uživatele při zadávání požadované informace.

Další informace o práci s program klávesnice na obrazovce, prohlédněte si prosím společnosti Apple [UIKeyboardType](https://developer.apple.com/library/ios/documentation/UIKit/Reference/UITextInputTraits_Protocol/index.html#//apple_ref/c/tdef/UIKeyboardType), [Správa klávesnice](https://developer.apple.com/library/tvos/documentation/StringsTextFonts/Conceptual/TextAndWebiPhoneOS/KeyboardManagement/KeyboardManagement.html#//apple_ref/doc/uid/TP40009542-CH5-SW1), [vlastní zobrazení pro vstupní Data](https://developer.apple.com/library/tvos/documentation/StringsTextFonts/Conceptual/TextAndWebiPhoneOS/InputViews/InputViews.html#//apple_ref/doc/uid/TP40009542-CH12-SW1) a [ Text průvodce programováním pro iOS](https://developer.apple.com/library/tvos/documentation/StringsTextFonts/Conceptual/TextAndWebiPhoneOS/Introduction/Introduction.html) dokumentaci.

<a name="Search" />

## <a name="search"></a>Hledat

Pole hledání specializované obrazovky poskytuje textové pole k dispozici a klávesnice na obrazovce, která umožňuje uživatelům filtrovat kolekce položek, které jsou zobrazeny níže klávesnice:

[![](text-fields-and-search-images/search01.png "Ukázkové výsledky hledání")](text-fields-and-search-images/search01.png#lightbox)

Jako uživatel zadá písmena do pole hledání, výsledky níže se automaticky projeví výsledky hledání. Uživatel můžete kdykoli, posun zaměření na výsledky a vyberte jednu z položek, které jsou uvedené.

Společnost Apple má následující návrhy pro práci s poli vyhledávání:

- **Zadejte poslední hledání** – protože zadávání textu pomocí vzdáleného Siri může to být zdlouhavé a uživatelé zpravidla opakujte požadavky search, zvažte přidání části výsledků vyhledávání poslední před aktuální výsledky v oblasti klávesnice.
- **Pokud je to možné, omezte počet výsledků** – protože velké seznam položek, může být pro uživatele k analýze a procházení, zvažte omezení počet vrácených výsledků.
- **V případě potřeby zadejte výsledek hledání filtry** – Pokud je obsah aplikace různě, zvažte přidání oboru řádky chcete umožnit uživatelům dál filtrovat výsledky hledání vrátila.

Další informace najdete v tématu společnosti Apple [referenci třídy UISearchController](https://developer.apple.com/library/tvos/documentation/UIKit/Reference/UISearchController/index.html).

<a name="Working-with-Text-Fields" />

## <a name="working-with-text-fields"></a>Práce s textová pole

Nejjednodušší způsob, jak pracovat s textových polí v aplikaci Xamarin.tvOS je chcete přidat do návrhu uživatelské rozhraní pomocí návrháře iOS.

Postupujte takto:

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

1. V **řešení Pad**, dvakrát klikněte `Main.storyboard` soubor otevřete pro úpravy.
1. Přetáhněte jeden nebo více **textových polí** int návrhovou plochu do zobrazení: 

    [![](text-fields-and-search-images/text02.png "Textové pole")](text-fields-and-search-images/text02.png#lightbox)
1. Vyberte **textových polí** a poskytněte každou jedinečnou **název** v **pomůcky** kartě **vlastnosti Pad**: 

    [![](text-fields-and-search-images/text03.png "Na kartě pomůcky Pad vlastnosti")](text-fields-and-search-images/text03.png#lightbox)
1. V **textové pole** části, můžete definovat prvky, jako **zástupný symbol** pomocný parametr a výchozí **hodnotu**: 

    [![](text-fields-and-search-images/text04.png "V části textové pole.")](text-fields-and-search-images/text04.png#lightbox)
1. Posuňte se dolů k definování vlastností, jako **kontrolu pravopisu**, **malá a velká písmena** a ve výchozím nastavení **klávesnice typ**: 

    [![](text-fields-and-search-images/text05.png "Kontrola pravopisu, malá a velká písmena a ve výchozím nastavení typ klávesnice")](text-fields-and-search-images/text05.png#lightbox) 
1. Uložte změny do vašeho scénáře.
    
# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)
    
1. V **Průzkumníku řešení**, dvakrát klikněte `Main.storyboard` soubor otevřete pro úpravy.
1. Přetáhněte jeden nebo více **textových polí** int návrhovou plochu do zobrazení: 

    [![](text-fields-and-search-images/text02-vs.png "Textové pole")](text-fields-and-search-images/text02-vs.png#lightbox)
1. Vyberte **textových polí** a poskytněte každou jedinečnou **název** v **pomůcky** kartě **Explorer vlastnosti**: 

    [![](text-fields-and-search-images/text03-vs.png "Na kartě pomůcky")](text-fields-and-search-images/text03-vs.png#lightbox)
1. V **textové pole** části, můžete definovat prvky, jako **zástupný symbol** pomocný parametr a výchozí **hodnotu**: 

    [![](text-fields-and-search-images/text04-vs.png "V části textové pole.")](text-fields-and-search-images/text04-vs.png#lightbox)
1. Posuňte se dolů k definování vlastností, jako **kontrolu pravopisu**, **malá a velká písmena** a ve výchozím nastavení **klávesnice typ**: 

    [![](text-fields-and-search-images/text05-vs.png "Kontrola pravopisu, malá a velká písmena a ve výchozím nastavení typ klávesnice")](text-fields-and-search-images/text05-vs.png#lightbox) 
1. Uložte změny do vašeho scénáře.
    
-----

V kódu můžete získat nebo nastavit hodnotu textové pole pomocí jeho `Text` vlastnost:

```csharp
Console.WriteLine ("User ID {0} and Password {1}", UserId.Text, Password.Text);
```

Volitelně můžete `Started` a `Ended` události textové pole pro zadávání textu počáteční a koncovou reagovat.

<a name="Working-with-Search-Fields" />

## <a name="working-with-search-fields"></a>Práce s vyhledávacích polí

Nejjednodušší způsob, jak pracovat s vyhledávacích polí v aplikaci Xamarin.tvOS je chcete přidat do návrhu uživatelské rozhraní pomocí návrháře rozhraní.

Postupujte takto:

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)
    
1. V **řešení Pad**, dvakrát klikněte `Main.storyboard` soubor otevřete pro úpravy.
1. Přetáhněte nového řadiče zobrazení kolekce do scénáře k dispozici výsledky hledání uživatele: 

    [![](text-fields-and-search-images/search02.png "Řadič zobrazení kolekce")](text-fields-and-search-images/search02.png#lightbox)
1. V **pomůcky** kartě **vlastnosti Pad**, použijte `SearchResultsViewController` pro **– třída** a `SearchResults` pro **Storyboard ID**: 

    [![](text-fields-and-search-images/search03.png "Na kartě pomůcky")](text-fields-and-search-images/search03.png#lightbox)
1. Vyberte **buňky prototypu** na návrhovou plochu.
1. V **pomůcky** kartě **Explorer vlastnosti**, použijte `SearchResultCell` pro **– třída** a `ImageCell` pro **identifikátor**: 

    [![](text-fields-and-search-images/search04.png "Na kartě pomůcky")](text-fields-and-search-images/search04.png#lightbox)
1. Rozložení návrh z **buňky prototypu** a vystavit každý prvek s jedinečným **název** v **pomůcky** kartě **vlastnosti Explorer**: 

    [![](text-fields-and-search-images/search05.png "Rozložení v návrhu prototypu buňky")](text-fields-and-search-images/search05.png#lightbox)
1. Uložte změny do vašeho scénáře.
    
# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)
    
1. V **Průzkumníku řešení**, dvakrát klikněte `Main.storyboard` soubor otevřete pro úpravy.
1. Přetáhněte nového řadiče zobrazení kolekce do scénáře k dispozici výsledky hledání uživatele: 

    [![](text-fields-and-search-images/seach02-vs.png "Řadič zobrazení kolekce")](text-fields-and-search-images/seach02-vs.png#lightbox)
1. V **pomůcky** kartě **Explorer vlastnosti**, použijte `SearchResultsViewController` pro **– třída** a `SearchResults` pro **Storyboard ID**: 

    [![](text-fields-and-search-images/search03-vs.png "Na kartě pomůcky")](text-fields-and-search-images/search03-vs.png#lightbox)
1. Vyberte **buňky prototypu** na návrhovou plochu.
1. V **pomůcky** kartě **Explorer vlastnosti**, použijte `SearchResultCell` pro **– třída** a `ImageCell` pro **identifikátor**: 

    [![](text-fields-and-search-images/search04-vs.png "Na kartě pomůcky")](text-fields-and-search-images/search04-vs.png#lightbox)
1. Rozložení návrh z **buňky prototypu** a vystavit každý prvek s jedinečným **název** v **pomůcky** kartě **vlastnosti Explorer**: 

    [![](text-fields-and-search-images/search05-vs.png "Rozložení v návrhu prototypu buňky")](text-fields-and-search-images/search05-vs.png#lightbox)
1. Uložte změny do vašeho scénáře.
    
-----

<a name="Provide-a-Data-Model" />

### <a name="provide-a-data-model"></a>Zadejte datový Model

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

Dále musíte zadat třídu tak, aby fungoval jako datový Model pro výsledky, který uživatel hledání. V **Průzkumníku řešení**, klikněte pravým tlačítkem na název projektu a vyberte **přidat** > **nový soubor...**   >  **Obecné** > **prázdné třídy** a poskytují **název**: 

[![](text-fields-and-search-images/search06.png "Vyberte třídu prázdný a zadejte název")](text-fields-and-search-images/search06.png#lightbox)

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Dále musíte zadat třídu tak, aby fungoval jako datový Model pro výsledky, který uživatel hledání. V **Průzkumníku řešení**, klikněte pravým tlačítkem na název projektu a vyberte **přidat** > **novou položku...**   >  **Apple** > **různé** > **třída** a poskytují **název**: 

[![](text-fields-and-search-images/search06-vs.png "Vyberte třídu a zadejte název")](text-fields-and-search-images/search06-vs.png#lightbox)

-----

Jako příklad aplikaci, která umožňuje uživatelům vyhledávat kolekce obrázků podle názvu a klíčové slovo vypadat následovně:

```csharp
using System;
using Foundation;

namespace tvText
{
    public class PictureInformation : NSObject
    {
        #region Computed Properties
        public string Title { get; set;}
        public string ImageName { get; set;}
        public string Keywords { get; set;}
        #endregion

        #region Constructors
        public PictureInformation (string title, string imageName, string keywords)
        {
            // Initialize
            this.Title = title;
            this.ImageName = imageName;
            this.Keywords = keywords;
        }
        #endregion
    }
}
```

<a name="The-Collection-View-Cell" />

### <a name="the-collection-view-cell"></a>Buňku zobrazení kolekce

S datovým modelem na místě, upravit **prototypu buňky** (`SearchResultViewCell.cs`) a nastavit jej jako vzhled jsou následující:

```csharp
using Foundation;
using System;
using UIKit;

namespace tvText
{
    public partial class SearchResultViewCell : UICollectionViewCell
    {
        #region Private Variables
        private PictureInformation _pictureInfo = null;
        #endregion

        #region Computed Properties
        public PictureInformation PictureInfo {
            get { return _pictureInfo; }
            set {
                _pictureInfo = value;
                UpdateUI ();
            }
        }
        #endregion

        #region Constructors
        public SearchResultViewCell (IntPtr handle) : base (handle)
        {
            // Initialize
            UpdateUI ();
        }
        #endregion

        #region Private Methods
        private void UpdateUI ()
        {
            // Anything to process?
            if (PictureInfo == null) return;

            try {
                Picture.Image = UIImage.FromBundle (PictureInfo.ImageName);
                Picture.AdjustsImageWhenAncestorFocused = true;
                Title.Text = PictureInfo.Title;
                TextColor = UIColor.LightGray;
            } catch {
                // Ignore errors if view isn't fully loaded
            }
        }
        #endregion
    }

}
```

`UpdateUI` Metoda se použije k zobrazení jednotlivých polí z **PictureInformation** položky ( `PictureInfo` vlastnost) v pojmenované prvky uživatelského rozhraní pokaždé, když vlastnost je aktualizováno. Například bitová kopie a titul, přidružený na obrázku.

<a name="The-Collection-View-Controller" />

### <a name="the-collection-view-controller"></a>Řadiče zobrazení kolekce

Potom upravte řadiče zobrazení kolekce výsledky vyhledávání (`SearchResultsViewController.cs`) a nastavit jej vypadat třeba takto:

```csharp
using Foundation;
using System;
using UIKit;
using System.Collections.Generic;

namespace tvText
{
    public partial class SearchResultsViewController : UICollectionViewController , IUISearchResultsUpdating
    {
        #region Constants
        public const string CellID = "ImageCell";
        #endregion

        #region Private Variables
        private string _searchFilter = "";
        #endregion

        #region Computed Properties
        public List<PictureInformation> AllPictures { get; set;}
        public List<PictureInformation> FoundPictures { get; set; }
        public string SearchFilter {
            get { return _searchFilter; }
            set {
                _searchFilter = value.ToLower();
                FindPictures ();
                CollectionView?.ReloadData ();
            }
        }
        #endregion

        #region Constructors
        public SearchResultsViewController (IntPtr handle) : base (handle)
        {
            // Initialize
            this.AllPictures = new List<PictureInformation> ();
            this.FoundPictures = new List<PictureInformation> ();
            PopulatePictures ();
            FindPictures ();

        }
        #endregion

        #region Private Methods
        private void PopulatePictures ()
        {
            // Clear list
            AllPictures.Clear ();

            // Add images
            AllPictures.Add (new PictureInformation ("Antipasta Platter","Antipasta","cheese,grapes,tomato,coffee,meat,plate"));
            AllPictures.Add (new PictureInformation ("Cheese Plate", "CheesePlate", "cheese,plate,bread"));
            AllPictures.Add (new PictureInformation ("Coffee House", "CoffeeHouse", "coffee,people,menu,restaurant,cafe"));
            AllPictures.Add (new PictureInformation ("Computer and Expresso", "ComputerExpresso", "computer,coffee,expresso,phone,notebook"));
            AllPictures.Add (new PictureInformation ("Hamburger", "Hamburger", "meat,bread,cheese,tomato,pickle,lettus"));
            AllPictures.Add (new PictureInformation ("Lasagna Dinner", "Lasagna", "salad,bread,plate,lasagna,pasta"));
            AllPictures.Add (new PictureInformation ("Expresso Meeting", "PeopleExpresso", "people,bag,phone,expresso,coffee,table,tablet,notebook"));
            AllPictures.Add (new PictureInformation ("Soup and Sandwich", "SoupAndSandwich", "soup,sandwich,bread,meat,plate,tomato,lettus,egg"));
            AllPictures.Add (new PictureInformation ("Morning Coffee", "TabletCoffee", "tablet,person,man,coffee,magazine,table"));
            AllPictures.Add (new PictureInformation ("Evening Coffee", "TabletMagCoffee", "tablet,magazine,coffee,table"));
        }

        private void FindPictures ()
        {
            // Clear list
            FoundPictures.Clear ();

            // Scan each picture for a match
            foreach (PictureInformation picture in AllPictures) {
                if (SearchFilter == "") {
                    // If no search term, everything matches
                    FoundPictures.Add (picture);
                } else if (picture.Title.Contains (SearchFilter) || picture.Keywords.Contains (SearchFilter)) {
                    // If the search term is in the title or keywords, we've found a match
                    FoundPictures.Add (picture);
                }
            }
        }
        #endregion

        #region Override Methods
        public override nint NumberOfSections (UICollectionView collectionView)
        {
            // Only one section in this collection
            return 1;
        }

        public override nint GetItemsCount (UICollectionView collectionView, nint section)
        {
            // Return the number of matching pictures
            return FoundPictures.Count;
        }

        public override UICollectionViewCell GetCell (UICollectionView collectionView, NSIndexPath indexPath)
        {
            // Get a new cell and return it
            var cell = collectionView.DequeueReusableCell (CellID, indexPath);
            return (UICollectionViewCell)cell;
        }

        public override void WillDisplayCell (UICollectionView collectionView, UICollectionViewCell cell, NSIndexPath indexPath)
        {
            // Grab the cell
            var currentCell = cell as SearchResultViewCell;
            if (currentCell == null)
                throw new Exception ("Expected to display a `SearchResultViewCell`.");

            // Display the current picture info in the cell
            var item = FoundPictures [indexPath.Row];
            currentCell.PictureInfo = item;
        }

        public override void ItemSelected (UICollectionView collectionView, NSIndexPath indexPath)
        {
            // If this Search Controller was presented as a modal view, close
            // it before continuing
            // DismissViewController (true, null);

            // Grab the picture being selected and report it
            var picture = FoundPictures [indexPath.Row];
            Console.WriteLine ("Selected: {0}", picture.Title);
        }

        public void UpdateSearchResultsForSearchController (UISearchController searchController)
        {
            // Save the search filter and update the Collection View
            SearchFilter = searchController.SearchBar.Text ?? string.Empty;
        }

        public override void DidUpdateFocus (UIFocusUpdateContext context, UIFocusAnimationCoordinator coordinator)
        {
            var previousItem = context.PreviouslyFocusedView as SearchResultViewCell;
            if (previousItem != null) {
                UIView.Animate (0.2, () => {
                    previousItem.TextColor = UIColor.LightGray;
                });
            }

            var nextItem = context.NextFocusedView as SearchResultViewCell;
            if (nextItem != null) {
                UIView.Animate (0.2, () => {
                    nextItem.TextColor = UIColor.Black;
                });
            }
        }
        #endregion
    }
}
```

Nejdřív `IUISearchResultsUpdating` rozhraní je přidat do třídy pro zpracování vyhledávání řadiče filtr aktualizované uživatelem:

```csharp
public partial class SearchResultsViewController : UICollectionViewController , IUISearchResultsUpdating
```

Konstanta je také definován zadat ID **prototypu buňky** (která odpovídá ID definované v Návrháři rozhraní výše), se použije později při řadič kolekce o nové buňky:

```csharp
public const string CellID = "ImageCell";
```

Úložiště se vytvoří pro úplný seznam položek prohledávaný filtru hledaný termín a seznam položek odpovídající tento termín:

```csharp
private string _searchFilter = "";
...

public List<PictureInformation> AllPictures { get; set;}
public List<PictureInformation> FoundPictures { get; set; }
public string SearchFilter {
    get { return _searchFilter; }
    set {
        _searchFilter = value.ToLower();
        FindPictures ();
        CollectionView?.ReloadData ();
    }
}
```

Když `SearchFilter` je změnit, je znovu načíst obsah zobrazení kolekce a aktualizovat seznam odpovídající položky. `FindPictures` Rutina zodpovídá za hledání položek, které odpovídají nové hledaný termín:

```csharp
private void FindPictures ()
{
    // Clear list
    FoundPictures.Clear ();

    // Scan each picture for a match
    foreach (PictureInformation picture in AllPictures) {
        if (SearchFilter == "") {
            // If no search term, everything matches
            FoundPictures.Add (picture);
        } else if (picture.Title.Contains (SearchFilter) || picture.Keywords.Contains (SearchFilter)) {
            // If the search term is in the title or keywords, we've found a match
            FoundPictures.Add (picture);
        }
    }
}
```

Hodnota `SearchFilter` budou aktualizovány (které se aktualizace zobrazení výsledků kolekce) když uživatel změní filtru v Kontroleru vyhledávání:

```csharp
public void UpdateSearchResultsForSearchController (UISearchController searchController)
{
    // Save the search filter and update the Collection View
    SearchFilter = searchController.SearchBar.Text ?? string.Empty;
}
```

`PopulatePictures` Metoda původně naplní kolekce dostupné položky:

```csharp
private void PopulatePictures ()
{
    // Clear list
    AllPictures.Clear ();

    // Add images
    AllPictures.Add (new PictureInformation ("Antipasta Platter","Antipasta","cheese,grapes,tomato,coffee,meat,plate"));
    ...
}
```

Z důvodu například všechny ukázkových dat se vytváří v paměti při načtení řadiče zobrazení kolekce. V reálné aplikaci tato data by se pravděpodobně načten z databáze nebo webové služby a podle potřeby k udržování z překročení televizoru Apple omezený paměti.

`NumberOfSections` a `GetItemsCount` metody poskytují počet odpovídajících položek:

```csharp
public override nint NumberOfSections (UICollectionView collectionView)
{
    // Only one section in this collection
    return 1;
}

public override nint GetItemsCount (UICollectionView collectionView, nint section)
{
    // Return the number of matching pictures
    return FoundPictures.Count;
}
```

`GetCell` Metoda vrátí novou **prototypu buňky** (na základě `CellID` definovaný nad ve scénáři) pro každou položku v zobrazení kolekce:

```csharp
public override UICollectionViewCell GetCell (UICollectionView collectionView, NSIndexPath indexPath)
{
    // Get a new cell and return it
    var cell = collectionView.DequeueReusableCell (CellID, indexPath);
    return (UICollectionViewCell)cell;
}
```

`WillDisplayCell` Metoda je volána před buňky se zobrazuje, takže lze nastavit:

```csharp
public override void WillDisplayCell (UICollectionView collectionView, UICollectionViewCell cell, NSIndexPath indexPath)
{
    // Grab the cell
    var currentCell = cell as SearchResultViewCell;
    if (currentCell == null)
        throw new Exception ("Expected to display a `SearchResultViewCell`.");

    // Display the current picture info in the cell
    var item = FoundPictures [indexPath.Row];
    currentCell.PictureInfo = item;
}
```

`DidUpdateFocus` Metoda poskytuje vizuální zpětnou vazbu pro uživatele se zvýrazněte položky v kolekci zobrazení výsledků:

```csharp
public override void DidUpdateFocus (UIFocusUpdateContext context, UIFocusAnimationCoordinator coordinator)
{
    var previousItem = context.PreviouslyFocusedView as SearchResultViewCell;
    if (previousItem != null) {
        UIView.Animate (0.2, () => {
            previousItem.TextColor = UIColor.LightGray;
        });
    }

    var nextItem = context.NextFocusedView as SearchResultViewCell;
    if (nextItem != null) {
        UIView.Animate (0.2, () => {
            nextItem.TextColor = UIColor.Black;
        });
    }
}
```

Nakonec `ItemSelected` metoda zpracovává uživatele výběrem položky (kliknutím na povrchu Touch se vzdálenými Siri) v kolekci zobrazení výsledků:

```csharp
public override void ItemSelected (UICollectionView collectionView, NSIndexPath indexPath)
{
    // If this Search Controller was presented as a modal view, close
    // it before continuing
    // DismissViewController (true, null);

    // Grab the picture being selected and report it
    var picture = FoundPictures [indexPath.Row];
    Console.WriteLine ("Selected: {0}", picture.Title);
}
```

Pokud pole hledání byl předložený jako modální dialogové okno zobrazení (přes horní část zobrazení volání ji), použijte `DismissViewController` metoda zrušíte vyhledávání zobrazení, když uživatel vybere položku. V tomto příkladu pole hledání se zobrazí jako obsah na kartě Karta zobrazení, není zde se zavře.

Další informace o zobrazení kolekcí najdete v tématu naše [práce se zobrazeními kolekce](~/ios/tvos/user-interface/collection-views.md) dokumentaci.

<a name="Presenting the Search Field" />

### <a name="presenting-the-search-field"></a>Představuje pole hledání

Existují dva hlavní způsoby, pole hledání (a přidružené program klávesnice na obrazovce a výsledky hledání) lze zobrazit v tvOS uživateli: 

- **Modální dialogové okno zobrazení** -lze zobrazit pole hledání přes aktuálního zobrazení a řadiče zobrazení jako modální dialogové okno zobrazení na celou obrazovku. Důvodem je obvykle v reakci na uživatel kliknutím na tlačítko nebo jiného elementu uživatelského rozhraní. Dialogové okno se zavře, když uživatel vybere položka ze seznamu výsledků.
- **Zobrazit obsah** -The pole hledání je přímé součástí dané zobrazení. Například jako obsah karty vyhledávání v Kontroleru zobrazení karty.

Například seznam obrázky výše uvedených možností vyhledávání pole hledání se zobrazí jako zobrazit obsah na kartě vyhledávání a View Controller karta vyhledávání vypadá takto:

```csharp
using System;
using UIKit;

namespace tvText
{
    public partial class SecondViewController : UIViewController
    {
        #region Constants
        public const string SearchResultsID = "SearchResults";
        #endregion

        #region Computed Properties
        public SearchResultsViewController ResultsController { get; set;}
        #endregion

        #region Constructors
        public SecondViewController (IntPtr handle) : base (handle)
        {
        }
        #endregion

        #region Private Methods
        public void ShowSearchController ()
        {
            // Build an instance of the Search Results View Controller from the Storyboard
            ResultsController = Storyboard.InstantiateViewController (SearchResultsID) as SearchResultsViewController;
            if (ResultsController == null)
                throw new Exception ("Unable to instantiate a SearchResultsViewController.");

            // Create an initialize a new search controller
            var searchController = new UISearchController (ResultsController) {
                SearchResultsUpdater = ResultsController,
                HidesNavigationBarDuringPresentation = false
            };

            // Set any required search parameters
            searchController.SearchBar.Placeholder = "Enter keyword (e.g. coffee)";

            // The Search Results View Controller can be presented as a modal view
            // PresentViewController (searchController, true, null);

            // Or in the case of this sample, the Search View Controller is being
            // presented as the contents of the Search Tab directly. Use either one
            // or the other method to display the Search Controller (not both).
            var container = new UISearchContainerViewController (searchController);
            var navController = new UINavigationController (container);
            AddChildViewController (navController);
            View.Add (navController.View);
        }
        #endregion

        #region Override Methods
        public override void ViewDidLoad ()
        {
            base.ViewDidLoad ();

            // If the Search Controller is being displayed as the content
            // of the search tab, include it here.
            ShowSearchController ();
        }

        public override void ViewDidAppear (bool animated)
        {
            base.ViewDidAppear (animated);

            // If the Search Controller is being presented as a modal view,
            // call it here to display it over the contents of the Search
            // tab.
            // ShowSearchController ();
        }
        #endregion
    }
}
```

Konstanta je definována nejprve, který odpovídá **Storyboard identifikátor** který byl přiřazen do řadiče zobrazení kolekce výsledky hledání v Návrháři rozhraní:

```csharp
public const string SearchResultsID = "SearchResults";
```

Dále `ShowSearchController` metoda vytvoří nový řadič kolekce zobrazení vyhledávání a zobrazí se potřeby:

```csharp
public void ShowSearchController ()
{
    // Build an instance of the Search Results View Controller from the Storyboard
    ResultsController = Storyboard.InstantiateViewController (SearchResultsID) as SearchResultsViewController;
    if (ResultsController == null)
        throw new Exception ("Unable to instantiate a SearchResultsViewController.");

    // Create an initialize a new search controller
    var searchController = new UISearchController (ResultsController) {
        SearchResultsUpdater = ResultsController,
        HidesNavigationBarDuringPresentation = false
    };

    // Set any required search parameters
    searchController.SearchBar.Placeholder = "Enter keyword (e.g. coffee)";

    // The Search Results View Controller can be presented as a modal view
    // PresentViewController (searchController, true, null);

    // Or in the case of this sample, the Search View Controller is being
    // presented as the contents of the Search Tab directly. Use either one
    // or the other method to display the Search Controller (not both).
    var container = new UISearchContainerViewController (searchController);
    var navController = new UINavigationController (container);
    AddChildViewController (navController);
    View.Add (navController.View);
}
```

V metodě výše, jednou `SearchResultsViewController` po vytvoření instance scénáře, nový `UISearchController` se vytvoří pro program klávesnice na obrazovce pro uživatele a k dispozici pole hledání. Výsledky hledání kolekce (podle definice `SearchResultsViewController`) se zobrazí pod tuto klávesnici.

Dále `SearchBar` je nakonfigurován s informacemi, jako **zástupný symbol** pomocný parametr. To poskytuje informace o uživateli typ vyhledávání se provádět.

Potom pole hledání se zobrazí uživateli v jednom ze dvou způsobů:

- **Modální dialogové okno zobrazení** – `PresentViewController` metoda je volána k dispozici hledání přes existující zobrazení přes celou obrazovku.
- **Zobrazit obsah** – `UISearchContainerViewController` je vytvořit tak, aby obsahovala řadičem vyhledávání. A `UINavigationController` se vytvoří tak, aby obsahovala kontejneru vyhledávání, pak přidáte řadič navigační řadiče zobrazení `AddChildViewController (navController)`a zobrazení uvedené `View.Add (navController.View)`.

Nakonec a znovu na základě typu prezentace buď `ViewDidLoad` nebo `ViewDidAppear` bude volání metody `ShowSearchController` metody na hledání pro uživatele:

```csharp
public override void ViewDidLoad ()
{
    base.ViewDidLoad ();

    // If the Search Controller is being displayed as the content
    // of the search tab, include it here.
    ShowSearchController ();
}

public override void ViewDidAppear (bool animated)
{
    base.ViewDidAppear (animated);

    // If the Search Controller is being presented as a modal view,
    // call it here to display it over the contents of the Search
    // tab.
    // ShowSearchController ();
}
```

Při spuštění aplikace a kartě vybraný uživatelem, nefiltrované úplný seznam položek bude zobrazovat uživatelům:

[![](text-fields-and-search-images/intro02.png "Výsledky hledání výchozí")](text-fields-and-search-images/intro02.png#lightbox)

Jako uživatel začne zadejte hledaný termín, seznam výsledků se filtrují podle tento termín a aktualizuje automaticky:

[![](text-fields-and-search-images/intro03.png "Filtrované výsledky hledání")](text-fields-and-search-images/intro03.png#lightbox)

Kdykoli můžete uživatele přepnutí položku ve výsledcích hledání a klikněte na prostor Touch vzdálené Siri ji vyberte.

<a name="Summary" />

## <a name="summary"></a>Souhrn

Tento článek má zahrnutých navrhování a práci s textem a vyhledávacích polí uvnitř Xamarin.tvOS aplikace. Je vám ukázal, jak vytvořit obsah textu a kolekce vyhledávání v Návrháři rozhraní a jeho ukázalo pro dva různé způsoby, které by mohly být pole hledání prezentovány v tvOS uživateli.



## <a name="related-links"></a>Související odkazy

- [Ukázky pro tvOS](https://developer.xamarin.com/samples/tvos/all/)
- [tvOS](https://developer.apple.com/tvos/)
- [tvOS lidské rozhraní příručky](https://developer.apple.com/tvos/human-interface-guidelines/)
- [Průvodce programováním aplikace pro tvOS](https://developer.apple.com/library/prerelease/tvos/documentation/General/Conceptual/AppleTV_PG/)
