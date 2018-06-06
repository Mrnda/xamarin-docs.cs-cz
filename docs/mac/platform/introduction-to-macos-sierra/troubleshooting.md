---
title: Xamarin.Mac - systému macOS Sierra řešení potíží
description: Tento dokument obsahuje několik tipy k řešení potíží pro práci s systému macOS Sierra v Xamarin.Mac aplikace. Tipy se týkají Mac App Storu, dotykový identifikátor, binární kompatibilitu, CFNetwork, CloudKit a další.
ms.prod: xamarin
ms.assetid: 323DD5EE-87CE-48E4-B234-1CF61B45A019
ms.technology: xamarin-mac
author: bradumbaugh
ms.author: brumbaug
ms.date: 09/22/2016
ms.openlocfilehash: 5b2571d9562fd137257e2dd0ea2ada8f071bab92
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 06/05/2018
ms.locfileid: "34792317"
---
# <a name="xamarinmac---macos-sierra-troubleshooting"></a>Xamarin.Mac - systému macOS Sierra řešení potíží

_Tento článek obsahuje několik tipy k řešení potíží pro práci s systému macOS Sierra v Xamarin.Mac aplikace._

V následujících částech jsou uvedené některé známé problémy, ke kterým dochází při používání systému macOS Sierra s Xamarin.mac a řešení těchto problémů:

- [App Store](#App-Store)
- [Apple Pay](#Apple-Pay)
- [Binární kompatibilitu](#Binary-Compatibility)
- [Protokol HTTP CFNetwork](#CFNetwork-HTTP-Protocol)
- [CloudKit](#CloudKit)
- [Základní Image](#CoreImage)
- [Oznámení](#Notifications)
- [NSUserActivity](#NSUserActivity)
- [Safari](#Safari)

<a name="App-Store" />

## <a name="app-store"></a>Obchod s aplikacemi

Známé problémy:

- Při testování nákupy v aplikaci v prostředí izolovaného prostoru, ověřovací dialog se mohou objevit dvakrát.
- Při testování nákupy v aplikaci s obsahem hostované v prostředí izolovaného prostoru, zobrazí se dialogové okno heslo pokaždé, když aplikace je převést na popředí, až po dokončení stahování obsahu.

<a name="Apple-Pay" />

## <a name="apple-pay"></a>Platím Apple

Pokud nesprávné vypršení platnosti datum nebo zabezpečení (SH) je zadán kód při přidávání nové platební karty Apple platit, Karta procesu zřizování se ukončí.

<a name="Binary-Compatibility" />

## <a name="binary-compatibility"></a>Binární kompatibilitu

Známé problémy:

- Volání metody `NSObject.ValueForKey` bude `null` klíč bude mít za následek výjimku.
- Obě `NSURLSession` a NSURLConnection` no longer RC4 cipher suites during the TLS handshake for `http://' adresy URL.
- Aplikace může na jejich upravit superview geometrie buď `ViewWillLayoutSubviews` nebo `LayoutSubviews` metody.
- Pro všechna připojení protokolem SSL/TLS symetrický šifrovací algoritmus RC4 nyní ve výchozím nastavení vypnutá. Kromě toho rozhraní API zabezpečit přenos již nepodporuje SSLv3 a se doporučuje, aby aplikace přestat používat šifrování SHA-1 a 3DES co nejdříve.

<a name="CFNetwork-HTTP-Protocol" />

## <a name="cfnetwork-http-protocol"></a>Protokol HTTP CFNetwork

`HTTPBodyStream` Vlastnost `NSMutableURLRequest` třída musí nastavit vyberte datový proud, od `NSURLConnection` a `NSURLSession` nyní výhradně vynutit tento požadavek.

<a name="CloudKit" />

## <a name="cloudkit"></a>CloudKit

Dlouho běžící operace vrátí _"Nemáte oprávnění k uložení souboru."_ došlo k chybě.

<a name="CoreImage" />

## <a name="core-image"></a>Základní Image

`CIImageProcessor` Rozhraní API nyní podporuje počet libovolný vstupní obrázků. `CIImageProcessor` Rozhraní API, která byla zahrnuta v systému macOS Sierra beta 1, se odeberou.

<a name="Notifications" />

## <a name="notifications"></a>Oznámení

Při práci s obsahu rozšíření oznámení, řadiče zobrazení nejsou správně vydání a může mít za následek selhání při dosažení limitu paměti rozšíření.

<a name="NSUserActivity" />

## <a name="nsuseractivity"></a>NSUserActivity

Po operaci aby Handoff `UserInfo` vlastnost `NSUserActivity` objektu může být prázdná. Explicitní volání `BecomeCurrent` `NSUserActivity` objektu jako aktuální alternativní řešení.

<a name="Safari" />

## <a name="safari"></a>Safari

WebGeolocation vyžaduje zabezpečený (`https://`) adresy URL pro práci na iOS 10 a systému macOS Sierra, aby se zabránilo zneužití aplikace data o umístění.







## <a name="related-links"></a>Související odkazy

- [Ukázky pro Mac](https://developer.xamarin.com/samples/mac/)
- [Co je nového v OS X 10.12](https://developer.apple.com/library/prerelease/content/releasenotes/MacOSX/WhatsNewInOSX/Articles/OSXv10.html#//apple_ref/doc/uid/TP40017145-SW1)
