---
title: Výkon Xamarin.iOS
description: Tento dokument popisuje techniky, které lze použít ke zlepšení výkonu a využití paměti v aplikacích Xamarin.iOS.
ms.prod: xamarin
ms.assetid: 02b1f628-52d9-49de-8479-f2696546ca3f
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 01/29/2016
ms.openlocfilehash: 40a2acf28819279b2a0d5c1d50c651a79b455465
ms.sourcegitcommit: bf05041cc74fb05fd906746b8ca4d1403fc5cc7a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/04/2018
ms.locfileid: "39514460"
---
# <a name="xamarinios-performance"></a>Výkon Xamarin.iOS

Nízký výkon aplikace prezentuje v mnoha způsoby. Může být aplikace vypadá to, že nereaguje, může způsobit pomalé posouvání a může snížit výdrži baterie. Ale optimalizace výkonu zahrnuje více než jen implementace efektivního kódu. Prostředí uživatele s výkonem aplikace musíte také zvážit. Třeba zajistit, že operace spuštění bez blokování uživatele od provádění dalších aktivit může pomoct vylepšit uživatelské prostředí. 

Tento dokument popisuje techniky, které lze použít ke zlepšení výkonu a využití paměti v aplikacích Xamarin.iOS.

> [!NOTE]
> Před čtením tohoto článku byste si měli nejdřív přečíst [Cross-Platform výkonu](~/cross-platform/deploy-test/memory-perf-best-practices.md), který popisuje konkrétní postupy jiných platforem zlepšit využití paměti a výkonu aplikace založené na platformě Xamarin.

## <a name="avoid-strong-circular-references"></a>Vyhněte se silným cyklické odkazy

