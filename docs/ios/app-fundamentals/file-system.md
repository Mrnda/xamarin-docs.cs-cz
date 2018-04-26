---
title: Práce s systému souborů
description: Xamarin.iOS můžete použít stejné třídy System.IO pro práci s souborů a adresářů v iOS, který byste použili v jakékoli aplikaci .NET. Navzdory známé třídy a metody, ale iOS implementuje určitá omezení na soubory, které můžou vytvořit nebo získat přístup a také poskytuje zvláštní funkce pro určité adresáře. Tento článek popisuje těchto omezení a funkce a vysvětluje, jak funguje přístup k souborům aplikace pro Xamarin.iOS.
ms.prod: xamarin
ms.assetid: 37DF2F38-901E-8F8E-269A-5EE0CCD28C08
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/18/2017
ms.openlocfilehash: 0706e416861e5636413577d38bf524ce9184bc4d
ms.sourcegitcommit: dc882e9631b4ed52596b944a6fbbdde309346943
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/26/2018
---
# <a name="working-with-the-file-system"></a>Práce s systému souborů

_Xamarin.iOS můžete použít stejné třídy System.IO pro práci s souborů a adresářů v iOS, který byste použili v jakékoli aplikaci .NET. Navzdory známé třídy a metody, ale iOS implementuje určitá omezení na soubory, které můžou vytvořit nebo získat přístup a také poskytuje zvláštní funkce pro určité adresáře. Tento článek popisuje těchto omezení a funkce a vysvětluje, jak funguje přístup k souborům aplikace pro Xamarin.iOS._

Můžete použít Xamarin.iOS a `System.IO` třídy v *.NET základní třídy knihovny (BCL)* pro přístup k souboru systému iOS. `File` Třída umožňuje vytvářet, mazat a číst soubory a `Directory` třída umožňuje vytvořit, odstranit nebo vytvořit výčet obsah adresáře. Můžete také použít `Stream` podtřídy, což může poskytnout vyšší stupeň kontroly nad operací se soubory (jako je hledání kompresi nebo pozice v souboru).

iOS ukládá některé omezení na co můžete dělat aplikace pomocí systému souborů pro zachování zabezpečení dat aplikace a chránit uživatele před maligní aplikace. Tato omezení jsou součástí *karanténě aplikace* – sadu pravidel, která omezuje přístup aplikaci na soubory, předvolby, síťové prostředky, hardware, atd. Aplikace je omezený na čtení a zápis souborů v rámci jeho domovského adresáře (umístění instalace); soubory jiná aplikace nemůže získat přístup.

iOS má také některé funkce specifické pro systém souborů: některých adresářů vyžadují speciální zacházení s ohledem na zálohování a upgrady a aplikace může také sdílení souborů přes iTunes.

