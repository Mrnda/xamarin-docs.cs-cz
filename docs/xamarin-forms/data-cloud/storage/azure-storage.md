---
title: Ukládání a přístup k datům v úložišti Azure
description: Azure Storage je řešení škálovatelné cloudové úložiště, které lze použít pro ukládání nestrukturovaných a strukturovaných dat. Tento článek ukazuje, jak používat Xamarin.Forms k uložení textové a binární data ve službě Azure Storage a jak získat přístup k datům.
ms.prod: xamarin
ms.assetid: 5B10D37B-839B-4CD0-9C65-91014A93F3EB
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 06/16/2017
ms.openlocfilehash: 63afeec81eff350b034e8dd3a13da52801937826
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/04/2018
ms.locfileid: "30789869"
---
# <a name="storing-and-accessing-data-in-azure-storage"></a>Ukládání a přístup k datům v úložišti Azure

_Azure Storage je řešení škálovatelné cloudové úložiště, které lze použít pro ukládání nestrukturovaných a strukturovaných dat. Tento článek ukazuje, jak používat Xamarin.Forms k uložení textové a binární data ve službě Azure Storage a jak získat přístup k datům._

## <a name="overview"></a>Přehled

Úložiště Azure poskytuje čtyři služby úložiště:

- Úložiště objektů BLOB. Objekt blob může být text nebo binárních dat, například zálohování, virtuální počítače, soubory médií nebo dokumenty.
- Úložiště Table je úložiště klíč atribut typu NoSQL.
- Queue Storage je službou zasílání zpráv pro zpracování pracovního postupu a komunikaci mezi cloudové služby.
- File Storage nabízí sdílené úložiště pomocí protokolu SMB.

Existují dva typy účtů úložiště:

- Účty úložiště pro obecné účely poskytuje přístup ke službám Azure Storage z jednoho účtu.
- Účet úložiště Blob je specializovaný účet úložiště pro ukládání objektů BLOB. Tento typ účtu se doporučuje, když potřebujete ukládání dat objektů blob.

V tomto článku a doplňujícími ukázkovou aplikaci, ukazuje odesílání bitové kopie a textové soubory do úložiště objektů blob a jejich stahování. Kromě toho také ukazuje načítání seznamu souborů z úložiště objektů blob a odstraňování souborů.

