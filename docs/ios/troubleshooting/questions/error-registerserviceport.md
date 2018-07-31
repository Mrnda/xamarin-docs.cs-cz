---
title: Chyba iOS designeru s RegisterServicePort
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 929A0080-B126-4744-BF88-A4A1EFBB6CC2
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 04/03/2018
ms.openlocfilehash: 9edcc822b170c3463908b9f5fb1db8b798346e3e
ms.sourcegitcommit: aa9b9b203ab4cd6a6b4fd51e27d865e2abf582c1
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/30/2018
ms.locfileid: "39350703"
---
# <a name="ios-designer-error-with-registerserviceport"></a>Chyba iOS designeru s RegisterServicePort

## <a name="sample-error"></a>Ukázka chyby
> System.AggregateException: Došlo k jedné nebo více chybám---> System.SystemException –: RegisterServicePort (com.xamarin.MTHosting.2a0b1, com.apple.PowerManagement.control): vrátí jádra:-308 (-308): server (ipc/mig) se ukončila.

## <a name="explanation"></a>Vysvětlení
Chyby s `RegisterServicePort` a chybové zprávy podobné jako výše jsou obvykle problém s spyware/malwaru v počítači. Zvažte prosím možnost [komentáře v této sestavě chyb](https://bugzilla.xamarin.com/show_bug.cgi?id=21907#c4) pro víc informací, včetně propojení [diskuzi na fóru Apple](https://discussions.apple.com/thread/5596008) o tom, jak odebrat možné infekce. 

Chcete-li pomoci při diagnostice problému, otevřete aplikaci macOS **konzoly** a odstranit každý soubor v **diagnostické zprávy uživateli** části [ http://screencast.com/t/y9i3NKcuMy ](http://screencast.com/t/y9i3NKcuMy). Potom spusťte Visual Studio pro Mac a pokuste se použít návrháře. Pokud všechny nové soubory protokolu se v této části po návrháře se nepodařilo inicializovat, uložte prosím tyto nám k analýze.  

Mějte prosím na paměti, že je nejdůležitější, čímž vyhledáte tento soubor: 
> /usr/lib/libimckit.dylib

Bez ohledu na výše uvedené výsledky, pokud tento soubor existuje, výše uvedené spywaru nebo malware problém je k dispozici ve vašem počítači.  

Následující odkaz obsahuje kroky k odebrání spywaru nebo malwaru: [http://www.thesafemac.com/arg-genieo/](http://www.thesafemac.com/arg-genieo/)  

