---
title: 'Hello, Android Multiobrazovka: Podrobné informace'
description: V této příručce dvě části je základní aplikace Phoneword (vytvořený ve Hello, Android průvodce) rozšířit pro zpracování druhý obrazovky. Cestou se vydávají základních stavebních bloků Android aplikace. O podrobnější prohlídku do Android architektury je součástí můžete vyvinout lépe porozumět struktura aplikace pro Android a funkce.
ms.topic: quickstart
ms.prod: xamarin
ms.assetid: E4150036-7760-4023-BD33-B7BDE7B7AF5B
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/15/2018
ms.openlocfilehash: f1c19d43aa1f9010307df3fb954ac1029221ccd4
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/04/2018
---
# <a name="hello-android-multiscreen-deep-dive"></a>Hello, Android Multiobrazovka: Podrobné informace

_V této příručce dvě části je základní aplikace Phoneword (vytvořený ve Hello, Android průvodce) rozšířit pro zpracování druhý obrazovky. Cestou se vydávají základních stavebních bloků Android aplikace. O podrobnější prohlídku do Android architektury je součástí můžete vyvinout lépe porozumět struktura aplikace pro Android a funkce._

## <a name="hello-android-multiscreen-deep-dive"></a>Hello, Android Multiobrazovka podrobné informace

V [Hello, Android Multiobrazovka rychlý Start](~/android/get-started/hello-android-multiscreen/hello-android-multiscreen-quickstart.md), vytvořené a spustil vaší první aplikace Xamarin.Android více obrazovky.
Nyní je čas k vývoji lépe pochopili, Android navigaci a architektura tak, aby mohou vytvářet složitější aplikace.

V této příručce zaměříte pokročilejší Android architektura jako Android *aplikace stavební bloky* vydávají. Android navigace pomocí *záměry* je vysvětleno a Android hardwarové navigační jsou prozkoumali možnosti. Nově přidané do aplikace Phoneword jsou odebrán, když budete vyvíjet více ucelený přehled o aplikace vztah s operačním systémem a dalších aplikací.


## <a name="android-architecture-basics"></a>Základy Android architektura

V [Hello, Android podrobné informace](~/android/get-started/hello-android/hello-android-deepdive.md), jste se dozvěděli, že aplikace pro Android jsou jedinečné programy tím, že neobsahují jeden vstupní bod. Místo toho operačního systému (nebo jiná aplikace) spustí některého z registrovaných aktivity aplikace, který pak zahájí proces pro danou aplikaci. Toto podrobné informace do Android architektury rozšíří vaše pochopení jak Android aplikací, které se vytvářejí zavedením Android stavební bloky aplikací a jejich funkce.


### <a name="android-application-blocks"></a>Bloky aplikace pro Android

Aplikace pro Android se skládá z kolekce speciální třídy Android názvem *aplikace bloky* dodávat společně s libovolný počet prostředky aplikace - bitové kopie, motivy, pomocné třídy atd &ndash; tyto jsou koordinuje volá se soubor XML *Android Manifest*.

Bloky aplikace formulářů páteřní aplikací pro Android, protože umožňují provádět akce, které nelze provést normálně s běžné třídy. Nejdůležitější ty, které jsou dva _aktivity_ a _služby_:

-   **Aktivita** &ndash; aktivity odpovídá na obrazovku s uživatelským rozhraním a se koncepčně podobá na webové stránce ve webové aplikaci. V informačním kanálu aplikaci, přihlašovací obrazovku bude první aktivitu, například posuvný seznam položek zprávy by jinou aktivitu a stránce podrobností pro každou položku by třetí. Další informace o činnosti v [životního cyklu aktivity](~/android/app-fundamentals/activity-lifecycle/index.md) průvodce.

-   **Služba** &ndash; Android služby podporovat aktivity převzetí dlouhotrvající úlohy a je spuštěná na pozadí. Služby není k dispozici uživatelské rozhraní a slouží ke zpracování úloh, které nejsou vázané na obrazovky &ndash; například přehrávat skladbu na pozadí nebo nahrávání fotografie na server. Další informace o službách najdete v tématu [vytváření služeb](~/android/app-fundamentals/services/index.md) a [Android služby](~/android/app-fundamentals/services/index.md) příručky.


Aplikace platformy Android nesmíte používat všechny typy bloků a často má několik bloků jednoho typu. Například aplikace Phoneword z [Hello, Android rychlý Start](~/android/get-started/hello-android/hello-android-quickstart.md) se skládá právě jednu aktivitu (obrazovky) a některých souborů prostředků. Aplikace na jednoduchý hudební přehrávač může mít několik aktivit a služby pro přehrávání hudby, když je aplikace na pozadí.

### <a name="intents"></a>Záměry

Jiné základní koncept v aplikacích Android je *záměr*.
Android je uspořádaná kolem *Princip nejnižších nutných oprávnění* &ndash; aplikací mít přístup jenom na bloky potřebují k práci, a budou mít omezený přístup na bloky, které tvoří operačního systému nebo jiné aplikace. Podobně, jako jsou bloky *volně vázány* &ndash; jsou navrženy tak, aby měl málo znalosti a omezený přístup k jiné bloky (i bloky, které jsou součástí stejné aplikace).

Pro komunikaci, bloky aplikace odesílat asynchronní zprávy názvem *záměry* a zpět. Záměry obsahují informace o přijímající bloku a někdy některá data. Záměrem odeslaný jeden triggery pro součást aplikace určitou akci v jiné aplikaci součásti vazby dvě součásti aplikace a umožní, aby mohla komunikovat. Odesílání tříd Intent a zpět, lze získat bloky ke koordinaci komplexní akce, jako je spuštění aplikace fotoaparát trvat a uložte, shromažďování informací o umístění nebo navigace z jedné obrazovky na další.


