---
title: "Správce emulátorů Google"
description: "Jak vytvořit a spravovat pomocí Správce emulátor Google Android virtuální zařízení."
ms.topic: article
ms.prod: xamarin
ms.assetid: 0C0BBEC0-C84A-4558-B905-4EF81FCD62F9
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 12/22/2017
ms.openlocfilehash: f275ff6c7d3e6eeec5eb3878cc39633d70238f66
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 02/27/2018
---
# <a name="google-emulator-manager"></a>Správce emulátorů Google

Po ověření, zda je povoleno hardwarová akcelerace (jak je popsáno v [Android emulátoru hardwarovou akceleraci](~/android/get-started/installation/android-emulator/hardware-acceleration.md)), dalším krokem je vytvoření virtuálního zařízení používat pro testování a ladění aplikace. Můžete použít starší verze správce emulátorů Google (také označované jako *Manager virtuální zařízení Android (AVD)*) Chcete-li vytvořit virtuální zařízení za účelem použití emulátoru Android SDK.

> [!NOTE]
> **Poznámka:** při cílení Android 8.0 Oreo, je nutné použít [Správce zařízení Xamarin Android](~/android/get-started/installation/android-emulator/xamarin-device-manager.md) vytvořit a nakonfigurovat virtuální zařízení.

<a name="sysimg" />

## <a name="installing-system-images"></a>Instalace bitové kopie systému

V závislosti na tom, které rozhraní API systému Android úrovni, kterou chcete zacílit musíte stáhnout a nainstalovat bitové kopie systému specifické úrovně rozhraní API, které jsou používány emulátoru sady SDK pro Android. Pro každou úroveň rozhraní API systému Android jsou sady **x86** bitové kopie systému, které budete muset stáhnout a nainstalovat pro vytvoření virtuálního zařízení.

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

K instalaci bitové kopie nezbytné systému, spustit Android SDK Manager (**nástroje > Android > Android SDK Manager**) a přejděte na úrovni rozhraní API, které chcete podporovat. Pro každou úroveň rozhraní API povolte zaškrtnutí políčka vedle následující bitové kopie systému:

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

K instalaci bitové kopie nezbytné systému, spustit Android SDK Manager (**nástroje > SDK Manager**) a přejděte na úrovni rozhraní API, které chcete podporovat. Pro každou úroveň rozhraní API povolte zaškrtnutí políčka vedle následující bitové kopie systému:

-----

-   **Bitové kopie systému Intel x86 Atom**
-   **Bitové kopie systému Intel rozhraní API Google x86 Atom**

Bitovou kopii systému pozdější přidá rozhraní Google API (například Google Maps API) k virtuálnímu zařízení. 

Na následujícím snímku obrazovky **Intel x86 Atom** bitové kopie se nainstalují, aby bylo možné vytvořit virtuální zařízení se systémem Android 6.0:

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![Výběr bitové kopie systému Android 6.0 x86 emulátoru systému Android](google-emulator-manager-images/win/03-select-x86-images-sml.png)](google-emulator-manager-images/win/03-select-x86-images.png)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

[![Výběr bitové kopie systému Android 6.0 x86 emulátoru systému Android](google-emulator-manager-images/mac/02-select-x86-images-sml.png)](google-emulator-manager-images/mac/02-select-x86-images.png)

-----

Pokud vyvíjíte aplikace 64-bit, nainstalujte následující bitové kopie systému:

-   **Bitové kopie systému Intel x86 Atom_64**
-   **Bitové kopie systému Intel rozhraní API Google x86 Atom_64**

Tyto bitové kopie systému 64-bit můžete spouštět aplikace pro 32-bit; ale 32bitovou verzi **bitovou kopii systému Intel x86 Atom** mírně rychleji spustí v emulátoru Android SDK.

Pokud vyvíjíte aplikace pro Android nosit, nainstalujte následující bitové kopie systému:

