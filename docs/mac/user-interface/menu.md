---
title: Nabídky
description: Tento článek se zabývá práce s nabídkami v aplikaci Xamarin.Mac. Popisuje vytvoření a udržování nabídky a položek nabídky v Xcode a Tvůrce rozhraní a práce s nimi prostřednictvím kódu programu.
ms.prod: xamarin
ms.assetid: 5D367F8E-3A76-4995-8A89-488530FAD802
ms.technology: xamarin-mac
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/14/2017
ms.openlocfilehash: 50c9cf333ff7965bbdfbb964a2301e677eb6aa59
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/04/2018
---
# <a name="menus"></a>Nabídky

_Tento článek se zabývá práce s nabídkami v aplikaci Xamarin.Mac. Popisuje vytvoření a udržování nabídky a položek nabídky v Xcode a Tvůrce rozhraní a práce s nimi prostřednictvím kódu programu._

Při práci s C# a rozhraní .NET v aplikaci Xamarin.Mac, máte přístup do stejné kakao nabídek, které nemá vývojář pracující v Objective-C a Xcode. Protože Xamarin.Mac integruje přímo s Xcode, můžete vytvořit a udržovat vaše nabídek, nabídky a položky nabídky (nebo je můžete také vytvořit přímo v kódu jazyka C#) na Xcode rozhraní tvůrce.

Nabídky jsou nedílnou součástí aplikace systému Mac uživatelské prostředí a běžně zobrazují v různých částí uživatelského rozhraní:

- **Panel nabídek aplikace** -Toto je hlavní nabídky, který se zobrazí v horní části obrazovky pro každou aplikaci Mac.
- **Kontextové nabídky** – ty se zobrazí, když uživatel klikne pravým tlačítkem myši nebo ovládací prvek klikne na položku v okně.
- **Stavový řádek** -Toto je oblast na úplně pravé straně panelu nabídek aplikace, která se zobrazí v horní části obrazovky (nalevo od hodiny řádku nabídky) a zvětšování vlevo přidaná do ní.
- **Ukotvení nabídky** -nabídky pro každou aplikaci v ukotvení, se zobrazí, když uživatel klikne pravým tlačítkem myši nebo ovládací prvek klikne na ikonu aplikace nebo když uživatel left-clicks ikonu a obsahuje tlačítko myši směrem dolů.
- **Rozevírací seznamy a automaticky otevírané okno tlačítko** -rozbalovací tlačítka zobrazí vybrané položky a zobrazí seznam možností, které lze vybírat při kliknutí na uživatelem. Rozevírací seznam je typ rozbalovací tlačítka obvykle slouží k výběru příkazy, které jsou specifické pro kontext aktuální úlohy. Obě může vyskytovat kdekoli v okně.

