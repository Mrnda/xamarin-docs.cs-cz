---
title: "Práce s tabulkami v iOS návrháře"
description: "V předchozí části jsme prozkoumali vývoj s použitím tabulky. V tomto části páté a finální jsme bude agregovat toho, co jsme naučili dosavadní a vytvořit základní aplikaci případě seznamu pomocí scénáře."
ms.topic: article
ms.prod: xamarin
ms.assetid: D8416E10-481A-0B6E-4081-B146E6358004
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/22/2017
ms.openlocfilehash: c59ddde44b0e47122865c55a7964707f106d2691
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 02/27/2018
---
# <a name="working-with-tables-in-the-ios-designer"></a>Práce s tabulkami v iOS návrháře

Scénářů jsou WYSIWYG způsob, jak vytvářet aplikace pro iOS a jsou podporovány v sadě Visual Studio na Mac a Windows. Další informace o scénářů, najdete v části [Úvod k scénářů](~/ios/user-interface/storyboards/index.md) dokumentu. Scénářů taky umožňují upravit rozložení buněk *v* tabulky, který usnadňuje vývoj s tabulkami a buněk

Při konfiguraci vlastností zobrazení tabulky v iOS Designer, existují dva typy obsahu buňky, můžete si vybrat z: **dynamické** nebo **statické** obsah prototypu.

<a name="Prototype_Content" />

## <a name="dynamic-prototype-content"></a>Obsah dynamické prototypu

A `UITableView` s prototypu obsah obvykle slouží k zobrazení seznamu dat kde buňky prototypu (nebo buněk, jako je můžete definovat více než jeden) jsou znovu použít pro každou položku v seznamu. Buněk nebudete muset vytvořit instanci, jsou získány v `GetView` metoda voláním `DequeueReusableCell` metodu jeho `UITableViewSource`.

 <a name="Static_Content" />


## <a name="static-content"></a>Statický obsah

`UITableView`s pomocí funkce statický obsah umožňuje tabulky třeba navrhnout na návrhovou plochu vpravo. Buňky můžete přetáhnout do tabulky a přizpůsobit změnou vlastností a přidání ovládacích prvků.

 <a name="Creating_a_Storyboard-driven_app" />


## <a name="creating-a-storyboard-driven-app"></a>Vytvoření aplikace řízené scénáře

Příklad StoryboardTable obsahuje jednoduchý seznam podrobnosti aplikaci, která používá oba typy UITableView v scénáře. Zbytek této části popisuje, jak sestavit příklad seznamu malé úkolů, která bude po dokončení vypadat například takto:

 [ ![Příklad obrazovky](creating-tables-in-a-storyboard-images/image13a.png)](creating-tables-in-a-storyboard-images/image13a.png)

Uživatelské rozhraní bude vytvořené s nástroji scénáře a obě obrazovky bude používat UITableView. Hlavní obrazovky používá *prototypu obsah* rozložení řádek a podrobností obrazovky používá *statický obsah* vytvořit formulář zadávání dat pomocí vlastní buňky rozložení.

## <a name="walkthrough"></a>Návod

