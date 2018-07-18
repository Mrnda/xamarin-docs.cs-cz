---
title: Bezplatné zřizování pro aplikace Xamarin.iOS
description: Tento dokument popisuje, jak mohou vývojáři Xamarin.iOS testovat své aplikace na fyzickém zařízení bez nutnosti registrace pro placené programu pro vývojáře společnosti Apple.
ms.prod: xamarin
ms.assetid: A5CE2ECF-8057-49ED-8393-EB0C5977FE4C
ms.technology: xamarin-ios
author: asb3993
ms.author: amburns
ms.date: 07/16/2018
ms.openlocfilehash: 22ac17e211562eccbc49cc213e06079e77dd08c0
ms.sourcegitcommit: e98a9ce8b716796f15de7cec8c9465c4b6bb2997
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/18/2018
ms.locfileid: "39111154"
---
# <a name="free-provisioning-for-xamarinios-apps"></a>Bezplatné zřizování pro aplikace Xamarin.iOS

Bezplatné zřizování umožňuje vývojářům Xamarin.iOS pro nasazování a testování své aplikace na zařízení s Iosem **bez** se zapojil **Apple Developer Program**.
Při testování simulátor cenných a pohodlné, je také nezbytné pro testování aplikací na zařízeních s Iosem fyzický k ověření, že fungují správně v nabídce skutečná paměť, úložiště a omezení připojení k síti.

Nasazení aplikace do zařízení pomocí bezplatné zřizování:

- Pomocí Xcode vytvořte nezbytné *Podpisová identita* (developer certifikát a privátní klíč) a *zřizovací profil* (který obsahuje explicitní ID aplikace a UDID připojené zařízení s iOS).
- Při nasazování aplikace Xamarin.iOS pomocí podpisovou identitu a zřizovací profil se vytvořil pomocí Xcode v sadě Visual Studio pro Mac nebo Visual Studio 2017.

> [!IMPORTANT]
> [Automatické zřizování](~/ios/get-started/installation/device-provisioning/automatic-provisioning.md) umožňuje sadě Visual Studio for Mac nebo Visual Studio 2017 automaticky nastavit zařízení pro testování vývojářem. Ale automatické zřizování není kompatibilní s bezplatné zřizování. Chcete-li použít automatické zřizování, musí mít placený účet Apple Developer Program.

## <a name="requirements"></a>Požadavky

Nasazení vaší aplikace Xamarin.iOS na zařízení s bezplatné zřizování:

