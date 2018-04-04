---
title: Animace s CCAction
description: Třída CCAction zjednodušuje přidání animace do CocosSharp hry. Tyto animací lze použít k implementaci funkce nebo vylepšení.
ms.prod: xamarin
ms.assetid: 74DBD02A-6F10-4104-A61B-08CB49B733FB
ms.technology: xamarin-cross-platform
author: charlespetzold
ms.author: chape
ms.date: 03/24/2017
ms.openlocfilehash: 21d7cd17d7d08f05e044f648fc09b71b7a837065
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/04/2018
---
# <a name="animating-with-ccaction"></a>Animace s CCAction

_Třída CCAction zjednodušuje přidání animace do CocosSharp hry. Tyto animací lze použít k implementaci funkce nebo vylepšení._

`CCAction` je základní třídu, která slouží k animace CocosSharp objektů. Tento průvodce obsahuje integrovanou `CCAction` implementace pro běžné úkoly jako umístění, škálování a otáčení. Vypadá také o tom, jak vytvořit vlastní implementace, která dědí z `CCAction`.

Tato příručka používá projekt s názvem **ActionProject** který [Zde si můžete stáhnout](https://developer.xamarin.com/samples/mobile/CCAction). Tato příručka používá `CCDrawNode` třídy, která je popsaná v [geometrie kreslení pomocí CCDrawNode](~/graphics-games/cocossharp/ccdrawnode.md) průvodce.


## <a name="running-the-actionproject"></a>Spuštění ActionProject

**ActionProject** je CocosSharp řešení, které se dají vytvářet pro iOS a Android. Funguje i jako ukázka kódu pro použití `CCAction` třídy a jako v reálném čase ukázku běžné `CCAction` implementace.

Při spuštění, ActionProject zobrazuje tři `CCLabel` instance na levé straně obrazovky a vizuální objekt vykreslovat ve dvou `CCDrawNode` instancí zobrazení různé akce:

![](ccaction-images/image1.png "ActionProject zobrazuje tři instance CCLabel na levé straně obrazovky a vizuální objekt vykreslovat dvěma instancemi CCDrawNode k zobrazení různých akcí")

Popisky na levé straně označují, který typ `CCAction` se vytvoří, když klepnete na obrazovce. Ve výchozím nastavení **pozice** vybraná hodnota, což vede k `CCMove` se vytvoří a použijí na červené kolečko akce:

![](ccaction-images/image2.gif "Hodnota pozice zaškrtnuto, což vede k CCMove akce se vytvoří a použijí na červené kolečko")

Kliknutím na popisky na levé straně změny typu `CCAction` provádí na kruhu. Například klepnutím na **pozice** popisek cyklovat mezi různé hodnoty, které mohou být změněny:

![](ccaction-images/image3.gif "Kliknutím na popisek pozice cyklovat mezi různé hodnoty, které mohou být změněny")

## <a name="common-variable-changing-ccaction-classes"></a>Společné třídy CCAction změna proměnné

**ActionProject** používá následující `CCAction`-dědění třídy, které jsou součástí CocosSharp:

 - `CCMoveTo` – Změny `CCNode` instance `Position` vlastnost
 - `CCScaleTo` – Změny `CCNode` instance `Scale` vlastnost
 - `CCRotateTo` – Změny `CCNode` instance `Rotation` vlastnost

Tato příručka označuje tyto akce jako *změna proměnná*, což znamená, že budou mít přímý vliv na proměnnou z `CCNode` , budou přidány do. Jiné typy akcí se označují jako *usnadnění* akce, které jsou popsané dál v této příručce.

**ActionProject** ukazuje, že účelem z těchto akcí je upravit proměnnou v čase. Konkrétně tyto `CCActions` konstruktory trvat dva argumenty: délka dobu trvat a hodnota pro přiřazení. Následující část kódu ukazuje, jak jsou vytvořeny tři typy akcí:


```csharp
switch (VariableOptions [currentVariableIndex])
{
    case "Position":
        coreAction = new CCMoveTo(timeToTake, touch.Location);

        break;
    case "Scale":
        var distance = CCPoint.Distance (touch.Location, drawNodeRoot.Position);
        var desiredScale = distance / DefaultCircleRadius;
        coreAction = new CCScaleTo(timeToTake, desiredScale);

        break;
    case "Rotation":
        float differenceY = touch.Location.Y - drawNodeRoot.PositionY;
        float differenceX = touch.Location.X - drawNodeRoot.PositionX;

        float angleInDegrees = -1 * CCMathHelper.ToDegrees(
            (float)System.Math.Atan2(differenceY, differenceX));

        coreAction = new CCRotateTo (timeToTake, angleInDegrees);

        break; 
...
}
```

Po vytvoření akce ho je přidat do CCNode následujícím způsobem:


```csharp
nodeToAddTo.AddAction (coreAction); 
```

`AddAction` Spustí `CCAction` chování instance a automaticky provede jeho logiku snímek po snímku až do dokončení.

Každý typ uvedené výše zakončení slovem *k* to znamená `CCAction` upraví `CCNode` tak, aby hodnota argumentu představuje poslední stav po dokončení akce. Například vytváření `CCMoveTo` s pozice X = 100 a Y = 200 za následek `CCNode` instance `Position` nastaveno na hodnotu X = 100, Y = 200 na konci čas zadaný, bez ohledu na to `CCNode` instance počáteční umístění.

Každý "Na" Třída také má verzi "Službou", které bude přidat hodnota argumentu aktuální hodnota na `CCNode`. Například vytváření `CCMoveBy` s pozice X = 100 a Y = 200 způsobí `CCNode` instance přesouvání jednotky vpravo 100 až 200 jednotky od pozice bylo na po spuštění akce.


## <a name="easing-actions"></a>Usnadnění akce

Ve výchozím nastavení, změna proměnné akce provede *lineární interpolace* – akce přesune směrem požadovanou hodnotu konstantní rychlostí. Pokud interpolace *pozice* lineárně, přesunutí objektu bude okamžitě spustit a zastavit Přesun na začátku a konci akce a jeho rychlost zůstanou konstantní jako akci provádí. 

Bez lineární interpolace méně jarring a přidá element polština, takže CocosSharp nabízí celou řadu zmírnit akce, které se dají použít k úpravě změna proměnné akce.

V **ActionProject** ukázce jsme můžete přepínat mezi tyto typy akcí nejvýraznější kliknutím na druhý popisek (což výchozí nastavení **<None>**):

![](ccaction-images/image4.gif "Může uživatel přepínat mezi tyto typy akcí nejvýraznější kliknutím na druhý popisek")

Nejvýraznější akce jsou zvláště efektivní, protože nejsou vázané na žádné konkrétní nastavení proměnné akce. To znamená, že stejné nejvýraznější akci lze použít k přiřazení pozice, otáčení, škálování nebo vlastní akce (jak se bude zobrazovat dál v této příručce).

Nejvýraznější akce zabalení nastavení proměnné akce (tak dlouho, dokud akce proměnná nastavení dědí z `CCFiniteTimeAction`) přijetím nastavení proměnné akce jako argument v jejich konstruktory.

Například, pokud mají štítky **pozice**, **CCEaseElastic**, pak následující kód spustí, když se zjistí touch (Všimněte si, že kód byla vynechána označte všechny příslušné řádky):


```csharp
CCFiniteTimeAction coreAction = null; 
...
coreAction = new CCMoveTo(timeToTake, touch.Location); 
...
CCAction easing = null; 
...
easing = new CCEaseSineOut (coreAction); 
...
nodeToAddTo.AddAction (easing); 
```

Jak je znázorněno v aplikaci, přesně stejnou zmírnit lze použít pro další nastavení proměnné akce, jako `CCRotateTo`:

![](ccaction-images/image5.gif "Přesně stejnou zmírnit lze použít na jiných nastavení proměnné akce, jako je například CCRotateTo")


## <a name="easing-in-out-and-inout"></a>Usnadnění In, Out a InOut

Mají všechny nejvýraznější akce `In`, `Out`, nebo `InOut` připojenou k nejvýraznější typu. Tyto termíny označují při použití usnadnění: `In` znamená zmírnit, se použijí na začátku, `Out` znamená na konci, a `InOut` znamená jak na začátek a konec.

`In` Zmírnit akce bude mít vliv na způsob použití proměnné v rámci celého interpolace, (i na začátku a konci), ale obvykle nejvíce rozpoznatelném charakteristiky nejvýraznější akce bude probíhat na začátku. Podobně `Out` nejvýraznější akce jsou charakteristické jejich chování na konci interpolace. Například `CCEaseBounceOut` bude mít za následek objekt skákání na konci akce.


### <a name="out"></a>Out

`Out` usnadnění obecně platí nejvíce patrné změny na konci interpolace. Například `CCEaseExponentialOut` zpomalí změna změna proměnné, jak se blíží hodnota cíle:

![](ccaction-images/image6.gif "CCEaseExponentialOut zpomalí změna změna proměnné, jak se blíží hodnota cíle")


### <a name="in"></a>V

`In` usnadnění obecně použije nejvíce patrné změnu na začátku interpolace. Například `CCEaseExponentialIn` pomaleji přesune na začátku akce:

![](ccaction-images/image7.gif "CCEaseExponentialIn pomaleji přesune na začátku akce")


### <a name="inout"></a>InOut

`InOut` Obecně platí změny nejvíce patrné jak na začátek a konec. `InOut` usnadnění je obvykle symetrický. Například `CCEaseExponentialInOut` pomalu přesune na začátku a konci akce:

![](ccaction-images/image8.gif "CCEaseExponentialInOut pomalu přesune na začátku a konci akce")


## <a name="implementing-a-custom-ccaction"></a>Implementace vlastních CCAction

Všechny třídy, které jsme probrali, pokud jsou součástí CocosSharp poskytuje běžné funkce. Vlastní `CCAction` implementace můžete poskytovat větší flexibilita. Například `CCAction` který určuje vyplněný poměr zobrazí panel prostředí je možné, aby panelu prostředí zvětší bez problémů vždy, když uživatel mírou prostředí.

`CCAction` implementace obvykle vyžadují dvě třídy:

 - `CCFiniteTimeAction` implementace – třída akce konečný čas je zodpovědná za spuštění akce. Je třída, která je vytvořena instance, buď přímo na Přidat `CCNode` nebo nejvýraznější akce. Musíte vytvořit instanci a vrátit `CCFiniteTimeActionState`, který bude provádět aktualizace.
 - `CCFiniteTimeActionState` implementace – třída stavu akce konečný čas je zodpovědný za aktualizace proměnné zahrnutých v akci. Funkce aktualizace, který přiřazuje hodnotu na cíli podle hodnota time musí implementovat. Tato třída není výslovně odkazována mimo `CCFiniteTimeAction` ji vytvoří. Jednoduše funguje "pozadí".

**ActionProject** poskytuje vlastní `CCFiniteTimeAction` implementace volá `LineWidthAction,` sloužící k umožňuje upravit šířku žlutý linií nad červeném kroužku. Všimněte si, že je pouze úlohy vytváření instancí a vrátíte se `LineWidthState` instance:


```csharp
public class LineWidthAction : CCFiniteTimeAction
{
    float endWidth;

    public LineWidthAction (float duration, float width) : base(duration)
    {
        endWidth = width;
    }

    public override CCFiniteTimeAction Reverse ()
    {
        throw new NotImplementedException ();
    }

    protected override CCActionState StartAction (CCNode target)
    {
        return new LineWidthState (this, target, endWidth);
    }
}
```

Jak je uvedeno nahoře, `LineWidthState` funguje přiřazování na řádku `Width` vlastnost podle kolik `time` byla úspěšná:


```csharp
public class LineWidthState : CCFiniteTimeActionState
{
    float deltaWidth;
    float startWidth;

    LineNode castedTarget;

    public LineWidthState(LineWidthAction action, CCNode target, float endWidth) : base(action, target)
    {
        castedTarget = target as LineNode;

        if (castedTarget == null)
        {
            throw new InvalidOperationException ("The argument target must be a LineNode");
        }

        startWidth = castedTarget.Width;
        deltaWidth = endWidth - startWidth;
    }

    public override void Update (float time)
    {
        castedTarget.Width = startWidth + deltaWidth * time;
    }
} 
```

LineWidthAction je možné kombinovat s žádnou nejvýraznější akci, chcete-li změnit šířku čáry různými způsoby, jak je znázorněno v následujícím animace:

![](ccaction-images/image9.gif "LineWidthAction je možné kombinovat s žádnou nejvýraznější akci, chcete-li změnit šířku čáry různými způsoby, jak je znázorněno v této animace")


### <a name="interpolation-and-the-update-method"></a>Interpolace a aktualizační metody

Pouze logiku, kromě zajištění dostatečného ukládání hodnot v třídách výše, je umístěn v `LineWidthState.Update` metoda. `startWidth` Proměnná ukládá šířku cíle `LineNode` při zahájení akce a `deltaWidth` proměnná ukládá, kolik hodnota se změní v průběhu akce.

Nahrazením `time` proměnné s hodnotou 0, můžeme uvidíte, že cíl `LineNode` bude na počáteční pozici:


```csharp
castedTarget.Width = startWidth + deltaWidth * 0; 
```

Podobně jsme uvidíte, že cíl `LineNode` budou na místě určení podle nahraďte proměnnou čas s hodnotou 1:


```csharp
castedTarget.Width = startWidth + deltaWidth * 1; 
```

`time` Hodnota se obvykle být mezi 0 a 1 – ale ne vždy - a `Update` implementace by neměl předpokládají tyto hranice. Některé metody nejvýraznější (například `CCEaseBackIn` a `CCEaseBackOut`) bude poskytovat hodnotu čas mimo rozsah od 0 do 1.


## <a name="conclusion"></a>Závěr

Interpolace a zmírnit jsou důležitou součástí vytvoření dokonalý hry, zejména při vytváření uživatelského rozhraní. Tento průvodce popisuje, jak používat `CCActions` promítnout standardních hodnot, jako je například pozici a oběh i vlastní hodnoty. `LineWidthState` a `LineWidthAction` třídy ukazují, jak implementovat vlastní akce.

## <a name="related-links"></a>Související odkazy

- [CCAction](https://developer.xamarin.com/api/type/CocosSharp.CCAction)
- [CCMoveTo](https://developer.xamarin.com/api/type/CocosSharp.CCMoveTo)
- [CCScaleTo](https://developer.xamarin.com/api/type/CocosSharp.CCScaleTo)
- [CCRotateTo](https://developer.xamarin.com/api/type/CocosSharp.CCRotateTo)
- [CCDrawNode](https://developer.xamarin.com/api/type/CocosSharp.CCDrawNode)
- [Úplnou ukázku najdete na](https://developer.xamarin.com/samples/mobile/CCAction/)
