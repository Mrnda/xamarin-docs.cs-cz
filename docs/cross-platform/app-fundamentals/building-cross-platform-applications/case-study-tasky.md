---
title: 'Případová studie: Tasky'
description: Tento dokument popisuje, jak se zásadami vytváření aplikací a platformy byly použity v Tasky přenosné ukázkové aplikaci. Dotýká se na návrhu mobilních aplikací, zápisu společný kód pro nové použití a implementace projekty specifických pro platformy, které používají iOS, Android a Windows Phone platformy.
ms.prod: xamarin
ms.assetid: B581B2D0-9890-C383-C654-0B0E12DAD5A6
ms.technology: xamarin-cross-platform
author: asb3993
ms.author: amburns
ms.date: 03/23/2017
ms.openlocfilehash: 690edabd53752ff0347fdb232a4bbfcb1ba6e84d
ms.sourcegitcommit: dc882e9631b4ed52596b944a6fbbdde309346943
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/26/2018
---
# <a name="case-study-tasky"></a>Případová studie: Tasky

_Tento dokument popisuje, jak se zásadami vytváření aplikací a platformy byly použity v Tasky přenosné ukázkové aplikaci. Dotýká se na návrhu mobilních aplikací, zápisu společný kód pro nové použití a implementace projekty specifických pro platformy, které používají iOS, Android a Windows Phone platformy._

*Tasky* *přenosné* je aplikace seznamu úkolů jednoduché. Tento dokument popisuje, jak byl navržen a sestaven následující pokyny z [vytváření aplikací a platformy](~/cross-platform/app-fundamentals/building-cross-platform-applications/index.md) dokumentu. Diskuse věnována následujícím oblastem:

<a name="Design_Process" />

## <a name="design-process"></a>Proces návrhu

Doporučuje se vytvořit silniční mapy pro co chcete dosáhnout před zahájením kódování. To platí hlavně pro vývoj pro různé platformy, které vytváříte funkce, které se zveřejní několika způsoby. Počínaje jasno, co již vytváříte ušetříte čas a úsilí později v cyklu vývoje.

 <a name="Requirements" />

### <a name="requirements"></a>Požadavky

Prvním krokem při navrhování aplikace je k identifikaci požadované funkce. Může jít o vysoké cíle nebo podrobné případy použití. Tasky má přehledné funkční požadavky:

 -  Zobrazení seznamu úloh
 -  Přidání, úpravy a odstraňování úkolů
 -  Nastavte stav úlohy, udělat.

Měli byste zvážit použití funkce specifické pro platformu.  Tasky, můžete využít výhod monitorování geografických zón iOS nebo Windows Phone živé dlaždice? I v případě, že nepoužíváte funkce specifické pro platformu v první verzi, měli byste naplánovat dopředu a ujistěte se, že vaše podnikání a datové vrstvy jim můžete přizpůsobit.

 <a name="User_Interface_Design" />

### <a name="user-interface-design"></a>Návrh uživatelského rozhraní

