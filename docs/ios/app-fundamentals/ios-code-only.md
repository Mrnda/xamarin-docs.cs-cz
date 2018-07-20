---
title: Vytváření uživatelských rozhraní iOS v kódu v Xamarin.iosu
description: Tento dokument popisuje, jak použít kód pro vytvoření uživatelského rozhraní pro aplikace Xamarin.iOS. Tento článek popisuje kontrolery zobrazení, vytváření hierarchie zobrazení, zpracování otáčení a další.
ms.prod: xamarin
ms.assetid: 7CB1FEAE-0BB3-4CDC-9076-5BD555003F1D
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 05/03/2018
ms.openlocfilehash: 5e9bf9555d10c8b34ad9323529d4af5ea66110f8
ms.sourcegitcommit: 8555a4dd1a579b2206f86c867125ee20fbc3d264
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/19/2018
ms.locfileid: "39156779"
---
# <a name="creating-ios-user-interfaces-in-code-in-xamarinios"></a>Vytváření uživatelských rozhraní iOS v kódu v Xamarin.iosu

Uživatelské rozhraní aplikace pro iOS se trochu prezentace – aplikace obvykle získá jedno okno, ale to můžete vyplnit okna s jak velký počet objektů, které potřebuje, a objekty a pravidla lze změnit v závislosti na tom, jaké aplikace chce zobrazit. Objekty v tomto scénáři – věcí, které se uživateli zobrazí – se nazývají zobrazení. K sestavení na jedné obrazovce v aplikaci, zobrazení skládaný na sebe navzájem v obsahu zobrazení hierarchie a hierarchie spravuje Kontroleru jednoho zobrazení. Aplikace s více obrazovkami mají více obsahu zobrazení hierarchie, každý s vlastní kontroler zobrazení a aplikace umístí zobrazení v okně vytvoření různých obsahu zobrazení hierarchie založený na obrazovce, jejímž je uživatel v.

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Následující diagram znázorňuje vztahy mezi oken, zobrazení, dílčích zobrazení a kontroler zobrazení, které vdechnou uživatelského rozhraní na obrazovce zařízení: 

