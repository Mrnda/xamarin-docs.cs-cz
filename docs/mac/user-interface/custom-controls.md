---
title: "Vytváření vlastních ovládacích prvků"
description: "Tento článek popisuje, jak vytvořit vlastní ovládací prvky a pracovat s nimi v Tvůrci rozhraní."
ms.topic: article
ms.prod: xamarin
ms.assetid: 004534B1-5AEE-452C-BBBE-8C2673FD49B7
ms.technology: xamarin-mac
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/14/2017
ms.openlocfilehash: 3ea88810384dfe8b1a08080953db19caddf25d6a
ms.sourcegitcommit: 0fdb243b46cf21be47584900805cadcd077121bf
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 03/12/2018
---
# <a name="creating-custom-controls"></a>Vytváření vlastních ovládacích prvků

_Tento článek popisuje, jak vytvořit vlastní ovládací prvky a pracovat s nimi v Tvůrci rozhraní._

Při práci s C# a rozhraní .NET v aplikaci Xamarin.Mac, máte přístup ke stejné uživatelské ovládací prvky, vývojář práce *jazyka Objective-C*, *Swift* a *Xcode* nemá . Protože Xamarin.Mac integruje přímo s Xcode, můžete na Xcode _rozhraní tvůrce_ vytvořit a udržovat vaše uživatelské ovládací prvky (nebo je můžete také vytvořit přímo v kódu jazyka C#).

Zatímco systému macOS poskytuje širokou řadu předdefinovaných uživatelské ovládací prvky, může být pokusů, které je potřeba vytvořit vlastní ovládací prvek zajistit, že funkce není zadaný se na pole nebo tak, aby odpovídaly vlastní motiv uživatelského rozhraní (například herní rozhraní).

