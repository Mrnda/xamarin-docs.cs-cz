---
title: Synchronizace Offline dat s Azure Mobile Apps
description: Offline synchronizace umožňuje uživatelům interakci s mobilních aplikací, zobrazení, přidání nebo úprava dat, i tam, kde není k dispozici síťové připojení. Změny se ukládají do místní databáze, a když je zařízení online, změny mohou být synchronizovány s Azure Mobile Apps instancí. Tento článek vysvětluje, jak přidat funkci offline synchronizace Xamarin.Forms aplikace.
ms.prod: xamarin
ms.assetid: DBB343B0-2709-4C20-A669-5522B9956D9B
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 10/02/2017
ms.openlocfilehash: 8623127444836d3335f42f9ba7a40de6aedfef70
ms.sourcegitcommit: d80d93957040a14b4638a91b0eac797cfaade840
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 06/07/2018
ms.locfileid: "34848366"
---
# <a name="synchronizing-offline-data-with-azure-mobile-apps"></a>Synchronizace Offline dat s Azure Mobile Apps

_Offline synchronizace umožňuje uživatelům interakci s mobilních aplikací, zobrazení, přidání nebo úprava dat, i tam, kde není k dispozici síťové připojení. Změny se ukládají do místní databáze, a když je zařízení online, změny mohou být synchronizovány s Azure Mobile Apps instancí. Tento článek vysvětluje, jak přidat funkci offline synchronizace Xamarin.Forms aplikace._

## <a name="overview"></a>Přehled

