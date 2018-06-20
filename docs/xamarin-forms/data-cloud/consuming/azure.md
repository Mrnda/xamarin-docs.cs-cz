---
title: Použití Azure mobilní aplikace
description: Tento článek, který je pouze pro Azure Mobile Apps, použít back-end Node.js, vysvětluje, jak pro dotazování, vložit, aktualizovat a odstranit data uložená v tabulce v instanci Azure Mobile Apps.
ms.prod: xamarin
ms.assetid: 2B3EFD0A-2922-437D-B151-4B4DE46E2095
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 09/20/2016
ms.openlocfilehash: 73d74b59ef6e59028eec7cad19feec21908b6329
ms.sourcegitcommit: d70fcc6380834127fdc58595aace55b7821f9098
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 06/19/2018
ms.locfileid: "36269041"
---
# <a name="consuming-an-azure-mobile-app"></a>Použití Azure mobilní aplikace

_Azure Mobile Apps umožňují vývoj aplikací s škálovatelné back-EndY hostované v Azure App Service, se podpora pro mobilní ověřování, offline synchronizace a nabízených oznámení. Tento článek, který je pouze pro Azure Mobile Apps, použít back-end Node.js, vysvětluje, jak pro dotazování, vložit, aktualizovat a odstranit data uložená v tabulce v instanci Azure Mobile Apps._

