---
title: Pomocí rozložen formou dlaždic s CocosSharp
description: Na dlaždicích je efektivní, flexibilní a mapování vyspělá aplikací pro vytvoření dlaždice ortogonální a Izometrické pro hry. CocosSharp poskytuje integraci pro vedle sebe na nativní formát souborů.
ms.prod: xamarin
ms.assetid: 804C042C-F62A-4E6C-B10F-06528637F0E2
author: charlespetzold
ms.author: chape
ms.date: 03/28/2017
ms.openlocfilehash: 8a7782097324829b8150b968c5658a864d1fab4a
ms.sourcegitcommit: 0a72c7dea020b965378b6314f558bf5360dbd066
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 05/09/2018
ms.locfileid: "33921297"
---
# <a name="using-tiled-with-cocossharp"></a>Pomocí rozložen formou dlaždic s CocosSharp

_Na dlaždicích je efektivní, flexibilní a mapování vyspělá aplikací pro vytvoření dlaždice ortogonální a Izometrické pro hry. CocosSharp poskytuje integraci pro vedle sebe na nativní formát souborů._

*Vedle sebe* aplikace je standard pro vytvoření *dlaždice map* pro použití v vývoj her. Tento průvodce provede postup trvat existující .tmx soubor (soubor vytvořený vedle sebe) a použít ho v herním CocosSharp. Konkrétně tato příručka popisuje:

 - Účelem dlaždice map
 - Práce se soubory .tmx
 - Důležité informace pro vykreslování obrázky pixelů
 - Pomocí dlaždice vlastnosti za běhu

Po dokončení bude máme následující ukázku:

![](tiled-images/image1.png "Ukázkovou aplikaci vytvořili podle pokynů v této příručce")


## <a name="the-purpose-of-tile-maps"></a>Účelem dlaždice map

Dlaždice map existují v vývoj her pro desetiletí ale stále běžně se používají v 2D hry po jejich efektivitu a esthetics. Dlaždice map jsou můžete dosáhnout velmi vysoký stupeň efektivitu prostřednictvím jejich použití sad dlaždice – Zdrojová bitová kopie používá dlaždice map. Dlaždice sada je kolekce zkombinované do jednoho souboru bitové kopie. I když dlaždice sady odkazují obrázků použitých v rámci služby maps dlaždice, soubory, které obsahují více bitových kopií menší se také nazývají pohyblivý symbol listy nebo pohyblivý symbol mapuje v vývoj her. Jsme můžete vizualizovat použití sady dlaždice přidáním mřížce do sady dlaždice, který budeme používat v naši ukázku:

![](tiled-images/image2.png "Vizualizovaných zobrazení použití sady dlaždice přidáním mřížce do sady dlaždice, který se použije v ukázku")

