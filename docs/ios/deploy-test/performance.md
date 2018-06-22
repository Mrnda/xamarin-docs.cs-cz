---
title: Výkon Xamarin.iOS
description: Tento dokument popisuje techniky, které lze použít ke zlepšení výkonu a využití paměti v aplikacích Xamarin.iOS.
ms.prod: xamarin
ms.assetid: 02b1f628-52d9-49de-8479-f2696546ca3f
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 01/29/2016
ms.openlocfilehash: afff9d3924c673edc363292efa1a9b7df43a9218
ms.sourcegitcommit: e16517edcf471b53b4e347cd3fd82e485923d482
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 05/07/2018
ms.locfileid: "33798587"
---
# <a name="xamarinios-performance"></a>Výkon Xamarin.iOS

Výkon nízký aplikace prezentuje mnoha způsoby. Aplikace může díky pravděpodobně reagovat, může způsobit pomalé posouvání a může snížit z baterie. Ale optimalizace výkonu zahrnuje více než jen implementace efektivní kódu. Musíte také zvážit možnosti pro uživatele s výkonem aplikace. Například zajistíte, že operace spustit bez blokování uživatele z jiné aktivity vám může pomoct vylepšit možnosti pro uživatele. 

Tento dokument popisuje techniky, které lze použít ke zlepšení výkonu a využití paměti v aplikacích Xamarin.iOS.

> [!NOTE]
> Před přečtení tohoto článku měli nejdřív přečíst [napříč platformami výkonu](~/cross-platform/deploy-test/memory-perf-best-practices.md), který popisuje konkrétní techniky jiné platformy ke zlepšení využití paměti a výkon aplikace vytvořené pomocí platformy Xamarin.

## <a name="avoid-strong-circular-references"></a>Vyhněte se silné cyklické odkazy

