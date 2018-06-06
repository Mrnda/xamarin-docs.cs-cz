---
title: Prostředí pro spuštění aplikace Xamarin.iOS
description: Tento dokument popisuje, jak nastavit proměnné prostředí dočasné a trvalé pro aplikace Xamarin.iOS. Proměnné mohou být zadané ve vlastnostech projektu nebo jako další argumenty pro nástroj balení mtouch.
ms.prod: xamarin
ms.assetid: 9801644A-89BB-4491-AD28-7F3B97D2CD62
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.openlocfilehash: 246c65729f9327dd1ccf549603b4c2b1feb023e8
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 06/05/2018
ms.locfileid: "34784963"
---
# <a name="execution-environment-for-xamarinios-apps"></a>Prostředí pro spuštění aplikace Xamarin.iOS

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

