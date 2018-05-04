---
title: Začínáme s jazyka Objective-C
ms.prod: xamarin
ms.assetid: 4ABC0247-B608-42D4-89CB-D2E598097142
ms.technology: xamarin-cross-platform
author: topgenorth
ms.author: toopge
ms.date: 11/14/2017
ms.openlocfilehash: 1769c2cd5e9f68e80f7f4c0ef5e2393971b659f9
ms.sourcegitcommit: 4b0582a0f06598f3ff8ad5b817946459fed3c42a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 05/03/2018
---
# <a name="getting-started-with-objective-c"></a>Začínáme s jazyka Objective-C

Toto je úvodní stránce pro Objective-C, která popisuje základní informace pro všechny podporované platformy.

## <a name="requirements"></a>Požadavky

Použít vložení .NET pomocí jazyka Objective-C, budete potřebovat s adresou Mac:

* systému macOS 10.12 (Sierra) nebo novější
* Xcode 8.3.2 nebo novější
* [Mono 5.0](http://www.mono-project.com/download/)

Můžete nainstalovat [Visual Studio pro Mac](https://www.visualstudio.com/vs/visual-studio-mac/) upravit a kompilace kódu C#.

> [!NOTE]
> * Dřívějších verzích systému macOS, Xcode a Mono _může_ pracovat, ale testována a nepodporované
> * Generování kódu lze provést v systému Windows, ale je možné zkompilovat na počítači Mac, kde je nainstalován Xcode

## <a name="installing-net-embedding-from-nuget"></a>Instalace rozhraní .NET vložení z NuGet

Postupujte podle těchto [pokyny](~/tools/dotnet-embedding/get-started/install/install.md) k instalaci a konfiguraci vložení .NET pro váš projekt.

Vyvolání typu Ukázkový příkaz, je uvedena ve [systému macOS](~/tools/dotnet-embedding/get-started/objective-c/macos.md) a [iOS](~/tools/dotnet-embedding/get-started/objective-c/ios.md) Začínáme příručky.

## <a name="platforms"></a>Platformy

Jazyka Objective-C je jazyk, který se nejčastěji používá k psaní aplikací pro systému macOS, iOS, tvOS a watchOS; vložení .NET podporuje všechny tyto platformy. Práce s každou platformu znamená některé [jsou zde vysvětleny hlavní rozdíly a tyto](~/tools/dotnet-embedding/objective-c/platforms.md).

### <a name="macos"></a>macOS

[Vytvoření aplikace systému macOS](~/tools/dotnet-embedding/get-started/objective-c/macos.md) je nejjednodušší, protože nezahrnuje tolik další kroky, jako je nastavení identity, provisining profily, simulátorů a zařízení. Můžete se doporučuje začínat dokumentu systému macOS před pro iOS.

### <a name="ios--tvos"></a>iOS / tvOS

Zkontrolujte prosím, že jste už nastavili pro vývoj aplikací pro iOS než se pokusíte vytvořit pomocí rozhraní .NET vložení. [Pokynů](~/tools/dotnet-embedding/get-started/objective-c/ios.md) předpokládají, že jste již úspěšně vytvořené a nasazené aplikace pro iOS z vašeho počítače.

Podporu pro tvOS je obdobou jak iOS funguje, jenom pomocí tvOS projekty v integrovaného vývojového prostředí (Visual Studio a Xcode) namísto iOS projekty.

> [!NOTE]
> Podpora pro watchOS bude v budoucí verzi k dispozici a bude velmi podobný iOS/tvOS.

## <a name="further-reading"></a>Další čtení

* [Vložení .NET funkce specifické pro Objective-C](~/tools/dotnet-embedding/objective-c/index.md)
* [Osvědčené postupy pro Objective-C](~/tools/dotnet-embedding/objective-c/best-practices.md)
* [Vložení omezení rozhraní .NET](~/tools/dotnet-embedding/limitations.md)
* [Na projektu s otevřeným zdrojem](https://github.com/mono/Embeddinator-4000/blob/master/Contributing.md)
* [Kódy a popisy chyb](~/tools/dotnet-embedding/errors.md)
* [Cílové platformy](~/tools/dotnet-embedding/objective-c/platforms.md)

## <a name="related-links"></a>Související odkazy

- [Ukázka počasí (iOS & systému macOS)](https://github.com/jamesmontemagno/embeddinator-weather)
