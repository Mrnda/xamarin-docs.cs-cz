---
title: Úprava metadat NuGet
description: Tento dokument popisuje, jak upravit metadata NuGet pro s více platformami knihovny pomocí možnosti projektu. Popisuje požadované a volitelné metadat.
ms.prod: xamarin
ms.assetid: 147BA370-67A7-4E6C-BF17-AA7C536C0A48
author: asb3993
ms.author: amburns
ms.date: 03/23/2017
ms.openlocfilehash: 3680b02003a844668b0b5c97e5d4c0d296ae3500
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 06/05/2018
ms.locfileid: "34779967"
---
# <a name="editing-nuget-metadata"></a>Úprava metadat NuGet

_Použijte možnosti projektu a upravit metadata NuGet pro s více platformami knihovny_

Typy projektu knihovny (například PCL nebo .NET Standard nebo nový typ projektu NuGet) mají **balíček NuGet** tématu **možnosti projektu** okno.

**Metadata** nakonfiguruje hodnoty používané v části [ **příponou .nuspec** souboru manifestu balíčku NuGet](https://docs.microsoft.com/nuget/create-packages/creating-a-package#the-role-and-structure-of-the-nuspec-file).

## <a name="required-information"></a>Požadované informace

**Obecné** karta obsahuje čtyři pole, které je třeba zadat se generovat balíček NuGet:

[![](metadata-images/metadata-general-sml.png "Okno požadovaná metadata balíčků NuGet")](metadata-images/metadata-general.png#lightbox)

- **ID** – identifikátor balíčku, který by měl být jedinečný v rámci Nuget.org (nebo kdekoli budou distribuována balíčku). Potom zadejte [pokyny](https://docs.microsoft.com/nuget/create-packages/creating-a-package#choosing-a-unique-package-identifier-and-setting-the-version-number) a používat pouze znaky, které jsou platné v adrese URL (bez mezer a vyhnout se žádné speciální znaky).
- **Verze** – vyberte číslo verze, která je konzistentní s [pravidla verzí NuGet](https://docs.microsoft.com/nuget/create-packages/dependency-versions).
- **Autoři** – čárkami oddělený seznam názvů.
- **Popis** – přehled funkcí balíčku, který se zobrazí, když uživatelé mají vybrat balíček.

> [!NOTE]
> Nezapomeňte při vytváření nové verze pro distribuci ke NuGet nebo jiných uživatelů, zvýšíte číslo verze.

Další informace najdete v tématu [požadované referenční dokumentace elementů](https://docs.microsoft.com/nuget/schema/nuspec#required-metadata-elements) Další informace najdete také jako tyto podrobné pokyny v [výběr balíčku jedinečný identifikátor a nastavení číslo verze](https://docs.microsoft.com/nuget/create-packages/creating-a-package#choosing-a-unique-package-identifier-and-setting-the-version-number) a [ Typ balíčku nastavení](https://docs.microsoft.com/nuget/create-packages/creating-a-package#setting-a-package-type).

> [!IMPORTANT]
> Všechna pole na této kartě je třeba zadat; jinak, zobrazí se chybová zpráva: _"projektu nemá NuGet metadata proto nevytvoří balíčku NuGet. V části Metadata v možnosti projektu lze zadat metadata balíčků NuGet"_

## <a name="optional-metadata"></a>Volitelná Metadata

**Podrobnosti** karta obsahuje volitelná pole, které mají být zahrnuty v souboru manifestu balíčku NuGet.

[![](metadata-images/metadata-detail-sml.png "Okno volitelná metadata balíčků NuGet")](metadata-images/metadata-detail.png#lightbox)

Odkazovat [s volitelnými elementy](https://docs.microsoft.com/nuget/schema/nuspec#optional-metadata-elements) Další informace o povinná a nepovinná pole.

> [!NOTE]
> Pokud se na distribuci balíčku NuGet [NuGet.org](https://www.nuget.org) se doporučuje zadejte co nejvíce informací.


## <a name="related-links"></a>Související odkazy

- [referenční dokumentace příponou .nuspec](https://docs.microsoft.com/nuget/schema/nuspec#general-form-and-schema)