V některých situacích je možné vytvořit cykly silné odkazů, které by mohly bránit s jejich paměť uvolnit systémem uvolňování objektů. Například vezměte si situaci, kde [ `NSObject` ](https://developer.xamarin.com/api/type/Foundation.NSObject/)-odvozené podtřídy, jako je například třída, která dědí z [ `UIView` ](https://developer.xamarin.com/api/type/UIKit.UIView/), se přidá do `NSObject`-odvozený kontejneru a je důrazně na něj odkazovat z Objective-C, jak je znázorněno v následujícím příkladu kódu:

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

Když tento kód vytvoří `Container` instance, C# objektu bude mít silného odkazu na objekt Objective-C. Podobně platí `MyView` instance bude také mít silného odkazu na objekt Objective-C.

Kromě toho volání `container.AddSubview` zvýší počet odkazů na nespravovanou `MyView` instance. Pokud k tomu dojde, vytvoří modul runtime Xamarin.iOS `GCHandle` instance zachovat `MyView` objektu ve spravovaném kódu zachování připojení, protože neexistuje žádná záruka, že všechny spravované objekty budou uchovávat na ni odkaz. Z pohledu spravovaného kódu `MyView` objekt bude uvolněn po [ `AddSubview` ](https://developer.xamarin.com/api/member/UIKit.UIView.AddSubview/p/UIKit.UIView/) volání, kdyby není pro `GCHandle`.

Nespravovanou `MyView` objektu bude mít `GCHandle` odkazující na spravovaný objekt, označované jako *odhalila zřetelný*. Spravovaný objekt bude obsahovat odkaz na `Container` instance. Pak `Container` instance bude mít spravované referenční materiály k `MyView` objektu.

V případech, kde obsažený objekt uchovává odkaz na jeho kontejneru máte několik možností, které jsou k dispozici se cyklický odkaz:

-  Cyklus přerušit ručně nastavením odkaz na kontejner, aby `null`.
-  Ručně odeberte obsaženého objektu z kontejneru.
-  Volání `Dispose` na objektech.
-  Vyhněte se cyklický odkaz udržování Slabý odkaz na kontejner. Další informace o slabé odkazy.

### <a name="using-weakreferences"></a>Pomocí WeakReferences

Jedním ze způsobů, aby se zabránilo cyklus je použití nestálý odkaz z podřízené na nadřazený prvek. Například výše uvedený kód může být napsaná takto:

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

Tady obsaženého objektu nebude zachování nadřazeného objektu. Ale nadřazeného zachová podřízené aktivní prostřednictvím volání na Hotovo `container.AddSubView`.

K tomu dojde také za rozhraní API, použijte vzor zdroj delegáta nebo data, kde peer třída obsahuje implementaci; Iosu například při nastavení [ `Delegate` ](https://developer.xamarin.com/api/property/MonoTouch.UIKit.UITableView.Delegate/) vlastnost nebo [ `DataSource` ](https://developer.xamarin.com/api/property/MonoTouch.UIKit.UITableView.DataSource/) v [ `UITableView` ](https://developer.xamarin.com/api/type/UIKit.UITableView/) třídy.

V případě třídy, které jsou vytvořeny výhradně pro účely provádění protokol, třeba [ `IUITableViewDataSource` ](https://developer.xamarin.com/api/type/MonoTouch.UIKit.IUITableViewDataSource/), co můžete dělat se místo vytvoření podtřídy, stačí implementovat rozhraní ve třídě a přepsat Metoda a přiřaďte `DataSource` vlastnost `this`.

#### <a name="weak-attribute"></a>Slabé atribut

[Xamarin.iOS 11.10](https://developer.xamarin.com/releases/ios/xamarin.ios_11/xamarin.ios_11.10/#WeakAttribute) zavedené `[Weak]` atribut. Stejně jako `WeakReference <T>`, `[Weak]` lze použít k rozdělení [silné cyklické odkazy](https://docs.microsoft.com/en-us/xamarin/ios/deploy-test/performance#avoid-strong-circular-references), ale i menší kód.

Uvažujme následující kód, který používá `WeakReference <T>`:

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

Ekvivalentní kód využívající `[Weak]` je mnohem přesnější:

```csharp
public class MyFooDelegate : FooDelegate {
    [Weak] MyViewController controller;
    public MyFooDelegate (MyViewController ctrl) => controller = ctrl;
    public void CallDoSomething () => controller.DoSomething ();
}
```

Toto je další příklad použití `[Weak]` v kontextu [delegování](https://developer.apple.com/library/content/documentation/General/Conceptual/DevPedia-CocoaCore/Delegation.html) vzoru:

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

### <a name="disposing-of-objects-with-strong-references"></a>Uvolnění objektů s odkazy na silné

Pokud existuje silného odkazu a je těžké a odeberte závislost, ujistěte se, `Dispose` metoda vymazat nadřazený ukazatel.

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

Podřízený objekt, který udržuje silného odkazu k nadřazené úloze, zrušte odkaz na prvek v `Dispose` implementace:

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

Další informace o uvolnění silné odkazů naleznete v tématu [vydání IDisposable prostředky](~/cross-platform/deploy-test/memory-perf-best-practices.md#idisposable).
K dispozici je také vhodné diskuze v tomto blogovém příspěvku: [Xamarin.iOS, systému uvolňování paměti a me](http://krumelur.me/2015/04/27/xamarin-ios-the-garbage-collector-and-me/).

### <a name="more-information"></a>Další informace

Další informace najdete v tématu [pravidla, která vyhnout zachovat cykly](http://www.cocoawithlove.com/2009/07/rules-to-avoid-retain-cycles.html) na Cocoa s láskou [to je chyba v uvolňování paměti MonoTouch](http://stackoverflow.com/questions/13058521/is-this-a-bug-in-monotouch-gc) na StackOverflow, a [Proč nelze MonoTouch GC kill spravovaných objektů s refcount > 1? ](http://stackoverflow.com/questions/13064669/why-cant-monotouch-gc-kill-managed-objects-with-refcount-1) na StackOverflow.

## <a name="optimize-table-views"></a>Optimalizovat zobrazení tabulek

Uživatelé očekávají posouvání hladký a časy rychlé načtení [ `UITableView` ](https://developer.xamarin.com/api/type/UIKit.UITableView/) instancí. Ale posouvání výkonu může být negativně když buněk obsahují zobrazení hluboce vnořené hierarchie, nebo když buněk obsahují složitá rozložení. Existují však techniky, které je možné vyhnout se špatná `UITableView` výkonu:

- Znovu použít buňky. Další informace najdete v tématu [opakovaně používat buňky](#reusecells).
- Snižte počet dílčích zobrazení.
- Mezipaměť obsahu buňky, který je načten z webové služby.
- Mezipaměti výšky všech řádků, pokud nejsou stejné.
- Ujistěte se, v buňce a jiných zobrazení, neprůhledné.
- Vyhnout se tak škálování image a přechody.

Společně tyto postupy pomohou zajistit [ `UITableView` ](https://developer.xamarin.com/api/type/UIKit.UITableView/) instance plynulé posouvání.

### <a name="reuse-cells"></a>Opakované použití buňky

Při velkém množství řádků v zobrazení [ `UITableView` ](https://developer.xamarin.com/api/type/UIKit.UITableView/), bylo by o plýtvání paměti při vytváření stovek [ `UITableViewCell` ](https://developer.xamarin.com/api/type/UIKit.UITableViewCell/) objekty při pouze malý počet je se zobrazí na obrazovce najednou. Místo toho pouze v buňkách viditelný na obrazovce, je možné načíst do paměti, s **obsah** načítání do těchto znovu použít buňky. To zabraňuje vytváření instancí stovky dalších objektů, šetří čas a paměti.

Proto když buňky dané zařízení zmizí z obrazovky jeho zobrazení lze umístit do fronty pro opětovné použití, jak je znázorněno v následujícím příkladu kódu:

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

Procházení, [ `UITableView` ](https://developer.xamarin.com/api/type/UIKit.UITableView/) volání `GetCell` přepsání požádat o nový pohledů pro zobrazení. Toto přepsání pak volá [ `DequeueReusableCell` ](https://developer.xamarin.com/api/member/UIKit.UITableView.DequeueReusableCell/p/Foundation.NSString/) metoda a pokud je k dispozici pro použití se vrátí buňky.

Další informace najdete v tématu [buňky znovu použít](~/ios/user-interface/controls/tables/populating-a-table-with-data.md) v [naplnění tabulky daty](~/ios/user-interface/controls/tables/populating-a-table-with-data.md).

## <a name="use-opaque-views"></a>Použijte neprůhledný zobrazení

Ujistěte se, že všechna zobrazení, které mají bez průhledných definované jejich [ `Opaque` ](https://developer.xamarin.com/api/property/UIKit.UIView.Opaque/) sadu vlastností. Tím se zajistí optimální vykreslení zobrazení kreslení systému. To je zvlášť důležité, když je zobrazení součástí [ `UIScrollView` ](https://developer.xamarin.com/api/type/UIKit.UIScrollView/), nebo je součástí komplexní animace. V opačném případě výkresu systém bude složeného zobrazení s další obsah, což může výrazně ovlivnit výkon.

## <a name="avoid-fat-xibs"></a>Vyhněte se systémem souborů fat soubory XIb

I když soubory XIb do značné míry byly nahrazeny scénářů, existují některé okolnosti kde soubory XIb stále slouží. Když XIB je načten do paměti, veškerý její obsah jsou načtena do paměti, včetně všech imagí. Pokud XIB obsahuje zobrazení, které není okamžitě používá, pak se nevyužité paměti. Proto při použití soubory XIb Ujistěte se, že existuje pouze jeden XIB na kontroler zobrazení a pokud je to možné, rozdělte na samostatné soubory XIb kontroler zobrazení zobrazit hierarchii.

## <a name="optimize-image-resources"></a>Optimalizace prostředků obrázků

Bitové kopie jsou některé nejdražší prostředky, které používají aplikace a jsou často zachytí na vysoké rozlišení. Proto se při zobrazení bitové kopie ze sady prostředků aplikace v [ `UIImageView` ](https://developer.xamarin.com/api/type/UIKit.UIImageView/), ujistěte se, že na obrázku a `UIImageView` se stejnou velikost. Změna měřítka obrázků za běhu může být náročná operace, zejména v případě, `UIImageView` je součástí [ `UIScrollView` ](https://developer.xamarin.com/api/type/UIKit.UIScrollView/).

Další informace najdete v tématu [optimalizovat prostředky obrázků](~/cross-platform/deploy-test/memory-perf-best-practices.md#optimizeimages) v [Cross-Platform výkonu](~/cross-platform/deploy-test/memory-perf-best-practices.md) průvodce.

## <a name="test-on-devices"></a>Testovat na zařízeních

Počáteční nasazení a testování aplikací na fyzickém zařízení co nejdříve. Simulátory chování a omezení zařízení nemusíte zajistit dokonalou shodu a je tedy důležité, otestovat ve scénáři skutečných zařízení napravovat nejvíce.

Zejména simulátoru žádným způsobem simulovat paměti nebo procesoru omezení fyzického zařízení.

## <a name="synchronize-animations-with-the-display-refresh"></a>Synchronizovat animací pomocí zobrazení aktualizací

Hry nejméně odinstalovávají, jsou úzkou smyčky ke spuštění logika hry a aktualizovat obrazovku. Typické rámce sazby sahají od třicet do 60 snímků za sekundu. Někteří vývojáři pocit, že by měl aktualizovat obrazovku tolikrát, kolikrát nejvíce za sekundu, kombinování svých her simulace s aktualizacemi na obrazovku a mít tendenci se dostat nad rámec 60 snímků za sekundu.

Však server zobrazení provádí aktualizace obrazovky na maximální limit je šedesát časy za sekundu. Proto se pokus o aktualizaci obrazovky rychleji, než toto omezení může vést k obrazovce opětné a přerušování micro. Doporučujeme k uspořádání kódu tak, aby aktualizace obrazovky jsou synchronizovány s aktualizací zobrazení. Toho lze dosáhnout pomocí [ `CoreAnimation.CADisplayLink` ](https://developer.xamarin.com/api/type/CoreAnimation.CADisplayLink/) třídy, který je vhodný pro vizualizaci časovač a hrách, které běží na šedesát snímků za sekundu.

## <a name="avoid-core-animation-transparency"></a>Vyhněte se transparentnost základní animace

Předcházení základní animace transparentnosti zlepšení výkonu skládání rastrového obrázku. Obecně se vyhýbejte transparentní vrstvy a rozmazaný ohraničení Pokud je to možné.

## <a name="avoid-code-generation"></a>Vyhněte se vytváření kódu

Generování kódu dynamicky s `System.Reflection.Emit` nebo *Dynamic Language Runtime* je třeba se vyhnout, protože jádra s Iosem zabraňuje spuštění dynamické kódu.

## <a name="summary"></a>Souhrn

Tento článek popisuje a popsané techniky pro zvýšení výkonu aplikací vytvořených pomocí aplikace Xamarin.iOS. Společně tyto postupy mohou výrazně snížit množství práce prováděné procesoru a paměti spotřebované aplikací.

## <a name="related-links"></a>Související odkazy

- [Výkon napříč platformami](~/cross-platform/deploy-test/memory-perf-best-practices.md)
