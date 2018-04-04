---
title: 'Návod: Použití prvku JSON pro vytvoření uživatelského rozhraní'
description: MonoTouch.Dialog (strojový překladů. D) zahrnuje podporu pro dynamické generování uživatelského rozhraní pomocí JSON data. V tomto kurzu budeme zabývat použití JSONElement vytvořit uživatelské rozhraní z formátu JSON, který je buď součástí aplikace, nebo načíst z vzdálené adresy Url.
ms.prod: xamarin
ms.assetid: E353DF14-51D7-98E3-59EA-16683C770C23
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.openlocfilehash: 322857295383d17da03507bdd5ac78753f8c0619
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/04/2018
---
# <a name="walkthrough-using-a-json-element-to-create-a-user-interface"></a>Návod: Použití prvku JSON pro vytvoření uživatelského rozhraní

_MonoTouch.Dialog (strojový překladů. D) zahrnuje podporu pro dynamické generování uživatelského rozhraní pomocí JSON data. V tomto kurzu budeme zabývat použití JSONElement vytvořit uživatelské rozhraní z formátu JSON, který je buď součástí aplikace, nebo načíst z vzdálené adresy Url._


STROJOVÝ PŘEKLADŮ. D podporuje vytváření uživatelské rozhraní deklarovat ve formátu JSON. Když jsou elementy deklarováno s použitím JSON, strojový překladů. D přidružených elementů pro vytvoření automaticky. JSON je možné načíst buď z místního souboru Analyzovaná `JsonObject` instance nebo i vzdálenou adresou Url.

STROJOVÝ PŘEKLADŮ. D podporuje plný rozsah funkcí, které jsou k dispozici v rozhraní API elementy při použití formátu JSON. Například aplikace na následujícím snímku obrazovky je zcela deklarováno s použitím JSON:

