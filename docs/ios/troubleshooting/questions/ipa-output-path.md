---
title: Můžete změnit výstupní cestu k souboru soubor IPA?
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: F5E5DCC6-F7CC-48E2-89E8-709E9C269502
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/21/2017
ms.openlocfilehash: 06074566b3d3a05e05a1646c70de211f908f3aa9
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/04/2018
---
# <a name="can-i-change-the-output-path-of-the-ipa-file"></a>Můžete změnit výstupní cestu k souboru soubor IPA?

## <a name="for-cycle-7-and-higher"></a>Cyklus 7 a vyšší
Ano, můžete použít vlastní cíle MSBuild dosáhnout. Nejjednodušší možnost je pravděpodobně zkopírovat `.ipa` souboru po byla vytvořena.

Takto bude fungovat pro všechny iOS projekt, který používá modul sestavení nástroje MSBuild na Mac nebo Windows. (Poznámka: všechny projekty unifikované API použijte modul MSBuild sestavení.)

1. Otevřete `.csproj` souboru pro projekt aplikace pro iOS v textovém editoru a pak přidejte následující řádky na konci (bezprostředně před uzavírací `</Project>` značka):
    
    ```
    <PropertyGroup>
           <CreateIpaDependsOn>
           $(CreateIpaDependsOn);
            CopyIpa
           </CreateIpaDependsOn>
    </PropertyGroup>
    
    <Target Name="CopyIpa"
        Condition="'$(OutputType)' == 'Exe'
            And '$(ComputedPlatform)' == 'iPhone'
            And '$(BuildIpa)' == 'true'">
        <Copy
            SourceFiles="$(IpaPackagePath)"
            DestinationFolder="$(OutputPath)"
        />
    </Target>
    ```

2. Můžete nastavte na požadovanou výstupní složky. Můžete použít jako obvykle vlastnosti nástroje MSBuild (např. $(OutputPath)) v rámci tento argument, pokud chcete.

## <a name="notes"></a>Poznámky
- `CreateIpaDependsOn` Vlastnost je definována v `Xamarin.iOS.Common.targets` souboru, který je součástí Xamarin.iOS. Se chová, jak je popsáno v části *vlastnosti 'DependsOn' přepíše* na [ https://msdn.microsoft.com/en-us/library/ms366724.aspx ](https://msdn.microsoft.com/en-us/library/ms366724.aspx).

- Můžete použít **přesunout** úloh ne **kopie** úloh, pokud vaše preferované. Pokud si zvolíte, že jsou v systému Windows, vytváření a, budete muset použít název plně kvalifikovaný úkolů `<Microsoft.Build.Tasks.Move>` předejdete to nejednoznačnost s XamarinVS úlohy sestavení.

## <a name="for-versions-before-xamarin-studio-6005174--xamarin-for-visual-studio-410530"></a>Pro verze před Xamarin Studio 6.0.0.5174 | Xamarin pro Visual Studio 4.1.0.530

Ano, můžete použít vlastní cíle MSBuild dosáhnout. Nejjednodušší možnost je pravděpodobně zkopírovat `.ipa` souboru po byla vytvořena.

Takto bude fungovat pro všechny iOS projekt, který používá modul sestavení nástroje MSBuild na Mac nebo Windows. (Poznámka: všechny projekty unifikované API použijte modul MSBuild sestavení.)

1. Otevřete `.csproj` souboru pro projekt aplikace pro iOS v textovém editoru a pak přidejte následující řádky na konci (bezprostředně před uzavírací `</Project>` značka).

    ```csharp
    <PropertyGroup>
        <CreateIpaDependsOn>
            $(CreateIpaDependsOn);
            CopyIpa
        </CreateIpaDependsOn>
    </PropertyGroup>
    
    <Target Name="CopyIpa"
        Condition="'$(OutputType)' == 'Exe'
            And '$(ComputedPlatform)' == 'iPhone'
            And '$(BuildIpa)' == 'true'">
        <Copy
            SourceFiles="$(OutputPath)$(IpaPackageName)"
            DestinationFolder="/Users/macuser/Desktop/"
        />
    </Target>
    ```

2. Nastavte `DestinationFolder` na požadovaný výstupní složky. Můžete použít jako obvykle vlastnosti nástroje MSBuild (jako je `$(OutputPath)`) v rámci tento argument, pokud chcete.

## <a name="notes"></a>Poznámky
- `CreateIpaDependsOn` Vlastnost je definována v `Xamarin.iOS.Common.targets` souboru, který je součástí Xamarin.iOS. Se chová, jak je popsáno v části *přepíše "DependsOn" vlastnosti* na [ https://msdn.microsoft.com/en-us/library/ms366724.aspx ](https://msdn.microsoft.com/en-us/library/ms366724.aspx).

- Můžete použít **přesunout** úloh ne **kopie** úloh, pokud vaše preferované. Pokud si zvolíte, že jsou v systému Windows, vytváření a, budete muset použít název plně kvalifikovaný úkolů `<Microsoft.Build.Tasks.Move>` předejdete to nejednoznačnost s XamarinVS úlohy sestavení.
