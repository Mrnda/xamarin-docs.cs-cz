---
title: Ověřování uživatelů s databází Azure Cosmos DB dokumentu
description: Databáze dokumentů Azure Cosmos DB podporují dělené kolekce, které může mít rozsah více serverů a oddíly, spolu s podporou neomezené úložiště a propustnosti. Tento článek vysvětluje, jak spojovat řízení přístupu se dělené kolekce tak, aby uživatel přístup jenom k své vlastní dokumenty v aplikaci Xamarin.Forms.
ms.topic: article
ms.prod: xamarin
ms.assetid: 11ED4A4C-0F05-40B2-AB06-5A0F2188EF3D
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 06/16/2017
ms.openlocfilehash: 8de64d6489b4022e43bcf694f3b13d6f7eaaecbd
ms.sourcegitcommit: 7b76c3d761b3ffb49541e2e2bcf292de6587c4e7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 03/23/2018
---
# <a name="authenticating-users-with-an-azure-cosmos-db-document-database"></a>Ověřování uživatelů s databází Azure Cosmos DB dokumentu

_Databáze dokumentů Azure Cosmos DB podporují dělené kolekce, které může mít rozsah více serverů a oddíly, spolu s podporou neomezené úložiště a propustnosti. Tento článek vysvětluje, jak spojovat řízení přístupu se dělené kolekce tak, aby uživatel přístup jenom k své vlastní dokumenty v aplikaci Xamarin.Forms._

## <a name="overview"></a>Přehled

Klíč oddílu je nutné zadat při vytváření oddílů kolekcí a dokumenty se stejným klíčem oddílu budou ukládat do stejného oddílu. Proto zadání identitu uživatele jako klíč oddílu způsobí dělenou kolekci, které se uloží pouze dokumenty pro daného uživatele. To také zajistí bude škálovat databáze dokumentů Azure Cosmos DB jako počet uživatelů, a zvýšit položky.

Udělit přístup, musí všechny kolekce a model řízení přístupu rozhraní SQL API definuje dva typy konstrukce přístup:

- **Hlavní klíče** povolit plný přístup správce ke všem prostředkům v rámci účtu Cosmos DB a vytvářejí, když je vytvořen účet Cosmos DB.
- **Tokeny prostředků** zaznamenat vztah mezi uživateli databáze a tato oprávnění má uživatel pro konkrétní prostředek Cosmos DB, jako jsou kolekce nebo dokumentu.

Vystavení hlavní klíč otevře účet Cosmos DB možnost škodlivý nebo nedbalosti použití. Ale tokeny prostředků Azure Cosmos DB poskytovat nouzový mechanismus, který umožňuje klientům číst, zapisovat a odstranit konkrétní prostředky v účtu Azure Cosmos DB podle udělená oprávnění.

Typické přístup na vyžádání, generování a dodáte tokeny prostředků do mobilní aplikace je použití tokenu zprostředkovatele prostředků. Následující diagram znázorňuje souhrnné informace o tom, jak ukázková aplikace používá token zprostředkovatele prostředků ke správě přístupu k datům databáze dokumentu:

![](authentication-images/documentdb-authentication.png "Proces ověřování databáze dokumentu")

Token zprostředkovatele prostředků je služba webového rozhraní API střední vrstvy, hostované v Azure App Service, která má hlavní klíč účtu Cosmos DB. Ukázková aplikace používá token zprostředkovatele prostředků ke správě přístupu k datům databáze dokumentu následujícím způsobem:

1. Přihlašování kontaktuje aplikaci Xamarin.Forms Azure App Service k zahájení tok ověřování.
1. Aplikační služba Azure provádí tok ověřování OAuth službou Facebook. Po dokončení tok ověřování aplikace Xamarin.Forms obdrží token přístupu.
1. Xamarin.Forms aplikace používá přístupový token k vyžádání tokenu prostředků z tokenu zprostředkovatele prostředků.
1. Zprostředkovatel tokenu prostředků používá přístupový token k vyžádání identitu uživatele ze sítě Facebook. Identitu uživatele se pak používá k vyžádání tokenu prostředku z databáze Cosmos, který se používá k udělení přístupu pro čtení a zápis do dělené kolekce ověřeného uživatele.
1. Xamarin.Forms aplikace používá token prostředku přímý přístup k prostředkům Cosmos DB s oprávněními definované token prostředku.

> [!NOTE]
> Když vyprší platnost token prostředku, obdrží požadavků databáze následné dokumentu 401 neoprávněný výjimka. V tomto okamžiku by měl Xamarin.Forms aplikace opětovné zřízení identity a požádat o nový token prostředku.

