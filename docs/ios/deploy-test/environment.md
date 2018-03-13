---
title: "Prostředí"
ms.topic: article
ms.prod: xamarin
ms.assetid: 9801644A-89BB-4491-AD28-7F3B97D2CD62
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.openlocfilehash: 7489c2fe38e8433811c5f298296baebacf1c0727
ms.sourcegitcommit: 0fdb243b46cf21be47584900805cadcd077121bf
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 03/12/2018
---
# <a name="environment"></a>Prostředí

*Prostředí pro spuštění* je sada proměnných prostředí, které ovlivňují spuštění programu. Proměnné prostředí lze nastavit dočasně ve vlastnostech projektu nebo trvale tak, že zadáte další argumenty, které mají nástroje balení mtouch.

## <a name="temporary-environment-variables"></a>Proměnné dočasného prostředí

Proměnné dočasného prostředí se nastavují v projektu **vlastnosti**/**možnosti** okno v **spustit > Obecné** části. Tyto proměnné prostředí jsou pouze v platnost při spuštění aplikace pomocí sady Visual Studio pro Mac, je-li aplikace spuštěna ručně klepnutím na něm, které nejsou nastaveny těchto proměnných prostředí.

## <a name="permanent-environment-variables"></a>Proměnné prostředí trvalé

Proměnné prostředí trvalé jsou nastavené tak, že zadáte další argumenty, které mají nástroje balení mtouch. Tyto proměnné prostředí jsou zkompilovány do spustitelného souboru a bude nastavena, i když není aplikace spuštěna pomocí Xamarin Studio.

## <a name="example"></a>Příklad

```csharp
# log all exceptions to the device log
--setenv:MONO_TRACE=E:all

# to pass multipe environment variables, use --setenv multiple times
--setenv:MONO_TRACE=E:all --setenv:GC_DONT_GC=1
```

