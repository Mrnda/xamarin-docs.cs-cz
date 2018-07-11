---
title: Nastavení sady Android SDK pro Xamarin.Android
description: Visual Studio obsahuje správce sady Android SDK, který používáte pro stažení nástroje sady Android SDK, platformy a další součásti, které potřebujete pro vývoj aplikací Xamarin.Android.
ms.prod: xamarin
ms.assetid: 9A857F52-2EC1-414F-8010-CEE67B60A4B4
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 07/10/2018
ms.openlocfilehash: 895496f6a198f679ce08322ae48fe88e03b85629
ms.sourcegitcommit: 632955f8cdb80712abd8dcc30e046cb9c435b922
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/10/2018
ms.locfileid: "37947267"
---
# <a name="setting-up-the-android-sdk-for-xamarinandroid"></a>Nastavení sady Android SDK pro Xamarin.Android

_Visual Studio obsahuje správce sady Android SDK, který používáte pro stažení nástroje sady Android SDK, platformy a další součásti, které potřebujete pro vývoj aplikací Xamarin.Android._


## <a name="overview"></a>Přehled

Tato příručka vysvětluje, jak pomocí Xamarin Android SDK Manageru v sadě Visual Studio a Visual Studio pro Mac.

> [!NOTE]
> Tento průvodce se týká jenom pro Visual Studio 2017 a Visual Studio pro Mac.  

Xamarin Android SDK Manageru (instalují jako součást **vývoj mobilních aplikací pomocí .NET** úloh) umožňuje stáhnout nejnovější Android komponenty, které potřebujete pro vývoj aplikací Xamarin.Android. Nahradí Googlu samostatný správce sady SDK, která se už nepoužívá.

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

## <a name="requirements"></a>Požadavky

Použití Správce Xamarin Android SDK, budete potřebovat následující:

- Visual Studio 2017 (Community, Professional nebo Enterprise edition). Vyžaduje se Visual Studio 2017 verze 15.7 nebo novější.

- Visual Studio Tools for Xamarin verze 4.10.0 nebo novější. 

Xamarin Android SDK Manageru není kompatibilní s Visual Studio
2015. Uživatelé sady Visual Studio 2015 by měl používat správce sady SDK nástroje poskytované společností Google v sadě Android SDK.


