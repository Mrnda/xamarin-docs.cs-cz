---
title: Přehrávání zvuku s AVAudioPlayer v Xamarin.Mac
description: Tento dokument popisuje, jak přehrát zvuk s AVAudioPlayer v Xamarin.Mac aplikaci. Popisuje AVAudioPlayer na vysoké úrovni a odkazy na další dokumentaci, která zde popsány podrobněji.
ms.prod: xamarin
ms.assetid: 4A683A94-F75D-4EAF-8497-E9443653250B
ms.technology: xamarin-mac
author: bradumbaugh
ms.author: brumbaug
ms.date: 10/19/2016
ms.openlocfilehash: 9e5b9ec43189999f8a0aee29eb50221b494e2133
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 06/05/2018
ms.locfileid: "34791852"
---
# <a name="playing-sound-with-avaudioplayer-in-xamarinmac"></a>Přehrávání zvuku s AVAudioPlayer v Xamarin.Mac

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
