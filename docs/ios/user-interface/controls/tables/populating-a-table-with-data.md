---
title: Naplnění tabulku s daty
ms.prod: xamarin
ms.assetid: 6FE64DDF-1029-EB9B-6EEC-1C7DFDFDF3AF
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/22/2017
ms.openlocfilehash: c139b96adfc325e7c251f8093eab338ddf0c6337
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/04/2018
---
# <a name="populating-a-table-with-data"></a>Naplnění tabulku s daty

Přidání řádků, které mají `UITableView` potřebujete implementovat `UITableViewSource` podtřídami a přepsání volá metody, které zobrazení tabulky k naplnění sám sebe.

Tento průvodce obsahuje:

- Vytvoření podtřídy UITableViewSource
- Opakované použití buňky
- Přidání indexu
- Přidávání záhlaví a zápatí


<a name="Subclassing_UITableViewSource" />

## <a name="subclassing-uitableviewsource"></a>Vytvoření podtřídy UITableViewSource

A `UITableViewSource` podtřídami je přiřazen ke každému `UITableView`. Zobrazení tabulky dotazuje třída zdroje určit, jak k vykreslení samotné (pro příklad, kolik řádků, které jsou požadovány a výšku každý řádek, pokud se liší od výchozí nastavení). Co je nejdůležitější – zdroj poskytuje jednotlivých buněk zobrazení naplněný daty.

Požadovat, aby tabulku zobrazit data jenom dva povinné způsoby:

