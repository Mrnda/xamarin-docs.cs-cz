---
title: Sestavení systému macOS moderní aplikace
description: Tento článek se zabývá několik tipy, funkce a techniky, které vývojář může použít k vytvoření aplikace moderní systému macOS v Xamarin.Mac.
ms.prod: xamarin
ms.assetid: F20EE590-246E-40EB-B309-D9D8C090C7F1
ms.technology: xamarin-mac
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/14/2017
ms.openlocfilehash: 4eb4ff4a9e4784d816e2cbe8734e0422573cad92
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/04/2018
ms.locfileid: "30787776"
---
# <a name="building-modern-macos-apps"></a>Sestavení systému macOS moderní aplikace

_Tento článek se zabývá několik tipy, funkce a techniky, které vývojář může použít k vytvoření aplikace moderní systému macOS v Xamarin.Mac._

<a name="Building-Modern-Looks-with-Modern-Views" />

## <a name="building-modern-looks-with-modern-views"></a>Vytváření moderní vypadá s moderní zobrazení

Moderní vzhled bude zahrnovat moderní vzhled okna a panelu nástrojů jako je například aplikace příklad vidíte níže:

[![](modern-cocoa-apps-images/content08.png "Příkladem moderní aplikace Mac uživatelského rozhraní")](modern-cocoa-apps-images/content08.png#lightbox)

<a name="Enabling-Full-Sized-Content-Views" />

### <a name="enabling-full-sized-content-views"></a>Povolení úplné velikosti zobrazení obsahu

K dosažení této vypadá v aplikaci Xamarin.Mac, bude nejvhodnější použít na vývojáře _úplné zobrazení obsahu velikost_, tj. obsah rozšiřuje pod oblastí nástroj a záhlaví a bude automaticky hranice pomocí systému macOS.

Chcete-li povolit tuto funkci v kódu, vytvořte vlastní třídu `NSWindowController` vypadá takto:

```csharp
using System;
using Foundation;
using AppKit;

namespace MacModern
{
    public partial class MainWindowController : NSWindowController
    {
        #region Constructor
        public MainWindowController (IntPtr handle) : base (handle)
        {
        }
        #endregion

        #region Override Methods
        public override void WindowDidLoad ()
        {
            base.WindowDidLoad ();

            // Set window to use Full Size Content View
            Window.StyleMask = NSWindowStyle.FullSizeContentView;
        }
        #endregion
    }
}
```

Tuto funkci lze také povolit v Xcode na rozhraní tvůrce výběrem okna a kontrola **úplné, velikost zobrazení obsahu**:

[![](modern-cocoa-apps-images/content01.png "Úpravy hlavní storyboard v Tvůrci rozhraní na Xcode")](modern-cocoa-apps-images/content01.png#lightbox)

Při použití úplný přehled velikost obsahu, vývojář možná muset posunout obsah pod oblastí panelu nástroj a název tak, aby konkrétní obsah (například popisky) není Vysuňte pod nimi.

K zkomplikovat tento problém, může mít oblasti nadpisu a panelu nástrojů dynamické výšky na základě akce, které uživatel právě provádí, verzi systému macOS uživatel nainstaloval nebo hardwaru Mac, který aplikace spuštěná.

V důsledku toho nebude fungovat jednoduše pevného kódování posun při rozložení uživatelského rozhraní. Vývojář muset dynamické přístup.

Apple má zahrnuté [klíč-hodnota pozorovatelné](~/mac/app-fundamentals/databinding.md#Observing_Value_Changes) `ContentLayoutRect` vlastnost `NSWindow` třídy pro získání aktuální oblast obsahu v kódu. Vývojář může tato hodnota slouží k ručně umístit požadované prvky. při změně oblast obsahu.

Lepší řešení je použití automatického rozložení a velikost třídy na pozici prvky uživatelského rozhraní v kódu nebo rozhraní tvůrce.

Kódu jako v následujícím příkladu je možné na pozici prvky uživatelského rozhraní pomocí třídy velikost a automatické rozložení v Kontroleru zobrazení aplikace:

```csharp
using System;
using AppKit;
using Foundation;

namespace MacModern
{
    public partial class ViewController : NSViewController
    {
        #region Computed Properties
        public NSLayoutConstraint topConstraint { get; set; }
        #endregion

        ...

        #region Override Methods
        public override void UpdateViewConstraints ()
        {
            // Has the constraint already been set?
            if (topConstraint == null) {
                // Get the top anchor point
                var contentLayoutGuide = ItemTitle.Window?.ContentLayoutGuide as NSLayoutGuide;
                var topAnchor = contentLayoutGuide.TopAnchor;

                // Found?
                if (topAnchor != null) {
                    // Assemble constraint and activate it
                    topConstraint = topAnchor.ConstraintEqualToAnchor (topAnchor, 20);
                    topConstraint.Active = true;
                }
            }

            base.UpdateViewConstraints ();
        }
        #endregion
    }
}
```

Tento kód vytvoří úložiště pro horní omezení, které se použijí pro popisek (`ItemTitle`) k zajištění, že není listu v oblasti nadpisu a panelu nástrojů:

```csharp
public NSLayoutConstraint topConstraint { get; set; }
```

Přepsáním řadiče zobrazení `UpdateViewConstraints` metody vývojář otestujte, pokud již byla vytvořena potřebné omezení a v případě potřeby vytvořte.

Pokud nové omezení musí být vytvořen, `ContentLayoutGuide` vlastnost okna přístupem a přetypovat na ovládací prvek, který musí být omezené `NSLayoutGuide`:

```csharp
var contentLayoutGuide = ItemTitle.Window?.ContentLayoutGuide as NSLayoutGuide;
```

Vlastnost TopAnchor `NSLayoutGuide` přistupuje a pokud je k dispozici, je použita k vytvoření nové omezení s požadovanou velikost odsazení a nové omezení přišla active provést:

```csharp
// Assemble constraint and activate it
topConstraint = topAnchor.ConstraintEqualToAnchor (topAnchor, 20);
topConstraint.Active = true;
```

<a name="Enabling-Streamlined-Toolbars" />

### <a name="enabling-streamlined-toolbars"></a>Povolení možnosti efektivní panely nástrojů

Normální systému macOS okno obsahuje standardní záhlaví v spustí na horním okraji okna. Pokud okno také obsahuje panel nástrojů, zobrazí se v této oblasti záhlaví:

[![](modern-cocoa-apps-images/content02.png "Standardním panelu nástrojů Mac")](modern-cocoa-apps-images/content02.png#lightbox)

Když použijete možnosti efektivní panel nástrojů, zmizí oblasti nadpisu a panelu nástrojů se přesune do pozice v záhlaví, vložené pomocí tlačítka Zavřít okno, minimalizovat a maximalizovat:

[![](modern-cocoa-apps-images/content03.png "Zjednodušená Mac panelu nástrojů")](modern-cocoa-apps-images/content03.png#lightbox)

Zjednodušená nástrojů se povolit přepsáním `ViewWillAppear` metodu `NSViewController` a díky tomu vypadat podobně jako následující:

```csharp
public override void ViewWillAppear ()
{
    base.ViewWillAppear ();

    // Enable streamlined Toolbars
    View.Window.TitleVisibility = NSWindowTitleVisibility.Hidden;
}
```

Tomu se obvykle používá u _materiálů uložených aplikace_ (aplikace v jednom okně) jako mapy, kalendáře, poznámky a předvolbách systému. 

<a name="Using-Accessory-View-Controllers" />

### <a name="using-accessory-view-controllers"></a>Pomocí řadiče příslušenství zobrazení

V závislosti na návrh aplikace vývojář také chtít doplnit záhlaví jsou oblasti s řadič zobrazení příslušenství, který se zobrazí vpravo pod oblastí panelu název nebo nástrojů zajistit závislé na kontextu ovládací prvky pro uživatele na základě aktivity se aktuálně zapojený do:

[![](modern-cocoa-apps-images/content04.png "Příklad řadiče zobrazení příslušenství")](modern-cocoa-apps-images/content04.png#lightbox)

Řadiče zobrazení příslušenství bude automaticky hranice a po změně velikosti v systému bez zásahu vývojáře.

Pokud chcete přidat řadič zobrazení příslušenství, postupujte takto:

1. V **Průzkumníku řešení**, dvakrát klikněte `Main.storyboard` soubor otevřete pro úpravy.
2. Přetáhněte **vlastní View Controller** do okna hierarchie: 

    [![](modern-cocoa-apps-images/content05.png "Přidávání nového řadiče zobrazení vlastní")](modern-cocoa-apps-images/content05.png#lightbox)
3. Rozložení příslušenství zobrazení pro uživatelské rozhraní: 

    [![](modern-cocoa-apps-images/content06.png "Navrhování nového zobrazení")](modern-cocoa-apps-images/content06.png#lightbox)
4. Vystavení příslušenství zobrazení jako **výstupu** a všemi ostatními **akce** nebo **výstupy** pro jeho uživatelského rozhraní: 

    [![](modern-cocoa-apps-images/content07.png "Přidání požadovaných výstupu")](modern-cocoa-apps-images/content07.png#lightbox)
5. Uložte změny.
6. Vraťte se k Visual Studio pro Mac synchronizovat změny.

Upravit `NSWindowController` vypadá takto:

```csharp
using System;
using Foundation;
using AppKit;

namespace MacModern
{
    public partial class MainWindowController : NSWindowController
    {
        #region Constructor
        public MainWindowController (IntPtr handle) : base (handle)
        {
        }
        #endregion

        #region Override Methods
        public override void WindowDidLoad ()
        {
            base.WindowDidLoad ();

            // Create a title bar accessory view controller and attach
            // the view created in Interface Builder
            var accessoryView = new NSTitlebarAccessoryViewController ();
            accessoryView.View = AccessoryViewGoBar;

            // Set the location and attach the accessory view to the
            // titlebar to be displayed
            accessoryView.LayoutAttribute = NSLayoutAttribute.Bottom;
            Window.AddTitlebarAccessoryViewController (accessoryView);

        }
        #endregion
    }
}
```

Klíčových bodů tohoto kódu je, kde zobrazení je nastaven na vlastní zobrazení, která byla definovaná v Tvůrci rozhraní a zveřejněné jako **výstupu**:

```csharp
accessoryView.View = AccessoryViewGoBar;
```

A `LayoutAttribute` , který definuje _kde_ příslušenství se zobrazí:

```csharp
accessoryView.LayoutAttribute = NSLayoutAttribute.Bottom;
```

Protože teď je plně lokalizované systému macOS, `Left` a `Right` `NSLayoutAttribute` vlastnosti jsou zastaralé a měl by být nahrazen `Leading` a `Trailing`.

<a name="Using-Tabbed-Windows" />

### <a name="using-tabbed-windows"></a>Pomocí systému Windows s kartami

Kromě toho může v systému macOS přidat řadiče zobrazení příslušenství do okna aplikace. Chcete-li například vytvořit – záložkami Windows, kde některé aplikace Windows jsou sloučeny do jednoho virtuálního okna:

[![](modern-cocoa-apps-images/content08.png "Příkladem okno s kartami Mac")](modern-cocoa-apps-images/content08.png#lightbox)

Obvykle vývojář bude nutné provést akci omezené použití Tabbed Windows ve svých aplikacích Xamarin.Mac, bude systém zpracovat automaticky následujícím způsobem:

- Windows bude automaticky – záložkami, kdy `OrderFront` metoda je volána.
- Systém Windows bude automaticky Untabbed, kdy `OrderOut` metoda je volána.
- V kódu všechny systémy windows Tabbed jsou stále považovány za "viditelné" ale žádné – přední krajní karty jsou skrytý na základě systému pomocí CoreGraphics.
- Použití `TabbingIdentifier` vlastnost `NSWindow` skupiny systému Windows společně na karty.
- Pokud se jedná `NSDocument` založené na aplikaci, některé z těchto funkcí povolí automaticky (například na tlačítko plus přidávané na posuvníku) bez nutnosti vývojáře akce.
- Jinou hodnotu než`NSDocument` aplikací můžete povolit tlačítko "plus" ve skupině kartě přidat nový dokument přepsáním `GetNewWindowForTab` metodu `NSWindowsController`.

Všechny vytvořené uvedení společně `AppDelegate` aplikace, které chtěli používat systém založený na kartách Windows může vypadat následovně:

```csharp
using AppKit;
using Foundation;

namespace MacModern
{
    [Register ("AppDelegate")]
    public class AppDelegate : NSApplicationDelegate
    {
        #region Computed Properties
        public int NewDocumentNumber { get; set; } = 0;
        #endregion

        #region Constructors
        public AppDelegate ()
        {
        }
        #endregion

        #region Override Methods
        public override void DidFinishLaunching (NSNotification notification)
        {
            // Insert code here to initialize your application
        }

        public override void WillTerminate (NSNotification notification)
        {
            // Insert code here to tear down your application
        }
        #endregion

        #region Custom Actions
        [Export ("newDocument:")]
        public void NewDocument (NSObject sender)
        {
            // Get new window
            var storyboard = NSStoryboard.FromName ("Main", null);
            var controller = storyboard.InstantiateControllerWithIdentifier ("MainWindow") as NSWindowController;

            // Display
            controller.ShowWindow (this);
        } 
        #endregion
    }
}
```

Kde `NewDocumentNumber` vlastnost uchovává informace o počtu nové dokumenty vytvořené a `NewDocument` metoda vytvoří nový dokument a zobrazí jej.

`NSWindowController` Pak může vypadat:

```csharp
using System;
using Foundation;
using AppKit;

namespace MacModern
{
    public partial class MainWindowController : NSWindowController
    {
        #region Application Access
        /// <summary>
        /// A helper shortcut to the app delegate.
        /// </summary>
        /// <value>The app.</value>
        public static AppDelegate App {
            get { return (AppDelegate)NSApplication.SharedApplication.Delegate; }
        }
        #endregion

        #region Constructor
        public MainWindowController (IntPtr handle) : base (handle)
        {
        }
        #endregion

        #region Public Methods
        public void SetDefaultDocumentTitle ()
        {
            // Is this the first document?
            if (App.NewDocumentNumber == 0) {
                // Yes, set title and increment
                Window.Title = "Untitled";
                ++App.NewDocumentNumber;
            } else {
                // No, show title and count
                Window.Title = $"Untitled {App.NewDocumentNumber++}";
            }
        }
        #endregion

        #region Override Methods
        public override void WindowDidLoad ()
        {
            base.WindowDidLoad ();

            // Prefer Tabbed Windows
            Window.TabbingMode = NSWindowTabbingMode.Preferred;
            Window.TabbingIdentifier = "Main";

            // Set default window title
            SetDefaultDocumentTitle ();

            // Set window to use Full Size Content View
            // Window.StyleMask = NSWindowStyle.FullSizeContentView;

            // Create a title bar accessory view controller and attach
            // the view created in Interface Builder
            var accessoryView = new NSTitlebarAccessoryViewController ();
            accessoryView.View = AccessoryViewGoBar;

            // Set the location and attach the accessory view to the
            // titlebar to be displayed
            accessoryView.LayoutAttribute = NSLayoutAttribute.Bottom;
            Window.AddTitlebarAccessoryViewController (accessoryView);

        }

        public override void GetNewWindowForTab (NSObject sender)
        {
            // Ask app to open a new document window
            App.NewDocument (this);
        }
        #endregion
    }
}
```

Kde statických `App` vlastnost poskytuje zástupce pro zajištění `AppDelegate`. `SetDefaultDocumentTitle` Metoda nastaví nový název dokumentů na základě počtu nové dokumenty vytvořené.

Následující kód, řekne systému macOS, který aplikace upřednostňuje použití karet a poskytuje řetězec, který umožňuje seskupit do karty aplikace systému Windows:

```csharp
// Prefer Tabbed Windows
Window.TabbingMode = NSWindowTabbingMode.Preferred;
Window.TabbingIdentifier = "Main";
```

A následující metodu přepsání přidá tlačítko plus na posuvníku, tím se vytvoří nový dokument při kliknutí na uživatelem:

```csharp
public override void GetNewWindowForTab (NSObject sender)
{
    // Ask app to open a new document window
    App.NewDocument (this);
}
```

<a name="Using-Core-Animation" />

### <a name="using-core-animation"></a>Použití jádra animace

Základní animace je modul vykreslování základní napájený grafiky, který je součástí systému macOS. Základní animace optimalizovaná využívat výhod GPU (grafiky zpracování Unit) k dispozici v systému macOS moderní hardwaru a systémem grafické operace na procesoru, které mohou být zpomaleny na počítač.

`CALayer`, Poskytuje základní animace, lze použít pro úlohy, jako je rychlé a plynulé posouvání a animací. Uživatelské rozhraní aplikace by se skládá z více dílčích zobrazení a vrstev a plně využít výhod základní animace.

A `CALayer` objekt poskytuje několik vlastností, které umožňuje vývojářům řídit, co se zobrazí na obrazovce pro uživatele, jako:

- `Content` -Může být `NSImage` nebo `CGImage` obsah vrstvy, která poskytuje.
- `BackgroundColor` -Nastaví barvu pozadí vrstvě jako `CGColor`
- `BorderWidth` -Nastaví šířku ohraničení.
- `BorderColor` -Nastaví barvu ohraničení.

Abyste mohli využívat základní grafické prvky v uživatelském rozhraní aplikace, musí používat _vrstvy zálohovaný_ zobrazení, která Apple naznačuje, že vývojář měli vždy povolit v zobrazení obsahu okně. Tímto způsobem všechny podřízené zobrazení automaticky zdědí zálohování vrstva i.

Kromě toho Apple doporučuje použití vrstvu založenou na zobrazení a přidání nového `CALayer` jako podvrstvu vzhledem k tomu, že systém bude automaticky zpracovávat několik požadované nastavení (jako jsou ty vyžadují sítnice zobrazení).

Vrstvy zálohování se dá nastavit podle nastavení `WantsLayer` z `NSView` k `true` nebo uvnitř Tvůrce rozhraní Xcode společnosti v části **zobrazení důsledky Inspector** kontrolou **základní animace vrstvy**:

[![](modern-cocoa-apps-images/content09.png "Inspector důsledky zobrazení")](modern-cocoa-apps-images/content09.png#lightbox)

<a name="Redrawing-Views-with-Layers" />

#### <a name="redrawing-views-with-layers"></a>Překreslování zobrazení s vrstvami

Jiné důležitým krokem při použití zobrazení zálohovaný vrstvy v aplikaci Xamarin.Mac se nastaví `LayerContentsRedrawPolicy` z `NSView` k `OnSetNeedsDisplay` v `NSViewController`. Příklad:

```csharp
public override void ViewWillAppear ()
{
    base.ViewWillAppear ();

    // Set the content redraw policy
    View.LayerContentsRedrawPolicy = NSViewLayerContentsRedrawPolicy.OnSetNeedsDisplay;
}
```

Pokud vývojář není nastavena tato vlastnost, zobrazení bude překreslen, při každé změně původu rámce, který není žádoucí z důvodů výkonu. S Tato vlastnost nastavena na `OnSetNeedsDisplay` Vývojář nutné ručně nastavit `NeedsDisplay` k `true` obsahu, který ho překreslit, ale vynutit.

Při zobrazení je označena jako nekonzistentní, systém kontroluje, zda `WantsUpdateLayer` vlastnosti zobrazení. Vrátí-li `true` potom `UpdateLayer` metoda je volána, jinak `DrawRect` aktualizovat obsah zobrazení je volána metoda zobrazení.

Společnost Apple má následující návrhy pro aktualizace zobrazení obsahu vyžádání:

- Apple upřednostní pomocí `UpdateLater` přes `DrawRect` vždy, když to možné, protože poskytuje nárůst významně zvýšit výkon.
- Použijte stejný `layer.Contents` pro prvky uživatelského rozhraní, které vypadají podobně jako.
- Apple také upřednostní vývojáři tvoří jejich uživatelské rozhraní pomocí standardní zobrazení, jako třeba `NSTextField`, znovu kdykoli je to možné.

Použít `UpdateLayer`, vytvořte vlastní třídu `NSView` a ujistěte se, kód vypadat následovně:

```csharp
using System;
using Foundation;
using AppKit;

namespace MacModern
{
    public partial class MainView : NSView
    {
        #region Computed Properties
        public override bool WantsLayer {
            get { return true; }
        }

        public override bool WantsUpdateLayer {
            get { return true; }
        }
        #endregion

        #region Constructor
        public MainView (IntPtr handle) : base (handle)
        {
        }
        #endregion

        #region Override Methods
        public override void DrawRect (CoreGraphics.CGRect dirtyRect)
        {
            base.DrawRect (dirtyRect);

        }

        public override void UpdateLayer ()
        {
            base.UpdateLayer ();

            // Draw view 
            Layer.BackgroundColor = NSColor.Red.CGColor;
        }
        #endregion
    }
}
```

<a name="Using-Modern-Drag-and-Drop" />

## <a name="using-modern-drag-and-drop"></a>Pomocí moderní – přetažení

K dispozici moderní přetažení prostředí pro uživatele, musí vývojář přijmout _přetáhněte vločkování_ ve své aplikaci operace přetažení. Přetáhněte Flocking je, kde každý jednotlivých soubor nebo položka přetažen původně se zobrazí jako jednotlivé element, který uživatel stále operaci přetažení hejn (skupina společně v rámci kurzor s počtem počet položek).

Pokud uživatel ukončí operaci přetažení, budou jednotlivé prvky Unflock a vrátit do jejich původního umístění.

Následující příklad kódu umožňuje přetahováním vločkování na vlastní zobrazení:

```csharp
using System;
using System.Collections.Generic;
using Foundation;
using AppKit;

namespace MacModern
{
    public partial class MainView : NSView, INSDraggingSource, INSDraggingDestination
    {
        #region Constructor
        public MainView (IntPtr handle) : base (handle)
        {
        }
        #endregion

        #region Override Methods
        public override void MouseDragged (NSEvent theEvent)
        {
            // Create group of string to be dragged
            var string1 = new NSDraggingItem ((NSString)"Item 1");
            var string2 = new NSDraggingItem ((NSString)"Item 2");
            var string3 = new NSDraggingItem ((NSString)"Item 3");

            // Drag a cluster of items
            BeginDraggingSession (new [] { string1, string2, string3 }, theEvent, this);
        }
        #endregion
    }
}
```

Bylo dosaženo flocking účinek odesláním každou položku tažením `BeginDraggingSession` metodu `NSView` jako samostatný prvek v poli.

Při práci se službou `NSTableView` nebo `NSOutlineView`, použijte `PastboardWriterForRow` metodu `NSTableViewDataSource` třída spustit operaci přetažení:

```csharp
using System;
using System.Collections.Generic;
using Foundation;
using AppKit;

namespace MacModern
{
    public class ContentsTableDataSource: NSTableViewDataSource
    {
        #region Constructors
        public ContentsTableDataSource ()
        {
        }
        #endregion

        #region Override Methods
        public override INSPasteboardWriting GetPasteboardWriterForRow (NSTableView tableView, nint row)
        {
            // Return required pasteboard writer
            ...
            
            // Pasteboard writer failed
            return null;
        }
        #endregion
    }
}
```

To umožňuje vývojáři poskytnout konkrétního `NSDraggingItem` pro každou položku v tabulce, která je právě přetáhli oproti starší metoda `WriteRowsWith` , zapisovat všechny řádky jako jednu skupinu na pracovní plochu.

Při práci s `NSCollectionViews`, znovu použít `PasteboardWriterForItemAt` metoda naproti tomu `WriteItemsAt` metoda při přetažení začíná.

Vývojář musí vždy vyhnout vkládání velkých souborů na pracovní ploše. Nový systému macOS Sierra, _souboru lišící_ umožňuje vývojářům umístit odkazy na základě souborů na pracovní ploše, která budou splněny později po dokončení operace odstranění pomocí nového uživatele `NSFilePromiseProvider` a `NSFilePromiseReceiver` třídy.

<a name="Using-Modern-Event-Tracking" />

## <a name="using-modern-event-tracking"></a>Pomocí sledování moderní událostí

Pro element uživatelského rozhraní (například `NSButton`) která byla přidaná do oblasti název nebo panel nástrojů, uživatel musí být možné ji aktivovat událost jako normální (například zobrazení automaticky otevíraném okně) a klikněte na element. Ale vzhledem k tomu, že položka se také v oblasti názvu nebo panel nástrojů, uživatel bude schopen klikněte a přetáhněte elementu, který chcete přesunout okno také.

K tomu v kódu, vytvořte vlastní třídu pro element (například `NSButton`) a přepsat `MouseDown` událostí následujícím způsobem:

```csharp
public override void MouseDown (NSEvent theEvent)
{
    var shouldCallSuper = false;

    Window.TrackEventsMatching (NSEventMask.LeftMouseUp, 2000, NSRunLoop.NSRunLoopEventTracking, (NSEvent evt, ref bool stop) => {
        // Handle event as normal
        stop = true;
        shouldCallSuper = true;
    });

    Window.TrackEventsMatching(NSEventMask.LeftMouseDragged, 2000, NSRunLoop.NSRunLoopEventTracking, (NSEvent evt, ref bool stop) => {
        // Pass drag event to window
        stop = true;
        Window.PerformWindowDrag (evt);
    });

    // Call super to handle mousedown
    if (shouldCallSuper) {
        base.MouseDown (theEvent);
    }
}
```

Tento kód používá `TrackEventsMatching` metodu `NSWindow` připojeného element uživatelského rozhraní k zachytávat `LeftMouseUp` a `LeftMouseDragged` události. Pro `LeftMouseUp` událostí, element uživatelského rozhraní odpoví jako normální. Pro `LeftMouseDragged` událostí, události je předán `NSWindow`na `PerformWindowDrag` metoda přesunout okně na obrazovce.

Volání `PerformWindowDrag` metodu `NSWindow` třída poskytuje následující výhody:

- Umožňuje pro okno chcete přesunout, i když je aplikace přestala (jako např. kdy zpracování hloubkové smyčky).
- Místo přepínání bude fungovat podle očekávání.
- Na panelu prostory se zobrazí jako normální.
- Okno uchycení a zarovnání fungovat normálně.

<a name="Using-Modern-Container-View-Controls" />

## <a name="using-modern-container-view-controls"></a>Použití ovládacích prvků moderní kontejner zobrazení

systému macOS Sierra poskytuje mnoho vylepšení moderní existující kontejner zobrazení ovládacích prvků k dispozici v předchozích verzích operačního systému.

<a name="Table View Enhancements" />

## <a name="table-view-enhancements"></a>Vylepšení zobrazení tabulky

Vývojáři měli vždycky používat nové `NSView` základě verzi ovládací prvky kontejneru zobrazení například `NSTableView`. Příklad:

```csharp
using System;
using System.Collections.Generic;
using Foundation;
using AppKit;

namespace MacModern
{
    public class ContentsTableDelegate : NSTableViewDelegate
    {
        #region Constructors
        public ContentsTableDelegate ()
        {
        }
        #endregion

        #region Override Methods
        public override NSView GetViewForItem (NSTableView tableView, NSTableColumn tableColumn, nint row)
        {
            // Build new view
            var view = new NSView ();
            ...

            // Return the view representing the item to display
            return view;
        }
        #endregion
    }
}
```

To umožňuje vlastní akce řádek tabulky má být přiřazena k dané řádky v tabulce (například práva k odstranění řádku k načtení). Chcete-li toto chování, přepsat `RowActions` metodu `NSTableViewDelegate`:

```csharp
using System;
using System.Collections.Generic;
using Foundation;
using AppKit;

namespace MacModern
{
    public class ContentsTableDelegate : NSTableViewDelegate
    {
        #region Constructors
        public ContentsTableDelegate ()
        {
        }
        #endregion

        #region Override Methods
        public override NSView GetViewForItem (NSTableView tableView, NSTableColumn tableColumn, nint row)
        {
            // Build new view
            var view = new NSView ();
            ...

            // Return the view representing the item to display
            return view;
        }

        public override NSTableViewRowAction [] RowActions (NSTableView tableView, nint row, NSTableRowActionEdge edge)
        {
            // Take action based on the edge
            if (edge == NSTableRowActionEdge.Trailing) {
                // Create row actions
                var editAction = NSTableViewRowAction.FromStyle (NSTableViewRowActionStyle.Regular, "Edit", (action, rowNum) => {
                    // Handle row being edited
                    ...
                });

                var deleteAction = NSTableViewRowAction.FromStyle (NSTableViewRowActionStyle.Destructive, "Delete", (action, rowNum) => {
                    // Handle row being deleted
                    ...
                });

                // Return actions
                return new [] { editAction, deleteAction };
            } else {
                // No matching actions
                return null;
            }
        }
        #endregion
    }
}
```

Statické `NSTableViewRowAction.FromStyle` se používá k vytvoření nové tabulky řádek akce styly následující:

- `Regular` -Provede standardní-destruktivní akce, například upravit obsah na řádek.
- `Destructive` -Provede destruktivní akce, jako je například odstranit řádek z tabulky. Tato akce bude vykreslen s červeným pozadím.

<a name="Scroll-View-Enhancements" />

## <a name="scroll-view-enhancements"></a>Posuňte zobrazení vylepšení 

Při použití posuňte zobrazení (`NSScrollView`) přímo, nebo jako součást jiného ovládacího prvku (například `NSTableView`), můžete v oblasti nadpisu a panelu nástrojů v aplikaci Xamarin.Mac pomocí moderní vzhled a zobrazení posuňte obsah posuňte zobrazení.

První položka v oblasti posuňte zobrazení obsahu v důsledku toho může být částečně skryté, název a panelu nástrojů oblastí.

Chcete-li tento problém, Apple přidala dvě nové vlastnosti do `NSScrollView` třídy:

- `ContentInsets` – Umožňuje vývojáři k poskytování `NSEdgeInsets` objekt definování posun, které se použijí na začátek posuňte zobrazení.
- `AutomaticallyAdjustsContentInsets` -Pokud `true` posuňte zobrazení automaticky zpracuje `ContentInsets` pro vývojáře.

Pomocí `ContentInsets` Vývojář můžete upravit začátek posuňte zobrazení umožňující zahrnutí příslušenství, jako:

- Řazení indikátor jako uvedeno v aplikaci Mail.
- Pole hledání.
- Tlačítko aktualizace nebo aktualizace.

<a name="Auto-Layout-and-Localization-in-Modern-Apps" />

## <a name="auto-layout-and-localization-in-modern-apps"></a>Automatické rozložení a lokalizace v moderní aplikace

Apple má několik technologií součástí Xcode, který umožňuje vývojářům snadno vytvářet mezinárodní systému macOS aplikace. Xcode teď umožňuje vývojáři oddělení uživatelsky orientovaný textu od návrhu aplikace uživatelské rozhraní v jeho Storyboard soubory a poskytuje nástroje k udržování toto oddělení, pokud se změní uživatelského rozhraní.

Další informace najdete v tématu společnosti Apple [mezinárodní prostředí a lokalizace průvodce](https://developer.apple.com/library/content/documentation/MacOSX/Conceptual/BPInternational/InternationalizingYourUserInterface/InternationalizingYourUserInterface.html).

<a name="Implementing-Base-Internationalization" />

### <a name="implementing-base-internationalization"></a>Implementace základní internacionalizace 

Implementací základní internacionalizace nabízejí vývojář jednoho souboru scénáře představují uživatelském rozhraní aplikace a oddělit všechny řetězce zobrazující se uživatelům. 

Při vytváření vývojář počáteční Storyboard souboru (nebo souborů), zadejte uživatelské rozhraní aplikace, se budou vytvořeny v základní internacionalizace (jazyk, který mluví Vývojář).

Vývojáři dále můžete exportovat, lokalizace a základní internacionalizace řetězce (v návrhu Storyboard uživatelského rozhraní), které by bylo možné převést do různých jazyků.

Později tyto lokalizace lze importovat a Xcode vygeneruje řetězec jazykové soubory pro scénáře.

<a name="Implementing-Auto-Layout-to-Support-Localization" />

### <a name="implementing-auto-layout-to-support-localization"></a>Implementace automatického rozložení pro podporu lokalizace

Protože lokalizované verze řetězce hodnoty může významně různé velikosti nebo čtení směr, vývojář, používejte automatické rozložení poloha a velikost uživatelské rozhraní aplikace v souboru scénáře.

Apple navrhnout, následujícím způsobem:

- **Odebrat pevnou šířku omezení** -všechna zobrazení založený na textu má být povoleno se změnit velikost na základě jejich obsahu. Pevná šířka zobrazení Oříznout obsah v určitém jazyce.
- **Použít vnitřní velikosti obsahu** - ve výchozím nastavení založený na textu zobrazení bude automaticky nastavit podle jejich obsahu. Pro zobrazení založený na textu, které nejsou správně Změna velikosti, vyberte je v Xcode na rozhraní tvůrce a potom vyberte **upravit** > **velikost pro nedostatek místa**.
- **Použít úvodní a koncové atributy** – protože můžete změnit směr textu založené na jazyce uživatele, použijte nové `Leading` a `Trailing` omezení atributy oproti existující `Right` a `Left` atributy. `Leading` a `Trailing` automaticky upraví podle směru jazyky.
- **Zobrazení kódu PIN k přiléhající zobrazení** – to umožňuje zobrazení, která změňte umístění a velikost mění v reakci na jazyku vybranému na zobrazení je obcházet.
- **Není nastavený Windows minimální a maximální velikosti** -povolit Windows, chcete-li změnit velikost jako jazyk vybraný změní jejich obsahu oblasti.
- **Testování rozložení změny neustále** – během vývoje v aplikaci musí být neustále testovány v různých jazycích. Najdete v článku společnosti Apple [testování mezinárodní vaše aplikace](https://developer.apple.com/library/content/documentation/MacOSX/Conceptual/BPInternational/TestingYourInternationalApp/TestingYourInternationalApp.html#//apple_ref/doc/uid/10000171i-CH7-SW1) dokumentace pro další podrobnosti.
- **Použít NSStackViews pro kód Pin zobrazení společně**  -  `NSStackViews` umožňuje jejich obsah a posunutí růst způsoby předvídatelný a obsah, když změníte velikost podle jazyk vybraný.

<a name="Localizing-in-Xcodes-Interface-Builder" />

### <a name="localizing-in-xcodes-interface-builder"></a>Lokalizace v Tvůrci rozhraní na Xcode

Apple poskytl několik funkcí v Tvůrci rozhraní Xcode je, že vývojář použít při návrhu nebo úpravách aplikace uživatelského rozhraní pro podporu lokalizace. **Směr textové** části **atribut Inspector** umožňuje vývojáři poskytují pomocné parametry v tom, jak by měla použít a aktualizována v vyberte zobrazení založený na textu směr (například `NSTextField`):

[![](modern-cocoa-apps-images/content10.png "Směr textové možnosti")](modern-cocoa-apps-images/content10.png#lightbox)

Existují tři možné hodnoty pro **směr textové**:

- **Fyzická** – rozložení je založené na řetězci přiřazenou ovládacímu prvku.
- **Zleva doprava** – rozložení je vždy nuceno zprava doleva.
- **Zprava doleva** – rozložení je vždy nuceno zprava doleva.

Existují dvě možné hodnoty pro **rozložení**:

- **Zleva doprava** – rozložení je vždy zleva doprava.
- **Zprava doleva** – rozložení je vždy zprava doleva.

Obvykle tyto nesmí změnit, dokud se vyžaduje konkrétní souvislost.

**Zrcadlení** vlastnost říká systému k převrácení vlastnosti konkrétní ovládacích prvků (jako je například umístění bitové kopie buněk). Má tři možné hodnoty:

- **Automaticky** -pozice bude automaticky měnit v závislosti na směru jazyk vybraný.
- **V pravém na levé straně rozhraní** -pozici změní jenom v práva k levé založené na jazyce.
- **Nikdy** -pozice se nikdy nezmění.

Pokud má zadanou Vývojář **Center**, **do bloku** nebo **úplné** zarovnání na obsah zobrazení založený na textu, tyto nebude nikdy přetočený na základě jazyk vybraný.

Před systému macOS Sierra nebude automaticky zrcadlí ovládací prvky vytvořené v kódu. Vývojář se musel zpracování zrcadlení pomocí kódu takto:

```csharp
public override void ViewDidLoad ()
{
    base.ViewDidLoad ();

    // Setting a button's mirroring based on the layout direction
    var button = new NSButton ();
    if (button.UserInterfaceLayoutDirection == NSUserInterfaceLayoutDirection.LeftToRight) {
        button.Alignment = NSTextAlignment.Right;
        button.ImagePosition = NSCellImagePosition.ImageLeft;
    } else {
        button.Alignment = NSTextAlignment.Left;
        button.ImagePosition = NSCellImagePosition.ImageRight;
    }
}
```

Kde `Alignment` a `ImagePosition` je nastaveno na základě `UserInterfaceLayoutDirection` ovládacího prvku.

systému macOS Sierra přidá několik nové usnadnění práce konstruktorů (prostřednictvím statických `CreateButton` metoda), proveďte několik parametrů (například název, Image a akce) a bude automaticky zrcadlení správně. Příklad:

```csharp
var button2 = NSButton.CreateButton (myTitle, myImage, () => {
    // Take action when the button is pressed
    ...
});
```

<a name="Using-System-Appearances" />

## <a name="using-system-appearances"></a>Pomocí systému vzhled

Moderní systému macOS aplikace může přijmout nové tmavý vzhled rozhraní, která funguje dobře pro vytváření, úpravy nebo prezentační aplikace bitové kopie:

[![](modern-cocoa-apps-images/content11.png "Příkladem tmavý okno uživatelského rozhraní Mac")](modern-cocoa-apps-images/content11.png#lightbox)

Tento krok můžete provést přidáním jeden řádek kódu, než se zobrazí okno. Příklad:

```csharp
using System;
using AppKit;
using Foundation;

namespace MacModern
{
    public partial class ViewController : NSViewController
    {
        ...
    
        #region Override Methods
        public override void ViewWillAppear ()
        {
            base.ViewWillAppear ();

            // Apply the Dark Interface Appearance
            View.Window.Appearance = NSAppearance.GetAppearance (NSAppearance.NameVibrantDark);

            ...
        }
        #endregion
    }
}
```

Statické `GetAppearance` metodu `NSAppearance` třída se používá k získání pojmenované vzhled ze systému (v tomto případě `NSAppearance.NameVibrantDark`).

Společnost Apple má následující návrhy pro používání vzhledy systému:

- Dáváte přednost pojmenované barvy přes pevně zakódované hodnoty (například `LabelColor` a `SelectedControlColor`).
- Použijte styl standardního ovládacího prvku systému, kde je to možné.

Systému macOS aplikaci, která používá objevuje systému budou automaticky fungovat správně pro uživatele, kteří mají povolené funkce pro usnadnění přístupu z aplikace předvolbách systému. V důsledku toho Apple naznačuje, že vývojáři měli vždycky používat systém vzhledy ve svých aplikacích systému macOS.

<a name="Designing-UIs-with-Storyboards" />

## <a name="designing-uis-with-storyboards"></a>Navrhování uživatelská rozhraní s scénářů

Scénářů umožňuje vývojářům nejen návrhu toku jednotlivé elementy, které provést až uživatelské rozhraní aplikace, ale na vizualizovat a navrhnout uživatelského rozhraní a hierarchii dané elementů.

Řadiče umožňuje vývojářům shromažďovat elementy do jednotky složení a Segues abstraktní a odebrat typické "dohledání kód" pro přesunutí v celé hierarchii zobrazení potřeba:

[![](modern-cocoa-apps-images/content12.png "Úpravy uživatelského rozhraní v Tvůrci rozhraní na Xcode")](modern-cocoa-apps-images/content12.png#lightbox)

Další informace najdete v tématu naše [Úvod do scénářů](~/mac/platform/storyboards/index.md) dokumentaci.

Existuje velký počet instancí, kde bude daný scény definované v scénáře vyžadují data z předchozí scény v hierarchii zobrazení. Společnost Apple má následující návrhy pro předávání informací mezi scény:

- Data závislosti měli vždy sebe dolů prostřednictvím hierarchie.
- Vyhněte hardcoding strukturální závislosti uživatelského rozhraní, jak je to omezuje možnost uživatelského rozhraní.
- Pomocí rozhraní jazyka C# lze zadat obecné data závislosti.

View Controller, který funguje jako zdroj Segue, můžete přepsat `PrepareForSegue` metoda a proveďte všechny inicializace požadována (například předávání dat) před Segue se spustí zobrazíte cílového řadiče zobrazení. Příklad:

```csharp
public override void PrepareForSegue (NSStoryboardSegue segue, NSObject sender)
{
    base.PrepareForSegue (segue, sender);

    // Take action based on Segue ID
    switch (segue.Identifier) {
    case "MyNamedSegue":
        // Prepare for the segue to happen
        ...
        break;
    }
}
```

Další informace najdete v tématu naše [Segues](~/mac/platform/storyboards/indepth.md#Segues) dokumentaci.

<a name="Propagating-Actions" />

## <a name="propagating-actions"></a>Šíření akce

Podle návrhu aplikace systému macOS, může nastat situace, když obslužná rutina doporučené akce v ovládacím prvku uživatelského rozhraní je možné jinde v hierarchii uživatelského rozhraní. To se obvykle stává nabídky a položky nabídky, které za provozu ve svých vlastních scény oddělené od zbytku uživatelském rozhraní aplikace.

Vývojář tuto situaci můžete vyřešit, můžete vytvořit vlastní akce a předat akce řetězem respondér. Další informace najdete v tématu naše [práce s akcí okna Vlastní](~/mac/user-interface/menu.md) dokumentaci.

<a name="Modern-Mac-Features" />

## <a name="modern-mac-features"></a>Funkce moderní Mac

Apple použila několik funkcí zobrazující se uživatelům v systému macOS Sierra umožňuje vývojářům co nejlépe platformy Mac, jako například:

- **NSUserActivity** – to umožňuje aplikaci k popisu aktivity, která je aktuálně součástí uživatele. `NSUserActivity` byla původně vytvořena pro podporu, aby HandOff, kde může být aktivitu spustit na jednom zařízení uživatele zachyceny a pokračoval na jiném zařízení. `NSUserActivity` funguje stejně v systému macOS jak v iOS, naleznete v tématu naše [Úvod k předání](~/ios/platform/handoff.md) iOS dokumentace pro další podrobnosti.
- **Siri na Mac** -Siri používá aktuální aktivita (`NSUserActivity`) k poskytování kontext, který má uživatel může vydávat příkazy.
- **Stav obnovení** – Pokud uživatel ukončí aplikaci v systému macOS a pak novější relaunches ho, bude aplikace automaticky vrácen do předchozího stavu. Vývojář můžete použít rozhraní API pro obnovení stavu ke kódování a obnovit stavy přechodný uživatelského rozhraní, před uživateli se zobrazí uživatelské rozhraní. Pokud je aplikace `NSDocument` na základě, obnovení stavu je automaticky zpracována. Chcete-li povolit obnovení stavu pro jinou hodnotu než`NSDocument` aplikací, nastavte `Restorable` z `NSWindow` třídy k `true`.
- **Dokumenty v cloudu** -dřív systému macOS Sierra aplikace museli výslovně vyslovení souhlasu s práce s dokumenty v uživatele Icloudu jednotky. V systému macOS Sierra uživatele na **plochy** a **dokumenty** složek může synchronizovat s jejich Icloudu jednotky automaticky v systému. V důsledku toho mohou být odstraněny místní kopie dokumentů pro uvolnění místa na počítači uživatele. `NSDocument` Tato změna bude zpracována automaticky aplikací. Všechny ostatní typy aplikací budou muset použít `NSFileCoordinator` synchronizaci čtení a zápis dokumentů.

<a name="Summary" />

## <a name="summary"></a>Souhrn

Tento článek má zahrnutých několik tipy, funkce a techniky, které vývojář může použít k vytvoření aplikace moderní systému macOS v Xamarin.Mac.



## <a name="related-links"></a>Související odkazy

- [Ukázky systému macOS](https://developer.xamarin.com/samples/mac/)
