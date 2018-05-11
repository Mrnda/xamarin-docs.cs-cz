---
title: Instalace rozhraní .NET vložení
ms.prod: xamarin
ms.assetid: 47106AF3-AC6E-4A0E-B30B-9F73C116DDB3
author: chamons
ms.author: chhamo
ms.date: 4/18/2018
ms.openlocfilehash: 1675889dceb1d364abe74461b32aa4c895a144a0
ms.sourcegitcommit: 0a72c7dea020b965378b6314f558bf5360dbd066
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 05/09/2018
---
# <a name="installing-net-embedding"></a>Instalace rozhraní .NET vložení

## <a name="installing-net-embedding-from-nuget"></a>Instalace rozhraní .NET vložení z NuGet

Zvolte **Přidat > Přidání balíčků NuGet...**  a nainstalujte **Embeddinator 4000** ze Správce balíčků NuGet:

![Správce balíčků NuGet](images/visualstudionuget.png)

Dojde k instalaci **Embeddinator 4000.exe** a **objcgen** do **balíčky nebo Embeddinator-4000 nebo nástroje** adresáře.

Obecně platí měla by být vybrána nejnovější verze Embeddinator-4000 pro stahování. Podpora jazyka Objective-C vyžaduje 0.4 nebo novější.

## <a name="running-manually"></a>Ruční spuštění

Teď, když je nainstalovaná NuGet, můžete spustit nástrojů ručně.

- Otevřete terminál (macOS) nebo příkazového řádku (Windows)
- Změnit adresář, do kořenového adresáře řešení
- Nástroji je nainstalován v:
    - **. / balíčky nebo Embeddinator-4000. [Verze] / tools/objcgen** (Objective-C)
    - **. / balíčky nebo Embeddinator-4000. [VERSION]/tools/Embeddinator-4000.exe** (Java/C) 
- V systému macOS **objcgen** můžete spustit přímo. 
- V systému Windows **Embeddinator 4000.exe** můžete spustit přímo.
- V systému macOS **Embeddinator 4000.exe** musí být spuštěn s **mono**: 
    - `mono ./packages/Embeddinator-4000.[VERSION]/tools/Embeddinator-4000.exe`

Každé vyvolání příkazu bude nutné počet paramaters uvedeny v dokumentaci specifické pro platformu.

## <a name="automatic-binding-generation"></a>Vazba automatické generování

Existují dva přístupy pro automatické spouštění rozhraní .NET vložení součást procesu sestavení.

- Vlastní cíle MSBuild
- Kroky po sestavení

Když tento dokument popisuje, jak, pomocí vlastní cíle MSBuild je upřednostňovaný. Kroky sestavení Post nespustí nutně při sestavování z příkazového řádku.

### <a name="custom-msbuild-targets"></a>Vlastní cíle MSBuild

Chcete-li přizpůsobit vaše sestavení pomocí cílů nástroje MSbuild, nejprve vytvořit **Embeddinator 4000.targets** souboru vedle vašeho csproj podobný následujícímu:

```xml
<Project DefaultTargets="Build" xmlns="http://schemas.microsoft.com/developer/msbuild/2003">
    <Target Name="RunEmbeddinator" AfterTargets="AfterBuild" Inputs="$(OutputPath)/$(AssemblyName).dll" Outputs="$(IntermediateOutputPath)/Embeddinator/$(AssemblyName).framework/$(AssemblyName)">
        <Exec Command="" />
    </Target>
</Project>
```

Text příkazu by měla být zde vyplněna jednu z uvedených v dokumentaci specifické pro platformu u volání vložení rozhraní .NET.

Chcete-li použít tento cíl:

- Zavřete projekt v Visual Studio 2017 nebo Visual Studio pro Mac
- V textovém editoru otevřete csproj knihovny
- Přidejte tento řádek v dolní části výše konečné `</Project>` řádku:

```xml
 <Import Project="Embeddinator-4000.targets" />
```

- Otevření projektu

### <a name="post-build-steps"></a>Kroky po sestavení

Postup pro přidání kroku po sestavení ke spuštění .NET vložení lišit v závislosti na prostředí IDE:

#### <a name="visual-studio-for-mac"></a>Visual Studio for Mac

V sadě Visual Studio pro Mac, přejděte na **možnosti projektu > sestavení > vlastní příkazy** a přidejte **po sestavení** krok.

Nastavte příkaz uvedený v dokumentaci specifické pro platformu.

> [!NOTE]
> Nezapomeňte použít číslo verze, které jste nainstalovali z NuGet

Pokud se chystáte to pokračujícího vývoje v projektu jazyka C#, může také přidání vlastního příkazu k odstranění **výstup** adresář před spuštěním vložení .NET:

```shell
rm -Rf '${SolutionDir}/output/'
```

![Vlastní sestavovací akce](images/visualstudiocustombuild.png)

> [!NOTE]
> Vygenerovaný vazby budou umístěny v adresáři indikován `--outdir` nebo `--out` parametr. Název generované vazba se může lišit, jako některé platformy platí omezení na názvy balíčků.

#### <a name="visual-studio-2017"></a>Visual Studio 2017

Nastavíme samé v podstatě, ale trochu liší se v nabídkách v Visual Studio 2017. Příkazy prostředí jsou také mírně lišit.

Přejděte na **možnosti projektu > události sestavení** a zadejte příkaz uvedený v dokumentaci specifické pro platformu do **události po sestavení příkazového řádku** pole. Příklad:

![Vložení na Windows .NET](images/visualstudiowindows.png)
