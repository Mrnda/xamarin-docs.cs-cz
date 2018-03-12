---
title: "Přechody řadiče zobrazení"
ms.topic: article
ms.prod: xamarin
ms.assetid: 2C33018F-D64A-4BAA-A34E-082EF311D162
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 06/14/2017
ms.openlocfilehash: f85867f08b21a525937e1c938c7c8fd224554abc
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 02/27/2018
---
# <a name="view-controller-transitions"></a>Přechody řadiče zobrazení

UIKit přidává podporu pro přizpůsobení animovaný přechodu, který nastane, když prezentací řadiče zobrazení. Tato podpora je součástí integrované řadiče, stejně jako všechny vlastní řadiče, které dědí přímo z `UIViewController`. Kromě toho `UICollectionViewController` využívá řadiče přechod přizpůsobení využít animované přechody v kolekci zobrazení rozložení.

## <a name="custom-transitions"></a>Vlastní přechody

Animovaný přechod mezi řadiče zobrazení v iOS 7 je plně přizpůsobitelná. `UIViewController` nyní zahrnuje `TransitioningDelegate` vlastnost, která poskytuje třídu vlastní animator systému, když dojde k přechodu.

Použít vlastní přechodu se `PresentViewController`:

1.  Nastavte `ModalPresentationStyle` k `UIModalPresentationStyle.Custom` na řadiči předkládané.
2.  Implementace `UIViewControllerTransitioningDelegate` vytvořit třídu animator, což je instance `UIViewControllerAnimatedTransitioning` .
3.  Nastavte `TransitioningDelegate` vlastnost, která má instance `UIViewControllerTransitioningDelegate` , také na řadiči předkládané.
4.  K dispozici řadiče zobrazení.


Například následující kód představuje řadič zobrazení typu `ControllerTwo` – `UIViewController` podtřídami:

```csharp
showTwo.TouchUpInside += (object sender, EventArgs e) => {

    controllerTwo = new ControllerTwo ();

    this.PresentViewController (controllerTwo, true, null);
};
```

Spuštění aplikace a klepnete na tlačítko způsobí, že animace výchozí druhého řadiče zobrazení pro animaci v dole, jak je uvedeno níže:

 ![](transitions-images/no-custom-transition.png "Spuštění aplikace a klepnete na tlačítko způsobí, že animace výchozí druhého řadiče zobrazení a animace v dole")

Nastavení však `ModalPresentationStyle` a `TransitioningDelegate` výsledkem vlastní animace pro přechod:

```csharp
showTwo.TouchUpInside += (object sender, EventArgs e) => {

    controllerTwo = new ControllerTwo () {
        ModalPresentationStyle = UIModalPresentationStyle.Custom;
        };

    transitioningDelegate = new TransitioningDelegate ();
    controllerTwo.TransitioningDelegate = transitioningDelegate;

    this.PresentViewController (controllerTwo, true, null);
};
```

`TransitioningDelegate` Zodpovídá za vytvoření instance `UIViewControllerAnimatedTransitioning` podtřídami - názvem `CustomAnimator` v následujícím příkladu:

```csharp
public class TransitioningDelegate : UIViewControllerTransitioningDelegate
{
    CustomTransitionAnimator animator;

    public override IUIViewControllerAnimatedTransitioning PresentingController (UIViewController presented, UIViewController presenting, UIViewController source)
    {
        animator = new CustomTransitionAnimator ();
        return animator;
    }
}
```

Pokud nastane přechod systém vytvoří instanci `IUIViewControllerContextTransitioning`, který je předán animator metody. `IUIViewControllerContextTransitioning` obsahuje `ContainerView` animace se vyskytl, a také řadiče zobrazení inicializaci přechodu a probíhá přešla do řadiče zobrazení.

`UIViewControllerAnimatedTransitioning` Třída zpracovává skutečné animace. Musí být implementována dvě metody:

1.  `TransitionDuration` – Vrátí trvání animace v sekundách.
1.  `AnimateTransition` – provede skutečné animace.


Například následující třídy implementuje `UIViewControllerAnimatedTransitioning` pro animaci rámečku kontroleru zobrazení:

