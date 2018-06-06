---
title: Možnosti Peněženka v Xamarin.iOS
description: Přidání funkcí do aplikace často vyžaduje další nastavení zřizování. Tato příručka vysvětluje potřebné pro Peněženka možnosti instalace.
ms.prod: xamarin
ms.assetid: BD9475E6-F586-488C-93D4-8A2A1629B99B
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/15/2017
ms.openlocfilehash: 811c5bc707a5768e72ccb2d20541d16af67ab835
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 06/05/2018
ms.locfileid: "34785083"
---
# <a name="wallet-capabilities-in-xamarinios"></a>Možnosti Peněženka v Xamarin.iOS

_Přidání funkcí do aplikace často vyžaduje další nastavení zřizování. Tato příručka vysvětluje potřebné pro Peněženka možnosti instalace._

Peněženka je aplikace, která uchovává a zobrazí čárové kódy a další obsah, což umožňuje uživatelům zobrazit lístků, nástupu předává a kupóny přímo z jejich zařízení. Tyto informace jsou uloženy na _předat_. Například nástupu úspěšná nebo jeden lístek bude singulární průchodu. 

Vývojáři mohou pracovat s Peněženka v mnoha různými způsoby:

*   Pokud chcete vytvořit pass, nemusí aplikace má být sestaven. Passfile je komprimované archiv, který obsahuje některé soubory volitelná metadata a soubory JSON. Připravit, [předat ID typu](~/ios/platform/passkit.md) a [průchodu certifikát](~/ios/platform/passkit.md) je vyžadován. Tyto informace je pak deklarován v souboru JSON. Další informace o zřizování Passfile naleznete v [Úvod do PassKit](~/ios/platform/passkit.md) průvodce.

*   Doprovodné aplikace jsou napsané pro distribuci předává. Mají funkce pro vytváření, úpravy a aktualizaci předává a pak je přidejte do Peněženka aplikace. Dobrým příkladem tento typ aplikace bude aplikace filmu – když uživatel uzavře lístek prostřednictvím aplikace, že lístek lze přidat přímo z aplikace do Peněženka. Pokud chcete použít doprovodné aplikace, profil zřizování musí obsahovat ID aplikace s Peněženka funkce, které lze nastavit pomocí následujících kroků. Aplikace musí obsahovat také požadovaná oprávnění.

*   Přenos aplikace jsou aplikace, které není manipulaci předá přímo. Mají minimální interakci s průchodu nad rámec jeho přijetí a umožňuje tak uživatele je přidáte do Peněženka. Tyto aplikace není nutné žádné speciální zřizování nebo oprávnění, ale bude používat některé metody od PassKit rozhraní.

## <a name="developer-center"></a>Středisko pro vývojáře

Pokud chcete vytvořit nový profil pro zřizování pro použití s Peněženka, postupujte takto:

1.  Vyhledejte [identifikátory, certifikátů a profilů](https://developer.apple.com/account/ios/certificate/) části portálu pro vývojáře Apple.
2.  V části **identifikátory**, přejděte do **ID aplikace**: 
    
    ![Výběr ID aplikace](wallet-capabilities-images/image17.png)

3.  Klikněte **+** ikonu v horní pravé části stránky.
4.  Tím, že ho zaregistrovat nové ID aplikace **název** a identifikátor svazku. (Všimněte si, že tento identifikátor svazku musí odpovídat ID sady prostředků ve vašem projektu):
   
    ![Přidáním podrobností o ID aplikace](wallet-capabilities-images/image18.png)

5.  Vyberte **Peněženka** služby App Service ze seznamu služeb:
    
    ![Vyberte služby obrazovky](wallet-capabilities-images/image19.png)

6.  Stiskněte klávesu **pokračovat**a potom **zaregistrovat** k vytvoření ID aplikace.

V případě potřeby se dá přidat funkce Peněženka upravit stávající ID aplikace.

Toto ID aplikace teď lze vygenerovat nebo znovu vygenerovat nový profil pro zřizování, jak je popsáno v [práce s možností](~/ios/deploy-test/provisioning/capabilities/index.md) průvodce:

![Pomocí nově vytvořené ID aplikace, které chcete vytvořit profil pro zřizování](wallet-capabilities-images/image20.png)


Další informace o použití Peněženka naleznete v následujících příručkách:

*   [Úvod do PassKit](~/ios/platform/passkit.md)
 
## <a name="next-steps"></a>Další kroky
 
Následující seznam popisuje další kroky, které může být nutné věnovat:

* Používání oboru názvů framework ve vaší aplikaci.
* Přidejte požadované oprávnění do vaší aplikace. Informace o požadované oprávnění a postup přidání je podrobně [práce oprávnění](~/ios/deploy-test/provisioning/entitlements.md) průvodce.
* V dané aplikaci **iOS podepisování sady**, ujistěte se, že **vlastní oprávnění** je nastaven na **Entitlements.plist**. Toto je _není_ sestavení výchozí nastavení pro ladění a simulátoru iOS.

Pokud narazíte na problémy s aplikační služby, podívejte se na [Poradce při potížích s](~/ios/deploy-test/provisioning/capabilities/index.md) hlavní příručce.