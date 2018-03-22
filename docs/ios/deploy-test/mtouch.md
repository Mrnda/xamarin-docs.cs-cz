---
title: mtouch
ms.topic: article
ms.prod: xamarin
ms.assetid: BCA491DA-E4C1-8689-3EC9-E4C72495A798
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.openlocfilehash: bd9be12ee1d67c7c071cf8fcfb49b4d888258dae
ms.sourcegitcommit: 73bd0c7e5f237f0a1be70a6c1384309bb26609d5
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 03/22/2018
---
# <a name="mtouch"></a>mtouch


iPhone aplikace jsou dodávána jako sady aplikace. Jedná se o adresáře s příponou `.app` obsahující kódu, data, konfiguračních souborů a manifestu, který používá iPhone Další informace o vaší aplikace.

Proces zapnutí spustitelný soubor rozhraní .NET do aplikace většinou vycházejí z `mtouch` příkaz, nástroj, který se integruje se řadu kroky potřebné k vypnutí aplikace do sady. Tento nástroj se používá také ke spuštění aplikace v simulátoru a nasazení softwaru do samotného zařízení iPhone nebo iPod Touch.


## <a name="detailed-instructions"></a>Podrobné pokyny

Zkontrolujte naše [mtouch(1)](http://docs.go-mono.com/?link=man%3amtouch(1)) ruční stránka s všechny možné používá mtouch nástroje.

## <a name="installation"></a>Instalace

V systému Mac `mtouch` je instalován s Xamarin.iOS. Se nachází v následujícím adresáři:

**/Library/Frameworks/Xamarin.IOS.Framework/versions/Current/Bin**

Chcete-li `mtouch` vhodnější použít, přidejte její adresář nadřazené do vašeho systému `PATH` proměnné prostředí.  

Například k tomu v Bash, přidejte následující řádek na konec vaší **~/.bash_profile** souboru:

```bash
export PATH=$PATH:/Library/Frameworks/Xamarin.iOS.framework/Versions/Current/bin
```

> [!WARNING]
> Chcete-li použít `mtouch`, nespoléhejte na existenci **/Developer/MonoTouch/usr/bin**, symbolický odkaz, který odkazuje na **/Library/Frameworks/Xamarin.iOS.framework/Versions/Current/bin**. Tento symbolický odkaz existuje pouze udržovat kompatibilitu s MonoTouch starší verze, které nebyly nainstalovány v **/Library/Frameworks/...** , a může zmizet v budoucí verzi.

## <a name="building"></a>Sestavování

`mtouch` Příkaz můžete zkompilovat kód třemi různými způsoby:

-  Kompilace pro testování simulátoru.
-  Kompilace pro nasazení zařízení.
-  Nasazení vaší spustitelný soubor do zařízení.


### <a name="building-for-the-simulator"></a>Vytváření pro simulátoru

Když začnete, nejběžnější použité scénář budou vám vyzkoušet aplikaci v simulátoru, takže budete používat `mtouch -sim` zkompilovat kód do balíčku simulátoru. Děje se to jako:

```bash
$ mtouch -sim Hello.app hello.exe
```

### <a name="building-for-the-device"></a>Vytváření pro zařízení

K vytvoření software pro zařízení se sestavení aplikace pomocí `mtouch -dev` možnost, kromě toho je třeba zadat název certifikátu používaného k podepsání aplikace. Následující ukazuje, jak je aplikace vytvořené pro zařízení:

```bash
$ mtouch -dev -c "iPhone Developer: Miguel de Icaza" foo.exe
```

V tomto případě používáme "iPhone Developer: Miguel de Icaza" certifikát pro podepsání aplikace. Tento krok je velmi důležité, nebo se načíst aplikaci udělit fyzického zařízení.

 <a name="Running_your_Application" />


## <a name="running-your-application"></a>Spuštění aplikace


### <a name="launching-on-the-simulator"></a>Spouštění v simulátoru

Spouštění v simulátoru je velmi jednoduchý, až budete mít sadě aplikací:

```bash
$ mtouch --sdkroot /Applications/Xcode.app -launchsim Hello.app 
```

Pokud `--sdkroot` není nastaven příznak se výchozí hodnota je xcode vyberte cestu a způsobí následující upozornění:

> např: upozornění MT0061: Ne Xcode.app zadán (pomocí – sdkroot), pomocí systému Xcode vykazované 'xcode – vybrat – tisk path': /Applications/Xcode.app/Contents/Developer 

Výše uvedené příkazového řádku způsobí některé výstup takto:

```bash
Launching application
Application launched
PID: 98460
Press enter to terminate the application
```



Důrazně doporučujeme, abyste protokolu standardní výstupní zařízení a standardní chyba soubory pomoct vaší ladění. Výstup `Console.WriteLine` přejde na `stdout`a také výstup `Console.Error.WriteLine` a jakékoliv další chybové zprávy modulu runtime přejde na `stderr`.

Chcete-li to provést, použijte `--stdout` a `--stderr` příznaky:

```bash
../../tools/mtouch/mtouch --launchsim=Hello.app --stdout=output --stderr=error
```

Pokud vaše aplikace nezdaří, zobrazí se výstup a chyby a Diagnostikujte problém.


### <a name="deploying-to-a-device"></a>Nasazení do zařízení

K nasazení na zařízení, budete muset zřídit zařízení, jak je popsáno v společnosti Apple [Správa zařízení](http://developer.apple.com/library/ios/#documentation/Xcode/Conceptual/ios_development_workflow/00-About_the_iOS_Application_Development_Workflow/introduction.html) dokumentu. Po správně zřizují zařízení, můžete použít příkaz mtouch kompilované ".app" nasadit do zařízení. Uděláte to pomocí tohoto příkazu:

```bash
$ mtouch —sdkroot /Applications/Xcode.app -installdev=MyApp.app
```

Pokud `--sdkroot` není nastaven příznak se výchozí hodnota je xcode vyberte cestu a způsobí následující upozornění:

> např: upozornění MT0061: Ne Xcode.app zadán (pomocí – sdkroot), pomocí systému Xcode vykazované 'xcode – vybrat – tisk path': /Applications/Xcode.app/Contents/Developer 

Tyto kroky jsou obvykle provádí Visual Studio for Mac.

## <a name="reference"></a>Odkaz

Najdete v článku [mtouch(1)](http://docs.go-mono.com/?link=man%3amtouch(1)) ruční stránku Podrobnosti o další možnosti příkazového řádku.



## <a name="related-links"></a>Související odkazy

- [mtouch(1)](http://iosapi.xamarin.com/?link=man%3amtouch(1))
