---
title: Android emulátor sady Visual Studio
description: Tato příručka vysvětluje, jak konfigurovat a používat Android emulátor sady Visual Studio pro vývoj aplikací Xamarin.Android ve Visual Studiu 2015.
ms.prod: xamarin
ms.assetid: CD128CB9-499F-4558-B49F-77248824EFDF
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 04/30/2018
ms.openlocfilehash: 29e35d0dee614d28eed08fbe8799fc74c5ad1eba
ms.sourcegitcommit: 4b0582a0f06598f3ff8ad5b817946459fed3c42a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 05/03/2018
---
# <a name="visual-studio-android-emulator"></a>Android emulátor sady Visual Studio

_Tato příručka vysvětluje, jak konfigurovat a používat Android emulátor sady Visual Studio pro vývoj aplikací Xamarin.Android ve Visual Studiu 2015._

## <a name="visual-studio-android-emulator-overview"></a>Přehled Android emulátor sady Visual Studio

Microsoft Visual Studio 2015 zahrnuje Android emulátoru, který lze použít jako cíl pro ladění aplikace Xamarin.Android: *Visual Studio Emulator for Android*. Tento emulátor používá technologie Hyper-V možnostech vývojovém počítači, výsledkem je rychlejší spuštění a časy spuštění než výchozí emulátor, která se dodává s SDK pro Android. Emulátor sady Visual Studio pro Android můžete použít jako alternativu k výchozí emulátor sady SDK pro Android, při vývoji aplikace pro Xamarin.Android.

> [!NOTE]
> Android emulátor sady Visual Studio jsou kompatibilní jenom s Visual Studio 2015 &ndash; nefunguje s Visual Studio 2017.

Tato příručka vysvětluje, jak spustit v emulátoru Microsoft Android ze sady Visual Studio k testování aplikace, a popisuje různé funkce, které jsou k dispozici v emulátoru. Se dozvíte, jak vybrat *profily zařízení* (podobně jako definice zařízení v emulátoru výchozí sady SDK pro Android) k simulaci různé typy zařízení se systémem Android. Nakonec část s řešením potíží vysvětluje běžné nástrahy a alternativní řešení.

## <a name="requirements"></a>Požadavky

