---
title: Pomocí Xamarin Team města
description: Tento průvodce popisuje kroky s použitím TeamCity kompilaci mobilních aplikací a odesílat je na Xamarin Test Cloud.
ms.prod: xamarin
ms.assetid: AC2626CB-28A7-4808-B2A9-789D67899546
author: topgenorth
ms.author: toopge
ms.date: 03/23/2017
ms.openlocfilehash: e7279c03c730e95f211b555e5b832942c19ea8aa
ms.sourcegitcommit: 26033c087f49873243751deded8037d2da701655
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 06/25/2018
ms.locfileid: "36935412"
---
# <a name="using-team-city-with-xamarin"></a>Pomocí Xamarin Team města

_Tento průvodce popisuje kroky s použitím TeamCity kompilaci mobilních aplikací a odesílat je na Xamarin Test Cloud._

Jak je popsáno v [Úvod do průběžnou integraci](~/tools/ci/intro-to-ci.md) příručce průběžnou integraci (CI) je osvědčeným postupem při vývoji mobilních aplikací kvality. Celá řada možností přijatelná pro nepřetržitou integraci serverového softwaru; Tato příručka je zaměřena na [TeamCity](http://www.jetbrains.com/teamcity/) z JetBrains.

Existuje několik různých permutací TeamCity instalace. Následuje seznam některých z těchto:

- **Služba systému Windows** – v tomto scénáři, spustí TeamCity až při spuštění systému Windows jako služby systému Windows. Musí být spárována s hostitelem sestavení Mac zkompilovat všechny aplikace pro iOS.

- **Spuštění démona OS X** – koncepčně, to je velmi podobné ke spuštění jako služba Windows popsané v předchozím kroku. Ve výchozím nastavení spustí pomocí kořenového účtu.

- **Uživatelský účet v OS X** – je možné spustit TeamCity pod účtem uživatele, který spuštění při každém přihlášení uživatele.

Předchozí scénářů systémem TeamCity pod uživatelským účtem OS X je nejjednodušší a nejsnadnější instalační program.

Jsou spojené s nastavením TeamCity několik kroků:

- **Instalace TeamCity** – instalace TeamCity není součástí této příručky. Tato příručka předpokládá, že TeamCity je nainstalován a spuštěn pod uživatelským účtem. Pokyny, [instalace TeamCity](http://confluence.jetbrains.com/display/TCD8/Installation) lze nalézt v [TeamCity 8 dokumentaci](http://confluence.jetbrains.com/display/TCD8/TeamCity+Documentation) podle JetBrains.

- **Příprava serveru sestavení** – tento krok zahrnuje instalaci potřebný software, nástrojů a certifikáty potřebné k vytváření mobilních aplikací a připravte je pro distribuci.

- **Vytváření A sestavit skript** – tento krok není nezbytně nutné, ale užitečné podpory k sestavení aplikací bezobslužné je skript buildu. Pomocí skriptu sestavení vám pomůže s odstraňování problémů sestavení, které mohou vzniknout a zajišťuje konzistentní, opakovatelných způsob, jak vytvořit binární soubory pro distribuci, i když není practiced průběžnou integraci.

- **Vytvoření projektu A TeamCity** – po dokončení předchozí tři kroky jsou nutné vytvoříme TeamCity projekt, který bude obsahovat všechny meta-data potřebné k načtení zdrojový kód, kompilace projektů a odeslání testy, abyste Xamarin Test Cloud.

## <a name="requirements"></a>Požadavky

Prostředí s [Xamarin Test Cloud](https://developer.xamarin.com/guides/testcloud) je vyžadován.

Znalost TeamCity 8.1 je povinný. Instalace TeamCity je nad rámec tohoto dokumentu. Předpokládá se, zda je nainstalována na OS X Mavericks TeamCity a je spuštěná s pověřeními běžného uživatelského účtu a není kořenového účtu.

Sestavení server by měl být samostatný počítač, se systémem OS X, který je vyhrazen pro nepřetržité integrace. V ideálním případě by server sestavení nebude zodpovědná za jiných rolí, například databázového serveru, webového serveru nebo pracovní stanice pro vývojáře.

> [!IMPORTANT]
> Tato příručka nepopisuje "bezobslužných" instalace nástroje Xamarin.

[!include[](~/tools/ci/includes/firewall-information.md)]

## <a name="preparing-the-build-server"></a>Příprava serveru sestavení

Nainstalujte všechny potřebné nástroje, softwaru a certifikátů k vytvoření mobilní aplikace je zásadní krok v konfiguraci serveru sestavení. Je důležité, aby server sestavení schopný zkompilovat mobilní řešení a spustit všechny testy. Chcete-li minimalizovat problémy s konfigurací, musí být ve stejném účtu uživatele, který je hostitelem TeamCity nainstalován software a nástroje. Následuje seznam se požaduje:

1. **Visual Studio pro Mac** – to zahrnuje Xamarin.iOS a Xamarin.Android.
2. **Přihlášení k úložišti součástí Xamarin** – tento krok je volitelný a je pouze vyžaduje, pokud vaše aplikace používá součásti z úložišti součástí Xamarin. Proaktivně protokolování do úložiště součásti v tomto okamžiku zabrání všechny problémy, když se pokusí TeamCity sestavení kompilace aplikace.
3. **Xcode** – Xcode je nutný k kompilace a podepsat aplikace pro iOS.
4. **Nástroje příkazového řádku Xcode** – to je popsaný v kroku 1 v části instalace [aktualizace Ruby s rbenv](https://developer.xamarin.com/guides/testcloud/calabash/updating-ruby-using-rbenv/) průvodce.
5. **Podepisování Identity & profily zřizování** – naimportovat certifikáty a zřizování profilu prostřednictvím XCode. Naleznete v Průvodci společnosti Apple na [export podepisování identity a profilů zřizování](https://developer.apple.com/library/ios/recipes/xcode_help-accounts_preferences/articles/export_signing_assets.html) další podrobnosti.
6. **Android Keystores** – zkopírujte do adresáře, zda má uživatel TeamCity tj přístup, požadované Android keystores `~/Documents/keystores/MyAndroidApp1`.
7. **Calabash** – Toto je volitelný krok, pokud má vaše aplikace vytvořené pomocí Calabash testy. Najdete v tématu [instalaci Calabash na OS X Mavericks](https://developer.xamarin.com/guides/testcloud/calabash/osx-installation/) průvodce a [aktualizace Ruby s rbenv](https://developer.xamarin.com/guides/testcloud/calabash/updating-ruby-using-rbenv/) Průvodce pro další informace.

Následující diagram znázorňuje všechny tyto součásti:

![](teamcity-images/image1.png "Tento diagram znázorňuje všechny tyto součásti")

Jakmile veškerý software nainstalován, přihlaste se k uživatelskému účtu a potvrdit, že veškerý software správně nainstalován a pracuje. To by měla zahrnovat kompilování řešení a podáním žádosti o testovací Cloud. To může výrazně zjednodušit spuštěním skriptu buildu, jak je popsáno v následující části.

## <a name="create-a-build-script"></a>Vytvoření skriptu sestavení

Přestože je šíři TeamCity pro zpracování všech aspektů kompilování a odesílání mobilní aplikace pro testovací Cloud samostatně, důrazně doporučujeme vytvořit skript sestavení. Skript sestavení nabízí následující výhody:

1. **Dokumentace** – skript sestavení slouží jako formulář dokumentace na tom, jak je software založený. Touto akcí odeberete některé "magic", která je přiřazena k nasazení aplikace a umožňuje vývojářům soustředit se na funkce.
1. **Opakovatelnosti** – skriptu buildu zajistí, že pokaždé, když je aplikace zkompilovat a nasadit, odehrává se přesně stejným způsobem, bez ohledu na to, kdo nebo co funguje. Tato opakovatelných konzistence odebere problémy nebo chybách, ke kterým může ovládat v důsledku nesprávně spuštění sestavení nebo lidským chybám.
1. **Správa verzí** – skriptu buildu můžou být součástí správy zdrojového kódu. To znamená, že změny skriptu buildu možné sledovat, sledovat a opravy v případě chyby nebo nepřesnosti nebyly nalezeny.
1. **Příprava prostředí** – skript sestavení může obsahovat logiku k instalaci všech požadovaných 3. stran závislosti. Tím bude zajištěno, že jsou aplikace vytvořené s nástroji správné komponenty.

Skript sestavení může být stejně jednoduché jako soubor prostředí Powershell (v systému Windows) nebo bash skript (na OS X). Při vytváření skriptu buildu, existuje několik možností pro skriptovací jazyky:

- [**Převislým** ](https://github.com/jimweirich/rake) – je to konkrétní jazyk domény (DSL) pro sestavování projektů, podle Ruby. Převislým má výhodu v podobě oblíbenosti a bohatý ekosystém knihovny.

- [**psake** ](https://github.com/psake/psake) – to je knihovna prostředí Windows Powershell pro tvorbu softwaru

- [**ZFALŠOVAT** ](http://fsharp.github.io/FAKE/) – to je DSL, na základě v jazyce F # umožňuje využívat stávající knihovny .NET, v případě potřeby.

Skriptovací jazyk, který se používá závisí na vašich předvoleb a požadavky. [TaskyPro Calabash](https://github.com/xamarin/test-cloud-samples/tree/master/TaskyPro/TaskyPro-Calabash) příklad obsahuje příklad použití převislým jako [sestavení skriptu](https://github.com/xamarin/test-cloud-samples/blob/master/TaskyPro/TaskyPro-Calabash/Rakefile).

> [!NOTE]
> Je možné použít sestavení systému založeného na XML jako je například MSBuild nebo NAnt, ale tyto nedostatečná expressiveness a udržovatelnosti DSL, který je vyhrazen pro vytváření softwaru.

### <a name="parameterizing-the-build-script"></a>Parametrizace skriptu buildu

Proces vytváření a testování softwaru vyžaduje informace, které by měly být udržovány tajný. V případě vytváření APK může vyžadovat heslo pro úložiště klíčů a alias klíče v úložišti klíčů. Obdobně testovací Cloud vyžaduje klíč rozhraní API, které jsou jedinečné pro vývojáře. Tyto typy hodnot by neměly být pevný programového ve skriptu buildu. Místo toho budou by měla být předána jako proměnné skriptu buildu.

Méně citlivou jsou hodnoty, jako je iOS ID zařízení nebo ID zařízení se systémem Android, které určují, co zařízení, která testování cloudu by měl použít pro testování běží. Tyto nejsou hodnoty, které musí být chráněny, ale se můžou změnit na sestavení.

Ukládání těchto typů proměnných mimo skriptu buildu také usnadňuje sdílení sestavení skript v rámci organizace, například s vývojáři. Vývojáři mohou použít přesný stejného skriptu jako server sestavení, ale můžete použít vlastní keystores a klíče rozhraní API.

Existují dvě možnosti pro ukládání těchto citlivé hodnoty:

- **Konfigurační soubor** – k ochraně klíč rozhraní API cloudu Test, který je tato hodnota by neměla být zaškrtnuta do správy verzí. Soubor můžete vytvořit pro každý počítač. Jak hodnoty se čtou z tohoto souboru závisí na skriptovací jazyk používaný.

- **Proměnné prostředí** – tyto lze snadno nastavit u jednotlivých počítačů a ared bez ohledu na základní skriptovací jazyk.

Existují výhody a nevýhody každé z těchto možností. TeamCity vhodně pracuje s proměnné prostředí, tak tato příručka doporučí Tato technika, při vytváření sestavení skripty.

### <a name="build-steps"></a>Kroky sestavení

Sestavení skriptu musí být schopni provést následující kroky:

- **Kompilace aplikace** – to zahrnuje podepsání aplikace s správného profilu zřizování.

- **Odeslání aplikace pro Xamarin Test Cloud** – to zahrnuje podepisování a zip zarovnání APK s příslušnou úložiště klíčů.

Tyto dva kroky budou vysvětlené podrobněji níže.

#### <a name="compiling-a-xamarinios-application"></a>Kompilování aplikace pro Xamarin.iOS

[!include[](~/tools/ci/includes/commandline-compile-of-xamarin-ios-ipa.md)]

#### <a name="compiling-a-xamarinandroid-application"></a>Kompilování aplikace pro Xamarin.Android

Kompilace aplikace platformy Android, použijte **xbuild** (nebo **msbuild** v systému Windows):

```bash
/Library/Frameworks/Mono.framework/Commands/xbuild /t:SignAndroidPackage /p:Configuration=Release /path/to/android.csproj
```

Všimněte si, že ke kompilaci aplikace Xamarin Android **xbuild** používá projekt a že k vytvoření aplikace systému iOS **xbuild** vyžaduje řešení.

#### <a name="submitting-xamarinuitests-to-test-cloud"></a>Odesílání Xamarin.UITests testovací cloudu

UITests jsou odeslány pomocí `test-cloud.exe` aplikaci, jak je znázorněno v následujícím fragmentu kódu:

```bash
test-cloud.exe <path-to-apk-or-ipa-file> <test-cloud-team-api-key> --devices <device-selection-id> --assembly-dir <path-to-tests-containing-test-assemblies> --nunit-xml report.xml --user <email>
```

Při spuštění testu, bude vrácen výsledků testů ve formátu souboru XML styl NUnit názvem **report.xml**. TeamCity se zobrazí informace v protokolu sestavení.

Další informace o tom, jak odeslat UITests testovací cloudu obrátit na tato příručka [Příprava Xamarin.UITests pro nahrávání](/appcenter/test-cloud/preparing-for-upload/uitest/).

#### <a name="submitting-calabash-tests-to-test-cloud"></a>Odesílání Calabash testů k testování cloudu

Calabash testů jsou odeslány pomocí `test-cloud` gem, jak je znázorněno v následujícím fragmentu kódu:

```bash
test-cloud submit /path/to/APK-or-IPA <test-cloud-team-api-key> --devices <device-id> --user <email>
```
Pokud chcete odeslat aplikace platformy Android testování cloudu, je třeba nejprve znovu sestavit server APK test pomocí calabash android:

```bash
$ calabash-android build </path/to/signed/APK>
$ test-cloud submit /path/to/APK <test-cloud-team-api-key> --devices <ANDROID_DEVICE_ID> --profile=android --config=config/cucumber.yml --pretty
```
Další informace o odesílání Calabash testy, přečtěte si je Xamarin Průvodce na [odesílání Calabash testů pro testovací Cloud](https://developer.xamarin.com/guides/testcloud/calabash/working-with/submitting-tests-to-xamarin-test-cloud/).

## <a name="creating-a-teamcity-project"></a>Vytvoření projektu TeamCity

Jakmile je nainstalována TeamCity a Visual Studio pro Mac, můžete vytvořit projekt, je čas vytvoření projektu v TeamCity pro sestavení projektu a odešlete ji do cloudu testu.

1. Spustí protokolování do TeamCity prostřednictvím webového prohlížeče. Přejděte do kořenové projektu:

    ![Přejděte do projektu kořenové](teamcity-images/image2.png "přejděte do projektu kořenové") pod kořenového projektu vytvořte nový projekt dílčí:

    ![Přejděte do kořenové projektu pod v kořenové projektu, vytvořte nový projekt dílčí](teamcity-images/image3.png "přejděte do kořenové projektu pod v kořenové projektu vytvořte nový projekt dílčí")
2. Po vytvoření dílčí projekt, přidejte novou konfiguraci sestavení:

    ![Po vytvoření dílčí projekt, přidejte novou konfiguraci sestavení](teamcity-images/image5.png "po vytvoření dílčí projekt, přidejte novou konfiguraci sestavení")
3. Připojte konfigurace sestavení projektu VC. To se provádí prostřednictvím na obrazovce nastavení řízení verze:

    ![To se provádí prostřednictvím na obrazovce nastavení řízení verze](teamcity-images/image6.png "to se provádí prostřednictvím nastavení řízení verze obrazovky")

    Pokud není žádný VC projekt vytvořen, máte možnost vytvořit jednu ze stránky nové kořenové VC vidíte níže:

    ![Pokud není žádný VC projekt vytvořen, máte možnost vytvořit jednu ze stránky nové kořenové VC](teamcity-images/image7.png "Pokud neexistuje žádný projekt VC vytvořen, máte možnost vytvořit jednu ze stránky nové kořenové VC")

    Kroky sestavení rozpoznat, jakmile byla přiřazena kořenu VC, TeamCity bude najdete v článku věnovaném projektu a pokuste se automaticky. Pokud jste obeznámeni s TeamCity, pak můžete vybrat jeden z kroků zjištěné sestavení. Je bezpečné prozatím ignorovat kroky zjištěné sestavení.

4. V dalším kroku nakonfigurujte vytvořit aktivační událost. Sestavení se to fronty, pokud jsou splněny určité podmínky, například pokud uživatel potvrdí kód do úložiště. Následující snímek obrazovky ukazuje, jak přidat aktivační událost sestavení:

    ![Tato obrazovka ukazuje, jak přidat aktivační událost sestavení](teamcity-images/image8.png "tento snímek obrazovky ukazuje, jak přidat aktivační událost sestavení") příklad konfigurace aktivační události sestavení můžete vidět na následujícím snímku obrazovky:

    ![Příklad konfigurace aktivační událost sestavení se zobrazí v tomto snímku obrazovky](teamcity-images/image9.png "na tomto snímku obrazovky můžete zobrazit příklad konfigurace aktivační události sestavení")

5. V předchozí části Parametrizace sestavit skript navrhované ukládání některé hodnoty jako proměnné prostředí. Tyto proměnné lze přidat na konfiguraci sestavení prostřednictvím obrazovce parametry. Přidání proměnné pro klíč rozhraní API testu cloudu, ID zařízení iOS a Android ID zařízení, jak ukazuje následující snímek obrazovky:

    ![Přidání proměnné pro klíč rozhraní API testu cloudu, ID zařízení iOS a Android ID zařízení](teamcity-images/image11.png "přidejte proměnné pro klíč rozhraní API testu cloudu, ID zařízení iOS a Android ID zařízení")

6. Posledním krokem je přidání kroku sestavení, který bude vyvolat skriptu buildu zkompilovat aplikace a zařazování aplikace pro testovací Cloud. Na následujícím snímku obrazovky je příklad krok sestavení, který používá Rakefile pro vytvoření aplikace:

    ![Tomto snímku obrazovky je příklad krok sestavení, který používá k vytvoření aplikace Rakefile](teamcity-images/image12.png "tomto snímku obrazovky je příklad krok sestavení, který používá Rakefile pro vytvoření aplikace")

7. V tomto okamžiku je konfigurace sestavení je dokončena. Je vhodné aktivovat build a ověřte, že projekt správně nakonfigurován. Dobrým způsobem, jak to udělat je malý, zanedbatelný změnu potvrdit do úložiště. TeamCity by měl zjistit potvrzení a spuštění sestavení.

8. Po dokončení sestavení, zkontrolujte v protokolu sestavení a zda existují nějaké problémy nebo upozornění s sestavení, které vyžadují pozornost.

## <a name="summary"></a>Souhrn

Tato příručka zahrnutých použití TeamCity k sestavení Xamarin mobilních aplikací a odesílat je na Test Cloud. Jsme se bavili vytváření sestavení skriptu pro automatizaci procesu sestavení. Skriptu buildu postará kompilace aplikace, odesílá do cloudu Test a čeká se výsledky

Potom jsme popsané postupy k vytvoření projektu v TeamCity, který bude ve frontě sestavení pokaždé, když vývojář potvrdí kódu a zavolá skriptu buildu.

## <a name="related-links"></a>Související odkazy

- [Příprava Xamarin.UITests zaregistrovaných nahrávání](/appcenter/test-cloud/preparing-for-upload/uitest/)
- [Instalace a konfigurace TeamCity](http://confluence.jetbrains.com/display/TCD8/Installing+and+Configuring+the+TeamCity+Server)