```csharp
public class CustomTransitionAnimator : UIViewControllerAnimatedTransitioning
{
    public CustomTransitionAnimator ()
    {
    }

    public override double TransitionDuration (IUIViewControllerContextTransitioning transitionContext)
    {
        return 1.0;
    }

    public override void AnimateTransition (IUIViewControllerContextTransitioning transitionContext)
        {
            var inView = transitionContext.ContainerView;
            var toVC = transitionContext.GetViewControllerForKey (UITransitionContext.ToViewControllerKey);
            var toView = toVC.View;

            inView.AddSubview (toView);

            var frame = toView.Frame;
            toView.Frame = CGRect.Empty;

            UIView.Animate (TransitionDuration (transitionContext), () => {
                toView.Frame = new CGRect (20, 20, frame.Width - 40, frame.Height - 40);
            }, () => {
                transitionContext.CompleteTransition (true);
            });
        }
}
```

Teď, po klepnutí na tlačítko animace se implementuje v `UIViewControllerAnimatedTransitioning` třída se používá:

 ![](transitions-images/custom-transition.png "Příklad přiblížení či oddálení platí systémem")

## <a name="collection-view-transitions"></a>Přechody zobrazení kolekce

Zobrazení kolekce mít integrovanou podporu pro vytváření animované přechody:

-  **Navigace řadiče** – animovaný přechod mezi dvěma `UICollectionViewController` instance může volitelně ošetřit automaticky při `UINavigationController` je spravuje.
-  **Přechod rozložení** – nový `UICollectionViewTransitionLayout` třída umožňuje interaktivní přechod mezi rozložení.


### <a name="navigation-controller-transitions"></a>Navigace řadiče přechody

Při použití v rámci řadič navigace `UICollectionViewController` zahrnuje podporu pro animované přechody mezi řadiči. Tato podpora je integrovaná a vyžaduje jenom pár jednoduchých kroků implementace:

1.  Nastavit `UseLayoutToLayoutNavigationTransitions` k `false` na `UICollectionViewController` .
1.  Přidá instanci `UICollectionViewController` do kořenového adresáře řadičem navigační zásobníku.
1.  Vytvořte druhý `UICollectionViewController` a nastavit jeho `UseLayoutToLayoutNavigtionTransitions` vlastnost `true` .
1.  Push druhý `UICollectionViewController` do zásobníku řadičem navigace.


Přidá následující kód `UICollectionViewController` podtřídami s názvem `ImagesCollectionViewController` do kořenového adresáře řadič navigační zásobník s `UseLayoutToLayoutNavigationTransitions` vlastnost nastavena na hodnotu `false`:

```csharp
UIWindow window;
ImagesCollectionViewController viewController;
UICollectionViewFlowLayout layout;
UINavigationController navController;

public override bool FinishedLaunching (UIApplication app, NSDictionary options)
{
    window = new UIWindow (UIScreen.MainScreen.Bounds);

    // create and initialize a UICollectionViewFlowLayout
    layout = new UICollectionViewFlowLayout (){
        SectionInset = new UIEdgeInsets (10,5,10,5),
        MinimumInteritemSpacing = 5,
        MinimumLineSpacing = 5,
        ItemSize = new CGSize (100, 100)
    };

    viewController = new ImagesCollectionViewController (layout) {
            UseLayoutToLayoutNavigationTransitions = false;
        }

    navController = new UINavigationController (viewController);

    window.RootViewController = navController;
    window.MakeKeyAndVisible ();
    
    return true;
}
```

Když je položka vybrána, druhé instance `ImagesController` je vytvořen pouze v tomto případě použití třídy různých rozložení. Pro tento kontroler `UseLayoutToLayoutNavigtionTransitions` je nastaven na `true`, jak je uvedeno níže:

```csharp
CircleLayout circleLayout;
ImagesCollectionViewController controller2;

...

public override void ItemSelected (UICollectionView collectionView, NSIndexPath indexPath)
{
    // UseLayoutToLayoutNavigationTransitions when item is selected
        circleLayout = new CircleLayout (Monkeys.Instance.Count){
                ItemSize = new CGSize (100, 100)
            };
            
    controller2 = new ImagesCollectionViewController (circleLayout) {
        UseLayoutToLayoutNavigationTransitions = true;
        }

    NavigationController.PushViewController (controller2, true);
}
```

