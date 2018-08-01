---
title: Standardní ovládací prvky v Xamarin.Mac
description: Tento článek popisuje práci s standardní ovládací prvky AppKit například tlačítka, popisky, textových polí, zaškrtněte políčka a segmentované ovládací prvky v aplikaci Xamarin.Mac. Popisuje jejich přidáním do rozhraní s Tvůrce rozhraní a interakci s nimi v kódu.
ms.prod: xamarin
ms.assetid: d2593883-d255-431f-9781-75f04d8cecea
ms.technology: xamarin-mac
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/14/2017
ms.openlocfilehash: e9d110d0d7a46d431ea6fca50caffe05903ddce2
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 06/05/2018
ms.locfileid: "34793042"
---
# <a name="standard-controls-in-xamarinmac"></a>Standardní ovládací prvky v Xamarin.Mac

_Tento článek popisuje práci s standardní ovládací prvky AppKit například tlačítka, popisky, textových polí, zaškrtněte políčka a segmentované ovládací prvky v aplikaci Xamarin.Mac. Popisuje jejich přidáním do rozhraní s Tvůrce rozhraní a interakci s nimi v kódu._

Při práci s C# a rozhraní .NET v aplikaci Xamarin.Mac, máte přístup ke stejné AppKit prvky, které vývojář práce *jazyka Objective-C* a *Xcode* nepodporuje. Protože Xamarin.Mac integruje přímo s Xcode, můžete na Xcode _rozhraní tvůrce_ vytvořit a udržovat vaše ovládací prvky Appkit (nebo je můžete také vytvořit přímo v kódu jazyka C#).

Prvky uživatelského rozhraní, které se používají k vytvoření uživatelského rozhraní aplikace Xamarin.Mac jsou AppKit prvky. Se skládá z elementů, jako jsou tlačítka, popisky, textových polí, zaškrtněte políčka a Segmentovaným ovládací prvky a způsobit okamžité akcí nebo nezobrazí výsledky, když uživatel manipuluje je.

