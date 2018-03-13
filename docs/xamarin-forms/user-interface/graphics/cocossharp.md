---
title: "Použití CocosSharp v Xamarin.Forms"
description: "CocosSharp slouží k přidání do aplikace pro pokročilé vizualizaci přesné tvaru, image a vykreslování textu"
ms.topic: article
ms.prod: xamarin
ms.assetid: E0F404D5-5C6B-4288-92EC-78996C674E4E
ms.technology: xamarin-forms
author: charlespetzold
ms.author: chape
ms.date: 05/03/2016
ms.openlocfilehash: 395defa300da7b8f68746162d877a4fdb17ded9e
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 03/09/2018
---
# <a name="using-cocossharp-in-xamarinforms"></a>Použití CocosSharp v Xamarin.Forms

_CocosSharp slouží k přidání do aplikace pro pokročilé vizualizaci přesné tvaru, image a vykreslování textu_

> [!VIDEO https://youtube.com/embed/eYCx63FeqVU]

**Momentální 2016: Kokosové # v Xamarin.Forms**

## <a name="overview"></a>Přehled

CocosSharp je výkonná technologie pro zobrazení grafiky, čtení dotykové ovládání, přehrávání zvuku a správa obsahu. Tato příručka vysvětluje, jak přidat CocosSharp Xamarin.Forms aplikace. Pokrývá následující:

* [Co je CocosSharp?](#what)
* [Přidání balíčků CocosSharp Nuget](#nuget)
* [Návod: Přidání CocosSharp do aplikace na platformě Xamarin.Forms](#add)

<a name="what" />

## <a name="what-is-cocossharp"></a>Co je CocosSharp?

[CocosSharp](~/graphics-games/cocossharp/index.md) herní modul s otevřeným zdrojem, která je k dispozici na platformě Xamarin.
CocosSharp je efektivní runtime knihovnu, která zahrnuje následující funkce:

* Pomocí vykreslování obrázku [CCSprite – třída](https://developer.xamarin.com/api/type/CocosSharp.CCSprite/)
* Tvar vykreslování pomocí [CCDrawNode – třída](https://developer.xamarin.com/api/type/CocosSharp.CCDrawNode/)
* Pomocí logiky každých rámce [CCNode.Schedule – metoda](https://developer.xamarin.com/api/member/CocosSharp.CCNode.Schedule/p/System.Action%7BSystem.Single%7D/)
* Pomocí správy obsahu (načítání a uvolňování prostředků, jako jsou soubory PNG) [CCTextureCache – třída](https://developer.xamarin.com/api/type/CocosSharp.CCTextureCache/)
* Animace pomocí [CCAction – třída](https://developer.xamarin.com/api/type/CocosSharp.CCAction/)

Hlavním cílem je CocosSharp je zjednodušení vytváření 2D hry napříč platformami; však může být také skvělé přidání do aplikace Xamarin formuláře. Vzhledem k tomu, že hry obvykle vyžadují efektivní vykreslování a přesnou kontrolu nad vizuály, CocosSharp lze použít k přidání výkonné vizualizace a účinky na jiné herní aplikace.

Xamarin.Forms je založena na nativní, specifické pro platformu systémy uživatelského rozhraní. Například [ `Button`s](https://developer.xamarin.com/api/type/Xamarin.Forms.Button/) vypadat jinak, na iOS a Android a dokonce můžou lišit podle verze operačního systému. Naopak CocosSharp nepoužívá žádné vizuální objekty specifických pro platformy, takže všechny vizuální objekty vypadají stejně na všech platformách. Samozřejmě překlad IP adres a poměr stran se liší mezi zařízeními, a to může mít vliv na to, jak CocosSharp vykreslí jeho vizuální prvky. Tyto podrobnosti budou popsané v této příručce.

Podrobnější informace najdete v [CocosSharp části](~/graphics-games/cocossharp/index.md).

<a name="nuget" />

## <a name="adding-the-cocossharp-nuget-packages"></a>Přidání balíčků CocosSharp Nuget

Před použitím CocosSharp, vývojáři potřeba provést několik dodatky k jejich Xamarin.Forms projektu.
Tato příručka předpokládá Xamarin.Forms projektu s iOS, Android a PCL projektu.
Kód, bude napsán v projektu PCL; ale knihovny, je nutné přidat na iOS a Android projekty.

Balíček CocosSharp Nuget obsahuje všechny objekty, které jsou potřebné k vytváření objektů CocosSharp.
Balíček nuget CocosSharp.Forms zahrnuje `CocosSharpView` třídy, která se používá k hostiteli CocosSharp v Xamarin.Forms.
Přidat **CocosSharp.Forms** NuGet a **CocosSharp** bude automaticky přidáno také.
Chcete-li to provést, klikněte pravým tlačítkem na PCL <span class="UIItem">balíčky</span> složky a vyberte <span class="UIItem">přidat balíčky... </span>. Zadejte hledaný termín <span class="UIItem">CocosSharp.Forms</span>, vyberte <span class="UIItem">CocosSharp pro Xamarin.Forms</span>, pak klikněte na tlačítko <span class="UIItem">přidat balíček</span>.

![](cocossharp-images/image1.png "Balíčky dialogové okno Přidání")

Obě **CocosSharp** a **CocosSharp.Forms** balíčky NuGet se zařadí do projektu:

![](cocossharp-images/image2.png "Balíčky složky")

Zopakujte výše uvedené kroky pro projekty specifických pro platformy (třeba iOS a Android).

<a name="add" />

## <a name="walkthrough-adding-cocossharp-to-a-xamarinforms-app"></a>Návod: Přidání CocosSharp do aplikace na platformě Xamarin.Forms

Postupujte podle těchto kroků jednoduché CocosSharp zobrazení přidat do aplikace na platformě Xamarin.Forms:

1. [Vytváření Xamarin Forms stránky](#1)
1. [Přidání CocosSharpView](#2)
1. [Vytváření GameScene](#3)
1. [Přidání kruh](#4)
1. [Interakci s CocosSharp](#5)

Po zobrazení CocosSharp byla úspěšně přidána do aplikace na platformě Xamarin.Forms, přejděte [CocosSharp dokumentace](~/graphics-games/cocossharp/index.md) Další informace o vytváření obsahu pomocí CocosSharp.

<a name="1" />

### <a name="1-creating-a-xamarin-forms-page"></a>1. Vytváření Xamarin Forms stránky

CocosSharp může být hostovaný v kontejneru, Xamarin.Forms. Tato ukázka pro tuto stránku používá stránku s názvem `HomePage`. `HomePage` je rozdělený na polovinu podle `Grid` zobrazíte jak Xamarin.Forms a CocosSharp mohou být vykresleny současně na stejné stránce.

Nejprve, nastavte stránky tak, aby obsahovala `Grid` a dvě `Button` instancí:


```csharp
public class HomePage : ContentPage
{
public HomePage ()
    {
        // This is the top-level grid, which will split our page in half
        var grid = new Grid ();
        this.Content = grid;
        grid.RowDefinitions = new RowDefinitionCollection {
            // Each half will be the same size:
            new RowDefinition{ Height = new GridLength(1, GridUnitType.Star)},
            new RowDefinition{ Height = new GridLength(1, GridUnitType.Star)},
        };
        CreateTopHalf (grid);
        CreateBottomHalf (grid);
    }
    void CreateTopHalf(Grid grid)
    {
        // We'll be adding our CocosSharpView here:
    }
    void CreateBottomHalf(Grid grid)
    {
        // We'll use a StackLayout to organize our buttons
        var stackLayout = new StackLayout();
        // The first button will move the circle to the left when it is clicked:
        var moveLeftButton = new Button {
            Text = "Move Circle Left"
        };
        stackLayout.Children.Add (moveLeftButton);

        // The second button will move the circle to the right when clicked:
        var moveCircleRight = new Button {
            Text = "Move Circle Right"
        };
        stackLayout.Children.Add (moveCircleRight);
        // The stack layout will be in the bottom half (row 1):

        grid.Children.Add (stackLayout, 0, 1);
    }
}
```

V systému iOS `HomePage` se zobrazí, jak je znázorněno na následujícím obrázku:

![](cocossharp-images/image3.png "Snímek domovské stránky")

<a name="2" />

### <a name="2-adding-a-cocossharpview"></a>2. Přidání CocosSharpView

`CocosSharpView` Třída slouží k vložení do aplikace na platformě Xamarin.Forms CocosSharp. Vzhledem k tomu `CocosSharpView` dědí z [Xamarin.Forms.View](https://developer.xamarin.com/api/type/Xamarin.Forms.View/) třída, poskytuje známé rozhraní pro rozložení a použitím do kontejnerů rozložení, jako [Xamarin.Forms.Grid](https://developer.xamarin.com/api/type/Xamarin.Forms.Grid/). Přidejte nový `CocosSharpView` do projektu dokončení `CreateTopHalf` metoda:


```csharp
void CreateTopHalf(Grid grid)
{
    // This hosts our game view.
    var gameView = new CocosSharpView () {
        // Notice it has the same properties as other XamarinForms Views
        HorizontalOptions = LayoutOptions.FillAndExpand,
        VerticalOptions = LayoutOptions.FillAndExpand,
        // This gets called after CocosSharp starts up:
        ViewCreated = HandleViewCreated
    };
    // We'll add it to the top half (row 0)
    grid.Children.Add (gameView, 0, 0);
}
```

Inicializace CocosSharp není okamžitý, takže pro při registraci události `CocosSharpView` dokončil jeho vytvoření. K tomu `HandleViewCreated` metoda:


```csharp
void HandleViewCreated (object sender, EventArgs e)
{
    var gameView = sender as CCGameView;
    if (gameView != null)
    {
        // This sets the game "world" resolution to 100x100:
        gameView.DesignResolution = new CCSizeI (100, 100);
        // GameScene is the root of the CocosSharp rendering hierarchy:
        gameScene = new GameScene (gameView);
        // Starts CocosSharp:
        gameView.RunWithScene (gameScene);
    }
}
```

`HandleViewCreated` Metoda má dvě důležité podrobnosti, které jsme budete vidí. První je `GameScene` třídy, která budou vytvořeny v další části. Je důležité si uvědomit, že aplikace nebude kompilace až `GameScene` je vytvořena a `gameScene` odkaz na instanci je vyřešený.

Je druhý důležité podrobnosti `DesignResolution` vlastnosti, která definuje oblast viditelné ve hře CocosSharp objektů. `DesignResolution` Vlastnost se hledá v po vytvoření `GameScene`.

<a name="3" />

### <a name="3-creating-the-gamescene"></a>3. Vytváření GameScene

`GameScene` Třída dědí z na CocosSharp `CCScene`. `GameScene` je první bod, kde budeme řešit čistě CocosSharp. Kód obsažené v `GameScene` bude fungovat v jakékoli aplikaci, CocosSharp, zda je umístěna v rámci projektu Xamarin.Forms, nebo ne.

`CCScene` Třída je visual kořenovém všechny CocosSharp vykreslování. Žádné viditelné CocosSharp objekt musí být obsažena v `CCScene`. Přesněji řečeno, vizuální objekty musí být přidaný do `CCLayer` instance a ty `CCLayer` instancí musí být přidaný do `CCScene`.

Následující graf může pomoct vizualizovat typické CocosSharp hierarchie:

![](cocossharp-images/image4.png "Typical CocosSharp Hierarchy")

Pouze jeden `CCScene` může být aktivní v jednom okamžiku. Většina her používá více `CCLayer` instance řazení obsahu, ale naše aplikace používá jenom jeden. Podobně Většina her používat víc vizuální objekty, ale nemůžeme budete mít pouze jednu v naší aplikaci. Podrobnější diskuzi o CocosSharp visual hierarchie lze nalézt v [skákání herní návod](~/graphics-games/cocossharp/first-game/index.md).

Zpočátku `GameScene` třída bude téměř prázdný – stačí vytvoříme ji vyhovět odkaz v `HomePage`. Přidejte novou třídu do vaší PCL s názvem `GameScene`. Musí dědit z `CCScene` třídy následujícím způsobem:


```csharp
public class GameScene : CCScene
{
    public GameScene (CCGameView gameView) : base(gameView)
    {

    }
}
```

Teď, když `GameScene` je definován, jsme mohli vrátit k `HomePage` a přidejte pole:


```csharp
// Keep the GameScene at class scope
// so the button click events can access it:
GameScene gameScene;
```

Nyní jsme můžete zkompilovat naše projektu a spusťte ji zobrazíte CocosSharp systémem. Jsme nepřidali nic, aby naše `GameScene,` tak, aby horní polovině naší stránce s začernit – výchozí barvu scény CocosSharp:

![](cocossharp-images/image5.png "Blank GameScene")

<a name="4" />

### <a name="4-adding-a-circle"></a>4. Přidání kruh

Aplikace má aktuálně spuštěné instance CocosSharp stroje, zobrazení prázdnou `CCScene`. Potom přidáme objekt visual: kruh. `CCDrawNode` Třídu lze použít k vykreslení celou řadu geometrické obrazce, jak je uvedeno v [geometrie kreslení pomocí Průvodce CCDrawNode](~/graphics-games/cocossharp/ccdrawnode.md).

Přidat kruh na našem `GameScene` třídy a doložit v konstruktoru, jak je znázorněno v následujícím kódu:


```csharp
public class GameScene : CCScene
{
    CCDrawNode circle;
    public GameScene (CCGameView gameView) : base(gameView)
    {
        var layer = new CCLayer ();
        this.AddLayer (layer);
        circle = new CCDrawNode ();
        layer.AddChild (circle);
        circle.DrawCircle (
            // The center to use when drawing the circle,
            // relative to the CCDrawNode:
            new CCPoint (0, 0),
            radius:15,
            color:CCColor4B.White);
        circle.PositionX = 20;
        circle.PositionY = 50;
    }
}
```

Spuštění aplikace teď zobrazuje kruh na levé straně oblasti CocosSharp zobrazení:

![](cocossharp-images/image6.png "Kruh v GameScene")


#### <a name="understanding-designresolution"></a>Principy DesignResolution

Teď, když se zobrazí objekt visual CocosSharp nám prozkoumat `DesignResolution` vlastnost.

`DesignResolution` Představuje šířku a výšku oblasti CocosSharp pro umístění a velikosti. Skutečné rozlišení oblasti se měří v *pixelů* při `DesignResolution` se měří ve světě *jednotky*. Následující diagram znázorňuje rozlišení různých částí zobrazení, zobrazovat na zařízení iPhone 5 rozlišení obrazovky 640 x 1136 pixelů:

![](cocossharp-images/image7.png "iPhone 5s návrhu řešení")

Diagramu výše zobrazí na vnější straně obrazovky v černého textu v pixelech. Jednotky se zobrazí uvnitř diagram bílý text. Zde jsou některé důležité podrobnosti zobrazené výše:

* Zobrazení CocosSharp pochází z vlevo dole. Posunutí doprava zvyšuje hodnotu X a přesunutí nahoru hodnoty Y. Všimněte si, že je obrácený hodnotu Y ve srovnání s některé jiné stroje 2D rozložení, kde (0,0) je horní části nalevo od plátna.
* Výchozí chování CocosSharp je zachování poměru stran jeho zobrazení. Vzhledem k tomu, že první řádek v mřížce je větší než výška, CocosSharp nevyplní celou šířku jeho buňky znázorněného desítkovém bílé rámeček. Toto chování může být změněn, jak je popsáno v [Průvodce zpracování více řešení v CocosSharp](~/graphics-games/cocossharp/resolutions.md).
* V tomto příkladu bude udržovat CocosSharp zobrazení oblasti 100 jednotek vodorovně a svisle bez ohledu na velikost nebo poměr stran jeho zařízení. To znamená, že kód můžete předpokládat, že představuje X = 100 vázaný úplně vpravo-o CocosSharp zobrazit, rozložení pro zachování konzistentní u všech zařízení.


#### <a name="ccdrawnode-details"></a>Podrobnosti o CCDrawNode

Naše používá jednoduchou aplikaci `CCDrawNode` třída k vykreslení kruh. Tato třída může být velmi užitečná pro obchodní aplikace, protože poskytuje vektorové geometrie vykreslování – funkce chybí Xamarin.Forms. Kromě kroužky `CCDrawNode` třídu lze použít k vykreslení obdélníků, křivky, řádky a vlastní mnohoúhelníky. `CCDrawNode` je také snadno použitelný protože nevyžaduje použití soubory bitových kopií (například .png). Podrobnější diskuzi o CCDrawNode lze nalézt v [geometrie kreslení pomocí Průvodce CCDrawNode](~/graphics-games/cocossharp/ccdrawnode.md).

<a name="5" />

### <a name="5-interacting-with-cocossharp"></a>5. Interakci s CocosSharp

Vizuální prvky CocosSharp (například `CCDrawNode`) dědí `CCNode` třídy. `CCNode` poskytuje dvě vlastnosti, které se dají použít pro umístění objektu relativně k nadřazenému: `PositionX` a `PositionY`. Naše kód aktuálně používá tyto dvě vlastnosti na pozici střed kruhu, jak ukazuje tento fragment kódu:


```csharp
circle.PositionX = 20;
circle.PositionY = 50;
```

Je důležité si uvědomit, že CocosSharp objekty jsou umístěny hodnotami explicitní pozici, na rozdíl od většiny Xamarin.Forms zobrazení, která se automaticky umístí podle chování ovládacích prvků jejich nadřazené rozložení.

Přidáme kód chcete umožnit uživatelům dvě tlačítka Přesunout doleva nebo doprava na kruh 10 jednotkami (není pixelů, protože kruhu nevykresluje v prostoru CocosSharp world jednotku). Nejdříve vytvoříme dvě veřejné metody v `GameScene` třídy:


```csharp
public void MoveCircleLeft()
{
    circle.PositionX -= 10;
}

public void MoveCircleRight()
{
    circle.PositionX += 10;
}
```

Potom přidáme obslužné rutiny pro dvě tlačítka v `HomePage` reakce na kliknutí. Po dokončení naše `CreateBottomHalf` metoda obsahuje následující kód:


```csharp
void CreateBottomHalf(Grid grid)
{
    // We'll use a StackLayout to organize our buttons
    var stackLayout = new StackLayout();

    // The first button will move the circle to the left when it is clicked:
    var moveLeftButton = new Button {
        Text = "Move Circle Left"
    };
    moveLeftButton.Clicked += (sender, e) => gameScene.MoveCircleLeft ();
    stackLayout.Children.Add (moveLeftButton);

    // The second button will move the circle to the right when clicked:
    var moveCircleRight = new Button {
        Text = "Move Circle Right"
    };
    moveCircleRight.Clicked += (sender, e) => gameScene.MoveCircleRight ();
    stackLayout.Children.Add (moveCircleRight);

    // The stack layout will be in the bottom half (row 1):
    grid.Children.Add (stackLayout, 0, 1);
}
```

Kruhu CocosSharp přesune teď v odpovědi na kliknutí. Můžeme také jasně uvidí hranice plátno CocosSharp přesunutím kruhu dostatečně doleva nebo doprava:

![](cocossharp-images/image8.png "GameScene s přesunutím kruhu.")

## <a name="summary"></a>Souhrn

Tato příručka ukazuje, jak přidat do existující Xamarin.Forms CocosSharp projektu, jak vytvořit interakce mezi Xamarin.Forms a CocosSharp a popisuje různé aspekty při vytváření rozložení v CocosSharp.

Herní modul CocosSharp nabízí mnoho funkcí a hloubku, aby tato příručka pouze scratches prostor co můžete dělat CocosSharp. Vývojáři zájem o další materiály o CocosSharp najdete mnoho články v [CocosSharp části](~/graphics-games/cocossharp/index.md).



## <a name="related-links"></a>Související odkazy

- [Rozhraní API CocosSharp](https://developer.xamarin.com/api/root/CocosSharp/)
- [CocosSharpForms (sample)](https://developer.xamarin.com/samples/xamarin-forms/CocosSharpForms/)
