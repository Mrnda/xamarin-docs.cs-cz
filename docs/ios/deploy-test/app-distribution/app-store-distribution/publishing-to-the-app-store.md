---
title: Publikování aplikace na platformě Xamarin.iOS pro App Store
description: Tento dokument popisuje, jak nakonfigurovat, sestavit a publikovat aplikace pro Xamarin.iOS pro distribuci v App Store.
ms.prod: xamarin
ms.assetid: DFBCC0BA-D233-4DC4-8545-AFBD3768C3B9
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 06/25/2018
ms.openlocfilehash: 60aa177ccb14c443f1599b4ce42c07faa695baed
ms.sourcegitcommit: 7d766f8a080ee6999e47c873a9f2ccec8fa5dd5a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/03/2018
ms.locfileid: "37439171"
---
# <a name="publishing-xamarinios-apps-to-the-app-store"></a>Publikování aplikace na platformě Xamarin.iOS pro App Store

Publikování aplikace do [App Store](https://www.apple.com/ios/app-store/), vývojář aplikací, musíte nejprve odeslat – společně se snímky obrazovky, popis, ikony a další informace – do společnosti Apple k revizi. Po schválení aplikace Apple umístí se na App Store, kde uživatelé můžou ji zakoupit a nainstalovat přímo z jejich zařízení s Iosem.

Tato příručka popisuje, jak postupovat, Příprava aplikace pro App Store a odesílat do společnosti Apple k revizi. Konkrétně se popisuje:

> [!div class="checklist"]
> - Následující pokyny pro recenze App Store
> - Nastavení ID aplikace a oprávnění
> - Ikona App Store a ikony aplikace
> - Nastavení App Store, zřizovací profil
> - Aktualizuje **vydání** konfigurace sestavení
> - Konfigurace aplikace v iTunes Connectu
> - Sestavení aplikace a odesláním do společnosti Apple

> [!IMPORTANT]
> Apple [udává](https://developer.apple.com/news/?id=05072018a) , že od července 2018 se všechny aplikace a aktualizace odeslané do App Store musí sestavené v IOS 11 SDK a [musí podporovat zobrazení pro iPhone X](~/ios/platform/introduction-to-ios11/updating-your-app/visual-design.md).

## <a name="app-store-guidelines"></a>Pokyny pro App Store

Před odesláním aplikace pro publikování v App Store, ujistěte se, že splňuje standardy definované společnosti Apple [pokyny pro recenze v App Store](https://developer.apple.com/appstore/resources/approval/guidelines.html).
Při odesílání aplikace pro App Store společnosti Apple kontroly a ujistit se, že splňuje tyto požadavky. Pokud tomu tak není, Apple je odmítne – a budete muset řešit zmiňovanou problémy a odešlete znovu.
Proto je vhodné se seznámit pokyny co nejdříve do procesu vývoje.

Pár věcí, které dávejte pozor na při odesílání aplikace:

1. Zajistěte, aby že aplikace popis odpovídá jeho funkce.
2. Test, který nemá k chybě aplikace v rámci normálního využití. To zahrnuje využití na každé zařízení s Iosem, které podporuje.

Také se podívejte na [App Store související prostředky](https://developer.apple.com/app-store/resources/) , které poskytuje Apple.

## <a name="set-up-an-app-id-and-entitlements"></a>Nastavení ID aplikace a oprávnění

Každá aplikace systému iOS má jedinečné ID aplikace, která má přidruženou sadu aplikační služby volá *nároků*. Povolit oprávnění aplikace k provádění nejrůznějších operací, jako dostat nabízené oznámení, přístup k funkce iOS jako HealthKit a další.

K vytvoření ID aplikace a vyberte libovolné potřebné oprávnění, navštivte [portálu Apple Developer](https://developer.apple.com/account/) a postupujte podle těchto kroků:

1. V **certifikáty, ID a profily** vyberte **identifikátory > ID aplikace**.
2. Klikněte na tlačítko **+** tlačítko a zadejte **název** a **ID sady prostředků** pro novou aplikaci.
3. Přejděte do dolní části obrazovky a vyberte některou **App Services** aplikaci Xamarin.iOS, která se vyžaduje. Aplikační služby jsou popsané v [práce s funkcemi v Xamarin.iosu](~/ios/deploy-test/provisioning/capabilities/index.md) průvodce.
4. Klikněte na tlačítko **pokračovat** a postupujte podle na obrazovce pokynů a vytvořte nové ID aplikace.

Kromě výběru a konfigurace požadované aplikační služby, při definování ID aplikace, musíte nakonfigurovat ID aplikace a oprávnění v projektu Xamarin.iOS úpravou **Info.plist** a  **Do souboru Entitlements.plist** soubory. Další informace, podívejte se na [práce s nároky v Xamarin.iosu](~/ios/deploy-test/provisioning/entitlements.md) příručku, která popisuje, jak vytvořit **do souboru Entitlements.plist** Souborová služba a význam různá nastavení oprávnění obsahuje.

## <a name="include-an-app-store-icon"></a>Zahrnout ikona App Store

Při odeslání aplikace do společnosti Apple, se ujistěte, že obsahuje katalog prostředků, který obsahuje ikonu App Store. Se dozvíte, jak to provést, podívejte se na [App Store ikony v Xamarin.iosu](~/ios/app-fundamentals/images-icons/app-store-icon.md) průvodce.

## <a name="set-the-apps-icons-and-launch-screens"></a>Nastavení ikon aplikace a spouštěcí obrazovky

Pro společnosti Apple a zpřístupnit tak aplikaci pro iOS na App Store musí mít správné ikony a spouštěcí obrazovky pro všechna zařízení s Iosem, na kterém můžete spustit. Další informace o nastavení ikony aplikace a spouštěcí obrazovky přečtěte si následující příručky:

- [Ikony aplikace v Xamarin.iosu](~/ios/app-fundamentals/images-icons/app-icons.md)
- [Spouštěcí obrazovky pro aplikace Xamarin.iOS](~/ios/app-fundamentals/images-icons/launch-screens.md)

## <a name="create-and-install-an-app-store-provisioning-profile"></a>Vytvoření a instalace aplikace App Store, zřizovací profil

používá iOS *zřizovací profily* řídit, jak je možné nasadit konkrétní aplikaci sestavení. Jsou to soubory, které obsahují informace o certifikátu použitého k podepsání aplikace, ID aplikace a nainstalovanou aplikaci. Pro vývoj a ad hoc distribuci zřizovací profil také obsahuje seznam povolených zařízení, na které můžete aplikaci nasadit. Pro App Store distribuci pouze certifikátu a informace o ID aplikace jsou však součástí od App Store je jediným mechanismus pro distribuci veřejného.

Vytvoření a instalace aplikace App Store, zřizovací profil, postupujte podle těchto kroků:

1. Přihlaste se k [portálu Apple Developer](https://developer.apple.com/account/).
2. V **certifikáty, ID a profily**vyberte **zřizovací profily > distribuce**.
3. Klikněte na tlačítko **+** tlačítka, vyberte **App Store**a klikněte na tlačítko **pokračovat**.
4. Vyberte svou aplikaci **ID aplikace** ze seznamu a klikněte na tlačítko **pokračovat**.
5. Vyberte podpisový certifikát a klikněte na tlačítko **pokračovat**.
6. Zadejte **název profilu** a klikněte na tlačítko **pokračovat** k vygenerování profilu.
7. Pomocí Xamarinu pro [Správa účtů Apple](~/cross-platform/macios/apple-account-management.md) nástroje pro stažení nově vytvořené zřizování profilu do vašeho macu. Pokud jste na počítači Mac, můžete také stáhnout přímo z portálu Apple Developer zřizovací profil a poklepejte na něj nainstalovat.

Podrobné pokyny najdete v tématu [vytváření distribučního profilu](~/ios/get-started/installation/device-provisioning/manual-provisioning.md#provisioningprofile) a [výběr distribučního profilu v projektu Xamarin.iOS](~/ios/deploy-test/app-distribution/app-store-distribution/index.md#selectprofile).

## <a name="update-the-release-build-configuration"></a>Aktualizace konfigurace sestavení pro vydání

Nové projekty Xamarin.iOS automaticky nastavit **ladění** a **vydání** _konfigurace sestavení_. O správné konfiguraci **vydání** sestavení, postupujte podle těchto kroků:

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

1. Z **oblasti řešení**, otevřete **Info.plist**. Vyberte **ruční zřizování**. Soubor uložte a zavřete.
2. Klikněte pravým tlačítkem na **název projektu** v **oblasti řešení**vyberte **možnosti**a přejděte **iOS Build** kartu.
3. Nastavte **konfigurace** k **vydání** a **platformy** k **iPhone**.
4. K sestavení konkrétní sadou SDK pro iOS, vyberte ho z **verze sady SDK** seznamu. V opačném případě ponechte tuto hodnotu na **výchozí**.
5. Odstranění limit nepoužitý kód propojení snižuje celkové velikosti aplikace. Ve většině případů **chování Linkeru** by měla být nastavena na výchozí hodnotu **propojit jen sady SDK architektury**. V některých případech, jako je při použití některých knihoven třetích stran, může být potřeba nastavit hodnotu **není odkaz** zajistit, že se neodebere potřebný kód. Další informace najdete [aplikace Xamarin.iOS propojení](~/ios/deploy-test/linker.md) průvodce.
6. Zkontrolujte **obrázků PNG optimalizovat** dále snížit velikost vaší aplikace.
7. Ladění by měl _není_ zapnout, protože to způsobí, že sestavení zbytečně velký.
8. Pro iOS 11, vyberte jednu z architektury zařízení, které podporuje **ARM64**. Další informace o sestavování pro zařízení s Iosem 64-bit, najdete v tématu **povolení 64bitové sestavení z aplikace na platformě Xamarin.iOS** část [informace o 32bitové/64bitové platformě](~/cross-platform/macios/32-and-64/index.md) dokumentaci.
9. Možná budete chtít použít **LLVM** kompilátoru k sestavení kódu menší a rychlejší. Tato možnost však zvyšuje dobu potřebnou ke kompilaci.
10. Podle potřeb vaší aplikace, můžete také chtít upravit typ **uvolňování** právě používá a nastaveny pro **internacionalizace**.

    Po nastavení možností je popsáno výše, nastavení sestavení by měl vypadat nějak takto:

    ![nastavení sestavení iOS](publishing-to-the-app-store-images/build-m157.png "nastavení sestavení pro iOS")

    Také se podívejte na za [mechanismy sestavování pro iOS](~/ios/deploy-test/ios-build-mechanics.md) příručku, která popisuje další nastavení buildu.

11. Přejděte **podepsání sady prostředků aplikace pro iOS** kartu. Pokud tady možnosti se nedají upravovat, ujistěte se, že **ručního zřizování** výběru v **Info.plist** souboru.
12. Ujistěte se, že **konfigurace** je nastavena na **vydání** a **platformy** je nastavena na **iPhone**.
13. Nastavte **Podpisová identita** k **distribuce (automaticky)**.
14. Pro **zřizovací profil**, vyberte App Store, zřizovací profil [vytvořené výše](#create-and-install-an-app-store-provisioning-profile).

    Váš projekt sady Možnosti podpisu by teď měl vypadat nějak takto:

    ![Podepsání sady prostředků aplikace pro iOS](publishing-to-the-app-store-images/bundleSigning-m157.png "podepsání sady prostředků aplikace pro iOS")

15. Klikněte na tlačítko **OK** uložte změny ve vlastnostech projektu.

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

1. Z **Průzkumníka řešení**, otevřete **Info.plist**. Vyberte **ruční zřizování**. Soubor uložte a zavřete.
2. Ujistěte se, že Visual Studio 2017 byla [spárované k hostiteli buildu Mac](~/ios/get-started/installation/windows/connecting-to-mac/index.md).
3. Klikněte pravým tlačítkem na **název projektu** v **Průzkumníku řešení**, vyberte **vlastnosti**a přejděte **iOS Build** kartu.
4. Nastavte **konfigurace** k **vydání** a **platformy** k **iPhone**.
5. K sestavení konkrétní sadou SDK pro iOS, vyberte ho z **verze sady SDK** seznamu. V opačném případě ponechte tuto hodnotu na **výchozí**.
6. Odstranění limit nepoužitý kód propojení snižuje celkové velikosti aplikace. Ve většině případů **chování Linkeru** by měla být nastavena na výchozí hodnotu **propojit jen sady SDK architektury**. V některých případech, jako je při použití některých knihoven třetích stran, může být potřeba nastavit hodnotu **není odkaz** zajistit, že se neodebere potřebný kód. Další informace najdete [aplikace Xamarin.iOS propojení](~/ios/deploy-test/linker.md) průvodce.
7. Zkontrolujte **obrázků PNG optimalizovat** dále snížit velikost vaší aplikace.
8. Ladění by nemělo být povoleno, protože to způsobí, že sestavení zbytečně velký.
9. Pro iOS 11, vyberte jednu z architektury zařízení, které podporuje **ARM64**. Další informace o sestavování pro zařízení s Iosem 64-bit, najdete v tématu **povolení 64bitové sestavení z aplikace na platformě Xamarin.iOS** část [informace o 32bitové/64bitové platformě](~/cross-platform/macios/32-and-64/index.md) dokumentaci.
10. Možná budete chtít použít **LLVM** kompilátoru k sestavení kódu menší a rychlejší. Tato možnost však zvyšuje dobu potřebnou ke kompilaci.
11. Podle potřeb vaší aplikace, můžete také chtít upravit typ **uvolňování** právě používá a nastaveny pro **internacionalizace**.

    Po nastavení možností je popsáno výše, nastavení sestavení by měl vypadat nějak takto:

    ![nastavení sestavení iOS](publishing-to-the-app-store-images/build-w157.png "nastavení sestavení pro iOS")

    Také se podívejte na za [mechanismy sestavování pro iOS](~/ios/deploy-test/ios-build-mechanics.md) příručku, která popisuje další nastavení buildu.

12. Přejděte **podepsání sady prostředků aplikace pro iOS** kartu. Pokud tady možnosti se nedají upravovat, ujistěte se, že **ručního zřizování** výběru v **Info.plist** souboru.
13. Ujistěte se, že **konfigurace** je nastavena na **vydání** a **platformy** je nastavena na **iPhone**.
14. Nastavte **Podpisová identita** k **distribuce (automaticky)**.
15. Pro **zřizovací profil**, vyberte App Store, zřizovací profil [vytvořené výše](#create-and-install-an-app-store-provisioning-profile).

    Váš projekt sady Možnosti podpisu by teď měl vypadat nějak takto:

    ![Podepsání sady prostředků nastavení iOS](publishing-to-the-app-store-images/bundleSigning-w157.png "podepsání sady prostředků nastavení pro iOS")

16. Přejděte **iOS – možnosti souborů IPA** kartu.
17. Ujistěte se, že **konfigurace** je nastavena na **vydání** a **platformy** je nastavena na **iPhone**.
18. Zkontrolujte, **sestavení iTunes Package Archive (IPA)** zaškrtávací políčko. Toto nastavení způsobí, že každý **vydání** (protože to je vybraná konfigurace) od sestavení k vygenerování souboru .ipa. Tento soubor lze odeslat společnosti Apple na vydání v App Store.

    > [!NOTE]
    > **iTunes Metadata** a **iTunes** nejsou nutné pro App Store vydané verze. Další informace, podívejte se na [soubor iTunesMetadata.plist v aplikace Xamarin.iOS](~/ios/deploy-test/app-distribution/itunesmetadata.md) a [obrázky pro iTunes](~/ios/app-fundamentals/images-icons/app-icons.md#itunes-artwork).

19. .Ipa název souboru, který se liší od názvu projektu Xamarin.iOS, zadejte ho **název balíčku** pole.

    ![Podepsání sady prostředků nastavení iOS](publishing-to-the-app-store-images/ipaOptions-w157.png "podepsání sady prostředků nastavení pro iOS")

20. Uložte konfiguraci sestavení a zavřete ho.

-----

## <a name="configure-your-app-in-itunes-connect"></a>Nakonfigurujte svoji aplikaci ve službě iTunes Connect

[iTunes Connect](https://itunesconnect.apple.com) je sada webové nástroje pro správu svých aplikacích pro iOS v App Store. Vaše aplikace Xamarin.iOS musejí být správně nakonfigurovány ve službě iTunes Connect, než může být odeslána do společnosti Apple k revizi a uvolněna na App Store.

Zjistěte, jak to provést, najdete [konfigurace aplikace ve službě iTunes Connect](~/ios/deploy-test/app-distribution/app-store-distribution/itunesconnect.md) průvodce.

## <a name="build-and-submit-your-app"></a>Vytvoření a odeslání aplikace

Správně nakonfigurované nastavení sestavení a iTunes Connect čeká na váš příspěvek teď můžete sestavit svoji aplikaci a odešlete do společnosti Apple.

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

1. V sadě Visual Studio pro Mac, vyberte **vydání** konfiguraci sestavení a zařízení (ne simulátor), pro které chcete sestavit.

    ![Sestavit výběr konfigurace a platforma](publishing-to-the-app-store-images/chooseConfig-m157.png "výběru konfigurace a platforma sestavení")

2. Z **sestavení** nabídce vyberte možnost **archivovat pro publikování**.
3. Po vytvoření archivu **archivy** zobrazení se zobrazí:

    ![Zobrazit archivy](publishing-to-the-app-store-images/archives-m157.png "archivuje zobrazení")

    > [!NOTE]
    > Ve výchozím nastavení **archivuje** zobrazení se zobrazí pouze archivy pro otevřené řešení. Pokud chcete zobrazit všechna řešení s archivy, zkontrolujte **zobrazit všechny archivy** zaškrtávací políčko. Je vhodné zachovat původní archivy tak, aby ladicí informace, které patří mezi ně můžete použít pro symbolizaci zpráv o chybách v případě potřeby.

4. Klikněte na tlačítko **podepsat a distribuovat...**  otevřete Průvodce publikováním.
5. Vyberte **App Store** distribuční kanál. Klikněte na tlačítko **Další**.

    ![Výběr distribuční kanál](publishing-to-the-app-store-images/distChannel-m157.png "distribuční kanál výběr")

6. V **zřizovací profil** okna, vyberte podpisovou identitu, aplikace a zřizovací profil. Klikněte na tlačítko **Další**.

    ![Výběr profilu zřizování](publishing-to-the-app-store-images/provProfileSelect-m157.png "výběr profilu zřizování")

7. Ověřte podrobnosti balíčku a klikněte na tlačítko **publikovat** uložení souboru .ipa pro aplikaci:

    ![Ověření podrobností aplikace](publishing-to-the-app-store-images/publish-m157.png "ověření podrobnosti aplikace")

8. Jakmile vaše .ipa byla uložena, vaše aplikace je připravená k odeslání do služby iTunes Connect.

    ![Připravena k odeslání](publishing-to-the-app-store-images/readyToGo-m157.png "připravena k odeslání")

9. Klikněte na tlačítko **otevřít zavaděč aplikace** a přihlaste se (Všimněte si, že je potřeba [vytvořit heslo aplikace konkrétní](https://support.apple.com/ht204397) zadání Apple ID).

    > [!NOTE]
    > Další informace o nástroji, podívejte se na [dokumentace společnosti Apple o zavaděč aplikací](https://help.apple.com/itc/apploader/#/apdS673accdb).

10. Vyberte **dodávat aplikace** a klikněte na tlačítko **zvolit** tlačítka:

    ![Vyberte doručování aplikací](publishing-to-the-app-store-images/publishvs01.png "vyberte doručování aplikací")

11. Vyberte soubor .ipa jste vytvořili výše a klikněte na tlačítko **OK** tlačítko.
12. Zavaděč aplikací se ověření souboru:

    ![Obrazovka ověření](publishing-to-the-app-store-images/publishvs02.png "obrazovky ověření")

13. Klikněte na tlačítko **Další** tlačítko a aplikace bude ověřovat na App Store:

    ![Ověřování App Store](publishing-to-the-app-store-images/publishvs03.png "ověření proti App Store")

14. Klikněte na tlačítko **odeslat** tlačítko k odeslání aplikací do společnosti Apple k revizi.
15. Zavaděč aplikací bude informovat, když soubor se úspěšně odeslal.

    > [!NOTE]
    > Apple může zamítnout aplikací s využitím **iTunesMetadata.plist** součástí soubor .ipa výsledkem je chyba, jako je následující:
    >
    > `ERROR: ERROR ITMS-90047: "Disallowed paths ( "iTunesMetadata.plist" ) found at: Payload/iPhoneApp1.app"`
    >
    > Alternativní řešení této chyby, podívejte se na [tento příspěvek na fórech Xamarin](https://forums.xamarin.com/discussion/40388/disallowed-paths-itunesmetadata-plist-found-at-when-submitting-to-app-store/p1).

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

> [!NOTE]
> Visual Studio 2017 v současné době nepodporuje **archivovat pro publikování** pracovní postup najít v sadě Visual Studio pro Mac.

1. Ujistěte se, že Visual Studio 2017 byla [spárované k hostiteli buildu Mac](~/ios/get-started/installation/windows/connecting-to-mac/index.md).
2. Vyberte **vydání** ze sady Visual Studio 2017 **konfigurace řešení** rozevíracího seznamu a **iPhone** z **platformy řešení** rozevírací seznam.

    ![Sestavit výběr konfigurace a platforma](publishing-to-the-app-store-images/chooseConfig-w157.png "výběru konfigurace a platforma sestavení")

3. Sestavte projekt. Tím se vytvoří soubor s příponou .ipa.

    > [!NOTE]
    > [Aktualizace konfigurace sestavení pro vydání](#update-the-release-build-configuration) nakonfigurované nastavení sestavení aplikace chcete vytvořit soubor .ipa pro každou část tohoto dokumentu **vydání** sestavení.

4. Najít soubor .ipa na počítači s Windows, klikněte pravým tlačítkem na název projektu Xamarin.iOS ve Visual Studiu 2017 **Průzkumníka řešení** a zvolte **otevřít složku v Průzkumníku souborů**. Pak na právě otevřené Windows **Průzkumníka souborů**, přejděte **bin/iPhone/Release** podadresáře. Pokud nemáte [přizpůsobit umístění výstupu soubor .ipa](#customize-the-ipa-location), by měla být v tomto adresáři.
5. Chcete-li zobrazit místo toho soubor .ipa na hostiteli buildu Mac, klikněte pravým tlačítkem na název projektu Xamarin.iOS ve Visual Studiu 2017 **Průzkumníka řešení** (ve Windows) a vyberte **zobrazit soubor IPA na serveru sestavení**. Tím se otevře **Finder** okno na hostiteli buildu Mac se vybraný soubor .ipa.
6. Na hostiteli buildu Mac otevřete **Spouštěče aplikací**. V Xcode, vyberte **Xcode > Otevřít vývojářský nástroj > Spouštěče aplikací**.

    > [!NOTE]
    > Další informace o nástroji, podívejte se na [dokumentace společnosti Apple o zavaděč aplikací](https://help.apple.com/itc/apploader/#/apdS673accdb).

7. Přihlaste se ke spuštění aplikace (Všimněte si, že je potřeba [vytvořit heslo aplikace konkrétní](https://support.apple.com/ht204397) zadání Apple ID).
8. Vyberte **dodávat aplikace** a klikněte na tlačítko **zvolit** tlačítka:

    ![Vyberte doručování aplikací ] (publishing-to-the-app-store-images/publishvs01.png "vyberte doručování aplikací")

9. Vyberte soubor .ipa vytvořili výše a klikněte na tlačítko **OK**.
10. Zavaděč aplikací se ověření souboru:

    ![Obrazovka ověření](publishing-to-the-app-store-images/publishvs02.png "obrazovky ověření")

11. Klikněte na tlačítko **Další** tlačítko a aplikace bude ověřovat na App Store:

    ![Ověřování App Store](publishing-to-the-app-store-images/publishvs03.png "ověření proti App Store")

12. Klikněte na tlačítko **odeslat** tlačítko k odeslání aplikací do společnosti Apple k revizi.
13. Zavaděč aplikací bude informovat, když soubor se úspěšně odeslal.

    > [!NOTE]
    > Apple může zamítnout aplikací s využitím **iTunesMetadata.plist** součástí soubor .ipa výsledkem je chyba, jako je následující:
    >
    > `ERROR: ERROR ITMS-90047: "Disallowed paths ( "iTunesMetadata.plist" ) found at: Payload/iPhoneApp1.app"`
    >
    > Alternativní řešení této chyby, podívejte se na [tento příspěvek na fórech Xamarin](https://forums.xamarin.com/discussion/40388/disallowed-paths-itunesmetadata-plist-found-at-when-submitting-to-app-store/p1).

-----

## <a name="itunes-connect-status"></a>iTunes Connect stav

Pokud chcete zobrazit stav odeslání vaší aplikace, přihlaste se do služby iTunes Connect a vyberte svou aplikaci. Počáteční stav by měl být **čekání na kontrolu**, i když dočasně může načíst **nahrát přijatých** během zpracování.

![Čekání na kontrolu](publishing-to-the-app-store-images/image21.png "čekání na kontrolu")

## <a name="tips-and-tricks"></a>Tipy a triky

### <a name="customize-the-ipa-location"></a>Přizpůsobení umístění .ipa

**MSBuild** vlastnost `IpaPackageDir`, umožňuje přizpůsobení umístění výstupu soubor .ipa. Pokud `IpaPackageDir` nastavena do vlastního umístění, se umístí soubor .ipa v dané oblasti místo výchozí podadresář časovým razítkem. To může být užitečné při vytváření automatizovaných sestavení, která závisí na konkrétní adresář fungovala správně, jako jsou ty používané pro sestavení kontinuální integrace (CI).

Existuje několik způsobů je to možné používat novou vlastnost. Například do výstupního souboru .ipa pro staré výchozí adresář (stejně jako v Xamarin.iOS 9.6 a nižší), můžete nastavit `IpaPackageDir` vlastnost `$(OutputPath)` pomocí jedné z následujících přístupů. Oba přístupy jsou kompatibilní s všechna sestavení Unified API Xamarin.iOS, včetně integrovaného vývojového prostředí sestavení, jakož i sestavení příkazového řádku, které používají **msbuild** nebo **mdtool**:

- První možností je nastavit `IpaPackageDir` vlastnosti v rámci `<PropertyGroup>` prvek **MSBuild** souboru. Můžete například přidat následující `<PropertyGroup>` do spodní části souboru .csproj projektu aplikace iOS (těsně před uzavírací `</Project>` značka):

    ```xml
    <PropertyGroup>
      <IpaPackageDir>$(OutputPath)</IpaPackageDir>
    </PropertyGroup>
    ```

- Lepším řešením je přidat `<IpaPackageDir>` elementu k dolnímu okraji existující `<PropertyGroup>` , který odpovídá konfiguraci sloužící k sestavení soubor .ipa. To je lepší, protože ji připravil projektu pro budoucí kompatibilitu s plánované nastavení na stránce vlastností projektu iOS – možnosti souborů IPA. Pokud aktuálně používáte službu `Release|iPhone` konfigurace sestavení soubor .ipa skupině kompletní aktualizovaná vlastnost může vypadat nějak takto:

    ```xml
    <PropertyGroup Condition=" '$(Configuration)|$(Platform)' == 'Release|iPhone'">
       <Optimize>true</Optimize>
       <OutputPath>bin\iPhone\Release</OutputPath>
       <ErrorReport>prompt</ErrorReport>
       <WarningLevel>4</WarningLevel>
       <ConsolePause>false</ConsolePause>
       <CodesignKey>iPhone Developer</CodesignKey>
       <MtouchUseSGen>true</MtouchUseSGen>
       <MtouchUseRefCounting>true</MtouchUseRefCounting>
       <MtouchFloat32>true</MtouchFloat32>
       <CodesignEntitlements>Entitlements.plist</CodesignEntitlements>
       <MtouchLink>SdkOnly</MtouchLink>
       <MtouchArch>;ARMv7, ARM64</MtouchArch>
       <MtouchHttpClientHandler>HttpClientHandler</MtouchHttpClientHandler>
       <MtouchTlsProvider>Default</MtouchTlsProvider>
       <PlatformTarget>x86&</PlatformTarget>
       <BuildIpa>true</BuildIpa>
       <IpaPackageDir>$(OutputPath</IpaPackageDir>
    </PropertyGroup>
    ```

Alternativní metoda pro **msbuild** sestavení příkazového řádku, je přidat `/p:` argument příkazového řádku k nastavení `IpaPackageDir` vlastnost. V tomto případě Všimněte si, že **msbuild** Nerozbaluje `$()` výrazy předané na příkazovém řádku, takže není možné použít `$(OutputPath)` syntaxe. Místo toho musíte zadat úplnou cestu.

```bash
msbuild /p:Configuration="Release" /p:Platform="iPhone" /p:ServerAddress="192.168.1.3" /p:ServerUser="macuser" /p:IpaPackageDir="%USERPROFILE%\Builds" /t:Build SingleViewIphone1.sln
```

Nebo následující na počítači Mac:

```bash
msbuild /p:Configuration="Release" /p:Platform="iPhone" /p:IpaPackageDir="$HOME/Builds" /t:Build SingleViewIphone1.sln
```

Vaší distribuce sestavení vytvořeno a archivovat, nyní jste připraveni k odeslání vaší žádosti do služby iTunes Connect.

## <a name="summary"></a>Souhrn

Tento článek popisuje, jak nakonfigurovat, sestavit a odeslat aplikaci pro iOS na vydání v App Store.

## <a name="related-links"></a>Související odkazy

- [Portál pro vývojáře Apple (Apple)](https://developer.apple.com/account/)
- [iTunes Connect (Apple)](https://itunesconnect.apple.com)
- [Pokyny pro recenze v App Store (Apple)](https://developer.apple.com/appstore/resources/approval/guidelines.html)
- [Zamítnutí běžné aplikace (Apple)](https://developer.apple.com/app-store/review/rejections/)
- [Práce s funkcemi v Xamarin.iosu](~/ios/deploy-test/provisioning/capabilities/index.md)
- [Práce s nároky v Xamarin.iosu](~/ios/deploy-test/provisioning/entitlements.md)
- [Konfigurace aplikace v iTunes Connectu](~/ios/deploy-test/app-distribution/app-store-distribution/itunesconnect.md)
- [Ikony aplikace v Xamarin.iosu](~/ios/app-fundamentals/images-icons/app-icons.md)
- [Spouštěcí obrazovky pro aplikace Xamarin.iOS](~/ios/app-fundamentals/images-icons/launch-screens.md)
- [Dokumentace k Application zavaděč (Apple)](https://help.apple.com/itc/apploader/#/apdS673accdb)
