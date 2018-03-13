---
title: "Datové vazby a klíč hodnota kódování"
description: "Tento článek popisuje použití kódování a klíč hodnota sledování povolit pro datovou vazbu k elementům uživatelského rozhraní v Xcode na rozhraní tvůrce klíč hodnota."
ms.topic: article
ms.prod: xamarin
ms.assetid: 72594395-0737-4894-8819-3E1802864BE7
ms.technology: xamarin-mac
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/14/2017
ms.openlocfilehash: b7ffd069a8c99c2cdfd0ecb58fe7ef762e5a46f3
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 03/09/2018
---
# <a name="data-binding-and-key-value-coding"></a>Datové vazby a klíč hodnota kódování

_Tento článek popisuje použití kódování a klíč hodnota sledování povolit pro datovou vazbu k elementům uživatelského rozhraní v Xcode na rozhraní tvůrce klíč hodnota._

## <a name="overview"></a>Přehled

Při práci s C# a rozhraní .NET v aplikaci Xamarin.Mac, máte přístup k stejný klíč hodnota kódování a techniky vazby dat, vývojář pracující *jazyka Objective-C* a *Xcode* nepodporuje. Protože Xamarin.Mac integruje přímo s Xcode, můžete na Xcode _rozhraní tvůrce_ k vytvoření vazby dat s prvky uživatelského rozhraní místo psaní kódu.

Pomocí kódování a datové vazby v aplikaci Xamarin.Mac techniky klíč hodnota, může výrazně snížit množství kód, který máte k zápisu a udržovat naplnit a pracovat s prvky uživatelského rozhraní. Máte také výhodou další oddělení zálohování dat (_datový Model_) z vaší přední ukončení uživatelské rozhraní (_Model-View-Controller_), tečka na začátku k snadněji provádět údržbu, flexibilnější aplikace návrh.

