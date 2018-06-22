---
title: Úvod do Android opotřebení
description: Se zavedením Google Android nosit už nejste právě telefony a tablety s omezeným přístupem při rozhodování o vývoji kvalitních aplikací pro Android. Podpora pro Xamarin.Android pro Android nosit umožňuje spustit na zápěstí kód C#! Tento úvod poskytuje základní přehled Android nosit, popisuje hlavní funkce a nabízí přehled funkcí, které jsou k dispozici v systému Android nosit 2.0. Vypíše některé oblíbenější Android nosit zařízení a poskytuje odkazy na základní Google Android nosit dokumentaci pro další čtení.
ms.prod: xamarin
ms.assetid: EAEF99F0-8FBE-47E4-8644-E7244CFAF464
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 03/01/2018
ms.openlocfilehash: 0ab166bb71c23d456cb70d35a2794717110642fd
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/04/2018
ms.locfileid: "30772091"
---
# <a name="introduction-to-android-wear"></a>Úvod do Android opotřebení

_Se zavedením Google Android nosit už nejste právě telefony a tablety s omezeným přístupem při rozhodování o vývoji kvalitních aplikací pro Android. Podpora pro Xamarin.Android pro Android nosit umožňuje spustit na zápěstí kód C#! Tento úvod poskytuje základní přehled Android nosit, popisuje hlavní funkce a nabízí přehled funkcí, které jsou k dispozici v systému Android nosit 2.0. Vypíše některé oblíbenější Android nosit zařízení a poskytuje odkazy na základní Google Android nosit dokumentaci pro další čtení._


## <a name="overview"></a>Přehled

Android opotřebení běží na různých zařízení, včetně first-generation Motorola 360, sledovat kontaktní skupina na G nebo za zařízením Samsung provozu. Druhé generace, včetně na Sony SmartWatch 3, má taky jsme vydali s další funkce, včetně předdefinovaných GPS a offline Hudba přehrávání. Pro Android nosit 2.0, Google ve spolupráci s kontaktní skupina pro dva nové sleduje: Sport sledovat kontaktní skupině a stylu sledovat kontaktní skupině.

![Zařízení se systémem Android nosit 2.0](intro-to-wear-images/hero-image.png "příklad nosit 2.0 zařízení se systémem Android")

Podpora Xamarin.Android 5.0 a novější podporuje Android nosit prostřednictvím našich Android 4.4W (rozhraní API 20) a balíčku NuGet, který přidá další prvky uživatelského rozhraní a opotřebením motoru specifické. Xamarin.Android 5.0 a novější také zahrnuje funkce pro zabalení aplikace a opotřebením motoru. Balíčky NuGet jsou také k dispozici pro Android nosit 2.0 jak je popsáno v této příručce.


## <a name="android-wear-basics"></a>Základní informace o opotřebení Android

Android opotřebení má zlepší rozhraní uživatele, který se liší od kapesních aplikací pro Android. První wave opotřebení aplikací byly navržené k rozšíření doprovodné kapesních aplikace v některé způsobem, ale od verze Android opotřebení 2.0, opotřebení aplikace lze použít samostatné. Při nasazení aplikace a opotřebením motoru, se zabalí a doprovodné kapesních aplikaci. Protože nosit většinu aplikací závisí na kapesních doprovodné aplikace, potřebují nějakým způsobem ke komunikaci s kapesních aplikace. Následující části popisují tyto scénáře použití a popisují základní Android nosit funkce. 



### <a name="usage-scenarios"></a>Scénáře použití

První verze součásti Android nosit se zaměřuje především na rozšíření aktuální kapesních aplikace s rozšířenou oznámení a synchronizaci dat mezi wearable aplikace a kapesních aplikace. Tyto scénáře jsou proto relativně jednoduché k implementaci.


#### <a name="wearable-notifications"></a>Wearable oznámení

