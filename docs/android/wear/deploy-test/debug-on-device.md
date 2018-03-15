---
title: "Ladění na opotřebení ze zařízení"
description: "Tento článek vysvětluje postup ladění aplikace Xamarin.Android opotřebení na opotřebení ze zařízení."
ms.topic: article
ms.prod: xamarin
ms.assetid: 01668E4B-BB83-4C26-B23A-F788173FB823
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/16/2018
ms.openlocfilehash: c12764610d0fd9834914b8114818b2ccd7d7def0
ms.sourcegitcommit: 8e722d72c5d1384889f70adb26c5675544897b1f
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 03/15/2018
---
# <a name="debug-on-a-wear-device"></a>Ladění na opotřebení ze zařízení

_Tento článek vysvětluje postup ladění aplikace Xamarin.Android opotřebení na opotřebení ze zařízení._


## <a name="overview"></a>Přehled

Pokud máte zařízení s Androidem nosit například Android Smartwatch nosit, můžete spustit aplikaci na zařízení místo použití emulátoru. (Pokud jste se ještě v tématu obeznámeni s procesem nasazení a spuštění aplikace Android nosit [nosit Hello,](~/android/wear/get-started/hello-wear.md).)

## <a name="prepare-the-wear-device"></a>Připravte opotřebení ze zařízení:

Pokud chcete povolit ladění na zařízení Android nosit pomocí následujících kroků:

1.  Otevřete **nastavení** nabídky na zařízení Android nosit.

2.  Přejděte do dolní části nabídky a klepněte na **o**.

3.  Klepněte na číslo sestavení 7 časy.

4.  Na **nastavení** nabídky, klepněte na **možnosti pro vývojáře**.

5.  Potvrďte, že **ADB ladění** je povoleno.


## <a name="debugging-over-usb"></a>Ladění přes USB

Pokud zařízení opotřebení má USB port, můžete připojit opotřebení ze zařízení do počítače, nasazení do ní a spustit nebo ladění aplikace, jako při práci telefon se systémem Android (Další informace najdete v tématu [ladění na zařízení](~/android/deploy-test/debugging/debug-on-device.md)).


## <a name="debugging-over-bluetooth"></a>Ladění přes Bluetooth

Pokud zařízení opotřebení nemá USB port, můžete nasadit aplikace do zařízení a opotřebením motoru přes Bluetooth směrování výstupu ladění aplikace pro Android phone, která je připojena k vašemu počítači. 

### <a name="prepare-your-phone"></a>Příprava vašeho telefonu

Příprava vašeho telefonu pro připojení Bluetooth na zařízení a opotřebením motoru použijte následující kroky: 

1.  Pokud jste tak již neučinili, nastavte telefon pro Xamarin.Android vývoj jak je popsáno v [nastavit zařízení pro vývoj](~/android/get-started/installation/set-up-device-for-development.md).

2.  Stáhněte a nainstalujte bezplatnou [Android nosit](https://play.google.com/store/apps/details?id=com.google.android.wearable.app) aplikaci z obchodu Google Play.

### <a name="connect-the-device"></a>Připojte zařízení

Pomocí následujících kroků připojte opotřebení ze zařízení na váš telefon:

1.  V telefonu, bude fungovat jako zprostředkující Bluetooth (výše nakonfigurované), spusťte aplikaci Android nosit. 

2.  Klepněte **nastavení** ikonu.

3.  Povolit **ladění přes Bluetooth**. Měli byste vidět následující stav zobrazené na obrazovce aplikace Android nosit:

        Host: disconnected
        Target: connected

4.  Připojte telefon k počítači přes USB. V počítači zadejte následující příkazy:

    ```shell
    adb forward tcp:4444 localabstract:/adb-hub
    adb connect 127.0.0.1:4444
    ```

    Pokud port 4444 není k dispozici, můžete použít jakýkoli dostupný port ke které máte přístup. 

    **Poznámka:**: Pokud je restartovat Visual Studio nebo Visual Studio pro Mac, je nutné spustit tyto příkazy znovu k nastavení připojení k zařízení a opotřebením motoru.

5.  Po opotřebení ze zařízení zobrazí výzva, potvrďte, že jsou umožňuje **ADB ladění**. V aplikaci Android nosit byste měli vidět stav změnit na:

        Host: connected
        Target: connected

6.  Po dokončení výše uvedené kroky systémem `adb devices` zobrazuje stav telefonu a zařízení Android nosit:

        List of devices attached
        127.0.0.1:4444    device
        019ad61df0a69399  device

V tomto okamžiku můžete nasadit aplikace do zařízení a opotřebením motoru.

<a name="screenshots" />

### <a name="taking-screenshots"></a>Pořízení snímků obrazovky

Snímek obrazovky opotřebení ze zařízení, můžete provést tak, že zadáte následující příkaz: 

```shell
adb -s 127.0.0.1:4444 shell screencap -p /sdcard/DCIM/screencap.png
```

Zkopírujte na snímku obrazovky v počítači tak, že zadáte následující příkaz:

```shell
adb -s 127.0.0.1:4444 pull /sdcard/DCIM/screencap.png
```

Odstraňte snímek obrazovky na zařízení tak, že zadáte následující příkaz:

```shell
adb -s 127.0.0.1:4444 shell rm /sdcard/DCIM/screencap.png
```


### <a name="uninstalling-an-app"></a>Odinstalace aplikace

Můžete odinstalovat aplikaci z opotřebení ze zařízení tak, že zadáte následující příkaz:

```shell
adb -s 127.0.0.1:4444 uninstall <package name>
```

Například odebrat aplikaci s názvem balíčku `com.xamarin.weartest`, zadejte následující příkaz:

```shell
adb -s 127.0.0.1:4444 uninstall com.xamarin.weartest
```

Další informace o ladění zařízení Android nosit přes Bluetooth najdete v tématu [ladění přes Bluetooth](https://developer.android.com/training/wearables/apps/bt-debugging.html).


## <a name="debugging-a-wear-app-with-a-companion-phone-app"></a>Ladění aplikace a opotřebením motoru a doprovodné telefonní aplikaci

Aplikace pro Android opotřebení ze spojených s doprovodné telefon se systémem Android aplikace pro distribuci na webu Google Play (Další informace najdete v tématu [práce s balení](~/android/wear/deploy-test/packaging.md)). Však stále vyvíjíte aplikace a opotřebením motoru a jeho doprovodné aplikace samostatně. Při uvolnění aplikace přes obchod Google Play se aplikace opotřebení ze spojených s aplikací doprovodné a automaticky nainstaluje Pokud je to možné.

Chcete-li ladit aplikace a opotřebením motoru a doprovodné aplikaci: 

1.  Sestavení a nasazení aplikace doprovodné na telefonu.

2.  Klikněte pravým tlačítkem na projekt a opotřebením motoru a nastavte jej jako výchozí spuštění projekt.

3.  Nasazení projektu opotřebení wearable zařízení.

4.  Spuštění a ladění opotřebení aplikace na zařízení.

 
## <a name="summary"></a>Souhrn

Tento článek vysvětlit, jak nakonfigurovat zařízení se systémem Android nosit pro ladění opotřebení ze sady Visual Studio prostřednictvím Bluetooth a postup ladění aplikace a opotřebením motoru a doprovodné telefonní aplikaci. Tu taky běžné ladění tipy k ladění aplikace opotřebení přes Bluetooth.
