---
title: Vytvoření projektu CocosSharp více platformami
description: 'Tento návod ukazuje, jak vytvořit nové řešení CocosSharp více platformami. Výsledkem tohoto návodu je Visual Studio pro Mac řešení, která se skládá ze tří projektů: jeden projektu knihovny přenosných tříd, jeden projekt specifické pro Android a projekt jeden specifické pro iOS. Výsledný projektu se zobrazí prázdné černé obrazovce při spuštění.'
ms.topic: article
ms.prod: xamarin
ms.assetid: 37C97693-B0A8-4064-97B6-A6FAB5BA4FB7
ms.technology: xamarin-cross-platform
author: charlespetzold
ms.author: chape
ms.date: 03/27/2017
ms.openlocfilehash: 2906035ce9bd44d111b89ccfe7443896775315b7
ms.sourcegitcommit: 7b88081a979381094c771421253d8a388b2afc16
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 03/30/2018
---
# <a name="creating-a-multi-platform-cocossharp-project"></a>Vytvoření projektu CocosSharp více platformami

_Tento návod ukazuje, jak vytvořit nové řešení CocosSharp více platformami. Výsledkem tohoto návodu je Visual Studio pro Mac řešení, která se skládá ze tří projektů: jeden projektu knihovny přenosných tříd, jeden projekt specifické pro Android a projekt jeden specifické pro iOS. Výsledný projektu se zobrazí prázdné černé obrazovce při spuštění._

Modul 2D herní CocosSharp umožňuje kódu a obsahu být sdílen napříč různými platformami. Tento návod ukazuje, jak vytvořit projekt, který podporuje iOS a Android vývoj. Konkrétně tento návod popisuje v následujících tématech:

 - Instalace CocosSharp
 - Vytvoření nové řešení
 - `LoadGame` – Metoda

# <a name="installing-cocossharp"></a>Instalace CocosSharp

Nejprve přidáme CocosSharp k sadě Visual Studio for Mac. Pokud se systémem Mac, vyberte **Visual Studio pro Mac** > **Add-in správce...**  . Pokud se systémem Windows, vyberte **nástroje** > **Add-in správce...**  . Klikněte na tlačítko **Galerie** rozbalte **CocosSharp položky**, vyberte **šablony projektů CocosSharp**a nakonec klikněte na tlačítko **instalaci...**  .

![CocosSharp addin](part1-images/xamarinstudioaddinsmac.png "")

# <a name="creating-a-new-solution"></a>Vytvoření nové řešení

Teď, když je nainstalovaná CocosSharp, vytvoříme řešení. V sadě Visual Studio pro Mac, vyberte **soubor** > **nový** > **řešení...** . Vyberte **aplikace** možnost pod **napříč platformami** vyberte **prázdný projekt CocosSharp**a potom klikněte na **Další**:

![](part1-images/image1.png "Vyberte možnost aplikace části napříč platformami, vyberte CocosSharp prázdný projekt a pak klikněte na tlačítko Další")

Zadejte název **BouncingGame** pro **název projektu**, pak klikněte na tlačítko **vytvořit**:

![](part1-images/image2.png "Zadejte název BouncingGame jako název projektu a potom klikněte na možnost vytvořit")

Jakmile se projekt je vytvořený a Visual Studio pro Mac, jsme můžete zkompilovat a potom ho spusťte zobrazíte šedé pozadí: 

![](part1-images/image3.png "Jakmile se projekt už vytvořený a Visual Studio pro Mac, zkompilování a spuštění zobrazte šedé pozadí")


# <a name="loadgame-method"></a>LoadGame – metoda

Výchozí projekt CocosSharp obsahuje třídy, které jsou specifické pro iOS a Android pro nastavení `CCGameView`, který se používá ke spuštění CocosSharp. `CCGameView` Se vytvoří instance způsobem specifické pro platformu: vytvoří projekt pro iOS `CCGameView` v `Main.storyboard` souborů, když vytvoří Android `CCGameView` v `Main.axml` souboru. Každá platforma používá instanci CCGameView v `LoadGame` metodu, která provádí některé základní nastavení. I když jsme nebude možné úpravy tento kód, Podívejme se na některé důležité podrobnosti:

 - Nastaví kód `gameView.DesignResolution` na 1024 × 768. To standardizuje, umístění v zařízeních bez ohledu na to, poměr stran, fyzické rozlišení nebo orientaci aktuální zařízení. 
 - Kód přidá několik cest hledání. Cesty hledání také povolit obsah má být načten bez předpony adresářů. Například když `"Sounds"` cesta je přidána jako cesty vyhledávání, a potom soubor v adresáři `"Content/Sounds/mysound.xnb"` může být jednoduše načíst jako `"mysound.xnb"`. Cesty hledání jsou podobné `using` příkazů v kódu, což se může snížit kód, ale můžou taky vést nejednoznačnosti.
 - Kód vytvoří `GameLayer` instance. `GameLayer` je `CCLayer`-dědění třídy, které přidáme všechny herní logiku. Větší hry může vyžadovat více `CCLayer` instance nebo i několika `CCScene` instance (která může obsahovat více `CCLayer` instance), ale nemůžeme budete přilepit na jednu `CCLayer` pro tato hra.

#  <a name="summary"></a>Souhrn

Tento názorný postup popsané postupy k vytvoření projektu CocosSharp napříč platformami pomocí sady Visual Studio for Mac. Výsledkem je prázdný obrazovky, který můžete použít jako výchozí bod pro všechny herní projekt.