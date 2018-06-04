---
title: Funkce Oreo
description: Jak začít vyvíjet aplikace pro nejnovější verzi systému Android pomocí Xamarin.Android.
ms.prod: xamarin
ms.assetid: EAEF7341-7A00-4439-9FAF-43882637BEF8
ms.technology: xamarin-android
ms.custom: video
author: mgmclemore
ms.author: mamcle
ms.date: 05/30/2018
ms.openlocfilehash: 3776a0554e5ae496f9e39612ec9bab971c6f1f88
ms.sourcegitcommit: a7febc19102209b21e0696256c324f366faa444e
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 06/04/2018
ms.locfileid: "34732759"
---
# <a name="oreo-features"></a>Funkce Oreo

_Jak začít vyvíjet aplikace pro nejnovější verzi systému Android pomocí Xamarin.Android._

[Android 8.0 Oreo](https://developer.android.com/index.html) najdete nejnovější verze Android na webu Google. Android Oreo obsahuje mnoho nových funkcí, které vás zajímají vývojářům Xamarin.Android. Tyto funkce patří kanály oznámení, oznámení odznaky, vlastní písem v XML, zaváděná písma, automatické vyplňování a obrázek obrázku (PIP). Android Oreo zahrnuje nové rozhraní API pro tyto nové capabilties a tato rozhraní API jsou k dispozici pro aplikace Xamarin.Android při použití Xamarin.Android 8.0 a novější.

[![Android Oreo nejdůležitější image](oreo-images/01-android-o-logo-sml.png)](oreo-images/01-android-o-logo.png#lightbox)

Tento článek je strukturovaná můžete začít pracovat v vývoj Xamarin.Android aplikací pro Android 8.0 Oreo. Vysvětluje, jak nainstalovat potřebné aktualizace, konfigurace sady SDK a vytvořte emulátoru (nebo zařízení) pro testování. Také poskytuje přehled nových funkcí v systému Android Oreo 8.0, s odkazy na ukázkových aplikací, které ukazují, jak používat funkce Android Oreo v aplikacích pro Xamarin.Android.


## <a name="requirements"></a>Požadavky

Toto je potřeba používat aplikace pro Xamarin Android Oreo funkce:

-   **Visual Studio** &ndash; Pokud používáte systém Windows, je vyžadována verze 15,5 nebo novější sady Visual Studio.  Pokud používáte algoritmu Mac, je třeba Visual Studio pro Mac verze 7.2.0.

-   **Xamarin.Android** &ndash; Xamarin.Android 8.0 nebo novější musí být nainstalovaná a nakonfigurovaná pomocí sady Visual Studio.

-   **Sady SDK pro Android** &ndash; Android SDK 8.0 (26 rozhraní API) nebo novější musí být nainstalován prostřednictvím Android SDK Manager.



## <a name="getting-started"></a>Začínáme

Abyste mohli začít používat Android Oreo s Xamarin.Android, musíte stáhnout a nainstalovat nejnovější nástroje a balíčky SDK před vytvořením projektu Android Oreo:

1. Aktualizovat na nejnovější verzi sady Visual Studio.

2. Nainstalujte **Android 8.0.0 (rozhraní API 26)** nebo novější balíčky a nástroje prostřednictvím sady SDK Manager.

3. Vytvoření nového projektu Xamarin.Android s cílem Android Oreo (26 rozhraní API).

4. Nakonfigurujte emulátoru nebo zařízení pro testování aplikací pro Android Oreo.

Každý z těchto kroků je vysvětlené v následujících částech:



### <a name="update-visual-studio-and-xamarinandroid"></a>Aktualizace sady Visual Studio a Xamarin.Android

Přidání podpory Android Oreo do sady Visual Studio, postupujte takto:

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

-   Pokud používáte Visual Studio 2017: 

    1. Aktualizace Visual Studio 2017 verze 15,5 nebo novější (viz [aktualizace Visual Studio 2017](https://docs.microsoft.com/en-us/visualstudio/install/update-visual-studio)).

    2. Pomocí Správce SDK [pokyny k instalaci](~/android/get-started/installation/android-sdk.md#installation) instalace nástroje Xamarin SDK Manager.
       Xamarin SDK Manager musíte nainstalovat, protože Google neposkytuje samostatnou SDK grafického uživatelského rozhraní správce, který podporuje rozhraní API 26.0 a novější.

-   Pokud používáte Visual Studio 2015, jsme doporučujeme, abyste přechod na starší verzi sady SDK nástroje na 25 a pomocí Správce staré Google emulátor grafickým uživatelským rozhraním. Nástroje sady SDK 25 můžete stále používat společně se rozhraní API 26, 27 a novější a nebude mít vliv na vývoj pro nové platformy. Tím získáte rozhraní pro správu vaší Android SDK pro starší verze VS.

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

-   Aktualizovat na nejnovější stabilní verze Visual Studio 2017 pro Mac, jak je popsáno v [aktualizace Visual Studio pro Mac](https://docs.microsoft.com/en-us/visualstudio/mac/update).

-----

Další informace o podpoře Xamarin pro Android Oreo najdete v tématu [poznámky k verzi Xamarin.Android 8.0](https://developer.xamarin.com/releases/android/xamarin.android_8/xamarin.android_8.0/).



### <a name="install-the-android-sdk"></a>Instalace sady SDK pro Android

Vytvoření projektu s Xamarin.Android 8.0, musíte nejprve použít Xamarin Android SDK Manager k instalaci sady SDK platforma pro **Android 8.0 - Oreo** nebo novější. Je také nutné nainstalovat Android SDK Tools 26.0 nebo novější.

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

1. Spusťte správce SDK (v sadě Visual Studio, klikněte na tlačítko **nástroje > Android > Android SDK Manager**).

2. Nainstalujte **Android 8.0 - Oreo** balíčky. Pokud používáte sady SDK pro Android emulátoru, nezapomeňte zahrnout **x86** bitové kopie systému, které budete potřebovat:

    [![Výběr Android 8.0 balíčky v Android SDK Manager](oreo-images/win/01-android-o-packages.png)](oreo-images/win/01-android-o-packages.png#lightbox)

3. Nainstalujte **Android SDK Tools 26.0.2** nebo novější, **Android SDK nástrojů platformy 26.0.0** nebo novější, a **Android SDK – nástroje sestavení 26.0.0** (nebo novější):

    [![Výběr nástrojů sady SDK pro Android 26 ve správci sady SDK pro Android](oreo-images/win/02-sdk-tools.png)](oreo-images/win/02-sdk-tools.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

1. Spusťte správce SDK (v sadě Visual Studio pro Mac, klikněte na tlačítko **nástroje > SDK Manager**).

2. Nainstalujte **Android 8.0 - Oreo** SDK balíčky. Pokud používáte sady SDK pro Android emulátoru, nezapomeňte zahrnout **x86** bitové kopie systému, které budete potřebovat:

    [![Výběr balíčků Android 8.0 ve Správci SDK](oreo-images/mac/01-android-o-packages.png)](oreo-images/mac/01-android-o-packages.png#lightbox)

3. Nainstalujte **Android SDK Tools 26.0.2** nebo novější, **Android SDK nástrojů platformy 26.0.0** nebo novější, a **Android SDK – nástroje sestavení 26.0.0** (nebo novější):

    [![Výběr nástrojů sady SDK pro Android 26 ve Správci SDK](oreo-images/mac/02-sdk-tools.png)](oreo-images/mac/02-sdk-tools.png#lightbox)

-----



### <a name="start-a-xamarinandroid-project"></a>Zahájení projektu Xamarin.Android

Vytvoření nového projektu Xamarin.Android. Pokud jste pro vývoj pro Android pomocí Xamarinu nové, přečtěte si téma [Hello, Android](~/android/get-started/hello-android/index.md) Další informace o vytváření projektů Xamarin.Android.

Když vytvoříte projekt pro Android, musíte nakonfigurovat nastavení verze cíl Android 8.0 nebo novější. Například pokud chcete zacílit na projekt pro Android 8.0, musíte nakonfigurovat úroveň cílové rozhraní API systému Android projektu pro **Android 8.0 (rozhraní API 26)**. Doporučujeme také nastavit cílové úrovni framework na rozhraní API 26 nebo novější. Další informace o konfiguraci úrovní úrovně rozhraní API systému Android, najdete v části [Principy Android API úrovně](~/android/app-fundamentals/android-api-levels.md).


### <a name="configure-an-emulator-or-device"></a>Konfigurace zařízení nebo emulátoru

Pokud se pokusíte spustit výchozí Google grafického uživatelského rozhraní správce AVD po instalaci Android SDK nástroje 26.0 nebo novější, se můžou zobrazit následující chyba dialog, který dá pokyn k použití nástroje příkazového řádku AVD manager **avdmanager** místo :

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

![Dialogové okno s upozorněním správce emulátoru Android](oreo-images/win/03-avd-warning.png)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

![Dialogové okno s upozorněním správce emulátoru Android](oreo-images/mac/03-avd-warning.png)

-----

Tato zpráva se zobrazí, protože Google již poskytuje samostatnou správce AVD grafického uživatelského rozhraní, která podporuje rozhraní API 26.0 a novější. Pro Android 8.0 Oreo, je nutné použít Správce emulátoru Android Xamarin nebo příkazového řádku `avdmanager` nástroj pro Android Oreo vytvořit virtuální zařízení.

Chcete-li vytvořit a spravovat virtuální zařízení pomocí Správce zařízení Android, najdete v části [Správa virtuálního zařízení pomocí Správce zařízení Android](~/android/get-started/installation/android-emulator/device-manager.md).
Pokud chcete vytvořit virtuální zařízení bez Správce zařízení Android, postupujte podle kroků v další části.


#### <a name="creating-virtual-devices-using-avdmanager"></a>Vytvoření virtuálního zařízení používat avdmanager

Chcete-li použít **avdmanager** při vytváření nové virtuální zařízení, postupujte podle těchto kroků:

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

1.  Otevřete okno příkazového řádku a nastavte `JAVA_HOME` umístění sady Java SDK ve vašem počítači. Pro typické instalace Xamarin můžete použít následující příkaz:

    ```cmd
    setx JAVA_HOME "C:\Program Files\Java\jdk1.8.0_131"
    ```

2.  Přidat umístění sady SDK pro Android `bin` složku pro vaše `PATH`.
    Pro typické instalace Xamarin můžete použít následující příkaz:

    ```cmd
    setx PATH "%PATH%;C:\Program Files (x86)\Android\android-sdk\tools\bin"
    ```

3.  Zavřete okno příkazového řádku a otevřete nové okno příkazového řádku. Vytvoření nového virtuálního zařízení pomocí [avdmanager](https://developer.android.com/studio/command-line/avdmanager.html) příkaz. Například k vytvoření AVD s názvem **AVD. Oreo 8.0** pomocí x86 bitovou kopii systému pro úroveň rozhraní API 26, použijte následující příkaz:

    ```cmd
    avdmanager create avd -n AVD-Oreo-8.0 -k "system-images;android-26;google_apis;x86"
    ```

4.  Po zobrazení výzvy s **chcete vytvořit vlastní hardwarového profilu [ne]** můžete zadat **žádné** a přijměte výchozí profil hardwaru. Pokud pro sdělení **Ano**, **avdmanager** vás vyzve seznam dotazy pro přizpůsobení hardwarového profilu.

Po jste **avdmanager** Pokud chcete vytvořit virtuální zařízení, budou zahrnuty v rozevírací nabídce zařízení:

[![Nové AVD přidány do zařízení rozevírací nabídky](oreo-images/win/04-android-o-avd-sml.png)](oreo-images/win/04-android-o-avd.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

1.  Otevřete **Terminálové** okno a změňte umístění adresáře nástrojů sady SDK pro Android na vaše Mac. Pro typické instalace Xamarin můžete použít následující příkaz:

    ```bash
    cd ~/Library/Developer/Xamarin/android-sdk-macosx/tools/bin
    ```

2.  Vytvoření nového virtuálního zařízení pomocí [avdmanager](https://developer.android.com/studio/command-line/avdmanager.html) příkaz. Například k vytvoření AVD s názvem **AVD. Oreo 8.0** pomocí x86 bitovou kopii systému pro úroveň rozhraní API 26, použijte následující příkaz:

    ```bash
    avdmanager create avd -n AVD-Oreo-8.0 -k "system-images;android-26;google_apis;x86"
    ```

3.  Po zobrazení výzvy s **chcete vytvořit vlastní hardwarového profilu [ne]** můžete zadat **žádné** a přijměte výchozí profil hardwaru. Pokud pro sdělení **Ano**, **avdmanager** vás vyzve seznam dotazy pro přizpůsobení profilu hardwaru.

Po použití **avdmanager** Pokud chcete vytvořit virtuální zařízení, budou zahrnuty v rozevírací nabídce zařízení:

[![Nové AVD přidány do zařízení rozevírací nabídky](oreo-images/mac/04-android-o-avd-sml.png)](oreo-images/mac/04-android-o-avd.png#lightbox)

-----

Další informace o konfiguraci pro testování a ladění emulátoru Androidu najdete v tématu [ladění pomocí emulátor Google Android](~/android/deploy-test/debugging/android-sdk-emulator/index.md).

Pokud používáte fyzické zařízení, například Nexus nebo jeden bod, můžete buď zařízení prostřednictvím automaticky aktualizovat prostřednictvím aktualizace letecké (OTA) nebo stáhnout bitovou kopii systému a flash zařízení přímo. Další informace o ruční aktualizaci zařízení pro Android Oreo najdete v tématu [objekt pro vytváření bitových kopií pro Nexus a pixelů zařízení](https://developers.google.com/android/images).



## <a name="new-features"></a>Nové funkce

Android Oreo zavádí řadu nových funkcí a funkce, jako třeba kanály oznámení, oznámení odznaky, vlastní písem v XML, ke stažení písma, automatické vyplňování a obrázek v obraze. V následujících částech zvýrazněte tyto funkce a poskytuje odkazy na vám pomůže začít používat je ve vaší aplikaci.



### <a name="notification-channels"></a>Kanály oznámení

*Kanály oznámení* jsou aplikace definované kategorie pro oznámení.
Můžete vytvořit kanál oznámení pro každý typ oznámení, že budete muset odeslat, a můžete vytvořit kanály oznámení tak, aby odrážela volby provedené uživatele vaší aplikace. Nová funkce kanály oznámení umožňuje poskytnout uživatelům jemně odstupňovanou kontrolu nad různé druhy oznámení. Například Pokud implementujete zasílání zpráv aplikace, můžete vytvořit kanály samostatné oznámení pro každou skupinu konverzace, který je vytvořen uživatelem.

[Kanály oznámení](~/android/app-fundamentals/notifications/local-notifications.md#notif-chan) vysvětluje, jak vytvořit kanál oznámení a použít jej pro publikování místního oznámení. Příklad kódu reálného, najdete v článku [NotificationChannels](https://developer.xamarin.com/samples/monodroid/android-o/NotificationChannels) ukázku; této ukázkové aplikace spravuje dva kanály a nastaví možnosti Další oznámení.



### <a name="notification-badges"></a>Odznaky oznámení

Odznaky oznámení jsou malých bodů, které se zobrazují nad ikony aplikace, jak je vidět na tomto snímku obrazovky:

[![Příklad odznaky oznámení na ikony aplikace](oreo-images/02-badges-sml.png)](oreo-images/02-badges.png#lightbox)

Tyto tečky označit, že nová oznámení pro jeden nebo více kanálů oznámení v aplikaci přidružené k této aplikace ikonu &ndash; Toto jsou oznámení, která ještě nebyla uživatel zavře nebo reagovali na ni. Uživatelé mohou dlouho-stisknutím klávesy na ikonu přehled upozornění související s oznámení oznámení v zavření nebo funguje na oznámení z nabídky stiskněte dlouho této appeaars.

Další informace o odznaky oznámení najdete v tématu Android Developer [oznámení odznaky](https://developer.android.com/guide/topics/ui/notifiers/notifications.html#Badges) tématu.



### <a name="custom-fonts-in-xml"></a>Vlastní písem v kódu XML

Zavádí Android Oreo *písem v kódu XML*, které umožňuje umožňuje začlenit vlastní písma jako prostředky. OpenType (**.otf**) a TrueType (**ttf**) podporované jsou formáty písma. Chcete-li přidat písma jako prostředky, postupujte takto:

1. Vytvoření **prostředky nebo písma** složky.

2. Zkopírujte soubory písma (například **ttf** a **.otf** soubory) k **prostředky nebo písma**. 

3. V případě potřeby přejmenujte každý soubor písma, aby se dodržuje konvence pojmenování souborů Android (tj, použijte pouze malá *a – z*, *0-9*a podtržítka v názvech souborů). Například soubor písma `Pacifico-Regular.ttf` může být přejmenován na něco podobného jako `pacifico.ttf`.

4. Použít vlastní písma s použitím nové `android:fontFamily` atribut v rozvržení XML. Například následující `TextView` deklarace používá přidaném **pacifico.ttf** písma prostředků:

   ```xml
   <TextView
     android:text="Example Text in Pacifico Regular"
     android:layout_width="wrap_content"
     android:layout_height="wrap_content"
     android:fontFamily="@font/pacifico" />
   ```

Můžete také vytvořit písma rodiny XML soubor, který popisuje víc písem a také stylu a tloušťky podrobnosti. Další informace najdete v tématu Android Developer [písem v kódu XML](https://developer.android.com/guide/topics/ui/look-and-feel/fonts-in-xml.html) tématu.


### <a name="downloadable-fonts"></a>Zaváděná písma

Od verze Android Oreo, aplikace může požádat o písma z poskytovatele namísto sdružování je do APK. Písma se stáhnou ze sítě, podle potřeby. Tato funkce snižuje velikost APK uspořit dat využití paměti a mobilní telefon. Také můžete tuto funkci verze Android API 14 a vyšší nainstalováním balíček Android knihovně 26 podpory.

Když aplikace potřebuje písmo, můžete vytvořit `FontsRequest` objektu (Výběr písma ke stažení) a pak předejte jej `FontsContract` metoda chcete stáhnout. Následující kroky popisují proces stahování písma podrobněji:

1.  Vytváření instancí [FontRequest](https://developer.android.com/reference/android/provider/FontRequest.html) objektu. 

2.  Podtřída a vytvoření instancí [FontsContract.FontRequestCallback](https://developer.android.com/reference/android/provider/FontsContract.FontRequestCallback.html).

3.  Implementace [FontRequestCallback.OnTypeFaceRetrieved](https://developer.android.com/reference/android/provider/FontsContract.FontRequestCallback.html#onTypefaceRetrieved%28android.graphics.Typeface%29) metodu, která slouží ke zpracování dokončení žádosti písma.

4.  Implementace [FontRequestCallback.OnTypeFaceRequestFailed](https://developer.android.com/reference/android/provider/FontsContract.FontRequestCallback.html#onTypefaceRequestFailed%28int%29) metoda, která slouží k informování aplikace všechny chyby, které se uskuteční během zpracování žádosti o písma.

5.  Volání [FontsContract.RequestFonts](https://developer.android.com/reference/android/provider/FontsContract.html#requestFonts(android.content.Context,%20android.provider.FontRequest,%20android.os.Handler,%20android.os.CancellationSignal,%20android.provider.FontsContract.FontRequestCallback)) metoda pro načtení písmo od zprostředkovatele písma. 

Při volání `RequestFonts` metoda, nejdřív zkontroluje, pokud písmo se místně uloží do mezipaměti (z předchozího volání `RequestFont`). Pokud není v mezipaměti, volá zprostředkovatele písma, asynchronně načte písmo a pak předá výsledky zpět do aplikace vyvoláním vaší `OnTypeFaceRetrieved` metoda.

[Ke stažení písem](https://developer.xamarin.com/samples/monodroid/android-o/DownloadableFonts) příklad ukazuje, jak používat funkci ke stažení písem, která byla zavedená v systému Android Oreo. 

Další informace o stahování písem najdete v tématu Android Developer [ke stažení písem](https://developer.android.com/guide/topics/ui/look-and-feel/downloadable-fonts.html) tématu.



### <a name="autofill"></a>Automatické vyplňování

Nové _automatické vyplňování_ framework v Android Oreo usnadňuje uživatelům zpracování opakované úlohy, jako je například přihlašovací údaje, vytvoření účtu služby a transakce s kreditními kartami. Uživatelé stráví méně času znovu zadáním informace (což může vést k vstupnímu chyby). Než vaše aplikace může pracovat s rozhraní automatické vyplňování, musí být povolena služby Automatické vyplňování v nastavení systému (uživatelům můžete povolit nebo zakázat automatické vyplňování).

[AutofillFramework](https://developer.xamarin.com/samples/monodroid/android-o/AutoFillFramework/) příklad znázorňuje použití automatické vyplňování Framework. Zahrnuje implementace klienta aktivity se zobrazeními, které by měly být autofilled a služba, která může poskytnout automatické vyplňování dat klientovi aktivity.

Další informace o nové funkce automatického vyplňování a optimalizaci aplikace pro automatické vyplňování najdete v tématu Android Developer [automatické vyplňování Framework](https://developer.android.com/guide/topics/text/autofill.html) tématu.



### <a name="picture-in-picture-pip"></a>Obrázek v obraze (PIP)

Android Oreo umožňuje pro aktivitu spustit v režimu obraz v obraze (PIP) na obrazovce jinou aktivitou překrytí. Tato funkce je určená pro přehrávání videa.

Chcete-li určit, že aktivita vaší aplikace můžete použít režim PIP, nastavte na hodnotu true v manifestu systému Android příznak následující:

```xml
android:supportsPictureInPicture
```

K určení chování vaši aktivitu, pokud je v režimu PIP, můžete použít novou [PictureInPictureParams](https://developer.android.com/reference/android/app/PictureInPictureParams.html) objektu. `PictureInPictureParams` představuje sadu parametrů, které používáte k inicializaci a aktualizovat aktivitu v režimu PIP (například aktivity upřednostňované poměr stran). Byly přidány následující nové metody PIP do `Activity` v Android Oreo:

-   [EnterPictureInPictureMode](https://developer.android.com/reference/android/app/Activity.html#enterPictureInPictureMode%28android.app.PictureInPictureParams%29) &ndash; vloží aktivity v režimu PIP. Aktivita je umístěn v horním rohu obrazovky a zbytek na obrazovce, naplní se předchozí aktivity, který byl na obrazovce.

-   [SetPictureInPictureParams](https://developer.android.com/reference/android/app/Activity.html#setPictureInPictureParams%28android.app.PictureInPictureParams%29) &ndash; aktualizace nastavení konfigurace PIP aktivity (například změnit v poměru stran).

[PictureInPicture](https://developer.xamarin.com/samples/monodroid/android-o/PictureInPicture) příklad ukazuje základní informace o využití pro kapesní zařízení byla zavedená v Oreo režimu obraz v obraze (PiP). Ukázka hraje video, které bude pokračovat bez přerušení při a zpět přepínání mezi režimy zobrazení nebo jiných aktivit.



### <a name="other-features"></a>Další funkce

Android Oreo obsahuje mnoho nových funkcí, například Emoji podpory knihovny API umístění pozadí omezení, barvu barevného celou rozsahu pro aplikace, nové zvukových kodeků, webového zobrazení vylepšení, podpora navigace Vylepšené klávesnice a nové rozhraní API AAudio (pro zvuk) pro zvuk s nízkou latencí vysoce výkonné, další informace o těchto funkcích najdete v části Android Developer [Android Oreo funkce a rozhraní API](https://developer.android.com/about/versions/oreo/android-8.0.html) tématu.



## <a name="behavior-changes"></a>Změny chování

Android Oreo zahrnuje celou řadu systému a změny chování rozhraní API, které mohou mít vliv na funkci existující aplikace. Tyto změny jsou popsány následujícím způsobem.


### <a name="background-execution-limits"></a>Omezení provádění pozadí

K vylepšení zkušeností uživatele, Android Oreo s sebou omezení na co můžete dělat aplikace při spuštění na pozadí. Například pokud uživatel je sledování videa nebo hraní her, spuštěná na pozadí aplikace může narušovat výkon aplikace náročné na video spuštěná v popředí. V důsledku toho Android Oreo umístí následující omezení na aplikace, které nejsou přímo interakci s uživatelem:

1.  **Omezení služby na pozadí** &ndash; když aplikace běží na pozadí, má okno několik minut, ve kterém je stále povoleno vytváření a používání služby. Na konci tohoto okna Android zastaví služba na pozadí aplikace a zpracovává jako _nečinnosti_.

2.  **Vysílání omezení** &ndash; Android 7.0 (25 rozhraní API) umístit do všesměrové vysílání, které aplikace se zaregistruje pro příjem omezení. Android Oreo díky přísnější tato omezení. Například aplikace Android Oreo už registrovat pro implicitní všesměrové vysílání příjemci v jejich manifesty.

Další informace o nové spuštění omezení pozadí najdete v tématu Android Developer [omezení provádění pozadí](https://developer.android.com/about/versions/oreo/background.html) tématu.


### <a name="breaking-changes"></a>Nejnovější změny

Aplikace, které cílí na Android Oreo nebo vyšší, musíte změnit své aplikace pro podporu k následujícím změnám, kde je to možné:

- Android Oreo ukončuje používání možnost nastavit prioritu jednotlivých oznámení. Místo toho nastavit úroveň doporučené důležitosti při vytváření kanál oznámení. Úroveň důležitosti, který přiřadíte kanál oznámení se vztahuje na všechny zprávy oznámení, které jsou účtovány na ni.

- Pro aplikace cílený na Android Oreo `PendingIntent.GetService()` nefunguje z důvodu nového omezení vztahujících se na služby spuštěny na pozadí. Pokud cílíte na Android Oreo, měli byste použít [PendingIntent.GetBroadcast](https://developer.xamarin.com/api/member/Android.App.PendingIntent.GetBroadcast/p/Android.Content.Context/System.Int32/Android.Content.Intent/Android.App.PendingIntentFlags/) místo.  


## <a name="sample-code"></a>Ukázkový kód

Několik ukázky Xamarin.Android jsou k dispozici pro ukazují, jak můžete využít výhod funkcí Android Oreo:

-   [NotificationsChannels](https://developer.xamarin.com/samples/monodroid/android-o/NotificationChannels) ukazuje, jak používat nového systému kanály oznámení byla zavedená v systému Android Oreo. Tato ukázka spravuje dva kanály oznámení: jednu s výchozí důležitosti a dalších s vysokou důležitostí.

-   [PictureInPicture](https://developer.xamarin.com/samples/monodroid/android-o/PictureInPicture) ukazuje základní informace o využití pro kapesní zařízení byla zavedená v Oreo režimu obraz v obraze (PiP). Ukázka hraje video, které bude pokračovat bez přerušení při a zpět přepínání mezi režimy zobrazení nebo jiných aktivit.

-   [AutofillFramework](https://developer.xamarin.com/samples/monodroid/android-o/AutoFillFramework) demonstruje použití automatické vyplňování Framework. Zahrnuje implementace klienta aktivity se zobrazeními, které by měly být autofilled a služba, která může poskytnout automatické vyplňování dat klientovi aktivity.

-   [Zaváděná písma](https://developer.xamarin.com/samples/monodroid/android-o/DownloadableFonts) představuje příklad, jak používat funkci ke stažení písem popsané výše.

-   [EmojiCompat](https://developer.xamarin.com/samples/monodroid/android-o/EmojiCompat) ukazuje použití EmojiCompat podpory knihovny. V této knihovně můžete zabránit aplikaci z zobrazující chybějící emoji znaků jako "tofu" znaků.

-   [Umístění aktualizace čekající na vyřízení záměr](https://developer.xamarin.com/samples/monodroid/android-o/AndroidPlayLocation/LocUpdPendIntent) ukazuje využití rozhraní API umístění chcete získat aktualizace o umístění zařízení pomocí `PendingIntent`.

-   [Služba popředí aktualizace umístění](https://developer.xamarin.com/samples/monodroid/android-o/AndroidPlayLocation/LocUpdFgService) ukazuje, jak používat rozhraní API umístění k získání aktualizací o umístění zařízení pomocí služby popředí vázané a spuštěna.


## <a name="video"></a>Video

> [!VIDEO https://youtube.com/embed/OuvEcaMO-Ho]

**8.0 Oreo vývoj pro Android pomocí jazyka C#**


## <a name="summary"></a>Souhrn

Tento článek zavedená Android Oreo a vysvětlení, jak nainstalovat a nakonfigurovat na Android Oreo nejnovější nástroje a balíčky pro vývoj pro Xamarin.Android. Poskytl přehled klíčových funkcí, které jsou k dispozici v systému Android Oreo odkazy na příkladu zdrojového kódu pro několik nových funkcí. Zahrnutý odkazy na dokumentaci k rozhraní API a Android Developer témata vám pomohou začít vytváření aplikací pro Android Oreo. Zvýrazněná také nejdůležitější změny Android Oreo chování, které by mohlo mít vliv na stávající aplikace.


## <a name="related-links"></a>Související odkazy

- [Android 8.0 Oreo](https://developer.android.com/index.html)
