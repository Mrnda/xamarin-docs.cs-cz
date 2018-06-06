---
title: Dialogová okna v Xamarin.Mac
description: Tento článek se zabývá práce se dialogová okna a modální okna v aplikaci Xamarin.Mac. Popisuje vytváření modální okna v Xcode a rozhraní tvůrce, práce s standardní dialogová okna a interakci s tyto ovládací prvky v kódu jazyka C#.
ms.prod: xamarin
ms.assetid: 55451990-B77B-4D44-B8BB-F874EC503B0C
ms.technology: xamarin-mac
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/14/2017
ms.openlocfilehash: 7d9a93c8503d7e25f098e871378a22455b597e90
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 06/05/2018
ms.locfileid: "34792691"
---
# <a name="dialogs-in-xamarinmac"></a>Dialogová okna v Xamarin.Mac

Při práci s C# a rozhraní .NET v aplikaci Xamarin.Mac, máte přístup ke stejným dialogová okna a modální okna, developer, práce *jazyka Objective-C* a *Xcode* nemá. Protože Xamarin.Mac integruje přímo s Xcode, můžete na Xcode _rozhraní tvůrce_ vytvořit a udržovat modální okna (nebo je můžete také vytvořit přímo v kódu jazyka C#).

Zobrazí se dialogové okno se zobrazí v reakci na akci uživatele a obvykle poskytuje uživatelům způsoby, jak můžete dokončit akci. Zobrazí se dialogové okno vyžaduje odpověď od uživatele, než je možné uzavřít.

Windows může být použité v nemodální stavu (třeba textový editor, který může mít více dokumentů současně) nebo modální (například dialogu Export, které musí být před pokračováním aplikace zavře).

