---
title: "Jak lze ručně nainstalovat Android, podporují knihovny potřebné balíčky Xamarin.Android.Support?"
ms.topic: article
ms.prod: xamarin
ms.assetid: A9CB8CA8-8A6D-405E-B84C-A16CE452C0F7
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/16/2018
ms.openlocfilehash: 26dd7e23352bf0911c2a7268518ddebf6626596a
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 02/27/2018
---
# <a name="how-can-i-manually-install-the-android-support-libraries-required-by-the-xamarinandroidsupport-packages"></a>Jak lze ručně nainstalovat Android, podporují knihovny potřebné balíčky Xamarin.Android.Support?

## <a name="example-steps-for-xamarinandroidsupportv4"></a>Příklady kroků pro Xamarin.Android.Support.v4 

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Stáhněte požadované balíček Xamarin.Android.Support NuGet (například nainstalováním ji pomocí Správce balíčků NuGet).

Použití `ildasm` ke kontrole, kterou verzi **android_m2repository.zip** balíček NuGet musí:

```cmd
ildasm /caverbal /text /item:Xamarin.Android.Support.v4 packages\Xamarin.Android.Support.v4.23.4.0.1\lib\MonoAndroid403\Xamarin.Android.Support.v4.dll | findstr SourceUrl
```
Příklad výstupu:

```cmd
property string 'SourceUrl' = string('https://dl-ssl.google.com/android/repository/android_m2repository_r32.zip')
property string 'SourceUrl' = string('https://dl-ssl.google.com/android/repository/android_m2repository_r32.zip')
property string 'SourceUrl' = string('https://dl-ssl.google.com/android/repository/android_m2repository_r32.zip')
```

Stáhněte si **android\_m2repository.zip** z Google pomocí adresy URL vrátil z **ildasm**. Alternativně můžete zkontrolovat kterou verzi _Android úložiště podporu_ je již nainstalován v Android SDK Manager:

!["Android SDK Manager zobrazující Android podporu úložiště verze 32 nainstalována"](install-android-support-library-images/sdk-extras.png)

Pokud verze odpovídá je ta, kterou potřebujete pro balíček NuGet, pak nemusíte stahovat žádné nové. Vám může místo toho znovu zip existující **m2repository** adresář, který se nachází v **funkce\\android** v _SDK cesta_ (jak je znázorněno horní části pro Android Okno Správce SDK).

