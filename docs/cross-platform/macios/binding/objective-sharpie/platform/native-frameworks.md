---
title: "Vazba nativní rozhraní"
ms.topic: article
ms.prod: xamarin
ms.assetid: 91AE058A-3A1F-41A9-9DE4-4B96880A1869
ms.technology: xamarin-cross-platform
author: asb3993
ms.author: amburns
ms.date: 01/15/2016
ms.openlocfilehash: 4588a879452b4ee49535ac8882977be2c064a43f
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 02/27/2018
---
# <a name="binding-native-frameworks"></a>Vazba nativní rozhraní

Někdy je nativní knihovny distribuován jako [framework](https://developer.apple.com/library/mac/documentation/MacOSX/Conceptual/BPFrameworks/Concepts/WhatAreFrameworks.html). Cíle Sharpie poskytuje pohodlné funkce vazby správně definován architektury prostřednictvím `-framework` možnost.

Například vytvoření vazby [Adobe Creative SDK Framework](https://creativesdk.adobe.com/downloads.html) pro iOS je jednoduchý:

<pre>$ <b>sharpie bind \
    -framework AdobeCreativeSDKFoundation.framework \
    -sdk iphoneos8.1</b></pre>

V některých případech bude rozhraní zadejte **Info.plist** který určuje, na které SDK rozhraní by měl být zkompilovány. Pokud tyto informace existuje a žádné explicitní `-sdk` možnost je předán, cíl Sharpie bude odvození z rozhraní framework **Info.plist** (buď `DTSDKName` klíč nebo jejich kombinaci `DTPlatformName` a `DTPlatformVersion`klíčů).

`-framework` Možnost neumožňuje explicitní hlavičkových souborů, které mají být předány. Soubor hlaviček zastřešující je zvolen pomocí konvence na základě názvu framework. Pokud nelze nalézt hlavičku zastřešující, cíl Sharpie nebude pokoušet vytvořit vazbu rozhraní a vazby je nutné ručně provést zadáním soubory hlavičky správné zastřešující analyzovat, společně s všechny argumenty framework pro clang (například `-F`možnost Cesta hledání framework).

Pod pokličkou zadání `-framework` je právě zástupce. Následující argumenty vazby jsou stejné jako `-framework` sdružená výše.
Pro zvláštní význam je `-F .` cesty pro hledání framework poskytované clang (poznamenejte si místa a období, které jsou vyžadovány jako součást příkazu).

<pre>$ <b>sharpie bind \
    -sdk iphoneos8.1 \
    AdobeCreativeSDKFoundation.framework/Headers/AdobeCreativeSDKFoundation.h \
    -scope AdobeCreativeSDKFoundation.framework/Headers \
    -c -F .</b></pre>
