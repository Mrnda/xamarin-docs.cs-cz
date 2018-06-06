---
title: Úprava tabulek s Xamarin.iOS
description: Tento dokument popisuje, jak upravit tabulek v Xamarin.iOS. Popisuje prstem chcete odstranit, upravit režimu a vložení řádku.
ms.prod: xamarin
ms.assetid: EC197F25-E865-AFA3-E5CF-B33FAB7744A0
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/22/2017
ms.openlocfilehash: 28ebf1157a1bfc9f7bd910fd11365b29cecb9529
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 06/05/2018
ms.locfileid: "34789987"
---
# <a name="editing-tables-with-xamarinios"></a>Úprava tabulek s Xamarin.iOS

Úpravy funkce tabulky jsou povolit přepsáním metody v `UITableViewSource` podtřídy. Nejjednodušší úprav chování je prstem odstranění gesta, která může být implementováno s přepsání jedné metody.
Složitější úpravy (včetně přesunutí řádky) lze provádět pomocí tabulky v režimu úprav.

## <a name="swipe-to-delete"></a>Prstem k odstranění

Prstem odstranit funkce je gesto přirozené v iOS, která uživatelé očekávají, že. 

 [![](editing-images/image10.png "Příklad prstem k odstranění")](editing-images/image10.png#lightbox)

Existují tři metoda přepsání, které ovlivňují gesto prstem zobrazíte **odstranit** tlačítko v buňce:

-   **CommitEditingStyle** – zdroje tabulky zjistí, pokud tato metoda je přepsat a automaticky povolí gesto prstem odstranit. Implementaci metody by měly volat `DeleteRows` na `UITableView` způsobí buněk zmizet a také odebrat podkladová data z modelu (například pole, slovníku nebo databáze). 
-   **CanEditRow** – Pokud CommitEditingStyle přepsána, všechny řádky se předpokládá, že bude možné upravit. Pokud tato metoda je implementována a vrátí hodnotu false (pro některé konkrétní řádky nebo pro všechny řádky) pak gesto prstem odstranění nebudete mít k dispozici v dané buňky. 
-   **TitleForDeleteConfirmation** – v případě potřeby určuje text pro **odstranit** tlačítko. Pokud tato metoda není implementována text tlačítka bude "Odstranit". 


Tyto metody jsou implementované v `TableSource` třídy následovně:

```csharp
public override void CommitEditingStyle (UITableView tableView, UITableViewCellEditingStyle editingStyle, Foundation.NSIndexPath indexPath)
{
    switch (editingStyle) {
        case UITableViewCellEditingStyle.Delete:
            // remove the item from the underlying data source
            tableItems.RemoveAt(indexPath.Row);
            // delete the row from the table
            tableView.DeleteRows (new NSIndexPath[] { indexPath }, UITableViewRowAnimation.Fade);
            break;
        case UITableViewCellEditingStyle.None:
            Console.WriteLine ("CommitEditingStyle:None called");
            break;
    }
}
public override bool CanEditRow (UITableView tableView, NSIndexPath indexPath)
{
    return true; // return false if you wish to disable editing for a specific indexPath or for all rows
}
public override string TitleForDeleteConfirmation (UITableView tableView, NSIndexPath indexPath)
{   // Optional - default text is 'Delete'
    return "Trash (" + tableItems[indexPath.Row].SubHeading + ")";
}
```

V tomto příkladu `UITableViewSource` se aktualizovalo a použít `List<TableItem>` (namísto pole řetězců) jako zdroj dat, protože ji podporuje přidávání a odstraňování položek z kolekce.


## <a name="edit-mode"></a>Režim úprav

Pokud je tabulka umístěna v režimu úprav uživateli se zobrazí widget red 'zastavit' na každý řádek, který objeví tlačítko odstranění, když dotýkal. Tabulce také 'popisovač, ikona označující, zda řádek přetažením lze změnit pořadí.
**TableEditMode** ukázka implementuje tyto funkce, jak je vidět.

 [![](editing-images/image11.png "Ukázka TableEditMode implementuje tyto funkce, jak je znázorněno")](editing-images/image11.png#lightbox)

Existuje několik různých metod na `UITableViewSource` které ovlivnit chování režimu úprav tabulky:

-   **CanEditRow** – jestli se dá upravit každý řádek. Vrátí hodnotu false, aby nedošlo k prstem odstranit a odstranění v režimu úprav. 
-   **CanMoveRow** – návratový hodnota true pro povolení úchytu, nebo hodnotu false, aby přesunutí. 
-   **EditingStyleForRow** – Pokud tabulka je v režimu úprav, návratovou hodnotou z tato metoda určuje, zda buňky, zobrazí ikonu red odstranění nebo zeleným přidat ikonu. Vrátí `UITableViewCellEditingStyle.None` Pokud řádek by neměl být upravovat. 
-   **MoveRow** – volána, když je řádek přesunout tak, aby podkladová struktura dat můžete upravit tak, aby odpovídaly data, jak je uvedeno v tabulce. 


Implementace pro první tři metody je poměrně rovnou předem – Pokud chcete použít `indexPath` Pokud chcete změnit chování konkrétní řádky, jednoduše používat pevné kódování návratový hodnoty pro celou tabulku.

```csharp
public override bool CanEditRow (UITableView tableView, NSIndexPath indexPath)
{
    return true; // return false if you wish to disable editing for a specific indexPath or for all rows
}
public override bool CanMoveRow (UITableView tableView, NSIndexPath indexPath)
{
    return true; // return false if you don't allow re-ordering
}
public override UITableViewCellEditingStyle EditingStyleForRow (UITableView tableView, NSIndexPath indexPath)
{
    return UITableViewCellEditingStyle.Delete; // this example doesn't support Insert
}
```

`MoveRow` Implementace je trochu složitější, protože se musí změnit podkladová struktura dat tak, aby odpovídala nové pořadí. Protože data je implementovaný jako `List` následující kód odstraní datové položce na jeho původní umístění a vloží ho do nového umístění. Pokud data byla uložena v tabulce databáze SQLite jako sloupec 'pořadí' (například), tato metoda by místo muset provádět některé operace SQL chcete změnit pořadí čísla v tomto sloupci.

```csharp
public override void MoveRow (UITableView tableView, NSIndexPath sourceIndexPath, NSIndexPath destinationIndexPath)
{
    var item = tableItems[sourceIndexPath.Row];
    var deleteAt = sourceIndexPath.Row;
    var insertAt = destinationIndexPath.Row;
    
    // are we inserting 
    if (destinationIndexPath.Row < sourceIndexPath.Row) {
        // add one to where we delete, because we're increasing the index by inserting
        deleteAt += 1;
    } else {
        // add one to where we insert, because we haven't deleted the original yet
        insertAt += 1;
    }
    tableItems.Insert (insertAt, item);
    tableItems.RemoveAt (deleteAt);
}
```

Nakonec získat tabulky do režimu úprav, **upravit** tlačítko potřebuje volat `SetEditing` podobné výjimky

```csharp
table.SetEditing (true, true);
```

a po dokončení uživatel upravuje, **provádí** tlačítko měli vypnout režimu úprav:

```csharp
table.SetEditing (false, true);
```


## <a name="row-insertion-editing-style"></a>Úpravy stylu vložení řádku

Vložení řádků z tabulky je neobvyklé uživatelské rozhraní – hlavní příklad v aplikacích iOS standardní **upravit kontakt** obrazovky. Tento snímek obrazovky ukazuje, jak funkce vložení řádku funguje – v upravit režimu je další řádek, (při kliknutí na), vloží do data další řádky. Po dokončení úprav, dočasný **(Přidat nové)** řádek je odebrat.

 [![](editing-images/image12.png "Po dokončení úprav dočasný přidat nový řádek je odebrat.")](editing-images/image12.png#lightbox)

Existuje několik různých metod na `UITableViewSource` které ovlivnit chování režimu úprav tabulky. Tyto metody je implementovaná takto v příkladu kódu:

-   **EditingStyleForRow** – vrátí `UITableViewCellEditingStyle.Delete` pro řádky obsahující data a vrátí `UITableViewCellEditingStyle.Insert` pro poslední řádek (které se přidají konkrétně se bude chovat, jako tlačítko pro vložení). 
-   **CustomizeMoveTarget** – zatímco uživatel přechází buňku návratovou hodnotou z této volitelné metody můžete přepsat si sami vyberou umístění. To znamená, že můžete zabránit v jejich 'vyřazování' buňka v určitých pozic – například v tomto příkladu, která zabraňuje všechny řádky z přesouvání po **(Přidat nové)** řádek. 
-   **CanMoveRow** – návratový hodnota true pro povolení úchytu, nebo hodnotu false, aby přesunutí. V příkladu má poslední řádek 'úchytu' skrýt, protože je určena k serveru jako pouze tlačítko pro vložení. 


Přidáme také dva vlastních metod k přidání řádku, Vložit a pak ji znovu odebrat když už se nevyžaduje. Nazývají se z **upravit** a **provádí** tlačítka:

-   **WillBeginTableEditing** – když **upravit** tlačítko je dotýkal ho volání `SetEditing` uvést v tabulce v režimu úprav. Tím se spustí metodu WillBeginTableEditing, kde zobrazujeme **(Přidat nové)** řádek na konec tabulky tak, aby fungoval jako tlačítko Vložit. 
-   **DidFinishTableEditing** – Pokud je na tlačítko Hotovo dotýkal `SetEditing` nebude volán znovu vypnutí režimu úprav. Odebere ukázkový kód **(Přidat nové)** řádků z tabulky při úpravě se už nevyžaduje. 


Tato metoda přepsání jsou implementované v ukázkovém souboru **TableEditModeAdd/Code/TableSource.cs**:

```csharp
public override UITableViewCellEditingStyle EditingStyleForRow (UITableView tableView, NSIndexPath indexPath)
{
    if (tableView.Editing) {
        if (indexPath.Row == tableView.NumberOfRowsInSection (0) - 1)
            return UITableViewCellEditingStyle.Insert;
        else
            return UITableViewCellEditingStyle.Delete;
    } else // not in editing mode, enable swipe-to-delete for all rows
        return UITableViewCellEditingStyle.Delete;
}
public override NSIndexPath CustomizeMoveTarget (UITableView tableView, NSIndexPath sourceIndexPath, NSIndexPath proposedIndexPath)
{
    var numRows = tableView.NumberOfRowsInSection (0) - 1; // less the (add new) one
    if (proposedIndexPath.Row >= numRows)
        return NSIndexPath.FromRowSection(numRows - 1, 0);
    else
        return proposedIndexPath;
} 
public override bool CanMoveRow (UITableView tableView, NSIndexPath indexPath)
{
    return indexPath.Row < tableView.NumberOfRowsInSection (0) - 1;
}
```

Tyto dvě vlastní metody slouží k přidání a odebrání **(Přidat nové)** řádek v tabulce je úpravách režimu povolená nebo zakázaná:

```csharp
public void WillBeginTableEditing (UITableView tableView)
{
    tableView.BeginUpdates ();
    // insert the 'ADD NEW' row at the end of table display
    tableView.InsertRows (new NSIndexPath[] { 
            NSIndexPath.FromRowSection (tableView.NumberOfRowsInSection (0), 0) 
        }, UITableViewRowAnimation.Fade);
    // create a new item and add it to our underlying data (it is not intended to be permanent)
    tableItems.Add (new TableItem ("(add new)"));
    tableView.EndUpdates (); // applies the changes
}
public void DidFinishTableEditing (UITableView tableView)
{
    tableView.BeginUpdates ();
    // remove our 'ADD NEW' row from the underlying data
    tableItems.RemoveAt ((int)tableView.NumberOfRowsInSection (0) - 1); // zero based :)
    // remove the row from the table display
    tableView.DeleteRows (new NSIndexPath[] { NSIndexPath.FromRowSection (tableView.NumberOfRowsInSection (0) - 1, 0) }, UITableViewRowAnimation.Fade);
    tableView.EndUpdates (); // applies the changes
}
```

Nakonec se vytvoří tento kód **upravit** a **provádí** tlačítka s lambdas, které povolí nebo zakáže režimu úprav, kdy jsou dotyku:

```csharp
done = new UIBarButtonItem(UIBarButtonSystemItem.Done, (s,e)=>{
    table.SetEditing (false, true);
    NavigationItem.RightBarButtonItem = edit;
    tableSource.DidFinishTableEditing(table);
});
edit = new UIBarButtonItem(UIBarButtonSystemItem.Edit, (s,e)=>{
    if (table.Editing)
        table.SetEditing (false, true); // if we've half-swiped a row
    tableSource.WillBeginTableEditing(table);
    table.SetEditing (true, true);
    NavigationItem.LeftBarButtonItem = null;
    NavigationItem.RightBarButtonItem = done;
});
```

Tento vzor uživatelského rozhraní vložení řádku nepoužívá velmi často, ale můžete také `UITableView.BeginUpdates` a `EndUpdates` metody pro animaci vložení nebo odebrání buněk v tabulce. Pravidla pro používání těchto metod je, že rozdíl v hodnotě vrácené `RowsInSection` mezi `BeginUpdates` a `EndUpdates` volání musí odpovídat net počet buněk přidat/odstranit pomocí `InsertRows` a `DeleteRows` metody. Pokud se základní zdroj dat není změní tak, aby odpovídaly vložení nebo odstranění na tabulce zobrazení dojde k chybě.


## <a name="related-links"></a>Související odkazy

- [WorkingWithTables (ukázka)](https://developer.xamarin.com/samples/monotouch/WorkingWithTables)
