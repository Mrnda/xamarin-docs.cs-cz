---
title: "Místní oznámení"
description: "V této části ukazuje, jak implementovat místní oznámení v Xamarin.Android. To vysvětluje různé prvky uživatelského rozhraní Android oznámení a popisuje rozhraní API je zahrnuta s vytváření a zobrazování oznámení."
ms.topic: article
ms.prod: xamarin
ms.assetid: 03E19D14-7C81-4D5C-88FC-C3A3A927DB46
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/16/2018
ms.openlocfilehash: 7566ebac0f487ef321c512c988c79f34e50777ac
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 02/27/2018
---
# <a name="local-notifications"></a>Místní oznámení

_V této části ukazuje, jak implementovat místní oznámení v Xamarin.Android. To vysvětluje různé prvky uživatelského rozhraní Android oznámení a popisuje rozhraní API je zahrnuta s vytváření a zobrazování oznámení._

## <a name="local-notifications-overview"></a>Přehled místní oznámení

Toto téma vysvětluje, jak implementovat místní oznámení v aplikaci Xamarin.Android. Popisuje různé součásti Android oznámení, vysvětluje styly různých oznámení, které jsou k dispozici pro vývojáře aplikací a zavádí některé z rozhraní API, které se používají k vytvoření a publikování oznámení.

Android poskytuje dvě systém řídí oblasti pro zobrazení ikon upozornění a oznámení o uživateli. Při prvním publikování oznámení, zobrazí se jeho ikona v *oznamovací oblasti*, jak je znázorněno na následujícím snímku obrazovky:

![Příklad oznamovací oblasti na zařízení](local-notifications-images/01-notification-shade.png)

Pokud chcete získat podrobnosti o oznámení, může uživatel otevřít panel oznámení (který rozbalí každý ikonu oznámení a odhalit obsah oznámení) a provádět všechny akce přidružené k oznámení. Ukazuje snímek následující obrazovky *nástroj oznámení drawer* odpovídající oznamovací oblasti zobrazí výše:

[![Příklad oznámení zásuvky zobrazení tři oznámení](local-notifications-images/02-notification-drawer-sml.png)](local-notifications-images/02-notification-drawer.png)

Oznámení systému Android používat dva typy rozložení:

-   ***Základní rozložení*** &ndash; compact, pevné prezentace formátu.

-   ***Rozšířené rozložení*** &ndash; prezentace formátu, který můžete rozšířit na větší velikost, získáte další informace.

Každý z těchto typů rozložení (a postupy při jejich vytváření) je vysvětlené v následujících částech.

<a name="base-layout" />

### <a name="base-layout"></a>Základní rozložení

Všechna oznámení Android jsou postaveny na základní rozložení formátu, který minimálně obsahuje následující prvky:

1.  A *ikonu oznámení*, který představuje původní aplikaci nebo typ oznámení, pokud aplikace podporuje různé typy oznámení.

2.  Oznámení *název*, nebo název odesílatele, pokud se oznámení o osobní zprávu.

3.  Oznámení.

4.  A *časové razítko*.

Tyto prvky jsou zobrazeny, jak je znázorněno v následujícím diagramu:

[![Umístění elementů oznámení](local-notifications-images/03-notification-callouts-sml.png)](local-notifications-images/03-notification-callouts.png)

Základní rozložení jsou omezená na 64 nezávislé na hustotě pixelů (dp) na výšku. Android vytvoří tento styl základní oznámení ve výchozím nastavení.

Oznámení můžete volitelně zobrazit velké ikonu, která představuje odesílatele fotografií nebo aplikace. Při velkých ikona je použita v oznámení v systému Android 5.0 nebo novější, ikonu malé oznámení se zobrazí jako oznámení "BADGE" přes velkých ikon:

![Fotografie jednoduché oznámení](local-notifications-images/04-simple-notification-photo.png)

Od verze Android 5.0, oznámení můžete také zobrazit na zamykací obrazovky:

[![Příklad zamykací obrazovky oznámení](local-notifications-images/05-lockscreen-notification-sml.png)](local-notifications-images/05-lockscreen-notification.png)

Uživatel může poklepání zamykací obrazovky oznámení k odemknutí zařízení a přejít na aplikaci, která pochází oznámení, nebo prstem k zavření oznámení. Aplikace můžete nastavit úroveň viditelnosti oznámení k řízení, co se zobrazí na zamykací obrazovky a mohli uživatelé vybrat, jestli se má povolit citlivého obsahu, která se má zobrazit v oznámeních zamykací obrazovky.

Android 5.0 zavedená do formátu prezentace oznámení s vysokou prioritou *z pohotového*. Oznámení z pohotového posuňte se dolů z horní části obrazovky na několik sekund a pak retreat zálohovat oznamovací oblasti:

[![Příklad heads-up oznámení](local-notifications-images/06-heads-up-notification-sml.png)](local-notifications-images/06-heads-up-notification.png)

Oznámení z pohotového umožňují systému uživatelského rozhraní pro umístění důležité informace u uživatele, a to bez přerušení stavu aktuálně probíhající aktivity.

Android zahrnuje podporu pro metadata oznámení tak, aby oznámení lze seřadit a zobrazit inteligentně. Metadata oznámení také určuje, jak mají zobrazovat oznámení zamykací obrazovky a ve formátu z pohotového. Aplikace můžete nastavit následující typy oznámení metadat:

-   **Priorita** &ndash; úroveň priority určuje, jak a kdy se zobrazí oznámení. Například v systému Android 5.0 se zobrazují oznámení s vysokou prioritou jako z pohotového oznámení.

-   **Viditelnost** &ndash; Určuje, kolik oznámení obsahu se zobrazí, když se zobrazí oznámení na zamykací obrazovky.

-   **Kategorie** &ndash; informuje určuje způsob zpracování oznámení v různých situacích, například pokud je zařízení v systému *nebudou narušovat* režimu.

