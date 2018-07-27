---
title: Sdílení kódu na různých platformách
description: Tento dokument obsahuje odkazy na různá vodítka, které popisují techniky pro sdílení kódu, včetně přenosných knihoven tříd, sdílené projekty, .NET Standard a NuGet.
ms.prod: xamarin
ms.assetid: 7D179ACF-09A6-46EE-B49D-E27AB5F09CD4
author: conceptdev
ms.author: crdun
ms.date: 07/18/2018
ms.openlocfilehash: 3a2c3f98e3ba83db0794a68ff1d62a9845a111c0
ms.sourcegitcommit: 46bb04016d3c35d91ff434b38474e0cb8197961b
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/26/2018
ms.locfileid: "39270186"
---
# <a name="sharing-code-on-multiple-platforms"></a>Sdílení kódu na různých platformách

Tyto články popisují různé možnosti, které jsou k dispozici pro sdílení kódu napříč platformami, včetně Windows, Android, iOS a další.

## <a name="code-sharing-overviewcode-sharingmd"></a>[Přehled sdílení kódu](code-sharing.md)

Další informace o různých sdílení možnosti, které jsou k dispozici pro projekty Xamarin, včetně knihovny .NET Standard a sdílené projekty kódu. Přenosné knihovny tříd jsou také podporovány, ale se považují za zastaralé a .NET Standard.

## <a name="net-standardcross-platformapp-fundamentalsnet-standardmd"></a>[.NET Standard](~/cross-platform/app-fundamentals/net-standard.md)

.NET standard je upřednostňovanou možností pro sdílení kódu napříč platformami. Kód je sestaveno s konkrétní verzi (2.0 poskytuje nejlepší rozhraní API kompatibilitu s existující kód rozhraní .NET Framework) a je pak možné spotřebované jinými projekty, které podporují tuto úroveň nebo vyšší. Projekty .NET standard teď se podporují v sadě Visual Studio 2017 a Visual Studio pro Mac.

## <a name="shared-projectscross-platformapp-fundamentalsshared-projectsmd"></a>[Sdílené projekty](~/cross-platform/app-fundamentals/shared-projects.md)

Sdílené projekty umožňují napsat společný kód, který je odkazován celou řadou projektů různé aplikace. Kód je zkompilován jako součást každé odkazující projekt a může obsahovat direktivy kompilátoru pomáhají začlenit funkce specifické pro platformu v sdílené kódové základny. Tento článek popisuje, jak fungují sdílené projekty a jak vytvořit a použít je s projekty Xamarin.

## <a name="portable-class-librariescross-platformapp-fundamentalspclmd"></a>[Přenosné knihovny tříd](~/cross-platform/app-fundamentals/pcl.md)

Přenosné knihovny tříd projektů umožňují vytváření a distribuce sestavení obsahující sdílený kód ke spuštění na více platformách. K vytvoření přenosné knihovny tříd (nebo "PCL") můžete nejprve vybrat platformy, na kterých chcete cílit, a programovat proti dílčí sadu rozhraní .NET Framework, která je k dispozici v profilu definován pro tyto platformy. PCLs považují za zastaralé v nejnovějším verzím sady Visual Studio; Vývojáři jsou ukončena. doporučujeme místo toho použít .NET Standard 2.0.

## <a name="nuget-projects-multiplatform-libraries-for-code-sharingcross-platformapp-fundamentalsnuget-multiplatform-librariesindexmd"></a>[Projekty NuGet: Multiplatformní knihovny pro sdílení kódu](~/cross-platform/app-fundamentals/nuget-multiplatform-libraries/index.md)

Balíčky NuGet se dá automaticky vygenerovat na standardní projekty PCL nebo .NET a sdílené projekty se dá zabalit do balíčků NuGet "bait a přepínač" pomocí samostatného typu projektu NuGet. Tato část vysvětluje, jak vytvořit balíčky NuGet pro jednotlivé scénáře sdílení kódu.

## <a name="manually-creating-nuget-packages-for-xamarincross-platformapp-fundamentalsnuget-manualmd"></a>[Ruční vytváření balíčků NuGet pro Xamarin](~/cross-platform/app-fundamentals/nuget-manual.md)

Tipy pro vytváření balíčků NuGet, které využívají platformu Xamarin.
