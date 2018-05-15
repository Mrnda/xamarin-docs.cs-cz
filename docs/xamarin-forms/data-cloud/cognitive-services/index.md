---
title: Přidání Intelligence kognitivní službou
description: Kognitivní služby společnosti Microsoft se sadu rozhraní API, sady SDK a službách, které jsou k dispozici pro vývojáře k jejich aplikacím inteligentnější přidáním funkce, jako je rozpoznávání obličeje, rozpoznávání řeči a znalosti jazyka. Tento článek obsahuje úvod do ukázkovou aplikaci, která ukazuje, jak má být vyvolán některé z rozhraní API kognitivní služby společnosti Microsoft.
ms.prod: xamarin
ms.assetid: 74121ADB-1322-4C1E-A103-F37257BC7CB0
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 02/08/2017
ms.openlocfilehash: 86253e42db7da2da6eb8b03e2d4a4b3c943b7e17
ms.sourcegitcommit: b0a1c3969ab2a7b7fe961f4f470d1aa57b1ff2c6
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 05/10/2018
---
# <a name="adding-intelligence-with-cognitive-services"></a>Přidání Intelligence kognitivní službou

_Kognitivní služby společnosti Microsoft se sadu rozhraní API, sady SDK a službách, které jsou k dispozici pro vývojáře k jejich aplikacím inteligentnější přidáním funkce, jako je rozpoznávání obličeje, rozpoznávání řeči a znalosti jazyka. Tento článek obsahuje úvod do ukázkovou aplikaci, která ukazuje, jak má být vyvolán některé z rozhraní API kognitivní služby společnosti Microsoft._

## <a name="overview"></a>Přehled

Doprovodné ukázka je aplikaci seznamu úkolů, která poskytuje funkce, které:

- Zobrazení seznamu úloh.
- Přidávání a úprava úloh prostřednictvím softwarová klávesnice, nebo provedením rozpoznávání řeči s rozhraním API pro rozpoznávání řeči společnosti Microsoft. Další informace o provádění rozpoznávání řeči najdete v tématu [rozpoznávání řeči pomocí rozhraní API pro rozpoznávání řeči Microsoft](speech-recognition.md).
- Pomocí rozhraní API zkontrolujte služby Bing pravopisu úlohy kontroly pravopisu. Další informace najdete v tématu [pomocí Bingu pravopisu zkontrolujte rozhraní API pro kontrolu pravopisu](spell-check.md).
- Převede úlohy z angličtina, němčina, pomocí rozhraní API překladač. Další informace najdete v tématu [překlad Text pomocí rozhraní API překladač](text-translation.md).
- Odstraňování úkolů.
- Nastavte stav úkolu, udělat'.
- Hodnocení aplikace s rozpoznáním rozpoznávání emocí úrovně, pomocí rozhraní API řez. Další informace najdete v tématu [rozpoznávání emocí úrovně rozpoznávání pomocí rozhraní API vzhled](emotion-recognition.md).

Úlohy jsou uloženy v místní databázi SQLite. Další informace o použití místní databáze SQLite najdete v tématu [práci s místní databází](~/xamarin-forms/app-fundamentals/databases.md).

`TodoListPage` Se zobrazí, když je aplikace spuštěna. Tato stránka zobrazuje seznam všech úloh uložené v místní databázi a umožňuje uživateli vytvořit novou úlohu a hodnocení aplikace:

![](images/sample-application-1.png "TodoListPage")

Nové položky můžete vytvořit kliknutím na *+* tlačítko, která přejde `TodoItemPage`. Na této stránce můžete také tak, že vyberete úlohu přesměrováni do:

![](images/sample-application-2.png "TodoItemPage")

`TodoItemPage` Umožňuje vytvořit, upravovat, kontrola pravopisu úkoly přeložit, uložit a odstranění. Rozpoznávání řeči lze použít k vytvoření nebo úpravě úlohy. Můžete toho dosáhnout stisknutím tlačítka mikrofon spusťte záznam a stisknutím tlačítka stejné podruhé ukončíte záznam, který odešle záznamu do rozhraní API pro rozpoznávání řeči Bing.

Klepnutím na tlačítko smilies na `TodoListPage` přejde `RateAppPage`, který se používá k provádění rozpoznávání emocí úrovně rozpoznávání obrázku obličeje výrazu:

![](images/sample-application-3.png "RateAppPage")

`RateAppPage` Umožňuje uživateli fotografie z jejich setkávají, která je odeslána do rozhraní API setkávají s vrácený rozpoznávání emocí úrovně se zobrazuje.

## <a name="understanding-the-application-anatomy"></a>Principy Anatomy aplikace

Projekt přenosných třída knihovny PCL () pro ukázkovou aplikaci se skládá z pěti hlavních složek:

|Folder|Účel|
|--- |--- |
|Modely|Obsahuje třídy modelu dat pro aplikaci. To zahrnuje `TodoItem` třídy, která modelů jednu položku dat používá aplikace. Složka také obsahuje třídy používané k odpovědím modelu JSON vrácená z různých Microsoft kognitivní služby rozhraní API.|
|Úložiště|Obsahuje `ITodoItemRepository` rozhraní a `TodoItemRepository` třídu, která se používá k provádění operací databáze.|
|Služby|Obsahuje rozhraní a třídy, které se používají pro přístup k jiné Microsoft kognitivní služby rozhraní API, společně s rozhraní, které jsou používány `DependencyService` třída vyhledat třídy, které implementují rozhraní v projektech platformy.|
|Utils|Obsahuje `Timer` třídy, která je používána `AuthenticationService` třída obnovit přístupový token JWT každých 9 minut.|
|zobrazení|Obsahuje stránky pro aplikaci.|

