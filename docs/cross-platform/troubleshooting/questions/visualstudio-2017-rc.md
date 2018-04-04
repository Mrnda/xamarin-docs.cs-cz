---
title: Můžete použít Visual Studio 2017 Release Candidate s Xamarinem?
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 8E752F36-F73A-4EFC-9F82-4E18FDE1C9E2
ms.technology: xamarin-cross-platform
author: asb3993
ms.author: amburns
ms.date: 03/29/2017
ms.openlocfilehash: 18329d226df5d129bd648c82b01e1ea045d131f4
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/04/2018
---
# <a name="can-i-use-visual-studio-2017-release-candidate-with-xamarin"></a>Můžete použít Visual Studio 2017 Release Candidate s Xamarinem?

## <a name="can-i-use-visual-studio-2017-release-candidate-with-xamarin"></a>Můžete použít Visual Studio 2017 Release Candidate s Xamarinem?

Ano. Budete moci využít Xamarin prostřednictvím Visual Studio 2017 Release Candidate. Ale pamatujte, že aktuálně, instalace Xamarin pro Visual Studio 2017 RC bude odinstalujte všechny starší verze Xamarin nainstalován do Visual Studio 2015 nebo 2013. Xamarin pro Visual Studio 2017 nelze nainstalovat ve stejnou dobu jako Xamarin pro Visual Studio 2015 nebo 2013 v důsledku Visual Studio 2017 přesun mimo MSI balení směrem k využití systému Instalační program Visual Studio.

Při týmem je aktuálně vyhledávání do způsoby, jak Nepoužívat toto očekávané chování, doporučujeme uživatelům zvolit svoje vývojové prostředí na základě svých potřeb. 

> [!IMPORTANT]
> Pokud vytváříte projekty Xamarin.iOS, ujistěte se, že Visual Studio pro Mac spárované systému Mac na stejné verzi Xamarin.iOS kanálu.

## <a name="how-do-i-install-xamarin-to-visual-studio-2017-release-candidate"></a>Jak nainstalovat Xamarin pro Visual Studio 2017 Release Candidate?

### <a name="installing-during-the-visual-studio-2017-rc-installer"></a>Instalace během instalační program Visual Studio 2017 RC

* Vyberte **Xamarin** součásti jako součást nové **instalační program Visual Studio**

  [![](visualstudio-2017-rc-images/install1-sml.png "Visual Studio 2017 RC instalační obrazovky")](visualstudio-2017-rc-images/install1-orig.png#lightbox)

Dojde k instalaci rozšíření sady Visual Studio pro vývoj Xamarin.iOS a Xamarin.Android.

### <a name="installing-or-reinstalling-xamarin-in-an-existing-installation-of-visual-studio-2017-rc"></a>Instalaci nebo přeinstalování Xamarin v existující instalaci sady Visual Studio 2017 RC

#### <a name="using-the-visual-studio-installer"></a>Pomocí instalačního programu sady Visual Studio:

1. Hledat aplikace Instalační program Visual Studio

  [![](visualstudio-2017-rc-images/reinstall1-sml.png "Výsledky hledání pro aplikaci instalačního programu sady Visual Studio")](visualstudio-2017-rc-images/reinstall1-orig.png#lightbox)

2. Vyberte:. **Mobilní vývoj pomocí rozhraní .NET (Preview)** na kartě úlohy nebo

  [![](visualstudio-2017-rc-images/reinstall2-sml.png "Karta úlohy instalační program VS") ](visualstudio-2017-rc-images/reinstall2-orig.png#lightbox) b. **Xamarin** v **jednotlivých součástí** karta

  [![](visualstudio-2017-rc-images/reinstall3-sml.png "Karta součásti Instalační program VS")](visualstudio-2017-rc-images/reinstall3-orig.png#lightbox)

#### <a name="using-the-visual-studio-installer-within-visual-studio"></a>Pomocí instalačního programu sady Visual Studio v sadě Visual Studio:
1. Přejděte na úvodní stránce 2017 Visual Studio
2. Klikněte na **více šablony projektu** pod **nový projekt** části

    [![](visualstudio-2017-rc-images/reinstall4-sml.png "Visual Studio úvodní stránky")](visualstudio-2017-rc-images/reinstall4-orig.png#lightbox)
3. Klikněte na `Open Visual Studio Installer` v levém podokně

    [![](visualstudio-2017-rc-images/reinstall5-sml.png "Nové obrazovky projektu")](visualstudio-2017-rc-images/reinstall5-orig.png#lightbox)
4. Vyberte:
    
    a. **Mobilní vývoj pomocí rozhraní .NET (Preview)** na kartě úlohy nebo

    [![](visualstudio-2017-rc-images/reinstall2-sml.png "Karta úlohy instalační program VS") ](visualstudio-2017-rc-images/reinstall2-orig.png#lightbox) b. **Xamarin** v **jednotlivých součástí** karta

    [![](visualstudio-2017-rc-images/reinstall3-sml.png "Karta součásti Instalační program VS")](visualstudio-2017-rc-images/reinstall3-orig.png#lightbox)
