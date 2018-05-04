---
title: Začínáme s Javou
ms.prod: xamarin
ms.assetid: B9A25E9B-3EC2-489A-8AD3-F78287609747
ms.technology: xamarin-cross-platform
author: topgenorth
ms.author: toopge
ms.date: 03/28/2018
ms.openlocfilehash: e61e610de9186978e2924c0e69e7517a39a54f04
ms.sourcegitcommit: 4b0582a0f06598f3ff8ad5b817946459fed3c42a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 05/03/2018
---
# <a name="getting-started-with-java"></a>Začínáme s Javou

Toto je úvodní stránce for Java, které popisuje základní informace pro všechny podporované platformy.

## <a name="requirements"></a>Požadavky

Použití rozhraní .NET vkládání s Java, budete potřebovat:

* Java 1,8 nebo novější
* [Mono 5.0](http://www.mono-project.com/download/)

Pro počítače Mac:

* Xcode 8.3.2 nebo novější

Pro Windows:

* Visual Studio 2017 s podpory C++
* Windows 10 SDK

Pro Android:

* [Xamarin.Android 7.5](https://www.visualstudio.com/xamarin/) nebo novější
* [Android Studio 3.x](https://developer.android.com/studio/index.html) s Javou 1.8

Můžete použít [Visual Studio pro Mac](https://www.visualstudio.com/vs/visual-studio-mac/) upravit a kompilace kódu C#.

> [!NOTE]
> Starší verze Xcode, Visual Studio, Xamarin.Android, Android Studio a Mono _může_ pracovat, ale testována a není podporována.

## <a name="installation"></a>Instalace

Vložení .NET je aktuálně k dispozici na [NuGet](https://www.nuget.org/packages/Embeddinator-4000/):

```shell
nuget install Embeddinator-4000
```

To umístí **Embeddinator 4000.exe** do **balíčky nebo Embeddinator-4000 nebo nástroje** adresáře.

Kromě toho lze sestavení .NET vložení ze zdroje, naleznete v našem [úložiště git](https://github.com/mono/Embeddinator-4000/) a [přidání](https://github.com/mono/Embeddinator-4000/blob/master/Contributing.md) dokumentu pokyny.

## <a name="platforms"></a>Platformy

Java je aktuálně ve verzi preview stavu systému macOS, Windows a Android.

Platforma je vybrána předáním `--platform=<platform>` argument příkazového řádku k nástroji vložení .NET. Aktuálně `macOS`, `Windows`, a `Android` jsou podporovány.

### <a name="macos-and-windows"></a>systému macOS a systému Windows

Pro vývoj by měl být možné používat všechny IDE Java, která podporuje Java 1.8. Android Studio můžete použít i pro to v případě potřeby [zde](https://stackoverflow.com/questions/16626810/can-android-studio-be-used-to-run-standard-java-projects). Výstupní soubor JAR můžete použít, stejně jako všechny standardní soubor jar Java.

### <a name="android"></a>Android

Zkontrolujte prosím, že jste už nastavili k vývoji aplikací pro Android, než se pokusíte vytvořit pomocí rozhraní .NET vložení. [Pokynů](~/tools/dotnet-embedding/get-started/java/android.md) předpokládají, že jste již úspěšně vytvořené a nasazené aplikace pro Android z vašeho počítače.

Android Studio se doporučuje pro vývoj, ale jiné integrovaného vývojového prostředí by měla fungovat, dokud se podporují [formát souboru AAR](https://developer.android.com/studio/projects/android-library.html).

## <a name="further-reading"></a>Další čtení

* [Začínáme v systému Android](~/tools/dotnet-embedding/get-started/java/android.md)
* [Zpětná volání v systému Android](~/tools/dotnet-embedding/android/callbacks.md)
* [Předběžné Android Research](~/tools/dotnet-embedding/android/index.md)
* [Vložení omezení rozhraní .NET](~/tools/dotnet-embedding/limitations.md)
* [Na projektu s otevřeným zdrojem](https://github.com/mono/Embeddinator-4000/blob/master/Contributing.md)
* [Kódy a popisy chyb](~/tools/dotnet-embedding/errors.md)
