---
title: Základní animace v Xamarin.iOS
description: Tento článek prozkoumá základní animace framework, znázorňující, jak umožňuje vysoký výkon, plynulá animace v UIKit, dobře, jak používat přímo k ovládacího prvku animace nižší úrovně.
ms.prod: xamarin
ms.assetid: D4744147-FACB-415B-8155-3A6B3C35E527
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/18/2017
ms.openlocfilehash: 5cc6019ed148b870e38659eb30ac7f2738481a50
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 06/05/2018
ms.locfileid: "34786813"
---
# <a name="core-animation-in-xamarinios"></a>Základní animace v Xamarin.iOS

_Tento článek prozkoumá základní animace framework, znázorňující, jak umožňuje vysoký výkon, plynulá animace v UIKit, dobře, jak používat přímo k ovládacího prvku animace nižší úrovně._

iOS zahrnuje [ *základní animace* ](https://developer.apple.com/library/ios/documentation/Cocoa/Conceptual/CoreAnimation_guide/Introduction/Introduction.html) k podpoře animace zobrazení ve vaší aplikaci.
Proveďte všechny vysoce smooth animace v iOS například posouvání tabulek a k načtení mezi různá zobrazení stejně jako udělají, protože spoléhají na základní animace, interně.

Základní animace a základní grafické rozhraní můžou spolupracovat a vytvořit Krásný, animovaný 2D grafiky. Ve skutečnosti základní animace můžete i transformace grafiky 2D v 3D prostoru vytváření úžasné, formou prostředí. Ale pokud chcete vytvořit true 3D grafiky, potřebovali byste pomocí něčeho jako OpenGL ES, nebo o hry zapnout do rozhraní API, jako je například MonoGame, i když 3D je nad rámec tohoto článku.

<a name="Using_Core_Animation" />

## <a name="core-animation"></a>Základní animace

iOS framework Core animace používá k vytvoření efekty animace například přechod mezi zobrazeními, klouzavé nabídky a posouvání efekty a další. Existují dva způsoby, jak pracovat s animace:

- [Prostřednictvím UIKit](#Using_UIKit_Animation), což zahrnuje animace na základě zobrazení, jakož i animované přechody mezi řadiči.
- [Prostřednictvím základní animace](#Using_Core_Animation), které vrstvy přímo, povolení pro řízení citlivější.

<a name="Using_UIKit_Animation" />

## <a name="using-uikit-animation"></a>Pomocí UIKit animace

UIKit poskytuje několik funkcí, které usnadňují přidání animace do aplikace. I když se interně používá základní animace, ho abstrahuje ji rychle, abyste pracovat pouze s zobrazeních a řadičích.

Tato část popisuje UIKit animace funkcí, včetně:

-  Přechody mezi řadiči
-  Přechodů mezi zobrazení
-  Zobrazení vlastností animace


### <a name="view-controller-transitions"></a>Přechody řadiče zobrazení

 `UIViewController` poskytuje integrovanou podporu pro přechod mezi zobrazení řadičů prostřednictvím `PresentViewController` metoda. Při použití `PresentViewController`, volitelně může být animovaný přechodu do druhého řadiče.

Například vezměte v úvahu aplikace s dva řadiče, kde dotykové ovládání tlačítka na první řadič volá `PresentViewController` zobrazíte další zařízení. Chcete-li řídit, jaké přechod animace se používá k zobrazení druhého řadiče, jednoduše nastavte její [ `ModalTransitionStyle` ](https://developer.xamarin.com/api/type/UIKit.UIModalTransitionStyle/) vlastnost, jak je uvedeno níže:

```csharp
SecondViewController vc2 = new SecondViewController {
    ModalTransitionStyle = UIModalTransitionStyle.PartialCurl
};
```

V takovém případě `PartialCurl` animace se používá, i když některé další jsou k dispozici, včetně:

-  `CoverVertical` – Snímky nahoru v dolní části obrazovky
-  `CrossDissolve` – Setmívá původní zobrazení & rozetmívá nové zobrazení
-  `FlipHorizontal` -Překlopit k vodorovných zprava doleva. Přechod na propouštění převrátí zleva doprava.


Animace přechodu, předat `true` jako druhý argument `PresentViewController`:

```csharp
PresentViewController (vc2, true, null);
```

Následující snímek obrazovky ukazuje, jak přechodu vypadá `PartialCurl` případ:

 ![](core-animation-images/06-view-transitions.png "Tento snímek obrazovky ukazuje PartialCurl přechodu")

### <a name="view-transitions"></a>Přechody zobrazení

Kromě přechody mezi řadiči UIKit také podporuje animování přechodů mezi zobrazení se Prohodit jedno zobrazení pro jinou.

Například vyslovte měl řadič s `UIImageView`, kde klepnutím na obrázku, by se měl zobrazit druhý `UIImageView`. Pro animaci bitovou kopii je jednoduché, volání superview zobrazení pro přechod do druhého zobrazení image `UIView.Transition`, předání `toView` a `fromView` jak je uvedeno níže:

```csharp
UIView.Transition (
    fromView: view1,
    toView: view2,
    duration: 2,
    options: UIViewAnimationOptions.TransitionFlipFromTop |
        UIViewAnimationOptions.CurveEaseInOut,
    completion: () => { Console.WriteLine ("transition complete"); });
```

`UIView.Transition` také trvá `duration` parametr, který řídí, jak dlouho bude animace spuštěna, a také [ `options` ](https://developer.xamarin.com/api/type/UIKit.UIViewAnimationOptions/) k určení akcí, například animace použití a nejvýraznější funkce. Kromě toho můžete zadat dokončení obslužná rutina, která bude volána po dokončení animace.

Zobrazení animovaný přechod mezi bitovou kopii na snímku obrazovky níže zobrazit, kdy `TransitionFlipFromTop` se používá:

 ![](core-animation-images/07-animated-transition.png "Tento snímek obrazovky ukazuje animovaný přechod mezi image zobrazení, pokud se používá TransitionFlipFromTop")

### <a name="view-property-animations"></a>Zobrazení vlastností animace

UIKit podporuje celou řadu vlastností animace na `UIView` třídy zdarma, včetně:

-  Rámec
-  Hranice
-  Střed
-  Alpha
-  Transformace
-  Barva


Tyto animací dojít implicitně zadáním změny vlastností v `NSAction` delegáta předaný statických `UIView.Animate` metoda. Například následující kód animuje středový bod `UIImageView`:

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

To vede image animace a zpět v horní části obrazovky, jak je uvedeno níže:

 ![](core-animation-images/08-animate-center.png "Obrázku animace a zpět v horní části obrazovky jako výstup")

Stejně jako u `Transition` metody `Animate` umožňuje nastavit, společně s nejvýraznější funkce pro dobu trvání. Tento příklad používá také `UIViewAnimationOptions.Autoreverse` možnost, která způsobí, že animace animace z hodnoty zpět na počáteční jeden. Však také nastaví kód `Center` zpět na počáteční hodnoty v obslužné rutině dokončení. Při animace je interpolace hodnoty vlastností v čase, na skutečnou hodnotu modelu vlastnosti je vždy konečná hodnota, která byla nastavena. V tomto příkladu je hodnota bod po pravé straně superview. Bez nastavení `Center` počáteční bod, který je, kde kvůli dokončení animace `Autoreverse` nastaveno, bitovou kopii by snap zpět na pravé straně po dokončení animace, jak je uvedeno níže:

 ![](core-animation-images/09-animation-complete.png "Bez nastavení centru na počáteční bod, bitovou kopii by snap zpět na pravé straně po dokončení animace")

## <a name="using-core-animation"></a>Použití jádra animace

 `UIView` animace povolit spoustu schopností a by měl použít pokud je to možné kvůli usnadnění implementace. Jak už bylo zmíněno dříve, použijte UIView animací framework Core animace. Ale některé věci nelze provést s `UIView` animací, jako je například animaci další vlastnosti, které nelze animovaný se zobrazením nebo interpolace podél – lineární cesty. V takových případech, kde je nutné jemnějšího ovládání základní animace lze také přímo.

### <a name="layers"></a>Vrstev.

Při práci s animace jádra, animace se odehrává prostřednictvím *vrstvy*, které jsou typu `CALayer`. Vrstva se koncepčně podobá zobrazení v, že je vrstvy hierarchie, podobně jako zobrazení hierarchie. Ve skutečnosti vrstvy zpět zobrazení, se zobrazení přidat podporu pro interakci s uživatelem. Můžete získat přístup k vrstvě všechna zobrazení v zobrazení `Layer` vlastnost. Ve skutečnosti kontext použít v `Draw` metodu `UIView` skutečně vytvořena z vrstvy. Interně vrstvě zálohování `UIView` má jeho delegáta nastavenou na zobrazení sebe, což je co volá `Draw`. Ano, při kreslení na `UIView`, jsou ve skutečnosti kreslení na jeho vrstvu.

Vrstva animace může být implicitním nebo explicitním. Implicitní animace jsou deklarativní. Jednoduše deklarovat, co by se měl změnit vlastnosti vrstvy a animace prostě funguje. Explicitní animací na druhé straně jsou vytvořené prostřednictvím třídu animace, který je přidán do vrstvy. Explicitní animací povolit přidání kontrolu nad vytváření animace. V následujících částech se pustíte do implicitní a explicitní animací podrobněji.

### <a name="implicit-animations"></a>Implicitní animace

Jedním ze způsobů animace vlastnosti vrstvy je prostřednictvím implicitní animace. `UIView` animace vytvořit implicitní animace. Můžete však vytvořit implicitní animací přímo na vrstvu také.

Například následující kód nastaví vrstvy `Contents` z bitové kopie, nastaví barva a šířka ohraničení a přidá vrstvě jako podvrstvu zobrazení vrstvy:

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

Chcete-li přidat implicitní animace pro vrstvy, jednoduše zabalit změny vlastností v `CATransaction`. To umožňuje animace vlastnosti, které nebude animatable s animace zobrazení, jako `BorderWidth` a `BorderColor` jak je uvedeno níže:

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

Tento kód také animuje vrstvy `Position`, což je umístění, měřenou z levé horní části superlayer souřadnice bodu ukotvení vrstvy. Bodu ukotvení vrstvy je bod normalizovaný v rámci vrstvy souřadnicový systém.

Následující obrázek znázorňuje bod pozice a ukotvení:

 ![](core-animation-images/10-postion-anchorpt.png "Následující obrázek ukazuje bod pozice a ukotvení")

Při spuštění v příkladu `Position`, `BorderWidth` a `BorderColor` animace, jak je vidět na následujících snímcích obrazovky:

 ![](core-animation-images/11-implicit-animation.png "Když se spustí v příkladu, pozice, hodnota BorderWidth a Barva okraje animace znázorněné")

### <a name="explicit-animations"></a>Explicitní animace

Kromě implicitní animací základní animace zahrnuje celou řadu třídy, které dědí od `CAAnimation` které umožňují zapouzdření animace, které jsou pak explicitně přidat do vrstvy. Tyto rutiny umožňují řídit citlivější animací, jako je například úprava počáteční hodnotu animace, seskupování animace a určení klíčových snímků umožňující – lineární cesty.

Následující kód ukazuje příklad použití explicitní animace `CAKeyframeAnimation` pro vrstvu uvedené dříve (v části implicitní animace):

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

Tento kód se změní `Position` vrstvy vytvořením cestu, která se pak používá k definování animace klíčových snímků. Všimněte si, že vrstvy `Position` nastavena na konečná hodnota `Position` z animace. Bez toho by vrstva náhle vrátit k jeho `Position` před animace vzhledem k tomu, že animace pouze změní hodnotu prezentace a ne na skutečnou hodnotu modelu. Nastavením hodnoty modelu na konečná hodnota z animace umístění na konci animace zůstane vrstvě.

Na následujících snímcích obrazovky zobrazit vrstvu obsahující bitovou kopii animace pomocí zadané cesty:

 ![](core-animation-images/12-explicit-animation.png "Tento snímek obrazovky ukazuje vrstvu, která obsahuje bitovou kopii animace pomocí zadané cesty")
 
## <a name="summary"></a>Souhrn

V tomto článku jsme se podívali na animace možnosti poskytované prostřednictvím *základní animace* architektury. Jsme se zaměřili na základní animace, zobrazující jak ho pohání animace v UIKit i jeho použití přímo pro ovládací prvek animace nižší úrovně.

## <a name="related-links"></a>Související odkazy

- [Ukázka základní animace](https://developer.xamarin.com/samples/monotouch/GraphicsAndAnimation/)
- [Core Graphics](~/ios/platform/graphics-animation-ios/core-graphics.md)
- [Grafika a návod animace](~/ios/platform/graphics-animation-ios/graphics-animation-walkthrough.md)
- [Core Animation](https://developer.xamarin.com/recipes/ios/animation/coreanimation)
