---
title: Nasazení watchOS aplikace k obchodu s aplikacemi
description: Tento dokument popisuje postup nasazení watchOS aplikací vytvořených pomocí Xamarinu k obchodu s aplikacemi. Podívejte se na distribuční zřizovacích profilů a iTunes připojení trvá a poskytuje také Rady pro odstraňování potíží.
ms.prod: xamarin
ms.assetid: DBE16040-70D2-4F61-B5F3-C8D213DBC754
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/17/2017
ms.openlocfilehash: 70613c79c2ec0c81f1dbdc218b747f809f859767
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 06/05/2018
ms.locfileid: "34790981"
---
# <a name="deploying-watchos-apps-to-the-app-store"></a>Nasazení watchOS aplikace k obchodu s aplikacemi

> [!IMPORTANT]
> Nezapomeňte si přečíst [společnosti Apple sledovat Kit odeslání průvodce](https://developer.apple.com/app-store/watch/)a zobrazit [Poradce při potížích s](#Troubleshooting) oddíl pro všechny problémy, může mít.

- Ujistěte se, že máte:
  - [**Profily zřizování distribuční** ](#provisioning) vytvořené pro projekty.
  - **Cíl nasazení** (`MinimumOSVersion`) pro iOS nadřazená aplikace nastaven na **8.2** nebo dřívější (8.3 není podporována.).

- V [ **iTunes připojit**](#iTunes_Connect):

  - Vytvoření položky aplikace pro iOS (nebo přidejte **novou verzi** do existující aplikace).
  - Přidejte ikona sledování a snímky.

- Potom v [Visual Studio pro Mac](#xamarin_studio) (Visual Studio není aktuálně podporována.):

  - Klikněte pravým tlačítkem na aplikaci pro iOS a vyberte **nastavit jako spouštěný projekt**.
  - Změnit na **obchod** konfigurace.
  - Použití **archivu** funkce vytvořit archiv aplikace.

- Nakonec přepnout [Xcode 6.2 +](#xcode)

  - Přejděte na **okno > organizátora** a zvolte **archivy**.
  - Vyberte ze seznamu aplikací a archivu.
  - (Volitelně) **Ověření...**  archivu.
  - **Odeslání...**  archivu a postupujte podle kroků k nahrání do iTunes připojit ke kontrole a schválení.

Přečtěte si konkrétní typy související s tyto položky níže. Najdete v článku [Poradce při potížích s](#Troubleshooting) část, pokud máte potíže.

<a name="provisioning" />

## <a name="distribution-provisioning-profiles"></a>Distribuce profily zřizování

K sestavení pro nasazení obchodu s aplikacemi, je nutné vytvořit **profil zřizování distribuce** pro daná ID aplikace ve vašem řešení.

Pokud máte zástupný znak ID aplikace, *jenom jeden profil zřizování se bude vyžadovat*; ale pokud máte samostatnou ID aplikace pro každý projekt, pak pro každý ID aplikace budete potřebovat profil pro zřizování:

![](appstore-images/provisioningprofile-distribution-sml.png "Profil distribuce úložiště aplikací")

Po vytvoření všechny tři profily, se objeví v seznamu. Mějte na paměti, stáhněte a nainstalujte každé z nich (poklepáním na něm):

![](appstore-images/provisioningprofiles-sml.png "V seznamu dostupných profilů")

Můžete ověřit v profilu pro zřizování **možnosti projektu** výběrem **sestavení > iOS podepisování sady** obrazovky a výběrem **AppStore | iPhone** konfigurace.

**Profil zřizování** seznamu se zobrazí všechny profily odpovídající – byste měli vidět odpovídající profily, které jste vytvořili v tomto rozevíracím seznamu.

![](appstore-images/options-selectprofile-sml.png "Dialogové okno podepisování sady iOS")

<a name="iTunes_Connect"/>

## <a name="itunes-connect"></a>iTunes Connect

Postupujte podle [přehled distribuce aplikace](~/ios/deploy-test/app-distribution/index.md), zejména:

- [Konfigurace aplikace v iTunes Connect](~/ios/deploy-test/app-distribution/app-store-distribution/itunesconnect.md)
- [Publikování do obchodu App Store](~/ios/deploy-test/app-distribution/app-store-distribution/publishing-to-the-app-store.md)

Při konfiguraci aplikace v iTunes připojení, nezapomeňte přidat ikona sledování a snímky:

![](appstore-images/itunesconnect-watch-sml.png "Ikona sledování a snímky obrazovky v iTunes Connect")

Soubor ikony musí být 1 024 x 1 024 pixelů a bude mít masku cyklické u, jakmile se zobrazí. Ikona by neměl mít kanálu alfa.

Alespoň jeden snímek je vyžadován, může být až pět odeslána.
Jejich 312 x 390 pixelů a předvedení sledovat aplikaci v akci.
Kukátko simulátoru 42mm můžete pořizovat snímky obrazovky při této velikosti.


<a name="xamarin_studio" />

## <a name="visual-studio-for-mac"></a>Visual Studio for Mac

1. Zkontrolujte, zda je aplikace pro iOS spouštěný projekt. Pokud ne, klikněte pravým tlačítkem na ho nastavit:

  ![](appstore-images/xs-startup.png "Nastavení spuštění projektu")

2. Vyberte **AppStore** konfigurace sestavení:

  ![](appstore-images/xs-appstore.png "Konfigurace sestavení AppStore")

3. Vyberte **sestavení > archivu** položky nabídky ke spuštění procesu archivu:

  ![](appstore-images/xs-archive.png "V nabídce sestavení")

Můžete také **zobrazení > archivy...**  položku nabídky zobrazíte archivy, které jste předtím vytvořili.

  ![](appstore-images/xs-archives-sml.png "Zobrazení archivy")

<a name="xcode" />

## <a name="xcode"></a>Xcode

Xcode se automaticky zobrazí archivy vytvořené v sadě Visual Studio for Mac.

1. Spusťte Xcode a vyberte **okno > organizátora**:

  ![](appstore-images/xc-organizer.png "Nabídky okna")

2. Přepnout **archivy** a vyberte archiv, který byl vytvořen pomocí sady Visual Studio pro Mac:

  ![](appstore-images/xc-archives.png "Na kartě archivy")

3. Volitelně **ověření...**  archivu, zvolte **odeslání...**  pro nahrání aplikace pro službu iTunes připojit.

4. Zvolte vývojový tým (Pokud jste patřit do více než jedné) a pak potvrďte odesílání:

  ![](appstore-images/xc-submit1.png "V části team vývoj")

5. Navštivte iTunes připojit znovu můžete zobrazit nahrané binárního souboru. Přejděte na stránku konfigurace vaší aplikace a zvolte **předběžné verze** v hlavní nabídce zobrazíte **sestavení** seznamu:

  [![](appstore-images/itc-prerelease-sml.png "Stránka Konfigurace aplikace v iTunes Connect")](appstore-images/itc-prerelease.png#lightbox)

Aplikace pro schválení lze pak odeslat na **verze** stránky. Odkazovat [přehled distribuce aplikace iOS](~/ios/deploy-test/app-distribution/index.md) Další informace.


## <a name="troubleshooting"></a>Poradce při potížích

Tady jsou některé chyby, které mohou nastat během odesílání do obchodu s aplikacemi a kroky, které vám je opravit.

### <a name="archive-menu-option-is-not-visible-in-visual-studio-for-mac"></a>Možnost nabídky archivu není zobrazená v sadě Visual Studio pro Mac

Postupujte podle [výše uvedené kroky](#xamarin_studio) ke konfiguraci řešení pro archivaci. Pokud projekt po spuštění nelze nastavit správně, ověřte, že je konfigurace sestavení je nejprve nastavit možnost ladění a vydání před pokusem o změnit počáteční projekt. Vyberte v seznamu je konfigurace sestavení zpět do **AppStore**.

### <a name="invalid-icon"></a>Ikona neplatné

```csharp
Invalid Icon - The watch application '...watchkitextension.appex/WatchApp.app'
contains an icon file '...watchkitextension.appex/WatchApp.app/AppIcons27.5x27.5@2x.png'
with an alpha channel. Icons should not have an alpha channel.
```

Postupujte podle [pokyny k odebrání alfa kanálu](~/ios/watchos/troubleshooting.md) z vaší ikon.

### <a name="cfbundleversion-mismatch"></a>Neshoda CFBundleVersion

```csharp
CFBundleVersion Mismatch. The CFBundleVersion value '1' of watch application
'...watchkitextension.appex/WatchApp.app' does not match the CFBundleVersion
value '1.0' of its containing iOS application `YouriOS.app`.
```

Všechny projekty v řešení – aplikace pro iOS, sledovat rozšíření a sledování aplikace - musí používat stejné číslo verze. Upravit každou **Info.plist** tak, aby přesně odpovídá číslo verze.

### <a name="missing-icons"></a>Chybí ikony

```csharp
Missing Icons. No icons found for watch application '...watchkitextension.appex/WatchApp.app'.
Please make sure that its Info.plist file includes entries for CFBundleIconFiles.
```

Postupujte podle pokynů [práce s ikonami](~/ios/watchos/app-fundamentals/icons.md) přidat všechny požadované bitové kopie do projektu aplikace sledovat.

### <a name="missing-icon"></a>Chybí ikony

```csharp
Missing Icon. The watch application '...watchkitextension.appex/WatchApp.app'
is missing icon with name pattern '*44x44@2x.png' (Home Screen 42mm).
```

Zajistěte, abyste měli nejnovější verzi sady Visual Studio pro Mac a že vaše **AppIcons.appiconset** obsahuje kompletní sadu bitové kopie. Pokud tato chyba se přesto zobrazuje, zobrazte si zdroj **Contents.json** a ověřit tak obsahuje položku pro všechny požadované obrázky. Případně, zkontrolujte, že používáte nejnovější verzi Xamarin, odstranit a znovu vytvořit **AppIcons.appiconset**.

> [!IMPORTANT]
> Není známého problému v sadě Visual Studio pro podporu ikonu Sledování pro Mac: očekává, že obrázek 88 x 88 pixelů pro **29x29@3x** bitové kopie (což by měl být pixelů 87 x 87).


Nelze vyřešit v sadě Visual Studio pro Mac – buď upravit zdroj obrázku v Xcode nebo ručně upravit, pokud **Contents.json** souboru (tak, aby odpovídaly [této ukázce](https://github.com/xamarin/monotouch-samples/blob/master/WatchKit/WatchKitCatalog/WatchApp/Resources/Images.xcassets/AppIcons.appiconset/Contents.json#L126-L132)).



### <a name="invalid-watchkit-support"></a>Neplatný WatchKit podpory

```csharp
Invalid WatchKit Support - The bundle contains an invalid implementation of WatchKit.
The app may have been built or signed with non-compliant or pre-release tools.
```

Může se zobrazit tato během ověření a odeslání, nebo automatizované e-mailem z iTunes připojit po zdánlivě úspěšné odeslání.
<!--
Ensure you are using the latest version of Xcode and Xamarin's tools.
-->
> [!IMPORTANT]
> Je nutné **archivu** vaší aplikace v sadě Visual Studio pro Mac a potom přepnout na Xcode 6.2 + ověření a odeslat do iTunes připojit.


Použijte kanál stabilní Xamarin a Xcode 6.2 +.



### <a name="invalid-provisioning-profile"></a>Neplatný profil pro zřizování

```csharp
Invalid Provisioning Profile. The provisioning profile included in the bundle
...iOSWatchApp.watchkitapp [iOSWatchApp.app/PlugIns/...iOSWatchApp.watchkitextension.appex/WatchApp.app]
is invalid. [Missing code-signing certificate.]
```

**Profily zřizování distribuční** je nutné zadat pro všechny tři projekty v řešení sledování aplikací: aplikace pro iOS, sledovat rozšíření a sledování aplikace – buď explicitně (tři profily) nebo prostřednictvím profilu jeden zástupný znak. Zkontrolujte, že zřizovacích profilů existují v iOS Dev Center a že jste stáhli a přidat je do vašeho Mac.

### <a name="invalid-code-signing-entitlements"></a>Neplatná oprávnění pro podpis kódu

```csharp
ITMS-90046: Invalid Code Signing Entitlements. Your application bundle's signature contains
code signing entitlements that are not supported on iOS. Specifically, value
'...watchkitextension' for key 'application-identifier' in '...watchkitextension'
is not supported. The value should be a string startign with your TEAMID, followed
by a dot '.' followed by the bundle identifier.
```

Ujistěte se zřizovacích profilů nastavení správně ve vývojářském centru Apple a že jste stáhli a byly nainstalovány. Zkontrolujte také, že jsou nastaveny v sadě Visual Studio pro Mac na vlastnosti – okno pro každý projekt.

### <a name="invalid-architecture"></a>Neplatný architektura

```csharp
Invalid architecture: Apps that include an app extension
and framework must support arm64.
```

Aplikace pro sledování můžete přidat pouze [unifikované API (64 bitů)](~/cross-platform/macios/unified/index.md) aplikace na platformě Xamarin.iOS.
Klikněte pravým tlačítkem na projekt aplikace pro iOS potom přejděte na stránku **možnosti > sestavení > iOS sestavení > karta Upřesnit** a ujistěte se, že **podporované architektury** pro AppStore iPhone konfigurace zahrnuje **ARM64** (např. **ARMv7 + ARM64**).

### <a name="this-bundle-is-invalid"></a>Tato sada je neplatný.

```csharp
ITMS-90068: This bundle is invalid. The value provided for the key
MinimumOSVersion '8.3' is not acceptable.
```

Aplikace iOS nadřazené musí mít MinimumOSVersion nastaven na '8.2, nebo starší.

### <a name="non-public-api-usage"></a>Použití jiné veřejné rozhraní API

```csharp
Your app contains non-public API usage.
Please review the errors, and resubmit your application.
```

Ujistěte se, že používáte nejnovější verzi Xcode a Xamarin pro nástroje.
Váš kód by neměl přístup žádné jiné veřejné rozhraní API.

### <a name="build-error-mt5309"></a>Chyba MT5309 sestavení

```csharp
Error MT5309: Native linking error: clang: error: no such file or directory:
```

Tato chyba je pravděpodobně výsledkem vaší s přejmenovat instalaci Xcode z **Xcode.app**. Například tato chyba nastane, Pokud přejmenujete na vaše instalace **XCode 6.2.app**.



## <a name="related-links"></a>Související odkazy

- [Průvodce odeslání WatchKit Apple](https://developer.apple.com/app-store/watch/)
