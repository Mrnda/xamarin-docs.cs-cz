---
title: Přehrávání zvuku s Avaudioplayerem v Xamarin pro tvOS
description: Tento článek ukazuje, jak používat pomocnou třídu pro řízení přehrávání zvuku Avaudioplayerem pomocí aplikace pro Xamarin.iOS.
ms.prod: xamarin
ms.assetid: E0305572-DC64-48BB-BD97-0A5096E6CA04
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/16/2017
ms.openlocfilehash: 2ce1d4b8564ef9599581aabd6a72ba3af12ec251
ms.sourcegitcommit: b56b3f906d2c05a3f1be219ef41be8b79e519b8e
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/25/2018
ms.locfileid: "39241336"
---
# <a name="playing-sound-in-tvos-with-avaudioplayer-in-xamarin"></a>Přehrávání zvuku s Avaudioplayerem v Xamarin pro tvOS

## <a name="about-the-avaudioplayer"></a>O Avaudioplayerem

`AVAudioPlayer` Se používá k přehrávání zvuku data z paměti nebo ze souboru. Apple doporučuje použití této třídy přehrávání zvuku ve vaší aplikaci, pokud provádíte sítě streamování nebo vyžadují zvuku vstupně-výstupní operace s nízkou latencí.

Můžete použít `AVAudioPlayer` můžete provádět následující:

- Přehrávání zvuků dobu trvání s volitelné opakování ve smyčce.
- Přehrát více zvuky ve stejnou dobu volitelné synchronizace.
- Ovládací prvek objem, rychlost přehrávání a stereo umístění pro každý přehrání zvuku.
- Podpora funkce, jako je rychle převinout vpřed a vzad.
- Získáte data z měření úroveň přehrávání.

`AVAudioPlayer` podporuje zvuky v jakékoli zvukový formát zadaný, jako zařízení s iOS, tvOS a OS X `.aif`, `.wav` nebo `.mp3`.

## <a name="playing-sounds-in-tvos"></a>Přehrávání zvuků v tvOS

Protože tvOS podporuje stejné třídy zvuku nástrojů jako iOS, najdete v našich iOS [přehrávání zvuku s Avaudioplayerem](https://github.com/xamarin/recipes/tree/master/Recipes/ios/media/sound/avaudioplayer) dokumentaci úplné podrobnosti o přehrávání zvuku v aplikaci s Xamarin.tvOS.



## <a name="related-links"></a>Související odkazy

- [Odkaz na Avaudioplayerem](https://developer.apple.com/library/ios/documentation/AVFoundation/Reference/AVAudioPlayerClassReference/)
