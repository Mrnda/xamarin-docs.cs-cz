---
title: Apple Pay v Xamarin.iosu
description: Tato příručka se věnuje nastavení prostředí Xamarin.iOS pro použití se službou Apple Pay k úhradě fyzického zboží, jako je například food, Zábava a členství ve skupinách prostřednictvím vaší aplikace. Obsahuje informace o požadované identifikátory, certifikáty a oprávnění.
ms.prod: xamarin
ms.assetid: A25AE660-B145-465F-9CCE-8D82BFD614C6
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 06/05/2017
ms.openlocfilehash: f2a38a4305aa11c78f3e4e35265a86dc71642777
ms.sourcegitcommit: aa9b9b203ab4cd6a6b4fd51e27d865e2abf582c1
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/30/2018
ms.locfileid: "39351662"
---
# <a name="apple-pay-in-xamarinios"></a>Apple Pay v Xamarin.iosu

_Tato příručka se věnuje nastavení prostředí Xamarin.iOS pro použití se službou Apple Pay k úhradě fyzického zboží, jako je například food, Zábava a členství ve skupinách prostřednictvím vaší aplikace. Obsahuje informace o požadované identifikátory, certifikáty a oprávnění._

Oprávnění Apple Pay byla zavedena spolu s iOS 8, povolení uživatelé platí za fyzického zboží, jako je například food, Zábava a členství ve skupinách prostřednictvím svá zařízení s Iosem. Je k dispozici na iPhone 6 a iPhone 6 Plus a také se dají párovat s Apple Watch pro nákupy v obchodě. Při použití na Iphonu, používá Touch ID jako způsob, jak potvrďte a autorizujte transakcí, které uživatele kreditní nebo debetní kartu.

## <a name="requirements"></a>Požadavky

Oprávnění Apple Pay dostupná jenom v iOS 8 a vyšším a proto vyžaduje minimálně Xcode 6.

Následující položky jsou také muset integraci do aplikace oprávnění Apple Pay:

 - Platební platforma procesoru
 - Obchodní identifikátor
 - Certifikát služby Apple Pay
 - Oprávnění Apple Pay

Tento dokument se podívejte se na tyto položky podrobněji.

## <a name="differences-between-apple-pay-and-iap"></a>Rozdíly mezi Apple Pay a akvizice IAP

Hlavní rozdíl mezi oprávnění Apple Pay a *nákupy v aplikaci* (IAP), se vztahují na produkty, které se prodávají. *Fyzické* zboží se prodávají prostřednictvím oprávnění Apple Pay; food, ubytování a fyzické Zábava (například kina lístky) jsou všechny příklady tohoto objektu. Naproti tomu IAP prodává *virtuální* zboží, jako je například premium nebo další obsah a dalších měsíců předplatná – zvažte službu streamování nebo další odpovídající hru.

