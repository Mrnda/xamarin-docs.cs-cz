---
title: návrh uživatelského rozhraní v Xamarin.Mac.Storyboard/.XIb-Less
description: Tento článek popisuje vytvoření uživatelského rozhraní aplikace Xamarin.Mac přímo z kódu jazyka C#, bez .storyboard soubory, soubory .xib nebo rozhraní tvůrce.
ms.prod: xamarin
ms.assetid: 02310F58-DCF1-4589-9F4A-065DF64FC0E1
ms.technology: xamarin-mac
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/14/2017
ms.openlocfilehash: 239133c8a5bcce97aca0c4444624fe0541600354
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 06/05/2018
ms.locfileid: "34792398"
---
# <a name="storyboardxib-less-user-interface-design-in-xamarinmac"></a>návrh uživatelského rozhraní v Xamarin.Mac.Storyboard/.XIb-Less

_Tento článek popisuje vytvoření uživatelského rozhraní aplikace Xamarin.Mac přímo z kódu jazyka C#, bez .storyboard soubory, soubory .xib nebo rozhraní tvůrce._

## <a name="overview"></a>Přehled

Při práci s C# a rozhraní .NET v aplikaci Xamarin.Mac, máte přístup do stejné prvky uživatelského rozhraní a nástroje, které vývojář pracující *jazyka Objective-C* a *Xcode* nepodporuje. Obvykle při vytváření aplikace Xamarin.Mac, budete používat pro Xcode rozhraní tvůrce s .storyboard nebo .xib soubory pro vytváření a údržbu můžete uživatelském rozhraní aplikace.

Máte také možnost vytvořit některé nebo všechny uživatelského rozhraní aplikace Xamarin.Mac přímo v kódu jazyka C#. V tomto článku vám nabídneme základní informace o vytváření uživatelského rozhraní a prvky uživatelského rozhraní v kódu jazyka C#.

