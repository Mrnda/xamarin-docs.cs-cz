---
title: Úvod do knihovny přenosných tříd (PCL)
description: Tento článek představuje projekty Přenosná knihovna tříd (PCL) a provede procesem vytváření a využívání PCL projektů v sadě Visual Studio pro Mac a Visual Studio.
ms.prod: xamarin
ms.assetid: 76ba8f7a-9b6e-40f5-9a29-ff1274ece4f2
author: conceptdev
ms.author: crdun
ms.date: 07/18/2018
ms.openlocfilehash: 83b1da5cd10a46b8480b0755eeb16bf7434a5906
ms.sourcegitcommit: 46bb04016d3c35d91ff434b38474e0cb8197961b
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/26/2018
ms.locfileid: "39270086"
---
# <a name="portable-class-libraries-pcl"></a>Přenosné knihovny tříd (PCL)

> [!WARNING]
> Přenosné knihovny tříd (PCLs) se považují za zastaralé v nejnovějším verzím sady Visual Studio.
> Přestože stále můžete otevřít, upravit a zkompilovat PCLs, pro nové projekty se doporučuje použít [knihovny .NET Standard](~/cross-platform/app-fundamentals/net-standard.md).

Klíčovou součástí vytváření multiplatformních aplikací je schopnost sdílení kódu napříč různými projekty specifické pro platformu. To je ale složité fakt, že různé platformy často používají různé dílčí sady z .NET základní třídy knihovny (BCL) a proto jsou ve skutečnosti integrovány do jiného profilu knihovny .NET Core. To znamená, že jednotlivé platformy, můžete použít pouze knihovny tříd, které jsou cíleny na stejný profil, takže se bude zobrazovat tak, aby vyžadovala projekty knihovny samostatné třídy pro každou platformu.

Existují tři hlavní přístupy k sdílení kódu, které řeší tento problém: **projekty .NET Standard teď**, **sdílené projekty Asset**, a **Přenosná knihovna tříd (PCL) projekty**.

- **Projekty .NET standard teď** jsou oblíbený přístup ke sdílení kódu .NET, přečtěte si další informace o [projekty .NET Standard a Xamarin](~/cross-platform/app-fundamentals/net-standard.md).
- **Sdílené projekty Asset** použijte jednu sadu souborů a nabízí rychlý a jednoduchý způsob, ve kterém chcete sdílení kódu v rámci řešení a obvykle řídí direktivami podmíněné kompilace určování cest kódu pro různé platformy, které bude používat (Další informace informace najdete v tématu [sdílené projekty článku](~/cross-platform/app-fundamentals/shared-projects.md)).
- **PCL** projekty jsou určené pro konkrétní profily, které podporují známou sadu BCL třídy nebo funkce. Dolů na straně pro PCL je však, že často vyžadují další architektury úsilí k oddělení profil konkrétního kódu do své vlastní knihovny.

Tato stránka vysvětluje, jak vytvořit **PCL** projekt, který cílí na konkrétní profil, který může poté odkazovat více projektů pro konkrétní platformu.

## <a name="what-is-a-portable-class-library"></a>Co je přenosné knihovny tříd?

Při vytváření projektu aplikace nebo projekt knihovny, je omezen na práci na konkrétní platformu, pro kterou se vytvoří pro výslednou knihovnu DLL. Tím zabráníte psaní sestavení aplikace pro Windows a poté ho znovu použít pro Xamarin.iOS a Xamarin.Android.

Při vytváření přenosné knihovny tříd však můžete kombinací platforem, které chcete spustit kód. Kompatibilita volby provedené při vytváření knihovny přenosných tříd jsou přeloženy do identifikátor "Profil", který popisuje, které platformy podporuje knihovny.

Následující tabulka ukazuje některé funkce, které se liší podle platformy .NET. Zápis PCL sestavení, která se zaručeně spustí na konkrétní zařízení a platformy můžete jednoduše vyberte, které podporují je povinný při vytváření projektu.

|Funkce|.NET Framework|U aplikací pro UPW|Silverlight|Windows Phone|Xamarin|
|---|---|---|---|---|---|
|Jádro|A|A|A|A|A|
|LINQ|A|A|A|A|A|
|IQueryable|A|A|A|7.5 +|A|
|Serializace|A|A|A|A|A|
|Datové poznámky|4.0.3 +|A|A||A|

