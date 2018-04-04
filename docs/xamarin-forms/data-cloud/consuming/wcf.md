---
title: Využívají webové služby systému Windows Communication Foundation (WCF)
description: WCF je společnosti Microsoft jednotné rozhraní pro vytváření aplikací orientovaných na služby. Umožňuje vývojářům vytvářet zabezpečeným, spolehlivým, zpracovaných a umožňuje vzájemnou spolupráci distribuované aplikace. Tento článek ukazuje, jak používat služby WCF objekt přístup protokolu SOAP (Simple) z aplikace Xamarin.Forms.
ms.prod: xamarin
ms.assetid: 5696FF04-EF21-4B7A-8C8B-26DE28B5C0AD
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 09/20/2016
ms.openlocfilehash: c626008012ccdab2f8ed2c719b34a45471598d47
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/04/2018
---
# <a name="consuming-a-windows-communication-foundation-wcf-web-service"></a>Využívají webové služby systému Windows Communication Foundation (WCF)

_WCF je společnosti Microsoft jednotné rozhraní pro vytváření aplikací orientovaných na služby. Umožňuje vývojářům vytvářet zabezpečeným, spolehlivým, zpracovaných a umožňuje vzájemnou spolupráci distribuované aplikace. Tento článek ukazuje, jak používat služby WCF objekt přístup protokolu SOAP (Simple) z aplikace Xamarin.Forms._

WCF popisuje služby s různými jiné smlouvy, které zahrnují následující:

- **Kontrakty dat** – definovat datové struktury, které tvoří základ pro obsah ve zprávě.
- **Kontrakty zpráv** – vytváření zpráv ze stávajících dat smluv.
- **Poruch kontrakty** – povolit vlastní chyb SOAP zadat.
- **Kontrakty služeb** – zadejte operace, které podporují služby a zprávy vyžadované pro interakci s každou operaci. Také určí chování všechny vlastní chyby, které může být přidružen operace u každé služby.

Jsou rozdíly mezi webové služby ASP.NET (ASMX) a WCF, ale je důležité si uvědomit, že WCF podporuje stejné funkce, které poskytuje ASMX – protokolu SOAP zprávy přes protokol HTTP. Další informace o využívání služby ASMX najdete v tématu [využívání ASP.NET Web Services (ASMX)](~/xamarin-forms/data-cloud/consuming/asmx.md).

Obecně platí platforma Xamarin podporuje stejné podmnožinu klientské služby WCF, který se dodává s modulem runtime Silverlight. To zahrnuje nejběžnější implementace kódování a protokol služby WCF – kódovaný text zprávy protokolu SOAP přes HTTP přenosu pomocí protokolu `BasicHttpBinding` třídy. Kromě toho podpora WCF vyžaduje nástroje, které jsou k dispozici pouze v prostředí systému Windows ke generování proxy serveru.

Pokyny k nastavení služby WCF naleznete v souboru readme, který doprovází ukázkovou aplikaci. Však při spuštění ukázkové aplikace se připojí k službě WCF hostované Xamarin, která poskytuje přístup jen pro čtení k datům, jak je znázorněno na následujícím snímku obrazovky:

![](wcf-images/portal.png "Ukázkové aplikace")

> [!NOTE]
> Zabezpečení přenosu aplikace (ATS) vynucuje v iOS 9 a vyšší, zabezpečené připojení mezi prostředků z Internetu (třeba server back-end aplikace) a aplikace, a tím prevence náhodného zpřístupnění citlivých informací. Protože ATS je povolené ve výchozím nastavení v aplikacích pro iOS 9, všechna připojení budou platit ATS požadavky na zabezpečení. Pokud připojení těchto požadavků nesplňuje, budou se nezdaří s výjimkou.
> ATS můžete vyjádřit výslovný souhlas mimo Pokud není možné použít `HTTPS` protokolu a zabezpečení komunikace pro prostředků z Internetu. Toho lze dosáhnout aktualizací aplikace **Info.plist** souboru. Další informace najdete v části [zabezpečení přenosu aplikace](~/ios/app-fundamentals/ats.md).

## <a name="consuming-the-web-service"></a>Využívají webové služby

Služby WCF poskytuje následující operace:

