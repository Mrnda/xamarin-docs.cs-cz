---
title: Výběr dokumentu v Xamarin.iosu
description: Tento dokument popisuje iOS výběr dokumentu a jeho použití v Xamarin.iOS. To se podíváme na serveru služby iCloud, dokumenty, společný kód nastavení, rozšíření poskytovatelů dokumentu a další.
ms.prod: xamarin
ms.assetid: 89539D79-BC6E-4A3E-AEC6-69D9A6CC6818
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 06/05/2017
ms.openlocfilehash: ca0c7a6e655fdc44aa673a59be71bc83044d3085
ms.sourcegitcommit: 51c274f37369d8965b68ff587e1c2d9865f85da7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/30/2018
ms.locfileid: "39353331"
---
# <a name="document-picker-in-xamarinios"></a>Výběr dokumentu v Xamarin.iosu

Umožňuje nástroj pro výběr dokumentu dokumenty, které se sdílejí mezi aplikacemi. Tyto dokumenty mohou být uloženy v Icloudu nebo v adresáři jiné aplikace. Dokumenty jsou sdílené úložiště přes sadu [rozšíření poskytovatelů dokumentu](~/ios/platform/extensions.md) uživatel nainstaloval na svém zařízení. 

Z důvodu potíže role při ochraně dokumentů synchronizaci napříč aplikacemi a cloudu přinášejí určité množství nezbytné složitost.

## <a name="requirements"></a>Požadavky

Pokud chcete provést kroky uvedené v tomto článku jsou vyžadovány následující položky:

-  **Xcode 7 a iOS 8 nebo novější** – Apple Xcode 7 a iOS 8 nebo novějších rozhraní API musí být nainstalovaná a nakonfigurovaná v počítači vývojáře.
-  **Visual Studio nebo Visual Studio pro Mac** – by měl být nainstalována nejnovější verze sady Visual Studio pro Mac.
-  **Zařízení se systémem iOS** – zařízení s iOS s iOS 8 nebo novější.

## <a name="changes-to-icloud"></a>Změny na serveru služby iCloud

Implementace nové funkce nástroje pro výběr dokumentu, byly provedeny následující změny na Apple serveru služby iCloud služby:

-  Na serveru služby iCloud démon zcela přepsali jsme pomocí CloudKit.
-  Existujícího serveru služby iCloud funkce byly přejmenovány Icloudu jednotky.
-  Byla přidána podpora pro operační systém Microsoft Windows na serveru služby iCloud.
-  Ve Finderu operační systém Mac se přidala k složce serveru služby iCloud.
-  zařízení s Iosem můžete přístup k obsahu složky systému Mac OS serveru služby iCloud.

