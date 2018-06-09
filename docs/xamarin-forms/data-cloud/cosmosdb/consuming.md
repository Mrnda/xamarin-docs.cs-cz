---
title: Využívání dokumentu databázi Azure Cosmos DB
description: Tento článek vysvětluje, jak pomocí Azure Cosmos DB .NET standardní klientské knihovny integrovat do aplikace Xamarin.Forms databázi dokumentů Azure Cosmos DB.
ms.prod: xamarin
ms.assetid: 7C0605D9-9B7F-4002-9B60-2B5DAA3EA30C
ms.technology: xamarin-forms
ms.custom: xamu-video
author: davidbritch
ms.author: dabritch
ms.date: 06/16/2017
ms.openlocfilehash: 6e797eaad98f6fac66876aaebecd7ae53ad9dbab
ms.sourcegitcommit: 66682dd8e93c0e4f5dee69f32b5fc5a96443e307
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 06/08/2018
ms.locfileid: "35242502"
---
# <a name="consuming-an-azure-cosmos-db-document-database"></a>Využívání dokumentu databázi Azure Cosmos DB

_Databázi dokumentů Azure Cosmos DB je databáze NoSQL, která poskytuje přístup s nízkou latencí k dokumentů JSON, nabízí rychlý, vysoce dostupných, škálovatelných databázová služba pro aplikace, které vyžadují bezproblémové škálování a globální replikace. Tento článek vysvětluje, jak pomocí Azure Cosmos DB .NET standardní klientské knihovny integrovat do aplikace Xamarin.Forms databázi dokumentů Azure Cosmos DB._

