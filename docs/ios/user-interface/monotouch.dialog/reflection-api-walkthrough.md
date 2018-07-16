---
title: Vytvoření aplikace Xamarin.iOS pomocí rozhraní Reflection API
description: Tento dokument popisuje rozhraní MonoTouch.Dialog založených na atributech Reflection API, která vytvoří podle třídy upravena pomocí atributů uživatelského rozhraní.
ms.prod: xamarin
ms.assetid: C0F923D2-300E-DB9D-F390-9FA71B22DFD6
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.openlocfilehash: a1b77f46410ef20892485a866221bab2c872e54c
ms.sourcegitcommit: cb80df345795989528e9df78eea8a5b45d45f308
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/14/2018
ms.locfileid: "39038469"
---
# <a name="creating-a-xamarinios-application-using-the-reflection-api"></a>Vytvoření aplikace Xamarin.iOS pomocí rozhraní Reflection API

Pro MT. D Reflection API umožňuje třídy bude upravena pomocí atributů této pro MT. D používá k vytvoření obrazovek automaticky. Rozhraní API reflexe obsahuje vazbu mezi tyto třídy a co se zobrazí na obrazovce. I když toto rozhraní API neposkytuje podrobné ovládací prvek, který nemá prvky rozhraní API, snižuje složitost podle automaticky vytvářela hierarchie elementů podle třídy dekorace.

## <a name="setting-up-mtd"></a>Nastavení pro MT. D

PRO MT. D je distribuován spolu s Xamarin.iOS. Ho Pokud chcete použít, klikněte pravým tlačítkem na **odkazy** uzel Xamarin.iOS projektu v sadě Visual Studio 2017 nebo Visual Studio pro Mac a přidejte odkaz na **MonoTouch.Dialog 1** sestavení. Pak přidejte `using MonoTouch.Dialog` příkazy ve zdrojovém kódu podle potřeby.

## <a name="getting-started-with-the-reflection-api"></a>Začínáme s rozhraním API reflexe

Pomocí rozhraní Reflection API je stejně jednoduché jako:

1.  Vytvoření třídy dekorován pro MT. Atributy D.
1.  Vytváření `BindingContext` instance, předejte instanci třídy výše. 
1.  Vytváření `DialogViewController` , předají se jí `BindingContext’s` `RootElement` . 