### <a name="androidmanifestxml"></a>AndroidManifest.XML

Když přidáte blok do aplikace, není zaregistrována speciální soubor XML s názvem **Android Manifest**. Manifest uchovává informace o všech bloků aplikace v aplikaci, jakož i požadavky na verzi, oprávnění a propojené knihovny &ndash; všechno, co se operační systém musí znát pro spuštění aplikace. **Android Manifest** funguje taky s aktivity a záměry k řízení, jaké akce jsou vhodné pro danou aktivitu. Tyto funkce systému Android Manifest jsou popsané v [práce s Android Manifest](~/android/platform/android-manifest.md) průvodce.

Ve verzi jedním obrazovky aplikace Phoneword, jenom jedna aktivita, jeden záměr a `AndroidManifest.xml` byly použity, spolu s další prostředky jako ikony. Ve verzi Phoneword více obrazovky byla přidána další aktivitu; ho byl spuštěn z první aktivitu pomocí záměrem. Následující části jsou zde popsány jak záměry pomoci vytvořit navigační v aplikacích Android.

## <a name="android-navigation"></a>Android navigace

Záměry používaly umožňují přecházet mezi jednotlivými obrazovky. Je čas můžete začít vytvářet tento kód na tom, jak záměry fungovat a pochopení jejich role v Android navigační.


### <a name="launching-a-second-activity-with-an-intent"></a>Spouští se druhá aktivita se záměrem

V aplikaci Phoneword se záměrem použitý ke spuštění druhého obrazovku (aktivitu). Začněte vytvořením záměrem, předávání v aktuální *kontextu* (`this`odkazující na aktuální **kontextu**) a typ bloku aplikace, který hledáte (`TranslationHistoryActivity`):

```csharp
Intent intent = new Intent(this, typeof(TranslationHistoryActivity));
```

**Kontextu** je rozhraní pro globální informace o prostředí aplikace &ndash; umožňuje nově vytvořené objekty vědět, co se děje s aplikací. Pokud si myslíte záměru jako zprávu, že zadáváte název příjemce zprávy (`TranslationHistoryActivity`) a adresa příjemce (`Context`).

Android nabízí možnost, chcete-li záměrem připojit jednoduché datové (komplexní se zpracují data jinak). V příkladu Phoneword `PutStringArrayExtra` se používá pro připojení k záměr seznam telefonních čísel a `StartActivity` nazývá na straně příjemce záměr. Dokončený kód v vypadá takto:

```csharp
translationHistoryButton.Click += (sender, e) =>
{
    var intent = new Intent(this, typeof(TranslationHistoryActivity));
    intent.PutStringArrayListExtra("phone_numbers", _phoneNumbers);
    StartActivity(intent);
};
```


## <a name="additional-concepts-introduced-in-phoneword"></a>Další koncepty představené v Phoneword

Aplikace Phoneword zavedl několik konceptů, které nejsou zahrnuté v této příručce. Tyto koncepty patří:

**Řetězce prostředků** &ndash; v Phoneword aplikace, text `TranslationHistoryButton` byla nastavena na `"@string/translationHistory"`. `@string` Syntaxe znamená, že řetězcová hodnota je uložena v _souboru prostředků řetězec_, **Strings.xml**. Následující hodnota `translationHistory` řetězec byla přidána do **Strings.xml**:

```xml
<?xml version="1.0" encoding="utf-8"?>
<resources>
    <string name="translationHistory">Call History</string>
</resources>
```

Další informace o řetězce a dalších prostředků Android, najdete v části [Průvodce Android prostředky](~/android/app-fundamentals/resources-in-android/index.md).

**ListView a ArrayAdapter** &ndash; A _ListView_ je součást uživatelského rozhraní, která poskytuje jednoduchý způsob, jak k dispozici posouvání seznam řádků. A `ListView` instance vyžaduje _adaptér_ ke kanálu s daty obsaženými v řádku zobrazení. Následující řádek kódu se používá k naplnění uživatelského rozhraní `TranslationHistoryActivity`:

```csharp
this.ListAdapter = new ArrayAdapter<string>(this, Android.Resource.Layout.SimpleListItem1, phoneNumbers);
```

ListViews a adaptéry jsou nad rámec tohoto dokumentu, ale jsou popsané v velmi komplexní [ListViews a adaptéry](~/android/user-interface/layouts/list-view/index.md) průvodce.
[Naplnění dat s ListView](~/android/user-interface/layouts/list-view/populating.md) se pomocí integrovaných `ListActivity` a `ArrayAdapter` třídy vytvořit a naplnit `ListView` bez definování vlastní rozložení, stejně jako v příkladu Phoneword provádělo.


## <a name="summary"></a>Souhrn

Blahopřejeme, jste dokončili svou první více obrazovku aplikace pro Android. Tato příručka zavedená *Android aplikace stavební bloky* a *záměry* a použít je k sestavení více monitorovaná aplikace pro Android. Nyní máte sice solidní základ, je nutné začít vyvíjet aplikace Xamarin.Android.

V dalším kroku získáte informace k vytváření aplikací pro různé platformy pomocí Xamarinu ve [vytváření aplikací a platformy provede](~/cross-platform/app-fundamentals/building-cross-platform-applications/index.md).
