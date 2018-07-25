---
title: Přehrávání zvuku s Avaudioplayerem v Xamarin.Mac.
description: Tento dokument popisuje, jak přehrávání zvuku s Avaudioplayerem v aplikaci pro Xamarin.Mac. Tento článek popisuje Avaudioplayerem na vysoké úrovni a odkazy na další dokumentaci, která popisuje podrobněji.
ms.prod: xamarin
ms.assetid: 4A683A94-F75D-4EAF-8497-E9443653250B
ms.technology: xamarin-mac
author: bradumbaugh
ms.author: brumbaug
ms.date: 10/19/2016
ms.openlocfilehash: 03a0207ce8c742f0ca98ab6c75ed3e7b2f37d09e
ms.sourcegitcommit: b56b3f906d2c05a3f1be219ef41be8b79e519b8e
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/25/2018
ms.locfileid: "39241349"
---
# <a name="playing-sound-with-avaudioplayer-in-xamarinmac"></a>Přehrávání zvuku s Avaudioplayerem v Xamarin.Mac.

## <a name="about-the-avaudioplayer"></a>O Avaudioplayerem

`AVAudioPlayer` Třída se používá k přehrávání zvuku dat z paměti nebo ze souboru. Apple doporučuje použití této třídy přehrávání zvuku ve vaší aplikaci, pokud provádíte sítě streamování nebo vyžadují zvuku vstupně-výstupní operace s nízkou latencí.

Můžete použít `AVAudioPlayer` třídě, aby provedl následující:

- Přehrávání zvuků dobu trvání s volitelné opakování ve smyčce.
- Přehrát více zvuky ve stejnou dobu volitelné synchronizace.
- Ovládací prvek objem, rychlost přehrávání a stereo umístění pro každý přehrání zvuku.
- Podpora funkce, jako je rychle převinout vpřed a vzad.
- Získáte data z měření úroveň přehrávání.

`AVAudioPlayer` podporuje zvuky v žádné zvukové formátu poskytovaném rutinami pro iOS, tvOS a macOS, jako je například AIF, WAV nebo MP3.

## <a name="playing-sounds-in-macos"></a>Přehrávání zvuků v systému macOS

Protože macOS podporuje stejné třídy zvuku nástrojů jako iOS, najdete v našich iOS [přehrávání zvuku s Avaudioplayerem](https://github.com/xamarin/recipes/tree/master/Recipes/ios/media/sound/avaudioplayer) dokumentaci úplné podrobnosti o přehrávání zvuku ve aplikace Xamarin.Mac.

## <a name="related-links"></a>Související odkazy

- [Odkaz na Avaudioplayerem](https://developer.apple.com/documentation/avfoundation/avaudioplayer)
