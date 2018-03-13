---
title: Xamarin Android Device Manager
description: "Správce zařízení Android Xamarin, momentálně ve verzi preview, nahradí Google starší verze Správce zařízení. Tato příručka vysvětluje, jak vytvořit a nakonfigurovat virtuální zařízení se systémem Android (AVDs), emulovat zařízení se systémem Android pomocí Správce zařízení Xamarin Android. Tato virtuální zařízení můžete použít ke spuštění a testování aplikace bez nutnosti se spoléhat na fyzické zařízení."
ms.topic: article
ms.prod: xamarin
ms.assetid: ECB327F3-FF1C-45CC-9FA6-9C11032BD5EF
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 03/01/2018
ms.openlocfilehash: 447657d6f8509623272f37c48c7aecbdfd4cbaad
ms.sourcegitcommit: 0fdb243b46cf21be47584900805cadcd077121bf
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 03/12/2018
---
# <a name="xamarin-android-device-manager"></a>Xamarin Android Device Manager

_Správce zařízení Android Xamarin, momentálně ve verzi preview, nahradí Google starší verze Správce zařízení. Tato příručka vysvětluje, jak vytvořit a nakonfigurovat virtuální zařízení se systémem Android (AVDs), emulovat zařízení se systémem Android pomocí Správce zařízení Xamarin Android. Tato virtuální zařízení můžete použít ke spuštění a testování aplikace bez nutnosti se spoléhat na fyzické zařízení._

![Momentálně ve verzi preview](~/media/shared/preview.png)

 
## <a name="overview"></a>Přehled

Po ověření, zda je povoleno hardwarová akcelerace (jak je popsáno v [hardwarovou akceleraci](~/android/get-started/installation/android-emulator/hardware-acceleration.md)), dalším krokem je vytvoření virtuálního zařízení používat pro testování a ladění aplikace. Chcete-li vytvořit virtuální zařízení za účelem použití emulátoru Android SDK můžete použít Správce zařízení Xamarin Android.

Proč byste se měli pomocí Správce zařízení Xamarin Android místo [Správce zařízení Google](~/android/get-started/installation/android-emulator/google-emulator-manager.md)?
Od verze nástroje pro Android SDK 26.0.1 Google má odebranou podporu architektury jejich uživatelského rozhraní AVD a sady SDK správce považuje jejich nových nástrojů rozhraní příkazového řádku (rozhraní příkazového řádku). Z důvodu této změny je nutné použít [Xamarin SDK Manager](~/android/get-started/installation/android-sdk.md) a Xamarin Android Správce zařízení při aktualizaci nástroje pro Android SDK 26.0.1 a novější (což je vyžadováno pro vývoj pro Android 8.0 Oreo).


# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Tato příručka vysvětluje, jak nainstalovat a pomocí Správce zařízení Android Xamarin pro Visual Studio v systému Windows (nebo [pro Mac](?tabs=vsmac)):

