---
title: Základní NFC v Xamarin.iOS
description: Tento dokument popisuje, jak číst téměř pole značky komunikace v Xamarin.iOS pomocí rozhraní API byla zavedená v iOS 11.
ms.prod: xamarin
ms.technology: xamarin-ios
ms.assetid: 846B59D3-F66A-48F3-A78C-84217697194E
author: bradumbaugh
ms.author: brumbaug
ms.date: 09/25/2016
ms.openlocfilehash: c42048f9c00238fb73e354ea86322c3d19bae601
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 06/05/2018
ms.locfileid: "34787374"
---
# <a name="core-nfc-in-xamarinios"></a>Základní NFC v Xamarin.iOS

_Značky čtení komunikace NFC (Near Field) pomocí iOS 11_

CoreNFC je novou architekturou v iOS 11, která poskytuje přístup k _Bezkontaktní komunikace_ přepínač (NFC) ke čtení značky z v rámci aplikací. Funguje na zařízeních iPhone 7, Plus 7, 8, 8 Plus a X.

Čtečka značky NFC v zařízení s iOS podporuje všechny typy značek NFC 1 až 5, které obsahují _formát Exchange dat NFC_ informace (NDEF).

Existují některá omezení zajímat:

- CoreNFC podporuje pouze značky čtení (ne zápis nebo formátování).
- Prohledávání značky musí být spuštěna uživatelem a časový limit po 60 sekund.
- Aplikace musí být viditelná v popředí pro skenování.
- CoreNFC můžete otestovat pouze na skutečné zařízení (ne v simulátoru).

