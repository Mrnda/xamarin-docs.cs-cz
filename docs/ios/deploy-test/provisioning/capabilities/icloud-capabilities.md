---
title: Icloudu možnosti
description: Přidání funkcí do aplikace často vyžaduje další nastavení zřizování. Tato příručka vysvětluje nastavení potřebná pro funkce serveru služby iCloud.
ms.prod: xamarin
ms.assetid: 3CBAC982-D8DE-48DD-97CD-32B551D9DB85
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/15/2017
ms.openlocfilehash: e426423854e7c569576c374ea1284c4de099a2d1
ms.sourcegitcommit: bc39d85b4585fcb291bd30b8004b3f7edcac4602
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/16/2018
---
# <a name="icloud-capabilities"></a>Icloudu možnosti

_Přidání funkcí do aplikace často vyžaduje další nastavení zřizování. Tato příručka vysvětluje nastavení potřebná pro funkce serveru služby iCloud._

Icloudu poskytuje iOS uživatelům pohodlný a jednoduchý způsob ukládání obsahu a sdílet mezi zařízeními. Existují čtyři způsoby, že vývojáři mohou pomocí Icloudu poskytují možnosti pro úložiště pro své uživatele: úložiště klíč-hodnota, UIDocument úložiště, CoreData a pomocí CloudKit přímo k poskytování úložiště pro jednotlivé soubory a adresáře. Další informace o těchto, najdete v části [Úvod do Icloudu](~/ios/data-cloud/introduction-to-icloud.md) průvodce.

Přidání na serveru služby iCloud schopnost aplikace je mírně obtížnější než ostatní App Services z důvodu _kontejnery_. Kontejnery se používají v Icloudu k ukládání informací o aplikaci a povolit všechny informace obsažené v jednom serveru služby iCloud účtu k oddělené – jako sandboxing na zařízení s iOS uživatele. Další informace o kontejnerech, najdete v části [Úvod do CloudKit](~/ios/data-cloud/intro-to-cloudkit.md) průvodce.

> [!IMPORTANT]
> Apple [poskytuje nástroje](https://developer.apple.com/support/allowing-users-to-manage-data/) , což vývojářům správně zpracovat Evropské unie obecné Data Protection nařízení (GDPR).

<a name="icloud-developer-center" />

## <a name="developer-center"></a>Středisko pro vývojáře

Při zřizování nové aplikace prostřednictvím středisku pro vývojáře existují dva kroky, které je třeba přijmout:

1.  Vytvoření kontejneru.
2.  Vytvoření ID aplikace pomocí funkce serveru služby iCloud a přidejte do ní kontejneru.
3. Vytvořit profil zřizování, který obsahuje ID této aplikace

Následující postup vás provede tyto kroky:

1.  Vyhledejte [Apple Developer Center](https://developer.apple.com/account/) a přejděte k části identifikátor, certifikátů a profilů: 
    
     ![Hlavní stránka Apple Developer Center](icloud-capabilities-images/image22.png)

2.  V části **identifikátory** vyberte **Icloudu kontejnery**a pak vyberte **+** vytvořit nový kontejner:  
    
    ![obrazovka kontejneru Icloudu](icloud-capabilities-images/image23.png)

3.  Zadejte **popis** a jedinečný **identifikátor** kontejneru serveru služby iCloud: 
    
    ![obrazovka registrace kontejneru Icloudu](icloud-capabilities-images/image24.png)

4.  Stiskněte klávesu **pokračovat**, zkontrolujte správnost informací a stiskněte klávesu **zaregistrovat** vytvořit na serveru služby iCloud kontejneru:  
    
    ![obrazovka registrace kontejneru Icloudu](icloud-capabilities-images/image25.png)

K vytvoření nového ID aplikace a přidat kontejner, postupujte takto:

1.  V [středisku pro vývojáře](https://developer.apple.com/account/), klikněte na **ID aplikace** pod **identifikátory**: 
    
    ![Identifikátor oddílu ve středisku pro vývojáře](icloud-capabilities-images/image26.png)

2.  Vyberte **+** tlačítko pro přidání nové aplikace ID: 
    
    ![Přidat nové tlačítko ID aplikace](icloud-capabilities-images/image27.png)

3.  Zadejte **název** pro ID aplikace a pojmenujte ho **explicitní ID aplikace**:
    
    ![Zadejte podrobnosti o nové ID aplikace](icloud-capabilities-images/image28.png)

4.  V části **App Services** vyberte **Icloudu** a zvolte **CloudKit zahrnují podporu**:
    
    ![Vyberte Icloudu aplikační služby](icloud-capabilities-images/image29.png)

5.  Vyberte **pokračovat** a potom **zaregistrovat**. Všimněte si, že se na obrazovce potvrzení, Icloudu se zobrazí s Konfigurovatelný vybrané s žlutý symbol:   
    
    ![Potvrzovací obrazovka](icloud-capabilities-images/image30.png)

6.  Vraťte se do seznamu ID aplikace a vyberte ten, který jste právě vytvořili: 
    
    ![Vyberte ID aplikace obrazovky](icloud-capabilities-images/image31.png)

7.  Přejděte do dolní části této rozšířené části a klikněte na **upravit**:
    
    ![Upravit ID aplikace](icloud-capabilities-images/image32.png)

8.  Přejděte dolů na seznamu a klikněte na serveru služby iCloud **upravit** tlačítko:  
    
    ![Upravit Icloudu ID aplikace](icloud-capabilities-images/image33.png)

9.  Vyberte kontejner pro použití s tímto ID aplikace:  
    
    ![Vyberte kontejner obrazovky](icloud-capabilities-images/image34.png)

10. Potvrďte kontejneru přiřazení a stiskněte klávesu **přiřadit**.
 
Toto ID aplikace teď lze vygenerovat nebo znovu vygenerovat nový profil pro zřizování, jak je popsáno v [práce s možností](~/ios/deploy-test/provisioning/capabilities/index.md) průvodce. 

Další informace o používání serveru služby iCloud najdete v následujících příručkách:

*   [Úvod do Icloudu](~/ios/data-cloud/introduction-to-icloud.md)
*   [Úvod do CloudKit](~/ios/data-cloud/intro-to-cloudkit.md)
*   [Úvod do dokumentu výběr.](~/ios/platform/document-picker.md)

## <a name="next-steps"></a>Další kroky
 
Následující seznam popisuje další kroky, které může být nutné věnovat:

* Používání oboru názvů framework ve vaší aplikaci.
* Přidejte požadované oprávnění do vaší aplikace. Informace o požadované oprávnění a postup přidání je podrobně [práce oprávnění](~/ios/deploy-test/provisioning/entitlements.md) průvodce.
* V dané aplikaci **iOS podepisování sady**, ujistěte se, že **vlastní oprávnění** je nastaven na **Entitlements.plist**. Toto je _není_ sestavení výchozí nastavení pro ladění a simulátoru iOS.

Pokud narazíte na problémy s aplikační služby, podívejte se na [Poradce při potížích s](~/ios/deploy-test/provisioning/capabilities/index.md) hlavní příručce.