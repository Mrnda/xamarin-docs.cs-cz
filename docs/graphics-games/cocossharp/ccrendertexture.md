---
title: Výkon a vizuální efekty s CCRenderTexture
description: CCRenderTexture vývojářům umožňuje zvýšit výkon jejich CocosSharp hry snížením kreslení volání a slouží k vytváření vizuálních efektů. Tato příručka doprovází ukázka CCRenderTexture zajistit praktických příklad toho, jak efektivně používat tuto třídu.
ms.topic: article
ms.prod: xamarin
ms.assetid: F02147C2-754B-4FB4-8BE0-8261F1C5F574
ms.technology: xamarin-cross-platform
author: charlespetzold
ms.author: chape
ms.openlocfilehash: 36661344fc0f4b9e132e3f721c50f82f3a8db057
ms.sourcegitcommit: 4f1b508caa8e7b6ccf85d167ea700a5d28b0347e
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/03/2018
---
# <a name="performance-and-visual-effects-with-ccrendertexture"></a>Výkon a vizuální efekty s CCRenderTexture

_CCRenderTexture vývojářům umožňuje zvýšit výkon jejich CocosSharp hry snížením kreslení volání a slouží k vytváření vizuálních efektů. Tato příručka doprovází ukázka CCRenderTexture zajistit praktických příklad toho, jak efektivně používat tuto třídu._

`CCRenderTexture` Třída poskytuje funkce pro vykreslování více objektů CocosSharp do jednoho texture. Po vytvoření, `CCRenderTexture` instance lze použít k vykreslení grafiky efektivně a implementovat vizuálních efektů. `CCRenderTexture` umožňuje více objektů k vykreslení na jednom texture jednou. Potom může být tento texture znovu každých rámce, snižuje celkový počet volání kreslení.