-   **Bitové kopie systému Android opotřebení Intel x86 Atom**
-   **Bitové kopie systému Intel rozhraní API Google x86 Atom**

Když jsou nainstalovány tyto bitové kopie systému, můžete vytvořit **x86**– na základě virtuální zařízení se systémem Android výběrem odpovídající úroveň rozhraní API a CPU/ABI volby při konfiguraci virtuálních zařízení (to je popsána dále).


<a name="virtualdevice" />

## <a name="configuring-virtual-devices"></a>Konfigurace virtuálního zařízení

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Virtuální zařízení je nakonfigurováno pomocí **správce emulátoru Android** (také označuje jako _Android správce virtuálního zařízení_ nebo _správce AVD_). Chcete-li spustit Správce emulátoru Android ze sady Visual Studio, klikněte na tlačítko **správce emulátoru Android** ikonu na panelu nástrojů:

[ ![Umístění ikony AVD](google-emulator-manager-images/win/04-avd-icon-sml.png)](google-emulator-manager-images/win/04-avd-icon.png)

Můžete také spustit Správce emulátoru Android z řádku nabídek výběrem **nástroje > Android > Správce emulátoru Android**:

[![Umístění položky nabídky Správce emulátoru Android](google-emulator-manager-images/win/05-avd-manager-menu-item-sml.png)](google-emulator-manager-images/win/05-avd-manager-menu-item.png)

**Manager virtuální zařízení Android (AVD)** dialogové okno zobrazí seznam existující virtuální zařízení se systémem Android:

[![Android správce virtuálního zařízení](google-emulator-manager-images/win/06-virtual-device-manager-sml.png)](google-emulator-manager-images/win/06-virtual-device-manager.png)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

Virtuální zařízení je nakonfigurováno pomocí **správce emulátoru Android** (také označuje jako _Android správce virtuálního zařízení_ nebo _správce AVD_). 

Chcete-li spustit Správce emulátoru Android z řádku nabídek výběrem **nástroje > Správce emulátorů Google**:

[![Umístění položky nabídky Správce emulátoru Android](google-emulator-manager-images/mac/03-avd-manager-menu-item-sml.png)](google-emulator-manager-images/mac/03-avd-manager-menu-item.png)

**Manager virtuální zařízení Android (AVD)** dialogové okno zobrazí seznam existující virtuální zařízení se systémem Android:

[![Android správce virtuálního zařízení](google-emulator-manager-images/mac/05-virtual-device-manager-sml.png)](google-emulator-manager-images/mac/05-virtual-device-manager.png)

-----

Můžete vytvořit nové bitové kopie virtuálního zařízení s vlastnostmi různých zařízení a úrovně rozhraní API &ndash; v další části vysvětluje, jak vytvořit vlastní zařízení definice a virtuální zařízení.

<a name="custom-def" />

### <a name="creating-a-custom-device-definition"></a>Vytváření vlastních zařízení definice

Chcete-li vytvořit definici vlastní zařízení, klikněte na tlačítko **vytvořit...**  v **správce virtuální zařízení Android (AVD)**. Tím se otevře **vytvořit novou virtuální zařízení Android (AVD)** dialogové okno:

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![Definice vlastního zařízení podle Nexus 6](google-emulator-manager-images/win/07-custom-device-sml.png)](google-emulator-manager-images/win/07-custom-device.png)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

[![Definice vlastního zařízení podle Nexus 6](google-emulator-manager-images/mac/06-custom-device-sml.png)](google-emulator-manager-images/mac/06-custom-device.png)

-----

V tomto dialogovém okně nakonfigurujte následující možnosti:

-   **Název AVD** &ndash; jedinečný název pro definici vaše zařízení. Snímek obrazovky příkladu výše, je nastavit název **MyNexus**. Všimněte si, že název AVD nesmí obsahovat mezery &ndash; **OK** tlačítko bude zakázáno, pokud se pokusíte použít v názvu AVD mezery.

