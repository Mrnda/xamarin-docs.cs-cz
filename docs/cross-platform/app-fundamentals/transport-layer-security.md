---
title: Transport Layer Security (TLS)
description: Povolení protokolu TLS 1.2 pro projekty Xamarin pro Android, iOS a Mac
ms.prod: xamarin
ms.assetid: 399F71C6-16A4-4ABC-B30D-AF17D066A5FA
ms.technology: xamarin-cross-platform
author: asb3993
ms.author: amburns
ms.date: 10/10/2017
ms.openlocfilehash: 8b2d0288248f2468e6976ad4f7c46255690116c0
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/04/2018
---
# <a name="transport-layer-security-tls"></a>Transport Layer Security (TLS)

_Povolení protokolu TLS 1.2 pro projekty Xamarin pro Android, iOS a Mac_

Pomocí nejnovější verze [ _Transport Layer Security_ (TLS)](https://en.wikipedia.org/wiki/Transport_Layer_Security) je důležité zajistit síťové komunikace mezi aplikací jsou bezpečné.

> [!NOTE]
> Xamarin uvolní od [února 2017](https://releases.xamarin.com/stable-release-cycle-9/) TLS 1.2 ve výchozím nastavení používá nové projekty.

Podpora protokolu TLS 1.2 je nyní k dispozici v:

* Mono 4.8 (zahrnuje [podpora protokolu TLS 1.2](http://www.mono-project.com/docs/about-mono/releases/4.8.0/#tls-12-support))
* Xamarin.iOS
* Xamarin.Mac
* Xamarin.Android (vyžaduje Android 5.0 nebo novější)

Projekty musí odkazovat **System.Net.Http** sestavení. 

## <a name="updating-to-tls-12"></a>Aktualizace TLS 1.2

Tato část popisuje některé možnosti konfigurace sítě v projektech Xamarin, abyste mohli aktualizovat vaše _existující_ aplikace využívat výhod bezpečnější protokol.


# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Tato nastavení najdete v **možnosti projektu > Android možnosti** a kliknete na **Upřesnit** tlačítko: 

[![Konfigurace HttpClient a TLS v sadě Visual Studio](transport-layer-security-images/properties-vs-sml.png)](transport-layer-security-images/properties-vs.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)
Tato nastavení najdete v **vlastnosti projektu > sestavení možnosti > Upřesnit** karty:

[![Konfigurace HttpClient a TLS v Xamarin studiu a Visual Studio pro Mac](transport-layer-security-images/properties-xs-sml.png)](transport-layer-security-images/properties-xs.png#lightbox)

-----


### <a name="httpclient-implementation"></a>Implementace HttpClient

Vývojáři Xamarin byly vždy možné použít nativní síťové třídy v jejich kódu, ale je také možnost, která určuje, které síťový zásobník používá `HttpClient` třídy. To poskytuje známé .NET API, který má vyšší rychlosti a zabezpečení výhod nativní platformy.

Možnosti jsou:

- **Spravované zásobníku** – funkce poskytované Mono sítě, nebo
- **Nativní zásobníku** – různých síťových rozhraní API poskytovaných základní platformy (Android, iOS nebo systému macOS).

Spravované zásobníku poskytuje nejvyšší úroveň kompatibility s existující kód .NET, ale může být pomalejší a mít za následek větší velikost spustitelný soubor.

Nativní možnosti může být rychlejší a mít lepší zabezpečení (včetně protokolu TLS 1.2), ale nemusí poskytnout všechny funkce a možnosti `HttpClient` třídy.


### <a name="ssltls-implementation"></a>Implementace protokolu SSL/TLS

Možnosti projektu vám také umožní vybrat které implementace SSL/TLS na podporu:

- **Mono nebo spravované** – v systému Android protokolu TLS 1.1, TLS 1.0 na iOS a systému macOS.
- **Nativní** – protokol TLS 1.2 na Android, iOS a systému macOS.

Nové projekty Xamarin výchozí pro nativní implementaci, který podporuje protokol TLS 1.2, (který se doporučuje pro všechny projekty), ale můžete přepnout zpět do spravovaného kódu Pokud požadované z důvodu kompatibility.

> [!IMPORTANT]
> **Mono nebo spravované** možnost odeberou v [budoucí verze](https://developer.xamarin.com/releases/ios/xamarin.ios_10/xamarin.ios_10.8/).
>
> Nativní možnost se doporučuje.

## <a name="platform-specific-details"></a>Podrobnosti o specifických pro platformy

Výše uvedené souhrn vysvětluje nastavení projektu pro implementace HttpClient a SSL/TLS v projektech Xamarin. Implementace HttpClient lze také nastavit dynamicky v kódu a v systému iOS existují dvě možnosti nativní lze vybírat.

- [**Android**](~/android/app-fundamentals/http-stack.md)
- [**iOS a Mac**](~/cross-platform/macios/http-stack.md)


## <a name="summary"></a>Souhrn

Aplikace by měly používat zabezpečení TLS (Transport Layer) 1.2, pokud je to možné.
Nové aplikace se teď výchozí do této konfigurace, ale budete muset aktualizovat nastavení v existujících aplikací podle pokynů v tomto článku.

## <a name="related-links"></a>Související odkazy

- [Zabezpečení přenosu aplikací](~/ios/app-fundamentals/ats.md)
- [Xamarin.Android Environment](~/android/deploy-test/environment.md)
- [Xamarin cyklus 9 (únor 2017)](https://releases.xamarin.com/stable-release-cycle-9/)
- [Protokol TLS (Wikipedia)](https://en.wikipedia.org/wiki/Transport_Layer_Security)
- [Poznámky k verzi mono 4.8 – podpora TLS 1.2](http://www.mono-project.com/docs/about-monohttps://developer.xamarin.com/releases/4.8.0/#tls-12-support)
- [BoringSSL](https://boringssl.googlesource.com/boringssl/)
- [HttpClient, HttpClientHandler a WebRequestHandler vysvětlené](https://blogs.msdn.microsoft.com/henrikn/2012/08/07/httpclient-httpclienthandler-and-webrequesthandler-explained/)
- [System.Net.HttpClient](https://msdn.microsoft.com/en-us/library/system.net.http.httpclient(v=vs.118).aspx)
- [System.Net.HttpClientHandler](https://msdn.microsoft.com/en-us/library/system.net.http.httpclienthandler(v=vs.118).aspx)
- [System.Net.HttpMessageHandler](https://msdn.microsoft.com/en-us/library/system.net.http.httpmessagehandler(v=vs.118).aspx)
- [System.Net.HttpWebRequest](https://msdn.microsoft.com/en-us/library/system.net.httpwebrequest(v=vs.110).aspx)
- [System.Net.WebClient](https://msdn.microsoft.com/en-us/library/system.net.webclient(v=vs.110).aspx)
- [System.Net.WebRequest](https://msdn.microsoft.com/en-us/library/system.net.webrequest(v=vs.110).aspx)
- [java.net.URLConnection](http://developer.android.com/reference/java/net/URLConnection.html)
- [Foundation.CFNetwork](https://developer.xamarin.com/api/type/CoreFoundation.CFNetwork/)
- [Foundation.NSUrlConnection](https://developer.xamarin.com/api/type/Foundation.NSUrlConnection/)
- [System.Net.WebRequest](https://msdn.microsoft.com/en-us/library/system.net.webrequest(v=vs.110).aspx)
- [Klient HTTP (ukázka)](https://developer.xamarin.com/samples/monotouch/HttpClient/)
