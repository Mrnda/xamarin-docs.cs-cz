---
title: Interní distribuční pro aplikace pro Xamarin.iOS
description: Tento dokument poskytuje stručný přehled distribuce aplikací interně, jako je členem programu pro vývojáře Apple Enterprise.
ms.prod: xamarin
ms.assetid: 9466E51E-303E-466E-85D7-D0525E16BB37
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/19/2017
ms.openlocfilehash: 657370705233e923b482b67fc5afed12631c8187
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 06/05/2018
ms.locfileid: "34785025"
---
# <a name="in-house-distribution-for-xamarinios-apps"></a>Interní distribuční pro aplikace pro Xamarin.iOS

_Tento dokument poskytuje stručný přehled distribuce aplikací interně, jako je členem programu pro vývojáře Apple Enterprise._

Po byla vyvinuta aplikaci Xamarin.iOS, je dalším krokem v životního cyklu softwaru distribuovat aplikace pro uživatele. Vlastní aplikace mohou být distribuovány *interní* (dříve nazývané Enterprise) prostřednictvím **Apple Developer Enterprise Program**, který nabízí následující výhody:

- Aplikace nemusí být odeslány k posouzení společností Apple.
- Neexistují žádná omezení množství zařízení, do nichž můžete nasadit aplikace
    - Je důležité si uvědomit, že je Apple za velmi jasné, že interní aplikace jsou pouze pro interní použití.

Je také důležité si uvědomit, že Enterprise Program:

- Neposkytuje přístup k iTunes připojení pro distribuční nebo testování (včetně TestFlight).
- Náklady na členství je 299 za jeden rok.

Dál musí být podepsaný společností Apple všechny aplikace.

<a name="testing" />

## <a name="testing-your-application"></a>Testování aplikace

Testování vašich aplikací se provádí pomocí Ad Hoc distribuce. Další informace o testování, postupujte podle kroků v [Ad-Hoc distribuční](~/ios/deploy-test/app-distribution/ad-hoc-distribution.md) průvodce. Upozorňujeme, že je možné pouze testovat na maximálně 100 zařízení.

<a name="setup" />

## <a name="getting-set-up-for-distribution"></a>Získávání nastavil pro distribuci

Stejně jako u jiných programů Apple Developer, v části Apple Developer Enterprise Program, můžete vytvořit pouze správci týmu a agenty distribuce certifikátů a profilů zřizování.

Certifikáty Apple Developer Enterprise Program bude poslední tři roky a profily zřizování vyprší po jednom roce.

