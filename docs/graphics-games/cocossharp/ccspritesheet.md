---
title: "Vylepšení snímkový kmitočet s CCSpriteSheet"
description: "CCSpriteSheet poskytuje funkce pro kombinování a používání mnoho souborů bitové kopie v jedné texture. Snižuje počet texture zlepšit časů načítání herní a kmitočet snímků."
ms.topic: article
ms.prod: xamarin
ms.assetid: A1334030-750C-4C60-8B84-1A8A54B0D00E
ms.technology: xamarin-cross-platform
author: charlespetzold
ms.author: chape
ms.date: 03/24/2017
ms.openlocfilehash: ec8a641fbd15f826e92ada62f65b17dd46b369e4
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 02/27/2018
---
# <a name="improving-framerate-with-ccspritesheet"></a>Vylepšení snímkový kmitočet s CCSpriteSheet

_CCSpriteSheet poskytuje funkce pro kombinování a používání mnoho souborů bitové kopie v jedné texture. Snižuje počet texture zlepšit časů načítání herní a kmitočet snímků._

Mnoho hry vyžadují optimalizace ve snaze o bez problémů spustit a rychle zatížení na mobilní hardwaru. `CCSpriteSheet` Třída může pomoct vyřešit mnoho běžných potíží s výkonem, se kterými CocosSharp hry. Tento průvodce popisuje běžné problémy s výkonem a způsob jejich řešení pomocí `CCSpriteSheet` třídy.


# <a name="what-is-a-sprite-sheet"></a>Co je to list pohyblivý symbol?

A *pohyblivý symbol list*, který lze také odkazovat jako *texture atlas*, je obrázek, který kombinuje více bitových kopií do jednoho souboru. Tím lze vylepšit výkon modulu runtime, jakož i časů načtení obsahu.

Například na následujícím obrázku je jednoduchý pohyblivý symbol list vytvořené tři samostatné Image. Jednotlivé bitové kopie může mít libovolnou velikost, a výsledný list pohyblivý symbol není nutné zcela vyplněn:

![](ccspritesheet-images/image1.png "Jednotlivé bitové kopie může mít libovolnou velikost, a není potřeba zcela vyplněn výsledný seznam pohyblivý symbol")


## <a name="render-states"></a>Vykreslení stavy

Vizuální objekty CocosSharp (například `CCSprite`) zjednodušit kód vykreslování přes tradiční grafické kódu vykreslování rozhraní API, například MonoGame nebo OpenGL, které vyžadují vytvoření vrchol vyrovnávací paměti (jak je uvedeno v [kreslení 3D grafiky s Vrcholy v MonoGame](~/graphics-games/monogame/3d/part2.md) průvodce). Bez ohledu na jeho jednoduchost CocosSharp nebude odstraněn náklady na nastavení *vykreslení stavy*, které jsou počet, kód vykreslování musí přejít textury nebo jiné související vykreslování stavy.

Každý prvek visual CocosSharp na interní kód postupně, vykreslí pomocí procházení začátku vizuálním stromu s aktuálním `CCScene`. Zvažte například následující scény:

![](ccspritesheet-images/image2.png "Vezměte v úvahu tato scény")

CocosSharp by vykreslení čtyřmi hvězdičkami v pořadí:

![](ccspritesheet-images/image3.png "CocosSharp by v pořadí vykreslování čtyřmi hvězdičkami")

Protože každý `CCSprite` používá stejné struktury CocosSharp můžete seskupit všechny čtyři hvězdiček. Tento kód vyžaduje pouze jeden vykreslení stavu přiřazení (přiřazení hvězdičkami texture) na jeden rámeček. Tento scénář je velmi efektivní.

Samozřejmě velmi málo hry použít jedinou bitovou kopii. Následující scény zavádí planetu obrázek:

![](ccspritesheet-images/image4.png "Tato scény zavádí planetu grafiky")

V ideálním případě by měl CocosSharp kreslení všechny Sprite pomocí jedné image nejprve (například hvězdiček) a potom zbytek Sprite pomocí jiného obrázku (planety):

![](ccspritesheet-images/image5.png "V ideálním případě by měl CocosSharp kreslení všechny Sprite pomocí jedné image nejprve například hvězdiček, pak zbytek Sprite pomocí jiného obrázku planety")

Řazení vyšší vyžaduje dva stavy vykreslení: jednu na první jednou hvězdičkou, na první světě.

Pokud všechny `CCSprite` instance jsou podřízené objekty stejné `CCNode`, pak CocosSharp optimalizuje pořadí kreslení ke snížení vykreslení změny stavu. Pokud na druhé straně, `CCSprite` instance jsou uspořádány tak, aby nelze k optimalizaci vykreslování CocosSharp (jako třeba když jsou součástí jiné entity `CCNode` instance), pak pořadí nemusí být optimální. Nejhorší možné kreslení pořadí, výsledkem je pět stavy vykreslování obrázku:

![](ccspritesheet-images/image6.png "Ukazuje to nejhorší možné kreslení pořadí, výsledkem je pět vykreslení stavy")

Vykreslení stavy může být obtížné optimalizace, protože pořadí kreslení musí orientují stromu visual `CCNode` instance. Tento strom je často strukturovaná být snadno pracovat (například entity obsahující jejich visual podřízené) nebo z důvodu na požadované rozložení visual uspořádané podle definice umělcem.

Ideálním případě je samozřejmě tak, aby měl jeden vykreslení stavu, bez ohledu s více bitových kopií. Hry CocosSharp se dá dosáhnout kombinace všechny bitové kopie do jednoho souboru a poté načítání než jednoho souboru (spolu s jeho doplňujícími **.plist** soubor) do `CCSpriteSheet`. Pomocí `CCSpriteSheet` třída stane i důležitější pro hry, které máte velký počet obrázků, nebo které mají velmi komplikovanější rozložení. 

## <a name="load-times"></a>Časů načtení

Kombinace více bitových kopií do jednoho souboru taky zlepšuje časů načítání herní několik důvodů:

 - Kombinování více bitových kopií do jednoho souboru můžete snížit celkový počet pixelů používá prostřednictvím efektivní okolních
 - Načítání méně souborů znamená menší nároky na soubor, jako je například analýze .png záhlaví
 - Načítání souborů méně vyžaduje menší počet vyhledávání, čas, což je důležité pro médium založené na disku například DVD a pevné disky tradiční počítače

# <a name="using-ccspritesheet-in-code"></a>Použití CCSpriteSheet v kódu

Chcete-li vytvořit `CCSpriteSheet` instance, kód musíte zadat bitovou kopii a soubor, který definuje oblasti bitovou kopii pro každý snímek. Bitovou kopii je možné načíst jako **.png** nebo **.xnb** souboru (Pokud se používá [obsahu kanálu](~/graphics-games/cocossharp/content-pipeline/index.md)). Soubor definice snímky **.plist** soubor, který lze vytvořit ručně nebo *TexturePacker* (který probereme níže).

