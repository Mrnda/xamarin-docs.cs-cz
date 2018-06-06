---
title: Odběry a vytváření sestav v Xamarin.iOS
description: Tento dokument popisuje – obnovení odběry, bezplatných předplatných, automaticky obnovitelných odběry a pomocí iTunes připojení Pokud chcete sestavu podle těchto položek.
ms.prod: xamarin
ms.assetid: 27EE4234-07F5-D2CD-DC1C-86E27C20141E
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/18/2017
ms.openlocfilehash: 7e0873107a60b48e5ebfd8e159f3bf3b85d02867
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 06/05/2018
ms.locfileid: "34787026"
---
# <a name="subscriptions-and-reporting-in-xamarinios"></a>Odběry a vytváření sestav v Xamarin.iOS

## <a name="about-non-renewing-subscriptions"></a>O bez obnovení odběrů

Obnovení není odběry jsou určené pro produkty, které představují prodej služby s omezením čas například (jeden týden přístupu k navigační aplikaci) nebo časově omezené přístup k archivu data.   
   
Hlavní rozdíly mezi-obnovení odběry a jinými typy produktu:

-  Definice produktu v iTunes Connect nezahrnuje termín. Kód aplikace musí být schopen odvodit doby platnosti z ID produktu. 
-  Můžete ji získat několikrát (například použití produktu). Aplikace se vyžaduje ke správě předplatného termín nebo vypršení platnosti a obnovení a brání uživateli, kdybyste kupovali překrývající se odběry. 
-  Nákupy nepodporuje funkci StoreKit obnovit. Pokud předplatné by měly být dostupné přes všechny uživatele zařízení, aplikace bude mít navrhovat a implementovat tuto funkci ve spojení se vzdáleným serverem. Aplikace jsou také zodpovědné za zálohování up stav odběru pro případy, když je zařízení zálohovaná pak obnovit z zálohování. 
-  Přehled implementace
-  Obnovení není odběry by měla být implementována normálně pomocí serveru doručit. pracovní postup a spravuje jako použití produkty. 


## <a name="about-free-subscriptions"></a>O bezplatných předplatných

Bezplatných předplatných umožňují vývojářům umístění volné obsahu do aplikace Newsstand (jejich nelze použít v aplikacích jiných Newsstand). Jakmile začne bezplatné předplatné bude k dispozici na všechny uživatele zařízení. Bezplatných předplatných nikdy nevyprší; pouze ukončení při odinstalaci aplikace.

### <a name="implementation-overview"></a>Přehled implementace

Bezplatných předplatných mnohem chovat jako automatické obnovitelných odběry. Aplikace musí mít k dispozici, zakoupit' bezplatné předplatné produktu v iTunes připojit. Při zakoupení uživatelem, by měla být ověřena nákupu bezplatné předplatné jako o automatické prodloužit předplatné produkt. Bezplatné předplatné transakcí lze obnovit.


## <a name="about-auto-renewable-subscriptions"></a>O obnovitelných automaticky předplatných

Automaticky prodloužit předplatné se používá hlavně v aplikacích Newsstand. Představují produktu, který uděluje přístup uživatelů k dynamický obsah za dané období čas, který je nakonfigurován v iTunes Connect (nastavit období od 7 dní až 1 rok). Odběry obnovit automaticky poplatků Apple ID uživatele na konci každého období odběru, pokud uživatel požádá na více systémů. Tento typ produktu dobře funguje v časopise nebo news odběry, kde uživatel získá přístup k jednotlivé problémy publikované, ale jejich předplatné je platný.

### <a name="implementation-overview"></a>Přehled implementace

Automatické obnovitelných odběry by měla být implementována pomocí pracovního postupu Server-Delivered produkty (naleznete *přijetí ověření a produkty Server-Delivered* části).

#### <a name="shared-secret"></a>Sdílený tajný klíč

V aplikaci nákupu sdílený tajný klíč musí použít v požadavku JSON při ověřování obnovitelnými automaticky odběry na vašem serveru. Sdílený tajný klíč je vytvořen nebo přístup přes iTunes připojit.

