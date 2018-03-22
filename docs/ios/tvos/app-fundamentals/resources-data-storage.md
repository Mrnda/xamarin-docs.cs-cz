---
title: "Prostředky a úložiště dat"
description: "Tento článek se zabývá práci s prostředky a úložiště dat v aplikaci Xamarin.tvOS."
ms.topic: article
ms.prod: xamarin
ms.assetid: C56B5046-D2C0-4B63-9CE0-ADAA0EFD368A
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/16/2017
ms.openlocfilehash: e72c013516de5bcf97e2e9f58a7a4f5cd87c730b
ms.sourcegitcommit: 73bd0c7e5f237f0a1be70a6c1384309bb26609d5
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 03/22/2018
---
# <a name="resources-and-data-storage"></a>Prostředky a úložiště dat

_Tento článek se zabývá práci s prostředky a úložiště dat v aplikaci Xamarin.tvOS._

<a name="tvOS-Resource-Limitations" />

## <a name="tvos-resource-limitations"></a>tvOS omezení prostředků

Na rozdíl od zařízení iOS poskytuje nové Apple TV velmi omezené trvalé, místní úložiště pro tvOS aplikace nebo data. Pro velmi malé položky (například uživatelské předvolby), vaše aplikace tvOS stále má přístup k `NSUserDefaults` s [maximálně 500 KB dat](https://forums.developer.apple.com/message/50696#50696). Ale pokud vaše aplikace Xamarin.tvOS musí zachovat větší množství informací, musí uložení a načtení dat z [Icloudu](#iCloud-Data-Storage).

Kromě toho tvOS omezuje velikost aplikace Apple TV, na 200MB. Pokud vaše aplikace vyžaduje prostředky nad rámec této velikosti, že bude muset být zabalené a načíst pomocí [prostředky na vyžádání](#On-Demand-Resources) (až o 2 GB). Zadané tato omezení, je důležité správně čas stahování další prostředky pro dosažení co nejlepších výsledků přinášejí uživatelům vaší aplikace. Další informace najdete v tématu společnosti Apple [Průvodce prostředky na vyžádání](https://developer.apple.com/library/prerelease/tvos/documentation/FileManagement/Conceptual/On_Demand_Resources_Guide/index.html#//apple_ref/doc/uid/TP40015083).

<a name="Non-Persistent-Downloads" />

## <a name="non-persistent-downloads"></a>Dočasnou stahování

Dočasné mezipaměti adresáře, který jeho další prostředky a prostředky se stáhnou do každé tvOS aplikace je k dispozici. Tento adresář se zachovají, dokud aplikace stále běží. Však jako Apple TV musí uvolnit místo pro další aplikace nebo systému využití, můžete odstranit tuto mezipaměť.

Aplikaci nelze v důsledku toho spoléhají na stažený obsah, který je k dispozici při příštím jeho spuštění. Aplikace Xamarin.tvOS vždy zkontrolujte existenci požadované prostředky a podle potřeby je stáhnout.

> [!IMPORTANT]
> Při stahování další prostředky a prostředky podle potřeby, Apple varuje proti využívání veškeré místo v mezipaměti vaší aplikace, protože může vést k neočekávaným výsledkům.




<a name="Managing-Resources" />

## <a name="managing-resources"></a>Správa prostředků

Jak je uvedeno výše, kvůli omezené, dočasnou úložiště informací, které jsou k dispozici pro aplikace, tvOS, tato omezení vyžadují pečlivé plánování vytvoření vysoký výkon uživatele pro vaši aplikaci Xamarin.tvOS.

<a name="iCloud-Data-Storage" />

### <a name="icloud-data-storage"></a>Úložiště dat serveru služby iCloud

Úložiště na Apple TV je omezená, a proto ne jenom velmi omezená trvalé, místní úložiště, aplikace musí zaručeno, že všechny informace dříve stažené budou k dispozici při příštím spuštění.

V důsledku toho musí aplikace Xamarin.tvOS ukládání uživatelských dat v úložiště dat Icloudem. Apple nabízí dvě možnosti úložiště dat založených na serveru služby iCloud pro tvOS aplikace:

- **Icloudu klíč-hodnota úložiště (KVS)** – pro malé informace (méně než 1 MB), že vaše aplikace může vyžadovat (např. uživatelské předvolby), můžete použít Icloudu KVS úložiště. Icloudu KVS dat je automaticky synchronizované s cloudem a všechny uživatele zařízení se systémem stejné aplikaci. Další informace najdete v tématu [klíč-hodnota úložiště](~/ios/data-cloud/introduction-to-icloud.md) části našich [Úvod do Icloudu](~/ios/data-cloud/introduction-to-icloud.md) dokument nebo společnosti Apple [návrhu pro Data klíč-hodnota v Icloudu](https://developer.apple.com/library/prerelease/tvos/documentation/General/Conceptual/iCloudDesignGuide/Chapters/DesigningForKey-ValueDataIniCloud.html#//apple_ref/doc/uid/TP40012094-CH7) dokumentace.
- **CloudKit** – úložiště větší údaje (větší než 1 MB), použijte CloudKit Framework společnosti Apple. Na rozdíl od Icloudu KVS úložiště CloudKit data se dají sdílet mezi všichni uživatelé aplikace (stejně jako se privátní jednomu uživateli). Vytvoří další informace, najdete v tématu naše [Úvod do CloudKit](~/ios/data-cloud/intro-to-cloudkit.md) dokumentace nebo společnosti Apple [CloudKit rychlý Start](https://developer.apple.com/library/prerelease/tvos/documentation/DataManagement/Conceptual/CloudKitQuickStart/Introduction/Introduction.html#//apple_ref/doc/uid/TP40014987).

<a name="On-Demand-Resources" />

### <a name="on-demand-resources"></a>Prostředky na vyžádání

Prostředky na vyžádání poskytují obsah aplikace a prostředky (odděleně od sady prostředků aplikace), které jsou hostované na webu App Store a stahovat podle požadavků vaší aplikace. Až o 2 GB dat nelze zpracovat pomocí prostředky na vyžádání. Umožňují stahování počáteční aplikace na menší hodnotu (tvOS aplikace jsou omezeny na maximálně 200MB), současně stále zajišťuje bohaté prostředky podle potřeby.

Pokud aplikace tvOS požádá o prostředky na vyžádání, systém bude automaticky spravovat stahování a úložiště tohoto obsahu do mezipaměti adresáře aplikace. Aplikace musí počkat tento obsah se stáhnou a k dispozici, pak teprve může pokračovat.

Tyto prostředky mohou nadále do mezipaměti na Apple TV v rámci více spustí aplikace, proto povolené rychlosti spuštění cyklu. Aplikace však nelze spoléhat na žádný již dříve stažené obsah, který je k dispozici při příštím jeho spuštění. Najdete v článku [Non-trvalé stáhne](#Non-Persistent-Downloads) části výše další podrobnosti.

Xcode použijete k vytvoření sady související obsah (například všechny prostředky pro herní úroveň 2) přidružené k udělení značky prostředku. Později vaše aplikace bude požadovat na vyžádání prostředků tak, že zadáte tuto značku prostředků. Aplikace by měl k dispozici uživatelského rozhraní pro uživatele s oznámením, že obsah se stahuje. Další informace najdete v tématu společnosti Apple [Průvodce prostředky na vyžádání](https://developer.apple.com/library/prerelease/tvos/documentation/FileManagement/Conceptual/On_Demand_Resources_Guide/index.html#//apple_ref/doc/uid/TP40015083).

> [!IMPORTANT]
> Potřeba dát pozor narazila na rovnováhu mezi počet, kolikrát má aplikace ke stažení na vyžádání prostředky a velikosti jednotlivých souborů ke stažení. Uživatel může Pochopením s vaší aplikací, pokud hraní her přerušení neustále stáhnout nový obsah, nebo pokud jeden stahování trvá příliš dlouho.




<a name="Summary" />

## <a name="summary"></a>Souhrn

Tento článek má zahrnutých umístit do aplikace Xamarin.tvOS systémem tvOS úložiště omezení velikosti, prostředků a data. Ho má uvedené možnosti pro řešení těchto omezení a návrhy k vytvoření vysoký výkon uživatele pro vaši aplikaci.



## <a name="related-links"></a>Související odkazy

- [Ukázky pro tvOS](https://developer.xamarin.com/samples/tvos/all/)
- [tvOS](https://developer.apple.com/tvos/)
- [tvOS lidské rozhraní příručky](https://developer.apple.com/tvos/human-interface-guidelines/)
- [Průvodce programováním aplikace pro tvOS](https://developer.apple.com/library/prerelease/tvos/documentation/General/Conceptual/AppleTV_PG/)
