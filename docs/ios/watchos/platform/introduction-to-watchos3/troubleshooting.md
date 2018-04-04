---
title: watchOS 3 Poradce při potížích
description: Tento článek obsahuje několik tipy k řešení potíží pro práci s watchOS 3 v aplikacích Apple Watch Xamarin.
ms.prod: xamarin
ms.assetid: 5911D898-0E23-40CC-9F3C-5F61B4D50ADC
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/17/2017
ms.openlocfilehash: 159c6a6dadcaa325abc7fd747abc9b2ba2f26a9c
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/04/2018
---
# <a name="watchos-3-troubleshooting"></a>watchOS 3 Poradce při potížích

_Tento článek obsahuje několik tipy k řešení potíží pro práci s watchOS 3 v aplikacích Apple Watch Xamarin._

Na této stránce jsou uvedené některé známé problémy, ke kterým dochází při používání watchOS 3 s Xamarinem a řešení těchto problémů.

## <a name="activities"></a>Aktivity

Aktivity sdílení fungovala správně musí být všechny spárované sleduje Apple spuštěna watchOS 3.

Známé problémy:

- Odeslání odpovědi k oznámení sdílení aktivity někdy se nezdaří.
- Odeslání odpovědi k oznámení sdílení aktivity zprávou může selhat.
- Kontextová text nad zprávu oznámení sdílení aktivity budou nesprávné.


## <a name="apple-pay"></a>Platím Apple

Známé problémy:

- Pokud nesprávné datum vypršení platnosti nebo SH kódu je zadán pro nové pozor platebních v Apple platit, když stiskne **Další** běžící proces dojde k chybě.
- Nákupy v aplikaci dotykový identifikátor nutnosti číslo kódu PIN může dojít k chybě.



## <a name="auto-mac-unlock"></a>Mac automatické odemknutí

S použitím watchOS 3 beta 2 (nebo novější) a systému macOS Sierra beta 2 (nebo novější), pokud je na serveru služby iCloud účet uživatele povoleno dvoufaktorové ověřování, můžete použít jejich Apple Watch na automatické odemknutí jejich Mac.



## <a name="background-refresh"></a>Aktualizace na pozadí

Narušení systémové prostředky způsobí watchOS 3 havárie aplikace s následujícími kódy výjimka:

- **0xc51bad01** -aplikace využívat příliš mnoho času procesoru.
- **0xc51bad02** -aplikace využívat příliš mnoho času wall.
- **0xc51bad03** -aplikace neměl dostatek runtime k dokončení aktuální úlohy.



## <a name="clock"></a>Clock

Komplikace z nově nainstalovaných aplikací Apple Watch může zobrazují jako prázdné. Restartování Apple Watch chcete tento problém vyřešit.


## <a name="connectivity"></a>Připojení k

Známé problémy:

- watchOS nebude požádat uživatele o přístupových oprávněních pro chráněný uživatelská data na Apple Watch. Udělit přístup na zařízení iPhone aplikace před použitím dat v aplikaci sledování.
- Apple Watch ocitnout ve stavu, kde všechny přenosy WatchConnectivity selžou, restartovat Apple Watch opravit.


## <a name="notifications"></a>Oznámení

Pokud přílohy média je příliš velký, má být použit na uživatele iPhone, ale není jejich Apple Watch.


## <a name="nsurlconnection"></a>NSURLConnection

Všechny `NSURLConnection` připojení pomocí starší protokoly TLS, se nezdaří. Pro všechna připojení protokolem SSL/TLS symetrický šifrovací algoritmus RC4 nyní ve výchozím nastavení vypnutá. Kromě toho rozhraní API zabezpečit přenos již nepodporuje SSLv3 a se doporučuje, aby aplikace přestat používat šifrování SHA-1 a 3DES co nejdříve.

Od verze watchOS 3 je výhradně vynucení zabezpečení připojení protokolem SSL/TLS společností Apple. Ovlivněné služby a aplikace by měl aktualizovat webové servery do pomocí nejnovější verze protokolu TLS.


## <a name="nsurlsession"></a>NSURLSession

Od verze watchOS 3 `HTTPBodyStream` vlastnost `NSMutableURLRequest` třída musí nastavit vyberte datový proud, od `NSURLConnection` a `NSURLSession` nyní výhradně vynutit tento požadavek.


## <a name="privacy"></a>Ochrana osobních údajů

Známé problémy:

Při práci s `https://` adresy URL obou `NSURLSession` a `NSURLConnection` již nepodporují RC4 sady šifer během metody handshake TLS. Jeden z následujících kódů chyb může být generována:

- **-1200 nebo-98** – `NSURLErrorSecurityConnectionFailed` a SecureTransport chyb.
- **-1200 [3:-9824]** -zatížení http se nezdařilo.
- **-1200**  -  `NSURLConnection` dokončeno s chybami.

Od verze watchOS 3 je výhradně vynucení zabezpečení připojení protokolem SSL/TLS společností Apple. Ovlivněné služby a aplikace by měl aktualizovat webové servery do pomocí nejnovější verze protokolu TLS. V tématu [NSURLConnection](#NSURLConnection) výše pro další informace.


## <a name="snapshots"></a>Snímky

WatchKit aplikace, které nebyly přijaty nové `HandelBackgroundTask` rozhraní API se už nebude pravidelné aktualizace v watchOS 3. 


## <a name="watchkit"></a>WatchKit

SpriteKit a SceneKit scény bude pozastavena, když aplikace přejde na pozadí v watchOS ukotvení.


## <a name="related-links"></a>Související odkazy

- [Ukázky pro watchOS](https://developer.xamarin.com/samples/watchos/all/)
- [Co je nového v watchOS 3](https://developer.apple.com/library/prerelease/content/releasenotes/General/WhatsNewInwatchOS/Articles/watchOS3.html#//apple_ref/doc/uid/TP40017085-SW1)
