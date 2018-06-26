---
title: Nastavení služby SDK pro Android pro Xamarin.Android
description: Visual Studio obsahuje Android SDK Manager, který nahrazuje Google samostatné sady SDK Manager. Tato příručka vysvětluje, jak používat ke stahování nástroje, platformy a další součásti, které potřebujete pro vývoj aplikací Xamarin.Android sady SDK pro Android SDK Manager.
ms.prod: xamarin
ms.assetid: 9A857F52-2EC1-414F-8010-CEE67B60A4B4
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 06/22/2018
ms.openlocfilehash: 6a3f3f79e81339cc903d85081ca173a7ac707f6a
ms.sourcegitcommit: 26033c087f49873243751deded8037d2da701655
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 06/25/2018
ms.locfileid: "36935474"
---
# <a name="setting-up-the-android-sdk-for-xamarinandroid"></a>Nastavení služby SDK pro Android pro Xamarin.Android

_Visual Studio obsahuje Android SDK Manager, který nahrazuje Google samostatné sady SDK Manager. Tato příručka vysvětluje, jak používat ke stahování nástroje, platformy a další součásti, které potřebujete pro vývoj aplikací Xamarin.Android sady SDK pro Android SDK Manager._


## <a name="overview"></a>Přehled

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Tato příručka vysvětluje, jak nainstalovat a používat Xamarin Android SDK Manager pro sadu Visual Studio v systému Windows (nebo [pro Mac](?tabs=vsmac)).

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

Tato příručka vysvětluje, jak nainstalovat a používat Xamarin Android SDK Manager pro sadu Visual Studio pro Mac (nebo [pro systém Windows](?tabs=vswin)).

> [!NOTE]
> Tento průvodce se týká pouze pro Visual Studio 2017 a Visual Studio for Mac.  

-----

Xamarin Android SDK Manager vám pomůže stáhnout nejnovější Android součásti, které potřebujete pro vývoj aplikací Xamarin.Android.
Nahradí Google samostatný správce sady SDK, která se už nepoužívá.

