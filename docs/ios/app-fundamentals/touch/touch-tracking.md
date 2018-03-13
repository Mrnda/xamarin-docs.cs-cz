---
title: "Sledování prstem více dotykového ovládání"
description: "Tento dokument ukazuje, jak sledovat touch události z více prsty"
ms.topic: article
ms.prod: xamarin
ms.assetid: 48E8B20D-0833-43D2-976A-0605DDB386E3
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/18/2017
ms.openlocfilehash: a9e3842611aab86d23a2b0c2a832efce18c22465
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 03/09/2018
---
# <a name="multi-touch-finger-tracking"></a>Sledování prstem více dotykového ovládání

_Tento dokument ukazuje, jak sledovat touch události z více prsty_

Existují situace, kdy aplikace s více touch potřebuje ke sledování prsty, které jednotlivé současně přechází na obrazovce. Jeden Typická aplikace je finger-paint program. Chcete uživatele mohli kreslení jedné prstem, ale také k vykreslení více prsty najednou. Jako váš program zpracovává více událostí dotykového ovládání, musí se k rozlišení mezi tyto prsty.

Když prstem nejprve dotykem na obrazovce, vytvoří iOS [ `UITouch` ](https://developer.xamarin.com/api/type/UIKit.UITouch/) objekt pro tento prstu. Tento objekt zůstává stejná jako prstu přesune na obrazovce a potom zruší na obrazovce, v tomto okamžiku je objekt zlikvidován. Ke sledování prsty, program byste neměli ukládání to `UITouch` objektu přímo. Místo toho můžete použít [ `Handle` ](https://developer.xamarin.com/api/property/Foundation.NSObject.Handle/) vlastnost typu `IntPtr` k jednoznačné identifikaci těchto `UITouch` objekty.

Program, který sleduje prsty, které jednotlivé téměř vždy udržuje slovník pro sledování dotykového ovládání. Pro aplikace iOS slovník klíč je `Handle` hodnotu, která identifikuje konkrétní prstu. Hodnota slovníku závisí na aplikaci. V [FingerPaint](https://developer.xamarin.com/samples/monotouch/ApplicationFundamentals/FingerPaint) programu, každý prstem tahu (z dotykového ovládání k uvolnění) souvisí s objekt, který obsahuje všechny informace potřebné k vykreslení linií této prstem. Program definuje malá `FingerPaintPolyline` třídu pro tento účel:

```csharp
class FingerPaintPolyline
{
    public FingerPaintPolyline()
    {
        Path = new CGPath();
    }

    public CGColor Color { set; get; }

    public float StrokeWidth { set; get; }

    public CGPath Path { private set; get; }
}
```

Každý lomenou čáru má barvu, šířku tahu a s iOS grafiky [ `CGPath` ](https://developer.xamarin.com/api/type/CoreGraphics.CGPath/) objekt, který chcete nahromadí a jak se přitahuje vykreslení více bodů řádku.


Je součástí všech zbytek kód ukazuje následující obrázek `UIView` odvozených s názvem `FingerPaintCanvasView`. Aby třída udržuje slovník objektů typu `FingerPaintPolyline` během doby, které jsou právě aktivně vykreslovány pomocí jednoho nebo více prsty:

```csharp
Dictionary<IntPtr, FingerPaintPolyline> inProgressPolylines = new Dictionary<IntPtr, FingerPaintPolyline>();
```

Tohoto slovníku umožňuje zobrazení rychle získat `FingerPaintPolyline` na základě informace spojené s každou prstem `Handle` vlastnost `UITouch` objektu.

`FingerPaintCanvasView` Třída také udržuje `List` objekt pro čáru lomených, které byly dokončeny:

```csharp
List<FingerPaintPolyline> completedPolylines = new List<FingerPaintPolyline>();
```

Objekty v tomto `List` jsou ve stejném pořadí, že byly vykreslován.

`FingerPaintCanvasView` přepsání pět metody definované `View`:

- [`TouchesBegan`](https://developer.xamarin.com/api/member/UIKit.UIResponder.TouchesBegan/p/Foundation.NSSet/UIKit.UIEvent/)
- [`TouchesMoved`](https://developer.xamarin.com/api/member/UIKit.UIResponder.TouchesMoved/p/Foundation.NSSet/UIKit.UIEvent/)
- [`TouchesEnded`](https://developer.xamarin.com/api/member/UIKit.UIResponder.TouchesEnded/p/Foundation.NSSet/UIKit.UIEvent/)
- [`TouchesCancelled`](https://developer.xamarin.com/api/member/UIKit.UIResponder.TouchesCancelled/p/Foundation.NSSet/UIKit.UIEvent/)
- [`Draw`](https://developer.xamarin.com/api/member/UIKit.UIView.Draw/p/CoreGraphics.CGRect/)

Různé `Touches` přepsání hromadit body, které tvoří čáru lomených.

[`Draw`] Přepsání nevykresluje dokončené čáru lomených a pak čáru lomených v průběhu:

```csharp
public override void Draw(CGRect rect)
{
    base.Draw(rect);

    using (CGContext context = UIGraphics.GetCurrentContext())
    {
        // Stroke settings
        context.SetLineCap(CGLineCap.Round);
        context.SetLineJoin(CGLineJoin.Round);

        // Draw the completed polylines
        foreach (FingerPaintPolyline polyline in completedPolylines)
        {
            context.SetStrokeColor(polyline.Color);
            context.SetLineWidth(polyline.StrokeWidth);
            context.AddPath(polyline.Path);
            context.DrawPath(CGPathDrawingMode.Stroke);
        }

        // Draw the in-progress polylines
        foreach (FingerPaintPolyline polyline in inProgressPolylines.Values)
        {
            context.SetStrokeColor(polyline.Color);
            context.SetLineWidth(polyline.StrokeWidth);
            context.AddPath(polyline.Path);
            context.DrawPath(CGPathDrawingMode.Stroke);
        }
    }
}
```

Každý z `Touches` přepsání potenciálně sestavy akce více prsty, uvedené jednu nebo více `UITouch` objekty uložené v `touches` argument pro metodu. `TouchesBegan` Přepsání cyklus prostřednictvím těchto objektů. Pro každou `UITouch` objektů, metoda vytvoří a inicializuje novou `FingerPaintPolyline` objektu, včetně ukládání počáteční umístění prstu získaný `LocationInView` metoda. To `FingerPaintPolyline` objekt přidán do `InProgressPolylines` pomocí slovníku `Handle` vlastnost `UITouch` objektu jako slovník klíč:

```csharp
public override void TouchesBegan(NSSet touches, UIEvent evt)
{
    base.TouchesBegan(touches, evt);

    foreach (UITouch touch in touches.Cast<UITouch>())
    {
        // Create a FingerPaintPolyline, set the initial point, and store it
        FingerPaintPolyline polyline = new FingerPaintPolyline
        {
            Color = StrokeColor,
            StrokeWidth = StrokeWidth,
        };

        polyline.Path.MoveToPoint(touch.LocationInView(this));
        inProgressPolylines.Add(touch.Handle, polyline);
    }
    SetNeedsDisplay();
}
```

Ukončí volání metody `SetNeedsDisplay` generovat volání `Draw` přepsání a k aktualizaci na obrazovce.

Jako prstem nebo prsty přesunout na obrazovce, `View` získá několik volání jeho `TouchesMoved` přepsat. Toto přepsání podobně projde `UITouch` objekty uložené v `touches` argument a přidá do cesty grafiky aktuální umístění prstu:

```csharp
public override void TouchesMoved(NSSet touches, UIEvent evt)
{
    base.TouchesMoved(touches, evt);

    foreach (UITouch touch in touches.Cast<UITouch>())
    {
        // Add point to path
        inProgressPolylines[touch.Handle].Path.AddLineToPoint(touch.LocationInView(this));
    }
    SetNeedsDisplay();
}
```

`touches` Kolekce obsahuje pouze ty `UITouch` objekty pro prsty, které jste přesunuli od posledního volání `TouchesBegan` nebo `TouchesMoved`. Pokud byste někdy potřebovali `UITouch` objekty odpovídající *všechny* prsty, které aktuálně v kontaktu s obrazovky, tyto informace jsou dostupné prostřednictvím `AllTouches` vlastnost `UIEvent` argument pro metodu.

`TouchesEnded` Přepsání má dvě úlohy. Vytvoření posledního bodu ji musíte přidat do cesty grafiky a přenos `FingerPaintPolyline` objektu z `inProgressPolylines` adresář k `completedPolylines` seznamu:

```csharp
public override void TouchesEnded(NSSet touches, UIEvent evt)
{
    base.TouchesEnded(touches, evt);

    foreach (UITouch touch in touches.Cast<UITouch>())
    {
        // Get polyline from dictionary and remove it from dictionary
        FingerPaintPolyline polyline = inProgressPolylines[touch.Handle];
        inProgressPolylines.Remove(touch.Handle);

        // Add final point to path and save with completed polylines
        polyline.Path.AddLineToPoint(touch.LocationInView(this));
        completedPolylines.Add(polyline);
    }
    SetNeedsDisplay();
}
```

`TouchesCancelled` Přepsání se zpracovává souborem jednoduše zrušení `FingerPaintPolyline` objekt ve slovníku:

```csharp
public override void TouchesCancelled(NSSet touches, UIEvent evt)
{
    base.TouchesCancelled(touches, evt);

    foreach (UITouch touch in touches.Cast<UITouch>())
    {
        inProgressPolylines.Remove(touch.Handle);
    }
    SetNeedsDisplay();
}
```

Jako celek by tohoto zpracování umožňuje [FingerPaint](https://developer.xamarin.com/samples/monotouch/ApplicationFundamentals/FingerPaint) programu Sledování prsty, které jednotlivé a kreslení výsledky na obrazovce:

[![](touch-tracking-images/image01.png "Sledování jednotlivých prsty a kreslení výsledky na obrazovce")](touch-tracking-images/image01.png#lightbox)

Nyní jste se seznámili jak můžete sledovat jednotlivé prsty, které na obrazovce a rozlišit mezi nimi.



## <a name="related-links"></a>Související odkazy

- [Ekvivalentní Průvodce Xamarin Android](~/android/app-fundamentals/touch/touch-tracking.md)
- [FingerPaint (ukázka)](https://developer.xamarin.com/samples/monotouch/ApplicationFundamentals/FingerPaint)
