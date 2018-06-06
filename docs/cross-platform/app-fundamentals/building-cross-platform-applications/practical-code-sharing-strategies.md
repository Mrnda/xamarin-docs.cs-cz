---
title: Část 5 - praktické kód sdílení strategie
description: Tento dokument popisuje praktická kód sdílení strategie pro scénáře, jako je databáze, přístup k souborům, síťových operací a asynchronní kódu.
ms.prod: xamarin
ms.assetid: 328D042A-FF78-A7B6-1574-B5AF49A1AADB
author: asb3993
ms.author: amburns
ms.date: 03/23/2017
ms.openlocfilehash: d912f93025fb1b9bc511c1aeab9040dc1acf0e48
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 06/05/2018
ms.locfileid: "34781689"
---
# <a name="part-5---practical-code-sharing-strategies"></a>Část 5 - praktické kód sdílení strategie

V této části jsou uvedené příklady jak sdílet kód pro běžné scénáře aplikace.



## <a name="data-layer"></a>Datová vrstva

Datová vrstva se skládá z modulu úložiště a metody pro čtení a zápisu informací. Z důvodů výkonu, flexibilitu a napříč platformami kompatibility SQLite databázový stroj se doporučuje pro aplikace Xamarin napříč platformami.
Běží na širokou škálu platforem včetně systému Windows, Android, iOS a macu.


### <a name="sqlite"></a>SQLite