Proč by chcete použít místo správce SDK, který je součástí sady Android SDK Xamarin Android SDK Manager? Ve verzi 25.2.3 balíček nástroje pro Android SDK Google zavedl nový nástroj pro údržbu SDK pro Android. Tento nový nástroj  **[sdkmanager](https://developer.android.com/studio/command-line/sdkmanager.html)**, je nástroj příkazového řádku, který nahrazuje správce samostatné uživatelského rozhraní pro Android SDK. Proto pokud aktualizujete na verzi sady SDK nástroje 26.0.1 (povinné pro Android 8.0) nebo novější a chcete pokračovat ke správě Android SDK prostřednictvím uživatelského rozhraní, musíte použít Xamarin Android SDK Manager.

## <a name="requirements"></a>Požadavky

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Pokud chcete používat Xamarin Android SDK Manager, budete potřebovat následující:

- Visual Studio 2017 (Community, Professional nebo Enterprise edition). Visual Studio 2017 verze 15,5 nebo novější je povinný.

- Nástroje sady Visual Studio pro Xamarin verze 4.5.0 nebo novější. 

Xamarin Android SDK Manager není kompatibilní s Visual Studio
2015. Uživatelé sady Visual Studio 2015 by měl používat SDK Manager nástroje poskytované subsystémem pro Google Android SDK.

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

-   Visual Studio pro Mac 7.0.0.3146 (nebo novější).

-----

Xamarin Android SDK Manager taky vyžaduje Java Development Kit (která je automaticky nainstalována s Xamarin.Android).
Používá Xamarin.Android [JDK 8](http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html), což je vyžadováno, pokud jste pro úroveň rozhraní API 24 vývoj nebo větší (JDK 8 také podporuje úrovně rozhraní API starší než 24). Můžete dál používat [JDK 7](http://www.oracle.com/technetwork/java/javase/downloads/jdk7-downloads-1880260.html) Pokud vývoj speciálně pro úroveň rozhraní API 23 nebo starším.

> [!IMPORTANT]
> Xamarin.Android nepodporuje JDK 9.


# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

## <a name="installation"></a>Instalace

Xamarin SDK Manager lze přidat na Visual Studio 2017 v průběhu instalace. Při instalaci sady Visual Studio, klikněte na tlačítko **jednotlivých součástí** a přejděte dolů k položce **vývoj aktivity** části.
Povolit **Xamarin SDK Manager** Pokud již není označena:

![Povolení Xamarin SDK Manager z jednotlivých součástí](android-sdk-images/win/01-sdk-manager-install.png)

Pokud jste již nainstalovali Visual Studio 2017, přečtěte si téma [upravit Visual Studio 2017](https://docs.microsoft.com/en-us/visualstudio/install/modify-visual-studio) pro pokyny o tom, jak upravit sadu Visual Studio, postupujte podle výše uvedený postup pro povolení Xamarin SDK Manager. Pokud se zobrazí výzva k aktualizaci SDK Manager, můžete tento stejný postup instalace nástroje Xamarin SDK Manager.

Když kliknete na tlačítko **nástroje > Android > Android SDK Manager** (jak je vysvětleno vedle), bude Xamarin Android SDK Manager spuštěna místo Google Android SDK Manager. Pokud používáte starší verzi SDK pro Android, která podporuje samostatné Google Android SDK Manager, instalace nástroje Xamarin Android SDK Manager nevytvoří konflikt &ndash; stále můžete spustit samostatný Google SDK Manager z mimo Visual Studio pro správu sady SDK pro Android.

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)
 
-----

 
## <a name="sdk-manager"></a>SDK Manager 

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Chcete-li spustit Správce SDK v sadě Visual Studio, klikněte na tlačítko **nástroje > Android > Android SDK Manager**:

[![Umístění položky nabídky Android SDK Manager](android-sdk-images/win/02-sdk-manager-menu-item-sml.png)](android-sdk-images/win/02-sdk-manager-menu-item.png#lightbox)

**Xamarin Android SDK Manager** se otevře v **sady Android SDK a nástroje** obrazovky. Tato obrazovka má dvě karty &ndash; **platformy** a **nástroje**:

[![Snímek obrazovky Android SDK Manager otevřete na kartě platformy](android-sdk-images/win/03-sdk-manager-platforms-sml.png)](android-sdk-images/win/03-sdk-manager-platforms.png#lightbox)

**Sady Android SDK a nástroje** obrazovky je podrobně popsaná v další v následujících částech.

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

Chcete-li spustit Správce SDK v sadě Visual Studio pro Mac, klikněte na tlačítko **nástroje > SDK Manager**:
 
![Umístění položky nabídky Android SDK Manager](android-sdk-images/mac/sdkmanager-01.png )

**Android SDK Manager** se otevře v **okno Předvolby**, který obsahuje tři karty **platformy**, **nástroje**a **Umístění**:

![Snímek obrazovky Android SDK Manager otevřete na kartě platformy](android-sdk-images/mac/sdkmanager-02.png)

Karty nástroje Xamarin Android SDK Manager jsou popsány v následujících částech.

-----



# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

### <a name="android-sdk-location"></a>Umístění sady SDK pro Android

Umístění sady SDK pro Android je nakonfigurováno v horní části **sady Android SDK a nástroje** obrazovky, jak je vidět na předchozím obrázku. Toto umístění musí být nakonfigurováno správně před **platformy** a **nástroje** karty budou fungovat správně. Musíte nastavit umístění sady SDK pro Android pro jeden nebo více z následujících důvodů:

1. Xamarin SDK Manager se nepodařilo najít SDK pro Android. 

2. Instalaci sady SDK pro Android v alternativního umístění (jiné než výchozí). 

Chcete-li nastavit umístění sady SDK pro Android, klikněte na tlačítko &hellip; tlačítko nejvíce vpravo **Android SDK umístění**. Tím se otevře **vyhledat složku** dialogovém okně můžete použít pro navigace k umístění sady SDK pro Android. Na následujícím snímku obrazovky SDK pro Android v části **Program Files (x86)\\Android** je vybrat:

![Snímek obrazovky dialogového okna Windows najít složku vyhledání sady sdk pro android](android-sdk-images/win/05-browse-for-folder.png)

Když kliknete na tlačítko **OK**, Xamarin Android SDK Manager bude spravovat Android SDK, který je nainstalován do vybraného umístění.

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

### <a name="locations-tab"></a>Karta umístění

**Umístění** karta má tři nastavení pro konfiguraci umístění sady SDK pro Android, Android NDK a Java SDK (JDK). Tato umístění musí být nakonfigurované správně před **platformy** a **nástroje** karty budou fungovat správně.

Při spuštění Správce SDK se automaticky určuje cestu pro každý nainstalovaný balíček a znamená, že byla **nalezené** tím, že umístíte zelená ikona zaškrtnutí vedle cesta:

![Snímek obrazovky s kartou umístění](android-sdk-images/mac/sdkmanager-03.png)

Klikněte **obnovit výchozí hodnoty** tlačítko způsobit SDK Manager hledání SDK, NDK a JDK na jejich výchozí umístění. 

Obvykle se používá **umístění** a změňte umístění sady Android SDK a sadu JDK Java. Není potřeba instalovat NDK vyvíjet aplikace Xamarin.Android &ndash; na NDK se používá pouze v případě potřeby k vývoji částí aplikace pomocí nativního kódu jazyků, například C a C++.

-----


### <a name="tools-tab"></a>Na kartě nástroje

**Nástroje** karta zobrazuje seznam _nástroje_ a _funkce_. Tato karta slouží k instalaci nástroje pro Android SDK, nástrojů platformy a nástroje pro vytváření.
Navíc můžete nainstalovat Android emulátoru, nízké úrovně ladicího programu (LLDB), na NDK HAXM akcelerace a knihovny webu Google Play.

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Například ke stažení balíčku emulátor Google Android, zaškrtněte políčko vedle **emulátoru Android** a klikněte na tlačítko **použít změny** tlačítko:

[![Instalace Android emulátor na kartě nástrojů](android-sdk-images/win/06-install-emulator-sml.png)](android-sdk-images/win/06-install-emulator.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

Například ke stažení balíčku emulátor Google Android, zaškrtněte políčko vedle **emulátoru Android** a klikněte na tlačítko **instalovat aktualizace** tlačítko:

![Instalace Android emulátor na kartě nástrojů](android-sdk-images/mac/sdkmanager-08.png)

-----


Může zobrazit dialogové okno se zprávou, _lze aktualizovat některé součásti. Chcete teď aktualizovat?_ Klikněte na tlačítko **Ano**. V dalším kroku se zobrazí dialogové okno přijetí licence:

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

![Licence přijetí obrazovky](android-sdk-images/win/07-license-acceptance.png)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

![Licence přijetí obrazovky](android-sdk-images/mac/sdkmanager-09.png)

-----

Klikněte na tlačítko **přijmout** Pokud souhlasíte s podmínkami a ujednáními. V dolní části okna označuje indikátor průběhu stahování a instalace probíhá. Po dokončení instalace **nástroje** karta se zobrazí, zda byly nainstalovány vybrané nástroje a funkce.



### <a name="platforms-tab"></a>Karta platformy

**Platformy** karta zobrazuje seznam verzí sady SDK platformy spolu s další zdroje informací (jako jsou bitové kopie systému) pro každou platformu.

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![Snímek obrazovky podokna platformy](android-sdk-images/win/08-platforms-pane-sml.png)](android-sdk-images/win/08-platforms-pane.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

![Snímek obrazovky podokna platformy](android-sdk-images/mac/sdkmanager-11.png)

-----

Tato obrazovka uvádí verzi systému Android (například **Android 7.0**), kódový název (**cukrovinkách typu nugát**), úroveň rozhraní API (například **24**) a stav (**nainstalovaná** Pokud je nainstalován platformě). Můžete použít **platformy** kartě k instalaci součásti pro úroveň rozhraní API systému Android, kterou chcete zacílit (Další informace o verzích Android a úrovně rozhraní API najdete v tématu [Principy Android API úrovně](~/android/app-fundamentals/android-api-levels.md)).

Pokud jsou nainstalovány všechny součásti platformy, zobrazí se vedle názvu platformy zaškrtnout. Pokud jsou nainstalovány všechny součásti platformy, se naplní pole pro platformu. 

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Můžete rozbalit platformy zobrazíte jeho součásti (a které součásti jsou nainstalovány) kliknutím **+** políčko nalevo od platformu.
Klikněte na tlačítko **-** k unexpand komponentu výpis pro platformu.

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

Můžete rozbalit platformy zobrazíte jeho součásti (a které součásti jsou nainstalovány) kliknutím **šipku** nalevo od platformu.
Klikněte na tlačítko **šipka dolů** k unexpand komponentu výpis pro platformu.

-----

Pokud chcete přidat jiné platformě k sadě SDK, klikněte na políčko vedle platformou až na značku zaškrtnutí se zobrazí k instalaci všech součástí, pak klikněte **použít změny**:

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![Příklad přidávání Android cukrovinkách typu nugát 7.1 součástí do sady SDK pro Android](android-sdk-images/win/09-adding-a-platform-sml.png)](android-sdk-images/win/09-adding-a-platform.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

![Příklad přidávání Android 4.4 součástí do sady SDK pro Android](android-sdk-images/mac/sdkmanager-12.png)

-----

K instalaci pouze sadu SDK jedním klepnutím políčko vedle platformu. Pak můžete vybrat všechny jednotlivé komponenty, které budete potřebovat:

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![Příklad přidání některé součásti Android 7.1](android-sdk-images/win/10-adding-some-components-sml.png)](android-sdk-images/win/10-adding-some-components.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

![Příklad přidání některé součásti Android 4.4](android-sdk-images/mac/sdkmanager-13.png)

-----


# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Všimněte si, že počet součástí k instalaci, zobrazí se vedle **použít změny** tlačítko. V předchozím příkladu jsou připravené k instalaci šest součástí. Po kliknutí **použít změny** tlačítko, zobrazí se **přijetí licence** obrazovky:

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

Všimněte si, že počet součástí k instalaci, zobrazí se vedle **použít změny** tlačítko. Po kliknutí **použít změny** tlačítko, zobrazí se **přijetí licence** obrazovky:

-----

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

![Dialogové okno přijetí licence karta platformy](android-sdk-images/win/11-license-screen.png)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

![Dialogové okno přijetí licence karta platformy](android-sdk-images/mac/sdkmanager-14.png)

-----

Klikněte na tlačítko **přijmout** Pokud souhlasíte s podmínkami a ujednáními. Toto dialogové okno může zobrazí více než jednou po několika součástí k instalaci. V dolní části okna označí indikátor průběhu stahování a instalace probíhá. Po dokončení procesu stahování a instalace (může to trvat velký počet minut, v závislosti na tom, kolik součásti nutné stáhnout), přidané součásti jsou označené jako zaškrtnout a uveden jako **nainstalovaná**.

Teď můžete začít vyvíjet aplikace pro nejnovější, nejvyšší úroveň rozhraní API systému Android.


 
## <a name="summary"></a>Souhrn

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Tato příručka vysvětluje postup instalace a použití nástroje Xamarin Android SDK Manager v sadě Visual Studio.

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

Tato příručka vysvětlené použití nástroje Xamarin Android SDK Manager v sadě Visual Studio for Mac.

-----


## <a name="related-links"></a>Související odkazy

- [Změny nástrojů sady Android SDK](~/android/troubleshooting/sdk-cli-tooling-changes.md)
- [Principy úrovní rozhraní API systému Android](~/android/app-fundamentals/android-api-levels.md)
- [sdkmanager](https://developer.android.com/studio/command-line/sdkmanager.html)
- [avdmanager](https://developer.android.com/studio/command-line/avdmanager.html)
