---
title: Principy aplikací Xamarin.Mac
description: Tento dokument obsahuje odkazy na pokyny, které popisují různé koncepty, které jsou nezbytné pro zjištění při vývoji aplikací Xamarin.Mac.
ms.prod: xamarin
ms.assetid: 5A36B3A7-F197-4AC3-A40D-B2C49362FF06
ms.technology: xamarin-mac
author: bradumbaugh
ms.author: brumbaug
ms.date: 12/17/2015
ms.openlocfilehash: e085cf33615d216e1ce9963254050ef2b623ae40
ms.sourcegitcommit: d0da5ce4158239abd2b36f67550e9b475055766a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/27/2018
ms.locfileid: "39320801"
---
# <a name="xamarinmac-application-fundamentals"></a>Principy aplikací Xamarin.Mac

## <a name="common-patterns-and-idiomsmacapp-fundamentalspatternsmd"></a>[Společné vzory a idiomy](~/mac/app-fundamentals/patterns.md)

V celém rozhraní API Apple zveřejňovány prostřednictvím jazyka C# určité idiomy a vzory objevují znovu a znovu. Pokud máte zkušenosti s programování s Xamarin.iOS, může to vypadat povědomě. Dokumentace ke službě bude často odkazovat tyto vzory a idiomy opakovaně, tak s důkladného porozumění je vám pomůže vyznat se na dokumentaci, kterou najdete.

## <a name="understanding-mac-apismacapp-fundamentalsmac-apismd"></a>[Vysvětlení rozhraní API Macu](~/mac/app-fundamentals/mac-apis.md)

Pro mnoho času vývoj s využitím Xamarin.Mac představit, čtení a zápis v jazyce C# bez obav, mnoho základní rozhraní API jazyka Objective-C. Ale někdy budete potřebujete od společnosti Apple, přečtěte si dokumentaci rozhraní API přeložit odpověď z přetečení zásobníku do řešení pro váš problém, nebo porovnat s existující vzorek.

## <a name="console-appsmacapp-fundamentalsconsolemd"></a>[Aplikace konzoly](~/mac/app-fundamentals/console.md)

Můžete také sestavit "bezobslužný" konzolové aplikace, které přistupují k macOS nativní rozhraní API pomocí Xamarin.Mac.

## <a name="working-with-xib-filesmacapp-fundamentalsxibmd"></a>[Práce se soubory .xib](~/mac/app-fundamentals/xib.md)

Tento článek popisuje práci se soubory .xib vytvořili v Xcode Tvůrce rozhraní pro vytváření a údržbu uživatelská rozhraní pro aplikace Xamarin.Mac.

## <a name="storyboardxib-less-user-interface-designmacapp-fundamentalsxibless-uimd"></a>[.Storyboard/.XIb méně návrh uživatelského rozhraní](~/mac/app-fundamentals/xibless-ui.md)

Tento článek se týká vytvoření uživatelského rozhraní pro aplikace Xamarin.Mac přímo z kódu jazyka C# bez použití Tvůrce rozhraní Xcode .storyboard nebo .xib soubory.

## <a name="working-with-imagesmacapp-fundamentalsimagemd"></a>[Práce s obrázky](~/mac/app-fundamentals/image.md)

Tento článek se týká práce s obrázky a ikony aplikace Xamarin.Mac. Popisuje vytvoření a správa imagí musí provést k vytvoření ikonu a používání imagí v kódu jazyka C# a Tvůrce rozhraní Xcode vaší aplikace.

## <a name="data-binding-and-key-value-codingmacapp-fundamentalsdatabindingmd"></a>[Vytváření datových vazeb a kódování klíč hodnota](~/mac/app-fundamentals/databinding.md)

Tento článek se věnuje psaní kódu a sledování umožňující vytváření datových vazeb k elementům uživatelského rozhraní v Xcode Tvůrce rozhraní klíč hodnota klíč hodnota. Tímto způsobem můžete výrazně snížit množství kódu jazyka C#, který musí být napsaný pro vaše aplikace Xamarin.Mac. 

## <a name="working-with-databasesmacapp-fundamentalsdatabasesmd"></a>[Práce s databázemi](~/mac/app-fundamentals/databases.md)

Tento článek se věnuje psaní kódu a sledování povolit pro datovou vazbu s přímým přístupem do databáze SQLite k elementům uživatelského rozhraní v Xcode Tvůrce rozhraní klíč hodnota klíč hodnota. Věnuje se také používání SQLite.NET ORM pro poskytnutí přístupu k datům SQLite.

## <a name="working-with-copy-and-pastemacapp-fundamentalscopy-pastemd"></a>[Práce s kopírování a vkládání](~/mac/app-fundamentals/copy-paste.md)

Tento článek popisuje práci s pracovní plocha poskytují kopírování a vložení do aplikace Xamarin.Mac. Ukazuje, jak pracovat s standardních datových typů, které je možné sdílet mezi více aplikacemi a jak podporovat vlastní data v rámci aplikace s daným.

## <a name="sandboxing-a-xamarinmac-appmacapp-fundamentalssandboxingmd"></a>[Izolace (sandbox) aplikace Xamarin.Mac](~/mac/app-fundamentals/sandboxing.md)

Tento článek se týká izolace (sandbox) aplikace Xamarin.Mac pro verzi na App Store. Pokrývá všechny prvky, které patří do izolace (sandbox): kontejner adresářů, oprávnění, určit uživatele oprávnění, oddělení oprávnění a vynucení jádra.

## <a name="playing-sound-with-avaudioplayermacapp-fundamentalssoundsmd"></a>[Přehrávání zvuku s Avaudioplayerem](~/mac/app-fundamentals/sounds.md)

Tento článek ukazuje, jak používat pomocnou třídu pro řízení přehrávání zvuku Avaudioplayerem použití.

## <a name="reporting-bugsmacapp-fundamentalstroubleshootingmd"></a>[Zasílání zpráv o chybách](~/mac/app-fundamentals/troubleshooting.md)

V některých případech můžeme všechny zablokuje při práci na projektu, nelze získat rozhraní API pro práci způsob, jakým chceme, aby nebo při práci na chybu. Naším cílem v Xamarinu je pro vás být úspěšné při zápisu mobilních a desktopových aplikací, a nabízíme některé prostředky, které pomůžou.
