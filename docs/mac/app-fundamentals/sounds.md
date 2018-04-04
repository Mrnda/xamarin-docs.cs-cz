---
title: Přehrávání zvuku s AVAudioPlayer
description: Tento článek ukazuje, jak používat pomocná třída pro ovládání přehrávání zvuku použití AVAudioPlayer.
ms.prod: xamarin
ms.assetid: 4A683A94-F75D-4EAF-8497-E9443653250B
ms.technology: xamarin-mac
author: bradumbaugh
ms.author: brumbaug
ms.date: 10/19/2016
ms.openlocfilehash: 25e3285a1da1b6a112629001d5412fdd0788c705
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/04/2018
---
# <a name="playing-sound-with-avaudioplayer"></a>Přehrávání zvuku s AVAudioPlayer

_Tento článek ukazuje, jak používat pomocná třída pro ovládání přehrávání zvuku použití AVAudioPlayer._

## <a name="about-the-avaudioplayer"></a>O AVAudioPlayer

`AVAudioPlayer` Třída se používá k přehrávání zvuku data z paměti nebo souboru. Apple doporučuje použití této třídy přehrávání zvuku ve vaší aplikaci, pokud dělají sítě streamování nebo vyžadovat zvuk vstupně-výstupních operací s nízkou latencí.

Můžete použít `AVAudioPlayer` třída proveďte následující:

- Přehrávání zvuků žádné trvání s volitelné opakování ve smyčce.
- Přehrávání zvuků více současně s volitelné synchronizace.
- Řízení svazku, přehrávání rychlost a stereo umístění pro každý přehrání zvuku.
- Podporovat funkce, jako je rychloposuv vpřed nebo rewind.
- Získejte přehrávání úroveň data měření.

`AVAudioPlayer` podporuje zvuků v libovolném Zvuk formátu poskytované iOS, tvOS a systému macOS například AIF, WAV nebo MP3.

## <a name="playing-sounds-in-macos"></a>Přehrávání zvuků v systému macOS

Protože systému macOS podporuje stejné třídy zvuk nástrojů jako iOS, najdete v tématu naše iOS [přehrávání zvuku s AVAudioPlayer](https://developer.xamarin.com/recipes/ios/media/sound/avaudioplayer/) dokumentace pro všechny podrobnosti o přehrávání zvuku ve Xamarin.Mac aplikaci.



## <a name="related-links"></a>Související odkazy

- [Odkaz na AVAudioPlayer](https://developer.apple.com/documentation/avfoundation/avaudioplayer)
