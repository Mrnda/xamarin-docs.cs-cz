---
title: "Návod – vytváření aplikací pomocí rozhraní API elementy"
description: "Tento článek je založen na informace uvedené v Úvod do dialogu MonoTouch článku. Představuje návod, který ukazuje způsob použití MonoTouch.Dialog (strojový překladů. D) elementy rozhraní API rychle začít vytvářet aplikace s strojový překladů. D."
ms.topic: article
ms.prod: xamarin
ms.assetid: F1124734-DF44-F1F3-0832-46F52A788CDC
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.openlocfilehash: 19e20015d1872cbaea21dd8b8e5431981e463c33
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 02/27/2018
---
# <a name="walkthrough---creating-an-application-using-the-elements-api"></a>Návod – vytváření aplikací pomocí rozhraní API elementy

_Tento článek je založen na informace uvedené v Úvod do dialogu MonoTouch článku. Představuje návod, který ukazuje způsob použití MonoTouch.Dialog (strojový překladů. D) elementy rozhraní API rychle začít vytvářet aplikace s strojový překladů. D._

V tomto návodu použijeme strojový překladů. Rozhraní API elementy D vytvořit seznam podrobnosti styl aplikace, která zobrazí seznam úloh. Když uživatel vybere <span class="ui"> + </span> tlačítko na navigačním panelu, bude přidán nový řádek do tabulky pro úlohu. Výběr řádku bude přejděte na obrazovce podrobností, které umožňuje aktualizovat popis úlohy a datum splatnosti, jak je uvedeno dále:

 [ ![](elements-api-walkthrough-images/01-task-list-app.png "Výběr řádku bude přejděte na obrazovce podrobností, které umožňuje aktualizovat popis úlohy a datum splatnosti")](elements-api-walkthrough-images/01-task-list-app.png)

 <a name="Elements_API_Walkthrough" />


## <a name="elements-api-walkthrough"></a>Prvky rozhraní API návod

V [Úvod do dialogového okna MonoTouch](~/ios/user-interface/monotouch.dialog/index.md) článku jsme získávají dobré porozumění různé části strojový překladů. D. Umožňuje použít rozhraní API elementy všechny najednou uvést do aplikace.

 <a name="Setting_up_the_Multi-Screen_Application" />


## <a name="setting-up-the-multi-screen-application"></a>Nastavení aplikace s více obrazovky

Ke spuštění procesu vytvoření obrazovky, vytvoří MonoTouch.Dialog `DialogViewController`a potom se přidají `RootElement`.

Pokud chcete vytvořit více obrazovky aplikace s MonoTouch.Dialog, je potřeba:

1.  Vytvoření  `UINavigationController.`
1.  Vytvoření  `DialogViewController.`
1.  Přidat `DialogViewController` jako kořen  `UINavigationController.` 
1.  Přidat `RootElement` na  `DialogViewController.`
1.  Přidat `Sections` a `Elements` na  `RootElement.` 


 <a name="Using_A_UINavigationController" />


### <a name="using-a-uinavigationcontroller"></a>Použití UINavigationController

Vytvoření aplikace stylu navigace, je potřeba vytvořit `UINavigationController`a poté přidejte jej jako `RootViewController` v `FinishedLaunching` metodu `AppDelegate`. Chcete-li `UINavigationController` pracovat s MonoTouch.Dialog, přidáme `DialogViewController` k `UINavigationController` jak je uvedeno níže:

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

Výše uvedený kód vytvoří instanci `RootElement` a předá ji do `DialogViewController`. `DialogViewController` Má vždy `RootElement` v horní části hierarchii. V tomto příkladu `RootElement` je vytvořena s řetězcem "Seznam úkolů,", který slouží jako název navigační řadiče navigačním panelu. Spuštění aplikace v tomto okamžiku by prezentovat obrazovky vidíte níže:

 [ ![](elements-api-walkthrough-images/02-to-do-list-screen-.png "Spuštění aplikace bude k dispozici na obrazovce zobrazeny zde")](elements-api-walkthrough-images/02-to-do-list-screen-.png)

Podívejme se, jak používat MonoTouch.Dialog je hierarchická struktura `Sections` a `Elements` přidat další obrazovky.

 <a name="Creating_the_Dialog_Screens" />


### <a name="creating-the-dialog-screens"></a>Vytvoření obrazovek dialogové okno

A `DialogViewController` je `UITableViewController` podtřídami, který MonoTouch.Dialog používá k přidání obrazovky. MonoTouch.Dialog vytvoří obrazovky přidáním `RootElement` k `DialogViewController`, jak jsme viděli výše. `RootElement` Může mít `Section` instancí, které představují části tabulky.
Oddíly jsou tvořeny prvky, dalších částech nebo i v jiných `RootElements`. Vnořením `RootElements`, MonoTouch.Dialog automaticky vytvoří navigační stylu aplikaci, jako ukážeme Další.

 <a name="Using_DialogViewController" />


### <a name="using-dialogviewcontroller"></a>Pomocí DialogViewController