-   **RowsInSection** – návratový [ `nint` ](http://developer.xamarin.com/guides/cross-platform/macios/nativetypes/) počet celkový počet řádků dat by se měla zobrazovat v tabulce.
-   **GetCell** – návratový `UITableCellView` naplněný daty pro odpovídající řádek indexu předaný metodě.


Ukázkový soubor BasicTable **TableSource.cs** má nejjednodušší možné implementace `UITableViewSource`. V následující fragment kódu můžete zjistit, že přijímá pole řetězců zobrazíte v tabulce a vrátí výchozí styl buňky obsahující každý řetězec:

```csharp
public class TableSource : UITableViewSource {

        string[] TableItems;
        string CellIdentifier = "TableCell";

        public TableSource (string[] items)
        {
            TableItems = items;
        }

        public override nint RowsInSection (UITableView tableview, nint section)
        {
            return TableItems.Length;
        }

        public override UITableViewCell GetCell (UITableView tableView, NSIndexPath indexPath)
        {
            UITableViewCell cell = tableView.DequeueReusableCell (CellIdentifier);
            string item = TableItems[indexPath.Row];

            //---- if there are no cells to reuse, create a new one
            if (cell == null)
            { cell = new UITableViewCell (UITableViewCellStyle.Default, CellIdentifier); }

            cell.TextLabel.Text = item;

            return cell;
        }
}
```

A `UITableViewSource` můžete použít jinou datovou strukturu z pole jednoduchým řetězcem (jak je uvedeno v tomto příkladu) do seznamu <> nebo jiné kolekce. Implementace `UITableViewSource` metody izoluje tabulky z podkladová struktura data.

Pokud chcete použít tento podtřídami, vytvořit pole řetězců vytvořit zdroj pak přiřaďte ho do instance systému `UITableView`:

```csharp
public override void ViewDidLoad ()
{
    base.ViewDidLoad ();
    table = new UITableView(View.Bounds); // defaults to Plain style
    string[] tableItems = new string[] {"Vegetables","Fruits","Flower Buds","Legumes","Bulbs","Tubers"};
    table.Source = new TableSource(tableItems);
    Add (table);
}
```

Výsledná tabulka vypadá takto:

 [![](populating-a-table-with-data-images/image3.png "Ukázková tabulka systémem")](populating-a-table-with-data-images/image3.png#lightbox)

Většina tabulek povolit uživatelům touch řádek, vyberte ho a provádět další akce (například přehrávat skladbu nebo volání a obraťte se na zobrazení další obrazovky). Jak toho docílit, je několik věcí, které je potřeba udělat. Nejdříve vytvoříme AlertController pro zobrazení zprávy, když uživatel klikněte na řádek přidáním následujícího `RowSelected` metoda:

```csharp
public override void RowSelected (UITableView tableView, NSIndexPath indexPath)
{
    UIAlertController okAlertController = UIAlertController.Create ("Row Selected", tableItems[indexPath.Row], UIAlertControllerStyle.Alert);
    okAlertController.AddAction(UIAlertAction.Create("OK", UIAlertActionStyle.Default, null));
    ...

    tableView.DeselectRow (indexPath, true);
}
```

Dále vytvořte instanci Kontroleru zobrazení:

```csharp
HomeScreen owner;
```
Přidejte konstruktor ke třídě UITableViewSource, která přebírá řadič zobrazení jako parametr a uloží ho do pole:

```csharp
public TableSource (string[] items, HomeScreen owner)
{
    ...
    this.owner = owner;

}
```
Změňte metodu ViewDidLoad, kde se má vytvořit třídu UITableViewSource předat `this` odkaz:

```csharp
table.Source = new TableSource(tableItems, this);
```
Nakonec zpět na vaše `RowSelected` metoda, volání `PresentViewController` na pole v mezipaměti:

```csharp
public override void RowSelected (UITableView tableView, NSIndexPath indexPath)
{
    ...
    owner.PresentViewController (okAlertController, true, null);

    ...
}
```


Teď můžete uživatele touch řádek a zobrazí se výstraha:



 [![](populating-a-table-with-data-images/image4.png "Řádek vybranou výstrahu.")](populating-a-table-with-data-images/image4.png#lightbox)


## <a name="cell-reuse"></a>Opakované použití buňky

V tomto příkladu je pouze šest položek, takže není žádné buňky opakované použití požadované. Při zobrazení stovkami nebo tisíci řádků, ale bylo by plýtvání paměti k vytvoření stovkami nebo tisíci `UITableViewCell` objekty při jen několik vejít na obrazovce najednou.

Abyste předešli této situaci, když buňku dané zařízení zmizí z obrazovky, kterou jeho zobrazení je umístěn ve frontě pro opakované použití. Jako uživatel posune, v tabulce volá `GetCell` k vyžádání nové zobrazení zobrazíte – Pokud chcete znovu použít existující buňky, (který není aktuálně zobrazeného) jednoduše volání `DequeueReusableCell` metoda. Pokud je k dispozici pro opakované použití, který bude vrácen buňku, jinak vrátí hodnotu null a kódu musíte vytvořit novou instanci buňky.

Tento fragment kódu z příkladu ukazuje vzoru:

```csharp
// request a recycled cell to save memory
UITableViewCell cell = tableView.DequeueReusableCell (cellIdentifier);
// if there are no cells to reuse, create a new one
if (cell == null)
    cell = new UITableViewCell (UITableViewCellStyle.Default, cellIdentifier);
```

`cellIdentifier` Efektivně vytvoří samostatné fronty pro různé typy buňky. V tomto příkladu, že všechny buňky vypadat stejné tak pouze jeden pevně kódovaný identifikátor se používá. Kdyby různé typy buňky musí každý mají jiný identifikátor řetězec, když jsou vytvořeny instance i vyžádání z fronty opakované použití.

### <a name="cell-reuse-in-ios-6"></a>Opakované použití buňky v iOS 6 +

iOS 6 přidat vzor buňky opakované použití, podobně jako jeden Úvod s zobrazení kolekcí. I když existující opakované použití vzoru uvedené výše se pořád podporuje pro zpětné kompatibility, tento nový vzor je vhodnější jako nemusíte null kontrola na buňku.

Pomocí nové vzoru aplikace registruje buňky třídu nebo xib, který se má použít při volání buď `RegisterClassForCellReuse` nebo `RegisterNibForCellReuse` v konstruktoru kontroleru. Pak když dequeueing buňky v `GetCell` metoda, jednoduše volání `DequeueReusableCell` předávání identifikátor je zaregistrovaný pro třídu buňky nebo xib a cesta k indexu.

Například následující kód registruje třídu vlastní buňky v UITableViewController:

```csharp
public class MyTableViewController : UITableViewController
{
  static NSString MyCellId = new NSString ("MyCellId");

  public MyTableViewController ()
  {
    TableView.RegisterClassForCellReuse (typeof(MyCell), MyCellId);
  }
  ...
}
```

Pomocí třídy MyCell registrován, může být vyjmutou buňky v `GetCell` metodu `UITableViewSource` bez nutnosti velmi hodnotu null. Zkontrolujte, jak je uvedené níže:

```csharp
class MyTableSource : UITableViewSource
{
  public override UITableViewCell GetCell (UITableView tableView, NSIndexPath indexPath)
  {
    // if cell is not available in reuse pool, iOS will create one automatically
    // no need to do null check and create cell manually
    var cell = (MyCell) tableView.DequeueReusableCell (MyCellId, indexPath);

    // do whatever you need to with cell, such as assigning properties, etc.

    return cell;
  }
}
```

Mějte na paměti, při použití nové vzor opakované použití s třídou vlastní buňky, potřebujete implementovat konstruktor, který přebírá `IntPtr`, jak je znázorněno v následujícím fragmentu, v opačném případě jazyka Objective-C nebude možné vytvořit instanci třídy buňky:

```csharp
public class MyCell : UITableViewCell
{
  public MyCell (IntPtr p):base(p)
  {
  }
  ...
}
```

Vidíte příklady témat popsané výše v **BasicTable** ukázka propojené s v tomto článku.

<a name="Adding_an_Index" />

## <a name="adding-an-index"></a>Přidání indexu

Index umožňuje uživateli posuňte dlouhými seznamy, obvykle v abecedním pořadí i když můžete indexu ať kritériích nechcete. **BasicTableIndex** ukázka načte mnohem delší seznam položek ze souboru k předvedení index. Každá položka v indexu odpovídá 'oddíl' v tabulce.

 [![](populating-a-table-with-data-images/image5.png "Index zobrazení")](populating-a-table-with-data-images/image5.png#lightbox)

Pro podporu data za tabulky musí být seskupené, tak BasicTableIndex ukázka vytvoří oddíly `Dictionary<>` z pole řetězců pomocí první písmena jednotlivých položek jako slovník klíč:

```csharp
indexedTableItems = new Dictionary<string, List<string>>();
foreach (var t in items) {
    if (indexedTableItems.ContainsKey (t[0].ToString ())) {
        indexedTableItems[t[0].ToString ()].Add(t);
    } else {
        indexedTableItems.Add (t[0].ToString (), new List<string>() {t});
    }
}
keys = indexedTableItems.Keys.ToArray ();
```

`UITableViewSource` Podtřídami potom vyžaduje následující metody přidat nebo upravit tak, aby použít `Dictionary<>` :

-   **NumberOfSections** – tato metoda je volitelné, ve výchozím nastavení předpokládá tabulky jeden oddíl. Při zobrazení indexu tato metoda by měla vrátit počet položek v indexu (například 26 Pokud index obsahuje všechna písmena anglické abecedy).
-   **RowsInSection** – vrátí počet řádků v daném oddílu.
-   **SectionIndexTitles** – vrátí pole řetězce, které se použije k zobrazení index. Ukázkový kód vrátí pole písmena.


Aktualizované metody v ukázkovém souboru **BasicTableIndex/TableSource.cs** vypadat takto:

```csharp
public override nint NumberOfSections (UITableView tableView)
{
    return keys.Length;
}
public override nint RowsInSection (UITableView tableview, nint section)
{
    return indexedTableItems[keys[section]].Count;
}
public override string[] SectionIndexTitles (UITableView tableView)
{
    return keys;
}
```

Indexy jsou obecně používány pouze s styl prostý tabulky.


<a name="Adding_Headers_and_Footers" />

## <a name="adding-headers-and-footers"></a>Přidávání záhlaví a zápatí

Záhlaví a zápatí lze použít k vizuálně seskupení řádků v tabulce. Struktura dat vyžaduje je velmi podobný jako při přidávání indexu – `Dictionary<>` skutečně dobře funguje. Místo použití abecedy k seskupení buněk, v tomto příkladu seskupí zeleninu botanické typem.
Výstup vypadá takto:

 [![](populating-a-table-with-data-images/image6.png "Ukázka záhlaví a zápatí")](populating-a-table-with-data-images/image6.png#lightbox)

Chcete-li zobrazit záhlaví a zápatí `UITableViewSource` podtřídami vyžaduje tyto další metody:

-   **TitleForHeader** – vrátí text, který chcete použít jako záhlaví
-   **TitleForFooter** – vrátí text, který bude použit jako zápatí.


Aktualizované metody v ukázkovém souboru **BasicTableHeaderFooter/Code/TableSource.cs** vypadat takto:

```csharp
public override string TitleForHeader (UITableView tableView, nint section)
{
    return keys[section];
}
public override string TitleForFooter (UITableView tableView, nint section)
{
    return indexedTableItems[keys[section]].Count + " items";
}
```

Můžete přizpůsobit vzhled záhlaví a zápatí se zobrazením objektu, pomocí `GetViewForHeader` a `GetViewForFooter` metoda přepsání na `UITableViewSource`.


## <a name="related-links"></a>Související odkazy

- [WorkingWithTables (ukázka)](https://developer.xamarin.com/samples/monotouch/WorkingWithTables)
