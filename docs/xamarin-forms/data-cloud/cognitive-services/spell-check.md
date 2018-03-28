---
title: Kontrola pravopisu pomocí kontrolu pravopisu Bing rozhraní API
description: Kontrola pravopisu Bing provede kontrolu pro text, kontextové pravopisu poskytování vložené návrhy opravy. Tento článek vysvětluje, jak Opravte pravopis chyb v aplikaci Xamarin.Forms pomocí Bingu pravopisu zkontrolujte REST API.
ms.topic: article
ms.prod: xamarin
ms.assetid: B40EB103-FDC0-45C6-9940-FB4ACDC2F4F9
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 02/08/2017
ms.openlocfilehash: 420eea4622d9c90c3587899fb24e707524990b19
ms.sourcegitcommit: 20ca85ff638dbe3a85e601b5eb09b2f95bda2807
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 03/28/2018
---
# <a name="spell-checking-using-the-bing-spell-check-api"></a>Kontrola pravopisu pomocí kontrolu pravopisu Bing rozhraní API

_Kontrola pravopisu Bing provede kontrolu pro text, kontextové pravopisu poskytování vložené návrhy opravy. Tento článek vysvětluje, jak Opravte pravopis chyb v aplikaci Xamarin.Forms pomocí Bingu pravopisu zkontrolujte REST API._

## <a name="overview"></a>Přehled

Bing pravopisu zkontrolujte REST API má dvou režimů a režimu se musí zadat při vytváření požadavku na rozhraní API:

- `Spell` Krátký textový (až 9 slova) opraví beze změn velká a malá písmena.
- `Proof` vyřeší dlouhý text, obsahuje opravy velká a malá písmena a základní interpunkce a potlačí agresivní opravy.

