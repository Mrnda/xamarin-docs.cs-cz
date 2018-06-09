---
title: Databáze Microsoft Xamarin.Forms
description: Xamarin.Forms podporuje aplikací řízené databázi pomocí SQLite databázový stroj, takže je možné načíst objekty a uložit v sdíleného kódu. Tento článek popisuje, jak můžete Xamarin.Forms aplikace čtení a zápisu dat do místní databáze SQLite pomocí SQLite.Net.
ms.prod: xamarin
ms.assetid: F687B24B-7DF0-4F8E-A21A-A9BB507480EB
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 05/31/2018
ms.openlocfilehash: 91df4d36dd8d98712063a30773f927a82676b18e
ms.sourcegitcommit: 66682dd8e93c0e4f5dee69f32b5fc5a96443e307
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 06/08/2018
ms.locfileid: "35243607"
---
# <a name="xamarinforms-local-databases"></a>Databáze Microsoft Xamarin.Forms

_Xamarin.Forms podporuje aplikací řízené databázi pomocí SQLite databázový stroj, takže je možné načíst objekty a uložit v sdíleného kódu. Tento článek popisuje, jak můžete Xamarin.Forms aplikace čtení a zápisu dat do místní databáze SQLite pomocí SQLite.Net._

## <a name="overview"></a>Přehled