Pokud chcete spustit v emulátoru, počítač musí splňovat požadavky na spuštění technologie Hyper-V. Technologie Hyper-V vyžaduje 64bitový verzi Pro verzi systému Windows 8, Windows 8.1, Windows 10 nebo vyšší. Další informace o požadavcích najdete v tématu [systémové požadavky pro emulátor sady Visual Studio pro Android](https://msdn.microsoft.com/en-us/library/mt228280.aspx).

> [!NOTE]
> Nelze použít HAXM (používané v emulátoru Android SDK) během je povolená technologie Hyper-V. Další informace o omezení a potenciální problémy s HAXM najdete v tématu [HAXM virtualizace konflikty](~/android/deploy-test/debugging/android-sdk-emulator/troubleshooting.md#virt-conflicts).


## <a name="running-the-emulator"></a>Spuštěný v emulátoru

Visual Studio zpřístupňuje několik předem nakonfigurovaná cílové zařízení profily v **cíl ladění** rozevírací nabídky (jako je zobrazené na následujícím snímku obrazovky). Cíle emulátoru Android Microsoft začínají **VS emulátoru**:

[![Profily předem nakonfigurovaná cílového zařízení](visual-studio-android-emulator-images/01-vs-emulator-defaults-vs-sml.png)](visual-studio-android-emulator-images/01-vs-emulator-defaults-vs.png#lightbox)

Když Visual Studio spustí aplikace pro Xamarin.Android, emulátoru bude spuštěna s cílem vybrané zařízení a aplikace je nasazená na emulátoru. Zobrazí se v levém dolním rohu nástroje Visual Studio, která určuje, se spouští v emulátoru:

[![Výchozí emulátor VS](visual-studio-android-emulator-images/02-emulator-starting-vs-sml.png)](visual-studio-android-emulator-images/02-emulator-starting-vs.png#lightbox)

Po spuštění prodlevě zobrazí se obrazovka emulátoru jak je znázorněno na levé straně níže. Přetáhněte ikonu zámku na obrazovce směrem nahoru k odemknutí zařízení.
Aplikace Xamarin.Android by měl být spuštěn v emulátoru pak, jak je znázorněno na pravé straně:

[![Snímky obrazovky emulátoru](visual-studio-android-emulator-images/03-first-screen-vs-sml.png)](visual-studio-android-emulator-images/03-first-screen-vs.png#lightbox)

Stejně jako u výchozí emulátor sady SDK pro Android, je možné nastavit zarážky v kódu, zkontrolujte proměnné a zobrazit zásobníku volání. Svislé nástrojů napravo od emulátoru poskytuje přístup k emulátoru funkce:

[![Tlačítka na panelu nástrojů svislé](visual-studio-android-emulator-images/04-vertical-toolbar-vs-sml.png)](visual-studio-android-emulator-images/04-vertical-toolbar-vs.png#lightbox)

Následující seznam shrnuje funkce každý tlačítka na panelu nástrojů svislé:

-   **Zavřít** &ndash; ukončí aplikaci emulátor. Toto tlačítko nepoužívá často &ndash; obvykle emulátor je ponechat spuštěnou po prvním spuštění (Chcete-li zamezit tak zpoždění restartování emulátoru) a ukončeno, pouze pokud je již není potřeba.

-   **Minimalizovat** &ndash; nechá spuštěným emulátorem ale minimalizuje na hlavním panelu.

-   **Napájení** &ndash; Simulates zapnutí zařízení a vypnutí. (V emulátoru zůstane spuštěný.)

-   **Více touch** &ndash; překryvy několik teček na zařízení zobrazit které působí jako touch body roztáhnout a přiblížení a oddálení.
    Přetáhněte jeden tečkou způsobí, že přesunout v opačném směru, simulaci Pohyb prstem dvě tečky.

-   **Jeden vstupní bod myši** &ndash; vrátí zařízení do jediný bod vstup (po použití více dotykové ovládání).

-   **Otočit doleva či Otočit vpravo** &ndash; pomáhá testovat, jak se aplikace reaguje na změny orientace. Například při prvním *Otočit doleva* po kliknutí na tlačítko, emulátoru dojde k přepnutí na režim na šířku. Pokud *Otočit doprava* emulátoru se vrátit do režimu na výšku stisknutí tlačítka.

-   **Přizpůsobit obrazovce** &ndash; zvětší velikost obrazovce emulátoru tak, aby odpovídal na pracovní ploše.

-   **Zvětšení** &ndash; škáluje obrazovce emulátoru 33 %, 50 %, 66 %, 100 %, nebo vlastní určitý počet procent.


*Další nástroje* tlačítko se zobrazí dialogové okno otevře, které zobrazuje emulátoru speciální funkce:

[![Další nástroje – dialogové okno](visual-studio-android-emulator-images/05-additional-tools-vs-sml.png)](visual-studio-android-emulator-images/05-additional-tools-vs.png#lightbox)


Každý další funkce je dostupná z řady karet v horní části dialogového okna:

-   **Zrychlení** &ndash; simuluje zařízení pohyb v 3D prostoru.

-   **Umístění** &ndash; uvede mapu, která slouží k výběru a simulovat GPS umístění. Na této mapě *mapování body* lze vytvořit pro simulaci přesun mezi umístěními.

-   **Baterie** &ndash; poskytuje jezdce k simulaci množství poplatků left v baterie.

-   **Snímek obrazovky** &ndash; na této kartě **zaznamenat** tlačítko, které trvá snímek a zobrazí rychlých preview. *Uložit* tlačítko uloží na snímku obrazovky.

-   **Fotoaparát** &ndash; Simulates pořizování snímku prostřednictvím pevné animovaný obrázky obrázek ze souboru nebo z připojených webová kamera v hostitelském počítači. Je možné vybrat přední nebo zadní kamery.

-   **Karta SD** &ndash; emulátoru můžete vytvořte složku na hostitelském počítači k dispozici pro zařízení jako SD karty.
    Když aplikace čte a zapisuje soubory simulované SD karty, jsou dostupné přímo z plochy bez použití `adb` příkaz.

-   **Síť** &ndash; zobrazí souhrn nastavení sítě v emulátoru (emulátoru opětovně používá připojení k síti hostitelského počítače).

Další informace o tom, jak tyto funkce používají, najdete v části [emulátor představení sady Visual Studio pro Android](https://blogs.msdn.microsoft.com/visualstudioalm/2014/11/12/introducing-visual-studios-emulator-for-android/).



## <a name="configuring-device-profiles"></a>Konfigurování profilů zařízení

Microsoft Android emulátoru zahrnuje sadu profily zařízení, které představují nejoblíbenější Android verze, velikost obrazovky a vlastnosti hardwaru zařízení se systémem Android na trhu. Kromě toho tyto profily zařízení jsou již nakonfigurované na různých verzí Android například KitKat, typu Lupa a Marshmallow.

*Správce emulátorů* slouží k instalaci, odinstalaci a spustit profily zařízení. Z **nástroje** nabídce vyberte možnost **Visual Studio Emulator for Android...**  , které je uvedené v tomto snímku obrazovky:

[![Spouštění v emulátoru z nabídky Nástroje](visual-studio-android-emulator-images/06-launch-emulator-manager-vs-sml.png)](visual-studio-android-emulator-images/06-launch-emulator-manager-vs.png#lightbox)

Tím se otevře **profily zařízení** dialogové okno. Nainstalované profily jsou vyznačené v horní části seznamu profil zařízení. Jsou neaktivní profily, které nejsou nainstalovány (ale jsou k dispozici pro instalaci):

[![Ikony profily zařízení](visual-studio-android-emulator-images/07-device-profiles-vs-sml.png)](visual-studio-android-emulator-images/07-device-profiles-vs.png#lightbox)

Pokud chcete nainstalovat nový profil, klikněte na ikonu instalace profilu (klesající odkazováno šipka jak je uvedené výše uvedený snímek obrazovky). Například když kliknete na ikonu instalace profilu **5.7" Phone XHDPI Marshmallow (6.0.0)**, správce emulátorů stáhne profil, jak je vidět tady:

[![Příklad stahování profily](visual-studio-android-emulator-images/08-downloading-profile-vs-sml.png)](visual-studio-android-emulator-images/08-downloading-profile-vs.png#lightbox)

Po stažení profilu zařízení, je označený k označení, že profil byl úspěšně nainstalován. Kliknutím *zobrazit podrobnosti* ikonu se zobrazí typ platformy, architektura procesoru, velikost nebo rozlišení obrazovky a dostupné paměti na zařízení:

[![Zobrazit podrobnosti profil zařízení](visual-studio-android-emulator-images/09-show-details-vs-sml.png)](visual-studio-android-emulator-images/09-show-details-vs.png#lightbox)

Když Visual Studio **cíl ladění** je otevřít rozevírací nabídky, profil nově nainstalované zařízení je nyní k dispozici jako cíl:

[![Nový profil v rozevírací nabídce Cíl](visual-studio-android-emulator-images/10-debug-target-vs-sml.png)](visual-studio-android-emulator-images/10-debug-target-vs.png#lightbox)

Tento seznam lze zkrátit kliknutím **odinstalovat tento profil** v *správce emulátorů* odebrat profily nepoužívaných zařízení. Všimněte si, že aktuálně neexistuje žádný způsob, jak vytvořit profil vlastní zařízení v této emulátoru.


## <a name="troubleshooting"></a>Poradce při potížích

Tato část popisuje některé běžné chyby a řešení při používání *Visual Studio Emulator for Android* s Xamarin.Android.

<a name="cant_connect" />

### <a name="emulator-will-not-start"></a>Emulátor se nespustí.

V některých případech emulátoru nespustí, pokud existují incompabilities mezi procesoru hostitele a virtuálního počítače technologie Hyper-V. Chcete-li tento problém obejít, nakonfigurujte Hyper-V a omezit funkce procesoru, které může mít virtuální počítač &ndash; tím zlepšuje kompatibilitu virtuálního počítače s verzemi procesoru jiného hostitele.
K provedení této změny pomocí následujících kroků:

1.  Klikněte **spustit** tlačítko, zadejte v **konzoly MMC**a stiskněte klávesu **Enter**. Klikněte na tlačítko **Správce technologie Hyper-V** jak je znázorněno zde:

    [![Správce technologie Hyper-V](visual-studio-android-emulator-images/15-launch-hyperv-manager.png)](visual-studio-android-emulator-images/15-launch-hyperv-manager.png#lightbox)

2.  Ve Správci technologie Hyper-V **virtuální počítače** pravém podokně klikněte na emulátoru, chcete-li použít a klikněte na tlačítko Upravit **nastavení...** ":

    [![Položku nabídky nastavení virtuálních počítačů](visual-studio-android-emulator-images/16-vm-settings.png)](visual-studio-android-emulator-images/16-vm-settings.png#lightbox)

3.  V okně nastavení vyhledat **kompatibility** část (v části **hardwaru > procesoru**) a povolte **migrovat do fyzického počítače s jinou verzí procesoru**:

    [![Možnost zaškrtnuta možnost migrace](visual-studio-android-emulator-images/17-set-compatibility-vs-sml.png)](visual-studio-android-emulator-images/17-set-compatibility-vs.png#lightbox)

4.  Klikněte na tlačítko **OK** a zavřete okno Správce technologie Hyper-V.



### <a name="app-deploys-and-starts-but-fails-immediately"></a>Aplikace nasadí a spustí ale selže okamžitě

V této situaci se spustí emulátoru, aplikace se úspěšně nasadí do emulátoru a spuštění aplikace. Aplikaci však selže okamžitě.
V mnoha případech je také důvodem incompabilities mezi procesoru hostitele a virtuálního počítače technologie Hyper-V. Chcete-li tuto chybu vyřešit, postupujte podle pokynů v [nelze spustit emulátoru](#cant_connect) (výše).


### <a name="emulator-stops-with-the-diagnostic-message-libaot-mscorlibdllso-not-found"></a>Emulátor zastaví diagnostická zpráva: **libaot-mscorlib.dll.so nebyl nalezen**

Chcete-li tuto chybu vyřešit, použijte následující kroky zakázat rychlé nasazení:

1.  Postupujte podle pokynů v [nelze spustit emulátoru](#cant_connect) (výše).

2.  Dvakrát klikněte na projekt **vlastnosti**.

3.  Klikněte na tlačítko **Android možnosti** a zrušte výběr **použití rychlého nasazení (pouze v režimu ladění)**:

    [![Použijte možnost rychlého nasazení nezaškrtnuto](visual-studio-android-emulator-images/18-fast-deployment-vs-sml.png)](visual-studio-android-emulator-images/18-fast-deployment-vs.png#lightbox)



### <a name="drag-and-drop-does-not-work"></a>Přetažení nefunguje

Pokud *Visual Studio Emulator for Android* se spustí jako správce (nebo pokud jste spustíte z ze sady Visual Studio s oprávněním správce se spuštěným Visual Studio), přetažení z. APK nebo. ZIP soubory pravděpodobně není pracovní. Chcete-li tento problém obejít, spusťte *Visual Studio Emulator for Android* bez zvýšenými oprávněními (tj, ne jako správce).


### <a name="other-errors"></a>Další chyby

Výše uvedené tipy k řešení potíží se věnují nejběžnějších problémů při používání Android emulátor sady Visual Studio s Xamarin.Android. Podrobnější informace k řešení potíží s Visual Studio Android Emulator najdete v tématu [řešení potíží s Visual Studio Emulator for Android](https://msdn.microsoft.com/en-us/library/mt228282.aspx).



## <a name="summary"></a>Souhrn

Tento článek zavedená *Visual Studio Emulator for Android*; ho vysvětlené použití emulátoru k ladění aplikací Xamarin.Android v sadě Visual Studio, ho popisu funkce tlačítek ve svislém panelu nástrojů a je poskytovala Stručný přehled funkcí dostupných ve **další nástroje** dialogové okno. Ho vysvětlení, jak používat *správce emulátorů* k instalaci, odinstalaci a spusťte profily zařízení. A *Poradce při potížích s* části vysvětlené běžné problémy a řešení pomocí emulátoru.


## <a name="related-links"></a>Související odkazy

- [Emulátor sady Visual Studio pro Android](https://www.visualstudio.com/vs/msft-android-emulator/)
- [Představení emulátor sady Visual Studio pro Android](https://blogs.msdn.microsoft.com/visualstudioalm/2014/11/12/introducing-visual-studios-emulator-for-android/)
