---
title: CloudKit
description: "Icloudu rozhraní API umožňují aplikacím iOS 8 k uložení dat v serveru služby iCloud s podporou pro automatickou synchronizaci mezi uživatelský účet. Pomocí CloudKit poskytuje uživatelům konzistentní a bezproblémové možnosti ověřování v zařízení s podporou serveru služby iCloud. Tento článek se zabývá povolení CloudKit v aplikaci pro systém iOS 8 pomocí rozhraní API pro usnadnění práce."
ms.topic: article
ms.prod: xamarin
ms.assetid: 66B207F2-FAA0-4551-B43B-3DB9F620C397
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 05/11/2016
ms.openlocfilehash: c4ee5c0457dd1faea74cbbc30dd2d0f42087a8d0
ms.sourcegitcommit: 73bd0c7e5f237f0a1be70a6c1384309bb26609d5
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 03/22/2018
---
# <a name="cloudkit"></a>CloudKit

_Icloudu rozhraní API umožňují aplikacím iOS 8 k uložení dat v serveru služby iCloud s podporou pro automatickou synchronizaci mezi uživatelský účet. Pomocí CloudKit poskytuje uživatelům konzistentní a bezproblémové možnosti ověřování v zařízení s podporou serveru služby iCloud. Tento článek se zabývá povolení CloudKit v aplikaci pro systém iOS 8 pomocí rozhraní API pro usnadnění práce._

Rozhraní framework CloudKit zjednodušuje vývoj aplikací, že přístup k serveru služby iCloud. To zahrnuje načtení data aplikací a asset práva a také schopnost bezpečně uložit informace o aplikaci. Tato sada poskytuje uživatelům vrstvu anonymity tím, že přístup k aplikacím s jejich Icloudu ID bez sdílení osobní údaje.

Vývojáři můžou zaměřit na své aplikace na straně klienta a umožní Icloudu eliminovat potřeba psát logiku aplikace na straně serveru. CloudKit poskytuje ověřování, privátní a veřejné databáze a strukturovaných dat a služby úložiště asset.

## <a name="requirements"></a>Požadavky

Pokud chcete provést kroky uvedené v tomto článku se vyžaduje následující text:

-  **Xcode a sada SDK iOS** – Xcode společnosti Apple a iOS 8 rozhraní API musí být nainstalovaná a nakonfigurovaná na počítači pro vývojáře.
-  **Visual Studio pro Mac** – nejnovější verze sady Visual Studio pro Mac musí být nainstalováno a nakonfigurováno na zařízení uživatele.
-  **iOS 8 zařízení** – zařízení s iOS s nejnovější verzí systému iOS 8 pro testování.

## <a name="what-is-cloudkit"></a>Co je CloudKit?

