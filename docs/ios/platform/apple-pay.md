---
title: Platím Apple
description: Tato příručka popisuje nastavení Xamarin.iOS prostředí pro použití s dotykový identifikátor platit pro fyzické zboží, například jídlo, zábavy a členství ve skupinách prostřednictvím vaší aplikace. Obsahuje informace o požadované identifikátory, certifikáty a oprávnění.
ms.prod: xamarin
ms.assetid: A25AE660-B145-465F-9CCE-8D82BFD614C6
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.openlocfilehash: fc7c247e5edcdc25d53c34c922801a5497b8c367
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/04/2018
---
# <a name="apple-pay"></a>Platím Apple

_Tato příručka popisuje nastavení Xamarin.iOS prostředí pro použití s dotykový identifikátor platit pro fyzické zboží, například jídlo, zábavy a členství ve skupinách prostřednictvím vaší aplikace. Obsahuje informace o požadované identifikátory, certifikáty a oprávnění._


Dotykový identifikátor přinesl spolu s iOS 8, uživatelé budou platit pro fyzické zboží například jídlo, zábavy a členství ve skupinách prostřednictvím jejich zařízení s iOS. Je k dispozici na zařízení iPhone 6 a iPhone 6 Plus a také může být spárována s Apple Watch pro nákupy v obchodě. Při použití v zařízení iPhone, používá Touch ID jako způsob, jak potvrďte a autorizovat uživatele kreditní nebo debetní karty transakce.


## <a name="requirements"></a>Požadavky

Dotykový identifikátor je k dispozici v rámci iOS 8 a vyšší a proto vyžaduje minimálně Xcode 6.

Vyžaduje k integraci dotykový identifikátor do vaší aplikace jsou také následující položky:

 - Platebních procesor platformy
 - Obchodní identifikátor
 - Certifikát služby Apple platit
 - Dotykový identifikátor nároku

Tento dokument se podívejte se na tyto položky podrobněji.

## <a name="differences-between-apple-pay-and-iap"></a>Rozdíly mezi Apple mzdy a IAP

Hlavní rozdíl mezi dotykový identifikátor a *nákupu v aplikaci* (IAP), se vztahují na produkty, které budou prodeje. *Fyzické* zboží se prodávají prostřednictvím dotykový identifikátor; jídlo, ubytovací a fyzické Zábava (například filmu lístků) jsou všechny příklady tohoto objektu. Naproti tomu IAP prodává *virtuální* zboží, jako je například premium nebo další obsah a další měsíce odběry – zvažte streamování služby nebo navíc život ve hře.