-   **Zařízení** &ndash; vyberte profil, kterou chcete emulovat (například **Nexus 5** nebo **Nexus 6**).

-   **Cíl** &ndash; vyberte úroveň rozhraní API systému Android pro virtuální zařízení. Toto nastavení musí být větší než nebo rovna minimální verze Android vaší aplikace.

-   **CPU/ABI** &ndash; vyberte **Atom Intel rozhraní API Google (x86)** tak, aby rozhraní Google API bude k dispozici v definici vaše zařízení.

-   **Vzhledu** &ndash; vyberte vzhled virtuálního zařízení. Na snímku obrazovky příkladu výše **HVGA** vzhledu je vybraná (emulátoru snímek na konci tohoto článku je příkladem **HVGA** vzhledu).

-   **Možnosti paměti** &ndash; obvykle výchozí nastavení paměti RAM je příliš vysoká. a, v systému Windows, způsobí, že upozornění: **emulace RAM větší, než může selhat 768M**. Pro většinu uživatelů doporučujeme nastavení paměti RAM na 768MB (jak je uvedeno výše uvedený snímek obrazovky). Velké hodnoty paměti RAM může zpomalit emulátor.

-   **Použijte hostitele GPU** &ndash; tato možnost způsobí, že emulátoru používat hostitele počítače grafické zpracování jednotky (GPU) k provádění operací grafiky. Doporučujeme vám, že povolíte tuto možnost, chcete-li dále zvýšit výkon emulátor. Další informace o **emulace možnosti** tématu najdete v tématu [co jsou snímku a jeho použití hostitele GPU emulace používá možnosti pro?](https://android.stackexchange.com/questions/51739/what-is-snapshot-and-use-host-gpu-emulation-options-for)


Zbývající možnosti může být ponechány na jejich výchozí nastavení. Až budete připravení, klikněte na tlačítko **OK** k vytvoření nové virtuální zařízení. V dialogovém okně Další jsou podrobné výsledky novou konfiguraci virtuálního zařízení:

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

![Dialogové okno výsledků po vytvoření nového AVD](google-emulator-manager-images/win/08-create-results.png)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

![Dialogové okno výsledků po vytvoření nového AVD](google-emulator-manager-images/mac/07-create-results.png)

-----

Podrobné vysvětlení vlastnosti konfigurace, které jsou uvedené v tomto dialogovém okně najdete v tématu [vlastnosti profilu hardwaru](https://developer.android.com/studio/run/managing-avds.html#hpproperties).
Po kliknutí na tlačítko **OK**, nová konfigurace zařízení se zobrazí v seznamu existující virtuální zařízení se systémem Android. Na následujícím snímku obrazovky **MyNexus** byl přidán do seznamu:

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![MyNexus přidán do seznamu zařízení](google-emulator-manager-images/win/09-added-to-list-sml.png)](google-emulator-manager-images/win/09-added-to-list.png)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

[![MyNexus přidán do seznamu zařízení](google-emulator-manager-images/mac/08-added-to-list-sml.png)](google-emulator-manager-images/mac/08-added-to-list.png)

-----

Nové vlastní virtuální zařízení je také přidat do rozevírací nabídky zařízení:

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![MyNexus přidány do rozevírací nabídky zařízení](google-emulator-manager-images/win/10-available-custom-device-sml.png)](google-emulator-manager-images/win/10-available-custom-device.png)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

[![MyNexus přidány do rozevírací nabídky zařízení](google-emulator-manager-images/mac/09-available-custom-device-sml.png)](google-emulator-manager-images/mac/09-available-custom-device.png)

-----


<a name="cloning" />

### <a name="cloning-a-device-definition"></a>Klonování definici zařízení

