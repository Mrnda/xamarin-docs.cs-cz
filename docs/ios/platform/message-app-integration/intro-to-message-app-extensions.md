---
title: "Základní informace o rozšíření aplikace zpráv"
description: "Tento článek ukazuje jak zahrnout příponu zpráva aplikace Xamarin.iOS řešení, které se integruje se službou aplikace zprávy a představuje nové funkce pro uživatele."
ms.topic: article
ms.prod: xamarin
ms.assetid: 0CFB494C-376C-449D-B714-9E82644F9DA3
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/16/2017
ms.openlocfilehash: 083920aba3c8dc83b157b591e194c43935dcc566
ms.sourcegitcommit: 73bd0c7e5f237f0a1be70a6c1384309bb26609d5
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 03/22/2018
---
# <a name="message-app-extension-basics"></a>Základní informace o rozšíření aplikace zpráv

_Tento článek ukazuje jak zahrnout příponu zpráva aplikace Xamarin.iOS řešení, které se integruje se službou aplikace zprávy a představuje nové funkce pro uživatele._

Nové rozšíření aplikace zpráv do systému iOS 10, se integruje s **zprávy** aplikaci a uvede nové funkce pro uživatele. Rozšíření můžete odeslat text, štítky, souborů médií a interaktivní zprávy.

## <a name="about-message-app-extensions"></a>O rozšíření aplikace zpráv

Jak jsme uvedli výše, rozšíření aplikace zpráva se integruje s **zprávy** aplikaci a uvede nové funkce pro uživatele. Rozšíření můžete odeslat text, štítky, souborů médií a interaktivní zprávy. K dispozici jsou dva typy rozšíření aplikace zpráva:

- **Balíčky štítku** – obsahuje kolekci štítky, které uživatel může přidávat do zprávy. Štítku balíčky lze vytvořit bez psaní jakéhokoli kódu.
- **iMessage aplikace** -může být vlastní uživatelské rozhraní v rámci aplikace zprávy pro výběr štítky, zadáním textu, včetně mediální soubory (s převody volitelné typu) a vytváření, úpravy a odesílání zpráv interakce.

Rozšíření zpráv aplikace poskytují tři hlavní typy obsahu:

- **Interaktivní zprávy** -jsou pro typ obsahu vlastní zprávu, který generuje aplikaci, když uživatel klepnutím na zprávu, aplikace bude spuštěna v popředí.
- **Štítky** -jsou obrázky generované aplikací, které můžou být součástí zprávy odeslané mezi uživateli.
- **Další obsah podporované** – aplikace můžete zadat obsah, jako jsou fotografie, videa, text nebo odkazy na další obsah typy, které mají vždy podporuje aplikace zprávy.

Nové do systému iOS 10, aplikace zprávu nyní obsahuje vlastní vyhrazené, integrované aplikace úložiště. Všechny aplikace, které zahrnují rozšíření zpráv aplikace se zobrazí a povýší v tomto úložišti. Panel nové zprávy aplikace se zobrazí všechny aplikace, které byly staženy z obchodu s aplikacemi zprávy zajistit rychlý přístup uživatelům.

Také nové v iOS 10, Apple přidala vložené uvedení aplikace, která umožňuje uživatelům snadno zjistit, aplikace. Například pokud jeden uživatel odesílá obsah do jiné aplikaci, která 2. uživatel nemá nainstalovaný (např. pásek příkladu), název aplikace pro odesílání je uvedený v části obsah v historii zpráv. Pokud uživatel klepnutím aplikace název, jsme otevřít úložiště zpráv aplikace a aplikaci vybrali v úložišti.

Rozšíření zpráv aplikace jsou podobné pro existující aplikace iOS, že vývojář se seznámit s vytvářením a budou mít přístup ke všem standardní rozhraní a funkcí standardní iOS aplikace. Příklad:

- Mají přístup k nákupy v aplikaci.
- Mají přístup k Apple platit.
- Mají přístup k hardwaru zařízení, jako jsou fotoaparát.

Rozšíření zpráv aplikace jsou podporovány pouze na iOS 10, ale je viditelný v zařízení watchOS a systému macOS obsah, který tato rozšíření odeslat. Nové _nedávno stránky_ přidá do watchOS 3, zobrazí poslední štítky, které byly odeslány z telefonu, včetně těch, které z rozšíření zpráv aplikace, a umožní uživatelům odesílat tyto štítky z hodinek.

## <a name="about-the-messages-framework"></a>O rozhraní zprávy

Nové na iOS 10 rozhraní zprávy poskytuje rozhraní mezi rozšíření zpráv aplikace a aplikace zprávy na zařízení s iOS uživatele. Když uživatel spustí aplikaci z uvnitř aplikace zprávy, toto rozhraní umožňuje aplikaci, která mají být zjišťované a poskytuje data a kontext potřebné k rozložení jeho uživatelském rozhraní.

Jakmile je aplikace spuštěná, uživatel pracuje se má vytvořit nový obsah sdílet přes zprávu. Aplikace pak používá rozhraní zprávy k přenosu nově vytvořený obsah do aplikace pro zpracování zprávy.

