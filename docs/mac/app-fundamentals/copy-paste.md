---
title: "Kopírování a vkládání"
description: "Tento článek se zabývá práci s pracovní plochou poskytují kopírování a vložení v aplikaci Xamarin.Mac. Ukazuje, jak pracovat s standardní datovými typy, které můžete sdílet mezi více aplikacemi a jak pro podporu vlastních dat v rámci dané aplikace."
ms.topic: article
ms.prod: xamarin
ms.assetid: 7E9C99FB-B7B4-4C48-B20F-84CB48543083
ms.technology: xamarin-mac
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/14/2017
ms.openlocfilehash: 115f3340c5678c0ead06cf773e193fbdc4ba3d07
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 02/27/2018
---
# <a name="copy-and-paste"></a>Kopírování a vkládání

_Tento článek se zabývá práci s pracovní plochou poskytují kopírování a vložení v aplikaci Xamarin.Mac. Ukazuje, jak pracovat s standardní datovými typy, které můžete sdílet mezi více aplikacemi a jak pro podporu vlastních dat v rámci dané aplikace._

## <a name="overview"></a>Přehled

Při práci s C# a rozhraní .NET v aplikaci Xamarin.Mac, máte přístup k stejný pracovní plochy (kopírování a vkládání) podpoře s vývojář pracující v Objective-C.

V tomto článku jsme se pokrývajících dva hlavní způsoby použití s pracovní plochou v aplikaci Xamarin.Mac:

1. **Standardní typy dat** -vzhledem k tomu, že pracovní ploše činnosti obvykle provádí mezi dvěma aplikacemi nesouvisejícími, ani aplikace zná typy dat, která dalších podporuje. Pokud chcete maximalizovat potenciální pro sdílení, pracovní plocha pojme více reprezentace daná položka (pomocí standardní sadu běžné typy dat), to umožní spotřebitelskou aplikaci vyberte verzi, která je nejvhodnější pro své potřeby.
2. **Vlastní Data** – pro podporu kopírování a vkládání komplexní dat v rámci vaší Xamarin.Mac můžete definovat vlastní datový typ, který bude zpracován adresou pracovní plocha. Například aplikaci kreslení pomocí vektoru umožňující uživateli kopírování a vkládání obrazců komplexní, které se skládají z více datových typů a body.

[![Příklad spuštěné aplikaci](copy-paste-images/intro01.png "příklad spuštěné aplikaci")](copy-paste-images/intro01-large.png)