Dlaždice map uspořádejte jednotlivé dlaždice ze sady dlaždice. Je třeba poznamenat, každý dlaždice map nemusí ukládat vlastní kopii dlaždici nastavit – místo toho více dlaždice map, můžete odkazovat stejnou sadu dlaždice. To znamená, že kromě zajištění dostatečného sadu dlaždice, dlaždice map vyžadovat velmi málo paměti. To umožňuje vytváření velký počet dlaždice map, i když se používají k vytvoření velkého hraní her oblast, například [posouvání platformer](http://en.wikipedia.org/wiki/Platform_game) prostředí. Na obrázku je možné uspořádání pomocí stejné sady dlaždice:

![](tiled-images/image3.png "Tento obrázek ukazuje možné uspořádání pomocí stejné sady dlaždice")

![](tiled-images/image4.png "Tento obrázek ukazuje možné uspořádání pomocí stejné sady dlaždice")


## <a name="working-with-tmx-files"></a>Práce se soubory .tmx

Formát souboru .tmx je soubor XML vytvořené aplikací vedle sebe, což může být [stáhnout zdarma na webu vedle sebe](http://www.mapeditor.org/). Formát souboru .tmx ukládá informace o dlaždice map. Obvykle hry, bude mít jeden .tmx soubor pro každou oblast úrovně nebo samostatné.

Tato příručka se zaměřuje na tom, jak používat existující soubory .tmx ve CocosSharp; však další kurzy najdete online, včetně [tohoto úvodu do editoru mapy vedle sebe](http://gamedevelopment.tutsplus.com/tutorials/introduction-to-tiled-map-editor--gamedev-2838).

Budete muset rozbalte [soubor zip obsahu](https://github.com/xamarin/mobile-samples/blob/master/BouncingGame/Resources/Tiled.zip?raw=true) pro použití v našem hra. Nejprve si všimněte si, je, že dlaždice map pomocí obou .tmx souboru (dungeon.tmx) a také jeden nebo více soubory obrázků, které definují dlaždici nastavit data (dungeon_1.png). Herní musí obsahovat oba tyto soubory se načíst .tmx za běhu, proto přidat do projektu **obsahu** složky (která je součástí **prostředky** složku v projektech pro Android). Konkrétně přidat soubory do složky s názvem **tilemaps** uvnitř **obsahu** složky:

![](tiled-images/image5.png "Přidání souborů do složky s názvem tilemaps uvnitř složky obsahu")

**Dungeon.tmx** za běhu do můžete nyní načíst soubor `CCTileMap` objektu. V dalším kroku změnit `GameLayer` (nebo ekvivalentní kořenový objekt kontejneru) tak, aby obsahovala `CCTileMap` instance a přidejte jej jako podřízenou:


```csharp
public class GameLayer : CCLayer
{
    CCTileMap tileMap;

    public GameLayer ()
    {
    }

    protected override void AddedToScene ()
    {
        base.AddedToScene ();

        tileMap = new CCTileMap ("tilemaps/dungeon.tmx");
        this.AddChild (tileMap);
    }
} 
```

Pokud jsme spustit hru, kterou jsme se zobrazí mapy dlaždice se objeví v levém dolním rohu obrazovky:

![](tiled-images/image6.png "Pokud hra běží, zobrazí mapování dlaždice v okraje v levém dolním rohu obrazovky")


## <a name="considerations-for-rendering-pixel-art"></a>Důležité informace pro vykreslování obrázky pixelů

Obrázky pixelů, v rámci video herní vývoje, odkazuje na 2D visual obrázky, která se obvykle vytvoří pomocí ruční a je často nízkým rozlišením. Obrázky pixelů může být restriktivně náročné vytvořit, tak často patří s nízkým rozlišením dlaždice, například 16 nebo 32 pixelů šířka a výška pixelů obrázky dlaždice sady času. Pokud není škálovat za běhu, je často příliš malá pro většina moderních telefonů a tabletů obrázky pixelů.

Jsme můžete upravit zobrazené dimenze v našem herní GameAppDelegate.cs souboru, kde přidáme volání `CCScene.SetDefaultDesignResolution`:


```csharp
 public override void ApplicationDidFinishLaunching (CCApplication application, CCWindow mainWindow)
{
    application.PreferMultiSampling = false;
    application.ContentRootDirectory = "Content";
    application.ContentSearchPaths.Add ("animations");
    application.ContentSearchPaths.Add ("fonts");
    application.ContentSearchPaths.Add ("sounds");

    CCSize windowSize = mainWindow.WindowSizeInPixels;

    float desiredWidth = 1024.0f;
    float desiredHeight = 768.0f;
    
    // This will set the world bounds to be (0,0, w, h)
    // CCSceneResolutionPolicy.ShowAll will ensure that the aspect ratio is preserved
    CCScene.SetDefaultDesignResolution (desiredWidth, desiredHeight, CCSceneResolutionPolicy.ShowAll);
    
    // Determine whether to use the high or low def versions of our images
    // Make sure the default texel to content size ratio is set correctly
    // Of course you're free to have a finer set of image resolutions e.g (ld, hd, super-hd)
    if (desiredWidth < windowSize.Width)
    {
        application.ContentSearchPaths.Add ("images/hd");
        CCSprite.DefaultTexelToContentSizeRatio = 2.0f;
    }
    else
    {
        application.ContentSearchPaths.Add ("images/ld");
        CCSprite.DefaultTexelToContentSizeRatio = 1.0f;
    }

    // New code:
    CCScene.SetDefaultDesignResolution (380, 240, CCSceneResolutionPolicy.ShowAll);

    CCScene scene = new CCScene (mainWindow);
    GameLayer gameLayer = new GameLayer ();

    scene.AddChild (gameLayer);

    mainWindow.RunWithScene (scene);
} 
```

Další informace o `CCSceneResolutionPolicy`, najdete v našem návodu na [zpracování řešení v CocosSharp](~/graphics-games/cocossharp/resolutions.md).

Pokud jsme teď spustit hru, uvidíme herní proveďte zálohu celé obrazovky naše zařízení:

![](tiled-images/image7.png "Herní proveďte zálohu celé obrazovky zařízení")

Nakonec jsme budete chtít zakázat vyhlazení na našem dlaždice map. `Antialiased` Vlastnost se týká rozostření vliv při vykreslování objekty, které jsou přiblížení. Vyhlazení může být užitečné při snižování pixelován vzhled grafických objektů, ale může také přinést vlastní vykreslování artefakty. Konkrétně vyhlazení rozostří obsah každou dlaždici. Však nejsou hranice okrajů každou dlaždici, takže jednotlivé dlaždice zvýraznění místo prolnutí pomocí sousedících dlaždice. Také je třeba poznamenat, hry obrázky pixelů často zachovat jejich vzhled pixelován udržovat *retro* působí.

Nastavit `Antialiased` k `false` poté, co vytvořen `tileMap`:


```csharp
protected override void AddedToScene ()
{
    base.AddedToScene ();

    tileMap = new CCTileMap ("tilemaps/dungeon.tmx");

    // new code:
    tileMap.Antialiased = false;

    this.AddChild (tileMap);
} 
```

Nyní se nezobrazí naše dlaždice map rozmazaně:

![](tiled-images/image8.png "Nyní se nezobrazí dlaždice map rozmazaně")


## <a name="using-tile-properties-at-runtime"></a>Pomocí dlaždice vlastnosti za běhu

Pokud budeme mít `CCTileMap` načítání .tmx souboru a jeho, zobrazení, ale žádný způsob, jak pracovat s ním. Konkrétně některé dlaždice (například naše poklad hrudníku) musí být vlastní logiky. Jsme projdete kroky k zjištění vlastnosti vlastní dlaždice a různé způsoby, jak reagovat na tyto vlastnosti jednou identifikovat za běhu.

Před jsme psaní jakéhokoli kódu, budeme potřebovat k přidání vlastností do našich dlaždice map prostřednictvím vedle sebe. Chcete-li to provést, otevřete soubor dungeon.tmx v programu vedle sebe. Ujistěte se, že otevřete soubor, který se používá v herní projektu.

Jakmile otevřete, vyberte prsou poklad v dlaždici nastavit a zobrazte její vlastnosti:

![](tiled-images/image9.png "Jakmile otevřete, vyberte prsou poklad na dlaždici nastavit a zobrazte její vlastnosti")

Pokud poklad hrudníku vlastností se nezobrazí, klikněte pravým tlačítkem na prsou poklad a vyberte **dlaždice vlastnosti**:

![](tiled-images/image10.png "Pokud poklad hrudníku vlastností se nezobrazí, klikněte pravým tlačítkem na prsou poklad a vyberte dlaždici vlastnosti")

Název a hodnotu s jsou implementované vlastnosti vedle sebe. Přidání vlastnosti, klikněte na tlačítko **+** tlačítko, zadejte název **IsTreasure**, klikněte na tlačítko **OK**, pak zadejte hodnotu **true**: 

![](tiled-images/image11.png "Přidání vlastnosti, klikněte na tlačítko, zadejte název IsTreasure, klikněte na tlačítko OK a pak zadejte hodnotu true")

Nezapomeňte si uložte soubor .tmx po úpravě vlastností.

Nakonec přidáme kódu a hledat v našem nově přidané vlastnosti. Jsme se projít každý `CCTileMapLayer` (naše mapy má vrstvy 2), pak prostřednictvím jednotlivých řádků a sloupců, které chcete vyhledat všechny dlaždice, které mají `IsTreasure` vlastnost:


```csharp
public class GameLayer : CCLayer
{
    CCTileMap tileMap;

    public GameLayer ()
    {
    }

    protected override void AddedToScene ()
    {
        base.AddedToScene ();

        tileMap = new CCTileMap ("tilemaps/dungeon.tmx");

        // new code:
        tileMap.Antialiased = false;

        this.AddChild (tileMap);

        HandleCustomTileProperties (tileMap);
    }

    void HandleCustomTileProperties(CCTileMap tileMap)
    {
        // Width and Height are equal so we can use either
        int tileDimension = (int)tileMap.TileTexelSize.Width;

        // Find out how many rows and columns are in our tile map
        int numberOfColumns = (int)tileMap.MapDimensions.Size.Width;
        int numberOfRows = (int)tileMap.MapDimensions.Size.Height;

        // Tile maps can have multiple layers, so let's loop through all of them:
        foreach (CCTileMapLayer layer in tileMap.TileLayersContainer.Children)
        {
            // Loop through the columns and rows to find all tiles
            for (int column = 0; column < numberOfColumns; column++)
            {
                // We're going to add tileDimension / 2 to get the position
                // of the center of the tile - this will help us in 
                // positioning entities, and will eliminate the possibility
                // of floating point error when calculating the nearest tile:
                int worldX = tileDimension * column + tileDimension / 2;
                for (int row = 0; row < numberOfRows; row++)
                {
                    // See above on why we add tileDimension / 2
                    int worldY = tileDimension * row + tileDimension / 2;

                    HandleCustomTilePropertyAt (worldX, worldY, layer);
                }
            }
        }
    }

    void HandleCustomTilePropertyAt(int worldX, int worldY, CCTileMapLayer layer)
    {
        CCTileMapCoordinates tileAtXy = layer.ClosestTileCoordAtNodePosition (new CCPoint (worldX, worldY));

        CCTileGidAndFlags info = layer.TileGIDAndFlags (tileAtXy.Column, tileAtXy.Row);

        if (info != null)
        {
            Dictionary<string, string> properties = null;

            try
            {
                properties = tileMap.TilePropertiesForGID (info.Gid);
            }
            catch
            {
                // CocosSharp 
            }

            if (properties != null && properties.ContainsKey ("IsTreasure") && properties["IsTreasure"] == "true" )
            {
                layer.RemoveTile (tileAtXy);

                // todo: Create a treasure chest entity
            }
        }
    }
} 
```

Většinu kódu je není potřeba vysvětlovat, ale probereme zpracování poklad dlaždic. V takovém případě jsme se odebrat všechny dlaždice, které jsou označeny jako poklad beden. To je proto poklad beden bude pravděpodobně nutné vlastního kódu v době běhu kolizí vliv a zadávají přehrávač obsahu poklad při otevření. Kromě toho poklad muset reagovat na právě otevřené (změna jeho vzhled) a může mít logiku pro jen při zobrazování všechny obrazovce nepřátel byly nepotlačí.

Jinými slovy, bude prsou poklad těžit z se entity a místo se jednoduché dlaždice v `CCTileMap`. Další informace o herní entity, najdete v článku [Průvodce entity v CocosSharp](~/graphics-games/cocossharp/entities.md).


## <a name="summary"></a>Souhrn

Tento návod popisuje jak načíst .tmx soubory vytvořené serverem vedle sebe na CocosSharp aplikaci. Zobrazuje postup úpravy aplikace řešení, pokud je účet pro obrázky nízkým rozlišením pixelů a jak najít dlaždice podle jejich vlastností k provedení vlastní logiky, jako je vytváření instancí entit.

## <a name="related-links"></a>Související odkazy

- [Vedle sebe webu](http://www.mapeditor.org/)
- [Obsahu zip](https://github.com/xamarin/mobile-samples/blob/master/BouncingGame/Resources/Tiled.zip?raw=true)
