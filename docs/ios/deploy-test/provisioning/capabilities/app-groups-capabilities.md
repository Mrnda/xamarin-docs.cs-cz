---
title: "Možnosti skupiny aplikací"
description: "Přidání funkcí do aplikace často vyžaduje další nastavení zřizování. Tato příručka vysvětluje nastavení potřebná pro funkce aplikace skupiny."
ms.topic: article
ms.prod: xamarin
ms.assetid: 0A61220B-BBAC-492B-9D3B-578986E64064
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/15/2017
ms.openlocfilehash: 687c894fbda8dc8b7c6aceb7eef73971b50de57e
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 02/27/2018
---
# <a name="app-group-capabilities"></a>Možnosti skupiny aplikací

_Přidání funkcí do aplikace často vyžaduje další nastavení zřizování. Tato příručka vysvětluje nastavení potřebná pro funkce aplikace skupiny._

Skupinu aplikace umožňuje různé aplikace (nebo aplikace a její rozšíření) pro přístup k umístění úložiště sdílený soubor. Skupiny aplikací lze použít pro data, jako jsou:

*   [Nastavení Apple Watch](~/ios/watchos/app-fundamentals/settings.md)
*   [Sdílené NSUserDefaults](~/ios/app-fundamentals/user-defaults.md)
*   [Sdílené soubory](~/ios/watchos/app-fundamentals/parent-app.md#files)

## <a name="configure-a-new-app-group"></a>Konfigurace nové skupiny aplikací

Sdílené umístění je konfigurován pomocí [aplikace skupiny](https://developer.apple.com/library/content/documentation/Miscellaneous/Reference/EntitlementKeyReference/Chapters/EnablingAppSandbox.html#//apple_ref/doc/uid/TP40011195-CH4-SW19), která je nakonfigurována v **certifikáty, identifikátory a profily** části na [Apple Developer Center](https://developer.apple.com/account/). Tato hodnota musí odkazovat také v Entitlements.plist každý projekt.

Aplikace bude mít skupina identifikátor, který je obvykle ID sady se skupinou. Předpona. Například ID sady `com.xamarin.WatchSettings` by měla mít skupině aplikace `group.com.xamarin.WatchSettings`.

Pokud chcete vytvořit novou skupinu aplikací, postupujte takto:

1.  Navštivte společnosti Apple [iOS Developer Center](https://developer.apple.com/account/), otevřete váš **účet** a přihlaste se.
2.  Vyberte **certifikáty, identifikátory a profily**.
3.  V části **identifikátory** vyberte **skupin aplikací** a klikněte na tlačítko  **+**  tlačítko vytvořte novou skupinu.
4.  Zadejte **název** a **identifikátor** pro nové skupiny a klikněte na **pokračovat** tlačítko: 
   
    ![Podrobnosti o přidání skupiny aplikací](app-groups-capabilities-images/image52.png)

5.  Klikněte na tlačítko **zaregistrovat** tlačítko pro vytvoření skupiny a **provádí** se vraťte do seznamu registrovaných skupiny aplikací.

## <a name="configure-an-app-to-use-app-groups"></a>Konfigurace aplikace pro pomocí skupin aplikací

Pomocí vytvořenou skupinu aplikaci nakonfigurujte tak, aby aplikace můžete používat ID aplikace.

Postupujte takto:

1.  Navštivte společnosti Apple [iOS Developer Center](https://developer.apple.com/account/)a přihlaste se pomocí účtu vývojáře Apple.
2.  Z **Program prostředky** nabídce vyberte možnost **certifikáty, identifikátory a profily**.
3.  V části **identifikátory** vyberte **ID aplikace** a klikněte na  **+**  tlačítko pro vytvoření nového ID.
4.  Zadejte název pro ID aplikace a pojmenujte ho ID explicitní aplikace.
5.  V části **App Services** povolit **skupin aplikací**, klikněte na tlačítko Pokračovat:

    ![Přidat skupinu aplikaci aplikační služby](app-groups-capabilities-images/image53.png)

6.  Ověřte nastavení a klikněte **zaregistrovat** tlačítko vytvořte ID aplikace.
7.  Klikněte **provádí** tlačítko se vraťte do seznamu zaregistrované ID aplikace.
8.  Vyberte nově vytvořené ID aplikace ze seznamu a klikněte na **upravit** tlačítko:

    ![Vyberte ze seznamu ID aplikace](app-groups-capabilities-images/image54.png)

9.  V rámci služby v **aplikace skupiny**, klikněte **upravit** tlačítko:

    ![Vyberte ze seznamu ID aplikace](app-groups-capabilities-images/image55.png)

10. Vyberte skupinu aplikací, která byla vytvořena výše a klikněte na **pokračovat** tlačítko:

    ![Přidat skupinu aplikací](app-groups-capabilities-images/image56.png)

11. Klikněte na tlačítko **přiřadit** tlačítko, pak se **provádí** tlačítko se vraťte do seznamu zaregistrované ID aplikace.
12. Opakujte tyto kroky pro všechny aplikace (nebo rozšíření), které budou používat skupiny aplikací.

## <a name="next-steps"></a>Další kroky
 
Následující seznam popisuje další kroky, které může být nutné věnovat:

* Používání oboru názvů framework ve vaší aplikaci.
* Přidejte požadované oprávnění do vaší aplikace. Informace o požadované oprávnění a postup přidání je podrobně [práce oprávnění](~/ios/deploy-test/provisioning/entitlements.md) průvodce.
* V dané aplikaci **iOS podepisování sady**, ujistěte se, že **vlastní oprávnění** je nastaven na **Entitlements.plist**. Toto je _není_ sestavení výchozí nastavení pro ladění a simulátoru iOS.

Pokud narazíte na problémy s aplikační služby, podívejte se na [Poradce při potížích s](~/ios/deploy-test/provisioning/capabilities/index.md) hlavní příručce.