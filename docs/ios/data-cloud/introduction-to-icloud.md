---
title: Pomocí serveru služby iCloud Xamarin.iOS
description: Tento dokument popisuje iCloud a jeho použití v aplikacích Xamarin.iOS. Popisuje úložiště hodnot klíčů, úložiště dokumentů a zálohy na Icloudu.
ms.prod: xamarin
ms.assetid: C6F3B87C-C195-4434-EF14-D66E63894F09
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 06/09/2016
ms.openlocfilehash: b72ecc40994d9336c4941f3db700796edd80e81f
ms.sourcegitcommit: 51c274f37369d8965b68ff587e1c2d9865f85da7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/30/2018
ms.locfileid: "39353214"
---
# <a name="using-icloud-with-xamarinios"></a>Pomocí serveru služby iCloud Xamarin.iOS

Úložiště iCloud rozhraní API v systému iOS 5 umožňuje aplikacím ukládat dokumenty uživatele a specifická data do centrálního umístění a přístup k tyto položky zařízení pro všechny uživatele.

K dispozici jsou čtyři typy úložiště:

- **Úložiště hodnot klíčů** – Pokud chcete sdílet malých objemů dat s vaší aplikací na dalším zařízením uživatele.

- **Úložiště UIDocument** – Pokud chcete ukládat dokumenty a další data v účtu iCloud uživatele pomocí podtřída UIDocument.

- **CoreData** -úložiště databáze SQLite.

- **Jednotlivé soubory a adresáře** – pro správu mnoho různých souborů přímo v systému souborů.

Tento dokument popisuje první dva typy - páry klíč-hodnota a UIDocument podtřídy – a jak používat tyto funkce v Xamarin.iOS.

