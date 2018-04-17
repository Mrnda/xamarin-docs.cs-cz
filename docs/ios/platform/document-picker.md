---
title: Výběr dokumentu.
description: Řadiče zobrazení dokumentu výběr udělí uživatelům přístup k souborům mimo izolovaného prostoru aplikace. Je jednoduchý mechanismus pro sdílení dokumentů mezi aplikacemi. Umožňuje také složitějších pracovních postupů, protože uživatelé mohou upravovat jednotlivý dokument s více aplikacemi. Tento článek obsahuje úvod do aplikace pro Xamarin.iOS pomocí nástroje pro výběr dokumentu a změny v dokumentech Icloudu nutné ho podporují.
ms.prod: xamarin
ms.assetid: 89539D79-BC6E-4A3E-AEC6-69D9A6CC6818
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.openlocfilehash: d9b98611c7d269e590ce6fe2ce0270ef71dacf1e
ms.sourcegitcommit: bc39d85b4585fcb291bd30b8004b3f7edcac4602
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/16/2018
---
# <a name="document-picker"></a>Výběr dokumentu.

_Řadiče zobrazení dokumentu výběr udělí uživatelům přístup k souborům mimo izolovaného prostoru aplikace. Je jednoduchý mechanismus pro sdílení dokumentů mezi aplikacemi. Umožňuje také složitějších pracovních postupů, protože uživatelé mohou upravovat jednotlivý dokument s více aplikacemi. Tento článek obsahuje úvod do aplikace pro Xamarin.iOS pomocí nástroje pro výběr dokumentu a změny v dokumentech Icloudu nutné ho podporují._

Nástroje pro výběr dokumentu umožňuje dokumenty ke sdílení mezi aplikacemi. Tyto dokumenty můžou být uložená v Icloudu nebo v adresáři jinou aplikaci. Dokumenty jsou sdíleny prostřednictvím sadu [dokumentu poskytovatele rozšíření](~/ios/platform/extensions.md) uživatel nainstaloval na svém zařízení. 

Z důvodu je obtížné zachovat dokumenty synchronizována v rámci aplikace a cloudu ale vést k určité množství potřebné složitost.

## <a name="requirements"></a>Požadavky

Pokud chcete provést kroky uvedené v tomto článku se vyžaduje následující text:

-  **Xcode 7 a iOS 8 nebo novější** – společnosti Apple Xcode 7 a iOS 8 nebo novější rozhraní API muset být nainstalovaná a nakonfigurovaná na počítači pro vývojáře.
-  **Visual Studio nebo Visual Studio pro Mac** – musí být nainstalována nejnovější verze sady Visual Studio pro Mac.
-  **Zařízení iOS** – zařízení se systémem iOS se systémem iOS 8 nebo novější.

## <a name="changes-to-icloud"></a>Změny na serveru služby iCloud

Pokud chcete implementovat nové funkce nástroje pro výběr dokumentu, byly provedeny následující změny icloudem společnosti Apple služby:

-  Na serveru služby iCloud démon má byla kompletně přepsaná pomocí CloudKit.
-  Existující Icloudu, kterou je funkce přejmenovat Icloudu jednotky.
-  Byla přidána podpora pro operačního systému Microsoft Windows na serveru služby iCloud.
-  V nástroji hledání Mac OS se přidal k složce serveru služby iCloud.
-  zařízení s iOS můžete přístup k obsahu složky systému Mac OS serveru služby iCloud.

