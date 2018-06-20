---
title: Odinstalace Xamarin
description: Tento dokument popisuje postup odinstalace Xamarin na Mac a Windows. Poskytuje podrobné pokyny týkající se odinstalace Mono, Xamarin.Android, Xamarin.iOS a další nástroje.
ms.prod: xamarin
ms.assetid: b83a85ec-842a-444c-8f82-c2464eda099b
author: asb3993
ms.author: amburns
ms.date: 04/08/2017
ms.openlocfilehash: 444559672f25b13b7d3a769d6de4bd6384174965
ms.sourcegitcommit: d70fcc6380834127fdc58595aace55b7821f9098
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 06/19/2018
ms.locfileid: "36268885"
---
# <a name="uninstalling-xamarin"></a>Odinstalace Xamarin

Tato příručka vysvětluje, jak odebrat Xamarin ze systému macOS nebo ze sady Visual Studio v systému Windows.

Pokud je nutné znovu nainstalovat Xamarin pomocí Universal instalační program, vždy doporučujeme nejprve restartování počítače.

## <a name="uninstalling-xamarin-on-macos"></a>V systému macOS odinstalace Xamarin

Tato příručka slouží k odinstalaci každý produkt jednotlivě tak, že přejdete do části relevantní. Celý Xamarin nástrojů, která zahrnuje uvedené produkty, můžou se odinstalovat celou způsob prostřednictvím podle této příručky:

- [Mono](#uninstallmono)
- [Xamarin.Android](#uninstallandroid)
- [Xamarin.iOS](#uninstallios)
- [Xamarin.Mac](#uninstallmac)
- [Sešity](#uninstallworkbooks)
- [Xamarin Profiler](#uninstallprofiler)
- [Instalační program](#uninstallinstaller)

> [!TIP]
> Uvádíme [odinstalovat skriptu](https://raw.githubusercontent.com/MicrosoftDocs/visualstudio-docs/master/mac/resources/uninstall-vsmac.sh) musíte použít při odebírání Xamarin z počítače systému macOS. Další informace o používání skriptu najdete v tématu [pomocí skriptu odinstalovat](#uninstallscript) části této příručky.

### <a name="uninstalling-visual-studio-for-mac"></a>Odinstalace Visual Studio pro Mac

Prvním krokem při odinstalaci Xamarin z algoritmu Mac, je nalezení **Visual Studio.app** v **/Applications** adresáře a přetáhněte ji do **odpadkový koš**. Případně, klikněte pravým tlačítkem a vyberte **přesunout do Koš** jak je znázorněno na následujícím obrázku:

![Přesunutí aplikace Visual Studio koše](uninstalling-xamarin-images/uninstall-image1.png)

Visual Studio pro Mac, odstraněním této sady prostředků aplikace odebere, i když může být další soubory týkající se Xamarin stále v systému souborů.

K odebrání všech trasování sady Visual Studio pro Mac, spusťte následující příkazy v terminálu:

```bash
sudo rm -rf "/Applications/Visual Studio.app"
rm -rf ~/Library/Caches/VisualStudio
rm -rf ~/Library/Preferences/VisualStudio
rm -rf ~/Library/Preferences/Visual\ Studio
rm -rf ~/Library/Logs/VisualStudio
rm -rf ~/Library/VisualStudio
rm -rf ~/Library/Preferences/Xamarin/
rm -rf ~/Library/Developer/Xamarin
rm -rf ~/Library/Application\ Support/VisualStudio
rm -rf ~/Library/Application\ Support/VisualStudio/7.0/LocalInstall/Addins/
```

> [!NOTE]
> Informace o odinstalaci sady Visual Studio pro Mac, najdete [odinstalovat](https://docs.microsoft.com/visualstudio/mac/uninstall) Průvodce na webu docs.microsoft.com

<a name="uninstallmono" />

### <a name="uninstall-mono-sdk-mdk"></a>Odinstalujte Mono SDK (MDK)

Mono je open source implementace rozhraní .NET Framework a umožňuje Xamarin Products—Xamarin.iOS, Xamarin.Android a Xamarin.Mac vývoj těchto platforem v jazyce C#.

> [!WARNING]
> Existují další aplikace mimo Xamarin, které také používají Mono, jako je například Unity. Ujistěte se, že neexistují žádné další závislosti na Mono před odinstalací jej.

Chcete-li odebrat rozhraní Mono, spusťte následující příkazy v terminálu:

```bash
sudo rm -rf /Library/Frameworks/Mono.framework
sudo pkgutil --forget com.xamarin.mono-MDK.pkg
sudo rm /etc/paths.d/mono-commands
```

<a name="uninstallandroid" />

### <a name="uninstall-xamarinandroid"></a>Odinstalace Xamarin.Android

Existuje několik položek, které jsou požadovány při používání Xamarin.Android, jako je například Android SDK a sadu Java SDK, které musí být odebrány při odinstalaci Xamarin.Android. Tato část vás provede odinstalovat všechny potřebné součásti.

Chcete-li odebrat Xamarin.Android, spusťte následující příkazy v terminálu:

```bash
sudo rm -rf /Developer/MonoDroid
rm -rf ~/Library/MonoAndroid
sudo pkgutil --forget com.xamarin.android.pkg
sudo rm -rf /Library/Frameworks/Xamarin.Android.framework
```

#### <a name="uninstall-android-sdk-and-java-sdk"></a>Odinstalování SDK pro Android SDK a Java

Je vyžadována pro vývoj aplikací pro Android SDK pro Android. Pokud chcete úplně odebrat všechny části sady SDK pro Android, vyhledejte soubor v **~/Library/Developer/Xamarin/** a přesunout ji do **Koš**.

Java SDK (JDK) není nutné odinstalovat, protože je již před zabalené jako součást systému Mac OS X.

#### <a name="uninstall-android-avd"></a>Odinstalujte Android AVD

> [!WARNING]
> Existují další aplikace mimo Visual Studio pro Mac kteří také používají Android AVD a tyto další android součásti, jako je například Android Studio.
> Odebrání tento adresář může způsobit, že projekty pro přerušení v Android Studio. 

Chcete-li odebrat všechny Android AVDs a další součásti Android, použijte následující příkaz:

```bash
rm -rf ~/.android
```

Pokud chcete odebrat jenom Android AVDs, použijte následující příkaz:

```bash
rm -rf ~/.android/avd
```

<a name="uninstallios" />

### <a name="uninstall-xamarinios"></a>Odinstalace Xamarin.iOS

Xamarin.iOS umožňuje iOS vývoj aplikací pomocí jazyka C# nebo F #. Pokud chcete z počítače odinstalovat Xamarin.iOS, postupujte následujícím způsobem:

Odebere všechny soubory Xamarin.iOS, použijte následující příkazy v terminálu:

```bash
rm -rf ~/Library/MonoTouch
sudo rm -rf /Library/Frameworks/Xamarin.iOS.framework
sudo rm -rf /Developer/MonoTouch
sudo pkgutil --forget com.xamarin.monotouch.pkg
sudo pkgutil --forget com.xamarin.xamarin-ios-build-host.pkg
sudo pkgutil --forget com.xamarin.xamarin.ios.pkg
```

<a name="uninstallmac" />

### <a name="uninstall-xamarinmac"></a>Odinstalace Xamarin.Mac

Xamarin.Mac může být odstraněn z počítače pomocí následujících příkazů pro eradikaci produktu a licenci pro počítače Mac v uvedeném pořadí:

```bash
sudo rm -rf /Library/Frameworks/Xamarin.Mac.framework
rm -rf ~/Library/Xamarin.Mac
```

<a name="uninstallworkbooks" />

### <a name="uninstall-workbooks"></a>Odinstalujte sešity

Odebrání Xamarin sešity verze 1.2.2 a novějším, použijte následující příkazy v terminálu:

```bash
sudo /Library/Frameworks/Xamarin.Interactive.framework/Versions/Current/uninstall
```

Starší verze najdete v tématu [sešity](~/tools/workbooks/install.md#uninstall-macos) odinstalovat průvodce.

<a name="uninstallprofiler" />

### <a name="uninstall-the-xamarin-profiler"></a>Odinstalace Xamarin profileru

Odebrat profileru Xamarin, použijte následující příkazy v terminálu:

```bash
sudo rm -rf "/Applications/Xamarin Profiler.app"
```

<a name="uninstallinstaller" />

### <a name="uninstall-the-xamarin-installer"></a>Odinstalujte instalační program Xamarin

K odebrání všech trasování Xamarin Universal instalačního programu, použijte následující příkazy:

```bash
rm -rf ~/Library/Caches/XamarinInstaller/
rm -rf ~/Library/Caches/VisualStudioInstaller/
rm -rf ~/Library/Logs/XamarinInstaller/
rm -rf ~/Library/Logs/VisualStudioInstaller/
rm -rf ~/Library/Preferences/Xamarin/
rm -rf "~/Library/Preferences/Visual Studio/"
```

<a name="uninstallscript" />

### <a name="using-the-uninstall-script"></a>Odinstalační skript pro použití

Tento skript odinstalovat umožňuje odinstalujte Visual Studio pro Mac a jeho přidružených součásti Xamarin najednou.

Skript obsahuje většinu příkazů, které se nacházejí v následujícím článku. Existují dvě hlavní opomenutí ze skriptu a nejsou zahrnuty z důvodu možných externí závislosti:

- Odinstalace Mono
- Odinstalace Android AVD

Chcete-li spustit skript, proveďte následující kroky:

1. Klikněte pravým tlačítkem na skript a vyberte možnost Uložit jako... Uložte soubor na vašem Mac.

2.  Otevřete **Terminálové** a změnit pracovní adresář, do které jste stáhli skript:

        $ cd /location/of/file

3. Spustitelný soubor skriptu a spustit ji s **sudo**:

        $ chmod +x ./xamarin_uninstall.sh
        $ sudo ./xamarin_uninstall.sh

4. Nakonec odstraňte odinstalační skript.

V tomto okamžiku Xamarin má být odinstalován z počítače.

<a name="uninstallwindows" />

## <a name="uninstalling-xamarin-on-windows"></a>Odinstalace Xamarinem v systému Windows

Xamarin byl podporován na následující:

- [Visual Studio 2017](#uninstallvs2017)
- [Visual Studio 2015](#uninstallvs2015)
- [Visual Studio 2013](#uninstallvs2015) [**nepodporované**]
- [Xamarin Studio](#uninstallxamarinstudio) [**nepodporované**]

<a name="uninstallvs2017" />

### <a name="visual-studio-2017"></a>Visual Studio 2017

Xamarin se odinstaluje z použití instalačního programu aplikace Visual Studio 2017:

1. Použití **nabídky Start** otevřete **instalační program Visual Studio**.

2. Stiskněte **upravit** tlačítko pro instanci chcete změnit.

    [![](uninstalling-xamarin-images/vs2017-02-sml.png "Klikněte na tlačítko Upravit")](uninstalling-xamarin-images/vs2017-02.png#lightbox)

3. V **úlohy** kartě, zrušte výběr **vývoj mobilních řešení s .NET** možnost (v **mobilní a herní** části).

    [![](uninstalling-xamarin-images/vs2017-03-sml.png "Zrušte zaškrtnutí políčka zatížení vývoj mobilních řešení")](uninstalling-xamarin-images/vs2017-03.png#lightbox)

4. Klikněte **upravit** tlačítko v pravém dolním rohu okna.

5. Instalační program odebere zrušte vybrané součásti (Visual Studio 2017 musí být uzavřeny předtím, než instalační program provede všechny změny).

    [![](uninstalling-xamarin-images/vs2017-04-sml.png "Klikněte na tlačítko Upravit")](uninstalling-xamarin-images/vs2017-04.png#lightbox)

Odinstalovat jednotlivých součástí Xamarin (například profileru nebo sešity), při přechodu **jednotlivých součástí** v kroku 3 a zaškrtnutí políčka konkrétní součásti:

[![](uninstalling-xamarin-images/vs2017-components-sml.png "Odinstalace jednotlivých součástí")](uninstalling-xamarin-images/vs2017-components.png#lightbox)

Úplně odinstalujte Visual Studio 2017, zvolte **odinstalace** z panelu tři nabídce vedle položky **spusťte** tlačítko.

[![](uninstalling-xamarin-images/vs2017-uninstall-sml.png "Úplně odinstalujte Visual Studio")](uninstalling-xamarin-images/vs2017-uninstall.png#lightbox)

> [!IMPORTANT]
> Pokud máte dva (nebo více) instancí sady Visual Studio nainstalována souběžného (SxS) – například verze a verze Preview – odinstalaci jedna instance může odstranit některé funkce Xamarin z jiné sady Visual Studio instance, včetně:
>
> - Xamarin Profiler
> - Xamarin sešity nebo Inspector
> - Simulátoru vzdálené Xamarin iOS
> - Apple Bonjour SDK
>
> Za určitých podmínek může jedna z instancí SxS odinstalace způsobit nesprávné odebrání těchto funkcí. To může snížit výkon na platformě Xamarin v sadě Visual Studio instance, které zůstaly po odinstalaci SxS instance systému.
>
>Tato isresolved spuštěním **oprava** možnosti v instalačním programu sady Visual Studio bude znovu instalaci chybějících součástí.


## <a name="uninstalling-older-and-unsupported-products"></a>Odinstalace starší a nepodporovanou produktů

<a name="uninstallvs2015"></a>

### <a name="visual-studio-2015-and-earlier"></a>Visual Studio 2015 a starší

Chcete-li zcela odinstalovat sadu Visual Studio 2015, použijte [podporu odpověď na visualstudio.com](https://www.visualstudio.com/vs/support/vs2015/uninstall-visual-studio-2015/).

Xamarin, můžou se odinstalovat z počítače s Windows pomocí **ovládací panely**. Přejděte na **programy a funkce** nebo **programy > odinstalovat Program** jak je uvedeno dále:

 [![](uninstalling-xamarin-images/image3.png "Přejděte na programy a funkce nebo programy odinstalovat Program, jak je znázorněno zde")](uninstalling-xamarin-images/image3.png#lightbox) 

V Ovládacích panelech odinstalujte některé z následujících umístění, které jsou k dispozici:

- Xamarin
- Xamarin pro Windows
- Xamarin.Android
- Xamarin.iOS
- Xamarin pro Visual Studio

V Průzkumníku odstraňte všechny zbývající soubory ze složky rozšíření Xamarin Visual Studio (všechny verze, včetně programové soubory a soubory programu (x86)):

``` 
C:\Program Files*\Microsoft Visual Studio 1*.0\Common7\IDE\Extensions\Xamarin
```

Odstranění sady Visual Studio MEF součást mezipaměti adresáře, který by měla být umístěná v následujícím umístění:

``` 
%LOCALAPPDATA%\Microsoft\VisualStudio\1*.0\ComponentModelCache
```

Zkontrolujte v **VirtualStore** překrytí adresáři, pokud Windows může mít uložené všechny soubory **Extensions\Xamarin** nebo **ComponentModelCache** adresáře existuje:

``` 
%LOCALAPPDATA%\VirtualStore
```

Otevřete editor registru (regedit) a vyhledejte následující klíč:

``` 
HKEY_LOCAL_MACHINE\SOFTWARE\Wow6432Node\Microsoft\Windows\CurrentVersion\SharedDlls
```

Najít a odstranit všechny položky, které odpovídají tento vzor:

``` 
C:\Program Files*\Microsoft Visual Studio 1*.0\Common7\IDE\Extensions\Xamarin
```

Podívejte se na tento klíč:

``` 
HKEY_CURRENT_USER\Software\Microsoft\VisualStudio\1*.0\ExtensionManager\PendingDeletions
```

Odstraňte všechny položky, které vypadají stejně, jako mohou být s Xamarin. Například nic obsahující podmínky `mono` nebo `xamarin`.

Otevřete příkazový řádek správce cmd.exe a potom spusťte `devenv /setup` a `devenv /updateconfiguration` příkazy pro každou nainstalovanou verzi sady Visual Studio. Například pro Visual Studio 2015:

```cmd
"%ProgramFiles(x86)%\Microsoft Visual Studio 14.0\Common7\IDE\devenv.exe" /setup
"%ProgramFiles(x86)%\Microsoft Visual Studio 14.0\Common7\IDE\devenv.exe" /updateconfiguration
```

<a name="uninstallxamarinstudio"></a>

### <a name="uninstall-xamarin-studio-on-windows"></a>Odinstalace Xamarin Studio v systému Windows

Xamarin Studio je odinstalován z počítače s Windows pomocí **ovládací panely**. Přejděte na **programy a funkce** nebo **programy > odinstalovat Program** 

Pokud chcete odinstalovat Xamarin Studio, Najít **ve tvaru 5.x.x Xamarin Studio** v seznamu programy a klikněte na tlačítko **odinstalovat** tlačítko. 

### <a name="uninstall-xamarin-studio-on-mac"></a>Odinstalace Xamarin Studio pro Mac

Prvním krokem při odinstalaci Xamarin Studio z algoritmu Mac, je nalezení **Xamarin Studio.app** v **/Applications** adresáře a přetáhněte ji do **odpadkový koš**. Případně, klikněte pravým tlačítkem a vyberte **přesunout do Koš** jak je uvedeno dále:

 [![](uninstalling-xamarin-images/image1.png "Případně klikněte pravým tlačítkem a vyberte možnost přesunout na Koš, jak je znázorněno zde")](uninstalling-xamarin-images/image1.png#lightbox)

Odstranění této sady prostředků aplikace Xamarin Studio odstraní, ale existují další soubory týkající se Xamarin stále v systému souborů.

K odebrání všech trasování Xamarin Studio, by měl v terminálu spustit následující příkazy:

```bash
sudo rm -rf "/Applications/Xamarin Studio.app"
rm -rf ~/Library/Caches/XamarinStudio-*
rm -rf ~/Library/Preferences/XamarinStudio-*
rm -rf ~/Library/Logs/XamarinStudio-*
rm -rf ~/Library/XamarinStudio-*
```

## <a name="summary"></a>Souhrn

Tento článek poskytuje instrukcí na úplně odinstalovat Xamarin z Mac prostřednictvím terminálu příkazy. Také poskytuje instrukce v odinstalaci Xamarin z počítače s Windows pomocí **programy a funkce** možnost (pro Visual Studio 2015 a starší) a pomocí **instalační program Visual Studio** pro Visual Studio 2017.


## <a name="related-links"></a>Související odkazy

- [Odinstalujte skriptu (ukázka)](https://raw.githubusercontent.com/MicrosoftDocs/visualstudio-docs/master/mac/resources/uninstall-vsmac.sh)