Tento článek popisuje funkce a omezení IOS systém souborů podrobně a obsahuje ukázkovou aplikaci, která demonstruje použití Xamarin.iOS provést některé operace jednoduché souborového systému:

 [![](file-system-images/05-sampleapp.png "Ukázka provádění některých operací jednoduchého souboru systému iOS")](file-system-images/05-sampleapp.png#lightbox)

 <a name="General_File_Access" />


## <a name="general-file-access"></a>Přístup k souborům obecné

Xamarin.iOS umožňuje používat .NET `System.IO` třídy pro operace systému souborů v systému iOS.

Následující fragmenty kódu ukazují některé běžné operace souboru. Najdete je všechny níže v `SampleCode.cs` souboru v ukázkové aplikace pro tohoto článku.

 <a name="Working_with_directories" />


### <a name="working-with-directories"></a>Práce s adresáře

Tento kód vytvoří výčet podadresáře v aktuálním adresáři (určená elementem ". /" parametr), což je umístění spustitelného souboru vaší aplikace.
Výstup bude seznam všech souborů a složek, které jsou nasazeny s vaší aplikací (zobrazený v okně konzoly při ladění).

```csharp
var directories = Directory.EnumerateDirectories("./");
foreach (var directory in directories) {
      Console.WriteLine(directory);
}
```

 <a name="Reading_files" />


### <a name="reading-files"></a>Načítání souborů

Pro čtení z textového souboru, je zapotřebí pouze jeden řádek kódu. V tomto příkladu se zobrazí obsah textového souboru v okně výstupu aplikace.

```csharp
var text = File.ReadAllText("TestData/ReadMe.txt");
Console.WriteLine(text);
```

 <a name="XML_Serialization" />


### <a name="xml-serialization"></a>Serializace XML

I když práce s kompletní `System.Xml` obor názvů je nad rámec tohoto článku, můžete snadno rekonstruovat dokument XML z systému souborů pomocí třídy StreamReader takto:

```csharp
using (TextReader reader = new StreamReader("./TestData/test.xml")) {
      XmlSerializer serializer = new XmlSerializer(typeof(MyObject));
      var xml = (MyObject)serializer.Deserialize(reader);
}
```

V MSDN dokumentaci pro [System.Xml](http://msdn.microsoft.com/library/system.xml.aspx) obor názvů pro další informace o [serializace](http://msdn.microsoft.com/library/system.xml.serialization.aspx). Také byste měli revidovat [Xamarin.iOS dokumentace](~/ios/deploy-test/linker.md) na linkeru – obvykle budete muset přidat `[Preserve]` atribut třídy, které chcete serializovat.

 <a name="Creating_Files_and_Directories" />


### <a name="creating-files-and-directories"></a>Vytváření souborů a adresářů

Tento příklad ukazuje způsob použití `Environment` třídy pro přístup do složky Dokumenty, kde můžeme vytvořit souborů a adresářů.

```csharp
var documents =
 Environment.GetFolderPath (Environment.SpecialFolder.MyDocuments); 
var filename = Path.Combine (documents, "Write.txt");
File.WriteAllText(filename, "Write this text into a file");
```

Vytvoření adresáře je velmi podobný proces:

```csharp
var documents =
 Environment.GetFolderPath (Environment.SpecialFolder.MyDocuments);
var directoryname = Path.Combine (documents, "NewDirectory");
Directory.CreateDirectory(directoryname);
```

Další informace o System.IO – obor názvů najdete v tématu [dokumentace MSDN](http://msdn.microsoft.com/library/system.io.aspx).


### <a name="serializing-json"></a>Serializace Json

Práce s Json dat v aplikaci Xamarin.iOS je velmi snadné použití [Json.NET](http://www.newtonsoft.com/json) vysoce výkonné rozhraní JSON pro balíček NuGet .NET. Jednoduše přidejte balíček NuGet do projektu aplikace: 

[![](file-system-images/json01.png "Přidání balíček NuGet do projektu aplikace")](file-system-images/json01.png#lightbox)

V dalším kroku přidejte třídu tak, aby fungoval jako datový model pro serializaci nebo deserializaci (v tomto případě `Account.cs`):

```csharp
using System;
using System.Collections.Generic;
using Foundation; // for Preserve attribute, which helps serialization with Linking enabled

namespace FileSystem
{
    [Preserve]
    public class Account
    {
        #region Computed Properties
        public string Email { get; set; }
        public bool Active { get; set; }
        public DateTime CreatedDate { get; set; }
        public List<string> Roles { get; set; }
        #endregion

        #region Constructors
        public Account() {

        }
        #endregion
    }
}
```

Nakonec vytvořte instanci `Account` třídy, serializovat na json data a zapisovat do souboru:

```csharp
// Create a new record
var account = new Account(){
    Email = "monkey@xamarin.com",
    Active = true,
    CreatedDate = new DateTime(2015, 5, 27, 0, 0, 0, DateTimeKind.Utc),
    Roles = new List<string> {"User", "Admin"}
};

// Serialize object
var json = JsonConvert.SerializeObject(account, Newtonsoft.Json.Formatting.Indented);

// Save to file
var documents = Environment.GetFolderPath (Environment.SpecialFolder.MyDocuments);
var filename = Path.Combine (documents, "account.json");
File.WriteAllText(filename, json);
```
Odkazovat na Json .NET na [dokumentace](http://www.newtonsoft.com/json/help) Další informace o práci s daty json v aplikacích .NET.

<a name="Special_Considerations" />

## <a name="special-considerations"></a>Zvláštní upozornění

Bez ohledu podobnosti mezi Xamarin.iOS a .NET operací se soubory, iOS a Xamarin.iOS se liší od .NET některé důležité způsoby.

 <a name="runtimeaccessible" />


### <a name="making-project-files-accessible-at-runtime"></a>Zpřístupnění souborů projektu za běhu

Ve výchozím nastavení pokud přidá soubor do projektu, se nebudou zahrnuty do konečné sestavení a proto nebudou dostupné pro vaši aplikaci. Chcete-li zahrnout soubor sestavení, označte jej pomocí speciální sestavení akce, volá se obsah.

Označit souboru pro zahrnutí, klikněte pravým tlačítkem na soubory a zvolte **akce sestavení &gt; obsahu** v sadě Visual Studio for Mac. Můžete také změnit **akce sestavení** v souboru **vlastnosti** listu.

 <a name="Case_Sensitivity" />


### <a name="case-sensitivity"></a>Rozlišování velkých a malých písmen

Je důležité si uvědomit, že je systém souborů iOS *velká a malá písmena*. To znamená, že názvy souborů a adresářů musí přesně shodovat – Readme lze považovat za různé názvy souborů.

To může být matoucí pro vývojáře .NET, kteří znají více souborů systému Windows, což je *malá a velká písmena*– "Files", "FILES" a "files" by všechny označují do stejného adresáře.

Ano i když je zařízení s iOS se velká a malá písmena a váš kód by měly být zapsány s, nezapomeňte, iOS, které je simulátoru není případu velká a malá písmena ve výchozím nastavení. To znamená, že pokud vaše filename malá a velká písmena se liší mezi samotný soubor a na ni odkazují v kódu, kódu mohou i nadále fungovat v simulátoru ale, že dojde k selhání na skutečné zařízení. Toto je jedním z důvodů, proč je důležité k nasazení do samotného zařízení časná a často během vývoje iOS.

 <a name="Path_Separator" />


### <a name="path-separator"></a>Oddělovač cesty

iOS používá dopředné lomítko '/' jako oddělovač cesty (které se liší od systému Windows, který používá zpětné lomítko ' \').

Tento rozdíl matoucí, je vhodné používat `System.IO.Path.Combine` metodu, která upraví aktuální platformě, nikoli používat pevné kódování oddělovač konkrétní cesty. Toto je jednoduchá krok, která vytváří víc přenosného kódu pro jiné platformy.

 <a name="Application_Sandbox" />


## <a name="application-sandbox"></a>Izolovaný prostor aplikace

Vaše aplikace přístup k systému souborů (a jiným prostředkům, jako je například funkce sítě a hardwaru) je omezená z bezpečnostních důvodů. Toto omezení se označuje jako *karanténě aplikace*. Z hlediska systému souborů je omezená na vytváření a odstraňování souborů a adresářů v jeho domovského adresáře vaší aplikace.

Domovský adresář představuje jedinečné umístění v systému souborů, které jsou uloženy vaše aplikace a všechna jeho data. Nelze zvolit (nebo změnit) umístění domovský adresář pro vaši aplikaci; iOS a Xamarin.iOS poskytují ale vlastnosti a metody pro správu souborů a adresářů v.

 <a name="The_Application_Bundle" />


## <a name="the-application-bundle"></a>Sada aplikací

*Aplikace sady* je složka, která obsahuje aplikaci.
Je rozlišující z jiných složkách tak, že .app příponu, přidá do název adresáře. Vaší aplikace sady obsahuje spustitelný soubor a veškerý obsah (soubory, obrázky, atd.) nezbytné pro svůj projekt.

Při procházení k vaší sady aplikace v systému Mac OS, zobrazí se ikona jiný než se zobrazí v jiných adresářích (a příponou .app je skrytý); je však pouze regulární adresáře, který je jinak zobrazení operačního systému.

Pokud chcete zobrazit sady aplikací pro ukázkový kód, klikněte pravým tlačítkem na projekt v sadě Visual Studio pro Mac a vyberte **otevřít složku obsahující**. Potom přejděte na **bin/Debug/** kde byste měli najít ikony aplikace (podobně jako tento snímek obrazovky).

 [![](file-system-images/40-bundle.png "Přejděte do bin/Debug najít ikony aplikace podobně jako tento snímek obrazovky")](file-system-images/40-bundle.png#lightbox)

Klikněte pravým tlačítkem na tuto ikonu a vyberte **zobrazit obsah balíčku** procházet obsah adresáře sady aplikace. Obsah se zobrazí stejně jako obsah regulární adresáře, jak je vidět tady:

 [![](file-system-images/45-bundle.png "Obsah sady prostředků aplikace")](file-system-images/45-bundle.png#lightbox)

Je sada aplikací nainstalovaných v simulátoru, nebo na svém zařízení během testování a nakonec je co je odeslána společnosti Apple pro zařazení do obchodu s aplikacemi.

 <a name="Application_Directories" />


## <a name="application-directories"></a>Adresáře aplikace

Při instalaci aplikace na zařízení, operačního systému se vytvoří jeho domovského adresáře a umístí vaší aplikace sady uvnitř. Váš kód můžete přístup k sadě aplikace načíst data, ale nic by měly být zapsány do kořenového adresáře, jako je podepsaná a všechny změny budou zneplatnit vaší aplikace a zabránit jeho spuštění.

Ano, i když se nic by měly být zapsány do kořenového adresáře, <b>v iOS 7 a starší</b> vytvoří číslo z adresáře v rámci kořenového adresáře aplikace, které jsou k dispozici pro použití. <b>V iOS 8 jsou dostupné pro uživatele adresáře <a href="https://developer.apple.com/library/ios/technotes/tn2406/_index.html" target="_blank">není umístěný</a> v kořenovém adresáři aplikace</b>.

Tyto adresáře a jejich účely jsou uvedeny níže:

&nbsp;

|Adresář|Popis|
|---|---|
|[ApplicationName] .app nebo|**V iOS 7 a starší** jde `ApplicationBundle` adresáři, kde se ukládají vaše spustitelný soubor aplikace. Strukturu adresáře, který vytvoříte v aplikaci existuje v tomto adresáři (například obrázky a další typy souborů, které jste označili jako prostředky ve vaší sadě Visual Studio pro Mac projekt).<br /><br />Pokud potřebujete přístup k obsahu souborům uvnitř vaší aplikace sady, cesta k tento adresář je k dispozici prostřednictvím `NSBundle.MainBundle.BundlePath` vlastnost.|
|Dokumenty nebo|Použijte tento adresář k uložení dokumenty uživatele a datové soubory aplikace.<br /><br />Obsah tohoto adresáře může být dostupné pro uživatele prostřednictvím (i když je toto zakázáno ve výchozím nastavení) pro sdílení souborů iTunes. Přidat `UIFileSharingEnabled` Boolean klíč do souboru Info.plist umožníte uživatelům přístup k těmto souborům.<br /><br />I když aplikaci okamžitě neumožňuje sdílení souborů, neměli byste umístění souborů, které by měl být skrytá. uživatelé v tomto adresáři (například soubory databáze, pokud máte v úmyslu je sdílet). Tak dlouho, dokud citlivé soubory zůstávají skryté, tyto soubory nebude zveřejněné (a potenciálně přesunout, modified nebo deleted podle iTunes) Pokud je povoleno sdílení souborů v budoucí verzi.<br /><br /> Můžete použít `Environment.GetFolderPath (Environment.SpecialFolder.MyDocuments)` metodu za účelem získání cesty k adresáři dokumenty pro vaši aplikaci.<br /><br />Obsah tohoto adresáře jsou zálohovány pomocí iTunes.|
|Knihovna /|Adresář knihovny je vhodná k uložení souborů, které nebyly vytvořeny přímo pomocí uživatele, například databáze nebo jiné soubory generované aplikací. Obsah tohoto adresáře se nikdy zveřejňují pro uživatele na základě iTunes.<br /><br />Můžete vytvořit vlastní podadresáře v knihovně; Existují však již některé systém vytvořil adresáře tady, byste měli vědět, včetně předvoleb a mezipaměti.<br /><br />Obsah tohoto adresáře (s výjimkou mezipamětí podadresáři) jsou zálohovány pomocí iTunes. Vlastní adresáře, které vytvoříte v knihovně, budou zálohovány.|
|Knihovna/Předvolby /|Soubory specifické pro aplikaci předvoleb jsou uloženy v tomto adresáři. Přímo nevytvářejte tyto soubory. Místo toho použijte `NSUserDefaults` třídy.<br /><br />Obsah tohoto adresáře jsou zálohovány pomocí iTunes.|
|Knihovna/mezipamětí /|Adresář mezipaměti je vhodná k ukládají datové soubory, které mohou pomoci aplikaci spustit, ale který lze snadno znovu vytvořit v případě potřeby. Aplikace by měl vytvořit a odstraňte tyto soubory podle potřeby a nebude moci v případě potřeby znovu vytvořit tyto soubory. systém iOS 5 může také odstranit tyto soubory (v situacích, velmi nízkou úložiště), ale nebudou ji tak učinit, když aplikace běží.<br /><br />Obsah tohoto adresáře není zálohované serverem iTunes, což znamená, že nebudou přítomen, pokud uživatel na zařízení obnoví, a nemusí být dispozici po nainstalování aktualizovanou verzi vaší aplikace.<br /><br />Například v případě, že aplikace nemůže připojit k síti, můžete použít k adresáři mezipaměti pro ukládání dat a souborů a poskytuje dobrý offline prostředí. Můžete uložit a tato data načíst rychle při čekání na odezvu síťové aplikace, ale ji není třeba zálohovat a lze snadno obnovit nebo znovu vytvořit po dokončení obnovení nebo verzi aktualizace.|
|tmp/|Aplikace může ukládat dočasné soubory, které jsou vyžadovány pouze na krátkou dobu v tomto adresáři. Kvůli úspoře místa mají být odstraněny soubory, pokud už je potřeba. Operační systém může také odstranit soubory z tohoto adresáře Pokud aplikace není spuštěna.<br /><br />Obsah tohoto adresáře není zálohované serverem iTunes.<br /><br />Například může být adresáři tmp použít k ukládání dočasných souborů, které se stáhnou pro zobrazení pro uživatele (například avatarů Twitter nebo přílohy e-mailu), ale který může odstranit po jejich jste se zobrazit (a znovu stáhnout, pokud je v budoucnu potřebovat ).|

Tento snímek obrazovky ukazuje strukturu adresáře v okně Vyhledávací:

 [![](file-system-images/08-library-directory.png "Tento snímek obrazovky ukazuje strukturu adresáře v okně Finder")](file-system-images/08-library-directory.png#lightbox)

 <a name="Accessing_Other_Directories_Programmatically" />

### <a name="accessing-other-directories-programmatically"></a>Přístup k další adresáře prostřednictvím kódu programu

Starší adresáře a souboru příklady získat přístup `Documents` adresáře. Zápis do jiného adresáře, je nutné vytvořit cestu pomocí "..." syntaxe, jak je vidět tady:

```csharp
var documents = Environment.GetFolderPath (Environment.SpecialFolder.MyDocuments);
var library = Path.Combine (documents, "..", "Library");
var filename = Path.Combine (library, "WriteToLibrary.txt");
File.WriteAllText(filename, "Write this text into a file in Library");
```

Vytvoření adresáře je velmi podobný jako:

```csharp
var documents = Environment.GetFolderPath (Environment.SpecialFolder.MyDocuments);
var library = Path.Combine (documents, "..", "Library");
var directoryname = Path.Combine (library, "NewLibraryDirectory");
Directory.CreateDirectory(directoryname);
```

Cesty, které se `Caches` a `tmp` adresáře konstruovat takto:

```csharp
var documents = Environment.GetFolderPath (Environment.SpecialFolder.MyDocuments);
var cache = Path.Combine (documents, "..", "Library", "Caches");
var tmp = Path.Combine (documents, "..", "tmp");
```

 <a name="Sharing_Files_with_the_User_through_iTunes" />


## <a name="sharing-files-with-the-user-through-itunes"></a>Sdílení souborů s uživatelem prostřednictvím iTunes

Uživatelé přístup k souborům v adresáři vaší aplikace dokumenty úpravou `Info.plist` a vytváření **aplikace podporuje sdílení iTunes** (`UIFileSharingEnabled`) záznam v **zdroj** zobrazení, jako Zde se zobrazují:

 [![](file-system-images/09-uifilesharingenabled-plist.png "Přidání aplikace podporuje iTunes sdílení vlastnost")](file-system-images/09-uifilesharingenabled-plist.png#lightbox)

Tyto soubory jsou přístupné v iTunes při připojení zařízení a uživatel zvolí `Apps` kartě. Například následující snímek obrazovky ukazuje soubory ve vybrané aplikaci sdílet přes iTunes:

 [![](file-system-images/10-itunes-file-sharing.png "Tento snímek obrazovky ukazuje soubory ve vybrané aplikaci sdílet přes iTunes")](file-system-images/10-itunes-file-sharing.png#lightbox)

Uživatelé můžou používat pouze položky na nejvyšší úrovni v tomto adresáři přes iTunes. Obsah jakéhokoliv podadresáře nemohou zobrazit (i když mohou zkopírujte je do jejich počítače nebo je odstranit). Například s GoodReader, souborů PDF a EPUB lze sdílet s aplikací tak, aby uživatelé mohou číst na jejich zařízeních s iOS.

Uživatelé, kteří upravovat obsah jejich složky Dokumenty může způsobit problémy, pokud nejsou opatrní. Vaše aplikace by měla pamatujte na to a být odolné vůči destruktivní aktualizace složky Dokumenty.

Ukázkový kód pro tento článek vytvoří soubor a složku ve složce Dokumenty (v **SampleCode.cs**) a umožňuje sdílení souborů v **Info.plist** souboru. Tento snímek obrazovky ukazuje, jak se zobrazují v iTunes:

 [![](file-system-images/15-itunes-file-sharing-example.png "Tento snímek obrazovky ukazuje, jak se soubory se zobrazí v iTunes")](file-system-images/15-itunes-file-sharing-example.png#lightbox)

Odkazovat [práce s obrázky](~/ios/app-fundamentals/images-icons/index.md) informace o tom, jak nastavení ikon pro aplikace a pro všechny typy vlastní dokumentu vytvoříte článku.

Pokud `UIFileSharingEnabled` klíč je false nebo nejsou k dispozici, pak sdílení souborů je ve výchozím nastavení zakázána a uživatelé nebudou moci pracovat s vaší Documentsdirectory.

 <a name="Backup_and_Restore" />

## <a name="backup-and-restore"></a>Zálohování a obnovení

Když je zařízení zálohována iTunes, všechny adresáře, které jsou vytvořené v domovském adresáři vaší aplikace se uloží s následující výjimkou:

-   **[ApplicationName] .app** – aplikaci sady *nemá* získat zálohovat, ale jenom v případě, že došlo ke změně sady (při aktualizaci aplikace je například nainstalovaná). Tento adresář neměli upravovat i přesto, vzhledem k tomu, že je podepsaný a proto musí zůstat beze změny, po instalaci. 
-   **Knihovna/mezipamětí** – adresář mezipaměti je určený pro pracovní soubory, které nemusejí být zálohovány. 
-   **TMP** – tento adresář se používá pro dočasné soubory, které jsou vytvořeny a odstranit, pokud již nejsou potřebné, nebo pro soubory odstraní tento iOS když potřebuje místa. 


Zálohování velké množství dat může trvat dlouhou dobu. Pokud se rozhodnete, je třeba zálohovat všechny konkrétní dokumenty a data, aplikace by měla používat pouze dokumenty a knihovna složky pro tento. Přechodný dat nebo soubory, které se dá snadno načíst ze sítě použijte mezipaměti nebo adresáři tmp.

Upozorňujeme, že iOS bude 'vyčistit, systém souborů v případě zařízení zásadní místa na disku. Tento proces budou odebrány všechny soubory z knihovny nebo mezipaměti a tmp složky aplikací, které nejsou aktuálně spuštěna.

 <a name="Complying_with_iOS5_iCloud_Backup_Restrictions" />

## <a name="complying-with-ios5-icloud-backup-restrictions"></a>V souladu s Icloudem iOS5 zálohování omezení

Apple zavedená *Icloudu zálohování* funkce s iOS 5. Pokud je povolená Icloudu zálohování, všechny soubory ve vaší aplikace domovského adresáře (s výjimkou adresáře, které nejsou zálohovány normálně, například aplikace sady, `Caches` a `tmp`) jsou zálohovaná na serveru služby iCloud servery. Tato funkce poskytuje uživatele pomocí úplné zálohy v případě ztráty, krádeže nebo poškozený jejich zařízení.

Protože Icloudu pouze poskytuje 5Gb místa na "free", každý uživatel a vyhnout se zbytečně využití šířky pásma, Apple očekává jenom zálohování základní uživatelem generovaný data aplikací. Pro dosažení souladu s iOS pokyny úložiště dat byste měli omezit množství dat, který získá zálohovaná se následující položky:

-  Uložte pouze uživatelem generovaný data nebo data, která nemůže být jinak znovu vytvořena, v adresáři dokumentů (což je zálohovaná). 
-  Ukládat další data, která se dá snadno znovu vytvoří nebo znovu stáhnout v `Library/Caches` nebo `tmp` (který není zálohovaná a může být 'čištění'). 
-  Pokud máte soubory, které může být vhodné pro `Library/Caches` nebo `tmp` složky, ale nechcete, aby, čištění,, uchováváte jinde (, jako `Library/YourData` ) a použít ' Nezálohujte nahoru ' atribut zabránit tím ba zálohování serveru služby iCloud soubory ndwidth a úložný prostor. Tato data se stále používá místo v zařízení, měli byste pečlivě k její správě a odstranit ji, pokud je to možné. 

' Nezálohujte nahoru ' nastavený atribut, pomocí `NSFileManager` – třída. Zajistěte třídě `using Foundation` a volání `SetSkipBackupAttribute` podobné výjimky:

```csharp
var documents = Environment.GetFolderPath (Environment.SpecialFolder.MyDocuments);
var filename = Path.Combine (documents, "LocalOnly.txt");
File.WriteAllText(filename, "This file will never get backed-up. It would need to be re-created after a restore or re-install");
NSFileManager.SetSkipBackupAttribute (filename, true); // backup will be skipped for this file
```

Když `SetSkipBackupAttribute` je `true` soubor nebude zálohovaná, bez ohledu na to, je uložen v adresáři (i na `Documents` directory). Můžete zadat dotaz na atribut pomocí `GetSkipBackupAttribute` metodu a můžete ho resetovat voláním `SetSkipBackupAttribute` metoda s `false`, podobné výjimky:

```csharp
NSFileManager.SetSkipBackupAttribute (filename, false); // file will be backed-up
```

## <a name="sharing-data-between-ios-apps-and-app-extensions"></a>Sdílení dat mezi iOS aplikace a přípony aplikace

Vzhledem k tomu, že rozšíření aplikace spouštět jako součást aplikace hostitele (na rozdíl od jejich obsahující aplikace), sdílení dat neprobíhá automaticky zahrnuty, třeba další práci. Skupiny aplikací jsou že používá mechanismus iOS umožňující různé aplikace sdílet data. Pokud aplikace nejsou správně konfigurována s správná oprávnění a zřizování, získají přístup k sdíleného adresáře mimo jejich izolovaného prostoru normální iOS.

### <a name="configure-an-app-group"></a>Konfigurace skupiny aplikací

Sdílené umístění je konfigurován pomocí [aplikace skupiny](https://developer.apple.com/library/ios/documentation/Miscellaneous/Reference/EntitlementKeyReference/Chapters/EnablingAppSandbox.html#//apple_ref/doc/uid/TP40011195-CH4-SW19), která je nakonfigurována v **certifikáty, identifikátory a profily** části na [iOS Dev Center](https://developer.apple.com/devcenter/ios/). Tato hodnota musí také odkazovat v každém projektu **Entitlements.plist**.

Informace o vytváření a konfiguraci skupinu aplikací, najdete v části [možnosti skupiny aplikace](~/ios/deploy-test/provisioning/capabilities/app-groups-capabilities.md) průvodce.

### <a name="files"></a>Soubory

Aplikace pro iOS a rozšíření můžete také sdílet soubory pomocí běžných cesta k souboru (danou nejsou správně konfigurována s správná oprávnění a zřizování):

```csharp
var FileManager = new NSFileManager ();
var appGroupContainer =FileManager.GetContainerUrl ("group.com.xamarin.WatchSettings");
var appGroupContainerPath = appGroupContainer.Path

Console.WriteLine ("Group Path: " + appGroupContainerPath);

// use the path to create and update files
...
```
> [!IMPORTANT]
> Pokud cesta skupiny vrácená `null`, zkontrolujte konfiguraci oprávnění a profilu pro zřizování a ujistěte se, že jsou správná.


<a name="Application_Version_Updates" />

## <a name="application-version-updates"></a>Aktualizace aplikace verzí

Po stažení nové verze aplikace iOS vytvoří nový domovský adresář a ukládá nové sady aplikace v ní. iOS následně přesunuto následující složky z předchozí verze vaší aplikace sady do nové domovskému adresáři:

-  **Dokumenty**
-  **Knihovna**


Další adresáře může být také zkopírovat mezi a umístit pod domovského adresáře nové, ale se nezaručuje ke kopírování, takže aplikace neměli spoléhat na toto chování systému.

<a name="Summary" />

## <a name="summary"></a>Souhrn

Tento článek vám ukázal, že jsou operace souborového systému jako jednoduchý Xamarin.iOS stejně jako u jakékoli jiné aplikace rozhraní .NET. Také zavedená izolovaného prostoru aplikací a zkontrolován důsledky zabezpečení, které způsobuje, že. V dalším kroku prozkoumali konceptu sadě aplikací. Nakonec uvedené specializované adresáře, které jsou k dispozici pro vaše aplikace a vysvětlení jejich rolí během upgradů aplikací a zálohování.


## <a name="related-links"></a>Související odkazy

- [FileSystemSampleCode](https://developer.xamarin.com/samples/FileSystemSampleCode/)
- [Průvodce programováním v systému souborů](http://developer.apple.com/library/ios/#documentation/FileManagement/Conceptual/FileSystemProgrammingGUide/Introduction/Introduction.html)
- [Vaše aplikace podporuje registraci soubor typy.](http://developer.apple.com/library/ios/#documentation/FileManagement/Conceptual/DocumentInteraction_TopicsForIOS/Articles/RegisteringtheFileTypesYourAppSupports.html#/apple_ref/doc/uid/TP40010411-SW1)
