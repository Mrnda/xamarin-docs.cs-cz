---
title: "Cvičení aplikace"
description: "Tento článek se zabývá rozšířením Apple provedl cvičení aplikací ve watchOS 3 a jejich implementaci v Xamarin."
ms.topic: article
ms.prod: xamarin
ms.assetid: F1D19635-A738-43E5-9873-1FC1BA44EEDF
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/17/2017
ms.openlocfilehash: 77bad4c31ad0cb11476c656aa495707d2a94aa8f
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 03/09/2018
---
# <a name="workout-apps"></a>Cvičení aplikace

_Tento článek se zabývá rozšířením Apple provedl cvičení aplikací ve watchOS 3 a jejich implementaci v Xamarin._


Nové k watchOS 3 cvičení související s aplikací mít možnost spustit na pozadí na Apple Watch a získat přístup k datům HealthKit. Aplikace pro iOS 10 na základě jejich nadřazené má také možnost spustit aplikaci watchOS 3 na základě bez zásahu uživatele.

Podrobně se budeme v následujících tématech:

## <a name="about-workout-apps"></a>O aplikacích cvičení

Uživatelé vhodnosti a cvičení aplikací může být vysoce vyhrazené devoting několik hodin dne směrem k cílům jejich stavu a vhodnosti. V důsledku toho očekávané reakce, snadno použitelné aplikace, které přesně shromažďovat a zobrazovat data a bezproblémově integrovat Apple stavu.

Dobře navrženým vhodnosti nebo cvičení aplikace pomáhá uživatelům grafu jejich aktivity k dosažení jejich vhodnosti cíle. Pomocí Apple Watch vhodnosti a cvičení aplikací mít okamžitý přístup na rychlost vysílat Kalorie zápis na disk a aktivitu zjištění.

