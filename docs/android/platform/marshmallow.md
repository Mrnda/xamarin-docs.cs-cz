---
title: Funkce marshmallow
description: Tento článek vám pomůže začít používat pro vývoj aplikací pro Android 6.0 Marshmallow pomocí Xamarin.Android.
ms.prod: xamarin
ms.assetid: E4D6F183-98D2-460A-9D65-937639A899E0
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 03/01/2018
ms.openlocfilehash: 4f0bcd25662d3def719a89ccf833e845eb1728f2
ms.sourcegitcommit: b56b3f906d2c05a3f1be219ef41be8b79e519b8e
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/25/2018
ms.locfileid: "39242235"
---
# <a name="marshmallow-features"></a>Funkce marshmallow

_Tento článek vám pomůže začít používat pro vývoj aplikací pro Android 6.0 Marshmallow pomocí Xamarin.Android._

Tento článek poskytuje přehled nových funkcí v Androidu Marshmallow 6.0, vysvětluje postup přípravy Xamarin.Android pro vývoj pro Android Marshmallow a obsahuje odkazy na ukázkové aplikace, které ukazují, jak používat nové Android Marshmallow funkce v aplikacích pro Xamarin.Android. 


## <a name="overview"></a>Přehled

[Android 6.0 Marshmallow](http://developer.android.com/about/versions/marshmallow/index.html), je pro Android další hlavní verzi po Androidu Lollipop.
Xamarin.Android podporuje Android Marshmallow a zahrnuje:

-   **Rozhraní API 23/Android 6.0 vazby** &ndash; s Androidem 6.0 přidá mnoho nových rozhraní API pro nové funkce popsané níže; tato rozhraní API jsou dostupná pro aplikace Xamarin.Android, když Cílová úroveň rozhraní API 23. Další informace o rozhraních API pro Android 6.0, naleznete v tématu [rozhraní API s Androidem 6.0](http://developer.android.com/preview/api-overview.html). 

[![Tablety a telefony s Marshmallow Hero bitové kopie](marshmallow-images/android-m-hero-sml.png)](marshmallow-images/android-m-hero.png#lightbox)

I když Marshmallow vydání se zaměřuje především na "polština a kvalitu", poskytuje také mnoho nových funkcí, které vás zajímají pro vývojáře prostředí Xamarin.Android. Tyto funkce patří: 

-   **Oprávnění modulu runtime** &ndash; toto vylepšení umožňuje uživatelům schválení oprávnění zabezpečení pro případ od případu v době běhu. 

-   **Vylepšení ověřování** &ndash; od verze Android Marshmallow, aplikace teď můžete pomocí otisku prstu senzorů ověřovat uživatele a nový *ověřte přihlašovací údaje* funkce minimalizuje nutnost zadávání hesla. 

-   **Propojení App** &ndash; tato funkce pomáhá eliminovat nutnost s **výběr aplikace** vyvstane automaticky přidruží webové domény aplikace. 

-   **Přímé sdílení** &ndash; můžete definovat *přímé sdílení cíle* , které usnadňují sdílení intuitivní a rychlé uživatelům; tato funkce umožňuje uživatelé sdílet obsah s jinými aplikacemi. 

-   **Hlasové interakce** &ndash; toto nové rozhraní API vám umožní vytvářet konverzační hlasové funkce do vaší aplikace. 

-   **Režim zobrazení 4 KB** &ndash; v Androidu Marshmallow aplikaci požádat o rozlišení 4 K zobrazení na hardwaru, která ho podporuje. 

-   **Nové funkce zvuk** &ndash; počínaje Marshmallow, Android teď podporuje protokol MIDI. Nabízí taky nové třídy k vytvoření digitální zvukový zachytávání a přehrávání objektů a nabízí nové zachytávání rozhraní API pro přidružení zvuku a vstupní zařízení. 

-   **Nové funkce videa** &ndash; Marshmallow poskytuje novou třídu, která pomáhá aplikacím k vykreslení datových proudů audio a video synchronizované, tato třída také poskytuje podporu pro rychlost přehrávání dynamické. 

-   **Android for Work** &ndash; Marshmallow zahrnuje rozšířené ovládací prvky pro zařízení vlastněných společností, jednoho uživatele. Podporuje tichou instalaci a odinstalaci aplikací podle vlastníka zařízení, automatické přijetí aktualizací systému, vylepšené certifikát správy, sledování využití dat, správu oprávnění a oznámení o stavu pracovních. 

-   **Material Design Support Library** &ndash; nové *návrhu Support Library* poskytuje návrh komponent a vzory, které usnadňuje rychlé vytváření Material Design vzhled a chování do vaší aplikace. 

Kromě toho byly vydány mnoho aktualizací základní knihovna pro Android pomocí systému Android M a tyto aktualizace nabízí nové funkce systému Android M a dřívějších verzích Androidu.

Kromě toho mnoho aktualizací základní knihovna pro Android, které byly vydané s Androidem Marshmallow, a tyto aktualizace nabízí nové funkce pro Android Marshmallow a dřívějších verzích Androidu. Tento článek vysvětluje, jak začít vytvářením aplikací s Androidem Marshmallow, a nabízí že přehled o nové funkce zvýrazní v Android 6.0. 

## <a name="requirements"></a>Požadavky

Pokud chcete používat nové funkce Android Marshmallow v aplikacích založených na Xamarinu jsou vyžadovány následující položky: 

-   **Xamarin.Android** &ndash; Xamarin.Android 5.1.7.12 nebo novější musí být nainstalované a nakonfigurované pomocí sady Visual Studio nebo Xamarin Studio.

-   **Visual Studio pro Mac** nebo **sady Visual Studio** &ndash; Pokud používáte Visual Studio pro Mac verze 5.9.7.22 nebo novější je povinný. Pokud používáte Visual Studio, verze 3.11.1537 nebo novější nástroje Xamarin pro Visual Studio je povinný. 

-   **Sada SDK pro Android** &ndash; 6.0 Android SDK (API 23) nebo novější musí být nainstalován prostřednictvím správce sady Android SDK.

-   **Java Developer Kit** &ndash; Xamarin.Android vyžaduje [sadu JDK 1.8](http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html) nebo novější, pokud vyvíjíte pro úroveň rozhraní API 24 nebo větší (sadu JDK 1.8 také podporuje úrovně rozhraní API starších než 24, včetně Marshmallow). 64-bit verze sadu JDK 1.8 je povinné, pokud používáte vlastní ovládací prvky nebo prohlížeč formulářů.

Můžete dál používat [JDK 1.7](http://www.oracle.com/technetwork/java/javase/downloads/jdk7-downloads-1880260.html) vývoj speciálně pro rozhraní API úrovně 23 nebo dřívější. 


## <a name="getting-started"></a>Začínáme

Abyste mohli začít používat s Androidem Marshmallow s Xamarin.Android, musíte stáhnout a nainstalovat nejnovější nástroje a sady SDK balíčky, než budete moct vytvořit projekt Android Marshmallow: 

1.  Nainstalujte nejnovější aktualizace z Xamarin **stabilní** kanálu. 

2.  Instalace sady SDK pro Android Marshmallow 6.0 balíčky a nástrojů.

3.  Vytvoření nového projektu Xamarin.Android, který cílí na Android 6.0 Marshmallow (API úrovně 23). 

4.  Konfigurace zařízení nebo emulátoru systému Android marshmallow od.

Každý z těchto kroků je vysvětlené v následujících částech:


### <a name="install-xamarin-updates"></a>Aktualizace Xamarinu

Xamarin aktualizovat tak, že obsahují podporu systému Android marshmallow od 6.0, změňte kanálu aktualizace **stabilní** a nainstalujte všechny aktualizace. Další informace o instalaci aktualizace z kanálu aktualizace, naleznete v tématu [změnit kanálu aktualizace](https://github.com/xamarin/recipes/tree/master/Recipes/cross-platform/ide/change_updates_channel). 


### <a name="install-the-android-60-sdk"></a>Nainstalovat SDK pro Android 6.0

Do systému Android marshmallow od vytvoření projektu Xamarin.Android, musíte nejprve použít správce sady Android SDK nainstalovat SDK pro Android 6.0:

-   Spustit správce sady Android SDK (v sadě Visual Studio pro Mac, použijte **nástroje > správce sady SDK**; v sadě Visual Studio, použijte **nástroje > Android > správce sady Android SDK**) a nainstalujte nejnovější Android SDK Tools:

    [![Výběr nástroje sady Android SDK v správce sady Android SDK](marshmallow-images/mnc-preview-tools.png)](marshmallow-images/mnc-preview-tools.png#lightbox)

-   Také, nainstalujte nejnovější **s Androidem 6.0** balíčků sady SDK:

    [![Výběr sady SDK pro Android 6.0 balíčky Android SDK Manager](marshmallow-images/mnc-preview-packages.png)](marshmallow-images/mnc-preview-packages.png#lightbox)

Je nutné nainstalovat Android SDK Tools revize 24.3.4 nebo novější.
Další informace o použití Android SDK Manager k instalaci sady Android SDK 6.0 najdete v tématu [správce sady SDK](http://developer.android.com/tools/help/sdk-manager.html).



### <a name="start-a-xamarinandroid-project"></a>Spuštění projektu Xamarin.Android

Vytvoření nového projektu Xamarin.Android. Pokud jste ještě na vývoj pro Android s využitím kódu Xamarin, najdete v článku [Hello, Android](~/android/get-started/hello-android/index.md) Další informace o vytváření projekty Android. 

Když vytvoříte projekt pro Android, musíte nakonfigurovat nastavení verzí do cíle s Androidem MarshMallow 6.0. Pro Marshmallow cílit na váš projekt, musíte nakonfigurovat svůj projekt pro **rozhraní API úrovně 23 (Xamarin.Android 6.0 podporu)**. Další informace o konfiguraci úrovně rozhraní Android API úrovně, naleznete v tématu [Principy Android API úrovně](~/android/app-fundamentals/android-api-levels.md).



### <a name="configure-an-emulator-or-device"></a>Konfigurace zařízení nebo emulátoru

Pokud používáte emulátor, spusťte Správce AVD s Androidem a vytvoření nového zařízení pomocí následujících nastavení:

-   Zařízení: Nexus 5, 6 nebo 9.
-   Cíl: Android 6.0 – rozhraní API úrovně 23
-   ABI: x86

Tato virtuální zařízení je třeba nakonfigurovat pro emulaci Nexus 5:

[![Konfigurace AVD pomocí Nexus 5 zařízení s Androidem 6.0 cíl a Intel Atom (x86)](marshmallow-images/android-m-avd.png)](marshmallow-images/android-m-avd.png#lightbox)

Pokud používáte fyzické zařízení, jako je například Nexus 5, 6 nebo 9, můžete nainstalovat image ve verzi preview nástroje Android Marshmallow. Další informace o aktualizaci zařízení s Androidem Marshmallow, naleznete v tématu [bitové kopie systému hardwaru](http://developer.android.com/preview/download.html#images).



## <a name="new-features"></a>Nové funkce

Mnoho změny zavedené v Androidu Marshmallow se zaměřuje na zlepšení zkušeností uživatele Androidu, zvýšit výkon a opravu chyb. Marshmallow však také zavádí některé rozsáhlé změn se základními informacemi platformy Android. Následující části zvýrazněte tato vylepšení a poskytují odkazy vám pomůžou začít s použitím nové funkce s Androidem Marshmallow ve vaší aplikaci. 



### <a name="runtime-permissions"></a>Oprávnění modulu runtime

V systému Android oprávnění byla výrazně Optimalizovaná a zjednodušená od Androidu Lollipop. V Androidu Marshmallow uživatelům udělit oprávnění pro případ od případu za běhu, spíše než v čas instalace. Pro podporu této funkce v Androidu Marshmallow nebo novější, navrhněte aplikaci tak, aby zobrazit výzvu uživateli pro oprávnění za běhu (v rámci které se vyžadují oprávnění). Tato změna usnadňuje uživatelům začít používat aplikace okamžitě, protože zjednodušuje proces instalace a upgrade vaší aplikace. 

Zobrazit [žádosti o oprávnění modulu Runtime v Androidu Marshmallow](https://blog.xamarin.com/requesting-runtime-permissions-in-android-marshmallow/) další podrobnosti (včetně příkladů kódu) o implementaci oprávnění modulu Runtime v Xamarin.Android apps.
Xamarin poskytuje také ukázkovou aplikaci, která ukazuje, jak fungují oprávnění modulu runtime v Androidu Marshmallow (a novější): [RuntimePermissions](https://developer.xamarin.com/samples/monodroid/android-m/RuntimePermissions).

Tato ukázková aplikace ukazuje následující:

-   Návod k ověření a požádat o oprávnění v době běhu.
-   Jak deklarovat oprávnění pro zařízení s Androidem M.

Chcete-li použít tato ukázková aplikace:

1.  Klepněte **fotoaparát** nebo **kontakty** tlačítka pro zobrazení oprávnění vyžádat dialogového okna.
2.  Udělit oprávnění k zobrazení fragmentů fotoaparát nebo kontakty.

Další informace o nových funkcích oprávnění modulu runtime v Androidu Marshmallow najdete v tématu [práce s oprávněními systému](https://developer.android.com/preview/features/runtime-permissions.html).



### <a name="authentication-enhancements"></a>Vylepšení ověřování

Android Marshmallow zahrnuje dvě vylepšení ověřování, které pomohou eliminovat potřebu hesla:

-   **Otisků prstů ověřování** &ndash; k ověřování uživatelů používá skenování otisků prstů.

-   **Zkontrolujte přihlašovací údaje** &ndash; ověřuje uživatele, založené na jak dlouho zařízení byl odemčen.

Odkazy a ukázkové aplikace popsané dále pomáhají známý se s těmito novými funkcemi.


#### <a name="fingerprint-authentication"></a>Ověřování pomocí otisku prstu

Na zařízení, která podporují otisk prstu skenování hardwaru, můžete použít novou `FingerPrintManager` třídy k ověření uživatele.
Další informace o funkci ověřování otiskem prstu v Androidu Marshmallow najdete v tématu [ověřování pomocí otisku prstu](https://developer.android.com/preview/api-overview.html#fingerprint-authentication).

Xamarin poskytuje ukázkovou aplikaci, která ukazuje, jak používat registrované otisky prstů k ověření uživatele ve vaší aplikaci: [FingerprintDialog](https://developer.xamarin.com/samples/monodroid/android-m/FingerprintDialog).

Chcete-li použít tato ukázková aplikace:

1.  Touch **nákupní** tlačítka otevřete dialogové okno ověřování otiskem prstu.
2.  Kontrola v registrovaných prstu k ověření.

Všimněte si, že tato ukázková aplikace vyžaduje zařízení se čtečkou otisků prstů.
Tato aplikace neukládá váš otisk prstu (nebo heslo).



#### <a name="voice-interactions"></a>Hlasové interakce

Nové funkce hlasové interakce zavedené v Androidu Marshmallow umožňuje uživatelům vaší aplikace použití hlasu k potvrzení akce a vyberte ze seznamu možností. Další informace o hlasové interakce najdete v tématu [přehled rozhraní API hlasové interakce](https://developers.google.com/voice-actions/interaction/). 

V tématu [konverzaci přidat do aplikace pro Android s hlasové interakce](https://blog.xamarin.com/add-a-conversation-to-your-android-app-with-voice-interactions/) najdete další podrobnosti (včetně příkladů kódu) implementace hlasové interakce v Xamarin.Android apps.
Ukázková aplikace je k dispozici, který ukazuje, jak použít rozhraní API hlasové interakce v aplikaci Xamarin.Android: [hlasové interakce](https://github.com/jamesmontemagno/MarshmallowSamples/tree/master/VoiceInteractions).



#### <a name="confirm-credential"></a>Zkontrolujte přihlašovací údaje

Pomocí nové *ověřte přihlašovací údaje* funkce systému Android Marshmallow můžete uvolnit uživatelé nebudou muset pamatovat si a zadání hesel konkrétní aplikace je založena na jak dlouho byl odemčen jejich zařízení prostřednictvím.
K tomuto účelu můžete použít novou `SetUserAuthenticationValidityDurationSeconds` metodu `KeyGenerator`. Použití `KeyGuardManager`společnosti `CreateConfirmDeviceCredentialIntent` metoda opakované ověření uživatele z vaší aplikace. Další informace o této nové funkce v Androidu Marshmallow najdete v tématu [ověřte přihlašovací údaje](https://developer.android.com/preview/api-overview.html#confirm-credential).

Xamarin poskytuje ukázkovou aplikaci, která ukazuje, jak používat přihlašovací údaje zařízení (například kód PIN, vzor nebo heslo) ve vaší aplikaci: [ConfirmCredential](https://developer.xamarin.com/samples/monodroid/android-m/ConfirmCredential/)

Chcete-li použít tato ukázková aplikace:

1.  Nastavení zabezpečené uzamčení obrazovky na zařízení (**Secure > zabezpečení > Screenlock**).
2.  Klepněte **nákupní** tlačítko a ověřte přihlašovací údaje zabezpečení zámku obrazovky.



### <a name="chrome-custom-tabs"></a>Chrome vlastní karty

Vývojáři aplikací pro rozpoznávání tváře zvolit po klepnutí adresu URL: aplikaci můžete spustit prohlížeč nebo pomocí prohlížeče v aplikaci na základě `WebView`. Obě možnosti nést výzvy &ndash; spouští se prohlížeč je těžké kontextu přepínač, který není přizpůsobitelný při `WebView`s nesdílejí stavu pomocí prohlížeče. Také můžete použít z `WebView`s můžete přidat další údržby režii. 

*Chrome vlastní karty* umožňuje snadno a elegantně zobrazit webů s využitím výkonu Chrome bez nutnosti opustit aplikaci uživatelům. Tato funkce poskytuje aplikaci další kontrolu nad komfortem při uživatelské web; Zkontrolujte přechody mezi nativním a webový obsah bezproblémové bez nutnosti uchýlit se k `WebView`. Vaše aplikace může také ovlivnit, jak Chrome vypadá a funguje přizpůsobením následující: 

-   Barva nástrojů

-   Nastavení a ukončení animace

-   Vlastní akce v nabídce nástrojů a přetečení Chrome

-   Před spuštěním a obsahu před načtením (pro rychlejší načítání) pro Chrome

Chcete-li využít výhod této funkce v aplikaci Xamarin.Android, stáhněte a nainstalujte [karty knihovna pro Android podporu vlastní](https://www.nuget.org/packages/Xamarin.Android.Support.CustomTabs/).
Další informace o této funkci najdete v tématu [Chrome vlastní karty](https://developer.chrome.com/multidevice/android/customtabs).



### <a name="material-design-support-library"></a>Knihovna podpory Material Design

Androidu Lollipop zavedené [Material Design](http://www.google.com/design/spec/material-design/introduction.html) jako nový jazyk návrhu aktualizovat prostředí Android (naleznete v tématu [motiv Material](~/android/user-interface/material-theme.md) informace o používání specifikacím material design v Xamarin.Android apps). S Androidem Marshmallow, Google zavedené *Android návrhu Support Library* aby bylo snazší pro aplikace aby museli vývojáři uplatňovat materiálu navrhnout vzhled a chování. Tato knihovna obsahuje následující součásti:

-   **CoordinatorLayout** &ndash; nové `CoordinatorLayout` widgetu je podobný, ale výkonnější než `FrameLayout`. Můžete použít `CoordinatorLayout` jako kontejner pro podřízené zobrazení, nebo jako rozložení nejvyšší úrovně a poskytuje `layout_anchor` atribut, který slouží k zobrazení ukotvení vzhledem k dalším zobrazením.

-   **Sbalování panelů nástrojů** &ndash; nové `CollapsingToolbarLayout` je sbalení panelu aplikace, který tvoří obálku pro `Toolbar`. (Všimněte si, že *panel aplikace* je, co bylo dříve označované jako *panel akcí*.)

-   **Plovoucí tlačítko akce** &ndash; kulaté tlačítko, která označuje primární akce na rozhraní vaší aplikace.

-   **Číslo s plovoucí čárkou popisky pro úpravy textu** &ndash; používá nový `TextInputLayout` widgetu (která zabalí `EditText`) zobrazíte popisek s plovoucí desetinnou čárkou, když pomocného parametru je skrytý, pokud uživatel zadá text.

-   **Zobrazení navigace** &ndash; nové `NavigationView` widgetu vám pomůže používat navigační panel tak, aby je uživatelé přejít.

-   **Snackbar** &ndash; nové `SnackBar` je widget (podobně jako oznámení) lehký zpětnou vazbu mechanismus, který zobrazí zprávu (BRIEF) v dolní části obrazovky, povolí nad všemi dalšími prvky na obrazovce.

-   **Materiál karty** &ndash; nové `TabLayout` pomůcky máte k dispozici vodorovné rozložení pro zobrazení tabulek jako způsob, jak implementovat navigace nejvyšší úrovně ve vaší aplikaci.

Abyste mohli využívat [návrhu Support Library](http://developer.android.com/tools/support-library/features.html#design) v aplikaci Xamarin.Android, stáhněte a nainstalujte Xamarin [Xamarin podporu knihovny návrhu](https://www.nuget.org/packages/Xamarin.Android.Support.Design/) balíček NuGet.

Zobrazit [krásné Material Design s knihovnou Android podporu návrhu](https://blog.xamarin.com/add-beautiful-material-design-with-the-android-support-design-library/) další podrobnosti (včetně příkladů kódu), o použití knihovny podporu návrhu materiál do aplikace Xamarin.Android.
Xamarin poskytuje ukázkovou aplikaci, která demos nový návrh Androidu knihovny v Xamarin.Android &ndash; [Cheesesquare](https://developer.xamarin.com/samples/monodroid/android5.0/Cheesesquare).
Tato ukázka demonstruje následující funkce knihovny návrhu:


-   Sbalení panelu nástrojů
-   Plovoucí tlačítko akce
-   Zobrazení ukotvení
-   NavigationView
-   Snackbar

Další informace o knihovně návrhu, naleznete v tématu [Android návrhu Support Library](http://android-developers.blogspot.co.at/2015/05/android-design-support-library.html) v blogu Android Developer.


### <a name="additional-library-updates"></a>Další knihovny aktualizace

Kromě s Androidem Marshmallow Google oznámilo související aktualizace na několik jader s Androidem knihovny. Xamarin poskytuje podporu Xamarin.Android pro tyto aktualizace přes několik balíčků NuGet verzi preview: 

-   [Služby Google Play](https://www.nuget.org/packages?q=Xamarin+Google+Play+Services) &ndash; nejnovější verzi služby Google Play obsahuje novou *aplikace vyzve* funkci, která umožňuje uživatelům sdílet své aplikace s přáteli. Další informace o této funkci najdete v tématu [záběr rozbalte vaší aplikace Google aplikace vyzve](http://blog.xamarin.com/expand-your-apps-reach-with-googles-app-invites/). 

-   [Podpůrné knihovny pro Android](https://www.nuget.org/packages?q=xamarin+support+library) &ndash; funkce, které jsou k dispozici pouze pro knihovny rozhraní API poskytuje zpětně kompatibilní verze rozhraní Android API nabízejí tyto balíčky Nuget. 

-   [Knihovna pro Android přenosném](https://www.nuget.org/packages/Xamarin.Android.Wear) &ndash; tento NuGet zahrnuje vazby služby Google Play. Nejnovější verzi knihovny přenosném přináší nové funkce (včetně zjednodušují navigaci pro vlastní aplikace) pro platformu Android Wear. 


## <a name="summary"></a>Souhrn

Tento článek zavedená s Androidem Marshmallow a bylo vysvětleno, jak nainstalovat a nakonfigurovat na Marshmallow nejnovější nástroje a balíčky pro vývoj pro Xamarin.Android. Poskytuje také přehled o nejvíce skvělým novým funkcím Android Marshmallow pro vývoj pro Xamarin.Android.


## <a name="related-links"></a>Související odkazy

- [Android 6.0 Marshmallow](http://developer.android.com/about/versions/marshmallow/index.html)
- [Získání sady Android SDK](https://developer.android.com/sdk/index.html#Other)
- [Přehled funkcí](https://developer.android.com/preview/api-overview.html)
- [Zpráva k vydání verze](https://developer.xamarin.com/releases/android/xamarin.android_5/xamarin.android_5.1.99/)
- [RuntimePermissions (ukázka)](https://developer.xamarin.com/samples/monodroid/android-m/RuntimePermissions)
- [ConfirmCredential (ukázka)](https://developer.xamarin.com/samples/monodroid/android-m/ConfirmCredential)
- [FingerprintDialog (ukázka)](https://developer.xamarin.com/samples/monodroid/android-m/FingerprintDialog)
