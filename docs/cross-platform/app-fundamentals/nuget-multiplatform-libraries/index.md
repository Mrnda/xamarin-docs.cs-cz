---
title: Projekty NuGet (Nugetizer 3000)
description: "Automaticky vytvořte balíčky NuGet sdílet kód napříč platformami pomocí 3000 Nugetizer!"
ms.topic: article
ms.prod: xamarin
ms.assetid: F0A5A9BB-86CD-44C9-8EE8-74D1E5E74A30
ms.technology: xamarin-cross-platform
author: asb3993
ms.author: amburns
ms.date: 11/22/2017
ms.openlocfilehash: 49e7c00feb697d25d61a5e09b051c41945c260c6
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 03/09/2018
---
# <a name="nuget-projects-nugetizer-3000"></a>Projekty NuGet (Nugetizer 3000)

_Automaticky vytvořte balíčky NuGet sdílet kód napříč platformami pomocí 3000 Nugetizer!_

Je možné automaticky vytvořit balíčky NuGet pro sdílení kódu napříč platformami pomocí _Nugetizer 3000_. Tento díky je možné vytvořit balíčky NuGet z existující projekty knihovny nebo vytvoříte novou **projektu knihovny Multiplatform**.

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

Nugetizer 3000 je obsažen v sadě Visual Studio pro Mac 6.2.

[![](images/mulitplatform-library-sml.png "Vytvoření nové okno Multiplatform knihovny")](images/mulitplatform-library.png#lightbox)

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Chcete-li použít Nugetizer 3000 v sadě Visual Studio, [stáhněte a spusťte instalační program VSIX](http://bit.ly/nugetizer-2017).

-----

## <a name="building-nuget-packages"></a>Vytváření balíčků NuGet

Po nakonfigurování každé sestavení projektu výstupy kompletní balíček NuGet, které lze použít ke sdílení kódu interně s jinými aplikacemi nebo nahrán do [NuGet.org](https://www.nuget.org).

Existují tři scénáře pro použití této funkce:

- [Existující projekty knihoven](existing-library.md)

  Vytvořte balíček NuGet z existující projekty PCL (nebo .NET Standard).

- [Vytvoření nového projektu s více platformami knihovny](single-codebase.md)

  Vytvořte novou knihovnu, kterou chcete sdílet společný kód prostřednictvím balíčku NuGet, pomocí PCL nebo .NET Standard.

- [Vytváření nových projektů knihovny specifické pro platformu](platform-specific.md)

  Vytvořte novou knihovnu a NuGet, který zahrnuje specifické pro platformu kódu pro iOS a Android a používá sdílený projekt tak, aby obsahovala společný kód a specifické pro platformu projekty pro podporu funkcí konkrétní iOS nebo Android.

Odkazovat [Metadata průvodce](metadata.md) podrobnosti o požadované a volitelné metadata, která musí být přidaný do jakékoli balíček NuGet.


## <a name="further-nuget-information"></a>Další informace o NuGet

Další informace o [ruční vytvoření NuGets pro Xamarin](~/cross-platform/app-fundamentals/nuget-manual.md) a postup [zahrnují balíčku NuGet v aplikaci](https://docs.microsoft.com/visualstudio/mac/nuget-walkthrough).

Společnosti Microsoft [NuGet dokumentace](https://docs.microsoft.com/nuget/) obsahuje podrobnější informace o **.nupkg** formátu a pomocí balíčků NuGet v sadě Visual Studio.

Diskusní návrhu pro projekty balíček NuGet (také známa jako NuGetizer 3000) je k dispozici na [úložiště NuGet GitHub](https://github.com/NuGet/Home/wiki/NuGetizer-3000).


## <a name="related-links"></a>Související odkazy

- [Případy použití NuGetizer 3000](https://github.com/NuGet/Home/wiki/NuGetizer-Core-Scenarios)
- [Ručně vytvořit balíčky NuGet pro Xamarin](~/cross-platform/app-fundamentals/nuget-manual.md)
- [Dokumentace NuGet](https://docs.microsoft.com/nuget/)
- [Zpráva k vydání verze](https://developer.xamarin.com/releases/studio/xamarin.studio_6.2/xamarin.studio_6.2/#NuGetizer_3000)