[![Nabídce příklad](menu-images/intro01.png "nabídce příklad")](menu-images/intro01-large.png#lightbox)

V tomto článku vám nabídneme základní informace o práci s pruhy nabídky kakao, nabídky a položek nabídky v aplikaci Xamarin.Mac. Vysoce navržený na spolupracovat [Hello, Mac](~/mac/get-started/hello-mac.md) článek nejprve, konkrétně [Úvod do Xcode a rozhraní tvůrce](~/mac/get-started/hello-mac.md#Introduction_to_Xcode_and_Interface_Builder) a [výstupy a akce](~/mac/get-started/hello-mac.md#Outlets_and_Actions) oddíly, jak se popisuje klíčové koncepty a techniky, které budeme používat v tomto článku.

Můžete chtít podívejte se na [třídy vystavení jazyka C# nebo metody jazyka Objective-C](~/mac/internals/how-it-works.md) části [Xamarin.Mac Internals](~/mac/internals/how-it-works.md) dokumentu taky vysvětluje `Register` a `Export` atributy používá k navázání tříd jazyka C# jazyka Objective-C objekty a prvky uživatelského rozhraní.

## <a name="the-applications-menu-bar"></a>Panel nabídek aplikace 

Na rozdíl od aplikace spuštěné na operačním systému Windows, kde každé okno může mít svůj vlastní řádek nabídek k němu připojen všechny aplikace běžící v systému macOS má jeden nabídek, které v horní části obrazovky, který se používá pro každý okna v této aplikaci:

[![Panel nabídek](menu-images/appmenu01.png "řádku nabídek")](menu-images/appmenu01-large.png#lightbox)

Položky v této nabídce jsou aktivace nebo deaktivace na základě aktuálního kontextu nebo stav aplikace a jeho uživatelské rozhraní v každém okamžiku. Například: Pokud uživatel vybere textové pole, položky na **upravit** nabídky budou pocházet povoleno jako **kopie** a **Vyjmout**.

Podle Apple a ve výchozím nastavení mají všechny aplikace systému macOS standardní sadu nabídky a položky nabídky, které se zobrazují v řádku nabídek aplikace:

- **Nabídky Apple** – tato nabídka poskytuje přístup k systému široké položky, které jsou k dispozici pro uživatele za všech okolností, bez ohledu na to, jaké aplikace běží. Tyto položky nemůže být upraven sám vývojář.
- **Aplikace nabídky** -této nabídky tučným písmem zobrazí název aplikace a pomáhá identifikovat, jaké aplikace běží v současné době uživatele. Obsahuje položky, které platí pro aplikaci jako celek není daného dokumentu nebo procesu, jako je například opuštění aplikace.
- **Nabídka Soubor** – položky použít k vytvoření, otevření nebo uložení dokumentů, které vaše aplikace pracuje. Pokud vaše aplikace není na základě dokumentu, může tato nabídka přejmenovat nebo odstranit.
- **Upravit nabídku** – obsahuje příkazy, jako **Vyjmout**, **kopie**, a **vložení** které se používají k upravit nebo změnit prvky v uživatelském rozhraní aplikace.
- **Formát nabídky** – Pokud je aplikace pracuje s textem, tato nabídka blokování příkazy upravit formátování textu.
- **Nabídka Zobrazit** – obsahuje příkazy, které ovlivňují způsob zobrazení obsahu (Zobrazit) v uživatelském rozhraní aplikace.
- **Specifické pro aplikaci nabídky** – jedná se o žádné nabídky, které jsou specifické pro aplikaci (například záložky nabídky pro webový prohlížeč). By měl zobrazit mezi **zobrazení** a **okno** nabídky na panelu.
- **Nabídka okno** – obsahuje příkazy pro práci s windows v aplikaci, jakož i seznam aktuální otevřená okna.
- **Nabídka Nápověda** – Pokud vaše aplikace obsahuje nápovědu na obrazovce, v nabídce Nápověda naleznete by měla být v nabídce nejvíce vpravo na panelu. 

Další informace o řádku nabídek aplikace a standardní nabídky a položky nabídky, najdete v tématu společnosti Apple [Human Interface Guidelines](https://developer.apple.com/macos/human-interface-guidelines/menus/menu-anatomy/).

### <a name="the-default-application-menu-bar"></a>Panel nabídek výchozí aplikace

Vždy, když vytvoříte nový projekt Xamarin.Mac, automaticky získáte standardní, řádku výchozí aplikace nabídek, který má typické položky, které aplikace systému macOS by za normálních okolností mají (jak bylo popsáno výše v části). Panel nabídek výchozí aplikace je definována v **Main.storyboard** souboru (spolu s ostatními vaší aplikace uživatelského rozhraní) v části na projekt v **řešení Pad**:  

![Vyberte hlavní storyboard](menu-images/appmenu02.png "vyberte hlavní storyboard")

Dvakrát klikněte **Main.storyboard** soubor otevřete pro úpravy v Xcode na rozhraní tvůrce a vám bude nabídnuta rozhraní editoru nabídky:

[![Úpravy uživatelského rozhraní v Xcode](menu-images/defaultbar01.png "úpravy uživatelského rozhraní v Xcode")](menu-images/defaultbar01-large.png#lightbox)

Zde jsme můžete kliknutím na položky, jako **otevřete** položka nabídky v **soubor** nabídky a upravit nebo upravit jeho vlastnosti v **atributy Inspector**:

[![Úpravy atributy z nabídky](menu-images/defaultbar02.png "úpravy atributy z nabídky")](menu-images/defaultbar02-large.png#lightbox)

Budete se nám získat do přidávání, úpravy a odstranění nabídek a položky později v tomto článku. Pro teď my chceme jenom zobrazit jaké nabídky a položky nabídky jsou k dispozici ve výchozím nastavení a jak byli automaticky vystaveni kódu přes sadu předdefinovaných výstupy a akcí (Další informace najdete v našich [výstupy a akce](~/mac/get-started/hello-mac.md#Outlets_and_Actions) dokumentace).

Například, pokud jsme klikněte na **připojení Inspector** pro **otevřete** položky nabídky, uvidíte je automaticky drátové až `openDocument:` akce: 

[![Zobrazení připojené akce](menu-images/defaultbar03.png "zobrazení připojené akce")](menu-images/defaultbar03-large.png#lightbox)

Pokud vyberete **první respondér** v **rozhraní hierarchie** a posuňte se dolů v **připojení Inspector**, a zobrazí se vám definice `openDocument:` Akce, **otevřete** položky nabídky je připojen k (spolu s několika další výchozí akce pro aplikaci a nejsou automaticky svázanou ovládací prvky):

[![Zobrazení všech připojených akce](menu-images/defaultbar04.png "zobrazení všechny připojené akce")](menu-images/defaultbar04-large.png#lightbox) 

Proč je to důležité? V další části uvidí, jak se tyto akce automaticky definované pracovat s další prvky uživatelského rozhraní kakao automaticky povolit a zakázat položky nabídky, jakož i, nabízí integrované funkce pro položky.

Později budeme používat tyto vestavěné akce Povolit a zakázat položky z kódu a poskytují vlastní funkce, pokud je tato možnost vybrána.

<a name="Built-In_Menu_Functionality" />

### <a name="built-in-menu-functionality"></a>Funkce integrované nabídky

Pokud byste byli spustit nově vytvořené aplikace Xamarin.Mac před přidáním jakékoli položky uživatelského rozhraní nebo kód, můžete si všimnout, že některé položky se automaticky drátové up a povolí (pomocí plně funkce automaticky předdefinované), jako **Quit** položky v **aplikace** nabídky:

![Položku povoleno nabídky](menu-images/appmenu03.png "položku povoleno nabídky")

Při další položky nabídky, například **Vyjmout**, **kopie**, a **vložení** nejsou:

![Zakázat položky nabídky](menu-images/appmenu04.png "zakázané položky nabídky")

Pojďme zastavte aplikaci a dvakrát klikněte na **Main.storyboard** v soubor **Pad řešení** otevřete pro úpravy v Xcode je rozhraní tvůrce. V dalším kroku přetáhněte **textového zobrazení** z **knihovny** do okna řadiče zobrazení v **rozhraní editoru**:

[![Výběr zobrazení textu v knihovně](menu-images/appmenu05.png "výběru zobrazení textu v knihovně")](menu-images/appmenu05-large.png#lightbox)

V **Editor omezení** umožňuje připnout zobrazení textu okraje okna a nastavte ji kde zvětšování a zmenší se okno klepnutím na všechny čtyři red I světla v horní části editoru a na **přidat omezení 4** tlačítko:

[![Úpravy omezení](menu-images/appmenu06.png "úpravy omezení")](menu-images/appmenu06-large.png#lightbox)

Uloží změny do návrh uživatelského rozhraní a přepněte zpět sady Visual Studio pro Mac synchronizovat změny se Xamarin.Mac projektu. Nyní spusťte aplikaci, zadejte text do textového zobrazení, vyberte ho a otevřete **upravit** nabídky:

![Položky nabídky jsou automaticky povoleno nebo zakázáno](menu-images/appmenu07.png "položky nabídky jsou automaticky povoleno nebo zakázáno")

Všimněte si jak **Vyjmout**, **kopie**, a **vložení** položky jsou automaticky povolené a plně funkční, všechny bez nutnosti napsat jediný řádek kódu. 

Co se děje tady? Mějte na paměti, integrované předdefinovat akce, které pocházejí až položky nabídky Výchozí přes drátové sítě (jak je uvedené výše), většina prvky uživatelského rozhraní kakao, které jsou součástí systému macOS jste vytvořili v háky k určitým akcím (například `copy:`). Proto při jejich přidání do okna aktivní a vybrána, vyberte příslušnou položku nebo položky, které jsou připojené k této akci se automaticky povolí. Pokud uživatel vybere danou položku nabídky, funkcí, které jsou integrované v prvku uživatelského rozhraní je volána a provést všechny bez zásahu vývojáře.

### <a name="enabling-and-disabling-menus-and-items"></a>Povolování a zakazování nabídek a položek

Ve výchozím nastavení pokaždé, když uživatel události dojde `NSMenu` automaticky povolí nebo zakáže každý viditelné nabídky a nabídky položky na základě kontextu aplikace. Existují tři způsoby, jak povolit nebo zakázat položku:

- **Automatická nabídka povolení** -položku nabídky je povolena, pokud `NSMenu` můžete najít odpovídající objekt, který reaguje na akce, která položka je přes drátové sítě až. Například textového zobrazení výše, měl předdefinované háku k `copy:` akce.
- **Vlastní akce a validateMenuItem:** – pro všechny položky nabídky, která je vázána [okno nebo zobrazení vlastní akce kontroleru](#Working-with-Custom-Window-Actions), můžete přidat `validateMenuItem:` akce a ručně povolit nebo zakázat položky nabídky.
- **Povolení ruční nabídky** -ručně nastavit `Enabled` vlastnost jednotlivých `NSMenuItem` chcete povolit nebo zakázat jednotlivě každou položku v nabídce.

Vyberte systém, nastavte `AutoEnablesItems` vlastnost `NSMenu`. `true` automaticky (výchozí nastavení) a `false` je ruční. 

> [!IMPORTANT]
> Pokud chcete použít ruční nabídky povolení, žádné nabídky položky, včetně těch, které jsou řízené AppKit třídy jako `NSTextView`, jsou automaticky aktualizovány. Můžete je zodpovědná za povolování a zakazování všechny položky ručně v kódu.

#### <a name="using-validatemenuitem"></a>Pomocí validateMenuItem

Jak jsme uvedli výše, pro všechny položky nabídky, která je vázána [okno nebo zobrazení řadiče vlastní akce](#Working-with-Custom-Window-Actions), můžete přidat `validateMenuItem:` akce a ručně povolit nebo zakázat položky nabídky.

V následujícím příkladu `Tag` vlastnost se použije k rozhodování o typu položky nabídky, která bude mít povoleno nebo zakázáno podle `validateMenuItem:` akce na základě stavu vybraného textu v `NSTextView`. `Tag` Vlastnost byla nastavena v Tvůrci rozhraní pro každou položku nabídky:

![Nastavení vlastnosti značky](menu-images/validate01.png "nastavení vlastnosti značky")

A přidat do řadiče zobrazení následující kód:

```csharp
[Action("validateMenuItem:")]
public bool ValidateMenuItem (NSMenuItem item) {

    // Take action based on the menu item type
    // (As specified in its Tag)
    switch (item.Tag) {
    case 1:
        // Wrap menu items should only be available if
        // a range of text is selected
        return (TextEditor.SelectedRange.Length > 0);
    case 2:
        // Quote menu items should only be available if
        // a range is NOT selected.
        return (TextEditor.SelectedRange.Length == 0);
    }

    return true;
}
```

Když je spuštěn tento kód a není vybraný žádný text v `NSTextView`, dvě wrap nebudou k dispozici (ani v případě, že jsou připojené k akce v kontroleru zobrazení):

![Zakázáno zobrazování položek](menu-images/validate02.png "zobrazující zakázané položky")

Pokud je vybraná část textu a znovu otevřít nabídku, bude k dispozici položky nabídky dvě wrap:

![Povolit zobrazování položek](menu-images/validate03.png "zobrazuje povolené položky")

## <a name="enabling-and-responding-to-menu-items-in-code"></a>Povolení a zpracování položek nabídky v kódu

Jak jsme viděli výše, právě přidáním konkrétní prvky uživatelského rozhraní kakao naše návrh uživatelského rozhraní (například textové pole), několik položek nabídky Výchozí povolí a fungovat automaticky, bez nutnosti psaní jakéhokoli kódu. Další Podíváme se na přidání vlastní kód C# do našich Xamarin.Mac projektu povolit položku nabídky a poskytují funkce, když ho uživatel vybere.

Například umožňují indikované chceme, aby mohli používat uživateli **otevřete** položky v **souboru** nabídce vyberte složku. Vzhledem k tomu, že chceme to být celé aplikace funkce a bez omezení okna udělení nebo elementu uživatelského rozhraní, vytvoříme přidat kód pro zpracování to pro naše delegáta aplikace.

V **řešení Pad**, dvakrát klikněte `AppDelegate.CS` soubor otevřete pro úpravy:

![Výběr aplikace delegáta](menu-images/appmenu08.png "výběr delegáta aplikace")

Přidejte následující kód níže `DidFinishLaunching` metoda:

```csharp
[Export ("openDocument:")]
void OpenDialog (NSObject sender)
{
    var dlg = NSOpenPanel.OpenPanel;
    dlg.CanChooseFiles = false;
    dlg.CanChooseDirectories = true;

    if (dlg.RunModal () == 1) {
        var alert = new NSAlert () {
            AlertStyle = NSAlertStyle.Informational,
            InformativeText = "At this point we should do something with the folder that the user just selected in the Open File Dialog box...",
            MessageText = "Folder Selected"
        };
        alert.RunModal ();
    }
}
```

Umožňuje spustit nyní aplikaci a otevřete **souboru** nabídky: 

![V nabídce Soubor](menu-images/appmenu09.png "nabídky Soubor")

Všimněte si, že **otevřete** položky nabídky je teď povolené. Pokud jsme vybrali, zobrazí se dialogové okno otevřít:

![Dialogové okno Otevřít](menu-images/appmenu10.png "otevřete dialogové okno")

Když kliknete na **otevřete** tlačítko, zobrazí se zpráva naše výstrahy:

![Dialogové okno zprávu příklad](menu-images/appmenu11.png "zprávu příklad dialogové okno")

Klíč řádku byl `[Export ("openDocument:")]`, sdělí `NSMenu` , naše **AppDelegate** má metodu `void OpenDialog (NSObject sender)` který reaguje na `openDocument:` akce. Pokud z výše, budete si vzpomenout **otevřete** položky nabídky je automaticky přes drátové sítě až tato akce ve výchozím nastavení v Tvůrci rozhraní:

[![Zobrazení připojené akce](menu-images/defaultbar03.png "zobrazení připojené akce")](menu-images/defaultbar03-large.png#lightbox)

Další Podíváme se na vytváření vlastní nabídky, položek nabídky a akce a reagovat na je v kódu.

### <a name="working-with-the-open-recent-menu"></a>Práce s otevřít poslední nabídky

Ve výchozím nastavení **soubor** obsahuje nabídky **otevřít poslední** položku, která uchovává informace o posledních několik souborů, které uživatel otevřel s vaší aplikací. Pokud vytváříte `NSDocument` Xamarin.Mac aplikace na základě této nabídky se budou zpracovávat pro vás automaticky. Pro žádný jiný druh Xamarin.Mac aplikace bude zodpovědný za správu a reagovat na tuto položku nabídky ručně.

Ruční zpracování **otevřít poslední** nabídky, je nejprve nutné ho informovat, že nový soubor byla otevřít nebo uložit pomocí následujících:

```csharp
// Add document to the Open Recent menu
NSDocumentController.SharedDocumentController.NoteNewRecentDocumentURL(url);
```

I když není vaší aplikace pomocí `NSDocuments`, nadále používat `NSDocumentController` udržovat **otevřít poslední** nabídky odesláním `NSUrl` s umístěním souboru, který se `NoteNewRecentDocumentURL` metodu `SharedDocumentController`.

Dále je nutné přepsat `OpenFile` metoda delegáta aplikace, chcete-li spustit všechny soubory, které si uživatel vybere ze **otevřít poslední** nabídky. Příklad:

```csharp
public override bool OpenFile (NSApplication sender, string filename)
{
    // Trap all errors
    try {
        filename = filename.Replace (" ", "%20");
        var url = new NSUrl ("file://"+filename);
        return OpenFile(url);
    } catch {
        return false;
    }
}
```

Vrátí `true` Pokud soubor můžete otevřít, jinak vrátí `false` a integrované upozornění se zobrazí uživateli, že soubor nelze otevřít.

Protože název souboru a cesta vrátil z **otevřít poslední** nabídky, může obsahovat mezeru, potřebujeme, abyste se vyhnuli správně tento znak před vytvořením `NSUrl` nebo jsme dojde k chybě. Provedeme to pomocí následujícího kódu:

```csharp
filename = filename.Replace (" ", "%20");
```

Nakonec se nám vytvořit `NSUrl` který odkazuje na soubor, použijte metodu helper v aplikaci delegovat na otevřete nové okno a načíst soubor do ní:

```csharp
var url = new NSUrl ("file://"+filename);
return OpenFile(url);
```

Vyžádání všechno, co společně, Podívejme se na příklad implementace v **AppDelegate.cs** souboru:

```csharp
using AppKit;
using Foundation;
using System.IO;
using System;

namespace MacHyperlink
{
    [Register ("AppDelegate")]
    public class AppDelegate : NSApplicationDelegate
    {
        #region Computed Properties
        public int NewWindowNumber { get; set;} = -1;
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

        public override bool OpenFile (NSApplication sender, string filename)
        {
            // Trap all errors
            try {
                filename = filename.Replace (" ", "%20");
                var url = new NSUrl ("file://"+filename);
                return OpenFile(url);
            } catch {
                return false;
            }
        }
        #endregion

        #region Private Methods
        private bool OpenFile(NSUrl url) {
            var good = false;

            // Trap all errors
            try {
                var path = url.Path;

                // Is the file already open?
                for(int n=0; n<NSApplication.SharedApplication.Windows.Length; ++n) {
                    var content = NSApplication.SharedApplication.Windows[n].ContentViewController as ViewController;
                    if (content != null && path == content.FilePath) {
                        // Bring window to front
                        NSApplication.SharedApplication.Windows[n].MakeKeyAndOrderFront(this);
                        return true;
                    }
                }

                // Get new window
                var storyboard = NSStoryboard.FromName ("Main", null);
                var controller = storyboard.InstantiateControllerWithIdentifier ("MainWindow") as NSWindowController;

                // Display
                controller.ShowWindow(this);

                // Load the text into the window
                var viewController = controller.Window.ContentViewController as ViewController;
                viewController.Text = File.ReadAllText(path);
                viewController.SetLanguageFromPath(path);
                viewController.View.Window.SetTitleWithRepresentedFilename (Path.GetFileName(path));
                viewController.View.Window.RepresentedUrl = url;

                // Add document to the Open Recent menu
                NSDocumentController.SharedDocumentController.NoteNewRecentDocumentURL(url);

                // Make as successful
                good = true;
            } catch {
                // Mark as bad file on error
                good = false;
            }

            // Return results
            return good;
        }
        #endregion

        #region actions
        [Export ("openDocument:")]
        void OpenDialog (NSObject sender)
        {
            var dlg = NSOpenPanel.OpenPanel;
            dlg.CanChooseFiles = true;
            dlg.CanChooseDirectories = false;

            if (dlg.RunModal () == 1) {
                // Nab the first file
                var url = dlg.Urls [0];

                if (url != null) {
                    // Open the document in a new window
                    OpenFile (url);
                }
            }
        }
        #endregion
    }
}
```

V závislosti na požadavcích vaší aplikace, nemusí má uživatel stejný soubor otevřete v okně více než jeden ve stejnou dobu. V našem příkladu aplikace, pokud uživatel vybere soubor, který je již otevřeno (buď z **otevřít poslední** nebo **otevřete...** položky nabídky), je okna, která obsahuje soubor začlenění na stránce do popředí.

K tomu, jsme použili následující kód v našem pomocnou metodu:

```csharp
var path = url.Path;

// Is the file already open?
for(int n=0; n<NSApplication.SharedApplication.Windows.Length; ++n) {
    var content = NSApplication.SharedApplication.Windows[n].ContentViewController as ViewController;
    if (content != null && path == content.FilePath) {
        // Bring window to front
        NSApplication.SharedApplication.Windows[n].MakeKeyAndOrderFront(this);
        return true;
    }
}
```

Jsme chtěli naše `ViewController` třída pro uložení cestu k souboru v jeho `Path` vlastnost. V dalším kroku jsme projít všechny aktuálně otevřené windows v aplikaci. Pokud soubor je již otevřen v jednom ze systému windows, načtením před všechny ostatní pomocí systému windows:

```csharp
NSApplication.SharedApplication.Windows[n].MakeKeyAndOrderFront(this);
```

Pokud není nalezena žádná shoda, je otevřené nové okno se načíst soubor a soubor je uvedeno v **otevřít poslední** nabídky:

```csharp
// Get new window
var storyboard = NSStoryboard.FromName ("Main", null);
var controller = storyboard.InstantiateControllerWithIdentifier ("MainWindow") as NSWindowController;

// Display
controller.ShowWindow(this);

// Load the text into the window
var viewController = controller.Window.ContentViewController as ViewController;
viewController.Text = File.ReadAllText(path);
viewController.SetLanguageFromPath(path);
viewController.View.Window.SetTitleWithRepresentedFilename (Path.GetFileName(path));
viewController.View.Window.RepresentedUrl = url;

// Add document to the Open Recent menu
NSDocumentController.SharedDocumentController.NoteNewRecentDocumentURL(url);
```

<a name="Working-with-Custom-Window-Actions" />

### <a name="working-with-custom-window-actions"></a>Práce s akcí vlastní okna

Stejně jako integrované **první respondér** akce, které jsou předem drátové na standardních položek nabídky, můžete vytvořit nové vlastní akce a propojit je položek nabídky v Tvůrci rozhraní.

Nejprve definujte vlastní akce v jednom okně řadičů vaší aplikace. Příklad:

```csharp
[Action("defineKeyword:")]
public void defineKeyword (NSObject sender) {
    // Preform some action when the menu is selected
    Console.WriteLine ("Request to define keyword");
}
```

Potom poklikejte na soubor storyboard aplikace v **řešení Pad** otevřete pro úpravy v Xcode je rozhraní tvůrce. Vyberte **první respondér** v části **scény aplikace**, potom přepnout **atributy Inspector**:

![Atributy Inspector](menu-images/action01.png "Inspector atributy")

Klikněte na tlačítko **+** tlačítko v dolní části **atributy Inspector** přidat nové vlastní akce:

![Přidání nové akce](menu-images/action02.png "přidání nové akce")

Poskytněte stejný název jako vlastní akci, kterou jste vytvořili na vašem řadiči okno:

![Úprava názvu akce](menu-images/action03.png "úpravy názvu akce")

Ovládací prvek klikněte na tlačítko a přetáhněte ji z položku nabídky **první respondér** v části **scény aplikace**. V automaticky otevřeném okně seznamu, vyberte novou akci, kterou jste právě vytvořili (`defineKeyword:` v tomto příkladu):

![Připojení akce](menu-images/action04.png "připojení akce")

Uložte změny do scénáře a vrátit k sadě Visual Studio pro Mac, aby synchronizovat změny. Pokud spustíte aplikaci, položku nabídky, které jste připojeni k vlastní akci bude automaticky být povoleno nebo zakázáno (na základě okno s akcí se otevřít) a vyberete položku nabídky se aktivují vypnout akce:

[![Testování nové akce](menu-images/action05.png "testování nové akce")](menu-images/action05-large.png#lightbox)

<a name="Adding,_Editing_and_Deleting_Menus" />

### <a name="adding-editing-and-deleting-menus"></a>Přidání, úpravy nebo odstranění nabídky

Jak jsme mohli vidět v předchozích částech, Xamarin.Mac aplikace se dodává s přednastavené počet výchozí nabídky a položky nabídky, které budou automaticky aktivovat a reagovat na konkrétní ovládacích prvků uživatelského rozhraní. Můžeme také viděli, jak přidat kód pro naše aplikace, která bude také povolit a reagovat na tyto položky výchozí.

V této části se podíváme na odebrání položky nabídky, které společnost Microsoft nepotřebuje, reorganizace nabídky a přidání nového nabídek, položek nabídky a akce.

Dvakrát klikněte **Main.storyboard** souboru v **řešení Pad** otevřete pro úpravy:

[![Úpravy uživatelského rozhraní v Xcode](menu-images/maint01.png "úpravy uživatelského rozhraní v Xcode")](menu-images/maint01-large.png#lightbox)

Pro naše konkrétní aplikaci Xamarin.Mac jsme nebudete používat výchozí **zobrazení** nabídky, přidáme ho odebrat. V **rozhraní hierarchie** vyberte **zobrazení** položky nabídky, která je součástí hlavní nabídce:

![Výběrem položky nabídky zobrazení](menu-images/maint02.png "vyberete položku nabídky zobrazení")

Stiskněte delete nebo backspace k odstranění v nabídce. V dalším kroku jsme nebudete používat všechny položky v **formát** nabídky a My chcete přesunout položky budeme používat získáte dílčí nabídky. V **rozhraní hierarchie** vyberte následující položky:

![Zvýraznění více položek](menu-images/maint03.png "zvýraznění více položek")

Přetáhněte na nadřazené položky **nabídky** podnabídce tam, kde právě jsou:

[![Přetahování položek nabídky do nabídky nadřazené](menu-images/maint04.png "přetahování položek nabídky do nadřazené nabídky")](menu-images/maint04-large.png#lightbox)

Vaše nabídky by teď měl vypadat podobně jako:

[![Položky v novém umístění](menu-images/maint05.png "položky v novém umístění")](menu-images/maint05-large.png#lightbox)

Další umožňuje přetáhněte **Text** dílčí nabídky se z v části **formátu** nabídky a umístěte ji na hlavním panelu nabídek mezi **formátu** a **okno** nabídky:

[![V nabídce Text](menu-images/maint06.png "nabídky textu")](menu-images/maint06-large.png#lightbox)

Přejděte zpět v části **formátu** nabídce a odstranění **písma** položku dílčí nabídky. Potom vyberte **formát** nabídky a přejmenujte ji "Font":

[![V nabídce písma](menu-images/maint07.png "písma nabídce")](menu-images/maint07-large.png#lightbox)

V dalším kroku vytvoříme vlastní nabídky předdefinované frází, které automaticky získat, připojí k text v zobrazení textu když je tato možnost vybrána. Do vyhledávacího pole v dolní části na **knihovny Inspector** typu v "nabídku." To znamená, že snadno najít a pracovat s všechny prvky uživatelského rozhraní nabídky:

![Knihovna Inspector](menu-images/maint08.png "Inspector knihovny")

Nyní Pojďme vytvořit naše nabídku pomocí následujícího postupu:

1. Přetáhněte **položky nabídky** z **knihovny Inspector** na panelu nabídek mezi **Text** a **okno** nabídky: 

    ![Když vyberete novou položku nabídky v knihovně](menu-images/maint10.png "vyberete novou položku nabídky v knihovně")
2. Přejmenujte položku "Frází": 

    [![Název nabídky nastavení](menu-images/maint09.png "nastavení název nabídky")](menu-images/maint09-large.png#lightbox)
3. Další přetáhněte **nabídky** z **knihovny Inspector**: 

    ![Výběr nabídky z knihovny](menu-images/maint11.png "výběrem nabídky z knihovny")
4. Pak vyřadit **nabídky** na novém **položky nabídky** jsme právě vytvořili a změnit její název "Frází": 

    [![Úpravy název nabídky](menu-images/maint12.png "úpravy název nabídky")](menu-images/maint12-large.png#lightbox)
5. Nyní Pojďme přejmenovat tři výchozí **položky nabídky** "Address", "Data" a "S pozdravem": 

    [![V nabídce frází](menu-images/maint13.png "frází nabídce")](menu-images/maint13-large.png#lightbox)
6. Přidejme čtvrtý **položky nabídky** tak, že přetáhnete **položky nabídky** z **knihovny Inspector** a volání metody "Podpis": 

    [![Úprava názvu](menu-images/maint14.png "úpravy název položky nabídky")](menu-images/maint14-large.png#lightbox)
7. Uložte změny do panelu nabídek.

Nyní vytvoříme sadu vlastní akce, aby naše nové položky nabídky se zveřejňují pro kód C#. V Xcode umožňuje přepnout **pomocníka** zobrazení:

[![Vytváření akce požadované](menu-images/maint15.png "vytváření požadované akce")](menu-images/maint15-large.png#lightbox)

Pojďme postupujte takto:

1. Přetáhněte ovládací prvek z **adresu** položku nabídky **AppDelegate.h** souboru.
2. Přepínač **připojení** typ **akce**: 

    [![Výběr typu akce](menu-images/maint17.png "vyberete typ akce")](menu-images/maint17-large.png#lightbox)
3. Zadejte **název** "phraseAddress" a stiskněte klávesu **Connect** tlačítko pro vytvoření nové akce: 

    [![Konfigurace akce](menu-images/maint18.png "konfigurace akce")](menu-images/maint18-large.png#lightbox)
4. Zopakujte výše uvedené kroky pro **datum**, **s pozdravem**, a **podpis** položky nabídky: 

    [![Dokončených akcí](menu-images/maint19.png "dokončených akcí")](menu-images/maint19-large.png#lightbox)
5. Uložte změny do panelu nabídek.

Dále je potřeba vytvořit výstupu pro naše textového zobrazení, takže jsme můžete upravit jeho obsah z kódu. Vyberte **ViewController.h** v soubor **pomocníka Editor** a vytvořit nové výstupu názvem `documentText`:

[![Vytváření výstupu](menu-images/maint20.png "vytváření výstupu")](menu-images/maint20-large.png#lightbox)

Vraťte se k Visual Studio pro Mac synchronizaci změn z Xcode. Dále upravit **ViewController.cs** souboru a nastavit jej vypadat třeba takto:

```csharp
using System;

using AppKit;
using Foundation;

namespace MacMenus
{
    public partial class ViewController : NSViewController
    {
        #region Application Access
        public static AppDelegate App {
            get { return (AppDelegate)NSApplication.SharedApplication.Delegate; }
        }
        #endregion

        #region Computed Properties
        public override NSObject RepresentedObject {
            get {
                return base.RepresentedObject;
            }
            set {
                base.RepresentedObject = value;
                // Update the view, if already loaded.
            }
        }

        public string Text {
            get { return documentText.Value; }
            set { documentText.Value = value; }
        } 
        #endregion

        #region Constructors
        public ViewController (IntPtr handle) : base (handle)
        {
        }
        #endregion

        #region Override Methods
        public override void ViewDidLoad ()
        {
            base.ViewDidLoad ();

            // Do any additional setup after loading the view.
        }

        public override void ViewWillAppear ()
        {
            base.ViewWillAppear ();

            App.textEditor = this;
        }

        public override void ViewWillDisappear ()
        {
            base.ViewDidDisappear ();

            App.textEditor = null;
        }
        #endregion
    }
}
```

To zpřístupňuje text naše textového zobrazení mimo `ViewController` třídy a informovalo delegát aplikace, když okno získá nebo ztratí fokus. Nyní upravit **AppDelegate.cs** souboru a nastavit jej vypadat třeba takto:

```csharp
using AppKit;
using Foundation;
using System;

namespace MacMenus
{
    [Register ("AppDelegate")]
    public partial class AppDelegate : NSApplicationDelegate
    {
        #region Computed Properties
        public ViewController textEditor { get; set;} = null;
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

        #region Custom actions
        [Export ("openDocument:")]
        void OpenDialog (NSObject sender)
        {
            var dlg = NSOpenPanel.OpenPanel;
            dlg.CanChooseFiles = false;
            dlg.CanChooseDirectories = true;

            if (dlg.RunModal () == 1) {
                var alert = new NSAlert () {
                    AlertStyle = NSAlertStyle.Informational,
                    InformativeText = "At this point we should do something with the folder that the user just selected in the Open File Dialog box...",
                    MessageText = "Folder Selected"
                };
                alert.RunModal ();
            }
        }

        partial void phrasesAddress (Foundation.NSObject sender) {

            textEditor.Text += "Xamarin HQ\n394 Pacific Ave, 4th Floor\nSan Francisco CA 94111\n\n";
        }

        partial void phrasesDate (Foundation.NSObject sender) {

            textEditor.Text += DateTime.Now.ToString("D");
        }

        partial void phrasesGreeting (Foundation.NSObject sender) {

            textEditor.Text += "Dear Sirs,\n\n";
        }

        partial void phrasesSignature (Foundation.NSObject sender) {

            textEditor.Text += "Sincerely,\n\nKevin Mullins\nXamarin,Inc.\n";
        }
        #endregion
    }
}
```

Zde jsme provedli `AppDelegate` částečné třídy tak, aby používáme akce a výstupy, definované v Tvůrci rozhraní. Můžeme také zpřístupnit `textEditor` ke sledování, které okno je aktuálně aktivní.

Ke zpracování naše vlastní nabídky a položky nabídky se používají následující metody:

```csharp
partial void phrasesAddress (Foundation.NSObject sender) {

    if (textEditor == null) return;
    textEditor.Text += "Xamarin HQ\n394 Pacific Ave, 4th Floor\nSan Francisco CA 94111\n\n";
}

partial void phrasesDate (Foundation.NSObject sender) {

    if (textEditor == null) return;
    textEditor.Text += DateTime.Now.ToString("D");
}

partial void phrasesGreeting (Foundation.NSObject sender) {

    if (textEditor == null) return;
    textEditor.Text += "Dear Sirs,\n\n";
}

partial void phrasesSignature (Foundation.NSObject sender) {

    if (textEditor == null) return;
    textEditor.Text += "Sincerely,\n\nKevin Mullins\nXamarin,Inc.\n";
}
```

Nyní když jsme spuštění aplikace, všechny položky v **frázi** nabídky aktivní a přidá udělení fráze do textového zobrazení při výběru:

![Příkladem spuštěné aplikace](menu-images/maint21.png "příklad spuštěné aplikace")

Teď, když máme základní informace o práci s panelu nabídek aplikace dolů, podíváme se na vytvoření vlastní kontextové nabídky.

### <a name="creating-menus-from-code"></a>Vytváření nabídek z kódu

Kromě vytvoření nabídky a položky nabídky pomocí Tvůrce rozhraní na Xcode, může nastat situace, když Xamarin.Mac aplikace potřebuje k vytvoření, úpravě nebo odebrání nabídky, dílčí nabídky nebo položku nabídky z kódu.

V následujícím příkladu je vytvořen třídu pro uložení informací o položek nabídky a dílčí nabídky, které se dynamicky vytvoří v chodu:

```csharp
using System;
using System.Collections.Generic;
using Foundation;
using AppKit;

namespace AppKit.TextKit.Formatter
{
    public class LanguageFormatCommand : NSObject
    {
        #region Computed Properties
        public string Title { get; set; } = "";
        public string Prefix { get; set; } = "";
        public string Postfix { get; set; } = "";
        public List<LanguageFormatCommand> SubCommands { get; set; } = new List<LanguageFormatCommand>();
        #endregion

        #region Constructors
        public LanguageFormatCommand () {

        }

        public LanguageFormatCommand (string title)
        {
            // Initialize
            this.Title = title;
        }

        public LanguageFormatCommand (string title, string prefix)
        {
            // Initialize
            this.Title = title;
            this.Prefix = prefix;
        }

        public LanguageFormatCommand (string title, string prefix, string postfix)
        {
            // Initialize
            this.Title = title;
            this.Prefix = prefix;
            this.Postfix = postfix;
        }
        #endregion
    }
}
```

#### <a name="adding-menus-and-items"></a>Přidání nabídek a položek

Tato třída definována, následující rutina provede analýzu kolekce `LanguageFormatCommand`objekty a rekurzivně vytvářet nové nabídky a položky nabídky jejich přidáním do dolní části existující nabídky (vytvořena v Tvůrci rozhraní), který byl předán v:

```csharp
private void AssembleMenu(NSMenu menu, List<LanguageFormatCommand> commands) {
    NSMenuItem menuItem;

    // Add any formatting commands to the Formatting menu
    foreach (LanguageFormatCommand command in commands) {
        // Add separator or item?
        if (command.Title == "") {
            menuItem = NSMenuItem.SeparatorItem;
        } else {
            menuItem = new NSMenuItem (command.Title);

            // Submenu?
            if (command.SubCommands.Count > 0) {
                // Yes, populate submenu
                menuItem.Submenu = new NSMenu (command.Title);
                AssembleMenu (menuItem.Submenu, command.SubCommands);
            } else {
                // No, add normal menu item
                menuItem.Activated += (sender, e) => {
                    // Apply the command on the selected text
                    TextEditor.PerformFormattingCommand (command);
                };
            }
        }
        menu.AddItem (menuItem);
    }
}
``` 

Pro libovolný `LanguageFormatCommand` objekt, který má prázdnou `Title` vlastnost, tato rutina vytvoří **položky nabídky oddělovače** (dynamické Šedá čára) mezi části nabídky:

```csharp
menuItem = NSMenuItem.SeparatorItem;
```

Pokud je zadaný název, vytvoří se nová položka nabídky s tímto názvem:

```csharp
menuItem = new NSMenuItem (command.Title);
``` 

Pokud `LanguageFormatCommand` objekt obsahuje podřízené `LanguageFormatCommand` objekty, je vytvořena dílčí nabídky a `AssembleMenu` metoda je volána k sestavení se této nabídky rekurzivně:

```csharp
menuItem.Submenu = new NSMenu (command.Title);
AssembleMenu (menuItem.Submenu, command.SubCommands);
```

Pro všechny nové položky nabídky, která nemá dílčí nabídky je přidán kód pro zpracování položky nabídky se vybraný uživatelem:

```csharp
menuItem.Activated += (sender, e) => {
    // Do something when the menu item is selected
    ...
};
```

#### <a name="testing-the-menu-creation"></a>Testování vytvoření nabídky

Se všemi výše uvedený kód na místě Pokud následující kolekce `LanguageFormatCommand` byly vytvořeny objekty:

```csharp
// Define formatting commands
FormattingCommands.Add(new LanguageFormatCommand("Stong","**","**"));
FormattingCommands.Add(new LanguageFormatCommand("Emphasize","_","_"));
FormattingCommands.Add(new LanguageFormatCommand("Inline Code","`","`"));
FormattingCommands.Add(new LanguageFormatCommand("Code Block","```\n","\n```"));
FormattingCommands.Add(new LanguageFormatCommand("Comment","<!--","-->"));
FormattingCommands.Add (new LanguageFormatCommand ());
FormattingCommands.Add(new LanguageFormatCommand("Unordered List","* "));
FormattingCommands.Add(new LanguageFormatCommand("Ordered List","1. "));
FormattingCommands.Add(new LanguageFormatCommand("Block Quote","> "));
FormattingCommands.Add (new LanguageFormatCommand ());

var Headings = new LanguageFormatCommand ("Headings");
Headings.SubCommands.Add(new LanguageFormatCommand("Heading 1","# "));
Headings.SubCommands.Add(new LanguageFormatCommand("Heading 2","## "));
Headings.SubCommands.Add(new LanguageFormatCommand("Heading 3","### "));
Headings.SubCommands.Add(new LanguageFormatCommand("Heading 4","#### "));
Headings.SubCommands.Add(new LanguageFormatCommand("Heading 5","##### "));
Headings.SubCommands.Add(new LanguageFormatCommand("Heading 6","###### "));
FormattingCommands.Add (Headings);

FormattingCommands.Add(new LanguageFormatCommand ());
FormattingCommands.Add(new LanguageFormatCommand("Link","[","]()"));
FormattingCommands.Add(new LanguageFormatCommand("Image","![]("-")"));
FormattingCommands.Add(new LanguageFormatCommand("Image Link","[ ![]("-")](linkimagehere/index.md)"));
```

A že kolekce předaný `AssembleMenu` – funkce (s **formát** nabídky nastaven jako základní), by se vytvořit následující dynamické nabídky a položky nabídky:

![Nové položky nabídky ve spuštěné aplikaci](menu-images/dynamic01.png "nové položky nabídky ve spuštěné aplikaci")

#### <a name="removing-menus-and-items"></a>Odstranění nabídek a položek

Pokud je třeba odebrat všechny nabídky nebo položku nabídky z uživatelského rozhraní aplikace, můžete použít `RemoveItemAt` metodu `NSMenu` třída jednoduše tím, že ho na nule na základě index položky k odebrání.

Například odebrat nabídky a vytvořit rutina výše uvedené položky nabídky, můžete použít následující kód:

```csharp
public void UnpopulateFormattingMenu(NSMenu menu) {

    // Remove any additional items
    for (int n = (int)menu.Count - 1; n > 4; --n) {
        menu.RemoveItemAt (n);
    }
}
```

V případě výše uvedený kód budou vytvořeny první čtyři nabídky položky v Xcode na tvůrce rozhraní a zaskladněním k dispozici v aplikaci, nejsou odebrány dynamicky.

<a name="Contextual_Menus" />

## <a name="contextual-menus"></a>Kontextové nabídky

Kontextové nabídky se zobrazí, když uživatel klikne pravým tlačítkem myši nebo ovládací prvek klikne na položku v okně. Ve výchozím nastavení některé prvky uživatelského rozhraní, které jsou součástí systému macOS již mají kontextové nabídky, které jsou připojené k nim (například zobrazení textu). Může však nastat situace, když chcete vytvořit vlastní vlastní kontextové nabídky pro element uživatelského rozhraní, který jsme přidali do okna.

Umožňuje upravit naše **Main.storyboard** souboru v Xcode a přidejte **okno** okna naše návrhu, nastavte jeho **– třída** na "NSPanel" v **Identity Inspector**, přidejte nový **pomocníka** položkou **okno** nabídce a jeho připojení k nové okno pomocí **zobrazit Segue**:

[![Nastavení typu segue](menu-images/context01.png "segue typ nastavení")](menu-images/context01-large.png#lightbox)

Pojďme postupujte takto:

1. Přetáhněte **popisek** z **knihovny Inspector** na **Panel** okno a nastavit jeho text na "Vlastnost": 

    [![Úpravy hodnotu popisku](menu-images/context03.png "úpravy hodnotu popisku")](menu-images/context03-large.png#lightbox)
2. Další přetáhněte **nabídky** z **knihovny Inspector** do řadiče zobrazení v zobrazení hierarchie a přejmenování tří položek nabídky výchozí **dokumentu**, **textu**  a **písma**:

    [![Položky nabídky požadované](menu-images/context02.png "položky požadované nabídky")](menu-images/context02-large.png#lightbox)
3. Ovládací prvek nyní přetažení z **vlastnost popisek** na **nabídky**:

    [![Přetažení k vytvoření segue](menu-images/context04.png "přetažení k vytvoření segue")](menu-images/context04-large.png#lightbox)
4. Z tohoto dialogového okna místní vyberte **nabídky**: 

    ![Nastavení typu segue](menu-images/context05.png "segue typ nastavení")
5. Z **Identity Inspector**, nastavte řadič zobrazení třídy "PanelViewController": 

    [![Nastavení třídy segue](menu-images/context10.png "nastavení segue – třída")](menu-images/context10-large.png#lightbox)
6. Přepněte zpět na Visual Studio pro Mac k synchronizaci a vraťte se do rozhraní tvůrce.
7. Přepnout **pomocníka Editor** a vyberte **PanelViewController.h** souboru.
8. Vytvoří akci pro **dokumentu** položky nabídky názvem `propertyDocument`: 

    [![Konfigurace akce](menu-images/context06.png "konfigurace akce")](menu-images/context06-large.png#lightbox)
9. Pro zbývající položky nabídky opakujte vytváření akce: 

    [![Akce požadované](menu-images/context07.png "požadované akce")](menu-images/context07-large.png#lightbox)
10. Nakonec vytvořte výstupu pro **vlastnost popisek** názvem `propertyLabel`: 

    [![Konfigurace výstupu](menu-images/context08.png "konfigurace výstupu")](menu-images/context08-large.png#lightbox)
11. Uložte změny a vrátit k sadě Visual Studio pro Mac k synchronizaci s Xcode.

Upravit **PanelViewController.cs** souboru a přidejte následující kód:

```csharp
partial void propertyDocument (Foundation.NSObject sender) {
    propertyLabel.StringValue = "Document";
}

partial void propertyFont (Foundation.NSObject sender) {
    propertyLabel.StringValue = "Font";
}

partial void propertyText (Foundation.NSObject sender) {
    propertyLabel.StringValue = "Text";
}
```

Nyní když jsme aplikaci spustit a klikněte pravým tlačítkem na název vlastnosti v panelu, uvidíme naše vlastní kontextové nabídky. Pokud jsme vyberte a položky v nabídce, jeho hodnota se změní:

![V kontextové nabídce systémem](menu-images/context09.png "v kontextové nabídce systémem")

Další Podíváme se na vytváření stav panelu nabídek.

## <a name="status-bar-menus"></a>Stav nabídky panelu nástrojů

Stav nabídky panelu nástrojů zobrazí kolekci stav položky nabídky, které zajišťují interakci s nebo zpětnou vazbu uživateli, jako jsou nabídky nebo bitovou kopii odrážející stav aplikace. V nabídce panelu Stav aplikace je povoleno a aktivní i v případě, že aplikace běží na pozadí. Systémové stavový řádek nachází na pravé straně řádku nabídek, aplikace a je aktuálně k dispozici v systému macOS pouze stavový řádek.

Umožňuje upravit naše **AppDelegate.cs** souboru a nastavit `DidFinishLaunching` metoda vypadá takto:

```csharp
public override void DidFinishLaunching (NSNotification notification)
{
    // Create a status bar menu
    NSStatusBar statusBar = NSStatusBar.SystemStatusBar;

    var item = statusBar.CreateStatusItem (NSStatusItemLength.Variable);
    item.Title = "Text";
    item.HighlightMode = true;
    item.Menu = new NSMenu ("Text");

    var address = new NSMenuItem ("Address");
    address.Activated += (sender, e) => {
        PhraseAddress(address);
    };
    item.Menu.AddItem (address);

    var date = new NSMenuItem ("Date");
    date.Activated += (sender, e) => {
        PhraseDate(date);
    };
    item.Menu.AddItem (date);

    var greeting = new NSMenuItem ("Greeting");
    greeting.Activated += (sender, e) => {
        PhraseGreeting(greeting);
    };
    item.Menu.AddItem (greeting);

    var signature = new NSMenuItem ("Signature");
    signature.Activated += (sender, e) => {
        PhraseSignature(signature);
    };
    item.Menu.AddItem (signature);
}
```

`NSStatusBar statusBar = NSStatusBar.SystemStatusBar;` umožňuje nám přístup do systémového stavový řádek. `var item = statusBar.CreateStatusItem (NSStatusItemLength.Variable);` Vytvoří novou položku panelu stavu. Zde jsme vytvoření nabídky a počet položek nabídky a v nabídce přiřadit položky stavového řádku, který jsme právě vytvořili. 

Pokud jsme aplikaci spustit, zobrazí se nová položka panelu stavu. Výběrem položky z nabídky se změní text v zobrazení textu: 

![V nabídce panelu Stav spuštění](menu-images/statusbar01.png "nabídku ve stavovém řádku systémem")

V dalším kroku Podíváme se na vytváření položek nabídky vlastní ukotvení.

## <a name="custom-dock-menus"></a>Vlastní ukotvení nabídky

V nabídce ukotvení se zobrazí pro vás aplikace systému Mac, když uživatel klikne pravým tlačítkem myši nebo ovládací prvek klikne na ikonu aplikace v ukotvení:

![Vlastní ukotvení nabídky](menu-images/dock01.png "vlastní ukotvení nabídky")

Umožňuje vytvořit vlastní ukotvení nabídku pro naši aplikaci následujícím způsobem:

1. V sadě Visual Studio pro Mac, klikněte pravým tlačítkem na projekt a vyberte aplikace **přidat** > **nový soubor...** Z tohoto dialogového okna nový soubor vyberte **Xamarin.Mac** > **prázdný definice rozhraní**, použijte "DockMenu" pro **název** a klikněte na tlačítko **nový**  tlačítko Nový vytvořte **DockMenu.xib** souboru:

    ![Přidání definice prázdného rozhraní](menu-images/dock02.png "přidání definice prázdného rozhraní")
2. V **řešení Pad**, dvakrát klikněte **DockMenu.xib** soubor otevřete pro úpravy v Xcode. Vytvořte novou **nabídky** s následující položky: **adresu**, **datum**, **s pozdravem**, a **podpisu** 

    [![Rozložení rozhraní](menu-images/dock03.png "rozložení uživatelského rozhraní")](menu-images/dock03-large.png#lightbox)
3. Dále umožňuje připojení k naší existující akce, které jsme vytvořili pro naše vlastní nabídky v naší nové položky nabídky [přidání, úpravy a odstraňování nabídky](#Adding,_Editing_and_Deleting_Menus) část výše. Přepnout **připojení Inspector** a vyberte **první respondér** v **rozhraní hierarchie**. Posuňte se dolů a najděte `phraseAddress:` akce. Přetáhněte řádek z kruhu na této akce **adresu** položky nabídky:

    [![Přetahování k přenosu až akce](menu-images/dock04.png "přetahování k přenosu až akce")](menu-images/dock04-large.png#lightbox)
4. Opakujte pro všechny ostatní položky nabídky jejich připojením k jejich odpovídající akce: 

    [![Akce požadované](menu-images/dock05.png "požadované akce")](menu-images/dock05-large.png#lightbox)
5. Potom vyberte **aplikace** v **rozhraní hierarchie**. V **připojení Inspector**, přetáhněte řádek z kruhu na `dockMenu` výstupu do nabídky jsme právě vytvořili:

    [![Přetahování přenosová až výstupu](menu-images/dock06.png "přetahování přenosová až výstupu")](menu-images/dock06-large.png#lightbox)
6. Uložte změny a přepněte zpět na Visual Studio pro Mac k synchronizaci s Xcode.
7. Dvakrát klikněte **Info.plist** soubor otevřete pro úpravy: 

    [![Úpravy souboru Info.plist](menu-images/dock07.png "úprav souboru Info.plist")](menu-images/dock07-large.png#lightbox)
8. Klikněte **zdroj** karta v dolní části obrazovky: 

    [![Výběr zobrazení zdroje](menu-images/dock08.png "výběr zobrazení zdroje")](menu-images/dock08-large.png#lightbox)
9. Klikněte na tlačítko **přidat novou položku**, klikněte na zelenou a tlačítko, nastavte název vlastnosti do "AppleDockMenu" a hodnota, která má "DockMenu" (název souboru naší nové .xib bez přípony): 

    [![Přidání položky DockMenu](menu-images/dock09.png "přidání položky DockMenu")](menu-images/dock09-large.png#lightbox)

Nyní když jsme spuštění aplikace a klikněte pravým tlačítkem na ikonu na ukotvení, bude zobrazena naší nové položky nabídky:

![Příkladem nabídce ukotvení systémem](menu-images/dock10.png "příklad nabídce ukotvení systémem")

Pokud jsme zvolili jednu z vlastních položek z nabídky, text v zobrazení naše text se změní.

<a name="Pop-up_Menus_and_Pull-Down_Lists" />

## <a name="pop-up-button-and-pull-down-lists"></a>Rozevírací seznamy a rozbalovací tlačítka

Automaticky otevírané okno tlačítko zobrazí vybrané položky a zobrazí seznam možností, které lze vybírat při kliknutí na uživatelem. Rozevírací seznam je typ rozbalovací tlačítka obvykle slouží k výběru příkazy, které jsou specifické pro kontext aktuální úlohy. Obě může vyskytovat kdekoli v okně.

Umožňuje vytvořit vlastní místní tlačítko pro naši aplikaci následujícím způsobem:

1. Upravit **Main.storyboard** souboru v Xcode a přetáhněte **tlačítko místní** z **knihovny Inspector** do **Panel** okno jsme vytvořili v [kontextové nabídky](#Contextual_Menus) části: 

    [![Přidání tlačítka místní](menu-images/popup01.png "přidání tlačítka automaticky otevřeném okně.")](menu-images/popup01-large.png#lightbox)
2. Přidat novou položku nabídky a nastavit názvy položek v automaticky otevřeném okně na: **adresu**, **datum**, **s pozdravem**, a **podpisu** 

    [![Konfigurace položky nabídky](menu-images/popup02.png "konfigurace položky nabídky")](menu-images/popup02-large.png#lightbox)
3. Dále umožňuje připojení k existující akce, které jsme vytvořili pro naše vlastní nabídky v naší nové položky nabídky [přidání, úpravy a odstraňování nabídky](#Adding,_Editing_and_Deleting_Menus) část výše. Přepnout **připojení Inspector** a vyberte **první respondér** v **rozhraní hierarchie**. Posuňte se dolů a najděte `phraseAddress:` akce. Přetáhněte řádek z kruhu na této akce **adresu** položky nabídky: 

    [![Přetahování k přenosu až akce](menu-images/popup03.png "přetahování k přenosu až akce")](menu-images/popup03-large.png#lightbox)
4. Opakujte pro všechny ostatní položky nabídky jejich připojením k jejich odpovídající akce: 

    [![Všechny požadované akce](menu-images/popup04.png "všechny požadované akce")](menu-images/popup04-large.png#lightbox)
5. Uložte změny a přepněte zpět na Visual Studio pro Mac k synchronizaci s Xcode.

Nyní když jsme spuštění aplikace a z místní nabídce vyberte položku, se změní textu v našem textového zobrazení:

![Příkladem místní spuštění](menu-images/popup05.png "příklad místní spuštění")

Můžete vytvořit a pracovat s rozevírací seznamy přesný stejným způsobem jako rozbalovací tlačítka. Místo připojení k existující akce, můžete vytvořit vlastní vlastní akce stejně, jako jsme to udělali pro naše v kontextové nabídce [kontextové nabídky](#Contextual_Menus) části.

## <a name="summary"></a>Souhrn

Tento článek podrobně prohlédnout práce s nabídkami a položek nabídky v aplikaci Xamarin.Mac trvá. Nejprve jsme se zaměřili na řádku nabídek aplikace, pak jsme se podívali na vytváření kontextové nabídky, v dalším jsme se zaměřili na panelu nabídek stav a vlastní ukotvení nabídky. Nakonec jsme zahrnutých rozevírací seznamy a místní nabídky.


## <a name="related-links"></a>Související odkazy

- [MacMenus (ukázka)](https://developer.xamarin.com/samples/mac/MacMenus/)
- [Hello, Mac](~/mac/get-started/hello-mac.md)
- [Lidské Interface Guidelines - nabídky](https://developer.apple.com/macos/human-interface-guidelines/menus/menu-anatomy/)
- [Úvod do nabídky a aplikace automaticky otevírané okno seznamy](https://developer.apple.com/library/content/documentation/Cocoa/Conceptual/MenuList/MenuList.html)
