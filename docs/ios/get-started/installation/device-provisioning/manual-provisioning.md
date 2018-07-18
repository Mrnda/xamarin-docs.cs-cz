---
title: Ruční zřizování pro Xamarin.iOS
description: Po úspěšné instalaci Xamarin.iOS na další krok ve vývoji pro iOS je ke zřízení zařízení s Iosem. Tato příručka se věnuje nastavení vývoje certifikátů a profilů pomocí ruční zřizování.
ms.prod: xamarin
ms.assetid: E26ACC94-F4A5-4FF5-B7D4-BE596745A665
ms.technology: xamarin-ios
author: asb3993
ms.author: amburns
ms.date: 07/15/2017
ms.openlocfilehash: dd0afe03adbd021717a88cd4409e3e1351ba9b50
ms.sourcegitcommit: e98a9ce8b716796f15de7cec8c9465c4b6bb2997
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/18/2018
ms.locfileid: "39111183"
---
# <a name="manual-provisioning-for-xamarinios"></a>Ruční zřizování pro Xamarin.iOS

_Po úspěšné instalaci Xamarin.iOS na další krok ve vývoji pro iOS je ke zřízení zařízení s Iosem. Tato příručka se věnuje nastavení vývoje certifikátů a profilů pomocí ruční zřizování._

> [!NOTE]
> Pokyny na této stránce jsou relevantní pro vývojáře, kteří zaplatili přístup k programu Apple Developer. Pokud máte bezplatný účet, prosím podívejte se na [bezplatné zřizování](~/ios/get-started/installation/device-provisioning/free-provisioning.md) Průvodce pro další informace o testování v zařízení.

## <a name="creating-a-signing-identity"></a>Vytváření Podpisová identita

Prvním krokem při nastavování zařízení vývoje je vytvořit podpisovou identitu. Podpisová identita se skládá ze dvou kroků:

- Certifikát pro vývoj
- Privátní klíč

