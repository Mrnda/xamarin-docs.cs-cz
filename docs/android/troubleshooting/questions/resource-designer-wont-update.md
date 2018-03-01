---
title: Moje Android Resource.designer.cs soubor nebude aktualizovat.
ms.topic: article
ms.prod: xamarin
ms.assetid: 3F7376E3-59CC-4722-AEED-BB50E4D952AA
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 06/19/2017
ms.openlocfilehash: b169bcc64af15de3d87bfb7f8059b4251f1a3ad9
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 02/27/2018
---
# <a name="my-android-resourcedesignercs-file-will-not-update"></a>Moje Android Resource.designer.cs soubor nebude aktualizovat.

> [!NOTE]
> **Poznámka:** tento problém byl vyřešen v Xamarin Studio 5.1.4 a novějších verzích. Ale pokud k tomuto problému dochází v sadě Visual Studio pro Mac, prosím soubor [nové chyb](~/cross-platform/troubleshooting/questions/howto-file-bug.md) s vaší úplná Správa verzí informace a úplné sestavení výstup protokolu.

Chyby v Xamarin.Studio 5.1 dříve poškozené soubory .csproj odstraněním částečně nebo zcela kód xml v souboru .csproj. To by způsobilo důležité části Android sestavení systému (například aktualizaci Android Resource.designer.cs) k selhání. Od verze 5.1.4 stabilní verzi na 15. července, tato chyba byla opravena; ale v mnoha případech je třeba opravit ručně, jak je popsáno níže souboru projektu.


## <a name="two-possible-approaches-to-fixing-up-the-project-file"></a>Dva možné přístupy k opravě soubor projektu

### <a name="either"></a>Buď:

1) Vytvořit zcela nový projekt aplikace Xamarin.Android, nastavte všechny vlastnosti projektu tak, aby odpovídala staré projekt a přidejte všechny vaše prostředky, atd. zdrojové soubory zpět do projektu.

### <a name="or"></a>NEBO

2) Vytvořit záložní kopii souboru .csproj původní projektu, otevřete v textovém editoru a znovu přidají chybí elementy ze souboru .csproj řádně vygenerovaný.

### <a name="if-this-does-not-solve-the-problem"></a>Pokud tento problém nevyřeší

Po experimentování se tyto prvky, můžete si všimnout, po přidání zpět elementy a projekt znovu sestavit, soubor Resource.designer.cs by aktualizace, ale pak může pořád máte zavřete a znovu ho otevřete řešení získat doplňování kódu rozpoznat nové typy obsažené v Resource.designer.cs. 
