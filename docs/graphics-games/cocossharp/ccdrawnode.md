---
title: Kreslení geometrie pomocí CCDrawNode
description: CCDrawNode poskytuje metody pro kreslení primitivní objekty, například řádky, kružnice a trojúhelníčky.
ms.topic: article
ms.prod: xamarin
ms.assetid: 46A3C3CE-74CC-4A3A-AB05-B694AE182ADB
ms.technology: xamarin-cross-platform
author: charlespetzold
ms.author: chape
ms.date: 03/24/2017
ms.openlocfilehash: 5a2471981f2e88ff8af9a803ff8f5a99e5b9266f
ms.sourcegitcommit: 4f1b508caa8e7b6ccf85d167ea700a5d28b0347e
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/03/2018
---
# <a name="drawing-geometry-with-ccdrawnode"></a>Kreslení geometrie pomocí CCDrawNode

_`CCDrawNode` poskytuje metody pro kreslení primitivní objekty, například řádky, kružnice a trojúhelníčky._

`CCDrawNode` Třída v CocosSharp poskytuje několik metod pro kreslení běžné geometrické obrazce. Dědí z `CCNode` třídy a obvykle se přidá `CCLayer` instance. Tento průvodce popisuje, jak používat `CCDrawNode` instance k provedení vlastní vykreslení. Také poskytuje kompletní seznam funkcí dostupných kreslení se snímky obrazovky a příklady kódu.


## <a name="creating-a-ccdrawnode"></a>Vytváření CCDrawNode

`CCDrawNode` Třídu lze použít k vykreslení geometrické objekty, například kružnice, obdélníky a řádky. Například následující ukázkový kód ukazuje, jak vytvořit `CCDrawNode` instanci, která je v nakreslí `CCLayer` implementace třídy:


```csharp
public class GameLayer : CCLayer
{
    public GameLayer ()
    {
        var drawNode = new CCDrawNode ();
        this.AddChild (drawNode);
        // Origin is bottom-left of the screen. This moves
        // the drawNode 100 pixels to the right and 100 pixels up
        drawNode.PositionX = 100;
        drawNode.PositionY = 100;

        drawNode.DrawCircle (
            center: new CCPoint (0, 0),
            radius: 20,
            color: CCColor4B.White);

    }
} 
```

Tento kód vytvoří následující kruhu za běhu:

![](ccdrawnode-images/image1.png "Tento kód vytvoří tento kruh za běhu")


## <a name="draw-method-details"></a>Kreslení podrobnosti o metodě

Podívejme se na několik podrobnosti související s vykreslením s `CCDrawNode`:


### <a name="draw-methods-positions-are-relative-to-the-ccdrawnode"></a>Kreslení metody, které jsou pozice relativně k CCDrawNode

Všechny metody kreslení vyžadují alespoň jeden pozice hodnotu pro vykreslování. Tato hodnota pozice je vzhledem k `CCDrawNode` instance. To znamená, že `CCDrawNode` sám má pozice a všechny kreslení volání na `CCDrawNode` také trvat jednu nebo více hodnot pozice. Chcete-li pochopit, jak tyto hodnoty sloučit, podíváme se na několik příkladů.

Nejprve podíváme `DrawCircle` výše uvedeném příkladu:


```csharp
...
drawNode.PositionX = 100;
drawNode.PositionY = 100;

drawNode.DrawCircle (center: new CCPoint (0, 0),
...
```

V takovém případě `CCDrawNode` je nastavený na (100,100), a vykresleného kroužek je u (0,0) vzhledem k `CCDrawNode`, což se zarovnaný 100 pixelů až a napravo od okraje v levém dolním rohu obrazovky herní kruhu.

`CCDrawNode` Také může být umístěný na počátku (spodní levé straně obrazovky) spoléhat na kruh pro odsazení:


```csharp
...
drawNode.PositionX = 0;
drawNode.PositionY = 0;

drawNode.DrawCircle (center: new CCPoint (50, 60),
...
```

Kód výše má za následek středu kruhu na 50 jednotky (`drawNode.PositionX` + `CCPoint.X`) napravo od levé straně obrazovky a 60 (`drawNode.PositionY` + `CCPoint.Y`) jednotky výše dolní části obrazovky.

Po zavolání metody kreslení, vykresleného objekt nelze upravit, pokud `CCDrawNode.Clear` metoda je volána, tak, aby přemístění je třeba provést na `CCDrawNode` sám sebe.

