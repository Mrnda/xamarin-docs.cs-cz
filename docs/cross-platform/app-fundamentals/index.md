---
title: Sdílení kódu
description: Základní koncepty aplikace
ms.prod: xamarin
ms.assetid: 7D179ACF-09A6-46EE-B49D-E27AB5F09CD4
ms.technology: xamarin-cross-platform
author: asb3993
ms.author: amburns
ms.date: 02/18/2018
ms.openlocfilehash: 01116a35dca80cd92ea16232a2abb127f60d9f0a
ms.sourcegitcommit: 4b0582a0f06598f3ff8ad5b817946459fed3c42a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 05/03/2018
---
# <a name="sharing-code"></a>Sdílení kódu

Tato část poskytuje návod na některých běžných úloh věcí nebo koncepty, které vývojáři potřeba myslet při vývoji mobilních aplikací.

## <a name="code-sharing-overviewcode-sharingmd"></a>[Přehled sdílení kódu](code-sharing.md)

Další informace o různých kód sdílení možnosti jsou dostupné pro Xamarin projektech, včetně přenosné knihovny tříd (PCLs), sdílených projektů a standardní knihovny .NET.


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
