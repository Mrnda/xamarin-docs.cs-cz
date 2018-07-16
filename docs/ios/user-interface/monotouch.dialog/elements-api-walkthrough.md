---
title: Vytvoření aplikace Xamarin.iOS pomocí rozhraní Elements API
description: Tento článek je založen na informace uvedené v úvodu do dialogového okna MonoTouch článku. Představuje návod, který ukazuje způsob použití MonoTouch.Dialog (MT.) D) prvky rozhraní API mohli rychle začít vytvářet aplikace s pro MT. D.
ms.prod: xamarin
ms.assetid: F1124734-DF44-F1F3-0832-46F52A788CDC
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.openlocfilehash: dcd6f1260be3414c515010c2fd615910c7b5c054
ms.sourcegitcommit: cb80df345795989528e9df78eea8a5b45d45f308
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/14/2018
ms.locfileid: "39038339"
---
# <a name="creating-a-xamarinios-application-using-the-elements-api"></a>Vytvoření aplikace Xamarin.iOS pomocí rozhraní Elements API

_Tento článek je založen na informace uvedené v úvodu do dialogového okna MonoTouch článku. Představuje návod, který ukazuje způsob použití MonoTouch.Dialog (MT.) D) prvky rozhraní API mohli rychle začít vytvářet aplikace s pro MT. D._

