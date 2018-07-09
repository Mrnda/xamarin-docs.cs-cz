---
title: Cíle Sharpie nástroje a příkazy
description: Tento dokument obsahuje přehled nástroje s cílem Sharpie a argumenty příkazového řádku pro použití s nimi.
ms.prod: xamarin
ms.assetid: A84E209B-8932-4CC1-BAD1-7FD51F798A97
author: asb3993
ms.author: amburns
ms.date: 10/05/2015
ms.openlocfilehash: 718b5104ddc4593d080b88b062c42d371d9e8e2e
ms.sourcegitcommit: ec50c626613f2f9af51a9f4a52781129bcbf3fcb
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/05/2018
ms.locfileid: "37855065"
---
# <a name="objective-sharpie-tools--commands"></a>Cíle Sharpie nástroje a příkazy

_Přehled nástroje s cílem Sharpie a argumenty příkazového řádku k jejich použití._

<style type="text/css"> .Terminal modrá {barva: rgb(10,96,254);} .terminal zelená {barva: rgb(12,156,26);} .terminal – purpurová {color: rgb(152,12,103);} </style>


Jakmile cíle Sharpie úspěšně [nainstalované](~/cross-platform/macios/binding/objective-sharpie/get-started.md), otevřete terminál a seznamte se s <em>příkazy</em> Sharpie cíle může nabídnout:

<pre>$ <b>sharpie -help</b>
usage: sharpie [OPTIONS] TOOL [TOOL_OPTIONS]

Options:
  -h, -help                Show detailed help
  -v, -version             Show version information

Telemetry Options:
  -tlm-about               Show a detailed overview of what usage and binding
                             information will be submitted to Xamarin by
                             default when using Objective Sharpie.
  -tlm-do-not-submit       Do not submit any usage or binding information to
                             Xamarin. Run 'sharpie -tml-about' for more
                             information.
  -tlm-do-not-identify     Do not submit Xamarin account information when
                             submitting usage or binding information to Xamarin
                             for analysis. Binding attempts and usage data will
                             be submitted anonymously if this option is
                             specified.

Available Tools:
  xcode              Get information about Xcode installations and available SDKs.
  pod                Create a Xamarin C# binding to Objective-C CocoaPods
  bind               Create a Xamarin C# binding to Objective-C APIs
  update             Update to the latest release of Objective Sharpie
  verify-docs        Show cross reference documentation for [Verify] attributes
  docs               Open the Objective Sharpie online documentation</pre>

Cíle Sharpie obsahuje následující nástroje:

|Nástroj|Popis|
|--- |--- |
|**xcode**|Poskytuje informace o aktuální instalaci Xcode a verze zařízení s iOS a Mac sad SDK, které jsou k dispozici. Použijeme tyto informace později při generování jsme naše vazby.|
|**pod**|Hledá, nakonfiguruje, nainstaluje (v místním adresáři) a Objective-C váže [CocoaPod](https://cocoapods.org/) knihovny z hlavní technický úložiště. Tento nástroj vyhodnotí nainstalovaných CocoaPod automaticky odvodit správný vstup předat `bind` nástroje níže. Novinka v 3.0!|
|**Vytvoření vazby**|Analyzuje soubory hlaviček (`*.h`) v knihovně jazyka Objective-C do původní [ApiDefinition.cs a StructsAndEnums.cs](~/cross-platform/macios/binding/objective-sharpie/platform/apidefinitions-structsandenums.md) soubory.|
|**update**|Kontroluje novější verze Sharpie cíle a soubory ke stažení a spustí instalační program, pokud je k dispozici.|
|**Ověřte dokumentace**|Obsahuje podrobné informace o `[Verify]` atributy.|
|**dokumentace**|Přejde na tento dokument ve výchozím webovém prohlížeči.|

Můžete zobrazit nápovědu pro konkrétní cíl Sharpie nástroj, zadejte název tohoto nástroje a `-help` možnost. Například `sharpie xcode -help` vrátí následující výstup:

<pre>$ <b>sharpie xcode -help</b>
usage: sharpie xcode [OPTIONS]

Options:
  -h, -help        Show detailed help
  -v, -verbose     Be verbose with output

Xcode Options:
  -sdks            List all available Xcode SDKs. Pass -verbose for more details.</pre>

Než začneme proces vytváření vazby, musíte získat informace o našem aktuálním nainstalovaných sad SDK tak, že do terminálu zadáte následující příkaz `sharpie xcode -sdks`. Výstup se může lišit v závislosti na tom, jaké verze Xcode jste nainstalovali. Cíle Sharpie hledá sady SDK nainstalovat v jakékoli `Xcode*.app` pod `/Applications` adresáře:

<pre>$ <b>sharpie xcode -sdks</b>
<span class="terminal-blue">sdk:</span> appletvos9.0    <span class="terminal-green">arch:</span> arm64
<span class="terminal-blue">sdk:</span> iphoneos9.1     <span class="terminal-green">arch:</span> arm64   armv7
<span class="terminal-blue">sdk:</span> iphoneos9.0     <span class="terminal-green">arch:</span> arm64   armv7
<span class="terminal-blue">sdk:</span> iphoneos8.4     <span class="terminal-green">arch:</span> arm64   armv7
<span class="terminal-blue">sdk:</span> macosx10.11     <span class="terminal-green">arch:</span> x86_64  i386
<span class="terminal-blue">sdk:</span> macosx10.10     <span class="terminal-green">arch:</span> x86_64  i386
<span class="terminal-blue">sdk:</span> watchos2.0      <span class="terminal-green">arch:</span> armv7</pre>

Z výše uvedeného můžeme vidět, že máme `iphoneos9.1` na naše počítači nainstalované sady SDK a má `arm64` podpora architektury. Použijeme tuto hodnotu pro všechny ukázky v této části. Pomocí těchto informací v místě, jsme připraveni k analýze souborech hlaviček knihovny Objective-C do počáteční `ApiDefinition.cs` a `StructsAndEnums.cs` vazby projektu.

## <a name="related-links"></a>Související odkazy

- [Xamarin University kurz: Vytvoření knihovny vazeb Objective-C](https://university.xamarin.com/classes/track/all#building-an-objective-c-bindings-library)
- [Xamarin University kurz: Vytvoření knihovny vazeb Objective-C pomocí cíle Sharpie](https://university.xamarin.com/classes/track/all#build-an-objective-c-bindings-library-with-objective-sharpie)