Vytvořte nové řešení v sadě Visual Studio pomocí **(vytvořit) nový projekt... > jednoho zobrazení App(C#)**a pojmenujte ji _StoryboardTables_.

 [ ![Vytvořit dialogové okno Nový projekt](creating-tables-in-a-storyboard-images/npd.png)](creating-tables-in-a-storyboard-images/npd.png)

Otevře se řešení s některé soubory C# a `Main.storyboard` souborů, které jsou již vytvořeny. Dvakrát klikněte `Main.storyboard` soubor otevřete v iOS Designer.

<a name="Modifying_the_Storyboard" />

## <a name="modifying-the-storyboard"></a>Úprava scénáři

Scénáři bude upravit v tři kroky:

-  První, rozložení požadované zobrazení řadičů a nastavte jejich vlastnosti.
-  Poté vytvořte uživatelské rozhraní přetažením objektů do zobrazení
-  Nakonec přidejte požadovanou třídu UIKit do jednotlivých zobrazení a pojmenujte různé ovládací prvky, může být odkazováno v kódu.


Po dokončení scénáři lze přidat kód pro zajištění spolupráce všech.

<a name="Layout_The_View_Controllers" />

### <a name="layout-the-view-controllers"></a>Rozložení řadiče zobrazení

První změna do scénáře je odstranění existujícího podrobné zobrazení a nahraďte ho UITableViewController. Postupujte podle těchto kroků:

1.  Vyberte na panelu v dolní části řadiče zobrazení a odstraňte ji.
2.  Přetáhněte **navigační řadič** a **tabulky řadiče zobrazení** na scénář z panelu nástrojů. 
3.  Vytvoření segue ze zobrazení řadiče kořenové do druhý řadiče zobrazení tabulky, který byl právě přidali. K vytvoření segue, řízení + přetažení *z buněk podrobností* pro nově přidaného UITableViewController. Zvolte možnost **zobrazit*** pod **Segue výběr**. 
4.  Vyberte nové segue jste vytvořili a přidělte mu identifikátor k odkazu na to segue v kódu. Klikněte na segue a zadejte `TaskSegue` pro **identifikátor** v **vlastnosti Pad**, podobné výjimky:    
  [ ![Pojmenování segue panelu vlastnost](creating-tables-in-a-storyboard-images/image16a-sml.png)](creating-tables-in-a-storyboard-images/image16a.png) 

5. V dalším kroku nakonfigurujte dvě zobrazení tabulek tak, že je vyberete a pomocí vlastnosti odsazení. Je nutné vybrat zobrazení a není View Controller – Osnova dokumentu můžete pomoci s výběrem.

6.  Změnit kořenový řadič zobrazení jako **obsahu: dynamické prototypy** (zobrazení na návrhovou plochu, která budou označené jako **prototypu obsahu** ):

    [ ![Vlastnost obsahu na dynamické prototypů](creating-tables-in-a-storyboard-images/image17a.png)](creating-tables-in-a-storyboard-images/image17a.png)

7.  Změnit nové **UITableViewController** být **obsahu: statické buněk**. 


8. Nové UITableViewController musí mít jeho název třídy nebo identifikátor nastavit. Vyberte typ a View Controller _TaskDetailViewController_ pro **– třída** v **vlastnosti Pad** – tím se vytvoří nový `TaskDetailViewController.cs` souboru v řešení Odsazení. Zadejte **StoryboardID** jako _podrobností_, jak je popsáno v následujícím příkladu. Bude se používat novější načíst toto zobrazení v kódu jazyka C#:  

    [ ![Nastavení ID scénáře](creating-tables-in-a-storyboard-images/image18a.png)](creating-tables-in-a-storyboard-images/image18a.png)

9. Scénáře návrhovou plochu, která by teď měl vypadat takto (název položky v zobrazení řadiče kořenové navigaci byla změněna na "Případě tabule"):

    [ ![Návrhové plochy](creating-tables-in-a-storyboard-images/image20a-sml.png)](creating-tables-in-a-storyboard-images/image20a.png)  



<a name="Create_the_UI" />

### <a name="create-the-ui"></a>Vytvoření uživatelského rozhraní

Teď, zobrazení a segues jsou nakonfigurované, prvky uživatelského rozhraní je třeba přidat.

#### <a name="root-view-controller"></a>Kořenový řadič zobrazení

Nejdřív vyberte prototypu buňky v Kontroleru zobrazení hlavní a nastavte **identifikátor** jako _taskcell_, jak je uvedeno dále. Bude se používat novější v kódu načíst instanci této UITableViewCell:

 [ ![nastavení identifikátor buňky](creating-tables-in-a-storyboard-images/image22a-sml.png)](creating-tables-in-a-storyboard-images/image22a.png)

Potom budete muset vytvořit tlačítko, které přidá nové úkoly, jak je uvedeno dále:

[ ![tlačítko položku na navigačním panelu panelu](creating-tables-in-a-storyboard-images/image23-sml.png)](creating-tables-in-a-storyboard-images/image23.png)

Postupujte takto: 

-  Přetáhněte **položka tlačítka panelu** z panelu nástrojů _pravé straně navigačního panelu_.
-  V **vlastnosti Pad**v části **položka tlačítka panelu** vyberte **identifikátor: Přidejte** (Chcete-li  *+*  plus tlačítko). 
-  Pojmenujte ho tak, aby je možné zjistit v kódu v pozdější fázi. Všimněte si, že budete muset poskytnout název třídy řadiče kořenové zobrazení (například **ItemViewController**) a umožní vám nastavit název položky tlačítko panelu.


#### <a name="taskdetail-view-controller"></a>Zobrazení TaskDetail řadiče

Zobrazení podrobností vyžaduje mnoho další práci. Zobrazení buněk tabulky je třeba přetáhli zobrazení a potom naplněný popisky, tlačítka a zobrazení textu. Následující snímek obrazovky ukazuje dokončení uživatelského rozhraní se dvě části. Jeden oddíl má tři buněk, tři popisky, dvou textových polí a jedno přepnout, zatímco druhý oddíl obsahuje jednu buňku s dvě tlačítka:

 [ ![rozložení zobrazení podrobností](creating-tables-in-a-storyboard-images/image24a-sml.png)](creating-tables-in-a-storyboard-images/image24a.png)

Postup pro sestavení dokončení rozložení jsou:

Vyberte zobrazení, tabulky a otevřete **Pad vlastnost**. Aktualizujte následující vlastnosti:

-  **Části**: _2_ 
-  **Styl**: _seskupené_
-  **Oddělovač**: _None_
-  **Výběr**: _žádná výběru_

Vyberte v horní části a v části **vlastnosti > části zobrazení tabulky** změnit **řádky** k _3_, jak je uvedeno dále:


 [ ![nastavení v horní části na tři řádky](creating-tables-in-a-storyboard-images/image29-sml.png)](creating-tables-in-a-storyboard-images/image29.png)

Pro každou buňku otevřete **Pad vlastnosti** a nastavte:

-  **Styl**: _vlastní_
-  **Identifikátor**: Zvolte jedinečný identifikátor pro každou buňku (např. "_název_","_poznámky_","_provádí_").
-  Přetáhněte požadovaných ovládacích prvků k vytvoření rozložení ukazuje snímek obrazovky (umístit **UILabel**, **UITextField** a **UISwitch** na buňky. správný a nastavte popisků správně tj. Název, poznámky a Hotovo).


V druhé části nastavit **řádky** k _1_ a získat popisovač změny velikosti dolní buňky zajistit vyšší.

-  **Nastavit identifikátor**: na jedinečnou hodnotu (např. "save"). 
-  **Nastavení pozadí**: _vymazat barva_ .
-  Přetáhněte dvě tlačítka na buňky a nastavte jejich názvy správně (tj. _Uložit_ a _odstranit_), jak je uvedeno dále:

   [ ![nastavení dvě tlačítka v dolní části](creating-tables-in-a-storyboard-images/image30-sml.png)](creating-tables-in-a-storyboard-images/image30.png)

V tomto okamžiku můžete také nastavit omezení u buněk a ovládací prvky zajistit přizpůsobivé rozložení.

### <a name="adding-uikit-class-and-naming-controls"></a>Přidání třídy UIKit a názvy ovládacích prvků

Existují několik závěrečné kroky při vytváření naše scénáře. Nejprve jsme musí pojmenujte všechny naše ovládacích prvků v části **Identity > název** takže je můžete použít v kódu později. Název tyto následujícím způsobem:

-  **Title UITextField** : _TitleText_
-  **Poznámky k UITextField** : _NotesText_
-  **UISwitch** : _DoneSwitch_
-  **Odstranit UIButton** : _DeleteButton_
-  **Uložit UIButton** : _SaveButton_


<a name="Adding_Code" />

## <a name="adding-code"></a>Přidání kódu

Zbývající práce bude provedeno v sadě Visual Studio na Mac nebo systém Windows pomocí jazyka C#. Všimněte si, že názvy vlastností, které jsou použity v kódu odrážejí hodnoty nastavení v Průvodci výše.

Chceme, nejprve vytvořit `Chores` třídy, která bude poskytovat způsob, jak získat a nastavit hodnotu ID, názvu, poznámky a Hotovo Boolean, takže používáme tyto hodnoty v celé aplikaci.

Ve vašem `Chores` třídy přidejte následující kód:

```csharp
public class Chores {
    public int Id { get; set; }
    public string Name { get; set; }
    public string Notes { get; set; }
    public bool Done { get; set; }
  }
```

Dále vytvořte `RootTableSource` třídu, která dědí z `UITableViewSource`. 

Rozdíl mezi touto a zobrazení tabulky bez scénáře je, že `GetView` metoda nepotřebuje k vytváření instancí žádné buňky – `theDequeueReusableCell` metoda vždy vrátí instanci buňky prototypu (s odpovídajícím identifikátorem).

Následující kód je z `RootTableSource.cs` souboru:

```csharp
public class RootTableSource : UITableViewSource
{
// there is NO database or storage of Tasks in this example, just an in-memory List<>
Chores[] tableItems;
string cellIdentifier = "taskcell"; // set in the Storyboard

    public RootTableSource(Chores[] items)
    {
        tableItems = items;
    }

public override nint RowsInSection(UITableView tableview, nint section)
{
  return tableItems.Length;
}

public override UITableViewCell GetCell(UITableView tableView, NSIndexPath indexPath)
{
  // in a Storyboard, Dequeue will ALWAYS return a cell, 
  var cell = tableView.DequeueReusableCell(cellIdentifier);
  // now set the properties as normal
  cell.TextLabel.Text = tableItems[indexPath.Row].Name;
  if (tableItems[indexPath.Row].Done)
    cell.Accessory = UITableViewCellAccessory.Checkmark;
  else
    cell.Accessory = UITableViewCellAccessory.None;
  return cell;
}
public Chores GetItem(int id)
{
  return tableItems[id];
}
```

Použít `RootTableSource` třídy, vytvoří novou kolekci v `ItemViewController`pro konstruktor:

```csharp
chores = new List<Chore> {
      new Chore {Name="Groceries", Notes="Buy bread, cheese, apples", Done=false},
      new Chore {Name="Devices", Notes="Buy Nexus, Galaxy, Droid", Done=false}
    };
```

V `ViewWillAppear` předat kolekce ke zdroji a přiřaďte do tabulky zobrazení:

```csharp
public override void ViewWillAppear(bool animated)
{
    base.ViewWillAppear(animated);

    TableView.Source = new RootTableSource(chores.ToArray());
}
```

Pokud spouštíte aplikaci nyní hlavní obrazovky bude nyní načíst a zobrazit seznam dvě úlohy. Když je úloha dotýkal segue definované ve scénáři způsobí, že zobrazení podrobností obrazovky, ale v tuto chvíli se nebudou zobrazovat žádná data.

' Odeslat parametr ' v segue, přepsat `PrepareForSegue` metoda a nastavit vlastnosti u `DestinationViewController` ( `TaskDetailViewController` v tomto příkladu). Budou mít po vytvoření instance třídy Kontroleru zobrazení cílové, ale není ještě zobrazí uživateli, – to znamená, můžete nastavit vlastnosti u třídy ale ne upravovat všechny ovládací prvky uživatelského rozhraní:

```csharp
public override void PrepareForSegue (UIStoryboardSegue segue, NSObject sender)
    {
      if (segue.Identifier == "TaskSegue") { // set in Storyboard
        var navctlr = segue.DestinationViewController as TaskDetailViewController;
        if (navctlr != null) {
          var source = TableView.Source as RootTableSource;
          var rowPath = TableView.IndexPathForSelectedRow;
          var item = source.GetItem(rowPath.Row);
          navctlr.SetTask (this, item); // to be defined on the TaskDetailViewController
        }
      }
    }
```

V `TaskDetailViewController` `SetTask` metoda přiřadí jeho parametry vlastnosti, může být odkazováno v ViewWillAppear. Vlastnosti ovládacích prvků nelze upravovat v `SetTask` protože neexistuje při `PrepareForSegue` se označuje jako:

```csharp
Chore currentTask {get;set;}
    public ItemViewController Delegate {get;set;} // will be used to Save, Delete later

public override void ViewWillAppear (bool animated)
    {
      base.ViewWillAppear (animated);
      TitleText.Text = currentTask.Name;
      NotesText.Text = currentTask.Notes;
      DoneSwitch.On = currentTask.Done;
    }

    // this will be called before the view is displayed
    public void SetTask (ItemViewController d, Chore task) {
      Delegate = d;
      currentTask = task;
    }
```

Segue bude nyní otevřete na obrazovce podrobnosti a zobrazit informace o vybraných úloh. Bohužel neexistuje žádná implementace pro **Uložit** a **odstranit** tlačítka. Před implementací tlačítek, přidejte tyto metody do **ItemViewController.cs** aktualizovat základní data a zavřete obrazovce podrobností:

```csharp
public void SaveTask(Chores chore)
{
  var oldTask = chores.Find(t => t.Id == chore.Id);
        NavigationController.PopViewController(true);
}

public void DeleteTask(Chores chore)
{
  var oldTask = chores.Find(t => t.Id == chore.Id);
  chores.Remove(oldTask);
        NavigationController.PopViewController(true);
}
```

Potom budete muset tlačítko Přidat `TouchUpInside` obslužná rutina události `ViewDidLoad` metodu **TaskDetailViewController.cs**. `Delegate` Vlastnost odkaz na `ItemViewController` byla vytvořena speciálně, takže můžete říkáme `SaveTask` a `DeleteTask`, který v rámci jejich operace zavřete toto zobrazení:

```csharp
SaveButton.TouchUpInside += (sender, e) => {
        currentTask.Name = TitleText.Text;
        currentTask.Notes = NotesText.Text;
        currentTask.Done = DoneSwitch.On;
        Delegate.SaveTask(currentTask);
      };

DeleteButton.TouchUpInside += (sender, e) => Delegate.DeleteTask(currentTask);
```

Poslední zbývající část funkce k sestavení je vytvoření nové úlohy. V **ItemViewController.cs** přidejte metodu, která vytvoří nové úlohy a otevře zobrazení podrobností. K vytváření instancí zobrazení z scénáře použití `InstantiateViewController` metoda s `Identifier` pro tento zobrazení – v tomto příkladu, které se bude 'Podrobnosti':

```csharp
public void CreateTask () 
    {
      // first, add the task to the underlying data
      var newId = chores[chores.Count - 1].Id + 1;
      var newChore = new Chore{Id = newId};
      chores.Add (newChore);

      // then open the detail view to edit it
      var detail = Storyboard.InstantiateViewController("detail") as TaskDetailViewController;
      detail.SetTask (this, newChore);
      NavigationController.PushViewController (detail, true);
    }
```

Nakonec propojit tlačítko na navigačním panelu v **ItemViewController.cs**na `ViewDidLoad` metoda k volání:

```csharp
AddButton.Clicked += (sender, e) => CreateTask ();
```

Která se dokončí příklad scénáře – dokončení aplikace vypadá takto:

[ ![Dokončení aplikace](creating-tables-in-a-storyboard-images/image28a.png)](creating-tables-in-a-storyboard-images/image28a.png)

Příklad ukazuje:

-  Vytvoření tabulky s prototypu obsahu kde jsou definovány buněk pro nové použití zobrazíte seznam data. 
-  Vytvoření tabulky pomocí funkce statický obsah můžete vytvořit vstupní formulář. To zahrnuty změna styl tabulky a přidání oddílů, buněk a ovládacích prvků uživatelského rozhraní. 
-  Postup vytvoření segue a přepsat `PrepareForSegue` metoda oznámit cílové zobrazení všech parametrů vyžaduje. 
-  Načítání zobrazení storyboard přímo s `Storyboard.InstantiateViewController` metoda.



## <a name="related-links"></a>Související odkazy

- [StoryboardTable (ukázka)](https://developer.xamarin.com/samples/monotouch/StoryboardTable/)
- [Úvod do scénářů](~/ios/user-interface/storyboards/index.md)
