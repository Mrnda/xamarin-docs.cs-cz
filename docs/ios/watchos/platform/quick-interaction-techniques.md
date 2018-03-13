---
title: "Rychlé interakce techniky pro watchOS 3"
description: "Tento článek se zabývá techniky rychlé interakce Apple přidala v watchOS 3 a jejich implementaci Xamarin.iOS pro Apple Watch."
ms.topic: article
ms.prod: xamarin
ms.assetid: 26697F68-AF7E-4A36-988F-85E2674A4DD1
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/17/2017
ms.openlocfilehash: bf93744914a0caf4f6599fc333ae200468d66e48
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 03/09/2018
---
# <a name="quick-interaction-techniques-for-watchos-3"></a>Rychlé interakce techniky pro watchOS 3

_Tento článek se zabývá techniky rychlé interakce Apple přidala v watchOS 3 a jejich implementaci Xamarin.iOS pro Apple Watch._

Poskytnete rychlé uživatelské interakce jsou nezbytné pro vytváření byly atraktivní aplikace Apple Watch a komplikace. Nové za účelem watchOS 3, Apple přidala podporu pro rozpoznávání gesto, přístup k digitální Crown a nové techniky oznámení pro uživatele a navigace. To, společně s přidanou podporu pro SceneKit a SpriteKit, umožňuje vývojářům snadno vytvářet bohaté, přehledné rozhraní, která se rychle a dobře reagovaly.

## <a name="what-are-quick-interactions"></a>Co jsou rychlé interakce

Pro vývojáře, který slouží k vytváření aplikací pro iOS nebo systému macOS (kde množství času, který uživatel stráví interakci s aplikací se měří v minutách nebo hodinách), navrhování úspěšné aplikace pro Apple Watch, může být složité a vyžaduje jinou přístup.

V watchOS obvykle chce uživatel zvýšit jejich zápěstí, rychle využívat aplikaci (obvykle pro krátké několik sekund), pak vyřadit své zápěstí a pokračovat, ať bylo jejich dělali.

Následuje několik příkladů typické rychlé interakce na Apple Watch:

- Spouštění časovač.
- Kontrola počasí.
- Označení položku vypnout seznamu úkolů.

Chcete-li těchto cílů dosáhnout, musí být aplikace na Apple Watch:

- **Přehledné** – to znamená, že s rychlým přehled uživatel měli být schopní získat informace, které potřebují. 
- **Řešitelné** – to znamená, aby uživatelé měli být schopní rychlý a dobře informované rozhodnutí.
- **Přizpůsobivý** – což znamená, že uživatel nikdy čekat přijímat informace, které potřebují nebo k dosažení akce chtějí mít.

### <a name="quick-interactions-length"></a>Délka rychlé interakce

Z důvodu přehledné povaha aplikace Apple Watch, Apple naznačuje, že ideální délka rychlé interakce by měl dvou sekund nebo méně. V důsledku této dvě druhý limit bude třeba vývojář tráví značné množství času, jak navrhování a implementace Apple Watch aplikace. 

## <a name="new-watchos-3-features-and-apis"></a>Nové watchOS 3 funkce a rozhraní API

Apple přidala několik nových funkcí a rozhraní API k WatchKit vývojář jako pomůcku při přidávání rychlé interakce do své aplikace Apple Watch:

- watchOS 3 poskytuje přístup k nové typy, jako vstup od uživatele:
    - Nástroje pro rozpoznávání gesto
    - Digitální Crown otočení 
- watchOS 3 poskytuje nové způsoby zobrazení a aktualizaci informací, jako například:
    - Rozšířené navigační tabulky
    - Nová podpora framework oznámení pro uživatele
    - Integrace SpriteKit a SceneKit

Implementací tyto nové funkce Vývojář můžete zajistit, že jejich aplikace watchOS 3 Glanceable, Actionable a Responsive.

### <a name="gesture-recognizer-support"></a>Podpora pro rozpoznávání gesto

Pokud vývojář implementovala gesto rozpoznávání v iOS, měly by být velmi dobře fungování nástroje pro rozpoznávání gesto v watchOS 3. Pokud chcete aktualizovat, jsou nástroje pro rozpoznávání gesto objekty, které analyzovat nízké úrovně touch události do gesta rozpoznatelném, předem definovaná.

watchOS 3 bude podporovat čtyři následující gesto nástroje pro rozpoznávání:

