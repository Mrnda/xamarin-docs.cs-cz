---
title: Funkce typu Lupa
description: "Tento článek obsahuje podrobný přehled nových funkcí byla zavedená v systému Android 5.0 (typu Lupa). Tyto funkce patří názvem materiálu motiv, jakož i nové podpůrné funkce, jako třeba animací, zobrazení stíny a drawable barevný nádech nového stylu rozhraní uživatele. Android 5.0 také zahrnuje rozšířené oznámení, dvě nové uživatelské rozhraní pomůcky, nové plánovače úloh a několik nových rozhraní API ke zlepšení úložiště, sítě, připojení a multimediální možnosti."
ms.topic: article
ms.prod: xamarin
ms.assetid: 1CE99CFE-FAAC-49FC-AEDC-1A21FC6E946E
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/16/2018
ms.openlocfilehash: de6829a0a698133ad9002ead1cd7c534a30b1f6c
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 03/09/2018
---
# <a name="lollipop-features"></a>Funkce typu Lupa

_Tento článek obsahuje podrobný přehled nových funkcí byla zavedená v systému Android 5.0 (typu Lupa). Tyto funkce patří názvem materiálu motiv, jakož i nové podpůrné funkce, jako třeba animací, zobrazení stíny a drawable barevný nádech nového stylu rozhraní uživatele. Android 5.0 také zahrnuje rozšířené oznámení, dvě nové uživatelské rozhraní pomůcky, nové plánovače úloh a několik nových rozhraní API ke zlepšení úložiště, sítě, připojení a multimediální možnosti._

## <a name="lollipop-overview"></a>Přehled typu Lupa

Android 5.0 (typu Lupa) představuje o nový jazyk návrhu *materiálu návrhu*, a s ním podpůrný přetypovat nových funkcí, aby aplikace jednodušší a intuitivnější používat. V návrhu materiálu Android 5.0 nejen dává telefony Android facelift; také poskytuje novou sadu pravidel návrhu pro tablety systémem Android, stolní počítače, sleduje a inteligentní televizní přijímače. Tato pravidla návrhu zdůraznil jednoduchost a minimalism při provádění pomocí známých taktilní atributů (například realistické prostor a hraniční upozornění) pomáhají uživatelům rychle a intuitivně pochopit rozhraní.

*Podstatným motiv* je ztělesněním tyto zásady designu uživatelského rozhraní v Android. Tento článek se začne tím, že zahrnuje materiálu motiv podpůrné funkce:

-   **Animace** &ndash; *Touch zpětné vazby* animací, *aktivity přechod* animací, *zobrazit přechod stavu* animace a *odhalit vliv*.

-   **Zobrazit stínů a zvýšení oprávnění** &ndash; zobrazení teď mají `elevation` vlastnost; zobrazení s vyšší `elevation` hodnoty přetypovat větší stínů na pozadí.

-   **Barva funkce** &ndash; *Drawable barevný nádech* umožňuje znovu použít bitovou kopii prostředky, změna jejich barvy a *viditelného barva extrakce* umožňuje dynamicky Motiv aplikace podle barev v obraze.

Mnoho motiv materiály, které funkce jsou již součástí rozhraní Android 5.0 prostředí, zatímco ostatní musí být explicitně přidán do aplikace. Například některé standardní zobrazení (např. tlačítka) již zahrnout animace zpětné vazby touch, zatímco aplikace musíte povolit většině zobrazení stínů.

Kromě vylepšení uživatelského rozhraní, nebude prostřednictvím materiálu motiv Android 5.0 také zahrnuje několik dalších nových funkcí, které jsou popsané v tomto článku:

-   **Rozšířené oznámení** &ndash; oznámení v systému Android 5.0 byly výrazně aktualizovány s nový vzhled, podporu pro oznámení zamykací obrazovky a novou *z pohotového* formát prezentace oznámení.

-   **Nové uživatelské rozhraní pomůcek** &ndash; nové `RecyclerView` pomůcky je jednodušší a předání informací o tom velkých datových sad a komplexní informace a nové aplikace `CardView` pomůcky poskytuje zjednodušenou prezentace karty jako formát pro zobrazení textu a bitové kopie.

-   **Nová rozhraní API** &ndash; Android 5.0 přidá nové rozhraní API pro podporu více sítí, vylepšené připojení Bluetooth, snadnější správu úložiště a flexibilnější řízení multimédií přehrávače a fotoaparát zařízení. Plánování funkce je k dispozici pro spuštění úlohy asynchronně v naplánovaném čase. Tato funkce vám pomůže životnosti, například plánování úloh, které se při zařízení je napájený ze sítě a poplatků.


## <a name="requirements"></a>Požadavky

Toto je potřeba použít nové funkce systému Android 5.0 v aplikace založené na Xamarinu:

-   **Xamarin.Android** &ndash; Xamarin.Android 4.20 nebo novější musí být nainstalovaná a nakonfigurovaná s Visual Studio nebo Visual Studio for Mac. 

-   **Sady SDK pro Android** &ndash; Android 5.0 (21 rozhraní API) nebo novější musí být nainstalován prostřednictvím Android SDK Manager.