[![](workout-apps-images/workout01.png "Vhodnosti a cvičení příklad aplikace")](workout-apps-images/workout01.png#lightbox)

Nový watchOS 3, _pozadí systémem_ cvičení poskytuje možnost spustit na pozadí na Apple Watch a získat přístup k datům HealthKit související aplikace.

Tento dokument se zavést funkci spuštěný na pozadí, zahrnují životní cyklus aplikace cvičení a zobrazit, jak můžete přispívat cvičení aplikace pro uživatele _aktivity okruhy_ na Apple Watch.

## <a name="about-workout-sessions"></a>O cvičení relací

Je srdcem každé cvičení aplikace _cvičení relace_ (`HKWorkoutSession`), uživatel může spustit a zastavit. Rozhraní API cvičení relace je snadno implementovat a poskytuje několik výhod cvičení aplikace, jako například:

- Pohybu a Kalorie vypálíte zjišťování na základě typu aktivity.
- Automatické příspěvku okruhy aktivity daného uživatele.
- Když v relaci, aplikace se automaticky zobrazí vždy, když uživatel probudí zařízení (ani jejich zápěstí vyvolání nebo interakci s Apple Watch).

## <a name="about-background-running"></a>O spuštění pozadí

Jak jsme uvedli výše, s watchOS 3 cvičení aplikace může být nastaveny na spouštění na pozadí. Pomocí pozadí systémem cvičení aplikace může zpracovat data ze senzorů Apple Watch při spuštění na pozadí. Například aplikace můžete nadále monitorovat rychlost vysílat uživatele, i když už se zobrazí na obrazovce.

Pozadí spuštěná také poskytuje možnost prezentovat živých zpětné vazby uživatele v průběhu aktivní relaci cvičení, například odeslání oznámení na hmatová informovat uživatele o průběhu aktuální.

Spuštění pozadí navíc umožňuje aplikaci rychle aktualizovat svoje uživatelské rozhraní, když se rychle přehled v jejich Apple Watch má uživatel nejnovější data.

Pokud chcete zachovat vysoký výkon na Apple Watch, sledování aplikace pomocí pozadí systémem měli omezit množství práce na pozadí a šetřit tak stav baterie. Pokud aplikace používá nadměrnému využití procesoru v pozadí, můžete získat pozastavil watchOS.

### <a name="enabling-background-running"></a>Povolení spuštěná na pozadí

Pokud chcete povolit spuštění pozadí, postupujte takto:

1. V **Průzkumníku řešení**, dvakrát klikněte na rozšíření sledovat doprovodné iPhone aplikace `Info.plist` soubor otevřete pro úpravy.
2. Přepnout **zdroj** zobrazení: 

    [![](workout-apps-images/plist01.png "Zobrazení zdroje")](workout-apps-images/plist01.png#lightbox)
3. Přidejte nový klíč s názvem `WKBackgroundModes` a nastavte **typ** k `Array`: 

    [![](workout-apps-images/plist02.png "Přidejte nový klíč s názvem WKBackgroundModes")](workout-apps-images/plist02.png#lightbox)
4. Přidat novou položku do pole s **typ** z `String` a hodnota `workout-processing`: 

    [![](workout-apps-images/plist03.png "Přidat novou položku do pole s řetězec typ a hodnotu cvičení zpracování")](workout-apps-images/plist03.png#lightbox)
5. Uložte změny do souboru.

## <a name="starting-a-workout-session"></a>Spuštění relace cvičení

Spuštění relace cvičení tři hlavní kroky:

[![](workout-apps-images/workout02.png "Tři hlavní kroky pro spuštění relace cvičení")](workout-apps-images/workout02.png#lightbox)

1. Aplikace musí požádat o autorizaci pro přístup k datům v HealthKit.
2. Vytvořte objekt konfigurace cvičení pro typ cvičení spuštění.
3. Vytvořte a spusťte relaci cvičení pomocí nově vytvořený cvičení konfigurace.

### <a name="requesting-authorization"></a>Požaduje autorizaci

Předtím, než aplikaci přístup k datům HealthKit uživatele, musí žádosti a přijímat autorizace od uživatele. V závislosti na povaze cvičení aplikace může mít následující typy požadavků:

- Oprávnění pro zápis dat:
    - Cvičení nebo
- Oprávnění pro čtení dat:
    - Energie zapsaný
    - Vzdálenost
    - Míra vysílat  

Předtím, než aplikace může požádat o autorizaci, musí být nakonfigurovaná pro přístup k HealthKit.

Postupujte takto:

1. V **Průzkumníku řešení**, dvakrát klikněte `Entitlements.plist` soubor otevřete pro úpravy.
2. Přejděte do dolní a zkontrolujte **povolit HealthKit**: 

    [![](workout-apps-images/auth01.png "Kontrola povolení HealthKit")](workout-apps-images/auth01.png#lightbox)
3. Uložte změny do souboru.
4. Postupujte podle pokynů [explicitní ID aplikace a profil zřizování](~/ios/platform/healthkit.md) a [přidružení ID aplikace a zřizování profilu se vaše aplikace Xamarin.iOS](~/ios/platform/healthkit.md) části [Úvod do HealthKit](~/ios/platform/healthkit.md) článku správně zřídit aplikaci.
5. Nakonec, postupujte podle pokynů v [programování Kit stavu](~/ios/platform/healthkit.md) a [žádají o oprávnění z pole uživatelské](~/ios/platform/healthkit.md) části [Úvod do HealthKit](~/ios/platform/healthkit.md) článek na žádost oprávnění pro přístup k úložišti dat HealthKit uživatele.

### <a name="setting-the-workout-configuration"></a>Nastavení konfigurace cvičení

Cvičení relací jsou vytvořené pomocí objektu konfigurace cvičení (`HKWorkoutConfiguration`) určující typ cvičení (například `HKWorkoutActivityType.Running`) a cvičení umístění (například `HKWorkoutSessionLocationType.Outdoor`):

```csharp
using HealthKit;
...

// Create a workout configuration
var configuration = new HKWorkoutConfiguration () {
    ActivityType = HKWorkoutActivityType.Running,
    LocationType = HKWorkoutSessionLocationType.Outdoor
};
```

### <a name="creating-a-workout-session-delegate"></a>Vytvoření relace delegáta cvičení 

Zpracování událostí, které může dojít během relace cvičení, aplikaci muset vytvořit instanci cvičení relace delegáta. Přidejte novou třídu do projektu a základní se odhlásit z `HKWorkoutSessionDelegate` třídy. Příklad venku spustit může vypadat takto:

```csharp
using System;
using Foundation;
using WatchKit;
using HealthKit;

namespace MonkeyWorkout.MWWatchExtension
{
    public class OutdoorRunDelegate : HKWorkoutSessionDelegate
    {
        #region Computed Properties
        public HKHealthStore HealthStore { get; private set; }
        public HKWorkoutSession WorkoutSession { get; private set;}
        #endregion

        #region Constructors
        public OutdoorRunDelegate (HKHealthStore healthStore, HKWorkoutSession workoutSession)
        {
            // Initialize
            this.HealthStore = healthStore;
            this.WorkoutSession = workoutSession;

            // Attach this delegate to the session
            workoutSession.Delegate = this;
        }
        #endregion

        #region Override Methods
        public override void DidFail (HKWorkoutSession workoutSession, NSError error)
        {
            // Handle workout session failing
            RaiseFailed ();
        }

        public override void DidChangeToState (HKWorkoutSession workoutSession, HKWorkoutSessionState toState, HKWorkoutSessionState fromState, NSDate date)
        {
            // Take action based on the change in state
            switch (toState) {
            case HKWorkoutSessionState.NotStarted:
                break;
            case HKWorkoutSessionState.Paused:
                RaisePaused ();
                break;
            case HKWorkoutSessionState.Running:
                RaiseRunning ();
                break;
            case HKWorkoutSessionState.Ended:
                RaiseEnded ();
                break;
            }

        }

        public override void DidGenerateEvent (HKWorkoutSession workoutSession, HKWorkoutEvent @event)
        {
            base.DidGenerateEvent (workoutSession, @event);
        }
        #endregion

        #region Events
        public delegate void OutdoorRunEventDelegate ();

        public event OutdoorRunEventDelegate Failed;
        internal void RaiseFailed ()
        {
            if (this.Failed != null) this.Failed ();
        }


        public event OutdoorRunEventDelegate Paused;
        internal void RaisePaused ()
        {
            if (this.Paused != null) this.Paused ();
        }

        public event OutdoorRunEventDelegate Running;
        internal void RaiseRunning ()
        {
            if (this.Running != null) this.Running ();
        }

        public event OutdoorRunEventDelegate Ended;
        internal void RaiseEnded ()
        {
            if (this.Ended != null) this.Ended ();
        }
        #endregion
    }
}
```

Tato třída se vytvoří několik událostí, které bude vyvolána jako stav relace cvičení změny (`DidChangeToState`) a pokud se nezdaří cvičení relace (`DidFail`). 

### <a name="creating-a-workout-session"></a>Vytvoření relace cvičení

Pomocí konfigurace cvičení a cvičení relace delegáta vytvořili výše vytvořit novou relaci cvičení a spusťte ji pro uživatele výchozí HealthKit úložiště:

```csharp
using HealthKit;
...

#region Computed Properties
public HKHealthStore HealthStore { get; set;} = new HKHealthStore ();
public OutdoorRunDelegate RunDelegate { get; set; }
#endregion
...

private void StartOutdoorRun ()
{
    // Create a workout configuration
    var configuration = new HKWorkoutConfiguration () {
        ActivityType = HKWorkoutActivityType.Running,
        LocationType = HKWorkoutSessionLocationType.Outdoor
    };

    // Create workout session
    // Start workout session
    NSError error = null;
    var workoutSession = new HKWorkoutSession (configuration, out error);

    // Successful?
    if (error != null) {
        // Report error to user and return
        return;
    }

    // Create workout session delegate and wire-up events
    RunDelegate = new OutdoorRunDelegate (HealthStore, workoutSession);

    RunDelegate.Failed += () => {
        // Handle the session failing
    };

    RunDelegate.Paused += () => {
        // Handle the session being paused
    };

    RunDelegate.Running += () => {
        // Handle the session running
    };

    RunDelegate.Ended += () => {
        // Handle the session ending
    };

    // Start session
    HealthStore.StartWorkoutSession (workoutSession);
}
```

Pokud tuto relaci cvičení spuštění aplikace a uživatel přepíná zpět na jejich vzhled sledovat, zobrazí se jen nepatrnou zelené ikony "spuštění man" výše písmo:

[![](workout-apps-images/workout03.png "Malá velikost zelené spuštěné man ikony zobrazují nad písmo")](workout-apps-images/workout03.png#lightbox)

Pokud uživatel klepne na tuto ikonu, že budete přesměrováni zpět do aplikace.

## <a name="data-collection-and-control"></a>Shromažďování dat a řízení

Jakmile relaci cvičení byla nakonfigurována a spuštěna, aplikace bude nutné shromažďovat data o relaci (například rychlost vysílat uživatele) a řízení stavu relace:

[![](workout-apps-images/workout04.png "Shromažďování dat a Diagram řízení")](workout-apps-images/workout04.png#lightbox)

1. **Sledování ukázky** -aplikaci bude nutné načíst informace z HealthKit, která se zobrazí uživateli a reagovali na ni.
2. **Sledování události** – aplikace bude muset reakce na události, které jsou generovány při HealthKit, nebo uživatelském rozhraní aplikace (například uživatel pozastavení cvičení).
3. **Zadejte stav běhu** -relace byla spuštěna a je aktuálně spuštěna.
4. **Zadejte stav pozastaveno** -uživatel pozastaví aktuální relace cvičení a můžete ho spustit znovu později. Uživatel může přepínat mezi spuštěné a pozastavený stav několikrát v rámci jedné relace cvičení.
5. **Ukončení relace cvičení** – kdykoli uživatele můžete ukončit relaci cvičení nebo může vypršet a končí na svůj vlastní, pokud se jednalo o měřené cvičení (například dvě míle spustit).

Posledním krokem je uložte výsledky cvičení relace uživatele HealthKit úložištěm.

### <a name="observing-healthkit-samples"></a>Sledování HealthKit ukázky

Aplikace bude muset otevřít _dotaz na objekt ukotvení_ pro každý z dat HealthKit body ho je zajímá, například vysílat míry nebo aktivní energie zapsaný. Pro každý datový bod se zjištěnými bude nutné vytvořit pro zachycení nová data, jako je odesílají do aplikace obslužnou rutinu aktualizace.

Z těchto datových bodů můžete aplikaci hromadit součty (například celkový spuštění vzdálenost) a aktualizujte jeho uživatelském rozhraní podle potřeby. Kromě toho můžete aplikace upozorněte uživatele, jakmile dosáhnou určité cíle nebo dosažení, jako je například dokončení další mil od spuštění.

Podívejte se na následující vzorový kód:

```csharp
private void ObserveHealthKitSamples ()
{
    // Get the starting date of the required samples
    var datePredicate = HKQuery.GetPredicateForSamples (WorkoutSession.StartDate, null, HKQueryOptions.StrictStartDate);

    // Get data from the local device
    var devices = new NSSet<HKDevice> (new HKDevice [] { HKDevice.LocalDevice });
    var devicePredicate = HKQuery.GetPredicateForObjectsFromDevices (devices);

    // Assemble compound predicate
    var queryPredicate = NSCompoundPredicate.CreateAndPredicate (new NSPredicate [] { datePredicate, devicePredicate });

    // Get ActiveEnergyBurned
    var queryActiveEnergyBurned = new HKAnchoredObjectQuery (HKQuantityType.Create (HKQuantityTypeIdentifier.ActiveEnergyBurned), queryPredicate, null, HKSampleQuery.NoLimit, (query, addedObjects, deletedObjects, newAnchor, error) => {
        // Valid?
        if (error == null) {
            // Yes, process all returned samples
            foreach (HKSample sample in addedObjects) {
                var quantitySample = sample as HKQuantitySample;
                ActiveEnergyBurned += quantitySample.Quantity.GetDoubleValue (HKUnit.Joule);
            }
            
            // Update User Interface
            ...
        }
    });

    // Start Query
    HealthStore.ExecuteQuery (queryActiveEnergyBurned);
                                          
}
```

Vytvoří predikátu k nastavte počáteční datum, která chce získat data pro použití `GetPredicateForSamples` metoda. Vytvoří sadu zařízení vyžádat HealthKit pomocí `GetPredicateForObjectsFromDevices` metody v tomto případě pouze místní Apple Watch (`HKDevice.LocalDevice`). Dva predikáty jsou sloučeny do složené predikátu (`NSCompoundPredicate`) pomocí `CreateAndPredicate` metoda.

Nový `HKAnchoredObjectQuery` je vytvořen pro datový bod požadovaného (v tomto případě `HKQuantityTypeIdentifier.ActiveEnergyBurned` pro datový bod Active zapsaný energie), žádné omezení není nastaveno na množství dat vrácených (`HKSampleQuery.NoLimit`) a obslužné rutiny aktualizace není definován pro zpracování dat vrácení, vraťte se do aplikace z HealthKit. 

Nová data se doručí do aplikace pro daný datový bod, když bude volána obslužná rutina aktualizace. Pokud žádná chyba je vrácena, aplikace můžete bezpečně číst data, proveďte všechny požadované výpočty a aktualizujte jeho uživatelském rozhraní podle potřeby.

Kód cyklicky prochází přes všechny ukázky (`HKSample`), vrátí se v `addedObjects` pole a jejich vrhá vzorku množství (`HKQuantitySample`). Potom získá hodnota typu double vzorku jako Joule – měrná (`HKUnit.Joule`) se shromáždí do spuštění úplného active energie zapsal cvičení a aktualizuje uživatelské rozhraní.

### <a name="achieved-goal-notification"></a>Dosažených cílem oznámení

Jak je uvedeno výše Pokud uživatele lze dosáhnout cíle v cvičení aplikaci (například dokončení první mil od spustit), může poslat hmatová zpětné vazby uživatele na základě Taptic modul. Aplikace by měl taky aktualizovat jeho uživatelského rozhraní v tomto okamžiku vzhledem k tomu, že uživatel více než pravděpodobně vyvolá jejich zápěstí zobrazíte událost, která se zobrazí výzva zpětnou vazbu.

Přehrávání hmatová zpětnou vazbu, použijte následující kód:

```csharp
// Play haptic feedback
WKInterfaceDevice.CurrentDevice.PlayHaptic (WKHapticType.Notification);
```

### <a name="observing-events"></a>Sledování události

Události jsou časová razítka, abyste měli na očích určitých bodech během cvičení uživatele můžete použít aplikace. Některé události budou vytvořené aplikací a uloženy do cvičení a některé události se HealthKit vytvoří automaticky.

Sledovat události, které jsou vytvořené pomocí HealthKit, přepíše aplikace `DidGenerateEvent` metodu `HKWorkoutSessionDelegate`:

```csharp
using System.Collections.Generic;
...

public List<HKWorkoutEvent> WorkoutEvents { get; set; } = new List<HKWorkoutEvent> ();
...

public override void DidGenerateEvent (HKWorkoutSession workoutSession, HKWorkoutEvent @event)
{
    base.DidGenerateEvent (workoutSession, @event);
    
    // Save HealthKit generated event
    WorkoutEvents.Add (@event);
    
    // Take action based on the type of event
    switch (@event.Type) {
    case HKWorkoutEventType.Lap:
        break;
    case HKWorkoutEventType.Marker:
        break;
    case HKWorkoutEventType.MotionPaused:
        break;
    case HKWorkoutEventType.MotionResumed:
        break;
    case HKWorkoutEventType.Pause:
        break;
    case HKWorkoutEventType.Resume:
        break;
    }
}
```

Apple přidala následující nové typy událostí v watchOS 3:

- `HKWorkoutEventType.Lap` -Jsou pro události, které cvičení rozdělit na části stejnou vzdálenost. Například pro označení jednoho okruhu kolem sledovat během spuštění.
- `HKWorkoutEventType.Marker` -V cvičení jsou pro libovolný body, které vás zajímají. Například dosažení určitý bod v postupu venku spustit.

Tyto nové typy může být vytvořené aplikací a uložená v cvičení pro pozdější použití při vytváření grafy a statistiky.

Pokud chcete vytvořit událost, postupujte takto:

```csharp
using System.Collections.Generic;
...

public float MilesRun { get; set; }
public List<HKWorkoutEvent> WorkoutEvents { get; set; } = new List<HKWorkoutEvent> ();
...

public void ReachedNextMile ()
{
    // Create and save marker event
    var markerEvent = HKWorkoutEvent.Create (HKWorkoutEventType.Marker, NSDate.Now);
    WorkoutEvents.Add (markerEvent);

    // Notify user
    NotifyUserOfReachedMileGoal (++MilesRun);
}
```

Tento kód vytvoří novou instanci třídy událostí značky (`HKWorkoutEvent`) a ukládá je do privátní kolekce událostí (které se později zapsána do relace cvičení) a upozorní uživatele prostřednictvím haptics události.

### <a name="pausing-and-resuming-workouts"></a>Pozastavení a obnovení cvičení nebo

V libovolném bodě v relaci cvičení můžete uživatele dočasně pozastavit cvičení a obnovit později. Například se může pozastavit vnitřních spustit a provést důležité volání a obnovit tak, že spustíte po dokončení volání.

Uživatelském rozhraní aplikace, měl by poskytnout způsob, jak pozastavení a obnovení cvičení (voláním HealthKit) tak, aby Apple Watch lze ušetřit místo na napájení a data, když uživatel pozastavilo, jejich aktivita. Aplikace by měl taky ignorovat jakékoli nové datové body, které může být přijata při cvičení relace je v pozastaveném stavu.

HealthKit bude odpovídat pozastavení a obnovení volání vygenerováním události pozastavení a obnovení. Během relace cvičení je pozastavena, žádné nové události nebo data se odešle do aplikace pomocí HealthKit dokud relaci je obnovit.

Pozastavení a obnovení cvičení relace použijte následující kód:

```csharp
public HKHealthStore HealthStore { get; set;} = new HKHealthStore ();
public HKWorkoutSession WorkoutSession { get; set;}
...

public void PauseWorkout ()
{
    // Pause the current workout
    HealthStore.PauseWorkoutSession (WorkoutSession);
}

public void ResumeWorkout ()
{
    // Pause the current workout
    HealthStore.ResumeWorkoutSession (WorkoutSession);
}
```

Pozastavení a obnovení události, které budou generovány z HealthKit mohou být zpracovány přepsáním `DidGenerateEvent` metodu `HKWorkoutSessionDelegate`:

```csharp
public override void DidGenerateEvent (HKWorkoutSession workoutSession, HKWorkoutEvent @event)
{
    base.DidGenerateEvent (workoutSession, @event);

    // Take action based on the type of event
    switch (@event.Type) {
    case HKWorkoutEventType.Pause:
        break;
    case HKWorkoutEventType.Resume:
        break;
    }
}
```

### <a name="motion-events"></a>Pohybu události

Také nové watchOS 3, je pozastavená pohybu (`HKWorkoutEventType.MotionPaused`) a obnovit pohybu (`HKWorkoutEventType.MotionResumed`) události. Tyto události jsou aktivováno automaticky HealthKit během spuštěné cvičení když uživatel spustí a zastaví přesunutí.

Aplikace obdrží o událost pohybu pozastavena, měla by zastavit shromažďování dat, dokud uživatel obnoví pohybu a obnoví pohybu události. Aplikace aplikace by neměl pozastavit cvičení relace v reakci na událost pohybu pozastavena.

> [!IMPORTANT]
> **Poznámka:** pohybu pozastavit a obnovit pohybu události jsou podporovány pouze pro typ aktivity RunningWorkout (`HKWorkoutActivityType.Running`).

Tyto události znovu, mohou být zpracovány přepsáním `DidGenerateEvent` metodu `HKWorkoutSessionDelegate`:

```csharp
public override void DidGenerateEvent (HKWorkoutSession workoutSession, HKWorkoutEvent @event)
{
    base.DidGenerateEvent (workoutSession, @event);
    
    // Take action based on the type of event
    switch (@event.Type) {
    case HKWorkoutEventType.MotionPaused:
        break;
    case HKWorkoutEventType.MotionResumed:
        break;
    }
}

```

## <a name="ending-and-saving-the-workout-session"></a>Ukončení a ukládání cvičení relace

Pokud uživatel dokončil jejich cvičení, aplikace bude potřeba k ukončení aktuální relace cvičení a uložte je do databáze HealthKit. Cvičení nebo uložit do HealthKit se automaticky zobrazí v seznamu aktivity cvičení.

Nové do systému iOS 10, jedná se o seznamu cvičení seznam aktivit na uživatele i zařízení iPhone. Takže i v případě, že není blízkým Apple Watch, zobrazí cvičení na telefonu.

Cvičení, které zahrnují energie ukázky nebo aktualizuje uživatele přesunout prstenec v aplikaci aktivity, 3. stran aplikace teď může přispívat k přesunutí cíle denní uživatele.

Následující kroky jsou nezbytné pro ukončení a uložení cvičení relace:

[![](workout-apps-images/workout05.png "Ukončení a ukládání Diagram relace cvičení")](workout-apps-images/workout05.png#lightbox)

1. Aplikace se nejprve ukončit relaci cvičení.
2. Cvičení relace je uložena do HealthKit.
3. Přidáte všechny ukázky (například energie zapsaný nebo vzdálenost) uložené relace cvičení.

### <a name="ending-the-session"></a>Ukončení relace

Chcete-li ukončit relaci cvičení, volejte `EndWorkoutSession` metodu `HKHealthStore` předávání v `HKWorkoutSession`:

```csharp
public HKHealthStore HealthStore { get; private set; }
public HKWorkoutSession WorkoutSession { get; private set;}
...

public void EndOutdoorRun ()
{
    // End the current workout session
    HealthStore.EndWorkoutSession (WorkoutSession);
}
```

Snímače zařízení se resetuje do jejich normálního režimu. Po dokončení končící cvičení HealthKit obdrží zpětné volání pro `DidChangeToState` metodu `HKWorkoutSessionDelegate`:

```csharp
public override void DidChangeToState (HKWorkoutSession workoutSession, HKWorkoutSessionState toState, HKWorkoutSessionState fromState, NSDate date)
{
    // Take action based on the change in state
    switch (toState) {
    ...
    case HKWorkoutSessionState.Ended:
        StopObservingHealthKitSamples ();
        RaiseEnded ();
        break;
    }

}
```

### <a name="saving-the-session"></a>Uložení relace

Jakmile aplikaci skončila relace cvičení, ji budou muset vytvořit cvičení (`HKWorkout`) a uložte ho (spolu s událostí) k úložišti dat HealthKit (`HKHealthStore`):

```csharp
public HKHealthStore HealthStore { get; private set; }
public HKWorkoutSession WorkoutSession { get; private set;}
public float MilesRun { get; set; }
public double ActiveEnergyBurned { get; set;}
public List<HKWorkoutEvent> WorkoutEvents { get; set; } = new List<HKWorkoutEvent> ();
...

private void SaveWorkoutSession ()
{
    // Build required workout quantities 
    var energyBurned = HKQuantity.FromQuantity (HKUnit.Joule, ActiveEnergyBurned);
    var distance = HKQuantity.FromQuantity (HKUnit.Mile, MilesRun);

    // Create any required metadata
    var metadata = new NSMutableDictionary ();
    metadata.Add (new NSString ("HKMetadataKeyIndoorWorkout"), new NSString ("NO"));

    // Create workout
    var workout = HKWorkout.Create (HKWorkoutActivityType.Running, 
                                    WorkoutSession.StartDate, 
                                    NSDate.Now, 
                                    WorkoutEvents.ToArray (), 
                                    energyBurned, 
                                    distance, 
                                    metadata);

    // Save to HealthKit
    HealthStore.SaveObject (workout, (successful, error) => {
        // Handle any errors
        if (error == null) {
            // Was the save successful
            if (successful) {

            }
        } else {
            // Report error
        }
    });

}
```

Tento kód vytvoří vyžaduje pro cvičení jako celkovou velikost zapsaný energie a vzdálenost `HKQuantity` objekty. Slovník metadat definování cvičení je vytvořen a je zadána umístění cvičení:

```csharp
metadata.Add (new NSString ("HKMetadataKeyIndoorWorkout"), new NSString ("NO"));
```

Nový `HKWorkout` je vytvořen objekt se stejným `HKWorkoutActivityType` jako `HKWorkoutSession`, počáteční a koncové datum, seznam (Probíhá shromážděných z výše uvedených oddílech) události, energii zapsaný, celkový počet vzdálenost a metadata slovníku. Tento objekt je uložený v úložišti Health a řešit případné chyby.  

### <a name="adding-samples"></a>Přidání ukázky

Při aplikaci sadu vzorků, které se uloží do cvičení, vygeneruje HealthKit připojení mezi ukázky a cvičení sám sebe, tak, aby aplikace můžete dotazovat HealthKit později pro všechny vzorky spojené s danou cvičení. Na základě těchto informací aplikace generovat grafy z cvičení dat a jejich vykreslení proti časová osa cvičení.

Pro aplikaci přispívat k přesunutí prstenec aktivity aplikace musí obsahovat energie ukázky s uložené cvičení. Kromě toho součty pro vzdálenost a energie musí odpovídat součet všech vzorků, které aplikace přidruží uložené cvičení.

Chcete-li přidat ukázky uložené cvičení, postupujte takto:

```csharp
using System.Collections.Generic;
using WatchKit;
using HealthKit;
...

public HKHealthStore HealthStore { get; private set; }
public List<HKSample> WorkoutSamples { get; set; } = new List<HKSample> ();
...


private void SaveWorkoutSamples (HKWorkout workout)
{
    // Add samples to saved workout
    HealthStore.AddSamples (WorkoutSamples.ToArray (), workout, (success, error) => {
        // Handle any errors
        if (error == null) {
            // Was the save successful
            if (success) {

            }
        } else {
            // Report error
        }
    });
}
```

Aplikace můžete volitelně výpočtu a vytvořit menší podmnožinu ukázky nebo jeden velký vzorku (pokrývání uzlů celý rozsah cvičení), který získá přidružený uložené cvičení.

## <a name="workouts-and-ios-10"></a>Cvičení nebo a iOS 10

Každou aplikaci cvičení watchOS 3 má aplikace na základě cvičení iOS 10 na nadřazené a nové na iOS 10, tato aplikace pro iOS můžete použít ke spuštění cvičení, která bude umístit Apple Watch cvičení režimu (bez zásahu uživatele) a spusťte aplikaci watchOS v režimu spuštění pozadí (viz [o pozadí systémem](#About-Background-Running) výše podrobnosti).

Při spuštěné aplikaci watchOS použitím WatchConnectivity pro zasílání zpráv a komunikaci s nadřazenou aplikaci pro iOS.

Podívejte se na tom, jak tento proces funguje:

[![](workout-apps-images/workout06.png "iPhone a Apple Watch komunikace diagram")](workout-apps-images/workout06.png#lightbox)

1. Vytvoří aplikace iPhone `HKWorkoutConfiguration` objektu a nastaví cvičení typ a umístění.
2. `HKWorkoutConfiguration` Objektu je odeslán Apple Watch verze aplikace, a pokud ještě není spuštěná, je spuštěna v systému.
3. Pomocí předaný v cvičení konfiguraci, spuštění aplikace watchOS 3 novou relaci cvičení (`HKWorkoutSession`).

> [!IMPORTANT]
> **Poznámka:** v pořadí pro nadřazené iPhone aplikaci spustit cvičení na Apple Watch watchOS 3 aplikace musí mít pozadí systémem povolena. Najdete v tématu [povolení spuštění pozadí](#Enabling-Background-Running) výše další podrobnosti.

Tento proces je velmi podobný proces spouštění relaci cvičení přímo v aplikaci watchOS 3. Na zařízení iPhone použijte následující kód:

```csharp
using System;
using HealthKit;
using WatchConnectivity;
...

#region Computed Properties
public HKHealthStore HealthStore { get; set; } = new HKHealthStore ();
public WCSession ConnectivitySession { get; set; } = WCSession.DefaultSession;
#endregion
...

private void StartOutdoorRun ()
{
    // Can the app communicate with the watchOS version of the app?
    if (ConnectivitySession.ActivationState == WCSessionActivationState.Activated && ConnectivitySession.WatchAppInstalled) {
        // Create a workout configuration
        var configuration = new HKWorkoutConfiguration () {
            ActivityType = HKWorkoutActivityType.Running,
            LocationType = HKWorkoutSessionLocationType.Outdoor
        };

        // Start watch app
        HealthStore.StartWatchApp (configuration, (success, error) => {
            // Handle any errors
            if (error == null) {
                // Was the save successful
                if (success) {
                    ...
                }
            } else {
                // Report error
                ...
            }
        });
    }
}
```

Tento kód zajišťuje, že je nainstalovaná verze watchOS aplikace a verze iPhone k nim mohla připojit nejdřív:

```csharp
if (ConnectivitySession.ActivationState == WCSessionActivationState.Activated && ConnectivitySession.WatchAppInstalled) {
    ...
}
```

Potom vytvoří `HKWorkoutConfiguration` obvyklým a používá `StartWatchApp` metodu `HKHealthStore` poslat Apple Watch a spusťte aplikaci a cvičení relace.

A v aplikaci sledování operačního systému, použijte následující kód v `WKExtensionDelegate`:

```csharp
using WatchKit;
using HealthKit;
...

#region Computed Properties
public HKHealthStore HealthStore { get; set;} = new HKHealthStore ();
public OutdoorRunDelegate RunDelegate { get; set; }
#endregion
...


public override void HandleWorkoutConfiguration (HKWorkoutConfiguration workoutConfiguration)
{
    // Create workout session
    // Start workout session
    NSError error = null;
    var workoutSession = new HKWorkoutSession (workoutConfiguration, out error);

    // Successful?
    if (error != null) {
        // Report error to user and return
        return;
    }

    // Create workout session delegate and wire-up events
    RunDelegate = new OutdoorRunDelegate (HealthStore, workoutSession);

    RunDelegate.Failed += () => {
        // Handle the session failing
    };

    RunDelegate.Paused += () => {
        // Handle the session being paused
    };

    RunDelegate.Running += () => {
        // Handle the session running
    };

    RunDelegate.Ended += () => {
        // Handle the session ending
    };

    // Start session
    HealthStore.StartWorkoutSession (workoutSession);
}
```

Jak dlouho trvá `HKWorkoutConfiguration` a vytvoří novou `HKWorkoutSession` a připojí instanci vlastní `HKWorkoutSessionDelegate`. Spuštění relace cvičení proti uživatele HealthKit Health Store.

## <a name="bringing-all-the-pieces-together"></a>Spojením všechny části

Všechny informace uvedené v tomto dokumentu trvá, může aplikace na základě cvičení watchOS 3 a její nadřazené iOS 10 na základě cvičení aplikace obsahovat následujících částí:

1. **iOS 10 `ViewController.cs`**  -zpracovává počáteční připojení sledování relace a cvičení na Apple Watch.
2. **watchOS 3 `ExtensionDelegate.cs`**  -zpracovává watchOS 3 verze aplikace cvičení.
3. **watchOS 3 `OutdoorRunDelegate.cs`**  – vlastní `HKWorkoutSessionDelegate` zpracovat události pro cvičení.

> [!IMPORTANT]
> **Poznámka:** kód uvedené v následující části obsahují jenom části nutné implementovat nové a vylepšené funkce, které jsou zadané aplikace cvičení v watchOS 3. Všechny podpůrné kód a kód k dispozici a aktualizaci uživatelského rozhraní nezahrnuje ale můžete snadno vytvořit pomocí následujících naší watchOS dokumentaci.<p/>



### <a name="viewcontrollercs"></a>ViewController.cs

`ViewController.cs` Soubor ve verzi cvičení aplikace pro iOS 10 nadřazené by mělo zahrnovat následující kód:

```csharp
using System;
using HealthKit;
using UIKit;
using WatchConnectivity;

namespace MonkeyWorkout
{
    public partial class ViewController : UIViewController
    {
        #region Computed Properties
        public HKHealthStore HealthStore { get; set; } = new HKHealthStore ();
        public WCSession ConnectivitySession { get; set; } = WCSession.DefaultSession;
        #endregion

        #region Constructors
        protected ViewController (IntPtr handle) : base (handle)
        {
            // Note: this .ctor should not contain any initialization logic.
        }
        #endregion

        #region Private Methods
        private void InitializeWatchConnectivity ()
        {
            // Is Watch Connectivity supported?
            if (!WCSession.IsSupported) {
                // No, abort
                return;
            }

            // Is the session already active?
            if (ConnectivitySession.ActivationState != WCSessionActivationState.Activated) {
                // No, start session
                ConnectivitySession.ActivateSession ();
            }
        }

        private void StartOutdoorRun ()
        {
            // Can the app communicate with the watchOS version of the app?
            if (ConnectivitySession.ActivationState == WCSessionActivationState.Activated && ConnectivitySession.WatchAppInstalled) {
                // Create a workout configuration
                var configuration = new HKWorkoutConfiguration () {
                    ActivityType = HKWorkoutActivityType.Running,
                    LocationType = HKWorkoutSessionLocationType.Outdoor
                };

                // Start watch app
                HealthStore.StartWatchApp (configuration, (success, error) => {
                    // Handle any errors
                    if (error == null) {
                        // Was the save successful
                        if (success) {
                            ...
                        }
                    } else {
                        // Report error
                        ...
                    }
                });
            }
        }
        #endregion

        #region Override Methods
        public override void ViewDidLoad ()
        {
            base.ViewDidLoad ();

            // Start Watch Connectivity
            InitializeWatchConnectivity ();
        }
        #endregion
    }
}
```

### <a name="extensiondelegatecs"></a>ExtensionDelegate.cs

`ExtensionDelegate.cs` Souboru v watchOS 3 verze aplikace cvičení by mělo zahrnovat následující kód:

```csharp
using System;
using Foundation;
using WatchKit;
using HealthKit;

namespace MonkeyWorkout.MWWatchExtension
{
    public class ExtensionDelegate : WKExtensionDelegate
    {
        #region Computed Properties
        public HKHealthStore HealthStore { get; set;} = new HKHealthStore ();
        public OutdoorRunDelegate RunDelegate { get; set; }
        #endregion

        #region Constructors
        public ExtensionDelegate ()
        {
            
        }
        #endregion

        #region Private Methods
        private void StartWorkoutSession (HKWorkoutConfiguration workoutConfiguration)
        {
            // Create workout session
            // Start workout session
            NSError error = null;
            var workoutSession = new HKWorkoutSession (workoutConfiguration, out error);

            // Successful?
            if (error != null) {
                // Report error to user and return
                return;
            }

            // Create workout session delegate and wire-up events
            RunDelegate = new OutdoorRunDelegate (HealthStore, workoutSession);

            RunDelegate.Failed += () => {
                // Handle the session failing
                ...
            };

            RunDelegate.Paused += () => {
                // Handle the session being paused
                ...
            };

            RunDelegate.Running += () => {
                // Handle the session running
                ...
            };

            RunDelegate.Ended += () => {
                // Handle the session ending
                ...
            };
            
            RunDelegate.ReachedMileGoal += (miles) => {
                // Handle the reaching a session goal
                ...
            };

            RunDelegate.HealthKitSamplesUpdated += () => {
                // Update UI as required
                ...
            };

            // Start session
            HealthStore.StartWorkoutSession (workoutSession);
        }

        private void StartOutdoorRun ()
        {
            // Create a workout configuration
            var workoutConfiguration = new HKWorkoutConfiguration () {
                ActivityType = HKWorkoutActivityType.Running,
                LocationType = HKWorkoutSessionLocationType.Outdoor
            };

            // Start the session
            StartWorkoutSession (workoutConfiguration);
        }
        #endregion

        #region Override Methods
        public override void HandleWorkoutConfiguration (HKWorkoutConfiguration workoutConfiguration)
        {
            // Start the session
            StartWorkoutSession (workoutConfiguration);
        }
        #endregion
    }
}
```

### <a name="outdoorrundelegatecs"></a>OutdoorRunDelegate.cs

`OutdoorRunDelegate.cs` Souboru v watchOS 3 verze aplikace cvičení by mělo zahrnovat následující kód:

```csharp
using System;
using System.Collections.Generic;
using Foundation;
using WatchKit;
using HealthKit;

namespace MonkeyWorkout.MWWatchExtension
{
    public class OutdoorRunDelegate : HKWorkoutSessionDelegate
    {
        #region Private Variables
        private HKAnchoredObjectQuery QueryActiveEnergyBurned;
        #endregion

        #region Computed Properties
        public HKHealthStore HealthStore { get; private set; }
        public HKWorkoutSession WorkoutSession { get; private set;}
        public float MilesRun { get; set; }
        public double ActiveEnergyBurned { get; set;}
        public List<HKWorkoutEvent> WorkoutEvents { get; set; } = new List<HKWorkoutEvent> ();
        public List<HKSample> WorkoutSamples { get; set; } = new List<HKSample> ();
        #endregion

        #region Constructors
        public OutdoorRunDelegate (HKHealthStore healthStore, HKWorkoutSession workoutSession)
        {
            // Initialize
            this.HealthStore = healthStore;
            this.WorkoutSession = workoutSession;

            // Attach this delegate to the session
            workoutSession.Delegate = this;

        }
        #endregion

        #region Private Methods
        private void ObserveHealthKitSamples ()
        {
            // Get the starting date of the required samples
            var datePredicate = HKQuery.GetPredicateForSamples (WorkoutSession.StartDate, null, HKQueryOptions.StrictStartDate);

            // Get data from the local device
            var devices = new NSSet<HKDevice> (new HKDevice [] { HKDevice.LocalDevice });
            var devicePredicate = HKQuery.GetPredicateForObjectsFromDevices (devices);

            // Assemble compound predicate
            var queryPredicate = NSCompoundPredicate.CreateAndPredicate (new NSPredicate [] { datePredicate, devicePredicate });

            // Get ActiveEnergyBurned
            QueryActiveEnergyBurned = new HKAnchoredObjectQuery (HKQuantityType.Create (HKQuantityTypeIdentifier.ActiveEnergyBurned), queryPredicate, null, HKSampleQuery.NoLimit, (query, addedObjects, deletedObjects, newAnchor, error) => {
                // Valid?
                if (error == null) {
                    // Yes, process all returned samples
                    foreach (HKSample sample in addedObjects) {
                        // Accumulate totals
                        var quantitySample = sample as HKQuantitySample;
                        ActiveEnergyBurned += quantitySample.Quantity.GetDoubleValue (HKUnit.Joule);

                        // Save samples
                        WorkoutSamples.Add (sample);
                    }

                    // Inform caller
                    RaiseHealthKitSamplesUpdated ();
                }
            });

            // Start Query
            HealthStore.ExecuteQuery (QueryActiveEnergyBurned);
                                                  
        }

        private void StopObservingHealthKitSamples ()
        {
            // Stop query
            HealthStore.StopQuery (QueryActiveEnergyBurned);
        }

        private void ResumeObservingHealthkitSamples ()
        {
            // Resume current queries 
            HealthStore.ExecuteQuery (QueryActiveEnergyBurned);
        }

        private void NotifyUserOfReachedMileGoal (float miles)
        {
            // Play haptic feedback
            WKInterfaceDevice.CurrentDevice.PlayHaptic (WKHapticType.Notification);

            // Raise event
            RaiseReachedMileGoal (miles);
        }

        private void SaveWorkoutSession ()
        {
            // Build required workout quantities
            var energyBurned = HKQuantity.FromQuantity (HKUnit.Joule, ActiveEnergyBurned);
            var distance = HKQuantity.FromQuantity (HKUnit.Mile, MilesRun);

            // Create any required metadata
            var metadata = new NSMutableDictionary ();
            metadata.Add (new NSString ("HKMetadataKeyIndoorWorkout"), new NSString ("NO"));

            // Create workout
            var workout = HKWorkout.Create (HKWorkoutActivityType.Running, 
                                            WorkoutSession.StartDate, 
                                            NSDate.Now, 
                                            WorkoutEvents.ToArray (), 
                                            energyBurned, 
                                            distance, 
                                            metadata);

            // Save to HealthKit
            HealthStore.SaveObject (workout, (successful, error) => {
                // Handle any errors
                if (error == null) {
                    // Was the save successful
                    if (successful) {
                        // Add samples to workout
                        SaveWorkoutSamples (workout);
                    }
                } else {
                    // Report error
                    ...
                }
            });

        }

        private void SaveWorkoutSamples (HKWorkout workout)
        {
            // Add samples to saved workout
            HealthStore.AddSamples (WorkoutSamples.ToArray (), workout, (success, error) => {
                // Handle any errors
                if (error == null) {
                    // Was the save successful
                    if (success) {
                        ...
                    }
                } else {
                    // Report error
                    ...
                }
            });
        }
        #endregion

        #region Public Methods
        public void PauseWorkout ()
        {
            // Pause the current workout
            HealthStore.PauseWorkoutSession (WorkoutSession);
        }

        public void ResumeWorkout ()
        {
            // Pause the current workout
            HealthStore.ResumeWorkoutSession (WorkoutSession);
        }

        public void ReachedNextMile ()
        {
            // Create and save marker event
            var markerEvent = HKWorkoutEvent.Create (HKWorkoutEventType.Marker, NSDate.Now);
            WorkoutEvents.Add (markerEvent);

            // Notify user
            NotifyUserOfReachedMileGoal (++MilesRun);
        }

        public void EndOutdoorRun ()
        {
            // End the current workout session
            HealthStore.EndWorkoutSession (WorkoutSession);
        }
        #endregion

        #region Override Methods
        public override void DidFail (HKWorkoutSession workoutSession, NSError error)
        {
            // Handle workout session failing
            RaiseFailed ();
        }

        public override void DidChangeToState (HKWorkoutSession workoutSession, HKWorkoutSessionState toState, HKWorkoutSessionState fromState, NSDate date)
        {
            // Take action based on the change in state
            switch (toState) {
            case HKWorkoutSessionState.NotStarted:
                break;
            case HKWorkoutSessionState.Paused:
                StopObservingHealthKitSamples ();
                RaisePaused ();
                break;
            case HKWorkoutSessionState.Running:
                if (fromState == HKWorkoutSessionState.Paused) {
                    ResumeObservingHealthkitSamples ();
                } else {
                    ObserveHealthKitSamples ();
                }
                RaiseRunning ();
                break;
            case HKWorkoutSessionState.Ended:
                StopObservingHealthKitSamples ();
                SaveWorkoutSession ();
                RaiseEnded ();
                break;
            }

        }

        public override void DidGenerateEvent (HKWorkoutSession workoutSession, HKWorkoutEvent @event)
        {
            base.DidGenerateEvent (workoutSession, @event);

            // Save HealthKit generated event
            WorkoutEvents.Add (@event);

            // Take action based on the type of event
            switch (@event.Type) {
            case HKWorkoutEventType.Lap:
                ...
                break;
            case HKWorkoutEventType.Marker:
                ...
                break;
            case HKWorkoutEventType.MotionPaused:
                ...
                break;
            case HKWorkoutEventType.MotionResumed:
                ...
                break;
            case HKWorkoutEventType.Pause:
                ...
                break;
            case HKWorkoutEventType.Resume:
                ...
                break;
            }
        }
        #endregion

        #region Events
        public delegate void OutdoorRunEventDelegate ();
        public delegate void OutdoorRunMileGoalDelegate (float miles);

        public event OutdoorRunEventDelegate Failed;
        internal void RaiseFailed ()
        {
            if (this.Failed != null) this.Failed ();
        }


        public event OutdoorRunEventDelegate Paused;
        internal void RaisePaused ()
        {
            if (this.Paused != null) this.Paused ();
        }

        public event OutdoorRunEventDelegate Running;
        internal void RaiseRunning ()
        {
            if (this.Running != null) this.Running ();
        }

        public event OutdoorRunEventDelegate Ended;
        internal void RaiseEnded ()
        {
            if (this.Ended != null) this.Ended ();
        }

        public event OutdoorRunMileGoalDelegate ReachedMileGoal;
        internal void RaiseReachedMileGoal (float miles)
        {
            if (this.ReachedMileGoal != null) this.ReachedMileGoal (miles);
        }

        public event OutdoorRunEventDelegate HealthKitSamplesUpdated;
        internal void RaiseHealthKitSamplesUpdated ()
        {
            if (this.HealthKitSamplesUpdated != null) this.HealthKitSamplesUpdated ();
        }
        #endregion
    }
}
```

## <a name="best-practices"></a>Doporučené postupy

Apple navrhuje pomocí následující osvědčené postupy při návrhu a implementace cvičení aplikace v watchOS 3 a iOS 10:

- Zajistěte, aby aplikace cvičení watchOS 3 funkční i v případě, že se nemůže připojit k iPhone a iOS 10 verzi aplikace.
- Vzdálenost HealthKit použijte, když GPS není k dispozici, protože je schopna generovat vzdálenost ukázky bez GPS.
- Povolte uživatelům spustit cvičení z Apple Watch nebo iPhone.
- Povolit aplikaci zobrazíte cvičení nebo z jiných zdrojů (například další aplikace, 3. stran) v jeho zobrazení na historická data.
- Ujistěte se, že aplikace nemá odstranit zobrazení cvičení nebo v historická data.

## <a name="summary"></a>Souhrn

Tento článek má zahrnutých rozšířením Apple provedl cvičení aplikací ve watchOS 3 a jejich implementaci v Xamarin.



## <a name="related-links"></a>Související odkazy

- [iOS 10 – ukázky](https://developer.xamarin.com/samples/ios/iOS10/)
- [Úvod do HealthKit](~/ios/platform/healthkit.md)