Rozhraní framework zprávy a rozšíření zpráv aplikace jsou postavená na existující iOS technologie přípony aplikace. Další informace o rozšíření aplikace, najdete v tématu společnosti Apple [Průvodce programováním rozšíření aplikace](https://developer.apple.com/library/prerelease/content/documentation/General/Conceptual/ExtensibilityPG/index.html#//apple_ref/doc/uid/TP40014214).

Na rozdíl od jiných rozšíření body, které Apple poskytl v celém systému nemusí vývojář zajistit aplikace hostitele pro rozšíření aplikace jejich zpráv vzhledem k tomu, že zpráva aplikace funguje jako kontejner. Vývojář však má možnost včetně přípony aplikace zpráva uvnitř iOS nový nebo existující aplikace a přesouvání společně s sady.

Pokud rozšíření zpráv aplikace je součástí sady aplikaci iOS, zobrazí se na domovskou obrazovku zařízení i v panel zpráv aplikace z aplikace zprávy ikona aplikace. Pokud není součástí sady prostředků aplikace, lze rozšíření aplikace zpráva zobrazí pouze v panel zpráv aplikace.

I v případě, že rozšíření zpráv aplikace není součástí sady aplikací hostitele, vývojář muset zadat ikony aplikace sady rozšíření aplikace zprávu, protože se jedná ikonu, která se zobrazí v dalších částech tohoto systému například zprávy přihrádka aplikace nebo nastavení , pro rozšíření.

## <a name="about-stickers"></a>O štítky

Apple navrhnout štítky jako nový způsob iMessage uživatelům komunikovat tím, že štítky odeslán vložené jako jakýkoli jiný obsah zprávy nebo se lze připojit k předchozí zpráva bublinách uvnitř konverzace.

Jaké jsou štítky?

- Jsou bitové kopie, které poskytuje rozšíření aplikace zprávu.
- Může se jednat animovaný nebo statických bitových kopií.
- Poskytuje nový způsob, jak sdílet obsah bitové kopie z uvnitř aplikace.

Existují dva způsoby, jak vytvořit štítky:

1. Rozšíření štítku zpráv Pack aplikace mohou být vytvořeny z uvnitř Xcode bez včetně kódu. Je vyžadováno je prostředků pro štítky a ikony aplikace.
2. Vytvořením standardní příponou aplikace zprávu, která poskytuje štítky z kódu prostřednictvím rozhraní zprávy.

### <a name="creating-sticker-packs"></a>Vytvoření sady štítku

Štítku balíčky jsou vytvořené ze šablony speciální uvnitř Xcode a jednoduše zadejte statickou sadu prostředků bitové kopie, které lze použít jako štítky. Jak jsme uvedli výše, nevyžadují žádný kód, vývojář nastavuje soubory obrázků jednoduše tažením do složky štítku Pack v katalogu Asset štítky.

Obrázek, který má být zahrnut v sadě štítku musí splňovat následující požadavky:

- Bitové kopie musí být ve formátu PNG, APNG, GIF nebo JPEG. Apple navrhuje pomocí pouze formáty PNG a APNG plynoucí z poskytování štítku prostředky.
- Animovaný štítky podporují pouze formáty APNG a GIF.
- Obrázky štítku by měl poskytovat průhledné pozadí, vzhledem k tomu, že se daly umístit nad bublinách zpráva v konverzaci uživatelem.
- Soubory jednotlivých bitové kopie musí být menší než 500kb.
- Bitové kopie nemůže být menší než 100 x 100 bodů nebo větší této 206 x 206 body.

> [!IMPORTANT]
> Obrázky štítku by měl být vždy uvedených v `@3x` řešení v rozsahu 300 x 300 k 618 x 618 pixelů. Systém automaticky vygeneruje `@2x` a `@1x` verze za běhu podle potřeby.

Apple navrhuje testování prostředky Image štítku proti různých jiné barevnou pozadí (například prázdné, černé, red, žlutý a více stejné barvy) a více fotografie, zajistit, že vypadat nejlépe ve všech situacích možné.

Štítku balíčky můžete zadat štítky v jednom ze tří dostupných velikostí.

- **Malé** – 100 x 100 bodů.
- **Střední** – 136 x 136 body. Toto je výchozí velikost.
- **Velké** – 206 x 206 body.

Použití Xcode Inspector atributy k nastavení velikosti pro celý balíček štítku a umožněte jenom bitové kopie prostředky, které odpovídají na požadovanou velikost pro nejlepší výsledky v prohlížeči štítku uvnitř aplikace zprávy.

Další informace najdete v tématu naše [zmrzlinová Tvůrce](https://developer.xamarin.com/samples/monotouch/ios10/IceCreamBuilder/) společnosti Apple a aplikace [zprávy referenční](https://developer.apple.com/reference/messages).

## <a name="creating-a-custom-sticker-experience"></a>Vytváření vlastních štítku prostředí

Pokud aplikace vyžaduje více flexibility než poskytuje sadu štítku nebo ovládací prvek, je obsahovat příponu aplikace zprávu a zadejte štítky prostřednictvím rozhraní zprávy prostředí štítku vlastní.

Jaké jsou výhody vytváří vlastní štítku prostředí?

1. Umožňuje aplikaci přizpůsobit zobrazení štítky uživatelům aplikace. Například k dispozici štítky ve formátu než rozložení mřížky standardní nebo na jiné barevnou pozadí.
2. Umožňuje dynamicky vytvořit z kódu místo nebudou zahrnuty jako statický obrázek prostředky štítky.
3. Umožňuje štítku image prostředky dynamicky stáhnout z webu pro vývojáře bez nutnosti vydání nové verze na obchod s aplikacemi.
4. Chcete-li vytvořit štítky na průběžně umožňuje pro přístup k fotoaparátu zařízení.
5. Umožňuje pro nákupy v aplikaci, tak uživatel můžete zakoupit další štítky z uvnitř aplikace.

K vytváření vlastních štítku prostředí, postupujte takto:

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

1. Start Visual Studio for Mac.
2. Otevřete řešení přidat příponu aplikace zprávy na. 
3. Vyberte **iOS** > **rozšíření** > **iMessage rozšíření** a klikněte na **Další** tlačítko: 

    [![](intro-to-message-app-extensions-images/message01.png "Vyberte iMessage rozšíření")](intro-to-message-app-extensions-images/message01.png#lightbox)
4. Zadejte **název rozšíření** a klikněte na **Další** tlačítko: 

    [![](intro-to-message-app-extensions-images/message02.png "Zadejte název rozšíření")](intro-to-message-app-extensions-images/message02.png#lightbox)
5. Klikněte **vytvořit** tlačítko k sestavení rozšíření: 

    [![](intro-to-message-app-extensions-images/message03.png "Klikněte na tlačítko Vytvořit")](intro-to-message-app-extensions-images/message03.png#lightbox)

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

1. Spuštění sady Visual Studio.
2. Otevřete řešení přidat příponu aplikace zprávy na. 
3. Vyberte **iOS** > **rozšíření** > **iMessage rozšíření** a klikněte na **Další** tlačítko: 

    [![](intro-to-message-app-extensions-images/message01w.png "Vyberte iMessage rozšíření")](intro-to-message-app-extensions-images/message01.png#lightbox)
4. Zadejte **název rozšíření** a klikněte na **OK** tlačítko

-----

Ve výchozím nastavení `MessagesViewController.cs` soubor bude přidán k řešení. Toto je hlavní vstupní bod do rozšíření a dědí z `MSMessageAppViewController` třídy.

Rozhraní framework zpráv poskytuje třídy pro prezentování dostupné štítky pro uživatele:

- `MSStickerBrowserViewController` – Ovládací prvky zobrazení, které zobrazí štítky v. Také vyhovuje `IMSStickerBrowserViewDataSource` rozhraní vrátí počet štítku a štítku pro daný prohlížeč index.
- `MSStickerBrowserView` -Toto je zobrazení, zobrazí se dostupné štítky v.
- `MSStickerSize` -Rozhodne velikosti jednotlivých buněk mřížky štítky, které jsou uvedené v okně prohlížeče.

### <a name="creating-a-custom-sticker-browser"></a>Vytváření vlastních štítku prohlížeče

Vývojář můžete dále přizpůsobit štítku prostředí pro uživatele tím, že poskytuje vlastní štítku prohlížeče (`MSMessageAppBrowserViewController`) v rozšíření aplikace zprávu. V prohlížeči štítku vlastní změny, jak mají štítky zobrazovat uživatelům při výběru štítku pro zahrnutí do datového proudu zpráv.

Postupujte takto:

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

1. V **řešení Pad**, klikněte pravým tlačítkem na název projektu rozšíření a vyberte **přidat** > **nový soubor...**   >  **iOS | Apple Watch** > **řadič rozhraní**.
2. Zadejte `StickerBrowserViewController` pro **název** a klikněte na **nový** tlačítko: 

    [![](intro-to-message-app-extensions-images/browser01.png "Zadejte pro název StickerBrowserViewController")](intro-to-message-app-extensions-images/browser01.png#lightbox)
3. Otevřete `StickerBrowserViewController.cs` soubor pro úpravy.

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

1. V **Průzkumníku řešení**, klikněte pravým tlačítkem na název projektu rozšíření a vyberte **přidat** > **nový soubor...**   >  **iOS | Apple Watch** > **řadič rozhraní**.
2. Zadejte `StickerBrowserViewController` pro **název** a klikněte na **nový** tlačítko: 

    [![](intro-to-message-app-extensions-images/browser01w.png "Zadejte pro název StickerBrowserViewController")](intro-to-message-app-extensions-images/browser01.png#lightbox)
3. Otevřete `StickerBrowserViewController.cs` soubor pro úpravy.

-----

Ujistěte se, `StickerBrowserViewController.cs` vypadat třeba takto:

```csharp
using System;
using System.Collections.Generic;
using UIKit;
using Messages;
using Foundation;

namespace MonkeyStickers
{
    public partial class StickerBrowserViewController : MSStickerBrowserViewController
    {
        #region Computed Properties
        public List<MSSticker> Stickers { get; set; } = new List<MSSticker> ();
        #endregion

        #region Constructors
        public StickerBrowserViewController (MSStickerSize stickerSize) : base (stickerSize)
        {
        }
        #endregion

        #region Private Methods
        private void CreateSticker (string assetName, string localizedDescription)
        {

            // Get path to asset
            var path = NSBundle.MainBundle.PathForResource (assetName, "png");
            if (path == null) {
                Console.WriteLine ("Couldn't create sticker {0}.", assetName);
                return;
            }

            // Build new sticker
            var stickerURL = new NSUrl (path);
            NSError error = null;
            var sticker = new MSSticker (stickerURL, localizedDescription, out error);
            if (error == null) {
                // Add to collection
                Stickers.Add (sticker);
            } else {
                // Report error
                Console.WriteLine ("Error, couldn't create sticker {0}: {1}", assetName, error);
            }
        }

        private void LoadStickers ()
        {

            // Load sticker assets from disk
            CreateSticker ("canada", "Canada Sticker");
            CreateSticker ("clouds", "Clouds Sticker");
            ...
            CreateSticker ("tree", "Tree Sticker");
        }
        #endregion

        #region Public Methods
        public void ChangeBackgroundColor (UIColor color)
        {
            StickerBrowserView.BackgroundColor = color;

        }
        #endregion

        #region Override Methods
        public override void ViewDidLoad ()
        {
            base.ViewDidLoad ();

            // Initialize
            LoadStickers ();
        }

        public override nint GetNumberOfStickers (MSStickerBrowserView stickerBrowserView)
        {
            return Stickers.Count;
        }

        public override MSSticker GetSticker (MSStickerBrowserView stickerBrowserView, nint index)
        {
            return Stickers[(int)index];
        }
        #endregion
    }
}
```

Prohlédněte si kód výše podrobně. Vytvoří úložiště pro štítky, které poskytuje rozšíření:

```csharp
public List<MSSticker> Stickers { get; set; } = new List<MSSticker> ();
```

A přepsání dvě metody `MSStickerBrowserViewController` třída poskytující data pro prohlížeče z tohle úložiště dat:

```csharp
public override nint GetNumberOfStickers (MSStickerBrowserView stickerBrowserView)
{
    return Stickers.Count;
}

public override MSSticker GetSticker (MSStickerBrowserView stickerBrowserView, nint index)
{
    return Stickers[(int)index];
}
```

`CreateSticker` Metoda získá cestu prostředek bitové kopie ze sady rozšíření a použije k vytvoření nové instance `MSSticker` z tento prostředek, který se přidá do kolekce:

```csharp
private void CreateSticker (string assetName, string localizedDescription)
{

    // Get path to asset
    var path = NSBundle.MainBundle.PathForResource (assetName, "png");
    if (path == null) {
        Console.WriteLine ("Couldn't create sticker {0}.", assetName);
        return;
    }

    // Build new sticker
    var stickerURL = new NSUrl (path);
    NSError error = null;
    var sticker = new MSSticker (stickerURL, localizedDescription, out error);
    if (error == null) {
        // Add to collection
        Stickers.Add (sticker);
    } else {
        // Report error
        Console.WriteLine ("Error, couldn't create sticker {0}: {1}", assetName, error);
    }
}
```

`LoadSticker` Metoda je volána z `ViewDidLoad` vytvořit pásek z asset pojmenovaný obrázek (součástí sady aplikace) a přidejte ho do kolekce štítky.

Chcete-li implementovat vlastní štítku prohlížeče, upravte `MessagesViewController.cs` souboru a nastavit jej vypadat třeba takto:

```csharp
using System;
using UIKit;
using Messages;

namespace MonkeyStickers
{
    public partial class MessagesViewController : MSMessagesAppViewController
    {
        #region Computed Properties
        public StickerBrowserViewController BrowserViewController { get; set;}
        #endregion

        #region Constructors
        protected ViewController (IntPtr handle) : base (handle)
        {
            // Note: this .ctor should not contain any initialization logic.
        }
        #endregion

        #region Override Methods
        public override void ViewDidLoad ()
        {
            base.ViewDidLoad ();


            // Create new browser and configure it
            BrowserViewController = new StickerBrowserViewController (MSStickerSize.Regular);
            BrowserViewController.View.Frame = View.Frame;
            BrowserViewController.ChangeBackgroundColor (UIColor.Gray);

            // Add to view
            AddChildViewController (BrowserViewController);
            BrowserViewController.DidMoveToParentViewController (this);
            View.AddSubview (BrowserViewController.View);
        }
        #endregion
    }
}
```

Podívejte se na tento kód trvá podrobně, vytvoří úložiště pro vlastní prohlížeče:

```csharp
public StickerBrowserViewController BrowserViewController { get; set;}
```

A v `ViewDidLoad` metoda, se vytvoří a konfiguruje novou prohlížeče:

```csharp
// Create new browser and configure it
BrowserViewController = new StickerBrowserViewController (MSStickerSize.Regular);
BrowserViewController.View.Frame = View.Frame;
BrowserViewController.ChangeBackgroundColor (UIColor.Gray);
```

Pak přidá do prohlížeče do zobrazení a zobrazit ji:

```csharp
// Add to view
AddChildViewController (BrowserViewController);
BrowserViewController.DidMoveToParentViewController (this);
View.AddSubview (BrowserViewController.View);
```

### <a name="further-sticker-customization"></a>Další přizpůsobení štítku

Další přizpůsobení štítku je možné zahrnutím právě dvě třídy v rozšíření aplikace zpráva:

- `MSStickerView`
- `MSSticker`

Pomocí výše uvedených metod, rozšíření může podporovat výběru štítku, který není závislý na metodě standardní štítku prohlížeče. Kromě toho je možné přepnout zobrazení štítku mezi dvěma režimy jiné zobrazení:

- **Compact** -Toto je výchozí režim, kde zobrazení štítku zabírají 25 % dolní zobrazení zpráv.
- **Rozšířit** -The štítku zobrazení vyplní celé celého zobrazení zpráv.

Toto zobrazení štítku je možné přepnout mezi těmito režimy buď programově, nebo ručně uživatelem.

Podívejte se na následující příklad zpracovávat přepínat mezi dva režimy jiné zobrazení. Dva různé řadiče zobrazení se bude vyžadovat pro každý stav. `StickerBrowserViewController` Obslužné rutiny **Compact** zobrazení a vypadá podobně jako následující:

```csharp
using System;
using System.Collections.Generic;
using UIKit;
using Messages;
using Foundation;

namespace MessageExtension
{
    public partial class StickerBrowserViewController : MSStickerBrowserViewController
    {
        #region Computed Properties
        public MessagesViewController MessagesAppViewController { get; set; }
        public List<MSSticker> Stickers { get; set; } = new List<MSSticker> ();
        #endregion

        #region Constructors
        public StickerBrowserViewController (MessagesViewController messagesAppViewController, MSStickerSize stickerSize) : base (stickerSize)
        {
            // Initialize
            this.MessagesAppViewController = messagesAppViewController;
        }
        #endregion

        #region Private Methods
        private void CreateSticker (string assetName, string localizedDescription)
        {

            // Get path to asset
            var path = NSBundle.MainBundle.PathForResource (assetName, "png");
            if (path == null) {
                Console.WriteLine ("Couldn't create sticker {0}.", assetName);
                return;
            }

            // Build new sticker
            var stickerURL = new NSUrl (path);
            NSError error = null;
            var sticker = new MSSticker (stickerURL, localizedDescription, out error);
            if (error == null) {
                // Add to collection
                Stickers.Add (sticker);
            } else {
                // Report error
                Console.WriteLine ("Error, couldn't create sticker {0}: {1}", assetName, error);
            }
        }

        private void LoadStickers ()
        {

            // Load sticker assets from disk
            CreateSticker ("add", "Add New Sticker");
            CreateSticker ("canada", "Canada Sticker");
            CreateSticker ("clouds", "Clouds Sticker");
            CreateSticker ("tree", "Tree Sticker");
        }
        #endregion

        #region Public Methods
        public void ChangeBackgroundColor (UIColor color)
        {
            StickerBrowserView.BackgroundColor = color;

        }
        #endregion

        #region Override Methods
        public override void ViewDidLoad ()
        {
            base.ViewDidLoad ();

            // Initialize
            LoadStickers ();

        }

        public override nint GetNumberOfStickers (MSStickerBrowserView stickerBrowserView)
        {
            return Stickers.Count;
        }

        public override MSSticker GetSticker (MSStickerBrowserView stickerBrowserView, nint index)
        {
            // Wanting to add a new sticker?
            if (index == 0) {
                // Yes, ask controller to present add sticker interface
                MessagesAppViewController.AddNewSticker ();
                return null;
            } else {
                // No, return existing sticker
                return Stickers [(int)index];
            }
        }
        #endregion
    }
}
```

`AddStickerViewController` Zpracuje **rozšířené** štítku zobrazení a vypadá takto:

```csharp
using System;
using Foundation;
using UIKit;
using Messages;

namespace MessageExtension
{
    public class AddStickerViewController : UIViewController
    {
        #region Computed Properties
        public MessagesViewController MessagesAppViewController { get; set;}
        public MSSticker NewSticker { get; set;}
        #endregion

        #region Constructors
        public AddStickerViewController (MessagesViewController messagesAppViewController)
        {
            // Initialize
            this.MessagesAppViewController = messagesAppViewController;
        }
        #endregion

        #region Override Method
        public override void ViewDidLoad ()
        {
            base.ViewDidLoad ();

            // Build interface to create new sticker
            var cancelButton = new UIButton (UIButtonType.RoundedRect);
            cancelButton.TouchDown += (sender, e) => {
                // Cancel add new sticker
                MessagesAppViewController.CancelAddNewSticker ();
            };
            View.AddSubview (cancelButton);

            var doneButton = new UIButton (UIButtonType.RoundedRect);
            doneButton.TouchDown += (sender, e) => {
                // Add new sticker to collection
                MessagesAppViewController.AddStickerToCollection (NewSticker);
            };
            View.AddSubview (doneButton);
            
            ...
        }
        #endregion
    }
}
```

`MessageViewController` Implementuje tyto řadiče zobrazení k řízení požadovaný stav:

```csharp
using System;
using UIKit;
using Messages;

namespace MessageExtension
{
    public partial class MessagesViewController : MSMessagesAppViewController
    {
        #region Computed Properties
        public bool IsAddingSticker { get; set;}
        public StickerBrowserViewController BrowserViewController { get; set; }
        public AddStickerViewController AddStickerController { get; set;}
        #endregion

        #region Constructors
        protected MessagesViewController (IntPtr handle) : base (handle)
        {
            // Note: this .ctor should not contain any initialization logic.
        }
        #endregion

        #region Public Methods
        public void PresentStickerBrowser ()
        {
            // Is the Add sticker view being displayed?
            if (IsAddingSticker) {
                // Yes, remove it from view
                AddStickerController.RemoveFromParentViewController ();
                AddStickerController.View.RemoveFromSuperview ();
            }

            // Add to view
            AddChildViewController (BrowserViewController);
            BrowserViewController.DidMoveToParentViewController (this);
            View.AddSubview (BrowserViewController.View);

            // Save mode
            IsAddingSticker = false;
        }

        public void PresentAddSticker ()
        {
            // Is the sticker browser being displayed?
            if (!IsAddingSticker) {
                // Yes, remove it from view
                BrowserViewController.RemoveFromParentViewController ();
                BrowserViewController.View.RemoveFromSuperview ();
            }

            // Add to view
            AddChildViewController (AddStickerController);
            AddStickerController.DidMoveToParentViewController (this);
            View.AddSubview (AddStickerController.View);

            // Save mode
            IsAddingSticker = true;
        }

        public void AddNewSticker ()
        {
            // Switch to expanded view mode
            Request (MSMessagesAppPresentationStyle.Expanded);
        }

        public void CancelAddNewSticker ()
        {
            // Switch to compact view mode
            Request (MSMessagesAppPresentationStyle.Compact);
        }

        public void AddStickerToCollection (MSSticker sticker)
        {
            // Add sticker to collection
            BrowserViewController.Stickers.Add (sticker);

            // Switch to compact view mode
            Request (MSMessagesAppPresentationStyle.Compact);
        }
        #endregion

        #region Override Methods
        public override void ViewDidLoad ()
        {
            base.ViewDidLoad ();

            // Create new browser and configure it
            BrowserViewController = new StickerBrowserViewController (this, MSStickerSize.Regular);
            BrowserViewController.View.Frame = View.Frame;
            BrowserViewController.ChangeBackgroundColor (UIColor.Gray);

            // Create new Add controller and configure it as well
            AddStickerController = new AddStickerViewController (this);
            AddStickerController.View.Frame = View.Frame;

            // Initially present the sticker browser
            PresentStickerBrowser ();
        }

        public override void DidTransition (MSMessagesAppPresentationStyle presentationStyle)
        {
            base.DidTransition (presentationStyle);

            // Take action based on style
            switch (presentationStyle) {
            case MSMessagesAppPresentationStyle.Compact:
                PresentStickerBrowser ();
                break;
            case MSMessagesAppPresentationStyle.Expanded:
                PresentAddSticker ();
                break;
            }
        }
        #endregion
    }
}
```

Když uživatel požádá k přidání nového štítku do jejich dostupné kolekce nový `AddStickerViewController` přišla viditelné kontrolerem a zobrazením štítku zadá **rozšířené** zobrazení:

```csharp
// Switch to expanded view mode
Request (MSMessagesAppPresentationStyle.Expanded);
```

Když uživatel vybere štítku přidat, je přidán do jejich dostupné kolekce a **Compact** se požaduje zobrazení:

```csharp
public void AddStickerToCollection (MSSticker sticker)
{
    // Add sticker to collection
    BrowserViewController.Stickers.Add (sticker);

    // Switch to compact view mode
    Request (MSMessagesAppPresentationStyle.Compact);
}
```

`DidTransition` Je metoda potlačena za účelem zpracování přepínání mezi dvěma způsoby:

```csharp
public override void DidTransition (MSMessagesAppPresentationStyle presentationStyle)
{
    base.DidTransition (presentationStyle);

    // Take action based on style
    switch (presentationStyle) {
    case MSMessagesAppPresentationStyle.Compact:
        PresentStickerBrowser ();
        break;
    case MSMessagesAppPresentationStyle.Expanded:
        PresentAddSticker ();
        break;
    }
}
``` 

## <a name="summary"></a>Souhrn

Má zahrnuté v tomto článku obsahovat příponu zpráva aplikace Xamarin.iOS řešení, které se integruje s **zprávy** aplikace a přítomen nových funkcí pro uživatele. O něm zmínka pomocí rozšíření odeslat text, štítky, souborů médií a interaktivní zprávy.



## <a name="related-links"></a>Související odkazy

- [Tvůrce zmrzlinová (ukázka)](https://developer.xamarin.com/samples/monotouch/ios10/IceCreamBuilder/)
- [Odkaz na zprávy](https://developer.apple.com/reference/messages)
- [Průvodce programováním v rozšíření aplikace](https://developer.apple.com/library/prerelease/content/documentation/General/Conceptual/ExtensibilityPG/index.html#//apple_ref/doc/uid/TP40014214)