Vývoj pro certifikáty a související [klíče](#understanding-certificate-key-pairs) jsou důležité pro vývojáře iOS: vytvořit svoji identitu prostřednictvím Apple a přidružit k dané zařízení a profil pro vývoj, podobají uvedení digitální podpis vaše aplikace. Apple vyhledává certifikáty pro řízení přístupu k zařízení jsou povoleny pro nasazení.

Vývojové týmy, certifikátů a profilů může spravovat přístup k [certifikáty, identifikátory & profily](https://developer.apple.com/account/overview.action) oddílu (vyžaduje se přihlášení) z centra společnosti Apple. Apple vyžaduje podpisovou identitu pro sestavení svého kódu pro zařízení nebo simulátor.  

> [!IMPORTANT]
> Je důležité si uvědomit, že máte jenom dva iOS Development certifikáty v daný okamžik. Pokud je potřeba vytvořit žádné další, je potřeba zrušit existující. Jakýkoli počítač použitím odvolaného certifikátu nebude možné podepsat své aplikace.

Ke generování podpisovou identitu, postupujte takto:

1. Přihlaste se k [profily certifikátů, identifikátory a části portálu pro vývojáře](https://developer.apple.com/account/overview.action) a vyberte **certifikáty** v článku **aplikací pro iOS** sloupce. Pak, klikněte na tlačítko **+** k vytvoření nového certifikátu:

    [![](manual-provisioning-images/cert-plus.png "Klikněte + vytvořit nový certifikát")](manual-provisioning-images/cert-plus.png#lightbox)

2. Vyberte **vývoj aplikací pro iOS** možnost pro typ certifikátu a klikněte na tlačítko **pokračovat**. Tato obrazovka se mohou lišit v závislosti na vašich oprávnění k účtu:

    [![](manual-provisioning-images/cert-first.png "Vyberte možnost vývoj aplikací pro typ certifikátu iOS")](manual-provisioning-images/cert-first.png#lightbox)

3. Požadavek žádost o podepsání certifikátu, který se nahraje k vygenerování certifikátu ručně. Chcete-li to provést, spusťte **přístup do řetězce klíčů** v počítačích Mac. Přejděte do hlavní nabídky a vyberte **Průvodce certifikací** a **vyžádat certifikát od certifikační autority...** , jak je znázorněno níže:

      [![](manual-provisioning-images/key-first.png "Požádat o žádosti o podepsání certifikátu")](manual-provisioning-images/key-first.png#lightbox)

4. Vyplňte svoje informace a vyberte možnost **uložit na disk**:

    [![](manual-provisioning-images/key-second.png "Vyplňte svoje informace")](manual-provisioning-images/key-second.png#lightbox)

5. Uložte žádost o podepsání certifikátu v místě, kde je možné ji snadno najít:

    [![](manual-provisioning-images/cert-third.png "Uložit žádost o podepsání certifikátu")](manual-provisioning-images/cert-third.png#lightbox)

6. Vraťte se na portál zřizování, nahrajte certifikát na portál a odeslat:

    [![](manual-provisioning-images/cert-second.png "Nahrajte certifikát do portálu")](manual-provisioning-images/cert-second.png#lightbox)

    Pokud nemáte oprávnění správce, certifikát musí schválit agenta správce nebo týmu.

7. Po schválení certifikátu si ho stáhněte z portálu pro zřizování:

    [![](manual-provisioning-images/status-dev.png "Stáhněte si certifikát z portálu pro zřizování")](manual-provisioning-images/status-dev.png#lightbox)

8. Poklikejte na stažený certifikát, který chcete spustit přístup do řetězce klíčů a otevřete **mé certifikáty** panel zobrazuje nové certifikáty a přidružený privátní klíč:

    [![](manual-provisioning-images/keychain.png "Certifikátu v nástroji Keychain Access")](manual-provisioning-images/keychain.png#lightbox)

### <a name="understanding-certificate-key-pairs"></a>Principy páry klíčů certifikátu

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

Profil vývojáře obsahuje certifikáty, jejich přidružených klíčů a žádné zřizovací profily, které jsou přidružené k účtu. Jsou ve skutečnosti dvě verze profilu pro vývojáře – jedna je na portálu pro vývojáře a druhý se nachází na místním počítači Mac. Typ klíče, které obsahují je rozdíl mezi těmito dvěma: _profil na portálu jsou uloženy všechny veřejné klíče související s certifikáty, zatímco kopírování na místním počítači Mac obsahuje privátní klíče_. Pro certifikáty, které budou platit páry klíčů se musí shodovat. Mít zálohu profil pro vývojáře na místním počítači Mac, protože pokud soukromé klíče jsou ztraceny, certifikáty a zřizovací profily bude nutné ho znovu vygenerovat.

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Profil vývojáře obsahuje certifikáty, jejich přidružených klíčů a žádné zřizovací profily, které jsou přidružené k účtu. Jsou ve skutečnosti dvě verze profilu pro vývojáře – jedna je na portálu pro vývojáře a druhý se nachází v počítačích Mac. Typ klíče, které obsahují je rozdíl mezi těmito dvěma: _profil na portálu Domů, všechny veřejné klíče související s certifikáty, zatímco kopie na jste už Mac obsahuje privátní klíče_. Pro certifikáty, které budou platit páry klíčů se musí shodovat. Zabraňte zálohování profilu pro vývojáře Mac Xamarin sestavení hostitele, vzhledem k tomu, že pokud soukromé klíče jsou ztraceny, certifikáty a zřizovací profily bude nutné ho znovu vygenerovat.

-----

> [!WARNING]
> Došlo ke ztrátě certifikátu a přidružené klíče může být mimořádně rušivé, bude vyžadovat odvolání existujících certifikátů a znovu zřizování žádná přidružená zařízení, včetně těch, které registrované pro ad-hoc nasazení. Po úspěšném nastavení vývoje certifikáty, záložní kopii exportovat a uložit na bezpečném místě. Další informace o tom, jak to provést, najdete v části Export a import certifikátů a profilů [udržování certifikátů](https://developer.apple.com/library/ios/documentation/IDEs/Conceptual/AppDistributionGuide/MaintainingCertificates/MaintainingCertificates.html) Průvodce dokumentace společnosti Apple.

<a name="provisioning" />

## <a name="provisioning-an-ios-device-for-development"></a>Zřizování zařízení pro vývoj se systémem iOS

Teď, když jste vytvořili svou identitu pomocí Apple a mít certifikát pro vývoj, musíte vytvořit zřizovací profil a požadované entity tak, aby byl možný k nasazení aplikace do zařízení Apple. Zařízení musí používat verzi iOS, která podporuje Xcode, může být nutné aktualizovat zařízení, Xcode nebo obojí.

<a name="adddevice" />

## <a name="add-a-device"></a>Přidat zařízení

Při vytváření profilu zřizování pro vývoj, jsme musí stanovit, zařízení, která může aplikaci spustit. Chcete-li povolit, až 100 zařízení za kalendářní rok lze přidat na náš portál pro vývojáře a tady můžeme vybrat zařízení mají být přidány do konkrétní zřizovacího profilu. Proveďte následující postup na počítači Mac pro přidání zařízení na portál pro vývojáře

1. Spuštění Xcode.
2. Připojení zařízení dá zřídit k Macu pomocí jeho zadaný kabelu USB.
2. Z **Windows** nabídky vyberte možnost **zařízení**:

  [![](manual-provisioning-images/add01.png "V nabídce Windows vyberte zařízení")](manual-provisioning-images/add01.png#lightbox)

3. Vyberte zařízení s Iosem požadované z **zařízení** seznamu na levé straně okna zařízení.
4. Zvýrazněte **identifikátor** řetězců a zkopírujte do schránky:

  [![](manual-provisioning-images/add02.png "Zvýrazněte identifikátor řetězce")](manual-provisioning-images/add02.png#lightbox)

5. V prohlížeči Safari, přejděte [Apple Developer Center](https://developer.apple.com/membercenter/index.action) a přihlaste se.
6. Klikněte na tlačítko **certifikáty, identifikátory & profily** odkaz:

  [![](manual-provisioning-images/add03.png "Klikněte na tlačítko certifikátů, profily identifikátory propojení")](manual-provisioning-images/add03.png#lightbox)

7. Klikněte na **zařízení** odkaz:

  [![](manual-provisioning-images/add04.png "Klikněte na odkaz na zařízení")](manual-provisioning-images/add04.png#lightbox)

8. Klikněte na tlačítko **+** tlačítka:

  [![](manual-provisioning-images/add05.png "Klikněte tlačítko +")](manual-provisioning-images/add05.png#lightbox)

9. Zadejte název nové zařízení a vložte zařízení **identifikátor** , který jsme do zkopírovali výše **UUID** pole:

  [![](manual-provisioning-images/add06.png "Zadejte název nové zařízení a zařízení identifikátor")](manual-provisioning-images/add06.png#lightbox)

10. Klikněte na tlačítko **pokračovat** tlačítko.
11. A konečně, přečtěte si informace a klikněte na tlačítko **zaregistrovat** tlačítka:

  [![](manual-provisioning-images/add07.png "Přečtěte si informace")](manual-provisioning-images/add07.png#lightbox)

Opakujte předchozí postup pro jakékoli zařízení s iOS, která se použije k testování nebo ladění aplikace pro Xamarin.iOS.

Po přidání zařízení do portálu pro vývojáře, je potřeba vytvořit zřizovací profil a přidejte zařízení.

<a name="provisioningprofile" />

## <a name="creating-a-development-provisioning-profile"></a>Vytváření vývoj zřizovací profil

Jako vývoj certifikátem, zřizovací profily můžete ručně vytvořit prostřednictvím [certifikáty, identifikátory & profily](https://developer.apple.com/account/overview.action) části Členové Center společnosti Apple.

Před vytvořením zřizovacího profilu *ID aplikace* musí být provedeny. ID aplikace je řetězec ve stylu reverzní DNS, který jednoznačně identifikuje aplikaci. Budou následující kroky ukazují, jak vytvořit **ID aplikace zástupný znak**, který slouží k sestavení a instalaci většiny aplikací. **Explicitní ID aplikace** pouze povolit instalaci jedné aplikace (s odpovídajícím ID sady) a jsou obecně používány pro určité funkce iOS, třeba oprávnění Apple Pay a HealthKit. Informace o vytvoření explicitní ID aplikace, najdete [práce s funkcemi](~/ios/deploy-test/provisioning/capabilities/index.md) průvodce.

### <a name="app-id"></a>ID aplikace

1. V [portál pro vývojáře](https://developer.apple.com/account/overview.action) přejděte *profily certifikátů, identifikátory a* oddílu v Centru pro vývojáře Apple. Vyberte **App ID** pod **identifikátory**.
2. Klikněte na tlačítko **+** tlačítko a zadejte **název**:

    [![](manual-provisioning-images/appid05a.png "Zadejte název")](manual-provisioning-images/appid05a.png#lightbox)
3. Předpona, která aplikace by měla předvolby. Vyberte **ID aplikace zástupný znak** pro tuto příponu aplikace. Zadejte ID sady prostředků ve formátu `com.[DomainName].*`:

  [![](manual-provisioning-images/appid05b.png "Zadejte ID sady prostředků")](manual-provisioning-images/appid05b.png#lightbox)

3. Klikněte na tlačítko **pokračovat** tlačítko a postupovat podle pokynů na obrazovce vytvořte nové ID aplikace.

### <a name="provisioning-profile"></a>Zřizovací profil

Po vytvoření ID aplikace je možné vytvořit zřizovací profil. Tento profil zřizování obsahuje informace o *co* (nebo více aplikací, pokud je ID aplikace zástupný znak) tento profil se týká, *kdo* profilu (v závislosti na tom, co vývojář certifikáty se přidávají), můžete použít. a *co* zařízení můžou instalovat aplikace.

Chcete-li ručně vytvořit zřizovací profil pro vývoj, postupujte takto:

1. Používat prohlížeč Safari a přejděte do [člen středisko pro vývojáře Apple](https://developer.apple.com/membercenter/index.action)a v části *certifikáty, identifikátory & profily* vyberte zřizovací profily.
2. Klikněte na tlačítko **+** tlačítko v pravém horním rohu na vytvoření nového profilu.
3. Z **vývoj** vyberte přepínač vedle **vývoj aplikací pro iOS**a stiskněte klávesu **pokračovat**:

    [![](manual-provisioning-images/provisioning-profile01.png "Vyberte typ vytvoření profilu")](manual-provisioning-images/provisioning-profile01.png#lightbox)
4. Z rozevírací nabídky vyberte aplikaci ID, aby používal:

    [![](manual-provisioning-images/provisioning-profile02.png "Vyberte aplikaci, aby používal ID")](manual-provisioning-images/provisioning-profile02.png#lightbox)
5. Vyberte certifikáty, které chcete zahrnout profil pro zřizování a stisknutím klávesy **pokračovat**:

    [![](manual-provisioning-images/provisioning-profile03.png "Vyberte certifikáty, které chcete zahrnout do zřizovacího profilu")](manual-provisioning-images/provisioning-profile03.png#lightbox)
6. Vyberte všechny, které aplikace se nainstaluje na zařízení.

    [![](manual-provisioning-images/provisioning-profile04.png "Vyberte zařízení, které aplikace se nainstaluje na")](manual-provisioning-images/provisioning-profile04.png#lightbox)
7. Poskytnout zřizovací profil snadno rozpoznatelné název a stiskněte klávesu **pokračovat** chcete vytvořit profil:

    [![](manual-provisioning-images/provisioning-profile05.png "Zadejte profil zřizování s snadno rozpoznatelné název")](manual-provisioning-images/provisioning-profile05.png#lightbox)
8. Stisknutím klávesy **Stáhnout** stáhnout zřizovací profil na počítači Mac:

    [![](manual-provisioning-images/provisioning-profile06.png "Stáhnout profil zřizování")](manual-provisioning-images/provisioning-profile06.png#lightbox)
9. Poklikejte na soubor k instalaci profilu zřizování v prostředí Xcode. Všimněte si, že Xcode nemusí zobrazit, že všechny vizuály clues, aby nainstaloval profil s výjimkou počáteční. Můžete to ověřit tak, že přejdete do **Xcode > Předvolby > účty**. Vyberte vaše Apple ID a klikněte na tlačítko **zobrazit podrobnosti...** . Nový profil zřizování by měly být uvedeny, jak je znázorněno níže:

      [![](manual-provisioning-images/provisioning-profile07.png "Zobrazení profil v Xcode")](manual-provisioning-images/provisioning-profile07.png#lightbox)

Po úspěšném vytvoření zřizovacího profilu může být nutné aktualizovat Xcode tak, že všechny certifikáty vývoj jsou k dispozici se sadou Visual Studio pro Mac a Visual Studio.

<a name="download" />

## <a name="downloading-profiles-and-certificates-in-xcode"></a>Stahování profilů a certifikátů v Xcode

Certifikáty a zřizovací profily, které byly vytvořeny na portálu pro vývojáře Apple nebude automaticky v Xcode. Proto může být nutné si je stáhnout tak, že jsou dostupné ve Visual Studio pro Mac a Visual Studio. Chcete-li aktualizovat a stáhnout všechny certifikáty vytvořené na portálu pro vývojáře Apple, proveďte následující:

1.   Ukončení sady Visual Studio pro Mac nebo Visual Studio.
2.   Spuštění Xcode.
3.   Zvolte **Xcode nabídky > Předvolby...**
4.   Klikněte na tlačítko **účty** kartu.
5.   Vyberte tým a klikněte na tlačítko **profily ruční stažení** tlačítko: [ ![ ] (manual-provisioning-images/selectteam1.png "stahování ruční profily")](manual-provisioning-images/selectteam1.png#lightbox)

6.   Ukončete Xcode.
7.  Spusťte sadu Visual Studio pro Mac nebo Visual Studio.

Nové certifikáty nebo zřizovací profily bude k dispozici v sadě Visual Studio pro Mac nebo Visual Studio a připravené k použití.

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

> [!IMPORTANT]
> Může být potřeba zastavit a restartovat Visual Studio pro Mac, předtím, než se zobrazí všechny nové nebo upravené certifikáty nebo aktualizovat Xcode profily.

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

> [!IMPORTANT]
> Může být potřeba zastavit a restartovat aplikaci Visual Studio, než se zobrazí všechny nové nebo upravené certifikáty nebo aktualizovat Xcode profily.

-----

<a name="appservices" />

## <a name="provisioning-for-application-services"></a>Zřizování pro aplikační služby

Apple nabízí celou řadu zvláštní aplikační služby, také nazývané možnosti, které můžete aktivovat pro aplikace pro Xamarin.iOS. Tyto aplikace služby musí být nakonfigurovaná na obou iOS Provisioning Portal při **ID aplikace** se vytvoří a **do souboru Entitlements.plist** souboru, který je součástí projektu aplikace pro Xamarin.iOS. Informace o přidání aplikačních služeb do vaší aplikace [Úvod k možnostem](~/ios/deploy-test/provisioning/capabilities/index.md) průvodce a [práce s nároky](~/ios/deploy-test/provisioning/entitlements.md) průvodce.

* Vytvoření ID aplikace se službami požadovanou aplikaci.
* Vytvořte nový [zřizovací profil](#provisioningprofile) , který obsahuje ID této aplikace.
* Nastavit oprávnění v projektu Xamarin.iOS

## <a name="deploying-to-a-device"></a>Nasazení do zařízení

V tomto okamžiku zřizování by měl být úplný a aplikace je připravená k nasazení do zařízení. Chcete-li to provést, postupujte podle kroků níže:

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

> [!IMPORTANT]
> Než začnete, ujistěte se, že k výběru **ručního zřizování** v **Info.plist**.

1. Připojte zařízení k macu
2. V projektu **Info.plist**, ujistěte se, že identifikátor sady odpovídá ID aplikace (Pokud je ID aplikace není zástupný znak):

  ![](manual-provisioning-images/deploydevice01xs.png "Zadání identifikátoru")

3. Klikněte pravým tlačítkem na projekt zobrazit dialogové okno Možnosti projektu a přejděte do **sestavení > podepsání sady prostředků aplikace pro iOS**. Z rozevíracího seznamu vedle i **Podpisová identita** a **zřizovací profil**, ověřte, že Visual Studio for Mac můžete vidět správné profily a vybrat konkrétní identitu & profilu:

  ![](manual-provisioning-images/deploydevice02xs.png "Vyberte konkrétní identitu & profil")

Pokud je nastavené na **automatické**, Visual Studio pro Mac vybere identitu a profil na základě ID sady prostředků, která byla nastavena v kroku #2.

4. Ujistěte se, že nastavte konfiguraci sestavení **iPhone** / **iPad**, namísto simulátoru.
5. Klikněte na tlačítko **spustit** v sadě Visual Studio pro Mac a zobrazte aplikaci spuštěnou na zařízení.

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

> [!IMPORTANT]
> Než začnete, ujistěte se, že k výběru **ručního zřizování** v **projektu > Vlastnosti zřizování...** .

1. Připojení zařízení k hostiteli buildu Mac.
2. V projektu **Info.plist**, ujistěte se, že identifikátor sady odpovídá ID aplikace:

  ![](manual-provisioning-images/servicevs01.png "Zadání identifikátoru")

3. Klikněte pravým tlačítkem na projekt, chcete-li zobrazit dialogové okno Možnosti projektu a přejděte do **sestavení > podepsání sady prostředků aplikace pro iOS**. Z rozevíracího seznamu vedle i **Podpisová identita** a **zřizovací profil** ověřte, že Visual Studio lze zobrazit správné profily a vybrat konkrétní identitu & profil.

    Pokud je nastavené na **automatické**, Visual Studio vybere identitu a profil na základě ID sady prostředků, která byla nastavena v kroku #2.

4. Ujistěte se, že nastavte konfiguraci sestavení **iPhone** nebo **iPad**, namísto simulátoru.
5. Klikněte na tlačítko **spustit** v sadě Visual Studio a zobrazte aplikaci spuštěnou na zařízení.


-----

## <a name="summary"></a>Souhrn

Tato příručka popsané kroky potřebné k nastavení vývojového prostředí pro Xamarin.iOS. To prozkoumat, jak aplikace je kódu podepsanému pomocí informací o vývojáře, jejich tým, zařízení, které aplikace můžou běžet na a id jednotlivých aplikací.

## <a name="related-links"></a>Související odkazy

- [Bezplatné zřizování](~/ios/get-started/installation/device-provisioning/free-provisioning.md)
- [Distribuce aplikace](~/ios/deploy-test/app-distribution/index.md)
- [Odstraňování potíží](~/ios/deploy-test/troubleshooting.md)
- [Apple – průvodci distribucí aplikace](https://developer.apple.com/library/ios/documentation/IDEs/Conceptual/AppDistributionGuide/Introduction/Introduction.html)
