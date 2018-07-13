---
title: Přístup ke vzdáleným datům
description: Tato kapitola popisuje, jak aplikaci eShopOnContainers mobilní aplikace přistupuje k datům z kontejnerizované mikroslužby.
ms.prod: xamarin
ms.assetid: 42eba6f5-9784-4e1a-9943-5c1fbeea7452
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 08/07/2017
ms.openlocfilehash: 009a4025bc9df6f657964b7e97e559635ef0a929
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/12/2018
ms.locfileid: "38996162"
---
# <a name="accessing-remote-data"></a>Přístup ke vzdáleným datům

Ujistěte se, mnohá moderní řešení založeného na webu využívání webových služeb hostovaných webových serverů k zajištění funkce pro vzdálené klientské aplikace. Operace, které zveřejňuje webová služba tvoří webového rozhraní API.

Klientské aplikace by měla moct využívat webového rozhraní API bez znalosti, jak se implementují dat nebo operace, které zveřejňuje rozhraní API. To vyžaduje, aby rozhraní API dodržuje běžných standardů, které umožňují klientské aplikaci a webovou službou shodnout na které formáty data a strukturu dat, která se vyměňují mezi klientské aplikace a webové služby.

## <a name="introduction-to-representational-state-transfer"></a>Úvod do služby Representational State Transfer

Representational State Transfer (REST) je styl architektury pro vytváření distribuovaných systémů založených na hypermédiích. Primární výhoda REST modelu je, že je založená na otevřených standardech a bez vazby implementaci modelu nebo klientských aplikací, kteří k nim žádnou konkrétní implementaci. Proto webovou službu REST může implementovat s využitím Microsoft ASP.NET Core MVC a klientské aplikace může být vývoj pomocí jakéhokoli jazyka a nástrojů, která mohou generovat požadavky HTTP a parsovat odpovědi HTTP.

REST model navigační schéma používá k reprezentaci objekty a služby přes síť, označuje jako prostředky. Systémy, které obvykle implementují REST pomocí protokolu HTTP k přenosu žádosti o přístup k těmto prostředkům. V takových systémech klientská aplikace odešle žádost ve formě identifikátoru URI, který identifikuje prostředek a metodou HTTP (například GET, POST, PUT nebo DELETE), která určuje operaci provést na tento prostředek. Text požadavku HTTP obsahuje všechna data potřebná k provedení operace.

> [!NOTE]
> REST definuje bezstavový model požadavků. Proto požadavky HTTP musí být nezávislé a může dojít v libovolném pořadí.

Odpověď zbývající požadavku umožňuje používat standardní stavové kódy HTTP. Například požadavek, který vrátí platná data by měl obsahovat kód odpovědi HTTP 200 (OK), zatímco požadavek, který nejde najít nebo odstranění zadaného prostředku by mělo vrátit odpověď, která zahrnuje stavový kód HTTP 404 (Nenalezeno).

RESTful webového rozhraní API zveřejňuje sadu připojených prostředků a poskytuje základních operací, které umožňují aplikaci manipulovat s těmito prostředky a snadno procházet mezi nimi. Z tohoto důvodu jsou orientované na data, která zveřejňuje a použijte k dispozici zařízení pomocí protokolu HTTP pro provoz na těchto datech identifikátory URI, který tvoří typické RESTful webového rozhraní API.

Zobrazí data zahrnutá ve klientskou aplikaci v požadavku HTTP a odpovídající zprávy odpovědi z webového serveru, může se vám v různých formátech, označované jako typy médií. Když klientská aplikace odešle požadavek, který vrací data v textu zprávy, můžete určit typy médií, dokáže zpracovat v `Accept` záhlaví požadavku. Pokud server podporuje tento typ média, můžete odpovědět s odpovědí, který obsahuje `Content-Type` hlavičku, která určuje formát dat v těle zprávy. Pak je na starost klientská aplikace k parsování zprávy s odpovědí a odpovídajícím způsobem interpretace výsledků v textu zprávy.

Další informace o REST, naleznete v tématu [návrh rozhraní API](/azure/architecture/best-practices/api-design/) a [implementace rozhraní API](/azure/architecture/best-practices/api-implementation/).

## <a name="consuming-restful-apis"></a>Použití rozhraní REST API

Aplikaci eShopOnContainers mobilní aplikace používá vzor Model-View-ViewModel (MVVM) a prvky modelu, který představuje model domény entity používané v aplikaci. Třídy kontroleru a úložiště v aplikaci eShopOnContainers referenční aplikace přijímají a vrací mnoho z těchto objektů modelu. Proto se používají jako objektů pro přenos dat (DTO), které obsahují všechna data, která je předána mezi mobilní aplikaci a kontejnerizovaných mikroslužeb. Hlavní výhodou používání DTO předejte data a přijímat data z webové služby je, že předáním další data v jednom vzdáleného volání aplikace můžete snížit počet vzdálené volání, které je potřeba provést.

