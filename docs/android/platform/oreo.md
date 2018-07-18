---
title: Funkce Oreo
description: Jak začít používat Xamarin.Android pro vývoj aplikací pro nejnovější verzi Androidu.
ms.prod: xamarin
ms.assetid: EAEF7341-7A00-4439-9FAF-43882637BEF8
ms.technology: xamarin-android
ms.custom: video
author: mgmclemore
ms.author: mamcle
ms.date: 07/06/2018
ms.openlocfilehash: af560848240fec9558cc63969bcc269eedbd5424
ms.sourcegitcommit: 632955f8cdb80712abd8dcc30e046cb9c435b922
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/10/2018
ms.locfileid: "37947283"
---
# <a name="oreo-features"></a>Funkce Oreo

_Jak začít používat Xamarin.Android pro vývoj aplikací pro nejnovější verzi Androidu._

[Android 8.0 Oreo](https://developer.android.com/index.html) je k dispozici z Googlu nejnovější verzi Androidu. Android Oreo nabízí mnoho nových funkcí, které vás zajímají pro vývojáře prostředí Xamarin.Android. Tyto funkce patří kanály oznámení, oznámení, oznámení, vlastní písma v XML, ke stažení písma, automatické vyplňování a picture in picture (PIP). Android Oreo zahrnuje nové rozhraní API pro tyto nové capabilties a tato rozhraní API jsou dostupná pro aplikace Xamarin.Android při použití Xamarin.Android 8.0 a novější.

[![Android Oreo ústřední obrázek](oreo-images/01-android-o-logo-sml.png)](oreo-images/01-android-o-logo.png#lightbox)

Tento článek je strukturována tak, aby vám pomohli při vývoji aplikace Xamarin.Android pro Android 8.0 Oreo. Vysvětluje, jak nainstalovat potřebné aktualizace, nakonfigurujte sadu SDK a vytvořte emulátoru (nebo zařízení) pro účely testování. Nabízí také přehled nových funkcí v Androidu 8.0 Oreo s odkazy na ukázkové aplikace, které ukazují, jak používat funkce Android Oreo do aplikace Xamarin.Android.


## <a name="requirements"></a>Požadavky

Pokud chcete používat Android Oreo funkce v aplikacích založených na Xamarinu jsou vyžadovány následující položky:

-   **Visual Studio** &ndash; Pokud používáte Windows, je vyžadována verze 15.5 nebo novější sady Visual Studio.  Pokud používáte počítač Mac, Visual Studio pro Mac verze 7.2.0 je povinný.

-   **Xamarin.Android** &ndash; Xamarin.Android 8.0 nebo novější musí být nainstalované a nakonfigurované pomocí sady Visual Studio.

-   **Sada SDK pro Android** &ndash; Android SDK 8.0 (API 26) nebo novější musí být nainstalován prostřednictvím správce sady Android SDK.



## <a name="getting-started"></a>Začínáme

Abyste mohli začít používat Android Oreo s Xamarin.Android, musíte stáhnout a nainstalovat nejnovější nástroje a sady SDK balíčky, než budete moct vytvořit projekt Android Oreo:

1. Aktualizovat na nejnovější verzi sady Visual Studio.

2. Nainstalujte **Android 8.0.0 (API 26)** nebo novější balíčky a nástrojů přes správce sady SDK.

3. Vytvoření nového projektu Xamarin.Android, který cílí na Android Oreo (API 26).

4. Nakonfigurujte emulátoru nebo zařízení pro testování aplikací pro Android Oreo.

Každý z těchto kroků je vysvětlené v následujících částech:



### <a name="update-visual-studio-and-xamarinandroid"></a>Aktualizace sady Visual Studio a Xamarin.Android

Přidání podpory pro Android Oreo do sady Visual Studio, postupujte takto:

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

-   Pokud používáte Visual Studio 2017: 

    1. Aktualizace na Visual Studio 2017 verze 15.7 nebo novější (viz [aktualizace sady Visual Studio 2017](https://docs.microsoft.com/visualstudio/install/update-visual-studio)).

    2. Použití [správce sady SDK](~/android/get-started/installation/android-sdk.md) instalace úrovně rozhraní API 26.0 nebo vyšší.

-   Pokud používáte sadu Visual Studio 2015 jsme doporučujeme downgradu SDK Tools 25 a pomocí staré Google Emulator manager grafického uživatelského rozhraní. Nástroje sady SDK 25 pořád můžou použít společně s 26 rozhraní API 27 a novější a nebude mít vliv na vývoj řešení pro nové platformy. Tím získáte rozhraní pro správu vaší sady Android SDK pro starší verzi sady VS.

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

-   Aktualizovat na nejnovější stabilní verze sady Visual Studio 2017 pro Mac, jak je vysvětleno v [aktualizace sady Visual Studio pro Mac](https://docs.microsoft.com/visualstudio/mac/update).

-----

Další informace o podpoře Xamarin pro Android Oreo, najdete v článku [zpráva k vydání verze Xamarin.Android 8.0](https://developer.xamarin.com/releases/android/xamarin.android_8/xamarin.android_8.0/).



### <a name="install-the-android-sdk"></a>Instalace sady Android SDK

Vytvoření projektu s Xamarin.Android 8.0, musíte nejprve použít Xamarin Android SDK Manager k instalaci sady SDK platformy pro **Android 8.0 – Oreo** nebo novější. Je také nutné nainstalovat Android SDK Tools 26.0 nebo novější.

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

1. Spustit správce sady SDK (v sadě Visual Studio, klikněte na tlačítko **nástroje > Android > správce sady Android SDK**).

2. Nainstalujte **Android 8.0 – Oreo** balíčky. Pokud používáte emulátor sady Android SDK, je potřeba zahrnout **x86** bitové kopie systému, které budete potřebovat:

    [![Výběr Android 8.0 balíčky Android SDK Manager](oreo-images/win/01-android-o-packages.png)](oreo-images/win/01-android-o-packages.png#lightbox)

3. Nainstalujte **Android SDK Tools 26.0.2** nebo novější, **platformy – nástroje sady Android SDK 26.0.0** nebo novější, a **sestavení-nástroje sady Android SDK 26.0.0** (nebo novější):

    [![Vyberte Android SDK Tools 26 ve správci sady Android SDK](oreo-images/win/02-sdk-tools.png)](oreo-images/win/02-sdk-tools.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

1. Spustit správce sady SDK (v sadě Visual Studio pro Mac, klikněte na tlačítko **nástroje > správce sady SDK**).

2. Nainstalujte **Android 8.0 – Oreo** balíčků sady SDK. Pokud používáte emulátor sady Android SDK, je potřeba zahrnout **x86** bitové kopie systému, které budete potřebovat:

    [![Výběr balíčků s Androidem 8.0 ve správci sady SDK](oreo-images/mac/01-android-o-packages.png)](oreo-images/mac/01-android-o-packages.png#lightbox)

3. Nainstalujte **Android SDK Tools 26.0.2** nebo novější, **platformy – nástroje sady Android SDK 26.0.0** nebo novější, a **sestavení-nástroje sady Android SDK 26.0.0** (nebo novější):

    [![Vyberte Android SDK Tools 26 ve správci sady SDK](oreo-images/mac/02-sdk-tools.png)](oreo-images/mac/02-sdk-tools.png#lightbox)

-----



### <a name="start-a-xamarinandroid-project"></a>Spuštění projektu Xamarin.Android

Vytvoření nového projektu Xamarin.Android. Pokud jste ještě na vývoj pro Android s využitím kódu Xamarin, najdete v článku [Hello, Android](~/android/get-started/hello-android/index.md) Další informace o vytváření projekty Xamarin.Android.

Když vytvoříte projekt pro Android, musíte nakonfigurovat nastavení verzí pro cílový Android 8.0 nebo novější. Například pokud chcete zaměřit svůj projekt pro Android 8.0, musíte nakonfigurovat úroveň rozhraní Android API cílový projekt tak, aby **Android 8.0 (API 26)**. Doporučuje se nastavit také cílovou úrovní rozhraní API 26 nebo novější. Další informace o konfiguraci úrovně rozhraní Android API úrovně, naleznete v tématu [Principy Android API úrovně](~/android/app-fundamentals/android-api-levels.md).


### <a name="configure-an-emulator-or-device"></a>Konfigurace zařízení nebo emulátoru

Pokud se pokusíte spustit využívající grafické rozhraní Google AVD Manager výchozí po instalaci Android SDK Tools 26.0 nebo později, může se zobrazit následující chyba dialog, který dává pokyn k použití nástroje příkazového řádku AVD manager **avdmanager** místo :

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

![Dialogové okno upozornění správce emulátoru androidu](oreo-images/win/03-avd-warning.png)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

![Dialogové okno upozornění správce emulátoru androidu](oreo-images/mac/03-avd-warning.png)

-----

Tato zpráva je zobrazena, protože Google už obsahuje samostatných grafické uživatelské rozhraní AVD Manageru, která podporuje rozhraní API 26.0 a novější. Pro Android 8.0 Oreo, je nutné použít Správce emulátoru Xamarin Androidu nebo příkazového řádku `avdmanager` nástroj k vytvoření virtuálního zařízení pro Android Oreo.

Chcete-li vytvořit a spravovat virtuální zařízení pomocí Správce zařízení s Androidem, najdete v článku [spravovat virtuální zařízení pomocí Správce zařízení s Androidem](~/android/get-started/installation/android-emulator/device-manager.md).
Pokud chcete vytvořit virtuální zařízení bez Správce zařízení s Androidem, postupujte podle kroků v další části.


#### <a name="creating-virtual-devices-using-avdmanager"></a>Vytváří se virtuální zařízení používat avdmanager

Chcete-li použít **avdmanager** k vytvoření nové virtuální zařízení, postupujte podle těchto kroků:

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

1.  Otevřete okno příkazového řádku a nastavte `JAVA_HOME` umístění sady Java SDK ve vašem počítači. Při typické instalaci Xamarin můžete použít následující příkaz:

    ```cmd
    setx JAVA_HOME "C:\Program Files\Java\jdk1.8.0_131"
    ```

2.  Přidat umístění sady Android SDK `bin` složku pro váš `PATH`.
    Při typické instalaci Xamarin můžete použít následující příkaz:

    ```cmd
    setx PATH "%PATH%;C:\Program Files (x86)\Android\android-sdk\tools\bin"
    ```

3.  Zavřete okno příkazového řádku a otevřete nové okno příkazového řádku. Vytvoření nového virtuálního zařízení s použitím [avdmanager](https://developer.android.com/studio/command-line/avdmanager.html) příkazu. Například k vytvoření AVD s názvem **AVD. Oreo 8.0** x86 pomocí image systému pro úroveň rozhraní API 26, použijte následující příkaz:

    ```cmd
    avdmanager create avd -n AVD-Oreo-8.0 -k "system-images;android-26;google_apis;x86"
    ```

4.  Po zobrazení výzvy s **chcete vytvořit vlastní hardwarového profilu [Žádný]** můžete zadat **žádné** a přijměte výchozí profil hardwaru. Pokud pro sdělení **Ano**, **avdmanager** vás vyzve k seznam otázek pro přizpůsobení hardwarový profil.

Poté co **avdmanager** k vytvoření virtuálního zařízení, budou zahrnuty v rozevírací nabídce zařízení:

[![Nové AVD přidány do zařízení rozevírací nabídky](oreo-images/win/04-android-o-avd-sml.png)](oreo-images/win/04-android-o-avd.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

1.  Otevřít **terminálu** okna a změňte umístění adresáře nástroje sady Android SDK na vašem počítači Mac. Při typické instalaci Xamarin můžete použít následující příkaz:

    ```bash
    cd ~/Library/Developer/Xamarin/android-sdk-macosx/tools/bin
    ```

2.  Vytvoření nového virtuálního zařízení s použitím [avdmanager](https://developer.android.com/studio/command-line/avdmanager.html) příkazu. Například k vytvoření AVD s názvem **AVD. Oreo 8.0** x86 pomocí image systému pro úroveň rozhraní API 26, použijte následující příkaz:

    ```bash
    avdmanager create avd -n AVD-Oreo-8.0 -k "system-images;android-26;google_apis;x86"
    ```

3.  Po zobrazení výzvy s **chcete vytvořit vlastní hardwarového profilu [Žádný]** můžete zadat **žádné** a přijměte výchozí profil hardwaru. Pokud pro sdělení **Ano**, **avdmanager** vás vyzve k seznam otázek pro přizpůsobení hardwarovém profilu.

Když použijete **avdmanager** k vytvoření virtuálního zařízení, budou zahrnuty v rozevírací nabídce zařízení:

[![Nové AVD přidány do zařízení rozevírací nabídky](oreo-images/mac/04-android-o-avd-sml.png)](oreo-images/mac/04-android-o-avd.png#lightbox)

-----

Další informace o konfiguraci nástroje pro testování a ladění v emulátoru Androidu najdete v tématu [ladění v emulátoru Android](~/android/deploy-test/debugging/debug-on-emulator.md).

Pokud používáte fyzické zařízení, jako je například Nexus nebo Pixel, můžete buď aktualizujte svoje zařízení prostřednictvím automaticky přes aktualizace air (OTA) nebo stáhnout bitovou kopii systému a flash zařízení přímo. Další informace o ruční aktualizaci zařízení na Android Oreo najdete v tématu [objekt pro vytváření bitových kopií pro Nexus a pixelů zařízení](https://developers.google.com/android/images).



## <a name="new-features"></a>Nové funkce

Android Oreo zavádí řadu nových funkcí a možností, jako je například kanály oznámení, oznámení odznáčků, vlastní písma ve formátu XML, ke stažení písem, automatické vyplňování a obrázek v obrázku. V dalších částech zvýrazněte tyto funkce a poskytují odkazy vám pomůžou začít používat je ve vaší aplikaci.



### <a name="notification-channels"></a>Kanály oznámení

*Kanály oznámení* jsou aplikace definované kategorie pro oznámení.
Můžete vytvořit kanál oznámení pro každý typ oznámení, které je nutné odeslat, a můžou vytvářet kanály oznámení tak, aby odrážely volby provedené uživatelů vaší aplikace. Nová funkce kanály oznámení umožňuje poskytnout uživatelům velice přesně kontrolovat různé druhy oznámení. Například Pokud implementujete aplikaci zasílání zpráv, můžete vytvořit kanály samostatné oznámení pro každou skupinu konverzace, která je vytvořená uživatelem.

[Kanály oznámení](~/android/app-fundamentals/notifications/local-notifications.md#notif-chan) vysvětluje, jak vytvořit kanál oznámení a jeho použití pro odesílání místní oznámení. Příklad kódu reálného světa, najdete v článku [NotificationChannels](https://developer.xamarin.com/samples/monodroid/android-o/NotificationChannels) ukázka; Tato ukázková aplikace spravuje dva kanály a nastaví možnosti Další oznámení.



### <a name="notification-badges"></a>Oznámení Odznáčků

Odznáčky oznámení jsou malé tečky, které se zobrazují nad ikony aplikace, jak je znázorněno na tomto snímku obrazovky:

[![Příklad oznámení odznáčků na ikony aplikace](oreo-images/02-badges-sml.png)](oreo-images/02-badges.png#lightbox)

Tyto tečky značí, že se nové oznámení pro jeden nebo více kanálů oznámení do aplikace přidružené k této aplikaci ikony &ndash; jde o upozornění, která ještě není uživatel zavře nebo reagovali na ni. Uživatelé můžou dlouho-stisknutím klávesy na ikonu okamžitý přehled o oznámení související s odznáčkem oznámení, zavření nebo funguje na oznámení z nabídky stiskněte dlouho této appeaars.

Další informace o odznáčky oznámení, naleznete v části Android Developer [oznámení Odznáčků](https://developer.android.com/guide/topics/ui/notifiers/notifications.html#Badges) tématu.



### <a name="custom-fonts-in-xml"></a>Vlastní písma ve formátu XML

Android Oreo zavádí *písma v XML*, takže je možné začlenit vlastní písma jako prostředky. OpenType (**.otf**) a TrueType (**ttf**) jsou podporované formáty. Přidání písma jako prostředků, postupujte takto:

1. Vytvoření **prostředky/písma** složky.

2. Zkopírujte soubory písma (například **ttf** a **.otf** souborů) do **prostředky/písma**. 

3. V případě potřeby přejmenovat každý soubor písma tak, že dodržuje zásady vytváření názvů Android soubor (například použití pouze malá *a – z*, *0-9*a podtržítka. v názvech souborů). Například soubor písma `Pacifico-Regular.ttf` může být přejmenován na něco jako `pacifico.ttf`.

4. Použít vlastní písma s použitím nového `android:fontFamily` atribut do rozložení XML. Například následující `TextView` deklarace používá přidání **pacifico.ttf** písmo prostředků:

   ```xml
   <TextView
     android:text="Example Text in Pacifico Regular"
     android:layout_width="wrap_content"
     android:layout_height="wrap_content"
     android:fontFamily="@font/pacifico" />
   ```

Můžete také vytvořit písmo řady soubor XML, který popisuje několik písma také stylu a tloušťky podrobnosti. Další informace najdete v části Android Developer [písma v XML](https://developer.android.com/guide/topics/ui/look-and-feel/fonts-in-xml.html) tématu.


### <a name="downloadable-fonts"></a>Ke stažení písma

Od verze Android Oreo, aplikace můžete požádat písma zprostředkovatele spíše než sdružování do APK. Písma se stáhnou ze sítě podle potřeby. Tato funkce se zmenší velikost APK, zachování dat využití paměti a mobilní telefon. Tuto funkci můžete použít také ve verzích rozhraní Android API 14 a vyšší nainstalováním balíček Android podporu knihovně 26.

Když vaše aplikace potřebuje písmo, můžete vytvořit `FontsRequest` (určující písmem, které chcete stáhnout) a pak ji předejte do `FontsContract` metoda chcete stáhnout. Následující kroky popisují proces stahování písma podrobněji:

1.  Vytvořit instanci [FontRequest](https://developer.android.com/reference/android/provider/FontRequest.html) objektu. 

2.  Podtřídy a vytvoření instance [FontsContract.FontRequestCallback](https://developer.android.com/reference/android/provider/FontsContract.FontRequestCallback.html).

3.  Implementace [FontRequestCallback.OnTypeFaceRetrieved](https://developer.android.com/reference/android/provider/FontsContract.FontRequestCallback.html#onTypefaceRetrieved%28android.graphics.Typeface%29) metodu, která se používá ke zpracování dokončení požadavku písma.

4.  Implementace [FontRequestCallback.OnTypeFaceRequestFailed](https://developer.android.com/reference/android/provider/FontsContract.FontRequestCallback.html#onTypefaceRequestFailed%28int%29) metodu, která slouží k informování aplikace všech chyb, provést během zpracování žádosti o písma.

5.  Volání [FontsContract.RequestFonts](https://developer.android.com/reference/android/provider/FontsContract.html#requestFonts(android.content.Context,%20android.provider.FontRequest,%20android.os.Handler,%20android.os.CancellationSignal,%20android.provider.FontsContract.FontRequestCallback)) metodu pro načtení písmo v poskytovateli písma. 

Cuando llama al método `RequestFonts`, primero verifica si la fuente está localmente en caché (desde una llamada anterior a `RequestFont`). Další informace o nové spuštění limity na pozadí, naleznete v části Android Developer `OnTypeFaceRetrieved`omezení zpracování na pozadí tématu.

Vzorek [Stahovatelné písma](https://developer.xamarin.com/samples/monodroid/android-o/DownloadableFonts) ukazuje, jak používat funkci Stahovatelné písma, která byla představena v systému Android Oreo. 

Další informace o stahování písem naleznete v tématu Témata ke [Stahovatelné písma](https://developer.android.com/guide/topics/ui/look-and-feel/downloadable-fonts.html) pro vývojáře Android.



### <a name="autofill"></a>Při vytváření kanálu oznámení se místo toho nastavte úroveň doporučené důležitosti.

Nový rámec _automatického vyplňování_ v systému Android Oreo usnadňuje uživatelům zvládat opakované úkoly, jako je přihlašovací údaje, vytváření účtů a transakce s kreditními kartami. Pro aplikace zaměřené na Androidu Oreo  nefunguje z důvodu nového omezení umístit services spuštěna na pozadí. Pokud se zaměřujete na Androidu Oreo, měli byste použít PendingIntent.GetBroadcast místo.

Vzor [AutofillFramework](https://developer.xamarin.com/samples/monodroid/android-o/AutoFillFramework/) demonstruje použití modulu Autofill Framework. Několik ukázky Xamarin.Android jsou k dispozici ukazují, jak využít výhod funkcí Android Oreo.

Další informace o nové funkci [Automatické vyplňování](https://developer.android.com/guide/topics/text/autofill.html) a o tom, jak optimalizovat aplikaci pro automatické vyplňování, naleznete v tématu Tvorba automatizovaných aplikací pro vývojáře systému Android.



### <a name="picture-in-picture-pip"></a>Tato ukázka spravuje dva kanály oznámení: jednu s výchozí význam a druhý s vysokou důležitostí.

PictureInPicture ukazuje základní použití režimu obrázku v obrázku (PiP) pro kapesní zařízení zavedený Oreo. AutofillFramework demonstruje použití rámce automatického vyplňování.

Ke stažení písma poskytuje příklad, jak používat funkce ke stažení písma popsané výše.

```xml
android:supportsPictureInPicture
```

Chcete-li určit, jak se má vaše činnost chovat, když je v režimu PIP, použijte nový objekt [PictureInPictureParams](https://developer.android.com/reference/android/app/PictureInPictureParams.html). `PictureInPictureParams` Tuto knihovnu můžete zabránit aplikaci ze zobrazení chybějících emoji znaků jako "tofu" znaků. Následující nové metody PIP byly přidány k `Activity` Android Oreo:

-   [Služba umístění aktualizace popředí](https://developer.android.com/reference/android/app/Activity.html#enterPictureInPictureMode%28android.app.PictureInPictureParams%29) znázorňuje, jakým způsobem má být aktualizován o umístění zařízení pomocí služby popředí provázaná a začít používat rozhraní API pro umístění. Android 8.0 Oreo vývoj pomocí jazyka C#

-   [SetPictureInPictureParams](https://developer.android.com/reference/android/app/Activity.html#setPictureInPictureParams%28android.app.PictureInPictureParams%29)&ndash; Aktualizuje nastavení konfigurace PIP aktivity (například změna poměru stran).

Vzor [PictureInPicture](https://developer.xamarin.com/samples/monodroid/android-o/PictureInPicture) demonstruje základní použití režimu Picture-in-Picture (Picture-in-Picture) pro přenosné zařízení představené v Oreo. Zahrnuté odkazy na dokumentaci k rozhraní API a Android Developer témat, která vám pomůže začít pracovat v vytváření aplikací pro Android Oreo.



### <a name="other-features"></a>Další funkce

Aplikace Android Oreo obsahuje mnoho dalších nových funkcí, jako je knihovna podpory Emoji, rozhraní API pro umístění, omezení na pozadí, širokou škálu barev pro aplikace, nové zvukové kodeky, vylepšení WebView, vylepšenou navigaci pomocí klávesnice a nové rozhraní API pro audio -výkon s nízkou latencí zvuku, Další informace o těchto funkcích naleznete v tématu Funkce Android Developer [Android Oreo Features and APIs](https://developer.android.com/about/versions/oreo/android-8.0.html).



## <a name="behavior-changes"></a>Android 8.0 Oreo

Android Oreo zahrnuje širokou škálu systémových a změny chování rozhraní API, které můžou mít vliv na funkce nasazovaných aplikací tak existující. Tyto změny jsou popsány následujícím způsobem.


### <a name="background-execution-limits"></a>Omezení zpracování na pozadí

Vylepšit uživatelské prostředí, má aplikace Android Oreo omezení Co může aplikace provádět při běhu na pozadí. Například pokud uživatel se sledováním videa nebo hraní her, aplikace běžící na pozadí by mohla narušit výkon video náročné aplikace spuštěná v popředí. V důsledku toho Android Oreo umístí následující omezení na aplikací, které nejsou přímo interakci s uživatelem:

1.  **Omezení služby na pozadí** &ndash; když aplikace běží na pozadí, má okno několik minut, ve kterém je stále povoleno vytváření a používání služby. Na konci tohoto okna, Android, zastaví službu na pozadí aplikace a považuje se _nečinnosti_.

2.  **Vysílání omezení** &ndash; Android 7.0 (rozhraní API 25) přikládá všesměrové vysílání, které aplikace registruje omezení. Android Oreo provádí přísnější těchto omezení. Například Android Oreo aplikace už zaregistrovat přijímače všesměrového vysílání pro implicitní vysílání ve svých manifestech.

Další informace o nové spuštění limity na pozadí, naleznete v části Android Developer [omezení zpracování na pozadí](https://developer.android.com/about/versions/oreo/background.html) tématu.


### <a name="breaking-changes"></a>Nejnovější změny

Aplikace, které cílí na Android Oreo nebo vyšší, musíte upravit jejich aplikací pro podporu následujících změn, kde je to možné:

- Android Oreo zastarání možnost nastavit prioritu jednotlivých oznámení. Při vytváření kanálu oznámení se místo toho nastavte úroveň doporučené důležitosti. Úroveň důležitosti, které přiřadíte do kanálu oznámení se vztahuje na všechny zprávy oznámení, které pošlete do něj.

- Pro aplikace zaměřené na Androidu Oreo `PendingIntent.GetService()` nefunguje z důvodu nového omezení umístit services spuštěna na pozadí. Pokud se zaměřujete na Androidu Oreo, měli byste použít [PendingIntent.GetBroadcast](https://developer.xamarin.com/api/member/Android.App.PendingIntent.GetBroadcast/p/Android.Content.Context/System.Int32/Android.Content.Intent/Android.App.PendingIntentFlags/) místo.  


## <a name="sample-code"></a>Ukázkový kód

Několik ukázky Xamarin.Android jsou k dispozici ukazují, jak využít výhod funkcí Android Oreo.

-   [NotificationsChannels](https://developer.xamarin.com/samples/monodroid/android-o/NotificationChannels) ukazuje, jak používat nové kanály oznámení zavedené v Androidu Oreo. Tato ukázka spravuje dva kanály oznámení: jednu s výchozí význam a druhý s vysokou důležitostí.

-   [PictureInPicture](https://developer.xamarin.com/samples/monodroid/android-o/PictureInPicture) ukazuje základní použití režimu obrázku v obrázku (PiP) pro kapesní zařízení zavedený Oreo. Zahrnuté odkazy na dokumentaci k rozhraní API a Android Developer témat, která vám pomůže začít pracovat v vytváření aplikací pro Android Oreo.

-   [AutofillFramework](https://developer.xamarin.com/samples/monodroid/android-o/AutoFillFramework) demonstruje použití rámce automatického vyplňování. Několik ukázky Xamarin.Android jsou k dispozici ukazují, jak využít výhod funkcí Android Oreo.

-   [Ke stažení písma](https://developer.xamarin.com/samples/monodroid/android-o/DownloadableFonts) poskytuje příklad, jak používat funkce ke stažení písma popsané výše.

-   [EmojiCompat](https://developer.xamarin.com/samples/monodroid/android-o/EmojiCompat) demonstruje použití EmojiCompat podporu knihovny. Tuto knihovnu můžete zabránit aplikaci ze zobrazení chybějících emoji znaků jako "tofu" znaků.

-   [Aktualizace umístění Pendingintent](https://developer.xamarin.com/samples/monodroid/android-o/AndroidPlayLocation/LocUpdPendIntent) ukazuje využití rozhraní API pro umístění má být aktualizován o umístění zařízení pomocí `PendingIntent`.

-   [Služba umístění aktualizace popředí](https://developer.xamarin.com/samples/monodroid/android-o/AndroidPlayLocation/LocUpdFgService) znázorňuje, jakým způsobem má být aktualizován o umístění zařízení pomocí služby popředí provázaná a začít používat rozhraní API pro umístění.


## <a name="video"></a>Video

> [!VIDEO https://youtube.com/embed/OuvEcaMO-Ho]

**Android 8.0 Oreo vývoj pomocí jazyka C#**


## <a name="summary"></a>Souhrn

Tento článek zavedené Android Oreo a bylo vysvětleno, jak nainstalovat a nakonfigurovat nejnovější nástroje a balíčky pro vývoj pro Xamarin.Android v Androidu Oreo. Přehled klíčových funkcí, které jsou k dispozici v Androidu Oreo je k dispozici s odkazy na ukázkovém zdrojovém kódu pro několik nových funkcí. Zahrnuté odkazy na dokumentaci k rozhraní API a Android Developer témat, která vám pomůže začít pracovat v vytváření aplikací pro Android Oreo. Také zvýrazněna nejdůležitější změny Android Oreo chování, které by mohly mít vliv na stávající aplikace.


## <a name="related-links"></a>Související odkazy

- [Android 8.0 Oreo](https://developer.android.com/index.html)
