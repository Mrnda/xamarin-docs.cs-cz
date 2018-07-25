---
title: Funkce pro verzi nougat
description: Jak začít používat pro vývoj aplikací pro Android verzi Nougat Xamarin.Android.
ms.prod: xamarin
ms.assetid: 5C74ABE2-C862-4ED0-8EA5-C7FEE5251D4B
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 06/02/2018
ms.openlocfilehash: 2e15de944f05fede802dbf52987d80a46fb890ef
ms.sourcegitcommit: b56b3f906d2c05a3f1be219ef41be8b79e519b8e
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/25/2018
ms.locfileid: "39242150"
---
# <a name="nougat-features"></a>Funkce pro verzi nougat

_Jak začít používat pro vývoj aplikací pro Android verzi Nougat Xamarin.Android._

Tento článek obsahuje přehled funkce zavedené v Androidu verzi Nougat vysvětluje postup přípravy Xamarin.Android pro verzi Nougat Android development a obsahuje odkazy na ukázkové aplikace, které ukazují, jak používat funkce s Androidem verzi Nougat v Aplikace Xamarin.Android.


## <a name="overview"></a>Přehled

[Android verzi Nougat](https://developer.android.com/about/versions/nougat/android-7.0.html) je odezvě Googlu na Android 6.0 Marshmallow. Poskytuje podporu pro Xamarin.Android **Android 7.x vazby** v Xamarin Android 7.0 nebo novější. Android verzi Nougat přidá mnoho nových rozhraní API pro verzi Nougat funkce popsané níže; Tato rozhraní API jsou dostupná pro aplikace Xamarin.Android, při použití Xamarin.Android 7.0.

[![Tablety s Androidem a telefony s Androidem verzi Nougat Hero bitové kopie](nougat-images/android-n-hero-sml.png)](nougat-images/android-n-hero.png#lightbox)

Další informace o rozhraní API pro Android 7.x, naleznete v tématu [Android 7.1 pro vývojáře](http://developer.android.com/preview/api-overview.html).
Seznam známých problémů Xamarin.Android 7.0, najdete v tématu [poznámky k verzi](https://developer.xamarin.com/releases/android/xamarin.android_7/xamarin.android_7.0/).

Android verzi Nougat obsahuje mnoho nových funkcí, které vás zajímají pro vývojáře prostředí Xamarin.Android. Tyto funkce patří:

-   **Podpora více oken** &ndash; toto vylepšení umožňuje uživatelům najednou otevřít dvě aplikace na obrazovce.

-   **Vylepšení oznámení** &ndash; systém přepracovaný oznámení v Androidu verzi Nougat zahrnuje *přímé odpovědi* funkce, která umožňují rychle reagovat na text zprávy přímo z oznámení UŽIVATELSKÉ ROZHRANÍ. Navíc pokud vaše aplikace vytvoří upozornění pro přijaté zprávy, nové *dodávat oznámení* funkce můžete svázat oznámení společně jako jednu skupinu při doručení zprávy více než jeden.

-   **Data spořič** &ndash; tato funkce je nová služba systému, která pomáhá snižovat používání mobilních dat aplikace; poskytuje uživatelům řídit využití mobilních dat aplikace.

Kromě toho Android verzi Nougat přináší mnoho dalších vylepšení zajímavé pro vývojáře aplikací, jako jsou nové funkce Konfigurace zabezpečení sítě, Doze na cestách, klíče ověření identity, nové rychlé nastavení rozhraní API, podpora více národní prostředí, ICU4J rozhraní API, WebView vylepšení, přístup k funkcím pro jazyk Java 8, přístup k adresáři s vymezeným oborem, o vlastní ukazatele rozhraní API, podpora VR platforem, virtuální soubory a zpracování optimalizace na pozadí.

Tento článek vysvětluje, jak začít vytvářet aplikace s Androidem verzi Nougat vyzkoušet nové funkce a plánování migrace nebo funkci práce pro zaměření na novou platformu Android verzi Nougat.


## <a name="requirements"></a>Požadavky

Používat nové funkce Android verzi Nougat v aplikacích založených na Xamarinu jsou vyžadovány následující položky:

-   **Visual Studio nebo Visual Studio pro Mac** &ndash; Pokud používáte Visual Studio, verze 4.2.0.628 nebo novější sady Visual Studio Tools for Xamarin je povinný. Pokud používáte Visual Studio pro Mac verze 6.1.0 nebo vyšší sady Visual Studio pro Mac je povinný.

-   **Xamarin.Android** &ndash; Xamarin.Android 7.0 nebo novější musí být nainstalované a nakonfigurované pomocí sady Visual Studio nebo Visual Studio pro Mac.

-   **Sada SDK pro Android** – Android 7.0 sady SDK (API 24) nebo novější musí být nainstalován prostřednictvím správce sady Android SDK.

-   **Java Developer Kit** &ndash; Xamarin Android 7.0 vývoj vyžaduje [JDK 8](http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html) nebo novější, pokud vyvíjíte pro úroveň rozhraní API 24 nebo větší (JDK 8 také podporuje úrovně rozhraní API starších než 24). Vyžaduje se verze 64bitové sady JDK 8, pokud používáte vlastní ovládací prvky nebo prohlížeč formulářů.

> [!IMPORTANT]
> Xamarin.Android nepodporuje JDK 9.

Všimněte si, že aplikace musí znovu vytvořit s Xamarin C6SR4 nebo novější verzi Androidu Nougat pracovat spolehlivě. Protože Android verzi Nougat můžete propojit jenom s [poskytované NDK nativních knihoven](https://developer.android.com/about/versions/nougat/android-7.0-changes.html), stávající aplikace pomocí knihovny, například **Mono.Data.Sqlite.dll** dojít k chybě při spuštění na platformě Nougat s Androidem, pokud nejsou správně znovu sestavit.



## <a name="getting-started"></a>Začínáme

Abyste mohli začít používat verzi Nougat Android s Xamarin.Android, musíte stáhnout a nainstalovat nejnovější nástroje a sady SDK balíčky, než budete moct vytvořit projekt Android verzi Nougat:

1.  Nainstalujte nejnovější aktualizace Xamarin.Android z Xamarin.

2.  Nainstalujte **Android 7.0 (rozhraní API 24)** balíčky a nástroje nebo novější.

3.  Vytvoření nového projektu Xamarin.Android, který cílí na Android verzi Nougat.

4.  Konfigurace zařízení nebo emulátor pro Android verzi Nougat.

Každý z těchto kroků je vysvětlené v následujících částech:


### <a name="install-xamarin-updates"></a>Aktualizace Xamarinu

Chcete-li přidat podporu Xamarinu pro Android verzi Nougat, změňte kanálu aktualizace v sadě Visual Studio nebo Visual Studio pro Mac na kanál Stable a použijte nejnovější aktualizace. Pokud potřebujete funkce, které jsou aktuálně k dispozici pouze v kanálu alfa nebo Beta, můžete přepnout do kanálu alfa nebo Beta (kanály alfa a beta verze také poskytovat podporu pro Android 7.x). Informace o tom, jak změnit kanálů aktualizací (vydání) najdete v tématu [Změna kanálu aktualizace](https://github.com/xamarin/recipes/tree/master/Recipes/cross-platform/ide/change_updates_channel).



### <a name="install-the-android-sdk"></a>Instalace sady Android SDK

Vytvoření projektu se systémem Android 7.0 Xamarin, musíte nejprve použít Android SDK Manager k instalaci **sadu SDK platformy Android N (rozhraní API 24)** nebo novější. Je také nutné nainstalovat nejnovější **Android SDK Tools**:

1.  Spustit správce sady Android SDK (v sadě Visual Studio pro Mac, použijte **nástroje > Otevřít správce sady Android SDK&hellip;**; v sadě Visual Studio, použijte **nástroje > Android > správce sady Android SDK**).

2.  Nainstalujte **Android 7.0 (rozhraní API 24)** nebo novější:

    [![Výběr Android 7.0 balíčky Android SDK Manager](nougat-images/preview-packages.png)](nougat-images/preview-packages.png#lightbox)

3.  Nainstalujte nejnovější nástroje sady Android SDK:

    [![Vyberte nejnovější nástroje sady Android SDK v správce sady Android SDK](nougat-images/preview-tools.png)](nougat-images/preview-tools.png#lightbox)

    Musíte nainstalovat Android SDK Tools revize 25.2.2 nebo novější, Android SDK platformy nástroje 24.0.3 nebo novější a Android SDK sestavení 24.0.2 nebo novější.

4.  Ověřte, že **umístění sady Java Development Kit** je nakonfigurovaný pro sadu JDK 1.8:

    [![Konfigurace sady JDK 8 cesty v části Možnosti nástrojů](nougat-images/use-jdk-1.8.png)](nougat-images/use-jdk-1.8.png#lightbox)

    Toto nastavení můžete zobrazit v sadě Visual Studio, klikněte na tlačítko **nástroje > Možnosti > Xamarin > Nastavení Androidu**. V sadě Visual Studio pro Mac, klikněte na tlačítko **Předvolby > Projekty > umístění sad SDK > Android**.



### <a name="start-a-xamarinandroid-project"></a>Spuštění projektu Xamarin.Android

Vytvoření nového projektu Xamarin.Android. Pokud jste ještě na vývoj pro Android s využitím kódu Xamarin, najdete v článku [Hello, Android](~/android/get-started/hello-android/index.md) Další informace o vytváření projekty Xamarin.Android.

Když vytvoříte projekt pro Android, musíte nakonfigurovat nastavení verzí pro cílový Android 7.0 nebo novější. Například pokud chcete zaměřit svůj projekt pro Android 7.0, musíte nakonfigurovat úroveň rozhraní Android API cílový projekt tak, aby **Android 7.0 (rozhraní API 24 - verzi Nougat)**. Doporučuje se, že nastavíte cílovou úrovní rozhraní API 24 nebo novější. Další informace o konfiguraci úrovně rozhraní Android API úrovně, naleznete v tématu [Principy Android API úrovně](~/android/app-fundamentals/android-api-levels.md).


> [!NOTE]
> Momentálně je nutné nastavit **minimální verzi systému Android** k **Android 7.0 (rozhraní API 24 - verzi Nougat)** k nasazení aplikace do zařízení s Androidem verzi Nougat nebo emulátorů.



### <a name="configure-an-emulator-or-device"></a>Konfigurace zařízení nebo emulátoru

Pokud používáte emulátor, spusťte Správce AVD s Androidem a vytvoření nového zařízení pomocí následujících nastavení:

-   Zařízení: Nexus 5 X Nexus 6 Nexus 6P, Nexus Player Nexus 9 nebo Pixel C.
-   Cíl: Android 7.0 – úroveň rozhraní API 24
-   ABI: x86 nebo x86\_64

Tato virtuální zařízení je třeba nakonfigurovat pro emulaci Nexus 6:

[![Konfigurace AVD pomocí zařízení Nexus 6, cíl Android 7.0 a Intel Atom x86 CPU/ABI](nougat-images/android-n-avd.png)](nougat-images/android-n-avd.png#lightbox)

Pokud používáte fyzické zařízení, jako je například Nexus 5 X, 6 nebo 9, můžete buď aktualizujte svoje zařízení prostřednictvím automaticky přes aktualizace air (OTA) nebo stáhnout bitovou kopii systému a flash zařízení přímo. Další informace o ruční aktualizaci zařízení na Android verzi Nougat najdete v tématu [OTA bitových kopií pro na zařízeních Nexus](https://developers.google.com/android/nexus/ota).

Všimněte si, že zařízeních Nexus 5 nepodporuje verzi Nougat Android.



## <a name="new-features"></a>Nové funkce

Android verzi Nougat zavádí řadu nových funkcí a možností, jako je podpora více oken, vylepšení oznámení a úspora Data. V dalších částech zvýrazněte tyto funkce a poskytují odkazy vám pomůžou začít používat je ve vaší aplikaci.



### <a name="multi-window-mode"></a>Režim více okna

Režim více okna umožňuje uživatelům najednou otevřít dvě aplikace s podporou úplné multitaskingu. Tyto aplikace můžete spustit vedle sebe (prostředí) nebo jedna – nad the další (na výšku) v režimu rozdělenou obrazovkou.
Uživatelům můžete přetáhnout na oddělovač mezi aplikacemi, změnit jejich velikost, a můžete vyjmout a vložit obsah mezi aplikacemi. Pokud dvě aplikace jsou uvedeny v režimu více oken, zůstane spuštěný nevybrané aktivity je pozastavené, ale stále zobrazená vybranou aktivitou. Režim více okna neprovede žádné změny životního cyklu aktivita pro Android.

[![Příklad aplikace běžící v režimu na výšku a šířku více oken](nougat-images/multi-window-mode.png)](nougat-images/multi-window-mode.png#lightbox)

Můžete nakonfigurovat, jak aktivity aplikace Xamarin.Android podporují více okno režimu. Můžete například nakonfigurovat atributy, které nastavit minimální velikost a výchozí výškou a šířkou vaší aplikace v režimu více oken. Můžete použít novou `Activity.IsInMultiWindowMode` vlastnosti k určení, zda je vaše aktivity v režimu více oken. Příklad:

```csharp
if (!IsInMultiWindowMode) {
    multiDisabledMessage.Visibility = ViewStates.Visible;
} else {
    multiDisabledMessage.Visibility = ViewStates.Gone;
}
```

[MultiWindowPlayground](https://developer.xamarin.com/samples/monodroid/android-n/MultiWindowPlayground/) ukázkovou aplikaci zahrnuje kód jazyka C#, který ukazuje, jak využít výhod více oken uživatelská rozhraní s vaší aplikací.

Další informace o režimu více oken, najdete v článku [podpora více oken](https://developer.android.com/guide/topics/ui/multi-window.html).



### <a name="enhanced-notifications"></a>Vylepšené oznámení

Android verzi Nougat zavádí přepracovaný oznámení systému. Obsahuje novou funkci přímé odpovědi, které umožňuje uživatelům rychlé reakce na oznámení pro příchozí textové zprávy přímo v oznámení uživatelského rozhraní. Spouští se systémem Android 7.0, oznámení, že se zprávy mohou být spojeny dohromady jako jedné skupiny při doručení zprávy více než jeden. Vývojáři můžou také přizpůsobit oznámení zobrazení, využívat systém dekorace v oznámeních a využijte výhod nových šablon oznámení při generování oznámení.


#### <a name="direct-reply"></a>Přímé odpovědi

Když uživatel obdrží oznámení pro příchozí zprávy, s Androidem verzi Nougat umožňuje odpovědět na zprávu v rámci oznámení (spíše než otevřete aplikaci zasílání zpráv k odeslání odpovědi).
Tato funkce odpověď vložené umožňuje uživatelům rychle reagovat na zprávu SMS nebo text přímo v rámci rozhraní oznámení:

[![Snímek obrazovky s polem přímé odpovědi vložené oznámení](nougat-images/notifications-inline-reply-sml.png)](nougat-images/notifications-inline-reply.png#lightbox)

Pokud chcete tuto funkci podporovat ve vaší aplikaci, musíte přidat *vložené akce odpovědi* do vaší aplikace prostřednictvím [RemoteInput](https://developer.xamarin.com/api/type/Android.App.RemoteInput/) objektu tak, aby uživatelé můžete odpovědět prostřednictvím text přímo z oznámení uživatelského rozhraní.
Například následující kód sestavení `RemoteInput` pro příjem textové zadání, vytvoří pendingintent pro akci odpovědi a vytvořit vzdáleného vstupní povolené akce:

```csharp
// Build a RemoteInput for receiving text input:
var remoteInput = new Android.Support.V4.App.RemoteInput.Builder (EXTRA_REMOTE_REPLY)
    .SetLabel (GetString (Resource.String.reply))
    .Build ();

// Build a Pending Intent for the reply action to trigger:
PendingIntent replyIntent = PendingIntent.GetBroadcast (ApplicationContext,
                                conversation.ConversationId,
                                GetMessageReplyIntent (conversation.ConversationId),
                                PendingIntentFlags.UpdateCurrent);

// Build an Android 7.0 compatible Remote Input enabled action:
NotificationCompat.Action actionReplyByRemoteInput = new NotificationCompat.Action.Builder (
    Resource.Drawable.notification_icon,
    GetString (Resource.String.reply),
    replyIntent).AddRemoteInput (remoteInput).Build ();
```

Tuto akci je přidat do oznámení:

```csharp
// Create the notification:
NotificationCompat.Builder builder = new NotificationCompat.Builder (ApplicationContext)
   .SetSmallIcon (Resource.Drawable.notification_icon)
   ...
   .AddAction (actionReplyByRemoteInput);
```

[Služba zasílání zpráv](https://developer.xamarin.com/samples/monodroid/android-n/MessagingService/) zahrnuje kód v C#, který ukazuje, jak rozšířit oznámení s ukázkovou aplikaci `RemoteInput` objektu. Další informace o přidání odpověď vložené akce, které vaše aplikace pro Android 7.0 nebo novější, najdete v článku Android [odpověď na oznámení](https://developer.android.com/guide/topics/ui/notifiers/notifications.html#direct) tématu.


#### <a name="bundled-notifications"></a>Jako součást balíčku oznámení

Android verzi Nougat můžete seskupit zpráv s oznámením (třeba podle zpráv tématu) a zobrazit skupiny spíše než každé samostatné zprávy.
To *dodávat oznámení* funkce umožňuje uživatelům zavřít nebo z archivní skupinu oznámení v jedné akci. Uživatel může snímku dolů a rozbalte sadu oznámení zobrazíte jednotlivá oznámení podrobně:

[![Snímek obrazovky s příkladem připojené oznámení](nougat-images/bundled-notifications-sml.png)](nougat-images/bundled-notifications.png#lightbox)

Pro podporu sady oznámení, vaše aplikace může používat [Builder.SetGroup](https://developer.xamarin.com/api/member/Android.App.Notification+Builder.SetGroup/p/System.String/) metoda Seskupit podobné oznámení. Další informace o skupinách připojené oznámení v systému Android N, naleznete v části Android [sdružování oznámení](https://developer.android.com/guide/topics/ui/notifiers/notifications.html#bundle) tématu.


#### <a name="custom-views"></a>Vlastní zobrazení

Android verzi Nougat umožňuje vytvářet vlastní oznámení zobrazení systému oznámení záhlaví, akcí a rozšiřitelná rozložení. Další informace o zobrazení vlastní oznámení ve verzi Nougat s Androidem, najdete v části Android [vylepšení oznámení](https://developer.android.com/about/versions/nougat/android-7.0.html#notification_enhancements) tématu.



### <a name="data-saver"></a>Spořič dat

Od verze Android verzi Nougat, uživatelé můžou povolit nové *Data spořič* nastavení, že bloky využití dat na pozadí. Toto nastavení také signály aplikace pro použití méně dat v popředí, kdykoli je to možné. [ConnectivityManager](https://developer.xamarin.com/api/type/Android.Net.ConnectivityManager/) bylo rozšířeno v Androidu verzi Nougat tak, aby vaše aplikace můžete zkontrolovat, zda má uživatel povolen spořič Data tak, aby vaše aplikace může měli snažit omezit jeho využití dat je-li spořič Data.

Další informace o nové funkci spořič dat v Androidu verzi Nougat, najdete v části Android [optimalizace využití dat sítě](https://developer.android.com/training/basics/network-ops/data-saver.html) tématu.



### <a name="app-shortcuts"></a>Zástupci aplikací

Android 7.1 zavedená *zástupci aplikací* funkce, která umožňuje uživatelům rychlé spuštění běžné nebo doporučené úlohy s vaší aplikací.
Aktivujte nabídku zkratek uživatel dlouho stisknutí ikonu aplikace pro sekundy nebo více &ndash; s rychlé pronikavost se zobrazí v nabídce.
Uvolnění stisknutím způsobí, že v nabídce zůstat:

[![Příklad obrazovky místní nabídky aplikaci pro aplikaci zasílání zpráv](nougat-images/app-shortcuts-sml.png)](nougat-images/app-shortcuts.png#lightbox)

Tato funkce je k dispozici jenom úroveň rozhraní API 25 nebo vyšší.
Další informace o nové funkci zástupci aplikací v Android 7.1, najdete v části Android [zástupci aplikací](https://developer.android.com/guide/topics/ui/shortcuts.html) tématu.


### <a name="sample-code"></a>Ukázkový kód

Několik ukázky Xamarin.Android jsou k dispozici ukazují, jak využít výhod funkcí pro verzi Nougat Android:

-   [MultiWindowPlayground](https://developer.xamarin.com/samples/monodroid/android-n/MultiWindowPlayground/) ukazuje použití rozhraní API více oken, která je k dispozici ve verzi Nougat s Androidem. Ukázkové aplikace můžete přepnout do režimu více windows najdete v článku o jejím dopadu na životní cyklus a chování aplikace.

-   [Zasílání zpráv služby](https://developer.xamarin.com/samples/monodroid/android-n/MessagingService/) používá jednoduché služby, který odesílá oznámení `NotificationCompatManager`. Rozšiřuje také zobrazí oznámení `RemoteInput` objektu, který chcete povolit zařízení s Androidem verzi Nougat odpoví prostřednictvím text přímo z oznámení bez nutnosti otevírat aplikaci.

-   [Aktivní oznámení](https://developer.xamarin.com/samples/monodroid/android-n/ActiveNotifications/) ukazuje, jak používat `NotificationManager` API zjistit, kolik oznámení se aktuálně zobrazuje vaší aplikace.

-   [Přístup k adresáři s rozsahem](https://developer.xamarin.com/samples/monodroid/android-n/ScopedDirectoryAccess/) ukazuje, jak používat vymezenou directory přístup k rozhraní API pro snadný přístup ke konkrétní adresáře. Tato stránka slouží jako alternativu s tím, že k definování `READ_EXTERNAL_STORAGE` nebo `WRITE_EXTERNAL_STORAGE` oprávnění v manifestu.

-   [Přímé spouštěcí](https://developer.xamarin.com/samples/monodroid/android-n/DirectBoot/) ukazuje, jak ukládat data v úložišti zařízení zašifrované, které jsou vždycky dostupné, zatímco zařízení, který naběhne i před a po zadání credentials(PIN/Pattern/Password) všechny uživatele.


## <a name="summary"></a>Souhrn

Tento článek zavedené Android verzi Nougat a bylo vysvětleno, jak nainstalovat a nakonfigurovat nejnovější nástroje a balíčky pro Xamarin.Android vývoj na platformě Nougat s Androidem. Také poskytuje přehled klíčových funkcí, které jsou k dispozici v Androidu verzi Nougat s odkazy na ukázkovém zdrojovém kódu můžete pustit do vytváření aplikací pro Android verzi Nougat práce.


## <a name="related-links"></a>Související odkazy

- [Android 7.1 pro vývojáře](https://developer.android.com/about/versions/nougat/android-7.1.html)
- [Zpráva k vydání verze pro Xamarin Android 7.0](https://developer.xamarin.com/releases/android/xamarin.android_7/xamarin.android_7.0/)