Další informace o vytváření oddílů Cosmos DB najdete v tématu [postup oddílu a škálování v Azure Cosmos DB](/azure/cosmos-db/partition-data/). Další informace o řízení přístupu Cosmos DB najdete v tématu [zabezpečení přístupu k datům databáze Cosmos](/azure/cosmos-db/secure-access-to-data/) a [řízení přístupu v rozhraní SQL API](/rest/api/documentdb/access-control-on-documentdb-resources/).

## <a name="setup"></a>Instalace

Proces pro integraci tokenu zprostředkovatele prostředků do aplikace Xamarin.Forms vypadá takto:

1. Vytvořte účet Cosmos DB, který bude používat řízení přístupu. Další informace najdete v tématu [Cosmos DB konfigurace](#cosmosdb_configuration).
1. Vytvořte aplikační službu Azure k hostování tokenu zprostředkovatele prostředků. Další informace najdete v tématu [konfigurace služby Azure App Service](#app_service_configuration).
1. Vytvoření aplikace pro Facebook k provedení ověřování. Další informace najdete v tématu [konfigurace aplikace Facebook](#facebook_configuration).
1. Konfigurace Azure App Service k snadno ověřování službou Facebook. Další informace najdete v tématu [konfiguraci ověřování služby Azure App](#app_service_authentication_configuration).
1. Nakonfigurujte ukázkovou aplikaci Xamarin.Forms ke komunikaci s Azure App Service a Cosmos DB. Další informace najdete v tématu [konfigurace aplikace Xamarin.Forms](#forms_application_configuration).

<a name="cosmosdb_configuration" />

### <a name="azure-cosmos-db-configuration"></a>Konfigurace Azure Cosmos DB

Proces vytvoření Cosmos DB účet, který bude používat řízení přístupu je následující:

1. Vytvořte účet Cosmos DB. Další informace najdete v tématu [vytvoření účtu Azure Cosmos DB](/azure/cosmos-db/sql-api-dotnetcore-get-started#step-1-create-an-azure-cosmos-db-account).
1. V účtu Cosmos DB vytvořit novou kolekci s názvem `UserItems`, zadávání klíče oddílu z `/userid`.

<a name="app_service_configuration" />

### <a name="azure-app-service-configuration"></a>Konfigurace služby Azure App Service

Proces hostování tokenu zprostředkovatele prostředků v Azure App Service je následující:

1. Na portálu Azure vytvořte novou webovou aplikaci služby App Service. Další informace najdete v tématu [vytvořit webovou aplikaci ve službě App Service Environment](/azure/app-service-web/app-service-web-how-to-create-a-web-app-in-an-ase/).
1. Na portálu Azure otevře se okno nastavení aplikace pro webovou aplikaci a přidejte následující nastavení:
    - `accountUrl` – Hodnota musí být adresa URL Cosmos DB účtu v okně klíče účtu Cosmos DB.
    - `accountKey` – Hodnota musí být Cosmos DB hlavního klíče (primární nebo sekundární) v okně klíče účtu Cosmos DB.
    - `databaseId` – Hodnota musí být název databáze Cosmos DB.
    - `collectionId` – Hodnota musí být název Cosmos DB kolekce (v tomto případě `UserItems`).
    - `hostUrl` – Hodnota musí být adresa URL webové aplikace v okně Přehled účtu služby App Service.

    Následující snímek obrazovky ukazuje tuto konfiguraci:

    [![](authentication-images/azure-web-app-settings.png "Nastavení webové aplikace služby App Service")](authentication-images/azure-web-app-settings-large.png#lightbox "nastavení webové aplikace služby App Service")

1. Publikujte řešení tokenu zprostředkovatele prostředků do webové aplikace služby Azure App Service.

<a name="facebook_configuration" />

### <a name="facebook-app-configuration"></a>Konfigurace aplikace Facebook

Proces vytvoření aplikace pro síť Facebook provádět ověřování vypadá takto:

1. Vytvoření aplikace pro Facebook. Další informace najdete v tématu [zaregistrujte a nakonfigurujte aplikaci](https://developers.facebook.com/docs/apps/register) v Centru pro vývojáře Facebook.
1. Přidání produktu Facebook přihlášení do aplikace. Další informace najdete v tématu [přidat Facebook přihlášení na webu nebo aplikace](https://developers.facebook.com/docs/facebook-login) v Centru pro vývojáře Facebook.
1. Přihlášení Facebook nakonfigurujte následujícím způsobem:
  - Povolte přihlášení klienta OAuth.
  - Povolte přihlášení na Web OAuth.
  - Nastavení přesměrování OAuth platný identifikátor URI pro identifikátor URI webové aplikace App Service se `/.auth/login/facebook/callback` připojí.

  Následující snímek obrazovky ukazuje tuto konfiguraci:

  ![](authentication-images/facebook-oauth-settings.png "Nastavení sítě Facebook přihlášení OAuth")

Další informace najdete v tématu [registrace vaší aplikace se sítí Facebook](/azure/app-service-mobile/app-service-mobile-how-to-configure-facebook-authentication#a-nameregister-aregister-your-application-with-facebook).

<a name="app_service_authentication_configuration" />

### <a name="azure-app-service-authentication-configuration"></a>Konfigurace ověřování služby Azure App Service

Proces pro konfiguraci služby App Service, snadné ověřování vypadá takto:

1. Na portálu Azure přejděte do webové aplikace App Service.
1. Na portálu Azure otevřete ověřování / autorizace okno a proveďte následující konfiguraci:
  - Ověřování služby aplikace by měl být zapnut.
  - Pro případ, když požadavek nebude ověřený musí být nastavena na **přihlášení pomocí Facebooku**.

  Následující snímek obrazovky ukazuje tuto konfiguraci:

  [![](authentication-images/app-service-authentication-settings.png "Nastavení ověřování webové aplikace služby App Service")](authentication-images/app-service-authentication-settings-large.png#lightbox "nastavení ověřování webové aplikace služby App Service")

Webové aplikace App Service musí také nakonfigurován pro komunikaci se povolit tok ověřování aplikace pro síť Facebook. To lze provést výběrem zprostředkovatele identity Facebook a zadáním **ID aplikace** a **tajný klíč aplikace** hodnoty z nastavení aplikace sítě Facebook v Centru pro vývojáře Facebook. Další informace najdete v tématu [Facebook přidat informace do vaší aplikace](/azure/app-service-mobile/app-service-mobile-how-to-configure-facebook-authentication#a-namesecrets-aadd-facebook-information-to-your-application).

<a name="forms_application_configuration" />

### <a name="xamarinforms-application-configuration"></a>Konfigurace aplikace Xamarin.Forms

Proces pro konfiguraci ukázkovou aplikaci Xamarin.Forms vypadá takto:

1. Otevřete řešení Xamarin.Forms.
1. Otevřete `Constants.cs` a aktualizujte hodnoty následující konstanty:
  - `EndpointUri` – Hodnota musí být adresa URL Cosmos DB účtu v okně klíče účtu Cosmos DB.
  - `DatabaseName` – Hodnota musí být název databáze dokumentu.
  - `CollectionName` – Hodnota musí být název databáze kolekci dokumentů (v tomto případě `UserItems`).
  - `ResourceTokenBrokerUrl` – Hodnota musí být adresa URL webové aplikace tokenu zprostředkovatele prostředků v okně Přehled účtu služby App Service.

## <a name="initiating-login"></a>Inicializace přihlášení

Ukázkovou aplikaci zahájí se proces přihlášení pomocí Xamarin.Auth k přesměrování prohlížeče na adresu URL zprostředkovatele identity, jak je ukázáno v následujícím příkladu kódu:

```csharp
var auth = new Xamarin.Auth.WebRedirectAuthenticator(
  new Uri(Constants.ResourceTokenBrokerUrl + "/.auth/login/facebook"),
  new Uri(Constants.ResourceTokenBrokerUrl + "/.auth/login/done"));
```

To způsobí, že tok ověřování OAuth, inicializovat mezi Azure App Service a Facebook, který zobrazí stránku Facebook přihlášení:

![](authentication-images/login.png "Přihlašovací službě Facebook.")

Přihlášení můžete zrušit stisknutím **zrušit** v systému iOS nebo stisknutím tlačítka **zpět** na Android, v takovém případě zůstane neověřené uživatele a uživatelské rozhraní pro zprostředkovatele identity je tlačítko odebrat z obrazovky.

Další informace o Xamarin.Auth najdete v tématu [ověřování uživatelů pomocí zprostředkovatele Identity](~/xamarin-forms/data-cloud/authentication/oauth.md).

## <a name="obtaining-a-resource-token"></a>Získání tokenu prostředků

Po úspěšném ověření `WebRedirectAuthenticator.Completed` je aktivována událost. Následující příklad kódu ukazuje zpracování této události:

```csharp
auth.Completed += async (sender, e) =>
{
  if (e.IsAuthenticated && e.Account.Properties.ContainsKey("token"))
  {
    var easyAuthResponseJson = JsonConvert.DeserializeObject<JObject>(e.Account.Properties["token"]);
    var easyAuthToken = easyAuthResponseJson.GetValue("authenticationToken").ToString();

    // Call the ResourceBroker to get the resource token
    using (var httpClient = new HttpClient())
    {
      httpClient.DefaultRequestHeaders.Add("x-zumo-auth", easyAuthToken);
      var response = await httpClient.GetAsync(Constants.ResourceTokenBrokerUrl + "/api/resourcetoken/");
      var jsonString = await response.Content.ReadAsStringAsync();
      var tokenJson = JsonConvert.DeserializeObject<JObject>(jsonString);
      resourceToken = tokenJson.GetValue("token").ToString();
      UserId = tokenJson.GetValue("userid").ToString();

      if (!string.IsNullOrWhiteSpace(resourceToken))
      {
        client = new DocumentClient(new Uri(Constants.EndpointUri), resourceToken);
        ...
      }
      ...
    }
  }
};
```

Výsledkem úspěšného ověření je přístupový token, který je k dispozici `AuthenticatorCompletedEventArgs.Account` vlastnost. Přístupový token je extrahována a použít v požadavek GET na token zprostředkovatele prostředků `resourcetoken` rozhraní API.

`resourcetoken` Rozhraní API používá přístupový token k vyžádání identitu uživatele ze sítě Facebook, která se pak použije k vyžádání tokenu prostředku z databáze Cosmos. Pokud již dokument platné oprávnění pro uživatele v databázi dokumentů existuje, načítání a dokumentu JSON, který obsahuje token prostředku se vrátí k aplikaci Xamarin.Forms. Pokud pro uživatele neexistuje platná oprávnění dokumentu, uživatele a oprávnění se vytvoří v databázi dokumentů a token prostředku se extrahují z dokumentu oprávnění a vrátí aplikaci Xamarin.Forms v dokumentu JSON.

> [!NOTE]
> Uživatel databáze dokumentu je prostředek přidružené k databázi dokumentů a každou databázi může obsahovat nula nebo více uživatelů. Oprávnění databáze dokumentu je prostředek přidružit k uživateli databáze dokumentů a každý uživatel může obsahovat nula nebo více oprávnění. Prostředek oprávnění poskytuje přístup k tokenu zabezpečení, který uživatel vyžaduje při pokusu o přístup k prostředku, jako je například dokument.

Pokud `resourcetoken` rozhraní API úspěšně dokončí, odešle stavový kód HTTP 200 (OK) v odpovědi, společně s dokumentu JSON, který obsahuje token prostředku. Následující data JSON zobrazí zprávu s typické úspěšné odpovědi:

```csharp
{
  "id": "John Smithpermission",
  "token": "type=resource&ver=1&sig=zx6k2zzxqktzvuzuku4b7y==;a74aukk99qtwk8v5rxfrfz7ay7zzqfkbfkremrwtaapvavw2mrvia4umbi/7iiwkrrq+buqqrzkaq4pp15y6bki1u//zf7p9x/aefbvqvq3tjjqiffurfx+vexa1xarxkkv9rbua9ypfzr47xpp5vmxuvzbekkwq6txme0xxxbjhzaxbkvzaji+iru3xqjp05amvq1r1q2k+qrarurhmjzah/ha0evixazkve2xk1zu9u/jpyf1xrwbkxqpzebvqwma+hyyaazemr6qx9uz9be==;",
  "expires": 4035948,
  "userid": "John Smith"
}
```

`WebRedirectAuthenticator.Completed` Obslužná rutina události načte odpověď `resourcetoken` rozhraní API a extrahuje token prostředku a id uživatele. Token prostředku je předána jako argument pro `DocumentClient` konstruktor, který zapouzdřuje koncový bod, přihlašovací údaje a připojení zásady používané pro přístup k Cosmos DB a slouží ke konfiguraci a spuštění požadavky DHCP proti Cosmos DB. Token prostředku je odeslán s každou žádostí přímý přístup k prostředku a označuje, že je udělen přístup pro čtení a zápis k dělenou kolekci ověřené uživatele.

## <a name="retrieving-documents"></a>Načítání dokumentů

Načítání dokumenty, které pouze patří k ověřenému uživateli lze dosáhnout vytvořením dokumentu dotaz, který obsahuje id uživatele jako klíč oddílu a je ukázáno v následujícím příkladu kódu:

```csharp
var query = client.CreateDocumentQuery<TodoItem>(collectionLink,
                        new FeedOptions
                        {
                          MaxItemCount = -1,
                          PartitionKey = new PartitionKey(UserId)
                        })
          .Where(item => !item.Id.Contains("permission"))
          .AsDocumentQuery();
while (query.HasMoreResults)
{
  Items.AddRange(await query.ExecuteNextAsync<TodoItem>());
}
```

Asynchronně načte všechny dokumenty, které patří do ověřeného uživatele ze zadané kolekci a umístí je do dotazu `List<TodoItem>` kolekce pro zobrazení.

`CreateDocumentQuery<T>` Určuje metoda `Uri` argument, který představuje kolekci, která mají být získána dokumenty a `FeedOptions` objektu. `FeedOptions` Objektu určuje, že dotaz a id uživatele jako klíč oddílu může vráceno neomezený počet položek. Tím se zajistí, že jen k dokumentům ve dělenou kolekci uživatele jsou vráceny ve výsledku.

> [!NOTE]
> Všimněte si, že oprávnění dokumenty, které jsou vytvořené pomocí tokenu zprostředkovatele prostředků, jsou uloženy ve stejné kolekci dokumentu jako dokumenty, které vytvořila aplikace Xamarin.Forms. Proto dokumentu dotaz obsahuje `Where` klauzuli, která se použije k dotazu vůči kolekce dokumentů predikát pro filtrování. Tuto klauzuli zajistí, že nejsou oprávnění dokumenty vrácená z kolekce dokumentů.

Další informace o načtení dokumentů z kolekce dokumentů najdete v tématu [načítání dokumentu kolekce dokumentů](~/xamarin-forms/data-cloud/cosmosdb/consuming.md#document_query).

## <a name="inserting-documents"></a>Vkládání dokumentů

Před vkládání dokumentu do kolekce dokumentů, `TodoItem.UserId` vlastnost je třeba aktualizovat hodnotu používá jako klíč oddílu, jak je ukázáno v následujícím příkladu kódu:

```csharp
item.UserId = UserId;
await client.CreateDocumentAsync(collectionLink, item);
```

Tím se zajistí, že dokument se vloží do dělenou kolekci uživatele.

Další informace o dokumentu vložení do kolekce dokumentů najdete v tématu [vkládání dokumentu do kolekce dokumentů](~/xamarin-forms/data-cloud/cosmosdb/consuming.md#inserting_document).

## <a name="deleting-documents"></a>Odstraňování dokumentů

Při odstranění dokumentu z dělenou kolekci, jako ukázáno v následujícím příkladu kódu je třeba zadat hodnotu klíče oddílu:

```csharp
await client.DeleteDocumentAsync(UriFactory.CreateDocumentUri(Constants.DatabaseName, Constants.CollectionName, id),
                 new RequestOptions
                 {
                   PartitionKey = new PartitionKey(UserId)
                 });
```

To zajistí, že Cosmos DB ví, které oddíly kolekce k odstranění dokumentu z.

Další informace o odstranění dokumentu z kolekce dokumentů najdete v tématu [odstranění dokumentu z kolekce dokumentů](~/xamarin-forms/data-cloud/cosmosdb/consuming.md#deleting_document).

## <a name="summary"></a>Souhrn

Tento článek vysvětlení najdete postup kombinace řízení přístupu s dělené kolekce tak, aby uživatel přístup jenom k své vlastní dokumenty databáze dokumentů v aplikaci Xamarin.Forms. Určení identitu uživatele jako klíč oddílu zajistí, že dělenou kolekci lze pouze ukládat dokumenty pro tohoto uživatele.


## <a name="related-links"></a>Související odkazy

- [TODO Azure Cosmos DB Auth (ukázka)](https://developer.xamarin.com/samples/xamarin-forms/WebServices/TodoDocumentDBAuth/)
- [Používání databáze dokumentů Azure Cosmos DB](~/xamarin-forms/data-cloud/cosmosdb/consuming.md)
- [Zabezpečení přístupu k datům v Azure Cosmos DB](/azure/cosmos-db/secure-access-to-data/)
- [Řízení přístupu v rozhraní SQL API](/rest/api/documentdb/access-control-on-documentdb-resources/).
- [Vytvoření oddílů a škálování v Azure Cosmos DB](/azure/cosmos-db/partition-data/)
- [Klientská knihovna pro Azure Cosmos DB](https://www.nuget.org/packages/Microsoft.Azure.DocumentDB.Core)
- [Rozhraní API Azure Cosmos DB](https://msdn.microsoft.com/library/azure/dn948556.aspx)
