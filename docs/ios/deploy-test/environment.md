---
title: "Prostředí"
ms.topic: article
ms.prod: xamarin
ms.assetid: 67BFD4E1-276C-4B9F-9BD8-A5218D2BD529
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.openlocfilehash: ef5cfa2a9eee0d5d01922e5b7b3f89a209396c56
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 02/27/2018
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

