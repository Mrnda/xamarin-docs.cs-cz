---
title: Android P ve verzi Preview
description: Jak začít používat Xamarin.Android pro vývoj aplikací pro nejnovější verzi Androidu.
ms.prod: xamarin
ms.assetid: 6575DD32-9DC8-44E6-85EF-1F8BD07D3780
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 07/27/2018
ms.openlocfilehash: a3eef6f2537a4b09f603787d7cdbf70a173fca80
ms.sourcegitcommit: aa9b9b203ab4cd6a6b4fd51e27d865e2abf582c1
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/30/2018
ms.locfileid: "39351859"
---
# <a name="android-p-preview"></a>Android P ve verzi Preview

_Jak začít používat Xamarin.Android pro vývoj aplikací pro nejnovější verzi Androidu._

![](~/media/shared/preview.png)

[Android Developer P Preview](https://developer.android.com/preview/) je nyní k dispozici z Googlu. Počet nových funkcí a rozhraní API budou k dispozici v této verzi a většina z nich je nutné využít nové funkce hardwaru nejnovější zařízení s Androidem.

![Android P ústřední obrázek](android-p-images/01-android-p-logo.png)

Tento článek je strukturována tak, aby vám pomohli při vývoji aplikace Xamarin.Android pro Android ve verzi Preview P. Vysvětluje, jak nainstalovat potřebné aktualizace, nakonfigurujte sadu SDK a přípravu emulátoru nebo zařízení pro účely testování. Také nabízí přehled nových funkcí v Androidu P a poskytuje ukázkovém zdrojovém kódu, který ukazuje, jak používat některé z klíčových funkcí Android P.


## <a name="requirements"></a>Požadavky

Pokud chcete používat Android P funkce v aplikacích založených na Xamarinu jsou vyžadovány následující položky:

-   **Visual Studio** &ndash; Pokud používáte Windows, 15,8 ve verzi Preview 5 nebo novější sady Visual Studio je vyžadována verze.  Pokud používáte počítač Mac, je vyžadována aktuální Beta verze sady Visual Studio pro Mac nebo novější.

-   **Xamarin.Android** &ndash; Xamarin.Android 9.0.0.17 nebo novější je nutné nainstalovat Visual Studio.

-   **Java Developer Kit** &ndash; Xamarin Android 9.0 vývoj vyžaduje [JDK 8](http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html) (nebo si můžete vyzkoušet preview od Microsoftu distribuci [OpenJDK](~/android/get-started/installation/openjdk.md)).

-   **Sada SDK pro Android** &ndash; Android SDK API 28 nebo novější musí být nainstalován prostřednictvím správce sady Android SDK.

## <a name="getting-started"></a>Začínáme

Abyste mohli začít používat Android P s Xamarin.Android, musíte stáhnout a nainstalovat nejnovější nástroje a sady SDK balíčky, než budete moct vytvořit svůj první projekt Android P:

1. Aktualizace na Visual Studio 15.8 ve verzi Preview 5 nebo novější. Pokud používáte Visual Studio pro Mac, přepněte do sady Visual Studio pro Mac [Beta](https://docs.microsoft.com/visualstudio/mac/update) kanálu.

2. Nainstalujte **P Android (rozhraní API 28)** nebo novější balíčky a nástrojů přes správce sady SDK.

3. Vytvoření nového projektu Xamarin.Android, který cílí na Android P (28 rozhraní API).

4. Nakonfigurujte emulátoru nebo zařízení pro testování aplikací pro Android P.

Každý z těchto kroků je vysvětlené v následujících částech:


### <a name="update-visual-studio"></a>Aktualizace sady Visual Studio

Chcete-li přidat podporu Android P se sadou Visual Studio, aktualizujte na Visual Studio 2017 verze 15,8 ve verzi Preview 5 nebo novější jak je uvedeno v [aktualizace sady Visual Studio 2017 na nejnovější verzi](https://docs.microsoft.com/visualstudio/install/update-visual-studio).
Chcete-li přidat podporu Android P se sadou Visual Studio pro Mac, aktualizovat na nejnovější verzi beta verze sady Visual Studio 2017 for Mac podle pokynů v [aktualizace sady Visual Studio pro Mac](https://docs.microsoft.com/visualstudio/mac/update).


### <a name="install-the-android-sdk"></a>Instalace sady Android SDK

Vytvoření projektu s Xamarin.Android 9.0, musíte nejprve použít Android SDK Manager k instalaci sady SDK platformy pro **P Android (úroveň rozhraní API 28)** nebo novější.

1. Spusťte správce sady SDK. V sadě Visual Studio, klikněte na tlačítko **nástroje > Android > správce sady Android SDK**. V sadě Visual Studio pro Mac, klikněte na tlačítko **nástroje > správce sady SDK**.

2. V pravém dolním rohu klikněte na ikonu ozubeného kola a vyberte **úložiště > Google (nepodporované)**:

    [![Nastavení úložiště Google](android-p-images/vs/set-repo-sml.png)](android-p-images/vs/set-repo.png#lightbox)

3. Nainstalujte **Android P** balíčky, které se zobrazují jako **28 platformy Android SDK** v **platformy** kartu (Další informace o používání správce sady SDK najdete v tématu (Android SDK Setup)[~/android/get-started/installation/android-sdk.md]): 

    [![Instalace balíčků Android P](android-p-images/vs/sdk-manager-sml.png)](android-p-images/vs/sdk-manager.png#lightbox)

4. Pokud používáte emulátor, vytvoření virtuálního zařízení, která podporuje **28 úroveň rozhraní API**. Další informace o vytváření virtuálního zařízení, najdete v části [spravovat virtuální zařízení pomocí Správce zařízení s Androidem](~/android/get-started/installation/android-emulator/device-manager.md).



### <a name="start-a-xamarinandroid-project"></a>Spuštění projektu Xamarin.Android

Vytvoření nového projektu Xamarin.Android. Pokud jste ještě na vývoj pro Android s využitím kódu Xamarin, najdete v článku [Hello, Android](~/android/get-started/hello-android/index.md) Další informace o vytváření projekty Xamarin.Android.

Když vytvoříte projekt pro Android, musíte nakonfigurovat nastavení verzí do cíle s Androidem P nebo novější. Například pokud chcete zaměřit svůj projekt pro Android P, musíte nakonfigurovat úroveň rozhraní Android API cílový projekt tak, aby **P Android (rozhraní API 28)**. Doporučuje se nastavit také cílovou úrovní rozhraní API 28 nebo novější. Další informace o konfiguraci úrovně rozhraní Android API úrovně, naleznete v tématu [Principy Android API úrovně](~/android/app-fundamentals/android-api-levels.md).


### <a name="configure-a-device-or-emulator"></a>Konfigurace zařízení nebo emulátoru

Pokud používáte fyzické zařízení, jako je například Pixel, můžete aktualizovat zařízení s Androidem ve verzi Preview P podle pokynů v [zařízení s Androidem Beta P](https://developer.android.com/preview/devices/).

Pokud používáte emulátor, vytvoření virtuálního zařízení pro úroveň rozhraní API pomocí image na základě x86 28. Informace o používání Správce zařízení s Androidem k vytvoření a Správa virtuálního zařízení, najdete v tématu [spravovat virtuální zařízení pomocí Správce zařízení s Androidem](~/android/get-started/installation/android-emulator/device-manager.md).
Informace o použití emulátoru Androidu pro testování a ladění, naleznete v tématu [ladění pomocí emulátoru Google Android](~/android/deploy-test/debugging/android-sdk-emulator/index.md).



## <a name="new-features"></a>Nové funkce

Android P zavádí řadu nových funkcí. Některé z těchto nových funkcích jsou určené pro využití nové funkce hardwaru, které nabízí nejnovější zařízení s Androidem, zatímco jiné jsou navržené k dalšímu vylepšení prostředí pro uživatele androidu s:

-   **Zobrazit vystřižení podporu** &ndash; poskytuje rozhraní API pro vyhledání umístění a tvar _vystřižení_ v horní části obrazovky na novějších zařízeních s Androidem.

-   **Vylepšení oznámení** &ndash; zpráv s oznámením nyní můžete zobrazit obrázky a nový `Person` třída se používá k zjednodušení konverzace účastníky.

-   **Vnitřní umístění** &ndash; podpoře platforem pro protokol Round round-trip čas Wi-Fi, který umožňuje pro aplikace, které používají pro navigaci v vnitřních nastavení Wi-Fi zařízení.

-   **Podpora více fotoaparát** &ndash; nabízí možnost do datových proudů přístup současně z více fyzických kamery (jako je například duální – přední a zadní duální kamery).


Následující části zvýrazněte tyto funkce a poskytnout příklady stručný kódu vám pomůže je začít používat ve vaší aplikaci.

### <a name="display-cutout-support"></a>Podpora vystřižení zobrazení

Mají mnoho novější zařízení s Androidem edge okraje obrazovky *zobrazit vystřižení* (nebo "stupeň") v horní části obrazovky pro kamery a reproduktorů.
Na následujícím snímku obrazovky obsahuje příklad emulátor vystřižení:

[![Simulace vystřižení emulátoru androidu](android-p-images/02-example-cutout-sml.png)](android-p-images/02-example-cutout.png#lightbox)

Pokud chcete spravovat, jak vaše aplikace okně zobrazí jeho obsah na zařízeních s vystřižení zobrazení, byl přidán nový Android P [LayoutInDisplayCutoutMode](https://developer.android.com/reference/android/view/WindowManager.LayoutParams.html#layoutInDisplayCutoutMode) atribut rozložení okna. Tento atribut je možné nastavit na jednu z následujících hodnot:

-   [LayoutInDisplayCutoutModeNever](https://developer.android.com/reference/android/view/WindowManager.LayoutParams.html#LAYOUT_IN_DISPLAY_CUTOUT_MODE_NEVER) &ndash; okna nikdy může překrývat s vystřižení oblasti.

-   [LayoutInDisplayCutoutModeShortEdges](https://developer.android.com/reference/android/view/WindowManager.LayoutParams.html#LAYOUT_IN_DISPLAY_CUTOUT_MODE_SHORT_EDGES) &ndash; okno může rozšířit do oblasti Vystřižení, ale jenom na krátkou okraji obrazovky. 

-   [LayoutInDisplayCutoutModeDefault](https://developer.android.com/reference/android/view/WindowManager.LayoutParams.html#LAYOUT_IN_DISPLAY_CUTOUT_MODE_DEFAULT) &ndash; okno může rozšířit do oblasti vystřižení Pokud výřez je obsažena v panelu systému.

Například, abyste zabránili překrývající se oblasti vystřižení okna aplikace, nastavte režim vystřižení rozložení na *nikdy*: 

```csharp
Window.Attributes.LayoutInDisplayCutoutMode =
    Android.Views.LayoutInDisplayCutoutMode.Never;
```

Následující příklady popisují příklady těchto režimech vystřižení. Na levé straně na první snímku obrazovky je aplikace v jiných celoobrazovkového režimu. Na snímku obrazovky centra aplikace přejde celou obrazovku s `LayoutInDisplayCutoutMode` nastavena na `LayoutInDisplayCutoutModeShortEdges`. Všimněte si, že aplikace bílé pozadí zasahuje do oblasti vystřižení zobrazení:

[![Příklad vystřižení režimy zobrazení v emulátoru](android-p-images/03-cutout-modes-sml.png)](android-p-images/03-cutout-modes.png#lightbox)

Na posledním snímku obrazovky (výše na pravé straně), `LayoutInDisplayCutoutMode` je nastavena na `LayoutInDisplayCutoutModeShortNever` předtím, než se přejde na celou obrazovku.
Všimněte si, že aplikace bílé pozadí nejsou povolené pro rozšíření do oblasti vystřižení zobrazení.

Pokud potřebujete podrobnější informace o oblasti vystřižení na zařízení, můžete použít novou [DisplayCutout](https://developer.android.com/reference/android/view/DisplayCutout.html) třídy. `DisplayCutout` představuje oblast zobrazení, které nelze použít k zobrazení obsahu. Tyto informace můžete použít k načtení umístění a tvar výřez tak, aby vaše aplikace nebude pokoušet o zobrazit obsah v této oblasti nejsou funkční.

Další informace o nových funkcích vystřižení v Androidu P, naleznete v tématu [zobrazit vystřižení podporu](https://developer.android.com/preview/features#cutout).



### <a name="notifications-enhancements"></a>Vylepšení oznámení

Android P přináší následující vylepšení na zlepšení uživatelského rozhraní pro zasílání zpráv:

-   Kanály oznámení (počínaje [Android Oreo](~/android/platform/oreo.md)) teď podporuje blokování kanál skupiny.

-   Systém oznámení má tři nové-Not-narušit kategorie (nastavení priority, alarmy, zvuky systému a zdroje média). Kromě toho jsou sedm nových-Not-narušit režimy, které slouží k potlačení visual přerušení (například oznámení, oznámení světla, vzhled panelu stavu a spouštění aktivit celé obrazovky).

-   Nový [osoba](https://developer.android.com/reference/android/app/Person.html) třída byla přidána k reprezentaci odesílatele zprávy. Použití této třídy pomáhá optimalizovat vykreslování jednotlivým oznámením, že určení lidé podílející konverzace (včetně jejich avatary a identifikátory URI).

-   Oznámení můžete nyní zobrazit obrázky. 

Následující příklad ukazuje, jak používat nová rozhraní API pro generování oznámení, který obsahuje bitovou kopii. Na následujících snímcích obrazovky oznámení text nový tweet a je následována oznámení s vložený obrázek. Při oznámení jsou rozbaleny (jak je vidět na pravé straně), je zobrazen text prvního oznámení a součástí image se zvětší druhé oznámení:

[![Příklad oznámení s obrázkem](android-p-images/04-example-notifications-sml.png)](android-p-images/04-example-notifications.png#lightbox)

Následující příklad ukazuje, jak pro zahrnutí obrázku v oznámení Androidu P a demonstruje využití nového `Person` třídy:

1. Vytvoření `Person` objekt, který reprezentuje odesílatele. Například součástí názvu a ikony odesílatele `fromPerson`:

    ```csharp
    Icon senderIcon = Icon.CreateWithResource(this, Resource.Drawable.sender_icon);
    Person fromPerson = new Person.Builder()
        .SetIcon(senderIcon)
        .SetName("Mark Sender")
        .Build();
    ```

2. Vytvoření `Notification.MessagingStyle.Message` , který obsahuje bitovou kopii k odeslání, předávání bitovou kopii k novému [Notification.MessagingStyle.Message.SetData](https://developer.android.com/reference/android/app/Notification.MessagingStyle.Message.html#setData%28java.lang.String,%20android.net.Uri) metody.
   Příklad:

    ```csharp
    Uri imageUri = Uri.Parse("android.resource://com.xamarin.pminidemo/drawable/example_image");
    Notification.MessagingStyle.Message message = new Notification.MessagingStyle
            .Message("Here's a picture of where I'm currently standing", 0, fromPerson)
            .SetData("image/", imageUri);
    ```

3. Zpráva, kterou chcete přidat `Notification.MessagingStyle` objektu. Příklad:

    ```csharp
    Notification.MessagingStyle style = new Notification.MessagingStyle(fromPerson)
            .AddMessage(message);
    ```

4. Připojte tento styl Tvůrce oznámení. Příklad:

    ```csharp
    builder = new Notification.Builder(this, MY_CHANNEL)
        .SetContentTitle("Tour of the Colosseum")
        .SetContentText("I'm standing right here!")
        .SetSmallIcon(Resource.Mipmap.ic_notification)
        .SetStyle(style)
        .SetChannelId(MY_CHANNEL);
    ```

5. Publikování oznámení. Příklad:

    ```csharp
    const int notificationId = 1000;
    notificationManager.Notify(notificationId, builder.Build());
    ```

Další informace o vytváření oznámení najdete v tématu [místní oznámení](~/android/app-fundamentals/notifications/local-notifications.md).


### <a name="indoor-positioning"></a>Vnitřní umístění

Poskytuje podporu pro IEEE 802.11mc Android P (označované také jako _Wi-Fi round-trip Round-Time_ nebo _Wi-Fi požadavku_), která umožňuje aplikacím zjistit vzdálenost k jednomu nebo více Wi-Fi přístupových bodů. Na základě těchto informací je možné pro vaši aplikaci, abyste mohli využívat *vnitřních umístění* s přesností na jeden až dva měřiče. Na zařízeních s Androidem, které poskytují hardwarovou podporu pro IEEE 801.11mc vaše aplikace můžou nabízet navigačním funkcím, jako je založená na poloze ovládací prvek inteligentního zařízení nebo zapnout vypnout pokyny prostřednictvím úložiště:

[![Příklad vnitřních navigace pomocí požadavku Wi-Fi](android-p-images/05-wifi-rtt-sml.png)](android-p-images/05-wifi-rtt.png#lightbox)

Nové [WifiRttManager](https://developer.android.com/reference/android/net/wifi/rtt/WifiRttManager) třídy a několik pomocných tříd poskytuje prostředky pro měření vzdálenosti k Wi-Fi zařízení. Další informace o vnitřních zavedené v Androidu P umístění rozhraní API najdete v tématu [Android.Net.Wifi.Rtt](https://developer.android.com/reference/android/net/wifi/rtt/package-summary).


### <a name="multi-camera-support"></a>Podpora více fotoaparátu

Mnoho novější zařízení s Androidem mají dual-front a/nebo duální back kamery, které jsou užitečné pro funkce, jako stereo zpracování obrazu, vylepšené vizuálních efektů a přiblížení vylepšené funkce. Android P zavádí nový [více fotoaparát](https://developer.android.com/preview/features#camera) rozhraní API, které umožňuje pro aplikace pro použití *logické fotoaparát* (nebo *logické více fotoaparát*), která je založená na dvou nebo více fyzické kamery.
Chcete-li zjistit, pokud je zařízení podporuje logické více fotoaparát, můžete si prohlédnout možnosti jednotlivých fotoaparát v zařízení a podívejte se, pokud podporuje [RequestAvailableCapabilitiesLogicalMultiCamera](https://developer.android.com/reference/android/hardware/camera2/CameraMetadata#REQUEST_AVAILABLE_CAPABILITIES_LOGICAL_MULTI_CAMERA).

Android P obsahuje také nové [SessionConfiguration](https://developer.android.com/reference/android/hardware/camera2/params/SessionConfiguration.html) třídu, která slouží ke zpožděním při prvním zachycení a eliminovat tak nutnost začít a spustit fotoaparát datového proudu.

Další informace o více fotoaparátu podpory v Androidu P naleznete v tématu [více fotoaparátu podpory a fotoaparát aktualizace](https://developer.android.com/preview/features#camera).


### <a name="other-features"></a>Další funkce

Kromě toho Android P podporuje několik nových funkcí:

-   Nové [AnimatedImageDrawable](https://developer.android.com/reference/android/graphics/drawable/AnimatedImageDrawable.html) třídu, která lze použít pro kreslení a zobrazení animovaný obrázků.

-   Nový [ImageDecoder](https://developer.android.com/reference/android/graphics/ImageDecoder.html) třídu, která nahrazuje `BitmapFactory`. `ImageDecoder` slouží k dekódování `AnimatedImageDrawable`.

-   Podpora pro video HDR (vysoký dynamický rozsah) a HEIF (vysokou efektivitu Image File Format) bitové kopie.

-   [JobScheduler](https://developer.android.com/reference/android/app/job/JobScheduler.html) byla vylepšena, aby více inteligentnímu zpracování úlohy související se sítí. Nové [GetNetwork](https://developer.android.com/reference/android/app/job/JobParameters#getNetwork%28%29) metodu [JobParameters](https://developer.android.com/reference/android/app/job/JobParameters) třídy vrátí nejlepší síťová pro provádění všechny síťové požadavky pro danou úlohu.

Další informace o nejnovějších funkcích Android P najdete v tématu [Android P funkce a rozhraní API](https://developer.android.com/preview/features).


## <a name="behavior-changes"></a>Změny chování

Pokud cílová verze Androidu je nastaven na úroveň rozhraní API 28, existuje několik změn platformy, které můžou ovlivnit chování vaší aplikace, i když nejsou implementaci nových funkcí popsaných výše. Následuje stručný popis těchto změn:

-  Aplikace musí teď můžou vyžádat oprávnění popředí před použitím služby na popředí.

-  Pokud vaše aplikace obsahuje více než jeden proces, nelze sdílet jeden [WebView](https://developer.xamarin.com/api/type/Android.Webkit.WebView/) adresář dat napříč procesy.

-  Přímý přístup k adresáři data vaší aplikace podle cesty již není povolena.

Další informace o změnách chování pro aplikace zaměřené na Androidu P, naleznete v tématu [změny chování](https://developer.android.com/preview/behavior-changes.html#p-apps).


## <a name="sample-code"></a>Ukázkový kód

[AndroidPMiniDemo](https://github.com/xamarin/monodroid-samples/tree/master/android-p/AndroidPMiniDemo) je ukázková aplikace Xamarin.Android pro Android P, které ukazuje, jak nastavit vystřižení režimy zobrazení, jak používat nové `Person` třídy a jak se pošle oznámení, který obsahuje bitovou kopii.


## <a name="summary"></a>Souhrn

Tento článek zavedené ve verzi Preview sady Android P a bylo vysvětleno, jak nainstalovat a nakonfigurovat nejnovější nástroje a balíčky pro vývoj pro Xamarin.Android Android P ve verzi Preview. To přehled klíčových funkcí, které jsou k dispozici v Androidu P, opatřeného ukázkovém zdrojovém kódu pro některé z těchto funkcí. Zahrnuté odkazy na dokumentaci k rozhraní API a Android Developer témat, která vám pomůže začít pracovat v vytváření aplikací pro Android P. Také zvýrazněna nejdůležitější změny chování Android P, které může mít vliv na stávající aplikace.


## <a name="related-links"></a>Související odkazy

- [Android P Developer Preview](https://developer.android.com/preview/)
