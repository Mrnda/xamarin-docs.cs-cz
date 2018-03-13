---
title: "Principy úrovní rozhraní API systému Android"
description: "Xamarin.Android má několik nastavení úrovně rozhraní API systému Android, které určete kompatibilitu aplikace s více verzemi systému Android. Tato příručka vysvětluje, co toto nastavení znamená, způsob jejich konfigurace a jaký vliv mají vaše aplikace za běhu."
ms.topic: article
ms.prod: xamarin
ms.assetid: 58CB7B34-3140-4BEB-BE2E-209928C1878C
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 03/01/2018
ms.openlocfilehash: 907af0948e9d081f05cc201c49f94629a513c935
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 03/09/2018
---
# <a name="understanding-android-api-levels"></a>Principy úrovní rozhraní API systému Android

_Xamarin.Android má několik nastavení úrovně rozhraní API systému Android, které určete kompatibilitu aplikace s více verzemi systému Android. Tato příručka vysvětluje, co toto nastavení znamená, způsob jejich konfigurace a jaký vliv mají vaše aplikace za běhu._


## <a name="quick-start"></a>Rychlý Start

Xamarin.Android zpřístupní tři nastavení úrovně projektu rozhraní API systému Android:

-   [Cíl Frameworku](#framework) &ndash; Určuje, které framework použít při vytváření aplikace. Tato úroveň rozhraní API se používá v *zkompilovat* čas podle Xamarin.Android.

-   [Minimální verze Android](#minimum) &ndash; určuje nejstarší verzi systému Android, které chcete aplikaci, kterou chcete podporovat. Tato úroveň rozhraní API se používá v *spustit* čas Android.

-   [Cílová verze Android](#target) &ndash; Určuje verzi Androidu, který vaše aplikace je určená ke spuštění na. Tato úroveň rozhraní API se používá v *spustit* čas Android.

Než budete moct nakonfigurovat úroveň rozhraní API pro svůj projekt, musíte nainstalovat komponenty SDK platformy pro tuto úroveň rozhraní API. Další informace o stahování a instalaci součástí sady SDK pro Android, najdete v části [Android SDK instalace](~/android/get-started/installation/android-sdk.md).

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Za normálních okolností jsou všechny tři úrovně rozhraní API Xamarin.Android nastavené na stejnou hodnotu. Na **aplikace** nastavte **zkompilovat pomocí verzi systému Android (cílový Framework)** nejnovější stabilní verzi rozhraní API (nebo minimálně na verzi systému Android, která obsahuje všechny funkce, které potřebujete).
Na následujícím snímku obrazovky rozhraní Target Framework nastavena na **Android 7.1 (API úrovně 25 – cukrovinkách typu nugát)**:

[![Cíl Framework výchozí hodnota je verze kompilace pomocí verzi systému Android](android-api-levels-images/vs-defaults-sml.png)](android-api-levels-images/vs-defaults.png#lightbox)

Na **Android Manifest** nastavte minimální verzi systému Android **použití zkompilovat pomocí sady SDK verze** a nastavte verzi systému Android cíl na stejnou hodnotu jako je verze cílové rozhraní (v následujícím snímek obrazovky, cílové rozhraní Android je nastaven na **Android 7.1 (cukrovinkách typu nugát)**):

[![Minimální a cílové Android verze nastavte na verzi cílové rozhraní](android-api-levels-images/vs-manifest-defaults-sml.png)](android-api-levels-images/vs-manifest-defaults.png#lightbox)

Pokud chcete pro zachování zpětné kompatibility se starší verzí systému Android, nastavte **minimální verzi systému Android k cíli** nejstarší verzi Androidu, které chcete aplikaci, kterou chcete podporovat. (Upozorňujeme, že 14 úroveň rozhraní API je minimální úroveň API požadované pro [služby Google Play a podporu Firebase](https://android-developers.googleblog.com/2016/11/google-play-services-and-firebase-for-android-will-support-api-level-14-at-minimum.html).) Následující příklad konfigurace podporuje verze Android API 14 úroveň přispívají úroveň rozhraní API 25:

[![Kompilace pomocí rozhraní API úroveň 25 cukrovinkách typu nugát, minimální verzi systému Android nastavit na úrovni rozhraní API 14](android-api-levels-images/vs-minimum-sml.png)](android-api-levels-images/vs-minimum.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

Za normálních okolností jsou všechny tři úrovně rozhraní API Xamarin.Android nastavené na stejnou hodnotu. Nastavit **cílové rozhraní** nejnovější stabilní verzi rozhraní API (nebo minimálně na verzi systému Android, která obsahuje všechny funkce, které potřebujete). Chcete-li nastavit **cílové rozhraní**, přejděte na **sestavení > Obecné** v **možnosti projektu**. Na následujícím snímku obrazovky rozhraní Target Framework nastavena na **použít nejnovější nainstalované platformy (8.0)**:

[![Cílové rozhraní, jako výchozí bude použit pro použití nejnovější nainstalované platformu](android-api-levels-images/xs-default-target-sml.png)](android-api-levels-images/xs-default-target.png#lightbox)

Nastavení minimální a cílové Android verze naleznete v části **sestavení > aplikace pro Android** v **možnosti projektu**. Nastavte minimální Android verzi **automatické - použití cílová verze framework** a nastavte verzi systému Android cíl na stejnou hodnotu jako verzi rozhraní Target Framework. Na následujícím snímku obrazovky cílové rozhraní Android nastavena na **Android 8.0 (API úrovně 26)** tak, aby odpovídaly nastavení cílové rozhraní výše:

[![Nastavení úrovně cíle a framework v možnosti projektu](android-api-levels-images/xs-default-app-sml.png)](android-api-levels-images/xs-default-app.png#lightbox)

Pokud chcete zachovat zpětné kompatibility se starší verzí systému Android, změňte **minimální verzi systému Android** nejstarší verzi Androidu, které chcete aplikaci, kterou chcete podporovat. Upozorňujeme, že 14 úroveň rozhraní API je minimální úroveň API požadované pro [služby Google Play a podporu Firebase](https://android-developers.googleblog.com/2016/11/google-play-services-and-firebase-for-android-will-support-api-level-14-at-minimum.html).
Například následující konfigurace podporuje časná 14 úroveň rozhraní API systému Android verze:

[![Minimální a cílové verze nastavená na hodnotu automaticky - používají cílová verze framework](android-api-levels-images/xs-minimum-sml.png)](android-api-levels-images/xs-minimum.png#lightbox)

-----


Pokud vaše aplikace podporuje několik verzí systému Android, kódu musí obsahovat runtime kontroly, aby se zajistilo, že aplikace funguje s nastavením minimální Android verze (najdete v části [Runtime zkontroluje, zda verze Android](#runtimechecks) níže podrobnosti). Pokud se využívají nebo vytvoření knihovny, přečtěte si téma [úrovně rozhraní API a knihovny](#libraries) pod úroveň pro osvědčené postupy v rozhraní API pro konfiguraci nastavení pro knihovny.



## <a name="android-versions-and-api-levels"></a>Android verze a úrovně rozhraní API

Jako platformu Android zpracovaní a jsou vydávány nové verze Android, každou verzi systému Android je přiřazen jedinečný celé číslo identifikátor s názvem *úroveň rozhraní API*. Proto každou verzi systému Android odpovídá jedné úrovni rozhraní API systému Android. Protože uživatelé nainstalují aplikace ve starších verzích také jako poslední Android, musí být aplikace pro Android reálného navrženy pro práci s více úrovní rozhraní API systému Android.


### <a name="android-versions"></a>Android verze

Každé verzi systému Android přejde podle více názvů:

-   Verzi systému Android, jako například **Android 7.1**
-   A kód název, například _cukrovinkách typu nugát_
-   Odpovídající rozhraní API úrovně, jako například **úroveň rozhraní API 25**

Android kódový název může odpovídat více verzí a úrovně rozhraní API (jak je vidět v následujícím seznamu), ale každý Android verze odpovídá přesně jednu úroveň rozhraní API.

Kromě toho Xamarin.Android definuje *sestavení verze kódy* , mapování na aktuálně známé úrovně rozhraní API systému Android. V následujícím seznamu můžete převod mezi úroveň rozhraní API, verzi systému Android, název kód a kód verze sestavení Xamarin.Android.

-   **Rozhraní API 27 (Android 8.1)** &ndash; _Oreo_, vydaná prosinec 2017. Verze kódu sestavení `Android.OS.BuildVersionCodes.OMr1`

-   **Rozhraní API 26 (Android 8.0)** &ndash; _Oreo_, vydaná 2017 srpnu. Verze kódu sestavení `Android.OS.BuildVersionCodes.O`

-   **Rozhraní API 25 (Android 7.1)** &ndash; _cukrovinkách typu nugát_, vydaná prosinci 2016. Verze kódu sestavení `Android.OS.BuildVersionCodes.NMr1`

-   **Rozhraní API 24 (Android 7.0)** &ndash; _cukrovinkách typu nugát_, vydaná srpna 2016. Verze kódu sestavení `Android.OS.BuildVersionCodes.N`

-   **Rozhraní API 23 (Android 6.0)** &ndash; _Marshmallow_, vydaná srpen 2015. Verze kódu sestavení `Android.OS.BuildVersionCodes.M`

-   **Rozhraní API 22 (Android 5.1)** &ndash; _typu Lupa_, vydaná března 2015. Verze kódu sestavení `Android.OS.BuildVersionCodes.LollipopMr1`

-   **Rozhraní API 21 (Android 5.0)** &ndash; _typu Lupa_, vydaná listopadu 2014. Verze kódu sestavení `Android.OS.BuildVersionCodes.Lollipop`

-   **Rozhraní API 20 (Android 4.4W)** &ndash; _Kitkat sledovat_, vydaná června 2014. Verze kódu sestavení `Android.OS.BuildVersionCodes.KitKatWatch`

-   **Rozhraní API 19 (Android 4.4)** &ndash; _Kitkat_, vydaná října 2013. Verze kódu sestavení `Android.OS.BuildVersionCodes.KitKat`

-   **Rozhraní API 18 (Android 4.3)** &ndash; _položku Bean želé_, vydaná července 2013. Verze kódu sestavení `Android.OS.BuildVersionCodes.JellyBeanMr2`

-   **Rozhraní API 17 (Android 4.2-4.2.2)** &ndash; _položku Bean želé_, vydaná listopad 2012. Verze kódu sestavení `Android.OS.BuildVersionCodes.JellyBeanMr1`

-   **Rozhraní API 16 (Android 4.1 4.1.1)** &ndash; _položku Bean želé_, vydaná června 2012. Verze kódu sestavení `Android.OS.BuildVersionCodes.JellyBean`

-   **Rozhraní API 15 (Android 4.0.3-4.0.4)** &ndash; _Zmrzlinová Sandwichovy_, vydaná prosince 2011. Verze kódu sestavení `Android.OS.BuildVersionCodes.IceCreamSandwichMr1`

-   **Rozhraní API 14 (Android 4.0 4.0.2)** &ndash; _Zmrzlinová Sandwichovy_, vydaná Říjen 2011. Verze kódu sestavení `Android.OS.BuildVersionCodes.IceCreamSandwich`

-   **Rozhraní API 13 (Android 3.2)** &ndash; _Voštinový_, vydaná června 2011. Verze kódu sestavení `Android.OS.BuildVersionCodes.HoneyCombMr2`

-   **Rozhraní API 12 (Android 3.1.x)** &ndash; _Voštinový_, vydaná může 2011. Verze kódu sestavení `Android.OS.BuildVersionCodes.HoneyCombMr1`

-   **Rozhraní API 11 (Android 3.0.x)** &ndash; _Voštinový_, vydaná února 2011. Verze kódu sestavení `Android.OS.BuildVersionCodes.HoneyComb`

-   **Rozhraní API 10 (Android 2.3.3-2.3.4)** &ndash; _Perník_, vydaná února 2011. Verze kódu sestavení `Android.OS.BuildVersionCodes.GingerBreadMr1`

-   **Rozhraní API 9 (Android 2.3 2.3.2)** &ndash; _Perník_, vydaná listopadu 2010. Verze kódu sestavení `Android.OS.BuildVersionCodes.GingerBread`

-   **Rozhraní API 8 (Android 2.2.x)** &ndash; _Froyo_, vydaná června 2010. Verze kódu sestavení `Android.OS.BuildVersionCodes.Froyo`

-   **Rozhraní API 7 (Android 2.1.x)** &ndash; _Eclair_, vydaná leden 2010. Verze kódu sestavení `Android.OS.BuildVersionCodes.EclairMr1`

-   **Rozhraní API 6 (Android 2.0.1)** &ndash; _Eclair_, vydaná prosinec 2009. Verze kódu sestavení `Android.OS.BuildVersionCodes.Eclair01`

-   **Rozhraní API 5 (Android 2.0)** &ndash; _Eclair_, vydaná listopadem 2009. Verze kódu sestavení `Android.OS.BuildVersionCodes.Eclair`

-   **Rozhraní API 4 (Android 1.6)** &ndash; _prstenec_, vydaná září 2009. Verze kódu sestavení `Android.OS.BuildVersionCodes.Donut`

-   **Rozhraní API 3 (Android 1.5)** &ndash; _Cupcake_, vydaná může 2009. Verze kódu sestavení `Android.OS.BuildVersionCodes.Cupcake`

-   **Rozhraní API 2 (Android 1.1)** &ndash; _základní_, vydaná únor 2009. Verze kódu sestavení `Android.OS.BuildVersionCodes.Base11`

-   **Rozhraní API 1 (Android 1.0)** &ndash; _základní_, vydaná říjen 2008. Verze kódu sestavení `Android.OS.BuildVersionCodes.Base`


Jak uvádí tento seznam, jsou často vydávány nové verze Android &ndash; někdy několik verzí za jeden rok. V důsledku toho zahrnuje veškerý zařízení Android, která může spuštění aplikace z široké škály verze Android starší a novější. Jak může zaručit, že vaše aplikace poběží konzistentně a spolehlivě v mnoho různých verzích systému Android? Úrovně rozhraní API pro Android můžete spravovat tento problém.


### <a name="android-api-levels"></a>Úrovně rozhraní API systému Android

Každé zařízení se systémem Android spouští v přesně *jeden* úroveň rozhraní API &ndash; tato úroveň rozhraní API se musí být jedinečný pro platformu Android verze. Úroveň rozhraní API přesněji identifikuje verzi sada rozhraní API, které aplikace můžete volat do; identifikuje kombinace manifestu prvky, oprávnění, atd tento můžete kód proti jako vývojář. Úrovně rozhraní API systému Android na pomáhá určit, zda je aplikace kompatibilní s bitovou kopii systému Android před instalací aplikace na zařízení Android.

Když je integrovaná aplikace, obsahuje následující informace o úrovni rozhraní API:

-   *Cíl* úroveň rozhraní API systému Android, vytvořené pro spouštění v aplikaci.

-   *Minimální* úroveň rozhraní API systému Android, která zařízení se systémem Android musí mít ke spouštění vaší aplikace. 

Toto nastavení slouží k zajištění, že funkce potřebné pro správné fungování aplikace je k dispozici na zařízení s Androidem v průběhu instalace. Pokud ne, aplikace je blokováno v spuštěné na daném zařízení. Například pokud úroveň rozhraní API zařízení se systémem Android je nižší než minimální úroveň API, který zadáte pro vaši aplikaci, zařízení s Androidem zabrání uživateli v instalaci vaší aplikace.


## <a name="project-api-level-settings"></a>Nastavení projektu úroveň rozhraní API

Následující části popisují způsob použití sady SDK Manager Příprava vývojového prostředí pro úrovně rozhraní API, kterou chcete zacílit, za nímž následuje podrobné vysvětlení, jak nakonfigurovat *cílové rozhraní*, *minimální Verzi systému Android*, a *cílovou verzi systému Android* nastavení v Xamarin.Android.


### <a name="android-sdk-platforms"></a>Platformy Android SDK

Cíl a rozhraní API minimální úroveň mohli vybrat v Xamarin.Android, musíte nainstalovat verzi platformy Android SDK, která odpovídá na této úrovni rozhraní API. Rozsah hodnot k dispozici pro cílové rozhraní, minimální verzi systému Android a cílovou verzi systému Android je omezen na rozsah Android SDK verze, které jste nainstalovali. Správce sady SDK můžete ověřte, zda jsou nainstalovány požadované verze sady SDK pro Android, a ve kterém můžete přidat nové úrovně rozhraní API, které potřebujete pro vaši aplikaci. Pokud nejste obeznámeni s postup instalace úrovně rozhraní API, přečtěte si téma [Android SDK instalace](~/android/get-started/installation/android-sdk.md).

<a name="framework" />

### <a name="target-framework"></a>Cílová architektura

*Cílové rozhraní* (také označované jako `compileSdkVersion`) je verze konkrétní Android framework (úroveň rozhraní API), která vaše aplikace je zkompilovaném pro v čase vytvoření buildu. Toto nastavení určuje, jaké rozhraní API aplikace *očekává* má použít při spuštění, ale nemá žádný vliv, na kterém jsou ve skutečnosti k dispozici pro vaše aplikace API, při instalaci. Změna nastavení cílové rozhraní v důsledku toho nezmění modul runtime chování.

Cílovém Frameworku, který identifikuje jaké verze knihovny aplikace je propojený s &ndash; určí, které rozhraní API můžete použít ve vaší aplikaci. Například, pokud chcete použít [NotificationBuilder.SetCategory](https://developer.xamarin.com/api/member/Android.App.Notification+Builder.SetCategory/p/System.String/) metoda, která byla zavedena v systému Android 5.0 typu Lupa, nastavte v cílovém Frameworku, který **21 úroveň rozhraní API (typu Lupa)** nebo novější. Pokud nastavíte cílový Framework projektu na na rozhraní API úrovně, jako **rozhraní API úrovně 19 (KitKat)** a pokuste se zavolat `SetCategory` metoda ve vašem kódu, zobrazí se chyba kompilace.

Doporučujeme vám, že je vždy kompilovat s *nejnovější* dostupná verze cílové rozhraní. Díky tomu vám poskytne užitečné zprávy upozornění pro zastaralé rozhraní API, která může být volána vašeho kódu. Pomocí nejnovější verze cílové rozhraní je obzvláště důležité, pokud používáte nejnovější verze knihovny podporu &ndash; jednotlivých knihoven očekává vaší aplikace na kompilované minimální úroveň API knihovní podporu nebo vyšší. 

> [!NOTE]
> Počínaje srpen 2018, Google Play Console bude vyžadovat, nové aplikace cílí úroveň rozhraní API 26 (Android 8.0) nebo vyšší.
Existující aplikace, bude nutné cílit na úrovni rozhraní API 26 nebo vyšší od listopadu 2018. Další informace najdete v tématu [zlepšení aplikace zabezpečení a výkonu na webu Google Play let pocházet](https://android-developers.googleblog.com/2017/12/improving-app-security-and-performance.html).


# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Pro přístup k nastavení cílové rozhraní v sadě Visual Studio, otevřete vlastnosti projektu v **Průzkumníku řešení** a vyberte **aplikace** stránky:

[![Aplikace na stránce vlastností projektu](android-api-levels-images/vs-target-framework-sml.png)](android-api-levels-images/vs-target-framework.png#lightbox)

Nastavte cílové rozhraní, vyberte úroveň rozhraní API v rozevírací nabídce v části **zkompilovat pomocí Android verze** jako v příkladu nahoře.

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

Pro přístup k nastavení cílové rozhraní v sadě Visual Studio pro Mac, klikněte pravým tlačítkem na název projektu a vyberte **možnosti**; tento otevře **možnosti projektu** dialogové okno. V tomto dialogovém okně, přejděte na **sestavení > Obecné** jak je vidět tady:

[![Sestavení obecné části na stránce Možnosti projektu](android-api-levels-images/xs-target-framework-sml.png)](android-api-levels-images/xs-target-framework.png#lightbox)

Nakonfigurujte rozhraní Target Framework výběrem úroveň rozhraní API v rozevírací nabídce vpravo od **cílové rozhraní** jako v příkladu nahoře.

-----


<a name="minimum" />

### <a name="minimum-android-version"></a>Minimální verze Android

*Minimální verzi systému Android* (také označované jako `minSdkVersion`) je nejstarší verze operační systém Android (tj, nejnižší úroveň rozhraní API), můžete nainstalovat a spustit svoji aplikaci. Ve výchozím nastavení aplikace jenom lze nainstalovat na zařízení odpovídající nastavení cílové rozhraní nebo vyšší; Pokud je minimální Android nastavení verze *nižší* než nastavení pro cílové rozhraní, můžete taky spustit aplikaci v dřívějších verzích systému Android. Například pokud nastavíte na cílové rozhraní **Android 7.1 (cukrovinkách typu nugát)** a nastavte Android minimální verzi **Android 4.0.3 (Zmrzlinová Sandwichovy)**, vaše aplikace lze nainstalovat na jakékoli platformě z rozhraní API úrovně 15 úroveň rozhraní API 25 (včetně).

I když vaše aplikace může úspěšně sestavení a nainstalovat na tento rozsah platforem, to není zaručeno, že se úspěšně *spustit* na všech těchto platforem. Například pokud vaše aplikace je nainstalovaná na **Android 5.0 (typu Lupa)** a váš kód zavolá rozhraní API, která je k dispozici pouze v **Android 7.1 (cukrovinkách typu nugát)** a novějších, vaše aplikace bude chyba runtime a pravděpodobně k chybě. Proto musíte zajistit kódu &ndash; za běhu &ndash; zavolá pouze API, které podporuje zařízení s Androidem, která běží na. Jinými slovy kódu musí obsahovat explicitní runtime kontroly, aby se zajistilo, že vaše aplikace používá novější rozhraní API jenom na zařízení, která jsou dostatečně nová, které je podporují.
[Modul runtime vyhledává Android verze](#runtimechecks)dál v této příručce se vysvětluje postup přidání těchto kontrol za běhu kódu.


# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Pro přístup k systému Android minimální verze nastavení v sadě Visual Studio, otevřete vlastnosti projektu v **Průzkumníku řešení** a vyberte **Android Manifest** stránky. V rozevírací nabídce v části **minimální verzi systému Android** minimální verzi systému Android můžete vybrat pro aplikaci:

[![Minimální Android – možnost cílové nastavit zkompilovat pomocí verze sady SDK](android-api-levels-images/vs-minimum-version-sml.png)](android-api-levels-images/vs-minimum-version.png#lightbox)

Pokud vyberete **použití zkompilovat pomocí sady SDK verze**, Android minimální verze bude stejné jako nastavení cílové rozhraní.

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

Pro přístup k nastavení cílové rozhraní v sadě Visual Studio pro Mac, klikněte pravým tlačítkem na název projektu a vyberte **možnosti**; tento otevře **možnosti projektu** dialogové okno. Přejděte na **sestavení > aplikace pro Android**.
Pomocí rozevíracího seznamu nabídky napravo od **minimální verzi systému Android**, můžete nastavit minimální verzi systému Android pro aplikaci:

[![Minimální verze Android nastavená na hodnotu automaticky - použití cílová verze framework](android-api-levels-images/xs-minimum-version-sml.png)](android-api-levels-images/xs-minimum-version.png#lightbox)

Pokud vyberete **automatické &ndash; použít cílová verze framework**, Android minimální verze bude stejné jako nastavení cílové rozhraní.

-----


<a name="target" />

### <a name="target-android-version"></a>Cílová verze Android

*Cílová verze Android* (také označované jako `targetSdkVersion`) je rozhraní API úroveň zařízení s Androidem, kde se předpokládá, že aplikace spustit. Android toto nastavení používá k určení, zda povolit všechny kompatibility chování &ndash; zajistíte tak, že aplikace bude pořád fungovat očekávaným způsobem. Android používá nastavení cílové Android verzi vaší aplikace a zjistěte, které chování změny mohou být použity k vaší aplikaci, aniž by vás ho (jedná se jak Android poskytuje dopřednou kompatibilitu).

Cílová architektura a cílovou verzi systému Android, přitom má velmi podobné názvy nejsou samé. Nastavení cílového prostředí komunikuje cílové rozhraní API úrovně informace, které Xamarin.Android pro použití v *doba kompilace*, zatímco cílovou verzi systému Android komunikuje cílové rozhraní API úrovně informace do systému Android pro použití v  *čas spuštění* (Pokud aplikace je nainstalovaná a spuštěná na zařízení).

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Pro přístup k toto nastavení v sadě Visual Studio, otevřete vlastnosti projektu v **Průzkumníku řešení** a vyberte **Android Manifest** stránky. V rozevírací nabídce v části **cílovou verzi systému Android** můžete vybrat verzi systému Android cíl pro vaši aplikaci:

[![Cílová verze Android nastavit zkompilovat pomocí verze sady SDK](android-api-levels-images/vs-target-version-sml.png)](android-api-levels-images/vs-target-version.png#lightbox)

Doporučujeme, abyste explicitně nastavit cílovou verzi systému Android na nejnovější verzi Androidu, který používáte k testování aplikace. V ideálním případě by měla být nastavena na nejnovější verzi sady SDK pro Android &ndash; to umožňuje pomocí nových rozhraní API před projdete změny chování. Pro většinu vývojáře jsme *nepodporují* doporučujeme nastavení cílovou verzi systému Android **použití zkompilovat pomocí sady SDK verze**.

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

Pro přístup k nastavení cílové rozhraní v sadě Visual Studio pro Mac, klikněte pravým tlačítkem na název projektu a vyberte **možnosti**; tento otevře **možnosti projektu** dialogové okno. Přejděte na **sestavení > aplikace pro Android**.
Pomocí rozevíracího seznamu nabídky napravo od **cílovou verzi systému Android**, můžete nastavit cílovou verzi systému Android pro aplikaci:

[![Cílová verze Android nastavená na hodnotu automaticky - použití cílová verze framework](android-api-levels-images/xs-target-version-sml.png)](android-api-levels-images/xs-target-version.png#lightbox)

Doporučujeme, abyste explicitně nastavit cílovou verzi systému Android na nejnovější verzi Androidu, který používáte k testování aplikace. V ideálním případě by měla být nastavena na nejnovější dostupné verzi sady SDK pro Android &ndash; to umožňuje pomocí nových rozhraní API před projdete změny chování. Většina vývojářů, není doporučeno nastavení cílovou verzi systému Android **automatické - použití target framework verze**.

-----

Cílová verze Android by měl být obecně ohraničené minimální verze Android a cílové rozhraní. To je:

**Minimální verze Android < = cílová verze Android < = cílové rozhraní**

Další informace o úrovních SDK najdete v tématu Android Developer [sdk používá](https://developer.android.com/guide/topics/manifest/uses-sdk-element.html) dokumentaci.


<a name="runtimechecks" />

## <a name="runtime-checks-for-android-versions"></a>Modul runtime kontroly pro Android verze

Jako vydání každé nové verze systému Android, rozhraní API je aktualizována tak, aby poskytovala nové nebo nahrazení funkce. Několik výjimek funkce rozhraní API z dřívějších verzí systému Android se přenášejí do novější verze Android bez úprav. Výsledkem je Pokud vaše aplikace běží na konkrétní úrovni rozhraní API systému Android, obvykle bude možné spouštět na vyšší úrovni rozhraní API systému Android bez úprav. Ale co když chcete taky ke spouštění vaší aplikace v dřívějších verzích systému Android?

Pokud vyberete Android minimální verzi, která je *nižší* než vaše nastavení cílové rozhraní, nemusí být některé rozhraní API do vaší aplikace za běhu. Aplikace však stále můžete spustit na starší zařízení, ale s omezenou funkčností. Pro každé rozhraní API, která není k dispozici na odpovídající vaše nastavení Android minimální verze Android platformách, musí váš kód explicitně zkontrolujte hodnotu `Android.OS.Build.VERSION.SdkInt` vlastnosti k určení úrovně rozhraní API platformy aplikace spuštěná. Pokud je úroveň rozhraní API *nižší* než Android minimální verzi, která podporuje rozhraní API, který chcete použít, pak má váš kód najít způsob fungování bez provedení toto volání rozhraní API.

Například předpokládejme, že má být použít [NotificationBuilder.SetCategory](https://developer.xamarin.com/api/member/Android.App.Notification+Builder.SetCategory/p/System.String/) metoda zařadit do kategorií oznámení, když se používá **Android 5.0 typu Lupa** (a novější), ale stále chceme, aby naše aplikace spuštění v dřívějších verzích systému Android například **položku Android 4.1 želé Bean** (kde `SetCategory` není k dispozici). Odkaz na tabulku verzi systému Android na začátku tohoto průvodce, jsme zjistíte, že je kód verze sestavení pro **Android 5.0 typu Lupa** je `Android.OS.BuildVersionCodes.Lollipop`. K podpoře starší verze Android kde `SetCategory` není k dispozici, našem kódu, můžete zjistit úroveň rozhraní API za běhu a podmíněně volání `SetCategory` pouze v případě úroveň rozhraní API je větší než nebo rovna hodnotě kód typu Lupa sestavení verze:

```csharp
if (Android.OS.Build.VERSION.SdkInt >= Android.OS.BuildVersionCodes.Lollipop)
{
    builder.SetCategory(Notification.CategoryEmail);
}
```

V tomto příkladu cílové rozhraní vaší aplikace nastavena na **Android 5.0 (API úrovně 21)** a její minimální verzi systému Android je nastavena na **Android 4.1 (rozhraní API Level 16)**. Protože `SetCategory` je k dispozici na úrovni rozhraní API `Android.OS.BuildVersionCodes.Lollipop` a později tento ukázkový kód zavolá `SetCategory` pouze pokud je ve skutečnosti k dispozici &ndash; proběhne *není* pokus o volání `SetCategory` při rozhraní API úroveň je 16, 17, 18, 19 nebo 20. Funkce je snížen na tyto starší verze Android pouze, pokud nejsou oznámení seřazeny správně (vzhledem k tomu, že nejsou rozdělené podle typu), ale oznámení jsou stále publikovány k upozornění uživatele. Naše aplikace stále funguje, ale jeho funkce mírně dojde ke snížení.

Kontrola verze sestavení obecně platí, pomůže rozhodnout za běhu mezi dělat něco nový způsob porovnání staré způsob kódu. Příklad:

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

Neexistuje žádné rychlé a jednoduché pravidlo, které vysvětluje, jak snížit nebo změnit funkce vaší aplikace, pokud běží na starší verze Android, které chybí jeden nebo více rozhraní API. V některých případech (, jako `SetCategory` výše uvedeném příkladu), stačí jednoduše vynechejte volání rozhraní API, když není k dispozici. Ale v ostatních případech musíte implementovat alternativní funkce pro případ `Android.OS.Build.VERSION.SdkInt` je zjištěna být menší než rozhraní API úrovně, které aplikace potřebuje pro jeho optimální prostředí k dispozici.

<a name="libraries" />

## <a name="api-levels-and-libraries"></a>Úrovně rozhraní API a knihovny

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Když vytvoříte projekt Xamarin.Android knihovny (například knihovny tříd nebo knihovnu vazby), můžete nakonfigurovat jenom nastavení cílové rozhraní &ndash; cíl Android nastavení verze a minimální verzi systému Android nejsou k dispozici. Důvodem je, že neexistuje žádná **Android Manifest** stránky:

[![Je k dispozici pouze kompilace pomocí možnosti verzi systému Android](android-api-levels-images/vs-library-options-sml.png)](android-api-levels-images/vs-library-options.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

Když vytvoříte projekt Xamarin.Android knihovny, je žádné **aplikace pro Android** stránky, kde můžete konfigurovat cílový Android a Android minimální verze &ndash; minimální verzi systému Android a cíle Nastavení verzi systému Android nejsou k dispozici.
Důvodem je, že neexistuje žádná **sestavení > aplikace pro Android** stránky):

[![Obecná stránka bez možnosti minimální a cílové verze sestavení](android-api-levels-images/xs-library-options-sml.png)](android-api-levels-images/xs-library-options.png#lightbox)

-----

Cíl Android verze nastavení a minimální verzi systému Android nejsou k dispozici, protože výsledný knihovna není samostatné aplikace &ndash; knihovně by bylo možné spustit na jakékoli verzi systému Android, v závislosti na aplikaci, která je zabalit s. Můžete zadat, jak má být knihovny *zkompilovat*, ale nemůžete předpovědět, kterou platformu úroveň rozhraní API knihovny se spustí na. Myslete na to by měl být dodržen následující osvědčené postupy při použití nebo vytvoření knihovny:

-   **Při využívání knihovna pro Android** &ndash; Pokud knihovna pro Android spotřebovávají ve vaší aplikaci, nezapomeňte nastavit vaší aplikace cílové rozhraní nastavení do rozhraní API úrovně *vysoké jako* cíl Framework nastavení knihovny.

-   **Při vytváření knihovna pro Android** &ndash; knihovna pro Android pro použití při vytváření jiné aplikace, nezapomeňte nastavit jeho nastavení cílové rozhraní na minimální úroveň rozhraní API, kterou potřebuje kompilaci.

Aby se zabránilo situaci, kde se pokusí knihovnu volat rozhraní API, která není k dispozici v době běhu (který může způsobit aplikaci havárií) doporučují se tyto osvědčené postupy. Pokud jste vývojář knihovny, měli snažit omezit vaše použití volání rozhraní API na malé a dobře zavedené podmnožinu celkového API povrchu. To proto pomáhá zajistit, že vaše knihovna je možné bezpečně mezi širší rozsah Android verzemi.


## <a name="summary"></a>Souhrn

Tato příručka vysvětluje, jak úrovně rozhraní API systému Android se používají ke správě kompatibility aplikace v různých verzích Android. Je poskytovala podrobné kroky pro konfiguraci Xamarin.Android *cílové rozhraní*, *minimální verzi systému Android*, a *cílovou verzi systému Android* nastavení projektu. Je k dispozici pokyny pro instalaci balíčků SDK, pomocí Android SDK Manager zahrnuté příklady, jak napsat kód, jak nakládat s různými úrovněmi rozhraní API za běhu a vysvětlení způsobu správy úrovně rozhraní API při vytváření nebo využívání Android knihovny. Poskytuje také kompletní seznam úrovně rozhraní API související s verzi systému Android čísla (třeba Android 4.4), názvy verzi systému Android (například Kitkat) a kódy verze sestavení Xamarin.Android.


## <a name="related-links"></a>Související odkazy

- [Instalace sady Android SDK](~/android/get-started/installation/android-sdk.md)
- [Změní nástrojů sady SDK rozhraní příkazového řádku](~/android/troubleshooting/sdk-cli-tooling-changes.md)
- [Výběr vaší compileSdkVersion, minSdkVersion a targetSdkVersion](https://medium.com/google-developers/picking-your-compilesdkversion-minsdkversion-targetsdkversion-a098a0341ebd)
- [Co je úroveň rozhraní API?](http://developer.android.com/guide/topics/manifest/uses-sdk-element.html#ApiLevels)
- [Codenames, značky a čísla sestavení](https://source.android.com/source/build-numbers)