[![](dialog-images/dialog03.png "Otevřené dialogové okno")](dialog-images/dialog03.png#lightbox)

V tomto článku vám nabídneme základní informace o práci s dialogová okna a modální okna v aplikaci Xamarin.Mac. Vysoce navržený na spolupracovat [Hello, Mac](~/mac/get-started/hello-mac.md) článek nejprve, konkrétně [Úvod do Xcode a rozhraní tvůrce](~/mac/get-started/hello-mac.md#Introduction_to_Xcode_and_Interface_Builder) a [výstupy a akce](~/mac/get-started/hello-mac.md#Outlets_and_Actions) oddíly, jak se popisuje klíčové koncepty a techniky, které budeme používat v tomto článku.

Můžete chtít podívejte se na [třídy vystavení jazyka C# nebo metody jazyka Objective-C](~/mac/internals/how-it-works.md) části [Xamarin.Mac Internals](~/mac/internals/how-it-works.md) dokumentu taky vysvětluje `Register` a `Export` příkazy používá k navázání tříd jazyka C# jazyka Objective-C objekty a prvky uživatelského rozhraní.

<a name="Introduction_to_Dialogs" />

## <a name="introduction-to-dialogs"></a>Úvod do dialogová okna

Zobrazí se dialogové okno se zobrazí v reakci na akci uživatele (například ukládání souboru) a poskytuje způsob, jak uživatelé k dokončení této akce. Zobrazí se dialogové okno vyžaduje odpověď od uživatele, než je možné uzavřít.

Podle společnosti Apple existují tři způsoby, jak zobrazení dialogu:

- **Zdokumentujte modální** -A dokumentu modálních dialogových zabrání uživateli v jakékoli jiné v rámci daného dokumentu provádění, dokud se zavře.
- **Modální aplikace** – aplikace modálních dialogových zabráníte uživateli interakci s aplikací, dokud se zavře.
- **Nemodální** dialogu nemodální A umožňuje uživatelům změnit nastavení v dialogovém okně při stále interakci s okna dokumentu.

### <a name="modal-window"></a>Modální okna

Všechny standardní `NSWindow` lze použít jako vlastní dialogové okno zobrazením modálně:

[![](dialog-images/modal01.png "Modální okno s příklad")](dialog-images/modal01.png#lightbox)

### <a name="document-modal-dialog-sheets"></a>Listy modálních dialogových dokumentu

A _list_ je modální dialogové okno, který je připojen k okno daného dokumentu, brání uživatelům v interakci s okna, dokud se zavřete toto dialogové okno. List je připojený k okno, ve kterém ukáže, a v jednom okamžiku může být pouze jeden list otevřené pro okno.

[![](dialog-images/sheet08.png "Modální list příklad")](dialog-images/sheet08.png#lightbox)

### <a name="preferences-windows"></a>Předvolby Windows

Okno Předvolby je nemodální dialog, který obsahuje nastavení aplikace, které uživatel změní zřídka. Předvolby Windows často patří panel nástrojů, který umožňuje uživatelům přepínat mezi různé skupiny nastavení:

[![](dialog-images/dialog02.png "Okno s příklad předvoleb")](dialog-images/dialog02.png#lightbox)

### <a name="open-dialog"></a>Dialogové okno Otevřít

Otevřete dialogové okno poskytuje uživatelům konzistentní způsob, jak najít a otevřít položku v aplikaci:

[![](dialog-images/dialog03.png "Otevřete dialogové okno")](dialog-images/dialog03.png#lightbox)


### <a name="print-and-page-setup-dialogs"></a>Tiskové a dialogová okna nastavení stránky

systému macOS poskytuje standardní tisku a stránka Instalace dialogů, které aplikace můžete zobrazit tak, aby uživatelé měli konzistentní tisk prostředí každou aplikaci, kterou používají.

Dialogové okno tisku může být zobrazen jako obou bezplatné plovoucí dialogové okno:

[![](dialog-images/print01.png "Dialogové okno tisku")](dialog-images/print01.png#lightbox)

Nebo se může zobrazovat jako list:

[![](dialog-images/print02.png "Tisk seznamu vlastností")](dialog-images/print02.png#lightbox)

Dialogové okno nastavení stránky lze zobrazit jako obou bezplatné plovoucí dialogové okno:

[![](dialog-images/print03.png "Dialogové okno nastavení stránky")](dialog-images/print03.png#lightbox)

Nebo se může zobrazovat jako list:

[![](dialog-images/print04.png "Instalační program list stránky")](dialog-images/print04.png#lightbox)

### <a name="save-dialogs"></a>Uložit dialogová okna

Dialogové okno Uložit poskytuje uživatelům konzistentní způsob, jak uložit položku v aplikaci. Dialogové okno Uložit má dva stavy: **minimální** (také označované jako sbalené):

[![](dialog-images/save01.png "A uložte dialogové okno")](dialog-images/save01.png#lightbox)

A **rozšířené** stavu:

[![](dialog-images/save02.png "Rozšířené uložit dialogové okno")](dialog-images/save02.png#lightbox)

**Minimální** uložit dialogové okno lze také zobrazit jako list:

[![](dialog-images/save03.png "Minimální uložení listu")](dialog-images/save03.png#lightbox)

Jak můžete **rozšířené** uložit dialogové okno:

[![](dialog-images/save04.png "Rozšířené list uložit")](dialog-images/save04.png#lightbox)

Další informace najdete v tématu [v dialogových oknech](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/WindowDialogs.html#//apple_ref/doc/uid/20000957-CH43-SW1) části společnosti Apple [OS X Human Interface Guidelines](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/)

<a name="Adding_a_Modal_Window_to_a_Project" />

## <a name="adding-a-modal-window-to-a-project"></a>Přidávání modální okna do projektu

Kromě zajištění dostatečného okně hlavního dokumentu aplikace Xamarin.Mac chtít zobrazit další typy systému windows pro uživatele, například předvolby nebo panelů Inspector.

Pokud chcete přidat nové okno, postupujte takto:

1. V **Průzkumníku řešení**, otevřete `Main.storyboard` soubor pro úpravy v Tvůrci rozhraní pro Xcode.
2. Přetáhněte novou **View Controller** do návrhové plochy:

    [![](dialog-images/new01.png "Výběr řadič zobrazení z knihovny")](dialog-images/new01.png#lightbox)
3. V **Identity Inspector**, zadejte `CustomDialogController` pro **název třídy**: 

    [![](dialog-images/new02.png "Nastavení názvu – třída")](dialog-images/new02.png#lightbox)
4. Přepněte zpět na Visual Studio pro Mac, aby ji synchronizovat s Xcode a vytvořit `CustomDialogController.h` souboru.
5. Vraťte se na Xcode a navrhnout vaše rozhraní: 

    [![](dialog-images/new03.png "Návrh uživatelského rozhraní v Xcode")](dialog-images/new03.png#lightbox)
6. Vytvoření **modální Segue** v hlavním okně vaší aplikace na novém řadiči zobrazení tak, že přetáhnete ovládací prvek z uživatelského rozhraní elementu, který se otevře dialogové okno dialogovém okně. Přiřazení **identifikátor** `ModalSegue`: 

    [![](dialog-images/new06.png "Modální segue")](dialog-images/new06.png#lightbox)
6. Navázání, všechny **akce** a **výstupy**: 

    [![](dialog-images/new04.png "Konfigurace akce")](dialog-images/new04.png#lightbox)
6. Uložte změny a vrátit k sadě Visual Studio pro Mac k synchronizaci s Xcode.

Ujistěte se, `CustomDialogController.cs` soubor vypadá takto:

```csharp
using System;
using Foundation;
using AppKit;

namespace MacDialog
{
    public partial class CustomDialogController : NSViewController
    {
        #region Private Variables
        private string _dialogTitle = "Title";
        private string _dialogDescription = "Description";
        private NSViewController _presentor;
        #endregion

        #region Computed Properties
        public string DialogTitle {
            get { return _dialogTitle; }
            set { _dialogTitle = value; }
        }

        public string DialogDescription {
            get { return _dialogDescription; }
            set { _dialogDescription = value; }
        }

        public NSViewController Presentor {
            get { return _presentor; }
            set { _presentor = value; }
        }
        #endregion

        #region Constructors
        public CustomDialogController (IntPtr handle) : base (handle)
        {
        }
        #endregion

        #region Override Methods
        public override void ViewWillAppear ()
        {
            base.ViewWillAppear ();

            // Set initial title and description
            Title.StringValue = DialogTitle;
            Description.StringValue = DialogDescription;
        }
        #endregion

        #region Private Methods
        private void CloseDialog() {
            Presentor.DismissViewController (this);
        }
        #endregion

        #region Custom Actions
        partial void AcceptDialog (Foundation.NSObject sender) {
            RaiseDialogAccepted();
            CloseDialog();
        }

        partial void CancelDialog (Foundation.NSObject sender) {
            RaiseDialogCanceled();
            CloseDialog();
        }
        #endregion

        #region Events
        public EventHandler DialogAccepted;

        internal void RaiseDialogAccepted() {
            if (this.DialogAccepted != null)
                this.DialogAccepted (this, EventArgs.Empty);
        }

        public EventHandler DialogCanceled;

        internal void RaiseDialogCanceled() {
            if (this.DialogCanceled != null)
                this.DialogCanceled (this, EventArgs.Empty);
        }
        #endregion
    }
}
```

Tento kód zpřístupňuje několik vlastností nastavte název a popis dialogového okna a několik událostí reagování na dialogové okno se ke zrušení nebo přijmout.

Potom upravte `ViewController.cs` souboru, přepsat `PrepareForSegue` metoda vypadá takto:

```csharp
public override void PrepareForSegue (NSStoryboardSegue segue, NSObject sender)
{
    base.PrepareForSegue (segue, sender);

    // Take action based on the segue name
    switch (segue.Identifier) {
    case "ModalSegue":
        var dialog = segue.DestinationController as CustomDialogController;
        dialog.DialogTitle = "MacDialog";
        dialog.DialogDescription = "This is a sample dialog.";
        dialog.DialogAccepted += (s, e) => {
            Console.WriteLine ("Dialog accepted");
            DismissViewController (dialog);
        };
        dialog.Presentor = this;
        break;
    }
}
```

Tento kód inicializuje segue, který jsme definovali v Xcode na rozhraní tvůrce do našich dialogového okna a nastaví název a popis. Také obstará volba provedené uživatelem v dialogovém okně.

Můžeme spuštění aplikace a zobrazit dialogové okno Vlastní:

[![](dialog-images/new05.png "O příklad dialogové okno")](dialog-images/new05.png#lightbox)

Další informace o používání oken v aplikaci Xamarin.Mac, najdete v tématu naše [práce s Windows](~/mac/user-interface/window.md) dokumentaci.

<a name="Creating_a_Custom_Sheet" />

## <a name="creating-a-custom-sheet"></a>Vytváření vlastních stylů

A _list_ je modální dialogové okno, který je připojen k okno daného dokumentu, brání uživatelům v interakci s okna, dokud se zavřete toto dialogové okno. List je připojený k okno, ve kterém ukáže, a v jednom okamžiku může být pouze jeden list otevřené pro okno. 

Pokud chcete vytvořit vlastní list v Xamarin.Mac, můžeme takto:

1. V **Průzkumníku řešení**, otevřete `Main.storyboard` soubor pro úpravy v Tvůrci rozhraní pro Xcode.
2. Přetáhněte novou **View Controller** do návrhové plochy:

    [![](dialog-images/new01.png "Výběr řadič zobrazení z knihovny")](dialog-images/new01.png#lightbox)
2. Návrh uživatelského rozhraní:

    [![](dialog-images/sheet01.png "Návrh uživatelského rozhraní")](dialog-images/sheet01.png#lightbox)
3. Vytvoření **Segue list** z vaší hlavní okno na nový řadič zobrazení: 

    [![](dialog-images/sheet02.png "Výběr typu segue listu")](dialog-images/sheet02.png#lightbox)
4. V **Identity Inspector**, název řadiče zobrazení **třída** `SheetViewController`: 

    [![](dialog-images/sheet03.png "Nastavení názvu – třída")](dialog-images/sheet03.png#lightbox)
5. Zadejte všechny potřebné **výstupy** a **akce**: 

    [![](dialog-images/sheet04.png "Definování požadované výstupy a akce")](dialog-images/sheet04.png#lightbox)
6. Uložte změny a vrátit k sadě Visual Studio pro Mac k synchronizaci.

Potom upravte `SheetViewController.cs` souboru a nastavit jej vypadat třeba takto:

```csharp
using System;
using Foundation;
using AppKit;

namespace MacDialog
{
    public partial class SheetViewController : NSViewController
    {
        #region Private Variables
        private string _userName = "";
        private string _password = "";
        private NSViewController _presentor;
        #endregion

        #region Computed Properties
        public string UserName {
            get { return _userName; }
            set { _userName = value; }
        }

        public string Password {
            get { return _password;}
            set { _password = value;}
        }

        public NSViewController Presentor {
            get { return _presentor; }
            set { _presentor = value; }
        }
        #endregion

        #region Constructors
        public SheetViewController (IntPtr handle) : base (handle)
        {
        }
        #endregion

        #region Override Methods
        public override void ViewWillAppear ()
        {
            base.ViewWillAppear ();

            // Set initial values
            NameField.StringValue = UserName;
            PasswordField.StringValue = Password;

            // Wireup events
            NameField.Changed += (sender, e) => {
                UserName = NameField.StringValue;
            };
            PasswordField.Changed += (sender, e) => {
                Password = PasswordField.StringValue;
            };
        }
        #endregion

        #region Private Methods
        private void CloseSheet() {
            Presentor.DismissViewController (this);
        }
        #endregion

        #region Custom Actions
        partial void AcceptSheet (Foundation.NSObject sender) {
            RaiseSheetAccepted();
            CloseSheet();
        }

        partial void CancelSheet (Foundation.NSObject sender) {
            RaiseSheetCanceled();
            CloseSheet();
        }
        #endregion

        #region Events
        public EventHandler SheetAccepted;

        internal void RaiseSheetAccepted() {
            if (this.SheetAccepted != null)
                this.SheetAccepted (this, EventArgs.Empty);
        }

        public EventHandler SheetCanceled;

        internal void RaiseSheetCanceled() {
            if (this.SheetCanceled != null)
                this.SheetCanceled (this, EventArgs.Empty);
        }
        #endregion
    }
}
```

Potom upravte `ViewController.cs` souboru, upravte `PrepareForSegue` metodu a nastavit jej vypadat třeba takto:

```csharp
public override void PrepareForSegue (NSStoryboardSegue segue, NSObject sender)
{
    base.PrepareForSegue (segue, sender);

    // Take action based on the segue name
    switch (segue.Identifier) {
    case "ModalSegue":
        var dialog = segue.DestinationController as CustomDialogController;
        dialog.DialogTitle = "MacDialog";
        dialog.DialogDescription = "This is a sample dialog.";
        dialog.DialogAccepted += (s, e) => {
            Console.WriteLine ("Dialog accepted");
            DismissViewController (dialog);
        };
        dialog.Presentor = this;
        break;
    case "SheetSegue":
        var sheet = segue.DestinationController as SheetViewController;
        sheet.SheetAccepted += (s, e) => {
            Console.WriteLine ("User Name: {0} Password: {1}", sheet.UserName, sheet.Password);
        };
        sheet.Presentor = this;
        break;
    }
}
```

Pokud jsme spuštění aplikace a otevřete list, bude připojen do okna:

[![](dialog-images/sheet08.png "Příklad listu")](dialog-images/sheet08.png#lightbox)

<a name="Creating_a_Preferences_Dialog" />

## <a name="creating-a-preferences-dialog"></a>Vytvořit dialogové okno Předvolby

Před jsme rozložit předvolby zobrazení v rozhraní tvůrce, budeme potřebovat přidejte vlastní segue typ zpracování vypínání preference. Přidejte novou třídu do projektu a pojmenujte ji `ReplaceViewSeque `. Upravit třídy a nastavit jej vypadat následovně:

```csharp
using System;
using AppKit;
using Foundation;

namespace MacWindows
{
    [Register("ReplaceViewSeque")]
    public class ReplaceViewSeque : NSStoryboardSegue
    {
        #region Constructors
        public ReplaceViewSeque() {

        }

        public ReplaceViewSeque (string identifier, NSObject sourceController, NSObject destinationController) : base(identifier,sourceController,destinationController) {

        }

        public ReplaceViewSeque (IntPtr handle) : base(handle) {
        }

        public ReplaceViewSeque (NSObjectFlag x) : base(x) {
        }
        #endregion

        #region Override Methods
        public override void Perform ()
        {
            // Cast the source and destination controllers
            var source = SourceController as NSViewController;
            var destination = DestinationController as NSViewController;

            // Is there a source?
            if (source == null) {
                // No, get the current key window
                var window = NSApplication.SharedApplication.KeyWindow;

                // Swap the controllers
                window.ContentViewController = destination;

                // Release memory
                window.ContentViewController?.RemoveFromParentViewController ();
            } else {
                // Swap the controllers
                source.View.Window.ContentViewController = destination;

                // Release memory
                source.RemoveFromParentViewController ();
            }
        
        }
        #endregion

    }

}
```

S vlastní segue vytvořen jsme můžete přidat nové okno v Xcode na rozhraní tvůrce pro zpracování naše předvolby.

Pokud chcete přidat nové okno, postupujte takto:

1. V **Průzkumníku řešení**, otevřete `Main.storyboard` soubor pro úpravy v Tvůrci rozhraní pro Xcode.
2. Přetáhněte novou **okno řadiče** do návrhové plochy:

    [![](dialog-images/pref01.png "Vyberte řadič okno z knihovny")](dialog-images/pref01.png#lightbox)
3. Uspořádá okna téměř **řádku nabídek** designer:

    [![](dialog-images/pref02.png "Přidání nové okno")](dialog-images/pref02.png#lightbox)
4. Vytvořte kopie připojené řadiče zobrazení jako v zobrazení předvoleb bude být karty:

    [![](dialog-images/pref03.png "Přidávání na požadované řadiče zobrazení")](dialog-images/pref03.png#lightbox)
5. Přetáhněte novou **nástrojů řadič** z **knihovny**:

    [![](dialog-images/pref04.png "Vyberte panelu nástrojů řadič z knihovny")](dialog-images/pref04.png#lightbox)
6. A umístěte jej v okně v návrhové ploše:

    [![](dialog-images/pref05.png "Přidávání nového řadiče panelu nástrojů")](dialog-images/pref05.png#lightbox)
7. Rozložení panelu nástrojů v návrhu:

    [![](dialog-images/pref06.png "Rozložení panelu nástrojů")](dialog-images/pref06.png#lightbox)
8. Ovládací prvek klikněte a přetáhněte ji z každé **tlačítka panelu nástrojů** k zobrazení, které jste vytvořili výše. Vyberte **vlastní** segue typu:

    [![](dialog-images/pref07.png "Nastavení typu segue")](dialog-images/pref07.png#lightbox)
9. Vyberte nové Segue a nastavte **třída** k `ReplaceViewSegue`:

    [![](dialog-images/pref08.png "Nastavení segue – třída")](dialog-images/pref08.png#lightbox)
10. V **Menubar Návrhář** na návrhovou plochu, vyberte v nabídce aplikace **předvolby...** , řízení klikněte a přetáhněte ji do okna předvoleb k vytvoření **zobrazit** segue:

    [![](dialog-images/pref09.png "Nastavení typu segue")](dialog-images/pref09.png#lightbox)
11. Uložte změny a vrátit k sadě Visual Studio pro Mac k synchronizaci.

Pokud jsme spustit kód a vybrat **předvolby...**  z **nabídku aplikace**, zobrazí se okno:

[![](dialog-images/pref10.png "Okno Předvolby s příklad")](dialog-images/pref10.png#lightbox)

Další informace o práci s Windows a panelů nástrojů, najdete v tématu naše [Windows](~/mac/user-interface/window.md) a [panely nástrojů](~/mac/user-interface/toolbar.md) dokumentaci.

<a name="Saving-and-Loading-Preferences" />

### <a name="saving-and-loading-preferences"></a>Ukládání a načítání předvolby

V typické macOS aplikace, když uživatel provede změny do žádné aplikace uživatelské předvolby, tyto změny budou uloženy automaticky. Nejjednodušší způsob, jak to zpracování v aplikaci Xamarin.Mac, je vytvoření jedné třídy a spravovat všechny uživatelské předvolby sdílet celého systému.

Nejprve přidejte nový `AppPreferences` třídu do projektu a dědí `NSObject`. Preference budou navrhovány používat [datové vazby a klíč-hodnota kódování](~/mac/app-fundamentals/databinding.md) který provede procesem vytvoření a udržování předvolba forms mnohem jednodušší. Vzhledem k tomu, že preference budou tvořeny malé množství jednoduché datové typy, pomocí předdefinovaných v `NSUserDefaults` k ukládání a načítání hodnoty.

Upravit `AppPreferences.cs` souboru a nastavit jej vypadat třeba takto:

```csharp
using System;
using Foundation;
using AppKit;

namespace SourceWriter
{
    [Register("AppPreferences")]
    public class AppPreferences : NSObject
    {
        #region Computed Properties
        [Export("DefaultLangauge")]
        public int DefaultLanguage {
            get { 
                var value = LoadInt ("DefaultLangauge", 0);
                return value; 
            }
            set {
                WillChangeValue ("DefaultLangauge");
                SaveInt ("DefaultLangauge", value, true);
                DidChangeValue ("DefaultLangauge");
            }
        }

        [Export("SmartLinks")]
        public bool SmartLinks {
            get { return LoadBool ("SmartLinks", true); }
            set {
                WillChangeValue ("SmartLinks");
                SaveBool ("SmartLinks", value, true);
                DidChangeValue ("SmartLinks");
            }
        }

        // Define any other required user preferences in the same fashion
        ...

        [Export("EditorBackgroundColor")]
        public NSColor EditorBackgroundColor {
            get { return LoadColor("EditorBackgroundColor", NSColor.White); }
            set {
                WillChangeValue ("EditorBackgroundColor");
                SaveColor ("EditorBackgroundColor", value, true);
                DidChangeValue ("EditorBackgroundColor");
            }
        }
        #endregion

        #region Constructors
        public AppPreferences ()
        {
        }
        #endregion

        #region Public Methods
        public int LoadInt(string key, int defaultValue) {
            // Attempt to read int
            var number = NSUserDefaults.StandardUserDefaults.IntForKey(key);

            // Take action based on value
            if (number == null) {
                return defaultValue;
            } else {
                return (int)number;
            }
        }
            
        public void SaveInt(string key, int value, bool sync) {
            NSUserDefaults.StandardUserDefaults.SetInt(value, key);
            if (sync) NSUserDefaults.StandardUserDefaults.Synchronize ();
        }

        public bool LoadBool(string key, bool defaultValue) {
            // Attempt to read int
            var value = NSUserDefaults.StandardUserDefaults.BoolForKey(key);

            // Take action based on value
            if (value == null) {
                return defaultValue;
            } else {
                return value;
            }
        }

        public void SaveBool(string key, bool value, bool sync) {
            NSUserDefaults.StandardUserDefaults.SetBool(value, key);
            if (sync) NSUserDefaults.StandardUserDefaults.Synchronize ();
        }

        public string NSColorToHexString(NSColor color, bool withAlpha) {
            //Break color into pieces
            nfloat red=0, green=0, blue=0, alpha=0;
            color.GetRgba (out red, out green, out blue, out alpha);

            // Adjust to byte
            alpha *= 255;
            red *= 255;
            green *= 255;
            blue *= 255;

            //With the alpha value?
            if (withAlpha) {
                return String.Format ("#{0:X2}{1:X2}{2:X2}{3:X2}", (int)alpha, (int)red, (int)green, (int)blue);
            } else {
                return String.Format ("#{0:X2}{1:X2}{2:X2}", (int)red, (int)green, (int)blue);
            }
        }

        public NSColor NSColorFromHexString (string hexValue)
        {
            var colorString = hexValue.Replace ("#", "");
            float red, green, blue, alpha;

            // Convert color based on length
            switch (colorString.Length) {
            case 3 : // #RGB
                red = Convert.ToInt32(string.Format("{0}{0}", colorString.Substring(0, 1)), 16) / 255f;
                green = Convert.ToInt32(string.Format("{0}{0}", colorString.Substring(1, 1)), 16) / 255f;
                blue = Convert.ToInt32(string.Format("{0}{0}", colorString.Substring(2, 1)), 16) / 255f;
                return NSColor.FromRgba(red, green, blue, 1.0f);
            case 6 : // #RRGGBB
                red = Convert.ToInt32(colorString.Substring(0, 2), 16) / 255f;
                green = Convert.ToInt32(colorString.Substring(2, 2), 16) / 255f;
                blue = Convert.ToInt32(colorString.Substring(4, 2), 16) / 255f;
                return NSColor.FromRgba(red, green, blue, 1.0f);
            case 8 : // #AARRGGBB
                alpha = Convert.ToInt32(colorString.Substring(0, 2), 16) / 255f;
                red = Convert.ToInt32(colorString.Substring(2, 2), 16) / 255f;
                green = Convert.ToInt32(colorString.Substring(4, 2), 16) / 255f;
                blue = Convert.ToInt32(colorString.Substring(6, 2), 16) / 255f;
                return NSColor.FromRgba(red, green, blue, alpha);
            default :
                throw new ArgumentOutOfRangeException(string.Format("Invalid color value '{0}'. It should be a hex value of the form #RBG, #RRGGBB or #AARRGGBB", hexValue));
            }
        }

        public NSColor LoadColor(string key, NSColor defaultValue) {

            // Attempt to read color
            var hex = NSUserDefaults.StandardUserDefaults.StringForKey(key);

            // Take action based on value
            if (hex == null) {
                return defaultValue;
            } else {
                return NSColorFromHexString (hex);
            }
        }

        public void SaveColor(string key, NSColor color, bool sync) {
            // Save to default
            NSUserDefaults.StandardUserDefaults.SetString(NSColorToHexString(color,true), key);
            if (sync) NSUserDefaults.StandardUserDefaults.Synchronize ();
        }
        #endregion
    }
}
```

Tato třída obsahuje několik pomocné rutiny, jako `SaveInt`, `LoadInt`, `SaveColor`, `LoadColor`, atd. pro usnadnění práce s `NSUserDefaults` jednodušší. Navíc od `NSUserDefaults` nemá předdefinovaný způsob, jak zpracovat `NSColors`, `NSColorToHexString` a `NSColorFromHexString` metody se používají k převést barvy na webové hexadecimálních řetězců (`#RRGGBBAA` kde `AA` je průhlednost alfa), může být můžete snadno uložené a načtena.

V `AppDelegate.cs` souboru, vytvořte instanci **AppPreferences** objekt, který se použije celou aplikaci:

```csharp
using AppKit;
using Foundation;
using System.IO;
using System;

namespace SourceWriter
{
    [Register ("AppDelegate")]
    public class AppDelegate : NSApplicationDelegate
    {
        #region Computed Properties
        public int NewWindowNumber { get; set;} = -1;

        public AppPreferences Preferences { get; set; } = new AppPreferences();
        #endregion

        #region Constructors
        public AppDelegate ()
        {
        }
        #endregion
        
        ...
```

<a name="Wiring-Preferences-to-Preference-Views" />

### <a name="wiring-preferences-to-preference-views"></a>Vzájemné propojení předvolby s předvolby zobrazení

V dalším kroku připojit předvoleb třída k elementům uživatelského rozhraní v okně předvoleb a zobrazení vytvořili výše. V Tvůrci rozhraní, vyberte řadič zobrazení předvoleb a přepněte do **Identity Inspector**, vytvořte vlastní třídu pro kontroler: 

[![](dialog-images/prefs12.png "Nástroj Inspector Identity")](dialog-images/prefs12.png#lightbox)

Přepněte zpátky na Visual Studio pro Mac synchronizovat změny a otevřete nově vytvořený třídy pro úpravy. Ujistěte se, třída vypadat následovně:

```csharp
using System;
using Foundation;
using AppKit;

namespace SourceWriter
{
    public partial class EditorPrefsController : NSViewController
    {
        #region Application Access
        public static AppDelegate App {
            get { return (AppDelegate)NSApplication.SharedApplication.Delegate; }
        }
        #endregion

        #region Computed Properties
        [Export("Preferences")]
        public AppPreferences Preferences {
            get { return App.Preferences; }
        }
        #endregion

        #region Constructors
        public EditorPrefsController (IntPtr handle) : base (handle)
        {
        }
        #endregion
    }
}
```

Všimněte si, že tato třída provedla zde dvě věci: první je pomocné rutiny `App` vlastnost, aby se přístup k **AppDelegate** jednodušší. Druhý, `Preferences` vlastnost zpřístupní na globální **AppPreferences** třídy pro vytvoření datové vazby s všech ovládacích prvků uživatelského rozhraní umístit do tohoto zobrazení.

Potom poklikejte na soubor Storyboard ho znovu otevřete v Tvůrci rozhraní (a zobrazit pouze změny výše). Přetáhněte potřebné k vytváření rozhraní předvolby do zobrazení všech ovládacích prvků uživatelského rozhraní. Pro každý ovládací prvek, přepněte do **vazby Inspector** a vytvořit vazbu na jednotlivé vlastnosti **AppPreference** třídy:

[![](dialog-images/prefs13.png "Vazba Inspector")](dialog-images/prefs13.png#lightbox)

Výše uvedené kroky opakujte u všech panelů (řadiče zobrazení) a požadované vlastnosti předvoleb.

<a name="Applying-Preference-Changes-to-All-Open-Windows" />

### <a name="applying-preference-changes-to-all-open-windows"></a>Použití předvoleb změny všechna otevřená okna

Jak jsme uvedli výše, v typické systému macOS aplikace, když uživatel provede změny do žádné aplikace uživatelské předvolby, tyto změny se automaticky uloží a použity na všechny windows uživatel může mít otevřít v aplikaci.

Tento proces provést transparentně a bez obtíží pro koncového uživatele s minimální velikostí kódování pracovní vám umožní pečlivé plánování a návrh předvolby vaší aplikace a systému windows.

Pro jakékoli okno, které budou pracovat s aplikací předvolby, přidat následující vlastnost pomocná jeho obsahu řadiče zobrazení, abyste měli přístup k naší **AppDelegate** jednodušší:

```csharp
#region Application Access
public static AppDelegate App {
    get { return (AppDelegate)NSApplication.SharedApplication.Delegate; }
}
#endregion
```

Třída ke konfiguraci obsahu nebo chování v závislosti na uživatelské předvolby v dalším kroku přidejte:

```csharp
public void ConfigureEditor() {

    // General Preferences
    TextEditor.AutomaticLinkDetectionEnabled = App.Preferences.SmartLinks;
    TextEditor.AutomaticQuoteSubstitutionEnabled = App.Preferences.SmartQuotes;
    ...

}
``` 

Je třeba volat metodu konfigurace při prvním otevření okna a ujistěte se, že vyhovuje uživatelské předvolby:

```csharp
public override void ViewDidLoad ()
{
    base.ViewDidLoad ();

    // Configure editor from user preferences
    ConfigureEditor ();
    ...
}
```

Potom upravte `AppDelegate.cs` souboru a přidejte následující metodu použít jakékoli předvoleb změny všechna otevřená okna:

```csharp
public void UpdateWindowPreferences() {

    // Process all open windows
    for(int n=0; n<NSApplication.SharedApplication.Windows.Length; ++n) {
        var content = NSApplication.SharedApplication.Windows[n].ContentViewController as ViewController;
        if (content != null ) {
            // Reformat all text
            content.ConfigureEditor ();
        }
    }

}
```

Dál přidejte `PreferenceWindowDelegate` třídu do projektu a nastavit jej vypadat třeba takto:

```csharp
using System;
using AppKit;
using System.IO;
using Foundation;

namespace SourceWriter
{
    public class PreferenceWidowDelegate : NSWindowDelegate
    {
        #region Application Access
        public static AppDelegate App {
            get { return (AppDelegate)NSApplication.SharedApplication.Delegate; }
        }
        #endregion

        #region Computed Properties
        public NSWindow Window { get; set;}
        #endregion

        #region constructors
        public PreferenceWidowDelegate (NSWindow window)
        {
            // Initialize
            this.Window = window;

        }
        #endregion

        #region Override Methods
        public override bool WindowShouldClose (Foundation.NSObject sender)
        {
            
            // Apply any changes to open windows
            App.UpdateWindowPreferences();

            return true;
        }
        #endregion
    }
}
```

To způsobí, že veškeré změny předvoleb k odeslání do všechna otevřená okna předvolba okno zavře.

Nakonec upravte řadičem okno předvoleb a přidejte delegát vytvořili výše:

```csharp
using System;
using Foundation;
using AppKit;

namespace SourceWriter
{
    public partial class PreferenceWindowController : NSWindowController
    {
        #region Constructors
        public PreferenceWindowController (IntPtr handle) : base (handle)
        {
        }
        #endregion

        #region Override Methods
        public override void WindowDidLoad ()
        {
            base.WindowDidLoad ();

            // Initialize
            Window.Delegate = new PreferenceWidowDelegate(Window);
            Toolbar.SelectedItemIdentifier = "General";
        }
        #endregion
    }
}
```

Všechny tyto změny v místě pokud uživatel upravuje předvoleb aplikace a zavře okno předvoleb, změny se použijí pro všechna otevřená okna:

[![](dialog-images/prefs14.png "Okno Předvolby s příklad")](dialog-images/prefs14.png#lightbox)

<a name="The_Open_Dialog" />

## <a name="the-open-dialog"></a>Dialogové okno Otevřít

Otevřete dialogové okno poskytuje uživatelům konzistentní způsob, jak najít a otevřít položku v aplikaci. Chcete-li zobrazit dialogové okno otevřít v aplikaci Xamarin.Mac, použijte následující kód:

```csharp
var dlg = NSOpenPanel.OpenPanel;
dlg.CanChooseFiles = true;
dlg.CanChooseDirectories = false;
dlg.AllowedFileTypes = new string[] { "txt", "html", "md", "css" };

if (dlg.RunModal () == 1) {
    // Nab the first file
    var url = dlg.Urls [0];

    if (url != null) {
        var path = url.Path;

        // Create a new window to hold the text
        var newWindowController = new MainWindowController ();
        newWindowController.Window.MakeKeyAndOrderFront (this);

        // Load the text into the window
        var window = newWindowController.Window as MainWindow;
        window.Text = File.ReadAllText(path);
        window.SetTitleWithRepresentedFilename (Path.GetFileName(path));
        window.RepresentedUrl = url;

    }
}
```

Ve výše uvedeném kódu jsme jsou otevřete nové okno dokument k zobrazení obsahu souboru. Budete muset nahraďte ho kódu s použitím funkce se vyžaduje vaše aplikace.

Následující vlastnosti jsou k dispozici při práci se službou `NSOpenPanel`:

- **CanChooseFiles** – Pokud `true` uživatele můžete vybrat soubory.
- **CanChooseDirectories** – Pokud `true` kterého uživatel může vybrat adresáře.
- **AllowsMultipleSelection** – Pokud `true` kterého uživatel může vybrat více než jeden soubor současně.
- **ResolveAliases** – Pokud `true` výběrem a alias, přeloží cestu původní soubor.
- **AllowedFileTypes** -je pole typů souborů, které může uživatel vybrat jako buď rozšíření nebo _UTI_. Výchozí hodnota je `null`, což umožňuje libovolný soubor otevřít.

`RunModal ()` Metoda zobrazí dialogové okno otevřít a povolit uživatelům výběr souborů a složek (podle vlastnosti) a vrátí `1` Pokud uživatel klikne **otevřete** tlačítko.

Dialogové okno Otevřít jako pole adresy URL v vrátí vybrané soubory nebo adresáře uživatele `URL` vlastnost.

Pokud jsme spustit program a vybrat **otevřete...**  položky z **souboru** se zobrazí nabídka následující: 

[![](dialog-images/dialog03.png "Otevřené dialogové okno")](dialog-images/dialog03.png#lightbox)

<a name="The_Print_and_Page_Setup_Dialogs" />

## <a name="the-print-and-page-setup-dialogs"></a>Tiskové a dialogová okna nastavení stránky

systému macOS poskytuje standardní tisku a stránka Instalace dialogů, které aplikace můžete zobrazit tak, aby uživatelé měli konzistentní tisk prostředí každou aplikaci, kterou používají.

Následující kód zobrazí standardní dialogové okno tisku:

```csharp
public bool ShowPrintAsSheet { get; set;} = true;
...

[Export ("showPrinter:")]
void ShowDocument (NSObject sender) {
    var dlg = new NSPrintPanel();

    // Display the print dialog as dialog box
    if (ShowPrintAsSheet) {
        dlg.BeginSheet(new NSPrintInfo(),this,this,null,new IntPtr());
    } else {
        if (dlg.RunModalWithPrintInfo(new NSPrintInfo()) == 1) {
            var alert = new NSAlert () {
                AlertStyle = NSAlertStyle.Critical,
                InformativeText = "We need to print the document here...",
                MessageText = "Print Document",
            };
            alert.RunModal ();
        }
    }
}

```

Pokud se nastaví `ShowPrintAsSheet` vlastnost `false`, spusťte aplikaci a zobrazit dialogovém okně tisku, zobrazí se následující:

[![](dialog-images/print01.png "Dialogové okno tisku")](dialog-images/print01.png#lightbox)

Pokud nastavení `ShowPrintAsSheet` vlastnost `true`, spusťte aplikaci a zobrazit dialogovém okně tisku, zobrazí se následující:

[![](dialog-images/print02.png "Tisk seznamu vlastností")](dialog-images/print02.png#lightbox)

Následující kód se zobrazí dialogové okno rozložení stránky:

```csharp
[Export ("showLayout:")]
void ShowLayout (NSObject sender) {
    var dlg = new NSPageLayout();

    // Display the print dialog as dialog box
    if (ShowPrintAsSheet) {
        dlg.BeginSheet (new NSPrintInfo (), this);
    } else {
        if (dlg.RunModal () == 1) {
            var alert = new NSAlert () {
                AlertStyle = NSAlertStyle.Critical,
                InformativeText = "We need to print the document here...",
                MessageText = "Print Document",
            };
            alert.RunModal ();
        }
    }
}
```

Pokud se nastaví `ShowPrintAsSheet` vlastnost `false`, spusťte aplikaci a zobrazit dialogové okno tisku rozložení, zobrazí se následující:

[![](dialog-images/print03.png "Dialogové okno nastavení stránky")](dialog-images/print03.png#lightbox)

Pokud nastavení `ShowPrintAsSheet` vlastnost `true`, spusťte aplikaci a zobrazit dialogové okno tisku rozložení, zobrazí se následující:

[![](dialog-images/print04.png "Instalační program list stránky")](dialog-images/print04.png#lightbox)

Další informace o práci s tiskových a dialogová okna nastavení stránky, najdete v tématu společnosti Apple [NSPrintPanel](https://developer.apple.com/library/mac/documentation/Cocoa/Reference/ApplicationKit/Classes/NSPrintPanel_Class/index.html#//apple_ref/doc/uid/TP40004092), [NSPageLayout](https://developer.apple.com/library/mac/documentation/Cocoa/Reference/ApplicationKit/Classes/NSPageLayout_Class/index.html#//apple_ref/doc/uid/TP40004080) a [Úvod do tisk](http://sdg.mesonet.org/people/brad/XCode3/Documentation/DocSets/com.apple.adc.documentation.AppleSnowLeopard.CoreReference.docset/Contents/Resources/Documents/#documentation/Cocoa/Conceptual/Printing/Printing.html#//apple_ref/doc/uid/10000083-SW1) dokumentace.

<a name="The_Save_Dialog" />

## <a name="the-save-dialog"></a>Uložení dialogové okno

Dialogové okno Uložit poskytuje uživatelům konzistentní způsob, jak uložit položku v aplikaci.

Následující kód zobrazí standardní dialogové okno uložení:

```csharp
public bool ShowSaveAsSheet { get; set;} = true;
...

[Export("saveDocumentAs:")]
void ShowSaveAs (NSObject sender)
{
    var dlg = new NSSavePanel ();
    dlg.Title = "Save Text File";
    dlg.AllowedFileTypes = new string[] { "txt", "html", "md", "css" };

    if (ShowSaveAsSheet) {
        dlg.BeginSheet(mainWindowController.Window,(result) => {
            var alert = new NSAlert () {
                AlertStyle = NSAlertStyle.Critical,
                InformativeText = "We need to save the document here...",
                MessageText = "Save Document",
            };
            alert.RunModal ();
        });
    } else {
        if (dlg.RunModal () == 1) {
            var alert = new NSAlert () {
                AlertStyle = NSAlertStyle.Critical,
                InformativeText = "We need to save the document here...",
                MessageText = "Save Document",
            };
            alert.RunModal ();
        }
    }

}
```

`AllowedFileTypes` Vlastnost je typů souborů, které může uživatel vybrat k uložení souboru jako pole řetězců. Typ souboru můžete buď zadat jako rozšíření nebo _UTI_. Výchozí hodnota je `null`, což umožňuje žádný typ souboru, který se má použít.

Pokud se nastaví `ShowSaveAsSheet` vlastnost `false`, spusťte aplikaci a vyberte **uložit jako...**  z **souboru** nabídky, zobrazí se následující:

[![](dialog-images/save01.png "A uložte dialogové okno")](dialog-images/save01.png#lightbox)

Uživatele můžete rozšířit dialogové okno:

[![](dialog-images/save02.png "Rozšířené dialogové okno uložení")](dialog-images/save02.png#lightbox)

Pokud se nastaví `ShowSaveAsSheet` vlastnost `true`, spusťte aplikaci a vyberte **uložit jako...**  z **souboru** nabídky, zobrazí se následující:

[![](dialog-images/save03.png "A uložte listu")](dialog-images/save03.png#lightbox)

Uživatele můžete rozšířit dialogové okno:

[![](dialog-images/save04.png "Rozšířené list uložit")](dialog-images/save04.png#lightbox)

Další informace o práci se dialogové okno Uložit, najdete v tématu společnosti Apple [NSSavePanel](https://developer.apple.com/library/mac/documentation/Cocoa/Reference/ApplicationKit/Classes/NSSavePanel_Class/index.html#//apple_ref/doc/uid/TP40004098) dokumentaci.

<a name="Summary" />

## <a name="summary"></a>Souhrn

Tento článek podrobně prohlédnout práce modální okna, seznamy a systému standardní dialogová okna v aplikaci Xamarin.Mac trvá. Jsme viděli různé typy a používá modální okna, listy a dialogová okna, jak vytvořit a udržovat modální okna a listy v Xcode je rozhraní tvůrce a jak pracovat s modální okna, listů a dialogových oknech v kódu jazyka C#.

## <a name="related-links"></a>Související odkazy

- [MacWindows (ukázka)](https://developer.xamarin.com/samples/mac/MacWindows/)
- [Hello, Mac](~/mac/get-started/hello-mac.md)
- [Nabídky](~/mac/user-interface/menu.md)
- [Windows](~/mac/user-interface/window.md)
- [Panely nástrojů](~/mac/user-interface/toolbar.md)
- [Pokyny pro rozhraní lidské OS X](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/)
- [Úvod do systému Windows](https://developer.apple.com/library/mac/documentation/Cocoa/Conceptual/WinPanel/Introduction.html#//apple_ref/doc/uid/10000031-SW1)
- [Úvod do listů](https://developer.apple.com/library/mac/documentation/Cocoa/Conceptual/Sheets/Sheets.html#//apple_ref/doc/uid/10000002i)
- [Úvod k tisku](https://developer.apple.com/library/mac/documentation/Cocoa/Conceptual/Printing/osxp_aboutprinting/osxp_aboutprt.html)
