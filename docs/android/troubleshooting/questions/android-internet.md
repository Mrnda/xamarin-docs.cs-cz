---
title: Proč můj sestavení pro vydání Android se připojit k Internetu?
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: A6FE770B-A19A-4BF8-95E9-2CF880D4AFC5
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 03/09/2018
ms.openlocfilehash: 7f956defd0243e1927746a53e6b3b1b05d98f8d2
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/04/2018
---
# <a name="why-cant-my-android-release-build-connect-to-the-internet"></a>Proč můj sestavení pro vydání Android se připojit k Internetu?

## <a name="cause"></a>příčina

Nejčastější příčinou tohoto problému je, že **INTERNET** oprávnění je automaticky součástí sestavení ladicí verze, ale musíte ručně nastavit pro sestavení pro vydání. Důvodem je, že oprávnění k Internetu používá k povolení ladicí program připojit k procesu, jak je popsáno pro "DebugSymbols" [zde](~/android/deploy-test/building-apps/build-process.md).


## <a name="fix"></a>Oprava

Chcete-li vyřešit tento problém, můžete vyžadovat oprávnění k Internetu v Android Manifest. To lze provést pomocí editoru manifestu nebo zdrojový do manifestu:

-   Opravte v editoru: přejděte ve vašeho projektu Android **vlastnosti -> AndroidManifest.xml -> požadovaných oprávnění** a zkontrolujte **Internetu**

-   Odstraňte zdrojový: Otevřete AndroidManifest v editoru zdroje a přidání značka oprávnění uvnitř `<Manifest>` značky:

    ```xml
    <Manifest>
    ...
    <uses-permission android:name="android.permission.INTERNET" />
    </Manifest>
    ```
