---
title: "Funkce cukrovinkách typu nugát"
description: "Jak začít používat k vývoji aplikací pro Android cukrovinkách typu nugát Xamarin.Android."
ms.topic: article
ms.prod: xamarin
ms.assetid: 5C74ABE2-C862-4ED0-8EA5-C7FEE5251D4B
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 03/01/2018
ms.openlocfilehash: c666b7d5b680eab3c990950569868eacdb6f30af
ms.sourcegitcommit: 0fdb243b46cf21be47584900805cadcd077121bf
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 03/12/2018
---
# <a name="nougat-features"></a>Funkce cukrovinkách typu nugát

_Jak začít používat k vývoji aplikací pro Android cukrovinkách typu nugát Xamarin.Android._

Tento článek obsahuje přehled funkce byla zavedená v systému Android cukrovinkách typu nugát, vysvětluje, jak připravit Xamarin.Android pro vývoj pro Android cukrovinkách typu nugát a poskytuje odkazy na ukázkové aplikace, které ukazují, jak používat funkce Android cukrovinkách typu nugát v Aplikace Xamarin.Android.


## <a name="overview"></a>Přehled

[Android cukrovinkách typu nugát](https://developer.android.com/about/versions/nougat/android-7.0.html) je následnými Google na Android 6.0 Marshmallow. Poskytuje podporu pro Xamarin.Android **Android 7.x vazby** v Xamarin Android 7.0 nebo novější. Android cukrovinkách typu nugát přidá mnoho nových rozhraní API pro cukrovinkách typu nugát funkce popsané níže; Tato rozhraní API jsou k dispozici pro xamarin.Android se předpokládá, pokud použijete Xamarin.Android 7.0.

[![Nejdůležitější bitové kopie Android tablety a telefony s Androidem cukrovinkách typu nugát](nougat-images/android-n-hero-sml.png)](nougat-images/android-n-hero.png#lightbox)

Další informace o Android 7.x rozhraní API najdete v tématu [Android 7.1 pro vývojáře](http://developer.android.com/preview/api-overview.html).
Seznam známých problémů Xamarin.Android 7.0, najdete v tématu [poznámky k verzi](https://developer.xamarin.com/releases/android/xamarin.android_7/xamarin.android_7.0/).

Android cukrovinkách typu nugát poskytuje vývojářům Xamarin.Android mnoha nových funkcí, které vás zajímají. Tyto funkce patří:

-   **Podpora více oken** &ndash; toto vylepšení umožňuje uživatelům otevřít dvě aplikace na obrazovce najednou.

-   **Vylepšení oznámení** &ndash; obsahuje přepracovanou oznámení systému v Android cukrovinkách typu nugát *přímé odpovědi* funkce, která umožňují uživatelům rychle reagovat na textové zprávy přímo z oznámení. UŽIVATELSKÉ ROZHRANÍ. Navíc pokud vaše aplikace vytvoří oznámení pro přijaté zprávy, nové *dodávat oznámení* funkce můžete svázat oznámení společně jako jednu skupinu při příjmu zprávy více než jeden.

-   **Data šetřič** &ndash; tato funkce je nová služba systému, která pomáhá snižovat mobilních dat pomocí aplikace; poskytuje uživatelům ovládat, jak aplikace používat mobilní data.

Kromě toho Android cukrovinkách typu nugát přináší mnoho dalších vylepšení zajímat vývojáři aplikací, jako je například nové funkce Konfigurace zabezpečení sítě, Doze na cestách, ověření, nové rychlé nastavení rozhraní API, podpora více národního prostředí, ICU4J rozhraní API, webového zobrazení vylepšení, klíče přístup k funkcím jazyka Java 8, přístup k oboru adresářové, rozhraní API vlastní ukazatele, podpora VR platformy, virtuální soubory a zpracování optimalizace na pozadí.

Tento článek vysvětluje, jak začít vytváření aplikací s Androidem cukrovinkách typu nugát vyzkoušet nové funkce a plánování pracovní migrace nebo funkce pro nové platformu Android cukrovinkách typu nugát.


## <a name="requirements"></a>Požadavky

Toto je potřeba používat nové funkce systému Android cukrovinkách typu nugát v založené na Xamarinu aplikace:

-   **Visual Studio nebo Visual Studio pro Mac** &ndash; Pokud používáte Visual Studio, verze 4.2.0.628 nebo novější z Xamarin pro Visual Studio se vyžaduje. Pokud používáte Visual Studio pro Mac, verze 6.1.0 nebo později sady Visual Studio pro Mac, je nutné.

-   **Xamarin.Android** &ndash; Xamarin.Android 7.0 nebo novější musí být nainstalovaná a nakonfigurovaná s Visual Studio nebo Visual Studio for Mac.

-   **Sady SDK pro Android** -Android SDK 7.0 (24 rozhraní API) nebo novější musí být nainstalován prostřednictvím Android SDK Manager.

-   **Sady pro vývojáře Java** &ndash; Xamarin Android 7.0 vývoj vyžaduje [JDK 8](http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html) nebo novější, pokud vyvíjíte pro úroveň rozhraní API 24 nebo větší (JDK 8 také podporuje úrovně rozhraní API starší než 24). Pokud používáte vlastní ovládací prvky nebo prohlížeč formulářů je vyžaduje 64bitovou verzi 8 JDK.

> [!IMPORTANT]
> Xamarin.Android nepodporuje JDK 9.

Všimněte si, že aplikace je třeba znovu vytvořit s Xamarin C6SR4 nebo novější fungovat spolehlivě se systémem Android cukrovinkách typu nugát. Protože Android cukrovinkách typu nugát můžete propojit pouze [zadaný NDK nativní knihovny](https://developer.android.com/about/versions/nougat/android-7.0-changes.html), existující aplikace pomocí knihovny, například **Mono.Data.Sqlite.dll** může dojít k chybě při spuštění na Android cukrovinkách typu nugát, pokud nejsou správně znovu sestavit.



## <a name="getting-started"></a>Začínáme

Abyste mohli začít používat Android cukrovinkách typu nugát s Xamarin.Android, musíte stáhnout a nainstalovat nejnovější nástroje a balíčky SDK před vytvořením projektu Android cukrovinkách typu nugát:

1.  Nainstalujte nejnovější aktualizace Xamarin.Android z Xamarin.

2.  Nainstalujte **Android 7.0 (rozhraní API 24)** balíčky a nástroje nebo novější.

3.  Vytvoření nového projektu Xamarin.Android s cílem Android cukrovinkách typu nugát.

4.  Konfigurace pro Android cukrovinkách typu nugát emulátoru nebo zařízení.

Každý z těchto kroků je vysvětlené v následujících částech:


### <a name="install-xamarin-updates"></a>Nainstalovat Xamarin aktualizace

Chcete-li přidat podporu Xamarin Android cukrovinkách typu nugát, změňte kanálu aktualizace v sadě Visual Studio nebo Visual Studio pro Mac stabilní kanálu a použít nejnovější aktualizace. Pokud budete také potřebovat funkce, které jsou aktuálně k dispozici pouze v kanálu alfa nebo Beta, můžete přepnout do kanálu alfa nebo Beta (kanály Alpha a Beta taky poskytuje podporu pro Android 7.x). Informace o tom, jak změnit kanál aktualizace (verze) najdete v tématu [změna v kanálu aktualizace](https://developer.xamarin.com/recipes/cross-platform/ide/change_updates_channel/).



### <a name="install-the-android-sdk"></a>Instalace sady SDK pro Android

Vytvoření projektu s Xamarin Android 7.0, musíte nejprve použít Android SDK Manager k instalaci **SDK platformy Android N (rozhraní API 24)** nebo novější. Je také nutné nainstalovat nejnovější **nástroje pro Android SDK**:

1.  Spustit Android SDK Manager (v sadě Visual Studio pro Mac, použijte **nástroje > otevřete Android SDK Manager&hellip;**; v sadě Visual Studio, použijte **nástroje > Android > Android SDK Manager**).

2.  Nainstalujte **Android 7.0 (rozhraní API 24)** nebo novější:

    [![Výběr Android 7.0 balíčky v Android SDK Manager](nougat-images/preview-packages.png)](nougat-images/preview-packages.png#lightbox)

3.  Nainstalujte nejnovější nástroje pro Android SDK:

    [![Výběr nejnovější nástroje potřebné sady SDK pro Android v Android SDK Manager](nougat-images/preview-tools.png)](nougat-images/preview-tools.png#lightbox)

    Musíte nainstalovat nástroje pro Android SDK revize 25.2.2 nebo novější, Android nástrojů platformy SDK 24.0.3 nebo novější a Android nástroje sestavení sady SDK 24.0.2 nebo novější.

4.  Ověřte, zda **Java Development Kit umístění** je nakonfigurován pro JDK 1.8:

    [![Konfigurace cesta JDK 8 v části Možnosti nástrojů](nougat-images/use-jdk-1.8.png)](nougat-images/use-jdk-1.8.png#lightbox)

    Chcete-li toto nastavení můžete zobrazit v sadě Visual Studio, klikněte na tlačítko **nástroje > Možnosti > Xamarin > Nastavení Androidu**. V sadě Visual Studio pro Mac, klikněte na tlačítko **Předvolby > Projekty > SDK umístění > Android**.



### <a name="start-a-xamarinandroid-project"></a>Zahájení projektu Xamarin.Android

Vytvoření nového projektu Xamarin.Android. Pokud jste pro vývoj pro Android pomocí Xamarinu nové, přečtěte si téma [Hello, Android](~/android/get-started/hello-android/index.md) Další informace o vytváření projektů Xamarin.Android.

Když vytvoříte projekt pro Android, musíte nakonfigurovat nastavení verze cíl Android 7.0 nebo novější. Například pokud chcete zacílit na projekt pro Android 7.0, musíte nakonfigurovat úroveň cílové rozhraní API systému Android projektu pro **Android 7.0 (rozhraní API 24 - cukrovinkách typu nugát)**. Doporučujeme nastavit cílové úrovni framework na rozhraní API 24 nebo novější. Další informace o konfiguraci úrovní úrovně rozhraní API systému Android, najdete v části [Principy Android API úrovně](~/android/app-fundamentals/android-api-levels.md).


> [!NOTE]
> Aktuálně je nutné nastavit **minimální verzi systému Android** k **Android 7.0 (rozhraní API 24 - cukrovinkách typu nugát)** k nasazení aplikace do zařízení Android cukrovinkách typu nugát nebo emulátorů.



### <a name="configure-an-emulator-or-device"></a>Konfigurace zařízení nebo emulátoru

Pokud používáte emulátor, spusťte Správce AVD Android a vytvoření nového zařízení s následujícím nastavením:

-   Zařízení: Nexus 5 X Nexus 6, Nexus 6P Nexus Player Nexus 9 nebo pixelů C.
-   Cíl: Android 7.0 - úroveň rozhraní API 24
-   ABI: x86 nebo x86\_64

Tato virtuální zařízení je například nakonfigurován emulovat Nexus 6:

[![Konfigurace AVD pomocí zařízení Nexus 6, cílový Android 7.0 a Intel Atom x86 CPU/ABI](nougat-images/android-n-avd.png)](nougat-images/android-n-avd.png#lightbox)

Pokud používáte fyzické zařízení, jako je například Nexus 5 X, 6 nebo 9, můžete buď zařízení prostřednictvím automaticky aktualizovat prostřednictvím aktualizace letecké (OTA) nebo stáhnout bitovou kopii systému a flash zařízení přímo. Další informace o ruční aktualizaci zařízení pro Android cukrovinkách typu nugát najdete v tématu [OTA obrázků pro zařízení Nexus](https://developers.google.com/android/nexus/ota).

Všimněte si, že zařízení Nexus 5 nepodporuje Android cukrovinkách typu nugát.



## <a name="new-features"></a>Nové funkce

Android cukrovinkách typu nugát zavádí řadu nových funkcí a možností, jako je podpora více oken, vylepšení oznámení a šetřič Data. V následujících částech zvýrazněte tyto funkce a poskytuje odkazy na vám pomůže začít používat je ve vaší aplikaci.



### <a name="multi-window-mode"></a>Režim více okna

Více oken režim umožňuje uživatelům otevřít dvě aplikace současně s podporou úplné multitasking. Tyto aplikace, můžete spustit v režimu rozdělenou obrazovkou souběžného (na šířku) nebo jedna výše the další (na výšku).
Uživatele můžete přetáhnout na příčku mezi aplikacemi na jejich velikosti a můžou vyjmout a vložit obsah mezi aplikacemi. Když jsou v režimu více okno dvě aplikace, i nadále spustit, když nezaškrtnuté aktivity je pozastavený, ale stále viditelné vybranou aktivitou. Více oken režimu nedojde ke změně životního cyklu Android aktivity.

[![Příklad aplikace spuštěna v režimu více oken na šířku i na výšku](nougat-images/multi-window-mode.png)](nougat-images/multi-window-mode.png#lightbox)

Můžete nakonfigurovat, jak aktivity aplikace Xamarin.Android podporuje více oken režim. Můžete například nakonfigurovat atributy, které nastavit minimální velikost a výchozí výška a šířka vaší aplikace v režimu více oken. Můžete použít nové `Activity.IsInMultiWindowMode` vlastnosti k určení, pokud vaše aktivita je v režimu více oken. Příklad:

```csharp
if (!IsInMultiWindowMode) {
    multiDisabledMessage.Visibility = ViewStates.Visible;
} else {
    multiDisabledMessage.Visibility = ViewStates.Gone;
}
```

[MultiWindowPlayground](https://developer.xamarin.com/samples/monodroid/android-n/MultiWindowPlayground/) ukázková aplikace obsahuje kód C#, které ukazuje, jak chcete využít výhod více okno uživatelského rozhraní s vaší aplikací.

Další informace o více okno režimu najdete v tématu [podpora více oken](https://developer.android.com/guide/topics/ui/multi-window.html).



### <a name="enhanced-notifications"></a>Rozšířené oznámení

Android cukrovinkách typu nugát zavádí systém přepracovanou oznámení. Jeho součástí jsou nové funkce přímé odpovědi, která umožňuje uživatelům rychle odpovědi na oznámení pro příchozí textové zprávy přímo v oznámení uživatelského rozhraní. Od verze Android 7.0, oznámení, že zprávy mohou být spojeny dohromady jako jednu skupinu při příjmu zprávy více než jeden. Vývojáři navíc můžete přizpůsobit oznámení zobrazení, využívají dekorace systému v oznámeních a využívat nové šablony oznámení při generování oznámení.


#### <a name="direct-reply"></a>Přímé odpovědi

Když uživatel obdrží oznámení pro příchozí zprávy, Android cukrovinkách typu nugát umožňuje odpovědět na zprávu v rámci oznámení (nikoli otevře aplikace zasílání zpráv pro odesílání odpovědí).
Tato funkce vložené odpovědi umožňuje uživatelům rychle reagovat na zprávu SMS nebo text přímo v rámci rozhraní oznámení:

[![Snímek obrazovky oznámení s na vložené přímé odpovědi pole](nougat-images/notifications-inline-reply-sml.png)](nougat-images/notifications-inline-reply.png#lightbox)

Chcete-li tuto funkci podporovat ve vaší aplikaci, musíte přidat *akce odpovědi vložené* do vaší aplikace prostřednictvím [RemoteInput](https://developer.xamarin.com/api/type/Android.App.RemoteInput/) objektu tak, aby uživatelé můžete odpověď prostřednictvím textové přímo z uživatelského rozhraní oznámení.
Například následující kód sestavení `RemoteInput` pro příjem zadávání textu, vytvoří čekající záměr pro akce odpovědi a vytvořit vzdálené vstupní povolené akce:

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

Tato akce je přidána na oznámení:

```csharp
// Create the notification:
NotificationCompat.Builder builder = new NotificationCompat.Builder (ApplicationContext)
   .SetSmallIcon (Resource.Drawable.notification_icon)
   ...
   .AddAction (actionReplyByRemoteInput);
```

[Služba zasílání zpráv](https://developer.xamarin.com/samples/monodroid/android-n/MessagingService/) ukázková aplikace obsahuje kód C#, které ukazuje, jak rozšířit oznámení s `RemoteInput` objektu. Další informace o přidání odpovědi vložené akce do vaší aplikace pro Android 7.0 nebo novější, najdete v části pro Android [odpovídání na oznámení](https://developer.android.com/guide/topics/ui/notifiers/notifications.html#direct) tématu.


#### <a name="bundled-notifications"></a>Připojené oznámení

Android cukrovinkách typu nugát Seskupit zprávy oznámení (například podle zprávy téma), zobrazí skupiny, nikoli každou samostatnou zprávu.
To *dodávat oznámení* funkce umožňuje uživatelům zavřít nebo archivovat skupinu v jednu akci oznámení. Uživatel může snímku dolů rozbalte sady oznámení zobrazíte jednotlivá oznámení podrobně:

[![Snímek obrazovky příklad připojené oznámení](nougat-images/bundled-notifications-sml.png)](nougat-images/bundled-notifications.png#lightbox)

Pro podporu připojené oznámení, můžete použít aplikaci [Builder.SetGroup](https://developer.xamarin.com/api/member/Android.App.Notification+Builder.SetGroup/p/System.String/) metoda sady podobné oznámení. Další informace o skupinách připojené oznámení v Android N najdete v tématu Android [sdružování oznámení](https://developer.android.com/guide/topics/ui/notifiers/notifications.html#bundle) tématu.


#### <a name="custom-views"></a>Vlastní zobrazení

Android cukrovinkách typu nugát umožňuje vytvářet vlastní oznámení zobrazení s hlavičky oznámení systému, akce a rozšíření rozložení. Další informace o zobrazení vlastní oznámení v Android cukrovinkách typu nugát najdete v tématu Android [oznámení vylepšení](https://developer.android.com/about/versions/nougat/android-7.0.html#notification_enhancements) tématu.



### <a name="data-saver"></a>Spořič dat

Od verze Android cukrovinkách typu nugát, uživatelé mohou povolit nové *Data šetřič* nastavení, že bloky využití dat na pozadí. Toto nastavení také dá signál aplikaci použít méně dat v popředí, pokud je to možné. [ConnectivityManager](https://developer.xamarin.com/api/type/Android.Net.ConnectivityManager/) bylo rozšířeno v Android cukrovinkách typu nugát tak, aby vaše aplikace můžete zkontrolovat, zda má uživatel povolen šetřič dat tak, aby vaše aplikace může provést snaze omezit jeho využití dat, pokud je povoleno šetřič Data.

Další informace o nové funkci šetřič Data v Android cukrovinkách typu nugát najdete v tématu Android [optimalizace využití sítě Data](https://developer.android.com/training/basics/network-ops/data-saver.html) tématu.



### <a name="app-shortcuts"></a>Zástupce aplikace

Android 7.1 zavedená *zástupce aplikace* funkce, která umožňuje uživatelům rychle počáteční běžné nebo doporučené úkoly s vaší aplikací.
Pokud chcete aktivovat v nabídce zástupce, uživatel dlouho stisknutí ikonu pro aplikace pro druhý nebo více &ndash; s rychlé vibracím se zobrazí v nabídce.
Uvolnění stiskněte klávesu způsobí, že v nabídce zůstat:

[![Příklad obrazovky místní nabídky aplikaci pro aplikaci zasílání zpráv](nougat-images/app-shortcuts-sml.png)](nougat-images/app-shortcuts.png#lightbox)

Tato funkce je k dispozici pouze úroveň rozhraní API 25 nebo vyšší.
Další informace o nové funkci zástupce aplikace v nástroji Android 7.1 najdete v tématu Android [zástupce aplikace](https://developer.android.com/guide/topics/ui/shortcuts.html) tématu.


### <a name="sample-code"></a>Ukázkový kód

Několik ukázky Xamarin.Android jsou k dispozici pro ukazují, jak můžete využít výhod funkcí Android cukrovinkách typu nugát:

-   [MultiWindowPlayground](https://developer.xamarin.com/samples/monodroid/android-n/MultiWindowPlayground/) demonstruje použití rozhraní API služby Windows, která je k dispozici v systému Android cukrovinkách typu nugát. Ukázková aplikace můžete přepnout do režimu více windows zobrazíte jak ovlivňuje chování a životního cyklu aplikace.

-   [Zasílání zpráv Service](https://developer.xamarin.com/samples/monodroid/android-n/MessagingService/) používá jednoduchý služby, který odesílá oznámení `NotificationCompatManager`. Rozšiřuje také oznámení s `RemoteInput` objekt, který chcete povolit zařízením Android cukrovinkách typu nugát odpověď prostřednictvím textové přímo z oznámení bez nutnosti otevřít aplikaci.

-   [Aktivní oznámení](https://developer.xamarin.com/samples/monodroid/android-n/ActiveNotifications/) ukazuje, jak používat `NotificationManager` rozhraní API s oznámením, kolik oznámení je aktuálně zobrazení vaší aplikace.

-   [Obor přístupu Directory](https://developer.xamarin.com/samples/monodroid/android-n/ScopedDirectoryAccess/) ukazuje, jak používat vymezenou directory přístup rozhraní API pro snadno přístup k konkrétní adresáře. Tato stránka slouží jako alternativu k nutnosti definovat `READ_EXTERNAL_STORAGE` nebo `WRITE_EXTERNAL_STORAGE` oprávnění v manifestu.

-   [Přímé spouštění](https://developer.xamarin.com/samples/monodroid/android-n/DirectBoot/) ukazuje, jak k ukládání dat v úložišti zařízení zašifrovaná, která je vždy k dispozici, když zařízení, který naběhne i před a po zadání všech uživatelů credentials(PIN/Pattern/Password).


## <a name="summary"></a>Souhrn

Tento článek zavedená Android cukrovinkách typu nugát a vysvětlení, jak nainstalovat a nakonfigurovat na Android cukrovinkách typu nugát nejnovější nástroje a balíčky pro vývoj pro Xamarin.Android. Také poskytuje přehled klíčových funkcí, které jsou k dispozici v systému Android cukrovinkách typu nugát, s odkazy na příkladu zdrojového kódu můžete začít vytváření aplikací pro Android cukrovinkách typu nugát.


## <a name="related-links"></a>Související odkazy

- [Android 7.1 pro vývojáře](https://developer.android.com/about/versions/nougat/android-7.1.html)
- [Poznámky k verzi Xamarin Android 7.0](https://developer.xamarin.com/releases/android/xamarin.android_7/xamarin.android_7.0/)
