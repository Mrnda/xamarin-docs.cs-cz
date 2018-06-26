---
title: Ladění na emulátoru systému Android
description: Tato příručka vysvětluje, jak spouštět a ladění aplikací v sadě Visual Studio pomocí emulátoru systému Android.
ms.prod: xamarin
ms.assetid: AEA165A4-D81A-411B-91DF-2DED2EED27B5
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 06/22/2018
ms.openlocfilehash: 9e0eade7a2e033838f78f24270ec2bf9d4abc171
ms.sourcegitcommit: 26033c087f49873243751deded8037d2da701655
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 06/25/2018
ms.locfileid: "36935775"
---
# <a name="debugging-on-the-android-emulator"></a>Ladění na emulátoru systému Android

_V tomto průvodci se dozvíte, jak ke spuštění virtuálního zařízení v Android emulátoru pro ladění a testování vaší aplikace._

## <a name="overview"></a>Přehled

Emulátoru systému Android (nainstalován jako součást **pro vývoj mobilních řešení s .NET** zatížení), můžete spustit v různých konfiguracích k simulaci různých zařízení se systémem Android. Každé z nich tyto konfigurace je vytvořen jako _virtuální zařízení_. V tomto průvodci se dozvíte, jak spusťte emulátor ze sady Visual Studio a spusťte aplikaci v virtuálního zařízení. Informace o konfiguraci emulátoru systému Android a vytvoření nové virtuální zařízení najdete v tématu [Android – nastavení emulátoru](~/android/get-started/installation/android-emulator/index.md).


## <a name="using-a-pre-configured-virtual-device"></a>Pomocí předem nakonfigurovaných virtuálního zařízení

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Visual Studio obsahuje předem nakonfigurovaná virtuální zařízení, které se zobrazují v rozevírací nabídce zařízení. Například na následujícím snímku obrazovky Visual Studio 2017, jsou k dispozici několik předem konfigurovaný virtuální zařízení:

-   **VisualStudio\_android-23\_arm\_phone**

-   **VisualStudio\_android-23\_arm\_tablet**

-   **VisualStudio\_android-23\_x86\_phone** 

-   **VisualStudio\_android-23\_x86\_tablet** 