**Poznámka:** **viditelnost** a **kategorie** byly zavedeny v systému Android 5.0 a nejsou k dispozici v dřívějších verzích systému Android. Od verze Android 8.0 [kanály oznámení](#notif-chan) je možné určit, jak mají zobrazovat oznámení uživatelům.

<a name="expanded-layouts" />

### <a name="expanded-layouts"></a>Rozšířené rozložení

Od verze Android 4.1, oznámení se dá nakonfigurovat s stylů rozšířené rozložení, které umožňují uživatelům rozbalte výška oznámení. Chcete-li zobrazit další obsah. Například následující příklad ilustruje oznámení o rozšířené rozložení v smluvního režimu:

![Smluvní oznámení](local-notifications-images/07-contracted-notification.png)

Pokud je toto oznámení rozbalen, zobrazí se celá zpráva:

![Rozšířené oznámení](local-notifications-images/08-expanded-notification.png)

Android podporuje tři styly rozšířené rozložení pro jednotlivé události oznámení:

-   ***Dlouhý Text*** &ndash; v smluvního režimu zobrazí výňatek prvního řádku zprávy, za nímž následuje dvě tečky. V rozšířené režimu zobrazí celé zprávy (jak je vidět v předchozím příkladu).

-   ***Doručená pošta*** &ndash; v smluvního režimu zobrazuje počet nových zpráv. V rozšířené režimu zobrazí první e-mailové zprávě nebo seznam zpráv v doručené poště.

-   ***Obrázek*** &ndash; v smluvního režimu zobrazí pouze text zprávy. V rozšířené režimu zobrazí text a bitovou kopii.

[Kromě základních oznámení](#beyond-the-basic-notification) (dále v tomto článku) vysvětluje, jak vytvořit *dlouhý Text*, *doručené pošty*, a *Image* oznámení.

<a name="notification-creation" />

## <a name="notification-creation"></a>Vytvoření oznámení

Pokud chcete vytvořit oznámení v Android, můžete použít [Notification.Builder](https://developer.xamarin.com/api/type/Android.App.Notification+Builder/) třídy. `Notification.Builder` byla zavedena v systému Android 3.0 zjednodušení vytváření objektů oznámení. Pokud chcete vytvořit oznámení, které jsou kompatibilní se staršími verzemi systému Android, můžete použít [NotificationCompat.Builder](https://developer.android.com/reference/android/support/v4/app/NotificationCompat.Builder.html) místo `Notification.Builder` (najdete v části [kompatibility](#compatibility) dál v tomto tématu pro Další informace o používání `NotificationCompat.Builder`).
`Notification.Builder` poskytuje metody pro nastavení různých možností v oznámení, jako například:

-   Obsah, včetně nadpis, text zprávy a ikonu oznámení.

-   Styl oznámení, jako například *dlouhý Text*, *doručené pošty*, nebo *bitové kopie* stylu.

-   Priorita oznámení: minimální, nízká, výchozí, vysoká, nebo maximální.

-   Viditelnost oznámení na zamykací obrazovky: veřejné, privátní nebo tajný klíč.

-   Kategorie metadata, která pomáhá Android klasifikovat a filtrovat oznámení.

-   Volitelné záměrem určující aktivitu spustit, když je stisknuté oznámení.

Po nastavení těchto možností v Tvůrce generování oznámení objekt, který obsahuje nastavení. Publikování oznámení, předáte tento objekt oznámení *Správce oznámení*. Poskytuje Android [NotificationManager](https://developer.xamarin.com/api/type/Android.App.NotificationManager/) třídy, která je zodpovědná za publikování oznámení a jejich zobrazení pro uživatele. Odkaz na tuto třídu můžete získat z jakýkoliv kontext, například aktivitu nebo služby.

<a name="how-to-generate" />

### <a name="how-to-generate-a-notification"></a>Jak vygenerovat oznámení

Generovat oznámení v Android, postupujte takto:

1.  Vytváření instancí `Notification.Builder` objektu.

2.  Volání na různé metody `Notification.Builder` objekt, který chcete nastavit možnosti oznámení.

3.  Volání [sestavení](https://developer.xamarin.com/api/member/Android.App.Notification+Builder.Build/) metodu `Notification.Builder` objekt, který chcete vytvořit instanci objektu oznámení.

4.  Volání [oznámení](https://developer.xamarin.com/api/member/Android.App.NotificationManager.Notify/(System.Int32%2cAndroid.App.Notification)) metoda Správce oznámení k publikování oznámení.

Je nutné zadat alespoň následující informace pro každý oznámení:

-   Malé ikony (24/24 distribučního bodu ve velikosti)

-   Krátký název

-   Do textu oznámení.

Následující příklad kódu ukazuje, jak používat `Notification.Builder` ke generování základní oznámení. Všimněte si, že `Notification.Builder` metody podporují [řetězení metod](http://en.wikipedia.org/wiki/Method_chaining); to znamená, každá metoda vrátí objekt tvůrce, takže výsledek posledního volání metody, které můžete vyvolat další volání metody:

```csharp
// Instantiate the builder and set notification elements:
Notification.Builder builder = new Notification.Builder (this)
    .SetContentTitle ("Sample Notification")
    .SetContentText ("Hello World! This is my first notification!")
    .SetSmallIcon (Resource.Drawable.ic_notification);

// Build the notification:
Notification notification = builder.Build();

// Get the notification manager:
NotificationManager notificationManager =
    GetSystemService (Context.NotificationService) as NotificationManager;

// Publish the notification:
const int notificationId = 0;
notificationManager.Notify (notificationId, notification);
```

V tomto příkladu novou `Notification.Builder` objektu s názvem `builder` se vytvořit instanci, jsou nastavené nadpisu a textu oznámení a ikonu oznámení je načtena z **Resources/drawable/ic_notification.png**. Volání Tvůrce oznámení `Build` metoda vytvoří objekt oznámení s těmito nastaveními. Dalším krokem je volání `Notify` metoda Správce oznámení. Chcete-li vyhledat Správce oznámení, zavolejte `GetSystemService`, jak je uvedeno výše.

`Notify` Metoda přijímá dva parametry: identifikátor oznámení a oznámení objekt. Oznámení identifikátor je jedinečný celé číslo, které identifikuje oznámení do vaší aplikace. V tomto příkladu je identifikátor oznámení nastaven na hodnotu nula (0); ale v případě produkční aplikace můžete poskytnout jedinečný identifikátor každého oznámení. Opětovné použití předchozích hodnota identifikátoru v volání `Notify` způsobí, že poslední oznámení přepsání.

Když tento kód spustí na zařízení se systémem Android 5.0, generuje oznámení, že vypadá jako v následujícím příkladu:

![Výsledek oznámení ukázkový kód](local-notifications-images/09-hello-world.png)

Na levé straně oznámení se zobrazí na ikonu v oznamovací &ndash; tuto bitovou kopii systému v kroužku &ldquo;i&rdquo; má alfa kanálu, takže Android můžete kreslení šedé cyklické pozadí za ní. Můžete také zadat ikonu bez kanálu alfa. Zobrazit jako ikonu photographic image, najdete v tématu [velké ikony formátu](#large-icon-format) dál v tomto tématu.

Časové razítko je nastavena automaticky, ale toto nastavení můžete přepsat pomocí volání [SetWhen](https://developer.xamarin.com/api/member/Android.App.Notification+Builder.SetWhen/) metoda Tvůrce oznámení. Například následující příklad kódu nastaví časové razítko na aktuální čas:

```csharp
builder.SetWhen (Java.Lang.JavaSystem.CurrentTimeMillis());
```
<a name="sound-and-vibr" />

### <a name="enabling-sound-and-vibration"></a>Povolení zvuk a vibrace

Pokud chcete oznámení také přehrát zvuk, můžete volat Tvůrce oznámení [SetDefaults](https://developer.xamarin.com/api/member/Android.App.Notification+Builder.SetDefaults/) metoda a předejte jí `NotificationDefaults.Sound` příznak:

```csharp
// Instantiate the notification builder and enable sound:
Notification.Builder builder = new Notification.Builder (this)
    .SetContentTitle ("Sample Notification")
    .SetContentText ("Hello World! This is my first notification!")
    .SetDefaults (NotificationDefaults.Sound)
    .SetSmallIcon (Resource.Drawable.ic_notification);
```

Volání `SetDefaults` způsobí, že zařízení přehrát zvuk při publikování oznámení. Pokud chcete zařízení zavibrovat než zvukový signál, můžete předat `NotificationDefaults.Vibrate` k `SetDefaults.` Pokud chcete zařízení pro přehrávání zvuku a zavibrovat zařízení, můžete předat oba příznaky tak, aby `SetDefaults`:

```csharp
builder.SetDefaults (NotificationDefaults.Sound | NotificationDefaults.Vibrate);
```

Pokud povolíte zvuk bez zadání přehrávání zvuku, Android používá výchozí zvuk oznámení systému. Můžete však změnit zvuk, který bude přehraje voláním Tvůrce oznámení [SetSound](https://developer.xamarin.com/api/member/Android.App.Notification+Builder.SetSound/p/Android.Net.Uri/) metoda. Například na upozornění při přehrávání zvuku s oznámení (namísto výchozí oznámení zvuk), můžete získat identifikátor URI pro na upozornění zvuku z [RingtoneManager](https://developer.xamarin.com/api/type/Android.Media.RingtoneManager/) a předejte ji do `SetSound`:

```csharp
builder.SetSound (RingtoneManager.GetDefaultUri(RingtoneType.Alarm));
```

Alternativně můžete použít vyzváněcího výchozí systému zvukové pro oznámení:

```csharp
builder.SetSound (RingtoneManager.GetDefaultUri(RingtoneType.Ringtone));
```

Po vytvoření objektu oznámení, je možné nastavit vlastnosti oznámení na objekt oznámení (nikoli je nakonfigurovat pomocí předem `Notification.Builder` metody). Například místo volání `SetDefaults` způsob povolení vibrace na oznámení, můžete přímo upravit bitový příznak oznámení na [výchozí](https://developer.xamarin.com/api/property/Android.App.Notification.Defaults/) vlastnost:

```csharp
// Build the notification:
Notification notification = builder.Build();

// Turn on vibrate:
notification.Defaults |= NotificationDefaults.Vibrate;
```

Tento příklad způsobí, že zařízení zavibrovat při publikování oznámení.

<a name="updating-a-notification" />

### <a name="updating-a-notification"></a>Aktualizace oznámení

Pokud chcete aktualizovat obsah oznámení po publikování, můžete opakovaně použít existující `Notification.Builder` objekt, který chcete vytvořit nový objekt oznámení a publikování toto oznámení se identifikátorem poslední oznámení. Příklad:

```csharp
// Update the existing notification builder content:
builder.SetContentTitle ("Updated Notification");
builder.SetContentText ("Changed to this message.");

// Build a notification object with updated content:
notification = builder.Build();

// Publish the new notification with the existing ID:
notificationManager.Notify (notificationId, notification);
```

V tomto příkladu existující `Notification.Builder` objekt se používá k vytvoření nového objektu oznámení s jiný název a zprávy.
Nový objekt oznámení je publikována pomocí identifikátoru předchozí oznámení, a tím se aktualizuje obsah na dříve publikované oznámení:

![Aktualizované oznámení](local-notifications-images/12-updated-notification.png)

Tělo předchozí oznámení se znovu použije &ndash; pouze nadpis a text oznámení změní, když zobrazí oznámení na panel oznámení. Text nadpisu změní z hodnoty "Ukázka oznámení" na "Aktualizovat oznámení" a text zprávy se mění z "Hello, World! Toto je Můj první oznámení!" k "Změněno na tuto zprávu."

Dokud jednu ze tří akcí se stane, zůstává viditelná oznámení:

-   Uživatel zavře oznámení (nebo klepne na *Vymazat vše*).

-   Aplikace zavolá `NotificationManager.Cancel`a předejte oznámení jedinečný Identifikátor, který byl přiřazen při publikování oznámení.

-   Volání aplikace `NotificationManager.CancelAll`.

Další informace o aktualizaci Android oznámení najdete v tématu [upravit oznámení](http://developer.android.com/training/notify-user/managing.html#Updating).

<a name="starting-an-activity" />

### <a name="starting-an-activity-from-a-notification"></a>Aktivita od oznámení

V systému Android, je běžné oznámení přidružen *akce* &ndash; aktivitu, která se spustí, když uživatel klepnutím na oznámení. Tato aktivita se může nacházet v jiné aplikaci nebo dokonce v jiné úloze. Chcete-li přidat akci oznámení, musíte vytvořit [PendingIntent](https://developer.xamarin.com/api/type/Android.App.PendingIntent/) objektu a přidružit `PendingIntent` oznámení. A `PendingIntent` , je zvláštním typem záměru, která umožňuje příjemci aplikace spuštěna předdefinovaná úsek kódu s oprávněními odesílající aplikací. Když uživatel klepnutím na oznámení, Android spuštění aktivity určeného `PendingIntent`.

Následující fragment kódu ukazuje, jak vytvořit oznámení s `PendingIntent` aktivity původní aplikace, který se spustí `MainActivity`:

```csharp
// Set up an intent so that tapping the notifications returns to this app:
Intent intent = new Intent (this, typeof(MainActivity));

// Create a PendingIntent; we're only using one PendingIntent (ID = 0):
const int pendingIntentId = 0;
PendingIntent pendingIntent =
    PendingIntent.GetActivity (this, pendingIntentId, intent, PendingIntentFlags.OneShot);

// Instantiate the builder and set notification elements, including pending intent:
Notification.Builder builder = new Notification.Builder(this)
    .SetContentIntent (pendingIntent)
    .SetContentTitle ("Sample Notification")
    .SetContentText ("Hello World! This is my first action notification!")
    .SetSmallIcon (Resource.Drawable.ic_notification);

// Build the notification:
Notification notification = builder.Build();

// Get the notification manager:
NotificationManager notificationManager =
    GetSystemService (Context.NotificationService) as NotificationManager;

// Publish the notification:
const int notificationId = 0;
notificationManager.Notify (notificationId, notification);
```

Tento kód je velmi podobný kód oznámení v předchozí části, vyjma toho, že `PendingIntent` se přidá k objektu oznámení. V tomto příkladu `PendingIntent` souvisí s aktivitou původní aplikace předtím, než je předán do Tvůrce oznámení [SetContentIntent](https://developer.xamarin.com/api/member/Android.App.Notification+Builder.SetContentIntent/) metoda. `PendingIntentFlags.OneShot` Příznak předána `PendingIntent.GetActivity` metoda tak, aby `PendingIntent` použit pouze jednou. Když tento kód běží, zobrazí se následující upozornění:

![První akcí oznámení](local-notifications-images/10-first-action-notification.png)

Klepnutím toto oznámení trvá uživatele zpět na původní aktivita.

V produkční aplikace, musí vaše aplikace zpracovat *back zásobníku* uživatel stiskne **zpět** tlačítko v rámci aktivity oznámení (Pokud nejste obeznámeni s Androidem úkoly a zpět zásobníku, přečtěte si téma [ Úlohy a zpět zásobníku](http://developer.android.com/guide/components/tasks-and-back-stack.html)).
Ve většině případů navigace zpátky aktivita oznámení by měl vrátit uživatele z aplikace a zpět na domovskou obrazovku. Ke správě back zásobníku, vaše aplikace používá [TaskStackBuilder](https://developer.xamarin.com/api/type/Android.App.TaskStackBuilder/) třídy za účelem vytvoření `PendingIntent` s back zásobníku.

Skutečné je potřeba zamyslet se, že původní aktivita může potřebovat k odesílání dat do aktivity oznámení. Například oznámení může znamenat, že byl doručen textovou zprávu, a aktivita oznámení (zprávu zobrazení obrazovky), vyžaduje ID zprávy k zobrazení zprávy pro uživatele. Aktivita, která vytvoří `PendingIntent` můžete použít [Intent.PutExtra](https://developer.xamarin.com/api/member/Android.Content.Intent.PutExtra/p/System.String/System.String/) metodu pro přidání do záměr data (například řetězec), tak, aby se tato data předaná aktivitě oznámení.

Následující příklad kódu ukazuje, jak používat `TaskStackBuilder` ke správě back zásobníku a obsahuje ukázku postup odesílání řetězec jedné zprávy pro aktivitu oznámení názvem `SecondActivity`:

```csharp
// Setup an intent for SecondActivity:
Intent secondIntent = new Intent (this, typeof(SecondActivity));

// Pass some information to SecondActivity:
secondIntent.PutExtra ("message", "Greetings from MainActivity!");

// Create a task stack builder to manage the back stack:
TaskStackBuilder stackBuilder = TaskStackBuilder.Create(this);

// Add all parents of SecondActivity to the stack:
stackBuilder.AddParentStack (Java.Lang.Class.FromType (typeof (SecondActivity)));

// Push the intent that starts SecondActivity onto the stack:
stackBuilder.AddNextIntent (secondIntent);

// Obtain the PendingIntent for launching the task constructed by
// stackbuilder. The pending intent can be used only once (one shot):
const int pendingIntentId = 0;
PendingIntent pendingIntent =
    stackBuilder.GetPendingIntent (pendingIntentId, PendingIntentFlags.OneShot);

// Instantiate the builder and set notification elements, including
// the pending intent:
Notification.Builder builder = new Notification.Builder (this)
    .SetContentIntent (pendingIntent)
    .SetContentTitle ("Sample Notification")
    .SetContentText ("Hello World! This is my second action notification!")
    .SetSmallIcon (Resource.Drawable.ic_notification);

// Build the notification:
Notification notification = builder.Build();

// Get the notification manager:
NotificationManager notificationManager =
    GetSystemService (Context.NotificationService) as NotificationManager;

// Publish the notification:
const int notificationId = 0;
notificationManager.Notify (notificationId, notification);
```

V tomto příkladu kódu aplikace se skládá z dvě aktivity: `MainActivity` (která obsahuje výše uvedený kód oznámení), a `SecondActivity`, na obrazovce, uživateli se zobrazí po klepnou na oznámení. Když se tento kód spustí, se zobrazí jednoduché oznámení (podobně jako v předchozím příkladu). Klepnutím na na oznámení trvá uživateli `SecondActivity` obrazovky:

![Snímek obrazovky druhý aktivity](local-notifications-images/11-second-activity.png)

Řetězce zprávy (předaný do je záměrem `PutExtra` metoda) se načítá v `SecondActivity` přes tento řádek kódu:

```csharp
// Get the message from the intent:
string message = Intent.Extras.GetString ("message", "");
```

Tato načtené zpráva "Pozdrav z MainActivity!," se zobrazí v `SecondActivity` obrazovky, jak je znázorněno na výše uvedené snímku obrazovky. Když uživatel stiskne **zpět** tlačítko při v `SecondActivity`, navigace vede z aplikace a zpět na obrazovku předcházející spuštění aplikace.

Další informace o vytváření čekající záměry najdete v tématu [PendingIntent](https://developer.xamarin.com/api/type/Android.App.PendingIntent/).


<a name="notif-chan" />

## <a name="notification-channels"></a>Kanály oznámení

Od verze Android 8.0 (Oreo), můžete použít *kanály oznámení* funkce vytvořte uživatele přizpůsobitelné kanál pro každý typ oznámení, který chcete zobrazit. Kanály oznámení umožňují vám oznámení skupiny tak, aby všechna oznámení odesílat kanál vykazuje stejné chování. Například můžete mít kanál oznámení, který je určený pro oznámení, které vyžadují okamžitou pozornost a samostatné "tišším" kanál, který se používá pro informační zprávy.

**YouTube** aplikaci, která je nainstalovaná se systémem Android Oreo uvádí dvě kategorie oznámení: **stáhnout oznámení** a **Obecné oznámení**:

[![Oznámení obrazovky pro YouTube v Android Oreo](local-notifications-images/27-youtube-sml.png)](local-notifications-images/27-youtube.png)

Každý z těchto kategorií odpovídá kanál oznámení. Implementuje aplikace YouTube **stáhnout oznámení** kanál a **Obecné oznámení** kanál. Uživatel může klepnout **stáhnout oznámení**, který zobrazuje na obrazovku nastavení pro aplikace kanál oznámení:

[![Stáhnout obrazovky oznámení pro aplikace YouTube](local-notifications-images/28-yt-download-sml.png)](local-notifications-images/28-yt-download.png)

Na této obrazovce můžete upravit uživatele chování **Stáhnout** oznámení kanálu pomocí těchto kroků:

-   Nastavte význam úroveň na **naléhavé**, **vysokou**, **střední**, nebo **nízká**, které konfiguruje úroveň zvukové a vizuální přerušení.

-   Oznámení tečky zapněte nebo vypnout.

-   Blikající světlým zapněte nebo vypnout.

-   Zobrazit nebo skrýt oznámení na zamykací obrazovce.

-   Přepsání **nebudou narušovat** nastavení.

**Obecné oznámení** kanál obsahuje podobné nastavení:

[![Obrazovka Obecné oznámení pro aplikace YouTube](local-notifications-images/29-yt-general-sml.png)](local-notifications-images/29-yt-general.png)

Všimněte si, že nemáte absolutní ovládat, jak vaše kanály oznámení komunikovat s uživatelem &ndash; uživatele můžete upravit nastavení pro všechny kanál oznámení na zařízení, jak je vidět na snímcích obrazovky výše. Můžete ale nakonfigurovat výchozí hodnoty (jak budou popsány níže). Protože tyto příklady ilustrují, nové funkce kanály oznámení umožňuje vám poskytuje jemně odstupňovanou kontrolu nad různé druhy oznámení uživatelům.

Můžete přidat podporu pro kanály oznámení do aplikace? Pokud jste cílení na Android 8.0, vaše aplikace *musí* implementovat kanály oznámení.
Aplikace cílené na Oreo, které se pokouší odeslat místního oznámení pro uživatele bez použití kanál oznámení nebude možné zobrazit oznámení na Oreo zařízení. Pokud nemáte cíle Android 8.0, aplikace bude i nadále spustit na Android 8.0, ale s stejné chování oznámení jako by vykazovat, při spuštění na Android 7.1 nebo dřívější.

<a name="notif-chan-create" />

### <a name="creating-a-notification-channel"></a>Vytvoření kanálu oznámení

Pokud chcete vytvořit kanál oznámení, postupujte takto:

1. Vytvořit [NotificationChannel](https://developer.android.com/reference/android/app/NotificationChannel.html) objekt s následující:

    - Řetězec ID, které jsou jedinečné v rámci vašeho balíčku. V následujícím příkladu, řetězec `com.xamarin.myapp.urgent` se používá.

    - Viditelné pro uživatele název kanálu (méně než 40 znaků). V následujícím příkladu, název **naléhavé** se používá.

    - Význam kanál, který určuje, jak interruptive oznámení jsou odeslány na `NotificationChannel`. Může být význam `Default`, `High`, `Low`, `Max`, `Min`, `None`, nebo `Unspecified`.

    Předejte tyto hodnoty [konstruktor](https://developer.android.com/reference/android/app/NotificationChannel.html#NotificationChannel%28java.lang.String,%20java.lang.CharSequence,%20int%29) (v tomto příkladu `Resource.String.noti_chan_urgent` je nastaven na **naléhavé**):

    ```csharp
    public const string URGENT_CHANNEL = "com.xamarin.myapp.urgent";
    . . .
    string chanName = GetString (Resource.String.noti_chan_urgent);
    var importance = NotificationImportance.High;
    NotificationChannel chan =
       new NotificationChannel (URGENT_CHANNEL, chanName, importance);
    ```

2.  Konfigurace `NotificationChannel` objekt počátečního nastavení.
    Například následující kód konfiguruje `NotificationChannel` objektu tak, aby se oznámení odeslána do tohoto kanálu zavibrovat zařízení a objeví se na zamykací obrazovce ve výchozím nastavení:

    ```csharp
    chan.EnableVibration (true);
    chan.LockscreenVisibility = NotificationVisibility.Public;
    ```

3.  Odešlete oznámení objekt kanálu pro správce oznámení:

    ```csharp
    NotificationManager notificationManager =
        (NotificationManager) GetSystemService (NotificationService);
    notificationManager.CreateNotificationChannel (chan);
    ```

<a name="notif-chan-post" />

### <a name="posting-to-a-notifications-channel"></a>Publikování oznámení kanálu

Odeslat oznámení pro kanál oznámení, postupujte takto:

1.  Konfigurace oznámení pomocí `Notification.Builder`a předejte ID kanál, který má `SetChannelId` metoda. Příklad:

    ```csharp
    Notification.Builder builder = new Notification.Builder (this)
        .SetContentTitle ("Attention!")
        .SetContentText ("This is an urgent notification message!")
        .SetChannelId (URGENT_CHANNEL);
    ```

2.  Vytvoření a vydání oznámení pomocí Správce oznámení [oznámení](https://developer.xamarin.com/api/member/Android.App.NotificationManager.Notify/p/System.Int32/Android.App.Notification/) metoda:

    ```csharp
    const int notificationId = 0;
    notificationManager.Notify (notificationId, builder.Build());
    ```

Výše uvedené kroky k vytvoření jiné kanál oznámení pro informační zprávy, můžete opakovat. Tento druhý kanál může ve výchozím nastavení, zakázat vibrace, nastavte výchozí viditelnost zámek obrazovky na `Private`a nastavte důležitosti oznámení na `Default`.

Kompletní příklad Android Oreo oznámení kanálů v praxi, najdete v článku [NotificationChannels](https://developer.xamarin.com/samples/monodroid/android-o/NotificationChannels) ukázku; této ukázkové aplikace spravuje dva kanály a nastaví možnosti Další oznámení.



<a name="beyond-the-basic-notification" />

## <a name="beyond-the-basic-notification"></a>Kromě základních oznámení

Oznámení se jako výchozí formát no-frills základní rozložení v Android, ale můžete vylepšit tento základní formát tím, že další `Notification.Builder` volání metody. V této části se naučíte přidání fotografií velké ikony na oznámení a uvidíte příklady, jak vytvořit oznámení rozšířené rozložení.

<a name="large-icon-format" />

### <a name="large-icon-format"></a>Formát velkých ikon.

Ikona původní aplikace Android oznámení obvykle zobrazení (na levé straně oznámení). Ale zobrazit oznámení bitovou kopii nebo fotografie ( *velkých ikon*) místo standardní malé ikony. Zasílání zpráv aplikace může například zobrazit fotografie odesílatele, nikoli na ikonu aplikace.

Tady je příklad základní oznámení Android 5.0 &ndash; se zobrazí pouze ikonu malé aplikace:

![Příklad normální oznámení](local-notifications-images/13-sample-notification.png)

A zde je snímek obrazovky oznámení po úpravě ho k zobrazení velkých ikon &ndash; používá ikonu vytvořené z bitové kopie Xamarin opic kódu:

![Příklad oznámení velkých ikon.](local-notifications-images/14-large-icon-sample.png)

Všimněte si, že když se ve formátu velkých ikon oznámení, ikonu malé aplikace se zobrazí jako oznámení v pravém dolním rohu velkých ikon.

Chcete-li použít bitovou kopii jako velké ikony v oznámení, zavolejte Tvůrce oznámení [SetLargeIcon](https://developer.xamarin.com/api/member/Android.App.Notification+Builder.SetLargeIcon/) metoda a předejte jí rastrový obrázek bitové kopie. Na rozdíl od `SetSmallIcon`, `SetLargeIcon` přijímá pouze rastrový obrázek. Chcete-li převést soubor obrázku na rastrový obrázek, použijte [BitmapFactory](https://developer.xamarin.com/api/type/Android.Graphics.BitmapFactory/) třídy. Příklad:

```csharp
builder.SetLargeIcon (BitmapFactory.DecodeResource (Resources, Resource.Drawable.monkey_icon));
```

Tento příklad kódu otevře soubor bitové kopie na **Resources/drawable/monkey_icon.png**, převede ji na rastrový obrázek a předá výsledné rastrového obrázku na `Notification.Builder`. Obvykle je větší než malé ikony rozlišení obrázku zdroj &ndash; ale mnohem větší. Obrázek, který je příliš velký může způsobit zbytečné změny velikosti operace, které může zpoždění zveřejňování oznámení.
Další informace o velikosti ikonu oznámení na Android, najdete v části [ikony oznámení](http://developer.android.com/design/style/iconography.html#notification).

<a name="big-text-style" />

### <a name="big-text-style"></a>Styl Big textu

*Dlouhý Text* je styl šablonu rozšířené rozložení, který používáte pro zobrazení dlouhých zpráv v oznámení. Podobně jako všechny rozšířené rozložení oznámení je zobrazena oznámení dlouhý Text ve formátu compact prezentace:

![Příklad dlouhý Text oznámení](local-notifications-images/15-big-text-notification.png)

V tomto formátu se zobrazí pouze výňatek zprávy ukončila příkazem dvě tečky. Když uživatel nastavuje tažením směrem dolů na oznámení, rozšiřuje na nich celý oznámení:

![Rozšířené dlouhý Text oznámení](local-notifications-images/16-big-text-expanded.png)

Tento formát rozšířené rozložení také obsahuje souhrn text v dolní části oznámení. Maximální výška *dlouhý Text* oznámení je 256 distribučního bodu.

K vytvoření *dlouhý Text* doložit oznámení, `Notification.Builder` objektu jako předtím a pak vytvořit instanci a přidat [BigTextStyle](https://developer.xamarin.com/api/type/Android.App.Notification+BigTextStyle/) do objektu `Notification.Builder` objektu. Příklad:

```csharp
// Instantiate the Big Text style:
Notification.BigTextStyle textStyle = new Notification.BigTextStyle();

// Fill it with text:
string longTextMessage = "I went up one pair of stairs.";
longTextMessage += " / Just like me. ";
//...
textStyle.BigText (longTextMessage);

// Set the summary text:
textStyle.SetSummaryText ("The summary text goes here.");

// Plug this style into the builder:
builder.SetStyle (textStyle);

// Create the notification and publish it ...
```

V tomto příkladu text zprávy a text shrnutí jsou uloženy v `BigTextStyle` objektu (`textStyle`) předtím, než je předán do `Notification.Builder.`

<a name="image-style" />

### <a name="image-style"></a>Styl bitové kopie

*Bitové kopie* styl (také nazývané *velký obrázek* styl) je ve formátu rozšířené oznámení, můžete použít k zobrazení obrázku v textu oznámení. Například můžete použít snímek obrazovky aplikace nebo aplikace fotografií *Image* oznámení styl, který uživateli poskytnout oznámení o poslední bitové kopie, kterou jste zachytili. Všimněte si, že maximální výšku *Image* oznámení je 256 distribučního bodu &ndash; Android se změní velikost obrázku tak, aby odpovídal toto omezení maximální výška, v rámci dostupné paměti.

Podobně jako všechny rozšířit rozložení oznámení *Image* první se zobrazují oznámení ve sbaleném formátu, který zobrazuje výňatek doprovodné text zprávy:

![Oznámení Compact obrázek ukazuje žádný obrázek](local-notifications-images/17-image-compact.png)

Když uživatel nastavuje tažením směrem dolů *Image* oznámení, rozšíří odhalit bitovou kopii. Zde je například rozšířená verzi na předchozí oznámení:

![Rozšířené image oznámení zjistí informace o bitové kopie](local-notifications-images/18-image-expanded.png)

Všimněte si, že když oznámení se zobrazí ve sbaleném formátu, zobrazí oznámení text (text, který je předán do Tvůrce oznámení `SetContentText` metoda, jak je uvedeno výše). Ale když oznámení je rozšířit tak, aby odhalit bitovou kopii, zobrazí text shrnutí výše bitovou kopii.

K vytvoření *bitové kopie* doložit oznámení, `Notification.Builder` objektu jako předtím a pak vytvořit a vložit [BigPictureStyle](https://developer.xamarin.com/api/type/Android.App.Notification+BigPictureStyle/) objektu do `Notification.Builder` objektu. Příklad:

```csharp
// Instantiate the Image (Big Picture) style:
Notification.BigPictureStyle picStyle = new Notification.BigPictureStyle();

// Convert the image to a bitmap before passing it into the style:
picStyle.BigPicture (BitmapFactory.DecodeResource (Resources, Resource.Drawable.x_bldg));

// Set the summary text that will appear with the image:
picStyle.SetSummaryText ("The summary text goes here.");

// Plug this style into the builder:
builder.SetStyle (picStyle);

// Create the notification and publish it ...
```

Podobně jako `SetLargeIcon` metodu `Notification.Builder`, [BigPicture](https://developer.xamarin.com/api/member/Android.App.Notification+BigPictureStyle.BigPicture/) metodu `BigPictureStyle` vyžaduje rastrový obrázek, který chcete zobrazit v textu oznámení. V tomto příkladu [DecodeResource](https://developer.xamarin.com/api/member/Android.Graphics.BitmapFactory.DecodeResource/(Android.Content.Res.Resources%2cSystem.Int32)) metodu `BitmapFactory` čtení souboru bitové kopie nachází v **Resources/drawable/x_bldg.png** a převede jej na rastrový obrázek.

Můžete také zobrazit bitové kopie, které nejsou zabalené jako prostředek. Například následující ukázkový kód načte bitovou kopii z místní SD karty a zobrazí se v *Image* oznámení:

```csharp
// Using the Image (Big Picture) style:
Notification.BigPictureStyle picStyle = new Notification.BigPictureStyle();

// Read an image from the SD card, subsample to half size:
BitmapFactory.Options options = new BitmapFactory.Options();
options.InSampleSize = 2;
string imagePath = "/sdcard/Pictures/my-tshirt.jpg";
picStyle.BigPicture (BitmapFactory.DecodeFile (imagePath, options));

// Set the summary text that will appear with the image:
picStyle.SetSummaryText ("Check out my new T-Shirt!");

// Plug this style into the builder:
builder.SetStyle (picStyle);

// Create notification and publish it ...
```

V tomto příkladu souboru bitové kopie v umístění **/sdcard/Pictures/my-tshirt.jpg** je načtena, změně velikosti polovinu jeho původní velikost a potom převedou do rastrový obrázek pro použití v oznámení:

![Obrázek příkladu tričko v oznámení](local-notifications-images/19-tshirt-notification.png)

Pokud si nejste jisti předem velikost souboru bitové kopie, je vhodné zabalení volání [BitmapFactory.DecodeFile](https://developer.xamarin.com/api/member/Android.Graphics.BitmapFactory.DecodeFile/p/System.String/Android.Graphics.BitmapFactory+Options/) v obslužné rutiny výjimek &ndash; `OutOfMemoryError` může být vyvolána výjimka, pokud je obrázek příliš velký pro Android ke změně velikosti.

Další informace o načítání a dekódování velké rastrové obrázky, naleznete v části [zatížení velké bitmap efektivně](https://developer.xamarin.com/recipes/android/resources/general/load_large_bitmaps_efficiently).

<a name="inbox-style" />

### <a name="inbox-style"></a>Styl doručené pošty

*Doručené pošty* formát je šablonu rozšířené rozložení určený pro zobrazení samostatné řádky textu (například e-mailovou schránku souhrnné) v textu oznámení. *Doručené pošty* formát oznámení se napřed zobrazí ve sbaleném formátu:

![Příklad compact doručených zpráv oznámení](local-notifications-images/20-inbox-compact.png)

Když uživatel nastavuje tažením směrem dolů na oznámení, rozšiřuje na nich e-mailu souhrnné informace, jak je vidět na tomto snímku obrazovky:

![Příklad doručených zpráv oznámení rozšířit](local-notifications-images/21-inbox-expanded.png)

Chcete-li vytvořit *doručené pošty* doložit oznámení, `Notification.Builder` objektu jako předtím a přidejte [InboxStyle](https://developer.xamarin.com/api/type/Android.App.Notification+InboxStyle/) do objektu `Notification.Builder`. Příklad:

```csharp
// Instantiate the Inbox style:
Notification.InboxStyle inboxStyle = new Notification.InboxStyle();

// Set the title and text of the notification:
builder.SetContentTitle ("5 new messages");
builder.SetContentText ("chimchim@xamarin.com");

// Generate a message summary for the body of the notification:
inboxStyle.AddLine ("Cheeta: Bananas on sale");
inboxStyle.AddLine ("George: Curious about your blog post");
inboxStyle.AddLine ("Nikko: Need a ride to Evolve?");
inboxStyle.SetSummaryText ("+2 more");

// Plug this style into the builder:
builder.SetStyle (inboxStyle);
```

Chcete-li přidat nové řádky textu do textu oznámení, zavolejte [Addline](https://developer.xamarin.com/api/member/Android.App.Notification+InboxStyle.AddLine/p/System.String/) metodu `InboxStyle` objektu (maximální výšku *doručené pošty* oznámení je 256 distribučního bodu). Všimněte si, že na rozdíl od *dlouhý Text* styl, *doručené pošty* styl podporuje jednotlivé řádky textu v textu oznámení.

Můžete také *doručené pošty* styl pro všechna oznámení vyžadující zobrazíte jednotlivé řádky textu ve formátu rozšířené. Například *doručené pošty* styl oznámení je možné kombinovat více čekající oznámení do souhrnu oznámení &ndash; můžete aktualizovat jeden *doručené pošty* styl oznámení New. řádcích obsahu oznámení (najdete v části [aktualizace oznámení](#updating-a-notification) výše), spíš než generovat může nepřetržitý proud nové, většinou podobné oznámení. Další informace o tento přístup, najdete v části [shrnout oznámení](http://developer.android.com/design/patterns/notifications.html#summarize_your_notifications).

<a name="configuring-metadata" />

## <a name="configuring-metadata"></a>Konfigurace metadat

`Notification.Builder` obsahuje metody, které můžete volat nastavit metadata o oznámení, jako je například priorita, viditelnost a kategorie. Tyto informace využívá Android &mdash; společně s uživatelská nastavení &mdash; k určení, jak a kdy mají zobrazovat oznámení.

<a name="priority-settings" />

### <a name="priority-settings"></a>Nastavení priority

Nastavení priority oznámení určuje dvě výstupy v případě, že je publikovaná oznámení:

-   Kde se ve vztahu k jiné oznámení zobrazí oznámení.
    Například s vysokou prioritou oznámení uvádíme výše nižší prioritu oznámení v panel oznámení bez ohledu na to při každém oznámení byla publikována.

-   Zda se oznámení zobrazuje ve formátu Heads-up oznámení (Android 5.0 a novější). Pouze *vysokou* a *maximální* jako z pohotového oznámení se zobrazují oznámení s prioritou.

Xamarin.Android definuje následující výčty pro nastavení priority oznámení:

-   `NotificationPriority.Max` &ndash; Upozorní uživatele podmínku naléhavé nebo kritický stav (například příchozího hovoru, zapněte zapnout pokynů nebo nouzová výstraha). V systému Android 5.0 a novější zařízení se zobrazují oznámení s nejvyšší prioritou ve formátu z pohotového.

-   `NotificationPriority.High` &ndash; Informuje uživatele o důležitých událostech (například důležité e-mailů nebo doručení zpráv v reálném čase chat). V systému Android 5.0 a novější zařízení se zobrazují oznámení s vysokou prioritou ve formátu z pohotového.

-   `NotificationPriority.Default` &ndash; Upozorní uživatele podmínek, které mají střední úroveň význam.

-   `NotificationPriority.Low` &ndash; Naléhavé informace, že uživatel musí být informováni o (například připomenutí aktualizace softwaru nebo aktualizace sociálních sítí).

-   `NotificationPriority.Min` &ndash; Základní informace, že uživatel zaznamená pouze tehdy, když zobrazování oznámení (například informace o umístění nebo počasí).

Chcete-li nastavit prioritu oznámení, zavolejte [SetPriority](https://developer.xamarin.com/api/member/Android.App.Notification+Builder.SetPriority/) metodu `Notification.Builder` objekt, předávání v úroveň priority. Příklad:

```csharp
builder.SetPriority (NotificationPriority.High);
```

V následujícím příkladu, oznámení s vysokou prioritou, "Důležité zprávu!" se zobrazí v horní části panel oznámení:

![Příklad oznámení s vysokou prioritou](local-notifications-images/22-hi-priority-drawer.png)

Vzhledem k tomu, že toto je upozornění s vysokou prioritou, se také zobrazí jako oznámení z pohotového výše obrazovce aktuální aktivity uživatele v systému Android 5.0:

![Příklad z pohotového oznámení](local-notifications-images/23-heads-up-example.png)

V následujícím příkladu se zobrazí oznámení "Představit dne" nízkou prioritu v rámci úrovně oznámení o stavu baterie s vyšší prioritou:

![Příklad oznámení s nízkou prioritou](local-notifications-images/24-lo-priority-drawer.png)

Vzhledem k tomu, že se oznámení o "Myšlenku dne" oznámení nízkou prioritu, Android nebudou zobrazeny na Heads-up formátu.

<a name="visibility-settings" />

### <a name="visibility-settings"></a>Nastavení viditelnosti

Od verze Android 5.0 *viditelnost* nastavení je možné určit, kolik oznámení obsahu se zobrazí na zabezpečené zamykací obrazovky.
Xamarin.Android definuje následující výčty pro nastavení viditelnost oznámení:

-   `NotificationVisibility.Public` &ndash; Celý obsah oznámení se zobrazí na zabezpečené zamykací obrazovky.

-   `NotificationVisibility.Private` &ndash; Pouze nezbytné informace se zobrazí na zabezpečené zamykací obrazovky (například ikonu oznámení a název aplikace, které co), ale zbytek na oznámení podrobnosti jsou skryté. Všechna oznámení ve výchozím nastavení týkají `NotificationVisibility.Private`.

-   `NotificationVisibility.Secret` &ndash; Nezobrazí se na zabezpečené zamykací obrazovky ani ikonu oznámení. Obsah oznámení je k dispozici, až poté, co uživatel odemkne zařízení.

Chcete-li nastavit viditelnost oznámení, aplikace volání `SetVisibility` metodu `Notification.Builder` objekt, předávání v nastavení viditelnosti. Například volání `SetVisibility` díky oznámení `Private`:

```csharp
builder.SetVisibility (NotificationVisibility.Private);
```

Když `Private` odeslání oznámení, název a ikona aplikace se zobrazí na zabezpečené zamykací obrazovky. Místo oznámení uživateli se zobrazí "Odemčení zařízení zobrazíte toto oznámení":

![Odemknutí zařízení zprávu oznámení](local-notifications-images/25-lockscreen-private.png)

V tomto příkladu **NotificationsLab** je název původní aplikace. Tato verze zredigované oznámení se zobrazí pouze v případě, zamykací obrazovky je zabezpečený (tj, zabezpečené pomocí kódu PIN, vzor nebo heslo) &ndash; Pokud zamykací obrazovky nejsou zabezpečené, celý obsah oznámení je k dispozici na zamykací obrazovky.

<a name="category-settings" />

### <a name="category-settings"></a>Kategorie nastavení

Od verze Android 5.0, jsou k dispozici pro řazení a filtrování oznámení předdefinovaným kategoriím. Xamarin.Android poskytuje následující výčty pro tyto kategorie:

-   `Notification.CategoryCall` &ndash; Příchozí telefonní hovor.

-   `Notification.CategoryMessage` &ndash; Příchozí textová zpráva.

-   `Notification.CategoryAlarm` &ndash; Alarmů podmínka nebo časovače vypršení platnosti.

-   `Notification.CategoryEmail` &ndash; Příchozí e-mailové zprávy.

-   `Notification.CategoryEvent` &ndash; Události kalendáře.

-   `Notification.CategoryPromo` &ndash; Propagační zpráva nebo oznámení o inzerovaném programu.

-   `Notification.CategoryProgress` &ndash; Průběh operaci na pozadí.

-   `Notification.CategorySocial` &ndash; Aktualizace sociálních sítí.

-   `Notification.CategoryError` &ndash; Chyba zpracování na pozadí operace nebo ověřování.

-   `Notification.CategoryTransport` &ndash; Aktualizace přehrávání média.

-   `Notification.CategorySystem` &ndash; Vyhrazené pro použití systému (stav systému nebo zařízení).

-   `Notification.CategoryService` &ndash; Určuje, zda je spuštěna služba pozadí.

-   `Notification.CategoryRecommendation` &ndash; Doporučení související s aktuálně spuštěné aplikaci.

-   `Notification.CategoryStatus` &ndash; Informace o zařízení.

Když jsou seřazeny oznámení, na oznámení s prioritou přednost před jeho kategorie nastavení. Například s vysokou prioritou oznámení se zobrazí jako z pohotového i v případě, že patří do `Promo` kategorie. Chcete-li nastavit kategorii oznámení, zavolejte `SetCategory` metodu `Notification.Builder` objekt, předávání v nastavení kategorie. Příklad:

```csharp
builder.SetCategory (Notification.CategoryCall);
```

*Nebudou narušovat* funkce (novinka v systému Android 5.0) filtry oznámení na základě kategorií. Například *nebudou narušovat* obrazovky v **nastavení** umožňuje uživatelům vyloučit oznámení pro telefonní hovory a zprávy:

![Nebudou narušovat přepínače obrazovky](local-notifications-images/26-do-not-disturb.png)

Když si uživatel nakonfiguruje *nebudou narušovat* blokování všech přerušení s výjimkou telefonní hovory (jak je znázorněno na výše uvedené snímku obrazovky), umožňuje Android oznámení s nastavením kategorie `Notification.CategoryCall` předkládané při zařízení je v *nebudou narušovat* režimu. Všimněte si, že `Notification.CategoryAlarm` oznámení v nikdy blokují *nebudou narušovat* režimu.


<a name="compatibility" />

## <a name="compatibility"></a>Kompatibilita

Při vytváření aplikace, bude také spouštět v dřívějších verzích systému Android (již v rozhraní API úroveň 4), můžete použít [NotificationCompat.Builder](https://developer.android.com/reference/android/support/v4/app/NotificationCompat.Builder.html) místo `Notification.Builder`. Při vytváření oznámení s `NotificationCompat.Builder`, Android zajistí, že obsah základní oznámení se zobrazí správně na starší zařízení. Ale vzhledem k tomu, že některé funkce Rozšířené oznámení nejsou k dispozici ve starších verzích systému Android, váš kód musí explicitně zpracovat problémy s kompatibilitou styly rozšířené oznámení, kategorie a viditelnost úrovně popsané níže.

Použít `NotificationCompat.Builder` ve vaší aplikaci, musíte zahrnout [podporu knihovna pro Android v4](https://www.nuget.org/packages/Xamarin.Android.Support.v4/) NuGet ve vašem projektu.

Následující příklad kódu ukazuje, jak vytvořit základní oznámení pomocí `NotificationCompat.Builder`:

```csharp
// Instantiate the builder and set notification elements:
NotificationCompat.Builder builder = new NotificationCompat.Builder (this)
    .SetContentTitle ("Sample Notification")
    .SetContentText ("Hello World! This is my first notification!")
    .SetSmallIcon (Resource.Drawable.ic_notification);

// Build the notification:
Notification notification = builder.Build();
```

Jak ukazuje tento příklad, volání metod pro možnosti základní oznámení jsou stejné jako u `Notification.Builder`. Váš kód má však pro zpracování klesající kompatibility problémy složitější oznámení (popsané v další části).

[LocalNotifications](https://developer.xamarin.com/samples/monodroid/LocalNotifications) příklad ukazuje způsob použití `NotificationCompat.Builder` spustíte druhá aktivita z oznámení. Tento ukázkový kód je podrobně popsaný [pomocí místního oznámení v Xamarin.Android](~/android/app-fundamentals/notifications/local-notifications-walkthrough.md) návod.

<a name="notification-styles" />

### <a name="notification-styles"></a>Styly oznámení

Chcete-li vytvořit *dlouhý Text*, *Image*, nebo *doručené pošty* styl oznámení s `NotificationCompat.Builder`, aplikace musí používat verze kompatibility tyto styly. Chcete-li například použít *dlouhý Text* styl, vytváření instancí `NotificationCompat.BigTextstyle`:

```csharp
NotificationCompat.BigTextStyle textStyle = new NotificationCompat.BigTextStyle();

// Plug this style into the builder:
builder.SetStyle (textStyle);
```

Podobně můžete použít aplikaci `NotificationCompat.InboxStyle` a `NotificationCompat.BigPictureStyle` pro *doručené pošty* a *Image* styly v uvedeném pořadí.

<a name="priority-and-category" />

### <a name="notification-priority-and-category"></a>Oznámení Priority a kategorie

`NotificationCompat.Builder` podporuje `SetPriority` – metoda (k dispozici od verze Android 4.1). Ale `SetCategory` je metoda *není* nepodporuje `NotificationCompat.Builder` protože kategorie jsou součástí nového systému metadata oznámení byla zavedena v systému Android 5.0.

K podpoře starší verze Android, přičemž `SetCategory` není k dispozici, kód můžete zkontrolovat úroveň rozhraní API za běhu podmíněně volat `SetCategory` při úroveň rozhraní API je rovna nebo větší než Android 5.0 (API úrovně 21):

```csharp
if ((int) Android.OS.Build.Version.SdkInt >= 21) {
    builder.SetCategory (Notification.CategoryEmail);
}
```

V tomto příkladu aplikace na **cílové rozhraní** je nastaven na Android 5.0 a **minimální verze Android** je nastaven na **Android 4.1 (rozhraní API Level 16)**. Protože `SetCategory` je k dispozici na úrovni rozhraní API 21 a novější, zavolá tento příklad kódu `SetCategory` pouze pokud je k dispozici &ndash; nebude volání `SetCategory` při úroveň rozhraní API je menší než
21.

<a name="lockscreen-visibility" />

### <a name="lockscreen-visibility"></a>Viditelnost zamykací obrazovky

Protože Android nepodporuje zamykací obrazovky oznámení před Android 5.0 (API úrovně 21), `NotificationCompat.Builder` nepodporuje `SetVisibility` metoda. Jak jsme vysvětlili výše pro `SetCategory`, kód můžete zkontrolovat úroveň rozhraní API v modulu runtime a volání `SetVisiblity` pouze pokud je k dispozici:

```csharp
if ((int) Android.OS.Build.Version.SdkInt >= 21) {
    builder.SetVisibility (Notification.Public);
}
```

<a name="summary" />

## <a name="summary"></a>Souhrn

Tento článek vysvětluje vytvoření místního oznámení v Android. Ho popsané anatomie oznámení, je vysvětleno použití `Notification.Builder` při vytváření oznámení, jak styl oznámení ve velkých ikon, *dlouhý Text*, *bitové kopie* a *doručené pošty*  formátů, jak nastavit upozornění metadata nastavení, jako je například priorita, viditelnost a kategorie a jak spustit aktivitu z oznámení. Tento článek také popisuje, jak tato nastavení oznámení fungují s novou z pohotového zamykací obrazovky, a *nebudou narušovat* funkce byla zavedená v systému Android 5.0. Nakonec jste zjistili, jak používat `NotificationCompat.Builder` udržovat oznámení kompatibilitu s předchozími verzemi systému Android.

Pokyny pro návrh oznámení pro Android, najdete v části [oznámení](http://developer.android.com/preview/notifications.html).


## <a name="related-links"></a>Související odkazy

- [NotificationsLab (ukázka)](https://developer.xamarin.com/samples/monodroid/android5.0/NotificationsLab/)
- [LocalNotifications (ukázka)](https://developer.xamarin.com/samples/monodroid/LocalNotifications/)
- [Místní oznámení v Android návod](~/android/app-fundamentals/notifications/local-notifications-walkthrough.md)
- [Oznámení](http://developer.android.com/design/patterns/notifications.html)
- [Upozornění uživatele](http://developer.android.com/training/notify-user/index.html)
- [oznámení](https://developer.xamarin.com/api/type/Android.App.Notification/)
- [NotificationManager](https://developer.xamarin.com/api/type/Android.App.NotificationManager/)
- [NotificationCompat.Builder](https://developer.android.com/reference/android/support/v4/app/NotificationCompat.Builder.html)
- [PendingIntent](https://developer.xamarin.com/api/type/Android.App.PendingIntent/)
