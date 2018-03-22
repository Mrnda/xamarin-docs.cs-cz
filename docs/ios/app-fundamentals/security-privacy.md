---
title: "iOS zabezpečení a ochrany osobních údajů funkce"
description: "Tento článek se zabývá práci s zabezpečení a ochrany osobních údajů v iOS a jejich vliv na aplikace Xamarin.iOS."
ms.topic: article
ms.prod: xamarin
ms.assetid: 718C8721-C359-4650-878A-D68E159A3F53
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/16/2017
ms.openlocfilehash: 5e4bbc22403c6c0bfa5c8dc7ac4e3a39545051d4
ms.sourcegitcommit: 73bd0c7e5f237f0a1be70a6c1384309bb26609d5
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 03/22/2018
---
# <a name="ios-security-and-privacy-features"></a>iOS zabezpečení a ochrany osobních údajů funkce

_Tento článek se zabývá práci s zabezpečení a ochrany osobních údajů v iOS a jejich vliv na aplikace Xamarin.iOS._

Apple udělal několik vylepšení zabezpečení a ochrany osobních údajů v iOS 10 (a vyšší), které vám pomůžou vývojáře zlepšit zabezpečení své aplikace a zajištění ochrany osobních údajů koncového uživatele. Tento článek se zabývá implementace tyto funkce v aplikaci pro Xamarin.iOS.

Podrobně se budeme v následujících tématech:

- [Obecné vylepšení](#General-Enhancements)
- [Přístup k uživatelským datům privátní](#Accessing-Private-User-Data)
    - [Nastavení ochrany osobních údajů klíče](#Setting-Privacy-Keys)
    
<a name="General-Enhancements" />

## <a name="general-enhancements"></a>Obecné vylepšení

Následující obecné změny provedly k zabezpečení a ochrana osobních údajů v iOS 10:

- Běžné Data zabezpečení architektura (CDSA) rozhraní API se už nepoužívá a měl by být nahrazen SecKey rozhraní API pro generování asymetrické klíče.
- Nové `NSAllowsArbitraryLoadsInWebContent` klíč můžete přidat do aplikace `Info.plist` souboru a umožní načíst správně, je stále povolena ochrana zabezpečení přenosu Apple (ATS) pro zbytek aplikace při webové stránky. Další informace najdete v tématu naše [zabezpečení přenosu aplikace](~/ios/app-fundamentals/ats.md) dokumentaci.
- Protože nové schránky v iOS 10 a systému macOS Sierra umožňuje uživatelům kopírovat a vkládat mezi zařízeními, rozšířila rozhraní API umožňující schránky do být omezeno na konkrétní zařízení a být označen časovým razítkem automaticky vymazat k danému bodu. Kromě toho pojmenované pracovních plochách už jsou trvalé a měl by být nahrazen sdíleným kontejnerům pracovní ploše.
- Pro všechna připojení protokolem SSL/TLS symetrický šifrovací algoritmus RC4 nyní ve výchozím nastavení vypnutá. Kromě toho rozhraní API zabezpečit přenos již nepodporuje SSLv3 a doporučuje se, že vývojář přestat používat šifrování SHA-1 a 3DES co nejdříve.

<a name="Accessing-Private-User-Data" />

## <a name="accessing-private-user-data"></a>Přístup k uživatelským datům privátní

Aplikace běžící v systému iOS 10 (nebo novější), musí deklarovat staticky jejich záměr přístupu konkrétním funkcím nebo informace o uživateli zadáním jeden či více klíčů o ochraně osobních údajů v jejich `Info.plist` soubory, které vysvětlují, uživateli, proč aplikace chce získat přístup.

> [!IMPORTANT]
> Aplikace, které neposkytl požadované klíče bude bezobslužně ukončena systémem při pokusu o přístup k jedné ze omezené funkce nebo informace o uživateli, _bez chyby_! Pokud se aplikace spustí, neočekávaně selhání v systému iOS 10, zajistěte, aby všechny požadované `Info.plist` bylo zadáno.

Klíče jsou k dispozici související s následující o ochraně osobních údajů:

- **Soukromí - Apple Hudba využití popis** (`NSAppleMusicUsageDescription`) – umožňuje vývojáři popisující, proč aplikace žádá o přístup uživatele média knihovny.
- **Soukromí - Bluetooth periferní využití popis** (`NSBluetoothPeripheralUsageDescription`) – umožňuje vývojáři popisující, proč aplikace požaduje přístup k Bluetooth na zařízení uživatele.
- **Soukromí - kalendáře využití popis** (`NSCalendarsUsageDescription`) – umožňuje vývojáři popisující, proč aplikace žádá o přístup uživatele kalendáře.
- **Ochrana osobních údajů – popis použití fotoaparátu** (`NSCameraUsageDescription`) – umožňuje vývojáři popisující, proč aplikace požaduje přístup k fotoaparátu zařízení.
- **Soukromí - kontakty využití popis** (`NSContactsUsageDescription`) – umožňuje vývojáři popisující, proč aplikace vyžaduje přístup kontakty uživatele.
- **Soukromí - stavu sdílení využití popis** (`NSHealthShareUsageDescription`) – umožňuje vývojáři popisující, proč aplikace chce získat přístup k datům o stavu uživatele. Další informace najdete v tématu společnosti Apple [referenci třídy HKHealthStore](https://developer.apple.com/reference/healthkit/hkhealthstore).
- **Soukromí - stavu aktualizace využití popis** (`NSHealthUpdateUsageDescription`) – umožňuje vývojáři popisující, proč aplikace chce upravit data stavu uživatele. Další informace najdete v tématu společnosti Apple [referenci třídy HKHealthStore](https://developer.apple.com/reference/healthkit/hkhealthstore).
- **Soukromí - HomeKit využití popis** (`NSHomeKitUsageDescription`) – umožňuje vývojáři popisující, proč aplikace žádá o přístup uživatele HomeKit konfigurační Data.
- **Soukromí - umístění vždy využití popis** (`NSLocationAlwaysUsageDescription`) – umožňuje vývojáři popisující, proč aplikace chce mít vždy přístup k umístění uživatele.
- [Zastaralý] **Soukromí - umístění využití popis** (`NSLocationUsageDescription`) – umožňuje vývojáři popisující, proč aplikace chce získat přístup k umístění uživatele. *Poznámka: Tento klíč se již nepoužívá v iOS 8 (a vyšší). Použití `NSLocationAlwaysUsageDescription` nebo `NSLocationWhenInUseUsageDescription` místo.*
- **Soukromí - umístění při v použití využití popis** (`NSLocationWhenInUseUsageDescription`) – umožňuje vývojáři popisující, proč aplikace chce získat přístup k umístění uživatele, když je spuštěná.
- [Zastaralý] **Soukromí - média knihovny využití popis** -umožňuje vývojáři popisující, proč aplikace žádá o přístup do knihovny médií uživatele. *Poznámka: Tento klíč se již nepoužívá v iOS 8 (a vyšší). Použití `NSAppleMusicUsageDescription` místo.*
- **Soukromí - mikrofon využití popis** (`NSMicrophoneUsageDescription`) – umožňuje vývojáři popisující, proč aplikace vyžaduje přístup mikrofon zařízení.
- **Soukromí - pohybu využití popis** (`NSMotionUsageDescription`) – umožňuje vývojáři popisující, proč aplikace požaduje přístup k zrychlení v zařízení.
- **Soukromí - fotografií knihovny využití popis** (`NSPhotoLibraryUsageDescription`) – umožňuje vývojáři popisující, proč aplikace vyžaduje přístup knihovny fotografií uživatele.
- **Soukromí - připomenutí využití popis** (`NSRemindersUsageDescription`) – umožňuje vývojáři popisující, proč aplikace žádá o přístup uživatele připomenutí.
- **Ochrana osobních údajů – popis používání Siri** (`NSSiriUsageDescription`) – umožňuje vývojáři popisující, proč aplikace chce poslat Siri uživatelská data.
- **Ochrana osobních údajů – popis využití rozpoznávání řeči** (`NSSpeechRecognitionUsageDescription`) – umožňuje vývojáři popisující, proč aplikace chce odeslat uživatelská data na servery rozpoznávání řeči společnosti Apple.
- **Ochrana osobních údajů – popis použití zprostředkovatele TV** (`NSVideoSubscriberAccountUsageDescription`) – umožňuje vývojáři popisující, proč aplikace požaduje přístup k účtu uživatele TV zprostředkovatele.

Další informace o práci s `Info.plist` klíče, najdete v tématu společnosti Apple [informace vlastnost seznamu Key Reference](https://developer.apple.com/library/content/documentation/General/Reference/InfoPlistKeyReference/Introduction/Introduction.html#//apple_ref/doc/uid/TP40009248-SW1).

<a name="Setting-Privacy-Keys" />

## <a name="setting-privacy-keys"></a>Nastavení ochrany osobních údajů klíče

Provést následující příklad přístup k HomeKit v systému iOS 10 (a vyšší), bude nutné přidat Vývojář `NSHomeKitUsageDescription` klíče na aplikaci `Info.plist` souboru a zadejte řetězec deklarace, proč aplikace chce získat přístup k databázi HomeKit uživatele. Tento řetězec předloží čas uživatele při prvním spuštění aplikace:

[![](security-privacy-images/info01.png "Příklad NSHomeKitUsageDescription výstrahy")](security-privacy-images/info01.png#lightbox)

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Xamarin.iOS pro aktuální sady Visual Studio nepodporuje úpravy vylepšení zabezpečení `Info.plist` nastavení z prostředí IDE, takže se bude vyžadovat následující alternativní řešení:

1. Otevřete `Info.plist` souboru v editoru externí text.
2. Před poslední `</dict>` uzlu, přidejte následující uzly: `<key>NSHomeKitUsageDescription</key>`
3. Přidejte následující uzel zadejte požadované Popis: `<string>Allows the app to control HomeKit enabled devices.</string>`
4. `Info.plist` Soubor by měl vypadat takto: 

    [![](security-privacy-images/info02vs.png "Soubor Info.plist by měl vypadat jako následující")](security-privacy-images/info02vs.png#lightbox)
4. Uložte změny do souboru.
5. Vraťte se k sadě Visual Studio a pak ji znovu zkompilovat této aplikace.

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

Pokud chcete nastavit některého z klíčů o ochraně osobních údajů, postupujte takto:

1. Dvakrát klikněte `Info.plist` souboru v **Průzkumníku řešení** otevřete pro úpravy.
2. V dolní části obrazovky, přepnout **zdroj** zobrazení.
3. Přidejte nový **položka** do seznamu.
4. Z rozevíracího seznamu vyberte klíč osobních údajů (například **o ochraně osobních údajů – popis využití HomeKit**): 

    [![](security-privacy-images/info02.png "Vyberte klíč osobních údajů")](security-privacy-images/info02.png#lightbox)
5. Zadejte popis proč aplikace požaduje přístup k dané informace o funkci nebo uživatele: 

    [![](security-privacy-images/info03.png "Zadejte popis")](security-privacy-images/info03.png#lightbox)
6. Uložte změny do souboru.

-----

> [!IMPORTANT]
> V příkladu výše, nepodařilo se nastavit uvedené `NSHomeKitUsageDescription` klíče v `Info.plist` souboru by způsobilo aplikace _bezobslužně chybě_ (dochází k uzavření systému za běhu) bez chyby při spuštění v iOS 10 (nebo vyšší).

<a name="Summary" />

## <a name="summary"></a>Souhrn

Tento článek má zahrnutých zabezpečení a ochrany osobních údajů změny, Apple má v iOS 10 a jejich vliv na aplikace Xamarin.iOS.



## <a name="related-links"></a>Související odkazy

- [iOS 10 – ukázky](https://developer.xamarin.com/samples/ios/iOS10/)