> [!IMPORTANT]
> Apple [poskytuje nástroje](https://developer.apple.com/support/allowing-users-to-manage-data/) , což vývojářům umožňuje správně zpracovat Evropské unie obecného Regulation (GDPR).

## <a name="what-is-a-document"></a>Co je dokument?

Při odkazování na dokument v Icloudu, je jeden, samostatné entity a by měl být vnímané jako takový uživatel. Uživatel může chtít upravovat nebo sdílet s ostatními uživateli (třeba pomocí e-mailu).

Existuje několik typů souborů, že uživatel okamžitě rozpozná jako dokumenty, jako jsou například stránky, soubory hlavní vystoupení nebo čísla. Však není omezen na tento koncept serveru služby iCloud. Například stav hry (jako je například porovnávání šachy) můžete považován za dokumentu a uložená v Icloudu. Tento soubor může být předány mezi zařízeními uživatelů a zajistí, aby získaly hru, kde skončil na jiném zařízení.

## <a name="dealing-with-documents"></a>Práce s dokumenty

Před všemi zúčastněnými stranami je kód potřebný k pomocí nástroje pro výběr dokumentu s využitím kódu Xamarin, v tomto článku se bude zabývat osvědčené postupy pro práci s dokumenty Icloudu a některé změny provedené do stávajících rozhraní potřeba k podpoře nástroje pro výběr dokumentu.

### <a name="using-file-coordination"></a>Pomocí souboru koordinace

Vzhledem k tomu, že soubor můžete upravit z několika různých míst, musí se tak ztrátě dat použít koordinace.

 [![](document-picker-images/image1.png "Pomocí souboru koordinace")](document-picker-images/image1.png#lightbox)

Pojďme se podívat na obrázku výše:

1.  Zařízení se systémem iOS pomocí souboru koordinace vytvoří nový dokument a uloží jej do složky serveru služby iCloud.
2.  iCloud upravený soubor uloží do cloudu pro distribuci do všech zařízení.
3.  Připojené Mac vidí změněný soubor ve složce serveru služby iCloud a poznamenejte změny do souboru pomocí souboru koordinace.
4.  Zařízení bez použití souboru koordinace provede změnu v souboru a uloží jej do složky serveru služby iCloud. Tyto změny se okamžitě replikují do dalších zařízení.

Předpokládejme, původní zařízení se systémem iOS nebo Mac se úpravy souboru nyní ztrátě nebo přepsána verzí souborů ze zařízení nekoordinovaná jejich změny. Aby se zabránilo ztrátě dat, soubor koordinace je nezbytnost při práci s dokumenty založené na cloudu.

### <a name="using-uidocument"></a>Pomocí UIDocument

 `UIDocument` zjednodušuje věci (nebo `NSDocument` v systému macOS) tímto způsobem všechny těžkou pro vývojáře. Poskytuje integrované v souboru koordinaci s frontami pozadí zabránit zablokování uživatelského rozhraní aplikace.

 `UIDocument` poskytuje několik rozhraní API vysoké úrovně, která usnadňují náročnost vývoje aplikace Xamarin pro všechny účely vývojář vyžaduje.

Následující kód vytvoří podtřída `UIDocument` implementovat Obecný dokument založený na textu, který slouží k uložení a načtení textu ze serveru služby iCloud:

```csharp
using System;
using Foundation;
using UIKit;

namespace DocPicker
{
    public class GenericTextDocument : UIDocument
    {
        #region Private Variable Storage
        private NSString _dataModel;
        #endregion

        #region Computed Properties
        public string Contents {
            get { return _dataModel.ToString (); }
            set { _dataModel = new NSString(value); }
        }
        #endregion

        #region Constructors
        public GenericTextDocument (NSUrl url) : base (url)
        {
            // Set the default document text
            this.Contents = "";
        }

        public GenericTextDocument (NSUrl url, string contents) : base (url)
        {
            // Set the default document text
            this.Contents = contents;
        }
        #endregion

        #region Override Methods
        public override bool LoadFromContents (NSObject contents, string typeName, out NSError outError)
        {
            // Clear the error state
            outError = null;

            // Were any contents passed to the document?
            if (contents != null) {
                _dataModel = NSString.FromData( (NSData)contents, NSStringEncoding.UTF8 );
            }

            // Inform caller that the document has been modified
            RaiseDocumentModified (this);

            // Return success
            return true;
        }

        public override NSObject ContentsForType (string typeName, out NSError outError)
        {
            // Clear the error state
            outError = null;

            // Convert the contents to a NSData object and return it
            NSData docData = _dataModel.Encode(NSStringEncoding.UTF8);
            return docData;
        }
        #endregion

        #region Events
        public delegate void DocumentModifiedDelegate(GenericTextDocument document);
        public event DocumentModifiedDelegate DocumentModified;

        internal void RaiseDocumentModified(GenericTextDocument document) {
            // Inform caller
            if (this.DocumentModified != null) {
                this.DocumentModified (document);
            }
        }
        #endregion
    }
}
```

`GenericTextDocument` Třídy uvedené výše se použije v rámci tohoto článku, při práci s výběr dokumentu a externí dokumenty do aplikace Xamarin.iOS 8.

## <a name="asynchronous-file-coordination"></a>Koordinace asynchronní souboru

iOS 8 nabízí několik nových funkcí asynchronní koordinace souboru prostřednictvím nových rozhraní API souboru koordinace. Před iOS 8 všechny stávající rozhraní API souboru koordinace byly zcela synchronní. To znamená, že jste byli vývojář za implementaci vlastní pozadí služby Řízení front do souboru koordinace zabránit zablokování uživatelského rozhraní aplikace.

Nové `NSFileAccessIntent` třída obsahuje adresu URL odkazující na soubor a celou řadu možností pro ovládací prvek typu vyžaduje koordinaci. Následující kód ukazuje přesouvání souboru z jednoho umístění do druhého pomocí tříd Intent:

```csharp
// Get source options
var srcURL = NSUrl.FromFilename ("FromFile.txt");
var srcIntent = NSFileAccessIntent.CreateReadingIntent (srcURL, NSFileCoordinatorReadingOptions.ForUploading);

// Get destination options
var dstURL = NSUrl.FromFilename ("ToFile.txt");
var dstIntent = NSFileAccessIntent.CreateReadingIntent (dstURL, NSFileCoordinatorReadingOptions.ForUploading);

// Create an array
var intents = new NSFileAccessIntent[] {
    srcIntent,
    dstIntent
};

// Initialize a file coordination with intents
var queue = new NSOperationQueue ();
var fileCoordinator = new NSFileCoordinator ();
fileCoordinator.CoordinateAccess (intents, queue, (err) => {
    // Was there an error?
    if (err!=null) {
        Console.WriteLine("Error: {0}",err.LocalizedDescription);
    }
});
```

## <a name="discovering-and-listing-documents"></a>Zjišťování a zobrazení dokumentů

Způsob zjišťování a seznam dokumentů je pomocí stávajícího `NSMetadataQuery` rozhraní API. Tato část se bude zabývat nových funkcích `NSMetadataQuery` , která usnadňuje práci s dokumenty ještě jednodušší než dřív.

### <a name="existing-behavior"></a>Stávající chování

Před iOS 8 `NSMetadataQuery` bylo pomalé v sítích na změny vyzvednutí místního souboru jako například: Odstraní a vytvoří přejmenuje.

 [![](document-picker-images/image2.png "Přehled změn NSMetadataQuery místního souboru")](document-picker-images/image2.png#lightbox)

V diagramu:

1.  Pro soubory, které již existují v kontejneru aplikace `NSMetadataQuery` má existující `NSMetadata` záznamy předem vytvořené a zařazení tak, aby byly aplikace okamžitě k dispozici.
1.  Aplikace vytvoří nový soubor v kontejneru aplikace.
1.  Dochází ke zpoždění před `NSMetadataQuery` vidí změny do kontejneru aplikace a vytvoří požadované `NSMetadata` záznamu.


Kvůli zpoždění při vytváření `NSMetadata` záznamů, aplikace musí mít dva datové zdroje otevřete: jeden pro změny místního souboru a jeden pro cloud na základě změny.

### <a name="stitching"></a>Spojování

IOS 8 `NSMetadataQuery` je jednodušší použít přímo s novou funkci jako spojů:

 [![](document-picker-images/image3.png "Volá se, spojů NSMetadataQuery pomocí nové funkce")](document-picker-images/image3.png#lightbox)

Používání spojů v diagramu:

1.  Stejně jako předtím u souborů, které již existují v kontejneru aplikace `NSMetadataQuery` má existující `NSMetadata` záznamy předem vytvořené a zařazení.
1.  Aplikace vytvoří nový soubor v kontejneru aplikace pomocí koordinace souboru.
1.  Volání v kontejneru aplikace vidí změny a volání `NSMetadataQuery` vytvoření požadovaných `NSMetadata` záznamu.
1.  `NSMetadata` Záznam se vytvoří přímo po souboru a je k dispozici pro aplikaci.


Pomocí spojů aplikace není k dispozici k otevření zdroje dat pro monitorování místní a cloudové změny souborů. Nyní můžete aplikace využívají `NSMetadataQuery` přímo.

> [!IMPORTANT]
> Spojování funguje jenom v případě, že aplikace používá soubor koordinace uvedenou výše v části. Pokud se nepoužívá soubor koordinace, rozhraní API ve výchozím nastavení stávající chování pre iOS 8.




### <a name="new-ios-8-metadata-features"></a>Nové funkce metadat iOS 8

Byly přidány následující nové funkce do `NSMetadataQuery` v iOS 8:

-   `NSMetatadataQuery` můžete teď zobrazit seznam jiné než místní dokumenty uložené v cloudu.
-  Byly přidány nové rozhraní API pro přístup k informacím metadata dokumentů založené na cloudu. 
-  Je tu nový `NSUrl_PromisedItems` rozhraní API, které budou pro přístup k souboru atributy souborů, které může nebo nemusí mít obsah k dispozici místně.
-  Použít `GetPromisedItemResourceValue` metodu k získání informací o daný soubor nebo použijte `GetPromisedItemResourceValues` metodu k získání informací ve více než jeden soubor současně.


Pro práci s metadaty jsme přidali dvě nové příznaky koordinace souboru:

-   `NSFileCoordinatorReadImmediatelyAvailableMetadataOnly` 
-   `NSFileCoordinatorWriteContentIndependentMetadataOnly` 


Výše uvedené Flags není potřeba obsah souboru dokumentu být k dispozici místně je možné použít.

Následující segment kódu ukazuje, jak používat `NSMetadataQuery` pro dotazování pro konkrétní soubor existuje a soubor sestavení, pokud neexistuje:

```csharp
using System;
using System.Collections.Generic;
using System.Linq;
using System.Threading;
using Foundation;
using UIKit;
using ObjCRuntime;
using System.IO;


#region Static Properties
public const string TestFilename = "test.txt"; 
#endregion

#region Computed Properties
public bool HasiCloud { get; set; }
public bool CheckingForiCloud { get; set; }
public NSUrl iCloudUrl { get; set; }

public GenericTextDocument Document { get; set; }
public NSMetadataQuery Query { get; set; }
#endregion

#region Private Methods
private void FindDocument () {
    Console.WriteLine ("Finding Document...");

    // Create a new query and set it's scope
    Query = new NSMetadataQuery();
    Query.SearchScopes = new NSObject [] {
                NSMetadataQuery.UbiquitousDocumentsScope,
                NSMetadataQuery.UbiquitousDataScope,
                NSMetadataQuery.AccessibleUbiquitousExternalDocumentsScope
            };

    // Build a predicate to locate the file by name and attach it to the query
    var pred = NSPredicate.FromFormat ("%K == %@"
        , new NSObject[] {
            NSMetadataQuery.ItemFSNameKey
            , new NSString(TestFilename)});
    Query.Predicate = pred;

    // Register a notification for when the query returns
    NSNotificationCenter.DefaultCenter.AddObserver (this,
            new Selector("queryDidFinishGathering:"),           NSMetadataQuery.DidFinishGatheringNotification,
            Query);

    // Start looking for the file
    Query.StartQuery ();
    Console.WriteLine ("Querying: {0}", Query.IsGathering);
}

[Export("queryDidFinishGathering:")]
public void DidFinishGathering (NSNotification notification) {
    Console.WriteLine ("Finish Gathering Documents.");

    // Access the query and stop it from running
    var query = (NSMetadataQuery)notification.Object;
    query.DisableUpdates();
    query.StopQuery();

    // Release the notification
    NSNotificationCenter.DefaultCenter.RemoveObserver (this
        , NSMetadataQuery.DidFinishGatheringNotification
        , query);

    // Load the document that the query returned
    LoadDocument(query);
}

private void LoadDocument (NSMetadataQuery query) {
    Console.WriteLine ("Loading Document...");    

    // Take action based on the returned record count
    switch (query.ResultCount) {
    case 0:
        // Create a new document
        CreateNewDocument ();
        break;
    case 1:
        // Gain access to the url and create a new document from
        // that instance
        NSMetadataItem item = (NSMetadataItem)query.ResultAtIndex (0);
        var url = (NSUrl)item.ValueForAttribute (NSMetadataQuery.ItemURLKey);

        // Load the document
        OpenDocument (url);
        break;
    default:
        // There has been an issue
        Console.WriteLine ("Issue: More than one document found...");
        break;
    }
}
#endregion

#region Public Methods
public void OpenDocument(NSUrl url) {

    Console.WriteLine ("Attempting to open: {0}", url);
    Document = new GenericTextDocument (url);

    // Open the document
    Document.Open ( (success) => {
        if (success) {
            Console.WriteLine ("Document Opened");
        } else
            Console.WriteLine ("Failed to Open Document");
    });

    // Inform caller
    RaiseDocumentLoaded (Document);
}

public void CreateNewDocument() {
    // Create path to new file
    // var docsFolder = Environment.GetFolderPath (Environment.SpecialFolder.Personal);
    var docsFolder = Path.Combine(iCloudUrl.Path, "Documents");
    var docPath = Path.Combine (docsFolder, TestFilename);
    var ubiq = new NSUrl (docPath, false);

    // Create new document at path 
    Console.WriteLine ("Creating Document at:" + ubiq.AbsoluteString);
    Document = new GenericTextDocument (ubiq);

    // Set the default value
    Document.Contents = "(default value)";

    // Save document to path
    Document.Save (Document.FileUrl, UIDocumentSaveOperation.ForCreating, (saveSuccess) => {
        Console.WriteLine ("Save completion:" + saveSuccess);
        if (saveSuccess) {
            Console.WriteLine ("Document Saved");
        } else {
            Console.WriteLine ("Unable to Save Document");
        }
    });

    // Inform caller
    RaiseDocumentLoaded (Document);
}

public bool SaveDocument() {
    bool successful = false;

    // Save document to path
    Document.Save (Document.FileUrl, UIDocumentSaveOperation.ForOverwriting, (saveSuccess) => {
        Console.WriteLine ("Save completion: " + saveSuccess);
        if (saveSuccess) {
            Console.WriteLine ("Document Saved");
            successful = true;
        } else {
            Console.WriteLine ("Unable to Save Document");
            successful=false;
        }
    });

    // Return results
    return successful;
}
#endregion

#region Events
public delegate void DocumentLoadedDelegate(GenericTextDocument document);
public event DocumentLoadedDelegate DocumentLoaded;

internal void RaiseDocumentLoaded(GenericTextDocument document) {
    // Inform caller
    if (this.DocumentLoaded != null) {
        this.DocumentLoaded (document);
    }
}
#endregion
```

### <a name="document-thumbnails"></a>Miniatur dokumentů

Apple, jako je nejlepší uživatelské prostředí při výpisu dokumenty pro aplikaci použít verze Preview. Díky tomu kontextu koncoví uživatelé tak můžou rychle určit dokumentu, který chtějí pracovat.

Před iOS 8 povinné zobrazující náhledy dokumentů vlastní implementaci. Nový operační systém na iOS 8 jsou atributy systému souborů, které umožňuje vývojářům rychle pracovat s miniaturami dokumentu.

#### <a name="retrieving-document-thumbnails"></a>Načítání miniatur dokumentů 

Při volání `GetPromisedItemResourceValue` nebo `GetPromisedItemResourceValues` metody, `NSUrl_PromisedItems` rozhraní API, `NSUrlThumbnailDictionary`, je vrácena. Pouze klíč aktuálně v tento slovník je `NSThumbnial1024X1024SizeKey` a jeho odpovídající `UIImage`.

#### <a name="saving-document-thumbnails"></a>Uložení dokumentu miniatury

Nejjednodušší způsob, jak uložit miniaturu je pomocí `UIDocument`. Při volání `GetFileAttributesToWrite` metodu `UIDocument` a nastavení na miniaturu, bude automaticky uložen dokument se. Na serveru služby iCloud démon této změně najdete v článku, který se šíří do Icloudu. V systému Mac OS X miniatury jsou automaticky generovány pro vývojáře pomocí modulu plug-in rychlé vypadat.

Se základy práce s dokumenty Icloudu založené na místě, spolu s změny existujícího rozhraní API, jsme připraveni k implementaci kontroler zobrazení pro výběr dokumentu v Xamarin pro iOS 8 mobilní aplikace.


## <a name="enabling-icloud-in-xamarin"></a>Povolení Icloudu v Xamarinu

Před použitím nástroje pro výběr dokumentu v aplikaci Xamarin.iOS, je potřeba povolit ve vaší aplikaci a prostřednictvím Apple podporu serveru služby iCloud. 

Následující kroky návodu proces zřizování pro serveru služby iCloud.

1. Vytvoření kontejneru iCloud.
2. Vytvoření ID aplikace, která obsahuje na serveru služby iCloud služby App Service.
3. Vytvořit profil zřizování, který obsahuje ID této aplikace.

[Práce s funkcemi](~/ios/deploy-test/provisioning/capabilities/icloud-capabilities.md) Průvodce vás provede první dva kroky. K vytvoření zřizovacího profilu, postupujte podle kroků v [zřizovací profil](~/ios/get-started/installation/device-provisioning/index.md#provisioning-your-device) průvodce.



Následující kroky návodu procesem konfigurace vaší aplikace pro serveru služby iCloud:

Postupujte následovně:

1.  Otevřete projekt v sadě Visual Studio pro Mac nebo Visual Studio.
2.  V **Průzkumníka řešení**, klikněte pravým tlačítkem na projekt a vyberte možnosti.
3.  Dialogové okno Možnosti vyberte **aplikace pro iOS**, ujistěte se, že **identifikátor sady prostředků** odpovídá, která byla definována v **ID aplikace** vytvořené výše pro aplikaci. 
4.  Vyberte **podepsání sady prostředků aplikace pro iOS**, vyberte **Developer Identity** a **zřizovací profil** vytvořili výše.
5.  Klikněte na tlačítko **OK** tlačítko a uložte změny a zavřete dialogové okno.
6.  Klikněte pravým tlačítkem na `Entitlements.plist` v **Průzkumníka řešení** ho otevřete v editoru.

    > [!IMPORTANT]
    > V sadě Visual Studio budete muset otevřít editor oprávnění kliknutím pravým tlačítkem myši na něj, vyberete **otevřít v programu...** a vyberete Editor seznamu vlastností

7.  Zkontrolujte **povolit iCloud** , **dokumenty Icloudu** , **úložiště hodnot klíčů** a **CloudKit** .
8.  Zkontrolujte, **kontejneru** existuje pro aplikaci (vytvořené výše). Příklad: `iCloud.com.your-company.AppName`
9.  Uložte změny do souboru.

Další informace o oprávnění najdete [práce s nároky](~/ios/deploy-test/provisioning/entitlements.md) průvodce.

Pomocí výše uvedených nastavení v místě aplikace teď můžete použít cloudové dokumenty a nový kontroler zobrazení pro výběr dokumentu.

## <a name="common-setup-code"></a>Společný kód instalační program

Než začnete s kontroler zobrazení pro výběr dokumentu, je nějaký kód standardní instalační program vyžaduje. Začněte tím, že úpravy aplikace `AppDelegate.cs` souboru a nastavte ji vypadat nějak takto:

```csharp
using System;
using System.Collections.Generic;
using System.Linq;
using System.Threading;
using Foundation;
using UIKit;
using ObjCRuntime;
using System.IO;

namespace DocPicker
{

    [Register ("AppDelegate")]
    public partial class AppDelegate : UIApplicationDelegate
    {
        #region Static Properties
        public const string TestFilename = "test.txt"; 
        #endregion

        #region Computed Properties
        public override UIWindow Window { get; set; }
        public bool HasiCloud { get; set; }
        public bool CheckingForiCloud { get; set; }
        public NSUrl iCloudUrl { get; set; }

        public GenericTextDocument Document { get; set; }
        public NSMetadataQuery Query { get; set; }
        public NSData Bookmark { get; set; }
        #endregion

        #region Private Methods
        private void FindDocument () {
            Console.WriteLine ("Finding Document...");

            // Create a new query and set it's scope
            Query = new NSMetadataQuery();
            Query.SearchScopes = new NSObject [] {
                NSMetadataQuery.UbiquitousDocumentsScope,
                NSMetadataQuery.UbiquitousDataScope,
                NSMetadataQuery.AccessibleUbiquitousExternalDocumentsScope
            };

            // Build a predicate to locate the file by name and attach it to the query
            var pred = NSPredicate.FromFormat ("%K == %@",
                new NSObject[] {NSMetadataQuery.ItemFSNameKey
                , new NSString(TestFilename)});
            Query.Predicate = pred;

            // Register a notification for when the query returns
            NSNotificationCenter.DefaultCenter.AddObserver (this
                , new Selector("queryDidFinishGathering:")
                , NSMetadataQuery.DidFinishGatheringNotification
                , Query);

            // Start looking for the file
            Query.StartQuery ();
            Console.WriteLine ("Querying: {0}", Query.IsGathering);
        }


        [Export("queryDidFinishGathering:")]
        public void DidFinishGathering (NSNotification notification) {
            Console.WriteLine ("Finish Gathering Documents.");

            // Access the query and stop it from running
            var query = (NSMetadataQuery)notification.Object;
            query.DisableUpdates();
            query.StopQuery();

            // Release the notification
            NSNotificationCenter.DefaultCenter.RemoveObserver (this
                , NSMetadataQuery.DidFinishGatheringNotification
                , query);

            // Load the document that the query returned
            LoadDocument(query);
        }

        private void LoadDocument (NSMetadataQuery query) {
            Console.WriteLine ("Loading Document...");  

            // Take action based on the returned record count
            switch (query.ResultCount) {
            case 0:
                // Create a new document
                CreateNewDocument ();
                break;
            case 1:
                // Gain access to the url and create a new document from
                // that instance
                NSMetadataItem item = (NSMetadataItem)query.ResultAtIndex (0);
                var url = (NSUrl)item.ValueForAttribute (NSMetadataQuery.ItemURLKey);

                // Load the document
                OpenDocument (url);
                break;
            default:
                // There has been an issue
                Console.WriteLine ("Issue: More than one document found...");
                break;
            }
        }
        #endregion

        #region Public Methods

        public void OpenDocument(NSUrl url) {

            Console.WriteLine ("Attempting to open: {0}", url);
            Document = new GenericTextDocument (url);

            // Open the document
            Document.Open ( (success) => {
                if (success) {
                    Console.WriteLine ("Document Opened");
                } else
                    Console.WriteLine ("Failed to Open Document");
            });

            // Inform caller
            RaiseDocumentLoaded (Document);
        }

        public void CreateNewDocument() {
            // Create path to new file
            // var docsFolder = Environment.GetFolderPath (Environment.SpecialFolder.Personal);
            var docsFolder = Path.Combine(iCloudUrl.Path, "Documents");
            var docPath = Path.Combine (docsFolder, TestFilename);
            var ubiq = new NSUrl (docPath, false);

            // Create new document at path 
            Console.WriteLine ("Creating Document at:" + ubiq.AbsoluteString);
            Document = new GenericTextDocument (ubiq);

            // Set the default value
            Document.Contents = "(default value)";

            // Save document to path
            Document.Save (Document.FileUrl, UIDocumentSaveOperation.ForCreating, (saveSuccess) => {
                Console.WriteLine ("Save completion:" + saveSuccess);
                if (saveSuccess) {
                    Console.WriteLine ("Document Saved");
                } else {
                    Console.WriteLine ("Unable to Save Document");
                }
            });

            // Inform caller
            RaiseDocumentLoaded (Document);
        }

        /// <summary>
        /// Saves the document.
        /// </summary>
        /// <returns><c>true</c>, if document was saved, <c>false</c> otherwise.</returns>
        public bool SaveDocument() {
            bool successful = false;

            // Save document to path
            Document.Save (Document.FileUrl, UIDocumentSaveOperation.ForOverwriting, (saveSuccess) => {
                Console.WriteLine ("Save completion: " + saveSuccess);
                if (saveSuccess) {
                    Console.WriteLine ("Document Saved");
                    successful = true;
                } else {
                    Console.WriteLine ("Unable to Save Document");
                    successful=false;
                }
            });

            // Return results
            return successful;
        }
        #endregion

        #region Override Methods
        public override void FinishedLaunching (UIApplication application)
        {

            // Start a new thread to check and see if the user has iCloud
            // enabled.
            new Thread(new ThreadStart(() => {
                // Inform caller that we are checking for iCloud
                CheckingForiCloud = true;

                // Checks to see if the user of this device has iCloud
                // enabled
                var uburl = NSFileManager.DefaultManager.GetUrlForUbiquityContainer(null);

                // Connected to iCloud?
                if (uburl == null)
                {
                    // No, inform caller
                    HasiCloud = false;
                    iCloudUrl =null;
                    Console.WriteLine("Unable to connect to iCloud");
                    InvokeOnMainThread(()=>{
                        var okAlertController = UIAlertController.Create ("iCloud Not Available", "Developer, please check your Entitlements.plist, Bundle ID and Provisioning Profiles.", UIAlertControllerStyle.Alert);
                        okAlertController.AddAction (UIAlertAction.Create ("Ok", UIAlertActionStyle.Default, null));
                        Window.RootViewController.PresentViewController (okAlertController, true, null);
                    });
                }
                else
                {   
                    // Yes, inform caller and save location the the Application Container
                    HasiCloud = true;
                    iCloudUrl = uburl;
                    Console.WriteLine("Connected to iCloud");

                    // If we have made the connection with iCloud, start looking for documents
                    InvokeOnMainThread(()=>{
                        // Search for the default document
                        FindDocument ();
                    });
                }

                // Inform caller that we are no longer looking for iCloud
                CheckingForiCloud = false;

            })).Start();
                
        }
        
        // This method is invoked when the application is about to move from active to inactive state.
        // OpenGL applications should use this method to pause.
        public override void OnResignActivation (UIApplication application)
        {
        }
        
        // This method should be used to release shared resources and it should store the application state.
        // If your application supports background execution this method is called instead of WillTerminate
        // when the user quits.
        public override void DidEnterBackground (UIApplication application)
        {
            // Trap all errors
            try {
                // Values to include in the bookmark packet
                var resources = new string[] {
                    NSUrl.FileSecurityKey,
                    NSUrl.ContentModificationDateKey,
                    NSUrl.FileResourceIdentifierKey,
                    NSUrl.FileResourceTypeKey,
                    NSUrl.LocalizedNameKey
                };

                // Create the bookmark
                NSError err;
                Bookmark = Document.FileUrl.CreateBookmarkData (NSUrlBookmarkCreationOptions.WithSecurityScope, resources, iCloudUrl, out err);

                // Was there an error?
                if (err != null) {
                    // Yes, report it
                    Console.WriteLine ("Error Creating Bookmark: {0}", err.LocalizedDescription);
                }
            }
            catch (Exception e) {
                // Report error
                Console.WriteLine ("Error: {0}", e.Message);
            }
        }
        
        // This method is called as part of the transition from background to active state.
        public override void WillEnterForeground (UIApplication application)
        {
            // Is there any bookmark data?
            if (Bookmark != null) {
                // Trap all errors
                try {
                    // Yes, attempt to restore it
                    bool isBookmarkStale;
                    NSError err;
                    var srcUrl = new NSUrl (Bookmark, NSUrlBookmarkResolutionOptions.WithSecurityScope, iCloudUrl, out isBookmarkStale, out err);

                    // Was there an error?
                    if (err != null) {
                        // Yes, report it
                        Console.WriteLine ("Error Loading Bookmark: {0}", err.LocalizedDescription);
                    } else {
                        // Load document from bookmark
                        OpenDocument (srcUrl);
                    }
                }
                catch (Exception e) {
                    // Report error
                    Console.WriteLine ("Error: {0}", e.Message);
                }
            }

        }
        
        // This method is called when the application is about to terminate. Save data, if needed.
        public override void WillTerminate (UIApplication application)
        {
        }
        #endregion

        #region Events
        public delegate void DocumentLoadedDelegate(GenericTextDocument document);
        public event DocumentLoadedDelegate DocumentLoaded;

        internal void RaiseDocumentLoaded(GenericTextDocument document) {
            // Inform caller
            if (this.DocumentLoaded != null) {
                this.DocumentLoaded (document);
            }
        }
        #endregion
    }
}

```

> [!IMPORTANT]
> Ve výše uvedeném kódu obsahuje kód z výše uvedené části Discovering a výpis dokumenty. To je zde uvedený v celém rozsahu, jak se bude zobrazovat v aplikace skutečný. Pro zjednodušení tento příklad funguje s jeden pevně zakódované souborů (`test.txt`) pouze.

Výše uvedený kód poskytuje několik klávesové zkratky Icloudu jednotku pro snadnější pracovat ve zbývající části aplikace.

V dalším kroku přidejte následující kód všech zobrazení nebo zobrazení kontejneru, který se pomocí nástroje pro výběr dokumentu nebo práce s dokumenty založené na cloudu:

```csharp
using CloudKit;
...

#region Computed Properties
/// <summary>
/// Returns the delegate of the current running application
/// </summary>
/// <value>The this app.</value>
public AppDelegate ThisApp {
    get { return (AppDelegate)UIApplication.SharedApplication.Delegate; }
}
#endregion
```

Tento postup přidá zástupce zobrazíte `AppDelegate` a přístup k serveru služby iCloud zkratky vytvořili výše.

S tímto kódem na místě Pojďme se podívat na implementaci kontroler zobrazení pro výběr dokumentu v aplikaci Xamarin iOS 8.

## <a name="using-the-document-picker-view-controller"></a>Použití Kontroleru zobrazení pro výběr dokumentu

Před iOS 8 je velmi obtížné získat přístup k dokumentům z jiné aplikace, protože neexistoval způsob, jak zjistit dokumenty mimo aplikaci z v rámci aplikace.

### <a name="existing-behavior"></a>Stávající chování

 [![](document-picker-images/image31.png "Přehled stávající chování")](document-picker-images/image31.png#lightbox)

Pojďme se podívat na přístup k externím dokumentu před iOS 8:

1.  Nejprve uživatelé aplikaci, který původně vytvořil dokument otevřít.
1.  Dokument je vybraná a `UIDocumentInteractionController` se používá k odeslání dokumentu na novou aplikaci.
1.  Nakonec kopii původního dokumentu je umístěn v nové aplikace kontejneru.


Odtud dokument je k dispozici pro druhou aplikaci otevřít a upravit.

### <a name="discovering-documents-outside-of-an-apps-container"></a>Zjišťování dokumentů mimo kontejner aplikace

Aplikace v iOS 8, je možné získat přístup k dokumentům mimo svůj vlastní kontejner aplikace s lehkostí a elegancí:

 [![](document-picker-images/image32.png "Zjišťování dokumentů mimo kontejner aplikace")](document-picker-images/image32.png#lightbox)

Pomocí nového serveru služby iCloud výběr dokumentu ( `UIDocumentPickerViewController`), aplikace pro iOS můžete přímo zjistit a přístup mimo svůj kontejner aplikace. `UIDocumentPickerViewController` Poskytuje mechanismus pro uživateli udělit přístup k a upravit tyto zjištěné dokumenty pomocí oprávnění.

Aplikace musí přihlásit k jeho dokumentů zobrazí v na serveru služby iCloud výběr dokumentu a být k dispozici pro jiné aplikace při zjišťování a s nimi pracovat. Aby aplikace Xamarin iOS 8 a sdílet své aplikace kontejneru, upravit ho `Info.plist` v standardního textového editoru a přidejte následující dva řádky na konec slovníku (mezi `<dict>...</dict>` značek):

```xml
<key>NSUbiquitousContainerIsDocumentScopePublic</key>
<true/>
```

`UIDocumentPickerViewController` Poskytuje skvělé nové uživatelské rozhraní, který umožňuje uživateli zvolit dokumenty. Chcete-li zobrazit kontroler zobrazení pro výběr dokumentu v aplikaci Xamarin iOS 8, postupujte takto:

```csharp
using MobileCoreServices;
...

// Allow the Document picker to select a range of document types
        var allowedUTIs = new string[] {
            UTType.UTF8PlainText,
            UTType.PlainText,
            UTType.RTF,
            UTType.PNG,
            UTType.Text,
            UTType.PDF,
            UTType.Image
        };

        // Display the picker
        //var picker = new UIDocumentPickerViewController (allowedUTIs, UIDocumentPickerMode.Open);
        var pickerMenu = new UIDocumentMenuViewController(allowedUTIs, UIDocumentPickerMode.Open);
        pickerMenu.DidPickDocumentPicker += (sender, args) => {

            // Wireup Document Picker
            args.DocumentPicker.DidPickDocument += (sndr, pArgs) => {

                // IMPORTANT! You must lock the security scope before you can
                // access this file
                var securityEnabled = pArgs.Url.StartAccessingSecurityScopedResource();

                // Open the document
                ThisApp.OpenDocument(pArgs.Url);

                // IMPORTANT! You must release the security lock established
                // above.
                pArgs.Url.StopAccessingSecurityScopedResource();
            };

            // Display the document picker
            PresentViewController(args.DocumentPicker,true,null);
        };

pickerMenu.ModalPresentationStyle = UIModalPresentationStyle.Popover;
PresentViewController(pickerMenu,true,null);
UIPopoverPresentationController presentationPopover = pickerMenu.PopoverPresentationController;
if (presentationPopover!=null) {
    presentationPopover.SourceView = this.View;
    presentationPopover.PermittedArrowDirections = UIPopoverArrowDirection.Down;
    presentationPopover.SourceRect = ((UIButton)s).Frame;
}
```

> [!IMPORTANT]
> Vývojář musí volat `StartAccessingSecurityScopedResource` metodu `NSUrl` můžete získat přístup k externím dokumentu. `StopAccessingSecurityScopedResource` Musí být volána metoda uvolnění zámku zabezpečení, co nejdříve po načtení dokumentu.

### <a name="sample-output"></a>Vzorový výstup

Tady je příklad, jak by se výše uvedený kód zobrazí výběr dokumentu, při spuštění na zařízení iPhone:

1.  Uživatel spustí aplikaci, a zobrazí se hlavní rozhraní:   
 
    [![](document-picker-images/image33.png "Zobrazí se hlavní rozhraní")](document-picker-images/image33.png#lightbox)
1.  Odposlouchávání uživatele **akce** tlačítko v horní části obrazovky a se zobrazí výzva k výběru **dokumentu poskytovatele** ze seznamu dostupných zprostředkovatelů:   
 
    [![](document-picker-images/image34.png "Vybrat dokument zprostředkovatele ze seznamu dostupných zprostředkovatelů")](document-picker-images/image34.png#lightbox)
1.  **Kontroler zobrazení pro výběr dokumentu** se zobrazí pro vybrané **dokumentu poskytovatele**:   
 
    [![](document-picker-images/image35.png "Zobrazí se kontroler zobrazení pro výběr dokumentu")](document-picker-images/image35.png#lightbox)
1.  Uživatel klepne na **složku dokumentů** zobrazíte jeho obsah:   
 
    [![](document-picker-images/image36.png "Obsah složky dokumentu")](document-picker-images/image36.png#lightbox)
1.  Uživatel vybere **dokumentu** a **výběr dokumentu** je zavřený.
1.  Hlavní rozhraní se zobrazí znovu, **dokumentu** je načteno z externí kontejneru a jeho obsah zobrazí.


Skutečné zobrazení kontroler zobrazení pro výběr dokumentu závisí na dokument poskytovatelů, že uživatel má nainstalovanou na zařízení a které režimu pro výběr dokumentu se implementují. Výše uvedený příklad používá režim otevření, jiné typy režimu probereme podrobnější rozpis naleznete níže.

## <a name="managing-external-documents"></a>Správa externích dokumentů

Jak je popsáno výše, před iOS 8, aplikace může jenom přístup k dokumentům, které byly součástí jeho kontejneru aplikace. V Iosu 8 aplikace přístupné dokumenty z externích zdrojů:

 [![](document-picker-images/image37.png "Externí dokumenty přehled správy")](document-picker-images/image37.png#lightbox)

Když uživatel vybere dokument z externího zdroje, referenční dokument je zapsán do kontejneru aplikace, která odkazuje na původního dokumentu.

Jako pomoc při přidávání tato nová funkce do existující aplikace, bylo přidáno několik nových funkcí do `NSMetadataQuery` rozhraní API. Aplikace obvykle používá všudypřítomná obor dokumentu do seznamu dokumentů, kteří žijí v rámci jeho kontejneru aplikace. Pomocí tohoto oboru, pouze dokumenty v kontejneru aplikace bude pořád zobrazuje.

Pomocí nového všudypřítomná externí dokument oboru vrátí dokumenty, které za provozu mimo kontejner aplikace a vrátí metadata pro ně. `NSMetadataItemUrlKey` Bude odkazovat na adresu URL, kde se skutečně nachází dokumentu.

Někdy aplikace nechce pro práci s dokumenty neodkazují th odkaz. Místo toho aplikace chce pracovat přímo s referenčním dokumentu. Aplikace může být vhodné například pro zobrazení dokumentu ve složce vaší aplikace v uživatelském rozhraní, nebo aby uživatel mohl pohyb odkazy uvnitř složky.

IOS 8 nový `NSMetadataItemUrlInLocalContainerKey` byl poskytnut přístup přímo k referenčním dokumentu. Tento klíč odkazuje na skutečný odkaz na externím dokumentu v kontejneru aplikace.

`NSMetadataUbiquitousItemIsExternalDocumentKey` Slouží k otestování, jestli dokument je mimo kontejner aplikace. `NSMetadataUbiquitousItemContainerDisplayNameKey` Se používá pro přístup k názvu kontejneru, který je bydlení původní kopii externím dokumentu.

### <a name="why-document-references-are-required"></a>Proč jsou požadované odkazy na dokumenty

Hlavním důvodem této iOS 8 používá odkazy pro přístup k externí dokumenty je zabezpečení. Žádná aplikace je poskytnut přístup k jakékoli jiné aplikace v kontejneru. Pouze výběr dokumentu můžete to provést, protože je spuštěné mimo proces a má široký přístup k systému.

Je jediný způsob, jak získat k dokumentu mimo kontejner aplikace s použitím nástroje pro výběr dokumentu, a pokud adresy URL vrácené nástroje pro výběr oboru zabezpečení. Adresa URL obor zabezpečení obsahuje jenom dostatek informací pro vybraný dokument spolu s vymezeným oborem oprávnění nutná k aplikaci udělit přístup k tomuto dokumentu.

Je důležité si uvědomit, že pokud adresu URL obor zabezpečení se serializovat do řetězce a pak deserializovaný, informace o zabezpečení by dojít ke ztrátě a soubor by být nedostupný z adresy URL. Odkaz na dokument funkce poskytuje mechanismus pro získání zpátky k souborům, na které odkazují tyto adresy URL.

Pokud aplikace získá `NSUrl` z jednoho z referenční dokumenty již má obor zabezpečení připojené a slouží k přístupu k souboru. Z tohoto důvodu vysoce doporučuje se, že vývojáři použít `UIDocument` protože zpracovává všechna z těchto informací a procesy pro ně.

### <a name="using-bookmarks"></a>Použití záložek

Vždy není možné vytvořit výčet dokumenty aplikace chcete vrátit zpět do určitého dokumentu, například při provádění obnovení stavu. iOS 8 poskytuje mechanismus pro vytvoření záložky, jejichž přímým cílem daného dokumentu.

Následující kód vytvoří záložky z `UIDocument`společnosti `FileUrl` vlastnost:

```csharp
// Trap all errors
try {
    // Values to include in the bookmark packet
    var resources = new string[] {
        NSUrl.FileSecurityKey,
        NSUrl.ContentModificationDateKey,
        NSUrl.FileResourceIdentifierKey,
        NSUrl.FileResourceTypeKey,
        NSUrl.LocalizedNameKey
    };

    // Create the bookmark
    NSError err;
    Bookmark = Document.FileUrl.CreateBookmarkData (NSUrlBookmarkCreationOptions.WithSecurityScope, resources, iCloudUrl, out err);

    // Was there an error?
    if (err != null) {
        // Yes, report it
        Console.WriteLine ("Error Creating Bookmark: {0}", err.LocalizedDescription);
    }
}
catch (Exception e) {
    // Report error
    Console.WriteLine ("Error: {0}", e.Message);
}
```

Rozhraní API pro stávající záložky se používá k vytvoření záložky proti existující `NSUrl` , které lze uložit a načíst a zajistit tak přístup s přímým přístupem do externího souboru. Následující kód obnoví záložku, která byla vytvořena výše:

```csharp
if (Bookmark != null) {
    // Trap all errors
    try {
        // Yes, attempt to restore it
        bool isBookmarkStale;
        NSError err;
        var srcUrl = new NSUrl (Bookmark, NSUrlBookmarkResolutionOptions.WithSecurityScope, iCloudUrl, out isBookmarkStale, out err);

        // Was there an error?
        if (err != null) {
            // Yes, report it
            Console.WriteLine ("Error Loading Bookmark: {0}", err.LocalizedDescription);
        } else {
            // Load document from bookmark
            OpenDocument (srcUrl);
        }
    }
    catch (Exception e) {
        // Report error
        Console.WriteLine ("Error: {0}", e.Message);
    }
}
```

## <a name="open-vs-import-mode-and-the-document-picker"></a>Otevřít vs. Režim importu a nástroje pro výběr dokumentu

Kontroler zobrazení pro výběr dokumentu obsahuje dva různé režimy činnosti:

1.  **Otevřete režim** – v tomto režimu, když uživatel vybere a externím dokumentu, vytvoří nástroj pro výběr dokumentu záložku obor zabezpečení v kontejneru aplikace.   
 
    [![](document-picker-images/image37.png "Bezpečnostní obor záložky v kontejneru aplikace")](document-picker-images/image37.png#lightbox)
1.  **Importování** – v tomto režimu, když uživatel vybere a externím dokumentu, nástroje pro výběr dokumentu nebude vytvoření záložky, ale místo toho zkopírujte soubor do dočasné složky a poskytují přístup k aplikacím v dokumentu na tomto místě:   
 
    [![](document-picker-images/image38.png "Výběr dokumentu bude zkopírujte soubor do dočasné složky a poskytovat přístup k aplikacím v dokumentu na tomto místě")](document-picker-images/image38.png#lightbox)   
 Po ukončení aplikace z nějakého důvodu, vyprázdní dočasného umístění a soubor odebrán. Pokud aplikace potřebuje udržovat přístup k souboru, by měl vytvořit kopii a umístěte ho do svého kontejneru aplikace.


Otevřete režim je užitečný, pokud aplikace chce s jinou aplikací při spolupráci a sdílení všechny změny provedené v dokumentu s touto aplikací. Režim importu se používá při aplikace chcete sdílet své změny do dokumentu s ostatními aplikacemi.

## <a name="making-a-document-external"></a>Provedení externího dokumentu

Jak bylo uvedeno výše, aplikace pro iOS 8 nemá přístup do kontejnerů mimo svůj vlastní kontejner aplikace. Aplikace můžou zapisovat do vlastního kontejneru místně nebo do dočasného umístění, a pak pomocí režimu speciálním dokumentu s přesunout výsledný dokumentu mimo kontejner aplikace pro zvolené umístění uživatele.

Chcete-li přesunout dokument do externího umístění, postupujte takto:

1.  Nejprve vytvořte nový textový dokument v místním nebo dočasné umístění.
1.  Vytvoření `NSUrl` , která odkazuje na nový dokument.
1.  Otevřete nový kontroler zobrazení pro výběr dokumentu a předejte ji `NSUrl` s režim `MoveToService` . 
1.  Po kliknutí na nové umístění, dokument se přesunou z jeho aktuálního umístění do nového umístění.
1.  Referenční dokument se zapíšou do kontejneru aplikace aplikace tak, že soubor je pořád přístupný vytváření aplikace.


Následující kód je možné se přesunout dokument do externího umístění: `var picker = new UIDocumentPickerViewController (srcURL, UIDocumentPickerMode.MoveToService);`

Referenční dokument výše proces vrátí je přesně stejný jako jeden Autor otevřete režim výběr dokumentu. Existují však případů, kdy aplikace možná budete chtít přesunout dokument bez zachování na ni odkaz.

Pokud chcete přesunout dokument bez generování odkazu, použijte `ExportToService` režimu. Příklad: `var picker = new UIDocumentPickerViewController (srcURL, UIDocumentPickerMode.ExportToService);`

Při použití `ExportToService` režimu, je dokument zkopírován do externí kontejneru a stávající kopie zůstane v původním umístění.

## <a name="document-provider-extensions"></a>Rozšíření poskytovatelů dokumentu

Se systémem iOS 8 Apple chce, aby koncový uživatel bude mít přístup k žádným své dokumenty založené na cloudu, bez ohledu na to, kde jsou skutečně existuje. K dosažení tohoto cíle, iOS 8 obsahuje nový mechanismus rozšíření zprostředkovatel dokumentu.

### <a name="what-is-a-document-provider-extension"></a>Co je rozšíření zprostředkovatel dokumentu?

Jednoduše řečeno, rozšíření zprostředkovatel dokumentu je způsob pro vývojáře nebo třetí strany, zadejte do úložiště aplikací alternativní dokumentu, který je přístupný přesně stejným způsobem jako existující úložiště iCloud.

Uživatele můžete vybrat jednu z těchto umístění alternativní úložiště z nástroje pro výběr dokumentu a ten může použít přesně stejnou režimy přístupu (otevřít, importovat, přesunout nebo Export) pro práci se soubory v dané oblasti.

Tato možnost je implementovaná pomocí dvou různých rozšíření:

-  **Rozšíření výběr dokumentu** – poskytuje `UIViewController` podtřídu, která poskytuje grafické rozhraní pro uživatele k výběru dokument z umístění alternativní úložiště. Jako součást kontroler zobrazení pro výběr dokumentu se zobrazí tato podtřídy.
-  **Zadejte příponu souboru** – to je rozšířením bez uživatelského rozhraní, které se zabývají vlastně i obsah souborů. Tato rozšíření jsou k dispozici prostřednictvím souboru koordinace ( `NSFileCoordinator` ). Toto je jiný důležité případ, ve kterém jsou vyžadována koordinace souboru.


Následující diagram znázorňuje tok typické dat při práci s rozšíření poskytovatelů dokumentu:

 [![](document-picker-images/image39.png "Tento diagram znázorňuje tok typické dat při práci s rozšíření poskytovatelů dokumentu")](document-picker-images/image39.png#lightbox)

Spustí následující proces:

1.  Aplikace prezentuje Kontroleru pro výběr dokumentu umožňující uživateli vybrat soubor, který chcete pracovat.
1.  Uživatel vybere alternativní umístění a vlastní `UIViewController` rozšíření je volána k zobrazení uživatelského rozhraní.
1.  Uživatel vybere soubor z tohoto umístění a adresa URL se předá zpět do nástroje pro výběr dokumentu.
1.  Výběr dokumentu vybere adresu URL k souboru a vrátí ji do aplikace pro uživatele na.
1.  Adresa URL je předán koordinátor souboru k vrácení souborů obsahu do aplikace.
1.  Koordinátor soubor volá vlastní rozšíření zprostředkovatel souborů pro načtení souboru.
1.  Do souboru koordinátor se vrátí obsah souboru.
1.  Aplikace se vrátí obsah souboru.


### <a name="security-and-bookmarks"></a>Zabezpečení a záložky

Tato část zabere rychlý pohled na zabezpečení a trvalý soubor přístupu prostřednictvím záložky spolupracuje s rozšíření poskytovatelů dokumentu. Na rozdíl od na serveru služby iCloud poskytovatele dokumentu, které automaticky uloží do kontejneru aplikace zabezpečení a záložky, rozšíření poskytovatelů dokument není protože nejsou součástí systému odkaz na dokument.

Příklad: v nastavení organizace, která poskytuje zabezpečené úložiště dat vlastní pořádaného microsoftem, správci nechtějí důvěrné firemní informace získat přístup nebo zpracována v Icloudu veřejné servery. Proto nelze použít předdefinovaný systémový odkaz na dokument.

Systém Záložka je stále možné a zodpovídá rozšíření zprostředkovatel souborů správně zpracovat adresu URL označenou záložkou a doplňujte a vrátí obsah dokumentu, na kterou se odkazuje.

Z bezpečnostních důvodů má iOS 8 izolační vrstvy, která udržuje informace o tom, které má aplikace přístup ke které identifikátor uvnitř kterého soubor zprostředkovatele. Je třeba poznamenat, že tato vrstva izolace řídí všechny přístup k souborům.

Následující diagram znázorňuje tok dat při práci s záložek a poskytovatele rozšíření dokumentu:

 [![](document-picker-images/image40.png "Tento diagram znázorňuje tok dat při práci s záložek a poskytovatele rozšíření dokumentu")](document-picker-images/image40.png#lightbox)

Spustí následující proces:

1.  Aplikace je zaujmout na pozadí a musí zachovat jeho stav. Volá `NSUrl` k vytvoření záložky do souboru v alternativní úložiště.
1.  `NSUrl` volá rozšíření zprostředkovatel souborů pro získání trvalé adresy URL dokumentu. 
1.  Rozšíření zprostředkovatel souborů vrátí adresu URL jako řetězec `NSUrl` .
1.  `NSUrl` Obsahuje ureitou adresu URL do záložku a vrátí ji do aplikace.
1.  Když se aplikace se probudí z probíhá na pozadí a je potřeba obnovit stav, předá záložku na `NSUrl` .
1.  `NSUrl` volá rozšíření zprostředkovatel souborů s adresou URL souboru.
1.  Rozšíření zprostředkovatel souborů přistoupí k souboru a vrátí jeho umístění souboru, který se `NSUrl` .
1.  Umístění souboru je spojeny s informacemi o zabezpečení a vrátí aplikaci.


Z tohoto místa můžete aplikace přístup k souboru a s ním pracovat běžným způsobem.

### <a name="writing-files"></a>Zápis do souborů

Tato část zabere rychlý přehled jak zápis souborů do alternativního umístění pomocí rozšíření zprostředkovatel dokumentu funguje. Aplikace pro iOS pomocí souboru koordinace informace uložit na disk v kontejneru aplikace. Krátce po souboru byla úspěšně zapsána, rozšíření zprostředkovatel souborů budete informováni o změnu.

V tomto okamžiku rozšíření zprostředkovatel souborů můžete spustit nahrávání souboru do alternativního umístění (nebo označte ho jako změny a která vyžaduje odeslání).

### <a name="creating-new-document-provider-extensions"></a>Vytváření nových dokumentů poskytovatele rozšíření

Vytvoření nového dokumentu rozšíření poskytovatelů je mimo rozsah této úvodní článek. Tyto informace je k dispozici zde k zobrazení, podle rozšíření, která uživatel byl načten v zařízení s Iosem, aplikace může mít přístup k umístění úložiště dokumentů mimo Apple zadané umístění serveru služby iCloud.

Vývojáři měli vědět o této skutečnosti, při použití nástroje pro výběr dokumentu a práce s externí dokumenty. Neměli předpokládají tyto dokumentu jsou hostované v Icloudu.

Další informace o vytvoření poskytovatele úložiště nebo rozšíření výběr dokumentu, najdete v tématu [Úvod do rozšíření aplikace](~/ios/platform/extensions.md) dokumentu.

## <a name="migrating-to-icloud-drive"></a>Migrace icloud jednotky

V systému iOS 8 uživatelé mohou pokračovat v používání existujícího serveru služby iCloud dokumenty systému použít v iOS 7 (a staršími systémy) nebo můžete vybrat k migraci stávajících dokumentů do nové serveru služby iCloud mechanismu.

Na Mac OS X Yosemite Apple neposkytuje zpětné kompatibility tak, aby všechny dokumenty musí migrovat na serveru služby iCloud jednotky nebo se už aktualizovat napříč zařízeními.

Po migraci pod uživatelským účtem Icloudu jednotky uvidí pouze zařízení pomocí serveru služby iCloud jednotky šíření změny dokumentů mezi tato zařízení.

> [!IMPORTANT]
> Vývojáři měli vědět, že nové funkce popsané v tomto článku jsou k dispozici pouze pokud účet uživatele se migroval na disku serveru služby iCloud. 

## <a name="summary"></a>Souhrn

V tomto článku zahrnují změny existujícího serveru služby icloud rozhraní API nutné k podpoře serveru služby iCloud jednotky a nový kontroler zobrazení pro výběr dokumentu. Které soubor koordinaci a proč je důležité při práci s dokumenty založené na cloudu. Má zahrnuté nastavení povolit dokumenty cloudové aplikace pro Xamarin.iOS a daný úvodní Přehled práce s dokumenty mimo kontejner aplikace vaší aplikace pomocí kontroler zobrazení pro výběr dokumentu.

Kromě toho tento článek stručně popisuje rozšíření poskytovatelů dokumentu a proč vývojář by měl být vědomi, při psaní aplikace, které dokáže zpracovat dokumentů založené na cloudu.

## <a name="related-links"></a>Související odkazy

- [DocPicker (ukázka)](https://developer.xamarin.com/samples/monotouch/ios8/DocPicker/)
- [Úvod do iOSu 8](~/ios/platform/introduction-to-ios8.md)
- [Úvod do rozšíření aplikace](~/ios/platform/extensions.md)
