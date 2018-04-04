---
title: Herní podrobnosti ovocný spadá
description: Tento průvodce zkontroluje hra Fruity spadá pokrývajících běžné CocosSharp a vývoj her koncepty například fyziky, správy obsahu, herní stavu a herní návrhu.
ms.prod: xamarin
ms.assetid: A5664930-F9F0-4A08-965D-26EF266FED24
ms.technology: xamarin-cross-platform
author: charlespetzold
ms.author: chape
ms.date: 03/27/2017
ms.openlocfilehash: 2138e27c1097e014f302cc0b9de0fa411bed89fc
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/04/2018
---
# <a name="fruity-falls-game-details"></a>Herní podrobnosti ovocný spadá

_Tento průvodce zkontroluje hra Fruity spadá pokrývajících běžné CocosSharp a vývoj her koncepty například fyziky, správy obsahu, herní stavu a herní návrhu._

Ovocný spadá je jednoduché, na základě fyziky hra, ve kterém přehrávač seřadí červené a žlutý výsledek do barevnou kbelíků získat bodů. Cílem hry je získat tolik bodů bez upozornění pokles výsledek do nesprávného bin ukončení příslušnou hru.

![](fruity-falls-images/image1.png "Cílem hry je získat tolik bodů bez upozornění pokles výsledek do nesprávného bin ukončení hry")

Ovocný spadá rozšiřuje koncepty představené v [BouncingGame průvodce](~/graphics-games/cocossharp/bouncing-game.md) přidáním následující:

 - Obsah ve formátu PNG
 - Pokročilé fyziky
 - Herní stavu (přechod mezi scény)
 - Možnost vyladění herní koeficienty prostřednictvím proměnné, které jsou součástí jedné třídy
 - Integrovanou podporu visual ladění
 - Kód organizace pomocí herní entity
 - Herní zaměřuje na fun a opětovného přehrání hodnotou

Když [BouncingGame průvodce](~/graphics-games/cocossharp/bouncing-game.md) zaměřuje na základní koncepty CocosSharp představení, Fruity spadá ukazuje, jak a převeďte ho do všech společně do dokončení herní produktu. Vzhledem k tomu, že tato příručka odkazuje BouncingGame, čteček by se měli nejdřív seznámit [BouncingGame průvodce](~/graphics-games/cocossharp/bouncing-game.md) před přečtení tohoto průvodce.

Tento průvodce pokrývá implementaci a návrhu Fruity spadá do nabízejí přehled, který vám pomůže provádět vlastní herní. Pokrývá v následujících tématech:


- [Herní – třída](#gamecontroller-class)
- [Herní entity](#game-entities)
- [Výsledek grafiky](#fruit-graphics)
- [Fyziky](#physics)
- [Herní obsah](#game-content)
- [GameCoefficients](#gamecoefficients)


## <a name="gamecontroller-class"></a>Herní – třída

Tento projekt ovocný spadá PCL zahrnuje `GameController` třída, která je odpovědná za vytváření instancí hry a přesun mezi scény. Tato třída se používá k iOS a Android projekty k vyloučení duplicitní kódu.

Kód obsažené v `Initialize` metoda je podobný kódu v`Initialize` metoda v šabloně CocosSharp beze změny, ale obsahuje řadu úpravy.

Ve výchozím nastavení `GameView.ContentManager.SearchPaths` závisí na řešení zařízení. Jak je popsáno níže podrobněji, Fruity spadá používá stejný obsah, bez ohledu na zařízení se řešit, proto `Initialize` metoda přidá `Images` cestu (ne v podsložkách) k `SearchPaths`:


```csharp
contentSearchPaths.Add ("Images");
```

Nové šablony CocosSharp zahrnují třídu, která dědí z `CCLayer`, zdání herní vizuály a logiku musí být přidaní do této třídy. Místo toho Fruity spadá používá více `CCLayer` instance na ovládací prvek kreslení pořadí. Tyto `CCLayer` instance jsou obsaženy v `GameView` třídy, která dědí z `CCScene`. Ovocný spadá také zahrnuje několik scény, první probíhá `TitleScene`. Proto `Initialize` metoda vytvoří instanci `TitleScene` instanci, která je předána `RunWithScene`:


```csharp
var scene = new TitleScene (GameView);
GameView.Director.RunWithScene (scene);
```

Obsah pro Fruity spadá byla vytvořena jako obrázky s nízkým rozlišením pixelů estetických důvodů. `GameView.DesignResolution` Je nastaven tak, aby hra pouze 384 jednotky široké a 512 talovat:


```csharp
// We use a lower-resolution display to get a pixellated appearance
int width = 384;
int height = 512;
GameView.DesignResolution = new CCSizeI (width, height); 
```

Nakonec `GameController` třída poskytuje statickou metodu, která lze volat žádné `CCGameScene` pro přechod na jiný `CCScene`. Tato metoda se používá k přesunu mezi `TitleScene` a `GameScene`.


## <a name="game-entities"></a>Herní entity

Ovocný spadá využívá vzor entity pro většinu herní objektů. Podrobné vysvětlení tohoto vzoru lze nalézt v [Průvodce entity v CocosSharp](~/graphics-games/cocossharp/entities.md).

Implementace entity herní objekty naleznete ve složce entity v **FruityFalls.Common** projektu:

![](fruity-falls-images/image2.png "Složka entity v projektu FruityFalls.Common")

Entity jsou objekty, které dědí `CCNode`a může mít vizuály, kolizí a chování každých rámce.

`Fruit` Objekt představuje typické entitu CocosSharp: obsahuje vizuální objekt, kolizí a logiku každých rámce. Jeho konstruktoru zodpovídá za inicializaci výsledek:


```csharp
public Fruit ()
{
    CreateFruitGraphic ();
    if (GameCoefficients.ShowCollisionAreas)
    {
        CreateDebugGraphic ();
    }
    CreateCollision ();
    CreateExtraPointsLabel ();
    Acceleration.Y = GameCoefficients.FruitGravity;
}
```


### <a name="fruit-graphics"></a>Výsledek grafiky

`CreateFruitGraphic` Metoda vytvoří `CCSprite` instance a přidává ji k `Fruit`. `IsAntialiased` Je nastavena na hodnotu false umožnit hra pixelován vzhled. Tato hodnota nastavena na hodnotu false na všech `CCSprite` a `CCLabel` instance hodnota hry:


```csharp
private void CreateFruitGraphic()
{
    graphic = new CCSprite ("cherry.png");
    graphic.IsAntialiased = false;
    this.AddChild (graphic);
}
```

Vždy, když přehrávač dotykem `Fruit` instanci s `Paddle`, hodnotu bodu tento výsledek zvýší o 1. To poskytuje další výzvy k zkušeného hráči na přehlednější výsledek pro další body. Bod hodnotu výsledek se zobrazí pomocí `extraPointsLabel` instance.

`CreateExtraPointsLabel` Metoda vytvoří `CCLabel` instance a přidává ji k `Fruit`:


```csharp
private void CreateExtraPointsLabel()
{
    extraPointsLabel = new CCLabel("", "Arial", 12, CCLabelFormat.SystemFont);
    extraPointsLabel.IsAntialiased = false;
    extraPointsLabel.Color = CCColor3B.Black;
    this.AddChild(extraPointsLabel);
}
```

Ovocný spadá obsahuje dva různé typy výsledek – třešně a citrony. `CreateFruitGraphic` Přiřadí výchozí visual, ale vizuály výsledek lze změnit pomocí `FruitColor` vlastnost, která volá `UpdateGraphics`:


```csharp
private void UpdateGraphics()
{
if (GameCoefficients.ShowCollisionAreas)
    {
        debugGrahic.Clear ();
        const float borderWidth = 4;
        debugGrahic.DrawSolidCircle (
            CCPoint.Zero,
            GameCoefficients.FruitRadius,
            CCColor4B.Black);
        debugGrahic.DrawSolidCircle (
            CCPoint.Zero,
            GameCoefficients.FruitRadius - borderWidth,
            fruitColor.ToCCColor ());
    }
    if (this.FruitColor == FruitColor.Yellow)
    {
        graphic.Texture = CCTextureCache.SharedTextureCache.AddImage ("lemon.png");
        extraPointsLabel.Color = CCColor3B.Black;
        extraPointsLabel.PositionY = 0;
    }
    else
    {
        graphic.Texture = CCTextureCache.SharedTextureCache.AddImage ("cherry.png");
        extraPointsLabel.Color = CCColor3B.White;
        extraPointsLabel.PositionY = -8;
    }
}
```

První příkaz if v `UpdateGraphics` aktualizuje ladění grafiky, které se používají k vizualizaci kolizí oblasti. Tyto vizuální prvky jsou vypnuty před finální verzi nástroje hra se provádí, ale je možné mít během vývoje pro ladění fyziky. Druhá část změny `graphic.Texture` vlastnost voláním `CCTextureCache.SharedTextureCache.AddImage`. Tato metoda poskytuje přístup k texturou podle názvu souboru. Další informace o této metody můžete najít v [ukládání do mezipaměti Texture průvodce](~/graphics-games/cocossharp/texture-cache.md).

`extraPointsLabel` Barva objektů je upravena zachovat kontrast výsledek Image a jeho `PositionY` hodnota je upravena Center `CCLabel` na výsledek `CCSprite`:

![](fruity-falls-images/image3.png "Barva extraPointsLabel objektů je upravena zachovat kontrast výsledek Image a jeho PositionY hodnota je upravena na střed CCLabel na plodů CCSprite")

![](fruity-falls-images/image4.png "Barva extraPointsLabel objektů je upravena zachovat kontrast výsledek Image a jeho PositionY hodnota je upravena na střed CCLabel na plodů CCSprite")


### <a name="collision"></a>Kolizí

Ovocný spadá implementuje pomocí objekty ve složce geometrie vlastní kolizí řešení:

![](fruity-falls-images/image5.png "Ovocný spadá implementuje pomocí objekty ve složce geometrie řešení vlastní kolizí")

Kolizí v Fruity spadá je implementovaná pomocí buď `Circle` nebo `Polygon` objekt, obvykle s jedním z těchto dvou typů přidávané jako podřízenou entitu. Například `Fruit` objekt má `Circle` názvem `Collision` které se inicializuje v jeho `CreateCollision` metoda:


```csharp
private void CreateCollision()
{
    Collision = new Circle ();
    Collision.Radius = GameCoefficients.FruitRadius;
    this.AddChild (Collision);
}
```

Všimněte si, že `Circle` třída dědí z `CCNode` třídy, tak jej lze přidat jako podřízenou naše herní entity:


```csharp
public class Circle : CCNode
{
    ...
}
```

`Polygon` vytvoření je podobná `Circle` vytváření, jak je znázorněno `Paddle` – třída:


```csharp
private void CreateCollision()
{
    Polygon = Polygon.CreateRectangle(width, height);
    this.AddChild (Polygon);
}
```

Je zahrnuté logiky kolizí [dál v této příručce](#collision).


## <a name="physics"></a>Fyziky

Je možné oddělit fyziky v Fruity spadá do dvou kategorií: pohyb a kolizí. 


### <a name="movement-using-velocity-and-acceleration"></a>Přesun pomocí rychlost a akcelerace

Používá ovocný spadá `Velocity` a `Acceleration` hodnoty k ovládání jeho entit, podobně jako [BouncingGame](~/graphics-games/cocossharp/bouncing-game.md). Entity implementovat logiku jejich přesun metodu s názvem `Activity`, která je vyvolána jednou za snímek. Například uvidíme implementace přesun `Fruit` třída `Activity` metoda:

```csharp
public void Activity(float frameTimeInSeconds)
{
    timeUntilExtraPointsCanBeAdded -= frameTimeInSeconds;
    
    // linear approximation:
    this.Velocity += Acceleration * frameTimeInSeconds;

    // This is a linear approximation to drag. It's used to
    // keep the object from falling too fast, and eventually
    // to slow its horizontal movement. This makes the game easier
    this.Velocity -= Velocity * GameCoefficients.FruitDrag * frameTimeInSeconds;
    
    this.Position += Velocity * frameTimeInSeconds;
}
```
`Fruit` Objektu je jedinečné v jeho implementaci přetažení – hodnota, což zpomalí rychlosti ve vztahu k tom, jak rychlý výsledek je přesunutí. Tato implementace přetáhněte poskytuje *terminálu rychlosti*, což je maximální rychlost, kterou může vrátit výsledek instance. Přetáhněte také zpomaluje Vodorovný pohyb výsledek, který usnadňuje hra mírně přehrávání.

`Paddle` Objekt platí také `Velocity` v jeho `Activity` metoda, ale její `Velocity` se počítá pomocí `desiredLocation` hodnotu:


```csharp
public void Activity(float frameTimeInSeconds)
{
    // This code will cause the cursor to lag behind the touch point
    // Increasing this value reduces how far the paddle lags
    // behind the player’s finger. 
    const float velocityCoefficient = 4;
    // Get the velocity from current location and touch location
    Velocity = (desiredLocation - this.Position) * velocityCoefficient;
    this.Position += Velocity * frameTimeInSeconds;
    ...
}
```

Obvykle, hry, které používají `Velocity` k řízení polohy jejich objekty proveďte není přímo nastavit entity pozic alespoň není po inicializaci. Místo nastavení přímo `Paddle` pozici, `Paddle.HandleInput` metoda přiřadí `desiredLocation` hodnotu:

```csharp
public void HandleInput(CCPoint touchPoint)
{
    desiredLocation = touchPoint;
}
```

### <a name="collision"></a>Kolizí

Ovocný spadá implementuje polovičním realistické kolizí mezi výsledek a jiné collidable objekty, jako `Paddle` a `GameScene.Splitter`. Pomoc při ladění kolizí, Fruity spadá kolizí oblasti lze zviditelnit změnou `GameCoefficients.ShowDebugInfo` v `GameCoefficients.cs` souboru:


```csharp
public static class GameCoefficients
{
    ... 
    public const bool ShowCollisionAreas = true;
    ...
}
```

Nastavení této hodnoty na `true` umožňuje vykreslování kolizí oblastí:  

![](fruity-falls-images/image6.png "Nastavení této hodnoty na hodnotu true umožňuje vykreslování kolizí oblastí")

Logika kolizí začíná v `GameScene.Activity` metoda:


```csharp
private void Activity(float frameTimeInSeconds)
{
    if (hasGameEnded == false)
    {
        paddle.Activity(frameTimeInSeconds);
        foreach (var fruit in fruitList)
        {
            fruit.Activity(frameTimeInSeconds);
        }
        spawner.Activity(frameTimeInSeconds);
        DebugActivity();
        PerformCollision();
    }
}
```

`PerformCollision` zpracovává všechny kolize `Fruit` instancí s jinými objekty: 


```csharp
private void PerformCollision()
{
    // reverse for loop since fruit may be destroyed:
    for(int i = fruitList.Count - 1; i > -1; i--)
    {
        var fruit = fruitList[i];
        FruitVsPaddle(fruit);
        FruitPolygonCollision(fruit, splitter.Polygon, CCPoint.Zero);
        FruitVsBorders(fruit);
        FruitVsBins(fruit);
    }
}
```

#### <a name="fruitvsborders"></a>FruitVsBorders

`FruitVsBorders` kolizí provede vlastní logiky kolizí namísto spoléhání na logiku obsažené v jinou třídu. Tento rozdíl existuje, protože není perfektně plné kolizí mezi výsledek a na obrazovce ohraničení – je možné pro výsledek na poslat vypnout okraje obrazovky podle pečlivě desku pohyb. Výsledek bude kolísají z obrazovky při průchodu s desku, ale pokud přehrávač pomalu nabízených oznámení výsledek se přesune okraj a z obrazovky:


```csharp
private void FruitVsBorders(Fruit fruit)
{
    if(fruit.PositionX - fruit.Radius < 0 && fruit.Velocity.X < 0)
    {
        fruit.Velocity.X *= -1 * GameCoefficients.FruitCollisionElasticity;
    }
    if(fruit.PositionX + fruit.Radius > gameplayLayer.ContentSize.Width && fruit.Velocity.X > 0)
    {
        fruit.Velocity.X *= -1 * GameCoefficients.FruitCollisionElasticity;
    }
    if(fruit.PositionY + fruit.Radius > gameplayLayer.ContentSize.Height && fruit.Velocity.Y > 0)
    {
        fruit.Velocity.Y *= -1 * GameCoefficients.FruitCollisionElasticity;
    }
}
```

#### <a name="fruitvsbins"></a>FruitVsBins

`FruitVsBins` Metoda odpovídá za kontrole, jestli se všechny výsledek klesla do jedné ze dvou přihrádky. Pokud ano, přehrávač poskytnut body (Pokud je výsledek/bin barvy shoda) nebo hra končí (Pokud barvy neshodují):


```csharp
private void FruitVsBins(Fruit fruit)
{
    foreach(var bin in fruitBins)
    {
        if(bin.Polygon.CollideAgainst(fruit.Collision))
        {
            if(bin.FruitColor == fruit.FruitColor)
            {
                // award points:
                score += 1 + fruit.ExtraPointValue;
                scoreText.Score = score;
                Destroy(fruit);
            }
            else
            {
                EndGame();
            }
            break;
        }
    }
}
```

#### <a name="fruitvspaddle-and-fruitpolygoncollision"></a>FruitVsPaddle a FruitPolygonCollision

Výsledek, oproti desku a výsledek oproti rozdělovače (oblasti oddělujících dvě přihrádek) kolizí oba tyto nástroje spoléhají na `FruitPolygonCollision` metoda. Tato metoda má tři části:

1. Otestovat, zda je v konfliktu objekty
1. Přesun objektů (právě výsledek v Fruity spadá), tak, aby už překrývala.
1. Upravte rychlosti objektů (právě výsledek v Fruity spadá) pro simulaci vrátit odesílateli následující kód ukazuje body přidali výše:


```csharp
private static bool FruitPolygonCollision(Fruit fruit, Polygon polygon, CCPoint polygonVelocity)
{
    // Test whether the fruit collides
    bool didCollide = polygon.CollideAgainst(fruit.Collision);
    if (didCollide)
    {
        var circle = fruit.Collision;
        // Get the separation vector to reposition the fruit so it doesn't overlap the polygon
        var separation = CollisionResponse.GetSeparationVector(circle, polygon);
        fruit.Position += separation;
        // Adjust the fruit's Velocity to make it bounce:
        var normal = separation;
        normal.Normalize();
        fruit.Velocity = CollisionResponse.ApplyBounce(
            fruit.Velocity, 
            polygonVelocity, 
            normal, 
            GameCoefficients.FruitCollisionElasticity);
    }
    return didCollide;
}
```

Odpověď ovocný spadá kolizí je jednostranně – pouze výsledek rychlost a pozice se upraví. Je vhodné poznamenat, že jiné hry může mít kolizí, což ovlivňuje oba objekty související se situací, jako je znak vkládání do sebe navzájem boulder nebo dvě aut chybám.

Výpočty za logice kolizí součástí `Polygon` a `CollisionResponse` třídy je nad rámec tohoto průvodce, ale jako zapsána, tyto metody lze použít pro mnoho typů her. Mnohoúhelníku a `CollisionResponse` třídy i podporují obdélníkový a konvexní mnohoúhelníky, takže tento kód můžete použít pro mnoho typů her.

 


## <a name="game-content"></a>Herní obsah

Obrázky v Fruity spadá okamžitě odlišuje od BouncingGame příslušnou hru. Herní návrhů jsou podobné, přehrávače se okamžitě zobrazí rozdíl v vzhled dvě hry. Hráči často rozhodněte, jestli pokusit hry s jeho vizuály. Proto je životně důležité, aby vývojáři investovat prostředky při vytvoření přitažlivé herní.

Obrázky pro Fruity spadá byl vytvořen sledovat tyto cíle:

 - Konzistentní motiv
 - Důraz na právě jeden znak – opic Xamarin
 - Dojde k vytvoření uvolnění zábavná jasně barvy
 - Kontrastu mezi popředí a na pozadí, takže se dají snadno sledovat vizuální objekty hraní her
 - Schopnost vytvářet jednoduché vizuálních efektů bez zdroj těžký animace


### <a name="content-location"></a>Umístění obsahu

Ovocný spadá zahrnuje veškerý jeho obsah ve složce bitové kopie v projektu pro Android:

![](fruity-falls-images/image7.png "Zahrnuje veškerý jeho obsah ovocný spadá do složky bitových kopií ve projektu pro Android")

Tyto stejné soubory, které jsou propojeny v projektu iOS předejdete duplikace a Ano změny souborů ovlivnit obou projektů:

![](fruity-falls-images/image8.png "Tyto stejné soubory, které jsou propojeny v projektu iOS předejdete duplikace a Ano změny souborů ovlivnit obou projektů")

Je vhodné poznamenat, že obsah není obsažen v rámci **Ld** nebo **Hd** složkách, které jsou součástí výchozí šablony CocosSharp. **Ld** a **Hd** složky jsou určena k použití pro hry, které poskytují dvě sady obsahu – jeden pro zařízení s nízkým rozlišením, jako jsou telefony a jeden pro zařízení, vyšší řešení, jako jsou tablety. Obrázky Fruity spadá je záměrně vytvořen s pixelován estetické, takže není nutné pro poskytování obsahu pro jiné velikosti obrazovky. Proto **Ld** a **Hd** složky byla úplně odebraná z projektu.


### <a name="gamescene-layering"></a>GameScene vrstvení

Jak je uvedeno výše v této příručce `GameScene` zodpovídá za všechny herní objekt konkretizaci, umisťování a logiku mezi objekty (například kolizí). Všechny objekty jsou přidány do jednoho z čtyři `CCLayer` instancí:


```csharp
CCLayer backgroundLayer;
CCLayer gameplayLayer;
CCLayer foregroundLayer;
CCLayer hudLayer;
```

Tyto čtyři vrstvy, které jsou vytvořené v `CreateLayers` metoda. Všimněte si, že budou přidány do `GameScene` v pořadí podle zpět do popředí:


```csharp
private void CreateLayers()
{
    backgroundLayer = new CCLayer();
    this.AddLayer(backgroundLayer);
    gameplayLayer = new CCLayer();
    this.AddLayer(gameplayLayer);
    foregroundLayer = new CCLayer();
    this.AddLayer(foregroundLayer);
    hudLayer = new CCLayer();
    this.AddLayer(hudLayer);
}
```

Po vytvoření vrstvy, které jsou všechny vizuální objekty jsou přidány do vrstvy pomocí `AddChild` metoda, jak je znázorněno `CreateBackground` metoda:


```csharp
private void CreateBackground()
{
    var background = new CCSprite("background.png");
    background.AnchorPoint = new CCPoint(0, 0);
    background.IsAntialiased = false;
    backgroundLayer.AddChild(background);
}
```


### <a name="vine-entity"></a>Révy entity

`Vine` Entity jednoznačně slouží pro obsah – nemá žádný vliv na hraní her. Skládá se z dvacet `CCSprite` instance, číslo vybraná omyl a zajistit révový vždy dosáhne horní části obrazovky:


```csharp
public Vine ()
{
    const int numberOfVinesToAdd = 20;
    for (int i = 0; i < numberOfVinesToAdd; i++)
    {
        var graphic = new CCSprite ("vine.png");
        graphic.AnchorPoint = new CCPoint(.5f, 0);
        graphic.PositionY = i * graphic.ContentSize.Height;
        this.AddChild (graphic);
    }
}
```

První `CCSprite` je nastavený tak pozici zbytku odpovídá jeho dolní hrany. To se provádí nastavením AnchorPoint na `new CCPoint(.5f, 0)`. `AnchorPoint` Používá vlastnost *normalized souřadnice* které rozsahu mezi 0 a 1 bez ohledu na velikost `CCSprite`:

![](fruity-falls-images/image9.png "Vlastnost AnchorPoint pomocí normalizované souřadnice které rozsahu mezi 0 a 1 bez ohledu na velikost CCSprite")

Následné révy Sprite jsou umístěny pomocí `graphic.ContentSize.Height` hodnotu, která vrátí výšku obrázku v pixelech. Následující diagram znázorňuje, jak na obrázku révy dlaždice směrem nahoru:

![](fruity-falls-images/image10.png "Tento diagram znázorňuje, jak na obrázku révy dlaždice směrem nahoru")

Od počátku `Vine` entita je dole zbytku a pak ho umístění je jasné, jak je znázorněno v `Paddle.CreateVines` metoda:


```csharp
private void CreateVines()
{
    // Increasing this value moves the vines closer to the 
    // center of the paddle.
    const int vineDistanceFromEdge = 4;
    leftVine = new Vine ();
    var leftEdge = -width / 2.0f;
    leftVine.PositionX = leftEdge + vineDistanceFromEdge;
    this.AddChild (leftVine);
    rightVine = new Vine ();
    var rightEdge = width / 2.0f;
    rightVine.PositionX = rightEdge - vineDistanceFromEdge;
    this.AddChild (rightVine);
}
```

Révový instancí v `Paddle` entita také mít zajímavé otočení chování. Vzhledem k tomu, `Paddle` entity jako celek otočí podle player vstup (který tento průvodce popisuje podrobněji níže), `Vine` instance je také postiženo tento otočení. Ale otáčení `Vine` instance spolu s `Paddle` vytváří nežádoucího vizuální efekt, jak je znázorněno v následujícím animace:

![](fruity-falls-images/image11.gif "Však otáčení instancí révy společně s desku vytváří nežádoucího visual vliv jak je znázorněno v následujícím animace")

K vyvážení `Paddle` otáčení `leftVine` a `rightVine` instance otáčejí proti směru desku, jak je znázorněno v `Paddle.Activity` metoda:


```csharp
public void Activity(float frameTimeInSeconds)
{
    ...
    // This value adds a slight amount of rotation
    // to the vine. Having a small amount of tilt looks nice.
    float vineAngle = this.Velocity.X / 100.0f;
    leftVine.Rotation = -this.Rotation + vineAngle;
    rightVine.Rotation = -this.Rotation + vineAngle;
}
```

Všimněte si, zda je přidána malé množství otočení zpět do révy prostřednictvím `vineAngle` koeficient. Tuto hodnotu můžete změnit na upravit, kolik keřů otočit.


## <a name="gamecoefficients"></a>GameCoefficients

Každých dobré hry je produkt iterace, takže Fruity spadá zahrnuje třídy s názvem `GameCoefficients` který určuje, jak se přehrávají hra. Tato třída obsahuje výrazovou proměnné, které se používají v rámci HRA do ovládacího prvku fyziky, rozložení, například a vyhodnocování.

Například fyziky výsledek jsou řízeny následující proměnné:


```csharp
public static class GameCoefficients
{
    ...
    
    // The strength of the gravity. Making this a 
    // smaller (bigger negative) number will make the
    // fruit fall faster. A larger (smaller negative) number
    // will make the fruit more floaty.
    public const float FruitGravity = -60;
    // The impact of "air drag" on the fruit, which gives
    // the fruit terminal velocity (max falling speed) and slows
    // the fruit's horizontal movement (makes the game easier)
    public const float FruitDrag = .1f;
    // Controls fruit collision bouncyness. A value of 1
    // means no momentum is lost. A value of 0 means all
    // momentum is lost. Values greater than 1 create a spring-like effect
    public const float FruitCollisionElasticity = .5f;
    
    ...
}
```

Jako názvy implikují, tyto proměnné lze použít k úpravě jak rychlé výsledek spadá, jak vodorovné přesun zpomaluje přes čas a desku bounciness.

Herní koeficienty uložené v kódu nebo datové soubory (například XML) lze obzvlášť užitečný pro týmy s herní návrhářů, kteří nejsou i programátoři.

`GameCoefficients` Třída také obsahuje hodnoty pro zapnutí ladicí informace, jako například kolizí nebo informace o oblastech:


```csharp
public static class GameCoefficients
{
    ...
    // This controls whether debug information is displayed on screen.
    public const bool ShowDebugInfo = false;
    public const bool ShowCollisionAreas = false;
    ...
}
```


## <a name="conclusion"></a>Závěr

Tato příručka prozkoumali herní Fruity spadá. Koncepty, včetně obsahu, fyziky a správy stavu herní o něm zmínka.

## <a name="related-links"></a>Související odkazy

- [Dokumentace CocosSharp rozhraní API](https://developer.xamarin.com/api/namespace/CocosSharp/)
- [Dokončené projektu (ukázka)](https://developer.xamarin.com/samples/mobile/FruityFalls/)
