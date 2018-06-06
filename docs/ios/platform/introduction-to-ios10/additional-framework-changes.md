---
title: Změny architektury iOS dalších 10
description: Tento dokument popisuje menší změny a vylepšení probíhají do existujících architektur v iOS 10 a popisuje, jak chcete-li použít tyto aktualizace v Xamarin.iOS.
ms.prod: xamarin
ms.assetid: 0E2217F1-FC96-4D0A-ABAB-D40AD8F96502
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/29/2017
ms.openlocfilehash: 4b9a230157593b66446e2949e57a925d94208752
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 06/05/2018
ms.locfileid: "34787556"
---
# <a name="additional-ios-10-frameworks-changes"></a>Změny architektury iOS dalších 10

_Tento článek se zabývá další, méně závažné změny nebo vylepšení stávajících rozhraní pro iOS 10._

## <a name="av-foundation-framework-additions"></a>Přidání Framework AV Foundation

Rozhraní framework AVFoundation obsahuje následující vylepšení:

- V iOS 10 Vývojář již nemá k implementaci různých [AVPlayerItem](https://developer.xamarin.com/api/type/AVFoundation.AVPlayerItem/) chování v závislosti na typu obsahu. Jednoduše nastavit `Rate` vlastnost a AVFoundation určí, kdy dostatek obsahu k dispozici pro přehrávání bez zablokování.
- Nové [AVCapturePhotoOutput](https://developer.xamarin.com/api/type/AVFoundation.AVCaptureFileOutput/) nahradí třída zastaralá `AVCaptureStillImageOutput` třídy a poskytuje jednotnou metodu pro zpracování všech pracovních postupů fotografie poskytuje sofistikované řízení a monitorování procesu zachycení a podporu pro nové funkce, jako jsou fotografie za provozu a formát RAW zachycení.
- Nové `AVPlayerLooper` třída usnadňuje cykly určitou část média při přehrávání.
- `AVAssetDownloadURLSession` Třída umožňuje stažení a novější přehrávání FairPlay šifrované datové proudy HLS.
- Ve výchozím nastavení [AVCaptureSession](https://developer.xamarin.com/api/type/AVFoundation.AVCaptureSession/) třída automaticky podporuje celou color, rozsah celou zachycení při hardwaru zařízení podporuje. Najdete v článku společnosti Apple [iOS odkaz kompatibility zařízení](https://developer.apple.com/library/prerelease/content/documentation/DeviceInformation/Reference/iOSDeviceCompatibility/Introduction/Introduction.html#//apple_ref/doc/uid/TP40013599) další podrobnosti.

## <a name="avkit-additions"></a>Přidání AVKit

Rozhraní framework AVKit nyní zahrnuje nové `UpdatesNowPlayingInfoCenter` vlastnost k označení, pokud by měly být aktualizovány informace o Centru nyní přehrávání.

## <a name="core-data-enhancements"></a>Vylepšení dat jádra

iOS 10 obsahuje následující vylepšení Framework Core dat:

- [NSManagedObjectContext](https://developer.xamarin.com/api/type/CoreData.NSManagedObjectContext/) objekty s úložišti dat SQLite Podpora režimu deníku WAL nové generace dotazu funkci, kde můžete spravovat kontexty objektu (MOC) připnut k verze konkrétní databáze pro budoucí načítání a Chybující transakce.
- Kořenové [NSManagedObjectContext](https://developer.xamarin.com/api/type/CoreData.NSManagedObjectContext/) objekty podporuje souběžné chybující a načítání bez serializace.
- [NSPersistentStoreCoordinator](https://developer.xamarin.com/api/type/CoreData.NSPersistentStoreCoordinator/) třída udržuje fond úložišť dat SQLite.
- Několik vhodných metod jsou přidané do `NSManagedObject` usnadnit k provedení načtení a vytvoření podtřídy.
- Pomocí vysoké úrovni `NSPersistenceContainer` k odkazu `NSPersistentStoreCoordinator`, [NSManagedObjectModel](https://developer.xamarin.com/api/type/CoreData.NSManagedObjectModel/) a dalším prostředkům konfigurace základní Data.

Další informace najdete v tématu společnosti Apple [základní Data Framework – referenční informace](https://developer.apple.com/reference/coredata).

## <a name="core-image-enhancements"></a>Vylepšení základní bitové kopie

iOS 10 díky těmto vylepšením pro základní bitovou kopii prostředí:

- Vývojář může nyní zpracovat obrázků v barevný prostor mimo kontext základní Image pracovní barevný prostor převedením směřující barevný prostor před a po zpracování.
- Pro zařízení s iOS, které používají A8 a A9 procesorů se teď podporuje formát RAW obrázku. Základní Image teď poskytuje podporu pro dekódování NEZPRACOVANÁ Image buď z předdefinovaných iSight fotoaparátu nebo z fotoaparátu 3. stran. Použití `FilterWithImageData` nebo `FilterWithImageURL` metody [CIFilter](https://developer.xamarin.com/api/type/CoreImage.CIFilter/) třídy ke zpracování nezpracovaných bitové kopie.
- Jsme provedli několik vylepšení výkonu vykreslování `UIImage` vykreslování (Pokud je zajištěna podpora úložiště bitové kopie základní Image) ve `UIImageView` objekty. 
- `UIImage` objekty s příznakem celou rozsah vykreslí jako celou rozsah, barvy v `UIImageView` objekty na zařízeních s iOS, které podporují celou barev.
- Základní Image jádra kódu můžete nyní vyžádá formáty výstupu konkrétní pixelů.
- `ImageWithExtent` Metodu [CIFilter](https://developer.xamarin.com/api/type/CoreImage.CIFilter/) třída slouží k vložení vlastní zpracování do operace filtru. Základní Image bude vyvolání daného zpětného volání mezi filtry při zpracování obrazu pro výstup nebo zobrazení.

Kromě toho byly přidány následující nové filtry základní Image:

- `CINinePartTiled`
- `CINinePartStretched`
- `CIHueSaturationValueGradient`
- `CIEdgePreserveUpsampleFilter`
- `CIClamp`

## <a name="core-motion-additions"></a>Přidání pohybu jádra

Nové iOS 10 framework Core pohybu zahrnuje pedometer události, které chcete povolit aplikaci přijímat rychlé a v reálném čase oznámení uživatele, pozastavení a obnovení sledování během spuštění. Použití [CMPedometer](https://developer.xamarin.com/api/type/CoreMotion.CMPedometer/) k registraci pro události pedometer popředí nebo na pozadí.

## <a name="foundation-enhancements"></a>Vylepšení Foundation

Architektura Foundation pro iOS 10 byly provedeny následující vylepšení:

- Použít novou [NSMeasurementFormatter](https://developer.apple.com/reference/foundation/nsmeasurementformatter) třída k formátování lokalizované měření pro zobrazení pro koncového uživatele.
- Použít novou [NSDateInterval](https://developer.apple.com/reference/foundation/nsdateinterval) třídy pro vytvoření interval výpočtů datum a čas, jako je například doby trvání, při porovnání intervalech a testování pro interval průniků.
- Použít novou [NSMeasurement](https://developer.apple.com/reference/foundation/nsmeasurement) třída pro převod mezi různé jednotky z měr (Měrná jednotka) nebo výpočet hodnot v různých UOMs.

- Použít novou [NSUnit](https://developer.apple.com/reference/foundation/nsunit) a [NSDimension](https://developer.apple.com/reference/foundation/nsdimension) třídy pro představující konkrétní UOMs.
- Několik nových vlastností se přidala [NSLocal](https://developer.apple.com/reference/foundation/nslocale) třída získat místní informace a formáty dostupná zobrazení.

## <a name="gamekit-enhancements"></a>Vylepšení GameKit

Rozhraní GameKit v iOS 10 byly provedeny následující vylepšení:

- **Aplikace herní Centrum** je zastaralý a odebrat z iOS. Pokud aplikace používá GameKit, ho _musí_ k dispozici vlastní rozhraní pro zobrazení GameKit funkcí, jako jsou tabulky, atd. 
- Nový typ účtu jen Icloudu byla implementované [GKCloudPlayer](https://developer.apple.com/reference/gamekit/gkcloudplayer) třídy.
- Nové [GKGameSession](https://developer.apple.com/reference/gamekit/gkgamesession) třída poskytuje zobecněný řešení pro správu úložiště dat v herním centru. `GKGameSession` udržuje seznam přehrávače a aplikace je zodpovědná za implementaci jak a kdy je účastnické data uložená, načíst nebo vyměňují mezi přehrávače. V mnoha případech herní relací můžete nahradit existující shody na základě zapnout, v reálném čase odpovídá nebo trvalé herní uložit metody.

## <a name="gameplaykit-enhancements"></a>Vylepšení GameplayKit

Rozhraní GameplayKit v iOS 10 byly provedeny následující vylepšení:

- Použít novou [GKMeshGraph](https://developer.apple.com/reference/gameplaykit/gkmeshgraph) třída k poskytování vysoce výkonné, vyhledávání přirozený cesty.
- Generování procedurální šumu přidala a slouží k vylepšení realism v přirozeném vyhledávání textury, přidat realism do pohyby kamery a pomáhají generovat bohaté herní světů.
- Rozdělení dat herní world pro efektivní vyhledávání pomocí prostorových dělení.
- Nové Monte Carlo Strategický plánovač ([GKMonteCarloStrategist](https://developer.apple.com/reference/gameplaykit/gkmontecarlostrategist)) se přidal pro výpočty vyčerpávající možné přesunout.
- Byla přidána podpora 3D existujícího agenta a chování cesta hledání pomocí nové [GKAgent3D](https://developer.apple.com/reference/gameplaykit/gkagent3d) a [GKGraphNode3D](https://developer.apple.com/reference/gameplaykit/gkgraphnode3d) třídy.
- Nové [GKScene](https://developer.apple.com/reference/gameplaykit/gkscene) a [GKSKNodeComponent](https://developer.apple.com/reference/gameplaykit/gksknodecomponent) zkontrolujte třídy kombinace GameplayKit a SpriteKit jednodušší než kdy dřív.
- Byla přidána nové rozhraní API rozhodovacího stromu ([GKDecisionTree](https://developer.apple.com/reference/gameplaykit/gkdecisiontree) a [GKDecisionNode](https://developer.apple.com/reference/gameplaykit/gkdecisionnode)) k vylepšení AI herní sestavení.

## <a name="healthkit-enhancements"></a>Vylepšení HealthKit

Rozhraní HealthKit v iOS 10 byly provedeny následující vylepšení:

- Byly přidány nové klíče metadata pro počasí typy (například `HKWeatherConditionClear` a `HKWeatherConditionCloudy`) a cvičení typy (například `HKWorkoutActivityTypeFlexibility` a `HKWorkoutActivityTypeWheelchairRunPace`) byly přidány.
- Nové `HKCDADocument` třída byla přidána k reprezentaci architektura pro klinické dokumentu (CDA) formátu dokumentu.
- Použít novou [HKWorkoutConfiguration](https://developer.apple.com/reference/healthkit/hkworkoutconfiguration) třídu k určení `ActivityType` a `LocationType` z cvičení.
- Nové [HKWheelchairUseObject](https://developer.apple.com/reference/healthkit/hkwheelchairuseobject) a `WheelchairUse` metodu [HKHealthStore](https://developer.apple.com/reference/healthkit/hkhealthstore) třídy byly přidány pro práci s invalidního související data o stavu.

## <a name="homekit-enhancements"></a>Vylepšení HomeKit

Rozhraní HomeKit v iOS 10 byly provedeny následující vylepšení:

- Byly přidány nové služby a vlastnosti.
- IPad se dá nakonfigurovat tak, aby fungoval jako rozbočovač HomeKit poskytnout příslušenství vzdálený přístup, spuštění aktivační události automatizace a povolení sdíleného oprávnění uživatele.
- Byla přidána podpora pro příslušenství fotoaparát a zvonku.
- Pro příslušenství byly zadány další kontext a konfigurace.

Najdete v tématu naše [Úvod do HomeKit](~/ios/platform/homekit.md) Další informace naleznete v dokumentaci.

## <a name="metal-enhancements"></a>Vylepšení systému

Rozhraní systému v iOS 10 byly provedeny následující vylepšení:

- 3D aplikace a hry teď můžete použít _teselace_ efektivně vykreslit komplexní scény a geometry prostřednictvím GPU.
- Poskytuje jemně odstupňovanou kontrolu přidělení prostředků za účelem optimalizace výkonu operačního systému na základě aplikací pomocí prostředků haldách a Memoryless vykreslení cíle.
- Pomocí funkce specializace můžete vytvořit kolekci vysoce optimalizovaný materiálu a světla kombinace kláves pro scény.

Další informace najdete v tématu společnosti Apple [Průvodce programováním v systému](https://developer.apple.com/library/prerelease/content/documentation/Miscellaneous/Conceptual/MetalProgrammingGuide/Introduction/Introduction.html#//apple_ref/doc/uid/TP40014221).

## <a name="modelio-enhancements"></a>Vylepšení ModelIO

Rozhraní ModelIO v iOS 10 byly provedeny následující vylepšení:

 - Formát souboru USD se teď podporuje.
 - Vzdálenost pole byla přidána podpora pro podepsané [MDLVoxelArray](https://developer.apple.com/reference/modelio/mdlvoxelarray) třídy.
 - Použít novou `MDLLightProbeIrradianceDataSource` třída jako pomůcku při sběru dat Light umístění.
 - Použít novou `MDLMaterialPropertyGraph` třída snadná podpora runtime změny modelů.

## <a name="photos-enhancements"></a>Vylepšení fotografie

Rozhraní framework fotografie v iOS 10 byly provedeny následující vylepšení:

- Použití [CIImageProcessorInput](https://developer.apple.com/reference/coreimage/ciimageprocessorinput) a [CIImageProcessorOutput](https://developer.apple.com/reference/coreimage/ciimageprocessoroutput) třídy, abyste mohli využívat nové funkce procesoru základní Image k provádění úprav.
- Úpravy fotografií za provozu je nyní k dispozici pro aplikace, které podporují rozhraní fotografie a úpravu rozšíření fotografií (pro použití v rámci fotky a fotoaparát aplikace).
- Použít novou [PHLivePhotoEditingContext](https://developer.apple.com/reference/photos/phlivephotoeditingcontext) třída pro úpravy použity video i přesto obsah fotografie za provozu.

## <a name="replaykit-enhancements"></a>Vylepšení ReplayKit

Rozhraní ReplayKit v iOS 10 byly provedeny následující vylepšení:

- Použití [RPScreenRecorder](https://developer.apple.com/reference/replaykit/rpscreenrecorder), [RPBroadcastActivityViewController](https://developer.apple.com/reference/replaykit/rpbroadcastactivityviewcontroller) a [RPBroadcastController](https://developer.apple.com/reference/replaykit/rpbroadcastcontroller) třídy pro podporu všesměrového vysílání z zaznamenávají médií prostřednictvím 3. stran weby.
- Rozšíření uživatelského rozhraní všesměrové vysílání a vysílání nahrát jsou potřebné k podpoře služby všesměrového vysílání ReplayKit 3. stran v aplikaci.

## <a name="scenekit-enhancements"></a>Vylepšení SceneKit

Rozhraní SceneKit v iOS 10 byly provedeny následující vylepšení:

- [SCNCamera](https://developer.xamarin.com/api/type/SceneKit.SCNCamera/) třídy můžete poskytnout větší realism pomocí funkce záhlaví a efekty. Adaptivní ohrožení použijte k vytvoření automatického účinky nebo použití vinětování, barevné lemování a barvu třídění přidat fillmatic účinky na příslušnou hru.
- SceneKit nyní zahrnuje nový systém fyzicky na základě vykreslování (PBR) realističtější výsledků s jednodušší vytváření asset.
- Použít novou [SCNLightingModelPhysicallyBased](https://developer.apple.com/reference/scenekit/scnlightingmodelphysicallybased) stínování modelu produktu širokou škálu efekty stínování realistické současně vyžaduje jenom tři základní vlastnosti (`Diffuse`, `Metalness` a `Roughness`).
- Od PBR stínování funguje nejlépe s osvětlení založené na prostředí, použijte `LightingEnvironment` vlastnosti pro přiřazení na základě bitové kopie osvětlení celý scény.
- Použití `IESProfileURL` vlastnost import reálného světla zařízení, které definují osvětlení podle skutečné hodnoty například intenzitou (v lumenů) a teploty barvu (ve stupních Kelvin).
- Funkce fotoaparátu PBR a záhlaví poskytují lepší výsledky než tradiční vykreslování technik a v důsledku toho SceneKit nyní provádí všech výpočtů barev v prostoru lineární barev (pomocí P3 barevný rozsah na celou color zařízení zobrazí).
- Barva nyní SceneKit odpovídá všechny barvy čtení informací o profilu barev.
- SceneKit interpretuje – hodnoty barev součásti v lineární prostoru barva RGB pro všechny typy shaderu.
- Obě lineární barvu vykreslování místa a celou color lze zakázat zadáním `SCNDisableLinearSpaceRendering` a `SCNDisableWideGamut` klíče v dané aplikaci `Info.plist`.
- Sestavení primátů libovolný mnohoúhelníku, (buď načíst ze souborů nebo vygenerovat prostřednictvím kódu programu) a zadejte geometrie s novou [SCNGeometryPrimitiveTypePolygon](https://developer.apple.com/reference/scenekit/1772322-scenekit_enumerations/scngeometryprimitivetype/scngeometryprimitivetypepolygon) třídy.
- Vzhledem k tomu, že SceneKit čte a upravit informace o profilu barev v texture Image, ujistěte se, že tyto informace jsou poskytovány pomocí Asset katalogy pro všechny Image.

## <a name="spritekit-enhancements"></a>Vylepšení SpriteKit

Rozhraní SpriteKit v iOS 10 byly provedeny následující vylepšení:

- Vlastní shadery můžete zadat atributy (`SKAttribute`), můžete nakonfigurovat samostatně pomocí každý uzel, který používá shaderu zadáním hodnotu atributu (`SKAttributeValue`).
- Tilemaps teď podporují tvarů odmocnina, šestiúhelníkový a Izometrické dlaždice pro vytváření 2D, 2,5 D a posouvání straně hry pomocí `SKTileMapMode`, `SKTileGroup`, `SKTileGroupRule` a `SKTileSet` třídy.
- Použít novou `SKWarpGeometry` třídu k roztažení nebo zkreslit [SKSpriteNode](https://developer.xamarin.com/api/type/SpriteKit.SKSpriteNode/) nebo [SKEffectNode](https://developer.xamarin.com/api/type/SpriteKit.SKEffectNode/) vykreslování. Nové [SKAction](https://developer.xamarin.com/api/type/SpriteKit.SKAction/) třídu lze použít pro animaci přechodů mezi osnově účinky.
- [SKView](https://developer.xamarin.com/api/type/SpriteKit.SKView/) třída poskytuje několik nových metod umožnit jemně odstupňovanou kontrolu nad kdy a jak je vykreslen scény.

## <a name="scrollview-enhancements"></a>Vylepšení ScrollView

Do ovládacího prvku ScrollView v iOS 10.3 byly provedeny následující vylepšení:

- `UIScrollView` nyní zahrnují `IndexDisplayMode` vlastnost řídit, jak se zobrazuje index, zatímco uživatel je posouvání jako `UIScrollViewIndexDisplayMode` systému:
    - `Automatic` -Zobrazení indexu je řízena operačního systému.
    - `AlwaysHidden` -Zobrazení indexu je vždy skrytá.

Najdete v článku [iOSTenThree ukázka](https://developer.xamarin.com/samples/monotouch/iOS10/iOSTenThree) pro použití.

## <a name="uikit-enhancements"></a>Vylepšení UIKit

Rozhraní UIKit v iOS 10 byly provedeny následující vylepšení:

- Nové [UIPasteboard](https://developer.xamarin.com/api/type/UIKit.UIPasteboard/) rozhraní API poskytuje nové možnosti (třeba omezení životnosti) a bude automaticky deklarovat kompatibilní typy obsahu pro běžné typy tříd.
- Nová podpora plně interaktivní, na základě objektů, přerušitelné animace byla přidána a může být propojený gesta. Stránku najdete Apple na [referenci na protokol UIViewAnimating](https://developer.apple.com/reference/uikit/uiviewanimating), [referenci třídy UIViewPropertyAnimator](https://developer.apple.com/reference/uikit/uiviewpropertyanimator), [referenci na protokol UITimingCurveProvider](https://developer.apple.com/reference/uikit/uitimingcurveprovider), [Referenci třídy UICubicTimingParameters](https://developer.apple.com/reference/uikit/uicubictimingparameters) a [referenci třídy UISpringTimingParameter](https://developer.apple.com/reference/uikit/uispringtimingparameters) Další informace.
- Nové `UIPreviewInteraction` a `UIPreviewInteractionDelegate` mohla vývojář aplikace nabízí vlastní rozhraní pro operace funkce Náhled a pop.
- Nové `UIAccessibilityCustomRotor` třída umožňuje aplikaci poskytují vlastní, konkrétní funkce pro usnadnění technologie, například hlasových přes.
- Použití `UIAccessibilityIsAssistiveTouchRunning` a `UIAccessibilityAssistiveTouchStatusDidChangeNotification` symboly, které chcete zjistit, zda je povoleno AssistiveTouch.
- Použití `UIAccessibilityHearingDevicePairedEar` a `UIAccessibilityHearingDevicePairedEarDidChangeNotification` symboly a získat stav každého spárovat pomůcky sluchu MFI s připojením.
- Pro podporu dynamický typ v popisky, použít novou textových polí a polí `PreferredFontForTextStyle` metodu `UIFont` třídy.
- Rozhodnout, pokud element by měl aktualizovat jeho písmo při zařízení `UIContentSizeCategory` změny, použijte `AdjustsFontForContentSizeCategory` vlastnost `UIContentSizeCategoryAdjusting` delegovat.
- `OpenURL` Metodu `UIApplication` třídy se nazývá asynchronně a teď podporuje doplňování obslužná rutina, která je volána po dokončení akce Otevřít.
- Spustit CloudKit sdílení a upravit její vlastnosti pomocí nové `UICloudSharingController` a `UICloudSharingControllerDelegate` třídy.
- Využít výhod prefetched buněk k vylepšení možností posouvání `UICollectionViews` s novým `UICollectionViewDataSourcePrefetching` delegovat.
- Vývojáři nyní můžete řídit vzhled oznámení "BADGE" pro položky panelu karta (například barvu textu a pozadí).
- Ovládací prvek obnovit se teď podporuje ve všech posuňte zobrazení a posuňte zobrazení podtřídy (například `UICollectionView`).

## <a name="webkit-enhancements"></a>Vylepšení WebKit

Rozhraní WebKit v iOS 10 byly provedeny následující vylepšení:

- Funkce Náhled a pop podporu byl přidán do `WKWebView` třídy. Použití `ShouldPreviewElement` metoda k určení, zda se dané webové zobrazení na Zobrazit náhled.


## <a name="related-links"></a>Související odkazy

- [iOS 10 – ukázky](https://developer.xamarin.com/samples/ios/iOS10/)
- [Co je nového v iOS 10](https://developer.apple.com/library/prerelease/content/releasenotes/General/WhatsNewIniOS/Articles/iOS10.html#//apple_ref/doc/uid/TP40017084-SW1)