Klíč rozhraní API musí být získána používat API kontrola pravopisu Bing. To lze získat v [zkuste kognitivní služby](https://azure.microsoft.com/try/cognitive-services/)

Seznam jazyků podporovaných rozhraním API kontrola pravopisu Bing najdete v tématu [podporované jazyky](/azure/cognitive-services/bing-spell-check/bing-spell-check-supported-languages/). Další informace o API kontrola pravopisu Bing najdete v tématu [dokumentace kontrola pravopisu Bing](/azure/cognitive-services/bing-spell-check/).

## <a name="authentication"></a>Ověřování

Každý požadavek do služby Bing pravopisu zkontrolujte API vyžaduje klíč rozhraní API, která musí být zadán jako hodnotu `Ocp-Apim-Subscription-Key` záhlaví. Následující příklad kódu ukazuje, jak přidat klíč rozhraní API do `Ocp-Apim-Subscription-Key` hlavičky požadavku:

```csharp
public BingSpellCheckService()
{
    httpClient = new HttpClient();
    httpClient.DefaultRequestHeaders.Add("Ocp-Apim-Subscription-Key", Constants.BingSpellCheckApiKey);
}
```

Chyba odpovědi 401 způsobí selhání předat platný klíč rozhraní API API kontrola pravopisu Bing.

## <a name="performing-spell-checking"></a>Provádění kontrola pravopisu

Kontrola pravopisu lze dosáhnout tím, že požadavek GET nebo POST na `SpellCheck` rozhraní API v `https://api.cognitive.microsoft.com/bing/v7.0/SpellCheck`. Při provádění požadavek GET, text, který má být zaškrtnuto pravopisu odeslán jako parametr dotazu. Při provádění požadavku POST, se odešlou text, který má být zaškrtnuto pravopisu v textu žádosti. Požadavky GET jsou omezeny na 1 500 znaků z důvodu omezení délky řetězce parametr na dotaz kontrolu pravopisu. Požadavky POST by měl obvykle se proto pokud krátké řetězce probíhá pravopisu zaškrtnutí.

V ukázkové aplikaci `SpellCheckTextAsync` metoda vyvolá proces kontroly pravopisu:

```csharp
public async Task<SpellCheckResult> SpellCheckTextAsync(string text)
{
    string requestUri = GenerateRequestUri(Constants.BingSpellCheckEndpoint, text, SpellCheckMode.Spell);
    var response = await SendRequestAsync(requestUri);
    var spellCheckResults = JsonConvert.DeserializeObject<SpellCheckResult>(response);
    return spellCheckResults;
}
```

`SpellCheckTextAsync` Metoda vygeneruje URI požadavku a pak odešle požadavek na `SpellCheck` rozhraní API, které vrátí odpověď JSON obsahující výsledek. Odpověď JSON je deserializovat s výsledkem nevrátila volání metody pro zobrazení.

### <a name="configuring-spell-checking"></a>Konfigurace kontrola pravopisu

Proces kontroly pravopisu lze nakonfigurovat tak, že zadáte parametry dotazu HTTP:

```csharp
string GenerateRequestUri(string spellCheckEndpoint, string text, SpellCheckMode mode)
{
  string requestUri = spellCheckEndpoint;
  requestUri += string.Format("?text={0}", text);                         // text to spell check
  requestUri += string.Format("&mode={0}", mode.ToString().ToLower());    // spellcheck mode - proof or spell
  return requestUri;
}
```

Tato metoda nastaví text, který má být pravopisu zaškrtnutí a režim kontrolu pravopisu.

Další informace o Bing pravopisu zkontrolujte REST API najdete v tématu [referenční dokumentace rozhraní API kontrola pravopisu v7](/rest/api/cognitiveservices/bing-spell-check-api-v7-reference/).

### <a name="sending-the-request"></a>Odesílání požadavku

`SendRequestAsync` Metoda provede požadavek GET Bing pravopisu zkontrolujte REST API a vrátí odpověď:

```csharp
async Task<string> SendRequestAsync(string url)
{
    var response = await httpClient.GetAsync(url);
    return await response.Content.ReadAsStringAsync();
}
```

Tato metoda vytváří požadavek GET přidáváním klíč rozhraní API jako hodnotu `Ocp-Apim-Subscription-Key` záhlaví. Potom posílá požadavek GET `SpellCheck` rozhraní API pomocí adresu URL požadavku zadání text k převodu a režimu kontrolu pravopisu. Odpověď je pak číst a vrátí volání metody.

`SpellCheck` Rozhraní API bude odesílat stavový kód HTTP 200 (OK) v odpovědi, za předpokladu, že žádost je platná, což naznačuje, že požadavek byl úspěšný a že požadované informace je v odpovědi. Seznam objektů odpovědi najdete v tématu [odpovědi objekty](/rest/api/cognitiveservices/bing-spell-check-api-v7-reference#response-objects).

### <a name="processing-the-response"></a>Zpracování odpovědi

Rozhraní API odpověď se vrátí ve formátu JSON. Následující data JSON zobrazí zprávu odpovědi pro chybný text `Go shappin tommorow`:

```json
{  
   "_type":"SpellCheck",
   "flaggedTokens":[  
      {  
         "offset":3,
         "token":"shappin",
         "type":"UnknownToken",
         "suggestions":[  
            {  
               "suggestion":"shopping",
               "score":1
            }
         ]
      },
      {  
         "offset":11,
         "token":"tommorow",
         "type":"UnknownToken",
         "suggestions":[  
            {  
               "suggestion":"tomorrow",
               "score":1
            }
         ]
      }
   ],
   "correctionType":"High"
}
```

`flaggedTokens` Pole obsahuje pole slova v textu, které byly označení není zadán správně nebo jsou gramaticky nesprávný. Pole bude prázdný, pokud nebyly nalezeny žádné pravopisné nebo gramatické chyby. Značky v rámci pole jsou:

- `offset` – od nuly posun od začátku textového řetězce na slovo, které by byla příznakem.
- `token` – aplikace word v textovém řetězci, který není zadána správně nebo není gramaticky správné.
- `type` – Typ chyby, která způsobila slovo příznakem. Existují dvě možné hodnoty – `RepeatedToken` a `UnknownToken`.
- `suggestions` – pole slova, která bude Opravte pravopis a gramatiku chybu. Pole se skládá z `suggestion` a `score`, která informuje o úrovni jistota, že je správný navrhovanou opravu.

V ukázkové aplikaci se deserializovat JSON odpovědi do `SpellCheckResult` instance s výsledkem nevrátila volání metody pro zobrazení. Následující příklad kódu ukazuje jak `SpellCheckResult` instanci, jsou zpracovávány pro zobrazení:

```csharp
var spellCheckResult = await bingSpellCheckService.SpellCheckTextAsync(TodoItem.Name);
foreach (var flaggedToken in spellCheckResult.FlaggedTokens)
{
  TodoItem.Name = TodoItem.Name.Replace(flaggedToken.Token, flaggedToken.Suggestions.FirstOrDefault().Suggestion);
}
```

Tento kód projde `FlaggedTokens` kolekce a nahradí všechny překlepu nebo gramaticky nesprávná slova ve zdrojovém textu s prvním návrhu. Na následujících snímcích obrazovky zobrazit před a po kontrolu pravopisu:

![](spell-check-images/before-spell-check.png "Před kontrola pravopisu")

![](spell-check-images/after-spell-check.png "Po kontrolu pravopisu")

## <a name="summary"></a>Souhrn

Tento článek vysvětleny postupy Opravte pravopis chyb v aplikaci Xamarin.Forms pomocí Bingu pravopisu zkontrolujte REST API. Kontrola pravopisu Bing provede kontrolu pro text, kontextové pravopisu poskytování vložené návrhy opravy.

## <a name="related-links"></a>Související odkazy

- [Bing pravopisu naleznete v dokumentaci](/azure/cognitive-services/bing-spell-check/)
- [Využívání RESTful webová služba](~/xamarin-forms/data-cloud/consuming/rest.md)
- [TODO kognitivní služby (ukázka)](https://developer.xamarin.com/samples/xamarin-forms/WebServices/TodoCognitiveServices/)
- [API kontrola pravopisu Bing v7 – referenční informace](/rest/api/cognitiveservices/bing-spell-check-api-v7-reference/)
