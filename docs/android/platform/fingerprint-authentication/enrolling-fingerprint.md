---
title: Registrace otisk prstu
description: Jak nastavit až zamykací obrazovka a registrovat otisk prstu na emulátoru nebo zařízení s Androidem.
ms.prod: xamarin
ms.assetid: 52092F63-00EE-4F8B-A49F-65C9CCBA7EF2
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/16/2018
ms.openlocfilehash: 4faee5decb102d17d9a270b96cef4a12fc9dbef4
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/04/2018
---
# <a name="enrolling-a-fingerprint"></a>Registrace otisk prstu

## <a name="enrolling-a-fingerprint-overview"></a>Registrace přehled otisk prstu

Je možné využít otisk prstu ověřování, pokud zařízení již byla nakonfigurována pomocí otisku prstu ověřování aplikace platformy Android. Tato příručka popisuje postup registrace otisk prstu na emulátoru nebo zařízení s Androidem. Emulátorů nemají skutečné hardwarové kontrolovat otisk prstu, ale je možné k simulaci kontrolu otisk prstu za pomoci most ladění Android (popsaný níže).  Tato příručka popisuje postup povolení zámek obrazovky na zařízení s Androidem a jejich registrace otisk prstu pro ověřování.

## <a name="requirements"></a>Požadavky

K registraci otisk prstu, musí mít zařízení se systémem Android nebo emulátor systémem úroveň rozhraní API 23 (Android 6.0).

Použití nástroje Android ladění most (ADB) vyžaduje znalost na příkazovém řádku a **adb** spustitelný soubor musí být v CESTĚ Bash, prostředí PowerShell nebo příkazový řádek prostředí.

## <a name="configuring-a-screen-lock-and-enrolling-a-fingerprint"></a>Konfigurace zamykací obrazovka a registraci otisk prstu 

Nastavení zámku obrazovky, proveďte následující kroky:

1. Přejděte na **Nastavení > zabezpečení**a vyberte **zamykací obrazovka**:

    ![Umístění výběru zámek obrazovky na obrazovce zabezpečení](enrolling-fingerprint-images/testing-01.png)

2. Na další obrazovce, která se zobrazí umožní vyberte a nakonfigurujte jednu z metod zabezpečení zámek obrazovky: 

    ![Vyberte prstem vzor, kódu PIN nebo heslo](enrolling-fingerprint-images/testing-02.png)

   Vyberte a dokončení jedné z metod zámek obrazovky k dispozici.

3. Po nakonfigurování screenlock vrátit do **Nastavení > zabezpečení** a vyberte **otisk prstu**:

    ![Umístění výběru otisku prstu na obrazovce zabezpečení](enrolling-fingerprint-images/testing-03.png)

4. Odtud postupujte podle pořadí přidat otisk prstu na zařízení:

    [![Pořadí snímky obrazovky pro přidání do zařízení otisk prstu](enrolling-fingerprint-images/testing-04-sml.png)](enrolling-fingerprint-images/testing-04.png#lightbox)

5. V poslední obrazovka zobrazí se výzva k umístit na skeneru otisk prstu: 

    ![Obrazovka, která vás vyzve k umístí prstu senzoru](enrolling-fingerprint-images/testing-05.png)

    Pokud používáte zařízení se systémem Android, tím dotykové ovládání prstem na skeneru dokončíte proces. 
    
    
### <a name="simulating-a-fingerprint-scan-on-the-emulator"></a>Simulaci kontrolu otisk prstu v emulátoru

V Android emulátoru je možné k simulaci kontrolu otisk prstu pomocí most ladění Android. Při spuštění OS X relaci Terminálové při v systému Windows spusťte příkazového řádku nebo relaci prostředí Powershell a spusťte `adb`:

```shell
$ adb -e emu finger touch 1
```

Hodnota **1** je _prstem\_id_ pro prstu kontrolovanou "". Je jedinečný celé číslo, které přiřadíte pro každý virtuální otisků prstů. V budoucnu, kdy aplikace běží můžete spustit tento stejný ADB příkaz pokaždé, když na emulátoru zobrazí výzvu pro otisk prstu, můžete spustit `adb` příkazů a předejte ji _prstem\_id_ k simulaci kontroly otisk prstu .

Po dokončení kontroly otisk prstu Android oznámit, že byl přidán otisk prstu:  

![Obrazovka zobrazení otisk prstu přidat!](enrolling-fingerprint-images/testing-06.png)

## <a name="summary"></a>Souhrn 

Tato příručka zahrnutých nastavení zamykací obrazovka a registrace otisk prstu na zařízení se systémem Android nebo v emulátoru Androidu. 

