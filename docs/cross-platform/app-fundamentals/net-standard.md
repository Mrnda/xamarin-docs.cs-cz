---
title: Sdílení kódu pomocí knihovny .NET Standard
description: Tento dokument popisuje způsob použití knihovny .NET Standard ke sdílení kódu. Popisuje vytvoření knihovny .NET Standard, úpravy nastavení a použití v aplikaci.
ms.prod: xamarin
ms.assetid: 8C30F8D3-1920-453E-9E8B-D40696736FF2
author: conceptdev
ms.author: crdun
ms.date: 07/18/2018
ms.openlocfilehash: 65ba1915a2a968a14f0ce21bcada76e1b83531b0
ms.sourcegitcommit: 46bb04016d3c35d91ff434b38474e0cb8197961b
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/26/2018
ms.locfileid: "39270633"
---
# <a name="net-standard-library-code-sharing"></a>Knihovna .NET standard sdílení kódu

Knihovny .NET standard mají jednotným rozhraním API pro všechny platformy .NET, včetně Xamarinu a .NET Core. Vytvoření jednoho knihovna .NET Standard a použít ji z jakékoli modul runtime podporující Standard platformy .NET. Odkazovat na [tento graf](https://docs.microsoft.com/dotnet/standard/net-standard#net-implementation-support) podrobnosti o podporovaných platformách.

.NET Standard verze 1.0 prostřednictvím 1.6 nabízejí postupně větší podsady rozhraní .NET Framework, .NET Standard 2.0 poskytuje nejlepší úroveň podpory pro aplikace Xamarin a pro přenesení stávajících přenosné knihovny tříd.

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

## <a name="visual-studio-for-mac"></a>Visual Studio for Mac

Tato část vás provede postupy vytvoření a použití standardní knihovnu .NET pomocí sady Visual Studio pro Mac.

### <a name="creating-a-net-standard-library"></a>Vytváří se knihovna .NET Standard

Přidání knihovny .NET Standard do řešení je poměrně jasně.

1. V **přidat nový projekt** dialogového okna, vyberte **.NET Core** kategorie a pak vyberte **knihovna .NET Standard**:

    ![Vytvoření knihovny .NET Standard](net-standard-images/vsm01-m157.png "vytvoření nové .NET Standard knihovny")

2. Na další obrazovce vyberte cílovou architekturu - **.NET Standard 2.0** se doporučuje:

    [![Zvolte .NET Standard 2.0](net-standard-images/vsm01a-m157-sml.png)](net-standard-images/vsm01a-m157.png#lightbox)

3. Na poslední obrazovce zadejte název projektu a klikněte na tlačítko **vytvořit**.

4. Knihovna .NET Standard projektu se zobrazí, jak je znázorněno v Průzkumníku řešení. Uzel závislosti indikuje, že používá knihovnu [NETStandard.Library](https://www.nuget.org/packages/NETStandard.Library/).

    ![Uzel závislosti v rámci řešení označuje .NET Standard](net-standard-images/vsm02-m157.png)

#### <a name="editing-net-standard-library-settings"></a>Úprava nastavení knihovna .NET Standard

Knihovna .NET Standard nastavení můžete zobrazit a změnit tak, že kliknete pravým tlačítkem na projekt a vyberete `Options` jak je znázorněno na tomto snímku obrazovky:

![Upravit v možnostech projektu .NET Standard Cílová architektura](net-standard-images/vsm03-m157.png "upravit verzi rozhraní .NET Framework standardní cíl v možnostech projektu")

Uvnitř můžete změnit verzi `netstandard` tak, že změníte `Target Framework` hodnotu rozevíracího seznamu.

**Kromě:** můžete upravit `.csproj` přímo na tuto hodnotu změnit.

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

## <a name="visual-studio-2017-windows"></a>Visual Studio 2017 (Windows)

Tato část vás provede postupem vytvoření a použití standardní knihovnu .NET pomocí sady Visual Studio.

### <a name="creating-a-net-standard-library"></a>Vytvoření knihovny .NET Standard

Přidání knihovny .NET Standard do řešení je poměrně jasně.

1. V **nový projekt** dialogového okna, vyberte **.NET Standard** kategorie a pak vyberte **knihovna tříd (.NET Standard)**.

    ![Vytvoření nové knihovny tříd standardní .NET](net-standard-images/vs01-w157.png "vytvořit novou .NET Standard knihovnu tříd")

2. Knihovna .NET Standard projektu se zobrazí, jak je znázorněno v Průzkumníku řešení. Uzel závislosti indikuje, že používá knihovnu [NETStandard.Library](https://www.nuget.org/packages/NETStandard.Library/).

    ![NETStandard.Library ve složce projektu](net-standard-images/vs02-w157.png "projekt .NET Standard v řešení")

### <a name="editing-net-standard-library-settings"></a>Úprava nastavení knihovny .NET Standard

Knihovna .NET Standard nastavení můžete zobrazit a změnit tak, že kliknete pravým tlačítkem na projekt a vyberete **vlastnosti** jak je znázorněno na tomto snímku obrazovky:

![Upravit .NET standard cílových platforem ve vlastnostech projektu](net-standard-images/vs03-w157.png "odkazovat stejným způsobem jako ostatní projekty knihovny .NET Standard")

**Kromě:** můžete upravit `.csproj` přímo k úpravě `TargetFramework` element a změnit, která verze je cílem (např.) `<TargetFramework>netstandard2.0</TargetFramework>`).

### <a name="using-a-net-standard-library-project"></a>Pomocí projektu knihovny .NET Standard

Po vytvoření knihovny .NET Standard, můžete přidat na ni odkaz z projektu žádné kompatibilní aplikace nebo knihovna stejně, jako je obvykle přidat odkazy. V sadě Visual Studio, klikněte pravým tlačítkem na uzel odkazy a zvolte **přidat odkaz...**  poté přejděte **projektů > řešení** kartu, jak je znázorněno:

![Odkazování na knihovny .NET Standard](net-standard-images/vs04.png "v sadě Visual Studio, klikněte pravým tlačítkem na uzel odkazy a zvolte Přidat odkaz... potom přepněte na kartu řešení projektů, jak je znázorněno")

-----

## <a name="related-links"></a>Související odkazy

* [.NET standard](https://docs.microsoft.com/dotnet/standard/net-standard) – podrobné informace a porovnání s PCL.
