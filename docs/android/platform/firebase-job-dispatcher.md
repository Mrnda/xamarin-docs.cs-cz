---
title: "Dispečer firebase úlohy"
description: "Tato příručka popisuje, jak při plánování práce na pozadí pomocí knihovny dispečera úloh Firebase z Google."
ms.topic: article
ms.prod: xamarin
ms.assetid: 3DB9C7A3-D351-481D-90C5-BEC25D1B9910
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/16/2018
ms.openlocfilehash: 6b55e525849d57f2ad9e40ea64b75cfc65ef0727
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 02/27/2018
---
# <a name="firebase-job-dispatcher"></a>Dispečer firebase úlohy

_Tato příručka popisuje, jak při plánování práce na pozadí pomocí knihovny dispečera úloh Firebase z Google._

## <a name="firebase-job-dispatcher-overview"></a>Přehled dispečera úloh firebase

Doporučené způsoby zachovat přizpůsobivý uživateli aplikace pro Android je zajistit, že komplexní nebo dlouhotrvající pracovních probíhá na pozadí. Je ale důležité, aby práce pozadí nebude negativní dopad na možnosti pro uživatele se zařízením. 

Úloha na pozadí například může dotazovat na web každých několik minut na dotaz pro změny konkrétní datové sady. Vypadá to neškodný, ale může mít katastrofální dopad na zařízení. Aplikace se ukončí probouzení zařízení, zvýšit procesoru na s vyšší spotřebou, napájený kombinace, provedení síťové požadavky a pak zpracování výsledků. Bude hůř, protože zařízení není ihned vypněte a vrátit do nečinného stavu snížené spotřeby. Práce špatně naplánované pozadí může nechtěně zachovat zařízení ve stavu s požadavky na nepotřebné a nadměrné spotřeby. Prakticky této seeming nevinnosti aktivity (dotazování web) bude nepoužitelná zařízení v poměrně krátké době.

Android již poskytuje několik rozhraní API usnadní provede práci na pozadí, ale žádná z nich jsou komplexní řešení:

