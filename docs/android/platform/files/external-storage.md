---
title: Přístup k souborům v externích úložištích, se Xamarin.androidem
description: Tato příručka popisuje přístup k souborům v externích úložištích, v Xamarin.Android
ms.prod: xamarin
ms.assetid: 40da10b2-a207-4f9c-a2dd-165d9b662f33
ms.technology: xamarin-android
author: topgenorth
ms.author: toopge
ms.date: 07/23/2018
ms.openlocfilehash: 380100d38febf567fde94096455fd846d9d3d2d3
ms.sourcegitcommit: 9bb9e8297d3edd9a50585f4ba53c1b4f0bcd1d3e
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/23/2018
ms.locfileid: "39212149"
---
# <a name="external-storage"></a>Externí úložiště

Externí úložiště odkazuje na úložiště souborů, které nejsou v interní úložiště a nikoli výhradně přístupný aplikaci, která je zodpovědná za soubor. Primárním účelem externího úložiště je zadejte umístění pro soubory, které jsou určené ke sdílení mezi aplikacemi nebo které jsou příliš velký a nevejde se na interní úložiště.

V minulosti mluvy externí úložiště uvedené diskový oddíl na vyměnitelné médium, jako jsou SD karty (označovanou také jako _přenosném úložném_). Již toto rozlišení není tak důležitý jako zařízení s Androidem se vyvinula a velký počet zařízení s Androidem už nebude podporovat vyměnitelného úložiště. Některá zařízení některé z jejich interních stálé paměti se místo toho přidělí které Android provádět stejné funkce vyměnitelné médium. To se označuje jako _emulované_ úložiště a je stále považovány za externí úložiště. Některá zařízení s Androidem můžete také může mít několik oddílů externího úložiště. Například aplikace pro Android tablet (navíc k jeho vnitřního úložiště) může mít emulován úložiště a jeden nebo více slotů pro SD karty. Android všechny tyto oddíly jsou považovány za externí úložiště.

Na zařízeních, které mají více uživatelů každý uživatel bude mít vyhrazené adresáře na oddíl primární externího úložiště pro jejich externího úložiště. Aplikace, které běží jako jeden uživatel nebude mít přístup k souborům z jiného uživatele na zařízení. Soubory pro všechny uživatele jsou stále světě – umožňovat čtení a zápis světě –; ale Android se izolovaný prostor všechny uživatelské profily od ostatních.

Čtení a zápis do souborů je téměř shodné v Xamarin.Android, jako je na jakékoli jiné aplikace .NET. Aplikace Xamarin.Android Určuje cestu k souboru, který bude manipulovat, pak používá standardní .NET idiomy pro přístup k souborům. Protože skutečné cesty k interní a externí úložiště se můžou lišit od zařízení nebo z verze Androidu na verzi Androidu, nedoporučuje se pevný kód cestu k souborům. Místo toho Xamarin.Android zpřístupňuje nativních rozhraní Android API, která vám pomůže s určení cesty k souborům na interní a externí úložiště.

Tato příručka popisuje koncepty a rozhraní API v Androidu, která jsou specifická pro externí úložiště.

## <a name="public-and-private-files-on-external-storage"></a>Soubory veřejného a privátního v externích úložištích

Existují dva různé typy souborů, které aplikace si může udržovat přehled o externí úložiště:

* **Privátní** soubory &ndash; soukromé soubory jsou soubory, které jsou specifické pro vaše aplikace (ale jsou stále world čitelný a world zapisovatelný). Android očekává, že soukromé soubory jsou uložená v konkrétním adresáři v externích úložištích. I když soubory se nazývají "privátní", jsou stále viditelné a přístupné pro jiné aplikace na zařízení, že nejsou žádné zvláštní ochranu poskytované Android.

* **Veřejné** soubory &ndash; jsou to soubory, se nepovažují za specifické pro aplikace a jsou určené k volně sdílet.

