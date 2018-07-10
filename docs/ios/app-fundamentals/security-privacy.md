---
title: Zabezpečení a ochrany osobních údajů funkce iOS
description: Tento dokument popisuje zabezpečení a ochrany osobních údajů funkce zařízení se systémy IOS a jak je používat s Xamarin.iOS. To se podíváme na aktualizace provedené v iOS 10 a jak přistupovat k datům uživatele.
ms.prod: xamarin
ms.assetid: 718C8721-C359-4650-878A-D68E159A3F53
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/16/2017
ms.openlocfilehash: 1a28bf394d29c09bfd264f03e0eea6c8b582f271
ms.sourcegitcommit: ec50c626613f2f9af51a9f4a52781129bcbf3fcb
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/05/2018
ms.locfileid: "37854750"
---
# <a name="ios-security-and-privacy-features"></a>Zabezpečení a ochrany osobních údajů funkce iOS

_Tento článek popisuje práci s zabezpečení a ochrana osobních údajů v iOS a způsob, jakým ovlivňují aplikace Xamarin.iOS._

Apple bylo provedeno několik vylepšení zabezpečení a ochrana osobních údajů v iOS 10 (nebo novější), které vám pomůžou developer zlepšit zabezpečení svých aplikací a zajištění ochrany osobních údajů koncového uživatele. Tento článek se zabývá implementaci těchto funkcí v aplikaci pro Xamarin.iOS.
    
<a name="General-Enhancements" />

## <a name="general-enhancements"></a>Obecná vylepšení

Následující obecné změny se provedly pro zabezpečení a ochrana osobních údajů v Iosu 10:

- Common Data zabezpečení architektura (CDSA) rozhraní API je zastaralá a by měla být nahrazena SecKey rozhraní API pro generování asymetrické klíče.
- Nové `NSAllowsArbitraryLoadsInWebContent` klíč můžete přidat do vaší aplikace **Info.plist** souboru a vám umožní načíst správně ochrany zabezpečení přenosu Apple (ATS) je stále povolen pro ostatní aplikace web pages. Další informace najdete v tématu naše [App Transport Security](~/ios/app-fundamentals/ats.md) dokumentaci.
- Protože nová schránky v iOS 10 a macOS Sierra umožňuje uživateli zkopírovat a vložit mezi zařízeními, rozšířila rozhraní API umožňující schránky omezena na konkrétní zařízení a časovým razítkem vymazání automaticky časovém okamžiku. Kromě toho pojmenované pracovních plochách už jsou trvalé a by měla být nahrazena sdíleným kontejnerům pracovní plochy.
- Pro všechna připojení SSL/TLS je teď ve výchozím nastavení zakázán symetrický šifrovací algoritmus RC4. Kromě toho rozhraní API pro zabezpečení Transport už nepodporuje protokol SSLv3 a doporučuje se, že vývojář přestat používat šifrování SHA-1 a 3DES co nejdříve.

<a name="Accessing-Private-User-Data" />

## <a name="accessing-private-user-data"></a>Přístup k uživatelským datům privátní

Aplikace pro iOS 10 (nebo novější) musí deklarovat staticky jejich cílem je přístup ke konkrétní funkce nebo informace o uživateli tak, že zadáte jeden nebo více klíčů o ochraně osobních údajů v jejich **Info.plist** soubory, které vysvětlují, proč se aplikace chce získat uživateli přístup.

> [!IMPORTANT]
> Aplikace, které nezvládají poskytovat požadované klíče tiše ukončí systému při pokusu o přístup k některé z funkcí s omezeným přístupem nebo informace o uživateli, _bez chyb_! Pokud se aplikace spustí neočekávaně služeb při selhání v systému iOS 10, ujistěte se, že všechny požadované **Info.plist** nebyly zadány.

Klíče jsou k dispozici související s následující o ochraně osobních údajů:

- **Soukromí – použití Apple Music popis** (`NSAppleMusicUsageDescription`) – umožňuje vývojářům popsat, proč chce dostat ke knihovně média uživatele aplikace.
- **Soukromí – popis použití periferií Bluetooth** (`NSBluetoothPeripheralUsageDescription`) – umožňuje vývojářům popsat, proč se chce, aby aplikace přístup k Bluetooth v zařízení uživatele.
- **Soukromí – popis použití kalendáře** (`NSCalendarsUsageDescription`) – umožňuje vývojářům popsat, proč aplikace požaduje přístup uživatele kalendáře.
- **Soukromí – popis použití fotoaparátu** (`NSCameraUsageDescription`) – umožňuje vývojářům popsat, proč aplikaci chce, aby se pro přístup k fotoaparátu zařízení.
- **Soukromí – popis použití kontaktů** (`NSContactsUsageDescription`) – umožňuje vývojářům popsat, proč se aplikace chce mít přístup ke kontaktům uživatele.
- **Soukromí – popis použití sdílení stavu** (`NSHealthShareUsageDescription`) – umožňuje vývojářům popsat, proč se chce, aby aplikace přístup k datům o stavu uživatele. Další informace najdete v tématu společnosti Apple [referenční třída HKHealthStore](https://developer.apple.com/reference/healthkit/hkhealthstore).
- **Soukromí – popis použití aktualizace stavu** (`NSHealthUpdateUsageDescription`) – umožňuje vývojářům popsat, proč se chce, aby aplikace upravit data stavu uživatele. Další informace najdete v tématu společnosti Apple [referenční třída HKHealthStore](https://developer.apple.com/reference/healthkit/hkhealthstore).
- **Soukromí – popis použití Homekitu** (`NSHomeKitUsageDescription`) – umožňuje vývojářům popsat, proč chce získat přístup k datům konfigurace oprávnění HomeKit uživatele aplikace.
- **Soukromí – vždy popis použití polohy** (`NSLocationAlwaysUsageDescription`) – umožňuje vývojářům popsat, proč se chce, aby aplikace měla vždycky přístup k umístění uživatele.
- [Zastaralé] **Soukromí – popis použití polohy** (`NSLocationUsageDescription`) – umožňuje vývojářům popsat, proč aplikaci chce, aby se pro přístup k umístění uživatele. *Poznámka: Tento klíč je zastaralá v iOS 8 (a vyšší). Použití `NSLocationAlwaysUsageDescription` nebo `NSLocationWhenInUseUsageDescription` místo.*
- **Soukromí – popis umístění. když v použití využití** (`NSLocationWhenInUseUsageDescription`) – umožňuje vývojářům popsat, proč se chce, aby aplikace přístup k poloze uživatele během jejího běhu.
- [Zastaralé] **Soukromí – popis použití knihovny médií** – umožňuje vývojářům popsat, proč aplikace požaduje přístup do knihovny médií pro daného uživatele. *Poznámka: Tento klíč je zastaralá v iOS 8 (a vyšší). Použití `NSAppleMusicUsageDescription` místo.*
- **Soukromí – popis použití mikrofonu** (`NSMicrophoneUsageDescription`) – umožňuje vývojářům popsat, proč se chce, aby aplikace přistupovat k mikrofonu. zařízení.
- **Soukromí – popis použití pohybu** (`NSMotionUsageDescription`) – umožňuje vývojářům popsat, proč aplikaci chce, aby se pro přístup k zařízení akcelerometr.
- **Soukromí – popis použití knihovny fotek** (`NSPhotoLibraryUsageDescription`) – umožňuje vývojářům popsat, proč aplikace požaduje přístup knihovny fotografií uživatele.
- **Soukromí – popis použití připomenutí** (`NSRemindersUsageDescription`) – umožňuje vývojářům popsat, proč aplikace požaduje přístup uživatele připomenutí.
- **Soukromí – popis použití Siri** (`NSSiriUsageDescription`) – umožňuje vývojářům popsat, proč aplikace chce uživatel data posílat Siri.
- **Soukromí – popis použití rozpoznávání řeči** (`NSSpeechRecognitionUsageDescription`) – umožňuje vývojářům popsat, proč se chce, aby aplikace odesílat data uživatele do společnosti Apple servery rozpoznávání řeči.
- **Soukromí – popis použití poskytovatele TV** (`NSVideoSubscriberAccountUsageDescription`) – umožňuje vývojářům popsat, proč aplikaci chce, aby se pro přístup k uživatelskému účtu poskytovatele TV.

Další informace o práci s **Info.plist** klíčů, získáte od společnosti Apple [informace vlastnost seznamu Key Reference](https://developer.apple.com/library/content/documentation/General/Reference/InfoPlistKeyReference/Introduction/Introduction.html#//apple_ref/doc/uid/TP40009248-SW1).

<a name="Setting-Privacy-Keys" />

## <a name="setting-privacy-keys"></a>Nastavení ochrany osobních údajů klíče

Přijmout na následující příklad přístup k oprávnění HomeKit na iOS 10 (nebo novější), vývojář bude muset přidat `NSHomeKitUsageDescription` klíče aplikace **Info.plist** souboru a zadejte řetězec deklarace, proč aplikace požaduje přístup uživatele HomeKit databáze. Do doby uživatele při prvním spuštění aplikace se zobrazí tento řetězec:

![Představuje příklad upozornění NSHomeKitUsageDescription](security-privacy-images/info01.png "NSHomeKitUsageDescription představuje příklad upozornění")

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Aktuálně Xamarin.iOS pro Visual Studio nepodporuje úpravy **Info.plist** editoru manifestu klíče o ochraně osobních údajů z ve výchozím nastavení iOS. Místo toho budete muset použít obecný editor PList, takže postupujte takto:

1. Klikněte pravým tlačítkem na **Info.plist** soubor **Průzkumníka řešení** a vyberte **otevřít v programu...** .
2. Vyberte **obecný Editor PList** ze seznamu programů k otevření souboru, potom klikněte na tlačítko **OK**.

    ![Vyberte Editor PList obecný](security-privacy-images/InfoEditorSelectionVs.png "vyberte Editor obecného souboru PList")
3. Klikněte na tlačítko **+** tlačítko na poslední řádek v editoru a přidejte novou položku do seznamu. To bude volat "Vlastní vlastnost" s nastaveným typem `String` a prázdnou hodnotu.
4. Klikněte na název vlastnosti a zobrazí se rozevírací seznam.
5. Z rozevíracího seznamu vyberte klíč osobních údajů (například **soukromí – popis použití Homekitu**): 

    ![Vyberte klíč osobních údajů](security-privacy-images/InfoPListEditorSelectKey.png "vyberte klíč osobních údajů")
6. Zadejte popis do sloupci Hodnota proč aplikaci chce, aby se pro přístup k dané informace o funkci nebo uživateli: 

    ![Zadejte popis](security-privacy-images/InfoPListSetValue.png "zadejte popis")
7. Uložte změny do souboru.

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

Pokud chcete nastavit některé z klíče o ochraně osobních údajů, postupujte takto:

1. Dvakrát klikněte **Info.plist** ve **Průzkumníka řešení** otevřete pro úpravy.
2. V dolní části obrazovky, přepněte **zdroj** zobrazení.
3. Přidat nový **položka** do seznamu.
4. Z rozevíracího seznamu vyberte klíč osobních údajů (například **soukromí – popis použití Homekitu**): 

    ![Vyberte klíč osobních údajů](security-privacy-images/info02.png "vyberte klíč osobních údajů")
5. Zadejte popis proč aplikaci chce, aby se pro přístup k dané informace o funkci nebo uživateli: 

    ![Zadejte popis](security-privacy-images/info03.png "zadejte popis")
6. Uložte změny do souboru.

-----

> [!IMPORTANT]
> V příkladu výše, nepodařilo se nastavit uvedené `NSHomeKitUsageDescription` klíče v **Info.plist** souboru by mělo za následek aplikace _tiše neúspěšné_ (zavírá systému za běhu) bez chyb při spuštění v iOS 10 ( nebo vyšší).

<a name="Summary" />

## <a name="summary"></a>Souhrn

V tomto článku zahrnují zabezpečení a ochrany osobních údajů změny, Apple má v iOS 10 a způsob, jakým ovlivňují aplikace Xamarin.iOS.

## <a name="related-links"></a>Související odkazy

- [Ukázky pro iOS 10](https://developer.xamarin.com/samples/ios/iOS10/)
