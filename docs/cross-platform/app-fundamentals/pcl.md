---
title: Úvod do knihovny přenosných tříd
description: Tento článek představuje projekty přenosných třída knihovny PCL () a provede procesem vytvoření a použití PCL projekty v sadě Visual Studio pro Mac a Visual Studio.
ms.prod: xamarin
ms.assetid: 76ba8f7a-9b6e-40f5-9a29-ff1274ece4f2
ms.technology: xamarin-cross-platform
author: asb3993
ms.author: amburns
ms.date: 03/23/2017
ms.openlocfilehash: feef0a4083d2455cc189ddab6ed22762c044d848
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/04/2018
---
# <a name="introduction-to-portable-class-libraries"></a>Úvod do knihovny přenosných tříd

_Tento článek představuje projekty přenosných třída knihovny PCL () a provede procesem vytvoření a použití PCL projekty v sadě Visual Studio pro Mac a Visual Studio._


Klíčovou součástí vytváření aplikací pro různé platformy je schopnost sdílet kódu v rámci různých projektů specifické pro platformu. To však ztěžuje skutečnost, že různé platformy často používají jinou sadu dílčí knihovny pro třídy Base .NET (BCL) a proto jsou ve skutečnosti vytvořená tak, aby jiný profil knihovny .NET Core. To znamená, že každou platformu můžete použít pouze knihovny tříd, které jsou cíleny na stejný profil, takže by se tak, aby vyžadovala projektů knihovny tříd samostatné pro každou platformu.

Existují tři hlavní přístupy sdílení kódu, které řeší tento problém: **.NET Standard projekty**, **projekty přenosných třída knihovny PCL ()**, a **sdílených projektů Asset**.

- **.NET standard projekty** [.NET Standard](~/cross-platform/app-fundamentals/net-standard.md).
-  **PCL** projektech cílové konkrétní profily, které podporují se známou sadou BCL třídy nebo funkce. Na straně dolů na PCL je však, že často vyžadují velmi architektury úsilí k oddělení profil konkrétního kódu do své vlastní knihovny. Podrobnější informace na tyto dva přístupy, najdete v článku [sdílení kódu možnosti Průvodce](~/cross-platform/app-fundamentals/code-sharing.md) .
-  **Sdílených projektů Asset** použít jednu sadu souborů a nabízí rychlý a jednoduchý způsob, do kterého chcete sdílet kódu v rámci řešení a obecně využívá Podmíněná kompilace direktivy k určení cesty kódu pro různé platformy, které budou používat (Další informace informace najdete v tématu [sdílených projektů článku](~/cross-platform/app-fundamentals/shared-projects.md) a [nastavení průvodce řešení pro různé platformy Xamarin](~/cross-platform/app-fundamentals/code-sharing.md) ).


Tato stránka vysvětluje, jak vytvořit **PCL** projektu, jehož cílem konkrétní profil, který může pak odkazovat více projektů specifické pro platformu.


## <a name="what-is-a-portable-class-library"></a>Co je přenosné knihovny tříd?

Když vytvoříte projekt aplikace nebo projektu knihovny, výsledná DLL je omezené na pracující na konkrétní platformu, kterou je vytvořeno. Nebudete z psaní sestavení pro aplikace pro Windows a pak ho znovu pomocí na Xamarin.iOS a Xamarin.Android.

Když vytvoříte přenosné knihovny tříd, ale můžete kombinaci platformy, které chcete, aby váš kód pro spuštění. Možnosti výběru kompatibility, které jste při vytváření přenosné knihovny tříd se přeložit na identifikátor "Profilu", který popisuje platforem podporuje knihovny.

Následující tabulka uvádí některé funkce, které se liší podle platformy .NET. Zápis PCL sestavení, který zaručeně běžet na určité zařízení nebo platformách jednoduše zvolíte podpory, které je potřeba při vytvoření projektu.

|Funkce|.NET Framework|Aplikace UWP|Silverlight|Windows Phone|Xamarin|
|---|---|---|---|---|---|
|Jádro|A|A|A|A|A|
|LINQ|A|A|A|A|A|
|IQueryable|A|A|A|7.5 +|A|
|Serializace|A|A|A|A|A|
|Datových poznámek|4.0.3 +|A|A||A|

Sloupec Xamarin odráží fakt, že Xamarin.iOS a Xamarin.Android podporuje všechny profily, které jsou součástí sady Visual Studio a dostupnost funkcí ve vytvořené knihovny bude omezena pouze jiné platformy, které zvolíte pro podporu.

