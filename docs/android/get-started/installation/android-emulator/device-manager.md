---
title: Správa virtuálního zařízení pomocí Správce zařízení Android
description: Tento článek vysvětluje, jak vytvořit a nakonfigurovat virtuální zařízení se systémem Android (AVDs), emulovat fyzického zařízení se systémem Android pomocí Správce zařízení Android. Tato virtuální zařízení můžete použít ke spuštění a testování aplikace bez nutnosti se spoléhat na fyzické zařízení.
ms.prod: xamarin
ms.assetid: ECB327F3-FF1C-45CC-9FA6-9C11032BD5EF
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 06/22/2018
ms.openlocfilehash: a7c1aeafd94d7e2639617cda13312ee8a09e2c94
ms.sourcegitcommit: 26033c087f49873243751deded8037d2da701655
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 06/25/2018
ms.locfileid: "36935325"
---
# <a name="managing-virtual-devices-with-the-android-device-manager"></a>Správa virtuálního zařízení pomocí Správce zařízení Android

_Tento článek vysvětluje, jak vytvořit a nakonfigurovat virtuální zařízení se systémem Android (AVDs), emulovat fyzického zařízení se systémem Android pomocí Správce zařízení Android. Tato virtuální zařízení můžete použít ke spuštění a testování aplikace bez nutnosti se spoléhat na fyzické zařízení._

## <a name="overview"></a>Přehled

Po ověření, zda je povoleno hardwarová akcelerace (jak je popsáno v [hardwarovou akceleraci emulátoru výkonu](~/android/get-started/installation/android-emulator/hardware-acceleration.md)), dalším krokem je používat _Správce zařízení Android_ (také označované jako _Správce zařízení Xamarin Android_) Chcete-li vytvořit virtuální zařízení, které můžete použít k testování a ladění aplikace.

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Tento článek vysvětluje, jak vytvořit, duplicitní, přizpůsobit a spuštění virtuálního zařízení se systémem Android pomocí Správce zařízení Android.

