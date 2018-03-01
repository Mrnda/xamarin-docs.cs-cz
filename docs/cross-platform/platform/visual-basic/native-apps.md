---
title: Visual Basic.NET v Xamarin iOS a Android
description: "V tomto návod ukazuje, jak vytvářet nativní aplikace Xamarin.iOS a Xamarin.Android, které využívají obchodní logiky, které jsou napsané v Visual Basic.NET."
ms.topic: article
ms.prod: xamarin
ms.assetid: 455fda67-3879-4299-8036-b12840e6a498
ms.technology: xamarin-cross-platform
author: asb3993
ms.author: amburns
ms.date: 03/23/2017
ms.openlocfilehash: dc107ee865ea93cdc12148a5498cf3d512f1dae9
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 02/27/2018
---
# <a name="visual-basicnet-in-xamarin-ios-and-android"></a>Visual Basic.NET v Xamarin iOS a Android

[TaskyPortable](/samples/mobile/VisualBasic/TaskyPortableVB/) ukázkovou aplikaci ukazuje, jak kód jazyka Visual Basic zkompilovat do knihovny přenosných tříd může být použita s funkcí Xamarin. Tady jsou některé snímky obrazovky výsledná aplikace běžící v systému iOS, Android a Windows Phone:

 [ ![](native-apps-images/image5.png "iOS, Android a Windows telefony spuštění aplikace vytvořené s nástroji Visual Basic")](native-apps-images/image5.png)

IOS, Android a Windows Phone, které všechny projekty v příkladu jsou napsané v C#. Je uživatelské rozhraní pro jednotlivé aplikace vytvořené s nativní technologie (scénářů, Xml a jazyk Xaml v uvedeném pořadí), zatímco `TodoItem` správu zajišťuje knihovny přenosných tříd jazyka Visual Basic pomocí `IXmlStorage` implementace poskytovaná nativní projektu.

## <a name="sample-walkthrough"></a>Ukázka návod