Pojďme se podívat na příklad, který ukazuje, jak použít rozhraní API reflexe. V tomto příkladu vytvoříme obrazovky pro zadávání dat jednoduché jak je znázorněno níže:

 [![](reflection-api-walkthrough-images/01-expense-entry.png "V tomto příkladu vytvoříme obrazovky pro zadávání dat jednoduché jak je znázorněno zde")](reflection-api-walkthrough-images/01-expense-entry.png#lightbox)

## <a name="creating-a-class-with-mtd-attributes"></a>Vytvoření třídy s pro MT. Atributy D

První věc, kterou potřebujeme k používání rozhraní API reflexe je třída upravena pomocí atributů. Tyto atributy budou používat pro MT. D interně k vytvoření objektů z rozhraní Elements API. Představte si třeba následující definici třídy:

```csharp
public class Expense
{
        [Section("Expense Entry")]

        [Entry("Enter expense name")]
        public string Name;
        
        [Section("Expense Details")]
  
        [Caption("Description")]
        [Entry]
        public string Details;
        
        [Checkbox]
        public bool IsApproved = true;
}
```

`SectionAttribute` Způsobí oddíly `UITableView` vytváří s řetězcovým argumentem použitých k naplnění v části záhlaví. Jakmile je deklarován oddíl, každé pole, která ji následuje se zahrne v této části, dokud jiný oddíl deklarován.
Typ prvku uživatelského rozhraní pro pole vytvořena, bude záviset na typu pole a pro MT. Upravení jeho atribut D.

Například `Name` je pole `string` a je doplněn `EntryAttribute`. Výsledkem je řádek přidán do tabulky s polem pro zadání textu a zadaný popisek. Podobně `IsApproved` pole je `bool` s `CheckboxAttribute`, což v tabulce Řádek s zaškrtávacího políčka napravo od buňky tabulky. PRO MT. D používá název pole automaticky přidá mezeru, se v tomto případě vytvořit popisek, protože není zadaný v atributu.

## <a name="adding-the-bindingcontext"></a>Přidání BindingContext

Použít `Expense` třídy, je nutné vytvořit `BindingContext`. A `BindingContext` je třída, která vytvoří vazbu data ze třídy s atributy k vytvoření hierarchie prvků. Pokud chcete jeden vytvořit, můžeme jednoduše vytvořit její instanci a předat konstruktoru instance třídy s atributy.

Například, chcete-li přidat uživatelské rozhraní, které jsme deklarovány pomocí atributu `Expense` třídy, zahrňte následující kód `FinishedLaunching` metodu `AppDelegate`:

```csharp
var expense = new Expense ();
var bctx = new BindingContext (null, expense, "Create a task");
```

Pak vše, musíme použít pro vytvoření uživatelského rozhraní je přidání `BindingContext` k `DialogViewController` a nastavte ji jako `RootViewController` okna, jak je znázorněno níže:

```csharp
UIWindow window;

public override bool FinishedLaunching (UIApplication app, 
        NSDictionary options)
{
   
        window = new UIWindow (UIScreen.MainScreen.Bounds);
            
        var expense = new Expense ();
        var bctx = new BindingContext (null, expense, "Create a task");
        var dvc = new DialogViewController (bctx.Root);
            
        window.RootViewController = dvc;
        window.MakeKeyAndVisible ();
            
        return true;
}
```

Spuštění aplikace nyní výsledky na obrazovce uvedené nahoře se zobrazuje.

### <a name="adding-a-uinavigationcontroller"></a>Přidání UINavigationController

Všimněte si však, že název "Vytvoření úkolu", který jsme předán `BindingContext` se nezobrazí. Je to proto, `DialogViewController` není součástí `UINavigatonController`. Změníme kód pro přidání `UINavigationController` jako v okně `RootViewController,` a přidejte `DialogViewController` jako kořen `UINavigationController` jak je znázorněno níže:

```csharp
nav = new UINavigationController(dvc);
window.RootViewController = nav;
```

Nyní když jsme aplikaci spustit, název se zobrazí v `UINavigationController’s` navigační panel jako na snímku obrazovky níže ukazuje:

 [![](reflection-api-walkthrough-images/02-create-task.png "Teď když spustíme aplikaci, název se zobrazí v navigačním panelu UINavigationControllers")](reflection-api-walkthrough-images/02-create-task.png#lightbox)

Zahrnutím `UINavigationController`, jsme teď můžete využít výhod dalších funkcí pro mt. D, pro které je nezbytné navigace. Například můžeme přidat výčet, který `Expense` třídy definovat kategorie pro výdajů a pro MT. D, bude automaticky vytvořit obrazovku pro výběr. Abychom si předvedli, upravte `Expense` třídy, aby obsahoval `ExpenseCategory` pole následujícím způsobem:

```csharp
public enum Category
{
        Travel,
        Lodging,
        Books
}
        
public class Expense
{
        …

        [Caption("Category")]
        public Category ExpenseCategory;
}
```

Spuštění aplikace nyní vrátí nový řádek v tabulce pro kategorii, jak je znázorněno:

 [![](reflection-api-walkthrough-images/03-set-details.png "Spuštění aplikace nyní výsledkem nový řádek v tabulce u kategorie, jak je znázorněno")](reflection-api-walkthrough-images/03-set-details.png#lightbox)

Výběr řádku vrátí aplikaci přejdete do nové obrazovky s řádky odpovídající výčtu, jak je znázorněno níže:

 [![](reflection-api-walkthrough-images/04-set-category.png "Výběr řádku v aplikaci přejdete do nové obrazovky s řádky odpovídající výčtu výsledků")](reflection-api-walkthrough-images/04-set-category.png#lightbox)

 <a name="Summary" />


## <a name="summary"></a>Souhrn

V tomto článku jsou uvedené návod k rozhraní API reflexe. Můžeme vám ukázal, jak přidat atributy na třídu řídit, co se zobrazí. Také jsme probírali použití `BindingContext` vázat data ze třídy do hierarchie element, který je vytvořen, jak se dá použít pro MT. D s `UINavigationController`.


## <a name="related-links"></a>Související odkazy

- [MTDReflectionWalkthrough (ukázka)](https://developer.xamarin.com/samples/MTDReflectionWalkthrough/)
- [Záznam dění na monitoru - Miguela de Icaza vytvoří přihlašovací obrazovka aplikace iOS s MonoTouch.Dialog](http://youtu.be/3butqB1EG0c)
- [Záznam dění na monitoru – snadno vytvářet iOS uživatelské rozhraní s MonoTouch.Dialog](http://youtu.be/j7OC5r8ZkYg)
- [Úvod do dialogového okna MonoTouch](~/ios/user-interface/monotouch.dialog/index.md)
- [Návod prvky rozhraní API](~/ios/user-interface/monotouch.dialog/elements-api-walkthrough.md)
- [Návod elementu JSON](~/ios/user-interface/monotouch.dialog/monotouch.dialog-json-markup.md)
- [Dialogové okno MonoTouch na Githubu](https://github.com/migueldeicaza/MonoTouch.Dialog)
- [TweetStation aplikace](https://github.com/migueldeicaza/TweetStation)
- [Referenční třída UITableViewController](http://developer.apple.com/library/ios/#DOCUMENTATION/UIKit/Reference/UITableViewController_Class/Reference/Reference.html)
- [Referenční třída UINavigationController](http://developer.apple.com/library/ios/#documentation/UIKit/Reference/UINavigationController_Class/Reference/Reference.html)
