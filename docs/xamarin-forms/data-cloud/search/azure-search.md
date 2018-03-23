---
title: Vyhledávání dat s Azure Search
description: Vyhledávání systému Azure je Cloudová služba, která poskytuje indexování a dotazování možnosti pro odeslaná data. Touto akcí odeberete, požadavky na infrastrukturu a vyhledávací algoritmus složité kroky tradičně spojené s implementací funkce vyhledávání v aplikaci. Tento článek ukazuje, jak integrovat Azure Search na aplikaci Xamarin.Forms pomocí knihovnu Microsoft Azure Search.
ms.topic: article
ms.prod: xamarin
ms.assetid: A4AEF233-3672-4174-9DBA-15BEE3030C0B
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 12/05/2016
ms.openlocfilehash: 24db1404e218eea86356f9bbc004e7d5850c2e7a
ms.sourcegitcommit: 7b76c3d761b3ffb49541e2e2bcf292de6587c4e7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 03/23/2018
---
# <a name="searching-data-with-azure-search"></a>Vyhledávání dat s Azure Search

_Vyhledávání systému Azure je Cloudová služba, která poskytuje indexování a dotazování možnosti pro odeslaná data. Touto akcí odeberete, požadavky na infrastrukturu a vyhledávací algoritmus složité kroky tradičně spojené s implementací funkce vyhledávání v aplikaci. Tento článek ukazuje, jak integrovat Azure Search na aplikaci Xamarin.Forms pomocí knihovnu Microsoft Azure Search._

## <a name="overview"></a>Přehled

Data se ukládají ve službě Azure Search jako indexům a dokumentům. *Index* je úložiště dat, které lze vyhledat pomocí služby Azure Search a je koncepčně podobá do databázové tabulky. A *dokumentu* jednu jednotku s možností vyhledávání dat v indexu a je koncepčně podobá řádku databáze. Při nahrávání dokumentů a odesílání vyhledávacích dotazů do služby Azure Search, jsou vytvářeny požadavky do konkrétního indexu ve vyhledávací službu.

Každý požadavek do služby Azure Search musí obsahovat název služby a klíč rozhraní API. Existují dva typy klíč rozhraní API:

- *Klíče správce* udělit úplná práva ke všem operacím. To zahrnuje správu služby, vytvoření nebo odstranění indexů a datových zdrojů.
- *Klíče dotazů* udělují přístup jen pro čtení k indexům a dokumentům a má být používána aplikace, které vydávají požadavky hledání.

Nejčastější požadavek do služby Azure Search je při spuštění dotazu. Existují dva typy dotazu, který lze odeslat:

- A *vyhledávání* dotaz vyhledávání pro jednu nebo více položek v všechna prohledatelná pole v indexu. Vyhledávací dotazy jsou vytvořeny pomocí zjednodušenou syntaxi nebo syntaxe dotazů Lucene. Další informace najdete v tématu [jednoduchá syntaxe dotazů ve službě Azure Search](/rest/api/searchservice/Simple-query-syntax-in-Azure-Search/), a [syntaxe dotazů Lucene ve službě Azure Search](/rest/api/searchservice/Lucene-query-syntax-in-Azure-Search/).
- A *filtru* dotazu vyhodnocuje logický výraz na všech filtrovatelných polí v indexu. Dotazů filtru jsou vytvořeny pomocí podmnožinu jazyka filtrování OData. Další informace najdete v tématu [syntaxe výrazu OData pro službu Azure Search](/rest/api/searchservice/OData-Expression-Syntax-for-Azure-Search/).

Vyhledávací dotazy a dotazů filtru můžete použít samostatně nebo společně. Při použití společně dotazu filter se použije první na celý index, a potom se provede dotaz rozhraní vyhledávání na výsledky dotazu filter.