[![](ios-code-only-images/image9.png "Tento diagram znázorňuje vztahy mezi oken, zobrazení, dílčích zobrazení a kontroler zobrazení")](ios-code-only-images/image9.png#lightbox)

Tyto zobrazení hierarchie lze sestavit pomocí [návrháře Xamarin pro iOS](~/ios/user-interface/designer/index.md) v sadě Visual Studio, ale je dobré mít základní znalosti práce zcela v kódu. Tento článek vás provede některé základní body ke zprovoznění a spuštění s vývojem pro rozhraní pouze kód uživatele.

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

Následující diagram znázorňuje vztahy mezi oken, zobrazení, dílčích zobrazení a kontroler zobrazení, které vdechnou uživatelského rozhraní na obrazovce zařízení: 

[![](ios-code-only-images/image9.png "Tento diagram znázorňuje vztahy mezi oken, zobrazení, dílčích zobrazení a kontroler zobrazení")](ios-code-only-images/image9.png#lightbox)

Tyto zobrazení hierarchie lze sestavit pomocí [návrháře Xamarin pro iOS](~/ios/user-interface/designer/index.md) v sadě Visual Studio pro Mac, ale je dobré mít základní znalosti práce zcela v kódu. Tento článek vás provede některé základní body ke zprovoznění a spuštění s vývojem pro rozhraní pouze kód uživatele.

-----

## <a name="creating-a-code-only-project"></a>Vytvoření projektu kódu

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

## <a name="ios-blank-project-template"></a>Prázdná šablona projektu iOS

Nejprve vytvořte projekt pro iOS pomocí sady Visual Studio **soubor > Nový Projekt > Visual C# > iPhone & iPad > aplikace (Xamarin) pro iOS** projektu, je uvedeno níže:

[![Dialogové okno nového projektu](ios-code-only-images/blankapp.w157-sml.png)](ios-code-only-images/blankapp.w157.png#lightbox)

Vyberte **prázdnou aplikaci** šablony projektu:

[![Vyberte šablonu dialogové okno](ios-code-only-images/blankapp-2.w157-sml.png)](ios-code-only-images/blankapp-2.w157.png#lightbox)

Šablonu prázdného projektu přidá 4 soubory do projektu:

[![Soubory projektu](ios-code-only-images/empty-project.w157-sml.png "soubory projektu")](ios-code-only-images/empty-project.w157.png#lightbox)


1. **AppDelegate.cs** – obsahuje `UIApplicationDelegate` podtřídy, `AppDelegate` , který se používá pro zpracování událostí aplikací z iOS. Okna aplikace se vytvoří v `AppDelegate`společnosti `FinishedLaunching` metody.
1. **Main.cs** – obsahuje vstupní bod aplikace, která určuje třídu pro `AppDelegate` .
1. **Soubor info.plist** – soubor seznamu vlastností, který obsahuje informace o konfiguraci aplikace.
1. **Do souboru Entitlements.plist** – soubor seznamu vlastností, který obsahuje informace o možnostech a oprávnění aplikace.


# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

## <a name="ios-templates"></a>iOS šablony


Visual Studio pro Mac neposkytuje prázdnou šablonu. Všechny šablony se dodávají s podporou scénářů, které Apple doporučuje jako primární způsob, jak vytvořit uživatelské rozhraní. Nicméně je možné vytvořit uživatelské rozhraní zcela v kódu. 

Následující postup vás provede scénáři odebrání aplikace: 


1. Můžete vytvořit nový projekt iOS aplikace s jedním zobrazením šablony:
    
    [![](ios-code-only-images/single-view-app.png "Použít šablonu aplikace s jedním zobrazením")](ios-code-only-images/single-view-app.png#lightbox)

1. Odstranit `Main.Storyboard` a `ViewController.cs` soubory. Proveďte **není** odstranit `LaunchScreen.Storyboard`. Kontroler zobrazení by měl odstranit, protože se kódu na pozadí pro kontroler zobrazení, která je vytvořena ve scénáři:
1. Je nutné vybrat **odstranit** v místním dialogovém okně:
    
    [![](ios-code-only-images/delete.png "Vyberte odstranit ze zobrazení dialogu")](ios-code-only-images/delete.png#lightbox)

1. V souboru Info.plist, odstraňte informace uvnitř **informace o nasazení > hlavní rozhraní** možnost:
    
    [![](ios-code-only-images/main-interface.png "Odstranění informací uvnitř možnost Hlavní rozhraní")](ios-code-only-images/main-interface.png#lightbox)

1. Nakonec přidejte následující kód do vašeho `FinishedLaunching` metody ve třídě AppDelegate:
        
        public override bool FinishedLaunching(UIApplication app, NSDictionary options)
        {
            // create a new window instance based on the screen size
            window = new UIWindow(UIScreen.MainScreen.Bounds);

            // make the window visible
            window.MakeKeyAndVisible();

            return true;
        }

Kód, který byl přidán do `FinishedLaunching` metoda v kroku 5 výše, je minimální množství kódu potřebného k vytvoření okna pro vaše aplikace pro iOS.


-----



aplikace iOS se vytvářejí pomocí [vzor MVC](~/ios/get-started/hello-ios-multiscreen/hello-ios-multiscreen-deepdive.md#Model_View_Controller). Na první obrazovce se zobrazí aplikace je vytvořený z kontroleru zobrazení v okně root. Najdete v článku [Hello, iOS s více obrazovkami](~/ios/get-started/hello-ios-multiscreen/index.md) Průvodce pro další podrobnosti o MVC vzorku samotný.

Implementace `AppDelegate` přidal šablona vytvoří okna aplikace nástroje, které existuje jen jednou pro každou aplikaci iOS a zobrazí ho následujícím kódem:

```csharp
public class AppDelegate : UIApplicationDelegate
{
    public override UIWindow Window
            {
                get;
                set;
            }

    public override bool FinishedLaunching(UIApplication app, NSDictionary options)
    {
        // create a new window instance based on the screen size
        window = new UIWindow(UIScreen.MainScreen.Bounds);

        // make the window visible
        window.MakeKeyAndVisible();

        return true;
    }
}
```

Pokud byste chtěli nyní tuto aplikaci spustit, by pravděpodobně získat výjimku vyvolanou oznamující, že `Application windows are expected to have a root view controller at the end of application launch`. Pojďme přidat kontroler a nastavte ji kontroler zobrazení kořenové aplikace.

## <a name="adding-a-controller"></a>Přidání Kontroleru

Vaše aplikace může obsahovat mnoho Kontrolery zobrazení, ale musí mít jeden kořenový Kontroleru zobrazení k řízení všech Kontrolerů zobrazení.  Přidání kontroleru do okna tak, že vytvoříte `UIViewController` instance a nastavíte ho na `window.RootViewController` vlastnost:

```csharp
public class AppDelegate : UIApplicationDelegate
{
    // class-level declarations

    public override UIWindow Window
    {
        get;
        set;
    }

    public override bool FinishedLaunching(UIApplication application, NSDictionary launchOptions)
    {
        // create a new window instance based on the screen size
        Window = new UIWindow(UIScreen.MainScreen.Bounds);

        var controller = new UIViewController();
        controller.View.BackgroundColor = UIColor.LightGray;

        Window.RootViewController = controller;

        // make the window visible
        Window.MakeKeyAndVisible();

        return true;
    }
}
```

Má každý kontroler přidružené zobrazení, která je přístupná z `View` vlastnost. Ve výše uvedeném kódu se změní zobrazení `BackgroundColor` vlastnost `UIColor.LightGray` tak, aby ji uvidí, jak je znázorněno níže:

 [![](ios-code-only-images/image1.png "V zobrazení na pozadí je viditelné světle šedá")](ios-code-only-images/image1.png#lightbox)

Může některý nastavíme `UIViewController` podtřídy jako `RootViewController` tímto způsobem, včetně řadičů z UIKit nezaloženými jsme napsat sami. Například následující kód přidá `UINavigationController` jako `RootViewController`:

```csharp
public class AppDelegate : UIApplicationDelegate
{
    // class-level declarations

    public override UIWindow Window
    {
        get;
        set;
    }

    public override bool FinishedLaunching(UIApplication application, NSDictionary launchOptions)
    {
        // create a new window instance based on the screen size
        Window = new UIWindow(UIScreen.MainScreen.Bounds);

        var controller = new UIViewController();
        controller.View.BackgroundColor = UIColor.LightGray;
        controller.Title = "My Controller";

        var navController = new UINavigationController(controller);

        Window.RootViewController = navController;

        // make the window visible
        Window.MakeKeyAndVisible();

        return true;
    }
}
```

Tímto se vytvoří kontroler vnořené kontroler navigace, jak je znázorněno níže:

 [![](ios-code-only-images/image2.png "Kontroler vnořené kontroler navigace")](ios-code-only-images/image2.png#lightbox)

## <a name="creating-a-view-controller"></a>Vytvoření Kontroleru zobrazení

Teď, když jsme jste viděli, jak přidat kontroler, jako `RootViewController` okraji okna, Pojďme zjistit, jak vytvořit vlastní zobrazení řadič v kódu.

Přidejte novou třídu s názvem `CustomViewController` jak je znázorněno níže:

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![](ios-code-only-images/customviewcontroller.w157-sml.png "Přidejte novou třídu s názvem CustomViewController")](ios-code-only-images/customviewcontroller.w157.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

[![](ios-code-only-images/new-file.png "Přidejte novou třídu s názvem CustomViewController")](ios-code-only-images/new-file.png#lightbox)

-----

Třída by měla dědit z `UIViewController`, což je v `UIKit` obor názvů, jak je znázorněno:

```csharp
using System;
using UIKit;

namespace CodeOnlyDemo
{
    class CustomViewController : UIViewController
    {
    }
}
```

<a name="Initializing_the_View"/>

## <a name="initializing-the-view"></a>Inicializace zobrazení

`UIViewController` obsahuje metodu nazvanou `ViewDidLoad` , která je volána, když kontroler zobrazení je prvním načtení do paměti. Toto je provést inicializaci zobrazení, jako je nastavení vlastnosti na příslušné místo.

Například následující kód přidá tlačítko a obslužné rutiny události při stisknutí tlačítka Vložit nový kontroler zobrazení do navigačního zásobníku:

```csharp
using System;
using CoreGraphics;
using UIKit;

namespace CodyOnlyDemo
{
    public class CustomViewController : UIViewController
    {
        public CustomViewController ()
        {
        }

        public override void ViewDidLoad ()
        {
            base.ViewDidLoad ();

            View.BackgroundColor = UIColor.White;
            Title = "My Custom View Controller";

            var btn = UIButton.FromType (UIButtonType.System);
            btn.Frame = new CGRect (20, 200, 280, 44);
            btn.SetTitle ("Click Me", UIControlState.Normal);

            var user = new UIViewController ();
            user.View.BackgroundColor = UIColor.Magenta;

            btn.TouchUpInside += (sender, e) => {
                this.NavigationController.PushViewController (user, true);
            };

            View.AddSubview (btn);

        }
    }
}
```

Načíst tento kontroler ve vaší aplikaci a ukázka jednoduchou navigaci, vytvořte novou instanci třídy `CustomViewController`. Vytvořit nový kontroler navigace, předejte instanci kontroleru zobrazení a nastavte nový kontroler navigace v okně `RootViewController` v `AppDelegate` stejně jako dříve:

```csharp
var cvc = new CustomViewController ();

var navController = new UINavigationController (cvc);

Window.RootViewController = navController;
```

Nyní při načtení aplikace, `CustomViewController` je načteno uvnitř kontroler navigace:

 [![](ios-code-only-images/customvc.png "Načtení CustomViewController uvnitř kontroler navigace")](ios-code-only-images/customvc.png#lightbox)
 
Kliknutím na tlačítko, bude _nabízených_ nový kontroler zobrazení do navigačního zásobníku:

[![](ios-code-only-images/customvca.png "Nový kontroler zobrazení vloženy do navigačního zásobníku")](ios-code-only-images/customvca.png#lightbox)

## <a name="building-the-view-hierarchy"></a>Vytváření zobrazení hierarchie

V předchozím příkladu jsme začali vytvářet uživatelské rozhraní v kódu tak, že přidáte tlačítko na kontroler zobrazení.

iOS uživatelské rozhraní se skládají z zobrazit hierarchii. Další zobrazení, jako je například popisky, tlačítka, posuvníky a podobně se přidají jako dílčích zobrazení některých nadřazeného zobrazení.

Například můžeme upravit `CustomViewController` vytvořit přihlašovací obrazovka, ve kterém může uživatel zadat uživatelské jméno a heslo. Na obrazovce bude obsahovat dvě textová pole a tlačítko.

### <a name="adding-the-text-fields"></a>Přidávání textových polí

Nejdřív odeberte tlačítko a události rutinu, která byla přidána do [inicializaci zobrazení](#Initializing_the_View) oddílu. 

Přidejte ovládací prvek pro uživatelské jméno ve vytváření a inicializaci `UITextField` a následným přidáním do zobrazení hierarchie, jak je znázorněno níže:

```csharp
class CustomViewController : UIViewController
{
    UITextField usernameField;

    public override void ViewDidLoad()
    {
        base.ViewDidLoad();

        View.BackgroundColor = UIColor.Gray;

        nfloat h = 31.0f;
        nfloat w = View.Bounds.Width;

        usernameField = new UITextField
        {
            Placeholder = "Enter your username",
            BorderStyle = UITextBorderStyle.RoundedRect,
            Frame = new CGRect(10, 82, w - 20, h)
        };

        View.AddSubview(usernameField);
    }
}
```

Když vytvoříme `UITextField`, jsme nastavili `Frame` vlastnost k definování jeho umístění a velikosti. V Iosu 0,0 bod se souřadnicemi je v levém horním rohu znakem + x napravo a + y dolů. Po nastavení `Frame` společně s pár dalších vlastností říkáme `View.AddSubview` přidáte `UITextField` do zobrazení hierarchie. Díky tomu `usernameField` dílčí zobrazení `UIView` instanci, která `View` odkazy na vlastnosti. Dílčí zobrazení se přidá s pořadí vykreslování, která je vyšší než jeho nadřazeném zobrazení, aby se zobrazovalo před nadřazeného zobrazení na obrazovce.

Aplikace se `UITextField` zahrnuté jsou uvedené níže:

 [![](ios-code-only-images/image4.png "Aplikace s UITextField zahrnuté")](ios-code-only-images/image4.png#lightbox)

Můžeme přidat `UITextField` pro heslo podobným způsobem, jenom tentokrát nastavíme `SecureTextEntry` vlastnost na hodnotu true, jak je znázorněno níže:

```csharp
public class CustomViewController : UIViewController
{
    UITextField usernameField, passwordField;
    public override void ViewDidLoad()
    {
       // keep the code the username UITextField
        passwordField = new UITextField
        {
            Placeholder = "Enter your password",
            BorderStyle = UITextBorderStyle.RoundedRect,
            Frame = new CGRect(10, 114, w - 20, h),
            SecureTextEntry = true
        };

      View.AddSubview(usernameField); 
      View.AddSubview(passwordField);
   }
}

```

Nastavení `SecureTextEntry = true` skryje textem zadaným ve `UITextField` tímto uživatelem, jak je znázorněno níže:

 [![](ios-code-only-images/image4a.png "Nastavení SecureTextEntry true skryje textem zadaným uživatelem.")](ios-code-only-images/image4a.png#lightbox)

### <a name="adding-the-button"></a>Přidání tlačítka

V dalším kroku přidáme vám tlačítko tak, že uživatel může odeslat uživatelské jméno a heslo. Přidáno tlačítko Zobrazit hierarchii, stejně jako jakýkoli jiný ovládací prvek, jejím předáním jako argument pro nadřazené zobrazení `AddSubview` metoda znovu.

Následující kód přidá tlačítko a zaregistruje obslužná rutina události `TouchUpInside` události:

```csharp
var submitButton = UIButton.FromType (UIButtonType.RoundedRect);

submitButton.Frame = new CGRect (10, 170, w - 20, 44);
submitButton.SetTitle ("Submit", UIControlState.Normal);

submitButton.TouchUpInside += (sender, e) => {
    Console.WriteLine ("Submit button pressed");
};

View.AddSubview(submitButton);
```

Přihlašovací obrazovka s tímto na místě, se teď zobrazí jak je znázorněno níže:

 [![](ios-code-only-images/image5.png "Přihlašovací obrazovky")](ios-code-only-images/image5.png#lightbox)

Na rozdíl od v předchozích verzích systému iOS, výchozí pozadí tlačítka je transparentní. Změna na tlačítko `BackgroundColor` změní vlastnost toto:

```csharp
submitButton.BackgroundColor = UIColor.White;
```

Výsledkem bude čtvercové tlačítko spíše než typické zaokrouhlí s okraji tlačítko. K získání zakulacený edge, použijte následující fragment kódu:

```csharp
submitButton.Layer.CornerRadius = 5f;
```

Tyto změny zobrazení bude vypadat takto:

[![](ios-code-only-images/image6.png "Spuštění příkladu zobrazení.")](ios-code-only-images/image6.png#lightbox)
 
## <a name="adding-multiple-views-to-the-view-hierarchy"></a>Přidání více zobrazení do zobrazení hierarchie

iOS poskytuje zařízení, které chcete přidat více zobrazení do zobrazení hierarchie s použitím `AddSubviews`.

```csharp
View.AddSubviews(new UIView[] { usernameField, passwordField, submitButton }); 
```

## <a name="adding-button-functionality"></a>Přidání funkce tlačítko

Po kliknutí na tlačítko se vaši uživatelé očekávají něco stane. Například se zobrazí výstraha nebo se provádí navigaci na jinou obrazovku. 

Přidejme nějaký kód tak, aby nabízel druhý kontroler zobrazení do navigačního zásobníku.

Nejprve vytvořte druhý kontroler zobrazení:

```csharp
var loginVC = new UIViewController () { Title = "Login Success!"};
loginVC.View.BackgroundColor = UIColor.Purple;
```

Pak přidejte funkci, která `TouchUpInside` události:

```csharp
submitButton.TouchUpInside += (sender, e) => {
                this.NavigationController.PushViewController (loginVC, true);
            };
```

Navigace je znázorněno níže:

[![](ios-code-only-images/navigation.png "Navigace je znázorněna v tomto grafu")](ios-code-only-images/navigation.png#lightbox)

Všimněte si, že ve výchozím nastavení, když použijete kontroler navigace, iOS poskytuje aplikaci navigačním panelu a tlačítko Zpět, aby bylo možné přesunout zpátky do zásobníku.

## <a name="iterating-through-the-view-hierarchy"></a>Zobrazit hierarchii iterace

Je možné k iteraci v rámci hierarchie dílčí zobrazení a vybrat všechny konkrétní zobrazení. Pro příklad, jak najít jednotlivé `UIButton` a poskytněte jinou toto tlačítko `BackgroundColor`, můžete použít následující fragment kódu

```csharp
foreach(var subview in View.Subviews)
{
    if (subview is UIButton)
    {
         var btn = subview as UIButton;
         btn.BackgroundColor = UIColor.Green;
    }
}
```

To, ale nebude fungovat, pokud je zobrazení probíhají pro `UIView` jako všechna zobrazení bude vracet jako `UIView` jako objekty přidané do nadřazeného zobrazení sami dědit `UIView`.

## <a name="handling-rotation"></a>Zpracování otáčení

Pokud uživatel otočí zařízení na šířku, ovládací prvky se velikost vhodným způsobem, jak ukazuje následující snímek obrazovky:

 [![](ios-code-only-images/image7.png "Pokud uživatel otočí zařízení na šířku, ovládací prvky odpovídajícím způsobem velikost")](ios-code-only-images/image7.png#lightbox)

Jeden způsob, jak tento problém vyřešit, je nastavení `AutoresizingMask` vlastnosti na každé zobrazení. V tomto případě chceme, aby ovládací prvky pro roztažení vodorovně, takže doporučujeme nastavit každý `AutoresizingMask`. Následující příklad je určený pro `usernameField`, ale stejné by bylo potřeba použít u každého miniaplikace v hierarchii zobrazení.

```csharp
usernameField.AutoresizingMask = UIViewAutoresizing.FlexibleWidth;
```

Nyní když jsme otočit zařízení nebo simulátoru, všechno, co roztáhne a vyplní další místo, jak je znázorněno níže:

 [![](ios-code-only-images/image8.png "Všechny ovládací prvky roztáhnout tak, aby vyplnil další místo")](ios-code-only-images/image8.png#lightbox)

## <a name="creating-custom-views"></a>Vytváření vlastních pohledů

Kromě použití ovládacích prvků, které jsou součástí UIKit, je také možné vlastní zobrazení. Můžete vytvořit vlastní zobrazení dědění z `UIView` a přepsáním `Draw`. Pojďme vytvořit vlastní zobrazení a přidejte ho do zobrazení hierarchie předvést.

### <a name="inheriting-from-uiview"></a>Dědění z UIView

První věc, kterou budeme muset udělat, je vytvořit třídu pro vlastního zobrazení. Provedeme to pomocí **třídy** šablony v sadě Visual Studio, chcete-li přidat prázdnou třídu s názvem `CircleView`. Základní třída musí být nastavená na `UIView`, které jsme si možná Vzpomínáte `UIKit` oboru názvů. Budete také potřebovat `System.Drawing` i obor názvů. Další různých `System.*` obory názvů nemusí být použitý v tomto příkladu, takže můžete bez obav odstranit je.

Třída by měla vypadat nějak takto:

```csharp
using System;

namespace CodeOnlyDemo
{
    class CircleView : UIView
    {
    }
}
```

### <a name="drawing-in-a-uiview"></a>Kreslení v UIView

Každý `UIView` má `Draw` metodu, která je volána v systému, když je potřeba vykreslit. `Draw` by se nikdy volat přímo. Během zpracování spuštění smyčky je volána v systému. Při prvním průchodu spuštění smyčky po zobrazení se přidá do zobrazení hierarchie jeho `Draw` metoda je volána. Následující volání `Draw` dojít, když je zobrazení označilo jako vyžadující vykreslit pomocí volání buď `SetNeedsDisplay` nebo `SetNeedsDisplayInRect` pro zobrazení.

Můžeme přidat kód pro vykreslování do našich zobrazení tak, že přidáte takový kód uvnitř přepsané `Draw` způsob, jak je znázorněno níže:

```csharp
public override void Draw(CGRect rect)
{
    base.Draw(rect);

    //get graphics context
    using (var g = UIGraphics.GetCurrentContext())
    {
        // set up drawing attributes
        g.SetLineWidth(10.0f);
        UIColor.Green.SetFill();
        UIColor.Blue.SetStroke();

        // create geometry
        var path = new CGPath();
        path.AddArc(Bounds.GetMidX(), Bounds.GetMidY(), 50f, 0, 2.0f * (float)Math.PI, true);

        // add geometry to graphics context and draw
        g.AddPath(path);
        g.DrawPath(CGPathDrawingMode.FillStroke);
    }
}
```

Protože `CircleView` je `UIView`, můžete také nastavit `UIView` také vlastnosti. Například jsme nastavili `BackgroundColor` v konstruktoru:

```csharp
public CircleView()
{
    BackgroundColor = UIColor.White;
}
```

Použít `CircleView` jsme právě vytvořili, můžete buď přidáme ji jako dílčí zobrazení na Zobrazit hierarchii v existujícího řadiče, jako jsme to udělali s `UILabels` a `UIButton` dříve, nebo nám můžete načíst jako zobrazení nového řadiče. Pojďme si ten.

### <a name="loading-a-view"></a>Načítání zobrazení

 `UIViewController` obsahuje metodu s názvem `LoadView` , která je volána metodou kontroleru k vytvoření jeho zobrazení. To je vhodné místo, kde můžete vytvořit zobrazení a přiřadit ji k kontroleru `View` vlastnost.

Nejdřív potřebujeme kontroleru, vytvořte novou prázdnou třídu s názvem `CircleController`.

V `CircleController` přidejte následující kód pro nastavení `View` k `CircleView` (byste neměli volat `base` implementace přepsání):

```csharp
using UIKit;

namespace CodeOnlyDemo
{
    class CircleController : UIViewController
    {
        CircleView view;

        public override void LoadView()
        {
            view = new CircleView();
            View = view;
        }
    }
}
```

Nakonec musíme prezentovat kontroleru za běhu. Provedeme to přidáním obslužné rutiny události na tlačítka pro odeslání, který jsme přidali dříve, následujícím způsobem:

```csharp
submitButton.TouchUpInside += delegate
{
    Console.WriteLine("Submit button clicked");

    //circleController is declared as class variable
    circleController = new CircleController();
    PresentViewController(circleController, true, null);
};
```

Nyní po spuštění aplikace a klepněte na tlačítko pro odeslání, se zobrazí nové zobrazení s kruh:

 [![](ios-code-only-images/circles.png "Zobrazí se nové zobrazení s kruh")](ios-code-only-images/circles.png#lightbox)

## <a name="creating-a-launch-screen"></a>Vytvoření obrazovky spuštění

A [spouštěcí obrazovka](~/ios/app-fundamentals/images-icons/launch-screens.md) se zobrazí při spuštění vaší aplikace jako způsob, jak zobrazit uživatelům, že je responzivní. Protože spouštěcí obrazovka se zobrazí, když se načítá aplikaci, nemůže být vytvořen v kódu, protože aplikace je stále načítáno do paměti. 

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Pokud vaše vytvoření projektu v sadě Visual Studio, spusťte obrazovku poskytnuty v podobě .xib soubor, který se nachází v iOS **prostředky** složky v projektu. 

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

Pokud vaše vytvoření projektu pro iOS v sadě Visual Studio pro Mac, na obrazovce spuštění je poskytována pro vás ve formě souboru ve scénáři. 

-----

To může upravit dvojité kliknutí na tlačítko a jeho otevřením v iOS designeru.

Apple doporučuje .xib nebo soubor scénáře se používá pro aplikace pro iOS 8 nebo novější, při spuštění buď soubor v iOS designeru budete používat třídy velikostí a automatické rozložení přizpůsobit si rozložení tak, aby vypadá dobře a zobrazí správně, pro všechna zařízení velikosti. Statické spouštěcí image je možné kromě .xib nebo scénáři povolit podporu pro cílení na dřívější verze aplikace.

Další informace o vytvoření obrazovky spuštění najdete v níže uvedených dokumentech:

- [Vytvoření obrazovky spuštění pomocí .xib](https://developer.xamarin.com/recipes/ios/general/templates/launchscreen-xib/)
- [Správa spouštěcí obrazovky s použitím scénářů](~/ios/app-fundamentals/images-icons/launch-screens.md)

> [!IMPORTANT]
> Od verze iOS 9 Apple doporučuje že scénáři by měla sloužit jako primární způsob vytvoření obrazovky spuštění.

### <a name="creating-a-launch-image-for-pre-ios-8-applications"></a>Vytvoření Image spusťte pro iOS před 8 aplikací

Statický obrázek je možné kromě .xib nebo scénáře spouštěcí obrazovky, pokud je aplikace určena pro verze před iOS 8. 

Tento statický obrázek lze nastavit v souboru Info.plist nebo katalog prostředků (pro iOS 7) ve vaší aplikaci. Je potřeba zadat samostatné Image pro každou velikost zařízení (320 × 480 měřiče, 640 × 960, 640 × 1136), vaše aplikace může běžet na. Další informace o velikosti obrazovky spuštění zobrazit [obrázky po spuštění obrazovky](~/ios/app-fundamentals/images-icons/launch-screens.md) průvodce.

> [!IMPORTANT]
> Pokud aplikace nemá žádná obrazovka spuštění, můžete si všimnout, že se nevejde plně na obrazovce. Pokud je to tento případ, by měl nezapomeňte zahrnout alespoň 640 × 1136 image s názvem `Default-568@2x.png` do souboru Info.plist. 



## <a name="summary"></a>Souhrn

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Tento článek byl věnován k vývoji aplikací pro iOS prostřednictvím kódu programu v sadě Visual Studio. Jsme se podívali na tom, jak vytvořit projekt z šablony Prázdný projekt diskuzemi o tom, jak vytvořit a přidat kontroler zobrazení kořenové do okna. Jsme pak jsme si ukázali, jak používat ovládací prvky z UIKit vytvoření hierarchie v kontroleru zobrazení pro vývoj obrazovce aplikace. Dále jsme se zaměřili jak provést zobrazení rozložení odpovídajícím způsobem v různých orientace a jsme viděli, jak vytvořit vlastní zobrazení ve vytváření podtříd `UIView`, také o tom se načíst zobrazení v rámci kontroleru. Nakonec Prozkoumali jsme Přidání spouštěcí obrazovky do aplikace.

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

Tento článek byl věnován k vývoji aplikací pro iOS prostřednictvím kódu programu v sadě Visual Studio pro Mac. Jsme se podívali na tom, jak vytvořit projekt z šablony jedno zobrazení diskuzemi o tom, jak vytvořit a přidat kontroler zobrazení kořenové do okna. Jsme pak jsme si ukázali, jak používat ovládací prvky z UIKit vytvoření hierarchie v kontroleru zobrazení pro vývoj obrazovce aplikace. Dále jsme se zaměřili jak provést zobrazení rozložení odpovídajícím způsobem v různých orientace a jsme viděli, jak vytvořit vlastní zobrazení ve vytváření podtříd `UIView`, také o tom se načíst zobrazení v rámci kontroleru. Nakonec Prozkoumali jsme Přidání spouštěcí obrazovky do aplikace.

-----




## <a name="related-links"></a>Související odkazy

- [SimpleLogin (ukázka)](https://developer.xamarin.com/samples/monotouch/SimpleLogin)
