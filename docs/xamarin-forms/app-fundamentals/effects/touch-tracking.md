---
title: "Vyvolání událostí z efekty"
description: "Efekt můžete definovat a vyvolat událost signalizace změny v základní nativní zobrazení. Tento článek ukazuje, jak implementovat nízké úrovně více touch prstem sledování a jak generovat události, které poukazují touch aktivity."
ms.topic: article
ms.prod: xamarin
ms.assetid: 6A724681-55EB-45B8-9EED-7E412AB19DD2
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 03/01/2017
ms.openlocfilehash: 0fd037e62bcdb1b2be4c93dc0d32ca76f4e1ba8e
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 03/09/2018
---
# <a name="invoking-events-from-effects"></a>Vyvolání událostí z efekty

_Efekt můžete definovat a vyvolat událost signalizace změny v základní nativní zobrazení. Tento článek ukazuje, jak implementovat nízké úrovně více touch prstem sledování a jak generovat události, které poukazují touch aktivity._

Účinek popsané v tomto článku poskytuje přístup k nízké úrovně touch události. Tyto události nízké úrovně nejsou k dispozici prostřednictvím existující `GestureRecognizer` třídy, ale jsou důležité pro některé typy aplikací. Například finger-paint potřebám aplikace ke sledování jednotlivých prsty, které v jejich přesunutí na obrazovce. Hudba klávesnice potřebuje ke zjištění odposlouchávání a uvolní na jednotlivé klíče, jakož i prstu, gliding z jednoho klíče na druhou v glissando.

Vliv je ideální pro více touch prstem sledování, protože může být připojen k žádnému Xamarin.Forms prvku.

## <a name="platform-touch-events"></a>Dotykové ovládání události platformy

IOS, Android a univerzální platformu Windows obsahují nízké úrovně rozhraní API, které umožňuje aplikacím ke zjištění touch aktivity. Tyto platformy, které všechny rozlišit mezi tři základní typy touch události:

- *Stisknutí*, když prstem dotykem obrazovky
- *Přesunout*, pokud se přesune prstem klepnou na obrazovce
- *Vydané*, až bude vydaná prstu z obrazovky

V prostředí s více touch můžete více prsty touch obrazovky ve stejnou dobu. Různých platforem zahrnuje (ID) identifikační číslo, které aplikace můžete použít k rozlišení mezi více prstů.

V iOS `UIView` třída definuje tři přepisovatelné metody `TouchesBegan`, `TouchesMoved`, a `TouchesEnded` odpovídající tyto tři základní události. Článek [více Touch prstem sledování](~/ios/app-fundamentals/touch/touch-tracking.md) popisuje, jak používat tyto metody. Aplikace iOS však není nutné přepsat třídu odvozenou z `UIView` používat tyto metody. IOS `UIGestureRecognizer` definuje také stejné tři metody a můžete připojit instance třídy, která je odvozena z `UIGestureRecognizer` žádnému `UIView` objektu.

V systému Android `View` třída definuje přepisovatelné metodu s názvem `OnTouchEvent` zpracovat všechny aktivity dotykového ovládání. Typ aktivity touch je definované členy výčtu `Down`, `PointerDown`, `Move`, `Up`, a `PointerUp` jak je popsáno v článku [více Touch prstem sledování](~/android/app-fundamentals/touch/touch-tracking.md). Android `View` také definuje událost s názvem `Touch` umožňuje obslužné rutiny události být připojen k žádné `View` objektu.

V univerzální platformu Windows (UWP), `UIElement` třída definuje události s názvem `PointerPressed`, `PointerMoved`, a `PointerReleased`. Tyto možnosti jsou popsány v článku [zpracovat vstup ukazatel článku na webu MSDN](/windows/uwp/input-and-devices/handle-pointer-input/) a dokumentaci rozhraní API pro [ `UIElement` ](/uwp/api/windows.ui.xaml.uielement/) třídy.

`Pointer` Rozhraní API v univerzální platformu Windows slouží ke sjednocení myši, touch a pera vstup. Z tohoto důvodu `PointerMoved` událost vyvolána, pokud myši přesune mezi element i v případě, že není tlačítko myši stisknuté. `PointerRoutedEventArgs` Objekt, který doprovází tyto události má vlastnost s názvem `Pointer` má vlastnost s názvem `IsInContact` který označuje, zda stisknutí tlačítka myši nebo prstem je v kontaktu s obrazovky.

Kromě toho UWP definuje dva další události s názvem `PointerEntered` a `PointerExited`. Ty naznačují, když myši nebo prstem přesune z jednoho prvku na jiný. Představte si třeba dvě u elementů s názvem a B. Oba elementy nainstalovali obslužné rutiny události ukazatele. Když prstem stiskem tlačítka na A, `PointerPressed` událost vyvolána. Při přesunu prstu, A vyvolá `PointerMoved` události. Pokud se prstu přesune z A do B, A vyvolá `PointerExited` vyvolá událost a B `PointerEntered` událostí. Pokud pak vydání prstu, vyvolá B `PointerReleased` událostí.

IOS a Android platformy se liší od UWP: zobrazení, které nejprve získá volání `TouchesBegan` nebo `OnTouchEvent` při prstem dotykem zobrazení nadále získat všechny touch aktivity i když Pokud prstu přesune na různá zobrazení. UWP můžete chovají podobně, pokud aplikace zaznamená ukazatele: V `PointerEntered` obslužné rutiny události, volání element `CapturePointer` a pak získá všechny aktivity touch z této prstu.