V tomto článku vám nabídneme základní informace o práci s pracovní plochou v aplikaci Xamarin.Mac podporu kopírování a vložení operations. Vysoce navržený na spolupracovat [Hello, Mac](~/mac/get-started/hello-mac.md) článek nejprve, konkrétně [Úvod do Xcode a rozhraní tvůrce](~/mac/get-started/hello-mac.md#Introduction_to_Xcode_and_Interface_Builder) a [výstupy a akce](~/mac/get-started/hello-mac.md#Outlets_and_Actions) oddíly, jak se popisuje klíčové koncepty a techniky, které budeme používat v tomto článku.

Můžete chtít podívejte se na [třídy vystavení jazyka C# nebo metody jazyka Objective-C](~/mac/internals/how-it-works.md) části [Xamarin.Mac Internals](~/mac/internals/how-it-works.md) dokumentu taky vysvětluje `Register` a `Export` atributy Umožňuje propojit až tříd jazyka C# a uživatelského rozhraní jazyka Objective-C objekty elementy.

## <a name="getting-started-with-the-pasteboard"></a>Začínáme s s pracovní plochou

Pracovní plocha zobrazí standardizované mechanismus pro výměnu dat v dané aplikaci nebo mezi aplikacemi. Typické použití pro pracovní plocha v aplikaci Xamarin.Mac je zpracování kopírování a vložení operace, ale jsou podporovány také řadu dalších operací (například přetažení & vyřaďte a aplikačních služeb).

Chcete-li získat můžete rychle vypnout základů, přidáme začínat jednoduchý, praktické Úvod k použití v aplikaci Xamarin.Mac pracovních plochách. Později poskytujeme podrobné vysvětlení, jak funguje s pracovní plochou a metody použité.

V tomto příkladu budeme vytvářet jednoduché dokumentů na základě aplikace, která spravuje okno obsahující zobrazení obrazu. Uživatel bude moci kopírovat a vkládat obrázky mezi dokumenty v aplikaci a do nebo z jiných aplikací nebo více oken uvnitř stejné aplikaci.

### <a name="creating-the-xamarin-project"></a>Vytvoření projektu Xamarin

Nejprve přidáme vytvořte novou aplikaci Xamarin.Mac dokumentů na základě, že jsme se přidávání kopírování a vložte podporu pro.

Postupujte takto:

1. Spuštění sady Visual Studio pro Mac a kliknutím **nový projekt...**  odkaz.
2. Vyberte **Mac** > **aplikace** > **kakao aplikace**, klikněte **Další** tlačítko: 

    [![Vytvoření nového projektu aplikace kakao](copy-paste-images/sample01.png "vytvoření nového projektu aplikace kakao")](copy-paste-images/sample01-large.png)
3. Zadejte `MacCopyPaste` pro **název projektu** a všem ostatním jako výchozí. Klikněte na tlačítko Další: 

    [![Nastavení názvu projektu](copy-paste-images/sample01a.png "nastavení názvu projektu")](copy-paste-images/sample01a-large.png)

4. Klikněte **vytvořit** tlačítko: 

    [![Potvrzení nová nastavení projektu](copy-paste-images/sample02.png "potvrzení nová nastavení projektu")](copy-paste-images/sample02-large.png)

### <a name="add-an-nsdocument"></a>Přidat NSDocument

Další přidáme vlastní `NSDocument` třídu, která bude sloužit jako úložiště pozadí pro uživatelské rozhraní aplikace. Bude obsahovat jedno zobrazení bitové kopie a vědět, jak zkopírovat bitovou kopii ze zobrazení do výchozí pracovní plochy a jak provést bitové kopie z výchozí pracovní plochy a zobrazit v zobrazení bitové kopie.

Klikněte pravým tlačítkem na projekt Xamarin.Mac v **řešení Pad** a vyberte **přidat** > **nový soubor...** :

![Přidání do projektu NSDocument](copy-paste-images/sample03.png "přidání NSDocument do projektu")

Zadejte `ImageDocument` pro **název** a klikněte na **nový** tlačítko. Upravit **ImageDocument.cs** třídy a nastavit jej vypadat třeba takto:

```csharp
using System;
using AppKit;
using Foundation;
using ObjCRuntime;

namespace MacCopyPaste
{
    [Register("ImageDocument")]
    public class ImageDocument : NSDocument
    {
        #region Computed Properties
        public NSImageView ImageView {get; set;}

        public ImageInfo Info { get; set; } = new ImageInfo();

        public bool ImageAvailableOnPasteboard {
            get {
                // Initialize the pasteboard
                NSPasteboard pasteboard = NSPasteboard.GeneralPasteboard;
                Class [] classArray  = { new Class ("NSImage") };

                // Check to see if an image is on the pasteboard
                return pasteboard.CanReadObjectForClasses (classArray, null);
            }
        }
        #endregion

        #region Constructor
        public ImageDocument ()
        {
        }
        #endregion

        #region Public Methods
        [Export("CopyImage:")]
        public void CopyImage(NSObject sender) {

            // Grab the current image
            var image = ImageView.Image;

            // Anything to process?
            if (image != null) {
                // Get the standard pasteboard
                var pasteboard = NSPasteboard.GeneralPasteboard;

                // Empty the current contents
                pasteboard.ClearContents();

                // Add the current image to the pasteboard
                pasteboard.WriteObjects (new NSImage[] {image});

                // Save the custom data class to the pastebaord
                pasteboard.WriteObjects (new ImageInfo[] { Info });

                // Using a Pasteboard Item
                NSPasteboardItem item = new NSPasteboardItem();
                string[] writableTypes = {"public.text"};

                // Add a data provier to the item
                ImageInfoDataProvider dataProvider = new ImageInfoDataProvider (Info.Name, Info.ImageType);
                var ok = item.SetDataProviderForTypes (dataProvider, writableTypes);

                // Save to pasteboard
                if (ok) {
                    pasteboard.WriteObjects (new NSPasteboardItem[] { item });
                }
            }

        }

        [Export("PasteImage:")]
        public void PasteImage(NSObject sender) {

            // Initialize the pasteboard
            NSPasteboard pasteboard = NSPasteboard.GeneralPasteboard;
            Class [] classArray  = { new Class ("NSImage") };

            bool ok = pasteboard.CanReadObjectForClasses (classArray, null);
            if (ok) {
                // Read the image off of the pasteboard
                NSObject [] objectsToPaste = pasteboard.ReadObjectsForClasses (classArray, null);
                NSImage image = (NSImage)objectsToPaste[0];

                // Display the new image
                ImageView.Image = image;
            }

            Class [] classArray2 = { new Class ("ImageInfo") };
            ok = pasteboard.CanReadObjectForClasses (classArray2, null);
            if (ok) {
                // Read the image off of the pasteboard
                NSObject [] objectsToPaste = pasteboard.ReadObjectsForClasses (classArray2, null);
                ImageInfo info = (ImageInfo)objectsToPaste[0];

            }

        }
        #endregion
    }
}
```

Podívejme se na některé z kód rozpis naleznete níže.

Následující kód obsahuje vlastnosti, které chcete otestovat existenci data bitové kopie na pracovní ploše výchozí, pokud bitová kopie je k dispozici, `true` jinak se vrátí `false`:

```csharp
public bool ImageAvailableOnPasteboard {
    get {
        // Initialize the pasteboard
        NSPasteboard pasteboard = NSPasteboard.GeneralPasteboard;
        Class [] classArray  = { new Class ("NSImage") };

        // Check to see if an image is on the pasteboard
        return pasteboard.CanReadObjectForClasses (classArray, null);
    }
}
```

Následující kód zkopíruje bitovou kopii z pohledu připojené image do výchozí pracovní plochy:

```csharp
[Export("CopyImage:")]
public void CopyImage(NSObject sender) {

    // Grab the current image
    var image = ImageView.Image;

    // Anything to process?
    if (image != null) {
        // Get the standard pasteboard
        var pasteboard = NSPasteboard.GeneralPasteboard;

        // Empty the current contents
        pasteboard.ClearContents();

        // Add the current image to the pasteboard
        pasteboard.WriteObjects (new NSImage[] {image});

        // Save the custom data class to the pastebaord
        pasteboard.WriteObjects (new ImageInfo[] { Info });

        // Using a Pasteboard Item
        NSPasteboardItem item = new NSPasteboardItem();
        string[] writableTypes = {"public.text"};

        // Add a data provider to the item
        ImageInfoDataProvider dataProvider = new ImageInfoDataProvider (Info.Name, Info.ImageType);
        var ok = item.SetDataProviderForTypes (dataProvider, writableTypes);

        // Save to pasteboard
        if (ok) {
            pasteboard.WriteObjects (new NSPasteboardItem[] { item });
        }
    }

}
```

A následující kód vloží bitové kopie z výchozí pracovní plochy a zobrazí v zobrazení připojené image (Pokud je pracovní plocha obsahuje platnou bitovou kopií):

```csharp
[Export("PasteImage:")]
public void PasteImage(NSObject sender) {

    // Initialize the pasteboard
    NSPasteboard pasteboard = NSPasteboard.GeneralPasteboard;
    Class [] classArray  = { new Class ("NSImage") };

    bool ok = pasteboard.CanReadObjectForClasses (classArray, null);
    if (ok) {
        // Read the image off of the pasteboard
        NSObject [] objectsToPaste = pasteboard.ReadObjectsForClasses (classArray, null);
        NSImage image = (NSImage)objectsToPaste[0];

        // Display the new image
        ImageView.Image = image;
    }

    Class [] classArray2 = { new Class ("ImageInfo") };
    ok = pasteboard.CanReadObjectForClasses (classArray2, null);
    if (ok) {
        // Read the image off of the pasteboard
        NSObject [] objectsToPaste = pasteboard.ReadObjectsForClasses (classArray2, null);
        ImageInfo info = (ImageInfo)objectsToPaste[0]
    }
}
```

Tento dokument na místě vytvoříme uživatelské rozhraní pro aplikaci Xamarin.Mac.

### <a name="building-the-user-interface"></a>Vytvoření uživatelského rozhraní

Dvakrát klikněte **Main.storyboard** soubor otevřete v Xcode. V dalším kroku dobře přidat panelu nástrojů a bitovou kopii a nakonfigurovat je následujícím způsobem:

[![Úpravy panelu nástrojů](copy-paste-images/sample04.png "úpravy panelu nástrojů")](copy-paste-images/sample04-large.png)

Přidat kopírování a vkládání **položka panelu nástrojů Image** na levé straně panelu nástrojů. Budeme používat tyto zástupce pro kopírování a vkládání v nabídce Upravit. Dál přidejte čtyři **položek panelu nástrojů obrázků** na pravé straně panelu nástrojů. Použijeme tyto k naplnění bitovou kopii s některé výchozí Image.

Další informace o práci s panely nástrojů, najdete v tématu naše [panely nástrojů](~/mac/user-interface/toolbar.md) dokumentaci.

Dále umožňuje vystavit následující akce pro naše položky panelu nástrojů a bitovou kopii a výstupy dobře:

[![Vytváření výstupy a akce](copy-paste-images/sample05.png "vytváření výstupy a akce")](copy-paste-images/sample05-large.png)

Další informace o práci s výstupy a akce, najdete v tématu [výstupy a akce](~/mac/get-started/hello-mac.md#Outlets_and_Actions) části našich [Hello, Mac](~/mac/get-started/hello-mac.md) dokumentaci.

### <a name="enabling-the-user-interface"></a>Povolení uživatelského rozhraní

Naše uživatelského rozhraní, vytvořili v Xcode a naše elementu uživatelského rozhraní, které jsou zveřejňovány prostřednictvím akce a výstupy musíme přidejte kód k povolení uživatelského rozhraní. Dvakrát klikněte **ImageWindow.cs** souboru v **Pad řešení** vypadá takto:

```csharp
using System;
using Foundation;
using AppKit;

namespace MacCopyPaste
{
    public partial class ImageWindow : NSWindow
    {
        #region Private Variables
        ImageDocument document;
        #endregion

        #region Computed Properties
        [Export ("Document")]
        public ImageDocument Document {
            get {
                return document;
            }
            set {
                WillChangeValue ("Document");
                document = value;
                DidChangeValue ("Document");
            }
        }

        public ViewController ImageViewController {
            get { return ContentViewController as ViewController; }
        }

        public NSImage Image {
            get {
                return ImageViewController.Image;
            }
            set {
                ImageViewController.Image = value;
            }
        }
        #endregion

        #region Constructor
        public ImageWindow (IntPtr handle) : base (handle)
        {
        }
        #endregion

        #region Override Methods
        public override void AwakeFromNib ()
        {
            base.AwakeFromNib ();

            // Create a new document instance
            Document = new ImageDocument ();

            // Attach to image view
            Document.ImageView = ImageViewController.ContentView;
        }
        #endregion

        #region Public Methods
        public void CopyImage (NSObject sender)
        {
            Document.CopyImage (sender);
        }

        public void PasteImage (NSObject sender)
        {
            Document.PasteImage (sender);
        }

        public void ImageOne (NSObject sender)
        {
            // Load image
            Image = NSImage.ImageNamed ("Image01.jpg");

            // Set image info
            Document.Info.Name = "city";
            Document.Info.ImageType = "jpg";
        }

        public void ImageTwo (NSObject sender)
        {
            // Load image
            Image = NSImage.ImageNamed ("Image02.jpg");

            // Set image info
            Document.Info.Name = "theater";
            Document.Info.ImageType = "jpg";
        }

        public void ImageThree (NSObject sender)
        {
            // Load image
            Image = NSImage.ImageNamed ("Image03.jpg");

            // Set image info
            Document.Info.Name = "keyboard";
            Document.Info.ImageType = "jpg";
        }

        public void ImageFour (NSObject sender)
        {
            // Load image
            Image = NSImage.ImageNamed ("Image04.jpg");

            // Set image info
            Document.Info.Name = "trees";
            Document.Info.ImageType = "jpg";
        }
        #endregion
    }
}
```

Podívejme se na tento kód rozpis naleznete níže.

Nejprve zveřejňujeme instanci `ImageDocument` třídu, která jsme vytvořili výše:

```csharp
private ImageDocument _document;
...

[Export ("Document")]
public ImageDocument Document {
    get { return _document; }
    set {
        WillChangeValue ("Document");
        _document = value;
        DidChangeValue ("Document");
    }
}
```

Pomocí `Export`, `WillChangeValue` a `DidChangeValue`, máme instalační program `Document` vlastnost umožňující klíč-hodnota kódování a datové vazby v Xcode.

Také zveřejňujeme bitovou kopii z bitové kopie a jsme přidali na naše uživatelské rozhraní v Xcode s následující vlastnost:

```csharp
public ViewController ImageViewController {
    get { return ContentViewController as ViewController; }
}

public NSImage Image {
    get {
        return ImageViewController.Image;
    }
    set {
        ImageViewController.Image = value;
    }
}
```

Pokud hlavní okno načíst a zobrazit, vytvoříme instanci naše `ImageDocument` třídy a rozhraní pro bitovou kopii připojit i do ní následující kód:

```csharp
public override void AwakeFromNib ()
{
    base.AwakeFromNib ();

    // Create a new document instance
    Document = new ImageDocument ();

    // Attach to image view
    Document.ImageView = ImageViewController.ContentView;
}
```

Nakonec v reakci na uživatele na zkopírovat a vložit položky panelu nástrojů na tlačítko říkáme instanci `ImageDocument` třídy vykonávají samotnou práci:

```csharp
partial void CopyImage (NSObject sender) {
    Document.CopyImage(sender);
}

partial void PasteImage (Foundation.NSObject sender) {
    Document.PasteImage(sender);
}
```

### <a name="enabling-the-file-and-edit-menus"></a>Povolování nabídek souboru a úpravy

Poslední věcí, kterou je potřeba udělat, je povolit **nový** položky nabídky z **soubor** (Chcete-li vytvořit nové instance třídy Naše hlavní okno), v nabídce a umožňuje **Vyjmout**, **kopie**  a **vložení** položky nabídky z **upravit** nabídky.

Chcete-li povolit **nový** nabídky Upravit položku, **AppDelegate.cs** souboru a přidejte následující kód:

```csharp
public int UntitledWindowCount { get; set;} =1;
...

[Export ("newDocument:")]
void NewDocument (NSObject sender) {
    // Get new window
    var storyboard = NSStoryboard.FromName ("Main", null);
    var controller = storyboard.InstantiateControllerWithIdentifier ("MainWindow") as NSWindowController;

    // Display
    controller.ShowWindow(this);

    // Set the title
    controller.Window.Title = (++UntitledWindowCount == 1) ? "untitled" : string.Format ("untitled {0}", UntitledWindowCount);
}
```

Další informace najdete v tématu [práce s více oken](~/mac/user-interface/window.md) části našich [Windows](~/mac/user-interface/window.md) dokumentaci.

Chcete-li povolit **Vyjmout**, **kopie** a **vložení** upravit položky nabídky **AppDelegate.cs** souboru a přidejte následující kód:

```csharp
[Export("copy:")]
void CopyImage (NSObject sender)
{
    // Get the main window
    var window = NSApplication.SharedApplication.KeyWindow as ImageWindow;

    // Anything to do?
    if (window == null)
        return;

    // Copy the image to the clipboard
    window.Document.CopyImage (sender);
}

[Export("cut:")]
void CutImage (NSObject sender)
{
    // Get the main window
    var window = NSApplication.SharedApplication.KeyWindow as ImageWindow;

    // Anything to do?
    if (window == null)
        return;

    // Copy the image to the clipboard
    window.Document.CopyImage (sender);

    // Clear the existing image
    window.Image = null;
}

[Export("paste:")]
void PasteImage (NSObject sender)
{
    // Get the main window
    var window = NSApplication.SharedApplication.KeyWindow as ImageWindow;

    // Anything to do?
    if (window == null)
        return;

    // Paste the image from the clipboard
    window.Document.PasteImage (sender);
}
```

Pro každou položku nabídky jsme získat okno aktuální, nejhornější, klíče a vysílat na našem `ImageWindow` třídy:

```csharp
var window = NSApplication.SharedApplication.KeyWindow as ImageWindow;
```

Odtud říkáme `ImageDocument` instance třídy tohoto okna zpracování kopírování a vložení akce. Příklad: 

```csharp
window.Document.CopyImage (sender);
```

Chceme jenom **Vyjmout**, **kopie** a **vložení** položky nabídky, které budou dostupné v případě, že je image data na pracovní ploše výchozí nebo v dobře aktuální aktivní okno image.

Přidejme **EditMenuDelegate.cs** do projektu Xamarin.Mac souboru a nastavit jej vypadat třeba takto:

```csharp
using System;
using AppKit;

namespace MacCopyPaste
{
    public class EditMenuDelegate : NSMenuDelegate
    {
        #region Override Methods
        public override void MenuWillHighlightItem (NSMenu menu, NSMenuItem item)
        {
        }

        public override void NeedsUpdate (NSMenu menu)
        {
            // Get list of menu items
            NSMenuItem[] Items = menu.ItemArray ();

            // Get the key window and determine if the required images are available
            var window = NSApplication.SharedApplication.KeyWindow as ImageWindow;
            var hasImage = (window != null) && (window.Image != null);
            var hasImageOnPasteboard = (window != null) && window.Document.ImageAvailableOnPasteboard;

            // Process every item in the menu
            foreach(NSMenuItem item in Items) {
                // Take action based on the menu title
                switch (item.Title) {
                case "Cut":
                case "Copy":
                case "Delete":
                    // Only enable if there is an image in the view
                    item.Enabled = hasImage;
                    break;
                case "Paste":
                    // Only enable if there is an image on the pasteboard
                    item.Enabled = hasImageOnPasteboard;
                    break;
                default:
                    // Only enable the item if it has a sub menu
                    item.Enabled = item.HasSubmenu;
                    break;
                }
            }
        }
        #endregion
    }
}
```

Znovu, jsme získat aktuální, nejhornější okna a použít jeho `ImageDocument` instance třídy se, zda existuje data požadovaná obrázku. Pak používáme `MenuWillHighlightItem` na základě metod k povolení nebo zakázání každou položku v tomto stavu.

Upravit **AppDelegate.cs** souboru a nastavit `DidFinishLaunching` metoda vypadá takto:
 
```csharp
public override void DidFinishLaunching (NSNotification notification)
{
    // Disable automatic item enabling on the Edit menu
    EditMenu.AutoEnablesItems = false;
    EditMenu.Delegate = new EditMenuDelegate ();
}
```

Nejprve Zakážeme automatické povolení a zákaz položek nabídky v nabídce Upravit. V dalším kroku jsme připojte instanci `EditMenuDelegate` třídu, která jsme vytvořili výše.

Další informace najdete v tématu naše [nabídky](~/mac/user-interface/menu.md) dokumentaci.

### <a name="testing-the-app"></a>Testování aplikace

S vše na místě jsme připraveni k testování aplikace. Sestavení a spuštění aplikace a zobrazí se hlavní rozhraní:

![Spuštění aplikace](copy-paste-images/run01.png "spuštění aplikace")

Pokud otevřete v nabídce Upravit, Všimněte si, že **Vyjmout**, **kopie** a **vložení** jsou zakázané, protože neexistuje žádný obrázek v bitové kopii dobře nebo ve výchozí pracovní plochy:

![Otevření nabídky Úpravy](copy-paste-images/run02.png "otevření nabídky Upravit")

Pokud dobře přidat bitovou kopii do bitové kopie a znovu otevřete v nabídce Upravit, se teď povolené položky:

![Zobrazení položek nabídek Úpravy jsou povolené](copy-paste-images/run03.png "zobrazující položky nabídky Úpravy jsou povolené.")

Pokud je zkopírovat bitovou kopii a vybrat **nový** z nabídky Soubor můžete vložit do nového okna této bitové kopie:

![Vkládání bitovou kopii do nové okno](copy-paste-images/run04.png "vkládání bitovou kopii do nové okno")

V následujících částech provedeme podrobný pohled práce s pracovní plochou v aplikaci Xamarin.Mac.

## <a name="about-the-pasteboard"></a>O s pracovní plochou

V systému macOS (dříve označované jako OS X) s pracovní plochou (`NSPasteboard`) poskytuje podporu pro několik server zpracovává jako je kopírování a vkládání, přetažením či aplikačních služeb. V následujících částech provedeme bližší pohled na několik klíčových konceptů pracovní ploše.

### <a name="what-is-a-pasteboard"></a>Co je pracovní ploše?

`NSPasteboard` Třída poskytuje standardizovaná mechanismus pro výměnu informací mezi aplikacemi nebo v rámci dané aplikace. Hlavní funkce pracovní ploše je pro zpracování operace kopírování a vložení:

1. Když uživatel vybere v aplikaci a používá **Vyjmout** nebo **kopie** položky nabídky, jeden nebo více reprezentace vybrané položky jsou umístěny na pracovní ploše.
2. Když uživatel používá **vložení** položku nabídky (v rámci stejné aplikaci nebo jinou), je verze dat, který může zpracovat zkopírovaných z pracovní ploše a přidat do aplikace.

Méně zřejmé používá pracovní plochy zahrnují najít, přetáhněte, přetáhněte a aplikace služby operace:

- Když uživatel zahájí operaci přetažení, přetáhněte data zkopírován k pracovní ploše. Pokud operace tažení končí pokles do jiné aplikaci, aplikaci zkopíruje data z pracovní plocha.
- Pro překlad služby data k převodu se zkopírují na pracovní plochu žádající aplikací. Služba aplikace načte data z pracovní plocha, nemá překlad, pak vloží data zpět na pracovní ploše.

Ve své nejjednodušší podobě pracovních plochách slouží k přesouvání dat v dané aplikaci nebo mezi aplikacemi a pro ně existovat v oblasti speciální globální paměť mimo proces aplikace. Koncepty pracovních plochách jsou snadno grasps, existuje několik složitější podrobnosti, které musí vzít v úvahu. Ty se budeme rozpis naleznete níže.

### <a name="named-pasteboards"></a>Pojmenované pracovních plochách

Pracovní ploše může být veřejných nebo privátních a mohou být použity pro různé účely v aplikaci nebo mezi více aplikacemi. systému macOS poskytuje několik standardní pracovních plochách, každý s konkrétní, dobře definované využití:

- `NSGeneralPboard` -Výchozí pracovní plochy **Vyjmout**, **kopie** a **vložení** operace.
- `NSRulerPboard` – Podporuje **Vyjmout**, **kopie** a **vložení** operací na **pravítek**.
- `NSFontPboard` – Podporuje **Vyjmout**, **kopie** a **vložení** operací na `NSFont` objekty.
- `NSFindPboard` – Podporuje specifické pro aplikaci najít panely, které můžete sdílet hledaný text.
- `NSDragPboard` – Podporuje **přetažením** operace.

Většině případů budete používat jednu z pracovních plochách definovaná systémem. Ale můžou nastat situace, které vyžadují, abyste vytvořili vlastní pracovních plochách. V těchto situacích můžete použít `FromName (string name)` metodu `NSPasteboard` třídy za účelem vytvoření vlastní pracovní plocha se zadaným názvem.

Volitelně můžete volat `CreateWithUniqueName` metodu `NSPasteboard` třídy za účelem vytvoření jedinečným názvem pracovní plocha.

### <a name="pasteboard-items"></a>Pracovní ploše položky

Každá část dat, která zapisuje aplikace k pracovní ploše se považuje za _položku pracovní plocha_ a pracovní ploše může obsahovat více položek ve stejnou dobu. Tímto způsobem můžete aplikaci zápisu několik verzí dat byly zkopírovány na pracovní ploše (například ve formátu prostého textu a formátovaný text) a načítání aplikace může číst vypnout pouze data, může zpracovat (například prostý text pouze).

### <a name="data-representations-and-uniform-type-identifiers"></a>Reprezentace dat a identifikátory uniform typu

Pracovní ploše operace trvají obvykle mezi dva (nebo více) aplikace, které nemají žádné informace o sobě navzájem nebo typy dat, že každý zpracovat. Jak jsme uvedli výše v části, chcete-li maximalizovat potenciál ke sdílení informací, pracovní ploše mohou být uloženy více reprezentace dat je zkopírovat a vložit.

Každý reprezentace je identifikován pomocí Uniform typ identifikátor (UTI), což není nic jiného než jednoduchý řetězec, který jednoznačně identifikuje typ data se zobrazí (Další informace najdete v tématu společnosti Apple [Uniform přehled identifikátory typu ](https://developer.apple.com/library/prerelease/mac/documentation/FileManagement/Conceptual/understanding_utis/understand_utis_intro/understand_utis_intro.html#//apple_ref/doc/uid/TP40001319) dokumentaci). 

Pokud vytváříte vlastní datový typ (například kreslení objekt v vektor kreslení aplikace), můžete vytvořit vlastní UTI jednoznačně identifikovat v kopírování a vložení operace.

Při aplikaci připraví se vložit daty zkopírovanými z pracovní ploše, musí ho najít reprezentaci, která nejlépe odpovídá jeho dalo (Pokud žádný neexistuje). Obvykle to bude nejkomplexnější typ k dispozici (například formátovaný text pro aplikaci word zpracování), návratem zpět k nejjednodušší formuláře, které jsou k dispozici jako požadované (prostý text pro jednoduché textovém editoru).

<a name="Promised_Data" />

### <a name="promised-data"></a>Přislíbeném dat

Obecně řečeno měl by poskytnout tolik reprezentace dat kopírovány co chcete maximalizovat sdílení mezi aplikacemi. Ale kvůli omezení čas nebo paměti, může být nepraktické každý typ dat ve skutečnosti zapisovat do pracovní plocha.

V takovém případě první reprezentace dat můžete umístit na pracovní ploše a přijímající aplikace může požadovat znázornění jiný, který může být vygenerovaný na průběžně těsně před operaci vložení.

Při umístění počáteční položky na pracovní ploše, zadáte, jeden nebo více dostupných reprezentace jsou poskytovány objekt, který odpovídá `NSPasteboardItemDataProvider` rozhraní. Tyto objekty poskytne navíc reprezentace na vyžádání, jak vyžádala přijímající aplikace.

### <a name="change-count"></a>Změňte počet

Každý pracovní plocha udržuje _změnit počet_ , čas přírůstcích každý nový vlastník je deklarován. Aplikace můžete určit, pokud obsah s pracovní plochou se změnil od posledního ho zkontrolován kontrolou hodnoty počtu změn.

Použití `ChangeCount` a `ClearContents` metody `NSPasteboard` třída k úpravě počtu změn dané pracovní plocha.

## <a name="copying-data-to-a-pasteboard"></a>Kopírování dat na pracovní ploše

Operace kopírování provést první přístup k pracovní ploše, zrušíte všechny existující obsah a zápis tolik reprezentace dat, jako jsou vyžadovány k pracovní ploše.

Příklad:

```csharp
// Get the standard pasteboard
var pasteboard = NSPasteboard.GeneralPasteboard;

// Empty the current contents
pasteboard.ClearContents();

// Add the current image to the pasteboard
pasteboard.WriteObjects (new NSImage[] {image});
```

Obvykle budete právě psát pro obecné pracovní ploše, jak je uvedeno v příkladu nahoře v. Libovolný objekt, který můžete poslat `WriteObjects` metoda *musí* požadavkům `INSPasteboardWriting` rozhraní. Několik předdefinovaných třídy (například `NSString`, `NSImage`, `NSURL`, `NSColor`, `NSAttributedString`, a `NSPasteboardItem`) automaticky požadavkům na tomto rozhraní.

Pokud píšete třídu vlastních dat k pracovní ploše musí odpovídat `INSPasteboardWriting` rozhraní nebo být uzavřen do instance `NSPasteboardItem` – třída (najdete v článku [vlastní datové typy](#Custom_Data_Types) části).

## <a name="reading-data-from-a-pasteboard"></a>Čtení dat z pracovní ploše

Jak jsme uvedli výše, chcete-li maximalizovat potenciální pro sdílení dat mezi aplikacemi, více reprezentace kopírovaných dat lze zapisovat k pracovní ploše. Je přijímající aplikace k výběru nejkomplexnější verze možné pro své možnosti (Pokud žádný neexistuje).

### <a name="simple-paste-operation"></a>Operace vložení jednoduché

Čtení dat z pracovní plochy pomocí `ReadObjectsForClasses` metoda. Bude vyžadovat dva parametry:

1. Pole `NSObject` na základě typy tříd, které chcete číst z pracovní plocha. Byste měli objednat to s nejvíce požadovaný typ dat nejprve s všechny ostatní typy v snížení předvoleb.
2. Slovník obsahující další omezení (například omezení pro konkrétní typy obsahu, adresa URL) nebo slovníku prázdný, pokud jsou vyžadovány žádné další omezení.

Metoda vrátí pole položek, které splňují kritéria, která jsme předaná a proto obsahuje maximálně stejný počet datových typů, které jsou požadovány. je také možné, že žádný z požadovaných typů jsou přítomny, a vrátí prázdné pole.

Například následující kód kontroluje, pokud `NSImage` existuje v obecné pracovní ploše a zobrazí v dobře obrázku, pokud ano:

```csharp
[Export("PasteImage:")]
public void PasteImage(NSObject sender) {

    // Initialize the pasteboard
    NSPasteboard pasteboard = NSPasteboard.GeneralPasteboard;
    Class [] classArray  = { new Class ("NSImage") };

    bool ok = pasteboard.CanReadObjectForClasses (classArray, null);
    if (ok) {
        // Read the image off of the pasteboard
        NSObject [] objectsToPaste = pasteboard.ReadObjectsForClasses (classArray, null);
        NSImage image = (NSImage)objectsToPaste[0];

        // Display the new image
        ImageView.Image = image;
    }

    Class [] classArray2 = { new Class ("ImageInfo") };
    ok = pasteboard.CanReadObjectForClasses (classArray2, null);
    if (ok) {
        // Read the image off of the pasteboard
        NSObject [] objectsToPaste = pasteboard.ReadObjectsForClasses (classArray2, null);
        ImageInfo info = (ImageInfo)objectsToPaste[0];
            }
}
```

### <a name="requesting-multiple-data-types"></a>Vyžaduje více datové typy

Na základě typu aplikace Xamarin.Mac vytváří, to může být schopná zpracovat více reprezentace data vložená. V takovém případě existují dva scénáře pro načítání dat z pracovní plocha:

1. Ujistěte se, jediné volání `ReadObjectsForClasses` metoda a poskytování pole všech vyjádření, které očekáváte (v upřednostňovaném pořadí).
2. Více volat `ReadObjectsForClasses` typy pokaždé, když metoda požadující jiné pole.

Najdete v článku **jednoduchá operace vložení** části výše pro další informace o načítání dat z pracovní ploše.

### <a name="checking-for-existing-data-types"></a>Kontrola existující datové typy

V určitých časech, které chcete zkontrolovat, zda pracovní ploše obsahuje znázornění dat bez ve skutečnosti čtení dat z pracovní plocha (například povolení **vložení** položky nabídky jenom v případě, že existuje platná data).

Volání `CanReadObjectForClasses` metoda pracovní plochy, které chcete zobrazit, pokud obsahuje daného typu.

Například následující kód určuje, zda obsahuje obecné pracovní ploše `NSImage` instance:

```csharp
public bool ImageAvailableOnPasteboard {
    get {
        // Initialize the pasteboard
        NSPasteboard pasteboard = NSPasteboard.GeneralPasteboard;
        Class [] classArray  = { new Class ("NSImage") };

        // Check to see if an image is on the pasteboard
        return pasteboard.CanReadObjectForClasses (classArray, null);
    }
}
```

### <a name="reading-urls-from-the-pasteboard"></a>Čtení z pracovní plocha adresy URL

Podle toho, funkce dané aplikace Xamarin.Mac, může být požadované adresy URL číst z pracovní ploše, ale pouze tehdy, pokud splňují danou sadu kritérií, (například odkazující na soubory nebo adresy URL konkrétní datový typ). V takovém případě můžete zadat další kritéria vyhledávání pomocí druhý parametr `CanReadObjectForClasses` nebo `ReadObjectsForClasses` metody.

<a name="Custom_Data_Types" />

## <a name="custom-data-types"></a>Vlastní datové typy

Existují situace, kdy budete muset uložit vlastní vlastní typy k pracovní ploše z Xamarin.Mac aplikace. Například vektor kreslení aplikaci, která umožňuje uživatelům kopírovat a Vložit kreslení objektů.

V takovém případě budete muset navrhnout vaše vlastní třída dat tak, aby dědila z `NSObject` a vyhovuje několik rozhraní (`INSCoding`, `INSPasteboardWriting` a `INSPasteboardReading`). Volitelně můžete `NSPasteboardItem` k zapouzdření data zkopírovat a vložit.

Obě tyto možnosti se bude vztahovat rozpis naleznete níže.

### <a name="using-a-custom-class"></a>Použití vlastní třídy

V této části jsme bude rozšíření na jednoduchý příklad aplikaci, kterou jsme vytvořili na začátku tohoto dokumentu a přidání vlastní třídy ke sledování informací o imagi, kterou jsme se kopírování a vkládání mezi windows.

Přidejte novou třídu do projektu a pojmenujte ji **ImageInfo.cs**. Upravte soubor a nastavit jej vypadat následovně:

```csharp
using System;
using AppKit;
using Foundation;

namespace MacCopyPaste
{
    [Register("ImageInfo")]
    public class ImageInfo : NSObject, INSCoding, INSPasteboardWriting, INSPasteboardReading
    {
        #region Computed Properties
        [Export("name")]
        public string Name { get; set; }

        [Export("imageType")]
        public string ImageType { get; set; }
        #endregion

        #region Constructors
        [Export ("init")]
        public ImageInfo ()
        {
        }
        
        public ImageInfo (IntPtr p) : base (p)
        {
        }

        [Export ("initWithCoder:")]
        public ImageInfo(NSCoder decoder) {

            // Decode data
            NSString name = decoder.DecodeObject("name") as NSString;
            NSString type = decoder.DecodeObject("imageType") as NSString;

            // Save data
            Name = name.ToString();
            ImageType = type.ToString ();
        }
        #endregion

        #region Public Methods
        [Export ("encodeWithCoder:")]
        public void EncodeTo (NSCoder encoder) {

            // Encode data
            encoder.Encode(new NSString(Name),"name");
            encoder.Encode(new NSString(ImageType),"imageType");
        }

        [Export ("writableTypesForPasteboard:")]
        public virtual string[] GetWritableTypesForPasteboard (NSPasteboard pasteboard) {
            string[] writableTypes = {"com.xamarin.image-info", "public.text"};
            return writableTypes;
        }

        [Export ("pasteboardPropertyListForType:")]
        public virtual NSObject GetPasteboardPropertyListForType (string type) {

            // Take action based on the requested type
            switch (type) {
            case "com.xamarin.image-info":
                return NSKeyedArchiver.ArchivedDataWithRootObject(this);
            case "public.text":
                return new NSString(string.Format("{0}.{1}", Name, ImageType));
            }

            // Failure, return null
            return null;
        }

        [Export ("readableTypesForPasteboard:")]
        public static string[] GetReadableTypesForPasteboard (NSPasteboard pasteboard){
            string[] readableTypes = {"com.xamarin.image-info", "public.text"};
            return readableTypes;
        }

        [Export ("readingOptionsForType:pasteboard:")]
        public static NSPasteboardReadingOptions GetReadingOptionsForType (string type, NSPasteboard pasteboard) {

            // Take action based on the requested type
            switch (type) {
            case "com.xamarin.image-info":
                return NSPasteboardReadingOptions.AsKeyedArchive;
            case "public.text":
                return NSPasteboardReadingOptions.AsString;
            }

            // Default to property list
            return NSPasteboardReadingOptions.AsPropertyList;
        }

        [Export ("initWithPasteboardPropertyList:ofType:")]
        public NSObject InitWithPasteboardPropertyList (NSObject propertyList, string type) {

            // Take action based on the requested type
            switch (type) {
            case "com.xamarin.image-info":
                return new ImageInfo();
            case "public.text":
                return new ImageInfo();
            }

            // Failure, return null
            return null;
        }
        #endregion
    }
}
    
```

V následujících částech provedeme podrobné podívejte se na tuto třídu.

#### <a name="inheritance-and-interfaces"></a>Dědičnost a rozhraní

Před třídu vlastních dat může být číst nebo z pracovní ploše, musí odpovídat `INSPastebaordWriting` a `INSPasteboardReading` rozhraní. Kromě toho musí dědit z `NSObject` a také požadavkům `INSCoding` rozhraní:

```csharp
[Register("ImageInfo")]
public class ImageInfo : NSObject, INSCoding, INSPasteboardWriting, INSPasteboardReading
...
```

Třída musí být také zpřístupněny jazyka Objective-C pomocí `Register` směrnice a musí vystavit všechny požadované vlastnosti nebo metody pomocí `Export`. Příklad:

```csharp
[Export("name")]
public string Name { get; set; }

[Export("imageType")]
public string ImageType { get; set; }
```

Nemůžeme se vystavení dvě pole dat, která bude tato třída obsahovat - název obrázku a jeho typ (jpg, png, atd.). 

Další informace najdete v tématu [třídy vystavení jazyka C# nebo metody jazyka Objective-C](~/mac/internals/how-it-works.md) části [Xamarin.Mac Internals](~/mac/internals/how-it-works.md) dokumentace, vysvětluje `Register` a `Export` atributy Umožňuje propojit až tříd jazyka C# a uživatelského rozhraní jazyka Objective-C objekty elementy.

#### <a name="constructors"></a>Konstruktory

Dva konstruktory (správně vystavený jazyka Objective-C) budou potřebné pro naše třída vlastních dat tak, aby ho mohou číst z pracovní ploše:

```csharp
[Export ("init")]
public ImageInfo ()
{
}

[Export ("initWithCoder:")]
public ImageInfo(NSCoder decoder) {

    // Decode data
    NSString name = decoder.DecodeObject("name") as NSString;
    NSString type = decoder.DecodeObject("imageType") as NSString;

    // Save data
    Name = name.ToString();
    ImageType = type.ToString ();
}
```

Nejprve zveřejňujeme _prázdný_ konstruktor pod výchozí metodu jazyka Objective-C `init`.

V dalším kroku zveřejňujeme `NSCoding` kompatibilní konstruktor, který se použije k vytvoření nové instance objektu z pracovní plocha při vkládání pod názvem exportovaný `initWithCoder`.

Tento konstruktor přijímá `NSCoder` (jako vytvořené `NSKeyedArchiver` při zapsána do pracovní plocha), extrahuje spárovat klíč/hodnota data a uloží ji do pole vlastností třídy data.

#### <a name="writing-to-the-pasteboard"></a>Zápis do s pracovní plochou

Pomocí odpovídají `INSPasteboardWriting` rozhraní, potřebujeme ke zveřejnění dvě metody a volitelně třetí metodu, tak, aby třídy je možné zapsat do pracovní plocha.

Nejdřív je potřeba říct, pracovní plocha jaké datový typ vyjádření, které vlastní třídy je možné zapsat do:

```csharp
[Export ("writableTypesForPasteboard:")]
public virtual string[] GetWritableTypesForPasteboard (NSPasteboard pasteboard) {
    string[] writableTypes = {"com.xamarin.image-info", "public.text"};
    return writableTypes;
}
```

Každý reprezentace je identifikován pomocí Uniform typ identifikátor (UTI), což není nic jiného než jednoduchý řetězec, který jednoznačně identifikuje typu dat, se zobrazí (Další informace najdete v tématu společnosti Apple [Uniform přehled identifikátory typu ](https://developer.apple.com/library/prerelease/mac/documentation/FileManagement/Conceptual/understanding_utis/understand_utis_intro/understand_utis_intro.html#//apple_ref/doc/uid/TP40001319) dokumentaci).

Pro naše vlastní formát vytváříme vlastní UTI: "informace com.xamarin.image" (Všimněte si, že je v zpětný zápis stejně jako identifikátor aplikace). Naše třída se taky může zápis standardní řetězec k pracovní ploše (`public.text`). 

Dále je potřeba vytvořit objekt požadovaný formát, který získá ve skutečnosti zapsána do pracovní plocha:

```csharp
[Export ("pasteboardPropertyListForType:")]
public virtual NSObject GetPasteboardPropertyListForType (string type) {

    // Take action based on the requested type
    switch (type) {
    case "com.xamarin.image-info":
        return NSKeyedArchiver.ArchivedDataWithRootObject(this);
    case "public.text":
        return new NSString(string.Format("{0}.{1}", Name, ImageType));
    }

    // Failure, return null
    return null;
}
```

Pro `public.text` typu, jsme se vrací jednoduchý, formátu `NSString` objektu. Pro vlastní `com.xamarin.image-info` typ se používá `NSKeyedArchiver` a `NSCoder` rozhraní ke kódování třídy archivu spárovat klíč/hodnota pro vlastní data. Budeme muset implementovat metodu ve skutečnosti zpracování kódování:

```csharp
[Export ("encodeWithCoder:")]
public void EncodeTo (NSCoder encoder) {

    // Encode data
    encoder.Encode(new NSString(Name),"name");
    encoder.Encode(new NSString(ImageType),"imageType");
}
```

Páry klíč/hodnota jednotlivých se zapisují do kodér a bude dekódovat pomocí druhý konstruktor, který jsme přidali výše.

Volitelně jsme může zahrnovat následující metodu k definování žádné možnosti při zápisu dat do pracovní plocha:

```csharp
[Export ("writingOptionsForType:pasteboard:"), CompilerGenerated]
public virtual NSPasteboardWritingOptions GetWritingOptionsForType (string type, NSPasteboard pasteboard) {
    return NSPasteboardWritingOptions.WritingPromised;
}
```

Aktuálně pouze `WritingPromised` možnost je k dispozici a by měl být použit při pouze dohodnutými a ve skutečnosti není zapsána do pracovní plocha daného typu. Další informace najdete v tématu [dohodnutými Data](#Promised_Data) část výše.

Pomocí těchto metod zavedené následující kód slouží k zápisu naše vlastní třídu k pracovní ploše:

```csharp
// Get the standard pasteboard
var pasteboard = NSPasteboard.GeneralPasteboard;

// Empty the current contents
pasteboard.ClearContents();

// Add info to the pasteboard
pasteboard.WriteObjects (new ImageInfo[] { Info });
```

#### <a name="reading-from-the-pasteboard"></a>Čtení ze s pracovní plochou

Pomocí odpovídají `INSPasteboardReading` rozhraní, potřebujeme ke zveřejnění tři metody tak, aby vlastní datové třídy lze číst z pracovní plocha.

Nejdřív je potřeba říct, pracovní plocha jaké datový typ vyjádření, které vlastní třídu může číst ze schránky:

```csharp
[Export ("readableTypesForPasteboard:")]
public static string[] GetReadableTypesForPasteboard (NSPasteboard pasteboard){
    string[] readableTypes = {"com.xamarin.image-info", "public.text"};
    return readableTypes;
}
```

Znovu, jsou definovány jako jednoduchý UTIs a se stejnými typy definované v **zápis k pracovní ploše** část výše.

Dále je potřeba říct pracovní plocha _jak_ každý typ UTI bude načten pomocí následující metody:

```csharp
[Export ("readingOptionsForType:pasteboard:")]
public static NSPasteboardReadingOptions GetReadingOptionsForType (string type, NSPasteboard pasteboard) {

    // Take action based on the requested type
    switch (type) {
    case "com.xamarin.image-info":
        return NSPasteboardReadingOptions.AsKeyedArchive;
    case "public.text":
        return NSPasteboardReadingOptions.AsString;
    }

    // Default to property list
    return NSPasteboardReadingOptions.AsPropertyList;
}
```

Pro `com.xamarin.image-info` typu, jsme informace o tom, pracovní plocha dekódování dvojice klíč/hodnota, kterou jsme vytvořili s `NSKeyedArchiver` při zápisu třídy k pracovní ploše voláním `initWithCoder:` konstruktor, který jsme přidali do třídy.

Nakonec je potřeba přidejte následující metodu přečíst další reprezentace UTI data z pracovní plocha:

```csharp
[Export ("initWithPasteboardPropertyList:ofType:")]
public NSObject InitWithPasteboardPropertyList (NSObject propertyList, string type) {

    // Take action based on the requested type
    switch (type) {
    case "public.text":
        return new ImageInfo();
    }

    // Failure, return null
    return null;
}
```

Všechny tyto metody jsou zavedené možné vlastní datové třídy přečíst z pracovní plocha, pomocí následujícího kódu:

```csharp
// Initialize the pasteboard
NSPasteboard pasteboard = NSPasteboard.GeneralPasteboard;
var classArrayPtrs = new [] { Class.GetHandle (typeof(ImageInfo)) };
NSArray classArray = NSArray.FromIntPtrs (classArrayPtrs);

// NOTE: Sending messages directly to the base Objective-C API because of this defect:
// https://bugzilla.xamarin.com/show_bug.cgi?id=31760
// Check to see if image info is on the pasteboard
ok = bool_objc_msgSend_IntPtr_IntPtr (pasteboard.Handle, Selector.GetHandle ("canReadObjectForClasses:options:"), classArray.Handle, IntPtr.Zero);

if (ok) {
    // Read the image off of the pasteboard
    NSObject [] objectsToPaste = NSArray.ArrayFromHandle<Foundation.NSObject>(IntPtr_objc_msgSend_IntPtr_IntPtr (pasteboard.Handle, Selector.GetHandle ("readObjectsForClasses:options:"), classArray.Handle, IntPtr.Zero));
    ImageInfo info = (ImageInfo)objectsToPaste[0];
}
```

### <a name="using-a-nspasteboarditem"></a>Použití NSPasteboardItem

Můžou nastat situace, když budete muset napsat vlastní položky k pracovní ploše, který nezaručují vytvoření vlastní třídy, nebo chcete poskytnout data ve formátu běžné podle potřeby. V takových situacích můžete použít `NSPasteboardItem`.

A `NSPasteboardItem` poskytuje jemně odstupňovanou kontrolu nad daty, která jsou zapsána do pracovní ploše a je určená pro dočasný přístup - ho by měla být uvolněna z po jeho byla zapsána na pracovní ploše.

#### <a name="writing-data"></a>Zápis dat

Zápis vlastních dat, aby se `NSPasteboardItem` budete muset zadat vlastní `NSPasteboardItemDataProvider`. Přidejte novou třídu do projektu a pojmenujte ji **ImageInfoDataProvider.cs**. Upravte soubor a nastavit jej vypadat následovně:

```csharp
using System;
using AppKit;
using Foundation;

namespace MacCopyPaste
{
    [Register("ImageInfoDataProvider")]
    public class ImageInfoDataProvider : NSPasteboardItemDataProvider
    {
        #region Computed Properties
        public string Name { get; set;}
        public string ImageType { get; set;}
        #endregion

        #region Constructors
        [Export ("init")]
        public ImageInfoDataProvider ()
        {
        }

        public ImageInfoDataProvider (string name, string imageType)
        {
            // Initialize
            this.Name = name;
            this.ImageType = imageType;
        }

        protected ImageInfoDataProvider (NSObjectFlag t){
        }

        protected internal ImageInfoDataProvider (IntPtr handle){

        }
        #endregion

        #region Override Methods
        [Export ("pasteboardFinishedWithDataProvider:")]
        public override void FinishedWithDataProvider (NSPasteboard pasteboard)
        {
            
        }

        [Export ("pasteboard:item:provideDataForType:")]
        public override void ProvideDataForType (NSPasteboard pasteboard, NSPasteboardItem item, string type)
        {

            // Take action based on the type
            switch (type) {
            case "public.text":
                // Encode the data to string 
                item.SetStringForType(string.Format("{0}.{1}", Name, ImageType),type);
                break;
            }

        }
        #endregion
    }
}
```

Jako jsme to udělali s třídou vlastních dat, je potřeba použít `Register` a `Export` direktivy ke zveřejnění na cíl C. Třída musí dědit z `NSPasteboardItemDataProvider` a musí implementovat `FinishedWithDataProvider` a `ProvideDataForType` metody.

Použití `ProvideDataForType` metody můžete zajistit data, která bude uzavřen do `NSPasteboardItem` následujícím způsobem:

```csharp
[Export ("pasteboard:item:provideDataForType:")]
public override void ProvideDataForType (NSPasteboard pasteboard, NSPasteboardItem item, string type)
{

    // Take action based on the type
    switch (type) {
    case "public.text":
        // Encode the data to string 
        item.SetStringForType(string.Format("{0}.{1}", Name, ImageType),type);
        break;
    }

}
```

V takovém případě jsme ukládání dva kusy informace o našich bitové kopie (název a hodnotu ImageType) a zápis do jednoduchého řetězce (`public.text`).

Typ zapsat data do pracovní ploše, použijte následující kód:

```csharp
// Get the standard pasteboard
var pasteboard = NSPasteboard.GeneralPasteboard;

// Using a Pasteboard Item
NSPasteboardItem item = new NSPasteboardItem();
string[] writableTypes = {"public.text"};

// Add a data provider to the item
ImageInfoDataProvider dataProvider = new ImageInfoDataProvider (Info.Name, Info.ImageType);
var ok = item.SetDataProviderForTypes (dataProvider, writableTypes);

// Save to pasteboard
if (ok) {
    pasteboard.WriteObjects (new NSPasteboardItem[] { item });
}
```

#### <a name="reading-data"></a>Čtení dat

Chcete-li číst data zpátky do pracovní plocha, použijte následující kód:

```csharp
// Initialize the pasteboard
NSPasteboard pasteboard = NSPasteboard.GeneralPasteboard;
Class [] classArray  = { new Class ("NSImage") };

bool ok = pasteboard.CanReadObjectForClasses (classArray, null);
if (ok) {
    // Read the image off of the pasteboard
    NSObject [] objectsToPaste = pasteboard.ReadObjectsForClasses (classArray, null);
    NSImage image = (NSImage)objectsToPaste[0];

    // Do something with data
    ...
}
            
Class [] classArray2 = { new Class ("ImageInfo") };
ok = pasteboard.CanReadObjectForClasses (classArray2, null);
if (ok) {
    // Read the image off of the pasteboard
    NSObject [] objectsToPaste = pasteboard.ReadObjectsForClasses (classArray2, null);
    
    // Do something with data
    ...
}
```

## <a name="summary"></a>Souhrn

Tento článek trvá podrobný pohled práce s pracovní plochou v aplikaci Xamarin.Mac podporu kopírování a vložení operace. Poprvé je jednoduchý příklad vás seznámí s operacemi standardní pracovních plochách. V dalším kroku trvalo podrobný pohled na pracovní ploše a jak číst a zapisovat data z něj. Nakonec hledá pomocí vlastní datový typ pro podporu kopírování a vkládání komplexní datových typů v rámci aplikace.



## <a name="related-links"></a>Související odkazy

- [MacCopyPaste (sample)](https://developer.xamarin.com/samples/mac/MacCopyPaste/)
- [Hello, Mac](~/mac/get-started/hello-mac.md)
- [Průvodce programováním v pracovní plochy](https://developer.apple.com/library/content/documentation/Cocoa/Conceptual/PasteboardGuide106/Articles/pbGettingStarted.html)
- [systému macOS Human Interface Guidelines](https://developer.apple.com/macos/human-interface-guidelines/overview/themes/)
