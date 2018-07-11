---
title: Řešení potíží s Xamarin Live Player
description: Tento dokument popisuje známé problémy s Xamarin Live Player a možné opravy. Popisuje problémy s připojením, problémy s konfigurací a další.
ms.prod: xamarin
ms.assetid: 29A97ADA-80E0-40A1-8B26-C68FFABE7D26
author: topgenorth
ms.author: toopge
ms.date: 05/17/2017
ms.openlocfilehash: 3db14db2c64e024ef1c04275661f610f9407dfb7
ms.sourcegitcommit: 632955f8cdb80712abd8dcc30e046cb9c435b922
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2018
ms.locfileid: "38831329"
---
# <a name="troubleshooting-xamarin-live-player"></a>Řešení potíží s Xamarin Live Player

![Funkce ve verzi Preview](~/media/shared/preview.png)

Tento článek popisuje některé běžné problémy a vysvětluje, jak je opravit.

## <a name="mobile-device-does-not-connect-after-scanning-barcode-or-entering-code"></a>Mobilní zařízení se nepřipojuje po skenování čárových kódů (nebo zadávání kódu)

Nastane, pokud mobilní zařízení s Xamarin Live Player není ve stejné síti jako počítač se spuštěným integrovaného vývojového prostředí. Projděte si následující:

- Ověřte, zda zařízení a počítače ve stejné síti Wi-Fi.
  - Pokud počítač je také připojený k drátové síti, zkuste odpojit drátového připojení.
- Síť může být úzce zabezpečené (například některých podnikových sítích), blokují porty vyžadované Xamarin Live Player.
- Aplikace Xamarin Live Player zavřete a restartujte ji.

## <a name="error-while-trying-to-deploy-message-in-ide"></a>Zpráva "Chyba při pokusu o nasazení" v integrovaném vývojovém prostředí

**"IOException: Nelze číst data z přenosového připojení: operace na neblokujícím soketu by blokovaly"**

K této chybě dochází často Pokud mobilní zařízení s Xamarin Live Player není ve stejné síti jako služba počítači se systémem Visual Studio; této situaci často dochází při připojování k zařízení, která dříve byla úspěšně spárovat.

* Zkontrolujte, jestli zařízení i počítače jsou ve stejné síti Wi-Fi.
* Síť může být úzce zabezpečené (například některých podnikových sítích), blokují porty vyžadované Xamarin Live Player. Tyto porty jsou povinné pro Xamarin Live Player:
  * 37847 – přístup k síti interní 
  * 8090 – externí síťový přístup

## <a name="manually-configure-device"></a>Ruční konfigurace zařízení

Pokud se nemůžete připojit přes Wi-Fi na zařízení se můžete pokusit ručně nakonfigurovat své zařízení prostřednictvím konfiguračního souboru pomocí následujících kroků:

**Krok 1: Otevřete konfigurační soubor**

Přejděte do složky dat aplikací:

* Windows: **%userprofile%\AppData\Roaming**
* macOS: **~/Users/$USER/.config**

V této složce najdete **PlayerDeviceList.xml** neexistuje bude potřeba ho vytvořit.

**Krok 2: Získání IP adresy**

Přejděte do aplikace Xamarin Live Player **o > Test připojení > spustit Test připojení**.

Poznamenejte si IP adresy, budete potřebovat IP adresu uvedené při konfiguraci zařízení.

**Krok 3: Získání párování kódu**

V přehrávači Xamarin Live Player tap **pár** nebo **pár znovu**, stiskněte klávesu **zadejte ručně**. Číselný kód se zobrazí, které budete muset aktualizovat konfigurační soubor.

**Krok 4: Generování identifikátoru GUID**

Přejděte na: https://www.guidgenerator.com/online-guid-generator.aspx a vygenerujte nový identifikátor guid a ujistěte se, že je velká písmena na.

**Krok 5: Konfigurace zařízení**