`DialogViewController`, Se `UITableViewController` podtřídami, má `UITableView` jako jeho zobrazení. V tomto příkladu jsme chcete přidat položky do tabulce pokaždé, když <span class="ui"> + </span> je stisknuté tlačítko. Vzhledem k tomu `DialogViewController` byla přidána do `UINavigationController`, můžeme použít `NavigationItem`na `RightBarButton` vlastnost přidat <span class="ui"> + </span> tlačítko, jak je uvedeno níže:

```csharp
_addButton = new UIBarButtonItem (UIBarButtonSystemItem.Add);
_rootVC.NavigationItem.RightBarButtonItem = _addButton;
```

Pokud jsme vytvořili `RootElement` dříve, jsme předán ho jedné `Section` instance tak, aby přidáme může elementy jako <span class="ui"> + </span> je tlačítko stisknuté uživatelem. V tomto událostí provedete obslužné rutiny pro tlačítko jsme můžete použít následující kód:

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

Tento kód vytvoří novou `Task` objekt pokaždé, když je stisknuté tlačítko. Následující příklad zobrazuje jednoduchou implementaci `Task` třídy:

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

 []()

Úkolu `Name` vlastnost se používá k vytvoření `RootElement`na titulek společně s čítač proměnné s názvem `n` pro každý nový úkol, který se zvýší. MonoTouch.Dialog se změní na řádky, které jsou přidány do elementů `TableView` při každé `taskElement` je přidána.

 <a name="Presenting_and_Managing_Dialog_Screens" />


## <a name="presenting-and-managing-dialog-screens"></a>Prezentace a správu obrazovky dialogové okno

Použili jsme `RootElement` tak, aby MonoTouch.Dialog by automaticky vytvořit nové obrazovky pro každý úkol podrobnosti a přejděte k němu, když je vybraný řádek.

Vlastní obrazovka podrobnosti úlohy se skládá ze dvou částech; Každý z těchto částí obsahuje jen jeden prvek. První prvek je vytvořený z `EntryElement` zajistit upravitelné řádek pro úkolu `Description` vlastnost. Pokud je vybraný element, klávesnice pro úpravy textu se zobrazí, jak je uvedeno níže:

 [ ![](elements-api-walkthrough-images/03-create-task.png "Pokud je vybraný element, klávesnice pro úpravy textu se zobrazí, jak je znázorněno")](elements-api-walkthrough-images/03-create-task.png)

Druhý oddíl obsahuje `DateElement` která umožňuje nám spravovat úkolu `DueDate` vlastnost. Výběr datum automaticky načte výběr data, jak je znázorněno:

 [ ![](elements-api-walkthrough-images/04-date-picker.png "Výběr data jako výběr datum automaticky načte.")](elements-api-walkthrough-images/04-date-picker.png)

V obou `EntryElement` a `DateElement` případech (nebo pro libovolný element zadávání dat v MonoTouch.Dialog), všechny změny na hodnoty se zachovají automaticky. Jsme to ukazují úpravy datum a mezi obrazovce kořenové a různé podrobnosti úlohy, kde se zachovají hodnoty na obrazovkách podrobností navigace a zpět.

 <a name="Summary" />


## <a name="summary"></a>Souhrn

V tomto článku jsou uvedené návod, který vám ukázal, jak použít rozhraní API MonoTouch.Dialog elementy. O něm zmínka základní kroky pro vytvoření více obrazovky aplikace s strojový překladů. D, včetně toho, jak používat `DialogViewController` a jak přidat elementy a oddíly, k vytvoření obrazovky. Kromě toho se vám ukázal, jak používat strojový překladů. D ve spojení s `UINavigationController`.


## <a name="related-links"></a>Související odkazy

- [MTDWalkthrough (ukázka)](https://developer.xamarin.com/samples/MTDWalkthrough/)
- [Záznam dění na monitoru - Miguel de Icaza vytvoří obrazovka pro přihlášení iOS s MonoTouch.Dialog](http://youtu.be/3butqB1EG0c)
- [Záznam dění na monitoru - snadno vytvářet iOS uživatelského rozhraní s MonoTouch.Dialog](http://youtu.be/j7OC5r8ZkYg)
- [Úvod do MonoTouch.Dialog](~/ios/user-interface/monotouch.dialog/index.md)
- [Návod rozhraní API reflexe](~/ios/user-interface/monotouch.dialog/reflection-api-walkthrough.md)
- [Návod Element JSON](~/ios/user-interface/monotouch.dialog/json-element-walkthrough.md)
- [Dialogové okno MonoTouch na Githubu](https://github.com/migueldeicaza/MonoTouch.Dialog)
- [TweetStation aplikace](https://github.com/migueldeicaza/TweetStation)
- [Odkaz na UITableViewController – třída](http://developer.apple.com/library/ios/#DOCUMENTATION/UIKit/Reference/UITableViewController_Class/Reference/Reference.html)
- [Odkaz na UINavigationController – třída](http://developer.apple.com/library/ios/#documentation/UIKit/Reference/UINavigationController_Class/Reference/Reference.html)
