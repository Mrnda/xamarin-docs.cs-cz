---
title: Jak lze aktualizovat verze Java Development Kit (JDK)?
description: "Tento článek ukazuje, jak aktualizovat verzi jazyka Java Development Kit (JDK) v systému Windows a macu."
ms.topic: article
ms.prod: xamarin
ms.assetid: 4b3ac51d-18dd-4034-87b4-4365194e4ece
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/16/2018
ms.openlocfilehash: 126d4b2eaf1c2408704c0f2d11c2f88a7c25f406
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 02/27/2018
---
# <a name="how-do-i-update-the-java-development-kit-jdk-version"></a>Jak lze aktualizovat verze Java Development Kit (JDK)?

_Tento článek ukazuje, jak aktualizovat verzi jazyka Java Development Kit (JDK) v systému Windows a macu._

## <a name="overview"></a>Přehled

Xamarin.Android používá Java Development Kit (JDK) pro integraci sady Android SDK pro vytváření aplikací pro Android a systémem Android návrháře. Nejnovější verze sady SDK pro Android (API 24 a vyšší) vyžadují JDK 8 (1.8). Pokud jste ještě neprovedli aktualizaci na JDK 8, postupujte podle těchto kroků nainstalujte a povolte ho:

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

1.  Stáhnout JDK 8 (1.8) z [Oracle webu](http://www.oracle.com/technetwork/java/javase/downloads/index.html):

    ![Snímek obrazovky sadu JDK stáhnout na webu Oracle](update-jdk-images/image1.png)

2.  Vyberte 64bitovou verzi povolit vykreslování [vlastní ovládací prvky](https://developer.xamarin.com/releases/vs/xamarin.vs_4/xamarin.vs_4.2/#androiddesignercustomcontrols) v Návrháři Xamarin Android:

    ![Vyberte balíček Windows x64 JDK stáhnout ze stránky pro stažení JDK](update-jdk-images/image2.png)

3.  Spustit .exe a nainstalovat **nástroje pro vývoj**:

    ![Instalace nástroje pro vývoj v instalačním programu JDK](update-jdk-images/image3.png)

4.  Otevřete Visual Studio a aktualizace **Java Development Kit umístění** tak, aby odkazoval na nový JDK pod **nástroje > Možnosti > Xamarin > Android Nastavení > Java Development Kit umístění > změnu**:

    ![Nastavení cesty pro sadu JDK stránce Android nastavení možností IDE](update-jdk-images/image4.png)

Ujistěte se, že restartujte Visual Studio po aktualizaci umístění.

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

1.  Stáhnout JDK 8 (1.8) z [Oracle webu](http://www.oracle.com/technetwork/java/javase/downloads/index.html):

    ![Snímek obrazovky sadu JDK stáhnout na webu Oracle](update-jdk-images/image1.png)

2.  Otevřete soubor .dmg a spusťte instalační program .pkg:

    ![Instalační program JDK spuštěné v systému macOS](update-jdk-images/image5.png)

Mac OS automaticky nastaví na novou verzi JDK jako výchozí aktualizací **/System/Library/Frameworks/JavaVM.framework/Versions/Current**. Který pak zkontrolujte **Java SDK (JDK)** umístění se nastaví na výchozí očekávané **USR** pod **Visual Studio pro Mac > Předvolby > Projekty > SDK umístění > Android > Java SDK (JDK) > umístění**:

![Nastavení umístění JDK na stránce umístění pro Android SDK](update-jdk-images/image6.png)

-----
