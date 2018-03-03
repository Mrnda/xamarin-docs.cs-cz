---
title: "Práce s akcemi řádek"
description: "Tato příručka ukazuje, jak vytvořit vlastní prstem akce pro řádky tabulky s UISwipeActionsConfiguration nebo UITableViewRowAction"
ms.topic: article
ms.prod: xamarin
ms.assetid: 340FB633-0C46-40AA-9963-FF17D7CA6858
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 09/25/2017
ms.openlocfilehash: e9d3e2eecd4c03e7b3046e1ad86dd8a0d70a7f73
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 02/27/2018
---
# <a name="working-with-row-actions"></a>Práce s akcemi řádek

_Tato příručka ukazuje, jak vytvořit vlastní prstem akce pro řádky tabulky s UISwipeActionsConfiguration nebo UITableViewRowAction_

![Ukázka prstem akce v řádcích](row-action-images/action02.png)

iOS nabízí dva způsoby, jak provádět akce v tabulce: `UISwipeActionsConfiguration` a `UITableViewRowAction`.

`UISwipeActionsConfiguration` byla zavedena v systému iOS 11 a slouží k definování sady akcí, které by měl provést umístit při swipes uživatele _v obou směrech_ na řádek v tabulce zobrazení. Toto chování je podobné jako nativní Mail.app 

`UITableViewRowAction` Třída se používá k definování akci, která se provede při swipes uživatele vodorovně na řádek v tabulce zobrazení vlevo.
Například když úpravy tabulce k načtení vlevo na řádek zobrazí **odstranit** tlačítko ve výchozím nastavení. Připojením více instancí `UITableViewRowAction` třídy k `UITableView`, lze definovat více vlastních akcí, každou s vlastní text, formátování a chování.


## <a name="uiswipeactionsconfiguration"></a>UISwipeActionsConfiguration

Existují tři kroky potřebné při implementaci prstem akce s `UISwipeActionsConfiguration`:

1. Přepsání `GetLeadingSwipeActionsConfiguration` nebo `GetTrailingSwipeActionsConfiguration` metody. Tyto metody vrací `UISwipeActionsConfiguration`. 
2. Vytvoření instance `UISwipeActionsConfiguration` má být vrácen. Tato třída má pole `UIContextualAction`.
3. Vytvoření `UIContextualAction`.

Tyto jsou vysvětlené podrobněji v následujících částech.

### <a name="1-implementing-the-swipeactionsconfigurations-methods"></a>1. Implementace metody SwipeActionsConfigurations

`UITableViewController` (a také `UITableViewSource` a `UITableViewDelegate`) obsahovat dvě metody: `GetLeadingSwipeActionsConfiguration` a `GetTrailingSwipeActionsConfiguration`, které se používají k implementaci sadu akcí prstem na zobrazení řádku tabulky. Úvodní akce prstem odkazuje prstem z na levé straně obrazovky v jazyce zleva doprava a pravé straně obrazovky v jazyce zprava doleva. 

