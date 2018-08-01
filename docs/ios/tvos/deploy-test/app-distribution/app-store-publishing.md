---
title: Publikování do App Storu Apple TV
description: Tento dokument popisuje postup publikování aplikace pro Apple TV App Store. Popisuje, jak nakonfigurovat, zřízení, vytvoření a odeslání tvOS aplikací vytvořených pomocí Xamarinu.
ms.prod: xamarin
ms.assetid: 52448C93-DC19-40FA-BF8C-608AE680FF49
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/16/2017
ms.openlocfilehash: ac905caaf0bdefe7f0c5502be0bd63102ca5a813
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 06/05/2018
ms.locfileid: "34789301"
---
# <a name="publishing-to-the-apple-tv-app-store"></a>Publikování do App Storu Apple TV

V pořadí distribuovat aplikace na všechna zařízení Apple TV, Apple vyžaduje, aby publikovat prostřednictvím aplikace *Apple TV App Store*, provedení nákupní centrální umístění pro tvOS aplikace obchodu s aplikacemi. Vývojáři mnoho typů aplikací můžete velkými písmeny v masivním úspěch tento jediný bod rozdělení. Apple TV App Store je to řešení na klíč, nabídky vývojáři aplikací distribuce a platebních systémy.

Proces podání žádosti o App Storu Apple TV zahrnuje:

1. Vytvoření *ID aplikace* a výběrem *oprávnění*.
2. Vytváření *distribuční profil pro zřizování.*
3. Pomocí tohoto profilu k sestavení aplikace.
4. Odeslání vaší aplikace prostřednictvím *iTunes Connect*.


V tomto článku vám nabídneme všechny kroky potřebné k zřizovat, vytvoření a odeslání aplikace pro distribuci Apple TV App Store.

<a name="Before_you_Submit" />

## <a name="before-you-submit-an-application"></a>Před odesláním aplikace