- Diskrétní gesta typy:
    - Gesto prstem (`WKSwipeGestureRecognizer`).
    - Klepněte na gesto (`WKTapGestureRecognizer`).
- Průběžné gesto typy:
    - Gesto Pan (`WKPanGestureRecognizer`).
    - Gesto dlouho stiskněte (`WKLongPressGestureRecognizer`).

Chcete-li implementovat jednu nové rozpoznávání gesto, jednoduše přetáhněte ji na návrhovou plochu v iOS Návrhář v sadě Visual Studio pro Mac a nakonfigurujte jeho vlastnosti.

V kódu reagovat na akci pro rozpoznávání pro zpracování gesto se aktivuje uživatelem. Znovu to se provádí stejným způsobem, jako by být zpracována v iOS.

#### <a name="discrete-gesture-states"></a>Diskrétní gesto stavy

Pro diskrétní gesta akce je volána při gesta se rozpozná a stavu (`WKGestureRecognizerState`) je přiřazen jako:

[![](quick-interaction-techniques-images/quick01.png "Diskrétní gesto stavy")](quick-interaction-techniques-images/quick01.png#lightbox)

Všechny diskrétní gesta spustí `Possible` stavu a přejít na buď `Failed` nebo `Recognized` stavu. Při použití diskrétní gesta, vývojáři obvykle není přímo se týkají stavu. Místo toho spoléhají na akci volána po gesta pouze rozpoznání.

#### <a name="continuous-gesture-states"></a>Průběžné gesto stavy

Průběžné gesta se mírně liší od diskrétní gesta, kde akce je volat vícekrát, jako je právě rozpoznat gesta:

[![](quick-interaction-techniques-images/quick02.png "Průběžné gesto stavy")](quick-interaction-techniques-images/quick02.png#lightbox)

Znovu spustí průběžné gesta `Possible` stavu, ale průběhu přes více aktualizací. Zde bude vývojář musíte vzít v úvahu rozpoznávání rukopisu na stav a aktualizovat uživatelském rozhraní aplikace během `Changed` fáze dokud gesta nakonec `Recognized` nebo `Canceled`.

#### <a name="gesture-recognizer-usage-tips"></a>Tipy pro použití funkce rozpoznávání gesto

Při práci s gesto rozpoznávání v watchOS 3, Apple naznačovat následující:

- Nástroje pro rozpoznávání gesto přidáte do skupiny elementy místo jednotlivých ovládacích prvků. Vzhledem k tomu, že Apple Watch má menší velikost fyzické obrazovky, skupiny elementy jsou obvykle větší a jednodušší cíle pro uživatele k volání. Taky nástroje pro rozpoznávání gesto může dojít ke konfliktu s součástí gesta již v nativní ovládací prvky uživatelského rozhraní.
- Nastavit vztahy závislostí v Storyboard aplikace sledovat.
- Některé gesto mají přednost před jiné typy gesto, jako například:
    - Posouvání
    - Vynutit dotykového ovládání
 
### <a name="digital-crown-rotation"></a>Digitální Crown otočení

Implementací digitální podporu Crown ve svých aplikacích watchOS 3 může vývojář poskytnout vyšší navigační interakci rychlost a přesnost pro své uživatele.

Od watchOS 2, může Apple Watch aplikace používat `WKInterfacePicker` objekt, který má přístup k digitální Crown tím, že poskytuje seznam `WKPickerItems` a výběr stylu (seznamu, skládané nebo bitovou kopii pořadí). watchOS povoleno pak aby uživatelé používali digitální Crown vybrat položku ze seznamu.

Při použití `WKInterfacePicker`, WatchKit zpracovává většinu práce podle:

- Kreslení v seznamu a elementy jednotlivých rozhraní.
- Zpracování událostí digitální Crown.
- Volání metody akce, když je vybrána položka.

Nové ke watchOS 3, vývojáři teď má přímý přístup k digitální Crown otočení události, které vám umožní vytvořit své vlastní prvky uživatelského rozhraní, které reagují na otočení hodnoty.

Digitální Crown přístup poskytuje následující prvky:

- `WKCrownSequencer` – Poskytuje přístup k otočení za sekundu.
- `WKCrownDelegate` – Poskytuje přístup k událostem otáčení delta.

#### <a name="rotations-per-second"></a>Otočení za sekundu

Přístup k otočení za sekundu z digitální Crown je užitečná při práci s fyziky na základě animace. Chcete-li získat přístup k otočení za sekundu, použijte `CrownSequencer` vlastnost `WKInterfaceController` sledovat rozšíření. Příklad:

```csharp
var rotationsPerSecond = CrownSequencer.RotationsPerSecond;
```

#### <a name="rotational-deltas"></a>Otáčení rozdílů

Pomocí otáčení rozdílů z digitální Crown počet otočení. Použití `CrownDidRotate` přepsat metodu `WKCrownDelegate` pro přístup k otáčení rozdíly. Příklad:

```csharp
using System;
using WatchKit;
using Foundation;

namespace MonkeyWatch.MonkeySeeExtension
{
    public class CrownDelegate : WKCrownDelegate
    {
        #region Computed Properties
        public double AccumulatedRotations { get; set;}
        #endregion

        #region Constructors
        public CrownDelegate ()
        {
        }
        #endregion

        #region Override Methods
        public override void CrownDidRotate (WKCrownSequencer crownSequencer, double rotationalDelta)
        {
            base.CrownDidRotate (crownSequencer, rotationalDelta);

            // Accumulate rotations
            AccumulatedRotations += rotationalDelta;
        }
        #endregion
    }
}
```

Zde aplikace udržuje jedná (`AccumulatedRotations`) k určení počtu otočení. Jeden úplné otočení digitální Crown rovná Akumulovaná rozdíl `1.0` a rotaci poloviční by `0.5`.

Apple opustil až vývojáře k určení, jak počty otočení odpovídají citlivosti změny na prvek uživatelského rozhraní, Probíhá aktualizace.

Znaménko (`+/-`) otáčení rozdílové Určuje směr, že uživatel je vypnutí digitální Crown:

[![](quick-interaction-techniques-images/quick03.png "Znaménko otáčení Delta Určuje směr, že uživatel je vypnutí digitální Crown")](quick-interaction-techniques-images/quick03.png#lightbox)


Pokud uživatel je posouvání nahoru, WatchKit vrátí kladné rozdílů a pokud posouvání dolů, pak záporné rozdílů, bude vrácen, bez ohledu na to, jaké orientaci uživatele je používání sledovat v.

#### <a name="digital-crown-focus"></a>Digitální Crown fokusu

Stejně jako další prvky rozhraní digitální Crown obsahuje koncepci fokus. Tento fokus může být posunut od digitální Crown další prvky rozhraní založené na tom, jak je uživateli interakci s hodinek. 

Například některé z těchto ovládacích může vám ukrást fokus digitální Crown:

- Výběr.
- Posuvník
- Posouvání řadiče

Je vývojáře k určení, kdy jejich vlastní rozhraní element musí být fokus digitální Crown. Apple navrhuje pomocí nové nástroje pro rozpoznávání gesto získat fokus ve vlastní element uživatelského rozhraní.

### <a name="vertical-paging"></a>Svislé stránkování

Standardním způsobem, že uživatel přejde zobrazení tabulky v aplikaci watchOS je přejděte na požadovanou část dat, klepněte na konkrétní řádek zobrazit v podrobném zobrazení, klepněte na tlačítko zpět po dokončení zobrazení podrobností a opakujte postup pro všechny ostatní informace, které y mají zájem z tabulky:

[![](quick-interaction-techniques-images/quick04.png "Přesouvání mezi tabulky a zobrazení podrobností")](quick-interaction-techniques-images/quick04.png#lightbox)

Nové watchOS 3 Vývojář můžete povolit svislé stránkování na jejich ovládací prvky zobrazení tabulky. Tato funkce povolena může uživatel posuňte najde řádek tabulky zobrazení a klepněte na řádek zobrazíte její podrobnosti jako před. Však budou můžete nyní prstem si vyberte na další řádek v tabulce nebo dolů vyberte na předchozí řádek (nebo použijte digitální Crown), všechny bez nutnosti vrátit zobrazení tabulky nejprve:

[![](quick-interaction-techniques-images/quick05.png "Přesouvání mezi tabulky a zobrazení podrobností a k načtení nahoru a dolů přecházet mezi další řádky")](quick-interaction-techniques-images/quick05.png#lightbox)

Tento režim povolit, otevřete aplikaci watchOS Storyboard v Xcode pro úpravy, zvolte zobrazení tabulky a zkontrolujte **svislé stránkování podrobností** políčko:

[![](quick-interaction-techniques-images/quick06.png "Zaškrtnutím políčka svislé stránkování podrobností")](quick-interaction-techniques-images/quick06.png#lightbox)

Zajistěte, že v tabulce používá Segues pro zobrazení v podrobném zobrazení a uložit změny do scénáře a návrat na Visual Studio pro Mac k synchronizaci.

Vývojář může prostřednictvím kódu programu zapojení svislé stránkování konkrétního řádku pomocí následujícího kódu proti zobrazení tabulky:

```csharp
// Segue into Vertical Paging and select the first row
MenuTable.PerformSegue (0);
```

Při použití svislé stránkování, musí vývojář Uvědomte si, že WatchKit automaticky zpracuje předběžné zavádění řadičů a některé metody životního cyklu řadiče v důsledku toho může být volána před uživatelské rozhraní je ve skutečnosti viditelná.

### <a name="notification-enhancements"></a>Vylepšení oznámení

Oznámení jsou primární formu rychlé interakce, které uživatel obvykle dojde na watchOS a byly k dispozici, a to od první Apple Watch a watchOS 1.

Typické rychlé interakce oznámení vypadá takto:

1. Uživatel pracuje hmatová oznámení, když je obdržena nové oznámení.
2. Vyvolají jejich zápěstí zobrazíte rozhraní krátké vypadat pro oznámení.
3. Pokud se přitom zachovat jejich zápěstí vyvolána, watchOS automaticky přejde do rozhraní dlouho vypadat oznámení.

Existuje několik způsobů, které uživatel může reagovat na upozornění:

- Pro také definovaný a zobrazí oznámení bude uživatel Neprovádět žádnou akci a jednoduše zavření oznámení.
- Může se také klepněte oznámení. Chcete-li spustit aplikaci watchOS.
- Pro oznámení, která podporuje vlastní akce může uživatel vyberte jednu z vlastní akce. To může být buď:
    - **Akce popředí** -tyto spustit aplikaci k provedení akce.
    - **Akce na pozadí** – byly vždy směrovány do iPhone v watchOS 2, ale lze ho směrovat na watchApp v watchOS 3.

Novinky z watchOS 3:

* Oznámení pomocí podobné rozhraní API pro všechny platformy (iOS, watchOS, tvOS a systému macOS).
* Místního oznámení můžete naplánovat na Apple Watch.
* Pozadí oznámení budou směrovány do rozšíření pro aplikace, pokud bylo naplánováno na Apple Watch.

#### <a name="notification-scheduling-and-delivery"></a>Plánování oznámení a doručení

Oznámení z iPhone uživatele bude dál Apple Watch když dojde k následujícímu:

* Pro iPhone obrazovky je vypnuté.
* Apple Watch je právě použité a byl odemčen.

Místní oznámení v watchOS 3, můžete naplánovat na Apple Watch a jsou pouze na hodinek doručovat. Je vývojáři naplánovat odpovídající iPhone oznámení, pokud je vyžadována aplikace.

Zahrnutím stejný identifikátor oznámení na Apple Watch i iPhone verzích oznámení zabrání duplicitní oznámení z se zobrazí na sledovat. Verze Apple Watch oznámení se mají přednost před verze iPhone.

Vzhledem k tomu, že watchOS 3 používá stejnou `UINotification` rozhraní API framework jako iOS 10, najdete v tématu naše iOS 10 [Framework oznámení uživatele](~/ios/platform/user-notifications/index.md) dokumentace pro další podrobnosti.

### <a name="using-spritekit-and-scenekit"></a>Pomocí SpriteKit a SceneKit

Nové k watchOS 3, vývojář teď můžete použít objekty SpritKit a SceneKit v návrhu uživatelské rozhraní jejich aplikace k dispozici 2D i 3D grafický.

Dva nové třídy rozhraní Přibyla na podporu této funkce:

- `WKInterfaceSKScene` -Pro práci s SpriteKit 2D grafiky.
- `WKInterfaceSCNScene` -Pro práci s 3D grafický SceneKit.

Použití těchto objektů, přetáhněte je na návrhovou plochu uvnitř Storyboard aplikace sledovat v Tvůrci rozhraní na Xcode a použít **atributy Inspector** jejich konfigurace.

Z tohoto bodu práci s buď SpriteKit nebo SceneKit scény pracuje, stejná jako v aplikaci pro iOS. Sledování aplikace bude k dispozici `WKInterfaceSKScene` mezi voláním `Present` metody. Pro SceneKit, jednoduše nastavit `Scene` vlastnost `WKInterfaceSCNScene` objektu.

## <a name="actionable-complications"></a>Řešitelné komplikace

V watchOS 2 Společnost Apple vydala komplikace pro aplikace, 3. stran. V watchOS 3 se rozšířila Apple schopnosti, které vývojář můžete zahrnout WatchKit komplikace. 

Kromě toho více předdefinovaných v sledovat řezy můžete vkládat komplikace a existující sledovat otočená, že již podporované komplikace můžete nyní zahrnutá i další komplikace.

Nové je také možnost pro uživatele k rychle prstem doleva nebo doprava přechodu prochází všechna sledování ploch, které jste nainstalovali na jejich Apple Watch. Pomocí nové galerie na Apple Watch doprovodné iPhone aplikace, může uživatel přidání a přizpůsobení nové řezy sledovat a jakékoli komplikací, ke kterým můžou mezi ně patřit.

Z důvodu těchto nových funkcích Apple naznačuje, že každou aplikaci v Apple Watch také obsahovat alespoň jeden komplikace a jako takový všechny nativní Apple Watch aplikace teď mají komplikací, ke.

Komplikace poskytují následující funkce, které chcete aplikaci:

- Jsou vysoce přehledné vzhledem k tomu, že vždy se nacházejí na tučné sledovat.
- Komplikace často aktualizaci watchOS. Jakékoli aplikaci, která zahrnuje komplikace na vzhled aktuálně zobrazený sledovat uživatele se aktualizuje alespoň dvakrát jednu hodinu.
- Všechny aplikace s komplikace na vzhled aktuálně zobrazený sledovat uživatele je udržována v paměti, která umožňuje rychle spusťte aplikaci a zvyšuje rychlost odpovědí z aplikace.
- Komplikace usnadňují uživatelům spustit specifické funkce v aplikaci watchOS.

## <a name="glanceable-notification"></a>Přehledné oznámení

Oznámení na Apple Watch poskytují skvělý, přizpůsobitelné způsob, jak rychle informovat uživatele o události nebo nové informace, jako jsou příchozí zprávy nebo dosažení cíle v aplikaci cvičení.

Pomocí oznámení lze rychle zobrazit cenné informace pro uživatele. V mnoha situacích můžete odebrat dobře navrženou oznámení nezbytné pro uživatele ve skutečnosti spusťte aplikaci.

Nový watchOS 3, všechny teď podporují oznámení:

- SpriteKit
- SceneKit
- Vložené Video

## <a name="enhanced-ui-with-spritekit-and-scenekit"></a>Vylepšené uživatelské rozhraní SpriteKit a SceneKit

Obvykle může vývojář myslíte herní uživatelského rozhraní při SpriteKit a SceneKit se zmiňují. Však SpriteKit a SceneKit může být užitečné pro vytváření jiný herní uživatelská rozhraní, které zahrnují vlastní rozložení obsahu a animace, které jinak nejsou v WatchKit samostatně.

Oznámení pro uživatele z fotografie aplikace pro sdílení můžete například použít SpriteKit nabízí bohaté uživatelské prostředí včetně uživatele, který odeslány na obrázku spolu s skutečný obrázek a další vlastní informace, které vylepšuje uživatelské prostředí.

Kromě toho můžete SpriteKit a SceneKit kombinované s standardní prvky uživatelského rozhraní WatchKit návrhu uživatelské rozhraní aplikace.

## <a name="simple-navigation"></a>Jednoduché navigace

watchOS 3 uvede několik způsobů, že vývojář může zjednodušit navigace v rámci jejich watchOS aplikací, jako je například nový [svislé stránkování](#Vertical-Paging), [podporu pro rozpoznávání gesto](#Gesture-Recognizer-Support) a [digitální Crown Otočení](#Digital-Crown-Rotation) funkce uvedené výše.

Digitální Crown je jedinečná Apple Watch a mohou být používány mnoha různými způsoby zjednodušení navigace. Například aplikace časovače vám pomůže digitální Crown přesouváním prostřednictvím dostupné časovače délky.

Vlastních gest s sebou může nést nových a jedinečných způsoby pro uživatele pro interakci s sledování aplikace a také umožňuje zjednodušit navigační aplikace.

Apple naznačují, hledají způsoby, jak sloučit všechny nové funkce Rychlé interakce, které jsou přidané do watchOS 3 a bohaté, snadno a rychle použít rozhraní watchOS aplikace k dispozici.

## <a name="finishing-the-quick-interaction"></a>Dokončením rychlé interakce

Dobře navrženým rychlé interakce prostředí bude uživateli přidělit vyřadit jejich zápěstí (a musí se vypnout s aplikací) se při dokončení aktuální interakce.

Kde to konkrétně stane je problém při sledování aplikace je to jakýkoli typ síťového připojení nebo sdílení informací s jeho doprovodné iPhone aplikací. To může často vést k čekání indikátor během transakce probíhající, která není žádoucí během rychlé interakce. Proveďte v následujícím příkladu:

[![](quick-interaction-techniques-images/quick07.png "Diagram aplikace sledovat provádění síťové připojení a sdílení informací s jeho doprovodné iPhone aplikací")](quick-interaction-techniques-images/quick07.png#lightbox)

1. Uživatel vybere položku, kterou chcete zakoupit na hodinek.
2. Jejich klepněte na tlačítko Koupit.
3. Aplikace spustí transakce sítě a zobrazí indikátor načítání.
4. Některé později dokončení transakce a aplikace zobrazí souhlas nákupu.
5. Zahodí jejich zápěstí a disengages v aplikaci uživatele.

Od doby, uživatel klepnutím na tlačítko Koupit až do dokončení transakce mají jejich zápěstí vyvolá prohlížení načítání indikátoru. Tuto situaci vyřešíte Apple naznačuje, že vývojář musí poskytnout okamžitou zpětnou vazbu uživateli místo zobrazuje indikátor načítání. 

Pomocí modelu navrhované společnosti Apple, podívejte se na stejné rychlé interakce znovu:

[![](quick-interaction-techniques-images/quick08.png "Diagram jablka navrhované modelu")](quick-interaction-techniques-images/quick08.png#lightbox)

1. Uživatel vybere položku, kterou chcete zakoupit na hodinek.
2. Jejich klepněte na tlačítko Koupit.
3. Aplikace spustí transakce sítě a zobrazí se zpráva, že nákupu byla úspěšně spuštěna.
4. Zahodí jejich zápěstí a disengages v aplikaci uživatele.
5. Pokud transakce úspěšně dokončí některé později, aplikace zobrazí místního oznámení informovat uživatele o úspěšném nákupu.

Tento čas, jakmile uživatel klepnutím na koupit tlačítko, které zobrazí se zpráva, aby zahájil nákupu, tak můžou s jistotou vyřadit jejich zápěstí a ukončení rychlé interakci v tomto okamžiku. Později je informován úspěch nebo selhání transakce v oznámení pro uživatele. Tímto způsobem uživatele pouze komunikuje s aplikací během "aktivní" fáze procesu.

Pro aplikace, které dělají sítě, mohou použít pozadí `NSURLSession` pro zpracování komunikaci sítě s stahování. To vám umožní aplikaci probouzet na pozadí zpracovat stažené informace. Pro aplikace, které vyžadují zpracování na pozadí použijte ke zpracování požadované zpracování Assertion úloh na pozadí.

## <a name="quick-interaction-design-tips"></a>Tipy k návrhu rychlé interakce

Vzhledem k tomu, že požadovaná délka rychlé interakce je dvou sekund nebo méně, musí vývojář soustředit na návrh interakce aplikace od samého začátku procesu návrhu. Najít oblastech, kde tyto interakce můžete zjednodušit (pomocí techniky popsané výše) a využívat nové funkce nástroje watchOS 3 k vytvoření aplikace rychlý a dobře reagovaly.

Apple navrhuje následující:

- Tak, že převedou nejčastěji používaných funkcí aplikace dál se zaměřit na rychlý interakce.
- Použijte komplikace a oznámení uživateli prezentovat běžné funkce a funkce.
- Vytvořte bohatou a přehledné uživatelské rozhraní s SceneKit a SpriteKit.
- Pokud je to možné, Zjednodušte navigace v rámci aplikace.
- Nikdy nastavení uživatele, počkejte, než, mohly zrušíte jejich zápěstí a musí se vypnout s aplikací, co nejdříve.

## <a name="summary"></a>Souhrn

Tento článek má zahrnutých techniky rychlé interakce Apple přidala v watchOS 3 a jejich implementaci Xamarin.iOS pro Apple Watch.

## <a name="related-links"></a>Související odkazy

- [Ukázky pro watchOS](https://developer.xamarin.com/samples/watchos/all/)
