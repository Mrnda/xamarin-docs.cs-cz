---
title: "Úvod do fastlane pro iOS"
description: "Tento průvodce představuje různé fastlane nástroje, které slouží k kódové podepisování aplikací iOS"
ms.topic: article
ms.prod: xamarin
ms.assetid: 8202C57D-22FF-4224-A5B1-AAEF12B7C106
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/19/2017
ms.openlocfilehash: 4bba92180e77accaa42b70843fb5dbf12c94d632
ms.sourcegitcommit: 73bd0c7e5f237f0a1be70a6c1384309bb26609d5
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 03/22/2018
---
# <a name="introduction-to-fastlane-for-ios"></a>Úvod do fastlane pro iOS

_Tento průvodce představuje různé fastlane nástroje, které slouží k kódové podepisování aplikací iOS_

fastlane je opensourcový projekt, k vytvoření, která zjednodušují proces matoucí a často zdlouhavé vydání systémy iOS a Android. Obsahuje několik nástrojů, že každý zpracovat konkrétních aspektů verze aplikace, například:

- [poskytování](https://github.com/fastlane/fastlane/tree/master/deliver#readme) – spravuje a snímky obrazovky nahrávání, metadat a sady aplikací pro službu iTunes připojit.
- [vytvořit](https://github.com/fastlane/fastlane/tree/master/produce#readme) – vytvoří a aplikace v iTunes připojit a portál pro vývojáře (často označované jako ID aplikace). Zahrnuje taky podporu pro skupiny aplikací a aplikačních služeb.
- [pem](https://github.com/fastlane/fastlane/tree/master/pem#readme) – vytváří a spravuje profily zřizování nabízená oznámení.
- [sebou](https://github.com/fastlane/fastlane/tree/master/gym#readme) – to lze použít k vytvoření a podepsání aplikace pro iOS. (Aplikace Xamarin již pomocí nástroje MSBuild sestavení, přihlášení a archivu aplikace)
- [certifikát](https://github.com/fastlane/fastlane/tree/master/cert#readme) – vytváří a spravuje certifikáty pro podepisování kódu 
- [sigh](https://github.com/fastlane/fastlane/tree/master/sigh#readme) – vytváří a spravuje zřizovacích profilů
- [odpovídat](https://github.com/fastlane/fastlane/tree/master/match#readme) – vytvoří a udržuje profilů a certifikátů a ukládá je do úložiště git tak, aby mohou být synchronizovány mezi vývojovým týmem.

fastlane lze různými způsoby: pomocí příkazu terminálu, prostřednictvím souborové prostředky, nebo pomocí proměnných prostředí pro sestavení průběžnou integraci. 

Tento průvodce se nastavení zařízení pro vývoj pomocí aplikace pro iOS a se zaměřuje na **cert**, **sigh** a **odpovídat** nástrojů. 

Obsah můžete použít jako odrazový můstek na podporu s distribuce aplikací, včetně plně automatizace procesu na průběžnou integraci serveru. Je ale důležité si uvědomit, že fastlane je 3. stran, která provádět nástroje k podpoře projekty Xcode a proto některé nástroje nebo příkazy, jako `fastlane init` nemusí fungovat podle očekávání se csproj soubory. Další informace o použití fastlane další nástroje nebo vydání pro Android pomocí fastlane, najdete v části [https://fastlane.tools/](https://fastlane.tools/)

<a name="Installation" />

## <a name="installation"></a>Instalace

1. Ujistěte se, že jsou na počítači systému macOS nainstalovány nástroje příkazového řádku Xcode. Chcete-li nainstalovat nástroje, použijte příkaz `xcode-select --install` v terminálu. Pokud již jsou nainstalovány, zobrazí se chybová zpráva:

    ```bash
    error: command line tools are already installed, use "Software Update" to install updates
    ```

2. Stažení nástroje fastlane z [ https://download.fastlane.tools ](https://download.fastlane.tools). 

    > [!NOTE]
    > Je možné nainstalovat nástroje fastlane pomocí Homebrew `brew cask install fastlane` nebo prostřednictvím Rubygems (2.0 nebo novější) pomocí `sudo gem install fastlane –NV`. Ale pomocí Instalační služby zajistí, že jsou k dispozici správnými závislostmi. 

3. Nainstalovat fastlane tak, že bylo nutné je rozbalit soubor a dvakrát klikněte na `install` spustitelný soubor. Pokud dojde k chybě, který radí v souboru "nelze otevřít, protože je z neidentifikovaný vývojáře", klikněte na tlačítko OK a postupujte takto:
    - CTRL + klikněte na `install` spustitelný soubor. To se zobrazí dialogové okno níže:

      ![](images/fastlane-image12.png "Dialogové okno instalace")
    
    - Kliknutím na tlačítko OK zahájíte instalaci nástrojů pro fastlane

4. Terminálové zobrazí výzvu pomocí dialogu znázorněné dole. Stiskněte klávesu `y`:

  ![](images/fastlane-image13.png "Výzva terminálu")
 
4. Spustit `which fastlane` před použitím fastlane poprvé. Cesta by měl vypadat asi takto: 

    ```bash
    /Users/[user]/.fastlane/bin
    ```

5. Pokud cesta odpovídá výše, jste připravení začít.

     Pokud ne, postupujte takto: Otevřete v systému macOS `.bash_profile`, což je soubor ve formátu prostého textu skrytá v domovském adresáři, pomocí následujícího příkazu:

    ```bash
    open ~/.bash_profile
    ```

6. Přidejte následující proměnné prostředí PATH a uložte jej: 

    ```bash
    export PATH="$HOME/.fastlane/bin:$PATH"
    ```

7.  Spustit `which fastlane` znovu pro potvrzení cesta vypadá jako `/Users/[user]/.fastlane/bin`


## <a name="updating-fastlane"></a>Aktualizace fastlane

fastlane je velmi aktivní opensourcový projekt, který pravidelně nabízených oznámení nové verze. Když je k dispozici nová verze fastlane, budete informováni, když spustíte příkaz žádné fastlane:

[![](images/fastlane-image0.png "Aktualizace řádku dráhy fast")](images/fastlane-image0.png#lightbox)


Pokud chcete aktualizovat na novou verzi fastlane, stáhněte si nejnovější balíček z [zde](https://download.fastlane.tools) a dvakrát klikněte na instalovat balíček, který chcete spustit:

[![](images/fastlane-image0a.png "Spuštění instalace balíčku")](images/fastlane-image0a.png#lightbox)


## <a name="contents"></a>Obsah

Tato série příručky uvádí některé z nástrojů, aby fastlane používá pro podepisování kódu vaší aplikace pro iOS v rámci přípravy pro vývoj nebo distribuci. Nástroje pro aktuálně zahrnuté jsou:

- [cert](~/ios/deploy-test/provisioning/fastlane/cert.md)
- [sigh](~/ios/deploy-test/provisioning/fastlane/sigh.md)
- [match](~/ios/deploy-test/provisioning/fastlane/match.md)

certifikát a sigh řešit vytváření a správě podpisových certifikátů a profilů zřizování v místním počítači. Shoda trvá tento proces další krok. Vytváří a spravuje certifikáty a zřizovacích profilů a ukládá je v úložišti git, což jim umožní být přístupný pro všechny členy vývojový tým. Každá část a zjistěte, jak fungují a jak je můžete si přečíst.

## <a name="using-fastlane-tools-with-xamarin"></a>Pomocí nástrojů fastlane xamarinu

Jakmile vytvoříte podepisování identity a profily s fastlane zřizování, nastavení sady podepisování možnosti v sadě Visual Studio pro Mac by měla být jasné, zajištění, že jsou v systému macOS řetězce klíčů a certifikátů a privátních klíčů profily zřizování jsou ve složce `~/Library/MobileDevice/Provisioning Profiles`.

Pokud chcete nastavit možnosti pro aplikace pro Xamarin.iOS pro podpis kódu, klikněte pravým tlačítkem na název projektu, vyberte **možnosti projektu > sestavení > iOS podepisování sady** a nastavit identitu podepisování a profil zřizování explicitně, jako ukázáno níže:

[![](images/fastlane-image11.png "Explicitně nastavit identitu podepisování a profil zřizování")](images/fastlane-image11.png#lightbox)

## <a name="related-links"></a>Související odkazy

- [fastlane dokumentace](https://fastlane.tools/)
- [fastlane dokumentace podepisování kódu](https://docs.fastlane.tools/codesigning/getting-started/)
- [Příručka pro podepisování kódu](https://codesigning.guide/)