Přístup UWP prokáže být velmi užitečná pro některé typy aplikací, například hudba klávesnici. Každý klíč můžete zpracování událostí dotykového ovládání pro tento klíč a rozpoznat, kdy se má prstem posouvá z jednoho klíče na jiný pomocí `PointerEntered` a `PointerExited` události.

Z tohoto důvodu touch sledování účinek popsané v tomto článku implementuje UWP přístup.

## <a name="the-touch-tracking-effect-api"></a>Rozhraní API vliv sledování dotykového ovládání

[ **Touch sledování účinku ukázky** ](https://developer.xamarin.com/samples/xamarin-forms/effects/TouchTrackingEffectDemos/) ukázka obsahuje třídy (a výčet), které implementují nízké úrovně sledování dotykového ovládání. Tyto typy patří do oboru názvů `TouchTracking` a začínat slovem `Touch`. **TouchTrackingEffectDemos** zahrnuje projektu knihovny přenosných tříd `TouchActionType` výčet typu touch událostí:

```csharp
public enum TouchActionType
{
    Entered,
    Pressed,
    Moved,
    Released,
    Exited,
    Cancelled
}
```

Všechny platformy také zahrnovat událost, která určuje, že touch událost byla zrušena.

`TouchEffect` Je odvozena od třídy v PCL `RoutingEffect` a definuje událost s názvem `TouchAction` a metodu s názvem `OnTouchAction` , který spustí `TouchAction` událostí:

```csharp
public class TouchEffect : RoutingEffect
{
    public event TouchActionEventHandler TouchAction;

    public TouchEffect() : base("XamarinDocs.TouchEffect")
    {
    }

    public bool Capture { set; get; }

    public void OnTouchAction(Element element, TouchActionEventArgs args)
    {
        TouchAction?.Invoke(element, args);
    }
}
```

Všimněte si také `Capture` vlastnost. K zachycení událostí dotykového ovládání, musí aplikace nastavte tuto vlastnost na `true` před verzí `Pressed` událostí. Události dotykového ovládání, jinak hodnota chovají stejně jako v univerzální platformu Windows.

`TouchActionEventArgs` Třída v PCL obsahuje všechny informace, které doprovází jednotlivých událostí:

```csharp
public class TouchActionEventArgs : EventArgs
{
    public TouchActionEventArgs(long id, TouchActionType type, Point location, bool isInContact)
    {
        Id = id;
        Type = type;
        Location = location;
        IsInContact = isInContact;
    }

    public long Id { private set; get; }

    public TouchActionType Type { private set; get; }

    public Point Location { private set; get; }

    public bool IsInContact { private set; get; }
}
```

Aplikace můžete použít `Id` vlastnost pro sledování jednotlivých prstů. Upozornění `IsInContact` vlastnost. Tato vlastnost je vždy `true` pro `Pressed` události a `false` pro `Released` události. Je také vždy `true` pro `Moved` událostí na iOS a Android. `IsInContact` Vlastnost může být `false` pro `Moved` stisknutí událostí pro univerzální platformu Windows při spuštění programu na ploše a ukazatel myši přesune bez tlačítko.

Můžete použít `TouchEffect` třídy ve svých vlastních aplikacích zahrnutím soubor v projektu PCL na řešení a přidáním instanci do `Effects` skupiny libovolný element Xamarin.Forms. Připojit, aby obslužná rutina `TouchAction` událost, která má získat události dotykového ovládání.

Chcete-li použít `TouchEffect` ve vaší vlastní aplikaci, budete také potřebovat implementace platformy součástí **TouchTrackingEffectDemos** řešení.

## <a name="the-touch-tracking-effect-implementations"></a>Implementace vliv sledování dotykového ovládání

IOS, Android a UWP implementace `TouchEffect` jsou popsány níže počínaje nejjednodušší implementace (UWP) a konče implementace iOS, protože je více strukturálně složitější než jiné.

### <a name="the-uwp-implementation"></a>Implementace UWP

Implementace UWP `TouchEffect` je nejjednodušší. Obvyklým způsobem, třída odvozená z `PlatformEffect` a obsahuje dva atributy sestavení:

```csharp
[assembly: ResolutionGroupName("XamarinDocs")]
[assembly: ExportEffect(typeof(TouchTracking.UWP.TouchEffect), "TouchEffect")]

namespace TouchTracking.UWP
{
    public class TouchEffect : PlatformEffect
    {
        ...
    }
}
```

`OnAttached` Přepsání uloží určité informace jako pole a připojí obslužné rutiny pro všechny události ukazatele:

```csharp
public class TouchEffect : PlatformEffect
{
    FrameworkElement frameworkElement;
    TouchTracking.TouchEffect effect;
    Action<Element, TouchActionEventArgs> onTouchAction;

    protected override void OnAttached()
    {
        // Get the Windows FrameworkElement corresponding to the Element that the effect is attached to
        frameworkElement = Control == null ? Container : Control;

        // Get access to the TouchEffect class in the PCL
        effect = (TouchTracking.TouchEffect)Element.Effects.
                    FirstOrDefault(e => e is TouchTracking.TouchEffect);

        if (effect != null && frameworkElement != null)
        {
            // Save the method to call on touch events
            onTouchAction = effect.OnTouchAction;

            // Set event handlers on FrameworkElement
            frameworkElement.PointerEntered += OnPointerEntered;
            frameworkElement.PointerPressed += OnPointerPressed;
            frameworkElement.PointerMoved += OnPointerMoved;
            frameworkElement.PointerReleased += OnPointerReleased;
            frameworkElement.PointerExited += OnPointerExited;
            frameworkElement.PointerCanceled += OnPointerCancelled;
        }
    }
    ...
}    
```

`OnPointerPressed` Obslužná rutina vyvolá událost vliv voláním `onTouchAction` pole `CommonHandler` metoda:

```csharp
public class TouchEffect : PlatformEffect
{
    ...
    void OnPointerPressed(object sender, PointerRoutedEventArgs args)
    {
        CommonHandler(sender, TouchActionType.Pressed, args);

        // Check setting of Capture property
        if (effect.Capture)
        {
            (sender as FrameworkElement).CapturePointer(args.Pointer);
        }
    }
    ...
    void CommonHandler(object sender, TouchActionType touchActionType, PointerRoutedEventArgs args)
    {
        PointerPoint pointerPoint = args.GetCurrentPoint(sender as UIElement);
        Windows.Foundation.Point windowsPoint = pointerPoint.Position;  

        onTouchAction(Element, new TouchActionEventArgs(args.Pointer.PointerId,
                                                        touchActionType,
                                                        new Point(windowsPoint.X, windowsPoint.Y),
                                                        args.Pointer.IsInContact));
    }
}
```

`OnPointerPressed` také zkontroluje hodnotu `Capture` vlastnost ve třídě vliv v PCL a volání `CapturePointer` Pokud je `true`.

 Další UWP obslužné rutiny událostí jsou i jednodušší:

```csharp
public class TouchEffect : PlatformEffect
{
    ...
    void OnPointerEntered(object sender, PointerRoutedEventArgs args)
    {
        CommonHandler(sender, TouchActionType.Entered, args);
    }
    ...
}
```

### <a name="the-android-implementation"></a>Android implementace

Implementace Android a iOS se nutně složitější, protože se musí implementovat `Exited` a `Entered` událostí v případě, že prstem přesune z jednoho prvku na jiný. Obě implementace jsou strukturovaná podobně jako.

Android `TouchEffect` třída nainstaluje obslužnou rutinu pro `Touch` událostí:

```csharp
view = Control == null ? Container : Control;
...
view.Touch += OnTouch;
```

Třída definuje také dva statické slovník:

```csharp
public class TouchEffect : PlatformEffect
{
    ...
    static Dictionary<Android.Views.View, TouchEffect> viewDictionary =
        new Dictionary<Android.Views.View, TouchEffect>();

    static Dictionary<int, TouchEffect> idToEffectDictionary =
        new Dictionary<int, TouchEffect>();
    ...
```

`viewDictionary` Získá nový záznam pokaždé, když `OnAttached` přepsání se označuje jako:

```csharp
viewDictionary.Add(view, this);
```

Položka je odebrána ze slovníku v `OnDetached`. Všechny instance řetězce `TouchEffect` je přidružen ke konkrétní zobrazení, který účinek je připojen k. Statické slovníku umožňuje žádné `TouchEffect` instance provedení výčtu všechna zobrazení a jejich odpovídajících `TouchEffect` instance. To je nezbytné, aby bylo možné přenosu události z jednoho zobrazení.

Android přiřadí ID kód k touch události, který umožňuje sledovat jednotlivé prsty aplikaci. `idToEffectDictionary` Přidruží tento kód ID s `TouchEffect` instance. Přidat položku do tohoto slovníku při `Touch` obslužná rutina volat stiskněte prstu:

```csharp
void OnTouch(object sender, Android.Views.View.TouchEventArgs args)
{
    ...
    switch (args.Event.ActionMasked)
    {
        case MotionEventActions.Down:
        case MotionEventActions.PointerDown:
            FireEvent(this, id, TouchActionType.Pressed, screenPointerCoords, true);

            idToEffectDictionary.Add(id, this);

            capture = pclTouchEffect.Capture;
            break;

```

Položka je odebrána ze `idToEffectDictionary` uvolnění prstu na obrazovce. `FireEvent` Metoda jednoduše shromažďuje všechny informace potřebné k volání `OnTouchAction` metoda:

```csharp
void FireEvent(TouchEffect touchEffect, int id, TouchActionType actionType, Point pointerLocation, bool isInContact)
{
    // Get the method to call for firing events
    Action<Element, TouchActionEventArgs> onTouchAction = touchEffect.pclTouchEffect.OnTouchAction;

    // Get the location of the pointer within the view
    touchEffect.view.GetLocationOnScreen(twoIntArray);
    double x = pointerLocation.X - twoIntArray[0];
    double y = pointerLocation.Y - twoIntArray[1];
    Point point = new Point(fromPixels(x), fromPixels(y));

    // Call the method
    onTouchAction(touchEffect.formsElement,
        new TouchActionEventArgs(id, actionType, point, isInContact));
}
```

Všechny ostatní typy dotykového ovládání, které jsou zpracovány v dvěma různými způsoby: Pokud `Capture` vlastnost je `true`, událost touch je docela jednoduché překlad, aby `TouchEffect` informace. Získá složitější při `Capture` je `false` protože události touch muset na jiný přesunout z jednoho zobrazení. To má na starosti `CheckForBoundaryHop` metodu, která je volána v průběhu přesunutí události. Tato metoda využívá obou statické slovník. Výčtu prostřednictvím `viewDictionary` k určení zobrazení, který je aktuálně dotykové ovládání prstu, a používá `idToEffectDictionary` k uložení aktuálního `TouchEffect` instance (a proto aktuální zobrazení) spojené s konkrétní ID:

```csharp
void CheckForBoundaryHop(int id, Point pointerLocation)
{
    TouchEffect touchEffectHit = null;

    foreach (Android.Views.View view in viewDictionary.Keys)
    {
        // Get the view rectangle
        try
        {
            view.GetLocationOnScreen(twoIntArray);
        }
        catch // System.ObjectDisposedException: Cannot access a disposed object.
        {
            continue;
        }
        Rectangle viewRect = new Rectangle(twoIntArray[0], twoIntArray[1], view.Width, view.Height);

        if (viewRect.Contains(pointerLocation))
        {
            touchEffectHit = viewDictionary[view];
        }
    }

    if (touchEffectHit != idToEffectDictionary[id])
    {
        if (idToEffectDictionary[id] != null)
        {
            FireEvent(idToEffectDictionary[id], id, TouchActionType.Exited, pointerLocation, true);
        }
        if (touchEffectHit != null)
        {
            FireEvent(touchEffectHit, id, TouchActionType.Entered, pointerLocation, true);
        }
        idToEffectDictionary[id] = touchEffectHit;
    }
}
```

Pokud došlo ke změně `idToEffectionDictionary`, potenciálně volá metodu `FireEvent` pro `Exited` a `Entered` přenos z jednoho zobrazení do jiného. Ale prstu může mít byl přesunut do oblasti obsazena zobrazení bez připojené `TouchEffect`, nebo z této oblasti k zobrazení s účinek připojen.

Upozornění `try` a `catch` blokovat při přístupu k zobrazení. Na stránce, která je přešli, a pak přejde zpět na domovskou stránku `OnDetached` metoda není volána a položky zůstanou v `viewDictionary` , ale je zrušen považuje za Android.

### <a name="the-ios-implementation"></a>IOS implementace

Implementace iOS je podobná Android implementace vyjma toho, že iOS `TouchEffect` třída musí vytvořit instanci odvozený ze `UIGestureRecognizer`. Toto je třída v projektu iOS s názvem `TouchRecognizer`. Tato třída udržuje dvě statické slovníky, které ukládají `TouchRecognizer` instancí:

```csharp
static Dictionary<UIView, TouchRecognizer> viewDictionary =
    new Dictionary<UIView, TouchRecognizer>();

static Dictionary<long, TouchRecognizer> idToTouchDictionary =
    new Dictionary<long, TouchRecognizer>();
```

Velká část strukturu tohoto `TouchRecognizer` třídy je podobná Android `TouchEffect` třídy.

## <a name="putting-the-touch-effect-to-work"></a>Uvedení Touch vliv na práci

[ **TouchTrackingEffectDemos** ](https://developer.xamarin.com/samples/xamarin-forms/effects/TouchTrackingEffectDemos/) program obsahuje pět stránek, které testování účinků sledování dotykového ovládání pro běžné úlohy.

**BoxView přetahování** stránky umožňuje přidat `BoxView` elementy, aby `AbsoluteLayout` a přetáhněte je na obrazovce. [Souboru XAML](https://github.com/xamarin/xamarin-forms-samples/blob/master/Effects/TouchTrackingEffectDemos/TouchTrackingEffectDemos/TouchTrackingEffectDemos/BoxViewDraggingPage.xaml) vytvoří dvě instance `Button` zobrazení pro přidání `BoxView` elementů `AbsoluteLayout` a vymazání `AbsoluteLayout`.

Metodu v [souboru kódu na pozadí](https://github.com/xamarin/xamarin-forms-samples/blob/master/Effects/TouchTrackingEffectDemos/TouchTrackingEffectDemos/TouchTrackingEffectDemos/BoxViewDraggingPage.xaml.cs) , přidá nový `BoxView` k `AbsoluteLayout` také přidá `TouchEffect` do objektu `BoxView` a připojí obslužné rutiny události v tom smyslu:

```csharp
void AddBoxViewToLayout()
{
    BoxView boxView = new BoxView
    {
        WidthRequest = 100,
        HeightRequest = 100,
        Color = new Color(random.NextDouble(),
                          random.NextDouble(),
                          random.NextDouble())
    };

    TouchEffect touchEffect = new TouchEffect();
    touchEffect.TouchAction += OnTouchEffectAction;
    boxView.Effects.Add(touchEffect);
    absoluteLayout.Children.Add(boxView);
}
```

`TouchAction` Obslužné rutiny události zpracovává všechny události dotykového ovládání pro všechny `BoxView` elementů, ale je potřeba některé postupujte opatrně: dvěma prsty ho nemůže povolit na jednom `BoxView` protože programu pouze implementuje přetahování a dvěma prsty by narušovat navzájem. Z tohoto důvodu stránce definuje třídu embedded pro každý prstem aktuálně sledované:

```csharp
class DragInfo
{
    public DragInfo(long id, Point pressPoint)
    {
        Id = id;
        PressPoint = pressPoint;
    }

    public long Id { private set; get; }

    public Point PressPoint { private set; get; }
}

Dictionary<BoxView, DragInfo> dragDictionary = new Dictionary<BoxView, DragInfo>();
```

`dragDictionary` Obsahuje položku pro každou `BoxView` aktuálně přetažením.

`Pressed` Touch akce přidá položku do tohoto slovníku a `Released` akce odebere ho. `Pressed` Logiku musí zkontrolujte, jestli už položku ve slovníku pro tento `BoxView`. Pokud ano, `BoxView` je již přetažen a novou událost je druhý finger na který stejné `BoxView`. Pro `Moved` a `Released` akce, obslužné rutiny události musí kontrola, zda slovník má záznam pro tento `BoxView` a že touch `Id` vlastnost, pro který přetáhnout `BoxView` odpovídá položky dictionary:

```csharp
void OnTouchEffectAction(object sender, TouchActionEventArgs args)
{
    BoxView boxView = sender as BoxView;

    switch (args.Type)
    {
        case TouchActionType.Pressed:
            // Don't allow a second touch on an already touched BoxView
            if (!dragDictionary.ContainsKey(boxView))
            {
                dragDictionary.Add(boxView, new DragInfo(args.Id, args.Location));

                // Set Capture property to true
                TouchEffect touchEffect = (TouchEffect)boxView.Effects.FirstOrDefault(e => e is TouchEffect);
                touchEffect.Capture = true;
            }
            break;

        case TouchActionType.Moved:
            if (dragDictionary.ContainsKey(boxView) && dragDictionary[boxView].Id == args.Id)
            {
                Rectangle rect = AbsoluteLayout.GetLayoutBounds(boxView);
                Point initialLocation = dragDictionary[boxView].PressPoint;
                rect.X += args.Location.X - initialLocation.X;
                rect.Y += args.Location.Y - initialLocation.Y;
                AbsoluteLayout.SetLayoutBounds(boxView, rect);
            }
            break;

        case TouchActionType.Released:
            if (dragDictionary.ContainsKey(boxView) && dragDictionary[boxView].Id == args.Id)
            {
                dragDictionary.Remove(boxView);
            }
            break;
    }
}
```

`Pressed` Logiky sad `Capture` vlastnost `TouchEffect` do objektu `true`. To má za následek doručování všechny další události pro tento prstem stejné obslužné rutiny události.

`Moved` Logiku přesune `BoxView` změnou `LayoutBounds` přidružená vlastnost. `Location` Vlastnost argumenty událostí je vždy vzhledem k `BoxView` právě přetáhli a pokud `BoxView` je právě přetáhli konstantní rychlostí, `Location` vlastnosti po sobě jdoucích událostí, které budou přibližně stejné. Například, pokud stiskem tlačítka prstem `BoxView` centra, `Pressed` akce úložiště `PressPoint` vlastnost (50, 50), který zůstává stejná pro následných událostech. Pokud `BoxView` je během konstantní sazby následné šikmo přetažen `Location` vlastnosti během `Moved` akce může být hodnotách (55, 55), v takovém případě `Moved` logiku přidá 5 vodorovného a svislého pozici `BoxView`. Tento krok přesune `BoxView` tak, aby jeho center znovu přímo pod prstu.

Můžete přesunout více `BoxView` elementů současně pomocí různých prstů.

[![](touch-tracking-images/boxviewdragging-small.png "Trojitá snímek obrazovky stránky přetahování BoxView")](touch-tracking-images/boxviewdragging-large.png#lightbox "Trojitá snímek obrazovky stránky BoxView přetahování")

### <a name="subclassing-the-view"></a>Vytvoření podtřídy zobrazení

Často je snazší pro element Xamarin.Forms pro zpracování vlastních událostí dotykového ovládání. **Přetahovatelným přetahování BoxView** stránky funguje stejně jako **BoxView přetahování** stránky, ale prvky, které jsou instancemi drags uživatele [ `DraggableBoxView` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/Effects/TouchTrackingEffectDemos/TouchTrackingEffectDemos/TouchTrackingEffectDemos/DraggableBoxView.cs) Třída odvozená z `BoxView`:

```csharp
class DraggableBoxView : BoxView
{
    bool isBeingDragged;
    long touchId;
    Point pressPoint;

    public DraggableBoxView()
    {
        TouchEffect touchEffect = new TouchEffect
        {
            Capture = true
        };
        touchEffect.TouchAction += OnTouchEffectAction;
        Effects.Add(touchEffect);
    }

    void OnTouchEffectAction(object sender, TouchActionEventArgs args)
    {
        switch (args.Type)
        {
            case TouchActionType.Pressed:
                if (!isBeingDragged)
                {
                    isBeingDragged = true;
                    touchId = args.Id;
                    pressPoint = args.Location;
                }
                break;

            case TouchActionType.Moved:
                if (isBeingDragged && touchId == args.Id)
                {
                    TranslationX += args.Location.X - pressPoint.X;
                    TranslationY += args.Location.Y - pressPoint.Y;
                }
                break;

            case TouchActionType.Released:
                if (isBeingDragged && touchId == args.Id)
                {
                    isBeingDragged = false;
                }
                break;
        }
    }
}
```

V konstruktoru vytvoří a připojí `TouchEffect`a nastaví `Capture` při prvním vytvoření instance objektu. Vlastní třídy ukládá není nutná žádná slovník `isBeingDragged`, `pressPoint`, a `touchId` hodnotami spojené s každou prstu. `Moved` Mění zpracování `TranslationX` a `TranslationY` vlastností pro logiku bude fungovat i v případě nadřazeného `DraggableBoxView` není `AbsoluteLayout`.

### <a name="integrating-with-skiasharp"></a>Integrace s SkiaSharp

Následující dva předvádění vyžadují grafiky a používají SkiaSharp pro tento účel. Můžete chtít Další informace o [pomocí SkiaSharp v Xamarin.Forms](~/xamarin-forms/user-interface/graphics/skiasharp/index.md) před, abyste si prostudovali tyto příklady. První dva články ("SkiaSharp kreslení základy" a "SkiaSharp řádky a cesty") zahrnují vše, co budete potřebovat sem.

**Kreslení elipsy** stránka umožňuje Nakreslení elipsy potažením prstem na obrazovce. Podle toho, jak přesunout prstu, můžete se třemi tečkami kreslení z levé horní do dolní nebo z jiných rohu do opačné rohu. Se třemi tečkami vykreslením s náhodných barvy a krytí.

[![](touch-tracking-images/ellipsedrawing-small.png "Trojitá snímek obrazovky stránky kreslení elipsy")](touch-tracking-images/ellipsedrawing-large.png#lightbox "Trojitá snímek obrazovky stránky elipsy kreslení")

Pokud jste pak touch mezi na symbol tří teček, můžete ji přetáhněte na jiné místo. To vyžaduje techniku známou jako "vstupů do testování," který zahrnuje hledání grafického objektu na určitém místě. Výpustky SkiaSharp nejsou Xamarin.Forms prvky, takže nemůžou provést vlastní `TouchEffect` zpracování. `TouchEffect` Se musí vztahovat na celou `SKCanvasView` objektu.

[EllipseDrawPage.xaml](https://github.com/xamarin/xamarin-forms-samples/blob/master/Effects/TouchTrackingEffectDemos/TouchTrackingEffectDemos/TouchTrackingEffectDemos/EllipseDrawingPage.xaml) soubor vytvoří `SKCanvasView` v jedné buňce `Grid`. `TouchEffect` Objekt je připojený k který `Grid`:

```xaml
<Grid x:Name="canvasViewGrid"
        Grid.Row="1"
        BackgroundColor="White">

    <skia:SKCanvasView x:Name="canvasView"
                        PaintSurface="OnCanvasViewPaintSurface" />
    <Grid.Effects>
        <tt:TouchEffect Capture="True"
                        TouchAction="OnTouchEffectAction" />
    </Grid.Effects>
</Grid>
```

Android a univerzální platformu Windows `TouchEffect` lze připojit přímo na `SKCanvasView`, ale na iOS, která nebude fungovat. Všimněte si, že `Capture` je nastavena na `true`.

Každý elipsy, který vykreslí SkiaSharp je reprezentována objekt typu `EllipseDrawingFigure`:

```csharp
class EllipseDrawingFigure
{
    SKPoint pt1, pt2;

    public EllipseDrawingFigure()
    {
    }

    public SKColor Color { set; get; }

    public SKPoint StartPoint
    {
        set
        {
            pt1 = value;
            MakeRectangle();
        }
    }

    public SKPoint EndPoint
    {
        set
        {
            pt2 = value;
            MakeRectangle();
        }
    }

    void MakeRectangle()
    {
        Rectangle = new SKRect(pt1.X, pt1.Y, pt2.X, pt2.Y).Standardized;
    }

    public SKRect Rectangle { set; get; }

    // For dragging operations
    public Point LastFingerLocation { set; get; }

    // For the dragging hit-test
    public bool IsInEllipse(SKPoint pt)
    {
        SKRect rect = Rectangle;

        return (Math.Pow(pt.X - rect.MidX, 2) / Math.Pow(rect.Width / 2, 2) +
                Math.Pow(pt.Y - rect.MidY, 2) / Math.Pow(rect.Height / 2, 2)) < 1;
    }
}
```

`StartPoint` a `EndPoint` vlastnosti se používají při programu je zpracování dotykové ovládání; `Rectangle` vlastnost se používá pro kreslení se třemi tečkami. `LastFingerLocation` Vlastnost stává play při přetažení se třemi tečkami a `IsInEllipse` metoda pomůcek při testování stiskněte klávesu. Vrátí metoda `true` Pokud se bod nachází uvnitř se třemi tečkami.

[Souboru kódu na pozadí](https://github.com/xamarin/xamarin-forms-samples/blob/master/Effects/TouchTrackingEffectDemos/TouchTrackingEffectDemos/TouchTrackingEffectDemos/EllipseDrawingPage.xaml.cs) udržuje tři kolekce:

```csharp
Dictionary<long, EllipseDrawingFigure> inProgressFigures = new Dictionary<long, EllipseDrawingFigure>();
List<EllipseDrawingFigure> completedFigures = new List<EllipseDrawingFigure>();
Dictionary<long, EllipseDrawingFigure> draggingFigures = new Dictionary<long, EllipseDrawingFigure>();
```

`draggingFigure` Slovník obsahuje podmnožinu `completedFigures` kolekce. SkiaSharp `PaintSurface` obslužné rutiny události jednoduše vykreslí objekty v těchto `completedFigures` a `inProgressFigures` kolekce:

```csharp
SKPaint paint = new SKPaint
{
    Style = SKPaintStyle.Fill
};
...
void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
{
    SKCanvas canvas = args.Surface.Canvas;
    canvas.Clear();

    foreach (EllipseDrawingFigure figure in completedFigures)
    {
        paint.Color = figure.Color;
        canvas.DrawOval(figure.Rectangle, paint);
    }
    foreach (EllipseDrawingFigure figure in inProgressFigures.Values)
    {
        paint.Color = figure.Color;
        canvas.DrawOval(figure.Rectangle, paint);
    }
}
```

Trickiest část zpracování touch je `Pressed` zpracování. To je, kdy vstupů do testování se provádí, ale pokud kód zjistí elipsy v části prstem uživatele, že elipsy můžete pouze přetáhnout Pokud je aktuálně přetažen jiné prstu. Pokud není žádná elipsy v části prstem uživatele, kód spustí proces vykreslování nové elipsy:

```csharp
case TouchActionType.Pressed:
    bool isDragOperation = false;

    // Loop through the completed figures
    foreach (EllipseDrawingFigure fig in completedFigures.Reverse<EllipseDrawingFigure>())
    {
        // Check if the finger is touching one of the ellipses
        if (fig.IsInEllipse(ConvertToPixel(args.Location)))
        {
            // Tentatively assume this is a dragging operation
            isDragOperation = true;

            // Loop through all the figures currently being dragged
            foreach (EllipseDrawingFigure draggedFigure in draggingFigures.Values)
            {
                // If there's a match, we'll need to dig deeper
                if (fig == draggedFigure)
                {
                    isDragOperation = false;
                    break;
                }
            }

            if (isDragOperation)
            {
                fig.LastFingerLocation = args.Location;
                draggingFigures.Add(args.Id, fig);
                break;
            }
        }
    }

    if (isDragOperation)
    {
        // Move the dragged ellipse to the end of completedFigures so it's drawn on top
        EllipseDrawingFigure fig = draggingFigures[args.Id];
        completedFigures.Remove(fig);
        completedFigures.Add(fig);
    }
    else // start making a new ellipse
    {
        // Random bytes for random color
        byte[] buffer = new byte[4];
        random.NextBytes(buffer);

        EllipseDrawingFigure figure = new EllipseDrawingFigure
        {
            Color = new SKColor(buffer[0], buffer[1], buffer[2], buffer[3]),
            StartPoint = ConvertToPixel(args.Location),
            EndPoint = ConvertToPixel(args.Location)
        };
        inProgressFigures.Add(args.Id, figure);
    }
    canvasView.InvalidateSurface();
    break;
```

Další příklad SkiaSharp **Malování prstem** stránky. Barva linky a šířku tahu můžete vybrat ze dvou `Picker` zobrazení a potom kreslení jeden nebo více prsty:

[![](touch-tracking-images/fingerpaint-small.png "Trojitá snímek obrazovky stránky Malování prstem")](touch-tracking-images/fingerpaint-large.png#lightbox "Trojitá snímek obrazovky stránky prstem Malování")

Tento příklad také vyžaduje samostatné třídy představující každý řádek vykresluje na obrazovce:

```csharp
class FingerPaintPolyline
{
    public FingerPaintPolyline()
    {
        Path = new SKPath();
    }

    public SKPath Path { set; get; }

    public Color StrokeColor { set; get; }

    public float StrokeWidth { set; get; }
}
```

`SKPath` Objektu slouží k vykreslení každého řádku. [FingerPaint.xaml.cs](https://github.com/xamarin/xamarin-forms-samples/blob/master/Effects/TouchTrackingEffectDemos/TouchTrackingEffectDemos/TouchTrackingEffectDemos/FingerPaintPage.xaml.cs) soubor udržuje dvě kolekce tyto objekty, jednu pro tyto čáru lomených aktuálně přitahuje a druhou pro dokončené čáru lomených:

```csharp
Dictionary<long, FingerPaintPolyline> inProgressPolylines = new Dictionary<long, FingerPaintPolyline>();
List<FingerPaintPolyline> completedPolylines = new List<FingerPaintPolyline>();
```

`Pressed` Zpracování vytvoří novou `FingerPaintPolyline`, volání `MoveTo` na objekt cesta pro uložení počáteční bod a přidá tento objekt, který chcete `inProgressPolylines` slovníku. `Moved` Zpracování volání `LineTo` u objektu cestu k nové pozici prstem a `Released` zpracování přenáší dokončené lomenou čáru z `inProgressPolylines` k `completedPolylines`. Znovu je poměrně jednoduché skutečné SkiaSharp kreslení kódu:

```csharp
SKPaint paint = new SKPaint
{
    Style = SKPaintStyle.Stroke,
    StrokeCap = SKStrokeCap.Round,
    StrokeJoin = SKStrokeJoin.Round
};
...
void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
{
    SKCanvas canvas = args.Surface.Canvas;
    canvas.Clear();

    foreach (FingerPaintPolyline polyline in completedPolylines)
    {
        paint.Color = polyline.StrokeColor.ToSKColor();
        paint.StrokeWidth = polyline.StrokeWidth;
        canvas.DrawPath(polyline.Path, paint);
    }

    foreach (FingerPaintPolyline polyline in inProgressPolylines.Values)
    {
        paint.Color = polyline.StrokeColor.ToSKColor();
        paint.StrokeWidth = polyline.StrokeWidth;
        canvas.DrawPath(polyline.Path, paint);
    }
}
```

### <a name="tracking-view-to-view-touch"></a>Sledování Touch zobrazení – zobrazení

Nastavili v předchozích příkladech `Capture` vlastnost `TouchEffect` k `true`, buď když `TouchEffect` byl vytvořen nebo když `Pressed` k události došlo. Tím se zajistí, že stejného elementu přijímá všechny události související s prstu, která nejprve stisknuta zobrazení. Konečný vzorek nemá *není* nastavit `Capture` k `true`. To způsobí, že různé chování v případě prstem v kontaktu s obrazovky přechází z jednoho prvku na jiný. Elementu, který prstu přesune z přijímá událost `Type` vlastnost nastavena na hodnotu `TouchActionType.Exited` a druhý prvkem přijímá událost `Type` nastavení `TouchActionType.Entered`.

Tento typ zpracování touch je velmi užitečná pro Hudba klávesnice. Klíč by mohli zjistit, kdy ji stisknutí, ale i když prstem snímky z jednoho klíče na jiný.

**Tichou klávesnice** stránka definuje malého [ `WhiteKey` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/Effects/TouchTrackingEffectDemos/TouchTrackingEffectDemos/TouchTrackingEffectDemos/WhiteKey.cs) a [ `BlackKey` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/Effects/TouchTrackingEffectDemos/TouchTrackingEffectDemos/TouchTrackingEffectDemos/BlackKey.cs) třídy, které jsou odvozeny od [ `Key` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/Effects/TouchTrackingEffectDemos/TouchTrackingEffectDemos/TouchTrackingEffectDemos/Key.cs), která je odvozena z `BoxView`.

`Key` Třída je připravený k použití v programu pro skutečné Hudba. Definuje veřejné vlastnosti s názvem `IsPressed` a `KeyNumber`, který má být nastavena na kód vymezenému MIDI standard. `Key` Třída také definuje událost s názvem `StatusChanged`, které je voláno, když `IsPressed` změny vlastností.

Více prsty, které jsou povoleny na každý klíč. Z tohoto důvodu `Key` třída udržuje `List` touch ID čísel všech prstů aktuálně dotykové ovládání klíči:

```csharp
List<long> ids = new List<long>();
```

`TouchAction` Obslužné rutiny události přidá ID, které má `ids` seznam pro obě `Pressed` typ události a `Entered` typ, ale pouze tehdy, když `IsInContact` vlastnost je `true` pro `Entered` událostí. ID je odebrán z `List` pro `Released` nebo `Exited` událostí:

```csharp
void OnTouchEffectAction(object sender, TouchActionEventArgs args)
{
    switch (args.Type)
    {
      case TouchActionType.Pressed:
          AddToList(args.Id);
          break;

        case TouchActionType.Entered:
            if (args.IsInContact)
            {
                AddToList(args.Id);
            }
            break;

        case TouchActionType.Moved:
            break;

        case TouchActionType.Released:
        case TouchActionType.Exited:
            RemoveFromList(args.Id);
            break;
    }
}

```

`AddToList` a `RemoveFromList` obě metody zkontrolujte, zda `List` došlo ke změně mezi prázdný a není prázdný a pokud ano, vyvolá `StatusChanged` událostí.

Různé `WhiteKey` a `BlackKey` jsou uspořádány elementy na stránce [souboru XAML](https://github.com/xamarin/xamarin-forms-samples/blob/master/Effects/TouchTrackingEffectDemos/TouchTrackingEffectDemos/TouchTrackingEffectDemos/SilentKeyboardPage.xaml), která vypadá nejlepší telefonu se nachází v režimu na šířku:

[![](touch-tracking-images/silentkeyboard-small.png "Trojitá snímek obrazovky stránky tichou klávesnice")](touch-tracking-images/silentkeyboard-large.png#lightbox "Trojitá snímek obrazovky stránky tichou klávesnice")

Pokud jste oblouku prstu napříč klíče, se zobrazí mírné změny v barvu, události touch přenášených z jednoho klíče na jiný.

## <a name="summary"></a>Souhrn

Tento článek vám ukázal, jak vyvolat události v vliv a zápis a používání efektu, který implementuje nízké úrovně zpracování s více touch.


## <a name="related-links"></a>Související odkazy

- [Více Touch prstem sledování v iOS](~/ios/app-fundamentals/touch/touch-tracking.md)
- [Sledování v Android prstem více dotykového ovládání](~/android/app-fundamentals/touch/touch-tracking.md)
- [Touch sledování účinku (ukázka)](https://developer.xamarin.com/samples/xamarin-forms/effects/TouchTrackingEffectDemos/)
