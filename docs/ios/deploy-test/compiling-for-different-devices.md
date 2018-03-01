---
title: "Kompilování pro různá zařízení"
ms.topic: article
ms.prod: xamarin
ms.assetid: 3B259248-887E-3E4F-E09C-7AD28C2A8CEE
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/18/2017
ms.openlocfilehash: 13838215b32abe49a5fe07b04088bc4216250844
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 02/27/2018
---
# <a name="compiling-for-different-devices"></a>Kompilování pro různá zařízení

Vlastnosti sestavení vaší spustitelný soubor lze konfigurovat v projektu **iOS sestavení** stránku vlastností, který se nachází kliknutím pravým tlačítkem myši na název projektu a přejděte na **možnosti > iOS sestavení** v Visual Studio pro Mac, a **vlastnosti** v sadě Visual Studio:

[[ide name="xs"]]

[ ![](compiling-for-different-devices-images/image1.png "Stránka vlastností iOS sestavení projektů")](compiling-for-different-devices-images/image1.png) 

[[/ide]] 

[[ide name="vs"]]

[ ![](compiling-for-different-devices-images/image1a.png "Stránka vlastností iOS sestavení projektů")](compiling-for-different-devices-images/image1a.png)

[[/ide]]

Kromě možností konfigurace k dispozici v Uživatelském rozhraní můžete předat také vlastní sadu možností příkazového řádku a [Xamarin.iOS sestavení nástroj (mtouch)](~/ios/deploy-test/mtouch.md).

