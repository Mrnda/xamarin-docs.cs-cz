---
title: Icloudu
description: Společnost Apple vydala v iOS 5 Icloudu jako službu, která umožňují aplikacím ukládat data na servery společnosti Apple a mít jeho synchronizaci na všech zařízeních, která používá stejnou osoba (prostřednictvím jejich Apple ID). Je také zálohování součást, kde data v zařízeních se zálohovaná na servery společnosti Apple. Tento dokument popisuje, jak používat některé z na serveru služby iCloud poskytuje rozhraní API společností Apple k ukládání a načítání dat ze svých serverů s C# – ukázky pro uložení párů malá klíč hodnota data a pro ukládání dokumentů. Také popisuje, jak Icloudu zálohování mohou mít vliv na návrh vaší aplikace.
ms.prod: xamarin
ms.assetid: C6F3B87C-C195-4434-EF14-D66E63894F09
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 06/09/2016
ms.openlocfilehash: a62d4621a8f3ace64401d64e35c806317a591c03
ms.sourcegitcommit: bc39d85b4585fcb291bd30b8004b3f7edcac4602
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/16/2018
---
# <a name="icloud"></a>Icloudu

_Společnost Apple vydala v iOS 5 Icloudu jako službu, která umožňují aplikacím ukládat data na servery společnosti Apple a mít jeho synchronizaci na všech zařízeních, která používá stejnou osoba (prostřednictvím jejich Apple ID). Je také zálohování součást, kde data v zařízeních se zálohovaná na servery společnosti Apple. Tento dokument popisuje, jak používat některé z na serveru služby iCloud poskytuje rozhraní API společností Apple k ukládání a načítání dat ze svých serverů s C# – ukázky pro uložení párů malá klíč hodnota data a pro ukládání dokumentů. Také popisuje, jak Icloudu zálohování mohou mít vliv na návrh vaší aplikace._

Úložiště iCloud rozhraní API v iOS 5 umožňuje aplikacím ukládat dokumenty uživatele a specifická data do centrálního umístění a přístup k těmto položkám ze zařízení, všechny uživatele.

K dispozici jsou čtyři typy úložiště:

- **Klíč-hodnota úložiště** – Pokud chcete sdílet malé množství dat s vaší aplikací na jiných zařízení uživatele.

- **Úložiště UIDocument** – Pokud chcete ukládat dokumenty a další data v účtem Icloudu uživatele pomocí podtřídou třídy UIDocument.

- **CoreData** -úložiště databáze SQLite.

- **Jednotlivé soubory a adresáře** – pro správu spoustu různých souborů přímo v systému souborů.

Tento dokument popisuje první dva typy - páry klíč-hodnota a UIDocument podtřídy – a jak používat tyto funkce v Xamarin.iOS.

