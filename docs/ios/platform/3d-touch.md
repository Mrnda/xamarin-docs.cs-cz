---
title: "Úvod do 3D dotykového ovládání"
description: "Tento článek popisuje použití nové iPhone 6s a iPhone 6s Plus 3D Touch gesta ve vaší aplikaci."
ms.topic: article
ms.prod: xamarin
ms.assetid: 806D051E-3791-40F7-9776-4E4D3E56F7F3
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/19/2017
ms.openlocfilehash: 66cc67b38d70992fe815732407317fab3dc52528
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 02/27/2018
---
# <a name="introduction-to-3d-touch"></a>Úvod do 3D dotykového ovládání

_Tento článek popisuje použití nové iPhone 6s a iPhone 6s Plus 3D Touch gesta ve vaší aplikaci._

[ ![](3d-touch-images/info01.png "Příklady 3D Touch v aplikacích s podporou")](3d-touch-images/info01.png)

Tento článek zajistí a základní informace o použití nových 3D Touch rozhraní API pro přidání citlivé gesta přetížení pro vaše aplikace Xamarin.iOS, které jsou spuštěny na nové iPhone 6s a iPhone 6s Plus zařízení.

S 3D dotykového ovládání je nyní aplikaci pro iPhone schopen nejen říct, že uživatel je klepnou na displeji zařízení, ale to bude možné smysl kolik zatížení je vyvození uživatele a reagovat na úrovni různé zatížení.

3D Touch poskytuje následující funkce do vaší aplikace:

- [Zatížení velkých a malých písmen](#Pressure-Sensitivity) – aplikací lze měřit teď jak pevného nebo light uživatele je klepnou na obrazovce a proveďte výhod těchto informací.
  Například aplikace Malování provádět řádku silnější nebo tenčí podle jak pevného je uživatel klepnou na obrazovce.
- [Prohlížení a Pop](#Peek-and-Pop) -aplikace můžete teď umožňují uživatelům interakci s jeho data bez nutnosti přejít z jejich aktuálního kontextu. Stisknutím kombinace kláves pevný na obrazovce na obrazovce, můžete prohlížet položku, kterou mají zájem (např. zobrazení náhledu zprávy). Stisknutím kombinace kláves těžší, můžete pop do položky.
- [Rychlé akce](#Quick-Action) -myslíte o rychlé akce jako kontextové nabídky, které mohou být odebrány up když uživatel klikne pravým tlačítkem myši na položku v aplikace na ploše.
  Pomocí rychlé akce, můžete přidat zástupce pro funkce v aplikaci přímo z ikony aplikace na domovské obrazovce.
- [Testování 3D Touch v simulátoru](#Testing-3D-Touch-in-the-Simulator) -správného hardwaru Mac, můžete otestovat 3D aplikace Touch povolené v simulátoru iOS.

<a name="Pressure-Sensitivity" />

## <a name="pressure-sensitivity"></a>Citlivost

Jak je uvedeno výš, pomocí nové vlastnosti [UITouch](https://developer.xamarin.com/api/type/UIKit.UITouch/) třídy můžete měřit objem přetížení uživatele je použití na obrazovce zařízení s iOS a použijte tyto informace v uživatelském rozhraní. Například provádění tahu štětce více průhledná nebo neprůhledná na základě množství přetížení.

[ ![](3d-touch-images/pressure01.png "Tah štětce se vykresluje jako více průhledná nebo neprůhledná podle množství přetížení")](3d-touch-images/pressure01.png)

V důsledku 3D dotykového ovládání, pokud vaše aplikace běží v systému iOS 9 (nebo vyšší) a zařízení s iOS je schopen podpůrné 3D dotykového ovládání, změny v přetížení způsobí, že `TouchesMoved` událost, která má být vyvolána.

Například při monitorování `TouchesMoved` události [UIView](https://developer.xamarin.com/api/type/UIKit.UIView/), získat aktuální zatížení, který uživatel aplikuje na obrazovku můžete použít následující kód:

```csharp
public override void TouchesMoved (NSSet touches, UIEvent evt)
{
    base.TouchesMoved (touches, evt);
    UITouch touch = touches.AnyObject as UITouch;
    if (touch != null)
    {
        // Get the pressure
        var force = touch.Force;
        var maxForce = touch.MaximumPossibleForce;

        // Do something with the touch and the pressure
        ...
    }
}
```

`MaximumPossibleForce` Vrátí nejvyšší možná hodnota pro vlastnost `Force` vlastnost [UITouch](https://developer.xamarin.com/api/type/UIKit.UITouch/) na zařízení s iOS, která aplikace běží na základě.

> [!IMPORTANT]
> **Poznámka:** způsobí, že změny v tlak `TouchesMoved` událost, která má být vyvolána, i v případě X / Souřadnice Y nezměnily. Z důvodu této změny v chování, by měly být připraveny aplikace pro iOS `TouchesMoved` událost, která má být volána častěji a pro X / Y koordinuje být stejný jako poslední `TouchesMoved` volání.




Další informace najdete v tématu společnosti Apple [TouchCanvas: pomocí UITouch efektivně a efektivně](https://developer.apple.com/library/prerelease/ios/samplecode/TouchCanvas/) ukázkovou aplikaci a [referenci třídy UITouch](https://developer.apple.com/library/prerelease/ios/documentation/UIKit/Reference/UITouch_Class/).

<a name="Peek-and-Pop" />

## <a name="peek-and-pop"></a>Prohlížení a Pop

3D Touch poskytuje nové způsoby pro uživatele k interakci s informacemi v rámci vaší aplikace rychlejší než kdy dřív, aniž by bylo nutné přejít z jejich aktuálního umístění.

Například pokud vaše aplikace zobrazuje tabulku zpráv, může uživatel stisknout pevný na na položku, kterou chcete zobrazit náhled jeho obsah v zobrazení překrytí (který Apple odkazuje jako *prohlížet*).

[ ![](3d-touch-images/peekandpop01.png "Příklad prohlížení v obsahu")](3d-touch-images/peekandpop01.png)

Pokud uživatel stiskne těžší, zadá zobrazení regulární zprávy (což se označuje jako *Pop*-ping do zobrazení).

### <a name="checking-for-3d-touch-availability"></a>Kontrola dostupnosti 3D dotykového ovládání

Při práci se službou [UIViewController]() Pokud chcete zobrazit, pokud aplikace běží na zařízení s iOS podporuje 3D Touch můžete použít následující kód:

```csharp
public override void TraitCollectionDidChange(UITraitCollection previousTraitCollection)
{
    //Important: call the base function
    base.TraitCollectionDidChange(previousTraitCollection);

    //See if the new TraitCollection value includes force touch
    if (TraitCollection.ForceTouchCapability == UIForceTouchCapability.Available) {
        //Do something with 3D touch, for instance...
        RegisterForPreviewingWithDelegate (this, View);
        ...
```

Tuto metodu lze volat před *nebo po* `ViewDidLoad()`. 

### <a name="handling-peek-and-pop"></a>Funkce Náhled zpracování a Pop

Na zařízení s iOS, který může zpracovat 3D Touch, používáme instanci `UIViewControllerPreviewingDelegate` třídy pro zpracování zobrazení **prohlížet** a **Pop** položku Podrobnosti. Například pokud jsme měli řadič zobrazení tabulky názvem `MasterViewController ` jsme použít následující kód pro podporu **prohlížet** a **Pop**:

```csharp
using System;
using System.Collections.Generic;
using UIKit;
using Foundation;
using CoreGraphics;

namespace DTouch
{
    public class PreviewingDelegate : UIViewControllerPreviewingDelegate
    {
        #region Computed Properties
        public MasterViewController MasterController { get; set; }
        #endregion

        #region Constructors
        public PreviewingDelegate (MasterViewController masterController)
        {
            // Initialize
            this.MasterController = masterController;
        }

        public PreviewingDelegate (NSObjectFlag t) : base(t)
        {
        }

        public PreviewingDelegate (IntPtr handle) : base (handle)
        {
        }
        #endregion

        #region Override Methods
        /// Present the view controller for the "Pop" action.
        public override void CommitViewController (IUIViewControllerPreviewing previewingContext, UIViewController viewControllerToCommit)
        {
            // Reuse Peek view controller for details presentation
            MasterController.ShowViewController(viewControllerToCommit,this);
        }

        /// Create a previewing view controller to be shown at "Peek".
        public override UIViewController GetViewControllerForPreview (IUIViewControllerPreviewing previewingContext, CGPoint location)
        {
            // Grab the item to preview
            var indexPath = MasterController.TableView.IndexPathForRowAtPoint (location);
            var cell = MasterController.TableView.CellAt (indexPath);
            var item = MasterController.dataSource.Objects [indexPath.Row];

            // Grab a controller and set it to the default sizes
            var detailViewController = MasterController.Storyboard.InstantiateViewController ("DetailViewController") as DetailViewController;
            detailViewController.PreferredContentSize = new CGSize (0, 0);

            // Set the data for the display
            detailViewController.SetDetailItem (item);
            detailViewController.NavigationItem.LeftBarButtonItem = MasterController.SplitViewController.DisplayModeButtonItem;
            detailViewController.NavigationItem.LeftItemsSupplementBackButton = true;

            // Set the source rect to the cell frame, so everything else is blurred.
            previewingContext.SourceRect = cell.Frame;

            return detailViewController;
        }
        #endregion
    }
}
```

`GetViewControllerForPreview` Metoda se používá k provádění **prohlížet** operaci. Získá přístup k tabulce buňku a zálohování dat a potom se načte `DetailViewController` z aktuální scénáře. Nastavením `PreferredContentSize` k (0,0) vás žádáme pro výchozí **prohlížet** zobrazit velikost. Nakonec jsme všechno ale buňky jsme se zobrazení pomocí rozostření `previewingContext.SourceRect = cell.Frame` a vrátíme nové zobrazení pro zobrazení.

`CommitViewController` Opětovně používá jsme vytvořili v zobrazení **prohlížet** pro **Pop** zobrazit při stisknutí těžší.

### <a name="registering-for-peek-and-pop"></a>Registrace pro funkce Náhled a Pop

Z řadiče zobrazení, který chcete povolit uživatelům **prohlížet** a **Pop** položky z potřebujeme k registraci pro tuto službu. V příkladu výše uvedené tabulce řadiče zobrazení (`MasterViewController`), by používáme následující kód:

```csharp
public override void ViewDidLoad ()
{
    base.ViewDidLoad ();

    // Check to see if 3D Touch is available
    if (TraitCollection.ForceTouchCapability == UIForceTouchCapability.Available) {
        // Regiser for Peek and Pop
        RegisterForPreviewingWithDelegate(new PreviewingDelegate(this), View);
    }
    ...

}
```

Zde voláme `RegisterForPreviewingWithDelegate` metoda s instancí `PreviewingDelegate` jsme vytvořili výše. Na zařízeních s iOS, které podporují 3D Touch může uživatel stisknout pevný na položku, kterou chcete funkce Náhled na to. Pokud budou stiskněte ještě těžší, objeví se položka do ní standardní zobrazení zobrazení.

Další informace najdete v tématu naše [iOS 9 ukázka ApplicationShortcuts](https://developer.xamarin.com/samples/monotouch/iOS9/ViewControllerPreview/) a společnosti Apple [ViewControllerPreviews: pomocí UIViewController náhled rozhraní API](https://developer.apple.com/library/prerelease/ios/samplecode/ViewControllerPreviews/Introduction/Intro.html) ukázkovou aplikaci [ Referenční třída UIPreviewAction](https://developer.apple.com/library/prerelease/ios/documentation/UIKit/Reference/UIPreviewAction_Class/), [referenci třídy UIPreviewActionGroup](https://developer.apple.com/library/prerelease/ios/documentation/UIKit/Reference/UIPreviewActionGroup_Class/) a [referenci na protokol UIPreviewActionItem](https://developer.apple.com/library/prerelease/ios/documentation/UIKit/Reference/UIPreviewActionItem_Protocol/).

<a name="Quick-Actions" />

## <a name="quick-actions"></a>Rychlé akce

Pomocí 3D Touch a rychlé akce, můžete přidat běžné, rychlé a snadné na zástupce přístup k funkcím ve vaší aplikaci z ikonu domovskou obrazovku na zařízení s iOS.

Jak jsme uvedli výše, si můžete představit rychlé akce jako kontextové nabídky, které mohou být odebrány up když uživatel klikne pravým tlačítkem myši na položku v aplikace na ploše. Měli byste použít rychlé akce zajistit zástupce nejběžnější funkce nebo funkce vaší aplikace.

[ ![](3d-touch-images/quickactions01.png "Příkladem rychlé akce nabídky")](3d-touch-images/quickactions01.png)

<a name="Defining-Static-Quick-Actions" />

### <a name="defining-static-quick-actions"></a>Definování statické rychlé akce

Pokud jeden nebo více rychlé akce požadované aplikace jsou statické a není potřeba změnit, můžete je definovat v dané aplikaci `Info.plist` souboru. Tento soubor v externím editoru upravit a přidat následující klíče:

```xml
<key>UIApplicationShortcutItems</key>
<array>
    <dict>
        <key>UIApplicationShortcutItemIconType</key>
        <string>UIApplicationShortcutIconTypeSearch</string>
        <key>UIApplicationShortcutItemSubtitle</key>
        <string>Will search for an item</string>
        <key>UIApplicationShortcutItemTitle</key>
        <string>Search</string>
        <key>UIApplicationShortcutItemType</key>
        <string>com.company.appname.000</string>
    </dict>
    <dict>
        <key>UIApplicationShortcutItemIconType</key>
        <string>UIApplicationShortcutIconTypeShare</string>
        <key>UIApplicationShortcutItemSubtitle</key>
        <string>Will share an item</string>
        <key>UIApplicationShortcutItemTitle</key>
        <string>Share</string>
        <key>UIApplicationShortcutItemType</key>
        <string>com.company.appname.001</string>
    </dict>
</array>
```

Zde jsme se definování dvě statické položky rychlé akce pomocí následující klíče:

- `UIApplicationShortcutItemIconType` -Určuje ikona, která se zobrazí položka rychlé akce jako jeden z následujících hodnot:
  - `UIApplicationShortcutIconTypeAdd`
  - `UIApplicationShortcutIconTypeAlarm`
  - `UIApplicationShortcutIconTypeAudio`
  - `UIApplicationShortcutIconTypeBookmark`
  - `UIApplicationShortcutIconTypeCapturePhoto`
  - `UIApplicationShortcutIconTypeCaptureVideo`
  - `UIApplicationShortcutIconTypeCloud`
  - `UIApplicationShortcutIconTypeCompose`
  - `UIApplicationShortcutIconTypeConfirmation`
  - `UIApplicationShortcutIconTypeContact`
  - `UIApplicationShortcutIconTypeDate`
  - `UIApplicationShortcutIconTypeFavorite`
  - `UIApplicationShortcutIconTypeHome`
  - `UIApplicationShortcutIconTypeInvitation`
  - `UIApplicationShortcutIconTypeLocation`
  - `UIApplicationShortcutIconTypeLove`
  - `UIApplicationShortcutIconTypeMail`
  - `UIApplicationShortcutIconTypeMarkLocation`
  - `UIApplicationShortcutIconTypeMessage`
  - `UIApplicationShortcutIconTypePause`
  - `UIApplicationShortcutIconTypePlay`
  - `UIApplicationShortcutIconTypeProhibit`
  - `UIApplicationShortcutIconTypeSearch`
  - `UIApplicationShortcutIconTypeShare`
  - `UIApplicationShortcutIconTypeShuffle`
  - `UIApplicationShortcutIconTypeTask`
  - `UIApplicationShortcutIconTypeTaskCompleted`
  - `UIApplicationShortcutIconTypeTime`
  - `UIApplicationShortcutIconTypeUpdate`

        ![](3d-touch-images/uiapplicationshortcuticontype.png "UIApplicationShortcutIconType imagery")

* `UIApplicationShortcutItemSubtitle` -Definuje subtitle pro položku.
* `UIApplicationShortcutItemTitle` -Definuje název položky.
* `UIApplicationShortcutItemType` -Je řetězcová hodnota, kterou budeme používat k identifikaci položky v naší aplikaci. Další informace naleznete v následující části.

> [!IMPORTANT]
> **Poznámka:** položky místní rychlé akce, které jsou nastavené `Info.plist` nelze otevřít soubor s `Application.ShortcutItems` vlastnost. Jejich pouze předaná do `HandleShortcutItem` obslužné rutiny události. 




<a name="Identifying-Quick-Action-Items" />

### <a name="identifying-quick-action-items"></a>Identifikace rychlé akčních položek

Jak už jste viděli výše, když jste definovali rychlé akce ve vaší aplikaci `Info.plist`, jste přiřadili řetězcové hodnoty `UIApplicationShortcutItemType` klíč identifikovat.

Chcete-li tyto identifikátory snazší s ním pracovat v kódu, přidejte třídu s názvem `ShortcutIdentifier` do vaší aplikace projektu a nastavit jej vypadat třeba takto:

```csharp
using System;

namespace AppSearch
{
    public static class ShortcutIdentifier
    {
        public const string First = "com.company.appname.000";
        public const string Second = "com.company.appname.001";
        public const string Third = "com.company.appname.002";
        public const string Fourth = "com.company.appname.003";
    }
}
```

<a name="Handling-a-Quick-Action" />

### <a name="handling-a-quick-action"></a>Zpracování rychlé akce

Potom budete muset upravit vaší aplikace `AppDelegate.cs` soubor pro zpracování výběru položky rychlé akce z vaší aplikace ikonu na domovské obrazovce uživatelem.

Udělejte následující úpravy:

```csharp
using System;
...

public UIApplicationShortcutItem LaunchedShortcutItem { get; set; }

public bool HandleShortcutItem(UIApplicationShortcutItem shortcutItem) {
    var handled = false;

    // Anything to process?
    if (shortcutItem == null) return false;

    // Take action based on the shortcut type
    switch (shortcutItem.Type) {
    case ShortcutIdentifier.First:
        Console.WriteLine ("First shortcut selected");
        handled = true;
        break;
    case ShortcutIdentifier.Second:
        Console.WriteLine ("Second shortcut selected");
        handled = true;
        break;
    case ShortcutIdentifier.Third:
        Console.WriteLine ("Third shortcut selected");
        handled = true;
        break;
    case ShortcutIdentifier.Fourth:
        Console.WriteLine ("Forth shortcut selected");
        handled = true;
        break;
    }

    // Return results
    return handled;
}

public override bool FinishedLaunching (UIApplication application, NSDictionary launchOptions)
{
    var shouldPerformAdditionalDelegateHandling = true;

    // Get possible shortcut item
    if (launchOptions != null) {
        LaunchedShortcutItem = launchOptions [UIApplication.LaunchOptionsShortcutItemKey] as UIApplicationShortcutItem;
        shouldPerformAdditionalDelegateHandling = (LaunchedShortcutItem == null);
    }

    return shouldPerformAdditionalDelegateHandling;
}

public override void OnActivated (UIApplication application)
{
    // Handle any shortcut item being selected
    HandleShortcutItem(LaunchedShortcutItem);

    // Clear shortcut after it's been handled
    LaunchedShortcutItem = null;
}

public override void PerformActionForShortcutItem (UIApplication application, UIApplicationShortcutItem shortcutItem, UIOperationHandler completionHandler)
{
    // Perform action
    completionHandler(HandleShortcutItem(shortcutItem));
}
```

Nejprve definujeme veřejné `LaunchedShortcutItem` vlastnost sledovat poslední vybrané položky rychlé akce uživatelem. Potom jsme přepsat `FinishedLaunching` metoda a zjistit, pokud `launchOptions` byly předány a v případě, že obsahuje položku rychlé akce. Pokud tomu tak je, uložíme rychlé akce v `LaunchedShortcutItem` vlastnost.

V dalším kroku jsme přepsat `OnActivated` metoda a předejte jí všechny vybrané položky Snadné spuštění `HandleShortcutItem` metodu zachovalo podle požadavků. Právě jsme jsou pouze zápis zprávy, která se **konzoly**. V reálné aplikaci by zpracování, která akce nebyla nutná. Poté, co byla akce `LaunchedShortcutItem` není zaškrtnuta vlastnost.

Nakonec, pokud vaše aplikace je již spuštěna, `PerformActionForShortcutItem` metoda by měla volat za účelem zpracovat položky rychlé akce, takže je potřeba přepsat a volat naše `HandleShortcutItem` metoda také zde.

<a name="Creating-Dynamic-Quick-Action-Items" />

### <a name="creating-dynamic-quick-action-items"></a>Vytváření položek dynamické rychlé akce

Kromě definování statické rychlé akce položky ve vaší aplikaci `Info.plist` souboru, můžete vytvořit dynamické na průběžně rychlé akce. Chcete-li definovat dvě nové dynamické rychlé akce, upravte vaše `AppDelegate.cs` soubor znovu a upravte `FinishedLaunching` metodu vypadat podobně jako následující:

```csharp
public override bool FinishedLaunching (UIApplication application, NSDictionary launchOptions)
{
    var shouldPerformAdditionalDelegateHandling = true;

    // Get possible shortcut item
    if (launchOptions != null) {
        LaunchedShortcutItem = launchOptions [UIApplication.LaunchOptionsShortcutItemKey] as UIApplicationShortcutItem;
        shouldPerformAdditionalDelegateHandling = (LaunchedShortcutItem == null);
    }

    // Add dynamic shortcut items
    if (application.ShortcutItems.Length == 0) {
        var shortcut3 = new UIMutableApplicationShortcutItem (ShortcutIdentifier.Third, "Play") {
            LocalizedSubtitle = "Will play an item",
            Icon = UIApplicationShortcutIcon.FromType(UIApplicationShortcutIconType.Play)
        };

        var shortcut4 = new UIMutableApplicationShortcutItem (ShortcutIdentifier.Fourth, "Pause") {
            LocalizedSubtitle = "Will pause an item",
            Icon = UIApplicationShortcutIcon.FromType(UIApplicationShortcutIconType.Pause)
        };

        // Update the application providing the initial 'dynamic' shortcut items.
        application.ShortcutItems = new UIApplicationShortcutItem[]{shortcut3, shortcut4};
    }

    return shouldPerformAdditionalDelegateHandling;
}
```

Nyní probíhá kontrola a zjistěte, zda `application` již obsahuje sadu dynamicky vytvořit `ShortcutItems`, pokud tomu tak není vytvoříme dvě nové `UIMutableApplicationShortcutItem` objekty, které chcete definovat nové položky a přidat je do `ShortcutItems` pole.

Kód jsme již přidán do [zpracování rychlé akce](#Handling-a-Quick-Action) výše uvedené části, bude zpracovávat tyto dynamické rychlé akce stejně jako ty, které statické.

Je potřeba poznamenat, že můžete vytvořit kombinaci statických a dynamických rychlé akčních položek (jak jsme tady dělají), můžete nejsou omezeny na jeden z nich.

Další informace najdete naše [iOS 9 ukázka ViewControllerPreview](https://developer.xamarin.com/samples/monotouch/iOS9/ViewControllerPreview/) a najdete v článku společnosti Apple [ApplicationShortcuts: pomocí UIApplicationShortcutItem](https://developer.apple.com/library/prerelease/ios/samplecode/ApplicationShortcuts/) ukázkovou aplikaci [ Referenční třída UIApplicationShortcutItem](https://developer.apple.com/library/prerelease/ios/documentation/UIKit/Reference/UIApplicationShortcutItem_class/), [referenci třídy UIMutableApplicationShortcutItem](https://developer.apple.com/library/prerelease/ios/documentation/UIKit/Reference/UIMutableApplicationShortcutItem_class/) a [UIApplicationShortcutIcon odkazu](https://developer.apple.com/library/prerelease/ios/documentation/UIKit/Reference/UIApplicationShortcutIcon_Class/).

<a name="Testing-3D-Touch-in-the-Simulator" />

## <a name="testing-3d-touch-in-the-simulator"></a>Testování v simulátoru 3D dotykového ovládání

Když trackpadu pomocí nejnovější verzi Xcode a simulátoru iOS na kompatibilní Mac s Force Touch povolíte, můžete otestovat 3D funkce dotykového ovládání v simulátoru.

Chcete-li tuto funkci povolit, spuštění libovolné aplikace v simulované zařízení iPhone hardwaru, který podporuje 3D Touch (iPhone 6s a vyšší). Potom vyberte **hardwaru** nabídky v iOS simulátoru a povolit **trackpadu vynucené použití pro 3D touch** položky nabídky:

[ ![](3d-touch-images/simulator01.png "Vyberte nabídku hardwaru v simulátoru iOS a povolit použití Force trackpadu pro položku nabídky 3D dotykového ovládání")](3d-touch-images/simulator01.png)

Tato funkce aktivní můžete silněji na trackpadu Mac povolit 3D dotykového ovládání, stejně jako na skutečné iPhone hardwaru.

## <a name="summary"></a>Souhrn

Tento článek obsahuje zavedla nových 3D Touch rozhraní API k dispozici v systému iOS 9 pro iPhone 6s a iPhone 6s Plus. O něm zmínka přidání citlivost do aplikace; pomocí funkce Náhled a Pop rychle zobrazit informace v aplikaci z aktuálního kontextu bez navigační; a pomocí rychlé akce zajistit zástupce aplikace je nejčastěji používané funkce.



## <a name="related-links"></a>Související odkazy

- [iOS 9 ViewControllerPreview vzorku](https://developer.xamarin.com/samples/monotouch/ios9/ViewControllerPreview/)
- [iOS 9 ApplicationShortcuts vzorku](https://developer.xamarin.com/samples/monotouch/ios9/ApplicationShortcuts/)
- [iOS 9 pro vývojáře](https://developer.apple.com/ios/pre-release/)
- [iOS 9.0](https://developer.apple.com/library/prerelease/ios/releasenotes/General/WhatsNewIniOS/Articles/iOS9.html)
- [Příprava aplikace iPhone pro 3D dotykového ovládání](https://developer.apple.com/ios/3d-touch/)