`UseLayoutToLayoutNavigationTransitions` Musí být nastavena vlastnost před přidáním řadičem do zásobníku navigace. S touto sadou vlastností Normální vodorovné posuvné přechodu nahradí animovaný přechod mezi rozložení dva řadiče, jak je uvedeno dále:

![](transitions-images/nav2.png "Animovaný přechod mezi rozložení dva řadiče")

### <a name="transition-layout"></a>Přechod rozložení

Kromě podpory přechod rozložení v rámci navigační řadiče, nové rozložení názvem `UICollectionViewTransitionLayout` je nyní k dispozici. Tato třída rozložení umožňuje interaktivní řízení během procesu přechodu rozložení tím, že `TransitionProgress` nastavení z kódu. `UICollectionViewTransitionLayout` se liší od - a není to náhrada za - `SetCollectionViewLayout` metoda z iOS 6, která způsobila, že na přechod animovaný rozložení k dojít. Dané metody neposkytnul integrovanou podporu pro řízení průběh animovaný přechodu.

 `UICollectionViewTransitionLayout` umožňuje, například rozpoznávání gesto nakonfigurovat tak, aby řízení přechod mezi rozložení v odezvě na interakci s uživatelem, pomocí správy původní rozložení, jakož i určený rozložení pro přechod.

Kroky pro implementaci na interaktivní přechod v rámci pomocí nástroje pro rozpoznávání gesto `UICollectionViewTransitionLayout` jsou následující:

1.  Vytvořte pro rozpoznávání gesto.
1.  Volání `StartInteractiveTransition` metodu `UICollectionView` , předání rozložení cíl a dokončení obslužná rutina.
1.  Nastavte `TransitionProgress` vlastnost `UICollectionViewTransitionLayout` vrácená z instance `StartInteractiveTransition` metoda.
1.  Zrušení platnosti rozložení.
1.  Volání `FinishInteractiveTransition` metodu `UICollectionView` dokončení přechodu nebo `CancelInteractiveTransition` metoda ji zrušit.  `FinishInteractiveTransition` způsobí, že animace k dokončení jeho přechodu do cílové rozložení, zatímco `CancelInteractiveTransition` výsledkem animace vrátí k původní rozložení.
1.  Dokončení přechodu v obslužné rutině dokončení zpracování `StartInteractiveTransition` metoda.
1.  Přidáte nástroj pro rozpoznávání gesto zobrazení kolekce.


Následující kód implementuje na přechod interaktivní rozložení v rámci pro rozpoznávání roztahováním gesto:

```csharp
imagesController = new ImagesCollectionViewController (flowLayout);

nfloat sf = 0.4f;
UICollectionViewTransitionLayout trLayout = null;
UICollectionViewLayout nextLayout;

pinch = new UIPinchGestureRecognizer (g => {

    var progress = Math.Abs(1.0f -  g.Scale)/sf;

    if(trLayout == null){
        if(imagesController.CollectionView.CollectionViewLayout is CircleLayout)
            nextLayout = flowLayout;
        else
            nextLayout = circleLayout;

        trLayout = imagesController.CollectionView.StartInteractiveTransition (nextLayout, (completed, finished) => {   
            Console.WriteLine ("transition completed");
            trLayout = null;
        });
    }

    trLayout.TransitionProgress = (nfloat)progress;

    imagesController.CollectionView.CollectionViewLayout.InvalidateLayout ();

    if(g.State == UIGestureRecognizerState.Ended){
        if (trLayout.TransitionProgress > 0.5f)
            imagesController.CollectionView.FinishInteractiveTransition ();
        else
            imagesController.CollectionView.CancelInteractiveTransition ();
    }

});

imagesController.CollectionView.AddGestureRecognizer (pinch);

```

Jako uživatel pinches zobrazení kolekce `TransitionProgress` nastavena relativně k měřítku roztahováním. Tato implementace Pokud uživatel končí roztahováním před přechodu je 50 % dokončení přechodu, je zrušená. Přechodu, jinak je dokončena.




## <a name="related-links"></a>Související odkazy

- [Úvod do systému iOS 7 (ukázka)](https://developer.xamarin.com/samples/monotouch/IntroToiOS7)
- [iOS 7 přehled uživatelského rozhraní](~/ios/platform/introduction-to-ios7/ios7-ui.md)
- [Backgrounding](~/ios/app-fundamentals/backgrounding/index.md)