---
title: TextKit
description: Rozhraní API Text Kit nabízí výkonné text rozložení a vykreslování funkce Xamarin.iOS.
ms.prod: xamarin
ms.assetid: 1D0477E8-CD1E-48A9-B7C8-7CA892069EFF
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/19/2017
ms.openlocfilehash: 78ca0d9505c9f56dc6476bd04dab560a70b2c4b0
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/04/2018
---
# <a name="text-kit"></a>Text Kit

Text Kit je nové rozhraní API, které nabízí výkonné text rozložení a vykreslování funkce. Je postavená na nízkou úroveň základní Text framework, ale je mnohem jednodušší než základní Text.

Chcete-li k dispozici pro standardní ovládací prvky funkce Text Kit, byly několik ovládacích prvků textu iOS znovu implementovaná používat Kit Text, včetně:

-  UITextView
-  UITextField
-  UILabel


## <a name="architecture"></a>Architektura

Text Kit poskytuje Vrstvená architektura, která odděluje úložiště text z rozložení a zobrazení, včetně následující třídy:

-  `NSTextContainer` – Poskytuje systém souřadnic a geometry, který se používá k rozložení textu.
-  `NSLayoutManager` – Rozložen text vypnutím text do glyfů. 
-  `NSTextStorage` – Obsahuje textová data a také zpracovává aktualizace vlastností text dávky. Všechny aktualizace batch jsou předávány Správce rozložení pro vlastní zpracování změny, jako je například přepočítání rozložení a překreslování text.


Tyto tři třídy se použijí pro zobrazení, který vykreslí text. Integrovaný text zpracování zobrazení, jako například `UITextView`, `UITextField`, a `UILabel` už je nastavit, ale můžete vytvořit a použít je k jakémukoli `UIView` také instance.

Následující obrázek ukazuje této architektury:

 ![](textkit-images/textkitarch.png "Následující obrázek ukazuje architekturu Text Kit")

## <a name="text-storage-and-attributes"></a>Text úložiště a atributy

`NSTextStorage` Třída obsahuje text, který se zobrazí při zobrazení. Komunikuje také všechny změny na text -, jako jsou například změny znaků nebo jejich atributů – Pokud chcete Správce rozložení pro zobrazení. `NSTextStorage` dědí z `MSMutableAttributed` řetězec, povolení změn na text atributy, které mají být zadán v dávkách mezi `BeginEditing` a `EndEditing` volání.

Například následující fragment kódu Určuje změnu popředí a pozadí, barvy a cílem konkrétní rozsahy:

```csharp
textView.TextStorage.BeginEditing ();
textView.TextStorage.AddAttribute(UIStringAttributeKey.ForegroundColor, UIColor.Green, new NSRange(200, 400));
textView.TextStorage.AddAttribute(UIStringAttributeKey.BackgroundColor, UIColor.Black, new NSRange(210, 300));
textView.TextStorage.EndEditing ();
```

Po `EndEditing` je volána, změny se odesílají do Správce rozložení, který pak provede všechny potřebné rozložení a výpočty vykreslování textu, který se má zobrazit v zobrazení.

## <a name="layout-with-exclusion-path"></a>Rozložení s Cesta vyloučení

Text Kit také podporuje rozložení a umožňuje komplexní scénáře, jako je více sloupci a průchodu textu kolem zadané cesty volají *vyloučení cesty*. Vyloučení cesty se používají ke kontejneru text, který upravuje geometrie rozložení textu, způsobuje kolem zadané cesty toku textu.

Přidání cesta pro vyloučení vyžaduje, aby nastavení `ExclusionPaths` vlastnost rozložení správcem. Nastavení této vlastnosti způsobí, že Správce rozložení a zrušit platnost rozložení textu toku textu kolem cesta vyloučení.

### <a name="exclusion-based-on-a-cgpath"></a>Vyloučení podle CGPath

Vezměte v úvahu následující `UITextView` podtřídami implementace:

```csharp
public class ExclusionPathView : UITextView
{
    CGPath exclusionPath;
    CGPoint initialPoint;
    CGPoint latestPoint;
    UIBezierPath bezierPath;

    public ExclusionPathView (string text)
    {
        Text = text;
        ContentInset = new UIEdgeInsets (20, 0, 0, 0);
        BackgroundColor = UIColor.White;
        exclusionPath = new CGPath ();
        bezierPath = UIBezierPath.Create ();

        LayoutManager.AllowsNonContiguousLayout = false;
    }

    public override void TouchesBegan (NSSet touches, UIEvent evt)
    {
        base.TouchesBegan (touches, evt);

        var touch = touches.AnyObject as UITouch;

        if (touch != null) {
            initialPoint = touch.LocationInView (this);
        }
    }

    public override void TouchesMoved (NSSet touches, UIEvent evt)
    {
        base.TouchesMoved (touches, evt);

        UITouch touch = touches.AnyObject as UITouch;

        if (touch != null) {
            latestPoint = touch.LocationInView (this);
            SetNeedsDisplay ();
        }
    }

    public override void TouchesEnded (NSSet touches, UIEvent evt)
    {
        base.TouchesEnded (touches, evt);

        bezierPath.CGPath = exclusionPath;
        TextContainer.ExclusionPaths = new UIBezierPath[] { bezierPath };
    }

    public override void Draw (CGRect rect)
    {
        base.Draw (rect);

        if (!initialPoint.IsEmpty) {

            using (var g = UIGraphics.GetCurrentContext ()) {

                g.SetLineWidth (4);
                UIColor.Blue.SetStroke ();

                if (exclusionPath.IsEmpty) {
                    exclusionPath.AddLines (new CGPoint[] { initialPoint, latestPoint });
                } else {
                    exclusionPath.AddLineToPoint (latestPoint);
                }

                g.AddPath (exclusionPath);
                g.DrawPath (CGPathDrawingMode.Stroke);
            }
        }
    }
}
```

Tento kód přidává podporu pro vykreslení zobrazení textu pomocí základní grafické prvky. Vzhledem k tomu `UITextView` třída je teď vytvořená pro použití Text Kit pro jeho vykreslování textu a rozložení, může použít všechny funkce Kit Text, například nastavení vyloučení cesty.

> [!IMPORTANT]
> Tento příklad podtřídy `UITextView` přidat touch kreslení podpory. Vytvoření podtřídy `UITextView` není nutné funkce Text Kit.



Poté, co uživatel nevykresluje zobrazení textu vykresleného `CGPath` se použije pro `UIBezierPath` instance nastavením `UIBezierPath.CGPath` vlastnost:

```csharp
bezierPath.CGPath = exclusionPath;
```

Aktualizace následující řádek kódu umožňuje rozložení textu aktualizovat cesty:

```csharp
TextContainer.ExclusionPaths = new UIBezierPath[] { bezierPath };
```

Následující snímek obrazovky ukazuje, jak rozložení textu změní tok vykresleného cesty:

<!-- ![](textkit-images/exclusionpath1.png "This screenshot illustrates how the text layout changes to flow around the drawn path")--> 
![](textkit-images/exclusionpath2.png "Tato obrazovka ukazuje, jak rozložení textu změní tok vykresleného cesty")

Všimněte si, že Správce rozložení `AllowsNonContiguousLayout` vlastnost nastavena na hodnotu false v tomto případě. To způsobí, že rozložení pro všechny případy, kdy se text změní být přepočítána. Toto nastavení na hodnotu true, mohou využít výkonu zabráněním rozložení úplné obnovení, zejména v případě rozsáhlých dokumentů. Nastavení však `AllowsNonContiguousLayout` na hodnotu true by bránily cesta vyloučení aktualizaci rozložení v některých případech – například pokud je zadán text za běhu bez koncové znaků CR vrátit před cesta probíhá nastavení.


## <a name="related-links"></a>Související odkazy

- [Úvod do systému iOS 7 (ukázka)](https://developer.xamarin.com/samples/monotouch/IntroToiOS7)
- [Přehled uživatelského rozhraní iOSu 7](~/ios/platform/introduction-to-ios7/ios7-ui.md)
- [Zpracovávání úloh na pozadí](~/ios/app-fundamentals/backgrounding/index.md)