> [!IMPORTANT]
> Apple [poskytuje nástroje](https://developer.apple.com/support/allowing-users-to-manage-data/) , což vývojářům umožňuje správně zpracovat Evropské unie obecného Regulation (GDPR).

## <a name="requirements"></a>Požadavky

- Nejnovější stabilní verze Xamarin.iOS
- Xcode 8 nebo novější
- Visual Studio pro Mac nebo Visual Studio 2015 a novější.

## <a name="preparing-for-icloud-development"></a>Příprava pro vývoj na Icloudu

Aplikace musí být nakonfigurován pro použití Icloudu v [portálem pro zřizování Apple Developer](https://developer.apple.com/account/ios/overview.action) a samotného projektu. Před vývojem pro Icloudu (nebo vyzkoušení ukázky) použijte následující postup.

Chcete-li správně nakonfigurovat aplikaci pro přístup k serveru služby iCloud:

-   **Najít vaše TeamID** – Přihlaste se k [developer.apple.com](http://developer.apple.com) a přejděte **centra > svůj účet > Přehled účtů pro vývojáře** ID týmu (nebo jednotlivé ID pro jednoho vývojáře ). Je řetězec znaků 10 ( **A93A5CM278** třeba) – to je součástí "identifikátor kontejneru".

-   **Vytvoření nového ID aplikace** – Chcete-li vytvořit ID aplikace, postupujte podle kroků uvedených v [zřizování pro Store technologie části Průvodce Device Provisioning](~/ios/deploy-test/provisioning/capabilities/icloud-capabilities.md)a nezapomeňte se podívat **Icloudu** jako povolené služby:

 [![](introduction-to-icloud-images/icloud-sml.png "Kontrola serveru služby iCloud jako povolené služby")](introduction-to-icloud-images/icloud.png#lightbox)

- **Vytvoření nového profilu zřizování** – Chcete-li vytvořit zřizovací profil, postupujte podle kroků uvedených v [Device Provisioning průvodce](~/ios/get-started/installation/device-provisioning/index.md#provisioning-your-device) .

- **Přidejte identifikátor kontejneru do souboru Entitlements.plist** – formát identifikátor kontejneru je `TeamID.BundleID`. Další informace najdete [práce s nároky](~/ios/deploy-test/provisioning/entitlements.md) průvodce.

- **Konfigurace vlastností projektu** – v souboru Info.plist zkontrolujte soubor **identifikátor sady prostředků** odpovídá **ID sady prostředků** nastavena, když [vytvoření ID aplikace ](~/ios/deploy-test/provisioning/capabilities/index.md); Podepisování sad prostředků používá pro iOS **zřizovací profil** obsahující ID aplikace se na serveru služby iCloud služby App Service a **vlastní oprávnění** vybraný soubor. To všechno můžete dělat v sadě Visual Studio v podokně vlastností projektu.

- **Povolit iCloud na vašem zařízení** – přejděte na **Nastavení > Icloudu** a ujistěte se, že zařízení je přihlášen.
Vyberte a zapnout **dokumenty a Data** možnost.

- **Zařízení musí používat k testování Icloudu** -nebude fungovat v simulátoru.
Ve skutečnosti opravdu potřebujete dva nebo více zařízení všech přihlášení pomocí stejného Apple ID, které chcete zobrazit v akci na Icloudu.


## <a name="key-value-storage"></a>Úložiště hodnot klíčů

Úložiště hodnot klíčů je určený pro malé množství dat, která uživatel může jako trvalý napříč zařízení – například poslední stránky, kterou se zobrazit v knize nebo magazine. Úložiště hodnot klíčů není vhodné používat pro data zálohování.

Existují některá omezení je potřeba vědět při použití úložiště klíč / hodnota:

- **Maximální velikost klíče** – názvy klíč nesmí být delší než 64 bajtů.

- **Maximální hodnota velikosti** – více než 64 kB nelze ukládat v jedné hodnoty.

- **Úložiště dvojic klíč hodnota maximální velikost pro aplikaci** – aplikace může ukládat až 64 kB daty klíč hodnota pouze celkem. Pokusí se nastavit klíče nad rámec tohoto limitu se nezdaří a zachová předchozí hodnotu.

- **Datové typy** – pouze základní typy, jako jsou řetězce, čísla a logické hodnoty mohou být uloženy.

**ICloudKeyValue** příklad ukazuje, jak to funguje. Vzorový kód vytvoří klíč s názvem pro každé zařízení: můžete nastavit tento klíč na jednom zařízení a podívejte se na hodnotu získat rozšíří na další. Také vytvoří klíč s názvem "Sdílené", který lze upravovat na libovolném zařízení – Pokud upravíte současně na mnoha zařízeních, Icloudu rozhodne hodnotu "wins" (použití časového razítka na změně), který získá rozšířena.

Tento snímek obrazovky znázorňuje ukázku používá. Při přijetí oznámení o změnách ze serveru služby iCloud jsou zobrazeny v posouvání zobrazení textu v dolní části obrazovky a aktualizuje ve vstupních polí.



 [![](introduction-to-icloud-images/icloud-kv-arrows.png "Tok zpráv mezi zařízeními")](introduction-to-icloud-images/icloud-kv-arrows.png#lightbox)

### <a name="setting-and-retrieving-data"></a>Nastavení nebo načtení dat

Tento kód ukazuje, jak nastavit hodnotu řetězce.

```csharp
var store = NSUbiquitousKeyValueStore.DefaultStore;
store.SetString("testkey", "VALUE IN THE CLOUD");  // key and value
store.Synchronize();
```

Volání synchronizovat zajistí, že hodnota se ukládají do jenom úložiště místního disku. Synchronizace na serveru služby iCloud probíhá na pozadí a nejde "Vynutit" kódem aplikace. Pomocí funkční připojení k síti synchronizace často proběhne do 5 sekund, ale pokud síť není nízký (nebo odpojených) aktualizace může trvat mnohem déle.

Můžete načíst hodnotu s tímto kódem:

```csharp
var store = NSUbiquitousKeyValueStore.DefaultStore;
display.Text = store.GetString("testkey");
```

Hodnota je načten z místního úložiště dat – tato metoda se nepokusí ke kontaktování serveru služby iCloud serverů pro získání hodnotu "posledního". iCloud aktualizuje místní úložiště dat podle vlastního plánu.

### <a name="deleting-data"></a>Odstranění dat

Pokud chcete úplně odebrat pár klíč hodnota, použijte metodu odebrat následujícím způsobem:

```csharp
var store = NSUbiquitousKeyValueStore.DefaultStore;
store.Remove("testkey");
store.Synchronize();
```

### <a name="observing-changes"></a>Sledování změn

Aplikace může také přijímat oznámení, když se změní hodnoty serveru služby iCloud přidáním pozorovatel k `NSNotificationCenter.DefaultCenter`.
Následující kód z **KeyValueViewController.cs** `ViewWillAppear` metoda ukazuje, jak k naslouchání pro tato oznámení a vytvoří seznam z nich byly změněny klíče:

```csharp
keyValueNotification =
NSNotificationCenter.DefaultCenter.AddObserver (
    NSUbiquitousKeyValueStore.DidChangeExternallyNotification, notification => {
    Console.WriteLine ("Cloud notification received");
    NSDictionary userInfo = notification.UserInfo;

    var reasonNumber = (NSNumber)userInfo.ObjectForKey (NSUbiquitousKeyValueStore.ChangeReasonKey);
    nint reason = reasonNumber.NIntValue;

    var changedKeys = (NSArray)userInfo.ObjectForKey (NSUbiquitousKeyValueStore.ChangedKeysKey);
    var changedKeysList = new List<string> ();
    for (uint i = 0; i < changedKeys.Count; i++) {
        var key = changedKeys.GetItem<NSString> (i); // resolve key to a string
        changedKeysList.Add (key);
    }
    // now do something with the list...
});
```

Váš kód pak můžou některé akce se seznamem změněné klíčů, jako je například aktualizaci jejich místní kopie nebo aktualizaci uživatelského rozhraní s novými hodnotami.

Možné změny důvody jsou: ServerChange (0), InitialSyncChange (1) nebo QuotaViolationChange (2). Můžete používat z důvodu a provádět jiné zpracování v případě potřeby (například budete muset odebrat některé klíče kvůli *QuotaViolationChange*).

## <a name="document-storage"></a>Úložiště dokumentů

iCloud úložiště dokumentů je určen ke správě dat, která jsou důležité pro vaši aplikaci (a pro uživatele). Slouží ke správě souborů a další data, která vaše aplikace potřebuje ke spuštění, zatímco ve stejnou dobu poskytuje zálohování na iCloud a sdílení funkcí mezi všechny uživatele zařízení.

Tento diagram znázorňuje, jak to všechno dohromady vyhovuje. Každé zařízení má data uložená na místním úložišti (UbiquityContainer) a na Icloudu operačního systému, který proces démon se postará o odesílání a přijímání dat v cloudu. Všechny přístup k souborům UbiquityContainer musíte to udělat pomocí FilePresenter/FileCoordinator zabránit souběžný přístup. `UIDocument` Třída implementuje tyto pro vás; tento příklad ukazuje způsob použití UIDocument.

 [![](introduction-to-icloud-images/icloud-overview.png "Přehled úložiště dokumentu")](introduction-to-icloud-images/icloud-overview.png#lightbox)

Příklad iCloudUIDoc implementuje jednoduchou `UIDocument` podtřídy, která obsahuje textové pole. Text je vykreslen v `UITextView` a úpravy se rozšíří pomocí serveru služby iCloud s jinými zařízeními s zprávu s oznámením zobrazí červeně. Vzorový kód není využívání pokročilejší funkce Icloudu, jako je řešení konfliktů.

Tento snímek obrazovky ukazuje ukázkové aplikace - po změně text a stisknutím klávesy **UpdateChangeCount** dokumentu synchronizována prostřednictvím serveru služby iCloud s jinými zařízeními.

 [![](introduction-to-icloud-images/iclouduidoc.png "Tento snímek obrazovky ukazuje ukázkovou aplikaci po změně text a stisknutím klávesy UpdateChangeCount")](introduction-to-icloud-images/iclouduidoc.png#lightbox)

Existuje pět částí iCloudUIDoc ukázku:

1. **Přístup k UbiquityContainer** – určení, zda je povolena na Icloudu a pokud ano cestu k oblasti úložiště iCloud vaší aplikace.

1. **Vytvoření podtřídy UIDocument** – vytvořit třídu pro zprostředkující mezi úložiště iCloud a objekty modelu.

1. **Vyhledání a otevírání dokumenty Icloudu** – použijte `NSFileManager` a `NSPredicate` dokumenty Icloudu najít a otevřít je.

1. **Zobrazení dokumenty Icloudu** -vystavení vlastností z vaší `UIDocument` tak, že budete moct setkat s ovládacích prvků uživatelského rozhraní.

1. **Ukládání dokumentů s Icloudem** – Ujistěte se, že jsou trvalé změny provedené v uživatelském rozhraní na disk a na Icloudu.

Spustit všechny operace na Icloudu (nebo by měly být spuštěny) asynchronně, takže nemusíte blokují při čekání na určitou akci. Zobrazí se třemi různými způsoby toho dosáhnout v ukázce:

 **Vlákna** – v `AppDelegate.FinishedLaunching` počáteční volání `GetUrlForUbiquityContainer` se provádí v jiném vlákně, aby se zabránilo blokování hlavního vlákna.

 **NotificationCenter** – registrace pro oznámení, když asynchronní operace, jako `NSMetadataQuery.StartQuery` dokončení.

 **Obslužné rutiny dokončení** – předejte metod ke spuštění při dokončení asynchronní operace, jako je `UIDocument.Open`.

### <a name="accessing-the-ubiquitycontainer"></a>Přístup k UbiquityContainer

Prvním krokem při používání serveru služby iCloud úložiště dokumentů je určit, zda je povoleno Icloudu a pokud ano umístění "všudypřítomnosti kontejneru" (adresář, kde jsou uloženy soubory povolena na Icloudu na zařízení).

Tento kód je v `AppDelegate.FinishedLaunching` metoda vzorku.

```csharp
// GetUrlForUbiquityContainer is blocking, Apple recommends background thread or your UI will freeze
ThreadPool.QueueUserWorkItem (_ => {
    CheckingForiCloud = true;
    Console.WriteLine ("Checking for iCloud");
    var uburl = NSFileManager.DefaultManager.GetUrlForUbiquityContainer (null);
    // OR instead of null you can specify "TEAMID.com.your-company.ApplicationName"

    if (uburl == null) {
        HasiCloud = false;
        Console.WriteLine ("Can't find iCloud container, check your provisioning profile and entitlements");

        InvokeOnMainThread (() => {
            var alertController = UIAlertController.Create ("No \uE049 available",
            "Check your Entitlements.plist, BundleId, TeamId and Provisioning Profile!", UIAlertControllerStyle.Alert);
            alertController.AddAction (UIAlertAction.Create ("OK", UIAlertActionStyle.Destructive, null));
            viewController.PresentViewController (alertController, false, null);
        });
    } else { // iCloud enabled, store the NSURL for later use
        HasiCloud = true;
        iCloudUrl = uburl;
        Console.WriteLine ("yyy Yes iCloud! {0}", uburl.AbsoluteUrl);
    }
    CheckingForiCloud = false;
});
```

I když se ukázka není Uděláte to tak, Apple doporučuje volání GetUrlForUbiquityContainer pokaždé, když se aplikace pochází na popředí.

### <a name="creating-a-uidocument-subclass"></a>Vytváření UIDocument podtřídy

Všechny Icloudu soubory a adresáře (ie. nic uložené v adresáři UbiquityContainer) se musí spravovat NSFileManager metody, implementace protokolu NSFilePresenter a zápis přes NSFileCoordinator.
Nejjednodušší způsob, jak to udělat se zapsat sami, ale podtřídy UIDocument, která to udělá za vás.

Existují pouze dvě metody, které je nutné implementovat v UIDocument podtřídu pro práci s serveru služby iCloud:

- **LoadFromContents** -předá NSData obsah souboru můžete rozbalit do vaší třídy/es modelu.

- **ContentsForType** -požadavek, kde zadáte NSData reprezentace třídy modelu/es na uložení na disk (a v cloudu).

Tento ukázkový kód z **iCloudUIDoc\MonkeyDocument.cs** ukazuje, jak implementovat UIDocument.

```csharp
public class MonkeyDocument : UIDocument
{
    // the 'model', just a chunk of text in this case; must easily convert to NSData
    NSString dataModel;
    // model is wrapped in a nice .NET-friendly property
    public string DocumentString {
        get {
            return dataModel.ToString ();
        }
        set {
            dataModel = new NSString (value);
        }
    }
    public MonkeyDocument (NSUrl url) : base (url)
    {
        DocumentString = "(default text)";
    }
    // contents supplied by iCloud to display, update local model and display (via notification)
    public override bool LoadFromContents (NSObject contents, string typeName, out NSError outError)
    {
        outError = null;

        Console.WriteLine ("LoadFromContents({0})", typeName);

        if (contents != null)
            dataModel = NSString.FromData ((NSData)contents, NSStringEncoding.UTF8);

        // LoadFromContents called when an update occurs
        NSNotificationCenter.DefaultCenter.PostNotificationName ("monkeyDocumentModified", this);
        return true;
    }
    // return contents for iCloud to save (from the local model)
    public override NSObject ContentsForType (string typeName, out NSError outError)
    {
        outError = null;

        Console.WriteLine ("ContentsForType({0})", typeName);
        Console.WriteLine ("DocumentText:{0}",dataModel);

        NSData docData = dataModel.Encode (NSStringEncoding.UTF8);
        return docData;
    }
}
```

Datový model v tomto případě je velmi jednoduché – textové pole. Datový model může být komplexního, jako povinné, například k dokumentu Xml nebo binární data. Primární role UIDocument implementace je pro převod mezi třídách modelu a reprezentaci NSData, které lze uložit/načíst na disku.

### <a name="finding-and-opening-icloud-documents"></a>Hledání a otevírání dokumenty Icloudu

Ukázková aplikace zabývá pouze jeden soubor - test.txt – proto kódu v **AppDelegate.cs** vytvoří `NSPredicate` a `NSMetadataQuery` vás pod rouškou speciálně pro tento název souboru. `NSMetadataQuery` Běží asynchronně a odešle oznámení, když se dokončí. `DidFinishGathering` Získá volány oznámení pozorovatele, zastaví dotaz a volá LoadDocument, který používá `UIDocument.Open` metodu obslužné rutiny dokončení pokusí se načíst soubor a zobrazit je v `MonkeyDocumentViewController`.

```csharp
string monkeyDocFilename = "test.txt";
void FindDocument ()
{
    Console.WriteLine ("FindDocument");
    query = new NSMetadataQuery {
        SearchScopes = new NSObject [] { NSMetadataQuery.UbiquitousDocumentsScope }
    };

    var pred = NSPredicate.FromFormat ("%K == %@", new NSObject[] {
        NSMetadataQuery.ItemFSNameKey, new NSString (MonkeyDocFilename)
    });

    Console.WriteLine ("Predicate:{0}", pred.PredicateFormat);
    query.Predicate = pred;

    NSNotificationCenter.DefaultCenter.AddObserver (
        this,
        new Selector ("queryDidFinishGathering:"),
        NSMetadataQuery.DidFinishGatheringNotification,
        query
    );

    query.StartQuery ();
}

[Export ("queryDidFinishGathering:")]
void DidFinishGathering (NSNotification notification)
{
    Console.WriteLine ("DidFinishGathering");
    var metadataQuery = (NSMetadataQuery)notification.Object;
    metadataQuery.DisableUpdates ();
    metadataQuery.StopQuery ();

    NSNotificationCenter.DefaultCenter.RemoveObserver (this, NSMetadataQuery.DidFinishGatheringNotification, metadataQuery);
    LoadDocument (metadataQuery);
}

void LoadDocument (NSMetadataQuery metadataQuery)
{
    Console.WriteLine ("LoadDocument");

    if (metadataQuery.ResultCount == 1) {
        var item = (NSMetadataItem)metadataQuery.ResultAtIndex (0);
        var url = (NSUrl)item.ValueForAttribute (NSMetadataQuery.ItemURLKey);
        doc = new MonkeyDocument (url);

        doc.Open (success => {
            if (success) {
                Console.WriteLine ("iCloud document opened");
                Console.WriteLine (" -- {0}", doc.DocumentString);
                viewController.DisplayDocument (doc);
            } else {
                Console.WriteLine ("failed to open iCloud document");
            }
        });
    } // TODO: if no document, we need to create one
}
```

### <a name="displaying-icloud-documents"></a>Zobrazení dokumenty Icloudu

Zobrazení UIDocument nesmí být nijak neliší do jiné třídy modelu
- Zobrazí se vlastnosti v ovládacích prvcích uživatelského rozhraní, může být upraven uživatelem a pak zapíše zpět do modelu.

V příkladu **iCloudUIDoc\MonkeyDocumentViewController.cs** zobrazí text MonkeyDocument v `UITextView`. `ViewDidLoad` čeká na oznámení odeslané `MonkeyDocument.LoadFromContents` metody. `LoadFromContents` je volána, když na Icloudu má nová data k souboru, tak, aby oznámení určuje, že aktualizovaný dokument.

```csharp
NSNotificationCenter.DefaultCenter.AddObserver (this,
    new Selector ("dataReloaded:"),
    new NSString ("monkeyDocumentModified"),
    null
);
```

Obslužné rutiny oznámení ukázkový kód volá metodu v tomto případě aktualizovat uživatelské rozhraní – bez nutnosti jakékoli zjišťování konfliktů nebo rozlišení.

```csharp
[Export ("dataReloaded:")]
void DataReloaded (NSNotification notification)
{
    doc = (MonkeyDocument)notification.Object;
    // we just overwrite whatever was being typed, no conflict resolution for now
    docText.Text = doc.DocumentString;
}
```

### <a name="saving-icloud-documents"></a>Ukládá dokumenty Icloudu

Přidat UIDocument na serveru služby iCloud můžete volat `UIDocument.Save` přímo (pro pouze nové dokumenty) nebo přesunout existující soubor pomocí `NSFileManager.DefaultManager.SetUbiquitious`. Příklad kódu vytvoří nový dokument přímo v kontejneru všudypřítomnosti s tímto kódem (existují dvě dokončení obslužné rutiny, jeden pro `Save` operace a druhý pro otevřené):

```csharp
var docsFolder = Path.Combine (iCloudUrl.Path, "Documents"); // NOTE: Documents folder is user-accessible in Settings
var docPath = Path.Combine (docsFolder, MonkeyDocFilename);
var ubiq = new NSUrl (docPath, false);
var monkeyDoc = new MonkeyDocument (ubiq);
monkeyDoc.Save (monkeyDoc.FileUrl, UIDocumentSaveOperation.ForCreating, saveSuccess => {
Console.WriteLine ("Save completion:" + saveSuccess);
if (saveSuccess) {
    monkeyDoc.Open (openSuccess => {
        Console.WriteLine ("Open completion:" + openSuccess);
        if (openSuccess) {
            Console.WriteLine ("new document for iCloud");
            Console.WriteLine (" == " + monkeyDoc.DocumentString);
            viewController.DisplayDocument (monkeyDoc);
        } else {
            Console.WriteLine ("couldn't open");
        }
    });
} else {
    Console.WriteLine ("couldn't save");
}
```

Následné změny v dokumentu "neuloží" přímo, namísto toho nám říct `UIDocument` , která se změnila s `UpdateChangeCount`, a automaticky naplánuje uložit na disk operace:

```csharp
doc.UpdateChangeCount (UIDocumentChangeKind.Done);
```

### <a name="managing-icloud-documents"></a>Správa dokumentů na Icloudu

Uživatelé mohou spravovat dokumenty Icloudu **dokumenty** adresáře "kontejneru všudypřítomnosti" mimo vaši aplikaci nastavení; můžete zobrazit seznam souborů a posunutí prstem odstranit. Kód aplikace by měl být schopný zvládnout situaci, ve kterém jsou dokumenty odstraněno uživatelem. Neukládejte interní aplikační data v **dokumenty** adresáře.

 [![](introduction-to-icloud-images/icloudstorage.png "Správa pracovního postupu dokumenty Icloudu")](introduction-to-icloud-images/icloudstorage.png#lightbox)



Uživatelé obdrží upozornění na jiné, při pokusu o odebrání serveru služby iCloud povolené aplikace z jejich zařízení, a informujte je o stavu serveru služby iCloud dokumenty týkající se této aplikace.

 [![](introduction-to-icloud-images/icloud-delete1.png "Ukázkové dialogové okno, když se uživatel pokusí odebrat aplikaci povoleno Icloudu z jejich zařízení")](introduction-to-icloud-images/icloud-delete1.png#lightbox)

 [![](introduction-to-icloud-images/icloud-delete2.png "Ukázkové dialogové okno, když se uživatel pokusí odebrat aplikaci povoleno Icloudu z jejich zařízení")](introduction-to-icloud-images/icloud-delete2.png#lightbox)

## <a name="icloud-backup"></a>Zálohy na Icloudu

Při zálohování do Icloudu není funkce, která se využívají přímo od vývojářů, způsob návrhu aplikace mohou ovlivnit uživatelské prostředí.
Poskytuje Apple [iOS pokyny pro úložiště dat](http://developer.apple.com/icloud/documentation/data-storage/) pro vývojáře v jejich aplikací pro iOS.

Většina důležitý aspekt spočívá v tom, jestli vaše aplikace ukládá velké soubory, které nejsou generované uživatelem (například magazine čtečky aplikaci, která ukládá obsah na problém hundred-plus v megabajtech). Apple upřednostňuje neukládejte tento druh dat, kde ji bude možné zálohovaná na serveru služby iCloud a zbytečně vyplnit kvóty uživatele serveru služby iCloud.

Aplikace, které ukládat velké objemy dat tímto způsobem byste buď uložit v jednom z adresáře uživatele, které není zálohovanou (např.) Mezipaměti nebo technického) nebo použijte `NSFileManager.SetSkipBackupAttribute` použít příznak k těmto souborům tak, aby je iCloud ignoruje během operace zálohování.

## <a name="summary"></a>Souhrn

Tento článek zavedeny nové funkce Icloudu zahrnuté v systému iOS 5. Ho prozkoumat kroky vyžadované ke konfiguraci projektu pro použití serveru služby iCloud a pak k dispozici příklady, jak implementovat funkce serveru služby iCloud.

Úložiště hodnot klíčů příklad jsme vám ukázali, jak lze ukládat malé množství dat, podobně jako způsob, jakým se ukládají NSUserPreferences serveru služby iCloud. Příklad UIDocument ukázal vytváření komplexních datových můžete ukládat a synchronizovat napříč více zařízeními prostřednictvím serveru služby iCloud.

Nakonec zahrnuté stručný popis na přidání serveru služby iCloud Backup, jak by mělo ovlivnit návrh vaší aplikace.



## <a name="related-links"></a>Související odkazy

- [Úvod do Icloudu (ukázka)](https://developer.xamarin.com/samples/monotouch/IntroductionToiCloud)
- [iCloud seminář ukázkový kód](https://github.com/xamarin/Seminars/tree/master/2012-03-22-iCloud)
- [Snímky přednášky na Icloudu](http://www.slideshare.net/Xamarin/using-icloud-with-monotouch)
- [iCloud NSUbiquitousKeyValueStore](https://developer.apple.com/library/prerelease/ios/)
- [Úložiště iCloud](http://support.apple.com/kb/HT4847)