-   **Sady pro vývojáře Java** &ndash; Xamarin.Android vyžaduje [JDK 1.8](http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html) nebo novější, pokud vyvíjíte pro úroveň rozhraní API 24 nebo větší (JDK 1.8 také podporuje úrovně rozhraní API starší než 24, včetně typu Lupa). 64bitové verze JDK 1.8 je vyžaduje, pokud používáte vlastní ovládací prvky nebo prohlížeč formulářů.

Můžete dál používat [JDK 1.7](http://www.oracle.com/technetwork/java/javase/downloads/jdk7-downloads-1880260.html) Pokud vývoj speciálně pro úroveň rozhraní API 23 nebo starším.


## <a name="setting-up-an-android-50-project"></a>Nastavení projektu pro Android 5.0

Pokud chcete vytvořit projekt Android 5.0, je nutné nainstalovat nejnovější nástroje a balíčky SDK. Pro nastavení projektu Xamarin.Android s cílem Android 5.0 použijte následující postup:

1. Instalace nástrojů pro Xamarin.Android a aktivovat licence na aplikaci Xamarin. V tématu [instalační program a instalace](~/android/get-started/installation/index.md) pro další informace o instalaci Xamarin.Android.

2. Pokud používáte Visual Studio pro Mac, nainstalujte nejnovější aktualizace systému Android 5.0.

3. Spustit Android SDK Manager (v sadě Visual Studio pro Mac, použijte **nástroje &gt; otevřete Android SDK Manager&hellip;**) a nainstalujte nástroje pro Android SDK 23.0.5 nebo novější:

    [![Nástroje pro výběr sady SDK pro Android v Android SDK Manager](lollipop-images/android-l-tools-sml.png)](lollipop-images/android-l-tools.png#lightbox)

   Také instalaci nejnovější sady SDK pro Android 5.0 balíčků (API 21 nebo novější):

    [![Instalace sady SDK pro Android 5.0 balíčků v Android SDK Manager](lollipop-images/android-l-sdk-pkgs-sml.png)](lollipop-images/android-l-sdk-pkgs.png#lightbox)

   Další informace o používání Android SDK Manager najdete v tématu [SDK Manager](http://developer.android.com/tools/help/sdk-manager.html).

4. Vytvoření nového projektu Xamarin.Android. Pokud jste pro vývoj pro Android pomocí Xamarinu nové, přečtěte si téma [Hello, Android](~/android/get-started/hello-android/index.md) Další informace o vytváření projektů Android. Když vytvoříte projekt pro Android, ujistěte se, zda je verze konfigurace pro Android 5.0.
   V sadě Visual Studio pro Mac, přejděte na **možnosti projektu &gt; sestavení &gt; Obecné** a nastavte **cílové rozhraní** k **Android 5.0 (typu Lupa)** nebo později:

    ![Nastavení cílového Framwework na Android 5.0 typu Lupa](lollipop-images/target-framework.png)

   V části **možnosti projektu &gt; sestavení &gt; aplikace pro Android**, nastavit minimální a cílové verzi systému Android k **automatické - použití target framework verze**:

    ![Nastavení minimální a cílové Android verze na automaticky](lollipop-images/minimum-android-version.png)

5. Nakonfigurujte emulátoru nebo zařízení se systémem Android k testování aplikace. Pokud používáte emulátor, přečtěte si téma [Android – nastavení emulátoru](~/android/get-started/installation/android-emulator/index.md) se dozvíte, jak nakonfigurovat emulátoru Androidu pro použití s Xamarin Studio nebo Visual Studio. Pokud používáte zařízení se systémem Android, najdete v části [nastavení se sada SDK Preview](https://developer.android.com/preview/setup-sdk.html) se dozvíte, jak aktualizace zařízení Android 5.0. Konfigurace zařízení s Androidem pro spouštění a ladění aplikací Xamarin.Android najdete v tématu [nastavit zařízení pro vývoj](~/android/get-started/installation/set-up-device-for-development.md).

Poznámka: Pokud aktualizujete stávající projekt pro Android, který byl cílení na Android L Preview, musíte aktualizovat **cílové rozhraní** a **verzi systému Android** hodnoty popsané výše.

## <a name="important-changes"></a>Důležité změny

Dříve publikované aplikace pro Android může mít vliv změny v systému Android 5.0. Konkrétně Android 5.0 používá nový modul runtime a formát značně změněné oznámení.

### <a name="android-runtime"></a>Android Runtime

Android 5.0 používá jako výchozí runtime místo Dalvik nový Android Runtime (OBRÁZEK). OBRÁZKY implementuje několik hlavní nové funkce:

-   **Napřed předčasné (AOT) kompilace** &ndash; AOT lze vylepšit výkon aplikace kompilování kódu aplikace před prvním spuštění aplikace. Pokud je aplikace nainstalovaná, UMĚLECKÝCH generuje kompilované aplikace, která je spustitelný soubor pro cílové zařízení.

-   **Vylepšené uvolňování paměti (GC)** &ndash; GC vylepšení v obrázky mohou rovněž zlepšit výkon aplikace. Uvolňování paměti teď místo dvou používá jeden GC pozastavení a souběžných operací GC dokončete více včas.

-   **Vylepšení ladění aplikace** &ndash; obrázky poskytuje další diagnostiky podrobností, které se vám pomůžou při analýze výjimky a havárií sestavy.

Existující aplikace by měla fungovat bez změn v rámci obrázky &ndash; s výjimkou aplikace, které využívají techniky, které jsou jedinečné pro předchozí modul runtime Dalvik, který nemusí fungovat v části obrázky. Další informace o těchto změnách najdete v tématu [ověření chování aplikace na Android Runtime (obrázky)](http://developer.android.com/guide/practices/verifying-apps-art.html).


### <a name="notification-changes"></a>Oznámení změn

Oznámení se v systému Android 5.0 významně změnily:

-   **Zvuky a vibrace jsou zpracovány jinak** &ndash; vyznívá oznámení a vibrace teď zpracovává `Notification.Builder` místo `Ringtone`, `MediaPlayer`, a `Vibrator`.

-   **Nové barevné schéma** &ndash; v souladu s materiálu motiv oznámení jsou vykreslovány jako tmavý text přes pozadí bílé nebo velmi malé. Také lze upravit alfa kanály v ikony oznámení Android ke koordinaci s systému barevná schémata. 

-   **Zamykací obrazovky oznámení** &ndash; oznámení můžete nyní se zobrazí na zamykací obrazovky zařízení.

-   **Z pohotového** &ndash; s vysokou prioritou oznámení se nyní zobrazí v malém okně plovoucí (z pohotového oznámení) Pokud se zařízení odemkne a obrazovky je zapnutý.

Ve většině případů portování stávající funkce oznámení aplikaci pro Android 5.0 vyžaduje následující kroky:

1.  Převést kód pro použití `Notification.Builder` (nebo `NotificationsCompat.Builder`) pro vytvoření oznámení. 

2.  Ověřte, zda jsou vaše existující prostředky oznámení lze zobrazit v nové barevné schéma materiálu motivu.

3.  Rozhodněte, jaké viditelnost, vaše oznámení by měla obsahovat poté, co se zobrazí na zamykací obrazovky. Pokud není veřejný oznámení, který obsah by měla zobrazovat ve zamykací obrazovky?

4.  Nastavit kategorii oznámení tak, že jsou správně zpracovány v nové Android 5.0 *nebudou narušovat* režimu.

Pokud vaše oznámení k dispozici přenos, ovládací prvky zobrazení média přehrávání stavu, použijte `RemoteControlClient`, nebo volejte `ActivityManager.GetRecentTasks`, najdete v části [důležité změny chování](http://developer.android.com/preview/api-overview.html#Behaviors) pro další informace o aktualizaci oznámení pro Android 5.0.

Informace o vytváření oznámení v Android najdete v tématu [místního oznámení](~/android/app-fundamentals/notifications/local-notifications.md). [Kompatibility](~/android/app-fundamentals/notifications/local-notifications.md#compatibility) tohoto článku vysvětluje, jak vytvořit oznámení, které jsou klesající kompatibilní s předchozími verzemi systému Android.


## <a name="material-theme"></a>Podstatným motiv

Nový motiv materiálu Android 5.0 přináší obezřetností změny vzhledu a chování rozhraní Android. Vizuální prvky teď použít taktilní povrchy, které převezmou tučné obrázků, typografii a jasně barev návrhu na základě tisk. Příklady motivu materiálu jsou použité v ukázkách na následujících snímcích obrazovky:

[![Snímky obrazovky materiálu motiv domovské obrazovce, obrazovky aplikace a nastavení obrazovky](lollipop-images/android-5-gallery-labeled-sml.png)](lollipop-images/android-5-gallery-labeled.png#lightbox)

Android 5.0 zobrazí přání s domovskou obrazovku zobrazený na levé straně. Snímek obrazovky center na první obrazovce seznam aplikací a je na snímku obrazovky na pravé straně **nastavení** obrazovky. Google [materiálu návrhu](https://material.io/guidelines/material-design/introduction.html) specifikace vysvětluje základní pravidla návrhu za nový motiv materiálu koncept.

Podstatným motiv obsahuje tři předdefinované typů, které můžete použít ve vaší aplikaci: `Theme.Material` tmavým motivem (výchozí), `Theme.Material.Light` motiv a `Theme.Material.Light.DarkActionBar` motivu: 

[![Snímky obrazovky tmavý, světlým a DarkActionBar motivů](lollipop-images/three-material-themes-sml.png)](lollipop-images/three-material-themes.png#lightbox)

Další informace o používání funkcí materiálu motiv v Xamarin.Android apps najdete v tématu [materiálu motiv](~/android/user-interface/material-theme.md).


## <a name="animations"></a>Animace

Android 5.0 poskytuje animací touch zpětnou vazbu, aktivity přechod animace a zobrazení stavu přechodu animace do intuitivnější použít rozhraní aplikace. Navíc můžete použít aplikace Android 5.0 *odhalit vliv* animací pro skrytí nebo zobrazení odhalit. Můžete použít *zakřivené pohybu* nastavení pro konfiguraci jak rychle nebo pomalu animací vykreslují.


### <a name="touch-feedback-animations"></a>Touch animací zpětné vazby

Animace zpětnou vazbu touch poskytují uživatelům vizuální zpětnou vazbu, při zobrazení byly změněny. Například tlačítka nyní zobrazit efekt ripple po dotyku &ndash; Toto je výchozí touch zpětnou vazbu animace v systému Android 5.0. Animace ripple je implementováno modulem nové `RippleDrawable` třídy. Účinek ripple můžete nakonfigurovat na končit na rozsah zobrazení nebo přesáhne hranice zobrazení. Například následující posloupnosti snímky obrazovky znázorňuje účinek ripple v tlačítko během touch animace:

![Rámce pomocí rámce snímky obrazovky ripple animace na tlačítka](lollipop-images/touch-animation.png)

Obraťte se na počáteční touch pomocí tlačítka provádí první obrázek na levé straně, zbývající pořadí (zleva doprava) ukazuje, jak ripple účinek šíří a okrajem tlačítko. Při ukončení animace ripple, vrátí se zobrazení k původnímu vzhledu. Animace ripple výchozí probíhá za zlomek druhý, ale délku animace lze přizpůsobit pro delší nebo kratší délky času.

Další informace o touch zpětnou vazbu animace v systému Android 5.0, najdete v části [přizpůsobit Touch zpětné vazby](http://developer.android.com/training/material/animations.html#Touch).


### <a name="activity-transition-animations"></a>Aktivita přechod animace

Aktivita přechod animací uživatelům představu o visual kontinuity v případě jedné aktivity přejde na jiný. Aplikace můžete určit tři typy animací přechod:

-   **Zadejte přechod** &ndash; pro vstupu aktivitu v scény.

-   **Ukončete přechod** &ndash; pro když aktivita opustí scény.

-   **Sdílené element přechod** &ndash; pro změnu zobrazení, které jsou společná pro dvě aktivity jako první aktivitu přechody na další.

Například následující posloupnosti snímky obrazovky znázorňuje přechod sdílené element:

[![Podle snímky obrazovky rámce sdílené elementu animace přechodu rámečku](lollipop-images/activity-transition-sml.png)](lollipop-images/activity-transition.png#lightbox)

Sdílené element (fotografie pásy) je jedním z několika zobrazení v první aktivitu; se zvětšuje k zobrazení pouze v druhé aktivitu jako první aktivitu přechody na druhý.

#### <a name="enter-transition-animation-types"></a>Zadejte typy animace přechodu

Android 5.0 pro enter přechody, poskytuje tři typy animací:

-   **Rozbalit animace** &ndash; zvětší zobrazení na softwaru scény.

-   **Vysuňte animace** &ndash; přesune zobrazení v z jednoho z okrajů scény.

-   **Vykreslit animace** &ndash; vykreslí zobrazení do scény.

#### <a name="exit-transition-animation-types"></a>Ukončení přechodu animace typy

Pro ukončení přechody Android 5.0 poskytuje tři typy animací:

-   **Rozbalit animace** &ndash; zmenšuje zobrazení k centru scény.

-   **Vysuňte animace** &ndash; přesune zobrazení na jednu z okrajů scény.

-   **Vykreslit animace** &ndash; vykreslí zobrazení mimo scény.

#### <a name="shared-element-transition-animation-types"></a>Sdílené typy animace elementu přechodu

Sdílené element přechody podporují více typy animací, například:

-   Změna rozložení nebo klip hranice zobrazení.

-   Změna škálování a otočení zobrazení.

-   Změna velikosti a měřítka typu pro zobrazení.

Další informace o aktivity přechod animace v systému Android 5.0 najdete v tématu [přizpůsobit přechody aktivity](http://developer.android.com/training/material/animations.html#Transitions).


### <a name="view-state-transition-animations"></a>Zobrazení stavu přechodu animace

Android 5.0 umožňuje animací spustit, když se změní stav zobrazení. Pomocí jedné z následujících postupů můžete animace přechodů mezi stavy zobrazení:

-   Vytvořte drawables, který použije animaci změny stavu, které jsou spojené s konkrétním zobrazení. Nové `AnimatedStateListDrawable` třída umožňuje vytvářet drawables, který zobrazí animace mezi změny stavu zobrazení.

-   Definujte animace funkce, které běží při změně stavu zobrazení. Nové `StateListAnimator` třída umožňuje definovat animator, který spouští při změně stavu zobrazení.

Další informace o zobrazení stavu přechodu animace v systému Android 5.0 najdete v tématu [animace změny stavu zobrazení](http://developer.android.com/training/material/animations.html#ViewState).


### <a name="reveal-effect"></a>Odhalit vliv

*Odhalit vliv* je kruh výstřižek této změny radius odhalit nebo skrytí zobrazení. Tento efekt lze řídit nastavení počáteční a finální radius výstřižek kruhu. Následující posloupnosti snímky obrazovky znázorňuje animace zobrazení vliv z centra obrazovky:

[![Rámce pomocí rámce snímky obrazovky animace zobrazení](lollipop-images/reveal-center-sml.png)](lollipop-images/reveal-center.png#lightbox)

Další pořadí znázorňuje vliv animace zobrazení, která se provádí z levého dolního rohu obrazovky:

[![Rámce pomocí rámce snímky obrazovky výstřižek animace](lollipop-images/reveal-left-sml.png)](lollipop-images/reveal-left.png#lightbox)

Odhalit, že animace lze vrátit zpět; To znamená kruhu výstřižek můžete zmenšit skrytí zobrazení místo zvětšit na nich zobrazení.

Další informace o zobrazení účinek Android 5.0 v, najdete v části [použijte efekt odhalit](http://developer.android.com/training/material/animations.html#Reveal).


### <a name="curved-motion"></a>Zakřivené pohybu

Kromě tyto funkce animace Android 5.0 také nabízí nová rozhraní API, které vám umožní zadat čas a pohybu křivek animací. Android 5.0 používá tyto křivky promítnout dočasné a prostorových pohyb během animace. Tři křivky jsou definovány v systému Android 5.0:

-   **Rychlé\_out\_lineární\_v** &ndash; rychle zrychluje a pokračuje v až do konce animace urychlit.

-   **Rychlé\_out\_pomalé\_v** &ndash; Accelerates rychle a pomalu zpomaluje na konci animace.

-   **Lineární\_out\_pomalé\_v** &ndash; Begins s rychlosti ve špičce a pomalu zpomaluje na konec animace.

Můžete použít nové `PathInterpolator` třídu k určení, jak probíhá interpolace pohybu. `PathInterpolator` je interpolátor, který prochází skrz animace cesty podle zadaného kontrolních bodů a křivek pohybu. Další informace o tom, jak zadat nastavení zakřivené pohybu v systému Android 5.0 najdete v tématu [použití zakřivené pohybu](http://developer.android.com/training/material/animations.html#CurvedMotion).


## <a name="view-shadows--elevation"></a>Zobrazení stínů & zvýšení oprávnění

V systému Android 5.0, můžete zadat *zvýšení* zobrazení nastavením nové `Z` vlastnost. Více `Z` hodnotu způsobí, že zobrazení přetypovat větší stínu na pozadí, které pravděpodobně float vyšší výše na pozadí zobrazení. Počáteční zvýšení zobrazení můžete nastavit tak, že nakonfigurujete jeho `elevation` atribut v rozložení.

Následující příklad ilustruje stínů přetypovat podle prázdnou `TextView` řízení, pokud jeho atribut zvýšení oprávnění nastavená na 2dp, 4dp a 6dp, v uvedeném pořadí:

[![Snímky obrazovky progessively větší stínů zobrazení](lollipop-images/view-shadows-sml.png)](lollipop-images/view-shadows.png#lightbox)

Nastavení stínových zobrazení může být statická (jak je uvedeno výše) nebo jejich lze použít v animací, aby zobrazení zobrazí dočasně zvýšení výše pozadí zobrazení. Můžete použít `ViewPropertyAnimator` třídy animace zvýšení oprávnění zobrazení. Zvýšení úrovně zobrazení je součet hodnot jeho rozložení `elevation` nastavení plus `translationZ` vlastnost, která můžete nastavit prostřednictvím `ViewPropertyAnimator` volání metody.

Další informace o zobrazení stínů v systému Android 5.0 najdete v tématu [výstřižek zobrazení a definování stínů](http://developer.android.com/training/material/shadows-clipping.html).


## <a name="color-features"></a>Barva funkce

Android 5.0 obsahuje dvě nové funkce pro správu barev v aplikacích:

-   *Drawable barevný nádech* umožňuje měnit barvy obrázku prostředků změnou atribut rozložení.

-   *Extrakce viditelného barva* umožňuje dynamicky přizpůsobit barevný motiv vaší aplikace pro koordinaci s paletu barev zobrazeného obrázku.


### <a name="drawable-tinting"></a>Drawable barevný nádech

Android 5.0 rozložení rozpoznat novou `tint` atribut, který slouží k nastavení barvy drawables bez nutnosti vytvořit několik verzí těchto prostředků k zobrazení různých barev. Chcete-li tuto funkci používat, je definovat rastrový obrázek jako alfa maska a použití `tint` atribut pro definování barvy prostředku. Díky tomu je možné, můžete vytvářet prostředky jednou a barvu, která je ve svém rozvržení tak, aby odpovídaly vaší motivu.

V následujícím příkladu, jediný zdroj obrázku &ndash; bílé logo s průhledným pozadím &ndash; se používá k vytvoření TINT – rozdíly:

![Logo bílé Xamarin s průhledným pozadím](lollipop-images/xamarin-logo-white.png)

Jak je znázorněno v následujících příkladech, zobrazí se toto logo nad blue cyklické pozadí. Bitovou kopii na levé straně je, jak se zobrazuje logo bez `tint` nastavení. Na obrázku center, logo `tint` nastavený atribut tmavý šedou barvu. Na obrázku na pravé straně `tint` je nastaven na světle šedou:

![Příklady výše logo s jinou TINT – nastavení](lollipop-images/drawable-tinting.png)

Další informace o drawable barevný nádech v systému Android 5.0 najdete v tématu [Drawable barevný nádech](http://developer.android.com/training/material/drawables.html#DrawableTint).


### <a name="prominent-color-extraction"></a>Extrakce viditelného barev

Nové Android 5.0 `Palette` třída umožňuje extrahovat barvy z obrázku tak, aby lze následně použít dynamicky na vlastních barev palety. `Palette` Třídy extrahuje šest barvy z obrázku a popisků tyto barvy podle jejich relativní úrovně sytost barev a také průraznost:

-   Živoucí

-   Živoucí světlý

-   Živoucí světlý

-   Ztlumen

-   Muted světlý

-   Slabá ztlumen

Na následujících snímcích obrazovky, například fotografie zobrazení aplikace extrahuje výrazné barvy z obrázku na zobrazení a přizpůsobit barevné schéma aplikace tak, aby odpovídaly bitovou kopii pomocí těchto barev:

[![Snímky obrazovky extrakce barvu motivu zelené, růžové a modré](lollipop-images/prominent-color-extraction-sml.png)](lollipop-images/prominent-color-extraction.png#lightbox)

Na snímcích obrazovky výše na panelu akcí nastavena na extrahované "živoucí light" barvy a pozadí nastavena na extrahované "živoucí světlý" barev. V příkladu výše každý řádek kvadratických malé barva je zahrnuta pro ilustraci barvy palety, které se extrahují z bitové kopie.

Další informace o extrakce barev v systému Android 5.0 najdete v tématu [extrahování výrazné barvy z obrázku](http://developer.android.com/training/material/drawables.html#ColorExtract).


## <a name="new-ui-widgets"></a>Nové widgety uživatelského rozhraní

Android 5.0 obsahuje dvě pomůcky nového uživatelského rozhraní:

-   `RecyclerView` &ndash; Zobrazení skupinu, která zobrazí seznam posouvatelného položky.

-   `CardView` &ndash; Základní rozložení se zaoblenými hranami.

Obě pomůcky zahrnují podporu zaručená pro motiv materiálu funkce; například `RecyclerView` používá animací pro přidávání a odebírání zobrazení, a `CardView` používá zobrazení stínů aby každou kartu pravděpodobně float výše na pozadí. Na následujících snímcích obrazovky jsou uvedeny příklady tyto nové pomůcky:

[![Snímky obrazovky aplikace vytvořené s RecyclerView](lollipop-images/recyclerview-cardview-sml.png)](lollipop-images/recyclerview-cardview.png#lightbox)

Snímek obrazovky na levé straně je příkladem `RecyclerView` používá v e-mailovou aplikaci a na snímku obrazovky na právo je příkladem `CardView` jako použít v aplikaci rezervace cesta.


### <a name="recyclerview"></a>RecyclerView

`RecyclerView` je podobná `ListView,` ale lépe je vhodná pro velké sady zobrazení nebo seznamy s prvky, které se dynamicky mění. Jako `ListView,` zadat adaptér pro přístup k podkladové datové sady. Ale na rozdíl od `ListView,` používáte *manager rozložení* na pozici položky v rámci `RecyclerView`. Správce rozložení také postará zobrazení recyklace; spravuje opakovaného použití zobrazení položky, které již nejsou viditelné pro uživatele.

Při použití `RecyclerView` pomůcky, je nutné zadat `LayoutManager` a adaptér. Jak je vidět na tomto obrázku `LayoutManager` je zprostředkovatel mezi adaptéru a `RecyclerView`:

![Diagram RecyclerView s podpůrnými LayoutManager adaptéru a datové sady](lollipop-images/recyclerview-diagram.png)

Na následujících snímcích obrazovky ilustraci `RecyclerView` obsahuje 100 položky (každá položka se skládá z `ImageView` a `TextView`):

[![Snímky obrazovky aplikace RecyclerView posouvání pomocí bitové kopie](lollipop-images/recyclerview-scroll-sml.png)](lollipop-images/recyclerview-scroll.png#lightbox)

`RecyclerView` zpracuje tento velké sady dat snadno &ndash; posouvání od začátku seznamu na konec seznamu v této ukázce aplikace trvá jenom pár sekund. `RecyclerView` podporuje také animací; ve skutečnosti animací pro přidávání a odebírání položek jsou povolené ve výchozím nastavení. Když je přidat položku do `RecyclerView`, je oznámení v, jak je znázorněno v tomto pořadí snímky obrazovky:

[![Rámce pomocí rámce snímek obrazovky fotografie směrem položek v](lollipop-images/recyclerview-animation-sml.png)](lollipop-images/recyclerview-animation.png#lightbox)

Další informace o `RecyclerView`, najdete v části [RecyclerView](~/android/user-interface/layouts/recycler-view/index.md).


### <a name="cardview"></a>Zobrazení karty aplikace

`CardView` je jednoduché zobrazení, která simuluje plovoucí karet se zaoblenými hranami. Protože `CardView` má stínů předdefinovaných zobrazení, poskytuje snadný způsob můžete přidat hloubky do vaší aplikace. Na následujících snímcích obrazovky zobrazit tři orientované text příklady `CardView`:

[![Příklad snímky obrazovky aplikace RecyclerView pomocí položky na základě zobrazení karty aplikace](lollipop-images/recyclerview-cardview-sml.png)](lollipop-images/recyclerview-cardview.png#lightbox)

Každý z karty ve výše uvedeném příkladu obsahuje `TextView`; barvu pozadí jsou nastavena pomocí `cardBackgroundColor` atribut.

Další informace o `CardView`, najdete v části [zobrazení karty aplikace](~/android/user-interface/controls/card-view.md).


## <a name="enhanced-notifications"></a>Rozšířené oznámení

Oznámení systému Android 5.0 se aktualizovala výrazně se nový formát visual a nové funkce. Oznámení mají nový vzhled v systému Android 5.0. Například oznámení v systému Android 5.0 teď použijte tmavý text přes světla pozadí:

![Příklad e zrušení rozšířené oznámení Android 5.0](lollipop-images/expanded-notification-contracted.png)

Při zobrazení velkých ikon v oznámení (jak je uvedeno v předchozím příkladu), Android 5.0 uvede malé ikony jako oznámení "BADGE" přes velkých ikon. 

V systému Android 5.0 můžete také zobrazí oznámení na zamykací obrazovky zařízení.
Zde je například příklad snímek obrazovky zamykací obrazovky při jednom oznámení:

[![Snímek obrazovky oznámení na zamykací obrazovce](lollipop-images/lockscreen-notification-sml.png)](lollipop-images/lockscreen-notification.png#lightbox)

Uživatelé mohou poklepání oznámení na zamykací obrazovky k odemknutí zařízení a přejít na aplikaci, která pochází oznámení, nebo prstem k zavření oznámení. Oznámení obsahovat nový *viditelnost* nastavení, která určuje, kolik obsahu lze zobrazit na zamykací obrazovky. Uživatelé mohou zvolit, jestli se má povolit citlivého obsahu, která se má zobrazit v oznámeních zamykací obrazovky.

Android 5.0 zavádí nový formát prezentace oznámení s vysokou prioritou názvem *z pohotového*. Oznámení z pohotového posuňte se dolů z horní části obrazovky na několik sekund a pak retreat zpět na odstín oznámení v horní části obrazovky. Oznámení z pohotového umožňují systému uživatelského rozhraní pro umístění důležité informace u uživatele, a to bez přerušení probíhající aktivity. Následující příklad ukazuje jednoduchý z pohotového oznámení, že se zobrazí nad aplikace:

[![Příklad heads-up oznámení](lollipop-images/heads-up-notification-sml.png)](lollipop-images/heads-up-notification.png#lightbox)

Oznámení z pohotového jsou obvykle používány pro následující události:

-   Další novou zprávu

-   Příchozí telefonního hovoru

-   Indikace nízký stav baterie.

-   Alarm

Android 5.0 zobrazí oznámení ve formátu z pohotového jenom v případě, že má nastavení priority vysoká nebo max.

V systému Android 5.0 můžete zadat metadata oznámení pomohou Android řazení a více inteligentně zobrazovat oznámení. Android 5.0 organizuje oznámení podle priority, viditelnost a kategorie.
Kategorie oznámení jsou používány pro filtrování oznámení, která lze zobrazit, když je zařízení v *nebudou narušovat* režimu.

Podrobné informace o vytváření a spouštění oznámení s nejnovější funkce systému Android 5.0 naleznete v tématu [místního oznámení](~/android/app-fundamentals/notifications/local-notifications.md).


## <a name="new-apis"></a>Nová rozhraní API

Kromě nových funkcí vzhled a chování popsané výše Android 5.0 přidá nové rozhraní API, které rozšiřují možnosti existující multimédia, úložiště a funkce bezdrátové nebo připojením. Android 5.0 obsahuje také nové rozhraní API, která poskytuje podporu pro nové funkce plánovače úloh.

### <a name="camera"></a>Fotoaparát

Android 5.0 poskytuje několik nových rozhraní API pro možnosti rozšířené fotoaparát. Nové `Android.Hardware.Camera2` obor názvů zahrnuje funkce pro přístup k fotoaparátu jednotlivých zařízení připojené k zařízení se systémem Android. Navíc `Android.Hardware.Camera2` modelů každé fotoaparát zařízení jako kanál: přijímá žádosti o zachycení, zachycení image a pak výstupu výsledek. Tento přístup umožňuje aplikace do fronty více žádostí zachycení fotoaparát zařízení.

Následující rozhraní API umožňují tyto nové funkce:

-   `CameraManager.GetCameraIdList` &ndash; Umožňuje programový přístup fotoaparát zařízení; používáte `CameraManager.OpenCamera` pro připojení k určité fotoaparát zařízení.

-   `CameraCaptureSession` &ndash; Zaznamená nebo datové proudy obrázky z fotoaparátu zařízení. Můžete implementovat `CameraCaptureSession.CaptureListener` rozhraní pro zpracování nové události zachycení bitové kopie.

-   `CaptureRequest` &ndash; Definuje parametry zachycení.

-   `CaptureResult` &ndash; Poskytuje výsledky operace zachycení bitové kopie.

Další informace o kamera nové rozhraní API v systému Android 5.0 najdete v tématu [média](http://developer.android.com/about/versions/android-5.0.html#Media).

### <a name="audio-playback"></a>Přehrávání zvuku

Android 5.0 aktualizace `AudioTrack` třídy pro lepší přehrávání zvuku:

-   `ENCODING_PCM_FLOAT` &ndash; Nakonfiguruje `AudioTrack` tak, aby přijímal zvuk data ve formátu s plovoucí desetinnou čárkou pro lepší dynamický rozsah, větší prostor a vyšší kvality (díky zvýšené přesnost). Také pomáhá zamezit zvuk výstřižek s plovoucí desetinnou čárkou formátu.

-   `ByteBuffer` &ndash; Teď můžete zadat zvuk data `AudioTrack` jako bajtové pole.

-   `WRITE_NON_BLOCKING` &ndash; Tato možnost zjednodušuje ukládání do vyrovnávací paměti a více vláken pro některé aplikace.

Další informace o `AudioTrack` vylepšení v systému Android 5.0, najdete v části [média](http://developer.android.com/about/versions/android-5.0.html#Media).

### <a name="media-playback-control"></a>Ovládání přehrávání média

Android 5.0 zavádí nové `Android.Media.MediaController` třídy, které nahradí `RemoteControlClient`. `Android.Media.MediaController` poskytuje zjednodušenou přenos řízení rozhraní API a nabízí ovládání vláken přehrávání mimo kontext uživatelského rozhraní. Následující nová rozhraní API zpracování řízení přenosu:

-   `Android.Media.Session.MediaSession` &ndash; Relace řízení média, která zpracovává více řadičů. Volání `MediaSession.GetSessionToken` požádat o token, který vaše aplikace používá k interakci se relace.

-   `MediaController.TransportControls` &ndash; Obslužné rutiny přenosu příkazy, jako **přehrání**, **Zastavit**, a **přeskočit**.

Navíc můžete použít nové `Android.App.Notification.MediaStyle` třídy relaci média přidružit obsah bohaté oznámení (například extrahování a zobrazující alba).

Další informace o nových funkcích řízení přehrávání média v nástroji Android 5.0 najdete v tématu [média](http://developer.android.com/about/versions/android-5.0.html#Media).

### <a name="storage"></a>Úložiště

Android 5.0 aktualizací přístup rozhraní úložiště, aby bylo snazší pro aplikace pro práci s adresářů a dokumenty:

-   Vyberte adresář podstrom, můžete sestavit a odeslat `Android.Intent.Action.OPEN_DOCUMENT_TREE` záměr. Tento záměr způsobí, že systém zobrazíte všechny instance zprostředkovatele, které podporují výběr podstrom; Uživatel pak umožňuje a vybere adresáře.

-   Chcete-li vytvořit a spravovat nové dokumenty nebo adresáře kdekoli pod podstrom, použijete nové `CreateDocument`, `RenameDocument`, a `DeleteDocument` metody `DocumentsContract`.

-   Chcete-li získat cesty k médiu adresáře na všech zařízeních sdílené úložiště, zavolejte nové `Android.Content.Context.GetExternalMediaDirs` metoda.

Další informace o nové úložiště s rozhraním API v systému Android 5.0 najdete v tématu [úložiště](http://developer.android.com/preview/api-overview.html#Storage).

### <a name="wireless--connectivity"></a>Bezdrátové sítě a připojení

Android 5.0 přidá následující vylepšení rozhraní API pro bezdrátové sítě a připojení:

-   Nové *více sítě* rozhraní API, která umožňují pro aplikace a vyberte sítí s specifické možnosti před vytvořením připojení.

-   Funkci Bluetooth všesměrové vysílání, která umožňuje zařízení se systémem Android 5.0 tak, aby fungoval jako periferních zařízení nízkou energií Bluetooth.

-   Vylepšení NFC, které usnadňují použití funkce bezkontaktní komunikace pro sdílení dat s jinými zařízeními.

Další informace o nové bezdrátové sítě a připojení k rozhraní API v systému Android 5.0 najdete v tématu [bezdrátové sítě a připojení](http://developer.android.com/preview/api-overview.html#Wireless).

### <a name="job-scheduling"></a>Plánování úloh

Android 5.0 představuje novou `JobScheduler` rozhraní API, které uživatelům pomáhá minimalizovat baterie vyprazdňování plánování určitých úloh spustit pouze v případě, že zařízení je napájený ze sítě a poplatků. Tato funkce plánovače úloh lze také při plánování úloh spustit, když jsou vhodnější do této úlohy, jako je například stahování velký soubor, když je zařízení připojené přes síť Wi-Fi místo k měřené síti podmínky.

Další informace o nové rozhraní API v systému Android 5.0 plánování úloh najdete v tématu [plánování úloh](http://developer.android.com/preview/api-overview.html#JobScheduler).

## <a name="summary"></a>Souhrn

V tomto článku poskytují přehled důležité nové funkce v systému Android 5.0 pro Xamarin.Android vývojáři aplikací:

-   Podstatným motiv

-   Animace

-   Zobrazení stíny a zvýšení oprávnění

-   Barva funkce, jako je například drawable barevný nádech a viditelného barvu extrakce

-   Nové `RecyclerView` a `CardView` pomůcky

-   Vylepšení oznámení

-   Nové rozhraní API pro fotoaparát, přehrávání zvuku, řízení média, úložiště, bezdrátové sítě nebo připojením a plánování úloh

Pokud jste pro Xamarin Android vývoj nové, přečtěte si [instalační program a instalace](~/android/get-started/installation/index.md) můžete začít pracovat s Xamarin.Android.
[Hello, Android](~/android/get-started/hello-android/index.md) je vynikající Úvod pro informace o způsobu vytváření projektů Android.



## <a name="related-links"></a>Související odkazy

- [Android L Developer Preview](http://developer.android.com/preview/index.html)
- [Získání sady SDK pro Android](https://developer.android.com/sdk/index.html#Other)
- [Podstatným návrhu](http://developer.android.com/preview/material/index.html)
- [Principy podstatným návrhu](http://static.googleusercontent.com/media/www.google.com/en/us/design/material-design.pdf)
