---
title: Odinstalace Xamarin
description: "Odinstalace Xamarin produkty z počítače"
ms.topic: article
ms.prod: xamarin
ms.assetid: b83a85ec-842a-444c-8f82-c2464eda099b
ms.technology: xamarin-cross-platform
author: asb3993
ms.author: amburns
ms.date: 04/08/2017
ms.openlocfilehash: 1b998628efc133590a543dd45730070a457d61d5
ms.sourcegitcommit: d450ae06065d8f8c80f3588bc5a614cfd97b5a67
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 03/21/2018
---
# <a name="uninstalling-xamarin"></a>Odinstalace Xamarin

> [!IMPORTANT]
> Tento článek vysvětluje, jak odinstalovat Xamarin Studio nebo jiné produkty Xamarin z počítače Mac nebo Windows. Informace o odinstalaci sady Visual Studio pro Mac, najdete [odinstalovat](https://docs.microsoft.com/visualstudio/mac/uninstall) Průvodce na webu docs.microsoft.com

Existuje několik Xamarin produktů, které umožňují vývoj aplikací pro různé platformy, včetně samostatné aplikace, jako je Xamarin Studio a rozšíření do jiných aplikací, jako je podpora pro Xamarin v sadě Visual Studio.

Tato příručka vysvětluje, jak odstranit Xamarin funkce v systému macOS nebo ze sady Visual Studio v systému Windows:

1.  [Odinstalace Xamarin Studio](#uninstallxamarinstudio)
  1.  [Odinstalace Mono](#uninstallmono)
  1.  [Odinstalace Xamarin.Android](#uninstallandroid)
  1.  [Odinstalace Xamarin.iOS](#uninstallios)
  1.  [Odinstalace Xamarin.Mac](#uninstallmac)
  2.  [Odinstalace Inspector a sešitů](#uninstallworkbooks)
1.  [Odinstalace Xamarin ze systému Windows](#uninstallwindows)
  1.  [Odinstalace Xamarin ze sady Visual Studio 2015 a starší](#uninstallvs2015)
  1.  [Probíhá odinstalace Xamarin z Visual Studio 2017](#uninstallvs2017)
1.  [Odinstalace Visual Studio pro Mac](#uninstallvsmac)

Pokud je nutné znovu nainstalovat Xamarin pomocí Universal instalační program, vždy doporučujeme nejprve restartování počítače.

## <a name="uninstalling-xamarin-on-mac"></a>Odinstalace Xamarin v systému Mac

Tato příručka slouží k odinstalaci každý produkt jednotlivě tak, že přejdete do části relevantní. Podle této příručky celou způsob prostřednictvím lze odinstalovat celá sada nástrojů Xamarin.

Získat nápovědu k použití [odinstalovat skriptu](http://download.xamarin.com/developer/cross-platform/xamarin_uninstall.sh), přejít na [pomocí skriptu odinstalovat](#uninstallscript) v dolní části této příručky.

<a name="uninstallxamarinstudio" />

### <a name="uninstall-xamarin-studio"></a>Odinstalace Xamarin Studio

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

<a name="uninstallmono" />

### <a name="uninstall-mono-sdk-mdk"></a>Odinstalujte Mono SDK (MDK)

Mono je implementace s otevřeným zdrojem společnosti Microsoft .NET Framework a umožňuje Xamarin Products—Xamarin.iOS, Xamarin.Android a Xamarin.Mac vývoj těchto platforem v jazyce C#.

> [!IMPORTANT]
> Poznámka: K dispozici jsou další aplikace mimo Xamarin, které také využívají Mono, jako je například Unity. Ujistěte se, že neexistují žádné další závislosti na Mono před odinstalací jej.

Rozhraní Framework Mono odebrat z počítače, spusťte následující příkazy v terminálu:

```bash
sudo rm -rf /Library/Frameworks/Mono.framework
sudo pkgutil --forget com.xamarin.mono-MDK.pkg
sudo rm /etc/paths.d/mono-commands
```

<a name="uninstallandroid" />

### <a name="uninstall-xamarinandroid"></a>Odinstalace Xamarin.Android

Existuje několik položek, které jsou potřebné pro instalaci a použití Xamarin.Android, jako je například Android SDK a sadu Java SDK. Další informace o těchto požadované součásti jsou k dispozici v [ruční instalace](https://docs.microsoft.com/visualstudio/mac/installation/) průvodce.

Chcete-li odebrat Xamarin.Android použijte následující příkazy:

```bash
sudo rm -rf /Developer/MonoDroid
rm -rf ~/Library/MonoAndroid
sudo pkgutil --forget com.xamarin.android.pkg
sudo rm -rf /Library/Frameworks/Xamarin.Android.framework
```

#### <a name="uninstall-android-sdk-and-java-sdk"></a>Odinstalování SDK pro Android SDK a Java

Je vyžadována pro vývoj aplikací pro Android SDK pro Android. Pokud chcete úplně odebrat všechny části sady SDK pro Android, vyhledejte soubor v **~/Library/Developer/Xamarin/** a přesunout ji do **Koš**, jak je uvedeno dále:

 [![](uninstalling-xamarin-images/image2.png "Pokud chcete úplně odebrat všechny části sady SDK pro Android, vyhledejte soubor a přesunout ho koše, jak je znázorněno zde")](uninstalling-xamarin-images/image2.png#lightbox)

Java SDK (JDK) není nutné odinstalovat, protože je již před zabalené jako součást systému Mac OS X.

<a name="uninstallios" />

### <a name="uninstall-xamarinios"></a>Odinstalace Xamarin.iOS

Xamarin.iOS umožňuje iOS vývoj aplikací pomocí jazyka C# nebo F # pomocí Xamarin studia v počítačích Mac.
Sestavení hostitele Xamarin byl také automaticky nainstalován dřívějších verzí aplikace Xamarin.iOS umožňující vývoj pro iOS v sadě Visual Studio. Pokud chcete odinstalovat i z počítače, postupujte následujícím způsobem:

Použijte následující příkazy v terminálu odebere všechny soubory Xamarin.iOS systému souborů:

```bash
rm -rf ~/Library/MonoTouch
sudo rm -rf /Library/Frameworks/Xamarin.iOS.framework
sudo rm -rf /Developer/MonoTouch
sudo pkgutil --forget com.xamarin.monotouch.pkg
sudo pkgutil --forget com.xamarin.xamarin-ios-build-host.pkg
```

#### <a name="uninstall-the-mac-build-host"></a>Odinstalace sestavení hostitele Mac

Poznámka: Toto již byl pravděpodobně odebrán Pokud jste již aktualizovali Xamarin 4 spustit následující příkaz v terminálu k odebrání sestavení hostitelskou aplikaci:

```bash
sudo rm -rf "/Applications/Xamarin.iOS Build Host.app"
```

Proces sestavení hostitele nebo `launchd` může úloha stále spuštěna nebo naslouchá na určité porty.
Jeho stav můžete zkontrolovat spuštěním `launchctl list | grep com.xamarin.mtvs.buildserver` v terminálu.

```bash
sudo launchctl unload /Library/LaunchAgents/com.xamarin.mtvs.buildserver.plist
sudo rm -f /Library/LaunchAgents/com.xamarin.mtvs.buildserver.plist
```

<a name="uninstallmac" />

### <a name="uninstall-xamarinmac"></a>Odinstalace Xamarin.Mac

Jakmile Xamarin Studio byla úspěšně odinstalována, může být Xamarin.Mac odstraněn z počítače pomocí následujících příkazů pro eradikaci produktu a licenci pro počítače Mac v uvedeném pořadí:

```bash
sudo rm -rf /Library/Frameworks/Xamarin.Mac.framework
rm -rf ~/Library/Xamarin.Mac
```

<a name="uninstallworkbooks" />

### <a name="uninstall-workbooks-and-inspector"></a>Odinstalujte sešitů a Inspector

Příkaz Bash odebere verze Xamarin Inspector a sešitů 1.2.2 a vyšší:

```bash
sudo /Library/Frameworks/Xamarin.Interactive.framework/Versions/Current/uninstall
```

Starší verze najdete v tématu [sešity](~/tools/workbooks/install.md#uninstall-macos) odinstalovat průvodce.

### <a name="uninstall-the-xamarin-installer"></a>Odinstalujte instalační program Xamarin

K odebrání všech trasování Xamarin Universal instalačního programu, použijte následující příkazy:

```bash
rm -rf ~/Library/Caches/XamarinInstaller/
rm -rf ~/Library/Logs/XamarinInstaller/
rm -rf ~/Library/Preferences/Xamarin/
```

<a name="uninstallscript" />

### <a name="using-the-uninstall-script"></a>Odinstalační skript pro použití

[Odinstalovat skriptu](http://download.xamarin.com/developer/cross-platform/xamarin_uninstall.sh) Xamarin odebere z počítače. Odinstalační skript pro použití:

1.  Odinstalační skript pro stažení a poznamenejte si umístění stahování. Ve výchozím nastavení **/stáhne** adresáře.
1.  Otevřete **Terminálové** a změnit pracovní adresář, do které jste stáhli skript:

        $ cd /location/of/file

1. Spustitelný soubor skriptu a spustit ji s **sudo**:

        $ chmod +x ./xamarin_uninstall.sh
        $ sudo ./xamarin_uninstall.sh

1. Nakonec odstraňte odinstalační skript.

V tomto okamžiku Xamarin má být odinstalován z počítače.

<a name="uninstallwindows" />

## <a name="uninstalling-xamarin-on-windows"></a>Odinstalace Xamarinem v systému Windows

<a name="uninstallvs2015" />

### <a name="visual-studio-2015-and-earlier"></a>Visual Studio 2015 a starší

Xamarin, můžou se odinstalovat z počítače s Windows pomocí **ovládací panely**. Přejděte na **programy a funkce** nebo **programy > odinstalovat Program** jak je uvedeno dále:

 [![](uninstalling-xamarin-images/image3.png "Přejděte na programy a funkce nebo programy odinstalovat Program, jak je znázorněno zde") ](uninstalling-xamarin-images/image3.png#lightbox) [ ![ ] (uninstalling-xamarin-images/image3.png "přejděte do programy a funkce nebo programy odinstalovat Program jako Zde znázorněný")](uninstalling-xamarin-images/image3.png#lightbox)

Pokud chcete odinstalovat Xamarin Studio, Najít **ve tvaru 5.x.x Xamarin Studio** v seznamu programy a klikněte na tlačítko **odinstalovat** tlačítko. Chcete-li odebrat rozšíření Xamarin pro Visual Studio a sady SDK, Najít **Xamarin** v seznamu programy a klikněte na tlačítko **odinstalovat**. Tyto jsou zobrazené na následující snímek obrazovky:

 [![](uninstalling-xamarin-images/image4a.png "Tyto jsou zobrazené na tomto snímku obrazovky")](uninstalling-xamarin-images/image4a.png#lightbox)

Tyto programy se můžou odebrat také do úplně všech součástí Xamarin:

-  Android SDK


  [![](uninstalling-xamarin-images/image5.png "Tyto programy může být odebrán také úplně všechny součásti Xamarin")](uninstalling-xamarin-images/image5.png#lightbox)
-  GTK#


  [![](uninstalling-xamarin-images/image6.png "Tyto programy může být odebrán také úplně všechny součásti Xamarin")](uninstalling-xamarin-images/image6.png#lightbox)
-  Xamarin Universal Installer


 [![](uninstalling-xamarin-images/image7.png "Tyto programy může být odebrán také úplně všechny součásti Xamarin")](uninstalling-xamarin-images/image7.png#lightbox)
-  Java SDK (buďte opatrní při odebrání, protože na něm mohou být další závislosti)


 [![](uninstalling-xamarin-images/image8.png "Buďte opatrní při odebrání sady Java SDK, protože na něm mohou být další závislosti")](uninstalling-xamarin-images/image8.png#lightbox)

Úplně odinstalujte Visual Studio, postupujte podle [pokyny společnosti Microsoft](https://msdn.microsoft.com/library/mt720585.aspx).


<a name="uninstallvs2017" />

## <a name="visual-studio-2017"></a>Visual Studio 2017

Xamarin odinstalovat pomocí instalačního programu aplikace Visual Studio 2017:

1. Použití **nabídky Start** otevřete **instalační program Visual Studio**.

  [![](uninstalling-xamarin-images/vs2017-01-sml.png "Spusťte instalační program sady Visual Studio")](uninstalling-xamarin-images/vs2017-01.png#lightbox)

1. Stiskněte **upravit** tlačítko pro instanci chcete změnit.

  [![](uninstalling-xamarin-images/vs2017-02-sml.png "Klikněte na tlačítko Upravit")](uninstalling-xamarin-images/vs2017-02.png#lightbox)

1. V **úlohy** kartě, zrušte výběr **vývoj mobilních řešení s .NET** možnost (v **mobilní a herní** části).

  [![](uninstalling-xamarin-images/vs2017-03-sml.png "Zrušte zaškrtnutí políčka zatížení vývoj mobilních řešení")](uninstalling-xamarin-images/vs2017-03.png#lightbox)

1. Klikněte **upravit** tlačítko v pravém dolním rohu okna.
1. Instalační program odebere zrušte vybrané součásti (Visual Studio 2017 musí být uzavřeny předtím, než instalační program provede všechny změny).

  [![](uninstalling-xamarin-images/vs2017-04-sml.png "Klikněte na tlačítko Upravit")](uninstalling-xamarin-images/vs2017-04.png#lightbox)

Odinstalovat jednotlivých součástí Xamarin (například profileru nebo sešity), při přechodu **jednotlivých součástí** v kroku 3 a zrušením zaškrtnutí konkrétní součásti:

[![](uninstalling-xamarin-images/vs2017-components-sml.png "Odinstalace jednotlivých součástí")](uninstalling-xamarin-images/vs2017-components.png#lightbox)

Úplně odinstalujte Visual Studio 2017, zvolte **odinstalace** z panelu tři nabídce vedle položky **spusťte** tlačítko.

[![](uninstalling-xamarin-images/vs2017-uninstall-sml.png "Úplně odinstalujte Visual Studio")](uninstalling-xamarin-images/vs2017-uninstall.png#lightbox)

> [!IMPORTANT]
> **Upozornění:** Pokud máte nainstalovanou sadu Visual Studio – souběžného (SxS) – například verze a verze Preview – dva (nebo více) instancí odinstalace jedna instance může odstranit některé funkce Xamarin z jiné instance Visual Studio včetně:
>
> - Xamarin Profiler
> - Xamarin sešity nebo Inspector
> - Simulátoru vzdálené Xamarin iOS
> - Apple Bonjour SDK
>
> Za určitých podmínek může jedna z instancí SxS odinstalace způsobit nesprávné odebrání těchto funkcí. To může snížit výkon na platformě Xamarin v sadě Visual Studio instance, které zůstaly po odinstalaci SxS instance systému.
>
>To lze vyřešit tak, že spustíte **Repair** možnosti v instalačním programu sady Visual Studio bude znovu instalaci chybějících součástí.

<a name="uninstallvsmac" />

## <a name="uninstalling-visual-studio-for-mac"></a>Odinstalace Visual Studio pro Mac

Odinstalace Visual Studio pro Mac, ale chcete dál používat Xamarin Studio, vyhledejte **Visual Studio.app** v **/Applications** adresáře a přetáhněte jej do Can. Koš Případně, klikněte pravým tlačítkem a vyberte **přesunout do Koš** jak je uvedeno dále:

 [![](uninstalling-xamarin-images/image9.png "Klikněte pravým tlačítkem na ikonu sady Visual Studio a vyberte možnost přesunout na Koš")](uninstalling-xamarin-images/image9.png#lightbox)

Úplně odinstalujte Xamarin z počítače, nejprve odstranit sady Visual Studio pro Mac a potom postupujte podle kroků uvedených v [odinstalovat Xamarin Studio](#uninstallxamarinstudio) části.

## <a name="summary"></a>Souhrn

V tomto článku jsme se podívali na úplně odinstalovat Xamarin z Mac prostřednictvím terminálu příkazy, a také odinstalaci Xamarin ze systému Windows počítače prostřednictvím **programy a funkce** možnost (pro Visual Studio 2015 a starší) a pomocí **instalační program Visual Studio** pro Visual Studio 2017.


## <a name="related-links"></a>Související odkazy

- [Odinstalujte skriptu (ukázka)](http://download.xamarin.com/developer/cross-platform/xamarin_uninstall.sh)
