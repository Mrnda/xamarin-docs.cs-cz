---
title: Využívání ASP.NET webové služby (ASMX)
description: ASMX poskytuje možnost vytvářet webové služby, které odesílat zprávy pomocí objekt přístup protokolu SOAP (Simple). SOAP je nezávislé na platformě a nezávislé na jazyku protokol pro vytváření a přístup k webovým službám. Příjemci služby ASMX není potřeba nic vědět o platformu, objektový model nebo programovací jazyk používaný k implementaci služby. Potřebují pouze pochopit, jak odesílat a přijímat zprávy protokolu SOAP. Tento článek ukazuje, jak používat služby ASMX protokolu SOAP z Xamarin.Forms aplikace.
ms.prod: xamarin
ms.assetid: D5533964-5528-4D35-9C2B-FAFB632472AC
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 09/20/2016
ms.openlocfilehash: c45f0de039abc3f98b7c269f183e2883a495910b
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/04/2018
---
# <a name="consuming-an-aspnet-web-service-asmx"></a>Využívání ASP.NET webové služby (ASMX)

_ASMX poskytuje možnost vytvářet webové služby, které odesílat zprávy pomocí objekt přístup protokolu SOAP (Simple). SOAP je nezávislé na platformě a nezávislé na jazyku protokol pro vytváření a přístup k webovým službám. Příjemci služby ASMX není potřeba nic vědět o platformu, objektový model nebo programovací jazyk používaný k implementaci služby. Potřebují pouze pochopit, jak odesílat a přijímat zprávy protokolu SOAP. Tento článek ukazuje, jak používat služby ASMX protokolu SOAP z Xamarin.Forms aplikace._

Zprávu SOAP je dokument XML, který obsahuje následující prvky:

- Kořenový element s názvem *obálky* identifikující v dokumentu XML jako zprávu protokolu SOAP.
- Volitelný *záhlaví* element, který obsahuje informace specifické pro aplikaci, například data ověřování. Pokud *záhlaví* nachází element musí být první podřízený prvek *obálky* elementu.
- Požadovanou *textu* elementu, který obsahuje zprávu protokolu SOAP určené k příjemce.
- Volitelný *selhání* element, který slouží k označení chybové zprávy. Pokud *selhání* element přítomen, podřízený element musí být *textu* elementu.

SOAP můžete pracovat s mnoha přenosové protokoly, včetně protokolů HTTP, SMTP, TCP a UDP. Ale služby ASMX mohou pracovat pouze prostřednictvím protokolu HTTP. Platforma Xamarin podporuje standardní implementace SOAP 1.1 přes protokol HTTP, a to zahrnuje podporu pro řadu standardní konfigurace služby ASMX.

Pokyny k nastavení služby ASMX naleznete v souboru readme, který doprovází ukázkovou aplikaci. Však při spuštění ukázkové aplikace, se připojí k ASMX Xamarin hostované službě, která poskytuje přístup jen pro čtení k datům, jak je znázorněno na následujícím snímku obrazovky:

![](asmx-images/portal.png "Ukázkové aplikace")

> [!NOTE]
> Zabezpečení přenosu aplikace (ATS) vynucuje v iOS 9 a vyšší, zabezpečené připojení mezi prostředků z Internetu (třeba server back-end aplikace) a aplikace, a tím prevence náhodného zpřístupnění citlivých informací. Protože ATS je povolené ve výchozím nastavení v aplikacích pro iOS 9, všechna připojení budou platit ATS požadavky na zabezpečení. Pokud připojení těchto požadavků nesplňuje, budou se nezdaří s výjimkou.
> ATS můžete vyjádřit výslovný souhlas mimo Pokud není možné použít `HTTPS` protokolu a zabezpečení komunikace pro prostředků z Internetu. Toho lze dosáhnout aktualizací aplikace **Info.plist** souboru. Další informace najdete v části [zabezpečení přenosu aplikace](~/ios/app-fundamentals/ats.md).

## <a name="consuming-the-web-service"></a>Využívají webové služby

Služba ASMX poskytuje následující operace:

|Operace|Popis|Parametry|
|--- |--- |--- |
|GetTodoItems|Získat seznam položek úkolů|
|CreateTodoItem|Vytvořit novou položku seznamu úkolů|XML serializovat TodoItem|
|EditTodoItem|Aktualizujte položku seznamu úkolů|XML serializovat TodoItem|
|DeleteTodoItem|Odstranit položku seznamu úkolů|XML serializovat TodoItem|

Další informace o modelu dat používaných v aplikaci najdete v tématu [modelování data](~/xamarin-forms/data-cloud/walkthrough.md).

> [!NOTE]
> Ukázková aplikace využívá ASMX Xamarin hostované služby, která poskytuje přístup jen pro čtení k webové službě. Proto operace, které vytvářet, aktualizovat a odstraňovat data nemění data využívat v aplikaci. Však je k dispozici v IIS s možnostmi hostitele verzi služby ASMX **TodoASMXService** složky v doprovodné ukázkovou aplikaci. Tato verze IIS s možnostmi hostitele povolení služby ASMX úplné vytvářet, aktualizovat, číst a odstranit přístup k datům.

