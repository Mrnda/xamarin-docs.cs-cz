---
title: "Sledování prstem více dotykového ovládání"
description: "Toto téma ukazuje, jak sledovat touch události z více prsty"
ms.topic: article
ms.prod: xamarin
ms.assetid: 048D51F9-BD6C-4B44-8C53-CCEF276FC5CC
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/16/2018
ms.openlocfilehash: 53e972211ce506b6bf32ee4785c853982528d92e
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 02/27/2018
---
# <a name="multi-touch-finger-tracking"></a>Sledování prstem více dotykového ovládání

_Toto téma ukazuje, jak sledovat touch události z více prsty_

Existují situace, kdy aplikace s více touch potřebuje ke sledování prsty, které jednotlivé současně přechází na obrazovce. Jeden Typická aplikace je finger-paint program. Chcete uživatele mohli kreslení jedné prstem, ale také k vykreslení více prsty najednou. Jako váš program zpracovává více událostí dotykového ovládání, musí se k rozlišení události, které odpovídají každé prstu. Android poskytuje kód ID pro tento účel, získávání a tento kód pro zpracování může být ale poněkud složité.

Pro všechny události související s konkrétní prstu kód ID zůstává stejné. Kód ID je přiřazen, pokud prstem nejprve dotykem obrazovky a stává neplatným po prstu zruší na obrazovce.
Tyto kódy ID jsou obecně velmi malé celá čísla a Android opětovně používá, je pro události novější dotykového ovládání.

