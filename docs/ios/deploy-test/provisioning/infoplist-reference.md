---
title: Odkaz na Info.plist
ms.prod: xamarin
ms.assetid: 944DFDB5-ADBA-4D6E-984C-5AEC19A1CC57
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 01/18/2017
ms.openlocfilehash: 8ad2c5e8137dede71d9858aa144e440d984c1a75
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/04/2018
---
# <a name="infoplist-reference"></a>Odkaz na Info.plist

Další informace o práci s Info.Plist klíče naleznete [práce zabezpečení a ochrana osobních údajů](~/ios/app-fundamentals/security-privacy.md) průvodce. 

## <a name="location"></a>Umístění 

Přístup k umístění uživatele také vyžaduje změny Info.plist. Musí být nastavená týkající se data o umístění následující klíče: 

* **NSLocationWhenInUseUsageDescription** – když přistupujete umístění uživatele, když jsou interakci s vaší aplikací. 
* **NSLocationAlwaysUsageDescription** – když vaše aplikace přístup k umístění uživatele na pozadí.

## <a name="photos"></a>Fotografie 

NSPhotoLibraryUsageDescription  

## <a name="contacts"></a>Kontakty 

NSContactsUsageDescription 

## <a name="calendar-data"></a>Kalendářní data 
    
NSCalendarsUsageDescription 

## <a name="reminder-data"></a>Připomenutí dat 
    
NSRemindersUsageDescription 

## <a name="bluetooth-peripherals"></a>Periferní zařízení Bluetooth 
    
NSBluetoothPeripheralUsageDescription 

## <a name="microphone"></a>Mikrofon 

NSMicrophoneUsageDescription 

## <a name="camera"></a>Fotoaparát 
    
NSCameraUsageDescription 

## <a name="reading-healthkit"></a>Čtení HealthKit  

NSHealthShareUsageDescription 

NSHealthUpdateUsageDescription 

## <a name="background-modes"></a>Režimy pozadí 
    
UIBackgroundModes 

## <a name="homekit"></a>HomeKit 

NSHomeKitUsageDescription 


## <a name="related-links"></a>Související odkazy

- [Apple referenční příručka.](https://developer.apple.com/library/content/documentation/General/Reference/InfoPlistKeyReference/Articles/iPhoneOSKeys.html#//apple_ref/doc/uid/TP40009252-SW10)
