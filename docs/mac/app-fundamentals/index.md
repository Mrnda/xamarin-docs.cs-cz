---
title: "Základy aplikací"
description: "Tento dokument obsahuje odkazy na příručky, které popisují různé koncepty, které jsou potřebné pro pochopení při vývoji aplikace Xamarin.Mac."
ms.topic: article
ms.prod: xamarin
ms.assetid: 5A36B3A7-F197-4AC3-A40D-B2C49362FF06
ms.technology: xamarin-mac
author: bradumbaugh
ms.author: brumbaug
ms.date: 12/17/2015
ms.openlocfilehash: 9b64647fdb455978c3833ce1bc37f5e8784b9a78
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 02/27/2018
---
# <a name="application-fundamentals"></a>Základy aplikací

## <a name="common-patterns-and-idiomsmacapp-fundamentalspatternsmd"></a>[Běžné vzory a idioms](~/mac/app-fundamentals/patterns.md)

V celém rozhraní API Apple zveřejňovány prostřednictvím jazyka C# určitých idioms a vzory se spustit znovu a znovu. Pokud máte zkušenosti s programování s Xamarin.iOS, může to vypadat povědomě. Dokumentace bude často odkazovat tyto vzory a idioms opakovaně, tak mít solidní znalosti z nich vám pomůže smysl, které můžete najít dokumentaci.

## <a name="understanding-mac-apismacapp-fundamentalsmac-apismd"></a>[Rozhraní API pro rozpoznávání Mac](~/mac/app-fundamentals/mac-apis.md)

Pro většinu doby vývoj s Xamarin.Mac vezměte v úvahu, čtení a zápis v jazyce C# bez mnohem problém na základní rozhraní API jazyka Objective-C. V některých případech můžete ale budete potřebovat od společnosti Apple, přečtěte si dokumentaci rozhraní API převede na odpověď od Stack Overflow řešení pro váš problém, nebo porovnat s existující vzorek.

## <a name="working-with-xib-filesmacapp-fundamentalsxibmd"></a>[Práce se soubory .xib](~/mac/app-fundamentals/xib.md)

Tento článek se zabývá práci s .xib soubory vytvořené v Xcode na tvůrce rozhraní k vytváření a údržbě uživatelského rozhraní pro aplikaci Xamarin.Mac.

## <a name="storyboardxib-less-user-interface-designmacapp-fundamentalsxibless-uimd"></a>[.Storyboard/.XIb méně návrh uživatelského rozhraní](~/mac/app-fundamentals/xibless-ui.md)

Tento článek popisuje vytvoření uživatelského rozhraní aplikace Xamarin.Mac přímo z kódu jazyka C# bez použití Tvůrce Xcode na rozhraní s .storyboard nebo .xib soubory.

## <a name="working-with-imagesmacapp-fundamentalsimagemd"></a>[Práce s obrázky](~/mac/app-fundamentals/image.md)

Tento článek se zabývá práce s obrázky a ikony v aplikaci Xamarin.Mac. Vysvětluje vytváření a údržbu Image nutné k vytvoření ikonu a pomocí bitové kopie v kódu jazyka C# a Tvůrce Xcode je rozhraní vaší aplikace.

## <a name="data-binding-and-key-value-codingmacapp-fundamentalsdatabindingmd"></a>[Datové vazby a klíč hodnota kódování](~/mac/app-fundamentals/databinding.md)

Tento článek popisuje použití kódování a klíč hodnota sledování povolit pro datovou vazbu k elementům uživatelského rozhraní v Xcode na rozhraní tvůrce klíč hodnota. Touto technikou, můžete výrazně snížit množství kódu C#, který musí být napsané pro aplikace Xamarin.Mac. 

## <a name="working-with-databasesmacapp-fundamentalsdatabasesmd"></a>[Práce s databázemi](~/mac/app-fundamentals/databases.md)

Tento článek popisuje použití kódování a klíč hodnota sledování pro vazbu dat s přímým přístupem do databáze SQLite k elementům uživatelského rozhraní v Xcode na rozhraní tvůrce klíč hodnota. Také vysvětluje použití SQLite.NET ORM pro poskytnutí přístupu k datům SQLite.

## <a name="working-with-copy-and-pastemacapp-fundamentalscopy-pastemd"></a>[Práce s kopírování a vkládání](~/mac/app-fundamentals/copy-paste.md)

Tento článek se zabývá práci s pracovní plochou poskytují kopírování a vložení v aplikaci Xamarin.Mac. Ukazuje, jak pracovat s standardní datovými typy, které můžete sdílet mezi více aplikacemi a jak pro podporu vlastních dat v rámci aplikace udělení.

## <a name="sandboxing-a-xamarinmac-appmacapp-fundamentalssandboxingmd"></a>[Sandboxing Xamarin.Mac aplikace](~/mac/app-fundamentals/sandboxing.md)

Tento článek se zabývá sandboxing Xamarin.Mac aplikaci pro verzi na webu App Store. Pokrývá všechny prvky, které patří do sandboxing: kontejneru adresáře, oprávnění, určit uživatele oprávnění, oprávnění oddělení a vynucení jádra.

## <a name="playing-sound-with-avaudioplayermacapp-fundamentalssoundsmd"></a>[Přehrávání zvuku s AVAudioPlayer](~/mac/app-fundamentals/sounds.md)

Tento článek ukazuje, jak používat pomocná třída pro ovládání přehrávání zvuku použití AVAudioPlayer.

## <a name="reporting-bugsmacapp-fundamentalstroubleshootingmd"></a>[Zasílání zpráv o chybách](~/mac/app-fundamentals/troubleshooting.md)

Někdy jsme všechny uváznout při práci na projektu, nemohou získat rozhraní API pro pracovat způsobem, který má být buď v pokusu o obejít chyby. V Xamarin Naším cílem je pro vás být úspěšný zápis mobilních a desktopových aplikací, a nabízíme některé prostředky, které pomohou.