Program, který sleduje prsty, které jednotlivé téměř vždy udržuje slovník pro sledování dotykového ovládání. Slovník klíč je ID kód, který identifikuje konkrétní prstu. Hodnota slovníku závisí na aplikaci. V [FingerPaint](https://developer.xamarin.com/samples/monodroid/ApplicationFundamentals/FingerPaint) programu, každý prstem tahu (z dotykového ovládání k uvolnění) souvisí s objekt, který obsahuje všechny informace potřebné k vykreslení linií této prstem. Program definuje malá `FingerPaintPolyline` třídu pro tento účel:

```csharp
class FingerPaintPolyline
{
    public FingerPaintPolyline()
    {
        Path = new Path();
    }

    public Color Color { set; get; }

    public float StrokeWidth { set; get; }

    public Path Path { private set; get; }
}
```

Každý lomenou čáru má barvu, šířku tahu a Android grafiky [ `Path` ](https://developer.xamarin.com/api/type/Android.Graphics.Path/) objekt, který chcete nahromadí a jak se přitahuje vykreslení více bodů řádku.

Zbývající část kód ukazuje následující obrázek je součástí `View` odvozených s názvem `FingerPaintCanvasView`. Aby třída udržuje slovník objektů typu `FingerPaintPolyline` během doby, které jsou právě aktivně vykreslovány pomocí jednoho nebo více prsty:

```csharp
Dictionary<int, FingerPaintPolyline> inProgressPolylines = new Dictionary<int, FingerPaintPolyline>();
```

Tohoto slovníku umožňuje zobrazení rychle získat `FingerPaintPolyline` informace spojené s konkrétní prstu.

`FingerPaintCanvasView` Třída také udržuje `List` objekt pro čáru lomených, které byly dokončeny:

```csharp
List<FingerPaintPolyline> completedPolylines = new List<FingerPaintPolyline>();
```

Objekty v tomto `List` jsou ve stejném pořadí, že byly vykreslován.

`FingerPaintCanvasView` přepsání dvě metody definované `View`: [ `OnDraw` ](https://developer.xamarin.com/api/member/Android.Views.View.OnDraw/p/Android.Graphics.Canvas/) a [ `OnTouchEvent` ](https://developer.xamarin.com/api/member/Android.Views.View.OnTouchEvent/p/Android.Views.MotionEvent/).
V jeho `OnDraw` nevykresluje dokončené čáru lomených přepsání, zobrazení a poté nakreslí čáru lomených v průběhu.

Přepis metody `OnTouchEvent` metoda začíná získáním `pointerIndex` z hodnoty `ActionIndex` vlastnost. To `ActionIndex` hodnota rozlišuje více prsty, ale není konzistentní napříč více událostí. Z tohoto důvodu použijete `pointerIndex` k získání ukazatele `id` z hodnoty `GetPointerId` metoda. Toto ID *je* konzistentní napříč více událostí:

```csharp
public override bool OnTouchEvent(MotionEvent args)
{
    // Get the pointer index
    int pointerIndex = args.ActionIndex;

    // Get the id to identify a finger over the course of its progress
    int id = args.GetPointerId(pointerIndex);

    // Use ActionMasked here rather than Action to reduce the number of possibilities
    switch (args.ActionMasked)
    {
        // ...
    }

    // Invalidate to update the view
    Invalidate();

    // Request continued touch input
    return true;
}
```

Všimněte si, že používá přepsání `ActionMasked` vlastnost `switch` příkaz místo `Action` vlastnost. Tady je důvod, proč:

Pokud pracujete s více touch `Action` vlastnost má hodnotu `MotionEventsAction.Down` pro první prstu na touch obrazovky a pak hodnoty `Pointer2Down` a `Pointer3Down` jako druhý a třetí prsty také touch na obrazovce. Jako zajistěte čtvrté a páté prsty, obraťte se `Action` vlastnost má číselných hodnot, které neodpovídá i pro členy `MotionEventsAction` – výčet! Potřebovali byste prozkoumat hodnoty bitové příznaky hodnoty interpretovat jejich významu.

Podobně jako prsty nechte na obrazovce kontakt `Action` vlastnost má hodnoty `Pointer2Up` a `Pointer3Up` pro druhý a třetí prsty, a `Up` pro první prstu.

`ActionMasked` Vlastnost přebírá méně počet hodnot, protože je určen pro použití ve spojení s `ActionIndex` vlastnost k rozlišení mezi více prstů. Když prsty touch na obrazovce, můžete pouze rovnat vlastnost `MotionEventActions.Down` pro první prstu a `PointerDown` pro následné prstů. Jako prsty ponechte na obrazovce `ActionMasked` má hodnoty `Pointer1Up` pro následné prsty a `Up` pro první prstu.

Při použití `ActionMasked`, `ActionIndex` umožňuje rozlišovat mezi následné prsty touch a nechte na obrazovce, ale je obvykle není nutné používat tuto hodnotu s výjimkou jako argument pro jiné metody v `MotionEvent` objektu. Pro více dotykového ovládání, jedním z nejdůležitějších z těchto metod je `GetPointerId` názvem ve výše uvedeném kódu. Metoda vrátí hodnotu, která vám pomůže klíče slovníku přidružit konkrétní události prstů.

`OnTouchEvent` Potlačení v [FingerPaint](https://developer.xamarin.com/samples/monodroid/ApplicationFundamentals/FingerPaint) programu procesy `MotionEventActions.Down` a `PointerDown` události stejně jako tím, že vytvoříte novou `FingerPaintPolyline` objektu a její přidání do slovníku:

```csharp
public override bool OnTouchEvent(MotionEvent args)
{
    // Get the pointer index
    int pointerIndex = args.ActionIndex;

    // Get the id to identify a finger over the course of its progress
    int id = args.GetPointerId(pointerIndex);

    // Use ActionMasked here rather than Action to reduce the number of possibilities
    switch (args.ActionMasked)
    {
        case MotionEventActions.Down:
        case MotionEventActions.PointerDown:

            // Create a Polyline, set the initial point, and store it
            FingerPaintPolyline polyline = new FingerPaintPolyline
            {
                Color = StrokeColor,
                StrokeWidth = StrokeWidth
            };

            polyline.Path.MoveTo(args.GetX(pointerIndex),
                                 args.GetY(pointerIndex));

            inProgressPolylines.Add(id, polyline);
            break;
        // ...
    }
    // ...        
}
```

Všimněte si, že `pointerIndex` slouží také k získání pozici prstu v rámci zobrazení. Všechny informace touch přidružen `pointerIndex` hodnotu. `id` Jednoznačně identifikuje prsty napříč více zpráv, tak, aby je použít k vytvoření položky dictionary.

Podobně `OnTouchEvent` přepsat také obslužné rutiny `MotionEventActions.Up` a `Pointer1Up` stejně jako přenesením dokončené lomenou čáru na `completedPolylines` kolekce, mohou být vykresleny během `OnDraw` přepsat. Kód také odebere `id` položky ze slovníku:

```csharp
public override bool OnTouchEvent(MotionEvent args)
{
    // ...
    switch (args.ActionMasked)
    {
        // ...
        case MotionEventActions.Up:
        case MotionEventActions.Pointer1Up:

            inProgressPolylines[id].Path.LineTo(args.GetX(pointerIndex),
                                                args.GetY(pointerIndex));

            // Transfer the in-progress polyline to a completed polyline
            completedPolylines.Add(inProgressPolylines[id]);
            inProgressPolylines.Remove(id);
            break;

        case MotionEventActions.Cancel:
            inProgressPolylines.Remove(id);
            break;
    }
    // ...        
}
```

Teď pro složité část.

Mezi na nabídku a událostí, obecně existuje mnoho `MotionEventActions.Move` události. Tyto jsou seskupeny v jednom volání `OnTouchEvent`, a jejich musí proto liší od `Down` a `Up` události. `pointerIndex` Hodnotu dříve získali z `ActionIndex` vlastností se musí ignorovat. Místo toho metodu musí získat více `pointerIndex` hodnoty podle opakování ve smyčce mezi 0 a `PointerCount` vlastnost a získat `id` každého z nich `pointerIndex` hodnoty:

```csharp
public override bool OnTouchEvent(MotionEvent args)
{
    // ...
    switch (args.ActionMasked)
    {
        // ...
        case MotionEventActions.Move:

            // Multiple Move events are bundled, so handle them differently
            for (pointerIndex = 0; pointerIndex < args.PointerCount; pointerIndex++)
            {
                id = args.GetPointerId(pointerIndex);

                inProgressPolylines[id].Path.LineTo(args.GetX(pointerIndex),
                                                    args.GetY(pointerIndex));
            }
            break;
        // ...
    }
    // ...        
}
```

Tento typ zpracování umožňuje [FingerPaint](https://developer.xamarin.com/samples/monodroid/ApplicationFundamentals/FingerPaint) programu Sledování prsty, které jednotlivé a kreslení výsledky na obrazovce:

[![Příklad snímek obrazovky FingerPaint příklad](touch-tracking-images/image01.png)](touch-tracking-images/image01.png)

Nyní jste se seznámili jak můžete sledovat jednotlivé prsty, které na obrazovce a rozlišit mezi nimi.


## <a name="related-links"></a>Související odkazy

- [Průvodce ekvivalentní Xamarin iOS](~/ios/app-fundamentals/touch/touch-tracking.md)
- [FingerPaint (ukázka)](https://developer.xamarin.com/samples/monodroid/ApplicationFundamentals/FingerPaint)
