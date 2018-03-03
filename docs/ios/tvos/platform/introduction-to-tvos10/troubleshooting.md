---
title: "Poradce při potížích"
description: "Tento článek obsahuje několik tipy k řešení potíží pro práci s tvOS 10 v Xamarin.tvOS aplikace."
ms.topic: article
ms.prod: xamarin
ms.assetid: EA5564BB-C415-49A2-B70C-3DBF5E0F3FAB
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/16/2017
ms.openlocfilehash: d45b9ed04d3ae4815d408d82068e588a2cbcc6f8
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 02/27/2018
---
# <a name="troubleshooting"></a>Poradce při potížích

_Tento článek obsahuje několik tipy k řešení potíží pro práci s tvOS 10 v Xamarin.tvOS aplikace._

V následujících částech jsou uvedené některé známé problémy, ke kterým dochází při používání tvOS 10 s Xamarin.tvOS a řešení těchto problémů:

- [App Store](#App-Store)
- [Binární kompatibilitu](#Binary-Compatibility)
- [Protokol HTTP CFNetwork](#CFNetwork-HTTP-Protocol)
- [CloudKit](#CloudKit)
- [CoreImage](#CoreImage)
- [NSUserActivity](#NSUserActivity)
- [UIKit](#UIKit)

<a name="App-Store" />

## <a name="app-store"></a>Obchod s aplikacemi

Známé problémy:

 - Při testování nákupy v aplikaci v prostředí izolovaného prostoru, ověřovací dialog se mohou objevit dvakrát.
 - Při testování nákupy v aplikaci s obsahem hostované v prostředí izolovaného prostoru, zobrazí se dialogové okno heslo pokaždé, když aplikace je převést na popředí, až po dokončení stahování obsahu.

<a name="Binary-Compatibility" />

## <a name="binary-compatibility"></a>Binární kompatibilitu

Známé problémy:

 - Volání metody `NSObject.ValueForKey` bude `null` klíč bude mít za následek výjimku.
 - Odkazování na písmo podle názvu, při volání metody `UIFont.WithName` způsobí, že havárie.
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

## <a name="coreimage"></a>CoreImage

`CIImageProcessor` Rozhraní API nyní podporuje počet libovolný vstupní obrázků. `CIImageProcessor` Rozhraní API, která byla součástí tvOS 10 beta 1, se odeberou.

<a name="NSUserActivity" />

## <a name="nsuseractivity"></a>NSUserActivity

Po operaci aby Handoff `UserInfo` vlastnost `NSUserActivity` objektu může být prázdná. Explicitní volání `BecomeCurrent` NSUserActivity' objektu jako aktuální alternativní řešení.

<a name="UIKit" />

## <a name="uikit"></a>UIKit

Známé problémy:

 - Změní na pozadí vzhled `UINavigationBar`, `UITabBar` nebo `UIToolBar` může vést k předání rozložení přeložit nový vzhled. Probíhá pokus o upravit tyto vzhledy uvnitř `LayoutSubviews`, `UpdateConstraints`, `WillLayoutSubviews` nebo `DidUpdateSubviews` událost může mít za následek smyčky nekonečné rozložení.
 - V tvOS 10, volání `RemoveGestureRecognizer` metodu `UIView` objekt explicitně zruší všechny funkce rozpoznávání gesto probíhá.
 - Vidění řadiče zobrazení teď může ovlivnit vzhled stavový řádek.
 - tvOS 10 vyžaduje vývojáři volání `base.AwakeFromNib` při vytvoření podtřídy `UIViewController` a přepsáním `AwakeFromNib` metoda.
 - Aplikace s vlastní `UIView` podtříd přepsat `LayoutSubviews` a nesprávné rozložení před voláním `base.LayoutSubviews` mohou aktivovat smyčky nekonečné rozložení v tvOS 10.
 - Specifické pro směr nebo flippable imagí prostředky jsou žádné překlopení bylo přiřazeno `UIButton` objekty.





## <a name="related-links"></a>Související odkazy

- [Ukázky pro tvOS](https://developer.xamarin.com/samples/tvos/all/)
- [Co je nového v tvOS 10](https://developer.apple.com/library/prerelease/content/releasenotes/General/WhatsNewinTVOS/Articles/tvOS10.html#//apple_ref/doc/uid/TP40017259-SW1)
