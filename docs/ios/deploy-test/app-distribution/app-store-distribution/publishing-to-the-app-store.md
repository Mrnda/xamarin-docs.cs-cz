---
title: "Publikování do obchodu s aplikacemi"
description: "Tento článek ukazuje, jak nakonfigurovat, vytvářet a publikovat aplikace pro Xamarin.iOS pro distribuci přes obchod s aplikacemi. Obsahuje podrobný průvodce, který popisuje postup přípravy vaší aplikace pro distribuci, jak používat nástroje společnosti Apple k odeslání aplikace ke kontrole a, nakonec, jak publikovat aplikace k obchodu s aplikacemi."
ms.topic: article
ms.prod: xamarin
ms.assetid: DFBCC0BA-D233-4DC4-8545-AFBD3768C3B9
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 08/23/2017
ms.openlocfilehash: dfa3d1f89d813f2e57863e615c701cd78c655ac0
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 03/09/2018
---
# <a name="publishing-to-the-app-store"></a>Publikování do obchodu s aplikacemi

_Tento článek ukazuje, jak nakonfigurovat, vytvářet a publikovat aplikace pro Xamarin.iOS pro distribuci přes obchod s aplikacemi. Obsahuje podrobný průvodce, který popisuje postup přípravy vaší aplikace pro distribuci, jak používat nástroje společnosti Apple k odeslání aplikace ke kontrole a, nakonec, jak publikovat aplikace k obchodu s aplikacemi._

V pořadí distribuovat aplikace pro všechna zařízení iOS, Apple vyžaduje, aby publikovat prostřednictvím aplikace *App Store*, což nákupní centrální umístění pro aplikace pro iOS App Storu. S více než 500 000 aplikace v úložišti velkými písmeny vývojáři mnoho typů aplikací v masivním úspěch tento jediný bod rozdělení. Obchod s aplikacemi je to řešení na klíč, nabídky vývojáři aplikací distribuce a platebních systémy.

Proces odesílání aplikace k obchodu s aplikacemi zahrnuje:

1. Vytvoření **ID aplikace** a výběrem **oprávnění**.
2. Vytváření **distribuční profil pro zřizování**.
3. Pomocí tohoto profilu k sestavení aplikace.
4. Odesílá se vaše aplikace prostřednictvím **iTunes Connect**.


Tento článek se zabývá všechny kroky potřebné k zřizovat, vytvoření a odeslání aplikace pro distribuci obchodu s aplikacemi.

## <a name="before-you-submit-an-application"></a>Před odesláním aplikace

