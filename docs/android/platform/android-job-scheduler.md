---
title: Plánovač úloh Android
description: Tato příručka popisuje, jak při plánování práce na pozadí pomocí rozhraní API systému Android plánovače úloh.
ms.prod: xamarin
ms.assetid: 673BB8C3-C5CC-43EC-BA8F-758F15D986C9
ms.technology: xamarin-android
author: topgenorth
ms.author: toopge
ms.date: 03/19/2018
ms.openlocfilehash: dc72b7e4da330185b00541f923d9c4b64b91bc95
ms.sourcegitcommit: d450ae06065d8f8c80f3588bc5a614cfd97b5a67
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 03/21/2018
ms.locfileid: "30005663"
---
# <a name="android-job-scheduler"></a>Plánovač úloh Android

_Tato příručka popisuje, jak při plánování práce na pozadí pomocí Android API plánovače úloh, která je k dispozici na Android zařízení se systémem Android 5.0 (API úrovně 21) a vyšší._


## <a name="overview"></a>Přehled 

Doporučené způsoby zachovat přizpůsobivý uživateli aplikace pro Android je zajistit, že komplexní nebo dlouhotrvající pracovních probíhá na pozadí. Je ale důležité, aby práce pozadí nebude negativní dopad na možnosti pro uživatele se zařízením. 

Úlohu na pozadí například může dotazovat webu každé tři nebo čtyři minut dotazu pro změny konkrétní datové sady. Vypadá to neškodný, ale by mít katastrofální dopad na výdrž baterie. Aplikace bude opakovaně probuzení zařízení, zvýšit procesoru s vyšší spotřebou, napájení kombinace, síťové požadavky a pak zpracování výsledků. Bude hůř, protože zařízení není ihned vypněte a vrátit do nečinného stavu snížené spotřeby. Práce špatně naplánované pozadí může nechtěně zachovat zařízení ve stavu s požadavky na nepotřebné a nadměrné spotřeby. Tato zdánlivě nevinnosti aktivita (dotazování web) bude nepoužitelná zařízení v poměrně krátké době.

Android poskytuje následující rozhraní API, které pomáhají při provádění práce na pozadí, ale samotné nejsou dostatečná pro plánování inteligentního úloh. 