Tato příručka prozkoumá postup použití `CCRenderTexture` objekt, který chcete zvýšit výkon vykreslování karty ve hře shromážditelného karty (CCG). Také ukazuje, jak `CCRenderTexture` lze použít na průhledný celý entity. Tato příručka odkazuje `CCRenderTexture` [ukázkový projekt](https://developer.xamarin.com/samples/mobile/CCRenderTexture/).

![](ccrendertexture-images/image1.png "Tato příručka odkazuje CCRenderTexture ukázkový projekt")


## <a name="card--a-typical-entity"></a>Karta – typické entity

Před hledáním na tom, jak používat `CCRenderTexture` objektu, nejprve jsme budete Seznamte označována s `Card` entity, který Pokud chcete prozkoumat použijeme v rámci tohoto projektu `CCRenderTexture` – třída. `Card` Třída je typické entity, vzoru entity uvedených v následující [Entity průvodce](~/graphics-games/cocossharp/entities.md). Karta třída má všechny jeho součásti visual (instance `CCSprite` a `CCLabel`) uveden jako pole:


```csharp
class Card : CCNode
{
    bool usesRenderTexture;
    List<CCNode> visualComponents = new List<CCNode>();
    CCSprite background;
    CCSprite colorIcon;
    CCSprite monsterSprite;
    CCLabel monsterNameDisplay;
    CCLabel hpDisplay;
    CCLabel descriptionDisplay;
```

Karta instance lze vykreslit pomocí `CCRenderTexture`, nebo kreslení jednotlivých součástí visual jednotlivě. Jednotlivé komponenty sice objekt nezávislé `CCNode` Správa nadřazených systém používaný v entit umožňuje `Card` chovat jako jednoho objektu – alespoň ve většině případů. Například pokud `Card` změnit jejich umístění, ke změně velikosti nebo otáčet entity a potom dopad na všechny obsažené objekty visual aby karty zobrazí jako jednoho objektu. Informace o kartách chovat jako jeden objekt, jsme můžete upravit `GameLayer.AddedToScene` metodu a nastavit `useRenderTextures` proměnnou `false`:

    
```csharp
protected override void AddedToScene ()
{
    base.AddedToScene();
    GameView.Stats.Enabled = true;
    const bool useRenderTextures = false;
    ...
}
```

`GameLayer` Kód nepřesouvá každý vizuální prvek nezávisle, ještě každou vizuální prvek v rámci `Card` entity je umístěný správně:

![](ccrendertexture-images/image1.png "Kód GameLayer nepřesouvá každý vizuální prvek nezávisle ještě každou vizuální prvek v rámci entity karta je správně nastavený")

Ukázka je zakódovaný vystavit dva problémy, které může dojít, když se vykreslí každý visual component:

- Z důvodu více volání kreslení můžete sníží výkon
- Některé vizuální efekty, jako je například transparentnosti, nelze implementovat přesně, jak se podíváme na novější


### <a name="card-draw-calls"></a>Karta kreslení volání

Naše kód je zjednodušení co může nacházet ve úplnou *shromážditelného hru* (CCG) jako je například "The shromažďování: Magic" nebo "Hearthstone". Naše herní pouze zobrazuje tři karty najednou a má malý počet možných jednotky (modrá, zelená a oranžová). Naopak úplné hru může na obrazovce mají více než 20 karty v daném okamžiku a přehrávače může mít stovky karty lze vybírat při vytváření svých balíčky. I když naše herní nezpůsobuje žádné aktuálně problémy s výkonem, může být úplné hry s podobné implementace.

CocosSharp poskytuje určitý pohled na vykreslování výkonu vystavení volání kreslení prováděných za rámce. Naše `GameLayer.AddedToScene` metoda nastaví `GameView.Stats.Enabled` k `true`, což výkonu informace zobrazené v levém dolním rohu obrazovky:

![](ccrendertexture-images/image2.png "Metoda GameLayer.AddedToScene nastaví na hodnotu true, což vede k výkonu informace zobrazené v levém dolním rohu obrazovky GameView.Stats.Enabled")

Všimněte si, že bez ohledu na obrazovce s tří karet, se musí devatenáct volání kreslení (Kreslení každý karty má za následek šest volání, textu zobrazení účty informace o výkonu pro další). Kreslení volání mají významný dopad na výkon hra, takže CocosSharp poskytuje několik způsobů, jak je snížit. Jednoho technika je podrobněji popsaná [CCSpriteSheet průvodce](~/graphics-games/cocossharp/ccspritesheet.md). Jiné technik je použít `CCRenderTexture` ke snížení každé entity do jednoho volání, jako podíváme v této příručce.


### <a name="card-transparency"></a>Karta průhlednosti

Naše `Card` zahrnuje entity `Opacity` vlastnosti do ovládacího prvku průhlednosti, jak je znázorněno v následující fragment kódu:


```csharp
public override byte Opacity
{
    get
    {
        return base.Opacity;
    }
    set
    {
        base.Opacity = value;
        if (usesRenderTexture)
        {
            this.renderTexture.Sprite.Opacity = value;
        }
        else
        {
            foreach (var component in visualComponents)
            {
                component.Opacity = value;
            }
        }
    }
}
```

Všimněte si, že vykreslení podporuje setter použití textury nebo vykreslování jednotlivé komponenty jednotlivě. Zobrazíte jeho dopad změnit `opacity` hodnotu `127` (přibližně poloviční krytí) v `GameLayer.AddedToScene` důsledkem toho bude každá součást má `Opacity` hodnotu `127`:


```csharp
protected override void AddedToScene ()
{
    base.AddedToScene();
    GameView.Stats.Enabled = true;
    const bool useRenderTextures = false;
    const byte opacity = 127;
    ...
}
```

Hra se teď vykreslení karet s některé průhlednost způsobuje se objevily tmavšího vzhledem k tomu, že je černá na pozadí:

![](ccrendertexture-images/image3.png "Hry teď vykreslí karet s některé průhlednost způsobuje se objevily tmavšího vzhledem k tomu, že je černá na pozadí")

Na první pohled může vypadat, jako kdyby naše karty jste správně provedli transparentní. Na snímku obrazovky se však zobrazí několik visual problémy.

Vzhledem k tomu, že je naše pozadí černá, by Očekáváme, že každý součástí naší karty stane tmavšího kvůli průhlednost. To znamená Čím transparentní bude na kartě, bude tmavšího. V krytí 0 `Card` bude zcela transparentní (úplně černá). Však některé části našich karty nebyla budou tmavšího, po krytí byl změněn na `127`. I horší některé části našich karty ve skutečnosti se stala světlejší při jejich aktivovala více transparentní. Podívejme se na části našich karty, které byly černým *před* byly transparentní – konkrétně text podrobností a černým jsou podrobněji popsány dále kolem obrázku monster. Pokud jsme tyto umístěte vedle sebe, jsme dopad použití průhlednost najdete:

![](ccrendertexture-images/image4.png "Pokud se umístit vedle sebe, si můžete prohlédnout dopad použití průhlednosti")

Jak je uvedeno výše, by měly být všechny části karty tmavšího při čím dál víc transparentní, ale v různých oblastech to tak není:

- Osnova robot stane světlejší (přejde z černé šedá)
- Text elementu description bude světlejší (přejde z černé šedá)
- Zelená součástí robota stane méně zcela využita, ale nebude se stát tmavšího

Pomoc při vizualizovat, proč k tomu dojde, musíme mějte na paměti, že je jednotlivých součástí visual nezávisle vykresleného, každý částečně transparentní. První součást visual vykreslovat je na kartu pozadí. Následné transparentní prvky vykreslovat nad kartu a bude mít vliv na pozadí karty. Pokud jsme odebrat část textu z našich karty a přesunout dolů, tj. grafiku robot uvidíte, jak má dopad na pozadí karty na robota. Všimněte si oranžové řádku v horním poli můžete zobrazit na robota a oblasti robot, který se překrývá blue stripe v centru karty vykreslením tmavšího:

![](ccrendertexture-images/image5.png "Všimněte si oranžové řádku v horním poli můžete zobrazit na robota a oblasti robot, který se překrývá blue stripe v centru karty vykreslením tmavšího")

Použití `CCRenderTexture` umožňuje, aby celá karta transparentní bez dopadu na vykreslování jednotlivých součástí v rámci karty, protože jsme se zobrazí v této příručce.


## <a name="using-ccrendertexture"></a>Pomocí CCRenderTexture

Teď, když jste si myslíme, problémy s jednotlivě vykreslování jednotlivé komponenty, jsme vám zapnout vykreslování na `CCRenderTexture` a porovnání chování.

Chcete-li povolit vykreslování k `CCRenderTexture`, změnit `userRenderTextures` proměnnou `true` v `GameLayer.AddedToScene`:


```csharp
protected override void AddedToScene ()
{
    base.AddedToScene();
    GameView.Stats.Enabled = true;
    const bool useRenderTextures = true;
```


### <a name="card-draw-calls"></a>Karta kreslení volání

Pokud jsme teď spustit hru, uvidíme volání kreslení omezený ze devatenáct čtyři (každou kartu omezený ze šest pro jednu):

![](ccrendertexture-images/image6.png "Pokud hra se teď spustí, volání kreslení horší z devatenáct čtyři každou kartu omezený ze šest pro jednu")

Jak jsme uvedli může tento typ snížení mít významný dopad na hry s další visual entity na obrazovce.


### <a name="card-transparency"></a>Karta průhlednosti

Jednou `useRenderTextures` je nastaven na `true`, jinak vykreslí transparentní karty:

![](ccrendertexture-images/image7.png "Jinak vykreslí useRenderTextures nastavenou na hodnotu true, transparentní karty")

Umožňuje porovnat kartu transparentní robot pomocí textury vykreslení (vlevo) a bez (vpravo):

![](ccrendertexture-images/image8.png "Porovnání kartu transparentní robot pomocí vykreslování textury (levém) vs bez (vpravo)")

Nejobvyklejší rozdíly jsou podrobnosti text (černé místo světla šedá) a pohyblivý robot symbol (tmavý místo lehký a odbarvený).


## <a name="ccrendertexture-details"></a>Podrobnosti o CCRenderTexture

Teď, když jste viděli výhody použití `CCRenderTexture`, Podívejme se na tom, jak se používá v `Card` entity.

`CCRenderTexture` Je na plátno, který může být cílem vykreslování. Má dva hlavní rozdíly ve srovnání s herní obrazovky:

1. `CCRenderTexture` Potrvají mezi rámce. To znamená, že `CCRenderTexture` musí být vykreslen pouze při změnách. V našem případě `Card` entity nikdy změní, takže je poskytováno pouze jednou. Pokud existuje `Card` změnit součásti a potom na kartu by bylo potřeba překreslit k jeho `CCRenderTexture`. Například pokud hodnota HP (stavu body) změnit, pokud napadení, pak na kartu by třeba k vykreslení samotné tak, aby odrážely novou hodnotu HP.
1. `CCRenderTexture` v pixelech nejsou vázané na obrazovce. A `CCRenderTexture` může být větší nebo menší než rozlišení zařízení. `Card` Kód vytvoří `CCRenderTexture` pomocí velikosti jeho pozadí pohyblivý symbol. Karta obsahuje odkaz na `CCRenderTexture` názvem `renderTexture`:


```csharp
CCRenderTexture renderTexture;
```

`renderTexture` Instance zůstane `null` dokud `UseRenderTexture` je přiřazen vlastnost na hodnotu true, který volá `SwitchToRenderTexture`:


```csharp
private void SwitchToRenderTexture()
{
    // The card needs to be moved to the origin (0,0) so it's rendered on the render target. 
    // After it's rendered to the CCRenderTexture, it will be moved back to its old position
    var oldPosition = this.Position;
    // Make sure visuals are part of the card so they get rendered
    bool areVisualComponentsAlreadyAdded = this.Children != null && this.Children.Contains(visualComponents[0]);
    if (!areVisualComponentsAlreadyAdded)
    {
        // Temporarily add them so we can render the object:
        foreach (var component in visualComponents)
        {
            this.AddChild(component);
        }
    }
    // Create the render texture if it hasn't yet been made:
    if (renderTexture == null)
    {
        // Even though the game is zoomed in to create a pixellated look, we are using
        // high-resolution textures. Therefore, we want to have our canvas be 2x as big as 
        // the background so fonts don't appear pixellated
        var pixelResolution = background.ContentSize * 2;
        var unitResolution = background.ContentSize;
        renderTexture = new CCRenderTexture(unitResolution, pixelResolution);
        //renderTexture.Sprite.Scale = .5f;
    }
    // We don't want the render target to be a child of this when we update it:
    if (this.Children != null && this.Children.Contains(renderTexture.Sprite))
    {
        this.Children.Remove(renderTexture.Sprite);
    }
    // Move this instance back to the origin so it is rendered inside the render texture:
    this.Position = CCPoint.Zero;
    // Clears the CCRenderTexture
    renderTexture.BeginWithClear(CCColor4B.Transparent);
    // Visit renders this object and all of its children
    this.Visit();
    // Ends the rendering, which means the CCRenderTexture's Sprite can be used
    renderTexture.End();
    // We no longer want the individual components to be drawn, so remove them:
    foreach (var component in visualComponents)
    {
        this.RemoveChild(component);
    }
    // Move this back to its original position:
    this.Position = oldPosition;
    // add the render texture sprite to this:
    renderTexture.Sprite.AnchorPoint = CCPoint.Zero;
    this.AddChild(renderTexture.Sprite);
}
```

`SwitchToRenderTexture` Nelze volat metodu vždy, když textury je nutné aktualizovat. Může být volána, zda karta už používá jeho `CCRenderTexture` nebo přepíná na `CCRenderTexture` poprvé.

V následujících částech prozkoumat `SwitchToRenderTexture` metoda. 


### <a name="ccrendertexture-size"></a>Velikost CCRenderTexture

Konstruktor CCRenderTexture vyžaduje dvě sady dimenzí. První řídí velikost `CCRenderTexture` při kreslení a druhý určuje pixelů šířka a výška její obsah. `Card` Vytvoří instanci entity jeho `CCRenderTexture` pomocí na pozadí [ContentSize](https://developer.xamarin.com/api/property/CocosSharp.CCSprite.ContentSize/). Naše herní má `DesignResolution` z 512 podle 384, jak je znázorněno v `ViewController.LoadGame` na iOS a `MainActivity.LoadGame` v systému Android:


```csharp
int width = 512;
int height = 384;
// Set world dimensions
gameView.DesignResolution = new CCSizeI(width, height);
```

`CCRenderTexture` Konstruktor je volán s `background.ContentSize` jako první parametr označující, že `CCRenderTexture` by měla být právě tak velké, jaká na pozadí `CCSprite`. Od karty pozadí `CCSprite` je 200 pixelů vysoký, karta bude zabírat přibližně polovinu výšky na obrazovce.

Druhý parametr předaný `CCRenderTexture` konstruktor Určuje rozlišení pixelů `CCRenderTexture`. Jak je popsáno v [CocosSharp řešení průvodce](~/graphics-games/cocossharp/resolutions.md), šířku a výšku oblasti lze zobrazit v herní jednotky je často není stejný jako pixelů rozlišení obrazovky. Podobně CCRenderTexture může použít větší řešení než jeho velikost, takže vizuální prvky se zobrazí na zařízení s vysokým rozlišením vyšším kontrastem.

Rozlišení pixelů je dvojnásobku velikosti CCRenderTexture, aby se zabránilo text z vypadající pixelován:


```csharp
var unitResolution = background.ContentSize;
var pixelResolution = background.ContentSize * 2;
renderTexture = new CCRenderTexture(unitResolution, pixelResolution);
```

Pokud chcete porovnat, jsme můžete změnit hodnotu pixelResolution tak, aby odpovídala `background.ContentSize` (bez se zdvojnásobí) a porovnání výsledek: 


```csharp
var unitResolution = background.ContentSize;
var pixelResolution = background.ContentSize;
renderTexture = new CCRenderTexture(unitResolution, pixelResolution);
```

![](ccrendertexture-images/image9.png "Chcete porovnat, můžete změnit hodnotu pixelResolution tak, aby odpovídala na pozadí. ContentSize bez probíhá se používají a porovnání výsledek")


### <a name="rendering-to-a-ccrendertexture"></a>Vykreslování CCRenderTexture

Vizuální objekty v CocosSharp obvykle se nevykreslí explicitně. Místo toho visual objekty jsou přidány na `CCLayer` který je součástí `CCScene`. CocosSharp automaticky vykreslí `CCScene` a jeho visual hierarchie v každém snímku bez jakékoli vykreslování kódu volána. 

Naopak `CCRenderTexture` musí být explicitně vykreslovány. Tato vykreslování lze rozdělit do tří kroků:

1. `CCRenderTexture.BeginWithClear` je volána, která určuje, že všechny následné vykreslování se zaměří volání `CCRenderTexture`.
1. Objekty, která dědí z `CCNode` (jako `Card` entity) jsou vykresleny také `CCRenderTexture` voláním `Visit`.
1. `CCRenderTexture.End` je volána, která určuje, že vykreslování k `CCRenderTexture` dokončení.

Libovolný počet objektů, které lze vykreslit k `CCRenderTexture` mezi jeho `Begin` a `End` volání. Před vykreslením, přidají se všech nezbytných objektů viditelné jako podřízené objekty:


```csharp
bool areVisualComponentsAlreadyAdded = this.Children != null && this.Children.Contains(visualComponents[0]);
if (!areVisualComponentsAlreadyAdded)
{
    // Temporarily add them so we can render the object:
    foreach (var component in visualComponents)
    {
        this.AddChild(component);
    }
}
```

`renderTexture` By neměl být část karty při vykreslování, proto je odebrána:


```csharp
// We don't want the render texture to be a child of the card 
// when we call Visit
if (this.Children != null && this.Children.Contains(renderTexture.Sprite))
{
    this.RemoveChild(renderTexture.Sprite);
}
```

Nyní `Card` instance může vykreslit své vlastní `CCRenderTexture` instance:


```csharp
// Clears the CCRenderTexture
renderTexture.BeginWithClear(CCColor4B.Transparent);
// Visit renders this object and all of its children
this.Visit();
// Ends the rendering, which means the CCRenderTexture's Sprite can be used
renderTexture.End();
```

Po dokončení vykreslování jednotlivé součásti jsou odebrány a `CCRenderTexture` je znovu přidat.


```csharp
// We no longer want the individual components to be drawn, so remove them:
foreach (var component in visualComponents)
{
    this.RemoveChild(component);
}
// add the render target sprite to this:
this.AddChild(renderTexture.Sprite);
```

## <a name="summary"></a>Souhrn

Tato příručka zahrnutých `CCRenderTexture` pomocí `Card` entity, který může být použit ve hře collectible karty. Je vám ukázal, jak používat `CCRenderTexture` třída zlepšit obnovovací frekvence a správně implementovat průhlednost celou entity.

I když tato příručka `CCRenderTexture` obsažené v rámci entity, této třídy lze použít k vykreslení více entit nebo i celý `CCLayer` instancí dopady celou obrazovku a vylepšení výkonu.

## <a name="related-links"></a>Související odkazy

- [Referenční dokumentace rozhraní API CCRenderTexture](https://developer.xamarin.com/api/type/CocosSharp.CCRenderTexture/)
- [Úplný projekt (ukázka)](https://developer.xamarin.com/samples/mobile/CCRenderTexture/)