Xamarin Android SDK Manageru také vyžaduje Java Development Kit (která je automaticky nainstalována se Xamarin.androidem).
Xamarin.Android používá [JDK 8](http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html), což je vyžadováno, pokud jste vývoj pro úroveň rozhraní API 24 nebo větší (JDK 8 také podporuje úrovně rozhraní API starších než 24). Můžete dál používat [JDK 7](http://www.oracle.com/technetwork/java/javase/downloads/jdk7-downloads-1880260.html) vývoj speciálně pro rozhraní API úrovně 23 nebo dřívější.

> [!IMPORTANT]
> Xamarin.Android nepodporuje JDK 9.

 
## <a name="sdk-manager"></a>Správce sady SDK 

Chcete-li spustit správce sady SDK v sadě Visual Studio, klikněte na tlačítko **nástroje > Android > správce sady Android SDK**:

[![Umístění položky nabídky správce sady Android SDK](android-sdk-images/win/02-sdk-manager-menu-item-sml.png)](android-sdk-images/win/02-sdk-manager-menu-item.png#lightbox)

**Správce sady Xamarin Android SDK** se otevře v **sady Android SDK a nástroje** obrazovky. Tato obrazovka obsahuje dvě karty &ndash; **platformy** a **nástroje**:

[![Snímek obrazovky sady Android SDK Manager otevřete v kartě platformy](android-sdk-images/win/03-sdk-manager-platforms-sml.png)](android-sdk-images/win/03-sdk-manager-platforms.png#lightbox)

**Sady Android SDK a nástroje** obrazovky je popsáno podrobněji v následujících částech.


### <a name="android-sdk-location"></a>Umístění sady Android SDK

Umístění sady Android SDK je nakonfigurovaný v horní části **sady Android SDK a nástroje** obrazovky, jak je vidět na předchozím obrázku. Toto umístění musí být nakonfigurované správně před **platformy** a **nástroje** karty budou fungovat správně. Budete muset nastavit umístění sady Android SDK pro jeden nebo více z následujících důvodů:

1. Správce sady SDK Xamarin se nepovedlo najít sady Android SDK. 

2. Instalaci sady Android SDK do alternativního umístění (jiné než výchozí). 

Nastavit umístění sady Android SDK, klikněte &hellip; tlačítko na pravé straně **Android umístění sady SDK**. Tím se otevře **vyhledat složku** dialogové okno pro účely navigace k umístění sady Android SDK. Na následujícím snímku obrazovky sady Android SDK v rámci **Program Files (x86)\\Android** je výběru:

![Snímek obrazovky dialogového okna Windows vyhledat složku umístění sady android sdk](android-sdk-images/win/05-browse-for-folder.png)

Po kliknutí na **OK**, Xamarin Android SDK Manageru bude správa sady Android SDK, který je nainstalován do vybraného umístění.



### <a name="tools-tab"></a>Karta Nástroje

**Nástroje** karta zobrazuje seznam _nástroje_ a _funkce_. Tato karta slouží k instalaci nástroje sady Android SDK, nástroje pro platformu a vytváření buildů.
Navíc můžete nainstalovat emulátor Androidu nižší úrovně ladicího programu (LLDB), sada NDK, modul HAXM akcelerace a knihovny webu Google Play.


Například pokud chcete stáhnout emulátor Google Android balíčku, zaškrtněte políčko vedle položky **emulátoru Androidu** a klikněte na tlačítko **použít změny** tlačítka:

[![Instalace emulátoru Androidu na kartě nástrojů](android-sdk-images/win/06-install-emulator-sml.png)](android-sdk-images/win/06-install-emulator.png#lightbox)



Může zobrazit dialogové okno se zprávou, _některé komponenty je možné aktualizovat. Chcete je aktualizovat?_ Klikněte na tlačítko **Ano**. V dalším kroku se zobrazí dialogové okno přijetí licence:


![Obrazovka přijetí licence](android-sdk-images/win/07-license-acceptance.png)


Klikněte na tlačítko **přijmout** Pokud vyjadřujete souhlas s podmínkami a ujednáními. V dolní části okna indikátor průběhu označuje průběh stahování a instalaci. Po dokončení instalace **nástroje** kartě se zobrazí, zda byly nainstalovány vybrané nástroje a funkce.



### <a name="platforms-tab"></a>Kartu platformy

**Platformy** karta zobrazuje seznam verzí sady SDK platformy společně s další prostředky (jako jsou bitové kopie systému) pro každou platformu.


[![Snímek obrazovky podokna platformy](android-sdk-images/win/08-platforms-pane-sml.png)](android-sdk-images/win/08-platforms-pane.png#lightbox)


Tato obrazovka se zobrazí výpis verze Android (, jako **Android 7.0**), název kódu (**verzi Nougat**), úroveň rozhraní API (například **24**) a stav (**nainstalováno** Pokud je nainstalovaná platforma). Můžete použít **platformy** kartu k instalaci součásti pro rozhraní Android API úrovně, kterou chcete cílit na (Další informace o verzí Androidu a úrovně rozhraní API najdete v tématu [Principy Android API úrovně](~/android/app-fundamentals/android-api-levels.md)).

Pokud jsou nainstalovány všechny součásti platformy, zobrazí se značka zaškrtnutí vedle názvu platformy. Pokud jsou nainstalovány všechny součásti platformy, je vyplněno pole pro danou platformu. 


Můžete rozbalit platformu zobrazíte jeho součástí (a komponenty, které jsou nainstalované) kliknutím **+** políčko nalevo od platformy.
Klikněte na tlačítko **-** k unexpand komponentu výpis pro platformu.


Přidat jiné platformy v sadě SDK, klikněte na políčko vedle platformy do značky zaškrtnutí zobrazí k instalaci všech součástí, pak klikněte na **použít změny**:


[![Příklad přidání Android 7.1 verzi Nougat součásti sady Android SDK](android-sdk-images/win/09-adding-a-platform-sml.png)](android-sdk-images/win/09-adding-a-platform.png#lightbox)


Chcete-li nainstalovat pouze sadu SDK kliknutím na políčko vedle platformu jednou. Pak můžete vybrat všechny jednotlivé komponenty, které budete potřebovat:


[![Příklad přidání některé součásti Android 7.1](android-sdk-images/win/10-adding-some-components-sml.png)](android-sdk-images/win/10-adding-some-components.png#lightbox)




Všimněte si, že počet součástí k instalaci, zobrazí se vedle položky **použít změny** tlačítko. Ve výše uvedeném příkladu se šesti komponentami specifikovanými společností připraveno k instalaci. Po klepnutí **použít změny** tlačítko, zobrazí se **přijetí licence** obrazovky:



![Dialogové okno přijetí licence kartu platformy](android-sdk-images/win/11-license-screen.png)


Klikněte na tlačítko **přijmout** Pokud vyjadřujete souhlas s podmínkami a ujednáními. Může se zobrazit toto dialogové okno více než jednou při více součástí k instalaci. V dolní části okna označí indikátor průběhu stahování a instalace. Po dokončení procesu stahování a instalace (to může trvat řádech minut, v závislosti na tom, kolik komponent není třeba stahovat), přidání součásti jsou označené značka zaškrtnutí a uveden jako **nainstalováno**.



# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)


## <a name="requirements"></a>Požadavky

Použití Správce Xamarin Android SDK, budete potřebovat následující:

-   Visual Studio pro Mac 7.5 (nebo novější).

Xamarin Android SDK Manageru také vyžaduje Java Development Kit (která je automaticky nainstalována se Xamarin.androidem).
Xamarin.Android používá [JDK 8](http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html), což je vyžadováno, pokud jste vývoj pro úroveň rozhraní API 24 nebo větší (JDK 8 také podporuje úrovně rozhraní API starších než 24). Můžete dál používat [JDK 7](http://www.oracle.com/technetwork/java/javase/downloads/jdk7-downloads-1880260.html) vývoj speciálně pro rozhraní API úrovně 23 nebo dřívější.

> [!IMPORTANT]
> Xamarin.Android nepodporuje JDK 9.

 
## <a name="sdk-manager"></a>Správce sady SDK 

Chcete-li spustit správce sady SDK v sadě Visual Studio pro Mac, klikněte na tlačítko **nástroje > správce sady SDK**:
 
![Umístění položky nabídky správce sady Android SDK](android-sdk-images/mac/sdkmanager-01.png )

**Správce sady Android SDK** se otevře v **okno předvoleb**, která obsahuje tři karty **platformy**, **nástroje**a **Umístění**:

![Snímek obrazovky sady Android SDK Manager otevřete v kartě platformy](android-sdk-images/mac/sdkmanager-02.png)

Na kartách aplikace Xamarin Android SDK Manager jsou popsány v následujících částech.


### <a name="locations-tab"></a>Umístění karty

**Umístění** karta má tři nastavení pro konfiguraci umístění sady Android SDK, sady Android NDK a Java SDK (JDK). Tato umístění musí být nakonfigurované správně před **platformy** a **nástroje** karty budou fungovat správně.

Při spuštění správce sady SDK automaticky určí cestu pro každý nainstalovaný balíček a označuje, že se **nalezeno** umístěním zelená ikona zaškrtnutí vedle cesta:

![Snímek obrazovky s kartou umístění](android-sdk-images/mac/sdkmanager-03.png)

Klikněte na tlačítko **resetovat na výchozí hodnoty** tlačítko a způsobit, že správce sady SDK k vyhledání sady SDK, sada NDK a JDK na jejich výchozích umístění. 

Obvykle se používají **umístění** kartu Upravit umístění sady Android SDK a/nebo Java JDK. Není potřeba instalovat NDK pro vývoj aplikací Xamarin.Android &ndash; NDK se používá jenom v případě, že potřebujete k vývoji součástí vaší aplikace pomocí jazyků nativního kódu, jako je C a C++.

### <a name="tools-tab"></a>Karta Nástroje

**Nástroje** karta zobrazuje seznam _nástroje_ a _funkce_. Tato karta slouží k instalaci nástroje sady Android SDK, nástroje pro platformu a vytváření buildů.
Navíc můžete nainstalovat emulátor Androidu nižší úrovně ladicího programu (LLDB), sada NDK, modul HAXM akcelerace a knihovny webu Google Play.


Například pokud chcete stáhnout emulátor Google Android balíčku, zaškrtněte políčko vedle položky **emulátoru Androidu** a klikněte na tlačítko **instalovat aktualizace** tlačítka:

![Instalace emulátoru Androidu na kartě nástrojů](android-sdk-images/mac/sdkmanager-08.png)


Může zobrazit dialogové okno se zprávou, _některé komponenty je možné aktualizovat. Chcete je aktualizovat?_ Klikněte na tlačítko **Ano**. V dalším kroku se zobrazí dialogové okno přijetí licence:


![Obrazovka přijetí licence](android-sdk-images/mac/sdkmanager-09.png)


Klikněte na tlačítko **přijmout** Pokud vyjadřujete souhlas s podmínkami a ujednáními. V dolní části okna indikátor průběhu označuje průběh stahování a instalaci. Po dokončení instalace **nástroje** kartě se zobrazí, zda byly nainstalovány vybrané nástroje a funkce.



### <a name="platforms-tab"></a>Kartu platformy

**Platformy** karta zobrazuje seznam verzí sady SDK platformy společně s další prostředky (jako jsou bitové kopie systému) pro každou platformu.


![Snímek obrazovky podokna platformy](android-sdk-images/mac/sdkmanager-11.png)


Tato obrazovka se zobrazí výpis verze Android (, jako **Android 7.0**), název kódu (**verzi Nougat**), úroveň rozhraní API (například **24**) a stav (**nainstalováno** Pokud je nainstalovaná platforma). Můžete použít **platformy** kartu k instalaci součásti pro rozhraní Android API úrovně, kterou chcete cílit na (Další informace o verzí Androidu a úrovně rozhraní API najdete v tématu [Principy Android API úrovně](~/android/app-fundamentals/android-api-levels.md)).

Pokud jsou nainstalovány všechny součásti platformy, zobrazí se značka zaškrtnutí vedle názvu platformy. Pokud jsou nainstalovány všechny součásti platformy, je vyplněno pole pro danou platformu. 


Můžete rozbalit platformu zobrazíte jeho součástí (a komponenty, které jsou nainstalované) kliknutím **šipku** nalevo od platformy.
Klikněte na tlačítko **šipka dolů** k unexpand komponentu výpis pro platformu.


Přidat jiné platformy v sadě SDK, klikněte na políčko vedle platformy do značky zaškrtnutí zobrazí k instalaci všech součástí, pak klikněte na **použít změny**:


![Příklad přidání Android 4.4 součásti sady Android SDK](android-sdk-images/mac/sdkmanager-12.png)


Chcete-li nainstalovat pouze sadu SDK kliknutím na políčko vedle platformu jednou. Pak můžete vybrat všechny jednotlivé komponenty, které budete potřebovat:


![Příklad přidání některé komponenty s Androidem 4.4](android-sdk-images/mac/sdkmanager-13.png)




Všimněte si, že počet součástí k instalaci, zobrazí se vedle položky **použít změny** tlačítko. Po klepnutí **použít změny** tlačítko, zobrazí se **přijetí licence** obrazovky:



![Dialogové okno přijetí licence kartu platformy](android-sdk-images/mac/sdkmanager-14.png)


Klikněte na tlačítko **přijmout** Pokud vyjadřujete souhlas s podmínkami a ujednáními. Může se zobrazit toto dialogové okno více než jednou při více součástí k instalaci. V dolní části okna označí indikátor průběhu stahování a instalace. Po dokončení procesu stahování a instalace (to může trvat řádech minut, v závislosti na tom, kolik komponent není třeba stahovat), přidání součásti jsou označené značka zaškrtnutí a uveden jako **nainstalováno**.

-----

 
## <a name="summary"></a>Souhrn

Tato příručka bylo vysvětleno, jak nainstalovat a používat nástroj Správce sady Xamarin Android SDK v sadě Visual Studio a Visual Studio pro Mac.


## <a name="related-links"></a>Související odkazy

- [Změny nástrojů sady Android SDK](~/android/troubleshooting/sdk-cli-tooling-changes.md)
- [Principy úrovní rozhraní API systému Android](~/android/app-fundamentals/android-api-levels.md)
- [sdkmanager](https://developer.android.com/studio/command-line/sdkmanager.html)
- [avdmanager](https://developer.android.com/studio/command-line/avdmanager.html)