* **[Záměrné služby](~/android/app-fundamentals/services/creating-a-service/intent-services.md)**  &ndash; záměr služby jsou skvěle hodí k provedení práce, ale poskytují způsob, jak plánování práce.
* **[AlarmManager](https://developer.android.com/reference/android/app/AlarmManager.html)**  &ndash; tyto rozhraní API pouze povolit, aby se naplánovaná, ale poskytují způsob, jak skutečně provést nějakou práci. Také AlarmManager umožňuje pouze čas na základě omezení, což znamená, že vyvolat alarm v určitém čase nebo po uplynutí určité doby. 
* **[Vysílání příjemci](~/android/app-fundamentals/broadcast-receivers.md)**  &ndash; Android aplikace se dá nastavit všesměrového vysílání příjemci pro práci v reakci na systémové události nebo tříd Intent. Ale všesměrového vysílání příjemci neposkytují žádné kontrolu nad spuštění úlohy. Také se omezí změny v operační systém Android když bude fungovat všesměrového vysílání příjemci nebo druhy práci, kterou může reagovat. 

Existují dvě klíčové funkce pro efektivní provádění práce pozadí (někdy označovány jako _úloha na pozadí_ nebo _úlohy_):

1. **Inteligentně plánování práce** &ndash; je důležité, když aplikace pracuje na pozadí, dělá to tak, jako dobrý občanem. V ideálním případě by neměl aplikace potřebují spustit úlohu. Místo toho by měla aplikace zadat podmínky, které musí být splněné, pokud úlohu můžete spustit a pak plánování této úlohy v operačním systému, která provede práci, pokud jsou splněny podmínky. To umožňuje Android a spusťte úlohu zajistit maximální efektivity na zařízení. Například síťové požadavky může zpracovat v dávce ke spuštění všech ve stejnou dobu, aby maximální využití režijní náklady spojené s prací v síti.
2. **Zapouzdření prací** &ndash; kód k provedení práce na pozadí by měl být zapouzdřené v diskrétní komponenty, která lze spustit nezávisle na uživatelské rozhraní a bude je poměrně snadné ho přeplánovat práce selže-li dokončit z nějakého důvodu.

Android Plánovač úloh je architektura založená na operační systém Android, která poskytuje rozhraní fluent API zjednodušit plánování práce na pozadí.  Android Plánovač úloh se skládá z následujících typů:

* `Android.App.Job.JobScheduler` Je systémová služba, která slouží k plánování, spouštění a v případě potřeby zrušit, úlohy jménem aplikace platformy Android.
* `Android.App.Job.JobService` Je abstraktní třída, která musí být rozšířena logiky, která se spustit úlohu v hlavní vlákno aplikace. To znamená, že `JobService` zodpovídá za způsob práce se má provést asynchronně.
* `Android.App.Job.JobInfo` Objektu obsahuje kritéria pro Průvodce Android, když má být úloha spuštěna.

Při plánování práce s Androidem Plánovač úloh, musí aplikace pro Xamarin.Android zapouzdřovat kód na třídu, která rozšiřuje `JobService` třídy. `JobService` má tři metody životního cyklu, která může být volána po dobu životnosti úlohy:

* **BOOL OnStartJob (JobParameters parametry)** &ndash; tato metoda je volána `JobScheduler` k práci a běží na hlavní vlákno aplikace. Je je zodpovědností `JobService` pro asynchronně práci a `true` Pokud je pracovní zbývající, nebo `false` Pokud práci.
    
    Když `JobScheduler` volá tuto metodu, budou žádosti a zachovat wakelock z Android po dobu trvání úlohy. Po dokončení úlohy se má na starosti `JobService` říct `JobScheduler` tuto skutečnost voláním `JobFinished` – metoda (popsána dále).

* **JobFinished (JobParameters parametry, bool needsReschedule)** &ndash; musí být tato metoda volána `JobService` říct `JobScheduler` který práci. Pokud `JobFinished` není volán `JobScheduler` neodeberete wakelock, způsobuje nepotřebné baterie vyprazdňování. 

* **BOOL OnStopJob (JobParameters parametry)** &ndash; volá se, když je úloha předčasně zastavena Android. Měla by vrátit `true` Pokud úloha by ho přeplánovat na základě kritérií opakování (podrobněji popsané níže).

Je možné zadat _omezení_ nebo _aktivační události_ , bude řídit, kdy úlohu můžete nebo se má spustit. Například je možné omezit úlohu tak, že lze spustit pouze když je poplatků zařízení nebo se provede při spuštění úlohy při obrázku.

Tato příručka popisuje podrobně implementovat `JobService` třídy a naplánovat její `JobScheduler`.

## <a name="requirements"></a>Požadavky

Android plánovače úloh vyžaduje úroveň rozhraní API systému Android 21 (Android 5.0) nebo vyšší. 

## <a name="using-the-android-job-scheduler"></a>Pomocí plánovače úloh Android

Existují tři kroky pro používání rozhraní API systému Android JobScheduler:

1. Implementujte typ JobService k zapouzdření prací.
2. Použití `JobInfo.Builder` objekt k vytvoření `JobInfo` objekt, který bude obsahovat kritéria pro `JobScheduler` chcete spustit úlohu. 
3. Naplánovat úlohu pomocí `JobScheduler.Schedule`.

### <a name="implement-a-jobservice"></a>Implementace JobService

Všechny pracovní provádí knihovně Android plánovače úloh je třeba provést v typu, který rozšiřuje `Android.App.Job.JobService` abstraktní třídy. Vytváření `JobService` je velmi podobný vytváření `Service` s Androidem framework: 

1. Rozšíření `JobService` třídy.
2. Uspořádání podtřídy s `ServiceAttribute` a nastavte `Name` parametr na řetězec, který se skládá z název balíčku a název třídy (viz následující příklad).
3. Nastavte `Permission` vlastnost `ServiceAttribute` řetězec `android.permission.BIND_JOB_SERVICE`.
4. Přepsání `OnStartJob` metoda, přidání kódu k provedení práce. Android bude volat tuto metodu na hlavní vlákno aplikace a spustit úlohu. Práci, kterou bude trvat déle, že je třeba provést několik milisekund na vlákno k zabránění blokování aplikace.
5. Při práci, `JobService` musí volat `JobFinished` metoda. Tato metoda je jak `JobService` informuje `JobScheduler` , se provádí pracovní. Selhání volání `JobFinished` v důsledku toho `JobService` uvedení nepotřebné požadavky na zařízení, zkrátit dobu životnosti baterie. 
6. Je vhodné také přepsat `OnStopJob` metoda. Tato metoda je volána Android, když úloha se právě vypíná předtím, než se dokončí a poskytuje `JobService` s možností správně odstranění žádné prostředky. Tato metoda by měla vrátit `true` Pokud je potřeba změnit plán úlohy, nebo `false` Pokud není desireable znovu spustit úlohu.
   

Následující kód je příkladem nejjednodušším `JobService` pro aplikaci, asynchronně k práci pomocí TPL:

```csharp
[Service(Name = "com.xamarin.samples.downloadscheduler.DownloadJob", 
         Permission = "android.permission.BIND_JOB_SERVICE")]
public class DownloadJob : JobService
{
    public override bool OnStartJob(JobParameters jobParams)
    {            
        Task.Run(() =>
        {
            // Work is happening asynchronously
                      
            // Have to tell the JobScheduler the work is done. 
            JobFinished(jobParams, false);
        });

        // Return true because of the asynchronous work
        return true;  
    }

    public override bool OnStopJob(JobParameters jobParams)
    {
        // we don't want to reschedule the job if it is stopped or cancelled.
        return false; 
    }
}
```

### <a name="creating-a-jobinfo-to-schedule-a-job"></a>Vytváření informací o úloze k naplánování úlohy

Vytvoření aplikace Xamarin.Android není instance `JobService` přímo, místo toho se předají `JobInfo` do objektu `JobScheduler`. `JobScheduler` Vytvoří instanci požadované `JobService` objekt, plánování a spouštění `JobService` podle metadat v `JobInfo`. A `JobInfo` objekt musí obsahovat tyto informace:

* **JobId** &ndash; jedná se `int` hodnotu, která se používá k identifikaci úlohy `JobScheduler`. Opětovné použití této hodnoty bude aktualizovat všechny existující úlohy. Hodnota musí být jedinečný pro danou aplikaci. 
* **JobService** &ndash; tento parametr je `ComponentName` identifikující explicitně typu, `JobScheduler` využít ke spuštění úlohy. 
  

Tato metoda rozšíření ukazuje, jak vytvořit `JobInfo.Builder` s Android `Context`, jako je například aktivita:

```csharp
public static class JobSchedulerHelpers
{
    public static JobInfo.Builder CreateJobBuilderUsingJobId<T>(this Context context, int jobId) where T:JobService
    {
        var javaClass = Java.Lang.Class.FromType(typeof(T));
        var componentName = new ComponentName(context, javaClass);
        return new JobInfo.Builder(jobId, componentName);
    }
}

// Sample usage - creates a JobBuilder for a DownloadJob andsets the Job ID to 1.
var jobBuilder = this.CreateJobBuilderUsingJobId<DownloadJob>(1);

var jobInfo = jobBuilder.Build();  // creats a JobInfo object.
```

Výkonná funkce systému Android Plánovač úloh je možné řídit při spuštění úlohy nebo za jakých podmínky úlohu může spustit. Následující tabulka popisuje některé metody v `JobInfo.Builder` které umožňují aplikace k ovlivnění při úlohu můžete spustit:  


|  Metoda | Popis   |
|---|---|
| `SetMinimumLatency`  | Určuje, že ke zpoždění (v milisekundách), který má být dodržen před úlohu spustit. |
| `SetOverridingDeadline`  | Deklaruje se, že úloha musí spustit před uplynutí této doby (v milisekundách). |
| `SetRequiredNetworkType`  | Určuje požadavky sítě pro úlohu. |
| `SetRequiresBatteryNotLow` | Úloha může spustit pouze pokud zařízení není uživateli zobrazit upozornění "nízký stav baterie". |
| `SetRequiresCharging` | Úloha může spustit pouze při nabíjení. |
| `SetDeviceIdle` | Úloha se bude spouštět, když je zařízení zaneprázdněný. |
| `SetPeriodic` | Určuje, že by měl být pravidelně úloha. |
| `SetPersisted` | Úloha má perisist mezi jednotlivými restartováními zařízení. | 


`SetBackoffCriteria` Obsahuje některé pokyny, jak dlouho je `JobScheduler` má čekat před pokusem o znovu spustit úlohu. Kritéria omezení rychlosti skládá ze dvou částí: zpoždění v milisekundách (výchozí hodnotu 30 sekund) a typ regrese, který se má použít (někdy označovány jako _zásady omezení rychlosti_ nebo _zásady opakování_) . Dvě zásady, které jsou zapouzdřené v `Android.App.Job.BackoffPolicy` výčtu:

* `BackoffPolicy.Exponential` &ndash; Zásadu exponenciálního omezení rychlosti se zvýší hodnota omezení rychlosti počáteční exponenciálnímu po každé selhání. Při prvním úlohy nezdaří, vyčká knihovny počáteční interval, který je zadán před změnou plánu úlohy – příklad 30 sekund. Ještě jednou, v případě selhání úlohy knihovny počkejte alespoň 60 sekund, než se pokusíte spustit úlohu. Po třetí neúspěšný pokus o, knihovny bude čekat 120 sekundách a tak dále. Jedná se o výchozí hodnotu.
* `BackoffPolicy.Linear` &ndash; Tuto strategii je lineární omezení rychlosti, která úloha by ho přeplánovat na spuštění v nastavených intervalech (dokud neproběhne úspěšně). Lineární omezení rychlosti je nejvhodnější pro práci, kterou musí dokončit co nejdříve nebo problémy, které se rychle vyřešit samy. 

Pro další informace o vytvoření `JobInfo` objekt, přečtěte si prosím [Google dokumentaci `JobInfo.Builder` – třída](https://developer.android.com/reference/android/app/job/JobInfo.Builder.html).

#### <a name="passing-parameters-to-a-job-via-the-jobinfo"></a>Předávání parametrů do úlohy prostřednictvím informací o úloze

Parametry jsou předány do úlohy tak, že vytvoříte `PersistableBundle` předá spolu s `Job.Builder.SetExtras` metoda:

```csharp
var jobParameters = new PersistableBundle();
jobParameters.PutInt("LoopCount", 11);

var jobBuilder = this.CreateJobBuilderUsingJobId<DownloadJob>(1)
                     .SetExtras(jobParameters)
                     .Build();
```

`PersistableBundle` Je k němu přistupovat z `Android.App.Job.JobParameters.Extras` vlastnost `OnStartJob` metodu `JobService`:

```csharp
public override bool OnStartJob(JobParameters jobParameters)
{
    var loopCount = jobParams.Extras.GetInt("LoopCount", 10);
    
    // rest of code omitted
} 
```

### <a name="scheduling-a-job"></a>Plánování úlohy

Pokud chcete naplánovat úlohu, bude aplikace pro Xamarin.Android získat odkaz na `JobScheduler` system a volání `JobScheduler.Schedule` metoda s `JobInfo` objekt, který byl vytvořen v předchozím kroku. `JobScheduler.Schedule` Vrátí okamžitě s jedním ze dvou hodnot celé číslo:

* **JobScheduler.ResultSuccess** &ndash; úloha už byla úspěšně naplánovat. 
* **JobScheduler.ResultFailure** &ndash; nešlo naplánovat úlohu. To je obvykle způsobena konfliktní `JobInfo` parametry.

Tento kód je příkladem plánování úlohy a upozornění uživatele výsledků pokusu o plánování:

```csharp
var jobScheduler = (JobScheduler)GetSystemService(JobSchedulerService);
var scheduleResult = jobScheduler.Schedule(jobInfo);

if (JobScheduler.ResultSuccess == scheduleResult)
{
    Snackbar.Make(FindViewById(Android.Resource.Id.Content), Resource.String.jobscheduled_success, Snackbar.LengthShort);
}
else
{
    Snackbar.Make(FindViewById(Android.Resource.Id.Content), Resource.String.jobscheduled_failure, Snackbar.LengthShort);
}
```
 
### <a name="cancelling-a-job"></a>Zrušení úlohy

Je možné zrušit všechny úlohy, které byly naplánovány nebo jednoduše jednu úlohu pomocí `JobsScheduler.CancelAll()` metoda nebo `JobScheduler.Cancel(jobId)` metoda:

```csharp
// Cancel all jobs
jobSchduler.CancelAll(); 

// to cancel a job with jobID = 1
jobScheduler.Cancel(1)
```
  
## <a name="summary"></a>Souhrn

Tato příručka popsané, jak používat Android plánovače úloh inteligentně provádět práce na pozadí. Je popsané postupy zapouzdření práce, kterou je možné provést, protože `JobService` a jak používat `JobScheduler` při plánování práce, zadáte kritéria s `JobTrigger` a způsob zpracování chyb s `RetryStrategy`.

## <a name="related-links"></a>Související odkazy

- [Inteligentní plánování úloh](https://developer.android.com/topic/performance/scheduling.html)
- [Referenční dokumentace rozhraní API JobScheduler](https://developer.android.com/reference/android/app/job/JobScheduler.html)
- [Plánování úloh, jako je aplikace pro s JobScheduler](https://medium.com/google-developers/scheduling-jobs-like-a-pro-with-jobscheduler-286ef8510129)
- [Android baterie a optimalizace paměti - Google vstupně-výstupních operací 2016 (video)](https://www.youtube.com/watch?v=VC2Hlb22mZM&feature=youtu.be)
- [Univerzity Xamarin Android JobScheduler - René země-](https://www.youtube.com/watch?v=aSjBBPYjelE)