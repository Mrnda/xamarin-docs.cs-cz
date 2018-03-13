---
title: "Přístup k vzdálených dat"
ms.topic: article
ms.prod: xamarin
ms.assetid: 42eba6f5-9784-4e1a-9943-5c1fbeea7452
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 08/07/2017
ms.openlocfilehash: 0eec51a6c95894482a57bfe3bb1f95aec2045af4
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 03/09/2018
---
# <a name="accessing-remote-data"></a>Přístup k vzdálených dat

Ujistěte se, mnohá moderní řešení založené na webu pomocí webových služeb, hostované webové servery, nabízí funkce pro vzdálené klientské aplikace. Operace, které zpřístupní webové služby tvoří webového rozhraní API.

Klienta aplikace by měla umět využívají webové rozhraní API bez znalosti, jak jsou implementované dat nebo operace, které zpřístupňuje rozhraní API. To vyžaduje, aby rozhraní API dodržuje běžné standardů, které umožňují klienta aplikace a webové služby dohodnout se na které formáty dat mají být použity a struktura dat, která se vyměňují mezi klientské aplikace a webové služby.

## <a name="introduction-to-representational-state-transfer"></a>Úvod do Representational State Transfer

Přenos REST (Representational State) je styl architektury pro vytváření distribuované systémy založené na hypermédií. Primární výhod modelu REST je, že má založených na standardech otevřete a bez vazby implementace modelu nebo klientských aplikací, které k němu přístup pro všechny konkrétní implementaci. Proto webové služby REST, může být implementovaná pomocí Microsoft ASP.NET Core MVC a klientské aplikace může být vývoj s použitím všechny jazyk a sady nástrojů, která může generovat požadavků HTTP a analyzovat odpovědi HTTP.

REST model navigační schéma používá k reprezentaci objekty a služby přes síť, označuje jako prostředky. Systémy, které obvykle implementovat REST pomocí protokolu HTTP k předání žádosti o přístup k těmto prostředkům. V těchto systémech klientské aplikace odešle žádost o ve formě identifikátoru URI, který identifikuje prostředek a metodu HTTP (třeba GET, POST, PUT nebo odstranit), která určuje operaci provést na tento prostředek. Text žádosti HTTP obsahuje všechna data potřebná k provedení operace.

> [!NOTE]
> REST definuje model bezstavové požadavku. Proto požadavky HTTP musí být nezávislý a může dojít v libovolném pořadí.

Odpověď REST požadavku díky použití standardní stavové kódy HTTP. Požadavek, který vrátí platná data například by měla obsahovat kód odpovědi HTTP 200 (OK), zatímco žádost, kterému se nepodařilo najít nebo odstranit zadaný prostředek by měl vrátit odpověď zahrnující stavový kód protokolu HTTP 404 (není nalezena).

RESTful webová rozhraní API zveřejňuje sadu připojené prostředků a poskytuje základních operací, které umožňují aplikacím pracovat s tyto prostředky a snadno přejít mezi nimi. Z tohoto důvodu se identifikátory URI, které tvoří typické RESTful webového rozhraní API zaměřeno na data, která zpřístupňuje a použití zadané zařízení pomocí protokolu HTTP k provozu na tato data.

Údaje zahrnuté ve klientskou aplikaci v požadavku HTTP a odpovídající zprávy odpovědi z webového serveru, může být zpracovány v různých formátech, označuje jako typy médií. Pokud klientské aplikace odešle požadavek, který vrací data v textu zprávy, může však určovat typy médií dokáže zpracovat v `Accept` hlavičky žádosti. Pokud server podporuje tento typ média, může odpovědět odpovědi, která zahrnuje `Content-Type` hlavičky, která určují formát dat v textu zprávy. Pak je zodpovědností klientské aplikace a analyzovat zprávu odpovědi správně interpretovat výsledky v textu zprávy.

Další informace o REST najdete v tématu [rozhraní API návrhu](/azure/architecture/best-practices/api-design/) a [implementace rozhraní API](/azure/architecture/best-practices/api-implementation/).

## <a name="consuming-restful-apis"></a>Použití rozhraní RESTful API