> [!VIDEO https://youtube.com/embed/BoVH12igmbg]

**Microsoft Azure Cosmos DB, pomocí [univerzity Xamarin](https://university.xamarin.com/)**

Účet databáze dokumentů Azure Cosmos DB se dá zřídit pomocí předplatného Azure. Každý účet databáze může mít nula nebo více databází. Databázi dokumentů v Azure Cosmos DB je logický kontejner pro kolekce dokumentů a uživatele.

Databázi dokumentů Azure Cosmos DB může obsahovat nula nebo více kolekcí dokumentu. Každá kolekce dokumentů může mít různé výkonu úrovni, umožňuje větší propustnost pro často používaná kolekcí a méně propustnost pro kolekce málokdy načítaná.

Každá kolekce dokument se skládá z nula nebo více dokumentů JSON. Dokumenty v kolekci jsou bez schémat a proto není potřeba sdílet stejnou strukturu nebo pole. Jako dokumenty jsou přidány do kolekce dokumentů, Cosmos DB automaticky indexuje je a budou k dispozici být dotazována.

Pro účely vývoje mohou být také využívány databázi dokumentů prostřednictvím emulátor. Pomocí emulátoru, aplikace vyvinuté a testování bude místně, bez vytváření předplatného Azure nebo nákladům. Další informace o emulátoru najdete v tématu [vývoj místně pomocí emulátoru Azure DB Cosmos](/azure/cosmos-db/local-emulator/).

V tomto článku a doplňujícími ukázkovou aplikaci, ukazuje, kde jsou uloženy úkoly v databázi dokumentů Azure Cosmos DB aplikaci seznamu úkolů. Další informace o ukázkovou aplikaci najdete v tématu [pochopení vzorku](~/xamarin-forms/data-cloud/walkthrough.md).

Další informace o databázi Cosmos Azure najdete v tématu [dokumentaci k Azure Cosmos DB](/azure/cosmos-db/).

## <a name="setup"></a>Instalace

Proces pro integraci databázi dokumentů Azure Cosmos DB na aplikaci Xamarin.Forms vypadá takto:

1. Vytvořte účet Cosmos DB. Další informace najdete v tématu [vytvoření účtu Azure Cosmos DB](/azure/cosmos-db/sql-api-dotnetcore-get-started#step-1-create-an-azure-cosmos-db-account).
1. Přidat [Azure Cosmos DB .NET Standard klientské knihovny](https://www.nuget.org/packages/Microsoft.Azure.DocumentDB.Core) balíček NuGet, abyste platformy projekty v řešení Xamarin.Forms.
1. Přidat `using` direktivy pro `Microsoft.Azure.Documents`, `Microsoft.Azure.Documents.Client`, a `Microsoft.Azure.Documents.Linq` obory názvů do třídy, které bude přístup k účtu Cosmos DB.

Po provedení těchto kroků, Azure Cosmos DB .NET Standard klientské knihovny slouží ke konfiguraci a spuštění požadavky DHCP proti databázi dokumentů.

> [!NOTE]
> Azure Cosmos DB .NET Standard klientské knihovny lze nainstalovat pouze do projektů platformy a ne do projektu přenosných třída knihovny PCL (). Ukázkové aplikace je proto sdílený přístup k projektu (SAP) Chcete-li zabránit zdvojení kódu. Ale `DependencyService` třídy lze použít v projektu PCL k vyvolání obsažené v projektech specifické pro platformu Azure Cosmos DB .NET standardní kód knihovny klienta.

## <a name="consuming-the-azure-cosmos-db-account"></a>Použití účtu Azure Cosmos DB

`DocumentClient` Typ zapouzdří koncový bod, přihlašovací údaje a připojení zásady používané pro přístup k účtu Azure Cosmos DB a slouží ke konfiguraci a spuštění požadavků na účet. Následující příklad kódu ukazuje, jak vytvořit instanci této třídy:

```csharp
DocumentClient client = new DocumentClient(new Uri(Constants.EndpointUri), Constants.PrimaryKey);
```

Je třeba zadat Cosmos DB Uri a primární klíč k `DocumentClient` konstruktor. To je možné získat na portálu Azure. Další informace najdete v tématu [připojit k účtu Azure Cosmos DB](/azure/cosmos-db/sql-api-dotnetcore-get-started#Connect).

### <a name="creating-a-database"></a>Vytvoření databáze

Dokument databáze je logický kontejner pro kolekce dokumentů a uživatelé a může být vytvořený na portálu Azure nebo programově pomocí `DocumentClient.CreateDatabaseIfNotExistsAsync` metoda:

```csharp
public async Task CreateDatabase(string databaseName)
{
  ...
  await client.CreateDatabaseIfNotExistsAsync(new Database
  {
    Id = databaseName
  });
  ...
}
```

`CreateDatabaseIfNotExistsAsync` Určuje metoda `Database` objektu jako argument, s `Database` objekt zadání názvu databáze jako jeho `Id` vlastnost. `CreateDatabaseIfNotExistsAsync` Metoda vytvoří databázi, pokud neexistuje, nebo vrátí databáze, pokud již existuje. Ale ukázkovou aplikaci ignoruje žádná data vrácený `CreateDatabaseIfNotExistsAsync` metoda.

> [!NOTE]
> `CreateDatabaseIfNotExistsAsync` Metoda vrátí `Task<ResourceResponse<Database>>` objekt a stavový kód odpovědi jde zkontrolovat k určení, zda byl vytvořen databázi nebo existující databáze byla vrácena.

### <a name="creating-a-document-collection"></a>Vytvoření kolekce dokumentů

Kolekce dokumentů je kontejner dokumentů JSON a může být vytvořený na portálu Azure nebo programově pomocí `DocumentClient.CreateDocumentCollectionIfNotExistsAsync` metoda:

```csharp
public async Task CreateDocumentCollection(string databaseName, string collectionName)
{
  ...
  // Create collection with 400 RU/s
  await client.CreateDocumentCollectionIfNotExistsAsync(
    UriFactory.CreateDatabaseUri(databaseName),
    new DocumentCollection
    {
      Id = collectionName
    },
    new RequestOptions
    {
      OfferThroughput = 400
    });
  ...
}
```

`CreateDocumentCollectionIfNotExistsAsync` Metoda vyžaduje dva argumenty povinné – jako zadaný název databáze `Uri`a `DocumentCollection` objektu. `DocumentCollection` Objekt představuje kolekci dokumentů, jejíž název je zadán s `Id` vlastnost. `CreateDocumentCollectionIfNotExistsAsync` Metoda vytvoří kolekce dokumentů, pokud neexistuje, nebo vrátí kolekce dokumentů, pokud již existuje. Ale ukázkovou aplikaci ignoruje žádná data vrácený `CreateDocumentCollectionIfNotExistsAsync` metoda.

> [!NOTE]
> `CreateDocumentCollectionIfNotExistsAsync` Metoda vrátí `Task<ResourceResponse<DocumentCollection>>` objekt a stavový kód odpovědi jde zkontrolovat k určení, zda byl vytvořen kolekce dokumentů nebo vrátila existující kolekci dokumentu.

Volitelně můžete `CreateDocumentCollectionIfNotExistsAsync` metoda můžete také zadat `RequestOptions` objekt, který zapouzdřuje možnosti, které lze zadat pro požadavky vydané k účtu Cosmos DB. `RequestOptions.OfferThroughput` Vlastnost se používá k definování úroveň výkonu kolekce dokumentů, a v ukázce je aplikace nastavena na 400 jednotek žádosti za sekundu. Tato hodnota by měla být zvětšit nebo zmenšit v závislosti na tom, zda kolekce se bude často nebo zřídka přistupovat k.

> [!IMPORTANT]
> Všimněte si, že `CreateDocumentCollectionIfNotExistsAsync` metoda vytvoří novou kolekci s vyhrazenou propustností, kterou se hradí.

<a name="document_query" />

### <a name="retrieving-document-collection-documents"></a>Načítání dokumentu kolekce dokumentů

Obsah dokumentu kolekce můžete načíst pomocí vytvoření a spuštění dotazu na dokumentu. Dotaz dokumentu je vytvořena s `DocumentClient.CreateDocumentQuery` metoda:

```csharp
public async Task<List<TodoItem>> GetTodoItemsAsync()
{
  ...
  var query = client.CreateDocumentQuery<TodoItem>(collectionLink)
            .AsDocumentQuery();
  while (query.HasMoreResults)
  {
    Items.AddRange(await query.ExecuteNextAsync<TodoItem>());
  }
  ...
}
```

Tento dotaz asynchronně načte všechny dokumenty v zadané kolekci a umístí dokumentů v `List<TodoItem>` kolekce pro zobrazení.

`CreateDocumentQuery<T>` Určuje metoda `Uri` argument, který představuje kolekci, která mají být získána pro dokumenty. V tomto příkladu `collectionLink` proměnné je úrovni třídy pole, které určuje `Uri` představující kolekci dokument k načtení dokumenty z:

```csharp
Uri collectionLink = UriFactory.CreateDocumentCollectionUri(Constants.DatabaseName, Constants.CollectionName);
```

`CreateDocumentQuery<T>` Metoda vytvoří dotaz, který spouští synchronně a vrátí `IQueryable<T>` objektu. Ale `AsDocumentQuery` metoda převede `IQueryable<T>` do objektu `IDocumentQuery<T>` objekt, který mohou být spouštěny asynchronně. Asynchronní dotaz proveden s `IDocumentQuery<T>.ExecuteNextAsync` metodu, která načte další stránky výsledků z databáze dokumentů s `IDocumentQuery<T>.HasMoreResults` vlastnost, která určuje, jestli existují další výsledky má být vrácen z dotazu.

Dokumenty můžou být Filtroval včetně na straně serveru `Where` klauzule dotazu, která se vztahuje k dotazu vůči kolekce dokumentů predikát pro filtrování:

```csharp
var query = client.CreateDocumentQuery<TodoItem>(collectionLink)
          .Where(f => f.Done != true)
          .AsDocumentQuery();
```

Tento dotaz načte všechny dokumenty z kolekce, jehož `Done` vlastnost rovná `false`.

<a name="inserting_document" />

### <a name="inserting-a-document-into-a-document-collection"></a>Vkládání dokumentu do kolekce dokumentů

Dokumenty jsou uživatelem definovaný obsah JSON a lze vložit do kolekce dokumentů s `DocumentClient.CreateDocumentAsync` metoda:

```csharp
public async Task SaveTodoItemAsync(TodoItem item, bool isNewItem = false)
{
  ...
  await client.CreateDocumentAsync(collectionLink, item);
  ...
}
```

`CreateDocumentAsync` Určuje metoda `Uri` argument, který představuje kolekci dokumentu musí být zařazeny do, a `object` argument, který představuje dokumentu, který má být vložen.

### <a name="replacing-a-document-in-a-document-collection"></a>Nahrazení dokumentu v kolekci dokumentů

Dokumenty lze nahradit v kolekci dokumentů s `DocumentClient.ReplaceDocumentAsync` metoda:

```csharp
public async Task SaveTodoItemAsync(TodoItem item, bool isNewItem = false)
{
  ...
  await client.ReplaceDocumentAsync(UriFactory.CreateDocumentUri(Constants.DatabaseName, Constants.CollectionName, item.Id), item);
  ...
}
```

`ReplaceDocumentAsync` Určuje metoda `Uri` argument, který představuje dokumentu v kolekci, která by měla být nahrazená, a `object` argument, který představuje data aktualizované dokumentu.

<a name="deleting_document" />

### <a name="deleting-a-document-from-a-document-collection"></a>Odstranění dokumentu z kolekce dokumentů

Dokument můžete odstranit z kolekce dokumentů s `DocumentClient.DeleteDocumentAsync` metoda:

```csharp
public async Task DeleteTodoItemAsync(string id)
{
  ...
  await client.DeleteDocumentAsync(UriFactory.CreateDocumentUri(Constants.DatabaseName, Constants.CollectionName, id));
  ...
}
```

`DeleteDocumentAsync` Určuje metoda `Uri` argument, který představuje dokumentu v kolekci, měla by být odstraněna.

### <a name="deleting-a-document-collection"></a>Odstraňuje se kolekce dokumentů

Kolekce dokumentů můžete odstranit z databáze se `DocumentClient.DeleteDocumentCollectionAsync` metoda:

```csharp
await client.DeleteDocumentCollectionAsync(collectionLink);
```

`DeleteDocumentCollectionAsync` Určuje metoda `Uri` argument, který představuje kolekci dokumentů k odstranění. Všimněte si, dokumenty uložené v kolekci odstraní také volání této metody.

### <a name="deleting-a-database"></a>Odstranění databáze

Databázi můžete odstranit z databáze Cosmos databázový účet se `DocumentClient.DeleteDatabaesAsync` metoda:

```csharp
await client.DeleteDatabaseAsync(UriFactory.CreateDatabaseUri(Constants.DatabaseName));
```

`DeleteDatabaseAsync` Určuje metoda `Uri` argument, který reprezentuje databázi pro odstranění. Všimněte si, že volání této metody budou odstraněny také uloženy v databázi kolekce dokumentů a dokumenty uložené v kolekcích dokumentů.

## <a name="summary"></a>Souhrn

Tento článek vysvětlení, jak používat Azure Cosmos DB .NET standardní klientské knihovny pro integraci databázi dokumentů Azure Cosmos DB na aplikaci Xamarin.Forms. Databázi dokumentů Azure Cosmos DB je databáze NoSQL, která poskytuje přístup s nízkou latencí k dokumentů JSON, nabízí rychlý, vysoce dostupných, škálovatelných databázová služba pro aplikace, které vyžadují bezproblémové škálování a globální replikace.


## <a name="related-links"></a>Související odkazy

- [TODO Azure Cosmos DB (ukázka)](https://developer.xamarin.com/samples/xamarin-forms/WebServices/TodoDocumentDB/)
- [Dokumentace Azure Cosmos DB](/azure/cosmos-db/)
- [Klientská knihovna pro Azure Cosmos DB .NET Standard](https://www.nuget.org/packages/Microsoft.Azure.DocumentDB.Core)
- [Rozhraní API Azure Cosmos DB](https://docs.microsoft.com/en-us/dotnet/api/overview/azure/cosmosdb/client?view=azure-dotnet)