Rozhraní používá se také klíčovým rozdílem; [PassKit](https://developer.apple.com/library/ios/documentation/PassKit/Reference/PKPaymentAuthorizationViewController_Ref/) se používá pro dotykový identifikátor, při [StoreKit](https://developer.apple.com/library/ios/documentation/PassKit/Reference/PKPaymentAuthorizationViewController_Ref/) poskytuje rozhraní API pro IAP.

S dotykový identifikátor, Apple [stavy](https://developer.apple.com/apple-pay/Getting-Started-with-Apple-Pay.pdf) , "[] nebude uplatňovat uživatelům obchodníků nebo vývojáři použít Apple platím pro platby". V porovnání má IAP poplatků 30 % pro každou transakci. Kromě toho se dotykový identifikátor, transakce neprochází Apple vůbec, místo toho projde platebních platformy.


## <a name="using-a-payment-processor-platform"></a>Pomocí platformu platebních procesoru

Jedním z základní částí dotykový identifikátor je zpracování platby. I když je možné dělat toto sami, se vyžaduje významné znalost kryptografie
- podle popisu v společnosti Apple [zpracování platebních průvodce](https://developer.apple.com/library/ios/ApplePay_Guide/ProcessPayment.html).
Platebních zpracování platformy, na druhé straně zpracování těchto operací pro vás, což vám umožní soustředit na sestavení aplikace.

Dvě možnosti:

- **Stripe** -zaregistrovat na [Stripe.com](https://stripe.com/) pro přístup k jejich rozhraní API.

- **JudoPay** – podívejte se na jejich [Xamarin ukázkový kód na githubu](https://github.com/Judopay/Xamarin-Sample-App)a zaregistrovat v [JudoPay.com](https://www.judopay.com/).


## <a name="provisioning-for-apple-pay"></a>Zřizování pro platím Apple

Konfigurace aplikace používat dotykový identifikátor vyžaduje Instalační program na portál pro vývojáře Apple a v rámci vaší aplikace. Existuje několik kroků, které by měl být zahájen úspěšně zřídit aplikace pro Apple platím:

1. Vytvoření obchodní ID:
    - Postupujte podle kroků [sem](~/ios/deploy-test/provisioning/capabilities/apple-pay-capabilities.md#merchantid)
2. Vytvoření ID aplikace pomocí funkce použít platit a přidejte do ní obchodní:
    - Postupujte podle kroků [sem](~/ios/deploy-test/provisioning/capabilities/apple-pay-capabilities.md#appid)
3. Vygenerujte certifikát pro ID obchodní:
    - Postupujte podle kroků [sem](~/ios/deploy-test/provisioning/capabilities/apple-pay-capabilities.md#certificate)
4. Generovat profil zřizování s nově vytvořené ID aplikace:
    - Postupujte podle kroků [sem](~/ios/get-started/installation/device-provisioning/manual-provisioning.md#provisioning)
5. Přidejte dotykový identifikátor oprávnění:
    - Vyberte nárocích platím Apple jako podrobné [sem](~/ios/deploy-test/provisioning/entitlements.md), nebo ručně přidat dvojici klíč/hodnota do souboru z [sem](~/ios/deploy-test/provisioning/entitlements.md)


## <a name="working-with-apple-pay"></a>Práce s platím Apple

Apple udělal několik vylepšení Apple platit v iOS 10, které umožní uživatele, aby zabezpečené platby z webů a prostřednictvím interakci s Siri a mapy.

S iOS 10 několik nových rozhraní API přidané které pracují s iOS a watchOS pro podporu dynamické platebních sítí a nové testovacím prostředí izolovaného prostoru.


### <a name="apple-pay-website-integration"></a>Integrace Apple platím webu

Nové iOS 10, vývojáři mohou obsahovat dotykový identifikátor přímo do jejich weby pomocí **ApplePay JS**. Uživatelé s přístupem webu s Safari v systému iOS nebo systému macOS můžete nastavit platby s dotykový identifikátor ověřením transakce na jejich zařízení iPhone nebo Apple Watch. Další informace najdete v tématu společnosti Apple [ApplePay JP Framework – referenční informace](https://developer.apple.com/reference/applepayjs).

### <a name="passkit-framework-enhancements"></a>Vylepšení PassKit Framework

V iOS 10 rozhraní PassKit rozšířila na podporu dotykový identifikátor mimo `UIKit` a umožnit karty vystavitelů vlastní karty z v rámci své aplikace k dispozici.


#### <a name="supporting-apple-pay-outside-of-uikit"></a>Podpora Apple platím mimo UIKit

Pomocí [PKPaymentAuthorizationController](https://developer.apple.com/reference/passkit/pkpaymentauthorizationcontroller) a [PKPaymentAuthorixationControllerDelegate](https://developer.apple.com/reference/passkit/pkpaymentauthorizationcontrollerdelegate), aplikace může podporovat stejné funkce poskytované službou [ PKPaymentAuthorizationViewController](https://developer.apple.com/reference/passkit/pkpaymentauthorizationviewcontroller) bez použití UIKit. Toto nové rozhraní API je potřeba pro podporu dotykový identifikátor v Apple Watch (a v konkrétních tříd Intent), je volitelné v jiných situacích (třeba existující aplikace). Apple navrhuje Přesun do nového rozhraní API co nejdříve poskytuje tak podporu dotykový identifikátor ve všech pro vývojáře aplikací s jednotného kódu. Další informace o tříd Intent a integrace Siri, najdete v tématu naše [Úvod do SiriKit](~/ios/platform/sirikit/index.md) dokumentaci.

#### <a name="presenting-issuer-cards-from-within-apps"></a>Prezentace vystavitele z karty v rámci aplikací

S iOS 10 nové funkce přidané PassKit rozhraní, které umožňují karty vystavitelů jejich karet z v rámci svých vlastních aplikací k dispozici. Můžete přidat na vývojáře `PKPaymentButtonTypeInStore` UIButton pro uživatelské rozhraní aplikace, které se zobrazí tlačítko dotykový identifikátor pro kartu.

`PresentPaymentPass` Metodu [PKPassLibrary](https://developer.apple.com/reference/passkit/pkpasslibrary) třídy lze také pro zobrazení karty prostřednictvím kódu programu.

### <a name="new-payment-network-support"></a>Nová podpora platebních sítě

Nové na iOS 10 aplikace může automaticky podporují novou síť platebních až bude k dispozici bez vývojáře museli změnit, znovu zkompiluje aplikace a odešlete ji znovu k obchodu s aplikacemi.

Nové [AvailableNetworks](https://developer.apple.com/reference/passkit/pkpaymentrequest/1833288-availablenetworks) metodu `PKPaymentNetwork` třída umožňuje aplikaci, ke zjišťování sítě, které jsou k dispozici na zařízení uživatele za běhu. Kromě toho [SupportedNetworks](https://developer.apple.com/reference/passkit/pkpaymentrequest/1619329-supportednetworks) vlastnost rozšířila provést jako argument název poskytovatele platebních. Pomocí těchto metod, můžete aplikaci automaticky podporují všechny síť, která podporuje poskytovatele platebních služeb.

Další informace najdete v tématu naše [Apple platit konfigurace](~/ios/platform/apple-pay.md) a společnosti Apple [Apple platit průvodce](https://developer.apple.com/apple-pay/).

### <a name="new-testing-environment"></a>Nové prostředí pro testování

S iOS 10 Společnost Apple vydala nové prostředí pro testování, které umožňuje vývojáři zřídit testovací platební karty přímo na zařízení s iOS. Toto nové prostředí testování pak vrátí šifrované testovací platebních data do aplikace.

Pokud chcete povolit nové testovacího prostředí, postupujte takto:

1. Vytvořte nový účet testování Icloudu v iTunes připojit.
2. Přihlaste se k zařízení s iOS k novému účtu testování.
3. Nastavte na požadovanou oblast k testování aplikace v.
4. Použijte jednu z platební karty test [Apple platit průvodce](https://developer.apple.com/apple-pay/) pro platby.

> [!IMPORTANT]
> Přepnutím Icloudu účty se zařízení automaticky změní nového testovacího prostředí. Však stále Apple **vyžaduje** aplikace má být testována s reálné karty v produkčním prostředí před odesláním do iTunes App Storu.

## <a name="summary"></a>Souhrn

V tomto článku jsme prozkoumali různé položky, které jsou potřeba v rámci aplikace použít dotykový identifikátor. Jsme se podívali na postup vytvoření ID obchodních a jak se používají v rámci **Entitlements.plist**, který má být změněn ručně.


## <a name="related-links"></a>Související odkazy

- [Nákupy v aplikaci](~/ios/platform/in-app-purchasing/index.md)
- [Úvod do PassKit](~/ios/platform/passkit.md)
- [PassKit](https://developer.apple.com/library/ios/documentation/PassKit/Reference/PKPaymentAuthorizationViewController_Ref/)
- [Apple Pay](https://developer.apple.com/apple-pay/)
- [Emporium (ukázka)](https://developer.xamarin.com/samples/monotouch/ios9/Emporium/)
