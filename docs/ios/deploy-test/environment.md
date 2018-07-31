---
title: Prostředí pro spouštění pro aplikace Xamarin.iOS
description: Tento dokument popisuje, jak nastavit proměnné prostředí dočasné a trvalé pro aplikace Xamarin.iOS. Proměnné lze ve vlastnostech projektu nebo jako další argumenty mtouch balení nástroji.
ms.prod: xamarin
ms.assetid: 9801644A-89BB-4491-AD28-7F3B97D2CD62
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 06/05/2017
ms.openlocfilehash: 5296f03cae28e1025c760004c520a2b415ec493d
ms.sourcegitcommit: aa9b9b203ab4cd6a6b4fd51e27d865e2abf582c1
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/30/2018
ms.locfileid: "39351579"
---
# <a name="execution-environment-for-xamarinios-apps"></a>Prostředí pro spouštění pro aplikace Xamarin.iOS

*Prostředí pro spouštění* je sada proměnných prostředí, které ovlivňují provádění programu. Proměnné prostředí můžete nastavit dočasně ve vlastnostech projektu nebo trvale tak, že zadáte další argumenty mtouch balení nástroji.

## <a name="temporary-environment-variables"></a>Dočasné proměnné

Dočasných proměnných prostředí jsou nastaveny v projektu **vlastnosti**/**možnosti** okna **spuštění > Obecné** oddílu. Tyto proměnné prostředí platí pouze při spuštění aplikace pomocí sady Visual Studio pro Mac, je-li aplikace spuštěna ručně po klepnutí na to, které tyto proměnné prostředí nejsou nastavené.

## <a name="permanent-environment-variables"></a>Trvalé proměnné

Proměnné trvalé prostředí jsou nastaveny tak, že zadáte další argumenty mtouch balení nástroji. Tyto proměnné prostředí jsou zkompilovány do spustitelného souboru a bude nastavena, i v případě, že aplikace není spuštěn ze sady Visual Studio pro Mac.

## <a name="example"></a>Příklad

```csharp
# log all exceptions to the device log
--setenv:MONO_TRACE=E:all

# to pass multipe environment variables, use --setenv multiple times
--setenv:MONO_TRACE=E:all --setenv:GC_DONT_GC=1
```