Rozhraní používá se také klíčovým rozdílem mezi; [PassKit](https://developer.apple.com/library/ios/documentation/PassKit/Reference/PKPaymentAuthorizationViewController_Ref/) je použita pro oprávnění Apple Pay, zatímco [StoreKit](https://developer.apple.com/library/ios/documentation/PassKit/Reference/PKPaymentAuthorizationViewController_Ref/) poskytuje rozhraní API pro IAP.

Pomocí oprávnění Apple Pay, Apple [stavy](https://developer.apple.com/apple-pay/Getting-Started-with-Apple-Pay.pdf) , že ho "[] neúčtují uživatele, si obchodníci můžou nebo vývojáři použít Apple Pay platby". V porovnání akvizice IAP má 30 % poplatek za každou transakci. Kromě toho se oprávnění Apple Pay, transakce neprocházejí přes Apple vůbec, místo toho se prochází platební platforma.

## <a name="using-a-payment-processor-platform"></a>Pomocí platební platforma procesoru

Jedním ze základních součástí oprávnění Apple Pay je zpracování plateb. Přestože je možné to udělat sami, vyžaduje významné znalost kryptografie
- Podrobné společnosti Apple [zpracování platby průvodce](https://developer.apple.com/library/ios/ApplePay_Guide/ProcessPayment.html).
Platformy pro zpracování platby, na druhé straně zpracování těchto operací pro vás a umožňují vám soustředit se na vytváření vaší aplikace.

Dvě možnosti:

- **Stripe** – zaregistrujte se na [Stripe.com](https://stripe.com/) pro přístup k jejich rozhraní API.

- **JudoPay** – podívejte se na jejich [Xamarin ukázkového kódu na githubu](https://github.com/Judopay/Xamarin-Sample-App)a zaregistrovat [JudoPay.com](https://www.judopay.com/).

## <a name="provisioning-for-apple-pay"></a>Zřizování pro Apple Pay

Konfigurace aplikace k používání oprávnění Apple Pay vyžaduje nastavení na portálu pro vývojáře Apple a v rámci vaší aplikace. Existuje mnoho kroků, které byste měli dodržet, k úspěšnému přidělení vaší aplikace pro Apple pay:

1. Vytvořte obchodní ID:
    - Postupujte podle kroků [zde](~/ios/deploy-test/provisioning/capabilities/apple-pay-capabilities.md#merchantid)
2. Vytvoření ID aplikace s možností použít platit a přidejte do ní obchodníka:
    - Postupujte podle kroků [zde](~/ios/deploy-test/provisioning/capabilities/apple-pay-capabilities.md#appid)
3. Vygenerujte certifikát pro obchodní ID:
    - Postupujte podle kroků [zde](~/ios/deploy-test/provisioning/capabilities/apple-pay-capabilities.md#certificate)
4. Vytvořit zřizovací profil s nově vytvořené ID aplikace:
    - Postupujte podle kroků [zde](~/ios/get-started/installation/device-provisioning/manual-provisioning.md#provisioning)
5. Přidáte oprávnění Apple Pay oprávnění:
    - Vyberte oprávnění Apple platit jako podrobné [tady](~/ios/deploy-test/provisioning/entitlements.md), nebo ručně přidejte dvojici klíč/hodnota do souboru z [zde](~/ios/deploy-test/provisioning/entitlements.md)

## <a name="working-with-apple-pay"></a>Práce s Apple Pay

Apple provedl několik vylepšení oprávnění Apple Pay v Iosu 10, které umožňují uživateli vytvořit zabezpečené platby z webů a prostřednictvím interakce s Siri a mapy.

Se systémem iOS 10 několik nových rozhraní API se přidaly, které pracují s iOS a watchOS pro podporu dynamické platby sítí a nové testovací prostředí izolovaného prostoru.

### <a name="apple-pay-website-integration"></a>Integrace webu platit Apple

Nový operační systém na iOS 10, Vývojář můžete začlenit oprávnění Apple Pay přímo do svých webů pomocí **ApplePay JS**. Uživatelé s přístupem webu s prohlížečem Safari v iOS nebo macOS můžete platby se oprávnění Apple Pay ověřením transakce na jejich iPhone nebo Apple Watch. Další informace najdete v tématu společnosti Apple [odkaz na rozhraní ApplePay JP](https://developer.apple.com/reference/applepayjs).

### <a name="passkit-framework-enhancements"></a>Vylepšení PassKit Framework

V Iosu 10 PassKit framework rozšířila na podporu oprávnění Apple Pay mimo `UIKit` a umožňují karty vystavitelů prezentovat svoje vlastní karty z v rámci svých aplikací.


#### <a name="supporting-apple-pay-outside-of-uikit"></a>Podpora Apple Pay mimo UIKit

S použitím [PKPaymentAuthorizationController](https://developer.apple.com/reference/passkit/pkpaymentauthorizationcontroller) a [PKPaymentAuthorixationControllerDelegate](https://developer.apple.com/reference/passkit/pkpaymentauthorizationcontrollerdelegate), aplikace může podporovat stejné funkce poskytované službou [ PKPaymentAuthorizationViewController](https://developer.apple.com/reference/passkit/pkpaymentauthorizationviewcontroller) bez použití UIKit. Toto nové rozhraní API je potřeba pro podporu oprávnění Apple Pay na Apple Watch (a v konkrétních tříd Intent), je volitelné v jiných situacích (třeba existující aplikace). Apple navrhuje Přesun do nového rozhraní API co nejdříve poskytnout širokou podporu v rámci všech aplikací pro vývojáře Apple Pay jediného základu kódu. Další informace o záměry a integrace Siri, najdete v našich [Úvod do Sirikitu](~/ios/platform/sirikit/index.md) dokumentaci.

#### <a name="presenting-issuer-cards-from-within-apps"></a>Nabízí ten samý vystavitele z karty v rámci aplikace

Se systémem iOS 10 byly přidány nové funkce do rozhraní PassKit, díky kterým můžou karty vydavatelů zobrazíte jejich karty z v rámci své vlastní aplikace. Vývojář můžete přidat `PKPaymentButtonTypeInStore` tlačítka uživatelského rozhraní pro uživatelské rozhraní aplikace, které se zobrazí tlačítko oprávnění Apple Pay pro kartu.

`PresentPaymentPass` Metodu [PKPassLibrary](https://developer.apple.com/reference/passkit/pkpasslibrary) třídy lze také programově zobrazit karty.

### <a name="new-payment-network-support"></a>Nová podpora sítě platby

Nový operační systém na iOS 10, aplikace může automaticky podporují novou síť platby až bude k dispozici bez developer by bylo nutné změnit, znovu zkompilovat aplikaci a znovu ho pro App Store.

Nové [AvailableNetworks](https://developer.apple.com/reference/passkit/pkpaymentrequest/1833288-availablenetworks) metodu `PKPaymentNetwork` třída umožňuje aplikaci ke zjišťování sítě, které jsou k dispozici na uživatelské zařízení za běhu. Kromě toho [SupportedNetworks](https://developer.apple.com/reference/passkit/pkpaymentrequest/1619329-supportednetworks) vlastnost má došlo k rozbalení se název poskytovatele platebních jako argument. Použití těchto metod, můžete aplikaci automaticky podporují jakoukoli sítí podporující poskytovatele platebních služeb.

Další informace najdete v tématu naše [Apple Configuration platit](~/ios/platform/apple-pay.md) a společnosti Apple [Apple platit průvodce](https://developer.apple.com/apple-pay/).

### <a name="new-testing-environment"></a>Nové testovací prostředí

Se systémem iOS 10 Apple představila nové testovací prostředí, který umožňuje vývojářům pro zřízení testovacího platebních karet přímo na zařízení s Iosem. Tento nový testovací prostředí do aplikace vrátí šifrované zkušební platební údaje.

Pokud chcete povolit nové testovací prostředí, postupujte takto:

1. Vytvořte nový testovací Icloudu účet ve službě iTunes Connect.
2. Přihlášení k zařízení s Iosem nový zkušební účet.
3. Nastavte a otestujte aplikaci v požadované oblasti.
4. Použijte jednu z platební karty test [Apple platit průvodce](https://developer.apple.com/apple-pay/) pro platby.

> [!IMPORTANT]
> Díky přepínání účty serveru služby iCloud, bude zařízení automaticky přepnout na nové testovací prostředí. Nicméně stále Apple **vyžaduje** aplikace má být testována skutečným karty v provozním prostředí před odesláním do služby iTunes App Store.

## <a name="summary"></a>Souhrn

V tomto článku Prozkoumali jsme různé položky muset použít oprávnění Apple Pay v rámci vaší aplikace. Jsme se podívali na tom, jak vytvořit obchodní ID a způsobu jejich použití v rámci **do souboru Entitlements.plist**, které je potřeba se manuálně upravovat.

## <a name="related-links"></a>Související odkazy

- [Nákupy v aplikaci](~/ios/platform/in-app-purchasing/index.md)
- [Úvod do PassKit](~/ios/platform/passkit.md)
- [PassKit](https://developer.apple.com/library/ios/documentation/PassKit/Reference/PKPaymentAuthorizationViewController_Ref/)
- [Apple Pay](https://developer.apple.com/apple-pay/)
- [Emporium (ukázka)](https://developer.xamarin.com/samples/monotouch/ios9/Emporium/)