Tato příručka popisuje, jak byl implementován jazyka Visual Basic v [TaskyPortableVB](https://github.com/xamarin/mobile-samples/tree/master/VisualBasic/TaskyPortableVB) ukázka Xamarin pro iOS a Android.

> ⚠️ Přečtěte si pokyny na [Visual PCLs pro](/guides/cross-platform/application_fundamentals/pcl/portable_visual_basic_net/) než budete pokračovat v této příručce.

## <a name="visualbasicportablelibrary"></a>VisualBasicPortableLibrary

Knihovny přenosných tříd jazyka Visual Basic lze vytvořit pouze v sadě Visual Studio.
Příklad knihovna obsahuje základní informace o naší aplikaci čtyři souborů Visual Basic:

-  IXmlStorage.vb
-  TodoItem.vb
-  TodoItemManager.vb
-  TodoItemRepositoryXML.vb


### <a name="ixmlstoragevb"></a>IXmlStorage.vb

Protože chování přístup k souboru, takže značně lišit mezi platformami, přenosné knihovny tříd neposkytují `System.IO` úložiště rozhraní API v žádný profil souborů. To znamená, pokud nám chcete pracovat přímo s systém souborů v našem kódu, přenosné, je potřeba na našem nativní projekty zpětné volání na každou platformu.  Vytvořením naše kód jazyka Visual Basic pro jednoduché rozhraní, které může být implementováno v jazyce C# na každou platformu můžete máme ke sdílení kód jazyka Visual Basic, který stále má přístup k systému souborů.

Ukázkový kód používá tento velmi jednoduché rozhraní, které obsahuje pouze dvě metody: ke čtení a zápisu serializovaného soubor Xml.

```vb
Public Interface IXmlStorage
    Function ReadXml(filename As String) As List(Of TodoItem)
    Sub WriteXml(tasks As List(Of TodoItem), filename As String)
End Interface
```

iOS, Android a Windows Phone implementace tohoto rozhraní se zobrazí dál v této příručce.

### <a name="todoitemvb"></a>TodoItem.vb

Tato třída obsahuje obchodní objekt, který chcete použít v celé aplikaci. Ho budou určené v jazyce Visual Basic a sdílet s iOS, Android a Windows Phone projekty, které jsou napsané v C#.

Zobrazí se zde definici třídy:

```vb
Public Class TodoItem
    Property ID() As Integer
    Property Name() As String
    Property Notes() As String
    Property Done() As Boolean
End Class
```

Příklad používá k načtení a uložení objektů TodoItem serializace XML a deaktivace serializace.

### <a name="todoitemmanagervb"></a>TodoItemManager.vb

Třída Manager uvede 'API' pro přenosné kód. Poskytuje základní operace CRUD pro `TodoItem` třídy, ale žádné implementace tyto operace.

```vb
Public Class TodoItemManager
    Private _repository As TodoItemRepositoryXML
    Public Sub New(filename As String, storage As IXmlStorage)
        _repository = New TodoItemRepositoryXML(filename, storage)
    End Sub
    Public Function GetTask(id As Integer) As TodoItem
        Return _repository.GetTask(id)
    End Function
    Public Function GetTasks() As List(Of TodoItem)
        Return New List(Of TodoItem)(_repository.GetTasks())
    End Function
    Public Function SaveTask(item As TodoItem) As Integer
        Return _repository.SaveTask(item)
    End Function
    Public Function DeleteTask(item As TodoItem) As Integer
        Return _repository.DeleteTask(item.ID)
    End Function
End Class
```

V konstruktoru instance IXmlStorage přebírá jako parametr. To umožňuje každou platformu a zadejte vlastní pracovní implementaci ale stále umožní kód přenosné popisují další funkce, které je možné sdílet.

### <a name="todoitemrepositoryvb"></a>TodoItemRepository.vb

Třída úložiště obsahuje logiku pro správu seznamu objektů TodoItem. Kód dokončení jsou uvedené dole – logiku existuje hlavně pro správu hodnotu jedinečné ID v TodoItems, jak jsou přidat a odebrat z kolekce.

```vb
Public Class TodoItemRepositoryXML
    Private _filename As String
    Private _storage As IXmlStorage
    Private _tasks As List(Of TodoItem)

    ''' <summary>Constructor</summary>
    Public Sub New(filename As String, storage As IXmlStorage)
        _filename = filename
        _storage = storage
        _tasks = _storage.ReadXml(filename)
    End Sub
    ''' <summary>Inefficient search for a Task by ID</summary>
    Public Function GetTask(id As Integer) As TodoItem
        For t As Integer = 0 To _tasks.Count - 1
            If _tasks(t).ID = id Then
                Return _tasks(t)
            End If
        Next
        Return New TodoItem() With {.ID = id}
    End Function
    ''' <summary>List all the Tasks</summary>
    Public Function GetTasks() As IEnumerable(Of TodoItem)
        Return _tasks
    End Function
    ''' <summary>Save a Task to the Xml file
    ''' Calculates the ID as the max of existing IDs</summary>
    Public Function SaveTask(item As TodoItem) As Integer
        Dim max As Integer = 0
        If _tasks.Count > 0 Then
            max = _tasks.Max(Function(t As TodoItem) t.ID)
        End If
        If item.ID = 0 Then
            item.ID = ++max
            _tasks.Add(item)
        Else
            Dim j = _tasks.Where(Function(t) t.ID = item.ID).First()
            j = item
        End If
        _storage.WriteXml(_tasks, _filename)
        Return max
    End Function
    ''' <summary>Removes the task from the XMl file</summary>
    Public Function DeleteTask(id As Integer) As Integer
        For t As Integer = 0 To _tasks.Count - 1
            If _tasks(t).ID = id Then
                _tasks.RemoveAt(t)
                _storage.WriteXml(_tasks, _filename)
                Return 1
            End If
        Next
        Return -1
    End Function
End Class
```

> ℹ️ **Poznámka:** tento kód je příkladem mechanismus velmi základní úložiště dat je.
> Je poskytována k předvedení toho, jak můžete přenosné knihovny tříd code proti rozhraní pro přístup k funkce specifické pro platformu (v tomto případě načítání a ukládání soubor Xml).
> Ho ho nemají být alternativní databáze produkční kvality.

## <a name="ios-android-and-windows-phone-application-projects"></a>iOS, Androidem a s projekty aplikací pro Windows Phone

Tato část obsahuje implementace specifické pro platformu pro rozhraní IXmlStorage a ukazuje, jak se používají v každé žádosti. Projekty aplikace jsou napsané v C#.

### <a name="ios-and-android-ixmlstorage"></a>iOS a Android IXmlStorage

Xamarin.iOS a Xamarin.Android poskytují úplné `System.IO` funkce vám umožní snadno načíst a uložte soubor Xml pomocí následující třídy:

```csharp
public class XmlStorageImplementation : IXmlStorage
{
    public XmlStorageImplementation(){}
    public List<TodoItem> ReadXml(string filename)
    {
        if (File.Exists(filename))
        {
            var serializer = new XmlSerializer(typeof(List<TodoItem>));
            using (var stream = new FileStream(filename, FileMode.Open))
            {
                return (List<TodoItem>)serializer.Deserialize(stream);
            }
        }
        return new List<TodoItem>();
    }
    public void WriteXml(List<TodoItem> tasks, string filename)
    {
        var serializer = new XmlSerializer(typeof(List<TodoItem>));
        using (var writer = new StreamWriter(filename))
        {
            serializer.Serialize(writer, tasks);
        }
    }
}
```

V aplikaci iOS `TodoItemManager` a `XmlStorageImplementation` jsou vytvořené v **AppDelegate.cs** souboru, jak ukazuje tento fragment kódu. První čtyři řádky právě vytváříte cestu k souboru, kde bude uložena data; Zobrazit poslední dva řádky dvou tříd vytváření instancí.

```csharp
var xmlFilename = "TodoList.xml";
string documentsPath = Environment.GetFolderPath(Environment.SpecialFolder.Personal); // Documents folder
string libraryPath = Path.Combine(documentsPath, "..", "Library"); // Library folder
var path = Path.Combine(libraryPath, xmlFilename);
var xmlStorage = new XmlStorageImplementation();
TaskMgr = new TodoItemManager(path, xmlStorage);
```

V aplikaci platformy Android `TodoItemManager` a `XmlStorageImplementation` jsou vytvořené v **Application.cs** souboru, jak ukazuje tento fragment kódu. První tři řádky právě vytváříte cestu k souboru, kde bude uložena data; Zobrazit poslední dva řádky dvou tříd vytváření instancí.

```csharp
var xmlFilename = "TodoList.xml";
string libraryPath = Environment.GetFolderPath(Environment.SpecialFolder.Personal);
var path = Path.Combine(libraryPath, xmlFilename);
var xmlStorage = new AndroidTodo.XmlStorageImplementation();
TaskMgr = new TodoItemManager(path, xmlStorage);
```

Zbytek kódu aplikace je primárně nevadí uživatelské rozhraní a pomocí `TaskMgr` třídy k načtení a uložení `TodoItem` třídy.

### <a name="windows-phone-ixmlstorage"></a>Windows Phone IXmlStorage

Windows Phone neposkytuje úplný přístup k systému souborů zařízení, místo toho vystavení IsolatedStorage rozhraní API. `IXmlStorage` Implementace pro Windows Phone vypadá takto:

```csharp
public class XmlStorageImplementation : IXmlStorage
{
    public XmlStorageImplementation(){}
    public List<TodoItem> ReadXml(string filename)
    {
        IsolatedStorageFile fileStorage = IsolatedStorageFile.GetUserStoreForApplication();
        if (fileStorage.FileExists(filename))
        {
            var serializer = new XmlSerializer(typeof(List<TodoItem>));
            using (var stream = new StreamReader(new IsolatedStorageFileStream(filename, FileMode.Open, fileStorage)))
            {
                return (List<TodoItem>)serializer.Deserialize(stream);
            }
        }
        return new List<TodoItem>();
    }
    public void WriteXml(List<TodoItem> tasks, string filename)
    {
        IsolatedStorageFile fileStorage = IsolatedStorageFile.GetUserStoreForApplication();
        var serializer = new XmlSerializer(typeof(List<TodoItem>));
        using (var writer = new StreamWriter(new IsolatedStorageFileStream(filename, FileMode.OpenOrCreate, fileStorage)))
        {
            serializer.Serialize(writer, tasks);
        }
    }
}
```

`TodoItemManager` a `XmlStorageImplementation` jsou vytvořené v **App.xaml.cs** souboru, jak ukazuje tento fragment kódu.

```csharp
var filename = "TodoList.xml";
var xmlStorage = new XmlStorageImplementation();
TodoMgr = new TodoItemManager(filename, xmlStorage);
```

Zbývající aplikace Windows Phone se skládá z Xaml a C# k vytvoření uživatelského rozhraní a používat `TodoMgr` třídy k načtení a uložení `TodoItem` objekty.

# <a name="visual-basic-pcl-in-visual-studio-for-mac"></a>Visual Basic PCL v sadě Visual Studio pro Mac

Visual Studio pro Mac nepodporuje jazyk Visual Basic – nelze vytvořit nebo zkompilovat projekty Visual Basic pomocí sady Visual Studio for Mac.

Visual Studio pro Mac na podporu pro knihovny přenosných tříd znamená, že ho můžete odkazovat PCL sestavení, které byly vytvořeny z jazyka Visual Basic.

Tato část vysvětluje, jak pro kompilaci PCL sestavení v sadě Visual Studio a pak se ujistěte, že bude uložen v systému správy verzí a odkazují jiné projekty.

## <a name="keeping-the-pcl-output-from-visual-studio"></a>Zachování PCL výstup ze sady Visual Studio

Ve výchozím nastavení bude ignorovat nakonfigurován většina systémů řízení verze (včetně sady TFS a Git) **/bin/** adresáře, což znamená kompilované PCL sestavení se neuloží. To znamená, že by musíte ručně zkopírovat do všech počítačů se systémem Visual Studio pro Mac se přidat odkaz na něj.

K zajištění, že váš systém správy verzí můžete uložit výstup sestavení PCL, můžete vytvořit skript po sestavení, který se zkopíruje do kořenu projektu. Tento krok po sestavení pomáhá zajistit sestavení lze snadno přidat do správy zdrojového kódu a sdílet s jinými projekty.

### <a name="visual-studio-2017"></a>Visual Studio 2017

1. Klikněte pravým tlačítkem na projekt a zvolte **vlastnosti > události sestavení** části.

2. Přidat _po sestavení_ skript, který kopíruje výstupní knihovnu DLL z tohoto projektu do kořenového adresáře projektu (což je mimo **/bin/**). V závislosti na konfiguraci řízení verze knihovny DLL teď by mohli být přidán do správy zdrojového kódu.

  [ ![](native-apps-images/image6-vs-sml.png "Události sestavení skriptu buildu post zkopírovat knihovnu DLL jazyka Visual Basic")](native-apps-images/image6-vs.png)

### <a name="visual-studio-2015"></a>Visual Studio 2015

1.  Klikněte pravým tlačítkem na projekt a zvolte **vlastnosti > zkompilovat** , pak zkontrolujte všechny konfigurace je vybraný v levém horním hřeben poli. Klikněte **události sestavení...**  tlačítka na vpravo dole.

  [ ![](native-apps-images/image6.png "V části Vlastnosti kompilace projektu")](native-apps-images/image6.png)

1.  Přidejte skript po sestavení, který kopíruje výstupní knihovnu DLL z tohoto projektu do kořenového adresáře projektu (což je mimo **/bin/** ). V závislosti na konfiguraci řízení verze knihovny DLL teď by mohli být přidán do správy zdrojového kódu.

  [ ![](native-apps-images/image7.png "Okno události sestavení")](native-apps-images/image7.png)

### <a name="all-versions"></a>Všechny verze

Příště sestavení projektu, sestavení přenosné knihovny tříd se zkopírují do kořenového adresáře projektu a kontrola v a potvrzení/nabízenou změny knihovnou DLL budete uložené (tak, aby ho můžete stáhnout na Mac pomocí sady Visual Studio pro Mac).

  [ ![](native-apps-images/image8-sml.png "Umístění souboru Visual Basic sestavení výstupu")](native-apps-images/image8.png)


Toto sestavení potom přidáním do Xamarin projektů v sadě Visual Studio pro Mac, i když samotné jazyka Visual Basic nepodporuje Xamarin iOS nebo Android projekty.

## <a name="referencing-the-pcl-in-visual-studio-for-mac"></a>Odkazování na PCL v sadě Visual Studio pro Mac

Protože Xamarin jazyka Visual Basic nepodporuje ho nelze načíst projekt PCL (ani aplikace Windows Phone), jak je vidět na tomto snímku obrazovky:

 [ ![](native-apps-images/image9.png "Visual Studio pro Mac řešení")](native-apps-images/image9.png)

V projektech Xamarin.iOS a Xamarin.Android jsme stále může zahrnovat sestavení jazyka Visual Basic PCL knihovny DLL:

1.  Klikněte pravým tlačítkem na **odkazy** uzel a vyberte možnost **upravit odkazy...**

  [ ![](native-apps-images/image10.png "Projekt upravit odkazy nabídky")](native-apps-images/image10.png)

1.  Vyberte **.Net sestavení** kartě a přejděte do výstupní knihovnu DLL v adresáři projektu jazyka Visual Basic. Přestože projekt nelze otevřít v sadě Visual Studio pro Mac, všechny soubory musí být došlo od správy zdrojového kódu. Klikněte na tlačítko **přidat** pak **OK** přidat toto sestavení pro iOS a Android aplikace.

  [ ![](native-apps-images/image11-sml.png "Klikněte na tlačítko Přidat pak OK přidat toto sestavení pro iOS a Android aplikace")](native-apps-images/image11.png)

1.  IOS a Android aplikace můžete nyní zahrnují aplikační logiku poskytované knihovny přenosných tříd jazyka Visual Basic. Tento snímek obrazovky ukazuje aplikace pro iOS, která odkazuje na PCL Visual Basic a má kód, který používá funkce z této knihovny.

  [ ![](native-apps-images/image12-sml.png "Upravit odkazy přidejte okno sestavení rozhraní .NET")](native-apps-images/image12.png)


Pokud provedete změny do projektu jazyka Visual Basic v sadě Visual Studio nezapomeňte sestavení projektu, uložte výsledné sestavení knihovny DLL ve správě zdrojového kódu a pak pro vyžádání obsahu této nová knihovna DLL od správy zdrojového kódu na počítači Mac, aby sestavení v sadě Visual Studio pro Mac obsahují nejnovější funkce.


## <a name="summary"></a>Souhrn

Tento článek vám ukázal, jak využívat kód jazyka Visual Basic v aplikacích Xamarin pomocí sady Visual Studio a knihovny přenosných tříd. I když Xamarin jazyka Visual Basic nepodporuje přímo, kompilace jazyka Visual Basic do PCL umožňuje kód napsaný v jazyce Visual Basic má být zahrnut v iOS a Android.

## <a name="related-links"></a>Související odkazy

- [TaskyPortableVB (ukázka)](https://github.com/xamarin/mobile-samples/tree/master/VisualBasic/TaskyPortableVB)
- [Vývoj pro různé platformy s rozhraním .NET Framework (Microsoft)](http://msdn.microsoft.com/en-us/library/gg597391(v=vs.110).aspx)
