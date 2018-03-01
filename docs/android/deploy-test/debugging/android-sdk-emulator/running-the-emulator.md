---
title: "Spuštěný emulátoru sady SDK pro Android"
description: "Postup ladění aplikace pomocí emulátoru Android SDK"
ms.topic: article
ms.prod: xamarin
ms.assetid: AEA165A4-D81A-411B-91DF-2DED2EED27B5
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/16/2018
ms.openlocfilehash: 2995d9126617a767013ed1f5cb808f22ce0fd2da
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 02/27/2018
---
# <a name="running-the-android-sdk-emulator"></a>Spuštěný emulátoru sady SDK pro Android

V tomto průvodci se dozvíte, jak ke spuštění virtuálního zařízení v Android emulátoru sady SDK pro ladění a testování vaší aplikace.

## <a name="using-a-pre-configured-virtual-device"></a>Pomocí předem nakonfigurovaných virtuálního zařízení

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Visual Studio obsahuje předem nakonfigurovaná virtuální zařízení, které se zobrazují v rozevírací nabídce zařízení. Například na následujícím snímku obrazovky Visual Studio 2017, jsou k dispozici několik předem konfigurovaný virtuální zařízení:

-   **VisualStudio\_android-23\_arm\_phone**

-   **VisualStudio\_android-23\_arm\_tablet**

-   **VisualStudio\_android-23\_x86\_phone** 

-   **VisualStudio\_android-23\_x86\_tablet** 

[ ![Virtuální zařízení](running-the-emulator-images/win/01-virtual-devices-sml.png)](running-the-emulator-images/win/01-virtual-devices.png)

