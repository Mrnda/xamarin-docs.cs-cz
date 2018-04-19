---
title: Část 2 – implementace WalkingGame
description: Tento návod ukazuje, jak přidat herní logiku a obsah tak, aby prázdný projekt MonoGame vytvořit ukázku animovaný pohyblivý symbol přesunutí s dotykovým ovládáním.
ms.prod: xamarin
ms.assetid: F0622A01-DE7F-451A-A51F-129876AB6FFD
ms.technology: xamarin-cross-platform
author: charlespetzold
ms.author: chape
ms.date: 03/28/2017
ms.openlocfilehash: bc4ab2e77bfce9c9ba6043533bcfda5a359d322e
ms.sourcegitcommit: 775a7d1cbf04090eb75d0f822df57b8d8cff0c63
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/18/2018
---
# <a name="part-2--implementing-the-walkinggame"></a>Část 2 – implementace WalkingGame

_Tento návod ukazuje, jak přidat herní logiku a obsah tak, aby prázdný projekt MonoGame vytvořit ukázku animovaný pohyblivý symbol přesunutí s dotykovým ovládáním._

Předchozí části tohoto návodu vám ukázal, jak vytvořit prázdné projekty MonoGame. Vytvoříme na tyto předchozí části tím, že jednoduchý ukázkový herní. Tento článek obsahuje následující části:

- Rozbalení náš herní obsah
- Přehled MonoGame – třída
- Vykreslování naše první pohyblivý symbol
- Vytváření CharacterEntity
- Přidání CharacterEntity hře
- Vytvoření třídy animace
- Přidání první animace do CharacterEntity
- Přidání Přesun na znak
- Odpovídající přesouvání a animace


## <a name="unzipping-our-game-content"></a>Rozbalení náš herní obsah

Než začneme psaní kódu, jsme se mají být extrahovány naše herní *obsah*. Herní vývojáři často používají termín *obsah* odkazovat na jiný kód soubory, které jsou obvykle vytvořené pomocí visual umělci, herní Designer nebo zvuk Designer. Běžné typy obsahu, obsahovat soubory použít k zobrazení vizuální prvky, přehrát zvuk nebo řízení chování umělé intelligence (AI). Z herní vývojového týmu perspektivy obsah obvykle je vytvořena bez programátory.

