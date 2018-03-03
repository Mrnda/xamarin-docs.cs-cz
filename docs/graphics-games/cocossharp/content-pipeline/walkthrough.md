---
title: "Pomocí nástroje MonoGame kanálu"
description: "Nástroj MonoGame kanálu se používá k vytváření a správě obsahu projekty MonoGame. Soubory v obsahu projekty jsou Monogame kanálu nástroj zpracoval a výstupem jako soubory .xnb pro použití v aplikacích CocosSharp a MonoGame."
ms.topic: article
ms.prod: xamarin
ms.assetid: CACFBF5F-BBD4-4D46-8DDA-1F46466725FD
ms.technology: xamarin-cross-platform
author: charlespetzold
ms.author: chape
ms.date: 03/27/2017
ms.openlocfilehash: df3692777eaa0791385c9ef3d114fbc8a9ab752e
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 02/27/2018
---
# <a name="using-the-monogame-pipeline-tool"></a>Pomocí nástroje MonoGame kanálu

_Nástroj MonoGame kanálu se používá k vytváření a správě obsahu projekty MonoGame. Soubory v obsahu projekty jsou Monogame kanálu nástroj zpracoval a výstupem jako soubory .xnb pro použití v aplikacích CocosSharp a MonoGame._

Nástroj MonoGame kanálu poskytuje snadno použitelný prostředí pro převod souborů obsahu do **.xnb** souborů pro použití v aplikacích CocosSharp a MonoGame. Informace o obsahu kanálů a proč jsou užitečné v vývoj her, najdete v článku [tento úvod na obsah kanálů](~/graphics-games/cocossharp/content-pipeline/introduction.md)

Tento návod popisuje následující:

 - Instalace nástroje MonoGame kanálu
 - Vytvoření projektu CocosSharp
 - Vytvoření obsahu projektu
 - Zpracování souborů v nástroji MonoGame kanálu
 - Pomocí souborů za běhu

Tento návod používá CocosSharp projektu k předvedení způsobu **.xnb** soubory můžete načíst a použít v aplikaci. Uživatelé MonoGame také budou moci odkazovat tento návod, jak je CocosSharp a MonoGame používat stejné **.xnb** obsahu souborů.

Dokončení aplikace se zobrazí zobrazení texture z jedné pohyblivý symbol **.xnb** souborů a přípony zobrazení pohyblivý symbol písma z **.xnb** souboru:

![](walkthrough-images/image1.png "Dokončení aplikace se zobrazí jeden pohyblivý symbol zobrazení texture ze souboru .xnb")


# <a name="monogame-pipeline-platform-discussion"></a>Diskusní platformy MonoGame kanálu