[Azure Mobile Client SDK](https://www.nuget.org/packages/Microsoft.Azure.Mobile.Client/) poskytuje `IMobileServiceTable` rozhraní, což představuje operace, které lze provést u tabulek uložených v instanci Azure Mobile Apps. Tyto operace připojit přímo k instanci Azure Mobile Apps a se nezdaří, pokud mobilní zařízení nemá připojení k síti.

Pro podporu offline synchronizace, Azure Mobile Client SDK podporuje *synchronizovat tabulky*, které jsou poskytovány `IMobileServiceSyncTable` rozhraní. Toto rozhraní poskytuje stejné vytvořit, číst, aktualizovat, odstranit operace jako `IMobileServiceTable` rozhraní, ale operace číst nebo zapisovat do místního úložiště. Místní úložiště není naplněný nová data z instance Azure Mobile Apps, dokud nebude volání *vyžádání* data. Podobně se data neodesílají do instance Azure Mobile Apps dokud nebude volání *nabízené* místní změny.

Offline synchronizace zahrnuje taky podporu pro zjišťování konfliktů při stejné záznamu došlo ke změně v místním úložišti a v instanci Azure Mobile Apps a řešení konfliktů vlastní. Konflikty může být zpracován, buď v místním úložišti, nebo v instanci Azure Mobile Apps.

Další informace o offline synchronizace najdete v tématu [Offline synchronizací dat v Azure Mobile Apps](/azure/app-service-mobile/app-service-mobile-offline-data-sync/) a [zapnutí offline synchronizace pro mobilní aplikaci Xamarin.Forms](/azure/app-service-mobile/app-service-mobile-xamarin-forms-get-started-offline-data/).

## <a name="setup"></a>Instalace

Proces pro integraci offline synchronizace mezi aplikací Xamarin.Forms a instanci Azure Mobile Apps je následující:

1. Vytvoření instance Azure Mobile Apps. Další informace najdete v tématu [využívání služby Azure Mobile Apps](~/xamarin-forms/data-cloud/consuming/azure.md).
1. Přidat [Microsoft.Azure.Mobile.Client.SQLiteStore](https://www.nuget.org/packages/Microsoft.Azure.Mobile.Client.SQLiteStore/) balíček NuGet, abyste všechny projekty v řešení Xamarin.Forms.
1. (Volitelné) Povolení ověřování v Azure Mobile Apps instance a aplikaci Xamarin.Forms. Další informace najdete v tématu [ověřování uživatelů s Azure Mobile Apps](~/xamarin-forms/data-cloud/authentication/azure.md).

V následujícím oddílu najdete další instalační pokyny ke konfiguraci univerzální platformu Windows (UWP) projekty k použití balíčku Microsoft.Azure.Mobile.Client.SQLiteStore NuGet. Žádné další nastavení, je potřeba použít balíček Microsoft.Azure.Mobile.Client.SQLiteStore NuGet na iOS a Android.

### <a name="universal-windows-platform"></a>Univerzální platforma pro Windows

SQLite na univerzální platformu Windows (UWP), postupujte podle těchto kroků:

1. Nainstalujte [SQLite pro univerzální platformu Windows](http://sqlite.org/2016/sqlite-uwp-3120200.vsix) rozšíření sady Visual Studio ve vašem vývojovém prostředí.
1. V projektu UPW v sadě Visual Studio, klikněte pravým tlačítkem **odkazy > Přidat odkaz na**, přejděte na **rozšíření** a přidejte **SQLite pro univerzální platformu Windows** a  **Visual C++ 2015 Runtime pro Universal Windows Platform Apps** rozšíření projektu UPW.

## <a name="initializing-the-local-store"></a>Inicializace místní úložiště

Místní úložiště musí inicializovat před provedením jakékoli operace s tabulkou synchronizace. Dosahuje se v projektu knihovny přenosných – třída (PCL) řešení Xamarin.Forms:

```csharp
using Microsoft.WindowsAzure.MobileServices;
using Microsoft.WindowsAzure.MobileServices.SQLiteStore;
using Microsoft.WindowsAzure.MobileServices.Sync;

namespace TodoAzure
{
    public partial class TodoItemManager
    {
        static TodoItemManager defaultInstance = new TodoItemManager();
        IMobileServiceClient client;
        IMobileServiceSyncTable<TodoItem> todoTable;

        private TodoItemManager()
        {
            this.client = new MobileServiceClient(Constants.ApplicationURL);
            var store = new MobileServiceSQLiteStore("localstore.db");
            store.DefineTable<TodoItem>();
            this.client.SyncContext.InitializeAsync(store);
            this.todoTable = client.GetSyncTable<TodoItem>();
        }
        ...
  }
}
```

Je novou místní databázi SQLite vytvořené `MobileServiceSQLiteStore` třídy, za předpokladu, že ještě neexistuje. Potom, `DefineTable<T>` metoda vytvoří tabulku v místním úložišti, který odpovídá polí v `TodoItem` zadejte, za předpokladu, že ještě neexistuje.

A *kontext synchronizace* je přidružen `MobileServiceClient` instance a sleduje změny, které se provedou synchronizace tabulky. Kontext synchronizace udržuje fronty, zajišťuje uspořádaný seznam operací vytvoření, aktualizace a odstranění (vytvoření), který se k instanci Azure Mobile Apps odesílání později. `IMobileServiceSyncContext.InitializeAsync()` Metoda se používá k místním úložišti přidružit kontext synchronizace.

`todoTable` Pole je `IMobileServiceSyncTable`, a proto všechny operace CRUD použít místní úložiště.

## <a name="performing-synchronization"></a>Probíhá synchronizace

Místní úložiště se synchronizují s Azure Mobile Apps instance, kdy `SyncAsync` je volána metoda:

```csharp
public async Task SyncAsync()
{
  ReadOnlyCollection<MobileServiceTableOperationError> syncErrors = null;

  try
  {
    await this.client.SyncContext.PushAsync();

    // The first parameter is a query name that is used internally by the client SDK to implement incremental sync.
    // Use a different query name for each unique query in your program.
    await this.todoTable.PullAsync("allTodoItems", this.todoTable.CreateQuery());
  }
  catch (MobileServicePushFailedException exc)
  {
    if (exc.PushResult != null)
    {
      syncErrors = exc.PushResult.Errors;
    }
  }

  // Simple error/conflict handling.
  if (syncErrors != null)
  {
    foreach (var error in syncErrors)
    {
      if (error.OperationKind == MobileServiceTableOperationKind.Update && error.Result != null)
      {
        // Update failed, revert to server's copy
        await error.CancelAndUpdateItemAsync(error.Result);
      }
      else
      {
        // Discard local change
        await error.CancelAndDiscardItemAsync();
      }

      Debug.WriteLine(@"Error executing sync operation. Item: {0} ({1}). Operation discarded.", error.TableName, error.Item["id"]);
    }
  }
}
```

`IMobileServiceSyncTable.PushAsync` Metoda funguje na kontext synchronizace, nikoli konkrétní tabulku a odešle všechny změny vytvoření od poslední nabízeného oznámení.

Vyžádání obsahu se provádí pomocí `IMobileServiceSyncTable.PullAsync` metoda na jednu tabulku. První parametr `PullAsync` metoda je název dotazu, který se používá pouze v mobilním zařízení. Poskytnutí dotazu nesmí být nulová název výsledky při provádění Azure Mobile klienta SDK *přírůstkové synchronizace*, kde pokaždé, když vyžádanou operaci vrátí výsledky, nejnovější `updatedAt` časové razítko z výsledků je uložený v místním systémové tabulky. Operace následné stažení pak jen načítají záznamy po této časové razítko. Alternativně *úplné synchronizace* lze dosáhnout pomocí předání `null` jako název dotazu, výsledkem všechny záznamy načítány na každou vyžádanou operaci. Následující všechny synchronizační operace je přijatá data vloží do místního úložiště.

> [!NOTE]
> Pokud je u tabulku, která má čekající místní aktualizace spustit vyžádání obsahu, vyžádání obsahu nejprve spustit push na kontext synchronizace. Tím se minimalizují konflikty mezi změny, které jsou již zařazeny do fronty a nová data z instance Azure Mobile Apps.

`SyncAsync` Metoda zahrnuje také základní implementace pro zpracování konfliktů při stejné záznamu došlo ke změně v místním úložišti a v instanci Azure Mobile Apps. Při konfliktu, aby data byla aktualizována v místním úložišti a v instanci Azure Mobile Apps `SyncAsync` metoda aktualizuje data v místním úložišti od dat uložených v instanci Azure Mobile Apps. Když dojde k jakékoli jiné konfliktu, `SyncAsync` metoda zahodí místní změny. To zpracovává scénář, kde existuje místní změny dat, který je odstraněný z Azure Mobile Apps instance.

V případě produkční aplikace by měla vývojáři psát vlastní `IMobileServiceSyncHandler` implementace zpracování konfliktů, který je vhodný pro případ jejich použití. Další informace najdete v tématu [použít optimistickou metodu souběžného pro řešení konfliktů](https://azure.microsoft.com/documentation/articles/app-service-mobile-dotnet-how-to-use-client-library/#optimisticconcurrency) na portálu Azure a [přímým podrobné informace o offline podporovaných v spravovanou klientskou sadou SDK](https://blogs.msdn.microsoft.com/azuremobile/2014/04/07/deep-dive-on-the-offline-support-in-the-managed-client-sdk/) na blogy MSDN.

## <a name="purging-data"></a>Vyprazdňování dat

Tabulky v místním úložišti lze vymazat dat pomocí `IMobileServiceSyncTable.PurgeAsync` metoda. Tato metoda podporuje scénáře, jako je odebrání zastaralá data, která aplikace již nevyžaduje. Například ukázkové aplikace se zobrazí jenom `TodoItem` instancí, které nejsou úplné. Proto dokončené položky již nebude nutné jej ukládat místně. Vymazání dokončené položky z místního úložiště se dá udělat následujícím způsobem:

```csharp
await todoTable.PurgeAsync(todoTable.Where(item => item.Done));
```

Volání `PurgeAsync` také aktivuje operace push. Proto všechny položky, které jsou označeny jako dokončené místně odešle do instance Azure Mobile Apps než jsou odstraněny z místního úložiště. Ale pokud existují operace čekající na vyřízení synchronizace s instancí Azure Mobile Apps, k vyprázdnění vyvolá výjimku `InvalidOperationException` Pokud `force` parametr je nastaven na `true`. Alternativní strategie je prozkoumat `IMobileServiceSyncContext.PendingOperations` vlastnosti, která vrátí počet čekajících operací, které nebyly se nabídne k instanci Azure Mobile Apps a k vyprázdnění provádět pouze pokud je vlastnost nula.

> [!NOTE]
> Vyvolání `PurgeAsync` s `force` parametr nastaven na `true` všechny neuložené změny budou ztraceny.

## <a name="initiating-synchronization"></a>Inicializace synchronizace

V ukázkové aplikaci `SyncAsync` metoda je volána prostřednictvím `TodoList.OnAppearing` metoda:

```csharp
protected override async void OnAppearing()
{
  base.OnAppearing();

  // Set syncItems to true to synchronize the data on startup when running in offline mode
  await RefreshItems(true, syncItems: true);
}
```

To znamená, že se aplikace pokusí o synchronizaci s Azure Mobile Apps instancí při spuštění.

Navíc můžete inicializovat synchronizace v iOS a Android pomocí přijetí změn aktualizovat seznam dat a na platformách systému Windows pomocí **synchronizace** tlačítko v uživatelském rozhraní. Další informace najdete v tématu [pro vyžádání obsahu pro aktualizaci](~/xamarin-forms/user-interface/listview/interactivity.md#Pull_to_Refresh).

## <a name="summary"></a>Souhrn

Tento článek vysvětlení najdete postup přidání funkci offline synchronizace k aplikaci Xamarin.Forms. Offline synchronizace umožňuje uživatelům interakci s mobilních aplikací, zobrazení, přidání nebo úprava dat, i tam, kde není k dispozici síťové připojení. Změny se ukládají do místní databáze, a když je zařízení online, změny mohou být synchronizovány s Azure Mobile Apps instancí.


## <a name="related-links"></a>Související odkazy

- [TodoAzureAuthOfflineSync (ukázka)](https://developer.xamarin.com/samples/xamarin-forms/WebServices/TodoAzureAuthOfflineSync/)
- [Použití Azure mobilní aplikace](~/xamarin-forms/data-cloud/consuming/azure.md)
- [Ověřování uživatelů s Azure Mobile Apps](~/xamarin-forms/data-cloud/authentication/azure.md)
- [Azure mobilního klienta SDK](https://www.nuget.org/packages/Microsoft.Azure.Mobile.Client/)
- [MobileServiceClient](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.mobileservices.mobileserviceclient(v=azure.10).aspx)
