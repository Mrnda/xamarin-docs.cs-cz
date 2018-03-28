---
title: Návod - použití vlastních ovládacích prvků pomocí návrháře Xamarin pro iOS
description: Tento článek obsahuje podrobný postup znázorňující postup vytvoření vlastního ovládacího prvku a použít jej v Návrháři Xamarin pro iOS. Ukazuje, jak zpřístupnit ovládacího prvku v panelu nástrojů návrháře, přetáhněte nebo vyřazením do zobrazení. Kromě toho ukazuje, jak implementovat ovládacího prvku, vykreslí správně v době návrhu a prostředí runtime, jakož i vytvoření vlastnosti, které lze nastavit v době návrhu.
ms.topic: article
ms.prod: xamarin
ms.assetid: 9032B32E-97BD-4DA6-9955-811B84682578
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/22/2017
ms.openlocfilehash: 4138ba0da1dd2174c53e6e35105c3199ea941f7f
ms.sourcegitcommit: 20ca85ff638dbe3a85e601b5eb09b2f95bda2807
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 03/28/2018
---
# <a name="walkthrough---using-custom-controls-with-the-xamarin-designer-for-ios"></a>Návod - použití vlastních ovládacích prvků pomocí návrháře Xamarin pro iOS

_Tento článek obsahuje podrobný postup znázorňující postup vytvoření vlastního ovládacího prvku a použít jej v Návrháři Xamarin pro iOS. Ukazuje, jak zpřístupnit ovládacího prvku v panelu nástrojů návrháře, přetáhněte nebo vyřazením do zobrazení. Kromě toho ukazuje, jak implementovat ovládacího prvku, vykreslí správně v době návrhu a prostředí runtime, jakož i vytvoření vlastnosti, které lze nastavit v době návrhu._

## <a name="requirements"></a>Požadavky

Návrhář Xamarin pro iOS je k dispozici v sadě Visual Studio pro Mac a Visual Studio 2015 a 2017 v systému Windows.

Tato příručka předpokládá znalost obsahu zahrnutého v [Začínáme provede](~/ios/get-started/index.md).

## <a name="walkthrough"></a>Návod

> [!IMPORTANT]
> Počínaje Xamarin.Studio 5.5, způsob, ve kterém jsou vytvořeny vlastní ovládací prvky se mírně liší na starší verze. Chcete-li vytvořit vlastní ovládací prvek, buď `IComponent` rozhraní je to nutné (přidružené implementace metody) nebo může být třída být opatřena poznámkou `[DesignTimeVisible(true)]`. Druhá metoda se používá v následujícím příkladu návod.


