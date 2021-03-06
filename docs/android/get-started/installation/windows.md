---
title: Instalace systému Windows
description: Tento průvodce popisuje kroky pro instalaci Xamarin.Android pro sadu Visual Studio v systému Windows, a vysvětluje postup konfigurace Xamarin.Android pro vytvoření vaší první aplikace Xamarin.Android.
ms.prod: xamarin
ms.assetid: 2BE4D5AD-D468-B177-8F96-837D084E7DE1
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 06/22/2018
ms.openlocfilehash: 1eb8d4ec9ad60f0f9e81676920df4d950a875088
ms.sourcegitcommit: 3f2737f8abf9b855edf060474aa222e973abda3f
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 06/28/2018
ms.locfileid: "37066439"
---
# <a name="windows-installation"></a>Instalace systému Windows

_Tento průvodce popisuje kroky pro instalaci Xamarin.Android pro sadu Visual Studio v systému Windows, a vysvětluje postup konfigurace Xamarin.Android pro vytvoření vaší první aplikace Xamarin.Android._


## <a name="overview"></a>Přehled

Vzhledem k tomu, že je nyní součástí Xamarin všechny edice sady Visual Studio bez jakýchkoli nákladů a nevyžaduje samostatné licence, instalační program sady Visual Studio můžete použít ke stažení a instalaci nástrojů pro Xamarin.Android.
(Ruční instalace a licencování kroky, které musely u starších verzí systému Xamarin.Android již nejsou potřebné.) V tomto průvodci se dozvíte toto:

-   Jak nakonfigurovat vlastní umístění Java Development Kit, Android SDK a Android NDK.

-   Postup spuštění Android SDK Manager stahování a instalaci dalších součástí sady SDK pro Android.

-   Postup přípravy emulátoru nebo zařízení s Androidem pro ladění a testování.

-   Postup vytvoření prvního projektu aplikace Xamarin.Android.

Na konci tohoto průvodce, budete mít funkční Xamarin.Android instalace integrovaná do sady Visual Studio, a pak budete připraveni začít vytvářet vaší první aplikace Xamarin.Android.

## <a name="installation"></a>Instalace

Podrobné informace o instalaci Xamarin pro použití s Visual Studio v systému Windows najdete v tématu [nainstalovat Windows](~/cross-platform/get-started/installation/windows.md) průvodce.


## <a name="configuration"></a>Konfigurace

Xamarin.Android používá k vytvoření aplikace Java Development Kit (JDK) a sady SDK pro Android. Během instalace instalační program sady Visual Studio umístí do výchozího umístění těchto nástrojů a nakonfiguruje vývojového prostředí s konfigurací správnou cestu. Můžete zobrazit a změnit kliknutím na těchto umístění **nástroje > Možnosti > Xamarin > Nastavení Androidu**:

![Obrazovka o aplikaci Xamarin Android dialogové okno nastavení](windows-images/07-settings.png)

Pro většinu uživatelů tyto výchozí umístění bude fungovat bez další změny. Můžete však sady Visual Studio nakonfigurovat vlastní umístění pro tyto nástroje (například pokud jste nainstalovali Java JDK, sady SDK pro Android nebo NDK v jiném umístění). Klikněte na tlačítko **změnit** vedle cestu, která chcete změnit, pak přejděte do nového umístění.

