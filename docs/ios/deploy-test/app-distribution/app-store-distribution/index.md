---
title: Distribuce obchodu s aplikacemi
description: Tento dokument popisuje, jak se bude distribuovat aplikace pro Xamarin.iOS na webu App Store. Popisuje, jak vytvořit certifikát pro distribuční, postup vytvoření distribučního profil pro zřizování a jak nakonfigurovat iTunes připojit a odeslání aplikace.
ms.prod: xamarin
ms.assetid: B07E2C1F-A6DF-43CB-BFB0-0252A5558467
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 08/23/2017
ms.openlocfilehash: 42d287285fc9b8842dfb0c86b627d1d5c84189e1
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 06/05/2018
ms.locfileid: "34784813"
---
# <a name="app-store-distribution"></a>Distribuce obchodu s aplikacemi

Po byla vyvinuta aplikace Xamarin.iOS, je dalším krokem v životního cyklu softwaru distribuovat aplikace pro uživatele, kteří používají iTunes App Storu. Toto je nejběžnější způsob distribuce aplikací. Tím, že publikujete aplikaci v Apple App Store, se může být dostupné k příjemce po celém světě.

> [!IMPORTANT]
> Je **důležité** si uvědomit, že chcete použít iTunes připojit a proto publikování aplikace k obchodu s aplikacemi, které **musí** být buď součástí jednotlivých nebo organizace Program vývojáře Apple. Nebudete moci postupujte podle kroků na této stránce, pokud jste členem vývojáře Apple **Enterprise** Program.

Distribuce aplikace – stejně jako v vývoji aplikace – vyžaduje, aby aplikace byly zřízené odpovídající pomocí *profil pro zřizování*. Profily zřizování jsou soubory, které obsahují informace, jakož i identity aplikace a mechanismus určený distribuce pro podpis kódu. Také obsahují informace o jaká zařízení mohou být aplikace nasazeny na distribuce App Store.

<a name="provisioning" />

## <a name="provisioning-an-app-for-app-store-distribution"></a>Zřizování aplikace pro distribuci obchodu s aplikacemi

Bez ohledu na to, jak budete chtít verzi aplikace pro Xamarin.iOS, budete potřebovat k sestavení *profil zřizování distribuce* konkrétní k němu. Tento profil umožňuje aplikaci být digitálně podepsané pro verzi, takže se dá nainstalovat na zařízení s iOS. Podobně jako u vývojovém profil pro zřizování, profil distribuce bude obsahovat následující:

- ID aplikace
- Distribuce certifikátů

Můžete vybrat stejné **ID aplikace** a **zařízení** které jste použili pro váš vývojový profil pro zřizování, ale pokud jste již nemáte, budete muset vytvořit certifikát distribuční k identifikaci vaší organizace při odesílání aplikace k obchodu s aplikacemi. Kroky k vytvoření certifikátu distribučního jsou popsané v následující části.

> [!NOTE]
> Distribuce certifikátů a profilů zřizování, můžete vytvořit pouze agenty týmu a správci.

<a name="creatingcertificate" />

## <a name="creating-a-distribution-certificate"></a>Vytvoření certifikátu distribuce

