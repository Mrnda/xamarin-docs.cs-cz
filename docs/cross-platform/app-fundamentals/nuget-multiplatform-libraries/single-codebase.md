---
title: "Vytvoření nové knihovny s více platformami pro NuGet"
ms.topic: article
ms.prod: xamarin
ms.assetid: E7B55354-9BBE-4122-BCE3-3506B79090DD
ms.technology: xamarin-cross-platform
author: asb3993
ms.author: amburns
ms.date: 03/23/2017
ms.openlocfilehash: e95cf18c281732c85c2029e4ff35e8dd8be0f5e2
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 02/27/2018
---
# <a name="creating-a-new-multiplatform-library-for-nuget"></a>Vytvoření nové knihovny s více platformami pro NuGet

Vytvoření projektu knihovny Multiplatform, který používá PCL nebo .NET Standard znamená, že výsledné NuGet mohou být přidány do všech rozhraní .NET projektu, který podporuje je profil cílového, včetně projekty ASP.NET nebo desktopových aplikacích pomocí WinForms, WPF nebo UWP.

Knihovny může obsahovat pouze kód nepodporuje vybraný profil PCL nebo .NET Standard, stejně jako ostatní NuGets, které jsou přidány.
To je vhodné obchodní logiky a algoritmy, které může být vyjádřený zcela v knihovně základní třídy rozhraní .NET.

Jediné sestavení se vytvoří a součástí balíčku NuGet.

Pokud budete později potřebovat funkce specifické pro platformu, [projekty specifické pro platformu lze přidat](#add-platforms).

## <a name="steps-to-create-a-multiplatform-library-nuget"></a>Postup vytvoření s více platformami knihovny NuGet

1. Vyberte **soubor > Nový řešení** (nebo klikněte pravým tlačítkem do stávajícího řešení a zvolte **Přidat > Nový projekt**).

2. Zvolte **Multiplatform knihovny** z **Multiplatform > Knihovna** části:

  [ ![](single-codebase-images/mulitplatform-library-sml.png "Konfigurace více platformami knihovny pro jeden základní kód")](single-codebase-images/mulitplatform-library.png)

3. Zadejte **název** a **popis**a zvolte **jeden pro všechny platformy**:

  [ ![](single-codebase-images/single-configure-sml.png "Konfigurace více platformami knihovny pro jeden základní kód")](single-codebase-images/single-configure.png)

4. Dokončete průvodce. Jediná knihovna projektu se vytvoří v řešení.

5. Klikněte pravým tlačítkem na nový projekt knihovny a potom vyberte **možnosti**. **Sestavení > Obecné** část umožňuje **cílové rozhraní** nastavit – zvolte profil .NET přenosné PCL nebo .NET Standard verze:

  [ ![](single-codebase-images/single-choose-type-sml.png "Zvolte pro typ knihovny PCL nebo .NET Standard")](single-codebase-images/single-choose-type.png)

6. Také v **možnosti projektu** okno, otevřete **balíček NuGet > Metadata** a zadejte [požadovaná metadata](~/cross-platform/app-fundamentals/nuget-multiplatform-libraries/metadata.md) (a také veškerá metadata, volitelný):

  [ ![](single-codebase-images/single-metadata-sml.png "Zadejte požadovaná metadata")](single-codebase-images/single-metadata.png)

7. Klikněte pravým tlačítkem na projekt knihovny a zvolte **vytvořit balíček NuGet** (nebo sestavení nebo nasazení řešení) a **.nupkg** soubor balíčku NuGet se uloží do **/bin/** složka (ladění nebo verze, v závislosti na konfiguraci):

  ![](single-codebase-images/create-nuget-package.png "Soubor balíčku NuGet se uloží do složky bin ladění nebo verze, v závislosti na konfiguraci")


## <a name="verifying-the-output"></a>Ověření výstup

Balíčky NuGet jsou také soubory ZIP, takže je možné zkontrolovat interní vygenerovaný balíček.

Tento snímek obrazovky ukazuje obsah na základě PCL NuGet – pouze jednoho sestavení PCL je součástí:

![](single-codebase-images/nuget-output.png "Soubory obsažené v balíčku NuGet")

<a name="add-platforms" />

# <a name="adding-platform-specific-code"></a>Přidání kódu pro konkrétní platformu

Na základě PCL projekty a .NET Standard na základě projekty nemůže obsahovat odkazy na specifické pro platformu (například funkce systému Android nebo iOS).

Pokud potřebujete rozšířit, aby obsahovaly kód specifický pro platformu existujícího projektu PCL nebo .NET Standard projektu, stačí pravým tlačítkem myši na projekt a výběrem **Přidat > přidejte platformu implementace...** :

[ ![](single-codebase-images/add-later-sml.png "Přidejte platformu implementace nabídky")](single-codebase-images/add-later.png)

Jeden nebo více projekty platformy mohou být přidány do řešení a existující knihovny PCL nebo .NET Standard volitelně možné převést na sdílený projektu:

[ ![](single-codebase-images/add-later-platforms-sml.png "Přidání možnosti platformy jako iOS, Android a sdílet projektu")](single-codebase-images/add-later-platforms-sml.png)

Po převodu do projektu sdílené, přejděte **možnosti projektu > balíček NuGet > referenční sestavení**
[části](~/cross-platform/app-fundamentals/nuget-multiplatform-libraries/platform-specific.md) a ujistěte se, že veškeré požadované profily jsou vybrané (tak, aby NuGet dál, aby byl kompatibilní s projekty, které byl dříve používán v).


## <a name="related-links"></a>Související odkazy

- [Průvodce metadat](~/cross-platform/app-fundamentals/nuget-multiplatform-libraries/metadata.md)
