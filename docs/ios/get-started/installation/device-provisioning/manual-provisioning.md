---
title: Ručního zřizování pro Xamarin.iOS
description: Po úspěšné instalaci Xamarin.iOS na další krok v vývoj pro iOS je ke zřízení zařízení s iOS. Tato příručka popisuje nastavení profilů a certifikátů vývoj pomocí ručního zřizování.
ms.prod: xamarin
ms.assetid: E26ACC94-F4A5-4FF5-B7D4-BE596745A665
ms.technology: xamarin-ios
author: asb3993
ms.author: amburns
ms.date: 07/15/2017
ms.openlocfilehash: c0404a1fd8f7e878638b9483c65c637f6b4faa66
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 06/05/2018
ms.locfileid: "34786100"
---
# <a name="manual-provisioning-for-xamarinios"></a>Ručního zřizování pro Xamarin.iOS

_Po úspěšné instalaci Xamarin.iOS na další krok v vývoj pro iOS je ke zřízení zařízení s iOS. Tato příručka popisuje nastavení profilů a certifikátů vývoj pomocí ručního zřizování._

<a name="signingidentity" />

## <a name="creating-a-signing-identity"></a>Vytváření podpisové Identity

Prvním krokem při nastavování zařízení vývoj je vytvoření podpisové identity. Podpisové identity se skládá ze dvou akcí:

- Vývojový certifikát
- Privátní klíč

