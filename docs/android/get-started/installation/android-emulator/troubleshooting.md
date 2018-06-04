---
title: Odstraňování problémů s instalací emulátoru
description: Tento článek vysvětluje, jak diagnostikovat a vyřešit problémy, které mohou nastat při pomocí Správce zařízení Android.
ms.prod: xamarin
ms.assetid: 4F053CC9-9378-47CB-8002-978A6558C4D0
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 06/01/2018
ms.openlocfilehash: 4cbd7d7626d114856d6c68fe7bc9feb7c2a5abc2
ms.sourcegitcommit: a7febc19102209b21e0696256c324f366faa444e
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 06/04/2018
ms.locfileid: "34733922"
---
# <a name="troubleshooting-emulator-setup-problems"></a>Odstraňování problémů s instalací emulátoru

_Tento článek vysvětluje, jak diagnostikovat a vyřešit problémy, které mohou nastat při konfiguraci emulátoru systému Android pomocí Správce zařízení Android. Informace o řešení potíží při spuštění emulátoru systému Android, najdete v článku [Google Android emulátoru řešení potíží s](~/android/deploy-test/debugging/android-sdk-emulator/troubleshooting.md)._

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

## <a name="android-sdk-in-non-standard-location"></a>Sady SDK pro Android v nestandardním umístění

SDK pro Android je obvykle nainstalovaný v následujícím umístění:

**C:\\Program Files (x86)\\Android\\android sdk**

Pokud sada SDK není nainstalovaná v tomto umístění, může se tato chyba při spuštění Správce zařízení Android:

![Chyba instance sady Android SDK](troubleshooting-images/win/01-sdk-error.png)

Chcete-li tento problém obejít, postupujte takto:

1. Z plochy, přejděte na **C:\\uživatelé\\*uživatelské jméno*\\data aplikací\\Roaming\\XamarinDeviceManager**:

    ![Umístění souboru protokolu Správce zařízení Android](troubleshooting-images/win/02-log-files.png)

2. Dvojitým kliknutím otevřete jeden ze souborů protokolu a vyhledejte **cesta k souboru konfigurace**. Příklad:

    [![Cesta k souboru konfigurace v souboru protokolu](troubleshooting-images/win/03-config-file-path-sml.png)](troubleshooting-images/win/03-config-file-path.png#lightbox)

3. Přejděte do tohoto umístění a poklikejte na soubor **user.config** ho otevřete. 

4. V **user.config**, vyhledejte **&lt;UserSettings&gt;** elementu a přidejte **AndroidSdkPath** atribut ho. Nastavte tento atribut na cestu nainstalovanou SDK pro Android v počítači a soubor uložte. Například **&lt;UserSettings&gt;** by vypadat třeba takto, pokud byla nainstalována SDK pro Android v **C:\\programy\\Android\\SDK**:
        
    ```xml
    <UserSettings SdkLibLastWriteTimeUtcTicks="636409365200000000" AndroidSdkPath="C:\Programs\Android\SDK" />
    ```

Po provedení této změny na **user.config**, byste měli mít spusťte Správce zařízení Android.

## <a name="snapshot-disables-wifi-on-android-oreo"></a>Snímek zakáže Wi-Fi na Android Oreo

Pokud máte AVD nakonfigurovaný pro Android Oreo simulované Wi-Fi přístup, restartování AVD po snímku může způsobit Wi-Fi přístup zakázat.

Chcete-li vyřešit tento problém

1. Vyberte AVD ve Správci zařízení Android.

2. V nabídce Další možnosti, klikněte na tlačítko **odhalit v Průzkumníku**.

3. Přejděte na **snímky > default_boot**.

4. Odstranit **snapshot.pb** souboru:

    ![Umístění souboru snapshot.pb](troubleshooting-images/win/05-delete-snapshot.png)

5. Restartujte AVD. 

Po provedení těchto změn AVD restartuje ve stavu, který umožňuje Wi-Fi znovu fungovat.

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

## <a name="snapshot-disables-wifi-on-android-oreo"></a>Snímek zakáže Wi-Fi na Android Oreo

Pokud máte AVD nakonfigurovaný pro Android Oreo simulované Wi-Fi přístup, restartování AVD po snímku může způsobit Wi-Fi přístup zakázat.

Chcete-li vyřešit tento problém

1. Vyberte AVD ve Správci zařízení Android.

2. V nabídce Další možnosti, klikněte na tlačítko **odhalit v hledání**.

3. Přejděte na **snímky > default_boot**.

4. Odstranit **snapshot.pb** souboru:

    [![Umístění souboru snapshot.pb](troubleshooting-images/mac/02-delete-snapshot-sml.png)](troubleshooting-images/mac/02-delete-snapshot.png#lightbox)

5. Restartujte AVD. 

Po provedení těchto změn AVD restartuje ve stavu, který umožňuje Wi-Fi znovu fungovat.


-----

## <a name="generating-a-bug-report"></a>Generování sestavy chyb

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Pokud narazíte na potíže s Android Správce zařízení které nelze vyřešit pomocí výše uvedených tipy k řešení potíží, kliknutím pravým tlačítkem myši na záhlaví okna a výběrem prosím soubor sestavy chyb **Generovat sestavy chyb**:

[![Umístění položky nabídky pro vyplnění sestavy chyb](troubleshooting-images/win/04-bug-report-sml.png)](troubleshooting-images/win/04-bug-report.png#lightbox)


# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

Pokud narazíte na potíže s Android Správce zařízení které nelze vyřešit pomocí výše uvedených tipy k řešení potíží, kliknutím na prosím soubor sestavy chyb **pomoci > Generovat sestavy chyb**:

[![Umístění položky nabídky pro vyplnění sestavy chyb](troubleshooting-images/mac/01-bug-report-sml.png)](troubleshooting-images/mac/01-bug-report.png#lightbox)

-----