Projekt PCL také obsahuje některé důležité soubory:

|Soubor|Účel|
|--- |--- |
|Constants.cs|`Constants` Třídy, která určuje pro Microsoft kognitivní služby rozhraní API, která jsou vyvolány klíče rozhraní API a koncové body. Konstanty klíče rozhraní API vyžadovat aktualizace pro přístup k jiné kognitivní rozhraní API služby.|
|App.xaml.cs|`App` Třída je odpovědná za vytváření instancí i na první stránku, která se zobrazí v aplikaci na každou platformu, a `TodoManager` třídu, která se použije k vyvolání operace databáze.|

### <a name="nuget-packages"></a>Balíčky NuGet

Ukázková aplikace používá následující balíčky NuGet:

- `Newtonsoft.Json` – poskytuje pro rozhraní .NET JSON framework.
- `PCLStorage` – poskytuje sadu napříč platformami místního souboru vstupně-výstupní operace rozhraní API.
- `sqlite-net-pcl` – poskytuje úložiště databáze SQLite.
- `Xam.Plugin.Media` – poskytuje fotografií vezme napříč platformami a výběr rozhraní API.

Kromě toho tyto instalace balíčků NuGet také vlastní závislosti.

### <a name="modeling-the-data"></a>Modelování dat

Ukázková aplikace používá `TodoItem` třídy modelu data, která se zobrazí a uloží do místní databáze SQLite. Následující příklad kódu ukazuje `TodoItem` třídy:

```csharp
public class TodoItem
{
  [PrimaryKey, AutoIncrement]
  public int ID { get; set; }
  public string Name { get; set; }
  public bool Done { get; set; }
}
```

`ID` Vlastnost slouží k jednoznačné identifikaci jednotlivých `TodoItem` instance a je upraven pomocí SQLite atributů, které vlastnost automaticky rostoucí primární klíč v databázi.

### <a name="invoking-database-operations"></a>Vyvolání databázových operací

`TodoItemRepository` Třída implementuje databázových operací a instance třídy je možné přistupovat prostřednictvím `App.TodoManager` vlastnost. `TodoItemRepository` Třída poskytuje následující metody k vyvolání operace databáze:

- **GetAllItemsAsync** – načte všechny položky z místní databáze SQLite.
- **GetItemAsync** – načte zadanou položku z místní databáze SQLite.
- **SaveItemAsync** – vytvoří nebo aktualizuje položku v místní databázi SQLite.
- **DeleteItemAsync** – Odstraní určenou položku z místní databáze SQLite.

### <a name="platform-project-implementations"></a>Implementace projektu platformy

`Services` Složka v projektu PCL obsahuje `IFileHelper` a `IAudioRecorderService` rozhraní, které jsou používány `DependencyService` třída vyhledat třídy, které implementují rozhraní v projektech platformy.

`IFileHelper` Rozhraní je implementováno modulem `FileHelper` třídy v každém projektu platformy. Tato třída se skládá z jedné metody `GetLocalFilePath`, která vrací cestu místního souboru pro ukládání databáze SQLite.

`IAudioRecorderService` Rozhraní je implementováno modulem `AudioRecorderService` třídy v každém projektu platformy. Tato třída se skládá z `StartRecording`, `StopRecording`a podpůrné metody, které pomocí platformy API záznam zvuku z mikrofon zařízení a uložte ho jako soubor wav. V systému iOS `AudioRecorderService` používá `AVFoundation` rozhraní API pro záznam zvuku. V systému Android `AudioRecordService` používá `AudioRecord` rozhraní API pro záznam zvuku. Na univerzální platformu Windows (UWP), `AudioRecorderService` používá `AudioGraph` rozhraní API pro záznam zvuku.

### <a name="invoking-cognitive-services"></a>Vyvolání kognitivní služeb

Ukázkovou aplikaci vyvolá následující kognitivní služby společnosti Microsoft:

- Microsoft řeči rozhraní API. Další informace najdete v tématu [rozpoznávání řeči pomocí rozhraní API pro rozpoznávání řeči Microsoft](speech-recognition.md).
- Kontrola pravopisu v Bingu rozhraní API. Další informace najdete v tématu [pomocí Bingu pravopisu zkontrolujte rozhraní API pro kontrolu pravopisu](spell-check.md).
- Převede rozhraní API. Další informace najdete v tématu [překlad Text pomocí rozhraní API překladač](text-translation.md).
- Vzhled rozhraní API. Další informace najdete v tématu [rozpoznávání emocí úrovně rozpoznávání pomocí rozhraní API vzhled](emotion-recognition.md).

## <a name="related-links"></a>Související odkazy

- [Dokumentace kognitivní služby společnosti Microsoft](https://www.microsoft.com/cognitive-services/documentation)
- [TODO kognitivní služby (ukázka)](https://developer.xamarin.com/samples/xamarin-forms/WebServices/TodoCognitiveServices/)