Je důležité si uvědomit, že certifikáty s vypršenou platností nelze obnovit, a místo toho budete muset nahradit novou jako podrobné vypršela platnost certifikátu [pod](#certificate).

<a name="certificate" />

## <a name="creating-a-distribution-certificate"></a>Vytvoření certifikátu distribuce

1. Vyhledejte *certifikáty, identifikátory a profily* části Apple Developer Member Center.
2. V části *certifikáty*, vyberte **produkční**.
3. Klikněte **+** tlačítko Vytvořit nový certifikát.
4. V části *produkční* záhlaví, vyberte **interní a Ad Hoc**:

   [![](in-house-distribution-images/createcertmanually01.png "Vyberte interní a Ad Hoc")](in-house-distribution-images/createcertmanually01.png#lightbox)

5. Klikněte na tlačítko Pokračovat a postupujte podle pokynů vytvořte žádost o podepsání certifikátu prostřednictvím přístup do řetězce klíčů:

   [![](in-house-distribution-images/createcertmanually02.png "Vytvoření žádost o podepsání certifikátu prostřednictvím přístup do řetězce klíčů")](in-house-distribution-images/createcertmanually02.png#lightbox)

6. Po vytvoření vaší CSR podle pokynů, klikněte na tlačítko Pokračovat a odeslat do centra pro vaše oddělení služeb zákazníkům:

   [![](in-house-distribution-images/createcertmanually03.png "Nahrát oddělení služeb zákazníkům do centra")](in-house-distribution-images/createcertmanually03.png#lightbox)

7. Klikněte na tlačítko vygenerovat certifikát vytvořit.
8. Stažení dokončené certifikátu a poklikejte na soubor k její instalaci.
9. V tomto okamžiku by váš certifikát by měl být nainstalovat na počítač, ale budete muset aktualizovat profilech, ujistěte se, že jsou viditelné v Xcode.

Případně je možné žádost o certifikát pomocí dialogu Předvolby v Xcode. Chcete-li to provést, postupujte následujícím způsobem:

1. Vyberte váš tým a klikněte na *zobrazit podrobnosti*:

    [![](in-house-distribution-images/selectteam.png "Vyberte váš tým")](in-house-distribution-images/selectteam.png#lightbox)

2. Klikněte na tlačítko **vytvořit** vedle položky **iOS certifikátu distribučního**:

   [![](in-house-distribution-images/selectcert.png "Vytvoření certifikátu distribučního iOS")](in-house-distribution-images/selectcert.png#lightbox)

2.   Klikněte na tlačítko **plus (+)** tlačítko a vyberte **iOS App Storu**:

   [![](in-house-distribution-images/selectcert.png "Vyberte iOS App Storu")](in-house-distribution-images/selectcert.png#lightbox)

<a name="profile" />

## <a name="creating-a-distribution-provisioning-profile"></a>Vytvoření distribučního profil pro zřizování

<a name="appid" />

### <a name="creating-an-app-id"></a>Vytvoření ID aplikace

Jako všechny ostatní zřizování profilu vytvoříte, ID aplikace bude nutné k identifikaci aplikace, které budete distribuovat do zařízení uživatele. Pokud jste to ještě nevytvořili, použijte následující postup k jeho vytvoření:


1. V [Apple Developer Center](https://developer.apple.com/account/overview.action) vyhledejte *certifikát, identifikátory a profily* části. Vyberte **ID aplikace** pod **identifikátory**.
2. Klikněte **+** tlačítko a zadejte **název** který bude identifikovat na portálu.
3. Předpona aplikace musí být již nastavená jako ID vašeho týmu a nelze změnit. Vyberte buď explicitní nebo ID aplikace zástupný znak a zadejte ID sady v následujícím formátu zpětné DNS: **explicitní**: com. [DomainName].[AppName] **zástupné**: com. [DomainName]. *
4. Vyberte některé [App Services](~/ios/get-started/installation/device-provisioning/index.md#appservices) vyžadující vaší aplikace.
5. Klikněte **pokračovat** tlačítko a postupujte podle pokynů na obrazovce pro vytvoření nového ID aplikace.

Jakmile máte požadované součásti potřebné pro vytváření profil distribuce, použijte následující postup k jeho vytvoření:

1. Vraťte se na portál Apple zřizování a vyberte **zřizování** > **distribuční**:

   [![](in-house-distribution-images/distribute01.png "Vyberte zřizování > Distribuce")](in-house-distribution-images/distribute01.png#lightbox)

2. Klikněte **+** tlačítko a vyberte typ profil distribuce, kterou chcete vytvořit jako **interní**:

   [![](in-house-distribution-images/distribute02.png "Vytvoření profilu interní distribuční")](in-house-distribution-images/distribute02.png#lightbox)

3. Klikněte **pokračovat** tlačítko a vyberte z rozevíracího seznamu, který chcete vytvořit profil distribuce pro ID aplikace:

   [![](in-house-distribution-images/distribute03.png "Z rozevíracího seznamu vyberte ID aplikace")](in-house-distribution-images/distribute03.png#lightbox)

4. Klikněte **pokračovat** tlačítko a certifikát vybrat distribuční vyžaduje k podepisování aplikace:

   [![](in-house-distribution-images/distribute04.png "Vybrat distribuční certifikát vyžadovaný k podepsání aplikace")](in-house-distribution-images/distribute04.png#lightbox)

6. Klikněte **pokračovat** tlačítko a zadejte **název** pro nový profil distribuce:

   [![](in-house-distribution-images/distribute06.png "Zadejte název nového profilu distribuce")](in-house-distribution-images/distribute06.png#lightbox)

7. Klikněte **generování** tlačítko pro vytvoření nového profilu a dokončení procesu.

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

 Bude pravděpodobně nutné ukončit Visual Studio pro Mac a mít Xcode obnovte její seznam dostupných podepisování identit a profilů zřizování (podle pokynů v [požaduje podepisování identity](~/ios/get-started/installation/device-provisioning/manual-provisioning.md#download) část) před nový Profil distribuce je k dispozici v sadě Visual Studio for Mac.

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Bude pravděpodobně nutné ukončit Visual Studio a mít Xcode (na hostiteli vytvářet Mac), aktualizujte seznam dostupných podepisování identity a profilů zřizování (podle pokynů v [požaduje podepisování identity](~/ios/get-started/installation/device-provisioning/manual-provisioning.md#download) část) předtím, než nový profil distribuce je k dispozici v sadě Visual Studio.

-----

<a name="inhouse" />

## <a name="distributing-your-app-in-house"></a>Interně distribuce aplikace

S Apple Developer Enterprise Program držitel licence je osoba odpovědná pro distribuci aplikace a se [pokyny](http://adcdownload.apple.com/Documentation/License_Agreements__Apple_Developer_Enterprise_Program/Apple_Developer_Program_Enterprise_Agreement_20150608.pdf) nastavit společností Apple.

Aplikace mohou být distribuovány bezpečně pomocí celou řadu různých prostředků, jako třeba:

- Místně přes iTunes
- MDM server
- Interní a zabezpečené webovém serveru
- E-mailu

Distribuovat aplikace v některém z těchto způsobů musíte nejprve vytvořit soubor IPA, jak je popsáno v následující části.


### <a name="creating-an-ipa-for-in-house-deployment"></a>Vytváření IPA pro interní nasazení

Po zřízení, aplikace se dá zabalit do souboru označuje jako *soubor IPA*. Toto je soubor zip, který obsahuje aplikace, spolu s další metadata a ikony. Soubor IPA se používá k přidání aplikace místně do iTunes tak, aby mohou být synchronizovány přímo do zařízení, která je zahrnutá v profilu zřizování.

Další informace o vytváření k IPA naleznete [soubor IPA podporu](~/ios/deploy-test/app-distribution/ipa-support.md) průvodce.


## <a name="summary"></a>Souhrn

V tomto článku jste dali stručný přehled distribuce aplikací Xamarin.iOS interně.

## <a name="related-links"></a>Související odkazy

- [Distribuce v obchodě App Store](~/ios/deploy-test/app-distribution/app-store-distribution/index.md)
- [Jednorázová distribuce](~/ios/deploy-test/app-distribution/ad-hoc-distribution.md)
- [Soubor iTunesMetadata.plist](~/ios/deploy-test/app-distribution/itunesmetadata.md)
- [Podpora IPA](~/ios/deploy-test/app-distribution/ipa-support.md)
