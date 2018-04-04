---
title: Licencování služby Google
ms.prod: xamarin
ms.assetid: E96BDCC3-454A-A797-5819-905E2BB1AC41
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 12/20/2017
ms.openlocfilehash: ebc557ce16858c675b291f17620616a2dba4c054
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/04/2018
---
# <a name="google-licensing-services"></a>Licencování služby Google

Před Google Play aplikace pro Android spoléhali na starší verze, které kopie ochrany poskytované trhu Google zajistit, který jen na autorizované uživatele může spouštět aplikace na jejich zařízeních. Omezení kopírování ochranný mechanismus ho provedené méně než ideální řešení pro ochranu aplikací.

Správa licencí Google je náhradou za tento starší kopie ochranný mechanismus.
Správa licencí Google je flexibilní, zabezpečení, síťovou službu, která aplikace pro Android mohou dotazovat k určení, pokud je aplikace licenci ke spuštění na daném zařízení.

Správa licencí Google je flexibilní, v tom, že aplikace pro Android máte plnou kontrolu nad kdy ke kontrole licence, jak často chcete-li zkontrolovat licenční a jak se zpracovat odpověď ze serveru licencování.

Správa licencí Google je zabezpečený, v tom, že každá odpověď je podepsaná pomocí pár klíče RSA, jež jsou sdílena výhradně mezi serverem Google Play a aplikace. Google Play obsahuje veřejný klíč pro vývojáře, která se vloží v rámci aplikace pro Android a slouží k ověřování odpovědi. Server webu Google Play interně zajišťuje privátní klíč.

Aplikace, která nasadila Google licencování odešle požadavek ke službě hostované v aplikaci Google Play na zařízení. Google Play pak odešle tento požadavek na Google licenční server, který odpoví stav licence: 

[![Diagram pracovního postupu licenční server](google-licensing-services-images/gp-licensing-service-overview.png)](google-licensing-services-images/gp-licensing-service-overview.png#lightbox)

Výše uvedené diagram znázorňuje tento pracovní postup: 

-   Aplikace poskytuje název balíčku *hodnotu nonce* (kryptografických authenticator) sloužící k ověření, odpověď serveru a zpětné volání, které může zpracovat odpovědi asynchronně. 

-   Google Play poskytuje informace, jako je účet Google a zařízení samostatně, jako je například číslo IMSI. 

Služba Licencování Google se také klíčovou součástí soubory rozšíření APK, (které jsou popsány dále v tomto dokumentu). Soubory rozšíření APK využívat Google licencování služby k získání adresy URL rozšíření soubory, které budou staženy.


## <a name="requirements"></a>Požadavky

Aplikace, které nejsou zakoupené prostřednictvím webu Google Play se žádné výhody nezískají ze služby Google licencování. Pokud Google Play není nainstalovaný na zařízení, potom aplikace, které používají služby Licencování budou i nadále fungovat normálně na daném zařízení.

Google Play vyžaduje přístup k Internetu pro funkci. Aplikace může ukládat do mezipaměti licence na bylo možné ošetřit scénáře, kde zařízení nebude mít přístup k serverům Google Play licencování.

Bezplatné aplikace vyžadují jenom Google licencování Pokud aplikace používá APK rozšíření soubory.
