---
title: Transport Layer Security (TLS) 1.2
description: Povolení protokolu TLS 1.2 pro projekty Xamarin pro Android, iOS a Mac
ms.prod: xamarin
ms.assetid: 399F71C6-16A4-4ABC-B30D-AF17D066A5FA
ms.technology: xamarin-cross-platform
author: asb3993
ms.author: amburns
ms.date: 04/20/2018
ms.openlocfilehash: 6205e8633ccdd2c1e568e7de8103c38eb9edbc2f
ms.sourcegitcommit: dc882e9631b4ed52596b944a6fbbdde309346943
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/26/2018
---
# <a name="transport-layer-security-tls-12"></a>Transport Layer Security (TLS) 1.2

Pomocí nejnovější verze [ _Transport Layer Security_ (TLS)](https://en.wikipedia.org/wiki/Transport_Layer_Security) je důležité zajistit síťové komunikace mezi aplikací jsou bezpečné.

> [!WARNING]
> **Duben, 2018** – kvůli zvýšení zabezpečení požadavky, včetně soulad s normami PCI, Hlavní poskytovatelé cloudové a webové servery se očekává, zastavit, podpora TLS verze starší než 1.2.  Xamarin projektů vytvořených v předchozích verzích sady Visual Studio výchozí používat starší verze protokolu TLS.
>
> Aby se zajistilo, pokračovat v práci s těmito servery a služby, aplikace **by měl aktualizovat projekty Xamarin, aby toto nastavení, pak znovu sestavit a znovu nasaďte aplikace** pro vaše uživatele.

Projekty musí odkazovat **System.Net.Http** sestavení a nakonfigurovat, jak je uvedeno níže.

## <a name="update-android-to-tls-12"></a>Aktualizace Android TLS 1.2

Aktualizace **implementace HttpClient** a **SSL/TLS implementace** možnosti Povolit zabezpečení protokolu TLS 1.2.

> [!NOTE]
> Vyžaduje Android 5.0 nebo novější.

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

Tato nastavení najdete v **vlastnosti projektu > Android možnosti** a kliknete na **Upřesnit** tlačítko:

[![Konfigurace HttpClient a TLS v sadě Visual Studio](transport-layer-security-images/android-win-sml.png)](transport-layer-security-images/android-win.png#lightbox)

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

Tato nastavení najdete v **možnosti projektu > sestavení > Android sestavení** karty:

[![Konfigurace HttpClient a TLS v sadě Visual Studio pro Mac](transport-layer-security-images/android-mac-sml.png)](transport-layer-security-images/android-mac.png#lightbox)

-----

## <a name="update-ios-to-tls-12"></a>Aktualizace iOS TLS 1.2

Aktualizace **implementace HttpClient** možnost povolit TSL 1.2 zabezpečení.

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

Toto nastavení lze nalézt v **vlastnosti projektu > iOS sestavení**:

[![Konfigurace HttpClient a TLS v sadě Visual Studio](transport-layer-security-images/ios-win-sml.png)](transport-layer-security-images/ios-win.png#lightbox)

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

Toto nastavení lze nalézt v **možnosti projektu > sestavení > iOS sestavení** karty:

[![Konfigurace HttpClient v sadě Visual Studio pro Mac](transport-layer-security-images/ios-mac-sml.png)](transport-layer-security-images/ios-mac.png#lightbox)

-----

## <a name="update-macos-to-tls-12-in-visual-studio-for-mac"></a>Aktualizace systému macOS na TLS 1.2 v sadě Visual Studio pro Mac

Aktualizace **implementace HttpClient** možnost **možnosti projektu > sestavení > Mac sestavení** a povolit TSL 1.2 zabezpečení:

[![Konfigurace HttpClient v sadě Visual Studio pro Mac](transport-layer-security-images/macos-mac-sml.png)](transport-layer-security-images/macos-mac.png#lightbox)

## <a name="alternative-configuration-options"></a>Možnosti alternativní konfigurace

Tato část popisuje alternativy TLS 1.2 podporované konfigurace uvedené výše.
Vývojáři aplikací zvažte alternativy pouze, pokud tito uživatelé pochopit rizika pomocí různé úrovně podpory protokolu TLS.

### <a name="httpclient-implementation"></a>Implementace HttpClient

Vývojáři Xamarin byly vždy možné použít nativní síťové třídy v jejich kódu, ale je také možnost, která určuje, které síťový zásobník používá `HttpClient` třídy. To poskytuje známé .NET API, který má vyšší rychlosti a zabezpečení výhod nativní platformy.

Možnosti jsou:

- **Spravované zásobníku** – funkce poskytované Mono sítě, nebo
- **Nativní zásobníku** – různých síťových rozhraní API poskytovaných základní platformy (Android, iOS nebo systému macOS).

Spravované zásobníku poskytuje nejvyšší úroveň kompatibility s existující kód .NET, ale může být pomalejší a mít za následek větší velikost spustitelný soubor.

Nativní možnosti může být rychlejší a mít lepší zabezpečení (včetně protokolu TLS 1.2), ale nemusí poskytnout všechny funkce a možnosti `HttpClient` třídy.

### <a name="ssltls-implementation-android"></a>Implementace protokolu SSL/TLS (Android)

Možnosti projektu Android vám také umožní vybrat které implementace SSL/TLS na podporu:

- **Spravovaná/mono** – protokol TLS 1.1 v systému Android
- **Nativní** – protokol TLS 1.2 v systému Android.

Nové projekty Xamarin výchozí pro nativní implementaci, který podporuje protokol TLS 1.2, (který se doporučuje pro všechny projekty), ale můžete přepnout zpět do spravovaného kódu Pokud požadované z důvodu kompatibility.

> [!IMPORTANT]
> **Mono nebo spravované** možnost byla [ze systémů iOS a Mac](https://developer.xamarin.com/releases/ios/xamarin.ios_10/xamarin.ios_10.8/) možnosti projektu.
>
> Možnost nativního se vždy používá na iOS a Mac platformách.

## <a name="platform-specific-details"></a>Podrobnosti o specifických pro platformy

Výše uvedené souhrn vysvětluje nastavení projektu pro implementace HttpClient a SSL/TLS v projektech Xamarin. Implementace HttpClient lze nastavit i dynamicky v kódu. Tyto příručky specifické pro platformu pro další informace najdete:

- [**Android**](~/android/app-fundamentals/http-stack.md)
- [**iOS a Mac**](~/cross-platform/macios/http-stack.md)


## <a name="summary"></a>Souhrn

Aplikace by měly používat zabezpečení TLS (Transport Layer) 1.2, pokud je to možné.
By měl aktualizovat nastavení v existujících aplikací podle pokynů v tomto článku, pak znovu sestavit a znovu nasadit na vaše zákazníky.

## <a name="related-links"></a>Související odkazy

- [Zabezpečení přenosu aplikací](~/ios/app-fundamentals/ats.md)
- [Prostředí Xamarin.Android](~/android/deploy-test/environment.md)
- [Xamarin cyklus 9 (únor 2017)](https://releases.xamarin.com/stable-release-cycle-9/)
- [Protokol TLS (Wikipedia)](https://en.wikipedia.org/wiki/Transport_Layer_Security)
- [Poznámky k verzi mono 4.8 – podpora TLS 1.2](http://www.mono-project.com/docs/about-mono/releases/4.8.0/#tls-12-support)
- [BoringSSL](https://boringssl.googlesource.com/boringssl/)
- [HttpClient, HttpClientHandler a WebRequestHandler vysvětlené](https://blogs.msdn.microsoft.com/henrikn/2012/08/07/httpclient-httpclienthandler-and-webrequesthandler-explained/)
- [System.Net.HttpClient](https://msdn.microsoft.com/library/system.net.http.httpclient(v=vs.118).aspx)
- [System.Net.HttpClientHandler](https://msdn.microsoft.com/library/system.net.http.httpclienthandler(v=vs.118).aspx)
- [System.Net.HttpMessageHandler](https://msdn.microsoft.com/library/system.net.http.httpmessagehandler(v=vs.118).aspx)
- [System.Net.HttpWebRequest](https://msdn.microsoft.com/library/system.net.httpwebrequest(v=vs.110).aspx)
- [System.Net.WebClient](https://msdn.microsoft.com/library/system.net.webclient(v=vs.110).aspx)
- [System.Net.WebRequest](https://msdn.microsoft.com/library/system.net.webrequest(v=vs.110).aspx)
- [java.net.URLConnection](http://developer.android.com/reference/java/net/URLConnection.html)
- [Foundation.CFNetwork](https://developer.xamarin.com/api/type/CoreFoundation.CFNetwork/)
- [Foundation.NSUrlConnection](https://developer.xamarin.com/api/type/Foundation.NSUrlConnection/)
- [System.Net.WebRequest](https://msdn.microsoft.com/library/system.net.webrequest(v=vs.110).aspx)
- [Klient HTTP (ukázka)](https://developer.xamarin.com/samples/monotouch/HttpClient/)
