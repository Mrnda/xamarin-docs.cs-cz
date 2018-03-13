---
title: "Zásobník HttpClient a SSL/TLS implementace selektor pro iOS/systému macOS"
description: "Zásobník HttpClient a SSL/TLS implementace selektor určuje implementace HttpClient a SSL/TLS, která se použije v aplikaci Xamarin iOS, tvOS nebo systému macOS."
ms.topic: article
ms.prod: xamarin
ms.assetid: 12101297-BB04-4410-85F0-A0D41B7E6591
ms.technology: xamarin-cross-platform
author: asb3993
ms.author: amburns
ms.date: 06/12/2017
ms.openlocfilehash: eff096b1dca15b9b11038a599987f632bca2352f
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 03/09/2018
---
# <a name="httpclient-stack-and-ssltls-implementation-selector-for-iosmacos"></a>Zásobník HttpClient a SSL/TLS implementace selektor pro iOS/systému macOS

## <a name="httpclient-stack-selector"></a>Výběr zobrazení HttpClient

K dispozici pro Xamarin.iOS, Xamarin.tvOS a Xamarin.Mac: Tato volba určuje, které `HttpClient` implementace používat. Výchozí hodnota je stále HttpClient, který používá technologii `HttpWebRequest`, zatímco lze nyní volitelně přepnout implementace, která používá iOS, tvOS nebo systému macOS nativní přenosy (`NSUrlSession` nebo `CFNetwork` v závislosti na operačním systémem). Vzhůru je menší binární soubory a rychlejší stahování, nevýhodou je, že vyžaduje smyčky událostí, aby byl spuštěn pro spouštění asynchronních operací.

Projekty musí odkazovat **System.Net.Http** sestavení.

<a name="Selecting-a-HttpClient-Stack" />

### <a name="selecting-a-httpclient-stack"></a>Výběr HttpClient zásobníku

Chcete-li upravit HttpClient používá aplikaci:

1. Dvakrát klikněte **název projektu** v **Průzkumníku řešení** otevřete dialogové okno Možnosti projektu.
2. Přepnout **sestavení** nastavení pro svůj projekt (například **iOS sestavení** pro aplikace Xamarin.iOS).
3. Z **implementace HttpClient** rozevíracího seznamu, vyberte HttpClient zadejte jako jednu z následujících: **spravované**, **CFNetwork** nebo **NSUrlSession**.