[![](standard-controls-images/intro01.png "Příklad hlavní obrazovky aplikace")](standard-controls-images/intro01.png#lightbox)

V tomto článku vám nabídneme základní informace o práci s ovládacími prvky AppKit v aplikaci Xamarin.Mac. Vysoce navržený na spolupracovat [Hello, Mac](~/mac/get-started/hello-mac.md) článek nejprve, konkrétně [Úvod do Xcode a rozhraní tvůrce](~/mac/get-started/hello-mac.md#Introduction_to_Xcode_and_Interface_Builder) a [výstupy a akce](~/mac/get-started/hello-mac.md#Outlets_and_Actions) oddíly, jak se popisuje klíčové koncepty a techniky, které budeme používat v tomto článku.

Můžete chtít podívejte se na [třídy vystavení jazyka C# nebo metody jazyka Objective-C](~/mac/internals/how-it-works.md) části [Xamarin.Mac Internals](~/mac/internals/how-it-works.md) dokumentu taky vysvětluje `Register` a `Export` příkazy používá k navázání tříd jazyka C# jazyka Objective-C objekty a prvky uživatelského rozhraní.

<a name="Introduction_to_Controls_and_Views" />

## <a name="introduction-to-controls-and-views"></a>Úvod do zobrazení a ovládací prvky

(dříve označované jako Mac OS X) v systému macOS obsahuje standardní sadu ovládacích prvků uživatelského rozhraní pomocí rozhraní AppKit. Se skládá z elementů, jako jsou tlačítka, popisky, textových polí, zaškrtněte políčka a Segmentovaným ovládací prvky a způsobit okamžité akcí nebo nezobrazí výsledky, když uživatel manipuluje je.

Všechny prvky AppKit mít standardní, integrované vzhledu, která budou vhodná pro většinu používá, některé zadejte alternativní vzhled pro použití v oblasti rámce okna nebo v _Živost vliv_ kontext, například jako bočním panelu oblasti nebo v Pomůcka Center oznámení.

Při práci s ovládacími prvky AppKit, Apple navrhnout podle následujících pokynů:

- Vyhněte se kombinování velikosti ovládacího prvku ve stejném zobrazení.
- Obecně platí Vyhněte se změna velikosti ovládacích prvků ve svislém směru.
- Pomocí systému písma a velikost správné textu v rámci ovládacího prvku.
- Použijte správnou mezer mezi ovládacími prvky.

Další informace najdete v tématu stránku [o ovládací prvky a zobrazení](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/ControlsAll.html#//apple_ref/doc/uid/20000957-CH46-SW1) části společnosti Apple [OS X Human Interface Guidelines](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/).

<a name="Using_Controls_in_a_Window_Frame" />

### <a name="using-controls-in-a-window-frame"></a>Použití ovládacích prvků v rámec okna

Existují podmnožinu AppKit ovládací prvky, které zahrnují styl zobrazení, která umožňuje, aby se zahrnout do oblasti rámce okna. Příklad naleznete v části nástrojů poštovní aplikaci:

[![](standard-controls-images/mailapp.png "Rámec okna Mac")](standard-controls-images/mailapp.png#lightbox)

- **Zaokrouhlí texturou tlačítko** – `NSButton` s styl `NSTexturedRoundedBezelStyle`.
- **Texturou zaokrouhlené Segmentovaným řízení** – `NSSegmentedControl` s styl `NSSegmentStyleTexturedRounded`.
- **Texturou zaokrouhlené Segmentovaným řízení** – `NSSegmentedControl` s styl `NSSegmentStyleSeparated`.
- **Zaokrouhlí texturou místní nabídky** – `NSPopUpButton` s styl `NSTexturedRoundedBezelStyle`.
- **Zaokrouhlí texturou rozevírací nabídky** – `NSPopUpButton` s styl `NSTexturedRoundedBezelStyle`.
- **Panel hledání** – `NSSearchField`.

Při práci s AppKit ovládacích prvků v rámec okna, Apple navrhnout podle následujících pokynů:

- Nepoužívejte styly ovládacího prvku konkrétní rámec okna v těle okna.
- Nezadávejte ovládacích prvků okno textu nebo styly v rámce okna.

Další informace najdete v tématu stránku [o ovládací prvky a zobrazení](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/ControlsAll.html#//apple_ref/doc/uid/20000957-CH46-SW1) části společnosti Apple [OS X Human Interface Guidelines](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/).

<a name="Creating_a_User_Interface_in_Interface_Builder" />

## <a name="creating-a-user-interface-in-interface-builder"></a>Vytvoření uživatelského rozhraní v Tvůrci rozhraní

Když vytvoříte novou aplikaci Xamarin.Mac kakao, zobrazí okno Standardní prázdné, ve výchozím nastavení. Toto systému windows je definována v `.storyboard` automaticky zahrnutý v projektu. Chcete-li upravit návrh vašeho systému windows v **Průzkumníku řešení**, dvakrát klikněte `Main.storyboard` souboru:

[![](standard-controls-images/edit01.png "Výběr hlavní Storyboard v Průzkumníku řešení")](standard-controls-images/edit01.png#lightbox)

Otevře se okno návrhu v Xcode na rozhraní Tvůrce:

[![](standard-controls-images/edit02.png "Úpravy storyboard v Xcode")](standard-controls-images/edit02.png#lightbox)

Chcete-li vytvořit uživatelské rozhraní, budete přetáhněte prvky uživatelského rozhraní (AppKit ovládací prvky) z **knihovny Inspector** k **rozhraní editoru** v Tvůrci rozhraní. V příkladu níže **svislé rozděleným zobrazením** řízení byl nedovolenému z **knihovny Inspector** a umístit do okna v **rozhraní editoru**:

[![](standard-controls-images/edit03.png "Výběr zobrazení rozdělení z knihovny")](standard-controls-images/edit03.png#lightbox)

Další informace o vytváření uživatelské rozhraní v Tvůrci rozhraní, najdete v tématu naše [Úvod do Xcode a rozhraní tvůrce](~/mac/get-started/hello-mac.md#Introduction_to_Xcode_and_Interface_Builder) dokumentaci.

<a name="Sizing_and_Positioning" />

### <a name="sizing-and-positioning"></a>Změna velikosti a rozmístění

Jakmile ovládacího prvku byl zahrnut v uživatelském rozhraní, použijte **editor omezení** nastavit její umístění a velikost ručním zadáním hodnoty a řídit způsob řízení je automaticky nastavený a když velikost nadřazené okno nebo zobrazení Změní velikost:

[![](standard-controls-images/edit04.png "Nastavení omezení")](standard-controls-images/edit04.png#lightbox)

Použití **Red I světla** místa vně **Autoresizing** pole na _Flash disk_ ovládacího prvku (x, y) daného umístění. Příklad: 

[![](standard-controls-images/edit05.png "Úpravy omezení")](standard-controls-images/edit05.png#lightbox)

Určuje, že vybraný ovládací prvek (v **zobrazení hierarchie** & **rozhraní editoru**) se zablokuje a horního umístění okno nebo zobrazení po změně velikosti nebo přesunout. 

Další prvky editoru řídit vlastnosti, například výška a šířka:

[![](standard-controls-images/edit06.png "Nastavení výška")](standard-controls-images/edit06.png#lightbox)

Můžete také ovládat zarovnání elementů s omezeními použití **zarovnání Editor**:

[![](standard-controls-images/edit07.png "Editor zarovnání")](standard-controls-images/edit07.png#lightbox)

> [!IMPORTANT]
> Na rozdíl od iOS kde (0,0) je horní levém dolním rohu obrazovky, v systému macOS (0,0) je nižší levém dolním rohu. To je proto systému macOS používá matematické souřadnicový systém s číselné hodnoty zvýšení hodnoty směrem nahoru a doprava. Musíte to vzít v úvahu při vkládání ovládacích prvků AppKit na uživatelské rozhraní.

<a name="Setting_a_Custom_Class" />

### <a name="setting-a-custom-class"></a>Nastavení vlastní třídy

Existují situace, když bude práce s ovládacími prvky AppKit, které muset podtřídami a existujícího ovládacího prvku a můžete vytvořit vlastní vlastní verzí této třídy. Například definování vlastní verzi zdrojového seznamu:

```csharp
using System;
using AppKit;
using Foundation;

namespace AppKit
{
    [Register("SourceListView")]
    public class SourceListView : NSOutlineView
    {
        #region Computed Properties
        public SourceListDataSource Data {
            get {return (SourceListDataSource)this.DataSource; }
        }
        #endregion

        #region Constructors
        public SourceListView ()
        {

        }

        public SourceListView (IntPtr handle) : base(handle)
        {

        }

        public SourceListView (NSCoder coder) : base(coder)
        {

        }

        public SourceListView (NSObjectFlag t) : base(t)
        {

        }
        #endregion

        #region Override Methods
        public override void AwakeFromNib ()
        {
            base.AwakeFromNib ();

        }
        #endregion

        #region Public Methods
        public void Initialize() {

            // Initialize this instance
            this.DataSource = new SourceListDataSource (this);
            this.Delegate = new SourceListDelegate (this);

        }

        public void AddItem(SourceListItem item) {
            if (Data != null) {
                Data.Items.Add (item);
            }
        }
        #endregion

        #region Events
        public delegate void ItemSelectedDelegate(SourceListItem item);
        public event ItemSelectedDelegate ItemSelected;

        internal void RaiseItemSelected(SourceListItem item) {
            // Inform caller
            if (this.ItemSelected != null) {
                this.ItemSelected (item);
            }
        }
        #endregion
    }
}
```

Kde `[Register("SourceListView")]` zpřístupňuje instrukce `SourceListView` třídy jazyka Objective-C tak, aby se dá rozhraní tvůrce. Další informace najdete v tématu [třídy vystavení jazyka C# nebo metody jazyka Objective-C](~/mac/internals/how-it-works.md) části [Xamarin.Mac Internals](~/mac/internals/how-it-works.md) dokumentů, vysvětluje `Register` a `Export` příkazy používané k navázání tříd jazyka C# jazyka Objective-C objekty a prvky uživatelského rozhraní.

Výše uvedený kód na místě, můžete přetáhnout prvek AppKit základní typu, který bude rozšiřovat na návrhovou plochu (v příkladu níže, **zdrojového seznamu**), přepnout **Identity Inspector** a nastavte **vlastní třída** název, který je vystaven jazyka Objective-C (například `SourceListView`):

[![](standard-controls-images/edit10.png "Nastavení vlastní třídy v Xcode")](standard-controls-images/edit10.png#lightbox)

<a name="Exposing_Outlets_and_Actions" />

### <a name="exposing-outlets-and-actions"></a>Vystavení výstupy a akcí

Můžete získat přístup k ovládacím prvku AppKit v kódu jazyka C#, je nutné vystavit jako buď **výstupu** nebo a **akce**. Vybrat daný ovládací prvek buď **rozhraní hierarchie** nebo **rozhraní editoru** a přepněte do **pomocníka zobrazení** (zajistěte, abyste měli `.h`okna Vybrat pro úpravy):

[![](standard-controls-images/edit11.png "Výběr správný soubor pro úpravu")](standard-controls-images/edit11.png#lightbox)

Přetáhněte ovládací prvek z ovládacího prvku AppKit do dané `.h` souboru chcete začít vytvářet **výstupu** nebo **akce**:

[![](standard-controls-images/edit12.png "Přetahování pro vytvoření aplikace výstupu nebo akce")](standard-controls-images/edit12.png#lightbox)

Vyberte typ ohrožení vytvořte a přiřaďte **výstupu** nebo **akce** **název**: 

[![](standard-controls-images/edit13.png "Konfigurace výstupu nebo akce")](standard-controls-images/edit13.png#lightbox)


Další informace o práci s **výstupy** a **akce**, najdete v tématu [výstupy a akce](~/mac/get-started/hello-mac.md#Outlets_and_Actions) části našich [Úvod do Xcode a rozhraní Tvůrce](~/mac/get-started/hello-mac.md#Introduction_to_Xcode_and_Interface_Builder) dokumentaci.

<a name="Synchronizing_Changes_with_Xcode" />

### <a name="synchronizing-changes-with-xcode"></a>Synchronizace změn s Xcode

Když přepnete zpět do Visual Studio pro Mac od Xcode, všechny změny, které jste provedli v Xcode automaticky synchronizovat s Xamarin.Mac projektu.

Pokud jste vybrali `SplitViewController.designer.cs` v **Průzkumníku řešení** budete moci zobrazit jak vaše **výstupu** a **akce** byla drátové nahoru v našem kódu C#:

[![](standard-controls-images/sync01.png "Synchronizace změn s Xcode")](standard-controls-images/sync01.png#lightbox)

Všimněte si jak v definici `SplitViewController.designer.cs` souboru:

```csharp
[Outlet]
AppKit.NSSplitViewItem LeftController { get; set; }

[Outlet]
AppKit.NSSplitViewItem RightController { get; set; }

[Outlet]
AppKit.NSSplitView SplitView { get; set; }
```

Zarovnání pomocí definice v `MainWindow.h` souboru v Xcode:

```csharp
@interface SplitViewController : NSSplitViewController {
    NSSplitViewItem *_LeftController;
    NSSplitViewItem *_RightController;
    NSSplitView *_SplitView;
}

@property (nonatomic, retain) IBOutlet NSSplitViewItem *LeftController;

@property (nonatomic, retain) IBOutlet NSSplitViewItem *RightController;

@property (nonatomic, retain) IBOutlet NSSplitView *SplitView;
```

Jak vidíte, Visual Studio pro Mac čeká na změny `.h` souboru a poté tyto změny v příslušné automaticky synchronizuje `.designer.cs` soubor umístěte je do vaší aplikace. Může také zjistíte, že `SplitViewController.designer.cs` je konkrétní třídu, takže nemá Visual Studio pro Mac k úpravě `SplitViewController.cs ` který by přepsala veškeré změny, které jsme provedli pro třídu.

Obvykle se nikdy musíte otevřít `SplitViewController.designer.cs` sami, ho se netýkají pro vzdělávací účely.

> [!IMPORTANT]
> Ve většině případů se Visual Studio pro Mac automaticky najdete v části veškeré změny provedené v Xcode a synchronizovat je se svým projektem Xamarin.Mac. Ve vypnutém výskyt, který se synchronizace nedojde automaticky přepněte zpět na Xcode a je zpět do Visual Studio pro Mac znovu. To se obvykle ji synchronizační cyklus.

<a name="Working_with_Buttons" />

## <a name="working-with-buttons"></a>Práce s tlačítka

AppKit poskytuje několik typů tlačítka, které lze použít v návrhu uživatelské rozhraní. Další informace najdete v tématu [tlačítka](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/ControlsButtons.html#//apple_ref/doc/uid/20000957-CH48-SW1) části společnosti Apple [OS X Human Interface Guidelines](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/). 

[![](standard-controls-images/buttons01.png "Příkladem tlačítko různé typy")](standard-controls-images/buttons01.png#lightbox)

Pokud má byla tlačítko zveřejňovány prostřednictvím **výstupu**, následující kód bude odpovídat na ni se stisknutí:

```csharp
ButtonOutlet.Activated += (sender, e) => {
        FeedbackLabel.StringValue = "Button Outlet Pressed";
};
```

Pro tlačítka, který byl vystaven prostřednictvím **akce**, `public partial` metoda se automaticky vytvoří za vás s názvem, který jste zvolili v Xcode. Reagovat na **akce**, dokončení částečné metody ve třídě, která **akce** definované ve. Příklad:

```csharp
partial void ButtonAction (Foundation.NSObject sender) {
    // Do something in response to the Action
    FeedbackLabel.StringValue = "Button Action Pressed";
}
```

Pro tlačítka, které mají stav (jako je **na** a **vypnout**), stav můžete zkontrolovat nebo nastavit `State` vlastnost k dané `NSCellStateValue` výčtu. Příklad:

```csharp
DisclosureButton.Activated += (sender, e) => {
    LorumIpsum.Hidden = (DisclosureButton.State == NSCellStateValue.On);
};
```

Kde `NSCellStateValue` může být:

- **Na** – tlačítko se posune nebo požadovaný ovládací prvek (například změnami zaškrtávací políčko).
- **Vypnout** – tlačítko není nabídnutých nebo ovládací prvek není vybrán.
- **Smíšený** -směs **na** a **vypnout** stavy.

<a name="Mark-a-Button-as-Default-and-Set-Key-Equivalent" />

### <a name="mark-a-button-as-default-and-set-key-equivalent"></a>Označit jako výchozí a nastavte klíče ekvivalentní tlačítko

Pro žádné tlačítko, které jste přidali do návrh uživatelského rozhraní, můžete označit jako toto tlačítko _výchozí_ tlačítko, které bude aktivováno, když uživatel stiskne **vrátit nebo zadejte** klíče na klávesnici. V systému macOS obdrží toto tlačítko barvu pozadí blue ve výchozím nastavení.

Pokud chcete nastavit jako výchozí tlačítko, vyberte ho v Xcode na rozhraní tvůrce. Vedle **atribut Inspector**, vyberte **klíče ekvivalentní** pole a stiskněte klávesu **vrátit nebo zadejte** klíč:

[![](standard-controls-images/buttons03.png "Úpravy klíče ekvivalentní")](standard-controls-images/buttons03.png#lightbox)

Stejným způsobem můžete přiřadit všechny klíče pořadí, které lze použít k aktivaci tlačítko pomocí klávesnice místo myši. Pomocí klávesy například klíče příkaz C na předchozím obrázku.

Při spuštění aplikace a okna pomocí tlačítka je klíč a zaměřuje, pokud uživatel stiskne příkaz-C, bude aktivován pro tlačítko akce (jako – Pokud měl uživatel klikne na tlačítko).

<a name="Working_with_Checkboxes_and_Radio_Buttons" />

## <a name="working-with-checkboxes-and-radio-buttons"></a>Práce s přepínač tlačítka a zaškrtávací políčka

AppKit poskytuje několik typů zaškrtávací políčka a skupin přepínačů, mohou být používány váš návrh uživatelského rozhraní. Další informace najdete v tématu [tlačítka](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/ControlsButtons.html#//apple_ref/doc/uid/20000957-CH48-SW1) části společnosti Apple [OS X Human Interface Guidelines](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/). 

[![](standard-controls-images/buttons02.png "Příkladem typy k dispozici zaškrtávací políčko")](standard-controls-images/buttons02.png#lightbox)


Zaškrtávací políčka a přepínače (zveřejňovány prostřednictvím **výstupy**) mají stav (jako je **na** a **vypnout**), stav můžete zkontrolovat nebo nastavit `State` vlastnost k dané `NSCellStateValue` výčtu. Příklad:

```csharp
AdjustTime.Activated += (sender, e) => {
    FeedbackLabel.StringValue = string.Format("Adjust Time: {0}",AdjustTime.State == NSCellStateValue.On);
};
```

Kde `NSCellStateValue` může být:

- **Na** – tlačítko se posune nebo požadovaný ovládací prvek (například změnami zaškrtávací políčko).
- **Vypnout** – tlačítko není nabídnutých nebo ovládací prvek není vybrán.
- **Smíšený** -směs **na** a **vypnout** stavy.

Vyberte tlačítko ve skupině přepínačů, vystavit přepínačů k výběru jako **výstupu** a nastavit jeho `State` vlastnost. Příklad:

```csharp
partial void SelectCar (Foundation.NSObject sender) {
    TransportationCar.State = NSCellStateValue.On;
    FeedbackLabel.StringValue = "Car Selected";
}
```

Pokud chcete získat kolekci přepínačů fungují jako skupina a automaticky zpracovávat vybraném stavu, vytvořte novou **akce** a k němu připojí každé tlačítko ve skupině:

![](standard-controls-images/buttons04.png "Vytvoření nové akce")

V dalším kroku přiřadit jedinečný `Tag` pro každý přepínač v **atribut Inspector**:

![](standard-controls-images/buttons05.png "Úpravy značku tlačítko přepínačů")

Uložte změny a vrátit k sadě Visual Studio pro Mac, přidat kód pro zpracování **akce** všech přepínačů jsou připojené k:

```csharp
partial void NumberChanged(Foundation.NSObject sender)
{
    var check = sender as NSButton;
    Console.WriteLine("Changed to {0}", check.Tag);
}
```

Můžete použít `Tag` vlastnosti výběr byl přepínače.

<a name="Working_with_Menu_Controls" />

## <a name="working-with-menu-controls"></a>Práce s ovládacími prvky nabídky

AppKit poskytuje několik typů nabídky ovládacích prvků, které je možné při návrhu uživatelské rozhraní. Další informace najdete v tématu [nabídky ovládací prvky](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/ControlswithMenus.html#//apple_ref/doc/uid/20000957-CH100-SW1) části společnosti Apple [OS X Human Interface Guidelines](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/). 

[![](standard-controls-images/menu01.png "Příklad nabídky ovládací prvky")](standard-controls-images/menu01.png#lightbox)

<a name="Providing-Menu-Control-Data" />

### <a name="providing-menu-control-data"></a>Poskytuje Data ovládacího prvku nabídka

Ovládací prvky nabídky, která je k dispozici v systému macOS můžete nastavit k naplnění rozevíracího seznamu, buď z interní seznamu (která se dá předem definované v Tvůrci rozhraní nebo naplněno prostřednictvím kódu) nebo tím, že poskytuje zdroj dat vlastní, externí.

<a name="Working-with-Internal-Data" />

#### <a name="working-with-internal-data"></a>Práce s interní dat

Kromě definování položky v nabídce ovládací prvky rozhraní tvůrci (například `NSComboBox`), poskytují úplnou sadu metod, které vám umožní přidat, upravit nebo odstranit položky ze seznamu interní, která budou spravovat:

- `Add` -Přidá novou položku na konec seznamu.
- `GetItem` -Vrátí položku na daném indexu.
- `Insert` -Vloží novou položku v seznamu v daném umístění.
- `IndexOf` -Vrátí index zadané položky.
- `Remove` – Odebere danou položku ze seznamu.
- `RemoveAll` – Odebere všechny položky ze seznamu.
- `RemoveAt` – Odebere položku na daném indexu.
- `Count` -Vrátí počet položek v seznamu.

> [!IMPORTANT]
> Pokud používáte zdroj dat externí (`UsesDataSource = true`), volání metod výše vyvolá výjimku.

<a name="Working-with-an-External-Data-Source" />

#### <a name="working-with-an-external-data-source"></a>Práce s externího zdroje dat

Místo použití předdefinované interních datových zajistit řádky pro vaše Menu – ovládací prvek, můžete volitelně použít externí zdroj dat a zadejte vlastní úložiště zálohování pro položky (například databáze SQLite).

Pro práci s externího zdroje dat, vytvoříte instanci zdroje dat nabídky ovládacího prvku (`NSComboBoxDataSource` třeba) a přepsat několik metod, jak poskytnout potřebná data:

- `ItemCount` -Vrátí počet položek v seznamu.
- `ObjectValueForItem` -Vrací hodnotu položky pro dané index.
- `IndexOfItem` -Vrátí index pro hodnotu položky udělení.
- `CompletedString` -Vrátí první hodnotu odpovídající položky pro hodnotu částečně typu položky. Tato metoda je volána, pouze pokud bylo povoleno automatické dokončování (`Completes = true`).

Najdete v tématu [databáze a polích se seznamem](~/mac/app-fundamentals/databases.md#Databases-and-ComboBoxes) části [práce s databázemi](~/mac/app-fundamentals/databases.md) dokumentu další podrobnosti.

<a name="Adjusting-the-Lists-Appearance" />

### <a name="adjusting-the-lists-appearance"></a>Úprava vzhled v seznamu

Upravit vzhled ovládacího prvku nabídky k dispozici jsou tyto metody:

- `HasVerticalScroller` -Pokud `true`, ovládací prvek zobrazí svislý posuvník. 
- `VisibleItems` -Upravte počet položek zobrazí, když je otevřen ovládacího prvku. Výchozí hodnota je pět (5).
- `IntercellSpacing` -Upravte množství místa kolem daná položka tím, že poskytuje `NSSize` kde `Width` Určuje levý a pravý okraj a `Height` Určuje velikost místa před a po položku.
- `ItemHeight` -Určuje výšku každou položku v seznamu.

Pro typy rozevíracího seznamu `NSPopupButtons`, první položku poskytuje název pro ovládací prvek. Příklad: 

[![](standard-controls-images/menu02.png "Ovládací prvek příklad nabídky")](standard-controls-images/menu02.png#lightbox)

Chcete-li změnit název, vystavit tuto položku jako **výstupu** a použít kód takto:

```csharp
DropDownSelected.Title = "Item 1";
```

<a name="Manipulating-the-Selected-Items" />

### <a name="manipulating-the-selected-items"></a>Manipulace s vybrané položky

Následující metody a vlastnosti umožňují pracovat s vybrané položky v seznamu nabídky ovládacího prvku:

- `SelectItem` -Vybere položku na daném indexu.
- `Select` -Vyberte hodnotu daná položka.
- `DeselectItem` -Zruší výběr položku na daném indexu.
- `SelectedIndex` -Vrátí index aktuálně vybrané položky.
- `SelectedValue` -Vrátí hodnotu aktuálně vybrané položky.

Použití `ScrollItemAtIndexToTop` k dispozici položka v daném indexu v horní části seznamu a `ScrollItemAtIndexToVisible` o přechod do seznamu, dokud se nezobrazí položku na daném indexu.

<a name="Responding to Events" />

### <a name="responding-to-events"></a>Reagování na události

Ovládací prvky nabídky poskytují následující události reagovat na interakci s uživatelem:

- `SelectionChanged` -Je volána, když uživatel vybral hodnotu ze seznamu.
- `SelectionIsChanging` -Je volána, než se stane aktivní výběr novou položku vybraného uživatele.
- `WillPopup` -Je volána, než se zobrazí rozevírací seznam položek.
- `WillDismiss` -Je volána před zavřením rozevírací seznam položek.

Pro `NSComboBox` ovládací prvky, obsahují všechny stejné události, jako `NSTextField`, například `Changed` událost, která je volána, když uživatel upravuje hodnotu text v poli se seznamem.

Volitelně můžete reagovat interní položky nabídky dat definované v Tvůrci rozhraní vybrat připojením položka, která má **akce** a použít kód jako následující reagovat na **akce** se Aktivuje uživatele:

```csharp
partial void ItemOne (Foundation.NSObject sender) {
    DropDownSelected.Title = "Item 1";
    FeedbackLabel.StringValue = "Item One Selected";
}
```

Další informace o práci s nabídkami a nabídky ovládací prvky, najdete v tématu naše [nabídky](~/mac/user-interface/menu.md) a [rozbalovací tlačítka a zobrazí rozevírací seznam](~/mac/user-interface/menu.md) dokumentaci.

<a name="Working_with_Selection_Controls" />

## <a name="working-with-selection-controls"></a>Práce s ovládacími prvky pro výběr

AppKit poskytuje několik typů výběr ovládacích prvků, které je možné při návrhu uživatelské rozhraní. Další informace najdete v tématu [ovládacích prvků výběr](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/ControlsSelection.html#//apple_ref/doc/uid/20000957-CH49-SW1) části společnosti Apple [OS X Human Interface Guidelines](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/). 

[![](standard-controls-images/select01.png "Příklad výběru ovládací prvky")](standard-controls-images/select01.png#lightbox)

Existují dva způsoby, jak sledovat, kdy ovládacího prvku pro výběr má interakci s uživatelem, tak jej jako vystavení **akce**. Příklad:

```csharp
partial void SegmentButtonPressed (Foundation.NSObject sender) {
    FeedbackLabel.StringValue = string.Format("Button {0} Pressed",SegmentButtons.SelectedSegment);
}
```

Nebo připojením **delegáta** k `Activated` událostí. Příklad:

```csharp
TickedSlider.Activated += (sender, e) => {
    FeedbackLabel.StringValue = string.Format("Stepper Value: {0:###}",TickedSlider.IntValue);
};
```

Chcete-li nastavit nebo načíst hodnotu ovládacího prvku pro výběr, použijte `IntValue` vlastnost. Příklad:

```csharp
FeedbackLabel.StringValue = string.Format("Stepper Value: {0:###}",TickedSlider.IntValue);
```

Ovládací prvky speciální (například barva dobře a dobře Image) mají vlastnosti specifické pro jejich typy hodnot. Příklad:

```csharp
CollorWell.Color = NSColor.Red;
ImageWell.Image = NSImage.ImageNamed ("tag.png");

```

`NSDatePicker` Má následující vlastnosti pro práci přímo s datem a časem:

- **DateValue** -aktuální datum a čas hodnotu jako `NSDate`.
- **Místní** -umístění uživatele jako `NSLocal`.
- **TimeInterval** -jako hodnota času `Double`.
- **Časové pásmo** -uživatele časovém pásmu jako `NSTimeZone`.

<a name="Working_with_Indicator_Controls" />

## <a name="working-with-indicator-controls"></a>Práce s ovládacími prvky indikátoru

AppKit poskytuje několik typů ukazatele ovládacích prvků, které je možné při návrhu uživatelské rozhraní. Další informace najdete v tématu [ukazatele ovládacích prvků](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/ControlsIndicators.html#//apple_ref/doc/uid/20000957-CH50-SW1) části společnosti Apple [OS X Human Interface Guidelines](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/). 

[![](standard-controls-images/level01.png "Příklad ukazatele ovládacích prvků")](standard-controls-images/level01.png#lightbox)

Existují dva způsoby, jak sledovat, kdy má ovládací prvek indikátor interakci s uživatelem, buď ji jako vystavení **akce** nebo **výstupu** a připojení **delegáta** k `Activated`událostí. Příklad:

```csharp
LevelIndicator.Activated += (sender, e) => {
    FeedbackLabel.StringValue = string.Format("Level: {0:###}",LevelIndicator.DoubleValue);
};
```

Načíst nebo nastavit hodnotu ovládacího prvku indikátoru, použijte `DoubleValue` vlastnost. Příklad:

```csharp
FeedbackLabel.StringValue = string.Format("Rating: {0:###}",Rating.DoubleValue);
```

Při zobrazení by měl být animované Neurčitost a asynchronní indikátory průběhu. Použití `StartAnimation` metoda pro spuštění animace, když jsou zobrazeny. Příklad:

```csharp
Indeterminate.StartAnimation (this);
AsyncProgress.StartAnimation (this);
```

Volání `StopAnimation` metoda zastaví animace.

<a name="Working_with_Text_Controls" />

## <a name="working-with-text-controls"></a>Práce s ovládacích prvků textu

AppKit poskytuje několik typů ovládacích prvků Text, který lze použít v návrhu uživatelské rozhraní. Další informace najdete v tématu [ovládacích prvků textu](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/ControlsText.html#//apple_ref/doc/uid/20000957-CH51-SW1) části společnosti Apple [OS X Human Interface Guidelines](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/). 

[![](standard-controls-images/text01.png "Příklad ovládacích prvků textu")](standard-controls-images/text01.png#lightbox)

U textových polí (`NSTextField`), tyto události lze použít ke sledování interakce s uživatelem:

- **Změnit** -je aktivována, vždy, když uživatel změní hodnotu pole. Například na každý znak zadali.
- **EditingBegan** -je aktivována, když uživatel vybere pole pro úpravy.
- **EditingEnded** – Pokud uživatel stiskne klávesu Enter v poli nebo opustí pole.

Použití `StringValue` vlastnost pro čtení nebo nastavte hodnotu pole. Příklad:

```csharp
FeedbackLabel.StringValue = string.Format("User ID: {0}",UserField.StringValue);
```

Pro pole, které zobrazují nebo upravit číselné hodnoty, můžete použít `IntValue` vlastnost. Příklad:

```csharp
FeedbackLabel.StringValue = string.Format("Number: {0}",NumberField.IntValue);
```

`NSTextView` Poskytuje doporučenou fulltextové upravit a zobrazit oblast s integrovanou formátování. Jako `NSTextField`, použijte `StringValue` vlastnost pro čtení nebo nastavte hodnotu oblasti.

Příklad komplexní příklad práci s zobrazení textu v aplikaci Xamarin.Mac, najdete v tématu [SourceWriter ukázkovou aplikaci](https://developer.xamarin.com/samples/mac/SourceWriter/). SourceWriter je editor jednoduché zdrojového kódu, který poskytuje podporu pro doplňování kódu a zvýraznění syntaxe jednoduché.

Kód SourceWriter má plně komentář, pokud je k dispozici odkazy být zadané a z klíčové technologie nebo metody na příslušné informace v dokumentaci k Xamarin.Mac příručky.

<a name="Working_with_Content_Views" />

## <a name="working-with-content-views"></a>Práce se zobrazeními obsahu

AppKit poskytuje několik typů obsahu zobrazení, které je možné při návrhu uživatelské rozhraní. Další informace najdete v tématu [obsahu zobrazení](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/ControlsView.html#//apple_ref/doc/uid/20000957-CH52-SW1) části společnosti Apple [OS X Human Interface Guidelines](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/).

[![](standard-controls-images/content01.png "Zobrazení příkladu obsahu")](standard-controls-images/content01.png#lightbox)

<a name="Popovers" />

### <a name="popovers"></a>Popovers

Popover je prvku přechodný uživatelského rozhraní, která poskytuje funkce, které přímo souvisí s konkrétní a ovládací prvek nebo oblast na obrazovce. Popover float výše okno, které obsahuje ovládací prvek nebo oblasti, která souvisí s a jeho ohraničení zahrnuje šipka indikující bodem, ze kterého vyplývá.

Pokud chcete vytvořit popover, postupujte takto:

1. Otevřete `.storyboard` okna, které chcete přidat popover k poklepáním v souboru **Průzkumníku řešení**
2. Přetáhněte **zobrazit řadiče** z **knihovny Inspector** na **rozhraní editoru**: 

    [![](standard-controls-images/content02.png "Výběr řadič zobrazení z knihovny")](standard-controls-images/content02.png#lightbox)
4. Zadejte velikost a rozložení **vlastní zobrazení**: 

    [![](standard-controls-images/content04.png "Úpravy rozložení")](standard-controls-images/content04.png#lightbox)
5. Ovládací prvek klikněte a přetáhněte ze zdroje automaticky otevřeném okně na **View Controller**: 

    [![](standard-controls-images/content05.png "Vytvoření segue tažením")](standard-controls-images/content05.png#lightbox)
6. Vyberte **Popover** z místní nabídky: 

    [![](standard-controls-images/content06.png "Nastavení typu segue")](standard-controls-images/content06.png#lightbox)
7. Uložte změny a vrátit k sadě Visual Studio pro Mac k synchronizaci s Xcode.

<a name="Tab_Views" />

### <a name="tab-views"></a>Karta zobrazení

Karta zobrazení sestává ze seznamu karta (podobně jako do ovládacího prvku Segmentovaným který vypadá) v kombinaci s sadu zobrazení, které se nazývají _podokna_. Když uživatel vybere novou kartu, zobrazí se v podokně, který je připojen k němu. Podokno každého obsahuje vlastní sadu ovládacích prvků.

Při práci s kartě zobrazení v Xcode na rozhraní tvůrce, použijte **atribut Inspector** nastavit počet karet:

[![](standard-controls-images/content08.png "Úpravy počet karet")](standard-controls-images/content08.png#lightbox)

Vyberte každé kartě v **rozhraní hierarchie** k nastavení jeho **název** a přidejte prvky uživatelského rozhraní pro jeho **podokně**:

[![](standard-controls-images/content09.png "Úpravy na karty v Xcode")](standard-controls-images/content09.png#lightbox)

<a name="Data_Binding_AppKit_Controls" />

## <a name="data-binding-appkit-controls"></a>Ovládací prvky AppKit vazba dat

Pomocí technik klíč-hodnota kódování a datové vazby v aplikaci Xamarin.Mac může výrazně snížit množství kód, který máte k zápisu a udržovat naplnit a pracovat s prvky uživatelského rozhraní. Máte také výhodou další oddělení zálohování dat (_datový Model_) z vaší přední ukončení uživatelské rozhraní (_Model-View-Controller_), tečka na začátku k snadněji provádět údržbu, flexibilnější aplikace návrh.

Klíč-hodnota kódování (KVC) je mechanismus pro přístup k vlastnostem objektu nepřímo, použití klíče (speciálně formátované řetězce) k identifikaci vlastnosti místo k nim přistupovat pomocí instance proměnné nebo metody přístupových objektů (`get/set`). Implementací klíč-hodnota kódování kompatibilní přístupových objektů v aplikaci Xamarin.Mac získáte přístup k jiné funkce systému macOS například klíč-hodnota sledování (KVO), vazby dat, základní Data, kakao vazby a scriptability.

Další informace najdete v tématu [jednoduché datové vazby](~/mac/app-fundamentals/databinding.md#Simple_Data_Binding) části našich [datové vazby a klíč-hodnota kódování](~/mac/app-fundamentals/databinding.md) dokumentaci.


<a name="Summary" />

## <a name="summary"></a>Souhrn

Tento článek podrobně prohlédnout práce se standardní AppKit ovládacími prvky, jako je například tlačítek, popisky, textových polí, zaškrtněte políčka a Segmentovaným ovládacích prvků v aplikaci Xamarin.Mac trvá. O něm zmínka jejich přidáním do návrh uživatelského rozhraní v Tvůrci rozhraní na Xcode, je vystavení kódu pomocí akce a výstupy a práce s ovládacími prvky AppKit v kódu jazyka C#.

## <a name="related-links"></a>Související odkazy

- [MacControls (ukázka)](https://developer.xamarin.com/samples/mac/MacControls/)
- [Hello, Mac](~/mac/get-started/hello-mac.md)
- [Windows](~/mac/user-interface/window.md)
- [Vazba dat a kódování párů klíč-hodnota](~/mac/app-fundamentals/databinding.md)
- [Pokyny pro rozhraní lidské OS X](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/)
- [O ovládacích prvcích a zobrazení](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/ControlsAll.html#//apple_ref/doc/uid/20000957-CH46-SW1)