Obsah použít se zde naleznete [na githubu](https://github.com/xamarin/mobile-samples/blob/master/WalkingGameMG/Resources/charactersheet.png?raw=true). Tyto soubory stáhnout do umístění, které jsme budou přistupovat k dále v tomto návodu budeme potřebovat.

## <a name="monogame-class-overview"></a>Přehled MonoGame – třída

Pro začátek se podíváme MonoGame třídy používané v základní vykreslování:

- `SpriteBatch` – použitý k vykreslení grafiky 2D na obrazovku. *Sprite* jsou 2D vizuální prvky, které se používají k zobrazení obrázků na obrazovce. `SpriteBatch` Objekt můžete kreslení jedné pohyblivý symbol v době mezi jeho `Begin` a `End` metody nebo více Sprite je možné seskupit, nebo *dávkové*.
- `Texture2D` – představuje objekt obrázku za běhu. `Texture2D` instance jsou často vytvořené z formátů souborů, jako je soubor ve formátu PNG nebo BMP, i když je lze také vytvořit dynamicky za běhu. `Texture2D` instance se používají při vykreslování pomocí `SpriteBatch` instance.
- `Vector2` – představuje pozici v 2D souřadnicový systém, který se často používá k umístění vizuální objekty. Také zahrnuje MonoGame `Vector3` a `Vector4` , ale pouze použijeme `Vector2` v tomto návodu.
- `Rectangle` – oblasti okolo s pozici, šířku a výšku. Budeme používat to k definování jaká část naší `Texture2D` k vykreslení, když jsme začátek práce s animace.

Můžeme také Upozorňujeme, že MonoGame není spravován společností Microsoft – bez ohledu na jeho obor názvů. `Microsoft.Xna` Obor názvů se používá v MonoGame, aby bylo snazší migrovat existující projekty XNA MonoGame.

## <a name="rendering-our-first-sprite"></a>Vykreslování naše první pohyblivý symbol

Další jsme se zakreslit jeden pohyblivý symbol obrazovky ukazují, jak provádět 2D vykreslování v MonoGame.

### <a name="creating-a-texture2d"></a>Vytváření Texture2D

Je potřeba vytvořit `Texture2D` instance mají používat při generování naše pohyblivý symbol. Je veškerý obsah herní součástí složku s názvem **obsahu,** umístěný v projektu specifické pro platformu. MonoGame sdílených projektů nemůže obsahovat obsah, jako obsah musí používat build akce, které jsou specifické pro platformu. Vývojáři CocosSharp najdete složku obsahu známé koncept – se nacházejí v jednom místě v CocosSharp a MonoGame projekty. Složka obsahu naleznete v projektu pro iOS a ve složce prostředky v projektu pro Android.

Chcete-li přidat naše herní obsah, klikněte pravým tlačítkem na **obsah** složky a vyberte **Přidat > Přidat soubory...** Přejděte do umístění, kam jste extrahovali soubor content.zip a vyberte **charactersheet.png** souboru. Pokud se zobrazí dotaz, o tom, jak přidat soubor do složky, jsme měli vybrat **kopie** možnost:

![](part2-images/image1.png "Pokud se zobrazí dotaz, o tom, jak přidat soubor do složky, vyberte možnost Kopírovat")

Složka obsahu obsahuje teď charactersheet.png soubor:

![](part2-images/image2.png "Složka obsahu teď obsahuje soubor charactersheet.png")

V dalším kroku kódu se načíst soubor charactersheet.png a vytvoření přidáme `Texture2D`. Chcete tuto, otevřete `Game1.cs` souboru a přidejte následující pole do třídy Game1.cs:

```csharp
Texture2D characterSheetTexture;
```

V dalším kroku vytvoříme `characterSheetTexture` v `LoadContent` metoda. Před všechny změny `LoadContent` metoda vypadá takto:

```csharp
protected override void LoadContent()
{
    // Create a new SpriteBatch, which can be used to draw textures.
    spriteBatch = new SpriteBatch(GraphicsDevice);
    // TODO: use this.Content to load your game content here
}
```

Jsme upozorňujeme ale, že výchozí projekt již vytvoří `spriteBatch` instance pro nás. Budeme používat to později, ale nemůžeme nebude přidávat žádné další kód pro přípravu `spriteBatch` pro použití. Na druhé straně naše `spriteSheetTexture` nevyžaduje inicializace, takže jsme inicializuje ho po `spriteBatch` je vytvořena:

```csharp
protected override void LoadContent()
{
    // Create a new SpriteBatch, which can be used to draw textures.
    spriteBatch = new SpriteBatch(GraphicsDevice);
    using (var stream = TitleContainer.OpenStream ("Content/charactersheet.png"))
    {
        characterSheetTexture = Texture2D.FromStream (this.GraphicsDevice, stream);

    }
}
```

Teď, když máme `SpriteBatch` instance a `Texture2D` přidáme naše vykreslování kód pro instance `Game1.Draw` metoda:

```csharp
protected override void Draw(GameTime gameTime)
{
    GraphicsDevice.Clear(Color.CornflowerBlue);

    spriteBatch.Begin ();

    Vector2 topLeftOfSprite = new Vector2 (50, 50);
    Color tintColor = Color.White;

    spriteBatch.Draw(characterSheetTexture, topLeftOfSprite, tintColor);

    spriteBatch.End ();

    base.Draw(gameTime);
}
```

Spuštění hra teď zobrazuje jeden pohyblivý symbol zobrazení texture vytvořené z charactersheet.png:

![](part2-images/image3.png "Spuštění hra nyní zobrazuje jeden pohyblivý symbol zobrazení texture vytvořené z charactersheet.png")

## <a name="creating-the-characterentity"></a>Vytváření CharacterEntity

Pokud jsme přidali kód pro vykreslení jednoho pohyblivý symbol na obrazovku; ale když se nám vyvíjet úplné hru bez vytvoření jiné třídy, jsme spuštěná do kódu organizace problémy.

### <a name="what-is-an-entity"></a>Co je entita?

Běžný vzor uspořádání herní kód je pro vytvoření nové třídy pro každý herní *entity* typu. Entity v vývoj her je objekt, který může obsahovat některé z těchto vlastností (ne všechny jsou povinné):

- Vizuální prvek například pohyblivý symbol, text nebo 3D modelu
- Fyziky nebo každých rámce chování například jednotku patrolling cesta sady nebo neodpovídá na požadavky vstupní znak player
- Můžete vytvořit a zničen dynamicky, například zapnutí zobrazování a shromažďovaných přehrávače

Systémy entity organizace může být složité a mnoho herní moduly nabízet třídy ke správě entity. Jsme budete být implementace velmi jednoduché entity systému, takže je vhodné poznamenat úplné hry obvykle vyžadují další organizace v rámci pro vývojáře.


### <a name="defining-the-characterentity"></a>Definování CharacterEntity

Naše entity, který budete nazýváme `CharacterEntity`, bude mít následující vlastnosti:

- Umožňuje načíst vlastní `Texture2D`
- Možnost k vykreslení samostatně, včetně obsahující volání metod k aktualizaci vysvětlovat dalším animace
- `X` Y vlastnosti, které řídí znak na pozici.
- Možnost aktualizovat – konkrétně, pro čtení hodnoty z stiskem obrazovky a odpovídajícím způsobem nastavit pozici.

Chcete-li přidat `CharacterEntity` naše hru, klikněte pravým tlačítkem nebo ovládací prvek, klikněte na **WalkingGame** projektu a vyberte **Přidat > Nový soubor...** . Vyberte **prázdné třídy** možnost a zadejte název nového souboru **CharacterEntity**, pak klikněte na tlačítko **nový**.

Nejprve přidáme schopnost `CharacterEntity` načíst `Texture2D` i, kreslení sám sebe. Jsme upraví nově přidané `CharacterEntity.cs` následujícím způsobem:

```csharp
using System;
using Microsoft.Xna.Framework.Graphics;
using Microsoft.Xna.Framework;
using Microsoft.Xna.Framework.Input.Touch;


namespace WalkingGame
{
    public class CharacterEntity
    {
        static Texture2D characterSheetTexture;

        public float X
        {
            get;
            set;
        }

        public float Y
        {
            get;
            set;
        }

        public CharacterEntity (GraphicsDevice graphicsDevice)
        {
            if (characterSheetTexture == null)
            {
                using (var stream = TitleContainer.OpenStream ("Content/charactersheet.png"))
                {
                    characterSheetTexture = Texture2D.FromStream (graphicsDevice, stream);
                }
            }
        }

        public void Draw(SpriteBatch spriteBatch)
        {
            Vector2 topLeftOfSprite = new Vector2 (this.X, this.Y);
            Color tintColor = Color.White;
            spriteBatch.Draw(characterSheetTexture, topLeftOfSprite, tintColor);
        }
    }
}
```

Výše uvedený kód přidá odpovědnost umístění, vykreslování a načítání obsahu `CharacterEntity`. Podívejme se na chvíli tak, aby odkazoval na některé aspekty prováděné ve výše uvedeném kódu.

### <a name="why-are-x-and-y-floats"></a>Proč se X a Y obtékaných objektů?

Vývojáři, kteří herní programování může zajímat, proč `float` typ je používán naproti tomu `int` nebo `double`. Z důvodu je nejběžnější pro umístění v kódu nízké úrovně vykreslování 32bitovou hodnotu. Typ dvojitý zabírá 64 bitů pro přesnost, což je málokdy potřeba pro umístění objektů. I když rozdíl 32bitové se může zdát zanedbatelný, obsahovat mnoho moderní hry grafiky, které vyžadují zpracování desetitisíce hodnoty pozice každého rámce (30 nebo 60 x za sekundu). Vyjímání množství paměti, která musíte přesunout prostřednictvím grafiky kanálu o polovinu může mít významný dopad na výkon hra.

`int` Datový typ lze příslušné jednotky měření pro umístění, ale omezení ho můžete umístit na způsob, jakým jsou umístěny entity. Například pomocí celočíselné hodnoty je obtížné implementovat smooth přesun pro hry, které podporují přiblížení nebo 3D kamery (což by jsou povolené `SpriteBatch`).

Nakonec bude vidíte, že logiku, která přesune naše znak po obrazovce bude jím pomocí hodnot časování příslušnou hru. Výsledek násobení rychlosti jak dlouho má předán v zadaném období zřídka v důsledku toho celé číslo, takže je potřeba použít `float` pro `X` a `Y`.

### <a name="why-is-charactersheettexture-static"></a>Proč je characterSheetTexture statické?

`characterSheetTexture` `Texture2D` Instance je reprezentace runtime charactersheet.png souboru. Jinými slovy, obsahuje hodnoty barvu pro každý pixelů jako extrahována ze zdroje **charactersheet.png** souboru. Kdyby naše herní vytvořte dvě `CharacterEntity` instance, pak každé z nich by potřebují přístup k informacím z charactersheet.png. V takovém případě by chceme způsobit snížení výkonu načítání charactersheet.png dvakrát, ani by by měla obsahovat duplicitní texture paměti uložené v paměti ram. Použití `static` pole můžeme sdílet stejný `Texture2D` napříč všemi `CharacterEntity` instance.

Pomocí `static` členy pro modul runtime reprezentace obsahu je zneužívající vlastností prohlížeče řešení a neřeší problémy vzniklé v větší hry, jako je třeba uvolnění obsah při přesouvání z jedné úrovně do jiné. Složitější řešení, které jsou nad rámec tohoto návodu, zahrnují pomocí obsahu kanálu pro MonoGame nebo vytváření vlastních správy obsahu systému.

### <a name="why-is-spritebatch-passed-as-an-argument-instead-of-instantiated-by-the-entity"></a>Proč je předán SpriteBatch jako argumentu Instead of mohl vytvořit jeho instanci Entity?

`Draw` Metoda, jak jsou implementované výše trvá `SpriteBatch` argument, ale naopak `characterSheetTexture` je mohl vytvořit jeho instanci `CharacterEntity`.

Důvodem je, protože nejúčinnější vykreslování je možné, kdy stejné `SpriteBatch` instance se používá pro všechny `Draw` volání a pokud všechny `Draw` se volání mezi jednu sadu `Begin` a `End` volání. Samozřejmě naše herní bude obsahovat pouze jednu entitu instanci, ale složitější hry budou využívat výhody vzor, který umožňuje více entit používat stejné `SpriteBatch` instance.


## <a name="adding-characterentity-to-the-game"></a>Přidání CharacterEntity hře

Teď, když jsme přidali naše `CharacterEntity` s kódem pro vykreslování sám sebe, jsme můžete nahraďte kód v `Game1.cs` použití instance této nové entity. K tomu budete nahradit jsme `Texture2D` pole s `CharacterEntity` pole `Game1`:

```csharp
public class Game1 : Game
{
    GraphicsDeviceManager graphics;
    SpriteBatch spriteBatch;
    // New code:
    CharacterEntity character;

    public Game1()
    {
    ...
```

Potom přidáme instance této entity v `Game1.Initialize`:

```csharp
protected override void Initialize()
{
    character = new CharacterEntity (this.GraphicsDevice);

    base.Initialize();
}
```

Také je potřeba odebrat `Texture2D` vytvoření z `LoadContent` vzhledem k tomu, který se teď zpracovává uvnitř naše `CharacterEntity`. `Game1.LoadContent` by měl vypadat přibližně takto:

```csharp
protected override void LoadContent()
{
    // Create a new SpriteBatch, which can be used to draw textures.
    spriteBatch = new SpriteBatch(GraphicsDevice);
}
```

Je to, že bez ohledu na jeho název `LoadContent` metoda není bude jediným místem, kde je možné načíst obsah – mnoho hry implementace obsahu načítání v jejich metoda Initialize nebo dokonce v jejich aktualizace kód jako obsah, nemusí být potřeba dokud nedosáhne přehrávač Některé bod příslušnou hru.

Nakonec jsme můžete metodu kreslení upravit takto:


```csharp
protected override void Draw(GameTime gameTime)
{
    GraphicsDevice.Clear(Color.CornflowerBlue);

    // We'll start all of our drawing here:
    spriteBatch.Begin ();

    // Now we can do any entity rendering:
    character.Draw(spriteBatch);
    // End renders all sprites to the screen:
    spriteBatch.End ();

    base.Draw(gameTime);
}
```

Pokud jsme spustit hru, zobrazí znak. Vzhledem k tomu, že X a Y výchozí hodnotu 0, znak, který je umístěn na levém horním rohu obrazovky:

![](part2-images/image4.png "Vzhledem k tomu, že X a Y výchozí hodnotu 0, pak znak, který je umístěn na levém horním rohu obrazovky")

## <a name="creating-the-animation-class"></a>Vytvoření třídy animace

Aktuálně naše `CharacterEntity` zobrazí celý **charactersheet.png** souboru. Toto uspořádání více bitových kopií v jednom souboru se označuje jako *pohyblivý symbol list*. Obvykle pohyblivý symbol vykreslí pouze část listu pohyblivý symbol. Jsme upraví `CharacterEntity` k vykreslení část tohoto **charactersheet.png**, a tato část se změní v čase zobrazíte vysvětlovat dalším animace.

Vytvoříme `Animation` třída k řízení logiku a stavu CharacterEntity animace. Třída animace bude obecné třídy, které by mohly být použity žádné entity, ne jenom `CharacterEntity` animace. Ultimate `Animation` třída zajistí `Rectangle` který `CharacterEntity` použije při kreslení sám sebe. Také vytvoříme `AnimationFrame` třída, která se použije k definování animace.


### <a name="defining-animationframe"></a>Definování AnimationFrame

`AnimationFrame` nebudou obsahovat veškeré logiky související s animace. Budeme používat ji pouze pro ukládání dat. Chcete-li přidat `AnimationFrame` třídu, klikněte pravým tlačítkem nebo ovládací prvek, klikněte na **WalkingGame** sdílený projekt a vyberte **Přidat > Nový soubor...** Zadejte název **AnimationFrame** a klikněte na **nový** tlačítko. Upravíme `AnimationFrame.cs` souboru tak, aby obsahoval následující kód:


```csharp
using System;
using Microsoft.Xna.Framework;

namespace WalkingGame
{
    public class AnimationFrame
    {
        public Rectangle SourceRectangle { get; set; }
        public TimeSpan Duration { get; set; }
    }
}
```

`AnimationFrame` Třída obsahuje dva údaje:

- `SourceRectangle` – Definuje oblast `Texture2D` který se zobrazí ve `AnimationFrame`. Tato hodnota se měří v pixelech, s na horní levé, který (0, 0).
- `Duration` – Definuje jak dlouho `AnimationFrame` se zobrazí, když se používá v `Animation`.

### <a name="defining-animation"></a>Definování animace

`Animation` Třída bude obsahovat `List<AnimationFrame>` a také logiku k přepínači, který rámec je aktuálně zobrazený podle jak dlouho byla úspěšná.

Chcete-li přidat `Animation` třídu, klikněte pravým tlačítkem nebo ovládací prvek, klikněte na **WalkingGame** sdílený projekt a vyberte **Přidat > Nový soubor...** . Zadejte název **animace** a klikněte na **nový** tlačítko. Upravíme `Animation.cs` souboru tak, aby obsahovala následující kód:


```csharp
using System;
using System.Collections.Generic;
using System.Linq;
using Microsoft.Xna.Framework;

namespace WalkingGame
{
    public class Animation
    {
        List<AnimationFrame> frames = new List<AnimationFrame>();
        TimeSpan timeIntoAnimation;

        TimeSpan Duration
        {
            get
            {
                double totalSeconds = 0;
                foreach (var frame in frames)
                {
                    totalSeconds += frame.Duration.TotalSeconds;
                }

                return TimeSpan.FromSeconds (totalSeconds);
            }
        }

        public void AddFrame(Rectangle rectangle, TimeSpan duration)
        {
            AnimationFrame newFrame = new AnimationFrame()
            {
                SourceRectangle = rectangle,
                Duration = duration
            };

            frames.Add(newFrame);
        }

        public void Update(GameTime gameTime)
        {
            double secondsIntoAnimation = 
                timeIntoAnimation.TotalSeconds + gameTime.ElapsedGameTime.TotalSeconds;


            double remainder = secondsIntoAnimation % Duration.TotalSeconds;

            timeIntoAnimation = TimeSpan.FromSeconds (remainder);
        }
    }
}
```

Podívejme se na některé z podrobností o `Animation` třídy.

### <a name="frames-list"></a>rámce seznamu

`frames` Člen je co ukládat data pro naše animace. Kód, který vytvoří instanci animací přidá `AnimationFrame` instance k `frames` seznamu prostřednictvím `AddFrame` metoda. Obsáhlejší implementaci může nabídku `public` metody nebo vlastnosti úpravy `frames`, ale budete omezená funkce pro přidání rámce pro účely tohoto postupu.

### <a name="duration"></a>Doba trvání

Doba trvání vrátí celkovou dobu trvání `Animation,` získaný přidáním platnost všech uzavřeného `AnimationFrame` instance. Tato hodnota může uložit do mezipaměti, pokud `AnimationFrame` byly neměnné objekt, ale vzhledem k tomu, že implementovali jsme AnimationFrame jako třída, které je možné změnit po přidání do animace, potřebujeme k výpočtu tato hodnota vždy, když vlastnost přistupuje.

### <a name="update"></a>Aktualizace

`Update` Metoda by měla být volána každý snímek (tedy pokaždé, když se aktualizuje celou hru). Jejím účelem je zvýšit `timeIntoAnimation` člen, který se používá k vrácení aktuálně zobrazený rámečku. Logika `Update` brání `timeIntoAnimation` z někdy je větší než doba trvání animace celý.

Vzhledem k tomu, že budeme používat `Animation` třída zobrazíte vysvětlovat dalším animace, pak by měla obsahovat tento animace opakování, když je dosaženo konce. Jsme se dá dosáhnout pomocí kontroly, zda `timeIntoAnimation` je větší než `Duration` hodnotu. V takovém případě bude cyklus zpět na začátku a zachovat zbývající, což vede k opakování animace.

### <a name="obtaining-the-current-frames-rectangle"></a>Získávání aktuální rámečku obdélníku

Účelem `Animation` třída je nakonec k poskytování `Rectangle` který může být použit při kreslení pohyblivý symbol. Aktuálně `Animation` třída obsahuje logiku pro změnu `timeIntoAnimation` člena, který použijeme k získání který `Rectangle` má být zobrazen. Provedeme to tak, že vytvoříte `CurrentRectangle` vlastnost `Animation` třídy. Zkopírujte do této vlastnosti `Animation` třídy:

```csharp
public Rectangle CurrentRectangle
{
    get
    {
        AnimationFrame currentFrame = null;

        // See if we can find the frame
        TimeSpan accumulatedTime;
        foreach(var frame in frames)
        {
            if (accumulatedTime + frame.Duration >= timeIntoAnimation)
            {
                currentFrame = frame;
                break;
            }
            else
            {
                accumulatedTime += frame.Duration;
            }
        }

        // If no frame was found, then try the last frame, 
        // just in case timeIntoAnimation somehow exceeds Duration
        if (currentFrame == null)
        {
            currentFrame = frames.LastOrDefault ();
        }

        // If we found a frame, return its rectangle, otherwise
        // return an empty rectangle (one with no width or height)
        if (currentFrame != null)
        {
            return currentFrame.SourceRectangle;
        }
        else
        {
            return Rectangle.Empty;
        }
    }
}
```

## <a name="adding-the-first-animation-to-characterentity"></a>Přidání první animace do CharacterEntity

`CharacterEntity` Bude obsahovat animací pro procházení a stojící, jakož i odkaz na aktuální `Animation` zobrazení.

Nejprve přidáme naše první `Animation,` který použijeme k otestování funkce jako dosavadní zapsána. Umožňuje přidat následující členy k třídě CharacterEntity:

```csharp
Animation walkDown;
Animation currentAnimation;
```

V dalším kroku nastavíme `walkDown` animace. Opravte `CharacterEntity` konstruktor následujícím způsobem:

```csharp
public CharacterEntity (GraphicsDevice graphicsDevice)
{
    if (characterSheetTexture == null)
    {
        using (var stream = TitleContainer.OpenStream ("Content/charactersheet.png"))
        {
            characterSheetTexture = Texture2D.FromStream (graphicsDevice, stream);
        }
    }

    walkDown = new Animation ();
    walkDown.AddFrame (new Rectangle (0, 0, 16, 16), TimeSpan.FromSeconds (.25));
    walkDown.AddFrame (new Rectangle (16, 0, 16, 16), TimeSpan.FromSeconds (.25));
    walkDown.AddFrame (new Rectangle (0, 0, 16, 16), TimeSpan.FromSeconds (.25));
    walkDown.AddFrame (new Rectangle (32, 0, 16, 16), TimeSpan.FromSeconds (.25));
}
```

Jak už bylo zmíněno dříve, musíme volání `Animation.Update` pro založené na čase animací přehrávání. Také je potřeba přiřadit `currentAnimation`. Pro nyní jsme budete přiřadit `currentAnimation` k `walkDown`, ale nemůžeme budete nahrazujete tento kód později při implementaci logiku pohyb. Přidáme `Update` metodu `CharacterEntity` následujícím způsobem:


```csharp
public void Update(GameTime gameTime)
{
    // temporary - we'll replace this with logic based off of which way the
    // character is moving when we add movement logic
    currentAnimation = walkDown;

    currentAnimation.Update (gameTime);
}
```

Teď, když máme `currentAnimation` právě přiřazen a aktualizovat, jsme můžete použít k provedení naše kreslení. Upravíme `CharacterEntity.Draw` následujícím způsobem:

```csharp
public void Draw(SpriteBatch spriteBatch)
{
    Vector2 topLeftOfSprite = new Vector2 (this.X, this.Y);
    Color tintColor = Color.White;
    var sourceRectangle = currentAnimation.CurrentRectangle;

    spriteBatch.Draw(characterSheetTexture, topLeftOfSprite, sourceRectangle, Color.White);
}
```

Poslední krok, dokud získávání `CharacterEntity` animace je k volání nově přidaný `Update` metoda z `Game1`. Upravit `Game1.Update` následujícím způsobem:

```csharp
protected override void Update(GameTime gameTime)
{
    character.Update (gameTime);
    base.Update(gameTime);
}
```

Nyní `CharacterEntity` přehraje jeho `walkDown` animace:

![](part2-images/image5.gif "Nyní CharacterEntity přehraje jeho walkDown animace")

## <a name="adding-movement-to-the-character"></a>Přidání Přesun na znak

V dalším kroku jsme přidali přesun naše znak použití ovládacích prvků dotykového ovládání. Když uživatel dotykem na obrazovce, se přesune znak směrem místě, kde je dotýkal na obrazovce. Pokud nejsou zjištěny žádné úpravy, bude spadat znak na místě.

### <a name="defining-getdesiredvelocityfrominput"></a>Definování GetDesiredVelocityFromInput

Budeme používat na MonoGame `TouchPanel` třídy, která poskytuje informace o aktuálním stavu dotykovou obrazovku. Umožňuje přidat metodu, která zkontroluje `TouchPanel` a vrátit požadované rychlosti naše znak:


```csharp
Vector2 GetDesiredVelocityFromInput()
{
    Vector2 desiredVelocity = new Vector2 ();

    TouchCollection touchCollection = TouchPanel.GetState();

    if (touchCollection.Count > 0)
    {
        desiredVelocity.X = touchCollection [0].Position.X - this.X;
        desiredVelocity.Y = touchCollection [0].Position.Y - this.Y;

        if (desiredVelocity.X != 0 || desiredVelocity.Y != 0)
        {
            desiredVelocity.Normalize();
            const float desiredSpeed = 200;
            desiredVelocity *= desiredSpeed;
        }
    }

    return desiredVelocity;
}
```

`TouchPanel.GetState` Metoda vrátí `TouchCollection` obsahující informace o kde je uživatel klepnou na obrazovce. Pokud uživatel není klepnou na obrazovce, pak se `TouchCollection` bude prázdný, v takovém případě jsme neměli přesouvat znak.

Pokud uživatel je klepnou na obrazovce, jsme se přesune znak směrem první dotykového ovládání, jinými slovy, `TouchLocation` indexem 0. Nejdřív vytvoříme požadované rychlosti, aby se rovnala rozdíl mezi je znak umístění a první touch umístění:

```csharp
        desiredVelocity.X = touchCollection [0].Position.X - this.X;
        desiredVelocity.Y = touchCollection [0].Position.Y - this.Y;
```

Co následuje je bit matematické, což ponechá znak přesunutí stejnou rychlostí. Vysvětlení, proč je důležité, můžeme Představte si situaci, kde je uživatel klepnou na 500 pixelů obrazovky mimo kde znak, který se nachází. První řádek kde `desiredVelocity.X` je sada by přiřadit hodnotu 500. Ale v případě, že uživatel byly klepnou na obrazovce ve vzdálenosti pouze 100 jednotky z znak, pak se `desiredVelocity.X `by byl nastaven na 100. Výsledkem bude, že je znak přesun rychlost by reagovat na jak daleko bodem touch je z znak. Vzhledem k tomu, že chceme znak, který má vždy stejnou rychlostí přesunout, musíme upravit desiredVelocity.

`if (desiredVelocity.X != 0 || desiredVelocity.Y != 0)` Příkaz kontroluje zjišťování pokud rychlost je jiný-nula – jinými slovy, abyste měli jistotu, že uživatel není klepnou na stejném místě jako je znak aktuální pozici. Pokud ne, pak potřebujeme nastavit rychlost je znak být konstantní bez ohledu na to, jak daleko touch je. Jsme stavu dosáhnout zakázáním normalizace vektoru rychlosti, což vede k jeho se o délce 1. Vektor rychlosti 1 znamená, že znak přesune na 1 pixel za sekundu. To jsme budete zrychlení vynásobením hodnoty požadované rychlostí 200.


### <a name="applying-velocity-to-position"></a>Použití rychlosti na pozici

Rychlost, kterou vrátil `GetDesiredVelocityFromInput` je nutné použít na znak `X` a `Y` hodnoty nemá žádný vliv za běhu. Upravíme `Update` metoda následujícím způsobem:

```csharp
public void Update(GameTime gameTime)
{

    var velocity = GetDesiredVelocityFromInput ();

    this.X += velocity.X * (float)gameTime.ElapsedGameTime.TotalSeconds;
    this.Y += velocity.Y * (float)gameTime.ElapsedGameTime.TotalSeconds;

    // temporary - we'll replace this with logic based off of which way the
    // character is moving when we add movement logic
    currentAnimation = walkDown;

    currentAnimation.Update (gameTime);
}
```

Co implementovali jsme tady se nazývá *založené na čase* přesun (Naproti tomu *na základě rámce* pohyb). Přesun založené na čase vynásobí hodnotu rychlosti (v našem případě hodnoty uloženy v `velocity` proměnné) podle uplynutí jak dlouhá doba od poslední aktualizace, která je uložena v `gameTime.ElapsedGameTime.TotalSeconds`. Pokud hra probíhá na menší počet snímků za sekundu, uplynulý čas mezi rámce se zvyšuje – konečným výsledkem je, že objektů s použitím založené na čase přesun přesune vždy stejnou rychlostí bez ohledu na to obnovovací frekvence.

Pokud jsme naše herní spustit nyní, uvidíme znak, který je přesunutí směrem touch umístění:

![](part2-images/image6.gif "Znak, který je cestu k umístění dotykového ovládání")

## <a name="matching-movement-and-animation"></a>Odpovídající přesouvání a animace

Jakmile jsme naše znak přesouvání a přehrávání jednoho animace, jsme můžete definovat zbytek naše animace a pak je používat tak, aby odrážela přesun znaku. Po dokončení bude máme osm animací celkem:

- Animace proti nahoru, dolů, doleva a doprava
- Animace pro stojící stále a směřující nahoru, dolů, doleva a doprava

### <a name="defining-the-rest-of-the-animations"></a>Definování zbytek animací

Nejprve přidáme `Animation` instance k `CharacterEntity` třída pro všechny naše animací na stejném místě, které jsme přidali `walkDown`. Jakmile jsme to provést, `CharacterEntity` bude mít následující `Animation` členy:

```csharp
Animation walkDown;
Animation walkUp;
Animation walkLeft;
Animation walkRight;

Animation standDown;
Animation standUp;
Animation standLeft;
Animation standRight;

Animation currentAnimation;
```

Nyní budeme definovat animace v `CharacterEntity` konstruktor následujícím způsobem:

```csharp
public CharacterEntity (GraphicsDevice graphicsDevice)
{
    if (characterSheetTexture == null)
    {
        using (var stream = TitleContainer.OpenStream ("Content/charactersheet.png"))
        {
            characterSheetTexture = Texture2D.FromStream (graphicsDevice, stream);
        }
    }

    walkDown = new Animation ();
    walkDown.AddFrame (new Rectangle (0, 0, 16, 16), TimeSpan.FromSeconds (.25));
    walkDown.AddFrame (new Rectangle (16, 0, 16, 16), TimeSpan.FromSeconds (.25));
    walkDown.AddFrame (new Rectangle (0, 0, 16, 16), TimeSpan.FromSeconds (.25));
    walkDown.AddFrame (new Rectangle (32, 0, 16, 16), TimeSpan.FromSeconds (.25));

    walkUp = new Animation ();
    walkUp.AddFrame (new Rectangle (144, 0, 16, 16), TimeSpan.FromSeconds (.25));
    walkUp.AddFrame (new Rectangle (160, 0, 16, 16), TimeSpan.FromSeconds (.25));
    walkUp.AddFrame (new Rectangle (144, 0, 16, 16), TimeSpan.FromSeconds (.25));
    walkUp.AddFrame (new Rectangle (176, 0, 16, 16), TimeSpan.FromSeconds (.25));

    walkLeft = new Animation ();
    walkLeft.AddFrame (new Rectangle (48, 0, 16, 16), TimeSpan.FromSeconds (.25));
    walkLeft.AddFrame (new Rectangle (64, 0, 16, 16), TimeSpan.FromSeconds (.25));
    walkLeft.AddFrame (new Rectangle (48, 0, 16, 16), TimeSpan.FromSeconds (.25));
    walkLeft.AddFrame (new Rectangle (80, 0, 16, 16), TimeSpan.FromSeconds (.25));

    walkRight = new Animation ();
    walkRight.AddFrame (new Rectangle (96, 0, 16, 16), TimeSpan.FromSeconds (.25));
    walkRight.AddFrame (new Rectangle (112, 0, 16, 16), TimeSpan.FromSeconds (.25));
    walkRight.AddFrame (new Rectangle (96, 0, 16, 16), TimeSpan.FromSeconds (.25));
    walkRight.AddFrame (new Rectangle (128, 0, 16, 16), TimeSpan.FromSeconds (.25));

    // Standing animations only have a single frame of animation:
    standDown = new Animation ();
    standDown.AddFrame (new Rectangle (0, 0, 16, 16), TimeSpan.FromSeconds (.25));

    standUp = new Animation ();
    standUp.AddFrame (new Rectangle (144, 0, 16, 16), TimeSpan.FromSeconds (.25));

    standLeft = new Animation ();
    standLeft.AddFrame (new Rectangle (48, 0, 16, 16), TimeSpan.FromSeconds (.25));

    standRight = new Animation ();
    standRight.AddFrame (new Rectangle (96, 0, 16, 16), TimeSpan.FromSeconds (.25));
}
```

Je třeba poznamenat, že výše uvedený kód byl přidán do `CharacterEntity` konstruktor zachovat Tento názorný postup, který je kratší. Hry bude obvykle oddělení definici znak animace do své vlastní třídy nebo zatížení tyto informace z formátu dat jako XML nebo JSON.

V dalším kroku jsme budete upravit logika pro použijte animací podle směru, která přechází znak nebo podle poslední animace, pokud znak právě byla zastavena. K tomuto účelu upravíme `Update` metoda:


```csharp
public void Update(GameTime gameTime)
{
    var velocity = GetDesiredVelocityFromInput ();

    this.X += velocity.X * (float)gameTime.ElapsedGameTime.TotalSeconds;
    this.Y += velocity.Y * (float)gameTime.ElapsedGameTime.TotalSeconds;


    if (velocity != Vector2.Zero)
    {
        bool movingHorizontally = Math.Abs (velocity.X) > Math.Abs (velocity.Y);
        if (movingHorizontally)
        {
            if (velocity.X > 0)
            {
                currentAnimation = walkRight;
            }
            else
            {
                currentAnimation = walkLeft;
            }
        }
        else
        {
            if (velocity.Y > 0)
            {
                currentAnimation = walkDown;
            }
            else
            {
                currentAnimation = walkUp;
            }
        }
    }
    else
    {
        // If the character was walking, we can set the standing animation
        // according to the walking animation that is playing:
        if (currentAnimation == walkRight)
        {
            currentAnimation = standRight;
        }
        else if (currentAnimation == walkLeft)
        {
            currentAnimation = standLeft;
        }
        else if (currentAnimation == walkUp)
        {
            currentAnimation = standUp;
        }
        else if (currentAnimation == walkDown)
        {
            currentAnimation = standDown;
        }
        else if (currentAnimation == null)
        {
            currentAnimation = standDown;
        }

        // if none of the above code hit then the character
        // is already standing, so no need to change the animation.
    }

    currentAnimation.Update (gameTime);
}
```

Kód k přepínači animací je rozdělena na dva bloky. První zkontroluje, jestli rychlosti není rovno `Vector2.Zero` – sdělí nám zda znak je přesunutí. Pokud znak je přesunutí, pak se podíváme na to `velocity.X` a `velocity.Y` hodnoty a určit, které vysvětlovat dalším animace přehrávání.

Pokud není přesunutí znak, pak chceme nastavit je znak `currentAnimation` k stojící animace – jsme jenom to však udělat, pokud `currentAnimation` je proti animace nebo pokud nebyla nastavena animace. Pokud `currentAnimation` není jedním z čtyři vysvětlovat dalším animací, pak znak, který je již stálý, není třeba změnit `currentAnimation`.

Výsledek tohoto kódu je, že bude znak správně animace při procházení a pak čelí poslední směr, který byl proti, když se zastaví:

![](part2-images/image7.gif "Výsledek tohoto kódu je, že bude znak správně animace při procházení a pak čelí poslední směr, který byl proti, když se zastaví")

## <a name="summary"></a>Souhrn

Tento návod vám ukázal, jak pracovat s MonoGame vytvořit napříč platformami herní s Sprite, přesunutí objektů, vstupní detekce a animace. Vysvětluje vytvoření třídy pro obecné účely animace. Také ukázal, jak vytvořit entitu znak pro uspořádání logiku kódu.

## <a name="related-links"></a>Související odkazy

- [Zdroj obrázku CharacterSheet (ukázka)](https://github.com/xamarin/mobile-samples/blob/master/WalkingGameMG/Resources/charactersheet.png?raw=true)
- [Proti herní dokončení (ukázka)](https://developer.xamarin.com/samples/mobile/WalkingGameMG/)