Otevřete **PlayerDeviceList.xml** nahoru v editoru, jako je Visual Studio nebo Visual Studio Code. Budete muset ručně konfigurovat vaše zařízení v tomto souboru. Ve výchozím nastavení, soubor musí obsahovat následující prázdný `Devices` – element XML:

```xml
<DeviceList xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns:xsd="http://www.w3.org/2001/XMLSchema">
<Devices>

</Devices>
</DeviceList>
```

**Přidejte zařízení iOS:**

```xml
<PlayerDevice>
<SecretCode>ENTER-PAIR-CODE-HERE</SecretCode>
<UniqueIdentifier>ENTER-GUID-HERE</UniqueIdentifier>
<Name>iPhone Player</Name>
<Platform>iOS</Platform>
<AndroidApiLevel>0</AndroidApiLevel>
<DebuggerEndPoint>ENTER-IP-HERE:37847</DebuggerEndPoint>
<HostEndPoint />
<NeedsAppInstall>false</NeedsAppInstall>
<IsSimulator>false</IsSimulator>
<SimulatorIdentifier />
<LastConnectTimeUtc>2018-01-08T20:36:03.9492291Z</LastConnectTimeUtc>
</PlayerDevice>
```

**Přidejte zařízení s Androidem:**

```xml
<PlayerDevice>
<SecretCode>ENTER-PAIR-CODE-HERE</SecretCode>
<UniqueIdentifier>ENTER-GUID-HERE</UniqueIdentifier>
<Name>Android Player</Name>
<Platform>Android</Platform>
<AndroidApiLevel>24</AndroidApiLevel>
<DebuggerEndPoint>ENTER-IP-HERE:37847</DebuggerEndPoint>
<HostEndPoint />
<NeedsAppInstall>false</NeedsAppInstall>
<IsSimulator>false</IsSimulator>
<SimulatorIdentifier />
<LastConnectTimeUtc>2018-01-08T20:34:42.2332328Z</LastConnectTimeUtc>
</PlayerDevice>
```

**Zavřete a znovu otevřete Visual Studio.** Vaše zařízení měla zobrazit v seznamu.

## <a name="type-or-namespace-cannot-be-found-message-in-ide"></a>Zpráva "nelze nalézt typ nebo obor názvů" v integrovaném vývojovém prostředí

Zkontrolujte, zda jste vybrali **spouštěný projekt** , který odpovídá typu vašeho zařízení (iOS nebo Android) a že v konfiguraci odpovídá typu zařízení (např.) **Ladění | iPhone simulátor** pro iOS).

## <a name="constructor-on-type-interpretedxamarinformsbutton-not-found-message-in-player"></a>Zpráva "Konstruktor typu nebyl nalezen InterpretedXamarin.Forms.Button" v přehrávači

Některé systémové třídy nelze přepsat, třeba:

```csharp
public class SomeCustomButton : Xamarin.Forms.Button { ... }
```

## <a name="mainactivitycs-resourcelayout-does-not-contain-a-definition-for-main"></a>"MainActivity.cs: 'Resource.Layout' neobsahuje definici pro 'Main'"

Tato chyba vyskytuje v uživatelských rozhraní, které jsou definovány v souborech AXML pro projekty pro Android.
AXML soubory nejsou aktuálně podporované v přehrávači Xamarin Live Player.

### <a name="android-toolbar-and-tabs-render-incorrectly-using-xamarinforms"></a>Android nástrojů a karty vykreslit nesprávně pomocí Xamarin.Forms

Projekty Xamarin.Forms Android musí používat "Toolbar.axml" a "Tabbar.axml" pro názvy souborů relevantní rozložení. Tyto názvy; používá výchozí šablony Přejmenování je způsobí problémy s vykreslováním.

Ohlaste prosím všechny další problémy ve [bugzilla](https://aka.ms/live-player-report-issue).

## <a name="related-links"></a>Související odkazy

- [Omezení](~/tools/live-player/limitations.md)
- [Instalace a nastavení](~/tools/live-player/install.md)
