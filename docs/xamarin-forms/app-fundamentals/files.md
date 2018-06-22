---
title: Zpracování souborů v Xamarin.Forms
description: Práce s Xamarin.Forms se soubory se dá dosáhnout pomocí kódu v rozhraní .NET standardní knihovny, nebo pomocí vložené prostředky.
ms.prod: xamarin
ms.assetid: 9987C3F6-5F04-403B-BBB4-ECB024EA6CC8
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 06/21/2018
ms.openlocfilehash: 0be441a7be9777698212e690aca95fdd75e5050f
ms.sourcegitcommit: eac092f84b603958c761df305f015ff84e0fad44
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 06/21/2018
ms.locfileid: "36310150"
---
# <a name="file-handling-in-xamarinforms"></a>Zpracování souborů v Xamarin.Forms

_Práce s Xamarin.Forms se soubory se dá dosáhnout pomocí kódu v rozhraní .NET standardní knihovny, nebo pomocí vložené prostředky._

## <a name="overview"></a>Přehled

Spustí kód Xamarin.Forms ve více platformách - z nichž každá má svou vlastní systému souborů. Dříve vynutila si snadno byl čtení a zápis souborů provést pomocí nativních rozhraní API souboru na každou platformu. Alternativně vložené prostředky jsou jednodušší řešení k distribuci datových souborů s aplikací. S standardní rozhraní .NET 2.0 je možné sdílet soubor přístupového kódu v rozhraní .NET standardní knihovny.

Informace o zpracování souborů bitové kopie, naleznete [práce s obrázky](~/xamarin-forms/user-interface/images.md) stránky.

<a name="Loading_and_Saving_Files" />

## <a name="saving-and-loading-files"></a>Ukládání a načítání souborů

`System.IO` Třídy lze použít pro přístup k systému souborů na každou platformu. `File` Třída umožňuje vytvářet, mazat a číst soubory a `Directory` třída umožňuje vytvořit, odstranit nebo vytvořit výčet obsah adresáře. Můžete také `Stream` podtřídy, což může poskytnout vyšší stupeň kontroly nad operací se soubory (jako je hledání kompresi nebo pozice v souboru).

Textový soubor lze zapsat pomocí `File.WriteAllText` metoda:

```csharp
File.WriteAllText(fileName, text);
```

Textový soubor lze číst pomocí `File.ReadAllText` metoda:

```csharp
string text = File.ReadAllText(fileName);
```

Kromě toho `File.Exists` metoda určuje, zda zadaný soubor existuje:

```csharp
bool doesExist = File.Exists(fileName);
```

Cestu k souboru na každou platformu můžete určit z .NET standardní knihovny pomocí hodnotu [ `Environment.SpecialFolder` ](xref:System.Environment.SpecialFolder) výčet jako první argument `Environment.GetFolderPath` metoda. To je pak možné kombinovat s název souboru s `Path.Combine` metoda:

```csharp
string fileName = Path.Combine(Environment.GetFolderPath(Environment.SpecialFolder.LocalApplicationData), "temp.txt");
```

Tyto operace jsou ukázáno v ukázkové aplikace, která zahrnuje stránky, který se uloží a načte text:

