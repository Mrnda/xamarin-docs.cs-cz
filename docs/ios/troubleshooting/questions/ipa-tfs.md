---
title: Jak můžete zkopírovat soubor IPA výstupní soubory do složky, vyřaďte TFS?
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: B0F1E09E-7315-45BA-B7FF-44D2063EE19C
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/21/2017
ms.openlocfilehash: 087a20ea3b573595e6cbd2b40d77de649676391e
ms.sourcegitcommit: dc882e9631b4ed52596b944a6fbbdde309346943
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/26/2018
ms.locfileid: "31883706"
---
# <a name="how-can-i-copy-ipa-output-files-to-the-tfs-drop-folder"></a>Jak můžete zkopírovat soubor IPA výstupní soubory do složky, vyřaďte TFS?

Otevřete `.csproj` souboru pro projekt aplikace pro iOS v textovém editoru a pak přidejte následující řádky na konci (bezprostředně před uzavírací `</Project>` značka):

```xml
<PropertyGroup>
    <CreateIpaDependsOn>
        $(CreateIpaDependsOn);
        CopyIpa
    </CreateIpaDependsOn>
</PropertyGroup>

<Target Name="CopyIpa"
    Condition="'$(OutputType)' == 'Exe'
        And '$(ComputedPlatform)' == 'iPhone'
        And '$(BuildIpa)' == 'true'
        And '$(TF_BUILD)' == 'true'">
    <Copy
        SourceFiles="$(OutputPath)$(IpaPackageName)"
        DestinationFolder="$(TF_BUILD_BINARIESDIRECTORY)"
    />
</Target>
```

## <a name="notes"></a>Poznámky

-   Toto je obecná stejné techniky popsané v [můžete změnit výstupní cestu k souboru soubor IPA?](~/ios/troubleshooting/questions/ipa-output-path.md). Dva důležité body jsou nastavit `$(TF_BUILD_BINARIESDIRECTORY)` jako cílovou složku a přidat další podmínka tak `CopyIpa` lze spustit pouze pro sestavení sady TFS.

-   Popis `TF_BUILD_BINARIESDIRECTORY` najdete v části [ https://msdn.microsoft.com/library/hh850448.aspx ](https://msdn.microsoft.com/library/hh850448.aspx).

## <a name="additional-references"></a>Další odkazy

- [Dokumentace k instalaci sady TFS pro použití s Xamarin](https://docs.microsoft.com/vsts/tfvc/overview)
- [Úloze sestavení sady TFS: Xamarin.Android](https://docs.microsoft.com/vsts/build-release/tasks/build/xamarin-android)
- [Úloze sestavení sady TFS: Xamarin.iOS](https://docs.microsoft.com/vsts/build-release/tasks/build/xamarin-ios)

### <a name="next-steps"></a>Další kroky
Tento dokument popisuje aktuální chování od Xamarin 3.11.666 pro Visual Studio a Xamarin.iOS 8.10.3 na Mac sestavení hostitele. O další pomoc, kontaktujte nás, nebo pokud tento problém zůstane i po použití výše uvedené informace najdete v tématu [jaké možnosti podpory jsou dostupná pro Xamarin?](~/cross-platform/troubleshooting/support-options.md) informace o možnostech kontaktní, návrhy, a také jak soubor nové chyby v případě potřeby. 