- Apple ID, které se používají nesmí být připojené k programu pro vývojáře Apple.
- Aplikace Xamarin.iOS musíte použít explicitní ID aplikace, není zástupný znak ID aplikace.
- Identifikátor sady prostředků používaných v aplikaci Xamarin.iOS musí být jedinečný a nedá se používají v jiné aplikaci dříve. Libovolný identifikátor sady prostředků používá bezplatné zřizování **nelze** znovu použít.
- Pokud již distribuované aplikace se nedají nasadit tuto aplikaci bezplatné zřizování.
- Pokud vaše aplikace používá App Services, budete muset vytvořit zřizovací profil, jak je uvedeno v [zřizování zařízení](~/ios/get-started/installation/device-provisioning/index.md#appservices) průvodce. 

Podívejte se na [omezení](#limitations) část tohoto dokumentu pro další informace o omezeních přidružené k bezplatné zřizování a odkazovat [Průvodce distribuce aplikací](~/ios/deploy-test/app-distribution/index.md) Další informace o distribuce aplikací pro iOS.

## <a name="testing-on-device-with-free-provisioning"></a>Testování na zařízení s bezplatné zřizování

Postupujte podle následujících kroků pro testování vaší aplikace Xamarin.iOS pomocí bezplatné zřizování.

### <a name="use-xcode-to-create-a-signing-identity-and-provisioning-profile"></a>Pomocí Xcode můžete vytvořit podpisovou identitu a zřizovací profil

1. Pokud nemáte Apple ID, [vytvořit](https://appleid.apple.com).
2. Xcode otevřete a přejděte do **Xcode > Předvolby**.
3. V části **účty**, použijte **+** tlačítko můžete přidat existující Apple ID. By měla vypadat podobně jako na následujícím snímku obrazovky:

    ![Předvolby Xcode – účty](free-provisioning-images/launchapp1.png "předvolby Xcode – účty")

4. Zavřít předvolby Xcode.
5. Připojte zařízení s Iosem, ke které chcete nasadit vaši aplikaci.
6. V prostředí Xcode vytvořte nový projekt. Zvolte **soubor > Nový > projekt** a vyberte **aplikace s jedním zobrazením**.
7. V dialogovém okně Nový projekt, nastavte **týmu** Apple ID, který jste právě přidali. V rozevíracím seznamu by měla vypadat podobně jako **svůj název (osobní týmu)**:

    ![Vytvořit novou aplikaci](free-provisioning-images/launchapp2.png "vytvoření nové aplikace")

8. Po vytvoření nového projektu, vyberte schéma sestavení Xcode, který cílí na zařízení s Iosem (a ne simulátor).

    ![Vyberte schéma sestavení Xcode](free-provisioning-images/xcodescheme.png "vyberte schéma sestavení Xcode")

9. Otevřete nastavení projektu vaší aplikace tak, že vyberete jeho uzel nejvyšší úrovně v Xcode **navigátoru projektů**.
10. V části **Obecné > Identity**, ujistěte se, že **identifikátor sady prostředků** _přesně odpovídá_ identifikátor sady prostředků aplikace Xamarin.iOS.

    ![Identifikátor sady prostředků nastavení](free-provisioning-images/launchapp5.png "nastavení identifikátoru sady prostředků")

    > [!IMPORTANT]
    > Xcode pouze vytvoření zřizovacího profilu pro explicitní ID aplikace a musí být stejné jako ID aplikace z aplikace Xamarin.iOS.
    > Pokud se liší, nebude možné používat bezplatné zřizování k nasazení aplikace Xamarin.iOS.

11. V části **informace o nasazení**, zkontrolujte, že cíl nasazení odpovídá nebo je nižší než verze iOS nainstalovaný na zařízení s Iosem připojené.
12. V části **podepisování**vyberte **automaticky spravovat podepisování** a z rozevíracího seznamu vyberte váš tým:

    ![Automaticky spravovat podepisování](free-provisioning-images/launchapp6.png "automaticky spravovat podepisování")

    Xcode automaticky vygenerovat zřizovací profil a podpisovou identitu za vás. To můžete zobrazit kliknutím na ikonu informace vedle zřizovací profil:

    ![Zobrazit profil zřizování](free-provisioning-images/launchapp7.png "zobrazit zřizovací profil")

    > [!TIP]
    > Pokud dojde k selhání, když Xcode se pokusí vygenerovat zřizovacího profilu, zkontrolujte, zda že tento Xcode aktuálně vybrané sestavení schéma, zaměřuje připojené zařízení s iOS, ne simulátor.

13. Testování v Xcode, nasazení prázdné aplikace do zařízení kliknutím na tlačítko pro spuštění.

### <a name="deploy-your-xamarinios-app"></a>Nasazení aplikace Xamarin.iOS

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

1. Připojit zařízení s Iosem k hostiteli buildu Mac přes USB nebo [bezdrátově](~/ios/deploy-test/wireless-deployment.md).
2. V sadě Visual Studio pro Mac **oblasti řešení**, dvakrát klikněte na **Info.plist**.
3. V **podepisování**vyberte **ručního zřizování**.
4. Klikněte na tlačítko **podepsání sady prostředků aplikace pro iOS...** tlačítko.
5. Pro **konfigurace**vyberte **ladění**.
6. Pro **platformy**vyberte **iPhone**.
7. Vyberte **Podpisová identita** vytvořili pomocí Xcode.
8. Vyberte **zřizovací profil** vytvořili pomocí Xcode.

    ![Nastavte podpisovou identitu a zřizovací profil](free-provisioning-images/launchapp8.png "nastavit podpisovou identitu a zřizovací profil")

    > [!TIP]
    > Pokud nevidíte podpisové identity nebo správný zřizovacího profilu, budete muset restartovat Visual Studio pro Mac.

9. Klikněte na tlačítko **OK** uložte a zavřete **možnosti projektu**.
10. Vyberte zařízení s Iosem a spuštění aplikace.

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

1. Ujistěte se, že Visual Studio 2017 byla [spárované k hostiteli buildu Mac](~/ios/get-started/installation/windows/connecting-to-mac/index.md).
2. Připojit zařízení s Iosem k hostiteli buildu Mac přes USB nebo [bezdrátově](~/ios/deploy-test/wireless-deployment.md).
3. Visual Studio 2017 **Průzkumníka řešení**, klikněte pravým tlačítkem na projekt Xamarin.iOS a vyberte **vlastnosti**.
4. Přejděte do **podepsání sady prostředků aplikace pro iOS**.
5. Pro **konfigurace**vyberte **ladění**.
6. Pro **platformy**vyberte **iPhone**.
7. Vyberte **ruční zřizování**.
8. Vyberte **Podpisová identita** vytvořili pomocí Xcode.
9. Vyberte **zřizovací profil** vytvořili pomocí Xcode.
    
    ![Nastavte podpisovou identitu a zřizovací profil](free-provisioning-images/setprofile-w157.png "nastavit podpisovou identitu a zřizovací profil")

    > [!TIP]
    > Xcode vytvoří tento podpisovou identitu a zřizovací profil a uložené na hostiteli buildu Mac se vaše. Soubory byly přístupné pro Visual Studio 2017, protože byl [spárované](~/ios/get-started/installation/windows/connecting-to-mac/index.md) k hostiteli buildu Mac. Pokud nejsou uvedené, budete muset restartovat Visual Studio 2017.

10. Uložte a zavřete dialogové okno Vlastnosti projektu.
11. Vyberte zařízení s Iosem a spuštění aplikace.

-----

## <a name="limitations"></a>Omezení

Apple uložila několik omezení na kdy a jak vám pomůže bezplatné zřizování spouštět svoje aplikace na zařízení s Iosem, zajištění, že je lze nasadit pouze do *vaše* zařízení:

- Přístup do služby iTunes Connect je omezená a proto službami, jako je publikování pro App Store a testovacího prostředí nejsou k dispozici pro vývojáře volně zřizování svých aplikací. Účet Apple Developer (firemní nebo osobní) je nutné distribuovat přes Ad Hoc a interní prostředky.
- Zřizovací profily vytvořené pomocí bezplatné zřizování platnost vyprší po jeden týden a podpisové identity vyprší po jednom roce. 
- Protože Xcode vytvoří jenom zřizovací profily pro explicitní ID aplikace, které musí použít [výše uvedené pokyny](#testing-on-device-with-free-provisioning) pro každou aplikaci, kterou chcete nainstalovat.
- Zřizování pro většinu aplikační služby není možné bezplatné zřizování. To zahrnuje oprávnění Apple Pay, Game Center, Icloudu, nákupy v aplikaci, nabízená oznámení a peněženky. Úplný seznam možností, které poskytuje Apple [podporované schopnosti (iOS)](https://help.apple.com/developer-account/#/dev21218dfd6) průvodce. Zřídit aplikace pro použití s aplikačními službami, najdete v tématu [práce s funkcemi](~/ios/deploy-test/provisioning/capabilities/index.md) vodítka.

## <a name="summary"></a>Souhrn

Tato příručka prozkoumat výhody a omezení pro instalaci aplikace na zařízení s iOS pomocí bezplatné zřizování. To poskytuje podrobný návod, který jsme vám ukázali, jak nainstalovat aplikaci Xamarin.iOS pomocí bezplatné zřizování.

## <a name="related-links"></a>Související odkazy

- [Zřizování zařízení](~/ios/get-started/installation/device-provisioning/index.md)
- [Zřizování pro aplikační služby](~/ios/get-started/installation/device-provisioning/index.md#appservices)