Je možné vybrat existující definice zařízení a *klon* ji k vytvoření nové definice vlastní zařízení. Toto je strategii je dobré použijte, pokud existující definice zařízení vyžadující pouze několik menší nastavení podle svých potřeb. **Zařízení definice** ve **Manager virtuální zařízení Android (AVD)** obsahuje seznam všech definic dostupného zařízení:

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![Seznam definic dostupného zařízení](google-emulator-manager-images/win/11-device-definitions-sml.png)](google-emulator-manager-images/win/11-device-definitions.png)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

[![Seznam definic dostupného zařízení](google-emulator-manager-images/mac/10-device-definitions-sml.png)](google-emulator-manager-images/mac/10-device-definitions.png)

-----

Předem nakonfigurovaná zařízení v tomto seznamu nemůže být upraven &ndash; lze upravovat pouze uživatele vytvořit virtuální zařízení. Je možné k odvození nové definice zařízení z definice předem nakonfigurovaná zařízení výběrem definici zařízení a kliknutím na **klon**. Například výběr **Nexus 5** definice a kliknutím na **klon** uvede následující dialogové okno:

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![Dialogové okno zařízení klonování](google-emulator-manager-images/win/12-clone-device-sml.png)](google-emulator-manager-images/win/12-clone-device.png)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

[![Dialogové okno zařízení klonování](google-emulator-manager-images/mac/11-clone-device-sml.png)](google-emulator-manager-images/mac/11-clone-device.png)

-----

Na snímku obrazovky další název se změní na **Nexus 5 vlastní** a parametry zařízení jsou upravit tak, aby vytvořte novou definici vlastní zařízení:

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

![Vlastní Nexus 5 AVD](google-emulator-manager-images/win/13-custom-nexus.png)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

[![Vlastní Nexus 5 AVD](google-emulator-manager-images/mac/12-custom-nexus-sml.png)](google-emulator-manager-images/mac/12-custom-nexus.png)

-----

Kliknutím na tlačítko **klon zařízení** vytvoří nové definice zařízení, která se teď zobrazí v **zařízení definice** seznamu:

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![Vlastní nexus 5 se zobrazí jako nové definice zařízení uživatele](google-emulator-manager-images/win/14-new-definition-sml.png)](google-emulator-manager-images/win/14-new-definition.png)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

[![Vlastní nexus 5 se zobrazí jako nové definice zařízení uživatele](google-emulator-manager-images/mac/13-new-definition-sml.png)](google-emulator-manager-images/mac/13-new-definition.png)

-----

Všimněte si, že každý definice vytvořené uživatelem zařízení se zobrazí se zelenou ikonou, jako v příkladu nahoře. Toto nové definice zařízení lze použít k vytvoření nové AVD definici výběrem a kliknutím na **vytvořit AVD**. Zobrazí se **vytvořit novou virtuální zařízení Android (AVD)** dialogové okno. V následujícím příkladu, název **AVD\_pro\_Nexus\_5\_vlastní** se automaticky vygeneroval pro nové virtuální zařízení:

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![Vytvoření AVD z definice zařízení Nexus 5 vlastní uživatele](google-emulator-manager-images/win/15-create-avd-sml.png)](google-emulator-manager-images/win/15-create-avd.png)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

[![Vytvoření AVD z definice zařízení Nexus 5 vlastní uživatele](google-emulator-manager-images/mac/14-create-avd-sml.png)](google-emulator-manager-images/mac/14-create-avd.png)

-----

Po **OK** po kliknutí na vlastní konfigurace zařízení se zobrazí v seznamu existující virtuální zařízení se systémem Android. Kromě toho je přidán do rozevírací nabídky zařízení:

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![Přidat nové vlastní AVD do zařízení rozevírací nabídky](google-emulator-manager-images/win/16-new-avd-sml.png)](google-emulator-manager-images/win/16-new-avd.png)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

[![Přidat nové vlastní AVD do zařízení rozevírací nabídky](google-emulator-manager-images/mac/15-new-avd-sml.png)](google-emulator-manager-images/mac/15-new-avd.png)

-----

