---
title: Zpracování více řešení v CocosSharp
description: Tato příručka ukazuje, jak pracovat s CocosSharp pro vývoj her, které zobrazí správně v zařízeních různých řešení.
ms.prod: xamarin
ms.assetid: 859ABF98-2646-431A-A4A8-3E7E48DA5A43
author: charlespetzold
ms.author: chape
ms.date: 03/28/2017
ms.openlocfilehash: 577a3edbd106b6fba298b3ee5999265ef955f9dd
ms.sourcegitcommit: 0a72c7dea020b965378b6314f558bf5360dbd066
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 05/09/2018
ms.locfileid: "33920816"
---
# <a name="handling-multiple-resolutions-in-cocossharp"></a>Zpracování více řešení v CocosSharp

_Tato příručka ukazuje, jak pracovat s CocosSharp pro vývoj her, které zobrazí správně v zařízeních různých řešení._

CocosSharp poskytuje metody pro standardizace rozměry objektu ve hře bez ohledu na fyzické počet pixelů na displeji zařízení. Tradičně může hry vyvinuté pro počítače a herní konzole cílové jednoho řešení. Moderní – a hlavně her pro mobilní zařízení – musí být vytvořeny tak, aby dokázala pojmout širokou škálu zařízení. Tato příručka ukazuje, jak standardizovat CocosSharp hry bez ohledu na řešení charakteristika fyzické zobrazení.

Výchozí chování řešení CocosSharp je tak, aby odpovídala fyzické pixelů s ve hře souřadnice. Následující tabulka ukazuje, jak různá zařízení by vykreslení pozadí pohyblivý symbol prostředí s šířka a výška 368 x 240. První řádek je technicky není skutečné zařízení, ale spíš očekávané vykreslování pohyblivý symbol, bez ohledu na zařízení řešení:


| **Zařízení** | **Rozlišení obrazovky** | **Příklad – snímek obrazovky** |
|--- | --- |--- |
|Požadované zobrazení|368 x 240 (s černým řádky poměru stran)| ![368 x 240 (s černým řádky poměru stran)](resolutions-images/image1.png) |
|iPhone 4s|960x640| ![iPhone 4s 960x640](resolutions-images/image2.png) |
|iPhone 6 Plus|1920x1080| ![iPhone 6 Plus 1920 × 1080](resolutions-images/image3.png) |

Tento dokument popisuje, jak používat CocosSharp k opravě problému uvedené v předchozí tabulce. To znamená, že jsme zaměříme jak provádět jakékoli zařízení vykreslení, jak je znázorněno v prvním řádku – bez ohledu na rozlišení obrazovky.


## <a name="working-with-setdesignresolutionsize"></a>Práce s SetDesignResolutionSize

`CCScene` Třída obvykle slouží jako Kořenový kontejner pro všechny objekty visual, ale také poskytuje statickou metodu s názvem `SetDesignResolutionSize` pro zadání výchozí velikost pro všechny scény. Jinými slovy `SetDesignResolutionSize` metoda umožňuje vývojářům vývoj her, které se zobrazí správně na celou řadu řešení hardwaru. Šablony projektů CocosSharp použití této metody nastavit výchozí velikost projektu na 1024 × 768, jak je znázorněno v následujícím kódu:


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
    
    // This will set the world bounds to (0,0, w, h)
    // CCSceneResolutionPolicy.ShowAll will ensure that the aspect ratio is preserved
    CCScene.SetDesignResolutionSize (desiredWidth, desiredHeight, CCSceneResolutionPolicy.ShowAll);
    ...