Služba Azure Search také podporuje načítání návrhy založené na vstup vyhledávání. Další informace najdete v tématu [návrhu dotazů](#suggestions).

## <a name="setup"></a>Instalace

Proces pro integraci Azure Search na aplikaci Xamarin.Forms vypadá takto:

1. Vytvoření služby Azure Search. Další informace najdete v tématu [vytvoření služby Azure Search pomocí portálu Azure](/azure/search/search-create-service-portal/).
1. Odeberte Silverlight jako cílové rozhraní z řešení Xamarin.Forms přenosných třída knihovny PCL (). To můžete udělat změnou profilem PCL žádné profil, který podporuje vývoj pro různé platformy, ale nepodporuje Silverlight, jako je například profil 151 nebo 92.
1. Přidat [knihovnu Microsoft Azure Search](https://www.nuget.org/packages/Microsoft.Azure.Search) balíček NuGet do projektu PCL v řešení Xamarin.Forms.

Po provedení těchto kroků, rozhraní API služby Microsoft Search knihovny lze spravovat vyhledávání indexy a zdroje dat., odeslání a správě dokumentů a spouštět dotazy.

## <a name="creating-the-azure-search-index"></a>Vytvoření indexu Azure Search

Schématu indexu musí být definován, se mapuje na strukturu dat má proběhnout. To může být dokončené na portálu Azure nebo programově pomocí `SearchServiceClient` třídy. Tato třída spravuje připojení do služby Azure Search a slouží k vytvoření indexu. Následující příklad kódu ukazuje, jak vytvořit instanci této třídy:

```csharp
var searchClient =
  new SearchServiceClient(Constants.SearchServiceName, new SearchCredentials(Constants.AdminApiKey));
```

`SearchServiceClient` Přetížení konstruktoru přebírá název vyhledávací služby a `SearchCredentials` objektu jako argumenty, s `SearchCredentials` objekt zabalení *klíč správce* pro službu Azure Search. *Klíč správce* je potřeba vytvořit index.

> [!NOTE]
>  Jediný `SearchServiceClient` instance se používá v aplikaci k zabránilo otevírání příliš mnoha připojení do služby Azure Search.

Index je definovaný `Index` objektu, jak je ukázáno v následujícím příkladu kódu:

```csharp
static void CreateSearchIndex()
{
  var index = new Index()
  {
    Name = Constants.Index,
    Fields = new[]
    {
      new Field("id", DataType.String) { IsKey = true, IsRetrievable = true },
      new Field("name", DataType.String) { IsRetrievable = true, IsFilterable = true, IsSortable = true, IsSearchable = true },
      new Field("location", DataType.String) { IsRetrievable = true, IsFilterable = true, IsSortable = true, IsSearchable = true },
      new Field("details", DataType.String) { IsRetrievable = true, IsFilterable = true, IsSearchable = true },
      new Field("imageUrl", DataType.String) { IsRetrievable = true }
    },
    Suggesters = new[]
    {
      new Suggester("nameSuggester", SuggesterSearchMode.AnalyzingInfixMatching, new[] { "name" })
    }
  };

  searchClient.Indexes.Create(index);
}
```

`Index.Name` By měla být nastavena na název indexu a `Index.Fields` vlastnost musí být nastavená na pole `Field` objekty. Každý `Field` instance Určuje název, typ a všechny vlastnosti, které určují, jak se používá pole. Tyto vlastnosti:

- `IsKey` – Určuje, zda pole klíče indexu. Pouze jedno pole v indexu typu `DataType.String`, musí být určeny jako klíčové pole.
- `IsFacetable` – Určuje, zda je možné provést Fasetové navigace na toto pole. Výchozí hodnota je `false`.
- `IsFilterable` – Označuje, zda lze pole ve filtrovacích dotazech. Výchozí hodnota je `false`.
- `IsRetrievable` – Určuje, jestli pole můžete získat ve výsledcích hledání. Výchozí hodnota je `true`.
- `IsSearchable` – Označuje, zda je pole součástí fulltextové vyhledávání. Výchozí hodnota je `false`.
- `IsSortable` – Určuje, zda lze pole použít ve `OrderBy` výrazy. Výchozí hodnota je `false`.

> [!NOTE]
> Při změně indexu po nasazení nutné znovu sestavit a načíst data.

`Index` Volitelně můžete zadat objekt `Suggesters` vlastnosti, která definuje pole v indexu, který se má použít pro podporu automatického dokončování nebo vyhledávací dotazy návrhu. `Suggesters` Vlastnost musí být nastavená na pole `Suggester` objekty, které definují pole, která slouží k vytváření návrhu výsledky hledání.

Po vytvoření `Index` objektu, vytvoření indexu voláním `Indexes.Create` na `SearchServiceClient` instance.

> [!NOTE]
> Při vytváření indexu z aplikace, které musí být udržovány reakce, použijte `Indexes.CreateAsync` metoda.

Další informace najdete v tématu [vytvoření indexu Azure Search pomocí .NET SDK](/azure/search/search-create-index-dotnet/).

## <a name="deleting-the-azure-search-index"></a>Odstranění indexu Azure Search

Index může odstranit volání `Indexes.Delete` na `SearchServiceClient` instance:

```csharp
searchClient.Indexes.Delete(Constants.Index);
```

## <a name="uploading-data-to-the-azure-search-index"></a>Nahrávání dat do indexu Azure Search

Po definování indexu, je možné uložit data do jednou ze dvou modelů:

- **Model pro vyžádání obsahu** – data je pravidelně konzumována z Azure Cosmos DB, Azure SQL Database, úložiště objektů Blob Azure nebo SQL Server hostované v virtuální počítač Azure.
- **Push model** – data je prostřednictvím kódu programu odeslat do indexu. Jedná se o model přijaté v tomto článku.

A `SearchIndexClient` instance musí být vytvořený importovat data do indexu. Můžete to provést pomocí volání `SearchServiceClient.Indexes.GetClient` metoda, jak je ukázáno v následujícím příkladu kódu:

```csharp
static void UploadDataToSearchIndex()
{
  var indexClient = searchClient.Indexes.GetClient(Constants.Index);

  var monkeyList = MonkeyData.Monkeys.Select(m => new
  {
    id = Guid.NewGuid().ToString(),
    name = m.Name,
    location = m.Location,
    details = m.Details,
    imageUrl = m.ImageUrl
  });

  var batch = IndexBatch.New(monkeyList.Select(IndexAction.Upload));
  try
  {
    indexClient.Documents.Index(batch);
  }
  catch (IndexBatchException ex)
  {
    // Sometimes when the Search service is under load, indexing will fail for some
    // documents in the batch. Compensating actions like delaying and retrying should be taken.
    // Here, the failed document keys are logged.
    Console.WriteLine("Failed to index some documents: {0}",
      string.Join(", ", ex.IndexingResults.Where(r => !r.Succeeded).Select(r => r.Key)));
  }
}
```

Data určená k importu na index je zabalena jako `IndexBatch` objekt, který zapouzdřuje kolekci `IndexAction` objekty. Každý `IndexAction` instance obsahuje dokument a vlastnost, která říká službě Azure Search, jakou akci chcete provést v dokumentu. V příkladu výše `IndexAction.Upload` akce je zadán, výsledkem bude vložen do indexu, pokud je nový, dokumentu nebo nahrazený, pokud již existuje. `IndexBatch` Objekt se pak posílají do indexu zavoláním `Documents.Index` metodu `SearchIndexClient` objektu. Informace o dalších indexování akcí najdete v tématu [rozhodněte, jakou akci indexování použít](/azure/search/search-import-data-dotnet#ii-decide-which-indexing-action-to-use).

> [!NOTE]
> Jenom 1000 dokumentů mohou být součástí jedné žádosti indexování.

Všimněte si, že v příkladu výše, `monkeyList` kolekce je vytvořena jako anonymní objekt z kolekce `Monkey` objekty. Tím se vytvoří data `id` pole a přeloží mapování pascalcase `Monkey` názvy vlastností na camelCase vyhledávání názvy polí indexu. Alternativně toto mapování provést taky tak, že přidáte `[SerializePropertyNamesAsCamelCase]` atribut `Monkey` třídy.

Další informace najdete v tématu [nahrát data do služby Azure Search pomocí .NET SDK](/azure/search/search-import-data-dotnet/).

## <a name="querying-the-azure-search-index"></a>Dotazování indexu Azure Search

A `SearchIndexClient` instance musí být vytvořený dotazování indexu. Pokud je aplikace spuštěna dotazy, se doporučuje použijte Princip nejnižších nutných oprávnění a vytvořit `SearchIndexClient` přímo, předávání *klíč dotazu* jako argument. To zajistí, že uživatelé mají přístup jen pro čtení k indexům a dokumentům. Tento přístup je znázorněn v následujícím příkladu kódu:

```csharp
SearchIndexClient indexClient =
  new SearchIndexClient(Constants.SearchServiceName, Constants.Index, new SearchCredentials(Constants.QueryApiKey));
```

`SearchIndexClient` Přetížení konstruktoru přebírá název vyhledávací služby, název indexu a `SearchCredentials` objektu jako argumenty, s `SearchCredentials` objekt zabalení *klíč dotazu* pro službu Azure Search.

### <a name="search-queries"></a>Vyhledávací dotazy

Index může být dotazován voláním `Documents.SearchAsync` metodu `SearchIndexClient` instance, jak je ukázáno v následujícím příkladu kódu:

```csharp
async Task AzureSearch(string text)
{
  Monkeys.Clear();

  var searchResults = await indexClient.Documents.SearchAsync<Monkey>(text);
  foreach (SearchResult<Monkey> result in searchResults.Results)
  {
    Monkeys.Add(new Monkey
    {
      Name = result.Document.Name,
      Location = result.Document.Location,
      Details = result.Document.Details,
      ImageUrl = result.Document.ImageUrl
    });
  }
}
```

`SearchAsync` Metoda přebírá argumentem textové vyhledávání a volitelné `SearchParameters` objekt, který můžete použít pro další upřesnění dotazu. Vyhledávací dotaz je zadaný jako argument textu vyhledávání, při při nastavení lze zadat dotaz filter `Filter` vlastnost `SearchParameters` argument. Následující příklad kódu ukazuje, že oba typy dotazů:

```csharp
var parameters = new SearchParameters
{
  Filter = "location ne 'China' and location ne 'Vietnam'"
};
var searchResults = await indexClient.Documents.SearchAsync<Monkey>(text, parameters);
```

Tento dotaz filtru se použije na celý index a odebere dokumenty z výsledků kde `location` pole není rovno Číně a není rovno Vietnam. Po filtrování se provádí vyhledávací dotaz na výsledky dotazu filter.

> [!NOTE]
> Chcete-li filtrovat bez vyhledávání, předat `*` jako argument textové vyhledávání.

`SearchAsync` Metoda vrátí `DocumentSearchResult` objekt, který obsahuje výsledky dotazu. Tento objekt je ve výčtu uvedena, s jednotlivými `Document` objektu vytváří jako `Monkey` objektu a přidat do `Monkeys` `ObservableCollection` pro zobrazení. Tyto snímky obrazovky zobrazit vyhledávání výsledky dotazu vrácená z Azure Search:

![](azure-search-images/search.png "Výsledky hledání")

Další informace o vyhledávání a filtrování najdete v tématu [dotazování indexu Azure Search pomocí .NET SDK](/azure/search/search-query-dotnet/).

<a name="suggestions" />

### <a name="suggestion-queries"></a>Návrh dotazy

Služba Azure Search umožňuje návrhy vyžadované podle vyhledávací dotaz voláním `Documents.SuggestAsync` metodu `SearchIndexClient` instance. Tento postup je znázorněn v následujícím příkladu kódu:

```csharp
async Task AzureSuggestions(string text)
{
  Suggestions.Clear();

  var parameters = new SuggestParameters()
  {
    UseFuzzyMatching = true,
    HighlightPreTag = "[",
    HighlightPostTag = "]",
    MinimumCoverage = 100,
    Top = 10
  };

  var suggestionResults =
    await indexClient.Documents.SuggestAsync<Monkey>(text, "nameSuggester", parameters);

  foreach (var result in suggestionResults.Results)
  {
    Suggestions.Add(new Monkey
    {
      Name = result.Text,
      Location = result.Document.Location,
      Details = result.Document.Details,
      ImageUrl = result.Document.ImageUrl
    });
  }
}
```

`SuggestAsync` Metoda přebírá argument textu vyhledávání, název modulu pro návrhy použít (která je definována v indexu), a volitelné `SuggestParameters` objekt, který můžete použít pro další upřesnění dotazu. `SuggestParameters` Instance nastaví následující vlastnosti:

- `UseFuzzyMatching` – Pokud nastavíte hodnotu `true`, Azure Search najdete návrhy i tehdy, je nahrazovanou nebo chybí znak v hledaný text.
- `HighlightPreTag` – tag, který se přidá jako předpona k návrhu přístupů.
- `HighlightPostTag` – značka, který se připojí k návrhu přístupů.
- `MinimumCoverage` – představuje procento index, který musí být pokryté komponentami návrhu dotazu pro dotaz na hlášené úspěšné. Výchozí hodnota je 80.
- `Top` – počet návrhy pro načtení. Musí být celé číslo mezi 1 a 100, výchozí hodnota je 5.

Celkový efekt je, že top 10 výsledky z indexu budou vráceny s zvýrazňování a výsledky bude obsahovat dokumenty, které obsahují napsaná podobně hledaný text.

`SuggestAsync` Metoda vrátí `DocumentSuggestResult` objekt, který obsahuje výsledky dotazu. Tento objekt je ve výčtu uvedena, s jednotlivými `Document` objektu vytváří jako `Monkey` objektu a přidat do `Monkeys` `ObservableCollection` pro zobrazení. Na následujících snímcích obrazovky zobrazit návrh výsledky vrácené z Azure Search:

![](azure-search-images/suggest.png "Výsledky návrhu")

Všimněte si, že v ukázkové aplikaci `SuggestAsync` metoda je volána, pouze když uživatel dokončí vložení hledaný termín. Ale ho lze také pro podporu automatického dokončování vyhledávací dotazy spuštěním na každý keypress.

## <a name="summary"></a>Souhrn

Tento článek ukázal, jak integrovat Azure Search na aplikaci Xamarin.Forms pomocí knihovnu Microsoft Azure Search. Vyhledávání systému Azure je Cloudová služba, která poskytuje indexování a dotazování možnosti pro odeslaná data. Touto akcí odeberete, požadavky na infrastrukturu a vyhledávací algoritmus složité kroky tradičně spojené s implementací funkce vyhledávání v aplikaci.


## <a name="related-links"></a>Související odkazy

- [Služba Azure Search (ukázka)](https://developer.xamarin.com/samples/xamarin-forms/WebServices/AzureSearch/)
- [Dokumentace Azure Search](/azure/search/)
- [Prohledat knihovnu Microsoft Azure](https://www.nuget.org/packages/Microsoft.Azure.Search/)
