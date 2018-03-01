---
title: "Začínáme s C"
ms.topic: article
ms.prod: xamarin
ms.assetid: 2A27BE0F-95FB-4C3A-8A43-72540179AA85
ms.technology: xamarin-cross-platform
author: topgenorth
ms.author: toopge
ms.date: 11/14/2017
ms.openlocfilehash: ea8335348416dc00074d83e09b74521da7abcb66
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 02/27/2018
---
# <a name="getting-started-with-c"></a>Začínáme s C


## <a name="requirements"></a>Požadavky

Použití rozhraní .NET vkládání s C budete potřebovat spuštěný počítače Mac nebo Windows:

* systému macOS 10.12 (Sierra) nebo novější
* Xcode 8.3.2 nebo novější

* Windows 7, 8, 10 nebo novější
* Visual Studio 2013 nebo novější

* [Mono](http://www.mono-project.com/download/)


## <a name="installation"></a>Instalace

Dalším krokem je ke stažení a instalaci nástrojů .NET vložení na váš počítač.

Binární sestavení pro C a Java generátory ještě nejsou k dispozici, ale budou brzo dostupné.

Případně je možné vytvořit z našich úložiště git, najdete v článku [přidání](https://github.com/mono/Embeddinator-4000/blob/master/docs/Contributing.md) dokumentu pokyny.


## <a name="generation"></a>Generování

Ke generování kódu jazyka C, invoke .NET vložení nástroj předávání správné příznaky pro jazyk C:

### <a name="windows"></a>Windows:

```csharp
$ build/lib/Debug/Embeddinator-4000.exe --gen=c --output=managed_c --platform=windows --compile managed.dll
```

Ujistěte se, že zavolat z příkazového prostředí sady Visual Studio konkrétní verzi sady Visual Studio jste, které se budou zaměřovat.

### <a name="macos"></a>macOS

```csharp
$ mono build/lib/Debug/Embeddinator-4000.exe --gen=c --output=managed_c --platform=macos --compile managed.dll
```

### <a name="output-files"></a>Výstupní soubory

Pokud všechno proběhne správně, zobrazí se následující výstup:

```csharp
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

Vzhledem k tomu `--compile` příznak byl předán nástroj, .NET vložení by také sestavili výstupní soubory do sdílené knihovny, které můžete vyhledat vedle generované soubory, `libmanaged.dylib` souboru v systému macOS, a `managed.dll` v systému Windows.

Používat sdílené knihovny, můžete zahrnout `managed.h` C záhlaví souboru, který poskytuje deklarace C odpovídající příslušné spravovanou knihovnu rozhraní API a odkaz s výše uvedených zkompilovat sdílenou knihovnu.

## <a name="further-reading"></a>Další čtení

* [Omezení Embeddinator](~/tools/dotnet-embedding/limitations.md)
* [Na projektu s otevřeným zdrojem](https://github.com/mono/Embeddinator-4000/blob/master/docs/Contributing.md)
* [Kódy a popisy chyb](~/tools/dotnet-embedding/errors.md)
