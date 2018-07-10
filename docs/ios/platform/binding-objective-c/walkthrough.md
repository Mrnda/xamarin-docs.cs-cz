---
title: 'Návod: Vytvoření vazby mezi knihovnou Objective-C pro iOS'
description: Tento článek poskytuje praktické návod k vytvoření vazby Xamarin.iOS pro existující knihovnou Objective-C, InfColorPicker. Vztahuje se na témata, jako je kompilování statickou knihovnou Objective-C, vytvoříte jejich vazbu a vazbu používá aplikace pro Xamarin.iOS.
ms.prod: xamarin
ms.assetid: D3F6FFA0-3C4B-4969-9B83-B6020B522F57
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 05/02/2017
ms.openlocfilehash: 8285a82920f0d95a88855c5257535048c6de41d5
ms.sourcegitcommit: ec50c626613f2f9af51a9f4a52781129bcbf3fcb
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/05/2018
ms.locfileid: "37854854"
---
# <a name="walkthrough-binding-an-ios-objective-c-library"></a>Návod: Vytvoření vazby mezi knihovnou Objective-C pro iOS

_Tento článek poskytuje praktické návod k vytvoření vazby Xamarin.iOS pro existující knihovnou Objective-C, InfColorPicker. Vztahuje se na témata, jako je kompilování statickou knihovnou Objective-C, vytvoříte jejich vazbu a vazbu používá aplikace pro Xamarin.iOS._

