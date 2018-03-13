---
title: "Návod: Vytvoření aplikace pomocí rozhraní API reflexe"
description: "Kromě rozhraní API elementy MonoTouch.Dialog (strojový překladů. D) také zahrnuje rozhraní API reflexe založená na atributu. Rozhraní API reflexe umožňuje vytváření obrazovky s strojový překladů. D stejně snadná jako stavební třídy s atributy. Tento článek obsahuje procházení prostřednictvím znázorňující postup vytvoření aplikace pomocí rozhraní API reflexe."
ms.topic: article
ms.prod: xamarin
ms.assetid: C0F923D2-300E-DB9D-F390-9FA71B22DFD6
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.openlocfilehash: ec5ca2883c6e109a67ee8a4ecb25fe938d0df4ec
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 03/09/2018
---
# <a name="walkthrough-creating-an-application-using-the-reflection-api"></a>Návod: Vytvoření aplikace pomocí rozhraní API reflexe

_Kromě rozhraní API elementy MonoTouch.Dialog (strojový překladů. D) také zahrnuje rozhraní API reflexe založená na atributu. Rozhraní API reflexe umožňuje vytváření obrazovky s strojový překladů. D stejně snadná jako stavební třídy s atributy. Tento článek obsahuje procházení prostřednictvím znázorňující postup vytvoření aplikace pomocí rozhraní API reflexe._


Strojový překladů. Rozhraní API reflexe D umožňuje třídy být upraven pomocí atributů této strojový překladů. D používá k vytvoření obrazovky automaticky. Rozhraní API reflexe poskytuje vazbu mezi tyto třídy a co se zobrazí na obrazovce. Ačkoli toto rozhraní API neobsahuje podrobné ovládací prvek, který nemá elementy rozhraní API, snižuje složitost podle budovy automaticky se element hierarchii podle třídy decoration.

 <a name="Getting_Started_with_the_Reflection_API" />


## <a name="getting-started-with-the-reflection-api"></a>Začínáme s reflexí rozhraní API

Pomocí rozhraní API reflexe je stejně jednoduché jako:

1.  Vytvoření třídy označených pomocí strojový překladů. Atributy D.
1.  Vytváření `BindingContext` instance, předání instance třídy výše. 
1.  Vytváření `DialogViewController` , předání `BindingContext’s` `RootElement` . 


