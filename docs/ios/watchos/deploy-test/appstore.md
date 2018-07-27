---
title: Nasazení watchOS aplikace do App Store
description: Tento dokument popisuje, jak nasadit aplikace pro watchOS vytvořených pomocí Xamarinu pro App Store. To se podíváme na distribuční zřizovací profily a iTunes Connect a nabízí také Rady pro odstraňování potíží.
ms.prod: xamarin
ms.assetid: DBE16040-70D2-4F61-B5F3-C8D213DBC754
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/17/2017
ms.openlocfilehash: 90058f5074759fdded5d259004cb40c0cb7ea212
ms.sourcegitcommit: ffb0f3dbf77b5f244b195618316bbd8964541e42
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/26/2018
ms.locfileid: "39276064"
---
# <a name="deploying-watchos-apps-to-the-app-store"></a>Nasazení watchOS aplikace do App Store

> [!IMPORTANT]
> Nezapomeňte si přečíst [společnosti Apple Watch Kit odeslání průvodce](https://developer.apple.com/app-store/watch/)a podívejte se [Poradce při potížích s](#Troubleshooting) oddílu pro všechny problémy, které máte uzavřeny.

- Ujistěte se, že máte:
  - [**Distribuční zřizovací profily** ](#provisioning) vytvořené pro vaše projekty.
  - **Cíl nasazení** (`MinimumOSVersion`) pro iOS nadřazená aplikace nastavená na **8.2** nebo starší (8.3 není podporována).

- V [ **iTunes Connect**](#iTunes_Connect):

  - Vytvoření položky aplikace pro iOS (nebo přidat **novou verzi** do existující aplikace).
  - Přidáte ikona sledování a snímky obrazovky.

- Potom v [Visual Studio for Mac](#xamarin_studio) (Visual Studio není aktuálně podporováno.):

  - Klikněte pravým tlačítkem na aplikaci pro iOS a zvolte **nastavit jako spouštěný projekt**.
  - Přejděte **App Store** konfigurace.
  - Použití **archivu** funkce vytvořit archiv aplikace.

- A konečně, přepněte na [Xcode 6.2 +](#xcode)

  - Přejděte **okna > médií** a zvolte **archivy**.
  - Vyberte ze seznamu aplikací a archivu.
  - (Volitelně) **Ověření...**  archivu.
  - **Odeslání...**  archiv a postupujte podle uvedeného postupu nahrajte do služby iTunes Connect ke kontrole a schválení.

Přečtěte si konkrétní tipy týkající se těchto položek níže. Zobrazit [Poradce při potížích s](#Troubleshooting) části, pokud máte problémy.

<a name="provisioning" />

## <a name="distribution-provisioning-profiles"></a>Distribuční zřizovací profily

K sestavení pro nasazení App Store, je potřeba vytvořit **distribuční zřizovací profil** pro každé ID aplikace ve vašem řešení.

Pokud máte zástupný znak ID aplikace *jenom jeden profil zřizování se bude vyžadovat*; ale pokud máte samostatnou ID aplikace pro každý projekt, musíte zřizovacího profilu pro každé ID aplikace:

![](appstore-images/provisioningprofile-distribution-sml.png "Profil distribuce App Store")

Po vytvoření všech tří profilů, se objeví v seznamu. Nezapomeňte si stáhněte a nainstalujte každé z nich (na něj poklikejte na něj):

![](appstore-images/provisioningprofiles-sml.png "Seznamu dostupných profilů")

Můžete ověřit v profilu zřizování **možnosti projektu** tak, že vyberete **sestavení > podepsání sady prostředků aplikace pro iOS** obrazovky a vyberete **AppStore | iPhone** konfigurace.

**Zřizovací profil** v seznamu se zobrazí všechny odpovídající profily – měli byste vidět odpovídající profily, které jste vytvořili v tomto rozevíracím seznamu.

![](appstore-images/options-selectprofile-sml.png "Dialogové okno s Iosem podepisování sad prostředků")

<a name="iTunes_Connect"/>

## <a name="itunes-connect"></a>iTunes Connect

Postupujte podle [přehled distribuce aplikace](~/ios/deploy-test/app-distribution/index.md), zejména:

- [Konfigurace aplikace v iTunes Connectu](~/ios/deploy-test/app-distribution/app-store-distribution/itunesconnect.md)
- [Publikování do obchodu App Store](~/ios/deploy-test/app-distribution/app-store-distribution/publishing-to-the-app-store.md)

Při konfiguraci aplikace ve službě iTunes Connect, nezapomeňte si přidat ikona sledování a snímky obrazovky:

![](appstore-images/itunesconnect-watch-sml.png "Ikona sledování a snímky obrazovky ve službě iTunes Connect")

Soubor ikony musí být 1 024 x 1 024 pixelů a bude mít cyklické maska u, jakmile se zobrazí. Ikona by neměl mít kanál alfa.

Alespoň jeden screenshot vyžaduje se, až 5 může být odeslána.
Tyto by měl být 312 x 390 pixelů a ukazují aplikace na hodinkách v akci.
Simulátor 42mm watch můžete pořizovat snímky obrazovky v tomto rozsahu.


<a name="xamarin_studio" />

## <a name="visual-studio-for-mac"></a>Visual Studio for Mac

1. Ujistěte se, že aplikace iOS bude projekt po spuštění. Pokud ne, klikněte pravým tlačítkem na ji:

  ![](appstore-images/xs-startup.png "Nastavení projektu po spuštění")

2. Zvolte **AppStore** konfiguraci sestavení:

  ![](appstore-images/xs-appstore.png "Konfigurace sestavení AppStore")

3. Zvolte **sestavení > archivu** položky nabídky k zahájení procesu archivu:

  ![](appstore-images/xs-archive.png "V nabídce sestavení")

Můžete také **zobrazení > archivy...**  položka nabídky Zobrazit archivy, které jste dříve vytvořili.

  ![](appstore-images/xs-archives-sml.png "Zobrazit archivy")

<a name="xcode" />

## <a name="xcode"></a>Xcode

Xcode se automaticky zobrazí archivy vytvořené v sadě Visual Studio pro Mac.

1. Spuštění Xcode a zvolte **okna > médií**:

  ![](appstore-images/xc-organizer.png "Nabídka okna")

2. Přepněte **archivy** kartě a vyberte archiv, který byl vytvořen pomocí sady Visual Studio pro Mac:

  ![](appstore-images/xc-archives.png "Na kartě archivy")

3. Volitelně **ověření...**  archivu, klikněte na tlačítko **odeslání...**  pro nahrání aplikace do služby iTunes Connect.

4. Vybrat vývojový tým (Pokud musíte patřit do více než jeden) a potom Potvrdit odeslání:

  ![](appstore-images/xc-submit1.png "Části týmový vývoj")

5. Navštivte iTunes Connect znovu, abyste viděli nahrané binárního souboru. Přejděte na stránku konfigurace vaší aplikace a zvolte **předběžné verze** z hlavní nabídky zobrazíte **sestavení** seznamu:

  [![](appstore-images/itc-prerelease-sml.png "Na stránce konfigurace aplikací v iTunes Connectu")](appstore-images/itc-prerelease.png#lightbox)

Pak můžete odeslat ke schválení aplikace na **verze** stránky. Odkazovat [přehled distribuce aplikace iOS](~/ios/deploy-test/app-distribution/index.md) Další informace.


## <a name="troubleshooting"></a>Poradce při potížích

Tady jsou některé chyby, které mohou nastat při odesílání do App Store a kroky, které můžete provést a opravte je.

### <a name="archive-menu-option-is-not-visible-in-visual-studio-for-mac"></a>Možnost nabídky Archiv není viditelný v sadě Visual Studio pro Mac

Postupujte podle [výše uvedené kroky](#xamarin_studio) ke konfigurování řešení pro archivaci. Pokud projekt po spuštění nelze nastavit správně, ujistěte se, že je konfigurace sestavení nastavená na ladit nebo vydaná verze, než se pokusíte změnit projekt při spuštění první. Potom nastavte konfiguraci sestavení zpět na **AppStore**.

### <a name="invalid-icon"></a>Neplatná ikona

```csharp
Invalid Icon - The watch application '...watchkitextension.appex/WatchApp.app'
contains an icon file '...watchkitextension.appex/WatchApp.app/AppIcon27.5x27.5@2x.png'
with an alpha channel. Icons should not have an alpha channel.
```

Postupujte podle [pokyny k odebrání alfa kanál](~/ios/watchos/troubleshooting.md) z vaší ikony.

### <a name="cfbundleversion-mismatch"></a>Neshoda CFBundleVersion

```csharp
CFBundleVersion Mismatch. The CFBundleVersion value '1' of watch application
'...watchkitextension.appex/WatchApp.app' does not match the CFBundleVersion
value '1.0' of its containing iOS application `YouriOS.app`.
```

Všechny projekty v řešení - aplikace pro iOS, rozšíření Watch a aplikace na hodinkách - musí používat stejné číslo verze. Upravit každou **Info.plist** souboru tak, aby číslo verze přesně odpovídá.

### <a name="missing-icons"></a>Chybí ikony

```csharp
Missing Icons. No icons found for watch application '...watchkitextension.appex/WatchApp.app'.
Please make sure that its Info.plist file includes entries for CFBundleIconFiles.
```

Postupujte podle pokynů [práce s ikonami](~/ios/watchos/app-fundamentals/icons.md) požadované Image přidat do projektu aplikace Watch.

### <a name="missing-icon"></a>Chybějící ikona

```csharp
Missing Icon. The watch application '...watchkitextension.appex/WatchApp.app'
is missing icon with name pattern '*44x44@2x.png' (Home Screen 42mm).
```

Ujistěte se, máte nejnovější verzi sady Visual Studio pro Mac a že vaše **AppIcon.appiconset** obsahuje kompletní sadu bitových kopií. Pokud stále dochází k této chybě, zobrazte si zdroj **Contents.json** k potvrzení obsahuje položku pro všechny požadované Image. Můžete také, když zajistíte, že používáte nejnovější verzi Xamarinu, odstranit a znovu vytvořit **AppIcon.appiconset**.

> [!IMPORTANT]
> Existuje známého problému v sadě Visual Studio pro Mac Watch ikonu podpory: očekává, že obrázek 88 x 88 pixel **29x29@3x** image (která by měla být 87 x 87 pixelů).


Nelze vyřešit v sadě Visual Studio for Mac – buď úpravy prostředku obrázku v prostředí Xcode nebo ručně upravit **Contents.json** souboru (tak, aby odpovídaly [této ukázce](https://github.com/xamarin/monotouch-samples/blob/master/WatchKit/WatchKitCatalog/WatchApp/Resources/Images.xcassets/AppIcons.appiconset/Contents.json#L126-L132)).



### <a name="invalid-watchkit-support"></a>Neplatný podporu WatchKit

```csharp
Invalid WatchKit Support - The bundle contains an invalid implementation of WatchKit.
The app may have been built or signed with non-compliant or pre-release tools.
```

Tato zpráva se může zobrazit během ověření a odeslání, nebo automatizovaných e-mailu ze služby iTunes Connect po zdánlivě úspěšně nahrávaly.
<!--
Ensure you are using the latest version of Xcode and Xamarin's tools.
-->
> [!IMPORTANT]
> Je nutné **archivu** vaší aplikace v sadě Visual Studio pro Mac a přepněte do Xcode 6.2 + ověřit a nahrát do služby iTunes Connect.


Použití kanálu stabilní Xamarin a Xcode 6.2 +.



### <a name="invalid-provisioning-profile"></a>Platný zřizovací profil

```csharp
Invalid Provisioning Profile. The provisioning profile included in the bundle
...iOSWatchApp.watchkitapp [iOSWatchApp.app/PlugIns/...iOSWatchApp.watchkitextension.appex/WatchApp.app]
is invalid. [Missing code-signing certificate.]
```

**Distribuční zřizovací profily** je nutné zadat pro všechny tři projekty v řešení aplikace watch: aplikace pro iOS, rozšíření Watch a aplikace na hodinkách – buď explicitně (tři profily), nebo prostřednictvím profilu jeden zástupný znak. Zkontrolujte, že existují zřizovací profily IOS Dev Center a že jste stáhli a přidali je do vašeho macu.

### <a name="invalid-code-signing-entitlements"></a>Neplatná oprávnění pro podepisování kódu

```csharp
ITMS-90046: Invalid Code Signing Entitlements. Your application bundle's signature contains
code signing entitlements that are not supported on iOS. Specifically, value
'...watchkitextension' for key 'application-identifier' in '...watchkitextension'
is not supported. The value should be a string startign with your TEAMID, followed
by a dot '.' followed by the bundle identifier.
```

Zkontrolujte zřizovací profily jsou nastavení správně ve vývojářském centru Apple a že jste stáhli a nainstalovali. Zkontrolujte také, že jsou nastaveny v sadě Visual Studio pro Mac v okně Vlastnosti pro každý projekt.

### <a name="invalid-architecture"></a>Neplatná architektura

```csharp
Invalid architecture: Apps that include an app extension
and framework must support arm64.
```

Můžete přidat jenom aplikace Watch [sjednocené rozhraní API (64-bit)](~/cross-platform/macios/unified/index.md) aplikace Xamarin.iOS.
Klikněte pravým tlačítkem na projekt aplikace pro iOS a přejděte na **možnosti > sestavení > iOS Build > Upřesnit** a ujistěte se, že **podporované architektury** pro AppStore – zařízení iPhone konfigurace zahrnuje **ARM64** (např.) **ARMv7 + ARM64**).

### <a name="this-bundle-is-invalid"></a>Tato sada je neplatný.

```csharp
ITMS-90068: This bundle is invalid. The value provided for the key
MinimumOSVersion '8.3' is not acceptable.
```

Vaše aplikace pro iOS nadřazené musí mít MinimumOSVersion nastavena na hodnotu "8.2" nebo starší.

### <a name="non-public-api-usage"></a>Použití neveřejné rozhraní API

```csharp
Your app contains non-public API usage.
Please review the errors, and resubmit your application.
```

Ujistěte se, že používáte nejnovější verzi nástroje Xcode a rozhraní Xamarin.
Váš kód by neměl přístup k libovolné neveřejné rozhraní API.

### <a name="build-error-mt5309"></a>Chyba MT5309 sestavení

```csharp
Error MT5309: Native linking error: clang: error: no such file or directory:
```

Tato chyba je pravděpodobně výsledkem vaší s přejmenovat instalaci Xcode z **Xcode.app**. Například tato chyba nastane, Pokud přejmenujete svou instalaci na **XCode 6.2.app**.



## <a name="related-links"></a>Související odkazy

- [Průvodce odeslání WatchKit Apple](https://developer.apple.com/app-store/watch/)
