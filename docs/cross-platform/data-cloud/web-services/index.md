---
title: Úvod k webovým službám
description: Tato příručka ukazuje, jak používat technologie jiné webové služby. Obsahuje následující témata komunikaci s služby REST, SOAP služby a služby Windows Communication Foundation.
ms.prod: xamarin
ms.assetid: 72627B90-586A-02B6-E231-F7CE015A1B97
ms.technology: xamarin-cross-platform
author: asb3993
ms.author: amburns
ms.date: 03/23/2017
ms.openlocfilehash: ad18382a7143c7b1cc6bbecb3867c042512eb562
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/04/2018
---
# <a name="introduction-to-web-services"></a>Úvod k webovým službám

_Tato příručka ukazuje, jak používat technologie jiné webové služby. Obsahuje následující témata komunikaci s služby REST, SOAP služby a služby Windows Communication Foundation._

Aby správně fungoval, jsou závislé na cloudu mnoho mobilních aplikací, a proto integraci do mobilní aplikace webové služby je běžný scénář. Platformě Xamarin podporuje použití technologie jiné webové služby a zahrnuje podporu v vytvořená a třetích stran pro použití služby dosáhl standardu RESTful, ASMX a Windows Communication Foundation (WCF).

Tento článek popisuje v následujících tématech:

- [Služby REST](#rest)
- [ASP.Net Web Services (ASMX)](#asmx)
- [WCF Services](#wcf)

Pro zákazníky používající Xamarin.Forms, jsou dokončení příklady použití každé z těchto technologií v [Xamarin.Forms webové služby](~/xamarin-forms/data-cloud/index.md) dokumentaci.

> [!IMPORTANT]
> Zabezpečení přenosu aplikace (ATS) vynucuje v iOS 9, zabezpečené připojení mezi prostředků z Internetu (třeba server back-end aplikace) a aplikace, a tím prevence náhodného zpřístupnění citlivých informací.
> Protože ATS je povolené ve výchozím nastavení v aplikacích pro iOS 9, všechna připojení budou platit ATS požadavky na zabezpečení. Pokud připojení těchto požadavků nesplňuje, budou se nezdaří s výjimkou.

Vám může odhlásit ATS pokud není možné použít `HTTPS` protokolu a zabezpečení komunikace pro prostředků z Internetu. Toho lze dosáhnout aktualizací aplikace **Info.plist** souboru. Další informace najdete v části [zabezpečení přenosu aplikace](~/ios/app-fundamentals/ats.md).



<a name="rest" />

## <a name="rest"></a>REST

Přenos REST (Representational State) je styl architektury pro vytváření webových služeb. REST jsou vytvářeny požadavky prostřednictvím protokolu HTTP použití stejných příkazů HTTP, které webových prohlížečů slouží k načtení webové stránky a k odesílání dat na servery. Příkazy jsou:

- **ZÍSKAT** – tato operace se používá k načtení dat z webové služby.
- **POST** – tato operace se používá k vytvoření nové položky dat na webové službě.
- **UVEĎTE** – tato operace se používá k aktualizaci položky dat na webovou službu.
- **OPRAVA** – tato operace se používá k aktualizaci položky dat na webové služby prostřednictvím popisu sadu pokyny o tom, jak by měl být upraven položky. Tento příkaz se nepoužívá v ukázkové aplikaci.
- **Odstranit** – tato operace se používá k odstranění položky dat na webovou službu.

Rozhraní API, které dodržovat REST se nazývají rozhraní RESTful API a jsou definovány pomocí webové služby:

- Základní identifikátor URI.
- Metody HTTP, jako je například GET, POST, PUT, PATCH nebo odstranit.
- Typ média pro data, jako je například JSON JavaScript Object Notation ().

Jednoduchost REST pomohlo, vytvořte ji primární metodu pro přístup k webovým službám v mobilních aplikacích.

## <a name="consuming-rest-services"></a>Využívání služeb REST

Existuje několik knihoven a třídy, které slouží k využívání služeb REST a následující témata popisují je. Další informace o využívání služby najdete v tématu [využívání RESTful webová služba](~/xamarin-forms/data-cloud/consuming/rest.md).

### <a name="httpclient"></a>HttpClient

[Knihovny klienta HTTP Microsoft](https://www.nuget.org/packages/Microsoft.Net.Http) poskytuje `HttpClient` třídy, která se používá k odesílání a přijímání požadavků prostřednictvím protokolu HTTP. Poskytuje funkce pro odesílání požadavků HTTP a příjmu odpovědi HTTP z identifikátoru URI identifikuje prostředek. Každý požadavek odeslán jako asynchronní operaci. Další informace o asynchronní operace najdete v tématu [Přehled podpory asynchronní](~/cross-platform/platform/async.md).

`HttpResponseMessage` Třída reprezentuje zprávu odpovědi HTTP po provedl se požadavek HTTP přijata webovou službu. Obsahuje informace o odpověď, a to včetně stavový kód, hlavičky a text. `HttpContent` Třída reprezentuje obsahu HTTP a hlavičky obsahu, jako například `Content-Type` a `Content-Encoding`. Obsah lze číst pomocí kteréhokoli `ReadAs` metody, například `ReadAsStringAsync` a `ReadAsByteArrayAsync`, v závislosti na formát data.

Další informace o `HttpClient` třídy najdete v tématu [vytváření objektu HTTPClient](~/xamarin-forms/data-cloud/consuming/rest.md).

<a name="Using_HTTPWebRequest" />

### <a name="httpwebrequest"></a>HTTPWebRequest

Volání webové služby s `HTTPWebRequest` zahrnuje:

-  Vytváření instancí žádosti pro konkrétní identifikátoru URI.
-  Nastavení různé vlastnosti protokolu HTTP na instanci požadavku.
-  Načítání `HttpWebResponse` z požadavku.
-  Čtení dat z odpovědi.

Například následující kód načte data z USA National knihovny lékařství webové služby:

```csharp
var rxcui = "198440";
var request = HttpWebRequest.Create(string.Format(@"http://rxnav.nlm.nih.gov/REST/RxTerms/rxcui/{0}/allinfo", rxcui));
request.ContentType = "application/json";
request.Method = "GET";

using (HttpWebResponse response = request.GetResponse() as HttpWebResponse)
{
  if (response.StatusCode != HttpStatusCode.OK)
     Console.Out.WriteLine("Error fetching data. Server returned status code: {0}", response.StatusCode);
  using (StreamReader reader = new StreamReader(response.GetResponseStream()))
  {
               var content = reader.ReadToEnd();
               if(string.IsNullOrWhiteSpace(content)) {
                       Console.Out.WriteLine("Response contained empty body...");
               }
               else {
                       Console.Out.WriteLine("Response Body: \r\n {0}", content);
               }

               Assert.NotNull(content);
  }
}
```

Tento příklad vytvoří `HttpWebRequest` , vrátí data formátu JSON. Data jsou vrácena v `HttpWebResponse`, ze kterého `StreamReader` získat přečíst data.

<a name="Using_RESTSHARP" />

### <a name="restsharp"></a>RestSharp

Používá jiný přístup k využívání služeb REST [RestSharp](http://restsharp.org/) knihovny. RestSharp zapouzdří požadavků HTTP, včetně podpory pro načítání výsledků jako Nezpracovaný řetězec obsah, nebo jako deserializovaný objekt C#. Například následující kód vytvoří žádost USA Řetězec ve formátu National knihovny lékařství webové služby a načte výsledky jako JSON:

```csharp
var request = new RestRequest(string.Format("{0}/allinfo", rxcui));
request.RequestFormat = DataFormat.Json;
var response = Client.Execute(request);
if(string.IsNullOrWhiteSpace(response.Content) || response.StatusCode != System.Net.HttpStatusCode.OK) {
       return null;
}
rxTerm = DeserializeRxTerm(response.Content);
```

`DeserializeRxTerm` je metoda, která bude trvat Nezpracovaný řetězec z JSON `RestSharp.RestResponse.Content` vlastnost a převádět je do objektu C#. Při deserializaci data vrácená z webových služeb je popsána dále v tomto článku.

<a name="Using_NSUrlconnection" />

### <a name="nsurlconnection"></a>NSUrlConnection

Kromě třídy, které jsou k dispozici v Mono základní třídy knihovny (BCL), jako například `HttpWebRequest`a knihoven jiných dodavatelů C#, jako je například RestSharp, specifické pro platformu třídy jsou také k dispozici pro využívání webových služeb. Například v iOS `NSUrlConnection` a `NSMutableUrlRequest` lze použít třídy.

Následující příklad kódu ukazuje, jak volat USA National knihovny lékařství webové službě pomocí třídy iOS:

```csharp
var rxcui = "198440";
var request = new NSMutableUrlRequest(new NSUrl(string.Format("http://rxnav.nlm.nih.gov/REST/RxTerms/rxcui/{0}/allinfo", rxcui)),
       NSUrlRequestCachePolicy.ReloadRevalidatingCacheData, 20);
request["Accept"] = "application/json";

var connectionDelegate = new RxTermNSURLConnectionDelegate();
var connection = new NSUrlConnection(request, connectionDelegate);
connection.Start();

public class RxTermNSURLConnectionDelegate : NSUrlConnectionDelegate
{
       StringBuilder _ResponseBuilder;
       public bool IsFinishedLoading { get; set; }
       public string ResponseContent { get; set; }

       public RxTermNSURLConnectionDelegate()
               : base()
       {
               _ResponseBuilder = new StringBuilder();
       }

       public override void ReceivedData(NSUrlConnection connection, NSData data)
       {
               if(data != null) {
                       _ResponseBuilder.Append(data.ToString());
               }
       }
       public override void FinishedLoading(NSUrlConnection connection)
       {
               IsFinishedLoading = true;
               ResponseContent = _ResponseBuilder.ToString();
       }
}
```

Třídy specifické pro platformu pro využívání webových služeb obecně platí, by měl být omezeny na scénáře, kde je probíhá přesně nativního kódu do jazyka C#. Pokud je to možné, tak, aby bylo možné ho sdílené platformě by měla být přenosná kódu webové služby přístup.

<a name="Using_ServiceStack_Client" />

### <a name="servicestack"></a>ServiceStack

Další možností pro volání webové služby je [služby zásobníku](http://www.servicestack.net/) knihovny. Například následující kód ukazuje, jak používat službu zásobníku `IServiceClient.GetAsync` metoda vydat žádost o služby:

```csharp
client.GetAsync<CustomersResponse>("",
          (response) => {
               foreach(var c in response.Customers) {
                       Console.WriteLine(c.CompanyName);
               }
       },
       (response, ex) => {
               Console.WriteLine(ex.Message);
       });
```

> [!IMPORTANT]
> I když se nástroje jako ServiceStack a RestSharp usnadňují volání a využívat REST služby, je někdy netriviální využívat XML nebo formátu JSON, který není v souladu s standardní _kontraktu_ konvence serializace. V případě potřeby vyvolání žádosti a zpracování příslušné serializace explicitně pomocí knihovny ServiceStack.Text popsané níže.


<a name="Options_for_consuming_RESTful_data" />

## <a name="consuming-restful-data"></a>Použití RESTful dat

RESTful webové služby obvykle používají JSON zprávy klientovi vrátit data. JSON je založený na textu výměnu formátu, který vytváří compact datových částí, což vede k nároky na menší šířku pásma při odesílání dat. V této části prozkoumá mechanismy pro použití RESTful odpovědi ve formátu JSON a prostý. starý XML (POX).

<a name="Using_System.JSON" />

### <a name="systemjson"></a>System.JSON

Platformě Xamarin se dodává s podporou pro formát JSON mimo pole. Pomocí `JsonObject`, jak je znázorněno v následujícím příkladu kódu se dá načíst výsledky:

```csharp
var obj = JsonObject.Parse(json);
var properties = obj["rxtermsProperties"];
term.BrandName = properties["brandName"];
term.DisplayName = properties["displayName"];
term.Synonym = properties["synonym"];
term.FullName = properties["fullName"];
term.FullGenericName = properties["fullGenericName"];
term.Strength = properties["strength"];
```

Je ale důležité si uvědomit, `System.Json` nástroje načtení celého data do paměti.

<a name="Using_JSON.NET" />

### <a name="jsonnet"></a>JSON.NET

[NewtonSoft JSON.NET knihovny](http://www.newtonsoft.com/json) je často používaný knihovna pro serializaci a deserializaci JSON zprávy. Následující příklad kódu ukazuje, jak používat technologie JSON.NET k deserializaci. zprávu JSON do objektu C#:

```csharp
var term = new RxTerm();
var properties = JObject.Parse(json)["rxtermsProperties"];
term.BrandName = properties["brandName"].Value<string>();
term.DisplayName = properties["displayName"].Value<string>();
term.Synonym = properties["synonym"].Value<string>();;
term.FullName = properties["fullName"].Value<string>();;
term.FullGenericName = properties["fullGenericName"].Value<string>();;
term.Strength = properties["strength"].Value<string>();
term.RxCUI = properties["rxcui"].Value<string>();
```

<a name="Using_ServiceStack.Text" />

### <a name="servicestacktext"></a>ServiceStack.Text

ServiceStack.Text je navržen pro práci s knihovnou ServiceStack knihovna serializace JSON. Následující příklad kódu ukazuje, jak při rozložení pomocí formátu JSON `ServiceStack.Text.JsonObject`:

```csharp
var result = JsonObject.Parse(json).Object("rxtermsProperties")
       .ConvertTo(x => new RxTerm {
               BrandName = x.Get("brandName"),
               DisplayName = x.Get("displayName"),
               Synonym = x.Get("synonym"),
               FullName = x.Get("fullName"),
               FullGenericName = x.Get("fullGenericName"),
               Strength = x.Get("strength"),
               RxTermDoseForm = x.Get("rxtermsDoseForm"),
               Route = x.Get("route"),
               RxCUI = x.Get("rxcui"),
               RxNormDoseForm = x.Get("rxnormDoseForm"),
       });
```

<a name="Using_System.Xml.Linq" />

### <a name="systemxmllinq"></a>System.Xml.Linq

V případě využívání webové služby XML na základě REST, technologie LINQ to XML lze analyzovat kód XML a naplnit C# vložený objekt, jak je ukázáno v následujícím příkladu kódu:

```csharp
var doc = XDocument.Parse(xml);
var result = doc.Root.Descendants("rxtermsProperties")
.Select(x=> new RxTerm()
       {
               BrandName = x.Element("brandName").Value,
               DisplayName = x.Element("displayName").Value,
               Synonym = x.Element("synonym").Value,
               FullName = x.Element("fullName").Value,
               FullGenericName = x.Element("fullGenericName").Value,
               //bind more here...
               RxCUI = x.Element("rxcui").Value,
       });
```

<a name="asmx" />

## <a name="aspnet-web-service-asmx"></a>Webová služba ASP.NET (ASMX)

ASMX poskytuje možnost vytvářet webové služby, které odesílat zprávy pomocí objekt přístup protokolu SOAP (Simple). SOAP je nezávislé na platformě a nezávislé na jazyku protokol pro vytváření a přístup k webovým službám. Příjemci služby ASMX není potřeba nic vědět o platformu, objektový model nebo programovací jazyk používaný k implementaci služby. Potřebují pouze pochopit, jak odesílat a přijímat zprávy protokolu SOAP.

Zprávu SOAP je dokument XML, který obsahuje následující prvky:

- Kořenový element s názvem *obálky* identifikující v dokumentu XML jako zprávu protokolu SOAP.
- Volitelný *záhlaví* element, který obsahuje informace specifické pro aplikaci, například data ověřování. Pokud *záhlaví* nachází element musí být první podřízený prvek *obálky* elementu.
- Požadovanou *textu* elementu, který obsahuje zprávu protokolu SOAP určené k příjemce.
- Volitelný *selhání* element, který slouží k označení chybové zprávy. Pokud *selhání* element přítomen, podřízený element musí být *textu* elementu.

SOAP můžete pracovat s mnoha přenosové protokoly, včetně protokolů HTTP, SMTP, TCP a UDP. Ale služby ASMX mohou pracovat pouze prostřednictvím protokolu HTTP. Platforma Xamarin podporuje standardní implementace SOAP 1.1 přes protokol HTTP, a to zahrnuje podporu pro řadu standardní konfigurace služby ASMX.

### <a name="generating-a-proxy"></a>Generování proxy server

A *proxy* musí být generovány využívat ASMX služby, který umožňuje aplikaci připojit ke službě. Proxy serveru je vytvořený ve využívání služby metadata, která definuje metody a přidružené služby konfigurace. Tato data použita jako dokument služby popis jazyka WSDL (Web), který je generovaný webovou službu. Proxy server je vytvořené pomocí sady Visual Studio pro Mac nebo Visual Studio pro přidání odkazu na web pro webovou službu k projektům specifické pro platformu.

Adresa URL webové služby může být hostované vzdáleného zdroje nebo místního souboru Systémový prostředek přístupné přes `file:///` předpona cestu, např.:

```csharp
file:///Users/myUserName/projects/MyProjectName/service.wsdl
```

[![](images/add-webreference-dialog.png "Adresa URL webové služby může být hostované vzdáleného zdroje nebo místního souboru Systémový prostředek přístupné přes předponu cesta k souboru")](images/add-webreference-dialog.png#lightbox)

Tím se vytvoří proxy serveru ve složce Web nebo odkazy na službu projektu. Vzhledem k tomu, že se generuje proxy server kódu, nesmí být měněny.

<a name="Manually_adding_a_proxy_to_a_project" />

#### <a name="manually-adding-a-proxy-to-a-project"></a>Ruční přidání proxy serveru do projektu

Pokud máte stávající proxy, která byla vygenerována pomocí nástrojů kompatibilní, mohou být využívány tento výstup, když jsou součástí projektu. V sadě Visual Studio pro Mac, použijte **přidání souborů...** možnost nabídky Přidat proxy server. Kromě toho potřebuje *System.Web.Services.dll* bude odkazovat pomocí **přidat odkazy...** Dialogové okno.

### <a name="consuming-the-proxy"></a>Využívání proxy serveru

Třídy generované proxy poskytují metody pro použití webové služby, které používají vzor návrhu asynchronní programování modelu (APM). V tomto vzoru asynchronní operaci je implementovaný jako dvě metody s názvem *BeginOperationName* a *EndOperationName*, které začínají a končí asynchronní operaci.

*BeginOperationName* metoda zahájí asynchronní operaci a vrátí objekt, který implementuje `IAsyncResult` rozhraní. Po volání *BeginOperationName*, aplikace můžete pokračovat v provádění instrukcí na volající vlákno, zatímco probíhá asynchronní operaci na vlákno fondu vláken.

Pro každé volání *BeginOperationName*, aplikace by měly volat také *EndOperationName* získat výsledky operace. Návratová hodnota *EndOperationName* je stejný typ vrácený metodu synchronní webové služby. Následující příklad kódu ukazuje příklad tohoto:

```csharp
public async Task<List<TodoItem>> RefreshDataAsync ()
{
  ...
  var todoItems = await Task.Factory.FromAsync<ASMXService.TodoItem[]> (
    todoService.BeginGetTodoItems,
    todoService.EndGetTodoItems,
    null,
    TaskCreationOptions.None);
  ...
}
```

Task Parallel Library (TPL) může zjednodušit proces využívání dvojici APM zahájení a ukončení metoda zapouzdřením asynchronních operací ve stejném `Task` objektu. Tato zapouzdření poskytuje více přetížení `Task.Factory.FromAsync` metoda. Tato metoda vytvoří `Task` , která se spouští `TodoService.EndGetTodoItems` metoda jednou `TodoService.BeginGetTodoItems` metoda dokončí, s `null` parametr určující, zda je předávána žádná data do `BeginGetTodoItems` delegovat. Nakonec hodnotu `TaskCreationOptions` – výčet Určuje, že má být použita výchozí chování pro vytváření a spouštění úloh.

Další informace o APM najdete v tématu [modelu asynchronního programování](https://msdn.microsoft.com/en-us/library/ms228963(v=vs.110).aspx) a [TPL a tradiční rozhraní .NET Framework asynchronní programování](https://msdn.microsoft.com/en-us/library/dd997423(v=vs.110).aspx) na webu MSDN.

Další informace o využívání služby ASMX najdete v tématu [využívání služby Web ASP.NET (ASMX)](~/xamarin-forms/data-cloud/consuming/asmx.md).

<a name="wcf" />

## <a name="windows-communication-foundation-wcf"></a>Windows Communication Foundation (WCF)

WCF je společnosti Microsoft jednotné rozhraní pro vytváření aplikací orientovaných na služby. Umožňuje vývojářům vytvářet zabezpečeným, spolehlivým, zpracovaných a umožňuje vzájemnou spolupráci distribuované aplikace.

WCF popisuje služby s různými jiné smlouvy, které zahrnují následující:

- **Kontrakty dat** – definovat datové struktury, které tvoří základ pro obsah ve zprávě.
- **Kontrakty zpráv** – vytváření zpráv ze stávajících dat smluv.
- **Poruch kontrakty** – povolit vlastní chyb SOAP zadat.
- **Kontrakty služeb** – zadejte operace, které podporují služby a zprávy vyžadované pro interakci s každou operaci. Také určí chování všechny vlastní chyby, které může být přidružen operace u každé služby.

Jsou rozdíly mezi webové služby ASP.NET (ASMX) a WCF, ale je důležité si uvědomit, že WCF podporuje stejné funkce, které poskytuje ASMX – protokolu SOAP zprávy přes protokol HTTP.

Obecně platí platforma Xamarin podporuje stejné podmnožinu klientské služby WCF, který se dodává s modulem runtime Silverlight. To zahrnuje nejběžnější implementace kódování a protokol služby WCF – kódovaný text zprávy protokolu SOAP přes HTTP přenosu pomocí protokolu `BasicHttpBinding` třídy. Kromě toho podpora WCF vyžaduje nástroje, které jsou k dispozici pouze v prostředí systému Windows ke generování proxy serveru.

Další informace o použití platformy Xamarin využívat službou WCF webové služby s `BasicHttpBinding` třídy najdete v tématu [návod - práce s použitím technologie WCF](walkthrough-working-with-wcf.md).

### <a name="generating-a-proxy"></a>Generování proxy server

A *proxy* musí být generovány využívat služby WCF, který umožňuje aplikaci připojit ke službě. Proxy serveru je vytvořený ve využívání služby metadata, která definují metody a přidružená služba konfigurace. Tato data použita ve formě webové služby popis Language (WSDL) dokumentu, který je generovaný webovou službu. Proxy server se dají vytvářet pomocí zprostředkovatele služby Microsoft WCF webové služby odkaz v Visual Studio 2017 přidání odkazu na službu pro webovou službu do standardní knihovna pro .NET.

Alternativu k vytvoření proxy server pomocí zprostředkovatele služby Microsoft WCF webové služby odkaz v Visual Studio 2017 je použití ServiceModel Metadata Utility Tool (svcutil.exe). Další informace najdete v tématu [ServiceModel Metadata Utility Tool (Svcutil.exe)](https://docs.microsoft.com/en-us/dotnet/framework/wcf/servicemodel-metadata-utility-tool-svcutil-exe).

<a name="Calling_a_WCF_Service_with_Client_Credential_Security" />

### <a name="configuring-the-proxy"></a>Konfigurace proxy serveru

Konfigurace vygenerovaný proxy server bude obvykle trvat dva argumenty konfigurace (v závislosti na protokolu SOAP 1.1 nebo ASMX nebo WCF) během inicializace: `EndpointAddress` a přidružené informace o vazbě, jak je znázorněno v následujícím příkladu:

```csharp
var binding = new BasicHttpBinding () {
       Name= "basicHttpBinding",
       MaxReceivedMessageSize = 67108864,
};

binding.ReaderQuotas = new System.Xml.XmlDictionaryReaderQuotas() {
       MaxArrayLength = 2147483646,
       MaxStringContentLength = 5242880,
};

var timeout = new TimeSpan(0,1,0);
binding.SendTimeout= timeout;
binding.OpenTimeout = timeout;
binding.ReceiveTimeout = timeout;

client = new Service1Client (binding, new EndpointAddress ("http://192.168.1.100/Service1.svc"));
```

Vazba slouží k určení přenosu, kódování a protokol údaje požadované pro aplikace a služby pro komunikaci mezi sebou. `BasicHttpBinding` Určuje, že kódovaný text zprávy protokolu SOAP se odešlou přes přenosu protokolu HTTP. Zadání adresy koncového bodu umožňuje aplikaci připojit k jiné instance služby WCF, za předpokladu, že existuje více instancí publikované.

### <a name="consuming-the-proxy"></a>Využívání proxy serveru

Třídy generované proxy poskytují metody pro použití webové služby, které používají vzor návrhu asynchronní programování modelu (APM). V tomto vzoru asynchronní operaci je implementovaný jako dvě metody s názvem *BeginOperationName* a *EndOperationName*, které začínají a končí asynchronní operaci.

*BeginOperationName* metoda zahájí asynchronní operaci a vrátí objekt, který implementuje `IAsyncResult` rozhraní. Po volání *BeginOperationName*, aplikace můžete pokračovat v provádění instrukcí na volající vlákno, zatímco probíhá asynchronní operaci na vlákno fondu vláken.

Pro každé volání *BeginOperationName*, aplikace by měly volat také *EndOperationName* získat výsledky operace. Návratová hodnota *EndOperationName* je stejný typ vrácený metodu synchronní webové služby. Následující příklad kódu ukazuje příklad tohoto:

```csharp
public async Task<List<TodoItem>> RefreshDataAsync ()
{
  ...
  var todoItems = await Task.Factory.FromAsync <ObservableCollection<TodoWCFService.TodoItem>> (
    todoService.BeginGetTodoItems,
    todoService.EndGetTodoItems,
    null,
    TaskCreationOptions.None);
  ...
}
```

Task Parallel Library (TPL) může zjednodušit proces využívání dvojici APM zahájení a ukončení metoda zapouzdřením asynchronních operací ve stejném `Task` objektu. Tato zapouzdření poskytuje více přetížení `Task.Factory.FromAsync` metoda. Tato metoda vytvoří `Task` , která se spouští `TodoServiceClient.EndGetTodoItems` metoda jednou `TodoServiceClient.BeginGetTodoItems` metoda dokončí, s `null` parametr určující, zda je předávána žádná data do `BeginGetTodoItems` delegovat. Nakonec hodnotu `TaskCreationOptions` – výčet Určuje, že má být použita výchozí chování pro vytváření a spouštění úloh.

Další informace o APM najdete v tématu [modelu asynchronního programování](https://msdn.microsoft.com/en-us/library/ms228963(v=vs.110).aspx) a [TPL a tradiční rozhraní .NET Framework asynchronní programování](https://msdn.microsoft.com/en-us/library/dd997423(v=vs.110).aspx) na webu MSDN.

Další informace o využívání služby WCF najdete v tématu [využívají webové služby Windows Communication Foundation (WCF)](~/xamarin-forms/data-cloud/consuming/wcf.md).

<a name="Calling_a_WCF_Service_with_Transport_Security" />

#### <a name="using-transport-security"></a>Pomocí zabezpečení přenosu

Služby WCF mohou využívat zabezpečení na úrovni přenosu pro ochranu proti zachycení zpráv. Platforma Xamarin podporuje vazby, které využívají zabezpečení na úrovni přenosu pomocí protokolu SSL. Však může být případy, ve kterých se zásobníkem muset ověřit certifikát, což vede k neočekávané chování. Je možné přepsat ověření registrace `ServerCertificateValidationCallback` delegáta před vyvoláním služby, jak je ukázáno v následujícím příkladu kódu:

```csharp
System.Net.ServicePointManager.ServerCertificateValidationCallback +=
(se, cert, chain, sslerror) => { return true; };
```

Tím se zachová přenos šifrování při ignoruje ověření certifikátu na straně serveru. Tento přístup však efektivně ignoruje si nemuseli dělat starosti důvěryhodnosti spojenou s certifikátem a nemusí být vhodné. Další informace najdete v tématu [pomocí důvěryhodných kořenových certifikačních autorit úctou](http://www.mono-project.com/UsingTrustedRootsRespectfully) na [mono project.com](http://www.mono-project.com).

<a name="Calling_a_WCF_Service_with_Client_Credential_Security" />

#### <a name="using-client-credential-security"></a>Pomocí zabezpečení přihlašovacích údajů klienta

Služby WCF může taky požadovat, aby klienti služby k ověření pomocí přihlašovacích údajů. Xamarin platforma nepodporuje protokol WS-zabezpečení, která umožní klientům posílat přihlašovací údaje do obálky protokolu SOAP zprávy. Však platformě Xamarin podporuje možnost odeslat pověření základního ověřování protokolu HTTP na server tak, že zadáte odpovídající `ClientCredentialType`:

```csharp
basicHttpBinding.Security.Transport.ClientCredentialType = HttpClientCredentialType.Basic;
```

Potom můžete zadat pověření základního ověřování:

```csharp
client.ClientCredentials.UserName.UserName = @"foo";
client.ClientCredentials.UserName.Password = @"mrsnuggles";
```

V příkladu výše, pokud se zobrazí zpráva "Nemá dostatek trampolines typu 0" můžete zvýšit počet typu 0 trampolines přidáním `–aot “trampolines={number of trampolines}”` argument sestavení. Další informace najdete v tématu [Poradce při potížích s](~/ios/troubleshooting/troubleshooting.md#trampolines).

Další informace o základní ověřování protokolu HTTP, i když v kontextu webové služby REST, najdete v části [ověřování RESTful webová služba](~/xamarin-forms/data-cloud/authentication/rest.md).

## <a name="summary"></a>Souhrn

Tato příručka ukázal, jak používat technologie jiné webové služby. Obsahuje následující témata komunikaci s služby REST, SOAP služby a služby Windows Communication Foundation.

## <a name="related-links"></a>Související odkazy

- [WebServices Sample](https://developer.xamarin.com/samples/mobile/WebServices/WebServiceSamples/)
- [Webové služby v platformě Xamarin.Forms](~/xamarin-forms/data-cloud/index.md)
- [Nástroj ServiceModel Metadata Utility (svcutil.exe)](https://docs.microsoft.com/en-us/dotnet/framework/wcf/servicemodel-metadata-utility-tool-svcutil-exe)
- [BasicHttpBinding](http://msdn.microsoft.com/en-us/library/system.servicemodel.basichttpbinding.aspx)