Podívejme se na příklad na ukazují, jak používat rozhraní API reflexe. V tomto příkladu využijeme záznam obrazovky jednoduché datové, jak je uvedeno níže:

 [![](reflection-api-walkthrough-images/01-expense-entry.png "V tomto příkladu jsme jak je vidět tady budete vytvářet obrazovky jednoduché datové položky")](reflection-api-walkthrough-images/01-expense-entry.png#lightbox)

 <a name="Creating_a_Class_with_MT.D_Attributes" />


## <a name="creating-a-class-with-mtd-attributes"></a>Vytvoření třídy s strojový překladů. Atributy D

První věc, kterou je potřeba použít rozhraní API reflexe je třída dekorované s atributy. Tyto atributy se použije v strojový překladů. D interně k vytvoření objekty z rozhraní API elementy. Zvažte například následující definice třídy:

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

`SectionAttribute` Bude mít za následek části `UITableView` vytváří s argumentem řetězec se používá k naplnění hlavičky v části. Jakmile je deklarovaná oddíl, každé pole, která následuje ho budou zahrnuty v této části, dokud je deklarovaná jinou část.
Typ element uživatelského rozhraní pro pole vytvořena, bude záviset na typu pole a strojový překladů. Atribut D architekturu ho.

Například `Name` pole `string` a je upravena pomocí `EntryAttribute`. Výsledkem řádek přidávané do tabulky s textového pole a zadaný popisek. Podobně `IsApproved` pole `bool` s `CheckboxAttribute`, což v tabulce Řádek s zaškrtávací políčko je na pravé straně buňky tabulky. STROJOVÝ PŘEKLADŮ. D používá název pole automaticky přidat do prostoru, se v takovém případě vytvořit popisek, protože není zadaný v atributu.

 <a name="Adding_the_BindingContext" />


## <a name="adding-the-bindingcontext"></a>Přidávání vazby

Použít `Expense` třída, musíme vytvořit `BindingContext`. A `BindingContext` je třída, která vytvoří vazbu data z s atributy třídy vytvořit hierarchii elementů. Pokud chcete vytvořit, jsme jednoduše vytvoří instanci a předat instanci s atributy třídy konstruktoru.

Například přidejte uživatelské rozhraní, které jsme deklarováno s použitím atribut `Expense` třídy, zahrnují následující kód v `FinishedLaunching` metodu `AppDelegate`:

```csharp
var expense = new Expense ();
var bctx = new BindingContext (null, expense, "Create a task");
```

Pak se musí udělat k vytvoření uživatelského rozhraní je přidat `BindingContext` k `DialogViewController` a nastavte ji jako `RootViewController` okna, jak je uvedeno níže:

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

Spuštění aplikace nyní výsledky na obrazovce uvedené výše se zobrazuje.

 <a name="Adding_a_UINavigationController" />


### <a name="adding-a-uinavigationcontroller"></a>Přidání UINavigationController

Ale Všimněte si, že název "vytvořit úlohu", jsme předaný `BindingContext` se nezobrazí. Důvodem je, že `DialogViewController` nejsou součástí `UINavigatonController`. Nyní změníme kódu k přidání `UINavigationController` jako okna `RootViewController,` a přidejte `DialogViewController` jako kořen `UINavigationController` jak je uvedeno níže:

```csharp
nav = new UINavigationController(dvc);
window.RootViewController = nav;
```

Nyní když jsme aplikaci spustit, název se zobrazí v `UINavigationController’s` navigační panel jako na snímku obrazovky níže znázorňuje:

 [![](reflection-api-walkthrough-images/02-create-task.png "Teď když jsme aplikaci spustit, název se zobrazí v navigačním panelu UINavigationControllers")](reflection-api-walkthrough-images/02-create-task.png#lightbox)

Zahrnutím `UINavigationController`, jsme teď můžete využít výhod dalších funkcí strojový překladů. D, pro které je nutné navigace. Například nyní můžete přidat výčet k `Expense` třída zadat kategorii výdajů a strojový překladů. D automaticky vytvoří obrazovku pro výběr. K předvedení, změnit `Expense` třídy patří `ExpenseCategory` pole následujícím způsobem:

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

Spuštění aplikace teď výsledkem nový řádek v tabulce pro kategorii, jak je znázorněno:

 [![](reflection-api-walkthrough-images/03-set-details.png "Spuštění aplikace teď výsledkem nový řádek v tabulce pro kategorii, jak je znázorněno")](reflection-api-walkthrough-images/03-set-details.png#lightbox)

Výběr řádku výsledkem aplikace přejdete na nové obrazovce s řádky odpovídající výčtu, jak je uvedeno níže:

 [![](reflection-api-walkthrough-images/04-set-category.png "Výběr řádku výsledkem aplikace přejdete na nové obrazovce s řádky odpovídající výčtu")](reflection-api-walkthrough-images/04-set-category.png#lightbox)

 <a name="Summary" />


## <a name="summary"></a>Souhrn

V tomto článku jsou uvedené návod rozhraní API reflexe. Můžeme vám ukázal, jak k přidání atributů do třídy k řízení, co se zobrazí. Také jsme probrali použití `BindingContext` vázat data ze třídy do hierarchie element, který je vytvořen, a také jak použití strojový překladů. D s `UINavigationController`.


## <a name="related-links"></a>Související odkazy

- [MTDReflectionWalkthrough (ukázka)](https://developer.xamarin.com/samples/MTDReflectionWalkthrough/)
- [Záznam dění na monitoru - Miguel de Icaza vytvoří obrazovka pro přihlášení iOS s MonoTouch.Dialog](http://youtu.be/3butqB1EG0c)
- [Záznam dění na monitoru - snadno vytvářet iOS uživatelského rozhraní s MonoTouch.Dialog](http://youtu.be/j7OC5r8ZkYg)
- [Úvod do dialogového okna MonoTouch](~/ios/user-interface/monotouch.dialog/index.md)
- [Prvky rozhraní API návod](~/ios/user-interface/monotouch.dialog/elements-api-walkthrough.md)
- [Návod Element JSON](~/ios/user-interface/monotouch.dialog/monotouch.dialog-json-markup.md)
- [Dialogové okno MonoTouch na Githubu](https://github.com/migueldeicaza/MonoTouch.Dialog)
- [TweetStation aplikace](https://github.com/migueldeicaza/TweetStation)
- [Odkaz na UITableViewController – třída](http://developer.apple.com/library/ios/#DOCUMENTATION/UIKit/Reference/UITableViewController_Class/Reference/Reference.html)
- [Odkaz na UINavigationController – třída](http://developer.apple.com/library/ios/#documentation/UIKit/Reference/UINavigationController_Class/Reference/Reference.html)
