---
title: "Zabezpečení přenosu aplikace"
description: "Zabezpečení přenosu aplikace (ATS) vynucuje zabezpečené připojení mezi prostředků z Internetu (třeba server back-end aplikace) a aplikací."
ms.topic: article
ms.prod: xamarin
ms.assetid: 0E2217F1-FC96-4D0A-ABAB-D40AD8F96502
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 06/13/2017
ms.openlocfilehash: 60858e05e222725f05eb67bd7aaa4e56d2ff3880
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 02/27/2018
---
# <a name="app-transport-security"></a>Zabezpečení přenosu aplikace

_Zabezpečení přenosu aplikace (ATS) vynucuje zabezpečené připojení mezi prostředků z Internetu (třeba server back-end aplikace) a aplikací._

Tento článek vás seznámí změny zabezpečení, které vynucuje zabezpečení přenosu aplikaci v aplikaci pro iOS 9 a [co to znamená pro projekty Xamarin.iOS](#xamarinsupport), bude se vztahovat [možnosti konfigurace ATS](#config) a bude se vztahovat postup [výslovný nesouhlas ATS](#optout) ATS v případě potřeby. Protože ATS je ve výchozím nastavení povolené, jakékoli nezabezpečené připojení k Internetu vyvolá výjimku v aplikacích pro iOS 9 (Pokud jste explicitně povoleno ji).


## <a name="about-app-transport-security"></a>O zabezpečení přenosu aplikace

Jak jsme uvedli výše, ATS zajistí, že všechny internetové komunikace v iOS 9 a OS X El Capitan odpovídají zabezpečené připojení osvědčených postupů, a tím prevence náhodného zpřístupnění citlivých informací, buď přímo prostřednictvím aplikace nebo knihovny, která je využívání.

Pro existující aplikace, implementovat `HTTPS` protokolu, pokud je to možné. Pro nové aplikace Xamarin.iOS, měli byste použít `HTTPS` výhradně při komunikaci s internetovým prostředkům. Kromě toho vysoké úrovně rozhraní API komunikace musí zašifrovat pomocí přeposílání TLS verze 1.2.

Jakékoli připojení, s [NSUrlConnection](https://developer.xamarin.com/api/type/Foundation.NSUrlConnection/), [CFUrl](https://developer.xamarin.com/api/type/CoreFoundation.CFUrl/) nebo [NSUrlSession](https://developer.xamarin.com/api/type/Foundation.NSUrlSession/) ATS použije ve výchozím nastavení v aplikacích pro iOS 9 a OS X 10.11 (El Capitan).

## <a name="default-ats-behavior"></a>Výchozí chování ATS

Vzhledem k tomu, že je ve výchozím nastavení v aplikacích, které jsou vytvořené pro iOS 9 a OS X 10.11 (El Capitan), všechna připojení pomocí povolené ATS [NSUrlConnection](https://developer.xamarin.com/api/type/Foundation.NSUrlConnection/), [CFUrl](https://developer.xamarin.com/api/type/CoreFoundation.CFUrl/) nebo [NSUrlSession](https://developer.xamarin.com/api/type/Foundation.NSUrlSession/) bude podléhají Požadavky na zabezpečení ATS. Pokud vaše připojení těchto požadavků nesplňuje, budou se nezdaří s výjimkou.

### <a name="ats-connection-requirements"></a>Požadavky na připojení ATS

ATS vynutí následující požadavky pro všechna připojení k Internetu:

- Všechny šifry připojení musí používat přeposílání. Zobrazit seznam přijatý šifry níže.
- Protokol zabezpečení TLS (Transport Layer) musí být verze 1.2 nebo vyšší.
- Alespoň SHA256 otisků prstů s 2 048 bitů nebo větší klíč RSA, nebo 256 bitů nebo větší klíč Elliptic Curve (ECC) musíte použít pro všechny certifikáty.

Znovu protože ATS je povolené ve výchozím nastavení v iOS 9, pokusy o připojení, které tyto požadavky nesplňují způsobí výjimku hlášeny. 

<a name="ATS-Compatible-Ciphers" />

### <a name="ats-compatible-ciphers"></a>Kompatibilní šifry ATS

Následující typ šifrovací přeposílání přijímají ATS zabezpečené internetové komunikace:

- `TLS_ECDHE_ECDSA_WITH_AES_256_GCM_SHA384`
- `TLS_ECDHE_ECDSA_WITH_AES_128_GCM_SHA256`
- `TLS_ECDHE_ECDSA_WITH_AES_256_CBC_SHA384`
- `TLS_ECDHE_ECDSA_WITH_AES_256_CBC_SHA`
- `TLS_ECDHE_ECDSA_WITH_AES_128_CBC_SHA256`
- `TLS_ECDHE_ECDSA_WITH_AES_128_CBC_SHA`
- `TLS_ECDHE_RSA_WITH_AES_256_GCM_SHA384`
- `TLS_ECDHE_RSA_WITH_AES_128_GCM_SHA256`
- `TLS_ECDHE_RSA_WITH_AES_256_CBC_SHA384`
- `TLS_ECDHE_RSA_WITH_AES_128_CBC_SHA256`
- `TLS_ECDHE_RSA_WITH_AES_128_CBC_SHA`

Další informace o práci s iOS internetové komunikace třídy, najdete v tématu společnosti Apple [referenci třídy NSURLConnection](https://developer.apple.com/library/prerelease/ios/documentation/Cocoa/Reference/Foundation/Classes/NSURLConnection_Class/index.html#//apple_ref/doc/uid/TP40003755), [CFURL odkaz](https://developer.apple.com/library/prerelease/ios/documentation/CoreFoundation/Reference/CFURLRef/index.html#//apple_ref/doc/uid/20001206) nebo [odkaz NSURLSession – třída ](https://developer.apple.com/library/prerelease/ios/documentation/Foundation/Reference/NSURLSession_class/index.html#//apple_ref/doc/uid/TP40013435).

<a name="xamarinsupport" />

## <a name="supporting-ats-in-xamarinios"></a>Podpora ATS v Xamarin.iOS

Protože ATS je povoleno ve výchozím nastavení v iOS 9 a OS X El Capitan, pokud vaše aplikace Xamarin.iOS nebo všechny knihovny nebo službu, která používá vytvoří připojení k Internetu, budete muset provést některé akce nebo připojení bude mít za následek výjimku hlášeny.

Pro existující aplikace, Apple naznačuje, které podporujete `HTTPS` protokolu co nejdříve. Pokud jste buď nelze vzhledem k tomu, že se připojujete k 3. stran webová služba, která nepodporuje `HTTPS` nebo pokud podporujete `HTTPS` by bylo nepraktické, můžete můžete odhlásit ATS. Najdete v článku [Opting mimo ATS](#Opting-Out-of-ATS) části níže další podrobnosti.

Pro novou aplikaci Xamarin.iOS, měli byste použít `HTTPS` výhradně při komunikaci s internetovým prostředkům. Znovu, můžou nastat situace (např. pomocí 3. stran webová služba) kde to není možné a budete muset odhlásit ATS.

Kromě toho ATS vynucuje vysoké úrovně komunikace rozhraní API k šifrování pomocí protokolu TLS verze 1.2 přeposílání. Najdete v článku [požadavky na připojení ATS](#ATS-Connection-Requirements) a [kompatibilní šifry ATS](#ATS-Compatible-Ciphers) částech výše další podrobnosti.

Při nemusí být obeznámeni s TLS ([Transport Layer Security](https://en.wikipedia.org/wiki/Transport_Layer_Security)) je nástupcem SSL ([Secure Socket Layer](https://en.wikipedia.org/wiki/Transport_Layer_Security)) a poskytuje kolekci kryptografické protokoly vynutit zabezpečení přes Síťová připojení.

Úroveň protokolu TLS řídí webové služby, která jsou využívání a je proto mimo ovládací prvek aplikace. Jak `HttpClient` a `ModernHttpClient` by měl automaticky používat nejvyšší úroveň šifrování TLS podporována serverem.

V závislosti na serveru, mluvení do (zejména pokud jde o 3. stran službu), bude pravděpodobně nutné zakázat přeposílání nebo vyberte nižší úroveň protokolu TLS. Najdete v článku [možnosti konfigurace ATS](#Configuring-ATS-Options) části níže další podrobnosti.

> [!IMPORTANT]
> **Poznámka:** zabezpečení přenosu aplikace se nevztahuje na pomocí aplikace Xamarin **implementace HTTPClient spravované**. Platí pro připojení pomocí CFNetwork **implementace HTTPClient** nebo **implementace NSURLSession HTTPClient** pouze.

### <a name="setting-the-httpclient-implementation"></a>Nastavení implementace HTTPClient

Chcete-li nastavit implementace HTTPClient používá aplikaci pro iOS, dvakrát klikněte na **projektu** v **Průzkumníku řešení** otevřete **možnosti projektu**. Přejděte na **iOS sestavení** a vyberte typ požadované klienta v části **implementace HttpClient** rozevíracího seznamu:

![](ats-images/client01.png "Nastavení iOS sestavení možnosti")


#### <a name="managed-handler"></a>Spravované obslužné rutiny

Spravované obslužné rutiny je plně spravovaná obslužná rutina HttpClient, který byl dodán s předchozími verzemi Xamarin.iOS a je výchozí obslužnou rutinu.

Odborníci na:

- Je nejvíce kompatibilní s rozhraní Microsoft .NET a starší verze Xamarin.

Cons:

- Není plně integrován s iOS (např. je omezený na TLS 1.0).
- Je obvykle mnohem nižší než nativních rozhraní API.
- Vyžaduje více spravovaného kódu a vytvoří větší aplikace.

#### <a name="cfnetwork-handler"></a>Obslužná rutina CFNetwork

Obslužná rutina CFNetwork na základě je založena na nativního `CFNetwork` framework.

Odborníci na:

- Používá nativní rozhraní API pro lepší výkon a menší velikosti spustitelný soubor.
- Přidává podporu pro novější standardy, jako je protokol TLS 1.2.

Cons:

- Vyžaduje iOS 6 nebo novější.
- Z watchOS není k dispozici.
- Některé funkce HttpClient a možnosti nejsou k dispozici.

#### <a name="nsurlsession-handler"></a>Obslužná rutina NSUrlSession

Obslužná rutina NSUrlSession na základě je založena na nativního `NSUrlSession` rozhraní API.

Odborníci na:

- Používá nativní rozhraní API pro lepší výkon a menší velikosti spustitelný soubor.
- Přidává podporu pro novější standardy, jako je protokol TLS 1.2.

Cons:

- Vyžaduje iOS 7 nebo novější.
- Některé funkce HttpClient a možnosti nejsou k dispozici. 

## <a name="diagnosing-ats-issues"></a>Diagnostika problémů s ATS

Při pokusu o připojení k Internetu, buď přímo, nebo z webového zobrazení v iOS 9, může být ve tvaru dojde k chybě:

> Zabezpečení přenosu aplikaci zablokoval zatížení prostředků HTTP (http://www.-the-blocked-domain.com) ve formě prostého textu, vzhledem k tomu, že není zabezpečený. Dočasné výjimky se dá nakonfigurovat pomocí souboru Info.plist vaší aplikace.

Zabezpečení přenosu aplikace (ATS) v iOS9, vynucuje zabezpečené připojení mezi prostředků z Internetu (třeba server back-end aplikace) a aplikací. Kromě toho ATS vyžaduje komunikaci pomocí `HTTPS` protokolu a nejdůležitější komunikace rozhraní API k šifrování pomocí protokolu TLS verze 1.2 přeposílání.

Vzhledem k tomu, že je ve výchozím nastavení v aplikacích, které jsou vytvořené pro iOS 9 a OS X 10.11 (El Capitan), všechna připojení pomocí povolené ATS `NSURLConnection`, `CFURL` nebo `NSURLSession` budou platit ATS požadavky na zabezpečení. Pokud vaše připojení těchto požadavků nesplňuje, budou se nezdaří s výjimkou.

Apple také poskytuje [TLSTool ukázkovou aplikaci](https://developer.apple.com/library/mac/samplecode/sc1236/Introduction/Intro.html#//apple_ref/doc/uid/DTS40014927-Intro-DontLinkElementID_2) mohou být zkompilovány (nebo volitelně převodu na Xamarinu a C#) a používá k diagnostikování problémů ATS/TLS. Najdete v tématu [Opting mimo ATS](#Opting-Out_of_ATS) části níže informace o tom, jak tento problém vyřešit.


<a name="config" />

## <a name="configuring-ats-options"></a>Konfigurace možností ATS

Nastavením hodnoty pro konkrétní klíče ve vaší aplikaci můžete nakonfigurovat několik funkcí ATS **Info.plist** souboru. Následující klíče jsou k dispozici pro řízení ATS (_odsazené ukazují, jak jsou vnořeny_):

```csharp
NSAppTransportSecurity
    NSAllowsArbitraryLoads
    NSAllowsArbitraryLoadsInWebContent
    NSExceptionDomains
    <domain-name-for-exception-as-string>
        NSExceptionMinimumTLSVersion
        NSExceptionRequiresForwardSecrecy
        NSExceptionAllowsInsecureHTTPLoads
        NSRequiresCertificateTransparency
        NSIncludesSubdomains
        NSThirdPartyExceptionMinimumTLSVersion
        NSThirdPartyExceptionRequiresForwardSecrecy
        NSThirdPartyExceptionAllowsInsecureHTTPLoads
```

Každý klíč má následující typ a význam:

- **NSAppTransportSecurity** (`Dictionary`) – obsahuje všechna nastavení klíče a hodnoty pro ATS.
- **NSAllowsArbitraryLoads** (`Boolean`) – Pokud `YES` ATS bude zakázáno pro všechny domény **není** uvedené v `NSExceptionDomains`. Pro uvedené domény se použije zadaná nastavení zabezpečení.
- **NSAllowsArbitraryLoadsInWebContent** (`Boolean`) – Pokud `YES` vám umožní načíst správně, je stále povolena ochrana zabezpečení přenosu Apple (ATS) pro zbytek aplikace při webové stránky.
- **NSExceptionDomains** (`Dictionary`) – kolekce domén a nastavení zabezpečení, která ATS by měly používat pro danou doménu.
- **< Domain-name-for-exception-as-string >** (`Dictionary`) – kolekce výjimek pro danou doménu (např. `www.xamarin.com`).
- **NSExceptionMinimumTLSVersion** (`String`)-minimální verze protokolu TLS jako `TLSv1.0`, `TLSv1.1` nebo `TLSv1.2` (což je výchozí hodnota).
- **NSExceptionRequiresForwardSecrecy** (`Boolean`) – Pokud `NO` domény nemusí šifru pomocí dopředného zabezpečení. Výchozí hodnota je `YES`.
- **NSExceptionAllowsInsecureHTTPLoads** (`Boolean`) – Pokud `NO` (výchozí) veškerá komunikace s touto doménou musí být v `HTTPS` protokolu.
- **NSRequiresCertificateTransparency** (`Boolean`) – Pokud `YES` domény vrstvy SSL (Secure Sockets) musí obsahovat platný průhlednost data. Výchozí hodnota je `NO`.
- **NSIncludesSubdomains** (`Boolean`) – Pokud `YES` tato nastavení přepíší všechny subdomény tuto doménu. Výchozí hodnota je `NO`.
- **NSThirdPartyExceptionMinimumTLSVersion** (`String`)-TLS verze používá při 3. stran služby mimo kontrolu vývojáře pro doménu.
- **NSThirdPartyExceptionRequiresForwardSecrecy** (`Boolean`) – Pokud `YES` 3. stran domény vyžaduje přeposílání.
- **NSThirdPartyExceptionAllowsInsecureHTTPLoads** (`Boolean`) – Pokud `YES` ATS vám umožní nezabezpečené komunikace s doménami 3. stran.

<a name="optout" />

### <a name="opting-out-of-ats"></a>Vyjádření výslovného nesouhlas ATS

Při vysoce navrhuje Apple pomocí `HTTPS` informace na základě protokolu a zabezpečenou komunikaci k Internetu, může dojít k situaci, které tato akce není vždy možné. Například, pokud jsou komunikaci s webovou službou 3. stran nebo pomocí Internetu doručit služby Active Directory ve vaší aplikaci.

Pokud vaše aplikace Xamarin.iOS musíte provést žádost na nezabezpečené domény, následující změny do vaší aplikace **Info.plist** souboru zakáže výchozí nastavení zabezpečení, které vynucuje ATS pro danou doménu:

```xml
<key>NSAppTransportSecurity</key>
<dict>
    <key>NSExceptionDomains</key>
    <dict>
        <key>www.the-domain-name.com</key>
        <dict>
            <key>NSExceptionMinimumTLSVersion</key>
            <string>TLSv1.0</string>
            <key>NSExceptionRequiresForwardSecrecy</key>
            <false/>
            <key>NSExceptionAllowsInsecureHTTPLoads</key>
            <true/>
            <key>NSIncludesSubdomains</key>
            <true/>
        </dict>
    </dict>
</dict>
```

V sadě Visual Studio pro Mac, dvakrát klikněte na `Info.plist` souboru v **Průzkumníku řešení**, přepnout **zdroj** zobrazení a přidat výše klíče:

[ ![](ats-images/ats01.png "Zobrazení zdroje souboru Info.plist")](ats-images/ats01.png)


Pokud aplikace potřebuje k načtení a zobrazuje webového obsahu z nezabezpečeného lokalit, přidejte následující do vaší aplikace **Info.plist** souboru umožnit webové stránky načíst správně, je stále povolena ochrana zabezpečení přenosu Apple (ATS) pro zbývající při aplikace:

```xml
<key>NSAppTransportSecurity</key>
<dict>
    <key> NSAllowsArbitraryLoadsInWebContent</key>
    <true/>
</dict>
```

Volitelně můžete provést následující změny do vaší aplikace **Info.plist** souboru zcela zakázat ATS pro všechny domény a komunikaci v síti internet:

```xml
<key>NSAppTransportSecurity</key>
<dict>
    <key>NSAllowsArbitraryLoads</key>
    <true/>
</dict>
```

V sadě Visual Studio pro Mac, dvakrát klikněte na `Info.plist` souboru v **Průzkumníku řešení**, přepnout **zdroj** zobrazení a přidat výše klíče:

[ ![](ats-images/ats02.png "Zobrazení zdroje souboru Info.plist")](ats-images/ats02.png)

> [!IMPORTANT]
> **Poznámka:** Pokud vaše aplikace vyžaduje připojení k nezabezpečené webové stránky, měli byste **vždy** zadejte doménu jako výjimky pomocí `NSExceptionDomains` místo vypnutí úplně pomocí ATS `NSAllowsArbitraryLoads`. `NSAllowsArbitraryLoads` lze používat pouze v případě extrémně nouze.




Znovu, zakázání ATS by měl _pouze_ použije jako poslední možnost, pokud přepnutí na zabezpečená připojení je k dispozici nebo nepraktické.

<a name="Summary" />

## <a name="summary"></a>Souhrn

Tento článek má zavedená aplikace přenosu zabezpečení (ATS) a popisuje způsob vynucuje zabezpečené komunikace s Internetem. Nejdřív jsme zahrnutých změn, které ATS vyžaduje pro aplikace Xamarin.iOS systémem iOS 9. Potom jsme zahrnutých řízení ATS funkce a možnosti. Nakonec jsme zahrnutých zrušení ATS ve vaší aplikaci Xamarin.iOS.



## <a name="related-links"></a>Související odkazy

- [Ukázky iOS 9](https://developer.xamarin.com/samples/ios/iOS9/)
- [iOS 9 pro vývojáře](https://developer.apple.com/ios/pre-release/)
- [iOS 9.0](https://developer.apple.com/library/prerelease/ios/releasenotes/General/WhatsNewIniOS/Articles/iOS9.html)