Patří mezi ně profily, které jsou kombinace:

-  Rozhraní .NET 4 nebo .NET 4.5
-  Silverlight 5
-  Windows Phone 8
-  Aplikace UWP

Si můžete přečíst více o možnostech různé profily na [webu společnosti Microsoft](http://msdn.microsoft.com/en-us/library/gg597391(v=vs.110).aspx) a zobrazit jiného člena komunity [PCL profil souhrnné](http://embed.plnkr.co/03ck2dCtnJogBKHJ9EjY) což zahrnuje podporované framework údaje a informace o dalších poznámek.



Vytváření PCL sdílet kódu má počet výhody a nevýhody versus alternativní propojení souboru:


**Výhody**

1. Centralizované kód sdílení – zapsat a otestovat kód v jednom projektu, které mohou být spotřebovávána další knihovny nebo aplikace.
1. Refaktoring operace ovlivní všechny kód načíst v řešení (přenosné knihovny tříd a specifické pro platformu projektů).
1. Projekt PCL lze snadno odkazovat pomocí jiné projekty v řešení, nebo je možné sdílet sestavení výstupu ostatním uživatelům odkaz v jejich řešení.


**Nevýhody**

1. Protože stejné Přenosná knihovna tříd je sdílen mezi více aplikacemi, specifické pro platformu knihovny nelze na něj odkazovat (např. Community.CsharpSqlite.WP7).
1. Přenosná knihovna tříd podmnožinu nesmí obsahovat třídy, které by jinak byly k dispozici v MonoTouch a Mono pro Android (například DllImport nebo System.IO.File).



Do určité míry je možné obejít i nevýhody code skutečné implementace v projektech platformy proti rozhraní nebo základní třída, která je definována v Přenosná knihovna tříd pomocí zprostředkovatele vzor nebo vkládání závislostí.



Tento diagram znázorňuje architekturu aplikace napříč platformami pomocí přenosné knihovny tříd sdílet kód, ale také pomocí vkládání závislostí předávat platformy – závislé funkce:



[![](pcl-images/image1.png "Tento diagram znázorňuje architekturu aplikace napříč platformami pomocí přenosné knihovny tříd sdílet kód, ale také pomocí vkládání závislostí předávat platformy – závislé funkce")](pcl-images/image1.png#lightbox)



# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)



## <a name="visual-studio-for-mac-walkthrough"></a>Visual Studio pro Mac návod


Tato část vás provede postup vytvoření a používání knihovny přenosných tříd pomocí sady Visual Studio for Mac. Najdete sekci Příklad PCL pro dokončení implementace.



### <a name="creating-a-pcl"></a>Vytváření PCL


Přidání knihovny přenosných tříd pro vaše řešení je velmi podobný jako při přidávání regulární projektu knihovny.


1. V dialogovém okně Nový projekt vyberte `Multiplatform > Library > Portable Library` možnost


    ![](pcl-images/image2.png "Multiplatform > Knihovna > přenosné knihovny")


1. Když PCL je vytvořena v sadě Visual Studio pro Mac, je automaticky nakonfigurovaný pomocí profil, který se dá použít pro Xamarin.iOS a Xamarin.Android. PCL projektu se zobrazí, jak je vidět na tomto snímku obrazovky:

    ![](pcl-images/image3.png "PCL projektu se zobrazí, jak je vidět na tomto snímku obrazovky")

PCL je nyní připraven pro kód, který se má přidat. Může být také odkaz Další projekty (aplikace projekty – projekty knihovny a i další projekty PCL).



### <a name="editing-pcl-settings"></a>Úprava nastavení PCL


K zobrazení a změna PCL nastavení pro tento projekt, klikněte pravým tlačítkem na projekt a zvolte **možnosti > sestavení > Obecné** zobrazíte obrazovce znázorněno zde:



[![](pcl-images/image4.png "K zobrazení a změna PCL nastavení pro tento projekt, klikněte pravým tlačítkem na projekt a zvolte možnosti sestavení obecné zobrazíte tady uvedené obrazovky")](pcl-images/image4.png#lightbox)



Nastavení na této obrazovce řídí platformy, na kterých tato konkrétní PCL je kompatibilní s. Změna některé z těchto možností mění profil používá tento PCL, který naopak řídí, jaké funkce můžou použít v přenosných kódu.



Změna libovolné `Target Framework` možnosti automaticky aktualizuje `Current Profile`; obrazovky se také zobrazí upozornění, pokud jsou vybrané nekompatibilní možnosti.



[![](pcl-images/image5.png "Změna některé z možností cílové rozhraní automaticky aktualizuje aktuální profil obrazovky se také zobrazí upozornění, pokud jsou vybrané nekompatibilní možnosti")](pcl-images/image5.png#lightbox)



Profil dojde ke změně po kód již byl přidán do PCL, je možné, že knihovny nebudou nadále kompilovány, pokud kód odkazuje na funkce, které nejsou součástí nově vybraný profil.


## <a name="working-with-a-pcl"></a>Práce PCL


Pokud kód je napsána v knihovny PCL, sady Visual Studio pro Mac editor rozpozná omezení vybraný profil a odpovídajícím způsobem upravit možnosti automatického dokončování. Například tento snímek obrazovky znázorňuje možnosti automatického dokončování System.IO, používá výchozí profil (Profile136) používá v sadě Visual Studio pro Mac – Všimněte si scrollbar označující o polovinu dostupných tříd se zobrazují (ve skutečnosti existují jenom 14 třídy k dispozici).



[![](pcl-images/image6.png "Vstupně-výstupní operace pomocí výchozí profil Profile136 používá v sadě Visual Studio pro Mac oznámení scrollbar, který označuje o polovinu dostupných tříd. zobrazí se ve skutečnosti jsou pouze 14 třídy, které jsou k dispozici")](pcl-images/image6.png#lightbox)



Porovnání s System.IO automatického dokončování v projektu Xamarin.iOS nebo Xamarin.Android – existují 40 třídy, které jsou k dispozici včetně běžně používané třídy jako `File` a `Directory` které nejsou v jakékoli PCL profilu.



[![](pcl-images/image7.png "Nejsou k dispozici včetně běžně používaných tříd jako soubor a adresáře, které nejsou v žádný profil PCL 40 třídy")](pcl-images/image7.png#lightbox)



Tento údaj zohledňuje základní kompromis použití PCL – možnost sdílet kód bezproblémově napříč mnoha platformách znamená, že některé rozhraní API nejsou k dispozici, protože nemají porovnatelný z hlediska implementace pro všechny možné platformy.



### <a name="using-pcl"></a>Pomocí PCL


Po vytvoření projektu PCL, můžete přidat odkaz na jeho ze žádného kompatibilní aplikace nebo knihovna projektu stejným způsobem jako za normálních okolností přidáte odkazy. V sadě Visual Studio pro Mac, klikněte pravým tlačítkem na uzel odkazy a zvolte `Edit References…` pak přejděte na kartu projekty, jak vidíte:



[![](pcl-images/image8.png "V sadě Visual Studio pro Mac klikněte pravým tlačítkem na uzel odkazy a zvolte Upravit odkazy a přepněte na kartu projekty, jak je znázorněno")](pcl-images/image8.png#lightbox)



Následující snímek obrazovky ukazuje panelu pro řešení pro ukázkovou aplikaci TaskyPortable zobrazující knihovny PCL dole a odkaz na knihovny PCL v projektu Xamarin.iOS.



[![](pcl-images/image9.png "Odsazení řešení pro ukázkovou aplikaci TaskyPortable")](pcl-images/image9.png#lightbox)



Výstup PCL (ie. výsledné sestavení knihoven DLL) lze také přidat jako odkaz na většinu projektů. Díky tomu PCL nejvhodnější způsob pro odeslání součásti napříč platformami a knihovny.




# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)




## <a name="visual-studio-walkthrough"></a>Návod pro Visual Studio


Tato část vás provede postup vytvoření a používání knihovny přenosných tříd pomocí sady Visual Studio. Najdete sekci Příklad PCL pro dokončení implementace.



### <a name="creating-a-pcl"></a>Vytváření PCL


Přidávání PCL do řešení v sadě Visual Studio se mírně liší při přidávání pravidelné projektové.



1. V dialogovém okně Přidat nový projekt, vyberte `Portable Class Library` možnost


    ![](pcl-images/image10.png "Přenosná knihovna tříd")


1. Visual Studio okamžitě, vyzve se následující dialogové okno tak, aby profil lze nakonfigurovat.
 Osové platformy, které potřebujete podporovat a klikněte na tlačítko OK.


    ![](pcl-images/image11.png "Osové platformy, které potřebujete podporovat a klikněte na tlačítko OK")


1. PCL projektu se zobrazí, jak je znázorněno v Průzkumníku řešení. Uzel odkazy budou indikovat, že knihovny používá podmnožinu rozhraní .NET Framework (definované profilem PCL).

    ![](pcl-images/image12.png "NET Framework definované profilem PCL")

PCL je nyní připraven pro kód, který se má přidat. Může být také odkaz Další projekty (aplikace projekty – projekty knihovny a i další projekty PCL).



### <a name="editing-pcl-settings"></a>Úprava nastavení PCL


Nastavení PCL můžete zobrazit a změnit tak, že kliknete pravým tlačítkem na projekt a výběr **vlastnosti > knihovny** , jak je vidět na tomto snímku obrazovky:



[![](pcl-images/image13.png "Nastavení PCL můžete zobrazit a změnit tak, že kliknete pravým tlačítkem na projekt a výběr vlastnosti knihovny, jak je vidět na tomto snímku obrazovky")](pcl-images/image13.png#lightbox)



Profil dojde ke změně po kód již byl přidán do PCL, je možné, že knihovny nebudou nadále kompilovány, pokud kód odkazuje na funkce, které nejsou součástí nově vybraný profil.



### <a name="working-with-a-pcl"></a>Práce PCL


Kód v knihovny PCL zápisu, Visual Studio rozpozná omezení vybraný profil a odpovídajícím způsobem upravit možnosti Intellisense. Například tento snímek obrazovky znázorňuje možnosti automatického dokončování System.IO, pomocí výchozí profil (Profile136) – Všimněte si scrollbar označující o polovinu dostupných tříd se zobrazují (ve skutečnosti existuje pouze 14 třídy).



[![](pcl-images/image14.png "Pomocí výchozí profil Profile136 vstupně-výstupní operace")](pcl-images/image14.png#lightbox)



Porovnání s System.IO automatického dokončování v pravidelné projektové – existují 40 třídy, které jsou k dispozici včetně běžně používané třídy jako `File` a `Directory` které nejsou v jakékoli PCL profilu.



[![](pcl-images/image15.png "Automatické dokončování v pravidelných projektu")](pcl-images/image15.png#lightbox)



Tento údaj zohledňuje základní kompromis použití PCL – možnost sdílet kód bezproblémově napříč mnoha platformách znamená, že některé rozhraní API nejsou k dispozici, protože nemají porovnatelný z hlediska implementace pro všechny možné platformy.



### <a name="using-pcl"></a>Pomocí PCL


Po vytvoření projektu PCL, můžete přidat odkaz na jeho ze žádného kompatibilní aplikace nebo knihovna projektu stejným způsobem jako za normálních okolností přidáte odkazy. V sadě Visual Studio, klikněte pravým tlačítkem na uzel odkazy a zvolte `Add Reference...` potom přepnout **řešení: projekty** kartě, jak je znázorněno:



[![](pcl-images/image16.png "Karta projekty, jak je znázorněno")](pcl-images/image16.png#lightbox)



Následující snímek obrazovky ukazuje na řešení panelu ukázkové aplikace TaskyPortable zobrazující knihovny PCL dole a odkaz na knihovny PCL v projektu Xamarin.iOS.



[![](pcl-images/image17.png "V podokně řešení pro ukázkovou aplikaci TaskyPortable")](pcl-images/image17.png#lightbox)



Výstup PCL (ie. výsledné sestavení knihoven DLL) lze také přidat jako odkaz na většinu projektů.
Díky tomu PCL nejvhodnější způsob pro odeslání součásti napříč platformami a knihovny.




-----



## <a name="pcl-example"></a>Příklad PCL


[TaskyPortable](https://developer.xamarin.com/samples/mobile/TaskyPortable/) ukázkovou aplikaci ukazuje, jak lze pomocí Xamarinu přenosné knihovny tříd.
Tady jsou některé snímky obrazovky výsledná aplikace běžící v systému iOS, Android a Windows Phone:



[![](pcl-images/image18.png "Tady jsou některé snímky obrazovky výsledná aplikace běžící v systému iOS, Android a Windows Phone")](pcl-images/image18.png#lightbox)



Sdílí počet dat a logiku třídy, které jsou čistě přenosné kódu a také ukazuje, jak začlenit specifické pro platformu požadavky s využitím vkládání závislostí pro implementaci databáze SQLite.




Struktura řešení jsou uvedeny níže (v sadě Visual Studio pro Mac a Visual Studio v uvedeném pořadí):



[![](pcl-images/image19.png "Struktura řešení je tady uvedené v sadě Visual Studio pro Mac a Visual Studio v uvedeném pořadí")](pcl-images/image19.png#lightbox)



Protože kód SQLite NET má specifické pro platformu částí (pro práci s implementacemi SQLite na každý jiný operační systém) pro demonstrační účely byla s teď vyčleněný do abstraktní třídy, které mohou být zkompilovány do knihovny přenosných tříd a skutečné kódu implementované jako podtřídy v iOS a Android projekty.



### <a name="taskyportablelibrary"></a>TaskyPortableLibrary

Přenosná knihovna tříd je omezená funkce rozhraní .NET, které může podporovat. Protože je kompilován běžet na několika platformách, nelze provádět použití `[DllImport]` funkce, které se používá v SQLite NET. Místo toho SQLite NET je implementovaný jako abstraktní třídu a pak odkazuje prostřednictvím rest sdíleného kódu. Výpis abstraktní rozhraní API je zobrazena níže:


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


Zbývající část sdílené kód používá abstraktní třída "úložiště" a "načtení" objektů z databáze. V jakékoli aplikaci, která používá tato abstraktní třída jsme musí projít dokončení implementace, která poskytuje funkce skutečného databáze.



### <a name="taskyandroid-and-taskyios"></a>TaskyAndroid a TaskyiOS


Projekty aplikace pro Android a iOS obsahovat uživatelského rozhraní a jiný kód specifické pro platformu používá k navázání sdílené kód PCL.



Tyto projekty také obsahovat implementaci abstraktní databáze rozhraní API, které na této platformě funguje. Na iOS a Android Sqlite databázový stroj je integrované do operačního systému, takže můžete použít implementaci `[DllImport]` znázorněné poskytnout konkrétní implementace připojení k databázi. Zobrazí se zde výňatek kód implementace specifických pro platformy:


```csharp
[DllImport("sqlite3", EntryPoint = "sqlite3_open")]
public static extern Result Open(string filename, out IntPtr db);

[DllImport("sqlite3", EntryPoint = "sqlite3_close")]
public static extern Result Close(IntPtr db);
```


Úplnou implementaci si můžete prohlédnout ve ukázkový kód.

### <a name="taskywinphone"></a>TaskyWinPhone


Aplikace Windows Phone má jeho uživatelském rozhraní vytvořené s XAML a obsahuje další kód specifický pro platformu pro připojení sdílené objekty s uživatelským rozhraním.



Na rozdíl od implementace používaná pro iOS a Android, musíte vytvořit a použít instanci aplikace Windows Phone `Community.Sqlite.dll` jako součást jeho abstraktní databáze rozhraní API. Místo použití `DllImport`, metody, třeba otevřete jsou implementované proti Community.Sqlite sestavení, které odkazuje `TaskWinPhone` projektu. Výňatek se zde zobrazí pro porovnání s iOS a Android verze výše


```csharp
public static Result Open(string filename, out Sqlite3.sqlite3 db)
{
    db = new Sqlite3.sqlite3();
    return (Result)Sqlite3.sqlite3_open(filename, ref db);
}

public static Result Close(Sqlite3.sqlite3 db)
{
    return (Result)Sqlite3.sqlite3_close(db);
}
```


## <a name="summary"></a>Souhrn


Tento článek má stručně popsané výhody a nástrahy přenosné knihovny tříd, ukázal, jak lze vytvářet a využívat PCLs z v sadě Visual Studio pro Mac a Visual Studio; a nakonec zavedl kompletní ukázkovou aplikaci – TaskyPortable –, která ukazuje PCL v akci.


## <a name="related-links"></a>Související odkazy

- [TaskyPortable (ukázka)](https://developer.xamarin.com/samples/mobile/TaskyPortable/)
- [Vytváření multiplatformních aplikací](~/cross-platform/app-fundamentals/building-cross-platform-applications/index.md)
- [Přenosné jazyka Visual Basic](~/cross-platform/platform/visual-basic/index.md)
- [Sdílené projekty](~/cross-platform/app-fundamentals/shared-projects.md)
- [Možnosti sdílení kódu](~/cross-platform/app-fundamentals/code-sharing.md)
- [Vývoj pro různé platformy s rozhraním .NET Framework (Microsoft)](http://msdn.microsoft.com/en-us/library/gg597391(v=vs.110).aspx)
