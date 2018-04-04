---
title: HomeKit
description: HomeKit je architektura společnosti Apple pro řízení zařízení domácí automatizace. Tento článek představuje HomeKit a popisuje konfiguraci testovací příslušenství v simulátoru HomeKit příslušenství a zápis jednoduchou aplikaci Xamarin.iOS k interakci se tyto příslušenství.
ms.prod: xamarin
ms.assetid: 90C0C553-916B-46B1-AD52-1E7332792283
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/22/2017
ms.openlocfilehash: 5699465330a4d2a5b983ec7661f80c1ed4f14bde
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/04/2018
---
# <a name="homekit"></a>HomeKit

_HomeKit je architektura společnosti Apple pro řízení zařízení domácí automatizace. Tento článek představuje HomeKit a popisuje konfiguraci testovací příslušenství v simulátoru HomeKit příslušenství a zápis jednoduchou aplikaci Xamarin.iOS k interakci se tyto příslušenství._

[![](homekit-images/accessory01.png "Aplikace s povoleným příklad HomeKit")](homekit-images/accessory01.png#lightbox)

Společnost Apple vydala HomeKit v iOS 8 jako způsob, jak se bezproblémově integruje více zařízení domácí automatizace od různých dodavatelů do jednoho, souvislý jednotky. Podporou společný protokol pro zjišťování, konfiguraci a řízení domácí automatizace zařízení HomeKit umožňuje zařízení bez vztahu výrobců spolupracují, aniž byste museli jednotlivých dodavatelů museli koordinaci úsilí.

S HomeKit můžete vytvořit aplikaci Xamarin.iOS, která řídí jakékoli HomeKit povolená zařízení bez použití dodavatele zadaný rozhraní API nebo aplikace. Pomocí HomeKit, můžete provést následující:

- Zjišťovat nové zařízení domácí automatizace HomeKit povolit a přidat je do databáze, která se zachová napříč všechny uživatele zařízení s iOS.
- Nastavení, konfiguraci, zobrazení a řídit jakémkoliv zařízení ve HomeKit _Domů konfigurační databáze_.
- Komunikovat s jakýmkoli předem nakonfigurovaná HomeKit a příkaz jejich provádět jednotlivé akce nebo ve vzájemné součinnosti, například zapnutí všechny indikátory v kuchyně fungovat.

Kromě obsluhující zařízení v Domů konfigurační databázi na HomeKit povolené aplikace, HomeKit poskytuje přístup k Siri hlasových příkazů. Vzhledem správně nakonfigurovaná instalační program HomeKit, uživatel může vydat hlasových příkazů, jako "Siri, zapnout indikátory v místnosti životností."

<a name="Home-Configuration-Database" />

## <a name="the-home-configuration-database"></a>Domácí konfigurační databáze

HomeKit uspořádá všechna zařízení automatizace v daném umístění do kolekce Domů. Tato kolekce poskytuje způsob pro uživatele k seskupení zařízení domácí automatizace do logicky uspořádaných jednotky s popisky smysluplný, čitelná pro člověka.

Kolekce domů je uložený v Domů konfigurační databázi, která bude automaticky zálohovaná a synchronizoval pro všechny uživatele zařízení s iOS. HomeKit obsahuje následující třídy pro práci s Domů konfigurační databáze:

- `HMHome` -Toto je nejvyšší úrovně kontejner, který obsahuje všechny informace a konfigurace pro všechny domovské automatizace zařízení na jednom fyzickém místě (např. jediný rodiny bydliště). Uživatel může mít více než jeden bydliště, jako je například jejich hlavní home a úklidové dovolené. Nebo mohou mít různé "je umístěno" stejné vlastnosti, jako je hlavní úklidové a úklidové hosta přes garáž. V obou případech alespoň jeden `HMHome` objekt _musí_ nastavit a uložit předtím, než mohou být zadány žádné další informace o HomeKit.
- `HMRoom` -Chvíli volitelné, `HMRoom` umožňuje uživateli zadat konkrétní místnostmi uvnitř domácí (`HMHome`), jako: kuchyně koupelnové, Garážový nebo místnosti životností. Uživatele můžete seskupit všechna zařízení domácí automatizace v určitém místě v jejich úklidové do `HMRoom` a použít je jako jednotku. Například žádostí Siri, chcete-li vypnout Garážový indikátory.
- `HMAccessory` -Představuje jednotlivce, fyzické HomeKit povoleno automatizace zařízení, které se nainstaloval bydliště uživatele (například inteligentní termostatu). Každý `HMAccessory` je přiřazena k `HMRoom`. Pokud uživatel nenakonfiguroval žádné místnostmi, přiřadí HomeKit příslušenství speciální výchozí místnosti.
- `HMService` -Představuje služby poskytované danou `HMAccessory`, jako je například stav zapnout nebo vypnout světlý nebo jeho barvu (Pokud je změna barev je podporována). Každý `HMAccessory` může mít více než jedna služba, jako je například otevírající dveře Garážový, která zahrnuje i lehkým. Navíc dané `HMAccessory` může mít služby, jako je aktualizace firmwaru, které jsou mimo uživatelský ovládací prvek.
- `HMZone` – Umožňuje uživatelům skupiny kolekce `HMRoom` objekty do logické zón, jako je například Upstairs, Downstairs nebo sklepení. Při volitelné, umožňuje interakce, jako je Siri s dotazem, chcete-li všechny světla downstairs vypnout.

<a name="Provisioning-a-HomeKit-App" />

## <a name="provisioning-a-homekit-app"></a>Zřizování aplikace HomeKit

Z důvodu požadavků zabezpečení uložených HomeKit aplikaci Xamarin.iOS, která používá rozhraní HomeKit musí být správně nakonfigurované i na portálu pro vývojáře Apple a v souboru projektu Xamarin.iOS.

Postupujte takto:

1. Přihlaste se [portál pro vývojáře Apple](http://developer.apple.com).
2. Klikněte na **certifikáty, identifikátory a profily**.
3. Pokud jste tak již neučinili, klikněte na **identifikátory** a vytvoření ID pro vaši aplikaci (například `com.company.appname`), jinak upravit své existující ID.
4. Ujistěte se, že **HomeKit** služby bude zkontrolován pro dané ID: 

    [![](homekit-images/provision01.png "Povolení služby HomeKit pro zadané ID.")](homekit-images/provision01.png#lightbox)
5. Uložte provedené změny.
4. Klikněte na **profily zřizování** > **vývoj** a vytvořte nový vývoj pro svoji aplikaci profil pro zřizování: 

    [![](homekit-images/provision02.png "Vytvořit nový vývoj zřizování profilu pro aplikaci")](homekit-images/provision02.png#lightbox)
5. Stáhnout a nainstalovat nový profil pro zřizování nebo pomocí Xcode stáhněte a nainstalujte profil.
6. Upravit možnosti projektu Xamarin.iOS a ujistěte se, že používáte profil zřizování, který jste právě vytvořili: 

    [![](homekit-images/provision03.png "Vyberte právě vytvořený profil zřizování")](homekit-images/provision03.png#lightbox)
7. Potom upravte vaše **Info.plist** souboru a ujistěte se, že používáte ID aplikace, která byla použita k vytvoření profilu pro zřizování: 

    [![](homekit-images/provision04.png "Nastavit ID aplikace ")](homekit-images/provision04.png#lightbox)
8. Nakonec upravte vaše **Entitlements.plist** souboru a ověřte, že **HomeKit** nárocích nebylo vybráno: 

    [![](homekit-images/provision05.png "Povolit HomeKit nároku")](homekit-images/provision05.png#lightbox)
9. Uložte změny do všechny soubory.

S těmito nastaveními zavedené aplikace je nyní připraven pro přístup k rozhraní API HomeKit Framework. Podrobné informace o zřizování, najdete v tématu naše [zřizování zařízení](~/ios/get-started/installation/device-provisioning/index.md) a [zřizování aplikace](~/ios/get-started/installation/device-provisioning/index.md) příručky.

> [!IMPORTANT]
> Testování aplikace HomeKit povoleno vyžaduje skutečné iOS zařízení, který správně zřízený pro vývoj. HomeKit nelze testovat z simulátoru iOS.

## <a name="the-homekit-accessory-simulator"></a>Simulátor HomeKit příslušenství

Zadejte způsob, jak otestovat všechny možné domácí automatizaci zařízení a služeb, aniž byste museli mít fyzického zařízení Apple vytvořena _HomeKit příslušenství simulátoru_. Pomocí této simulátoru, můžete nainstalovat a nakonfigurovat virtuální zařízení HomeKit.

### <a name="installing-the-simulator"></a>Instalace simulátoru

Apple poskytuje simulátoru příslušenství HomeKit jako samostatný soubor ke stažení ze Xcode, takže je potřeba nainstalovat ještě před pokračováním.

Postupujte takto:

1. Ve webovém prohlížeči, navštivte [soubory ke stažení pro vývojáře Apple](https://developer.apple.com/download/more/?name=for%20Xcode)
2. Stažení **další nástroje pro Xcode xxx** (kde xxx je verzi Xcode, který jste nainstalovali): 

    [![](homekit-images/simulator01.png "Stáhnout další nástroje pro Xcode")](homekit-images/simulator01.png#lightbox)
3. Otevřete bitové kopie disku a nainstalovat nástroje ve vaší **aplikace** adresáře.

S simulátoru příslušenství HomeKit nainstalován můžete vytvořit virtuální příslušenství pro testování.

### <a name="creating-virtual-accessories"></a>Vytvoření virtuální příslušenství

Pokud chcete spustit simulátoru HomeKit příslušenství a vytvořit několik virtuálních příslušenství, postupujte takto:

1. Ve složce aplikace spusťte simulátoru příslušenství HomeKit: 

    [![](homekit-images/simulator02.png "Simulátor HomeKit příslušenství")](homekit-images/simulator02.png#lightbox)
2. Klikněte **+** tlačítko a vyberte **nové příslušenství...** : 

    [![](homekit-images/simulator03.png "Přidat nové příslušenství")](homekit-images/simulator03.png#lightbox)
3. Zadejte informace o nové příslušenství a klikněte na **Dokončit** tlačítko: 

    [![](homekit-images/simulator04.png "Zadejte informace o nové příslušenství")](homekit-images/simulator04.png#lightbox)
4. Klikněte **přidat službu...** tlačítko a z rozevíracího seznamu vyberte typ služby: 

    [![](homekit-images/simulator05.png "Z rozevíracího seznamu vyberte typ služby")](homekit-images/simulator05.png#lightbox)
5. Zadejte **název** služby a klikněte na tlačítko **Dokončit** tlačítko: 

    [![](homekit-images/simulator06.png "Zadejte název pro službu")](homekit-images/simulator06.png#lightbox)
6. Volitelné vlastnosti můžete zadat pro službu kliknutím **přidat vlastnosti** tlačítko a konfiguraci požadovaných nastavení: 

    [![](homekit-images/simulator07.png "Požadovaná nastavení konfigurace.")](homekit-images/simulator07.png#lightbox)
7. Opakujte tento postup k vytvoření každého typu domácí automatizace virtuální zařízení, které podporuje HomeKit.

Některé ukázkové virtuální HomeKit příslušenství vytvořený a nakonfigurovaný můžete nyní využívat a řídit tato zařízení z vaší aplikace Xamarin.iOS.

## <a name="configuring-the-infoplist-file"></a>Konfigurace souboru Info.plist

Nové pro iOS 10 (a vyšší), bude nutné přidat Vývojář `NSHomeKitUsageDescription` klíče na aplikaci `Info.plist` souboru a zadejte řetězec deklarace, proč aplikace chce získat přístup k databázi HomeKit uživatele. Tento řetězec předloží čas uživatele při prvním spuštění aplikace:

[![](homekit-images/info01.png "Dialogové okno oprávnění HomeKit")](homekit-images/info01.png#lightbox)

Pokud chcete nastavit tento klíč, postupujte takto:

1. Dvakrát klikněte `Info.plist` souboru v **Průzkumníku řešení** otevřete pro úpravy.
2. V dolní části obrazovky, přepnout **zdroj** zobrazení.
3. Přidejte nový **položka** do seznamu.
4. Z rozevíracího seznamu vyberte **soukromí - HomeKit využití popis**: 

    [![](homekit-images/info02.png "Vyberte možnost ochrany osobních údajů – popis HomeKit využití")](homekit-images/info02.png#lightbox)
5. Zadejte popis proč aplikace chce získat přístup k databázi HomeKit uživatele: 

    [![](homekit-images/info03.png "Zadejte popis")](homekit-images/info03.png#lightbox)
6. Uložte změny do souboru.

> [!IMPORTANT]
> Nepodařilo se nastavit `NSHomeKitUsageDescription` klíče v `Info.plist` soubor bude mít za následek aplikace _bezobslužně chybě_ (dochází k uzavření systému za běhu) bez chyby při spuštění v iOS 10 (nebo vyšší).

## <a name="connecting-to-homekit"></a>Připojení k HomeKit

Ke komunikaci s HomeKit, vaše aplikace Xamarin.iOS musí nejprve vytvoří instanci `HMHomeManager` třídy. Správce domů je centrální vstupním bodem do HomeKit a zodpovídá za poskytování seznam dostupných domácnostech, aktualizace a údržba tohoto seznamu a vrátí uživatele _primární domovské_.

`HMHome` Objekt obsahuje všechny informace o udělení home, včetně všech místnostmi, skupiny a zón, které můžou obsahovat, společně s žádné příslušenství domácí automatizace, které byly nainstalovány. Před provedením jakékoli operace v HomeKit, alespoň jeden `HMHome` musíte vytvořit a přiřadit jako domovský primární.

Aplikace je zodpovědná za kontrolu, pokud existuje primární domovské a vytvoření a přiřazení jeden, pokud neexistuje.

### <a name="adding-a-home-manager"></a>Probíhá přidávání domácí Manager

Chcete-li přidat HomeKit sledování aplikace Xamarin.iOS, upravte **AppDelegate.cs** soubor pro úpravy a zajištění jeho vypadat třeba takto:

```csharp
using HomeKit;
...

public HMHomeManager HomeManager { get; set; }
...

public override void FinishedLaunching (UIApplication application)
{
    // Attach to the Home Manager
    HomeManager = new HMHomeManager ();
    Console.WriteLine ("{0} Home(s) defined in the Home Manager", HomeManager.Homes.Count());

    // Wire-up Home Manager Events
    HomeManager.DidAddHome += (sender, e) => {
        Console.WriteLine("Manager Added Home: {0}",e.Home);
    };

    HomeManager.DidRemoveHome += (sender, e) => {
        Console.WriteLine("Manager Removed Home: {0}",e.Home);
    };
    HomeManager.DidUpdateHomes += (sender, e) => {
        Console.WriteLine("Manager Updated Homes");
    };
    HomeManager.DidUpdatePrimaryHome += (sender, e) => {
        Console.WriteLine("Manager Updated Primary Home");
    };
}
```

Při prvním spuštění aplikace, uživatel se vyzve, zda chtějí povolit ho pro přístup k jejich HomeKit informace:

[![](homekit-images/home01.png "Uživatel bude požádán Pokud chtějí povolit ho pro přístup k jejich HomeKit informace")](homekit-images/home01.png#lightbox)

Pokud uživatel odpoví **OK**, bude moct pracovat s jejich HomeKit příslušenství aplikace v opačném případě není a volání HomeKit se nezdaří s chybou.

Pomocí Domů správce na místě potom aplikace bude muset vidět, pokud primární domovské nakonfigurovaný a pokud ne, poskytují způsob, jak uživatelům vytvořit a přiřadit jeden.

### <a name="accessing-the-primary-home"></a>Přístup k domovské primární

Jak jsme uvedli výše, musí být primární domovské vytvořit a nakonfigurovat před HomeKit je k dispozici a je, že aplikace zodpovědnost za poskytují způsob, jak vytvořit a přiřadit domovské primární, pokud uživatel ještě neexistuje.

Při prvním aplikace spustí nebo vrátí z na pozadí, je nutné sledovat `DidUpdateHomes` události `HMHomeManager` třída Zkontrolujte existenci domovské primární. Pokud žádný neexistuje, měl by poskytnout rozhraní pro uživatele k jeho vytvoření.

Následující kód mohou být přidány do řadiče zobrazení zkontrolujte domovské primární:

```csharp
using HomeKit;
...

public AppDelegate ThisApp {
    get { return (AppDelegate)UIApplication.SharedApplication.Delegate; }
}
...

// Wireup events
ThisApp.HomeManager.DidUpdateHomes += (sender, e) => {

    // Was a primary home found?
    if (ThisApp.HomeManager.PrimaryHome == null) {
        // Ask user to add a home
        PerformSegue("AddHomeSegue",this);
    }
};
```

Když správce Domů připojí k HomeKit, `DidUpdateHomes` událostí nebudou vydány, budou načteny všechny existující domácnostech manažera kolekce domácnostech a domovské primární budou načteny, pokud je k dispozici.

### <a name="adding-a-primary-home"></a>Přidání domovské primární

Pokud `PrimaryHome` vlastnost `HMHomeManager` je `null` po `DidUpdateHomes` událostí, musíte poskytnout způsob, jak uživatelům vytvořit a přiřadit domovské primární než budete pokračovat.

Aplikace obvykle nabídne formuláře k název nové domovské, který pak bude předána do správce domácí instalace jako domovský primární uživatele. Pro **HomeKitIntro** ukázkovou aplikaci, modální zobrazení byl vytvořen v Návrháři IOS a volá `AddHomeSegue` segue z rozhraní hlavní aplikace.

Poskytuje textové pole pro uživatele k zadání názvu pro nové home a tlačítko pro přidání na domovské stránce. Když uživatel klepnutím **přidat domovské** tlačítko následující kód volá domácí správce přidat na domovské stránce:

```csharp
// Add new home to HomeKit
ThisApp.HomeManager.AddHome(HomeName.Text,(home,error) =>{
    // Did an error occur
    if (error!=null) {
        // Yes, inform user
        AlertView.PresentOKAlert("Add Home Error",string.Format("Error adding {0}: {1}",HomeName.Text,error.LocalizedDescription),this);
        return;
    }

    // Make the primary house
    ThisApp.HomeManager.UpdatePrimaryHome(home,(err) => {
        // Error?
        if (err!=null) {
            // Inform user of error
            AlertView.PresentOKAlert("Add Home Error",string.Format("Unable to make this the primary home: {0}",err.LocalizedDescription),this);
            return ;
        }
    });

    // Close the window when the home is created
    DismissViewController(true,null);
});
```

`AddHome` Metoda se pokusí vytvořit nové domovské a obnoví v něm rutina daného zpětného volání. Pokud `error` vlastnost není `null`, došlo k chybě a by měla zobrazit uživatelům. Nejběžnějších chyb, jsou způsobeny jedinečný název domácí nebo domácí správce nebude moci komunikovat s HomeKit.

Pokud se na domovské stránce úspěšně vytvořil, je třeba volat `UpdatePrimaryHome` metodu a nastavit nové domovské jako domovský primární. Znovu Pokud `error` vlastnost není `null`, došlo k chybě a by měla zobrazit uživatelům.

Je třeba rovněž sledovat Domů manažera `DidAddHome` a `DidRemoveHome` události a aktualizace aplikace uživatelské rozhraní podle potřeby.

> [!IMPORTANT]
> `AlertView.PresentOKAlert` Metoda použitá ve výše uvedeném ukázkovém kódu je pomocná třída v HomeKitIntro aplikace, která vytvořila práce s iOS výstrahy jednodušší.


## <a name="finding-new-accessories"></a>Hledání nové příslušenství

Jakmile primární domovské byla definována nebo načíst ze domácí správce, můžete volat aplikace Xamarin.iOS `HMAccessoryBrowser` najít žádné nové příslušenství domácí automatizace a přidat je do síť doma.

Volání `StartSearchingForNewAccessories` metoda spusťte hledání nové příslušenství a `StopSearchingForNewAccessories` metoda po dokončení.

> [!IMPORTANT]
> `StartSearchingForNewAccessories` by nemělo být ponecháno systémem pro dlouhou dobu, protože budou mít nepříznivý vliv výdrž i výkon zařízení s iOS. Apple navrhuje volání `StopSearchingForNewAccessories` za minutu nebo pouze vyhledávání při rozhraní příslušenství najít pro uživatele.

`DidFindNewAccessory` Událostí bude volána, když se zjistí nové příslušenství a budou přidány do `DiscoveredAccessories` seznamu v prohlížeči příslušenství.

`DiscoveredAccessories` Seznam bude obsahovat kolekce `HMAccessory` objekty, které definují udělení HomeKit povoleno domácí automatizaci zařízení a jeho dostupné služby, jako je indikátory nebo Garážový dveře řízení.

Jakmile byl nalezen nový příslušenství, by měla zobrazit uživatelům a proto můžete jej vybrat a přidat jej do síť doma. Příklad:

[![](homekit-images/accessory01.png "Hledání nové příslušenství")](homekit-images/accessory01.png#lightbox)

Volání `AddAccessory` metoda přidáte vybrané příslušenství na domovské stránce kolekci. Příklad:

```csharp
// Add the requested accessory to the home
ThisApp.HomeManager.PrimaryHome.AddAccessory (_controller.AccessoryBrowser.DiscoveredAccessories [indexPath.Row], (err) => {
    // Did an error occur
    if (err !=null) {
        // Inform user of error
        AlertView.PresentOKAlert("Add Accessory Error",err.LocalizedDescription,_controller);
    }
});
```

Pokud `err` vlastnost není `null`, došlo k chybě a by měla zobrazit uživatelům. Jinak hodnota uživatele vyzve k zadání kódu instalační program pro zařízení, které chcete přidat:

[![](homekit-images/accessory02.png "Zadejte kód nastavení pro zařízení, které chcete přidat")](homekit-images/accessory02.png#lightbox)

V simulátoru příslušenství HomeKit toto číslo naleznete v části **instalační kód** pole:

[![](homekit-images/accessory03.png "Pole instalační kód v simulátoru HomeKit příslušenství")](homekit-images/accessory03.png#lightbox)

Pro skutečné HomeKit příslušenství instalační kód buď na budou vytištěny štítek na zařízení, samostatně, na pole produktu nebo v uživatelské příručce příslušenství.

Měli byste sledovat příslušenství prohlížeče `DidRemoveNewAccessory` událostí a aktualizace uživatel rozhraní odebrat ze seznamu dostupných určité příslušenství, jakmile uživatel byl přidán do kolekce jejich Domů.

## <a name="working-with-accessories"></a>Práce s příslušenství

Jednou domácí primární a byly vytvořeny a byla přidána příslušenství, k dispozici seznam příslušenství (a volitelně místnostmi) pro uživatele pro práci s.

`HMRoom` Objekt obsahuje všechny informace o daném místnosti a všechny příslušenství, které patří k němu. Místnostmi můžete volitelně uspořádány do několika zón. A `HMZone` obsahuje všechny informace o každé zóně a všechny místnostmi, které patří k němu.

Z důvodu v tomto příkladu budete jsme se udržuje věcí jednoduché a práce s síť doma příslušenství přímo, namísto uspořádáním do místnosti nebo pásma.

`HMHome` Objekt obsahuje seznam přiřazené příslušenství, kterou lze zobrazit na uživatele v jeho `Accessories` vlastnost. Příklad:

[![](homekit-images/accessory04.png "Příklad příslušenství")](homekit-images/accessory04.png#lightbox)

Zde formuláři, můžete vybrat daný příslušenství a pracovat s služby, které poskytuje uživatele.

## <a name="working-with-services"></a>Práce s služby

Když uživatel pracuje s daným zařízením povoleno domácí automatizace HomeKit, je obvykle prostřednictvím služby, které poskytuje. `Services` Vlastnost `HMAccessory` třída obsahuje kolekci `HMService` objekty, které definují služby zařízení nabízí.

Služby jsou věci jako indikátory, termostaty, Garážový dveře otevírající, přepínače nebo zámky. Některá zařízení (například otevírající dveře Garážový) bude poskytovat více služeb, jako je například lehkým a možnost spustit nebo zavřít dveří.

Kromě konkrétní služby, které poskytuje daný příslušenství, obsahuje každý Příslušenství `Information Service` , který definuje vlastnosti, například jeho název, výrobce, Model a sériové číslo.

### <a name="accessory-service-types"></a>Typy příslušenství služby

Následující typy služby jsou dostupné prostřednictvím `HMServiceType` výčtu:

- **AccessoryInformation** – poskytuje informace o zařízení dané domácí automatizace (Příslušenství).
- **AirQualitySensor** -definuje senzor letecké kvality.
- **Baterie** -definuje stav baterie určité příslušenství.
- **CarbonDioxideSensor** -definuje oxidu uhličitého senzoru.
- **CarbonMonoxideSensor** -definuje oxidu uhelnatého senzoru.
- **ContactSensor** -definuje kontaktní senzor (například okno otevření či zavření).
- **Dvířka** -definuje stav senzoru dveře (například otevřít nebo uzavřený).
- **Ventilátor** -definuje vzdálené řízené ventilátor.
- **GarageDoorOpener** -definuje otevírající dveře Garážový.
- **HumiditySensor** -definuje vlhkosti senzoru.
- **LeakSensor** -definuje úniku senzor (jako aktivní horních topení nebo prací počítače).
- **Žárovek** -definuje samostatný světlý nebo světlý, který je součástí jiného příslušenství (např. otevírající dveře Garážový).
- **LightSensor** -definuje senzoru světla.
- **LockManagement** -definuje službu, která spravuje zámek automatizované dveře.
- **LockMechanism** -definuje vzdálený zámek řízené (např. dvířka lock).
- **MotionSensor** -definuje snímač pohybu.
- **OccupancySensor** -definuje obsazení senzoru.
- **Výstupu** -definuje vzdálené řízené zásuvky.
- **SecuritySystem** -definuje domácí zabezpečení systému.
- **StatefulProgrammableSwitch** -definuje programovatelný přepínač, který zůstane v udělení stavu po aktivaci (např. flip přepínače).
- **StatelessProgrammableSwitch** -definuje programovatelný přepínač, který vrátí do původního stavu po aktivaci (např. tlačítka).
- **SmokeSensor** -definuje kouře senzoru.
- **Přepínač** -definuje o přepínač zapnout nebo vypnout jako standardní wall přepínače.
- **Senzor teploty** -definuje teplotní snímač.
- **Termostat** -definuje inteligentní termostatu použít k řízení TVK systému.
- **Okno** -definuje okno s automatizované, třtinového vzdáleně otevřít nebo uzavřený.
- **WindowCovering** -definuje okno vzdáleně řízený pokrývajících jako rolety, které se dá otevřít nebo uzavřený.

### <a name="displaying-service-information"></a>Zobrazení informací o služby

Po načtení `HMAccessory` můžete dotazovat jednotlivých `HNService` objekty poskytuje a zobrazit tyto informace pro uživatele:

[![](homekit-images/accessory05.png "Zobrazení informací o služby")](homekit-images/accessory05.png#lightbox)

Byste měli vždy by měl zkontrolovat `Reachable` vlastnost `HMAccessory` dřív, než se s ním pracovat. Určité příslušenství může být nedostupný uživatele není v rozsahu zařízení nebo pokud byl odpojen.

Jakmile služba byla vybrána, uživatele, můžete zobrazit nebo upravit jeden nebo více vlastností této služby na monitorovat nebo řídit zařízení dané domácí automatizace.

<a name="Working-with-Characteristics" />

## <a name="working-with-characteristics"></a>Práce s vlastnostmi

Každý `HMService` objekt může obsahovat kolekce `HMCharacteristic` objekty, které můžete zadat informace týkající se stavu služby (např. dveří otevření či zavření) nebo povolit uživatel může nastavit stav (např. nastavení barvy světla).

`HMCharacteristic` Ne jenom poskytuje informace o vlastnost a její stav, ale také poskytuje metody pro práci se stavem prostřednictvím _Metadata vlastnosti_ (`HMCharacteristisMetadata`). Tato metadata můžete zadat vlastnosti (například rozsahy minimální a maximální hodnota), které jsou užitečné při zobrazení informací o uživateli nebo povolením měnit stavy.

`HMCharacteristicType` Výčtu poskytuje sadu hodnot vlastnosti metadat, které nelze definovat ani upravit takto:

 - AdminOnlyAccess
 - AirParticulateDensity
 - AirParticulateSize
 - AirQuality
 - AudioFeedback
 - BatteryLevel
 - Také průraznost
 - CarbonDioxideDetected
 - CarbonDioxideLevel
 - CarbonDioxidePeakLevel
 - CarbonMonoxideDetected
 - CarbonMonoxideLevel
 - CarbonMonoxidePeakLevel
 - ChargingState
 - ContactState
 - CoolingThreshold
 - CurrentDoorState
 - CurrentHeatingCooling
 - CurrentHorizontalTilt
 - CurrentLightLevel
 - CurrentLockMechanismState
 - CurrentPosition
 - CurrentRelativeHumidity
 - CurrentSecuritySystemState
 - CurrentTemperature
 - CurrentVerticalTilt
 - FirmwareVersion
 - HardwareVersion
 - HeatingCoolingStatus
 - HeatingThreshold
 - HoldPosition
 - HUE
 - Identifikace
 - InputEvent
 - LeakDetected
 - LockManagementAutoSecureTimeout
 - LockManagementControlPoint
 - LockMechanismLastKnownAction
 - Protokoly
 - výrobce
 - Model
 - MotionDetected
 - Název
 - ObstructionDetected
 - OccupancyDetected
 - OutletInUse
 - OutputState
 - PositionState
 - PowerState
 - RotationDirection
 - RotationSpeed
 - Zpracováváním dat
 - sériové číslo
 - SmokeDetected
 - SoftwareVersion
 - StatusActive
 - StatusFault
 - StatusJammed
 - StatusLowBattery
 - StatusTampered
 - TargetDoorState
 - TargetHeatingCooling
 - TargetHorizontalTilt
 - TargetLockMechanismState
 - TargetPosition
 - TargetRelativeHumidity
 - TargetSecuritySystemState
 - TargetTemperature
 - TargetVerticalTilt
 - TemperatureUnits
 - Version

### <a name="working-with-a-characteristics-value"></a>Práce s hodnotou vlastnosti

Tuto aplikaci můžete má nejnovější stav dané vlastnosti voláním `ReadValue` metodu `HMCharacteristic` třídy. Pokud `err` vlastnost není `null`, došlo k chybě a může nebo nemusí být zobrazovat uživateli.

Na charakteristiku `Value` obsahuje aktuální stav daného vlastnost jako vlastnost `NSObject`a jako takový nelze pracovat s přímo v jazyce C#.

Načíst hodnotu, byla přidána následující pomocná třída do **HomeKitIntro** ukázkovou aplikaci:

```csharp
using System;
using Foundation;
using System.Globalization;
using CoreGraphics;

namespace HomeKitIntro
{
    /// <summary>
    /// NS object converter is a helper class that helps to convert NSObjects into
    /// C# objects
    /// </summary>
    public static class NSObjectConverter
    {
        #region Static Methods
        /// <summary>
        /// Converts to an object.
        /// </summary>
        /// <returns>The object.</returns>
        /// <param name="nsO">Ns o.</param>
        /// <param name="targetType">Target type.</param>
        public static Object ToObject (NSObject nsO, Type targetType)
        {
            if (nsO is NSString) {
                return nsO.ToString ();
            }

            if (nsO is NSDate) {
                var nsDate = (NSDate)nsO;
                return DateTime.SpecifyKind ((DateTime)nsDate, DateTimeKind.Unspecified);
            }

            if (nsO is NSDecimalNumber) {
                return decimal.Parse (nsO.ToString (), CultureInfo.InvariantCulture);
            }

            if (nsO is NSNumber) {
                var x = (NSNumber)nsO;

                switch (Type.GetTypeCode (targetType)) {
                case TypeCode.Boolean:
                    return x.BoolValue;
                case TypeCode.Char:
                    return Convert.ToChar (x.ByteValue);
                case TypeCode.SByte:
                    return x.SByteValue;
                case TypeCode.Byte:
                    return x.ByteValue;
                case TypeCode.Int16:
                    return x.Int16Value;
                case TypeCode.UInt16:
                    return x.UInt16Value;
                case TypeCode.Int32:
                    return x.Int32Value;
                case TypeCode.UInt32:
                    return x.UInt32Value;
                case TypeCode.Int64:
                    return x.Int64Value;
                case TypeCode.UInt64:
                    return x.UInt64Value;
                case TypeCode.Single:
                    return x.FloatValue;
                case TypeCode.Double:
                    return x.DoubleValue;
                }
            }

            if (nsO is NSValue) {
                var v = (NSValue)nsO;

                if (targetType == typeof(IntPtr)) {
                    return v.PointerValue;
                }

                if (targetType == typeof(CGSize)) {
                    return v.SizeFValue;
                }

                if (targetType == typeof(CGRect)) {
                    return v.RectangleFValue;
                }

                if (targetType == typeof(CGPoint)) {
                    return v.PointFValue;
                }
            }

            return nsO;
        }

        /// <summary>
        /// Convert to string
        /// </summary>
        /// <returns>The string.</returns>
        /// <param name="nsO">Ns o.</param>
        public static string ToString(NSObject nsO) {
            return (string)ToObject (nsO, typeof(string));
        }

        /// <summary>
        /// Convert to date time
        /// </summary>
        /// <returns>The date time.</returns>
        /// <param name="nsO">Ns o.</param>
        public static DateTime ToDateTime(NSObject nsO){
            return (DateTime)ToObject (nsO, typeof(DateTime));
        }

        /// <summary>
        /// Convert to decimal number
        /// </summary>
        /// <returns>The decimal.</returns>
        /// <param name="nsO">Ns o.</param>
        public static decimal ToDecimal(NSObject nsO){
            return (decimal)ToObject (nsO, typeof(decimal));
        }

        /// <summary>
        /// Convert to boolean
        /// </summary>
        /// <returns><c>true</c>, if bool was toed, <c>false</c> otherwise.</returns>
        /// <param name="nsO">Ns o.</param>
        public static bool ToBool(NSObject nsO){
            return (bool)ToObject (nsO, typeof(bool));
        }

        /// <summary>
        /// Convert to character
        /// </summary>
        /// <returns>The char.</returns>
        /// <param name="nsO">Ns o.</param>
        public static char ToChar(NSObject nsO){
            return (char)ToObject (nsO, typeof(char));
        }

        /// <summary>
        /// Convert to integer
        /// </summary>
        /// <returns>The int.</returns>
        /// <param name="nsO">Ns o.</param>
        public static int ToInt(NSObject nsO){
            return (int)ToObject (nsO, typeof(int));
        }

        /// <summary>
        /// Convert to float
        /// </summary>
        /// <returns>The float.</returns>
        /// <param name="nsO">Ns o.</param>
        public static float ToFloat(NSObject nsO){
            return (float)ToObject (nsO, typeof(float));
        }

        /// <summary>
        /// Converts to double
        /// </summary>
        /// <returns>The double.</returns>
        /// <param name="nsO">Ns o.</param>
        public static double ToDouble(NSObject nsO){
            return (double)ToObject (nsO, typeof(double));
        }
        #endregion
    }
}
```

`NSObjectConverter` Se používá, když aplikace potřebuje ke čtení aktuálního stavu vlastnosti. Příklad:

```csharp
var value = NSObjectConverter.ToFloat (characteristic.Value);
```

Výše uvedené řádku převede hodnotu do `float` , lze v kódu Xamarin C#.

Upravit `HMCharacteristic`, volání jeho `WriteValue` metoda a zabalení nové hodnoty v `NSObject.FromObject` volání. Příklad:

```csharp
Characteristic.WriteValue(NSObject.FromObject(value),(err) =>{
    // Was there an error?
    if (err!=null) {
        // Yes, inform user
        AlertView.PresentOKAlert("Update Error",err.LocalizedDescription,Controller);
    }
});
```

Pokud `err` vlastnost není `null`, došlo k chybě a by měla zobrazit uživatelům.

### <a name="testing-characteristic-value-changes"></a>Testování charakteristik hodnotu změny

Při práci s `HMCharacteristics` a simulované příslušenství, změny `Value` vlastnost lze sledovat v simulátoru HomeKit příslušenství.

Pomocí **HomeKitIntro** aplikaci spuštěnou na skutečné iOS hardwaru zařízení, změn vlastnosti hodnoty měla by se zobrazit v simulátoru příslušenství HomeKit téměř okamžitě. Například změna stavu light v aplikaci pro iOS:

[![](homekit-images/test01.png "Změna stavu light v aplikaci pro iOS")](homekit-images/test01.png#lightbox)

Stav světlým v simulátoru příslušenství HomeKit by se měl změnit. Pokud hodnota nezmění, zkontrolujte stav chybové zprávy při zápisu nové hodnoty charakteristik a zajistěte, aby byl příslušenství stále dostupný.

## <a name="advanced-homekit-features"></a>HomeKit pokročilé funkce

Tento článek má zahrnutých základní funkce, které jsou potřebné pro práci s HomeKit příslušenství v aplikaci pro Xamarin.iOS. Existuje však několik pokročilé funkce HomeKit, které nejsou součástí tento úvod:

- **Místnostmi** -HomeKit povoleno příslušenství můžete volitelně uspořádány do místnostmi koncovým uživatelem. To umožňuje HomeKit přítomen příslušenství způsobem, který je snadné k pochopení a pracovat s uživatelem. Další informace o vytváření a správě místnostmi, najdete v článku společnosti Apple [HMRoom](https://developer.apple.com/library/prerelease/ios/documentation/HomeKit/Reference/HMRoom_Class/index.html#//apple_ref/occ/cl/HMRoom) dokumentaci.
- **Zóny** -místnostmi lze případně uspořádat do zóny koncovým uživatelem. Zónu odkazuje na kolekci místnostmi, které uživatel může považovat za jednu jednotku. Příklad: Upstairs, Downstairs nebo sklepení. To umožňuje znovu HomeKit k dispozici a pracovat s příslušenství způsobem, který dává smysl pro koncového uživatele. Další informace o vytváření a správě zón, najdete v článku společnosti Apple [HMZone](https://developer.apple.com/library/prerelease/ios/documentation/HomeKit/Reference/HMZone_Class/index.html#//apple_ref/occ/cl/HMZone) dokumentaci.
- **Akce a akce nastaví** -akce Upravit vlastnosti příslušenství služby a je možné seskupit do sady. Nastaví akce fungují jako skripty, které řídí skupinu příslušenství a koordinaci své činnosti. "Sledovat Televizi" skript může například zavřete rolety, dim světla a zapnout televizi a jeho zvukové systém. Další informace o vytvoření a údržbu akce a akce sady, najdete v článku společnosti Apple [HMAction](https://developer.apple.com/library/prerelease/ios/documentation/HomeKit/Reference/HMAction_Class/index.html#//apple_ref/occ/cl/HMAction) a [HMActionSet](https://developer.apple.com/library/prerelease/ios/documentation/HomeKit/Reference/HMActionSet_Class/index.html#//apple_ref/occ/cl/HMActionSet) dokumentaci.
- **Aktivační události** – aktivační událost může aktivovat jeden nebo více akce nastavena při danou sadu podmínek jsou splněny. Například zapnout portch světlým a uzamčení všechny externí dveře při získá tmavý mimo. Další informace o vytvoření a údržbu aktivačních událostí, najdete v článku společnosti Apple [HMTrigger](https://developer.apple.com/library/prerelease/ios/documentation/HomeKit/Reference/HMTrigger_Class/index.html#//apple_ref/occ/cl/HMTrigger) dokumentaci.

Vzhledem k tomu, že tyto funkce používají stejné techniky popsané výše, by měla být snadno implementovat podle následující Apple [HomeKitDeveloper průvodce](https://developer.apple.com/library/ios/documentation/NetworkingInternet/Conceptual/HomeKitDeveloperGuide/Introduction/Introduction.html), [HomeKit uživatele Interface Guidelines](https://developer.apple.com/homekit/ui-guidelines/) a [ HomeKit Framework – referenční informace](https://developer.apple.com/library/ios/home_kit_framework_ref).

## <a name="homekit-app-review-guidelines"></a>Pokyny pro recenze HomeKit aplikace

Před odesláním HomeKit povolená aplikace Xamarin.iOS pro službu iTunes Connect pro verzi v iTunes App Store, zkontrolujte, postupujte podle pokynů společnosti Apple pro HomeKit povolené aplikace:

 - Primárním účelem aplikace _musí_ být domácí automatizace, pokud používáte rozhraní HomeKit.
 - Text marketing aplikace musí upozorněte uživatele, že se používá HomeKit a musí zadat zásady ochrany osobních údajů.
 - Shromažďování informací o uživateli nebo pro inzerování pomocí HomeKit výhradně zakázané.

Kompletní přečtěte si pokyny, prohlédněte si prosím společnosti Apple [přečtěte si pokyny v App Storu](https://developer.apple.com/app-store/review/guidelines/).

## <a name="whats-new-in-ios-9"></a>Co je nového v iOS 9

Apple má provést následující změny a dodatky HomeKit pro iOS 9:

- **Zachování stávající objekty** – Pokud se změní stávající příslušenství, správce Domů (`HMHomeManager`) bude informovat konkrétní položky, které bylo změněno.
- **Trvalé identifikátory** -nyní zahrnují všechny příslušné třídy HomeKit `UniqueIdentifier` povolena vlastnost k jednoznačné identifikaci daná položka napříč HomeKit aplikace (nebo instancí té samé aplikace).
- **Správa uživatelů** -přidat řadič předdefinovaných zobrazení a zajistit tak správu uživatele přes uživatele, kteří mají přístup k zařízením HomeKit v domovské primárního uživatele.
- **Možnosti uživatele** – HomeKit uživatelé nyní mají sadu oprávnění, které řídí, jaké funkce jsou možné využít v HomeKit a HomeKit povoleno příslušenství. Aplikace by měl zobrazit příslušné možnosti s aktuálním uživatelem. Například pouze správci mají moci udržovat jiných uživatelů.
- **Předdefinované scény** -předdefinované scény byly vytvořeny pro čtyři běžné události, které nastaly průměrná uživatele HomeKit: zprovoznění, ponechte, vrátit, přejděte na DNA. Tyto předdefinované scény nelze odstranit z síť doma.
- **Scény a Siri** -Siri má hlubší podporu pro scény v iOS 9 a může rozpoznat název všechny scény definované v HomeKit. Uživatel může spustit scény jednoduše tak, že hovořícího její název do Siri.
- **Příslušenství kategorie** -sadu předdefinovaných kategorií byl přidán ke všem příslušenství a pomáhá identifikovat typ příslušenství, který se přidává do síť doma nebo pracovali z vaší aplikace. Během instalace příslušenství jsou k dispozici tyto nové kategorie.
- **Podpora Apple Watch** – HomeKit je nyní k dispozici pro watchOS a Apple Watch bude řídit HomeKit povolené zařízení bez zařízení typu iPhone se blíží hodinek. HomeKit pro watchOS podporuje následující funkce: zobrazení domácnostech, řízení příslušenství a provádění scény.
- **Nový typ aktivační události** – kromě časovače typ aktivačních událostí v iOS 8, iOS 9 teď podporuje aktivační události na základě stavu příslušenství (například data snímačů) nebo informace o zeměpisné poloze podporovány. Použít aktivační události `NSPredicates` nastavit podmínky pro jejich provádění.
- **Vzdálený přístup** -s vzdáleného přístupu uživatele je teď možné řídit jejich HomeKit povoleno Domů automatizace příslušenství, když jsou mimo úklidové ve vzdáleném umístění. V iOS 8 to byla podporována pouze pokud má uživatel, a 3. generování Apple TV na domovské stránce. V systému iOS 9 toto omezení se ruší a vzdáleného přístupu je podporovaná prostřednictvím serveru služby iCloud a HomeKit příslušenství Protocol (HAP).
- **Nové možnosti Bluetooth nízká energie (Povolit)** -HomeKit teď podporuje další příslušenství typy, které mohou komunikovat prostřednictvím protokolu Bluetooth nízká energie (Povolit). Pomocí HAP zabezpečené tunelování, příslušenství HomeKit můžou zpřístupnit jiné příslušenství Bluetooth přes Wi-Fi (Pokud je mimo rozsah Bluetooth). V iOS 9 zakázat příslušenství mít plnou podporu pro oznámení a metadata.
- **Nové kategorie Příslušenství** -Apple přidány následující nové kategorie příslušenství v iOS 9: okno krytiny, motoricky dveří a systému Windows, poplašné systémy, senzory a programovatelný přepínače.

Další informace o nových funkcích HomeKit v iOS 9 najdete v tématu společnosti Apple [HomeKit Index](https://developer.apple.com/homekit/) a [co je nového v HomeKit](https://developer.apple.com/videos/wwdc/2015/?id=210) videa.

## <a name="summary"></a>Souhrn

Tento článek obsahuje zavedla framework domácí automatizace HomeKit společnosti Apple. Vám ukázal, jak nainstalovat a nakonfigurovat pomocí simulátoru příslušenství HomeKit testovací zařízení a jak vytvořit jednoduchou aplikaci Xamarin.iOS zjistit, komunikují a řídit zařízení domácí automatizace pomocí HomeKit.



## <a name="related-links"></a>Související odkazy

- [Ukázky iOS 9](https://developer.xamarin.com/samples/ios/iOS9/)
- [iOS 9 pro vývojáře](https://developer.apple.com/ios/pre-release/)
- [Co je nového v iOS 9.0](https://developer.apple.com/library/prerelease/ios/releasenotes/General/WhatsNewIniOS/Articles/iOS9.html)
- [Průvodce HomeKitDeveloper](https://developer.apple.com/library/ios/documentation/NetworkingInternet/Conceptual/HomeKitDeveloperGuide/Introduction/Introduction.html)
- [Pokyny HomeKit uživatelské rozhraní](https://developer.apple.com/homekit/ui-guidelines/)
- [HomeKit Framework – referenční informace](https://developer.apple.com/library/ios/home_kit_framework_ref)