[![Příkladem spuštěné aplikaci](databinding-images/intro01.png "příklad spuštěné aplikaci")](databinding-images/intro01-large.png#lightbox)

V tomto článku vám nabídneme základní informace o práci s klíč hodnota kódování a datové vazby v Xamarin.Mac aplikace. Vysoce navržený na spolupracovat [Hello, Mac](~/mac/get-started/hello-mac.md) článek nejprve, konkrétně [Úvod do Xcode a rozhraní tvůrce](~/mac/get-started/hello-mac.md#Introduction_to_Xcode_and_Interface_Builder) a [výstupy a akce](~/mac/get-started/hello-mac.md#Outlets_and_Actions) oddíly, jak se popisuje klíčové koncepty a techniky, které budeme používat v tomto článku.

Můžete chtít podívejte se na [třídy vystavení jazyka C# nebo metody jazyka Objective-C](~/mac/internals/how-it-works.md) části [Xamarin.Mac Internals](~/mac/internals/how-it-works.md) dokumentu taky vysvětluje `Register` a `Export` atributy Umožňuje propojit až tříd jazyka C# a uživatelského rozhraní jazyka Objective-C objekty elementy.

<a name="What_is_Key-Value_Coding" />

## <a name="what-is-key-value-coding"></a>Co je to klíč hodnota kódování

Klíč hodnota kódování (KVC) je mechanismus pro přístup k vlastnostem objektu nepřímo, použití klíče (speciálně formátované řetězce) k identifikaci vlastnosti místo k nim přistupovat pomocí instance proměnné nebo metody přístupových objektů (`get/set`). Implementací klíč hodnota kódování kompatibilní přístupových objektů v aplikaci Xamarin.Mac získáte přístup k další funkce systému macOS (dříve označované jako OS X), například klíč hodnota sledování (KVO), vazby dat, základní Data, kakao vazby a scriptability.

Pomocí kódování a datové vazby v aplikaci Xamarin.Mac techniky klíč hodnota, může výrazně snížit množství kód, který máte k zápisu a udržovat naplnit a pracovat s prvky uživatelského rozhraní. Máte také výhodou další oddělení zálohování dat (_datový Model_) z vaší přední ukončení uživatelské rozhraní (_Model-View-Controller_), tečka na začátku k snadněji provádět údržbu, flexibilnější aplikace návrh. 

Například podíváme se na následující definici třídy objektu KVC předpisy:

```csharp
using System;
using Foundation;

namespace MacDatabinding
{
    [Register("PersonModel")]
    public class PersonModel : NSObject
    {
        private string _name = "";
        
        [Export("Name")]
        public string Name {
            get { return _name; }
            set {
                WillChangeValue ("Name");
                _name = value;
                DidChangeValue ("Name");
            }
        }
        
        public PersonModel ()
        {
        }
    }
}
```

Nejdřív `[Register("PersonModel")]` atribut zaregistruje třídu a zpřístupní ho na cíl C. Potom musí dědit z třídy `NSObject` (nebo podtřídou, která dědí z `NSObject`), tento postup přidá některé základní metoda, která umožňují třída jako KVC kompatibilní. Dále `[Export("Name")]` atribut zpřístupňuje `Name` vlastnost a definuje hodnotu klíče, který se později použije pro přístup k vlastnosti prostřednictvím KVC a KVO techniky. 

Nakonec, abyste mohli být dodržen klíč-hodnota se změní na hodnotu vlastnosti, musíte přistupující zabalit změny její hodnota v `WillChangeValue` a `DidChangeValue` volání metod (určení stejným klíčem jako `Export` atributu).  Příklad:

```csharp
set {
    WillChangeValue ("Name");
    _name = value;
    DidChangeValue ("Name");
}
```

Tento krok je _velmi_ důležité pro datové vazby v Xcode je rozhraní tvůrce (Jak vidíme později v tomto článku).

Další informace najdete v tématu společnosti Apple [klíč-hodnota kódování Průvodce programováním](https://developer.apple.com/library/content/documentation/Cocoa/Conceptual/KeyValueCoding/index.html).

### <a name="keys-and-key-paths"></a>Klíčů a klíče cesty

A _klíč_ je řetězec, který identifikuje určité vlastnosti objektu. Klíč obvykle odpovídá názvu metody přistupujícího objektu v kódování kompatibilní objekt klíč hodnota. Klíče musí používat kódování ASCII, obvykle začínat malým písmenem a nesmí obsahovat prázdné znaky. Proto zadané v příkladu výše, `Name` by hodnotu klíče `Name` vlastnost `PersonModel` třídy. Klíč a název vlastnosti, která se zveřejňují nemusí být stejné, ale ve většině případů jsou.

A _cesta ke klíči_ je řetězec tečkou oddělené klíče, které slouží k určení procházení hierarchie vlastností objektu. Vlastnost klíče první v pořadí je relativní vzhledem k příjemce a každý následné klíč hodnotí relativní vůči hodnotě předchozí vlastnosti. Stejným způsobem použít s tečkami procházení objektu a jeho vlastnosti ve třídě C#.

Například, pokud jste rozbalili `PersonModel` třídy a přidat `Child` vlastnost:

```csharp
using System;
using Foundation;

namespace MacDatabinding
{
    [Register("PersonModel")]
    public class PersonModel : NSObject
    {
        private string _name = "";
        private PersonModel _child = new PersonModel();
        
        [Export("Name")]
        public string Name {
            get { return _name; }
            set {
                WillChangeValue ("Name");
                _name = value;
                DidChangeValue ("Name");
            }
        }
        
        [Export("Child")]
        public PersonModel Child {
            get { return _child; }
            set {
                WillChangeValue ("Child");
                _child = value;
                DidChangeValue ("Child");
            }
        }
        
        public PersonModel ()
        {
        }
    }
}
```

Cesta klíče na její název bude `self.Child.Name` nebo jednoduše `Child.Name` (založené na tom, jak byl používán klíč hodnota).

### <a name="getting-values-using-key-value-coding"></a>Získání hodnot pomocí kódování klíč hodnota

`ValueForKey` Vrátí metoda hodnotu pro zadaný klíč (jako `NSString`), relativně k instanci třídy KVC přijetí žádosti. Například pokud `Person` je instance `PersonModel` třídy definované výše:

```csharp
// Read value 
var name = Person.ValueForKey (new NSString("Name"));
```

To by vrátit hodnotu `Name` vlastnost pro tuto instanci `PersonModel`. 

### <a name="setting-values-using-key-value-coding"></a>Nastavení hodnot pomocí kódování klíč hodnota

Podobně `SetValueForKey` nastavit hodnotu pro zadaný klíč (jako `NSString`), relativně k instanci třídy KVC přijetí žádosti. Proto znovu pomocí instance `PersonModel` třídy, jak je uvedeno níže:

```csharp
// Write value
Person.SetValueForKey(new NSString("Jane Doe"), new NSString("Name"));
```

Změní hodnotu `Name` vlastnost `Jane Doe`.

<a name="Observing_Value_Changes" />

### <a name="observing-value-changes"></a>Sledování změn hodnota

Pomocí sledování klíč hodnota (KVO), můžete přiřadit konkrétní klíč třídy KVC kompatibilní pozorovatele a informováni, kdykoli hodnota tohoto klíče se mění (pomocí technik, KVC nebo přímý přístup k dané vlastnosti v kódu jazyka C#). Příklad:

```csharp
// Watch for the name value changing
Person.AddObserver ("Name", NSKeyValueObservingOptions.New, (sender) => {
    // Inform caller of selection change
    Console.WriteLine("New Name: {0}", Person.Name)
});
```

Nyní, kdykoli `Name` vlastnost `Person` instanci `PersonModel` třída je změněn, nová hodnota je zapsané do konzoly. 

Další informace najdete v tématu společnosti Apple [Úvod do Průvodce programováním v klíč-hodnota sledování](https://developer.apple.com/library/content/documentation/Cocoa/Conceptual/KeyValueObserving/KeyValueObserving.html#//apple_ref/doc/uid/10000177i).

## <a name="data-binding"></a>Datová vazba

V následujících částech vám ukáže, jak můžete použít klíč hodnota kódování a klíč hodnota sledování kompatibilní třída vázat data k elementům uživatelského rozhraní v Tvůrci rozhraní na Xcode místo čtení a zápis hodnoty pomocí kódu jazyka C#. Tímto způsobem oddělíte vaše _datový Model_ ze zobrazení, které se používají k jejich zobrazení, provedení aplikace Xamarin.Mac flexibilnější a snadnější zachování. Můžete také výrazně snížit množství kód, který má k zapsání.

<a name="Defining_your_Data_Model" />

### <a name="defining-your-data-model"></a>Definice datového modelu

Než budete moci vytvořit vazbu dat prvku uživatelského rozhraní v Tvůrci rozhraní, musíte mít KVC/KVO kompatibilní třídy definované v aplikaci Xamarin.Mac tak, aby fungoval jako _datový Model_ pro vazbu. Datový Model obsahuje všechna data, která se zobrazí v uživatelském rozhraní a přijímá veškeré úpravy data, která uživatel provede v uživatelském rozhraní při spuštění aplikace.

Například pokud jste zapisovali aplikaci, která spravovat skupinu zaměstnanců, můžete použít následující třídy k definování datového modelu:

```csharp
using System;
using Foundation;
using AppKit;

namespace MacDatabinding
{
    [Register("PersonModel")]
    public class PersonModel : NSObject
    {
        #region Private Variables
        private string _name = "";
        private string _occupation = "";
        private bool _isManager = false;
        private NSMutableArray _people = new NSMutableArray();
        #endregion

        #region Computed Properties
        [Export("Name")]
        public string Name {
            get { return _name; }
            set {
                WillChangeValue ("Name");
                _name = value;
                DidChangeValue ("Name");
            }
        }

        [Export("Occupation")]
        public string Occupation {
            get { return _occupation; }
            set {
                WillChangeValue ("Occupation");
                _occupation = value;
                DidChangeValue ("Occupation");
            }
        }

        [Export("isManager")]
        public bool isManager {
            get { return _isManager; }
            set {
                WillChangeValue ("isManager");
                WillChangeValue ("Icon");
                _isManager = value;
                DidChangeValue ("isManager");
                DidChangeValue ("Icon");
            }
        }

        [Export("isEmployee")]
        public bool isEmployee {
            get { return (NumberOfEmployees == 0); }
        }

        [Export("Icon")]
        public NSImage Icon {
            get {
                if (isManager) {
                    return NSImage.ImageNamed ("group.png");
                } else {
                    return NSImage.ImageNamed ("user.png");
                }
            }
        }

        [Export("personModelArray")]
        public NSArray People {
            get { return _people; }
        }

        [Export("NumberOfEmployees")]
        public nint NumberOfEmployees {
            get { return (nint)_people.Count; }
        }
        #endregion

        #region Constructors
        public PersonModel ()
        {
        }

        public PersonModel (string name, string occupation)
        {
            // Initialize
            this.Name = name;
            this.Occupation = occupation;
        }

        public PersonModel (string name, string occupation, bool manager)
        {
            // Initialize
            this.Name = name;
            this.Occupation = occupation;
            this.isManager = manager;
        }
        #endregion

        #region Array Controller Methods
        [Export("addObject:")]
        public void AddPerson(PersonModel person) {
            WillChangeValue ("personModelArray");
            isManager = true;
            _people.Add (person);
            DidChangeValue ("personModelArray");
        }

        [Export("insertObject:inPersonModelArrayAtIndex:")]
        public void InsertPerson(PersonModel person, nint index) {
            WillChangeValue ("personModelArray");
            _people.Insert (person, index);
            DidChangeValue ("personModelArray");
        }

        [Export("removeObjectFromPersonModelArrayAtIndex:")]
        public void RemovePerson(nint index) {
            WillChangeValue ("personModelArray");
            _people.RemoveObject (index);
            DidChangeValue ("personModelArray");
        }

        [Export("setPersonModelArray:")]
        public void SetPeople(NSMutableArray array) {
            WillChangeValue ("personModelArray");
            _people = array;
            DidChangeValue ("personModelArray");
        }
        #endregion
    }
}
```

Většina funkcí této třídy byly zahrnuté v [co je klíč hodnota kódování](#What_is_Key-Value_Coding) část výše. Ale Podíváme se na několik konkrétní prvky a několik dalších požadavků, které byly provedeny povolit tak, aby fungoval jako datový Model pro tuto třídu **pole řadiče** a **stromu řadiče** (který budeme používat později k datům Vytvoření vazby **stromové zobrazení**, **zobrazení osnovy** a **zobrazení kolekce**).

První, protože zaměstnanec může být správce, jste použili jsme `NSArray` (konkrétně `NSMutableArray` , je možné upravit hodnoty) umožňující zaměstnance, kteří budou spravovat tak, aby se jim:

```csharp
private NSMutableArray _people = new NSMutableArray();
...

[Export("personModelArray")]
public NSArray People {
    get { return _people; }
}
```

Dvě věci si zde:

1. Použili jsme `NSMutableArray` místo standardní pole jazyka C# nebo kolekci, protože, jako je požadavkem k vytvoření vazby dat k ovládacím prvkům AppKit **zobrazení tabulek**, **zobrazení osnovy** a **kolekce** .
2. Jsme vystavené přetypování jej do pole zaměstnanci `NSArray` pro datové vazby účely a změnit jeho C# naformátované název, `People`, který datová vazba očekává, `personModelArray` ve tvaru **{class_name} pole** (Poznámka: že první znak nebyly provedeny malá písmena).

Dále je potřeba přidat některé speciálně název veřejné metody pro podporu **pole řadiče** a **stromu řadiče**:

```csharp
[Export("addObject:")]
public void AddPerson(PersonModel person) {
    WillChangeValue ("personModelArray");
    isManager = true;
    _people.Add (person);
    DidChangeValue ("personModelArray");
}

[Export("insertObject:inPersonModelArrayAtIndex:")]
public void InsertPerson(PersonModel person, nint index) {
    WillChangeValue ("personModelArray");
    _people.Insert (person, index);
    DidChangeValue ("personModelArray");
}

[Export("removeObjectFromPersonModelArrayAtIndex:")]
public void RemovePerson(nint index) {
    WillChangeValue ("personModelArray");
    _people.RemoveObject (index);
    DidChangeValue ("personModelArray");
}

[Export("setPersonModelArray:")]
public void SetPeople(NSMutableArray array) {
    WillChangeValue ("personModelArray");
    _people = array;
    DidChangeValue ("personModelArray");
}
```

Tyto rutiny umožňují řadiče k vyžádání a upravit data, která se zobrazí. Jako zveřejněné `NSArray` vyšší, tyto mají velmi zvláštní zásady vytváření názvů (který se liší od typických C# názvů):

- `addObject:` -Přidá objekt do pole.
- `insertObject:in{class_name}ArrayAtIndex:` -Kde `{class_name}` je název vaší třídy. Tato metoda Vloží objekt do pole v daném indexu.
- `removeObjectFrom{class_name}ArrayAtIndex:` -Kde `{class_name}` je název vaší třídy. Tato metoda odstraní objekt v poli na daného indexu.
- `set{class_name}Array:` -Kde `{class_name}` je název vaší třídy. Tato metoda umožňuje nahradit existující carry novou.

Uvnitř těchto metod jsme jste zabalené změny pole v `WillChangeValue` a `DidChangeValue` zprávy pro KVO dodržování předpisů.

Nakonec od `Icon` vlastnost využívá hodnotu `isManager` vlastnost, změny `isManager` vlastnost nemusí odrážet v `Icon` pro elementy Data vázaná uživatelského rozhraní (během KVO):

```csharp
[Export("Icon")]
public NSImage Icon {
    get {
        if (isManager) {
            return NSImage.ImageNamed ("group.png");
        } else {
            return NSImage.ImageNamed ("user.png");
        }
    }
}
``` 

Pokud chcete opravit, který, můžeme použít následující kód:

```csharp
[Export("isManager")]
public bool isManager {
    get { return _isManager; }
    set {
        WillChangeValue ("isManager");
        WillChangeValue ("Icon");
        _isManager = value;
        DidChangeValue ("isManager");
        DidChangeValue ("Icon");
    }
}
```

Všimněte si, že kromě svůj vlastní klíč, `isManager` také odesílá přistupujícího objektu `WillChangeValue` a `DidChangeValue` zprávy týkající se `Icon` klíče, zobrazí tato změna také.

Budeme používat `PersonModel` datového modelu ve zbývající části tohoto článku.

<a name="Simple_Data_Binding" />

### <a name="simple-data-binding"></a>Jednoduchá datová vazba

Pomocí našeho datového modelu definované Podíváme se na jednoduchý příklad datové vazby v Xcode na rozhraní tvůrce. Například formulář přidejme do našich Xamarin.Mac aplikací, které lze upravit `PersonModel` , jsme definovali výše. Přidáme pár textová pole a zaškrtněte políčko Zobrazit a upravit vlastnosti našeho modelu.

Nejprve umožňuje přidat nový **View Controller** k naší **Main.storyboard** souborů v rozhraní tvůrce a název své třídy `SimpleViewController`: 

[![Přidávání nového řadiče zobrazení](databinding-images/simple01.png "přidávání nového řadiče zobrazení")](databinding-images/simple01-large.png#lightbox)

Pak se vraťte do sady Visual Studio pro Mac, upravit **SimpleViewController.cs** souboru (který se automaticky přidá do našich projekt) a vystavit instanci `PersonModel` jsme bude datové vazby naše formuláře. Přidejte následující kód:

```csharp
private PersonModel _person = new PersonModel();
...

[Export("Person")]
public PersonModel Person {
    get {return _person; }
    set {
        WillChangeValue ("Person");
        _person = value;
        DidChangeValue ("Person");
    }
}
```

Další při načtení zobrazení vytvoříme instanci naše `PersonModel` a jeho naplnění tento kód:

```csharp
public override void ViewDidLoad ()
{
    base.AwakeFromNib ();

    // Set a default person
    var Craig = new PersonModel ("Craig Dunn", "Documentation Manager");
    Craig.AddPerson (new PersonModel ("Amy Burns", "Technical Writer"));
    Craig.AddPerson (new PersonModel ("Joel Martinez", "Web & Infrastructure"));
    Craig.AddPerson (new PersonModel ("Kevin Mullins", "Technical Writer"));
    Craig.AddPerson (new PersonModel ("Mark McLemore", "Technical Writer"));
    Craig.AddPerson (new PersonModel ("Tom Opgenorth", "Technical Writer"));
    Person = Craig;

}
```

Nyní potřebujeme vytvořit našeho formuláře, dvakrát klikněte **Main.storyboard** soubor otevřete pro úpravy v Tvůrci rozhraní. Rozložení formuláře vypadat podobně jako následující:

[![Úpravy storyboard v Xcode](databinding-images/simple02.png "úpravy storyboard v Xcode")](databinding-images/simple02-large.png#lightbox)

Data vazby formuláře `PersonModel` který jsme zveřejněný prostřednictvím `Person` klíč, postupujte takto:

1. Vyberte **jméno zaměstnance** textové pole a přepínač tak, aby **vazby Inspector**.
2. Zkontrolujte **vytvořit vazbu na** a vyberte položku **jednoduché View Controller** z rozevíracího seznamu. Potom zadejte `self.Person.Name` pro **cesta ke klíči**: 

    [![Zadat cestu ke klíči](databinding-images/simple03.png "zadáte cestu ke klíči")](databinding-images/simple03-large.png#lightbox)
3. Vyberte **povolání** textové pole a kontroly **vytvořit vazbu na** pole a vyberte **jednoduché View Controller** z rozevíracího seznamu. Potom zadejte `self.Person.Occupation` pro **cesta ke klíči**:  

    [![Zadat cestu ke klíči](databinding-images/simple04.png "zadáte cestu ke klíči")](databinding-images/simple04-large.png#lightbox)
4. Vyberte **je manažera** zaškrtávací políčko a zkontrolujte **vytvořit vazbu na** a vyberte položku **jednoduché View Controller** z rozevíracího seznamu. Potom zadejte `self.Person.isManager` pro **cesta ke klíči**:  

    [![Zadat cestu ke klíči](databinding-images/simple05.png "zadáte cestu ke klíči")](databinding-images/simple05-large.png#lightbox)
5. Vyberte **číslo spravované zaměstnanci** textové pole a zkontrolujte **vytvořit vazbu na** a vyberte položku **jednoduché View Controller** z rozevíracího seznamu. Potom zadejte `self.Person.NumberOfEmployees` pro **cesta ke klíči**:  

    [![Zadat cestu ke klíči](databinding-images/simple06.png "zadáte cestu ke klíči")](databinding-images/simple06-large.png#lightbox)
6. Pokud zaměstnanec není správce, chceme skrýt číslo z zaměstnanci spravované popisek a textové pole.
7. Vyberte **číslo spravované zaměstnanci** štítku, rozbalte **Hidden** turndown a zkontrolujte **vytvořit vazbu na** a vyberte položku **jednoduché View Controller** z rozevíracího seznamu. Potom zadejte `self.Person.isManager` pro **cesta ke klíči**:  

    [![Zadat cestu ke klíči](databinding-images/simple07.png "zadáte cestu ke klíči")](databinding-images/simple07-large.png#lightbox)
8. Vyberte `NSNegateBoolean` z **hodnotu Transformer** rozevíracího seznamu:  

    ![Výběr klíče transformace NSNegateBoolean](databinding-images/simple08.png "výběr klíče transformace NSNegateBoolean")
9. Tato hodnota informuje datové vazby, pokud bude skrytá popisek hodnotu `isManager` vlastnost je `false`.
10. Opakujte kroky 7 a 8 pro **číslo spravované zaměstnanci** textové pole.
11. Uložte změny a vrátit k sadě Visual Studio pro Mac k synchronizaci s Xcode.

Pokud spustíte aplikaci, hodnoty z `Person` vlastnost bude automaticky vyplnit naše formuláře:

[![Zobrazení formuláře automaticky vyplněna](databinding-images/simple09.png "zobrazující formuláře automaticky vyplněna")](databinding-images/simple09-large.png#lightbox)

Jakékoli změny, které uživatele provádí formuláře zapíšou zpátky do `Person` vlastnost v Kontroleru zobrazení. Například unselecting **je správce** aktualizace `Person` instanci naše `PersonModel` a **číslo spravované zaměstnanci** jsou automaticky (prostřednictvím skryté popisek a textové pole Datová vazba):

[![Skrytí počet zaměstnanců bez manažerům](databinding-images/simple10.png "skrytí počet zaměstnanců pro jiný správce")](databinding-images/simple10-large.png#lightbox)

<a name="Table_View_Data_Binding" />

### <a name="table-view-data-binding"></a>Tabulka zobrazení datová vazba

Teď, když máme základy datové vazby stranou, podíváme se na složitější úlohy vazby dat pomocí _řadiče pole_ a datová vazba na zobrazení tabulky. Další informace o práci se zobrazení tabulek, najdete v tématu naše [zobrazení tabulek](~/mac/user-interface/table-view.md) dokumentaci.

Nejprve umožňuje přidat nový **View Controller** k naší **Main.storyboard** souborů v rozhraní tvůrce a název své třídy `TableViewController`:

[![Přidávání nového řadiče zobrazení](databinding-images/table01.png "přidávání nového řadiče zobrazení")](databinding-images/table01-large.png#lightbox)

V dalším kroku tady upravit **TableViewController.cs** souboru (který se automaticky přidá do našich projekt) a zveřejněte pole (`NSArray`) z `PersonModel` třídy, že jsme bude datové vazby naše formuláře. Přidejte následující kód:

```csharp
private NSMutableArray _people = new NSMutableArray();
...

[Export("personModelArray")]
public NSArray People {
    get { return _people; }
}
...

[Export("addObject:")]
public void AddPerson(PersonModel person) {
    WillChangeValue ("personModelArray");
    _people.Add (person);
    DidChangeValue ("personModelArray");
}

[Export("insertObject:inPersonModelArrayAtIndex:")]
public void InsertPerson(PersonModel person, nint index) {
    WillChangeValue ("personModelArray");
    _people.Insert (person, index);
    DidChangeValue ("personModelArray");
}

[Export("removeObjectFromPersonModelArrayAtIndex:")]
public void RemovePerson(nint index) {
    WillChangeValue ("personModelArray");
    _people.RemoveObject (index);
    DidChangeValue ("personModelArray");
}

[Export("setPersonModelArray:")]
public void SetPeople(NSMutableArray array) {
    WillChangeValue ("personModelArray");
    _people = array;
    DidChangeValue ("personModelArray");
}
```

Stejně jako jsme provedli na `PersonModel` třídy výše v [definice datového modelu](#Defining_your_Data_Model) části jsme jste zveřejněné čtyři speciálně pojmenovanou metody tak, aby řadič pole a čtení a zápis dat z našich kolekce `PersonModels`.

Další při načtení zobrazení, je potřeba naplnit naše pole s tímto kódem:

```csharp
public override void AwakeFromNib ()
{
    base.AwakeFromNib ();

    // Build list of employees
    AddPerson (new PersonModel ("Craig Dunn", "Documentation Manager", true));
    AddPerson (new PersonModel ("Amy Burns", "Technical Writer"));
    AddPerson (new PersonModel ("Joel Martinez", "Web & Infrastructure"));
    AddPerson (new PersonModel ("Kevin Mullins", "Technical Writer"));
    AddPerson (new PersonModel ("Mark McLemore", "Technical Writer"));
    AddPerson (new PersonModel ("Tom Opgenorth", "Technical Writer"));
    AddPerson (new PersonModel ("Larry O'Brien", "API Documentation Manager", true));
    AddPerson (new PersonModel ("Mike Norman", "API Documenter"));

}
```

Nyní potřebujeme vytvořit naše tabulka zobrazení, dvakrát klikněte **Main.storyboard** soubor otevřete pro úpravy v Tvůrci rozhraní. Rozložení tabulku, kterou chcete vypadat podobně jako následující:

[![Rozložení nové zobrazení tabulky](databinding-images/table02.png "rozložení nové zobrazení tabulky")](databinding-images/table02-large.png#lightbox)

Je potřeba přidat **řadiče pole** zajistit vázaných dat do našich tabulky, postupujte takto:

1. Přetáhněte **pole řadič** z **knihovny Inspector** na **rozhraní editoru**:  

    ![Výběr řadiče pole z knihovny](databinding-images/table03.png "výběr řadiče pole z knihovny")
2. Vyberte **řadiče pole** v **rozhraní hierarchie** a přepněte do **atribut Inspector**:  

    [![Výběr Inspector atributy](databinding-images/table04.png "výběr Inspector atributy")](databinding-images/table04-large.png#lightbox)
3. Zadejte `PersonModel` pro **název třídy**, klikněte **Plus** tlačítko a přidejte tři klíče. Název je `Name`, `Occupation` a `isManager`:  

    ![Přidání požadovaných klíče cesty](databinding-images/table05.png "přidání požadované klíče cesty")
4. Tato hodnota informuje řadič pole co správou pole, a vlastnosti, které by měl vystavit (pomocí klíče).
5. Přepnout **vazby Inspector** a v části **obsahu pole** vyberte **vytvořit vazbu na** a **řadiče zobrazení tabulky**. Zadejte **modelu cestu ke klíči** z `self.personModelArray`:  

    ![Zadávání cestu ke klíči](databinding-images/table06.png "zadávání cestu ke klíči")
6. Sváže řadičem pole, aby pole `PersonModels` který jsme zveřejněný v Kontroleru zobrazení.

Nyní potřebujeme vytvořit vazbu naše tabulka zobrazení řadič pole, postupujte takto:

1. Vyberte zobrazení, tabulky a **vazby Inspector**:  

    [![Výběr Inspector vazby](databinding-images/table07.png "výběr Inspector vazby")](databinding-images/table07-large.png#lightbox)
2. V části **obsahu tabulky** turndown, vyberte **vytvořit vazbu na** a **řadiče pole**. Zadejte `arrangedObjects` pro **řadiče klíč** pole:  

    ![Definování klíč řadič](databinding-images/table08.png "definování řadiče klíč")
3. Vyberte **buňku zobrazení tabulky** pod **zaměstnanec** sloupce. V **vazby Inspector** pod **hodnotu** turndown, vyberte **vytvořit vazbu na** a **tabulce buňku zobrazení**. Zadejte `objectValue.Name` pro **modelu cestu ke klíči**:  

    [![Nastavení cestu ke klíči modelu](databinding-images/table09.png "nastavení cestu ke klíči modelu")](databinding-images/table09-large.png#lightbox)
4. `objectValue` je aktuální `PersonModel` v poli je spravováno nástrojem řadič pole.
5. Vyberte **buňku zobrazení tabulky** pod **povolání** sloupce. V **vazby Inspector** pod **hodnotu** turndown, vyberte **vytvořit vazbu na** a **tabulce buňku zobrazení**. Zadejte `objectValue.Occupation` pro **modelu cestu ke klíči**:  

    [![Nastavení cestu ke klíči modelu](databinding-images/table10.png "nastavení cestu ke klíči modelu")](databinding-images/table10-large.png#lightbox)
6. Uložte změny a vrátit k sadě Visual Studio pro Mac k synchronizaci s Xcode.

Pokud jsme aplikaci spustit, v tabulce bude možné naplnit naše pole `PersonModels`:

[![Spuštění aplikace](databinding-images/table11.png "spuštění aplikace")](databinding-images/table11-large.png#lightbox)

<a name="Outline_View_Data_Binding" />

### <a name="outline-view-data-binding"></a>Vazba dat zobrazení osnovy

Datová vazba proti zobrazení osnovy je velmi podobné vazby proti zobrazení tabulky. Klíčovým rozdílem je, že budeme používat **stromu řadič** místo **řadiče pole** umožníte vázané data k zobrazení osnovy. Další informace o práci se zobrazení osnovy, najdete v tématu naše [zobrazení osnovy](~/mac/user-interface/outline-view.md) dokumentaci.

Nejprve umožňuje přidat nový **View Controller** k naší **Main.storyboard** souborů v rozhraní tvůrce a název své třídy `OutlineViewController`: 

[![Přidávání nového řadiče zobrazení](databinding-images/outline01.png "přidávání nového řadiče zobrazení")](databinding-images/outline01-large.png#lightbox)

V dalším kroku tady upravit **OutlineViewController.cs** souboru (který se automaticky přidá do našich projekt) a zveřejněte pole (`NSArray`) z `PersonModel` třídy, že jsme bude datové vazby naše formuláře. Přidejte následující kód:

```csharp
private NSMutableArray _people = new NSMutableArray();
...

[Export("personModelArray")]
public NSArray People {
    get { return _people; }
}
...

[Export("addObject:")]
public void AddPerson(PersonModel person) {
    WillChangeValue ("personModelArray");
    _people.Add (person);
    DidChangeValue ("personModelArray");
}

[Export("insertObject:inPersonModelArrayAtIndex:")]
public void InsertPerson(PersonModel person, nint index) {
    WillChangeValue ("personModelArray");
    _people.Insert (person, index);
    DidChangeValue ("personModelArray");
}

[Export("removeObjectFromPersonModelArrayAtIndex:")]
public void RemovePerson(nint index) {
    WillChangeValue ("personModelArray");
    _people.RemoveObject (index);
    DidChangeValue ("personModelArray");
}

[Export("setPersonModelArray:")]
public void SetPeople(NSMutableArray array) {
    WillChangeValue ("personModelArray");
    _people = array;
    DidChangeValue ("personModelArray");
}
```

Stejně jako jsme provedli na `PersonModel` třídy výše v [definice datového modelu](#Defining_your_Data_Model) části jsme jste zveřejněné čtyři speciálně pojmenovanou metody tak, aby řadič stromu a čtení a zápis dat z našich kolekce `PersonModels`.

Další při načtení zobrazení, je potřeba naplnit naše pole s tímto kódem:

```csharp
public override void AwakeFromNib ()
{
    base.AwakeFromNib ();

    // Build list of employees
    var Craig = new PersonModel ("Craig Dunn", "Documentation Manager");
    Craig.AddPerson (new PersonModel ("Amy Burns", "Technical Writer"));
    Craig.AddPerson (new PersonModel ("Joel Martinez", "Web & Infrastructure"));
    Craig.AddPerson (new PersonModel ("Kevin Mullins", "Technical Writer"));
    Craig.AddPerson (new PersonModel ("Mark McLemore", "Technical Writer"));
    Craig.AddPerson (new PersonModel ("Tom Opgenorth", "Technical Writer"));
    AddPerson (Craig);

    var Larry = new PersonModel ("Larry O'Brien", "API Documentation Manager");
    Larry.AddPerson (new PersonModel ("Mike Norman", "API Documenter"));
    AddPerson (Larry);

}
```

Nyní potřebujeme vytvořit naše zobrazení osnovy, dvakrát klikněte **Main.storyboard** soubor otevřete pro úpravy v Tvůrci rozhraní. Rozložení tabulku, kterou chcete vypadat podobně jako následující:

[![Vytváření zobrazení osnovy](databinding-images/outline02.png "vytváření zobrazení osnovy")](databinding-images/outline02-large.png#lightbox)

Je potřeba přidat **stromu řadiče** poskytovat vázané data do našich outline, postupujte takto:

1. Přetáhněte **stromu řadič** z **knihovny Inspector** na **rozhraní editoru**:  

    ![Výběr řadič stromu z knihovny](databinding-images/outline03.png "výběr řadič stromu z knihovny")
2. Vyberte **stromu řadič** v **rozhraní hierarchie** a přepněte do **atribut Inspector**:  

    [![Výběr atributu Inspector](databinding-images/outline04.png "výběr atributu Inspector")](databinding-images/outline04-large.png#lightbox)
3. Zadejte `PersonModel` pro **název třídy**, klikněte **Plus** tlačítko a přidejte tři klíče. Název je `Name`, `Occupation` a `isManager`:  

    ![Přidání požadovaných klíče cesty](databinding-images/outline05.png "přidání požadované klíče cesty")
4. Tato hodnota informuje řadičem stromu co správou pole, a vlastnosti, které by měl vystavit (pomocí klíče).
5. V části **stromu řadič** zadejte `personModelArray` pro **podřízené objekty**, zadejte `NumberOfEmployees` pod **počet** a zadejte `isEmployee` pod  **Listu**:  

    ![Nastavení klíče cesty řadiče stromu](databinding-images/outline05.png "nastavení klíče cesty řadiče stromu")
6. Tato hodnota informuje řadičem stromu kde najít všechny podřízené uzly, jsou počet podřízených uzlů došlo a zda má aktuální uzel podřízené uzly.
7. Přepnout **vazby Inspector** a v části **obsahu pole** vyberte **vytvořit vazbu na** a **vlastník souboru**. Zadejte **modelu cestu ke klíči** z `self.personModelArray`:  

    ![Úpravy cestu ke klíči](databinding-images/outline06.png "úpravy cestu ke klíči")
8. Sváže řadičem stromu do pole `PersonModels` který jsme zveřejněný v Kontroleru zobrazení.

Nyní potřebujeme vytvořit vazbu naše zobrazení osnovy řadič stromu, postupujte takto:

1. Vyberte zobrazení osnovy a v **vazby Inspector** vyberte:  

    [![Výběr Inspector vazby](databinding-images/outline07.png "výběr Inspector vazby")](databinding-images/outline07-large.png#lightbox)
2. V části **Outline zobrazit obsah** turndown, vyberte **vytvořit vazbu na** a **stromu řadiče**. Zadejte `arrangedObjects` pro **řadiče klíč** pole:  

    ![Nastavení klíče řadič](databinding-images/outline08.png "nastavení klíče řadiče")
3. Vyberte **buňku zobrazení tabulky** pod **zaměstnanec** sloupce. V **vazby Inspector** pod **hodnotu** turndown, vyberte **vytvořit vazbu na** a **tabulce buňku zobrazení**. Zadejte `objectValue.Name` pro **modelu cestu ke klíči**:  

    [![Zadat cestu ke klíči modelu](databinding-images/outline09.png "zadáte cestu ke klíči modelu")](databinding-images/outline09-large.png#lightbox)
4. `objectValue` je aktuální `PersonModel` v poli spravován adaptérem stromu.
5. Vyberte **buňku zobrazení tabulky** pod **povolání** sloupce. V **vazby Inspector** pod **hodnotu** turndown, vyberte **vytvořit vazbu na** a **tabulce buňku zobrazení**. Zadejte `objectValue.Occupation` pro **modelu cestu ke klíči**:  

    [![Zadat cestu ke klíči modelu](databinding-images/outline10.png "zadáte cestu ke klíči modelu")](databinding-images/outline10-large.png#lightbox)
6. Uložte změny a vrátit k sadě Visual Studio pro Mac k synchronizaci s Xcode.

Pokud jsme aplikaci spustit, bude možné obrys naplnit naše pole `PersonModels`:

[![Spuštění aplikace](databinding-images/outline11.png "spuštění aplikace")](databinding-images/outline11-large.png#lightbox)

### <a name="collection-view-data-binding"></a>Kolekce zobrazení datová vazba

Datová vazba s zobrazení kolekce je velmi podobný s zobrazení tabulky, jako řadič pole slouží k poskytování dat pro kolekci. Vzhledem k tomu, že zobrazení kolekce nemá formát přednastavené zobrazení, další práci a je nutné k poskytnutí zpětné vazby interakce uživatelů ke sledování výběr uživatele.

> [!IMPORTANT]
> Kvůli problému v Xcode 7 a systému macOS 10.11 (a vyšší) není možné použít uvnitř soubory Storyboard (.storyboard) zobrazení kolekcí. V důsledku toho bude muset nadále používat k definování zobrazení kolekce pro vaše aplikace Xamarin.Mac .xib soubory. Najdete v tématu naše [zobrazení kolekce](~/mac/user-interface/collection-view.md) Další informace naleznete v dokumentaci.

<!--KKM 012/16/2015 - Once Apple fixes the issue with Xcode and Collection Views in Storyboards, we can uncomment this section.

First, let's add a new **View Controller** to our **Main.storyboard** file in Interface Builder and name its class `CollectionViewController`: 

![](databinding-images/collection01.png)

Next, let's edit the **CollectionViewController.cs** file (that was automatically added to our project) and expose an array (`NSArray`) of `PersonModel` classes that we will be data binding our form to. Add the following code:

```csharp
private NSMutableArray _people = new NSMutableArray();
...

[Export("personModelArray")]
public NSArray People {
    get { return _people; }
}
...

[Export("addObject:")]
public void AddPerson(PersonModel person) {
    WillChangeValue ("personModelArray");
    _people.Add (person);
    DidChangeValue ("personModelArray");
}

[Export("insertObject:inPersonModelArrayAtIndex:")]
public void InsertPerson(PersonModel person, nint index) {
    WillChangeValue ("personModelArray");
    _people.Insert (person, index);
    DidChangeValue ("personModelArray");
}

[Export("removeObjectFromPersonModelArrayAtIndex:")]
public void RemovePerson(nint index) {
    WillChangeValue ("personModelArray");
    _people.RemoveObject (index);
    DidChangeValue ("personModelArray");
}

[Export("setPersonModelArray:")]
public void SetPeople(NSMutableArray array) {
    WillChangeValue ("personModelArray");
    _people = array;
    DidChangeValue ("personModelArray");
}
```

Just like we did on the `PersonModel` class above in the [Defining your Data Model](#Defining_your_Data_Model) section, we've exposed four specially named public methods so that the Array Controller and read and write data from our collection of `PersonModels`.

Next when the View is loaded, we need to populate our array with this code:

```csharp
public override void AwakeFromNib ()
{
    base.AwakeFromNib ();

    // Build list of employees
    AddPerson (new PersonModel ("Craig Dunn", "Documentation Manager", true));
    AddPerson (new PersonModel ("Amy Burns", "Technical Writer"));
    AddPerson (new PersonModel ("Joel Martinez", "Web & Infrastructure"));
    AddPerson (new PersonModel ("Kevin Mullins", "Technical Writer"));
    AddPerson (new PersonModel ("Mark McLemore", "Technical Writer"));
    AddPerson (new PersonModel ("Tom Opgenorth", "Technical Writer"));
    AddPerson (new PersonModel ("Larry O'Brien", "API Documentation Manager", true));
    AddPerson (new PersonModel ("Mike Norman", "API Documenter"));

}
```

Now we need to create our Collection View, double-click the **Main.storyboard** file to open it for editing in Interface Builder. Layout the Collection View to look something like the following:

![](databinding-images/collection02.png)

When you add a Collection View to a User Interface design, two extra elements are also added:

1.  **Collection View Item** -  That manages a single instance of an item in the collection.
2.  **View** - A custom view that provides the visual size and appearance of each item in the collection. This view is tied to and managed by the **Collection View Item**.

Select the view and make it look like the following using an Image View and two Text Fields:

![](databinding-images/collection03.png)

One thing to note here, a `NSBox` was added behind everything in the view with the following attributes:

![](databinding-images/collection04.png)

We'll be using this box to provide feedback to the user when an item is selected in the Collection View.

We need to add an **Array Controller** to provide bound data to our collection, do the following:

1. Drag an **Array Controller** from the **Library Inspector** onto the **Interface Editor**: <br/>![](databinding-images/table03.png)
2. Select **Array Controller** in the **Interface Hierarchy** and switch to the **Attribute Inspector**: <br/>![](databinding-images/collection05.png)
3. Enter `PersonModel` for the **Class Name**, click the **Plus** button and add four Keys. Name them `Icon`, `Name`, `Occupation` and `isManager`: <br/>![](databinding-images/table05.png)
4. This tells the Array Controller what it is managing an array of, and which properties it should expose (via Keys).
5. Switch to the **Bindings Inspector** and under **Content Array** select **Bind to** and **File's Owner**. Enter a **Model Key Path** of `self.personModelArray`: <br/>![](databinding-images/table06.png)
6. This ties the Array Controller to the array of `PersonModels` that we exposed on our View Controller.

Now we need to bind our Collection View to the Array Controller, do the following:

1. Select the Collection View and the **Binding Inspector**: <br/>![](databinding-images/collection06.png)
2. Under the **Contents** turndown, select **Bind to** and **Array Controller**. Enter `arrangedObjects` for the **Controller Key** field: <br/>![](databinding-images/collection07.png)
3. Under the **Selection Indexes** turndown, select **Bind to** and **Array Controller**. Enter `selectionIndexes` for the **Controller Key** field: <br/>![](databinding-images/collection08.png)
4. Select the **Image View**. In the **Bindings Inspector** under the **Value** turndown, select **Bind to** and **Person** (the name of our Collection View Item). Enter `representedObject.Icon` for the **Model Key Path**: <br/>![](databinding-images/collection09.png)
5. `representedObject` is the current `PersonModel` in the array being managed by the Array Controller.
6. Select the first **Label**. In the **Bindings Inspector** under the **Value** turndown, select **Bind to** and **Collection View Item**. Enter `representedObject.Name` for the **Model Key Path**: <br/>![](databinding-images/collection10.png)
7. Select the second **Label**. In the **Bindings Inspector** under the **Value** turndown, select **Bind to** and **Collection View Item**. Enter `representedObject.Occupation` for the **Model Key Path**: <br/>![](databinding-images/collection11.png)
8. Select the `NSBox`. In the **Bindings Inspector** under the **Hidden** turndown, select **Bind to** and **Collection View Item**. Enter `selected` for the **Model Key Path** and `NSNegateBoolean` for the **Value Transformer**: <br/>![](databinding-images/collection12.png)
9. Save your changes and return to Visual Studio for Mac to sync with Xcode.

If we run the application, the table will be populated with our array of `PersonModels`:

![](databinding-images/collection13.png)

For more information on working with Collection Views, please see our [Collection Views](~/mac/user-interface/collection-view.md) documentation.-->

## <a name="debugging-native-crashes"></a>Ladění nativního havárií

Že uděláte chybu v datové vazby může mít za následek _nativní havárií_ v nespravované kódu a způsobit, že aplikace Xamarin.Mac úplné selhání s `SIGABRT` Chyba:

[![Příklad dialogového okna nativní havárií](databinding-images/debug01.png "příklad dialogového okna nativní havárií")](databinding-images/debug01-large.png#lightbox)

Během vazby dat obvykle existují čtyři hlavní příčiny nativní dojde k chybě:

1. Datový Model nedědí z `NSObject` nebo podtřídou třídy `NSObject`.
2. Není vystavit vaší vlastnost pomocí jazyka Objective-C `[Export("key-name")]` atribut.
3. Není zalomen změn přistupujícího objektu hodnoty v `WillChangeValue` a `DidChangeValue` volání metod (určení stejným klíčem jako `Export` atributu).
4. Už máte chybným nebo je chybný klíč v **vazby Inspector** v Tvůrci rozhraní.

### <a name="decoding-a-crash"></a>Dekódování havárie

Umožňuje způsobit nativní havárie v naší datové vazby, proto jsme ukazují, jak najít a opravit. V Tvůrci rozhraní, zkuste změnit naše vazby první popisek v příkladu zobrazení kolekce z `Name` k `Title`:

[![Úpravy klíč vazby](databinding-images/debug02.png "úpravy klíč vazby")](databinding-images/debug02-large.png#lightbox)

Umožňuje změnu uložíte, přepněte zpět na Visual Studio pro Mac synchronizovat s Xcode a spuštění aplikace. Při zobrazení kolekce se zobrazí, bude v aplikaci na okamžik chybě s `SIGABRT` chyba (jak je znázorněno **výstupu aplikace** v sadě Visual Studio pro Mac) vzhledem k tomu, `PersonModel` nevystavuje vlastnost klíčem `Title`:

[![Příklad chybu vazby](databinding-images/debug03.png "příklad chybu vazby")](databinding-images/debug03-large.png#lightbox)

Pokud jsme posunete začátek chyby v **výstupu aplikace** vidíme klíč k řešení problému:

[![Hledání problém v protokolu chyb](databinding-images/debug04.png "hledání problém v protokolu chyb")](databinding-images/debug04-large.png#lightbox)

Tento řádek nám oznamuje, že klíč `Title` neexistuje v objektu, který jsme vazbu k. Pokud dojde ke změně vazby zpět do `Name` v Tvůrci rozhraní, uložit, synchronizace, znovu sestavit a spustit, bude aplikace spuštěná podle očekávání bez problém.

## <a name="summary"></a>Souhrn

Tento článek podrobně prohlédnout práce datové vazby a kódování klíč hodnota v aplikaci Xamarin.Mac trvá. Nejprve hledá v vystavení třída C# pro Objective-C pomocí klíč hodnota kódování (KVC) a klíč hodnota sledování (KVO). V dalším kroku se vám ukázal, jak používat třídu kompatibilní KVO a navázat jej Data prvky uživatelského rozhraní v Tvůrci rozhraní pro Xcode. Nakonec ukázal komplexní datová vazba pomocí **pole řadiče** a **stromu řadiče**.


## <a name="related-links"></a>Související odkazy

- [Scénáře MacDatabinding (ukázka)](https://developer.xamarin.com/samples/mac/MacDatabinding-Storyboard/)
- [MacDatabinding XIBs (ukázka)](https://developer.xamarin.com/samples/mac/MacDatabinding-XIBs/)
- [Hello, Mac](~/mac/get-started/hello-mac.md)
- [Standardní ovládací prvky](~/mac/user-interface/standard-controls.md)
- [Zobrazení tabulek](~/mac/user-interface/table-view.md)
- [Zobrazení osnovy](~/mac/user-interface/outline-view.md)
- [Zobrazení kolekce](~/mac/user-interface/collection-view.md)
- [Klíč hodnota kódování Průvodce programováním](https://developer.apple.com/library/content/documentation/Cocoa/Conceptual/KeyValueCoding/index.html)
- [Úvod do Průvodce programováním v sledování klíč hodnota](https://developer.apple.com/library/content/documentation/Cocoa/Conceptual/KeyValueObserving/KeyValueObserving.html)
- [Úvod do kakao vazby programování témata](https://developer.apple.com/library/content/documentation/Cocoa/Conceptual/CocoaBindings/CocoaBindings.html)
- [Úvod do kakao vazby odkaz](https://developer.apple.com/library/content/documentation/Cocoa/Reference/CocoaBindingsRef/CocoaBindingsRef.html)
- [NSCollectionView](https://developer.apple.com/documentation/appkit/nscollectionview)
- [systému macOS Human Interface Guidelines](https://developer.apple.com/macos/human-interface-guidelines/overview/themes/)
