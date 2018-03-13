---
title: "Výchozí prostředky"
ms.topic: article
ms.prod: xamarin
ms.assetid: 762572F0-173A-D994-0510-8F36BEF3D487
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 03/01/2018
ms.openlocfilehash: 3d9c747cdf8e43f33b9310ac1156550066b400eb
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 03/09/2018
---
# <a name="default-resources"></a>Výchozí prostředky

Výchozí prostředky jsou položky, které nejsou specifické pro všechny konkrétní zařízení nebo faktor formuláře a proto je výchozí volba podle operační systém Android Pokud žádné další specifické prostředky lze nalézt. Jako takový jsou nejběžnějším typem prostředek pro vytvoření. Jste uspořádány do dílčí adresáře **prostředky** adresář podle typu zdrojů:

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

![Výchozí soubory prostředků](default-resources-images/01-resource-files-vs.png)

Na předchozím obrázku projekt má výchozí hodnoty pro drawable prostředků, rozložení a hodnoty (soubory XML, které obsahují jednoduché hodnoty).

Úplný seznam typů prostředků najdete níže:

-  **Animator** &ndash; soubory XML, které popisují vlastnost animace.
   Vlastnost animací byly zavedeny v úroveň rozhraní API 11 (Android 3.0) a poskytuje pro animaci vlastností objektu. Vlastnost animace jsou pružnější a výkonnější způsob, jak popisuje animací na libovolný typ objektu.

-  **anim** &ndash; soubory XML, které popisují *doplnění* animace. Animace doplnění jsou řadu animace pokyny k provádění transformací na obsah zobrazení objektu nebo například otočení bitovou kopii nebo ročně zvýší velikost textu. Animace doplnění jsou omezené jenom zobrazit objekty.

-  **Barva** &ndash; soubory XML, které popisují stavu seznam barev. Pochopit seznamy stavu barvu, zvažte widget uživatelského rozhraní, jako je tlačítko.
   Může být, mají různé stavy, jako například stisknutí tlačítka nebo zakázána a tlačítko může změnit barvu s každou změny ve stavu. V seznamu je vyjádřeno v seznamu stavu.

-  **drawable** &ndash; Drawable prostředky jsou obecné koncept pro obrázky, které může být zkompilovány do aplikace a přístup volání rozhraní API nebo odkazují jiné prostředky XML.
   Některé příklady drawables jsou soubory rastrový obrázek (GIF, PNG, JPG), speciální s možností změny velikosti rastrové obrázky, které jsou známé jako [devět opravy](https://developer.android.com/guide/topics/graphics/2d-graphics.html#nine-patch), stavu uvádí obecný tvary definované v XML atd.
 
-  **rozložení** &ndash; soubory XML, které popisují rozvržení uživatelského rozhraní, jako je například aktivitu nebo řádek v seznamu.

-  **nabídky** &ndash; soubory XML, které popisují nabídky aplikace, jako *možnosti nabídky*, *kontextové nabídky*, a *dílčích*. Příklad nabídky, naleznete v části [místní nabídky ukázku](https://developer.xamarin.com/samples/monodroid/PopupMenuDemo/) nebo [standardní ovládací prvky](https://developer.xamarin.com/samples/mobile/StandardControls/) ukázka.

-  **Nezpracovaná** &ndash; libovolné soubory, které jsou uloženy v jejich raw, binární podobě. Tyto soubory jsou zkompilovány do aplikace pro Android v binárním formátu.

-  **hodnoty** &ndash; soubory XML, které obsahují jednoduché hodnoty. Soubor XML v adresáři hodnoty nedefinuje jediný zdroj, ale místo toho můžete definovat několik prostředků. Například jeden soubor XML může podržte seznam řetězcové hodnoty jiný soubor XML může obsahovat seznam – hodnoty barev.

-  **XML** &ndash; soubory XML, které jsou podobné funkce jako konfigurační soubory rozhraní .NET. Jedná se o libovolný XML, který lze číst v době běhu aplikací


# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

![Výchozí soubory prostředků](default-resources-images/01-resource-files-xs.png)

Na předchozím obrázku projekt má výchozí hodnoty pro drawable prostředků, rozložení a hodnoty (soubory XML, které obsahují jednoduché hodnoty).

Úplný seznam typů prostředků najdete níže:

-  **Animator** &ndash; soubory XML, které popisují vlastnost animace.
   Vlastnost animací byly zavedeny v úroveň rozhraní API 11 (Android 3.0) a poskytuje pro animaci vlastností objektu. Vlastnost animace jsou pružnější a výkonnější způsob, jak popisuje animací na libovolný typ objektu.

-  **anim** &ndash; soubory XML, které popisují *doplnění* animace. Animace doplnění jsou řadu animace pokyny k provádění transformací na obsah zobrazení objektu nebo například otočení bitovou kopii nebo ročně zvýší velikost textu. Animace doplnění jsou omezené jenom zobrazit objekty.

-  **Barva** &ndash; soubory XML, které popisují stavu seznam barev. Pochopit seznamy stavu barvu, zvažte widget uživatelského rozhraní, jako je tlačítko.
   Může být, mají různé stavy, jako například stisknutí tlačítka nebo zakázána a tlačítko může změnit barvu s každou změny ve stavu. V seznamu je vyjádřeno v seznamu stavu.

-  **písmo** &ndash; spouštění služby na úrovni rozhraní API 26, je možné vložit písma jako prostředek v aplikaci pro Android. 26 knihovny podporu bude backport písem úroveň rozhraní API 14. Vkládání písem umožňuje aplikacím

-  **Mipmap** &ndash; Drawable prostředky jsou obecné koncept pro obrázky, které může být zkompilovány do aplikace a přístup volání rozhraní API nebo odkazují jiné prostředky XML.
   Některé příklady drawables jsou soubory rastrový obrázek (GIF, PNG, JPG), speciální s možností změny velikosti rastrové obrázky, které jsou známé jako [devět opravy](https://developer.android.com/guide/topics/graphics/2d-graphics.html#nine-patch), stavu uvádí obecný tvary definované v XML atd.

-  **rozložení** &ndash; soubory XML, které popisují rozvržení uživatelského rozhraní, jako je například aktivitu nebo řádek v seznamu.

-  **nabídky** &ndash; soubory XML, které popisují nabídky aplikace, jako *možnosti nabídky*, *kontextové nabídky*, a *dílčích*. Příklad nabídky, naleznete v části [místní nabídky ukázku](https://developer.xamarin.com/samples/monodroid/PopupMenuDemo/) nebo [standardní ovládací prvky](https://developer.xamarin.com/samples/mobile/StandardControls/) ukázka.

-  **Nezpracovaná** &ndash; libovolné soubory, které jsou uloženy v jejich raw, binární podobě. Tyto soubory jsou zkompilovány do aplikace pro Android v binárním formátu.

-  **hodnoty** &ndash; soubory XML, které obsahují jednoduché hodnoty. Soubor XML v adresáři hodnoty nedefinuje jediný zdroj, ale místo toho můžete definovat několik prostředků. Například jeden soubor XML může podržte seznam řetězcové hodnoty jiný soubor XML může obsahovat seznam – hodnoty barev.

-  **XML** &ndash; soubory XML, které jsou podobné funkce jako konfigurační soubory rozhraní .NET. Jedná se o libovolný XML, který lze číst v době běhu aplikací

-----