Nástroj MonoGame kanálu je k dispozici v systému Windows, OS X a Linux. Tento názorný postup se spustí nástroj v systému Windows, ale bylo možné sledovat na Mac a Linux také. Informace o získávání nástroj nastavit maximální nebo Linux najdete v tématu [tuto stránku](http://www.monogame.net/2015/01/09/monogame-pipeline-tool-available-for-macos-and-linux/).

Nástroj MonoGame kanálu je možné vytvořit obsah pro aplikace pro iOS i když běží v systému Windows, takže vývojáře, kteří používají [Xamarin Mac Agent](~/ios/get-started/installation/windows/connecting-to-mac/index.md) budou moci pokračovat ve vývoji v systému Windows.


# <a name="installing-the-monogame-pipeline-tool"></a>Instalace nástroje MonoGame kanálu

Začneme nainstalováním MonoGame, včetně MonoGame obsahu kanálu. Upozorňujeme, že MonoGame obsahu kanálu je samostatný soubor ke stažení pro Mac. Všechny instalační programy MonoGame naleznete na [MonoGame stáhne stránky](http://www.monogame.net/downloads/). Společnost Microsoft bude stahovat MonoGame pro sadu Visual Studio, ale po instalaci Vývojář můžete použít MonoGame v sadě Visual Studio pro Mac příliš:

![](walkthrough-images/image2.png "Stáhnout MonoGame pro sadu Visual Studio, ale po instalaci vývojáře použití MonoGame v sadě Visual Studio pro Mac příliš")

Po stažení, můžeme budete spustit prostřednictvím Instalační program a přijměte výchozí možnosti. Po dokončení instalace, nástroj MonoGame kanálu je nainstalován a lze nalézt v hledání nabídky Start:

![](walkthrough-images/image3.png "Po dokončení instalace je nainstalován nástroj MonoGame kanál a lze nalézt v hledání nabídky Start")

Spusťte nástroj MonoGame kanál:

![](walkthrough-images/image4.png "Spusťte nástroj MonoGame kanálu")

Jakmile je spuštěný nástroj MonoGame kanálu, můžeme začít aby naše projekty herní a obsahu.


# <a name="creating-an-empty-cocossharp-project"></a>Vytváření prázdného projektu CocosSharp

Dalším krokem je vytvoření projektu CocosSharp. Je důležité o vytvoření projektu CocosSharp nejprve tak, aby naše obsahu projektu jsme můžete uložit ve struktuře složky vytvořené CocosSharp projektu. Informace o tom, jak vytvořit nový projekt najdete v tématu [průvodci BouncingGame](~/graphics-games/cocossharp/first-game/part1.md). Tato příručka jsme budete vytvářet projekt s názvem BouncingGame, ale žádné existující projekt CocosSharp bude pracovat správně. Pokud máte existující CocosSharp projekt, který chcete přidat obsah, můžete použít tento projekt místo BouncingGame projektu.

Po vytvoření projektu jsme budete spouštět na ověřte, zda je sestavení a všechno, co máme správně nastavit:

![](walkthrough-images/image5.png "Po vytvoření projektu, spuštění a ověřit, že sestavení a jestli všechno správně nastavit")


# <a name="creating-a-content-project"></a>Vytvoření obsahu projektu

Teď, když máme herní projektu, můžeme vytvořit kanál MonoGame projektu. To uděláte, v vyberte nástroj MonoGame kanálu **soubor > Nový...**  a přejděte do složky obsahu vašeho projektu. Pro Android, se nachází ve složce **[projektu root]\BouncingGame.Android\Assets\Content\**. Pro iOS, se nachází ve složce **[projektu root]\BouncingGame.iOS\Content\**.

Změna **název souboru** k **ContentProject** a klikněte na tlačítko **Uložit** tlačítko:

![](walkthrough-images/image8.png "Změňte název souboru na ContentProject a klikněte na tlačítko Uložit")

Po vytvoření projektu nástroj MonoGame kanálu se zobrazí informace o projektu při kořenu **ContentProject** je vybraná položka:

![](walkthrough-images/image9.png "Po vytvoření projektu nástroj MonoGame kanálu se zobrazí informace o projektu, pokud je vybraná položka ContentProject kořenové")

Podívejme se na některé z vašich nejdůležitějších možnosti obsahu projektu.


## <a name="output-folder"></a>Výstupní složky

Toto je složka (relativní vůči samotného obsahu projektu) kde výstup **.xnb** budou uloženy soubory. Pro zjednodušení použijeme složce držet naše vstupní a výstupní soubory. Jinými slovy Změníme **výstupní složky** být **.\**  :

![](walkthrough-images/image10.png "")


## <a name="platform"></a>Platforma

Definuje cílovou platformu pro obsah. Všimněte si, že toto je **Windows** ve výchozím nastavení, takže jsme budete chtít změňte na našem Cílová platforma, která je **Android** (nebo iOS Pokud podle společně s projektu iOS).

![](walkthrough-images/image11.png "Všimněte si, že toto je ve výchozím nastavení systému Windows, takže to změnit na cílovou platformu, která je Android nebo iOS, pokud podle společně s projektu iOS")


# <a name="processing-files-in-the-monogame-pipelinetool"></a>Zpracování souborů v MonoGame PipelineTool

V dalším kroku jsme přidali obsah tak, aby naše **ContentProject**. Pro tento projekt jsme přidali soubory do kořenového adresáře projektu, ale větší projekty bude obvykle uspořádání obsah ve složkách.

Přidáme do našich projektu dva soubory:

 - A **.png** soubor, který se použije k vykreslení pohyblivý symbol. Tento soubor můžete [stáhnout zde](https://github.com/xamarin/mobile-samples/blob/master/BouncingGame/Resources/ball.png?raw=true).
 - A **.spritefont** soubor, který se použije k vykreslení textu na obrazovce. Nástroj ContentPipeline podporuje vytváření nových souborů .spritefont, takže není žádný soubor ke stažení.


## <a name="adding-a-png-file"></a>Přidání souboru .png

Chcete-li přidat **.png** souboru do projektu, jsme budete nejdříve zkopírovat ho do stejného adresáře jako projektu kanálu, který má **.mgcb** rozšíření.

![](walkthrough-images/image12.png "Přidejte do projektu soubor PNG")

Potom přidáme soubor do projektu kanálu. Chcete-li to provést v nástroji MonoGame kanálu, vyberte **Upravit > Přidat položku...** , vyberte **ball.png** souboru a klikněte na tlačítko **otevřete**. Soubor bude nyní součástí obsahu projektu a pokud vyberete, zobrazí se jeho vlastnosti:

![](walkthrough-images/image13.png "Soubor bude nyní součástí obsahu projektu a pokud vyberete, zobrazí se jeho vlastnosti")

Jsme vám právě ponechat všechny hodnoty výchozích hodnot, jako je potřeba načíst soubor .xnb v CocosSharp žádné změny. Jsme můžete vytvořit tak, že vyberete soubor **sestavení > sestavení** nabídky možnost, která spustí sestavení a zobrazení výstupu o sestavení. Abychom mohli ověřit, že sestavení fungovala správně kontrolou **obsahu** složky pro novou **ball.xnb** souboru:

![](walkthrough-images/image14.png "Ověřit, zda sestavení funguje správně kontrolou složku obsahu pro nový soubor ball.xnb")


## <a name="adding-a-spritefont-file"></a>Přidání souboru .spritefont

Můžeme vytvořit soubor .spritefont pomocí nástroje MonoGame kanálu. CocosSharp vyžaduje písem v **písem** složky a šablony CocosSharp automaticky automaticky vytvořit složku písem. Jsme tuto složku přidat do nástroje MonoGame kanálu výběrem **Upravit > Přidat > existující složku...** . Vyhledejte **obsahu** složky a vyberte **písem** složky a klikněte na tlačítko **OK**:

![](walkthrough-images/browsetofonts.png "Přejděte do složky obsahu a vyberte složku, písem a klikněte na tlačítko OK")

Chcete-li přidat nový soubor .sprintefont, klikněte pravým tlačítkem na složku písma a vyberte **Přidat > novou položku...** , vyberte **SpriteFont popis** možnost, zadejte název **arial 36**a klikněte na tlačítko **Ok**. CocosSharp vyžaduje velmi konkrétní názvy souborů písem – musí být ve formátu [FontType]-[velikost písma]. Pokud písmo neodpovídá tento formát názvů, nebude možné načíst CocosSharp za běhu.

![](walkthrough-images/image15.png "Pokud písmo neodpovídá tento formát názvů, nebude možné načíst CocosSharp za běhu")

Soubor .spritefont je ve skutečnosti soubor XML, který lze upravovat v každém textovém editoru, včetně sady Visual Studio for Mac. Nejběžnější proměnné upravit v souboru .spritefont jsou `FontName` a `Size` vlastnost:


```csharp
    <!-- Modify this string to change the font that will be imported. -->
    <FontName>Arial</FontName>

    <!-- Size is a float value, measured in points. 
    Modify this value to change the size of the font. -->
    <Size>12</Size> 
```

Jsme budete v každém textovém editoru otevřete soubor. Jako naše **arial 36.spritefont** naznačuje název, necháme `FontName` jako `Arial` však změnit `Size` hodnotu `36`:


```csharp
    <!-- Modify this string to change the font that will be imported. -->
    <FontName>Arial</FontName>   
  
    <!-- Size is a float value, measured in points. 
    Modify this value to change the size of the font. -->4/10/2016 12:57:28 PM 
    <Size>36</Size>
```
 
# <a name="using-files-at-runtime"></a>Pomocí souborů za běhu

.Xnb soubory jsou teď vytvořená a připravené pro použití v našem projektu. Jsme přidali soubory k sadě Visual Studio pro Mac potom přidáme kód pro naše `GameScene.cs` souboru k načtení těchto souborů a jejich zobrazení.


## <a name="adding-xnb-files-to-visual-studio-for-mac"></a>Přidávání souborů .xnb k sadě Visual Studio pro Mac

Nejprve přidáme soubory do našich projektu. V sadě Visual Studio pro Mac, jsme budete rozbalte **BouncingGame.Android** projektu, rozbalte **prostředky** složku, klikněte pravým tlačítkem na **obsahu** složky a vyberte **Přidat > Přidat soubory...** Nejprve budete vyberte jsme **ball.xnb** jsme dříve vytvořené a klikněte na tlačítko **otevřete**. Potom zopakujte výše uvedené kroky, ale přidat **arial 36.xnb** souboru. Vybereme **ponechat soubor v jeho aktuálním podadresáři** Pokud Visual Studio pro Mac dotazem, jak přidat soubor. Po dokončení oba soubory musí být součástí našich projektu:

![](walkthrough-images/image20.png "Po dokončení oba soubory musí být součástí projektu")


## <a name="adding-gamescenecs"></a>Přidání GameScene.cs

Vytvoříme třídu s názvem `GameScene,` který bude obsahovat naše objekty pohyblivý symbol a text. Chcete-li to provést, klikněte pravým tlačítkem na **BouncingGame** (ne BouncingGame.Android) projektu a vyberte **Přidat > Nový soubor...** . Vyberte **Obecné** kategorie, vyberte **prázdné třídy** možnost a pak zadejte název **GameScene**.

Po vytvoření upravíme `GameScene.cs` souboru tak, aby obsahovala následující kód:


```csharp
using System;
using CocosSharp;

namespace BouncingGame
{
    public class GameScene : CCScene
    {
        // All visual elements must be added to a CCLayer:
        CCLayer mainLayer;

        // The CCSprite is used to display the "ball" texture
        CCSprite sprite;
        // The CCLabelTtf is used to display the Arial36 sprite font
        CCLabelTtf label;

        public GameScene(CCWindow mainWindow) : base(mainWindow)
        {
            // Instantiate the CCLayer first:
            mainLayer = new CCLayer ();
            AddChild (mainLayer);

            // Now we can create the Sprite using the ball.xnb file:
            sprite = new CCSprite ("ball");
            sprite.PositionX = 200;
            sprite.PositionY = 200;
            mainLayer.AddChild (sprite);

            // The font name (arial) and size (36) need to match 
            // the .spritefont definition and file name.  
            label = new CCLabelTtf ("Using font 36", "arial", 36);
            label.PositionX = 200;
            label.PositionY = 300;
            mainLayer.AddChild (label);
        }
    }
} 
```

Jsme nebude možné hovoříte o výše uvedený kód vzhledem k tomu, že pracovat s objekty visual CocosSharp jako CCSprite a CCLabelTtf, najdete v článku [CocosSharp úvodní příručka](~/graphics-games/cocossharp/first-game/index.md).

Také je potřeba přidat kód pro načtení naše nově vytvořené `GameScene`. K tomu otevřeme `GameAppDelegate.cs` souboru (který se nachází v **BouncingGame** PCL) a upravte `ApplicationDidFinishLaunching` metoda tak, aby vypadala jako:


```csharp
public override void ApplicationDidFinishLaunching (CCApplication application, CCWindow mainWindow)
{
    application.PreferMultiSampling = false;
    application.ContentRootDirectory = "Content";

    // New code:
    GameScene gameScene = new GameScene (mainWindow);
    mainWindow.RunWithScene (gameScene);
} 
```

Při spuštění, naše herní bude vypadat jako:

![](walkthrough-images/image1.png "Při spuštění, bude vypadat hry")


# <a name="summary"></a>Souhrn

Tento průvodce vám ukázal, jak používat nástroj MonoGame kanálu vytvořit .xnb soubory ze souboru vstupní soubor ve formátu PNG a také jak vytvořit nový soubor .xnb z .sprintefont nově vytvořený soubor. Také popsané, jak struktury CocosSharp projekty, aby používaly .xnb soubory a jak k načtení těchto souborů za běhu.

## <a name="related-links"></a>Související odkazy

- [MonoGame stahování](http://www.monogame.net/downloads/)
- [Dokumentace MonoGame kanálu](http://www.monogame.net/documentation/?page=Pipeline)
- [Počáteční BouncingGame projekt pro Android (ukázka)](https://developer.xamarin.com/samples/BouncingGameCompleteAndroid/)
- [Ball.PNG obrázek (ukázka)](https://github.com/xamarin/mobile-samples/blob/master/BouncingGame/Resources/ball.png?raw=true)
