---
title: Jak se downgrade balíčku NuGet?
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 2375F833-A630-471E-B8E9-5AD2CB81F264
author: asb3993
ms.author: amburns
ms.date: 05/08/2018
ms.openlocfilehash: 72fdf7246b148fa95ea312284957072ecda47121
ms.sourcegitcommit: aa9b9b203ab4cd6a6b4fd51e27d865e2abf582c1
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/30/2018
ms.locfileid: "39351213"
---
# <a name="how-do-i-downgrade-a-nuget-package"></a>Jak se downgrade balíčku NuGet?

Visual Studio pro Mac a Visual Studio k dispozici funkce pro starší verze balíčků výběru a instalace je automaticky. Podobně jako způsob aktualizace balíčků. Tyto kroky jsou popsané níže.

## <a name="visual-studio"></a>Visual Studio
1. Přejděte na **nástroje > Správce balíčků NuGet > Konzola správce balíčků**
2. Nastavení projektu v rámci **výchozí projekt**
3. Použijte následující syntaxi:

    > [Název balíčku] Install-Package-verze [kartu pro nabídky verze]

Můžete také zkopírovat a vložit přesný příkaz ze stránky balíčku NuGet. Příklad pro Xamarin.Forms. [https://www.nuget.org/packages/Xamarin.Forms/](https://www.nuget.org/packages/Xamarin.Forms/)

## <a name="visual-studio-for-mac"></a>Visual Studio for Mac
1. Ve vašem projektu, klikněte pravým tlačítkem na složku packages příkazu & select **přidat balíčky**
2. V searchbar můžete k vyhledání požadované balíčky následující syntaxi:

    `[PackageName] version:*`

### <a name="examples"></a>Příklady 
- Zobrazí seznam všech balíčků Xamarin.Forms: 

    `Xamarin.Forms version:`
- Zobrazí seznam všech balíčků 1.4.x Xamarin.Forms: 

    `Xamarin.Forms version:1.4`

*Poznámka: Pokud chcete přidat mezeru mezi `version:` & číslo verze, hledání se chovat, jako by nebyla zadána žádná verze.*

