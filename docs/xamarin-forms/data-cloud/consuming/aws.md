---
title: Využívání služby Amazon SimpleDB
description: Amazon SimpleDB je webová služba, která poskytuje možnost ukládání a dotazování dat v cloudu na Amazon. Tento článek vysvětluje, jak pomocí AWS sady SDK pro .NET dotazu, vytvářet, nahraďte a odstraňovat data uložená ve službě SimpleDB.
ms.prod: xamarin
ms.assetid: 823819AA-15F9-4144-B355-78A10AD37513
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 09/20/2016
ms.openlocfilehash: 1602319dbf5a5d00ac5de75f2d438b9aea692699
ms.sourcegitcommit: 271d3f7ea4abfcf87734d2c747a68cb8114d743c
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/12/2018
---
# <a name="consuming-an-amazon-simpledb-service"></a>Využívání služby Amazon SimpleDB

_Amazon SimpleDB je webová služba, která poskytuje možnost ukládání a dotazování dat v cloudu na Amazon. Tento článek vysvětluje, jak pomocí AWS sady SDK pro .NET dotazu, vytvářet, nahraďte a odstraňovat data uložená ve službě SimpleDB._

SimpleDB služeb používají žádosti a odpovědi model pro příjemce služby REST. Operace jsou vyvolány na službu SimpleDB odesláním požadavku, které mohou obsahovat data. Po zpracování požadavku, službu SimpleDB vrátí odpověď obsahující žádné výsledky. SimpleDB služby musí být vytvořeny prostřednictvím kódu programu a nedá se vytvořit pomocí [konzoly AWS](https://aws.amazon.com). Účet AWS je však potřeba vytvořit a přístup k žádné webové služby Amazon.

Ve službě SimpleDB data jsou uspořádány do domény, v rámci kterých se může data a spouštění dotazů na data. Domény se skládají z položek, které jsou popsány podle dvojic název hodnota atributu. Domény můžete představit jako probíhá s atributy, se podobně jako sloupce a položky se podobně jako řádky tabulky, podobně jako. Další informace o SimpleDB datového modelu najdete v tématu [datový Model](http://docs.aws.amazon.com/AmazonSimpleDB/latest/DeveloperGuide/DataModel.html) na webu Amazon společnosti.

Pokyny k nastavení požadované služby Amazon naleznete v souboru readme, který doprovází ukázkovou aplikaci. Při spuštění ukázkové aplikace, se připojí k fondu Amazon Cognito identity autorizace přístupu k SimpleDB služby, jak je znázorněno na následujícím snímku obrazovky:

![](aws-images/portal.png "Ukázkové aplikace")

> [!NOTE]
> Zabezpečení přenosu aplikace (ATS) vynucuje v iOS 9 a vyšší, zabezpečené připojení mezi prostředků z Internetu (třeba server back-end aplikace) a aplikace, a tím prevence náhodného zpřístupnění citlivých informací. Protože ATS je povolené ve výchozím nastavení v aplikacích pro iOS 9, všechna připojení budou platit ATS požadavky na zabezpečení. Pokud připojení těchto požadavků nesplňuje, budou se nezdaří s výjimkou.
> ATS můžete vyjádřit výslovný souhlas mimo Pokud není možné použít `HTTPS` protokolu a zabezpečení komunikace pro prostředků z Internetu. Toho lze dosáhnout aktualizací aplikace **Info.plist** souboru. Další informace najdete v části [zabezpečení přenosu aplikace](~/ios/app-fundamentals/ats.md).

## <a name="consuming-a-simpledb-service"></a>Využívání služby SimpleDB

Amazon Cognito Identity umožňuje AWS služby, jako je SimpleDB má být volána z aplikace do aplikace bez přihlašovacích údajů AWS přímo v kódu. Místo toho jedinečnou identitu fondu se vytvoří v [konzole Amazon Cognito](https://console.aws.amazon.com/cognito/home). Identita fondu obsahuje identity, která používá rolí k určení prostředků, jako je například SimpleDB, kterému mají přístup identitu.

[AWS SDK pro .NET](https://www.nuget.org/packages?q=Tags%3A%22aws-sdk-v3%22) poskytuje `CognitoAWSCredentials` a `AmazonSimpleDBClient` třídy, které jsou používány k aplikaci Xamarin.Forms přístup ke službě SimpleDB, jak je znázorněno v následujícím příkladu kódu:

```csharp
AmazonSimpleDBClient client;
...

public SimpleDBStorage ()
{
  var credentials = new CognitoAWSCredentials (
                      Constants.CognitoIdentityPoolId,
                      RegionEndpoint.USEast1);
  var config = new AmazonSimpleDBConfig ();
  config.RegionEndpoint = RegionEndpoint.USWest2;
  client = new AmazonSimpleDBClient (credentials, config);
  ...
}
```

Novou instanci třídy `CognitoAWSCredentials` třída se vytvoří zadáním id jedinečnou identitu fondu a oblast účtu Cognito Identity. V době psaní Cognito Identity je dostupná jenom v oblastech USEast1 a EUWest1. Však může komunikovat s služby Amazon mimo těchto oblastí.

Když `AmazonSimpleDBClient` se vytvoří instance, `CognitoAWSCredentials` instance musí být zadán spolu s `AmazonSimpleDBConfig` instanci, která určuje zeměpisnou oblast, kde se služba SimpleDB nachází. `CognitoAWSCredentials` Instance zajišťuje, že SimpleDB služby, které je přístupné ten, který je přidružený k účtu AWS, ve kterém byla vytvořena identity fondu, aniž by bylo třeba vkládat AWS přístupový klíč a tajný klíč v aplikaci.

Vytvoření domény služby SimpleDB voláním `AmazonSimpleDBClient.CreateDomainAsync` metoda, jak je znázorněno v následujícím příkladu kódu:

```csharp
string tableName = "Todo";
...

async Task CreateDomain ()
{
  ...
  await client.CreateDomainAsync (new CreateDomainRequest { DomainName = tableName });
  ...
}
```

`CreateDomainAsync` Metoda vyžaduje, `CreateDomainRequest` instance jako parametr. `CreateDomainRequest` Instance inicializuje `DomainName` vlastnost na hodnotu, která použije k identifikaci domény. Pokud chcete vytvořit doménu, tato hodnota musí být jedinečný mezi domény přidružené k účtu AWS. Jinak nebude vytvořena domény a odešlou se žádné chybové odpovědi k označení, že. Všechny operace u názvu domény pak dojde před existující domény, nikoli nově vytvořené domény.

### <a name="creating-simpledb-objects"></a>Vytváření objektů SimpleDB

Ukázková aplikace používá `TodoItem` třída pro data modelu. K ukládání `TodoItem` instance ve službě SimpleDB nejprve převeďte do `List` z `ReplaceableAttribute` objekty. To lze provést `ToSimpleDBReplaceableAttributes` metoda, jak je znázorněno v následujícím příkladu kódu:

```csharp
List<ReplaceableAttribute> ToSimpleDBReplaceableAttributes (TodoItem item)
{
  return new List<ReplaceableAttribute> () {
    new ReplaceableAttribute () {
      Name = "Name",
      Value = item.Name,
      Replace = true
    },
    new ReplaceableAttribute () {
      Name = "Notes",
      Value = item.Notes,
      Replace = true
    },
    new ReplaceableAttribute () {
      Name = "Done",
      Value = item.Done.ToString (),
      Replace = true
    }
  };
}
```

Tato metoda vytvoří `List` z nové `ReplaceableAttribute` instancí s `List` představující jedné `TodoItem` instance. Každý `ReplaceableAttribute` instance představuje jedinou vlastnost z `TodoItem` instance. Další informace o `ReplaceableAttribute` třídy najdete v tématu [ReplaceableAttribute třída](http://docs.aws.amazon.com/sdkfornet1/latest/apidocs/html/T_Amazon_SimpleDB_Model_ReplaceableAttribute.htm) na webu Amazon společnosti.

Podobně platí, když je načíst data ze služby SimpleDB, je nutné jej převést z `List` z `Attribute` instance k `TodoItem` instance. To je provedeno pomocí `FromSimpleDBAttributes` metoda, jak je znázorněno v následujícím příkladu kódu:

```csharp
TodoItem FromSimpleDBAttributes (List<Amazon.SimpleDB.Model.Attribute> attributeList, string id)
{
  var todoItem = new TodoItem ();
  todoItem.ID = id;
  todoItem.Name = attributeList.Where (attr => attr.Name == "Name").FirstOrDefault ().Value;
  todoItem.Notes = attributeList.Where (attr => attr.Name == "Notes").FirstOrDefault ().Value;
  todoItem.Done = Convert.ToBoolean (attributeList.Where (attr => attr.Name == "Done").FirstOrDefault ().Value);
  return todoItem;
}
```

Tato metoda jednoduše načte každý `Attribute` instanci z `List` a nastaví jej do nově vytvořené `TodoItem` instance.

Další informace o `Attribute` třídy najdete v tématu [třídy atributů](http://docs.aws.amazon.com/sdkfornet1/latest/apidocs/html/T_Amazon_SimpleDB_Model_Attribute.htm) na webu Amazon společnosti.

### <a name="querying-data"></a>Dotazování na Data

Může načíst obsah domény volání `AmazonSimpleDBClient.SelectAsync` metoda, jak je znázorněno v následujícím příkladu kódu:

```csharp
public async Task<List<TodoItem>> RefreshDataAsync ()
{
  ...
  var request = new SelectRequest () {
      SelectExpression = string.Format ("SELECT * from {0}", tableName)
  };
  var response = await client.SelectAsync (request);
  foreach (var item in response.Items) {
    Items.Add (FromSimpleDBAttributes (item.Attributes, item.Name));
  }
  ...
}
```

`SelectAsync` Metoda přijímá `SelectRequest` instance jako parametr, který určuje `Select` dotaz výrazu v jeho `SelectExpression` vlastnost. Formát výrazu dotazu je podobný formát standardní SQL `SELECT` příkaz. Další informace o výrazu dotazu najdete v tématu [pomocí vyberte vytvořit dotazy SimpleDB Amazon](http://docs.aws.amazon.com/AmazonSimpleDB/latest/DeveloperGuide/UsingSelect.html) na webu Amazon společnosti.

> [!NOTE]
> Pečlivě dodržujte z hlediska stylu citací pravidla při sestavování výrazu dotazu. Další informace najdete v tématu [vyberte citací pravidla](http://docs.aws.amazon.com/AmazonSimpleDB/latest/DeveloperGuide/QuotingRulesSelect.html) na webu Amazon společnosti.

`SelectAsync` Metoda vrátí odpověď obsahující kolekci položek a přidružených atributů, které odpovídají výrazu dotazu. Tato kolekce je potom převedou do `List` z `TodoItem` instance pro zobrazení.

### <a name="creating-and-replacing-data"></a>Vytváření a nahrazení dat

`AmazonSimpleDBClient.PutAttributesAsync` Metoda se používá k vytvoření a nahradit data v doméně služby SimpleDB, jak je znázorněno v následujícím příkladu kódu:

```csharp
public async Task SaveTodoItemAsync (TodoItem todoItem)
{
  ...
  var attributeList = ToSimpleDBReplaceableAttributes (todoItem);
  var request = new PutAttributesRequest () {
      DomainName = tableName,
      ItemName = todoItem.ID,
      Attributes = attributeList
  };
  await client.PutAttributesAsync (request);
  ...
}
```

`PutAttributesAsync` Metoda přijímá `PutAttributesRequest` instance jako parametr. `PutAttributesRequest` Instance Určuje dvojice název hodnota atributu, které mají být vytvořeny jako nové položky, nebo ve stávající položku. `List` z `ReplaceableAttribute` instance je sestavena `ToSimpleDBReplaceableAttributes` metoda. Tato metoda také nastavuje `Replace` vlastnost jednotlivých `ReplaceableAttribute` k `true`. To způsobí, že nová hodnota atributu má nahradit existující hodnotu atributu, pokud se nahrazuje data. Ale pokusu nahraďte hodnoty atributů, které nejsou k dispozici nebude mít za následek chybové odpovědi.

Hodnota `PutAttributesRequest.ItemName` vlastnost určuje, jestli se nová položka se zařadí do domény nebo jestli se nahradí stávající položku. Aplikace vytvoří novou položku, nastaví `TodoItem.ID` vlastnost na nový `Guid`. To zajišťuje, aby se každý `TodoItem` instance má jedinečný identifikátor. Proto pokud `PutAttributesRequest.ItemName` je nastavena na hodnotu, která neexistuje v doméně, služba SimpleDB vytvoří novou položku obsahující dvojice název hodnota zadaného atributu. Pokud `PutAttributesRequest.ItemName` je nastavena na hodnotu, která již existuje v doméně, službu SimpleDB dojde k aktualizaci položky s páry název hodnota zadaného atributu.

### <a name="deleting-data"></a>Odstraňování dat

`AmazonSimpleDBClient.DeleteAttributesAsync` Metoda se používá k odstranění dat z domény SimpleDB služby, jak je znázorněno v následujícím příkladu kódu:

```csharp
public async Task DeleteTodoItemAsync (TodoItem todoItem)
{
  ...
  var attributeList = ToSimpleDBAttributes (todoItem);
  var request = new DeleteAttributesRequest () {
      DomainName = tableName,
      ItemName = todoItem.ID,
      Attributes = attributeList
  };
  await client.DeleteAttributesAsync (request);
  ...
}
```

`DeleteAttributesAsync` Metoda přijímá `DeleteAttributesRequest` instance jako parametr.  `DeleteAttributesRequest` Instance Určuje atributy, které mají být odstraněny z položky s `List` z `Attribute` instance k odstranění právě vytvořené `ToSimpleDBAttributes` metoda. Položka je odstraněna, za předpokladu, že všechny atributy položky se odstraní.

## <a name="summary"></a>Souhrn

Tento článek vysvětluje používání AWS SDK pro .NET pro dotazování, vytvořit a nahradit a odstranit data uložená ve službě SimpleDB. Tato sada SDK obsahuje `CognitoAWSCredentials` a `AmazonSimpleDBClient` třídy, které jsou používány k aplikaci Xamarin.Forms přístup ke službě SimpleDB.


## <a name="related-links"></a>Související odkazy

- [TodoAWS (sample)](https://developer.xamarin.com/samples/xamarin-forms/WebServices/TodoAWS/)
- [Příručka vývojáře pro Xamarin SDK služby Amazon Web Services](http://docs.aws.amazon.com/mobile/sdkforxamarin/developerguide/)
- [Amazon Cognito Identity](http://docs.aws.amazon.com/cognito/devguide/identity/)
- [Dokumentaci pro vývojáře SimpleDB Amazon](http://docs.aws.amazon.com/AmazonSimpleDB/latest/DeveloperGuide/Welcome.html)
- [AmazonSimpleDBClient Class](http://docs.aws.amazon.com/sdkfornet1/latest/apidocs/html/T_Amazon_SimpleDB_AmazonSimpleDBClient.htm)
- [Amazon Web Services SDK pro .NET](https://www.nuget.org/packages?q=Tags%3A%22aws-sdk-v3%22)