1. Vyhledejte *certifikáty, identifikátory a profily* části Apple Developer Member Center.
2. V části *certifikáty*, vyberte **produkční**.
3. Klikněte **+** tlačítko Vytvořit nový certifikát.
4. V části *produkční* záhlaví, vyberte **App Store a Ad Hoc**:

    [![](images/createcertmanually01.png "Vyberte obchodu s aplikacemi a Ad Hoc")](images/createcertmanually01.png#lightbox)
5. Klikněte na tlačítko **pokračovat**a postupujte podle pokynů vytvořte žádost o podepsání certifikátu prostřednictvím přístup do řetězce klíčů:

    [![](images/createcertmanually02.png "Vytvoření žádost o podepsání certifikátu prostřednictvím přístup do řetězce klíčů")](images/createcertmanually02.png#lightbox)
6. Po vytvoření zástupce podle pokynů, klikněte na tlačítko **pokračovat**a odešlete zástupce do centra pro:

    [![](images/createcertmanually03.png "Nahrát oddělení služeb zákazníkům do centra")](images/createcertmanually03.png#lightbox)

7. Klikněte na tlačítko **generování** k vytvoření certifikátu.
8. Nakonec **Stáhnout** dokončené certifikátu a poklikejte na soubor k její instalaci.
9. V tomto okamžiku certifikát by měly být nainstalovány na počítači, ale budete muset [aktualizovat o profilech](~/ios/get-started/installation/device-provisioning/manual-provisioning.md#download), aby bylo zajištěno, že jsou viditelné v Xcode.

Případně je možné žádost o certifikát pomocí dialogu Předvolby v Xcode. Chcete-li to provést, postupujte následujícím způsobem:

1.   Vyberte váš tým a klikněte na **spravovat certifikáty...** : [ ![ ] (images/selectteam.png "Vyberte tým a zobrazit podrobnosti")](images/selectteam.png#lightbox)

2.   Klikněte na tlačítko **vytvořit** vedle položky **iOS certifikátu distribučního**: [ ![ ] (images/selectcert.png "vytvořit certifikát pro distribuční pro iOS")](images/selectcert.png#lightbox)

3.   V závislosti na vaší team oprávnění, podpisové identity se budou generovat, jak je uvedeno níže, nebo možná budete muset počkat do týmu agenta nebo jej schválí správce: [ ![ ] (images/generated.png "podpisové identity se budou generovat a Zobrazí dialogové okno")](images/generated.png#lightbox)


<a name="creatingprofile" />

## <a name="creating-a-distribution-profile"></a>Vytvoření profilu distribuce

<a name="creatingappid" />

### <a name="creating-an-app-id"></a>Vytvoření ID aplikace

Jako všechny ostatní profil zřizování vytvoříte, ID aplikace požadované k identifikaci aplikace, který je distribuován do zařízení uživatele. Pokud jste to ještě nevytvořili, použijte následující postup k jeho vytvoření:


1. V [Apple Developer Center](https://developer.apple.com/account/overview.action) vyhledejte *certifikát, identifikátory a profily* části. Vyberte **ID aplikace** pod **identifikátory**.
2. Klikněte **+** tlačítko a zadejte **název** který bude identifikovat na portálu.
3. Předpona aplikace musí být již nastavená jako ID vašeho týmu a nelze změnit. Vyberte buď explicitní nebo ID aplikace zástupný znak a zadejte ID sady v následujícím formátu zpětné DNS:
    - **Explicitní**: com. [DomainName]. [AppName]
    - **Wildcard**:com.[DomainName].*
4. Vyberte některé [App Services](~/ios/get-started/installation/device-provisioning/manual-provisioning.md#appservices) vyžadující aplikace.
5. Klikněte **pokračovat** tlačítko a postupujte podle pokynů na obrazovce pro vytvoření nového ID aplikace.


### <a name="creating-a-provisioning-profile"></a>Vytvoření profilu pro zřizování

Jakmile máte požadované součásti potřebné pro vytváření profil distribuce, použijte následující postup k jeho vytvoření:

1. Vraťte se na portál Apple zřizování a vyberte **zřizování** > **distribuční**:

    [![](images/distribute01.png "Zřizování RSelect > Distribuce")](images/distribute01.png#lightbox)

2. Klikněte **+** tlačítko a vyberte typ profil distribuce, kterou chcete vytvořit jako **obchod**:

    [![](images/distribute02.png "Vytvoření profilu distribuční obchodu s aplikacemi")](images/distribute02.png#lightbox)

3. Klikněte **pokračovat** tlačítko a vyberte z rozevíracího seznamu, který chcete vytvořit profil distribuce pro ID aplikace:

    [![](images/distribute03.png "Z rozevíracího seznamu vyberte ID aplikace")](images/distribute03.png#lightbox)

4. Klikněte **pokračovat** tlačítko a vyberte certifikát nutný k podepsání aplikace:

    [![](images/distribute04.png "Vyberte certifikát, který vyžaduje k podepisování aplikace")](images/distribute04.png#lightbox)

5. Klikněte **pokračovat** tlačítko a vyberte zařízení iOS, které aplikace pro Xamarin.iOS bude možné spustit:

    [![](images/distribute05.png "Vyberte iOS zařízení aplikaci bude možné spouštět na")](images/distribute05.png#lightbox)

6. Klikněte **pokračovat** tlačítko a zadejte **název** pro nový profil distribuce:

    [![](images/distribute06.png "Zadejte název nového profilu distribuce")](images/distribute06.png#lightbox)

7. Klikněte **generování** tlačítko pro vytvoření nového profilu a dokončení procesu.


# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

 Bude pravděpodobně nutné ukončit Visual Studio pro Mac a mít Xcode aktualizujte seznam dostupných podepisování identity a profilů zřizování (podle pokynů v [požaduje podepisování identity](~/ios/get-started/installation/device-provisioning/manual-provisioning.md#download) část) před nový Profil distribuce je k dispozici v sadě Visual Studio for Mac.

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

 Bude pravděpodobně nutné ukončit Visual Studio a mít Xcode (na hostiteli vytvářet Mac), obnovte její seznam dostupných podepisování identit a profilů zřizování (podle pokynů v [požaduje podepisování identity](~/ios/get-started/installation/device-provisioning/manual-provisioning.md#download) část) předtím, než nový profil distribuce je k dispozici v sadě Visual Studio.

-----

<a name="selectprofile" />

## <a name="selecting-a-distribution-profile-in-a-xamarinios-project"></a>Když vyberete profil distribuce v projektu Xamarin.iOS

Pokud jste připravení poslední sestavení aplikace Xamarin.iOS pro prodej v iTunes App Storu, vyberte profil distribuce, která byla vytvořena výše.

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

 V sadě Visual Studio pro Mac postupujte takto:

1. Dvakrát klikněte na název projektu v **Průzkumníku řešení** otevřete pro úpravy.
2. Vyberte **iOS podepisování sady** a **verze | iPhone** z **konfigurace** rozevíracího seznamu:

    ![](images/releasexs01.png "Vyberte verzi | iPhone z rozevíracího seznamu konfigurace")
3. Ve většině případů **identitu podepisování** a **profil zřizování** může být ponecháno jejich výchozí hodnoty **automatické** a vyberte správnou Visual Studio pro Mac profil na základě identifikátoru sady v Info.plist:

    ![](images/releasexs02.png "Nastavené na výchozí hodnoty automaticky identitu podepisování a profil zřizování")
4. V případě potřeby vyberte z rozevírací seznamy identitu podepisování a profil distribuce (jeden, vytvořili výše):

    ![](images/releasexs03.png "Vyberte identitu podepisování a distribuce profilů")
5. Klikněte **OK** tlačítko a uložte změny.

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

 V sadě Visual Studio postupujte takto:

1. Klikněte pravým tlačítkem myši na název projektu v **Průzkumníku řešení** a vyberte **vlastnosti** otevřete pro úpravy.
2. Vyberte **iOS podepisování sady** a **verze | iPhone** z **konfigurace** rozevíracího seznamu:

    ![](images/releasevs01.png "Vyberte verzi | iPhone z rozevíracího seznamu konfigurace")
3. Ve většině případů **identitu podepisování** a **profil zřizování** může být ponecháno jejich výchozí hodnoty **automatické** a Visual Studio vyberte správného profilu na základě identifikátoru sady v Info.plist

    ![](images/releasevs02.png "Nastavené na výchozí hodnoty automaticky identitu podepisování a profil zřizování")
4. V případě potřeby vyberte z rozevírací seznamy identitu podepisování a profil distribuce (jeden, vytvořili výše):

    ![](images/releasevs03.png "Vyberte identitu podepisování a profil distribuce")
5. Uložte změny do vlastností projektu.

-----

<a name="itunesconnect" />

## <a name="configuring-your-application-in-itunes-connect"></a>Konfigurace vaší aplikace v iTunes Connect

Jakmile se aplikace úspěšně po zřízení, dalším krokem je konfigurace aplikací v [iTunes Connect](https://itunesconnect.apple.com/WebObjects/iTunesConnect.woa), což je sada web na základě nástroje, mimo jiné, Správa aplikací pro iOS v App Storu.

Aplikace Xamarin.iOS bude muset být správně instalační program a nakonfigurované v iTunes připojit před lze odeslat společnosti Apple ke kontrole a nakonec, se uvolní pro prodej, nebo jako bezplatnou aplikaci v App Storu.

Další podrobnosti najdete v tématu naše [konfigurace aplikace v iTunes Connect](~/ios/deploy-test/app-distribution/app-store-distribution/itunesconnect.md) dokumentaci.

<a name="submitting" />

## <a name="submitting-an-app-to-itunes-connect"></a>Odesílá se aplikace pro službu iTunes Connect

Jakmile je aplikace podepsaná pomocí profil zřizování distribuce a vytvoření aplikace v iTunes připojit, binární aplikace je odeslat společnosti Apple ke kontrole. Po úspěšné zkontrolujte společností Apple je k dispozici v úložišti aplikací.

Další informace o publikování aplikací k obchodu s aplikacemi, najdete v části [publikování do obchodu s aplikacemi](~/ios/deploy-test/app-distribution/app-store-distribution/publishing-to-the-app-store.md).

<a name="windows" />

## <a name="automatically-copy-app-bundles-back-to-windows"></a>Automaticky zkopírujte .app sady zpět do systému Windows

[!include[](~/ios/includes/copy-app-bundle-to-windows.md)]

## <a name="summary"></a>Souhrn

Tento článek zahrnutých klíčové komponenty při přípravě aplikace pro Xamarin.iOS pro distribuci v úložišti aplikací.

## <a name="related-links"></a>Související odkazy

- [Konfigurace aplikace v iTunes Connect](~/ios/deploy-test/app-distribution/app-store-distribution/itunesconnect.md)
- [Publikování do obchodu App Store](~/ios/deploy-test/app-distribution/app-store-distribution/publishing-to-the-app-store.md)
- [Interní distribuční](~/ios/deploy-test/app-distribution/in-house-distribution.md)
- [Jednorázová distribuce](~/ios/deploy-test/app-distribution/ad-hoc-distribution.md)
- [Soubor iTunesMetadata.plist](~/ios/deploy-test/app-distribution/itunesmetadata.md)
- [Podpora IPA](~/ios/deploy-test/app-distribution/ipa-support.md)