V obchodě iTunes Connect domovské stránce vyberte **Moje aplikace**:   
   
 [![](subscriptions-and-reporting-images/image2.png "Vybrat Moje aplikace")](subscriptions-and-reporting-images/image2.png#lightbox)  
 
Vyberte aplikaci a klikněte na **nákupy v aplikaci** karty:

[![](subscriptions-and-reporting-images/image6.png "Klikněte na kartu nákupy v aplikaci")](subscriptions-and-reporting-images/image6.png#lightbox)

V dolní části stránky, vyberte **zobrazení nebo vygenerovat sdílený tajný klíč**:
   
 [![](subscriptions-and-reporting-images/image40.png "Vyberte zobrazení nebo vygenerovat sdílený tajný klíč")](subscriptions-and-reporting-images/image40.png#lightbox)

 [![](subscriptions-and-reporting-images/image41.png "Vygenerovat sdílený tajný klíč")](subscriptions-and-reporting-images/image41.png#lightbox)   
   
   
   
 Použít sdílený tajný klíč, že její zahrnutí do datové části JSON, kterou posílá servery společnosti Apple při ověřování přijetí nákupy v aplikaci pro předplatné prodloužit automaticky takto:

```csharp
{
   "receipt-data" : "(receipt bytes here)",
   "password"     : "(shared secret bytes here)"
}
```

Stav pole z odpovědi bude nula, pokud je platný, jako s jinými typy produktu nákupu.

#### <a name="downloading-items-after-the-initial-subscription-term"></a>Při stahování položky po platnosti počáteční předplatného

Jako součást dodáváme produkty, předplatné by měl kód často ověřit známé příjmu proti servery společnosti Apple. Pokud předplatné má auto obnovit od poslední kontroly, JSON odpověď bude obsahovat další pole, které aplikace transakce došlo k chybě (který by měl rozšířit platnosti odběry). Bude obsahovat odpověď JSON:

```csharp
{
   "status" : 0,
   "receipt" : { (receipt here) },
   "latest_receipt" : "(base-64 encoded receipt here)",
   "latest_receipt_info" : { (latest receipt info here) }
}
```

Pokud je stav nula je předplatné stále platné a v ostatních polích obsahovat platná data. Pokud je stav 21006 vypršela platnost předplatného. Najdete v článku [ověření automatické prodloužit předplatné přijetí](https://developer.apple.com/library/ios/releasenotes/General/ValidateAppStoreReceipt/Chapters/ValidateRemotely.html) dokumentace pro další kódy chyb.

#### <a name="restoring-auto-renewable-subscriptions"></a>Obnovení automaticky obnovitelných odběrů

Získáte zpět více transakcí – původní transakce nákupu plus oddělené transakci pro každý časový interval odběr byl obnoven. Je třeba sledovat počáteční data a podmínky, které chcete porozumět, co je doba platnosti.   
   
   
   
 Objekt SKPaymentTransaction nezahrnuje platnosti předplatného – měli byste použít jiné ID produktu ke každému termínu a napsat kód, který lze odvodit období odběru od data nákupu transakce.

#### <a name="testing-auto-renewal"></a>Testování automatického obnovení

Aby bylo snazší testovací předplatná, jsou jejich doby trvání komprimované při testování v izolovaném prostoru. 1 týden odběry obnovit každé 3 minuty, předplatné obnovit každou hodinu 1 rok. Odběry se automatického obnovení maximálně 6krát při testování v izolovaném prostoru.

## <a name="reporting"></a>Generování sestav

iTunes Connect ( [itunesconnect.apple.com](http://itunesconnect.apple.com)) poskytuje:   
   
 **Prodeje a trendy** – zobrazí podrobnosti o aplikaci soubory ke stažení, aktualizace a nákupy v aplikaci.   
   
 **Platby a finanční sestavy** – podrobnosti příjem vaší aplikace, jakož i seznam plateb, které byly provedeny vám a kolik jsou vůči.

Níže je uveden příklad prodeje a trendy sestavy:   

 [![](subscriptions-and-reporting-images/image42.png "Příklad prodeje a trendy sestavy")](subscriptions-and-reporting-images/image42.png#lightbox)   
   
 K dispozici je také [ **mobilní připojení MS**aplikace pro iOS (odkaz iTunes)](http://itunes.apple.com/us/app/itunes-connect-mobile/id376771144?mt=8).
Zde se zobrazují iPhone snímky obrazovky pro některý ze statistiky, které jsou k dispozici:   
   
 [![](subscriptions-and-reporting-images/image43.png "iPhone snímky obrazovky pro některý ze statistiky, které jsou k dispozici")](subscriptions-and-reporting-images/image43.png#lightbox)