[![](custom-controls-images/intro01.png "Příklad vlastního ovládacího prvku uživatelského rozhraní")](custom-controls-images/intro01.png#lightbox)

V tomto článku vám nabídneme základní informace o vytváření opakovaně použitelných vlastní uživatelské rozhraní ovládací prvky v aplikaci Xamarin.Mac. Vysoce navržený na spolupracovat [Hello, Mac](~/mac/get-started/hello-mac.md) článek nejprve, konkrétně [Úvod do Xcode a rozhraní tvůrce](~/mac/get-started/hello-mac.md#Introduction_to_Xcode_and_Interface_Builder) a [výstupy a akce](~/mac/get-started/hello-mac.md#Outlets_and_Actions) oddíly, jak se popisuje klíčové koncepty a techniky, které budeme používat v tomto článku.

Můžete chtít podívejte se na [třídy vystavení jazyka C# nebo metody jazyka Objective-C](~/mac/internals/how-it-works.md) části [Xamarin.Mac Internals](~/mac/internals/how-it-works.md) dokumentu taky vysvětluje `Register` a `Export` příkazy používá k navázání tříd jazyka C# jazyka Objective-C objekty a prvky uživatelského rozhraní.

<a name="Introduction-to-Outline-Views" />

## <a name="introduction-to-custom-controls"></a>Úvod do vlastní ovládací prvky

Jak jsme uvedli výše, může být časy, kdy je potřeba vytvořit opakovaně použitelného vlastní uživatelské rozhraní ovládacího prvku poskytují jedinečné funkce uživatelského rozhraní aplikace Xamarin.Mac nebo vytvořte vlastní motiv uživatelského rozhraní (například herní rozhraní).

V těchto situacích můžete snadno dědit z `NSControl` a vytvořit vlastní nástroj, který lze buď přidat do uživatelského rozhraní vaší aplikace prostřednictvím kódu C# nebo prostřednictvím rozhraní tvůrce pro Xcode. Dědění ze `NSControl` všechny standardní funkce, které je integrované uživatelské rozhraní pro ovládací prvek bude mít automaticky vlastní ovládací prvek (například `NSButton`).

Pokud vaše vlastní ovládací prvek uživatelské rozhraní zobrazí jenom informace (např. vytváření vlastních grafů a grafický nástroj), můžete chtít dědí `NSView` místo `NSControl`.

Základní kroky pro vytvoření vlastního ovládacího prvku bez ohledu na to, jaké základní třída se používá, je stejný.

V tohoto článku je vytvořte vlastní komponenty překlopit přepínače, která poskytuje jedinečný motiv uživatelské rozhraní a příklady vytváření plně funkční vlastního uživatelského rozhraní ovládacího prvku.

<a name="Building-the-Custom-Control" />

## <a name="building-the-custom-control"></a>Vytvoření vlastního ovládacího prvku

Vzhledem k tomu, že vlastní ovládací prvek vytváříme bude reagovat na vstup uživatele (levé tlačítko myší), přidáme dědění z `NSControl`. Tímto způsobem se naše vlastního ovládacího prvku automaticky mají všechny standardní funkce, které je integrované uživatelské rozhraní pro ovládací prvek a reagovat jako standardní systému macOS ovládacího prvku.

V sadě Visual Studio pro Mac otevřete projekt Xamarin.Mac, který chcete vytvořit vlastní ovládací prvek uživatelského rozhraní pro (nebo vytvořte novou). Přidání nové třídy a pojmenujte ji `NSFlipSwitch`:

[![](custom-controls-images/custom01.png "Přidání nové třídy")](custom-controls-images/custom01.png#lightbox)

Potom upravte `NSFlipSwitch.cs` třídy a nastavit jej vypadat třeba takto:

```csharp
using Foundation;
using System;
using System.CodeDom.Compiler;
using AppKit;
using CoreGraphics;

namespace MacCustomControl
{
    [Register("NSFlipSwitch")]
    public class NSFlipSwitch : NSControl
    {
        #region Private Variables
        private bool _value = false;
        #endregion

        #region Computed Properties
        public bool Value {
            get { return _value; }
            set {
                // Save value and force a redraw
                _value = value;
                NeedsDisplay = true;
            }
        }
        #endregion

        #region Constructors
        public NSFlipSwitch ()
        {
            // Init
            Initialize();
        }

        public NSFlipSwitch (IntPtr handle) : base (handle)
        {
            // Init
            Initialize();
        }

        [Export ("initWithFrame:")]
        public NSFlipSwitch (CGRect frameRect) : base(frameRect) {
            // Init
            Initialize();
        }

        private void Initialize() {
            this.WantsLayer = true;
            this.LayerContentsRedrawPolicy = NSViewLayerContentsRedrawPolicy.OnSetNeedsDisplay;
        }
        #endregion

        #region Draw Methods
        public override void DrawRect (CGRect dirtyRect)
        {
            base.DrawRect (dirtyRect);

            // Use Core Graphic routines to draw our UI
            ...

        }
        #endregion

        #region Private Methods
        private void FlipSwitchState() {
            // Update state
            Value = !Value;
        }
        #endregion

    }
}
```

Nejprve si všimněte si o našem vlastní třídy, v tom, že jsme se dědí z `NSControl` a pomocí **zaregistrovat** příkaz vystavit Tato třída jazyka Objective-C a na Xcode Tvůrce rozhraní:

```csharp
[Register("NSFlipSwitch")]
public class NSFlipSwitch : NSControl
```

V následujících částech provedeme podívejte se na zbytek ve výše uvedeném kódu podrobně.

<a name="Tracking-the-Controls-State" />

### <a name="tracking-the-controls-state"></a>Sledování stavu ovládacího prvku

Vzhledem k tomu, že je naše vlastní ovládací prvek přepínač, potřebujeme způsob, jak sledovat stav On nebo vypnout přepínače. Jsme zpracovat, s následujícím kódem v `NSFlipSwitch`:

```csharp
private bool _value = false;
...

public bool Value {
    get { return _value; }
    set {
        // Save value and force a redraw
        _value = value;
        NeedsDisplay = true;
    }
}
```

Při změně stavu přepínače, potřebujeme způsob, jak aktualizovat uživatelského rozhraní. Provedeme to vynucením ovládacího prvku na ho překreslit jeho uživatelském rozhraní s `NeedsDisplay = true`.

Podle potřeby naše řízení více prostředků, jeden zapnutí nebo vypnutí stavu (například více stavu přepínač s 3 pozice), může mít použili jsme **výčtu** ke sledování stavu. Pro náš příklad jednoduchou **bool** provede.

Jsme přidali i Pomocná metoda pro odkládacího souboru stavu přepínat mezi zapnout a vypnout:

```csharp
private void FlipSwitchState() {
    // Update state
    Value = !Value;
}
```

Později jsme budete rozbalte Tato pomocná třída k informování volající, kdy došlo ke změně stavu přepínače.

<a name="Drawing-the-Controls-Interface" />

### <a name="drawing-the-controls-interface"></a>Kreslení rozhraní ovládacího prvku

Jsme budete používat jádro grafické kreslení rutiny k vykreslení uživatelského rozhraní naše vlastního ovládacího prvku za běhu. Můžete provést, jsme musíme zapnout vrstev pro naše řízení. Provedeme to pomocí následující privátní metody:

```csharp
private void Initialize() {
    this.WantsLayer = true;
    this.LayerContentsRedrawPolicy = NSViewLayerContentsRedrawPolicy.OnSetNeedsDisplay;
}
```

Tato metoda je volána z každé z konstruktorů ovládacího prvku zajistit, aby byl správně nakonfigurován ovládacího prvku. Příklad:

```csharp
public NSFlipSwitch (IntPtr handle) : base (handle)
{
    // Init
    Initialize();
}
```

V dalším kroku musíme přepsat `DrawRect` metoda a přidat základní grafické rutiny k vykreslení ovládacího prvku:

```csharp
public override void DrawRect (CGRect dirtyRect)
{
    base.DrawRect (dirtyRect);

    // Use Core Graphic routines to draw our UI
    ...

}
```

Jsme budete mít úpravě vizuální reprezentace pro ovládací prvek při jeho stav se změní (například přechod z **na** k **vypnout**). Kdykoli se změny stavu, můžeme použít `NeedsDisplay = true` příkaz pro ovládací prvek donutit ho překreslit pro nový stav.

<a name="Responding-to-User-Input" />

### <a name="responding-to-user-input"></a>Reagovat na vstup uživatele

Existují dva základní způsobem, aby obsahovalo vstupu uživatele na našem vlastního ovládacího prvku: **přepsat rutiny zpracování myši** nebo **gesto rozpoznávání**. Jakou metodu používáme, budou založeny na funkci vyžadovanou společností Microsoft.

> [!IMPORTANT]
> Pro všechny vlastní ovládací prvek vytvoříte, měli byste použít buď **přepsání metody** _nebo_ **gesto rozpoznávání**, ale nikoli pro obě zároveň čas jejich může dojít ke konfliktu mezi sebou.

<a name="Summary" />

#### <a name="handling-user-input-with-override-methods"></a>Zpracování uživatelského vstupu pomocí přepsání metod

Objekty, které dědí od `NSControl` (nebo `NSView`) obsahuje několik přepsání metody pro zpracování myši nebo klávesové vstup. Pro náš příklad řízení, chceme překlopit stavu přepínat mezi **na** a **vypnout** když uživatel klikne na ovládací prvek s levým tlačítkem myši. Nyní můžete přidat následující přepsání metody `NSFliwSwitch` třídy pro zpracování toto:

```csharp
#region Mouse Handling Methods
// --------------------------------------------------------------------------------
// Handle mouse with Override Methods.
// NOTE: Use either this method or Gesture Recognizers, NOT both!
// --------------------------------------------------------------------------------
public override void MouseDown (NSEvent theEvent)
{
    base.MouseDown (theEvent);

    FlipSwitchState ();
}

public override void MouseDragged (NSEvent theEvent)
{
    base.MouseDragged (theEvent);
}

public override void MouseUp (NSEvent theEvent)
{
    base.MouseUp (theEvent);
}

public override void MouseMoved (NSEvent theEvent)
{
    base.MouseMoved (theEvent);
}
## endregion
```

Ve výše uvedeném kódu říkáme `FlipSwitchState` – metoda (definovanou výše) se překlopit On nebo vypnout stav přepínače v `MouseDown` metoda. Tato akce vynutí taky ovládacího prvku překreslit tak, aby odrážela aktuální stav.

<a name="Handling-User-Input-with-Gesture-Recognizers" />

#### <a name="handling-user-input-with-gesture-recognizers"></a>Zpracování uživatelského vstupu pomocí nástroje pro rozpoznávání gesto

Volitelně můžete použít nástroje pro rozpoznávání gesto pro zpracování uživateli interakci s ovládacím prvkem. Odebrat přepsání přidané v předchozím kroku, upravte `Initialize` metodu a nastavit jej vypadat třeba takto:

```csharp
private void Initialize() {
    this.WantsLayer = true;
    this.LayerContentsRedrawPolicy = NSViewLayerContentsRedrawPolicy.OnSetNeedsDisplay;

    // --------------------------------------------------------------------------------
    // Handle mouse with Gesture Recognizers.
    // NOTE: Use either this method or the Override Methods, NOT both!
    // --------------------------------------------------------------------------------
    var click = new NSClickGestureRecognizer (() => {
        FlipSwitchState();
    });
    AddGestureRecognizer (click);
}
```

Tady vytváříme novou `NSClickGestureRecognizer` a volání naše `FlipSwitchState` metoda změny stavu na přepínač, když uživatel klikne na jej s levým tlačítkem myši. `AddGestureRecognizer (click)` Metoda pro rozpoznávání gesto přidá do ovládacího prvku.

Znovu jakou metodu používáme závisí na co se pokoušíme provést pomocí našeho vlastního ovládacího prvku. Pokud potřebujeme nízkou úroveň přístupu na interakci s uživatelem, použijte metodu přepsat. Potřebujeme předdefinované funkce, jako je například kliknutí myší, pomocí nástroje pro rozpoznávání gesto.

<a name="Responding-to-State-Change-Events" />

### <a name="responding-to-state-change-events"></a>Reagování na události změny stavu

Když uživatel změní stav naše vlastního ovládacího prvku, budeme potřebovat způsob, jak reagovat na změny stavu v kódu (například dělat něco při klikne na tlačítko Vlastní). 

Chcete-li tuto funkčnost poskytovaly, upravte `NSFlipSwitch` třídu a přidejte následující kód:

```csharp
#region Events
public event EventHandler ValueChanged;

internal void RaiseValueChanged() {
    if (this.ValueChanged != null)
        this.ValueChanged (this, EventArgs.Empty);

    // Perform any action bound to the control from Interface Builder
    // via an Action.
    if (this.Action !=null) 
        NSApplication.SharedApplication.SendAction (this.Action, this.Target, this);
}
## endregion
```

Potom upravte `FlipSwitchState` metodu a nastavit jej vypadat třeba takto:

```csharp
private void FlipSwitchState() {
    // Update state
    Value = !Value;
    RaiseValueChanged ();
}
```

Nejprve poskytujeme `ValueChanged` událostí, že jsme můžete přidat obslužnou rutinu pro v C# – kód, který jsme můžete provést akci, když uživatel změní stav přepínače.

Druhé, protože naše vlastního ovládacího prvku dědí od `NSControl`, má automaticky **akce** , mohou být přiřazeny v Tvůrci rozhraní pro Xcode. Toto volání **akce** při změně stavu, použijeme následující kód:

```csharp
if (this.Action !=null) 
    NSApplication.SharedApplication.SendAction (this.Action, this.Target, this);
```

Nejdřív jsme zkontrolujte, zda **akce** byl přiřazen do ovládacího prvku. V dalším kroku říkáme **akce** Pokud byla definována.

<a name="Using-the-Custom-Control" />

## <a name="using-the-custom-control"></a>Pomocí vlastního ovládacího prvku

Pomocí našeho vlastního ovládacího prvku plně definována můžete buď přidáme ji do vaší aplikace Xamarin.Mac uživatelského rozhraní pomocí kódu jazyka C# nebo v Xcode na rozhraní tvůrce.

Přidání ovládacího prvku pomocí rozhraní tvůrce, nejprve provést čisté sestavení projektu Xamarin.Mac, a poté dvakrát klikněte `Main.storyboard` soubor otevřete v Tvůrci rozhraní pro úpravy:

[![](custom-controls-images/custom02.png "Úpravy storyboard v Xcode")](custom-controls-images/custom02.png#lightbox)

V dalším kroku přetáhněte `Custom View` do návrhu uživatelské rozhraní:

[![](custom-controls-images/custom03.png "Výběr vlastních zobrazení z knihovny")](custom-controls-images/custom03.png#lightbox)

S vlastní zobrazení stále vybrán, přepněte do **Identity Inspector** a změnit zobrazení **třída** k `NSFlipSwitch`:

[![](custom-controls-images/custom04.png "Nastavení zobrazení – třída")](custom-controls-images/custom04.png#lightbox)

Přepnout **pomocníka Editor** a vytvořit **výstupu** pro vlastní ovládací prvek (a zkontrolujte, zda vazby v `ViewControler.h` souboru a není `.m` souboru):

[![](custom-controls-images/custom05.png "Konfigurace nového výstupu")](custom-controls-images/custom05.png#lightbox)

Uložte změny, vraťte se na Visual Studio pro Mac a umožnit synchronizaci změn. Upravit `ViewController.cs` souboru a nastavit `ViewDidLoad` metoda vypadá takto:

```csharp
public override void ViewDidLoad ()
{
    base.ViewDidLoad ();

    // Do any additional setup after loading the view.
    OptionTwo.ValueChanged += (sender, e) => {
        // Display the state of the option switch
        Console.WriteLine("Option Two: {0}", OptionTwo.Value);
    };
}
``` 

Zde jsme reagovat na `ValueChanged` jsme definovali výše na události `NSFlipSwitch` třídy a vypsat aktuální **hodnotu** když uživatel klikne na ovládací prvek.

Volitelně můžete jsme vrátit do Tvůrce rozhraní a definovat **akce** na ovládací prvek:

[![](custom-controls-images/custom06.png "Konfigurace nové akce")](custom-controls-images/custom06.png#lightbox)

Znovu upravit `ViewController.cs` souboru a přidejte následující metodu:

```csharp
partial void OptionTwoFlipped (Foundation.NSObject sender) {
    // Display the state of the option switch
    Console.WriteLine("Option Two: {0}", OptionTwo.Value);
}
```

> [!IMPORTANT]
> Měli byste použít buď **událostí** , nebo definujte **akce** v Tvůrci rozhraní, ale neměli byste používat obě metody ve stejnou dobu nebo se může dojít ke konfliktu mezi sebou.

<a name="Summary" />

## <a name="summary"></a>Souhrn

Tento článek podrobně prohlédnout vytváření opakovaně použitelných vlastní uživatelské rozhraní ovládací prvky v aplikaci Xamarin.Mac trvá. Jsme viděli, jak kreslení vlastní ovládací prvky uživatelského rozhraní, dva hlavní způsoby reakce na myši a vstup uživatele a jak vystavit nový ovládací prvek na akce v Tvůrci rozhraní pro Xcode.

## <a name="related-links"></a>Související odkazy

- [MacCustomControl (ukázka)](https://developer.xamarin.com/samples/mac/MacCustomControl/)
- [Hello, Mac](~/mac/get-started/hello-mac.md)
- [Vazba dat a kódování párů klíč-hodnota](~/mac/app-fundamentals/databinding.md)
- [Pokyny pro rozhraní lidské OS X](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/)
- [Zpracování událostí myši](https://developer.apple.com/library/mac/documentation/Cocoa/Conceptual/EventOverview/HandlingMouseEvents/HandlingMouseEvents.html)