SQLite je implementace databáze open source. Zdroj a dokumentaci najdete na [SQLite.org](http://www.sqlite.org/). SQLite podpora je k dispozici na jednotlivých mobilních platformách:

-  **iOS** – součástí operačního systému.
- **Android** – součástí operačního systému od Android 2.2 (API úrovně 10).
- **Windows** – najdete v článku [SQLite pro univerzální platformu Windows rozšíření](https://visualstudiogallery.msdn.microsoft.com/4913e7d5-96c9-4dde-a1a1-69820d615936).


I když databázový stroj dostupné na všech platformách nativní metody pro přístup k databázi se liší. Jak iOS a Android nabízí integrované rozhraní API pro přístup k SQLite, který by bylo možné použít z Xamarin.iOS nebo Xamarin.Android, ale pomocí nativní metody SDK nabízí žádná možnost sdílet kódu (jiné než možná dotazy SQL sami, za předpokladu, že jsou uloženy jako řetězce) . Podrobnosti o nativní databázový funkce hledání `CoreData` v iOS nebo Android pro `SQLiteOpenHelper` třídy, protože tyto možnosti nejsou napříč platformami jsou nad rámec tohoto dokumentu.



### <a name="adonet"></a>ADO.NET

Podpora Xamarin.iOS a Xamarin.Android `System.Data` a `Mono.Data.Sqlite` (viz Xamarin.iOS [dokumentace](~/ios/data-cloud/system.data.md) Další informace).
Pomocí těchto oborů názvů, můžete napsat kód ADO.NET, který funguje na obou platformách. Upravit odkazů projektu zahrnout `System.Data.dll` a `Mono.Data.Sqlite.dll` a přidejte tyto příkazy using do vašeho kódu:

```csharp
using System.Data;
using Mono.Data.Sqlite;
```

Následující vzorový kód bude fungovat:

```csharp
string dbPath = Path.Combine (
        Environment.GetFolderPath (Environment.SpecialFolder.Personal),
        "items.db3");
bool exists = File.Exists (dbPath);
if (!exists)
    SqliteConnection.CreateFile (dbPath);
var connection = new SqliteConnection ("Data Source=" + dbPath);
connection.Open ();
if (!exists) {
    // This is the first time the app has run and/or that we need the DB.
    // Copy a "template" DB from your assets, or programmatically create one like this:
    var commands = new[]{
        "CREATE TABLE [Items] (Key ntext, Value ntext);",
        "INSERT INTO [Items] ([Key], [Value]) VALUES ('sample', 'text')"
    };
    foreach (var command in commands) {
        using (var c = connection.CreateCommand ()) {
            c.CommandText = command;
            c.ExecuteNonQuery ();
        }
    }
}
// use `connection`... here, we'll just append the contents to a TextView
using (var contents = connection.CreateCommand ()) {
    contents.CommandText = "SELECT [Key], [Value] from [Items]";
    var r = contents.ExecuteReader ();
    while (r.Read ())
        Console.Write("\n\tKey={0}; Value={1}",
                r ["Key"].ToString (),
                r ["Value"].ToString ());
}
connection.Close ();
```

Implementace reálného ADO.NET by samozřejmě rozdělí mezi různými způsoby a třídy (v tomto příkladu je pouze pro demonstrační účely).



### <a name="sqlite-net--cross-platform-orm"></a>SQLite-NET – ORM různé platformy

ORM (nebo objekt relační Mapper) se pokouší zjednodušit úložiště dat modelován v třídách. Spíše než ručně zápis dotazů SQL, že vytvoření tabulky nebo vyberte, INSERT a DELETE dat, které se extrahují ručně z třídy pole a vlastnosti, ORM přidává další vrstvu kód, který pro sebe. Pomocí reflexe k prozkoumání struktury tříd, ORM můžete automaticky vytvářet tabulky a sloupce, které odpovídají třídu a vytvářet dotazy číst a zapisovat data. To umožňuje kódu aplikace za účelem jednoduše odesílat a přijímat instance, která ORM, který se stará o všechny operace SQL pod pokličkou.

SQLite NET funguje jako jednoduchý ORM, který vám umožní uložit a načíst vaše třídy v SQLite. Skryje složitosti pro různé platformy SQLite přístup s kombinací direktivy kompilátoru a další triky.

Funkce SQLite NET:

-  Tabulky jsou definovány přidání atributů do třídy modelu.
-  Instance databáze je reprezentována podtřídou třídy `SQLiteConnection` , hlavní třídy v knihovně SQLite Net.
-  Data můžete vložit, předmětem dotazu a odstranit pomocí objektů. Žádné příkazy SQL se vyžadují (i když můžete zápisu příkazů SQL, v případě potřeby).
-  Základní dotazů Linq lze provést na kolekcích, vrácený SQLite NET.


Zdrojový kód a dokumentace pro SQLite NET je k dispozici na [SQLite Net na githubu](https://github.com/praeclarum/sqlite-net) a nebyla implementovaná v obou případové studie. Jednoduchý příklad kódu SQLite NET (z *Tasky Pro* Případová studie) jsou uvedeny níže.

Nejdřív `TodoItem` třída používá k definování pole jako primární klíč databázového atributy:

```csharp
public class TodoItem : IBusinessEntity
{
    public TodoItem () {}
    [PrimaryKey, AutoIncrement]
    public int ID { get; set; }
    public string Name { get; set; }
    public string Notes { get; set; }
    public bool Done { get; set; }
}
```

To umožňuje `TodoItem` tabulky má být vytvořena s následující řádek kódu (a žádné příkazy SQL) na `SQLiteConnection` instance:

```csharp
CreateTable<TodoItem> ();
```

Data v tabulce můžete také manipulovat s jinými metodami na `SQLiteConnection` (znovu, aniž by příkazů SQL):

```csharp
Insert (TodoItem); // 'task' is an instance with data populated in its properties
Update (TodoItem); // Primary Key field must be populated for Update to work
Table<TodoItem>.ToList(); // returns all rows in a collection
```

Najdete v článku Případová studie zdrojový kód pro dokončení příklady.



## <a name="file-access"></a>Přístup k souborům

Přístup k souborům je určitě klíčovou součástí všech aplikací. Běžných příkladů souborů, které může být součástí vkládaném aplikace:

-  Soubory databáze SQLite.
-  Uživatelem generovaný data (text, obrázky, zvuk, video).
-  Stažená data pro ukládání do mezipaměti (obrázky, html nebo soubory PDF).




### <a name="systemio-direct-access"></a>System.IO přímý přístup

Xamarin.iOS a Xamarin.Android povolit používání tříd v přístupu k systému souborů `System.IO` oboru názvů.

Každou platformu mají různý přístup omezení, které musí vzít v úvahu:

-  aplikace pro iOS spustit v izolovaném prostoru s přístupem k velmi omezený systému souborů. Další Apple Určuje, jak by měla použít systém souborů tak, že zadáte určité umístění, které jsou zálohované (a ostatních, které nejsou). Odkazovat [práce s systém souborů ve Xamarin.iOS](~/ios/app-fundamentals/file-system.md) průvodce další podrobnosti.
-  Android také omezuje přístup k určitým adresářům týkající se aplikace, ale také podporuje externí média (např. SD karty) a přístupu k sdíleným datům.
-  Windows Phone 8 (Silverlight) neumožňují přístup k souborům přímé – soubory můžete upravit pouze pomocí `IsolatedStorage`.
-  Projekty WinRT Windows 8.1 a Windows 10 UWP nabízejí pouze operace asynchronní souboru prostřednictvím `Windows.Storage` rozhraní API, které se liší od jiných platforem.

#### <a name="example-for-ios-and-android"></a>Příklad pro iOS a Android

Jednoduchý příklad, který zapíše a čtení textového souboru jsou uvedeny níže.
Pomocí `Environment.GetFolderPath` umožňuje stejný kód ke spuštění na iOS a Android, které každý vrací platný adresář podle jejich názvů systému souborů.

```csharp
string filePath = Path.Combine (
        Environment.GetFolderPath (Environment.SpecialFolder.Personal),
        "MyFile.txt");
System.IO.File.WriteAllText (filePath, "Contents of text file");
Console.WriteLine (System.IO.ReadAllText (filePath));
```

Odkazovat Xamarin.iOS [práce pomocí systému souborů](~/ios/app-fundamentals/file-system.md) dokumentu pro další informace o funkci filesystem specifické pro iOS. Při psaní kódu pro různé platformy souboru přístup, mějte na paměti, že některé systémy souborů malých a velkých písmen a mít jiný adresář oddělovačů. Je dobrým zvykem vždy nutné použít stejné velká a malá písmena pro názvy souborů a `Path.Combine()` metoda při vytváření cesty k souboru nebo adresáře.



### <a name="windowsstorage-for-windows-8-and-windows-10"></a>Windows.Storage pro Windows 8 a Windows 10

*Vytváření mobilních aplikací s Xamarin.Forms* [kniha](https://developer.xamarin.com/r/xamarin-forms/book/)
[kapitoly 20. Async a vstupně-výstupní soubor](https://developer.xamarin.com/r/xamarin-forms/book/chapter20.pdf) zahrnuje [ukázky pro Windows 8.1 a Windows 10](https://github.com/xamarin/xamarin-forms-book-preview-2/tree/master/Chapter20).

Použití [ `DependencyService` ](~/xamarin-forms/app-fundamentals/dependency-service/index.md) je možné ke čtení a soubor soubory na těchto platformách pomocí podporovaných rozhraní API:

```csharp
StorageFolder localFolder = ApplicationData.Current.LocalFolder;
IStorageFile storageFile = await localFolder.CreateFileAsync("MyFile.txt",
                                        CreationCollisionOption.ReplaceExisting);
await FileIO.WriteTextAsync(storageFile, "Contents of text file");
```

Odkazovat [kapitoly knihy](https://developer.xamarin.com/r/xamarin-forms/book/chapter20.pdf) další podrobnosti.


<a name="Isolated_Storage" />

### <a name="isolated-storage-on-windows-phone-7--8-silverlight"></a>Izolovaná úložiště na Windows Phone 7 a 8 (Silverlight)

Izolované úložiště je společné rozhraní API pro ukládání a načítání souborů ve všech iOS, Android a starší platformy Windows Phone.

Je výchozí mechanismus pro přístup k souborům ve Windows Phone (Silverlight), která nebyla implementovaná v Xamarin.iOS a Xamarin.Android povolit společný kód přístup k souborům k zapsání. `System.IO.IsolatedStorage` Třída může být odkazováno pro všechny tři platformy v [sdílený projekt](~/cross-platform/app-fundamentals/shared-projects.md).

Odkazovat [izolované úložiště – přehled pro Windows Phone](http://msdn.microsoft.com/library/windowsphone/develop/ff402541(v=vs.105).aspx) Další informace.

Rozhraní API izolované úložiště nejsou k dispozici v [přenosné knihovny tříd](~/cross-platform/app-fundamentals/pcl.md). Jeden alternativou k PCL je [PCLStorage NuGet](https://pclstorage.codeplex.com/)



### <a name="cross-platform-file-access-in-pcls"></a>Přístup k souborům a platformy v PCLs

Je také kompatibilní PCL Nuget – [PCLStorage](https://www.nuget.org/packages/PCLStorage/) – tento přístup k souborům a platformy zařízení pro Xamarin podporované platformy a nejnovější rozhraní API systému Windows.


## <a name="network-operations"></a>Síťové operace

Většina mobilní aplikace bude mít síťové součásti, například:

-  Stažení bitové kopie, videa a zvuku (např. miniatury, fotografie, Hudba).
-  Stahování dokumentů (např. HTML, PDF).
-  Odesílání dat uživatele (například fotografie nebo text).
-  Přístup k webovým službám nebo 3. stran rozhraní API (včetně SOAP, XML nebo JSON).


Rozhraní .NET Framework poskytuje několik různých tříd pro přístup k síťovým prostředkům: `HttpClient`, `WebClient`, a `HttpWebRequest`.

### <a name="httpclient"></a>HttpClient

`HttpClient` Třídy v `System.Net.Http` obor názvů je k dispozici v Xamarin.iOS, Xamarin.Android a většina platformy systému Windows. Je [Nuget knihovny klienta HTTP Microsoft](https://www.nuget.org/packages/Microsoft.Net.Http/) který slouží k zajištění toto rozhraní API knihovny přenosných tříd (a Windows Phone 8 Silverlight).

```csharp
var client = new HttpClient();
var request = new HttpRequestMessage(HttpMethod.Get, "https://xamarin.com");
var response = await myClient.SendAsync(request);
```

### <a name="webclient"></a>Webový klient

`WebClient` Třída poskytuje jednoduché rozhraní API načíst vzdálená data ze vzdálených serverů.

Operace Universal Windows Platofrm *musí* být asynchronní, i když Xamarin.iOS a Xamarin.Android podporují synchronní operace (které lze provést na vlákna na pozadí).

Kód pro jednoduché asychronous `WebClient` operace je:

```csharp
var webClient = new WebClient ();
webClient.DownloadStringCompleted += (sender, e) =>
{
    var resultString = e.Result;
    // do something with downloaded string, do UI interaction on main thread
};
webClient.Encoding = System.Text.Encoding.UTF8;
webClient.DownloadStringAsync (new Uri ("http://some-server.com/file.xml"));
```

 `WebClient` má také `DownloadFileCompleted` a `DownloadFileAsync` pro načítání binární data.

<a name="HttpWebRequest" />

### <a name="httpwebrequest"></a>HttpWebRequest

`HttpWebRequest` nabízí další přizpůsobení než `WebClient` a v důsledku vyžaduje další kódu pro použití.

Kód pro jednoduchou synchronní `HttpWebRequest` operace je:

```csharp
var request = HttpWebRequest.Create(@"http://some-server.com/file.xml ");
request.ContentType = "text/xml";
request.Method = "GET";
using (HttpWebResponse response = request.GetResponse() as HttpWebResponse)
{
    if (response.StatusCode != HttpStatusCode.OK)
        Console.WriteLine("Error fetching data. Server returned status code: {0}", response.StatusCode);
    using (StreamReader reader = new StreamReader(response.GetResponseStream()))
    {
        var content = reader.ReadToEnd();
        // do something with downloaded string, do UI interaction on main thread
    }
}
```

Zde je příklad v našich [webové služby dokumentaci](~/cross-platform/data-cloud/web-services/index.md).

 <a name="Reachability" />


### <a name="reachability"></a>Dostupnosti

Mobilní zařízení fungovat v různých síťových podmínkách z rychlé Wi-Fi nebo 4G připojení k nízký recepce a pomalých připojení dat OKRAJ. Z toho důvodu je dobrým zvykem zjistit, zda síť není k dispozici, a pokud ano, jaký typ sítě je k dispozici, před pokusem o připojení k vzdálené servery.

Činnosti, mobilní aplikace může trvat v těchto situacích:

-  Pokud síť není k dispozici, seznámit uživatele. Pokud se ručně zakázáno (např. Režim v letadle nebo vypnutí Wi-Fi) pak lze problém vyřešit.
-  Pokud je připojení 3G, může aplikace chovat jinak (například Apple neumožňuje aplikace větší než 20Mb stáhnout více než 3G). Aplikace může tyto informace slouží k uživatele varuje ohledně nadměrné stažení případech, kdy načítání velkých souborů.
-  I v případě, že síť není k dispozici, je dobrým zvykem k ověření připojení s cílovým serverem před zahájením dalších požadavků. To zabrání síťových operací aplikace z vypršení časového limitu opakovaně a také povolit informativnější chybová zpráva, který se má zobrazit uživateli.


Je [Xamarin.iOS ukázkové](https://github.com/xamarin/monotouch-samples/tree/master/ReachabilitySample) k dispozici (která je založena na společnosti Apple [dostupnosti ukázkový kód](http://developer.apple.com/library/ios/#samplecode/Reachability/Introduction/Intro.html) ) pomáhají zjišťovat dostupnost sítě.


## <a name="webservices"></a>WebServices

Najdete v dokumentaci na [práce s webovými službami](~/cross-platform/data-cloud/web-services/index.md), která zahrnuje přístup k REST, koncové body SOAP a WCF pomocí Xamarin.iOS. Je možné ruční plavidla žádosti webové služby a analyzovat odpovědi, ale nejsou k dispozici udělat mnohem jednodušší, včetně Azure, RestSharp a ServiceStack knihovny. V Xamarin aplikace jsou přístupné i základní operace WCF.

### <a name="azure"></a>Azure

Microsoft Azure je Cloudová platforma, která poskytuje širokou škálu služeb pro mobilní aplikace, včetně úložiště dat a synchronizace a nabízených oznámení.

Navštivte [azure.microsoft.com](https://azure.microsoft.com/) chcete vyzkoušet zdarma.

### <a name="restsharp"></a>RestSharp

RestSharp je knihovny .NET, které můžou být součástí mobilní aplikace k zajištění klienta REST, která zjednodušuje přístup k webovým službám. Pomáhá tím, že poskytuje jednoduché rozhraní API pro data žádosti a analyzovat odpověď REST. RestSharp může být užitečné

[RestSharp webu](http://restsharp.org/) obsahuje [dokumentace](https://github.com/restsharp/RestSharp/wiki) o tom, jak implementovat REST klienta pomocí RestSharp.
RestSharp obsahuje příklady Xamarin.iOS a Xamarin.Android na [githubu](https://github.com/restsharp/RestSharp/).

Je také Xamarin.iOS fragmentu kódu v našem [webové služby dokumentaci](~/cross-platform/data-cloud/web-services/index.md).

 <a name="ServiceStack" />


### <a name="servicestack"></a>ServiceStack

Na rozdíl od RestSharp je ServiceStack obou serverové řešení pro hostování webové služby a také klientské knihovny, která můžou se implementovat v mobilních aplikacích pro přístup k těchto služeb.

[ServiceStack webu](http://servicestack.net/) vysvětlující účel projektu a odkazy na dokument a ukázky kódu. Příklady na dokončení serverové implementace webové služby a také různé klientské aplikace, které k němu přístup.

Je [Xamarin.iOS příklad](http://www.servicestack.net/monotouch/remote-info/) na webu ServiceStack a fragmentu kódu v našem [dokumentace webové služby](~/cross-platform/data-cloud/web-services/index.md).


### <a name="wcf"></a>WCF

Nástroje pro Xamarin můžete využívat některé služby Windows Communication Foundation (WCF). Obecně platí Xamarin podporuje stejné podmnožinu klientské služby WCF, který se dodává s modulem runtime Silverlight. To zahrnuje nejběžnější implementace kódování a protokol služby WCF: kódovaný text zprávy protokolu SOAP přes HTTP přenosu pomocí protokolu `BasicHttpBinding`.

Z důvodu velikost a složitost architektury WCF může být implementace aktuálních a budoucích služby, které spadá mimo rozsah podporované Xamarin pro doménu klienta podmnožina. Kromě toho podpora WCF vyžaduje nástroje, které jsou k dispozici pouze v prostředí systému Windows ke generování proxy serveru.

 <a name="Threading" />


## <a name="threading"></a>Dělení na vlákna

Rychlost reakce aplikace je důležité pro mobilní aplikace – uživatelé očekávají, že aplikace pro načtení a provedení rychle. 'Ukotvené' obrazovka, která zastaví přijímá vstup uživatele se zobrazí k označení, že aplikace došlo k chybě, takže je důležité, abyste vytížit vlákna uživatelského rozhraní se dlouho běžící blokování volání, jako jsou síťové požadavky nebo pomalé místní operací (např. rozbalení souboru). Na konkrétní proces spuštění nesmí obsahovat dlouhotrvající úlohy – všechny mobilní platformy bude ukončit aplikaci, která trvá příliš dlouho načíst.

To znamená, že uživatelské rozhraní by měla implementovat na ukazatel průběhu nebo jinak funkční uživatelského rozhraní, který je rychle zobrazit a asynchronních úloh k provedení operace na pozadí. Provádění úlohy na pozadí vyžaduje použití vláken, což znamená, že potřebám úlohy na pozadí způsob, jak sdělit zpět do hlavního vlákna k určení průběhu nebo když byly dokončeny.

 <a name="Parallel_Task_Library" />


### <a name="parallel-task-library"></a>Knihovna paralelních úloh

Úkoly vytvořené s knihovnou paralelní úlohy můžete spustit asynchronně a vrátit na jejich volající vlákno, což je velmi užitečná pro spuštění dlouhotrvající operace bez blokování uživatelské rozhraní.

Operace jednoduché paralelní úloha může vypadat například takto:

```csharp
using System.Threading.Tasks;
void MainThreadMethod ()
{
    Task.Factory.StartNew (() => wc.DownloadString ("http://...")).ContinueWith (
        t => label.Text = t.Result, TaskScheduler.FromCurrentSynchronizationContext()
    );
}
```

Klíč je `TaskScheduler.FromCurrentSynchronizationContext()` který bude používat atribut SynchronizationContext.Current vlákna volání metody (zde hlavní vlákno, které běží `MainThreadMethod`) jako způsob, jak zařazování zpětné volání tento přístup z více vláken. To znamená, že pokud metoda je volána ve vlákně UI, se spustí `ContinueWith` operaci zpět ve vlákně UI.

Pokud kód je od jiná vlákna úlohy, použijte následující vzor vytvořit odkaz na vlákna uživatelského rozhraní a úlohu můžete stále zpětné volání do ní:

```csharp
static Context uiContext = TaskScheduler.FromCurrentSynchronizationContext();
```

 <a name="Invoking_on_the_UI_Thread" />


### <a name="invoking-on-the-ui-thread"></a>Vyvolání ve vlákně UI

Pro kód, který není využívat paralelní knihovna úloh každá platforma má svou vlastní syntaxe pro zařazování operace zpět do vlákna uživatelského rozhraní:

-  **iOS** – `owner.BeginInvokeOnMainThread(new NSAction(action))`
-  **Android** – `owner.RunOnUiThread(action)`
-  **Xamarin.Forms** – `Device.BeginInvokeOnMainThread(action)`
-  **Windows** – `Deployment.Current.Dispatcher.BeginInvoke(action)`



IOS a Android syntaxe vyžaduje třídu "context" být k dispozici, což znamená, že kód je potřeba tento objekt předat do všech metod, které vyžadují zpětného volání ve vlákně UI.

Chcete-li volání vlákna uživatelského rozhraní v sdíleného kódu, postupujte [IDispatchOnUIThread příklad](http://www.slideshare.net/follesoe/cross-platform-mobile-apps-using-net) (laskavým svolením [ @follesoe ](http://jonas.follesoe.no/)). Deklarace a program určený k `IDispatchOnUIThread` rozhraní v sdíleného kódu a potom implementovat třídy specifické pro platformu, jak je vidět tady:

```csharp
// program to the interface in shared code
public interface IDispatchOnUIThread {
    void Invoke (Action action);
}
// iOS
public class DispatchAdapter : IDispatchOnUIThread {
    public readonly NSObject owner;
    public DispatchAdapter (NSObject owner) {
        this.owner = owner;
    }
    public void Invoke (Action action) {
        owner.BeginInvokeOnMainThread(new NSAction(action));
    }
}
// Android
public class DispatchAdapter : IDispatchOnUIThread {
    public readonly Activity owner;
    public DispatchAdapter (Activity owner) {
        this.owner = owner;
    }
    public void Invoke (Action action) {
        owner.RunOnUiThread(action);
    }
}
// WP7
public class DispatchAdapter : IDispatchOnUIThread {
    public void Invoke (Action action) {
        Deployment.Current.Dispatcher.BeginInvoke(action);
    }
}
```

Vývojáři Xamarin.Forms by měl použít [ `Device.BeginInvokeOnMainThread` ](~/xamarin-forms/platform/device.md#Device_BeginInvokeOnMainThread) v společný kód (sdílených projektů nebo PCL).

 <a name="Platform_and_Device_Capabilities_and_Degradation" />


## <a name="platform-and-device-capabilities-and-degradation"></a>Platformy a možnosti zařízení a snížení

Další konkrétní příklady práci s různé možnosti jsou uvedeny v dokumentaci funkce platformy. Se zabývá zjišťování různé možnosti a postup řádně snížit aplikace zajistit vhodné uživatelské prostředí, i v případě, že aplikace nemůže pracovat plně nevyužívají.
