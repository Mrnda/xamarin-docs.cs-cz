---
title: "Návod: Vytvoření vazby iOS knihovna jazyka Objective-C"
description: "Tento článek obsahuje praktických návod, jak vytvořit vazbu Xamarin.iOS pro existující knihovnu jazyka Objective-C, InfColorPicker. Pokrývá oblastech, jako je kompilování statické knihovny jazyka Objective-C, jeho vazby a vazbu pomocí aplikace pro Xamarin.iOS."
ms.topic: article
ms.prod: xamarin
ms.assetid: D3F6FFA0-3C4B-4969-9B83-B6020B522F57
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/18/2017
ms.openlocfilehash: e4619f5b1d3f888b2557cf894aaa83106504766f
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 03/09/2018
---
# <a name="walkthrough-binding-an-ios-objective-c-library"></a>Návod: Vytvoření vazby iOS knihovna jazyka Objective-C

_Tento článek obsahuje praktických návod, jak vytvořit vazbu Xamarin.iOS pro existující knihovnu jazyka Objective-C, InfColorPicker. Pokrývá oblastech, jako je kompilování statické knihovny jazyka Objective-C, jeho vazby a vazbu pomocí aplikace pro Xamarin.iOS._

Při práci na iOS, můžete se setkat případy, ve které chcete používat knihovnu jazyka Objective-C třetích stran. V těchto situacích můžete použít Xamarin.iOS _vazby projektu_ k vytvoření [C# vazby](~/cross-platform/macios/binding/overview.md) který vám umožní využívat knihovny v nástroji aplikace Xamarin.iOS.

Obecně v ekosystému iOS najdete knihovny v 3 charakteristikami:

* Jako soubor předkompilovaných statické knihovny s `.a` rozšíření spolu s jeho záhlaví (souborů .h). Například [knihovnu Google Analytics](https://developers.google.com/analytics/devguides/collection/ios/v3/sdk-download?hl=es#download_sdk)
* Jako předkompilovaných rozhraní. Toto je právě složku obsahující statickou knihovnu, záhlaví a někdy další prostředky s `.framework` rozšíření. Například [Google AdMob knihovny](https://developers.google.com/admob/ios/download).
* Jako soubory zdrojového kódu. Například knihovny obsahující právě `.m` a `.h` Objective C soubory.

V prvním a druhém scénáři již nebude předkompilovaných CocoaTouch statickou knihovnu, v tomto článku se zaměříme na třetí scénář. Mějte na paměti, než se pustíte do vytvoření vazby, vždy zkontrolujte licenční zadaný s knihovnou zajistit, že jste volné pro vytvoření vazby.

Tento článek obsahuje podrobný postup pro vytvoření vazby projektu pomocí open source [InfColorPicker](https://github.com/InfinitApps/InfColorPicker) jazyka Objective-C projektu jako příklad, ale všechny informace v této příručce lze upravit a použít s žádným Knihovna jazyka Objective-C třetích stran. Knihovna InfColorPicker obsahuje opakovaně použitelné zobrazení kontroler, který umožňuje uživateli vybrat barvy založený na jeho reprezentace HSB, provedení přívětivější výběr barev.

[![](walkthrough-images/run01.png "Příklad knihovny InfColorPicker systémem iOS")](walkthrough-images/run01.png#lightbox)

Jsme zaměříme všechny potřebné kroky využívat toto konkrétní rozhraní API jazyka Objective-C v Xamarin.iOS:

- Nejdříve vytvoříme jazyka Objective-C statickou knihovnu, v pomocí Xcode.
- Potom jsme budete vazbu této statické knihovny s Xamarin.iOS.
- V dalším kroku zobrazit, jak můžete snížit zatížení pomocí automatického generování některé (ale ne všechny) cíl Sharpie nutné definice rozhraní API vyžaduje Xamarin.iOS vazby.
- Nakonec vytvoříme aplikaci Xamarin.iOS, která používá vazby.

Ukázkovou aplikaci ukazují, jak používat silné delegáta pro komunikaci mezi rozhraní API InfColorPicker a kódu jazyka C#. Po tom, jak používat silné delegáta jsme viděli, jak používat slabé delegátů k provádění stejných úloh jsme zaměříme.

## <a name="requirements"></a>Požadavky

Tento článek předpokládá, že máte některé znalost Xcode a jazyka Objective-C a jste četli naše [vazby Objective-C](~/cross-platform/macios/binding/index.md) dokumentaci. Kromě toho je nutné provést následující chcete provést kroky uvedené:

-  **Xcode a iOS SDK** -Xcode společnosti Apple a nejnovější rozhraní API pro iOS musí být nainstalovaná a nakonfigurovaná na počítači pro vývojáře.
-  **[Nástroje příkazového řádku Xcode](#Installing_the_Xcode_Command_Line_Tools)**  – nástroje pro příkazový řádek Xcode musí být nainstalován pro aktuálně nainstalovanou verzi Xcode (dole najdete podrobné informace o instalaci).
-  **Visual Studio pro Mac nebo Visual Studio** – nejnovější verze sady Visual Studio pro Mac nebo Visual Studio musí být nainstalovaná a nakonfigurovaná na vývojovém počítači. Je vyžadován pro vývoj aplikace pro Xamarin.iOS Apple Mac, a když pomocí sady Visual Studio můžete musí být připojen k [Xamarin.iOS sestavení hostitele](~/ios/get-started/installation/windows/connecting-to-mac/index.md)
-  **Nejnovější verzi cíl Sharpie** -aktuální kopii nástroje cíl Sharpie stáhnout z [zde](~/cross-platform/macios/binding/objective-sharpie/get-started.md). Pokud už máte nainstalovaný Sharpie cíl, můžete je aktualizovat ji na nejnovější verzi pomocí `sharpie update`

<a name="Installing_the_Xcode_Command_Line_Tools"/>

## <a name="installing-the-xcode-command-line-tools"></a>Instalace nástroje Xcode příkazového řádku

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)


Jak jsme uvedli výše, budeme používat nástroje příkazového řádku Xcode (konkrétně `make` a `lipo`) v tomto návodu. `make` Příkaz je velmi běžné Unix nástroj, který bude automatizovat kompilace spustitelné programy a knihovny pomocí _makefile_ určující, jak by měly být vytvořeny program. `lipo` Příkaz je nástroj příkazového řádku OS X pro vytváření souborů více architektura; ho bude kombinovat více `.a` soubory do jednoho souboru, který mohou využívat všechny architektury hardwaru.


# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)


Jak jsme uvedli výše, budeme nástroje příkazového řádku Xcode používat na **Mac sestavení hostitele** (konkrétně `make` a `lipo`) v tomto návodu. `make` Příkaz je velmi běžné Unix nástroj, který bude automatizovat kompilace spustitelné programy a knihovny pomocí _makefile_ k Určuje, jak vytvořit program. `lipo` Příkaz je nástroj příkazového řádku OS X pro vytváření souborů více architektura; ho bude kombinovat více `.a` soubory do jednoho souboru, který mohou využívat všechny architektury hardwaru.


-----

Podle společnosti Apple [sestavení z příkazového řádku s Xcode – nejčastější dotazy](https://developer.apple.com/library/ios/technotes/tn2339/_index.html) dokumentace, OS X 10.9 a je větší, **stáhne** podokně Xcode **Předvolby** dialogu není nadále podporuje stahování nástroje příkazového řádku.

Budete muset použít jednu z následujících metod k instalaci nástroje:

- **Instalaci Xcode** – při instalaci Xcode, obsahuje také program s všechny nástroje příkazového řádku. V OS X 10.9 překrytí (nainstalované v `/usr/bin`), můžete namapovat jakýkoli nástroj součástí `/usr/bin` odpovídající nástroj v Xcode. Například `xcrun` příkaz, který umožňuje najít nebo všechny nástroj uvnitř Xcode spustit z příkazového řádku.
- **Aplikace Terminálové** -z terminálu aplikace, můžete nainstalovat spuštěním nástroje příkazového řádku `xcode-select --install` příkaz:
    - Spuštění aplikace terminálu.
    - Typ `xcode-select --install` a stiskněte klávesu **Enter**, například:

    ```bash
    Europa:~ kmullins$ xcode-select --install
    ```

    - Zobrazí se výzva k instalaci nástroje příkazového řádku, klikněte na tlačítko **nainstalovat** tlačítko: [ ![ ] (walkthrough-images/xcode01.png "instalaci nástrojů příkazového řádku")](walkthrough-images/xcode01.png#lightbox)

    - Nástroje bude možné stáhnout a nainstalovat z servery společnosti Apple: [ ![ ] (walkthrough-images/xcode02.png "stahování nástroje")](walkthrough-images/xcode02.png#lightbox)

- **Soubory ke stažení pro vývojáře Apple** -balíček nástroje pro příkazový řádek je k dispozici [soubory ke stažení pro vývojáře Apple]() webové stránky. Přihlaste se pomocí Apple ID, potom vyhledat a stáhnout nástroje příkazového řádku: [ ![ ] (walkthrough-images/xcode03.png "hledání nástroje příkazového řádku")](walkthrough-images/xcode03.png#lightbox)

Pomocí nástroje příkazového řádku nainstalované jsme připraveni pokračovat v návodu.

## <a name="walkthrough"></a>Návod

V tomto návodu nabídneme následující kroky:

- **[Vytvořte statické knihovny](#Creating_A_Static_Library)**  – tento krok zahrnuje vytvoření statické knihovny **InfColorPicker** kód jazyka Objective-C. Statické knihovny bude mít `.a` příponu souboru a budou vloženy do sestavení .NET projektu knihovny.
- **[Vytvoření vazby projektu Xamarin.iOS](#Create_a_Xamarin.iOS_Binding_Project)**  – když máme statickou knihovnu, budeme ji používat k vytvoření vazby projektu Xamarin.iOS. Vazba projektu se skládá z statickou knihovnu, kterou jsme právě vytvořili a metadata ve formě kód C#, která vysvětluje, jak můžete použít rozhraní API jazyka Objective-C. Tato metadata se obvykle označuje jako definice rozhraní API. Použijeme  **[cíl Sharpie](#Using_Objective_Sharpie)**  a pomoci tak s vytvoření definice rozhraní API.
- **[Normalizuje definice rozhraní API](#Normalize_the_API_Definitions)**  – cíl Sharpie nemá úlohu skvělé nám pomoci, ale nemůže dělat všechno, co. Probereme některé změny, které je potřeba před použitím provést definice rozhraní API.
- **[Použití knihovny vazby](#Using_the_Binding)**  – nakonec vytvoříme aplikace pro Xamarin.iOS k ukazují, jak používat naše projektu nově vytvořená vazba.

Teď, když jsme pochopit, jaké kroky se podílejí, můžeme přesunout k rest návodu.

<a name="Creating_A_Static_Library"/>

## <a name="creating-a-static-library"></a>Vytvoření statické knihovny

Pokud jsme zkontrolovat kód pro InfColorPicker v Githubu:

[![](walkthrough-images/image02.png "Zkontrolujte kód pro InfColorPicker v Githubu")](walkthrough-images/image02.png#lightbox)

Vidíme následující tři adresáře v projektu:

- **InfColorPicker** -tento adresář obsahuje kód jazyka Objective-C pro projekt.
- **PickerSamplePad** -tento adresář obsahuje ukázkový iPad projekt.
- **PickerSamplePhone** -tento adresář obsahuje ukázkový iPhone projekt.

Umožňuje stáhnout projektu InfColorPicker z [Githubu](https://github.com/InfinitApps/InfColorPicker/archive/master.zip) a rozbalte ho v adresáři naše výběru. Otevírání až Xcode cíle pro `PickerSamplePhone` projektu, vidíte následující strukturu projektu Xcode Navigátor:

[![](walkthrough-images/image03.png "Struktura projektu Xcode Navigátor")](walkthrough-images/image03.png#lightbox)

Tento projekt dosahuje opakované použití kódu přímo InfColorPicker zdrojový kód (v červeným rámečkem) přidáte do každé ukázkový projekt. Kód pro ukázkový projekt je uvnitř blue pole. Protože tato konkrétní projekt neposkytuje nám s statickou knihovnu, je nutné pro nás vytvoření projektu Xcode kompilace se statickou knihovnou.

Prvním krokem je pro nás přidání InfoColorPicker zdrojový kód do se statickou knihovnou. Umožňuje dosáhnout postupujte takto:

1. Spusťte Xcode.
2. Z **soubor** nabídky vyberte možnost **nový** > **projektu...** :

    [![](walkthrough-images/image04.png "Spuštění nového projektu")](walkthrough-images/image04.png#lightbox)
3. Vyberte **Framework & knihovny**, **kakao Touch statické knihovny** šablonu a klikněte na **Další** tlačítko:

    [![](walkthrough-images/image05.png "Vyberte šablonu, kakao Touch statické knihovny")](walkthrough-images/image05.png#lightbox)
4. Zadejte `InfColorPicker` pro **název projektu** a klikněte na **Další** tlačítko:

    [![](walkthrough-images/image06.png "Zadejte název projektu InfColorPicker")](walkthrough-images/image06.png#lightbox)
5. Vyberte umístění pro uložení projektu a klikněte na **OK** tlačítko.
6. Teď je potřeba přidat zdroj z projektu InfColorPicker naše projektu statické knihovny. Protože **InfColorPicker.h** soubor již existuje v našem statické knihovny (ve výchozím nastavení), Xcode nebude povolovat nám ji přepsat. Z **vyhledávací**, přejděte na zdrojový kód InfColorPicker v původní projekt, který jsme rozbalené z Githubu, všechny soubory InfColorPicker zkopírujte a vložte je do našich nového projektu statické knihovny:

    [![](walkthrough-images/image12.png "Zkopírujte všechny soubory InfColorPicker")](walkthrough-images/image12.png#lightbox)

7. Vrátit do Xcode, klikněte pravým tlačítkem na **InfColorPicker** složky a vyberte **přidat soubory do "InfColorPicker..."**:

    [![](walkthrough-images/image08.png "Přidávání souborů")](walkthrough-images/image08.png#lightbox)

8. V dialogovém okně Přidat soubory přejděte na soubory InfColorPicker zdrojového kódu, které jsme právě zkopírovali, vyberte všechny a klikněte **přidat** tlačítko:

    [![](walkthrough-images/image09.png "Vyberte všechny a klikněte na tlačítko Přidat")](walkthrough-images/image09.png#lightbox)

9. Zdrojový kód se zkopírují do našich projektu:

    [![](walkthrough-images/image10.png "Zdrojový kód se zkopírují do projektu")](walkthrough-images/image10.png#lightbox)

10. Navigátor projektu Xcode, vyberte **InfColorPicker.m** souborů a komentář poslední dva řádky (kvůli způsobu, byla zapsána této knihovny, tento soubor není používán):

    [![](walkthrough-images/image14.png "Úpravy souboru InfColorPicker.m")](walkthrough-images/image14.png#lightbox)

11. Nyní potřebujeme zkontrolujte, zda jsou všechny rozhraní, které vyžadují knihovny. Tyto informace můžete najít v souboru README nebo otevřením jednoho z ukázkových projektů zadat. Tento příklad používá `Foundation.framework`, `UIKit.framework`, a `CoreGraphics.framework` tak přidejme je.

12. Vyberte **InfColorPicker cíl > fáze buildu** a rozbalte **odkaz binárních souborů a knihoven** části:

    [![](walkthrough-images/image16b.png "Rozbalte část odkaz binárních souborů a knihoven")](walkthrough-images/image16b.png#lightbox)

13. Použití  **+**  tlačítko otevřete dialogové okno umožňuje přidat požadované rámce architektury uvedené výše:

    [![](walkthrough-images/image16c.png "Přidejte že požadované rámce architektury uvedené výše")](walkthrough-images/image16c.png#lightbox)

14. **Odkaz binárních souborů a knihoven** části by teď měl vypadat jako obrázek níže:

    [![](walkthrough-images/image16d.png "V části propojení binárních souborů a knihoven")](walkthrough-images/image16d.png#lightbox)

V tuto chvíli jsme zavřít, ale máme není poměrně Hotovo. Vytvořil se statickou knihovnou, ale je třeba vytvořit k tvorbě Fat binární, který obsahuje všechny požadované architektury pro zařízení s iOS a simulátoru iOS.

### <a name="creating-a-fat-binary"></a>Vytváření Fat binární

Všechna zařízení s iOS mají procesory s technologií architektura ARM, které mají postupného vývoje. Každé nové architektury přidat nové pokyny a dalších vylepšení současně se zachovává zpětné kompatibility. Na zařízeních s iOS máme armv6, armv7, armv7s, nastaví instrukce arm64 – i když [používáme už armv6](~/ios/deploy-test/compiling-for-different-devices.md). Simulátoru iOS není používá technologii ARM a je istead x86 a simulátoru x86_64 zapnuté. Jsme, které znamená, že pro nás je, že jsme musí poskytnout knihovny na každou instrukci nastaví.

Se systémem souborů Fat knihovna `.a` souboru, který obsahuje všechny podporované architektury.

Vytvoření fat binární je třech krocích:

- Zkompilujte ARM 7 & ARM64 verzi se statickou knihovnou.
- Zkompilujte x86 a x84_64 verzi se statickou knihovnou.
- Použití `lipo` nástroj příkazového řádku sloučit dva statické knihovny do jednoho.

Když jsou tyto tři kroky místo jednoduché a může být nutné je opakovat v budoucnu, když knihovna jazyka Objective-C získává aktualizace, nebo pokud je nutné opravy chyb. Pokud se rozhodnete automatizovat tyto kroky, zjednoduší budoucí údržby a podpory vazby projektu iOS.

Existují celou řadu nástrojů, které jsou k dispozici pro automatizaci takových úloh - skript prostředí, [převislým](http://rake.rubyforge.org/), [xbuild](http://www.mono-project.com/Microsoft.Build), a [zkontrolujte](https://developer.apple.com/library/mac/documentation/Darwin/Reference/ManPages/man1/make.1.html). Když jsme nainstalovány nástroje pro příkazový řádek Xcode, jsme nainstalovali také zkontrolujte, tak, že se systému, který se použije v tomto návodu. Tady je **Makefile** , můžete použít k vytvoření více architektura sdílenou knihovnu, která bude fungovat na zařízení s iOS a simulátor pro všechny knihovny:

```bash
XBUILD=/Applications/Xcode.app/Contents/Developer/usr/bin/xcodebuild
PROJECT_ROOT=./YOUR-PROJECT-NAME
PROJECT=$(PROJECT_ROOT)/YOUR-PROJECT-NAME.xcodeproj
TARGET=YOUR-PROJECT-NAME

all: lib$(TARGET).a

lib$(TARGET)-i386.a:
    $(XBUILD) -project $(PROJECT) -target $(TARGET) -sdk iphonesimulator -configuration Release clean build
    -mv $(PROJECT_ROOT)/build/Release-iphonesimulator/lib$(TARGET).a $@

lib$(TARGET)-armv7.a:
    $(XBUILD) -project $(PROJECT) -target $(TARGET) -sdk iphoneos -arch armv7 -configuration Release clean build
    -mv $(PROJECT_ROOT)/build/Release-iphoneos/lib$(TARGET).a $@

lib$(TARGET)-arm64.a:
    $(XBUILD) -project $(PROJECT) -target $(TARGET) -sdk iphoneos -arch arm64 -configuration Release clean build
    -mv $(PROJECT_ROOT)/build/Release-iphoneos/lib$(TARGET).a $@

lib$(TARGET).a: lib$(TARGET)-i386.a lib$(TARGET)-armv7.a lib$(TARGET)-arm64.a
    xcrun -sdk iphoneos lipo -create -output $@ $^

clean:
    -rm -f *.a *.dll
```

Zadejte **Makefile** příkazů v editoru prostý text dle vlastního výběru a aktualizovat oddíly s **název projektu YOUR** s názvem projektu. Je také důležité zajistit, aby jsme vložte pokyny výše, zachována na karty v rámci pokynů.

Uložte soubor s názvem **Makefile** do stejného umístění jako statické knihovny InfColorPicker Xcode jsme vytvořili výše:

[![](walkthrough-images/lib00.png "Uložte soubor s názvem souboru pravidel")](walkthrough-images/lib00.png#lightbox)

Otevřete terminál aplikace na počítači Mac a přejděte do umístění vašeho souboru pravidel. Typ `make` do terminálu, stiskněte klávesu **Enter** a **Makefile** budou spuštěny:

[![](walkthrough-images/lib01.png "Ukázkový výstup souboru pravidel")](walkthrough-images/lib01.png#lightbox)

Když spustíte Ujistěte se, zobrazí se spoustu text posouvání pomocí. Pokud všechno fungovala správně, uvidíte slova **sestavení bylo úspěšné** a `libInfColorPicker-armv7.a`, `libInfColorPicker-i386.a` a `libInfColorPickerSDK.a` soubory budou zkopírovány do stejného umístění, jako **Makefile**:

[![](walkthrough-images/lib02.png "LibInfColorPicker armv7.a, libInfColorPicker i386.a a libInfColorPickerSDK.a soubory generované souboru pravidel")](walkthrough-images/lib02.png#lightbox)

Architekturách v rámci vaší Fat binární si můžete ověřit pomocí následujícího příkazu:

```bash
xcrun -sdk iphoneos lipo -info libInfColorPicker.a
```

To by měl zobrazit následující:

```bash
Architectures in the fat file: libInfColorPicker.a are: i386 armv7 x86_64 arm64
```

V tomto okamžiku prvním krokem naše iOS vazby po dokončení vytvořením statické knihovny pomocí Xcode a nástroje příkazového řádku Xcode `make` a `lipo`. Můžeme přesunout k dalšímu kroku a používat **cíl Sharpie** k automatizaci vytváření vazeb rozhraní API pro nás.

<a name="Create_a_Xamarin.iOS_Binding_Project"/>

## <a name="create-a-xamarinios-binding-project"></a>Vytvoření vazby projektu Xamarin.iOS

Před můžeme použít **cíl Sharpie** automatizovat proces vytváření vazby, potřebujeme k vytvoření vazby projektu Xamarin.iOS určený definice rozhraní API (který budeme používat **cíl Sharpie** nám pomáhají sestavení) a vytvořte vazbu C# pro nás.

Pojďme postupujte takto:

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

1. Start Visual Studio for Mac.
1. Z **soubor** nabídce vyberte možnost **nový** > **řešení...** :

    ![](walkthrough-images/bind01.png "Spuštění nové řešení")

1. V dialogovém okně nové řešení vyberte **knihovny** > **iOS vazby projektu**:

    ![](walkthrough-images/bind02.png "Vyberte iOS vazby projektu")

1. Klikněte **Další** tlačítko.

1. Zadejte "InfColorPickerBinding" jako **název projektu** a klikněte na tlačítko **vytvořit** tlačítko pro vytvoření řešení:

    ![](walkthrough-images/bind02a.png "Jako název projektu zadejte InfColorPickerBinding")

Vytvoří se řešení a dvě výchozí soubory budou zahrnuty:

![](walkthrough-images/bind03.png "Struktura řešení v Průzkumníku řešení")


# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)


1. Spuštění sady Visual Studio.

1. Z **soubor** nabídce vyberte možnost **nový** > **projektu...** :

    ![](walkthrough-images/bind01vs.png "Spuštění nového projektu")

1. Z dialogového okna Nový projekt, vyberte **iOS** > **vazby knihovny**:

    ![](walkthrough-images/bind02vs.png "Vyberte iOS vazby knihovny")

1. Zadejte "InfColorPickerBinding" jako **název** a klikněte na tlačítko **OK** tlačítko pro vytvoření řešení.

Vytvoří se řešení a dvě výchozí soubory budou zahrnuty:

![](walkthrough-images/bind03vs.png "Struktura řešení v Průzkumníku řešení")

-----



- **ApiDefinition.cs** – tento soubor bude obsahovat smluv, které definují, jak budou API jazyka Objective-C zabaleny v C#.
- **Structs.cs** – tento soubor bude obsahovat všechny struktury nebo výčtové hodnoty, které jsou vyžadované rozhraní a delegáti.

Jsme budete pracovat se tyto dva soubory později v tomto návodu. Nejprve musíme knihovně InfColorPicker přidejte do projektu vazby.

### <a name="including-the-static-library-in-the-binding-project"></a>Včetně se statickou knihovnou v projektu vazby

Teď máme naše základní vazby projektu připraven, je potřeba přidat Fat binární knihovny jsme vytvořili výše pro **InfColorPicker** knihovny.

Použijte následující postup přidání knihovny:

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

1. Klikněte pravým tlačítkem na **nativní odkazy** složky v řešení pro a vyberte **přidat nativní odkazy**:

    ![](walkthrough-images/bind04a.png "Přidat nativní odkazy")

1. Přejděte na Fat binární jsme provedli dříve (`libInfColorPickerSDK.a`) a stiskněte klávesu **otevřete** tlačítko:

    ![](walkthrough-images/bind05.png "Vyberte soubor libInfColorPickerSDK.a")
1. Soubor bude zahrnutý v projektu:

    ![](walkthrough-images/bind04.png "Včetně souboru")

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

1. Kopírování `libInfColorPickerSDK.a` z vaší **Mac sestavení hostitele** a vložte ji do projektu vazby.

1. Klikněte pravým tlačítkem na projekt a zvolte **Přidat > existující položka...** :

    ![](walkthrough-images/bind04vs.png "Přidání existující soubor")

1. Přejděte na `libInfColorPickerSDK.a` a stiskněte klávesu **přidat** tlačítko:

    ![](walkthrough-images/bind05vs.png "Adding libInfColorPickerSDK.a")

1. Soubor bude zahrnutý v projektu.

-----


Když soubor .a se přidá do projektu, Xamarin.iOS automaticky nastaví **akce sestavení** souboru, který se **ObjcBindingNativeLibrary**a vytvořte speciální soubor s názvem `libInfColorPickerSDK.linkwith.cs`.


Tento soubor obsahuje `LinkWith` atribut, který sděluje Xamarin.iOS jak popisovač statickou knihovnu, kterou jsme právě přidali. Obsah tohoto souboru jsou uvedeny v následující fragment kódu:

```csharp
using ObjCRuntime;

[assembly: LinkWith ("libInfColorPickerSDK.a", SmartLink = true, ForceLoad = true)]
```

`LinkWith` Atribut určuje se statickou knihovnou pro projekt a některé příznaky linkeru důležité.


Další krok, který je potřeba udělat je k vytvoření definice rozhraní API pro projekt InfColorPicker. Pro účely tohoto návodu použijeme Sharpie cíl pro generování souboru **ApiDefinition.cs**.

<a name="Using_Objective_Sharpie"/>

## <a name="using-objective-sharpie"></a>Pomocí cíle Sharpie

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)


Cíle Sharpie je příkazového řádku nástroje (poskytované Xamarin), který může být užitečné při vytváření definice potřebné k vytvoření vazby. 3. stran jazyka Objective-C knihovny jazyka C#. V této části, použijeme Sharpie cíl pro vytvoření počáteční **ApiDefinition.cs** InfColorPicker projektu.


# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)


Cíle Sharpie je příkazového řádku nástroje (poskytované Xamarin), který může být užitečné při vytváření definice potřebné k vytvoření vazby. 3. stran jazyka Objective-C knihovny jazyka C#. V této části, použijeme Sharpie cíl na našem **Mac sestavení hostitele** vytvořit počáteční **ApiDefinition.cs** InfColorPicker projektu.


-----

Abyste mohli začít, budeme stáhnout soubor Instalační služby systému Sharpie cíl popsaných v tomto [průvodce](~/cross-platform/macios/binding/objective-sharpie/get-started.md#installing). Spusťte instalační program a všechny výzvy, na obrazovce podle pokynů Průvodce instalací nainstalujte cíl Sharpie na našem vývojovém počítači.

Po máme cíl Sharpie úspěšně nainstalovaná, můžeme spustit aplikaci Terminálové a zadejte následující příkaz k získání nápovědy na všechny nástroje, které poskytuje pomoct vazby:

```bash
sharpie -help
```

Pokud jsme spustit příkaz výše, vygeneruje se následující výstup:

```bash
Europa:Resources kmullins$ sharpie -help
usage: sharpie [OPTIONS] TOOL [TOOL_OPTIONS]

Options:
  -h, --helpShow detailed help
  -v, --versionShow version information

Available Tools:

  xcode    Get information about Xcode installations and available SDKs.

  bind     Create a Xamarin C# binding to Objective-C APIs
Europa:Resources kmullins$
```

Pro účely tohoto návodu použijeme tyto nástroje Sharpie cíl:

- **xcode** – tento nám nástrojů poskytuje informace o našich aktuální instalaci Xcode a verze iOS a Mac rozhraní API, které jsme nainstalovali. Použijeme tyto informace později při se vygeneruje naše vazby.
- **Vytvoření vazby** -použijeme tento nástroj se analyzovat **.h** soubory v projektu InfColorPicker do počáteční **ApiDefinition.cs** a **StructsAndEnums.cs** soubory.

Potřebujete pomoc na konkrétní nástroj Sharpie cíl, zadejte název tohoto nástroje a `-help` možnost. Například `sharpie xcode -help` vrátí následující výstup:

```bash
Europa:Resources kmullins$ sharpie xcode -help
usage: sharpie xcode [OPTIONS]+

Options:
  -h, --help                 Show detailed help
  -v, --verbose              Be verbose with output
      --sdks                 List all available Xcode SDKs. Pass -verbose for
                               more details.
Europa:Resources kmullins$
```

Proces vytváření vazby můžeme začít, potřebujeme získat informace o našich aktuální nainstalovaných sadách SDK zadáním následujícího příkazu do terminálu `sharpie xcode -sdks`:

```bash
amyb:Desktop amyb$ sharpie xcode -sdks
sdk: appletvos9.2    arch: arm64
sdk: iphoneos9.3     arch: arm64   armv7
sdk: macosx10.11     arch: x86_64  i386
sdk: watchos2.2      arch: armv7
```

Z výše uvedeného vidíme, že máme `iphoneos8.1` SDK v našem počítači nainstalován. Tyto informace na místě a jsme připraveni analyzovat projektu InfColorPicker `.h` soubory do počáteční **ApiDefinition.cs** a `StructsAndEnums.cs` InfColorPicker projektu.

Zadejte následující příkaz v terminálu aplikace:

```bash
sharpie bind --output=InfColorPicker --namespace=InfColorPicker --sdk=[iphone-os] [full-path-to-project]/InfColorPicker/InfColorPicker/*.h
```

Kde `[full-path-to-project]` je úplná cesta k adresáři kde **InfColorPicker** soubor projektu Xcode je umístěn v našem počítači a [iphone-os] je sada SDK, které jsme nainstalovali, iOS, protože označeny `sharpie xcode -sdks` příkaz. Všimněte si, že v tomto příkladu jsme jste úspěšně  **\*.h** jako parametr, který zahrnuje *všechny* soubory hlaviček v tomto adresáři - normálně jste měli tuto činnost, ale místo toho pečlivě přečtěte soubory hlaviček najít nejvyšší úrovně **.h** souboru, který odkazuje na všechny ostatní příslušné soubory a, stačí předat do Sharpie cíl.

Následující [výstup](walkthrough-images/os05.png) vygeneruje v terminálu:

```bash
Europa:Resources kmullins$ sharpie bind -output InfColorPicker -namespace InfColorPicker -sdk iphoneos8.1 /Users/kmullins/Projects/InfColorPicker/InfColorPicker/InfColorPicker.h -unified
Compiler configuration:
    -isysroot /Applications/Xcode.app/Contents/Developer/Platforms/iPhoneOS.platform/Developer/SDKs/iPhoneOS.sdk -miphoneos-version-min=8.1 -resource-dir /Library/Frameworks/ObjectiveSharpie.framework/Versions/1.1.1/clang-resources -arch armv7 -ObjC

[  0%] parsing /Users/kmullins/Projects/InfColorPicker/InfColorPicker/InfColorPicker.h
In file included from /Users/kmullins/Projects/InfColorPicker/InfColorPicker/InfColorPicker.h:60:
/Users/kmullins/Projects/InfColorPicker/InfColorPicker/InfColorPickerController.h:28:1: warning: no 'assign',
      'retain', or 'copy' attribute is specified - 'assign' is assumed [-Wobjc-property-no-attribute]
@property (nonatomic) UIColor* sourceColor;
^
/Users/kmullins/Projects/InfColorPicker/InfColorPicker/InfColorPickerController.h:28:1: warning: default property
      attribute 'assign' not appropriate for non-GC object [-Wobjc-property-no-attribute]
/Users/kmullins/Projects/InfColorPicker/InfColorPicker/InfColorPickerController.h:29:1: warning: no 'assign',
      'retain', or 'copy' attribute is specified - 'assign' is assumed [-Wobjc-property-no-attribute]
@property (nonatomic) UIColor* resultColor;
^
/Users/kmullins/Projects/InfColorPicker/InfColorPicker/InfColorPickerController.h:29:1: warning: default property
      attribute 'assign' not appropriate for non-GC object [-Wobjc-property-no-attribute]
4 warnings generated.
[100%] parsing complete
[bind] InfColorPicker.cs
Europa:Resources kmullins$
```

A **InfColorPicker.enums.cs** a **InfColorPicker.cs** soubory budou vytvořeny v našem adresáře:

[![](walkthrough-images/os06.png "Soubory InfColorPicker.enums.cs a InfColorPicker.cs")](walkthrough-images/os06.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)


Oba tyto soubory otevřete v projektu vazby, který jsme vytvořili výše. Zkopírujte obsah **InfColorPicker.cs** soubor a vložte ji do **ApiDefinition.cs** souboru, nahraďte existující `namespace ...` blok kódu s obsahem  **InfColorPicker.cs** souboru (a `using` příkazy beze změn):

![](walkthrough-images/os07.png "Soubor InfColorPickerControllerDelegate")


# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)


Oba tyto soubory otevřete v projektu vazby, který jsme vytvořili výše. Zkopírujte obsah **InfColorPicker.cs** souboru (z **Mac sestavení hostitele**) a vložte ji do **ApiDefinition.cs** souboru, nahraďte existující `namespace ...` blok kódu s obsahem **InfColorPicker.cs** souboru (a `using` příkazy beze změn).


-----

<a name="Normalize_the_API_Definitions"/>

## <a name="normalize-the-api-definitions"></a>Normalizuje definice rozhraní API

Cíle Sharpie někdy má překladu problém `Delegates`, takže je potřeba upravit definici `InfColorPickerControllerDelegate` rozhraní a nahraďte `[Protocol, Model]` řádek následujícím kódem:

```csharp
[BaseType(typeof(NSObject))]
[Model]
```
Tak, aby definice vypadá takto:

[![](walkthrough-images/os11.png "Definice")](walkthrough-images/os11.png#lightbox)

V dalším kroku provedeme samé s obsahem `InfColorPicker.enums.cs` souboru, kopírování a vkládání, je `StructsAndEnums.cs` souboru ponechat `using` příkazy beze změn:

[![](walkthrough-images/os09.png "Obsah StructsAndEnums.cs souboru ")](walkthrough-images/os09.png#lightbox)

Také je možné, že cíl Sharpie má poznámkou vazba s `[Verify]` atributy. Tyto atributy znamenat, že ověřte, že cíl Sharpie nebyla správnou věc tak, že porovnáte vazba s původní deklaraci jazyka C nebo Objective-C (která bude k dispozici v komentář nad deklaraci vázané). Jakmile si ověříte vazby, byste měli odebrat ověřte atribut. Další informace najdete v části [ověřte](~/cross-platform/macios/binding/objective-sharpie/platform/verify.md) průvodce.

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)


V tomto okamžiku naše vazby projektu musí být úplný a Průvodce je připraven vytvořit. Pojďme naše vazby projekt sestavil a ujistěte se, že jsme skončila bez chyb:

[Sestavení projektu vazby a ujistěte se, že nejsou žádné chyby](walkthrough-images/os12.png)


# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)


V tomto okamžiku naše vazby projektu musí být úplný a Průvodce je připraven vytvořit. Pojďme naše vazby projekt sestavil a ujistěte se, že jsme skončila bez chyb.


-----

<a name="Using_the_Binding"/>

## <a name="using-the-binding"></a>Pomocí vazby

Postupujte podle těchto kroků k vytvoření ukázkové aplikace iPhone používat iOS, které knihovna vazby vytvořili výše:

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

1. **Vytvoření projektu Xamarin.iOS** – přidání nového projektu Xamarin.iOS názvem **InfColorPickerSample** k řešení, jak je vidět na následujících snímcích obrazovky:

    ![](walkthrough-images/use01.png "Přidání jediné zobrazení aplikace")

    ![](walkthrough-images/use01a.png "Nastavení identifikátor")

1. **Přidat odkaz na projekt vazby** -aktualizace **InfColorPickerSample** tak, aby měl odkaz na projekt **InfColorPickerBinding** projektu:

    ![](walkthrough-images/use02.png "Přidat odkaz na projekt vazby")

1. **Vytvoření uživatelského rozhraní pro iPhone** -dvakrát klikněte na **MainStoryboard.storyboard** souboru v **InfColorPickerSample** projektu a upravit v iOS Designer. Přidat **tlačítko** k zobrazení a pojmenujte ji `ChangeColorButton`, jak je znázorněno v následujícím:

    ![](walkthrough-images/use03.png "Přidání tlačítka do zobrazení")
1. **Přidat InfColorPickerView.xib** -The InfColorPicker Objective-C knihovna obsahuje **.xib** souboru. Xamarin.iOS nebude obsahovat to **.xib** v projektu vazby, které způsobí chyby v našem ukázkové aplikaci. Alternativní řešení pro tuto je přidání **.xib** souboru do našich projektu Xamarin.iOS. Vyberte projektu Xamarin.iOS, klikněte pravým tlačítkem a vyberte **Přidat > Přidat soubory**a přidejte **.xib** souboru, jak je znázorněno na následujícím snímku obrazovky:

    ![](walkthrough-images/use04.png "Přidat InfColorPickerView.xib")

1. Když se zobrazí dotaz, zkopírujte **.xib** souboru do projektu.

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)


1. **Vytvoření projektu Xamarin.iOS** – přidání nového projektu Xamarin.iOS názvem **InfColorPickerSample** k řešení, jak je znázorněno na následujícím snímku obrazovky:

    ![](walkthrough-images/use01vs.png "Vytvoření projektu Xamarin.iOS")

1. **Přidat odkaz na projekt vazby** -aktualizace **InfColorPickerSample** tak, aby měl odkaz na projekt **InfColorPickerBinding** projektu:

    ![](walkthrough-images/use02vs.png "Přidat odkaz na projekt vazby")

1. **Vytvoření uživatelského rozhraní pro iPhone** -dvakrát klikněte na **MainStoryboard.storyboard** souboru v **InfColorPickerSample** projektu a upravit v iOS Designer. Přidat **tlačítko** k zobrazení a pojmenujte ji `ChangeColorButton`, jak je znázorněno v následujícím:

    ![](walkthrough-images/use03vs.png "Vytvoření uživatelského rozhraní pro iPhone")

1. **Přidat InfColorPickerView.xib** -The InfColorPicker Objective-C knihovna obsahuje **.xib** souboru. Xamarin.iOS nebude obsahovat to **.xib** v projektu vazby, které způsobí chyby v našem ukázkové aplikaci. Alternativní řešení pro tuto je přidání **.xib** souboru do našich projektu Xamarin.iOS z našich **Mac sestavení hostitele**. Vyberte projektu Xamarin.iOS, klikněte pravým tlačítkem a vyberte **přidat** > **existující položka...** a přidejte **.xib** souboru.


-----



Dále umožňuje rychlý prohlédněte si protokoly v Objective-C a jak jsme je zpracovávat v kódu jazyka C# a vazeb.

### <a name="protocols-and-xamarinios"></a>Protokoly a Xamarin.iOS

V Objective-C, což je protokol definuje metody (nebo zprávy), můžete použít v některých případech. Koncepčně jsou velmi podobné rozhraní v jazyce C#. Hlavní rozdíl mezi protokol jazyka Objective-C a rozhraní jazyka C# je, protokoly můžou mít volitelné metody - metody, které není nutné implementovat třídu. Používá jazyka Objective-C @optional – klíčové slovo slouží k označení, které metody jsou volitelné. Další informace o protokoly najdete v části [události, protokoly a delegáti](~/ios/app-fundamentals/delegates-protocols-and-events.md).

**InfColorPickerController** má jeden takový protokol vidět v následujícím fragmentu kódu:

```csharp
@protocol InfColorPickerControllerDelegate

@optional

- (void) colorPickerControllerDidFinish: (InfColorPickerController*) controller;
// This is only called when the color picker is presented modally.

- (void) colorPickerControllerDidChangeColor: (InfColorPickerController*) controller;

@end
```

Tento protokol je používán **InfColorPickerController** informovat klienty, že uživatel vybral novou barvu a že **InfColorPickerController** po dokončení. Cíle Sharpie mapují tento protokol, jak je znázorněno v následující fragment kódu:

```csharp
[BaseType(typeof(NSObject))]
[Model]
public partial interface InfColorPickerControllerDelegate {

    [Export ("colorPickerControllerDidFinish:")]
    void ColorPickerControllerDidFinish (InfColorPickerController controller);

    [Export ("colorPickerControllerDidChangeColor:")]
    void ColorPickerControllerDidChangeColor (InfColorPickerController controller);
}

```

Při kompilaci knihovně vazby Xamarin.iOS vytvoří abstraktní základní třída, která je volána `InfColorPickerControllerDelegate`, které toto rozhraní implementuje s virtuální metody.

Existují dva způsoby, že jsme toto rozhraní implementovat aplikace pro Xamarin.iOS:

- **Silná delegovat** -pomocí silné delegáta zahrnuje vytvoření třídy C#, která je podtřídou `InfColorPickerControllerDelegate` a přepíše příslušné metody. **InfColorPickerController** instance této třídy bude používat pro komunikaci s klienty.
- **Slabé delegáta** -slabé delegát je mírně odlišný postup, který zahrnuje vytvoření veřejná metoda u některé třídy (, jako `InfColorPickerSampleViewController`) a potom tuto metodu pro vystavení `InfColorPickerDelegate` protokolu prostřednictvím `Export` atribut.

Silné delegáti poskytovat technologii Intellisense, bezpečnost typů a lepší zapouzdření. Z těchto důvodů byste měli používat silné delegáti kde je to možné, místo slabé delegáta.

V tomto návodu se budeme zabývat obě tyto metody: nejdřív implementace silné delegáta a pak vysvětlením, jak implementovat slabé delegáta.

### <a name="implementing-a-strong-delegate"></a>Implementace silné delegáta

Dokončit aplikace Xamarin.iOS pomocí silné delegáta reagovat na `colorPickerControllerDidFinish:` zpráva:

**Podtřída InfColorPickerControllerDelegate** -přidejte novou třídu do projektu názvem `ColorSelectedDelegate`. Třída upravte tak, aby měl následující kód:

```csharp
using InfColorPickerBinding;
using UIKit;

namespace InfColorPickerSample
{
    public class ColorSelectedDelegate:InfColorPickerControllerDelegate
    {
        readonly UIViewController parent;

        public ColorSelectedDelegate (UIViewController parent)
        {
            this.parent = parent;
        }

        public override void ColorPickerControllerDidFinish (InfColorPickerController controller)
        {
            parent.View.BackgroundColor = controller.ResultColor;
            parent.DismissViewController (false, null);
        }
    }
}
```

Xamarin.iOS vytvoří vazbu delegáta jazyka Objective-C vytvořením abstraktní základní třída, která je volána `InfColorPickerControllerDelegate`. Podtřída tento typ a přepsání `ColorPickerControllerDidFinish` metoda pro přístup k hodnotě `ResultColor` vlastnost `InfColorPickerController`.

**Vytvoření instance ColorSelectedDelegate** -naše obslužné rutiny události bude nutné instanci `ColorSelectedDelegate` typ, který jsme vytvořili v předchozím kroku. Upravit třídy `InfColorPickerSampleViewController` a přidejte následující proměnnou instance do třídy:

```csharp
ColorSelectedDelegate selector;
```

**Inicializace proměnnou ColorSelectedDelegate** – Pokud chcete zajistit, aby `selector` je platná instance, aktualizujte metodu `ViewDidLoad` v `ViewController` tak, aby odpovídala následující fragment kódu:

```csharp
public override void ViewDidLoad ()
{
    base.ViewDidLoad ();
    ChangeColorButton.TouchUpInside += HandleTouchUpInsideWithStrongDelegate;
    selector = new ColorSelectedDelegate (this);
}
```
**Implementujte metodu HandleTouchUpInsideWithStrongDelegate** -další implementaci obslužné rutiny události pro když uživatel dotykem **ColorChangeButton**. Upravit `ViewController`a přidejte následující metodu:

```csharp
using InfColorPicker;
...

private void HandleTouchUpInsideWithStrongDelegate (object sender, EventArgs e)
{
    InfColorPickerController picker = InfColorPickerController.ColorPickerViewController();
    picker.Delegate = selector;
    picker.PresentModallyOverViewController (this);
}

```

Jsme nejprve získat instanci `InfColorPickerController` prostřednictvím statickou metodu a ujistěte se, instance vědět naše silné delegáta přes vlastnost `InfColorPickerController.Delegate`. Tato vlastnost automaticky vygeneroval pro nás Sharpie cíl. Nakonec říkáme `PresentModallyOverViewController` zobrazíte zobrazení `InfColorPickerSampleViewController.xib` tak, aby uživatel může vybrat barvu.

**Spusťte aplikaci** – v tomto okamžiku máme Hotovo se všemi kódu. Pokud spustíte aplikaci, byste měli změnit barvu pozadí `InfColorColorPickerSampleView` jak je vidět na následujících snímcích obrazovky:

[![](walkthrough-images/run01.png "Spuštění aplikace")](walkthrough-images/run01.png#lightbox)

Blahopřejeme! V tomto okamžiku jste úspěšně vytvořili a vázaný knihovna jazyka Objective-C pro použití aplikace pro Xamarin.iOS. Dále umožňuje další informace o použití slabé delegáti.

### <a name="implementing-a-weak-delegate"></a>Implementace slabé delegáta

Místo vytvoření podtřídy třídu vázaný na protokol jazyka Objective-C u konkrétního delegáta, Xamarin.iOS také umožňuje implementovat protokol metody do třídy, která je odvozena z `NSObject`, architekturu vaší metody s `ExportAttribute`a potom poskytnutí odpovídající selektorů. Pokud tuto metodu, přiřadíte instance do třídy `WeakDelegate` vlastnost místo na `Delegate` vlastnost. Slabé delegáta nabízí flexibilitu provést třídě delegáta hierarchii dědičnosti různých níže. Podíváme se, jak implementovat a použití delegáta slabé v naší aplikaci Xamarin.iOS.

**Vytvoření obslužné rutiny události pro TouchUpInside** -vytvoříme novou obslužnou rutinu události pro `TouchUpInside` událostí na tlačítko změnit barvu pozadí. Tato obslužná rutina naplní na stejný atribut role jako `HandleTouchUpInsideWithStrongDelegate` obslužná rutina, že jsme vytvořili v předchozí části, ale bude používat delegáta slabé místo silné delegáta. Upravit třídy `ViewController`a přidejte následující metodu:

```csharp
private void HandleTouchUpInsideWithWeakDelegate (object sender, EventArgs e)
{
    InfColorPickerController picker = InfColorPickerController.ColorPickerViewController();
    picker.WeakDelegate = this;
    picker.SourceColor = this.View.BackgroundColor;
    picker.PresentModallyOverViewController (this);
}
```
**Aktualizovat ViewDidLoad** – musí se nám změnit `ViewDidLoad` tak, aby používala obslužné rutiny události, který jsme právě vytvořili. Upravit `ViewController` a změňte `ViewDidLoad` tak, aby připomínaly následující fragment kódu:


```csharp
public override void ViewDidLoad ()
{
    base.ViewDidLoad ();
    ChangeColorButton.TouchUpInside += HandleTouchUpInsideWithWeakDelegate;
}

```

**Zpracování colorPickerControllerDidFinish: zpráva** – když `ViewController` je dokončena, iOS bude odesílat zprávy `colorPickerControllerDidFinish:` k `WeakDelegate`. Je potřeba vytvořit metodu C#, která dokáže zpracovat tuto zprávu. K tomu, jsme vytvoření metody C# a pak ho s adorn `ExportAttribute`. Upravit `ViewController`a přidejte následující metodu do třídy:

```csharp
[Export("colorPickerControllerDidFinish:")]
public void ColorPickerControllerDidFinish (InfColorPickerController controller)
{
    View.BackgroundColor = controller.ResultColor;
    DismissViewController (false, null);
}

```

Spusťte aplikaci. By se měl chovat teď přesně tak, jak předtím, ale používá delegáta slabé místo silné delegáta. Tento názorný postup v tomto okamžiku jste úspěšně dokončena. Teď byste měli mít představu o tom, jak vytvářet a využívat vazby projektu Xamarin.iOS.

## <a name="summary"></a>Souhrn

Tento článek projít procesem vytváření a používání vazby projektu Xamarin.iOS. Nejprve jsme probrali postup existující knihovny jazyka Objective-C kompilována statické knihovny. Potom jsme popsané postupy k vytvoření vazby projektu Xamarin.iOS a jak používat cíl Sharpie ke generování definice rozhraní API jazyka Objective-C knihovny. Jsme probrali aktualizaci a vylepšení generovaného definice rozhraní API, aby byly vhodné pro veřejnou spotřeby. Po dokončení vazby projektu Xamarin.iOS byla k použití této vazby v aplikaci Xamarin.iOS, se zaměřením na použití silné Delegáti a slabé delegáti jsme přesunout.

## <a name="related-links"></a>Související odkazy

- [Příklad vazby (ukázka)](https://developer.xamarin.com/samples/monotouch/InfColorPicker/)
- [Knihovny pro vytváření vazeb Objective-C](~/cross-platform/macios/binding/objective-c-libraries.md)
- [Podrobnosti o vazby](~/cross-platform/macios/binding/overview.md)
- [Vazby typů referenční příručka](~/cross-platform/macios/binding/binding-types-reference.md)
- [Xamarin pro vývojáře jazyka Objective-C](~/ios/get-started/objective-c-developers/index.md)
- [Pokyny k návrhu architektury](http://msdn.microsoft.com/en-us/library/ms229042.aspx)
