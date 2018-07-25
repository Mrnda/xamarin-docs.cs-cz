---
title: Základní animace v Xamarin.iosu
description: Tento článek zkoumá rozhraní framework Core animace znázorňující, jak umožňuje vysoce výkonná a plynulé animace v Uikitu, jak použít přímo pro ovládací prvek animace nižší úrovně.
ms.prod: xamarin
ms.assetid: D4744147-FACB-415B-8155-3A6B3C35E527
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/18/2017
ms.openlocfilehash: 3d26e58822385c20f3c08d0b75ba468467c2c9b1
ms.sourcegitcommit: b56b3f906d2c05a3f1be219ef41be8b79e519b8e
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/25/2018
ms.locfileid: "39242128"
---
# <a name="core-animation-in-xamarinios"></a>Základní animace v Xamarin.iosu

_Tento článek zkoumá rozhraní framework Core animace znázorňující, jak umožňuje vysoce výkonná a plynulé animace v Uikitu, jak použít přímo pro ovládací prvek animace nižší úrovně._

zahrnuje iOS [ *základní animace* ](https://developer.apple.com/library/ios/documentation/Cocoa/Conceptual/CoreAnimation_guide/Introduction/Introduction.html) kvůli zajištění podpory animace pro zobrazení v aplikaci.
Všechny nabízenému plynulé animace v Iosu, jako je například posouvání tabulek prstů a potažení prstem mezi různá zobrazení tak dobře fungovat stejně, protože závisí na základní animace interně.

Základní animace a základní grafické rozhraní, můžou spolupracovat a vytvářet působivé, animovat 2D grafika. Ve skutečnosti animace Core můžete dokonce transformovat 2D grafika v 3D prostoru vytváření úžasných, formou prostředí. Však vytvořit true 3D grafiky, musíte pomocí něčeho jako OpenGL ES, nebo o zapnutí hry do rozhraní API, jako je například MonoGame, přestože 3D je nad rámec tohoto článku.

<a name="Using_Core_Animation" />

## <a name="core-animation"></a>Základní animace

k vytvoření efekty animace, jako je například převod mezi zobrazeními, klouzavé nabídky a posouvání účinky pár iOS používá animace Core framework. Existují dva způsoby, jak pracovat s animace:

- [Prostřednictvím UIKit](#Using_UIKit_Animation), což zahrnuje animacemi založenými na zobrazení, jakož i animované přechody mezi řadiči.
- [Prostřednictvím základní animace](#Using_Core_Animation), které vrstvy přímo, umožňující citlivější ovládacího prvku.

<a name="Using_UIKit_Animation" />

## <a name="using-uikit-animation"></a>Pomocí UIKit animace

UIKit poskytuje několik funkcí, které usnadňují přidání animace k aplikaci. I když se interně používá základní animace, ji odděluje ho mohli pracovat pouze s zobrazení a kontrolery.

Tato část popisuje UIKit animace funkce včetně:

-  Přechody mezi řadiči
-  Přechody mezi zobrazeními
-  Animace vlastností zobrazení


### <a name="view-controller-transitions"></a>Přechody Kontrolerů zobrazení

 `UIViewController` poskytuje integrovanou podporu pro přechod mezi kontrolery zobrazení prostřednictvím `PresentViewController` metody. Při použití `PresentViewController`, můžete volitelně animovat přechod na druhý kontroler.

Zvažte například aplikaci dva řadiče, kde stisknutím tlačítka v prvním řadiči volá `PresentViewController` zobrazíte druhý kontroler. Pokud chcete řídit, jaké přechod animace se používá k zobrazení druhého řadiče, stačí nastavit jeho [ `ModalTransitionStyle` ](https://developer.xamarin.com/api/type/UIKit.UIModalTransitionStyle/) vlastnost, jak je znázorněno níže:

```csharp
SecondViewController vc2 = new SecondViewController {
    ModalTransitionStyle = UIModalTransitionStyle.PartialCurl
};
```

V tomto případě `PartialCurl` animace se používá, i když některé další jsou k dispozici, včetně:

-  `CoverVertical` – Snímky nahoru v dolní části obrazovky
-  `CrossDissolve` – Stará zobrazení setmívá & nové zobrazení rozetmí.
-  `FlipHorizontal` -Převrátit na ose vodorovnou zprava doleva. Přechod na propuštění převrátí zleva doprava.


Má animovat přechod, předejte `true` jako druhý argument `PresentViewController`:

```csharp
PresentViewController (vc2, true, null);
```

Následující snímek obrazovky ukazuje, jak přechod vypadá `PartialCurl` případ:

 ![](core-animation-images/06-view-transitions.png "Tento snímek obrazovky ukazuje PartialCurl přechodu")

### <a name="view-transitions"></a>Zobrazit přechody

Kromě přechody mezi řadiči UIKit také podporuje animování přechody mezi zobrazeními pro jedno zobrazení dalších.

Například Řekněme, že jste měli kontroler `UIImageView`, kde by měl klepnete na ikonu zobrazení sekundy `UIImageView`. Animování image je stejně jednoduché jako volání funkce zobrazení superview přejít do zobrazení druhého image `UIView.Transition`, předají se jí `toView` a `fromView` jak je znázorněno níže:

```csharp
UIView.Transition (
    fromView: view1,
    toView: view2,
    duration: 2,
    options: UIViewAnimationOptions.TransitionFlipFromTop |
        UIViewAnimationOptions.CurveEaseInOut,
    completion: () => { Console.WriteLine ("transition complete"); });
```

`UIView.Transition` také využívá `duration` parametr, který určuje, jak dlouho běží animace, stejně jako [ `options` ](https://developer.xamarin.com/api/type/UIKit.UIViewAnimationOptions/) k určení věcí například animace a funkci uvolnění použít. Kromě toho můžete určit, která bude volána po dokončení animace obslužné rutiny dokončení.

Na snímku obrazovky níže ukazují animovaný přechod mezi image zobrazení, kdy `TransitionFlipFromTop` se používá:

 ![](core-animation-images/07-animated-transition.png "Tento snímek obrazovky ukazuje animovaný přechod mezi zobrazeními bitové kopie zadáním TransitionFlipFromTop")

### <a name="view-property-animations"></a>Animace vlastností zobrazení

UIKit podporuje širokou škálu vlastnosti animace v `UIView` třídy zdarma, včetně:

-  Rámec
-  Hranice
-  Střed
-  systém Alpha
-  Transformace
-  Barva


Tyto animace budete stát implicitně zadáním změny vlastností v `NSAction` delegát předaný do statické `UIView.Animate` metody. Například následující kód animuje středu `UIImageView`:

```csharp
pt = imgView.Center;

UIView.Animate (
    duration: 2, 
    delay: 0, 
    options: UIViewAnimationOptions.CurveEaseInOut | 
        UIViewAnimationOptions.Autoreverse,
    animation: () => {
        imgView.Center = new CGPoint (View.Bounds.GetMaxX () 
            - imgView.Frame.Width / 2, pt.Y);},
    completion: () => {
        imgView.Center = pt; }
);
```

Výsledkem obrázku animace vpřed a zpět v horní části obrazovky, jak je znázorněno níže:

 ![](core-animation-images/08-animate-center.png "Obrázku animace vpřed a zpět v horní části obrazovky jako výstup")

Stejně jako u `Transition` metody `Animate` umožňuje nastavit spolu s funkci přechodu pro dobu trvání. Tento příklad používá také `UIViewAnimationOptions.Autoreverse` možnost, která způsobí, že animace animace od hodnoty zpět do původní objektu. Však také nastaví kód `Center` zpět na počáteční hodnotu v obslužné rutině dokončení. Při animaci je interpolace hodnoty vlastností v čase, skutečnou hodnotu modelu vlastnosti je vždy konečnou hodnotu, která byla nastavena. V tomto příkladu je hodnota bod u pravé strany superview. Bez nastavení `Center` počáteční bod, což je, kde kvůli dokončení animace `Autoreverse` nastavena, image by Přichytit zpět na pravou stranu po dokončení animace, jak je znázorněno níže:

 ![](core-animation-images/09-animation-complete.png "Bez nastavení centru na počáteční bod, na obrázku by Přichytit zpět na pravou stranu po dokončení animace")

## <a name="using-core-animation"></a>Pomocí základní animace

 `UIView` animace povolit spoustu schopností a by měl být pokud je to možné použít kvůli snadné implementaci. Jak už bylo zmíněno dříve, pomocí animací UIView framework Core animace. Ale nejde udělat pár věcí u `UIView` animace, jako je například animace další vlastnosti, které nelze animovat se zobrazením nebo interpolace nelineárních cestě. V takových případech, kdy potřebujete lepší kontrolu Core animace lze také přímo.

### <a name="layers"></a>Vrstvy

Při práci s animací základní animace se odehrává prostřednictvím *vrstvy*, které jsou typu `CALayer`. Vrstva se koncepčně podobá zobrazení v tom, že je vrstvy hierarchie, podobně jako zobrazení hierarchie. Ve skutečnosti vrstvy zpět zobrazení, když je zobrazení přidání podpory pro interakci s uživatelem. Přistupujete k vrstvě všechna zobrazení v zobrazení `Layer` vlastnost. Ve skutečnosti kontextu použít v `Draw` metoda `UIView` skutečně vytvořena z vrstvy. Interně jsou vrstvy zálohování `UIView` má jeho delegáta nastavená na view samostatně, což je agentura `Draw`. Ano, při kreslení k `UIView`, jsou ve skutečnosti kreslení k její vrstvy.

Animace vrstva může být implicitní nebo explicitní. Implicitní animace jsou deklarativní. Jednoduše deklarujete, co by měl změnit vlastnosti vrstvy a animace prostě to funguje. Explicitní animací na druhé straně vytvářejí prostřednictvím třídu animace, která se přidá do vrstvy. Explicitní animace povolit přidání kontrolu nad vytvoření animace. V následujících částech se pustíte do implicitní a explicitní animace do větší hloubky.

### <a name="implicit-animations"></a>Implicitní animace

Jedním ze způsobů animace vlastností vrstvy je prostřednictvím implicitního animace. `UIView` animace vytvoření implicitního animace. Však můžete vytvořit přímo proti vrstvě také implicitní animace.

Například následující kód nastaví vrstvy `Contents` Nastaví šířku ohraničení a barvy z obrázku, a přidá vrstvu jako podvrstvu zobrazení vrstvy:

```csharp
public override void ViewDidLoad ()
{
    base.ViewDidLoad ();

    layer = new CALayer ();
    layer.Bounds = new CGRect (0, 0, 50, 50);
    layer.Position = new CGPoint (50, 50);
    layer.Contents = UIImage.FromFile ("monkey2.png").CGImage;
    layer.ContentsGravity = CALayer.GravityResize;
    layer.BorderWidth = 1.5f;
    layer.BorderColor = UIColor.Green.CGColor;

    View.Layer.AddSublayer (layer);
}
```

Pokud chcete přidat animaci implicitní pro vrstvu, jednoduše zabalit změny vlastností v `CATransaction`. Díky tomu animace vlastností, které by se animovatelné s animace zobrazení, jako například `BorderWidth` a `BorderColor` jak je znázorněno níže:

```csharp
public override void ViewDidAppear (bool animated)
{
    base.ViewDidAppear (animated);

    CATransaction.Begin ();
    CATransaction.AnimationDuration = 10;
    layer.Position = new CGPoint (50, 400);
    layer.BorderWidth = 5.0f;
    layer.BorderColor = UIColor.Red.CGColor;
    CATransaction.Commit ();
}
```

Tento kód také animuje vrstvy `Position`, což je umístění měřená v levém horním rohu superlayer souřadnice bodu ukotvení vrstvy. Bod ukotvení vrstvy je normalizované bod v rámci vrstvy souřadnicový systém.

Následující obrázek znázorňuje pozici a ukotvení bodu:

 ![](core-animation-images/10-postion-anchorpt.png "Tento obrázek ukazuje bodu pozici a ukotvení")

Při spuštění v příkladu `Position`, `BorderWidth` a `BorderColor` animace, jak je znázorněno na následujících snímcích obrazovky:

 ![](core-animation-images/11-implicit-animation.png "Při spuštění v příkladu animace umístění a hodnotou vlastnosti BorderWidth BorderColor jak je znázorněno")

### <a name="explicit-animations"></a>Explicitní animace

Kromě implicitní animace animace Core zahrnuje celou řadu tříd, které dědí z `CAAnimation` , které umožňují zapouzdřit animace, které jsou pak explicitně přidány do vrstvy. Tyto rutiny umožňují řídit citlivější animace, jako jsou úpravy počáteční hodnoty animace, seskupení animace a určení klíčové snímky na povolit nelineárních cesty.

Následující kód ukazuje příklad explicitního animací použitím polí `CAKeyframeAnimation` pro vrstvu výše (v části implicitní animace):

```csharp
public override void ViewDidAppear (bool animated)
{
    base.ViewDidAppear (animated);
    
    // get the initial value to start the animation from
    CGPoint fromPt = layer.Position;
    
    /* set the position to coincide with the final animation value
    to prevent it from snapping back to the starting position
    after the animation completes*/
    layer.Position = new CGPoint (200, 300);
    
    // create a path for the animation to follow
    CGPath path = new CGPath ();
    path.AddLines (new CGPoint[] { fromPt, new CGPoint (50, 300), new CGPoint (200, 50), new CGPoint (200, 300) });
    
    // create a keyframe animation for the position using the path
    CAKeyFrameAnimation animPosition = (CAKeyFrameAnimation)CAKeyFrameAnimation.FromKeyPath ("position");
    animPosition.Path = path;
    animPosition.Duration = 2;
    
    // add the animation to the layer.
    /* the "position" key is used to overwrite the implicit animation created
    when the layer positino is set above*/
    layer.AddAnimation (animPosition, "position");
}
```

Tento kód se změní `Position` vrstvy tak, že vytvoříte cestu, která se pak použije k definování klíčový snímek animace. Všimněte si, že vrstvy `Position` konečná hodnota nastavena `Position` z animace. Bez toho by vrstvu náhle vrátil k jeho `Position` před animace protože animace změní pouze hodnotu prezentace a nikoli skutečnou hodnotu modelu. Nastavením hodnoty modelu na konečnou hodnotu z animace vrstvě zůstat na místě na konci animace.

Na následujících snímcích obrazovky zobrazit vrstvu, která obsahuje image animace prostřednictvím zadaná cesta:

 ![](core-animation-images/12-explicit-animation.png "Tento snímek obrazovky ukazuje vrstvu, která obsahuje image animace pomocí zadané cesty")
 
## <a name="summary"></a>Souhrn

V tomto článku jsme se podívali na animace poskytované prostřednictvím *základní animace* architektur. Jsme se zaměřili na základní animace, zobrazuje, jak využívají animace v Uikitu i jak ho lze použít přímo pro ovládací prvek animace nižší úrovně.

## <a name="related-links"></a>Související odkazy

- [Ukázkové základní animace](https://developer.xamarin.com/samples/monotouch/GraphicsAndAnimation/)
- [Core Graphics](~/ios/platform/graphics-animation-ios/core-graphics.md)
- [Grafika a animace návodu](~/ios/platform/graphics-animation-ios/graphics-animation-walkthrough.md)
- [Core Animation](https://github.com/xamarin/recipes/tree/master/Recipes/ios/animation/coreanimation)