Je primárně koncepční rozdíly mezi tyto soubory. Soukromé soubory jsou privátní v tom smyslu, který je součástí aplikace, zatímco veřejné soubory jsou všechny soubory, které existují v externích úložištích, jsou považovány za. Android poskytuje dvou různých rozhraní API pro překlad cesty k souborům privátní a veřejné, ale jinak jsou stejná rozhraní API .NET použít ke čtení a zápis do těchto souborů. Toto jsou stejná rozhraní API, které jsou popsány v části na [čtení a zápis](~/android/platform/files/index.md#reading-or-writing-to-files-on-internal-storage).

### <a name="private-external-files"></a>Privátní externích souborů

Privátní externích souborů se považují za specifické pro aplikaci (podobně jako interní soubory), ale jsou uloženy v externích úložištích, z nejrůznějších důvodů (například je příliš velký pro interní úložiště). Podobně jako u interních souborů se tyto soubory se odstraní při odinstalaci aplikace uživatelem.

Primární umístění pro externí soubory soukromých nenajde voláním metody `Android.Content.Context.GetExternalFilesDir(string type)`. Tato metoda vrátí `Java.IO.File` objekt, který představuje privátní externího úložiště v adresáři aplikace. Předání `null` této metodě, vrátit cestu k adresáři úložiště uživatele pro aplikaci. Jako příklad pro aplikaci s názvem balíčku `com.companyname.app`, "root" adresář soukromé soubory externí bude:

```bash
/storage/emulated/0/Android/data/com.companyname.app/files/
```

Tento dokument bude odkazovat na adresář úložiště pro soubory soukromých v externích úložištích, jako _PRIVÁTNÍ\_externí\_úložiště_.


Parametr `GetExternalFilesDir()` je řetězec, který určuje _adresáře aplikace_. Toto je adresář poskytovat standardní umístění pro logické uspořádání souborů. Jsou k dispozici prostřednictvím konstanty na řetězcové hodnoty `Android.OS.Environment` třídy:

| `Android.OS.Environment` | Adresář |
|-|-|
| DirectoryAlarms | **_PRIVÁTNÍ\_externí\_úložiště_  /alarmů** |
| DirectoryDcim | **_PRIVÁTNÍ\_EXTERNÍ\_ÚLOŽIŠTĚ_/DCIM** |
| DirectoryDownloads | **_PRIVÁTNÍ\_externí\_úložiště_  /stažení** |
| DirectoryDocuments | **_PRIVÁTNÍ\_externí\_úložiště_  /dokumenty** |
| DirectoryMovies | **_PRIVÁTNÍ\_externí\_úložiště_/Movies** |
| DirectoryMusic | **_PRIVÁTNÍ\_externí\_úložiště_/Music** |
| DirectoryNotifications | **_PRIVÁTNÍ\_externí\_úložiště_/Notifications** |
| DirectoryPodcasts | **_PRIVÁTNÍ\_externí\_úložiště_/Podcasts** |
| DirectoryRingtones | **_PRIVÁTNÍ\_externí\_úložiště_/Ringtones** |
| DirectoryPictures | **_PRIVÁTNÍ\_externí\_úložiště_  /obrazky** |

Pro zařízení, která mají více oddílů externího úložiště bude mít každý oddíl adresáře, který je určený pro soukromé soubory. Metoda `Android.Content.Context.GetExternalFilesDirs(string type)` vrátí pole `Java.IO.Files`. Každý objekt bude představovat privátní directory specifické pro aplikaci na všech zařízeních sdílené/externí úložiště, kde aplikace může umístit soubory vlastní.

> [!IMPORTANT]
> Přesnou cestu k adresáři exteral privátní úložiště se může lišit od zařízení na zařízení a mezi verzemi androidu. Z tohoto důvodu aplikace musí není pevně kódu cestu k tomuto adresáři a místo toho použít rozhraní API pro Xamarin.Android, například `Android.Content.Context.GetExternalFilesDir()`.

### <a name="public-external-files"></a>Veřejné externí soubory

Veřejné soubory jsou soubory, které existují v externím úložišti, které nejsou uložené v adresáři, který přiděluje Android pro soukromé soubory. Veřejné soubory nebudou odstraněny při odinstalaci aplikace. Aplikace pro Android musí být udělena oprávnění předtím, než může číst nebo zapisovat všechny veřejné soubory. Je možné pro veřejné soubory existují kdekoli v externích úložištích, ale podle konvence Android očekává, že veřejné soubory existují v adresáři podle vlastnosti `Android.OS.Environment.ExternalStorageDirectory`. Tato vlastnost vrátí `Java.IO.File` objekt, který reprezentuje adresáři primární externího úložiště. Jako příklad `Android.OS.Environment.ExternalStorageDirectory` mohou odkazovat na následující adresář:

```bash
/storage/emulated/0/
```

Tento dokument bude odkazovat na adresář úložiště pro veřejné soubory v externích úložištích, jako _veřejné\_externí\_úložiště_.


Android také podporuje koncept aplikace adresáře na _veřejné\_externí\_úložiště_. Tyto adresáře se přesně shodují s diretories aplikace pro `_PRIVATE\_EXTERNAL\_STORAGE_` a jsou popsané v tabulce v předchozí části. Metoda `Android.OS.Environment.GetExternalStoragePublicDirectory(string directoryType)` vrátí `Java.IO.File` objektu, která odpovídají adresáři veřejné aplikace. `directoryType` Parametr je povinný parametr, nemůže být `null`.

Například volání `Environment.GetExternalStoragePublicDirectory(Environment.DirectoryDocuments).AbsolutePath` vrátí řetězec, který bude vypadat podobně jako:

```bash
/storage/emulated/0/Documents
```

> [!IMPORTANT]
> Přesnou cestu k adresáři veřejného externího úložiště se může lišit od zařízení na zařízení a mezi verzemi systému Android. Z tohoto důvodu aplikace musí není pevně kódu cestu k tomuto adresáři a místo toho použít rozhraní API pro Xamarin.Android, například `Android.OS.Environment.ExternalStorageDirectory`.

## <a name="working-with-external-storage"></a>Práce s externí úložiště

Jakmile aplikace Xamarin.Android získala úplná cesta k souboru, by měla využívat některé standardní rozhraní .NET API pro vytváření, čtení, zápisu a odstraňování souborů. Tím se maximalizuje množství pro různé platformy kompatibilní kód pro aplikace. Než se pokusíte o přístup k souboru musí však aplikace Xamarin.Android Ujistěte se, že je možné pro přístup k souboru.

1. **Ověření externího úložiště** &ndash; v závislosti na povaze externího úložiště, je možné, že nemusí být připojený a použitelný pro aplikaci. Všechny aplikace by měl zkontrolovat stav externího úložiště než se pokusíte použít.
2. **Provést kontrolu oprávnění modulu runtime** &ndash; aplikací pro Android musí požádat o oprávnění uživatele pro přístup k externímu úložišti. To znamená, že běhu by měl provést žádost o oprávnění, před všechny přístup k souborům. V průvodci [oprávnění v Xamarin.Android](~/android/app-fundamentals/permissions.md) obsahuje další podrobnosti o oprávnění Androidu.

Každá z těchto dvou úloh budou popsány níže.

### <a name="verifying-that-external-storage-is-available"></a>Ověřuje se, že je k dispozici externí úložiště

První krok před zápisem do externího úložiště se má zkontrolovat, že je z něj číst nebo zapisovat. `Android.OS.Environment.ExternalStorageState` Vlastnost obsahuje řetězec, který určuje stav externího úložiště. Tato vlastnost vrátí řetězec, který představuje stav. V této tabulce je seznam `ExternalStorageState` hodnoty, které mohou být vráceny `Environment.ExternalStorageState`:

| ExternalStorageState | Popis  |
|----------------------|---|
| MediaBadRemoval      | Médium se náhle odebral bez správně právě odpojován. |
| MediaChecking        | Médium je k dispozici, ale tohoto disku zaškrtněte.  |
| MediaEjecting        | Médium je právě probíhá odpojit a vysunut.  |
| MediaMounted         | Médium je připojené a mohou být čtena nebo zapisují do.  |
| MediaMountedReadOnly | Médium je připojený, ale lze číst pouze z. |
| MediaNofs            | Médium je k dispozici, ale neobsahuje vhodný pro Android a systém souborů. |
| MediaRemoved         | Není žádné médium, které jsou k dispozici. |
| MediaShared          | Médium je k dispozici, ale není připojený. Je právě sdílené úložiště přes USB s jiným zařízením.|
| MediaUnknown         | Stav média je neznámá Android. |
| MediaUnmountable     | Médium je k dispozici, ale nemůže být připojeno Android. |
| MediaUnmounted       | Médium je k dispozici, ale není připojený. |


Většina aplikací pro Android bude muset zkontrolovat, pokud je připojit externí úložiště. Následující fragment kódu ukazuje, jak ověřit, že je připojený externího úložiště pro přístup jen pro čtení nebo přístup pro čtení i zápis:

```csharp
bool isReadonly = Environment.MediaMountedReadOnly.Equals(Environment.ExternalStorageState);
bool isWriteable = Environment.MediaMounted.Equals(Environment.ExternalStorageState);
```

## <a name="external-storage-permissions"></a>Externí úložiště oprávnění

Android bude považovat za přístup k externí úložiště, aby byl _nebezpečných oprávnění_, což obvykle vyžaduje, aby uživatel k udělení oprávnění pro přístup k prostředku. Uživatel může tato oprávnění kdykoli odvolat.  To znamená, že běhu by měl provést žádost o oprávnění, před všechny přístup k souborům. Aplikace je automaticky udělena oprávnění číst a zapisovat vlastní privátní soubory. Je možné pro aplikace pro čtení a zápis soukromé soubory, které patří do jiných aplikací po se [oprávnění](~/android/app-fundamentals/permissions.md) uživatelem.

Všechny aplikace pro Android musí deklarovat dvě oprávnění pro externí úložiště **AndroidManifest.xml** . K identifikaci oprávnění, jeden z následujících dvou `uses-permission` prvky musíte přidat do **AndroidManifest.xml**:

```xml
<uses-permission android:name="android.permission.READ_EXTERNAL_STORAGE" />
<uses-permission android:name="android.permission.WRITE_EXTERNAL_STORAGE" />
```

> [!NOTE]
> Pokud uživatel udělí `WRITE_EXTERNAL_STORAGE`, pak `READ_EXTERNAL_STORAGE` je také implicitně udělen. Není nutné požádat o obě oprávnění v **AndroidManifest.xml**.

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Oprávnění se dá přidat i pomocí **Manifest v Androidu** karty **vlastnosti řešení**:

![Průzkumník řešení – požadovaná oprávnění pro Visual Studio 2017](./images/required-permissions.w157.png)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

Oprávnění se dá přidat i pomocí **Manifest v Androidu** karty **oblasti Vlastnosti řešení**:

[![Oblasti řešení – požadovaná oprávnění pro Visual Studio pro Mac](./images/required-permissions.m752-sml.png)](./images/required-permissions.m752.png#lightbox)

-----

Obecně řečeno musí schválit všechny nebezpečných oprávnění uživatele. Oprávnění pro externí úložiště jsou anomálie, nejsou výjimkou tohoto pravidla, v závislosti na verzi Androidu, na kterém běží aplikace:

![Vývojový diagram kontroly oprávnění externího úložiště](./images/external-permission-check-flowchart.png)

Další informace o provedení žádosti o oprávnění modulu runtime, naleznete v příručce [oprávnění v Xamarin.Android](~/android/app-fundamentals/permissions.md). **Monodroidu sample** [Místní_soubory](https://github.com/xamarin/monodroid-samples/tree/master/LocalFiles) také ukazuje jeden způsob, jak provádění kontroly oprávnění za běhu.

#### <a name="granting-and-revoking-permissions-with-adb"></a>Poskytování a odvolání oprávnění s ADB

Při vývoji aplikací pro Android, může být potřeba udělit dokumentů a odvolání přístupu oprávnění k testování různých pracovních postupů spojené s kontroly oprávnění za běhu. Je možné provést na příkazovém řádku pomocí ADB. Následující příkazový řádek fragmenty kódu ukazují, jak udělit nebo odvolat oprávnění pro aplikace pro Android, název balíčku je pomocí ADB **com.companyname.app**:

```bash
$ adb shell pm grant com.companyname.app android.permission.WRITE_EXTERNAL_STORAGE

$ adb shell pm revoke com.companyname.app android.permission.WRITE_EXTERNAL_STORAGE
```

## <a name="deleting-files"></a>Odstraňování souborů

Všechny standardní rozhraní API jazyka C# je možné odstranit soubor z externího úložiště, jako například [ `System.IO.File.Delete` ](xref:System.IO.File.Delete*). Je také možné použít rozhraní Java API za cenu přenositelnost kódu. Příklad:

```csharp
System.IO.File.Delete("/storage/emulated/0/Android/data/com.companyname.app/files/count.txt");
```

## <a name="related-links"></a>Související odkazy

* [Místní soubory Xamarin.Android ukázku **monodroidu – ukázky**](https://github.com/xamarin/monodroid-samples/tree/master/LocalFiles)
* [Oprávnění v Xamarin.Android](~/android/app-fundamentals/permissions.md)