Začněte hlavnímu návrhu, které se dají implementovat napříč cílové platformy. Pečlivě Poznámka platformy specifická uživatelského rozhraní omezení. Například `TabBarController` v iOS můžete zobrazit více než pět tlačítek, zatímco ekvivalentní Windows Phone můžou zobrazit až čtyři.
Zakreslit obrazovky flow pomocí nástroje podle vašeho výběru (funguje dokumentu).

 [![](case-study-tasky-images/taskydesign.png "Kreslení obrazovky flow pomocí nástroje vaše volba dokumentu funguje")](case-study-tasky-images/taskydesign.png#lightbox)

 <a name="Data_Model" />

### <a name="data-model"></a>Datový Model

Zároveň budete vědět, jaká data musí být uložená vám pomohou určit mechanismus trvalosti. V tématu [přístup k datům a platformy](~/cross-platform/app-fundamentals/index.md) informace o mechanismy úložiště k dispozici a pomoci při rozhodování mezi nimi. Pro tento projekt budeme používat SQLite.NET.

Tasky potřebuje k uložení tři vlastnosti pro každý TaskItem:

 -  **Název** – řetězec
 -  **Poznámky k** – řetězec
 -  **Provádí** – Boolean

 <a name="Core_Functionality" />

### <a name="core-functionality"></a>Základní funkce

Vezměte v úvahu rozhraní API, uživatelské rozhraní bude nutné využívat ke splnění požadavků. Seznam úkolů vyžaduje následující funkce:

 -   **Seznam všech úkolů** – Pokud chcete zobrazit seznam všech dostupných úloh hlavní obrazovky
 -  **Získat jeden úkol** – Pokud je dotýkal řádek úkolu
 -  **Uložit jeden úkol** – Pokud je upravit úlohu
 -  **Odstranit jeden úkol** – když se úloha odstraní.
 -  **Vytvořit prázdnou úlohu** – při vytváření nové úlohy

K dosažení opakované použití kódu, toto rozhraní API by měl být prováděn jednou *Přenosná knihovna tříd*.

 <a name="Implementation" />

### <a name="implementation"></a>Implementace

Jakmile návrh aplikace typové, zvažte, jak se může implementovat jako aplikace napříč platformami. To se stane Architektura aplikace. Následující pokyny v [vytváření aplikací a platformy](~/cross-platform/app-fundamentals/building-cross-platform-applications/index.md) dokumentu kód aplikace by měl být poškozená dolů do následujících částí:

 -   **Společný kód** – běžné projekt, který obsahuje znovu funkční kód pro ukládání dat úloh; vystavit třídu modelu a rozhraní API pro správu ukládání a načítání dat.
 -   **Kód specifický pro platformu** – projekty specifických pro platformy, které implementovat nativní uživatelského rozhraní pro každý operační systém využívá společný kód jako back end.

 [![](case-study-tasky-images/taskypro-architecture.png "Specifické pro platformu projekty implementovat nativní uživatelského rozhraní pro každý operační systém využívá společný kód jako back-end")](case-study-tasky-images/taskypro-architecture.png#lightbox)

Tyto dvě části jsou popsány v následujících částech.

 <a name="Common_(PCL)_Code" />

## <a name="common-pcl-code"></a>Společný kód (PCL)

Tasky přenositelností používá strategie knihovny přenosných tříd pro sdílení společný kód. Najdete v článku [sdílení kódu možnosti](~/cross-platform/app-fundamentals/code-sharing.md) dokumentu popis možností sdílení kódu.

Všechny společný kód, včetně vrstva přístupu k datům, databáze kódu a kontrakty, je umístěn v projektu knihovny.

Dole je zobrazená dokončený projekt PCL. Kód v knihovně přenosné je kompatibilní s každou cílovou platformu. Při nasazení, bude každý nativní aplikaci odkazovat této knihovny.

![](case-study-tasky-images/portable-project.png "Při nasazení, každý nativní aplikaci bude odkazovat této knihovny")

Třída obrázek ukazuje třídy seskupené podle vrstvy. `SQLiteConnection` Třída je často používaný kód z balíčku Sqlite NET. Zbytek třídy jsou vlastní kód pro Tasky. `TaskItemManager` a `TaskItem` třídy představují rozhraní API, který je zveřejněný k aplikacím, specifické pro platformu.

 [![](case-study-tasky-images/classdiagram-core.png "Třídy TaskItemManager a TaskItem představují rozhraní API, který je zveřejněný k aplikacím, specifické pro platformu")](case-study-tasky-images/classdiagram-core.png#lightbox)

Použití oboru názvů k oddělení vrstvy, které pomáhá spravovat odkazy mezi jednotlivé úrovně. Projekty specifické pro platformu pouhým zahrnout `using` příkaz obchodní vrstvy. Data Access Layer a datová vrstva by měl být zapouzdřené pomocí rozhraní API, který je zveřejněný prostřednictvím `TaskItemManager` v obchodní vrstvě.

 <a name="References" />

### <a name="references"></a>Odkazy

Knihovny přenosných tříd musí být použitelná napříč různými platformami, každý s různými úrovněmi podporou pro funkce platformy a framework. Z tohoto důvodu existují určitá omezení, na které lze použít balíčky a knihoven. Například Xamarin.iOS nepodporuje jazyka c# `dynamic` – klíčové slovo, takže knihovny přenosných tříd nelze použít žádné balíček, který závisí na dynamický kód, i když se takový kód bude fungovat v systému Android. Visual Studio pro Mac zabrání přidávání nekompatibilní balíčky a reference, ale budete chtít omezení mějte na paměti, aby se zabránilo později na výskyt nečekaných událostí.

Poznámka: Zobrazí se, že vaše projekty odkazují framework knihovny, které nebyly použity. Tyto odkazy jsou součástí šablony projektu Xamarin. Když kompilované aplikace, bude proces propojení odebrat neregistrované kódu, proto i když `System.Xml` bylo odkazováno, nebude zahrnutý do konečné aplikace protože používáme nejsou žádné funkce Xml.

 <a name="Data_Layer_(DL)" />

### <a name="data-layer-dl"></a>Datová vrstva (DL)

Datová vrstva obsahuje kód, který nemá fyzického úložiště dat – do databáze, plochých souborů nebo jinému kontrolnímu mechanismu. Tasky Datová vrstva se skládá ze dvou částí: knihovně SQLite NET a vlastní kód přidány do propojit nahoru.

Tasky spoléhá na balíček nuget Sqlite net (publikováno František Kreuger) pro vložení SQLite NET kód, který poskytuje rozhraní databáze relační objekt mapování (ORM). `TaskItemDatabase` Třída dědí z `SQLiteConnection` a přidá požadované vytvořit, číst, aktualizovat, odstranit (CRUD) metody pro čtení a zápisu dat do SQLite. Je na jednoduchý standardní implementace obecné metody CRUD, které by mohly být znovu použít v jiné projekty.

`TaskItemDatabase` Je typu singleton, zajistíte, že veškerý přístup k proti stejnou instanci. Zámek se používá při prevenci souběžný přístup z více vláken.

 <a name="SQLite_on_WIndows_Phone" />

#### <a name="sqlite-on-windows-phone"></a>SQLite na Windows Phone

Při iOS a Android obě dodávají spolu s SQLite jako součást operačního systému Windows Phone nezahrnuje kompatibilní databázového stroje. Sdílení kódu pro všechny tři platformy Windows phone nativní verzi SQLite je požadovaná. V tématu [práci s místní databází](~/xamarin-forms/app-fundamentals/databases.md) Další informace o nastavení projektu Windows Phone pro Sqlite.

 <a name="Using_an_Interface_to_Generalize_Data_Access" />

#### <a name="using-an-interface-to-generalize-data-access"></a>Pomocí rozhraní ke generalizaci přístup k datům

Datová vrstva trvá závislost `BL.Contracts.IBusinessIdentity` tak, aby ho můžete implementovat abstraktní metody pro přístup k vyžadující primární klíč. Všechny obchodní vrstvy třídu, která implementuje rozhraní nastavit jako trvalý pak v datové vrstvě.

Rozhraní právě určuje ve vlastnosti integer tak, aby fungoval jako primární klíč:

```csharp
public interface IBusinessEntity {
    int ID { get; set; }
}
```

Základní třída implementuje rozhraní a přidá atributy SQLite NET označit jako automaticky rostoucí primární klíč. Všechny třídy v obchodní vrstva, která implementuje tato základní třída může být nastavené jako trvalé v datové vrstvě:

```csharp
public abstract class BusinessEntityBase : IBusinessEntity {
    public BusinessEntityBase () {}
    [PrimaryKey, AutoIncrement]
    public int ID { get; set; }
}
```

To je například v datové vrstvě obecné metody, které používají rozhraní `GetItem<T>` metoda:

```csharp
public T GetItem<T> (int id) where T : BL.Contracts.IBusinessEntity, new ()
{
    lock (locker) {
        return Table<T>().FirstOrDefault(x => x.ID == id);
    }
}
```

 <a name="Locking_to_prevent_Concurrent_Access" />

#### <a name="locking-to-prevent-concurrent-access"></a>Aby se zabránilo souběžný přístup zamykání

A [zámku](http://msdn.microsoft.com/library/c5kehkcz(v=vs.100).aspx) je implementována v rámci `TaskItemDatabase` třídy, aby souběžný přístup k databázi. Tím je zajištěno serializován souběžný přístup z různých vláknech (jinak součást uživatelského rozhraní může pokus o čtení databáze ve stejnou dobu aktualizuje vlákna na pozadí). Příklad způsobu implementace zámek je uveden zde:

```csharp
static object locker = new object ();
public IEnumerable<T> GetItems<T> () where T : BL.Contracts.IBusinessEntity, new ()
{
    lock (locker) {
        return (from i in Table<T> () select i).ToList ();
    }
}
public T GetItem<T> (int id) where T : BL.Contracts.IBusinessEntity, new ()
{
    lock (locker) {
        return Table<T>().FirstOrDefault(x => x.ID == id);
    }
}
```

Většinu kódu Datová vrstva by bylo možné znovu v další projekty. Kód jenom konkrétní aplikace ve vrstvě `CreateTable<TaskItem>` volání v `TaskItemDatabase` konstruktor.

 <a name="Data_Access_Layer_(DAL)" />

### <a name="data-access-layer-dal"></a>Data Access Layer (DAL)

`TaskItemRepository` Třída zapouzdří mechanizmus pro ukládání dat s rozhraním API silného typu, který umožňuje `TaskItem` objekty, které chcete vytvořit, odstranit, načíst a aktualizovat.

 <a name="Using_Conditional_Compilation" />

#### <a name="using-conditional-compilation"></a>Pomocí Podmíněná kompilace

Třída používá Podmíněná kompilace nastavit umístění souboru – jedná se o příklad implementace odchylkami platformy. Vlastnost, která vrací cestu zkompiluje do různých kódu na jednotlivých platformách. Zde se zobrazují kód a direktivy kompilátoru specifických pro platformy:

```csharp
public static string DatabaseFilePath {
    get {
        var sqliteFilename = "TaskDB.db3";
#if SILVERLIGHT
        // Windows Phone expects a local path, not absolute
        var path = sqliteFilename;
#else
#if __ANDROID__
        // Just use whatever directory SpecialFolder.Personal returns
        string libraryPath = Environment.GetFolderPath(Environment.SpecialFolder.Personal); ;
#else
        // we need to put in /Library/ on iOS5.1+ to meet Apple's iCloud terms
        // (they don't want non-user-generated data in Documents)
        string documentsPath = Environment.GetFolderPath (Environment.SpecialFolder.Personal); // Documents folder
        string libraryPath = Path.Combine (documentsPath, "..", "Library"); // Library folder
#endif
        var path = Path.Combine (libraryPath, sqliteFilename);
                #endif
                return path;
    }
}
```

V závislosti na platformě, bude výstup "<app
path>/Library/TaskDB.db3" pro iOS, "<app
path>/Documents/TaskDB.db3" pro Android nebo právě "TaskDB.db3" pro Windows Phone.

### <a name="business-layer-bl"></a>Vrstvě podnikání (Blokovaných)

Obchodní vrstva implementuje třídy modelu a průčelí za k jejich správě.
V Tasky je Model `TaskItem` třídy a `TaskItemManager` implementuje vzor průčelí za poskytnout rozhraní API pro správu `TaskItems`.

 <a name="Façade" />

#### <a name="faade"></a>Průčelí za

 `TaskItemManager` zabalí `DAL.TaskItemRepository` zajistit Get, uložte a odstraňte metody, které se bude odkazovat prostřednictvím uživatelského rozhraní vrstvy a aplikaci.

Obchodní pravidla a logiku by se zde umístěny v případě potřeby – například všech ověřovacích pravidel, je nutné splnit před uložením objektu.

 <a name="API_for_Platform-Specific_Code" />

### <a name="api-for-platform-specific-code"></a>Rozhraní API pro kódu pro konkrétní platformu

Jakmile společný kód byl zapsán, uživatelské rozhraní musí vytvořeny tak, aby shromažďovat a zobrazovat data vystavené ho. `TaskItemManager` Třída implementuje vzor průčelí za zajistit jednoduché rozhraní API pro aplikace kód pro přístup.

Kód napsaný v každém projektu specifické pro platformu se obecně úzce párované nativní SDK těchto zařízení a přístup jenom k společný kód pomocí rozhraní API definované `TaskItemManager`. To zahrnuje metody a obchodní třídy ji vystavuje, jako například `TaskItem`.

Obrázky nejsou sdílet napříč platformami ale přidat nezávisle pro každý projekt. To je důležité, protože každou platformu zpracovává Image jiným způsobem, pomocí názvy různých souborů, adresářů a řešení.

Zbývající části popisují podrobnosti implementace specifické pro platformu Tasky uživatelského rozhraní.

 <a name="iOS_App" />

## <a name="ios-app"></a>Aplikace pro iOS

Existují jen někteří z nich potřebnou k implementaci iOS Tasky aplikace pomocí běžných PCL projektu pro uložení a načtení dat třídy. Projekt pro Xamarin.iOS dokončení iOS je zobrazena níže:

 ![](case-study-tasky-images/taskyios-solution.png "Zde je zobrazen projekt pro iOS")

Třídy jsou uvedeny v tomto diagramu seskupeny do vrstvy.

 [![](case-study-tasky-images/classdiagram-android.png "Třídy jsou uvedeny v tomto diagramu seskupeny do vrstvy")](case-study-tasky-images/classdiagram-android.png#lightbox)

 <a name="References" />

### <a name="references"></a>Odkazy

Aplikace pro iOS odkazuje na knihovny SDK specifické pro platformu – např. Xamarin.iOS a MonoTouch.Dialog-1.

Také musí odkazovat `TaskyPortableLibrary` PCL projektu.
Zobrazí seznam odkazů se zde:

 ![](case-study-tasky-images/taskyios-references.png "Zde je zobrazen seznam odkazů")

V tomto projektu pomocí tyto odkazy se implementují aplikační vrstvu a uživatelské rozhraní vrstvě.

 <a name="Application_Layer_(AL)" />

### <a name="application-layer-al"></a>Aplikační vrstvu (AL)

Aplikační vrstvu obsahuje třídy specifických pro platformy, které jsou vyžadovány pro vazbu objekty vystavené PCL do rozhraní. Specifické pro iOS aplikace má dvě třídy můžete zobrazit úlohy:

 -   **EditingSource** – Tato třída se používá k vytvoření vazby seznam úloh uživatelského rozhraní. Protože `MonoTouch.Dialog` byl použit pro seznam úkolů, musíme implementovat tohoto Pomocníka pro povolení funkce prstem odstranit v `UITableView` . Prstem odstranění je běžné na iOS, ale není Android nebo Windows Phone, tak, aby konkrétní projektu iOS jenom jeden, který implementuje ho.
 -   **TaskDialog** – Tato třída se používá k vytvoření vazby jeden úkol uživatelského rozhraní. Použije `MonoTouch.Dialog` rozhraní API reflexe ' zabalit ' `TaskItem` objekt s třídu, která obsahuje správné atributy, které se povolit vstupní obrazovky být správně naformátován.

`TaskDialog` Třídy používá `MonoTouch.Dialog` atributy vytvoření obrazovky založené na třídy a vlastnosti. Třída vypadá takto:

```csharp
public class TaskDialog {
    public TaskDialog (TaskItem task)
    {
        Name = task.Name;
        Notes = task.Notes;
        Done = task.Done;
    }
    [Entry("task name")]
    public string Name { get; set; }
    [Entry("other task info")]
    public string Notes { get; set; }
    [Entry("Done")]
    public bool Done { get; set; }
    [Section ("")]
    [OnTap ("SaveTask")]    // method in HomeScreen
    [Alignment (UITextAlignment.Center)]
    public string Save;
    [Section ("")]
    [OnTap ("DeleteTask")]  // method in HomeScreen
    [Alignment (UITextAlignment.Center)]
    public string Delete;
}
```

Upozornění `OnTap` atributy musí mít název metoda – těchto metod, musí existovat ve třídě, kde `MonoTouch.Dialog.BindingContext` se vytvoří (v tomto případě `HomeScreen` třídy, které jsou popsané v další části).

 <a name="User_Interface_Layer_(UI)" />

### <a name="user-interface-layer-ui"></a>Vrstva uživatelské rozhraní (UI)

Vrstva uživatelské rozhraní se skládá z následujících tříd:

1.   **AppDelegate** – obsahuje volání do rozhraní API vzhled k úpravě stylu písma a barvy používané v aplikaci. Tasky je jednoduchou aplikaci, nejsou žádné další úkoly inicializace spuštěné v `FinishedLaunching` .
2.   **Obrazovky** – měly podtřídy `UIViewController` které definují každý obrazovky a její chování. Shromáždit obrazovky uživatelského rozhraní pomocí třídy aplikační vrstvu a společné rozhraní API ( `TaskItemManager` ). V tomto příkladu se vytvoří obrazovky v kódu, ale jejich může být vytvořena pomocí Xcode na rozhraní tvůrce nebo návrháře storyboard.
3.   **Bitové kopie** – vizuální prvky jsou důležitou součástí každou aplikaci. Tasky má obrazovky a ikona úvodní Image, které je nutné zadat regulární a sítnice řešení pro iOS.

 <a name="Home_Screen" />

#### <a name="home-screen"></a>Domovskou obrazovku

Na obrazovce domů je `MonoTouch.Dialog` obrazovky, který zobrazí seznam úloh z databáze SQLite. Dědí z `DialogViewController` a implementuje kódu nastavit `Root` tak, aby obsahovala kolekce `TaskItem` objekty pro zobrazení.

 [![](case-study-tasky-images/ios-taskylist.png "Dědí z DialogViewController a implementuje kód pro nastavení kořenové tak, aby obsahovala kolekce objektů TaskItem pro zobrazení")](case-study-tasky-images/ios-taskylist.png#lightbox)

Dva hlavní metody týkající se zobrazení a interakci s seznamu úkolů jsou:

1.   **PopulateTable** – používá vrstvě podnikání `TaskManager.GetTasks` metoda pro načtení kolekce `TaskItem` objekty, které chcete zobrazit.
2.   **Vybraná** – Pokud je dotýkal řádek, na nové obrazovce zobrazí úlohy.

 <a name="Task_Details_Screen" />

#### <a name="task-details-screen"></a>Obrazovka podrobnosti úlohy

Podrobnosti úlohy je vstupní obrazovky, který umožňuje úloh, které se upravit ani odstranit.

Tasky používá `MonoTouch.Dialog`na rozhraní API reflexe zobrazíte na obrazovce se tedy žádné `UIViewController` implementace. Místo toho `HomeScreen` třídy vytvoří a zobrazí `DialogViewController` pomocí `TaskDialog` třídy z aplikační vrstvu.

Tento snímek obrazovky ukazuje prázdnou obrazovku, který ukazuje `Entry` atribut nastavení vodoznakového textu v **název** a **poznámky** pole:

 [![](case-study-tasky-images/ios-taskydetail.png "Tento snímek obrazovky ukazuje prázdnou obrazovku, která demonstruje atribut položku nastavení vodoznakového textu v polích pro název a poznámky")](case-study-tasky-images/ios-taskydetail.png#lightbox)

Funkce **podrobnosti úlohy** obrazovce (například uložení nebo odstranění úlohy) musí být implementován v `HomeScreen` třídy, protože to je, kdy `MonoTouch.Dialog.BindingContext` je vytvořena. Následující `HomeScreen` metody podporují obrazovce podrobnosti úlohy:

1.   **ShowTaskDetails** – vytvoří `MonoTouch.Dialog.BindingContext` k vykreslení na obrazovce. Vytvoří vstupní obrazovky pomocí reflexe načíst názvy vlastností a typy z `TaskDialog` třídy. Další informace, jako je například vodoznakového textu pro vstupních polí je implementováno s atributy ve vlastnostech.
2.   **SaveTask** – tato metoda odkazuje `TaskDialog` třídy prostřednictvím `OnTap` atribut. Je volána, když **Uložit** stisknutí a používá `MonoTouch.Dialog.BindingContext` načíst uživatelem zadaná data před uložením změn pomocí `TaskItemManager` .
3.   **DeleteTask** – tato metoda odkazuje `TaskDialog` třídy prostřednictvím `OnTap` atribut. Používá `TaskItemManager` odstranit data s využitím primárního klíče (vlastnost ID).

 <a name="Android_App" />

## <a name="android-app"></a>Aplikace pro Android

Dokončený projekt Xamarin.Android je na obrázku níže:

 ![](case-study-tasky-images/taskyandroid-solution.png "Projekt pro Android je zde obrázku")

Třída diagram s třídami, které jsou seskupené podle vrstvy:

 [![](case-study-tasky-images/classdiagram-android.png "Diagram třídy, s třídami, které jsou seskupené podle vrstvy")](case-study-tasky-images/classdiagram-android.png#lightbox)

 <a name="References" />

### <a name="references"></a>Odkazy

Projekt aplikace pro Android musí odkazovat sestavení Xamarin.Android specifické pro platformu třídám přístup ze sady SDK pro Android.

Také musí odkazovat na projekt PCL (např. TaskyPortableLibrary) pro přístup k společný kód dat a obchodní vrstvy.

 ![](case-study-tasky-images/taskyandroid-references.png "TaskyPortableLibrary pro přístup k společný kód dat a obchodní vrstvy")

 <a name="Application_Layer_(AL)" />

### <a name="application-layer-al"></a>Aplikační vrstvu (AL)

Podobně jako na verzi iOS, které jsme se podívali na dříve, aplikační vrstvě v systému Android verze obsahuje třídy specifických pro platformy, které jsou vyžadovány pro vazbu objekty vystavené základní do rozhraní.

 **TaskListAdapter** – Pokud chcete zobrazit seznam<T> objektů musíme implementovat adaptér pro vlastní objekty v zobrazení `ListView`. Adaptér Určuje, jaké rozložení se používá pro každou položku v seznamu – v tomto případě kód používá Android předdefinované rozložení `SimpleListItemChecked`.

 <a name="User_Interface_(UI)" />

### <a name="user-interface-ui"></a>Uživatelské rozhraní (UI)

Vrstva rozhraní Android aplikace uživatele je kombinací kód a kód XML.

 -   **Prostředky, rozvržení** – obrazovky rozložení a řádek buňka návrhu, které jsou implementované jako AXML soubory. AXML může být napsané ručně nebo umístěné na více systémů vizuálně pomocí uživatelského rozhraní návrháře Xamarin pro Android.
 -   **Prostředky/Drawable** – obrázků (ikon) a vlastní tlačítko.
 -   **Obrazovky** – aktivita podtříd definovat každý obrazovky a její chování. Sváže společně uživatelského rozhraní pomocí třídy aplikační vrstvu a společné rozhraní API (`TaskItemManager`).

 <a name="Home_Screen" />

#### <a name="home-screen"></a>Domovskou obrazovku

Na obrazovce Domů se skládá z podtřídy aktivity `HomeScreen` a `HomeScreen.axml` soubor, který definuje rozložení (umístění seznamu tlačítka a úloh). Na obrazovce vypadá takto:

 [![](case-study-tasky-images/android-taskylist.png "Na obrazovce vypadá takto")](case-study-tasky-images/android-taskylist.png#lightbox)

Kód Domů obrazovky definuje obslužné rutiny pro klepnutím na tlačítko a kliknutím na položky v seznamu, a také naplnění seznamu v `OnResume` – metoda (tak to odráží změny provedené v dialogovém okně Podrobnosti úlohy). Načtení dat pomocí obchodní vrstvy `TaskItemManager` a `TaskListAdapter` z aplikační vrstvu.

 <a name="Task_Details_Screen" />

#### <a name="task-details-screen"></a>Obrazovka podrobnosti úlohy

Obrazovce s podrobnostmi úloh se také skládá z `Activity` podtřídami a soubor AXML rozložení. Rozložení určuje umístění vstupní ovládací prvky a třída C# definuje chování k načtení a uložení `TaskItem` objekty.

 [![](case-study-tasky-images/android-taskydetail.png "Třída definuje chování k načtení a uložení TaskItem objekty")](case-study-tasky-images/android-taskydetail.png#lightbox)

Všechny odkazy na knihovny PCL jsou prostřednictvím `TaskItemManager` třídy.

 <a name="Windows_Phone_App" />

## <a name="windows-phone-app"></a>Aplikace Windows Phone
Dokončený projekt Windows Phone:

 ![](case-study-tasky-images/taskywp7-solution.png "Aplikace Windows Phone dokončený projekt Windows Phone")

Následující diagram představuje třídy seskupeny do vrstvy:

 [![](case-study-tasky-images/classdiagram-wp7.png "Tento diagram uvede třídy seskupeny do vrstvy")](case-study-tasky-images/classdiagram-wp7.png#lightbox)

 <a name="References" />

### <a name="references"></a>Odkazy

Projekt specifických pro platformy musí odkazovat na požadované knihovny specifických pro platformy (například `Microsoft.Phone` a `System.Windows`) k vytvoření platný aplikace Windows Phone.

Také musí odkazovat na projekt PCL (např. `TaskyPortableLibrary`) využívat `TaskItem` třídy a databáze.

 ![](case-study-tasky-images/taskywp7-references.png "TaskyPortableLibrary využívat TaskItem třídy a databáze")

 <a name="Application_Layer_(AL)" />

### <a name="application-layer-al"></a>Aplikační vrstvu (AL)

Znovu stejně jako u na iOS a Android verze aplikační vrstvu se skládá z nevizuálních elementy, které pomáhají při vytváření vazby dat s uživatelským rozhraním.

 <a name="ViewModels" />

#### <a name="viewmodels"></a>ViewModels

ViewModels zabalení data z PCL ( `TaskItemManager`) a uvede takovým způsobem, který mohou být spotřebovávána Silverlight/XAML datové vazby. Jedná se o příklad chování specifické pro platformu (jak je popsáno v dokumentu aplikací platformě).

 <a name="User_Interface_(UI)" />

### <a name="user-interface-ui"></a>Uživatelské rozhraní (UI)

XAML má jedinečné funkce vazby dat, které lze deklarovat v značek a snížit objem kód potřebný k zobrazení objektů:

1.   **Stránky** – soubory XAML a jejich codebehind definovat uživatelské rozhraní a odkazovat ViewModels a PCL projekt, zobrazit a shromažďovat data.
2.   **Bitové kopie** – úvodní obrazovka, pozadí a ikona Image jsou klíčovou součástí uživatelského rozhraní.

 <a name="MainPage" />

#### <a name="mainpage"></a>MainPage

Používá třída MainPage `TaskListViewModel` k zobrazení dat pomocí funkce XAML pro datové vazby. Na stránce `DataContext` je nastaven na modelu zobrazení, který je vyplněný asynchronně. `{Binding}` Syntaxi XAML Určuje, jak se zobrazí data.

 <a name="TaskDetailsPage" />

#### <a name="taskdetailspage"></a>TaskDetailsPage

Každý úkol se zobrazí při vytvoření vazby `TaskViewModel` do jazyka XAML definovaný v TaskDetailsPage.xaml. Úloha data se načítají prostřednictvím `TaskItemManager` v obchodní vrstvě.

 <a name="Results" />

## <a name="results"></a>Výsledky

Výsledná aplikace vypadat například takto na jednotlivých platformách:

 <a name="iOS" />

#### <a name="ios"></a>iOS

Aplikace používá návrh iOS standardní uživatelského rozhraní, jako je tlačítko "Přidat" se umístí na navigačním panelu a pomocí integrovaných **plus (+)** ikonu. Taky používá výchozí `UINavigationController` "zadní" tlačítko chování a podporuje 'prstem odstranit' v tabulce.

 [![](case-study-tasky-images/ios-taskylist.png "Také použije výchozí chování UINavigationController tlačítko Zpět a podporuje prstem odstranění v tabulce") ](case-study-tasky-images/ios-taskylist.png#lightbox) [ ![ ] (case-study-tasky-images/ios-taskylist.png "také používá výchozí UINavigationController chování tlačítka Zpět a podporuje prstem odstranění v tabulce")](case-study-tasky-images/ios-taskylist.png#lightbox)

 <a name="Android" />

#### <a name="android"></a>Android

Aplikace pro Android používá integrované ovládací prvky, včetně předdefinovaných rozložení řádků, které vyžadují 'značek, zobrazí. Chování back hardwaru nebo systému se podporuje kromě obrazovce back tlačítko.

 [![](case-study-tasky-images/android-taskylist.png "Chování back hardwaru nebo systému se podporuje kromě obrazovce back tlačítko")](case-study-tasky-images/android-taskylist.png#lightbox)[![](case-study-tasky-images/android-taskylist.png "back chování hardwaru nebo systému se podporuje kromě obrazovce tlačítko Zpět")](case-study-tasky-images/android-taskylist.png#lightbox)

 <a name="Windows_Phone" />

#### <a name="windows-phone"></a>Windows Phone

Aplikace Windows Phone používá standardní rozložení, vyplní panelu aplikace v dolní části obrazovky místo navigačního panelu v horní části.

 [![](case-study-tasky-images/wp-taskylist.png "Aplikace Windows Phone používá standardní rozložení, vyplní panelu aplikace v dolní části obrazovky místo navigačního panelu v horní části") ](case-study-tasky-images/wp-taskylist.png#lightbox) [ ![ ] (case-study-tasky-images/wp-taskylist.png "Windows Phone aplikace používá standardní rozložení, vyplní panelu aplikace v dolní části obrazovky místo navigačního panelu v horní části")](case-study-tasky-images/wp-taskylist.png#lightbox)

 <a name="Summary" />

## <a name="summary"></a>Souhrn

Tento dokument se poskytuje podrobné vysvětlení, jak se zásadami návrh vrstveného aplikace platí pro jednoduchou aplikaci pro usnadnění kódu opakovaného použití mezi tři mobilní platformy: iOS, Android a Windows Phone.

Má popisuje proces návrhu aplikace vrstvy a jaký kód popsané &amp; funkce nebyla implementovaná v každé vrstvě.

Kód si můžete stáhnout z [githubu](https://github.com/xamarin/mobile-samples/tree/master/TaskyPortable).

## <a name="related-links"></a>Související odkazy

- [Vytváření aplikací a platformy (hlavní dokument)](~/cross-platform/app-fundamentals/building-cross-platform-applications/index.md)
- [Tasky přenosné ukázkovou aplikaci (githubu)](https://github.com/xamarin/mobile-samples/tree/master/TaskyPortable)