### <a name="making-web-requests"></a>Vytváření webových požadavků

Mobilní aplikace používá aplikaci eShopOnContainers `HttpClient` třídy k podání žádostí o přes protokol HTTP s JSON se používají jako typ média. Tato třída poskytuje funkce pro asynchronní odesílání požadavků HTTP a příjem odpovědí HTTP z identifikátoru URI prostředků identifikovat. `HttpResponseMessage` Třída reprezentuje zprávy odpovědi HTTP přijaté z rozhraní REST API po požadavku HTTP. Obsahuje informace o odpověď, a to včetně stavový kód, záhlaví a libovolný text. `HttpContent` Třída reprezentuje těla protokolu HTTP a hlavičky obsahu, jako například `Content-Type` a `Content-Encoding`. Obsah lze číst pomocí kteréhokoli z `ReadAs` metody, například `ReadAsStringAsync` a `ReadAsByteArrayAsync`, v závislosti na formátu data.

<a name="making_a_get_request" />

#### <a name="making-a-get-request"></a>Provádění požadavku GET

`CatalogService` Třída se používá ke správě procesu načítání dat z katalogu mikroslužeb. V `RegisterDependencies` metodu `ViewModelLocator` třídy, `CatalogService` třída je registrována jako typ mapování proti `ICatalogService` typ kontejneru pro vkládání závislosti Autofac. Potom, pokud instance `CatalogViewModel` třída se vytvoří, jeho konstruktor přijímá `ICatalogService` typ, který řeší Autofac vrací instanci `CatalogService` třídy. Další informace o vkládání závislostí, naleznete v tématu [Úvod ke vkládání závislostí](~/xamarin-forms/enterprise-application-patterns/dependency-injection.md#introduction_to_dependency_injection).

Obrázek 10-1 ukazuje interakci tříd, které načítají data katalogu z katalogu mikroslužeb pro zobrazení podle `CatalogView`.

[![](accessing-remote-data-images/catalogdata.png "Načítání dat z mikroslužeb katalogu")](accessing-remote-data-images/catalogdata-large.png#lightbox "načítání dat z katalogu mikroslužeb")

**Obrázek 10-1**: načítání dat z katalogu mikroslužeb

Když `CatalogView` se přejde poté, `OnInitialize` metoda ve `CatalogViewModel` třída se nazývá. Tato metoda načte data katalogu z katalogu mikroslužeb, jak je ukázáno v následujícím příkladu kódu:

```csharp
public override async Task InitializeAsync(object navigationData)  
{  
    ...  
    Products = await _productsService.GetCatalogAsync();  
    ...  
}
```

Tato metoda volá `GetCatalogAsync` metodu `CatalogService` instanci, která se vloží do `CatalogViewModel` podle Autofac. Následující příklad kódu ukazuje `GetCatalogAsync` metody:

```csharp
public async Task<ObservableCollection<CatalogItem>> GetCatalogAsync()  
{  
    UriBuilder builder = new UriBuilder(GlobalSetting.Instance.CatalogEndpoint);  
    builder.Path = "api/v1/catalog/items";  
    string uri = builder.ToString();  

    CatalogRoot catalog = await _requestProvider.GetAsync<CatalogRoot>(uri);  
    ...  
    return catalog?.Data.ToObservableCollection();            
}
```

Tato metoda vytvoří identifikátor URI, který identifikuje prostředek žádost bude odeslána na a používá `RequestProvider` třídy volání metody GET protokolu HTTP na prostředku, před vrácením z výsledků `CatalogViewModel`. `RequestProvider` Třída obsahuje funkce, které odesílá požadavek v podobě identifikátoru URI, který identifikuje prostředek, lze metodu HTTP, která určuje operaci provést na tento prostředek a textu, která obsahuje veškerá data potřebná k provedení operace. Informace o tom, jak `RequestProvider` třídy je vloženou `CatalogService class`, naleznete v tématu [Úvod ke vkládání závislostí](~/xamarin-forms/enterprise-application-patterns/dependency-injection.md#introduction_to_dependency_injection).

Následující příklad kódu ukazuje `GetAsync` metodu `RequestProvider` třídy:

```csharp
public async Task<TResult> GetAsync<TResult>(string uri, string token = "")  
{  
    HttpClient httpClient = CreateHttpClient(token);  
    HttpResponseMessage response = await httpClient.GetAsync(uri);  

    await HandleResponse(response);  
    string serialized = await response.Content.ReadAsStringAsync();  

    TResult result = await Task.Run(() =>   
        JsonConvert.DeserializeObject<TResult>(serialized, _serializerSettings));  

    return result;  
}
```

Tato metoda volá `CreateHttpClient` metodu, která vrací instanci `HttpClient` třídy sadou příslušné záhlaví. Poté odešle Asynchronní požadavek GET na prostředku označeném identifikátorem URI, se ukládají v odpovědi `HttpResponseMessage` instance. `HandleResponse` Potom je volána metoda, která vyvolá výjimku, pokud odpověď neobsahuje úspěch stavového kódu protokolu HTTP. Pak je pro čtení odpovědi jako řetězec převést z JSON na `CatalogRoot` objekt a vrátí `CatalogService`.

`CreateHttpClient` Metoda je znázorněno v následujícím příkladu kódu:

```csharp
private HttpClient CreateHttpClient(string token = "")  
{  
    var httpClient = new HttpClient();  
    httpClient.DefaultRequestHeaders.Accept.Add(  
        new MediaTypeWithQualityHeaderValue("application/json"));  

    if (!string.IsNullOrEmpty(token))  
    {  
        httpClient.DefaultRequestHeaders.Authorization =   
            new AuthenticationHeaderValue("Bearer", token);  
    }  
    return httpClient;  
}
```

Tato metoda vytvoří novou instanci třídy `HttpClient` třídy a nastaví `Accept` hlavičky požadavků provedených `HttpClient` instance na `application/json`, což znamená, že se očekává, že obsah odpovědi má být formátováno pomocí formátu JSON. Pak v případě, že přístupový token byl předán jako argument `CreateHttpClient` metoda, přidá se do `Authorization` hlavičky požadavků provedených `HttpClient` instance řetězec s předponou `Bearer`. Další informace o ověřování najdete v tématu [autorizace](~/xamarin-forms/enterprise-application-patterns/authentication-and-authorization.md#authorization).

Při `GetAsync` metoda ve `RequestProvider` třídy volání `HttpClient.GetAsync`, `Items` metoda ve `CatalogController` třídu v projektu Catalog.API je vyvolána, která je uvedena v následujícím příkladu kódu:

```csharp
[HttpGet]  
[Route("[action]")]  
public async Task<IActionResult> Items(  
    [FromQuery]int pageSize = 10, [FromQuery]int pageIndex = 0)  
{  
    var totalItems = await _catalogContext.CatalogItems  
        .LongCountAsync();  

    var itemsOnPage = await _catalogContext.CatalogItems  
        .OrderBy(c=>c.Name)  
        .Skip(pageSize * pageIndex)  
        .Take(pageSize)  
        .ToListAsync();  

    itemsOnPage = ComposePicUri(itemsOnPage);  
    var model = new PaginatedItemsViewModel<CatalogItem>(  
        pageIndex, pageSize, totalItems, itemsOnPage);             

    return Ok(model);  
}
```

Tato metoda načte katalog dat v databázi SQL s využitím objektu EntityFramework a vrátí jako zpráva odpovědi, která zahrnuje úspěch stavový kód HTTP a soubor JSON ve formátu `CatalogItem` instancí.

#### <a name="making-a-post-request"></a>Provedení požadavku POST

`BasketService` Třída se používá ke správě načítání dat a aktualizovat košík mikroslužeb procesu. V `RegisterDependencies` metodu `ViewModelLocator` třídy, `BasketService` třída je registrována jako typ mapování proti `IBasketService` typ kontejneru pro vkládání závislosti Autofac. Potom, pokud instance `BasketViewModel` třída se vytvoří, jeho konstruktor přijímá `IBasketService` typ, který řeší Autofac vrací instanci `BasketService `třídy. Další informace o vkládání závislostí, naleznete v tématu [Úvod ke vkládání závislostí](~/xamarin-forms/enterprise-application-patterns/dependency-injection.md#introduction_to_dependency_injection).

Obrázek 10-2 ukazuje interakce třídy, které posílají data nákupního košíku, zobrazí `BasketView`, do nákupního košíku mikroslužeb.

[![](accessing-remote-data-images/basketdata.png "Odesílání dat do nákupního košíku mikroslužeb")](accessing-remote-data-images/basketdata-large.png#lightbox "odesílání dat do nákupního košíku mikroslužeb")

**Obrázek 10-2**: odesílání dat do nákupního košíku mikroslužeb

Při přidání položky do nákupního košíku `ReCalculateTotalAsync` metodu `BasketViewModel` třída se nazývá. Tato metoda aktualizuje celkovou hodnotu položek v nákupním košíku a odesílá data nákupní košík do nákupního košíku mikroslužeb, jak je ukázáno v následujícím příkladu kódu:

```csharp
private async Task ReCalculateTotalAsync()  
{  
    ...  
    await _basketService.UpdateBasketAsync(new CustomerBasket  
    {  
        BuyerId = userInfo.UserId,   
        Items = BasketItems.ToList()  
    }, authToken);  
}
```

Tato metoda volá `UpdateBasketAsync` metodu `BasketService` instanci, která se vloží do `BasketViewModel` podle Autofac. Následující metoda ukazuje `UpdateBasketAsync` metody:

```csharp
public async Task<CustomerBasket> UpdateBasketAsync(CustomerBasket customerBasket, string token)  
{  
    UriBuilder builder = new UriBuilder(GlobalSetting.Instance.BasketEndpoint);  
    string uri = builder.ToString();  
    var result = await _requestProvider.PostAsync(uri, customerBasket, token);  
    return result;  
}
```

Tato metoda vytvoří identifikátor URI, který identifikuje prostředek žádost bude odeslána na a používá `RequestProvider` třídy volání metody POST protokolu HTTP na prostředku, před vrácením z výsledků `BasketViewModel`. Všimněte si, že přístupový token, získané z IdentityServer během procesu ověřování se vyžaduje k autorizaci požadavků na mikroslužbách nákupní košík. Další informace o ověřování najdete v tématu [autorizace](~/xamarin-forms/enterprise-application-patterns/authentication-and-authorization.md#authorization).

Následující příklad kódu ukazuje jednu z `PostAsync` metody v `RequestProvider` třídy:

```csharp
public async Task<TResult> PostAsync<TResult>(  
    string uri, TResult data, string token = "", string header = "")  
{  
    HttpClient httpClient = CreateHttpClient(token);  
    ...  
    var content = new StringContent(JsonConvert.SerializeObject(data));  
    content.Headers.ContentType = new MediaTypeHeaderValue("application/json");  
    HttpResponseMessage response = await httpClient.PostAsync(uri, content);  

    await HandleResponse(response);  
    string serialized = await response.Content.ReadAsStringAsync();  

    TResult result = await Task.Run(() =>  
        JsonConvert.DeserializeObject<TResult>(serialized, _serializerSettings));  

    return result;  
}
```

Tato metoda volá `CreateHttpClient` metodu, která vrací instanci `HttpClient` třídy sadou příslušné záhlaví. Poté odešle požadavek POST asynchronní prostředku označeném identifikátorem URI, s daty serializovaná nákupní košík odesílány ve formátu JSON a ukládají v odpovědi `HttpResponseMessage` instance. `HandleResponse` Potom je volána metoda, která vyvolá výjimku, pokud odpověď neobsahuje úspěch stavového kódu protokolu HTTP. Potom, načtou odpovědi jako řetězec převést z JSON na `CustomerBasket` objekt a vrátí `BasketService`. Další informace o `CreateHttpClient` metodu, najdete v článku [provádění požadavek na získání](#making_a_get_request).

Při `PostAsync` metoda ve `RequestProvider` třídy volání `HttpClient.PostAsync`, `Post` metoda ve `BasketController` třídu v projektu Basket.API je vyvolána, která je uvedena v následujícím příkladu kódu:

```csharp
[HttpPost]  
public async Task<IActionResult> Post([FromBody]CustomerBasket value)  
{  
    var basket = await _repository.UpdateBasketAsync(value);  
    return Ok(basket);  
}
```

Tato metoda používá instanci `RedisBasketRepository` třídy k uchování dat. nákupní košík k mezipaměti Redis a vrátí jej jako zprávu odpovědi, která zahrnuje úspěch stavový kód HTTP a JSON ve formátu `CustomerBasket` instance.

#### <a name="making-a-delete-request"></a>U žádosti o odstranění

Obrázek 10-3 ukazuje interakce třídy, které odstranit nákupní košík data z mikroslužeb nákupní košík pro `CheckoutView`.

![](accessing-remote-data-images/checkoutdata.png "Při odstranění dat z nákupního košíku mikroslužeb")

**Obrázek 10-3**: odstranění dat z nákupního košíku mikroslužeb

Při vyvolání proces platby u pokladny `CheckoutAsync` metodu `CheckoutViewModel` třída se nazývá. Tato metoda vytvoří nová objednávka, před vymazáním nákupního košíku, jak je ukázáno v následujícím příkladu kódu:

```csharp
private async Task CheckoutAsync()  
{  
    ...  
    await _basketService.ClearBasketAsync(_shippingAddress.Id.ToString(), authToken);  
    ...  
}
```

Tato metoda volá `ClearBasketAsync` metodu `BasketService` instanci, která se vloží do `CheckoutViewModel` podle Autofac. Následující metoda ukazuje `ClearBasketAsync` metody:

```csharp
public async Task ClearBasketAsync(string guidUser, string token)  
{  
    UriBuilder builder = new UriBuilder(GlobalSetting.Instance.BasketEndpoint);  
    builder.Path = guidUser;  
    string uri = builder.ToString();  
    await _requestProvider.DeleteAsync(uri, token);  
}
```

Tato metoda sestavení, který identifikuje prostředek, který pošle požadavek na identifikátor URI a používá `RequestProvider` třídy vyvolat metodu DELETE HTTP pro prostředek. Všimněte si, že přístupový token, získané z IdentityServer během procesu ověřování se vyžaduje k autorizaci požadavků na mikroslužbách nákupní košík. Další informace o ověřování najdete v tématu [autorizace](~/xamarin-forms/enterprise-application-patterns/authentication-and-authorization.md#authorization).

Následující příklad kódu ukazuje `DeleteAsync` metodu `RequestProvider` třídy:

```csharp
public async Task DeleteAsync(string uri, string token = "")  
{  
    HttpClient httpClient = CreateHttpClient(token);  
    await httpClient.DeleteAsync(uri);  
}
```

Tato metoda volá `CreateHttpClient` metodu, která vrací instanci `HttpClient` třídy sadou příslušné záhlaví. Poté odešle asynchronního požadavku odstranění prostředku označeném identifikátorem URI. Další informace o `CreateHttpClient` metodu, najdete v článku [provádění požadavek na získání](#making_a_get_request).

Při `DeleteAsync` metoda ve `RequestProvider` třídy volání `HttpClient.DeleteAsync`, `Delete` metoda ve `BasketController` třídu v projektu Basket.API je vyvolána, která je uvedena v následujícím příkladu kódu:

```csharp
[HttpDelete("{id}")]  
public void Delete(string id)  
{  
    _repository.DeleteBasketAsync(id);  
}
```

Tato metoda používá instanci `RedisBasketRepository` třídy odstranit nákupní košík data z mezipaměti Redis.

## <a name="caching-data"></a>Ukládaní dat do mezipaměti

Výkon aplikace lze zvýšit pomocí ukládání do mezipaměti často používaná data do rychlého úložiště umístěného v blízkosti aplikace. Pokud je umístěno blíž k aplikaci než původní zdroj rychlé úložiště, pak ukládání do mezipaměti může výrazně zlepšit odpovědi v případech, kdy se načítají se data.

Nejběžnější způsob ukládání do mezipaměti je přímého čtení ukládání do mezipaměti, kde aplikace načte data odkazováním na mezipaměť. Pokud data v mezipaměti nejsou, má načíst z úložiště dat a přidají se do mezipaměti. Aplikace může implementovat přímého čtení do mezipaměti pomocí modelu doplňování mezipaměti. Tento model Určuje, zda položka je aktuálně v mezipaměti. Pokud položka není v mezipaměti, má číst z úložiště dat a přidají se do mezipaměti. Další informace najdete v tématu [s doplňováním mezipaměti](/azure/architecture/patterns/cache-aside/) vzor.

> [!TIP]
> Data v mezipaměti, který je často čtou a se mění zřídka. Tato data lze přidat do mezipaměti na vyžádání při prvním načtení aplikací. To znamená, že aplikace potřebuje načíst data jen jednou z úložiště dat a všechny další přístupy může zajišťovat mezipaměť.

Distribuované aplikace, jako například aplikaci eShopOnContainers odkazovat na aplikaci, by měla poskytnout jednu nebo obě z následujících mezipaměti:

-   Sdílené mezipaměti, který se dá dostat několik procesů nebo počítačů.
-   Soukromé mezipaměti, kde jsou data uložena místně na zařízení spuštění aplikace.

Mobilní aplikace aplikaci eShopOnContainers používá soukromé mezipaměti, kde jsou data uložena místně na zařízení, na kterém běží instance aplikace. Informace o mezipaměti používané aplikací odkaz na aplikaci eShopOnContainers najdete v tématu [Mikroslužby .NET: architektura pro Kontejnerizovaných aplikací .NET](https://aka.ms/microservicesebook).

> [!TIP]
> Mezipaměti můžete představit jako s přechodným úložištěm dat, která může kdykoli zmizet. Ujistěte se, že data se udržují v původní úložiště dat, stejně jako mezipaměť. Minimalizuje riziko ztráty dat se pak pokud mezipaměť přestane být k dispozici.

### <a name="managing-data-expiration"></a>Správa konce platnosti dat

Je vhodné očekávat, že data uložená v mezipaměti bude vždy konzistentní s původními daty. Data v původní úložiště dat může změnit poté, co je tam byla uložena, způsobí data uložená v mezipaměti stala zastaralými. Proto aplikace by měl implementovanou strategii, která pomáhá zajistit, že se data v mezipaměti jako co nejaktuálnější, ale můžete také zjistit a řešit situace, které vznikají při data v mezipaměti je zastaralá. Nejvíce ukládání do mezipaměti mechanismy povolit mezipaměť nakonfigurovat tak, aby platnost dat a proto zkrátit dobu, pro který může být data zastaralá.

> [!TIP]
> Nastavit dobu platnosti výchozí čas při konfiguraci mezipaměti. Počet mezipamětí implementovat vypršení platnosti, která zneplatní data a odstraní z mezipaměti, pokud není přístupné za určité období. Však musí být věnovat pozornost při výběru doby platnosti. Pokud je provedeno příliš krátký, data vyprší příliš brzy a výhody ukládání do mezipaměti se sníží. Pokud je k příliš dlouhý, rizika data budou zastaralá. Čas vypršení platnosti proto by měl odpovídat vzoru přístupu pro aplikace, které využívají data.

Když vyprší platnost dat uložených v mezipaměti, musí být odebrány z mezipaměti a aplikace musí získat data z původní data ukládat a umístěte ji zpět do mezipaměti.

Je také možné, že mezipaměť může vyplnit nahoru, pokud se data můžou zůstat po příliš dlouhou dobu. Proto se žádosti o přidání nových položek do mezipaměti může být nutné odebrat i některé položky v rámci procesu nazývaného *vyřazení*. Ukládání do mezipaměti služby obvykle vyřazení dat na základě nejnižší naposledy použitých. Existují však další zásady vyřazení, včetně naposledy použité a first-in-first-out. Další informace najdete v tématu [ukládání do mezipaměti](/azure/architecture/best-practices/caching/).

<a name="caching_images" />

### <a name="caching-images"></a>Ukládání do mezipaměti Imagí

Mobilní aplikace aplikaci eShopOnContainers využívá obrázky vzdálené produktů, které využívají samosprávné ukládat se do mezipaměti. Tyto Image jsou zobrazeny [ `Image` ](xref:Xamarin.Forms.Image) ovládacího prvku a `CachedImage` poskytuje ovládací prvek [FFImageLoading](https://www.nuget.org/packages/Xamarin.FFImageLoading.Forms/) knihovny.

Xamarin.Forms [ `Image` ](xref:Xamarin.Forms.Image) ovládací prvek podporuje ukládání do mezipaměti stažených imagí. Ukládání do mezipaměti je ve výchozím nastavení povolené a bude uchovávat image místně po dobu 24 hodin. Kromě toho může mít nakonfigurovanou čas vypršení platnosti [ `CacheValidity` ](xref:Xamarin.Forms.UriImageSource.CacheValidity) vlastnost. Další informace najdete v tématu [stáhli Image do mezipaměti](~/xamarin-forms/user-interface/images.md#Image_Caching).

Společnosti FFImageLoading `CachedImage` ovládací prvek je náhradou pro Xamarin.Forms [ `Image` ](xref:Xamarin.Forms.Image) ovládacího prvku, poskytuje další vlastnosti, které umožňují doplňující funkce. Mezi tato funkce poskytuje ovládací prvek konfigurovat ukládání do mezipaměti, a chyby a pro načítání obrázků zástupné symboly. Následující příklad kódu ukazuje, jak se používá v aplikaci eShopOnContainers mobilní aplikaci `CachedImage` v ovládacím prvku `ProductTemplate`, který je šablona dat používané [ `ListView` ](xref:Xamarin.Forms.ListView) v ovládacím prvku `CatalogView`:

```xaml
<ffimageloading:CachedImage
    Grid.Row="0"
    Source="{Binding PictureUri}"     
    Aspect="AspectFill">
    <ffimageloading:CachedImage.LoadingPlaceholder>
        <OnPlatform x:TypeArguments="ImageSource">
            <On Platform="iOS, Android" Value="default_campaign" />
            <On Platform="UWP" Value="Assets/default_campaign.png" />
        </OnPlatform>
    </ffimageloading:CachedImage.LoadingPlaceholder>
    <ffimageloading:CachedImage.ErrorPlaceholder>
        <OnPlatform x:TypeArguments="ImageSource">
            <On Platform="iOS, Android" Value="noimage" />
            <On Platform="UWP" Value="Assets/noimage.png" />
        </OnPlatform>
    </ffimageloading:CachedImage.ErrorPlaceholder>
</ffimageloading:CachedImage>
```

`CachedImage` Řídit sad `LoadingPlaceholder` a `ErrorPlaceholder` vlastnosti k obrázkům specifické pro platformu. `LoadingPlaceholder` Vlastnost určuje obrázek, který se bude zobrazovat během bitovou kopii určeného `Source` vlastnosti je načtena a `ErrorPlaceholder` vlastnost určuje obrázek, který se zobrazí, pokud dojde k chybě při pokusu o načtení obrázku určená `Source` vlastnost.

Jak již název napovídá, `CachedImage` ovládací prvek vzdálené imagí v zařízení po dobu uvedenou hodnotou ukládá do mezipaměti `CacheDuration` vlastnost. Pokud tato hodnota vlastnosti není explicitně nastavena, bude použita výchozí hodnota 30 dnů.

## <a name="increasing-resilience"></a>Zvýšení odolnosti

Všechny aplikace, které komunikují se vzdálenými službami a prostředky musí být citlivé na přechodné chyby. K přechodným chybám patří momentální ztráta síťového připojení ke službám, dočasná nedostupnost služby nebo vypršení časových limitů, ke kterému dochází, když je služba přetížená. Tyto chyby se často samy opraví, a pokud se akce opakuje po vhodně dlouhém bude nejspíš úspěšná.

Přechodné chyby může mít velký dopad na kvalitu zjištěné aplikace, i když byla důkladně otestována za všech předvídatelných okolností. Aby se zajistilo, spolehlivě funguje aplikaci, která komunikuje se službou vzdálené služby, musí být schopen provést všechny tyto:

-   Zjistit chyby při jejich výskytu a určit, pokud jsou pravděpodobně přechodné chyby.
-   Pokud se zjistí, že chyba je pravděpodobně přechodná a mějte přehled o počet pokusů, které operace, zkuste operaci zopakovat.
-   Použití strategie odpovídající opakování, která určuje počet opakovaných pokusů, zpoždění mezi jednotlivými pokusy a akce, které lze provést po neúspěšném pokusu.

Toto zpracování přechodných chyb můžete dosáhnout obalením všechny pokusy o přístup ke vzdálené službě v kódu, který implementuje modelu opakování.

### <a name="retry-pattern"></a>Model opakování

Pokud aplikace zjistí chybu při pokusu poslat žádost vzdálené služby, dokáže zpracovat selhání v některém z následujících způsobů:

-   Opakováním operace. Aplikace může zkusit neúspěšnou žádost okamžitě.
-   Opakování operace po prodlevě. Aplikace by měl čekat vhodnou dobu před opakováním žádosti.
-   Zrušení operace. Aplikace by měla operaci zrušit a ohlásit výjimku.

Strategie opakování by měly být vyladěné tak, aby odpovídaly obchodním požadavkům aplikace. Například je důležité optimalizovat počet opakování a interval operace probíhá pokus o opakování. Pokud je operace součástí interakce s uživatelem, interval opakování by měl být krátký s pouze několika opakovanými pokusy, aby uživatelé nečekali na odpověď. Pokud operace součástí dlouho běžící pracovní postup, kdy zrušení a restartování pracovního postupu je nákladné nebo časově náročné, je vhodné čekat mezi pokusy déle a opakovat je víckrát.

> [!NOTE]
> Strategie účinnou opakovat s minimální prodlevou mezi pokusy a velký počet opakování, může dojít ke snížení vzdálené služby, na kterém běží zavřít nebo plně využívá kapacitu. Kromě toho strategie opakování může také ovlivnit rychlost reakce aplikace v případě, že se neustále pokouší provést neúspěšnou operaci.

Pokud žádost selže i po uplynutí počtu opakování, je lepší pro aplikaci, aby se zabránilo další žádostí do stejného prostředku a ohlásí chybu. Pak po nastavené období aplikace můžete provést jeden nebo více požadavků prostředek, který chcete zobrazit, pokud jsou úspěšné. Další informace najdete v tématu [vzoru Circuit Breaker](#circuit_breaker_pattern).

> [!TIP]
> Nikdy implementujte mechanismus nekonečným počtem opakování. Použijte konečný počet opakování nebo implementujte [jistič](/azure/architecture/patterns/circuit-breaker/) vzor, který má povolit služba mohla zotavit.

V aplikaci eShopOnContainers mobilní aplikaci aktuálně neimplementuje modelu opakování při vytváření rozhraní RESTful webových požadavků. Ale `CachedImage` ovládacího prvku, poskytuje [FFImageLoading](https://www.nuget.org/packages/Xamarin.FFImageLoading.Forms/) knihovna podporuje zpracování přechodných chyb opakováním načítání obrázku. Pokud načítání obrázků selže, bude proveden další pokusy. Je určený počet pokusů o `RetryCount` vlastnost a opakování pokusu dojde po prodlevě určené `RetryDelay` vlastnost. Pokud tyto hodnoty vlastností nejsou nastavena explicitně, jejich výchozí hodnoty se použijí – 3 pro `RetryCount` vlastnost a 250ms pro `RetryDelay` vlastnost. Další informace o `CachedImage` řídí, najdete v článku [ukládání do mezipaměti Imagí](#caching_images).

Odkaz na aplikaci aplikaci eShopOnContainers implementaci modelu opakování. Další informace, včetně diskusi o tom, jak kombinací modelu opakování s `HttpClient` najdete v tématu [Mikroslužby .NET: architektura pro Kontejnerizovaných aplikací .NET](https://aka.ms/microservicesebook).

Další informace o modelu opakování, najdete v článku [opakujte](/azure/architecture/patterns/retry/) vzor.

<a name="circuit_breaker_pattern" />

### <a name="circuit-breaker-pattern"></a>Model jistič

V některých případech chyby můžou nastat z důvodu očekávané události, které trvají déle, chcete-li vyřešit. Tyto chyby můžou být v rozsahu od částečného výpadku připojení až po úplné selhání služby. V takových situacích je zbytečný pro aplikaci opakovat operaci, která pravděpodobně nebude úspěšná a místo toho by měl přijmout, operace se nezdařila a tuto chybu příslušně zpracovat.

Model jistič může zabránit aplikace opakovaně pokoušela provést operaci, která pravděpodobně selže, a zároveň umožnit aplikace ke zjištění, zda byl vyřešen v době.

> [!NOTE]
> Účel modelu jistič se liší od modelu opakování. Model opakování umožňuje aplikaci opakovat operaci s předpokladem, že bude úspěšná. Model jistič zabraňuje aplikaci provést operaci, která pravděpodobně selže.

Jistič funguje jako proxy pro operace, které může selhat. Proxy by měl sledovat počet chyb, ke kterým došlo a pomocí těchto informací můžete rozhodnout, jestli se má operace pokračovat nebo vrátit výjimka.

V aplikaci eShopOnContainers mobilní aplikaci aktuálně neimplementuje vzorek jističe. Aplikaci eShopOnContainers však nepodporuje. Další informace najdete v tématu [Mikroslužby .NET: architektura pro Kontejnerizovaných aplikací .NET](https://aka.ms/microservicesebook).

> [!TIP]
> Kombinovat modely opakování a jistič. Aplikaci můžete kombinovat opakování a jistič vzory pomocí modelu opakování vyvolat operaci prostřednictvím jističe. Logika opakovaných pokusů by však brát ohled na výjimky vrácené jističem a provádět opakované pokusy, pokud jistič signalizuje, že chyba není přechodná.

Další informace o modelu jistič, najdete v článku [jistič](/azure/architecture/patterns/circuit-breaker/) vzor.

## <a name="summary"></a>Souhrn

Ujistěte se, mnohá moderní řešení založeného na webu využívání webových služeb hostovaných webových serverů k zajištění funkce pro vzdálené klientské aplikace. Operace, které zveřejňuje webová služba tvoří webového rozhraní API a klientské aplikace by měl být schopen využívají webové rozhraní API bez znalosti, jak se implementují dat nebo operace, které zveřejňuje rozhraní API.

Výkon aplikace lze zvýšit pomocí ukládání do mezipaměti často používaná data do rychlého úložiště umístěného v blízkosti aplikace. Aplikace může implementovat přímého čtení do mezipaměti pomocí modelu doplňování mezipaměti. Tento model Určuje, zda položka je aktuálně v mezipaměti. Pokud položka není v mezipaměti, má číst z úložiště dat a přidají se do mezipaměti.

Při komunikaci s webovým rozhraním API, aplikace musí být citlivé na přechodné chyby. K přechodným chybám patří momentální ztráta síťového připojení ke službám, dočasná nedostupnost služby nebo vypršení časových limitů, ke kterému dochází, když je služba přetížená. Tyto chyby se často samy opraví, a pokud se akce opakuje po vhodně dlouhém, je pravděpodobné, že uspěje. Proto aplikace by měla všechny pokusy o přístup k webovému rozhraní API v kódu, který implementuje mechanismus pro zpracování přechodných chyb.


## <a name="related-links"></a>Související odkazy

- [Stáhněte si elektronickou knihu (2Mb PDF)](https://aka.ms/xamarinpatternsebook)
- [aplikaci eShopOnContainers (GitHub) (ukázka)](https://github.com/dotnet-architecture/eShopOnContainers)
