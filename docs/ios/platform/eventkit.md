---
title: EventKit
description: "Tato příručka poskytuje přehled o tom, jak přístupu a práce s kalendáři, CalendarEvents a připomenutí data uložená v databázi, kalendáře jako zveřejňovány prostřednictvím EventKit. Pokrývá hlavní třídy a jejich role v EventKit programování, jakož i počet běžné úlohy spojené s EventKit framework."
ms.topic: article
ms.prod: xamarin
ms.assetid: 00E88629-357D-1FCD-4FCE-1330D5D9D32C
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/19/2017
ms.openlocfilehash: a08bc67a9af653a9a646ad62071df0400ce58c12
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 03/09/2018
---
# <a name="eventkit"></a>EventKit

_Tato příručka poskytuje přehled o tom, jak přístupu a práce s kalendáři, CalendarEvents a připomenutí data uložená v databázi, kalendáře jako zveřejňovány prostřednictvím EventKit. Pokrývá hlavní třídy a jejich role v EventKit programování, jakož i počet běžné úlohy spojené s EventKit framework._

iOS obsahuje dvě předdefinované související kalendáře aplikace: kalendáře aplikace a aplikace připomenutí. Je dostatečně jasné, abyste pochopili, jak aplikace kalendáře spravuje kalendářní data, ale je méně zřejmé připomenutí v aplikaci. Připomenutí ve skutečnosti může mít data související s nimi z hlediska, pokud jsou z důvodu, když jste dokončeny, atd. Jako takový iOS ukládá všechna data kalendáře, zda byl kalendář události nebo připomenutí v jednom místě, volá se *kalendáře databáze*.

Rozhraní EventKit poskytuje způsob, jak získat přístup *kalendáře*, *kalendář události*, a *připomenutí* data, která ukládá kalendář databáze. Přístup do kalendáře a kalendář události byl k dispozici od sady iOS 4, ale přístup k připomenutí je nového v iOS 6.

V této příručce vytvoříme zahrnují:

-   **Základy EventKit** – to zavedete základní údaje EventKit prostřednictvím hlavní třídy a poskytuje představu o jejich využití. Tato část se vyžaduje čtení před boji se další části dokumentu. 
-   **Běžné úlohy** – části společné úkoly má být rychlý přehled o tom, jak provádět běžné akce, jako například; vytváření výčtu kalendáře, vytváření, ukládání a načítání kalendář události a připomenutí, a také pomocí předdefinovaných řadičů pro Vytvoření a úprava kalendář události. V této části nemusí číst vpředu vzadu, měl má představovat odkaz pro konkrétní úlohy. 


