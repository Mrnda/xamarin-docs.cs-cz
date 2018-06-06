---
title: Použití standardní knihovny .NET pro sdílení kódu
description: Tento dokument popisuje, jak sdílet kódu pomocí standardní knihovny .NET. Popisuje vytvoření .NET standardní knihovny, úpravy jeho nastavení a použití v aplikaci.
ms.prod: xamarin
ms.assetid: 8C30F8D3-1920-453E-9E8B-D40696736FF2
author: asb3993
ms.author: amburns
ms.date: 04/12/2017
ms.openlocfilehash: 82a89309e6462462471f42c3504d109ff0722917
ms.sourcegitcommit: 5db075bdd0b62d5d1d1567c267303a6a1888c8f2
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 06/06/2018
ms.locfileid: "34806787"
---
# <a name="using-net-standard-libraries-to-share-code"></a>Použití standardní knihovny .NET pro sdílení kódu

## <a name="net-standard"></a>Standardní rozhraní .NET

Standardní knihovny .NET je formální specifikaci rozhraní API .NET, která by měla být k dispozici na všechny moduly runtime rozhraní .NET. Motivace za standardní knihovna navazuje větší jednotnost v ekosystému .NET.
[ECMA 335](https://github.com/dotnet/coreclr/blob/master/Documentation/project-docs/dotnet-standards.md) pokračuje k navázání jednotnost chování modulu runtime rozhraní .NET, ale neexistuje žádné podobné specifikace pro knihovny pro třídy Base rozhraní .NET (BCL) pro implementace knihovny .NET.

Můžete si ho představit jako zjednodušenou, nové generace [Přenosná knihovna tříd](https://msdn.microsoft.com/library/gg597391.aspx).
Je jediná knihovna s jednotné rozhraní API pro všechny platformy .NET, včetně .NET Core. Stačí vytvořit jednu standardní knihovna pro .NET a použít ze všech modul runtime, který podporuje Standard platformy .NET.

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

## <a name="visual-studio-for-mac"></a>Visual Studio for Mac

Tato část vás provede postup vytvoření a použití standardní knihovny .NET pomocí sady Visual Studio for Mac. Informace naleznete v sekci Příklad standardní knihovny .NET pro dokončení implementace.

### <a name="creating-a-net-standard-library"></a>Vytvoření standardní knihovna pro .NET

Přidání standardní knihovna pro .NET pro vaše řešení je docela rovnou dál.

1. V dialogovém okně Přidat nový projekt, vyberte `.NET Core` kategorie a potom vyberte `Class Library(.NET Core)`.

  **Poznámka:** této šablony, bude přejmenován na `.NET Standard` v budoucí verzi sady Visual Studio for Mac.

  ![Vytvoření knihovny tříd .NET Core](net-standard-images/vsm01.png "vytvoření nové knihovny tříd rozhraní .NET Core")

2. Standardní knihovny .NET projektu se zobrazí, jak je znázorněno v Průzkumníku řešení. Uzel závislosti označí, že používá knihovna [NETStandard.Library](https://www.nuget.org/packages/NETStandard.Library/).

  ![Uzel závislosti v rámci řešení určuje .NET Standard](net-standard-images/vsm02.png)

#### <a name="editing-net-standard-library-settings"></a>Úprava nastavení standardní knihovny .NET

Nastavení standardní knihovny .NET můžete zobrazit a změnit tak, že kliknete pravým tlačítkem na projekt a výběrem `Options` jak je vidět na tomto snímku obrazovky:

![Upravit standardní rozhraní .NET framework cíl v možnosti projektu](net-standard-images/vsm03.png "upravit verzi rozhraní .NET Framework standardní cíl v možnosti projektu")

Můžete změnit ve vaší verzi `netstandard` změnou `Target Framework` hodnota rozevíracího seznamu.

**A dále:** můžete upravit `.csproj` přímo na tuto hodnotu změnit.

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

## <a name="visual-studio-2017-windows"></a>Visual Studio 2017 (Windows)

Tato část vás provede postup vytvoření a použití standardní knihovny .NET pomocí sady Visual Studio. Informace naleznete v sekci Příklad standardní knihovny .NET pro dokončení implementace.

### <a name="creating-a-net-standard-library"></a>Vytvoření standardní knihovna pro .NET

#### <a name="visual-studio-2017"></a>Visual Studio 2017

Přidání standardní knihovna pro .NET pro vaše řešení je docela rovnou dál.

1. V dialogovém okně Přidat nový projekt, vyberte `.NET Standard` kategorie a potom vyberte `Class Library(.NET Standard)`.

  ![Vytvoření nové knihovny tříd standardní .NET](net-standard-images/vs01.png "vytvořit nové standardní rozhraní .NET třídy knihovny")

2. Standardní knihovny .NET projektu se zobrazí, jak je znázorněno v Průzkumníku řešení. Uzel závislosti označí, že používá knihovna [NETStandard.Library](https://www.nuget.org/packages/NETStandard.Library/).

  ![NETStandard.Library ve složce projektu](net-standard-images/vs02.png ".NET Standard projekt v řešení")

#### <a name="editing-net-standard-library-settings"></a>Úprava nastavení standardní knihovny .NET

Nastavení standardní knihovny .NET můžete zobrazit a změnit tak, že kliknete pravým tlačítkem na projekt a výběrem `Properties` jak je vidět na tomto snímku obrazovky:

![Upravit standardní cílové rozhraní .NET v okně Vlastnosti projektu](net-standard-images/vs03.png "odkazovat .NET standardní knihovnu stejným způsobem jako jiné projekty")

Můžete změnit ve vaší verzi `netstandard` změnou `Target Framework` hodnota rozevíracího seznamu.

**A dále:** můžete upravit `.csproj` přímo na tuto hodnotu změnit.

#### <a name="using-net-standard-library"></a>Pomocí standardní knihovny .NET

Po vytvoření standardní knihovny .NET, můžete přidat odkaz na jeho ze žádného kompatibilní aplikace nebo knihovna projektu stejným způsobem jako za normálních okolností přidáte odkazy. V sadě Visual Studio, klikněte pravým tlačítkem na uzel odkazy a zvolte `Add Reference...` potom přepnout `Solution : Projects` kartě, jak je znázorněno:

![Odkazování na standardní knihovna pro .NET](net-standard-images/vs04.png "v sadě Visual Studio, klikněte pravým tlačítkem na uzel odkazy a zvolte možnost Přidat odkaz... potom přejděte na kartu řešení projekty, jak vidíte")

-----

