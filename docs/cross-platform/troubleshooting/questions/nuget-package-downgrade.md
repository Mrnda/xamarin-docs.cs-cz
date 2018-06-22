---
title: Jak se downgradovat balíček NuGet?
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 2375F833-A630-471E-B8E9-5AD2CB81F264
author: asb3993
ms.author: amburns
ms.openlocfilehash: 50a96340f8dada802303d6de140812801fdc836d
ms.sourcegitcommit: 0a72c7dea020b965378b6314f558bf5360dbd066
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 05/10/2018
ms.locfileid: "33947518"
---
# <a name="how-do-i-downgrade-a-nuget-package"></a>Jak se downgradovat balíček NuGet?

Visual Studio pro Mac & Visual Studio k dispozici funkce pro výběr starších verzí balíčků a jejich instalace automaticky. Podobně jako u jak aktualizace balíčků. Tyto kroky jsou popsané níže.

## <a name="visual-studio"></a>Visual Studio
1. Přejděte na **nástroje > Správce balíčků NuGet > Konzola správce balíčků**
2. Nastavte projekt pod **výchozí projekt**
3. Použijte následující syntaxi:

    > Install-Package [název balíčku]-verze [kartě nabídky verze]

Vám může také zkopírujte a vložte přesný příkaz z balíčku NuGet stránky. Příklad pro Xamarin.Forms. [https://www.nuget.org/packages/Xamarin.Forms/](https://www.nuget.org/packages/Xamarin.Forms/)

## <a name="visual-studio-for-mac"></a>Visual Studio for Mac
1. V projektu, klikněte pravým tlačítkem na složku balíčky & Vyberte **přidání balíčků**
2. V searchbar můžete pro vyhledání požadované balíčky následující syntaxi:

    `[PackageName] version:*`

### <a name="examples"></a>Příklady 
- Zobrazí seznam všech balíčků Xamarin.Forms: 

    `Xamarin.Forms version:`
- Zobrazí seznam všech balíčků 1.4.x Xamarin.Forms: 

    `Xamarin.Forms version:1.4`

*Poznámka: Pokud přidáte mezeru mezi `version:` & číslo verze hledání budou chovat, jako kdyby byla zadána žádná verze.*