V tomto návodu použijeme pro MT. D Elements API k vytvoření stylu hlavní podrobnosti o aplikaci, která zobrazí seznam úkolů. Když uživatel vybere <span class="ui"> + </span> tlačítko na navigačním panelu bude přidán nový řádek do tabulky pro úlohu. Výběr řádku přejde na obrazovku s podrobnostmi, která umožňuje aktualizovat popis úlohy a jejichž datum splatnosti, jak je znázorněno níže:

 [![](elements-api-walkthrough-images/01-task-list-app.png "Výběr řádku přejde na obrazovku s podrobnostmi, která umožňuje aktualizovat popis úlohy a jejichž datum splatnosti")](elements-api-walkthrough-images/01-task-list-app.png#lightbox)

 ## <a name="setting-up-mtd"></a>Nastavení pro MT. D

PRO MT. D je distribuován spolu s Xamarin.iOS. Ho Pokud chcete použít, klikněte pravým tlačítkem na **odkazy** uzel Xamarin.iOS projektu v sadě Visual Studio 2017 nebo Visual Studio pro Mac a přidejte odkaz na **MonoTouch.Dialog 1** sestavení. Pak přidejte `using MonoTouch.Dialog` příkazy ve zdrojovém kódu podle potřeby.

## <a name="elements-api-walkthrough"></a>Návod prvky rozhraní API

V [Úvod do dialogového okna MonoTouch](~/ios/user-interface/monotouch.dialog/index.md) článku jsme získali důkladného porozumění různé části pro MT. D. S použitím rozhraní Elements API umístit společně do aplikace.

## <a name="setting-up-the-multi-screen-application"></a>Nastavení aplikace s více obrazovkami

Pokud chcete spustit proces vytvoření obrazovky, vytvoří MonoTouch.Dialog `DialogViewController`a pak přidá `RootElement`.

Chcete-li vytvořit aplikaci s více obrazovkami s MonoTouch.Dialog, potřebujeme:

1.  Vytvoření `UINavigationController.`
1.  Vytvoření `DialogViewController.`
1.  Přidat `DialogViewController` jako kořen  `UINavigationController.` 
1.  Přidat `RootElement` k  `DialogViewController.`
1.  Přidat `Sections` a `Elements` k  `RootElement.` 

### <a name="using-a-uinavigationcontroller"></a>Použití UINavigationController

Vytvoření aplikace stylu navigace, potřebujeme vytvořit `UINavigationController`a přidejte ho jako `RootViewController` v `FinishedLaunching` metodu `AppDelegate`. Chcete-li `UINavigationController` pracovat MonoTouch.Dialog, přidáme `DialogViewController` k `UINavigationController` jak je znázorněno níže:

```csharp
public override bool FinishedLaunching (UIApplication app, 
        NSDictionary options)
{
        _window = new UIWindow (UIScreen.MainScreen.Bounds);
            
        _rootElement = new RootElement ("To Do List"){new Section ()};

        // code to create screens with MT.D will go here …

        _rootVC = new DialogViewController (_rootElement);
        _nav = new UINavigationController (_rootVC);
        _window.RootViewController = _nav;
        _window.MakeKeyAndVisible ();
            
        return true;
}
```

Výše uvedený kód vytvoří instanci `RootElement` a předává je do `DialogViewController`. `DialogViewController` Má vždy `RootElement` v horní části stránky ve své hierarchii. V tomto příkladu `RootElement` se vytvoří s řetězcem "Seznam úkolů,", který slouží jako název v navigačním panelu kontroler navigace. V tomto okamžiku spuštění aplikace by k dispozici na obrazovce vidíte níže:

 [![](elements-api-walkthrough-images/02-to-do-list-screen-.png "Spuštění aplikace bude k dispozici na obrazovce je vidět tady")](elements-api-walkthrough-images/02-to-do-list-screen-.png#lightbox)

Podívejme se, jak používat hierarchickou strukturu vaší MonoTouch.Dialog `Sections` a `Elements` na přidávání dalších obrazovek.

### <a name="creating-the-dialog-screens"></a>Vytvoření obrazovek dialogového okna

A `DialogViewController` je `UITableViewController` podtřídu, která MonoTouch.Dialog používá k přidání obrazovky. MonoTouch.Dialog vytvoří obrazovky tak, že přidáte `RootElement` k `DialogViewController`, jak jsme viděli výše. `RootElement` Může mít `Section` instancí, které představují části tabulku.
Oddíly jsou tvořené prvky, jiné části nebo i jiné `RootElements`. Vnořením `RootElements`, MonoTouch.Dialog automaticky vytvoří navigační – vizuální styl aplikaci, jak uvidíme dále.

### <a name="using-dialogviewcontroller"></a>Pomocí DialogViewController

`DialogViewController`, Se `UITableViewController` podtřídy, má `UITableView` jako jeho zobrazení. V tomto příkladu budeme chtít přidat položky do tabulky pokaždé, když <span class="ui"> + </span> klepnutí tlačítka. Protože `DialogViewController` byl přidán do `UINavigationController`, můžeme použít `NavigationItem`společnosti `RightBarButton` vlastnost přidat <span class="ui"> + </span> tlačítko, jak je znázorněno níže:

```csharp
_addButton = new UIBarButtonItem (UIBarButtonSystemItem.Add);
_rootVC.NavigationItem.RightBarButtonItem = _addButton;
```

Když jsme vytvořili `RootElement` dříve, můžeme předaný ho jediného `Section` instance tak, že můžeme přidat prvky, jako <span class="ui"> + </span> tlačítko klepnutí uživatelem. Můžeme použít následující kód k provádění obslužné rutiny pro tlačítko v tomto události:

```csharp
_addButton.Clicked += (sender, e) => {
                
        ++n;
                
        var task = new Task{Name = "task " + n, DueDate = DateTime.Now};
                
        var taskElement = new RootElement (task.Name){
                new Section () {
                        new EntryElement (task.Name, 
                                "Enter task description", task.Description)
                },
                new Section () {
                        new DateElement ("Due Date", task.DueDate)
                }
        };
        _rootElement [0].Add (taskElement);
};
```

Tento kód vytvoří novou `Task` objekt pokaždé, když klepnutí na tlačítko. Následující příklad zobrazuje jednoduchou implementaci `Task` třídy:

```csharp
public class Task
{   
        public Task ()
        {
        }
        
        public string Name { get; set; }
        
        public string Description { get; set; }

        public DateTime DueDate { get; set; }
}
```

Úkolu `Name` vlastnost se používá k vytvoření `RootElement`na titulek spolu s proměnnou čítače `n` , které se navyšuje pro každý nový úkol. MonoTouch.Dialog převede prvky na řádky, které jsou přidány do `TableView` při každé `taskElement` je přidána.

## <a name="presenting-and-managing-dialog-screens"></a>Zobrazení a správa obrazovky dialogového okna

Jsme použili `RootElement` tak, aby MonoTouch.Dialog by automaticky vytvořit novou obrazovku pro každý úkol podrobnosti a přejít k němu při výběru řádku.

Obrazovce s podrobnostmi o úlohy sám se skládá ze dvou částí; Každý z těchto oddílů obsahuje jen jeden prvek. První prvek je vytvořen z `EntryElement` stanovit upravitelný řádek úkolu `Description` vlastnost. Při výběru prvku klávesnice pro úpravy textu se zobrazí, jak je znázorněno níže:

 [![](elements-api-walkthrough-images/03-create-task.png "Při výběru prvku klávesnice pro úpravy textu se zobrazí, jak je znázorněno")](elements-api-walkthrough-images/03-create-task.png#lightbox)

Druhá část obsahuje `DateElement` , která umožňuje řídit úlohy `DueDate` vlastnost. Výběr data automaticky načte ovládacího prvku pro výběr data, jak je znázorněno:

 [![](elements-api-walkthrough-images/04-date-picker.png "Výběr data automaticky načte jako výběr data")](elements-api-walkthrough-images/04-date-picker.png#lightbox)

V obou `EntryElement` a `DateElement` případy (nebo libovolný prvek zadávání dat v MonoTouch.Dialog), všechny změny hodnoty jsou zachovány automaticky. Můžeme to ukazuje úpravou data a pak přejdete vpřed a zpět mezi kořenové obrazovky a různé podrobnosti úlohy, ve kterém jsou zachovány hodnoty v obrazovky podrobností.

## <a name="summary"></a>Souhrn

V tomto článku jsou uvedené návod, který jsme si ukázali, jak pomocí rozhraní Elements API MonoTouch.Dialog. Uvedené základní kroky k vytvoření aplikace s více obrazovkami se pro MT. D, včetně použití `DialogViewController` a přidávat prvky a oddíly, které vám umožní vytvářet obrazovky. Kromě toho se vám ukázal, jak používat pro MT. D ve spojení s `UINavigationController`.

## <a name="related-links"></a>Související odkazy

- [MTDWalkthrough (ukázka)](https://developer.xamarin.com/samples/MTDWalkthrough/)
- [Záznam dění na monitoru - Miguela de Icaza vytvoří přihlašovací obrazovka aplikace iOS s MonoTouch.Dialog](http://youtu.be/3butqB1EG0c)
- [Záznam dění na monitoru – snadno vytvářet iOS uživatelské rozhraní s MonoTouch.Dialog](http://youtu.be/j7OC5r8ZkYg)
- [Úvod do MonoTouch.Dialog](~/ios/user-interface/monotouch.dialog/index.md)
- [Návod pro rozhraní API reflexe](~/ios/user-interface/monotouch.dialog/reflection-api-walkthrough.md)
- [Návod elementu JSON](~/ios/user-interface/monotouch.dialog/json-element-walkthrough.md)
- [Dialogové okno MonoTouch na Githubu](https://github.com/migueldeicaza/MonoTouch.Dialog)
- [TweetStation aplikace](https://github.com/migueldeicaza/TweetStation)
- [Referenční třída UITableViewController](http://developer.apple.com/library/ios/#DOCUMENTATION/UIKit/Reference/UITableViewController_Class/Reference/Reference.html)
- [Referenční třída UINavigationController](http://developer.apple.com/library/ios/#documentation/UIKit/Reference/UINavigationController_Class/Reference/Reference.html)
