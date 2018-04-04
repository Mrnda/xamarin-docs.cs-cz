---
title: Začínáme s jazyka Objective-C
ms.prod: xamarin
ms.assetid: 4ABC0247-B608-42D4-89CB-D2E598097142
ms.technology: xamarin-cross-platform
author: topgenorth
ms.author: toopge
ms.date: 11/14/2017
ms.openlocfilehash: 96afe6bbfd1d9c6f4fd5ca3b7358ef0b89da30bb
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/04/2018
---
# <a name="getting-started-with-objective-c"></a>Začínáme s jazyka Objective-C

Toto je úvodní stránce pro Objective-C, která popisuje základní informace pro všechny podporované platformy.


## <a name="requirements"></a>Požadavky

Použití vložení .NET s jazyka Objective-C budete potřebovat s adresou Mac:

* systému macOS 10.12 (Sierra) nebo novější
* Xcode 8.3.2 nebo novější
* [Mono 5.0](http://www.mono-project.com/download/)

Můžete nainstalovat [Visual Studio pro Mac](https://www.visualstudio.com/vs/visual-studio-mac/) upravit a kompilace kódu C#.


Poznámky:

* Dřívějších verzích systému macOS, Xcode a Mono _může_ pracovat, ale testována a nepodporovaný;
* Generování kódu lze provést v systému Windows, ale je možné zkompilovat na počítači Mac, kde je nainstalován Xcode.


## <a name="installation"></a>Instalace

Dalším krokem je ke stažení a instalaci rozhraní .NET vložení na vaše Mac.

* [Balíček](https://dl.xamarin.com/embeddinator/Xamarin.Embeddinator-4000-0.2.0.79.pkg)
* [Zpráva k vydání verze](https://github.com/mono/Embeddinator-4000/tree/master/docs/releases)

Případně je možné vytvořit z našich [úložiště git](https://github.com/mono/Embeddinator-4000/tree/objc), najdete v článku [přidání](https://github.com/mono/Embeddinator-4000/blob/master/docs/Contributing.md) dokumentu pokyny.

Instalační program je instalační program standardní pkg na základě:

![Instalační program ÚVOD](images/install1.png)
![instalační program instalaci typu](images/install2.png)
![souhrn instalační program](images/install3.png)

Po instalaci pomocí instalačního programu, po spuštění novou relaci terminálu, můžete použít `objcgen` příkaz.
V opačném případě můžete vždy spustit nástroj přes jeho absolutní cestu: `/Library/Frameworks/Xamarin.Embeddinator-4000.framework/Commands/objcgen`.

## <a name="platforms"></a>Platformy

Jazyka Objective-C je jazyk, který se nejčastěji používá k psaní aplikací pro systému macOS, iOS, tvOS a watchOS; a embeddinator podporuje všechny tyto platformy. Práce s každou platformu znamená některé hlavní rozdíly a vysvětlení těchto najdete [zde](~/tools/dotnet-embedding/objective-c/platforms.md).

### <a name="macos"></a>macOS

[Vytvoření aplikace systému macOS](~/tools/dotnet-embedding/get-started/objective-c/macos.md) je nejjednodušší, protože nezahrnuje tolik další kroky, jako je nastavení identity, provisining profily, simulátorů a zařízení. Můžete se doporučuje začínat dokumentu systému macOS před pro iOS.

### <a name="ios--tvos"></a>iOS / tvOS

Zkontrolujte prosím, že jste už nastavili pro vývoj aplikací pro iOS než se pokusíte vytvořit pomocí embeddinator. [Pokynů](~/tools/dotnet-embedding/get-started/objective-c/ios.md) předpokládají, že jste již úspěšně vytvořené a nasazené aplikace pro iOS z vašeho počítače.

Podporu pro tvOS je obdobou jak iOS funguje, jenom pomocí tvOS projekty v integrovaného vývojového prostředí (Visual Studio a Xcode) namísto iOS projekty.

> [!NOTE]
> Poznámka: Podpora watchOS bude v budoucí verzi k dispozici a bude velmi podobný iOS/tvOS.


## <a name="further-reading"></a>Další čtení

* [Vložení .NET funkce specifické pro Objective-C](~/tools/dotnet-embedding/objective-c/index.md)
* [Osvědčené postupy pro Objective-C](~/tools/dotnet-embedding/objective-c/best-practices.md)
* [Vložení omezení rozhraní .NET](~/tools/dotnet-embedding/limitations.md)
* [Na projektu s otevřeným zdrojem](https://github.com/mono/Embeddinator-4000/blob/master/docs/Contributing.md)
* [Kódy a popisy chyb](~/tools/dotnet-embedding/errors.md)
* [Cílové platformy](~/tools/dotnet-embedding/objective-c/platforms.md)


## <a name="related-links"></a>Související odkazy

- [Ukázka počasí (iOS & systému macOS)](https://github.com/jamesmontemagno/embeddinator-weather)