Při práci na iOS, může nastat případy, ve které chcete používat knihovnu třetí strany Objective-C. V takových situacích můžete použít Xamarin.iOS _vazby projektu_ k vytvoření [vazby C#](~/cross-platform/macios/binding/overview.md) , který vám umožní využívat knihovny aplikace Xamarin.iOS.

Obecně v ekosystému iOS najdete knihovny v 3 charakteristikami:

* Jako soubor předkompilované statické knihovny s `.a` rozšíření spolu s jeho záhlaví (.h souborů). Například [knihovna Google Analytics](https://developers.google.com/analytics/devguides/collection/ios/v3/sdk-download?hl=es#download_sdk)
* Jako předkompilované rozhraní. Toto je pouze složky obsahující statickou knihovnu, záhlaví a někdy další prostředky s `.framework` rozšíření. Například [knihovny AdMob od Googlu](https://developers.google.com/admob/ios/download).
* Jako jen soubory zdrojového kódu. Například knihovnu obsahující pouze `.m` a `.h` Objective C soubory.

V prvním a druhém scénáři již nebude předkompilované CocoaTouch statickou knihovnu, v tomto článku se zaměříme na třetí scénář. Mějte na paměti, než začnete, chcete-li vytvořit vazbu, vždy zkontrolujte licence k dispozici s knihovnou zajistit, že máte zdarma a vytvořte jeho vazbu.

Tento článek obsahuje podrobný postup vytvoření vazby projektu pomocí open source [InfColorPicker](https://github.com/InfinitApps/InfColorPicker) Objective-C projektu jako příklad, ale všechny informace v této příručce lze upravit a použít s žádným Objective-C knihovny třetí strany. Knihovna InfColorPicker obsahuje opakovaně použitelné zobrazení kontroler, který umožňuje uživateli vybrat barvu podle jeho reprezentace HSB provádění přívětivější výběr barvy.

[![](walkthrough-images/run01.png "Příklad InfColorPicker knihovna pro iOS")](walkthrough-images/run01.png#lightbox)

Všechny nezbytné kroky používat toto konkrétní rozhraní API jazyka Objective-C v Xamarin.iosu probereme:

- Nejprve vytvoříme statické knihovny Objective-C pomocí Xcode.
- Potom jsme budete vytvořit vazbu tento statické knihovny s Xamarin.iOS.
- V dalším kroku ukazují, jak Sharpie cíle se dá snížit zatížení automatického generování některé (ale ne všech) nutné definice rozhraní API vyžaduje vazbu Xamarin.iOS.
- A konečně My vytvoříme aplikaci Xamarin.iOS, která používá vazbu.

Ukázková aplikace ukazuje, jak používat silné delegáta pro komunikaci mezi InfColorPicker rozhraní API a náš kód jazyka C#. Po zaznamenali jsme na tom, jak používat silné delegáta, budeme zabývat použití slabé delegáty k provádění stejných úloh.

## <a name="requirements"></a>Požadavky

Tento článek předpokládá, že máte některé znalost Xcode a jazyka Objective-C a jste si přečetli naše [vazeb Objective-C](~/cross-platform/macios/binding/index.md) dokumentaci. Kromě toho je nutné provést následující kroky uvedené:

-  **Xcode a sadu iOS SDK** -Xcode od společnosti Apple a nejnovější rozhraní API pro iOS musí být nainstalovaná a nakonfigurovaná v počítači vývojáře.
-  **[Nástroje příkazového řádku Xcode](#Installing_the_Xcode_Command_Line_Tools)**  – příkazového řádku nástroje Xcode, musí být nainstalována pro aktuálně nainstalovanou verzi Xcode (dole najdete podrobné informace o instalaci).
-  **Visual Studio pro Mac nebo Visual Studio** – nejnovější verzi sady Visual Studio pro Mac nebo Visual Studio by měl nainstalovaný a nakonfigurovaný na vývojovém počítači. Apple Mac, je třeba vyvíjet aplikace pro Xamarin.iOS a při použití sady Visual Studio musíte být připojeni k [Xamarin.iOS build host](~/ios/get-started/installation/windows/connecting-to-mac/index.md)
-  **Nejnovější verzi cíle Sharpie** -aktuální kopii cíle Sharpie nástroj stáhnout z [tady](~/cross-platform/macios/binding/objective-sharpie/get-started.md). Pokud už máte cíle Sharpie nainstalovaný, je můžete aktualizovat na nejnovější verzi pomocí `sharpie update`

<a name="Installing_the_Xcode_Command_Line_Tools"/>

## <a name="installing-the-xcode-command-line-tools"></a>Instalace nástroje Xcode příkazového řádku

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)


Jak je uvedeno výše, budeme používat nástroje příkazového řádku Xcode (konkrétně `make` a `lipo`) v tomto názorném postupu. `make` Příkaz je velmi běžné Unix nástroj, který automatizuje sestavování spustitelných programů a knihoven pomocí _makefile_ , která určuje, jak by měly být sestaveny program. `lipo` Příkaz je nástroj příkazového řádku systému OS X pro vytváření architektury více souborů, se budou kombinovat více `.a` soubory do jednoho souboru, který mohou využívat všechny architektury hardwaru.


# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)


Jak je uvedeno výše, budeme příkazového řádku nástroje Xcode používat na **hostitele sestavovací Mac** (konkrétně `make` a `lipo`) v tomto názorném postupu. `make` Příkaz je velmi běžné Unix nástroj, který automatizuje sestavování spustitelných programů a knihoven pomocí _makefile_ k Určuje, jak sestavit program. `lipo` Příkaz je nástroj příkazového řádku systému OS X pro vytváření architektury více souborů, se budou kombinovat více `.a` soubory do jednoho souboru, který mohou využívat všechny architektury hardwaru.


-----

Podle společnosti Apple [sestavení z příkazového řádku pomocí Xcode nejčastější dotazy k](https://developer.apple.com/library/ios/technotes/tn2339/_index.html) dokumentaci v OS X 10.9 nebo novější, **stáhne** podokně Xcode **Předvolby** dialogové okno není nadále podporuje stahování nástroje příkazového řádku.

Budete muset použít jednu z následujících metod k instalaci nástrojů:

- **Nainstalovat Xcode** – při instalaci Xcode, dodává jako součást balíčku s všechny nástroje příkazového řádku. V OS X 10.9 překrytí (nainstalované v `/usr/bin`), lze mapovat libovolný nástroj součástí `/usr/bin` odpovídající nástroje v Xcode. Například `xcrun` příkaz, který umožňuje najít nebo z příkazového řádku spustit libovolný nástroj v Xcode.
- **Aplikaci terminál** – z terminálu aplikace, můžete nainstalovat spuštěním nástroje příkazového řádku `xcode-select --install` příkaz:
    - Spusťte aplikaci terminálu.
    - Typ `xcode-select --install` a stiskněte klávesu **Enter**, například:

    ```bash
    Europa:~ kmullins$ xcode-select --install
    ```

    - Budete vyzváni k instalaci nástrojů příkazového řádku, klikněte na tlačítko **nainstalovat** tlačítko: [ ![ ] (walkthrough-images/xcode01.png "instalace nástrojů příkazového řádku")](walkthrough-images/xcode01.png#lightbox)

    - Nástroje se stáhnout a nainstalovat ze serverů společnosti Apple: [ ![ ] (walkthrough-images/xcode02.png "stahování nástroje")](walkthrough-images/xcode02.png#lightbox)

- **Soubory ke stažení pro vývojáře Apple** – balíček nástroje pro příkazový řádek je k dispozici [soubory ke stažení pro vývojáře Apple]() webové stránky. Přihlaste se pomocí Apple ID, vyhledejte a stáhněte si nástroje příkazového řádku: [ ![ ] (walkthrough-images/xcode03.png "hledání nástrojů příkazového řádku")](walkthrough-images/xcode03.png#lightbox)

Pomocí nástrojů příkazového řádku nainstalovat jsme připraveni pokračovat v návodu.

## <a name="walkthrough"></a>Názorný postup

V tomto podrobném návodu se budeme zabývat následující kroky:

- **[Vytvoření statické knihovny](#Creating_A_Static_Library)**  – tento krok zahrnuje vytvoření statické knihovny **InfColorPicker** kód Objective-C. Statická knihovna bude mít `.a` příponu souboru a bude vložen do sestavení .NET projektu knihovny.
- **[Vytvoření vazby projektu Xamarin.iOS](#Create_a_Xamarin.iOS_Binding_Project)**  – Jakmile budeme mít statickou knihovnu, použijeme ji k vytvoření vazby projektu Xamarin.iOS. Vazba projekt obsahuje statickou knihovnu, kterou jsme právě vytvořili a metadat ve formě kód jazyka C#, která vysvětluje, jak je možné rozhraní API jazyka Objective-C. Tato metadata se obvykle označuje jako definice rozhraní API. Použijeme **[cíle Sharpie](#Using_Objective_Sharpie)** nám s vytvořit definice rozhraní API.
- **[Normalizovat definice rozhraní API](#Normalize_the_API_Definitions)**  – cíl Sharpie nemá Skvělá práce nám pomoci, ale nemůže provádět vše. Probereme některé změny, které potřebujeme před použitím provedli definice rozhraní API.
- **[Použít knihovnu vazby](#Using_the_Binding)**  – nakonec vytvoříme aplikace Xamarin.iOS, která ukazují, jak použít náš projekt nově vytvořená vazba.

Teď, když jsme pochopit, jaké kroky souvisejí, přejdeme zbytek návodu.

<a name="Creating_A_Static_Library"/>

## <a name="creating-a-static-library"></a>Vytvoření statické knihovny

Pokud jsme prohlédnout kód pro InfColorPicker v Githubu:

[![](walkthrough-images/image02.png "Prozkoumejte kód pro InfColorPicker v Githubu")](walkthrough-images/image02.png#lightbox)

Můžeme vidět následující tři adresáře v projektu:

- **InfColorPicker** – tento adresář obsahuje kód Objective-C pro projekt.
- **PickerSamplePad** – tento adresář obsahuje ukázkový projekt pro iPad.
- **PickerSamplePhone** – tento adresář obsahuje ukázkový projekt pro iPhone.

Umožňuje stáhnout InfColorPicker projekt z [Githubu](https://github.com/InfinitApps/InfColorPicker/archive/master.zip) a rozbalte ho do adresáře naše volba. Případným otevřením cíl Xcode pro `PickerSamplePhone` projektu, uvidíme následující strukturu projektu v části Xcode Navigátor:

[![](walkthrough-images/image03.png "Struktura projektu v části Xcode Navigátor")](walkthrough-images/image03.png#lightbox)

Tento projekt dosahuje opakované využívání kódu přímo do každý projekt ukázky přidání InfColorPicker zdrojového kódu (v červeným rámečkem). Kód pro ukázkový projekt je v poli modrá. Protože tohoto konkrétního projektu neposkytuje nám se statickou knihovnou, je nutné pro nám vytvořit projekt Xcode pro Zkompilujte statickou knihovnu.

Prvním krokem je pro nás InfoColorPicker zdrojový kód se přidává do statické knihovny. Umožňuje dosáhnout postupujte takto:

1. Spuštění Xcode.
2. Z **souboru** nabídky vyberte možnost **nový** > **projektu...** :

    [![](walkthrough-images/image04.png "Zahájení projektu")](walkthrough-images/image04.png#lightbox)
3. Vyberte **rozhraní a knihovny**, **Cocoa Touch statickou knihovnu** šablony a kliknutím **Další** tlačítka:

    [![](walkthrough-images/image05.png "Vyberte šablonu Cocoa Touch statickou knihovnu")](walkthrough-images/image05.png#lightbox)

4. Zadejte `InfColorPicker` pro **název projektu** a klikněte na tlačítko **Další** tlačítka:

    [![](walkthrough-images/image06.png "Zadejte InfColorPicker jako název projektu.")](walkthrough-images/image06.png#lightbox)
5. Vyberte umístění, kam chcete projekt uložit a klikněte na tlačítko **OK** tlačítko.
6. Teď potřebujeme přidat zdroj z projektu InfColorPicker pro náš projekt statické knihovny. Vzhledem k tomu, **InfColorPicker.h** soubor již existuje v našich statické knihovny (ve výchozím nastavení), Xcode nepovolí, abychom ho přepsat. Z **Finder**, přejít do zdrojového kódu InfColorPicker v původní projekt, který jsme odblokujte z Githubu, všechny soubory InfColorPicker zkopírujte a vložte je do našich nový projekt statické knihovny:

    [![](walkthrough-images/image12.png "Zkopírujte všechny soubory InfColorPicker")](walkthrough-images/image12.png#lightbox)

7. Vraťte se do Xcode, klikněte pravým tlačítkem na **InfColorPicker** a pak zvolte položku **přidat soubory do "InfColorPicker..."**:

    [![](walkthrough-images/image08.png "Přidávání souborů")](walkthrough-images/image08.png#lightbox)

8. V dialogovém okně Přidat soubory přejděte InfColorPicker souborů se zdrojovým kódem, které jsme právě zkopírovali, vyberte je všechny a klikněte na tlačítko **přidat** tlačítka:

    [![](walkthrough-images/image09.png "Vybrat vše a klikněte na tlačítko Přidat")](walkthrough-images/image09.png#lightbox)

9. Zdrojový kód se zkopíruje do projektu:

    [![](walkthrough-images/image10.png "Zdrojový kód se zkopíruje do projektu")](walkthrough-images/image10.png#lightbox)

10. Navigátor projektů Xcode, vyberte **InfColorPicker.m** souboru a nastavte komentář poslední dva řádky (vzhledem ke způsobu, zapsán této knihovny, tento soubor nepoužívá):

    [![](walkthrough-images/image14.png "Úprava souboru InfColorPicker.m")](walkthrough-images/image14.png#lightbox)

11. Nyní potřebujeme zkontrolujte, jestli jsou všechny architektury vyžadované knihovny. Tyto informace můžete najít v souboru README nebo otevřením jednoho z ukázkových projektů k dispozici. Tento příklad používá `Foundation.framework`, `UIKit.framework`, a `CoreGraphics.framework` přidejme tedy je.

12. Vyberte **InfColorPicker cíl > fáze buildu** a rozbalte **propojení binárních souborů a knihoven** části:

    [![](walkthrough-images/image16b.png "Rozbalte část propojení binárních souborů a knihoven")](walkthrough-images/image16b.png#lightbox)

13. Použití **+** tlačítka otevřete dialogové okno vám umožňuje přidat výše uvedené požadované snímků architektury:

    [![](walkthrough-images/image16c.png "Přidání že výše uvedených architektur vyžaduje snímků")](walkthrough-images/image16c.png#lightbox)

14. **Propojení binárních souborů a knihoven** oddílu by teď měl vypadat jako na obrázku níže:

    [![](walkthrough-images/image16d.png "V části propojení binárních souborů a knihoven")](walkthrough-images/image16d.png#lightbox)

V tuto chvíli jsme zavřít, ale můžeme ještě není vše Hotovo. Vytvoření statické knihovny, ale musíme sestavte ho k vytvoření systému souborů Fat binární, který zahrnuje všechny povinné architektury pro zařízení s Iosem a simulátoru iOS.

### <a name="creating-a-fat-binary"></a>Vytváření Fat binárního souboru

Všechna zařízení s Iosem mají procesory technologii architektuře ARM, které mají vyvinuty v průběhu času. Každé nové architektury přidat nové pokyny a další vylepšení a přitom zachovávat zpětné kompatibility. Na zařízeních s Iosem máme armv6, armv7, armv7s, arm64 instrukční sadu – i když [jsme již nebudete používat armv6](~/ios/deploy-test/compiling-for-different-devices.md). Simulátoru iOS se používá technologii ARM a je istead x86 a x86_64 s využitím simulátoru. Společnost Microsoft, které znamená, že nám je, že jsme na každou instrukci nezajistil knihovny nastaví.

Fat knihovna je `.a` souboru, který obsahuje všechny podporované architektury.

Vytvoření systému souborů fat binární je o třech krocích:

- Zkompilujte ARM 7 a ARM64 verzi statickou knihovnu.
- Zkompilujte x86 a x84_64 verzi statickou knihovnu.
- Použití `lipo` nástroj příkazového řádku zkombinovat do jedné dvě statické knihovny.

Když jsou spíše jednoduché tyto tři kroky a může být nutné zopakovat je v budoucnosti, když knihovnou Objective-C obdrží aktualizace nebo pokud vyžadujeme opravy chyb. Pokud se rozhodnete k automatizaci těchto kroků, zjednoduší budoucí údržby a podpory vazby projektu pro iOS.

Celá řada nástrojů, které jsou k dispozici automatizovat takových úloh - skript prostředí, [převislým](http://rake.rubyforge.org/), [xbuild](http://www.mono-project.com/docs/tools+libraries/tools/xbuild/), a [zkontrolujte](https://developer.apple.com/library/mac/documentation/Darwin/Reference/ManPages/man1/make.1.html). Když jsme nainstalovali nástroje Xcode příkazového řádku, jsme také nainstalovali, ujistěte se tak, že se systém sestavení, který se použije pro tento návod. Tady je **Makefile** , můžete použít k vytvoření více architektura sdílené knihovny, který bude fungovat na zařízení s Iosem a simulátor pro všechny knihovny:

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

Zadejte **Makefile** příkazy v editoru prostého textu, které si vyberete a aktualizovat oddíly se **váš název projektu** s názvem projektu. Je také důležité zajistit, že jsme vložte pokyny výše, aby byla zachována kartách pokynů.

Uložte soubor s názvem **Makefile** do stejného umístění jako statická knihovna InfColorPicker Xcode jsme vytvořili výše:

[![](walkthrough-images/lib00.png "Uložte soubor s názvem souboru pravidel")](walkthrough-images/lib00.png#lightbox)

Otevřete aplikaci terminál na počítači Mac a přejděte do umístění vašeho souboru pravidel. Typ `make` do terminálu, stiskněte klávesu **Enter** a **Makefile** se spustí:

[![](walkthrough-images/lib01.png "Ukázkový výstup souboru pravidel")](walkthrough-images/lib01.png#lightbox)

Když spustíte, ujistěte se, zobrazí se hodně textu posouvání. Pokud vše funguje správně, zobrazí se vám slova **sestavení proběhlo úspěšně** a `libInfColorPicker-armv7.a`, `libInfColorPicker-i386.a` a `libInfColorPickerSDK.a` soubory budou zkopírovány do stejného umístění jako **Makefile**:

[![](walkthrough-images/lib02.png "LibInfColorPicker armv7.a, libInfColorPicker i386.a a libInfColorPickerSDK.a soubory generované záznamem souboru pravidel")](walkthrough-images/lib02.png#lightbox)

Architektury můžete ověřit v rámci Fat binární soubor pomocí následujícího příkazu:

```bash
xcrun -sdk iphoneos lipo -info libInfColorPicker.a
```

Mělo by se zobrazit následující:

```bash
Architectures in the fat file: libInfColorPicker.a are: i386 armv7 x86_64 arm64
```

V tuto chvíli jsme dokončení první krok naše vazby iOS tak, že vytvoříte statické knihovny pomocí Xcode a nástroje příkazového řádku Xcode `make` a `lipo`. Můžeme přejít k dalšímu kroku a použijte **cíle Sharpie** k automatizaci vytváření vazeb rozhraní API pro nás.

<a name="Create_a_Xamarin.iOS_Binding_Project"/>

## <a name="create-a-xamarinios-binding-project"></a>Vytvoření vazby projektu Xamarin.iOS

Dřív, než můžete **Sharpie cíle** automatizovat proces vytváření vazby, potřebujeme k vytvoření vazby projektu Xamarin.iOS k umístění definice rozhraní API (který budeme používat **Sharpie cíle** nám pomůže sestavení) a vytvoření vazby C# pro nás.

Provedeme následující:

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

1. Spusťte sadu Visual Studio pro Mac.
1. Z **souboru** nabídce vyberte možnost **nový** > **řešení...** :

    ![](walkthrough-images/bind01.png "Spouští se nové řešení")

1. V dialogovém okně Nová řešení vyberte **knihovny** > **iOS vazby projektu**:

    ![](walkthrough-images/bind02.png "Vybrat iOS vazby projektu")

1. Klikněte na tlačítko **Další** tlačítko.

1. Zadejte "InfColorPickerBinding" jako **název projektu** a klikněte na tlačítko **vytvořit** pro vytvoření řešení:

    ![](walkthrough-images/bind02a.png "Jako název projektu zadejte InfColorPickerBinding")

Řešení se nevytvoří a dvě výchozí soubory budou zahrnuty:

![](walkthrough-images/bind03.png "Struktury řešení v Průzkumníku řešení")


# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)


1. Spusťte sadu Visual Studio.

1. Z **souboru** nabídce vyberte možnost **nový** > **projektu...** :

    ![Spouští se nový projekt](walkthrough-images/bind01vs.png "zahájení projektu")

1. V dialogovém okně Nový projekt, vyberte **Visual C# > iPhone & iPad > Knihovna vazeb (Xamarin) pro iOS**:

    [![Vyberte knihovna vazeb pro iOS](walkthrough-images/bind02.w157-sml.png)](walkthrough-images/bind02.w157.png#lightbox)

1. Zadejte "InfColorPickerBinding" jako **název** a klikněte na tlačítko **OK** pro vytvoření řešení.

Řešení se nevytvoří a dvě výchozí soubory budou zahrnuty:

![](walkthrough-images/bind03vs.png "Struktury řešení v Průzkumníku řešení")

-----

- **ApiDefinition.cs** – tento soubor bude obsahovat smluv, které definují, jak se dá zabalit API jazyka Objective-C v jazyce C#.
- **Structs.cs** – tento soubor bude obsahovat všechny struktury nebo výčet hodnot, které jsou vyžadované rozhraní a delegátů.

Pomocí těchto dvou souborů později v tomto návodu budeme pracovat. Nejdřív potřebujeme přidat InfColorPicker knihovny do projektu vazby.

### <a name="including-the-static-library-in-the-binding-project"></a>Včetně statickou knihovnu v projektu vazby

Teď máme naši základní vazby projektu připravené, je potřeba přidat Fat binární knihovny jsme vytvořili výše pro **InfColorPicker** knihovny.

Postupujte podle těchto kroků přidejte knihovny:

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

1. Klikněte pravým tlačítkem na **nativní odkazy** složky v oblasti řešení a vyberte **přidat nativní odkazy**:

    ![](walkthrough-images/bind04a.png "Přidat nativní odkazy")

1. Přejděte na Fat binární jsme provedli dříve (`libInfColorPickerSDK.a`) a stiskněte klávesu **otevřít** tlačítka:

    ![](walkthrough-images/bind05.png "Vyberte soubor libInfColorPickerSDK.a")
1. Soubor bude zahrnutý v projektu:

    ![](walkthrough-images/bind04.png "Včetně soubor")

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

1. Kopírovat `libInfColorPickerSDK.a` z vaší **hostitele sestavovací Mac** a vložte ho do svého projektu vazby.

1. Klikněte pravým tlačítkem na projekt a zvolte **Přidat > existující položku...** :

    ![](walkthrough-images/bind04vs.png "Přidat existující soubor")

1. Přejděte `libInfColorPickerSDK.a` a stiskněte klávesu **přidat** tlačítka:

    ![](walkthrough-images/bind05vs.png "Přidání libInfColorPickerSDK.a")

1. Soubor bude zahrnutý v projektu.

-----

Když **.a** se přidá soubor do projektu, Xamarin.iOS automaticky nastaví **akce sestavení** souboru, který se **ObjcBindingNativeLibrary**a vytvořit soubor speciální volá se, `libInfColorPickerSDK.linkwith.cs`.


Tento soubor obsahuje `LinkWith` atribut, který říká Xamarin.iOS jak popisovač statickou knihovnu, kterou jsme právě přidali. Obsah tohoto souboru jsou uvedeny v následující fragment kódu:

```csharp
using ObjCRuntime;

[assembly: LinkWith ("libInfColorPickerSDK.a", SmartLink = true, ForceLoad = true)]
```

`LinkWith` Atribut identifikuje statické knihovny pro projekt a některé příznaky linkeru důležité.


Další věc, kterou budeme muset udělat, je vytvořit definice rozhraní API pro projekt InfColorPicker. Pro účely tohoto návodu budeme používat Sharpie cíle pro generování souboru **ApiDefinition.cs**.

<a name="Using_Objective_Sharpie"/>

## <a name="using-objective-sharpie"></a>Pomocí cíle Sharpie

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)


Cíle Sharpie je příkazový řádek, který nástroj (postkytovatel: Xamarin), který může být užitečné při vytváření definice potřebné k vytvoření vazby 3. stran Objective-C knihovny jazyka C#. V této části, použijeme Sharpie cíle vytvořit počáteční **ApiDefinition.cs** InfColorPicker projektu.


# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)


Cíle Sharpie je příkazový řádek, který nástroj (postkytovatel: Xamarin), který může být užitečné při vytváření definice potřebné k vytvoření vazby 3. stran Objective-C knihovny jazyka C#. V této části, použijeme Sharpie cíle na naše **hostitele sestavovací Mac** vytvořit počáteční **ApiDefinition.cs** InfColorPicker projektu.


-----

Začněte tím, můžeme stáhnout soubor Instalační služby systému Sharpie cíle popsané v tomto [průvodce](~/cross-platform/macios/binding/objective-sharpie/get-started.md#installing). Spusťte instalační program a postupujte podle všech pokynů Průvodce instalací nainstalovat Sharpie cíle na na našich vývojovém počítači.

Jakmile budeme mít cíl Sharpie úspěšně nainstalován, můžeme spustit aplikaci terminál a zadáním následujícího příkazu můžete zobrazit nápovědu pro všechny nástroje, které poskytuje pomáhat ve vazbě:

```bash
sharpie -help
```

Pokud se provede výše uvedený příkaz, se vygeneruje následující výstup:

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

Pro účely tohoto návodu použijeme tyto nástroje Sharpie cíle:

- **xcode** – tohoto nástroje získáváme informace o našich aktuální instalaci Xcode a verze zařízení s Iosem a rozhraní API Macu, kterou jsme nainstalovali. Použijeme tyto informace později při generování jsme naše vazby.
- **Vytvoření vazby** -použijeme tento nástroj pro analýzu **.h** soubory do původního projektu InfColorPicker **ApiDefinition.cs** a **StructsAndEnums.cs** soubory.

Můžete zobrazit nápovědu pro konkrétní cíl Sharpie nástroj, zadejte název tohoto nástroje a `-help` možnost. Například `sharpie xcode -help` vrátí následující výstup:

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

Než začneme proces vytváření vazby, musíte získat informace o našem aktuálním nainstalovaných sad SDK tak, že do terminálu zadáte následující příkaz `sharpie xcode -sdks`:

```bash
amyb:Desktop amyb$ sharpie xcode -sdks
sdk: appletvos9.2    arch: arm64
sdk: iphoneos9.3     arch: arm64   armv7
sdk: macosx10.11     arch: x86_64  i386
sdk: watchos2.2      arch: armv7
```

Z výše uvedeného můžeme vidět, že máme `iphoneos9.3` na naše počítači nainstalovaná sada SDK. Pomocí těchto informací v místě, jsme připraveni k analýze projektu InfColorPicker `.h` soubory do původního **ApiDefinition.cs** a `StructsAndEnums.cs` InfColorPicker projektu.

Zadejte následující příkaz v aplikaci terminál:

```bash
sharpie bind --output=InfColorPicker --namespace=InfColorPicker --sdk=[iphone-os] [full-path-to-project]/InfColorPicker/InfColorPicker/*.h
```

Kde `[full-path-to-project]` je úplná cesta k adresáři kde **InfColorPicker** soubor projektu Xcode je umístěn v našich počítači a [operační systém iphonu] je sady iOS SDK, kterou jsme nainstalovali, jak je uvedeno ve `sharpie xcode -sdks` příkazu. Všimněte si, že v tomto příkladu jsme splnění  **\*.h** jako parametr, který zahrnuje *všechny* soubory záhlaví v tomto adresáři – obvykle je by měla tuto akci, ale místo toho pečlivě přečtěte si soubory hlaviček najít na nejvyšší úrovni **.h** soubor, který odkazuje na všechny další příslušné soubory a předat ho jenom do Sharpie cíle.

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

A **InfColorPicker.enums.cs** a **InfColorPicker.cs** soubory budou vytvořeny v našich adresáře:

[![](walkthrough-images/os06.png "Soubory InfColorPicker.enums.cs a InfColorPicker.cs")](walkthrough-images/os06.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)


Otevřete oba soubory v projektu vazby, který jsme vytvořili výše. Zkopírujte obsah **InfColorPicker.cs** soubor a vložte ho do **ApiDefinition.cs** souboru, nahradí stávající `namespace ...` kód bloku s obsahem  **InfColorPicker.cs** souboru (opustit `using` příkazy beze změny):

![](walkthrough-images/os07.png "Soubor InfColorPickerControllerDelegate")


# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)


Otevřete oba soubory v projektu vazby, který jsme vytvořili výše. Zkopírujte obsah **InfColorPicker.cs** souboru (z **hostitele sestavovací Mac**) a vložte ho do **ApiDefinition.cs** souboru, nahradí stávající `namespace ...` blok kódu s obsahem **InfColorPicker.cs** souboru (opuštění `using` příkazy beze změny).


-----

<a name="Normalize_the_API_Definitions"/>

## <a name="normalize-the-api-definitions"></a>Normalizovat definice rozhraní API

Cíle Sharpie má někdy překladu problém `Delegates`, takže bude potřeba upravit definici `InfColorPickerControllerDelegate` rozhraní a nahraďte `[Protocol, Model]` řádek následujícím kódem:

```csharp
[BaseType(typeof(NSObject))]
[Model]
```
Tak, aby definice vypadá takto:

[![](walkthrough-images/os11.png "Definice")](walkthrough-images/os11.png#lightbox)

Dále jsme to samé udělá s obsahem `InfColorPicker.enums.cs` souboru, kopírování a vkládání v `StructsAndEnums.cs` souboru byste museli opustit `using` příkazy beze změny:

[![](walkthrough-images/os09.png "Obsah StructsAndEnums.cs souboru ")](walkthrough-images/os09.png#lightbox)

Můžete také zjistit, že má cíl Sharpie s poznámkami vazba s `[Verify]` atributy. Tyto atributy znamenat, že byste měli ověřit, že cíl Sharpie nebyla správnou věc porovnáním vazby s původní deklarací C/Objective-C (který bude k dispozici v komentář nad deklaraci vazby). Jakmile si ověříte, že vazby, byste měli odebrat atribut ověřit. Další informace najdete [ověřte](~/cross-platform/macios/binding/objective-sharpie/platform/verify.md) průvodce.

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)


V tomto okamžiku naše vazby projektu by měl být dokončena a připravena k sestavení. Pojďme naši vazby projekt sestavil a ujistěte se, že jsme jsem skončil bez chyb:

[Vytvoření vazby projektu a ujistěte se, že zde nejsou žádné chyby](walkthrough-images/os12.png)


# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)


V tomto okamžiku naše vazby projektu by měl být dokončena a připravena k sestavení. Pojďme naši vazby projekt sestavil a ujistěte se, že jsme jsem skončil bez chyb.


-----

<a name="Using_the_Binding"/>

## <a name="using-the-binding"></a>Pomocí vazby

Postupujte podle těchto kroků a vytvořte ukázkovou aplikaci pro iPhone používat iOS, které vazby knihovny vytvořili výše:

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

1. **Vytvoření projektu Xamarin.iOS** – přidat nový projekt Xamarin.iOS s názvem **InfColorPickerSample** k řešení, jak je znázorněno na následujících snímcích obrazovky:

    ![](walkthrough-images/use01.png "Přidání aplikace s jedním zobrazením")

    ![](walkthrough-images/use01a.png "Nastavení identifikátor")

1. **Přidat odkaz na projekt vazby** -aktualizace **InfColorPickerSample** projekt tak, aby obsahuje odkaz na **InfColorPickerBinding** projektu:

    ![](walkthrough-images/use02.png "Přidává se odkaz na projekt vazby")

1. **Vytvoření uživatelského rozhraní pro iPhone** – dvakrát klikněte na **MainStoryboard.storyboard** soubor **InfColorPickerSample** projekt tak, aby ji upravit v iOS designeru. Přidat **tlačítko** k zobrazení a jeho volání `ChangeColorButton`, jak je znázorněno v následujícím:

    ![](walkthrough-images/use03.png "Přidání tlačítka pro zobrazení")

1. **Přidat InfColorPickerView.xib** – The Objective-InfColorPicker C knihovna obsahuje **.xib** souboru. Xamarin.iOS to nezahrnují **.xib** v projektu vazby, které způsobí chyby za běhu v naší ukázkovou aplikací. Pro tento alternativním řešením je přidat **.xib** souboru pro náš projekt Xamarin.iOS. Vyberte projekt Xamarin.iOS, klikněte pravým tlačítkem a vyberte **Přidat > Přidat soubory**a přidejte **.xib** sdílené, jak je znázorněno na následujícím snímku obrazovky:

    ![](walkthrough-images/use04.png "Přidat InfColorPickerView.xib")

1. Když se zobrazí dotaz, zkopírujte **.xib** soubor do projektu.

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

1. **Vytvoření projektu Xamarin.iOS** – přidat nový projekt Xamarin.iOS s názvem **InfColorPickerSample** pomocí **aplikace s jedním zobrazením** šablony:

    [![projekt pro iOS aplikace (Xamarin)](walkthrough-images/use01.w157-sml.png)](walkthrough-images/use01.w157.png#lightbox)

    [![Vybrat šablonu](walkthrough-images/use01-2.w157-sml.png)](walkthrough-images/use01-2.w157.png#lightbox)

1. **Přidat odkaz na projekt vazby** -aktualizace **InfColorPickerSample** projekt tak, aby obsahuje odkaz na **InfColorPickerBinding** projektu:

    ![](walkthrough-images/use02vs.png "Přidat odkaz na projekt vazby")

1. **Vytvoření uživatelského rozhraní pro iPhone** – dvakrát klikněte na **MainStoryboard.storyboard** soubor **InfColorPickerSample** projekt tak, aby ji upravit v iOS designeru. Přidat **tlačítko** k zobrazení a jeho volání `ChangeColorButton`, jak je znázorněno v následujícím:

    ![](walkthrough-images/use03vs.png "Vytvoření uživatelského rozhraní pro iPhone")

1. **Přidat InfColorPickerView.xib** – The Objective-InfColorPicker C knihovna obsahuje **.xib** souboru. Xamarin.iOS to nezahrnují **.xib** v projektu vazby, které způsobí chyby za běhu v naší ukázkovou aplikací. Pro tento alternativním řešením je přidat **.xib** soubor do projektu Xamarin.iOS z našich **hostitele sestavovací Mac**. Vyberte projekt Xamarin.iOS, klikněte pravým tlačítkem a vyberte **přidat** > **existující položku...** a přidejte **.xib** souboru.

-----

Teď Pojďme rychle podívat na protokoly v Objective-C a jak nakládáme s nimi v kódu jazyka C# a vazby.

### <a name="protocols-and-xamarinios"></a>Protokoly a Xamarin.iOS

V jazyce Objective-C, což je protokol definuje metody (nebo zprávy), který je možné za určitých okolností. Entity jsou velmi podobné rozhraní v jazyce C#. Jedním z hlavních rozdílů mezi protokol Objective-C a interface C# je, že protokoly může mít nepovinný metody - metody, které není potřeba implementovat třídu. Objective-C používá @optional – klíčové slovo se používá k označení, které metody jsou volitelné. Další informace o protokolech najdete v části [událostí, protokoly a delegáti](~/ios/app-fundamentals/delegates-protocols-and-events.md).

**InfColorPickerController** má jeden takový protokol je znázorněno v následujícím fragmentu kódu:

```csharp
@protocol InfColorPickerControllerDelegate

@optional

- (void) colorPickerControllerDidFinish: (InfColorPickerController*) controller;
// This is only called when the color picker is presented modally.

- (void) colorPickerControllerDidChangeColor: (InfColorPickerController*) controller;

@end
```

Tento protokol je používán **InfColorPickerController** informovat klienty, že uživatel vybral novou barvu a že **InfColorPickerController** bylo dokončeno. Cíle Sharpie mapován tento protokol, jak je znázorněno v následujícím fragmentu kódu:

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

Při kompilaci knihovny vazby Xamarin.iOS vytvoří abstraktní základní třída, která je volána `InfColorPickerControllerDelegate`, který implementuje toto rozhraní s virtuální metody.

Existují dva způsoby, že jsme toto rozhraní implementují aplikace pro Xamarin.iOS:

- **Silná delegovat** – použití silné delegáta patří vytvoření třídy C#, která je podtřídou `InfColorPickerControllerDelegate` a přepisuje patřičné metody. **InfColorPickerController** instance této třídy bude používat pro komunikaci s klienty.
- **Slabé delegáta** – slabé delegát je mírně odlišné technika, která zahrnuje vytvoření veřejnou metodu v některé třídě (jako `InfColorPickerSampleViewController`) a potom vystavení metody k `InfColorPickerDelegate` přes protokol `Export` atribut.

Silné delegáti poskytují technologie Intellisense, bezpečnost typů a lepší zapouzdření. Z těchto důvodů byste měli používat silná delegáti kde je to možné, namísto slabé delegáta.

V tomto názorném postupu se budeme zabývat obě tyto metody: nejprve implementace silné delegáta a pak vám vysvětlíme, jak implementovat slabé delegáta.

### <a name="implementing-a-strong-delegate"></a>Implementace silných delegáta

Dokončení aplikace Xamarin.iOS pomocí silné delegáta reagovat na `colorPickerControllerDidFinish:` zpráva:

**Podtřídy InfColorPickerControllerDelegate** – přidání nové třídy projektu s názvem `ColorSelectedDelegate`. Upravte třídy tak, aby byly následující kód:

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

Xamarin.iOS vytvoří vazbu delegáta Objective-C tak, že vytvoříte abstraktní základní třída, která je volána `InfColorPickerControllerDelegate`. Podtřídy tento typ a přepsání `ColorPickerControllerDidFinish` metoda přistupovat k hodnotě `ResultColor` vlastnost `InfColorPickerController`.

**Vytvořte instanci ColorSelectedDelegate** – naše obslužné rutiny události bude nutné instance `ColorSelectedDelegate` typ, který jsme vytvořili v předchozím kroku. Upravit třídu `InfColorPickerSampleViewController` a přidejte následující proměnnou instance do třídy:

```csharp
ColorSelectedDelegate selector;
```

**Inicializace proměnné ColorSelectedDelegate** – Pokud chcete zajistit, aby `selector` je platná instance, aktualizujte metodu `ViewDidLoad` v `ViewController` tak, aby odpovídala následující fragment kódu:

```csharp
public override void ViewDidLoad ()
{
    base.ViewDidLoad ();
    ChangeColorButton.TouchUpInside += HandleTouchUpInsideWithStrongDelegate;
    selector = new ColorSelectedDelegate (this);
}
```
**Implementovat metodu HandleTouchUpInsideWithStrongDelegate** -vedle implementujte obslužnou rutinu události pro když uživatel dotýká **ColorChangeButton**. Upravit `ViewController`a přidejte následující metodu:

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

Nejprve získáme instance `InfColorPickerController` prostřednictvím statická metoda a ujistěte se, že instance přehled o našich silné delegáta prostřednictvím vlastnosti `InfColorPickerController.Delegate`. Tato vlastnost automaticky vygeneroval pro nás Sharpie cíle. Nakonec zavoláme `PresentModallyOverViewController` zobrazit `InfColorPickerSampleViewController.xib` tak, aby uživatel může vybrat barvu.

**Spusťte aplikaci** – v tuto chvíli máme Hotovo se všemi našeho kódu. Pokud aplikaci spouštíte, je třeba změnit barvu pozadí `InfColorColorPickerSampleView` jak je znázorněno na následujících snímcích obrazovky:

[![](walkthrough-images/run01.png "Spuštění aplikace")](walkthrough-images/run01.png#lightbox)

Blahopřejeme! V tomto okamžiku jste úspěšně vytvořili a svázán knihovnou Objective-C pro použití aplikace pro Xamarin.iOS. V dalším kroku Povíme si něco o použití slabé delegátů.

### <a name="implementing-a-weak-delegate"></a>Implementace slabého delegáta

Místo vytvoření podtřídy třídy vázán na protokol Objective-C u konkrétního delegáta, Xamarin.iOS také umožňuje implementovat protokol metody do třídy, která je odvozena z `NSObject`, upravení vaše metody s `ExportAttribute`a pak poskytnutí odpovídající selektory. Při volbě této možnosti můžete přiřadit instanci třídy má `WeakDelegate` vlastnost místo položky `Delegate` vlastnost. Slabé delegáta nabízí flexibilitu se vaše třída delegáta hierarchií různých dědičnosti. Podívejme se na tom, jak implementovat a použití delegáta slabé v naší aplikaci Xamarin.iOS.

**Vytvořte obslužnou rutinu události pro TouchUpInside** -vytvoříme novou obslužnou rutinu události pro `TouchUpInside` události změnit barvu pozadí tlačítka. Tato obslužná rutina vyplní jako stejný atribut role `HandleTouchUpInsideWithStrongDelegate` obslužné rutiny, že jsme vytvořili v předchozí části, ale použije slabé delegáta namísto silné delegáta. Upravit třídu `ViewController`a přidejte následující metodu:

```csharp
private void HandleTouchUpInsideWithWeakDelegate (object sender, EventArgs e)
{
    InfColorPickerController picker = InfColorPickerController.ColorPickerViewController();
    picker.WeakDelegate = this;
    picker.SourceColor = this.View.BackgroundColor;
    picker.PresentModallyOverViewController (this);
}
```
**Aktualizovat ViewDidLoad** -musí Změníme `ViewDidLoad` tak, aby používal obslužnou rutinu události, kterou jsme právě vytvořili. Upravit `ViewController` a změňte `ViewDidLoad` tak, aby připomínala následující fragment kódu:


```csharp
public override void ViewDidLoad ()
{
    base.ViewDidLoad ();
    ChangeColorButton.TouchUpInside += HandleTouchUpInsideWithWeakDelegate;
}

```

**Zpracování colorPickerControllerDidFinish: zpráva** – když `ViewController` je dokončená, iOS bude odesílat zprávy `colorPickerControllerDidFinish:` k `WeakDelegate`. Potřebujeme vytvořit metodu C#, která dokáže zpracovat tuto zprávu. K tomu vytvoříme metoda C# a potom ho s doplnění `ExportAttribute`. Upravit `ViewController`a přidejte následující metodu do třídy:

```csharp
[Export("colorPickerControllerDidFinish:")]
public void ColorPickerControllerDidFinish (InfColorPickerController controller)
{
    View.BackgroundColor = controller.ResultColor;
    DismissViewController (false, null);
}

```

Spusťte aplikaci. By se měl chovat nyní stejným způsobem jako předtím, ale používá slabé delegáta namísto silné delegáta. Tento názorný postup v tomto okamžiku jste úspěšně dokončen. Teď byste měli mít představu o tom, jak vytvářet a využívat vazby projektu Xamarin.iOS.

## <a name="summary"></a>Souhrn

Tento článek vás provede procesem vytvoření a použití vazby projektu Xamarin.iOS. Nejprve jsme probírali jak sestavit existující knihovnou Objective-C do statické knihovny. Potom jsme probrali vytvoření vazby projektu Xamarin.iOS a jak pomocí cíle Sharpie ke generování definice rozhraní API pro knihovnou Objective-C. Jsme probírali aktualizaci a upravit generované definice rozhraní API tak, aby byly vhodné pro zveřejnění. Po dokončení vazby projektu Xamarin.iOS byl spotřebovávalo, že vazba v aplikaci Xamarin.iOS, se zaměřením na použití delegátů silné a slabé delegáti jsme přešli.

## <a name="related-links"></a>Související odkazy

- [Příklad vazby (ukázka)](https://developer.xamarin.com/samples/monotouch/InfColorPicker/)
- [Knihovny pro vytváření vazeb Objective-C](~/cross-platform/macios/binding/objective-c-libraries.md)
- [Podrobnosti o vazbě](~/cross-platform/macios/binding/overview.md)
- [Vazby typů referenční příručka](~/cross-platform/macios/binding/binding-types-reference.md)
- [Xamarin pro vývojáře v Objective-C](~/ios/get-started/objective-c-developers/index.md)
- [Pokyny k návrhu architektury](http://msdn.microsoft.com/library/ms229042.aspx)
- [Xamarin University kurz: Vytvoření knihovny vazeb Objective-C](https://university.xamarin.com/classes/track/all#building-an-objective-c-bindings-library)
- [Xamarin University kurz: Vytvoření knihovny vazeb Objective-C pomocí cíle Sharpie](https://university.xamarin.com/classes/track/all#build-an-objective-c-bindings-library-with-objective-sharpie)