> [!IMPORTANT]
> Apple [poskytuje nástroje](https://developer.apple.com/support/allowing-users-to-manage-data/) , což vývojářům správně zpracovat Evropské unie obecné Data Protection nařízení (GDPR).

## <a name="requirements"></a>Požadavky

- Nejnovější stabilní verze Xamarin.iOS
- Xcode 8 nebo novější
- Visual Studio pro Mac nebo Visual Studio 2015 a novější.

## <a name="preparing-for-icloud-development"></a>Příprava pro vývoj na Icloudu

Aplikace musí být nakonfigurované na používání Icloudu v [portál zřizování Apple](https://developer.apple.com/account/ios/overview.action) a projekt sám sebe. Před vývoji pro Icloudu (nebo vyzkoušení ukázky) podle následujících pokynů.

Chcete-li správně nastavit aplikaci, aby přístup k serveru služby iCloud:

-   **Najít váš TeamID** -přihlášení k [developer.apple.com](http://developer.apple.com) a přejděte **Member Center > váš účet > Souhrn účtu vývojáře** k získání ID týmu (nebo jednotlivé ID pro vývojáře v jednom ). Bude řetězec 10 znaků ( **A93A5CM278** třeba) – to je součástí "kontejneru identifikátor".

-   **Vytvoření nového ID aplikace** – k vytvoření ID aplikace, postupujte podle kroků uvedených v [zřizování pro úložiště technologie část průvodce zřizování zařízení](~/ios/deploy-test/provisioning/capabilities/icloud-capabilities.md)a zkontrolujte **Icloudu** jako povolená služba:

 [![](introduction-to-icloud-images/icloud-sml.png "Zkontrolujte Icloudu jako povolené služby")](introduction-to-icloud-images/icloud.png#lightbox)

- **Vytvořit nový profil pro zřizování** – Pokud chcete vytvořit profil zřizování, postupujte podle kroků uvedených v [zřizování zařízení Průvodce](~/ios/get-started/installation/device-provisioning/index.md#Provisioning_Profile) .

- **Přidejte identifikátor kontejneru Entitlements.plist** -formát identifikátor kontejneru je `TeamID.BundleID`. Další informace najdete v části [práce oprávnění](~/ios/deploy-test/provisioning/entitlements.md) průvodce.

- **Konfigurace vlastností projektu** – v Info.plist zkontrolujte soubor **identifikátor svazku** odpovídá **ID sady** nastavena, když [vytvoření ID aplikace ](~/ios/deploy-test/provisioning/capabilities/index.md); Podepisování sady používá iOS **profil zřizování** obsahující ID aplikace se na serveru služby iCloud služby App Service a **vlastní oprávnění** soubor vybraný. Všechny stačí v sadě Visual Studio v podokně vlastností projektu.

- **Povolit Icloudu na vašem zařízení** – přejděte na **Nastavení > Icloudu** a ujistěte se, že zařízení je přihlášen.
Vyberte a zapnout **dokumenty a Data** možnost.

- **Zařízení musí používat pro testování Icloudu** -nebude fungovat v simulátoru.
Ve skutečnosti je skutečně potřebujete minimálně dva zařízení všechny přihlášení pod stejným Apple ID zobrazíte Icloudu v akci.


## <a name="key-value-storage"></a>Klíč hodnota úložiště

Klíč hodnota úložiště je určený pro malé množství dat, který uživatel může jako trvalé mezi zařízení – například poslední stránky, které budou si zobrazili v knize nebo časopis. Klíč hodnota úložiště není vhodné používat pro zálohování up data.

Existují některá omezení být vědět, když používáte klíč hodnota úložiště:

- **Maximální velikost klíče** -klíč názvy nesmí být delší než 64 bajtů.

- **Maximální hodnota velikosti** -nelze uložit více než 64 kB jedinou hodnotu.

- **Úložiště dvojic klíč hodnota maximální velikosti pro aplikaci** -aplikace může ukládat až 64 kB klíč hodnota dat pouze celkem. Pokusí se nastavit klíče překračuje tento limit se nezdaří a předchozí hodnotu zachová.

- **Datové typy** – pouze základní typy jako řetězců, čísel a může být uložený logické hodnoty.

**ICloudKeyValue** příklad ukazuje, jak to funguje. Ukázkový kód vytvoří klíč s názvem pro každé zařízení: můžete nastavit tento klíč na jednom zařízení a podívejte se na hodnotu get rozšíří do jiné. Vytvoří také klíč s názvem "Sdílené", který lze upravovat na libovolném zařízení – Pokud chcete upravit najednou na mnoho zařízení, Icloudu rozhodne hodnotu "wins" (použití časového razítka na změny) a získá rozšířeny.

Tento snímek obrazovky ukazuje ukázku používán. Při přijímání oznámení o změnách z Icloudu jsou vytištěn v posouvání zobrazení textu v dolní části obrazovky a aktualizována v vstupních polí.



 [![](introduction-to-icloud-images/icloud-kv-arrows.png "Tok zpráv mezi zařízeními")](introduction-to-icloud-images/icloud-kv-arrows.png#lightbox)

### <a name="setting-and-retrieving-data"></a>Nastavení nebo načtení dat

Tento kód ukazuje, jak nastavit hodnotu řetězce.

```csharp
var store = NSUbiquitousKeyValueStore.DefaultStore;
store.SetString("testkey", "VALUE IN THE CLOUD");  // key and value
store.Synchronize();
```

Volání metody synchronizace zajišťuje, že hodnota je uložit trvale na místní diskové úložiště jenom. Synchronizace na serveru služby iCloud se odehrává na pozadí a nelze "Vynutit" kód aplikace. S funkční připojení k síti synchronizace často dojde během 5 sekund, ale pokud síť není nízký (nebo odpojených) aktualizace může trvat výrazně déle.

Můžete načíst hodnotu s tímto kódem:

```csharp
var store = NSUbiquitousKeyValueStore.DefaultStore;
display.Text = store.GetString("testkey");
```

Hodnota je načten z místního úložiště dat – tato metoda se nepokusí ke kontaktování serveru služby iCloud serverů pro získání hodnotě "posledního". Icloudu se aktualizuje místním úložišti podle svůj vlastní plán.

### <a name="deleting-data"></a>Odstraňování dat

K úplnému odebrání pár klíč hodnota, použijte metodu odebrat takto:

```csharp
var store = NSUbiquitousKeyValueStore.DefaultStore;
store.Remove("testkey");
store.Synchronize();
```

### <a name="observing-changes"></a>Sledování změn

Aplikace také můžete přijímat upozornění, když jsou hodnoty změnit pomocí Icloudu přidáním pozorovatel k `NSNotificationCenter.DefaultCenter`.
Následující kód z **KeyValueViewController.cs** `ViewWillAppear` metoda ukazuje, jak čekat na těchto oznámení a vytvoří seznam z nich došlo ke změně klíčů:

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

Proveďte kódu můžete některé akce se seznamem změněné klíčů, jako je například místní kopii je aktualizace nebo aktualizace uživatelského rozhraní s novými hodnotami.

Změna možné důvody jsou: ServerChange (0), InitialSyncChange (1) nebo QuotaViolationChange (2). Můžete používat z důvodu a v případě potřeby provedení různých zpracování (například možná budete muset odebrat některé klíče jako výsledek toho *QuotaViolationChange*).

## <a name="document-storage"></a>Ukládání dokumentů

Icloudu úložiště dokumentů je navržená ke správě dat, která je důležité pro vaši aplikaci (a pro uživatele). Slouží ke správě souborů a další data, která vaše aplikace je potřeba spustit, když ve stejnou dobu sdílení funkcí v zařízeních všechny uživatele a poskytuje zálohování na iCloud.

Tento diagram znázorňuje, jak se všechny vešel společně. Každé zařízení má data uložená na místní úložiště (UbiquityContainer) a na Icloudu operačního systému, který má na starosti démon odesílat a přijímat data v cloudu. Přístup ke UbiquityContainer všech souborů je třeba provést prostřednictvím FilePresenter/FileCoordinator, aby se zabránilo souběžný přístup. `UIDocument` Třída implementuje těch, které pro vás; tento příklad ukazuje, jak používat UIDocument.

 [![](introduction-to-icloud-images/icloud-overview.png "V dokumentu úložiště – přehled")](introduction-to-icloud-images/icloud-overview.png#lightbox)

Příklad iCloudUIDoc implementuje jednoduchou `UIDocument` podtřídami, který obsahuje jedno textové pole. Text je vykreslen v `UITextView` a úpravy rozšířeny pomocí Icloudu s jinými zařízeními s zprávu oznámení zobrazené červeně. Ukázkový kód není řešit pokročilejší funkce serveru služby iCloud jako řešení konfliktů.

Tento snímek obrazovky ukazuje ukázkovou aplikaci - po změně textu a stisknutím klávesy **UpdateChangeCount** dokumentu se synchronizují prostřednictvím serveru služby iCloud s jinými zařízeními.

 [![](introduction-to-icloud-images/iclouduidoc.png "Tento snímek obrazovky ukazuje ukázkovou aplikaci po změně textu a stisknutím klávesy UpdateChangeCount")](introduction-to-icloud-images/iclouduidoc.png#lightbox)

Existují pět částí iCloudUIDoc ukázce:

1. **Přístup k UbiquityContainer** -zjistit, zda je povoleno Icloudu a pokud ano cesta k oblasti úložiště iCloud vaší aplikace.

1. **Vytváření podtřídy UIDocument** -vytvořte třídu pro zprostředkující mezi Icloudu úložiště a vaše objekty modelu.

1. **Hledání a otevírání dokumentů serveru služby iCloud** -použít `NSFileManager` a `NSPredicate` k vyhledání serveru služby iCloud dokumenty a jejich otevření.

1. **Zobrazování dokumentů serveru služby iCloud** -vystavení vlastností z vaší `UIDocument` tak, aby můžete pracovat s ovládacími prvky uživatelského rozhraní.

1. **Ukládání dokumentů serveru služby iCloud** – Ujistěte se, že jsou na disku a na Icloudu trvalé změny provedené v uživatelském rozhraní.

Všechny operace Icloudu spustit (nebo se má spustit) asynchronně, takže jejich neblokovat při čekání na určitou akci. Zobrazí se třemi různými způsoby toho dosáhnout v ukázce:

 **Vlákna** – v `AppDelegate.FinishedLaunching` počáteční volání `GetUrlForUbiquityContainer` se provádí na jiné vlákno k zabránění blokování hlavního vlákna.

 **NotificationCenter** – registrace pro oznámení, když asynchronní operace, jako `NSMetadataQuery.StartQuery` dokončení.

 **Obslužné rutiny dokončení** – předejte metod ke spuštění na dokončení asynchronní operace, jako je `UIDocument.Open`.

### <a name="accessing-the-ubiquitycontainer"></a>Přístup k UbiquityContainer

Prvním krokem při použití serveru služby iCloud úložiště dokumentů je určit, zda je povoleno Icloudu a pokud ano umístění všudypřítomnosti kontejner, (adresář, kde jsou uloženy soubory Icloudu povoleno na zařízení).

Tento kód je v `AppDelegate.FinishedLaunching` metoda ukázky.

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

I když ukázku, uděláte tak, Apple doporučuje volání GetUrlForUbiquityContainer vždy, když aplikace je teď dostupná popředí.

### <a name="creating-a-uidocument-subclass"></a>Vytváření UIDocument podtřídy

Všechny Icloudu soubory a adresáře (ie. nic uložené v adresáři UbiquityContainer) musí být spravované pomocí NSFileManager metod, implementaci protokolu NSFilePresenter a zápis prostřednictvím NSFileCoordinator.
Nejjednodušší způsob, jak provést všechny této se zapisuje na sami, ale podtřídami UIDocument, která dělá všechny pro vás.

Existují pouze dvě metody, které musí implementovat v podtřídy UIDocument pro práci s serveru služby iCloud:

- **LoadFromContents** -předá NSData obsahu souboru můžete rozbalit do třídy modelu/es.

- **ContentsForType** -požadavek na zadání NSData reprezentace třídy modelu nebo es uložit na disk (a v cloudu).

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

Datový model v tomto případě je velmi jednoduchý – textové pole. Datového modelu může být složité podle potřeby, například dokumentu Xml nebo binární data. Primární rolí UIDocument implementace je pro převod mezi tříd modelu a reprezentaci NSData, kterou lze uložit nebo načíst na disku.

### <a name="finding-and-opening-icloud-documents"></a>Hledání a otevírání dokumentů Icloudu

Ukázková aplikace zpracovává jenom s jedním souborem - test.txt - proto kód v **AppDelegate.cs** vytvoří `NSPredicate` a `NSMetadataQuery` hledat speciálně pro tento název souboru. `NSMetadataQuery` Běží asynchronně a odešle oznámení po dokončení. `DidFinishGathering` Získá nazvané oznámení pozorovatele, zastaví dotaz a LoadDocument, který používá `UIDocument.Open` metoda s obslužnou rutinou dokončení pokusu načíst soubor a zobrazit ji v `MonkeyDocumentViewController`.

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

### <a name="displaying-icloud-documents"></a>Zobrazování dokumentů s Icloudem

Zobrazení UIDocument nesmí být žádné jiné pro jiné třídy modelu
- vlastnosti se zobrazí v ovládacích prvků uživatelského rozhraní, může být upraven uživatelem a pak zpětně zapsat do modelu.

V příkladu **iCloudUIDoc\MonkeyDocumentViewController.cs** zobrazí text MonkeyDocument v `UITextView`. `ViewDidLoad` čeká na oznámení odeslaných za `MonkeyDocument.LoadFromContents` metoda. `LoadFromContents` je volána, když Icloudu má nová data souboru, takže oznámení určuje, zda dokument byl aktualizován.

```csharp
NSNotificationCenter.DefaultCenter.AddObserver (this,
    new Selector ("dataReloaded:"),
    new NSString ("monkeyDocumentModified"),
    null
);
```

Obslužná rutina oznámení ukázkový kód volá metodu v tomto případě aktualizovat uživatelské rozhraní – bez jakékoli zjišťování konfliktů nebo rozlišení.

```csharp
[Export ("dataReloaded:")]
void DataReloaded (NSNotification notification)
{
    doc = (MonkeyDocument)notification.Object;
    // we just overwrite whatever was being typed, no conflict resolution for now
    docText.Text = doc.DocumentString;
}
```

### <a name="saving-icloud-documents"></a>Ukládání dokumentů Icloudu

Chcete-li přidat UIDocument icloudem můžete volat `UIDocument.Save` přímo (pro pouze nové dokumenty) nebo přesunout existující soubor pomocí `NSFileManager.DefaultManager.SetUbiquitious`. Příklad kódu vytvoří nový dokument přímo v kontejneru všudypřítomnosti s tímto kódem (existují dvě dokončení obslužné rutiny tady, jeden pro `Save` operace a druhý pro Open):

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

Následující změny v dokumentu "uložíte" přímo, místo toho jsme říct `UIDocument` , se změnila s `UpdateChangeCount`, a automaticky ji bude plán uložení na disk operace:

```csharp
doc.UpdateChangeCount (UIDocumentChangeKind.Done);
```

### <a name="managing-icloud-documents"></a>Správa dokumentů na serveru služby iCloud

Uživatelé mohou spravovat Icloudu dokumenty v **dokumenty** adresář všudypřítomnosti kontejner, mimo aplikaci prostřednictvím nastavení; mohou prohlížet seznam souborů a prstem odstranit. Kód aplikace by měl být schopna zpracovávat situaci, kde jsou dokumenty odstraněno uživatelem. Neukládejte interní aplikační data v **dokumenty** adresáře.

 [![](introduction-to-icloud-images/icloudstorage.png "Správa dokumentů serveru služby iCloud pracovního postupu")](introduction-to-icloud-images/icloudstorage.png#lightbox)



Uživatelé obdrží upozornění na jiný, při pokusu o odebrání aplikace povoleno Icloudu z jejich zařízení, a informujte je o stavu serveru služby iCloud dokumenty související k dané aplikaci.

 [![](introduction-to-icloud-images/icloud-delete1.png "Ukázka dialogové okno, když se uživatel pokusí odebrat aplikaci povoleno Icloudu z jejich zařízení")](introduction-to-icloud-images/icloud-delete1.png#lightbox)

 [![](introduction-to-icloud-images/icloud-delete2.png "Ukázka dialogové okno, když se uživatel pokusí odebrat aplikaci povoleno Icloudu z jejich zařízení")](introduction-to-icloud-images/icloud-delete2.png#lightbox)

## <a name="icloud-backup"></a>Zálohování serveru služby iCloud

Při zálohování na iCloud není funkce, která je přímo přistupovat vývojáři, způsob návrhu aplikace může ovlivnit činnost koncového uživatele.
Poskytuje Apple [iOS pokyny úložiště dat](http://developer.apple.com/icloud/documentation/data-storage/) pro vývojáře podle ve svých aplikacích iOS.

Většina důležitý aspekt spočívá v tom, jestli vaše aplikace ukládá velkých souborů, které nejsou generované uživatelem (například katalogu čtečky aplikace, která ukládá hundred-plus obsah za problém v megabajtech). Apple upřednostní neukládejte toto řazení dat, kde bude mít zálohovaná na serveru služby iCloud a zbytečně vyplnění kvóty Icloudu uživatele.

Aplikace, které ukládají velké objemy dat, jako je to měli buď uložit v jednom z adresáře uživatele, které není zálohovaná (např. Mezipaměti nebo tmp) nebo použijte `NSFileManager.SetSkipBackupAttribute` příznak použít k těmto souborům tak, aby je iCloud ignoruje během operace zálohování.

## <a name="summary"></a>Souhrn

Tento článek zavedla nová funkce serveru služby iCloud součástí systém iOS 5. Je zkontrolován kroky nutné ke konfiguraci projektu pro použití Icloudu a pak poskytuje příklady, jak implementovat funkce serveru služby iCloud.

V příkladu klíč hodnota úložiště ukázán, jak lze pomocí Icloudu ukládat malé množství dat, podobně jako, které jsou uloženy NSUserPreferences. Příklad UIDocument vám ukázal, jak další komplexní data můžete ukládat a synchronizována v rámci více zařízení prostřednictvím serveru služby iCloud.

Nakonec zahrnuty stručné informace o přidání serveru služby iCloud zálohování, jak by mělo ovlivnit návrh vaší aplikace.



## <a name="related-links"></a>Související odkazy

- [Úvod do Icloudu (ukázka)](https://developer.xamarin.com/samples/monotouch/IntroductionToiCloud)
- [Icloudu seminář ukázkový kód](https://github.com/xamarin/Seminars/tree/master/2012-03-22-iCloud)
- [Snímky seminář Icloudu](http://www.slideshare.net/Xamarin/using-icloud-with-monotouch)
- [Icloudu NSUbiquitousKeyValueStore](https://developer.apple.com/library/prerelease/ios/)
- [Icloudu úložiště](http://support.apple.com/kb/HT4847)
