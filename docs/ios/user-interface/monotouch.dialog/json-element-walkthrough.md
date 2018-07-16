---
title: Vytvoření uživatelského rozhraní v Xamarin.iOS pomocí JSON
description: MonoTouch.Dialog (MT.) D) zahrnuje podporu pro dynamické generování uživatelského rozhraní pomocí dat JSON. V tomto kurzu provedeme procesem použití JSONElement k vytvoření uživatelského rozhraní z formátu JSON, který je součástí aplikace, nebo načíst z vzdálené adresy Url.
ms.prod: xamarin
ms.assetid: E353DF14-51D7-98E3-59EA-16683C770C23
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.openlocfilehash: 94cef78bb7eedc03192071f17af765ebb702e260
ms.sourcegitcommit: cb80df345795989528e9df78eea8a5b45d45f308
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/14/2018
ms.locfileid: "39038492"
---
# <a name="using-json-to-create-a-user-interface-in-xamarinios"></a>Vytvoření uživatelského rozhraní v Xamarin.iOS pomocí JSON

_MonoTouch.Dialog (MT.) D) zahrnuje podporu pro dynamické generování uživatelského rozhraní pomocí dat JSON. V tomto kurzu provedeme procesem použití JSONElement k vytvoření uživatelského rozhraní z formátu JSON, který je součástí aplikace, nebo načíst z vzdálené adresy Url._

PRO MT. D podporuje vytváření uživatelského rozhraní, které jsou deklarovány ve formátu JSON. Když elementů jsou deklarovány pomocí formátu JSON, pro MT. D související prvky pro vytvoření automaticky. JSON je možné načíst z místního souboru analyzovaný `JsonObject` instance nebo dokonce i vzdálené adresy Url.

PRO MT. D podporuje celou škálu funkcí, které jsou k dispozici v rozhraní API pro prvky při použití formátu JSON. Například aplikace na následujícím snímku obrazovky je zcela deklarovat pomocí formátu JSON:

[![](json-element-walkthrough-images/01-load-from-file.png "Aplikace na tomto snímku obrazovky je zcela deklarovány například pomocí JSON") ](json-element-walkthrough-images/01-load-from-file.png#lightbox) [ ![ ] (json-element-walkthrough-images/01-load-from-file.png "aplikace na tomto snímku obrazovky je zcela deklarovány například pomocí JSON")](json-element-walkthrough-images/01-load-from-file.png#lightbox)

Vraťme se k příklad z [prvky rozhraní API návod](~/ios/user-interface/monotouch.dialog/elements-api-walkthrough.md) kurz ukazuje, jak přidat obrazovky podrobností úlohy pomocí formátu JSON.

## <a name="setting-up-mtd"></a>Nastavení pro MT. D

PRO MT. D je distribuován spolu s Xamarin.iOS. Ho Pokud chcete použít, klikněte pravým tlačítkem na **odkazy** uzel Xamarin.iOS projektu v sadě Visual Studio 2017 nebo Visual Studio pro Mac a přidejte odkaz na **MonoTouch.Dialog 1** sestavení. Pak přidejte `using MonoTouch.Dialog` příkazy ve zdrojovém kódu podle potřeby.

## <a name="json-walkthrough"></a>Návod JSON

V příkladu v tomto návodu umožňuje úlohy, který se má vytvořit. Vyberete úlohu na první obrazovce se zobrazí obrazovce s podrobnostmi o, jak je znázorněno:

 [![](json-element-walkthrough-images/03-task-list.png "Vyberete úlohu na první obrazovce se zobrazí obrazovce s podrobnostmi o, jak je znázorněno")](json-element-walkthrough-images/03-task-list.png#lightbox)

## <a name="creating-the-json"></a>Vytváří se kód JSON

V tomto příkladu budete načteme JSON ze souboru v projektu s názvem `task.json`. PRO MT. D se očekává JSON tak, aby odpovídal syntaxi, která zrcadlí rozhraní Elements API. Stejně jako pomocí rozhraní Elements API z kódu, při použití formátu JSON, můžeme deklarovat oddíly a v těchto částech se nám přidat prvky. Chcete-li deklarovat oddíly a elementy ve formátu JSON, používáme řetězce "části" a "prvky" v uvedeném pořadí jako klíče. Pro každý prvek se nastavuje pomocí přidruženého prvku typu `type` klíč. Každých dalších prvků je nastavena s názvem vlastnosti jako klíč.

Například následující kód JSON popisuje části a prvky pro podrobnosti úlohy:

```csharp
{
    "title": "Task",
    "sections": [
        {
          "elements" : [
            {
                "id" : "task-description",
                "type": "entry",
                "placeholder": "Enter task description"
            },
            {
                "id" : "task-duedate",
                "type": "date",
                "caption": "Due Date",
                "value": "00:00"
            }
         ]
        }
    ]
  }
```

Všimněte si, že výše uvedené JSON obsahuje id pro každý prvek. Libovolný element může obsahovat identifikátor, na něj odkazovat v době běhu. Podíváme se, jak to se používá v okamžiku, kdy vám ukážeme, jak načíst JSON v kódu.

## <a name="loading-the-json-in-code"></a>Načítají se ve formátu JSON v kódu

Jakmile je definována ve formátu JSON, potřebujeme jejich načtení do služby pro MT. Pomocí D `JsonElement` třídy. Za předpokladu, že soubor pomocí tohoto kódu JSON, který jsme vytvořili výše byl přidán do projektu s názvem sample.json a zadané akci sestavení obsahu, načítání `JsonElement` je stejně jednoduché jako volání následující řádek kódu:

```csharp
var taskElement = JsonElement.FromFile ("task.json");
```

Protože přidáváme to na vyžádání pokaždé, když je vytvořena úloha, můžeme upravit tlačítko kliknutí z předchozího příkladu Elements API následujícím způsobem:

```csharp
_addButton.Clicked += (sender, e) => {

        ++n;

        var task = new Task{Name = "task " + n, DueDate = DateTime.Now};

        var taskElement = JsonElement.FromFile ("task.json");

        _rootElement [0].Add (taskElement);
};
```

## <a name="accessing-elements-at-runtime"></a>Přístup k prvkům v době běhu

Připomínáme, že id jsme přidali do oba prvky jsme deklarované v souboru JSON. Vlastnost id jsme můžete použít pro přístup k každý prvek v době běhu k úpravě jeho vlastností v kódu. Například následující kód odkazuje na prvky vstupní data a času k nastavení hodnot z objektu úlohy:

```csharp
_addButton.Clicked += (sender, e) => {

        ++n;

        var task = new Task{Name = "task " + n, DueDate = DateTime.Now};

        var taskElement = JsonElement.FromFile ("task.json");

        taskElement.Caption = task.Name;

        var description = taskElement ["task-description"] as EntryElement;

        if (description != null) {
                description.Caption = task.Name;
                description.Value = task.Description;       
        }

        var duedate = taskElement ["task-duedate"] as DateElement;

        if (duedate != null) {                
                duedate.DateValue = task.DueDate;
        }
        _rootElement [0].Add (taskElement);
};
```

## <a name="loading-json-from-a-url"></a>Načítání JSON z adresy url

PRO MT. D také podporuje dynamicky načítání JSON z externí adresu Url jednoduše předáním adresu Url do konstruktoru `JsonElement`. PRO MT. D se rozbalí hierarchii deklarované v kódu JSON na vyžádání, jako je navigace mezi obrazovkami. Představme si třeba, například následující soubor JSON umístěný v kořenové složce místní webový server:

```csharp
{
    "type": "root",
    "title": "home",
    "sections": [
       {
         "header": "Nested view!",
         "elements": [
           {
             "type": "boolean",
             "caption": "Just a boolean",
             "id": "first-boolean",
             "value": false
           },
           {
             "type": "string",
             "caption": "Welcome to the nested controller"
           }
         ]
       }
     ]
}
```

Můžete načteme to pomocí `JsonElement` stejně jako v následujícím kódu:

```csharp
_rootElement = new RootElement ("Json Example"){
        new Section (""){ new JsonElement ("Load from url",
                "http://localhost/sample.json")
        }
};
```

Za běhu budou se soubor načten a analyzován pomocí pro MT. D, když uživatel přejde na druhém zobrazení, jak je znázorněno v následujícím snímku obrazovky:

 [![](json-element-walkthrough-images/04-json-web-example.png "Soubor bude načten a analyzován pomocí pro MT. D, když uživatel přejde na druhém zobrazení")](json-element-walkthrough-images/04-json-web-example.png#lightbox)

## <a name="summary"></a>Souhrn

Tento článek vám ukázal, jak vytvořit pomocí rozhraní pro MT. D z formátu JSON. To vám ukázal, jak načíst JSON, které jsou obsaženy v souboru s aplikací i ze vzdálené adresy Url. Také ukázala, jak přistupovat k prvkům popsaných ve formátu JSON za běhu.

## <a name="related-links"></a>Související odkazy

- [MTDJsonDemo (ukázka)](https://developer.xamarin.com/samples/MTDJsonDemo/)
- [Záznam dění na monitoru - Miguela de Icaza vytvoří přihlašovací obrazovka aplikace iOS s MonoTouch.Dialog](http://youtu.be/3butqB1EG0c)
- [Záznam dění na monitoru – snadno vytvářet iOS uživatelské rozhraní s MonoTouch.Dialog](http://youtu.be/j7OC5r8ZkYg)
- [Úvod do MonoTouch.Dialog](~/ios/user-interface/monotouch.dialog/index.md)
- [Návod prvky rozhraní API](~/ios/user-interface/monotouch.dialog/elements-api-walkthrough.md)
- [Návod pro rozhraní API reflexe](~/ios/user-interface/monotouch.dialog/reflection-api-walkthrough.md)
- [Dialogové okno MonoTouch na Githubu](https://github.com/migueldeicaza/MonoTouch.Dialog)
- [TweetStation aplikace](https://github.com/migueldeicaza/TweetStation)
- [Referenční třída UITableViewController](http://developer.apple.com/library/ios/#DOCUMENTATION/UIKit/Reference/UITableViewController_Class/Reference/Reference.html)
- [Referenční třída UINavigationController](http://developer.apple.com/library/ios/#documentation/UIKit/Reference/UINavigationController_Class/Reference/Reference.html)
