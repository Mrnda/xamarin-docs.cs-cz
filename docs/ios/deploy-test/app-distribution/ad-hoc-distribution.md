---
title: Distribuce Ad-Hoc
description: "Tento dokument poskytuje přehled o Ad Hoc distribuční technik, které se primárně používají pro testování aplikací Xamarin.iOS s celou skupinou uživatelů."
ms.topic: article
ms.prod: xamarin
ms.assetid: 3B621CAD-103C-478A-97C3-829015F48D1A
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/19/2017
ms.openlocfilehash: 423240949daf45d8d179a3ca9f89677f490cc24d
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 02/27/2018
---
# <a name="ad-hoc-distribution"></a>Distribuce Ad-Hoc

_Tento dokument poskytuje přehled o Ad Hoc distribuční technik, které se primárně používají pro testování aplikací Xamarin.iOS s celou skupinou uživatelů._

Po byla vyvinuta aplikaci Xamarin.iOS, je dalším krokem v životního cyklu softwaru distribuovat aplikace pro uživatele pro testování.

iTunes připojení je jednou z možností pro správu aplikace testování a je popsána více v [TestFlight](~/ios/deploy-test/testflight.md) průvodce. Členové Apple Developer Enterprise Program však nemají přístup k iTunes připojit, takže *Ad Hoc* distribuce je nejlepší metody testování těchto aplikací.

Aplikace Xamarin.iOS může být testována uživatele prostřednictvím *ad hoc* distribuci, která je k dispozici v programu pro vývojáře Apple a Apple Developer Enterprise Program a umožňuje otestovat až 100 zařízení iOS.

