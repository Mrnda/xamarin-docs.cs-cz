---
title: Řešení potíží s Xamarin sešity v systému Android
description: Tento dokument obsahuje tipy k řešení potíží pro práci se sešity Xamarin pro Android. Popisuje podporu emulátoru, sešity, které nenačte a další témata.
ms.prod: xamarin
ms.assetid: F1BD293B-4EB7-4C18-A699-718AB2844DFB
author: topgenorth
ms.author: toopge
ms.openlocfilehash: b01fa5b67f8a7f5577c6598f63f8c38e8fa4cf3c
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 06/05/2018
ms.locfileid: "34793981"
---
# <a name="troubleshooting-xamarin-workbooks-on-android"></a>Řešení potíží s Xamarin sešity v systému Android

## <a name="emulator-support"></a>Podporu emulátoru

Pokud chcete spustit Android sešitu, musí být k dispozici pro použití emulátoru Androidu. Fyzické zařízení se systémem Android nejsou podporovány.

Doporučujeme vám emulátor Google spolu s HAXM, pokud počítač podporuje.
Pokud máte povolené v systému Hyper-V, přejděte k dispozici místo toho se systémem Android emulátor sady Visual Studio.

Musíte mít emulátoru se systémem Android 5.0 nebo novější. Emulátorů ARM nejsou podporovány. Použití `x86` nebo `x86_64` jenom zařízení.

Přečtěte si prosím [naší dokumentaci o nastavení Android emulátorů] [ android-emu] Pokud nejste obeznámeni s procesem.

> [!NOTE]
> Sešity 1.1 a starší se zkuste (a nezdaří!) pro použití ARM emulátorů, pokud jsou k dispozici. Chcete-li vyřešit tento, spusťte x86 emulátoru zvoleného před otevřením nebo vytvoření sešitu aplikace Android. Sešity bude vždy používat pro připojení k spuštěného emulátoru, dokud je kompatibilní.

## <a name="workbooks-wont-load"></a>Nenačte se sešity

### <a name="workbook-window-spins-forever-never-loads-windows"></a>Okno sešitu otáčí navždy, nikdy zatížením (Windows)

Nejprve zkontrolujte, že vaše emulátoru má přístup k síti plně funkční testování žádné web ve webovém prohlížeči je emulátor.

### <a name="visual-studio-android-emulator-cannot-connect-to-the-internet"></a>Visual Studio emulátoru systému Android se nemůže připojit k Internetu

Pokud vaše emulátoru nemá přístup k síti, můžete pomocí následujícího postupu vyřešit váš síťový přepínač technologie Hyper-V. Při přepínání mezi sítě Wi-Fi často musíte pravidelně opakujte tento postup:

0. **Ujistěte se, že všechny kritické síťové operace jsou dokončeny, jak to může dočasně odpojení systému Windows z Internetu.**
1. Zavřete emulátorů.
2. Otevřete `Hyper-V Manager`.
3. V části `Actions`, otevřete `Virtual Switch Manager...`.
4. Odstraňte všechny virtuální přepínače.
5. Klikněte na tlačítko `OK`.
6. Spustíte Android emulátor VS. Pravděpodobně se výzva k opětovnému vytvoření přepínače virtuální sítě.
7. Test VS emulátoru Android prohlížeče přístup k Internetu.

[android-emu]: https://developer.xamarin.com/guides/android/deployment,_testing,_and_metrics/debug-on-emulator/


## <a name="related-links"></a>Související odkazy

- [Zasílání zpráv o chybách](~/tools/workbooks/install.md#reporting-bugs)
