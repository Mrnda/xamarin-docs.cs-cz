---
title: Přístup k souborům se Xamarin.androidem
description: Tato příručka vysvětluje, přístup k souborům v Xamarin.Android
ms.prod: xamarin
ms.assetid: FC1CFC58-B799-4DD6-8ED1-DE36B0E56856
ms.technology: xamarin-android
author: topgenorth
ms.author: toopge
ms.date: 07/23/2018
ms.openlocfilehash: 5a4ddf606bb71bef10cf99660c198c5a8fdb1b69
ms.sourcegitcommit: 9bb9e8297d3edd9a50585f4ba53c1b4f0bcd1d3e
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/23/2018
ms.locfileid: "39212148"
---
# <a name="file-storage-and-access-with-xamarinandroid"></a>File Storage a přístup se Xamarin.androidem

Běžné požadavky pro aplikace pro Android se k práci se soubory &ndash; ukládá obrázky, stahování dokumentů nebo exportu dat sdílet s jinými programy. Android (která je založena na systému Linux) podporuje to tím, že poskytuje místo pro ukládání souborů. Android seskupí do dvou různých typů úložišť systému souborů:

* **Interní úložiště** &ndash; Toto je část systému souborů, který je přístupný pouze operační systém nebo aplikace.
* **Externí úložiště** &ndash; to je oddíl pro ukládání souborů, který je přístupný pro všechny aplikace, uživatele a případně dalších zařízení. Na některých zařízeních může být externím úložišti vyměnitelné (jako jsou SD karty).

Tyto skupiny jsou pouze koncepční a není nutně odkazovat na jeden oddíl nebo adresáře na zařízení. Zařízení s Androidem bude vždy poskytovat oddílu pro interní a externí úložiště. Je možné, že některá zařízení můžou mít více oddílů, které jsou považovány za externí úložiště. Bez ohledu na oddíl rozhraní API pro čtení zápis a vytváření souborů je stejný. Existují dvě sady rozhraní API, která může používat aplikace pro Xamarin.Android pro přístup k souborům:

1. **Rozhraní API .NET (poskytované Mono a zabalené Xamarin.Android)** &ndash; tyto zahrnuje [pomocné rutiny systému souborů](~/essentials/file-system-helpers.md?context=xamarin/android) poskytované [Xamarin.Essentials](~/essentials/index.md?context=xamarin/android). Rozhraní API .NET poskytují nejlepší kompatibility napříč platformami a jako takové výběru tohoto průvodce bude tato rozhraní API.
1. **Nativní přístup rozhraní API (poskytované Java a zabalené Xamarin.Android) soubor Java** &ndash; Java poskytuje vlastní rozhraní API pro čtení a zápis do souborů. Tyto jsou zcela přijatelná alternativa k k rozhraním API .NET, ale jsou specifické pro Android a nejsou vhodné pro aplikace, které mají být napříč platformami.

Čtení a zápis do souborů je téměř shodné v Xamarin.Android, jako je na jakékoli jiné aplikace .NET. Aplikace Xamarin.Android Určuje cestu k souboru, který bude manipulovat, pak používá standardní .NET idiomy pro přístup k souborům. Protože skutečné cesty k interní a externí úložiště se můžou lišit od zařízení nebo z verze Androidu na verzi Androidu, nedoporučuje se pevný kód cestu k souborům. Místo toho použijte rozhraní API pro Xamarin.Android určit cestu k souborům. Tímto způsobem rozhraní .NET API pro čtení a zápis do souborů poskytuje nativní Android rozhraní API, která vám pomůže s určení cesty k souborům na interní a externí úložiště.

Před diskuze o rozhraní API, které jsou spojené s přístup k souborům, je důležité pochopit některé podrobnosti okolní interní a externí úložiště. To probereme v další části.

## <a name="internal-vs-external-storage"></a>Interní a externí úložiště

Entity jsou velmi podobné interní a externí úložiště &ndash; jsou na obou místech, na kterých aplikace Xamarin.Android může ukládat soubory. Tato podobnosti může být matoucí pro vývojáře, kteří nejsou obeznámeni s Androidem, protože není jasné, kdy aplikace měli použít interní úložiště vs externího úložiště.

