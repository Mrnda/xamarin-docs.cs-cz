---
title: Podrobnosti o BouncingGame
description: Tento návod ukazuje, jak implementovat jednoduché skákající hry míč s použitím CocosSharp.
ms.prod: xamarin
ms.assetid: AC9FD56F-6E4A-40DA-8168-45A761D869FD
author: charlespetzold
ms.author: chape
ms.date: 03/29/2018
ms.openlocfilehash: d12d6fb8ecfcba5e5093b2af4790a51ef8cf8e47
ms.sourcegitcommit: 0a72c7dea020b965378b6314f558bf5360dbd066
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 05/09/2018
---
# <a name="bouncinggame-details"></a>Podrobnosti o BouncingGame

> [!TIP]
> Kód pro BouncingGame naleznete v [Xamarin ukázky](https://developer.xamarin.com/samples/mobile/BouncingGame/). Chcete-li nejlépe postupovat podle této příručky, stáhněte a prozkoumat kód jako průvodci odkazuje.

Tento návod ukazuje, jak implementovat jednoduché skákající hry míč s použitím CocosSharp. 

 - Dekomprimace herní obsah
 - Běžné CocosSharp vizuální prvky
 - Vizuální prvky pro přidání `GameLayer`
 - Implementace logiku každých rámce

Naše dokončení herní bude vypadat takto:

![BouncingGame, Dokončit](bouncing-game-images/image1.png "BouncingGame, byla dokončena")

## <a name="unzipping-game-content"></a>Dekomprimace herní obsah

Herní vývojáři často používají termín *obsah* odkazovat na jiný kód soubory, které jsou obvykle vytvořené pomocí visual umělci, herní Designer nebo zvuk Designer. Běžné typy obsahu, obsahovat soubory použít k zobrazení vizuální prvky, přehrát zvuk nebo řízení chování umělé intelligence (AI). Z hlediska herní vývojový tým obsah obvykle je vytvořena bez programátory. Naše herní obsahuje dva typy obsahu:

 - soubory PNG definovat, jak míč a desku nezobrazí.
 - Jeden xnb soubor, který chcete definovat písmo použité skóre zobrazení (podrobněji popsána při přidáme skóre zobrazení níže)

Tento obsah použít se zde naleznete v [obsahu zip](https://github.com/xamarin/mobile-samples/blob/master/BouncingGame/Resources/Content.zip?raw=true). Tyto soubory stažené a rozbalené do umístění, které jsme budou přistupovat k dále v tomto návodu budeme potřebovat.

## <a name="common-cocossharp-visual-elements"></a>Běžné CocosSharp vizuální prvky

CocosSharp poskytuje několik tříd, které slouží k zobrazení vizuální prvky. Některé prvky jsou přímo viditelná, zatímco jiné jsou používány pro organizaci. Hry, použijeme následující:

 - `CCNode` – Základní třída pro všechny objekty visual v CocosSharp. `CCNode` Třída obsahuje `AddChild` funkci, která slouží k vytvoření hierarchie nadřazených a podřízených také označuje jako *vizuálním stromu* nebo *scény grafu*. Dědí všechny třídy uvedené níže `CCNode`.
 - `CCScene` – Kořen stromu visual pro všechny hry CocosSharp. Všechny vizuální prvky, musí být součástí vizuálním stromu s `CCScene` v kořenovém adresáři, jinak nebudou viditelné.
 - `CCLayer` – Kontejner pro visual objekty, jako například `CCSprite`. Jak již název napovídá, `CCLayer` třída se používá k řízení jak visual elementy vrstvy na sebe.
 - `CCSprite` – Zobrazí bitovou kopii nebo část obrázku. `CCSprite` instance může být umístěný, změnit a zadat počet vizuálních efektů.
 - `CCLabel` – Zobrazí řetězec na obrazovce. Písmo použité `CCLabel` je definován v souboru xnb. Probereme xnbs podrobněji níže.

Abyste pochopili, jak se používají různé typy popíšeme jedné `CCSprite`. Vizuální prvky, musí být přidaný do `CCLayer`, a musí mít vizuálním stromu `CCScene` na svůj kořen. Proto relaci nadřazený podřízený pro jeden `CCSprite` by `CCScene`  >  `CCLayer`  >  `CCSprite`.

## <a name="adding-visual-elements-to-gamelayer"></a>Přidávání do GameLayer vizuální prvky

### <a name="adding-the-paddle-sprite"></a>Přidání pohyblivý symbol desku

Nejdřív vytvoříme pozadí ve hře začernit a také přidat jeden `CCSprite` vykreslování na obrazovce. Změnit `GameLayer` třída přidat `CCSprite` následujícím způsobem:

```csharp
using System;
using System.Collections.Generic;
using CocosSharp;
namespace BouncingGame
{
    public class GameLayer : CCLayer
    {
        CCSprite paddleSprite;
        public GameLayer () : base(CCColor4B.Black)
        {
            // "paddle" refers to the paddle.png image
            paddleSprite = new CCSprite ("paddle");
            paddleSprite.PositionX = 100;
            paddleSprite.PositionY = 100;
            AddChild (paddleSprite);
        }
        protected override void AddedToScene ()
        {
            base.AddedToScene ();
            // Use the bounds to layout the positioning of the drawable assets
            CCRect bounds = VisibleBoundsWorldspace;
            // Register for touch events
            var touchListener = new CCEventListenerTouchAllAtOnce ();
            touchListener.OnTouchesEnded = OnTouchesEnded;
            AddEventListener (touchListener, this);
        }
        void OnTouchesEnded (List<CCTouch> touches, CCEvent touchEvent)
        {
            if (touches.Count > 0)
            {
                // Perform touch handling here
            }
        }
    }
}
```

Výše uvedený kód vytvoří jednu `CCSprite` a přidá ji jako podřízený `GameLayer`. `CCSprite` Konstruktor umožňuje zadat soubor obrázku, který se použije jako řetězec. Naše kód informuje o tom CocosSharp a hledat v souboru s názvem `paddle` (rozšíření je vynechán, který probereme v této příručce). CocosSharp bude hledat názvy souborů `paddle` v kořenové složce obsahu (což je **obsah**) a také všechny složky přidán do `gameView.ContentManager.SearchPaths` (popsané v předchozí části).

Přidáme soubory přímo do kořenového adresáře **obsahu** složky pro iOS a Android. Chcete-li to provést, klikněte pravým tlačítkem nebo ovládací prvek, klikněte na **obsahu** složky v projektu iOS a vyberte **přidat** > **přidat soubory...** Přejděte do kterých jsme unzipped obsah, dříve a vyberte **paddle.png**. Pokud se zobrazí dotaz, o tom, jak přidat soubor do složky, jsme měli vybrat **kopie** možnost:

![Přidání souboru do složky dialogového okna](bouncing-game-images/image2.png "přidat soubor do složky dialogového okna")

Potom přidáme soubor projektu pro Android. Klikněte pravým tlačítkem nebo ovládací prvek, klikněte na složku obsahu (což je v **prostředky** složky na Android projektů) a vyberte **přidat** > **přidat soubory...** . Tentokrát, přejděte do projektu iOS **obsahu** složky. Dotaz, o tom, jak přidat soubor, vyberte **přidat odkaz** možnost:

![Přidání souboru do složky dialogového okna](bouncing-game-images/addalink.png "přidání souboru do složky dialogového okna")

Probereme, proč soubory měl být přidán do obou projektů níže. Každý projekt **obsahu** složky teď obsahují **paddle.png** souboru:

![Obsah složky obsahu](bouncing-game-images/image3.png "obsah složky obsahu")

Pokud jsme spustit hru ukážeme `CCSprite` přitahuje:

![Desku, na obrazovce vykreslovat](bouncing-game-images/image4.png "desku, vykreslovány na obrazovce")

### <a name="file-details"></a>Podrobnosti souboru

Pokud jeden soubor jste přidali do projektu a proces byl poměrně jednoduché. Jednoduše přidat **paddle.png** do souboru **obsahu** složek a odkaz v kódu. Pojďme se podívat na některé aspekty při práci se soubory v CocosSharp chvíli trvat.

#### <a name="capitalization"></a>malá a velká písmena

Všimněte si, že řetězec jsme použili v kódu pro přístup k souboru a název souboru jsou obě malá. Je to proto, že některé platformy (například simulátoru Windows desktop a iOS) je malá a velká písmena, zatímco jiné platformy (například zařízení Android a iOS) velká a malá písmena. Budeme používat všechny soubory malá pro zbývající část tohoto kurzu tak, aby soubory a kód zůstanou jako napříč platformami míře.

#### <a name="extensions"></a>Rozšíření

Konstruktor pro vytvoření pohyblivý symbol nezahrnuje rozšíření "PNG" při odkazování na soubor desku. Rozšíření pro soubory je obvykle vynechán při práci na projektech CocosSharp jako příponám souborů pro stejný typ prostředku se můžou lišit mezi platformami. Například může být zvukových souborů AIFF, MP3 nebo WMA formáty souborů, v závislosti na platformě. Ponechání vypnout rozšíření umožňuje stejný kód pro práci na všech platformách bez ohledu na příponu souboru.

#### <a name="content-in-platform-specific-projects"></a>Obsah v specifické pro platformu projekty

Na rozdíl od většiny soubory kódu, které mohou být ve PCL obsahu soubory musí být přidány do každého projektu specifické pro platformu. CocosSharp vyžaduje to dvou důvodů:

1. Každá platforma má jiné **akce sestavení**. Byl přidán do projektů iOS obsah by měl používat **BundleResource** akce sestavení. Byl přidán do projektů Android obsah by měl používat **AndroidAsset** akce sestavení.
2. Některé platformy vyžadují formáty souborů specifické pro platformu. Například soubory .xnb písem liší iOS a Android, jak jsme najdete dále v tomto návodu.

Pokud formát souboru neliší mezi platformy (například .png), každou platformu můžete použít ke stejnému souboru, kde jeden nebo více platforem může propojit soubor ze stejného umístění.

## <a name="orientation"></a>orientace

Stejně jako kterákoli jiná aplikace můžete spustit aplikace CocosSharp v orientaci na výšku nebo na šířku. Jsme budete upravit herní ke spuštění v režimu jen na výšku. Nejprve Změníme kód řešení hry pro zpracování poměr stran na výšku. Chcete-li to provést, změňte `width` a `height` hodnoty ve `LoadGame` metoda v `ViewController` – třída v systému iOS a `MainActivity` – třída v systému Android:

```csharp
void LoadGame (object sender, EventArgs e)
{
    CCGameView gameView = sender as CCGameView;

    if (gameView != null)
    {
        var contentSearchPaths = new List<string> () { "Fonts", "Sounds" };
        CCSizeI viewSize = gameView.ViewSize;

        int width = 768;
        int height = 1027;
    // ...
```

Další budeme muset upravit každý projekt specifické pro platformu pro spuštění v režimu na výšku.

### <a name="ios-orientation"></a>orientace iOS

Chcete-li upravit orientaci projekt pro iOS, vyberte **Info.plist** souboru v **BouncingGame.iOS** projektu a změňte **iPhone nebo iPod informace o nasazení** a **iPad informace o nasazení** zahrnout pouze orientace na výšku:

![Možnosti orientace](bouncing-game-images/image5.png "možnosti orientace")

### <a name="android-orientation"></a>Android orientace

Pokud chcete upravit orientaci Android projektu, otevřete soubor MainActivity.cs v projektu BouncingGame.Android. Upravte definici aktivity atribut tak určuje pouze orientaci na výšku, následujícím způsobem:

```csharp
[Activity (
    Label = "BouncingGame.Android",
    AlwaysRetainTaskState = true,
    Icon = "@drawable/icon",
    Theme = "@android:style/Theme.NoTitleBar",
    ScreenOrientation = ScreenOrientation.Portrait,
    LaunchMode = LaunchMode.SingleInstance,
    MainLauncher = true,
    ConfigurationChanges = ConfigChanges.Orientation | ConfigChanges.ScreenSize | ConfigChanges.Keyboard | ConfigChanges.KeyboardHidden)
]
public class MainActivity : Activity
{ 
...
```

## <a name="default-coordinate-system"></a>Výchozí souřadnicový systém

Naše kód, který vytvoří `CCSprite`, nastaví `PositionX` a `PositionY` hodnoty do 100. Ve výchozím nastavení, to znamená, že `CCSprite` center bude být umístěný na 100 pixelů až a doprava zleva dolní části obrazovky. Souřadnicový systém může být upravit připojení `CCCamera` k `CCLayer`. Budeme moci pracovat s `CCCamera` v tomto projektu, ale další informace o `CCCamera` lze nalézt v [dokumentace rozhraní API CocosSharp](https://developer.xamarin.com/api/type/CocosSharp.CCLayer/).

100 pixelů zmíněné "hra" pixelů oproti pixelů na hardware. To znamená, že systémem stejné herní zařízení jiné rozlišení (jako je iPad oproti zařízení typu iPhone) způsobí objekty je umístěný a vzhledem k obrazovce fyzické správnou velikost. Konkrétně oblasti viditelné ve hře bude vždy 1024 pixelů výšku a šířku 768 pixelů, protože se jedná řešení jsme jste zadali dříve v `LoadGame` metoda.

## <a name="adding-the-ball-sprite"></a>Přidání míč pohyblivý symbol

Teď, když jsme jste se seznámili se základy práce s `CCSprite`, přidáme druhý `CCSprite` – míč. Můžeme vám následující kroky, které jsou velmi podobné jak jsme vytvořili desku `CCSprite`. 

Nejprve přidáme **ball.png** bitovou kopii z rozbalené složce, do projektu iOS **obsahu** složky. Vyberte pro **kopie** souboru, který se **obsahu** adresáře. Postupujte podle kroků stejná jako výše a přidejte odkaz **ball.png** souboru v projektu pro Android.

Další vytvoření `CCSprite` u této míč přidáním členem označuje `ballSprite` k `GameScene` třídy a také vytváření instancí kód pro `ballSprite`. Po dokončení `GameLayer` třída bude vypadat například takto:

```csharp
public class GameLayer : CCLayer
{
    CCSprite paddleSprite;
    CCSprite ballSprite;
    public GameLayer () : base (CCColor4B.Black)
    {
        // "paddle" refers to the paddle.png image
        paddleSprite = new CCSprite ("paddle");
        paddleSprite.PositionX = 100;
        paddleSprite.PositionY = 100;
        AddChild (paddleSprite);
        ballSprite = new CCSprite ("ball");
        ballSprite.PositionX = 320;
        ballSprite.PositionY = 600;
        AddChild (ballSprite);
    }
```

## <a name="adding-the-score-label"></a>Přidání popisku skóre

Je posledním elementem visual přidáme hře `CCLabel` zobrazíte počet, kolikrát uživatel má úspěšně míč. `CCLabel` Používá písmo zadaným v konstruktoru pro zobrazení řetězce na obrazovce.

Přidejte následující kód k vytvoření `CCLabel` instance v `GameLayer`. Po dokončení kód by měl vypadat takto:

```csharp
public class GameLayer : CCLayer
{
    CCSprite paddleSprite;
    CCSprite ballSprite;
    CCLabel scoreLabel;
    public GameLayer () : base (CCColor4B.Black)
    {
        // "paddle" refers to the paddle.png image
        paddleSprite = new CCSprite ("paddle");
        paddleSprite.PositionX = 100;
        paddleSprite.PositionY = 100;
        AddChild (paddleSprite);
        ballSprite = new CCSprite ("ball");
        ballSprite.PositionX = 320;
        ballSprite.PositionY = 600;
        AddChild (ballSprite);
        scoreLabel = new CCLabel ("Score: 0", "Arial", 20, CCLabelFormat.SystemFont);
        scoreLabel.PositionX = 50;
        scoreLabel.PositionY = 1000;
        scoreLabel.AnchorPoint = CCPoint.AnchorUpperLeft;
        AddChild (scoreLabel);
    }
    // ...
```

Všimněte si, že je pomocí scoreLabel `AnchorPoint` z `CCPoint.AnchorUpperLeft`, to znamená, že `PositionX` a `PositionY` jeho pozice levého horního definovat hodnoty. To nám umožní pozici `scoreLabel` vůči levému hornímu obrazovky bez nutnosti zvažte jmenovky dimenzí.

## <a name="implementing-every-frame-logic"></a>Implementace logiku každých rámce

Hry dosavadní práce, uvede statické scény. Jsme přidali logiku pro kontrolu pohybu objekty v scény tak, že přidáte kód, který aktualizuje pozici objekty v vysoká frekvence. V takovém případě se kód spustí šedesát časy za sekundu - také označuje jako šedesát *rámce* za sekundu (Pokud hardwaru nemůže zpracovat tento často aktualizace). Konkrétně jsme přidali logiku, aby míč se a kolísají proti desku, chcete-li přesunout desku podle vstup a k aktualizaci skóre hráčů pokaždé, když kuličky narazilo desku.

`Schedule` Metodu, která zajišťuje `CCNode` třídy, umožňují nám přidat logiku každých rámce příslušnou hru. Přidáme kód po `// New code` konstruktoru GameLayer:

```csharp
public GameLayer () : base (CCColor4B.Black)
{
    // "paddle" refers to the paddle.png image
    paddleSprite = new CCSprite ("paddle");
    paddleSprite.PositionX = 100;
    paddleSprite.PositionY = 100;
    AddChild (paddleSprite);
    ballSprite = new CCSprite ("ball");
    ballSprite.PositionX = 320;
    ballSprite.PositionY = 600;
    AddChild (ballSprite);
        scoreLabel = new CCLabel ("Score: 0", "arial", 22, CCLabelFormat.SpriteFont);
    scoreLabel.PositionX = 50;
    scoreLabel.PositionY = 1000;
    scoreLabel.AnchorPoint = CCPoint.AnchorUpperLeft;
    AddChild (scoreLabel);
    // New code:
    Schedule (RunGameLogic);
}
```

Nyní vytvoří `RunGameLogic` metoda v `GameLayer` třídy, která bude obsahovat všechny logice každých rámce:

```csharp
void RunGameLogic(float frameTimeInSeconds)
{
}
```

Parametr float definuje, jak dlouho je rámečku v sekundách. Budeme používat tato hodnota při implementaci míč pohyb.

### <a name="making-the-ball-fall"></a>Provedení míč spadají

Abychom mohli provádět míč spadají buď ručně implementující kódem gravitace nebo pomocí integrované funkce Box2D v CocosSharp. Modul Box2D fyziky simulace je k dispozici CocosSharp hry. Je velmi výkonný a efektivní, ale vyžaduje psaní kódu instalační program. Vzhledem k tomu, že simulace fyziky je docela jednoduchá, zde provádí se ručně.

K implementaci gravitace budeme potřebovat do úložiště aktuální X a Y rychlosti kuličky. Přidáme dva členy `GameLayer` třídy:

```csharp
float ballXVelocity;
float ballYVelocity;
// How much to modify the ball's y velocity per second:
const float gravity = 140;
```

Další můžeme implementovat klesající logiku `RunGameLogic`:

```csharp
void RunGameLogic(float frameTimeInSeconds)
{
    // This is a linear approximation, so not 100% accurate
    ballYVelocity += frameTimeInSeconds * -gravity;
    ballSprite.PositionX += ballXVelocity * frameTimeInSeconds;
    ballSprite.PositionY += ballYVelocity * frameTimeInSeconds;
}
```

### <a name="moving-the-paddle-with-touch-input"></a>Přesunutí desku s dotykovým ovládáním

Teď, když je použit kuličky, přidáme Vodorovný pohyb desku pomocí `CCEventListenerTouchAllAtOnce` objektu. Tento objekt obsahuje počtu událostí související se vstup. V tomto případě chceme se upozornění, že všechny body touch přesunout tak, aby jsme můžete upravit pozici desku. `GameLayer` Již vytvoří `CCEventListenerTouchAllAtOnce`, takže potřebujeme jednoduše přiřadit `OnTouchesMoved` delegovat. Uděláte to tak, změňte metodu AddedToScene následujícím způsobem:

```csharp
protected override void AddedToScene ()
{
    base.AddedToScene ();
    // Use the bounds to layout the positioning of the drawable assets
    CCRect bounds = VisibleBoundsWorldspace;
    // Register for touch events
    var touchListener = new CCEventListenerTouchAllAtOnce ();
    touchListener.OnTouchesEnded = OnTouchesEnded;
    // new code:
    touchListener.OnTouchesMoved = HandleTouchesMoved;
    AddEventListener (touchListener, this);
}
```

Dále budete implementaci `HandleTouchesMoved`:

```csharp
void HandleTouchesMoved (System.Collections.Generic.List<CCTouch> touches, CCEvent touchEvent)
{
    // we only care about the first touch:
    var locationOnScreen = touches [0].Location;
    paddleSprite.PositionX = locationOnScreen.X;
}
```

### <a name="implementing-ball-collision"></a>Implementace míč kolizí

Pokud teď jsme si všimnout, že kuličky spadá přes desku spustíme příslušnou hru. Jsme budete implementovat *kolizí* (logika reagování na překrývající se herní objekty) v kódu každých rámce. Vzhledem k tomu, že změna přesunout objekty umisťuje každý snímek, kontrola kolizí je obvykle také provést každý snímek. Můžeme také přidali rychlosti na ose X při kuličky dotkne desku přidat některé výzvy hře, ale to znamená, že musíme zabránit kuličky pohybující se za okraji obrazovky. Provedeme to `RunGameLogic` kód po rychlosti postupu pro použití `ballSprite` po `// New Code`:

```csharp
void RunGameLogic(float frameTimeInSeconds)
{
    // This is a linear approximation, so not 100% accurate
    ballYVelocity += frameTimeInSeconds * -gravity;
    ballSprite.PositionX += ballXVelocity * frameTimeInSeconds;
    ballSprite.PositionY += ballYVelocity * frameTimeInSeconds;
    // New Code:
    // Check if the two CCSprites overlap...
    bool doesBallOverlapPaddle = ballSprite.BoundingBoxTransformedToParent.IntersectsRect(
        paddleSprite.BoundingBoxTransformedToParent);
    // ... and if the ball is moving downward.
    bool isMovingDownward = ballYVelocity < 0;
    if (doesBallOverlapPaddle && isMovingDownward)
    {
        // First let's invert the velocity:
        ballYVelocity *= -1;
        // Then let's assign a random value to the ball's x velocity:
        const float minXVelocity = -300;
        const float maxXVelocity = 300;
        ballXVelocity = CCRandom.GetRandomFloat (minXVelocity, maxXVelocity);
    }
    // First let’s get the ball position:   
    float ballRight = ballSprite.BoundingBoxTransformedToParent.MaxX;
    float ballLeft = ballSprite.BoundingBoxTransformedToParent.MinX;
    // Then let’s get the screen edges
    float screenRight = VisibleBoundsWorldspace.MaxX;
    float screenLeft = VisibleBoundsWorldspace.MinX;
    // Check if the ball is either too far to the right or left:    
    bool shouldReflectXVelocity = 
        (ballRight > screenRight && ballXVelocity > 0) ||
        (ballLeft < screenLeft && ballXVelocity < 0);
    if (shouldReflectXVelocity)
    {
        ballXVelocity *= -1;
    }
}
```

## <a name="adding-scoring"></a>Přidání vyhodnocování

Teď, když je možné přehrát hra, posledním krokem je přidání vyhodnocování logiky. Nejprve přidáme členem skóre do GameLayer třída s názvem `score`:

```csharp
int score;
```

Použijeme tuto proměnnou ke sledování skóre hráčů a zobrazit pomocí `scoreLabel`. Stačí přidat následující kód do příkazu if v `RunGameLogic` při míč a desku překrývají:

```csharp
// ...
if (doesBallOverlapPaddle && isMovingDownward)
{
    // First let's invert the velocity:
    ballYVelocity *= -1;
    // Then let's assign a random to the ball's x velocity:
    const float minXVelocity = -300;
    const float maxXVelocity = 300;
    ballXVelocity = CCRandom.GetRandomFloat (minXVelocity, maxXVelocity);
    // New code:
    score++;
    scoreLabel.Text = "Score: " + score;
}
// ...
```

Nyní můžeme spustit hru a najdete v článku, hry zobrazuje skóre, které zvýší jako kuličky nedoručitelných zpráv z desku:

![Dokončené hry s narůstajícími skóre](bouncing-game-images/image1.png "dokončené hry s narůstajícími skóre")

## <a name="summary"></a>Souhrn

Tento názorný postup zobrazí grafické, fyziky a vstup pomocí CocosSharp vytvoření hry s napříč platformami. Je prvním krokem při Začínáme s vývojem herní CocosSharp. Jsme se podívali na některé z nejběžnějších třídy v CocosSharp, jak vytvořit vizuálním stromu, takže objekty vykresluje správně a jak implementovat logiku herní každých rámce.

Tento názorný postup zahrnutých pouze malou část toho co nabízí herní modul CocosSharp. Informace a návody na další témata CocosSharp najdete v tématu [zbytek příručky CocosSharp](~/graphics-games/cocossharp/index.md).

## <a name="related-links"></a>Související odkazy

- [Dokončené herní (ukázka)](https://developer.xamarin.com/samples/mobile/BouncingGame/)
- [Herní obsah (ukázka)](https://github.com/xamarin/mobile-samples/blob/master/BouncingGame/Resources/Content.zip?raw=true)
