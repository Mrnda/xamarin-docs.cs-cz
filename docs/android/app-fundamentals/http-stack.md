---
title: "Zásobník HttpClient a SSL/TLS implementace selektor pro Android"
description: "Selektory zásobník HttpClient a SSL/TLS implementace určit implementace HttpClient a SSL/TLS, který bude používat aplikace pro Xamarin.Android."
ms.topic: article
ms.prod: xamarin
ms.assetid: D7ABAFAB-5CA2-443D-B902-2C7F3AD69CE2
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/16/2018
ms.openlocfilehash: bcb6f033c7fad76a17a7a5aa82f48a76b1ae501d
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 02/27/2018
---
# <a name="httpclient-stack-and-ssltls-implementation-selector-for-android"></a>Zásobník HttpClient a SSL/TLS implementace selektor pro Android

_Selektory zásobník HttpClient a SSL/TLS implementace určit implementace HttpClient a SSL/TLS, který bude používat aplikace pro Xamarin.Android._

## <a name="overview"></a>Přehled

Xamarin.Android poskytuje dva seznamem, které bude řídit nastavení TLS pro aplikace pro Android. Jednoho pole se seznamem zjistit, jaké `HttpMessageHandler` budou používat při vytváření instance `HttpClient` objektu, zatímco druhý identifikuje, které implementaci TLS, se použije v webových požadavků.

> [!NOTE]
> **Poznámka:** projekty musí odkazovat **System.Net.Http** sestavení.

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Nastavení pro HttpClient zásobníku se nacházejí v projektu možnosti projektu Xamarin.Android. Klikněte na **Android možnosti** a pak klikněte na **pokročilé možnosti** tlačítko. Bude se zobrazovat **pokročilé možnosti Android** dialogu, který má dvě pole se seznamem, jeden pro implementace HttpClient a jeden pro implementaci protokolu SSL/TLS:


[ ![Android možnosti sady Visual Studio](http-stack-images/tls07-vs-sml.png)](http-stack-images/tls07-vs.png)


# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

Nastavení pro HttpClient zásobníku se nacházejí v projektu možnosti pro projekt Xamarin.Android. Klikněte na **sestavení > Android sestavení** nastavení a klikněte na **Obecné** karta:

[ ![Visual Studio pro Android možnosti Mac](http-stack-images/tls07-xs-sml.png)](http-stack-images/tls07-xs.png)


-----

## <a name="httpclient-stack-selector"></a>Výběr zobrazení HttpClient

Tato možnost projektu prvky, které `HttpMessageHandler` implementace se použije vždy, když `HttpClient` dojde k vytvoření objektu. Ve výchozím nastavení, toto je spravovaný `HttpClientHandler`.

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[ ![Android pole se seznamem implementace HttpClient v sadě Visual Studio](http-stack-images/tls04-vs-sml.png)](http-stack-images/tls04-vs.png) 

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

![Android pole se seznamem implementace HttpClient v sadě Visual Studio pro Mac](http-stack-images/tls04-xs.png )

-----

### <a name="managed-httpclienthandler"></a>Spravované (HttpClientHandler)

Spravované obslužné rutiny je plně spravovaná obslužná rutina HttpClient dodané s předchozími verzemi Xamarin.Android.

#### <a name="pros"></a>Odborníci na:

- Je nejvíce kompatibilní (funkce) s MS .NET a starší verze Xamarin.

#### <a name="cons"></a>Cons:

- Ji není plně integrovaná s operačním systémem (např. omezeno na TLS 1.0).
- Je obvykle výrazně zpomalit (např. šifrování) než nativní rozhraní API.
- To vyžaduje více spravovaného kódu, vytváření větší aplikací.

### <a name="androidclienthandler"></a>AndroidClientHandler

AndroidClientHandler je novou obslužnou rutinu, která deleguje do nativního kódu Java/OS místo implementace všechno, co ve spravovaném kódu.

#### <a name="pros"></a>Odborníci na:

- Pomocí nativních rozhraní API pro lepší výkon a menší velikost spustitelný soubor.
- Podpora pro nejnovějších standardů, např. PROTOKOL TLS 1.2.

#### <a name="cons"></a>Cons:

- Vyžaduje Android 5.0 nebo novější.
- Některé funkce nebo možnosti HttpClient nejsou k dispozici.


### <a name="programatically-using-androidclienthandler"></a>Programově pomocí `AndroidClientHandler`

`Xamarin.Android.Net.AndroidClientHandler` Je `HttpMessageHandler` implementace speciálně pro Xamarin.Android. Instance této třídy použije nativního `java.net.URLConnection` implementace pro všechna připojení HTTP. To zajistí teoreticky zvýšení výkonu protokolu HTTP a menší velikosti APK.

