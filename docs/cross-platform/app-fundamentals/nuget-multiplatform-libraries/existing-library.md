---
title: Vytváření NuGet z existující projekty knihovny
description: Tento dokument popisuje, jak vytvořit balíček NuGet z existujícího projektu knihovny, že kódu možné sdílet s jinými vývojáři.
ms.prod: xamarin
ms.assetid: EDAC3E5E-DB7D-40A9-AE28-45C52ADA854E
author: asb3993
ms.author: amburns
ms.date: 04/20/2017
ms.openlocfilehash: 7f407b22d1793d585ae40aeae8c2d9b7616784e6
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 06/05/2018
ms.locfileid: "34779980"
---
# <a name="creating-a-nuget-from-existing-library-projects"></a>Vytváření NuGet z existující projekty knihovny

Knihovny PCL existující nebo .NET Standard mohou být změněny na NuGets prostřednictvím **možnosti projektu** okno:

1. Klikněte pravým tlačítkem na projekt knihovny v **řešení Pad** a zvolte **možnosti**.

2. Přejděte na **balíček NuGet > Metadata** a zadejte všechny [požadované informace](~/cross-platform/app-fundamentals/nuget-multiplatform-libraries/metadata.md) v **Obecné** karty:

  [![](existing-library-images/existing-metadata-sml.png "Zadejte požadovaná metadata")](existing-library-images/existing-metadata.png#lightbox)

3. Volitelně můžete [přidat další metadata](~/cross-platform/app-fundamentals/nuget-multiplatform-libraries/metadata.md) v **podrobnosti** kartě.

4. Po nakonfigurování metadata můžete kliknout pravým tlačítkem na projekt a zvolte **vytvořit balíček NuGet** a **.nupkg** soubor balíčku NuGet se uloží do **/bin/** složka (ladění nebo verze, v závislosti na konfiguraci).

  ![](existing-library-images/create-nuget-package.png "Zvolte vytvořit balíček NuGet z místní nabídky")

5. Chcete-li vytvořit balíček NuGet na _každých_ sestavení nebo nasazení, přejděte na **balíček NuGet > sestavení** části a značek **vytvořit balíček NuGet při sestavování projektu**:

    [![](existing-library-images/existing-tickbox-sml.png "Chcete-li vytvořit balíček NuGet značek")](existing-library-images/existing-tickbox.png#lightbox)

> [!NOTE]
> Vytváření NuGet balíček mohou být zpomaleny procesu sestavení. Pokud toto políčko není zaškrtnuté, můžete stále vygenerovat balíčku NuGet ručně kdykoli z místní nabídky projektu (zobrazené v kroku 4 výše).

## <a name="verifying-the-output"></a>Ověření výstup

Balíčky NuGet jsou také soubory ZIP, takže je možné zkontrolovat interní vygenerovaný balíček.

Tento snímek obrazovky ukazuje obsah na základě PCL NuGet – pouze jednoho sestavení PCL je součástí:

![](existing-library-images/nuget-output.png "Soubory obsažené v balíčku NuGet")


## <a name="related-links"></a>Související odkazy

- [Průvodce metadaty](~/cross-platform/app-fundamentals/nuget-multiplatform-libraries/metadata.md)