* **[Záměrné služby](~/android/app-fundamentals/services/creating-a-service/intent-services.md)**  &ndash; záměr služby jsou skvěle hodí k provedení práce, ale poskytují způsob, jak plánování práce.
* **[AlarmManager](https://developer.android.com/reference/android/app/AlarmManager)**  &ndash; tyto rozhraní API pouze povolit, aby se naplánovaná, ale poskytují způsob, jak skutečně provést nějakou práci. Také AlarmManager umožňuje pouze čas na základě omezení, což znamená, že vyvolat alarm v určitém čase nebo po uplynutí určité doby. 
* **[JobScheduler](https://developer.android.com/reference/android/app/job/JobScheduler.html)**  &ndash; The plán úlohy je skvělé rozhraní API, která funguje s operačním systémem při plánování úloh. Je však pouze k dispozici pro tyto Android aplikací, které se zaměřují úroveň rozhraní API 21 nebo vyšší. 
* **[Vysílání příjemci](~/android/app-fundamentals/broadcast-receivers.md)**  &ndash; Android aplikace se dá nastavit všesměrového vysílání příjemci pro práci v reakci na události široké systému nebo tříd Intent. Ale všesměrového vysílání příjemci neposkytují žádné kontrolu nad spuštění úlohy. Také se omezí změny v operační systém Android když bude fungovat všesměrového vysílání příjemci nebo druhy práci, kterou může reagovat. 
* **Správce sítě zpráv cloudu Google** &ndash; po dlouhou dobu to bylo pravděpodobně, nejlepší způsob, jak inteligentně plán pozadí fungovat. Ale GCMNetworkManager od již nepoužívá. 

Existují dvě klíčové funkce efektivně provádět práce pozadí (někdy označovány jako _úloha na pozadí_ nebo _úlohy_):

1. **Inteligentně plánování práce** &ndash; je důležité, když aplikace pracuje na pozadí, dělá to tak, jako dobrý občanem. V ideálním případě by neměl aplikace potřebují spustit úlohu. Místo toho by měla aplikace zadejte podmínky, které musí být splněné při úlohu můžete spustit a pak plánování, které fungují spustit, když ke splnění podmínek. To umožňuje Android pro inteligentně práci. Například síťové požadavky může zpracovat v dávce ke spuštění všech ve stejnou dobu, aby maximální využití režijní náklady spojené s prací v síti.
2. **Zapouzdření prací** &ndash; kód k provedení práce na pozadí by měl být zapouzdřené v diskrétní komponenty, která lze spustit nezávisle na uživatelské rozhraní a bude je poměrně snadné ho přeplánovat práce selže-li dokončit z nějakého důvodu.

Dispečera Firebase úlohy je knihovna z Google, který poskytuje rozhraní fluent API zjednodušit plánování práce na pozadí. Je určena k nahrazení pro Google Cloud Manager být. Dispečera Firebase úlohy se skládá z následujících rozhraní API:

* A `Firebase.JobDispatcher.JobService` je abstraktní třída, která musí být rozšířena logiky, která se spustí v úlohu na pozadí.
* A `Firebase.JobDispatcher.JobTrigger` deklaruje při úlohy by měl být spuštěn. To je obvykle vyjádřený jako okno času, například, počkejte minimálně 30 sekund před spuštěním úlohy, ale spustit úlohu v rámci 5 minut.
* A `Firebase.JobDispatcher.RetryStrategy` obsahuje informace o co se má provést při úlohu nepodaří spustit správně. Strategie opakování určuje jak dlouho má systém čekat před pokusem o znovu spustit úlohu. 
* A `Firebase.JobDispatcher.Constraint` je volitelná hodnota, která popisuje podmínku, která je nutné splnit před spuštěním úlohy, jako je zařízení v síti neměřený nebo poplatků.
* `Firebase.JobDispatcher.Job` Je rozhraní API, který kombinuje předchozí rozhraní API v a jednotku z – práce, která lze naplánovat pomocí `JobDispatcher`. `Job.Builder` Třída se používá k vytváření instancí `Job`.
* A `Firebasee.JobDispatcher.JobDispatcher` používá předchozí tři rozhraní API plánování práce s operačním systémem a poskytnout způsob, jak zrušit úlohy, v případě potřeby.

Při plánování práce s dispečera úloh Firebase, aplikace pro Xamarin.Android musí zapouzdřovat kód na typ, který rozšiřuje `JobService` třídy. `JobService` má tři metody životního cyklu, která může být volána po dobu životnosti úlohy:

* **`bool OnStartJob(IJobParameters parameters)`** &ndash; Tato metoda je, kde dojde k práci a vždy by měla být implementována. Běží na hlavní vlákno. Tato metoda vrátí `true` Pokud je pracovní zbývající, nebo `false` Pokud práci. 
* **`bool OnStopJob(IJobParameters parameters)`** &ndash; Volá se, když je úloha zastavena z nějakého důvodu. Měla by vrátit `true` Pokud úloha by ho přeplánovat na později.
* **`JobFinished(IJobParameters parameters, bool needsReschedule)`** &ndash; Tato metoda je volána, když `JobService` dokončil všechna asynchronní práce. 

Pokud chcete naplánovat úlohu, vytvoří instanci aplikace `JobDispatcher` objektu. Potom `Job.Builder` se používá k vytvoření `Job` objekt, který zajišťuje `JobDispatcher` který, pokusí se naplánovat na spuštění úlohy.

Tato příručka popisuje postup přidání dispečera úloh Firebase do aplikace Xamarin.Android a použít ji k plánování práce pozadí.

## <a name="requirements"></a>Požadavky

Dispečera úloh Firebase vyžaduje úroveň rozhraní API systému Android 9 nebo vyšší. Knihovna Firebase úlohy dispečera spoléhá na některé součásti poskytované služby Google Play; zařízení musí mít Google Play Services nainstalována.

## <a name="using-the-firebase-job-dispatcher-library-in-xamarinandroid"></a>Použití knihovny dispečera úloh Firebase v Xamarin.Android

Chcete-li začít pracovat s dispečera Firebase úlohy, nejprve přidejte [balíček Xamarin.Firebase.JobDispatcher NuGet](https://www.nuget.org/packages/Xamarin.Firebase.JobDispatcher/0.6.0-beta1) do projektu Xamarin.Android. Správce balíčků NuGet pro vyhledávání **Xamarin.Firebase.Jobdispatcher** balíčku.  

Po přidání knihovně dispečera úloh Firebase, vytvoření `JobService` třídy a potom ji spustit s instancí naplánovat `FirebaseJobDispatcher`.

### <a name="creating-a-jobservice"></a>Vytváření `JobService`

Všechny pracovní provádí knihovně Firebase dispečera úloh je třeba provést v typu, který rozšiřuje `Firebase.JobDispatcher.JobService` abstraktní třídy. Vytváření `JobService` je velmi podobný vytváření `Service` s Androidem framework: 

1. Rozšíření `JobService` – třída
2. Uspořádání podtřídy s `ServiceAttribute`. I když není nezbytně nutné, doporučuje se explicitně nastavit `Name` parametru pomáhají s laděním `JobService`. 
3. Přidat `IntentFilter` deklarovat `JobService` v **AndroidManifest.xml**. Také to pomůže knihovně dispečera úloh Firebase vyhledejte a vyvolání `JobService`.

Následující kód je příkladem nejjednodušším `JobService` pro aplikaci:

```csharp
[Service(Name = "com.xamarin.fjdtestapp.DemoJob")]
[IntentFilter(new[] {FirebaseJobServiceIntent.Action})]
public class DemoJob : JobService
{
    static readonly string TAG = "X:DemoService";

    public override bool OnStartJob(IJobParameters jobParameters)
    {
        Log.Debug(TAG, "DemoJob::OnStartJob");
        // Note: This runs on the main thread. Anything that takes longer than 16 milliseconds
         // should be run on a seperate thread.
        
        return false; // return false because there is no more work to do.
    }

    public override bool OnStopJob(IJobParameters jobParameters)
    {
        Log.Debug(TAG, "DemoJob::OnStartJob");
        // nothing to do.
        return false;
    }
}
```

### <a name="creating-a-firebasejobdispatcher"></a>Vytváření `FirebaseJobDispatcher`

Předtím, než bude naplánována veškerou práci, je nutné vytvořit `Firebase.JobDispatcher.FirebaseJobDispatcher` objektu. `FirebaseJobDispatcher` Zodpovídá za plánování `JobService`. Následující fragment kódu je možné vytvořit instanci `FirebaseJobDispatcher`: 
 
 ```csharp
// This is the "Java" way to create a FirebaseJobDispatcher object
IDriver driver = new GooglePlayDriver(context);
FirebaseJobDispatcher dispatcher = new FirebaseJobDispatcher(driver);
```

V předchozím fragmentu kódu `GooglePlayDriver` je třída, která pomáhá `FirebaseJobDispatcher` interakci s některými plánování rozhraní API v služby Google Play v zařízení. Parametr `context` je všechny Android `Context`, například aktivitu. Aktuálně `GooglePlayDriver` je jedinou `IDriver` implementace v knihovně Firebase úlohy dispečera. 

Poskytuje metodu rozšíření k vytvoření vazby Xamarin.Android pro dispečera Firebase úlohy `FirebaseJobDispatcher` z `Context`: 

```csharp
FirebaseJobDispatcher dispatcher = context.CreateJobDispatcher();
```

Jednou `FirebaseJobDispatcher` byla vytvořena instance, je možné vytvořit `Job` a spouští kód v `JobService` – třída. `Job` Je vytvořený `Job.Builder` objektu a budou popsané v další části.

### <a name="creating-a-firebasejobdispatcherjob-with-the-jobbuilder"></a>Vytváření `Firebase.JobDispatcher.Job` pomocí `Job.Builder`

`Firebase.JobDispatcher.Job` Třída je odpovědná za zapouzdřením meta-data potřebné ke spuštění `JobService`. A`Job` obsahuje informace, například žádné omezení, které je nutné splnit před spuštěním úlohy, pokud `Job` je periodický, nebo žádné aktivační události, které způsobí, že tato úloha se spustila.  Úplné alespoň `Job` musí mít _značky_ (jedinečného řetězce, který identifikuje úlohy `FirebaseJobDispatcher`) a typ `JobService` by měl být spuštěn. Vytvoří instanci dispečera úloh Firebase `JobService` když je na čase spuštění úlohy.  A `Job` je vytvořená pomocí instance `Firebase.JobDispatcher.Job.JobBuilder` třídy. 

Následující fragment kódu je nejjednodušší příklad toho, jak vytvořit `Job` pomocí vazby Xamarin.Android:

```csharp
Job myJob = dispatcher.NewJobBuilder()
                      .SetService<DemoJob>("demo-job-tag")
                      .Build();
```

`Job.Builder` Provádět některé základní ověřovací kontroly na vstupní hodnoty pro úlohu. Bude vyvolána výjimka, pokud není možné ji `Job.Builder` k vytvoření `Job`.  `Job.Builder` Vytvoří `Job` s následující výchozí nastavení:

* A `Job`na _životnost_ (jak dlouho naplánuje spuštění) je jenom dokud se zařízení restartuje &ndash; po restartování zařízení `Job` dojde ke ztrátě.
* A `Job` není opakovaného &ndash; ji lze spustit pouze jednou.
* A `Job` bude naplánována s co nejdříve spustit.
* Výchozí strategie opakování pro `Job` , je použít _exponenciálního omezení rychlosti_ (popsané v podrobněji níže v části [nastavení RetryStrategy](#Setting_a_RestryStrategy))

### <a name="scheduling-a-job"></a>Plánování `Job`

Po vytvoření `Job`, je nutné naplánovat `FirebaseJobDispatcher` před jeho spuštěním. Existují dvě metody pro plánování `Job`:

```csharp
// This will throw an exception if there was a problem scheduling the job
dispatcher.MustSchedule(myJob);

// This method will not throw an exception; an integer result value is returned
int scheduleResult = dispatcher.Schedule(myJob);
```

Hodnoty vrácené `FirebaseJobDispatcher.Schedule` bude mít jednu z následujících hodnot celé číslo:

* `FirebaseJobDispatcher.ScheduleResultSuccess` &ndash; `Job` Úspěšně se naplánovala.
* `FirebaseJobDispatcher.ScheduleResultUnknownError` &ndash; Neznámý problém došlo k chybě, která zabránila `Job` z naplánován.
* `FirebaseJobDispatcher.ScheduleResultNoDriverAvailable` &ndash; Neplatný `IDriver` byl použit nebo `IDriver` nějakým způsobem nebylo k dispozici. 
* `FirebaseJobDispatcher.ScheduleResultUnsupportedTrigger` &ndash; `Trigger` Nebyla podporovaná.
* `FirebaseJobDispatcher.ScheduleResultBadService` &ndash; Služba není správně nakonfigurovaná nebo není k dispozici.
 
### <a name="configuring-a-job"></a>Konfigurace projektu

Je možné přizpůsobit úlohu. Příklady, jak lze přizpůsobit úlohu zahrnují následující:

* [Předat parametry do úlohy](#Passing_Parameters_to_a_Job) &ndash; A `Job` může vyžadovat další hodnoty k provedení práce, například stažení souboru.
* [Nastavte omezení](#Setting_Constraints) &ndash; může být nutné jenom spustit úlohu při splnění určitých podmínek. Například spusťte pouze `Job` při ukládání zařízení. 
* [Určete, kdy `Job` měly být spuštěny](#Setting_Job_Triggers) &ndash; The dispečera úloh Firebase umožňuje aplikacím určit čas, kdy má být úloha spuštěna.  
* [Deklarovat strategie opakování pro neúspěšné úlohy](#Setting_a_RetryStrategy) &ndash; A _se strategie opakování při_ obsahuje pokyny, které `FirebaseJobDispatcher` co dělat s `Jobs` , se nepodařilo dokončit. 

Každý z těchto témat se být uvedeny další informace v následujících částech.

#### <a name="passing-parameters-to-a-job"></a>Předávání parametrů do úlohy

Parametry jsou předány do úlohy tak, že vytvoříte `Bundle` předá spolu s `Job.Builder.SetExtras` metoda:

```csharp
Bundle jobParameters = new Bundle();
jobParameters.PutInt(FibonacciCalculatorJob.FibonacciPositionKey, 25);

Job myJob = dispatcher.NewJobBuilder()
                      .SetService<DemoJob>("demo-job-tag")
                      .SetExtras(jobParameters)
                      .Build();

```

`Bundle` Je k němu přistupovat z `IJobParameters.Extras` vlastnost `OnStartJob` metoda:

```csharp
public override bool OnStartJob(IJobParameters jobParameters)
{
    int position = jobParameters.Extras.GetInt(FibonacciPositionKey, DEFAULT_VALUE);
    
    // rest of code omitted
} 
```


#### <a name="setting-constraints"></a>Nastavení omezení

Omezení může pomoci snižuje náklady a baterie vyprazdňování na zařízení. `Firebase.JobDispatcher.Constraint` Třída definuje jako celé číslo hodnoty těchto omezení:

* `Constraint.OnUnmeteredNetwork` &ndash; Spusťte úlohu, pouze když je zařízení připojené k síti neměřený. To je užitečné, abyste zabránili nabíhání poplatků za data uživatele.
* `Constraint.OnAnyNetwork` &ndash; Spusťte úlohu v síti, bez ohledu zařízení je připojené k. Pokud je zadaný společně s `Constraint.OnUnmeteredNetwork`, bude mít prioritu, tato hodnota.
* `Constraint.DeviceCharging` &ndash; Spusťte úlohu jenom v případě, že zařízení je poplatků.

Omezení se nastavují s `Job.Builder.SetConstraint` metoda: 

```csharp
Job myJob = dispatcher.NewJobBuilder()
                      .SetService<DemoJob>("demo-job-tag")
                      .SetConstraint(Constraint.DeviceCharging)
                      .Build();
```

#### <a name="setting-job-triggers"></a>Nastavení úloh aktivační události

`JobTrigger` Obsahuje pokyny, které operační systém o zahájení úlohy. A `JobTrigger` má _provádění okno_ , který definuje naplánovaném čase, kdy `Job` měly být spuštěny. Provádění okna _spustit okno_ hodnotu a _end okno_ hodnotu. Okno spuštění je počet sekund, po který zařízení čekat před spuštěním úlohy a koncová hodnota okno je maximální počet sekund čekání před spuštěná `Job`. 

A `JobTrigger` lze vytvořit pomocí `Firebase.Jobdispatcher.Trigger.ExecutionWindow` metoda.  Například `Trigger.ExecutionWindow(15,60)` znamená, že by měl od 15 do 60 sekund od kdy je naplánováno spuštění úlohy. `Job.Builder.SetTrigger` Metoda se používá ke 

```csharp
JobTrigger myTrigger = Trigger.ExecutionWindow(15,60);
Job myJob = dispatcher.NewJobBuilder()
                      .SetService<DemoJob>("demo-job-tag")
                      .SetTrigger(myTrigger)
                      .Build();
```

Výchozí hodnota `JobTrigger` pro úlohy je reprezentována hodnota `Trigger.Now`, která určuje, zda úlohy spustit co nejdříve po naplánován...

#### <a name="setting-a-retrystrategy"></a>Nastavení RetryStrategy

`Firebase.JobDispatcher.RetryStrategy` Slouží k určení, kolik zpoždění zařízení, používejte než to zkusíte znovu spusťte neúspěšnou úlohu. A `RetryStrategy` má _zásad_, která definuje, jaké základní doba algoritmus se použije znovu naplánovat úlohu došlo k chybě a provádění okno, které určuje okno, ve kterém má být naplánováno úlohy. To _přeplánování okno_ je definována dvě hodnoty. První hodnota je počet sekund čekání před změnou plánu úlohy ( _počáteční omezení rychlosti_ hodnotu), a druhé číslo, které je maximální počet sekund, než se musí spustit úlohu ( _maximální omezení rychlosti_hodnotu). 
 
Tyto hodnoty int jsou identifikovány dva typy zásady opakování:

* `RetryStrategy.RetryPolicyExponential` &ndash; _Exponenciálního omezení rychlosti_ zásad zvýší hodnota omezení rychlosti počáteční exponenciálnímu po každé selhání. Na první spuštění úlohy nezdaří, vyčká knihovny _initial interval, který je zadán před změnou plánu úlohy &ndash; příklad 30 sekund. Ještě jednou, v případě selhání úlohy knihovny počkejte alespoň 60 sekund, než se pokusíte spustit úlohu. Po třetí neúspěšný pokus o, knihovny bude čekat 120 sekundách a tak dále. Výchozí hodnota `RetryStrategy` pro dispečera Firebase úlohy je reprezentována knihovny `RetryStrategy.DefaultExponential` objektu. Má počáteční omezení rychlosti 30 sekund a maximální omezení rychlosti 3600 vteřin.
* `RetryStrategy.RetryPolicyLinear` &ndash; Tuto strategii je _lineární omezení rychlosti_ , úloha by ho přeplánovat na spuštění při nastavit intervaly (dokud neproběhne úspěšně). Lineární omezení rychlosti je nejvhodnější pro práci, kterou musí dokončit co nejdříve nebo problémy, které se rychle vyřešit samy. Definuje knihovně dispečera úloh Firebase `RetryStrategy.DefaultLinear` které má přeplánování okno nejméně 30 sekund a až 3 600 sekund.

Je možné definovat vlastní `RetryStrategy` s `FirebaseJobDispatcher.NewRetryStrategy` metoda. Trvá tři parametry:

1. `int policy` &ndash; _Zásad_ je jedním z předchozí `RetryStrategy` hodnoty, `RetryStrategy.RetryPolicyLinear`, nebo `RetryStrategy.RetryPolicyExponential`.
2. `int intialBackoffSeconds` &ndash; _Počáteční omezení rychlosti_ je zpoždění, v sekundách, po které je nutné před dalším pokusem o znovu spustit úlohu. Výchozí hodnota je 30 sekund. 
3. `int maximumBackoffSeconds` &ndash; _Maximální omezení rychlosti_ hodnota deklaruje maximální počet sekund se má počkat, než se pokusíte znovu spustit úlohu. Výchozí hodnota je 3 600 sekund. 

```csharp
RetryStrategy retry = dispatcher.NewRetryStrategy(RetryStrategy.RetryPolicyLinear, initialBackoffSeconds, maximumBackoffSet);

// Create a Job and set the RetryStrategy via the Job.Builder
Job myJob = dispatcher.NewJobBuilder()
                      .SetService<DemoJob>("demo-job-tag")
                      .SetRetryStrategy(retry)
                      .Build();
```

### <a name="cancelling-a-job"></a>Zrušení úlohy

Je možné zrušit všechny úlohy, které byly naplánovány nebo jednoduše jednu úlohu pomocí `FirebaseJobDispatcher.CancelAll()` metoda nebo `FirebaseJobDispatcher.Cancel(string)` metoda:

```csharp
int cancelResult = dispatcher.CancelAll(); 

// to cancel a single job:

int cancelResult = dispatcher.Cancel("unique-tag-for-job");
```

Buď metoda vrátí celočíselnou hodnotu:

* `FirebaseJobDispatcher.CancelResultSuccess` &ndash; Úloha byla úspěšně zrušena.
* `FirebaseJobDispatcher.CancelResultUnknownError` &ndash; Chyba zabránila úlohu ruší.
* `FirebaseJobDispatcher.CancelResult.NoDriverAvailable` &ndash; `FirebaseJobDispatcher` Se nepodařilo zrušit úlohu, protože neexistuje žádná platná `IDriver` k dispozici.

## <a name="summary"></a>Souhrn

Tato příručka popsané, jak používat dispečera úloh Firebase inteligentně provádět práce na pozadí. Ho popsané postupy zapouzdření práce, kterou je možné provést, protože `JobService` a jak `FirebaseJobDispatcher` při plánování práce, zadáte kritéria s `JobTrigger` a způsob zpracování chyb s `RetryStrategy`.


## <a name="related-links"></a>Související odkazy

- [Xamarin.Firebase.JobDispatcher na NuGet](https://www.nuget.org/packages/Xamarin.FirebaseJobDispatcher)
- [Úloha dispečera firebase na Githubu](https://github.com/firebase/firebase-jobdispatcher-android)
- [Xamarin.Firebase.JobDispatcher Binding](https://github.com/xamarin/XamarinComponents/tree/master/Android/FirebaseJobDispatcher)
- [Inteligentní plánování úloh](https://developer.android.com/topic/performance/scheduling.html)
- [Android baterie a optimalizace paměti - Google vstupně-výstupních operací 2016 (video)](https://www.youtube.com/watch?v=VC2Hlb22mZM&feature=youtu.be)