[![Ukládání a načítání textu](files-images/saveandload-sml.png "ukládání a načítání souborů v aplikaci")](files-images/saveandload.png#lightbox "ukládání a načítání souborů v aplikaci")

<a name="Loading_Files_Embedded_as_Resources" />

## <a name="loading-files-embedded-as-resources"></a>Načítání souborů vložených jako prostředky

Chcete-li vložit soubor do **.NET Standard** sestavení, vytvořit nebo přidat soubor a ujistěte se, že **akce sestavení: EmbeddedResource**.

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![Konfigurace vložených akce sestavení prostředku](files-images/vs-embeddedresource-sml.png "nastavení EmbeddedResource BuildAction")](files-images/vs-embeddedresource.png#lightbox "EmbeddedResource BuildAction nastavení")

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

[![Textový soubor vložených v PCL, konfigurace akce sestavení vložený prostředek](files-images/xs-embeddedresource-sml.png "nastavení EmbeddedResource BuildAction")](files-images/xs-embeddedresource.png#lightbox "EmbeddedResource BuildAction nastavení")

-----

`GetManifestResourceStream` slouží k přístupu ke vloženého souboru pomocí jeho **ID prostředku**. Ve výchozím nastavení je název souboru s předponou výchozí obor názvů pro projekt je integrovaný v - ID prostředku v tomto případě sestavení je **WorkingWithFiles** a název souboru je **PCLTextResource.txt**, Proto je ID prostředku `WorkingWithFiles.PCLTextResource.txt`.

```csharp
var assembly = IntrospectionExtensions.GetTypeInfo(typeof(LoadResourceText)).Assembly;
Stream stream = assembly.GetManifestResourceStream("WorkingWithFiles.PCLTextResource.txt");
string text = "";
using (var reader = new System.IO.StreamReader (stream)) {
    text = reader.ReadToEnd ();
}
```

`text` Proměnná pak lze zobrazit text nebo v opačném případě ho používat v kódu. Tento snímek obrazovky [ukázkovou aplikaci](https://developer.xamarin.com/samples/xamarin-forms/WorkingWithFiles/) zobrazí text v `Label` ovládacího prvku.

 [![Textový soubor vložených v PCL](files-images/pcltext-sml.png "Embedded textového souboru v PCL zobrazují v aplikaci")](files-images/pcltext.png#lightbox "Embedded textového souboru v PCL zobrazují v aplikaci")

Načítání a deserializaci XML je stejně jednoduché. Následující kód ukazuje soubor XML se načíst a deserializovat z prostředku, pak vázána `ListView` pro zobrazení. Soubor XML obsahuje pole `Monkey` objekty (je třída definovaná v ukázkovém kódu).

```csharp
var assembly = IntrospectionExtensions.GetTypeInfo(typeof(LoadResourceText)).Assembly;
Stream stream = assembly.GetManifestResourceStream("WorkingWithFiles.PCLXmlResource.xml");
List<Monkey> monkeys;
using (var reader = new System.IO.StreamReader (stream)) {
    var serializer = new XmlSerializer(typeof(List<Monkey>));
    monkeys = (List<Monkey>)serializer.Deserialize(reader);
}
var listView = new ListView ();
listView.ItemsSource = monkeys;
```

 [![Soubor XML vložené do PCL, zobrazí v ListView](files-images/pclxml-sml.png "vloženého souboru XML v PCL zobrazí v ListView")](files-images/pclxml.png#lightbox "vloženého souboru XML v PCL zobrazí v ListView")

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

Debug.WriteLine("Using this resource prefix: " + resourcePrefix);
// note that the prefix includes the trailing period '.' that is required
var assembly = IntrospectionExtensions.GetTypeInfo(typeof(SharedPage)).Assembly;
Stream stream = assembly.GetManifestResourceStream
    (resourcePrefix + "SharedTextResource.txt");
```

<a name="Organizing_Resources" />

### <a name="organizing-resources"></a>Uspořádání prostředků

Příklady uvedené nahoře předpokládají, který je vložený soubor v kořenovém .NET Standard projektu knihovny, ve kterém jsou rozlišována ID prostředku ve formátu **Namespace.Filename.Extension**, jako například `WorkingWithFiles.PCLTextResource.txt` a `WorkingWithFiles.iOS.SharedTextResource.txt`.

Je možné k uspořádání vložených prostředků ve složkách. Když je vložený prostředek umístěn ve složce, název složky stane součástí ID prostředku (odděleny tečkami), tak, aby se změní na formát ID prostředku **Namespace.Folder.Filename.Extension**. Umístění souborů použitých ukázková aplikace do složky **Moje_složka** by Zkontrolujte odpovídající prostředek ID `WorkingWithFiles.MyFolder.PCLTextResource.txt` a `WorkingWithFiles.iOS.MyFolder.SharedTextResource.txt`.

<a name="Debugging_Embedded_Resources" />

### <a name="debugging-embedded-resources"></a>Ladění vložené prostředky

Vzhledem k tomu, že je někdy složité pochopit, proč k určitému prostředku není načítá, následující kód ladění lze dočasně přidat k aplikaci kvůli ověření, že jsou správně nakonfigurovány prostředky. Výstup vložený do zadaného sestavení pro všechny známé prostředky **chyby** pad pomoci při ladění prostředků načítání problémy.

```csharp
using System.Reflection;
// ...
// use for debugging, not in released app code!
var assembly = IntrospectionExtensions.GetTypeInfo(typeof(SharedPage)).Assembly;
foreach (var res in assembly.GetManifestResourceNames()) {
    System.Diagnostics.Debug.WriteLine("found resource: " + res);
}
```

## <a name="summary"></a>Souhrn

Tento článek ukazuje některé operace jednoduchého souboru pro ukládání a načítání textu na zařízení a pro načítání vložené prostředky. S standardní rozhraní .NET 2.0 je možné sdílet soubor přístupového kódu v rozhraní .NET standardní knihovny.

## <a name="related-links"></a>Související odkazy

- [FilesSample](https://developer.xamarin.com/samples/xamarin-forms/WorkingWithFiles/)
- [Ukázky Xamarin.Forms](https://github.com/xamarin/xamarin-forms-samples)
- [Práce s systém souborů ve Xamarin.iOS](~/ios/app-fundamentals/file-system.md)
- [Soubory sešitu](https://developer.xamarin.com/workbooks/xamarin-forms/application-fundamentals/files/files.workbook)
