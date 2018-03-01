---
title: "Poradce při potížích"
description: "Známé problémy s Xamarin přehrávač za provozu a jejich řešení."
ms.topic: article
ms.prod: xamarin
ms.assetid: 29A97ADA-80E0-40A1-8B26-C68FFABE7D26
ms.technology: xamarin-cross-platform
author: topgenorth
ms.author: toopge
ms.date: 05/17/2017
ms.openlocfilehash: d7c5bedb03d7c869be65e3c704bac58a9cdfcbbd
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 02/27/2018
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

Tato chyba je často došlo, pokud mobilní zařízení se systémem Xamarin Live Player není ve stejné síti jako počítač spuštěný IDE; často se to stane, když připojení k zařízení, který byl dříve úspěšně spárovat.

* Zkontrolujte, jestli zařízení i počítače jsou ve stejné síti Wi-Fi.
* Síť může být úzce zabezpečené (například některých podnikových sítích), blokování porty vyžadované Player Xamarin za provozu. Následující porty jsou povinné pro Xamarin přehrávač za provozu:
  * 37847 – přístup k interní síti 
  * 8090 – přístup k externí síti

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
