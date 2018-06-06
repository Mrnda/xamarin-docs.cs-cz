---
title: Dotykový identifikátor na watchOS v Xamarinu
description: Tento článek se zabývá rozšířením Apple má provést dotykový identifikátor ve watchOS 3 a jejich implementaci Xamarin.iOS pro Apple Watch.
ms.prod: xamarin
ms.assetid: 32FF5D21-C252-485D-83AC-A7E592237962
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/17/2017
ms.openlocfilehash: 75d660ad0699b6fac3b1ae43046f322f380872b3
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 06/05/2018
ms.locfileid: "34791072"
---
# <a name="apple-pay-on-watchos-in-xamarin"></a>Dotykový identifikátor na watchOS v Xamarinu

Apple udělal několik vylepšení Apple platit v watchOS 3, která přidává podporu pro plateb v aplikaci. To umožňuje uživatelům bezpečně zadejte platby a kontaktní informace přímo z Apple Watch platit pro fyzické zboží a služeb.


## <a name="about-apple-pay-enhancements"></a>O vylepšení platím Apple

Jako Stated výše Apple udělal několik vylepšení Apple platit v watchOS 3, která povolit zabezpečené platby a kontaktní informace přímo z Apple Watch platit pro fyzické zboží a služeb. Tato vylepšení jsou poskytovány úpravy PassKit framework.

IOS 10 a watchOS 3 několik nových rozhraní API přidané které pracují s iOS a watchOS pro podporu dynamické platebních sítí a nové testovacím prostředí izolovaného prostoru.

## <a name="passkit-framework-enhancements"></a>Vylepšení PassKit Framework

V iOS 10 rozhraní PassKit rozšířila na podporu dotykový identifikátor mimo `UIKit` a umožnit karty vystavitelů jejich karet z v rámci své aplikace k dispozici. 

### <a name="supporting-apple-pay-outside-of-uikit"></a>Podpora Apple platím mimo UIKit

Pomocí [PKPaymentAuthorizationController](https://developer.apple.com/reference/passkit/pkpaymentauthorizationcontroller) a [PKPaymentAuthorixationControllerDelegate](https://developer.apple.com/reference/passkit/pkpaymentauthorizationcontrollerdelegate), aplikace může podporovat stejné funkce poskytované službou [ PKPaymentAuthorizationViewController](https://developer.apple.com/reference/passkit/pkpaymentauthorizationviewcontroller) bez použití UIKit. Toto nové rozhraní API je potřeba pro podporu dotykový identifikátor v Apple Watch (a v konkrétních tříd Intent), je volitelné v jiných situacích (třeba existující aplikace). Apple navrhuje Přesun do nového rozhraní API co nejdříve poskytuje tak podporu dotykový identifikátor ve všech pro vývojáře aplikací s jednotného kódu. Další informace o tříd Intent a integrace Siri, najdete v tématu naše [Úvod do SiriKit](~/ios/platform/sirikit/index.md) dokumentaci.

### <a name="presenting-issuer-cards-from-within-apps"></a>Prezentace vystavitele z karty v rámci aplikací

IOS 10 a watchOS 3 nové funkce přidané PassKit rozhraní, které umožňují karty vystavitelů jejich platební karty z v rámci svých vlastních aplikací k dispozici. Můžete přidat na vývojáře `PKPaymentButtonTypeInStore` UIButton pro uživatelské rozhraní aplikace, které se zobrazí tlačítko dotykový identifikátor pro kartu.

`PresentPaymentPass` Metodu [PKPassLibrary](https://developer.apple.com/reference/passkit/pkpasslibrary) třídy lze také pro zobrazení karty prostřednictvím kódu programu.

## <a name="new-payment-network-support"></a>Nová podpora platebních sítě

Nové iOS 10 a watchOS 3 aplikace může automaticky podporují novou síť platebních až bude k dispozici bez vývojáře museli změnit, znovu zkompiluje aplikace a odešlete ji znovu k obchodu s aplikacemi.

Nové [AvailableNetworks](https://developer.apple.com/reference/passkit/pkpaymentrequest/1833288-availablenetworks) metodu `PKPaymentNetwork` třída umožňuje aplikaci, ke zjišťování sítě, které jsou k dispozici na zařízení uživatele za běhu. Kromě toho [SupportedNetworks](https://developer.apple.com/reference/passkit/pkpaymentrequest/1619329-supportednetworks) vlastnost rozšířila provést jako argument název poskytovatele platebních. Pomocí těchto metod, můžete aplikaci automaticky podporují všechny síť, která podporuje poskytovatele platebních služeb.

Další informace najdete v tématu naše [Apple platit konfigurace](~/ios/platform/apple-pay.md) a společnosti Apple [Apple platit průvodce](https://developer.apple.com/apple-pay/).

## <a name="new-testing-environment"></a>Nové prostředí pro testování

IOS 10 a watchOS 3 Společnost Apple vydala nové prostředí pro testování, které umožňuje vývojáři zřídit testovací platební karty přímo na zařízení s iOS. Toto nové prostředí testování pak vrátí šifrované testovací platebních data do aplikace.

Pokud chcete povolit nové testovacího prostředí, postupujte takto:

1. Vytvořte nový účet testování Icloudu v iTunes připojit.
2. Přihlaste se k zařízení s iOS k novému účtu testování.
3. Nastavte na požadovanou oblast k testování aplikace v.
4. Použijte jednu z platební karty test [Apple platit průvodce](https://developer.apple.com/apple-pay/) pro platby.

> [!NOTE]
> Přepnutím Icloudu účty se zařízení automaticky změní nového testovacího prostředí. Však stále Apple **vyžaduje** aplikace má být testována s reálné karty v produkčním prostředí před odesláním do iTunes App Storu.

## <a name="summary"></a>Souhrn

Tento článek má zahrnutých rozšířením Apple má provést dotykový identifikátor ve watchOS 3 a jejich implementaci v Xamarin.iOS.
