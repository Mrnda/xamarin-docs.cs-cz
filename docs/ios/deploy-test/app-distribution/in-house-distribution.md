---
title: Interní distribuci pro aplikace Xamarin.iOS
description: Tento dokument poskytuje stručný přehled distribuce aplikací jako členem programu Apple Developer Enterprise interně.
ms.prod: xamarin
ms.assetid: 9466E51E-303E-466E-85D7-D0525E16BB37
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/19/2017
ms.openlocfilehash: 1dff0e614943805930cf7d838110c4a42eee6f48
ms.sourcegitcommit: 51c274f37369d8965b68ff587e1c2d9865f85da7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/30/2018
ms.locfileid: "39353201"
---
# <a name="in-house-distribution-for-xamarinios-apps"></a>Interní distribuci pro aplikace Xamarin.iOS

_Tento dokument poskytuje stručný přehled distribuce aplikací jako členem programu Apple Developer Enterprise interně._

Jakmile aplikace Xamarin.iOS byly vyvinuty, dalším krokem v životního cyklu vývoje softwaru je distribuovat aplikace pro uživatele. Vlastní aplikace mohou být distribuovány *interní* (dříve označované jako organizace) prostřednictvím **Apple Developer Enterprise Program**, který nabízí následující výhody:

- Vaše aplikace nemusí být odeslána k revizi společností Apple.
- Nejsou žádná omezení množství zařízení, na které můžete nasadit aplikace
    - Je důležité si uvědomit, že Apple umožňuje zcela jasné, že jsou interní aplikace pouze pro interní použití.

Je také důležité si uvědomit, že Enterprise Program:

- Neposkytuje přístup do služby iTunes Connect pro distribuci nebo testování (včetně testovacího prostředí).
- Náklady na členství jsou 299 $ za rok.

Všechny aplikace stále musí být podepsaný společností Apple.

<a name="testing" />

## <a name="testing-your-application"></a>Testování aplikace

Testování aplikace se provádí pomocí Ad Hoc distribuci. Další informace o testování, postupujte podle kroků v [Ad Hoc distribuci](~/ios/deploy-test/app-distribution/ad-hoc-distribution.md) průvodce. Mějte na paměti, že je možné pouze testovat na maximálně 100 zařízení.

<a name="setup" />

## <a name="getting-set-up-for-distribution"></a>Načítání nastavení pro distribuci

Stejně jako u jiných programů Apple Developer, v rámci programu Apple Developer Enterprise Program, můžete vytvořit pouze správci týmu a agentů distribuci certifikátů a profilů zřizování.

Certifikáty Apple Developer Enterprise Program bude trvat tři roky a zřizovací profily vyprší po jednom roce.

