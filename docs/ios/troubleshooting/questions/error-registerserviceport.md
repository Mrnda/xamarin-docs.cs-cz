---
title: "iOS Návrhář chyba s RegisterServicePort"
ms.topic: article
ms.prod: xamarin
ms.assetid: 929A0080-B126-4744-BF88-A4A1EFBB6CC2
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.openlocfilehash: 8042ea895de3a623279c9d5f3a5309b990489773
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 02/27/2018
---
# <a name="ios-designer-error-with-registerserviceport"></a>iOS Návrhář chyba s RegisterServicePort

## <a name="sample-error"></a>Ukázka chyby
> System.AggregateException: Došlo k jedné nebo více chybám---> System.SystemException: RegisterServicePort (com.xamarin.MTHosting.2a0b1, com.apple.PowerManagement.control): jádra vrátil:-308 (-308): (ipc/mig) serveru, byla

## <a name="explanation"></a>Vysvětlení
Chyby s `RegisterServicePort` a chybové zprávy podobné jako výše jsou běžně problém s spywaru nebo malwaru v počítači. . Zvažte prosím [komentář v této sestavě chyb](https://bugzilla.xamarin.com/show_bug.cgi?id=21907#c4) Další informace, společně s odkaz [diskuzi na fóru Apple](https://discussions.apple.com/thread/5596008) o tom, jak odebrat možné nakažení. 

Jako pomoc při diagnostikování problému, otevřete si aplikaci systému macOS **konzoly** a odstraňte všechny soubory uvnitř **uživatele diagnostické zprávy** části [http://screencast.com/t/y9i3NKcuMy](http://screencast.com/t/y9i3NKcuMy). Potom spusťte Visual Studio pro Mac a pokuste se použít návrháře. Pokud žádné nové soubory protokolu se v této části po návrháře se nepodařilo inicializovat, uložte tyto nemůžeme analyzovat.  

Uvědomte si, že tento soubor je nejdůležitější věcí, kterou chcete vyhledat: 
> /usr/lib/libimckit.dylib

Bez ohledu na výše uvedené výsledky, pokud tento soubor existuje, zmíněnými spywaru nebo malwaru problém je v počítači.  

Na následující odkaz obsahuje kroky k odebrání této spywaru nebo malwaru: [http://www.thesafemac.com/arg-genieo/](http://www.thesafemac.com/arg-genieo/)  

