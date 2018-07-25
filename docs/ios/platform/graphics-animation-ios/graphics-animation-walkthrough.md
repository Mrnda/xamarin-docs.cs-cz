---
title: Použití jádra grafika a animace jader v Xamarin.iOS
description: Tento článek ukazuje krok za krokem, jak vytvořit aplikaci, která používá jádro grafika a animace Core. Ukazuje, jak se má kreslit na obrazovce v reakci na dotykové ovládání pro uživatele, jakož i jak animovat bitovou kopii k dopravě podél cesty.
ms.prod: xamarin
ms.assetid: 4B96D5CD-1BF5-4520-AAA6-2B857C83815C
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/18/2017
ms.openlocfilehash: cecfd7f3a9678f298af3ed547aa7b50a18238729
ms.sourcegitcommit: b56b3f906d2c05a3f1be219ef41be8b79e519b8e
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/25/2018
ms.locfileid: "39242001"
---
# <a name="using-core-graphics-and-core-animation-in-xamarinios"></a>Použití jádra grafika a animace jader v Xamarin.iOS

V tomto návodu budeme nakreslení cesty pomocí základní grafické prvky v reakci na dotykovým ovládáním. Potom přidáme `CALayer` obsahující bitovou kopii, která jsme se animace v cestě.

Na následujícím snímku obrazovky je vidět hotová aplikace:

![](graphics-animation-walkthrough-images/00-final-app.png "Hotová aplikace")