Vypočítat hodnotu hash MD5 adresy URL, kterou vrátil **ildasm**. Formátu výsledný řetězec používat všechny velká písmena a mezery. Například upravit `$url` jako proměnnou potřebné a poté spusťte následující řádky 2 (na základě [původní kódu C# z Xamarin.Android](https://github.com/xamarin/xamarin-android/blob/8e8a4dd90f26eb39172876cc52181b6639e20524/src/Xamarin.Android.Build.Tasks/Tasks/GetAdditionalResourcesFromAssemblies.cs#L208)) v prostředí PowerShell:

```powershell
$url = "https://dl-ssl.google.com/android/repository/android_m2repository_r32.zip"
(([System.Security.Cryptography.MD5]::Create()).ComputeHash([System.Text.Encoding]::UTF8.GetBytes($url)) | %{ $_.ToString("X02") }) -join ""
```
Příklad výstupu:

```powershell
F16A3455987DBAE5783F058F19F7FCDF
```

Kopírování **android\_m2repository.zip** do **LOCALAPPDATA %\\Xamarin\\Zpráva prolétne\\**  složky. Přejmenujte soubor použít hodnotu hash MD5 z předchozí hodnotu hash MD5 výpočet krok. Příklad:

**%LOCALAPPDATA%\\Xamarin\\zips\\F16A3455987DBAE5783F058F19F7FCDF.zip**

(Volitelné) Rozbalte soubor do **LOCALAPPDATA %\\Xamarin\\Xamarin.Android.Support.v4\\23.4.0.0\\obsah\\**  (vytváření **obsah\\m2repository** podadresáři). Pokud tento krok přeskočíte, pak první sestavení, která používá knihovnu bude trvat trochu déle, protože ho budete potřebovat k dokončení tohoto kroku.
Číslo verze podadresář (**23.4.0.0** v tomto příkladu) není dost stejná jako verze balíčku NuGet. Můžete použít `ildasm` najít číslo správnou verzi:

```cmd
ildasm /caverbal /text /item:Xamarin.Android.Support.v4 packages\Xamarin.Android.Support.v4.23.4.0.1\lib\MonoAndroid403\Xamarin.Android.Support.v4.dll | findstr /C:"string 'Version'"
```
Příklad výstupu:

```cmd
property string 'Version' = string('23.4.0.0')}
property string 'Version' = string('23.4.0.0')}
property string 'Version' = string('23.4.0.0')}
```

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

Stáhněte požadované balíček Xamarin.Android.Support NuGet (například nainstalováním ji pomocí Správce balíčků NuGet).

Dvakrát klikněte _Xamarin.Android.Support.v4_ sestavení v části _odkazy_ části projektu pro Android v sadě Visual Studio pro Mac otevřete sestavení v prohlížeči sestavení. Ujistěte se, že _jazyk_ rozevíracího seznamu je nastaven na _C#_ a vyberte nejvyšší úrovně _Xamarin.Android.Support.v4_ sestavení z prohlížeče sestavení navigačním stromu. Vyhledejte `SourceUrl` vlastností v rámci jednoho z `IncludeAndroidResourcesFrom` nebo `JavaLibraryReference` atributy:

```csharp
[assembly: IncludeAndroidResourcesFrom ("./", PackageName = "Xamarin.Android.Support.v4", SourceUrl = "https://dl-ssl.google.com/android/repository/android_m2repository_r32.zip", EmbeddedArchive = "m2repository/com/android/support/support-v4/23.4.0/support-v4-23.4.0.aar", Version = "23.4.0.0")]
```

Stáhněte si **android\_m2repository.zip** pomocí Google `SourceUrl` vrácená z **ildasm**. Alternativně můžete zkontrolovat kterou verzi _Android úložiště podporu_ je již nainstalován v Android SDK Manager:

!["Android SDK Manager zobrazující Android podporu úložiště verze 32 nainstalována"](install-android-support-library-images/sdk-extras.png)

Pokud verze odpovídá je ta, kterou potřebujete pro balíček NuGet, pak nemusíte stahovat žádné nové. Vám může místo toho znovu zip existující **m2repository** adresář, který se nachází v **funkce/android** v _SDK cesta_ (jak je znázorněno horní části okna Android SDK Manager) .

Vypočítat hodnotu hash MD5 adresy URL, kterou vrátil **ildasm**. Formátu výsledný řetězec používat všechny velká písmena a mezery. Například řetězce adresy URL podle potřeby upravte a poté spusťte následující příkaz **Terminal.app** příkazového řádku:

```bash
echo -n "https://dl-ssl.google.com/android/repository/android_m2repository_r32.zip" | md5 | tr '[:lower:]' '[:upper:]'
```

Další možností je použít `csharp` překladač ke spuštění [stejné C# kód, který používá Xamarin.Android, samotné](https://github.com/xamarin/xamarin-android/blob/8e8a4dd90f26eb39172876cc52181b6639e20524/src/Xamarin.Android.Build.Tasks/Tasks/GetAdditionalResourcesFromAssemblies.cs#L208).
Upravit tak, aby to udělat, `url` jako proměnnou potřebné a poté spusťte následující příkaz **Terminal.app** příkazového řádku:

```bash
csharp -e 'var url = "https://dl-ssl.google.com/android/repository/android_m2repository_r32.zip"; string.Concat((System.Security.Cryptography.MD5.Create().ComputeHash(System.Text.Encoding.UTF8.GetBytes(url))).Select(b => b.ToString("X02")))'
```
Příklad výstupu:

```bash
F16A3455987DBAE5783F058F19F7FCDF
```

Kopírování **android\_m2repository.zip** k **$HOME/.local/share/Xamarin/zips/** složky. Přejmenujte soubor použít hodnotu hash MD5 z předchozí hodnotu hash MD5 výpočet krok. Příklad:

**$HOME/.local/share/Xamarin/zips/F16A3455987DBAE5783F058F19F7FCDF.zip**

(Volitelné) Rozbalte soubor do: 

**$HOME/.local/share/Xamarin/Xamarin.Android.Support.v4/23.4.0.0/content/**

(vytváření **obsah nebo m2repository** podadresáři). Pokud tento krok přeskočíte, pak první sestavení, která používá knihovnu bude trvat trochu déle, protože ho budete potřebovat k dokončení tohoto kroku.

Číslo verze podadresář (**23.4.0.0** v tomto příkladu) není dost stejná jako verze balíčku NuGet. Jako v **ildasm** krok dříve, můžete použít prohlížeč sestavení v sadě Visual Studio pro Mac najít číslo správnou verzi. Vyhledejte `Version` vlastností v rámci jednoho z `IncludeAndroidResourcesFrom` nebo `JavaLibraryReference` atributy:

```csharp
[assembly: IncludeAndroidResourcesFrom ("./", PackageName = "Xamarin.Android.Support.v4", SourceUrl = "https://dl-ssl.google.com/android/repository/android_m2repository_r32.zip", EmbeddedArchive = "m2repository/com/android/support/support-v4/23.4.0/support-v4-23.4.0.aar", Version = "23.4.0.0")]
```

-----


## <a name="additional-references"></a>Další odkazy

- [Chyby 43245](https://bugzilla.xamarin.com/show_bug.cgi?id=43245) – nepřesného "stažení se nezdařilo. "Stáhnout {0} a umístí jej do adresáře {1}." a ", nainstalujte balíček: {0}' k dispozici v instalační program sady SDK" chybové zprávy související s Xamarin.Android.Support balíčky

### <a name="next-steps"></a>Další kroky

Tento dokument popisuje aktuální chování od srpna 2016. Podle postupu popsaného v tomto dokumentu není součástí stabilní testování suite pro Xamarin, takže může v budoucnu rozdělit.

O další pomoc, kontaktujte nás, nebo pokud tento problém zůstane i po použití výše uvedené informace najdete v tématu [jaké možnosti podpory jsou dostupná pro Xamarin?](~/cross-platform/troubleshooting/support-options.md) informace o možnostech kontaktní, návrhy, a také jak soubor nové chyby v případě potřeby.