A *proxy* musí být generovány využívat službu ASMX, která umožňuje aplikaci připojit ke službě. Proxy serveru je vytvořený ve využívání služby metadata, která definuje metody a přidružené služby konfigurace. Tato data použita ve formě webové služby popis Language (WSDL) dokumentu, který je generovaný webovou službu. Proxy server je sestavena přidání odkazu na web pro webovou službu k projektům specifické pro platformu.

Třídy generované proxy poskytují metody pro použití webové služby, které používají vzor návrhu asynchronní programování modelu (APM). V tomto vzoru asynchronní operaci je implementovaný jako dvě metody s názvem *BeginOperationName* a *EndOperationName*, které začínají a končí asynchronní operaci.

*BeginOperationName* metoda zahájí asynchronní operaci a vrátí objekt, který implementuje `IAsyncResult` rozhraní. Po volání *BeginOperationName*, aplikace můžete pokračovat v provádění instrukcí na volající vlákno, zatímco probíhá asynchronní operaci na vlákno fondu vláken.

Pro každé volání *BeginOperationName*, aplikace by měly volat také *EndOperationName* získat výsledky operace. Návratová hodnota *EndOperationName* je stejný typ vrácený metodu synchronní webové služby. Například `EndGetTodoItems` metoda vrátí kolekci `TodoItem` instance. *EndOperationName* metoda zahrnuje také `IAsyncResult` parametr, který by měl být nastaven na instanci vrácený odpovídající volání *BeginOperationName* metoda.

Task Parallel Library (TPL) může zjednodušit proces využívání dvojici APM zahájení a ukončení metoda zapouzdřením asynchronních operací ve stejném `Task` objektu. Tato zapouzdření poskytuje více přetížení `TaskFactory.FromAsync` metoda.