1. Vytvořte nové řešení z **iOS > aplikace > jediné zobrazení aplikace > C#** šablony, pojmenujte ji `ScratchTicket`a pokračujte v průvodci Nový projekt:

    [![](ios-designable-controls-walkthrough-images/01new.png "Vytvořte nové řešení")](ios-designable-controls-walkthrough-images/01new.png#lightbox)

1. Vytvoření nového souboru prázdné třídy s názvem `ScratchTicketView`:

    [![](ios-designable-controls-walkthrough-images/02new.png "Vytvořte novou třídu ScratchTicketView")](ios-designable-controls-walkthrough-images/02new.png#lightbox)

1. Přidejte následující kód pro `ScratchTicketView` třídy:

    ```csharp
    using System;
    using System.ComponentModel;
    using CoreGraphics;
    using Foundation;
    using UIKit;
    
    namespace ScratchTicket
    {
        [Register("ScratchTicketView"), DesignTimeVisible(true)]
        public class ScratchTicketView : UIView
        {
            CGPath path;
            CGPoint initialPoint;
            CGPoint latestPoint;
            bool startNewPath = false;
            UIImage image;
    
            [Export("Image"), Browsable(true)]
            public UIImage Image
            {
                get { return image; }
                set
                {
                    image = value;
                    SetNeedsDisplay();
                }
            }
    
            public ScratchTicketView(IntPtr p)
                : base(p)
            {
                Initialize();
            }
    
            public ScratchTicketView()
            {
                Initialize();
            }
    
            void Initialize()
            {
                initialPoint = CGPoint.Empty;
                latestPoint = CGPoint.Empty;
                BackgroundColor = UIColor.Clear;
                Opaque = false;
                path = new CGPath();
                SetNeedsDisplay();
            }
    
            public override void TouchesBegan(NSSet touches, UIEvent evt)
            {
                base.TouchesBegan(touches, evt);
    
                var touch = touches.AnyObject as UITouch;
    
                if (touch != null)
                {
                    initialPoint = touch.LocationInView(this);
                }
            }
    
            public override void TouchesMoved(NSSet touches, UIEvent evt)
            {
                base.TouchesMoved(touches, evt);
    
                var touch = touches.AnyObject as UITouch;
    
                if (touch != null)
                {
                    latestPoint = touch.LocationInView(this);
                    SetNeedsDisplay();
                }
            }
    
            public override void TouchesEnded(NSSet touches, UIEvent evt)
            {
                base.TouchesEnded(touches, evt);
                startNewPath = true;
            }
    
            public override void Draw(CGRect rect)
            {
                base.Draw(rect);
    
                using (var g = UIGraphics.GetCurrentContext())
                {
                    if (image != null)
                        g.SetFillColor((UIColor.FromPatternImage(image).CGColor));
                    else
                        g.SetFillColor(UIColor.LightGray.CGColor);
                    g.FillRect(rect);
    
                    if (!initialPoint.IsEmpty)
                    {
                        g.SetLineWidth(20);
                        g.SetBlendMode(CGBlendMode.Clear);
                        UIColor.Clear.SetColor();
    
                        if (path.IsEmpty || startNewPath)
                        {
                            path.AddLines(new CGPoint[] { initialPoint, latestPoint });
                            startNewPath = false;
                        }
                        else
                        {
                            path.AddLineToPoint(latestPoint);
                        }
    
                        g.SetLineCap(CGLineCap.Round);
                        g.AddPath(path);        
                        g.DrawPath(CGPathDrawingMode.Stroke);
                    }
                }
            }
        }
    }
    ```


1. Přidat `FillTexture.png`, `FillTexture2.png` a `Monkey.png` soubory (k dispozici [z Githubu](https://github.com/xamarin/ios-samples/blob/master/ScratchTicket/Resources/images.zip?raw=true)) k **prostředky** složky.
    
1. Dvakrát klikněte `Main.storyboard` soubor otevřete v Návrháři:

    [![](ios-designable-controls-walkthrough-images/03new.png "IOS návrháře")](ios-designable-controls-walkthrough-images/03new.png#lightbox)


1. Přetažením **Image zobrazení** z **sada nástrojů** na zobrazení ve scénáři.

    [![](ios-designable-controls-walkthrough-images/04new.png "Zobrazení Image přidat do rozložení")](ios-designable-controls-walkthrough-images/04new.png#lightbox)


1. Vyberte **Image zobrazení** a změňte jeho **Image** vlastnost `Monkey.png`.

    [! [] (ios navrhovatelé – ovládací prvky návod-bitové kopie nebo 05new.png "vlastnost Image zobrazení nastavení bitové kopie do Monkey.png)](ios-designable-controls-walkthrough-images/05new.png#lightbox)

    
1. Protože používáme velikost třídy budeme potřebovat k omezení zobrazení této bitové kopie. Kliknutím na bitovou kopii na uvést do režimu omezení dvakrát. Umožňuje omezit k centru kliknutím popisovač Připnutí center Zarovnat a vertikálně a horizontálně:

    [![](ios-designable-controls-walkthrough-images/06new.png "Zarovnání obrázku")](ios-designable-controls-walkthrough-images/06new.png#lightbox)

1. Chcete-li omezit výšku a šířku, klikněte na popisovače Připnutí velikost (ve tvaru kost popisovačů) a vyberte šířku a výšku v uvedeném pořadí:

    [![](ios-designable-controls-walkthrough-images/07new.png "Přidání omezení")](ios-designable-controls-walkthrough-images/07new.png#lightbox)


1. Aktualizace rámečku založené na omezeních kliknutím na tlačítko Aktualizovat na panelu nástrojů:

    [![](ios-designable-controls-walkthrough-images/08new.png "Omezení panelu nástrojů")](ios-designable-controls-walkthrough-images/08new.png#lightbox)


1. V dalším kroku sestavení projektu tak, aby **pomocné zobrazení lístku** se zobrazí v části **vlastní součásti** v panelu nástrojů:

    [![](ios-designable-controls-walkthrough-images/09new.png "Sada nástrojů vlastní komponenty")](ios-designable-controls-walkthrough-images/09new.png#lightbox)


1. Přetáhnout myší **pomocné zobrazení lístku** tak, aby se přes opic bitové kopie. Upravte popisovače přetažení tak lístku zobrazení pomocné vztahuje opic úplně, jak je uvedeno níže:

    [![](ios-designable-controls-walkthrough-images/10new.png "Zobrazení pomocné lístku přes zobrazení bitové kopie")](ios-designable-controls-walkthrough-images/10new.png#lightbox)

1. Omezte pomocné lístku zobrazení zobrazení bitovou kopii podle kreslení ohraničující obdélník vyberte obou zobrazeních. Vyberte možnosti omezit na šířky, výšky, Center a střední a aktualizace snímky podle omezení, jak je uvedeno níže:

    [![](ios-designable-controls-walkthrough-images/11new.png "Centrování a přidání omezení")](ios-designable-controls-walkthrough-images/11new.png#lightbox)


1. Spusťte aplikaci a "pomocné vypnout" bitovou kopii na nich opic.

    [![](ios-designable-controls-walkthrough-images/10-app.png "Spuštění ukázkové aplikace")](ios-designable-controls-walkthrough-images/10-app.png#lightbox)

## <a name="adding-design-time-properties"></a>Přidání vlastnosti doby návrhu

Návrhář zahrnuje taky podporu návrhu pro vlastní ovládací prvky typu vlastnost typu numeric, výčet, string, bool, CGSize, UIColor a UIImage. K předvedení, přidejme vlastnost, která má `ScratchTicketView` nastavit bitovou kopii, která "poškrábání vypnout."

Přidejte následující kód, který `ScratchTicketView` třídy vlastnosti:

```csharp
[Export("Image"), Browsable(true)]
public UIImage Image 
{
    get { return image; }
    set { 
            image = value;
            SetNeedsDisplay ();
        }
}
```

Chceme může také přidat hodnotu null. Zkontrolujte `Draw` metoda, například takto:

```csharp
public override void Draw(CGRect rect)
{
    base.Draw(rect);

    using (var g = UIGraphics.GetCurrentContext())
    {
        if (image != null)
            g.SetFillColor ((UIColor.FromPatternImage (image).CGColor));
        else
            g.SetFillColor (UIColor.LightGray.CGColor);
            
        g.FillRect(rect);

        if (!initialPoint.IsEmpty)
        {
             g.SetLineWidth(20);
             g.SetBlendMode(CGBlendMode.Clear);
             UIColor.Clear.SetColor();

             if (path.IsEmpty || startNewPath)
             {
                 path.AddLines(new CGPoint[] { initialPoint, latestPoint });
                 startNewPath = false;
             }
             else
             {
                 path.AddLineToPoint(latestPoint);
             }

             g.SetLineCap(CGLineCap.Round);
             g.AddPath(path);       
             g.DrawPath(CGPathDrawingMode.Stroke);
        }
    }
}
```

Včetně `ExportAttribute` a `BrowsableAttribute` s argumentem nastavena na `true` výsledkem vlastnost se zobrazuje v nástroji designer **vlastnost** panelu. Změna vlastnosti pro jiný obrázek, které jsou součástí projektu, například `FillTexture2.png`, výsledkem aktualizaci ovládacího prvku v době návrhu, jak je uvedeno níže:

 [![](ios-designable-controls-walkthrough-images/11-customproperty.png "Úprava vlastností v době návrhu")](ios-designable-controls-walkthrough-images/10-app.png#lightbox)

## <a name="summary"></a>Souhrn

V tomto článku jsme projít postup vytvoření vlastního ovládacího prvku, jakož i využívat v aplikace pro iOS pomocí návrháře iOS. Jsme viděli, jak vytvořit a sestavení ovládacího prvku být dostupné pro aplikace v nástroji designer **sada nástrojů**. Kromě toho jsme se podívali na implementaci ovládacího prvku tak, aby vykreslí správně v době návrhu a prostředí runtime, jakož i jak vystavit vlastnosti vlastního ovládacího prvku v návrháři.



## <a name="related-links"></a>Související odkazy

- [ScratchTicket (ukázka)](https://developer.xamarin.com/samples/monotouch/ScratchTicket/)
- [požadované obrázky (ukázka)](https://github.com/xamarin/ios-samples/blob/master/ScratchTicket/Resources/images.zip?raw=true)
