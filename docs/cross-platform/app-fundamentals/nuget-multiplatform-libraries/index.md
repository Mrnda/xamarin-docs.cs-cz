---
title: Projekty knihovny Multiplatformní NuGet (neboli Nugetizer 3000)
description: Tento dokument popisuje, jak automaticky vytvořit balíčky NuGet pro sdílení kódu napříč platformami pomocí nástroje Nugetizer 3000.
ms.prod: xamarin
ms.assetid: F0A5A9BB-86CD-44C9-8EE8-74D1E5E74A30
author: conceptdev
ms.author: crdun
ms.date: 07/25/2018
ms.openlocfilehash: 1d48bc28aa4477361ca8057fda91ee3258f36a73
ms.sourcegitcommit: 46bb04016d3c35d91ff434b38474e0cb8197961b
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/26/2018
ms.locfileid: "39270425"
---
# <a name="nuget-multiplatform-library-projects-nugetizer-3000"></a>Projekty Multiplatformní knihovna NuGet (Nugetizer 3000)

_Automaticky vytvořte balíčky NuGet, se sdílení kódu napříč platformami využijte 'Nugetizer 3000'!_

Je možné automaticky vytvořit balíčky NuGet, se sdílení kódu napříč platformami pomocí _Nugetizer 3000_. Toto je je možné vytvořit balíčky NuGet z existujících projektů knihovny nebo vytvořit nový **projektu Multiplatformní knihovny**.

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

Nugetizer 3000 je součástí sady Visual Studio pro Mac &ndash; vyhledejte **knihovny > Mulitplatform knihovny** typ v projektu **soubor > Nový** okno:

[![](images/mulitplatform-library-sml.png "Vytvořte nový časový interval pro Multiplatformní knihovna")](images/mulitplatform-library.png#lightbox)

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

Chcete-li použít Nugetizer 3000 v sadě Visual Studio, [stáhněte a spusťte instalátor VSIX](http://bit.ly/nugetizer-2017).

-----

## <a name="building-nuget-packages"></a>Vytváření balíčků NuGet

Po nakonfigurování výstupy každé sestavení projektu kompletní balíček NuGet, který můžete použít ke sdílení kódu interně s jinými aplikacemi nebo nahrát na [NuGet.org](https://www.nuget.org).

Existují tři scénáře pro použití této funkce:

- [Existující projekty knihoven](existing-library.md)

  Vytvoří balíček NuGet z existujících projektů PCL (nebo .NET Standard).

- [Vytvoření nového projektu Multiplatformní knihovna](single-codebase.md)

  Vytvořte novou knihovnu sdílet společný kód prostřednictvím balíčku NuGet, pomocí PCL nebo .NET Standard.

- [Vytváření nových projektů knihovny pro konkrétní platformu](platform-specific.md)

  Vytvořte novou knihovnu a NuGet, který obsahuje kód specifický pro platformu pro zařízení s iOS a Android a používá sdílený projekt tak, aby obsahovala společný kód a projekty specifické pro platformu iOS nebo Android konkrétní funkci.

Odkazovat [Průvodce metadaty](metadata.md) podrobné informace o požadované a volitelné metadata, která musí být přidán do jakéhokoli balíčku NuGet.

## <a name="further-nuget-information"></a>Další informace o NuGet

Další informace o [ruční vytvoření balíčky Nuget pro Xamarin](~/cross-platform/app-fundamentals/nuget-manual.md) a jak [zahrnutí balíčku NuGet do aplikace](https://docs.microsoft.com/visualstudio/mac/nuget-walkthrough).

Společnosti Microsoft [dokumentace pro NuGet](https://docs.microsoft.com/nuget/) obsahuje podrobnější informace o **.nupkg** formátu a používat balíčky NuGet v sadě Visual Studio.

Diskuse k návrhu pro projekty balíčku NuGet (označovaný také jako NuGetizer 3000) je k dispozici na [úložiště NuGet GitHub](https://github.com/NuGet/Home/wiki/NuGetizer-3000).

## <a name="related-links"></a>Související odkazy

- [Případy použití NuGetizer 3000](https://github.com/NuGet/Home/wiki/NuGetizer-Core-Scenarios)
- [Ruční vytváření balíčků NuGet pro Xamarin](~/cross-platform/app-fundamentals/nuget-manual.md)
- [Dokumentace pro NuGet](https://docs.microsoft.com/nuget/)
- [Zpráva k vydání verze](https://developer.xamarin.com/releases/studio/xamarin.studio_6.2/xamarin.studio_6.2/#NuGetizer_3000)