Tento fragment kódu je příklad toho, jak explicitně pro jednu instanci `HttpClient` třídy:

```csharp
// Android 5.0 or higher, Xamarin.Android 6.1 or higher
HttpClient client = new HttpClient(new Xamarin.Android.Net.AndroidClientHandler ());
```

> [!NOTE]
>  **Poznámka:**: základní zařízení s Androidem musí podporovat protokol TLS 1.2 (tj. Android 5.0 a novější)


## <a name="ssltls-implementation-build-option"></a>Možnost sestavení implementace SSL/TLS.

Tato možnost projektu řídí, jaké základní knihovna TLS budou používat všechny webové žádosti obě `HttpClient` a `WebRequest`. Ve výchozím nastavení je vybrané TLS 1.2:

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[ ![Pole se seznamem implementaci protokolu TLS/SSL v sadě Visual Studio](http-stack-images/tls06-vs.png)](http-stack-images/tls05-vs.png)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

[ ![Pole se seznamem implementaci protokolu TLS/SSL v sadě Visual Studio pro Mac](http-stack-images/tls06-xs.png)](http-stack-images/tls05-xs.png)

-----

Příklad:

```csharp
var client = new HttpClient();
```

Pokud implementace HttpClient byla nastavena na **spravované** a implementaci TLS byla nastavena na **nativní TLS 1.2 +**, pak se `client` objekt automaticky používat spravovanou `HttpClientHandler` a TLS 1.2 (poskytované knihovně BoringSSL) pro jeho požadavky HTTP.

Ale pokud **implementace HttpClient** je nastaven na `AndroidHttpClient`, pak všechny `HttpClient` objekty budou používat základní třída jazyka Java `java.net.URLConnection` a nebudou mít vliv **implementaci protokolu TLS/SSL** hodnotu. `WebRequest` objekty by v knihovně BoringSSL.

## <a name="other-ways-to-control-ssltls-configuration"></a>Další způsoby, jak řídit konfiguraci protokolu SSL/TLS

Existují tři způsoby, že aplikace pro Xamarin.Android můžete řídit nastavení TLS:

1. Vyberte knihovnu HttpClient podrobné a výchozí TLS v možnostech projektu.
2. Programově pomocí `Xamarin.Android.Net.AndroidClientHandler`.
3. Deklarování proměnných prostředí (volitelné).

Z těchto tří možností doporučený přístup je použití možností projektu Xamarin.Android deklarovat výchozí `HttpMessageHandler` a TLS pro celou aplikaci. Poté, v případě potřeby prostřednictvím kódu programu vytvořit instanci `Xamarin.Android.Net.AndroidClientHandler` objekty.
Tyto možnosti jsou popsané výše.

Třetí možnost &ndash; použití proměnných prostředí &ndash; je popsáno níže.

### <a name="declare-environment-variables"></a>Deklarujte proměnné prostředí

Existují dvě proměnné prostředí, které se vztahují k použití protokolu TLS v Xamarin.Android:

-   `XA_HTTP_CLIENT_HANDLER_TYPE` &ndash; Tato proměnná prostředí deklaruje výchozí `HttpMessageHandler` , aplikace bude používat. Příklad:

    ```csharp
    XA_HTTP_CLIENT_HANDLER_TYPE=Xamarin.Android.Net.AndroidClientHandler
    ```

-   `XA_TLS_PROVIDER` &ndash; Tato proměnná prostředí se deklarovat, který TLS knihovny se použije, buď `btls`, `legacy`, nebo `default` (což je stejný jako vynechání Tato proměnná):

    ```csharp
    XA_TLS_PROVIDER=btls
    ```

Tato proměnná prostředí je nastavená tak, že přidáte _prostředí soubor_ do projektu. Soubor prostředí je soubor ve formátu prostého textu ve formátu Unix pomocí akce sestavení **AndroidEnvironment**:

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

![Snímek obrazovky AndroidEnvironment akce sestavení v sadě Visual Studio.](http-stack-images/tls03-vs.png)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

![Snímek obrazovky AndroidEnvironment sestavení akce v sadě Visual Studio for Mac.](http-stack-images/tls03-xs.png)

-----

Podrobnosti najdete [Xamarin.Android prostředí](~/android/deploy-test/environment.md) Průvodce pro další informace o proměnných prostředí a Xamarin.Android.


## <a name="related-links"></a>Související odkazy

- [Protokol TLS (Transport Layer Security)](~/cross-platform/app-fundamentals/transport-layer-security.md)
