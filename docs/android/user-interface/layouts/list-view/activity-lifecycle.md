---
title: ListView a životního cyklu aktivity
ms.prod: xamarin
ms.assetid: 40840D03-6074-30A2-74DA-3664703E3367
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/06/2018
ms.openlocfilehash: 6e15fb8796ae6a616c5eae44059caae3d9478aef
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/04/2018
ms.locfileid: "30764216"
---
# <a name="listview-and-the-activity-lifecycle"></a>ListView a životního cyklu aktivity

Aktivity projít určité stavy jako vaše aplikace běží, jako je například spuštění, spuštění, proto byla pozastavena a bude zastavena. Další informace a na zpracování přechodů mezi stavy konkrétní pokyny najdete v tématu [kurzu životního cyklu aktivity](~/android/app-fundamentals/activity-lifecycle/index.md).
Je důležité pochopit životní cyklus aktivity a místní vaší `ListView` kódu ve správném umístění.

Všechny příklady v tomto dokumentu provést v rámci aktivity, nastavení úlohy, `OnCreate` metoda a (Pokud je vyžadováno) provádět, rušením' v `OnDestroy`. Příklady obecně používají malých datových sad, které se nemění, takže znovu častěji načítání dat je zbytečné.

Ale pokud se vaše data se často mění nebo používá velké množství paměti Toto může být vhodné použít jiný životního cyklu metody k naplnění a aktualizovat vaše `ListView`. Například, pokud zadaná data se neustále mění (nebo může být ovlivněno aktualizací v jiných aktivit) a vytvoření adaptéru v `OnStart` nebo `OnResume` zajistí nejnovější data se zobrazí pokaždé, když se zobrazí aktivity.

Pokud karta používá prostředky jako paměti nebo spravované kurzoru, mějte na paměti k uvolnění těchto prostředků v metodě doplňkové na místech jejich byly instalace (např. objekty vytvořené v `OnStart` může probíhat za v `OnStop`).


## <a name="configuration-changes"></a>Změny konfigurace

Je důležité si pamatovat, tato konfigurace se změní &ndash; zejména obrazovky otočení a klávesnice viditelnost &ndash; může způsobit, že aktuální aktivita pro zrušen a znovu vytvořit (Pokud neurčíte jinak pomocí `ConfigurationChanges` atribut). To znamená, že za normálních podmínek, otáčení zařízení způsobí, že `ListView` a `Adapter` být znovu vytvořena a (Pokud jste kód napsali `OnPause` a `OnResume`) posunutí pozice a řádek výběru stavy, budou ztraceny.

Následující atribut by jinak znemožňovaly aktivitu z se zrušen a znovu vytvořit v důsledku změny konfigurace:

```csharp
[Activity(ConfigurationChanges="keyboardHidden|orientation")]
```

Potom by měly přepsat aktivity `OnConfigurationChanged` odpovídajícím způsobem odpovědět na tyto změny. Další informace o tom, jak zpracovat změny konfigurace naleznete v dokumentaci.