Mobilní aplikace eShopOnContainers využívá vzor Model-View-ViewModel (modelem MVVM) a prvků modelu představují vzor entity domény použít v aplikaci. Třídy kontroleru a úložiště v referenční aplikace eShopOnContainers přijmout a vrátí mnoho z těchto objektů modelu. Proto se používají jako objekty přenos dat (DTOs), které mají všechna data, která se předávají mezi mobilní aplikace a kontejnerizované mikroslužeb. Hlavní výhodou použití DTOs předat data a přijímat data z webové služby je, že předáním více dat na základě jednoho vzdáleného volání aplikace ke snížení počtu vzdálené volání, které je potřeba provést.

### <a name="making-web-requests"></a>Provádění webových požadavků

Použití mobilní aplikace eShopOnContainers `HttpClient` třída provádět požadavky pomocí protokolu HTTP s JSON používá jako typ média. Tato třída poskytuje funkce pro asynchronní odesílání požadavků HTTP a příjem odpovědí HTTP z identifikátoru URI identifikuje prostředek. `HttpResponseMessage` Třída reprezentuje zprávu odpovědi HTTP po provedl se požadavek HTTP přijata rozhraní REST API. Obsahuje informace o odpověď, a to včetně stavový kód, hlavičky a všechny text. `HttpContent` Třída reprezentuje obsahu HTTP a hlavičky obsahu, jako například `Content-Type` a `Content-Encoding`. Obsah lze číst pomocí kteréhokoli `ReadAs` metody, například `ReadAsStringAsync` a `ReadAsByteArrayAsync`, v závislosti na formát data.

<a name="making_a_get_request" />

#### <a name="making-a-get-request"></a>Provedení požadavek GET.