Než začneme stahování *GraphicsDemo* vzorku, který doprovází tento průvodce. Ho můžete stáhnout [tady](https://developer.xamarin.com/samples/monotouch/GraphicsAndAnimation/) a je umístěn uvnitř **GraphicsWalkthrough** directory spustit projekt s názvem **GraphicsDemo_starter** dvojitým kliknutím na něj, a Otevřete `DemoView` třídy.

## <a name="drawing-a-path"></a>Nakreslení cesty


1. V `DemoView` přidat `CGPath` proměnné do třídy a vytvořit její instanci v konstruktoru. Také deklarovat dvě `CGPoint` proměnné, `initialPoint` a `latestPoint`, budeme používat k zachycení bodu dotykového ovládání, ze kterého jsme vytvořit cestu:
    
    ```csharp
    public class DemoView : UIView
    {
        CGPath path;
        CGPoint initialPoint;
        CGPoint latestPoint;
    
        public DemoView ()
        {
            BackgroundColor = UIColor.White;
    
            path = new CGPath ();
        }
    }
    ```

2. Přidejte následující direktivy using:

    ```csharp
    using CoreGraphics;
    using CoreAnimation;
    using Foundation;
    ```

3. V dalším kroku přepsat `TouchesBegan` a `TouchesMoved,` a přidejte následující implementace zaznamenat dotykový počáteční bod a bod každé následné dotykové ovládání v uvedeném pořadí:

    ```csharp
    public override void TouchesBegan (NSSet touches, UIEvent evt){
    
        base.TouchesBegan (touches, evt);
    
        UITouch touch = touches.AnyObject as UITouch;
        
        if (touch != null) {
            initialPoint = touch.LocationInView (this);
        }
    }
    
    public override void TouchesMoved (NSSet touches, UIEvent evt){
    
        base.TouchesMoved (touches, evt);
    
        UITouch touch = touches.AnyObject as UITouch;
        
        if (touch != null) {
            latestPoint = touch.LocationInView (this);
            SetNeedsDisplay ();
        }
    }
    ```

    `SetNeedsDisplay` bude volána pokaždé, když v dnešní přesunout v pořadí pro `Draw` pro volání při dalším spuštění smyčky pass.

4. Přidáme na cestu v řádcích `Draw` metody a použít červenou, přerušované čáry pro kreslení pomocí. [Implementace `Draw` ](~/ios/platform/graphics-animation-ios/core-graphics.md) kódem je uvedeno níže:

    ```csharp
    public override void Draw (CGRect rect){
    
        base.Draw (rect);
    
        if (!initialPoint.IsEmpty) {
    
            //get graphics context
            using(CGContext g = UIGraphics.GetCurrentContext ()){
                    
                //set up drawing attributes
                g.SetLineWidth (2);
                UIColor.Red.SetStroke ();
    
                //add lines to the touch points
                if (path.IsEmpty) {
                    path.AddLines (new CGPoint[]{initialPoint, latestPoint});
                } else {
                    path.AddLineToPoint (latestPoint);
                }
            
                //use a dashed line
                g.SetLineDash (0, new nfloat[] { 5, 2 * (nfloat)Math.PI });
                                
                //add geometry to graphics context and draw it
                g.AddPath (path);       
                g.DrawPath (CGPathDrawingMode.Stroke);
            }
        }
    }
    ```

Pokud provozujeme aplikace nyní, jsme můžete dotykového ovládání pro kreslení na obrazovce, jak je znázorněno na následujícím snímku obrazovky:

![](graphics-animation-walkthrough-images/01-path.png "Kresby na obrazovce")

## <a name="animating-along-a-path"></a>Animace podél cesty

Teď, když jsme implementovali kód, který umožňuje uživatelům nakreslit cestu, přidáme kód pro animaci vrstvu vykresleného cestě.

1. Nejprve přidejte [ `CALayer` ](~/ios/platform/graphics-animation-ios/core-animation.md) proměnné do třídy a vytvořit v konstruktoru:

    ```csharp
    public class DemoView : UIView
        {
            …
    
            CALayer layer;
    
            public DemoView (){
                …
    
                //create layer
                layer = new CALayer ();
                layer.Bounds = new CGRect (0, 0, 50, 50);
                layer.Position = new CGPoint (50, 50);
                layer.Contents = UIImage.FromFile ("monkey.png").CGImage;
                layer.ContentsGravity = CALayer.GravityResizeAspect;
                layer.BorderWidth = 1.5f;
                layer.CornerRadius = 5;
                layer.BorderColor = UIColor.Blue.CGColor;
                layer.BackgroundColor = UIColor.Purple.CGColor;
            }
    ```

2. V dalším kroku přidáme vrstvy jako podvrstvu zobrazení vrstvy když uživatel zruší se jejich prstem na obrazovce. Pak vytvoříme pomocí cesty, animace klíčových snímků animace vrstvy `Position`.

    K tomu potřebujeme k přepsání `TouchesEnded` a přidejte následující kód:

    ```csharp
    public override void TouchesEnded (NSSet touches, UIEvent evt)
        {
            base.TouchesEnded (touches, evt);

            //add layer with image and animate along path

            if (layer.SuperLayer == null)
                Layer.AddSublayer (layer);

            // create a keyframe animation for the position using the path
            layer.Position = latestPoint;
            CAKeyFrameAnimation animPosition = (CAKeyFrameAnimation)CAKeyFrameAnimation.FromKeyPath ("position");
            animPosition.Path = path;
            animPosition.Duration = 3;
            layer.AddAnimation (animPosition, "position");
        }
    ```

3. Spusťte aplikaci nyní a po kreslení, vrstva s obrázkem přidá a přejde na vykresleného cestě:

![](graphics-animation-walkthrough-images/00-final-app.png "Přidá vrstva s obrázkem a přejde na vykresleného cestě")

## <a name="summary"></a>Souhrn

V tomto článku jsme prošli příklad, který grafika a animace koncepty spojených dohromady. Nejprve jsme vám ukázal, jak používat grafiku jádra k nakreslení cesty `UIView` v reakci na dotykové ovládání pro uživatele. Potom jsme vám ukázali jak používat animaci Core Nedělejte obrázky cestovní této cestě.


## <a name="related-links"></a>Související odkazy

- [Core Animation](~/ios/platform/graphics-animation-ios/core-animation.md)
- [Core Graphics](~/ios/platform/graphics-animation-ios/core-graphics.md)
- [Základní animace recepty](https://github.com/xamarin/recipes/tree/master/Recipes/ios/animation/coreanimation)
