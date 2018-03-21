---
title: "Prostředí"
ms.topic: article
ms.prod: xamarin
ms.assetid: 9801644A-89BB-4491-AD28-7F3B97D2CD62
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.openlocfilehash: c46d3aa151d4a596ca3da881a3c4e5e38bb41cbd
ms.sourcegitcommit: d450ae06065d8f8c80f3588bc5a614cfd97b5a67
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 03/21/2018
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

