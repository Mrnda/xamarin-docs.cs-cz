---
title: "Vytváření iOS uživatelského rozhraní v kódu"
description: "Xamarin.iOS nabízí dvě metody vytvoření uživatelského rozhraní pro vaši aplikaci – pomocí návrháře Xamarin pro iOS nebo v kódu. Tento článek prověří vytvoření uživatelského rozhraní iOS zcela v kódu. Ukazuje, jak začít ze šablony projektu stavět obrazovce aplikace v řadiči tím, vytváření hierarchie zobrazení z UIKit. Potom popisuje, jak vytvořit vlastní zobrazení, které je možné načíst v kontroleru."
ms.topic: article
ms.prod: xamarin
ms.assetid: 7CB1FEAE-0BB3-4CDC-9076-5BD555003F1D
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/18/2017
ms.openlocfilehash: b50c4bbef1510b739c4f7da7d732a4f4c66f13f3
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 03/09/2018
---
# <a name="creating-ios-user-interfaces-in-code"></a>Vytváření iOS uživatelského rozhraní v kódu

_Xamarin.iOS nabízí dvě metody vytvoření uživatelského rozhraní pro vaši aplikaci – pomocí návrháře Xamarin pro iOS nebo v kódu. Tento článek prověří vytvoření uživatelského rozhraní iOS zcela v kódu. Ukazuje, jak začít ze šablony projektu stavět obrazovce aplikace v řadiči tím, vytváření hierarchie zobrazení z UIKit. Potom popisuje, jak vytvořit vlastní zobrazení, které je možné načíst v kontroleru._

Uživatelské rozhraní aplikace iOS se podobá výkladní skříň – aplikace obvykle získá jeden interval, ale ho může zaplnit okno s jako mnoho objektů na to potřebuje, a objekty a uspořádání lze změnit v závislosti na tom, co aplikace chce, zobrazte. Objekty v tomto scénáři - věcí, které uživatel vidí - se nazývají zobrazení. K vytvoření jedné obrazovky v aplikaci, zobrazení jsou sebe v obsahu zobrazení hierarchie a hierarchie spravuje jeden řadič zobrazení. Aplikace s více obrazovek mají více obsahu zobrazení hierarchie, každou s vlastní řadiče zobrazení a aplikace umístí v okně vytvořit jiné obsahu zobrazení hierarchie založené na obrazovce, jejímž je uživatel v zobrazení.

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Následující obrázek znázorňuje vztahy mezi oken, zobrazení, dílčích zobrazení a View Controller, které přinášejí uživatelského rozhraní na obrazovce zařízení: 