Po odeslání aplikace pro Apple TV App Store publikace projde kontrolním procesu společností Apple zajistit, že splňuje společnosti Apple pokyny pro kvality a obsahu. Pokud vaše aplikace nesplňuje tyto pokyny, odmítnou Apple ho, po kterém budete muset vyřešit – neshoda citovalo společností Apple a pak znovu odešlete.
Proto být mít co šanci díky tomu prostřednictvím Apple zkontrolujte seznámení se s těmito pokyny a při pokusu přizpůsobit aplikaci na ně. Společnosti Apple pokyny jsou k dispozici na [App Store kontrolní pokyny](https://developer.apple.com/appstore/resources/approval/guidelines.html) a [připravit vaše aplikace odeslání nové Apple TV](https://developer.apple.com/tvos/submit/).

Pár věcí sledujte při odesílání aplikace, které jsou:

1. Ujistěte se, že popis aplikace odpovídá funkcí zahrnutých v aplikaci.
2. Test, který není havárií aplikace v rámci normálního využití. To zahrnuje využití na každé zařízení Apple TV, které podporujete.


Apple také udržuje seznam tipy odeslání Apple TV App Store. Můžete si přečíst tyto položky ve [distribuci na webu App Store](https://developer.apple.com/appstore/resources/submission/tips.html).

<a name="Configuring_your_Application_in_iTunes_Connect" />

## <a name="configuring-your-application-in-itunes-connect"></a>Konfigurace vaší aplikace v iTunes Connect

[iTunes Connect](https://itunesconnect.apple.com/WebObjects/iTunesConnect.woa) je sada nástrojů pro webové, mimo jiné, správu tvOS aplikací na webu Apple TV App Store. Vaše aplikace Xamarin.tvOS bude muset být správně instalační program a nakonfigurované v iTunes připojit před lze odeslat společnosti Apple ke kontrole a nakonec, se uvolní pro prodej, nebo jako bezplatnou aplikaci v Apple TV App Store.

Postupujte takto:

1. Ověřte, zda jsou na místě a až datum ve správné smlouvy **smlouvy, daň a bankovnictví** části iTunes připojit k vydání aplikace pro iOS zdarma nebo pro prodej.
2. Vytvořte novou **iTunes připojit záznam** pro aplikaci a zadejte jeho **zobrazovaný název** (jak je vidět v App Storu Apple TV).
3. Vyberte **prodej cena** nebo určit, že aplikace budou vydány zdarma.
4. Zadejte **App Store ikonu** (velkých ikon.) a snímky obrazovek svoji aplikaci v akci, na zařízení Apple TV podporuje. V tématu naše [práce s ikony a bitové kopie](~/ios/tvos/app-fundamentals/icons-images.md) průvodce další podrobnosti.
5. Zajištění čisté, stručného **popis** aplikace včetně její funkce a využívat pro koncového uživatele.
6. Zadejte **kategorie**, **dílčí kategorie**, a **klíčová slova** pomoct najít vaší aplikace v App Storu Apple TV uživateli.
7. Zadejte **kontaktujte** a **podporu** adres URL pro váš web vyžaduje společností Apple.
8. Nastavení aplikace **hodnocení**, který je využíván jiným rodičovské kontroly na webu Apple TV App Store.
9. Nakonfigurovat volitelné obchodu s aplikacemi technologie, jako třeba **herní Centrum** a **nákupy v aplikaci**.

Další podrobnosti najdete v tématu naše [konfigurace vaší tvOS aplikace v iTunes Connect](~/ios/tvos/deploy-test/app-distribution/itunes-connect.md) dokumentaci.

<a name="Preparing_for_App_Store_Distribution" />

## <a name="preparing-for-app-store-distribution"></a>Příprava pro distribuci obchodu s aplikacemi

Publikování aplikace do App Storu Apple TV, musíte nejprve vytvořit pro distribuci, který zahrnuje mnoho kroků. V následujících částech obsahuje všechno potřebné při přípravě Xamarin.tvOS aplikace pro publikaci tak, aby se dají vytvářet a odešlete ji do App Storu Apple TV ke kontrole a verzi.

<a name="Provisioning_for_Application_Services" />

### <a name="provisioning-for-application-services"></a>Zřizování pro aplikační služby

Apple poskytuje výběr speciální aplikační služby, označované taky jako oprávnění, které je možné aktivovat pro vaši aplikaci tvOS, pokud vytvoříte jedinečné ID pro ni. Ať používáte vlastní oprávnění, nebo Ne, musíte stále vytvořit jedinečné ID pro vaši aplikaci Xamarin.tvOS před publikováním na webu Apple TV App Store.

Vytvoření ID aplikace a volitelně vybrat oprávnění zahrnuje následující kroky pomocí společnosti Apple založené na webu iOS Provisioning Portal:

1. Vyberte **zřizování** > **vývoj**.
2. Klikněte **+** tlačítko a zadejte **název** a **ID sady** pro novou aplikaci.
3. Přejděte do dolní části obrazovky a vyberte některou **App Services** , bude nutné Xamarin.tvOS aplikace.
4. Klikněte **pokračovat** tlačítko a následující na obrazovce pokyny pro vytvoření nového ID aplikace.

Kromě výběr a konfigurace požadované aplikace služby při definování vaše ID aplikace, musíte také nakonfigurovat ID aplikace a oprávnění v projektu Xamarin.tvOS úpravou i `Info.plist` a `Entitlements.plist` soubory.

Proveďte postup v sadě Visual Studio pro Mac:

1. V **Průzkumníku řešení**, dvakrát klikněte `Info.plist` soubor otevřete pro úpravy.
2. V **tvOS cíl aplikací** , zadejte název vaší aplikace a zadejte **identifikátor svazku** jste vytvořili při definování ID aplikace.
3. Uložit změny `Info.plist` souboru.
4. V **Průzkumníku řešení**, dvakrát klikněte `Entitlements.plist` soubor otevřete pro úpravy.
5. Vyberte a nakonfigurujte oprávnění požadované pro vás Xamarin.tvOS aplikace tak, aby se shodovaly s instalačním programem, které jste provedli výše, pokud je definována ID aplikace.
6. Uložit změny `Entitlements.plist` souboru.

Podrobné pokyny najdete v tématu naše [zřizování pro služby aplikací](~/ios/get-started/installation/device-provisioning/manual-provisioning.md#appservices) dokumentaci. Když tento dokument byl napsané pro iOS, stejný postup se používají ke zřízení Xamarin.tvOS aplikace.

<a name="Setting_the_Apps_Icons_and_Launch_Screens" />

### <a name="setting-the-apps-icons-launch-image-and-top-shelf-image"></a>Nastavení ikony aplikace, spusťte Image a Image nejvyšší police

Pro tvOS aplikaci, třeba přijmout společností Apple pro zahrnutí v App Storu Apple TV vyžaduje správné ikony, spuštění a horní police obrázků pro všechna zařízení Apple TV, které budou běžet na. Budete potřebovat přidat požadované prostředky bitové kopie, který se zkompiluje do `Assets.car` souborů a součástí aplikace Xamarin.tvOS sady před odesláním do iTunes připojit.

Podrobné pokyny najdete v tématu naše [práce s ikony a bitové kopie](~/ios/tvos/app-fundamentals/icons-images.md) dokumentaci.

<a name="Creating_and_Installing_a_Distribution_Profile" />

### <a name="creating-and-installing-a-distribution-profile"></a>Vytvoření a instalace profil distribuce

používá tvOS *profily zřizování* k řízení nasazení sestavení konkrétní aplikace. Jsou to soubory, které obsahují informace o certifikát použitý k podepsání aplikace, *ID aplikace*, kde se dají nainstalovat aplikaci. Pro vývoj a distribuci ad-hoc profil zřizování také zahrnuje seznam povolených zařízení, do kterých můžete aplikaci nasadit. Pro distribuci Apple TV App Store, pouze informace ID certifikátu a aplikace jsou však součástí, vzhledem k tomu, že je pouze mechanismus pro distribuci veřejného prostřednictvím App Storu Apple TV.

Zřizování zahrnuje následující kroky pomocí společnosti Apple webové iOS Provisioning Portal:

1.  Vyberte **zřizování** > **distribuční**.
2.  Klikněte **+** tlačítko a vyberte typ profil distribuce, kterou chcete vytvořit jako **Apple TV App Store**.
3.  Vyberte **ID aplikace** z rozevíracího seznamu, který chcete vytvořit profil distribuce pro.
4.  Vyberte certifikát vyžadovaný k podepsání aplikace.
5.  Zadejte **název** pro nové **profil distribuce** a generovat profil.
6.  Aktualizujte seznam dostupných profilů v Xcode.
7.  Vyberte distribuční profil v sadě Visual Studio pro zřizování **obchod** _konfigurace sestavení_.

Podrobné pokyny najdete v tématu [vytváření profil distribuce](~/ios/deploy-test/app-distribution/app-store-distribution/index.md#creatingprofile) a [vyberete profil distribuce v projektu Xamarin.iOS](~/ios/deploy-test/app-distribution/app-store-distribution/index.md#selectprofile). Znovu oba tyto dokumenty jsou specifické pro iOS, ale stejný postup se používá pro tvOS aplikace.


<a name="Setting_the_Build_Configuration_for_your_Application" />

### <a name="setting-the-build-configuration-for-your-application"></a>Nastavení konfigurace sestavení pro aplikaci

Ve výchozím nastavení, když vytvoříte novou aplikaci Xamarin.tvOS _sestavení konfigurace_ se vytvářejí automaticky pro obě **ladění** a **verze** nasazení. Před provedením poslední sestavení vaší aplikace, který se odesílá do společnosti Apple, existuje několik změna, která budete muset provést základní **verze** konfigurace.

Postupujte takto:

1. Klikněte pravým tlačítkem na **název projektu** v **Průzkumníku řešení** a výběr **možnosti** k jejich otevření k úpravám.
2. Pokud cílíte na konkrétní verzi tvOS, vyberte ho v části **tvOS sestavení** > **iOS SDK verze**. Ve verzi Preview tvOS podpory, uveďte tuto hodnotu nastavit na **výchozí**.
3. Propojování snižuje celkové velikosti vaší aplikace distribuovatelného tím odstraňování se nepoužité metody, vlastnosti třídy, atd. a ve většině případů by měl být ponecháno na výchozí hodnotu **pouze odkaz framework SDK**. V některých situacích, jako např. kdy některé konkrétní použití 3. stran knihovny, budete muset nastavit hodnotu **není odkaz** zachovat potřebné element odebrání.
4. Pro odeslání Xamarin.tvOS aplikace, budete muset používat optimalizace kompilátoru LLVM. Ujistěte se, že **použít LLVM optimalizace kompilátoru** je zaškrtnuté políčko v části **verze** konfigurace.
5. Apple také vyžaduje, aby tvOS aplikace používají bitcode. V části **verze** konfigurace, přidejte `--bitcode=asmonly` k **mtouch další argumenty** pole.
6. **Soubory ve formátu PNG optimalizovat pro iOS** by měl být zaškrtnuto, jako to vám pomůže další zmenšete velikost dodávky vaší aplikace.
7. Ladění by *není* povolit sestavení budou provedeny zbytečně větší.


<a name="Building_and_Submitting_the_Distributable" />

## <a name="building-and-submitting-the-distributable"></a>Vytváření a odesílání distribuovatelného

S aplikací Xamarin.tvOS správně nakonfigurovány nyní jste připraveni udělat poslední distribuční sestavení, který se odesílá, aby Apple ke kontrole a verzi.

#### <a name="build-your-archive"></a>Sestavení vašem archivu

1. Vyberte **verze | Zařízení** konfigurace v sadě Visual Studio pro Mac:

    ![](app-store-publishing-images/buildxs01new.png "Vyberte konfigurace verze")
2. Z **sestavení** nabídce vyberte možnost **archivu pro publikování**:

    [![](app-store-publishing-images/buildxs02new.png "Vyberte archivu pro publikování")](app-store-publishing-images/buildxs02new.png#lightbox)
3. Po vytvoření archivu **archivy** zobrazení se zobrazí:

    [![](app-store-publishing-images/buildxs03new.png "Zobrazení archivy")](app-store-publishing-images/buildxs03new.png#lightbox)

### <a name="sign-and-distribute-your-app"></a>Podepisování a distribuce aplikace

Pokaždé, když vytváříte aplikace pro archiv, se automaticky otevře *archivy zobrazení*, zobrazení všech archivovány projekty; seskupené podle řešení. Ve výchozím nastavení toto zobrazení uvádí jenom aktuální, otevřete řešení. Pokud chcete zobrazit všechna řešení, které mají archivy, klikněte na **zobrazit všechny archivy** možnost.

Aby se zachovala archivy nasazované pro zákazníky (obchodu s aplikacemi nebo Enterprise nasazení), se doporučuje, aby všechny ladicí informace, které jsou generovány můžete symbolized později.

Podepište aplikaci, a příprava pro distribuci:

1. Vyberte **přihlásit a distribuovat...** , ilustrované níže:

    [![](app-store-publishing-images/buildxs04new.png ", Vyberte theSign a distribuci...")](app-store-publishing-images/buildxs04new.png#lightbox)
2. Otevře se Průvodce přidáním publikování. Vyberte **obchod** distribuční kanál vytvořit balíček a otevřete zavaděč aplikací:

    [![](app-store-publishing-images/distribute01.png "Vyberte distribuční kanál obchodu s aplikacemi")](app-store-publishing-images/distribute01.png#lightbox)
3. Na obrazovce profil zřizování vyberte podepisování identity a odpovídající profil pro zřizování nebo znovu podepsat pomocí jiné identity:

    [![](app-store-publishing-images/distribute02.png "Vyberte podpisový identity a odpovídající profil pro zřizování")](app-store-publishing-images/distribute02.png#lightbox)
4. Zkontrolujte podrobnosti vašeho balíčku a klikněte na tlačítko **publikovat** uložit vaše `.ipa` balíčku:

    [![](app-store-publishing-images/distribute03.png "Zkontrolujte podrobnosti o balíčku")](app-store-publishing-images/distribute03.png#lightbox)
5. Jednou vaše `.ipa` byl uložen, je aplikace připravená k odeslání do iTunes připojit prostřednictvím zavaděč aplikací:

    [![](app-store-publishing-images/distribute04.png "Nahrán do iTunes připojit prostřednictvím zavaděč aplikací")](app-store-publishing-images/distribute04.png#lightbox)

S distribuční vytvořit sestavení a archivovat, nyní jste připraveni k odeslání aplikace iTunes připojit.

<a name="Submitting_Your_App_to_Apple" />

## <a name="submitting-your-app-to-apple"></a>Odeslání vaší aplikace pro Apple

S distribuční sestavení dokončit jste připraveni k odeslání vaší aplikace pro iOS Apple ke kontrole a uvolněte na webu App Store.


Pracovní postup archivu v sadě Visual Studio pro Mac se zavaděč aplikací automaticky otevře, které jste uložili `.ipa`:

2. Vyberte *poskytovat aplikace* a klikněte na *zvolte* tlačítko:

    [![](app-store-publishing-images/publishvs01.png "Vyberte poskytování vaší aplikace")](app-store-publishing-images/publishvs01.png#lightbox)

3. Vyberte zip nebo soubor IPA vytvořili výše a klikněte na **OK** tlačítko.
4. Zavaděč aplikací budou ověření souboru:

    [![](app-store-publishing-images/publishvs02.png "Na obrazovce ověření zavaděč aplikací")](app-store-publishing-images/publishvs02.png#lightbox)
5. Klikněte *Další* tlačítko a aplikace bude ověřovat s obchodu s aplikacemi:

    [![](app-store-publishing-images/publishvs03.png "Aplikace ověřován proti obchodu s aplikacemi")](app-store-publishing-images/publishvs03.png#lightbox)
6. Klikněte **odeslat** tlačítko Odeslat ke kontrole aplikace společnosti Apple.
7. Zavaděč aplikací bude informovat, když byla úspěšně nahrána soubor.

<a name="iTunes_Connect_Status" />

### <a name="itunes-connect-status"></a>iTunes stav připojení

Pokud znovu se přihlásili k iTunes připojit a vyberte svou aplikaci ze seznamu dostupných aplikací, stav v iTunes Connect by měl zobrazit nyní, že je **čekání zkontrolujte** (dočasně lze číst **nahrát přijata** Když je zpracován):

[![](app-store-publishing-images/image21.png "Tento stav v iTunes připojit zobrazující čekání ke kontrole")](app-store-publishing-images/image21.png#lightbox)

<a name="Troubleshooting" />

## <a name="troubleshooting"></a>Poradce při potížích

Pokud máte problémy s odesláním aplikace Xamarin.tvOS do App Storu Apple TV, najdete v tématu naše [Poradce při potížích s](~/ios/tvos/troubleshooting.md) průvodce. Obsahuje několik známých problémů, které se můžete setkat a jak je vyřešit v Xamarin.tvOS.

<a name="Summary" />

## <a name="summary"></a>Souhrn

Tento článek zobrazí podrobný návod, jak konfiguraci, sestavování a odesílání aplikace pro Apple TV App Store publikace. Nejprve popsané kroky potřebné k vytvoření a nainstalovat distribuční profil pro zřizování. V dalším kroku ji projít jak používat Visual Studio pro Mac k vytvoření distribuční sestavení. Nakonec se ukázal, jak používat nástroj archivu Xcode a iTunes připojit k odesílání aplikace pro Apple TV App Store.


## <a name="related-links"></a>Související odkazy

- [Práce s ikonami a obrázky](~/ios/tvos/app-fundamentals/icons-images.md)
- [Připravte si aplikaci odeslání nové Apple TV](https://developer.apple.com/tvos/submit/)
- [Tipy pro odeslání obchodu s aplikacemi](https://developer.apple.com/appstore/resources/submission/tips.html)
- [Běžné aplikace zamítnutí](https://developer.apple.com/app-store/review/rejections/)
- [Pokyny pro recenze v App Storu](https://developer.apple.com/appstore/resources/approval/guidelines.html)