|Operace|Popis|Parametry|
|--- |--- |--- |
|GetTodoItems|Získat seznam položek úkolů|
|CreateTodoItem|Vytvořit novou položku seznamu úkolů|XML serializovat TodoItem|
|EditTodoItem|Aktualizujte položku seznamu úkolů|XML serializovat TodoItem|
|DeleteTodoItem|Odstranit položku seznamu úkolů|XML serializovat TodoItem|

Další informace o modelu dat používaných v aplikaci najdete v tématu [modelování data](~/xamarin-forms/data-cloud/walkthrough.md).

> [!NOTE]
> Ukázková aplikace využívá službu WCF hostované Xamarin, který poskytuje přístup jen pro čtení k webové službě. Proto operace, které vytvářet, aktualizovat a odstraňovat data nemění data využívat v aplikaci. Však je k dispozici v IIS s možnostmi hostitele verzi služby ASMX **TodoWCFService** složky v doprovodné ukázkovou aplikaci. Tato verze IIS s možnostmi hostitele povolí služby WCF, úplná vytvářet, aktualizovat, číst a odstranit přístup k datům.

A *proxy* musí být generovány využívat služby WCF, který umožňuje aplikaci připojit ke službě. Proxy serveru je vytvořený ve využívání služby metadata, která definují metody a přidružená služba konfigurace. Tato data použita ve formě webové služby popis Language (WSDL) dokumentu, který je generovaný webovou službu. Proxy server se dají vytvářet pomocí zprostředkovatele služby Microsoft WCF webové služby odkaz v Visual Studio 2017 přidání odkazu na službu pro webovou službu do standardní knihovna pro .NET. Alternativu k vytvoření proxy server pomocí zprostředkovatele služby Microsoft WCF webové služby odkaz v Visual Studio 2017 je použití ServiceModel Metadata Utility Tool (svcutil.exe). Další informace najdete v tématu [ServiceModel Metadata Utility Tool (Svcutil.exe)](/dotnet/framework/wcf/servicemodel-metadata-utility-tool-svcutil-exe/).

Třídy generované proxy poskytují metody pro použití webové služby, které používají vzor návrhu asynchronní programování modelu (APM). V tomto vzoru asynchronní operaci je implementovaný jako dvě metody s názvem *BeginOperationName* a *EndOperationName*, které začínají a končí asynchronní operaci.

*BeginOperationName* metoda zahájí asynchronní operaci a vrátí objekt, který implementuje `IAsyncResult` rozhraní. Po volání *BeginOperationName*, aplikace můžete pokračovat v provádění instrukcí na volající vlákno, zatímco probíhá asynchronní operaci na vlákno fondu vláken.

Pro každé volání *BeginOperationName*, aplikace by měly volat také *EndOperationName* získat výsledky operace. Návratová hodnota *EndOperationName* je stejný typ vrácený metodu synchronní webové služby. Například `EndGetTodoItems` metoda vrátí kolekci `TodoItem` instance. *EndOperationName* metoda zahrnuje také `IAsyncResult` parametr, který by měl být nastaven na instanci vrácený odpovídající volání *BeginOperationName* metoda.

Task Parallel Library (TPL) může zjednodušit proces využívání dvojici APM zahájení a ukončení metoda zapouzdřením asynchronních operací ve stejném `Task` objektu. Tato zapouzdření poskytuje více přetížení `TaskFactory.FromAsync` metoda.