> [!IMPORTANT]
> Apple [poskytuje nástroje](https://developer.apple.com/support/allowing-users-to-manage-data/) , což vývojářům správně zpracovat Evropské unie obecné Data Protection nařízení (GDPR).

## <a name="what-is-a-document"></a>Co je dokument?

Při odkazování na dokument v Icloudu, je jeden, samostatné entity a by měl být považována jako takový uživatel. Uživatel může chtít změnit dokument nebo sdílet s jinými uživateli (například pomocí e-mailu).

Existuje několik typů souborů, že uživatel okamžitě rozpozná jako dokumenty, jako je například stránky, soubory //Build nebo čísla. Však není omezen na tento koncept serveru služby iCloud. Například stav hry (například Šachy shoda) můžete zacházet jako dokument a uložené v serveru služby iCloud. Tento soubor by prošel mezi zařízení uživatele a povolení jejich vyzvednutí hry, kde skončil na jiném zařízení.

## <a name="dealing-with-documents"></a>Plánování práce s dokumenty

Předtím, než začnete je kód potřebný k pomocí nástroje pro výběr dokumentu s Xamarinem, v tomto článku budete tak, aby pokrývalo osvědčené postupy pro práci s Icloudem dokumenty a některé změny provedené v stávajících rozhraní API potřebné k podpoře nástroje pro výběr dokumentu.

### <a name="using-file-coordination"></a>Pomocí souboru spolupráce

Vzhledem k tomu, že soubor můžete upravit z několika různých místech, použije koordinaci předchází se tak ztrátě dat.

 [![](document-picker-images/image1.png "Pomocí souboru spolupráce")](document-picker-images/image1.png#lightbox)

Podívejme se na obrázku výše:

1.  Zařízení se systémem iOS pomocí souboru koordinaci vytvoří nový dokument a uloží do složky serveru služby iCloud.
2.  Icloudu změněný soubor uloží do cloudu pro distribuci ke každé zařízení.
3.  Připojené Mac vidí změněný soubor v na serveru služby iCloud složky a používá koordinaci souborů ke zkopírování změny do souboru.
4.  Zařízení není pomocí souboru koordinaci provede změny do souboru a uloží do složky serveru služby iCloud. Tyto změny jsou okamžitě replikovat do jiných zařízení.

Předpokládejme původní zařízení s iOS nebo Mac se úpravy souboru, nyní se jejich změny ztráty a přepsat verzi souboru ze nekoordinovaná zařízení. Pokud chcete zabránit ztrátě dat, je soubor koordinace musí při práci s dokumenty založená na cloudu.

### <a name="using-uidocument"></a>Pomocí UIDocument

 `UIDocument` Díky jednoduché věcí (nebo `NSDocument` v systému macOS) pomocí tohoto postupu všechny lifting těžký pro vývojáře. Poskytuje vytvořené v souboru koordinaci s fronty pozadí udržovat blokovat uživatelského rozhraní aplikace.

 `UIDocument` zpřístupní několika vyžaduje vysoké úrovně rozhraní API, která usnadňují náročnost vývoje aplikace Xamarin pro všechny vývojáře účel.

Následující kód vytvoří podtřídou třídy `UIDocument` implementovat obecné dokument založený na textu, který slouží k uložení a načtení textu ze serveru služby iCloud:

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

`GenericTextDocument` Třídy uvedené výše se použije v tomto článku při práci s výběr dokumentu a externí dokumentů v aplikaci Xamarin.iOS 8.

## <a name="asynchronous-file-coordination"></a>Asynchronní souboru spolupráce

iOS 8 poskytuje několik nových funkcí asynchronní koordinaci souboru prostřednictvím nových rozhraní API koordinaci souboru. Před iOS 8 byly všech stávajících rozhraní API souboru koordinaci zcela synchronní. Vynutila si, že vývojář je zodpovědná za implementaci vlastní pozadí služby Řízení front zabránit souboru koordinaci blokování uživatelského rozhraní aplikace.

Nové `NSFileAccessIntent` třída obsahuje adresu URL odkazující na soubor a celou řadu možností pro řízení typu koordinaci vyžaduje. Následující kód ukazuje přesun souboru z jednoho umístění do druhého pomocí tříd Intent:

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

## <a name="discovering-and-listing-documents"></a>Vyhledávání a výpisy dokumenty

Je způsob, jak zjistit a seznam dokumentů pomocí stávající `NSMetadataQuery` rozhraní API. Tato část popisuje nové funkce přidané do `NSMetadataQuery` , usnadnění práce s dokumenty i jednodušší než dřív.

### <a name="existing-behavior"></a>Chování existující

Před iOS 8 `NSMetadataQuery` bylo pomalé v sítích na změny ve výstupní místního souboru jako například: Odstraní, vytvoří a přejmenuje.

 [![](document-picker-images/image2.png "Přehled změny NSMetadataQuery místního souboru")](document-picker-images/image2.png#lightbox)

V diagramu:

1.  Pro soubory, které již existují v kontejneru aplikace `NSMetadataQuery` má existující `NSMetadata` záznamů předem vytvořené a zařazovány do fronty, takže jsou k dispozici okamžitě k aplikaci.
1.  Aplikace vytvoří nový soubor v kontejneru aplikace.
1.  Dochází ke zpoždění před `NSMetadataQuery` uvidí úpravy ke kontejneru aplikace a vytvoří požadované `NSMetadata` záznamu.


Z důvodu zpoždění při vytváření `NSMetadata` záznamu, aplikace měla mít dva datové zdroje otevřete: jeden pro změny místního souboru a jeden pro cloud na základě změny.

### <a name="stitching"></a>Ve hřbetu

V iOS 8 `NSMetadataQuery` je jednodušší použít přímo s novou funkci s názvem Stitching:

 [![](document-picker-images/image3.png "NSMetadataQuery pomocí nové funkce volá Stitching")](document-picker-images/image3.png#lightbox)

Pomocí Stitching v diagramu:

1.  Jako předtím, pro soubory, které již existují v kontejneru aplikace `NSMetadataQuery` má existující `NSMetadata` záznamů předem vytvořené a zařazovány do fronty.
1.  Aplikace vytvoří nový soubor v kontejneru aplikace pomocí souboru spolupráce.
1.  Háku v kontejneru aplikace uvidí úpravy a volání `NSMetadataQuery` vytvoření požadovaných `NSMetadata` záznamu.
1.  `NSMetadata` Záznamu je vytvořen přímo po souboru a je k dispozici pro aplikaci.


Pomocí Stitching aplikace už má otevřít zdroj dat pro monitorování místní a cloudové na základě změn souborů. Teď můžete aplikaci spoléhají na `NSMetadataQuery` přímo.

> [!IMPORTANT]
> Ve hřbetu funguje jenom v případě aplikace používá soubor koordinaci uvedenou výše v části. Pokud soubor koordinaci není používán, rozhraní API výchozí chování pro iOS 8 existující před.




### <a name="new-ios-8-metadata-features"></a>Nové funkce Metadata iOS 8

Následující nové funkce přidané do `NSMetadataQuery` v iOS 8:

-   `NSMetatadataQuery` Nyní můžete seznam nemístních dokumenty uložené v cloudu.
-  Přidaná nová rozhraní API pro přístup k informacím metadata v dokumentech cloudové. 
-  Nová `NSUrl_PromisedItems` rozhraní API, které budou pro přístup k souboru atributy souborů, které může nebo nemusí mít k dispozici obsah místně.
-  Použít `GetPromisedItemResourceValue` metodu za účelem získání informací o daného souboru nebo pomocí `GetPromisedItemResourceValues` metoda získat informace o na více než jeden soubor současně.


Pro práci s metadaty byly přidány dva nové příznaky koordinaci souboru:

-   `NSFileCoordinatorReadImmediatelyAvailableMetadataOnly` 
-   `NSFileCoordinatorWriteContentIndependentMetadataOnly` 


Výše uvedené příznaků obsah souboru dokumentu nemusí být k dispozici místně pro ně má být použit.

Následující segment kódu ukazuje, jak používat `NSMetadataQuery` pro dotazování existenci konkrétní soubor a soubor sestavení, pokud neexistuje:

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

Apple domnívá, že nejlepších výsledků při výpisu dokumentů pro aplikaci je pomocí verze Preview. Díky tomu kontext koncového uživatele, takže identifikují můžete rychle dokumentu, který chtějí pracovat.

Před iOS 8 zobrazující náhledy dokumentů vyžaduje vlastní implementaci. Nový iOS 8 jsou atributy systému souborů, které umožňuje vývojářům snadno pracovat s miniatur dokumentů.

#### <a name="retrieving-document-thumbnails"></a>Načítání miniatur dokumentů 

Při volání `GetPromisedItemResourceValue` nebo `GetPromisedItemResourceValues` metody, `NSUrl_PromisedItems` rozhraní API, `NSUrlThumbnailDictionary`, je vrácena. Pouze klíč právě tohoto slovníku `NSThumbnial1024X1024SizeKey` a jeho odpovídající `UIImage`.

#### <a name="saving-document-thumbnails"></a>Ukládání miniatur dokumentů

Nejjednodušší způsob, jak uložit na miniaturu je pomocí `UIDocument`. Při volání `GetFileAttributesToWrite` metodu `UIDocument` a nastavení miniaturu, ji budou automaticky uloženy po souboru dokumentu. Na serveru služby iCloud démon se zobrazit tato změna a rozšířit na serveru služby iCloud. V systému Mac OS X miniatury jsou automaticky generované pro vývojáře modulu plug-in rychlé vypadat.

Se základy práce s dokumenty Icloudu založené na místě změny existujícího rozhraní API, jsme připraveni k implementaci dokumentu výběr View Controller v Xamarin iOS 8 mobilní aplikace.


## <a name="enabling-icloud-in-xamarin"></a>Povolení Icloudu v Xamarinu

Před použitím nástroje pro výběr dokumentu v aplikaci Xamarin.iOS, musí být povolena ve vaší aplikaci a prostřednictvím Apple podporu serveru služby iCloud. 

Následující kroky návodu proces zřizování pro serveru služby iCloud.

1. Vytvořte serveru služby iCloud kontejneru.
2. Vytvoření ID aplikace, která obsahuje na serveru služby iCloud služby App Service.
3. Vytvořit profil zřizování, který zahrnuje číslem ID této aplikace.

[Práce s možností](~/ios/deploy-test/provisioning/capabilities/icloud-capabilities.md) Průvodce vás provede první dva kroky. Pokud chcete vytvořit profil pro zřizování, postupujte podle kroků v [profil zřizování](~/ios/get-started/installation/device-provisioning/index.md#Provisioning_Profile) průvodce.



Následující kroky návodu proces konfigurace aplikace pro serveru služby iCloud:

Postupujte takto:

1.  Otevřete projekt v sadě Visual Studio pro Mac nebo Visual Studio.
2.  V **Průzkumníku**, klikněte pravým tlačítkem na projekt a vyberte možnosti.
3.  V dialogové okno Možnosti vyberte **iOS aplikace**, ujistěte se, že **identifikátor svazku** odpovídá ten, který byl definován v **ID aplikace** vytvořili výše pro aplikaci. 
4.  Vyberte **iOS podepisování sady**, vyberte **vývojáře Identity** a **profil zřizování** vytvořili výše.
5.  Klikněte **OK** tlačítko Uložit změny a zavřete dialogové okno.
6.  Klikněte pravým tlačítkem na `Entitlements.plist` v **Průzkumníku řešení** a otevře se v editoru.

    > [!IMPORTANT]
    > V sadě Visual Studio budete muset otevřít editor oprávnění kliknutím pravým tlačítkem myši, vyberte **otevřít v programu...** a výběrem Editor seznamu vlastností

7.  Zkontrolujte **povolit Icloudu** , **Icloudu dokumenty** , **klíč hodnota úložiště** a **CloudKit** .
8.  Ujistěte se, **kontejneru** existuje pro aplikaci (jak vytvořili výše). Příklad: `iCloud.com.your-company.AppName`
9.  Uložte změny do souboru.

Další informace o oprávnění najdete v části [práce oprávnění](~/ios/deploy-test/provisioning/entitlements.md) průvodce.

K nastavení výše na místě aplikace teď můžete použít cloudové dokumenty a nového řadiče zobrazení výběr dokumentu.

## <a name="common-setup-code"></a>Běžné instalační kód

Než začnete s řadičem dokumentu výběr zobrazení, se vyžaduje kód některé standardní instalace. Začněte tím, že úpravy aplikace `AppDelegate.cs` souboru a nastavit jej vypadat třeba takto:

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
> Ve výše uvedeném kódu obsahuje kód z výše uvedené části Discovering a výpis dokumentů. Zobrazí se zde jako celek, jak se objevuje v aplikaci skutečný. Pro jednoduchost, tento příklad funguje jeden, pevně souborem (`test.txt`) jenom.

Výše uvedený kód zpřístupňuje několik zástupce serveru služby iCloud jednotky je mohli snadněji pracovat ve zbývající části aplikace.

Dál přidejte následující kód k žádnému zobrazení nebo zobrazení kontejneru, který bude pomocí nástroje pro výběr dokumentu nebo práce s dokumenty založená na cloudu:

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

Tento postup přidá zástupce pro zajištění `AppDelegate` a přístup k serveru služby iCloud zástupce vytvořili výše.

S tímto kódem na místě Podívejme se na implementaci řadiče zobrazení výběr dokumentu v aplikaci Xamarin iOS 8.

## <a name="using-the-document-picker-view-controller"></a>Pomocí řadiče zobrazení dokumentu výběr.

Před iOS 8 se velmi obtížné přístup k dokumentům z jiné aplikace, protože žádný způsob, jak zjistit dokumenty mimo aplikaci z aplikace.

### <a name="existing-behavior"></a>Chování existující

 [![](document-picker-images/image31.png "Existující chování – přehled")](document-picker-images/image31.png#lightbox)

Podívejme se na přístup k externím dokumentu před iOS 8:

1.  Nejdřív by uživatel musel otevřít aplikace, který původně vytvořil v dokumentu.
1.  Dokument je vybraná a `UIDocumentInteractionController` se používá k odeslání dokumentu do nové aplikace.
1.  Nakonec kopii původního dokumentu je umístěn v kontejneru novou aplikaci.


Zde je k dispozici pro druhý aplikace otevírat a upravovat v dokumentu.

### <a name="discovering-documents-outside-of-an-apps-container"></a>Zjišťování dokumenty mimo kontejner aplikace

Aplikace v iOS 8, je moct snadno přístup k dokumentům mimo svůj vlastní aplikace kontejneru:

 [![](document-picker-images/image32.png "Zjišťování dokumenty mimo kontejner aplikace")](document-picker-images/image32.png#lightbox)

Použití nového serveru služby iCloud výběr dokumentu ( `UIDocumentPickerViewController`), aplikace pro iOS můžete přímo zjistit a využít mimo jeho kontejneru aplikace. `UIDocumentPickerViewController` Poskytuje mechanismus pro uživatele a udělit přístup k upravit ty zjištěna dokumentů prostřednictvím oprávnění.

Aplikace musí přihlásit k jeho dokumentů zobrazí v na serveru služby iCloud výběr dokumentu a být k dispozici pro jiné aplikace zjistit a pracovat s nimi. Má aplikace Xamarin iOS 8 sdílet své aplikace kontejneru, upravit `Info.plist` v standardního textového editoru soubor a přidejte následující dva řádky do dolní části slovníku (mezi `<dict>...</dict>` značky):

```xml
<key>NSUbiquitousContainerIsDocumentScopePublic</key>
<true/>
```

`UIDocumentPickerViewController` Poskytuje skvělé nové uživatelské rozhraní, která umožňuje uživatelům zvolit dokumenty. Pokud chcete zobrazit řadiče zobrazení výběr dokumentu v aplikaci Xamarin iOS 8, postupujte takto:

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
> Vývojář musí volat `StartAccessingSecurityScopedResource` metodu `NSUrl` můžete získat přístup k externím dokumentu. `StopAccessingSecurityScopedResource` Musí být volána metoda uvolnit zámek zabezpečení při načtení dokumentu.

### <a name="sample-output"></a>Vzorový výstup

Tady je příklad, jakým způsobem se výše uvedený kód by zobrazení dokumentu výběr při spuštění na zařízení iPhone:

1.  Uživatel spustí aplikaci, a zobrazí se hlavní rozhraní:   
 
    [![](document-picker-images/image33.png "Zobrazí se hlavní rozhraní")](document-picker-images/image33.png#lightbox)
1.  Odposlouchávání uživatele **akce** tlačítka v horní části obrazovky a se zobrazí výzva k výběru **dokumentu zprostředkovatele** ze seznamu dostupných zprostředkovatelů:   
 
    [![](document-picker-images/image34.png "Vyberte zprostředkovatele dokumentu ze seznamu dostupných zprostředkovatelů")](document-picker-images/image34.png#lightbox)
1.  **Dokumentu výběr View Controller** se zobrazí pro vybrané **dokumentu zprostředkovatele**:   
 
    [![](document-picker-images/image35.png "Výběr řadiče zobrazení dokumentu se zobrazí.")](document-picker-images/image35.png#lightbox)
1.  Uživatel klepnutím na **složku dokumentů** zobrazíte její obsah:   
 
    [![](document-picker-images/image36.png "Obsah dokumentu složky")](document-picker-images/image36.png#lightbox)
1.  Uživatel vybere **dokumentu** a **dokumentu výběr** je uzavřený.
1.  Hlavní rozhraní se zobrazí znovu, **dokumentu** se načtou z externí kontejneru a její obsah zobrazí.


Skutečné zobrazení řadiče zobrazení dokumentu výběr závisí na poskytovateli dokumentu, že uživatel nainstaloval na zařízení a které režim výběru dokumentu byla implementace. Výše uvedený příklad používá režim otevření, jiné typy režimu budou popsané v níže uvedené podrobnosti.

## <a name="managing-external-documents"></a>Správa externích dokumentů

Jak je popsáno výše, před iOS 8, aplikace by mohla přístup jenom k dokumenty, které byly součástí jeho kontejneru aplikace. V iOS 8 aplikace můžete přístup k dokumentům z externích zdrojů:

 [![](document-picker-images/image37.png "Externí dokumenty přehled správy")](document-picker-images/image37.png#lightbox)

Když uživatel vybere dokumentu z externího zdroje, referenční dokument je zapsán do kontejneru aplikace, který odkazuje na původního dokumentu.

Jako pomoc při přidávání nové možnost do existující aplikace, některé nové funkce přidané `NSMetadataQuery` rozhraní API. Obvykle se aplikace používá oboru Všudypřítomný dokumentu do seznamu dokumentů, které za provozu v rámci příslušného kontejneru aplikace. Pomocí tohoto oboru, jen na dokumenty v kontejneru aplikace bude nezobrazí.

Pomocí nového Všudypřítomný externí oboru dokumentu vrátí dokumenty, které za provozu mimo kontejneru aplikace a vrátí metadata pro ně. `NSMetadataItemUrlKey` Bude odkazovat na adresu URL, kde se ve skutečnosti nachází v dokumentu.

Aplikace nemá někdy chtějí pracovat s dokumenty na odkazuje tý odkazu. Místo toho aplikace chce pracovat přímo v referenčním dokumentu. Aplikace může například chtít zobrazit dokument ve složce aplikace v uživatelském rozhraní, nebo chcete umožnit uživatelům pohyb odkazy do složky.

V iOS 8 nový `NSMetadataItemUrlInLocalContainerKey` poskytuje přímý přístup v referenčním dokumentu. Tento klíč odkazuje na skutečný odkaz na externí dokumentu v kontejneru aplikace.

`NSMetadataUbiquitousItemIsExternalDocumentKey` Slouží k otestování, jestli dokument je externí do kontejneru aplikace. `NSMetadataUbiquitousItemContainerDisplayNameKey` Se používá pro přístup k názvu kontejneru, který se nachází původní kopii externí dokumentu.

### <a name="why-document-references-are-required"></a>Proč se požadované odkazy na dokumenty

Hlavním důvodem této iOS 8 používá odkazy pro přístup k externí dokumenty je zabezpečení. Žádná aplikace je poskytnut přístup k žádné jiné aplikace kontejneru. Pouze výběr dokumentu to udělat, protože je spuštěné out-of-process a má široký přístup k systému.

Jediným způsobem, jak získat k dokumentu mimo kontejneru aplikace je pomocí nástroje pro výběr dokumentu, a pokud adresa URL vrácené nástroje pro výběr obor zabezpečení. Adresa URL obor zabezpečení obsahuje právě dostatek informací pro vybraný dokument spolu s vymezená práva potřebná k udělení přístupu aplikaci v dokumentu.

Je důležité si uvědomit, že pokud adresu URL obor zabezpečení byl serializován do řetězce a pak deserializovaný, informace o zabezpečení by dojít ke ztrátě a soubor by být nedostupný z adresy URL. Funkce odkaz na dokument poskytuje mechanismus pro návrat k souborům ukazující na tyto adresy URL.

Ano, pokud aplikace získá `NSUrl` z jednoho z odkazů na dokumenty, již má k oboru zabezpečení, který je připojen a slouží k přístupu k souboru. Z tohoto důvodu důrazně doporučujeme, vývojáři použít `UIDocument` protože zpracovává všechny této informace a procesy pro ně.

### <a name="using-bookmarks"></a>Použití záložek

Vždy není možné provést výčet dokumenty aplikace nelze vrátit zpět k určitému dokumentu, například při provádění obnovení stavu. iOS 8 poskytuje mechanismus pro vytvoření záložky, jejichž přímým cílem daného dokumentu.

Následující kód vytvoří záložku z `UIDocument`na `FileUrl` vlastnost:

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

Existujícího rozhraní API záložku se používá k vytvoření záložku proti existující `NSUrl` , mohou být uloženy a načteny poskytnout přímý přístup k externí soubor. Následující kód obnoví záložka, která byla vytvořena výše:

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

## <a name="open-vs-import-mode-and-the-document-picker"></a>Otevřete vs. Výběr dokumentu a režimu import.

Výběr řadiče zobrazení dokumentu funkce dvou různých režimech:

1.  **Otevřete režimu** – v tomto režimu při uživatel vybere a externí dokumentu, nástroje pro výběr dokumentu vytvořte záložku obor zabezpečení v kontejneru aplikace.   
 
    [![](document-picker-images/image37.png "Záložku v kontejneru aplikace obor zabezpečení")](document-picker-images/image37.png#lightbox)
1.  **Režim import** – v tomto režimu, když uživatel vybere a externí dokumentu, nástroje pro výběr dokumentu nebude vytvořte záložku, ale místo toho zkopírujte soubor do dočasného umístění a poskytnout přístup k aplikaci v dokumentu v tomto umístění:   
 
    [![](document-picker-images/image38.png "Nástroje pro výběr dokumentu bude zkopírujte soubor do dočasného umístění a poskytují přístup k aplikaci v dokumentu v tomto umístění")](document-picker-images/image38.png#lightbox)   
 Jakmile se aplikace ukončí z jakéhokoli důvodu, vyprázdnění dočasného umístění a odebrat soubor. Pokud aplikace potřebuje k získání přístupu k souboru, měl by vytvořit kopii a jeho následné uložení do své aplikace kontejneru.


Režim otevření je užitečné, když aplikace chce spolupracovat s jinou aplikací a sdílet všechny změny provedené v dokumentu s touto aplikací. Režim Import se používá, když aplikace nechce sdílet své změny do dokumentu s jinými aplikacemi.

## <a name="making-a-document-external"></a>Provedení externí dokumentu

Jak jsme uvedli výše, aplikace pro iOS 8 nemá přístup do kontejneru mimo svůj vlastní aplikace kontejneru. Aplikace můžete zapsat do vlastní kontejner místně nebo do dočasného umístění a pak použijte režim speciální dokumentu k přesunutí výsledný dokument mimo kontejneru aplikace pro zvolené umístění uživatele.

Pro přesun dokumentu do externího umístění, postupujte takto:

1.  Nejprve vytvořte nový dokument v umístění místní nebo dočasné.
1.  Vytvoření `NSUrl` který by odkazoval na nový dokument.
1.  Otevřete nový řadič zobrazení dokumentu výběr možností a předejte ji `NSUrl` s režimu `MoveToService` . 
1.  Jakmile uživatel vybere nové umístění, bude dokument přesunout z jeho aktuálního umístění do nového umístění.
1.  Odkaz na dokument se zapíšou do kontejneru aplikace aplikace tak, aby soubor můžete pořád použít vytváření aplikací.


Následující kód slouží k přesunutí dokumentu do externího umístění: `var picker = new UIDocumentPickerViewController (srcURL, UIDocumentPickerMode.MoveToService);`

V referenčním dokumentu vrácený proces výše je přesně stejný jako jeden vytvořené otevřete režim výběru dokumentu. Existují však pokusů, které aplikace může chtít přesunout dokumentu bez udržuje odkaz na něj.

Pokud chcete přesunout dokumentu bez generování odkaz, použijte `ExportToService` režimu. Příklad: `var picker = new UIDocumentPickerViewController (srcURL, UIDocumentPickerMode.ExportToService);`

Při použití `ExportToService` režimu, dokument se zkopíruje do kontejneru externí a existující kopie je ponechán v původním umístění.

## <a name="document-provider-extensions"></a>Rozšíření poskytovatelů dokumentu

S iOS 8 Apple chce koncový uživatel moct získat přístup k žádnému své dokumenty založená na cloudu, bez ohledu na to, kde se skutečně existuje. K dosažení tohoto cíle, poskytuje iOS 8 nového mechanismu dokumentu poskytovatele rozšíření.

### <a name="what-is-a-document-provider-extension"></a>Co je zprostředkovatel přípona?

Jednoduše řečeno, přípona zprostředkovatele je způsob, jak vývojář nebo třetích stran, k poskytování úložiště alternativní dokumentu aplikací, které je přístupné přesný stejným způsobem jako existující umístění úložiště iCloud.

Uživatele můžete vybrat jednu z těchto umístění alternativní úložiště z nástroje pro výběr dokument a mohou používat přesně stejnou režimů přístupu (Open, Import, přesunutí nebo Export) pro práci se soubory v tomto umístění.

Tato možnost je implementovaná pomocí dvou různých rozšíření:

-  **Výběr rozšíření dokumentů** – poskytuje `UIViewController` podtřídami, která poskytuje grafické rozhraní pro uživatele, zvolit z úložiště alternativní umístění dokumentu. Jako součást řadičem dokumentu výběr zobrazení se zobrazí tato podtřídy.
-  **Zadejte příponu souboru** – to je bez uživatelského rozhraní rozšíření, která pracuje s ve skutečnosti poskytování obsahu souborů. Tato rozšíření jsou k dispozici prostřednictvím koordinaci souboru ( `NSFileCoordinator` ). To je další důležité případu, kdy je potřeba soubor spolupráce.


Následující diagram znázorňuje tok typické dat při práci s dokumentu poskytovatele rozšíření:

 [![](document-picker-images/image39.png "Tento diagram zobrazuje tok typické dat při práci s dokumentu poskytovatele rozšíření")](document-picker-images/image39.png#lightbox)

Spustí následující proces:

1.  Aplikace uvede řadič dokumentu výběr umožňující uživateli vybrat soubor, který chcete pracovat.
1.  Uživatel vybere alternativní umístění a souboru vlastní `UIViewController` zobrazit uživatelské rozhraní se nazývá rozšíření.
1.  Uživatel vybere soubor z tohoto umístění a adresa URL je předán zpět do nástroje pro výběr dokumentu.
1.  Nástroje pro výběr dokumentu vybere adresu URL souboru a vrátí ji do aplikace pro uživatele při práci.
1.  Adresa URL je předán koordinátorem souboru k vrácení soubory obsahu do aplikace.
1.  Koordinátor souboru volá vlastní zprostředkovatele příponu souboru se načíst soubor.
1.  Obsah souboru se vrátíte na koordinátorem souboru.
1.  Obsah souboru se vrátí do aplikace.


### <a name="security-and-bookmarks"></a>Zabezpečení a záložky

Tato část bude trvat rychlý přehled zabezpečení a trvalé soubor přístupu prostřednictvím záložky funguje s příponami zprostředkovatele dokumentu. Na rozdíl od na serveru služby iCloud poskytovatele dokumentu, který automaticky uloží zabezpečení a záložky ke kontejneru aplikace, dokumentu poskytovatele rozšíření si, protože nejsou součástí systému odkaz na dokument.

Příklad: v prostředí organizace, která poskytuje zabezpečené úložiště vlastní společnosti, správci nechcete důvěrné podnikové informace získat přístup nebo zpracovávaných veřejné Icloudu servery. Proto nelze použít předdefinované referenčního systému dokumentu.

Záložka systému, je možné použít a je zodpovědností přípona souboru zprostředkovatele správně zpracovat záložkou adresy URL a vrátí obsah dokumentu, na kterou se odkazuje.

Z bezpečnostních důvodů má iOS 8 izolační vrstvy, které ukládá informace o tom, které má aplikace přístup ke které identifikátor uvnitř poskytovatele, kterého souboru. Je potřeba poznamenat, že tato vrstva izolace řídí všechny přístup k souborům.

Následující diagram znázorňuje tok dat při práci s záložky a přípona zprostředkovatele:

 [![](document-picker-images/image40.png "Tento diagram zobrazuje tok dat při práci s záložky a příponu zprostředkovatele dokumentu")](document-picker-images/image40.png#lightbox)

Spustí následující proces:

1.  Aplikace je zadejte na pozadí a musí se zachovat stav. Zavolá `NSUrl` k vytvoření záložek do souboru v alternativní úložiště.
1.  `NSUrl` volá zprostředkovatele příponu získat trvalé adresa URL k dokumentu. 
1.  Přípona souboru zprostředkovatele vrátí adresu URL jako řetězec tak, aby `NSUrl` .
1.  `NSUrl` Obsahuje ureitou adresu URL do záložku a vrátí ji do aplikace.
1.  Když aplikace se probudí z probíhá na pozadí a je potřeba obnovit stav, předá záložku `NSUrl` .
1.  `NSUrl` volá přípona souboru zprostředkovatele s adresou URL souboru.
1.  Poskytovatele rozšíření souborů má přístup k souboru a umístění souboru, který se vrátí `NSUrl` .
1.  Umístění souboru je instalován s informace o zabezpečení a vrátí aplikaci.


Tady můžete aplikaci přístup k souboru a s ním pracovat jako normální.

### <a name="writing-files"></a>Zápis souborů

Tato část bude trvat rychlý přehled jak zápis souborů do alternativního umístění s funguje dokumentu poskytovatele rozšíření. Aplikace systému iOS pomocí souboru koordinaci uložit informace o disku uvnitř kontejneru aplikace. Krátce po souboru byla úspěšně zapsána, přípona souboru zprostředkovatele budou informováni o změnu.

V tomto okamžiku přípona souboru zprostředkovatele můžete spustit soubor odeslat do alternativního umístění (nebo označit tento soubor jako nekonzistentní a vyžadují nahrávání).

### <a name="creating-new-document-provider-extensions"></a>Vytváření nového poskytovatele rozšíření dokumentu

Vytvoření nového dokumentu poskytovatele rozšíření je mimo rozsah této úvodní článek. Tyto informace se zde k ukazují, že, podle rozšíření, která uživatel načetl ve svém zařízení s iOS, aplikace může mít přístup k umístění úložiště dokumentů mimo Apple zadané umístění serveru služby iCloud.

Vývojáři měli vědět o této skutečnosti, při použití nástroje pro výběr dokumentu a práce s externí dokumenty. Převzaly nesmí těchto dokumentů, které jsou hostované v serveru služby iCloud.

Další informace o vytváření zprostředkovatele úložiště nebo rozšíření výběr dokumentu, najdete v tématu [Úvod do rozšíření aplikace](~/ios/platform/extensions.md) dokumentu.

## <a name="migrating-to-icloud-drive"></a>Migrace na serveru služby iCloud jednotky

V systému iOS 8 uživatelé mohou pokračovat pomocí existující Icloudu dokumenty systému používané v iOS 7 (a staršími systémy) nebo můžete vybrat k migraci stávající dokumenty do nového mechanismu jednotky serveru služby iCloud.

Na Mac OS X Yosemite, Apple neposkytuje zpětné kompatibility, je potřeba migrovat všechny dokumenty na serveru služby iCloud disku nebo se již nebude aktualizovat na zařízeních.

Po migraci uživatelský účet na serveru služby iCloud jednotky pouze zařízení pomocí Icloudu disku bude moct rozšířit změny na dokumenty v těchto zařízeních.

> [!IMPORTANT]
> Vývojáři měli vědět, že nové funkce popsaná v tomto článku jsou k dispozici pouze pokud účet uživatele se migroval na serveru služby iCloud jednotky. 

## <a name="summary"></a>Souhrn

Tento článek má zahrnutých změny icloudem existující rozhraní API potřebné k podpoře serveru služby iCloud jednotky a nového řadiče zobrazení výběr dokumentu. Které souboru koordinace a proč je důležité při práci s dokumenty založená na cloudu. Má zahrnutých instalace požadovaných k povolení dokumenty cloudové aplikace pro Xamarin.iOS a zadané úvodní prohlédnout práce s dokumenty mimo kontejner aplikace aplikace pomocí řadiče zobrazení výběr dokumentu.

Kromě toho tento článek stručně zahrnutých dokumentu poskytovatele rozšíření a proč vývojáři měli vědět, je při psaní aplikací, které může zpracovat cloudové dokumenty.

## <a name="related-links"></a>Související odkazy

- [DocPicker (ukázka)](https://developer.xamarin.com/samples/monotouch/ios8/DocPicker/)
- [Úvod do iOSu 8](~/ios/platform/introduction-to-ios8.md)
- [Úvod do rozšíření aplikace](~/ios/platform/extensions.md)