[http://iossupportmatrix.com/](http://iossupportmatrix.com/) je užitečné prostředků, které je možné zajistit, který chcete zahrnout všechny požadované zařízení, architektury a iOS verze.

 <a name="SDK_Options" />


## <a name="sdk-options"></a>Možnosti sady SDK

Visual Studio pro Mac můžete nakonfigurovat dvě důležité vlastnosti týkající se sady SDK: verze sady SDK iOS sloužící k vytvoření vašeho softwaru a cíl nasazení (nebo iOS minimální požadované verze).

IOS **verze sady SDK** možnost umožňuje použít různé verze Apple publikované sady SDK, to přesměruje Xamarin.iOS kompilátory, linkers a knihovny, které by měl odkazovat během buildu. 

**Cíl nasazení** nastavení slouží k výběru minimální požadovaná verze operačního systému, na kterém aplikace poběží. Je nastavena v souboru Info.plist vašeho projektu. Měli byste vybrat minimální verzi, která má všechna rozhraní API, které budete muset spustit svoji aplikaci.

Obecně platí, rozhraní API Xamarin.iOS zpřístupní všechny metody, které jsou k dispozici v nejnovější verzi sady SDK, a pokud je to nezbytné, poskytujeme pohodlí vlastnosti, které vám umožní detekce, zda je k dispozici v době běhu funkce (například `UIDevice.UserInterfaceIdiom` a `UIDevice.IsMultitaskingSupported` vždycky pracovali na Xamarin.iOS, provedeme všechnu práci na pozadí).

 <a name="Linking" />


## <a name="linking"></a>Propojení

Najdete na naší stránce s vyhrazenou [Linkeru](~/ios/deploy-test/linker.md) Další informace o tom, jak linkeru pomáhá snížit velikost vašeho spustitelné soubory a zjistěte, jak efektivně používat.

 <a name="Code_Generation_Engine" />


## <a name="code-generation-engine"></a>Modul generování kódu

Od verze Xamarin.iOS 4.0, existují dvě kódu generování back-EndY pro Xamarin.iOS. Modul generování regulární Mono kódu a jeden podle LLVM optimalizace kompilátoru. Každý motor má své výhody a nevýhody.

Obvykle během procesu vývoje pravděpodobně použijete modul generování Mono kód jako jeho vám umožní rychle iterovat. Verze sestavení a nasazení AppStore můžete přepnout do modulu LLVM generování kódu.

LLVM optimalizace back-end modul vytvoří kódu rychlejší a spolehlivější než Mono modul nemá cenu kompilace dlouhou dobu.

Můžete povolit z iOS možnosti sestavení v sadě Visual Studio pro Mac nebo Visual Studio.

[ ![](compiling-for-different-devices-images/image2.png "Povolení LLVM")](compiling-for-different-devices-images/image2.png)

[ ![](compiling-for-different-devices-images/image2a.png "Povolení LLVM")](compiling-for-different-devices-images/image2a.png)

 <a name="ARMV7_and_ARMV7s_support" />


## <a name="architecture-support"></a>Podpora architektury

<a name="armv6-discontinued" />

### <a name="armv6-xamarinios-discontinued-support-for-armv6-with-v810"></a>ARMv6 (Xamarin.iOS ukončení podpory pro ARMv6 s v8.10)

- iPhone (původní), 3G
- iPod 1, 2. generace

### <a name="armv7"></a>ARMv7

- iPhone 3GS, 4, 4S
- iPad 1, 2, 3, malé
- iPod 3, 4, 5. generace

### <a name="armv7s"></a>ARMv7s

- iPhone 5
- iPhone 5c
- iPad 4

Pokud cílíte pouze ARMv7s procesoru, kód, který vygenerovala bude mírně rychlejší, ale ho nebude možné spustit v systémech ARMv7 nebo ARMv6 pokud zkompilujete fat binární soubor, který obsahuje více spustitelné soubory v balíčku.

### <a name="arm64-xamarinios-started-supporting-arm64-in-v86"></a>ARM64 (Xamarin.iOS spuštěna podpora ARM64 v v8.6)

- iPhone 5s
- iPhone SE
- iPhone 6, 6 Plus
- iPhone 6s, 6s Plus
- iPhone 7, 7 Plus
- iPhone 8, 8 Plus
- iPhone X
- iPad letecké
- iPad Air 2
- iPad malé 2, 3, 4
- iPad Pro (vše)

Všimněte si, že sestavení, odeslána na obchod s aplikacemi musí obsahovat podporu 64bitové představuje požadavek nastaven [Apple](https://developer.apple.com/news/?id=12172014b). IOS 11 navíc podporuje pouze 64bitové aplikace.

 <a name="ARM_Thumb_Support" />


### <a name="arm-thumb-2-support"></a>Podpora ARM jezdec-2

Jezdec je kompaktnější sada instrukcí používané procesory ARM. Povolení podpory Flash, můžete snížit velikost vašeho spustitelný soubor, za cenu pomalejší časy spuštění. Jezdec je podporována v ARMv7 a ARMv7s.

 <a name="Conditional_framwork_useage" />


## <a name="conditional-framework-usage"></a>Použití podmíněného Framework

Pokud projekt chce využít některé z funkcí v iOS novější verze, budete muset podmíněně závisí na určitých nové architektury. Typickým příkladem tohoto objektu je chtějí využívat iAd při spouštění v systému iOS 4.0 nebo vyšší, ale stále podporu 3.x zařízení. K tomu potřebujete umožnit Xamarin.iOS vědět, že je potřeba propojit proti rozhraní iAd "slabě". Slabé vazby Ujistěte se, že rozhraní načteny pouze na vyžádání první čas, kdy je požadované třídy z rozhraní.

K tomu můžete provést následující kroky:

-  Otevřete váš **možnosti projektu** a přejděte do **iOS sestavení** podokně.
-  Přidat `'-gcc_flags "-weak_framework iAd"'` k **další možnosti** pro každou konfiguraci chcete slabě odkaz na:


[ ![](compiling-for-different-devices-images/image3.png "Další možnosti")](compiling-for-different-devices-images/image3.png)


Kromě to potřebujete ochrana vašeho využití typy neběžely na starší verzí systému iOS, kde možná neexistuje. Existuje několik metod k tomu, ale z nichž jeden je analýza `UIDevice.CurrentDevice.SystemVersion`.



## <a name="related-links"></a>Související odkazy

- [Linker](~/ios/deploy-test/linker.md)
- [Externí - iOS matici podpory](http://iossupportmatrix.com/)