Interní úložiště odkazuje na stálé paměti, který přiděluje Android do operačního systému, soubory Apk a pro jednotlivé aplikace. Zde není přístupná s výjimkou operačního systému nebo aplikace. Android se přidělí adresáře v oddílu interní úložiště pro každou aplikaci. Když se aplikace odinstaluje, budou odstraněny také všechny soubory, které jsou uloženy v interní úložiště v tomto adresáři. Interní úložiště je nejvhodnější pro soubory, které jsou přístupné pouze pro aplikace a nebude sdílet s jinými aplikacemi nebo bude mít velmi malou hodnotu po odinstalaci aplikace. Na Android 6.0 nebo novějším, soubory na interní úložiště může být automaticky zálohovány pomocí Google [funkce automatického zálohování v Android 6.0](https://developer.android.com/guide/topics/data/autobackup). Interní úložiště má následující nevýhody:

* Soubory nelze sdílet.
* Soubory se odstraní, když se aplikace odinstaluje.
* Volné místo na interní úložiště možná omezen.

Externí úložiště odkazuje na úložiště souborů, které není interní úložiště a výhradně k nemá přístup aplikaci. Primárním účelem externího úložiště je zadejte umístění pro soubory, které jsou určené ke sdílení mezi aplikacemi nebo které jsou příliš velký a nevejde se na interní úložiště. Využívat externí úložiště je, že je obvykle mnohem více místa pro soubory interní úložiště. Externí úložiště však není vždy zaručeně nacházet na zařízení a může vyžadovat zvláštní oprávnění uživatele pro přístup k ní.

> [!NOTE]
> Pro zařízení, které podporují více uživatelů Android budou pro každého uživatele jejich vlastní adresáře na interní a externí úložiště. Tento adresář je přístupný ostatním uživatelům na zařízení. Toto oddělení je viditelný pro aplikace, dokud nebudou pevně cesty k souborům na interní nebo externí úložiště.

Jako říci by měla aplikace Xamarin.Android raději ukládat soubory na interní úložiště, když je možné logicky a Spolehněte se na externí úložiště souborů potřeba sdílet s jinými aplikacemi, jsou velmi velké, nebo se uchovávají i v případě, že aplikace se odinstaluje. Například konfigurační soubor je nejvhodnější pro interní úložiště, protože nemá žádný význam s výjimkou aplikaci, která ji vytvoří.  Naproti tomu fotografie jsou vhodnými kandidáty pro externí úložiště. Můžou být velké a v mnoha případech uživatel může chtít sdílet nebo k nim přístup i v případě, že aplikace se odinstaluje.

Tato příručka je zaměřena na interní úložiště. Podrobnosti najdete v příručce [externího úložiště](~/android/platform/files/external-storage.md) podrobnosti o použití externího úložiště do aplikace Xamarin.Android.

## <a name="working-with-internal-storage"></a>Práce s interní úložiště

Adresář interní úložiště pro aplikace se určuje podle operačního systému a je přístupný pomocí aplikace pro Android `Android.Content.Context.FilesDir` vlastnost. Vrátí `Java.IO.File` objekt představující adresáře, který Android věnoval výhradně pro aplikaci.  Například aplikace pomocí názvu balíčku **com.companyname** adresář interní úložiště může být:

```bash
/data/user/0/com.companyname/files
```

Tento dokument bude odkazovat na adresář interní úložiště jako _interní\_úložiště_.

> [!IMPORTANT]
> Přesnou cestu k adresáři interní úložiště se může lišit od zařízení na zařízení a mezi verzemi systému Android. Z tohoto důvodu aplikace musí není pevně kódu cestu k adresáři interních souborů úložiště a místo toho použít rozhraní API pro Xamarin.Android, například `System.Environment.GetFolderPath()`.

Pokud chcete maximalizovat sdílení kódu, by měla použít aplikace Xamarin.Android (nebo aplikace Xamarin.Forms, které jsou určené pro Xamarin.Android) [ `System.Environment.GetFolderPath()` ](xref:System.Environment.GetFolderPath*) metody. V Xamarin.Android, tato metoda vrátí řetězec pro adresář, který je na stejné umístění jako `Android.Content.Context.FilesDir`. Tato metoda přebírá výčtu, `System.Environment.SpecialFolder`, který se používá k identifikaci sady pro výčtové konstanty, které představují cesty speciální složky, které jsou použity v operačním systému. Ne všechny `System.Environment.SpecialFolder` hodnoty se namapuje na platný adresář v Xamarin.Android. Následující tabulka popisuje, jaké cesta je možné očekávat ohledně zadanou hodnotu `System.Environment.SpecialFolder`:

| System.Environment.SpecialFolder | Cesta  |
|----------------------|---|
| `ApplicationData` | **_INTERNÍ\_úložiště_/.config** |
| `Desktop` | **_INTERNÍ\_úložiště_  /klasické pracovní plochy** |
| `LocalApplicationData` | **_INTERNÍ\_úložiště_/.local/share** |
| `MyComputer` | **_INTERNÍ\_úložiště_/.local/share** |
| `MyDocuments` | **_INTERNÍ\_ÚLOŽIŠTĚ_** |
| `MyMusic` | **_INTERNÍ\_úložiště_/Music** |
| `MyPictures` | **_INTERNÍ\_úložiště_/Music** |
| `MyVideos` | **_INTERNÍ\_úložiště_/Videos** |
| `Personal` | **_INTERNÍ\_ÚLOŽIŠTĚ_** |


### <a name="reading-or-writing-to-files-on-internal-storage"></a>Čtení nebo zápis do souborů na interní úložiště

Některé z [rozhraní API jazyka C# pro zápis](https://docs.microsoft.com/dotnet/csharp/programming-guide/file-system/how-to-write-to-a-text-file) do souboru je dostatečné; vše, co je nutné se k získání cesty k souboru, který je umístěn v adresáři přidělené na aplikaci. Důrazně doporučujeme, že asynchronní verze rozhraní .NET API se používají k minimalizovat problémy, které mohou být přidružit k přístupu k souborům blokování hlavního vlákna.

Tento fragment kódu je jedním z příkladů psaní celého čísla do textového souboru do adresáře interní úložiště aplikace UTF-8:

```csharp
public async Task SaveCountAsync(int count)
{
    var backingFile = Path.Combine(System.Environment.GetFolderPath(System.Environment.SpecialFolder.Personal), "count.txt");
    using (var writer = File.CreateText(backingFile))
    {
        await writer.WriteLineAsync(count.ToString());
    }
}
```

Další fragment kódu obsahuje jeden způsob, jak načíst celočíselnou hodnotu, která byla uložená v textovém souboru:

```csharp
public async Task<int> ReadCountAsync()
{
    var backingFile = Path.Combine(System.Environment.GetFolderPath(System.Environment.SpecialFolder.Personal), "count.txt");

    if (backingFile == null || !File.Exists(backingFile))
    {
        return 0;
    }

    var count = 0;
    using (var reader = new StreamReader(backingFile, true))
    {
        string line;
        while ((line = await reader.ReadLineAsync()) != null)
        {
            if (int.TryParse(line, out var newcount))
            {
                count = newcount;
            }
        }
    }

    return count;
}
```

## <a name="using--xamarinessentials-ndash-file-system-helpers"></a>Pomocí Xamarin.Essentials &ndash; pomocné rutiny systému souborů

[Xamarin.Essentials](~/essentials/file-system-helpers.md?context=xamarin/android) představuje sadu rozhraní API pro psaní kódu napříč platformami kompatibilní. [Pomocné rutiny systému souborů](~/essentials/file-system-helpers.md?context=xamarin/android) je třída, která obsahuje řadu pomocné rutiny pro zjednodušení vyhledávání adresářů mezipaměti a data vaší aplikace. Tento fragment kódu znázorňuje, jak najít adresář interní úložiště a adresář mezipaměti pro aplikace:

```csharp
// Get the path to a file on internal storage
var backingFile = Path.Combine(Xamarin.Essentials.FileSystem.AppDataDirectory, "count.txt");

// Get the path to a file in the cache directory
var cacheFile = Path.Combine(Xamarin.Essentials.FileSystem.CacheDirectory, "count.txt");
```

## <a name="hiding-files-from-the-mediastore"></a>Skrytí soubory z `MediaStore`

`MediaStore` Je komponentu s Androidem, která shromažďuje metadata o mediálních souborů (videa, Hudba, obrázky) na zařízení s Androidem. Cílem je zjednodušit sdílení těchto souborů přes všechny aplikace pro Android na zařízení.

Soubory soukromých nezobrazí jako ke sdílení médií. Například pokud aplikace obrázek uloží do své soukromé externí úložiště, pak tento soubor nebude vyzvednutí skenerem média (`MediaStore`).

Veřejné soubory neexistoval, použije podle `MediaStore`. Adresáře, které mají nula bajtů názvu souboru **. NOMEDIA** nebude skenovalo `MediaStore`.

## <a name="related-links"></a>Související odkazy

* [Externí úložiště](~/android/platform/files/external-storage.md)
* [Ukládání souborů na zařízení úložiště](https://developer.android.com/training/data-storage/files)
* [Pomocné rutiny systému souborů Xamarin.Essentials](~/essentials/file-system-helpers.md?context=xamarin/android)
* [Data záloh uživatele s automatické zálohování](https://developer.android.com/guide/topics/data/autobackup)
* [Adoptable úložiště](https://source.android.com/devices/storage/adoptable)
