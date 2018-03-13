---
title: "Principy aplikací"
description: "Základní koncepty aplikace"
ms.topic: article
ms.prod: xamarin
ms.assetid: 7D179ACF-09A6-46EE-B49D-E27AB5F09CD4
ms.technology: xamarin-cross-platform
author: asb3993
ms.author: amburns
ms.date: 02/18/2018
ms.openlocfilehash: c5b823370e5b65fbcf9ba366cb89c05e003b1a89
ms.sourcegitcommit: 0fdb243b46cf21be47584900805cadcd077121bf
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 03/12/2018
---
# <a name="application-fundamentals"></a>Principy aplikací

Tato část poskytuje návod na některých běžných úloh věcí nebo koncepty, které vývojáři potřeba myslet při vývoji mobilních aplikací.

##  <a name="building-cross-platform-applicationscross-platformapp-fundamentalsbuilding-cross-platform-applicationsindexmd"></a>[Vytváření multiplatformních aplikací](~/cross-platform/app-fundamentals/building-cross-platform-applications/index.md)

Výběr Xamarin a zachování pár věcí na paměti, když návrh a vývoj mobilních aplikací, můžete mějte na paměti strávíte kód sdílení různé mobilní platformy, zkrátit dobu uvedení na trh, využít existující talentu, splňují poptávka pro mobilní přístup a snížit složitost a platformy. &nbsp;Tento dokument popisuje klíčové pokyny k porozumění tyto výhody pro nástroj a kancelářské aplikace.

## <a name="code-sharing-optionscode-sharingmd"></a>[Možnosti sdílení kódu](code-sharing.md)

Další informace o různých kód sdílení možnosti jsou dostupné pro Xamarin projektech, včetně přenosné knihovny tříd (PCLs), sdílených projektů a standardní knihovny .NET.


## <a name="accessibilityaccessibilitymd"></a>[Usnadnění](accessibility.md)

Tipy pro vytváření aplikací dostupné.


## <a name="localizationlocalizationmd"></a>[Lokalizace](localization.md)

Pokyny pro zajištění deklaracemi národního prostředí aplikací, které by bylo možné převést do různých jazyků.


##  <a name="portable-class-librariescross-platformapp-fundamentalspclmd"></a>[Přenosné knihovny tříd](~/cross-platform/app-fundamentals/pcl.md)

Přenosná knihovna tříd projekty umožňují vytvářet a distribuovat sestavení, které obsahují sdílené kód pro spuštění ve více platformách. K vytvoření přenosné knihovny tříd (nebo "PCL") vyberte nejdřív platforem, které cíle a napsat kód pro dílčí sadu rozhraní .NET Framework, která je k dispozici v profilu definované pro tyto platformy. Tento dokument popisuje, jak vytvořit a používat PCLs s funkcí Xamarin.

##  <a name="shared-projectscross-platformapp-fundamentalsshared-projectsmd"></a>[Sdílené projekty](~/cross-platform/app-fundamentals/shared-projects.md)

Sdílení projektů umožňují zápisu společný kód, který je odkazován počet projekty jinou aplikaci. Kód kompiluje v rámci každého odkazující projektu a může obsahovat direktivy kompilátoru pomohou začlenit funkce specifické pro platformu v základu sdíleného kódu. Tento článek popisuje, jak fungují sdílených projektů a jak vytvořit a použít je s projekty Xamarin.

##  <a name="net-standardcross-platformapp-fundamentalsnet-standardmd"></a>[.NET Standard](~/cross-platform/app-fundamentals/net-standard.md)

.NET standard je nová možnost pro sdílení kódu napříč platformami. Funguje podobným způsobem na přenosné knihovny tříd; kód je založený na konkrétní verzi (aktuálně 1.0 prostřednictvím 1.6) a pak je spotřebované jinými projekty, které podporují této úrovni nebo vyšší. .NET standard projekty jsou podporované v Xamarin Studio 6.2, Visual Studio pro Windows a Visual Studio for Mac.

##  <a name="nuget-projects-multiplatform-libraries-for-code-sharingcross-platformapp-fundamentalsnuget-multiplatform-librariesindexmd"></a>[NuGet projekty: S více platformami knihovny pro sdílení kódu](~/cross-platform/app-fundamentals/nuget-multiplatform-libraries/index.md)

Balíčky NuGet může automaticky vygenerovat z PCL nebo .NET standardní projekty; a sdílených projektů se dá zabalit do balíčků NuGet "návnada a přepínač" pomocí samostatných typ projektu NuGet. Tato část vysvětluje, jak vytvořit balíčky NuGet pro každý scénář sdílení kódu.

##  <a name="manually-creating-nuget-packages-for-xamarincross-platformapp-fundamentalsnuget-manualmd"></a>[Ruční vytvoření balíčků NuGet pro Xamarin](~/cross-platform/app-fundamentals/nuget-manual.md)

Tipy pro vytváření balíčků NuGet, které pracují s platformou Xamarin.

##  <a name="cross-platform-data-accessxamarin-formsdata-cloudindexmd"></a>[Křížové platformy přístup k datům](~/xamarin-forms/data-cloud/index.md)

Většina aplikací mít některé požadavek k uložení dat v zařízení místně. Pokud je trivially malé množství dat, většinou je potřeba databáze a datové vrstvy v aplikaci pro správu přístupu k databázi. iOS a Android máte databázový stroj SQLite "součástí" a přístup k uložení a načtení dat se zjednodušilo díky Xamarin pro platformu. [Přístup k datům Android](~/android/data-cloud/data-access/index.md), [přístup k datům iOS](~/ios/data-cloud/data/index.md), a [přístup k datům Xamarin.Forms](~/xamarin-forms/data-cloud/index.md) příručky obsahují příklady, jak k přístupu ke SQLite na každou platformu.


##  <a name="transport-layer-securitytransport-layer-securitymd"></a>[Transport Layer Security](transport-layer-security.md)

Informace o selectingthe správné implementace SSL/TLS k zabezpečení připojení k síti vaší aplikace.


##  <a name="notificationsxamarin-formsdata-cloudpush-notificationsindexmd"></a>[Oznámení](~/xamarin-forms/data-cloud/push-notifications/index.md)

Mobilní aplikace použijte oznámení jako nerušivý způsob informující uživatele, který některé aplikace konkrétní události došlo. Oznámení jsou obvykle používány k upozornit uživatele stav procesy aplikace, které běží na pozadí. Příkladem může být stahování velký soubor. Tento soubor může trvat dlouhou dobu stáhnete, takže tato aktivita má probíhat na pozadí. Po dokončení stahování uživatel informován o fakt oznámení.
Kromě toho oznámení arů není právě omezený na místní aplikace. Je také možné pro serverové aplikace k publikování oznámení do mobilní aplikace. Tento článek popisuje postup použití oznámení pro Android a iOS.