Xamarin.Forms aplikace můžete použít [SQLite.NET PCL NuGet](https://www.nuget.org/packages/sqlite-net-pcl/) balíček začlenit databázových operací do sdíleného kódu odkazem `SQLite` třídy, které se dodávají v NuGet. Databázové operace může být definován v .NET Standard projektu knihovny řešení Xamarin.Forms s projekty specifické pro platformu vrácení cestu k uložení databáze.

Doprovodných [ukázkové aplikace](https://github.com/xamarin/xamarin-forms-samples/tree/master/Todo) je jednoduchou aplikaci seznamu úkolů. Tyto snímky obrazovky ukazují, jak ukázka zobrazuje na jednotlivých platformách:

[![Snímky obrazovky příklad databáze Xamarin.Forms](databases-images/todo-list-sml.png "TodoList první stránka snímky")](databases-images/todo-list.png#lightbox "TodoList první stránka snímky") [ ![ Snímky obrazovky příklad databáze Xamarin.Forms](databases-images/todo-list-sml.png "TodoList první stránka snímky")](databases-images/todo-list.png#lightbox "TodoList snímky obrazovky první stránka")

<a name="Using_SQLite_with_PCL" />

## <a name="using-sqlite"></a>Pomocí SQLite

V této části ukazuje, jak přidat balíčky SQLite.Net NuGet řešení Xamarin.Forms, zapisovat metody k provedení operace databáze a použít [ `DependencyService` ](~/xamarin-forms/app-fundamentals/dependency-service/index.md) k určení umístění pro uložení databázi na každou platformu.

<a name="XamarinForms_PCL_Project" />

### <a name="xamarinsforms-net-standard-or-pcl-project"></a>Xamarins.Forms .NET Standard nebo PCL projektu

Chcete-li přidat podporu SQLite do projektu Xamarin.Forms, pomocí funkce vyhledávání NuGet najděte **sqlite. net pcl** a instalaci balíčku:

![Přidejte balíček NuGet SQLite.NET PCL](databases-images/vs2017-sqlite-pcl-nuget.png "přidejte balíček NuGet SQLite.NET PCL")

Existuje několik balíčků NuGet s podobnými názvy, správný balíček s těmito atributy:

- **Autor:** Krueger František A.
- **ID:** sqlite. net pcl
- **Odkaz NuGet:** [sqlite. net pcl](https://www.nuget.org/packages/sqlite-net-pcl/)

> [!TIP]
> Použití **sqlite. net pcl** NuGet i v rozhraní .NET standardní projekty.

Po přidání odkazu zápisu rozhraní abstrahovat funkce specifické pro platformu, která je k určení umístění souboru databáze. Rozhraní používá v ukázce definuje jednu metodu:

```csharp
public interface IFileHelper
{
  string GetLocalFilePath(string filename);
}
```

Po definování rozhraní, použijte [ `DependencyService` ](~/xamarin-forms/app-fundamentals/dependency-service/index.md) získat implementace a získat cestu k souboru místní (Všimněte si, že ještě nebyla implementována toto rozhraní). Následující kód získá implementace v `App.Database` vlastnost:

```csharp
static TodoItemDatabase database;

public static TodoItemDatabase Database
{
  get
  {
    if (database == null)
    {
      database = new TodoItemDatabase(DependencyService.Get<IFileHelper>().GetLocalFilePath("TodoSQLite.db3"));
    }
    return database;
  }
}
```

`TodoItemDatabase` Konstruktor je zobrazena níže:

```csharp
public TodoItemDatabase(string dbPath)
{
  database = new SQLiteAsyncConnection(dbPath);
  database.CreateTableAsync<TodoItem>().Wait();
}
```

Tento postup vytvoří připojení jedné databáze, které se ukládají otevřete, když je aplikace spuštěná, proto zabraňující nákladů na otvírání a zavírání souborů databáze pokaždé, když probíhá operace databáze.

Zbývající část `TodoItemDatabase` třída obsahuje SQLite dotazy, které spustit napříč platformami. Příklad dotazu kód je uveden níže (Další informace o syntaxi najdete v [pomocí SQLite.NET](~/cross-platform/app-fundamentals/index.md) článek):

```csharp
public Task<List<TodoItem>> GetItemsAsync()
{
  return database.Table<TodoItem>().ToListAsync();
}

public Task<List<TodoItem>> GetItemsNotDoneAsync()
{
  return database.QueryAsync<TodoItem>("SELECT * FROM [TodoItem] WHERE [Done] = 0");
}

public Task<TodoItem> GetItemAsync(int id)
{
  return database.Table<TodoItem>().Where(i => i.ID == id).FirstOrDefaultAsync();
}

public Task<int> SaveItemAsync(TodoItem item)
{
  if (item.ID != 0)
  {
    return database.UpdateAsync(item);
  }
  else {
    return database.InsertAsync(item);
  }
}

public Task<int> DeleteItemAsync(TodoItem item)
{
  return database.DeleteAsync(item);
}
```

> [!NOTE]
> Výhodou použití asynchronní SQLite.Net API je této databáze, kterou operace přesunou do vlákna na pozadí. Kromě toho není třeba zapsat další souběžnosti kód zpracování, protože rozhraní API postará ho.

Všechny přístupový kód dat je napsána v projektu PCL ke sdílení pro všechny platformy. Jenom získávání místní cesta pro databáze vyžaduje kódu pro konkrétní platformu, jak je uvedeno v následující části.

<a name="PCL_iOS" />

### <a name="ios-project"></a>iOS projektu

Konfigurace aplikace iOS, přidejte jeden balíček NuGet do projektu iOS pomocí *NuGet* okno:

![Přidejte balíček NuGet SQLite.NET PCL](databases-images/vsmac-sqlite-nuget.png "přidejte balíček NuGet SQLite.NET PCL")

Vyžaduje pouze kód je `IFileHelper` implementace, která určuje cestu k souboru data. Následující kód umístí soubor databáze SQLite **knihovny nebo databází** složku v rámci izolovaného prostoru aplikace. Najdete v článku [iOS práce pomocí systému souborů](~/ios/app-fundamentals/file-system.md) dokumentace pro další informace o různých adresáře, které jsou k dispozici pro úložiště.

```csharp
[assembly: Dependency(typeof(FileHelper))]
namespace Todo.iOS
{
  public class FileHelper : IFileHelper
  {
    public string GetLocalFilePath(string filename)
    {
      string docFolder = Environment.GetFolderPath(Environment.SpecialFolder.Personal);
      string libFolder = Path.Combine(docFolder, "..", "Library", "Databases");

      if (!Directory.Exists(libFolder))
      {
        Directory.CreateDirectory(libFolder);
      }

      return Path.Combine(libFolder, filename);
    }
  }
}
```

Všimněte si, že tento kód obsahuje `assembly:Dependency` atributů tak, aby tato implementace zjistitelný pomocí `DependencyService`.

<a name="PCL_Android" />

### <a name="android-project"></a>Projekt pro Android

Konfigurace aplikace pro Android, přidat jeden balíček NuGet do projektu pro Android pomocí *NuGet* okno:

![](databases-images/vsmac-sqlite-nuget.png "Přidejte balíček NuGet SQLite.NET PCL")

Po přidání tohoto odkazu se vyžaduje jenom kód `IFileHelper` implementace, která určuje cestu k souboru data.

```csharp
[assembly: Dependency(typeof(FileHelper))]
namespace Todo.Droid
{
  public class FileHelper : IFileHelper
  {
    public string GetLocalFilePath(string filename)
    {
        string path = Environment.GetFolderPath(Environment.SpecialFolder.Personal);
        return Path.Combine(path, filename);
    }
  }
}
```

<a name="PCL_UWP" />

### <a name="windows-10-universal-windows-platform-uwp"></a>Windows 10 univerzální platformu Windows (UWP)

Konfigurace aplikace pro UPW, přidejte jeden balíček NuGet do projektu UPW pomocí *NuGet* okno:

![Přidejte balíček NuGet SQLite.NET PCL](databases-images/vs2017-sqlite-uwp-nuget.png "přidejte balíček NuGet SQLite.NET PCL")

Po přidání odkaz na implementaci `IFileHelper` rozhraní, pomocí konkrétní platformy `Windows.Storage` rozhraní API určit cestu k souboru data.

```csharp
using Windows.Storage;
...

[assembly: Dependency(typeof(FileHelper))]
namespace Todo.UWP
{
  public class FileHelper : IFileHelper
  {
    public string GetLocalFilePath(string filename)
    {
      return Path.Combine(ApplicationData.Current.LocalFolder.Path, filename);
    }
  }
}
```

## <a name="summary"></a>Souhrn

Xamarin.Forms podporuje aplikací řízené databázi pomocí SQLite databázový stroj, takže je možné načíst objekty a uložit v sdíleného kódu.

Tento článek zaměřuje na **přístup k** pomocí Xamarin.Forms databáze SQLite. Další informace o práci s SQLite.Net sám sebe, najdete v části [SQLite.NET v systému Android](~/android/data-cloud/data-access/using-sqlite-orm.md) nebo [SQLite.NET v systému iOS](~/ios/data-cloud/data/using-sqlite-orm.md) dokumentaci. Většinu kódu SQLite.Net je lze sdílet v rámci všech platformách; pouze konfiguraci umístění souboru databáze SQLite vyžaduje funkce specifické pro platformu.

## <a name="related-links"></a>Související odkazy

- [Ukázka todo](https://developer.xamarin.com/samples/xamarin-forms/Todo/)
- [Ukázky Xamarin.Forms](https://developer.xamarin.com/samples/xamarin-forms/all/)
- [Sešit databáze](https://developer.xamarin.com/workbooks/xamarin-forms/application-fundamentals/database/database.workbook)