Používá Xamarin.Android [JDK 8](http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html), což je vyžadováno, pokud jste pro úroveň rozhraní API 24 vývoj nebo větší (JDK 8 také podporuje úrovně rozhraní API starší než 24). Můžete dál používat [JDK 7](http://www.oracle.com/technetwork/java/javase/downloads/jdk7-downloads-1880260.html) Pokud vývoj speciálně pro úroveň rozhraní API 23 nebo starším.

> [!IMPORTANT]
> Xamarin.Android nepodporuje JDK 9.


### <a name="android-sdk-manager"></a>Správce sady Android SDK

Android používá více nastavení úrovně rozhraní API systému Android určete kompatibilitu aplikace v různých verzích Android (Další informace o úrovních rozhraní API systému Android, najdete v části [Principy Android API úrovně](~/android/app-fundamentals/android-api-levels.md)).
V závislosti na tom, jaké rozhraní API systému Android (úrovní) chcete zacílit musíte stáhnout a nainstalovat další součásti sady SDK pro Android. Kromě toho musíte k instalaci volitelných nástrojů a bitové kopie emulátoru součástí sady SDK pro Android. Chcete-li to provést, použijte **Android SDK Manager**. Můžete spustit **Android SDK Manager** kliknutím **nástroje > Android > Android SDK Manager**:

[![Postup spuštění Android SDK Manager](windows-images/08-sdk-manager-sml.png)](windows-images/08-sdk-manager.png#lightbox)

Ve výchozím nastavení nainstaluje Visual Studio Google Android SDK Manager:

![Snímek obrazovky příklad z Google Android SDK Manager](windows-images/09-google-sdk-manager.png)

Google Android SDK Manager můžete použít k instalaci verze balíčku nástroje pro Android SDK verzi 25.2.3. Ale pokud budete muset použít novější verzi balíčku nástroje pro Android SDK, je nutné nainstalovat modul plug-in Xamarin Android SDK Manager pro Visual Studio (k dispozici v aplikaci Visual Studio Marketplace). To je nezbytné, protože Google samostatné sady SDK Manager byla zrušena v 25.2.3 verzi balíčku, nástroje pro Android SDK. 

Další informace o používání nástroje Xamarin Android SDK Manager najdete v tématu [Android SDK instalace](~/android/get-started/installation/android-sdk.md).

### <a name="android-emulator"></a>Emulátoru systému Android

[Emulátoru Android](https://developer.android.com/studio/run/emulator) může být užitečné nástroje pro vývoj a testování aplikace Xamarin.Android. Například fyzické zařízení, jako je například tablet nemusí být snadno dostupné během vývoje nebo vývojář chtít spustit některé testy integrace ve svém počítači před potvrzením kódu.

Emulace zařízení v počítači se systémem Android zahrnuje následující součásti:

* **Emulátor Google Android** &ndash; Toto je emulátor na základě [QEMU](https://www.qemu.org/) vytvářející virtualizované zařízení se systémem na pracovní stanici pro vývojáře.
* **Obrázek emulátoru** &ndash; _bitová kopie emulátoru_ šablonu nebo je specifikace hardwaru a operačního systému, který je určen jako Virtualizovat. Například jedna bitová kopie emulátoru by identifikovat požadavky na hardware pro Nexus 5 X systémem Android 7.0 s Google Play Services nainstalována. Jiné bitová kopie emulátoru může konkrétní tabulky 10" systémem Android 6.0.
* **Virtuální zařízení Android (AVD)** &ndash; _virtuální zařízení se systémem Android_ je emulovaných zařízení se systémem Android vytvořit z image emulátor. Při spuštění a testování aplikací pro Android, bude Xamarin.Android spustit Android emulátor, spouštění konkrétní AVD, nainstalujte APK a spusťte aplikaci.

Přináší značné vylepšení výkonu při vývoji na x86 na počítačích můžete dosáhnout pomocí speciální emulátoru bitové kopie, které jsou optimalizované pro x86 architektura a mezi dvěma virtualizačních technologií:

1. Společnosti Microsoft Hyper-V &ndash; k dispozici v počítačích se systémem Windows 10. dubna Update.
2. Společnosti Intel hardwaru Accelerated spuštění správce (HAXM) &ndash; k dispozici na x86 počítače se systémem OS X, systému macOS nebo starší verzí systému Windows.

Další informace o emulátoru systému Android, Hyper-V a HAXM, najdete v tématu [hardwarovou akceleraci emulátoru výkonu](~/android/get-started/installation/android-emulator/hardware-acceleration.md) průvodce.

> [!NOTE]
> Ve starších verzích systému Windows HAXM není kompatibilní s technologií Hyper-V. V tomto scénáři je potřeba buď [zakázat technologie Hyper-V](~/android/get-started/installation/android-emulator/troubleshooting.md#disable-hyperv) nebo používat pomalejší emulátoru bitové kopie, které nemají x86 optimalizace.


<a name="device" />

### <a name="android-device"></a>Zařízení se systémem Android

Pokud máte fyzické zařízení Android má použít pro testování, to je vhodná doba na nastavit pro použití vývoj. V tématu [nastavit zařízení pro vývoj](~/android/get-started/installation/set-up-device-for-development.md) konfigurace zařízení s Androidem pro vývoj, připojte jej k počítači pro spouštění a ladění aplikací Xamarin.Android.


## <a name="create-an-application"></a>Vytvoření aplikace

Teď, když jste nainstalovali Xamarin.Android, můžete spustit Visual Studio vytvořte nový projekt. Klikněte na tlačítko **soubor > Nový > projekt** zahajte proces vytváření aplikace:

![Postup vytvoření nového projektu](windows-images/10-new-project.png)

V **nový projekt** dialogovém okně, vyberte **Android** pod **šablony** a klikněte na tlačítko **aplikace pro Android** v pravém podokně. Zadejte název pro vaši aplikaci (na tomto snímku obrazovky je aplikace volána **Moje aplikace**), pak klikněte na tlačítko **OK**:

[![Dialogové okno snímek obrazovky nový projekt, vytvořit prázdnou aplikaci pro Android](windows-images/11-first-app-sml.w157.png)](windows-images/11-first-app.w157.png#lightbox)

Je to! Nyní jste připraveni vytvořit aplikace pro Android pomocí Xamarin.Android!


## <a name="summary"></a>Souhrn

V tomto článku jste zjistili, jak nastavit a nainstalovat platformě Xamarin.Android v systému Windows, jak Visual Studio (volitelně) nakonfigurovat vlastní Java JDK a sady SDK pro Android umístění instalace, jak spustit SDK Manager k instalaci dalších sady SDK pro Android součásti, jak nastavit emulátoru nebo zařízení se systémem Android a k vytvoření vaší první aplikace.

Dalším krokem je Podíváme se na [Hello, Android](~/android/get-started/hello-android/index.md) kurzy se dozvíte, jak vytvořit funkční aplikaci pro Xamarin.Android.


## <a name="related-links"></a>Související odkazy

- [Stáhněte si Visual Studio](https://visualstudio.microsoft.com/vs/)
- [Instalace nástrojů Visual Studio Tools pro Xamarin](~/cross-platform/get-started/installation/windows.md)
- [Požadavky na systém](~/cross-platform/get-started/requirements.md)
- [Instalace sady Android SDK](~/android/get-started/installation/android-sdk.md)
- [Instalace emulátoru Androidu](~/android/get-started/installation/android-emulator/index.md)
- [Nastavit zařízení pro vývoj](~/android/get-started/installation/set-up-device-for-development.md)
- [Spuštění aplikace v emulátoru systému Android](https://developer.android.com/studio/run/emulator#Requirements)