Xamarin sloupce odráží fakt, že Xamarin.iOS a Xamarin.Android podporuje všechny profily, které jsou součástí sady Visual Studio a dostupnost funkcí ve všech knihoven, které vytvoříte bude omezena pouze jiné platformy, kterou chcete podporovat.

To zahrnuje profily, které jsou kombinací:

- Rozhraní .NET 4 nebo .NET 4.5
- Silverlight 5
- Windows Phone 8
- U aplikací pro UPW

Si můžete přečíst více o funkcích různé profily na [webu společnosti Microsoft](http://msdn.microsoft.com/library/gg597391(v=vs.110).aspx) uvidíme jiný člen komunity [profilem PCL souhrnu](http://embed.plnkr.co/03ck2dCtnJogBKHJ9EjY) obsahující nepodporuje informace o rozhraní framework a další poznámky.

**Výhody**

1. Sdílení centralizované kódu – psaní a testování kódu v jednom projektu, které mohou být spotřebovány jiných knihovnách nebo aplikacích.
2. Operace refaktoringu ovlivní všechny kódu načteného v řešení (přenosné knihovny tříd a projekty specifické pro platformu).
3. Projekt PCL dalo snadno odkazovat pomocí jiných projektů v řešení nebo výstupní sestavení je možné sdílet i ostatní odkazovat ve svých řešeních.

**Nevýhody**

1. Protože stejné knihovny přenosných tříd je sdílen mezi více aplikacemi, knihovny pro konkrétní platformu se nedá odkazovat (např.) Community.CsharpSqlite.WP7).
2. Třídy, které by jinak byly k dispozici v MonoTouch a Mono for Android (například DllImport nebo System.IO.File) nesmí obsahovat dílčí přenosné knihovny tříd.

> [!NOTE]
> Knihovny přenosných tříd se již nepoužívají v nejnovější verzi sady Visual Studio, a [knihovny .NET Standard](net-standard.md) doporučujeme místo toho.

Do určité míry mohou být i nevýhody požadavky pomocí vzoru poskytovatele nebo vkládání závislostí kódu se skutečná implementace v projektech platformy pro rozhraní nebo základní třídy, která je definována v přenosné knihovně tříd.

Tento diagram znázorňuje architekturu aplikace napříč platformami pomocí přenosné knihovny tříd ke sdílení kódu, ale také pomocí vkládání závislostí a zajistěte tak předání platformy – závislé funkce:

[![](pcl-images/image1.png "Tento diagram znázorňuje architekturu aplikace napříč platformami pomocí přenosné knihovny tříd ke sdílení kódu, ale také pomocí vkládání závislostí a zajistěte tak předání platformy – závislé funkce")](pcl-images/image1.png#lightbox)

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

## <a name="visual-studio-for-mac-walkthrough"></a>Visual Studio pro Mac návodu

Tato část vás provede postupy vytváření a používání knihovny přenosných tříd pomocí sady Visual Studio pro Mac. Najdete části Příklad PCL úplnou implementaci.

### <a name="creating-a-pcl"></a>Vytváření PCL

Přidání knihovny přenosných tříd pro vaše řešení je velmi podobné přidání pravidelné projektové knihovny.

1. V **nový projekt** dialogové okno Vybrat **Multiplatformní > knihovny > Portable Library** možnost:

    ![Vytvořte nový projekt PCL](pcl-images/image2.png)

1. Když PCL je vytvořen v sadě Visual Studio pro Mac se automaticky nakonfiguruje se profil, který se dá použít pro Xamarin.iOS a Xamarin.Android. Projekt PCL se zobrazí, jak je znázorněno na tomto snímku obrazovky:

    ![Projekt PCL v oblasti řešení](pcl-images/image3.png)

PCL je nyní připraven pro kód, který chcete přidat. Můžete jej také odkazovat pomocí jiné projekty (projekty aplikací, projekty knihovny a dokonce i v dalších projekty PCL).

### <a name="editing-pcl-settings"></a>Úprava nastavení PCL

Chcete-li zobrazit a změnit nastavení PCL pro tento projekt, klikněte pravým tlačítkem na projekt a zvolte **možnosti > sestavení > Obecné** zobrazíte na obrazovce je vidět tady:

[![Možnosti projektu PCL nastavení profilu](pcl-images/image4-sml.png)](pcl-images/image4.png#lightbox)

Klikněte na tlačítko **změn...**  změnit cílový profil pro tuto přenosnou knihovnu tříd.

Pokud se profil změní po kód již byla přidána do PCL, je možné, že knihovny nebudou nadále kompilovány, pokud kód odkazuje na funkce, které nejsou součástí profilu nově vybrané.

## <a name="working-with-a-pcl"></a>Práce s PCL

Při zápisu kódu v knihovně PCL, pozná omezení vybraný profil sady Visual Studio pro Mac editoru a odpovídajícím způsobem upravit možnosti automatického dokončování. Například tento snímek obrazovky ukazuje možnosti automatického dokončování pro System.IO, pomocí výchozího profilu (Profile136) používá v sadě Visual Studio for Mac – Všimněte si, že posuvník, který určuje přibližně polovinu dostupné třídy jsou zobrazeny (ve skutečnosti jsou jenom 14 dostupnost tříd).

[![Technologie IntelliSense seznam 14 tříd ve třídě System.IO PCL](pcl-images/image6.png)](pcl-images/image6.png#lightbox)

Porovnat s System.IO automatického dokončování v projektu Xamarin.iOS nebo Xamarin.Android – existují 40 třídy k dispozici včetně běžně používané třídy typu `File` a `Directory` které nejsou v jakékoli profilem PCL.

[![Technologie IntelliSense seznam 40 třídy v oboru názvů System.IO rozhraní .NET Framework](pcl-images/image7.png)](pcl-images/image7.png#lightbox)

To odpovídá základní kompromisy použití PCL – umožňuje bezproblémové sdílení kódu napříč mnoha platformách znamená, že určitých rozhraní API nejsou k dispozici, protože nemají srovnatelné implementace na všech platformách je to možné.

### <a name="using-pcl"></a>Využívající PCL

Po vytvoření projektu PCL přidáním stejně, jako je obvykle přidat odkazy na ni odkaz z projektu žádné kompatibilní aplikace nebo knihovna. V sadě Visual Studio pro Mac, klikněte pravým tlačítkem na uzel odkazy a zvolte **upravit odkazy...**  poté přejděte **projekty** kartu, jak je znázorněno:

[![Přidejte odkaz na PCL přes možnost Upravit odkazy](pcl-images/image8.png)](pcl-images/image8.png#lightbox)

Následující snímek obrazovky ukazuje oblasti řešení pro ukázkovou aplikaci TaskyPortable zobrazující knihovny PCL dole a odkaz na knihovny PCL v projektu Xamarin.iOS.

[![TaskyPortable ukázkové řešení zobrazující projekt PCL](pcl-images/image9.png)](pcl-images/image9.png#lightbox)

Výstup z PCL (tj. výsledné sestavení knihovny DLL) lze také přidat jako odkaz na většinu projektů. Díky tomu PCL ideální způsob, jak dodávat multiplatformní komponenty a knihovny.

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

## <a name="visual-studio-walkthrough"></a>Visual Studio návodu

Tato část vás provede postupem vytvoření a používání knihovny přenosných tříd pomocí sady Visual Studio. Najdete části Příklad PCL úplnou implementaci.

### <a name="creating-a-pcl"></a>Vytváření PCL

Přidání PCL do řešení v sadě Visual Studio se mírně liší na přidání běžného projektu:

1. V **přidat nový projekt** obrazovky, vyberte **knihovna tříd (starší přenosná)** možnost. Poznámka: Popis na pravé straně vás informuje o tom, že tento typ projektu je zastaralý.

    [![Okno nového projektu pro vytvoření knihovny přenosných tříd](pcl-images/image10-sml.png "přenosné knihovny tříd")](pcl-images/image10.png#lightbox)

2. Visual Studio vás vyzve okamžitě se zobrazí následující dialogové okno, tak, aby profil, který lze nakonfigurovat.
 Osové platformy, které potřebujete pro podporu a klikněte na tlačítko OK.

    [![Vyberte cílové platformy pro knihovnu](pcl-images/image11-sml.png "osové platformy, budete potřebovat pro podporu a klikněte na tlačítko OK")](pcl-images/image11.png#lightbox)

3. Projekt PCL se zobrazí, jak je znázorněno v Průzkumníku řešení &ndash; text **(přenosné)** zobrazí vedle názvu projektu označuje PCL:

    ![NET Framework definované profilem PCL](pcl-images/image12.png "NET Framework definované profilem PCL")

PCL je nyní připraven pro kód, který chcete přidat. Můžete jej také odkazovat pomocí jiné projekty (projekty aplikací, projekty knihovny a dokonce i v dalších projekty PCL).

### <a name="editing-pcl-settings"></a>Úprava nastavení PCL

PCL nastavení můžete zobrazit a změnit tak, že kliknete pravým tlačítkem na projekt a zvolíte **vlastnosti > knihovny** , jak je znázorněno na tomto snímku obrazovky:

[![Úprava cíle platformy](pcl-images/image13-sml.png)](pcl-images/image13.png#lightbox)

Pokud se profil změní po kód již byla přidána do PCL, je možné, že knihovny nebudou nadále kompilovány, pokud kód odkazuje na funkce, které nejsou součástí profilu nově vybrané.

> [!TIP]
> K dispozici je také zprávy, který radí **. NETStandard je doporučená metoda pro sdílení kódu**. To slouží jako ukazatel toho, že zatímco PCLs jsou stále podporovány, doporučuje se upgradovat na .NET Standard.

### <a name="working-with-a-pcl"></a>Práce s PCL

Při zápisu kódu v knihovně PCL, pozná omezení vybraný profil sady Visual Studio a odpovídajícím způsobem upravit možnosti technologie Intellisense. Například tento snímek obrazovky znázorňuje možnosti automatického dokončování pro System.IO, pomocí výchozího profilu (Profile136) – Všimněte si, že posuvník, který určuje přibližně polovinu dostupné třídy jsou zobrazeny (ve skutečnosti existují pouze 14 třídy k dispozici).

[![Snížený počet vstupně-výstupní operace tříd, které jsou k dispozici v PCL](pcl-images/image14.png)](pcl-images/image14.png#lightbox)

Porovnat s System.IO automatického dokončování v pravidelné projektové – existují 40 třídy k dispozici včetně běžně používané třídy typu `File` a `Directory` které nejsou v jakékoli profilem PCL.

[![Mnoho další vstupně-výstupní operace tříd k dispozici v rozhraní .NET Framework](pcl-images/image15.png)](pcl-images/image15.png#lightbox)

To odpovídá základní kompromisy použití PCL – umožňuje bezproblémové sdílení kódu napříč mnoha platformách znamená, že určitých rozhraní API nejsou k dispozici, protože nemají srovnatelné implementace na všech platformách je to možné.

> [!TIP]
> Představuje rozhraní .NET standard 2.0 mnohem větší svrchní oblasti rozhraní API než PCLs, včetně System.IO – obor názvů. Pro nové projekty .NET Standard je vhodná pro PCL.

### <a name="using-pcl"></a>Využívající PCL

Po vytvoření projektu PCL přidáním stejně, jako je obvykle přidat odkazy na ni odkaz z projektu žádné kompatibilní aplikace nebo knihovna. V sadě Visual Studio, klikněte pravým tlačítkem na uzel odkazy a zvolte `Add Reference...` pak přepnout do **řešení > projekty** kartu, jak je znázorněno:

[![Přidat odkaz na kartě přidat odkaz na projekty PCL](pcl-images/image16.png)](pcl-images/image16.png#lightbox)

Na následujícím snímku obrazovky se zobrazí v podokně řešení pro ukázkovou aplikaci TaskyPortable zobrazující knihovny PCL dole a odkaz na knihovny PCL v projektu Xamarin.iOS.

[![Ukázkové řešení TaskyPortable zobrazující knihovny PCL](pcl-images/image17.png)](pcl-images/image17.png#lightbox)

Výstup z PCL (tj. výsledné sestavení knihovny DLL) lze také přidat jako odkaz na většinu projektů.
Díky tomu PCL ideální způsob, jak dodávat multiplatformní komponenty a knihovny.

-----

## <a name="pcl-example"></a>Příklad PCL

[TaskyPortable](https://developer.xamarin.com/samples/mobile/TaskyPortable/) ukázkové aplikaci ukazuje použití přenosné knihovny tříd s využitím kódu Xamarin.
Tady jsou některé snímky výsledné aplikace běžící na zařízení s iOS a Android:

[![](pcl-images/image18.png "Tady jsou některé snímky výsledné aplikace pro iOS, Android a Windows Phone")](pcl-images/image18.png#lightbox)

Sdílí řadu data a logiku tříd, které jsou čistě přenositelný kód, a také ukazuje, jak začlenit požadavky specifické pro platformu pomocí vkládání závislostí pro implementaci databáze SQLite.

Struktury řešení jsou uvedené níže (v sadě Visual Studio pro Mac a Visual Studio v uvedeném pořadí):

[![](pcl-images/image19.png "Řešení struktura je znázorněna zde v sadě Visual Studio pro Mac a Visual Studio v uvedeném pořadí")](pcl-images/image19.png#lightbox)

Protože kód SQLite NET má specifické pro platformu kusy (pro práci s implementacemi SQLite na všech různých operačních systémech) pro demonstrační účely byla refaktorována do abstraktní třída, která může být zkompilovány do knihovny přenosných tříd a Skutečný kód implementovaný jako podtřídy v iOS a Android projekty.

### <a name="taskyportablelibrary"></a>TaskyPortableLibrary

Knihovny přenosných tříd je omezené funkce rozhraní .NET, které může podporovat. Protože kompilaci a spouštění na více platformách, nemůžete provádět využívání `[DllImport]` funkce, které se používá v SQLite NET. Místo toho SQLite-NET je implementovaný jako abstraktní třídu a pak postupujte podle zbývajících kroků sdílený kód odkazuje. Výpis abstraktní rozhraní API je zobrazena níže:

```csharp
public abstract class SQLiteConnection : IDisposable {

    public string DatabasePath { get; private set; }
    public bool TimeExecution { get; set; }
    public bool Trace { get; set; }
    public SQLiteConnection(string databasePath) {
         DatabasePath = databasePath;
    }
    public abstract int CreateTable<T>();
    public abstract SQLiteCommand CreateCommand(string cmdText, params object[] ps);
    public abstract int Execute(string query, params object[] args);
    public abstract List<T> Query<T>(string query, params object[] args) where T : new();
    public abstract TableQuery<T> Table<T>() where T : new();
    public abstract T Get<T>(object pk) where T : new();
    public bool IsInTransaction { get; protected set; }
    public abstract void BeginTransaction();
    public abstract void Rollback();
    public abstract void Commit();
    public abstract void RunInTransaction(Action action);
    public abstract int Insert(object obj);
    public abstract int Update(object obj);
    public abstract int Delete<T>(T obj);

    public void Dispose()
    {
        Close();
    }
    public abstract void Close();

}
```

Zbývající část sdílený kód používá abstraktní třídu pro "úložiště" a "načítání" objekty z databáze. V jakékoli aplikaci, která používá tato abstraktní třída musí předáváme zcela implementován, která poskytuje funkčnost skutečné databáze.

### <a name="taskyandroid-and-taskyios"></a>TaskyAndroid a TaskyiOS

IOS a aplikace pro Android projekty obsahují uživatelské rozhraní a jiného kódu specifické pro platformu používá k navázání sdíleného kódu ve PCL.

Tyto projekty obsahují také implementaci abstraktní databáze rozhraní API, které funguje na této platformě. V Iosu a Androidu Sqlite databázový stroj je vestavěné do operačního systému, abyste mohli použít implementaci `[DllImport]` jak je znázorněno na konkrétní implementaci připojení k databázi. Výňatek specifické pro platformu implementační kód je znázorněna zde:

```csharp
[DllImport("sqlite3", EntryPoint = "sqlite3_open")]
public static extern Result Open(string filename, out IntPtr db);

[DllImport("sqlite3", EntryPoint = "sqlite3_close")]
public static extern Result Close(IntPtr db);
```

Toto plně implementováno si můžete prohlédnout ve vzorovém kódu.

## <a name="summary"></a>Souhrn

Tento článek obsahuje stručně popsané výhody a nástrahy přenosné knihovny tříd, ukázal, jak vytvářet a využívat PCLs z v sadě Visual Studio pro Mac a Visual Studio; a nakonec zavedl kompletní ukázkovou aplikaci – TaskyPortable –, který zobrazuje PCL v akci.

## <a name="related-links"></a>Související odkazy

- [TaskyPortable (ukázka)](https://developer.xamarin.com/samples/mobile/TaskyPortable/)
- [Vytváření multiplatformních aplikací](~/cross-platform/app-fundamentals/building-cross-platform-applications/index.md)
- [Přenosné jazyka Visual Basic](~/cross-platform/platform/visual-basic/index.md)
- [Sdílené projekty](~/cross-platform/app-fundamentals/shared-projects.md)
- [Možnosti sdílení kódu](~/cross-platform/app-fundamentals/code-sharing.md)
- [Vývoj Multiplatformních aplikací pomocí rozhraní .NET Framework (Microsoft)](https://docs.microsoft.com/dotnet/standard/cross-platform/cross-platform-development-with-the-portable-class-library)
