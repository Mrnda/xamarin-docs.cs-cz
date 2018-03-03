---
title: "Instalace NUnit 2.6.4 pomocí nástroje NuGet"
description: "Tento průvodce obsahuje informace na starší verzi 3.0 NUnit k NUnit 2.6.4 pomocí nástroje NuGet."
ms.topic: article
ms.prod: xamarin
ms.assetid: 7683F2B8-7FDF-48C4-8E7D-649D4D4E79F0
ms.technology: xamarin-cross-platform
author: asb3993
ms.author: amburns
ms.date: 03/24/2017
ms.openlocfilehash: 9dc50aeec88131a1ce49c7e3357382c019774450
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 02/27/2018
---
# <a name="installing-nunit-264-using-nuget"></a>Instalace NUnit 2.6.4 pomocí nástroje NuGet

_Tento průvodce obsahuje informace na starší verzi 3.0 NUnit k NUnit 2.6.4 pomocí nástroje NuGet._

Vývojáři, kteří jsou zápis testů v sadě Visual Studio pro Mac nebo pomocí Xamarin.UITest by měl být pomocí [NUnit 2.6.4](http://nunit.org/index.php?p=docHome&r=2.6.4) není kompatibilní s Visual Studio pro Mac nebo Xamarin.UITest jako NUnit 3.0 nebo vyšší. Probíhá pokus o spuštění testů jednotek v sadě Visual Studio pro Mac nebo Xamarin.UITests s NUnit 3.0 se nezdaří.

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

Tato příručka popisuje postup instalace NUnit 2.6.4 pomocí nástroje NuGet pro Visual Studio for Mac. Tyto kroky odinstaluje NUnit 3.0 také v případě potřeby.

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Tato příručka popisuje starší verzi 3.0 NUnit k NUnit 2.6.4 pomocí nástroje NuGet v sadě Visual Studio 2015.

-----

## <a name="requirements"></a>Požadavky

Tato příručka předpokládá, že je do stávajícího řešení pro projekt mobilní aplikace a projekt testu.

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

## <a name="installing-nunit-264-in-visual-studio-for-mac"></a>Instalace NUnit 2.6.4 v sadě Visual Studio pro Mac

Následující kroky popisují postup instalace NUnit 2.6.4.


1. **Otevřete Správce balíčků** -klikněte pravým tlačítkem na **balíčky** a vyberte **přidat balíčky** z místní nabídky:

    [![](installing-nunit-using-nuget-images/add-packages-xs.png "Klikněte pravým tlačítkem na balíčky a v místní nabídce vyberte Přidat balíčky")](installing-nunit-using-nuget-images/add-packages-xs.png)
    
1. **Vyhledejte `NUnit version:2.6.4`**  -Visual Studio pro Mac odinstalovat NUnit 3.0 (v případě potřeby) a pak stáhnout a nainstalují NUnit 2.6.4. V **přidat balíčky** dialogové okno, zadejte text `nunit version:2.6.4` v **vyhledávání** pole umístěné v pravém horním rohu. Vyberte **NUnit** výsledky hledání a klikněte na **přidat balíček** tlačítko:

    [![](installing-nunit-using-nuget-images/nunit-search-xs.png "Ve výsledcích hledání vyberte NUnit a klikněte na tlačítko Přidat balíček")](installing-nunit-using-nuget-images/nunit-search-xs.png)


Je možné k potvrzení, že byla nainstalována NUnit 2.6.4 zkontrolováním číslo verze balíčku NUnit v řešení pro:

[![](installing-nunit-using-nuget-images/nunit-2-6-4-installed.png "Zkontrolujte číslo verze balíčku NUnit v řešení pro")](installing-nunit-using-nuget-images/nunit-2-6-4-installed.png)

## <a name="summary"></a>Souhrn

Tato příručka popsané postupy na starší verzi 3.0 NUnit k NUnit 2.6.4 v sadě Visual Studio pro Mac pomocí konzoly Správce balíčků.


# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

## <a name="installing-nunit-264-in-visual-studio"></a>Instalace NUnit 2.6.4 v sadě Visual Studio

Tato část je zaměřená na pomocí _Konzola správce balíčků NuGet_ ve Visual Studiu 2015 odinstalovat NUnit 3.0 a nainstalovat NUnit 2.6.4.


1. **Spuštění konzole Správce balíčků NuGet** – vyberte **nástroje > Správce balíčků NuGet > Konzola správce balíčků**:

    [![](installing-nunit-using-nuget-images/package-manager-console.png "Spuštění konzoly Správce balíčků NuGet - konzoly Správce balíčků Správce balíčků NuGet vyberte položku Nástroje")](installing-nunit-using-nuget-images/package-manager-console.png)
    
1. **Ověření verze NUnit** -může volitelně ověřit verzi NUnit, který je nainstalován spuštěním příkazu `Get-Package -Project <UITEST PROJECT>`:

    ```bash
    [PM] Get-Package -Project [TEST PROJECT NAME]
    
        Id                                  Versions                                 ProjectName
        --                                  --------                                 -----------
    NUnit                               {3.0.1}                                  [TEST PROJECT NAME]
    Xamarin.UITest                      {1.2.0}                                  [TEST PROJECT NAME]
    ```

Pokud se zobrazí NUnit 3.0 nebo vyšší, pak musí přejít na NUnit 2.6.4 starší.

1. **Odinstalace NUnit 3.0** -použití `Uninstall-Package` příkaz pro odinstalaci NUnit 3.0:

        <PM> Uninstall-Package NUnit -Project <TEST PROJECT NAME>
        Attempting to gather dependencies information for package 'NUnit.3.0.1' with respect to project '<TEST PROJECT NAME>', targeting '.NETFramework,Version=v4.5'
        Resolving actions to uninstall package 'NUnit.3.0.1'
        Resolved actions to uninstall package 'NUnit.3.0.1'
        Removed package 'NUnit.3.0.1' from 'packages.config'
        Successfully uninstalled 'NUnit.3.0.1' from <TEST PROJECT NAME>

1. **Nainstalujte NUnit 2.6.4** -nainstalovat Nunit 2.6.4 s `Install-Package` příkaz, jak je předvedeno v následující fragment kódu:

        <PM> Install-Package NUnit -Version 2.6.4 -Project <TEST PROJECT NAME>
        Attempting to gather dependencies information for package 'NUnit.2.6.4' with respect to project '<TEST PROJECT NAME>', targeting '.NETFramework,Version=v4.5'
        Attempting to resolve dependencies for package 'NUnit.2.6.4' with DependencyBehavior 'Lowest'
        Resolving actions to install package 'NUnit.2.6.4'
        Resolved actions to install package 'NUnit.2.6.4'
        Adding package 'NUnit.2.6.4' to folder 'Z:\Desktop\DowngradeNunit\packages'
        Added package 'NUnit.2.6.4' to folder 'Z:\Desktop\DowngradeNunit\packages'
        Added package 'NUnit.2.6.4' to 'packages.config'
        Successfully installed 'NUnit 2.6.4' to <TEST PROJECT NAME>
    
## <a name="summary"></a>Souhrn

Tato příručka popsané postupy na starší verzi 3.0 NUnit k NUnit 2.6.4 v sadě Visual Studio 2015 pomocí konzoly Správce balíčků.

-----

## <a name="related-links"></a>Související odkazy

- [NUnit 2.6.4](http://nunit.org/index.php?p=docHome&r=2.6.4)
- [Balíček NuGet NUnit 2.6.4](https://www.nuget.org/packages/NUnit/2.6.4)