[![Visual Studio pro Mac editor kódu](xibless-ui-images/intro01.png "sadě Visual Studio pro Mac editor kódu")](xibless-ui-images/intro01-large.png#lightbox)

<a name="Switching_a_Window_to_use_Code" />

## <a name="switching-a-window-to-use-code"></a>Přepínání a údržbu za účelem použití kódu

Když vytvoříte novou aplikaci Xamarin.Mac kakao, zobrazí okno Standardní prázdné, ve výchozím nastavení. Toto systému windows je definována v **Main.storyboard** (nebo tradičně **MainWindow.xib**) automaticky zahrnutý v projektu. To zahrnuje také **ViewController.cs** soubor, který spravuje hlavního zobrazení aplikace (nebo znovu tradičně **MainWindow.cs** a **MainWindowController.cs** souboru).

Přejděte do okna Xibless pro aplikaci, postupujte takto:

1. Otevřete aplikaci, kterou chcete přestat používat `.stroyboard` nebo .xib soubory k definování uživatelského rozhraní v sadě Visual Studio for Mac.
2. V **řešení Pad**, klikněte pravým tlačítkem na **Main.storyboard** nebo **MainWindow.xib** soubor a vyberte **odebrat**: 

    ![Odebrání hlavní storyboard nebo okno](xibless-ui-images/switch01.png "odebrání hlavní storyboard nebo okna")
3. Z **odebrat dialogové okno**, klikněte **odstranit** tlačítko úplné odebrání .storyboard nebo .xib z projektu: 

    ![Potvrzení odstranění](xibless-ui-images/switch02.png "potvrzení odstranění")

Nyní budeme muset upravit **MainWindow.cs** souboru k definování rozložení okna a úprava **ViewController.cs** nebo **MainWindowController.cs** soubor k vytvoření instance naše `MainWindow` třídy vzhledem k tomu, že se už používá soubor .storyboard nebo .xib.

Moderní aplikace Xamarin.Mac, které používají scénářů pro jejich uživatelské rozhraní nesmí obsahovat automaticky **MainWindow.cs**, **ViewController.cs** nebo **MainWindowController.cs** soubory. Podle potřeby, jednoduše přidejte novou prázdnou C# třídu do projektu (**přidat** > **nový soubor...**   >  **Obecné** > **prázdné třídy**) a pojmenujte ji stejná jako soubor chybí. 


### <a name="defining-the-window-in-code"></a>Definování intervalu v kódu

Potom upravte **MainWindow.cs** souboru a nastavit jej vypadat třeba takto:

```csharp
using System;
using Foundation;
using AppKit;
using CoreGraphics;

namespace MacXibless
{
    public partial class MainWindow : NSWindow
    {
        #region Private Variables
        private int NumberOfTimesClicked = 0;
        #endregion

        #region Computed Properties
        public NSButton ClickMeButton { get; set;}
        public NSTextField ClickMeLabel { get ; set;}
        #endregion

        #region Constructors
        public MainWindow (IntPtr handle) : base (handle)
        {
        }

        [Export ("initWithCoder:")]
        public MainWindow (NSCoder coder) : base (coder)
        {
        }

        public MainWindow(CGRect contentRect, NSWindowStyle aStyle, NSBackingStore bufferingType, bool deferCreation): base (contentRect, aStyle,bufferingType,deferCreation) {
            // Define the user interface of the window here
            Title = "Window From Code";

            // Create the content view for the window and make it fill the window
            ContentView = new NSView (Frame);

            // Add UI elements to window
            ClickMeButton = new NSButton (new CGRect (10, Frame.Height-70, 100, 30)){
                AutoresizingMask = NSViewResizingMask.MinYMargin
            };
            ContentView.AddSubview (ClickMeButton);

            ClickMeLabel = new NSTextField (new CGRect (120, Frame.Height - 65, Frame.Width - 130, 20)) {
                BackgroundColor = NSColor.Clear,
                TextColor = NSColor.Black,
                Editable = false,
                Bezeled = false,
                AutoresizingMask = NSViewResizingMask.WidthSizable | NSViewResizingMask.MinYMargin,
                StringValue = "Button has not been clicked yet."
            };
            ContentView.AddSubview (ClickMeLabel);
        }
        #endregion

        #region Override Methods
        public override void AwakeFromNib ()
        {
            base.AwakeFromNib ();

            // Wireup events
            ClickMeButton.Activated += (sender, e) => {
                // Update count
                ClickMeLabel.StringValue = (++NumberOfTimesClicked == 1) ? "Button clicked one time." : string.Format("Button clicked {0} times.",NumberOfTimesClicked);
            };
        }
        #endregion

    }
}
```

Probereme několik klíčových prvků.

Nejdřív jsme přidali pár _počítaný vlastnosti_ , bude fungovat stejně jako výstupy (jako v případě, že okno byl vytvořen v souboru .storyboard nebo .xib):

```csharp
public NSButton ClickMeButton { get; set;}
public NSTextField ClickMeLabel { get ; set;}
```

To nám umožní přístup k elementům uživatelského rozhraní, které bude zobrazovat v okně. Vzhledem k tomu, že okno není právě zvětšený ze souboru .storyboard nebo .xib, budeme potřebovat způsob, jak vytvořit instanci (jako ukážeme později v `MainWindowController` třídy). To je, jaké jsou tato nová metoda konstruktor:

```csharp
public MainWindow(CGRect contentRect, NSWindowStyle aStyle, NSBackingStore bufferingType, bool deferCreation): base (contentRect, aStyle,bufferingType,deferCreation) {
    ...
}
```

Toto je kde jsme se návrh rozložení okna a umístit všechny prvky uživatelského rozhraní, které jsou nutné k vytvoření požadované uživatelské rozhraní. Před přidáme všechny prvky uživatelského rozhraní do okna je nutné _zobrazení obsahu_ tak, aby obsahovala prvky:

```csharp
ContentView = new NSView (Frame);
```

Tím se vytvoří zobrazení obsahu, který naplní okna. Teď přidáme naše první prvek uživatelského rozhraní `NSButton`, do okna:

```csharp
ClickMeButton = new NSButton (new CGRect (10, Frame.Height-70, 100, 30)){
    AutoresizingMask = NSViewResizingMask.MinYMargin
};
ContentView.AddSubview (ClickMeButton);
```

Nejprve si všimněte si, zde je, že, na rozdíl od iOS, systému macOS používá matematické zápis k definování jeho okno souřadnicový systém. Proto počáteční bod je v levém dolním rohu okna s zvýšení hodnoty vpravo a směrem k horním pravém horním rohu okna. Když vytvoříme nové `NSButton`, jsme vzít v úvahu jak jsme definovali pozice a jeho velikost na obrazovce.

`AutoresizingMask = NSViewResizingMask.MinYMargin` Vlastnost říká tlačítku, že chceme zůstat ve stejném umístění, z horní části okna při změně okno velikosti ve svislém směru. Znovu toto je nutné kvůli (0,0) je v levém dolní části okna.

Nakonec `ContentView.AddSubview (ClickMeButton)` metoda přidá `NSButton` k zobrazení obsahu, aby se zobrazí na obrazovce, když je aplikace spustit a zobrazení okna.

Další štítek se přidá do okna, které se zobrazí počet, kolikrát, `NSButton` klepnutí na: 

```csharp
ClickMeLabel = new NSTextField (new CGRect (120, Frame.Height - 65, Frame.Width - 130, 20)) {
    BackgroundColor = NSColor.Clear,
    TextColor = NSColor.Black,
    Editable = false,
    Bezeled = false,
    AutoresizingMask = NSViewResizingMask.WidthSizable | NSViewResizingMask.MinYMargin,
    StringValue = "Button has not been clicked yet."
};
ContentView.AddSubview (ClickMeLabel);
``` 

Vzhledem k tomu, že systému macOS nemá konkrétní _popisek_ element uživatelského rozhraní, jsme přidali speciálně stylem, upravovat `NSTextField` tak, aby fungoval jako popisek. Jenom jako tlačítko před trvá velikost a umístění v úvahu tuto (0,0) je v levém dolní části okna. `AutoresizingMask = NSViewResizingMask.WidthSizable | NSViewResizingMask.MinYMargin` Používá vlastnost **nebo** operátor kombinace obou `NSViewResizingMask` funkce. Bude popisek zůstat ve stejném umístění, z horní části okna, když je v okně svisle ke změně velikosti a zmenšit a zvětšit šířku, jak je, že změníte velikost vodorovně.

Znovu `ContentView.AddSubview (ClickMeLabel)` metoda přidá `NSTextField` k zobrazení obsahu, které se zobrazí na obrazovce, když je aplikace spuštěna a otevřít okno.


### <a name="adjusting-the-window-controller"></a>Úprava řadičem okna

Od návrh `MainWindow` je již načtena ze souboru .storyboard nebo .xib, budeme potřebovat provádět některé úpravy řadičem okno. Upravit **MainWindowController.cs** souboru a nastavit jej vypadat třeba takto:

```csharp
using System;

using Foundation;
using AppKit;
using CoreGraphics;

namespace MacXibless
{
    public partial class MainWindowController : NSWindowController
    {
        public MainWindowController (IntPtr handle) : base (handle)
        {
        }

        [Export ("initWithCoder:")]
        public MainWindowController (NSCoder coder) : base (coder)
        {
        }

        public MainWindowController () : base ("MainWindow")
        {
            // Construct the window from code here
            CGRect contentRect = new CGRect (0, 0, 1000, 500);
            base.Window = new MainWindow(contentRect, (NSWindowStyle.Titled | NSWindowStyle.Closable | NSWindowStyle.Miniaturizable | NSWindowStyle.Resizable), NSBackingStore.Buffered, false);

            // Simulate Awaking from Nib
            Window.AwakeFromNib ();
        }

        public override void AwakeFromNib ()
        {
            base.AwakeFromNib ();
        }

        public new MainWindow Window {
            get { return (MainWindow)base.Window; }
        }
    }
}

```

Let – klíčové prvky tato úprava zabývat.

Nejprve definujeme novou instanci třídy `MainWindow` třídy a přiřaďte ho ke kontroleru základní okno `Window` vlastnost:

```csharp
CGRect contentRect = new CGRect (0, 0, 1000, 500);
base.Window = new MainWindow(contentRect, (NSWindowStyle.Titled | NSWindowStyle.Closable | NSWindowStyle.Miniaturizable | NSWindowStyle.Resizable), NSBackingStore.Buffered, false);
```

Jsme definovali umístění okna obrazovky s `CGRect`. Stejně jako souřadnicový systém okně na obrazovce definuje (0,0) jako nižší levém dolním rohu. V dalším kroku jsme definovali styl okna pomocí **nebo** operátor jak kombinací dvou nebo více `NSWindowStyle` funkce:

```csharp
... (NSWindowStyle.Titled | NSWindowStyle.Closable | NSWindowStyle.Miniaturizable | NSWindowStyle.Resizable) ...
``` 

Následující `NSWindowStyle` funkce jsou k dispozici:

- **Bez okrajů** -okno bude mít žádné ohraničení.
- **S názvem** -okno bude mít záhlaví.
- **Uzavíratelných** -okno obsahuje tlačítko Zavřít a je možné uzavřít.
- **Miniaturizable** -okna má tlačítko Miniaturize a může být minimální.
- **S možností změny velikosti** -okno bude mít na tlačítko Změnit velikost a být umožňující změnu velikosti.
- **Nástroj** -okna je okno Styl nástroj (panely).
- **DocModal** – Pokud je okno panelu, bude dokument modální místo modální systému. 
- **NonactivatingPanel** – Pokud je okno panelu, nebude se hlavní okno.
- **TexturedBackground** -okno bude mít texturované pozadí.
- **Bez měřítka** -nebude možné rozšířit okno.
- **UnifiedTitleAndToolbar** -oblastí nadpisu a panelu nástrojů okna bude připojena.
- **Hud** -okno se zobrazí jako z pohotového zobrazení panelu.
- **FullScreenWindow** -okně můžete zadat režim celé obrazovky.
- **FullSizeContentView** -okna zobrazení obsahu je za nadpis a nástrojů oblasti.

Zadejte poslední dvě vlastnosti _ukládání do vyrovnávací paměti typu_ pro okno a pokud bude třeba odložit kreslení okna. Další informace o `NSWindows`, najdete v tématu společnosti Apple [Úvod do Windows](https://developer.apple.com/library/mac/documentation/Cocoa/Conceptual/WinPanel/Introduction.html#//apple_ref/doc/uid/10000031-SW1) dokumentaci.

Navíc vzhledem k tomu, že okno není právě zvětšený ze souboru .storyboard nebo .xib, musíme simulovat v našem **MainWindowController.cs** voláním windows `AwakeFromNib` metoda:

```csharp
Window.AwakeFromNib ();
```

To vám umožní, že vám kód proti okno jenom jako standardní okno načtený ze souboru .storyboard nebo .xib.


### <a name="displaying-the-window"></a>Zobrazení okna

Souborem .storyboard nebo .xib odebrat a **MainWindow.cs** a **MainWindowController.cs** soubory upravit, které budete používat okna stejně jako všechny normálním vytvořený v okně Xcode je rozhraní tvůrce souborem .xib.

Následující se vytvořit novou instanci třídy okna a jeho řadiče a zobrazení okna na obrazovce:

```csharp
private MainWindowController mainWindowController;
...

mainWindowController = new MainWindowController ();
mainWindowController.Window.MakeKeyAndOrderFront (this);
```

Nyní pokud je aplikace spuštěna a několik počet kliknutí na tlačítko, následující bude zobrazena:

![Příklad aplikace spustit](xibless-ui-images/run01.png "příklad aplikaci spustit")


## <a name="adding-a-code-only-window"></a>Přidání pouze okno kódu

Pokud nám chcete přidat pouze kód, xibless okno na existující aplikaci Xamarin.Mac, klikněte pravým tlačítkem na projekt v **řešení Pad** a vyberte **přidat** > **nový soubor...** . V **nový soubor** dialogové okno Vybrat **Xamarin.Mac** > **kakao okno s řadiče**, jak je uvedeno dále:

![Přidávání nového řadiče okno](xibless-ui-images/add01.png "přidávání nového řadiče okna") 

Stejně jako dříve, My je odstraníme výchozí .storyboard nebo .xib soubor z projektu (v tomto případě **SecondWindow.xib**) a postupujte podle kroků v [přepínání a údržbu za účelem použití kódu](#Switching_a_Window_to_use_Code) část výše tak, aby pokrývalo definice uživatele systému Windows ke kódu.


## <a name="adding-a-ui-element-to-a-window-in-code"></a>Přidání prvku uživatelského rozhraní do okna v kódu

Zda okno vytvořená v kódu nebo načtený ze souboru .storyboard nebo .xib, může nastat situace, kdy chceme přidá prvek uživatelského rozhraní do okna z kódu. Příklad:

```csharp
var ClickMeButton = new NSButton (new CGRect (10, 10, 100, 30)){
    AutoresizingMask = NSViewResizingMask.MinYMargin
};
MyWindow.ContentView.AddSubview (ClickMeButton);
```

Výše uvedený kód vytvoří novou `NSButton` a přidává ji k `MyWindow` instance okno pro zobrazení. V podstatě prvků uživatelského rozhraní, které lze definovat v je Xcode rozhraní tvůrce v souboru .storyboard nebo .xib můžete vytvořit v kódu a zobrazí se v okně.


## <a name="defining-the-menu-bar-in-code"></a>Definování řádku nabídek v kódu

Kvůli aktuálním omezením v Xamarin.Mac není navrhované, že vytvoříte aplikaci Xamarin.Mac nabídky panelu –`NSMenuBar`– v kódu, ale i nadále používat **Main.storyboard** nebo **MainMenu.xib** souboru definujte ho. Ale nutné dodat, můžete přidávat a odebírat nabídky a položek nabídky v kódu jazyka C#.

Můžete třeba upravit **AppDelegate.cs** souboru a ujistěte se, `DidFinishLaunching` metoda vypadá takto:

```csharp
public override void DidFinishLaunching (NSNotification notification)
{
    mainWindowController = new MainWindowController ();
    mainWindowController.Window.MakeKeyAndOrderFront (this);

    // Create a Status Bar Menu
    NSStatusBar statusBar = NSStatusBar.SystemStatusBar;

    var item = statusBar.CreateStatusItem (NSStatusItemLength.Variable);
    item.Title = "Phrases";
    item.HighlightMode = true;
    item.Menu = new NSMenu ("Phrases");

    var address = new NSMenuItem ("Address");
    address.Activated += (sender, e) => {
        Console.WriteLine("Address Selected");
    };
    item.Menu.AddItem (address);

    var date = new NSMenuItem ("Date");
    date.Activated += (sender, e) => {
        Console.WriteLine("Date Selected");
    };
    item.Menu.AddItem (date);

    var greeting = new NSMenuItem ("Greeting");
    greeting.Activated += (sender, e) => {
        Console.WriteLine("Greetings Selected");
    };
    item.Menu.AddItem (greeting);

    var signature = new NSMenuItem ("Signature");
    signature.Activated += (sender, e) => {
        Console.WriteLine("Signature Selected");
    };
    item.Menu.AddItem (signature);
}
```

Výše vytvoří stavového řádku nabídky z kódu a zobrazí se při spuštění aplikace. Další informace o práci s nabídkami, najdete v tématu naše [nabídky](~/mac/user-interface/menu.md) dokumentaci.


## <a name="summary"></a>Souhrn

Tento článek trvá podrobné podívejte se na vytvoření uživatelského rozhraní aplikace Xamarin.Mac v kódu jazyka C# a pomocí Tvůrce rozhraní na Xcode .storyboard nebo .xib soubory.



## <a name="related-links"></a>Související odkazy

- [MacXibless (ukázka)](https://developer.xamarin.com/samples/mac/MacXibless/)
- [Windows](~/mac/user-interface/window.md)
- [Nabídky](~/mac/user-interface/menu.md)
- [systému macOS Human Interface Guidelines](https://developer.apple.com/macos/human-interface-guidelines/overview/themes/)
- [Úvod do systému Windows](https://developer.apple.com/library/content/documentation/Cocoa/Conceptual/WinPanel/Introduction.html)
