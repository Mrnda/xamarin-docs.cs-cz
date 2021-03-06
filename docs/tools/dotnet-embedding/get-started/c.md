---
title: Začínáme s C
description: Tento dokument popisuje, jak používat vložení .NET pro vložení kódu .NET do aplikace C. Popisuje, jak používat .NET vložení v aplikaci Visual Studio 2017 a Visual Studio for Mac.
ms.prod: xamarin
ms.assetid: 2A27BE0F-95FB-4C3A-8A43-72540179AA85
author: topgenorth
ms.author: toopge
ms.date: 04/19/2018
ms.openlocfilehash: 248d44f23495e45d9d35b34622de0f3b85ca3e8d
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 06/05/2018
ms.locfileid: "34794095"
---
# <a name="getting-started-with-c"></a>Začínáme s C

## <a name="requirements"></a>Požadavky

Používat C vložení .NET, budete potřebovat spuštěný počítače Mac nebo Windows:

### <a name="macos"></a>macOS

* systému macOS 10.12 (Sierra) nebo novější
* Xcode 8.3.2 nebo novější
* [Mono](http://www.mono-project.com/download/)

### <a name="windows"></a>Windows

* Windows 7, 8, 10 nebo novější
* Visual Studio 2015 nebo novější

## <a name="installing-net-embedding-from-nuget"></a>Instalace rozhraní .NET vložení z NuGet

Postupujte podle těchto [pokyny](~/tools/dotnet-embedding/get-started/install/install.md) k instalaci a konfiguraci vložení .NET pro váš projekt.

Příkaz volání, které byste měli nakonfigurovat bude vypadat (případně s různých čísel verzí a cesty):

### <a name="visual-studio-for-mac"></a>Visual Studio for Mac

```shell
mono {SolutionDir}/packages/Embeddinator-4000.0.4.0.0/tools/Embeddinator-4000.exe --gen=c --outdir=managed_c --platform=macos --compile managed.dll
```

### <a name="visual-studio-2017"></a>Visual Studio 2017

```shell
$(SolutionDir)\packages\Embeddinator-4000.0.2.0.80\tools\Embeddinator-4000.exe --gen=c --outdir=managed_c --platform=windows --compile managed.dll
```

## <a name="generation"></a>Generování

### <a name="output-files"></a>Výstupní soubory

Pokud všechno proběhne správně, zobrazí se následující výstup:

```shell
Parsing assemblies...
    Parsed 'managed.dll'
Processing assemblies...
Generating binding code...
    Generated: managed.h
    Generated: managed.c
    Generated: mscorlib.h
    Generated: mscorlib.c
    Generated: embeddinator.h
    Generated: glib.c
    Generated: glib.h
    Generated: mono-support.h
    Generated: mono_embeddinator.c
    Generated: mono_embeddinator.h
```

Vzhledem k tomu `--compile` příznak byl předán nástroj, .NET vložení by také sestavili výstupní soubory do sdílené knihovny, které můžete vyhledat vedle generované soubory, **libmanaged.dylib** souboru v systému macOS a **managed.dll** v systému Windows.

Používat sdílené knihovny, můžete zahrnout **managed.h** C záhlaví souboru, který poskytuje deklarace C odpovídající příslušné spravovanou knihovnu rozhraní API a odkaz s výše uvedených zkompilovat sdílenou knihovnu.

## <a name="further-reading"></a>Další čtení

* [Vložení omezení rozhraní .NET](~/tools/dotnet-embedding/limitations.md)
* [Na projektu s otevřeným zdrojem](https://github.com/mono/Embeddinator-4000/blob/master/Contributing.md)
* [Kódy a popisy chyb](~/tools/dotnet-embedding/errors.md)
