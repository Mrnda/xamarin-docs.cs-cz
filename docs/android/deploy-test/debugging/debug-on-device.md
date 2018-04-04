---
title: Ladění na zařízení
description: Tento článek vysvětluje postup ladění aplikace pro Xamarin.Android na fyzické zařízení Android.
ms.prod: xamarin
ms.assetid: 153D3746-A27F-198B-48FE-D219C0133A79
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/16/2018
ms.openlocfilehash: 1848bb624bf5f4bd627441a17fd077843c94edb9
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/04/2018
---
# <a name="debug-on-device"></a>Ladění na zařízení

_Tento článek vysvětluje postup ladění aplikace pro Xamarin.Android na fyzické zařízení Android._

## <a name="debug-on-device-overview"></a>Ladění na zařízení – přehled

Je možné k ladění aplikace Xamarin.Android na zařízení s Androidem pomocí sady Visual Studio pro Mac nebo Visual Studio. Než ladění dochází na zařízení, musí být [nastavení pro vývoj](~/android/get-started/installation/set-up-device-for-development.md) a připojený k PC nebo Mac.


## <a name="debug-application"></a>Ladění aplikace

Jakmile je zařízení připojené k počítači, ladění aplikace pro Xamarin.Android se provádí stejným způsobem jako ostatní Xamarin produktu nebo aplikace .NET. Ujistěte se, že **ladění** konfigurace a externí zařízení je vybraný v prostředí IDE, zajistíte, že jsou k dispozici nezbytné ladicími symboly a že IDE můžou připojit pro běžící aplikaci: 

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

![Vybrané konfiguraci ladění](debug-on-device-images/image1-vs.png)

Dále je nastavte zarážky v kódu:

![Breakpoint – nastavte na řádek kódu](debug-on-device-images/image2-vs.png)

Jakmile zařízení byla vybrána, bude Xamarin.Android se připojte k zařízení, nasaďte aplikaci a potom ho spusťte. Když je dosaženo zarážce, ladicí program zastaví aplikace, umožňuje aplikaci chcete ladit způsobem podobně jako všechny ostatní aplikace v C#: 

![Dosažení zarážek](debug-on-device-images/image3-vs.png)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

![Vybrané konfiguraci ladění](debug-on-device-images/image1-xs.png)

Dále je nastavte zarážky v kódu:

![Breakpoint – nastavte na řádek kódu](debug-on-device-images/image2-xs.png)

Jakmile zařízení byla vybrána, bude Xamarin.Android se připojte k zařízení, nasaďte aplikaci a potom ho spusťte. Když je dosaženo zarážce, ladicí program zastaví aplikace, umožňuje aplikaci chcete ladit způsobem podobně jako všechny ostatní aplikace v C#: 

![Dosažení zarážek](debug-on-device-images/image3-xs.png)

-----



## <a name="summary"></a>Souhrn

V tomto dokumentu popsané postupy k ladění aplikace pro Xamarin.Android nastavení boru přerušení a výběrem cílového zařízení.


## <a name="related-links"></a>Související odkazy

- [Nastavení zařízení pro vývoj](~/android/get-started/installation/set-up-device-for-development.md)
- [Nastavení Debuggable atribut](~/android/deploy-test/debuggable-attribute.md)
