---
title: "Další tvOS 10 architektury změny"
description: "Tento článek se zabývá další, méně závažné změny nebo vylepšení stávajících rozhraní pro tvOS 10."
ms.topic: article
ms.prod: xamarin
ms.assetid: F771640A-F92E-4954-82D5-2D720434971E
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/16/2017
ms.openlocfilehash: 004abfffd3a100b7a25a9647fe233fd676f61143
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 02/27/2018
---
# <a name="additional-tvos-10-frameworks-changes"></a>Další tvOS 10 architektury změny

_Tento článek se zabývá další, méně závažné změny nebo vylepšení stávajících rozhraní pro tvOS 10._


Kromě hlavní změny tvOS má Apple provedené změny a vylepšení několik existujících architektur v tvOS 10.

<a name="AV-Foundation-Framework" />

## <a name="av-foundation-framework-additions"></a>Přidání Framework AV Foundation

Rozhraní framework AVFoundation obsahuje následující vylepšení:

 - V tvOS 10 aplikace už implementuje různých [AVPlayerItem](https://developer.apple.com/reference/avfoundation/avplayeritem) chování v závislosti na typu obsahu. Jednoduše nastavit `Rate` vlastnost a AVFoundation určí, kdy dostatek obsahu k dispozici pro přehrávání bez zablokování.
 - Nové `AVPlayerLooper` třída usnadňuje cykly určitou část média při přehrávání.

<a name="AVKit-Framework-Enhancements" />

## <a name="avkit-framework-enhancements"></a>Vylepšení AVKit Framework

Rozhraní framework AVKit obsahuje následující vylepšení:

 - Teď má kontrolu nad přeskočení chování aplikace [AVPlayerViewController](https://developer.apple.com/reference/avkit/avplayerviewcontroller) tak gesto přeskočení může přesunout na další položku v seznamu stop nebo zálohy v rámci aktuální položky.

<a name="Core-Data-Enhancements" />

## <a name="core-data-enhancements"></a>Vylepšení dat jádra

tvOS 10 obsahuje následující vylepšení Framework Core dat:

 - Kořenové [NSManagedObjectContext](https://developer.apple.com/reference/coredata/nsmanagedobjectcontext) objekty podporuje souběžné chybující a načítání bez serializace.
 - [NSPersistentStoreCoordinator](https://developer.apple.com/reference/coredata/nspersistentstorecoordinator) třída udržuje fond úložišť dat SQLite.
 - [NSManagedObjectContext](https://developer.apple.com/reference/coredata/nsmanagedobjectcontext) objekty s úložišti dat SQLite Podpora režimu deníku WAL nové generace dotazu funkci, kde můžete spravovat kontexty objektu (MOC) připnut k verze konkrétní databáze pro budoucí načítání a Chybující transakce.
 - Pomocí vysoké úrovni `NSPersistenceContainer` k odkazu `NSPersistentStoreCoordinator`, [NSManagedObjectModel](https://developer.apple.com/reference/coredata/nsmanagedobjectmodel) a dalším prostředkům konfigurace základní Data.
 - Několik vhodných metod jsou přidané do `NSManagedObject` usnadnit k provedení načtení a vytvoření podtřídy.

Další informace najdete v tématu společnosti Apple [základní Data Framework – referenční informace](https://developer.apple.com/reference/coredata).

<a name="Core-Graphics-Enhancements" />

## <a name="core-graphics-enhancements"></a>Vylepšení grafiky jádra

tvOS 10 obsahuje následující vylepšení Framework grafiky jádra:

 - Nové [CGColorConverterRef](https://developer.apple.com/reference/coregraphics/cgcolorconverterref) třídu lze použít k provedení řady převody barev.

<a name="Core-Image-Enhancements" />

## <a name="core-image-enhancements"></a>Vylepšení základní bitové kopie

tvOS 10 díky těmto vylepšením pro základní bitovou kopii prostředí:

 - `ImageWithExtent` Metodu [CIFilter](https://developer.apple.com/reference/coreimage/cifilter) třída slouží k vložení vlastní zpracování do operace filtru. Základní Image bude vyvolání daného zpětného volání mezi filtry při zpracování obrazu pro výstup nebo zobrazení.
 - Aplikaci teď můžete zpracování obrázků v barevný prostor mimo kontext základní Image pracovní barevný prostor převádění směřující barevný prostor před a po zpracování.
 - Jsme provedli několik vylepšení výkonu vykreslování `UIImage` vykreslování (Pokud je zajištěna podpora úložiště bitové kopie základní Image) ve `UIImageView` objekty. 
 - `UIImage` objekty s příznakem celou rozsah vykreslí jako celou rozsah, barvy v `UIImageView` objekty na zařízeních s iOS, které podporují celou barev.
 - Základní Image jádra kódu můžete nyní vyžádá formáty výstupu konkrétní pixelů.

Kromě toho byly přidány následující nové filtry základní Image:

 - `CINinePartTiled`
 - `CINinePartStretched`
 - `CIHueSaturationValueGradient`
 - `CIEdgePreserveUpsampleFilter`
 - `CIClamp`

<a name="Foundation-Enhancements" />

## <a name="foundation-enhancements"></a>Vylepšení Foundation

Architektura Foundation pro tvOS 10 byly provedeny následující vylepšení:

 - Použít novou [NSDateInterval](https://developer.apple.com/reference/foundation/nsdateinterval) třídy pro vytvoření interval výpočtů datum a čas, jako je například doby trvání, při porovnání intervalech a testování pro interval průniků.
 - Několik nových vlastností se přidala [NSLocal](https://developer.apple.com/reference/foundation/nslocale) třída získat místní informace a formáty dostupná zobrazení.
 - Použít novou [NSMeasuerment](https://developer.apple.com/reference/foundation/nsmeasurement) třída pro převod mezi různé jednotky z měr (Měrná jednotka) nebo výpočet hodnot v různých UOMs.
 - Použít novou [NSMeasurementFormatter](https://developer.apple.com/reference/foundation/nsmeasurementformatter) třída k formátování lokalizované měření pro zobrazení pro koncového uživatele.
 - Použít novou [NSUnit](https://developer.apple.com/reference/foundation/nsunit) a [NSDimension](https://developer.apple.com/reference/foundation/nsdimension) třídy pro představující konkrétní UOMs.

<a name="GameKit-Enhancements" />

## <a name="gamekit-enhancements"></a>Vylepšení GameKit

Rozhraní GameKit v tvOS 10 byly provedeny následující vylepšení:

 - Nový typ účtu jen Icloudu byla implementované [GKCloudPlayer](https://developer.apple.com/reference/gamekit/gkcloudplayer) třídy.
 - Nové [GKGameSession](https://developer.apple.com/reference/gamekit/gkgamesession) třída poskytuje zobecněný řešení pro správu úložiště dat v herním centru. `GKGameSession` udržuje seznam přehrávače a aplikace je zodpovědná formuláře implementace, jak a kdy je uložený účastnické datum, načíst nebo vyměňují mezi přehrávače. V mnoha případech herní relací můžete nahradit existující shody na základě zapnout, v reálném čase odpovídá nebo trvalé herní uložit metody.

<a name="GameplayKit-Enhancements" />

## <a name="gameplaykit-enhancements"></a>Vylepšení GameplayKit

Rozhraní GameplayKit v tvOS 10 byly provedeny následující vylepšení:

 - Generování procedurální šumu přidala a slouží k vylepšení realism v přirozeném vyhledávání textury, přidat realism do pohyby kamery a pomáhají generovat bohaté herní světů.
 - Rozdělení dat herní world pro efektivní vyhledávání pomocí prostorových dělení.
 - Nové Monte Carlo Strategický plánovač ([GKMonteCarloStrategist](https://developer.apple.com/reference/gameplaykit/gkmontecarlostrategist)) se přidal pro výpočty vyčerpávající možné přesunout.
 - Byla přidána nové rozhraní API rozhodovacího stromu ([GKDecisionTree](https://developer.apple.com/reference/gameplaykit/gkdecisiontree) a [GKDecisionNode](https://developer.apple.com/reference/gameplaykit/gkdecisionnode)) k vylepšení AI herní sestavení.
 - Byla přidána podpora 3D existujícího agenta a chování cesta hledání pomocí nové [GKAgent3D](https://developer.apple.com/reference/gameplaykit/gkagent3d) a [GKGraphNode3D](https://developer.apple.com/reference/gameplaykit/gkgraphnode3d) třídy.
 - Použít novou [GKMeshGraph](https://developer.apple.com/reference/gameplaykit/gkmeshgraph) třída k poskytování vysoce výkonné, vyhledávání přirozený cesty.
 - Nové [GKScene](https://developer.apple.com/reference/gameplaykit/gkscene) a [GKSKNodeComponent](https://developer.apple.com/reference/gameplaykit/gksknodecomponent) zkontrolujte třídy kombinace GameplayKit a SpriteKit jednodušší než kdy dřív.

<a name="Metal-Enhancements" />

## <a name="metal-enhancements"></a>Vylepšení systému

Rozhraní systému v tvOS 10 byly provedeny následující vylepšení:

 - 3D aplikace a hry teď můžete použít _teselace_ efektivně vykreslit komplexní scény a geometry prostřednictvím GPU.
 - Pomocí funkce specializace můžete vytvořit kolekci vysoce optimalizovaný materiálu a světla kombinace kláves pro scény.
 - Poskytuje jemně odstupňovanou kontrolu přidělení prostředků za účelem optimalizace výkonu operačního systému na základě aplikací pomocí prostředků haldách a Memoryless vykreslení cíle.

Další informace najdete v tématu společnosti Apple [Průvodce programováním v systému](https://developer.apple.com/library/prerelease/content/documentation/Miscellaneous/Conceptual/MetalProgrammingGuide/Introduction/Introduction.html#//apple_ref/doc/uid/TP40014221).

<a name="Metal-Performance-Shaders-Enhancements" />

## <a name="metal-performance-shaders-enhancements"></a>Vylepšení shadery systému výkonu

Rozhraní framework shadery výkonu operačního systému v tvOS 10 byly provedeny následující vylepšení:

 - Mnoho nové jádra byly přidány do rozhraní shadery výkonu operačního systému umožňuje aplikaci využít vysoce optimalizovaný, data paralelní výpočty například operace neuronové sítě a místo převody barev.

<a name="ModelIO-Enhancements" />

## <a name="modelio-enhancements"></a>Vylepšení ModelIO

Rozhraní ModelIO v tvOS 10 byly provedeny následující vylepšení:

 - Formát souboru USD se teď podporuje.
 - Použít novou `MDLMaterialPropertyGraph` třída snadná podpora runtime změny modelů.
 - Vzdálenost pole byla přidána podpora pro podepsané [MDLVoxelArray](https://developer.apple.com/reference/modelio/mdlvoxelarray) třídy.
 - Použít novou `MDLLightProbeIrradianceDataSource` třída jako pomůcku při sběru dat Light umístění.

<a name="SceneKit-Enhancements" />

## <a name="scenekit-enhancements"></a>Vylepšení SceneKit

Rozhraní SceneKit v tvOS 10 byly provedeny následující vylepšení:

 - SceneKit nyní zahrnuje nový systém fyzicky na základě vykreslování (PBR) realističtější výsledků s jednodušší vytváření asset.
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

<a name="SpriteKit-Enhancements" />

## <a name="spritekit-enhancements"></a>Vylepšení SpriteKit

Rozhraní SpriteKit v tvOS 10 byly provedeny následující vylepšení:

 - Tilemaps teď podporují tvarů odmocnina, šestiúhelníkový a Izometrické dlaždice pro vytváření 2D, 2,5 D a posouvání straně hry pomocí `SKTileMapMode`, `SKTileGroup`, `SKTileGroupRule` a `SKTileSet` třídy.
 - Použít novou `SKWarpGeometry` třídu k roztažení nebo zkreslit [SKSpriteNode](https://developer.apple.com/reference/spritekit/skspritenode) nebo [SKEffectNode](https://developer.apple.com/reference/spritekit/skeffectnode) vykreslování. Nové [SKAction](https://developer.apple.com/reference/spritekit/skaction) třídu lze použít pro animaci přechodů mezi osnově účinky.
 - Vlastní shadery můžete zadat atributy (`SKAttribute`), můžete nakonfigurovat samostatně pomocí každý uzel, který používá shaderu zadáním hodnotu atributu (`SKAttributeValue`).
 - [SKView](https://developer.apple.com/reference/spritekit/skview) třída poskytuje několik nových metod umožnit jemně odstupňovanou kontrolu nad kdy a jak je vykreslen scény.

<a name="UIKit-Enhancements" />

## <a name="uikit-enhancements"></a>Vylepšení UIKit

Rozhraní UIKit v tvOS 10 byly provedeny následující vylepšení:

 - Fokus rozhraní API je vylepšená tak, aby podporují fokus položky bez zobrazení kromě `UIViews`. Položky, které podporují fokus _musí_ implementovat `IUIFocusItem` rozhraní.
 - Nové `UIGraphicsRender` třída poskytuje metody objektově orientované vytvoření rastrové obrázky nebo soubory PDF z UIKit vykreslování nebo základní grafické prvky a nahradí zastaralé `UIGraphicsBeginImageContext` metoda.
 - `UIUserInterfaceStyle` Třída byla přidána k určení, které motiv uživatelské rozhraní (tmavý nebo světla) je aktuálně aktivní.
 - Byla přidána nová podpora plně interaktivní, na základě objektů, přerušitelné animace a van ho propojit s gesta. Stránku najdete Apple na [referenci na protokol UIViewAnimating](https://developer.apple.com/reference/uikit/uiviewanimating), [referenci třídy UIViewPropertyAnimator](https://developer.apple.com/reference/uikit/uiviewpropertyanimator), [referenci na protokol UITimingCurveProvider](https://developer.apple.com/reference/uikit/uitimingcurveprovider), [Referenci třídy UICubicTimingParameters](https://developer.apple.com/reference/uikit/uicubictimingparameters) a [referenci třídy UISpringTimingParameter](https://developer.apple.com/reference/uikit/uispringtimingparameters) Další informace.
 - Nové `UIPreviewInteraction` a `UIPreviewInteractionDelegate` povolit aplikaci aplikace nabízí vlastní rozhraní pro operace funkce Náhled a pop.
 - Nové `UIAccessibilityCustomRotor` třída umožňuje aplikaci poskytují vlastní, konkrétní funkce pro usnadnění technologie, například hlasových přes.
 - Použití `UIAccessibilityIsAssistiveTouchRunning` a `UIAccessibilityAssistiveTouchStatusDidChangeNotification` symboly, které chcete zjistit, zda je povoleno AssistiveTouch.
 - Použití `UIAccessibilityHearingDevicePairedEar` a `UIAccessibilityHearingDevicePairedEarDidChangeNotification` symboly a získat stav každého spárovat pomůcky sluchu MFI s připojením.
 - Nové [UIPasteboard](https://developer.apple.com/reference/uikit/uipasteboard) rozhraní API poskytuje nové možnosti (třeba omezení životnosti) a bude automaticky deklarovat kompatibilní typy obsahu pro běžné typy tříd.
 - Pro podporu dynamický typ v popisky, použít novou textových polí a polí `PreferredFontForTextStyle` metodu `UIFont` třídy.
 - Rozhodnout, pokud element by měl aktualizovat písmo při zařízení `UIContentSizeCategory` změny, použijte `AdjustsFontForContentSizeCategory` vlastnost `UIContentSizeCategoryAdjusting` delegovat.
 - Aplikaci teď můžete řídit vzhled oznámení "BADGE" pro položky panelu karta například barvu textu a pozadí.
 - Ovládací prvek obnovit v teď podporuje v všechny posuňte zobrazení a posuňte zobrazení podtřídy (například `UICollectionView`).
 - `OpenURL` Metodu `UIApplication` třídy se nazývá asynchronně teď podporuje doplňování obslužná rutina, která je volána po dokončení open.
 - Spustit CloudKit sdílení a upravit její vlastnosti pomocí nové `UICloudSharingController` a `UICloudSharingControllerDelegate` třídy.
 - Využít výhod prefetched buněk k vylepšení možností posouvání `UICollectionViews` s novým `UICollectionViewDataSourcePrefetching` delegovat.



## <a name="related-links"></a>Související odkazy

- [Ukázky pro tvOS](https://developer.xamarin.com/samples/tvos/all/)
- [Co je nového v tvOS 10](https://developer.apple.com/library/prerelease/content/releasenotes/General/WhatsNewinTVOS/Articles/tvOS10.html#//apple_ref/doc/uid/TP40017259-SW1)
