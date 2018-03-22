---
title: "Možnosti platím Apple"
description: "Přidání funkcí do aplikace často vyžaduje další nastavení zřizování. Tato příručka vysvětluje potřebné pro dotykový identifikátor možnosti instalace."
ms.topic: article
ms.prod: xamarin
ms.assetid: 735CC916-16A4-471B-87F7-0535E24288D7
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/15/2017
ms.openlocfilehash: 655e9fc81d7079c355998f0da7b41ea7cc778c3f
ms.sourcegitcommit: 73bd0c7e5f237f0a1be70a6c1384309bb26609d5
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 03/22/2018
---
# <a name="apple-pay-capabilities"></a>Možnosti platím Apple

_Přidání funkcí do aplikace často vyžaduje další nastavení zřizování. Tato příručka vysvětluje potřebné pro dotykový identifikátor možnosti instalace._

Dotykový identifikátor umožňuje uživatelům platit pro fyzické zboží prostřednictvím zařízení iOS. Tato část popisuje, jak vytvořit všechny nezbytné součásti potřebné pro Apple platím v Centru pro vývojáře Apple.

Při zřizování nové aplikace prostřednictvím středisku pro vývojáře, existují tři kroky, které je třeba přijmout:

1.  Vytvoření obchodní ID.
2.  Vytvoření ID aplikace pomocí funkce použít platit a přidejte do ní obchodní.
3.  Vygenerujte certifikát pro ID obchodní.

Následující postup vás provede vytvořením výše uvedené položky:

<a name="merchantid" />

## <a name="create-merchant-id"></a>Vytvořit obchodní ID

ID obchodní slouží umožníte dotykový identifikátor vědět, může přijmout plateb a je předán na PassKit `PaymentRequest` metoda a použít v nárocích dotykový identifikátor:

1.  Vyhledejte [Apple Developer Center](https://developer.apple.com/account/) a přejděte k části identifikátor, certifikátů a profilů: 
 
    ![Výběr ID obchodní centra pro vývojáře](apple-pay-capabilities-images/image57.png)

2.  V části **identifikátory**, vyberte **obchodní ID**a pak vyberte  **+**  k vytvoření nové obchodní ID:  

3.  Vyplňte formulář, zobrazený dole, s nový popis a identifikátor. Popis umožňuje osobní ID pro vás a lze později změnit. Identifikátor musí být jedinečný pro vás a musí začínat řetězcem `merchant`. Apple doporučuje, aby identifikátor v následujícím formátu: `merchant.com.[Your-App-Name]`:
   
    ![Podrobnosti o nové obchodní ID](apple-pay-capabilities-images/image58.png)

4.  Potvrďte podrobnosti, a **zaregistrovat** vaše ID: 
    
    ![Obchodní ID potvrzení](apple-pay-capabilities-images/image59.png)

<a name="appid" />

## <a name="create-an-app-id-with-the-apple-pay-capability-that-includes-the-merchant-id"></a>Vytvoření ID aplikace pomocí dotykový identifikátor funkce, která obsahuje Identifikátor obchodního

1.  V [středisku pro vývojáře](https://developer.apple.com/account/) klikněte na **ID aplikace** pod **identifikátory**: 
    
    ![Vyberte ID aplikace ve středisku pro vývojáře](apple-pay-capabilities-images/image6.png)

2.  Vyberte  **+**  tlačítko pro přidání nové aplikace ID: 
   
    ![Přidat nové tlačítko ID aplikace](apple-pay-capabilities-images/image27.png)

3.  Zadejte název pro ID aplikace a pojmenujte ho explicitní ID aplikace:    
   
    ![Obrazovce s podrobnostmi ID aplikace ](apple-pay-capabilities-images/image35.png)

4.  V části aplikační služby vyberte dotykový identifikátor:    
  
    ![Platím Apple App Services](apple-pay-capabilities-images/image36.png)

5.  Vyberte **pokračovat** a potom **zaregistrovat**. Všimněte si, že na potvrzovací obrazovce dotykový identifikátor se zobrazí s Konfigurovatelný vybrané s žlutý symbol: 
   
    ![Potvrzovací obrazovka pro platím Apple](apple-pay-capabilities-images/image37.png)

6.  Vraťte se do seznamu ID aplikace a vyberte ten, který jste právě vytvořili:  
   
    ![Upravit ID aplikace](apple-pay-capabilities-images/image38.png)

7.  Přejděte do dolní části této rozšířené části a klikněte na **upravit**.
8.  Přejděte dolů v seznamu dotykový identifikátor a kliknutím **upravit** tlačítko:  
    
    ![Upravit podrobnosti Apple ID aplikace platit](apple-pay-capabilities-images/image39.png)

9.  Vyberte obchodní ID použít s tímto ID aplikace a klikněte na tlačítko **pokračovat**:  
    
    ![Vyberte obchodní ID pro ID aplikace](apple-pay-capabilities-images/image40.png)

10. Potvrďte ID obchodní přiřazení a stiskněte klávesu **přiřadit**:  
    
    ![Potvrzovací obrazovka](apple-pay-capabilities-images/image41.png)

Toto ID aplikace teď lze vygenerovat nebo znovu vygenerovat nový profil pro zřizování, jak je popsáno v [práce s možností](~/ios/deploy-test/provisioning/capabilities/index.md) průvodce. 

<a name="certificate" />

## <a name="create-a-certificate-for-your-merchant-id"></a>Vytvoření certifikátu pro vaše obchodní ID

K šifrování citlivých dat přidružený k transakci je vyžadován certifikát společností Apple. ID každé obchodní vytvořili musí mít svůj vlastní certifikát. 

Vytvoření certifikátu, postupujte podle následujících kroků:

1.  Vyberte obchodní ID, který byl vytvořen výše a stiskněte klávesu **upravit**: 
    
    ![ID obchodní dialogové okno Upravit](apple-pay-capabilities-images/image42.png)

2.  Na obrazovce iOS obchodní ID nastavení, klikněte na tlačítko **vytvořit certifikát**: 
   
    ![Vytvoření certifikátu zpracování platby](apple-pay-capabilities-images/image43.png)

3.  Odpovězte následující otázky: 

    ![Adresa Pokud platby budou zpracovány výhradně v Číně](apple-pay-capabilities-images/image44.png)

4.  V tomto okamžiku zobrazí se výzva k vytvoření _žádosti o podepsání certifikátu_: 

    ![Vytvoření žádosti o podepsání certifikátu](apple-pay-capabilities-images/image45.png)
    
    > [!IMPORTANT]
    > Pokud používáte poskytovatele platebních služeb pro dotykový identifikátor, takové JudoPay nebo Stripe, budou vám může poskytnout správně formátovaný CSR, který můžete použít v tomto okamžiku. Informace o vyžádání to nachází na [JudoPay](https://www.judopay.com/docs/version-52/apple-pay/getting-started/#create-an-apple-pay-certificate) a [Stripe](https://stripe.com/docs/apple-pay/apps#csr) lokalit. Pokud chcete vytvořit vlastní CSR, postupujte podle kroků 5 až 8 níže. Jakmile máte CSR, přejděte ke kroku 9.

5.  Otevřete aplikaci přístup do řetězce klíčů a přejděte do **přístup do řetězce klíčů > certifikací > žádosti o certifikát od certifikační autority:** 

     ![Vytvoření zástupce oddělení služeb pomocí řetězce klíčů na Macu](apple-pay-capabilities-images/image46.png)

6.  Zadejte e-mailovou adresu, zadejte název pro privátní klíč, ponechte prázdný, vyberte e-mailovou adresu certifikační Autority **uložit na Disk** a vyberte možnost **Chci zadat informace o dvojici klíčů**:

     ![Dialogové okno informace o certifikátu](apple-pay-capabilities-images/image47.png)

7.  Zástupce uložte do vhodného umístění: 

     ![Ukládání CSR do místního počítače](apple-pay-capabilities-images/image48.png)

8.  Na obrazovce informace dvojice klíč nastavit **velikost klíče** k **256 bitů** a **algoritmus** k **ECC** a klikněte na tlačítko **pokračovat**:

     ![Zadejte informace o dvojici klíčů dialogové okno](apple-pay-capabilities-images/image49.png)

9.  V Centru pro vývojáře, klikněte na tlačítko **pokračovat** nahrát oddělení služeb zákazníkům: 

     ![Příprava k nahrání CSR do středisku pro vývojáře](apple-pay-capabilities-images/image50.png)

10. Klikněte na tlačítko **zvolit soubor...** Chcete-li vybrat oddělení služeb zákazníkům a stiskněte klávesu **pokračovat** k nahrajte ho do portálu pro vývojáře: 

     ![Nahrání CSR do středisku pro vývojáře](apple-pay-capabilities-images/image51.png)

11. Po vygenerování certifikátu, stáhněte a poklikejte na něj nainstalovat do vašeho řetězce klíčů.

Další informace o používání dotykový identifikátor naleznete v průvodci následující:

*   [Úvod do platím Apple](~/ios/platform/apple-pay.md)

## <a name="next-steps"></a>Další kroky
 
Následující seznam popisuje další kroky, které může být nutné věnovat:

* Používání oboru názvů framework ve vaší aplikaci.
* Přidejte požadované oprávnění do vaší aplikace. Informace o požadované oprávnění a postup přidání je podrobně [práce oprávnění](~/ios/deploy-test/provisioning/entitlements.md) průvodce.
* V dané aplikaci **iOS podepisování sady**, ujistěte se, že **vlastní oprávnění** je nastaven na **Entitlements.plist**. Toto je _není_ sestavení výchozí nastavení pro ladění a simulátoru iOS.

Pokud narazíte na problémy s aplikační služby, podívejte se na [Poradce při potížích s](~/ios/deploy-test/provisioning/capabilities/index.md) hlavní příručce.