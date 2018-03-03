---
title: "Úlohy na pozadí"
description: "Ujistěte se, že aplikace sledovat má vždy nejnovější data a snímky ukotvení pomocí nové úlohy na pozadí watchOS 3."
ms.topic: article
ms.prod: xamarin
ms.assetid: 2049C430-7566-45F8-9E3D-1446F484981E
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/13/2017
ms.openlocfilehash: 524d551a96dd1352d86671238a63c103cef9b0c5
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 02/27/2018
---
# <a name="background-tasks"></a>Úlohy na pozadí

_Ujistěte se, že aplikace sledovat má vždy nejnovější data a snímky ukotvení pomocí nové úlohy na pozadí watchOS 3._

S watchOS 3 existují tři hlavní způsoby, sledování aplikací můžete zachovat informace o jeho aktuální: 

- Pomocí jedné z několik nové úlohy na pozadí. 
- S jedním z jeho komplikace na sledování tučné (poskytnutí velmi je čas aktualizovat). 
- Museli nové ukotvení kódu pin uživatele pro aplikaci (kde jeho ukládají v paměti a často aktualizovat). 

## <a name="keeping-an-app-up-to-date"></a>Zachovat aktuální aplikaci

Před hovoříte o všechny způsoby, které vývojář může chránit data a uživatelské rozhraní aplikace watchOS aktuální a aktualizované, bude v této části si prohlédněte typickou sadu způsobů použití a jak může uživatel přesunout mezi jejich iPhone a jejich Apple Watch celý den na základě  čas den a aktivity aktuálně dělají (například řídí).

Proveďte v následujícím příkladu:

[ ![](background-tasks-images/update00.png "Jak může uživatel přesunout mezi jejich iPhone a jejich Apple Watch během dne")](background-tasks-images/update00.png)

1. Ráno, při čekání na řádku pro kávy uživatel prohlíží aktuální novinky na jejich zařízení iPhone několik minut.
2. Před opuštěním kavárně, se rychle zjistit, počasí s komplikace na jejich sledovat řez.
3. Před oběd používají aplikace mapy na iPhone a najít blízkým restaurace sešit rezervace ke splnění klienta.
4. Při cestě do restaurace, získají oznámení na jejich Apple Watch a s rychlého přehledu, věděli, že jejich oběda pozdní běží.
5. Večer, používají aplikace mapy na iPhone ke kontrole provoz před řídí Domů.
6. Na cestě domácí, obdrží oznámení iMessage na jejich Apple Watch, které je vyzývají k vyzvednutí některé mléka a používají funkci Rychlé odpověď k odeslání odpovědi "OK".

Z důvodu "rychlého přehledu" (méně než tři sekundy) povaha jak je chce uživatel použít aplikaci Apple Watch, existuje obvykle není dostatek času pro aplikaci načíst požadované informace a aktualizujte jeho uživatelském rozhraní, než se zobrazí uživateli.

Pomocí nové rozhraní API Apple použila v watchOS 3, můžete naplánovat aplikace na _aktualizace na pozadí_ a mít připravené požadované informace, aby uživatel požaduje. V příkladu komplikace počasí výše popsané proveďte:

[ ![](background-tasks-images/update01.png "Příklad komplikace počasí")](background-tasks-images/update01.png)

1. Plány aplikace v určitém čase probouzet pomocí systému. 
2. Aplikace načte informace, že bude nutné vygenerovat aktualizace.
3. Aplikace regeneruje svoje uživatelské rozhraní tak, aby odrážela nová data.
4. Když uživatel glances na aplikace komplikace, obsahuje aktuální informace bez uživatel musí počkat na aktualizaci.

Jak je vidět výše, systém watchOS probudí aplikace pomocí jedné nebo více úloh, které má velmi omezená fondu k dispozici:

[ ![](background-tasks-images/update02.png "Systém watchOS probudí aplikace pomocí jedné nebo více úloh")](background-tasks-images/update02.png)

Apple navrhnout optimální využití této úlohy (protože je omezená prostředků do aplikace) tím, že na něj, dokud aplikace nedokončí proces aktualizace sám sebe.

Systém poskytuje tyto úlohy voláním nové `HandleBackgroundTasks` metodu `WKExtensionDelegate` delegovat. Příklad:

```csharp
using System;
using Foundation;
using WatchKit;

namespace MonkeyWatch.MonkeySeeExtension
{
    public class ExtensionDelegate : WKExtensionDelegate
    {
        #region Constructors
        public ExtensionDelegate ()
        {
        }
        #endregion

        #region Override Methods
        public override void HandleBackgroundTasks (NSSet<WKRefreshBackgroundTask> backgroundTasks)
        {
            // Handle background request here
            ...
        }
        #endregion
    }
}
```

Když aplikace dokončí danou úlohu, vrátí se do systému jeho označením dokončena:

[ ![](background-tasks-images/update03.png "Vrátí úlohu systému jeho označením byla dokončena")](background-tasks-images/update03.png)

<a name="New-Background-Tasks" />

## <a name="new-background-tasks"></a>Nové úlohy na pozadí

watchOS 3 zavádí několik úlohy na pozadí, které aplikace můžete použít k aktualizovat informace o zajištění, že obsahuje obsah, uživatel musí před otevření aplikace, jako například:

- **Aplikace aktualizace na pozadí** – [WKApplicationRefreshBackgroundTask](https://developer.apple.com/reference/watchkit/wkapplicationrefreshbackgroundtask) úloha umožňuje aplikaci aktualizovat její stav na pozadí. Obvykle to bude zahrnovat jiné úlohy, jako je například stahování nový obsah z Internetu pomocí [NSUrlSession](https://developer.apple.com/reference/foundation/nsurlsession).
- **Na pozadí aktualizovat snímek** – [WKSnapshotRefreshBackgroundTask](https://developer.apple.com/reference/watchkit/wksnapshotrefreshbackgroundtask) úloha umožňuje aplikaci aktualizovat jeho obsah a uživatelského rozhraní, než systém pořídí snímek, který se použije k naplnění ukotvení.
- **Sledování připojení na pozadí** – [WKWatchConnectivityRefreshBackgroundTask](https://developer.apple.com/reference/watchkit/wkwatchconnectivityrefreshbackgroundtask) úloha je spuštěna aplikace, když obdrží data na pozadí z spárované zařízení iPhone.
- **Adresa URL relace na pozadí** – [WKURLSessionRefreshBackgroundTask](https://developer.apple.com/reference/watchkit/wkurlsessionrefreshbackgroundtask) úloha je spuštěna aplikace při přenosu na pozadí vyžaduje ověření nebo dokončení (úspěšně nebo chyba).

Tyto úlohy budou podrobně v následujících částech.

<a name="WKApplicationRefreshBackgroundTask" />

### <a name="wkapplicationrefreshbackgroundtask"></a>WKApplicationRefreshBackgroundTask

`WKApplicationRefreshBackgroundTask` Je obecný úkol, který lze naplánovat tak, aby měl aplikace probuzený v budoucnosti:

[ ![](background-tasks-images/update04.png "WKApplicationRefreshBackgroundTask, probuzený v budoucnosti")](background-tasks-images/update04.png)

V modulu runtime úlohy můžete provést jakýkoli druh místní zpracování například aktualizace časová osa komplikace nebo načíst některá požadovaná data se aplikace `NSUrlSession`.


<a name="WKURLSessionRefreshBackgroundTask" />

### <a name="wkurlsessionrefreshbackgroundtask"></a>WKURLSessionRefreshBackgroundTask

Systém bude odesílat `WKURLSessionRefreshBackgroundTask` při data dokončil stahování a lze ji zpracovat aplikace:

[ ![](background-tasks-images/update05.png "WKURLSessionRefreshBackgroundTask po dokončení stahování dat")](background-tasks-images/update05.png)

Aplikace není ponechat spuštěnou při stahování dat na pozadí. Místo toho aplikace plánuje požadavku na data, pak je pozastavená a systém zpracovává stahování dat, pouze reawakening aplikaci po dokončení stahování.

<a name="WKSnapshotRefreshBackgroundTask" />

### <a name="wksnapshotrefreshbackgroundtask"></a>WKSnapshotRefreshBackgroundTask

V watchOS 3 byl přidán Apple ukotvení, kde mohou uživatelé připnout své oblíbené aplikace a rychle přistupovat k nim. Když uživatel stiskne tlačítko straně na Apple Watch, zobrazí se Galerie definovaného aplikace snímků. Uživatel může prstem doleva nebo doprava najít požadovanou aplikaci a pak klepněte na aplikaci spustíte nahraďte snímek spuštěné aplikaci rozhraní.

[ ![](background-tasks-images/update06.png "Nahraďte snímek rozhraní spuštěné aplikace")](background-tasks-images/update06.png)

Systém pravidelně trvá snímky uživatelském rozhraní aplikace (odesláním `WKSnapshotRefreshBackgroundTask`) a používá tyto snímky k naplnění ukotvení. watchOS poskytuje aplikaci příležitost k aktualizaci uživatelského rozhraní a obsahu předtím, než se provede tento snímek.

Snímky jsou velmi důležité pro watchOS 3, protože fungují jako obrázky verzi preview a spuštění aplikace. Pokud uživatel vyrovná v aplikaci v ukotvení, budou rozšíření na celou obrazovku, zadejte popředí a spustit, takže je nutné, že se snímek aktuální:

[ ![](background-tasks-images/update07.png "Pokud uživatel vyrovná v aplikaci v ukotvení, se rozbalí na celou obrazovku")](background-tasks-images/update07.png)

Znovu, bude vydávat systému `WKSnapshotRefreshBackgroundTask` tak, aby aplikace můžete připravit (aktualizace, data a uživatelského rozhraní) před pořízení snímku:

[ ![](background-tasks-images/update08.png "Aplikace můžete připravit se tak aktualizace dat a uživatelské rozhraní před pořízení snímku")](background-tasks-images/update08.png)

Když aplikaci označí `WKSnapshotRefreshBackgroundTask` dokončit, systém bude automaticky pořízení snímku uživatelském rozhraní aplikace.

> [!IMPORTANT]
> **Poznámka:** je důležité vždy naplánovat ` WKSnapshotRefreshBackgroundTask` po obdržel nová data a aktualizovat svoje uživatelské rozhraní aplikace nebo uživatele, se nezobrazí upravené informace.




Kromě toho když uživatel obdrží oznámení z aplikace a klepne na jeho uvedení aplikace do popředí, snímku, je potřeba aktuální vzhledem k tomu, že funguje jako spouštěcí obrazovku:

[ ![](background-tasks-images/update09.png "Uživatel obdrží oznámení z aplikace a klepne na jeho uvedení aplikace do popředí")](background-tasks-images/update09.png)

Pokud již uběhlo více než jednu hodinu vzhledem k tomu, že má uživatel zpracoval watchOS aplikaci, bude moct vrátit do výchozího stavu. Výchozí stav může znamenat různé věci pro různé aplikace a podle návrhu aplikace, pravděpodobně nemá výchozí stav vůbec.

<!--TODO - Possibly link to Apple's Designing Great Apple Watch Experiences video or add our own version here...-->

<a name="WKWatchConnectivityRefreshBackgroundTask" />

### <a name="wkwatchconnectivityrefreshbackgroundtask"></a>WKWatchConnectivityRefreshBackgroundTask

V watchOS 3, Apple má integrované sledování připojení s rozhraním API pozadí aktualizovat prostřednictvím nové `WKWatchConnectivityRefreshBackgroundTask`. Pomocí této nové funkce, aplikaci pro iPhone doručovat čerstvá data k jeho protějšku aplikace sledovat, když watchOS aplikace běží na pozadí:

[ ![](background-tasks-images/update10.png "Aplikaci pro iPhone může poskytnout čerstvá data k jeho protějšku aplikace sledovat, když aplikace watchOS běží na pozadí")](background-tasks-images/update10.png)

Probíhá inicializace komplikace Push, bude aplikace kontextu, odesílání souboru nebo aktualizace informací o uživateli z aplikace iPhone probuzení Apple Watch aplikace na pozadí.

Když je aplikace sledovat probuzený prostřednictvím `WKWatchConnectivityRefreshBackgroundTask` ji budou muset používat standardní metody rozhraní API pro příjem dat z aplikace iPhone.

[ ![](background-tasks-images/update11.png "Tok dat WKWatchConnectivityRefreshBackgroundTask")](background-tasks-images/update11.png)

1. Ujistěte se, že relace aktivoval.
2. Monitorování nové `HasContentPending` , pokud je hodnota vlastnosti `true`, aplikace má stále data ke zpracování. Jako dříve, aplikace by měla obsahovat na úlohu dokud se nedokončí zpracování všech dat.
3. Pokud není žádná další data ke zpracování (`HasContentPending = false`), označte úloha dokončena se vraťte do systému. Nedaří se to udělat vyčerpá aplikace přiděleném pozadí runtime výsledná v sestavě havárií.

<a name="The-Background-API-Lifecycle" />

## <a name="the-background-api-lifecycle"></a>Životní cyklus pozadí rozhraní API

Umístění všech částí do jednoho nového rozhraní API pozadí úlohy dohromady, typickou sadu interakce by vypadat následovně:

[ ![](background-tasks-images/update12.png "Životní cyklus pozadí rozhraní API")](background-tasks-images/update12.png)

1. Nejprve aplikace watchOS naplánuje pozadí úlohy awoke jako některé bod v budoucnu.
2. Aplikace je probuzený v systému a odeslat úlohu.
3. Aplikace zpracovává úlohu dokončit, ať pracovní nebyla nutná.
4. Výsledkem je zpracování úlohy, aplikace může třeba naplánovat další pozadí na dokončení další práci v budoucnu, jako je například stahování více obsahu pomocí úloh `NSUrlSession`.
5. Aplikace označí úloha dokončena a vrátí ji do systému.

<a name="Using-Resources-Responsibly" />

## <a name="using-resources-responsibly"></a>Jeho zodpovědné používání prostředků

Je důležité, aby aplikace watchOS chová jeho zodpovědné v rámci tento ekosystém omezením jeho vyprazdňování na sdílené prostředky systému.

Podívejte se na následující scénář:

[ ![](background-tasks-images/update13.png "Aplikace watchOS omezuje jeho vyprazdňování na sdílené prostředky systému")](background-tasks-images/update13.png)

1. Uživatel spustí aplikaci watchOS: 00: 00.
2. Aplikace naplánuje úlohu k probuzení a stáhněte si nový obsah za hodinu na 2:00 PM.
3. Ve 13:50 znovu uživatel otevře aplikaci, která umožňuje aktualizovat data uživatelského rozhraní a v tuto chvíli.
4. Místo probuzení úloh aplikace znovu za 10 minut, by měla aplikace přeplánovat úloha pro spuštění hodinu později ve 2:50.

Při každé aplikace se liší, Apple navrhuje vyhledávání vzorů využití, jako jsou ty, které jsou uvedené nahoře, šetřit systémové prostředky.

<a name="Implementing-Background-Tasks" />

## <a name="implementing-background-tasks"></a>Implementace úlohy na pozadí

Z důvodu příklad bude tento dokument pomocí falešných aplikace MonkeySoccer sportu, která sestavy fotbalový skóre pro uživatele. 

Podívejte se na následující scénář typickému využití:

[ ![](background-tasks-images/update14.png "Scénář typickému využití")](background-tasks-images/update14.png)

Uživatele Oblíbené fotbalový tým přehrávání big shoda v 19:00:00 do 21:00:00, aplikace by měl očekávat uživateli skóre pravidelně kontrolovat a rozhodne se na intervalu 30 minut aktualizace.

1. Uživatel otevře aplikaci a plány úloh pro aktualizaci pozadí 30 minutách. Rozhraní API pozadí umožňuje pouze jeden typ pozadí úloh, aby byl spuštěn v daném okamžiku.
2. Aplikace obdrží úlohy a aktualizuje jeho data a uživatelského rozhraní a pak plány pro jinou pozadí úloh 30 minutách. Je důležité, že vývojář pamatuje naplánovat jinou pozadí úloh nebo aplikace se nikdy se znovu awoken chcete získat další aktualizace.
3. Znovu aplikace obdrží úlohy a aktualizuje jeho data, aktualizuje jeho uživatelském rozhraní a plány jiné pozadí úloh 30 minutách.
4. Stejný postup se opakuje akci.
5. Poslední pozadí, které úlohu je přijatá a aplikace znovu aktualizací uživatelského rozhraní a jeho data. Vzhledem k tomu, že toto je poslední skóre nemá plán pro nové aktualizace na pozadí. 

<a name="Scheduling-for-Background-Update" />

## <a name="scheduling-for-background-update"></a>Plánování pro aktualizaci pozadí

Zadána výše uvedené scénáře MonkeySoccer aplikace můžete použít následující kód při plánování pro aktualizaci na pozadí:

```csharp
private void ScheduleNextBackgroundUpdate ()
{
    // Create a fire date 30 minutes into the future
    var fireDate = NSDate.FromTimeIntervalSinceNow (30 * 60);

    // Create 
    var userInfo = new NSMutableDictionary ();
    userInfo.Add (new NSString ("LastActiveDate"), NSDate.FromTimeIntervalSinceNow(0));
    userInfo.Add (new NSString ("Reason"), new NSString ("UpdateScore"));

    // Schedule for update
    WKExtension.SharedExtension.ScheduleBackgroundRefresh (fireDate, userInfo, (error) => {
        // Was the Task successfully scheduled?
        if (error == null) {
            // Yes, handle if needed
        } else {
            // No, report error
        }
    });
}
```

Vytvoří novou `NSDate` 30 minut do budoucích Pokud aplikace chce být awoken a vytvoří `NSMutableDictionary` pro uložení podrobnosti požadovanou úlohu. `ScheduleBackgroundRefresh` Metodu `SharedExtension` slouží k vyžádání plánování úkolu.

Vrátí systému `NSError` Pokud se jednalo o nelze naplánovat požadovanou úlohu.

<a name="Processing-the-Update" />

## <a name="processing-the-update"></a>Zpracování aktualizace

Potom přepněte bližší pohled na 5 minut okna s kroky potřebné k aktualizaci skóre:

[ ![](background-tasks-images/update15.png "Okno 5 minut zobrazující kroky potřebné k aktualizaci skóre")](background-tasks-images/update15.png)

1. Na 19:30:02: 00 probudí, systém a aplikace danou aktualizaci pozadí úloh. Jeho první prioritu má získat nejnovější skóre ze serveru. V tématu [plánování NSUrlSession](#Scheduling-a-NSUrlSession) níže.
2. Na 7:30:05 aplikace dokončí původní úlohu, systém převádí aplikace do režimu spánku a nadále stahovat požadovaná data na pozadí.
3. Po dokončení stahování systém vytvoří novou úlohu probuzení aplikace, takže může zpracovat stažené informace. V tématu [zpracování úlohy na pozadí](#Handling-Background-Tasks) a [zpracování dokončení stahování](#Handling-the-Download-Completing) níže. 
4. Aplikace uloží aktualizované informace a označí úloha dokončena. Vývojář může být tendenci aktualizujte uživatelské rozhraní aplikace v tomto okamžiku ale Apple navrhuje plánování úlohu snímku pro zpracování procesu. V tématu [plánování aktualizaci snímku](#Scheduling-a-Snapshot-Update) níže.
5. Aplikace obdrží úlohu snímku, aktualizuje jeho uživatelské rozhraní a označí úloha dokončena. V tématu [zpracování aktualizaci snímku](#Handling-a-Snapshot-Update) níže.

<a name="Scheduling-a-NSUrlSession" />

## <a name="scheduling-a-nsurlsession"></a>Plánování NSUrlSession

Následující kód slouží k plánování stažení nejnovější skóre:

```csharp
private void ScheduleURLUpdateSession ()
{
    // Create new configuration
    var configuration = NSUrlSessionConfiguration.CreateBackgroundSessionConfiguration ("com.example.urlsession");

    // Create new session
    var backgroundSession = NSUrlSession.FromConfiguration (configuration);

    // Create and start download task
    var downloadTask = backgroundSession.CreateDownloadTask (new NSUrl ("https://example.com/gamexxx/currentScores.json"));
    downloadTask.Resume ();
}
```

Se konfiguruje a vytvoří novou `NSUrlSession`, pak používá k vytvoření nové stažení této relaci úloh pomocí `CreateDownloadTask` metoda. Zavolá `Resume` metoda stažení úloha pro spuštění relace.

<a name="Handling-Background-Tasks" />

## <a name="handling-background-tasks"></a>Zpracování úlohy na pozadí

Přepsáním `HandleBackgroundTasks` metodu `WKExtensionDelegate`, aplikace může zpracovávat příchozí úlohy na pozadí:

```csharp
using System;
using System.Collections.Generic;
using Foundation;
using WatchKit;

namespace MonkeySoccer.MonkeySoccerExtension
{
    public class ExtensionDelegate : WKExtensionDelegate
    {
        #region Computed Properties
        public List<WKRefreshBackgroundTask> PendingTasks { get; set; } = new List<WKRefreshBackgroundTask> ();
        #endregion

        ...
        
        #region Public Methods
        public void CompleteTask (WKRefreshBackgroundTask task)
        {
            // Mark the task completed and remove from the collection
            task.SetTaskCompleted ();
            PendingTasks.Remove (task);
        }
        #endregion 

        #region Override Methods
        public override void HandleBackgroundTasks (NSSet<WKRefreshBackgroundTask> backgroundTasks)
        {
            // Handle background request
            foreach (WKRefreshBackgroundTask task in backgroundTasks) {
                // Is this a background session task?
                var urlTask = task as WKUrlSessionRefreshBackgroundTask;
                if (urlTask != null) {
                    // Create new configuration
                    var configuration = NSUrlSessionConfiguration.CreateBackgroundSessionConfiguration (urlTask.SessionIdentifier);

                    // Create new session
                    var backgroundSession = NSUrlSession.FromConfiguration (configuration, new BackgroundSessionDelegate (this, task), null);

                    // Keep track of all pending tasks
                    PendingTasks.Add (task);
                } else {
                    // Ensure that all tasks are completed
                    task.SetTaskCompleted ();
                }
            }
        }
        #endregion
        
        ...
    }
}
```

`HandleBackgroundTasks` Metoda procházení všechny úlohy, které systém odeslala aplikace (v `backgroundTasks`) hledání `WKUrlSessionRefreshBackgroundTask`. Pokud ho najde, znovu připojí ke relace a připojí `NSUrlSessionDownloadDelegate` pro zpracování dokončení stahování (najdete v části [zpracování stáhnout dokončení](#Handling-the-Download-Completing) níže):

```csharp
// Create new session
var backgroundSession = NSUrlSession.FromConfiguration (configuration, new BackgroundSessionDelegate (this, task), null);
```

Popisovač udržuje v úloze až po dokončení jej přidat do kolekce:

```csharp
public List<WKRefreshBackgroundTask> PendingTasks { get; set; } = new List<WKRefreshBackgroundTask> ();
...

// Keep track of all pending tasks
PendingTasks.Add (task);
```

Všechny úlohy, odeslané do aplikace nutné provést pro všechny úlohy, není aktuálně zpracovanou označte ji dokončení:

```csharp
if (urlTask != null) {
    ...
} else {
    // Ensure that all tasks are completed
    task.SetTaskCompleted ();
}
```

<a name="Handling-the-Download-Completing" />

## <a name="handling-the-download-completing"></a>Zpracování dokončení stahování

Aplikace MonkeySoccer používá následující `NSUrlSessionDownloadDelegate` delegáta pro zpracování dokončení stahování a zpracování požadovaná data:

```csharp
using System;
using Foundation;
using WatchKit;

namespace MonkeySoccer.MonkeySoccerExtension
{
    public class BackgroundSessionDelegate : NSUrlSessionDownloadDelegate
    {
        #region Computed Properties
        public ExtensionDelegate WatchExtensionDelegate { get; set; }

        public WKRefreshBackgroundTask Task { get; set; }
        #endregion

        #region Constructors
        public BackgroundSessionDelegate (ExtensionDelegate extensionDelegate, WKRefreshBackgroundTask task)
        {
            // Initialize
            this.WatchExtensionDelegate = extensionDelegate;
            this.Task = task;
        }
        #endregion

        #region Override Methods
        public override void DidFinishDownloading (NSUrlSession session, NSUrlSessionDownloadTask downloadTask, NSUrl location)
        {
            // Handle the downloaded data
            ...

            // Mark the task completed
            WatchExtensionDelegate.CompleteTask (Task);

        }
        #endregion
    }
}
```

Při inicializaci, udržuje popisovač do obou `ExtensionDelegate` a `WKRefreshBackgroundTask` který je vytvořený. Přepíše `DidFinishDownloading` metodu ke zpracování dokončení stahování. Použije `CompleteTask` metodu `ExtensionDelegate` k informování úlohu to bylo dokončeno a jeho odebrání z kolekce úkolů čekajících na zpracování. V tématu [zpracování úlohy na pozadí](#Handling-Background-Tasks) výše.

<a name="Scheduling-a-Snapshot-Update" />

## <a name="scheduling-a-snapshot-update"></a>Plánování aktualizaci snímku

Následující kód slouží k naplánovat úlohu snímku aktualizace uživatelského rozhraní pomocí nejnovější skóre:

```csharp
private void ScheduleSnapshotUpdate ()
{
    // Create a fire date of now
    var fireDate = NSDate.FromTimeIntervalSinceNow (0);

    // Create user info dictionary
    var userInfo = new NSMutableDictionary ();
    userInfo.Add (new NSString ("lastActiveDate"), NSDate.FromTimeIntervalSinceNow (0));
    userInfo.Add (new NSString ("reason"), new NSString ("UpdateScore"));

    // Schedule for update
    WKExtension.SharedExtension.ScheduleSnapshotRefresh (fireDate, userInfo, (error) => {
        // Was the Task successfully scheduled?
        if (error == null) {
            // Yes, handle if needed
        } else {
            // No, report error
        }
    });
}
```

Stejně jako `ScheduleURLUpdateSession` metody popsané výše, vytvoří novou `NSDate` když aplikace chce být awoken a vytvoří `NSMutableDictionary` pro uložení podrobnosti požadovanou úlohu. `ScheduleSnapshotRefresh` Metodu `SharedExtension` slouží k vyžádání plánování úkolu.

Vrátí systému `NSError` Pokud se jednalo o nelze naplánovat požadovanou úlohu.

<a name="Handling-a-Snapshot-Update" />

## <a name="handling-a-snapshot-update"></a>Zpracování aktualizaci snímku

Pro zpracování úlohu snímku `HandleBackgroundTasks` – metoda (najdete v části [zpracování úlohy na pozadí](#Handling-Background-Tasks) výše) je upravit tak, aby vypadat třeba takto:

```csharp
public override void HandleBackgroundTasks (NSSet<WKRefreshBackgroundTask> backgroundTasks)
{
    // Handle background request
    foreach (WKRefreshBackgroundTask task in backgroundTasks) {
        // Take action based on task type
        if (task is WKUrlSessionRefreshBackgroundTask) {
            var urlTask = task as WKUrlSessionRefreshBackgroundTask;

            // Create new configuration
            var configuration = NSUrlSessionConfiguration.CreateBackgroundSessionConfiguration (urlTask.SessionIdentifier);

            // Create new session
            var backgroundSession = NSUrlSession.FromConfiguration (configuration, new BackgroundSessionDelegate (this, task), null);

            // Keep track of all pending tasks
            PendingTasks.Add (task);
        } else if (task is WKSnapshotRefreshBackgroundTask) {
            var snapshotTask = task as WKSnapshotRefreshBackgroundTask;

            // Update UI
            ...

            // Create a expiration date 30 minutes into the future
            var expirationDate = NSDate.FromTimeIntervalSinceNow (30 * 60);

            // Create user info dictionary
            var userInfo = new NSMutableDictionary ();
            userInfo.Add (new NSString ("lastActiveDate"), NSDate.FromTimeIntervalSinceNow (0));
            userInfo.Add (new NSString ("reason"), new NSString ("UpdateScore"));

            // Mark task complete
            snapshotTask.SetTaskCompleted (false, expirationDate, userInfo);
        } else {
            // Ensure that all tasks are completed
            task.SetTaskCompleted ();
        }
    }
}
```

Metoda testů pro typ úlohy, na které se právě zpracovává. Pokud je `WKSnapshotRefreshBackgroundTask` ho získá přístup k úlohy:

```csharp
var snapshotTask = task as WKSnapshotRefreshBackgroundTask;
```

Metoda aktualizace uživatelského rozhraní a vytvoří `NSDate` říct systému, když bude snímek zastaralé. Vytvoří `NSMutableDictionary` s informace o uživateli k popisu nový snímek a označí úlohu snímku byla dokončena s tyto informace:

```csharp
// Mark task complete
snapshotTask.SetTaskCompleted (false, expirationDate, userInfo);
```

Kromě toho také obsahuje informace pro úlohu snímku, není aplikace vrátí do výchozího stavu (v první parametr). Aplikace, které nemají žádný koncept do výchozího stavu musí tato vlastnost vždy nastavená na `true`.

<a name="Working-Efficiently" />

## <a name="working-efficiently"></a>Efektivní práci

Jak je vidět v předchozím příkladu okna pět minut, které aplikace MonkeySoccer trvalo aktualizovat jeho skóre efektivní práci a použitím nové watchOS 3 úlohy na pozadí, aplikace byla pouze aktivní celkem 15 sekund: 

[ ![](background-tasks-images/update16.png "Aplikace byla pouze aktivní celkem 15 sekund")](background-tasks-images/update16.png)

To snižuje vliv, která aplikace bude mít na dostupné prostředky Apple Watch i výdrž baterie a rovněž umožňuje aplikaci lépe pracovat s jinými aplikacemi sledování a systémem.

<a name="How-Scheduling-Works" />

## <a name="how-scheduling-works"></a>Jak funguje plánování

Při aplikaci watchOS 3 je v popředí, je vždy naplánovat na spuštění a můžete provést jakýkoli typ zpracování požadované například aktualizace dat nebo ho překreslit jeho uživatelském rozhraní. Pokud se aplikace přesune do na pozadí, je obvykle pozastaveno systémem a jsou zastavit všechny operace modulu runtime. 

Při aplikaci na pozadí se může stát terčem systému rychle spustit konkrétní úlohu. Ano v watchOS 2, systém dočasně probuzení aplikace na pozadí, provádět akce, jako je zpracování oznámení dlouhou vzhled a aktualizace komplikace aplikace. V watchOS 3 existuje několik způsobů nové, které aplikace můžou běžet v pozadí.

Když aplikaci právě na pozadí, systém ukládá několik omezení:

- Ji dostane jenom pár sekund dokončete jakékoli dané úlohy. Systém vezme v úvahu jenom množství času předán, ale také kolik výkonu procesoru aplikace spotřebovává odvození toto omezení.
- Jakékoli aplikaci, která překračuje omezení budou ukončeny s následující kódy chyb:
    - **CPU** - 0xc51bad01
    - **Time** - 0xc51bad02
- Systém bude uložit různé omezení na základě typu požádal aplikaci k provedení úlohy na pozadí. Například `WKApplicationRefreshBackgroundTask` a `WKURLSessionRefreshBackgroundTask` úlohy jsou uvedeny poněkud déle runtimes přes jiné typy úloh na pozadí.

<a name="Complications-and-App-Updates" />

### <a name="complications-and-app-updates"></a>Komplikace a aktualizace aplikací

Kromě nové úlohy na pozadí, Apple přidala k watchOS 3 komplikace watchOS aplikace může mít vliv na tom, jak a kdy aplikace obdrží aktualizace na pozadí.

Komplikace jsou malé vizuální prvky, které poskytují užitečné informace na první pohled. V závislosti na tučné sledovat vybrána uživatel má schopnost přizpůsobit vzhled sledovat s jeden nebo více komplikace, které může být zadáno sledování aplikací v watchOS 3.

Pokud uživatel obsahuje jeden z komplikace aplikace na jejich vzhled sledovat, umožňuje aplikaci aktualizovat následující výhody:

- Způsobí, že systém mějte aplikace připravené k launch stavu, kde se pokusí spustit aplikaci na pozadí, uloží je paměť a poskytuje velmi je čas aktualizovat.
- Komplikace jsou zaručit alespoň 50 nabízené aktualizace za den.

Vývojář musí vždy snažit vytvořit poutavé komplikace pro svoje aplikace k přesvědčit uživatele do jejich přidáním do jejich vzhled sledovat z důvodů uvedených výše.

V watchOS 2 byly komplikace primární způsob přijímání aplikace za běhu v pozadí. V watchOS 3, bude aplikace komplikace stále zajištěn a získat více aktualizace za hodinu, ale můžete použít `WKExtensions` požádat o další runtime aktualizovat jeho komplikace.

Podívejte se na následující kód používá k aktualizaci komplikace z připojených zařízení iPhone aplikace:

```csharp
using System;
using WatchConnectivity;
using UIKit;
using Foundation;
using System.Collections.Generic;
using System.Linq;
...

private void UpdateComplication ()
{

    // Get session and the number of remaining transfers
    var session = WCSession.DefaultSession;
    var transfers = session.RemainingComplicationUserInfoTransfers;

    // Create user info dictionary
    var iconattrs = new Dictionary<NSString, NSObject>
        {
            {new NSString ("lastActiveDate"), NSDate.FromTimeIntervalSinceNow (0)},
            {new NSString ("reason"), new NSString ("UpdateScore")}
        };

    var userInfo = NSDictionary<NSString, NSObject>.FromObjectsAndKeys (iconattrs.Values.ToArray (), iconattrs.Keys.ToArray ());

    // Take action based on the number of transfers left
    if (transfers < 1) {
        // No transfers left, either attempt to send or inform
        // user of situation.
        ...
    } else if (transfers < 11) {
        // Running low on transfers, only send on important updates
        // else conserve for a significant change.
        ...
    } else {
        // Send data
        session.TransferCurrentComplicationUserInfo (userInfo);
    }
}
```

Použije `RemainingComplicationUserInfoTransfers` vlastnost `WCSession` chcete zobrazit, kolik s vysokou prioritou přenáší aplikace opustil den a potom klepněte na akce trvá v závislosti na toto číslo. Pokud aplikace začne nedostatek na přenosy, může zdržovat odesílání dílčími aktualizacemi a pouze odeslat informace po významné změny.

<a name="Scheduling-and-Dock" />

### <a name="scheduling-and-the-dock"></a>Plánování a ukotvení

V watchOS 3 byl přidán Apple ukotvení, kde mohou uživatelé připnout své oblíbené aplikace a rychle přistupovat k nim. Když uživatel stiskne tlačítko straně na Apple Watch, zobrazí se Galerie definovaného aplikace snímků. Uživatel může prstem doleva nebo doprava najít požadovanou aplikaci a pak klepněte na aplikaci spustíte nahraďte snímek spuštěné aplikaci rozhraní.

[ ![](background-tasks-images/dock01.png "Ukotvení")](background-tasks-images/dock01.png)

Systém pravidelně trvá snímky uživatelském rozhraní aplikace a používá tyto snímky k vyplnění dokumentaci. watchOS poskytuje aplikaci příležitost k aktualizaci uživatelského rozhraní a obsahu předtím, než se provede tento snímek.

Aplikace, které mají připnutá na ukotvení můžete očekávat následující:

- Uživatelé obdrží alespoň jednu aktualizovat za hodinu. To zahrnuje aplikaci aktualizovat úloh a úloh snímku.
- Aktualizace rozpočtu se distribuuje mezi všechny aplikace v ukotvení. Proto méně aplikace, které uživatel má připnutý, více potenciální aktualizace, které se zobrazí každou aplikaci.
- Aplikace budou zachovány v paměti, takže aplikace bude obnovena rychle, když vyberete ze ukotvení.

Poslední aplikaci spustili uživatele bude považovat za _nedávno použité_ aplikace a bude zabírat poslední slot v ukotvení. Zde, že uživatel trvale připnout na ukotvení. Naposledy použitých bude zacházeno, jako kterákoli jiná oblíbených položek aplikace uživatel má již připnuli k ukotvení.

> [!IMPORTANT]
> **Poznámka:** aplikace, které byly přidány pouze k obrazovce Domů nebude mít regulární plánování. Pro příjem regulární plánování a pozadí aktualizací, aplikace _musí_ přidat do ukotvení.

Jak jsme uvedli dříve v tomto dokumentu, snímky jsou velmi důležité pro watchOS 3 vzhledem k tomu, že fungovat jako obrázky verzi preview a spuštění aplikace. Pokud uživatel vyrovná v aplikaci v ukotvení, bude rozšíření na celou obrazovku, zadejte popředí a spustit, takže je nutné, že se snímek aktuální.

Může nastat situace, kdy systému rozhodne, které že potřebuje nový snímek uživatelském rozhraní aplikace. V této situace snímku požadavek nebude započítává nároky za běhu aplikace. Žádost o snímku systému aktivují:

- Aktualizaci s komplikace časové osy.
- Interakce uživatele s oznámení aplikaci.
- Přepnutí z aktivní na pozadí stavu.
- Po jedné hodině způsobená v pozadí stavu takže aplikace může vrátit do výchozího stavu.
- Při prvním spuštění watchOS.

<a name="Best-Practices" />

## <a name="best-practices"></a>Doporučené postupy 

Apple navrhuje následující osvědčené postupy při práci s úkoly na pozadí:

- Naplánujte tak často, jak aplikaci je třeba aktualizovat. Pokaždé, když aplikace běží, měl by znovu vyhodnotit jeho budoucích potřeb a upravit tento plán, podle potřeby.
- Pokud systém odešle úlohy na pozadí aktualizace a aplikace nevyžaduje aktualizace, odložení práce, dokud je ve skutečnosti nutná aktualizace.
- Vezměte v úvahu všechny runtime možností, které pro aplikaci:
    - Ukotvení a popředí aktivace.
    - Oznámení.
    - Komplikace aktualizace.
    - Aktualizace na pozadí.
- Použití `ScheduleBackgroundRefresh` pro modul runtime pro obecné účely pozadí jako:
    - Dotazování systémové informace.
    - Naplánovat budoucí `NSURLSessions` k žádosti o data na pozadí. 
    - Označuje čas přechody.
    - Spouštěcí komplikace aktualizace.

<a name="Snapshot-Best-Practices" />

## <a name="snapshot-best-practices"></a>Osvědčené postupy snímku

Při práci s aktualizací snímků, Apple umožňuje následující návrhy:

- Zrušení platnosti snímky jenom v případě, že je potřeba, například po významné změny obsahu.
- Vyhněte se zneplatnění vysoká frekvence snímků. Například časovače aplikace by neměla aktualizovat snímek za sekundu, ho lze provádět pouze když časovač skončila.

<a name="App-Data-Flow" />

## <a name="app-data-flow"></a>Tok dat aplikací

Apple návrhu pro práci s tok dat následující:

[ ![](background-tasks-images/update17.png "Diagram toku dat aplikací")](background-tasks-images/update17.png)

Událost externí (například sledovat připojením) se probudí aplikace. Vynutí se tak aplikaci aktualizovat jeho datového modelu (představující aktuální stav aplikace). V důsledku změny datový Model aplikace bude nutné aktualizovat jeho komplikace požádat o nový snímek, případně spustit na pozadí `NSURLSession` vyžádá další data a naplánovat další pozadí aktualizuje.

<a name="The-App-Lifecycle" />

## <a name="the-app-lifecycle"></a>Životní cyklus aplikace

Z důvodu ukotvení a možnost připnout oblíbených položek aplikace k němu Apple se domnívá, které uživatelé budou přesun mezi podstatně více aplikací, mnohem víc často, pak se nebyla s watchOS 2. Aplikace musí být v důsledku toho připravena ke zpracování této změny a rychle přesouvat mezi stavy popředí a na pozadí.

Společnost Apple má následující návrhy:

- Ujistěte se, že aplikace dokončí všechny úlohy na pozadí co nejdříve při vstupu popředí aktivace.
- Ujistěte se, k dokončení veškeré práce popředí před vstupem na pozadí při volání `NSProcessInfo.PerformExpiringActivity`.
- Při testování aplikaci v simulátoru watchOS, žádné rozpočty úloh bude vynuceno, takže aplikace můžete obnovit tolik, kolik potřebné k testování funkce.
- Vždy otestujte na skutečné Apple Watch hardware a zkontrolujte, zda není aplikace spuštěna po jeho rozpočty před publikováním pro službu iTunes připojit.
- Apple navrhuje zachování Apple Watch na nabíječce při testování a ladění.
- Ujistěte se, důkladně otestovat cold spuštění i obnovení aplikace.
- Ověřte, že všechny úlohy aplikace jsou je dokončena.
- Obměny počtu aplikací, které jsou připnuté v ukotvení k testování nejlepší a nejhorší případ scénáře.

<a name="Summary" />

## <a name="summary"></a>Souhrn

Tento článek má zahrnutých vylepšení provedena Apple watchOS a jak můžete použít k zachování aktualizovaného stavu sledování aplikace. Nejprve zahrnuté všechny nové pozadí úloh Apple přidala v watchOS 3. Potom zahrnutých životní cyklus pozadí rozhraní API a jak v aplikaci Xamarin watchOS implementovat úlohy na pozadí. Nakonec popsaná funguje jak plánování a Dal některé osvědčené postupy.



## <a name="related-links"></a>Související odkazy

- [iOS 10 – ukázky](https://developer.xamarin.com/samples/ios/iOS10/)