Nejjednodušší způsob, jak podporují Android nosit je využít sdílené povaha oznámení mezi přenosného terminálu a wearable zařízení. Pomocí oznámení v4 podporu rozhraní API a `WearableExtender` – třída (k dispozici v [Xamarin Android podporují knihovny](https://www.nuget.org/packages/Xamarin.Android.Support.v4/)), můžete klepněte do nativní funkce platformy, jako jsou karty styl doručené pošty nebo hlasový vstup. [RecipeAssistant](https://developer.xamarin.com/samples/monodroid/wear/RecipeAssistant/) ukázka poskytuje ukázkový kód, který ukazuje, jak poslat seznam oznámení do zařízení se systémem Android nosit. 



#### <a name="companion-applications"></a>Doprovodné aplikace

Další strategie je vytvoření kompletní aplikaci, která běží nativně na wearable zařízení a páry a doprovodné kapesních aplikaci. Dobrým příkladem tohoto přístupu je [kvízu s časovým limitem](https://developer.xamarin.com/samples/monodroid/wear/Quiz/) ukázkovou aplikaci, která ukazuje, jak vytvořit kvízu, který běží v kapesních zařízeních a vás kvízu s časovým limitem ptát na wearable zařízení. 



### <a name="user-interface"></a>Uživatelské rozhraní

Primární navigační vzor pro opotřebení je řadu karty ve svislém uspořádání. Každý z těchto karet můžete mít přidružené akce, které jsou na základě limitu na stejný řádek. `GridViewPager` Třída poskytuje tuto funkci; dodržuje stejný adaptér koncept jako `ListView`. Obvykle přidružíte `GridViewPager` s `FragmentGridPagerAdaptor` (nebo `GridPagerAdaptor`), která umožňuje představovat jednotlivých řádků a sloupců buňky jako `Fragment`: 

[![Nosit navigační](intro-to-wear-images/2d-picker-sml.png "nosit navigace")](intro-to-wear-images/2d-picker.png#lightbox)

Nosit díky použití tlačítka akce, která se skládá z big barevné kroužkem malé popisný text pod ním (jako ilustrované výše).  [GridViewPager](https://developer.xamarin.com/samples/monodroid/wear/GridViewPager/) příklad ukazuje způsob použití `GridViewPager` a `GridPagerAdapter` v aplikaci a opotřebením motoru.

Android nosit 2.0 přidá zásuvce navigace, nástroj drawer akce a vložené akce tlačítka s uživatelským rozhraním a opotřebením motoru. Další informace o prvky uživatelského rozhraní Android nosit 2.0, naleznete v části pro Android [Anatomy](https://www.google.com/design/spec-wear/system-overview/anatomy.html) tématu. 



### <a name="communications"></a>Komunikace

Android opotřebení nabízí dva různé komunikační rozhraní API pro usnadnění komunikace mezi wearable aplikace a doprovodné kapesních aplikace: 

**Data rozhraní API** &ndash; toto rozhraní API je podobná úložiště synchronizovaná data mezi wearable zařízení a kapesní zařízení. Android postará šíření změny mezi wearable a kapesních v případě, že je optimální Uděláte to tak. Pokud na nošení je mimo rozsah, fronty synchronizace na pozdější dobu. Hlavní vstupní bod pro toto rozhraní API je `WearableClass.DataApi`. Další informace o tomto rozhraní API najdete v tématu pro Android [synchronizuje datových položek](https://developer.android.com/training/wearables/data-layer/data-items.html) tématu. 

**Zpráva rozhraní API** &ndash; toto rozhraní API umožňuje použití cesty k nižší úrovně komunikace: malé datové části je odeslán jednosměrný bez synchronizace mezi kapesních a wearable aplikacemi.
Hlavní vstupní bod pro toto rozhraní API je `WearableClass.MessageApi`.
Další informace o tomto rozhraní API najdete v tématu pro Android [odesílání a přijímání zpráv](https://developer.android.com/training/wearables/data-layer/messages.html) tématu.

Můžete registrace zpětných volání pro příjem těchto zpráv z každé z rozhraní API naslouchací proces, nebo můžete taky implementovat služby ve vaší aplikaci, která je odvozena z `WearableListenerService`.
Tato služba bude automaticky vytvořena instance pomocí Android nosit.
[FindMyPhone](https://developer.xamarin.com/samples/monodroid/wear/FindMyPhoneSample/) sample ukazuje, jak implementovat `WearableListenerService`.



### <a name="deployment"></a>Nasazení

Každé wearable aplikace se nasazuje s vlastního souboru APK vložená do hlavní aplikace APK. Tato balení automaticky zpracovává v Xamarin.Android 5.0 nebo novější, ale musíte ručně provést pro verze aplikace Xamarin.Android dřívější než verze 5.0. 
[Práce s balení](~/android/wear/deploy-test/packaging.md) vysvětluje nasazení ve více podrobností. 



## <a name="going-further"></a>Budete pokračovat 

Nejlepší způsob, jak se seznámit s Androidem nosit je sestavení a testování vaší první aplikace. Následující seznam uvádí pořadí od doporučené zdroje, které vám pomohou rychle se zorientujte:

1.  [Nastavení a instalace](~/android/wear/get-started/installation.md) poskytuje podrobné pokyny pro instalaci a konfiguraci vývojového prostředí pro vytváření aplikací Xamarin.Android a opotřebením motoru. 

2.  Po instalaci požadované balíčky a nakonfigurovat emulátoru nebo zařízení, najdete v části [nosit Hello,](~/android/wear/get-started/hello-wear.md) podrobné pokyny, které vysvětlují, jak vytvořit projekt Android nosit malé této obslužné rutiny tlačítko klikne a zobrazí se Klikněte na tlačítko čítač na opotřebení ze zařízení. 

3.  [Nasazení a testování](~/android/wear/deploy-test/index.md) poskytuje podrobnější informace o konfiguraci a nasazení do emulátorů a zařízení, včetně pokyny k nasazení aplikace do zařízení a opotřebením motoru přes Bluetooth.

4.  [Práce s velikost obrazovky](~/android/wear/screen-sizes.md) vysvětluje, jak zobrazit náhled a optimalizovat uživatelského rozhraní pro různé velikost dostupné obrazovky na zařízeních a opotřebením motoru. 

5.  [Práce s balení](~/android/wear/deploy-test/packaging.md) popisuje kroky pro ruční balení opotřebení aplikace pro distribuci na webu Google Play.

Po vytvoření první aplikace a opotřebením motoru, můžete chtít zkuste jej sestavit vlastní sledování řez pro Android nosit. 
[Vytváření řez sledovat](~/android/wear/platform/creating-a-watchface.md) poskytuje podrobné pokyny a příklady kódu pro vývoj odřapíkovaného dolů služby vzhled digitální sledovat, za nímž následuje další kód, který rozšiřuje na analogovým stylu sledovat setkávají s další funkce. 



## <a name="android-wear-20"></a>Android opotřebení 2.0

Android nosit 2.0 přináší řadu nových funkcí a možností, jako například *komplikace*, zakřivené rozložení, navigace a akce zásobníky a rozbalených oznámeních. Navíc nosit 2.0 umožňuje můžete vytvářet samostatná aplikace, které pracují nezávisle na kapesních aplikace. Nové *zápěstí gesta* funkce umožňuje pohybového interakce s vaší aplikací. V následujících částech zvýrazněte tyto funkce a poskytuje odkazy vám pomohou začít s použitím jejich ve vaší aplikaci.



### <a name="install-wear-20-packages"></a>Instalace nosit 2.0 balíčky

Chcete-li sestavit aplikaci 2.0 nosit s Xamarin.Android, je nutné přidat **Xamarin.Android.Wear v2.0** balíčku do projektu (klikněte na tlačítko **kartu Procházet**):

[![Xamarin.Android.Wear v2.0](intro-to-wear-images/wear-nuget-2.0-sml.png "nainstalovat Xamarin.Android.Wear v2.0 NuGet")](intro-to-wear-images/wear-nuget-2.0.png#lightbox)

Balíček NuGet obsahuje vazby pro Android podporu Wearable i nosit Compat knihovny.

Kromě **Xamarin.Android.Wear**, doporučujeme vám nainstalovat **Xamarin.GooglePlayServices.Wearable** NuGet: 

[![Xamarin.GooglePlayServices.Wearable](intro-to-wear-images/gpsw-nuget-sml.png "nainstalovat Xamarin.GooglePlayServices.Wearable NuGet")](intro-to-wear-images/gpsw-nuget.png#lightbox)


### <a name="key-features-of-wear-20"></a>Klíčové funkce opotřebení 2.0

Android nosit 2.0 je největších aktualizace Android nosit od jeho spuštění počáteční 2014. V následujících částech klíčové funkce Android nosit 2.0 a jsou uvedeny odkazy na pomoci můžete začít používat tyto nové funkce ve vaší aplikaci. 


#### <a name="complications"></a>Komplikace

*Komplikace* jsou malé sledovat vzhled pomůcky, které se zobrazí na první pohled bez nutnosti prstem sledovat písmo. Komplikace jsou podobná pomůcky řídicího panelu stylu plochy; zobrazují informace, jako je například počasí, z baterie, kalendář události a vhodnosti aplikace statistiky: 

![Příklad komplikace](intro-to-wear-images/complications.png "komplikace příklad")

Další informace o komplikace najdete v tématu Android [sledovat vzhled komplikace](https://developer.android.com/wear/preview/features/complications.html) tématu. 



#### <a name="navigation-and-action-drawers"></a>Navigační a zásobníky akce 

Dva nové zásobníky jsou součástí nosit 2.0. *Nástroj navigační drawer*, která se zobrazí v horní části obrazovky, umožňuje uživatelům přecházet mezi jednotlivými zobrazení aplikace (jak je znázorněno na levé straně níže). *Nástroj akce drawer*, která se zobrazí v dolní části obrazovky (jak je znázorněno na pravé straně), umožňuje uživatelům si vybrat ze seznamu akce. 

![Navigační a akce zásobníky](intro-to-wear-images/drawers.png "navigaci a akce zásobníky")

Další informace o těchto dvou nové interaktivní zásobníky najdete v tématu Android [nosit navigaci a akce](https://developer.android.com/wear/preview/features/ui-nav-actions.html) tématu. 



#### <a name="curved-layouts"></a>Zakřivené rozložení 

Opotřebení 2.0 přináší nové funkce pro zobrazení zakřivené rozložení v zaokrouhlí opotřebení ze zařízení. Konkrétně nové `WearableRecyclerView` třída je optimalizována pro zobrazení svislé položek seznamu v zaokrouhlí zobrazí: 

![Příklad rozložení zakřivené](intro-to-wear-images/curved-layout.png "příklad zakřivené rozložení")

`WearableRecyclerView` rozšiřuje `RecyclerView` třídy pro podporu zakřivené rozložení a cyklické gesta posouvání. Další informace najdete v tématu Android [WearableRecyclerView](https://developer.android.com/reference/android/support/wearable/view/WearableRecyclerView.html) dokumentaci k rozhraní API. 



#### <a name="standalone-apps"></a>Samostatné aplikace 

Aplikace pro Android nosit 2.0 můžete pracovat nezávisle na kapesních aplikace. To znamená, že, například inteligentní sledování můžete pokračovat i v případě, že v doprovodném kapesní zařízení je vypnutý nebo daleko od wearable zařízení nabízí plnou funkčnost. Další informace o této funkci najdete v tématu Android [samostatné aplikace](https://developer.android.com/wear/preview/features/standalone-apps.html) tématu.



#### <a name="wrist-gestures"></a>Zápěstí gesta 

Zápěstí gesta umožňují uživatelům interakci s vaší aplikací bez použití dotykovou obrazovku &ndash; uživatelé mohli odpovídat na aplikace pomocí jednoho dlaně. Podporují se dva gesta zápěstí: 

-   Rychlý pohyb zápěstí out
-   Rychlý pohyb zápěstí v

Další informace najdete v tématu Android [zápěstí gesta](https://developer.android.com/wear/preview/features/gestures.html) tématu. 


Existuje mnoho funkcí další nosit 2.0 například vložené akce, inteligentní odpovědět, vzdálené vstup, rozbalených oznámeních a nový přemostění režim pro oznámení. Další informace o nových funkcích nosit 2.0, naleznete v části pro Android [přehled rozhraní API](https://developer.android.com/wear/preview/api-overview.html). 



## <a name="devices"></a>Zařízení

Tady jsou některé příklady zařízení, které můžou běžet Android nosit:

* [Motorola 360](https://moto360.motorola.com/)
* [Kukátko G kontaktní skupině](http://www.lg.com/us/smart-watches/lg-W100-g-watch)
* [Kukátko G kontaktní skupině R](http://www.lg.com/us/smartwatch/g-watch-r)
* [Samsung Gear Live](http://www.samsung.com/global/microsite/gear/gearlive_design.html)
* [Sony SmartWatch 3](http://www.sonymobile.com/global-en/products/smartwear/smartwatch-3-swr50/)
* [ASUS ZenWatch](http://www.asus.com/us/Phones/ASUS_ZenWatch_WI500Q/)



## <a name="further-reading"></a>Další čtení

Podívejte se na Google Android nosit dokumentaci:

* [O opotřebení Android](http://www.android.com/wear/)
* [Návrh aplikace Android opotřebení](https://developer.android.com/design/wear/index.html)
* [Knihovna Android.support.wearable ](https://developer.android.com/reference/android/support/wearable/view/package-summary.html)
* [Android Wear 2.0](https://developer.android.com/wear/preview/index.html)



## <a name="summary"></a>Souhrn

Tento úvod k dispozici přehled Android nosit. To uvedených základní funkce Android nosit a zahrnuty Přehled funkce byla zavedená v systému Android nosit 2.0. Ji poskytuje odkazy na základní čtení, což vývojářům začít s vývojem pro Xamarin.Android opotřebení a je uvedené příklady některých zařízení Android nosit aktuálně na trhu.


## <a name="related-links"></a>Související odkazy

- [Instalace a nastavení](~/android/wear/get-started/installation.md)
- [Začínáme](~/android/wear/get-started/index.md)
