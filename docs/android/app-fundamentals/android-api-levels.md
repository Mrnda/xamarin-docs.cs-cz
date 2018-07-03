---
title: Principy úrovní rozhraní API systému Android
description: Xamarin.Android má několik nastavení úrovně rozhraní Android API, které určete kompatibilitu vaší aplikace s více verzemi Androidu. Tato příručka vysvětluje, co ta nastavení znamenají, způsob jejich konfigurace a jaký vliv mají na vaši aplikaci za běhu.
ms.prod: xamarin
ms.assetid: 58CB7B34-3140-4BEB-BE2E-209928C1878C
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 07/02/2018
ms.openlocfilehash: 3b060567b47395bc213627c9378de4fca9db41bb
ms.sourcegitcommit: 081a2d094774c6f75437d28b71d22607e33aae71
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/03/2018
ms.locfileid: "37403335"
---
# <a name="understanding-android-api-levels"></a>Principy úrovní rozhraní API systému Android

_Xamarin.Android má několik nastavení úrovně rozhraní Android API, které určete kompatibilitu vaší aplikace s více verzemi Androidu. Tato příručka vysvětluje, co ta nastavení znamenají, způsob jejich konfigurace a jaký vliv mají na vaši aplikaci za běhu._


## <a name="quick-start"></a>Rychlý Start

Xamarin.Android zpřístupňuje tři nastavení rozhraní Android API úrovně projektu:

