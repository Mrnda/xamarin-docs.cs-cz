---
title: Prostředí
ms.prod: xamarin
ms.assetid: 9801644A-89BB-4491-AD28-7F3B97D2CD62
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.openlocfilehash: bc06ce3f3a26842340ce6e19741a8a7dfe8f086d
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/04/2018
---
# <a name="environment"></a>Prostředí

*Prostředí pro spuštění* je sada proměnných prostředí, které ovlivňují spuštění programu. Proměnné prostředí lze nastavit dočasně ve vlastnostech projektu nebo trvale tak, že zadáte další argumenty, které mají nástroje balení mtouch.

## <a name="temporary-environment-variables"></a>Proměnné dočasného prostředí

Proměnné dočasného prostředí se nastavují v projektu **vlastnosti**/**možnosti** okno v **spustit > Obecné** části. Tyto proměnné prostředí jsou pouze v platnost při spuštění aplikace pomocí sady Visual Studio pro Mac, je-li aplikace spuštěna ručně klepnutím na něm, které nejsou nastaveny těchto proměnných prostředí.

## <a name="permanent-environment-variables"></a>Proměnné prostředí trvalé

Proměnné prostředí trvalé jsou nastavené tak, že zadáte další argumenty, které mají nástroje balení mtouch. Tyto proměnné prostředí jsou zkompilovány do spustitelného souboru a bude nastavena, i v případě, že aplikace není spuštění ze sady Visual Studio for Mac.

## <a name="example"></a>Příklad

```csharp
# log all exceptions to the device log
--setenv:MONO_TRACE=E:all

# to pass multipe environment variables, use --setenv multiple times
--setenv:MONO_TRACE=E:all --setenv:GC_DONT_GC=1
```