```

Můžete změnit `desiredWidth` a `desiredHeight` proměnné výše a upravte zobrazení tak, aby odpovídala požadované rozlišení vaše hra. Například ve výše uvedeném kódu je možné upravovat takto zobrazuje na pozadí 368 x 240 jako celé obrazovky:


```csharp
public override void ApplicationDidFinishLaunching (CCApplication application, CCWindow mainWindow)
{
    application.PreferMultiSampling = false;
    application.ContentRootDirectory = "Content";
    application.ContentSearchPaths.Add ("animations");
    application.ContentSearchPaths.Add ("fonts");
    application.ContentSearchPaths.Add ("sounds");
    CCSize windowSize = mainWindow.WindowSizeInPixels;
    float desiredWidth = 368.0f;
    float desiredHeight = 240.0f;
    
    // This will set the world bounds to(0,0, w, h)
    // CCSceneResolutionPolicy.ShowAll will ensure that the aspect ratio is preserved
    CCScene.SetDesignResolutionSize (desiredWidth, desiredHeight, CCSceneResolutionPolicy.ShowAll);
    ...
```


## <a name="ccsceneresolutionpolicy"></a>CCSceneResolutionPolicy

`SetDesignResolutionSize` Umožňuje zadat, jak okně hry přizpůsobí požadované rozlišení. Následující části ukazují zobrazení bitovou kopii 500 x 500 jiné `CCSceneResolutonPolicy` hodnoty předaný `SetDesignResolutionSize` metoda. Následující hodnoty jsou poskytovány `CCSceneResolutionPolicy` výčtu:

 - `ShowAll` – Zobrazí celý požadovaný řešení zachová poměr stran.
 - `ExactFit` – Zobrazí celý požadovaný řešení, které nespravuje poměr stran.
 - `FixedWidth` – Zobrazí celý požadovanou šířku, zachová poměr stran.
 - `FixedHeight` – Zobrazí celý požadovanou výšku zachová poměr stran.
 - `NoBorder` – Zobrazí na celou výšku nebo celou šířku, zachová poměr stran. Pokud poměr stran zařízení je vyšší než poměr stran požadované rozlišení, zobrazí se celou šířku. Pokud poměr stran zařízení je menší než poměr stran požadované rozlišení, zobrazí se celý výšku.
 - `Custom` – Zobrazení závisí na `Viewport` vlastnost aktuálního `CCScene`.

Všechny snímky obrazovky vytváří iPhone 4s rozlišením (960 x 640) v orientaci na šířku a použít tuto bitovou kopii:

![](resolutions-images/image4.png "Všechny snímky obrazovky vytváří v řešení iPhone 4s 960 x 640 v orientaci na šířku a použít tuto bitovou kopii")


### <a name="ccsceneresolutionpolicyshowall"></a>CCSceneResolutionPolicy.ShowAll

`ShowAll` Určuje, že se nebude zobrazovat na obrazovce celý herní řešení, ale může zobrazovat *letterboxing* (černé pruhů), abyste nastavili pro jiné poměrům stran. Tato zásada se často používá jako zaručuje, že celá herní zobrazení se zobrazí na obrazovce bez narušení.

Příklad kódu:


```csharp
CCScene.SetDesignResolutionSize (500.0f, 500.0f, CCSceneResolutionPolicy.ShowAll);
```

Letterboxing je viditelná vlevo a vpravo od bitovou kopii k účtu pro fyzické poměr stran se širší než požadované rozlišení:

![](resolutions-images/image5.png "Letterboxing je viditelná vlevo a vpravo od bitovou kopii k účtu pro fyzické poměr stran se širší než požadované rozlišení")


### <a name="ccsceneresolutionpolicyexactfit"></a>CCSceneResolutionPolicy.ExactFit

`ExactFit` Určuje, že celý herní řešení se nebude zobrazovat na obrazovce s žádné letterboxing. Může být narušena oblasti zobrazitelné (nemusí být zachována poměr stran) podle poměr stran hardwaru.

Příklad kódu:


```csharp
CCScene.SetDesignResolutionSize (500.0f, 500.0f, CCSceneResolutionPolicy.ExactFit);
```

Žádné letterboxing je viditelná, ale vzhledem k tomu, že je obdélníková řešení zařízení je poškozený, herní zobrazení:

![](resolutions-images/image6.png "Žádné letterboxing je viditelná, ale vzhledem k tomu, že je obdélníková řešení zařízení je poškozený, herní zobrazení")


### <a name="ccsceneresolutionpolicyfixedwidth"></a>CCSceneResolutionPolicy.FixedWidth

`FixedWidth` Určuje, že šířka zobrazení bude odpovídat hodnotě šířka předaný `SetDesignResolutionSize`, ale zobrazitelné výška podléhá poměr stran fyzického zařízení. Hodnota height předaný `SetDesignResolutionSize` se ignoruje, protože se vypočítají za běhu na základě poměru stran fyzického zařízení. To znamená, že počítané výška může být nižší než požadovanou výšku (což vede k části herní zobrazení se mimo obrazovku) nebo počítané výšky může být větší než na požadovanou výšku (což vede k více herní zobrazení se zobrazuje). Vzhledem k tomu, že to může být více hra se zobrazuje, pak může zdát, jako kdyby došlo k letterboxing; ale místo navíc nebudou nutně černé, pokud se zobrazí všechny vizuální objekt existuje. 

Příklad kódu:


```csharp
CCScene.SetDesignResolutionSize (500.0f, 500.0f, CCSceneResolutionPolicy.FixedWidth);
```

Pro iPhone 4s má poměr stran 3:2, takže počítané výšku je přibližně 333 jednotky:

![](resolutions-images/image7.png "Pro iPhone 4s má poměr stran 3:2, takže počítané výšku je přibližně 333 jednotky")


### <a name="ccsceneresolutionpolicyfixedheight"></a>CCSceneResolutionPolicy.FixedHeight

Koncepčně `FixedHeight` se chová podobně jako `FixedWidth` – hra se orientují hodnota height předaný `SetDesignResolutionSize,` , ale bude vypočítat šířku za běhu na základě fyzické rozlišení. Jak je uvedeno výše, to znamená, že zobrazené šířka být vypnuto menší nebo větší než požadovanou šířku, výsledkem je součástí herní se obrazovky nebo více hra se zobrazuje v uvedeném pořadí.

Příklad kódu:


```csharp
CCScene.SetDesignResolutionSize (500.0f, 500.0f, CCSceneResolutionPolicy.FixedHeight);
```

Vzhledem k tomu na následujícím snímku obrazovky byla vytvořená aplikace spuštěna v režimu na šířku, je větší než 500 – konkrétně 750 počítané šířku. Tato zásada uchovává hodnotu 0 X vlevo zarovnaný, tak další řešení jsou viditelná na pravé straně obrazovky.

![](resolutions-images/image8.png "Tato zásada uchovává hodnotu 0 X vlevo zarovnaný, tak další řešení jsou viditelná na pravé straně obrazovky")


### <a name="ccsceneresolutionpolicynoborder"></a>CCSceneResolutionPolicy.NoBorder

`NoBorder` pokusy o zobrazení aplikace s žádné letterboxing při zachování původního poměru stran (bez narušení). Pokud požadovaný řešení poměr odpovídá poměr stran fyzické zařízení, žádný výstřižek dojde. Pokud poměrům stran neshodují, pak výstřižek se provedou.

Příklad kódu:


```csharp
CCScene.SetDesignResolutionSize (500.0f, 500.0f, CCSceneResolutionPolicy.FixedHeight);
```

Na následujícím snímku obrazovky zobrazuje horní a dolní části zobrazení oříznut, když se zobrazí všechny 500 pixelů Šířka zobrazení:

![](resolutions-images/image9.png "Horní a dolní části zobrazení oříznut, když se zobrazí všechny 500 pixelů Šířka zobrazení zobrazí tento snímek obrazovky")


### <a name="ccsceneresolutionpolicycustom"></a>CCSceneResolutionPolicy.Custom

`Custom` Umožňuje každý `CCScene` zadat vlastní vlastní zobrazení relativně k rozlišení určené ve `SetDesignResolutionSize`.

Definování scény jejich `Viewport` vlastnost vytvořením `CCViewport`, který je pak vytvořený pomocí `CCRect`. Další informace o `CCViewport` a `CCRect` najdete v článku [dokumentaci k rozhraní API CocosSharp](https://developer.xamarin.com/api/namespace/CocosSharp/). V rámci vytváření `CCViewport` `CCRect` konstruktoru parametry zadejte (v pořadí):

 - x – velikost vodorovně posunout zobrazení, kde každá jednotka představuje jeden šířka celou obrazovku. Například použití hodnotu výsledků .5f ve scény posunuty doprava o polovinu šířku obrazovky.
 - y – velikost svisle posunout zobrazení, kde každá jednotka představuje jeden výška celé obrazovky. Například použití hodnotu výsledků .5f ve scény posunuty o polovinu výšky obrazovky.
 - Šířka – vodorovné část, kterou by měla zabírat scény. Například na hodnotu 1 / 3.0f výsledkem scény vodorovně v jednu třetinu na obrazovce.
 - Výška – svislé část, kterou by měla zabírat scény. Například na hodnotu 1 / 3.0f výsledkem scény svisle v jednu třetinu na obrazovce.

Příklad kódu:


```csharp
CCScene.SetDesignResolutionSize (500.0f, 500.0f, CCSceneResolutionPolicy.Custom);
...
float horizontalDisplayPortion = 2 / 3.0f;
float verticalDisplayPortion = 1 / 2.0f;
float displayOffsetInScreenWidths = 0;
float displayOffsetInScreenHeights = .5f;
var rectangle = new CCRect (
    displayOffsetInScreenWidths, 
    displayOffsetInScreenHeights, 
    horizontalDisplayPortion, 
    verticalDisplayPortion);
scene.Viewport = new CCViewport (rectangle);
```

Kód výše má za následek následující:

![](resolutions-images/image10.png "Výše uvedený kód výsledků na tomto snímku obrazovky")


## <a name="defaulttexeltocontentsizeratio"></a>DefaultTexelToContentSizeRatio

`DefaultTexelToContentSizeRatio` Zjednodušuje použití textury vyšší řešení v zařízeních s vyšší rozlišení obrazovky. Konkrétně tato vlastnost umožňuje hry použití vyšší rozlišení prostředky bez nutnosti ke změně velikosti nebo umístění vizuální prvky. 

Hry může obsahovat třeba prostředek obrázky pro herní znak, který je 200 pixelů vysoký a herní obrazovky (jak je stanoveno `SetDesignResolutionSize`) může být 400 pixelů vysoký. V takovém případě znak, který bude zabírat poloviny obrazovky. Ale pokud 400 asset pixelů na výšku se používaly k znak pro zařízení s vyšší rozlišení, znak, který by zabírají výška celého zobrazení. Pokud je cílem zachovat znak stejnou velikost využívat výhod vyšší zařízení řešení, některé další kód je potřeba zachovat pohyblivý symbol znaku v polovině výšky obrazovky.

Jednoduchý příklad popsané výše představuje běžné potíže s vývoj her – přidání větší velikost prostředky bez nutnosti vývojáře k provedení změny velikosti pro každý element visual podle řešení zařízení. `DefaultTexelToContentSizeRatio` slouží pro změnu velikosti vizuální prvky globální vlastnost. Tato hodnota změní všechny vizuální prvky určitého typu přiřazenou hodnotou.

Tato vlastnost je součástí následující třídy: 

 - `CCApplication`
 - `CCSprite`
 - `CCLabelTtf`
 - `CCLabelBMFont`
 - `CCTMXLayer`

CocosSharp sady Visual Studio pro Mac šablony sadu `CCSprite.DefaultTexelToContentSizeRatio` podle šířky zařízení jako příklad, jak lze použít. Následující kód je součástí `CCApplicationDelegate.ApplicationDidFinishLaunching`:


```csharp
public override void ApplicationDidFinishLaunching (CCApplication application, CCWindow mainWindow)
{
    ...
    float desiredWidth = 1024.0f;
    float desiredHeight = 768.0f;
           
    ...
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
    ...           
}
```


### <a name="defaulttexeltocontentsizeratio-example"></a>Příklad DefaultTexelToContentSizeRatio

Chcete-li zobrazit jak `DefaultTexelToContentSizeRatio` má vliv na velikost visual prvky, zvažte kód uvedený výše:


```csharp
CCScene.SetDesignResolutionSize (500.0f, 500.0f, CCSceneResolutionPolicy.ShowAll);
```

Následující obrázek použití textury 500 x 500 důsledkem:

![](resolutions-images/image5.png "Důsledkem tuto bitovou kopii použití textury 500 x 500")

IPad sítnice běží na fyzických rozlišení 2048 × 1536. To znamená, že hra, jak je zobrazen výše by stretch 500 pixelů přes 1536 fyzické pixelů. Hry můžete využít výhod toto navíc řešení tak, že vytvoříte, co se obvykle označují jako *hd* prostředky – prostředky, které jsou určená ke spuštění na vyšší rozlišení obrazovky. Tato hra může například použít verzi hd tento texture velikost na 1 000 × 1 000. Ale použití větší image by způsobilo `CCSprite` šířka a výška 1 000 jednotek. Vzhledem k tomu, že naše herní vždy zobrazí 500 jednotek (z důvodu `SetDesignResolutioSize` volání), pak naše pohyblivý symbol 1 000 × 1 000 by byl příliš velký (pouze spodní levé části se zobrazí):

![](resolutions-images/image11.png "Vzhledem k tomu, že hra se vždy zobrazí 500 jednotek z důvodu volání SetDesignResolutioSize, pohyblivý symbol 1 000 × 1 000 by příliš velký, že zobrazí jenom spodní levé části ho")

Můžete nastavit jsme `CCSprite.DefaultTexelToContentSizeRatio` aby se zohlednily texture 1 000 × 1 000 dvakrát se jako vodorovně a svisle jako původní texture 500 x 500:


```csharp
CCSprite.DefaultTexelToContentSizeRatio = 2;
```

Teď Pokud jsme spustit hru texture 1 000 × 1 000 budou plně viditelné:

![](resolutions-images/image12.png "Teď Pokud jsme spustit hru texture 1 000 × 1 000 budou plně viditelné")


### <a name="defaulttexeltocontentsizeratio-details"></a>Podrobnosti o DefaultTexelToContentSizeRatio

`DefaultTexelToContentSizeRatio` Vlastnost je `static,` což znamená, že všechny Sprite v aplikaci budou sdílet stejnou hodnotu. Typické přístup pro hry s prostředky, které jsou vytvořené pro různá řešení bude obsahovat úplnou sadu prostředků pro jednotlivé kategorie řešení. Ve výchozím nastavení CocosSharp sady Visual Studio pro Mac šablony zadejte **ld** a **hd** složek pro prostředky, které by byly užitečné pro hry podpora dvě sady textury. Ukázkové obsahu složky s obsahem může vypadat podobně jako:

![](resolutions-images/image13.png "Ukázkové obsahu složky s obsahem bude vypadat podobně jako")

Všimněte si, že výchozí šablony také obsahuje kód podmíněně přidat do aplikace `ContentSearchPaths`:


```csharp
public override void ApplicationDidFinishLaunching (CCApplication application, CCWindow mainWindow)
{
    ...
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
    ...           
}
```

To znamená, že aplikace bude hledat soubory v cestách podle tom, zda je spuštěna v vysokým rozlišením nebo režim regulární řešení. Například pokud **hd** a **ld** složky obsahují soubor s názvem **background.png** potom následující kód by spustit a vyberte správný soubor pro řešení:


```csharp
backgroundSprite  = new CCSprite ("background");
```


## <a name="summary"></a>Souhrn

Tento článek popisuje postup vytvoření hry, které budou zobrazovat správně bez ohledu na zařízení řešení. Zobrazuje příklady použití různých `CCSceneResolutionPolicy` hodnoty pro změnu velikosti herní podle řešení zařízení. Také poskytuje příklad `DefaultTexelToContentSizeRatio` slouží k přizpůsobení několik sad obsahu bez nutnosti vizuální prvky na změnu velikosti jednotlivě.

## <a name="related-links"></a>Související odkazy

- [CocosSharp Úvod](~/graphics-games/cocossharp/index.md)
- [Dokumentace CocosSharp rozhraní API](https://developer.xamarin.com/api/namespace/CocosSharp/)
