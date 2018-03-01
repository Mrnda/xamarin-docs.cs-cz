---
title: "Standardní rozhraní .NET"
ms.topic: article
ms.prod: xamarin
ms.assetid: 8C30F8D3-1920-453E-9E8B-D40696736FF2
ms.technology: xamarin-cross-platform
author: asb3993
ms.author: amburns
ms.date: 04/12/2017
ms.openlocfilehash: 294d28c57978218986d62d1ee6579e8d283b8f72
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 02/27/2018
---
# <a name="net-standard"></a>Standardní rozhraní .NET

## <a name="using-net-standard-library-projects-to-share-code"></a>Pomocí standardní projektů knihovny .NET sdílení kódu

Standardní knihovny .NET je formální specifikaci rozhraní API .NET, která by měla být k dispozici na všechny moduly runtime rozhraní .NET. Motivace za standardní knihovna navazuje větší jednotnost v ekosystému .NET.
[ECMA 335](https://github.com/dotnet/coreclr/blob/master/Documentation/project-docs/dotnet-standards.md) pokračuje k navázání jednotnost chování modulu runtime rozhraní .NET, ale neexistuje žádné podobné specifikace pro knihovny pro třídy Base rozhraní .NET (BCL) pro implementace knihovny .NET.

Můžete si ho představit jako zjednodušenou, nové generace [Přenosná knihovna tříd](https://msdn.microsoft.com/library/gg597391.aspx).
Je jediná knihovna s jednotné rozhraní API pro všechny platformy .NET, včetně .NET Core. Stačí vytvořit jednu standardní knihovna pro .NET a použít ze všech modul runtime, který podporuje Standard platformy .NET.

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

## <a name="xamarin-studio"></a>Xamarin Studio

.NET standard projektů knihovny lze vytvořit v Xamarin Studio 6.2, první vytvořením projektu přenosné knihovny:

[ ![](net-standard-images/xs01-sml.png "Vytvoření nového projektu přenosné knihovny")](net-standard-images/xs01.png)

Po vytvoření projektu klikněte pravým tlačítkem a otevřete **možnosti projektu** okno.
V **Obecné** části projektu můžete převést na rozhraní .NET standardní a nastavení používá ve na konkrétní verzi **platformy** rozevíracího seznamu:

[ ![](net-standard-images/xs02-sml.png "Převést na rozhraní .NET standardní Obecné možnosti")](net-standard-images/xs02.png)

Pak můžete [vytvořit balíček NuGet](~/cross-platform/app-fundamentals/nuget-multiplatform-libraries/existing-library.md) sdílení knihovny s jinými vývojáři.

## <a name="visual-studio-for-mac-walkthrough"></a>Visual Studio pro Mac návod

Tato část vás provede postup vytvoření a použití standardní knihovny .NET pomocí sady Visual Studio for Mac. Informace naleznete v sekci Příklad standardní knihovny .NET pro dokončení implementace.

### <a name="creating-a-net-standard-library"></a>Vytvoření standardní knihovna pro .NET

Přidání standardní knihovna pro .NET pro vaše řešení je docela rovnou dál.

1. V dialogovém okně Přidat nový projekt, vyberte `.NET Core` kategorie a potom vyberte `Class Library(.NET Core)`.

  **Poznámka:** této šablony, bude přejmenován na `.NET Standard` v budoucí verzi sady Visual Studio for Mac.

  ![Vytvoření knihovny tříd .NET Core](net-standard-images/vsm01.png)

2. Standardní knihovny .NET projektu se zobrazí, jak je znázorněno v Průzkumníku řešení. Uzel závislosti označí, že používá knihovna [NETStandard.Library](https://www.nuget.org/packages/NETStandard.Library/).

  ![Uzel závislosti v rámci řešení určuje .NET Standard](net-standard-images/vsm02.png)

#### <a name="editing-net-standard-library-settings"></a>Úprava nastavení standardní knihovny .NET

Nastavení standardní knihovny .NET můžete zobrazit a změnit tak, že kliknete pravým tlačítkem na projekt a výběrem `Options` jak je vidět na tomto snímku obrazovky:

![Upravit standardní rozhraní .NET framework cíl v možnosti projektu](net-standard-images/vsm03.png)

Můžete změnit ve vaší verzi `netstandard` změnou `Target Framework` hodnota rozevíracího seznamu.

**A dále:** můžete upravit `.csproj` přímo na tuto hodnotu změnit.

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

## <a name="visual-studio-windows-walkthrough"></a>Návod pro Visual Studio (Windows)

Tato část vás provede postup vytvoření a použití standardní knihovny .NET pomocí sady Visual Studio. Informace naleznete v sekci Příklad standardní knihovny .NET pro dokončení implementace.

### <a name="creating-a-net-standard-library"></a>Vytvoření standardní knihovna pro .NET

#### <a name="visual-studio-2017"></a>Visual Studio 2017

Přidání standardní knihovna pro .NET pro vaše řešení je docela rovnou dál.

1. V dialogovém okně Přidat nový projekt, vyberte `.NET Standard` kategorie a potom vyberte `Class Library(.NET Standard)`.

  ![](net-standard-images/vs01.png "Vytvořit novou knihovnu .NET Standard – třída")

2. Standardní knihovny .NET projektu se zobrazí, jak je znázorněno v Průzkumníku řešení. Uzel závislosti označí, že používá knihovna [NETStandard.Library](https://www.nuget.org/packages/NETStandard.Library/).

  ![](net-standard-images/vs02.png ".NET standard projekt v řešení")

#### <a name="editing-net-standard-library-settings"></a>Úprava nastavení standardní knihovny .NET

Nastavení standardní knihovny .NET můžete zobrazit a změnit tak, že kliknete pravým tlačítkem na projekt a výběrem `Properties` jak je vidět na tomto snímku obrazovky:

![](net-standard-images/vs03.png "Referenční .NET Standard knihovnu stejným způsobem jako jiné projekty")

Můžete změnit ve vaší verzi `netstandard` změnou `Target Framework` hodnota rozevíracího seznamu.

**A dále:** můžete upravit `.csproj` přímo na tuto hodnotu změnit.

#### <a name="using-net-standard-library"></a>Pomocí standardní knihovny .NET

Po vytvoření standardní knihovny .NET, můžete přidat odkaz na jeho ze žádného kompatibilní aplikace nebo knihovna projektu stejným způsobem jako za normálních okolností přidáte odkazy. V sadě Visual Studio, klikněte pravým tlačítkem na uzel odkazy a zvolte `Add Reference...` potom přepnout `Solution : Projects` kartě, jak je znázorněno:

![](net-standard-images/vs04.png "V sadě Visual Studio klikněte pravým tlačítkem na uzel odkazy a zvolte možnost Přidat odkaz... potom přejděte na kartu řešení projekty, jak vidíte")

-----


## <a name="related-links"></a>Související odkazy

- [Zpráva k vydání verze](https://developer.xamarin.com/releases/studio/xamarin.studio_6.2/xamarin.studio_6.2/#.NET_Standard_Support)