Ukázková aplikace, které [Zde si můžete stáhnout](https://developer.xamarin.com/samples/mobile/SpriteSheetDemo/), vytvoří `CCSpriteSheet` z **.png** a **.plist** soubor pomocí následujícího kódu:

```csharp
CCSpriteSheet sheet = new CCSpriteSheet ("sheet.plist", "sheet.png"); 
```

Po načtení `CCSpriteSheet` obsahuje `List` z `CCSpriteFrame` instancí – jednotlivé instance odpovídající zdroj bitové kopie, použít k vytvoření celý list. U **SpriteSheetDemo** projektu, `CCSpriteSheet` obsahuje tři Image. **.Plist** soubor může být prověřovány v sadě Visual Studio pro Mac nebo libovolného textového editoru obrázků, které jsou k dispozici. Pokud jsme zobrazení **.plist** soubor v textovém editoru vidíme tři rámce (oddíly vynechání zdůraznit, názvy klíčů):


```csharp
...
<dict>
    <key>frames</key>
    <dict>
        <key>farBackground.png</key>
        ...
        <key>foreground.png</key>
        ...
<key>forestBackground.png</key>
...
```

Můžeme použít `Find` metody k vyhledání rámce podle názvu, následujícím způsobem (kód vynechání zdůraznit `CCSpriteFrame` využití):


```csharp
CCSpriteFrame frame;
...
frame = sheet.Frames.Find(item=>item.TextureFilename == "farBackground.png"); 
CCSprite sprite = new CCSprite (frame); 
...
```

Vzhledem k tomu, `CCSprite` konstruktor může trvat `CCSpriteFrame` parametr kód nikdy prozkoumat podrobnosti `CCSpriteFrame`, například které texture používá nebo oblasti obrázku v listu hlavní pohyblivý symbol.


#  <a name="creating-a-sprite-sheet-plist"></a>Vytváření pohyblivý symbol list .plist.

Soubor .plist je soubor formátu XML, který můžete vytvořit a upravit ručně. Podobně úpravy programy bitové kopie slouží k spojování více souborů do jednoho větší. Od vytvoření a údržbu listy pohyblivý symbol může být časově velmi náročné, podíváme se na programu TexturePacker, které můžete exportovat soubory ve formátu CocosSharp. TexturePacker nabízí bezplatný a "Pro" verze a je k dispozici pro systém Windows a Mac OS. Zbytek tohoto průvodce lze postupovat pomocí bezplatnou verzi. 

Může být TexturePacker [stáhli z webu TexturePacker](https://www.codeandweb.com/texturepacker). Po otevření TexturePacker nemá projektu načíst. Na výchozí obrazovce umožňuje přidat Sprite, otevřete nejnovější projekty (Pokud byla vytvořena další projekty) a vyberte formát, který má používat pro list pohyblivý symbol. CocosSharp používá formát dat Cocos2D:

![](ccspritesheet-images/image7.png "CocosSharp používá formát dat Cocos2D")

Soubory obrázků (například **.png**) mohou být přidány do TexturePacker přetáhněte-pádem je z Průzkumníka Windows v systému Windows nebo vyhledávací na macu. TexturePacker preview list pohyblivý symbol automaticky aktualizuje vždy, když je soubor přidán:

![](ccspritesheet-images/image8.png "Vždy, když je soubor přidán TexturePacker automaticky aktualizuje preview list pohyblivý symbol")

Chcete-li exportovat pohyblivý symbol list, klikněte na tlačítko **publikovat pohyblivý symbol list** tlačítko a vyberte umístění pro list pohyblivý symbol. TexturePacker uloží soubor s příponou .plist a souborem bitové kopie.

Pokud chcete používat výsledné soubory, přidejte do projektu CocosSharp .png i .plist. Informace o přidávání souborů do projektů CocosSharp najdete v tématu [implementace průvodci BouncingGame](~/graphics-games/cocossharp/first-game/part2.md). Jakmile jsou přidány soubory, může být načtena do `CCSpriteSheet` znázorněné byl dříve ve výše uvedeném kódu:

```csharp
CCSpriteSheet sheet = new CCSpriteSheet ("sheet.plist", "sheet.png"); 
```

## <a name="considerations-for-maintaining-a-texturepacker-sprite-sheet"></a>Důležité informace pro údržbu list pohyblivý TexturePacker symbol

Jako jsou vyvinuté hry, umělci může přidat, odebrat nebo změnit obrázky. Všechny změny vyžaduje aktualizované pohyblivý symbol listu. Pohyblivý symbol list údržby můžete usnadňují následující aspekty:

 - Zachovat původní soubory (soubory použít k vytvoření pohyblivý symbol listy) do složky ve vašem projektu a ujistěte se, že jsou přidány do správy verzí. Tyto soubory bude potřeba znovu vytvořit na list pohyblivý symbol vždy, když dojde ke změně.
 - Nepřidávejte původní soubory k sadě Visual Studio pro Mac nebo Visual Studio, nebo pokud jsou přidány, nastavte **akce sestavení** k **žádné**. Pokud soubory jsou přidány a mít specifické platformy **akce sestavení**, pak zbytečnému zvýší velikost souboru výsledná aplikace.
 - Zvažte použití *inteligentní složky* v TexturePacker. Inteligentní složky automaticky přidají všechny obsažené Image na list pohyblivý symbol. Tuto funkci můžete uložit mnoho času při vývoji hry s velký počet obrázků. 

    ![](ccspritesheet-images/image9.png "Tuto funkci můžete uložit mnoho času při vývoji hry s velký počet obrázků")
 - Dohlížet na velikosti texture list pohyblivý symbol. Některé starší hardware phone nepodporuje texture velikosti větší než 2048 x 2048. Navíc 32bitovou kopii 2048 x 2048 používá téměř 17 MB paměti RAM – významné množství paměti.
 - TexturePacker nezahrnuje složek v názvech pohyblivý symbol ve výchozím nastavení, takže je možné, název je v konfliktu. Je nejvhodnější rozhodněte, zda chcete zahrnout složku názvy nebo není na začátku vývoj. Větší hry měli zvážit použití názvy složek zabránit konfliktům. Chcete-li zahrnout cesty ke složce zadat, klikněte na tlačítko **upřesňující informace** v **Data** části a zkontrolujte **název složky Prepend**. 

    ![](ccspritesheet-images/image10.png "Zahrnout cesty ke složce zadat, klikněte na tlačítko Zobrazit pokročilé v datové části a zkontrolujte, zda název složky Prepend")

# <a name="summary"></a>Souhrn

Tento průvodce obsahuje informace o vytváření a používání `CCSpriteSheet` třídy. Také vysvětluje postup vytvoření souborů, které je možné načíst do `CCSpriteSheet` instance pomocí programu TexturePacker.

## <a name="related-links"></a>Související odkazy

- [CCSpriteSheet](https://developer.xamarin.com/api/type/CocosSharp.CCSpriteSheet/)
- [Úplnou ukázku (ukázka)](https://developer.xamarin.com/samples/mobile/SpriteSheetDemo/)
