---
title: Entity v CocosSharp
description: Vzor entity je efektivní způsob, jak uspořádat herní kódu. Ho zlepšuje čitelnost, usnadňuje kódu, které chcete zachovat a využívá funkce integrované nadřazený podřízený.
ms.prod: xamarin
ms.assetid: 1D3261CE-AC96-4296-8A53-A76A42B927A8
author: charlespetzold
ms.author: chape
ms.date: 03/27/2017
ms.openlocfilehash: 58a8d4e6fcb8a2165fafad74a5c59481d1550351
ms.sourcegitcommit: 0a72c7dea020b965378b6314f558bf5360dbd066
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 05/09/2018
ms.locfileid: "33921908"
---
# <a name="entities-in-cocossharp"></a>Entity v CocosSharp

_Vzor entity je efektivní způsob, jak uspořádat herní kódu. Ho zlepšuje čitelnost, usnadňuje kódu, které chcete zachovat a využívá funkce integrované nadřazený podřízený._

Vzor entita může zvýšit pro vývojáře v rámci CocosSharp prostřednictvím vylepšené kód organizace. Tento názorný postup bude praktických příklad znázorňující postup vytvoření dvě entity – lodě entitu a entitu odrážka. Tyto entity bude samostatný objekty, to znamená, že po vytvoření instance se bude automaticky vykreslen a provede přesun logiku v závislosti na jejich typu. 

Tato příručka obsahuje následující témata:

 - Úvod do herní entity
 - Obecné oproti typy konkrétní entit
 - Nastavení projektu
 - Vytváření tříd entit
 - Přidávání instancí entit na `GameLayer`
 - Reagovat na entity logiku `GameLayer`

Dokončení herní bude vypadat takto:

![](entities-images/image1.png "Dokončení herní bude vypadat například takto")


## <a name="introduction-to-game-entities"></a>Úvod do herní entity

Herní entity jsou třídy, které definují objekty vykreslování, kolizí, fyziky nebo umělé intelligence logiku, která potřebuje. Naštěstí entity, které jsou součástí základu kódu herní často odpovídat koncepční objekty ve hře. Pokud je hodnota true, identifikace entity, které je potřeba v hry lze snadněji provést. 