Obvykle byste vybrali **Visual Studio\_android 23\_x86\_phone** virtuální zařízení k testování a ladění telefonní aplikace. Pokud jeden z těchto předem konfigurovaný virtuální zařízení splňuje vaše požadavky (tj, odpovídá cíl své aplikace úroveň rozhraní API), přejděte k [spouštění v emulátoru](#launching) zahájíte používající vaši aplikaci v emulátoru. (Pokud ještě nejste obeznámeni s úrovně rozhraní API systému Android, najdete v části [Principy Android API úrovně](~/android/app-fundamentals/android-api-levels.md).)

Pokud projekt Xamarin.Android používá úroveň cílové rozhraní, která není kompatibilní s k dispozici virtuální zařízení, v rozevírací nabídce se zobrazí seznam nepoužitelná virtuální zařízení v části **nepodporované zařízení**. Například následující projekt má cílové rozhraní, nastavte na **Android 7.1 cukrovinkách typu nugát (25 rozhraní API)**, který není kompatibilní s **Android 6.0** virtuální zařízení, které jsou k dispozici ve výchozím nastavení:

[ ![Nekompatibilní virtuálního zařízení](running-the-emulator-images/win/02-incompatible-level-sml.png)](running-the-emulator-images/win/02-incompatible-level.png)

Můžete kliknout na **změnit cíl Android minimální** chcete změnit projekt, je minimální verze Android tak, aby odpovídala úroveň rozhraní API k dispozici virtuální zařízení. Alternativně můžete použít **správce emulátoru Android** k vytvoření nové virtuální zařízení, které podporují vaše cílové rozhraní API na úrovni, jak je popsáno později v [konfiguraci virtuálních zařízení](#virtualdevice). Než budete moct nakonfigurovat virtuální zařízení pro novou úroveň rozhraní API, je nutné nejprve nainstalovat odpovídající bitové kopie systému pro tuto úroveň rozhraní API &ndash; to je vysvětleno v následující části.

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

Visual Studio pro Mac obsahuje předem nakonfigurovaná virtuální zařízení, které se zobrazují v rozevírací nabídce zařízení. Například na následujícím snímku obrazovky jsou k dispozici dva předem konfigurovaný virtuální zařízení:

-   **Android\_Accelerated\_x86**

-   **Android\_ARMv7a**

[ ![Virtuální zařízení](running-the-emulator-images/mac/01-virtual-devices-sml.png)](running-the-emulator-images/mac/01-virtual-devices.png)

Obvykle byste vybrali **Android\_Accelerated\_x86** virtuální zařízení k testování a ladění telefonní aplikace. Pokud to předem nakonfigurované virtuální zařízení splňuje vaše požadavky (tj, odpovídá cíl své aplikace úroveň rozhraní API), přejděte k [spouštění v emulátoru](#launching) zahájíte používající vaši aplikaci v emulátoru. (Pokud ještě nejste obeznámeni s úrovně rozhraní API systému Android, najdete v části [Principy Android API úrovně](~/android/app-fundamentals/android-api-levels.md).)

-----

## <a name="creating-custom-virtual-devices"></a>Vytvoření vlastního virtuálního zařízení

Pokud chcete vytvořit vlastní virtuální zařízení, musíte použít Správce zařízení Xamarin Android nebo starší verze správce emulátorů Google, která je součástí sady SDK pro Android. Další informace o vytváření a přizpůsobení virtuální zařízení najdete v tématu [Správce zařízení Xamarin Android](~/android/get-started/installation/android-emulator/xamarin-device-manager.md).
Pokud byste radši chtěli použít starší verze správce emulátorů Google, přečtěte si téma [správce emulátorů Google](~/android/get-started/installation/android-emulator/google-emulator-manager.md).

Všimněte si, že pokud vyvíjíte pro Android 8.0 Oreo, je třeba použít Správce zařízení Xamarin Android.

<a name="launching" />

## <a name="launching-the-emulator"></a>Spouštění v emulátoru

V horní části rozhraní IDE, je rozevírací nabídky, které je možné vybrat **ladění** nebo **verze** režimu. Výběr **ladění** připojí ladicí program na proces aplikace běží uvnitř emulátoru. 

Po výběru virtuálního zařízení z rozevírací nabídky zařízení, vyberte buď **ladění** nebo **verze** režimu, klikněte **přehrání** tlačítko spustit aplikaci:

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![Režimy ladění a vydání, tlačítko Přehrát akci](running-the-emulator-images/win/17-debug-release-sml.png)](running-the-emulator-images/win/17-debug-release.png)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

[![Režimy ladění a vydání, tlačítko Přehrát akci](running-the-emulator-images/mac/16-debug-release-sml.png)](running-the-emulator-images/mac/16-debug-release.png)

-----

Po spuštění emulátoru systému Android, bude Xamarin.Android nasazení aplikace na emulátoru. Na emulátoru běží aplikace s bitovou kopií nakonfigurované virtuální zařízení. Níže je zobrazena snímku obrazovky s příkladem emulátoru Android SDK (emulátoru běží prázdnou aplikaci volat **Moje aplikace**):

![Emulátor s prázdnou aplikaci](running-the-emulator-images/emulator-running.png)

Emulátor, může být ponecháno systémem; není potřeba ho vypnout a restartovat při každém spuštění aplikace. Při prvním spuštění aplikace Xamarin.Android v emulátoru, Xamarin.Android sdílené runtime pro úroveň směrována rozhraní API je nainstalován, společně s aplikací. Instalace modulu CLR může chvíli trvat, takže počkejte. Instalace modulu runtime probíhá pouze v případě, že první aplikace Xamarin.Android je nasazen na emulátoru &ndash; následné nasazení je rychlejší, protože jenom aplikaci se zkopíruje do emulátoru.

Další informace o použití emulátoru Android SDK naleznete v následujících tématech Android Developer:

-   [Navigace na obrazovce](https://developer.android.com/studio/run/emulator.html#navigate)

-   [Provádění základních úloh v emulátoru](https://developer.android.com/studio/run/emulator.html#tasks)

-   [Práce s rozšířené ovládací prvky, nastavení a Nápověda](https://developer.android.com/studio/run/emulator.html#extended)

