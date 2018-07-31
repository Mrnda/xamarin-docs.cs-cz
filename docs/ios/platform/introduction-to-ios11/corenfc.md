---
title: Základní NFC v Xamarin.iosu
description: Tento dokument popisuje, jak číst téměř pole značky komunikace v Xamarin.iOS pomocí rozhraní API zavedené v Iosu 11.
ms.prod: xamarin
ms.technology: xamarin-ios
ms.assetid: 846B59D3-F66A-48F3-A78C-84217697194E
author: bradumbaugh
ms.author: brumbaug
ms.date: 09/25/2017
ms.openlocfilehash: 1381a4564f93fd091f181949454df3f06b31ae6b
ms.sourcegitcommit: aa9b9b203ab4cd6a6b4fd51e27d865e2abf582c1
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/30/2018
ms.locfileid: "39350830"
---
# <a name="core-nfc-in-xamarinios"></a>Základní NFC v Xamarin.iosu

_Čtení téměř Field Communication (NFC) značky pomocí iOS 11_

CoreNFC je novou architekturou v Iosu 11, který poskytuje přístup k _Bezkontaktní komunikace_ přepínač (NFC) ke čtení značek z v rámci aplikace. Funguje na zařízeních iPhone 7, Plus 7, 8, 8, Plus a X.

Čtečka značky NFC v zařízení s Iosem podporuje všechny typy značek NFC 1 až 5, které obsahují _formát výměny dat NFC_ informace (NDEF).

Existují některá omezení je potřeba vědět:

- CoreNFC podporuje pouze značky čtení (nikoli zápis nebo formátování).
- Značka kontroly musí být spuštěna uživatelem a časový limit po 60 sekund.
- Aplikace musí být viditelný v popředí ke skenování.
- CoreNFC lze pouze testovat na skutečných zařízeních (ne na simulátoru).

Tato stránka popisuje konfigurace požadované pro použití CoreNFC a ukazuje, jak použít rozhraní API pomocí ["TFCTagReader" ukázkový kód](https://developer.xamarin.com/samples/monotouch/ios11/NFCTagReader/).

## <a name="configuration"></a>Konfigurace

Pokud chcete povolit CoreNFC, je nutné nakonfigurovat tři položky ve vašem projektu:

- **Info.plist** klíč osobních údajů.
- **Do souboru Entitlements.plist** položka.
- Zřizovací profil s **čtení značek NFC** funkce.

### <a name="infoplist"></a>Info.plist

Přidat **NFCReaderUsageDescription** klíč osobních údajů a text, který se zobrazí uživatelům během prohledávání. Použít zprávu o vhodném pro vaši aplikaci (například popisují účel vyhledávání):

```xml
<key>NFCReaderUsageDescription</key>
<string>NFC tag to read NDEF messages into the application</string>
```

### <a name="entitlementsplist"></a>Do souboru Entitlements.plist

Musíte požádat o vaši aplikaci **téměř čtení značek komunikace pole** spárovat se schopností pomocí následující klíč/hodnota v vaše **do souboru Entitlements.plist**:

```xml
<key>com.apple.developer.nfc.readersession.formats</key>
<array>
  <string>NDEF</string>
</array>
```

### <a name="provisioning-profile"></a>Zřizovací profil

Vytvořte nový **ID aplikace** a ujistěte se, že **čtení značek NFC** služby je zaškrtnuté:

[![Stránka nového ID aplikace portálu pro vývojáře s čtení značek NFC vybrané](corenfc-images/app-services-nfc-sml.png)](corenfc-images/app-services-nfc.png#lightbox)

Můžete by pak vytvoření nového zřizovacího profilu pro toto ID aplikace, stáhněte a nainstalujte ho na vašem vývojovém počítači Mac.

## <a name="reading-a-tag"></a>Čtení značku

Po nakonfigurování vašeho projektu přidat `using CoreNFC;` do horní části souboru a postupujte podle tyto tři kroky pro implementaci NFC značky čtení funkcí:

### <a name="1-implement-infcndefreadersessiondelegate"></a>1. Implementace `INFCNdefReaderSessionDelegate`

Rozhraní má dvě metody k implementaci:

- `DidDetect` – Volána, když je úspěšné načtení značky.
- `DidInvalidate` – Volána, když dojde k chybě nebo je dosaženo 60 druhý časový limit.

#### <a name="diddetect"></a>DidDetect

Ve vzorovém kódu přidá do zobrazení tabulky, každý naskenované zpráva:

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

Tato metoda může být volána více než jednou (a pole zpráv, které je možné předat v) Pokud relace umožňuje více čtení značek. To se nastavuje pomocí třetího parametru `Start` – metoda (podrobně [kroku 2](#step2)).

#### <a name="didinvalidate"></a>DidInvalidate

Neplatnost může dojít z několika důvodů:

- Během hledání došlo k chybě.
- Aplikace, přestanou být aktivní.
- Uživatel se rozhodl zrušit vyhledávání.
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

Jakmile platnost relace byla zrušena, musí být vytvořený objekt novou relaci skenovat znovu.

<a name="step2" />

### <a name="2-start-an-nfcndefreadersession"></a>2. Spuštění `NFCNdefReaderSession`

Vyhledávání by měl začínat uživatelského požadavku, jako je například stisknutí tlačítka.
Následující kód vytvoří a spustí skenování relace:

```csharp
Session = new NFCNdefReaderSession(this, null, true);
Session?.BeginSession();
```

Parametry `NFCNdefReaderSession` konstruktoru jsou následující:

- `delegate` – Implementace `INFCNdefReaderSessionDelegate`. Ve vzorovém kódu, delegátu je implementována v kontroleru zobrazení tabulky, proto `this` slouží jako parametr delegátu.
- `queue` – Fronty, která jsou zpracovány zpětná volání. Může to být `null`, v takovém případě je potřeba použít `DispatchQueue.MainQueue` při aktualizaci ovládacích prvků uživatelského rozhraní (jak je znázorněno v ukázce).
- `invalidateAfterFirstRead` – Když `true`, zastaví prohledávání po první úspěšná kontrola; při `false` vyhledávání bude pokračovat a vrátil více výsledků dokud kontroly se zrušila, nebo je dosaženo 60 druhý časový limit.


### <a name="3-cancel-the-scanning-session"></a>3. Zrušit vyhledávání relace

Uživatele můžete zrušit skenování relace přes poskytnuté systémem tlačítko v uživatelském rozhraní:

![Tlačítko Storno. při hledání](corenfc-images/scan-cancel-sml.png)

Aplikace může programově zrušit kontroly voláním `InvalidateSession` metody:

```csharp
Session.InvalidateSession();
```

V obou případech delegáta pro `DidInvalidate` metoda bude volána.

## <a name="summary"></a>Souhrn

CoreNFC umožňuje aplikaci číst data ze značky NFC. Podporuje čtení širokou škálu formátů značky (typy NDEF 1 až 5), ale nepodporuje zápis nebo formátování.


## <a name="related-links"></a>Související odkazy

- [NFCTagReader (ukázka)](https://developer.xamarin.com/samples/monotouch/ios11/NFCTagReader/)
- [Úvod do jádra NFC (WWDC) (video)](https://developer.apple.com/videos/play/wwdc2017/718/)