Vývoj pro certifikáty a související [klíče](#keypairs) jsou důležité pro vývojář iOS: stanovují svoji identitu prostřednictvím Apple a přidružit k dané zařízení a profil pro vývoj, podobají uvedení digitální podpis vaše aplikace. Apple zkontroluje certifikáty k řízení přístupu k zařízení, že jsou povoleny pro nasazení.

Vývojové týmy, certifikáty a profily se dají spravovat přímým přístupem [certifikáty, identifikátory a profily](https://developer.apple.com/account/overview.action) oddílu centra členy společnosti Apple. Apple vyžaduje, abyste tak, aby měl podpisové identity k vytváření kódu pro zařízení ani simulátor.  

> [!IMPORTANT]
> Je důležité si uvědomit, že může mít dva iOS vývoj certifikáty pouze v jednom okamžiku. Pokud potřebujete vytvořit žádné další, musíte se k odvolání některého ze stávajících. Všechny počítače pomocí odvolaném certifikátu nebudou moct přihlásit ke své aplikace.

Chcete vygenerovat podpisovou identitu, postupujte takto:

1. Přihlášení k [identifikátory, certifikátů a profilů části portálu pro vývojáře](https://developer.apple.com/account/overview.action) a vyberte **certifikáty** tématu **aplikací iOS** sloupce. Pak stiskněte tlačítko **+** k vytvoření nového certifikátu:

    [![](manual-provisioning-images/cert-plus.png "Klikněte + vytvořit nový certifikát.")](manual-provisioning-images/cert-plus.png#lightbox)

2. Vyberte **vývoj aplikací pro iOS** možnost pro typ certifikátu a klikněte na tlačítko **pokračovat**. Tato obrazovka se mohou lišit v závislosti na váš účet oprávnění:

    [![](manual-provisioning-images/cert-first.png "Vyberte možnost vývoj aplikací pro typ certifikátu iOS")](manual-provisioning-images/cert-first.png#lightbox)

3. Požadavek žádost o podepsání certifikátu, který bude nahrán do vygenerovat certifikát ručně. To provedete spuštění **přístup do řetězce klíčů** v počítačích Mac. Přejděte do hlavní nabídky a vyberte **certifikací** a **vyžádat certifikát od certifikační autority...** , jak je uvedeno dále:

      [![](manual-provisioning-images/key-first.png "Vyžádání žádosti o podepsání certifikátu")](manual-provisioning-images/key-first.png#lightbox)

4. Vyplňte vaše informace a vyberte možnost **uložit na disk**:

    [![](manual-provisioning-images/key-second.png "Vyplňte vaše informace")](manual-provisioning-images/key-second.png#lightbox)

5. Uložte CSR v umístění, kde ji můžete snadno najít:

    [![](manual-provisioning-images/cert-third.png "Uložit oddělení služeb zákazníkům")](manual-provisioning-images/cert-third.png#lightbox)

6. Vraťte se na portál zřizování, nahrajte certifikát do portálu a odeslat:

    [![](manual-provisioning-images/cert-second.png "Nahrajte certifikát do portálu")](manual-provisioning-images/cert-second.png#lightbox)

    Pokud nemáte oprávnění správce, certifikát musí být schválen agentem správce nebo týmu.

7. Po schválení je certifikát, můžete ji stáhněte z portálu zřizování:

    [![](manual-provisioning-images/status-dev.png "Stažení certifikátu z portálu zřizování")](manual-provisioning-images/status-dev.png#lightbox)

8. Poklikejte na stažený certifikát, který chcete spustit přístup do řetězce klíčů a otevřete **Moje certifikáty** panelech zobrazující nových certifikátů a přidružený privátní klíč:

    [![](manual-provisioning-images/keychain.png "Certifikátu v přístup do řetězce klíčů")](manual-provisioning-images/keychain.png#lightbox)

<a name="keypairs" />

### <a name="understanding-certificate-key-pairs"></a>Principy páry klíčů certifikátů

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

Profil vývojáře obsahuje certifikáty, jejich přidružených klíčů a všechny zřizovacích profilů přidružené k účtu. Existují ve skutečnosti dvě verze profilu vývojáře – jeden je na portálu pro vývojáře a druhý je umístěn v místních počítačích Mac. Rozdíl mezi nimi je typ klíče, které obsahují: _profilu na portálu ve uložený všechny veřejné klíče související s certifikáty, i když bude pro kopii na místním počítači Mac obsahuje všechny privátní klíče_. Pro certifikáty, které budou platit páry klíčů se musí shodovat. Mít zálohu vývojáře profilu v místním systému Mac, protože pokud soukromé klíče jsou ztraceny, všechny certifikáty a zřizovacích profilů musí být znovu vygeneroval.

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Profil vývojáře obsahuje certifikáty, jejich přidružených klíčů a všechny zřizovacích profilů přidružené k účtu. Existují ve skutečnosti dvě verze profilu vývojáře – jeden je na portálu pro vývojáře a druhý je umístěn v počítačích Mac. Rozdíl mezi nimi je typ klíče, které obsahují: _profilu v portálu Domů všechny veřejné klíče související s certifikáty, zatímco kopii na jste jste Mac obsahuje všechny privátní klíče_. Pro certifikáty, které budou platit páry klíčů se musí shodovat. Mít zálohu vývojáře profilu z Mac Xamarin sestavení hostitele, protože pokud soukromé klíče jsou ztraceny, všechny certifikáty a zřizovacích profilů musí být znovu vygeneroval.

-----

> [!WARNING]
> Došlo ke ztrátě certifikát a přidružené klíče může být velmi rušivým zásahům, bude vyžadovat odvolání existujících certifikátů a znovu zřizování všechny přidružené zařízení, včetně těch, které registrované pro ad-hoc nasazení. Po nastavení úspěšně vývoj certifikáty, exportujte záložní kopii a jejich uložení na bezpečné místo. Další informace o tom, jak to udělat, najdete v sekci Export a import certifikátů a profilů [zachování certifikáty](https://developer.apple.com/library/ios/documentation/IDEs/Conceptual/AppDistributionGuide/MaintainingCertificates/MaintainingCertificates.html) průvodce v dokumentaci společnosti Apple.

<a name="provisioning" />

## <a name="provisioning-an-ios-device-for-development"></a>Zřizování pro vývoj pro zařízení se systémem iOS

Teď, když jste vytvořili svoji identitu prostřednictvím Apple a mít vývojový certifikát, musíte vytvořit profil pro zřizování a entitami, vyžaduje, je možné nasadit aplikace do zařízení se systémem Apple. Zařízení musí používat verzi iOS, která podporuje Xcode – bude pravděpodobně nutné aktualizovat zařízení, Xcode nebo obojí.

<a name="adddevice" />

## <a name="add-a-device"></a>Přidat zařízení

Při vytváření profilu pro zřizování pro vývoj, jsme musí stavu zařízení, která můžete spustit aplikace. Chcete-li povolit, až 100 zařízení za kalendářní rok lze přidat na náš portál pro vývojáře a zde jsme můžete vybrat zařízení, která chcete přidat do konkrétní profil pro zřizování. Postupujte podle následujících kroků na počítači Mac pro přidání zařízení na portál pro vývojáře

1. Spusťte Xcode.
2. Připojte zařízení zřídit k počítači Mac s jeho zadaný kabelu USB.
2. Z **Windows** nabídky vyberte možnost **zařízení**:

  [![](manual-provisioning-images/add01.png "Z nabídky systému Windows vyberte zařízení")](manual-provisioning-images/add01.png#lightbox)

3. Vyberte požadovanou iOS zařízení z **zařízení** seznamu na levé straně okna zařízení.
4. Zvýrazněte **identifikátor** řetězec a zkopírujte jej do schránky:

  [![](manual-provisioning-images/add02.png "Zvýrazněte řetězec identifikátoru")](manual-provisioning-images/add02.png#lightbox)

5. V prohlížeči Safari, přejděte na [Apple Developer Center](https://developer.apple.com/membercenter/index.action) a přihlaste se.
6. Klikněte **certifikáty, identifikátory a profily** odkaz:

  [![](manual-provisioning-images/add03.png "Klikněte na tlačítko certifikátů, profily identifikátory propojení")](manual-provisioning-images/add03.png#lightbox)

7. Klikněte na **zařízení** odkaz:

  [![](manual-provisioning-images/add04.png "Klikněte na odkaz zařízení")](manual-provisioning-images/add04.png#lightbox)

8. Klikněte **+** tlačítko:

  [![](manual-provisioning-images/add05.png "Klikněte tlačítko +")](manual-provisioning-images/add05.png#lightbox)

9. Zadejte název nové zařízení a vložte zařízení **identifikátor** kterou jsme zkopírovali, do výše **UUID** pole:

  [![](manual-provisioning-images/add06.png "Zadejte název nové zařízení a identifikátor zařízení")](manual-provisioning-images/add06.png#lightbox)

10. Klikněte **pokračovat** tlačítko.
11. Nakonec zkontrolujte zadané informace a klikněte **zaregistrovat** tlačítko:

  [![](manual-provisioning-images/add07.png "Přečtěte si informace")](manual-provisioning-images/add07.png#lightbox)

Výše uvedené kroky opakujte pro jakékoli zařízení s iOS, který se použije k otestování nebo ladění aplikace pro Xamarin.iOS.

Po přidání zařízení do portálu pro vývojáře, je nutné vytvořit profil pro zřizování a přidání zařízení.


<a name="provisioningprofile" />

## <a name="creating-a-development-provisioning-profile"></a>Vytváření vývojovém profil pro zřizování

Jako s certifikátem vývoj profily zřizování můžete ručně vytvořit pomocí [certifikáty, identifikátory a profily](https://developer.apple.com/account/overview.action) oddílu centra členy společnosti Apple.

Před vytvořením profilu pro zřizování, *ID aplikace* musí být provedeny. ID aplikace je řetězec styl zpětného DNS, který jednoznačně identifikuje aplikaci. Těchto kroků se ukazují, jak vytvořit **ID aplikace zástupné**, které lze použít k sestavení a instalaci většiny aplikací. **Explicitní ID aplikace** pouze povolit instalaci jednu aplikaci (s odpovídajícím ID sady) a jsou obecně používány pro určité funkce iOS například dotykový identifikátor a HealthKit. Informace o vytváření explicitní ID aplikace, najdete v části [práce s možností](~/ios/deploy-test/provisioning/capabilities/index.md) průvodce.

### <a name="app-id"></a>ID aplikace

1. V [portál pro vývojáře](https://developer.apple.com/account/overview.action) vyhledejte *certifikát, identifikátory a profily* části v Apple Developer Center. Vyberte **ID aplikace** pod **identifikátory**.
2. Klikněte **+** tlačítko a zadejte **název**:

    [![](manual-provisioning-images/appid05a.png "Zadejte název")](manual-provisioning-images/appid05a.png#lightbox)
3. Předpona aplikace by měla přednastavení. Vyberte **ID aplikace zástupné** pro tuto příponu aplikace. Zadejte ID sady ve formátu `com.[DomainName].*`:

  [![](manual-provisioning-images/appid05b.png "Zadejte ID sady")](manual-provisioning-images/appid05b.png#lightbox)

3. Klikněte **pokračovat** tlačítko a postupujte podle pokynů na obrazovce pro vytvoření nového ID aplikace.

### <a name="provisioning-profile"></a>Profil pro zřizování

Po vytvoření ID aplikace, je možné vytvořit profil zřizování. Tento profil zřizování obsahuje informace o *co* aplikace (nebo aplikace, pokud je ID aplikace zástupný znak) tento profil má vztah k, *kdo* profilu (v závislosti na tom, jaké vývojáře certifikáty přidají), můžete použít. a *co* zařízení můžou instalovat aplikace.

Chcete-li ručně vytvořit profil pro zřizování pro vývoj, postupujte takto:

1. Používat prohlížeč Safari a přejděte do [centra pro vývojáře Apple](https://developer.apple.com/membercenter/index.action)a v části *certifikáty, identifikátory a profily* vyberte profily zřizování.
2. Klikněte **+** tlačítko, v pravém horním rohu na vytvoření nového profilu.
3. Z **vývoj** vyberte přepínač vedle **vývoj aplikací pro iOS**a stiskněte klávesu **pokračovat**:

    [![](manual-provisioning-images/provisioning-profile01.png "Vyberte typ profilu k vytvoření")](manual-provisioning-images/provisioning-profile01.png#lightbox)
4. Z rozevírací nabídky ID vyberte aplikaci, použijte:

    [![](manual-provisioning-images/provisioning-profile02.png "Vyberte aplikaci ID, který chcete použít")](manual-provisioning-images/provisioning-profile02.png#lightbox)
5. Vyberte certifikáty, které chcete zahrnout do profilu pro zřizování a stiskněte klávesu **pokračovat**:

    [![](manual-provisioning-images/provisioning-profile03.png "Vyberte certifikáty, které zahrnují v profilu pro zřizování")](manual-provisioning-images/provisioning-profile03.png#lightbox)
6. Vyberte všechna zařízení, která aplikace bude nainstalována na.

    [![](manual-provisioning-images/provisioning-profile04.png "Vyberte všechna zařízení, která aplikace bude nainstalována na")](manual-provisioning-images/provisioning-profile04.png#lightbox)
7. Zadejte profil zřizování s osobní název a stiskněte klávesu **pokračovat** chcete vytvořit profil:

    [![](manual-provisioning-images/provisioning-profile05.png "Zadejte profil zřizování s osobní název")](manual-provisioning-images/provisioning-profile05.png#lightbox)
8. Stiskněte klávesu **Stáhnout** ke stažení profilu pro zřizování na Macu:

    [![](manual-provisioning-images/provisioning-profile06.png "Stažení profilu pro zřizování")](manual-provisioning-images/provisioning-profile06.png#lightbox)
9. Poklikejte na soubor k instalaci profilu pro zřizování v Xcode. Všimněte si, že Xcode nemusí zobrazit, že všechny visual clues, aby nainstaloval profil s výjimkou otevírání. To můžete ověřit procházením **Xcode > Předvolby > účty**. Vyberte svoje Apple ID a klikněte na **zobrazit podrobnosti...** . Nový profil pro zřizování by měl být uvedený, jak je uvedeno dále:

      [![](manual-provisioning-images/provisioning-profile07.png "Zobrazení profil v Xcode")](manual-provisioning-images/provisioning-profile07.png#lightbox)

Po úspěšném vytvoření profilu pro zřizování může být nutné obnovit Xcode tak, aby všechny certifikáty vývoj jsou k dispozici pro Visual Studio pro Mac a Visual Studio.

<a name="download" />

## <a name="downloading-profiles-and-certificates-in-xcode"></a>Stahování profilů a certifikátů v Xcode

Certifikáty a zřizovacích profilů, které byly vytvořeny v portálu pro vývojáře Apple nebude automaticky v Xcode. Proto může být potřeba stáhnout, takže se, že lze k nim Visual Studio pro Mac a Visual Studio. Aktualizovat a stáhnout všechny certifikáty, vytvořit na portálu pro vývojáře Apple, postupujte takto:

1.   Ukončete sady Visual Studio pro Mac nebo Visual Studio.
2.   Spusťte Xcode.
3.   Zvolte **Xcode nabídky > Předvolby...**
4.   Klikněte **účty** kartě.
5.   Vyberte tým a klikněte na tlačítko **stáhnout profily ruční** tlačítko: [ ![ ] (manual-provisioning-images/selectteam1.png "stahování ruční profily")](manual-provisioning-images/selectteam1.png#lightbox)

6.   Ukončete Xcode.
7.  Spuštění sady Visual Studio pro Mac nebo Visual Studio.

Nové certifikáty nebo zřizovacích profilů budou k dispozici v sadě Visual Studio pro Mac nebo Visual Studio a připravené k použití.

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

> [!IMPORTANT]
> Může být potřeba zastavte a restartujte Visual Studio pro Mac, než ho bude vidět žádné nové nebo upravené certifikáty nebo profily aktualizovat Xcode.

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

> [!IMPORTANT]
> Může být potřeba zastavte a restartujte Visual Studio, než ho bude vidět žádné nové nebo upravené certifikáty nebo profily aktualizovat Xcode.

-----

<a name="appservices" />

## <a name="provisioning-for-application-services"></a>Zřizování pro aplikační služby

Apple poskytuje výběr speciální aplikační služby, označované taky jako funkce, které může být aktivovaný pro aplikace pro Xamarin.iOS. Tyto aplikace služby je nutné nakonfigurovat na obou iOS Provisioning Portal při **ID aplikace** je vytvořena a v **Entitlements.plist** souboru, který je součástí projektu aplikace pro Xamarin.iOS. Informace o přidání aplikačních služeb do vaší aplikace, najdete v části [Úvod k funkcím](~/ios/deploy-test/provisioning/capabilities/index.md) průvodce a [práce oprávnění](~/ios/deploy-test/provisioning/entitlements.md) průvodce.

* Vytvoření ID aplikace se službami požadovaná aplikace.
* Vytvořte novou [profil pro zřizování](#provisioningprofile) obsahující číslem ID této aplikace.
* Nastavit oprávnění v projektu Xamarin.iOS

<a name="deploy" />

## <a name="deploying-to-a-device"></a>Nasazení do zařízení

V tomto okamžiku zřizování by měly být dokončené, a aplikace je připravená k nasazení do zařízení. Chcete-li to provést, postupujte následujícím způsobem:

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

> [!IMPORTANT]
> Než začnete, je nutné vybrat **ručního zřizování** v **Info.plist**.

1. Připojit zařízení k macu.
2. V projektu **Info.plist**, zajistěte, aby identifikátor balíčku shoduje ID aplikace (Pokud ID aplikace není zástupný znak):

  ![](manual-provisioning-images/deploydevice01xs.png "Zadat identifikátor")

3. Klikněte pravým tlačítkem na projekt, který má zobrazit dialogové okno Možnosti projektu a přejděte do **sestavení > iOS podepisování sady**. Z rozevíracího seznamu vedle i **identitu podepisování** a **profil zřizování**, ověřte, zda lze Visual Studio pro Mac najdete v části správné profily a vyberte konkrétní identity & profil:

  ![](manual-provisioning-images/deploydevice02xs.png "Vyberte konkrétní identity & profilu")

Pokud je nastavena v **automatické**, Visual Studio pro Mac vybere identity a profil na základě ID sady, které bylo nastaveno v kroku #2.

4. Nezapomeňte si nastavte konfiguraci sestavení na **iPhone** / **iPad**, místo simulátoru.
5. Klikněte na tlačítko **spustit** v sadě Visual Studio pro Mac a zobrazit aplikaci spuštěnou na zařízení.

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

> [!IMPORTANT]
> Než začnete, je nutné vybrat **ručního zřizování** v **Projekt > zřizování vlastnosti...** .

1. Připojte zařízení k sestavení hostitele Mac.
2. V projektu **Info.plist**, zajistěte, aby identifikátor balíčku shoduje ID aplikace:

  ![](manual-provisioning-images/servicevs01.png "Zadat identifikátor")

3. Klikněte pravým tlačítkem na projekt se zobrazí dialogové okno Možnosti projektu a přejděte do **sestavení > iOS podepisování sady**. Z rozevíracího seznamu vedle i **identitu podepisování** a **profil zřizování** ověřte, zda můžete sady Visual Studio najdete v části správné profily a vyberte konkrétní identity & profilu.

    Pokud je nastavena v **automatické**, Visual Studio vybere identity a profil na základě ID sady, které bylo nastaveno v kroku #2.

4. Nezapomeňte si nastavte konfiguraci sestavení na **iPhone** nebo **iPad**, místo simulátoru.
5. Klikněte na tlačítko **spustit** v sadě Visual Studio a zobrazit aplikaci spuštěnou na zařízení.


-----

## <a name="summary"></a>Souhrn

Tato příručka popsané kroky potřebné k nastavení vývojového prostředí pro Xamarin.iOS. Ho prozkoumali, jak aplikace je kód podepsaný s informacemi o vývojář, jejich tým, zařízení, které aplikace můžou běžet na a id jednotlivých aplikací.


## <a name="related-links"></a>Související odkazy

- [Bezplatné zřizování](~/ios/get-started/installation/device-provisioning/free-provisioning.md)
- [Distribuce aplikace](~/ios/deploy-test/app-distribution/index.md)
- [Odstraňování potíží](~/ios/deploy-test/troubleshooting.md)
- [Apple – průvodci distribucí aplikace](https://developer.apple.com/library/ios/documentation/IDEs/Conceptual/AppDistributionGuide/Introduction/Introduction.html)
