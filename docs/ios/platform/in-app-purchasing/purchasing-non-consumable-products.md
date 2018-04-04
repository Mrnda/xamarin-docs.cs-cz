---
title: Nákup-nespotřebitelné produkty
ms.prod: xamarin
ms.assetid: 635D9CA2-6BCA-53E1-7B10-968029AA3493
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/18/2017
ms.openlocfilehash: 0a581dc222e43f8d4742bd52dc56dc691449a8f2
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/04/2018
---
# <a name="purchasing-non-consumable-products"></a>Nákup-nespotřebitelné produkty

Non-consumable produkty jsou '' zákazník vlastní. Očekává se, že budou vždy mít přístup, a to i v případě ztráty nebo odcizení je jejich zařízení nebo se koupit nový. Jsou vhodné pro knihy, katalogu problémy, herní úrovně, filtry fotografií, 'funkce verze pro, atd. Jakmile uživatel koupil-nespotřebitelné produktu, mají nikdy platit pro něj znovu. Pokud váš kód omylem jim umožňuje akci, StoreKit zobrazí zprávu, která již byla zakoupili.

## <a name="non-consumable-products-sample"></a>Produkty nespotřebitelné – ukázka

[InAppPurchaseSample kód](https://developer.xamarin.com/samples/monotouch/StoreKit/) obsahuje projekt s názvem *NonConsumables*. Ukázka kódu ukazuje, jak implementovat-nespotřebitelné produkty jako příklad použijeme fotografií filtry. Po zakoupení filtru můžete použít ho k fotografie opakovaně. Nikdy budete muset znovu ji zakoupit.   
   
   
   
 Nákupní proces se zobrazí v této série snímky obrazovky – **koupit** tlačítko se změní tlačítko aktivace funkce:   
   
   
   
 [![](purchasing-non-consumable-products-images/image34.png "Nákupní proces se zobrazí v této série snímky obrazovky")](purchasing-non-consumable-products-images/image34.png#lightbox)   
   
   
   
 Nákupní proces je stejný jako použití produktu; Klíčovým rozdílem je v tom, jak je nákupu sledovány v kódu aplikace. V tomto příkladu je tlačítko Koupit pouze k dispozici, pokud již není zakoupena produktu, v opačném případě na tlačítko aktivuje funkci sám sebe.   
   
   
   

Následující diagram ukazuje interakce mezi třídami a serveru App Store k nákupu produktu nespotřebitelné:   
   
   
   
 [![](purchasing-non-consumable-products-images/image35.png "Interakce mezi třídami a server App Store, aby provedla-nespotřebitelné produktu nákupu")](purchasing-non-consumable-products-images/image35.png#lightbox)   
   
   
   
 Z použití příkladu klíčovým rozdílem je, že po dokončení nákupu uživatelského rozhraní se aktualizuje na zabránit znovu nákupu. V tomto příkladu oznámení úspěšné transakce aktualizace uživatelského rozhraní tak, aby **koupit** tlačítko je převeden na tlačítko, které aktivuje funkci sám sebe.

## <a name="re-purchasing-non-consumable-products"></a>Znovu nákupu-nespotřebitelné produkty

Váš kód by měl obvykle skrytí nebo změňte účel tlačítko nákupu, jakmile produkt úspěšně zakoupen, aby uživatel v pokusu o nákupu produktu znovu. Ukázkovou aplikaci dosahuje tím, že změna **koupit** tlačítko do tlačítko, které umožňuje Filtr fotografií příklad fungovat.   
   
   
   
 Existují situace, kdy aplikace nelze zjistit, zda je produkt nespotřebitelné již zakoupili:

-  Pokud aplikace je odstranit a znovu nainstalovat do zařízení, budou všechny záznamy nákupu pryč (Pokud / dokud uživatel nemá zálohování obnovení). 
-  Pokud má uživatel aplikace nainstalované v zařízení (minimálně dva) a provádí nákup na jedno ze zařízení. Jiná zařízení budou nadále zobrazit k nákupu produktu. 
-  Pokud zákazník se pokusí znovu nákupu produktu nespotřebitelné v těchto situacích, bude produkt znovu bezplatně splnění obchodu s aplikacemi. Uživatelské rozhraní zobrazí původně provést nákup (například se zobrazí upozornění na potvrzení a Apple ID se bude vyžadovat) ale pak uvidí uživatel zprávu vyzývající produktu již zakoupili.  
   
   
   
 Cesta kódu v tomto scénáři je přesně stejně jako regulární nákupu, jsou pouze rozdíly:

-  Uživatel není získat účtovat znovu produktu.
-  `SKPaymentTransaction` Objekt předaný aplikace bude mít `OriginalTransaction` vlastnost, která odkazuje na transakci, který byl vygenerován při počátečním zakoupení produktu. 
-  Aplikace, které prodeje Non-Consumable produkty musí implementovat také na StoreKit **obnovení** funkce, která pomáhá uživatelům načíst existující nákupy. 