Je důležité si uvědomit, že certifikáty s vypršenou platností nelze obnovit, a místo toho budete muset nahradit novou, jak je uvedeno certifikát s prošlou platností [níže](#certificate).

<a name="certificate" />

## <a name="creating-a-distribution-certificate"></a>Vytváří se certifikát distribuce

1. Přejděte *certifikáty, identifikátory & profily* část Apple Developer Member Center.
2. V části *certifikáty*vyberte **produkční**.
3. Klikněte na tlačítko **+** pro vytvoření nového certifikátu.
4. V části *produkční* záhlaví, vyberte **vnitřního a Ad Hoc**:

   [![](in-house-distribution-images/createcertmanually01.png "Výběr vnitřního a Ad Hoc")](in-house-distribution-images/createcertmanually01.png#lightbox)

5. Kliknutím na pokračovat a postupujte podle pokynů k vytvoření žádost o podepsání certifikátu prostřednictvím přístup do řetězce klíčů:

   [![](in-house-distribution-images/createcertmanually02.png "Vytvořit žádost o přístup do řetězce klíčů pro podepsání certifikátu")](in-house-distribution-images/createcertmanually02.png#lightbox)

6. Po vytvoření CSR podle pokynů, kliknutím na pokračovat a nahrát do centra CSR:

   [![](in-house-distribution-images/createcertmanually03.png "Odeslat žádost o podepsání certifikátu do centra")](in-house-distribution-images/createcertmanually03.png#lightbox)

7. Kliknutím na tlačítko Generovat k vytvoření vašeho certifikátu.
8. Stáhněte si certifikát dokončené a dvakrát klikněte na soubor k její instalaci.
9. V tuto chvíli váš certifikát se musí nainstalovat na počítači, ale budete muset aktualizovat profil, ujistěte se, že jsou viditelné v Xcode.

Alternativně je možné požádat o certifikát přes dialogové okno Předvolby v Xcode. Chcete-li to provést, postupujte podle kroků níže:

1. Vyberte váš tým a klikněte na tlačítko *zobrazit podrobnosti o*:

    [![](in-house-distribution-images/selectteam.png "Vyberte váš tým")](in-house-distribution-images/selectteam.png#lightbox)

2. Klikněte **vytvořit** vedle **distribuce certifikátu iOS**:

   [![](in-house-distribution-images/selectcert.png "Vytvoření distribuci certifikátů pro iOS")](in-house-distribution-images/selectcert.png#lightbox)

2.   Klikněte **plus (+)** tlačítko a vyberte **iOS App Store**:

   [![](in-house-distribution-images/selectcert.png "Vybrat iOS App Store")](in-house-distribution-images/selectcert.png#lightbox)

<a name="profile" />

## <a name="creating-a-distribution-provisioning-profile"></a>Vytvoření distribučního Zřizovacího profilu

<a name="appid" />

### <a name="creating-an-app-id"></a>Vytvoření ID aplikace

Jako s jakékoli další zřizovací profil vytvoříte, ID aplikace bude muset identifikovat aplikace, kterou budete distribuovat do zařízení uživatele. Pokud jste to ještě nevytvořili, postupujte podle následujících kroků, abyste ho vytvořit:


1. V [Apple Developer Center](https://developer.apple.com/account/overview.action) přejděte *profily certifikátů, identifikátory a* části. Vyberte **App ID** pod **identifikátory**.
2. Klikněte na tlačítko **+** tlačítko a zadejte **název** který bude identifikovat na portálu.
3. Předponu App by již nastaven jako ID vašeho týmu a nedá se změnit. Vyberte buď explicitní nebo ID aplikace zástupný znak a zadejte ID sady v následujícím formátu zpětné DNS: **explicitní**: com. [DomainName].[AppName] **zástupné**: com. [DomainName]. *
4. Vyberte některou [App Services](~/ios/get-started/installation/device-provisioning/index.md#provisioning-for-application-services) , která vaše aplikace vyžaduje.
5. Klikněte na tlačítko **pokračovat** tlačítko a postupovat podle pokynů na obrazovce vytvořte nové ID aplikace.

Jakmile budete mít požadované součásti potřebné pro vytvoření distribučního profilu, podle následujících pokynů k jeho vytvoření:

1. Vraťte se na portál Apple zřizování a vyberte **zřizování** > **distribuce**:

   [![](in-house-distribution-images/distribute01.png "Vyberte zřizování > Distribuce")](in-house-distribution-images/distribute01.png#lightbox)

2. Klikněte na tlačítko **+** tlačítko a vyberte typ distribučního profilu, který chcete vytvořit jako **interní**:

   [![](in-house-distribution-images/distribute02.png "Vytvoření profilu interní distribuci")](in-house-distribution-images/distribute02.png#lightbox)

3. Klikněte na tlačítko **pokračovat** tlačítko a vyberte z rozevíracího seznamu, který chcete vytvořit profil distribuce pro ID aplikace:

   [![](in-house-distribution-images/distribute03.png "Z rozevíracího seznamu vyberte ID aplikace")](in-house-distribution-images/distribute03.png#lightbox)

4. Klikněte na tlačítko **pokračovat** tlačítko a certifikát vybrat distribuční vyžaduje k podepsání aplikace:

   [![](in-house-distribution-images/distribute04.png "Vybrat distribuční certifikát potřebný k podepsání aplikace")](in-house-distribution-images/distribute04.png#lightbox)

6. Klikněte na tlačítko **pokračovat** tlačítko a zadejte **název** nového distribučního profilu:

   [![](in-house-distribution-images/distribute06.png "Zadejte název pro nový profil distribuce")](in-house-distribution-images/distribute06.png#lightbox)

7. Klikněte na tlačítko **generovat** tlačítko pro vytvoření nového profilu a dokončení procesu.

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

 Bude pravděpodobně nutné ukončení sady Visual Studio pro Mac a mít Xcode aktualizujte její seznam dostupných podepisování identity a zřizovací profily (podle pokynů v [žádosti o podepsání identity](~/ios/get-started/installation/device-provisioning/manual-provisioning.md#download) části) před nové Profil distribuce je k dispozici v sadě Visual Studio pro Mac.

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Bude pravděpodobně nutné ukončení sady Visual Studio a mít Xcode (pro hostitele sestavovací Mac), aktualizujte svůj seznam dostupných podepisování identity a zřizovací profily (podle pokynů v [žádosti o podepsání identity](~/ios/get-started/installation/device-provisioning/manual-provisioning.md#download) část) předtím, než se nový profil distribuce je k dispozici v sadě Visual Studio.

-----

<a name="inhouse" />

## <a name="distributing-your-app-in-house"></a>Interně distribuce aplikace

Pomocí programu Apple Developer Enterprise Program licence je osoba zodpovědná pro distribuci aplikace a týkajícími [pokyny](http://adcdownload.apple.com/Documentation/License_Agreements__Apple_Developer_Enterprise_Program/Apple_Developer_Program_Enterprise_Agreement_20150608.pdf) nastavená applem.

Vaše aplikace mohou být distribuovány bezpečně pomocí širokou škálu různých prostředků, jako například:

- Místně přes iTunes
- MDM server
- Interní a zabezpečený webový server
- E-mailu

Distribuce aplikace v některém z těchto způsobů musí nejprve vytvořit soubor IPA, jak je vysvětleno v další části.


### <a name="creating-an-ipa-for-in-house-deployment"></a>Vytváření IPA pro interní nasazení

Po zřízení, aplikace se dá zabalit do soubor známý jako *IPA*. Toto je soubor zip obsahující aplikaci, společně s další metadata a ikony. IPA se používá k přidání aplikace místně do iTunes, takže mohou být synchronizovány přímo do zařízení, která je zahrnutá v profilu zřizování.

Další informace o vytvoření viz soubor IPA [podpora IPA](~/ios/deploy-test/app-distribution/ipa-support.md) průvodce.


## <a name="summary"></a>Souhrn

Tento článek přiřadil stručný přehled o distribuci aplikací Xamarin.iOS interně.

## <a name="related-links"></a>Související odkazy

- [Distribuce v obchodě App Store](~/ios/deploy-test/app-distribution/app-store-distribution/index.md)
- [Jednorázová distribuce](~/ios/deploy-test/app-distribution/ad-hoc-distribution.md)
- [Soubor iTunesMetadata.plist](~/ios/deploy-test/app-distribution/itunesmetadata.md)
- [Podpora IPA](~/ios/deploy-test/app-distribution/ipa-support.md)
