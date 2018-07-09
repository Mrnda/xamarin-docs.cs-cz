---
title: Vytváření vazeb nativních architektur
description: Tento dokument popisuje způsob použití Sharpie cíle společnosti - framework možnost vytvořit vazbu na knihovnu distribuuje jako rozhraní.
ms.prod: xamarin
ms.assetid: 91AE058A-3A1F-41A9-9DE4-4B96880A1869
author: asb3993
ms.author: amburns
ms.date: 01/15/2016
ms.openlocfilehash: ca103ee027597813be51e126aaa05f9aa969af35
ms.sourcegitcommit: ec50c626613f2f9af51a9f4a52781129bcbf3fcb
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/05/2018
ms.locfileid: "37855094"
---
# <a name="binding-native-frameworks"></a>Vytváření vazeb nativních architektur

Někdy se nativní knihovna distribuuje jako [framework](https://developer.apple.com/library/mac/documentation/MacOSX/Conceptual/BPFrameworks/Concepts/WhatAreFrameworks.html). Cíle Sharpie poskytuje pohodlné funkce, která pro vazby správně definované architektur díky `-framework` možnost.

Například vytvoření vazby [Adobe Creative SDK Framework](https://creativesdk.adobe.com/downloads.html) pro iOS je jednoduchý:

<pre>$ <b>sharpie bind \
    -framework AdobeCreativeSDKFoundation.framework \
    -sdk iphoneos8.1</b></pre>

V některých případech bude rozhraní zadat **Info.plist** označující proti které sady SDK rozhraní by měl být zkompilován. Pokud tyto informace existuje a ne explicitní `-sdk` možnost předána, Sharpie cíle se odvodit z rozhraní framework **Info.plist** (buď `DTSDKName` klíč nebo ke kombinaci komponent `DTPlatformName` a `DTPlatformVersion`klíče).

`-framework` Možnost nedovoluje explicitní hlavičkové soubory, které mají být předány. Soubor hlaviček zastřešující zvolena podle konvence na základě názvu rozhraní framework. Pokud hlavičku zastřešující nebyl nalezen, Sharpie cíle se nepokusí vytvořit vazbu rozhraní framework a je nutné ručně provést vazbu tím, že poskytuje správný zastřešující soubory záhlaví analyzovat spolu s argumentů framework pro clang (například `-F`možnost cestu hledání framework).

Pod pokličkou určení `-framework` je jenom místní. Následující argumenty vazby jsou stejné jako `-framework` Zkrácený tvar vlastností výše.
Zvláštní význam je `-F .` cesta pro vyhledání rozhraní poskytnuté clang (Všimněte si, místa a období, které jsou požadovány jako součást příkazu).

<pre>$ <b>sharpie bind \
    -sdk iphoneos8.1 \
    AdobeCreativeSDKFoundation.framework/Headers/AdobeCreativeSDKFoundation.h \
    -scope AdobeCreativeSDKFoundation.framework/Headers \
    -c -F .</b></pre>

## <a name="related-links"></a>Související odkazy

- [Xamarin University kurz: Vytvoření knihovny vazeb Objective-C](https://university.xamarin.com/classes/track/all#building-an-objective-c-bindings-library)
- [Xamarin University kurz: Vytvoření knihovny vazeb Objective-C pomocí cíle Sharpie](https://university.xamarin.com/classes/track/all#build-an-objective-c-bindings-library-with-objective-sharpie)