[![](json-element-walkthrough-images/01-load-from-file.png "Například aplikace na tomto snímku obrazovky je zcela deklarováno s použitím JSON") ](json-element-walkthrough-images/01-load-from-file.png#lightbox) [ ![ ] (json-element-walkthrough-images/01-load-from-file.png "například aplikace na tomto snímku obrazovky je zcela deklarováno s použitím JSON")](json-element-walkthrough-images/01-load-from-file.png#lightbox)

Pojďme pokroku příklad z [prvky rozhraní API návod](~/ios/user-interface/monotouch.dialog/elements-api-walkthrough.md) kurzu znázorňující přidat úloha podrobností obrazovky pomocí JSON.

## <a name="json-walkthrough"></a>Návod JSON

V příkladu v tomto návodu umožňuje vytvořit úkoly. Pokud úloha je vybraná na první obrazovce, obrazovky podrobností se zobrazí, jak je znázorněno:

 [![](json-element-walkthrough-images/03-task-list.png "Pokud úloha je vybraná na první obrazovce, obrazovky podrobností se zobrazí, jak je znázorněno")](json-element-walkthrough-images/03-task-list.png#lightbox)

## <a name="creating-the-json"></a>Vytváření kódu JSON

V tomto příkladu jsme budete načíst JSON ze souboru v projektu s názvem `task.json`. STROJOVÝ PŘEKLADŮ. D očekává JSON tak, aby odpovídala syntaxe, které odpovídá prvky rozhraní API. Stejně jako pomocí rozhraní API elementy z kódu, pokud používáte JSON, jsme deklarovat části a v těchto částech přidáme elementy. Deklarace částí a elementy ve formátu JSON, používáme řetězce "části" a "prvků" v uvedeném pořadí jako klíče. Pro každý element, typ přidruženého prvku je nastaven pomocí `type` klíč. Každý další prvky je nastavena název vlastnosti jako klíč.

Například následujícím kódu JSON popisuje části a prvky pro podrobnosti úlohy:

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

Všimněte si výše uvedený kód JSON obsahuje id pro každý prvek. Libovolný element může obsahovat id, na něj odkazovat za běhu. Ukážeme, jak to se používá v okamžiku, kdy ukážeme, jak načíst JSON v kódu.

 <a name="Loading_the_JSON_in_Code" />


## <a name="loading-the-json-in-code"></a>Načítání JSON v kódu

Po definování formátu JSON, je potřeba načíst do strojový překladů. Pomocí D `JsonElement` třídy. Za předpokladu, že soubor s JSON jsme vytvořili výše má byl přidán do projektu s názvem sample.json a zadané akce sestavení obsahu, načítání `JsonElement` je jednoduché, volání následující řádek kódu:

```csharp
var taskElement = JsonElement.FromFile ("task.json");
```

Vzhledem k tomu, že jsme přidáváte na vyžádání pokaždé, když je vytvořena úloha, jsme upravte tlačítko klikli z předchozího příkladu elementy API následujícím způsobem:

```csharp
_addButton.Clicked += (sender, e) => {

        ++n;

        var task = new Task{Name = "task " + n, DueDate = DateTime.Now};

        var taskElement = JsonElement.FromFile ("task.json");

        _rootElement [0].Add (taskElement);
};
```

 <a name="Accessing_Elements_at_Runtime" />


## <a name="accessing-elements-at-runtime"></a>Přístup k elementům za běhu

Odvolat, že id jsme přidali na oba elementy jsme deklarovaného v souboru JSON. Vlastnost id jsme můžete použít pro přístup k každý prvek v době běhu změna jejich vlastností v kódu. Například následující kód odkazuje na položku a datum prvky pro nastavení hodnot z objektu úlohy:

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

 <a name="Loading_JSON_from_a_Url" />


## <a name="loading-json-from-a-url"></a>Načítání JSON z adresy Url

STROJOVÝ PŘEKLADŮ. D také podporuje dynamicky načítání JSON z externí adresu Url jednoduše předáním adresu Url do konstruktoru objektu `JsonElement`. STROJOVÝ PŘEKLADŮ. D rozšíří hierarchii deklarovaných v kódu JSON na vyžádání, jako je přecházet mezi obrazovky. Představte si třeba soubor JSON, jako je třeba níže umístěný v kořenovém adresáři místního webového serveru:

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

Nám můžete načíst to pomocí `JsonElement` jako v následujícím kódu:

```csharp
_rootElement = new RootElement ("Json Example"){
        new Section (""){ new JsonElement ("Load from url",
                "http://localhost/sample.json")
        }
};
```

V době běhu soubor načíst a analyzovat podle strojový překladů. D, když uživatel přejde na druhý zobrazení, jak ukazuje následující snímek obrazovky:

 [![](json-element-walkthrough-images/04-json-web-example.png "Soubor se načíst a analyzovat podle strojový překladů. Když uživatel přejde na druhý zobrazení D")](json-element-walkthrough-images/04-json-web-example.png#lightbox)

 <a name="Summary" />


## <a name="summary"></a>Souhrn

Tento článek vám ukázal, jak vytvořit pomocí rozhraní s strojový překladů. D z formátu JSON. Je vám ukázal, jak načíst JSON do souboru s aplikací i ze vzdálené adresy Url. Také ukázal, jak přístup k elementům popsané ve formátu JSON za běhu.


## <a name="related-links"></a>Související odkazy

- [MTDJsonDemo (ukázka)](https://developer.xamarin.com/samples/MTDJsonDemo/)
- [Záznam dění na monitoru - Miguel de Icaza vytvoří obrazovka pro přihlášení iOS s MonoTouch.Dialog](http://youtu.be/3butqB1EG0c)
- [Záznam dění na monitoru - snadno vytvářet iOS uživatelského rozhraní s MonoTouch.Dialog](http://youtu.be/j7OC5r8ZkYg)
- [Úvod do MonoTouch.Dialog](~/ios/user-interface/monotouch.dialog/index.md)
- [Prvky rozhraní API návod](~/ios/user-interface/monotouch.dialog/elements-api-walkthrough.md)
- [Návod rozhraní API reflexe](~/ios/user-interface/monotouch.dialog/reflection-api-walkthrough.md)
- [Dialogové okno MonoTouch na Githubu](https://github.com/migueldeicaza/MonoTouch.Dialog)
- [TweetStation aplikace](https://github.com/migueldeicaza/TweetStation)
- [Odkaz na UITableViewController – třída](http://developer.apple.com/library/ios/#DOCUMENTATION/UIKit/Reference/UITableViewController_Class/Reference/Reference.html)
- [Odkaz na UINavigationController – třída](http://developer.apple.com/library/ios/#documentation/UIKit/Reference/UINavigationController_Class/Reference/Reference.html)
