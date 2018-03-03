---
title: "Využívání RESTful webová služba"
description: "Integrace webové služby do aplikace je běžný scénář. Tento článek ukazuje, jak využívat RESTful webová služba z Xamarin.Forms aplikace."
ms.topic: article
ms.prod: xamarin
ms.assetid: B540910C-9C51-416A-AAB9-057BF76489C3
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 05/22/2017
ms.openlocfilehash: f8b748ad1b57218d1e8aab11bdc1037cf3cfa14c
ms.sourcegitcommit: 61f5ecc5a2b5dcfbefdef91664d7460c0ee2f357
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 02/28/2018
---
# <a name="consuming-a-restful-web-service"></a>Využívání RESTful webová služba

_Integrace webové služby do aplikace je běžný scénář. Tento článek ukazuje, jak využívat RESTful webová služba z Xamarin.Forms aplikace._

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

RESTful webové služby obvykle používají JSON zprávy klientovi vrátit data. JSON je textový formát pro výměnu, vytváří compact datových částí, čímž snižuje nároky na šířku pásma při odesílání dat. Ukázková aplikace používá open source [NewtonSoft JSON.NET knihovny](http://www.newtonsoft.com/json) k serializaci a deserializaci zprávy.

Jednoduchost REST pomohlo, vytvořte ji primární metodu pro přístup k webovým službám v mobilních aplikacích.

Pokyny k nastavení služby REST naleznete v souboru readme, který doprovází ukázkovou aplikaci. Však při spuštění ukázkové aplikace, se připojí k službě hostované Xamarin REST, která poskytuje přístup jen pro čtení k datům, jak je znázorněno na následujícím snímku obrazovky:

![](rest-images/portal.png "Ukázkové aplikace")

> [!NOTE]
> Zabezpečení přenosu aplikace (ATS) vynucuje v iOS 9 a vyšší, zabezpečené připojení mezi prostředků z Internetu (třeba server back-end aplikace) a aplikace, a tím prevence náhodného zpřístupnění citlivých informací. Protože ATS je povolené ve výchozím nastavení v aplikacích pro iOS 9, všechna připojení budou platit ATS požadavky na zabezpečení. Pokud připojení těchto požadavků nesplňuje, budou se nezdaří s výjimkou.
>
>ATS můžete vyjádřit výslovný souhlas mimo Pokud není možné použít **HTTPS** protokolu a zabezpečení komunikace pro prostředků z Internetu. Toho lze dosáhnout aktualizací aplikace **Info.plist** souboru. Další informace najdete v části [zabezpečení přenosu aplikace](~/ios/app-fundamentals/ats.md).

## <a name="consuming-the-web-service"></a>Využívají webové služby

Službu REST je určena pomocí ASP.NET Core a poskytuje následující operace:

<table>
  <thead>
    <tr>
      <th>Operace</th>
      <th>Metoda HTTP</th>
      <th>Relativní identifikátor URI</th>
      <th>Parametry</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>Získat seznam položek úkolů</td>
      <td>GET</td>
      <td>/API/todoitems /</td>
      <td></td>
    </tr>
    <tr>
      <td>Vytvořit novou položku seznamu úkolů</td>
      <td>POST</td>
      <td>/API/todoitems /</td>
      <td>Formátu JSON <code>TodoItem</code></td>
    </tr>
    <tr>
      <td>Aktualizujte položku seznamu úkolů</td>
      <td>PUT</td>
      <td>/API/todoitems /</td>
      <td>Formátu JSON <code>TodoItem</code></td>
    </tr>
    <tr>
      <td>Odstranit položku seznamu úkolů</td>
      <td>DELETE</td>
      <td>/API/todoitems / {id}</td>
      <td></td>
    </tr>
  </tbody>
</table>

Patří většina identifikátory URI `TodoItem` ID v cestě. Chcete-li například odstranit `TodoItem` jehož ID je `6bb8a868-dba1-4f1a-93b7-24ebce87e243`, klient odešle požadavek na odstranění `http://hostname/api/todoitems/6bb8a868-dba1-4f1a-93b7-24ebce87e243`. Další informace o modelu dat použít v ukázkové aplikaci najdete v tématu [modelování data](~/xamarin-forms/data-cloud/walkthrough.md).

Když rozhraní Web API přijme požadavek přesměruje požadavek na akci. Tyto akce jsou jednoduše veřejné metody v `TodoItemsController` třídy. Rozhraní používá směrovací tabulky určit akci, která má být vyvolán v reakci na žádost, která je znázorněno v následujícím příkladu kódu:

```csharp
config.Routes.MapHttpRoute(
    name: "TodoItemsApi",
    routeTemplate: "api/{controller}/{id}",
    defaults: new { controller="todoitems", id = RouteParameter.Optional }
);
```

Směrovací tabulka obsahuje šablonu trasy a když rozhraní Web API obdrží požadavek HTTP, pokouší se identifikátor URI pro šablonu trasy do směrovací tabulky. Pokud odpovídající trasy nebyl nalezen, že klient obdrží chybu 404 (nebyl nalezen). Pokud je nalezen odpovídající trasy, webového rozhraní API vybere kontroler a akce následujícím způsobem:

- Najít kontroler, přidá webového rozhraní API "controller" na hodnotu *{controller}* proměnné.
- Akce najdete webového rozhraní API vyhledá metodu protokolu HTTP a zjistí akce kontroleru, které jsou označených pomocí stejnou metodu HTTP jako atribut.
- *{Id}* zástupný symbol proměnné je namapována na parametr akce.

Služba REST používá základní ověřování. Další informace najdete v části [ověřování RESTful webová služba](~/xamarin-forms/data-cloud/authentication/rest.md). Další informace o směrování ASP.NET Web API najdete v tématu [směrování v rozhraní ASP.NET Web API](http://www.asp.net/web-api/overview/web-api-routing-and-actions/routing-in-aspnet-web-api) na webu technologie ASP.NET. Další informace o vytváření služby REST pomocí ASP.NET Core najdete v tématu [vytváření služeb back-end pro nativních aplikací Mobile](/aspnet/core/mobile/native-mobile-backend/).

> [!NOTE]
> Ukázková aplikace využívá službu hostované Xamarin REST, který poskytuje přístup jen pro čtení k webové službě. Proto operace, které vytvářet, aktualizovat a odstraňovat data nemění data využívat v aplikaci. Však je k dispozici v IIS s možnostmi hostitele verzi služby REST **TodoRESTService** složky z odpovídajícího [ukázkový kód](https://developer.xamarin.com/samples/xamarin-forms/WebServices/TodoREST/).
> Pokud službu REST hostujete sami, umožňuje úplné vytvářet, aktualizovat, přečtěte si a odstranit přístup k datům.

`HttpClient` Třída se používá k odesílání a přijímání požadavků prostřednictvím protokolu HTTP. Poskytuje funkce pro odesílání požadavků HTTP a příjem odpovědí HTTP z identifikátoru URI identifikuje prostředek. Každý požadavek odeslán jako asynchronní operaci. Další informace o asynchronní operace najdete v tématu [Přehled podpory asynchronní](~/cross-platform/platform/async.md).

`HttpResponseMessage` Třída reprezentuje zprávu odpovědi HTTP po provedl se požadavek HTTP přijata webovou službu. Obsahuje informace o odpověď, a to včetně stavový kód, hlavičky a všechny text. `HttpContent` Třída reprezentuje obsahu HTTP a hlavičky obsahu, jako například `Content-Type` a `Content-Encoding`. Obsah lze číst pomocí kteréhokoli `ReadAs` metody, například `ReadAsStringAsync` a `ReadAsByteArrayAsync`, v závislosti na formát data.

### <a name="creating-the-httpclient-object"></a>Vytvoření objektu HTTPClient

`HttpClient` Tak, aby objekt funguje tak dlouho, dokud aplikace potřebuje provést požadavků protokolu HTTP, jak je znázorněno v následujícím příkladu kódu je na úrovni třídy deklarována instance:

```csharp
public class RestService : IRestService
{
  HttpClient client;
  ...

  public RestService ()
  {
    client = new HttpClient ();
    client.MaxResponseContentBufferSize = 256000;
  }
  ...
}
```

`HttpClient.MaxResponseContentBufferSize` Vlastnost se používá k určení maximální počet bajtů, které mají ukládat do vyrovnávací paměti při čtení obsahu ve zprávě s odpovědí HTTP. Výchozí velikost této vlastnosti je maximální velikost celé číslo. Proto je vlastnost nastavena na hodnotu menší jako pojistku, chcete-li omezit množství dat, která aplikace bude přijímat jako odpověď z webové služby.

### <a name="retrieving-data"></a>Načítání dat

`HttpClient.GetAsync` Metoda se používá k odeslání požadavek GET na určený identifikátor URI webové služby a poté zobrazí odpověď z webové služby, jak je znázorněno v následujícím příkladu kódu:

```csharp
public async Task<List<TodoItem>> RefreshDataAsync ()
{
  ...
  // RestUrl = http://developer.xamarin.com:8081/api/todoitems/
  var uri = new Uri (string.Format (Constants.RestUrl, string.Empty));
  ...
  var response = await client.GetAsync (uri);
  if (response.IsSuccessStatusCode) {
      var content = await response.Content.ReadAsStringAsync ();
      Items = JsonConvert.DeserializeObject <List<TodoItem>> (content);
  }
  ...
}
```

Službu REST odešle stavový kód protokolu HTTP v `HttpResponseMessage.IsSuccessStatusCode` vlastnost označující, zda požadavek HTTP byla úspěšná nebo neúspěšná. Pro tuto operaci REST odešle služba stavový kód HTTP 200 (OK) v odpovědi, které označuje, že požadavek byl úspěšný a zda jsou požadované informace v odpovědi.

Pokud se operace protokolu HTTP byla úspěšná, přečtěte si obsah odpovědi, pro zobrazení. `HttpResponseMessage.Content` Vlastnost představuje obsah odpovědi HTTP a `HttpContent.ReadAsStringAsync` metoda asynchronně zapíše obsah HTTP na řetězec. Tento obsah je poté převést z JSON na `List` z `TodoItem` instance.

### <a name="creating-data"></a>Vytváření dat

`HttpClient.PostAsync` Metoda se používá k odeslání požadavku POST k webové službě určeného identifikátor URI a potom získat odpovědi z webové služby, jak je znázorněno v následujícím příkladu kódu:

```csharp
public async Task SaveTodoItemAsync (TodoItem item, bool isNewItem = false)
{
  // RestUrl = http://developer.xamarin.com:8081/api/todoitems/
  var uri = new Uri (string.Format (Constants.RestUrl, string.Empty));

  ...
  var json = JsonConvert.SerializeObject (item);
  var content = new StringContent (json, Encoding.UTF8, "application/json");

  HttpResponseMessage response = null;
  if (isNewItem) {
    response = await client.PostAsync (uri, content);
  }
  ...

  if (response.IsSuccessStatusCode) {
    Debug.WriteLine (@"             TodoItem successfully saved.");

  }
  ...
}
```

`TodoItem` Instance jsou převedeny na datové části JSON pro odesílání k webové službě. Tato datová část se pak vloží v textu obsahu HTTP, který se k webové službě odesílat, předtím, než je žádosti s `PostAsync` metoda.

Službu REST odešle stavový kód protokolu HTTP v `HttpResponseMessage.IsSuccessStatusCode` vlastnost označující, zda požadavek HTTP byla úspěšná nebo neúspěšná. Běžné odpovědi pro tuto operaci je:

- **201 (VYTVOŘENO)** – žádost výsledkem nový prostředek vytváří předtím, než byl odeslán do odpovědi.
- **400 (Chybný požadavek)** – požadavek nebyl pochopen serverem.
- **409 (konflikt)** – žádost nemohla být provedena kvůli konfliktu na serveru.

### <a name="updating-data"></a>Aktualizace dat

`HttpClient.PutAsync` Metoda se používá k odeslání požadavku PUT do určeného identifikátor URI webové služby a poté zobrazí odpověď z webové služby, jak je znázorněno v následujícím příkladu kódu:

```csharp
public async Task SaveTodoItemAsync (TodoItem item, bool isNewItem = false)
{
  ...
  response = await client.PutAsync (uri, content);
  ...
}
```
Operaci `PutAsync` metoda je stejná jako `PostAsync` metoda, která se používá pro vytváření dat ve webové službě. Však možné odpovědí odeslaných z webové služby se liší.

Službu REST odešle stavový kód protokolu HTTP v `HttpResponseMessage.IsSuccessStatusCode` vlastnost označující, zda požadavek HTTP byla úspěšná nebo neúspěšná. Běžné odpovědi pro tuto operaci je:

- **204 (ne obsahu)** – úspěšně zpracoval požadavek a odpověď je úmyslně prázdné.
- **400 (Chybný požadavek)** – požadavek nebyl pochopen serverem.
- **404 (není NALEZENA)** – požadovaný prostředek neexistuje na serveru.

### <a name="deleting-data"></a>Odstraňování dat

`HttpClient.DeleteAsync` Metoda se používá k odeslání požadavku na odstranění k webové službě určeného identifikátor URI a poté zobrazí odpověď z webové služby, jak je znázorněno v následujícím příkladu kódu:

```csharp
public async Task DeleteTodoItemAsync (string id)
{
  // RestUrl = http://developer.xamarin.com:8081/api/todoitems/{0}
  var uri = new Uri (string.Format (Constants.RestUrl, id));
  ...
  var response = await client.DeleteAsync (uri);
  if (response.IsSuccessStatusCode) {
    Debug.WriteLine (@"             TodoItem successfully deleted.");
  }
  ...
}
```

Službu REST odešle stavový kód protokolu HTTP v `HttpResponseMessage.IsSuccessStatusCode` vlastnost označující, zda požadavek HTTP byla úspěšná nebo neúspěšná. Běžné odpovědi pro tuto operaci je:

- **204 (ne obsahu)** – úspěšně zpracoval požadavek a odpověď je úmyslně prázdné.
- **400 (Chybný požadavek)** – požadavek nebyl pochopen serverem.
- **404 (není NALEZENA)** – požadovaný prostředek neexistuje na serveru.

## <a name="summary"></a>Souhrn

Tento článek zkontrolován využívání RESTful webová služba z aplikace platformě Xamarin.Forms pomocí `HttpClient` třídy. Jednoduchost REST pomohlo, vytvořte ji primární metodu pro přístup k webovým službám v mobilních aplikacích.


## <a name="related-links"></a>Související odkazy

- [Vytváření back-endových služeb pro nativní mobilní aplikace](/aspnet/core/mobile/native-mobile-backend/)
- [TodoREST (ukázka)](https://developer.xamarin.com/samples/xamarin-forms/WebServices/TodoREST/)
- [HttpClient](https://msdn.microsoft.com/library/system.net.http.httpclient(v=vs.110).aspx)
