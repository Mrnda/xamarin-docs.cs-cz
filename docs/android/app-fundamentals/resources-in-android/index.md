---
title: Android prostředky
description: Tento článek zavádí koncepci Android prostředky v Xamarin.Android a bude dokumentu jejich použití. Vysvětluje postup používat prostředky v aplikaci Android pro podporu více zařízení včetně různou velikost obrazovky a densities – a lokalizace aplikací.
ms.prod: xamarin
ms.assetid: C0DCC856-FA36-04CD-443F-68D26075649E
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/01/2018
ms.openlocfilehash: 7b6ba9cdc222019bfa2e1cb9a61b54e290e69bba
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/04/2018
ms.locfileid: "30764408"
---
# <a name="android-resources"></a>Android prostředky

_Tento článek zavádí koncepci Android prostředky v Xamarin.Android a bude dokumentu jejich použití. Vysvětluje postup používat prostředky v aplikaci Android pro podporu více zařízení včetně různou velikost obrazovky a densities – a lokalizace aplikací._


## <a name="overview"></a>Přehled

Aplikace pro Android je málokdy právě zdrojového kódu. Jsou často mnoho souborů, které tvoří aplikaci: video, obrázků, písem a zvukových souborů jenom k a další. Souhrnně tyto soubory bez zdrojového kódu se označují jako prostředky jsou kompilované během procesu sestavení (spolu s zdrojový kód) a zabalené jako APK pro distribuci a instalace do zařízení:

![Diagram balení](images/packaging-diagram.png)

Prostředky nabízí několik výhod do aplikace systému Android:

-  **Oddělení kódu** &ndash; odděluje zdrojového kódu z bitové kopie, řetězce, nabídek, animací, barvy, atd. Prostředky jako takový může pomoci podstatně při lokalizaci.

-  **Cíl více zařízení** &ndash; poskytuje jednodušší podporu konfigurací různých zařízení bez změny kódu.

-  **Kontroluje kompilaci** &ndash; prostředky jsou statické a kompilované do aplikace. To umožňuje využití prostředků, které mají být zkontrolovány při kompilaci, kdy budou snadno catch a opravit chyby, a běhu když je více obtížné vyhledat a nákladná opravit.

Při spuštění nového projektu Xamarin.Android je vytvořen speciální adresář s názvem prostředků, spolu s některé podadresáře:

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

![Složka prostředků a obsah](images/resources-folder-vs.png)

Na obrázku výše uvedené prostředky aplikace jsou uspořádány podle jejich typu do těchto podadresáře: budu patři bitové kopie **drawable** directory; přejděte zobrazení **rozložení** podadresáři atd.
 
# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

![Složka prostředků a obsah](images/resources-folder-xs.png)

Na obrázku výše uvedené prostředky aplikace jsou uspořádány podle jejich typu do těchto podadresáře: budu patři bitové kopie **mipmap** directory; přejděte zobrazení **rozložení** podadresáři atd.
 
-----

Existují dva způsoby pro přístup k těchto prostředků ve aplikace pro Xamarin.Android: *prostřednictvím kódu programu* v kódu a *deklarativně* v kódu XML pomocí speciální syntaxe jazyka XML.

Tyto prostředky se nazývají *výchozí prostředky* a jsou používány všechna zařízení, je-li uvedeno více konkrétních shody. Kromě toho může volitelně může mít každý typ prostředku *alternativní prostředky* , Android, může použít pro konkrétní zařízení. Například mohou být prostředky k dispozici pro národní prostředí uživatele, pro obrazovku, nebo pokud zařízení není otočený o 90 stupňů z výšky na šířku, atd. V každé z těchto případů Android načte prostředky pro použití aplikací bez jakékoli velmi kódování úsilí sám vývojář.

Alternativní prostředky jsou zadány přidáním krátký řetězec, názvem *kvalifikátor*, na konec objektu adresáře, která uchovává daného typu prostředků.

Například **prostředky/drawable-de** bude specifikujte bitové kopie pro zařízení, které jsou nastavené na němčině národním prostředí, při **prostředky/drawable-fr** by uložení bitové kopie pro zařízení, nastavte francouzské národní prostředí. Příklad poskytnout alternativní prostředky můžete zobrazit na obrázku níže, kde je spuštěn stejnou aplikaci právě v národním prostředí změny zařízení:

![Příklad obrazovky pro různá národní prostředí](images/localized-screenshots.png)

Tento článek bude trvat komplexní pohled na použití prostředků a zahrnovat následující témata:

-  **Android základy prostředků** &ndash; pomocí výchozí prostředky programově, tak i deklarativně, přidávání typů prostředků, jako jsou bitové kopie a písem do aplikace.

-  **Konkrétní konfigurací zařízení** &ndash; podpora různá rozlišení obrazovky a densities – v aplikaci.

-  **Lokalizace** &ndash; pomocí prostředků pro podporu různých oblastech aplikace mohou být použity.


## <a name="related-links"></a>Související odkazy

- [Používání assetů Androidu](~/android/app-fundamentals/resources-in-android/android-assets.md)
- [Principy aplikací](http://developer.android.com/guide/topics/fundamentals.html)
- [Prostředky aplikace](http://developer.android.com/guide/topics/resources/index.html)
- [Podpora více obrazovek](http://developer.android.com/guide/practices/screens_support.html)