Tato stránka popisuje konfigurace požadované pro použití CoreNFC a ukazuje, jak používat rozhraní API pomocí ["TFCTagReader" ukázkový kód](https://developer.xamarin.com/samples/monotouch/ios11/NFCTagReader/).

## <a name="configuration"></a>Konfigurace

Pokud chcete povolit CoreNFC, musíte nakonfigurovat tři položky ve vašem projektu:

- **Info.plist** klíč osobních údajů.
- **Entitlements.plist** položku.
- Profil zřizování s **čtení značky NFC** schopností.

### <a name="infoplist"></a>Info.plist

Přidat **NFCReaderUsageDescription** klíč osobních údajů a text, který se zobrazí uživatelům během prohledávání je. Použít zprávu vhodné pro vaši aplikaci (například vysvětlují účelem kontroly):

```xml
<key>NFCReaderUsageDescription</key>
<string>NFC tag to read NDEF messages into the application</string>
```

### <a name="entitlementsplist"></a>Entitlements.plist

Aplikace musí požádat **téměř pole komunikace značky čtení** spárujte schopností pomocí následující klíč/hodnota v vaše **Entitlements.plist**:

```xml
<key>com.apple.developer.nfc.readersession.formats</key>
<array>
  <string>NDEF</string>
</array>
```

### <a name="provisioning-profile"></a>Profil pro zřizování

Vytvořte novou **ID aplikace** a ujistěte se, že **čtení značky NFC** je zaškrtnuté služby:

[![Stránka nové ID aplikace portálu vývojáře s čtení značky NFC vybrané](corenfc-images/app-services-nfc-sml.png)](corenfc-images/app-services-nfc.png#lightbox)

Jste měli pak vytvořte nový profil pro zřizování pro toto ID aplikace, pak stáhněte a nainstalujte na váš vývojový Mac.

## <a name="reading-a-tag"></a>Čtení značku

Po nakonfigurování projektu přidejte `using CoreNFC;` na začátek souboru a postupujte podle značky tyto tři kroky pro implementaci NFC čtení funkce:

### <a name="1-implement-infcndefreadersessiondelegate"></a>1. Implementace `INFCNdefReaderSessionDelegate`

Rozhraní má dvě metody k implementaci:

- `DidDetect` – Voláno, když se značkou číst.
- `DidInvalidate` – Voláno, když dojde k chybě nebo je dosaženo 60 druhý časový limit.

#### <a name="diddetect"></a>DidDetect

V ukázkovém kódu je přidána každý naskenované zpráva zobrazení tabulky:

```csharp
public void DidDetect(NFCNdefReaderSession session, NFCNdefMessage[] messages)
{
    foreach (NFCNdefMessage msg in messages)
    {  // adds the messages to a list view
        DetectedMessages.Add(msg);
    }
    DispatchQueue.MainQueue.DispatchAsync(() =>
    {
        this.TableView.ReloadData();
    });
}
```

Tato metoda může volat vícekrát, (a může být předán pole zpráv) Pokud relace umožňuje pro vícenásobné čtení značky. Tento vztah se nastavuje pomocí třetího parametru `Start` – metoda (podrobně [krok 2](#step2)).

#### <a name="didinvalidate"></a>DidInvalidate

Zneplatnění může dojít z několika příčin:

- Došlo k chybě při kontrole.
- Aplikace, přestanou být v popředí.
- Uživatel se rozhodl zrušit kontroly.
- Kontrola byla zrušena aplikací.

Následující kód ukazuje, jak zpracovat chybu:

```csharp
public void DidInvalidate(NFCNdefReaderSession session, NSError error)
{
    var readerError = (NFCReaderError)(long)error.Code;
    if (readerError != NFCReaderError.ReaderSessionInvalidationErrorFirstNDEFTagRead &&
        readerError != NFCReaderError.ReaderSessionInvalidationErrorUserCanceled)
    {
      // some error handling
    }
}
```

Jakmile platnost relace byla zrušena, musí být vytvořený objekt novou relaci Prohledat znovu.

<a name="step2" />

### <a name="2-start-an-nfcndefreadersession"></a>2. Spuštění `NFCNdefReaderSession`

Kontrola by měla začínat znakem požadavku uživatele, například stisknutí tlačítka.
Následující kód vytvoří a spustí relaci prohledávání:

```csharp
Session = new NFCNdefReaderSession(this, null, true);
Session?.BeginSession();
```

Parametry `NFCNdefReaderSession` konstruktor jsou následující:

- `delegate` – Implementace `INFCNdefReaderSessionDelegate`. V ukázkovém kódu, delegát je implementovaná v kontroleru zobrazení tabulky, proto `this` slouží jako parametr delegáta.
- `queue` – Fronta, kterou jsou zpracovány zpětných volání. Může být `null`, v takovém případě je nutné používat `DispatchQueue.MainQueue` při aktualizaci ovládacích prvků uživatelského rozhraní (jak je znázorněno v ukázce).
- `invalidateAfterFirstRead` – Když `true`, kontrola přestane po prvním úspěšném kontroly; při `false` kontrolu bude pokračovat a vrátit více výsledky, dokud nebude kontrola byla zrušena, nebo je dosaženo 60 druhý časový limit.


### <a name="3-cancel-the-scanning-session"></a>3. Zrušit prohledávání relace

Uživatele můžete zrušit prohledávání relace pomocí tlačítka poskytované systémem v uživatelském rozhraní:

![Tlačítko Zrušit při prohledávání](corenfc-images/scan-cancel-sml.png)

Aplikace můžete programově zrušit kontroly voláním `InvalidateSession` metoda:

```csharp
Session.InvalidateSession();
```

V obou případech delegáta pro `DidInvalidate` volání metody.

## <a name="summary"></a>Souhrn

CoreNFC umožňuje aplikaci číst data z značky NFC. Podporuje čtení různých formátech značky (NDEF typy 1 až 5), ale nepodporuje zápis nebo formátování.


## <a name="related-links"></a>Související odkazy

- [NFCTagReader (ukázka)](https://developer.xamarin.com/samples/monotouch/ios11/NFCTagReader/)
- [Představení NFC jádra (WWDC) (video)](https://developer.apple.com/videos/play/wwdc2017/718/)
