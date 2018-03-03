---
title: Soubory
description: "Práce s Xamarin.Forms se soubory lze provést pomocí vložených prostředků nebo zápis proti nativní filesystem rozhraní API."
ms.topic: article
ms.prod: xamarin
ms.assetid: 9987C3F6-5F04-403B-BBB4-ECB024EA6CC8
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 03/22/2017
ms.openlocfilehash: 605374c0f2bfe656e564e48d14ffe18ce5b7dfe5
ms.sourcegitcommit: 61f5ecc5a2b5dcfbefdef91664d7460c0ee2f357
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 02/28/2018
---
# <a name="files"></a>Soubory

_Práce s Xamarin.Forms se soubory lze provést pomocí vložených prostředků nebo zápis proti nativní filesystem rozhraní API._

## <a name="overview"></a>Přehled

Spustí kód Xamarin.Forms ve více platformách - z nichž každá má svou vlastní systému souborů. To znamená, že čtení a zápis souborů je většina snadno done pomocí nativních rozhraní API souboru na každou platformu. Alternativně vložené prostředky jsou jednodušší řešení k distribuci datových souborů s aplikací.

Tento dokument popisuje následující běžné souborů zpracování scénáře:

-  [ **Vložené soubory jako prostředky** ](#Loading_Files_Embedded_as_Resources) -soubory lze dodávána jako součást k aplikaci a načíst pomocí rozhraní API reflexe.
-  [ **Ukládání a načítání souborů** ](#Loading_and_Saving_Files) -uživatele zapisovatelné úložiště může být implementována nativně a potom získat přístup pomocí `DependencyService` .


Volání komponenty jiných výrobců **PCLStorage** slouží také k čtení a zápis souborů uživatele dostupné úložiště z PCL kódu.

Informace o zpracování souborů bitové kopie, naleznete [práce s obrázky](~/xamarin-forms/user-interface/images.md) stránky.

<a name="Loading_Files_Embedded_as_Resources" />

## <a name="loading-files-embedded-as-resources"></a>Načítání souborů vložených jako prostředky

Chcete-li vložit soubor do **PCL** sestavení, vytvořit nebo přidat soubor a ujistěte se, že **akce sestavení: EmbeddedResource**.

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[ ![Konfigurace vložených akce sestavení prostředku](files-images/vs-embeddedresource-sml.png "nastavení EmbeddedResource BuildAction")](files-images/vs-embeddedresource.png "EmbeddedResource BuildAction nastavení")

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

[ ![Textový soubor vložených v PCL, konfigurace akce sestavení vložený prostředek](files-images/xs-embeddedresource-sml.png "nastavení EmbeddedResource BuildAction")](files-images/xs-embeddedresource.png "EmbeddedResource BuildAction nastavení")

-----

`GetManifestResourceStream` slouží k přístupu ke vloženého souboru pomocí jeho **ID prostředku**. Ve výchozím nastavení je název souboru s předponou výchozí obor názvů pro projekt je integrovaný v - ID prostředku v tomto případě sestavení je **WorkingWithFiles** a název souboru je **PCLTextResource.txt**, Proto je ID prostředku `WorkingWithFiles.PCLTextResource.txt`.

```csharp
var assembly = typeof(LoadResourceText).GetTypeInfo().Assembly;
Stream stream = assembly.GetManifestResourceStream("WorkingWithFiles.PCLTextResource.txt");
string text = "";
using (var reader = new System.IO.StreamReader (stream)) {
    text = reader.ReadToEnd ();
}
```

`text` Proměnná pak lze zobrazit text nebo v opačném případě ho používat v kódu. Tento snímek obrazovky [ukázkovou aplikaci](https://developer.xamarin.com/samples/xamarin-forms/WorkingWithFiles/) zobrazí text v `Label` ovládacího prvku.

 [ ![Textový soubor vložených v PCL](files-images/pcltext-sml.png "Embedded textového souboru v PCL zobrazují v aplikaci")](files-images/pcltext.png "Embedded textového souboru v PCL zobrazují v aplikaci")

Načítání a deserializaci XML je stejně jednoduché. Následující kód ukazuje soubor XML se načíst a deserializovat z prostředku, pak vázána `ListView` pro zobrazení. Soubor XML obsahuje pole `Monkey` objekty (je třída definovaná v ukázkovém kódu).

```csharp
var assembly = typeof(LoadResourceText).GetTypeInfo().Assembly;
Stream stream = assembly.GetManifestResourceStream("WorkingWithFiles.PCLXmlResource.xml");
List<Monkey> monkeys;
using (var reader = new System.IO.StreamReader (stream)) {
    var serializer = new XmlSerializer(typeof(List<Monkey>));
    monkeys = (List<Monkey>)serializer.Deserialize(reader);
}
var listView = new ListView ();
listView.ItemsSource = monkeys;
```

 [ ![Soubor XML vložené do PCL, zobrazí v ListView](files-images/pclxml-sml.png "vloženého souboru XML v PCL zobrazí v ListView")](files-images/pclxml.png "vloženého souboru XML v PCL zobrazí v ListView")

<a name="Embedding_in_Shared_Projects" />

### <a name="embedding-in-shared-projects"></a>Vložení do sdílených projektů

Sdílených projektů může také obsahovat soubory jako vložené prostředky, ale protože obsah projektu sdíleného kompilovány do odkazující projekty, Předpona použitá pro vložený soubor prostředků, které můžete změnit ID. To znamená, že ID prostředku pro každý vložený soubor může být různé pro jednotlivé platformy.

Existují dvě řešení pro tento problém s sdílených projektů:

-  **Synchronizovat projektů** -upravit vlastnosti projektu pro každou platformu používat **stejné** sestavení název a výchozí obor názvů. Tato hodnota může být "pevně zakódované" pak jako předpona ID v projektu sdíleného vložený prostředek.
-  **direktivy #if kompilátoru** – direktivy kompilátoru slouží k nastavení předpona ID správný zdroj a použít tuto hodnotu dynamicky vytvořit ID správné prostředku.


Kód ilustrující druhou možností je uveden níže. Direktivy kompilátoru se používají k výběru předponu pevně zakódované prostředků (což je obvykle stejné jako výchozí obor názvů pro odkazující projekt). `resourcePrefix` Proměnná se pak použije k vytvoření ID platným prostředkem zřetězením s názvem souboru vložený prostředek.

```csharp
#if __IOS__
var resourcePrefix = "WorkingWithFiles.iOS.";
#endif
#if __ANDROID__
var resourcePrefix = "WorkingWithFiles.Droid.";
#endif
#if WINDOWS_PHONE
var resourcePrefix = "WorkingWithFiles.WinPhone.";
#endif

Debug.WriteLine("Using this resource prefix: " + resourcePrefix);
// note that the prefix includes the trailing period '.' that is required
var assembly = typeof(SharedPage).GetTypeInfo().Assembly;
Stream stream = assembly.GetManifestResourceStream
    (resourcePrefix + "SharedTextResource.txt");
```

<a name="Organizing_Resources" />

### <a name="organizing-resources"></a>Uspořádání prostředků

Příklady uvedené nahoře předpokládá, že soubor vložené v kořenu projektu PCL, ve kterém jsou rozlišována ID prostředku ve formátu **Namespace.Filename.Extension**, jako například `WorkingWithFiles.PCLTextResource.txt` a `WorkingWithFiles.iOS.SharedTextResource.txt`.

Je možné k uspořádání vložených prostředků ve složkách. Když je vložený prostředek umístěn ve složce, název složky stane součástí ID prostředku (odděleny tečkami), tak, aby se změní na formát ID prostředku **Namespace.Folder.Filename.Extension**. Umístění souborů použitých ukázková aplikace do složky **Moje_složka** by Zkontrolujte odpovídající prostředek ID `WorkingWithFiles.MyFolder.PCLTextResource.txt` a `WorkingWithFiles.iOS.MyFolder.SharedTextResource.txt`.

<a name="Debugging_Embedded_Resources" />

### <a name="debugging-embedded-resources"></a>Ladění vložené prostředky

Vzhledem k tomu, že je někdy složité pochopit, proč k určitému prostředku není načítá, následující kód ladění lze dočasně přidat k aplikaci kvůli ověření, že jsou správně nakonfigurovány prostředky. Výstup vložený do zadaného sestavení pro všechny známé prostředky **chyby** pad pomoci při ladění prostředků načítání problémy.

```csharp
using System.Reflection;
// ...
// use for debugging, not in released app code!
var assembly = typeof(SharedPage).GetTypeInfo().Assembly;
foreach (var res in assembly.GetManifestResourceNames()) {
    System.Diagnostics.Debug.WriteLine("found resource: " + res);
}
```

<a name="Loading_and_Saving_Files" />

## <a name="saving-and-loading-files"></a>Ukládání a načítání souborů

Vzhledem k platformě Xamarin.Forms běží na více platforem, každou s vlastním systému souborů, neexistuje žádný jeden přístup pro načítání a ukládání souborů vytvořený uživatelem. Abychom ukázali, jak pro uložení a načtení textových souborů ukázková aplikace zahrnuje k obrazovce, která uloží a načte některé uživatelský vstup - dokončení obrazovky je zobrazena níže:

 [ ![Ukládání a načítání textu](files-images/saveandload-sml.png "ukládání a načítání souborů v aplikaci")](files-images/saveandload.png "ukládání a načítání souborů v aplikaci")

Každá platforma má mírně odlišné adresářovou strukturu a možnosti různých filesystem – například Xamarin.iOS a Xamarin.Android podporují většinu `System.IO` funkce ale Windows Phone podporuje pouze `IsolatedStorage` a [ `Windows.Storage` ](http://msdn.microsoft.com/library/windowsphone/develop/jj681698(v=vs.105).aspx) Rozhraní API.

Pokud chcete tento problém vyřešit, definuje ukázková aplikace rozhraní PCL Xamarin.Forms načíst a ukládat soubory. Nabízí jednoduché rozhraní API k načtení a uložení textové soubory, které se uloží v zařízení.

```csharp
public interface ISaveAndLoad {
    void SaveText (string filename, string text);
    string LoadText (string filename);
}
```

PCL kód pak používá [DependencyService](~/xamarin-forms/app-fundamentals/dependency-service/index.md) získat odkaz na nativní implementaci rozhraní. To umožňuje kód přenosné delegovat načítání a ukládání souborů do třídy, které jsou napsané v všechny projekty specifické pro platformu. V ukázce **Uložit** a **zatížení** tlačítka se zapisují, jak je znázorněno:

```csharp
var saveButton = new Button {Text = "Save"};
saveButton.Clicked += (sender, e) => {
    DependencyService.Get<ISaveAndLoad>().SaveText("temp.txt", input.Text);
};
var loadButton = new Button {Text = "Load"};
loadButton.Clicked += (sender, e) => {
    output.Text = DependencyService.Get<ISaveAndLoad>().LoadText("temp.txt");
};
```

Implementace pak musí být přidán do jednotlivých projektů specifické pro platformu než soubory ve skutečnosti můžete uložit a načíst.

### <a name="ios-and-android"></a>iOS a Android

Implementace rozhraní pro Xamarin.iOS a Xamarin.Android projekty může být identické. Kód jsou uvedeny níže, včetně `[assembly: Dependency (typeof (SaveAndLoad))]` atribut, který je vyžadován pro `DependencyService` pracovat.

```csharp
[assembly: Dependency (typeof (SaveAndLoad))]
namespace WorkingWithFiles {
    public class SaveAndLoad : ISaveAndLoad {
        public void SaveText (string filename, string text) {
            var documentsPath = Environment.GetFolderPath (Environment.SpecialFolder.Personal);
            var filePath = Path.Combine (documentsPath, filename);
            System.IO.File.WriteAllText (filePath, text);
        }
        public string LoadText (string filename) {
            var documentsPath = Environment.GetFolderPath (Environment.SpecialFolder.Personal);
            var filePath = Path.Combine (documentsPath, filename);
            return System.IO.File.ReadAllText (filePath);
        }
    }
}
```

### <a name="universal-windows-platform-uwp-windows-81-and-windows-phone-81"></a>Univerzální platformu Windows (UWP), Windows 8.1 a Windows Phone 8.1

Tyto platformy mít jiný rozhraní API – systém souborů [ `Windows.Storage` ](/windows/uwp/files/quickstart-reading-and-writing-files/) – to znamená používá k uložení a načtení souborů.
`ISaveAndLoad` Rozhraní může být implementováno, jak je uvedeno níže:

```csharp
using System;
using System.Threading.Tasks;
using Windows.Storage;
using WinApp;
using WorkingWithFiles;
using Xamarin.Forms;

[assembly: Dependency(typeof(SaveAndLoad_WinApp))]

namespace WindowsApp
{
    // https://msdn.microsoft.com/library/windows/apps/xaml/hh758325.aspx
    public class SaveAndLoad_WinApp : ISaveAndLoad
    {
        public async Task SaveTextAsync(string filename, string text)
        {
            StorageFolder localFolder = ApplicationData.Current.LocalFolder;
            StorageFile sampleFile = await localFolder.CreateFileAsync(filename, CreationCollisionOption.ReplaceExisting);
            await FileIO.WriteTextAsync(sampleFile, text);
        }
        public async Task<string> LoadTextAsync(string filename)
        {
            StorageFolder storageFolder = ApplicationData.Current.LocalFolder;
            StorageFile sampleFile = await storageFolder.GetFileAsync(filename);
            string text = await Windows.Storage.FileIO.ReadTextAsync(sampleFile);
            return text;
        }
    }
}
```


<a name="Saving_and_Loading_in_Shared_Projects" />

### <a name="saving-and-loading-in-shared-projects"></a>Ukládání a načítání ve sdílených projektů

Vzhledem k tomu sdílených projektů podporují direktivy kompilátoru je možné zahrnout všechny specifické pro platformu kód v souboru jednu třídu do projektu sdíleného (bez použití `DependencyService`).
Jediný `SaveAndLoad` třída může zapisovat obsahující obou výše, implementace selektivně zkompilovat do odkazující projektů pomocí direktivy kompilátoru jako `#if WINDOWS_PHONE`, `#if __IOS__`, a `#if __ANDROID__`.

## <a name="additional-information"></a>Další informace

Na základě PCL Xamarin.Forms projekty také můžete využít výhod [PCLStorage NuGet](http://www.nuget.org/packages/pclstorage) ([kód &amp; dokumentace](https://pclstorage.codeplex.com/)) při provádění operací se soubory způsobem napříč platformami.


## <a name="summary"></a>Souhrn

Tento dokument ukazuje některé operace jednoduchého souboru pro načítání vložené prostředky a ukládání a načítání textu v zařízení. Vývojářům můžete implementovat vlastní nativní souboru rozhraní API pomocí `DependencyService`, což komplexní vyžadovaná ke zpracování jejich požadavky na zpracování souboru.


## <a name="related-links"></a>Související odkazy

- [FilesSample](https://developer.xamarin.com/samples/xamarin-forms/WorkingWithFiles/)
- [Vytváření, zápisu a čtení souboru (UWP)](/windows/uwp/files/quickstart-reading-and-writing-files/)
- [Ukázky Xamarin.Forms](https://github.com/xamarin/xamarin-forms-samples)
- [Soubory sešitu](https://developer.xamarin.com/workbooks/xamarin-forms/application-fundamentals/files/files.workbook)