[![Implementace HttpClient vybírat spravované, CFNetwork nebo NSUrlSession](http-stack-images/http-xs-sml.png)](http-stack-images/http-xs.png#lightbox)

<a name="Managed" />

### <a name="managed-default"></a>Spravované (výchozí)

Spravované obslužné rutiny je plně spravovaná HttpClient obslužná rutina, která je dodávána se předchozí verze nástroje Xamarin.

#### <a name="pros"></a>Odborníci na:

 - Obsahuje funkci nejvíce kompatibilní nastavit pomocí rozhraní Microsoft .NET a starší verze Xamarin.

#### <a name="cons"></a>Cons:

 - Ji není plně integrována operační systémy Apple a je omezený na TLS 1.0.
 - Je obvykle výrazně zpomalit na třeba šifrování než nativních rozhraní API.
 - To vyžaduje více spravovaného kódu, proto vytvoření distribuovatelného větší aplikace.

<a name="CFNetwork" />

### <a name="cfnetwork"></a>CFNetwork

Obslužné rutiny na základě CFNetwork je založena na nativního `CFNetwork` framework k dispozici v iOS 6 nebo novější.

#### <a name="pros"></a>Odborníci na:

 - Používá nativních rozhraní API pro lepší výkon a menší velikost spustitelný soubor.
 - Podporuje pro novější standardy, jako je protokol TLS 1.2.

#### <a name="cons"></a>Cons:

 - Vyžaduje iOS 6 nebo novější.
 - Není k dispozici v watchOS.
 - Některé funkce nebo možnosti HttpClient nejsou k dispozici.

<a name="NSUrlSession" />

### <a name="nsurlsession"></a>NSUrlSession

Obslužné rutiny na základě NSURLSession je založena na nativního `NSURLSession` framework k dispozici v iOS 7 a novější.

#### <a name="pros"></a>Odborníci na:

 - Používá nativních rozhraní API pro lepší výkon a menší velikost spustitelný soubor.
 - Zajišťuje podporu pro nejnovější standardy, jako je protokol TLS 1.2.

#### <a name="cons"></a>Cons:

 - Vyžaduje iOS 7 nebo novější.
 - Některé funkce nebo možnosti HttpClient nejsou k dispozici.

### <a name="programmatically-setting-the-httpmessagehandler"></a>Objekt HttpMessageHandler prostřednictvím kódu programu nastavení

Kromě konfigurace projektu celou uvedené výše, můžete také vytvořit instanci `HttpClient` a vložit požadovanou `HttpMessageHandler` pomocí konstruktoru, jak je předvedeno v tyto fragmenty kódu:

```csharp
// This will use the default message handler for the application; as
// set in the Project Options for the project.
HttpClient client = new HttpClient();

// This will create an HttpClient that explicitly uses the CFNetworkHandler
HttpClient client = new HttpClient(new CFNetworkHandler());

// This will create an HttpClient that explicitly uses NSUrlSessionHandler
HttpClient client = new HttpClient(new NSUrlSessionHandler());
```

Díky tomu je možné použít jiné `HttpMessageHandler` z co je v deklarována **možnosti projektu** dialogové okno.

<a name="New-SSL-TLS-implementation-build-option" />
<a name="Selecting-a-SSL-TLS-implementation" />
<a name="Apple-TLS" />

## <a name="ssltls-implementation-build"></a>Implementace protokolu SSL/TLS sestavení

Protokol SSL (Secure Socket Layer) a jeho následníka TLS (Transport Layer Security) poskytuje podporu pro protokol HTTP a jiných síťových připojení prostřednictvím `System.Net.Security.SslStream`. Xamarin.iOS, Xamarin.tvOS nebo na Xamarin.Mac `System.Net.Security.SslStream` implementace zavolá společnosti Apple nativní implementaci protokolu SSL/TLS místo použití spravovaných implementace poskytované Mono. Nativní implementaci společnosti Apple podporuje TLS 1.2.

<a name="Mono" />
> [!WARNING]
> **Mono nebo spravované** TLS zprostředkovatele je omezen na SSL v3 a TLS v1. Tento zprostředkovatel TLS je zastaralá a již není k dispozici pro aplikace Xamarin.iOS. 

<a name="App-Transport-Security" />

## <a name="app-transport-security"></a>Zabezpečení přenosu aplikace

Společnosti Apple _zabezpečení přenosu aplikace_ (ATS) vynucuje zabezpečené připojení mezi prostředků z Internetu (třeba server back-end aplikace) a aplikací. ATS zajišťuje shodu všechny internetové komunikace s zabezpečené připojení osvědčených postupů, a zabrání náhodného zpřístupnění citlivých informací, buď přímo prostřednictvím aplikace nebo knihovny, které je využívají.

Protože ATS je povolené ve výchozím nastavení v aplikacích pro iOS 9, tvOS 9 a OS X 10.11 (El Capitan) a novější, všechna připojení pomocí `NSUrlConnection`, `CFUrl` nebo `NSUrlSession` budou platit ATS požadavky na zabezpečení. Pokud vaše připojení těchto požadavků nesplňuje, budou se nezdaří s výjimkou.

Podle vašeho výběru zásobník HttpClient a SSL/TLS implementace, můžete měnit pomocí aplikace pro práci s ATS správně.

Chcete-li získat další informace o ATS, najdete v tématu naše [Průvodce zabezpečením přenosu aplikace](~/ios/app-fundamentals/ats.md).

## <a name="known-issues"></a>Známé problémy

Tato část se bude zabývat známé problémy s podporou protokolu TLS v Xamarin.iOS.

### <a name="project-failed-to-load-with-error-requested-value-appletls-wasnt-found"></a>Projektu načtení nezdařilo s chybou "Požadovaná hodnota AppleTLS nebyl nalezen."

Xamarin.iOS 9.8 zavedená některé nové nastavení, které jsou obsažené **.csproj** soubor pro aplikace pro Xamarin.iOS. Tyto změny může způsobit problémy při otevření projektu se staršími verzemi Xamarin.iOS. Na následujícím snímku obrazovky je příklad chybovou zprávu, která nemusí být zobrazeny v tomto scénáři:

![Snímek obrazovky při pokusu o načtení projektu došlo k chybě požadovaná hodnota starší nebyl nalezen](http-stack-images/tlserror-xs.png)

Tato chyba je způsobená zavedení `MtouchTlsProvider` nastavení do souboru projektu v Xamarin.iOS 9.8. Pokud není možné aktualizovat Xamarin.iOS 9.8 (nebo vyšší), práce je přibližně ručně upravit **.csproj** souboru aplikace, odeberte `MtouchTlsprovider` elementu a potom uložte soubor změněné projektu.

Následující fragment kódu je příkladem co `MtouchTlsProvider` nastavení vypadat jako uvnitř **.csproj** souboru:

```xml
<MtouchTlsProvider>Default</MtouchTlsProvider>
```

## <a name="related-links"></a>Související odkazy

- [Protokol TLS (Transport Layer Security)](~/cross-platform/app-fundamentals/transport-layer-security.md)
- [Zabezpečení přenosu aplikací](~/ios/app-fundamentals/ats.md)
