---
title: Použití jádra grafika a základní animace v Xamarin.iOS
description: Tento článek ukazuje krok za krokem, jak vytvořit aplikaci, která používá základní grafika a základní animace. Zobrazuje postup kreslení na obrazovce v reakci na touch uživatele a také jak animace obrázek cestují podél cesty.
ms.prod: xamarin
ms.assetid: 4B96D5CD-1BF5-4520-AAA6-2B857C83815C
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/18/2017
ms.openlocfilehash: 7a4399a5d62e2000c2a15a65da8e0e427dc039e0
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 06/05/2018
ms.locfileid: "34787052"
---
# <a name="using-core-graphics-and-core-animation-in-xamarinios"></a>Použití jádra grafika a základní animace v Xamarin.iOS

V tomto návodu budeme kreslení pomocí základní grafické prvky v reakci na touch vstup cestu. Potom přidáme `CALayer` obsahující obrázek, který jsme se animace v cestě.

Na následujícím snímku obrazovky je vidět hotová aplikace:

![](graphics-animation-walkthrough-images/00-final-app.png "Hotová aplikace")

Než začneme stahování *GraphicsDemo* vzorku, který doprovází této příručce. Ho můžete stáhnout [sem](https://developer.xamarin.com/samples/monotouch/GraphicsAndAnimation/) a je umístěn uvnitř **GraphicsWalkthrough** directory spuštění projektu s názvem **GraphicsDemo_starter** dvojitým kliknutím na soubor, a Otevřete `DemoView` třídy.

## <a name="drawing-a-path"></a>Kreslení cesty


1. V `DemoView` přidat `CGPath` proměnnou třídy a vytvoří instanci v konstruktoru. Také deklarovat dvě `CGPoint` proměnné, `initialPoint` a `latestPoint`, že budeme používat k zachycení bodu dotykového ovládání, ze které můžeme vytvořit cestu:
    
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

3. V dalším kroku přepsat `TouchesBegan` a `TouchesMoved,` a přidejte následující implementace k zachycení bodu počáteční touch a každý bod následné touch v uvedeném pořadí:

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

    `SetNeedsDisplay` bude volána pokaždé, když úpravy přesunout v pořadí pro `Draw` volá se při dalším spuštění smyčky průchodu.

4. Přidáme řádky na cestu v `Draw` metoda a použití red, přerušované čáry k vykreslení s. [Implementace `Draw` ](~/ios/platform/graphics-animation-ios/core-graphics.md) kódem vidíte níže:

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

Pokud jsme spustit nyní aplikaci, jsme můžete touch kreslení na obrazovce, jak je znázorněno na následujícím snímku obrazovky:

![](graphics-animation-walkthrough-images/01-path.png "Kreslení na obrazovce")

## <a name="animating-along-a-path"></a>Animace podél cesty

Implementovali jsme kód, aby uživatelé mohli kreslení cestu, Pojďme přidat kód pro animaci vrstvu v vykresleného cestě.

1. Nejprve přidejte [ `CALayer` ](~/ios/platform/graphics-animation-ios/core-animation.md) proměnnou třídy a vytvořte ji v konstruktoru:

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

2. Potom přidáme vrstvě jako podvrstvu zobrazení vrstvy Pokud uživatel zruší si jejich prstem z obrazovky. Poté vytvoříme pomocí cesty, animace klíčových snímků animace vrstvy `Position`.

    K tomu je potřeba přepsat `TouchesEnded` a přidejte následující kód:

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

3. Spusťte aplikaci teď a po kreslení, vrstvu s bitovou kopií se přidá a přenáší podél vykresleného cesty:

![](graphics-animation-walkthrough-images/00-final-app.png "Vrstva se bitovou kopii je přidána a přenáší podél vykresleného cesty")

## <a name="summary"></a>Souhrn

V tomto článku jsme provedl příklad, který je vázaný grafika a animace koncepty společně. Nejdřív jsme vám ukázal, jak používat základní grafiky k vykreslení cestu `UIView` v reakci na uživatele touch. Potom jsme vám ukázal, jak lze pomocí základní animace na bitovou kopii cestují podél této cestě.


## <a name="related-links"></a>Související odkazy

- [Core Animation](~/ios/platform/graphics-animation-ios/core-animation.md)
- [Core Graphics](~/ios/platform/graphics-animation-ios/core-graphics.md)
- [Základní animace recepty](https://developer.xamarin.com/recipes/ios/animation/coreanimation)