Ad hoc distribuční nabízí výhodu v podobě není vyžadoval schválení obchodu s aplikacemi a může se nainstalovat bezdrátovou z webového serveru nebo přes iTunes. Je, ale nesmí být **100** zařízení za jeden rok členství pro vývoj a distribuce a tyto ručně je nutno přidat v centru člen podle jejich UDID. Další informace o přidání zařízení, najdete v článku [zřizování zařízení](~/ios/get-started/installation/device-provisioning/manual-provisioning.md#adddevice) průvodce.

Ad hoc distribuce vyžaduje, aby aplikace zřídit pomocí Ad Hoc *profil pro zřizování* obsahující informace, jakož i identity aplikace a zařízení, která může instalovat aplikaci pro podpis kódu.

Tento průvodce vám poskytne informace o zřizování pro Ad Hoc distribuci a informace o tom, jak distribuovat aplikace Xamarin.iOS.

<a name="setup" />

## <a name="setting-up-for-distribution"></a>Nastavení pro distribuci

I v případě, že budete chtít verzi aplikace pro Xamarin.iOS pro interní nasazení pro účely testování, budete potřebovat pro vytvoření Ad Hoc distribuční zřizování profilu konkrétní k němu. Tento profil umožňuje aplikaci být digitálně podepsané pro verzi, takže se dá nainstalovat na zařízení s iOS.

V další části se popisují, jak získat nastavit distribuční certifikát a profil pro zřizování distribuce.

> [!NOTE]
>  Poznámka: Pouze agenty týmu a správci můžete vytvořit distribuce certifikátů a profilů zřizování.

<a name="createcertificate" />

## <a name="create-a-distribution-certificate"></a>Vytvoření certifikátu distribuce


1. Vyhledejte *certifikáty, identifikátory a profily* části Apple Developer Member Center.
2. V části *certifikáty*, vyberte **produkční**.
3. Klikněte  **+**  tlačítko Vytvořit nový certifikát.
4. V části *produkční* záhlaví, vyberte **interní a Ad Hoc**, nebo **App Store a Ad Hoc**, v závislosti na vaší členství v programu:

  [ ![](ad-hoc-distribution-images/cert-first-small.png "Vyberte interní a Ad Hoc nebo obchodu s aplikacemi a Ad Hoc")](ad-hoc-distribution-images/cert-first-large.png)

5. Klikněte na tlačítko Pokračovat a postupujte podle pokynů vytvořte žádost o podepsání certifikátu prostřednictvím přístup do řetězce klíčů:

  [ ![](ad-hoc-distribution-images/createcertmanually02.png "Vytvoření žádost o podepsání certifikátu prostřednictvím přístup do řetězce klíčů")](ad-hoc-distribution-images/createcertmanually02.png)

6. Po vytvoření zástupce podle pokynů, klikněte na tlačítko Pokračovat a nahrát zástupce do centra pro:

  [ ![](ad-hoc-distribution-images/createcertmanually03.png "Nahrát oddělení služeb zákazníkům do centra")](ad-hoc-distribution-images/createcertmanually03.png)

7. Klikněte na tlačítko Generovat vytvořit certifikát.
8. Nakonec stažení dokončené certifikátu a poklikejte na soubor k její instalaci.
9. V tomto okamžiku certifikát by měly být nainstalovány na počítači, ale budete muset [aktualizovat o profilech](~/ios/get-started/installation/device-provisioning/manual-provisioning.md#download) zajistit, že jsou viditelné v Xcode.

Případně je možné žádost o certifikát pomocí dialogu Předvolby v Xcode. Chcete-li to provést, postupujte následujícím způsobem:

1.   Vyberte váš tým a klikněte na **spravovat certifikáty...** : [ ![ ] (ad-hoc-distribution-images/selectteam.png "Výběr týmu")](ad-hoc-distribution-images/selectteam.png)

2.   Klikněte na tlačítko **plus (+)** tlačítko a vyberte **iOS App Storu**: [ ![ ] (ad-hoc-distribution-images/selectcert.png "výběr iOS App Storu")](ad-hoc-distribution-images/selectcert.png)

<a name="createprofile" />

## <a name="create-a-distribution-provisioning-profile"></a>Vytvoření distribuce profil pro zřizování

<a name="createappid" />

### <a name="create-an-app-id"></a>Vytvoření ID aplikace
Jako všechny ostatní zřizování profilu vytvoříte, ID aplikace bude nutné k identifikaci aplikace, který bude distribuován na zařízení uživatele. Pokud jste to ještě nevytvořili, použijte následující postup k jeho vytvoření:


1. V [Apple Developer Center](https://developer.apple.com/account/overview.action) vyhledejte *certifikát, identifikátory a profily* části. Vyberte **ID aplikace** pod **identifikátory**.
2. Klikněte  **+**  tlačítko a zadejte **název** který bude identifikovat na portálu.
3. Předpona aplikace musí být již nastavená jako ID vašeho týmu a nelze změnit. Vyberte buď explicitní nebo ID aplikace zástupný znak a zadejte ID sady v následujícím formátu zpětné DNS:
    - **Explicitní**: `com.[DomainName].[AppName]`
    - **Zástupný znak**: `com.[DomainName].*`
4. Vyberte některé [App Services](~/ios/get-started/installation/device-provisioning/manual-provisioning.md#appservices) vyžadující vaší aplikace.
5. Klikněte **pokračovat** tlačítko a postupujte podle pokynů na obrazovce a vytvořit nové ID aplikace.

Jakmile máte požadované součásti potřebné pro vytváření profil distribuce, použijte následující postup k jeho vytvoření:

1. Vraťte se na portál Apple zřizování a vyberte **zřizování > distribuce**: [ ![ ] (ad-hoc-distribution-images/distribute01.png "vyberte zřizování > Distribuce")](ad-hoc-distribution-images/distribute01.png)

2. Klikněte  **+**  tlačítko a vyberte typ profil distribuce, kterou chcete vytvořit jako **Ad-Hoc**:

    [ ![](ad-hoc-distribution-images/distribute02.png "Vytvořit typ distribuční Ad-Hoc")](ad-hoc-distribution-images/distribute02.png)

3. Klikněte **pokračovat** tlačítko a vyberte z rozevíracího seznamu, který chcete vytvořit profil distribuce pro ID aplikace:

    [ ![](ad-hoc-distribution-images/distribute03.png "Z rozevíracího seznamu vyberte ID aplikace")](ad-hoc-distribution-images/distribute03.png)

4. Klikněte **pokračovat** tlačítko a certifikát vybrat distribuční vyžaduje k podepisování aplikace:

    [ ![](ad-hoc-distribution-images/distribute04.png "Vybrat distribuční certifikát vyžadovaný k podepsání aplikace")](ad-hoc-distribution-images/distribute04.png)

6. Klikněte **pokračovat** tlačítko a zadejte **název** pro nový profil distribuce:

    [ ![](ad-hoc-distribution-images/distribute06.png "Zadejte název nového profilu distribuce")](ad-hoc-distribution-images/distribute06.png)

7. Klikněte **generování** tlačítko pro vytvoření nového profilu a dokončení procesu.

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

Bude pravděpodobně nutné ukončit Visual Studio pro Mac a mít Xcode aktualizujte seznam dostupných podepisování identity a profilů zřizování (podle pokynů v [stahování profilů a certifikátů v Xcode](~/ios/get-started/installation/device-provisioning/manual-provisioning.md#download) část) předtím, než nový profil distribuce je k dispozici v sadě Visual Studio for Mac.

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Bude pravděpodobně nutné ukončit Visual Studio a mít Xcode (na hostiteli vytvářet Mac), aktualizujte seznam dostupných podepisování identity a profilů zřizování (podle pokynů v [stahování profilů a certifikátů v Xcode](~/ios/get-started/installation/device-provisioning/manual-provisioning.md#download)část) předtím, než nový profil distribuce je k dispozici v sadě Visual Studio.

-----

<a name="selectprofile" />

## <a name="selecting-a-distribution-profile-in-a-xamarinios-project"></a>Když vyberete profil distribuce v projektu Xamarin.iOS

Pokud jste připravení poslední sestavení aplikace pro Xamarin.iOS, vyberte profil distribuce, která byla vytvořena výše.

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

 V sadě Visual Studio pro Mac postupujte takto:

1. Dvakrát klikněte na název projektu v **Průzkumníku řešení** otevřete pro úpravy.
2. Vyberte **iOS podepisování sady** a typ sestavení z **konfigurace** rozevíracího seznamu:

    ![](ad-hoc-distribution-images/releasexs01.png "Vyberte typ sestavení z rozevíracího seznamu konfigurace")
3. Ve většině případů **identitu podepisování** a **profil zřizování** může být ponecháno výchozí hodnoty **automatické** a vyberte správnou Visual Studio pro Mac profil na základě identifikátoru sady v Info.plist:

    ![](ad-hoc-distribution-images/releasexs02.png "Nastavené na výchozí hodnoty automaticky identitu podepisování a profil zřizování")
4. V případě potřeby vyberte z rozevírací seznamy identitu podepisování a profil distribuce (jeden, vytvořili výše):

    ![](ad-hoc-distribution-images/releasexs03.png "Vyberte identitu podepisování a profil distribuce")
5. Klikněte **OK** tlačítko a uložte změny.

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)
 V sadě Visual Studio postupujte takto:

1. Klikněte pravým tlačítkem myši na název projektu v **Průzkumníku řešení** a vyberte **vlastnosti** otevřete pro úpravy.
2. Vyberte **iOS podepisování sady** a typ sestavení z **konfigurace** rozevíracího seznamu:

    ![](ad-hoc-distribution-images/releasevs01.png "Vyberte typ sestavení z rozevíracího seznamu konfigurace")
3. Ve většině případů **identitu podepisování** a **profil zřizování** může být ponecháno výchozí hodnoty **automatické** a Visual Studio vyberte správného profilu na základě sady identifikátoru v Info.plist:

    ![](ad-hoc-distribution-images/releasevs02.png "Nastavené na výchozí hodnoty automaticky identitu podepisování a profil zřizování")
4. V případě potřeby vyberte z rozevírací seznamy identitu podepisování a profil distribuce (jeden, vytvořili výše):

    ![](ad-hoc-distribution-images/releasevs03.png "Vyberte identitu podepisování a profil distribuce")
5. Uložte změny do vlastností projektu.

-----

<a name="adhoc" />

## <a name="ad-hoc-distribution"></a>Ad Hoc distribuce

Při [TestFlight](~/ios/deploy-test/testflight.md) je Oblíbené způsob beta testování a distribuci, je součástí iTunes připojit a proto není k dispozici pro členy **Apple Developer Enterprise Program**.

Ad Hoc distribuční umožňuje vývojářům aplikací testovací beta na široké škále zařízení při připojení iTunes není možné. Ad Hoc funguje podobným způsobem do interní distribuce a vyžaduje IPA bude vytvořen, které lze následně distribuovat buď bezdrátovou, nebo ručně přes iTunes.

<a name="IPA_Creation" />

### <a name="ipa-support-for-ad-hoc-deployment"></a>Soubor IPA je podpora pro Ad Hoc nasazení

Po zřízení, aplikace se dá zabalit do souboru označuje jako *soubor IPA*. Toto je soubor zip, který obsahuje aplikace, spolu s další metadata a ikony. Soubor IPA se používá k přidání aplikace místně do iTunes tak, aby mohou být synchronizovány přímo do zařízení, která je zahrnutá v profilu zřizování.

Další informace o vytvoření IPA najdete v tématu [soubor IPA podporu](~/ios/deploy-test/app-distribution/ipa-support.md) průvodce.

## <a name="summary"></a>Souhrn

Tento článek vysvětlené na Ad Hoc distribuční mechanismy, které jsou vyžadovány pro testování aplikací pro Xamarin.iOS.


## <a name="related-links"></a>Související odkazy

- [Distribuce v obchodě App Store](~/ios/deploy-test/app-distribution/app-store-distribution/index.md)
- [Interní distribuční](~/ios/deploy-test/app-distribution/in-house-distribution.md)
- [Soubor iTunesMetadata.plist](~/ios/deploy-test/app-distribution/itunesmetadata.md)
- [Podpora IPA](~/ios/deploy-test/app-distribution/ipa-support.md)
