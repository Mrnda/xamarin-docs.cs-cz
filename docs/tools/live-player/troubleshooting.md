---
title: Poradce při potížích
description: Známé problémy s Xamarin přehrávač za provozu a jejich řešení.
ms.topic: article
ms.prod: xamarin
ms.assetid: 29A97ADA-80E0-40A1-8B26-C68FFABE7D26
ms.technology: xamarin-cross-platform
author: topgenorth
ms.author: toopge
ms.date: 05/17/2017
ms.openlocfilehash: ab075cad0c3f3456ed23f3eb175dcdb3aa493510
ms.sourcegitcommit: 17a9cf246a4d33cfa232016992b308df540c8e4f
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 03/29/2018
---
# <a name="troubleshooting"></a>Poradce při potížích

![Funkce ve verzi Preview](~/media/shared/preview.png)

Tento článek popisuje některé běžné problémy a poskytuje postup opravte je.


## <a name="mobile-device-does-not-connect-after-scanning-barcode-or-entering-code"></a>Mobilní zařízení není připojit po skenování čárového kódu (nebo vstup kódu)

Nastane, když mobilní zařízení se systémem Xamarin Live Player není ve stejné síti jako počítač spuštěný rozhraní IDE. Podívejte se na následující:

- Potvrďte, že zařízení a počítače jsou ve stejné síti Wi-Fi.
  - Pokud počítač je také připojený k drátové síti, zkuste odpojit drátového připojení.
- Síť může být úzce zabezpečené (například některých podnikových sítích), blokování porty vyžadované Player Xamarin za provozu.
- Zavřete aplikaci Xamarin Player za provozu a restartujte ji.


## <a name="error-while-trying-to-deploy-message-in-ide"></a>Zpráva "Chyba při pokusu o nasazení" v integrovaném vývojovém prostředí

**"IOException: Nelze číst data z připojení pro přenos: operace soketem neblokující by blokovat"**

Tato chyba je často došlo, pokud mobilní zařízení se systémem Xamarin Live Player není ve stejné síti jako počítač, ke spuštění sady Visual Studio; často se to stane, když připojení k zařízení, který byl dříve úspěšně spárovat.

* Zkontrolujte, jestli zařízení i počítače jsou ve stejné síti Wi-Fi.
* Síť může být úzce zabezpečené (například některých podnikových sítích), blokování porty vyžadované Player Xamarin za provozu. Následující porty jsou povinné pro Xamarin přehrávač za provozu:
  * 37847 – přístup k interní síti 
  * 8090 – přístup k externí síti

## <a name="manually-configure-device"></a>Ručně nakonfigurovat nastavení zařízení

Pokud se nemůže připojit k zařízení přes Wi-Fi můžete zkusit ruční konfigurace do zařízení pomocí konfiguračního souboru pomocí následujících kroků:

**Krok 1: Otevřete konfigurační soubor**

Přejděte do složky data aplikací:

* Windows: **%userprofile%\AppData\Roaming**
* macOS: **~/Users/$USER/.config**

V této složce zjistíte **PlayerDeviceList.xml** Pokud neexistuje, je nutné ji vytvořit.

**Krok 2: Získání IP adresy**

V aplikaci Xamarin Live Player, přejděte na **o > Test připojení > spustit Test připojení**.

Všimněte si IP adresy, je nutné s IP adresou uvedenou při konfiguraci zařízení.

**Krok 3: Získání párování kódu**

Uvnitř klepnutí Xamarin Live Player **pár** nebo **pár znovu**, stiskněte **zadejte ručně**. Číselný kód se zobrazí, které budete muset aktualizovat konfigurační soubor.

**Krok 4: Generovat identifikátor GUID**

Přejděte na: https://www.guidgenerator.com/online-guid-generator.aspx a vygenerovat nový identifikátor guid a zkontrolujte, zda je velká na.


**Krok 5: Konfigurace zařízení**

Otevřete **PlayerDeviceList.xml** až v editoru, jako je například Visual Studio nebo Visual Studio Code. Budete muset ručně nakonfigurovat zařízení v tomto souboru. Ve výchozím nastavení, soubor by měl obsahovat následující prázdný `Devices` – element XML:

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

**Zavřete a znovu otevřete Visual Studio.** Zařízení musí zobrazí v seznamu.


## <a name="type-or-namespace-cannot-be-found-message-in-ide"></a>Zpráva "nelze nalézt typ nebo oboru názvů" v integrovaném vývojovém prostředí

Zkontrolujte, zda jste vybrali **spouštěný projekt** který odpovídá typu vašeho zařízení (iOS nebo Android) a že odpovídá konfiguraci typu zařízení (např. **Ladění | iPhone simulátoru** pro iOS).

## <a name="constructor-on-type-interpretedxamarinformsbutton-not-found-message-in-player"></a>Zpráva "Konstruktor typu InterpretedXamarin.Forms.Button nebyla nalezena" v Player

Nebylo možné přepsat některé třídy systému, například:

```csharp
public class SomeCustomButton : Xamarin.Forms.Button { ... }
```

## <a name="mainactivitycs-resourcelayout-does-not-contain-a-definition-for-main"></a>"MainActivity.cs: 'Resource.Layout' neobsahuje definici"Hlavní""

Tato chyba vyskytuje v uživatelského rozhraní definované v souborech AXML pro Android projekty.
AXML soubory nejsou aktuálně podporované v za provozu Player Xamarin.

### <a name="android-toolbar-and-tabs-render-incorrectly-using-xamarinforms"></a>Android panelu nástrojů a karty vykreslit nesprávně pomocí Xamarin.Forms

Projekty Xamarin.Forms Android musí používat "Toolbar.axml" a "Tabbar.axml" pro názvy souborů relevantní rozložení. Výchozí šablona používá tyto názvy; Přejmenování je způsobí, že vykreslování problémy.


Nahlaste všechny další problémy na [bugzilla](https://aka.ms/live-player-report-issue).


## <a name="related-links"></a>Související odkazy

- [Omezení](~/tools/live-player/limitations.md)
- [Instalace a nastavení](~/tools/live-player/install.md)