Objekty, které jsou vykreslovány pomocí `CCNodes` jsou také vliv `CCNode` instance `Rotation` a `Scale` vlastnosti.


### <a name="draw-methods-do-not-need-to-be-called-every-frame"></a>Kreslení metody nemusí být volána každý snímek

Zakreslit potřeba metody lze volat pouze jednou k vytvoření trvalého visual. V příkladu výše, volání `DrawCircle` v konstruktoru `GameLayer` – `DrawCircle` nemusí být volána každých rámce znovu kreslení kruhu, když se aktualizuje na obrazovce.

To se liší od metody kreslení v MonoGame, což obvykle vykreslí něco na obrazovku pro jen jeden snímek a které se musí volat každých rámce.

Pokud jsou volány kreslení metody každý snímek pak objekty budou se hromadit nakonec uvnitř volání `CCDrawNode` instance, výsledkem pokles v kmitočet snímků, jako jsou vykreslovány další objekty.


### <a name="each-ccdrawnode-supports-multiple-draw-calls"></a>Každý CCDrawNode podporuje více volání kreslení

`CCDrawNode` instance slouží k vykreslení více tvarů. To umožňuje komplexní vizuální objekty na být uzavřeny v jednoho objektu. Například následující kód slouží k vykreslení více kroužky s jedním `CCDrawNode`:


```csharp
for (int i = 0; i < 8; i++)
{
    drawNode.DrawCircle (
        center: new CCPoint (i*15, 0),
        radius: 20,
        color: CCColor4B.White);
} 
```

Výsledkem je na následujícím obrázku:

![](ccdrawnode-images/image2.png "Výsledkem této grafiky")


## <a name="draw-call-examples"></a>Kreslení příklady volání

Následující volání kreslení jsou k dispozici v `CCDrawNode`:

- [`DrawCatmullRom`](#drawcatmullrom)
- [`DrawCircle`](#drawcircle)
- [`DrawCubicBezier`](#drawcubicbezier)
- [`DrawEllipse`](#drawellipse)
- [`DrawLineList`](#drawlinelist)
- [`DrawPolygon`](#drawpolygon)
- [`DrawQuadBezier`](#drawquadbezier)
- [`DrawRect`](#drawrect)
- [`DrawSegment`](#drawsegment)
- [`DrawSolidArc`](#drawsolidarc)
- [`DrawSolidCircle`](#drawsolidcircle)
- [`DrawTriangleList`](#drawtrianglelist)


### <a name="drawcardinalspline"></a>DrawCardinalSpline

`DrawCardinalSpline` vytvoří křivka prostřednictvím proměnné počet bodů. 

`config` Parametr definuje, které body křivky předá. Následující příklad ukazuje křivkový, která prochází čtyři body.

`tension` Jak ostré nebo zaokrouhlí bodů na křivky zobrazí ovládací prvky parametrů. A `tension` hodnotu 0 způsobí zakřivené křivkový a `tension` hodnotu 1 způsobí křivky vykreslovány pomocí přímých čar a okrajů.

I když křivky jsou zakřivené čáry, CocosSharp nevykresluje křivky přímých řádků. `segments` Parametr řídí, kolik segmentuje sloužící k vykreslení křivky. Je větší počet výsledkem plynule zakřivené křivkový za cenu výkonu. 

Příklad kódu:


```csharp
var splinePoints = new List<CCPoint> ();
splinePoints.Add (new CCPoint (0, 0));
splinePoints.Add (new CCPoint (50, 70));
splinePoints.Add (new CCPoint (0, 140));
splinePoints.Add (new CCPoint (100, 210));

drawNode.DrawCardinalSpline (
    config: splinePoints,
    tension: 0,
    segments: 64,
    color:CCColor4B.Red); 
```

![](ccdrawnode-images/image3.png "Parametr segmenty řídí, kolik segmentuje sloužící k vykreslení křivky")


### <a name="drawcatmullrom"></a>DrawCatmullRom

`DrawCatmullRom` vytvoří křivka prostřednictvím proměnné počet bodů, podobně jako `DrawCardinalLine`. Tato metoda nezahrnuje pnutí parametr.

Příklad kódu:

```csharp
var splinePoints = new List<CCPoint> ();
splinePoints.Add (new CCPoint (0, 0));
splinePoints.Add (new CCPoint (80, 90));
splinePoints.Add (new CCPoint (100, 0));
splinePoints.Add (new CCPoint (0, 130)); 

drawNode.DrawCatmullRom (
    points: splinePoints,
    segments: 64); 
```

![](ccdrawnode-images/image4.png "DrawCatmullRom vytvoří křivka prostřednictvím proměnné počet bodů, podobně jako DrawCardinalLine")


### <a name="drawcircle"></a>DrawCircle

`DrawCircle` Vytvoří hraniční kruhu systému daného `radius`.

Příklad kódu:

```csharp
drawNode.DrawCircle (
    center:new CCPoint (0, 0),
    radius:20,
    color:CCColor4B.Yellow); 
```

![](ccdrawnode-images/image5.png "DrawCircle vytvoří hraniční kruhu daného úhlu")


### <a name="drawcubicbezier"></a>DrawCubicBezier

`DrawCubicBezier` Nakreslí zakřivenou mezi dva body, pomocí kontrolních bodů nastavení cesty mezi dva body.

Příklad kódu:

```csharp
drawNode.DrawCubicBezier (
    origin: new CCPoint (0, 0),
    control1: new CCPoint (50, 150),
    control2: new CCPoint (250, 150),
    destination: new CCPoint (170, 0),
    segments: 64,
    lineWidth: 1,
    color: CCColor4B.Green); 
```

 ![](ccdrawnode-images/image6.png "DrawCubicBezier nakreslí křivka mezi dva body.")


### <a name="drawellipse"></a>DrawEllipse

`DrawEllipse` Vytvoří obrys *elipsy*, což se často označuje jako oval (i když nejsou dva geometricky identické). Určeny obrazec se třemi tečkami `CCRect` instance.

Příklad kódu:


```csharp
drawNode.DrawEllipse (
    rect: new CCRect (0, 0, 130, 90),
    lineWidth: 2,
    color: CCColor4B.Gray); 
```

![](ccdrawnode-images/image8.png "DrawEllipse vytvoří obrys elipsy, který se často označuje jako oval")


### <a name="drawline"></a>DrawLine

`DrawLine` připojí se ke body řádku dané šířky. Tato metoda je podobná `DrawSegment`, s výjimkou vytvoří ploché koncových bodů a zaokrouhlí koncové body.

Příklad kódu:


```csharp
drawNode.DrawLine (
    from: new CCPoint (0, 0),
    to: new CCPoint (150, 30),
    lineWidth: 5,
    color:CCColor4B.Orange); 
```

![](ccdrawnode-images/image9.png "DrawLine připojí k body řádku dané šířky")


### <a name="drawlinelist"></a>DrawLineList

`DrawLineList` vytvoří více řádků připojením každý pár bodů určených `CCV3F_C4B` pole. `CCV3F_C4B` Struktura obsahuje hodnoty pro pozici a barvu. `verts` Parametru by měla vždycky obsahovat sudý počet bodů, jako je každý řádek definované dva body.

Příklad kódu:


```csharp
CCV3F_C4B[] verts = new CCV3F_C4B[] {
    // First line:
    new CCV3F_C4B( new CCPoint(0,0), CCColor4B.White),
    new CCV3F_C4B( new CCPoint(30,60), CCColor4B.White),
    // second line, will blend from white to red:
    new CCV3F_C4B( new CCPoint(60,0), CCColor4B.White),
    new CCV3F_C4B( new CCPoint(120,120), CCColor4B.Red)
};

drawNode.DrawLineList (verts); 
```

 ![](ccdrawnode-images/image10.png "Parametr verts by měla vždycky obsahovat sudý počet bodů, jak každý řádek je definované dva body.")




### <a name="drawpolygon"></a>DrawPolygon

`DrawPolygon` Vytvoří mnohoúhelníku vyplněné s tou druhou je proměnné šířky a barvy.

Příklad kódu:


```csharp
CCPoint[] verts = new CCPoint[] {
    new CCPoint(0,0),
    new CCPoint(0, 100),
    new CCPoint(50, 150),
    new CCPoint(100, 100),
    new CCPoint(100, 0)
};

drawNode.DrawPolygon (verts,
    count: verts.Length,
    fillColor: CCColor4B.White,
    borderWidth: 5,
    borderColor: CCColor4B.Red,
    closePolygon: true); 
```

![](ccdrawnode-images/image11.png "DrawPolygon vytvoří mnohoúhelníku vyplněné s tou druhou je proměnné šířky a barvy")


### <a name="drawquadbezier"></a>DrawQuadBezier

`DrawQuadBezier` dva body se připojí pomocí řádku. Se chová podobně jako `DrawCubicBezier` , ale podporuje pouze jeden řídicí bod.

Příklad kódu:


```csharp
drawNode.DrawQuadBezier (
    origin:new CCPoint (0, 0),
    control:new CCPoint (200, 0),
    destination:new CCPoint (0, 300),
    segments:64,
    lineWidth:1,
    color:CCColor4B.White);
```

![](ccdrawnode-images/image12.png "DrawQuadBezier připojí řádek dva body.")


### <a name="drawrect"></a>DrawRect

`DrawRect` Vytvoří obdélník vyplněné s tou druhou je proměnné šířky a barvy.

Příklad kódu: 


```csharp
var shape = new CCRect (
    0, 0, 100, 200);
drawNode.DrawRect(shape,
    fillColor:CCColor4B.Blue,
    borderWidth: 4,
    borderColor:CCColor4B.White); 
```

![](ccdrawnode-images/image13.png "DrawRect vytvoří obdélník vyplněné s tou druhou je proměnné šířky a barvy")


### <a name="drawsegment"></a>DrawSegment

`DrawSegment` dva body se připojí pomocí řádku proměnné šířky a barvy. Je podobná `DrawLine`, s výjimkou vytvoří zaokrouhlí koncových bodů, nikoli ploché koncové body.

Příklad kódu:


```csharp
drawNode.DrawSegment (from: new CCPoint (0, 0),
    to: new CCPoint (100, 200),
    radius: 5,
    color:new CCColor4F(1,1,1,1)); 
```

![](ccdrawnode-images/image14.png "DrawSegment připojí dva body řádku proměnné šířky a barvy")


### <a name="drawsolidarc"></a>DrawSolidArc

`DrawSolidArc` Vytvoří wedge vyplněné dané barvy a protokolu radius.

Příklad kódu:


```csharp
drawNode.DrawSolidArc(
    pos:new CCPoint(100, 100),
    radius:200,
    startAngle:0,
    sweepAngle:CCMathHelper.Pi/2, // this is in radians, clockwise
    color:CCColor4B.White); 
```

![](ccdrawnode-images/image15.png "DrawSolidArc vytvoří wedge vyplněné dané barvy a protokolu radius")


### <a name="drawsolidcircle"></a>DrawSolidCircle

`DrawCircle` Vytvoří kruh vyplněné daného úhlu.

Příklad kódu:


```csharp
drawNode.DrawSolidCircle(
    pos: new CCPoint (100, 100),
    radius: 50,
    color: CCColor4B.Yellow); 
```

![](ccdrawnode-images/image16.png "DrawCircle vytvoří kruh vyplněné daného úhlu")


### <a name="drawtrianglelist"></a>DrawTriangleList

`DrawTriangleList` Vytvoří seznam trojúhelníčky. Každý trojúhelníček je definována tři `CCV3F_C4B` instancí v matici. Počet vrcholy v poli předaných `verts` parametr musí být násobkem tři. Všimněte si, že pouze informace obsažené v `CCV3F_C4B` je pozice verts a jejich barva – `DrawTriangleList` metoda nepodporuje kreslení trojúhelníčky s textury.

Příklad kódu:


```csharp
CCV3F_C4B[] verts = new CCV3F_C4B[] {
    // First triangle:
    new CCV3F_C4B( new CCPoint(0,0), CCColor4B.White),
    new CCV3F_C4B( new CCPoint(30,60), CCColor4B.White),
    new CCV3F_C4B( new CCPoint(60,0), CCColor4B.White),
    // second triangle, each point has different colors:
    new CCV3F_C4B( new CCPoint(90,0), CCColor4B.Yellow),
    new CCV3F_C4B( new CCPoint(120,60), CCColor4B.Red),
    new CCV3F_C4B( new CCPoint(150,0), CCColor4B.Blue)
};

drawNode.DrawTriangleList (verts); 
```

![](ccdrawnode-images/image17.png "Vytvoří seznam trojúhelníčky DrawTriangleList")


## <a name="summary"></a>Souhrn

Tato příručka vysvětluje, jak vytvořit `CCDrawNode` a provádět na základě primitivní vykreslování. Poskytuje příklad jednotlivých volání kreslení.

## <a name="related-links"></a>Související odkazy

- [CCDrawNode rozhraní API](https://developer.xamarin.com/api/type/CocosSharp.CCDrawNode/)
- [Úplnou ukázku najdete na](https://developer.xamarin.com/samples/mobile/CCDrawNode/)