[![](ios-code-only-images/image9.png "Tento diagram znázorňuje vztahy mezi oken, zobrazení, dílčích zobrazení a View Controller")](ios-code-only-images/image9.png#lightbox)

Tyto zobrazení hierarchie se dá vytvořit pomocí [Xamarin Designer pro iOS](~/ios/user-interface/designer/index.md) v sadě Visual Studio, ale je dobrým základní znalosti o tom, jak pracovat zcela v kódu. Tento článek vás provede některé základní body ke zprovoznění a běh vývoj jen kód uživatelského rozhraní.

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

Následující obrázek znázorňuje vztahy mezi oken, zobrazení, dílčích zobrazení a View Controller, které přinášejí uživatelského rozhraní na obrazovce zařízení: 

[![](ios-code-only-images/image9.png "Tento diagram znázorňuje vztahy mezi oken, zobrazení, dílčích zobrazení a View Controller")](ios-code-only-images/image9.png#lightbox)


Tyto zobrazení hierarchie se dá vytvořit pomocí [Xamarin Designer pro iOS](~/ios/user-interface/designer/index.md) v sadě Visual Studio pro Mac, ale je dobrým základní znalosti o tom, jak pracovat zcela v kódu. Tento článek vás provede některé základní body ke zprovoznění a běh vývoj jen kód uživatelského rozhraní.


-----

## <a name="creating-a-code-only-project"></a>Vytvoření projektu jen kódu

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

## <a name="ios-blank-project-template"></a>Prázdná šablona projektu iOS

Nejprve vytvořte projekt pro iOS v sadě Visual Studio pomocí iPhone **prázdného projektu** šablony, viz následující obrázek, kterou rozšiřujeme budete přidávat řadiče a zobrazení.


[![](ios-code-only-images/blankapp-vs.png "Dialogové okno Nový projekt")](ios-code-only-images/blankapp-vs.png#lightbox)


Prázdná šablona projektu přidá 4 soubory do projektu:


[![](ios-code-only-images/empty-project.png "Soubory projektu")](ios-code-only-images/empty-project.png#lightbox)


1. **AppDelegate.cs** – obsahuje `UIApplicationDelegate` podtřídami, `AppDelegate` , který se používá ke zpracování událostí aplikací z iOS. Okno aplikace je vytvořen v `AppDelegate`na `FinishedLaunching` metoda.
1. **Main.cs** – obsahuje vstupní bod aplikace, která určuje třídu pro `AppDelegate` .
1. **Info.plist** -soubor seznamu vlastností, který obsahuje informace o konfiguraci aplikace.
1. **Entitlements.plist** – soubor seznamu vlastností, který obsahuje informace o možnostech a oprávnění aplikace.


# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

## <a name="ios-templates"></a>iOS šablony


Visual Studio pro Mac neposkytuje prázdné šablony. Součástí všech šablon scénáře podpory, které Apple doporučuje jako primární způsob, jak vytvořit uživatelského rozhraní. Nicméně je možné vytvořit uživatelské rozhraní v zcela v kódu. 

Následující postup vás provede odebrání scénáři z aplikace: 


1. Šablonu jediné zobrazení aplikace použijte k vytvoření nové iOS projektu:
    
    [![](ios-code-only-images/single-view-app.png "Použít šablonu jediné zobrazení aplikace")](ios-code-only-images/single-view-app.png#lightbox)

1. Odstranit `Main.Storyboard` a `ViewController.cs` soubory. Proveďte **není** odstranit `LaunchScreen.Storyboard`. Řadiče zobrazení by měl odstranit, protože je kódu na pozadí pro řadič zobrazení, která je vytvořena ve scénáři:
1. Je nutné vybrat **odstranit** z v místním dialogovém okně:
    
    [![](ios-code-only-images/delete.png "Vyberte možnost odstranit z v místním dialogovém okně")](ios-code-only-images/delete.png#lightbox)

1. V Info.plist, odstraňte informací uvnitř **informace o nasazení > Main rozhraní** možnost:
    
    [![](ios-code-only-images/main-interface.png "Odstranění informací uvnitř možnost Hlavní rozhraní")](ios-code-only-images/main-interface.png#lightbox)

1. Nakonec přidejte následující kód do vaší `FinishedLaunching` metodu v třídě AppDelegate:
        
        public override bool FinishedLaunching(UIApplication app, NSDictionary options)
        {
            // create a new window instance based on the screen size
            window = new UIWindow(UIScreen.MainScreen.Bounds);

            // make the window visible
            window.MakeKeyAndVisible();

            return true;
        }


-----

## <a name="creating-a-window"></a>Vytvoření okna

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

Kód, který byl přidán do `FinishedLaunching` metoda v kroku 3 výše, je minimální množství kód potřebný k vytvoření okna pro aplikace iOS.  

-----

aplikace pro iOS jsou vytvořeny pomocí [vzor MVC](~/ios/get-started/hello-ios-multiscreen/hello-ios-multiscreen-deepdive.md#Model_View_Controller). První obrazovka, která aplikace zobrazuje je vytvořený z okna kořenové zobrazení řadiče. Najdete v článku [Hello, iOS Multiscreen](~/ios/get-started/hello-ios-multiscreen/index.md) Průvodce pro další podrobnosti o MVC vzor sám sebe.

Implementace pro `AppDelegate` přidal šablona vytvoří okno aplikace služby, které existuje jenom jeden pro každou aplikaci iOS a zobrazí ho následujícím kódem:

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

Pokud jste ke spuštění této aplikace nyní, by pravděpodobně získat výjimka vyvolaná oznamující, že `Application windows are expected to have a root view controller at the end of application launch`. Umožňuje přidat řadič a nastavit jej jako řadič zobrazení kořenové aplikace.

## <a name="adding-a-controller"></a>Přidání Kontroleru

Aplikace může obsahovat mnoho řadičů zobrazení, ale musí mít jeden kořenový řadič zobrazení řídit všechny řadiče zobrazení.  Přidat řadič do okna tak, že vytvoříte `UIViewController` instance a jeho nastavení na `window.RootViewController` vlastnost:

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

Každý řadič má přidružené zobrazení, která je přístupná z `View` vlastnost. Ve výše uvedeném kódu změní zobrazení `BackgroundColor` vlastnost `UIColor.LightGray` tak, aby se nebude zobrazovat, jak je uvedeno níže:

 [![](ios-code-only-images/image1.png "Pozadí zobrazení je viditelné světle šedé")](ios-code-only-images/image1.png#lightbox)

Nemůžeme nastavit všechny `UIViewController` podtřídami jako `RootViewController` tímto způsobem je také možné, včetně řadičů z UIKit a také ty jsme si zápisu. Například následující kód přidá `UINavigationController` jako `RootViewController`:

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

To vytváří řadičem vnořené v rámci kontroleru navigace, jak je uvedeno níže:

 [![](ios-code-only-images/image2.png "Řadič vnořené v rámci kontroleru navigace")](ios-code-only-images/image2.png#lightbox)

## <a name="creating-a-view-controller"></a>Vytvoření řadiče zobrazení

Teď, když jste viděli jak přidat řadič jako `RootViewController` okna, podíváme se, jak vytvořit vlastní zobrazení řadič v kódu.

Přidejte novou třídu s názvem `CustomViewController` jak je uvedeno níže:

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![](ios-code-only-images/customviewcontroller.png "Přidejte novou třídu s názvem CustomViewController")](ios-code-only-images/customviewcontroller.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

[![](ios-code-only-images/new-file.png "Přidejte novou třídu s názvem CustomViewController")](ios-code-only-images/new-file.png#lightbox)

-----

Třída musí dědit z `UIViewController`, což je v `UIKit` obor názvů, jak je znázorněno:

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

`UIViewController` obsahuje metodu s názvem `ViewDidLoad` který je volán při řadiče zobrazení prvním načtení do paměti. Toto je příslušné místo pro provádí inicializace zobrazení, jako je třeba nastavení jeho vlastnosti.

Například následující kód přidá tlačítko a obslužné rutiny události tak, aby nabízel nového řadiče zobrazení do zásobníku navigační při stisknutí tlačítka:

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

Načíst tento řadič v aplikaci a ukázka jednoduché navigace, vytvořte novou instanci třídy `CustomViewController`. Vytvořit nový řadič navigace, předejte v instanci řadiče zobrazení a nastavte nový řadič navigace v okně `RootViewController` v `AppDelegate` jako dříve:

```csharp
var cvc = new CustomViewController ();

var navController = new UINavigationController (cvc);

Window.RootViewController = navController;
```

Nyní když načtení aplikace, `CustomViewController` je načteno uvnitř řadič navigace:

 [![](ios-code-only-images/customvc.png "CustomViewController je načtena uvnitř řadič navigace")](ios-code-only-images/customvc.png#lightbox)
 
Klepnutím na tlačítko, bude _nabízené_ nového řadiče zobrazení do zásobníku navigace:

[![](ios-code-only-images/customvca.png "Nový řadič zobrazení vloženy do zásobníku navigace")](ios-code-only-images/customvca.png#lightbox)

## <a name="building-the-view-hierarchy"></a>Vytváření zobrazení hierarchie

V předchozím příkladu jsme začala vytvářet uživatelské rozhraní v kódu přidáním tlačítka řadiče zobrazení.

iOS uživatelského rozhraní se skládají z hierarchie zobrazení. Další zobrazení, jako je například popisky tlačítek, posuvníků, atd., jsou přidány jako dílčích zobrazení některých nadřazeného zobrazení.

Například můžeme upravit za účelem `CustomViewController` vytvořit přihlašovací obrazovku, kde může uživatel zadat uživatelské jméno a heslo. Na obrazovce bude obsahovat dvě textové pole a tlačítko.

### <a name="adding-the-text-fields"></a>Přidání textová pole

Nejprve odeberte tlačítko a událostí obslužná rutina, která byla přidána do [inicializace zobrazení](#Initializing_the_View) části. 

Přidání ovládacího prvku pro uživatelské jméno pomocí vytvoření a inicializace `UITextField` a jeho následným přidáním do zobrazení hierarchie, jak je uvedeno níže:

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

Když vytvoříme `UITextField`, nastaví `Frame` vlastnost určit jeho umístění a velikost. V iOS 0,0 souřadnice je v levém horním s + x napravo a + y dolů. Po nastavení `Frame` společně s několik dalších vlastností říkáme `View.AddSubview` přidat `UITextField` do zobrazení hierarchie. Díky tomu `usernameField` dílčí zobrazení `UIView` instanci `View` vlastnost odkazy. Dílčí zobrazení se přidá s pořadí, které je vyšší než jeho nadřazeném zobrazení, zobrazí se před nadřazeného zobrazení na obrazovce.

Aplikace s `UITextField` zahrnuté jsou uvedeny níže:

 [![](ios-code-only-images/image4.png "Aplikace s UITextField zahrnuté")](ios-code-only-images/image4.png#lightbox)

Nyní můžete přidat `UITextField` hesla podobným způsobem, pouze v tomto případě nastaví `SecureTextEntry` vlastnost na hodnotu true, jak je uvedeno níže:

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

Nastavení `SecureTextEntry = true` skryje zadaný v text `UITextField` uživatelem, jak je uvedeno níže:

 [![](ios-code-only-images/image4a.png "Nastavení SecureTextEntry true skryje text zadaný uživatelem")](ios-code-only-images/image4a.png#lightbox)

### <a name="adding-the-button"></a>Přidání tlačítka

Tlačítko Další, přidáme, uživatel může odeslat uživatelské jméno a heslo. Tlačítko zobrazení hierarchie jako další ovládací prvek, přidá předáním jako argument pro nadřazené zobrazení `AddSubview` metoda znovu.

Následující kód přidá tlačítko a zaregistruje obslužné rutiny události pro `TouchUpInside` událostí:

```csharp
var submitButton = UIButton.FromType (UIButtonType.RoundedRect);

submitButton.Frame = new CGRect (10, 170, w - 20, 44);
submitButton.SetTitle ("Submit", UIControlState.Normal);

submitButton.TouchUpInside += (sender, e) => {
    Console.WriteLine ("Submit button pressed");
};

View.AddSubview(submitButton);
```

Obrazovka pro přihlášení s tímto na místě, se teď zobrazí jak je uvedeno níže:

 [![](ios-code-only-images/image5.png "Obrazovka pro přihlášení")](ios-code-only-images/image5.png#lightbox)

Na rozdíl od v předchozích verzích systému iOS, na pozadí výchozí tlačítko je transparentní. Změna na tlačítko `BackgroundColor` změny vlastností toto:

```csharp
submitButton.BackgroundColor = UIColor.White;
```

Výsledkem bude odmocnina tlačítko spíše než typické zaokrouhlené s okraji tlačítko. Pokud chcete získat zaokrouhlené hranici, použijte následující fragment kódu:

```csharp
submitButton.Layer.CornerRadius = 5f;
```

Tyto změny bude zobrazení vypadat například takto:

[![](ios-code-only-images/image6.png "Na příkladu spuštění zobrazení")](ios-code-only-images/image6.png#lightbox)
 
## <a name="adding-multiple-views-to-the-view-hierarchy"></a>Přidání více zobrazení do zobrazení hierarchie

iOS poskytuje budovy přidání více zobrazení do zobrazení hierarchie pomocí `AddSubviews`.

```csharp
View.AddSubviews(new UIView[] { usernameField, passwordField, submitButton }); 
```

## <a name="adding-button-functionality"></a>Přidání tlačítka funkce

Při kliknutí na tlačítko vaši uživatelé budou očekávat něco stane, zda tato výstraha nebo přejdete na další obrazovce. 

Přidejme nějaký kód, který má být umístěn v zásobníku navigační druhého řadiče zobrazení.

Nejprve vytvořte druhý řadiče zobrazení:

```csharp
var loginVC = new UIViewController () { Title = "Login Success!"};
loginVC.View.BackgroundColor = UIColor.Purple;
```

Potom přidá funkci, která `TouchUpInside` událostí:

```csharp
submitButton.TouchUpInside += (sender, e) => {
                this.NavigationController.PushViewController (loginVC, true);
            };
```

Dole je zobrazená navigaci:

[![](ios-code-only-images/navigation.png "Navigaci je znázorněna v tomto grafu")](ios-code-only-images/navigation.png#lightbox)

Všimněte si, že ve výchozím nastavení, pokud použijete řadič navigační iOS poskytuje aplikaci navigačním panelu a tlačítko Zpět a umožní vám vrátit zpět na zásobníku.

## <a name="iterating-through-the-view-hierarchy"></a>Iterace v rámci hierarchie zobrazení

Je možné k iteraci v rámci hierarchie dílčí zobrazení a vybrat žádné konkrétní zobrazení. Např. pro každou najít `UIButton` a poskytněte toto tlačítko jiné `BackgroundColor`, můžete použít následující fragment kódu

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

To, ale nebude fungovat, pokud je zobrazení se vstupní pro `UIView` jako všechna zobrazení, vrátí se znovu jako `UIView` jako objekty přidané do nadřazeného zobrazení sami dědění `UIView`.

## <a name="handling-rotation"></a>Zpracování otočení

Pokud uživatel otočí zařízení na šířku, ovládací prvky se velikost odpovídajícím způsobem, jak ukazuje následující snímek obrazovky:

 [![](ios-code-only-images/image7.png "Pokud uživatel otočí zařízení na šířku, ovládací prvky velikost nemění správně")](ios-code-only-images/image7.png#lightbox)

Je možné tento problém opravit nastavením `AutoresizingMask` vlastnost u jednotlivých zobrazení. V tomto případě chceme ovládacích prvků k roztahování vodorovně, proto doporučujeme nastavit každý `AutoresizingMask`. Následující příklad je určený pro `usernameField`, ale stejné by bylo potřeba použít na každou miniaplikaci v hierarchii zobrazení.

```csharp
usernameField.AutoresizingMask = UIViewAutoresizing.FlexibleWidth;
```

Nyní když jsme otočte zařízení nebo simulátoru, vše, co roztahovány k vyplnění další prostor, jak je uvedeno níže:

 [![](ios-code-only-images/image8.png "Všechny ovládací prvky roztáhnou tak, aby vyplňování další místa")](ios-code-only-images/image8.png#lightbox)

## <a name="creating-custom-views"></a>Vytváření vlastních zobrazení

Kromě použití ovládacích prvků, které jsou součástí UIKit, můžete také použít vlastní zobrazení. Můžete vytvořit vlastní zobrazení, která dědí z `UIView` a přepsáním `Draw`. Umožňuje vytvořit vlastní zobrazení a přidejte ji do zobrazení hierarchie k předvedení.

### <a name="inheriting-from-uiview"></a>Dědění z UIView

První věc, kterou je potřeba udělat je vytvoření třídy pro vlastní zobrazení. Provedeme to pomocí **třída** šablony v sadě Visual Studio lze přidat prázdný třídu s názvem `CircleView`. Základní třída musí být nastavena na `UIView`, které odvolání je v `UIKit` oboru názvů. Budete také potřebovat `System.Drawing` i obor názvů. Další různých `System.*` obory názvů nemusí být použité v tomto příkladu, takže můžete bez obav odstranit je.

Třída by měla vypadat takto:

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

Každý `UIView` má `Draw` metoda, která je volána v systému, když je potřeba vykreslit. `Draw` by nikdy volat přímo. V systému je volána při zpracování spuštění smyčky. Při prvním prostřednictvím spuštění smyčky po přidání zobrazení do zobrazení hierarchie, jeho `Draw` metoda je volána. Následující volání `Draw` dojít, když zobrazení je označen jako museli být vykresleny voláním buď `SetNeedsDisplay` nebo `SetNeedsDisplayInRect` v zobrazení.

Kreslení kód jsme můžete přidat do našich zobrazení přidáním takový kód uvnitř přepsané `Draw` metoda, jak je uvedeno níže:

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

Vzhledem k tomu `CircleView` je `UIView`, můžete také nastavit `UIView` také vlastnosti. Například můžete nastavit jsme `BackgroundColor` v konstruktoru:

```csharp
public CircleView()
{
    BackgroundColor = UIColor.White;
}
```

Použít `CircleView` jsme právě vytvořili, můžete buď přidáme ji jako dílčí zobrazení k zobrazení hierarchie v existujícím řadiči, jako jsme to udělali s `UILabels` a `UIButton` dříve, nebo se nám načíst jako zobrazení nového řadiče. Umožňuje provést ty druhé.

### <a name="loading-a-view"></a>Načítání zobrazení

 `UIViewController` má metodu s názvem `LoadView` který je volán adaptérem vytvoření jeho zobrazení. Toto je příslušné místo pro vytvoření zobrazení a přiřaďte ho ke kontroleru `View` vlastnost.

Nejdřív potřebujeme řadič, takže vytvořte novou prázdnou třídu s názvem `CircleController`.

V `CircleController` přidejte následující kód k nastavení `View` k `CircleView` (nesmí zavoláte `base` implementace v přepsání):

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

Nakonec je potřeba k dispozici řadičem za běhu. Umožňuje provést přidáním obslužné rutiny události na tlačítko pro odeslání, který jsme přidali dříve, a to takto:

```csharp
submitButton.TouchUpInside += delegate
{
    Console.WriteLine("Submit button clicked");

    //circleController is declared as class variable
    circleController = new CircleController();
    PresentViewController(circleController, true, null);
};
```

Teď když jsme aplikaci spustit a klepněte na tlačítko pro odeslání, nové zobrazení s kroužkem se zobrazí:

 [![](ios-code-only-images/circles.png "Zobrazí se nové zobrazení s kroužkem")](ios-code-only-images/circles.png#lightbox)

## <a name="creating-a-launch-screen"></a>Vytváření úvodní obrazovka

A [úvodní obrazovka](~/ios/app-fundamentals/images-icons/launch-screens.md) se zobrazí při spuštění vaší aplikace jako způsob, jak zobrazit pro vaše uživatele, že se jedná o reaguje. Protože úvodní obrazovka se zobrazí při načítání aplikace, nemůže být vytvořeny ve kódu, jako je aplikace stále probíhá načtena do paměti. 

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Když vaše vytvoření projektu v sadě Visual Studio, spusťte obrazovky se poskytuje pro vás ve formě .xib souboru, který lze nalézt v iOS **prostředky** složky uvnitř projektu. 

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

Pokud vaše vytvoření iOS projektu v sadě Visual Studio pro Mac, spusťte obrazovky je poskytována pro vás ve formě soubor scénáře. 

-----

To se dá upravit tak, že dvojité kliknutí na tlačítko a ho otevřít v Návrháři iOS.

Apple doporučuje .xib nebo Storyboard souboru se používá pro aplikace cílený na iOS 8 nebo novější, při spuštění buď soubor v Návrháři iOS, budete používat velikost třídy a automatického rozložení přizpůsobit rozložení tak, aby spokojeni a zobrazí správně, pro všechna zařízení velikosti. Statické spouštěcí image lze kromě .xib nebo Storyboard, povolit podporu pro cílení na dřívější verze aplikace.

Další informace o vytvoření obrazovky spustit najdete v dokumentech níže:

- [Vytvoření obrazovky spuštění pomocí .xib](https://developer.xamarin.com/recipes/ios/general/templates/launchscreen-xib/)
- [Správa spuštění obrazovky s scénářů](~/ios/app-fundamentals/images-icons/launch-screens.md)

> [!IMPORTANT]
> **Poznámka:** od verze iOS 9, Apple doporučuje, aby scénářů má být použit jako primární metodou pro vytvoření obrazovky spustit.

### <a name="creating-a-launch-image-for-pre-ios-8-applications"></a>Vytváření obrazem spustit pro starší než iOS 8 aplikací

Statický obrázek lze použít kromě .xib nebo Storyboard úvodní obrazovka, pokud aplikace cílí verzím iOS 8. 

Tento statický obrázek lze nastavit v souboru Info.plist nebo katalog Asset (pro iOS 7) ve vaší aplikaci. Musíte zajistit samostatné Image pro každé zařízení velikost (320 x 480, 640 x 960, 640 x 1136), které aplikace může být na. Další informace na velikost obrazovky spustit [spuštění bitové kopie obrazovky](~/ios/app-fundamentals/images-icons/launch-screens.md) průvodce.

> [!IMPORTANT]
> **Poznámka:** Pokud má vaše aplikace bez spuštění obrazovky, můžete si všimnout, že se nevejde plně na obrazovce. Pokud je to tento případ, musí si nezapomeňte zahrnout alespoň, 640 x 1136 obrázek s názvem `Default-568@2x.png` k vaší Info.plist. 



## <a name="summary"></a>Souhrn

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Tento článek popsané postupy pro vývoj aplikací pro iOS prostřednictvím kódu programu v sadě Visual Studio. Jsme se podívali na tom, jak sestavit projekt z prázdná šablona projektu, hovoříte o tom, jak vytvořit a přidat řadič zobrazení kořenové do okna. Jsme potom vám ukázal, jak pomocí ovládacích prvků z UIKit můžete vytvořit zobrazení hierarchie v kontroleru k vývoji obrazovce aplikace. Potom jsme se zaměřili na tom, jak chcete-li zobrazení Rozvrhněte správně v různých orientaci a jsme viděli, jak vytvořit vlastní zobrazení vytváření podtříd `UIView`, a jak načíst zobrazení v kontroleru. Nakonec jsme prozkoumali úvodní obrazovka přidání do aplikace.

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

Tento článek popsané postupy pro vývoj aplikací pro iOS prostřednictvím kódu programu v sadě Visual Studio for Mac. Jsme se podívali na tom, jak sestavit projekt ze šablony jednoho zobrazení, hovoříte o tom, jak vytvořit a přidat řadič zobrazení kořenové do okna. Jsme potom vám ukázal, jak pomocí ovládacích prvků z UIKit můžete vytvořit zobrazení hierarchie v kontroleru k vývoji obrazovce aplikace. Potom jsme se zaměřili na tom, jak chcete-li zobrazení Rozvrhněte správně v různých orientaci a jsme viděli, jak vytvořit vlastní zobrazení vytváření podtříd `UIView`, a jak načíst zobrazení v kontroleru. Nakonec jsme prozkoumali úvodní obrazovka přidání do aplikace.

-----




## <a name="related-links"></a>Související odkazy

- [SimpleLogin (ukázka)](https://developer.xamarin.com/samples/monotouch/SimpleLogin)
