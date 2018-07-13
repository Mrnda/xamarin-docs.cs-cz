---
title: Používání Cocossharpu v Xamarin.Forms
description: Cocossharpu lze přidat do aplikace pro pokročilé vizualizace přesné tvar, image a vykreslování textu
ms.prod: xamarin
ms.assetid: E0F404D5-5C6B-4288-92EC-78996C674E4E
ms.technology: xamarin-forms
author: charlespetzold
ms.author: chape
ms.date: 05/03/2016
ms.openlocfilehash: c823eb27552f0a42ad428ed6f36790e925079295
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/12/2018
ms.locfileid: "38998804"
---
# <a name="using-cocossharp-in-xamarinforms"></a>Používání Cocossharpu v Xamarin.Forms

_Cocossharpu lze přidat do aplikace pro pokročilé vizualizace přesné tvar, image a vykreslování textu_

> [!VIDEO https://youtube.com/embed/eYCx63FeqVU]

**Rozvoj 2016: Cocos # v Xamarin.Forms**

## <a name="overview"></a>Přehled

Cocossharpu je flexibilní a výkonné technologie pro zobrazení grafické, dotykové ovládání pro čtení, přehrávání audio a správě obsahu. Tato příručka vysvětluje, jak přidat Cocossharpu aplikace Xamarin.Forms. Zahrnuje následující:

* [Co je Cocossharpu?](#what)
* [Přidávají se balíčky Nuget Cocossharpu](#nuget)
* [Návod: Přidání Cocossharpu do aplikace na platformě Xamarin.Forms](#add)

<a name="what" />

## <a name="what-is-cocossharp"></a>Co je Cocossharpu?

[Cocossharpu](~/graphics-games/cocossharp/index.md) je open source herní modul, který je dostupný na platformě Xamarin.
Cocossharpu je modul runtime úsporných knihovnu, která zahrnuje následující funkce:

* Použití vykreslování obrázků [CCSprite třídy](https://developer.xamarin.com/api/type/CocosSharp.CCSprite/)
* Obrazec pomocí vykreslení [třídy CCDrawNode](https://developer.xamarin.com/api/type/CocosSharp.CCDrawNode/)
* Použití logiky každých rámce [CCNode.Schedule – metoda](https://developer.xamarin.com/api/member/CocosSharp.CCNode.Schedule/p/System.Action%7BSystem.Single%7D/)
* Pomocí správy obsahu (načítání a uvolňování prostředků, jako jsou soubory ve formátu PNG) [CCTextureCache třídy](https://developer.xamarin.com/api/type/CocosSharp.CCTextureCache/)
* Použití animací [třídy CCAction](https://developer.xamarin.com/api/type/CocosSharp.CCAction/)

Cocossharpu v hlavním cílem je zjednodušit proces vytváření 2D her pro různé platformy; však může být také skvělé přidání do aplikací pro formuláře Xamarin. Protože hry obvykle vyžadují efektivní vykreslování a mít naprostou kontrolu nad vizuály, Cocossharpu umožňuje přidat do aplikace bez hru výkonnou vizualizaci a efekty.

Xamarin.Forms je postavené na nativní, specifické pro platformu systémy uživatelského rozhraní. Například [ `Button`s](xref:Xamarin.Forms.Button) vypadat jinak v Iosu a Androidu a může dokonce se liší podle verze operačního systému. Naopak Cocossharpu nepoužívá všechny vizuální objekty specifické pro platformu, takže se zobrazí všechny vizuální objekty identické na všech platformách. Samozřejmě překlad IP adres a poměr stran lišit mezi zařízeními, a to může mít vliv na to, jak Cocossharpu vykreslí její vizuály. Tyto podrobnosti probereme později v tomto průvodci.

Podrobnější informace najdete v [Cocossharpu části](~/graphics-games/cocossharp/index.md).

<a name="nuget" />

## <a name="adding-the-cocossharp-nuget-packages"></a>Přidávají se balíčky Nuget Cocossharpu

Před použitím Cocossharpu, vývojáři muset udělat pár dodatky k jejich projektu Xamarin.Forms.
Tento průvodce to předpokládá projekt Xamarin.Forms s iOS, Android a .NET Standard projekt knihovny.
Veškerý kód, bude napsán v .NET Standard knihovny projektu. knihovny však musíte přidat do iOS a Android projekty.

Balíček Nuget Cocossharpu obsahuje všechny objekty, které jsou potřeba k vytvoření objektů Cocossharpu.
Balíček nuget CocosSharp.Forms zahrnuje `CocosSharpView` třídu, která se používá k hostiteli Cocossharpu v Xamarin.Forms.
Přidat **CocosSharp.Forms** NuGet a **Cocossharpu** se automaticky přidají také.
Chcete-li to provést, klikněte pravým tlačítkem na <span class="UIItem">balíčky</span> složky projekt knihovny .NET Standard a vyberte <span class="UIItem">přidat balíčky... </span>. Zadejte hledaný termín <span class="UIItem">CocosSharp.Forms</span>vyberte <span class="UIItem">Cocossharpu pro Xamarin.Forms</span>, pak klikněte na tlačítko <span class="UIItem">přidat balíček</span>.

![](cocossharp-images/image1.png "Přidat Dialog balíčky")

Obě **Cocossharpu** a **CocosSharp.Forms** balíčky NuGet se přidají do projektu:

![](cocossharp-images/image2.png "Složky balíčků")

Opakujte předchozí postup pro konkrétní platformu projekty (jako je iOS a Android).

<a name="add" />

## <a name="walkthrough-adding-cocossharp-to-a-xamarinforms-app"></a>Návod: Přidání Cocossharpu do aplikace na platformě Xamarin.Forms

Postupujte podle těchto kroků přidejte jednoduché Cocossharpu zobrazení aplikace na platformě Xamarin.Forms:

1. [Vytváření Xamarin Forms stránky](#1)
1. [Přidání CocosSharpView](#2)
1. [Vytváří GameScene](#3)
1. [Přidání kruh](#4)
1. [Interakce s Cocossharpem](#5)

Po zobrazení Cocossharpu úspěšně jste přidali do aplikace na platformě Xamarin.Forms, přejděte na web [Cocossharpu dokumentaci](~/graphics-games/cocossharp/index.md) Další informace o vytváření obsahu s Cocossharpem.

<a name="1" />

### <a name="1-creating-a-xamarin-forms-page"></a>1. Vytváření Xamarin Forms stránky

Je možné hostovat Cocossharpu v Xamarin.Forms kontejneru. Tento příklad pro tuto stránku používá stránku s názvem `HomePage`. `HomePage` je rozdělený na polovinu podle `Grid` zobrazit, jak Xamarin.Forms a Cocossharpu lze vykreslit souběžně na stejné stránce.

Nejprve, nastavte na stránce tak, aby obsahovala `Grid` a dva `Button` instancí:


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

V Iosu `HomePage` se zobrazí, jak je znázorněno na následujícím obrázku:

![](cocossharp-images/image3.png "Snímek obrazovky s domovskou stránku")

<a name="2" />

### <a name="2-adding-a-cocossharpview"></a>2. Přidání CocosSharpView

`CocosSharpView` Třída slouží k vložení Cocossharpu do aplikace na platformě Xamarin.Forms. Protože `CocosSharpView` dědí z [Xamarin.Forms.View](xref:Xamarin.Forms.View) třídy, poskytuje známému rozhraní pro rozložení a může sloužit do kontejnerů rozložení, jako [Xamarin.Forms.Grid](xref:Xamarin.Forms.Grid). Přidat nový `CocosSharpView` do projektu provedením `CreateTopHalf` metody:


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

Inicializace Cocossharpu není okamžitý, takže zaregistrujte se na událost při `CocosSharpView` dokončil jeho vytvoření. K tomu `HandleViewCreated` metody:


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

`HandleViewCreated` Metoda má dva důležité podrobnosti, které nám budete hledat. První je `GameScene` třídy, které budou vytvořeny v další části. Je důležité si uvědomit, že dokud nebude kompilovat aplikaci `GameScene` se vytvoří a `gameScene` odkazu na instanci je vyřešený.

Je druhý důležité podrobnosti `DesignResolution` vlastnost, která definuje hry pro objekty Cocossharpu viditelná oblast. `DesignResolution` Vlastnost se podívali na po vytvoření `GameScene`.

<a name="3" />

### <a name="3-creating-the-gamescene"></a>3. Vytváří GameScene

`GameScene` Třída dědí z vaší Cocossharpu `CCScene`. `GameScene` je první bod, ve kterém se zabýváme čistě s Cocossharpem. Kód obsažený v `GameScene` bude fungovat ve všech aplikacích Cocossharpu, zda je umístěno v rámci projektu Xamarin.Forms nebo ne.

`CCScene` Visual kořenové veškerého vykreslování Cocossharpu je třída. Musí obsahovat jakéhokoliv viditelného objektu Cocossharpu `CCScene`. Přesněji řečeno, vizuální objekty musí být přidané do `CCLayer` instancí a ty `CCLayer` instancí musí být přidané do `CCScene`.

Následující graf vám můžou pomoct vizualizovat typické Cocossharpu hierarchie:

![](cocossharp-images/image4.png "Typical CocosSharp Hierarchy")

Pouze jeden `CCScene` aktivní může být najednou. Většina her používá více `CCLayer` instancí k řazení obsahu, ale naše aplikace používá pouze jeden. Podobně Většina her používá víc vizuální objekty, ale pouze jsme si v naší aplikaci. Podrobnější diskuzi o Cocossharpu visual hierarchie najdete v [hry BouncingGame návod](~/graphics-games/cocossharp/bouncing-game.md).

Zpočátku `GameScene` třídy bude téměř prázdný – stačí vytvoříme vám ho tím se uspokojí odkaz v `HomePage`. Přidejte novou třídu do projektu knihovny .NET Standard s názvem `GameScene`. By měla dědit z `CCScene` třídy následujícím způsobem:


```csharp
public class GameScene : CCScene
{
    public GameScene (CCGameView gameView) : base(gameView)
    {

    }
}
```

Teď, když `GameScene` je definován, jsme mohli vrátit k `HomePage` a přidat pole:


```csharp
// Keep the GameScene at class scope
// so the button click events can access it:
GameScene gameScene;
```

Teď můžeme kompilace projektu a ji spustit a zjistit Cocossharpu systémem. Jsme nepřidali nic, aby naše `GameScene,` proto černé – výchozí barva scény Cocossharpu v horní polovině naši stránku:

![](cocossharp-images/image5.png "Prázdné GameScene")

<a name="4" />

### <a name="4-adding-a-circle"></a>4. Přidání kruh

Aplikace má aktuálně spuštěné instance modulu Cocossharpu zobrazení prázdná `CCScene`. V dalším kroku přidáme vizuální objekty: kruh. `CCDrawNode` Třída slouží k vykreslení širokou škálu geometrické tvary, jak je uvedeno v [kreslení geometrie v CCDrawNode příručce](~/graphics-games/cocossharp/ccdrawnode.md).

Přidejte kruh do našich `GameScene` třídy a instance v konstruktoru, jak je znázorněno v následujícím kódu:


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

Spuštění aplikace nyní zobrazuje kruhu na levé straně Cocossharpu zobrazované oblasti:

![](cocossharp-images/image6.png "Kruhu GameScene")


#### <a name="understanding-designresolution"></a>Principy DesignResolution

Teď, když se zobrazí vizuální objekty Cocossharpu, nám prozkoumat `DesignResolution` vlastnost.

`DesignResolution` Představuje šířku a výšku oblasti Cocossharpu pro umístění a velikosti objektů. Aktuální řešení v oblasti se měří v *pixelů* zatímco `DesignResolution` se měří v celém světě *jednotky*. Následující diagram znázorňuje rozlišení zobrazení, jak se zobrazuje na zařízeních iPhone 5 s rozlišení 640 × 1136 pixelů různých částí:

![](cocossharp-images/image7.png "iPhone 5s návrhu řešení")

Výše uvedeném diagramu zobrazí z vnějšku obrazovky v černý text v pixelech. Jsou jednotky zobrazeny uvnitř diagram bílým textem. Tady jsou některé důležité podrobnosti zobrazený nad:

* Zobrazení Cocossharpu pochází z vlevo dole. Přesuňte do pravé zvyšuje hodnotu X, a přesunutí nahoru hodnotu Y. Všimněte si, že je obrácený hodnotu Y ve srovnání s některých jiných modulů 2D rozložení, ve kterém (0; 0) je v levém horním rohu plátna.
* Výchozí chování Cocossharpu je zachování poměru stran u jeho zobrazení. Vzhledem k tomu, že první řádek v tabulce je větší než výška, Cocossharpu nevyplní celou šířku jeho buňky znázorněno tečkovaná bílý obdélník. Toto chování lze změnit, jak je popsáno v [provede zpracování řešení více rozlišení v Cocossharpu](~/graphics-games/cocossharp/resolutions.md).
* V tomto příkladu bude udržovat Cocossharpu zobrazení plochy 100 jednotek šířku a výšku bez ohledu na velikost nebo poměru stran u jeho zařízení. To znamená, že kód, můžete předpokládat, že X = 100 představuje vázán úplně vpravo Cocossharpu zobrazíte rozložení zachování konzistentní vzhledem k aplikacím na všech zařízeních.


#### <a name="ccdrawnode-details"></a>Podrobnosti o CCDrawNode

Náš jednoduchý aplikace používá `CCDrawNode` třídy nakreslit kruh. Tato třída může být velmi užitečné pro obchodní aplikace, protože poskytuje geometrie vektorové vykreslování – funkce chybí Xamarin.Forms. Kromě kruzích `CCDrawNode` třída slouží k vykreslení obdélníky, křivek, řádky a vlastní mnohoúhelníky. `CCDrawNode` je také snadno se používá od nevyžaduje použití soubory obrázků (například PNG). Podrobnější diskuzi o CCDrawNode najdete v [kreslení geometrie v CCDrawNode příručce](~/graphics-games/cocossharp/ccdrawnode.md).

<a name="5" />

### <a name="5-interacting-with-cocossharp"></a>5. Interakce s Cocossharpem

Cocossharpu vizuálních prvků (například `CCDrawNode`) dědí `CCNode` třídy. `CCNode` poskytuje dvě vlastnosti, které je možné použít pro umístění objektu relativně k nadřazenému: `PositionX` a `PositionY`. Náš kód aktuálně používá tyto dvě vlastnosti k umístění střed kruhu, jak je znázorněno v tomto fragmentu kódu:


```csharp
circle.PositionX = 20;
circle.PositionY = 50;
```

Je důležité si uvědomit, že Cocossharpu objekty jsou umístěny ve explicitními hodnotami pozic, na rozdíl od většiny zobrazení Xamarin.Forms, která se automaticky umístí podle jejich chování z jejich nadřazených rozložení ovládacích prvků.

Přidáme kód umožňující uživateli dvě tlačítka přesun kruh vlevo nebo vpravo o 10 jednotek (ne pixelů, protože kreslí kruh v prostoru světa jednotky Cocossharpu). Nejprve vytvoříme dvě veřejné metody v `GameScene` třídy:


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

V dalším kroku přidáme obslužné rutiny pro dvě tlačítka v `HomePage` reakce na kliknutí. Až budete hotovi, naše `CreateBottomHalf` metoda obsahuje následující kód:


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

Kruh Cocossharpu teď přesune v reakci na kliknutí. Můžeme vidět také jasně hranice plátno Cocossharpu přesunutím kruhu dostatečně úplně vlevo nebo vpravo:

![](cocossharp-images/image8.png "GameScene s přesunem kruh")

## <a name="summary"></a>Souhrn

Tato příručka ukazuje, jak přidat do existující Xamarin.Forms Cocossharpu projektu, jak vytvořit interakce mezi Xamarin.Forms a Cocossharpu a tento článek popisuje různé aspekty při vytváření rozložení v Cocossharpu.

Herní engine Cocossharpu nabízí mnoho funkcí a hloubka, tak tato příručka pouze zjednodušené co můžete dělat Cocossharpu. Vývojáři zájem o další materiály o Cocossharpu můžete najít mnoho články v [Cocossharpu části](~/graphics-games/cocossharp/index.md).



## <a name="related-links"></a>Související odkazy

- [Rozhraní API Cocossharpu](https://developer.xamarin.com/api/root/CocosSharp/)
- [CocosSharpForms (sample)](https://developer.xamarin.com/samples/xamarin-forms/CocosSharpForms/)
