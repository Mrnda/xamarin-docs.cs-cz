---
title: Mince herní podrobnosti času
description: Tato příručka popisuje podrobnosti implementace mince čas hry, včetně práce s dlaždice map, vytváření entit, animace Sprite a implementace efektivní kolizí.
ms.prod: xamarin
ms.assetid: 5D285684-0417-4E16-BD14-2D1F6DEFBB8B
author: charlespetzold
ms.author: chape
ms.date: 03/24/2017
ms.openlocfilehash: 2687b1c8eca8cfb68660c8a622278aa628459d07
ms.sourcegitcommit: 0a72c7dea020b965378b6314f558bf5360dbd066
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 05/09/2018
ms.locfileid: "33921934"
---
# <a name="coin-time-game-details"></a>Mince herní podrobnosti času

_Tato příručka popisuje podrobnosti implementace mince čas hry, včetně práce s dlaždice map, vytváření entit, animace Sprite a implementace efektivní kolizí._

Čas mince je úplné platformer herní pro iOS a Android. Cílem hry je pak oslovit dvířek ukončení a snižuje nepřátel a překážek a shromažďovat všechny mincí v úrovní.

![](cointime-images/image1.png "Cílem hry je shromažďování všech mincí v na úrovni a potom dosáhnout dvířek ukončení a snižuje nepřátel a překážek")

Tato příručka popisuje podrobnosti implementace v průběhu času mince, která obsahuje následující témata:

- [Práce se soubory tmx](#working-with-tmx-files)
- [Načítání na úrovni](#level-loading)
- [Přidání nové entity](#adding-new-entities)
- [Animovaný entity](#animated-entities)


## <a name="content-in-coin-time"></a>V čase mince obsahu

Čas mince je ukázkový projekt, který představuje může uspořádání úplné CocosSharp projektu. Mince časové struktury cílem zjednodušit přidání a správa obsahu. Používá **.tmx** soubory vytvořené serverem [vedle sebe](http://www.mapeditor.org) pro úrovně a soubory XML k definování animace. Úprava nebo přidání nového obsahu lze dosáhnout s minimálním úsilím. 

Když tento přístup může dobu mince projektu efektivní pro učení a experimentování, také odráží jak professional hry jsou vytvářeny. Tato příručka vysvětluje některé přístupů zjednodušena přidání a úpravy obsahu.


## <a name="working-with-tmx-files"></a>Práce se soubory tmx

Mince čas úrovně jsou definovány pomocí .tmx formát souboru, který je výstupem [vedle sebe](http://www.mapeditor.org) dlaždice map editor. Podrobnou diskuzi o práci s vedle sebe, najdete v článku [pomocí rozložen formou dlaždic s Kokosové Sharp průvodce](~/graphics-games/cocossharp/tiled.md). 

Každá úroveň je definována v vlastního souboru .tmx obsažené v **CoinTime nebo prostředky nebo obsah nebo úrovně** složky. Všechny úrovně mince čas sdílet jeden soubor tileset, která je definována v **mastersheet.tsx** souboru. Tento soubor definuje vlastní vlastnosti pro každou dlaždici, například jestli dlaždici má plnou kolizí nebo jestli by měla být nahrazená dlaždici instance entity. Soubor mastersheet.tsx umožňuje vlastnosti, které chcete být definovaný jenom jednou a použít napříč všechny úrovně. 


### <a name="editing-a-tile-map"></a>Úpravy dlaždice map

Chcete-li upravit mapu dlaždice, otevřete soubor .tmx v vedle sebe dvojitým kliknutím na soubor .tmx nebo ho otevřít pomocí nabídky souboru v vedle sebe. Čas mince úrovně dlaždice map obsahovat tři vrstvy: 

- **Entity** – tato vrstva obsahuje dlaždice, které se nahradí instance entit za běhu. Mezi příklady patří přehrávač, mincí, nepřátelé a end úroveň dvířka.
- **Geologické struktury** – tato vrstva obsahuje dlaždice, které obvykle mají plnou kolizí. Plnou kolizí umožňuje player vás na tyto dlaždice bez návratem. Dlaždice s plnou kolizí mohou chovat i jako bariéry a mezní hodnoty.
- **Pozadí** – vrstva pozadí obsahuje dlaždice, které se používají jako statické pozadí. Tato vrstva neposouvá se při fotoaparát přesune v rámci úrovně vytváření hloubku prostřednictvím paralaxy.

Jak se podíváme na později, kód úroveň načítání předpokládá, že tyto tři vrstvy na všech úrovních mince čas.

#### <a name="editing-terrain"></a>Úpravy geologické struktury

Dlaždice je možné použít kliknutím v **mastersheet** tileset a kliknutím na dlaždici mapy. Chcete-li například malovat nové geologické struktury v úrovní:

1. Vyberte vrstvu, geologické struktury
1. Klikněte na dlaždici kreslení
1. Klikněte na tlačítko nebo push a přetáhněte ji přes mapy pro malování dlaždice

    ![](cointime-images/image2.png "Klikněte na dlaždici k vykreslení 1")

Vlevo nahoře tileset obsahuje všechny geologické struktury v čase mince. Zahrnuje geologické struktury, což je plná, **SolidCollision** vlastnost, jak je vidět ve vlastnostech dlaždice na levé straně obrazovky:

![](cointime-images/image3.png "Geologické struktury, což je plná, zahrnuje vlastnost SolidCollision, jak je vidět ve vlastnostech dlaždice na levé straně obrazovky")

#### <a name="editing-entities"></a>Úpravy entit

Entity můžete přidat nebo odebrat z úrovně – stejně jako geologické struktury. **Mastersheet** tileset obsahuje všechny entity umístit o polovině vzdálenosti ve vodorovném směru, takže nemusí být zobrazeny bez posunutí doprava:

![](cointime-images/image4.png "Mastersheet tileset obsahuje všechny entity umístit o polovině vzdálenosti ve vodorovném směru, takže nemusí být zobrazeny bez posunutí doprava")

Nové entity, musí být umístěny na **entity** vrstvy.

![](cointime-images/image5.png "Nové entity, musí být umístěny na vrstvě entity")

Kód CoinTime vyhledá **EntityType** při načtení úrovní k identifikaci dlaždice, které by měl být nahrazen entity: 

![](cointime-images/image6.png "Kód CoinTime vyhledá EntityType při načtení úrovní k identifikaci dlaždice, které by měla být nahrazená entitami")

Jakmile soubor má byla upravena a uložili, změny se automaticky zobrazí-li projekt sestavit a spustit:

![](cointime-images/image7.png "Jakmile se soubor má byla upravena a uložili, změny se automaticky zobrazí vytvořené a spusťte projekt")


### <a name="adding-new-levels"></a>Přidání nové úrovně

Proces přidávání úrovně mince čas vyžaduje beze změn kódu a pouze několik malých změn do projektu. Chcete-li přidat novou úroveň:

1. Otevřete složku úrovně umístěné v <**CoinTime kořenové > \CoinTime\Assets\Content\levels**
1. Zkopírujte a vložte jednu z úrovní, jako například **level0.tmx**
1. Přejmenujte soubor nové .tmx tak, aby pokračuje úrovně číslo pořadí s existující úrovně, jako například **level8.tmx**
1. V sadě Visual Studio nebo Visual Studio pro Mac přidejte nový soubor .tmx ke složce Android úrovně. Ověřte, že soubor používá **AndroidAsset** akce sestavení.

    ![](cointime-images/image8.png "Ověřte, že soubor používá AndroidAsset akce sestavení")

1. Přidáte nový soubor .tmx do složky úrovně iOS. Nezapomeňte propojení souboru z původního umístění a ověřte, že používá **BundleResource** akce sestavení.

    ![](cointime-images/image9.png "Nezapomeňte propojení souboru z původního umístění a ověřte, že používá akci BundleResource sestavení")

Novou úroveň měly být zobrazeny na obrazovce vyberte úroveň na úroveň 9 (úroveň názvy souborů začínají hodnotou 0, ale tlačítka úrovně začínat číslem 1):

![](cointime-images/image10.png "Novou úroveň měly být zobrazeny na obrazovce vyberte úroveň na úroveň 9 úrovni souboru názvy start na 0, ale tlačítka úrovně začínat číslem 1")


## <a name="level-loading"></a>Načítání na úrovni

Jak je uvedeno výše, nové úrovně vyžadují žádné změny v kódu – hra automaticky rozpozná úrovně, pokud jsou správně s názvem a přidat do **úrovně** složku s akce správná sestavení (**BundleResource**nebo **AndroidAsset**).

Logika pro určení počet úrovní, které je součástí `LevelManager` třídy. Pokud instance `LevelManager` sestavený (s použitím vzoru singleton), `DetermineAvailbleLevels` metoda je volána:


```csharp
private void DetermineAvailableLevels()
{
    // This game relies on levels being named "levelx.tmx" where x is an integer beginning with
    // 1. We have to rely on MonoGame's TitleContainer which doesn't give us a GetFiles method - we simply
    // have to check if a file exists, and if we get an exception on the call then we know the file doesn't
    // exist. 
    NumberOfLevels = 0;
    while (true)
    {
        bool fileExists = false;
        try
        {
            using(var stream = TitleContainer.OpenStream("Content/levels/level" + NumberOfLevels + ".tmx"))
            {
            }
            // if we got here then the file exists!
            fileExists = true;
        }
        catch
        {
            // do nothing, fileExists will remain false
        }
        if (!fileExists)
        {
            break;
        }
        else
        {
            NumberOfLevels++;
        }
    }
}
```

CocosSharp neposkytuje přístup a platformy pro zjišťování, pokud soubory k dispozici, takže jsme muset spoléhat na `TitleContainer` třída pokusí otevřít datový proud. Pokud kód pro otevření datového proudu, vyvolá výjimku, pak soubor neexistuje a chvíli cykly zalomení. Po dokončení smyčky `NumberOfLevels` vlastnost sestavy, kolik úrovní do platné jsou součástí projektu.

`LevelSelectScene` Třídy používá `LevelManager.NumberOfLevels` k určení, kolik tlačítka pro vytvoření v `CreateLevelButtons` metoda:


```csharp
private void CreateLevelButtons()
{
    const int buttonsPerPage = 6;
    int levelIndex0Based = buttonsPerPage * pageNumber;
    int maxLevelExclusive = System.Math.Min (levelIndex0Based + 6, LevelManager.Self.NumberOfLevels);
    int buttonIndex = 0;
    float centerX = this.ContentSize.Center.X;
    const float topRowOffsetFromCenter = 16;
    float topRowY = this.ContentSize.Center.Y + topRowOffsetFromCenter;
    for (int i = levelIndex0Based; i < maxLevelExclusive; i++)
    {
        ...
    }
}
```

`NumberOflevels` Vlastnost se používá k určení tlačítek, která by měla být vytvořena. Tento kód zvažuje stránky, která je aktuálně zobrazení a pouze vytvoří nesmí být delší než šest tlačítka na stránce uživatele. Po kliknutí na tlačítko instance volání `HandleButtonClicked` metoda:


```csharp
private void HandleButtonClicked(object sender, EventArgs args)
{
    // levelNumber is 1-based, so subtract 1:
    var levelIndex = (sender as Button).LevelNumber - 1;
    LevelManager.Self.CurrentLevel = levelIndex;
    CoinTime.GameAppDelegate.GoToGameScene ();
}
```

Tato metoda přiřadí `CurrentLevel` vlastnost, která je používána `GameScene` při načítání úrovní. Po nastavení `CurrentLevel`, `GoToGameScene` vyvolá metoda přepínání scény z `LevelSelectScene` k `GameScene`.

`GameScene` Volá konstruktor `GoToLevel`, která provede logice úroveň načítání:


```csharp
private void GoToLevel(int levelNumber)
{
    LoadLevel (levelNumber);
    CreateCollision();
    ProcessTileProperties ();
    touchScreen = new TouchScreenInput(gameplayLayer);
    secondsLeft = secondsPerLevel;
}
```

Další provedeme podívejte se na metody volal v `GoToLevel`.


### <a name="loadlevel"></a>LoadLevel

`LoadLevel` Metoda zodpovídá za načítání souboru .tmx a jejím přidáním do `GameScene`. Tato metoda nevytvoří žádné objekty interaktivní třeba kolizí nebo entity – jednoduše vytvoří vizuály úrovně, které jsou také označovány jako *prostředí*.


```csharp
private void LoadLevel(int levelNumber)
{
    currentLevel = new CCTileMap ("level" + levelNumber + ".tmx");
    currentLevel.Antialiased = false;
    backgroundLayer = currentLevel.LayerNamed ("Background");
    // CCTileMap is a CCLayer, so we'll just add it under all entities
    this.AddChild (currentLevel);
    // put the game layer after
    this.RemoveChild(gameplayLayer);
    this.AddChild(gameplayLayer);
    this.RemoveChild (hudLayer);
    this.AddChild (hudLayer);
}
```

`CCTileMap` Konstruktor přebírá název souboru, který je vytvořený pomocí číslo úrovně předané do `LoadLevel`. Další informace o vytváření a práci s `CCTileMap` instancí, najdete v článku [pomocí rozložen formou dlaždic s CocosSharp průvodce](~/graphics-games/cocossharp/tiled.md).

V současné době CocosSharp nepovoluje změny pořadí vrstev bez odebrání a znovu je přidáte do své nadřazené `CCScene` (což je `GameScene` v tomto případě), takže posledních několika řádků metody jsou potřeba změnit pořadí vrstev.


### <a name="createcollision"></a>CreateCollision

`CreateCollision` Metoda konstrukce `LevelCollision` instanci, která se používá k provádění *plnou kolizí* mezi player a prostředí.


```csharp
private void CreateCollision()
{
    levelCollision = new LevelCollision();
    levelCollision.PopulateFrom(currentLevel);
}
```

Bez této kolizí přehrávač by přejít na úrovni a hra by možné přehrát. Plnou kolizí umožňuje player provede na místě a brání player z s návodem bariéry nebo přechod až prostřednictvím mezní hodnoty.

Kolizí v čase mince lze přidat s žádný další kód – pouze změny vedle sebe soubory. 


### <a name="processtileproperties"></a>ProcessTileProperties

Jakmile je načten úrovní a kolizí je vytvořen, `ProcessTileProperties` je volána k provedení logiku podle vlastnosti dlaždice. Zahrnuje mince čas `PropertyLocation` struktura k definování vlastností a souřadnice dlaždici s těmito vlastnostmi:


```csharp
public struct PropertyLocation
{
    public CCTileMapLayer Layer;
    public CCTileMapCoordinates TileCoordinates;
    public float WorldX;
    public float WorldY;
    public Dictionary<string, string> Properties;
}
```

Tato struktura se používá pro konstrukci vytváření instancí entit a odebrání nepotřebných dlaždice v `ProcessTileProperties` metoda:


```csharp
private void ProcessTileProperties()
{
    TileMapPropertyFinder finder = new TileMapPropertyFinder (currentLevel);
    foreach (var propertyLocation in finder.GetPropertyLocations())
    {
        var properties = propertyLocation.Properties;
        if (properties.ContainsKey ("EntityType"))
        {
            float worldX = propertyLocation.WorldX;
            float worldY = propertyLocation.WorldY;
            if (properties.ContainsKey ("YOffset"))
            {
                string yOffsetAsString = properties ["YOffset"];
                float yOffset = 0;
                float.TryParse (yOffsetAsString, out yOffset);
                worldY += yOffset;
            }
            bool created = TryCreateEntity (properties ["EntityType"], worldX, worldY);
            if (created)
            {
                propertyLocation.Layer.RemoveTile (propertyLocation.TileCoordinates);
            }
        }
        else if (properties.ContainsKey ("RemoveMe"))
        {
            propertyLocation.Layer.RemoveTile (propertyLocation.TileCoordinates);
        }
    }
}
```

Smyčka typu foreach vyhodnotí každou vlastnost dlaždice, kontrolu, pokud je klíč `EntityType` nebo `RemoveMe`. `EntityType` Označuje, že by měl být vytvářeny entity instance. `RemoveMe` Označuje, že na dlaždici měla by být úplně odebrána za běhu.

Pokud vlastnost s `EntityType` je-li nalezen klíč, pak `TryCreateEntity` je volána, které se pokouší vytvořit entitu pomocí odpovídající vlastnost `EntityType` klíč:


```csharp
private bool TryCreateEntity(string entityType, float worldX, float worldY)
{
    CCNode entityAsNode = null;
    switch (entityType)
    {
    case "Player":
        player = new Player ();
        entityAsNode = player;
        break;
    case "Coin":
        Coin coin = new Coin ();
        entityAsNode = coin;
        coins.Add (coin);
        break;
    case "Door":
        door = new Door ();
        entityAsNode = door;
        break;
    case "Spikes":
        var spikes = new Spikes ();
        this.damageDealers.Add (spikes);
        entityAsNode = spikes;
        break;
    case "Enemy":
        var enemy = new Enemy ();
        this.damageDealers.Add (enemy);
        this.enemies.Add (enemy);
        entityAsNode = enemy;
        break;
    }
    if(entityAsNode != null)
    {
        entityAsNode.PositionX = worldX;
        entityAsNode.PositionY = worldY;
        gameplayLayer.AddChild (entityAsNode);
    }
    return entityAsNode != null;
}
```


## <a name="adding-new-entities"></a>Přidání nové entity

Čas mince využívá vzor entity pro jeho herní objekty (která je popsaná v [Průvodce entity v CocosSharp](~/graphics-games/cocossharp/entities.md)). Dědí všechny entity `CCNode`, což znamená, mohou být přidány jako podřízené objekty `gameplayLayer`.

Každému typu entity, se taky odkazuje přímo prostřednictvím seznamu nebo jediné instance. Například `Player` odkazuje `player` pole a všechny `Coin` instancí se odkazuje v `coins` seznamu. Zachování přímé odkazy na entity (a odkazy na ně prostřednictvím `gameLayer.Children` seznamu) umožňuje kódu, který přistupuje k tyto entity snadněji číst a eliminuje potenciálně nákladné typ přetypování.

Existující kód poskytuje několik typů entit jako příklady, jak vytvořit nové entity. Následující postup slouží k vytvoření nové entity:


### <a name="1---define-a-new-class-using-the-entity-pattern"></a>1 - definujte novou třídu pomocí vzoru entity

Jediný požadavek pro vytvoření entity se má vytvořit třídu, která dědí z `CCNode`. Většina entity, jako mají některé visual `CCSprite`, který má být přidán jako podřízený entity v jeho konstruktoru.

Poskytuje CoinTime `AnimatedSpriteEntity` třída, která zjednodušuje vytváření animovaný entit. Animace se budeme v podrobněji [animovaný entity části](#animated-entities).


### <a name="2--add-a-new-entry-to-the-trycreateentity-switch-statement"></a>2 – přidáte novou položku do TryCreateEntity příkaz switch

Instance nové entity by měla být vytvořena instance v `TryCreateEntity`. Pokud entita vyžaduje každých rámce logiku jako kolizí, AI nebo čtení vstup, pak se `GameScene` musí zachovat odkaz na objekt. Potřeby více instancí (například `Coin` nebo `Enemy` instance), pak nový `List` musí být přidaní do `GameScene` – třída.


### <a name="3--modify-tile-properties-for-the-new-entity"></a>3 – upravte vlastnosti dlaždice pro novou entitu

Po kód podporuje vytváření nové entity, musí být přidán do tileset novou entitu. Tileset lze upravit tak, že otevřete všechny úrovně `.tmx` souboru. 

Je uložený tileset odděleně od .tmx v **mastersheet.tsx** souboru, takže je nutné importovat před se dá upravit:

![](cointime-images/image11.png "Je uložený tileset samostatné ze souboru .tsx, musí být naimportovány, než se dá upravit")

Po importu vlastnosti vybrané dlaždice se upravovat a EntityType lze přidat:

![](cointime-images/image12.png "Po importu vlastnosti vybrané dlaždice se upravovat, a přidáním EntityType")

Po vytvoření vlastnost jeho hodnota se dá nastavit tak, aby odpovídala nové `case` v `TryCreateEntity`:

![](cointime-images/image13.png "Po vytvoření vlastnost jeho hodnota se dá nastavit tak, aby odpovídala nové velká písmena v TryCreateEntity")

Po provedení změny tileset, musí být exportován – díky změny k dispozici pro všechny ostatní úrovně:

![](cointime-images/image14.png "Po provedení změny tileset, musí být exportován")

Tileset měli přepsat existující **mastersheet.tsx** tileset:

![](cointime-images/image15.png "Použí tileset měli přepsat existující tileset mastersheet.tsx")


## <a name="entity-tile-removal"></a>Odebrání dlaždice entity

Pokud dlaždice map je načten do hry, jsou jednotlivé dlaždice statické objekty. Vzhledem k tomu, že entity vyžadují vlastní chování, například přesun, kódu v době mince odebere dlaždice při vytvoření entity.

`ProcessTileProperties` obsahuje logiku pro odebrání dlaždice, které vytvoří entity pomocí `RemoveTile` metoda:


```csharp
private void ProcessTileProperties()
{
    TileMapPropertyFinder finder = new TileMapPropertyFinder (currentLevel);
    foreach (var propertyLocation in finder.GetPropertyLocations())
    {
        var properties = propertyLocation.Properties;
        if (properties.ContainsKey ("EntityType"))
        {
            ...
            bool created = TryCreateEntity (properties ["EntityType"], worldX, worldY);
            if (created)
            {
                propertyLocation.Layer.RemoveTile (propertyLocation.TileCoordinates);
            }
        }
        ...
    }
}
```

Toto automatické odebrání dlaždice je dostačující pro entity, které zabírají pouze jeden dlaždice v tileset, jako je například mincí a nepřátelům. Větší entity vyžadují další logiku a vlastnosti.

Dvířka vyžaduje dvě dlaždice, které se mají vykreslovat úplně:

![](cointime-images/image16.png "Dvířka vyžaduje dvě dlaždice, které se mají vykreslovat úplně")

Na dlaždici dolní v dvířka obsahuje vlastnosti pro vytvoření entity (**EntityType** nastavena na **dveře**):

![](cointime-images/image17.png "Na dlaždici dolní v dvířka obsahuje vlastnosti pro vytvoření EntityType nastavena na hodnotu dveře entity")

Vzhledem k tomu, že po se vytvoří instance dveře je odebrán pouze dolní dlaždice v dvířka, další logiku navíc je potřeba k odebrání dlaždici nejvyšší za běhu. Má nejvyšší dlaždice **RemoveMe** vlastnost nastavena na hodnotu **true**:

![](cointime-images/image18.png "Horní dlaždice má RemoveMe vlastnost nastavena na hodnotu true")



Tato vlastnost se používá k odebrání dlaždice v `ProcessTileProperties`:


```csharp
private void ProcessTileProperties()
{
    TileMapPropertyFinder finder = new TileMapPropertyFinder (currentLevel);
    foreach (var propertyLocation in finder.GetPropertyLocations())
    {
        var properties = propertyLocation.Properties;
        ...
        else if (properties.ContainsKey ("RemoveMe"))
        {
            propertyLocation.Layer.RemoveTile (propertyLocation.TileCoordinates);
        }
    }
}
```


## <a name="entity-offsets"></a>Posuny entity

Entity vytvořené z dlaždice jsou umístěny podle zarovnání na střed dlaždici centru entity. Větší entity, jako například `Door`, použijte další vlastnosti a logiku správně umístit. 

Dolní dveře dlaždice, který definuje `Door` Určuje umístění entity **YOffset** hodnotu 4. Bez této vlastnosti `Door` instance je umístěn na střed dlaždice:

![](cointime-images/image19.png "Bez této vlastnosti je umístěn instance dveře v centru dlaždice")

 

To je opraví použitím **YOffset** hodnotu `ProcessTileProperties`:


```csharp
private void ProcessTileProperties()
{
    TileMapPropertyFinder finder = new TileMapPropertyFinder (currentLevel);
    foreach (var propertyLocation in finder.GetPropertyLocations())
    {
        var properties = propertyLocation.Properties;
        if (properties.ContainsKey ("EntityType"))
        {
            float worldX = propertyLocation.WorldX;
            float worldY = propertyLocation.WorldY;
            if (properties.ContainsKey ("YOffset"))
            {
                string yOffsetAsString = properties ["YOffset"];
                float yOffset = 0;
                float.TryParse (yOffsetAsString, out yOffset);
                worldY += yOffset;
            }
            bool created = TryCreateEntity (properties ["EntityType"], worldX, worldY);
            ...
        }
...
    }
}
```


## <a name="animated-entities"></a>Animovaný entity

Čas mince obsahuje několik animovaný entity. `Player` a `Enemy` entity přehrávat animace procházení a `Door` entity hraje animace otevírání po všech mincí byly shromážděny.


### <a name="achx-files"></a>.achx soubory

Animace čas mince jsou definovány v .achx soubory. Každý animace je definována mezi `AnimationChain` značky, jak je znázorněno v následujícím animace definované v **propanimations.achx**:


```xml
<AnimationChain>
  <Name>Spikes</Name>
  <ColorKey>0</ColorKey>
  <Frame>
    <FlipHorizontal>false</FlipHorizontal>
    <FlipVertical>false</FlipVertical>
    <TextureName>..\images\mastersheet.png</TextureName>
    <FrameLength>0.1</FrameLength>
    <LeftCoordinate>1152</LeftCoordinate>
    <RightCoordinate>1168</RightCoordinate>
    <TopCoordinate>128</TopCoordinate>
    <BottomCoordinate>144</BottomCoordinate>
    <RelativeX>0</RelativeX>
    <RelativeY>0</RelativeY>
  </Frame>
</AnimationChain> 
```

Tato animace obsahuje pouze jeden snímek, výsledkem je zobrazení statický obrázek Špička entity. Entity můžete použít soubory .achx, zda se zobrazí jeden nebo více rámce animace. Další snímky mohou být přidány do .achx soubory bez nutnosti změny v kódu. 

Rámce definovat která image zobrazíte v `TextureName` parametr a souřadnice obrazovky v `LeftCoordinate`, `RightCoordinate`, `TopCoordinate`, a `BottomCoordinate` značky. Tyto představovat souřadnice pixelů rámečku animace do bitové kopie, který je používán – **mastersheet.png** v tomto případě.

![](cointime-images/image20.png "Tyto představovat souřadnice pixelů rámečku animace v bitové kopii")

`FrameLength` Vlastnost definuje počet sekund, po které má být zobrazena rámce v animace. Animací jedním jednotlivých ignorovat tuto hodnotu.

Všechny ostatní vlastnosti AnimationChain v souboru .achx ignorují mince doby.


### <a name="animatedspriteentity"></a>AnimatedSpriteEntity

Animace logiku, je součástí `AnimatedSpriteEntity` třídy, která slouží jako základní třída pro většiny entit používané v `GameScene`. Poskytuje následující funkce:

 - Načítání `.achx` soubory
 - Animace mezipaměť načíst animace
 - Instance CCSprite pro zobrazení animace
 - Logika pro změnu aktuálního snímku

Konstruktor špičky nabízí jednoduchý příklad toho, jak načíst a použít animací:


```csharp
public Spikes ()
{
    LoadAnimations ("Content/animations/propanimations.achx");
    CurrentAnimation = animations [0];
}
```

**PropAnimations.achx** tak konstruktoru přistupuje tento animace index obsahuje pouze jeden animace. Pokud soubor .achx obsahuje více animací, pak můžete odkazovat animací podle názvu, jak je znázorněno v `Enemy` konstruktor:


```csharp
walkLeftAnimation = animations.Find (item => item.Name == "WalkLeft");
walkRightAnimation = animations.Find (item => item.Name == "WalkRight");
```


## <a name="summary"></a>Souhrn

Tento průvodce popisuje podrobnosti implementace mince času. Mince čas je vytvořen jako dokončení hry, avšak je také projekt, který lze snadno upravit a rozšířit. Přidání nové úrovně a vytvoření nové entity, abyste pochopili, jak je implementována mince času se doporučuje tráví čas provedení změny úrovně čtečky.

## <a name="related-links"></a>Související odkazy

- [Herní projektu (ukázka)](https://developer.xamarin.com/samples/mobile/CoinTime/)
