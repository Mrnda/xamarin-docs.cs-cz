---
title: Instalace balíčků které sady SDK pro Android
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: F136AAE0-C6D2-4B0F-8F8C-7A6A94877266
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 05/30/2018
ms.openlocfilehash: df359fae545079c7ac1d7106ec86e9297f183f90
ms.sourcegitcommit: a7febc19102209b21e0696256c324f366faa444e
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 06/04/2018
ms.locfileid: "34732772"
---
# <a name="which-android-sdk-packages-should-i-install"></a>Instalace balíčků které sady SDK pro Android

Instalace sady Android SDK neobsahuje automaticky všechny minimální požadované balíčky pro vývoj. Při jednotlivých vývojáře musí lišit, následující balíčky obecně nutné pro vývoj pomocí Xamarin.Android:

## <a name="tools"></a>Nástroje

Nainstalujte všechny nejnovější nástroje ve složce nástroje ve Správci SDK:

- Nástroje sady SDK pro Android
- Nástrojů platformy Android SDK
- Android SDK – nástroje pro vytváření

## <a name="android-platforms"></a>Platformy Android

Nainstalujte "SDK platformy" pro Android, které jste nastavili jako minimální & cílové verze. 

Příklady:

- Cílové rozhraní API 23
- Minimální rozhraní API 23

Stačí nainstalovat sadu SDK platformy pro rozhraní API 23

- Cílové rozhraní API 23
- Minimální rozhraní API 15

Třeba nainstalovat sadu SDK platformy pro rozhraní API 15 a 23. Všimněte si, že není potřeba nainstalovat úrovně rozhraní API mezi minimální a cílem (i v případě, že jste backporting na tyto úrovně rozhraní API).

## <a name="system-images"></a>Bitové kopie systému

Toto jsou jenom potřeba, pokud chcete použít na více systémů pole emulátorů Android z Google. Další informace najdete v tématu [emulátoru Android – nastavení](~/android/get-started/installation/android-emulator/index.md)

## <a name="extras"></a>Funkce
Android SDK funkce nejsou obvykle nutné; ale je užitečné zajímat z nich, protože mohou být vyžadovány v závislosti na váš případ použití.

## <a name="further-reading"></a>Další čtení
Následující příručka obsahuje tyto možnosti a přejde do další podrobnosti o různých balíčcích, sady SDK manager má k dispozici: [Průvodce nastavením Android SDK Manager](http://www.themethodology.net/2015/02/android-sdk-manager-setup-for.html?m=1)

