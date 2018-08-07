---
title: Používání vlastních ovládacích prvků v IOS designeru
description: Tento dokument popisuje, jak vytvořit vlastní ovládací prvek a jeho použití s návrháři Xamarin pro iOS. Ukazuje, jak zpřístupnit ovládací prvek v panelu nástrojů návrháře iOS, implementaci ovládacího prvku tak, aby se správně vykresluje a navrhnout tak čas a další.
ms.prod: xamarin
ms.assetid: 9032B32E-97BD-4DA6-9955-811B84682578
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/22/2017
ms.openlocfilehash: 0097cdf006944a51d938ea91d3ea0b0c2aee08cf
ms.sourcegitcommit: bf51592be39b2ae3d63d029be1d7745ee63b0ce1
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/06/2018
ms.locfileid: "39573578"
---
# <a name="using-custom-controls-with-the-ios-designer"></a>Používání vlastních ovládacích prvků v IOS designeru

## <a name="requirements"></a>Požadavky

Návrhář Xamarin pro iOS je k dispozici v sadě Visual Studio pro Mac a Visual Studio 2015 a 2017 ve Windows.

Tento průvodce to předpokládá znalost obsah do [Začínáme provede](~/ios/get-started/index.md).

## <a name="walkthrough"></a>Názorný postup

> [!IMPORTANT]
> Od verze Xamarin.Studio 5.5, způsob, ve kterém se vytvoří vlastní ovládací prvky se mírně liší na starší verze. K vytvoření vlastního ovládacího prvku, buď `IComponent` rozhraní se ale vyžaduje (s přidružené implementace metody) nebo může být třída označena s `[DesignTimeVisible(true)]`. Druhá metoda se používá v následujícím příkladu návodu.


1. Vytvoření nového řešení z **iOS > aplikace > jediné zobrazení aplikace > C#** šablony, pojmenujte ho `ScratchTicket`a pokračujte v průvodci Nový projekt:

    [![](ios-designable-controls-walkthrough-images/01new.png "Vytvořit nové řešení")](ios-designable-controls-walkthrough-images/01new.png#lightbox)

1. Vytvořte nový soubor prázdnou třídu s názvem `ScratchTicketView`:

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

    [![](ios-designable-controls-walkthrough-images/03new.png "IOS Designer")](ios-designable-controls-walkthrough-images/03new.png#lightbox)


1. Přetažení **zobrazení obrázku** z **nástrojů** na zobrazení ve scénáři.

    [![](ios-designable-controls-walkthrough-images/04new.png "Zobrazení Image přidat do rozložení")](ios-designable-controls-walkthrough-images/04new.png#lightbox)


1. Vyberte **zobrazení obrázku** a změňte jeho **Image** vlastnost `Monkey.png`.

    [![](ios-designable-controls-walkthrough-images/05new.png "Nastavení vlastnosti Image zobrazit obrázek na Monkey.png")](ios-designable-controls-walkthrough-images/05new.png#lightbox)

    
1. Protože používáme třídy velikostí potřebujeme k omezení zobrazení tohoto obrázku. Kliknutím na obrázek, který dvakrát se ji převést do režimu omezení. Pojďme omezte ji do centra kliknutím popisovač Připnutí System center a vertikální i horizontální zarovnání:

    [![](ios-designable-controls-walkthrough-images/06new.png "Zarovnání obrázku")](ios-designable-controls-walkthrough-images/06new.png#lightbox)

1. Chcete-li omezit výšku a šířku, klikněte na úchyty připnete velikost (ve tvaru kost úchyty) a vyberte šířku a výšku v uvedeném pořadí:

    [![](ios-designable-controls-walkthrough-images/07new.png "Přidání omezení")](ios-designable-controls-walkthrough-images/07new.png#lightbox)


1. Aktualizujte rámce podle omezení kliknutím na tlačítko Aktualizovat na panelu nástrojů:

    [![](ios-designable-controls-walkthrough-images/08new.png "Panel nástrojů pro omezení")](ios-designable-controls-walkthrough-images/08new.png#lightbox)


1. V dalším kroku sestavení projektu tak, aby **pomocný zobrazení lístek** se zobrazí v části **vlastních komponent** v sadě nástrojů:

    [![](ios-designable-controls-walkthrough-images/09new.png "Panel vlastních komponent")](ios-designable-controls-walkthrough-images/09new.png#lightbox)


1. Přetáhnout myší **pomocný zobrazení lístek** tak, aby se zobrazí nad opic image. Přizpůsobit přetažením úchytů lístek zobrazení pomocný se vztahuje opic úplně, jak je znázorněno níže:

    [![](ios-designable-controls-walkthrough-images/10new.png "Zobrazení pomocné lístek zobrazení obrázku")](ios-designable-controls-walkthrough-images/10new.png#lightbox)

1. Omezení zobrazení pomocný lístek zobrazení obrázku kreslením ohraničující obdélník k výběru obou zobrazeních. Vyberte požadované možnosti, chcete-li omezit na šířku, výšku, System Center a střední a aktualizace snímky podle omezení, jak je znázorněno níže:

    [![](ios-designable-controls-walkthrough-images/11new.png "Zarovnání a přidání omezení")](ios-designable-controls-walkthrough-images/11new.png#lightbox)


1. Spusťte aplikaci a "pomocný vypnout" obrázek, který se zobrazí opic.

    [![](ios-designable-controls-walkthrough-images/10-app.png "Spuštění ukázkové aplikace")](ios-designable-controls-walkthrough-images/10-app.png#lightbox)

## <a name="adding-design-time-properties"></a>Přidání vlastnosti doby návrhu

Návrhář také zahrnuje podporu návrhu pro vlastní ovládací prvky číselný typ vlastnosti, výčtu, string, bool, CGSize, UIColor a UIImage. Abychom si předvedli, Pojďme přidat vlastnost, která má `ScratchTicketView` nastavení obrázku, který je "poškrábaný vypnout."

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

Může také chceme přidat kontrolu hodnot null do `Draw` metody, například takto:

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

Včetně `ExportAttribute` a `BrowsableAttribute` s argumentem nastaveným na hodnotu `true` výsledků ve vlastnosti se zobrazí v okně Návrhář **vlastnost** panel. Změna vlastnosti na jiné image, které jsou součástí projektu, jako například `FillTexture2.png`, výsledkem aktualizace ovládacího prvku v době návrhu, jak je znázorněno níže:

 [![](ios-designable-controls-walkthrough-images/11-customproperty.png "Úpravy vlastnosti doby návrhu")](ios-designable-controls-walkthrough-images/10-app.png#lightbox)

## <a name="summary"></a>Souhrn

V tomto článku jsme prošli postup při vytvoření vlastního ovládacího prvku, jakož i využívat v návrháři pro iOS pomocí aplikace pro iOS. Jsme viděli, jak vytvořit a sestavit ovládacího prvku, aby byl k dispozici pro aplikace v nástroji Návrhář **nástrojů**. Kromě toho jsme se podívali na implementaci ovládacího prvku tak, aby se správně vykresluje v době návrhu a modulu runtime, jakož i jak vystavit vlastnosti vlastní ovládací prvek v návrháři.



## <a name="related-links"></a>Související odkazy

- [ScratchTicket (ukázka)](https://developer.xamarin.com/samples/monotouch/ScratchTicket/)
- [požadované Image (ukázka)](https://github.com/xamarin/ios-samples/blob/master/ScratchTicket/Resources/images.zip?raw=true)