Například místo motivu [em čistou zálohu herní](http://en.wikipedia.org/wiki/Shoot_%27em_up) může zahrnovat následující entity:

 - `PlayerShip`
 - `EnemyShip`
 - `PlayerBullet`
 - `EnemyBullet`
 - `CollectableItem`
 - `HUD`
 - `Environment`

Tyto entity by vlastní třídy ve hře a každá instance by vyžadovaly žádné nebo téměř žádné nastavení nad rámec vytváření instancí.


## <a name="general-vs-specific-entity-types"></a>Obecné oproti typy konkrétní entit

Jedním ze základních otázek herní vývojáře, kteří používají systému entity, jimž je kolik ke generalizaci jejich entity. Většina konkrétní implementace nadefinovat třídy pro každý typ entity, i když se liší podle několik vlastností. Další obecné systémy budou kombinovat skupiny entit do jedné třídy a povolit instance, které chcete přizpůsobit.

Jsme můžete Představte si například hru místa, která definuje následující třídy:

 - `PlayerDogfighter`
 - `PlayerBomber`
 - `EnemyMissileShip`
 - `EnemyLaserShip`

Přístup se další zobecněn by k vytvoření jedné třídy pro player lodě a jedné třídy pro nepřátelský lodě, které může být nakonfigurované pro podporu různých lodě typy. Přizpůsobení může zahrnovat načíst, jaký typ odrážka entity k vytvoření při čistá, pohyb koeficienty, která image a dodává AI logiku pro tento. V takovém případě lze snížit seznamu entit na:

 - `PlayerShip`
 - `EnemyShip`

Tyto typy entit samozřejmě mohou být další zobecněn, tím, že přizpůsobení jednotlivých instancí řízení pohyb. Instance lodě Player by číst ze vstupu nepřátelský lodě instance může provádět AI logiku. To znamená, že může být zobecněn entity i další do jediné třídy:

 - `Ship`

Generalizace můžete pokračovat i další – hry mohou používat jediná základní třída pro všechny entity. Tento jedné třídy, které může být volána `GameEntity`, by být třída používaná pro každou instanci entity celý hry, včetně entit, které nejsou dodává jako jsou odrážek a collectible položky (power-ups).

Tato obecná většina systémů se často označuje jako *součásti systému*. V systému herní entit může mít jednotlivé komponenty, jako je například fyziky, AI, nebo vykreslování součásti přidat k přizpůsobení chování a vzhledu. Čistý součást systémům povolte ultimate flexibilitu a problémů způsobených použití dědičnosti například řetězy komplexní dědičnosti se můžete vyhnout. Stejně jako u jiných generalizace efektivní součástí systému může být složité nastavení a můžete snížit expressiveness kódu.

Úroveň generalizace používá závisí na několik rozhodnutí, včetně:

 - Herní velikost – menší hry si může dovolit vytvořit určité třídy, zatímco větší hry může být obtížné spravovat s velkým počtem třídy.
 - Vývoj – řízených daty hry, které jsou závislé na data (bitové kopie, 3D modelů a datových souborů, jako je například XML nebo JSON) může těžit z s zobecněn typy entit a jaké jsou specifikace na základě dat konfigurace. To je obzvláště importu pro hry, které chcete přidat nový obsah během vývoje nebo po vydala hra.
 - Herní modul vzory – některé herní stroje podporovat použití součástí systému důrazně zatímco jiné vývojáři k rozhodování o organizování entity. CocosSharp nevyžaduje využití součásti systému, takže se vývojáři volné implementovat libovolného typu entity. 

Z důvodu zjednodušení budeme používat konkrétní založené na třídě přístup s jednou entitou lodě a odrážek pro účely tohoto kurzu.


## <a name="project-setup"></a>Nastavení projektu

Než začneme implementace naše entity, je potřeba vytvořit projekt. Budeme ke zjednodušení vytváření projektu používat šablony projektů CocosSharp. [Zkontrolujte tento příspěvek](http://forums.xamarin.com/discussion/26822/cocossharp-project-templates-for-xamarin-studio) informace o vytvoření projektu CocosSharp ze sady Visual Studio pro Mac šablony. Zbytek tohoto průvodce použije název projektu **EntityProject**.

Po vytvoření naše projektu vytvoříme řešení naše hra na spuštění při 480 x 320. Chcete-li to provést, volejte `CCScene.SetDefaultDesignResolution` v `GameAppDelegate.ApplicationDidFinishLaunching` metoda následujícím způsobem:


```csharp
public override void ApplicationDidFinishLaunching (CCApplication application, CCWindow mainWindow)
{
    ...

    // New code for resolution setting:
    CCScene.SetDefaultDesignResolution(480, 320, CCSceneResolutionPolicy.ShowAll);
    
    CCScene scene = new CCScene (mainWindow);
    GameLayer gameLayer = new GameLayer ();

    scene.AddChild (gameLayer);
    mainWindow.RunWithScene (scene);
} 
```

Další informace o práci s CocosSharp řešení najdete v tématu naše [příručce na zpracování více řešení v CocosSharp](~/graphics-games/cocossharp/resolutions.md).


## <a name="adding-content-to-the-project"></a>Přidávání obsahu do projektu

Po vytvoření naše projektu přidáme soubory obsažené v [tento soubor zip obsahu](https://github.com/xamarin/mobile-samples/blob/master/BouncingGame/Resources/Entities.zip?raw=true). K tomu, stáhněte si soubor zip a rozbalte ho. Přidejte **ship.png** a **bullet.png** k **obsahu** složky. **Obsahu** uvnitř, bude složka **prostředky** složku v systému Android a bude v kořenovém adresáři projektu v systému iOS. Jakmile přidali, bychom měli vidět oba soubory v **obsahu** složky:

![](entities-images/image2.png "Po přidání, musí být oba soubory ve složce obsahu")


## <a name="creating-the-ship-entity"></a>Vytvoření příjemce entity

`Ship` Třída bude naše herní první entity. Chcete-li přidat `Ship` třídy, nejprve vytvořte složku s názvem **entity** na kořenové úrovni projektu. Přidejte novou třídu v **entity** složku s názvem `Ship`:

![](entities-images/image3.png "Přidejte novou třídu v entity složku s názvem příjemce")

První změna budete provedeme naše `Ship` třída je umožnit jeho dědí `CCNode` třídy. `CCNode` slouží jako základní třída pro třídy společných CocosSharp jako `CCSprite` a `CCLayer`a nám poskytuje následující funkce:

 - `Position` Vlastnost pro přesun lodě po obrazovce.
 - `Children` Vlastnost pro přidání `CCSprite.`
 - `Parent` vlastnosti, která slouží k připojení `Ship` instance druhý `CCNodes`. Jsme nebude pomocí této funkce v tomto kurzu; větší hry často využít výhod entity se připojuje k jiné `CCNodes`. 
 - `AddEventListener` Metoda pro zpracování vstupu pro přesun lodě.
 - `Schedule` Metoda čistá odrážek.

Také přidáme `CCSprite` instance tak, aby naše lodě můžete zobrazit na obrazovce:


```csharp
using System;
using CocosSharp;

namespace EntityProject
{
    public class Ship : CCNode
    {
        CCSprite sprite;

        public Ship () : base()
        {
            sprite = new CCSprite ("ship.png");
            // Center the Sprite in this entity to simplify
            // centering the Ship when it is instantiated
            sprite.AnchorPoint = CCPoint.AnchorMiddle;
            this.AddChild(sprite);
        }
    }
}
```

Potom přidáme lodě k naší `GameLayer` zobrazíte jej zobrazit v našem herní:


```csharp
public class GameLayer : CCLayer
{
    Ship ship;

    public GameLayer ()
    {
        ship = new Ship ();
        ship.PositionX = 240;
        ship.PositionY = 50;
        this.AddChild (ship);
    } 
    ...
```

Pokud jsme naše herní zobrazí naše lodě entity:

![](entities-images/image4.png "Při spuštění hry, zobrazí se lodě entity")


### <a name="why-inherit-from-ccnode-instead-of-ccsprite"></a>Proč dědí CCNode místo CCSprite?

V tomto okamžiku naše `Ship` třída je jednoduchý obálku pro `CCSprite` instance. Vzhledem k tomu `CCSprite` také dědí z `CCNode`, jsme může mít zděděno přímo z `CCSprite`, který by omezuje, kód v `Ship.cs`. Kromě toho, která dědí přímo z `CCSprite` snižuje počet objektů v paměti a můžete vylepšit výkon tím, že menší strom závislosti.

I přes tyto výhody jsme zděděno z `CCNode` ke skrytí některé `CCSprite` vlastnosti z každé instance. Například `Texture` vlastnosti by neměly být změněna mimo `Ship` třídy a, která dědí z `CCNode` umožňuje skrýt tuto vlastnost. Veřejné členy naše entit stát zvlášť důležité větší s růstem hry a další vývojáři jsou přidány do týmu.


## <a name="adding-input-to-the-ship"></a>Přidává se vstup do lodě

Teď, když je viditelný na obrazovce naše lodě přidáme vstup. Náš přístup bude vypadat podobně jako přístup použitý [BouncingGame průvodce](~/graphics-games/cocossharp/bouncing-game.md)kromě toho, že jsme umístění kód pro přesun `Ship` třída spíše než v obsahující `CCLayer` nebo `CCScene`.

Přidejte kód, který `Ship` pro podporu ani ji přesunout do všude, kde je uživatel klepnou na obrazovce:


```csharp
public class Ship : CCNode
{
    CCSprite sprite;

    CCEventListenerTouchAllAtOnce touchListener;

    public Ship () : base()
    {
        sprite = new CCSprite ("ship.png");
        // Center the Sprite in this entity to simplify
        // centering the Ship on screen
        sprite.AnchorPoint = CCPoint.AnchorMiddle;
        this.AddChild(sprite);

        touchListener = new CCEventListenerTouchAllAtOnce();
        touchListener.OnTouchesMoved = HandleInput;
        AddEventListener(touchListener, this);

    }

    private void HandleInput(System.Collections.Generic.List<CCTouch> touches, CCEvent touchEvent)
    {
        if(touches.Count > 0)
        {
            CCTouch firstTouch = touches[0];

            this.PositionX = firstTouch.Location.X;
            this.PositionY = firstTouch.Location.Y;
        } 
    }
} 
```

Mnoho čistou em až hry implementace maximální rychlost, mimicking tradiční založené na řadiči pohyb. Ale nutné dodat, budete jednoduše implementaci okamžitou přesun naše kód kratší.


## <a name="creating-the-bullet-entity"></a>Vytváření odrážka entity

Druhý entity v našem jednoduchá hra je entita pro zobrazení odrážek. Stejně jako `Ship` entity, `Bullet` bude obsahovat entity `CCSprite` tak, aby se na obrazovce. Logika pro přesun se liší v tom, že není závislý na vstupu uživatele pro přesun; Místo toho `Bullet` instance přesune v přímce pomocí vlastnosti rychlosti.

Nejprve přidáme vytvoření nového souboru třídy Naše **entity** složku a pojmenujte ji **Bullet**:

![](entities-images/image5.png "Přidat nový soubor třídy do složky entity a pojmenujte ji odrážka")

Po přidání upravíme `Bullet.cs` code následujícím způsobem:


```csharp
using System;
using CocosSharp;

namespace EntityProject
{
    public class Bullet : CCNode
    {
        CCSprite sprite;

        public float VelocityX
        {
            get;
            set;
        }

        public float VelocityY
        {
            get;
            set;
        }

        public Bullet () : base()
        {
            sprite = new CCSprite ("bullet.png");
            // Making the Sprite be centered makes
            // positioning easier.
            sprite.AnchorPoint = CCPoint.AnchorMiddle;
            this.AddChild(sprite);

            this.Schedule (ApplyVelocity);
        }

        void ApplyVelocity(float time)
        {
            PositionX += VelocityX * time;
            PositionY += VelocityY * time;
        }
    }
} 
```

Kromě zajištění dostatečného změna soubor použitý pro `CCSprite` k `bullet.png`, kód v `ApplyVelocity` zahrnuje přesun logiku, která je založena na dva koeficienty: `VelocityX` a `VelocityY`.

`Schedule` Metoda umožňuje přidání delegátů k volání každých rámce. V takovém případě je právě přidávána `ApplyVelocity` metoda tak, aby naše odrážka přesune podle jeho hodnoty rychlosti. `Schedule` Metoda trvá `Action<float>`, kde parametr float určuje množství času (v sekundách) od poslední rámce, který používáme k implementaci přesun založené na čase. Od čas se měří hodnota v sekundách, pak naše rychlosti hodnoty představují přesun *pixelů za sekundu*.


## <a name="adding-bullets-to-gamelayer"></a>Přidávání do GameLayer odrážek

Před přidáme některé `Bullet` instance na našem herní budeme kontejner, konkrétně `List<Bullet>`. Upravit `GameLayer` tak, aby zahrnovala seznam odrážek:


```csharp
    public class GameLayer : CCLayer
    {
        Ship ship;
        List<Bullet> bullets;

        public GameLayer ()
        {
            ship = new Ship ();
            ship.PositionX = 240;
            ship.PositionY = 50;
            this.AddChild (ship);

            bullets = new List<Bullet> ();
        }
        ... 
```

Dále musíte k naplnění `Bullet` seznamu. Logiku pro kdy vytvořit `Bullet` by měl být součástí `Ship` entity, ale `GameLayer` zodpovídá za uložení seznamu odrážek. Budeme používat vzor objektu pro vytváření umožňující `Ship` entity k vytvoření `Bullet` instance. Tento objekt pro vytváření zveřejní událost, `GameLayer` může zpracovat. 

K tomu nejdřív přidáme složku do našich projekt s názvem **objekty Factory**, a poté přidejte novou třídu s názvem `BulletFactory`:

![](entities-images/image6.png "Přidat složku do projektu názvem objekty Factory, a poté přidejte novou třídu s názvem BulletFactory")

Dále budete implementaci `BulletFactory` třída singleton:


```csharp
using System;

namespace EntityProject
{
    public class BulletFactory
    {
        static Lazy<BulletFactory> self = 
            new Lazy<BulletFactory>(()=>new BulletFactory());

        // simple singleton implementation
        public static BulletFactory Self
        {
            get
            {
                return self.Value;
            }
        }

        public event Action<Bullet> BulletCreated;

        private BulletFactory()
        {

        }

        public Bullet CreateNew()
        {
            Bullet newBullet = new Bullet ();

            if (BulletCreated != null)
            {
                BulletCreated (newBullet);
            }

            return newBullet;
        }
    }
} 
```

`Ship` Entity, bude zpracovávat vytváření `Bullet` instancí – konkrétně je bude zpracovávat jak často `Bullet` (tj. jak často je aktivováno odrážkou) by měl vytvořit instance, jejich umístění a jejich rychlosti.

Upravit `Ship` konstruktor entity přidat nový `Schedule` volání a pak tuto metodu implementovat následujícím způsobem:


```csharp
...
public Ship () : base()
{
    sprite = new CCSprite ("ship.png");
    // Center the Sprite in this entity to simplify
    // centering the Ship on screen
    sprite.AnchorPoint = CCPoint.AnchorMiddle;
    this.AddChild(sprite);

    touchListener = new CCEventListenerTouchAllAtOnce();
    touchListener.OnTouchesMoved = HandleInput;
    AddEventListener(touchListener, this);

    Schedule (FireBullet, interval: 0.5f);

}

void FireBullet(float unusedValue)
{
    Bullet newBullet = BulletFactory.Self.CreateNew ();
    newBullet.Position = this.Position;
    newBullet.VelocityY = 100;
} 
...
```

Posledním krokem je vytvoření zpracovat nové `Bullet` instancí v `GameLayer` kódu. Přidání obslužné rutiny události pro `BulletCreated` událost, která přidá nově vytvořený `Bullet` k příslušné seznamy:


```csharp
...
public GameLayer ()
{
    ship = new Ship ();
    ship.PositionX = 240;
    ship.PositionY = 50;
    this.AddChild (ship);

    bullets = new List<Bullet> ();
    BulletFactory.Self.BulletCreated += HandleBulletCreated;
}

void HandleBulletCreated(Bullet newBullet)
{
    AddChild (newBullet);
    bullets.Add (newBullet);
}
... 
```

Můžete spustit hru a najdete v části teď `Ship` čistá `Bullet` instancí:

![](entities-images/image1.png "Spuštění hry a lodě bude čistá odrážka instancí")


## <a name="why-gamelayer-has-ship-and-bullets-members"></a>Proč GameLayer má členy lodě a odrážek

Naše `GameLayer` třída definuje dvě pole k udržení odkazů na našem instancí entit (`ship` a `bullets`), ale neprovede žádnou akci s nimi. Kromě toho jsou zodpovědní za vlastní chování, například přesun a čistá entity. Proto proč přidáme `ship` a `bullets` polí k `GameLayer`?

Z důvodu jsme přidali tito členové totiž úplné herní implementace by vyžadovaly logiky `GameLayer` pro interakci mezi různými entitami. Například tato hra mohou vyvíjet zahrnout nepřátel, které může být zničený přehrávače. Tyto nepřátel by být obsažená v `List` v `GameLayer`, logiky k testování a zda `Bullet` instancí v konfliktu s nepřátel by se provedla v `GameLayer` také. Jinými slovy `GameLayer` je kořenový adresář *vlastníka* všechny entity instance a je zodpovědná za interakce mezi instancí entit.


## <a name="bullet-destruction-considerations"></a>Důležité informace o odstraňování odrážka

Naše herní aktuálně chybí kód pro odstraňování `Bullet` instance. Každý `Bullet` instance má logiku pro přesun na obrazovce, ale jsme nepřidali žádný kód žádné mimo obrazovku a zrušení `Bullet` instance.

Kromě toho zničení `Bullet` instancí nemusí patřit v `GameLayer`. Například místo zničený, když mimo obrazovku, `Bullet` entita může mít logiku odstranění samotného po určité době. V takovém případě `Bullet` potřebuje způsob, jakým komunikovat, že by měl být zničený k `GameLayer`, mnoho jako `Ship` entity oznamovat `GameLayer` , nový `Bullet` byla vytvořena prostřednictvím `BulletFactory`.

Nejjednodušší řešením je rozbalte odpovědnost třídy objektu pro vytváření pro podporu odstraňování. Pak objektu pro vytváření oznámení o instanci entity zničen, který může zpracovávat jiné objekty, jako `GameLayer` odebrání entity instance z jeho seznamy. 

## <a name="summary"></a>Souhrn

Tato příručka ukazuje, jak vytvořit CocosSharp entity, která dědí z `CCNode` třídy. Tyto entity jsou samostatný objekty, zpracování vytvářet své vlastní vizuály a vlastní logiky. Tato příručka označí kód, který patří do entity (pohyb a vytvoření ostatní entity) z kódu, který je součástí v kořenovém kontejneru entit (kolizí a logiku interakce ostatní entity).

## <a name="related-links"></a>Související odkazy

- [Dokumentace CocosSharp rozhraní API](https://developer.xamarin.com/api/namespace/CocosSharp/)
- [Obsahu zip](https://github.com/xamarin/mobile-samples/blob/master/BouncingGame/Resources/Entities.zip?raw=true)