V některých případech je možné vytvořit odkaz na silné cykly, které mohou bránit s jejich paměti uvolnit systémem uvolňování objektů. Představte si třeba případě kde [ `NSObject` ](https://developer.xamarin.com/api/type/Foundation.NSObject/)-odvozené podtřídami, jako jsou třídy, která dědí z [ `UIView` ](https://developer.xamarin.com/api/type/UIKit.UIView/), se přidá do `NSObject`-odvozené kontejneru a je důrazně na něj odkazovat z jazyka Objective-C, jak je znázorněno v následujícím příkladu kódu:

```csharp
class Container : UIView
{
    public void Poke ()
    {
    // Call this method to poke this object
    }
}

class MyView : UIView
{
    Container parent;
    public MyView (Container parent)
    {
        this.parent = parent;
    }

    void PokeParent ()
    {
        parent.Poke ();
    }
}

var container = new Container ();
container.AddSubview (new MyView (container));
```

Když tento kód vytvoří `Container` instance jazyka C# objektu bude mít silné odkaz na objekt jazyka Objective-C. Podobně `MyView` instance bude mít i silné odkaz na objekt jazyka Objective-C.

Kromě toho volání `container.AddSubview` zvýší počet odkazů na nespravovanou `MyView` instance. Pokud k tomu dojde, modul runtime Xamarin.iOS vytvoří `GCHandle` instance zachovat `MyView` objekt ve spravovaném kódu zachování připojení, protože není zaručeno, že všechny spravované objekty ponechá na něj odkaz. Z hlediska spravovaného kódu `MyView` objekt by uvolnit po [ `AddSubview` ](https://developer.xamarin.com/api/member/UIKit.UIView.AddSubview/p/UIKit.UIView/) volání, kdyby nebyla pro `GCHandle`.

Nespravovanou `MyView` objekt bude mít `GCHandle` odkazující na spravovaný objekt, označuje jako *silné odkaz*. Spravovaného objektu bude obsahovat odkaz na `Container` instance. Naopak `Container` instance bude mít spravovaný odkaz na `MyView` objektu.

V případech, kde obsažený objekt udržuje odkaz k jejímu kontejneru máte několik možností, které jsou k dispozici jak nakládat s cyklický odkaz:

-  Ručně rozdělit do cyklu nastavením odkaz na kontejner, aby `null`.
-  Ručně odeberte obsažený objekt z kontejneru.
-  Volání `Dispose` na objektech.
-  Vyhněte se udržuje slabé odkaz na kontejner cyklický odkaz. Další informace o slabé odkazy.

### <a name="using-weakreferences"></a>Pomocí WeakReferences

Jedním ze způsobů, aby se zabránilo cyklus je použití z podřízených slabé odkaz na nadřazený. Ve výše uvedeném kódu může být například zapsat takto:

```csharp
class Container : UIView
{
    public void Poke ()
    {
        // Call this method to poke this object
    }
}

class MyView : UIView
{
    WeakReference<Container> weakParent;
    public MyView (Container parent)
    {
        this.weakParent = new WeakReference<Container> (parent);
    }

    void PokeParent ()
    {
        if (weakParent.TryGetTarget (out var parent))
            parent.Poke ();
    }
}

var container = new Container ();
container.AddSubview (new MyView (container));
```

Zde obsažený objekt nebude zachování nadřazené připojení. Ale nadřazený zachová podřízená aktivní prostřednictvím volání Hotovo `container.AddSubView`.

To také probíhá iOS rozhraní API, které používají vzorec zdroj delegáta nebo data, kde třídu sdílené obsahuje implementaci; například při nastavení [ `Delegate` ](https://developer.xamarin.com/api/property/MonoTouch.UIKit.UITableView.Delegate/) vlastnost nebo [ `DataSource` ](https://developer.xamarin.com/api/property/MonoTouch.UIKit.UITableView.DataSource/) v [ `UITableView` ](https://developer.xamarin.com/api/type/UIKit.UITableView/) třídy.

V případě třídy, které jsou vytvořené výhradně z důvodu implementace protokolu, například [ `IUITableViewDataSource` ](https://developer.xamarin.com/api/type/MonoTouch.UIKit.IUITableViewDataSource/), jak je místo vytvoření podtřídy, právě můžete implementovat rozhraní ve třídě a přepsat Metoda a přiřadit `DataSource` vlastnost `this`.

#### <a name="weak-attribute"></a>Slabé atribut

[Xamarin.iOS 11.10](https://developer.xamarin.com/releases/ios/xamarin.ios_11/xamarin.ios_11.10/#WeakAttribute) zavedená `[Weak]` atribut. Jako `WeakReference <T>`, `[Weak]` lze rozdělit [silné cyklické odkazy](https://docs.microsoft.com/en-us/xamarin/ios/deploy-test/performance#avoid-strong-circular-references), ale s i méně kódem.

Vezměte v úvahu následující kód, který používá `WeakReference <T>`:

```csharp
public class MyFooDelegate : FooDelegate {
    WeakReference<MyViewController> controller;
    public MyFooDelegate (MyViewController ctrl) => controller = new WeakReference<MyViewController> (ctrl);
    public void CallDoSomething ()
    {
        MyViewController ctrl;
        if (controller.TryGetTarget (out ctrl)) {
            ctrl.DoSomething ();
        }
    }
}
```

Ekvivalentní kódu pomocí `[Weak]` je mnohem přesnější:

```csharp
public class MyFooDelegate : FooDelegate {
    [Weak] MyViewController controller;
    public MyFooDelegate (MyViewController ctrl) => controller = ctrl;
    public void CallDoSomething () => controller.DoSomething ();
}
```

Tady je další příklad použití `[Weak]` v kontextu [delegování](https://developer.apple.com/library/content/documentation/General/Conceptual/DevPedia-CocoaCore/Delegation.html) vzoru:

```csharp
public class MyViewController : UIViewController 
{
    WKWebView webView;

    protected MyViewController (IntPtr handle) : base (handle) { }

    public override void ViewDidLoad ()
    {
        base.ViewDidLoad ();
        webView = new WKWebView (View.Bounds, new WKWebViewConfiguration ());
        webView.UIDelegate = new UIDelegate (this);
        View.AddSubview (webView);
    }
}

public class UIDelegate : WKUIDelegate 
{
    [Weak] MyViewController controller;

    public UIDelegate (MyViewController ctrl) => controller = ctrl;

    public override void RunJavaScriptAlertPanel (WKWebView webView, string message, WKFrameInfo frame, Action completionHandler)
    {
        var msg = $"Hello from: {controller.Title}";
        var alertController = UIAlertController.Create (null, msg, UIAlertControllerStyle.Alert);
        alertController.AddAction (UIAlertAction.Create ("Ok", UIAlertActionStyle.Default, null));
        controller.PresentViewController (alertController, true, null);
        completionHandler ();
    }
}
```

### <a name="disposing-of-objects-with-strong-references"></a>Uvolnění objektů se silným odkazy

Pokud silné odkaz existuje a je obtížné odeberte závislost, ujistěte se, `Dispose` metoda vymazat nadřazené ukazatele.

Pro kontejnery, přepíší `Dispose` metoda odebrat obsažené objekty, jak je znázorněno v následujícím příkladu kódu:

```csharp
class MyContainer : UIView
{
    public override void Dispose ()
    {
        // Brute force, remove everything
        foreach (var view in Subviews)
        {
              view.RemoveFromSuperview ();
        }
        base.Dispose ();
    }
}
```

Podřízený objekt, který uchovává silné odkaz na nadřazený, zrušte zaškrtnutí odkaz na nadřazený objekt v `Dispose` implementace:

```csharp
class MyChild : UIView 
{
    MyContainer container;
    public MyChild (MyContainer container)
    {
        this.container = container;
    }
    public override void Dispose ()
    {
        container = null;
    }
}
```

Další informace o uvolnění silné odkazů najdete v tématu [verzi rozhraní IDisposable prostředky](~/cross-platform/deploy-test/memory-perf-best-practices.md#idisposable).
Je také Zajímavá diskuse v tomto příspěvku na blogu: [Xamarin.iOS, uvolňování paměti a mi](http://krumelur.me/2015/04/27/xamarin-ios-the-garbage-collector-and-me/).

### <a name="more-information"></a>Další informace

Další informace najdete v tématu [pravidla, která vyhnout zachovat cykly](http://www.cocoawithlove.com/2009/07/rules-to-avoid-retain-cycles.html) na kakao s láska [jedná se o chybu v MonoTouch GC](http://stackoverflow.com/questions/13058521/is-this-a-bug-in-monotouch-gc) na StackOverflow, a [Proč nelze MonoTouch GC kill spravovaných objektů s refcount > 1? ](http://stackoverflow.com/questions/13064669/why-cant-monotouch-gc-kill-managed-objects-with-refcount-1) na StackOverflow.

## <a name="optimize-table-views"></a>Optimalizace zobrazení tabulek

Uživatelé očekávají, že plynulé posouvání a rychlé zatížení dobu [ `UITableView` ](https://developer.xamarin.com/api/type/UIKit.UITableView/) instance. Ale posouvání výkonu může dojít při buňky obsahují hluboko vložené zobrazení hierarchie, nebo když obsahovat komplexní rozložení buněk. Existují však technik, které umožňuje vyhnout nízká `UITableView` výkonu:

- Znovu použít buněk. Další informace najdete v tématu [znovu použít buněk](#reusecells).
- Snižte počet dílčích zobrazení.
- Obsah buňky mezipaměti, které se načítají z webové služby.
- Výška žádné řádky mezipaměti, pokud nejsou identické.
- Zkontrolujte neprůhledného buňky a dalšími zobrazeními.
- Vyhněte se škálování bitové kopie a přechody.

Kolektivně tyto postupy pomohou zachovat [ `UITableView` ](https://developer.xamarin.com/api/type/UIKit.UITableView/) instance posouvání bez problémů.

### <a name="reuse-cells"></a>Opakované použití buněk

Při zobrazení stovky řádků v [ `UITableView` ](https://developer.xamarin.com/api/type/UIKit.UITableView/), bylo by plýtvání paměti k vytvoření stovky [ `UITableViewCell` ](https://developer.xamarin.com/api/type/UIKit.UITableViewCell/) objekty při pouze malý počet je jsou zobrazené na obrazovce najednou. Místo toho pouze v buňkách viditelný na obrazovce, může být načtena do paměti, se **obsah** načítá do těchto buněk znovu použít. Tím se zabrání instance stovky další objekty, ukládání času a paměti.

Proto když buňku zmizí z obrazovky jeho zobrazení lze umístit do fronty pro opakované použití, jak je znázorněno v následujícím příkladu kódu:

```csharp
class MyTableSource : UITableViewSource
{
    public override UITableViewCell GetCell (UITableView tableView, NSIndexPath indexPath)
    {
        // iOS will create a cell automatically if one isn't available in the reuse pool
        var cell = (MyCell) tableView.DequeueReusableCell (MyCellId, indexPath);

        // Perform required cell actions
        return cell;
    }
}
```

Jako uživatel posune, [ `UITableView` ](https://developer.xamarin.com/api/type/UIKit.UITableView/) volání `GetCell` přepsání požádat o nový pohledů pro zobrazení. Toto přepsání pak volání [ `DequeueReusableCell` ](https://developer.xamarin.com/api/member/UIKit.UITableView.DequeueReusableCell/p/Foundation.NSString/) metoda a pokud je k dispozici pro opakované použití, bude vrácen buňku.

Další informace najdete v tématu [buňky opakovaně](~/ios/user-interface/controls/tables/populating-a-table-with-data.md) v [naplnění tabulku s daty](~/ios/user-interface/controls/tables/populating-a-table-with-data.md).

## <a name="use-opaque-views"></a>Použít neprůhledný zobrazení

Zajistěte, aby měly všechny zobrazení, která mají žádné průhlednost definované jejich [ `Opaque` ](https://developer.xamarin.com/api/property/UIKit.UIView.Opaque/) sadu vlastností. Tím bude zajištěno optimálně vykreslení zobrazení kreslení systémem. To je zvlášť důležité při zobrazení je vložen do [ `UIScrollView` ](https://developer.xamarin.com/api/type/UIKit.UIScrollView/), nebo je součástí komplexní animace. V opačném případě kreslení systém bude složené zobrazení s další obsah, který může výrazně ovlivnit výkon.

## <a name="avoid-fat-xibs"></a>Vyhněte se systémem souborů fat XIBs

I když XIBs z velké části byly nahrazeny scénářů, existují určitých okolností kterém může XIBs pořád použít. Při XIB je načten do paměti, veškerý její obsah se do paměti bylo načteno, včetně všechny Image. Pokud XIB obsahuje zobrazení, které není ihned používá, není právě využito paměti. Proto při použití XIBs zajistěte, aby pouze jeden XIB za řadiče zobrazení a pokud je to možné, oddělte řadiče zobrazení zobrazení hierarchie do samostatných XIBs.

## <a name="optimize-image-resources"></a>Optimalizovat prostředky obrázků

Bitové kopie jsou některé nejnákladnější prostředky, které aplikace používají, a jsou často zaznamenané v vysoké řešení. Proto při zobrazení bitové kopie ze sady aplikace v [ `UIImageView` ](https://developer.xamarin.com/api/type/UIKit.UIImageView/), ujistěte se, že bitovou kopii a `UIImageView` se stejnou velikost. Změna měřítka obrázků v době běhu může být náročná operace, zvlášť pokud `UIImageView` vložené do [ `UIScrollView` ](https://developer.xamarin.com/api/type/UIKit.UIScrollView/).

Další informace najdete v tématu [optimalizovat prostředky obrázků](~/cross-platform/deploy-test/memory-perf-best-practices.md#optimizeimages) v [napříč platformami výkonu](~/cross-platform/deploy-test/memory-perf-best-practices.md) průvodce.

## <a name="test-on-devices"></a>Testování v zařízení

Začátek nasazení a testování aplikace na fyzické zařízení co nejdříve. Simulátorů neodpovídají perfektně chování a omezení zařízení, a proto je důležité a otestovat ve scénáři skutečné zařízení již v míře.

Zejména simulátoru žádným způsobem simulovat paměti nebo procesoru omezení fyzického zařízení.

## <a name="synchronize-animations-with-the-display-refresh"></a>Synchronizovat animace se aktualizace zobrazení

Hry mívají k úzkou smyčky ke spuštění herní logiku a aktualizovat obrazovku. Typické rámce sazby rozsahu od třicet na šedesát snímků za sekundu. Někteří vývojáři myslíte, že by měl aktualizovat na obrazovce co mnoho krát za sekundu, kombinace jejich herní simulace s aktualizacemi na obrazovku a rozhodnout nad rámec šedesát snímků za sekundu.

Však server zobrazení provádí aktualizace obrazovky v horní limit šedesát kolikrát za sekundu. Proto pokus o aktualizaci obrazovky rychleji, než toto omezení může vést k obrazovce zrušení a micro přerušování. Nejlepší struktura kódu je tak, aby aktualizace obrazovky jsou synchronizovány s aktualizací zobrazení. Toho lze dosáhnout pomocí [ `CoreAnimation.CADisplayLink` ](https://developer.xamarin.com/api/type/CoreAnimation.CADisplayLink/) třída, která je vhodná pro vizualizaci časovač a hry, které spouští v šedesát počet rámců za sekundu.

## <a name="avoid-core-animation-transparency"></a>Vyhněte se průhlednost základní animace

Zamezení základní animace průhlednost zlepšit výkon skládání rastrového obrázku. Obecně platí Vyhněte se transparentní vrstev a rozostřeného ohraničení Pokud je to možné.

## <a name="avoid-code-generation"></a>Vyhněte se vytváření kódu

Generování kódu dynamicky s `System.Reflection.Emit` nebo *Dynamic Language Runtime* je třeba se vyhnout, protože iOS jádra zabraňuje spuštění dynamické kódu.

## <a name="summary"></a>Souhrn

Tento článek popisuje a popsané techniky pro zvýšení výkonu aplikace sestavené s Xamarin.iOS. Tyto postupy souhrnně může výrazně snížit objem práce využití procesoru a paměti spotřebovávají aplikace.

## <a name="related-links"></a>Související odkazy

- [Napříč platformami výkonu](~/cross-platform/deploy-test/memory-perf-best-practices.md)