CloudKit je způsob, jak poskytnout vývojáře přístup k serveru služby iCloud servery. Poskytuje základ pro Icloudu jednotky a fotografie knihovny serveru služby iCloud. CloudKit je podporována v zařízení s Mac OS X a Apple iOS.

 [![](intro-to-cloudkit-images/image1.png "Jak CloudKit je podporována v zařízení Apple iOS i Mac OS X")](intro-to-cloudkit-images/image1.png#lightbox)

CloudKit používá infrastrukturu účet serveru služby iCloud. Pokud je uživatel přihlášen serveru služby iCloud účet na zařízení, bude CloudKit pomocí ID k identifikaci uživatele. Pokud není k dispozici žádný účet, bude poskytnuta omezený přístup jen pro čtení.

CloudKit podporuje koncept veřejné a privátní databází. Veřejné databáze poskytují "polévky" všechna data, která má uživatel přístup k. Privátní databáze jsou určené k uložení privátního data vázaná na konkrétního uživatele.

CloudKit podporuje strukturovaná i hromadně data. Je umožňuje zpracovávat bezproblémově přenosech velkých souborů. CloudKit postará efektivně přenos velkých souborů do a z Icloudu servery na pozadí, uvolnění vývojáři zaměřit se na jiné úlohy.

> [!NOTE]
> Je důležité si uvědomit, že je CloudKit _přenosu technologie_. Neposkytuje žádné trvalost; pouze umožňuje aplikaci posílat a přijímat informace ze serverů efektivně.

Době psaní tohoto textu Apple původně poskytuje CloudKit zdarma vysoké limit šířky pásma a úložiště kapacity. Pro větší projekty nebo aplikací na bázi velké uživatele s má instrukcemi Apple bude poskytována cenově dostupné cenové škálování.


## <a name="enabling-cloudkit-in-a-xamarin-application"></a>Povolení CloudKit v aplikaci Xamarin

Předtím, než aplikace Xamarin můžete využít rozhraní CloudKit, aplikace musí být správně zřízená jako podrobné v [práce s možností](~/ios/deploy-test/provisioning/capabilities/icloud-capabilities.md) a [práce oprávnění](~/ios/deploy-test/provisioning/entitlements.md) příručky

1.  Otevřete projekt v sadě Visual Studio pro Mac nebo Visual Studio.
2.  V **Průzkumníku řešení**, otevřete **Info.plist** soubor a zkontrolujte **identifikátor svazku** odpovídá ten, který byl definován v **ID aplikace**vytvořen jako součást zajišťování nastavení:
 
    [![](intro-to-cloudkit-images/image26a.png "Zadejte identifikátor balíku")](intro-to-cloudkit-images/image26a-orig.png#lightbox "Info.plist file displaying Bundle Identifier")

3.  Přejděte do dolní části **Info.plist** soubor a vyberte **povolit režimy pozadí**, **aktualizace umístění** a **vzdáleného oznámení**:

    [![](intro-to-cloudkit-images/image27a.png "Vyberte povolený režimy pozadí, aktualizace umístění a vzdálené oznámení")](intro-to-cloudkit-images/image27a-orig.png#lightbox "Info.plist file displaying background modes")
4.  Klikněte pravým tlačítkem na projekt pro iOS v řešení a vyberte **možnosti**.
5.  Vyberte **iOS podepisování sady**, vyberte **vývojáře Identity** a **profil zřizování** vytvořili výše.
6.  Ujistěte se, **Entitlements.plist** zahrnuje **povolit Icloudu** , **klíč hodnota úložiště** a **CloudKit** .
7.  Ujistěte se, **Všudypřítomnosti kontejneru** existuje pro aplikaci (jak vytvořili výše). Příklad: `iCloud.com.your-company.CloudKitAtlas`
8.  Uložte změny do souboru.


S těmito nastaveními zavedené aplikace je nyní připraven pro přístup k rozhraní API CloudKit Framework.

## <a name="cloudkit-api-overview"></a>Přehled rozhraní API CloudKit

Před implementací CloudKit v aplikaci Xamarin iOS, bude v tomto článku tak, aby pokrývalo základy CloudKit rozhraní, které bude zahrnovat následující témata:

1.  **Kontejnery** – izolované sila komunikace serveru služby iCloud.
2.  **Databáze** – veřejných a privátních jsou k dispozici pro aplikaci.
3.  **Zaznamenává** – mechanismus, ve kterém je přesunout strukturovaných dat do a z CloudKit.
4.  **Záznam zón** – jsou skupiny záznamů.
5.  **Zaznamenejte identifikátory** – jsou plně normalized a představují konkrétního umístění záznamu.
6.  **Referenční dokumentace** – zadejte nadřazený podřízený vztahů mezi související záznamy v rámci na danou databázi.
7.  **Prostředky** – povolit pro soubor velký, nestrukturovaných dat nahrán do Icloudu a přidružené daného záznamu.


### <a name="containers"></a>Kontejnery

Dané aplikace běžící v zařízení se systémem iOS bude vždy spuštěn hotová straně jiných aplikací a služeb na daném zařízení. V klientském zařízení aplikace bude zastaralý nebo v izolovaném prostoru nějakým způsobem. V některých případech toto je literál izolovaného prostoru a v jiných případech aplikace je jednoduše spuštěné v něm je vlastní paměťový prostor.

Koncept vyjmutí klientskou aplikaci a spuštění oddělené z jiných klientů je velmi mocné a nabízí následující výhody:

1.  **Zabezpečení** – jednu aplikaci nelze konfliktu s jinými aplikacemi klienta nebo operačního systému.
1.  **Stabilitu** – Pokud klientská aplikace spadne ji nelze vyjměte jiné aplikace operačního systému.
1.  **Ochrana osobních údajů** – každou klientskou aplikaci má omezený přístup k osobní informace uložené v zařízení.


CloudKit byl navržen stejné výhody zajistit i uvedených výše, a použít je pro práci s informacemi o cloudových:

 [![](intro-to-cloudkit-images/image31.png "CloudKit aplikace komunikovat pomocí kontejnery")](intro-to-cloudkit-images/image31.png#lightbox)

Stejně jako se aplikace z více spuštěné na zařízení, takže je aplikace komunikace s Icloudem z více. Každý z těchto různých komunikačních sila, se nazývají kontejnery.

Kontejnery jsou viditelné v rámci CloudKit prostřednictvím `CKContainer` třídy. Ve výchozím nastavení rozhovory jedné aplikace do jednoho kontejneru a tento kontejner odděluje data pro tuto aplikaci. To znamená, že několik aplikací můžete ukládat informace o stejný účet Icloudu, ale tyto informace budou mít nikdy intermingled.

Rozdělení do kontejnerů dat s Icloudem také umožňuje CloudKit k zapouzdření informace o uživateli. Tímto způsobem aplikace bude mít některé omezený přístup k serveru služby iCloud účtu a uživatelské informace uloženy ve všech přitom o ochraně osobních údajů a zabezpečení uživatele.

Kontejnery plně spravuje vývojář aplikace prostřednictvím portálu WWDR. Obor názvů kontejneru je globální mezi všechny vývojáře Apple, takže kontejner nesmí být pouze jedinečné pro danou vývojář aplikace, ale pro všechny vývojáře Apple a aplikace.

Apple navrhuje pomocí zpětný zápis DNS při vytváření oboru názvů pro kontejnery aplikací. Příklad:

```csharp
iCloud.com.company-name.application-name
```

Kontejnery jsou sice, ve výchozím nastavení vázán 1: 1 na danou aplikaci, mohou být sdíleny aplikace. Na jediný kontejner může koordinovat tak více aplikací. Jednu aplikaci může také komunikovat několika kontejnerů.

### <a name="databases"></a>Databáze

Jedním z primární funkce CloudKit je trvat aplikace datový model a replikace tento model až servery serveru služby iCloud. Některé informace je určené pro uživatele, která ho vytvořila, další informace je veřejná data, která by bylo možné vytvořit uživatelem pro veřejné použití (např. Zkontrolujte restaurace) nebo může být informace, které vývojář byla publikována pro aplikaci. V obou případech cílová skupina není pouze jednoho uživatele, ale je komunita uživatelů.

 [![](intro-to-cloudkit-images/image32.png "Diagram CloudKit kontejneru")](intro-to-cloudkit-images/image32.png#lightbox)

V rámci kontejneru především je veřejný databáze. To je, kde všechny informace o veřejném žije a společně mingles. Kromě toho jsou několik jednotlivé privátní databáze pro každého uživatele aplikace.

Při spuštění v zařízení s iOS, aplikace budou mít pouze přístup k informacím Icloudu aktuálně přihlášeného uživatele. Zobrazení aplikace kontejneru tak bude vypadat takto:

 [![](intro-to-cloudkit-images/image33.png "Zobrazení aplikací kontejneru")](intro-to-cloudkit-images/image33.png#lightbox)

Můžete zobrazit pouze databázi veřejné a privátní databázi asociované s Icloudem aktuálně přihlášeného uživatele.

Databáze jsou viditelné v rámci CloudKit prostřednictvím `CKDatabase` třídy. Každá aplikace má přístup k dvě databáze: databázi veřejné a privátní jeden.

Kontejner je počáteční vstupní bod do CloudKit. Následující kód slouží pro přístup k veřejné a privátní databázi z aplikace výchozí kontejner:

```csharp
using CloudKit;
...

public CKDatabase PublicDatabase { get; set; }
public CKDatabase PrivateDatabase { get; set; }
...

// Get the default public and private databases for
// the application
PublicDatabase = CKContainer.DefaultContainer.PublicCloudDatabase;
PrivateDatabase = CKContainer.DefaultContainer.PrivateCloudDatabase;
```

Zde jsou rozdíly mezi typy databáze:

||Veřejné databáze|Soukromá databáze|
|---|--- |--- |
|**Datový typ**|Sdílená Data|Data aktuálního uživatele|
|**kvóta**|V úvahu kvóty pro vývojáře|V úvahu kvóty uživatele|
|**Výchozí oprávnění**|Čitelný World|Uživatele, čtení|
|**Úpravy oprávnění**|Icloudu řídicí panel rolí prostřednictvím úrovní záznam – třída|Není k dispozici|

### <a name="records"></a>Záznamy

Kontejnery uložení databáze a uvnitř databáze jsou záznamy. Záznamy jsou mechanismus, ve kterém je přesunout strukturovaných dat do a z CloudKit:

 [![](intro-to-cloudkit-images/image34.png "Kontejnery uložení databáze a uvnitř databáze jsou záznamy")](intro-to-cloudkit-images/image34.png#lightbox)

Záznamy jsou viditelné v rámci CloudKit prostřednictvím `CKRecord` třídy, která zabalí páry klíč hodnota. Instance objektu v aplikaci je ekvivalentní `CKRecord` v CloudKit. Kromě toho každý `CKRecord` má typ záznamu, který je ekvivalentem třídy objektu.

Záznamy mají schéma za běhu, takže data je popsaný na CloudKit před pro zpracování předáno. Od tohoto okamžiku budou CloudKit interpretovat informace a zpracovat logistiky ukládání a načítání záznamu.

`CKRecord` Třída také podporuje širokou škálu metadat. Například záznam obsahuje informace o vytvoření a uživatele, která ji vytvořila. Záznam také obsahuje informace o jeho poslední změny a uživatelem, který ji upravit.

Záznamy obsahovat představu o změnu značku. Toto je předchozí verze revize daného záznamu. Změna značky se používá jako způsob šedé – určení, zda klient a server mít stejnou verzi daného záznamu.

Jak jsme uvedli výše, `CKRecords` zabalení páry klíč hodnota a jako takový následující typy dat může být uložený v záznamu:

1.   `NSString`
1.   `NSNumber`
1.   `NSData`
1.   `NSDate`
1.   `CLLocation`
1.   `CKReferences`
1.   `CKAssets`


Kromě typů jednu hodnotu záznam může obsahovat jakýkoli z výše uvedených typů homogenního pole.

Následující kód slouží k vytvoření nového záznamu a uložit v databázi:

```csharp
using CloudKit;
...

private const string ReferenceItemRecordName = "ReferenceItems";
...

var newRecord = new CKRecord (ReferenceItemRecordName);
newRecord ["name"] = (NSString)nameTextField.Text;
await CloudManager.SaveAsync (newRecord);
```

### <a name="record-zones"></a>Záznam zóny

Zaznamenává neexistují samy o sobě v rámci na danou databázi – existují skupiny záznamů v zóně záznam společně. Záznam zóny můžete představit jako tabulky v tradičních relačních databází:

 [![](intro-to-cloudkit-images/image35.png "Existují skupiny záznamů společně uvnitř zónu záznam")](intro-to-cloudkit-images/image35.png#lightbox)

Může být více záznamů v rámci daného záznamu zóny a více záznamů zóny v rámci na danou databázi. Každá databáze obsahuje výchozí zónu záznam:

 [![](intro-to-cloudkit-images/image36.png "Každá databáze obsahuje výchozí záznam zóna a vlastní zóny")](intro-to-cloudkit-images/image36.png#lightbox)

Toto je, kde jsou uloženy záznamy ve výchozím nastavení. Kromě toho lze vytvořit vlastní záznam zóny. Zaznamenejte představují zóny se provádí základní členitosti, na které Atomic potvrzení a sledování změn.

## <a name="record-identifiers"></a>Identifikátory záznamů

Identifikátory záznam jsou reprezentovány jako řazené kolekce členů, s oběma typy klienta zadaný název záznamu a zónu, ve kterém existuje záznam. Záznam identifikátory mít následující vlastnosti:

-  Jsou vytvořeny pomocí klientské aplikace.
-  Tyto jsou plně normalized a představují konkrétního umístění záznamu.
-  Přiřazením jedinečné ID záznamu v databázi cizí název záznamu, můžete se používá pro přemostění místních databází, které nejsou uložené v rámci CloudKit.


Když vývojáři vytvářet nové záznamy, mohou vybrat možnost předat identifikátor záznamu. Pokud není zadán identifikátor záznamu, bude UUID automaticky vytvořit a přiřadit k záznamu.

Když vývojáři vytvořit nový záznam identifikátory, můžete se k určení záznamů zóny, která bude každý záznam patřit. Pokud není zadaný žádný, použije se výchozí pásmo záznamu.

Identifikátory záznam jsou viditelné v rámci CloudKit prostřednictvím `CKRecordID` třídy. Následující kód slouží k vytvoření nového záznamu identifikátoru:

```csharp
var recordID =  new CKRecordID("My Record");
```

### <a name="references"></a>Odkazy

Referenční materiály poskytují vztahů mezi související záznamy v rámci na danou databázi:

 [![](intro-to-cloudkit-images/image37.png "Referenční materiály poskytují vztahů mezi související záznamy v rámci na danou databázi")](intro-to-cloudkit-images/image37.png#lightbox)

V předchozím příkladu nadřazené položky vlastní podřízené objekty tak, aby dítě na podřízený záznam nadřazeného záznamu. Relace přejde z podřízeného záznamu na nadřazený záznam a se označuje jako *zpět odkaz*.

Odkazy jsou viditelné v rámci CloudKit prostřednictvím `CKReference` třídy. Jsou způsob umožníte serveru Icloudu pochopit vztahy mezi záznamy.

Referenční materiály poskytují mechanismus za kaskádových odstraní. Je-li nadřazený záznam odstraněna z databáze, všechny podřízené záznamy (jak je uvedeno v relaci) automaticky odstraní z databáze také.

> [!NOTE]
> Při používání CloudKit nepropojená ukazatele se možnost. Například v době aplikace načtených seznam záznamů ukazatele, vybrán záznam a poté požádat pro záznam záznam již neexistuje v databázi. Aplikace musí být napsané tak pohodlné zpracování této situace.

Není požadováno zpět odkazy jsou upřednostněny při práci s CloudKit Framework. Apple má upřesnění systému se nejúčinnější typ odkazu.

Při vytváření odkaz, vývojáře můžete zadat záznam, který je již v paměti nebo vytvořit odkaz na identifikátor záznamu. Pokud pomocí identifikátor záznamu a odkaz na zadaný nebylo v databázi existují, by se vytvořily nepropojená ukazatel.

Následuje příklad vytvoření odkaz na záznam známé:

```csharp
var reference = new CKReference(newRecord, new CKReferenceAction());
```

### <a name="assets"></a>Prostředky

Prostředky povolit pro soubor velký, nestrukturovaných dat nahrán do Icloudu a přidružené daného záznamu:

 [![](intro-to-cloudkit-images/image38.png "Prostředky povolit pro soubor velký, nestrukturovaných dat nahrán do Icloudu a přidružené daného záznamu")](intro-to-cloudkit-images/image38.png#lightbox)

Na straně klienta `CKRecord` se vytvoří soubor, který se budou odesílat na serveru služby iCloud server, který popisuje. A `CKAsset` je vytvořen tak, aby obsahovala soubor a je propojený s záznam popisující ho.

Když se soubor odešle na server, záznamu je umístěn v databázi a bude soubor zkopírován do speciální databáze velkokapacitní úložiště. Vytvoří propojení mezi záznam ukazatele a nahrávaný soubor.

Prostředky jsou viditelné v rámci CloudKit prostřednictvím `CKAsset` třídy a se používají k ukládání velkých, Nestrukturovaná data. Vzhledem k tomu, že vývojář se nikdy chce mít velký, nestrukturovaných dat v paměti, prostředky se implementují pomocí souborů na disku.

Prostředky jsou vlastněny záznamů, které umožňuje prostředky, které mají být načteny z Icloudu pomocí záznamu jako ukazatel. Tímto způsobem serveru můžete shromažďovat prostředky paměti při odstraňování záznamu, který je vlastníkem prostředku.

Protože `CKAssets` jsou určené zpracovávat velké datové soubory, Apple navržené CloudKit efektivně nahrávání a stahování prostředky.

Následující kód slouží k vytvoření Assetu a přidružte ji k záznamu:

```csharp
var fileUrl = new NSUrl("LargeFile.mov");
var asset = new CKAsset(fileUrl);
newRecord ["name"] = asset;
```

Nyní budeme mít zahrnuté všechny základní objektů v rámci CloudKit. Kontejnery jsou přidružené k aplikacím a obsahují databáze. Databáze obsahuje záznamy, které jsou seskupeny do zóny záznam a ukazuje identifikátory záznamu. Vztahů nadřazenosti a podřízenosti jsou definované mezi záznamů pomocí odkazů. Nakonec velkých souborů můžete nahrát a přidružené k záznamů pomocí prostředky.

## <a name="cloudkit-convenience-api"></a>CloudKit vhodné rozhraní API

Apple nabízí dvě rozhraní API sady pro práci s CloudKit:

-  **Provozní rozhraní API** – nabízí všechny jeden funkce CloudKit. Toto rozhraní API pro složitějších aplikací, poskytuje jemně odstupňovanou kontrolu nad CloudKit.
-  **Vhodné rozhraní API** – nabízí podmnožinu funkcí CloudKit běžné, předem nakonfigurovaná. Poskytuje řešení pohodlný a snadný přístup pro včetně CloudKit funkcí v aplikaci pro systém iOS.


Rozhraní API pohodlí je většinou nejlepší volbou pro většinu aplikací iOS a Apple navrhne spouštění s ním. Zbývající část tohoto oddílu popisuje v následujících tématech pohodlí rozhraní API:

-  Ukládání záznam.
-  Načítání záznamu.
-  Aktualizace záznamu.


### <a name="common-setup-code"></a>Běžné instalační kód

Před Začínáme s rozhraním API pro usnadnění práce CloudKit, je požadován kód některé standardní instalace. Začněte tím, že úpravy aplikace `AppDelegate.cs` souboru a nastavit jej vypadat třeba takto:

```csharp
using System;
using System.Collections.Generic;
using System.Linq;
using Foundation;
using UIKit;
using CloudKit;

namespace CloudKitAtlas
{
    [Register ("AppDelegate")]
    public partial class AppDelegate : UIApplicationDelegate
    {
        #region Computed Properties
        public override UIWindow Window { get; set;}
        public CKDatabase PublicDatabase { get; set; }
        public CKDatabase PrivateDatabase { get; set; }
        #endregion

        #region Override Methods
        public override bool FinishedLaunching (UIApplication application, NSDictionary launchOptions)
        {
            application.RegisterForRemoteNotifications ();

            // Get the default public and private databases for
            // the application
            PublicDatabase = CKContainer.DefaultContainer.PublicCloudDatabase;
            PrivateDatabase = CKContainer.DefaultContainer.PrivateCloudDatabase;

            return true;
        }

        public override void RegisteredForRemoteNotifications (UIApplication application, NSData deviceToken)
        {
            Console.WriteLine ("Registered for Push notifications with token: {0}", deviceToken);
        }

        public override void FailedToRegisterForRemoteNotifications (UIApplication application, NSError error)
        {
            Console.WriteLine ("Push subscription failed");
        }

        public override void ReceivedRemoteNotification (UIApplication application, NSDictionary userInfo)
        {
            Console.WriteLine ("Push received");
        }
        #endregion
    }
}
```

Ve výše uvedeném kódu zpřístupní CloudKit databáze veřejné a privátní jako zástupce, aby se usnadnilo pro práci s ve zbývající části aplikace.

Dál přidejte následující kód k žádnému zobrazení nebo zobrazení kontejneru, který bude používat CloudKit:

```csharp
using CloudKit;
...

#region Computed Properties
public AppDelegate ThisApp {
    get { return (AppDelegate)UIApplication.SharedApplication.Delegate; }
}
#endregion
```

Tento postup přidá zástupce pro zajištění `AppDelegate` a přístup k databázi veřejné a privátní zástupce vytvořili výše.

S tímto kódem na místě Podívejme se na implementace rozhraní API CloudKit usnadnění práce v aplikaci Xamarin iOS 8.

### <a name="saving-a-record"></a>Ukládání záznam

Pomocí vzoru uvedené výše, když hovoříte o záznamy, bude následující kód vytvoří nový záznam a uložte je do veřejné databáze pomocí rozhraní API pro práci v aplikaci:

```csharp
private const string ReferenceItemRecordName = "ReferenceItems";
...

// Create a new record
var newRecord = new CKRecord (ReferenceItemRecordName);
newRecord ["name"] = (NSString)nameTextField.Text;

// Save it to the database
ThisApp.PublicDatabase.SaveRecord(newRecord, (record, err) => {
    // Was there an error?
    if (err != null) {
        ...
    }
});
```

Tři věci, které je třeba o ve výše uvedeném kódu:

1.  Při volání `SaveRecord` metodu `PublicDatabase`, vývojář nemá k určení, jak data se odesílají, co zóny se zápisem do atd. Rozhraní API pro usnadnění práce se postará o všechny tyto podrobnosti sám sebe.
1.  Volání asynchronních a poskytuje rutina zpětného volání při dokončení volání pomocí úspěch nebo selhání. Pokud volání selže, bude poskytnuta chybovou zprávu.
1.  CloudKit neposkytuje místního úložiště nebo trvalost; je pouze médium pro přenos. Proto při požadavku na Uložit záznam okamžitě odesláním na servery serveru služby iCloud.


> [!NOTE]
> Z důvodu "míru ztrát" paketů mobilní síťové komunikace, kde připojení jsou neustále vyřazována nebo přerušit, jedním z první aspekty, které má vývojář musí při práci s CloudKit je zpracování chyb.

### <a name="fetching-a-record"></a>Načítání záznamu

K načtení záznamu s záznamem vytvoří a uloží úspěšně na serveru služby iCloud serveru, použijte následující kód:

```csharp
// Create a record ID and fetch the record from the database
var recordID = new CKRecordID("MyRecordName");
ThisApp.PublicDatabase.FetchRecord(recordID, (record, err) => {
    // Was there an error?
    if (err != null) {
        ...
    }
});
```

Ve výše uvedeném kódu jako v případě uložení záznamu asynchronní, je a vyžaduje zpracování skvělé chyb.

### <a name="updating-a-record"></a>Aktualizace záznamu

Po záznam získána ze serveru služby iCloud serverů, můžete upravte záznamu a uložte změny zpět do databáze použít následující kód:

```csharp
// Create a record ID and fetch the record from the database
var recordID = new CKRecordID("MyRecordName");
ThisApp.PublicDatabase.FetchRecord(recordID, (record, err) => {
    // Was there an error?
    if (err != null) {

    } else {
        // Modify the record
        record["name"] = (NSString)"New Name";

        // Save changes to database
        ThisApp.PublicDatabase.SaveRecord(record, (r, e) => {
            // Was there an error?
            if (e != null) {
                 ...
            }
        });
    }
});
```

`FetchRecord` Metodu `PublicDatabase` vrátí `CKRecord` Pokud bylo volání úspěšné. Aplikace se pak upraví záznam a volání `SaveRecord` znovu ke zpětnému zápisu změn do databáze.

Tato část ukazuje typický cyklus, který aplikace bude používat při práci s rozhraním API pro usnadnění práce CloudKit. Aplikace bude uložte záznamů na serveru služby iCloud, načtení těchto záznamů z Icloudu, upravit záznamy a uložit tyto změny zpět na serveru služby iCloud.

## <a name="designing-for-scalability"></a>Návrh a škálovatelnosti

Tento článek má dosavadní prohlédli ukládání a načítání aplikace celý objekt modelu ze serveru služby iCloud serverů pokaždé, když má být pracoval s. Zatímco tento přístup funguje dobře u malé množství dat a velmi malé uživatelské základny, není vhodné i když množství informace nebo uživatel základní zvyšuje.

### <a name="big-data-tiny-device"></a>Velké objemy dat, jen nepatrnou zařízení

Oblíbenější aplikaci stane další data v databázi a tím menší je taky je do mají mezipaměť celý dat na zařízení. Následující postupy lze tento problém vyřešit:

-  **Zachovat velkého množství dat v cloudu** – CloudKit byla navržena pro zpracování velkých objemů dat efektivně.
-  **Klient by měl zobrazit pouze řez těchto dat** – ukončila činnost úplné minimum dat potřebných k zpracovat všechny úlohy v daném okamžiku.
-  **Můžete změnit zobrazení klientů** – protože každý uživatel má jiné předvolby, řez zobrazily dat můžete změnit pro všechny uživatele a jednotlivé uživatele zobrazení jakékoli dané řezu může lišit.
-  **Klient použije dotazy a zaměřit se hlediska** – dotazy umožňují uživateli zobrazit malou podmnožinu větší datovou sadu, která existuje v rámci cloudu.


### <a name="queries"></a>Dotazy

Jak jsme uvedli výše, dotazy umožňuje vývojářům vyberte malou podmnožinu větší datovou sadu, která existuje v cloudu. Dotazy jsou viditelné v rámci CloudKit prostřednictvím `CKQuery` třídy.

Dotazu se kombinuje tři různé věci: typu záznamu ( `RecordType`), predikátu ( `NSPredicate`) a volitelně popisovač řazení ( `NSSortDescriptors`). CloudKit podporuje většinu `NSPredicate`.

#### <a name="supported-predicates"></a>Podporované predikáty

CloudKit podporuje následující typy `NSPredicates` při práci s dotazy:


1. Odpovídající záznamy, kde název je rovna hodnotě uložené v proměnné:
    
    ```
    NSPredicate.FromFormat(string.Format("name = '{0}'", recordName))
    ```
   
2. Umožňuje odpovídající byl založen na dynamické hodnotu klíče, takže klíč nemá znát v době kompilace:
    
    ```
    NSPredicate.FromFormat(string.Format("{0} = '{1}'", key, value))
    ```
    
3. Odpovídající záznamy, kde hodnota záznamu je větší než zadaná hodnota:
   
    ```
    NSPredicate.FromFormat(string.Format("start > {0}", (NSDate)date))
    ```

4. Odpovídající záznamy, kde je záznamu umístění v rámci 100 měřidla daného umístění:
    
    ```
    var location = new CLLocation(37.783,-122.404);
    var predicate = NSPredicate.FromFormat(string.Format("distanceToLocation:fromLocation(Location,{0}) < 100", location));
    ```

5. CloudKit podporuje tokenizovaná vyhledávání. Toto volání se vytvoří dvě tokeny, jeden pro `after` a druhý pro `session`. Vrátí záznam, který obsahuje tyto dvě tokenů:
    
    ```
    NSPredicate.FromFormat(string.Format("ALL tokenize({0}, 'Cdl') IN allTokens", "after session"))
    ```
    
 6. Podporuje CloudKit složené predikáty spojena `AND` operátor.
    
    ```
    NSPredicate.FromFormat(string.Format("start > {0} AND name = '{1}'", (NSDate)date, recordName))
    ```
    


#### <a name="creating-queries"></a>Vytváření dotazů

Následující kód slouží k vytvoření `CKQuery` v aplikaci Xamarin iOS 8:

```csharp
var recordName = "MyRec";
var predicate = NSPredicate.FromFormat(string.Format("name = '{0}'", recordName));
var query = new CKQuery("CloudRecords", predicate);
```

Nejprve vytvoří predikátu k výběru pouze záznamy, které odpovídají zadaným názvem. Pak vytvoří dotaz, který vybere záznamy daného typu záznamu, které odpovídají predikát.

#### <a name="performing-a-query"></a>Provedení dotazu

Po vytvoření dotaz, použijte následující kód k provedení dotazu a zpracování vrácená záznamů:

```csharp
var recordName = "MyRec";
var predicate = NSPredicate.FromFormat(string.Format("name = {0}", recordName));
var query = new CKQuery("CloudRecords", predicate);

ThisApp.PublicDatabase.PerformQuery(query, CKRecordZone.DefaultRecordZone().ZoneId, (NSArray results, NSError err) => {
    // Was there an error?
    if (err != null) {
       ...
    } else {
        // Process the returned records
        for(nint i = 0; i < results.Count; ++i) {
            var record = (CKRecord)results[i];
        }
    }
});
```

Ve výše uvedeném kódu trvá dotaz vytvořili výše a provede veřejné v databázi. Vzhledem k tomu, že není zadaný žádný záznam zóny, budou prohledány všechny zóny. Pokud nedošlo k žádným chybám, pole `CKRecords` bude vrácen odpovídající parametrům dotazu.

Způsob, jak můžete myslíte o dotazech je, že jsou hlasování a jsou skvělý při vytváření řezů prostřednictvím rozsáhlých datových sad. Dotazy, ale nejsou vhodné pro velké, většinou statické datové sady z následujících důvodů:

-  Jsou chybné pro výdrž baterie zařízení.
-  Jsou chybné pro síťový provoz.
-  Chybný pro činnost koncového uživatele jsou vzhledem k tomu, jak často se aplikace dotazuje databázi informace, které jim je omezena. Uživatelé dnes očekávat nabízená oznámení, když dojde ke změně.


### <a name="subscriptions"></a>Předplatná

Při plánování práce s velký, většinou statické datové sady, dotaz nebude prováděna na klientském zařízení, může spustit na serveru jménem klienta. Dotaz by měl spuštěný na pozadí a budou spuštěny po každé jeden záznam uložit podle aktuální zařízení nebo jiné zařízení zásahu do stejné databáze.

Nakonec nabízených oznámení by měly být odeslány na každé zařízení připojit k databázi při spuštění dotazu na straně serveru.

Odběry jsou viditelné v rámci CloudKit prostřednictvím `CKSubscription` třídy. Že kombinují typu záznamu ( `RecordType`), predikátu ( `NSPredicate`) a Apple Push Notification ( `Push`).

> [!NOTE]
> CloudKit oznámení jsou mírně rozšířen jako obsahují datové části obsahující CloudKit konkrétní informace, jako je například co způsobilo nabízené provést.

#### <a name="how-subscriptions-work"></a>Jak fungují odběrů

Před implementací předplatného v kódu jazyka C#, podívejme rychlý přehled o fungování odběry:

 [![](intro-to-cloudkit-images/image39.png "Přehled o fungování odběrů")](intro-to-cloudkit-images/image39.png#lightbox)

Výše uvedené graf zobrazuje proces typické předplatné následujícím způsobem:

1.  Klientské zařízení se vytvoří nový odběr obsahující sadu podmínek, které aktivují předplatné a nabízených oznámení, která bude odeslána, když dojde k aktivační události.
2.  Předplatné je odeslal do databáze, kde se přidá do kolekce existující odběry.
3.  Druhé zařízení vytvoří nový záznam a uloží záznamů do databáze.
4.  Databáze vyhledává ve svém seznamu odběrů, jestli nový záznam odpovídá některé z jejich podmínek.
5.  Pokud je nalezena shoda, nabízená oznámení odešle do zařízení, které registrované předplatné s informacemi o záznam, který způsobuje její neočekávané aktivovat.


Replikace na místě Podíváme se na vytvoření odběrů v Xamarin iOS 8 aplikace.

#### <a name="creating-subscriptions"></a>Vytvoření předplatných

Následující kód slouží k vytvoření odběru:

```csharp
// Create a new subscription
DateTime date;
var predicate = NSPredicate.FromFormat(string.Format("start > {0}", (NSDate)date));
var subscription = new CKSubscription("RecordType", predicate, CKSubscriptionOptions.FiresOnRecordCreation);

// Describe the type of notification
var notificationInfo = new CKNotificationInfo();
notificationInfo.AlertLocalizationKey = "LOCAL_NOTIFICATION_KEY";
notificationInfo.SoundName = "ping.aiff";
notificationInfo.ShouldBadge = true;

// Attach the notification info to the subscription
subscription.NotificationInfo = notificationInfo;
```

Nejprve vytvoří predikát, který poskytuje podmínky pro aktivaci předplatného. V dalším kroku vytvoří předplatné na konkrétní typ záznamu a nastaví možnost při testování aktivační událost. Nakonec definuje typ oznámení, který se stane, když předplatné se aktivuje a připojí jej předplatné.

### <a name="saving-subscriptions"></a>Ukládání odběrů

S předplatným vytvořit následující kód uloží ho do databáze:

```csharp
// Save the subscription to the database
ThisApp.PublicDatabase.SaveSubscription(subscription, (s, err) => {
    // Was there an error?
    if (err != null) {

    }
});
```

Pomocí rozhraní API pro usnadnění práce, volání je asynchronní, jednoduchý a poskytuje snadno chyba zpracování.

#### <a name="handling-push-notifications"></a>Zpracování nabízených oznámení

Vývojář se použil Apple nabízených oznámení (AP), proces se zabývá oznámení vygenerovaná CloudKit musí být známé.

V `AppDelegate.cs`, přepsat `ReceivedRemoteNotification` třídy následujícím způsobem:

```csharp
public override void ReceivedRemoteNotification (UIApplication application, NSDictionary userInfo)
{
    // Parse the notification into a CloudKit Notification
    var notification = CKNotification.FromRemoteNotificationDictionary (userInfo);

    // Get the body of the message
    var alertBody = notification.AlertBody;

    // Was this a query?
    if (notification.NotificationType == CKNotificationType.Query) {
        // Yes, convert to a query notification and get the record ID
        var query = notification as CKQueryNotification;
        var recordID = query.RecordId;
    }
}
```

Ve výše uvedeném kódu požádá CloudKit k analýze informací o uživateli do CloudKit oznámení. Dále je extrahovat informace o výstraze. Nakonec je testován typ oznámení a oznámení se zpracovává odpovídajícím způsobem.

V této části ukazuje, jak k odpovědi na velké objemy dat, problém jen nepatrnou zařízení uvedené výše pomocí dotazů a odběry. Aplikace bude ponechte jeho velkých objemů dat v cloudu a použijte tyto technologie k zajištění zobrazení do této datové sady.

## <a name="cloudkit-user-accounts"></a>CloudKit uživatelské účty

Jak je uvedeno na začátku tohoto článku, CloudKit je postavená na stávající infrastruktury serveru služby iCloud. Následující část se bude zabývat, podrobně, jak se zveřejňují účty pro vývojáře, pomocí rozhraní API CloudKit.

### <a name="authentication"></a>Ověřování

Při plánování práce s uživatelskými účty, je nejprve je třeba zvážit ověřování. CloudKit podporuje ověřování pomocí Icloudu aktuálně přihlášeného uživatele na zařízení. Ověřování probíhá na pozadí a zpracovává v iOS. Tímto způsobem vývojáři nikdy mají na starosti podrobnosti implementace ověřování. Testují, pouze pokud je uživatel přihlášen.

### <a name="user-account-information"></a>Informace o uživatelském účtu

CloudKit poskytuje následující informace o uživateli vývojáři:

-  **Identity** – způsob Jednoznačná identifikace uživatele.
-  **Metadata** – možnost uložení a načtení informací o uživatelích.
-  **Ochrana osobních údajů** – zpracovává všechny informace v vědomá toho manor ochrany osobních údajů. Nic je vystaven, pokud má k němu souhlas uživatele.
-  **Zjišťování** – poskytuje uživatelům možnost zjišťování svých přátel, kteří používají stejné aplikaci.


V dalším kroku se podíváme na tato témata podrobně.

#### <a name="identity"></a>Identita

Jak jsme uvedli výše, CloudKit poskytuje způsob, jak aplikace, který jednoznačně identifikuje daného uživatele:

 [![](intro-to-cloudkit-images/image40.png "Jednoznačně identifing daného uživatele")](intro-to-cloudkit-images/image40.png#lightbox)

Není spuštěna v zařízení uživatele a všechny konkrétní uživatel privátní databáze uvnitř kontejneru CloudKit klientské aplikace. Klientská aplikace bude propojit s jedním z těchto konkrétních uživatelů. To je založené na uživatele, který je přihlášen Icloudu místně na zařízení.

Protože to pochází ze serveru služby iCloud je s formátováním zálohování úložiště uživatelské informace. A protože Icloudu je ve skutečnosti hostitelem kontejneru, můžete koordinaci uživatelů. Ve výše uvedené, uživatel s účtem Icloudu `user@icloud.com` je propojený s aktuální klienta.

Na základě kontejneru podle kontejneru je jedinečný, náhodně vygenerované ID uživatele vytvořena a přidružena účtem Icloudu uživatele (e-mailová adresa). Toto ID uživatele se vrátí do aplikace a je možné libovolným způsobem vývojář považuje za vhodné.

> [!NOTE]
> Různé aplikace spuštěné na stejném zařízení pro stejného uživatele Icloudu bude mít jiné ID uživatele, protože jsou připojeny k různé CloudKit kontejnery.

Následující kód získá ID uživatele CloudKit pro aktuálně přihlášeného v Icloudu uživatele na zařízení:

```csharp
public CKRecordID UserID { get; set; }
...

// Get the CloudKit User ID
CKContainer.DefaultContainer.FetchUserRecordId ((recordID, err) => {
    // Was there an error?
    if (err!=null) {
        Console.WriteLine("Error: {0}", err.LocalizedDescription);
    } else {
        // Save user ID
        UserID = recordID;
    }
});
```

Ve výše uvedeném kódu je žádostí kontejneru CloudKit poskytnout ID aktuálně přihlášeného uživatele. Vzhledem k tomu, že tyto informace pochází ze serveru služby iCloud serveru, je asynchronní volání a musí se zpracování chyb.

#### <a name="metadata"></a>Metadata

Každý uživatel v CloudKit má konkrétní Metadata, která popisuje je. Tato Metadata je reprezentován jako záznam CloudKit:

 [![](intro-to-cloudkit-images/image41.png "Každý uživatel v CloudKit má konkrétní Metadata, která popisuje je")](intro-to-cloudkit-images/image41.png#lightbox)

Vyhledávání v databázi privátní pro konkrétního uživatele existuje kontejner je jeden záznam, který definuje tohoto uživatele. Existuje mnoho záznamů uživatele v databázi veřejné, jednu pro každého uživatele kontejneru. Jedním z nich bude mít ID záznamu, který odpovídá aktuálně přihlášeného uživatele ID záznamu.

Uživatelské záznamy v databázi veřejné jsou world čitelné. Budou se považují ve většině případů, jako obyčejnou záznam a s typem `CKRecordTypeUserRecord`. Tyto záznamy jsou vyhrazená pro systém a nejsou k dispozici pro dotazy.

Použijte následující kód pro přístup k záznamu uživatele:

```csharp
public CKRecord UserRecord { get; set; }
...

// Get the user's record
PublicDatabase.FetchRecord(UserID, (record ,er) => {
    //was there an error?
    if (er != null) {
        Console.WriteLine("Error: {0}", er.LocalizedDescription);
    } else {
        // Save the user record
        UserRecord = record;
    }
});
```

Ve výše uvedeném kódu je žádostí veřejné databáze, která se vrátit záznam uživatele pro uživatele, který je ID jsme získat přístup k výše. Vzhledem k tomu, že tyto informace pochází ze serveru služby iCloud serveru, je asynchronní volání a musí se zpracování chyb.

#### <a name="privacy"></a>Ochrana osobních údajů

CloudKit byl návrh, ve výchozím nastavení, ochrany osobních údajů aktuálně přihlášeného uživatele. Ve výchozím nastavení je vystaven žádné identifikovatelné osobní identifikační informace o uživateli. Existují případy, kdy aplikace bude vyžadovat omezené množství informací o uživateli.

V těchto případech aplikace požádat, aby uživatel poskytnout tyto informace. Dialogové okno zobrazí uživateli, které je vyzývají, aby přihlásit k vystavení informací o jejich účtu.

#### <a name="discovery"></a>Zjišťování

Za předpokladu, že se uživatel přihlásí k povolení aplikace omezený přístup k jejich informace o uživatelském účtu, mohou být zjistitelný jiným uživatelům aplikace:

 [![](intro-to-cloudkit-images/image42.png "Uživatel může být zjistitelný jiným uživatelům aplikace")](intro-to-cloudkit-images/image42.png#lightbox)

Klientská aplikace je rozhovoru na kontejner a kontejner je rozhovoru Icloudu na informace o uživateli přístup. Uživatel může poskytnout e-mailovou adresu a zjišťování můžete použít k získání informací o uživateli zpět. Volitelně ID uživatele lze také ke zjišťování informací o uživateli.

CloudKit také poskytuje způsob, jak zjišťovat informace o každý uživatel, který může být přátel aktuálně přihlášeného uživatele na serveru služby iCloud uživatele pomocí dotazu na celou adresáře. Proces CloudKit bude pro vyžádání obsahu v obraťte se na uživatele a pokud chcete zobrazit, pokud ji můžete najít další uživatele aplikace, která odpovídají tyto adresy používat e-mailové adresy.

To umožňuje využít kniha Kontaktujte uživatele bez poskytování přístupu k němu nebo žádosti o schválení přístup ke kontaktům aplikace. Současně je kontaktní informace k dispozici pro aplikaci, jen proces CloudKit má přístup.

K recap, existují tři různé druhy vstupy k dispozici pro zjišťování uživatelů:

-  **ID záznamu uživatele** – zjišťování lze provést pro ID uživatele aktuálně přihlášeného uživatele v CloudKit uživatele.
-  **E-mailovou adresu uživatele** – uživatel může zadat e-mailovou adresu a lze použít pro zjišťování.
-  **Obraťte se na kniha** – adresáře uživatele lze použít ke zjištění uživatelů aplikace, které mají stejnou e-mailovou adresu, jak je uvedeno v jejich kontakty.


Zjišťování uživatelů vrátí následující informace:

-  **ID záznamu uživatele** -jedinečné ID uživatele v databázi veřejné.
-  **První a poslední název** – uložen v databázi veřejné.


Tyto informace budou vracet jenom pro uživatele, kteří mají vyjádřit výslovný souhlas do zjišťování.

Následující kód bude zjišťovat informace o uživateli teď přihlášení do Icloudu na zařízení:

```csharp
public CKDiscoveredUserInfo UserInfo { get; set; }
...

// Get the user's metadata
CKContainer.DefaultContainer.DiscoverUserInfo(UserID, (info, e) => {
    // Was there an error?
    if (e != null) {
        Console.WriteLine("Error: {0}", e.LocalizedDescription);
    } else {
        // Save the user info
        UserInfo = info;
    }
});
```

Použijte následující kód k dotazování všichni uživatelé v seznamu obraťte se na:

```csharp
// Ask CloudKit for all of the user's friends information
CKContainer.DefaultContainer.DiscoverAllContactUserInfos((info, er) => {
    // Was there an error
    if (er != null) {
        Console.WriteLine("Error: {0}", er.LocalizedDescription);
    } else {
        // Process all returned records
        for(int i = 0; i < info.Count(); ++i) {
            // Grab a user
            var userInfo = info[i];
        }
    }
});
```

V této části budeme mít zahrnutých čtyři hlavní oblasti přístup k účtu uživatele, která poskytují CloudKit k aplikaci. Z získávání identit a Metadata uživatele, na zásady ochrany osobních údajů, jsou součástí CloudKit a nakonec možnost zjišťování jiných uživatelů aplikace.

## <a name="the-development-and-production-environments"></a>Vývoj a produkční prostředí

CloudKit poskytuje samostatných vývoj a produkční prostředí pro typy záznamů aplikace a data. Vývojové prostředí je flexibilnější prostředí, která je k dispozici pouze pro členy vývojový tým. Pokud aplikace přidá nové pole do záznamu a uloží záznamů ve vývojovém prostředí, server automaticky aktualizuje informace o schématu.

Vývojáři mohou pomocí této funkce lze provést změny schématu během vývoje, což šetří čas. Jeden přímý přístup paměti je, že po přidání pole na záznam, přidruženého k danému poli datový typ nelze změnit prostřednictvím kódu programu. Chcete-li změnit typ pole, musí vývojář odstranit pole v [řídicí panel CloudKit](https://icloud.developer.apple.com/dashboard/) a přidejte ho znovu s novým typem.

Před nasazením aplikace, vývojáře můžete migrovat jejich schéma a data do produkčního prostředí pomocí **řídicí panel CloudKit**. Při spuštění v provozním prostředí, server zabrání aplikaci ve změně schématu prostřednictvím kódu programu. Vývojáři stále provádět změny s **řídicí panel CloudKit** , ale pokusí přidat pole do záznamu v provozním prostředí následek chyby.

> [!NOTE]
> Simulátor pracuje pouze s iOS **vývojového prostředí**. Když je připraven k testování aplikace v Vývojář **provozním prostředí**, fyzickém zařízení iOS se vyžaduje.


## <a name="shipping-a-cloudkit-enabled-app"></a>Aplikace s povoleným přesouvání CloudKit

Před přesouvání aplikace, která používá CloudKit, bude potřeba nakonfigurovat ji tak k cíli **provozním prostředí CloudKit** nebo aplikace budou odmítnuty společností Apple.

Postupujte takto:

1. V sadě Visual Studio pro Ma, kompilace aplikace pro **verze** > **zařízení iOS**: 

    [![](intro-to-cloudkit-images/shipping01.png "Kompilace aplikace pro vydání")](intro-to-cloudkit-images/shipping01.png#lightbox)

2. Z **sestavení** nabídce vyberte možnost **archivu**: 

    [![](intro-to-cloudkit-images/shipping02.png "Vyberte archivu")](intro-to-cloudkit-images/shipping02.png#lightbox)

3. **Archivu** bude vytvořen a zobrazí v sadě Visual Studio pro Mac: 

    [![](intro-to-cloudkit-images/shipping03.png "Archiv se vytvoří a zobrazí")](intro-to-cloudkit-images/shipping03.png#lightbox)

4. Spustit **Xcode**.
5. Z **okno** nabídce vyberte možnost **organizátora**: 

    [![](intro-to-cloudkit-images/shipping04.png "Vyberte organizátoru.")](intro-to-cloudkit-images/shipping04.png#lightbox)

6. Vyberte archivu aplikace a klikněte na **exportovat...**  tlačítko: 

    [![](intro-to-cloudkit-images/shipping05.png "Aplikace archivu")](intro-to-cloudkit-images/shipping05.png#lightbox)
    
7. Vyberte metodu pro export a klikněte na **Další** tlačítko: 

    [![](intro-to-cloudkit-images/shipping06.png "Vyberte metodu pro export")](intro-to-cloudkit-images/shipping06.png#lightbox)

8. Vyberte **vývojový tým** z rozevíracího seznamu a klikněte na **zvolte** tlačítko: 

    [![](intro-to-cloudkit-images/shipping07.png "Z rozevíracího seznamu vyberte vývojový tým")](intro-to-cloudkit-images/shipping07.png#lightbox)

9. Vyberte **produkční** z rozevíracího seznamu a klikněte na **Další** tlačítko: 

    [![](intro-to-cloudkit-images/shipping08.png "Z rozevíracího seznamu vyberte produkční")](intro-to-cloudkit-images/shipping08.png#lightbox)

10. Zkontrolujte nastavení a klikněte na **exportovat** tlačítko: 

    [![](intro-to-cloudkit-images/shipping09.png "Zkontrolujte nastavení")](intro-to-cloudkit-images/shipping09.png#lightbox)

11. Vyberte umístění pro generování výsledné aplikace `.ipa` souboru.

Proces je podobný pro odesílání aplikace přímo do iTunes připojit, stačí kliknout na **odeslání...**  tlačítko místo exportu... Po výběru archivu v okně médií.

## <a name="when-to-use-cloudkit"></a>Kdy použít CloudKit

Jak jsme viděli v tomto článku, CloudKit poskytuje snadný způsob pro aplikaci k ukládání a načítání informací ze serverů serveru služby iCloud. Který výše uvedeného, CloudKit nemá zastaralé nebo přestat používat existující nástroje nebo rozhraní.

### <a name="use-cases"></a>Případy použití

Následující případy použití by měly pomoci vývojářům při rozhodování, kdy používat konkrétní Icloudu framework nebo technologie:

-  **Icloudu úložiště dvojic klíč-hodnota** – asynchronně udržuje malé množství dat v aktuálním stavu a je velmi užitečná pro práci s předvolby aplikace. Nicméně je omezené pro velmi malé množství informací.
-  **Icloudu jednotky** – postavená na existující Icloudu rozhraní API pro dokumenty a poskytuje jednoduché rozhraní API synchronizaci nestrukturovaných dat ze systému souborů. Poskytuje úplné offline mezipaměti na Mac OS X a služba je skvělá pro dokument zaměřená na aplikace.
-  **Icloudu základní Data** – umožňuje data replikace mezi všemi zařízení uživatele. Data jsou jednoho uživatele a výborně hodí pro zachování privátní, strukturovaných dat synchronizované.
-  **CloudKit** – poskytuje veřejná data obou struktura a hromadně a umožňuje zpracovávat velké datové sady a nestrukturovaných velkých souborů. Jeho vázanou na uživatele účtem Icloudu a poskytuje přesměruje přenosy dat klientů.


Zachování těchto případů použití pamatovat, vývojáři měli vybrat správný Icloudu technologie a obě aktuální požadovanou aplikaci funkce, které poskytují dobrý škálovatelnost růstem do budoucna.

## <a name="summary"></a>Souhrn

Rychlý úvod do rozhraní API CloudKit má zahrnuté v tomto článku. Jak zřídit a nakonfigurovat aplikace Xamarin iOS používat CloudKit se má zobrazovat. Které funkce rozhraní API CloudKit pohodlí. Obsahuje zobrazení návrhu CloudKit povolené aplikace pro škálovatelnost pomocí dotazů a odběry. A nakonec se má zobrazovat informace uživatelského účtu, které je vystavené CloudKit k aplikaci.

## <a name="related-links"></a>Související odkazy

- [CloudKitAtlas (ukázka)](https://developer.xamarin.com/samples/monotouch/ios8/CloudKitAtlas/)
- [Úvod do iOSu 8](~/ios/platform/introduction-to-ios8.md)
- [Vytvoření profilu pro zřizování](~/ios/get-started/installation/device-provisioning/index.md)
