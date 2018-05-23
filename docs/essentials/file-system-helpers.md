---
title: Pomocníci systému Xamarin.Essentials souboru
description: Třída systém souborů obsahuje řadu pomocné rutiny můžete najít mezipaměti aplikace a data adresáře a otevírat soubory v balíčku aplikace.
ms.assetid: B3EC2DE0-EFC0-410C-AF71-7410AE84CF84
author: jamesmontemagno
ms.author: jamont
ms.date: 05/04/2018
ms.openlocfilehash: 14aabc319fefdbad86f29a9d27ce39b59da35e3e
ms.sourcegitcommit: 9f8e7393019791bbd6af4fefaa24a1602adabb4e
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 05/23/2018
---
# <a name="xamarinessentials-file-system-helpers"></a>Pomocníci systému Xamarin.Essentials souboru

![Předběžné verze NuGet](~/media/shared/pre-release.png)

**FileSystem** třída obsahuje řadu pomocné rutiny do adresáře mezipaměti a data aplikace najít a otevřít soubory v balíčku aplikace.

## <a name="using-file-system-helpers"></a>Pomocí systému souborů pomocné rutiny

Přidáte odkaz na Xamarin.Essentials v třídě:

```csharp
using Xamarin.Essentials;
```

Chcete-li získat aplikace adresář pro ukládání **ukládat data do mezipaměti**. Data do mezipaměti můžete použít pro všechna data, která je potřeba zachovat déle, než dočasná data, ale nesmí být data, která je potřeba správně fungovat.

```csharp
var cacheDir = FileSystem.CacheDirectory;
```

Chcete-li získat nejvyšší úrovně adresáře aplikace pro všechny soubory, které nejsou uživatelských dat souborů. Tyto soubory jsou zálohovány s operačním systémem synchronizuje framework. Viz níže podrobnosti implementace platformy.

```csharp
var mainDir = FileSystem.AppDataDirectory;
```

Otevřete soubor, který je seskupeny do balíčku aplikace:

```csharp
 using (var stream = await FileSystem.OpenAppPackageFileAsync(templateFileName))
 {
    using (var reader = new StreamReader(stream))
    {
        var fileContents = await reader.ReadToEndAsync();
    }
 }
```

## <a name="platform-implementation-specifics"></a>Podrobnosti implementace platformy

# <a name="androidtabandroid"></a>[Android](#tab/android)

- **CacheDirectory** – vrátí [CacheDir](https://developer.android.com/reference/android/content/Context.html#getCacheDir) aktuálního kontextu.
- **AppDataDirectory** – vrátí [FilesDir](https://developer.android.com/reference/android/content/Context.html#getFilesDir) aktuální kontext a jsou zálohovány pomocí [automatické zálohování](https://developer.android.com/guide/topics/data/autobackup.html) spouštění na rozhraní API 23 a vyšší.

Přidejte všechny soubory do **prostředky** složky v Android projektu a označit akce sestavení jako **AndroidAsset** pro použití s `OpenAppPackageFileAsync`.

# <a name="iostabios"></a>[iOS](#tab/ios)

- **CacheDirectory** – vrátí [knihovny nebo mezipamětí](https://developer.apple.com/library/content/documentation/FileManagement/Conceptual/FileSystemProgrammingGuide/FileSystemOverview/FileSystemOverview.html) adresáře.
- **AppDataDirectory** – vrátí [knihovny](https://developer.apple.com/library/content/documentation/FileManagement/Conceptual/FileSystemProgrammingGuide/FileSystemOverview/FileSystemOverview.html) adresář, který je zálohovaný iTunes a Icloudu.

Přidejte všechny soubory do **prostředky** složky v iOS projektu a označit akce sestavení jako **BundledResource** pro použití s `OpenAppPackageFileAsync`.

# <a name="uwptabuwp"></a>[UWP](#tab/uwp)

- **CacheDirectory** – vrátí [LocalCacheFolder](https://docs.microsoft.com/en-us/uwp/api/windows.storage.applicationdata.localcachefolder#Windows_Storage_ApplicationData_LocalCacheFolder) adresáře...
- **AppDataDirectory** – vrátí [LocalFolder](https://docs.microsoft.com/en-us/uwp/api/windows.storage.applicationdata.localfolder#Windows_Storage_ApplicationData_LocalFolder) adresář, který je zálohována do cloudu.

Přidejte všechny soubory do kořenového adresáře v projektu UPW a označte akce sestavení jako **obsahu** pro použití s `OpenAppPackageFileAsync`.

--------------

## <a name="api"></a>rozhraní API

- [Soubor Pomocníci systému zdrojového kódu](https://github.com/xamarin/Essentials/tree/master/Xamarin.Essentials/FileSystem)
- [Dokumentace rozhraní API systému souborů](xref:Xamarin.Essentials.FileSystem)
