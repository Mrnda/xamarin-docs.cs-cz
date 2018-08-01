---
title: Řešení potíží s sešity ke Xamarinu v Androidu
description: Tento dokument obsahuje tipy pro řešení potíží pro práci se sešity ke Xamarinu v Androidu. Popisuje podporu emulátorů, sešity, které se nenačte a dalšími tématy.
ms.prod: xamarin
ms.assetid: F1BD293B-4EB7-4C18-A699-718AB2844DFB
author: topgenorth
ms.author: toopge
ms.date: 03/30/2017
ms.openlocfilehash: b0333e1a40570374ee6218b7a848d2dd1c06b872
ms.sourcegitcommit: aa9b9b203ab4cd6a6b4fd51e27d865e2abf582c1
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/30/2018
ms.locfileid: "39351714"
---
# <a name="troubleshooting-xamarin-workbooks-on-android"></a>Řešení potíží s sešity ke Xamarinu v Androidu

## <a name="emulator-support"></a>Podporu emulátorů

Spustit Android sešitu, musí být k dispozici pro použití emulátoru Androidu. Fyzické zařízení se systémem Android nejsou podporovány.

Pokud počítač podporuje doporučujeme emulátor Googlu spolu s modulem HAXM.
Pokud máte na počítači povolená technologie Hyper-V, přejděte k dispozici místo toho se Android emulátor sady Visual Studio.

Musíte mít emulátoru, na kterém běží Android 5.0 nebo novější. Emulátory ARM nejsou podporovány. Použití `x86` nebo `x86_64` jenom zařízení.

Přečtěte si prosím [naši dokumentaci o nastavení Android emulátory] [ android-emu] Pokud nejste obeznámeni s procesem.

> [!NOTE]
> Sešity 1.1 a starší se zkuste (a selhání!) pomocí emulátorů ARM v případě, že jsou k dispozici. Chcete-li vyřešit tento, spuštění x86 emulátor podle vašeho výběru před otevřením či vytvořením sešitu s Androidem. Sešity budou vždy nejdřív připojit k běžící emulátor, tak dlouho, dokud je kompatibilní.

## <a name="workbooks-wont-load"></a>Sešity se nenačte.

### <a name="workbook-window-spins-forever-never-loads-windows"></a>Okna sešitu přede forever, nikdy zatížení (Windows)

Nejprve zkontrolujte, zda vaše emulátor plně funkční přístup k síti při testování všechny weby ve webovém prohlížeči v emulátoru.

### <a name="visual-studio-android-emulator-cannot-connect-to-the-internet"></a>Android emulátor sady Visual Studio se nemůže připojit k Internetu

Pokud vaše emulátor nemá přístup k síti, budete muset vyřešit váš síťový přepínač technologie Hyper-V pomocí těchto kroků. Při přepínání mezi sítě Wi-Fi často budete muset pravidelně opakujte tento postup:

0. **Ujistěte se, že všechny kritické síťové operace jsou dokončeny, protože to může dočasně odpojit Windows z Internetu.**
1. Zavřít emulátorů.
2. Otevřít `Hyper-V Manager`.
3. V části `Actions`, otevřete `Virtual Switch Manager...`.
4. Odstraňte všechny virtuální přepínače.
5. Klikněte na tlačítko `OK`.
6. Spusťte VS Android Emulator. Budete pravděpodobně výzva pro opětovné vytvoření přepínače virtuální sítě.
7. Otestujte, že VS emulátoru Androidu browser přístup k Internetu.

[android-emu]: https://developer.xamarin.com/guides/android/deployment,_testing,_and_metrics/debug-on-emulator/


## <a name="related-links"></a>Související odkazy

- [Zasílání zpráv o chybách](~/tools/workbooks/install.md#reporting-bugs)
