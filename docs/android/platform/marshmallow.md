---
title: Funkce marshmallow
description: Tento článek vám pomůže začít používat pomocí Xamarin.Android k vývoji aplikací pro Android 6.0 Marshmallow.
ms.prod: xamarin
ms.assetid: E4D6F183-98D2-460A-9D65-937639A899E0
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 03/01/2018
ms.openlocfilehash: d2150e18a377d61a2e79fabfc845f57cfab8a5c7
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/04/2018
ms.locfileid: "30774945"
---
# <a name="marshmallow-features"></a>Funkce marshmallow

_Tento článek vám pomůže začít používat pomocí Xamarin.Android k vývoji aplikací pro Android 6.0 Marshmallow._

Tento článek poskytuje přehled nových funkcí v systému Android Marshmallow 6.0, vysvětluje, jak připravit Xamarin.Android pro vývoj pro Android Marshmallow a poskytuje odkazy na ukázkové aplikace, které ukazují, jak využívat nové Android Marshmallow funkce v aplikacích pro Xamarin.Android. 


## <a name="overview"></a>Přehled

[Android 6.0 Marshmallow](http://developer.android.com/about/versions/marshmallow/index.html), je další Android hlavní verzi po Android typu Lupa.
Xamarin.Android podporuje Android Marshmallow a zahrnuje:

-   **Rozhraní API 23/Android 6.0 vazby** &ndash; Android 6.0 přidá mnoho nových rozhraní API pro nové funkce popsané níže; tato rozhraní API jsou k dispozici pro aplikace Xamarin.Android při cíle 23 úroveň rozhraní API. Další informace o rozhraní API systému Android 6.0 najdete v tématu [rozhraní API systému Android 6.0](http://developer.android.com/preview/api-overview.html). 

[![Nejdůležitější bitové kopie tablety a telefony s Marshmallow](marshmallow-images/android-m-hero-sml.png)](marshmallow-images/android-m-hero.png#lightbox)

I když Marshmallow verze se zaměřuje především na "polština a kvality", umožňuje také mnoha nových funkcí, které vás zajímají vývojářům Xamarin.Android. Tyto funkce patří: 

-   **Modul runtime oprávnění** &ndash; toto vylepšení umožňuje uživatelům schválit oprávnění zabezpečení pro případ od případu v době běhu. 

-   **Vylepšení ověřování** &ndash; od verze Android Marshmallow, aplikace teď můžete použít snímače otisků prstů k ověření uživatelů a novou *potvrďte pověření* funkce snižuje nutnost zadávání hesla. 

-   **Propojování aplikace** &ndash; tato funkce vám pomůže omezit potřebě mít **aplikace výběru** otevíraném automaticky přidružením aplikace webových domén. 

-   **Přímé sdílení** &ndash; můžete definovat *přímé sdílení cíle* které usnadňují sdílení rychlé a intuitivní uživatelům; tato funkce umožňuje uers sdílet obsah s jinými aplikacemi. 

-   **Hlas interakce** &ndash; toto nové rozhraní API umožňuje sestavovat funkce hlasového konverzačního do vaší aplikace. 

-   **Režim zobrazení 4 KB** &ndash; v Android Marshmallow vaše aplikace může požádat o 4 K rozlišení obrazovky na hardwaru, která ho podporuje. 

-   **Nové funkce audia** &ndash; počínaje Marshmallow, Android teď podporuje protokol MIDI. Poskytuje také nové třídy pro vytvoření digitální zvuku a přehrávání a nabízí nové háky rozhraní API pro přidružení audia a vstupní zařízení. 

-   **Nové funkce videa** &ndash; Marshmallow poskytuje novou třídu, která pomáhá aplikace vykreslení audio a video datové proudy synchronizované; Tato třída rovněž poskytuje podporu pro dynamické přehrávání rychlost. 

-   **Android for Work** &ndash; Marshmallow zahrnuje rozšířené ovládací prvky pro zařízení ve vlastnictví firmy, jednoho uživatele. Podporuje bezobslužnou instalaci a odinstalaci aplikace je vlastník zařízení, automatické přijetí aktualizací systému, vylepšené certifikát správy, data využití sledování, správu oprávnění a oznámení o stavu pracovních. 

-   **Knihovna podpory podstatným návrhu** &ndash; nové *knihovna podpory návrhu* poskytuje součásti návrhu a vzorů, které usnadňuje sestavení materiálu návrhu vzhled a chování do vaší aplikace. 

Kromě toho mnoho aktualizací základní knihovna pro Android, které byly vydané s Androidem M a tyto aktualizace poskytovat nové funkce systému Android M a dřívějších verzích systému Android.

Kromě toho mnoho aktualizací základní knihovna pro Android, které byly vydané s Android Marshmallow a tyto aktualizace zadejte nové funkce pro Android Marshmallow a dřívějších verzích systému Android. Tento článek vysvětluje, jak začít vytvářet aplikace s Android Marshmallow a nabízí že přehled nové funkce jsou vysvětlené v Android 6.0. 

## <a name="requirements"></a>Požadavky

Toto je potřeba použít nové funkce systému Android Marshmallow v aplikace založené na Xamarinu: 

-   **Xamarin.Android** &ndash; Xamarin.Android 5.1.7.12 nebo novější musí být nainstalovaná a nakonfigurovaná s Visual Studio nebo Xamarin Studio.

-   **Visual Studio pro Mac** nebo **Visual Studio** &ndash; Pokud používáte Visual Studio pro Mac, verze 5.9.7.22 nebo novější je požadovaná. Pokud používáte Visual Studio, verze 3.11.1537 nebo novější nástroje Xamarin pro Visual Studio se vyžaduje. 

-   **Sady SDK pro Android** &ndash; Android SDK 6.0 (API 23) nebo novější musí být nainstalován prostřednictvím Android SDK Manager.

-   **Sady pro vývojáře Java** &ndash; Xamarin.Android vyžaduje [JDK 1.8](http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html) nebo novější, pokud vyvíjíte pro úroveň rozhraní API 24 nebo větší (JDK 1.8 také podporuje úrovně rozhraní API starší než 24, včetně Marshmallow). 64bitové verze JDK 1.8 je vyžaduje, pokud používáte vlastní ovládací prvky nebo prohlížeč formulářů.

Můžete dál používat [JDK 1.7](http://www.oracle.com/technetwork/java/javase/downloads/jdk7-downloads-1880260.html) Pokud vývoj speciálně pro úroveň rozhraní API 23 nebo starším. 


## <a name="getting-started"></a>Začínáme

Abyste mohli začít používat Android Marshmallow s Xamarin.Android, musíte stáhnout a nainstalovat nejnovější nástroje a balíčky SDK před vytvořením projektu Android Marshmallow: 

1.  Nainstalujte nejnovější aktualizace Xamarin z **stabilní** kanál. 

2.  Instalace nástrojů a SDK pro Android Marshmallow 6.0 balíčky.

3.  Vytvoření nového projektu Xamarin.Android s cílem Android 6.0 Marshmallow (API 23 úroveň). 

4.  Konfigurace pro Android Marshmallow emulátoru nebo zařízení.

Každý z těchto kroků je vysvětlené v následujících částech:


### <a name="install-xamarin-updates"></a>Nainstalovat Xamarin aktualizace

Chcete-li aktualizovat Xamarin tak, že obsahují podporu systému Android marshmallow od 6.0, změňte aktualizace kanál, ke **stabilní** a nainstalovat všechny aktualizace. Další informace o instalaci aktualizace z tohoto kanálu aktualizace najdete v tématu [změnit kanál aktualizace](https://developer.xamarin.com/recipes/cross-platform/ide/change_updates_channel/). 


### <a name="install-the-android-60-sdk"></a>Instalace sady SDK Android 6.0

K vytvoření projektu Xamarin.Android pro Android Marshmallow, musíte nejprve použít Android SDK Manager k instalaci sady SDK pro Android 6.0:

-   Spustit Android SDK Manager (v sadě Visual Studio pro Mac, použijte **nástroje > SDK Manager**; v sadě Visual Studio, použijte **nástroje > Android > Android SDK Manager**) a nainstalujte nejnovější nástroje pro Android SDK:

    [![Nástroje pro výběr sady SDK pro Android v Android SDK Manager](marshmallow-images/mnc-preview-tools.png)](marshmallow-images/mnc-preview-tools.png#lightbox)

-   Také nainstalovat nejnovější **Android 6.0** SDK balíčky:

    [![Výběr sady SDK pro Android 6.0 balíčky v Android SDK Manager](marshmallow-images/mnc-preview-packages.png)](marshmallow-images/mnc-preview-packages.png#lightbox)

Je nutné nainstalovat nástroje pro Android SDK revize 24.3.4 nebo novější.
Další informace o použití Android SDK Manager k instalaci sady SDK pro Android 6.0 najdete v tématu [SDK Manager](http://developer.android.com/tools/help/sdk-manager.html).



### <a name="start-a-xamarinandroid-project"></a>Zahájení projektu Xamarin.Android

Vytvoření nového projektu Xamarin.Android. Pokud jste pro vývoj pro Android pomocí Xamarinu nové, přečtěte si téma [Hello, Android](~/android/get-started/hello-android/index.md) Další informace o vytváření projektů Android. 

Když vytvoříte projekt pro Android, musíte nakonfigurovat nastavení verze Android 6.0 MarshMallow cíl. Chcete-li cíle projektu pro systém Marshmallow, je nutné nakonfigurovat svůj projekt pro **úroveň rozhraní API 23 (Xamarin.Android verze 6.0 podporu)**. Další informace o konfiguraci úrovní úrovně rozhraní API systému Android, najdete v části [Principy Android API úrovně](~/android/app-fundamentals/android-api-levels.md).



### <a name="configure-an-emulator-or-device"></a>Konfigurace zařízení nebo emulátoru

Pokud používáte emulátor, spusťte Správce AVD Android a vytvoření nového zařízení s následujícím nastavením:

-   Zařízení: Nexus 5, 6 nebo 9.
-   Cíl: Android 6.0 - úroveň rozhraní API 23
-   ABI: x86

Tato virtuální zařízení je například nakonfigurován emulovat Nexus 5:

[![Konfigurace AVD pomocí zařízení Nexus 5, cílového Android 6.0 a Intel Atom (x86)](marshmallow-images/android-m-avd.png)](marshmallow-images/android-m-avd.png#lightbox)

Pokud používáte fyzické zařízení, jako je například Nexus 5, 6 nebo 9, můžete nainstalovat bitovou kopii preview systému Android Marshmallow. Další informace o aktualizaci vašeho zařízení na Android Marshmallow najdete v tématu [bitové kopie systému hardwaru](http://developer.android.com/preview/download.html#images).



## <a name="new-features"></a>Nové funkce

Mnoho změn byla zavedená v systému Android Marshmallow jsou zaměřené na vylepšení zkušeností uživatelů zařízení s Androidem, zvýšení výkonu a opravě chyb. Marshmallow však zavedli taky některé rozsáhlé změny na základní informace o platformě Android. Následující části zvýrazněte tato vylepšení a obsahují odkazy na vám pomůže začít pomocí nové funkce systému Android Marshmallow ve vaší aplikaci. 



### <a name="runtime-permissions"></a>Modul runtime oprávnění

V systému Android oprávnění je výrazně Optimalizovaná a zjednodušená od typu Android Lupa. V systému Android Marshmallow uživatelům udělit oprávnění na základě případ od případu za běhu, spíše než v době instalace. Pro podporu této funkce na Android Marshmallow nebo novější, navrhněte své aplikace a zobrazit výzvu uživateli pro oprávnění za běhu (v rámci kterých jsou potřeba oprávnění). Tato změna usnadňuje uživatelům začít používat aplikaci okamžitě, protože se zjednodušuje proces instalace a upgradu vaší aplikace. 

V tématu [požaduje oprávnění Runtime v systému Android Marshmallow](https://blog.xamarin.com/requesting-runtime-permissions-in-android-marshmallow/) další podrobnosti (včetně příkladů kódu) o implementaci Runtime oprávnění v aplikacích pro Xamarin.Android.
Xamarin také poskytuje ukázkové aplikace, která ukazuje, jak fungují runtime oprávnění v Android Marshmallow (nebo novější): [RuntimePermissions](https://developer.xamarin.com/samples/monodroid/android-m/RuntimePermissions).

Tato ukázková aplikace ukazuje následující:

-   Postup kontroly a žádosti o oprávnění za běhu.
-   Jak deklarovat oprávnění pro zařízení s Androidem M.

Použití této ukázkové aplikace:

1.  Klepněte **fotoaparát** nebo **kontakty** tlačítka se zobrazí oprávnění vyžádat dialogové okno.
2.  Udělit oprávnění k zobrazení fragmenty fotoaparátu nebo kontakty.

Další informace o nových funkcích oprávnění runtime v systému Android Marshmallow najdete v tématu [práce oprávnění systému](https://developer.android.com/preview/features/runtime-permissions.html).



### <a name="authentication-enhancements"></a>Vylepšení ověřování

Android Marshmallow obsahuje dvě vylepšení ověřování, které pomáhají eliminují nutnost použití hesla:

-   **Čtečka otisků ověřování** &ndash; kontrolu otisk prstu se používá k ověřování uživatelů.

-   **Potvrďte pověření** &ndash; ověřuje uživatele založené na tom, jak dlouho byl odemčen zařízení.

Odkazy a ukázkových aplikací, které jsou popsána dále můžou pomoct se známé se tyto nové funkce.


#### <a name="fingerprint-authentication"></a>Otisk prstu ověřování

Na zařízení, která podporují otisk prstu skenování hardwaru, můžete použít nové `FingerPrintManager` třída k ověření uživatele.
Další informace o funkci otisk prstu ověřování v systému Android Marshmallow najdete v tématu [otisk prstu ověřování](https://developer.android.com/preview/api-overview.html#fingerprint-authentication).

Xamarin obsahuje ukázkovou aplikaci, která ukazuje, jak používat registrované otisky prstů k ověření uživatele ve vaší aplikaci: [FingerprintDialog](https://developer.xamarin.com/samples/monodroid/android-m/FingerprintDialog).

Použití této ukázkové aplikace:

1.  Touch **nákupu** tlačítko otevřete dialogové okno ověřování otisků prstů.
2.  Kontrola v registrovaných prstu k ověření.

Všimněte si, že tato ukázková aplikace vyžaduje zařízení s čtečka otisků prstů.
Tato aplikace nejsou uložené otisk prstu (nebo heslo).



#### <a name="voice-interactions"></a>Hlasové interakce

Funkci nové hlasové interakce byla zavedená v systému Android Marshmallow umožňuje uživatelům vaší aplikace slouží k potvrzení akce a vyberte ze seznamu možností hlasu. Další informace o interakcích hlasové najdete v tématu [přehled rozhraní API interakce hlasové](https://developers.google.com/voice-actions/interaction/). 

V tématu [přidat konverzace do vaší aplikace Android pomocí hlasového interakce](https://blog.xamarin.com/add-a-conversation-to-your-android-app-with-voice-interactions/) další podrobnosti (včetně příkladů kódu) o implementaci interakcí hlasové v aplikacích pro Xamarin.Android.
Ukázkové aplikace je k dispozici, ukazuje, jak používat rozhraní API interakce hlasové v aplikaci Xamarin.Android: [hlasové interakce](https://github.com/jamesmontemagno/MarshmallowSamples/tree/master/VoiceInteractions).



#### <a name="confirm-credential"></a>Potvrďte přihlašovacích údajů

Pomocí nové *potvrďte pověření* funkce systému Android Marshmallow, můžete uvolnit uživatelé nebudou muset mějte na paměti a zadejte specifické pro aplikace hesla tak, že je založena na jak dlouho byl odemčen jejich zařízení.
K tomuto účelu můžete použít novou `SetUserAuthenticationValidityDurationSeconds` metodu `KeyGenerator`. Použití `KeyGuardManager`na `CreateConfirmDeviceCredentialIntent` metoda opakované ověření uživatele z vaší aplikace. Další informace o této nové funkce v systému Android Marshmallow najdete v tématu [potvrďte pověření](https://developer.android.com/preview/api-overview.html#confirm-credential).

Xamarin obsahuje ukázkovou aplikaci, která ukazuje, jak používat přihlašovací údaje zařízení (například PIN kód, vzor nebo heslo) ve vaší aplikaci: [ConfirmCredential](https://developer.xamarin.com/samples/monodroid/android-m/ConfirmCredential/)

Použití této ukázkové aplikace:

1.  Nastavení zabezpečení zámek obrazovky na vašem zařízení (**zabezpečeného > zabezpečení > Screenlock**).
2.  Klepněte **nákupu** tlačítko a potvrďte pověření zabezpečení zámku obrazovky.



### <a name="chrome-custom-tabs"></a>Vlastní karty Chrome

Vývojáři aplikací čelí vybrat, když uživatel klepne na adresu URL: aplikace můžete buď spustit prohlížeč, nebo použijte prohlížeče v aplikaci na základě `WebView`. Obě možnosti nést výzvy, &ndash; spuštění prohlížeče je těžká kontextu přepínač, který není přizpůsobitelné, při `WebView`s nesdílejí stavu pomocí prohlížeče. Navíc použití `WebView`s můžete přidat další údržby režii. 

*Vlastní karty pro Chrome* umožňuje snadno a který elegantně zobrazit weby s výkonným Chrome bez nutnosti vaši uživatelé nechte vaší aplikace. Tato funkce poskytuje aplikace větší kontrolu nad uživatele webové prostředí; Ujistěte se přechodů mezi nativní a webový obsah plynulejší bez nutnosti uchýlit k `WebView`. Aplikace může ovlivnit také jak Chrome vypadá a funguje přizpůsobením následující: 

-   Barva panelu nástrojů

-   Zadejte a ukončení animace

-   Vlastní akce v nabídce panelu nástrojů a přetečení Chrome

-   Před zahájení a obsahu před načtením (pro rychlejší načítání) pro Chrome

Chcete-li tuto funkci využít v aplikaci Xamarin.Android, stáhněte a nainstalujte [karty knihovna pro Android podporu vlastní](https://www.nuget.org/packages/Xamarin.Android.Support.CustomTabs/).
Další informace o této funkci najdete v tématu [Chrome vlastní karty](https://developer.chrome.com/multidevice/android/customtabs).



### <a name="material-design-support-library"></a>Knihovna podpory podstatným návrhu

Android typu Lupa zavedená [materiálu návrhu](http://www.google.com/design/spec/material-design/introduction.html) jako o nový jazyk návrhu aktualizace prostředí Android (najdete v části [materiálu motiv](~/android/user-interface/material-theme.md) informace o používání podstatným návrhu v aplikacích pro Xamarin.Android). Pomocí Android Marshmallow Google zavedená *knihovnu podpory Android návrhu* aby bylo snazší pro aplikaci vývojáři přijmout materiálu návrh vzhledu a chování. Tato knihovna obsahuje následující součásti:

-   **CoordinatorLayout** &ndash; nové `CoordinatorLayout` pomůcky je podobná ale výkonnější než `FrameLayout`. Můžete použít `CoordinatorLayout` jako kontejner pro podřízené zobrazení, nebo jako nejvyšší úrovně rozložení a poskytuje `layout_anchor` atribut, který slouží k zobrazení ukotvení relativně k jiným zobrazením.

-   **Sbalení panely nástrojů** &ndash; nové `CollapsingToolbarLayout` je ztenčeného panel aplikace, který představuje obálku pro `Toolbar`. (Všimněte si, že *indikátor aplikace* je, co byl dříve označované jako *panelu akcí*.)

-   **Plovoucí tlačítko akce** &ndash; zaokrouhlí tlačítko, které označuje primární akce na rozhraní vaší aplikace.

-   **Plovoucí popisky pro úpravy textu** &ndash; používá nový `TextInputLayout` pomůcky (která zabalí `EditText`) zobrazíte plovoucí štítek při nápovědu je skrytý, pokud uživatel zadá text.

-   **Zobrazení navigace** &ndash; nové `NavigationView` pomůcky vám pomůže používat panel navigační způsobem, který usnadňuje navigace.

-   **Snackbar** &ndash; nové `SnackBar` je widget (podobně jako oznámení) lightweight zpětnou vazbu mechanismus, který zobrazí krátkou zprávu v dolní části obrazovky, zobrazování nad všemi další prvky na obrazovce.

-   **Podstatným karty** &ndash; nové `TabLayout` pomůcky poskytuje vodorovném rozložení pro zobrazení karet jako způsob, jak implementovat navigaci na nejvyšší úrovně ve vaší aplikaci.

Chcete využít výhod [knihovna podpory návrhu](http://developer.android.com/tools/support-library/features.html#design) v aplikaci Xamarin.Android stáhnout a nainstalovat Xamarin [Xamarin podpory knihovny návrhu](https://www.nuget.org/packages/Xamarin.Android.Support.Design/) balíček NuGet.

V tématu [Krásný materiálu návrh s knihovnou Android návrhu podporu](https://blog.xamarin.com/add-beautiful-material-design-with-the-android-support-design-library/) další podrobnosti (včetně příkladů kódu) o použití knihovny materiálu návrhu podpory v aplikacích pro Xamarin.Android.
Xamarin obsahuje ukázkovou aplikaci, která demos nové knihovny návrh Androidu na Xamarin.Android &ndash; [Cheesesquare](https://developer.xamarin.com/samples/monodroid/android5.0/Cheesesquare).
Tento příklad znázorňuje následující funkce sady knihovně návrhu:


-   Sbalení panelu nástrojů
-   Plovoucí tlačítko akce
-   Zobrazení ukotvení
-   NavigationView
-   Snackbar

Další informace o knihovně návrhu najdete v tématu [knihovnu podpory Android návrhu](http://android-developers.blogspot.co.at/2015/05/android-design-support-library.html) v blogu Android Developer.


### <a name="additional-library-updates"></a>Další knihovny aktualizace

Kromě Android Marshmallow oznámila Google související aktualizace pro několik základní knihovny systému Android. Xamarin poskytuje Xamarin.Android podporu pro tyto aktualizace prostřednictvím několik balíčků NuGet verzi preview: 

-   [Služby Google Play](https://www.nuget.org/packages?q=Xamarin+Google+Play+Services) &ndash; nejnovější verze služby Google Play zahrnuje nové *žádostí aplikace* funkci, která umožňuje uživatelům sdílet své aplikace s přátel. Další informace o této funkci najdete v tématu [Reach rozšířit vaše aplikace s žádostí aplikace Google](http://blog.xamarin.com/expand-your-apps-reach-with-googles-app-invites/). 

-   [Android podpory knihovny](https://www.nuget.org/packages?q=xamarin+support+library) &ndash; tyto NuGets nabízejí funkce, které jsou dostupné jenom pro knihovnu rozhraní API při současném poskytování zpětně kompatibilní verze rozhraní Android API. 

-   [Knihovna pro Android Wearable](https://www.nuget.org/packages/Xamarin.Android.Wear) &ndash; tento NuGet zahrnuje vazby služby Google Play. Nejnovější verzi wearable knihovny přináší nové funkce (včetně usnadnil navigaci pro vlastní aplikace) pro platformu Android nosit. 


## <a name="summary"></a>Souhrn

Tento článek zavedená Android Marshmallow a vysvětlení, jak nainstalovat a nakonfigurovat na Marshmallow nejnovější nástroje a balíčky pro vývoj pro Xamarin.Android. Tu taky přehled nejvíce skvělé nové funkce systému Android Marshmallow pro vývoj pro Xamarin.Android.


## <a name="related-links"></a>Související odkazy

- [Android 6.0 Marshmallow](http://developer.android.com/about/versions/marshmallow/index.html)
- [Získání sady SDK pro Android](https://developer.android.com/sdk/index.html#Other)
- [Přehled funkcí](https://developer.android.com/preview/api-overview.html)
- [Zpráva k vydání verze](https://developer.xamarin.com/releases/android/xamarin.android_5/xamarin.android_5.1.99/)
- [RuntimePermissions (ukázka)](https://developer.xamarin.com/samples/monodroid/android-m/RuntimePermissions)
- [ConfirmCredential (ukázka)](https://developer.xamarin.com/samples/monodroid/android-m/ConfirmCredential)
- [FingerprintDialog (ukázka)](https://developer.xamarin.com/samples/monodroid/android-m/FingerprintDialog)