Další informace o službě Azure Storage najdete v tématu [Úvod do Storage](https://azure.microsoft.com/documentation/articles/storage-introduction/).

## <a name="introduction-to-blob-storage"></a>Úvod do úložiště objektů Blob

Úložiště objektů blob se skládá ze tří součástí, které jsou uvedeny v následujícím diagramu:

![](azure-storage-images/blob-storage.png "Koncepty úložiště objektů BLOB")

Veškerý přístup do služby Azure Storage je prostřednictvím účtu úložiště. Účet úložiště může obsahovat neomezený počet kontejnerů a kontejner můžete pojmout neomezený počet objektů BLOB až do limitu kapacity účtu úložiště.

Objekt blob je soubor libovolného typu a velikosti. Úložiště Azure podporuje tři typy jiný objektu blob:

- Objekty BLOB bloku jsou optimalizované pro streamování a ukládání cloudových objektů a jsou dobrou volbou pro ukládání záloh, souborů médií, dokumentů atd. Objekty BLOB bloku může být až do velikosti 195Gb.
- Append – objekty BLOB jsou podobné objektům BLOB bloku, ale jsou optimalizované pro doplňovací operace, například protokolování. Append – objekty BLOB může být až do velikosti 195Gb.
- Objekty BLOB stránky jsou optimalizované pro operace čtení a zápisu časté a jsou obvykle používány pro ukládání virtuálních počítačů a jejich disky. Objekty BLOB stránky může být velikost až 1Tb.

> [!NOTE]
> Všimněte si, že účty úložiště blob podporují bloku a připojit objekty BLOB, ale nepodporují objekty BLOB stránky.

Objekt blob je nahrané do úložiště Azure a stáhnout ze služby Azure Storage, pomocí datového proudu bajtů. Proto musí být soubory převeden do datového proudu bajtů před nahrávání a převeden zpět do jejich původního reprezentace po stažení.

Každý objekt, který je uložený ve službě Azure Storage má jedinečnou adresu URL. Název účtu úložiště tvoří subdoménu dané tuto adresu a kombinace formulářů název domény a subdomény *koncový bod* pro účet úložiště. Například pokud je název účtu úložiště *můj_účet_úložiště*, je výchozí koncový bod objektu blob pro účet úložiště `https://mystorageaccount.blob.core.windows.net`.

Adresa URL pro přístup k objektu v účtu úložiště je sestavena připojením umístění objektu v účtu úložiště ke koncovému bodu. Například adresu objektu blob bude mít formát `https://mystorageaccount.blob.core.windows.net/mycontainer/myblob`.

## <a name="setup"></a>Instalace

Proces pro integraci účtu Azure Storage na aplikaci Xamarin.Forms vypadá takto:

1. Vytvoření účtu úložiště. Další informace najdete v tématu [vytvořit účet úložiště](https://azure.microsoft.com/documentation/articles/storage-create-storage-account/#create-a-storage-account).
1. Přidat [Klientská knihovna pro úložiště Azure](https://www.nuget.org/packages/WindowsAzure.Storage/) aplikaci Xamarin.Forms.
1. Konfigurace připojovacího řetězce úložiště. Další informace najdete v tématu [připojování k úložišti Azure](#connecting).
1. Přidat `using` direktivy pro `Microsoft.WindowsAzure.Storage` a `Microsoft.WindowsAzure.Storage.Blob` obory názvů do třídy, které budou přistupovat k úložišti Azure.

> [!NOTE]
> Při této ukázce se používá sdílený přístup k projektu, Klientská knihovna pro úložiště Azure nyní podporuje také spotřebovávanou z projektu přenosných třída knihovny PCL ().

<a name="connecting" />

## <a name="connecting-to-azure-storage"></a>Připojování k úložišti Azure

Každý požadavek směřovaný na prostředky na účtu úložiště musí být ověřeny. Zatímco objekty BLOB může být nakonfigurované pro podporu anonymní ověřování, existují dva hlavní přístupy, které aplikace můžete použít k ověřování pro účet úložiště:

- Sdílený klíč. Tento postup používá Azure klíče účtu úložiště. název a účet pro přístup ke službě úložiště. Účet úložiště je přiřazen dva privátní klíče při vytváření, který lze použít pro ověření pomocí sdíleného klíče.
- Sdílený přístupový podpis. Toto je token, který může být připojí k adrese URL, který umožní Delegovaný přístup k prostředku úložiště, s oprávněními, určuje, pro dobu, po který je platný.

Lze zadat připojovací řetězce, které zahrnují ověření informace požadované pro přístup k prostředkům Azure Storage z aplikace. Kromě toho lze nakonfigurovat připojovací řetězec pro připojení k emulátoru úložiště Azure ze sady Visual Studio.

> [!NOTE]
> Úložiště Azure podporuje protokoly HTTP a HTTPS v připojovacím řetězci. Nicméně se doporučuje pomocí protokolu HTTPS.

### <a name="connecting-to-the-azure-storage-emulator"></a>Připojování k emulátoru úložiště Azure

Emulátor úložiště Azure poskytuje místní prostředí, které emuluje Azure blob, fronty a tabulky služby pro účely vývoje.

Následující připojovací řetězec by měl použít pro připojení k emulátoru úložiště Azure:

```csharp
UseDevelopmentStorage=true
```

Další informace o emulátoru úložiště Azure najdete v tématu [pomocí emulátoru úložiště Azure pro vývoj a testování](https://azure.microsoft.com/documentation/articles/storage-use-emulator/).

### <a name="connecting-to-azure-storage-using-a-shared-key"></a>Připojování k úložišti Azure pomocí sdíleného klíče

Následující formátu řetězce připojení se použít k připojení do služby Azure Storage se sdílený klíč:

```csharp
DefaultEndpointsProtocol=[http|https];AccountName=myAccountName;AccountKey=myAccountKey
```

`myAccountName` měl by být nahrazen název účtu úložiště a `myAccountKey` by měla být nahrazená s jedním ze dvou přístupových klíčů účtu.

> [!NOTE]
> Při používání sdíleného klíče ověřování, název účtu a klíč účtu budou distribuována pro každou osobu, vaše aplikace, které zajistí úplné pro čtení a zápis přístup k účtu úložiště. Proto použít pro testování pouze pro účely ověření pomocí sdíleného klíče a nikdy distribuci klíčů do jiných uživatelů.

### <a name="connecting-to-azure-storage-using-a-shared-access-signature"></a>Připojování k úložišti Azure pomocí sdíleného přístupového podpisu

Následující formátu řetězce připojení se použít k připojení k úložišti Azure s SAS:

`BlobEndpoint=myBlobEndpoint;SharedAccessSignature=mySharedAccessSignature`

`myBlobEndpoint` měl by být nahrazen adresu URL koncový bod služby blob a `mySharedAccessSignature` měl by být nahrazen vaší SAS. SAS poskytuje pověření pro přístup k prostředku, protokol a koncový bod služby.

> [!NOTE]
> Ověřování SAS se doporučuje pro výrobní aplikace. V případě produkční aplikace mají být načtena SAS z back-end služby na vyžádání, než je instalován s aplikace.

Další informace o sdílených přístupových podpisů najdete v tématu [pomocí sdíleného přístupového podpisy (SAS)](https://azure.microsoft.com/documentation/articles/storage-dotnet-shared-access-signature-part-1/).

## <a name="creating-a-container"></a>Vytvoření kontejneru

`GetContainer` Metoda se používá k načtení odkaz na kontejner s názvem, který je pak možné načíst objekty BLOB z kontejneru nebo přidat do kontejneru objektů BLOB. Následující příklad kódu ukazuje `GetContainer` metoda:

```csharp
static CloudBlobContainer GetContainer(ContainerType containerType)
{
  var account = CloudStorageAccount.Parse(Constants.StorageConnection);
  var client = account.CreateCloudBlobClient();
  return client.GetContainerReference(containerType.ToString().ToLower());
}
```

`CloudStorageAccount.Parse` Metoda analyzuje připojovací řetězec a vrátí `CloudStorageAccount` instanci, která představuje účet úložiště. A `CloudBlobClient` instanci, která se používá k načtení kontejnery a objekty BLOB, pak vytvoří `CreateCloudBlobClient` metoda. `GetContainerReference` Metoda načte zadaný kontejner jako `CloudBlobContainer` instance, než se vrátí k volání metody. V tomto příkladu je název kontejneru `ContainerType` hodnota výčtu, převést na řetězec malá písmena.

> [!NOTE]
> Názvy kontejnerů musí být psaný malými písmeny a musí začínat písmenem nebo číslicí. Kromě toho se může obsahovat pouze písmena, číslice a pomlčky znak a musí být v rozmezí 3 až 63 znaků.

`GetContainer` Metoda je volána následujícím způsobem:

```csharp
var container = GetContainer(containerType);
```

`CloudBlobContainer` Instance pak lze vytvořit kontejner, pokud ještě neexistuje:

```csharp
await container.CreateIfNotExistsAsync();
```

Ve výchozím nastavení je nově vytvořený kontejner soukromé. To znamená, že přístupový klíč k úložišti musí být zadán pro načtení objektů blob z kontejneru. Informace o tom, jak objektů BLOB v kontejneru veřejného najdete v tématu [vytvořit kontejner](https://azure.microsoft.com/documentation/articles/storage-dotnet-how-to-use-blobs/#create-a-container).

## <a name="uploading-data-to-a-container"></a>Nahrání dat do kontejneru

`UploadFileAsync` Metoda slouží k odesílání datového proudu bajtů dat do úložiště objektů blob a je znázorněno v následujícím příkladu kódu:

```csharp
public static async Task<string> UploadFileAsync(ContainerType containerType, Stream stream)
{
  var container = GetContainer(containerType);
  await container.CreateIfNotExistsAsync();

  var name = Guid.NewGuid().ToString();
  var fileBlob = container.GetBlockBlobReference(name);
  await fileBlob.UploadFromStreamAsync(stream);

  return name;
}
```

Po načtení odkaz na kontejner, metoda vytvoří kontejner, pokud ještě neexistuje. Nový `Guid` se pak vytvoří tak, aby fungoval jako objekt blob jedinečný název, a odkaz na objekt blob bloku se načítají jako `CloudBlockBlob` instance. Datový proud je poté odeslány do objektu blob pomocí `UploadFromStreamAsync` metodu, která vytvoří objekt blob, pokud ještě neexistuje, nebo ho přepíše, pokud neexistuje.

Předtím, než soubor je možné uložit do objektu blob úložiště pomocí této metody, musí nejprve převeden na datový proud bajtů. Tento postup je znázorněn v následujícím příkladu kódu:

```csharp
var byteData = Encoding.UTF8.GetBytes(text);
uploadedFilename = await AzureStorage.UploadFileAsync(ContainerType.Text, new MemoryStream(byteData));
```

`text` Data se převedou na bajtové pole, které je vnořena jako datový proud, který je předán `UploadFileAsync` metoda.

## <a name="downloading-data-from-a-container"></a>Stahování dat z kontejneru

`GetFileAsync` Metoda se používá ke stahování dat objektů blob z úložiště Azure a je znázorněno v následujícím příkladu kódu:

```csharp
public static async Task<byte[]> GetFileAsync(ContainerType containerType, string name)
{
  var container = GetContainer(containerType);

  var blob = container.GetBlobReference(name);
  if (await blob.ExistsAsync())
  {
    await blob.FetchAttributesAsync();
    byte[] blobBytes = new byte[blob.Properties.Length];

    await blob.DownloadToByteArrayAsync(blobBytes, 0);
    return blobBytes;
  }
  return null;
}
```

Po načtení odkaz na kontejner, načte metodu odkaz na objekt blob pro uložená data. Pokud existuje objekt blob, jeho vlastnosti jsou načteny `FetchAttributesAsync` metoda. Bajtové pole správnou velikost je vytvořen a stáhnou se objektu blob jako pole bajtů, které získá vrátil volání metody.

Po stažení jsou data objektu blob bajtů, musíte převést na jeho původní reprezentace. Tento postup je znázorněn v následujícím příkladu kódu:

```csharp
var byteData = await AzureStorage.GetFileAsync(ContainerType.Text, uploadedFilename);
string text = Encoding.UTF8.GetString(byteData);
```

Pole bajtů se načítají ze služby Azure Storage pomocí `GetFileAsync` řetězec kódovaný metoda, než je převést zpět na UTF8.

## <a name="listing-data-in-a-container"></a>Výpis Data v kontejneru

`GetFilesListAsync` Metoda se používá k načtení seznamu uložen v kontejneru objektů BLOB a je znázorněno v následujícím příkladu kódu:

```csharp
public static async Task<IList<string>> GetFilesListAsync(ContainerType containerType)
{
  var container = GetContainer(containerType);

  var allBlobsList = new List<string>();
  BlobContinuationToken token = null;

  do
  {
    var result = await container.ListBlobsSegmentedAsync(token);
    if (result.Results.Count() > 0)
    {
      var blobs = result.Results.Cast<CloudBlockBlob>().Select(b => b.Name);
      allBlobsList.AddRange(blobs);
    }
    token = result.ContinuationToken;
  } while (token != null);

  return allBlobsList;
}
```

Po načtení odkaz na kontejner, používá metodu kontejneru `ListBlobsSegmentedAsync` metoda pro načtení odkazů na objekty BLOB v kontejneru. Výsledky vrácené `ListBlobsSegmentedAsync` jsou uvedené metody při `BlobContinuationToken` instance není `null`. Každý objekt blob je přetypování z vráceného `IListBlobItem` k `CloudBlockBlob` v pořadí přístup `Name` vlastnost objektu blob, než je hodnota se přidá do `allBlobsList` kolekce. Jednou `BlobContinuationToken` instance je `null`, byla vrácena poslední název objektu blob a provádění ukončení smyčky.

## <a name="deleting-data-from-a-container"></a>Odstranění dat z kontejneru

`DeleteFileAsync` Metoda se používá k odstranění objektu blob z kontejneru a je znázorněno v následujícím příkladu kódu:

```csharp
public static async Task<bool> DeleteFileAsync(ContainerType containerType, string name)
{
  var container = GetContainer(containerType);
  var blob = container.GetBlobReference(name);
  return await blob.DeleteIfExistsAsync();
}
```

Po načtení odkaz na kontejner, metoda získá odkaz na objekt blob pro zadaný objekt blob. Objekt blob se odstraní pomocí `DeleteIfExistsAsync` metoda.

## <a name="summary"></a>Souhrn

Tento článek ukázal, jak používat Xamarin.Forms k uložení textové a binární data ve službě Azure Storage a jak získat přístup k datům. Azure Storage je řešení škálovatelné cloudové úložiště, které lze použít pro ukládání nestrukturovaných a strukturovaných dat.


## <a name="related-links"></a>Související odkazy

- [Úložiště Azure (ukázka)](https://developer.xamarin.com/samples/xamarin-forms/WebServices/AzureStorage/)
- [Úvod do Storage](https://azure.microsoft.com/documentation/articles/storage-introduction/)
- [Používání úložiště Blob z Xamarin](https://azure.microsoft.com/documentation/articles/storage-xamarin-blob-storage/)
- [Použití sdílených přístupových podpisů (SAS)](https://azure.microsoft.com/documentation/articles/storage-dotnet-shared-access-signature-part-1/)
- [Windows Azure Storage](https://www.nuget.org/packages/WindowsAzure.Storage/)