Následující příklad (z [TableSwipeActions](https://developer.xamarin.com/samples/monotouch/TableSwipeActions) ukázkové) ukazuje implementaci úvodní konfigurace prstem. Dvě akce jsou vytvořené pomocí možnosti kontextové akce, které jsou vysvětlené [pod](#create-uicontextualaction). Tyto akce jsou pak předané do nově inicializovaného [ `UISwipeActionsConfiguration` ](#create-uiswipeactionsconfigurations), který slouží jako návratová hodnota.


```csharp
public override UISwipeActionsConfiguration GetLeadingSwipeActionsConfiguration(UITableView tableView, NSIndexPath indexPath)
{
    //UIContextualActions
    var definitionAction = ContextualDefinitionAction(indexPath.Row);
    var flagAction = ContextualFlagAction(indexPath.Row);

    //UISwipeActionsConfiguration
    var leadingSwipe = UISwipeActionsConfiguration.FromActions(new UIContextualAction[] { flagAction, definitionAction });
    
    leadingSwipe.PerformsFirstActionWithFullSwipe = false;
    
    return leadingSwipe;
}  
```

<a name="create-uiswipeactionsconfigurations" />

### <a name="2-instantiate-a-uiswipeactionsconfiguration"></a>2. Vytváření instancí `UISwipeActionsConfiguration`

Vytváření instancí `UISwipeActionsConfiguration` pomocí `FromActions` metoda pro přidání nové pole `UIContextualAction`s, jak je znázorněno v následující fragment kódu:

```csharp
var leadingSwipe = UISwipeActionsConfiguration.FromActions(new UIContextualAction[] { flagAction, definitionAction })

leadingSwipe.PerformsFirstActionWithFullSwipe = false;
```

Je důležité si uvědomit, že je pořadí, ve kterém se zobrazí vaše akce závisí na tom, jak se předávají do pole. Například kód výše pro úvodní swipes zobrazí jako tak akce:

![úvodní akce prstem zobrazí na řádek tabulky](row-action-images/action03.png)

U koncové swipes, bude akce zobrazen, jak je znázorněno na následujícím obrázku:

![Koncové prstem akce zobrazí na řádek tabulky](row-action-images/action04.png)

Tento fragment kódu také využívá nové `PerformsFirstActionWithFullSwipe` vlastnost. Ve výchozím nastavení je tato vlastnost nastavena hodnotu true, což znamená, že je první akcí v poli se stane, když uživatel swipes plně na řádek. Pokud máte akci, která není destruktivní (například "odstranit", nemusí to být ideální chování a měli byste proto nastavit ho na `false`.

<a name="create-uicontextualaction" />

### <a name="create-a-uicontextualaction"></a>Vytvoření `UIContextualAction`

Kontextové akce je ve skutečnosti vytvoříte akci, která se zobrazí, když uživatel swipes řádku tabulky.

K chybě při inicializaci akce je nutné zadat `UIContextualActionStyle`, a název a `UIContextualActionHandler`. `UIContextualActionHandler` Přijímá tři parametry: akce, která akce se zobrazí v zobrazení a obslužnou rutinu dokončení:

```csharp
public UIContextualAction ContextualFlagAction(int row)
{
    var action = UIContextualAction.FromContextualActionStyle
                    (UIContextualActionStyle.Normal,
                        "Flag",
                        (FlagAction, view, success) => {
                            var alertController = UIAlertController.Create($"Report {words[row]}?", "", UIAlertControllerStyle.Alert);
                            alertController.AddAction(UIAlertAction.Create("Cancel", UIAlertActionStyle.Cancel, null)); 
                            alertController.AddAction(UIAlertAction.Create("Yes", UIAlertActionStyle.Destructive, null));
                            PresentViewController(alertController, true, null);
                            
                            success(true);
                        });

    action.Image = UIImage.FromFile("feedback.png");
    action.BackgroundColor = UIColor.Blue;

    return action;
}
```

Různé visual vlastnosti, jako je například barvy pozadí nebo obrázku akce se dá upravit. Výše uvedeném fragmentu kódu ukazuje na akce přidají a nastavení jeho modrou barvu pozadí.

Po vytvoření kontextové akce můžete použít k chybě při inicializaci `UISwipeActionsConfiguration` v `GetLeadingSwipeActionsConfiguration` metoda.

## <a name="uitableviewrowaction"></a>UITableViewRowAction

Můžete určit jeden nebo více řádek vlastní akce pro `UITableView`, budete muset vytvořit instanci `UITableViewDelegate` třídy a přepsat `EditActionsForRow` metoda. Příklad:

```csharp
using System;
using System.Collections.Generic;
using System.IO;
using Foundation;
using UIKit;

namespace BasicTable
{
    public class TableDelegate : UITableViewDelegate
    {
        #region Constructors
        public TableDelegate ()
        {
        }

        public TableDelegate (IntPtr handle) : base (handle)
        {
        }

        public TableDelegate (NSObjectFlag t) : base (t)
        {
        }

        #endregion

        #region Override Methods
        public override UITableViewRowAction[] EditActionsForRow (UITableView tableView, NSIndexPath indexPath)
        {
            UITableViewRowAction hiButton = UITableViewRowAction.Create (
                UITableViewRowActionStyle.Default,
                "Hi",
                delegate {
                    Console.WriteLine ("Hello World!");
                });
            return new UITableViewRowAction[] { hiButton };
        }
        #endregion
    }
}
```

Statické `UITableViewRowAction.Create` metoda se používá k vytvoření nové `UITableViewRowAction` , se zobrazí **HIS použití** když uživatel swipes vodorovně na řádek v tabulce vlevo. Později novou instanci třídy `TableDelegate` se vytvoří a připojené k `UITableView`. Příklad:

```csharp
TableDelegate tableDelegate;
...

// Replace the standard delete button with a "Hi" button
tableDelegate = new TableDelegate ();
table.Delegate = tableDelegate;

```

Při spuštění ve výše uvedeném kódu a swipes uživatele na řádku tabulky vlevo **HIS použití** tlačítko se zobrazí místo **odstranit** tlačítko, které je ve výchozím nastavení zobrazí:

[ ![](row-action-images/action01.png "Tlačítko HIS použití se zobrazí místo tlačítko Odstranit")](row-action-images/action01.png)

Pokud uživatel klepnutím **HIS použití** tlačítko `Hello World!` se zapíšou do konzoly nástroje v sadě Visual Studio pro Mac nebo Visual Studio při spuštění aplikace v režimu ladění.



## <a name="related-links"></a>Související odkazy

- [TableSwipeActions (ukázka)](https://developer.xamarin.com/samples/monotouch/TableSwipeActions)
- [WorkingWithTables (ukázka)](https://developer.xamarin.com/samples/monotouch/WorkingWithTables)
