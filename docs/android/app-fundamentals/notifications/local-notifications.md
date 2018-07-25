---
title: Místní oznámení
description: Tato část ukazuje, jak implementovat místní oznámení v Xamarin.Android. Vysvětluje různé elementy uživatelského rozhraní s Androidem oznámení a popisuje rozhraní API na péči o vytváření a zobrazování oznámení.
ms.prod: xamarin
ms.assetid: 03E19D14-7C81-4D5C-88FC-C3A3A927DB46
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/16/2018
ms.openlocfilehash: 6c8abbdb18bcaee405f8fe7fe8c22a930435c7e5
ms.sourcegitcommit: b56b3f906d2c05a3f1be219ef41be8b79e519b8e
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/25/2018
ms.locfileid: "39242443"
---
# <a name="local-notifications"></a>Místní oznámení

_Tato část ukazuje, jak implementovat místní oznámení v Xamarin.Android. Vysvětluje různé elementy uživatelského rozhraní s Androidem oznámení a popisuje rozhraní API na péči o vytváření a zobrazování oznámení._

## <a name="local-notifications-overview"></a>Přehled místní oznámení

Toto téma vysvětluje, jak implementovat místní oznámení v aplikaci Xamarin.Android. Tento článek popisuje různé části s Androidem oznámení, najdete v něm jiného oznamovacího styly, které jsou k dispozici pro vývojáře aplikací a představuje některé z rozhraní API, která se používají k vytvoření a publikování oznámení.

Android poskytuje dvě oblasti řízený systém pro zobrazení ikon upozornění a oznámení o uživateli. Při prvním publikování oznámení se jeho ikona je zobrazena *oznamovací oblasti*, jak je znázorněno na následujícím snímku obrazovky:

![Příklad oznamovací oblasti na zařízení](local-notifications-images/01-notification-shade.png)

Pokud chcete získat podrobnosti o oznámení, může uživatel otevřít panel oznámení (který rozbalí jednotlivé ikony upozornění zobrazíte obsah oznámení) a provádět všechny akce přidružené k oznámení. Snímek se následující obrazovka ukazuje *panel oznámení* , který odpovídá zobrazený nad oznamovací oblasti:

[![Příklad panel oznámení zobrazování tři oznámení](local-notifications-images/02-notification-drawer-sml.png)](local-notifications-images/02-notification-drawer.png#lightbox)

Oznámení systému Android pomocí dva druhy rozložení:

-   ***Základní rozložení*** &ndash; formátu compact, pevný prezentace.

-   ***Rozšířené rozložení*** &ndash; prezentace formátu, který můžete rozšířit na větší velikost zobrazíte další informace.

Každý z těchto typů rozložení (a postupy jejich vytvoření) je vysvětlené v následujících částech.


### <a name="base-layout"></a>Základní rozložení

Všechna oznámení Android jsou založené na formátu základní rozložení, který přinejmenším obsahuje následující prvky:

1.  A *ikona*, která představuje původní aplikace nebo typ oznámení, pokud aplikace podporuje různé typy oznámení.

2.  Oznámení *title*, nebo název odesílatele, pokud je osobní zprávu oznámení.

3.  Zprávy oznámení.

4.  A *časové razítko*.

Tyto prvky jsou zobrazeny, jak je znázorněno v následujícím diagramu:

[![Umístění prvků oznámení](local-notifications-images/03-notification-callouts-sml.png)](local-notifications-images/03-notification-callouts.png#lightbox)

Základní rozložení jsou omezená na 64 pixelech nezávislých na hustotě (dp) na výšku. Ve výchozím nastavení vytvoří Android tento styl oznámení na úrovni basic.

Volitelně můžete oznámení můžete zobrazit velké ikony, která představuje aplikace nebo fotografie odesílatele. Při velké ikony se používá v oznámení v Androidu 5.0 nebo novější, se zobrazí malá ikona jako oznámení přes velké ikony:

![Jednoduché oznámení fotografií](local-notifications-images/04-simple-notification-photo.png)

Od verze Android 5.0, může také zobrazovat oznámení na zamykací obrazovce:

[![Příklad oznámení zamykací obrazovky](local-notifications-images/05-lockscreen-notification-sml.png)](local-notifications-images/05-lockscreen-notification.png#lightbox)

Uživatel může dvojitého klepnutí na oznámení zamykací obrazovky k odemknutí zařízení a přejít na aplikaci, vytvoří se oznámení, nebo přejeďte prstem chcete oznámení zavřít. Aplikace můžete nastavit úroveň viditelnosti oznámení řídit, jak je zobrazeno na zamykací obrazovce, a uživatelé můžou vybrat, jestli povolíte citlivý obsah zobrazený v oznámení zamykací obrazovky.

Android 5.0 zavedeny do formátu prezentace oznámení s vysokou prioritou *indikačního*. Oznámení z pohotového Vysuňte dolů z horní části obrazovky na několik sekund a pak retreat zálohování do oznamovací oblasti:

[![Příklad heads-up oznámení](local-notifications-images/06-heads-up-notification-sml.png)](local-notifications-images/06-heads-up-notification.png#lightbox)

Indikační oznámení umožňují systému uživatelského rozhraní do důležité informace před uživatelem bez narušení stavu aktuálně spuštěné aktivity.

Android zahrnuje podporu pro metadata oznámení tak, aby je možné seřadit a monetizace zobrazí oznámení. Metadata oznámení také určuje, jak oznámení se zobrazují na zamykací obrazovce a ve formátu indikačního. Aplikace můžete nastavit následující typy metadat oznámení:

-   **Priorita** &ndash; úroveň priority určuje, jak a kdy se zobrazí oznámení. Například v Androidu 5.0 se zobrazují oznámení s vysokou prioritou jako pohotové oznámení.

-   **Viditelnost** &ndash; Určuje, kolik obsahu oznámení se zobrazí, když na zamykací obrazovce se zobrazí oznámení.

-   **Kategorie** &ndash; informuje zpracování oznámení v různých situacích, například když je zařízení v systému *Nerušit* režimu.

**Poznámka:** **viditelnost** a **kategorie** byly zavedené v Androidu 5.0 a nejsou k dispozici v dřívějších verzích Androidu. Od verze Android 8.0 [kanály oznámení](#notif-chan) se používají k řízení, jak se uživateli zobrazí oznámení.


### <a name="expanded-layouts"></a>Rozšířené rozložení

Od verze Android 4.1, oznámení lze nastavit pomocí stylů rozšířené rozložení, které uživateli umožňují rozšířit výška oznámení zobrazíte další obsah. Například následující příklad ukazuje oznámení o rozšířené rozložení v smluvní režimu:

![Smluvní oznámení](local-notifications-images/07-contracted-notification.png)

Po rozbalení toto oznámení se zobrazí celá zpráva:

![Rozšířená oznámení](local-notifications-images/08-expanded-notification.png)

Android podporuje tři styly rozšířené rozložení pro jednotlivé události oznámení:

-   ***Velký Text*** &ndash; v smluvní režimu zobrazí výňatek prvního řádku zprávy, za nímž následuje dvě tečky. V rozbaleném režimu zobrazí celou zprávu (jak je vidět v předchozím příkladu).

-   ***Doručená pošta*** &ndash; v smluvní režimu zobrazuje počet nových zpráv. V rozbaleném režimu se zobrazí první e-mailové zprávy nebo seznam zpráv v doručené poště.

-   ***Obrázek*** &ndash; v smluvní režimu, se zobrazí pouze text zprávy. V rozbaleném režimu se zobrazí text a obrázek.

[Nad rámec základní oznámení](#beyond-the-basic-notification) (dále v tomto článku) vysvětluje, jak vytvořit *velký Text*, *doručené pošty*, a *Image* oznámení.


## <a name="notification-creation"></a>Vytvoření oznámení

K vytvoření oznámení v Androidu, použijete [Notification.Builder](https://developer.xamarin.com/api/type/Android.App.Notification+Builder/) třídy. `Notification.Builder` byla zavedena v Androidu 3.0 pro zjednodušení vytváření objektů oznámení. Pokud chcete vytvořit oznámení, která jsou kompatibilní se staršími verzemi androidu, můžete použít [NotificationCompat.Builder](https://developer.android.com/reference/android/support/v4/app/NotificationCompat.Builder.html) místo na třídě `Notification.Builder` (naleznete v tématu [kompatibility](#compatibility) dále v tomto tématu Další informace o používání `NotificationCompat.Builder`).
`Notification.Builder` poskytuje metody pro nastavení různé možnosti v oznámení, jako například:

-   Obsah, včetně názvu, text zprávy a ikonu oznámení.

-   Styl oznámení, jako například *velký Text*, *doručené pošty*, nebo *Image* style.

-   Priorita oznámení: minimální, nízké, výchozí, vysoká, nebo maximální.

-   Viditelnost oznámení zamykací obrazovky: veřejný, privátní nebo tajný klíč.

-   Kategorie metadat, která pomáhá Android klasifikace a filtrovat oznámení.

-   Volitelné záměru, který označuje aktivita spustí po klepnutí na oznámení.

Po nastavení těchto možností v Tvůrci Generovat oznámení objekt, který obsahuje nastavení. Publikovat oznámení, předejte tento objekt oznámení *Správce oznámení*. Android poskytuje [správce](https://developer.xamarin.com/api/type/Android.App.NotificationManager/) třídy, který je zodpovědný za publikování oznámení a jejich zobrazení na uživatele. Odkaz na tuto třídu lze získat z jakékoli kontextu, jako je například aktivita nebo službu.


### <a name="how-to-generate-a-notification"></a>Postup generování oznámení

Ke generování oznámení v Androidu, postupujte podle těchto kroků:

1.  Vytvořit instanci `Notification.Builder` objektu.

2.  Volání různých metod `Notification.Builder` objektu, který chcete nastavit možnosti oznámení.

3.  Volání [sestavení](https://developer.xamarin.com/api/member/Android.App.Notification+Builder.Build/) metodu `Notification.Builder` objekt k vytvoření instance objektu oznámení.

4.  Volání [upozornění](https://developer.xamarin.com/api/member/Android.App.NotificationManager.Notify/(System.Int32%2cAndroid.App.Notification)) metoda Správce oznámení publikovat oznámení.

Je třeba zadat alespoň následující informace pro každý oznámení:

-   Malá ikona (24 × 24 distribučního bodu velikost)

-   Krátký název

-   Text oznámení

Následující příklad kódu ukazuje, jak používat `Notification.Builder` ke generování oznámení úrovně basic. Všimněte si, že `Notification.Builder` metody podporují [řetězení metod](http://en.wikipedia.org/wiki/Method_chaining); to znamená, každá metoda vrací objekt tvůrce, která se má vyvolat další volání metody můžete použít výsledek posledního volání metody:

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

V tomto příkladu nový `Notification.Builder` objektu s názvem `builder` je vytvořena instance, nastavení nadpisu a textu oznámení a oznámení ikona je načtena z **Resources/drawable/ic_notification.png**. Volání oznámení Tvůrce `Build` metoda vytvoří objekt oznámení s těmito nastaveními. Dalším krokem je volání `Notify` metoda Správce oznámení. Vyhledejte Správce oznámení, volání `GetSystemService`, jak je znázorněno výše.

`Notify` Metoda přijímá dva parametry: identifikátor oznámení a oznámení objektu. Identifikátor oznámení je jedinečné celé číslo, který identifikuje oznámení do vaší aplikace. V tomto příkladu identifikátoru oznámení nastavena na hodnotu nula (0); v produkční aplikace, budete však poskytnout jednotlivým oznámením jedinečný identifikátor. Opětovné použití předchozí hodnota identifikátoru ve volání `Notify` způsobí, že poslední oznámení přepsání.

Tento kód se spustí na zařízení s Androidem 5.0, vygeneruje upozornění, která vypadá jako v následujícím příkladu:

![Výsledek oznámení pro vzorový kód](local-notifications-images/09-hello-world.png)

Ikona oznámení se zobrazí na levé straně výrazu oznámení &ndash; tento snímek v kroužku &ldquo;můžu&rdquo; má alfa kanál tak, aby Android můžete nakreslit šedé pozadí cyklické jeho. Můžete také zadat ikonu bez kanál alfa. Zobrazit jako ikonu photographic image, najdete v článku [velký ikonu formát](#large-icon-format) dále v tomto tématu.

Časové razítko se nastavuje automaticky, ale toto nastavení lze přepsat pomocí volání [SetWhen](https://developer.xamarin.com/api/member/Android.App.Notification+Builder.SetWhen/) metody Tvůrce oznámení. Například následující příklad kódu nastaví časové razítko na aktuální čas:

```csharp
builder.SetWhen (Java.Lang.JavaSystem.CurrentTimeMillis());
```

### <a name="enabling-sound-and-vibration"></a>Povolení zvuk a vibrace

Pokud chcete oznámení také přehraje zvuk, můžete volat Tvůrce oznámení [SetDefaults](https://developer.xamarin.com/api/member/Android.App.Notification+Builder.SetDefaults/) metoda a předejte jí `NotificationDefaults.Sound` příznak:

```csharp
// Instantiate the notification builder and enable sound:
Notification.Builder builder = new Notification.Builder (this)
    .SetContentTitle ("Sample Notification")
    .SetContentText ("Hello World! This is my first notification!")
    .SetDefaults (NotificationDefaults.Sound)
    .SetSmallIcon (Resource.Drawable.ic_notification);
```

Toto volání `SetDefaults` způsobí, že zařízení přehraje zvuk, když se publikuje oznámení. Pokud chcete zařízení uvede do vibrace, spíše než zvukový signál, můžete předat `NotificationDefaults.Vibrate` k `SetDefaults.` Pokud chcete zařízení pro přehrávání zvuku a uvede do vibrace zařízení, můžete předat oba příznaky pro `SetDefaults`:

```csharp
builder.SetDefaults (NotificationDefaults.Sound | NotificationDefaults.Vibrate);
```

Pokud povolíte zvuk bez zadání možnosti přehrávání zvuku, používá Android výchozí zvuk oznámení systému. Však můžete změnit zvuk, která bude přehrána po zavolání Tvůrce oznámení [SetSound](https://developer.xamarin.com/api/member/Android.App.Notification+Builder.SetSound/p/Android.Net.Uri/) metody. Například alarm přehrávání zvuku s oznámení (namísto výchozí oznámení zvuk), můžete získat identifikátor URI alarm zvuk z [RingtoneManager](https://developer.xamarin.com/api/type/Android.Media.RingtoneManager/) a předat ho metodě `SetSound`:

```csharp
builder.SetSound (RingtoneManager.GetDefaultUri(RingtoneType.Alarm));
```

Alternativně můžete použít vyzváněcího výchozí systémové zvuku pro oznámení:

```csharp
builder.SetSound (RingtoneManager.GetDefaultUri(RingtoneType.Ringtone));
```

Po vytvoření objektu oznámení, je možné nastavit vlastnosti oznámení na objekt oznámení (namísto konfigurace je předem až `Notification.Builder` metody). Například namísto volání metody `SetDefaults` metoda umožňuje pronikavost na oznámení, mohou přímo upravit bitový příznak oznámení [výchozí](https://developer.xamarin.com/api/property/Android.App.Notification.Defaults/) vlastnost:

```csharp
// Build the notification:
Notification notification = builder.Build();

// Turn on vibrate:
notification.Defaults |= NotificationDefaults.Vibrate;
```

Tento příklad způsobí, že zařízení uvede do vibrace publikovaného oznámení.

<a name="updating-a-notification" />

### <a name="updating-a-notification"></a>Aktualizuje se upozornění

Pokud chcete aktualizovat obsah oznámení po jeho publikování, můžete znovu použít existující `Notification.Builder` objektu, který chcete vytvořit nový objekt oznámení a publikovat toto oznámení s identifikátorem posledního oznámení. Příklad:

```csharp
// Update the existing notification builder content:
builder.SetContentTitle ("Updated Notification");
builder.SetContentText ("Changed to this message.");

// Build a notification object with updated content:
notification = builder.Build();

// Publish the new notification with the existing ID:
notificationManager.Notify (notificationId, notification);
```

V tomto příkladu existující `Notification.Builder` objektu se používá k vytvoření nového objektu oznámení s jiný název a zprávu.
Nový objekt oznámení je publikována pomocí identifikátoru předchozí oznámení, a tím se aktualizuje obsah oznámení publikovali dříve:

![Aktualizovaná oznámení](local-notifications-images/12-updated-notification.png)

Tělo předchozí oznámení se znovu použije &ndash; pouze nadpis a text oznámení změn, zatímco v zásuvce oznámení se zobrazí oznámení. Text nadpisu změní z "Ukázkové oznámení" na "Aktualizovat oznámení" a text zprávy se změní z "Hello World! Toto je Moje první oznámení!" pro "Změna, aby se tato zpráva."

Oznámení zůstává viditelná, dokud jeden z tři věci se stane:

-   Je uživatel nezavře oznámení (nebo klepne *Vymazat vše*).

-   Aplikace provádí volání do `NotificationManager.Cancel`a předejte ID jedinečné oznámení, která byla přiřazena, když se publikuje oznámení.

-   Volání aplikace `NotificationManager.CancelAll`.

Další informace o aktualizaci oznámení systému Android, najdete v článku [upravit oznámení](http://developer.android.com/training/notify-user/managing.html#Updating).


### <a name="starting-an-activity-from-a-notification"></a>Aktivita od oznámení

V Androidu, je běžné, že oznámení přidruženo *akce* &ndash; aktivitu, která se spustí, když uživatel klepne na oznámení. Tato aktivita se může nacházet v jiné aplikaci, a dokonce i do jiného úkolu. Přidání akce pro oznámení, můžete vytvořit [PendingIntent](https://developer.xamarin.com/api/type/Android.App.PendingIntent/) objektu a přidružte `PendingIntent` oznámení. A `PendingIntent` je zvláštní druh záměru, který příjemce aplikacím umožňuje spustit předdefinované část kódu s oprávněními odesílací aplikace. Při klepnutí na oznámení Android spuštění aktivity určeném `PendingIntent`.

Následující fragment kódu ukazuje, jak vytvořit oznámení s `PendingIntent` , která se spustí aktivita původní aplikace `MainActivity`:

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

Tento kód je velmi podobný kód upozornění v předchozí části, s výjimkou, že `PendingIntent` se přidá do objektu oznámení. V tomto příkladu `PendingIntent` je přidružená k aktivitě původní aplikace předtím, než je předán do Tvůrce oznámení [SetContentIntent](https://developer.xamarin.com/api/member/Android.App.Notification+Builder.SetContentIntent/) metody. `PendingIntentFlags.OneShot` Příznak je předána `PendingIntent.GetActivity` metoda tak, aby `PendingIntent` je použit pouze jednou. Když tento kód se spustí, zobrazí se následující upozornění:

![První akce oznámení](local-notifications-images/10-first-action-notification.png)

Klepnutím na toto oznámení trvá uživatele zpět na původní aktivita.

V produkční aplikace, musí umět zpracovat vaší aplikace *zpět zásobník* když uživatel stiskne klávesu **zpět** tlačítko v rámci aktivity oznámení (Pokud nejste obeznámeni s Androidem úkoly a zpět zásobník, přečtěte si téma [ Úlohy a zpět zásobník](http://developer.android.com/guide/components/tasks-and-back-stack.html)).
Ve většině případů by měl zpětné navigace aktivita oznámení vrátit uživatel z aplikace a zpět na domovskou obrazovku. Ke správě back zásobníku, vaše aplikace používá [TaskStackBuilder](https://developer.xamarin.com/api/type/Android.App.TaskStackBuilder/) třídy za účelem vytvoření `PendingIntent` back zásobníkem.

Skutečná je potřeba zamyslet se, že původní aktivita může potřebovat k odesílání dat do aktivity oznámení. Například může ukazovat, že textová zpráva dorazila a aktivita oznámení (zpráva zobrazení obrazovky), vyžaduje ID zprávy pro zobrazení zprávy pro uživatele. Aktivity, která vytvoří `PendingIntent` můžete použít [Intent.PutExtra](https://developer.xamarin.com/api/member/Android.Content.Intent.PutExtra/p/System.String/System.String/) způsob, jak přidat data (například string) k příslušnému záměru tak, aby tato data se předají aktivitě oznámení.

Následující příklad kódu ukazuje, jak používat `TaskStackBuilder` ke správě back zásobníku a obsahuje příklad toho, jak odesílat oznámení aktivity volá řetězec jedna zpráva `SecondActivity`:

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

V tomto příkladu kód aplikace se skládá z dvou aktivit: `MainActivity` (která obsahuje oznámení kódu výše), a `SecondActivity`, uživateli se zobrazí po klepnutí na oznámení na obrazovce. Při spuštění tohoto kódu se zobrazí jednoduché oznámení (podobně jako v předchozím příkladu). Klepnutí na oznámení přesune uživatele `SecondActivity` obrazovky:

![Druhý snímek obrazovky aktivity](local-notifications-images/11-second-activity.png)

Řetězcovou zprávu (předán záměr `PutExtra` metoda) je načten v `SecondActivity` přes tento řádek kódu:

```csharp
// Get the message from the intent:
string message = Intent.Extras.GetString ("message", "");
```

Načíst tuto zprávu, "Greetings z MainActivity!," se zobrazí v `SecondActivity` obrazovky, jak je uvedeno ve výše uvedeném snímku obrazovky. Když uživatel stiskne klávesu **zpět** tlačítka při `SecondActivity`, navigace vede z aplikace a zpět na obrazovce, předchozí spuštění aplikace.

Další informace o vytváření čekající záměry najdete v tématu [PendingIntent](https://developer.xamarin.com/api/type/Android.App.PendingIntent/).


<a name="notif-chan" />

## <a name="notification-channels"></a>Kanály oznámení

Od verze Android 8.0 (Oreo), můžete použít *kanály oznámení* funkci pro vytvoření uživatelsky přizpůsobitelnými kanál pro každý typ oznámení, které chcete zobrazit. Kanály oznámení umožňují vám skupiny oznámení tak, aby všechna oznámení na kanál dodatku účtovány stejné chování. Například může mít kanál oznámení, která je určená pro oznámení, která vyžadují okamžitou pozornost a samostatný "náročném" kanál, který se používá pro informační zprávy.

**YouTube** aplikaci, která se instaluje se sadou Android Oreo obsahuje dvě kategorie upozornění: **stáhnout oznámení** a **Obecné oznámení**:

[![Oznámení obrazovky pro YouTube v Androidu Oreo](local-notifications-images/27-youtube-sml.png)](local-notifications-images/27-youtube.png#lightbox)

Každá z těchto kategorií odpovídá kanál oznámení. Implementuje aplikace YouTube **stáhnout oznámení** kanálu a **obecná upozornění** kanálu. Uživatel může klepnout **stáhnout oznámení**, zobrazuje na obrazovku nastavení kanálu oznámení pro aplikace:

[![Stáhněte si obrazovce oznámení pro aplikace YouTube](local-notifications-images/28-yt-download-sml.png)](local-notifications-images/28-yt-download.png#lightbox)

Na této obrazovce, uživatel může upravovat chování **Stáhnout** oznámení kanálu provedením následujících akcí:

-   Nastavit úroveň na důležitosti **naléhavé**, **vysokou**, **střední**, nebo **s nízkou**, které konfiguruje úroveň zvuku a vizuální přerušení.

-   Tečka oznámení zapněte nebo vypnout.

-   Blikající indikátor zapněte nebo vypnout.

-   Umožňuje zobrazit nebo skrýt oznámení na zamykací obrazovce.

-   Přepsat **Nerušit** nastavení.

**Obecná upozornění** kanál má podobné nastavení jako:

[![Obecná upozornění obrazovky pro aplikaci YouTube](local-notifications-images/29-yt-general-sml.png)](local-notifications-images/29-yt-general.png#lightbox)

Všimněte si, že nemáte absolutní kontrolu nad jak kanálů oznámení komunikovat s uživatelem &ndash; uživatel může upravit nastavení pro všechny kanál oznámení na zařízení, jak je znázorněno výše na snímcích obrazovky. Můžete ale nakonfigurovat výchozí hodnoty (jak se popsaných níže). Jak ukazují tyto příklady, novou funkci kanály oznámení umožňuje vám poskytnout přesnou kontrolu nad různé druhy oznámení uživatelům.

Měli byste přidat podporu pro kanály oznámení do vaší aplikace? Pokud na Androidu 8.0, vaše aplikace *musí* implementují kanály oznámení.
Aplikace je určená pro Oreo, která se pokouší odeslat místního oznámení pro uživatele bez použití kanálu oznámení se nezdaří pro zobrazení oznámení na zařízeních Oreo. Pokud není cílem Android 8.0, vaše aplikace bude stále spuštěn na Androidu 8.0, ale stejné chování oznámení jak by je při spuštění na Android 7.1 nebo starší.


### <a name="creating-a-notification-channel"></a>Vytvoření kanálu oznámení

Chcete-li vytvořit kanál oznámení, postupujte takto:

1. Vytvoření [NotificationChannel](https://developer.android.com/reference/android/app/NotificationChannel.html) objektu následujícím kódem:

    - Řetězec ID, který je jedinečný v rámci vašeho balíčku. V následujícím příkladu, řetězec `com.xamarin.myapp.urgent` se používá.

    - Zobrazovaná uživateli název kanálu (menší než 40 znaků). V následujícím příkladu název **naléhavé** se používá.

    - Důležitost kanál, který určuje, jak interruptive oznámení jsme zveřejnili na `NotificationChannel`. Může být význam `Default`, `High`, `Low`, `Max`, `Min`, `None`, nebo `Unspecified`.

    Předat tyto hodnoty [konstruktor](https://developer.android.com/reference/android/app/NotificationChannel.html#NotificationChannel%28java.lang.String,%20java.lang.CharSequence,%20int%29) (v tomto příkladu `Resource.String.noti_chan_urgent` je nastavena na **naléhavé**):

    ```csharp
    public const string URGENT_CHANNEL = "com.xamarin.myapp.urgent";
    . . .
    string chanName = GetString (Resource.String.noti_chan_urgent);
    var importance = NotificationImportance.High;
    NotificationChannel chan =
       new NotificationChannel (URGENT_CHANNEL, chanName, importance);
    ```

2.  Konfigurace `NotificationChannel` objektu s počátečním nastavením.
    Například následující kód konfiguruje `NotificationChannel` objektu tak, aby publikované do tohoto kanálu oznámení uvede do vibrace zařízení, který se ve výchozím nastavení se zobrazí na zamykací obrazovce:

    ```csharp
    chan.EnableVibration (true);
    chan.LockscreenVisibility = NotificationVisibility.Public;
    ```

3.  Odeslání oznámení objekt kanálu pro správce oznámení:

    ```csharp
    NotificationManager notificationManager =
        (NotificationManager) GetSystemService (NotificationService);
    notificationManager.CreateNotificationChannel (chan);
    ```


### <a name="posting-to-a-notifications-channel"></a>Odesílání oznámení kanálu

Odeslat oznámení na kanál oznámení, postupujte takto:

1.  Konfigurace oznámení pomocí `Notification.Builder`a předejte ID kanálu `SetChannelId` metoda. Příklad:

    ```csharp
    Notification.Builder builder = new Notification.Builder (this)
        .SetContentTitle ("Attention!")
        .SetContentText ("This is an urgent notification message!")
        .SetChannelId (URGENT_CHANNEL);
    ```

2.  Sestavení a vydání oznámení pomocí Správce oznámení [upozornění](https://developer.xamarin.com/api/member/Android.App.NotificationManager.Notify/p/System.Int32/Android.App.Notification/) metody:

    ```csharp
    const int notificationId = 0;
    notificationManager.Notify (notificationId, builder.Build());
    ```

Výše uvedené kroky k vytvoření jiného kanálu oznámení informační zprávy můžete opakovat. Tento druhý kanál můžete je ve výchozím nastavení, zakázat vibrace, nastavte výchozí viditelnost zámek obrazovky na `Private`a nastavte oznámení význam `Default`.

Kompletní příklad Android Oreo oznámení kanálů v akci, najdete v článku [NotificationChannels](https://developer.xamarin.com/samples/monodroid/android-o/NotificationChannels) ukázka; Tato ukázková aplikace spravuje dva kanály a nastaví možnosti Další oznámení.



<a name="beyond-the-basic-notification" />

## <a name="beyond-the-basic-notification"></a>Nad rámec základní oznámení

Oznámení ve výchozím nastavení formátu no-frills základní rozložení v Androidu, ale tento základní formát můžete vylepšit tím, že další `Notification.Builder` volání metody. V této části se dozvíte, jak přidat ikonu velké fotky na oznámení a uvidíte příklady toho, jak vytvořit rozšířené rozložení oznámení.

<a name="large-icon-format" />

### <a name="large-icon-format"></a>Formát velké ikony

Oznámení systému Android obvykle zobrazuje ikona původní aplikace (na levé straně oznámení). Však můžete zobrazit oznámení obrázek či fotografii ( *LargeIcon*) namísto na standardní ikonu. Zasílání zpráv aplikace může například zobrazit fotografii odesílatele a ne na ikonu aplikace.

Tady je příklad základní oznámení Android 5.0 &ndash; zobrazí pouze na ikonu malou aplikaci:

![Příklad normální oznámení](local-notifications-images/13-sample-notification.png)

A zde je snímek obrazovky oznámení po úpravách zobrazit velké ikony &ndash; používá ikony vytvořené z bitové kopie opic kódu Xamarin:

![Příklad oznámení velké ikony](local-notifications-images/14-large-icon-sample.png)

Všimněte si, že když oznámení se zobrazí ve formátu velké ikony, ikonu malou aplikaci zobrazí jako oznámení "BADGE" v pravém dolním rohu velké ikony.

Pokud chcete použít obraz jako velké ikony v oznámení, volání oznámení Tvůrce [SetLargeIcon](https://developer.xamarin.com/api/member/Android.App.Notification+Builder.SetLargeIcon/) metoda a předejte jí rastrový obrázek. Na rozdíl od `SetSmallIcon`, `SetLargeIcon` přijímá pouze rastrový obrázek. Převést obrázek na rastrový obrázek, můžete použít [BitmapFactory](https://developer.xamarin.com/api/type/Android.Graphics.BitmapFactory/) třídy. Příklad:

```csharp
builder.SetLargeIcon (BitmapFactory.DecodeResource (Resources, Resource.Drawable.monkey_icon));
```

Tento příklad kódu se otevře soubor bitové kopie na **Resources/drawable/monkey_icon.png**, převede ho na rastrový obrázek a předá výsledný rastrový obrázek pro `Notification.Builder`. Obvykle je větší než na ikonu rozlišení obrázku zdroj &ndash; ale mnohem větší. Bitovou kopii, která je příliš velká, může způsobit zbytečné operací změny velikosti, které by mohly zpoždění zveřejnění oznámení.
Další informace o velikostech ikonu oznámení v Androidu najdete v tématu [oznamovací ikony](http://developer.android.com/design/style/iconography.html#notification).


### <a name="big-text-style"></a>Styl velký objem textu

*Velký Text* styl je rozšířené rozložení šablony, který můžete použít pro zobrazování dlouhé zprávy v oznámeních. Stejně jako všechny rozšířené rozložení oznámení oznámení velký Text primárně zobrazen ve formátu kompaktnější:

![Příklad velký Text oznámení](local-notifications-images/15-big-text-notification.png)

V tomto formátu se zobrazí pouze výňatek zprávy ukončena dvě tečky. Když uživatel přetáhne na oznámení, rozšiřuje zobrazí celé oznámení:

![Rozšířená oznámení velký Text](local-notifications-images/16-big-text-expanded.png)

Tento formát rozšířené rozložení obsahuje také souhrnný text v dolní části oznámení. Maximální výška *velký Text* oznámení je 256 distribučního bodu.

K vytvoření *velký Text* oznámení, můžete vytvořit instanci `Notification.Builder` objektu, stejně jako dříve a pak vytvořit instanci a přidat [BigTextStyle](https://developer.xamarin.com/api/type/Android.App.Notification+BigTextStyle/) do objektu `Notification.Builder` objektu. Příklad:

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

V tomto příkladu, text zprávy a souhrnný text jsou uloženy v `BigTextStyle` objektu (`textStyle`) předtím, než jsou předány do `Notification.Builder.`


### <a name="image-style"></a>Styl obrázku

*Image* styl (také nazývané *celkový přehled* stylu) je ve formátu rozšířené oznámení, který můžete použít k zobrazení obrázku v textu oznámení. Například můžete použít snímek obrazovky aplikace nebo aplikace fotky *Image* oznámení styl poskytnout uživateli oznámení o poslední image, která byla zaznamenána. Všimněte si, že maximální výšku *Image* oznámení je 256 distribučního bodu &ndash; Android změní velikost obrázku tak, aby odpovídal toto omezení maximální výšku, v rámci omezení paměti k dispozici.

Stejně jako všechny rozbalené rozložení oznámení *Image* oznámení se nejprve zobrazí v kompaktním formátu, který zobrazí výňatek související text zprávy:

![Oznámení Compact obrázek ukazuje žádný obrázek](local-notifications-images/17-image-compact.png)

Když uživatel přetáhne *Image* oznámení, rozšiřuje zobrazí obrázek. Tady je příklad rozšířenou verzi předchozí oznámení:

![Rozšířené image oznámení zjistí informace o obrázku](local-notifications-images/18-image-expanded.png)

Všimněte si, že při oznámení se zobrazí v kompaktním formátu, se zobrazí oznámení text (text, který je předán do Tvůrce oznámení `SetContentText` způsob, jak je uvedeno výše). Ale při rozbalení oznámení zobrazí obrázek, zobrazí text souhrnu výše na obrázku.

K vytvoření *Image* oznámení, můžete vytvořit instanci `Notification.Builder` objektu jako předtím a pak vytvořit a vložit [BigPictureStyle](https://developer.xamarin.com/api/type/Android.App.Notification+BigPictureStyle/) objektu do `Notification.Builder` objektu. Příklad:

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

Podobně jako `SetLargeIcon` metoda `Notification.Builder`, [BigPicture](https://developer.xamarin.com/api/member/Android.App.Notification+BigPictureStyle.BigPicture/) metodu `BigPictureStyle` vyžaduje rastrový obrázek, který chcete zobrazit v textu oznámení. V tomto příkladu [DecodeResource](https://developer.xamarin.com/api/member/Android.Graphics.BitmapFactory.DecodeResource/(Android.Content.Res.Resources%2cSystem.Int32)) metoda `BitmapFactory` čtení souboru obrázku se nachází v **Resources/drawable/x_bldg.png** a převede jej na rastrový obrázek.

Můžete také zobrazit bitové kopie, které nejsou zabalené jako prostředek. Například následující ukázkový kód načte bitovou kopii z místní SD karty a zobrazí jej v *Image* oznámení:

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

V tomto příkladu se soubor bitové kopie nachází v **/sdcard/Pictures/my-tshirt.jpg** je načtena, se změněnou velikostí do poloviny původní velikost a poté převeden na rastrový obrázek pro použití v oznámení:

![Příklad trička image v oznámení](local-notifications-images/19-tshirt-notification.png)

Pokud si nejste jisti předem velikost souboru obrázku, je vhodné zabalte volání do [BitmapFactory.DecodeFile](https://developer.xamarin.com/api/member/Android.Graphics.BitmapFactory.DecodeFile/p/System.String/Android.Graphics.BitmapFactory+Options/) v obslužné rutině výjimky &ndash; `OutOfMemoryError` může být vyvolána výjimka, pokud je příliš velký pro bitovou kopii Android pro změnu velikosti.

Další informace o načítání a dekódování velké rastrové obrázky, naleznete v tématu [zatížení velké rastrové obrázky efektivně](https://github.com/xamarin/recipes/tree/master/Recipes/android/resources/general/load_large_bitmaps_efficiently).


### <a name="inbox-style"></a>Styl doručené pošty

*Doručené pošty* formát je šablonu rozšířené rozložení určená pro zobrazení samostatné řádky textu (například e-mailových schránek, souhrn) v textu oznámení. *Doručené pošty* formát oznámení se nejprve zobrazí v kompaktním formátu:

![Příklad oznámení compact doručené pošty](local-notifications-images/20-inbox-compact.png)

Když uživatel přetáhne na oznámení, rozšiřuje zobrazíte e-mailu souhrnné informace, jak je znázorněno na následujícím snímku obrazovky:

![Příklad oznámení Doručená pošta rozšířit](local-notifications-images/21-inbox-expanded.png)

Vytvoření *doručené pošty* oznámení, můžete vytvořit instanci `Notification.Builder` objektu, stejně jako předtím a přidejte [InboxStyle](https://developer.xamarin.com/api/type/Android.App.Notification+InboxStyle/) objektu `Notification.Builder`. Příklad:

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

Chcete-li přidat nové řádky textu do textu oznámení, zavolejte [Addline](https://developer.xamarin.com/api/member/Android.App.Notification+InboxStyle.AddLine/p/System.String/) metodu `InboxStyle` objektu (maximální výšku *doručené pošty* oznámení je 256 distribučního bodu). Všimněte si, že na rozdíl od *velký Text* styl *doručené pošty* styl podporuje jednotlivé řádky textu v textu oznámení.

Můžete také použít *doručené pošty* styl pro všechna oznámení, kterou je potřeba zobrazují jednotlivé řádky textu ve formátu rozbalený. Například *doručené pošty* styl oznámení je možné kombinovat více čekající oznámení do souhrnu oznámení &ndash; můžete aktualizovat jeden *doručené pošty* stylu oznámení New. řádky obsah oznámení (naleznete v tématu [aktualizace oznámení](#updating-a-notification) výše), spíše než generovat nepřetržitý datový proud nové, většinou podobně jako oznámení. Další informace o tento přístup, najdete v části [shrnout oznámení](http://developer.android.com/design/patterns/notifications.html#summarize_your_notifications).


## <a name="configuring-metadata"></a>Konfigurace metadat

`Notification.Builder` obsahuje metody, které můžete volat a nastavit metadata o oznámení, jako je například priorita, viditelnost a kategorie. Android na základě těchto informací &mdash; spolu s uživatelská nastavení &mdash; určíte, jak a kdy mají zobrazovat oznámení.


### <a name="priority-settings"></a>Nastavení priority

Nastavení priority oznámení určuje dva výsledky, když se publikuje oznámení:

-   Kde se ve vztahu k další oznámení zobrazí oznámení.
    Například s vysokou prioritou oznámení se zobrazují nad nižší prioritu oznámení v zásuvce oznámení bez ohledu na to při každé oznámení byla publikována.

-   Oznámení určuje, zda se zobrazí ve formátu Heads-up oznámení (Android 5.0 nebo novější). Pouze *vysokou* a *maximální* prioritu oznámení se zobrazují jako pohotové oznámení.

Xamarin.Android definuje následující výčty pro nastavení priority oznámení:

-   `NotificationPriority.Max` &ndash; Upozorní uživatele, aby o urgentní nebo kritický stav (například příchozí volání, zapněte zapnout pokynů nebo nouzový výstrahy). V Androidu 5.0 a novějším se zobrazují oznámení Maximální priorita ve formátu indikačního.

-   `NotificationPriority.High` &ndash; Informuje uživatele o důležitých událostech (například doručení zpráv chatování v reálném čase nebo důležitých e-mailů). V Androidu 5.0 a novějším se zobrazují oznámení s vysokou prioritou ve formátu indikačního.

-   `NotificationPriority.Default` &ndash; Upozorní uživatele z podmínek, které mají střední úroveň důležitosti.

-   `NotificationPriority.Low` &ndash; Urgentní informace, že uživatel musí být informováni o (například připomenutí aktualizace softwaru nebo aktualizace sociální sítě).

-   `NotificationPriority.Min` &ndash; Základní informace, že uživatel oznámení pouze tehdy, když zobrazení oznámení (například informace o umístění nebo počasí).

Chcete-li nastavit prioritu oznámení, zavolejte [SetPriority –](https://developer.xamarin.com/api/member/Android.App.Notification+Builder.SetPriority/) metodu `Notification.Builder` objekt předávajícího úroveň priority. Příklad:

```csharp
builder.SetPriority (NotificationPriority.High);
```

V následujícím příkladu, upozornění s vysokou prioritou, "Zpráva je důležitá!" Zobrazí se v horní části panel oznámení:

![Příklad upozornění s vysokou prioritou](local-notifications-images/22-hi-priority-drawer.png)

Protože toto je oznámení s vysokou prioritou, zobrazí se také jako pohotové oznámení nad aktuální obrazovka aktivita uživatele v Androidu 5.0:

![Příklad indikačního oznámení](local-notifications-images/23-heads-up-example.png)

V následujícím příkladu "Si mysleli, že pro tento den" oznámení s nízkou prioritou se zobrazuje v části s vyšší prioritou baterie úrovně oznámení:

![Příklad upozornění s nízkou prioritou](local-notifications-images/24-lo-priority-drawer.png)

Oznámení "Přiměje pro den", je oznámení s nízkou prioritou, nezobrazí se Android Heads-up formát.


### <a name="visibility-settings"></a>Nastavení viditelnosti

Od verze Android 5.0, *viditelnost* nastavení je možné určit, kolik obsahu oznámení se zobrazí na zabezpečené zamykací obrazovky.
Xamarin.Android definuje následující výčty pro nastavení viditelnosti oznámení:

-   `NotificationVisibility.Public` &ndash; Úplný obsah oznámení se zobrazí na zabezpečené zamykací obrazovky.

-   `NotificationVisibility.Private` &ndash; Pouze základní informace se zobrazí na zabezpečené zamykací obrazovce (například na ikonu oznámení a název aplikace, který zveřejnil ji), ale zbytek podrobnosti oznámení jsou skryté. Všechna oznámení ve výchozím nastavení `NotificationVisibility.Private`.

-   `NotificationVisibility.Secret` &ndash; Nezobrazí se na zabezpečené zamykací obrazovky, dokonce ani na ikonu oznámení. Obsah oznámení je k dispozici až poté, co uživatel odemknutí zařízení.

Chcete-li nastavit, zda se oznámení, volání aplikace `SetVisibility` metodu `Notification.Builder` objektu, předejte nastavení viditelnosti. Například toto volání `SetVisibility` díky oznámení `Private`:

```csharp
builder.SetVisibility (NotificationVisibility.Private);
```

Když `Private` se pošle oznámení, název a ikona aplikace se zobrazí na zabezpečené zamykací obrazovky. Místo zprávy oznámení uživateli se zobrazí "Odemčení zařízení pro toto oznámení":

![Odemknout zprávy oznámení zařízení](local-notifications-images/25-lockscreen-private.png)

V tomto příkladu **NotificationsLab** je název původní aplikace. Tato verze zrevidovaně oznámení se zobrazí pouze v případě, zamykací obrazovky je zabezpečený (například zabezpečené pomocí kódu PIN, vzorem nebo heslo) &ndash; Pokud zamykací obrazovky není bezpečná, úplný obsah oznámení je k dispozici na zamykací obrazovce.


### <a name="category-settings"></a>Nastavení kategorie

Od verze Android 5.0, jsou k dispozici pro řazení a filtrování oznámení předdefinovaným kategoriím. Xamarin.Android poskytuje následující výčty pro tyto kategorie:

-   `Notification.CategoryCall` &ndash; Příchozí telefonní hovor.

-   `Notification.CategoryMessage` &ndash; Příchozí textovou zprávu.

-   `Notification.CategoryAlarm` &ndash; Upozornění podmínky nebo časovač vypršení platnosti.

-   `Notification.CategoryEmail` &ndash; Příchozí e-mailové zprávy.

-   `Notification.CategoryEvent` &ndash; Události kalendáře.

-   `Notification.CategoryPromo` &ndash; Akční zpráv nebo oznámení o inzerovaném programu.

-   `Notification.CategoryProgress` &ndash; Průběh operace na pozadí.

-   `Notification.CategorySocial` &ndash; Sociální sítě aktualizace.

-   `Notification.CategoryError` &ndash; Selhání procesu na pozadí operace nebo ověřování.

-   `Notification.CategoryTransport` &ndash; Aktualizace přehrávání média.

-   `Notification.CategorySystem` &ndash; Rezervované pro systémové použití (stav systému nebo zařízení).

-   `Notification.CategoryService` &ndash; Určuje, zda je spuštěna služba na pozadí.

-   `Notification.CategoryRecommendation` &ndash; Zpráva doporučení související s aktuálně spuštěnou aplikaci.

-   `Notification.CategoryStatus` &ndash; Informace o zařízení.

Při řazení jsou oznámení, oznámení prioritu má přednost před jeho nastavením kategorie. Například s vysokou prioritou oznámení se zobrazí jako pohotové i v případě, že patří do `Promo` kategorie. Nastavení kategorie oznámení, volání `SetCategory` metodu `Notification.Builder` objektu, předejte nastavení kategorie. Příklad:

```csharp
builder.SetCategory (Notification.CategoryCall);
```

*Nerušit* funkce (Novinky Android 5.0) vyfiltruje oznámení na základě kategorií. Například *Nerušit* obrazovky v **nastavení** umožňuje uživateli vyloučení oznámení pro telefonní hovory a zprávy:

![Nerušit přepínače obrazovky](local-notifications-images/26-do-not-disturb.png)

Když uživatel konfiguruje *Nerušit* blokování přerušení všechny s výjimkou telefonní hovory (jak je znázorněno na snímku obrazovky výše) umožňuje Android oznámení s nastavením kategorie `Notification.CategoryCall` zobrazí se, když je zařízení Probíhá *Nerušit* režimu. Všimněte si, že `Notification.CategoryAlarm` oznámení nikdy blokována *Nerušit* režimu.


<a name="compatibility" />

## <a name="compatibility"></a>Kompatibilita

Pokud vytváříte aplikaci, která bude spustit také na starší verze systému Android (napravovat je co úroveň rozhraní API 4), můžete použít [NotificationCompat.Builder](https://developer.android.com/reference/android/support/v4/app/NotificationCompat.Builder.html) místo na třídě `Notification.Builder`. Při vytváření oznámení s `NotificationCompat.Builder`, Android zajišťuje, že obsah oznámení na úrovni basic se zobrazí správně na starší zařízení. Ale vzhledem k tomu, že některé funkce oznámení s týdenním nejsou k dispozici u starších verzí androidu, váš kód musí explicitně zpracovat problémy s kompatibilitou pro rozšířené oznámení styly, kategorie a viditelnost úrovní, jak je popsáno níže.

Použití `NotificationCompat.Builder` ve vaší aplikaci, musíte zahrnout [knihovnu podpory pro Android v4](https://www.nuget.org/packages/Xamarin.Android.Support.v4/) NuGet ve vašem projektu.

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

Jak ukazuje tento příklad, volání metod pro možnosti základní oznámení jsou stejné jako `Notification.Builder`. Má však váš kód pro zpracování klesající kompatibility problémy složitější oznámení (popsané v další části).

[LocalNotifications](https://developer.xamarin.com/samples/monodroid/LocalNotifications) Ukázka předvádí, jak používat `NotificationCompat.Builder` spustit druhou aktivitu z oznámení. Tento ukázkový kód je podrobně [používání místních oznámení v Xamarin.Android](~/android/app-fundamentals/notifications/local-notifications-walkthrough.md) návodu.


### <a name="notification-styles"></a>Styly oznámení

Chcete-li vytvořit *velký Text*, *Image*, nebo *doručené pošty* stylu oznámení s `NotificationCompat.Builder`, vaše aplikace musí používat kompatibility verzí z těchto stylů vyplývají. Chcete-li například použít *velký Text* stylu, vytvořit instanci `NotificationCompat.BigTextstyle`:

```csharp
NotificationCompat.BigTextStyle textStyle = new NotificationCompat.BigTextStyle();

// Plug this style into the builder:
builder.SetStyle (textStyle);
```

Podobně můžete použít aplikaci `NotificationCompat.InboxStyle` a `NotificationCompat.BigPictureStyle` pro *doručené pošty* a *Image* styly, v uvedeném pořadí.


### <a name="notification-priority-and-category"></a>Oznámení Priority a kategorie

`NotificationCompat.Builder` podporuje `SetPriority` – metoda (k dispozici od verze Android 4.1). Ale `SetCategory` je metoda *není* podporuje `NotificationCompat.Builder` protože kategorií jsou součástí nového systému metadata oznámení, která byla zavedena v Androidu 5.0.

Pro podporu starších verzí Androidu, kde `SetCategory` není k dispozici, váš kód můžete zkontrolovat úroveň rozhraní API za běhu, abyste podmíněně volání `SetCategory` při úroveň rozhraní API je roven nebo větší než Android 5.0 (úroveň rozhraní API 21):

```csharp
if ((int) Android.OS.Build.Version.SdkInt >= 21) {
    builder.SetCategory (Notification.CategoryEmail);
}
```

V tomto příkladu aplikaci prvku **Cílová architektura** je nastavena na Android 5.0 a **minimální verze Androidu** je nastavena na **Android 4.1 (rozhraní API Level 16)**. Protože `SetCategory` je k dispozici v rozhraní API úrovně 21 a novější, tento příklad kódu bude volat `SetCategory` pouze pokud je k dispozici &ndash; nebude volat `SetCategory` Pokud úroveň rozhraní API je menší než
21.


### <a name="lockscreen-visibility"></a>Viditelnost zamykací obrazovky

Protože Android nepodporuje oznámení zamykací obrazovky před Android 5.0 (úroveň rozhraní API 21), `NotificationCompat.Builder` nepodporuje `SetVisibility` metody. Jak jsme vysvětlili výše pro `SetCategory`, váš kód můžete zkontrolovat úroveň rozhraní API modulu runtime a volání `SetVisiblity` pouze pokud je k dispozici:

```csharp
if ((int) Android.OS.Build.Version.SdkInt >= 21) {
    builder.SetVisibility (Notification.Public);
}
```


## <a name="summary"></a>Souhrn

Tento článek vysvětlil, jak vytvořit místní oznámení v Androidu. Popisuje anatomie oznámení, to bylo vysvětleno, jak používat `Notification.Builder` k vytvoření oznámení, jak styl oznámení ve velké ikony, *velký Text*, *Image* a *doručené pošty*  formátů, konfigurace nastavení oznámení metadat jako je například priorita, viditelnost a kategorie a o spouštění aktivity z oznámení. Tento článek také popisuje, jak fungují tato nastavení oznámení s novou indikačního zamykací obrazovky, a *Nerušit* funkce zavedené v Androidu 5.0. Nakonec jste zjistili, jak používat `NotificationCompat.Builder` udržovat oznámení kompatibility se staršími verzemi androidu.

Pokyny pro návrh oznámení pro Android, najdete v článku [oznámení](http://developer.android.com/preview/notifications.html).


## <a name="related-links"></a>Související odkazy

- [NotificationsLab (ukázka)](https://developer.xamarin.com/samples/monodroid/android5.0/NotificationsLab/)
- [LocalNotifications (ukázka)](https://developer.xamarin.com/samples/monodroid/LocalNotifications/)
- [Místní oznámení v Androidu návodu](~/android/app-fundamentals/notifications/local-notifications-walkthrough.md)
- [Oznámení](http://developer.android.com/design/patterns/notifications.html)
- [Upozornění uživatele](http://developer.android.com/training/notify-user/index.html)
- [Oznámení](https://developer.xamarin.com/api/type/Android.App.Notification/)
- [Správce](https://developer.xamarin.com/api/type/Android.App.NotificationManager/)
- [NotificationCompat.Builder](https://developer.android.com/reference/android/support/v4/app/NotificationCompat.Builder.html)
- [PendingIntent](https://developer.xamarin.com/api/type/Android.App.PendingIntent/)
