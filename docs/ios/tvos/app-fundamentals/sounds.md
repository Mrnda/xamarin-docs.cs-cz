---
title: Přehrávání zvuku s AVAudioPlayer
description: Tento článek ukazuje, jak používat pomocná třída pro ovládání přehrávání zvuku použití AVAudioPlayer.
ms.prod: xamarin
ms.assetid: E0305572-DC64-48BB-BD97-0A5096E6CA04
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/16/2017
ms.openlocfilehash: c50aea9c4c35e91c2baa98c94db2fd7c61136d69
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/04/2018
---
# <a name="playing-sound-with-avaudioplayer"></a>Přehrávání zvuku s AVAudioPlayer

_Tento článek ukazuje, jak používat pomocná třída pro ovládání přehrávání zvuku použití AVAudioPlayer._

## <a name="about-the-avaudioplayer"></a>O AVAudioPlayer

`AVAudioPlayer` Se používá k přehrávání zvuku data z paměti nebo souboru. Apple doporučuje použití této třídy přehrávání zvuku ve vaší aplikaci, pokud dělají sítě streamování nebo vyžadovat zvuk vstupně-výstupních operací s nízkou latencí.

Můžete použít `AVAudioPlayer` proveďte následující:

- Přehrávání zvuků žádné trvání s volitelné opakování ve smyčce.
- Přehrávání zvuků více současně s volitelné synchronizace.
- Řízení svazku, přehrávání rychlost a stereo umístění pro každý přehrání zvuku.
- Podporovat funkce, jako je rychloposuv vpřed nebo rewind.
- Získejte přehrávání úroveň data měření.

`AVAudioPlayer` podporuje zvuků v libovolném Zvuk formátu poskytované tvOS, iOS a OS X, jako `.aif`, `.wav` nebo `.mp3`.

## <a name="playing-sounds-in-tvos"></a>Přehrávání zvuků v tvOS

Protože tvOS podporuje stejné třídy zvuk nástrojů jako iOS, najdete v tématu naše iOS [přehrávání zvuku s AVAudioPlayer](http://developer.xamarin.com/recipes/ios/media/sound/avaudioplayer/) dokumentace pro všechny podrobnosti o přehrávání zvuku ve Xamarin.tvOS aplikaci.



## <a name="related-links"></a>Související odkazy

- [Odkaz na AVAudioPlayer](https://developer.apple.com/library/ios/documentation/AVFoundation/Reference/AVAudioPlayerClassReference/)
