---
title: Jak můžu shromáždit aktuální zásobníky volání procesu Visual Studio?
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 64c24b09-2c4a-43ad-b94d-6cd05a1aee44
ms.technology: xamarin-cross-platform
author: asb3993
ms.author: amburns
ms.date: 03/30/2017
ms.openlocfilehash: 45ad6cc93d14fc1da077a40abea2c25668fb2269
ms.sourcegitcommit: 6f7033a598407b3e77914a85a3f650544a4b6339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/06/2018
---
# <a name="how-do-i-collect-the-current-call-stacks-of-the-visual-studio-process"></a>Jak můžu shromáždit aktuální zásobníky volání procesu Visual Studio?

Když v grafickém uživatelském rozhraní uzamkne (zablokuje, zablokuje se) v sadě Visual Studio, je důležitou shromažďovat diagnostické informace sada zásobníky volání z všechna vlákna procesu Visual Studio. Pokud chcete uložit tyto informace pro instanci "zamrzlých" sady Visual Studio, můžete druhou instanci sady Visual Studio:

1. Spusťte druhou instanci (nové okno) sady Visual Studio.

2. Zavřete všechny otevřené řešení v nové instanci sady Visual Studio.

3. Vyberte **ladění > připojit k procesu**.

  ![](vs-callstack-images/image1.png "Vyberte Ladění > připojit k procesu")

4. Vyberte původní "zamrzlých" instanci `devenv.exe` ze seznamu **dostupné procesy**.

5. Vyberte **ladění > přerušení všech**.

  ![](vs-callstack-images/image2.png "Vyberte Ladění > Rozdělit všechny")

6. Vyberte **ladění > Uložit výpisu jako**.

  ![](vs-callstack-images/image3.png "Vyberte Ladění > Uložit výpisu jako")

7. Změna **uložit jako typ** k **minimální výpis (\*.dmp)**. Vznikne tak mnohem menší soubor než **minimální výpis s haldy**, a halda není obvykle relevantní pro diagnostiku zablokuje.

  ![](vs-callstack-images/image4.png "Vznikne tak mnohem soubor menší než minimální výpis s haldy a halda není obvykle relevantní pro diagnostiku zablokuje")

8. Uložte soubor výpisu. Pokud odesílání souboru v režimu online, můžete ji ke snížení velikosti zip.
