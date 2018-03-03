---
title: Podpora IPA
description: "Tento článek vysvětluje postup vytvořit soubor IPA, který lze použít k nasazení aplikace pomocí Ad Hoc distribuce pro testování nebo pro interní distribuci interních aplikací."
ms.topic: article
ms.prod: xamarin
ms.assetid: D253C2DB-852E-6FC6-C9FD-574730B8DB19
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/19/2017
ms.openlocfilehash: bee3480fc90c2eac5629e336c57daa90adf9c346
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 02/27/2018
---
# <a name="ipa-support"></a>Podpora IPA

_Tento článek vysvětluje postup vytvořit soubor IPA, který lze použít k nasazení aplikace pomocí Ad Hoc distribuce pro testování nebo pro interní distribuci interních aplikací._

Kromě vydání aplikace pro prodej přes iTunes App Storu, můžete nasadit pro následující účely:

- **Ad Hoc testování** – aplikace pro iOS, mohou být nasazeny na až 100 uživatelů (identifikovaný zařízení s iOS konkrétní identifikátory UUID) pro platformu Alpha a pro účely testování verze Beta. V tématu naše [zřizování iOS zařízení pro vývoj](~/ios/get-started/installation/device-provisioning/manual-provisioning.md#provisioning) dokumentaci podrobné informace o přidávání testovacích zařízení iOS ke svému účtu vývojáře Apple a [Ad-Hoc](~/ios/deploy-test/app-distribution/ad-hoc-distribution.md) průvodce, další informace o tom, Chcete-li distribuovat tímto způsobem.
- **Interně nebo podnikové nasazení** – aplikace pro iOS můžete nasadit interně ve společnosti, které vyžaduje členství ve společnosti Apple Developer Enterprise program. Další informace o In úklidové distribuce je podrobně popsaná v [In úklidové distribuční](~/ios/deploy-test/app-distribution/in-house-distribution.md) průvodce.

V obou případech musíte vytvořit a digitálně podepsané pomocí správného profilu zřizování distribuční balíček IPA (speciální typ souboru zip). Tento článek popisuje kroky potřebné k sestavení balíčku soubor IPA a instalaci balíčku na zařízení s iOS přes iTunes na Mac nebo počítači s Windows.

<a name="iTunesMetadata" />

## <a name="the-itunesmetadataplist-file"></a>Soubor iTunesMetadata.plist

Vytvoření aplikace pro iOS v iTunes Connect (buď pro prodej nebo bezplatnou verzi z obchodu iTunes App) má vývojář můžete zadat informace, jako je aplikace genre, dílčí genre, o autorských právech, zařízení podporovaných iOS a vyžaduje zařízení Možnosti.

aplikace iOS, které se dodávají buď přes Ad Hoc nebo interní distribuční, musí být nějakým způsobem pro podporu těchto informací tak, aby bylo možné v iTunes a na zařízení uživatele viditelná. Ve výchozím nastavení je soubor malé iTunesMetadata.plist vytvoří při každém sestavení projektu a je uložen v adresáři projektu.

Vlastní **iTunesMetadata.plist** můžete také vytvořit zadat dodatečné informace, které distribuční. Chcete-li získat další informace o obsah tohoto souboru a jak ji vytvořit, najdete v tématu naše [obsah iTunesMetadata.plist](~/ios/deploy-test/app-distribution/itunesmetadata.md#iTunesMetadata_contents) a [vytváření iTunesMetadata.plist souboru](~/ios/deploy-test/app-distribution/itunesmetadata.md#iTunesMetadata_creating) dokumentaci.

<a name="iTunesArtwork" />

## <a name="itunes-artwork"></a>iTunes kresby

Při doručování aplikace způsoby App Store, musíte taky zahrnout 512 x 512 a 1 024 x 1 024 bitovou kopii, která se používá k reprezentování aplikace v iTunes.

Pokud chcete zadat iTunes kresby, postupujte takto:

1. Dvakrát klikněte **Info.plist** souboru v **Průzkumníku řešení** otevřete pro úpravy.
2. Přejděte k položce **iTunes kresby** části editoru.
3. Pro všechny chybějící bitovou kopii, klikněte na miniaturu v editoru, vyberte soubor bitové kopie pro požadovanou iTunes kresby z **otevření souboru** dialogové okno a kliknutím **OK** nebo **otevřete** tlačítko.
4. Tento krok opakujte až všechny potřeby byly nastaveny obrázky pro vaši aplikaci.

Podrobnosti najdete [iTunes kresby](~/ios/app-fundamentals/images-icons/app-icons.md) dokumentace pro další podrobnosti.

<a name="createipa" />

## <a name="creating-an-ipa"></a>Vytvoření IPA

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

Vytváření soubor IPA je nyní integrována do nový pracovní postup publikování. To pokud chcete udělat, postupujte podle pokynů k archivaci vaší aplikace, podepište ho a uložte soubor IPA je vaše.

Před spuštěním vytvoření IPA pro různé platformy řešení, ujistěte se, že jste vybrali jako spouštěný projekt projekt pro iOS:

![](ipa-support-images/setasstartup.png "Vybrat jako projekt po spuštění projektu pro iOS")

### <a name="build-your-archive"></a>Sestavení vašem archivu

K vytvoření IPA, _archivu_ verze sestavení naše aplikace musí být vytvořen. Archivu obsahuje naše aplikace a identifikační informace o něm.


1. Vyberte **verze | Zařízení** konfigurace v sadě Visual Studio pro Mac:!

    ![](ipa-support-images/buildxs01new.png "Vyberte verze | Konfigurace zařízení")

1. Z **sestavení** nabídce vyberte možnost **archivu pro publikování**:

    ![](ipa-support-images/buildxs02new.png "Vyberte archivu pro publikování")

1. Po vytvoření archivu **archivy** zobrazení se zobrazí:

    ![](ipa-support-images/buildxs03new.png "Zobrazí se zobrazení archivy")


### <a name="sign-and-distribute-your-app"></a>Podepisování a distribuce aplikace

Pokaždé, když vytváříte aplikace pro archiv, se automaticky otevře **archivy zobrazení**, zobrazení všech archivovány projekty; seskupené podle řešení. Ve výchozím nastavení v tomto zobrazení se zobrazí pouze aktuální, otevřete řešení. Pokud chcete zobrazit všechna řešení, které mají archivy, klikněte na **zobrazit všechny archivy** možnost.

Aby se zachovala archivy nasazované pro zákazníky (Ad-Hoc nebo interní nasazení), se doporučuje, aby všechny ladicí informace, které jsou generovány můžete symbolized později.

Všimněte si, že pro App Store navazuje **iTunesMetadata.plist** souboru a nastavte kresby iTunes bude tento automaticky zahrnut ve vašem IPA Pokud se nacházejí v archivu.

Podepište aplikaci, a příprava pro distribuci:


1. Vyberte **přihlásit a distribuovat...**  tlačítko znázorněné dole:

    ![](ipa-support-images/buildxs04new.png "Vyberte znaménko a distribuovat...")

1. Otevře se Průvodce přidáním publikování. Vyberte **Ad-Hoc** nebo **Enterprise**(interní) distribuční kanál k vytvoření balíčku:

    ![](ipa-support-images/distribute01.png "Vyberte Ad-Hoc nebo podnikové interní distribuční")

1. Na obrazovce profil zřizování vyberte podepisování identity a odpovídající profil pro zřizování nebo znovu podepsat pomocí jiné identity:

    ![](ipa-support-images/distribute02.png "Vyberte podpisový identity a odpovídající profil pro zřizování")

1. Zkontrolujte podrobnosti vašeho balíčku a klikněte na tlačítko **publikovat**:

    ![](ipa-support-images/distribute03.png "Zkontrolujte podrobnosti o balíčku")

1. Nakonec uložte vaše IPA k počítači:

    ![](ipa-support-images/distribute04.png "Uložit soubor IPA do počítače")


### <a name="building-via-the-command-line-on-mac"></a>Vytváření pomocí příkazového řádku (v systému Mac)

V některých případech například v prostředí CI ho může být nutné k sestavení můžete soubor IPA prostřednictvím příkazového řádku. Použijte následující postup k dosažení tohoto cíle:


1. Ujistěte se, **možnosti projektu > iOS možnosti IPA > zahrnují Image iTunesArtwork** je zaškrtnuta možnost a **sestavení ad-hoc/enterprise balíček (soubor IPA)** je zaškrtnuta možnost:

    ![](ipa-support-images/imagexs04.png "Zahrnují iTunesArtwork Image a sestavení ad-hoc/enterprise balíček, který soubor IPA je zaškrtnuté.")

    Pokud dáváte přednost, místo toho můžete upravit **.csproj** soubor v textovém editoru a ručně přidejte dva odpovídající vlastnosti, které chcete `PropertyGroup` konfigurace, který bude použit k vytvoření aplikace:

    ```xml
    <BuildIpa>true</BuildIpa>
    <IpaIncludeArtwork>false</IpaIncludeArtwork>
    ```

1. Chcete-li zahrnout volitelné **iTunesMetadata.plist** souboru, klikněte **...**  tlačítko, vyberte ho ze seznamu a klikněte **OK** tlačítko:

     ![](ipa-support-images/imagexs03.png "Vyberte iTunesMetadata.plist ze seznamu")

1. Volání **xbuild** (nebo **mdtool** pro klasické rozhraní API) přímo a předejte tuto vlastnost na příkazovém řádku:

    ```bash
    /Library/Frameworks/Mono.framework/Commands/xbuild YourSolution.sln /p:Configuration=Ad-Hoc /p:Platform=iPhone /p:BuildIpa=true
    ```

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Jakmile profilu pro zřizování byl vytvořen a vybrán, volitelné **iTunesMetadata.plist** byl vytvořen soubor a iTunes kresby nastavení v sadě Visual Studio, můžete vytvořit soubor IPA je pro distribuci. Potom budete muset nakonfigurujete svůj projekt. Postupujte takto:

1. V **Průzkumníku řešení**, klikněte pravým tlačítkem na název projektu Xamarin.iOS a vyberte **vlastnosti** k jejich otevření k úpravám:

    ![](ipa-support-images/imagevs01.png "Vyberte vlastnosti")

2. Vyberte **iOS IPA možnosti** a vyberte **Ad-Hoc** z **konfigurace** rozevíracího seznamu:

    ![](ipa-support-images/imagevs02.png "Vyberte z rozevíracího seznamu konfigurace Ad-Hoc")

    > [!NOTE]
> Konfigurace aplikace Ad-Hoc nemusí být dostupné pro projekty novější Xamarin.iOS. Pokud není k dispozici, vyberte **verze** konfigurace.

3. Chcete-li zahrnout možnost **iTunesMetadata.plist** souboru, klikněte **...**  tlačítko, vyberte ho ze seznamu a klikněte **otevřete** tlačítko:

    ![](ipa-support-images/imagevs03.png "Vyberte iTunesMetadata.plist ze seznamu")

4. Volitelně můžete zadat **název balíčku** pro soubor IPA, pokud není zadán, bude mít stejný název jako projektu Xamarin.iOS.
5. Uloží změny do vlastností projektu.
6. Vyberte **Ad Hoc** z **sestavení konfigurace** rozevíracího seznamu, pokud je k dispozici. V opačném případě vyberte **verze**:

    ![](ipa-support-images/imagevs05.png "Vyberte z rozevíracího seznamu konfigurací sestavení Ad Hoc")

7. Sestavení projektu k vytvoření balíčku IPA.
8. Soubor IPA budete stavět **Bin > zařízení iOS > Ad Hoc (nebo verze)** složky:

    ![](ipa-support-images/imagevs06.png "Soubor IPA v Průzkumníkovi souborů")

-----

<a name="Customizing-the-IPA-Location" />

## <a name="customizing-the-ipa-location"></a>Přizpůsobení umístění soubor IPA

Nový **MSBuild** vlastnost `IpaPackageDir` byl přidán do snadno přizpůsobit **.ipa** souboru výstupní umístění. Pokud `IpaPackageDir` do vlastního umístění, je nastaven **.ipa** souboru bude uložena v tomto umístění místo podadresáři označen časovým razítkem výchozí. To může být užitečné při vytváření automatizovaných sestavení, která závisí na cestu k adresáři konkrétní fungovala správně, jako jsou ty používané pro nepřetržitou integraci konfigurace sestavení.

Existuje několik možných způsobech použití nové vlastnosti:

Například k výstupu **.ipa** soubor na původní výchozí adresář (jako Xamarin.iOS 9.6 a nižší), můžete nastavit `IpaPackageDir` vlastnost `$(OutputPath)` pomocí jedné z následujících dvou přístupů. Oba přístupy, které jsou kompatibilní s všechny sestavení Unified Xamarin.iOS rozhraní API, včetně sestavení IDE a také příkazového řádku sestavení, které používají **xbuild**, **msbuild**, nebo **mdtool**:

- První možností je nastavit `IpaPackageDir` vlastností v rámci `<PropertyGroup>` element v **MSBuild** souboru. Můžete například přidat následující `<PropertyGroup>` k dolnímu okraji projekt aplikace pro iOS **.csproj** souboru (těsně před uzavírací `</Project>` značka):

    ```xml
    <PropertyGroup>
        <IpaPackageDir>$(OutputPath)</IpaPackageDir>
    </PropertyGroup>
    ```

- Lepším řešením je přidat `<IpaPackageDir>` element k dolnímu okraji existující `<PropertyGroup>` , který odpovídá konfiguraci sloužící k vytvoření **.ipa** souboru. To je lepší, protože jej pro budoucí kompatibility s plánované nastavení na stránce vlastností projektu soubor IPA možnosti iOS připravte projektu. Pokud aktuálně používáte `Release|iPhone` konfiguraci sestavení **.ipa** souboru skupiny dokončení aktualizované vlastností může vypadat podobně jako následující:

    ```xml
    <PropertyGroup Condition=" '$(Configuration)|$(Platform)' == 'Release|iPhone' ">
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

Alternativní technika pro **msbuild** nebo **xbuild** sestavení příkazového řádku je přidat `/p:` argument příkazového řádku nastavit `IpaPackageDir` vlastnost. V takovém případě Všimněte si, že **msbuild** nerozšiřuje `$()` výrazy předaná na příkazovém řádku, takže není možné použít `$(OutputPath)` syntaxe. Místo toho je musíte zadat úplnou cestu. Na mono **xbuild** příkaz rozbalte `$()` výrazy, ale je stále vhodnější použít úplnou cestu, protože **xbuild** přestanou nakonec pro [ napříč platformami verzi **msbuild** ](http://www.mono-project.com/docs/about-mono/releases/4.4.0/#msbuild-preview-for-os-x) v budoucích verzích.

Úplný příklad, který používá tento přístup může vypadat podobně jako následující v systému Windows:


```bash
msbuild /p:Configuration="Release" /p:Platform="iPhone" /p:ServerAddress="192.168.1.3" /p:ServerUser="macuser" /p:IpaPackageDir="%USERPROFILE%\Builds" /t:Build SingleViewIphone1.sln
```

Nebo následující v systému Mac:

```bash
xbuild /p:Configuration="Release" /p:Platform="iPhone" /p:IpaPackageDir="$HOME/Builds" /t:Build SingleViewIphone1.sln
```

<a name="installipa" />

## <a name="installing-an-ipa-using-itunes"></a>Instalace IPA pomocí iTunes

Výsledný soubor IPA balíček mohou být doručeny do testovací uživatele k instalaci na svoje zařízení s iOS nebo zasílány pro podnikové nasazení. Bez ohledu na to, jakou metodu jste vybrali, koncový uživatel nainstaluje balíček ve svých aplikacích iTunes na jejich Mac nebo počítači s Windows dvojitým kliknutím soubor IPA souboru (nebo na okno otevřít iTunes).

Nová aplikace iOS bude zobrazen v **Moje aplikace** část, kde můžete klikněte pravým tlačítkem myši na něm a získat informace o aplikaci:

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

 ![](ipa-support-images/installxs01.png "Nová aplikace iOS v části Moje aplikace")

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

 ![](ipa-support-images/installvs01.png "Nová aplikace iOS v části Moje aplikace")

-----

Uživatel teď můžou synchronizovat iTunes s jejich zařízení k instalaci nové aplikace iOS.

<a name="Summary" />

## <a name="summary"></a>Souhrn

Tento článek zahrnutých nastavení potřebné při přípravě aplikace pro Xamarin.iOS pro sestavení App Store. To vám ukázal, jak vytvořit balíček soubor IPA a má postup instalace výsledné iOS aplikace na zařízení s iOS koncového uživatele pro testování nebo interní distribuční.


## <a name="related-links"></a>Související odkazy

- [Distribuce v obchodě App Store](~/ios/deploy-test/app-distribution/app-store-distribution/index.md)
- [Konfigurace aplikace v iTunes Connect](~/ios/deploy-test/app-distribution/app-store-distribution/itunesconnect.md)
- [Publikování do obchodu App Store](~/ios/deploy-test/app-distribution/app-store-distribution/publishing-to-the-app-store.md)
- [Interní distribuční](~/ios/deploy-test/app-distribution/in-house-distribution.md)
- [Jednorázová distribuce](~/ios/deploy-test/app-distribution/ad-hoc-distribution.md)
- [Soubor iTunesMetadata.plist](~/ios/deploy-test/app-distribution/itunesmetadata.md)
- [Odstraňování potíží](~/ios/deploy-test/troubleshooting.md)
- [iTunes kresby](~/ios/app-fundamentals/images-icons/app-icons.md#itunes)
- [Distribuce firemních aplikací pro zařízení s iOS](http://developer.apple.com/library/ios/#featuredarticles/FA_Wireless_Enterprise_App_Distribution/Introduction/Introduction.html)