Po odeslání aplikace pro publikaci k obchodu s aplikacemi projde kontrolním procesu společností Apple zajistit, že splňuje společnosti Apple pokyny pro kvality a obsahu. Pokud vaše aplikace nesplňuje tyto pokyny, odmítnou Apple ho, po kterém budete muset vyřešit – neshoda citovalo společností Apple a pak znovu odešlete.
Proto být mít co šanci díky tomu prostřednictvím Apple zkontrolujte seznámení se s těmito pokyny a při pokusu přizpůsobit aplikaci na ně. Společnosti Apple pokyny jsou k dispozici zde: [App Store kontrolní pokyny](https://developer.apple.com/appstore/resources/approval/guidelines.html).

Pár věcí sledujte při odesílání aplikace, které jsou:

1. Ujistěte se, že popis aplikace odpovídá funkcí zahrnutých v aplikaci.
2. Test, který není v aplikaci k chybě v rámci normálního využití. To zahrnuje využití na každé zařízení s iOS, které podporujete.


Apple také udržuje seznam tipy odeslání obchodu s aplikacemi. Můžete si přečíst tyto položky ve [distribuci na webu App Store](https://developer.apple.com/appstore/resources/submission/tips.html).

## <a name="configuring-your-application-in-itunes-connect"></a>Konfigurace vaší aplikace v iTunes Connect

[iTunes Connect](https://itunesconnect.apple.com/WebObjects/iTunesConnect.woa) je sada nástrojů pro webové, mimo jiné, správu aplikací iOS na webu App Store. Aplikace Xamarin.iOS bude muset být správně instalační program a nakonfigurované v iTunes připojit před lze odeslat společnosti Apple ke kontrole a nakonec, se uvolní pro prodej, nebo jako bezplatnou aplikaci v App Storu.

Postupujte takto:

1. Ověřte, zda jsou na místě a až datum ve správné smlouvy **smlouvy, daň a bankovnictví** části iTunes připojit k vydání aplikace pro iOS zdarma nebo pro prodej.
2. Vytvořte novou **iTunes připojit záznam** pro aplikaci a zadejte jeho **zobrazovaný název** (jak je vidět na webu App Store).
3. Vyberte **prodej cena** nebo určit, že aplikace budou vydány zdarma.
5. Zajištění čisté, stručného **popis** aplikace včetně její funkce a využívat pro koncového uživatele.
6. Zadejte **kategorie**, **dílčí kategorie**, a **klíčová slova** pomoct uživateli najít vaší aplikace v úložišti aplikací.
7. Zadejte **kontaktujte** a **podporu** adres URL pro váš web vyžaduje společností Apple.
8. Nastavení aplikace **hodnocení**, který je využíván jiným rodičovské kontroly na webu App Store.
9. Nakonfigurovat volitelné obchodu s aplikacemi technologie, jako třeba **herní Centrum** a **nákupy v aplikaci**.

Další podrobnosti najdete v tématu naše [konfigurace aplikace v iTunes Connect](~/ios/deploy-test/app-distribution/app-store-distribution/itunesconnect.md) dokumentaci.

## <a name="preparing-for-app-store-distribution"></a>Příprava pro distribuci obchodu s aplikacemi

K publikování aplikace do obchodu s aplikacemi, musíte nejprve vytvořit pro distribuci, který zahrnuje mnoho kroků. V následujících částech obsahuje všechno potřebné při přípravě aplikace pro Xamarin.iOS pro publikaci tak, aby se dají vytvářet a odešlete ji do obchodu s aplikacemi pro revize a verze.

### <a name="provisioning-for-application-services"></a>Zřizování pro aplikační služby

Apple poskytuje výběr speciální aplikační služby, označované taky jako oprávnění, které je možné aktivovat pro aplikace iOS, pokud vytvoříte jedinečné ID pro ni. Ať používáte vlastní oprávnění, nebo Ne, musíte stále vytvořit jedinečné ID pro vaši aplikaci Xamarin.iOS před publikováním na webu App Store.

Vytvoření ID aplikace a volitelně vybrat oprávnění zahrnuje následující kroky pomocí společnosti Apple založené na webu iOS Provisioning Portal:

1. V **certifikáty, identifikátory a profily** části vyberte **identifikátory** > **ID aplikace**.
2. Klikněte  **+**  tlačítko a zadejte **název** a **ID sady** pro novou aplikaci.
3. Přejděte do dolní části obrazovky a vyberte některou **App Services** , bude nutné ve vaší aplikace pro Xamarin.iOS.
4. Klikněte **pokračovat** tlačítko a následující na obrazovce pokyny pro vytvoření nového ID aplikace.

Kromě výběr a konfigurace požadované aplikace služby při definování vaše ID aplikace, musíte také nakonfigurovat ID aplikace a oprávnění v projektu Xamarin.iOS úpravou i `Info.plist` a `Entitlements.plist` soubory.

Postupujte takto:

1. V **Průzkumníku řešení**, dvakrát klikněte `Info.plist` soubor otevřete pro úpravy.
2. V **iOS cíl aplikací** , zadejte název vaší aplikace a zadejte **identifikátor svazku** jste vytvořili při definování ID aplikace.
3. Uložit změny `Info.plist` souboru.
4. V **Průzkumníku řešení**, dvakrát klikněte `Entitlements.plist` soubor otevřete pro úpravy.
5. Vyberte a nakonfigurujte oprávnění požadované pro vás aplikace pro Xamarin.iOS tak, aby se shodovaly s instalačním programem, které jste provedli výše, pokud je definována ID aplikace.
6. Uložit změny `Entitlements.plist` souboru.

Podrobné pokyny najdete v tématu naše [zřizování pro služby aplikací](~/ios/get-started/installation/device-provisioning/manual-provisioning.md#appservices) dokumentaci.

### <a name="setting-the-store-icons"></a>Nastavení úložiště ikony

Ikony aplikace úložiště by měl nyní doručil katalog asset. Přidání ikony obchodu s aplikacemi, nejprve vyhledat **AppIcon** image nastavit **Assets.xcassets** souboru projektu.

Název požadované ikony v katalogu Asset **obchod** a měl by být **1 024 x 1 024** velikost. Apple uvedli, že ikonu úložiště pro aplikace v katalogu asset nemůže být průhledná ani obsahovat kanálu alfa.

Informace o nastavení ikonu úložiště najdete v části [ikonu Uložit aplikace](~/ios/app-fundamentals/images-icons/app-store-icon.md) průvodce.

### <a name="setting-the-apps-icons-and-launch-screens"></a>Nastavení ikony aplikace a spuštění obrazovky

Pro aplikace pro iOS třeba přijmout společností Apple pro zařazení do obchodu s aplikacemi vyžaduje správné ikony a spuštění obrazovky pro všechny IOS zařízení, která bude systémem. Ikony aplikace jsou přidány do vašich projektů v katalog asset prostřednictvím **AppIcon** image nastavit **Assets.xcassets** souboru. Spuštění obrazovky jsou přidány prostřednictvím scénáře.

Podrobné pokyny pro vytvoření ikony aplikace a spuštění obrazovky, najdete v článku [ikona aplikace](~/ios/app-fundamentals/images-icons/app-icons.md) a [spusťte obrazovky](~/ios/app-fundamentals/images-icons/launch-screens.md) příručky.

### <a name="creating-and-installing-a-distribution-profile"></a>Vytvoření a instalace profil distribuce

používá iOS *profily zřizování* k řízení nasazení sestavení konkrétní aplikace. Jsou to soubory, které obsahují informace o certifikát použitý k podepsání aplikace, *ID aplikace*, kde se dají nainstalovat aplikaci. Pro vývoj a distribuci ad-hoc profil zřizování také zahrnuje seznam povolených zařízení, do kterých můžete aplikaci nasadit. Pro distribuci obchodu s aplikacemi, pouze informace ID certifikátu a aplikace jsou však součástí, vzhledem k tomu, že je pouze mechanismus pro distribuci veřejného prostřednictvím App Storu.

Zřizování zahrnuje následující kroky pomocí společnosti Apple webové iOS Provisioning Portal:

1.  Vyberte **zřizování** > **distribuční**.
2.  Klikněte  **+**  tlačítko a vyberte typ profil distribuce, kterou chcete vytvořit jako **obchod**.
3.  Vyberte **ID aplikace** z rozevíracího seznamu, který chcete vytvořit profil distribuce pro.
4.  Vyberte platný certifikát pro podepsání aplikace produkční (distribuce).
5.  Zadejte **název** pro nové **profil distribuce** a generovat profil.
6.  V systému Mac, otevřete na Xcode a přejděte do **Předvolby > [vyberte Apple ID] > Zobrazit podrobnosti...** . Stažení všech dostupných profilů v Xcode na Mac.
7.  Návratový IDE a v části **iOS podepisování sady** možností vyberte profil zřizování distribuce pro správný _sestavení konfigurace_ (to bude buď **obchod**nebo **verze**).

Podrobné pokyny najdete v tématu [vytváření profil distribuce](~/ios/get-started/installation/device-provisioning/manual-provisioning.md#provisioningprofile) a [vyberete profil distribuce v projektu Xamarin.iOS](~/ios/deploy-test/app-distribution/app-store-distribution/index.md#selectprofile).

## <a name="setting-the-build-configuration-for-your-application"></a>Nastavení konfigurace sestavení pro aplikaci

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

Postupujte takto:

1. Klikněte pravým tlačítkem na **název projektu** v **řešení Pad** a výběr **možnost** k jejich otevření k úpravám.
2. Vyberte **iOS sestavení** a vyberte **verze | iPhone** z **konfigurace** rozevíracího seznamu:

    ![](publishing-to-the-app-store-images/configurevs01.png "Vyberte z rozevíracího seznamu konfigurace AppStore")

3. Pokud máte na iOS konkrétní verzi, která cílení, můžete vybrat z **verze sady SDK** rozevíracího seznamu, else ponechte tuto hodnotu ve výchozím nastavení.
4. Propojování snižuje celkové velikosti vaší aplikace distribuovatelného tím odstraňování se nepoužité metody, vlastnosti třídy, atd. a ve většině případů by měl být ponecháno na výchozí hodnotu **pouze sestavení SDK odkaz**. V některých situacích, jako např. kdy některé konkrétní použití 3. stran knihovny, budete muset nastavit hodnotu **není odkaz** zachovat požadované prvky. odebrání. Další informace najdete v části [iOS sestavení mechanismy](~/ios/deploy-test/ios-build-mechanics.md) průvodce.
5. **Soubory ve formátu PNG optimalizovat pro iOS** by měl být zaškrtnuto, jako to vám pomůže další zmenšete velikost dodávky vaší aplikace.
6. Ladění by _není_ povolit sestavení budou provedeny zbytečně velké.
8. Pro iOS 11, budete muset vyberte jednu z architektury zařízení, které podporuje **ARM64**. Další informace o vytváření pro zařízení s iOS 64bitová verze, najdete v tématu **povolení 64 Bit sestavení z aplikace na platformě Xamarin.iOS** části [32 nebo 64bitový platformy aspekty](~/cross-platform/macios/32-and-64/index.md) dokumentaci.
9. Můžete volitelně použít **LLVM** kompilátoru, která vytvoří kód menší a rychlejší, ale bude trvat déle, než se zkompilovat.
10. Podle potřeb vaší aplikace, může také chcete upravit typ **uvolňování paměti** používá a nastavení pro **internacionalizace**.
11. Uloží změny do konfigurace sestavení.

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Ve výchozím nastavení, když vytvoříte novou aplikaci Xamarin.iOS v sadě Visual Studio _sestavení konfigurace_ se vytvářejí automaticky pro obě **Ad Hoc** a **obchod** nasazení. Před provedením poslední sestavení aplikace, který se odesílá do společnosti Apple, existuje několik změna, která budete muset provést základní konfiguraci.

Postupujte takto:

1. Klikněte pravým tlačítkem na **název projektu** v **Průzkumníku řešení** a výběr **vlastnosti** k jejich otevření k úpravám.
2. Vyberte **iOS sestavení** a **AppStore** (nebo **verze** Pokud AppStore není k dispozici) z **konfigurace** rozevíracího seznamu:

    ![](publishing-to-the-app-store-images/configurevs01.png "Vyberte z rozevíracího seznamu konfigurace AppStore")

3. Pokud máte na iOS konkrétní verzi, která cílení, můžete vybrat z **verze sady SDK** rozevíracího seznamu, else ponechte tuto hodnotu ve výchozím nastavení.
4. Propojování snižuje celkové velikosti vaší aplikace distribuovatelného tím odstraňování se nepoužité metody, vlastnosti třídy, atd. a ve většině případů by měl být ponecháno na výchozí hodnotu **pouze sestavení SDK odkaz**. V některých situacích, jako např. kdy některé konkrétní použití 3. stran knihovny, budete muset nastavit hodnotu **není odkaz** zachovat požadované prvky. odebrání. Další informace najdete v části [iOS sestavení mechanismy](~/ios/deploy-test/ios-build-mechanics.md) průvodce.
5. **Soubory ve formátu PNG optimalizovat pro iOS** by měl být zaškrtnuto, jako to vám pomůže další zmenšete velikost dodávky vaší aplikace.
6. Ladění by _není_ povolit sestavení budou provedeny zbytečně velké.
7. Klikněte na **Upřesnit** karty:

    ![](publishing-to-the-app-store-images/configurevs02.png "Karta Upřesnit")

8. Pokud vaše aplikace Xamarin.iOS je cílení na zařízení s iOS 8 a 64 bit iOS, budete muset vyberte jednu z architektury zařízení, které podporuje **ARM64**. Další informace o vytváření pro zařízení s iOS 64bitová verze, najdete v tématu **povolení 64 Bit sestavení z aplikace na platformě Xamarin.iOS** části [32 nebo 64bitový platformy aspekty](~/cross-platform/macios/32-and-64/index.md) dokumentaci.
9. Můžete volitelně použít **LLVM** kompilátoru, která vytvoří kód menší a rychlejší, ale bude trvat déle, než se zkompilovat.
10. Podle potřeb vaší aplikace, může také chcete upravit typ **uvolňování paměti** používá a nastavení pro **internacionalizace**.
11. Uloží změny do konfigurace sestavení.

-----

## <a name="building-and-submitting-the-distributable"></a>Vytváření a odesílání distribuovatelného

Pomocí aplikace Xamarin.iOS správně nakonfigurovány nyní jste připraveni udělat poslední distribuční sestavení, který se odesílá, aby Apple ke kontrole a verzi.

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

### <a name="build-your-archive"></a>Sestavení vašem archivu

1. Vyberte **verze | Zařízení** konfigurace v sadě Visual Studio pro Mac:

    ![](publishing-to-the-app-store-images/buildxs01new.png "Vyberte verze | Konfigurace zařízení")
1. Z **sestavení** nabídce vyberte možnost **archivu pro publikování**:

    ![](publishing-to-the-app-store-images/buildxs02new.png "Vyberte archivu pro publikování")

1. Po vytvoření archivu **archivy** zobrazení se zobrazí:

    ![](publishing-to-the-app-store-images/buildxs03new.png "Zobrazí se zobrazení archivy")


> [!NOTE]
> Poznámka: Při starý _obchod_ a _Ad Hoc_ konfigurace teď byly odebrány ze všech pro Mac šablony projektů sady Visual Studio, můžete zjistit, že starší projekty stále zahrnuje tyto konfigurace . Pokud je to tento případ, můžete nadále používat **App Store | Zařízení** konfigurace v kroku 1 seznamu výše.

### <a name="sign-and-distribute-your-app"></a>Podepisování a distribuce aplikace

 Pokaždé, když vytváříte aplikace pro archiv, se automaticky otevře **archivy zobrazení**, zobrazení všech archivovány projekty; seskupené podle řešení. Ve výchozím nastavení toto zobrazení uvádí jenom aktuální, otevřete řešení. Pokud chcete zobrazit všechna řešení, které mají archivy, klikněte na **zobrazit všechny archivy** možnost.

 Aby se zachovala archivy nasazované pro zákazníky (obchodu s aplikacemi nebo Enterprise nasazení), se doporučuje, aby všechny ladicí informace, které jsou generovány můžete symbolized později.

 Podepište aplikaci, a příprava pro distribuci:


1. Vyberte **přihlásit a distribuovat...** , ilustrované níže:

    ![](publishing-to-the-app-store-images/buildxs04new.png "Vyberte znaménko a distribuovat...")

1. Otevře se Průvodce přidáním publikování. Vyberte **obchod** distribuční kanál vytvořit balíček a otevřete zavaděč aplikací:

    ![](publishing-to-the-app-store-images/distribute01.png "Otevřete aplikaci zavaděče")

1. Na obrazovce profil zřizování vyberte podepisování identity a odpovídající profil pro zřizování nebo znovu podepsat pomocí jiné identity:

    ![](publishing-to-the-app-store-images/distribute02.png "Vyberte podpisový identity a odpovídající profil pro zřizování")

1. Zkontrolujte podrobnosti vašeho balíčku a klikněte na tlačítko **publikovat** uložit vaše `.ipa` balíčku:

    ![](publishing-to-the-app-store-images/distribute03.png "Zkontrolujte podrobnosti o balíčku")

1. Jednou vaše `.ipa` byl uložen, je aplikace připravená k odeslání do iTunes připojit prostřednictvím zavaděč aplikací:

    ![](publishing-to-the-app-store-images/distribute04.png "Na obrazovce publikaci proběhlo úspěšně.")

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Modul plug-in Xamarin pro Visual Studio aktuálně nepodporuje archivaci pracovního postupu pro publikování aplikace pro iOS k obchodu s aplikacemi. V důsledku toho máte nahrávání IPA vytvořené prostřednictvím **sestavení Ad hoc IPA** příkaz, který je popsán níže.


## <a name="build-an-ipa"></a>Sestavení IPA

 Tato část popisuje vytváření IPA podobná pracovní postup při použití Ad Hoc nebo distribuční Enterprise. Však budou podepsány, pomocí obchodu s aplikacemi zřizování profilu, který byl vytvořen výše.

 Postupujte takto:

1. V **Průzkumníku řešení**, klikněte pravým tlačítkem na název projektu Xamarin.iOS a vyberte **vlastnosti** k jejich otevření k úpravám:

    ![](publishing-to-the-app-store-images/imagevs01.png "Vyberte vlastnosti")
1. Vyberte **iOS podepisování sady** a změna profilu pro zřizování na obchod s aplikacemi profil pro zřizování:

    ![](publishing-to-the-app-store-images/ipa01.png "Vyberte iOS podepisování sady a změňte profilu pro zřizování na obchod s aplikacemi profil pro zřizování")
1. Vyberte **iOS IPA možnosti** a vyberte **Ad-Hoc** z **konfigurace** rozevíracího seznamu (Pokud Ad-Hoc nezobrazí, vyberte **verze** Místo toho):

    ![](publishing-to-the-app-store-images/imagevs02.png "Vyberte z rozevíracího seznamu konfigurace Ad-Hoc")

1. Volitelně můžete zadat **název balíčku** pro soubor IPA, pokud není zadán, bude mít stejný název jako projektu Xamarin.iOS.
1. Uloží změny do vlastností projektu.
1. Vyberte **Ad Hoc** ze sady Visual Studio pro Mac **sestavení konfigurace** rozevíracího seznamu:

    ![](publishing-to-the-app-store-images/imagevs05.png "Vyberte z rozevíracího seznamu konfigurací sestavení Ad Hoc")
1. Sestavení projektu k vytvoření balíčku IPA.
1. Soubor IPA budou vytvořeny `Bin`  >  _zařízení iOS_  >  `Ad Hoc` složky:

    ![](publishing-to-the-app-store-images/imagevs06.png "Soubor IPA zobrazí v Průzkumníku souborů")

-----


## <a name="customizing-the-ipa-location"></a>Přizpůsobení umístění soubor IPA

Nový **MSBuild** vlastnost `IpaPackageDir` byl přidán do snadno přizpůsobit `.ipa` souboru výstupní umístění. Pokud `IpaPackageDir` do vlastního umístění, je nastaven `.ipa` souboru bude uložena v tomto umístění místo podadresáři označen časovým razítkem výchozí. To může být užitečné při vytváření automatizovaných sestavení, která závisí na cestu k adresáři konkrétní fungovala správně, jako jsou ty používané pro nepřetržitou integraci konfigurace sestavení.

Existuje několik možných způsobech použití nové vlastnosti:

Například k výstupu `.ipa` soubor na původní výchozí adresář (jako Xamarin.iOS 9.6 a nižší), můžete nastavit `IpaPackageDir` vlastnost `$(OutputPath)` pomocí jedné z následujících dvou přístupů. Oba přístupy, které jsou kompatibilní s všechny sestavení Unified Xamarin.iOS rozhraní API, včetně sestavení IDE a také příkazového řádku sestavení, které používají `xbuild`, `msbuild`, nebo `mdtool`:

  - První možností je nastavit `IpaPackageDir` vlastností v rámci `<PropertyGroup>` element v **MSBuild** souboru. Můžete například přidat následující `<PropertyGroup>` k dolnímu okraji projekt aplikace pro iOS `.csproj` souboru (těsně před uzavírací `</Project>` značka):

      ```xml
        <PropertyGroup>
            <IpaPackageDir>$(OutputPath)</IpaPackageDir>
        </PropertyGroup>
      ```
  - Lepším řešením je přidat `<IpaPackageDir>` element k dolnímu okraji existující `<PropertyGroup>` , který odpovídá konfiguraci sloužící k vytvoření `.ipa` souboru. To je lepší, protože jej pro budoucí kompatibility s plánované nastavení na stránce vlastností projektu soubor IPA možnosti iOS připravte projektu. Pokud aktuálně používáte `Release|iPhone` konfiguraci sestavení `.ipa` souboru skupiny dokončení aktualizované vlastností může vypadat podobně jako následující:

      ```xml
      <PropertyGroup Condition=" '$(Configuration)|$(Platform)' == 'Release|iPhone' )
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
Alternativní technika pro `msbuild` nebo `xbuild` sestavení příkazového řádku je přidat `/p:` argument příkazového řádku nastavit `IpaPackageDir` vlastnost. V takovém případě Všimněte si, že `msbuild` nerozšiřuje `$()` výrazy předaná na příkazovém řádku, takže není možné použít `$(OutputPath)` syntaxe. Místo toho je musíte zadat úplnou cestu. Na mono `xbuild` příkaz rozbalte `$()` výrazy, ale je stále vhodnější použít úplnou cestu, protože `xbuild` přestanou nakonec pro [napříč platformami verzi `msbuild` ](http://www.mono-project.com/docs/about-mono/releases/4.4.0/#msbuild-preview-for-os-x)v budoucích verzích. Úplný příklad, který používá tento přístup může vypadat podobně jako následující v systému Windows:

```bash
msbuild /p:Configuration="Release" /p:Platform="iPhone" /p:ServerAddress="192.168.1.3" /p:ServerUser="macuser" /p:IpaPackageDir="%USERPROFILE%\Builds" /t:Build SingleViewIphone1.sln
```

Nebo následující v systému Mac:

```bash
xbuild /p:Configuration="Release" /p:Platform="iPhone" /p:IpaPackageDir="$HOME/Builds" /t:Build SingleViewIphone1.sln
```

S distribuční vytvořit sestavení a archivovat, nyní jste připraveni k odeslání aplikace iTunes připojit.

### <a name="automatically-copy-app-bundles-back-to-windows"></a>Automaticky zkopírujte .app sady zpět do systému Windows

[!include[](~/ios/includes/copy-app-bundle-to-windows.md)]

## <a name="submitting-your-app-to-apple"></a>Odeslání vaší aplikace pro Apple

> [!NOTE]
> Poznámka: Apple nedávno změnila její proces ověření pro aplikace pro iOS a může odmítnout aplikací pomocí `iTunesMetadata.plist` součástí soubor IPA. Pokud dojde k chybě `ERROR: ERROR ITMS-90047: "Disallowed paths ( "iTunesMetadata.plist" ) found at: Payload/iPhoneApp1.app"`popsané řešení [sem](https://forums.xamarin.com/discussion/40388/disallowed-paths-itunesmetadata-plist-found-at-when-submitting-to-app-store/p1) má problém vyřešit.

S distribuční sestavení dokončit jste připraveni k odeslání vaší aplikace pro iOS Apple ke kontrole a uvolněte na webu App Store.

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

 Postupujte takto:

1. Spustit **Xcode**.
2. Z **okno** nabídky vyberte možnost **organizátora**.
3. Klikněte na **archivy** a vyberte archiv, který byl postavený výše:

    ![](publishing-to-the-app-store-images/publishxs01.png "Vyberte archivu k odeslání")
4. Klikněte na **ověření...**  tlačítko.
5. Vyberte účet, který chcete ověřit vůči a klikněte na **zvolte** tlačítko:

    ![](publishing-to-the-app-store-images/publishxs02.png "Vyberte účet, který chcete ověřit vůči")
6. Klikněte **ověřením** tlačítko:

    ![](publishing-to-the-app-store-images/publishxs03.png "Dialogového okna souhrnu souboru")
7. Pokud se vyskytly potíže s sady, bude zobrazovat.
8. Opravte všechny problémy a znovu sestavte archivu v sadě Visual Studio for Mac.
9. Klikněte na **odeslání...**  tlačítko.
10. Vyberte účet, který chcete ověřit vůči a klikněte na **zvolte** tlačítko:

    ![](publishing-to-the-app-store-images/publishxs04.png "Vyberte účet, který chcete ověřit vůči")
11. Klikněte **odeslání** tlačítko:

    ![](publishing-to-the-app-store-images/publishxs05.png "Dialogového okna souhrnu souboru")
12. Xcode bude informovat o tom můžete po dokončení nahrávání souboru pro službu iTunes připojit.


Pracovní postup archivu v sadě Visual Studio pro Mac se zavaděč aplikací automaticky otevře, které jste uložili _.ipa_:

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Odeslání vaší žádosti o Apple ke kontrole se provádí pomocí aplikace zavaděč aplikací. Na hostiteli sestavení Mac, je třeba provést tyto kroky. Můžete si stáhnout kopii zavaděč aplikací z [zde](https://itunesconnect.apple.com/apploader/ApplicationLoader_3.0.dmg).

-----

1. Vyberte *poskytovat aplikace* a klikněte na *zvolte* tlačítko:

    [![](publishing-to-the-app-store-images/publishvs01.png "Vyberte poskytování vaší aplikace")](publishing-to-the-app-store-images/publishvs01.png#lightbox)

2. Vyberte zip nebo soubor IPA vytvořili výše a klikněte na **OK** tlačítko.

3. Zavaděč aplikací budou ověření souboru:

    [![](publishing-to-the-app-store-images/publishvs02.png "Na obrazovce ověření")](publishing-to-the-app-store-images/publishvs02.png#lightbox)
4. Klikněte *Další* tlačítko a aplikace bude ověřovat s obchodu s aplikacemi:

    [![](publishing-to-the-app-store-images/publishvs03.png "Ověření proti obchodu s aplikacemi")](publishing-to-the-app-store-images/publishvs03.png#lightbox)
5. Klikněte **odeslat** tlačítko Odeslat ke kontrole aplikace společnosti Apple.
6. Zavaděč aplikací bude informovat, když byla úspěšně nahrána soubor.

## <a name="itunes-connect-status"></a>iTunes stav připojení

Pokud znovu se přihlásili k iTunes připojit a vybrat aplikaci ze seznamu dostupných aplikací, stav v iTunes Connect by měl zobrazit nyní, že je **čekání zkontrolujte** (dočasně lze číst **nahrát přijata** je zpracování):

[![](publishing-to-the-app-store-images/image21.png "Tento stav v iTunes připojení byste nyní měli vidět, že čekáte ke kontrole")](publishing-to-the-app-store-images/image21.png#lightbox)

## <a name="summary"></a>Souhrn

Tento článek zobrazí podrobný návod, jak konfiguraci, sestavování a žádá o App Store publikace. Nejprve popsané kroky potřebné k vytvoření a nainstalovat distribuční profil pro zřizování. V dalším kroku ji projít použití sady Visual Studio a Visual Studio pro Mac vytvořit distribuční sestavení. Nakonec se ukázal, jak používat iTunes Connect a nástroje k odesílání aplikace k obchodu s aplikacemi.


## <a name="related-links"></a>Související odkazy

- [Práce s obrázky](~/ios/app-fundamentals/images-icons/index.md)
- [iOS aplikace vývoj pracovní postup průvodce: distribuce aplikace](http://developer.apple.com/library/ios/#documentation/Xcode/Conceptual/ios_development_workflow/35-Distributing_Applications/distributing_applications.html)
- [Tipy pro odeslání obchodu s aplikacemi](https://developer.apple.com/appstore/resources/submission/tips.html)
- [Běžné aplikace zamítnutí](https://developer.apple.com/app-store/review/rejections/)
- [Pokyny pro recenze v App Storu](https://developer.apple.com/appstore/resources/approval/guidelines.html)
