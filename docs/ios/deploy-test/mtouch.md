---
title: Pomocí mtouch do sady prostředků aplikace Xamarin.iOS
description: Tento dokument popisuje mtouch, nástroj, spusťte v simulátoru jednotek celá řada kroků potřebných k zapnutí aplikace pro Xamarin.iOS do sady prostředků, a nasadit ho do fyzického zařízení.
ms.prod: xamarin
ms.assetid: BCA491DA-E4C1-8689-3EC9-E4C72495A798
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 06/05/2017
ms.openlocfilehash: 49507b6500755c617deacfb197ef089e3debd598
ms.sourcegitcommit: aa9b9b203ab4cd6a6b4fd51e27d865e2abf582c1
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/30/2018
ms.locfileid: "39351353"
---
# <a name="using-mtouch-to-bundle-xamarinios-apps"></a>Pomocí mtouch do sady prostředků aplikace Xamarin.iOS

aplikace pro iPhone se dodávají jako sady aplikace. Toto jsou adresáře s příponou `.app` obsahující váš kód, data, konfigurační soubory a manifestu, který používá iPhone Další informace o vaší aplikaci.

Proces přeměny spustitelný soubor .NET do aplikace většinou doprovází `mtouch` příkaz, nástroj, který integruje mnoho kroků potřebných k zapnutí aplikace do sady. Tento nástroj slouží také ke spuštění aplikace v simulátoru a nasazení softwaru pro skutečné zařízení iPhone nebo iPod Touch.

## <a name="detailed-instructions"></a>Podrobné pokyny

Zkontrolujte naše [mtouch(1)](http://docs.go-mono.com/?link=man%3amtouch(1)) ruční stránka se všemi možné používá mtouch nástroje.

## <a name="installation"></a>Instalace

Na počítači Mac `mtouch` je instalován s Xamarin.iOS. Najdete v následujícím adresáři:

**/Library/Frameworks/Xamarin.IOS.Framework/versions/Current/Bin**

Chcete-li `mtouch` vhodné použít, přidejte do svého systému svého nadřazeného adresáře `PATH` proměnné prostředí.  

Například, chcete-li to provést v prostředí Bash, přidejte následující řádek na konec vaše **~/.bash_profile** souboru:

```bash
export PATH=$PATH:/Library/Frameworks/Xamarin.iOS.framework/Versions/Current/bin
```

> [!WARNING]
> Chcete-li použít `mtouch`, nespoléhejte na existenci **/Developer/MonoTouch/usr/bin**, symbolický odkaz, který odkazuje na **/Library/Frameworks/Xamarin.iOS.framework/Versions/Current/bin**. Tento symbolický odkaz existuje pouze udržovat kompatibilitu s MonoTouch starší verze, které nebyly nainstalovány v **/Library/Frameworks/...** , a můžou zmizet v budoucí verzi.

## <a name="building"></a>Sestavování

`mtouch` Příkazu můžete zkompilovat kód třemi různými způsoby:

-  Kompilace pro testování simulátoru.
-  Kompilace pro nasazení zařízení.
-  Do zařízení nasadíte spustitelný soubor.


### <a name="building-for-the-simulator"></a>Vývoj aplikací pro simulátoru

Když začnete, nejběžnější používaný scénář bude si můžete vyzkoušet si aplikaci v simulátoru, takže budete používat `mtouch -sim` zkompilovat kód do simulátoru balíčku. Uděláte to takto:

```bash
$ mtouch -sim Hello.app hello.exe
```

### <a name="building-for-the-device"></a>Vývoj aplikací pro zařízení

K sestavení software pro toto zařízení vytvoříte aplikaci pomocí `mtouch -dev` možnost, kromě toho je potřeba zadat název certifikátu použitého k podepsání aplikace. Následuje ukázka, jak je aplikace sestavená pro zařízení:

```bash
$ mtouch -dev -c "iPhone Developer: Miguel de Icaza" foo.exe
```

V tomto konkrétním případě používáme "iPhone pro vývojáře: Miguela de Icaza" certifikát pro podepsání aplikace. Tento krok je velmi důležité, nebo fyzické zařízení odmítne načtení aplikace.

 <a name="Running_your_Application" />


## <a name="running-your-application"></a>Spuštění aplikace


### <a name="launching-on-the-simulator"></a>Spouštění v simulátoru

Spouští na simulátoru je velmi jednoduchý, jakmile budete mít sady prostředků aplikace:

```bash
$ mtouch --sdkroot /Applications/Xcode.app -launchsim Hello.app 
```

Pokud `--sdkroot` není nastaven příznak se výchozí nastavení pro cestu xcode-select následek následující upozornění:

> například: upozornění MT0061: No Xcode.app zadán (pomocí--sdkroot proto), pomocí systému Xcode udávaný rutinou "xcode-select--print-path": /Applications/Xcode.app/Contents/Developer 

Příkazový řádek výše uvedené vytvoří některé výstup podobný tomuto:

```bash
Launching application
Application launched
PID: 98460
Press enter to terminate the application
```



Důrazně doporučujeme také ponechat protokolu standardní výstup a chyby souborů pomáhat při vaší ladění. Výstup `Console.WriteLine` přejde na `stdout`a také výstup `Console.Error.WriteLine` a jakékoliv další chybové zprávy modulu runtime míří na `stderr`.

Chcete-li to provést, použijte `--stdout` a `--stderr` příznaky:

```bash
../../tools/mtouch/mtouch --launchsim=Hello.app --stdout=output --stderr=error
```

Pokud se aplikaci nepodaří, zobrazí se výstup a chyby a Diagnostikujte problém.


### <a name="deploying-to-a-device"></a>Nasazení do zařízení

Nasazení do zařízení, budete muset zřídit zařízení, jak je popsáno v od Applu [Správa zařízení](http://developer.apple.com/library/ios/#documentation/Xcode/Conceptual/ios_development_workflow/00-About_the_iOS_Application_Development_Workflow/introduction.html) dokumentu. Po správně zřízení zařízení můžete do zařízení nasadit kompilované ".app" mtouch příkazu. Uděláte to pomocí tohoto příkazu:

```bash
$ mtouch —sdkroot /Applications/Xcode.app -installdev=MyApp.app
```

Pokud `--sdkroot` není nastaven příznak se výchozí nastavení pro cestu xcode-select následek následující upozornění:

> například: upozornění MT0061: No Xcode.app zadán (pomocí--sdkroot proto), pomocí systému Xcode udávaný rutinou "xcode-select--print-path": /Applications/Xcode.app/Contents/Developer 

Tyto kroky jsou obvykle provádí Visual Studio pro Mac.

## <a name="reference"></a>Odkaz

Zobrazit [mtouch(1)](http://docs.go-mono.com/?link=man%3amtouch(1)) ruční stránku Podrobnosti o další možnosti příkazového řádku.



## <a name="related-links"></a>Související odkazy

- [mtouch(1)](http://iosapi.xamarin.com/?link=man%3amtouch(1))