[![Snímek obrazovky nástroje Správce zařízení Android na kartě zařízení](device-manager-images/win/01-devices-dialog-sml.png)](device-manager-images/win/01-devices-dialog.png#lightbox)

Můžete vytvořit a nakonfigurovat pomocí Správce zařízení Android _virtuální zařízení se systémem Android_ (AVDs), na kterých běží [emulátoru Android](~/android/deploy-test/debugging/debug-on-emulator.md).
Každý AVD je konfigurace aplikace emulátoru, která simuluje fyzického zařízení s Androidem. To umožňuje spuštění a testování vaší aplikace v různých konfiguracích, které simulují různé fyzické zařízení se systémem Android.

## <a name="requirements"></a>Požadavky

Pomocí Správce zařízení Android, budete potřebovat následující:

-   Visual Studio 2017 verze 15.7 nebo novější je povinný. Jsou podporovány edice Visual Studio Community, Professional a Enterprise.

-   Nástroje sady Visual Studio pro Xamarin verze 4.9 nebo novější.

-   Sadu Android SDK musí být nainstalovaný (najdete v části [nastavení Android SDK pro Xamarin.Android](~/android/get-started/installation/android-sdk.md)) a verze sady SDK nástroje 26.1.1 nebo novější musí být nainstalován jak je popsáno v následující části. (Pokud již není nainstalována), nainstalujte sady SDK pro Android v následujícím umístění: **C:\\Program Files (x86)\\Android\\android-sdk**.


## <a name="launching-the-device-manager"></a>Spuštění Správce zařízení

Spusťte Správce zařízení Android z **nástroje** nabídky kliknutím **nástroje > Android > Správce zařízení Android**:

[![Spouštění z nabídky Nástroje](device-manager-images/win/04-tools-menu-sml.png)](device-manager-images/win/04-tools-menu.png#lightbox)

Pokud se při spuštění následující chybový dialog, najdete v části [Android řešení potíží s emulátoru](~/android/get-started/installation/android-emulator/troubleshooting.md) pokyny alternativní řešení:

![Chyba instance sady Android SDK](device-manager-images/win/32-sdk-error.png)

Než budete moct použít Správce zařízení Android, je nutné nainstalovat verzi nástroje pro Android SDK 26.1.1 nebo novější. Pokud Android SDK nástroje 26.1.1 nebo novější není nainstalován, mohou se při spuštění zobrazit toto dialogové okno chyby:

![Dialogové okno chyby instance Android SDK](device-manager-images/win/02-sdk-instance-error.png)

Pokud se zobrazí toto dialogové okno chyby, klikněte na tlačítko **otevřete SDK Manager** otevřete Android SDK Manager. V Android SDK Manager, klikněte **nástroje** kartě a nainstalovat následující:

-   **Nástroje pro Android SDK 26.1.1** nebo novější 
-   **Android SDK nástrojů platformy 27.0.1** nebo novější  
-   **Android SDK – nástroje sestavení 27.0.3** nebo novější

Tyto balíčky se mají s **nainstalovaná** stav, jak je vidět na následujícím snímku obrazovky:

[![Instalace sady SDK pro Android nástrojů 27.0](device-manager-images/win/03-sdk-tools-sml.png)](device-manager-images/win/03-sdk-tools.png#lightbox)

Po instalaci těchto balíčků, můžete SDK Manager zavřete a znovu spusťte Správce zařízení Android.

## <a name="main-screen"></a>Hlavní obrazovky

Jakmile poprvé spustíte Správce zařízení Android, uvede k obrazovce, která se zobrazí všechny aktuálně nakonfigurované virtuální zařízení. Pro každé zařízení **název**, **operačního systému** (Android API úrovně) **procesoru**, **paměti** velikost a rozlišení obrazovky se zobrazí:

[![Seznam nainstalovaných zařízení a jejich parametrů](device-manager-images/win/05-installed-list-sml.png)](device-manager-images/win/05-installed-list.png#lightbox)

Po kliknutí na tlačítko zařízení v seznamu **spustit** tlačítko se zobrazí na pravé straně. Můžete kliknout na **spustit** tlačítko spusťte emulátor s tímto virtuálním zařízením:

[![Tlačítko Start pro obrázku zařízení](device-manager-images/win/06-start-button-sml.png)](device-manager-images/win/06-start-button.png#lightbox)

Po emulátoru začíná vybrané virtuální zařízení, **spustit** tlačítko se změní **Zastavit** tlačítko, které slouží k zastavení emulátoru:

[![Zastavit tlačítko spuštěné zařízení](device-manager-images/win/07-stop-button-sml.png)](device-manager-images/win/07-stop-button.png#lightbox)

### <a name="new-device"></a>Nové zařízení

Chcete-li vytvořit nové zařízení, klikněte na tlačítko **nový** tlačítko (nachází se v pravé horní části obrazovky):

[![Nové tlačítko pro vytvoření nového zařízení](device-manager-images/win/08-new-button-sml.png)](device-manager-images/win/08-new-button.png#lightbox)

Kliknutím na tlačítko **nový** spustí **nové zařízení** obrazovky:

[![Nové zařízení obrazovky Správce zařízení](device-manager-images/win/09-new-device-editor-sml.png)](device-manager-images/win/09-new-device-editor.png#lightbox)

Konfigurace nového zařízení v **nové zařízení** obrazovky, použijte následující postup:

1. Vyberte fyzické zařízení emulovat kliknutím **zařízení** rozevírací nabídce:

    [![Zařízení rozevírací nabídky](device-manager-images/win/10-device-menu-sml.png)](device-manager-images/win/10-device-menu.png#lightbox)

2. Vybrat bitovou kopii systému pro použití s tímto virtuálním zařízením kliknutím **bitovou kopii systému** rozevírací nabídce. Tato nabídka uvádí obrázky Správce zařízení nainstalovaný systém v části **nainstalovaná**. **Stáhnout** části je uveden seznam Image zařízení správce systému, které jsou nyní k dispozici ve svém vývojovém počítači, ale mohou být automaticky nainstalovány:

    [![Rozevírací nabídce bitové kopie systému](device-manager-images/win/11-system-image-menu-sml.png)](device-manager-images/win/11-system-image-menu.png#lightbox)

3. Zadejte nový název zařízení. V následujícím příkladu je název nové zařízení **25 rozhraní API 5 Nexus**:

    [![Pojmenování nové zařízení](device-manager-images/win/12-device-name-sml.png)](device-manager-images/win/12-device-name.png#lightbox)

4. Upravte všechny vlastnosti, které budete muset upravit. Změnit vlastnosti, najdete v části [úpravy Android virtuální vlastnosti zařízení](~/android/get-started/installation/android-emulator/device-properties.md).

5. Přidáte žádné další vlastnosti, které je potřeba explicitně nastaven. **Nové zařízení** obrazovky jsou uvedeny pouze vlastnosti nejvíce změnil, ale můžete kliknout na **přidat vlastnost** rozevírací nabídky (v levém dolním) Chcete-li přidat další vlastnosti. V následujícím příkladu `hw.lcd.backlight` se přidává vlastnost:

    [![Přidat vlastnost rozevírací nabídky](device-manager-images/win/13-add-property-menu-sml.png)](device-manager-images/win/13-add-property-menu.png#lightbox)

6. Klikněte **vytvořit** tlačítko (pravém dolním) k vytvoření nového zařízení:

    ![Vytvoření tlačítka](device-manager-images/win/14-create-button.png)

7. Můžete získat **přijetí licence** obrazovky. Klikněte na tlačítko **přijmout** Pokud souhlasíte s licenčními podmínkami:

    ![Licence přijetí obrazovky](device-manager-images/win/15-license-acceptance.png)

8. Správce zařízení Android nové zařízení přidá do seznamu nainstalovaných virtuální zařízení při zobrazování **vytváření** indikátor průběhu během vytváření zařízení:

    [![Průběh vytváření ukazatele](device-manager-images/win/16-creating-the-device-sml.png)](device-manager-images/win/16-creating-the-device.png#lightbox)

9. Po dokončení procesu vytváření nového zařízení se zobrazí v seznamu nainstalovaných virtuální zařízení s **spustit** tlačítko připraven ke spuštění:

   [![Nově vytvořený připraven ke spuštění zařízení](device-manager-images/win/17-created-device-sml.png)](device-manager-images/win/17-created-device.png#lightbox)


### <a name="edit-device"></a>Upravit zařízení

Chcete-li upravit stávající virtuální zařízení, vyberte zařízení a klikněte na tlačítko **upravit** tlačítko (umístěný v pravém horním rohu obrazovky):

[![Tlačítko pro úpravy nového zařízení upravit](device-manager-images/win/19-edit-button-sml.png)](device-manager-images/win/19-edit-button.png#lightbox)

Kliknutím na tlačítko **upravit** spustí Editor zařízení pro vybrané virtuální zařízení:

[![Editor zařízení obrazovky](device-manager-images/win/20-device-editor-sml.png)](device-manager-images/win/20-device-editor.png#lightbox)

**Zařízení Editor** obrazovky jsou uvedeny vlastnosti virtuálního zařízení z prvního sloupce s odpovídajícími hodnotami každé vlastnosti v druhém sloupci. Když vyberete vlastnost, podrobný popis této vlastnosti se zobrazí na pravé straně.

Například v následujícím snímku obrazovky `hw.lcd.density` mění se vlastnost z **420** k **240**:

[![Příklad úpravy zařízení](device-manager-images/win/21-device-editing-sml.png)](device-manager-images/win/21-device-editing.png#lightbox)

Po provedení změn nezbytné konfigurace, klikněte na tlačítko **Uložit** tlačítko.
Další informace o změně vlastnosti virtuální zařízení najdete v tématu [úpravy Android virtuální vlastnosti zařízení](~/android/get-started/installation/android-emulator/device-properties.md).

 
### <a name="additional-options"></a>Další možnosti

Další možnosti pro práci s zařízení jsou dostupné z &hellip; nabídky v horním pravém rohu:

[![Umístění další možnosti nabídky](device-manager-images/win/22-overflow-menu-sml.png)](device-manager-images/win/22-overflow-menu.png#lightbox)

V nabídce Další možnosti obsahuje následující položky:

-   **Duplicitní a upravit** &ndash; duplikuje aktuálně vybrané zařízení a otevře ji v **nové zařízení** obrazovky s jiný jedinečný název. Například výběr **VisualStudio_android 23_x86_phone** a kliknutím na **duplicitní a upravit** připojí k název čítače:

    [![Duplicitní a úpravy obrazovky](device-manager-images/win/23-dupe-and-edit-sml.png)](device-manager-images/win/23-dupe-and-edit.png#lightbox)

-   **V Průzkumníku odhalit** &ndash; otevře okno Průzkumníka Windows ve složce, která obsahuje soubory pro virtuální zařízení. Například výběr **API Nexus 5 X 25** a kliknutím na **odhalit v Průzkumníku** otevře okno takto:

    [![Výsledky kliknutím na zobrazení v Průzkumníku](device-manager-images/win/24-reveal-in-explorer-sml.png)](device-manager-images/win/24-reveal-in-explorer.png#lightbox)

-   **Obnovení továrního nastavení** &ndash; vybrané zařízení obnoví výchozí nastavení, aktualizovat je, vymazat všechny uživatele provedené vnitřní stav zařízení byl spuštěného (to také vymaže aktuální [rychlé spuštění](~/android/deploy-test/debugging/debug-on-emulator.md#quick-boot) snímku Pokud existuje). Tato změna nezmění úpravy, které provedete virtuálního zařízení během vytváření a úpravy. Zobrazí se dialogové okno s připomenutím tento resetování nelze vrátit zpět. Klikněte na tlačítko **vymazat data** potvrďte vynulování.

-   **Odstranit** &ndash; trvale odstraní vybrané virtuální zařízení.
    Dialogové okno se zobrazí se upozornění, že při odstranění zařízení nelze vrátit zpět. Klikněte na tlačítko **odstranit** Pokud jste si jisti, že chcete odstranit zařízení.


# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

Tento článek vysvětluje, jak vytvořit, duplicitní, přizpůsobit a spuštění virtuálního zařízení se systémem Android pomocí Správce zařízení Android.

[![Snímek obrazovky nástroje Správce zařízení Android na kartě zařízení](device-manager-images/mac/01-devices-dialog-sml.png)](device-manager-images/mac/01-devices-dialog.png#lightbox)

> [!NOTE]
> Tento průvodce se týká pouze pro Visual Studio for Mac.
Xamarin Studio není kompatibilní s Správce zařízení Android.

Můžete vytvořit a nakonfigurovat pomocí Správce zařízení Android *virtuální zařízení se systémem Android* (AVDs), na kterých běží [emulátoru Android](~/android/deploy-test/debugging/debug-on-emulator.md).
Každý AVD je konfigurace aplikace emulátoru, která simuluje fyzického zařízení s Androidem. To umožňuje spuštění a testování vaší aplikace v různých konfiguracích, které simulují různé fyzické zařízení se systémem Android.

## <a name="requirements"></a>Požadavky

- Visual Studio pro Mac 7.5 nebo novější.

- Android SDK 8.0 (26 rozhraní API) nebo novější musí být nainstalován prostřednictvím Android SDK Manager.


## <a name="launching-the-device-manager"></a>Spuštění Správce zařízení

Spusťte Správce zařízení Android kliknutím **nástroje > Správce zařízení**:

[![Spouštění z nabídky Nástroje](device-manager-images/mac/16-tools-menu-sml.png)](device-manager-images/mac/16-tools-menu.png#lightbox)

Než budete moct použít Správce zařízení Android, je nutné nainstalovat verzi nástroje pro Android SDK 26.0.2 nebo novější. Pokud Android SDK nástroje 26.0.2 nebo novější není nainstalovaná, zobrazí se toto dialogové okno chyby při spuštění:

![Dialogové okno chyby instance Android SDK](device-manager-images/mac/02-sdk-instance-error.png)

Pokud se zobrazí toto dialogové okno chyby, klikněte na tlačítko **OK** otevřete Android SDK Manager. V Android SDK Manager, klikněte **nástroje** kartě a nainstalovat následující:

-   **Nástroje pro Android SDK 26.0.2** nebo novější 
-   **Android SDK nástrojů platformy 26.0.0** nebo novější 
-   **Android SDK – nástroje sestavení 26.0.0** nebo novější

Tyto balíčky se mají s **nainstalovaná** stav, jak je vidět na následujícím snímku obrazovky:

[![Instalace sady SDK pro Android nástrojů 26.0](device-manager-images/mac/03-sdk-tools-sml.png)](device-manager-images/mac/03-sdk-tools.png#lightbox)

## <a name="main-screen"></a>Hlavní obrazovky

Jakmile poprvé spustíte Správce zařízení Android, uvede k obrazovce, která se zobrazí všechny aktuálně nakonfigurované virtuální zařízení. Pro každé zařízení **název**, **bitovou kopii systému** (Android API úrovně) **procesoru**, **paměti** velikost a rozlišení obrazovky se zobrazí:

[![Seznam nainstalovaných zařízení a jejich parametrů](device-manager-images/mac/05-devices-list-sml.png)](device-manager-images/mac/05-devices-list.png#lightbox)

Klikněte na tlačítko **přehrání** tlačítko spusťte emulátor s virtuálním zařízením podle vašeho výběru:

[![Tlačítko Start pro obrázku zařízení](device-manager-images/mac/06-start-button-sml.png)](device-manager-images/mac/06-start-button.png#lightbox)

Po emulátoru začíná vybrané virtuální zařízení, **přehrání** tlačítko se změní **Zastavit** tlačítko, které slouží k zastavení emulátoru:

[![Zastavit tlačítko spuštěné zařízení](device-manager-images/mac/07-stop-button-sml.png)](device-manager-images/mac/07-stop-button.png#lightbox)

### <a name="new-device"></a>Nové zařízení

Chcete-li vytvořit nové zařízení, klikněte na tlačítko **nové zařízení** tlačítko (nachází se v pravé horní části obrazovky):

[![Nové tlačítko pro vytvoření nového zařízení](device-manager-images/mac/08-new-button-sml.png)](device-manager-images/mac/08-new-button.png#lightbox)

Kliknutím na tlačítko **nové zařízení** spustí **nové zařízení** obrazovky:

[![Nové zařízení obrazovky Správce zařízení](device-manager-images/mac/09-new-device-editor-sml.png)](device-manager-images/mac/09-new-device-editor.png#lightbox)

Použijte následující postup ke konfiguraci nové zařízení na **nové zařízení** obrazovky:

1. Vyberte fyzické zařízení emulovat kliknutím **zařízení** rozevírací nabídce:

    [![Zařízení rozevírací nabídky](device-manager-images/mac/10-device-menu-sml.png)](device-manager-images/mac/10-device-menu.png#lightbox)

2. Vybrat bitovou kopii systému pro použití s tímto virtuálním zařízením kliknutím **bitovou kopii systému** rozevírací nabídce. Tato nabídka uvádí obrázky Správce zařízení nainstalovaný systém v části **nainstalovaná**. **Stáhnout** část (Pokud zobrazené) obsahuje Image zařízení správce systému, které jsou nyní k dispozici ve svém vývojovém počítači, ale mohou být automaticky nainstalovány:

    [![Rozevírací nabídce bitové kopie systému](device-manager-images/mac/11-system-image-menu-sml.png)](device-manager-images/mac/11-system-image-menu.png#lightbox)

3. Zadejte nový název zařízení. V následujícím příkladu je název nové zařízení **API Nexus 5 X 25**:

    [![Pojmenování nové zařízení](device-manager-images/mac/12-device-name-sml.png)](device-manager-images/mac/12-device-name.png#lightbox)

4. Upravte všechny vlastnosti, které budete muset upravit. Změnit vlastnosti, najdete v části [úpravy Android virtuální vlastnosti zařízení](~/android/get-started/installation/android-emulator/device-properties.md).

5. Přidáte žádné další vlastnosti, které je potřeba explicitně nastaven. **Nové zařízení** obrazovky jsou uvedeny pouze vlastnosti nejvíce změnil, ale můžete kliknout na **přidat vlastnost** rozevírací nabídky (v levém dolním) Chcete-li přidat další vlastnosti:

    [![Přidat vlastnost rozevírací nabídky](device-manager-images/mac/13-add-property-menu-sml.png)](device-manager-images/mac/13-add-property-menu.png#lightbox)

6. Můžete také kliknout na **vlastní** definovat nové vlastnosti pro zařízení:

    ![Vytvoření tlačítka](device-manager-images/mac/14-custom-button.png)

7. Klikněte **vytvořit** tlačítko (pravém dolním) k vytvoření nového zařízení:

    ![Vytvoření tlačítka](device-manager-images/mac/15-create-button.png)

8. Můžete získat **přijetí licence** obrazovky. Klikněte na tlačítko **přijmout** Pokud souhlasíte s licenčními podmínkami.

9. Správce zařízení Android nové zařízení přidá do seznamu nainstalovaných virtuální zařízení při zobrazování **vytváření** indikátor průběhu během vytváření zařízení:

    [![Průběh vytváření ukazatele](device-manager-images/mac/17-creating-the-device-sml.png)](device-manager-images/mac/17-creating-the-device.png#lightbox)

10. Po dokončení procesu vytváření nového zařízení se zobrazí v seznamu zařízení s **přehrání** tlačítko připraven ke spuštění:

   [![Nově vytvořený připraven ke spuštění zařízení](device-manager-images/mac/18-created-device-sml.png)](device-manager-images/mac/18-created-device.png#lightbox)


### <a name="edit-device"></a>Upravit zařízení

Chcete-li upravit stávající virtuální zařízení, vyberte **další možnosti** rozevírací nabídky (ikona ozubené kolečko) a vyberte **upravit**:
 
[![Upravit výběr nabídky Úpravy nové zařízení](device-manager-images/mac/19-edit-button-sml.png)](device-manager-images/mac/19-edit-button.png#lightbox)

Kliknutím na tlačítko **upravit** spustí Editor zařízení pro vybrané virtuální zařízení:
 
[![Editor zařízení obrazovky](device-manager-images/mac/20-device-editor-sml.png)](device-manager-images/mac/20-device-editor.png#lightbox)

**Zařízení Editor** obrazovky jsou uvedeny vlastnosti virtuálního zařízení z prvního sloupce s odpovídajícími hodnotami každé vlastnosti v druhém sloupci. Když vyberete vlastnost, podrobný popis této vlastnosti se zobrazí na pravé straně.

Například v následujícím snímku obrazovky `hw.lcd.density` změnit vlastnost z **320** k **240** a `hw.ramSize` vlastnost se změní na **768**:
 
[![Příklad úpravy zařízení](device-manager-images/mac/21-device-editing-sml.png)](device-manager-images/mac/21-device-editing.png#lightbox)

Po provedení změn nezbytné konfigurace, klikněte na tlačítko **Uložit** tlačítko.
Další informace o změně vlastnosti virtuální zařízení najdete v tématu [úpravy Android virtuální vlastnosti zařízení](~/android/get-started/installation/android-emulator/device-properties.md).

 
### <a name="additional-options"></a>Další možnosti

Další možnosti pro práci se zařízením jsou k dispozici v rozevírací nabídce nachází na levé straně **přehrání** tlačítko:

[![Umístění další možnosti nabídky](device-manager-images/mac/22-overflow-menu-sml.png)](device-manager-images/mac/22-overflow-menu.png#lightbox)

V nabídce Další možnosti obsahuje následující položky:

-   **Upravit** &ndash; otevře v editoru zařízení aktuálně vybrané zařízení, jak je popsáno výše.

-   **Duplicitní a upravit** &ndash; duplikuje aktuálně vybrané zařízení a otevře ji v **nové zařízení** obrazovky s jiný jedinečný název.
    Například výběr **API Nexus 5 X 25** a kliknutím na **duplicitní a upravit** připojí k název čítače:

    [![Duplicitní a úpravy obrazovky](device-manager-images/mac/23-dupe-and-edit-sml.png)](device-manager-images/mac/23-dupe-and-edit.png#lightbox)

-   **Odhalit v hledání** &ndash; otevře okno vyhledávací systému macOS ve složce, která obsahuje soubory pro virtuální zařízení. Například výběr **API Nexus 5 X 25** a kliknutím na **odhalit v hledání** otevře okno takto:

    [![Výsledky kliknutím na zobrazení v Průzkumníku](device-manager-images/mac/24-reveal-in-finder-sml.png)](device-manager-images/mac/24-reveal-in-finder.png#lightbox)

-   **Obnovení továrního nastavení** &ndash; vybrané zařízení obnoví výchozí nastavení, aktualizovat je, vymazat všechny uživatele provedené vnitřní stav zařízení byl spuštěn. Tato změna nezmění úpravy, které provedete virtuálního zařízení během vytváření a úpravy. Zobrazí se dialogové okno s připomenutím tento resetování nelze vrátit zpět. Klikněte na tlačítko **vymazat data** potvrďte vynulování.

-   **Odstranit** &ndash; trvale odstraní vybrané virtuální zařízení.
    Dialogové okno se zobrazí se upozornění, že při odstranění zařízení nelze vrátit zpět. Klikněte na tlačítko **odstranit** Pokud jste si jisti, že chcete odstranit zařízení.

-----

## <a name="troubleshooting"></a>Poradce při potížích

Následující části popisují, jak diagnostikovat a vyřešit problémy, které mohou nastat při konfiguraci virtuálních zařízení pomocí Správce zařízení Android.

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

### <a name="android-sdk-in-non-standard-location"></a>Sady SDK pro Android v nestandardním umístění

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

### <a name="snapshot-disables-wifi-on-android-oreo"></a>Snímek zakáže Wi-Fi na Android Oreo

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

### <a name="snapshot-disables-wifi-on-android-oreo"></a>Snímek zakáže Wi-Fi na Android Oreo

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

### <a name="generating-a-bug-report"></a>Generování sestavy chyb

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Pokud narazíte na potíže s Android Správce zařízení které nelze vyřešit pomocí výše uvedených tipy k řešení potíží, kliknutím pravým tlačítkem myši na záhlaví okna a výběrem prosím soubor sestavy chyb **Generovat sestavy chyb**:

[![Umístění položky nabídky pro vyplnění sestavy chyb](troubleshooting-images/win/04-bug-report-sml.png)](troubleshooting-images/win/04-bug-report.png#lightbox)


# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

Pokud narazíte na potíže s Android Správce zařízení které nelze vyřešit pomocí výše uvedených tipy k řešení potíží, kliknutím na prosím soubor sestavy chyb **pomoci > Generovat sestavy chyb**:

[![Umístění položky nabídky pro vyplnění sestavy chyb](troubleshooting-images/mac/01-bug-report-sml.png)](troubleshooting-images/mac/01-bug-report.png#lightbox)


-----

## <a name="summary"></a>Souhrn

Tato příručka se zavedl Správce zařízení Android, která je k dispozici v sadě Visual Studio pro Mac a Visual Studio Tools pro Xamarin. Je vysvětlené nezbytné funkce, jako je spuštění a zastavení emulátoru systému Android, vyberte virtuální zařízení se systémem Android (AVD) ke spuštění, vytváření nových virtuálních zařízení a jak upravit virtuálního zařízení. Ho vysvětlení, jak upravit vlastnosti profilu hardwaru pro další přizpůsobení a je poskytovala tipy k řešení potíží pro běžné problémy.


## <a name="related-links"></a>Související odkazy

- [Změny nástrojů sady Android SDK](~/android/troubleshooting/sdk-cli-tooling-changes.md)
- [Ladění na emulátoru systému Android](~/android/deploy-test/debugging/debug-on-emulator.md)
- [Sady SDK nástroje poznámky k verzi (Google)](https://developer.android.com/studio/releases/sdk-tools)
- [avdmanager](https://developer.android.com/studio/command-line/avdmanager.html)
- [sdkmanager](https://developer.android.com/studio/command-line/sdkmanager.html)