Všechny úlohy v této příručce jsou k dispozici v doprovodné ukázkovou aplikaci:

 [![](eventkit-images/01.png "Obrazovky doprovodné ukázkové aplikace")](eventkit-images/01.png#lightbox)

## <a name="requirements"></a>Požadavky

EventKit byla zavedena v systému iOS 4.0, ale přístup k datům připomenutí byla zavedena v systému iOS 6.0. Jako takový uděláte obecné EventKit vývoj, budete muset cíle minimálně verze 4.0 a 6.0 pro připomenutí.

Kromě toho není k dispozici v simulátoru, což znamená, že připomenutí také nebudou k dispozici data, pokud je nepřidáte nejprve připomenutí v aplikaci. Kromě toho požadavky na přístup se zobrazují pouze pro uživatele na skutečné zařízení. Jako takový vývoj EventKit nejlépe otestovali na zařízení.

## <a name="event-kit-basics"></a>Základy Kit událostí

Při práci s EventKit, je důležité mít, pochopit běžné třídy a jejich využití. Všechny tyto třídy lze nalézt v `EventKit` a `EventKitUI` (pro `EKEventEditController`).

### <a name="eventstore"></a>EventStore

*EventStore* třída je nejdůležitější – třída v EventKit, protože je potřeba provádět žádné operace v EventKit. Ho můžete představit jako trvalé úložiště nebo databázový stroj pro všechna data EventKit. Z `EventStore` máte přístup ke kalendářům i kalendář události do kalendáře aplikace a také připomenutí v aplikaci připomenutí.

Protože `EventStore` je jako databázový stroj, by měla být dlohotrvající, což znamená, že by měl vytvořen a zničen co nejméně po dobu životnosti instanci aplikace. Ve skutečnosti, které se doporučuje po vytvoření jednu instanci `EventStore` v aplikaci, byste mít tento odkaz přibližně po celou dobu života aplikace, pokud si nejste jisti, nebude nutné ho znovu. Kromě toho by měla všechna volání přejděte do jednoho `EventStore` instance. Z tohoto důvodu se doporučuje vzoru jednotlivý prvek pro zachování jediné instance, která je k dispozici.

#### <a name="creating-an-event-store"></a>Vytváření úložišti událostí

Následující kód ukazuje účinný způsob, jak vytvořit jednu instanci `EventStore` třídy a nastavit jej jako dostupné staticky v rámci aplikace:

```csharp
public class App
{
    public static App Current {
            get { return current; }
    }
    private static App current;

    public EKEventStore EventStore {
            get { return eventStore; }
    }
    protected EKEventStore eventStore;

    static App ()
    {
            current = new App();
    }
    protected App () 
    {
            eventStore = new EKEventStore ( );
    }
}
```

Výše uvedený kód používá k vytvoření instance instanci typu Singleton vzor `EventStore` při načtení aplikace. `EventStore` Lze přistupovat globálně z v aplikaci takto:

```csharp
App.Current.EventStore;
```

Všechny příklady sem použít tento vzor, že odkaz `EventStore` prostřednictvím `App.Current.EventStore`.

#### <a name="requesting-access-to-calendar-and-reminder-data"></a>Žádají o přístup do kalendáře a připomenutí dat

Dříve, než mohou přístup k žádným datům prostřednictvím EventStore, musíte aplikace nejdřív požádat o přístup na data událostí kalendáře nebo připomenutí dat, podle toho, který je nutné. Pro usnadnění tohoto `EventStore` poskytuje metodu s názvem `RequestAccess` který – při volání – zobrazení výstrah se zobrazí uživateli zprávu, že aplikace je žádají o přístup na buď kalendářní data nebo data připomenutí, v závislosti na tom, které `EKEntityType`je do ní předán. Protože vyvolá zobrazení výstrah, je asynchronní volání a bude volat obslužná rutina dokončení předány jako `NSAction` (nebo Lambda) k němu, který se zobrazí dva parametry; logickou hodnotu o tom, zda byl udělen přístup a `NSError`, která, pokud bude not null obsahovat veškeré informace o chybě v požadavku. Například následující programový bude požadovat přístup k datům událostí kalendáře a zobrazit výstrahu zobrazit, pokud požadavek nebyl udělen.

```csharp
App.Current.EventStore.RequestAccess (EKEntityType.Event, 
    (bool granted, NSError e) => {
            if (granted)
                    //do something here
            else
                    new UIAlertView ( "Access Denied", 
"User Denied Access to Calendar Data", null,
"ok", null).Show ();
            } );
```

Jakmile žádost byla udělena, je budou mít na paměti, dokud, aplikace je nainstalována na zařízení a nebude překryvné výstrahu uživateli.
Přístup je však uveden pouze k typ prostředku, kalendář události nebo připomenutí udělena. Pokud aplikace potřebuje přístup k obě, musí žádosti obě.

Protože je zapamatovaných oprávnění, je relativně levný k vytvoření žádosti pokaždé, když, takže je vhodné před provedením operace vždy požádat o přístup.

Kromě toho, protože obslužná rutina dokončení se volá na vlákno samostatné (bez uživatelského rozhraní), všechny aktualizace uživatelského rozhraní v obslužné rutině dokončení by měla být volána prostřednictvím `InvokeOnMainThread`, v opačném případě bude vyvolána výjimka, a pokud nebyla zachycena, dojde k chybě aplikace.

### <a name="ekentitytype"></a>EKEntityType

`EKEntityType` je výčet, který popisuje typ `EventKit` položky nebo data. Má dvě hodnoty: `Event` a připomenutí. Používá se v několik metod, včetně `EventStore.RequestAccess` říct `EventKit` jaký typ dat získat přístup k nebo načíst.

### <a name="ekcalendar"></a>EKCalendar

 *EKCalendar* představuje kalendář, který obsahuje skupinu kalendář události. Kalendáře mohou být uloženy ve spoustu různých místech, jako například místně, v *Icloudu*v 3. stran, jako umístění poskytovatele *systému Exchange Server* nebo *Google*atd. Kolikrát `EKCalendar` pozná, `EventKit` kde hledat události nebo místa k uložení je.

### <a name="ekeventeditcontroller"></a>EKEventEditController

 *EKEventEditController* lze nalézt v `EventKitUI` obor názvů a je předdefinovaný řadič, který slouží k úpravě nebo vytvoření události kalendáře. Podobně jako v fotoaparát řadiče předdefinovaných `EKEventEditController` nemá lifting těžký můžete v zobrazení uživatelského rozhraní a zpracování, ukládání.

### <a name="ekevent"></a>EKEvent

 *EKEvent* představuje události kalendáře. Obě `EKEvent` a `EKReminder` dědí `EKCalendarItem` a mít pole jako `Title`, `Notes`a tak dále.

### <a name="ekreminder"></a>EKReminder

 *EKReminder* představuje položku připomenutí.

### <a name="ekspan"></a>EKSpan

*EKSpan* je výčet, který popisuje rozpětí události při úpravě události, které můžete opakovat a má dvě hodnoty: *ThisEvent* a *FutureEvents*. `ThisEvent` znamená, že všechny změny se vztahuje pouze na konkrétní událost v řadě, který je odkazováno, zatímco `FutureEvents` bude mít vliv na tuto událost a všechny budoucí opakování.

## <a name="tasks"></a>Úlohy

Pro snadné použití má byla EventKit využití rozdělena na běžné úkoly, které jsou popsané v následujících částech.

### <a name="enumerate-calendars"></a>Zobrazení výčtu kalendáře

Chcete-li vytvořit výčet kalendářích, které uživatel byl nakonfigurován na zařízení, volejte `GetCalendars` na `EventStore` a předá typ kalendářů (připomenutí nebo události), které je třeba přijmout:

```csharp
EKCalendar[] calendars = 
App.Current.EventStore.GetCalendars ( EKEntityType.Event );
```

### <a name="add-or-modify-an-event-using-the-built-in-controller"></a>Přidat nebo upravit událost pomocí předdefinovaných řadiče

*EKEventEditViewController* nemá mnoho lifting těžký pro vás, pokud chcete vytvořit nebo upravit událost se stejné uživatelské rozhraní, které se zobrazí uživatelům při používání aplikace pro kalendáře:

 [![](eventkit-images/02.png "Uživatelské rozhraní, které se zobrazí uživatelům při používání aplikace pro kalendář")](eventkit-images/02.png#lightbox)

Pokud chcete použít, budete chtít deklarovat jako proměnnou a úrovni třídy tak, aby ho nebude získat uvolňování paměti by byl deklarován v rámci metody:

```csharp
public class HomeController : DialogViewController
{
        protected CreateEventEditViewDelegate eventControllerDelegate;
        ...
}
```

Potom spustíte: vytvoří instanci, poskytněte odkaz na `EventStore`, propojit až *EKEventEditViewDelegate* delegovat do něj a potom zobrazit pomocí `PresentViewController`:

```csharp
EventKitUI.EKEventEditViewController eventController = 
        new EventKitUI.EKEventEditViewController ();

// set the controller's event store - it needs to know where/how to save the event
eventController.EventStore = App.Current.EventStore;

// wire up a delegate to handle events from the controller
eventControllerDelegate = new CreateEventEditViewDelegate ( eventController );
eventController.EditViewDelegate = eventControllerDelegate;

// show the event controller
PresentViewController ( eventController, true, null );
```

Volitelně Pokud chcete k předběžnému naplnění události, můžete vytvořit zcela novou událost (jak je znázorněno níže) nebo můžete načíst uložené událostí:

```csharp
EKEvent newEvent = EKEvent.FromStore ( App.Current.EventStore );
// set the alarm for 10 minutes from now
newEvent.AddAlarm ( EKAlarm.FromDate ( DateTime.Now.AddMinutes ( 10 ) ) );
// make the event start 20 minutes from now and last 30 minutes
newEvent.StartDate = DateTime.Now.AddMinutes ( 20 );
newEvent.EndDate = DateTime.Now.AddMinutes ( 50 );
newEvent.Title = "Get outside and exercise!";
newEvent.Notes = "This is your reminder to go and exercise for 30 minutes.”;
```

Pokud chcete k předběžnému naplnění uživatelského rozhraní, nezapomeňte nastavit vlastnost událostí na řadiči:

```csharp
eventController.Event = newEvent;
```

Použít existující událost, najdete v článku *načíst události podle ID* později na části.

Delegát by měly přepsat `Completed` metoda, která se nazývá adaptérem po dokončení se uživatel s dialogové okno:

```csharp
protected class CreateEventEditViewDelegate : EventKitUI.EKEventEditViewDelegate
{
        // we need to keep a reference to the controller so we can dismiss it
        protected EventKitUI.EKEventEditViewController eventController;

        public CreateEventEditViewDelegate (EventKitUI.EKEventEditViewController eventController)
        {
                // save our controller reference
                this.eventController = eventController;
        }

        // completed is called when a user eith
        public override void Completed (EventKitUI.EKEventEditViewController controller, EKEventEditViewAction action)
        {
                eventController.DismissViewController (true, null);
                }
        }
}
```

Volitelně můžete v delegáta, můžete zkontrolovat *akce* v `Completed` metodou upravit událost a znovu uložit, nebo pokračovat v práci, pokud bylo zrušeno, etcetera:

```csharp
public override void Completed (EventKitUI.EKEventEditViewController controller, EKEventEditViewAction action)
{
        eventController.DismissViewController (true, null);

        switch ( action ) {

        case EKEventEditViewAction.Canceled:
                break;
        case EKEventEditViewAction.Deleted:
                break;
        case EKEventEditViewAction.Saved:
                // if you wanted to modify the event you could do so here,
// and then save:
                //App.Current.EventStore.SaveEvent ( controller.Event, )
                break;
        }
}
```

### <a name="creating-an-event-programmatically"></a>Vytváření událostí prostřednictvím kódu programu

Chcete-li vytvořit událost v kódu, použijte *FromStore* metoda factory na `EKEvent` třídy a u něho nastavený žádná data:

```csharp
EKEvent newEvent = EKEvent.FromStore ( App.Current.EventStore );
// set the alarm for 10 minutes from now
newEvent.AddAlarm ( EKAlarm.FromDate ( DateTime.Now.AddMinutes ( 10 ) ) );
// make the event start 20 minutes from now and last 30 minutes
newEvent.StartDate = DateTime.Now.AddMinutes ( 20 );
newEvent.EndDate = DateTime.Now.AddMinutes ( 50 );
newEvent.Title = "Get outside and do some exercise!";
newEvent.Notes = "This is your motivational event to go and do 30 minutes of exercise. Super important. Do this.";
```

Je nutné nastavit kalendář, který chcete události uloží do, ale pokud máte není nastavena žádná předvolba, můžete použít výchozí nastavení:

```csharp
newEvent.Calendar = App.Current.EventStore.DefaultCalendarForNewEvents;
```

Chcete-li uložit události, zavolejte *SaveEvent* metodu `EventStore`:

```csharp
NSError e;
App.Current.EventStore.SaveEvent ( newEvent, EKSpan.ThisEvent, out e );
```

Po uložení, *EventIdentifier* vlastnost aktualizují na jedinečný identifikátor, který lze později načíst události:

```csharp
Console.WriteLine ("Event Saved, ID: " + newEvent.CalendarItemIdentifier);
```

 `EventIdentifier` je že řetězec formátu GUID.

### <a name="create-a-reminder-programmatically"></a>Vytvoření připomenutí prostřednictvím kódu programu

Vytvoření připomenutí v kódu je podobné jako vytváření kalendář události:

```csharp
EKReminder reminder = EKReminder.Create ( App.Current.EventStore );
reminder.Title = "Do something awesome!";
reminder.Calendar = App.Current.EventStore.DefaultCalendarForNewReminders;
```

Chcete-li uložit, volejte *SaveReminder* metodu `EventStore`:

```csharp
NSError e;
App.Current.EventStore.SaveReminder ( reminder, true, out e );
```

### <a name="retrieving-an-event-by-id"></a>Načítání událost s ID

Pro získání událost podle jeho ID, pomocí *EventFromIdentifier* metodu `EventStore` a předejte ji `EventIdentifier` , byla vyžádána z události:

```csharp
EKEvent mySavedEvent = App.Current.EventStore.EventFromIdentifier ( newEvent.EventIdentifier );
```

Pro události, je jsou dva další identifikátor vlastnosti, ale `EventIdentifier` je jenom jeden, který je použitelný pro tento.

### <a name="retrieving-a-reminder-by-id"></a>Načítání připomenutí podle ID

Pro načtení připomenutí, použijte *GetCalendarItem* metodu `EventStore` a předejte ji *CalendarItemIdentifier*:

```csharp
EKCalendarItem myReminder = App.Current.EventStore.GetCalendarItem ( reminder.CalendarItemIdentifier );
```

Protože `GetCalendarItem` vrátí `EKCalendarItem`, musí být přetypovat `EKReminder` Pokud potřebujete přístup k datům připomenutí nebo použít instanci jako `EKReminder` později.

Nepoužívejte `GetCalendarItem` pro kalendář události, jako v době psaní, nefunguje.

### <a name="deleting-an-event"></a>Odstranění události

Chcete-li odstranit událost kalendáře, volejte *RemoveEvent* na vaše `EventStore` a předejte odkaz na události a odpovídající `EKSpan`:

```csharp
NSError e;
App.Current.EventStore.RemoveEvent ( mySavedEvent, EKSpan.ThisEvent, true, out e);
```

Všimněte si však po událost byla odstraněna, odkaz na událostí bude `null`.

### <a name="deleting-a-reminder"></a>Odstranění připomenutí

Chcete-li odstranit připomenutí, volejte *RemoveReminder* na `EventStore` a předejte odkaz na připomenutí:

```csharp
NSError e;
App.Current.EventStore.RemoveReminder ( myReminder as EKReminder, true, out e);
```

Všimněte si, že ve výše uvedeném kódu je přetypování na `EKReminder`, protože `GetCalendarItem` byla použita k jejich načtení

### <a name="searching-for-events"></a>Hledání událostí

Vyhledat události kalendáře, musíte vytvořit *NSPredicate* objektu prostřednictvím *PredicateForEvents* metodu `EventStore`. `NSPredicate` Je dotazu na datový objekt, že iOS se používá k vyhledání shody:

```csharp
DateTime startDate = DateTime.Now.AddDays ( -7 );
DateTime endDate = DateTime.Now;
// the third parameter is calendars we want to look in, to use all calendars, we pass null
NSPredicate query = App.Current.EventStore.PredicateForEvents ( startDate, endDate, null );
```

Po vytvoření `NSPredicate`, použijte *EventsMatching* metodu `EventStore`:

```csharp
// execute the query
EKCalendarItem[] events = App.Current.EventStore.EventsMatching ( query );
```

Všimněte si, že dotazy jsou synchronní (blokování) a může trvat dlouhou dobu, v závislosti na dotazu, takže možná budete chtít číselníku nové vlákno nebo úkolů, aby to udělal.

### <a name="searching-for-reminders"></a>Hledání připomenutí

Hledání připomenutí je podobná události; vyžaduje predikátu, ale volání již asynchronní, takže nemusí starat o blokování vlákno:

```csharp
// create our NSPredicate which we'll use for the query
NSPredicate query = App.Current.EventStore.PredicateForReminders ( null );

// execute the query
App.Current.EventStore.FetchReminders (
        query, ( EKReminder[] items ) => {
                // do someting with the items
        } );
```

## <a name="summary"></a>Souhrn

Tento dokument jste dali Přehled důležité součásti rozhraní EventKit a počet nejčastějších úloh. Rozhraní EventKit je však velmi velké a výkonné a obsahuje funkce, které nebyly zavedený tady, jako například: batch aktualizací, konfigurace nastavení upozornění, konfigurace opakování pro události, registrace a sleduje změny v databázi, kalendáře nastavení monitorovaná geografická zóna a další.  Další informace najdete v článku společnosti Apple [kalendář a Průvodce programováním připomenutí](https://developer.apple.com/library/prerelease/ios/#documentation/DataManagement/Conceptual/EventKitProgGuide/Introduction/Introduction.html).


## <a name="related-links"></a>Související odkazy

- [Kalendáře (ukázka)](https://developer.xamarin.com/samples/monotouch/Calendars/)
- [Úvod do iOSu 6](~/ios/platform/introduction-to-ios6/index.md)
- [Úvod do kalendáře a připomenutí](https://developer.apple.com/library/prerelease/ios/#documentation/DataManagement/Conceptual/EventKitProgGuide/Introduction/Introduction.html)
