---
title: Úvod do SkiaSharp
description: To poskytuje stručný úvod do Principy SkiaSharp
ms.prod: xamarin
ms.assetid: 19506F08-2603-465E-A806-6BD01638DE90
ms.technology: xamarin-cross-platform
author: charlespetzold
ms.author: chape
ms.date: 09/14/2017
ms.openlocfilehash: b16792a506b131be07c52275e3f40cbb8d5fca94
ms.sourcegitcommit: 775a7d1cbf04090eb75d0f822df57b8d8cff0c63
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/18/2018
---
# <a name="an-introduction-to-skiasharp"></a>Úvod do SkiaSharp

_To poskytuje stručný úvod do Principy SkiaSharp_

SkiaSharp poskytuje bohatý a výkonné 2D grafického rozhraní API, které můžete použít k vykreslení do 2D vyrovnávací paměti.  Ty můžete použít k implementaci vlastních prvků uživatelského rozhraní a 2D obrázky, které lze začlenit do vaší aplikace.  SkiaSharp je rozhraní .NET vazbu ke [Skia](https://skia.org) knihovny a dědí funkcí a výkonu v této knihovně.

Knihovny je aktuálně k dispozici jako napříč platformami [balíček NuGet](https://www.nuget.org/packages/SkiaSharp), můžete ho přidat do projektu přidáním odkazu NuGet.

Kreslení, vytvoří kód `SkCanvas` který popisuje prostor, kde bude probíhat kreslení operace.

## <a name="obtaining-an-skcanvas"></a>Získání SKCanvas

```csharp
using (var surface = SKSurface.Create (width: 640, height: 480, SKImageInfo.PlatformColorType, SKAlphaType.Premul)) {
    SKCanvas myCanvas = surface.Canvas;

    // Your drawing code goes here.
}
```

## <a name="drawing-on-skcanvas"></a>Kreslení na SKCanvas

`SKCanvas` Používá model kreslení podobně jako v smyslu jiných kreslení modelů, je možné, že znáte, používá barvy s kanálem volitelné průhlednost a můžete kreslení čar, oblouky, textu a obrázků.

Níže jsou uvedeny jen některé z mnoha různých věcí, které lze provést pomocí SkiaSharp.  V příkladech níže proměnnou `canvas` je typu SKCanvas.

### <a name="drawing-xamagon"></a>Kreslení Xamagon

Tento příklad nevykresluje logo společnosti Xamarin Xamagon:

```csharp
// clear the canvas / fill with white
canvas.Clear (SKColors.White);

// set up drawing tools
using (var paint = new SKPaint ()) {
  paint.IsAntialias = true;
  paint.Color = new SKColor (0x2c, 0x3e, 0x50);
  paint.StrokeCap = SKStrokeCap.Round;

  // create the Xamagon path
  using (var path = new SKPath ()) {
    path.MoveTo (71.4311121f, 56f);
    path.CubicTo (68.6763107f, 56.0058575f, 65.9796704f, 57.5737917f, 64.5928855f, 59.965729f);
    path.LineTo (43.0238921f, 97.5342563f);
    path.CubicTo (41.6587026f, 99.9325978f, 41.6587026f, 103.067402f, 43.0238921f, 105.465744f);
    path.LineTo (64.5928855f, 143.034271f);
    path.CubicTo (65.9798162f, 145.426228f, 68.6763107f, 146.994582f, 71.4311121f, 147f);
    path.LineTo (114.568946f, 147f);
    path.CubicTo (117.323748f, 146.994143f, 120.020241f, 145.426228f, 121.407172f, 143.034271f);
    path.LineTo (142.976161f, 105.465744f);
    path.CubicTo (144.34135f, 103.067402f, 144.341209f, 99.9325978f, 142.976161f, 97.5342563f);
    path.LineTo (121.407172f, 59.965729f);
    path.CubicTo (120.020241f, 57.5737917f, 117.323748f, 56.0054182f, 114.568946f, 56f);
    path.LineTo (71.4311121f, 56f);
    path.Close ();

    // draw the Xamagon path
    canvas.DrawPath (path, paint);
  }
}
```

### <a name="drawing-text"></a>Kreslení textu

```csharp
// clear the canvas / fill with white
canvas.DrawColor (SKColors.White);

// set up drawing tools
using (var paint = new SKPaint ()) {
  paint.TextSize = 64.0f;
  paint.IsAntialias = true;
  paint.Color = new SKColor (0x42, 0x81, 0xA4);
  paint.IsStroke = false;

  // draw the text
  canvas.DrawText ("Skia", 0.0f, 64.0f, paint);
}
```

### <a name="drawing-bitmaps"></a>Kreslení rastrové obrázky

```csharp
Stream fileStream = File.OpenRead ("MyImage.png");

// clear the canvas / fill with white
canvas.DrawColor (SKColors.White);

// decode the bitmap from the stream
using (var stream = new SKManagedStream(fileStream))
using (var bitmap = SKBitmap.Decode(stream))
using (var paint = new SKPaint()) {
  canvas.DrawBitmap(bitmap, SKRect.Create(Width, Height), paint);
}
```

### <a name="drawing-with-image-filters"></a>Kreslení pomocí filtrů bitové kopie

```csharp
Stream fileStream = File.OpenRead ("MyImage.png"); // open a stream to an image file

// clear the canvas / fill with white
canvas.DrawColor (SKColors.White);

// decode the bitmap from the stream
using (var stream = new SKManagedStream(fileStream))
using (var bitmap = SKBitmap.Decode(stream))
using (var paint = new SKPaint()) {
  // create the image filter
  using (var filter = SKImageFilter.CreateBlur(5, 5)) {
    paint.ImageFilter = filter;

    // draw the bitmap through the filter
    canvas.DrawBitmap(bitmap, SKRect.Create(width, height), paint);
  }
}
```

## <a name="more-information"></a>Další informace

Další informace o používání SkiaSharp naleznete na [online dokumentaci k rozhraní API](https://developer.xamarin.com/api/namespace/SkiaSharp/)


## <a name="related-links"></a>Související odkazy

- [IOS SkiaSharp sešitu](https://developer.xamarin.com/workbooks/graphics/skiasharp/logo/skialogo-ios.workbook)
