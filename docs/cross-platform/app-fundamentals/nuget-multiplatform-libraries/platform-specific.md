---
title: Vytváření nových projektů knihovny specifické pro platformu pro NuGet
ms.prod: xamarin
ms.assetid: D8BC4906-805F-4AFB-8D1A-88B7BF87E17F
author: asb3993
ms.author: amburns
ms.date: 03/23/2017
ms.openlocfilehash: 901dbe032d62047668f265e8c7f79593b3fbfcce
ms.sourcegitcommit: 0a72c7dea020b965378b6314f558bf5360dbd066
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 05/10/2018
---
# <a name="creating-new-platform-specific-library-projects-for-nuget"></a>Vytváření nových projektů knihovny specifické pro platformu pro NuGet

S více platformami projekty knihovny, které používají konkrétní platformy, jako je iOS a Android, nejlépe pracovat sdílených projektů.

NuGet může obsahovat kódu pro iOS a Android konkrétní, jak kód .NET, které jsou společné pro objekty.

Více sestavení se vytváří a integrované do jednoho balíčku NuGet. Standardy NuGet Ujistěte se, že balíček mohou být přidány do všech typech podporovaných projektu, například Xamarin iOS a Android projekty.

## <a name="steps-to-create-a-cross-platform-library-nuget"></a>Postup vytvoření NuGet knihovny a platformy

1. Vyberte **soubor > Nový řešení** (nebo klikněte pravým tlačítkem do stávajícího řešení a zvolte **Přidat > Nový projekt**).

2. Zvolte **Multiplatform knihovny** z **Multiplatform > Knihovna** části:

  [![](platform-specific-images/mulitplatform-library-sml.png "Konfigurace více platformami knihovny pro jeden základní kód")](platform-specific-images/multiplatform-library.png#lightbox)

3. Zadejte **název** a **popis**a zvolte **konkrétní platformy**:

  [![](platform-specific-images/specific-configure-sml.png "Konfigurace knihovny specifické pro platformu pro iOS a Android")](platform-specific-images/specific-configure.png#lightbox)

4. Dokončete průvodce. Následující projekty jsou přidány k řešení:

  - **Projekt pro Android** – kód specifický pro Android můžete volitelně přidat do projektu.
  - **iOS projektu** – kódu pro iOS konkrétní Volitelně lze přidat do projektu.
  - **NuGet projektu** – žádný kód se přidá k tomuto projektu. Odkazuje na jiné projekty a obsahuje konfiguraci metadata pro výstup balíček NuGet.
  - **Sdílený projekt** – společný kód musí být přidaní do tohoto projektu, včetně kódu pro konkrétní platformu uvnitř `#if` direktivy kompilátoru.

5. Klikněte pravým tlačítkem na projekt NuGet a zvolte **možnosti**, pak otevřete **balíček NuGet > Metadata** a zadejte [požadovaná metadata](~/cross-platform/app-fundamentals/nuget-multiplatform-libraries/metadata.md) (jako jakýmikoliv volitelné metadata):

  [![](platform-specific-images/specific-metadata-sml.png "Zadejte požadovaná metadata")](platform-specific-images/specific-metadata.png#lightbox)

6. Také v **možnosti projektu** okno, otevřete **referenční sestavení** tématu a zvolte PCL profily, které bude podporovat sdílenou knihovnu prostřednictvím "návnada a přepínač":

  ![](platform-specific-images/specific-reference-assemblies.png "Také v okně Možnosti projektu otevřete část referenční sestavení a vyberte profily, které PCL sdílené knihovny bude podporovat prostřednictvím návnada a přepínače")

  > [!NOTE]
> "Přepínač a návnada" znamená sestavení PCL bude obsahovat pouze rozhraní API vystavené knihovny (nemůže obsahovat kód specifický pro platformu). Při přidání NuGet do projektu Xamarin, sdílené knihovny se zkompiluje proti PCL, ale sestavení specifické pro platformu obsahovat kód, který se používá ve skutečnosti iOS nebo Android projektu.

7. Klikněte pravým tlačítkem na projekt a zvolte **vytvořit balíček NuGet** (nebo sestavení nebo nasazení řešení) a **.nupkg** soubor balíčku NuGet se uloží do **/bin/** (složky buď Debug a Release, v závislosti na konfiguraci).

  ![](platform-specific-images/create-nuget-package.png "Soubor balíčku NuGet se uloží do složky bin ladění nebo verze, v závislosti na konfiguraci")


## <a name="verifying-the-output"></a>Ověření výstup

Balíčky NuGet jsou také soubory ZIP, takže je možné zkontrolovat interní vygenerovaný balíček.

Vybrané tento snímek obrazovky ukazuje obsah NuGet specifické pro platformu, která podporuje iOS a Android a měl dvě referenční sestavení:

![](platform-specific-images/nuget-output.png "Soubory obsažené v balíčku NuGet")


## <a name="related-links"></a>Související odkazy

- [Průvodce metadaty](~/cross-platform/app-fundamentals/nuget-multiplatform-libraries/metadata.md)