`CatalogService` Třída se používá ke správě procesu načítání dat z katalogu mikroslužby. V `RegisterDependencies` metoda v `ViewModelLocator` třídy, `CatalogService` třída je registrována jako typ mapování proti `ICatalogService` typ s kontejneru pro vkládání závislosti Autofac. Pak, když instanci `CatalogViewModel` je vytvořit třídu, jeho konstruktor přijímá `ICatalogService` typ, který řeší Autofac, vrací instanci třídy `CatalogService` třídy. Další informace o vkládání závislostí v tématu [Úvod do vkládání závislostí](~/xamarin-forms/enterprise-application-patterns/dependency-injection.md#introduction_to_dependency_injection).

Obrázek 10-1 ukazuje interakce tříd, které čtou data katalogu z katalogu mikroslužbu pro zobrazení pomocí `CatalogView`.

[![](accessing-remote-data-images/catalogdata.png "Načítání dat z katalogu mikroslužbu")](accessing-remote-data-images/catalogdata-large.png#lightbox "načítání dat z mikroslužbu katalogu")

**Obrázek 10-1**: načítání dat z mikroslužbu katalogu

Když `CatalogView` přešli, `OnInitialize` metoda v `CatalogViewModel` třídy se nazývá. Tato metoda načítá data katalogu z katalogu mikroslužbu, jak je ukázáno v následujícím příkladu kódu:

```csharp
public override async Task InitializeAsync(object navigationData)  
{  
    ...  
    Products = await _productsService.GetCatalogAsync();  
    ...  
}
```

Tato metoda volá `GetCatalogAsync` metodu `CatalogService` instanci, která byla vloženy do `CatalogViewModel` podle Autofac. Následující příklad kódu ukazuje `GetCatalogAsync` metoda:

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

Tato metoda vytvoří identifikátor URI, který identifikuje prostředek, bude odeslána žádost a používá `RequestProvider` třída k vyvolání metody GET protokolu HTTP na prostředek, než vrátí výsledky do `CatalogViewModel`. `RequestProvider` Třída obsahuje funkce, které odešle žádost o ve formě identifikátoru URI, který identifikuje prostředek metodu HTTP, která určuje operaci provést na tento prostředek, a text, který obsahuje všechna data potřebná k provedení operace. Informace o tom, jak `RequestProvider` třída je vloženy do `CatalogService class`, najdete v části [Úvod do vkládání závislostí](~/xamarin-forms/enterprise-application-patterns/dependency-injection.md#introduction_to_dependency_injection).

Následující příklad kódu ukazuje `GetAsync` metoda v `RequestProvider` třídy:

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

Tato metoda volá `CreateHttpClient` metoda, která vrátí instanci `HttpClient` s nastavenou odpovídající hlavičky. Pak odešle požadavek GET asynchronní v prostředku označeném identifikátorem URI, se ukládají v odpovědi `HttpResponseMessage` instance. `HandleResponse` Potom je volána metoda, která vyvolá výjimku, pokud odpověď neobsahuje úspěšné stavový kód HTTP. Pak je určený pro čtení odpovědi jako řetězec převést z JSON na `CatalogRoot` objektu a vrátí `CatalogService`.

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

Tato metoda vytvoří novou instanci třídy `HttpClient` třídy a nastaví `Accept` záhlaví všechny žádosti provedené `HttpClient` instance k `application/json`, což naznačuje, že se očekává, že obsah všechny odpovědi na formátován pomocí JSON. Pak v případě, že přístupový token byl předán jako argument k `CreateHttpClient` metoda, se přidá do `Authorization` záhlaví všechny žádosti provedené `HttpClient` instance, řetězec s předponou `Bearer`. Další informace o autorizaci, najdete v části [autorizace](~/xamarin-forms/enterprise-application-patterns/authentication-and-authorization.md#authorization).

Když `GetAsync` metoda v `RequestProvider` třídy volání `HttpClient.GetAsync`, `Items` metoda v `CatalogController` – třída v projektu Catalog.API je vyvolána, která je uvedena v následujícím příkladu kódu:

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

Tato metoda načítá data katalogu z databáze SQL s využitím objektu EntityFramework a vrátí jako zprávu odpovědi, která zahrnuje úspěšné stavový kód HTTP a kolekce JSON formátu `CatalogItem` instance.

#### <a name="making-a-post-request"></a>Vytváření požadavku POST

`BasketService` Třída se používá ke správě načtení dat a aktualizovat proces s mikroslužbu nákupní košík. V `RegisterDependencies` metoda v `ViewModelLocator` třídy, `BasketService` třída je registrována jako typ mapování proti `IBasketService` typ s kontejneru pro vkládání závislosti Autofac. Pak, když instanci `BasketViewModel` je vytvořit třídu, jeho konstruktor přijímá `IBasketService` typ, který řeší Autofac, vrací instanci třídy `BasketService `třídy. Další informace o vkládání závislostí v tématu [Úvod do vkládání závislostí](~/xamarin-forms/enterprise-application-patterns/dependency-injection.md#introduction_to_dependency_injection).

Obrázek 10-2 ukazuje interakce tříd, které odesílají data košík zobrazuje `BasketView`, do nákupního košíku mikroslužby.

[![](accessing-remote-data-images/basketdata.png "Odesílání dat do košíku mikroslužbu")](accessing-remote-data-images/basketdata-large.png#lightbox "odesílání dat do mikroslužbu nákupní košík")

**Obrázek 10-2**: odesílání dat do mikroslužbu nákupní košík

Při přidání položky do nákupního košíku `ReCalculateTotalAsync` metoda v `BasketViewModel` třídy se nazývá. Tato metoda aktualizace celkovou hodnotu položky v košíku a odesílá data košík košík mikroslužbu, jak je ukázáno v následujícím příkladu kódu:

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

Tato metoda volá `UpdateBasketAsync` metodu `BasketService` instanci, která byla vloženy do `BasketViewModel` podle Autofac. Následující metoda ukazuje `UpdateBasketAsync` metoda:

```csharp
public async Task<CustomerBasket> UpdateBasketAsync(CustomerBasket customerBasket, string token)  
{  
    UriBuilder builder = new UriBuilder(GlobalSetting.Instance.BasketEndpoint);  
    string uri = builder.ToString();  
    var result = await _requestProvider.PostAsync(uri, customerBasket, token);  
    return result;  
}
```

Tato metoda vytvoří identifikátor URI, který identifikuje prostředek, bude odeslána žádost a používá `RequestProvider` třída k vyvolání metody POST HTTP na prostředek, než vrátí výsledky do `BasketViewModel`. Všimněte si, že přístupový token, získaný IdentityServer během procesu ověřování se vyžadují k autorizaci žádosti o mikroslužbu nákupní košík. Další informace o autorizaci, najdete v části [autorizace](~/xamarin-forms/enterprise-application-patterns/authentication-and-authorization.md#authorization).

Následující příklad kódu ukazuje jeden z `PostAsync` metody v `RequestProvider` třídy:

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

Tato metoda volá `CreateHttpClient` metoda, která vrátí instanci `HttpClient` s nastavenou odpovídající hlavičky. Pak odešle požadavek POST asynchronní v prostředku označeném identifikátorem URI, s daty serializovaných košík odesílány ve formátu JSON a ukládají v reakci `HttpResponseMessage` instance. `HandleResponse` Potom je volána metoda, která vyvolá výjimku, pokud odpověď neobsahuje úspěšné stavový kód HTTP. Pak je pro čtení odpovědi jako řetězec převést z JSON na `CustomerBasket` objektu a vrátí `BasketService`. Další informace o `CreateHttpClient` metodu, najdete v části [provedení požadavku na získat](#making_a_get_request).

Když `PostAsync` metoda v `RequestProvider` třídy volání `HttpClient.PostAsync`, `Post` metoda v `BasketController` – třída v projektu Basket.API je vyvolána, která je uvedena v následujícím příkladu kódu:

```csharp
[HttpPost]  
public async Task<IActionResult> Post([FromBody]CustomerBasket value)  
{  
    var basket = await _repository.UpdateBasketAsync(value);  
    return Ok(basket);  
}
```

Tato metoda používá instanci `RedisBasketRepository` třídy zachování košík dat do mezipaměti Redis a vrátí ji jako zprávu odpovědi, která zahrnuje úspěšné stavový kód HTTP a JSON formátované `CustomerBasket` instance.

#### <a name="making-a-delete-request"></a>Provedení požadavku na odstranění

Obrázek 10-3 ukazuje interakce tříd, které odstraní košík data z košíku mikroslužbu, `CheckoutView`.

![](accessing-remote-data-images/checkoutdata.png "Při odstranění dat z mikroslužbu nákupní košík")

**Obrázek 10-3**: odstranění dat z mikroslužbu nákupní košík

Po vyvolání projít procesem `CheckoutAsync` metoda v `CheckoutViewModel` třídy se nazývá. Tato metoda vytvoří nové pořadí, než dojde k vymazání nákupní košík, jak je ukázáno v následujícím příkladu kódu:

```csharp
private async Task CheckoutAsync()  
{  
    ...  
    await _basketService.ClearBasketAsync(_shippingAddress.Id.ToString(), authToken);  
    ...  
}
```

Tato metoda volá `ClearBasketAsync` metodu `BasketService` instanci, která byla vloženy do `CheckoutViewModel` podle Autofac. Následující metoda ukazuje `ClearBasketAsync` metoda:

```csharp
public async Task ClearBasketAsync(string guidUser, string token)  
{  
    UriBuilder builder = new UriBuilder(GlobalSetting.Instance.BasketEndpoint);  
    builder.Path = guidUser;  
    string uri = builder.ToString();  
    await _requestProvider.DeleteAsync(uri, token);  
}
```

Tato metoda vytvoří identifikátor URI, který identifikuje prostředek, který bude odeslána žádost a používá `RequestProvider` třída volat metodu DELETE HTTP u daného prostředku. Všimněte si, že přístupový token, získaný IdentityServer během procesu ověřování se vyžadují k autorizaci žádosti o mikroslužbu nákupní košík. Další informace o autorizaci, najdete v části [autorizace](~/xamarin-forms/enterprise-application-patterns/authentication-and-authorization.md#authorization).

Následující příklad kódu ukazuje `DeleteAsync` metoda v `RequestProvider` třídy:

```csharp
public async Task DeleteAsync(string uri, string token = "")  
{  
    HttpClient httpClient = CreateHttpClient(token);  
    await httpClient.DeleteAsync(uri);  
}
```

Tato metoda volá `CreateHttpClient` metoda, která vrátí instanci `HttpClient` s nastavenou odpovídající hlavičky. Poté odešle Asynchronní požadavek DELETE v prostředku označeném identifikátorem URI. Další informace o `CreateHttpClient` metodu, najdete v části [provedení požadavku na získat](#making_a_get_request).

Když `DeleteAsync` metoda v `RequestProvider` třídy volání `HttpClient.DeleteAsync`, `Delete` metoda v `BasketController` – třída v projektu Basket.API je vyvolána, která je uvedena v následujícím příkladu kódu:

```csharp
[HttpDelete("{id}")]  
public void Delete(string id)  
{  
    _repository.DeleteBasketAsync(id);  
}
```

Tato metoda používá instanci `RedisBasketRepository` třída odstraníte košík data z mezipaměti Redis.

## <a name="caching-data"></a>Ukládaní dat do mezipaměti

Výkon aplikace lze zvýšit pomocí ukládání do mezipaměti často používaná data do rychlé úložiště, který je umístěný zavřít aplikaci. Pokud se nachází blíže k aplikaci než původního zdroje rychlé úložiště, pak ukládání do mezipaměti může výrazně zlepšit odpovědi časový limit při získávání data.

Nejběžnější formu ukládání do mezipaměti je read-through ukládání do mezipaměti, kde aplikace načte data pod položkou mezipaměti. Není-li data v mezipaměti, má načíst z úložiště dat a přidány do mezipaměti. Aplikace můžete implementovat read-through ukládání do mezipaměti s doplňováním mezipaměti. Tento vzor Určuje, zda je položka v současné době v mezipaměti. Pokud položka není v mezipaměti, má číst z úložiště dat a přidány do mezipaměti. Další informace najdete v tématu [mezipaměti vyhraďte](/azure/architecture/patterns/cache-aside/) vzor.

> [!TIP]
> Data do mezipaměti, je často čtou a který zřídka mění. Tato data lze přidat do mezipaměti na vyžádání poprvé, že ho načte aplikaci. To znamená, že aplikace je potřeba načíst data pouze jednou z úložiště dat a že následné přístup může obsloužit pomocí mezipaměti.

Distribuované aplikace, jako eShopOnContainers odkazovat na aplikaci, by měl poskytovat jedné nebo obou následujících mezipaměti:

-   Sdílené mezipaměti, který je přístupný pomocí několika procesů nebo počítače.
-   Privátní mezipaměti, kde se data uchovávat místně na zařízení, na kterém aplikace běží.

Mobilní aplikace eShopOnContainers používá privátní mezipaměti, kde se data uchovávat místně na zařízení, které je spuštěna instance aplikace. Informace o mezipaměti používané eShopOnContainers referenční aplikací najdete v tématu [Mikroslužeb .NET: architektura pro kontejnerové aplikace .NET](https://aka.ms/microservicesebook).

> [!TIP]
> Mezipaměti si můžete představit jako přechodný datové úložiště, které může kdykoli ztratit. Ujistěte se, že data se udržuje v původní úložiště dat a také do mezipaměti. Minimalizuje případné ztráty dat se pak mezipaměti přestane být dostupný.

### <a name="managing-data-expiration"></a>Správa Data vypršení platnosti

Je nepraktické očekávat, že data uložená v mezipaměti bude vždy konzistentní s původní data. Data v úložišti dat původní může změnit po byl do mezipaměti, způsobuje data uložená v mezipaměti se zastaralé. Proto aplikace by měla implementovat strategii, která pomáhá zajistit, že data v mezipaměti je jako aktuální nejmenší, ale můžete také zjistit a řešení situací, které vznikají při již zastaralá data v mezipaměti. Většina ukládání do mezipaměti mechanismy povolit mezipaměť nakonfigurovat tak, aby platnost dat a proto zkrátit dobu, pro kterou data mohou být zastaralé.

> [!TIP]
> Nastavit dobu platnosti výchozí čas při konfiguraci mezipaměti. Mnoho mezipamětí implementovat vypršení platnosti, která by způsobila neplatnost dat a odstraní ji z mezipaměti, pokud není k němu připojil po zadanou dobu. Však musí dát pozor při výběru dobu platnosti. Pokud se provádí příliš krátký, data příliš rychle vyprší a bude snížen výhod ukládání do mezipaměti. Pokud je k příliš dlouhý, data rizika zastaralé. Čas vypršení platnosti, proto by měl odpovídat vzor přístupu pro aplikace, které používají data.

Pokud data uložená v mezipaměti vyprší, měla by být odebrána z mezipaměti a aplikace musí získat data z původní data ukládat a umístěte ji zpět do mezipaměti.

Je také možné, že mezipaměť může zaplnit Pokud dat může zůstat po příliš dlouhou dobu. Proto žádosti o přidání nových položek do mezipaměti bude pravděpodobně nutné odebrat některé položky v tento proces se označuje jako *vyřazení*. Ukládání do mezipaměti služby obvykle vyřazení dat na základě nejmenší naposledy použitých. Existují však další zásady vyřazení, včetně naposledy použité a objektů first in first out. Další informace najdete v tématu [ukládání do mezipaměti pokyny](/azure/architecture/best-practices/caching/).

<a name="caching_images" />

### <a name="caching-images"></a>Ukládání do mezipaměti bitové kopie

Mobilní aplikace eShopOnContainers spotřebuje obrázky vzdálené produktů, které těžit z mezipaměti. Tyto Image jsou zobrazeny [ `Image` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Image/) řízení a `CachedImage` řízení poskytované [FFImageLoading](https://www.nuget.org/packages/Xamarin.FFImageLoading.Forms/) knihovny.

Platformě Xamarin.Forms [ `Image` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Image/) řízení podporuje ukládání do mezipaměti stažené bitových kopií. Ukládání do mezipaměti je ve výchozím nastavení povolené a uloží obrázek místně po dobu 24 hodin. Kromě toho je možné nakonfigurovat čas vypršení platnosti se [ `CacheValidity` ](https://developer.xamarin.com/api/property/Xamarin.Forms.UriImageSource.CacheValidity/) vlastnost. Další informace najdete v tématu [stáhnout bitovou kopii do mezipaměti](~/xamarin-forms/user-interface/images.md#Image_Caching).

Na FFImageLoading `CachedImage` ovládací prvek je náhradou za platformě Xamarin.Forms [ `Image` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Image/) ovládací prvek a poskytne další vlastnosti, které umožňují doplňující funkce. Mezi tato funkce poskytuje ovládací prvek konfigurovat ukládání do mezipaměti, a podpora chyba a načítání zástupné symboly bitové kopie. Následující příklad kódu ukazuje, jak eShopOnContainers mobilní aplikace používá `CachedImage` řídit ve `ProductTemplate`, které je šablona dat používané [ `ListView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/) řídit ve `CatalogView`:

```xaml
<ffimageloading:CachedImage
    Grid.Row="0"
    Source="{Binding PictureUri}"     
    Aspect="AspectFill">
    <ffimageloading:CachedImage.LoadingPlaceholder>
        <OnPlatform x:TypeArguments="ImageSource">
            <On Platform="iOS, Android" Value="default_campaign" />
            <On Platform="UWP, WinRT, WinPhone" Value="Assets/default_campaign.png" />
        </OnPlatform>
    </ffimageloading:CachedImage.LoadingPlaceholder>
    <ffimageloading:CachedImage.ErrorPlaceholder>
        <OnPlatform x:TypeArguments="ImageSource">
            <On Platform="iOS, Android" Value="noimage" />
            <On Platform="UWP, WinRT, WinPhone" Value="Assets/noimage.png" />
        </OnPlatform>
    </ffimageloading:CachedImage.ErrorPlaceholder>
</ffimageloading:CachedImage>
```

`CachedImage` Řízení nastaví `LoadingPlaceholder` a `ErrorPlaceholder` vlastností bitové kopie specifické pro platformu. `LoadingPlaceholder` Vlastnost určuje obrázek, který se bude zobrazovat během bitovou kopii určeného `Source` vlastnosti je načtena a `ErrorPlaceholder` vlastnost určuje obrázek, který má být zobrazen v případě, že dojde k chybě při pokusu o načtení obrázku specifikace `Source` vlastnost.

Jak již název napovídá, `CachedImage` řízení vzdálené bitové kopie v zařízení ukládá do mezipaměti po dobu uvedenou hodnotou `CacheDuration` vlastnost. Hodnota této vlastnosti není explicitně nastavena, je použita výchozí hodnota 30 dní.

## <a name="increasing-resilience"></a>Zvyšuje odolnost

Všechny aplikace, které komunikují s vzdálené služby a prostředky musí být citlivé na přechodných. Přechodných zahrnují aktuálně ztrátu připojení k síti pro služby, dočasné nedostupnosti služeb nebo vypršení časových limitů, které vzniknou, když služba je zaneprázdněná. Tyto chyby jsou často samy opraví, a pokud se po vhodný zpoždění opakuje akci bude pravděpodobně úspěšné.

Přechodných může mít velký dopad na kvalitu dosahovaný aplikace, i v případě, že byla důkladně neotestovali za všech okolností předvídatelné. Zkontrolovat, že aplikaci, která komunikuje se službou vzdálené funkce spolehlivě, musí být schopen provést následující kroky:

-   Chyby rozpoznat, kdy dochází a určí, jestli je k poruchám mohly být přechodné.
-   Opakujte operaci, pokud zjistí, že selhání je pravděpodobně být dočasné a sledovat počet, který byl zopakován.
-   Použijte příslušné opakování strategie, která určuje počet opakovaných pokusů, zpoždění mezi jednotlivými pokusy o a akcích, lze provést po neúspěšný pokus.

Tato přechodná chyba zpracování lze dosáhnout zabalení všechny pokusy o přístup ke vzdálené službě v kódu, který implementuje vzor opakování.

### <a name="retry-pattern"></a>Opakujte vzor

Pokud aplikace zjistí chybu při pokusu poslat žádost o vzdálené služby, je selhání zpracovat v některém z následujících způsobů:

-   Opakováním operace. Aplikace může opakovat požadavek selhání okamžitě.
-   Opakováním operace po prodlevě. Aplikace má čekat vhodnou dobu před opakováním žádosti.
-   Zrušení operace. Aplikace by měl operaci zrušte a sestav k výjimce.

Strategie opakování má přizpůsobená pro splnění požadavků na obchodní aplikace. Například je důležité k optimalizaci počet opakování a intervalu probíhá pokus o operaci opakujte. Pokud je operace součástí interakce s uživatelem, interval opakování musí být krátké a pouze několik opakování pokusu o Vyvarujte se čekat na odpověď uživatele. Pokud je součást dlouhotrvající pracovního postupu, operace kde zrušení nebo restartování pracovního postupu je nákladné nebo časově náročné, je vhodné již čekání mezi pokusy a opakujte vícekrát.

> [!NOTE]
> Strategie agresivní opakování s minimálním zpožděním mezi pokusy a velký počet opakovaných pokusů, může dojít ke snížení Vzdálená služba, která běží blízkých nebo na kapacitě. Kromě toho tato strategie opakování mohou také ovlivnit rychlost reakce aplikace Pokud průběžně se pokouší provést operaci selhání.

Pokud se žádost o stále nedaří po uplynutí počtu opakování, je lepší pro aplikaci, aby se zabránilo další požadavky, chystáte stejného zdroje a ohlásí chybu. Potom po určené, může aplikace využít jeden nebo více požadavků pro tento prostředek zobrazit, pokud jsou úspěšné. Další informace najdete v tématu [jistič vzor](#circuit_breaker_pattern).

> [!TIP]
> Nikdy implementovat mechanismus nekonečná opakování. Použít omezený počet opakovaných pokusů, nebo implementovat [jistič](/azure/architecture/patterns/circuit-breaker/) vzor povolit služby pro obnovení.

Mobilní aplikace eShopOnContainers neimplementuje aktuálně vzoru opakovat při provádění RESTful webových požadavků. Ale `CachedImage` řízení, poskytované [FFImageLoading](https://www.nuget.org/packages/Xamarin.FFImageLoading.Forms/) knihovna podporuje přechodná chyba zpracování opakováním načítání bitové kopie. Pokud se nezdaří načítání bitové kopie, budou provedeny další pokusy. Počet pokusů o je zadána `RetryCount` vlastnost a opakování dojde po prodlevě určeného `RetryDelay` vlastnost. Pokud tyto hodnoty vlastností nejsou explicitně nastaven, jejich výchozí hodnoty se použijí – 3 pro `RetryCount` vlastnost a 250ms pro `RetryDelay` vlastnost. Další informace o `CachedImage` řízení najdete v tématu [ukládání do mezipaměti image](#caching_images).

Odkaz na aplikaci eShopOnContainers implementovat vzor opakování. Další informace, včetně diskuzi o tom, jak kombinací vzor opakování s `HttpClient` třídy najdete v tématu [Mikroslužeb .NET: architektura pro kontejnerové aplikace .NET](https://aka.ms/microservicesebook).

Další informace o vzoru opakování, najdete v článku [opakujte](/azure/architecture/patterns/retry/) vzor.

<a name="circuit_breaker_pattern" />

### <a name="circuit-breaker-pattern"></a>Vzor jistič

V některých situacích chyb může nastat v důsledku předpokládaného události, které trvat déle, než se opravit. Tyto chyby musí být v rozsahu částečné ztrátě připojení k úplnému selhání služby. V těchto situacích je zbytečný pro aplikaci, opakujte operaci, která je pravděpodobně úspěšná a místo toho může přijmout, operace se nezdařila a odpovídajícím způsobem zpracování této chyby.

Vzor jistič zabránit opakovaného pokusu o provedení operace, která je pravděpodobně selžou, a zároveň umožnit aplikace ke zjištění, zda byl vyřešen selhání aplikace.

> [!NOTE]
> Účelem tohoto vzoru jistič se liší od vzoru opakování. Vzor opakování umožňuje aplikaci opakovat operace v očekávání, který budete úspěšně. Vzor jistič brání aplikaci v provádění operace, která je pravděpodobně selžou.

Jistič funguje jako proxy pro operace, které může selhat. Proxy server by měl sledovat počet poslední chyby, ke kterým došlo v a použijte tyto informace můžete rozhodnout, jestli k povolení operace, aby bylo možné pokračovat, nebo pro návrat k výjimce okamžitě.

Mobilní aplikace eShopOnContainers neimplementuje aktuálně jistič vzor. EShopOnContainers však nepodporuje. Další informace najdete v tématu [Mikroslužeb .NET: architektura pro kontejnerové aplikace .NET](https://aka.ms/microservicesebook).

> [!TIP]
> Kombinovat opakování a jistič vzory. Aplikace můžete kombinovat vzory opakování a jistič pomocí vzoru opakování k vyvolání operace prostřednictvím jistič. Logika opakovaných pokusů by však být citlivé na jakékoli výjimky vrácený vypínač a abandon opakovaných pokusů, pokud vypínač signalizuje, že chybu není přechodný.

Další informace o vzoru jistič najdete v tématu [jistič](/azure/architecture/patterns/circuit-breaker/) vzor.

## <a name="summary"></a>Souhrn

Ujistěte se, mnohá moderní řešení založené na webu pomocí webových služeb, hostované webové servery, nabízí funkce pro vzdálené klientské aplikace. Operace, které zpřístupní webové služby tvoří webového rozhraní API a klientské aplikace by měla umět využívají webové rozhraní API bez znalosti, jak jsou implementované dat nebo operace, které zpřístupňuje rozhraní API.

Výkon aplikace lze zvýšit pomocí ukládání do mezipaměti často používaná data do rychlé úložiště, který je umístěný zavřít aplikaci. Aplikace můžete implementovat read-through ukládání do mezipaměti s doplňováním mezipaměti. Tento vzor Určuje, zda je položka v současné době v mezipaměti. Pokud položka není v mezipaměti, má číst z úložiště dat a přidány do mezipaměti.

Při komunikaci s webových rozhraní API, aplikace musí být citlivé na přechodných. Přechodných zahrnují aktuálně ztrátu připojení k síti pro služby, dočasné nedostupnosti služeb nebo vypršení časových limitů, které vzniknou, když služba je zaneprázdněná. Tyto chyby jsou často samy opraví, a pokud po vhodný zpoždění se opakuje akci, je pravděpodobně úspěšné. Aplikace má proto obtékat všechny pokusy o přístup k webovému rozhraní API v kódu, který implementuje přechodná chyba mechanismu pro zpracování.


## <a name="related-links"></a>Související odkazy

- [Stáhnout elektronická kniha (2Mb PDF)](https://aka.ms/xamarinpatternsebook)
- [eShopOnContainers (Githubu) (ukázka)](https://github.com/dotnet-architecture/eShopOnContainers)