[![Virtuální zařízení](debug-on-emulator-images/win/01-virtual-devices-sml.png)](debug-on-emulator-images/win/01-virtual-devices.png#lightbox)

Obvykle byste vybrali **Visual Studio\_android 23\_x86\_phone** virtuální zařízení k testování a ladění telefonní aplikace. Pokud jeden z těchto předem konfigurovaný virtuální zařízení splňuje vaše požadavky (tj, odpovídá cíl své aplikace úroveň rozhraní API), přejděte k [spouštění v emulátoru](#launching) zahájíte používající vaši aplikaci v emulátoru. (Pokud ještě nejste obeznámeni s úrovně rozhraní API systému Android, najdete v části [Principy Android API úrovně](~/android/app-fundamentals/android-api-levels.md).)

Pokud projekt Xamarin.Android používá úroveň cílové rozhraní, která není kompatibilní s k dispozici virtuální zařízení, v rozevírací nabídce uvádí nepoužitelná virtuální zařízení v části **nepodporované zařízení**. Například následující projekt má cílové rozhraní, nastavte na **Android 7.1 cukrovinkách typu nugát (25 rozhraní API)**, který není kompatibilní s **Android 6.0** virtuální zařízení, které jsou uvedeny v tomto příkladu:

[![Nekompatibilní virtuálního zařízení](debug-on-emulator-images/win/02-incompatible-level-sml.png)](debug-on-emulator-images/win/02-incompatible-level.png#lightbox)

Můžete kliknout na **změnit cíl Android minimální** chcete změnit projekt, je minimální verze Android tak, aby odpovídala úroveň rozhraní API k dispozici virtuální zařízení. Alternativně můžete použít [Správce zařízení Android](~/android/get-started/installation/android-emulator/device-manager.md) k vytvoření nové virtuální zařízení, které podporují vaše cílové rozhraní API úrovně.
Než budete moct nakonfigurovat virtuální zařízení pro novou úroveň rozhraní API, je nutné nejprve nainstalovat odpovídající bitové kopie systému pro tuto úroveň rozhraní API (v tématu [nastavení Android SDK pro Xamarin.Android](~/android/get-started/installation/android-sdk.md)).

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

Visual Studio pro Mac obsahuje předem nakonfigurovaná virtuální zařízení, které se zobrazují v rozevírací nabídce zařízení. Například na následujícím snímku obrazovky jsou k dispozici dva předem konfigurovaný virtuální zařízení:

-   **Android\_Accelerated\_x86**

-   **Android\_ARMv7a**

[![Virtuální zařízení](debug-on-emulator-images/mac/01-virtual-devices-sml.png)](debug-on-emulator-images/mac/01-virtual-devices.png#lightbox)

Obvykle byste vybrali **Android\_Accelerated\_x86** virtuální zařízení k testování a ladění telefonní aplikace. Pokud to předem nakonfigurované virtuální zařízení splňuje vaše požadavky (tj, odpovídá cíl své aplikace úroveň rozhraní API), přejděte k [spouštění v emulátoru](#launching) zahájíte používající vaši aplikaci v emulátoru. (Pokud ještě nejste obeznámeni s úrovně rozhraní API systému Android, najdete v části [Principy Android API úrovně](~/android/app-fundamentals/android-api-levels.md).)

-----

## <a name="editing-virtual-devices"></a>Úpravy virtuálního zařízení

Chcete-li upravit virtuální zařízení (nebo vytvořit nové), je nutné použít [Správce zařízení Android](~/android/get-started/installation/android-emulator/device-manager.md).


<a name="launching" />

## <a name="launching-the-emulator"></a>Spouštění v emulátoru

V horní části sady Visual Studio je rozevírací nabídky, které je možné vybrat **ladění** nebo **verze** režimu. Výběr **ladění** způsobí, že ladicího programu připojit k procesu aplikace běží uvnitř emulátoru po spuštění aplikace. Výběr **verze** režimu zakáže ladicí program (ale pořád se dá spustit příkazy protokolu aplikace a použití pro ladění). Po výběru virtuálního zařízení z rozevírací nabídky zařízení, vyberte buď **ladění** nebo **verze** režimu a potom klepněte na tlačítko Přehrát a spusťte aplikaci:

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![Režimy ladění a vydání, tlačítko Přehrát akci](debug-on-emulator-images/win/17-debug-release-sml.png)](debug-on-emulator-images/win/17-debug-release.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

[![Režimy ladění a vydání, tlačítko Přehrát akci](debug-on-emulator-images/mac/16-debug-release-sml.png)](debug-on-emulator-images/mac/16-debug-release.png#lightbox)

-----

Po spuštění emulátoru, bude Xamarin.Android nasazení aplikace na emulátoru. Na emulátoru běží aplikace s bitovou kopií nakonfigurované virtuální zařízení. Níže je zobrazena snímku obrazovky s příkladem emulátoru Android. V tomto příkladu je emulátor běží prázdnou aplikaci volat **Moje aplikace**:

![Emulátor s prázdnou aplikaci](debug-on-emulator-images/emulator-running.png)

Emulátor může ponechat spuštěnou: není nutné ho vypnout a počkejte ho chcete restartovat app pokaždé, když se spustí. Při prvním spuštění aplikace Xamarin.Android v emulátoru, Xamarin.Android sdílené runtime pro úroveň směrována rozhraní API je nainstalován, společně s aplikací. Instalace modulu CLR může chvíli trvat, takže počkejte. Instalace modulu runtime probíhá pouze v případě, že první aplikace Xamarin.Android je nasazen na emulátoru &ndash; následné nasazení je rychlejší, protože jenom aplikaci se zkopíruje do emulátoru.

<a name="quick-boot" />

## <a name="quick-boot"></a>Rychlé spuštění

Novější verze emulátoru systému Android zahrnují funkci _rychlé spuštění_ , spustí emulátoru v jenom pár sekund. Při zavření v emulátoru, pořídí snímek stavu virtuální zařízení tak, aby ho lze rychle obnovit z tohoto stavu po restartování.
Chcete-li tuto funkci používat, budete potřebovat následující:

-   Emulátoru systému Android verze 27.0.2 nebo novější
-   Verzi systému Android SDK Tools 26.1.1 nebo novější

Pokud jsou nainstalovány výše uvedené verze emulátoru a nástroje sady SDK, je funkce Rychlé spuštění povolena ve výchozím nastavení. 

První studený start virtuálního zařízení využívá místní bez zlepšování rychlosti, protože snímek ještě nebyla vytvořena:

![Snímek obrazovky cold spouštěcí](debug-on-emulator-images/cold-boot.png)

Při ukončení mimo emulátoru rychlé spuštění ukládá stav emulátoru snímek:

![Ukládání stavu na vypnutí](debug-on-emulator-images/saving-state.png)

Spustí další virtuální zařízení je mnohem rychlejší, protože emulátor jednoduše obnoví stav, kdy jste zavřeli emulátor.

![Načítání stavu na restartování](debug-on-emulator-images/loading-state.png)


## <a name="troubleshooting"></a>Poradce při potížích

Tipy a řešení běžných problémů s emulátoru najdete v tématu [Android řešení potíží s emulátoru](~/android/get-started/installation/android-emulator/troubleshooting.md).


## <a name="summary"></a>Souhrn

Tato příručka vysvětluje proces pro konfiguraci emulátoru Android spustit a otestovat aplikace Xamarin.Android. Je popsané kroky pro spouštění v emulátoru pomocí předem nakonfigurovaných virtuální zařízení a je poskytovala kroky pro nasazení aplikace do emulátoru ze sady Visual Studio. 

Další informace o použití emulátoru systému Android najdete v následujících tématech Android Developer:

-   [Navigace na obrazovce](https://developer.android.com/studio/run/emulator.html#navigate)

-   [Provádění základních úloh v emulátoru](https://developer.android.com/studio/run/emulator.html#tasks)

-   [Práce s rozšířené ovládací prvky, nastavení a Nápověda](https://developer.android.com/studio/run/emulator.html#extended)

-   [Spustit Rychlé spuštění pomocí emulátoru](https://developer.android.com/studio/run/emulator#quickboot)
