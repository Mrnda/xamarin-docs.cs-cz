---
title: Cíle Sharpie nástroje a příkazy
description: Tento dokument obsahuje přehled nástrojů součástí Sharpie cíl a argumenty příkazového řádku, které mají být použity s nimi.
ms.prod: xamarin
ms.assetid: A84E209B-8932-4CC1-BAD1-7FD51F798A97
author: asb3993
ms.author: amburns
ms.date: 10/05/2015
ms.openlocfilehash: 9ef566559249caca75281d9490d5314e08e26d44
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 06/05/2018
ms.locfileid: "34781062"
---
# <a name="objective-sharpie-tools--commands"></a>Cíle Sharpie nástroje a příkazy

_Přehled nástroje součástí Sharpie cíl a argumenty příkazového řádku, jejich použití._

<style type="text/css"> .Terminal blue {color: rgb(10,96,254);} .terminal zelená {barva: rgb(12,156,26);} .terminal purpurová {barva: rgb(152,12,103);} </style>


Jakmile Sharpie cíl je úspěšně [nainstalován](~/cross-platform/macios/binding/objective-sharpie/get-started.md), otevřete terminál a seznamte se s <em>příkazy</em> Sharpie cíl má nabízet:

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
|**Xcode**|Poskytuje informace o aktuální instalaci Xcode a verze iOS a Mac sady SDK, které jsou k dispozici. Použijeme tyto informace později při se vygeneruje naše vazby.|
|**pod**|Hledat, nakonfiguruje, nainstaluje (v místním adresáři) a sváže jazyka Objective-C [CocoaPod](https://cocoapods.org/) knihovny k dispozici z hlavní specifikace úložiště. Tento nástroj vyhodnotí nainstalované CocoaPod automaticky odvodit správnou vstup předat `bind` nástroj níže. Nové ve 3.0!|
|**Vazby**|Analyzuje hlavičkových souborů (`*.h`) v knihovně jazyka Objective-C do počáteční [ApiDefinition.cs a StructsAndEnums.cs](~/cross-platform/macios/binding/objective-sharpie/platform/apidefinitions-structsandenums.md) soubory.|
|**update**|Kontroluje novější verze Sharpie cíl a stáhne a spustí instalační program, pokud je k dispozici.|
|**Ověřte dokumentace**|Obsahuje podrobné informace o `[Verify]` atributy.|
|**Dokumentace**|Přejde na tento dokument ve webovém prohlížeči výchozí.|

Potřebujete pomoc na konkrétní nástroj Sharpie cíl, zadejte název tohoto nástroje a `-help` možnost. Například `sharpie xcode -help` vrátí následující výstup:

<pre>$ <b>sharpie xcode -help</b>
usage: sharpie xcode [OPTIONS]

Options:
  -h, -help        Show detailed help
  -v, -verbose     Be verbose with output

Xcode Options:
  -sdks            List all available Xcode SDKs. Pass -verbose for more details.</pre>

Proces vytváření vazby můžeme začít, potřebujeme získat informace o našich aktuální nainstalovaných sadách SDK zadáním následujícího příkazu do terminálu `sharpie xcode -sdks`. Výstupu se můžou lišit v závislosti na tom, jaké verze Xcode jste nainstalovali. Cíle Sharpie hledá sady SDK nainstalovat v jakékoli `Xcode*.app` pod `/Applications` directory:

<pre>$ <b>sharpie xcode -sdks</b>
<span class="terminal-blue">sdk:</span> appletvos9.0    <span class="terminal-green">arch:</span> arm64
<span class="terminal-blue">sdk:</span> iphoneos9.1     <span class="terminal-green">arch:</span> arm64   armv7
<span class="terminal-blue">sdk:</span> iphoneos9.0     <span class="terminal-green">arch:</span> arm64   armv7
<span class="terminal-blue">sdk:</span> iphoneos8.4     <span class="terminal-green">arch:</span> arm64   armv7
<span class="terminal-blue">sdk:</span> macosx10.11     <span class="terminal-green">arch:</span> x86_64  i386
<span class="terminal-blue">sdk:</span> macosx10.10     <span class="terminal-green">arch:</span> x86_64  i386
<span class="terminal-blue">sdk:</span> watchos2.0      <span class="terminal-green">arch:</span> armv7</pre>

Z výše uvedeného vidíme, že máme `iphoneos9.1` SDK v našem počítači nainstalován a obsahuje `arm64` podpora architektury. Použijeme tuto hodnotu pro všechny ukázky v této části. Tyto informace na místě a jsme připraveni k analýze jazyka Objective-C knihovny hlavičkových souborů do počáteční `ApiDefinition.cs` a `StructsAndEnums.cs` pro projekt vazby.

