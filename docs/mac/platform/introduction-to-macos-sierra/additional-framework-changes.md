---
title: Další systému macOS Sierra Framework změny
description: Tento dokument popisuje menší změny a vylepšení, které byla zavedená v systému macOS Sierra existující rozhraní. Zkontroluje změny rozhraní Accelerate, AppKit, AVFoundation, základní dat, základní Image, Foundation a další.
ms.prod: xamarin
ms.assetid: CA701269-D11E-4DE3-89C1-58EF8993A482
ms.technology: xamarin-mac
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/14/2017
ms.openlocfilehash: 3cfa2e9bcb0be4d65462914215045c9c7f01da5b
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 06/05/2018
ms.locfileid: "34792590"
---
# <a name="additional-macos-sierra-framework-changes"></a>Další systému macOS Sierra Framework změny

<a name="Accelerate-Framework-Enhancements" />

## <a name="accelerate-framework-enhancements"></a>Urychlit vylepšení Framework

Architektura urychlit pro systému macOS Sierra byly provedeny následující vylepšení:

- Přidání Quadrature (integrální calculus).
- Přidání základní funkce pro vytváření neuronové sítě.
- Přidání geometrickou predikátem funkce chcete otestovat třeba průnik dvou geometrickou objektů.

<a name="AppKit-Framework-Enhancements" />

## <a name="appkit-framework-enhancements"></a>Vylepšení AppKit Framework

Architektura AppKit pro systému macOS Sierra byly provedeny následující vylepšení:

