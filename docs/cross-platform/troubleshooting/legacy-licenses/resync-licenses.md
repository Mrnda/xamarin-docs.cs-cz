---
title: Jak můžu ručně synchronizovat Xamarin licence?
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: D0BD93E9-3A1F-4E5B-8EE8-36ADC33DCFE4
ms.technology: xamarin-cross-platform
author: asb3993
ms.author: amburns
ms.openlocfilehash: b06a1a7d525c91d7c3973b2b02d3d2835ce482f9
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/04/2018
---
# <a name="how-do-i-manually-resynchronize-xamarin-licenses"></a>Jak můžu ručně synchronizovat Xamarin licence?

> [!IMPORTANT]
> Tato příručka nevztahuje na většina uživatelů MSDN protože nemusí vlastníte nebo přihlaste Xamarin účty, pokud pomocí [Xamarin součásti ukládání](https://components.xamarin.com/) nebo [Visual Studio pro Mac (Mac)](~/cross-platform/get-started/requirements.md).




## <a name="overview"></a>Přehled

Obvykle licenční informace bude třeba znovu synchronizovat s Xamarin licenční server automaticky při spuštění prostředí IDE. Například aktualizace licencí, dírám a obnovení, zobrazí normálně automaticky po restartování rozhraní IDE. Licenční informace zobrazené v prostředí IDE by měl odpovídat informace zobrazené na vaše [stránku účtu](https://store.xamarin.com/account/my/subscription/computers). Pokud informace jsou stále neodpovídající, můžete nejdřív zkontrolujte, že vypadá správné informace o účtu úložiště a ručně synchronizovat licence odhlašuje, odstranit licenční soubory na disku a pak protokolování zpět v.

### <a name="note-for-ios-developers-on-windows"></a>Poznámka: pro vývojáře iOS v systému Windows

Vývojáři pro iOS v systému Windows také potřebovat k provedení těchto kroků pro sadu Visual Studio pro Mac; i když nemáte vyvíjíte přímo na hostiteli spárované Mac sestavení.

## <a name="quick-manual-refresh-steps"></a>Ruční aktualizace rychlé kroky

Tento rychlý proces přeskočí krok odstranění licenčních souborů na disku, který může být v některých případech dostatečné. 

1.  Odhlaste se z účtu Xamarin prostřednictvím integrovaného vývojového prostředí:
    -   Visual Studio for Mac
        -   Pravém horním rohu na úvodní obrazovce
        -   **Visual Studio pro Mac > účet** (Mac)
        -   **Nástroje > účet** (Windows)
    -   Visual Studio
        -   **Nástroje > účet Xamarin**
2.  Přihlaste se zpět k účtu Xamarin v prostředí IDE.

## <a name="extended-manual-license-refresh-steps"></a>Rozšířené kroky obnovení ruční licencí

1.  Odhlaste se z účtu Xamarin prostřednictvím rozhraní IDE. 
2.  Ukončete prostředí IDE.
3.  Zkontrolujte, že vypadá správné informace o účtu úložiště. Zejména kontrolovat duplicitní názvy počítačů v [stránka počítače](https://store.xamarin.com/account/my/subscription/computers).

4.  Pokud uvidíte dvojice duplicitní názvy počítačů, použijte **deaktivovat** rozevírací nabídky položka pro odstranění _i_ členy dvojice:
    
    ![Licence na -> Deaktivovat https://store.xamarin.com/account/my/subscription/computers ] (resync-licenses-images/deactivate.png "pomocí položky rozevírací nabídky deaktivovat odebrání obou členů, které odpovídá páru")

5.  Odstraňte všechny zbývající kopie souborů s licencí stále nachází na disku.
    -   Windows

        Odstraňte všechny následující složky:
        -   `%PROGRAMDATA%\Mono for Android`
        -   `%PROGRAMDATA%\MonoTouch`
        -   `%LOCALAPPDATA%\VirtualStore\ProgramData\Mono for Android`
        -   `%LOCALAPPDATA%\VirtualStore\ProgramData\MonoTouch`

        Ve výchozím nastavení instalace systému Windows:
        -   `%PROGRAMDATA%` se rozbalí a `C:\ProgramData`
        -   `%LOCALAPPDATA%` se rozbalí a `C:\Users\%USERNAME%\AppData\Local`

        `VirtualStore` Kopie licence jsou vytvářena automaticky nástrojem systému Windows v některých scénářích. Pokud existují VirtualStore kopie, budou číst _místo_ licencí v `%PROGRAMDATA%`.

    -   Mac

        Odstraňte všechny následující složky:

        -   `~/Library/MonoAndroid`
        -   `~/Library/MonoTouch`
        -   `~/Library/Xamarin.Mac`

        Dostanete tyto cesty v **vyhledávací** výběrem **přejděte > přejděte do složky** nabídce a vložením do dialogového okna. Vyhledávací automaticky nahrazuje ~ znak tilda složce uživatele.

6.  Otevřete Visual Studio pro Mac nebo Visual Studio a protokolu zpět do účtu Xamarin.

## <a name="supplementary-information-individual-license-file-locations"></a>Doplňující informace: umístění souborů jednotlivých licencí

### <a name="windows"></a>Windows

V systému Windows jsou jednotlivé licenční soubory uložené v následujících umístěních:

-   Xamarin.Android:  
     `%PROGRAMDATA%\Mono for Android\License\monoandroid.licx`
-   Xamarin.iOS:  
     `%PROGRAMDATA%\MonoTouch\License\monotouch.licx`

Licence pro *zkušební* odběry jsou jinak s názvem:

-   Xamarin.Android:  
     `%PROGRAMDATA%\Mono for Android\License\monoandroid.trial.licx`
-   Xamarin.iOS:  
     `%PROGRAMDATA%\MonoTouch\License\monotouch.trial.licx`

### <a name="mac"></a>Mac

V systému Mac jednotlivých licenčních souborů jsou uložené v následujících umístěních:

-   Xamarin.Android:  
     `~/Library/MonoAndroid/License`
-   Xamarin.iOS:  
     `~/Library/MonoTouch/License.v2`
-   Xamarin.Mac:  
     `~/Library/Xamarin.Mac/License`

Licence pro *zkušební* odběry jsou jinak s názvem:

-   Xamarin.Android:  
     `~/Library/MonoAndroid/License.trial`
-   Xamarin.iOS:  
     `~/Library/MonoTouch/License.trial`
-   Xamarin.Mac:  
     `~/Library/Xamarin.Mac/License.trial`

## <a name="additional-troubleshooting-steps"></a>Při řešení problému

-   Zkontrolujte, pokud máte jakékoli jiné znaky než ASCII ve své uživatelské jméno, v názvu počítače nebo v některém z plně rozšířené cesty souborů s licencí.

-   [Obraťte se na podporu vývojáře pro Xamarin](http://xamarin.com/support)