> [!NOTE]
> Spouštění služby na 30. června, všechny nové Azure Mobile Apps bude vytvořen s protokolem TLS 1.2 ve výchozím nastavení. Kromě toho se taky doporučuje tento existující Azure Mobile Apps být nakonfigurována na používání protokolu TLS 1.2. Informace o tom, jak vynutit TLS 1.2 v mobilní aplikaci Azure najdete v tématu [vynutit TLS 1.2](/azure/app-service/app-service-web-tutorial-custom-ssl#enforce-tls-1112). Informace o tom, jak nakonfigurovat projektu Xamarin pro použití protokolu TLS 1.2, najdete v článku [zabezpečení TLS (Transport Layer) 1.2](~/cross-platform/app-fundamentals/transport-layer-security.md).

Informace o tom, jak vytvořit instanci Azure Mobile Apps, která mohou být spotřebovávána Xamarin.Forms najdete v tématu [vytvoření aplikace na platformě Xamarin.Forms](https://azure.microsoft.com/documentation/articles/app-service-mobile-xamarin-forms-get-started/). Až projdete tyto pokyny, lze nastavit ke stažení ukázkové aplikace využívat Azure Mobile Apps instance nastavením `Constants.ApplicationURL` na adresu URL instance Azure Mobile Apps. Poté při spuštění ukázkové aplikace se připojí k instanci Azure Mobile Apps, jak je znázorněno na následujícím snímku obrazovky:

![](azure-images/portal.png "Ukázkové aplikace")

Přístup k Azure Mobile Apps je prostřednictvím [Azure Mobile Client SDK](https://www.nuget.org/packages/Microsoft.Azure.Mobile.Client/), a všechna připojení v ukázkové aplikaci Xamarin.Forms do Azure jsou vytvářeny pomocí protokolu HTTPS.

> [!NOTE]
> Zabezpečení přenosu aplikace (ATS) vynucuje v iOS 9 a vyšší, zabezpečené připojení mezi prostředků z Internetu (třeba server back-end aplikace) a aplikace, a tím prevence náhodného zpřístupnění citlivých informací. Protože ATS je povolené ve výchozím nastavení v aplikacích pro iOS 9, všechna připojení budou platit ATS požadavky na zabezpečení. Pokud připojení těchto požadavků nesplňuje, budou se nezdaří s výjimkou.
> ATS můžete vyjádřit výslovný souhlas mimo Pokud není možné použít `HTTPS` protokolu a zabezpečení komunikace pro prostředků z Internetu. Toho lze dosáhnout aktualizací aplikace **Info.plist** souboru. Další informace najdete v části [zabezpečení přenosu aplikace](~/ios/app-fundamentals/ats.md).

## <a name="consuming-an-azure-mobile-app-instance"></a>Použití Instance mobilní aplikace Azure

[Azure Mobile Client SDK](https://www.nuget.org/packages/Microsoft.Azure.Mobile.Client/) poskytuje `MobileServiceClient` třídy, která se používá aplikace Xamarin.Forms se připojit k instanci Azure Mobile Apps, jak je znázorněno v následujícím příkladu kódu:

```csharp
IMobileServiceTable<TodoItem> todoTable;
MobileServiceClient client;

public TodoItemManager ()
{
  client = new MobileServiceClient (Constants.ApplicationURL);
  todoTable = client.GetTable<TodoItem> ();
}
```

Když `MobileServiceClient` se vytvoří instance, aby identifikaci instance Azure Mobile Apps je nutné zadat adresu URL aplikace. Tuto hodnotu můžete získat z řídicího panelu pro mobilní aplikace v [portálu Microsoft Azure](https://portal.azure.com/).

Odkaz na `TodoItem` tabulky uložené v instanci Azure Mobile Apps musí být získána před provedením operace v této tabulce. Toho se dosáhne volání `GetTable` metodu `MobileServiceClient` instanci, která vrátí `IMobileServiceTable<TodoItem>` odkaz.

### <a name="querying-data"></a>Dotazování na Data

Může načíst obsah tabulky volání `IMobileServiceTable.ToEnumerableAsync` metodu, která asynchronně vyhodnocuje dotaz a vrátí výsledky. Data mohou být také Filtroval včetně na straně serveru `Where` klauzule dotazu. `Where` Klauzule platí řádek filtrování predikátu k dotazu vůči tabulky, jak je znázorněno v následujícím příkladu kódu:

```csharp
public async Task<ObservableCollection<TodoItem>> GetTodoItemsAsync (bool syncItems = false)
{
  ...
  IEnumerable<TodoItem> items = await todoTable
              .Where (todoItem => !todoItem.Done)
              .ToEnumerableAsync ();

  return new ObservableCollection<TodoItem> (items);
}
```

Tento dotaz vrací všechny položky z `TodoItem` tabulky, jehož `Done` vlastnost rovná `false`. Výsledky dotazu jsou pak umístit do `ObservableCollection` pro zobrazení.

### <a name="inserting-data"></a>Vkládání dat

Při vkládání dat v instanci Azure Mobile Apps, budou automaticky generovat nové sloupce v tabulce podle potřeby zadané schéma této dynamické je povolena v instanci Azure Mobile Apps. `IMobileServiceTable.InsertAsync` Metoda slouží k vložení nového řádku dat do zadané tabulky, jak je znázorněno v následujícím příkladu kódu:

```csharp
public async Task SaveTaskAsync (TodoItem item)
{
  ...
  await todoTable.InsertAsync (item);
  ...
}
```

Při provádění požadavek vložení, ID nesmí být zadané v datech předáním instanci Azure Mobile Apps. Pokud požadavek insert obsahuje ID `MobileServiceInvalidOperationException` bude vyvolána.

Po `InsertAsync` metoda dokončí, vyplní ID data v instanci Azure Mobile Apps v `TodoItem` instance v aplikaci Xamarin.Forms.

### <a name="updating-data"></a>Aktualizace dat

Při aktualizaci dat v instanci Azure Mobile Apps, budou automaticky generovat nové sloupce v tabulce podle potřeby zadané schéma této dynamické je povolena v instanci Azure Mobile Apps. `IMobileServiceTable.UpdateAsync` Metoda se používá k aktualizaci existující data s nové informace, jak je znázorněno v následujícím příkladu kódu:

```csharp
public async Task SaveTaskAsync (TodoItem item)
{
  ...
  await todoTable.UpdateAsync (item);
  ...
}
```

Při provádění při žádosti o aktualizaci, ID musí zadat tak, aby instance Azure Mobile Apps můžete identifikovat data, která mají být aktualizovány. Tato hodnota ID je uložena v `TodoItem.ID` vlastnost. Pokud se žádost o aktualizaci neobsahuje ID neexistuje žádný způsob pro instanci Azure Mobile Apps k určení dat aktualizovat a tudíž `MobileServiceInvalidOperationException` bude vyvolána.

### <a name="deleting-data"></a>Odstraňování dat

`IMobileServiceTable.DeleteAsync` Metoda se používá k odstranění dat z tabulky Azure Mobile Apps, jak je znázorněno v následujícím příkladu kódu:

```csharp
public async Task DeleteTaskAsync (TodoItem item)
{
  ...
  await todoTable.DeleteAsync(item);
  ...
}
```

Při provádění požadavku na odstranění, ID musí zadat tak, aby sinstance mobilní aplikace Azure můžete identifikovat data, která mají být odstraněny. Tato hodnota ID je uložena v `TodoItem.ID` vlastnost. Pokud požadavek delete neobsahuje ID, neexistuje žádný způsob pro instanci Azure Mobile Apps k určení dat má být odstraněn a to `MobileServiceInvalidOperationException` bude vyvolána.

## <a name="summary"></a>Souhrn

Tento článek vysvětlení, jak používat [Azure Mobile Client SDK](https://www.nuget.org/packages/Microsoft.Azure.Mobile.Client/) pro dotazování, vložit, aktualizovat a odstranit data uložená v tabulce v instanci Azure Mobile apps. Sada SDK poskytuje `MobileServiceClient` třídu, která používá aplikace Xamarin.Forms se připojit k instanci Azure Mobile Apps.


## <a name="related-links"></a>Související odkazy

- [TodoAzure (ukázka)](https://developer.xamarin.com/samples/xamarin-forms/WebServices/TodoAzure/)
- [Vytvoření aplikace na platformě Xamarin.Forms](https://azure.microsoft.com/documentation/articles/app-service-mobile-xamarin-forms-get-started/)
- [Azure mobilního klienta SDK](https://www.nuget.org/packages/Microsoft.Azure.Mobile.Client/)
- [MobileServiceClient](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.mobileservices.mobileserviceclient(v=azure.10).aspx)