- Několik vylepšení `NSCollectionView` jako například:
    - **Sbalitelné části** – umožňuje uživatelům sbalit zobrazení kolekce části do jednoho vodorovném řádku.
    - **Plovoucí hlavičky** -záhlaví a zápatí můžete nyní obtékané (v případě plovoucího o rozložení) pomocí rozhraní API stejné jako [UICollectionView](https://developer.apple.com/reference/uikit/uicollectionview) v iOS.
    - **Posuvný pozadí zobrazení** -pozadí zobrazení kolekce teď můžete nastavit posun spolu s obsah.
- Předání rozložení odložené zobrazení je optimalizovaná a rozšířené.
- Rozhraní API a přetažení nyní zahrnuje nové `NSFilePromiseProvider` a `NSFilePromiseReceiver` třídy pro podporu přetáhněte vločkování.
- Několik konstruktorů pohodlí byly přidány do existujících ovládacích prvků:
    -  `NSButton` zahrnuje nové konstruktory pro vytváření tlačítek, zaškrtávací políčka a přepínacích tlačítek.
    -  `NSTextField` zahrnuje nové konstruktory pro vytváření obálky a bez zabalení popisky, s atributy popisky a upravitelných textových polí.
    -  `NSSegmentedControl` zahrnuje nové konstruktory pro vytvoření segmentovaným ovládací prvky ze skupiny popisky nebo bitové kopie.
    -  `NSSlider` zahrnuje nové konstruktory pro vytvoření vodorovné lineární posuvníky.
    -  `NSImageView` zahrnuje nové konstruktory pro vytvoření zobrazení upravovat bitové kopie z danou `NSImage`.
-  Nové `NSGridView` byl přidán do automatického rozložení kolekce dílčí zobrazení do mřížka s proměnné velikosti řádků a sloupců, které lze dynamicky skrytý nebo vidět.

<a name="AVFoundation-Framework-Enhancements" />

## <a name="avfoundation-framework-enhancements"></a>Vylepšení AVFoundation Framework

Architektura AVFoundation pro systému macOS Sierra byly provedeny následující vylepšení:

- V systému macOS, aplikace již nemá k provedení různých [AVPlayerItem](https://developer.apple.com/reference/avfoundation/avplayeritem) chování v závislosti na typu obsahu. Jednoduše nastavit `Rate` vlastnost a AVFoundation určí, kdy dostatek obsahu k dispozici pro přehrávání bez zablokování.
- Nové `AVPlayerLooper` třída usnadňuje cykly určitou část média při přehrávání.
- `AVAssetDownloadURLSession` Třída umožňuje stažení a novější přehrávání FairPlay šifrované datové proudy HLS.

<a name="Core-Data-Framework-Enhancements" />

## <a name="core-data-framework-enhancements"></a>Základní Data Framework vylepšení

Architektura dat jádra pro systému macOS Sierra byly provedeny následující vylepšení:

- Kořenové [NSManagedObjectContext](https://developer.apple.com/reference/coredata/nsmanagedobjectcontext) objekty podporuje souběžné chybující a načítání bez serializace.
- [NSPersistentStoreCoordinator](https://developer.apple.com/reference/coredata/nspersistentstorecoordinator) třída udržuje fond úložišť dat SQLite.
- [NSManagedObjectContext](https://developer.apple.com/reference/coredata/nsmanagedobjectcontext) objekty s úložišti dat SQLite Podpora režimu deníku WAL nové generace dotazu funkci, kde můžete spravovat kontexty objektu (MOC) připnut k verze konkrétní databáze pro budoucí načítání a Chybující transakce.
- Pomocí vysoké úrovni `NSPersistenceContainer` k odkazu `NSPersistentStoreCoordinator`, [NSManagedObjectModel](https://developer.apple.com/reference/coredata/nsmanagedobjectmodel) a dalším prostředkům konfigurace základní Data.
- Několik vhodných metod jsou přidané do `NSManagedObject` usnadnit k provedení načtení a vytvoření podtřídy.

Další informace najdete v tématu společnosti Apple [základní Data Framework – referenční informace](https://developer.apple.com/reference/coredata).

<a name="Core-Image-Framework-Enhancements" />

## <a name="core-image-framework-enhancements"></a>Vylepšení Framework základní bitové kopie

Rozhraní základní bitovou kopii pro systému macOS Sierra byly provedeny následující vylepšení:

- `ImageWithExtent` Metodu [CIFilter](https://developer.apple.com/reference/coreimage/cifilter) třída slouží k vložení vlastní zpracování do operace filtru. Základní Image bude vyvolání daného zpětného volání mezi filtry při zpracování obrazu pro výstup nebo zobrazení.
- Aplikaci teď můžete zpracování obrázků v barevný prostor mimo kontext základní Image pracovní barevný prostor převádění směřující barevný prostor před a po zpracování.
- Jádra základní Image může nyní vyžádá výstupní formát konkrétní pixelů.
- Byly přidány následující nové filtry bitové kopie: `CINinePartTitled`, `CINinePartStretched`, `CIHueSaturationValueGradient`, `CIEdgePreserveUpsampleFilter` a `CIClamp`.

<a name="Foundation-Framework-Enhancements" />

## <a name="foundation-framework-enhancements"></a>Vylepšení Framework Foundation

Architektura Foundation pro systému macOS Sierra byly provedeny následující vylepšení:

- Použití [NSDimentions](https://developer.apple.com/reference/foundation/nsdimension) velkokapacitních rozhraní API pro představující, převod a řadu nejběžnější fyzické jednotky, jako zobrazení, délka, rychlosti, doba trvání a teploty.
- Použití [NSISO8601DateFormatter](https://developer.apple.com/reference/foundation/nsiso8601dateformatter) třídu pro analýzu a generování ISO 8601 formátu data.
- Použít novou [NSDateInterval](https://developer.apple.com/reference/foundation/nsdateinterval) třídy pro vytvoření interval výpočtů datum a čas, jako je například doby trvání, při porovnání intervalech a testování pro interval průniků.
- Použití [NSPersonNameComponentsFormatter](https://developer.apple.com/reference/foundation/nspersonnamecomponentsformatter) třída analyzovat elementy jméno osoby z řetězce.
- Použít novou [NSURLSessionTaskMetrics](https://developer.apple.com/reference/foundation/nsurlsessiontaskmetrics) třída k získání metriky pro adresu URL sítě relace.

Další informace najdete v tématu společnosti Apple [Foundation poznámky k verzi pro OS X v10.12 a iOS 10](https://developer.apple.com/library/prerelease/content/releasenotes/Miscellaneous/RN-Foundation-OSX10.12/index.html).

<a name="GameKit-Framework-Enhancements" />

## <a name="gamekit-framework-enhancements"></a>Vylepšení GameKit Framework

Architektura GameKit pro systému macOS Sierra byly provedeny následující vylepšení:

- **Aplikace herní Centrum** je zastaralý a odebrat ze systému macOS. Pokud aplikace používá GameKit, ho _musí_ k dispozici vlastní rozhraní pro zobrazení GameKit funkcí, jako jsou tabulky, atd. 
- Nový typ účtu jen Icloudu byla implementované [GKCloudPlayer](https://developer.apple.com/reference/gamekit/gkcloudplayer) třídy.
- Nové [GKGameSession](https://developer.apple.com/reference/gamekit/gkgamesession) třída poskytuje zobecněný řešení pro správu úložiště dat v herním centru. `GKGameSession` udržuje seznam přehrávače a aplikace je zodpovědná formuláře implementace, jak a kdy je uložený účastnické datum, načíst nebo vyměňují mezi přehrávače. V mnoha případech herní relací můžete nahradit existující shody na základě zapnout, v reálném čase odpovídá nebo trvalé herní uložit metody.

<a name="GamePlayKit-Framework-Enhancements" />

## <a name="gameplaykit-framework-enhancements"></a>Vylepšení GamePlayKit Framework

Architektura GamePlayKit pro systému macOS Sierra byly provedeny následující vylepšení:

- Generování procedurální šumu přidala a slouží k vylepšení realism v přirozeném vyhledávání textury, přidat realism do pohyby kamery a pomáhají generovat bohaté herní světů.
- Rozdělení dat herní world pro efektivní vyhledávání pomocí prostorových dělení.
- Nové Monte Carlo Strategický plánovač ([GKMonteCarloStrategist](https://developer.apple.com/reference/gameplaykit/gkmontecarlostrategist)) se přidal pro výpočty vyčerpávající možné přesunout.
- Byla přidána nové rozhraní API rozhodovacího stromu ([GKDecisionTree](https://developer.apple.com/reference/gameplaykit/gkdecisiontree) a [GKDecisionNode](https://developer.apple.com/reference/gameplaykit/gkdecisionnode)) k vylepšení AI herní sestavení.
- Byla přidána podpora 3D existujícího agenta a chování cesta hledání pomocí nové [GKAgent3D](https://developer.apple.com/reference/gameplaykit/gkagent3d) a [GKGraphNode3D](https://developer.apple.com/reference/gameplaykit/gkgraphnode3d) třídy.
- Použít novou [GKMeshGraph](https://developer.apple.com/reference/gameplaykit/gkmeshgraph) třída k poskytování vysoce výkonné, vyhledávání přirozený cesty.
- Nové [GKScene](https://developer.apple.com/reference/gameplaykit/gkscene) a [GKSKNodeComponent](https://developer.apple.com/reference/gameplaykit/gksknodecomponent) zkontrolujte třídy kombinace GameplayKit a SpriteKit jednodušší než kdy dřív.

<a name="Metal-Framework-Enhancements" />

## <a name="metal-framework-enhancements"></a>Vylepšení systému Framework

Architektura operačního systému pro systému macOS Sierra byly provedeny následující vylepšení:

- 3D aplikace a hry teď můžete použít _teselace_ efektivně vykreslit komplexní scény a geometry prostřednictvím GPU.
- Pomocí funkce specializace můžete vytvořit kolekci vysoce optimalizovaný materiálu a světla kombinace kláves pro scény.
- Poskytuje jemně odstupňovanou kontrolu přidělení prostředků za účelem optimalizace výkonu operačního systému na základě aplikací pomocí prostředků haldách a Memoryless vykreslení cíle.

Další informace najdete v tématu společnosti Apple [Průvodce programováním v systému](https://developer.apple.com/library/prerelease/content/documentation/Miscellaneous/Conceptual/MetalProgrammingGuide/Introduction/Introduction.html#//apple_ref/doc/uid/TP40014221).

<a name="ModelIO-Framework-Enhancements" />

## <a name="model-io-framework-enhancements"></a>Vylepšení vstupně-výstupních operací Framework modelu

Architektura vstupně-výstupních operací modelu pro systému macOS Sierra byly provedeny následující vylepšení:

- Formát souboru USD se teď podporuje.
- Použít novou `MDLMaterialPropertyGraph` třída snadná podpora runtime změny modelů.
- Vzdálenost pole byla přidána podpora pro podepsané [MDLVoxelArray](https://developer.apple.com/reference/modelio/mdlvoxelarray) třídy.
- Použít novou `MDLLightProbeIrradianceDataSource` třída jako pomůcku při sběru dat Light umístění.

<a name="Photos-Framework-Enhancements" />

## <a name="photos-framework-enhancements"></a>Vylepšení fotografie Framework

Architektura fotografie pro systému macOS Sierra byly provedeny následující vylepšení:

- Úpravy fotografií za provozu je nyní k dispozici pro aplikace, které podporují rozhraní fotografie a úpravu rozšíření fotografií (pro použití v rámci fotky a fotoaparát aplikace).
- Použít novou [PHLivePhotoEditingContext](https://developer.apple.com/reference/photos/phlivephotoeditingcontext) třída pro úpravy použity video i přesto obsah fotografie za provozu.
- Použití [CIImageProcessorInput](https://developer.apple.com/reference/coreimage/ciimageprocessorinput) a [CIImageProcessorOutput](https://developer.apple.com/reference/coreimage/ciimageprocessoroutput) třídy, abyste mohli využívat nové funkce procesoru základní Image k provádění úprav.
- Pro podporu fotografie za provozu [PHLivePhoto](https://developer.apple.com/reference/photos/phlivephoto) a [PHLivePhotoView](https://developer.apple.com/reference/photosui/phlivephotoview) třídy přenesené z iOS k systému macOS.

<a name="SceneKit-Framework-Enhancements" />

## <a name="scenekit-framework-enhancements"></a>Vylepšení SceneKit Framework

Architektura SceneKit pro systému macOS Sierra byly provedeny následující vylepšení:

- Nyní zahrnuje nový systém fyzicky na základě vykreslování (PBR) realističtější výsledků s jednodušší vytváření asset.
- Použít novou [SCNLightingModelPhysicallyBased](https://developer.apple.com/reference/scenekit/scnlightingmodelphysicallybased) stínování modelu produktu širokou škálu efekty stínování realistické současně vyžaduje jenom tři základní vlastnosti (`Diffuse`, `Metalness` a `Roughness`).
- Od PBR stínování funguje nejlépe s osvětlení založené na prostředí, použijte `LightingEnvironment` vlastnost přiřadit na základě bitové kopie osvětlení k tan celý scény.
- Použití `IESProfileURL` vlastnost import reálného světla zařízení, které definují osvětlení základní skutečných hodnot, jako je intenzitou (v lumenů) a barvu teploty (ve stupních Kelvin).
- [SCNCamera](https://developer.apple.com/reference/scenekit/scncamera) třídy můžete poskytnout větší realism pomocí funkce záhlaví a efekty. Adaptivní ohrožení použijte k vytvoření automatického účinky nebo použití vinětování, barevné lemování a barvu třídění přidat filmatic účinky na příslušnou hru.
- Funkce fotoaparátu PBR a záhlaví poskytují lepší výsledky než tradiční vykreslování technik a v důsledku toho SceneKit nyní provádí všech výpočtů barev v prostoru lineární barev (pomocí P3 barevný rozsah na celou color zařízení zobrazí).
- Barva nyní SceneKit odpovídá všechny barvy čtení informací o profilu barev.
- SceneKit interpretuje – hodnoty barev součásti v lineární prostoru barva RGB pro všechny typy shaderu.
- Vzhledem k tomu, že SceneKit čte a upravit informace o profilu barev v texture Image, ujistěte se, že tyto informace jsou poskytovány pomocí Asset katalogy pro všechny Image.
- Obě lineární barvu vykreslování místa a celou color lze zakázat zadáním `SCNDisableLinearSpaceRendering` a `SCNDisableWideGamut` klíče v dané aplikaci `Info.plist`.
- Sestavení primátů libovolný mnohoúhelníku, (buď načíst ze souborů nebo vygenerovat prostřednictvím kódu programu) a zadejte geometrie s novou [SCNGeometryPrimitiveTypePolygon](https://developer.apple.com/reference/scenekit/1772322-scenekit_enumerations/scngeometryprimitivetype/scngeometryprimitivetypepolygon) třídy.

<a name="Security-Framework-Enhancements" />

## <a name="security-framework-enhancements"></a>Vylepšení zabezpečení Framework

Zabezpečení rozhraní pro systému macOS Sierra byly provedeny následující vylepšení:

- `SecKey` Rozhraní má modernized a unified pro všechny platformy (iOS, tvOS, watchOS a systému macOS).

<a name="SpriteKit-Framework-Enhancements" />

## <a name="spritekit-framework-enhancements"></a>Vylepšení SpriteKit Framework

Architektura SpriteKit pro systému macOS Sierra byly provedeny následující vylepšení:

- Tilemaps teď podporují tvarů odmocnina, šestiúhelníkový a Izometrické dlaždice pro vytváření 2D, 2,5 D a posouvání straně hry pomocí `SKTileMapMode`, `SKTileGroup`, `SKTileGroupRule` a `SKTileSet` třídy.
- Použít novou `SKWarpGeometry` třídu k roztažení nebo zkreslit [SKSpriteNode](https://developer.apple.com/reference/spritekit/skspritenode) nebo [SKEffectNode](https://developer.apple.com/reference/spritekit/skeffectnode) vykreslování. Nové [SKAction](https://developer.apple.com/reference/spritekit/skaction) třídu lze použít pro animaci přechodů mezi osnově účinky.
- Vlastní shadery můžete zadat atributy (`SKAttribute`), můžete nakonfigurovat samostatně pomocí každý uzel, který používá shaderu zadáním hodnotu atributu (`SKAttributeValue`).
- [SKView](https://developer.apple.com/reference/spritekit/skview) třída poskytuje několik nových metod umožnit jemně odstupňovanou kontrolu nad kdy a jak je vykreslen scény.

<a name="New-Frameworks" />

## <a name="new-frameworks"></a>Nové rozhraní

Tyto architektury byly přidány do systému macOS Sierra:

- **Záměry Framework** -toto rozhraní povolit aplikaci zkontrolujte interakce (například akcí umístění nebo uživatel) a proveďte akce na základě těchto informací.
- **SafariServices Framework** -toto rozhraní povolit aplikaci k vývoji přípony aplikace pro prohlížeč Safari (například obsahu blokování) pro systému macOS a iOS.

## <a name="related-links"></a>Související odkazy

- [Ukázky pro Mac](https://developer.xamarin.com/samples/mac/)
- [Co je nového v OS X 10.12](https://developer.apple.com/library/prerelease/content/releasenotes/MacOSX/WhatsNewInOSX/Articles/OSXv10.html#//apple_ref/doc/uid/TP40017145-SW1)
