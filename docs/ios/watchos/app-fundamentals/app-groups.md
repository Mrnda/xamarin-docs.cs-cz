---
title: Práce s watchOS skupiny aplikací v Xamarinu
description: Tento dokument popisuje skupiny aplikací a jejich používání v aplikaci watchOS. Popisuje, jak nakonfigurovat skupinu aplikací, zřizování požadavky a důležité informace o Entitlements.plist a nasazení.
ms.prod: xamarin
ms.technology: xamarin-ios
ms.assetid: 6968606B-C287-424F-A321-2492E12BC0BB
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/17/2017
ms.openlocfilehash: 5736b25af3993e2da794422a1a6f040461532497
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 06/05/2018
ms.locfileid: "34790676"
---
# <a name="working-with-watchos-app-groups-in-xamarin"></a>Práce s watchOS skupiny aplikací v Xamarinu


Skupinu aplikace umožňuje různé aplikace (nebo aplikace a její rozšíření) pro přístup k umístění úložiště sdílený soubor. Skupiny aplikací lze použít pro data, jako jsou:

- Apple Watch [nastavení](~/ios/watchos/app-fundamentals/settings.md).
- Sdílené [NSUserDefaults](~/ios/watchos/app-fundamentals/parent-app.md#nsuserdefaults).
- Sdílené [soubory](~/ios/watchos/app-fundamentals/parent-app.md#files).

## <a name="configure-an-app-group"></a>Konfigurace skupiny aplikací

Sdílené umístění je konfigurován pomocí [aplikace skupiny](https://developer.apple.com/library/ios/documentation/Miscellaneous/Reference/EntitlementKeyReference/Chapters/EnablingAppSandbox.html#//apple_ref/doc/uid/TP40011195-CH4-SW19), která je nakonfigurována v **certifikáty, identifikátory a profily** části na [iOS Dev Center](https://developer.apple.com/devcenter/ios/). Tato hodnota musí také odkazovat v každém projektu **Entitlements.plist**.

### <a name="provisioning"></a>Zřizování

Identifikátor, který je obvykle vaše ID sady prostředků bude mít skupina aplikací s `group.` předponu. Můžeme použít například ID sady `com.xamarin.WatchSettings` a skupině aplikace `group.com.xamarin.WatchSettings`.

[![](app-groups-images/app-group-sml.png "Pomocí ID sady com.xamarin.WatchSettings a group.com.xamarin.WatchSettings skupiny aplikací")](app-groups-images/app-group.png#lightbox)

### <a name="entitlementsplist"></a>Entitlements.plist

A také konfigurace profilu pro zřizování, **povolit skupiny aplikací** v **Entitlements.plist** a zadejte ID, které jste zvolili:

[![](app-groups-images/entitlements-sml.png "Nakonfigurujte plist a zadejte ID")](app-groups-images/entitlements.png#lightbox)


### <a name="deployment"></a>Nasazení

Zkontrolujte konfiguraci správně v aplikaci skupiny vaše [nasazení](~/ios/watchos/deploy-test/index.md#App_Groups) zřizování.


Další informace najdete v tématu [možnosti skupiny aplikace](~/ios/deploy-test/provisioning/capabilities/app-groups-capabilities.md) dokumentaci.


## <a name="related-links"></a>Související odkazy

- [Společnosti Apple sdílení dat s obsahující aplikaci](https://developer.apple.com/library/ios/documentation/General/Conceptual/ExtensibilityPG/ExtensionScenarios.html)
- [Doc skupiny aplikace společnosti Apple](https://developer.apple.com/library/ios/documentation/Miscellaneous/Reference/EntitlementKeyReference/Chapters/EnablingAppSandbox.html#//apple_ref/doc/uid/TP40011195-CH4-SW19)
