---
title: Odinstalování Xamarinu
description: Tento dokument popisuje, jak odinstalovat Xamarin Mac a Windows. Poskytuje konkrétní pokyny k odinstalaci Mono, Xamarin.iOS, Xamarin.Android a další nástroje.
ms.prod: xamarin
ms.assetid: b83a85ec-842a-444c-8f82-c2464eda099b
author: asb3993
ms.author: amburns
ms.date: 04/08/2017
ms.openlocfilehash: 87f59e9f0c2150291a43cdfee4fe6c5dfc2058f8
ms.sourcegitcommit: 4c0093ee5d4aeb16c0e6f0c740c4796736971651
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/23/2018
ms.locfileid: "39203121"
---
# <a name="uninstalling-xamarin"></a>Odinstalování Xamarinu

Tato příručka vysvětluje, jak odebrat Xamarin ze systému macOS nebo ze sady Visual Studio na Windows.

Pokud je nutné znovu nainstalovat Xamarin pomocí univerzální instalačního programu, vždy doporučujeme nejprve restartování počítače.

## <a name="uninstalling-xamarin-on-macos"></a>Odinstalování Xamarinu v systému macOS

Tato příručka slouží k odinstalaci jednotlivých produktů jednotlivě tak, že přejdete do příslušné části. Na celou sadu nástrojů Xamarin, která zahrnuje uvedené produkty, můžou se odinstalovat podle této příručky celý způsob, jak prostřednictvím:

- [Mono](#uninstallmono)
- [Xamarin.Android](#uninstallandroid)
- [Xamarin.iOS](#uninstallios)
- [Xamarin.Mac](#uninstallmac)
- [Sešity](#uninstallworkbooks)
- [Xamarin Profiler](#uninstallprofiler)
- [Instalační program](#uninstallinstaller)

> [!TIP]
> Nabízíme [odinstalovat skript](https://raw.githubusercontent.com/MicrosoftDocs/visualstudio-docs/master/mac/resources/uninstall-vsmac.sh) pro použití při odebírání Xamarin z počítače s macOS. Další informace o používání skriptu, najdete v článku [pomocí skriptu odinstalace](#uninstallscript) oddíly této příručky.

### <a name="uninstalling-visual-studio-for-mac"></a>Odinstalace sady Visual Studio pro Mac

Prvním krokem při odinstalování Xamarinu na macu, je nalezení **Visual Studio.app** v **/Applications** adresáře a přetáhněte ji do **koše**. Alternativně klepněte pravým tlačítkem myši a vyberte **přesunout do koše** jak je znázorněno na následujícím obrázku:

![Aplikace Visual Studio přesunout do koše](uninstalling-xamarin-images/uninstall-image1.png)

Odstraněním této sady prostředků aplikace dojde k odebrání sady Visual Studio pro Mac, i když můžou existovat jiné soubory týkající se xamarinu stále v systému souborů.

Chcete-li odebrat všechna trasování sady Visual Studio pro Mac, spuštěním následujících příkazů v terminálu:

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

### <a name="uninstall-mono-sdk-mdk"></a>Odinstalujte modul Mono SDK (MDK)

Mono je open source implementace rozhraní .NET Framework a umožňují tak, že všechny Xamarin Products—Xamarin.iOS, Xamarin.Android a Xamarin.Mac vývoj z těchto platforem v jazyce C#.

> [!WARNING]
> Existují jiné aplikace mimo Xamarin využívající Mono, jako je Unity. Ujistěte se, že neexistují žádné závislosti na Mono před odinstalací.

Chcete-li odebrat Mono Frameworku, spuštěním následujících příkazů v terminálu:

```bash
sudo rm -rf /Library/Frameworks/Mono.framework
sudo pkgutil --forget com.xamarin.mono-MDK.pkg
sudo rm /etc/paths.d/mono-commands
```

<a name="uninstallandroid" />

### <a name="uninstall-xamarinandroid"></a>Odinstalujte Xamarin.Android

Existuje několik položek, které jsou potřeba při používání Xamarin.Android, jako jsou sady Android SDK a sady Java SDK, které musí být odstraněny při odinstalaci Xamarin.Android. Tato část vás provede s odinstalací všechny nezbytné součásti.

Chcete-li odebrat Xamarin.Android, spuštěním následujících příkazů v terminálu:

```bash
sudo rm -rf /Developer/MonoDroid
rm -rf ~/Library/MonoAndroid
sudo pkgutil --forget com.xamarin.android.pkg
sudo rm -rf /Library/Frameworks/Xamarin.Android.framework
```

#### <a name="uninstall-android-sdk-and-java-sdk"></a>Odinstalace Android SDK a Java SDK

Sady Android SDK je vyžadována pro vývoj aplikací pro Android. K úplnému odebrání všech součástí sady Android SDK, vyhledávat soubor za **~/Library/Developer/Xamarin/** a přesuňte ho do **koše**.

Java SDK (JDK) není potřeba odinstalovat, protože je již předem zabalené jako součást systému Mac OS X.

#### <a name="uninstall-android-avd"></a>Odinstalace Android AVD

> [!WARNING]
> Existují jiné aplikace mimo sadu Visual Studio pro Mac, které také používají Android AVD a tyto další součásti pro android, jako je například Android Studio.
> Odebírá se tento adresář může způsobit projektů zrušte v nástroji Android Studio. 

Chcete-li odebrat všechny Avd Android a další součásti pro Android, použijte následující příkaz:

```bash
rm -rf ~/.android
```

Pokud chcete odebrat pouze Android Avd, použijte následující příkaz:

```bash
rm -rf ~/.android/avd
```

<a name="uninstallios" />

### <a name="uninstall-xamarinios"></a>Odinstalujte Xamarin.iOS

Xamarin.iOS umožňuje vývoj aplikací pomocí C# nebo F # s Iosem. Chcete-li odinstalovat Xamarin.iOS z počítače, použijte následující postup:

Chcete-li odebrat všechny soubory Xamarin.iOS, použijte následující příkazy, v terminálu:

```bash
rm -rf ~/Library/MonoTouch
sudo rm -rf /Library/Frameworks/Xamarin.iOS.framework
sudo rm -rf /Developer/MonoTouch
sudo pkgutil --forget com.xamarin.monotouch.pkg
sudo pkgutil --forget com.xamarin.xamarin-ios-build-host.pkg
sudo pkgutil --forget com.xamarin.xamarin.ios.pkg
```

<a name="uninstallmac" />

### <a name="uninstall-xamarinmac"></a>Odinstalujte Xamarin.Mac

Xamarin.Mac lze z počítače odebrat pomocí následujících příkazů pro tento kyberzločinec odstranit produkt a licenci z počítače Mac:

```bash
sudo rm -rf /Library/Frameworks/Xamarin.Mac.framework
rm -rf ~/Library/Xamarin.Mac
```

<a name="uninstallworkbooks" />

### <a name="uninstall-workbooks"></a>Odinstalace sešity

Odebrání sešity ke Xamarinu verze 1.2.2 a vyšší, v terminálu použijte následující příkazy:

```bash
sudo /Library/Frameworks/Xamarin.Interactive.framework/Versions/Current/uninstall
```

Starší verze najdete v tématu [sešity](~/tools/workbooks/install.md#uninstall-macos) odinstalovat průvodce.

<a name="uninstallprofiler" />

### <a name="uninstall-the-xamarin-profiler"></a>Odinstalujte Xamarin Profiler

Chcete-li odebrat Xamarin Profiler, použijte následující příkazy, v terminálu:

```bash
sudo rm -rf "/Applications/Xamarin Profiler.app"
```

<a name="uninstallinstaller" />

### <a name="uninstall-the-xamarin-installer"></a>Odinstalovat instalační program Xamarin

Pokud chcete odebrat všechna trasování Xamarin univerzální instalačního programu, použijte následující příkazy:

```bash
rm -rf ~/Library/Caches/XamarinInstaller/
rm -rf ~/Library/Caches/VisualStudioInstaller/
rm -rf ~/Library/Logs/XamarinInstaller/
rm -rf ~/Library/Logs/VisualStudioInstaller/
rm -rf ~/Library/Preferences/Xamarin/
rm -rf "~/Library/Preferences/Visual Studio/"
```

<a name="uninstallscript" />

### <a name="using-the-uninstall-script"></a>Pomocí odinstalačního skriptu, který

Tento skript pro odinstalaci umožňuje Odinstalace sady Visual Studio pro Mac a její přidružené komponenty Xamarin najednou.

Skript obsahuje většinu příkazů, které se nacházejí v tomto článku. Existují dva hlavní opomenutí ze skriptu a nejsou zahrnuty z důvodu možných externích závislostí:

- Odinstalace Mono
- Probíhá odinstalace AVD na Androidu

Spusťte skript, proveďte následující kroky:

1. Klikněte pravým tlačítkem na skript a vyberte Uložit jako... Uložte soubor na vašem počítači Mac.

2.  Otevřít **terminálu** a ve kterém se skript stáhl změňte pracovní adresář:

        $ cd /location/of/file

3. Spustitelný soubor skriptu a spustit ji s **sudo**:

        $ chmod +x ./xamarin_uninstall.sh
        $ sudo ./xamarin_uninstall.sh

4. Nakonec odstraňte skript pro odinstalaci.

V tomto okamžiku Xamarin být odinstalován z počítače.

<a name="uninstallwindows" />

## <a name="uninstalling-xamarin-on-windows"></a>Odinstalování Xamarinu ve Windows

Xamarin je podporoval na následující:

- [Visual Studio 2017](#uninstallvs2017)
- [Visual Studio 2015](#uninstallvs2015)
- [Visual Studio 2013](#uninstallvs2015) [**nepodporované**]
- [Xamarin Studio](#uninstallxamarinstudio) [**nepodporované**]

<a name="uninstallvs2017" />

### <a name="visual-studio-2017"></a>Visual Studio 2017

Xamarin se odinstaluje ze sady Visual Studio 2017 pomocí instalačního programu aplikace:

1. Použití **nabídky Start** otevřít **instalační program sady Visual Studio**.

2. Stisknutím klávesy **změnit** tlačítko pro instanci chcete změnit.

    [![](uninstalling-xamarin-images/vs2017-02-sml.png "Klikněte na tlačítko Upravit")](uninstalling-xamarin-images/vs2017-02.png#lightbox)

3. V **úlohy** kartu, zrušte výběr **vývoj mobilních aplikací pomocí .NET** možnosti (v **mobilní aplikace a hry** části).

    [![](uninstalling-xamarin-images/vs2017-03-sml.png "Zrušte zaškrtnutí políčka úlohy vývoj mobilních aplikací")](uninstalling-xamarin-images/vs2017-03.png#lightbox)

4. Klikněte na tlačítko **změnit** tlačítko v pravém dolním rohu okna.

5. Instalační program odebere zrušení vybrané součásti (Visual Studio 2017 musí být uzavřena předtím, než instalační program můžete provést změny).

    [![](uninstalling-xamarin-images/vs2017-04-sml.png "Klikněte na tlačítko Upravit")](uninstalling-xamarin-images/vs2017-04.png#lightbox)

Jednotlivé komponenty Xamarin (například Profiler nebo sešity), můžou se odinstalovat při přechodu **jednotlivé komponenty** kartu v kroku 3 a zrušením zaškrtnutí políčka konkrétní součásti:

[![](uninstalling-xamarin-images/vs2017-components-sml.png "Odinstalace jednotlivé komponenty")](uninstalling-xamarin-images/vs2017-components.png#lightbox)

Chcete-li úplně odinstalovat Visual Studio 2017, zvolte **odinstalovat** z panelu tři nabídce vedle položky **spuštění** tlačítko.

[![](uninstalling-xamarin-images/vs2017-uninstall-sml.png "Úplně odinstalujte Visual Studio")](uninstalling-xamarin-images/vs2017-uninstall.png#lightbox)

> [!IMPORTANT]
> Pokud máte dva (nebo více) instance sady Visual Studio nainstalované souběžně (SxS) – například vydané verze a Preview verze – odinstalace jedna instance může odstranit některé funkce Xamarin z jiné sady Visual Studio instance, včetně:
>
> - Xamarin Profiler
> - Xamarin sešity/inspektoru
> - Xamarin vzdálený simulátor iOS
> - Apple Bonjour SDK
>
> Za určitých podmínek některé z instancí SxS odinstalace může vést k nesprávné odebrání těchto funkcí. To může snížit výkon platformy Xamarin na instancích sady Visual Studio, která v systému zůstat po odinstalaci SxS instance.
>
>To je vyřešit spuštěním **opravit** možnost v instalačním programu sady Visual Studio, který bude znovu nainstalovat chybějící součásti.


## <a name="uninstalling-older-and-unsupported-products"></a>Odinstalace starší a nepodporovaných produktů

<a name="uninstallvs2015"></a>

### <a name="visual-studio-2015-and-earlier"></a>Visual Studio 2015 a starší

Chcete-li úplně odinstalovat sadu Visual Studio 2015, použijte [odpovědí podpory na stránce visualstudio.com](https://visualstudio.microsoft.com/vs/support/vs2015/uninstall-visual-studio-2015/).

Xamarin, můžou se odinstalovat z počítače s Windows prostřednictvím **ovládací panely**. Přejděte do **programy a funkce** nebo **programy > odinstalovat Program** jak je znázorněno níže:

 [![](uninstalling-xamarin-images/image3.png "Přejděte na programy a funkce nebo programy odinstalovat Program, jak je znázorněno zde")](uninstalling-xamarin-images/image3.png#lightbox) 

Z ovládacího panelu odinstalujte některý z následujících umístění, které jsou k dispozici:

- Xamarin
- Xamarin pro Windows
- Xamarin.Android
- Xamarin.iOS
- Xamarin pro Visual Studio

V Průzkumníku odstraňte všechny zbývající soubory ze složky rozšíření Xamarin Visual Studio (všechny verze, včetně programové soubory a soubory programů (x86)):

``` 
C:\Program Files*\Microsoft Visual Studio 1*.0\Common7\IDE\Extensions\Xamarin
```

Odstraňte sady Visual Studio MEF komponentu mezipaměti adresář, který se musí nacházet v následujícím umístění:

``` 
%LOCALAPPDATA%\Microsoft\VisualStudio\1*.0\ComponentModelCache
```

Vrátit se změnami **VirtualStore** adresáři a zjistí, pokud Windows může mít uložené žádné překrytí pro soubory **Extensions\Xamarin** nebo **ComponentModelCache** adresáře existuje:

``` 
%LOCALAPPDATA%\VirtualStore
```

Otevřete editor registru (regedit) a vyhledejte následující klíč:

``` 
HKEY_LOCAL_MACHINE\SOFTWARE\Wow6432Node\Microsoft\Windows\CurrentVersion\SharedDlls
```

Vyhledat a odstranit všechny položky, které odpovídají tento model:

``` 
C:\Program Files*\Microsoft Visual Studio 1*.0\Common7\IDE\Extensions\Xamarin
```

Pohled pro tento klíč:

``` 
HKEY_CURRENT_USER\Software\Microsoft\VisualStudio\1*.0\ExtensionManager\PendingDeletions
```

Odstraňte všechny položky, které vypadají můžou souviset s Xamarin. Například nic podmínky `mono` nebo `xamarin`.

Otevřete cmd.exe příkazový řádek správce a spusťte `devenv /setup` a `devenv /updateconfiguration` příkazy pro každou nainstalovanou verzi sady Visual Studio. Například pro Visual Studio 2015:

```cmd
"%ProgramFiles(x86)%\Microsoft Visual Studio 14.0\Common7\IDE\devenv.exe" /setup
"%ProgramFiles(x86)%\Microsoft Visual Studio 14.0\Common7\IDE\devenv.exe" /updateconfiguration
```

<a name="uninstallxamarinstudio"></a>

### <a name="uninstall-xamarin-studio-on-windows"></a>Odinstalace sady Xamarin Studio ve Windows

Xamarin Studio se odinstaluje z počítače s Windows prostřednictvím **ovládací panely**. Přejděte do **programy a funkce** nebo **programy > odinstalovat Program** 

Chcete-li odinstalovat Xamarin Studio, vyhledejte **ve tvaru 5.x.x Xamarin Studio** v seznamu programy a klikněte na tlačítko **odinstalovat** tlačítko. 

### <a name="uninstall-xamarin-studio-on-mac"></a>Odinstalujte Xamarin Studio v systému Mac

Prvním krokem při odinstalaci Xamarin Studio v počítači Mac, je nalezení **Xamarin Studio.app** v **/Applications** adresáře a přetáhněte ji do **koše**. Alternativně klepněte pravým tlačítkem myši a vyberte **přesunout do koše** jak je znázorněno níže:

 [![](uninstalling-xamarin-images/image1.png "Alternativně klepněte pravým tlačítkem myši a vyberte možnost přesunout do koše, jak je znázorněno zde")](uninstalling-xamarin-images/image1.png#lightbox)

Při odstranění této sady prostředků aplikace odebere Xamarin Studio, ale existují další soubory týkající se xamarinu stále v systému souborů.

Odebrat všechna trasování Xamarin Studio, by měl být následující příkazy spusťte v terminálu:

```bash
sudo rm -rf "/Applications/Xamarin Studio.app"
rm -rf ~/Library/Caches/XamarinStudio-*
rm -rf ~/Library/Preferences/XamarinStudio-*
rm -rf ~/Library/Logs/XamarinStudio-*
rm -rf ~/Library/XamarinStudio-*
```

## <a name="summary"></a>Souhrn

Tento článek poskytuje instrukce na zcela odinstalování Xamarinu pomocí příkazů v terminálu na macu. Také poskytuje pokyny o odinstalaci z počítače s Windows pomocí Xamarinu **programy a funkce** možnost (pro Visual Studio 2015 a starší) a použití **instalační program sady Visual Studio** pro Visual Studio 2017.


## <a name="related-links"></a>Související odkazy

- [Odinstalace skriptu (ukázka)](https://raw.githubusercontent.com/MicrosoftDocs/visualstudio-docs/master/mac/resources/uninstall-vsmac.sh)