[![Snímek obrazovky Správce zařízení Android Xamarin na kartě zařízení](xamarin-device-manager-images/win/01-devices-dialog-sml.png)](xamarin-device-manager-images/win/01-devices-dialog.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

Tato příručka vysvětluje, jak nainstalovat a pomocí Správce zařízení Android Xamarin pro Visual Studio pro Mac (nebo [pro systém Windows](?tabs=vswin)):

[![Snímek obrazovky Správce zařízení Android Xamarin na kartě zařízení](xamarin-device-manager-images/mac/01-devices-dialog-sml.png)](xamarin-device-manager-images/mac/01-devices-dialog.png#lightbox)

> [!NOTE]
> Tento průvodce se týká pouze pro Visual Studio for Mac.
Xamarin Studio není kompatibilní s Správce zařízení Android Xamarin.

-----

Můžete vytvořit a nakonfigurovat pomocí Správce zařízení Android Xamarin *virtuální zařízení se systémem Android* (AVDs), spusťte [emulátoru Android SDK](~/android/deploy-test/debugging/android-sdk-emulator/index.md).
Každý AVD je konfigurace aplikace emulátoru, která simuluje fyzického zařízení s Androidem. To umožňuje spuštění a testování vaší aplikace v různých konfiguracích, které simulují různé fyzické zařízení se systémem Android. Správce zařízení Xamarin Android nahrazuje Google samostatné správce AVD (která se už nepoužívá).

V tomto průvodci se dozvíte, jak nainstalovat a spustit Správce zařízení Android. Se dozvíte, jak vytvářet, duplicitní, přizpůsobit a spustit virtuální zařízení. Tento průvodce také vysvětluje, jak konfigurovat vlastnosti pro každý virtuální zařízení (například úroveň rozhraní API, procesoru, paměti a řešení), povolit nebo zakázat simulované senzorů, například zrychlení, GPS, orientaci a senzoru světla a konfigurace typu hardwaru akcelerace používá virtuální zařízení.

## <a name="requirements"></a>Požadavky

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Pomocí Správce zařízení Xamarin Android, budete potřebovat následující:

-   Visual Studio 2017 verze 15,5 nebo novější je povinný. Visual Studio Community edition a vyšší je podporovaná.

-   Xamarin pro Visual Studio verze 4,8 nebo novější. Informace o aktualizaci Xamarin najdete v tématu [změnit kanál aktualizace](https://developer.xamarin.com/recipes/cross-platform/ide/change_updates_channel/).

-   Nejnovější verzi [instalační program Správce zařízení Xamarin](https://go.microsoft.com/fwlink/?linkid=865528) pro systém Windows.

-   **Sady SDK pro Android** &ndash; musí být nainstalována sada Android SDK (najdete v části [Android SDK instalace](~/android/get-started/installation/android-sdk.md)), a verze sady SDK nástroje 26.0 musí být nainstalována, jak je popsáno v následující části. (Pokud již není nainstalována), nainstalujte sady SDK pro Android v následujícím umístění: **C:\\Program Files (x86)\\Android\\android-sdk**.

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

-   Visual Studio pro Mac 7.4 nebo novější.

-   Nejnovější verzi [instalační program Správce zařízení Xamarin](https://go.microsoft.com/fwlink/?linkid=865527) pro systému macOS.

-   **Sady SDK pro Android** &ndash; Android SDK 8.0 (26 rozhraní API) nebo novější musí být nainstalován prostřednictvím sady SDK Manager.

-----

 
## <a name="installing-the-device-manager"></a>Instalace Správce zařízení

Při instalaci Správce zařízení Xamarin Android použijte následující kroky:

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

1. Stažení [instalační program Správce zařízení Xamarin](https://go.microsoft.com/fwlink/?linkid=865528) pro systém Windows.

2. Klikněte dvakrát na **Xamarin.DeviceManager.msi** a postupujte podle pokynů k instalaci: 

    ![Průvodce instalací Správce zařízení Xamarin Android](xamarin-device-manager-images/win/30-installer.png)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

1. Stažení [instalační program Správce zařízení Xamarin](https://go.microsoft.com/fwlink/?linkid=865527) pro systému macOS.

2. Klikněte dvakrát na **AndroidDevices.pkg** a postupujte podle pokynů k instalaci: 

    [![Průvodce instalací Správce zařízení Xamarin Android](xamarin-device-manager-images/mac/30-installer-sml.png)](xamarin-device-manager-images/mac/30-installer.png#lightbox)

-----

 
## <a name="launching-the-device-manager"></a>Spuštění Správce zařízení

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

V sadě Visual Studio 15,6 operací Preview 3 nebo novější, můžete spustit Správce Xamarin Android zařízení z **nástroje** nabídky. Pokud používáte Visual Studio 15,6 operací Preview 3 nebo novější, spusťte Správce zařízení kliknutím **nástroje > Správce emulátoru Android**:

[![Spouštění z nabídky Nástroje](xamarin-device-manager-images/win/04-tools-menu-sml.png)](xamarin-device-manager-images/win/04-tools-menu.png#lightbox)

Pokud používáte starší verze sady Visual Studio, správce Xamarin Android zařízení musí být spuštěn z Windows **spustit** nabídky.

![Správce zařízení Xamarin Android v nabídce start](xamarin-device-manager-images/win/31-start-menu.png)

Klikněte pravým tlačítkem na **Správce zařízení Xamarin Android** a vyberte **více > Spustit jako správce**. Pokud se při spuštění následující chybový dialog, najdete v článku [Poradce při potížích s](#troubleshooting) části Pokyny alternativní řešení:

![Chyba instance sady Android SDK](xamarin-device-manager-images/win/32-sdk-error.png)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

V sadě Visual Studio pro Mac 7.6 Preview 3 (aktuálně v alfa kanálu) nebo novější, můžete spustit Správce zařízení Xamarin Android výběrem **nástroje > Správce emulátorů**:

[![Spouštění z nabídky Nástroje](xamarin-device-manager-images/mac/16-tools-menu-sml.png)](xamarin-device-manager-images/mac/16-tools-menu.png#lightbox)

Pokud používáte starší verze sady Visual Studio pro Mac, správce Xamarin Android zařízení musí být spuštěn nezávisle. Vyhledejte **zařízení se systémem Android** v **aplikace** složku a dvojím kliknutím ho spusťte:

[![Správce zařízení Xamarin Android umístění v nástroji hledání](xamarin-device-manager-images/mac/31-location-in-finder-sml.png)](xamarin-device-manager-images/mac/31-location-in-finder.png#lightbox)


-----

Než budete moct použít Správce zařízení Android, je nutné nainstalovat verzi nástroje pro Android SDK 26.0.0 nebo novější. Pokud Android SDK nástroje 26.0.0 nebo novější není nainstalovaná, zobrazí se toto dialogové okno chyby při spuštění:

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

![Dialogové okno chyby instance Android SDK](xamarin-device-manager-images/win/02-sdk-instance-error.png)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

![Dialogové okno chyby instance Android SDK](xamarin-device-manager-images/mac/02-sdk-instance-error.png)

-----

Pokud se zobrazí toto dialogové okno chyby, klikněte na tlačítko **OK** otevřete Android SDK Manager. V Android SDK Manager, klikněte **nástroje** kartě a nainstalujte **Android SDK Tools 26.0.2** nebo novější, **Android SDK nástrojů platformy 26.0.0** nebo novější, a  **Android SDK – nástroje sestavení 26.0.0** (nebo novější):


# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![Instalace sady SDK pro Android nástrojů 26.0](xamarin-device-manager-images/win/03-sdk-tools-sml.png)](xamarin-device-manager-images/win/03-sdk-tools.png#lightbox)

Po instalaci těchto balíčků, můžete SDK Manager zavřete a znovu spusťte Správce zařízení Android.

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

[![Instalace sady SDK pro Android nástrojů 26.0](xamarin-device-manager-images/mac/03-sdk-tools-sml.png)](xamarin-device-manager-images/mac/03-sdk-tools.png#lightbox)

-----

 
## <a name="main-screen"></a>Hlavní obrazovky

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Jakmile poprvé spustíte Správce zařízení Android, uvede k obrazovce, která se zobrazí všechny aktuálně nakonfigurované virtuální zařízení. Pro každé zařízení **název**, **operačního systému** (Android API úrovně) **procesoru**, **paměti** velikost a rozlišení obrazovky se zobrazí:

[![Seznam nainstalovaných zařízení a jejich parametrů](xamarin-device-manager-images/win/05-installed-list-sml.png)](xamarin-device-manager-images/win/05-installed-list.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

Jakmile poprvé spustíte Správce zařízení Android, uvede k obrazovce, která se zobrazí všechny aktuálně nakonfigurované virtuální zařízení. Pro každé zařízení **název**, **bitovou kopii systému** (Android API úrovně) **procesoru**, **paměti** velikost a rozlišení obrazovky se zobrazí:

[![Seznam nainstalovaných zařízení a jejich parametrů](xamarin-device-manager-images/mac/05-devices-list-sml.png)](xamarin-device-manager-images/mac/05-devices-list.png#lightbox)

-----

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Po kliknutí na tlačítko zařízení v seznamu **spustit** tlačítko se zobrazí na pravé straně. Můžete kliknout na **spustit** tlačítko spusťte emulátor s tímto virtuálním zařízením:

[![Tlačítko Start pro obrázku zařízení](xamarin-device-manager-images/win/06-start-button-sml.png)](xamarin-device-manager-images/win/06-start-button.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

Klikněte na tlačítko **přehrání** tlačítko spusťte emulátor s virtuálním zařízením podle vašeho výběru:
 
[![Tlačítko Start pro obrázku zařízení](xamarin-device-manager-images/mac/06-start-button-sml.png)](xamarin-device-manager-images/mac/06-start-button.png#lightbox)
 
-----

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Po emulátoru začíná vybrané virtuální zařízení, **spustit** tlačítko se změní **Zastavit** tlačítko, které slouží k zastavení emulátoru:

[![Zastavit tlačítko spuštěné zařízení](xamarin-device-manager-images/win/07-stop-button-sml.png)](xamarin-device-manager-images/win/07-stop-button.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

Po emulátoru začíná vybrané virtuální zařízení, **přehrání** tlačítko se změní **Zastavit** tlačítko, které slouží k zastavení emulátoru:
 
[![Zastavit tlačítko spuštěné zařízení](xamarin-device-manager-images/mac/07-stop-button-sml.png)](xamarin-device-manager-images/mac/07-stop-button.png#lightbox)
 
-----

 
### <a name="new-device"></a>Nové zařízení

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Chcete-li vytvořit nové zařízení, klikněte na tlačítko **nový** tlačítko (nachází se v pravé horní části obrazovky):

[![Nové tlačítko pro vytvoření nového zařízení](xamarin-device-manager-images/win/08-new-button-sml.png)](xamarin-device-manager-images/win/08-new-button.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

Chcete-li vytvořit nové zařízení, klikněte na tlačítko **nové zařízení** tlačítko (nachází se v pravé horní části obrazovky):
 
[![Nové tlačítko pro vytvoření nového zařízení](xamarin-device-manager-images/mac/08-new-button-sml.png)](xamarin-device-manager-images/mac/08-new-button.png#lightbox)
 
-----

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Kliknutím na tlačítko **nový** spustí **nové zařízení** obrazovky:

[![Nové zařízení obrazovky Správce zařízení](xamarin-device-manager-images/win/09-new-device-editor-sml.png)](xamarin-device-manager-images/win/09-new-device-editor.png#lightbox)

Konfigurace nového zařízení v **nové zařízení** obrazovky, použijte následující postup:

1. Vyberte fyzické zařízení emulovat kliknutím **zařízení** rozevírací nabídce:

    [![Zařízení rozevírací nabídky](xamarin-device-manager-images/win/10-device-menu-sml.png)](xamarin-device-manager-images/win/10-device-menu.png#lightbox)

2. Vybrat bitovou kopii systému pro použití s tímto virtuálním zařízením kliknutím **bitovou kopii systému** rozevírací nabídce. Tato nabídka uvádí nainstalovaný systém obrázky v části **nainstalovaná**. **Stáhnout** části je uveden seznam bitové kopie systému, které jsou nyní k dispozici ve svém vývojovém počítači, ale mohou být automaticky nainstalovány:

    [![Rozevírací nabídce bitové kopie systému](xamarin-device-manager-images/win/11-system-image-menu-sml.png)](xamarin-device-manager-images/win/11-system-image-menu.png#lightbox)

3. Zadejte nový název zařízení. V následujícím příkladu je název nové zařízení **25 rozhraní API 5 Nexus**:

    [![Pojmenování nové zařízení](xamarin-device-manager-images/win/12-device-name-sml.png)](xamarin-device-manager-images/win/12-device-name.png#lightbox)

4. Upravte všechny vlastnosti, které budete muset upravit. Změnit vlastnosti, najdete v části [vlastnosti profilu](#properties) dál v této příručce.

5. Přidáte žádné další vlastnosti, které je potřeba explicitně nastaven. **Nové zařízení** obrazovky jsou uvedeny pouze vlastnosti nejvíce změnil, ale můžete kliknout na **přidat vlastnost** rozevírací nabídky (v levém dolním) Chcete-li přidat další vlastnosti. V následujícím příkladu `hw.lcd.backlight` se přidává vlastnost:

    [![Přidat vlastnost rozevírací nabídky](xamarin-device-manager-images/win/13-add-property-menu-sml.png)](xamarin-device-manager-images/win/13-add-property-menu.png#lightbox)

6. Klikněte **vytvořit** tlačítko (pravém dolním) k vytvoření nového zařízení:

    ![Vytvoření tlačítka](xamarin-device-manager-images/win/14-create-button.png)

7. Můžete získat **přijetí licence** obrazovky. Klikněte na tlačítko **přijmout** Pokud souhlasíte s licenčními podmínkami:

    ![Licence přijetí obrazovky](xamarin-device-manager-images/win/15-license-acceptance.png)

8. Správce zařízení Android přidá do seznamu nainstalovaných virtuální zařízení s nové zařízení **vytváření** indikátor průběhu, když dojde k zařízení:

    [![Průběh vytváření ukazatele](xamarin-device-manager-images/win/16-creating-the-device-sml.png)](xamarin-device-manager-images/win/16-creating-the-device.png#lightbox)

9. Po dokončení procesu vytváření nového zařízení se zobrazí v seznamu nainstalovaných virtuální zařízení s **spustit** tlačítko připraven ke spuštění:

   [![Nově vytvořený připraven ke spuštění zařízení](xamarin-device-manager-images/win/17-created-device-sml.png)](xamarin-device-manager-images/win/17-created-device.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

Kliknutím na tlačítko **nové zařízení** spustí **nové zařízení** obrazovky:

[![Nové zařízení obrazovky Správce zařízení](xamarin-device-manager-images/mac/09-new-device-editor-sml.png)](xamarin-device-manager-images/mac/09-new-device-editor.png#lightbox)

Použijte následující postup ke konfiguraci nové zařízení na **nové zařízení** obrazovky:

1. Vyberte fyzické zařízení emulovat kliknutím **zařízení** rozevírací nabídce:

    [![Zařízení rozevírací nabídky](xamarin-device-manager-images/mac/10-device-menu-sml.png)](xamarin-device-manager-images/mac/10-device-menu.png#lightbox)

2. Vybrat bitovou kopii systému pro použití s tímto virtuálním zařízením kliknutím **bitovou kopii systému** rozevírací nabídce. Tato nabídka uvádí nainstalovaný systém obrázky v části **nainstalovaná**. **Stáhnout** části (Pokud zobrazené) jsou uvedené bitové kopie systému, které jsou nyní k dispozici ve svém vývojovém počítači, ale mohou být automaticky nainstalovány:

    [![Rozevírací nabídce bitové kopie systému](xamarin-device-manager-images/mac/11-system-image-menu-sml.png)](xamarin-device-manager-images/mac/11-system-image-menu.png#lightbox)

3. Zadejte nový název zařízení. V následujícím příkladu je název nové zařízení **API Nexus 5 X 25**:

    [![Pojmenování nové zařízení](xamarin-device-manager-images/mac/12-device-name-sml.png)](xamarin-device-manager-images/mac/12-device-name.png#lightbox)

4. Upravte všechny vlastnosti, které budete muset upravit. Změnit vlastnosti, najdete v části [vlastnosti profilu](#properties) dál v této příručce.

5. Přidáte žádné další vlastnosti, které je potřeba explicitně nastaven. **Nové zařízení** obrazovky jsou uvedeny pouze vlastnosti nejvíce změnil, ale můžete kliknout na **přidat vlastnost** rozevírací nabídky (v levém dolním) Chcete-li přidat další vlastnosti:

    [![Přidat vlastnost rozevírací nabídky](xamarin-device-manager-images/mac/13-add-property-menu-sml.png)](xamarin-device-manager-images/mac/13-add-property-menu.png#lightbox)

6. Můžete také kliknout na **vlastní** definovat nové vlastnosti pro zařízení:

    ![Vytvoření tlačítka](xamarin-device-manager-images/mac/14-custom-button.png)

7. Klikněte **vytvořit** tlačítko (pravém dolním) k vytvoření nového zařízení:

    ![Vytvoření tlačítka](xamarin-device-manager-images/mac/15-create-button.png)

8. Můžete získat **přijetí licence** obrazovky. Klikněte na tlačítko **přijmout** Pokud souhlasíte s licenčními podmínkami.

9. Když dojde k zařízení, Správce zařízení Android přidá do seznamu zařízení s nové zařízení **vytváření** indikátor průběhu:

    [![Průběh vytváření ukazatele](xamarin-device-manager-images/mac/17-creating-the-device-sml.png)](xamarin-device-manager-images/mac/17-creating-the-device.png#lightbox)

10. Po dokončení procesu vytváření nového zařízení se zobrazí v seznamu zařízení s **přehrání** tlačítko připraven ke spuštění:

   [![Nově vytvořený připraven ke spuštění zařízení](xamarin-device-manager-images/mac/18-created-device-sml.png)](xamarin-device-manager-images/mac/18-created-device.png#lightbox)

-----


<a name="device-edit" />
 
### <a name="edit-device"></a>Upravit zařízení

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Chcete-li upravit stávající virtuální zařízení, vyberte zařízení a klikněte na tlačítko **upravit** tlačítko (umístěný v pravém horním rohu obrazovky):

[![Tlačítko pro úpravy nového zařízení upravit](xamarin-device-manager-images/win/19-edit-button-sml.png)](xamarin-device-manager-images/win/19-edit-button.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

Chcete-li upravit stávající virtuální zařízení, vyberte **další možnosti** rozevírací nabídky (ikona ozubené kolečko) a vyberte **upravit**:
 
[![Upravit výběr nabídky Úpravy nové zařízení](xamarin-device-manager-images/mac/19-edit-button-sml.png)](xamarin-device-manager-images/mac/19-edit-button.png#lightbox)
 
-----

Kliknutím na tlačítko **upravit** spustí Editor zařízení pro vybrané virtuální zařízení:

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![Editor zařízení obrazovky](xamarin-device-manager-images/win/20-device-editor-sml.png)](xamarin-device-manager-images/win/20-device-editor.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)
 
[![Editor zařízení obrazovky](xamarin-device-manager-images/mac/20-device-editor-sml.png)](xamarin-device-manager-images/mac/20-device-editor.png#lightbox)
 
-----

**Zařízení Editor** obrazovky jsou uvedeny vlastnosti virtuálního zařízení z prvního sloupce s odpovídajícími hodnotami každé vlastnosti v druhém sloupci. Když vyberete vlastnost, podrobný popis této vlastnosti se zobrazí na pravé straně.

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Například v následujícím snímku obrazovky `hw.lcd.density` mění se vlastnost z **420** k **240**:

[![Příklad úpravy zařízení](xamarin-device-manager-images/win/21-device-editing-sml.png)](xamarin-device-manager-images/win/21-device-editing.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

Například v následujícím snímku obrazovky `hw.lcd.density` změnit vlastnost z **320** k **240** a `hw.ramSize` vlastnost se změní na **768**:
 
[![Příklad úpravy zařízení](xamarin-device-manager-images/mac/21-device-editing-sml.png)](xamarin-device-manager-images/mac/21-device-editing.png#lightbox)
 
-----

Po provedení změn nezbytné konfigurace, klikněte na tlačítko **Uložit** tlačítko.
Další informace o změně vlastnosti virtuální zařízení najdete v tématu [vlastnosti profilu](#properties) dál v této příručce.


 
### <a name="additional-options"></a>Další možnosti

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Další možnosti pro práci s zařízení jsou dostupné z &hellip; nabídky v horním pravém rohu:

[![Umístění další možnosti nabídky](xamarin-device-manager-images/win/22-overflow-menu-sml.png)](xamarin-device-manager-images/win/22-overflow-menu.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

Další možnosti pro práci se zařízením jsou k dispozici v rozevírací nabídce nachází na levé straně **přehrání** tlačítko:

[![Umístění další možnosti nabídky](xamarin-device-manager-images/mac/22-overflow-menu-sml.png)](xamarin-device-manager-images/mac/22-overflow-menu.png#lightbox)

-----

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

V nabídce Další možnosti obsahuje následující položky:

-   **Duplicitní a upravit** &ndash; duplikuje aktuálně vybrané zařízení a otevře ji v **nové zařízení** obrazovky s jiný jedinečný název. Například výběr **VisualStudio_android 23_x86_phone** a kliknutím na **duplicitní a upravit** připojí k název čítače:

    [![Duplicitní a úpravy obrazovky](xamarin-device-manager-images/win/23-dupe-and-edit-sml.png)](xamarin-device-manager-images/win/23-dupe-and-edit.png#lightbox)

-   **V Průzkumníku odhalit** &ndash; otevře okno Průzkumníka Windows ve složce, která obsahuje soubory pro virtuální zařízení. Například výběr **API Nexus 5 X 25** a kliknutím na **odhalit v Průzkumníku** otevře okno takto:

    [![Výsledky kliknutím na zobrazení v Průzkumníku](xamarin-device-manager-images/win/24-reveal-in-explorer-sml.png)](xamarin-device-manager-images/win/24-reveal-in-explorer.png#lightbox)

-   **Obnovení továrního nastavení** &ndash; vybrané zařízení obnoví výchozí nastavení, aktualizovat je, vymazat všechny uživatele provedené vnitřní stav zařízení byl spuštěn. Tato změna nezmění úpravy, které provedete virtuálního zařízení během vytváření a úpravy. Zobrazí se dialogové okno s připomenutím tento resetování nelze vrátit zpět. Klikněte na tlačítko **vymazat data** potvrďte vynulování.

-   **Odstranit** &ndash; trvale odstraní vybrané virtuální zařízení.
    Dialogové okno se zobrazí se upozornění, že při odstranění zařízení nelze vrátit zpět. Klikněte na tlačítko **odstranit** Pokud jste si jisti, že chcete odstranit zařízení.

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

V nabídce Další možnosti obsahuje následující položky:

-   **Upravit** &ndash; otevře v editoru zařízení aktuálně vybrané zařízení, jak je popsáno výše v [upravit zařízení](#device-edit).

-   **Duplicitní a upravit** &ndash; duplikuje aktuálně vybrané zařízení a otevře ji v **nové zařízení** obrazovky s jiný jedinečný název.
    Například výběr **API Nexus 5 X 25** a kliknutím na **duplicitní a upravit** připojí k název čítače:

    [![Duplicitní a úpravy obrazovky](xamarin-device-manager-images/mac/23-dupe-and-edit-sml.png)](xamarin-device-manager-images/mac/23-dupe-and-edit.png#lightbox)

-   **Odhalit v hledání** &ndash; otevře okno vyhledávací systému macOS ve složce, která obsahuje soubory pro virtuální zařízení. Například výběr **API Nexus 5 X 25** a kliknutím na **odhalit v hledání** otevře okno takto:

    [![Výsledky kliknutím na zobrazení v Průzkumníku](xamarin-device-manager-images/mac/24-reveal-in-finder-sml.png)](xamarin-device-manager-images/mac/24-reveal-in-finder.png#lightbox)

-   **Obnovení továrního nastavení** &ndash; vybrané zařízení obnoví výchozí nastavení, aktualizovat je, vymazat všechny uživatele provedené vnitřní stav zařízení byl spuštěn. Tato změna nezmění úpravy, které provedete virtuálního zařízení během vytváření a úpravy. Zobrazí se dialogové okno s připomenutím tento resetování nelze vrátit zpět. Klikněte na tlačítko **vymazat data** potvrďte vynulování.

-   **Odstranit** &ndash; trvale odstraní vybrané virtuální zařízení.
    Dialogové okno se zobrazí se upozornění, že při odstranění zařízení nelze vrátit zpět." Klikněte na tlačítko **odstranit** Pokud jste si jisti, že chcete odstranit zařízení.

-----

<a name="properties" />
 
## <a name="profile-properties"></a>Vlastnosti profilu

**Nové zařízení** a **zařízení upravit** obrazovky seznam vlastností virtuálního zařízení z prvního sloupce s odpovídajícími hodnotami každé vlastnosti v druhém sloupci. Když vyberete vlastnost, podrobný popis této vlastnosti se zobrazí na pravé straně. Můžete upravit jeho *vlastnosti profilu hardwaru* a jeho *AVD vlastnosti*.
Vlastnosti profilu hardwaru (například `hw.ramSize` a `hw.accelerometer`) popisují fyzické charakteristiky emulované zařízení. Tyto vlastnosti obsahovat velikost obrazovky, množství dostupné paměti RAM, zda je k dispozici zrychlení. Vlastnosti AVD zadejte operaci AVD při spuštění. Můžete například nakonfigurovat AVD vlastností k určení, jak AVD používá vývojovém počítači grafické karty pro vykreslování.

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Vlastnosti můžete změnit pomocí následujících pokynů:

-   Chcete-li změnit vlastnost typu boolean, klikněte na tlačítko zaškrtnutí vpravo od vlastnost typu boolean:

    ![Změna vlastnost typu boolean](xamarin-device-manager-images/win/25-boolean-value.png)

-   Chcete-li změnit *výčtu* (výčtové) vlastnosti, klikněte na šipku dolů vpravo vlastnosti a vyberte novou hodnotu.

    ![Změna vlastnosti výčtu](xamarin-device-manager-images/win/27-enum-value.png)

-   Chcete-li změnit vlastnost řetězec nebo celé číslo, klikněte dvakrát na aktuální nastavení řetězec nebo celé číslo ve sloupci Hodnota a zadejte novou hodnotu.

    ![Změna ve vlastnosti integer.](xamarin-device-manager-images/win/26-integer-value.png)


# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

Vlastnosti můžete změnit pomocí následujících pokynů:

-   Chcete-li změnit vlastnost typu boolean, klikněte na tlačítko zaškrtnutí vpravo od vlastnost typu boolean:

    ![Změna vlastnost typu boolean](xamarin-device-manager-images/mac/25-boolean-value.png)

-   Chcete-li změnit *výčtu* (výčtové) vlastnosti, klikněte na rozevírací nabídku napravo od vlastnosti a vyberte novou hodnotu.

    ![Změna vlastnosti výčtu](xamarin-device-manager-images/mac/27-enum-value.png)

-   Chcete-li změnit vlastnost řetězec nebo celé číslo, klikněte dvakrát na aktuální nastavení řetězec nebo celé číslo ve sloupci Hodnota a zadejte novou hodnotu.

    ![Změna ve vlastnosti integer.](xamarin-device-manager-images/mac/26-integer-value.png)


-----

Poskytuje podrobné vysvětlení vlastností uvedených v následující tabulce **nové zařízení** a **zařízení Editor** obrazovky:

[!include[](~/android/includes/table.html)]

Další informace o těchto vlastnostech najdete v tématu [vlastnosti profilu hardwaru](https://developer.android.com/studio/run/managing-avds.html#hpproperties).


<a name="troubleshooting" />

## <a name="troubleshooting"></a>Poradce při potížích

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Následující část popisuje běžné problémy Správce zařízení Xamarin Android a alternativní řešení:

### <a name="android-sdk-in-non-standard-location"></a>Sady SDK pro Android v nestandardním umístění

SDK pro Android je obvykle nainstalovaný v následujícím umístění:

**C:\\Program Files (x86)\\Android\\android-sdk**

Pokud sada SDK není nainstalovaná v tomto umístění, může při spuštění se tato chyba:

![Chyba instance sady Android SDK](xamarin-device-manager-images/win/32-sdk-error.png)

Chcete-li vyřešit tento problém, postupujte takto:

1. Z plochy, přejděte na **C:\\uživatelé\\*uživatelské jméno*\\data aplikací\\Roaming\\XamarinDeviceManager**:

    ![Umístění souboru protokolu Správce zařízení Xamarin](xamarin-device-manager-images/win/33-log-files.png)

2. Dvojitým kliknutím otevřete jeden ze souborů protokolu a vyhledejte **cesta k souboru konfigurace**. Příklad:

    [![Cesta k souboru konfigurace v souboru protokolu](xamarin-device-manager-images/win/34-config-file-path-sml.png)](xamarin-device-manager-images/win/34-config-file-path.png#lightbox)

3. Přejděte do tohoto umístění a poklikejte na soubor **user.config** ho otevřete. 

4. V **user.config**, vyhledejte  **&lt;UserSettings&gt;**  elementu a přidejte **AndroidSdkPath** atribut ho. Nastavte tento atribut na cestu nainstalovanou SDK pro Android v počítači a soubor uložte. Například  **&lt;UserSettings&gt;**  by vypadat třeba takto, pokud byla nainstalována SDK pro Android v **C:\\programy\\Android\\SDK**:
        
    ```xml
    <UserSettings SdkLibLastWriteTimeUtcTicks="636409365200000000" AndroidSdkPath="C:\Programs\Android\SDK" />
    ```

Po provedení této změny na **user.config**, byste měli spustit Správce zařízení Xamarin Android.

### <a name="generating-a-bug-report"></a>Generování sestavy chyb

Pokud narazíte na potíže s Xamarin Android Správce zařízení které nelze vyřešit pomocí výše uvedených tipy k řešení potíží, kliknutím pravým tlačítkem myši na záhlaví okna a výběrem prosím soubor sestavy chyb **Generovat sestavy chyb**:

![Umístění položky nabídky pro vyplnění sestavy chyb](xamarin-device-manager-images/win/35-bug-report.png)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

V současné době nejsou žádné známé problémy nebo řešení pro Xamarin Android Správce zařízení v sadě Visual Studio for Mac. 

### <a name="generating-a-bug-report"></a>Generování sestavy chyb

Pokud narazíte na potíže, kliknutím na prosím soubor sestavy chyb **pomoci > Generovat sestavy chyb**:

![Umístění položky nabídky pro vyplnění sestavy chyb](xamarin-device-manager-images/mac/35-bug-report.png)

-----

 
 
## <a name="summary"></a>Souhrn

Tato příručka se zavedl správce Xamarin Android zařízení k dispozici v sadě Visual Studio pro Mac a Xamarin pro Visual Studio. Je vysvětlené nezbytné funkce, jako je spuštění a zastavení emulátoru systému Android, vyberte virtuální zařízení se systémem Android (AVD) ke spuštění, vytváření nových virtuálních zařízení a jak upravit virtuálního zařízení. Vysvětlení také najdete postup upravit vlastnosti profilu hardwaru pro další přizpůsobení.


## <a name="related-links"></a>Související odkazy

- [Změny nástrojů sady Android SDK](~/android/troubleshooting/sdk-cli-tooling-changes.md)
- [Ladění pomocí emulátoru sady SDK pro Android](~/android/deploy-test/debugging/android-sdk-emulator/index.md)
- [Sady SDK nástroje poznámky k verzi (Google)](https://developer.android.com/studiohttps://developer.xamarin.com/releases/sdk-tools.html)
- [avdmanager](https://developer.android.com/studio/command-line/avdmanager.html)
- [sdkmanager](https://developer.android.com/studio/command-line/sdkmanager.html)
