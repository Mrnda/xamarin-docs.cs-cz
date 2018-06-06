---
title: Další watchOS 3 architektury změny
description: Tento dokument popisuje různé framework změny zavedené ve watchOS 3 a postupy pro práci s nimi v Xamarin. Jsou uvedeny základní dat, základní pohybu, Foundation, HealthKit, HomeKit, PassKit a UIKit.
ms.prod: xamarin
ms.assetid: FE93796E-F699-4B14-B37D-D39F9D48E81E
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/17/2017
ms.openlocfilehash: af44096928c5e543ac99df3faec9f2e9215f666d
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 06/05/2018
ms.locfileid: "34791511"
---
# <a name="additional-watchos-3-frameworks-changes"></a>Další watchOS 3 architektury změny

_Tento článek se zabývá další, méně závažné změny nebo vylepšení stávajících rozhraní pro watchOS 3._

Kromě hlavních změn do systému iOS má Apple provedené změny a vylepšení několik existujících architektur v watchOS 3.


## <a name="core-data"></a>Základní Data

Tato vylepšení provedli Data základní rozhraní pro sledování OS 3:

- Kořenové [NSManagedObjectContext](https://developer.apple.com/reference/coredata/nsmanagedobjectcontext) objekty podporuje souběžné chybující a načítání bez serializace.
- [NSPersistentStoreCoordinator](https://developer.apple.com/reference/coredata/nspersistentstorecoordinator) třída udržuje fond úložišť dat SQLite.
- [NSManagedObjectContext](https://developer.apple.com/reference/coredata/nsmanagedobjectcontext) objekty s úložišti dat SQLite Podpora režimu deníku WAL nové generace dotazu funkci, kde můžete spravovat kontexty objektu (MOC) připnut k verze konkrétní databáze pro budoucí načítání a Chybující transakce.
- Pomocí vysoké úrovni `NSPersistenceContainer` k odkazu `NSPersistentStoreCoordinator`, [NSManagedObjectModel](https://developer.apple.com/reference/coredata/nsmanagedobjectmodel) a dalším prostředkům konfigurace základní Data.
- Několik vhodných metod jsou přidané do `NSManagedObject` usnadnit k provedení načtení a vytvoření podtřídy.

Další informace najdete v tématu společnosti Apple [základní Data Framework – referenční informace](https://developer.apple.com/reference/coredata).


## <a name="core-motion"></a>Základní pohybu

Architektura jádra pohybu pro sledování OS 3 být provedli následující vylepšení:

- Novou událost pohybu zařízení používá zrychlení a volný setrvačník k poskytování aktualizací pohybu a orientace. aplikace aplikace můžete zaregistrovat pro tuto aktualizaci (tempem až 100 Hz).
- Novou událost Pedometer umožňuje rychlé a v reálném čase oznámení, když uživatel pozastaví a obnoví systémem. Použití [CMPedometer](https://developer.apple.com/reference/coremotion/cmpedometer) k registraci pro události pedometer popředí nebo na pozadí.


## <a name="foundation"></a>Foundation

Architektura Foundation pro sledování 3 operačního systému se provedli následující vylepšení:

- Použít novou [NSDateInterval](https://developer.apple.com/reference/foundation/nsdateinterval) třídy pro vytvoření interval výpočtů datum a čas, jako je například doby trvání, při porovnání intervalech a testování pro interval průniků.
- Několik nových vlastností se přidala [NSLocal](https://developer.apple.com/reference/foundation/nslocale) třída získat místní informace a formáty dostupná zobrazení.
- Použít novou [NSMeasuerment](https://developer.apple.com/reference/foundation/nsmeasurement) třída pro převod mezi různé jednotky z měr (Měrná jednotka) nebo výpočet hodnot v různých UOMs.
- Použít novou [NSMeasurementFormatter](https://developer.apple.com/reference/foundation/nsmeasurementformatter) třída k formátování lokalizované měření pro zobrazení pro koncového uživatele.
- Použít novou [NSUnit](https://developer.apple.com/reference/foundation/nsunit) a [NSDimension](https://developer.apple.com/reference/foundation/nsdimension) třídy pro představující konkrétní UOMs.


## <a name="healthkit"></a>HealthKit

Tato vylepšení provedli HealthKit rozhraní pro sledování 3 operačního systému:

- Použít novou [HKWorkoutConfiguration](https://developer.apple.com/reference/healthkit/hkworkoutconfiguration) třídu k určení `ActivityType` a `LocationType` z cvičení.
- Nové [HKWheelchairUseObject](https://developer.apple.com/reference/healthkit/hkwheelchairuseobject) a `WheelchairUse` metodu [HKHealthStore](https://developer.apple.com/reference/healthkit/hkhealthstore) třídy byly přidány pro práci s invalidního související data o stavu.
- Byly přidány nové klíče metadata pro počasí typy (například `HKWeatherConditionClear` a `HKWeatherConditionCloudy`) a cvičení typy (například `HKWorkoutActivityTypeFlexibility` a `HKWorkoutActivityTypeWheelchairRunPace`) byly přidány.


## <a name="homekit"></a>HomeKit

Tato vylepšení provedli HomeKit rozhraní pro sledování 3 operačního systému:

- Přidat možnost zobrazení a interakci s HomeKit připojené IP kamery.
- Přidat několik nových služeb a vlastnosti.
- Přidat další kontext a konfiguraci příslušenství primární služeb a služeb odkaz.


## <a name="passkit"></a>PassKit

Tato vylepšení provedli PassKit rozhraní pro sledování 3 operačního systému:

- Rozšíří rozhraní pro podporu zabezpečeného plateb v aplikaci v Apple Watch fyzické zboží a služeb.
- Následující třídy jsou nyní k dispozici: [PKPayment](https://developer.apple.com/reference/passkit/pkpayment), [PKPaymentMethod](https://developer.apple.com/reference/passkit/pkpaymentmethod), [PKPaymentRequest](https://developer.apple.com/reference/passkit/pkpaymentrequest) a [PKPaymentToken](https://developer.apple.com/reference/passkit/pkpaymenttoken)


## <a name="uikit"></a>UIKit

Tato vylepšení provedli UIKit rozhraní pro sledování 3 operačního systému:

- Pro podporu dynamický typ v popisky, použít novou textových polí a polí `PreferredFontForTextStyle` metodu `UIFont` třídy.
- `ColorWithDisplayP3` Metoda byla přidaná kvůli podpoře široké barev.


## <a name="related-links"></a>Související odkazy

- [Začínáme (ukázka)](https://developer.xamarin.com/samples/monotouch/WatchKit/)
- [Co je nového v watchOS 3](https://developer.apple.com/library/prerelease/content/releasenotes/General/WhatsNewInwatchOS/Articles/watchOS3.html#//apple_ref/doc/uid/TP40017085-SW1)
