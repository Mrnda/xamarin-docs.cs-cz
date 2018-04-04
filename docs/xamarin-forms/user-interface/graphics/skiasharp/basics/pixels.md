---
title: Pixelů a jednotky nezávislé na zařízení
description: Prozkoumat rozdíly mezi SkiaSharp souřadnice a souřadnice Xamarin.Forms
ms.prod: xamarin
ms.technology: xamarin-forms
ms.assetid: 26C25BB8-FBE8-4B77-B01D-16A163A16890
author: charlespetzold
ms.author: chape
ms.date: 02/09/2017
ms.openlocfilehash: dd9694a05632cd5f37cb583d15bc93311a49cfdc
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/04/2018
---
# <a name="pixels-and-device-independent-units"></a>Pixelů a jednotky nezávislé na zařízení

_Prozkoumat rozdíly mezi SkiaSharp souřadnice a souřadnice Xamarin.Forms_

Tento článek popisuje rozdíly v souřadnicový systém používán SkiaSharp a Xamarin.Forms. Můžete získat informace o převod mezi dvěma systémy souřadnic a také kreslení grafiky, který vyplnění konkrétní oblasti:

![](pixels-images/screenfillexample.png "Objekt, který vyplní celé obrazovky")

Pokud nějakou dobu, jste byla programování v Xamarin.Forms, může být chování pro Xamarin.Forms souřadnice a velikosti. Kroužky v článcích dva předchozí vykreslován zdát trochu malé, aby vám.

Tyto kroužky *jsou* malé ve srovnání s Xamarin.Forms velikosti. Ve výchozím nastavení se SkiaSharp získává v jednotkách pixelů, zatímco Xamarin.Forms základny souřadnice a velikosti na jednotce nezávislé na zařízení, vytvořit základní platformou. (Další informace o Xamarin.Forms souřadnicový systém naleznete v [kapitoly 5. Plánování práce s velikostí](~/xamarin-forms/creating-mobile-apps-xamarin-forms/summaries/chapter05.md) knihy *vytváření mobilních aplikací s Xamarin.Forms*.)

Na stránku [ **SkewSharpFormsDemos** ](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/Demos/) program s názvem **prostor velikost** používá SkiaSharp textového výstupu zobrazuje velikost zobrazení plochy ze tří různých zdrojů:

- Normální Xamarin.Forms [ `Width` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Width/) a [ `Height` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Height/) vlastnosti `SKCanvasView` objektu.
- [ `CanvasSize` ](https://developer.xamarin.com/api/property/SkiaSharp.Views.Forms.SKCanvasView.CanvasSize/) Vlastnost `SKCanvasView` objektu.
- [ `Size` ](https://developer.xamarin.com/api/property/SkiaSharp.SKImageInfo.Size/) Vlastnost `SKImageInfo` hodnotu, která je konzistentní s `Width` a `Height` vlastnosti používané ve dvou předchozí stránky.

[ `SurfaceSizePage` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/SkiaSharpFormsDemos/SkiaSharpFormsDemos/SkiaSharpFormsDemos/Basics/SurfaceSizePage.cs) Třída ukazuje, jak zobrazit tyto hodnoty. Konstruktor uloží `SKCanvasView` objektu jako pole, aby byla přístupná v `PaintSurface` obslužné rutiny události:

```csharp
SKCanvasView canvasView;

public SurfaceSizePage()
{
    Title = "Surface Size";

    canvasView = new SKCanvasView();
    canvasView.PaintSurface += OnCanvasViewPaintSurface;
    Content = canvasView;
}
```

`SKCanvas` obsahuje šest různých `DrawText` metody, ale to [ `DrawText` ](https://developer.xamarin.com/api/member/SkiaSharp.SKCanvas.DrawText/p/System.String/System.Single/System.Single/SkiaSharp.SKPaint/) je nejjednodušší metoda:

```csharp
public void DrawText (String text, Single x, Single y, SKPaint paint)
```

Zadejte textový řetězec, kde je text, pokud chcete začít, souřadnic X a Y a `SKPaint` objektu. Souřadnice X Určuje, kde je umístěn na levé straně text, ale sledovat out: Určuje souřadnici Y umístění *směrného plánu* textu. Pokud někdy napsání ručně na řádkovaný dokumentu, do směrného plánu je na řádku, na které lokality znaků, a pod sestup které dolní dotahy (jako jsou ty na písmena g, p, otázek a y).

`SKPaint` Objekt umožňuje zadat barvu textu, rodiny písem a velikost textu. Ve výchozím nastavení [ `TextSize` ](https://developer.xamarin.com/api/property/SkiaSharp.SKPaint.TextSize/) vlastnost má hodnotu 12, což vede k velmi malé textové na zařízení s vysokým rozlišením, jako jsou telefony. V jinou možnost než nejjednodušší aplikace, budete také potřebovat některé informace o velikosti text, který je zobrazen. `SKPaint` Třída definuje [ `FontMetrics` ](https://developer.xamarin.com/api/property/SkiaSharp.SKPaint.FontMetrics/) vlastnost a několik [ `MeasureText` ](https://developer.xamarin.com/api/member/SkiaSharp.SKPaint.MeasureText/p/System.String/) metody, ale pro menší zvláštní požadavky [ `FontSpacing` ](https://developer.xamarin.com/api/property/SkiaSharp.SKPaint.FontSpacing/) vlastnost poskytuje doporučené hodnoty pro mezery mezi po sobě jdoucích řádků textu.

Následující `PaintSurface` obslužná rutina vytvoří `SKPaint` objekt pro `TextSize` 40 pixelů, které je požadované výšky text z horní části horních k dolnímu okraji dolní dotahy. `FontSpacing` Hodnotu, která `SKPaint` objekt vrátí o něco větší než, o 47 pixelů.

```csharp
void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
{
    SKImageInfo info = args.Info;
    SKSurface surface = args.Surface;
    SKCanvas canvas = surface.Canvas;

    canvas.Clear();

    SKPaint paint = new SKPaint
    {
        Color = SKColors.Black,
        TextSize = 40
    };

    float fontSpacing = paint.FontSpacing;
    float x = 20;               // left margin
    float y = fontSpacing;      // first baseline
    float indent = 100;

    canvas.DrawText("SKCanvasView Height and Width:", x, y, paint);
    y += fontSpacing;
    canvas.DrawText(String.Format("{0:F2} x {1:F2}",
                                  canvasView.Width, canvasView.Height),
                    x + indent, y, paint);
    y += fontSpacing * 2;
    canvas.DrawText("SKCanvasView CanvasSize:", x, y, paint);
    y += fontSpacing;
    canvas.DrawText(canvasView.CanvasSize.ToString(), x + indent, y, paint);
    y += fontSpacing * 2;
    canvas.DrawText("SKImageInfo Size:", x, y, paint);
    y += fontSpacing;
    canvas.DrawText(info.Size.ToString(), x + indent, y, paint);
}
```

Metoda zahájí první řádek textu souřadnici X 20 (pro malé okraje v levém) a souřadnici Y `fontSpacing`, což je trochu déle, než co je potřeba zobrazit úplnou výšku první řádek textu v horní části prostor pro zobrazení. Po každé volání `DrawText`, souřadnici Y vzroste jedno nebo dvě vždy o `fontSpacing`.

Tady je programy spuštěné na všech tří platformách:

[![](pixels-images/surfacesize-small.png "Trojitá snímek obrazovky stránky prostor velikost")](pixels-images/surfacesize-large.png#lightbox "Trojitá snímek obrazovky stránky prostor pro velikost")

Jak je vidět `CanvasSize` vlastnost `SKCanvasView` a `Size` vlastnost `SKImageInfo` hodnota jsou konzistentní v sestavách dimenze pixelů. `Height` a `Width` vlastnosti `SKCanvasView` jsou vlastnosti Xamarin.Forms a zobrazit zprávu velikost zobrazení v definované platformou jednotky nezávislé na zařízení.

Simulátoru iOS 7 na levé straně neobsahuje 2 počet pixelů na jednotka nezávislá na zařízení, 3 pixelů na jednotku se systémem Android 5 Nexus v centru a 925 Lumia Nokia na pravé straně je 2,25 pixelů na jednotku. Zda je proč jednoduché v kruhu uvedené starší vypadá o stejnou velikost na iPhone a Windows phone, ale je menší na telefon se systémem Android.

Pokud si přejete pracovat zcela v jednotky nezávislé na zařízení, můžete tak učinit nastavením `IgnorePixelScaling` vlastnost `SKCanvasView` k `true`. Nemusí ale jako výsledky. SkiaSharp vykreslí grafiky na menší prostor zařízení, s velikostí pixelů rovná velikosti zobrazení jednotky nezávislé na zařízení. (Například SkiaSharp využije zobrazení prostor 360 x 512 pixelů na Nexus 5.) Potom škálování této bitové kopie velikostí, což vede k významnému rastrový obrázek jaggies.

Pokud chcete zachovat stejné rozlišení obrázku, je lepší řešení napsat vlastní jednoduché funkce pro převod mezi dvěma systémy souřadnic.

Kromě `DrawCircle` metody `SKCanvas` také definuje dvě `DrawOval` metody, které Nakreslení elipsy. Elipsy je definované dva poloměr místo jednoho protokolu radius. Toto jsou známé jako *hlavní radius* a *menší radius*. `DrawOval` Metoda nevykresluje elipsy s dvěma poloměr paralelní na ose X a Y. Tohoto omezení lze překonat s transformací nebo použití cesty grafiky (na které se vztahují později), ale [to `DrawOval` metoda](https://developer.xamarin.com/api/member/SkiaSharp.SKCanvas.DrawOval/p/System.Single/System.Single/System.Single/System.Single/SkiaSharp.SKPaint/) názvy dvou poloměr argument `rx` a `ry` k označení, že jsou paralelní k osy X a Y:

```csharp
public void DrawOval (Single cx, Single cy, Single rx, Single ry, SKPaint paint)
```

Je možné k vykreslení elipsy, který vyplní povrch zobrazení? **Vyplnění elipsy** stránky ukazuje, jak. `PaintSurface` Obslužné rutiny událostí v [ **EllipseFillPage.xaml.cs** ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/SkiaSharpFormsDemos/SkiaSharpFormsDemos/SkiaSharpFormsDemos/Basics/EllipseFillPage.xaml.cs) třída odečítá poloviční šířku tahu z `xRadius` a `yRadius` hodnoty přizpůsobit celou elipsy a jeho popisují se v rámci prostor pro zobrazení:

```csharp
void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
{
    SKImageInfo info = args.Info;
    SKSurface surface = args.Surface;
    SKCanvas canvas = surface.Canvas;

    canvas.Clear();

    float strokeWidth = 50;
    float xRadius = (info.Width - strokeWidth) / 2;
    float yRadius = (info.Height - strokeWidth) / 2;

    SKPaint paint = new SKPaint
    {
        Style = SKPaintStyle.Stroke,
        Color = SKColors.Blue,
        StrokeWidth = strokeWidth
    };
    canvas.DrawOval(info.Width / 2, info.Height / 2, xRadius, yRadius, paint);
}
```

Zde je spuštěn na tři platformy:

[![](pixels-images/ellipsefill-small.png "Trojitá snímek obrazovky stránky prostor velikost")](pixels-images/ellipsefill-large.png#lightbox "Trojitá snímek obrazovky stránky prostor pro velikost")

[Jiných `DrawOval` metoda](https://developer.xamarin.com/api/member/SkiaSharp.SKCanvas.DrawOval/p/SkiaSharp.SKRect/SkiaSharp.SKPaint/) má [ `SGRect` ](https://developer.xamarin.com/api/type/SkiaSharp.SKRect/) argument, který je definována v souřadnice X a Y jeho levém horním a pravém dolním rohu obdélníku. Elipsy doplní obdélníku, což naznačuje, že je pravděpodobně možné ho použít **vyplnění elipsy** stránky takto:

```csharp
SKRect rect = new SKRect(0, 0, info.Width, info.Height);
canvas.DrawOval(rect, paint);
```

Nicméně, zkrátí všech hran obrysu se třemi tečkami na čtyři strany. Bude nutné upravit všechny `SKRect` na základě argumenty konstruktoru `strokeWidth` byly správné tyto funkce:

```csharp
SKRect rect = new SKRect(strokeWidth / 2,
                         strokeWidth / 2,
                         info.Width - strokeWidth / 2,
                         info.Height - strokeWidth / 2);
canvas.DrawOval(rect, paint);
```


## <a name="related-links"></a>Související odkazy

- [Rozhraní API SkiaSharp](https://developer.xamarin.com/api/root/SkiaSharp/)
- [SkiaSharpFormsDemos (sample)](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/Demos/)