-   [Cílové rozhraní Framework](#framework) &ndash; Určuje, které architekturu pro použití při vytváření vaší aplikace. Tato úroveň rozhraní API se používá v *kompilaci* čas pomocí Xamarin.Android.

-   [Minimální verze Androidu](#minimum) &ndash; určuje nejstarší verze Androidu, který má vaše aplikace podporovat. Tato úroveň rozhraní API se používá v *spustit* čas Android.

-   [Cílová verze Androidu](#target) &ndash; Určuje verzi Androidu, která vaše aplikace je určena pro spuštění v. Tato úroveň rozhraní API se používá v *spustit* čas Android.

Než budete moct nakonfigurovat úroveň rozhraní API pro váš projekt, je nutné nainstalovat součásti sady SDK platformy pro tuto úroveň rozhraní API. Další informace o stažení a instalaci součásti sady Android SDK najdete v tématu [instalace sady Android SDK](~/android/get-started/installation/android-sdk.md).

> [!NOTE]
> Od srpna 2018, konzolu Google Play bude vyžadovat, aby nové aplikace Cílová úroveň rozhraní API 26 (Android 8.0) nebo vyšší.
Existující aplikace bude muset Cílová úroveň rozhraní API 26 nebo vyšší od listopadu 2018. Další informace najdete v tématu [zlepšení aplikace zabezpečení a výkon na webu Google Play let pocházet](https://android-developers.googleblog.com/2017/12/improving-app-security-and-performance.html).

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Za normálních okolností jsou všechny tři úrovně rozhraní API Xamarin.Android nastavené na stejnou hodnotu. Na **aplikace** nastavte **zkompilovat pomocí verze Androidu (Cílová architektura)** na nejnovější stabilní verzi rozhraní API (nebo minimálně na verzi Androidu, který obsahuje všechny funkce, které potřebujete).
Na následujícím snímku obrazovky cílové rozhraní nastaveno **Android 7.1 (API úrovně 25 - verzi Nougat)**:

[![Cílové rozhraní Framework výchozí hodnota je verze zkompilovat pomocí verze Androidu](android-api-levels-images/vs-defaults-sml.png)](android-api-levels-images/vs-defaults.png#lightbox)

Na **Manifest v Androidu** nastavte minimální verzi systému Android **použití zkompilovat pomocí verze sady SDK** a nastaven cílovou verzi systému Android na stejnou hodnotu jako verze rozhraní .NET Framework (v následujícím snímek obrazovky, Cílová architektura pro Android je nastavena na **Android 7.1 (verzi Nougat)**):

[![Nastavte minimální a cílové Android verze na verzi rozhraní .NET Framework](android-api-levels-images/vs-manifest-defaults-sml.png)](android-api-levels-images/vs-manifest-defaults.png#lightbox)

Pokud chcete pro zachování zpětné kompatibility se starší verzí androidu, nastavte **minimální verzi systému Android k cíli** nejstarší verzi systému Android má vaše aplikace podporovat. (Všimněte si, že 14 úroveň rozhraní API je minimální úroveň rozhraní API požadovaná pro [služby Google Play a služby Firebase podporu](https://android-developers.googleblog.com/2016/11/google-play-services-and-firebase-for-android-will-support-api-level-14-at-minimum.html).) Následující příklad konfigurace podporuje Android verze z 14 úroveň rozhraní API do rozhraní API úrovně 25:

[![Zkompilovat pomocí rozhraní API úrovně 25 verzi Nougat minimální verzi systému Android nastaven na úroveň rozhraní API 14](android-api-levels-images/vs-minimum-sml.png)](android-api-levels-images/vs-minimum.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

Za normálních okolností jsou všechny tři úrovně rozhraní API Xamarin.Android nastavené na stejnou hodnotu. Nastavte **Cílová architektura** na nejnovější stabilní verzi rozhraní API (nebo minimálně na verzi Androidu, který obsahuje všechny funkce, které potřebujete). Chcete-li nastavit **Cílová architektura**, přejděte na **sestavení > Obecné** v **možnosti projektu**. Na následujícím snímku obrazovky cílové rozhraní nastaveno **použít poslední nainstalovanou platformu (8.0)**:

[![Cílová architektura jako výchozí se použije použít poslední nainstalovanou platformu](android-api-levels-images/xs-default-target-sml.png)](android-api-levels-images/xs-default-target.png#lightbox)

Nastavení minimální a cílové Android verze najdete v části **sestavení > aplikace pro Android** v **možnosti projektu**. Nastavte minimální verzi systému Android na **automaticky – použít cílovou verzi architektury** a nastaven cílovou verzi systému Android na stejnou hodnotu jako verze rozhraní .NET Framework. Na následujícím snímku obrazovky, Cílová architektura Android nastavená na **Android 8.0 (úroveň rozhraní API 26)** tak, aby odpovídala nastavení cílové architektury výše:

[![Nastavení úrovně cíle a framework v možnostech projektu](android-api-levels-images/xs-default-app-sml.png)](android-api-levels-images/xs-default-app.png#lightbox)

Pokud chcete zachovat zpětné kompatibility se starší verzí androidu, změňte **minimální verzi systému Android** nejstarší verzi systému Android má vaše aplikace podporovat. Všimněte si, že 14 úroveň rozhraní API je minimální úroveň rozhraní API požadovaná pro [služby Google Play a služby Firebase podporu](https://android-developers.googleblog.com/2016/11/google-play-services-and-firebase-for-android-will-support-api-level-14-at-minimum.html).
Například následující konfigurace podporuje napravovat je co 14 úroveň rozhraní API verze Androidu:

[![Minimální a cílové verzi nastavená na hodnotu automaticky – použít cílovou verzi rozhraní framework](android-api-levels-images/xs-minimum-sml.png)](android-api-levels-images/xs-minimum.png#lightbox)

-----


Pokud vaše aplikace podporuje více verzí Androidu, váš kód musí obsahovat kontroly za běhu k zajištění, že vaše aplikace funguje se nastavení minimální verze Androidu (viz [modul Runtime vyhledává verze Androidu](#runtimechecks) níže podrobnosti). Pokud jsou zpracovávání a vytvoření knihovny, přečtěte si téma [úrovně rozhraní API a knihovnami](#libraries) níže osvědčených postupech při konfiguraci rozhraní API pro úroveň nastavení pro knihovny.



## <a name="android-versions-and-api-levels"></a>Verze androidu a úrovně rozhraní API

S rozvojem platformy Android a se vydávají nové verze Androidu, každé verze Androidu je přiřazené jedinečné celé identifikátor, volá se, *úroveň rozhraní API*. Proto každé verze Androidu odpovídá jedné Android úroveň rozhraní API. Protože uživatelé nainstalují aplikace na starší stejně jako poslední verze systému Android, reálné aplikace pro Android musí navrženy pro práci s více úrovní rozhraní API systému Android.


### <a name="android-versions"></a>Verze androidu

Každá verze Android přejde podle více názvů:

-   Verze Androidu, jako například **Android 7.1**
-   A kód název, jako například _verzi Nougat_
-   Odpovídající rozhraní API na úrovni, jako například **úroveň rozhraní API 25**

Android kódový název může odpovídat více verzí a úrovně rozhraní API (jak je vidět v níže uvedeném seznamu), ale každá verze Androidu odpovídá přesně jednu úroveň rozhraní API.

Kromě toho Xamarin.Android definuje *sestavení verze kódy* , která mapují na aktuálně známé úrovně rozhraní Android API. V následujícím seznamu můžete převod mezi úroveň rozhraní API, verze Androidu, s kódovým názvem a kód sestavení verze Xamarin.Android.

-   **Rozhraní API 27 (Android 8.1)** &ndash; _Oreo_, kterou jsme vydali. prosince 2017. Sestavujte kód verze `Android.OS.BuildVersionCodes.OMr1`

-   **Rozhraní API 26 (Android 8.0)** &ndash; _Oreo_, kterou jsme vydali. srpna 2017. Sestavujte kód verze `Android.OS.BuildVersionCodes.O`

-   **Rozhraní API 25 (Android 7.1)** &ndash; _verzi Nougat_, kterou jsme vydali. prosince 2016. Sestavujte kód verze `Android.OS.BuildVersionCodes.NMr1`

-   **Rozhraní API 24 (Android 7.0)** &ndash; _verzi Nougat_, kterou jsme vydali. srpna 2016. Sestavujte kód verze `Android.OS.BuildVersionCodes.N`

-   **Rozhraní API 23 (Android 6.0)** &ndash; _Marshmallow_, kterou jsme vydali. srpna 2015. Sestavujte kód verze `Android.OS.BuildVersionCodes.M`

-   **Rozhraní API 22 (Android 5.1)** &ndash; _Lollipop_, kterou jsme vydali. března 2015. Sestavujte kód verze `Android.OS.BuildVersionCodes.LollipopMr1`

-   **Rozhraní API 21 (Android 5.0)** &ndash; _Lollipop_, vydáno v listopadu 2014. Sestavujte kód verze `Android.OS.BuildVersionCodes.Lollipop`

-   **Rozhraní API 20 (Android 4.4W)** &ndash; _Kitkat Watch_, kterou jsme vydali. června 2014. Sestavujte kód verze `Android.OS.BuildVersionCodes.KitKatWatch`

-   **Rozhraní API 19 (Android 4.4)** &ndash; _Kitkat_, kterou jsme vydali. října 2013. Sestavujte kód verze `Android.OS.BuildVersionCodes.KitKat`

-   **Rozhraní API 18 (Android verze 4.3)** &ndash; _položku Jelly Bean_, kterou jsme vydali červenec 2013. Sestavujte kód verze `Android.OS.BuildVersionCodes.JellyBeanMr2`

-   **Rozhraní API 17 (Android 4.2-4.2.2)** &ndash; _položku Jelly Bean_, vydáno v listopadu 2012. Sestavujte kód verze `Android.OS.BuildVersionCodes.JellyBeanMr1`

-   **Rozhraní API 16 (Android 4.1 4.1.1)** &ndash; _položku Jelly Bean_, kterou jsme vydali z června 2012. Sestavujte kód verze `Android.OS.BuildVersionCodes.JellyBean`

-   **Rozhraní API 15 (Android 4.0.3-4.0.4)** &ndash; _Ice Cream Sandwichovy_, kterou jsme vydali prosince 2011. Sestavujte kód verze `Android.OS.BuildVersionCodes.IceCreamSandwichMr1`

-   **Rozhraní API 14 (Android 4.0 – 4.0.2)** &ndash; _Ice Cream Sandwichovy_, kterou jsme vydali října 2011. Sestavujte kód verze `Android.OS.BuildVersionCodes.IceCreamSandwich`

-   **Rozhraní API 13 (Android 3.2)** &ndash; _Voštinový_, kterou jsme vydali 2011 dne. Sestavujte kód verze `Android.OS.BuildVersionCodes.HoneyCombMr2`

-   **Rozhraní API 12 (Android 3.1.x)** &ndash; _Voštinový_, kterou jsme vydali 2011 dne. Sestavujte kód verze `Android.OS.BuildVersionCodes.HoneyCombMr1`

-   **Rozhraní API 11 (Android 3.0.x)** &ndash; _Voštinový_, kterou jsme vydali. února 2011. Sestavujte kód verze `Android.OS.BuildVersionCodes.HoneyComb`

-   **Rozhraní API 10 (Android 2.3.3-2.3.4)** &ndash; _Perník_, kterou jsme vydali. února 2011. Sestavujte kód verze `Android.OS.BuildVersionCodes.GingerBreadMr1`

-   **Rozhraní API 9 (Android 2.3 2.3.2)** &ndash; _Perník_, kterou jsme vydali Listopad 2010. Sestavujte kód verze `Android.OS.BuildVersionCodes.GingerBread`

-   **Rozhraní API 8 (Android 2.2.x)** &ndash; _Froyo_, kterou jsme vydali červen 2010. Sestavujte kód verze `Android.OS.BuildVersionCodes.Froyo`

-   **Rozhraní API 7 (Android 2.1.x)** &ndash; _Eclair_, kterou jsme vydali od 2010. Sestavujte kód verze `Android.OS.BuildVersionCodes.EclairMr1`

-   **Rozhraní API 6 (s Androidem 2.0.1)** &ndash; _Eclair_, kterou jsme vydali prosince 2009. Sestavujte kód verze `Android.OS.BuildVersionCodes.Eclair01`

-   **Rozhraní API 5 (Android 2.0)** &ndash; _Eclair_, kterou jsme vydali listopadem 2009. Sestavujte kód verze `Android.OS.BuildVersionCodes.Eclair`

-   **Rozhraní API 4 (Android 1.6)** &ndash; _prstencový_, vydáno v září 2009. Sestavujte kód verze `Android.OS.BuildVersionCodes.Donut`

-   **Rozhraní API 3 (Android 1.5)** &ndash; _Cupcake_, kterou jsme vydali květen 2009. Sestavujte kód verze `Android.OS.BuildVersionCodes.Cupcake`

-   **Rozhraní API 2 (Android 1.1)** &ndash; _Base_, kterou jsme vydali únor 2009. Sestavujte kód verze `Android.OS.BuildVersionCodes.Base11`

-   **Rozhraní API 1 (Android 1.0)** &ndash; _Base_, vydáno v říjnu 2008. Sestavujte kód verze `Android.OS.BuildVersionCodes.Base`


Podle tohoto seznamu znamená, nová verze Androidu se vydávají často &ndash; někdy několik verzí za rok. V důsledku toho zahrnuje celou řadu zařízení s Androidem, které může být spuštění aplikace z široké škály starší a novějších verzí Androidu. Jak může zaručit, že vaše aplikace poběží konzistentně a spolehlivě na mnoho různých verzích Android? Úrovně rozhraní API pro Android můžete spravovat tento problém.


### <a name="android-api-levels"></a>Úrovně rozhraní Android API

Každé zařízení s Androidem běží na přesně *jeden* úroveň rozhraní API &ndash; tuto úroveň rozhraní API se musí být jedinečný na verzi platformy Android. Úroveň rozhraní API identifikuje přesně verzi sady rozhraní API, která vaše aplikace může volat do; Určuje kombinaci elementy manifestu, oprávnění atd tento můžete kód proti jako vývojář. Systém pro Android z úrovně rozhraní API pomáhá zjistit, jestli je aplikace kompatibilní s bitovou kopii systému Android před instalací aplikace na zařízení s Androidem.

Když je aplikace sestavená, obsahuje následující informace o úrovni rozhraní API:

-   *Cílové* úroveň rozhraní API systému Android, která je integrovaná aplikace pro spuštění na.

-   *Minimální* rozhraní Android API úrovně, které zařízení s Androidem musí mít ke spouštění vaší aplikace. 

Toto nastavení slouží k zajištění, že funkce potřebných ke spuštění aplikace správně jsou k dispozici na zařízení s Androidem v době instalace. V opačném případě se blokuje aplikace spuštěné na daném zařízení. Například pokud úroveň rozhraní API zařízení s Androidem je nižší než minimální úroveň rozhraní API, který zadáte pro vaše aplikace, zařízení s Androidem zabrání uživateli instalaci vaší aplikace.


## <a name="project-api-level-settings"></a>Nastavení projektu úroveň rozhraní API

Následující části popisují způsob použití sady SDK Manager Příprava vývojového prostředí pro úrovně rozhraní API, kterou chcete zacílit, za nímž následuje podrobné vysvětlení, jak nakonfigurovat *Cílová architektura*, *Minimum Verze androidu*, a *cílovou verzi systému Android* nastavení v Xamarin.Android.


### <a name="android-sdk-platforms"></a>Sady Android SDK platformy

Cíl nebo rozhraní API minimální úroveň můžete vybrat v Xamarin.Android, musíte nainstalovat verzi sady Android SDK platformy, která odpovídá na této úrovni rozhraní API. Řadu možností pro cílovou architekturu, minimální verzi systému Android a cílovou verzi systému Android je omezená na oblasti sady Android SDK verze, které jste nainstalovali. Správce sady SDK můžete ověřit, že jsou nainstalovány požadované verze sady Android SDK, a slouží k přidávání nových úrovní rozhraní API, které potřebujete pro svou aplikaci. Pokud nejste obeznámeni s instalace úrovně rozhraní API, přečtěte si téma [instalace sady Android SDK](~/android/get-started/installation/android-sdk.md).

<a name="framework" />

### <a name="target-framework"></a>Cílová architektura

*Cílová architektura* (označované také jako `compileSdkVersion`) je konkrétní verze architektury Android (úroveň rozhraní API), který vaše aplikace je zkompilován pro v okamžiku sestavení. Toto nastavení určuje aplikace, rozhraní API, jejichž *očekává, že* při spuštění, ale nemá žádný vliv, na kterém rozhraní API jsou ve skutečnosti k dispozici do vaší aplikace, po instalaci. V důsledku toho změnou nastavení cílové architektury nezmění chování za běhu.

Cílová architektura, která identifikuje knihovny verze vaší aplikace je propojená s &ndash; Určuje, které rozhraní API můžete použít ve vaší aplikaci. Například, pokud chcete použít [NotificationBuilder.SetCategory](https://developer.xamarin.com/api/member/Android.App.Notification+Builder.SetCategory/p/System.String/) metodě, která byla zavedena v Androidu 5.0 Lollipop, je nutné nastavit cílovou architekturu na **API úroveň 21 (Lollipop)** nebo novější. Pokud nastavíte cílové rozhraní projektu na rozhraní API úrovně, jako **úroveň API 19 (KitKat)** a zkuste volat `SetCategory` metodě v kódu, zobrazí se chyba při kompilaci.

Doporučujeme vám, že vždy kompilujete s *nejnovější* dostupná verze rozhraní .NET Framework. To vám poskytne užitečné zprávy upozornění pro všechny zastaralé rozhraní API, které může volat v kódu. Pomocí nejnovější verze rozhraní .NET Framework je zvlášť důležité při použití nejnovější verze knihovny podpory &ndash; každou knihovnu očekává, že vaše aplikace na kompilované na tuto knihovnu podpory minimální úroveň rozhraní API nebo vyšší. 


# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Pro přístup k nastavení cílové architektury v sadě Visual Studio, otevřete vlastnosti projektu v **Průzkumníka řešení** a vyberte **aplikace** stránky:

[![Aplikace na stránce vlastností projektu](android-api-levels-images/vs-target-framework-sml.png)](android-api-levels-images/vs-target-framework.png#lightbox)

Nastavte cílovou architekturu výběrem úroveň rozhraní API v rozevírací nabídce v části **zkompilovat pomocí verze Androidu** jak je znázorněno výše.

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

Pro přístup k nastavení cílové architektury v sadě Visual Studio pro Mac, klikněte pravým tlačítkem na název projektu a vyberte **možnosti**; tento otevře **možnosti projektu** dialogového okna. V tomto dialogovém okně, přejděte na **sestavení > Obecné** jak je znázorněno zde:

[![Vytvoření části Obecné na stránce Možnosti projektu](android-api-levels-images/xs-target-framework-sml.png)](android-api-levels-images/xs-target-framework.png#lightbox)

Nastavte cílovou architekturu výběrem úroveň rozhraní API v rozevírací nabídce napravo od **Cílová architektura** jak je znázorněno výše.

-----


<a name="minimum" />

### <a name="minimum-android-version"></a>Minimální verze Androidu

*Minimální verzi systému Android* (označované také jako `minSdkVersion`) je nejstarší verzi operačního systému Android (to znamená nejnižší úroveň rozhraní API), který můžete nainstalovat a spustit aplikaci. Ve výchozím nastavení aplikace může obsahovat jenom nainstalované na zařízeních odpovídající nastavení cílové architektury nebo vyšší. Pokud nastavení minimální verze Androidu je *nižší* než nastavení cílové rozhraní Framework vaší aplikace můžete spustit také na starší verze systému Android. Například nastavte cílovou architekturu **Android 7.1 (verzi Nougat)** a nastavte minimální verzi systému Android **Android 4.0.3 (Ice Cream Sandwichovy)**, vaše aplikace dá nainstalovat na libovolné platformě z rozhraní API úrovně 15 úroveň rozhraní API 25 (včetně).

I když vaše aplikace může úspěšně sestavit a nainstalovat tuto řadu platforem, to není zaručeno, že se úspěšně *spustit* ve všech těchto platformách. Například, pokud vaše aplikace je nainstalovaná na **Android 5.0 (Lollipop)** a váš kód volá rozhraní API, která je dostupná jenom v **Android 7.1 (verzi Nougat)** a novější, vaše aplikace se zobrazí chyba modulu runtime a potenciálně dojít k chybě. Proto musíte zajistit kódu &ndash; za běhu &ndash; volá pouze API, které jsou podporovány, která běží na zařízení s Androidem. Jinými slovy váš kód musí obsahovat explicitní runtime kontroly pro zajištění, že vaše aplikace používá pouze na zařízeních, která jsou aktuální, a proto pro jejich podporu novějších rozhraní API.
[Modul runtime vyhledává verze Androidu](#runtimechecks)dál v této příručce se vysvětluje, jak přidat tyto kontroly za běhu kódu.


# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Pro přístup k nastavení minimální verze Androidu v sadě Visual Studio, otevřete vlastnosti projektu v **Průzkumníka řešení** a vyberte **Manifest v Androidu** stránky. V rozevírací nabídce v části **minimální verzi systému Android** minimální verzi systému Android můžete vybrat pro vaši aplikaci:

[![Minimální verze systému Android k cílové možnosti zkompilovat pomocí verze sady SDK](android-api-levels-images/vs-minimum-version-sml.png)](android-api-levels-images/vs-minimum-version.png#lightbox)

Pokud vyberete **použití zkompilovat pomocí verze sady SDK**, minimální Android verze bude stejné jako nastavení cílové architektury.

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

Pro přístup k minimální verzi systému Android v sadě Visual Studio pro Mac, klikněte pravým tlačítkem na název projektu a vyberte **možnosti**; tento otevře **možnosti projektu** dialogového okna. Přejděte do **sestavení > aplikace pro Android**.
Pomocí rozevírací nabídky napravo od **minimální verzi systému Android**, můžete nastavit minimální verzi systému Android pro vaši aplikaci:

[![Minimální verze Androidu nastavená na hodnotu automaticky – použít cílovou verzi architektury](android-api-levels-images/xs-minimum-version-sml.png)](android-api-levels-images/xs-minimum-version.png#lightbox)

Pokud vyberete **automatické &ndash; použít cílovou verzi rozhraní framework**, minimální Android verze bude stejné jako nastavení cílové architektury.

-----


<a name="target" />

### <a name="target-android-version"></a>Cílová verze Androidu

*Cílová verze Androidu* (označované také jako `targetSdkVersion`) je rozhraní API na úrovni zařízení s Androidem, kde se očekává, že aplikaci spustit. Pomocí tohoto nastavení Android Určuje, jestli chcete povolit nějaká chování kompatibility &ndash; tím se zajistí, že vaše aplikace i nadále fungovat očekávaným způsobem. Android používá nastavení cílové Android verze vaší aplikace zjistit, jaké změny chování lze použít u vaší aplikace bez narušení ho (to je jak Android poskytuje dopřednou kompatibilitu).

Cílová architektura a cílovou verzi systému Android, přitom má velmi podobné názvy nejsou totéž. Nastavení cílové architektury komunikuje cílové rozhraní API úrovně informace pro Xamarin.Android pro použití v *kompilace*, zatímco cílovou verzi systému Android, komunikuje cílové rozhraní API úrovně informace do systému Android pro použití v  *doba běhu* (Pokud aplikace je nainstalovaná a spuštěná na zařízení).

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Pro přístup k toto nastavení v sadě Visual Studio, otevřete vlastnosti projektu v **Průzkumníka řešení** a vyberte **Manifest v Androidu** stránky. V rozevírací nabídce v části **cílovou verzi systému Android** cílovou verzi systému Android můžete vybrat pro vaši aplikaci:

[![Cílová verze Androidu nastavte na zkompilovat pomocí verze sady SDK](android-api-levels-images/vs-target-version-sml.png)](android-api-levels-images/vs-target-version.png#lightbox)

Doporučujeme explicitně nastavit cílovou verzi systému Android na nejnovější verzi Androidu, který použijete k testování vaší aplikace. V ideálním případě by měla být nastavena na nejnovější verzi sady Android SDK &ndash; díky tomu můžete používat nová rozhraní API před projdete změny chování. Pro většinu vývojářů jsme *nejsou* doporučujeme nastavení cílovou verzi systému Android **použití zkompilovat pomocí verze sady SDK**.

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

Pro přístup k toto nastavení v sadě Visual Studio pro Mac, klikněte pravým tlačítkem na název projektu a vyberte **možnosti**; tento otevře **možnosti projektu** dialogové okno. Přejděte do **sestavení > aplikace pro Android**. Pomocí rozevírací nabídky napravo od **cílovou verzi systému Android**, můžete nastavit cílovou verzi systému Android pro vaši aplikaci:

[![Cílová verze Androidu nastavená na hodnotu automaticky – použít cílovou verzi architektury](android-api-levels-images/xs-target-version-sml.png)](android-api-levels-images/xs-target-version.png#lightbox)

Doporučujeme explicitně nastavit cílovou verzi systému Android na nejnovější verzi Androidu, který použijete k testování vaší aplikace. V ideálním případě by měla být nastavena na nejnovější dostupné verzi sady Android SDK &ndash; díky tomu můžete používat nová rozhraní API před projdete změny chování. Pro většinu vývojářů, nedoporučujeme nastavení cílovou verzi systému Android **automaticky – použít cílovou verzi architektury**.

-----

Cílová verze Androidu musí být obecně ohraničené minimální verze Androidu a cílovou architekturu. To je:

**Minimální verze Androidu < = cílová verze Androidu < = Cílová architektura**

Další informace o úrovních SDK najdete v části Android Developer [sdk používá](https://developer.android.com/guide/topics/manifest/uses-sdk-element.html) dokumentaci.


<a name="runtimechecks" />

## <a name="runtime-checks-for-android-versions"></a>Kontroly za běhu pro verze Androidu

Každá nová verze systému Android je vydané, rozhraní API je aktualizované tak, aby nové nebo nahrazení funkce. S několika výjimkami funkce rozhraní API z dřívější verze Androidu se přenášejí do novější verze Androidu bez úprav. Proto pokud vaše aplikace běží na určitou úroveň rozhraní Android API, obvykle bude možné spouštět na novější rozhraní Android API úrovně bez úprav. Ale co když chcete ke spouštění vaší aplikace ve starších verzích Android?

Pokud vyberete minimální verzi systému Android, která je *nižší* než nastavení cílové architektury, nemusí být některá rozhraní API dostupné pro vaši aplikaci za běhu. Vaše aplikace však stále můžete spustit na starších zařízení, ale s omezenou funkčností. Pro každé rozhraní API, která není k dispozici na Androidu platformách odpovídající vaše nastavení minimální verze Androidu, musí váš kód explicitně kontrolu hodnoty `Android.OS.Build.VERSION.SdkInt` vlastnosti k určení úrovně rozhraní API platformy je aplikace spuštěná na. Pokud je úroveň rozhraní API *nižší* než minimální verzi systému Android, která podporuje rozhraní API, kterou chcete volat a pak váš kód musí najít způsob, jak správně fungovat bez provedení toto volání rozhraní API.

Například předpokládejme, že chceme použít [NotificationBuilder.SetCategory](https://developer.xamarin.com/api/member/Android.App.Notification+Builder.SetCategory/p/System.String/) metoda ke kategorizaci oznámení, když se používá **Androidu 5.0 Lollipop** (a novější), ale stále chceme naši aplikaci spuštění ve starších verzích Android například **položku s Androidem 4.1 Jelly Bean** (kde `SetCategory` není k dispozici). Odkazující na tabulku verzí Androidu na začátku tohoto průvodce, vidíme, že kód verze sestavení pro **Androidu 5.0 Lollipop** je `Android.OS.BuildVersionCodes.Lollipop`. Pro podporu starších verzí Androidu where `SetCategory` není k dispozici, náš kód můžete zjišťovat úroveň rozhraní API za běhu a podmíněně volání `SetCategory` pouze v případě úroveň rozhraní API je větší než nebo rovna hodnotě kód verze Lollipop sestavení:

```csharp
if (Android.OS.Build.VERSION.SdkInt >= Android.OS.BuildVersionCodes.Lollipop)
{
    builder.SetCategory(Notification.CategoryEmail);
}
```

V tomto příkladu naši aplikaci cílové rozhraní nastaveno **Android 5.0 (API úroveň 21)** a její minimální verzi systému Android je nastavená na **Android 4.1 (rozhraní API Level 16)**. Protože `SetCategory` je k dispozici v rozhraní API úrovně `Android.OS.BuildVersionCodes.Lollipop` a později, bude tento příklad kódu volat `SetCategory` pouze pokud je ve skutečnosti k dispozici &ndash; bude *není* pokus o volání `SetCategory` při rozhraní API úroveň je 16 17, 18, 19 a 20. Funkce se sníží na tyto starší verze Androidu pouze v rozsahu, který oznámení nejsou seřazené správně (vzhledem k tomu, že nejsou rozděleny podle typu), ale tato oznámení jsou stále publikovány k upozornění uživatele. Naše aplikace stále funguje, ale jeho funkcí je o něco snížila.

Kontrola verze sestavení obecně platí, pomáhá rozhodnout mezi něco nový způsob porovnání starý způsob běhu kódu. Příklad:

```csharp
if (Android.OS.Build.VERSION.SdkInt >= Android.OS.BuildVersionCodes.Lollipop)
{
    // Do things the Lollipop way
}
else
{
    // Do things the pre-Lollipop way
}
```

Neexistuje žádné rychlé a jednoduché pravidlo, které vysvětluje, jak snížit nebo změnit funkce vaší aplikace při spuštění ve starších verzích, které chybí jeden nebo více rozhraní API. V některých případech (například `SetCategory` výše uvedeném příkladu), stačí jednoduše vynechejte volání rozhraní API, když není k dispozici. Ale v jiných případech budete muset implementovat alternativní funkce pro případ `Android.OS.Build.VERSION.SdkInt` zjistí být menší než rozhraní API úrovně, že vaše aplikace potřebuje zobrazíte jeho optimální prostředí.

<a name="libraries" />

## <a name="api-levels-and-libraries"></a>Úrovně rozhraní API a knihovny

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Když vytvoříte projekt Xamarin.Android knihovny (například knihovny tříd nebo knihovna vazeb), můžete nakonfigurovat nastavení cílové architektury &ndash; minimální verzi systému Android a nastavení pro cílový Android verze nejsou k dispozici. Důvodem je, že neexistuje žádná **Manifest v Androidu** stránky:

[![Zkompilovat pomocí verze Androidu možnost je k dispozici jen](android-api-levels-images/vs-library-options-sml.png)](android-api-levels-images/vs-library-options.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

Když vytvoříte projekt Xamarin.Android knihovny, je žádné **aplikace pro Android** stránku, kde můžete nakonfigurovat minimální verzi systému Android a cílovou verzi systému Android &ndash; minimální verzi systému Android a cíl Verze androidu nastavení nejsou k dispozici.
Důvodem je, že neexistuje žádná **sestavení > aplikace pro Android** stránky):

[![Hlavní stránka bez minimální a cílové verzi možností sestavení](android-api-levels-images/xs-library-options-sml.png)](android-api-levels-images/xs-library-options.png#lightbox)

-----

Minimální verzi systému Android a nastavení pro cílový Android verze nejsou k dispozici, protože výsledná knihovny není samostatné aplikace &ndash; knihovny by bylo možné spustit na libovolné verzi Androidu, v závislosti na aplikaci, která je součástí balíčku. Můžete určit, jak má být knihovně *zkompilován*, ale nelze předvídat, kterou platformu úroveň rozhraní API knihovny se používají ke spouštění. S myslete na to by měl následující osvědčené postupy při zpracovávání a vytváření knihoven:

-   **Při využívání knihovna pro Android** &ndash; Pokud knihovna pro Android spotřebovávají ve vaší aplikaci, je nutné nastavit cílové rozhraní vaší aplikace nastavení do rozhraní API úrovně *vysokou jako* cíl Nastavení rozhraní knihovny.

-   **Při vytváření knihovna pro Android** &ndash; knihovna pro Android pro použití při vytváření jiné aplikace, nezapomeňte nastavit jeho nastavení cílové architektury na minimální úroveň rozhraní API, vyžadující kompilaci.

Aby se zabránilo situaci, kdy knihovnu pokouší volat rozhraní API, která není k dispozici za běhu (který může způsobit pád aplikace) doporučují tyto osvědčené postupy. Pokud jste vývojář knihovny, je nutné snažit omezit využití volání rozhraní API pro malé a dobře zavedený podmnožinu celkového svrchní oblasti rozhraní API. To uděláte tak pomáhá zajistit, že vaši knihovnu je možné bezpečně přes širší rozsah Android verze.


## <a name="summary"></a>Souhrn

Tato příručka vysvětluje, jak rozhraní Android API úrovně se používají ke správě kompatibility aplikací v různých verzích Androidu. Je k dispozici podrobný postup pro konfiguraci Xamarin.Android *Cílová architektura*, *minimální verzi systému Android*, a *cílovou verzi systému Android* nastavení projektu. Je k dispozici pokyny k instalaci balíčků sady SDK, pomocí Android SDK Manageru zahrnuté příklady, jak napsat kód vypořádat s různými úrovněmi rozhraní API za běhu a bylo vysvětleno, jak spravovat úrovně rozhraní API při vytváření nebo používání knihoven s Androidem. Poskytuje také kompletní seznam, který se týká úrovně rozhraní API čísel verzí Androidu (jako je například Android 4.4), verze Androidu názvy (například Kitkat) a kódy verzí Xamarin.Android sestavení.


## <a name="related-links"></a>Související odkazy

- [Instalace sady Android SDK](~/android/get-started/installation/android-sdk.md)
- [Změny nástrojů sady SDK rozhraní příkazového řádku](~/android/troubleshooting/sdk-cli-tooling-changes.md)
- [Výběr compileSdkVersion, minSdkVersion a targetSdkVersion](https://medium.com/google-developers/picking-your-compilesdkversion-minsdkversion-targetsdkversion-a098a0341ebd)
- [Co je úroveň rozhraní API?](http://developer.android.com/guide/topics/manifest/uses-sdk-element.html#ApiLevels)
- [Codenames, značky a čísla sestavení](https://source.android.com/source/build-numbers)
