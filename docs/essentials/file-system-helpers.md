---
title: 'Xamarin.Essentials: Pomocné rutiny systému souborů'
description: Třída systému souborů v Xamarin.Essentials obsahuje řadu pomocné rutiny najdete mezipaměti aplikace a data adresáře a otevírání souborů v balíčku aplikace.
ms.assetid: B3EC2DE0-EFC0-410C-AF71-7410AE84CF84
author: jamesmontemagno
ms.author: jamont
ms.date: 05/04/2018
ms.openlocfilehash: 13293ec05261cbdc1e70fd278002d1af18654851
ms.sourcegitcommit: 632955f8cdb80712abd8dcc30e046cb9c435b922
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2018
ms.locfileid: "38815615"
---
# <a name="xamarinessentials-file-system-helpers"></a>Xamarin.Essentials: Pomocné rutiny systému souborů

![Předběžné verze NuGet](~/media/shared/pre-release.png)

**Systému souborů** třída obsahuje řadu pomocné rutiny do mezipaměti a data adresáře aplikace najít a otevřít soubory v balíčku aplikace.

## <a name="using-file-system-helpers"></a>Použití pomocné rutiny systému souborů

Přidáte odkaz na Xamarin.Essentials ve své třídě:

```csharp
using Xamarin.Essentials;
```

Chcete-li získat adresáře aplikace k ukládání **ukládat data do mezipaměti**. Data v mezipaměti je použít pro všechna data, která je potřeba uchovávat déle než dočasná data, ale by neměly být data, která je potřeba správně fungovat.

```csharp
var cacheDir = FileSystem.CacheDirectory;
```

Chcete-li získat adresář nejvyšší úrovně vaší aplikace pro všechny soubory, které nejsou soubory dat uživatele. Tyto soubory jsou zálohovány pomocí synchronizace framework operačního systému. Viz podrobnosti implementace platformy níže.

```csharp
var mainDir = FileSystem.AppDataDirectory;
```

Otevřete soubor, který se dodává v sadě do balíčku aplikace:

```csharp
 using (var stream = await FileSystem.OpenAppPackageFileAsync(templateFileName))
 {
    using (var reader = new StreamReader(stream))
    {
        var fileContents = await reader.ReadToEndAsync();
    }
 }
```

## <a name="platform-implementation-specifics"></a>Specifika platforem implementace

# <a name="androidtabandroid"></a>[Android](#tab/android)

- **CacheDirectory** – vrátí [CacheDir](https://developer.android.com/reference/android/content/Context.html#getCacheDir) aktuálního kontextu.
- **AppDataDirectory** – vrátí [FilesDir](https://developer.android.com/reference/android/content/Context.html#getFilesDir) aktuálního kontextu a jsou zálohovány pomocí [automatické zálohování](https://developer.android.com/guide/topics/data/autobackup.html) spuštění na rozhraní API 23 a vyšší.

Přidejte všechny soubory do **prostředky** složky v Androidu projektu a označit akce sestavení jako **AndroidAsset** pro použití s `OpenAppPackageFileAsync`.

# <a name="iostabios"></a>[iOS](#tab/ios)

- **CacheDirectory** – vrátí [knihovny/mezipamětí](https://developer.apple.com/library/content/documentation/FileManagement/Conceptual/FileSystemProgrammingGuide/FileSystemOverview/FileSystemOverview.html) adresáře.
- **AppDataDirectory** – vrátí [knihovny](https://developer.apple.com/library/content/documentation/FileManagement/Conceptual/FileSystemProgrammingGuide/FileSystemOverview/FileSystemOverview.html) adresář, který je zajištěná dat v iTunes a Icloudu.

Přidejte všechny soubory do **prostředky** složku v systému iOS projektu a označit akce sestavení jako **BundledResource** pro použití s `OpenAppPackageFileAsync`.

# <a name="uwptabuwp"></a>[UPW](#tab/uwp)

- **CacheDirectory** – vrátí [LocalCacheFolder](https://docs.microsoft.com/en-us/uwp/api/windows.storage.applicationdata.localcachefolder#Windows_Storage_ApplicationData_LocalCacheFolder) adresáře...
- **AppDataDirectory** – vrátí [LocalFolder](https://docs.microsoft.com/en-us/uwp/api/windows.storage.applicationdata.localfolder#Windows_Storage_ApplicationData_LocalFolder) adresář, který je zálohovat do cloudu.

Přidejte všechny soubory do kořenového adresáře v projektu UWP a označte akce sestavení jako **obsahu** pro použití s `OpenAppPackageFileAsync`.

--------------

## <a name="api"></a>rozhraní API

- [Soubor pomocné rutiny systému zdrojového kódu](https://github.com/xamarin/Essentials/tree/master/Xamarin.Essentials/FileSystem)
- [Dokumentace k rozhraní API systému souborů](xref:Xamarin.Essentials.FileSystem)