Další informace o APM v tématu [modelu asynchronního programování](https://msdn.microsoft.com/library/ms228963(v=vs.110).aspx) a [TPL a tradiční rozhraní .NET Framework asynchronní programování](https://msdn.microsoft.com/library/dd997423(v=vs.110).aspx) na webu MSDN.

### <a name="creating-the-todoserviceclient-object"></a>Vytvoření objektu TodoServiceClient

Poskytuje třídu vygenerovaný proxy server `TodoServiceClient` třídy, která se používá ke komunikaci se službou WCF přes HTTP. Poskytuje funkce pro vyvolání metody webové služby, jako asynchronní operace z identifikátoru URI identifikovat instance služby. Další informace o asynchronní operace najdete v tématu [Přehled podpory asynchronní](~/cross-platform/platform/async.md).

`TodoServiceClient` Tak, aby objekt funguje tak dlouho, dokud aplikace musí využívat službu WCF, jak je znázorněno v následujícím příkladu kódu je na úrovni třídy deklarována instance:

```csharp
public class SoapService : ISoapService
{
  ITodoService todoService;
  ...

  public SoapService ()
  {
    todoService = new TodoServiceClient (
      new BasicHttpBinding (),
      new EndpointAddress (Constants.SoapUrl));
  }
  ...
}
```

`TodoServiceClient` Instance je nakonfigurovaná s vazbou informace a adresu koncového bodu. Vazba slouží k určení přenosu, kódování a protokol údaje požadované pro aplikace a služby pro komunikaci mezi sebou. `BasicHttpBinding` Určuje, že kódovaný text zprávy protokolu SOAP se odešlou přes přenosu protokolu HTTP. Zadání adresy koncového bodu umožňuje aplikaci připojit k jiné instance služby WCF, za předpokladu, že existuje více instancí publikované.

Další informace o konfiguraci odkaz na službu najdete v tématu [konfigurace odkaz na službu](~/cross-platform/data-cloud/web-services/index.md#wcf).

### <a name="creating-data-transfer-objects"></a>Vytváření objektů přenosu dat

Ukázková aplikace používá `TodoItem` třída pro data modelu. K ukládání `TodoItem` položek ve webové službě, nejprve převeďte k proxy serveru generované `TodoItem` typu. To lze provést `ToWCFServiceTodoItem` metoda, jak je znázorněno v následujícím příkladu kódu:

```csharp
TodoWCFService.TodoItem ToWCFServiceTodoItem (TodoItem item)
{
  return new TodoWCFService.TodoItem {
    ID = item.ID,
    Name = item.Name,
    Notes = item.Notes,
    Done = item.Done
  };
}
```

Tato metoda jednoduše vytvoří novou `TodoWCFService.TodoItem` instance a nastaví vlastnost identické z každou vlastnost `TodoItem` instance.

Podobně když data se načítají z webové služby, je nutné jej převést z proxy serveru generované `TodoItem` typ, který má `TodoItem` instance. To je provedeno pomocí `FromWCFServiceTodoItem` metoda, jak je znázorněno v následujícím příkladu kódu:

```csharp
static TodoItem FromWCFServiceTodoItem (TodoWCFService.TodoItem item)
{
  return new TodoItem {
    ID = item.ID,
    Name = item.Name,
    Notes = item.Notes,
    Done = item.Done
  };
}

```

Tato metoda jednoduše načte data z proxy serveru generované `TodoItem` zadejte a nastaví jej do nově vytvořené `TodoItem` instance.

### <a name="retrieving-data"></a>Načítání dat

`TodoServiceClient.BeginGetTodoItems` a `TodoServiceClient.EndGetTodoItems` metody se používají k volání `GetTodoItems` operace poskytované webovou službu. Tyto asynchronní metody jsou zapouzdřené v `Task` objektu, jak je znázorněno v následujícím příkladu kódu:

```csharp
public async Task<List<TodoItem>> RefreshDataAsync ()
{
  ...
  var todoItems = await Task.Factory.FromAsync <ObservableCollection<TodoWCFService.TodoItem>> (
    todoService.BeginGetTodoItems,
    todoService.EndGetTodoItems,
    null,
    TaskCreationOptions.None);

  foreach (var item in todoItems) {
    Items.Add (FromWCFServiceTodoItem (item));
  }
  ...
}
```

`Task.Factory.FromAsync` Metoda vytvoří `Task` , která se spouští `TodoServiceClient.EndGetTodoItems` metoda jednou `TodoServiceClient.BeginGetTodoItems` metoda dokončí, s `null` parametr určující, zda je předávána žádná data do `BeginGetTodoItems` delegovat. Nakonec hodnotu `TaskCreationOptions` – výčet Určuje, že má být použita výchozí chování pro vytváření a spouštění úloh.

`TodoServiceClient.EndGetTodoItems` Metoda vrátí `ObservableCollection` z `TodoWCFService.TodoItem` instance, které se potom převedou do `List` z `TodoItem` instance pro zobrazení.

### <a name="creating-data"></a>Vytváření dat

`TodoServiceClient.BeginCreateTodoItem` a `TodoServiceClient.EndCreateTodoItem` metody se používají k volání `CreateTodoItem` operace poskytované webovou službu. Tyto asynchronní metody jsou zapouzdřené v `Task` objektu, jak je znázorněno v následujícím příkladu kódu:

```csharp
public async Task SaveTodoItemAsync (TodoItem item, bool isNewItem = false)
{
  ...
  var todoItem = ToWCFServiceTodoItem (item);
  ...
  await Task.Factory.FromAsync (
    todoService.BeginCreateTodoItem,
    todoService.EndCreateTodoItem,
    todoItem,
    TaskCreationOptions.None);
  ...
}
```

`Task.Factory.FromAsync` Metoda vytvoří `Task` , která se spouští `TodoServiceClient.EndCreateTodoItem` metoda jednou `TodoServiceClient.BeginCreateTodoItem` dokončení metoda s `todoItem` parametr se data, která je předána do `BeginCreateTodoItem` delegáta k určení `TodoItem` vytvořit webovou službou. Nakonec hodnotu `TaskCreationOptions` – výčet Určuje, že má být použita výchozí chování pro vytváření a spouštění úloh.

Vyvolává službu web `FaultException` Pokud se nepodaří vytvořit `TodoItem`, která zpracovává aplikací.

### <a name="updating-data"></a>Aktualizace dat

`TodoServiceClient.BeginEditTodoItem` a `TodoServiceClient.EndEditTodoItem` metody se používají k volání `EditTodoItem` operace poskytované webovou službu. Tyto asynchronní metody jsou zapouzdřené v `Task` objektu, jak je znázorněno v následujícím příkladu kódu:

```csharp
public async Task SaveTodoItemAsync (TodoItem item, bool isNewItem = false)
{
  ...
  var todoItem = ToWCFServiceTodoItem (item);
  ...
  await Task.Factory.FromAsync (
    todoService.BeginEditTodoItem,
    todoService.EndEditTodoItem,
    todoItem,
    TaskCreationOptions.None);
  ...
}
```

`Task.Factory.FromAsync` Metoda vytvoří `Task` , která se spouští `TodoServiceClient.EndEditTodoItem` metoda jednou `TodoServiceClient.BeginCreateTodoItem` dokončení metoda s `todoItem` parametr se data, která je předána do `BeginEditTodoItem` delegáta k určení `TodoItem` aktualizovat webovou službou. Nakonec hodnotu `TaskCreationOptions` – výčet Určuje, že má být použita výchozí chování pro vytváření a spouštění úloh.

Vyvolává službu web `FaultException` Pokud se nepodaří najít, nebo aktualizace `TodoItem`, který zpracovává aplikací.

### <a name="deleting-data"></a>Odstraňování dat

`TodoServiceClient.BeginDeleteTodoItem` a `TodoServiceClient.EndDeleteTodoItem` metody se používají k volání `DeleteTodoItem` operace poskytované webovou službu. Tyto asynchronní metody jsou zapouzdřené v `Task` objektu, jak je znázorněno v následujícím příkladu kódu:

```csharp
public async Task DeleteTodoItemAsync (string id)
{
  ...
  await Task.Factory.FromAsync (
    todoService.BeginDeleteTodoItem,
    todoService.EndDeleteTodoItem,
    id,
    TaskCreationOptions.None);
  ...
}
```

`Task.Factory.FromAsync` Metoda vytvoří `Task` , která se spouští `TodoServiceClient.EndDeleteTodoItem` metoda jednou `TodoServiceClient.BeginDeleteTodoItem` dokončení metoda s `id` parametr se data, která je předána do `BeginDeleteTodoItem` delegáta k určení `TodoItem` odstranit webovou službou. Nakonec hodnotu `TaskCreationOptions` – výčet Určuje, že má být použita výchozí chování pro vytváření a spouštění úloh.

Vrátí služby webové `FaultException` Pokud se nepodaří najít, nebo odstranit `TodoItem`, který zpracovává aplikací.

## <a name="summary"></a>Souhrn

Tento článek ukázal, jak používat služby WCF SOAP z Xamarin.Forms aplikace. Obecně platí platforma Xamarin podporuje stejné podmnožinu klientské služby WCF, který se dodává s modulem runtime Silverlight. To zahrnuje nejběžnější implementace kódování a protokol služby WCF – kódovaný text zprávy protokolu SOAP přes HTTP přenosu pomocí protokolu `BasicHttpBinding` třídy. Kromě toho podpora WCF vyžaduje nástroje, které jsou k dispozici pouze v prostředí systému Windows ke generování proxy serveru.


## <a name="related-links"></a>Související odkazy

- [TodoWCF (ukázka)](https://developer.xamarin.com/samples/xamarin-forms/WebServices/TodoWCF/)
- [IAsyncResult](https://msdn.microsoft.com/library/system.iasyncresult(v=vs.110).aspx)
