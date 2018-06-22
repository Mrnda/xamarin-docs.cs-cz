---
title: Databáze Microsoft Xamarin.Forms
description: Xamarin.Forms podporuje aplikací řízené databázi pomocí SQLite databázový stroj, takže je možné načíst objekty a uložit v sdíleného kódu. Tento článek popisuje, jak můžete Xamarin.Forms aplikace čtení a zápisu dat do místní databáze SQLite pomocí SQLite.Net.
ms.prod: xamarin
ms.assetid: F687B24B-7DF0-4F8E-A21A-A9BB507480EB
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 06/21/2018
ms.openlocfilehash: feec4993a0719a083d713e084552b18aead8ee42
ms.sourcegitcommit: eac092f84b603958c761df305f015ff84e0fad44
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 06/21/2018
ms.locfileid: "36310137"
---
# <a name="xamarinforms-local-databases"></a>Databáze Microsoft Xamarin.Forms

_Xamarin.Forms podporuje aplikací řízené databázi pomocí SQLite databázový stroj, takže je možné načíst objekty a uložit v sdíleného kódu. Tento článek popisuje, jak můžete Xamarin.Forms aplikace čtení a zápisu dat do místní databáze SQLite pomocí SQLite.Net._

## <a name="overview"></a>Přehled

Xamarin.Forms aplikace můžete použít [SQLite.NET PCL NuGet](https://www.nuget.org/packages/sqlite-net-pcl/) balíček začlenit databázových operací do sdíleného kódu odkazem `SQLite` třídy, které se dodávají v NuGet. Databázové operace lze definovat v projektu knihovny .NET standardní řešení Xamarin.Forms.

Doprovodných [ukázkové aplikace](https://github.com/xamarin/xamarin-forms-samples/tree/master/Todo) je jednoduchou aplikaci seznamu úkolů. Tyto snímky obrazovky ukazují, jak ukázka zobrazuje na jednotlivých platformách:

[![Snímky obrazovky příklad databáze Xamarin.Forms](databases-images/todo-list-sml.png "TodoList první stránka snímky")](databases-images/todo-list.png#lightbox "TodoList první stránka snímky") [ ![ Snímky obrazovky příklad databáze Xamarin.Forms](databases-images/todo-list-sml.png "TodoList první stránka snímky")](databases-images/todo-list.png#lightbox "TodoList snímky obrazovky první stránka")

<a name="Using_SQLite_with_PCL" />

## <a name="using-sqlite"></a>Pomocí SQLite

Přidání podpory SQLite do Xamarin.Forms .NET standardní knihovny, pomocí funkce vyhledávání NuGet najít **sqlite. net pcl** a instalovat nejnovější balíček:

![Přidejte balíček NuGet SQLite.NET PCL](databases-images/vs2017-sqlite-pcl-nuget.png "přidejte balíček NuGet SQLite.NET PCL")

Existuje několik balíčků NuGet s podobnými názvy, správný balíček s těmito atributy:

- **Autor:** Krueger František A.
- **ID:** sqlite. net pcl
- **Odkaz NuGet:** [sqlite. net pcl](https://www.nuget.org/packages/sqlite-net-pcl/)

> [!NOTE]
> Bez ohledu název balíčku, použít **sqlite. net pcl** balíček NuGet i v rozhraní .NET standardní projekty.

Po přidání odkaz na přidání vlastnosti do `App` třídu, která vrací cestu místního souboru pro ukládání databáze:

```csharp
static TodoItemDatabase database;

public static TodoItemDatabase Database
{
  get
  {
    if (database == null)
    {
      database = new TodoItemDatabase(
        Path.Combine(Environment.GetFolderPath(Environment.SpecialFolder.LocalApplicationData), "TodoSQLite.db3"));
    }
    return database;
  }
}
```

`TodoItemDatabase` Konstruktor, který přebírá cesta k souboru databáze jako argument, jsou uvedeny níže:

```csharp
public TodoItemDatabase(string dbPath)
{
  database = new SQLiteAsyncConnection(dbPath);
  database.CreateTableAsync<TodoItem>().Wait();
}
```

Výhodou vystavení databáze, jako je typu singleton vytvořený připojení jedné databáze, který je uložen otevřete při aplikace běží, se provádí proto zabraňující nákladů na otvírání a zavírání souborů databáze pokaždé, když operace databáze.

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

## <a name="summary"></a>Souhrn

Xamarin.Forms podporuje aplikací řízené databázi pomocí SQLite databázový stroj, takže je možné načíst objekty a uložit v sdíleného kódu.

Tento článek zaměřuje na **přístup k** pomocí Xamarin.Forms databáze SQLite. Další informace o práci s SQLite.Net sám sebe, najdete v části [SQLite.NET v systému Android](~/android/data-cloud/data-access/using-sqlite-orm.md) nebo [SQLite.NET v systému iOS](~/ios/data-cloud/data/using-sqlite-orm.md) dokumentaci.

## <a name="related-links"></a>Související odkazy

- [Ukázka todo](https://developer.xamarin.com/samples/xamarin-forms/Todo/)
- [Ukázky Xamarin.Forms](https://developer.xamarin.com/samples/xamarin-forms/all/)
- [Sešit databáze](https://developer.xamarin.com/workbooks/xamarin-forms/application-fundamentals/database/database.workbook)