Další informace o APM v tématu [modelu asynchronního programování](https://msdn.microsoft.com/library/ms228963(v=vs.110).aspx) a [TPL a tradiční rozhraní .NET Framework asynchronní programování](https://msdn.microsoft.com/library/dd997423(v=vs.110).aspx) na webu MSDN.

### <a name="creating-the-todoservice-object"></a>Vytvoření objektu TodoService

Poskytuje třídu vygenerovaný proxy server `TodoService` třídy, která se používá ke komunikaci se službou ASMX přes protokol HTTP. Poskytuje funkce pro vyvolání metody webové služby, jako asynchronní operace z identifikátoru URI identifikovat instance služby. Další informace o asynchronní operace najdete v tématu [Přehled podpory asynchronní](~/cross-platform/platform/async.md).

`TodoService` Tak, aby objekt funguje tak dlouho, dokud aplikace musí využívat službu ASMX, jak je znázorněno v následujícím příkladu kódu je na úrovni třídy deklarována instance:

```csharp
public class SoapService : ISoapService
{
  ASMXService.TodoService asmxService;
  ...

  public SoapService ()
  {
    asmxService = new ASMXService.TodoService (Constants.SoapUrl);
  }
  ...
}
```

`TodoService` Konstruktor přebírá parametr volitelný řetězec, který určuje adresu URL instance služby ASMX. To umožňuje aplikaci připojit k jiné instance služby ASMX, za předpokladu, že existuje více instancí publikované.

### <a name="creating-data-transfer-objects"></a>Vytváření objektů přenosu dat

Ukázková aplikace používá `TodoItem` třída pro data modelu. K ukládání `TodoItem` položek ve webové službě, nejprve převeďte k proxy serveru generované `TodoItem` typu. To lze provést `ToASMXServiceTodoItem` metoda, jak je znázorněno v následujícím příkladu kódu:

```csharp
ASMXService.TodoItem ToASMXServiceTodoItem (TodoItem item)
{
  return new ASMXService.TodoItem {
    ID = item.ID,
    Name = item.Name,
    Notes = item.Notes,
    Done = item.Done
  };
}
```

Tato metoda jednoduše vytvoří novou `ASMService.TodoItem` instance a nastaví vlastnost identické z každou vlastnost `TodoItem` instance.

Podobně když data se načítají z webové služby, je nutné jej převést z proxy serveru generované `TodoItem` typ, který má `TodoItem` instance. To je provedeno pomocí `FromASMXServiceTodoItem` metoda, jak je znázorněno v následujícím příkladu kódu:

```csharp
static TodoItem FromASMXServiceTodoItem (ASMXService.TodoItem item)
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

`TodoService.BeginGetTodoItems` a `TodoService.EndGetTodoItems` metody se používají k volání `GetTodoItems` operace poskytované webovou službu. Tyto asynchronní metody jsou zapouzdřené v `Task` objektu, jak je znázorněno v následujícím příkladu kódu:

```csharp
public async Task<List<TodoItem>> RefreshDataAsync ()
{
  ...
  var todoItems = await Task.Factory.FromAsync<ASMXService.TodoItem[]> (
    todoService.BeginGetTodoItems,
    todoService.EndGetTodoItems,
    null,
    TaskCreationOptions.None);

  foreach (var item in todoItems) {
    Items.Add (FromASMXServiceTodoItem (item));
  }
  ...
}
```

`Task.Factory.FromAsync` Metoda vytvoří `Task` , která se spouští `TodoService.EndGetTodoItems` metoda jednou `TodoService.BeginGetTodoItems` metoda dokončí, s `null` parametr určující, zda je předávána žádná data do `BeginGetTodoItems` delegovat. Nakonec hodnotu `TaskCreationOptions` – výčet Určuje, že má být použita výchozí chování pro vytváření a spouštění úloh.

`TodoService.EndGetTodoItems` Metoda vrátí pole `ASMXService.TodoItem` instance, které se potom převedou do `List` z `TodoItem` instance pro zobrazení.

### <a name="creating-data"></a>Vytváření dat

`TodoService.BeginCreateTodoItem` a `TodoService.EndCreateTodoItem` metody se používají k volání `CreateTodoItem` operace poskytované webovou službu. Tyto asynchronní metody jsou zapouzdřené v `Task` objektu, jak je znázorněno v následujícím příkladu kódu:

```csharp
public async Task SaveTodoItemAsync (TodoItem item, bool isNewItem = false)
{
  ...
  var todoItem = ToASMXServiceTodoItem (item);
  ...
  await Task.Factory.FromAsync (
    todoService.BeginCreateTodoItem,
    todoService.EndCreateTodoItem,
    todoItem,
    TaskCreationOptions.None);
  ...
}
```

`Task.Factory.FromAsync` Metoda vytvoří `Task` , která se spouští `TodoService.EndCreateTodoItem` metoda jednou `TodoService.BeginCreateTodoItem` dokončení metoda s `todoItem` parametr se data, která je předána do `BeginCreateTodoItem` delegáta k určení `TodoItem` vytvořit webovou službou. Nakonec hodnotu `TaskCreationOptions` – výčet Určuje, že má být použita výchozí chování pro vytváření a spouštění úloh.

Vyvolává službu web `SoapException` Pokud se nepodaří vytvořit `TodoItem`, která zpracovává aplikací.

### <a name="updating-data"></a>Aktualizace dat

`TodoService.BeginEditTodoItem` a `TodoService.EndEditTodoItem` metody se používají k volání `EditTodoItem` operace poskytované webovou službu. Tyto asynchronní metody jsou zapouzdřené v `Task` objektu, jak je znázorněno v následujícím příkladu kódu:

```csharp
public async Task SaveTodoItemAsync (TodoItem item, bool isNewItem = false)
{
  ...
  var todoItem = ToASMXServiceTodoItem (item);
  ...
  await Task.Factory.FromAsync (
    todoService.BeginEditTodoItem,
    todoService.EndEditTodoItem,
    todoItem,
    TaskCreationOptions.None);
  ...
}
```

`Task.Factory.FromAsync` Metoda vytvoří `Task` , která se spouští `TodoService.EndEditTodoItem` metoda jednou `TodoService.BeginCreateTodoItem` dokončení metoda s `todoItem` parametr se data, která je předána do `BeginEditTodoItem` delegáta k určení `TodoItem` aktualizovat webovou službou. Nakonec hodnotu `TaskCreationOptions` – výčet Určuje, že má být použita výchozí chování pro vytváření a spouštění úloh.

Vyvolává službu web `SoapException` Pokud se nepodaří najít, nebo aktualizace `TodoItem`, který zpracovává aplikací.

### <a name="deleting-data"></a>Odstraňování dat

`TodoService.BeginDeleteTodoItem` a `TodoService.EndDeleteTodoItem` metody se používají k volání `DeleteTodoItem` operace poskytované webovou službu. Tyto asynchronní metody jsou zapouzdřené v `Task` objektu, jak je znázorněno v následujícím příkladu kódu:

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

`Task.Factory.FromAsync` Metoda vytvoří `Task` , která se spouští `TodoService.EndDeleteTodoItem` metoda jednou `TodoService.BeginDeleteTodoItem` dokončení metoda s `id` parametr se data, která je předána do `BeginDeleteTodoItem` delegáta k určení `TodoItem` odstranit webovou službou. Nakonec hodnotu `TaskCreationOptions` – výčet Určuje, že má být použita výchozí chování pro vytváření a spouštění úloh.

Vrátí služby webové `SoapException` Pokud se nepodaří najít, nebo odstranit `TodoItem`, který zpracovává aplikací.

## <a name="summary"></a>Souhrn

Tento článek ukázal, jak využívají webové služby ASMX z Xamarin.Forms aplikace. ASMX poskytuje možnost vytvářet webové služby, které odesílání zpráv přes protokol HTTP pomocí protokolu SOAP. Příjemci služby ASMX není potřeba nic vědět o platformu, objektový model nebo programovací jazyk používaný k implementaci služby. Potřebují pouze pochopit, jak odesílat a přijímat zprávy protokolu SOAP.


## <a name="related-links"></a>Související odkazy

- [TodoASMX (ukázka)](https://developer.xamarin.com/samples/xamarin-forms/WebServices/TodoASMX/)
- [IAsyncResult](https://msdn.microsoft.com/library/system.iasyncresult(v=vs.110).aspx)
