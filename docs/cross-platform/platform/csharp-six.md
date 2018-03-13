---
title: "Funkce C# 6 nové – přehled"
description: "Nejnovější verzi jazyka C# – verze 6 – stále vyvíjí jazyka, který má mít méně často používaný, lepší přehlednost a další konzistence. Čisticí inicializace syntaxe, možnost používat await v bloky catch/finally a null podmíněné? operátor jsou obzvláště užitečná."
ms.topic: article
ms.prod: xamarin
ms.assetid: 4B4E41A8-68BA-4E2B-9539-881AC19971B
ms.technology: xamarin-cross-platform
ms.custom: xamu-video
author: asb3993
ms.author: amburns
ms.date: 03/22/2017
ms.openlocfilehash: 80456287c92913b048b73f40d2db6dcb16cc270d
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 03/09/2018
---
# <a name="c-6-new-features-overview"></a>Funkce C# 6 nové – přehled

_Nejnovější verzi jazyka C# – verze 6 – stále vyvíjí jazyka, který má mít méně často používaný, lepší přehlednost a další konzistence. Čisticí inicializace syntaxe, možnost používat await v bloky catch/finally a null podmíněné? operátor jsou obzvláště užitečná._

Toto téma představuje nové funkce jazyka C# 6. Je plně nepodporuje mono kompilátoru a vývojářům můžete začít používat nové funkce pro všechny platformy cíl Xamarin.

Tento článek obsahuje stručný fragmenty kódu C# 6, které ilustrují základní použití.
Vzorová aplikace je příkazového řádku programu, který používá pro všechny cílové platformy Xamarin a využije k různým funkcím.


> [!VIDEO https://youtube.com/embed/7UdV7zGPfMU]

**Co je nového v C# 6 pomocí [univerzity Xamarin](https://university.xamarin.com/)**


## <a name="development-environment"></a>Vývojové prostředí

### <a name="mac"></a>Mac

* **Visual Studio pro Mac** podporu pro C# 6: můžete sestavit a zkompilovat Xamarin aplikací pomocí funkce jazyka C# 6.
  Další informace o [Visual Studio pro Mac](https://docs.microsoft.com/visualstudio/mac/).

### <a name="windows"></a>Windows

* **Visual Studio 2015 a 2017** a vyšší mít plnou podporu pro C# 6. Starší verze sady Visual Studio nebude podporovat C# 6.

* **Xamarin Studio pro Windows** nepodporuje funkce jazyka C# 6 v editoru.



## <a name="compiler"></a>Kompilátoru

Kompilátor Mono C# 6 je součástí Mono 4.0 nebo novější, což je [volně k dispozici ke stažení](http://www.mono-project.com/download/).
Visual Studio pro Mac se automaticky aktualizuje Mono instalace v systému.

Uživatelé Windows musí mít [Visual Studio 2015 nebo 2017 ^](https://www.visualstudio.com/) nainstalován kompilace kódu C# 6 (i v případě, že zvolíte Xamarin Studio pro Windows jako vaše IDE).

^ nebo  *[Microsoft sestavení nástroje 2015](http://www.microsoft.com/en-us/download/details.aspx?id=48159)*  pro příkazový řádek kompilace nebo sestavení servery, například.

## <a name="using-c-6"></a>Pomocí jazyka C# 6

Kompilátor jazyka C# 6 se používá v všechny nejnovější verze sady Visual Studio for Mac.
Ty pomocí kompilátory příkazového řádku by měl potvrďte, že `mcs --version` vrátí 4.0 nebo vyšší.
Visual Studio pro uživatele počítačů Mac můžete zkontrolovat, zda mají Mono 4 (nebo novější) nainstalována odkazující na **o sadě Visual Studio pro Mac > Visual Studio pro Mac > Zobrazit podrobnosti**.

## <a name="less-boilerplate"></a>Méně často používaný
### <a name="using-static"></a>pomocí statické
Výčty a určité třídy, jako `System.Math`, jsou primárně držitelé statické hodnoty a funkce. V jazyce C# 6, můžete importovat všechny statické členy typu s jedním `using static` příkaz. Compare – typické funkce trigonometrické v C# 5 a 6 C#:

```csharp
// Classic C#
class MyClass
{
    public static Tuple<double,double> SolarAngleOld(double latitude, double declination, double hourAngle)
    {
        var tmp = Math.Sin (latitude) * Math.Sin (declination) + Math.Cos (latitude) * Math.Cos (declination) * Math.Cos (hourAngle);
        return Tuple.Create (Math.Asin (tmp), Math.Acos (tmp));
    }
}

// C# 6
using static System.Math;

class MyClass
{
    public static Tuple<double, double> SolarAngleNew(double latitude, double declination, double hourAngle)
    {
        var tmp = Asin (latitude) * Sin (declination) + Cos (latitude) * Cos (declination) * Cos (hourAngle);
        return Tuple.Create (Asin (tmp), Acos (tmp));
    }
}
```

`using static` není zveřejnit `const` pole, jako například `Math.PI` a `Math.E`, přímo přístupné:

```csharp
for (var angle = 0.0; angle <= Math.PI * 2.0; angle += Math.PI / 8) ... 
//PI is const, not static, so requires Math.PI
```

### <a name="using-static-with-extension-methods"></a>použití statické s rozšiřující metody

`using static` Zařízení funguje trochu jinak se rozšiřující metody. I když rozšiřující metody jsou zapsány pomocí `static`, že nemají smysl bez instanci, na kterém se bude pracovat. Pokud Ano `using static` se používá s typem, který definuje rozšiřující metody, k dispozici na jejich cílový typ metody rozšíření (metody `this` typu). Například `using static System.Linq.Enumerable` můžete použít k rozšíření rozhraní API `IEnumerable<T>` objekty bez uvedení v všechny typy LINQ:

```csharp
using static System.Linq.Enumerable;
using static System.String;

class Program
{
    static void Main()
    {
        var values = new int[] { 1, 2, 3, 4 };
        var evenValues = values.Where (i => i % 2 == 0);
        System.Console.WriteLine (Join(",", evenValues));
    }
}
```

Předchozí příklad ukazuje rozdíl v chování: metoda rozšíření `Enumerable.Where` souvisí s poli při statickou metodu `String.Join` lze volat bez ohledu na `String` typu.

### <a name="nameof-expressions"></a>nameof výrazy
V některých případech chcete odkazovat na název, kterým jste dali proměnné nebo pole. V jazyce C# 6 `nameof(someVariableOrFieldOrType)` vrátí řetězec `"someVariableOrFieldOrType"`. Například při vyvolání `ArgumentException` budete velmi pravděpodobně má název, který argument je neplatný:

```csharp
throw new ArgumentException ("Problem with " + nameof(myInvalidArgument))
```

Hlavní výhodou `nameof` výrazy je, že jsou zaškrtnuté typ a jsou kompatibilní se používá technologii nástroje refaktoring. Typ kontrole `nameof` výrazy je zvláště Vítá v situacích, kde `string` se používá k dynamicky přidružení typů. Například v iOS `string` slouží k určení typu použitého k prototypu `UITableViewCell` objekty v `UITableView`. `nameof` můžete zajistit, že toto přidružení neselže z důvodu chybné nebo Nedbalými refaktoring:

```csharp
public override UITableViewCell GetCell (UITableView tableView, NSIndexPath indexPath)
{
    var cell = tableView.DequeueReusableCell (nameof(CellTypeA), indexPath);
    cell.TextLabel.Text = objects [indexPath.Row].ToString ();
    return cell;
}
```

I když můžete předat kvalifikovaný název, který `nameof`, pouze poslední elementu (za poslední `.`) je vrácen. Například můžete přidat datová vazba na platformě Xamarin.Forms:

```csharp
var myReactiveInstance = new ReactiveType ();
var myLabelOld.BindingContext = myReactiveInstance;
var myLabelNew.BindingContext = myReactiveInstance;
var myLabelOld.SetBinding (Label.TextProperty, "StringField");
var myLabelNew.SetBinding (Label.TextProperty, nameof(ReactiveType.StringField));
```

Dva volá, aby se `SetBinding` předali stejné hodnoty: `nameof(ReactiveType.StringField)` je `"StringField"`, nikoli `"ReactiveType.StringField"` podle původně očekávání.

## <a name="null-conditional-operator"></a>Operátor podmíněného hodnotu Null
Starší aktualizace C# představil koncepty typy s možnou hodnotou Null a operátor slučování null `??` ke snížení množství často používaný kód při zpracování hodnot Null. C# 6 pokračuje tento motiv "operátor podmíněného null" `?.`. Pokud se použije na objekt na pravé straně výrazu, operátor podmíněného hodnotu null, vrátí hodnotu člena, pokud objekt není `null` a `null` jinak:

```csharp
var ss = new string[] { "Foo", null };
var length0 = ss [0]?.Length; // 3
var length1 = ss [1]?.Length; // null
var lengths = ss.Select (s => s?.Length ?? 0); //[3, 0]
```

(I `length0` a `length1` jsou odvodit být typu `int?`)

Poslední řádek v předchozí příklad ukazuje `?` operátor podmíněného null v kombinaci s `??` operátor slučování hodnotu null. Vrátí nový operátor podmíněného null C# 6 `null` na 2. element v poli okamžiku operátor slučování null dojde k a zadává 0 `lengths` pole (na které je vhodné nebo není se samozřejmě konkrétní problém).

Operátor podmíněného null by měl vytváření vyžadovalo nadměrné snížíte počet potřeby kontrola null často používaný v mnoha, mnoho aplikací.

Existují určitá omezení na operátor podmíněného hodnotu null z důvodu nejednoznačnosti. Nelze provést okamžitě `?` se seznamem argumentů v závorkách, jako je může naděje dělat s delegáta:

```csharp
SomeDelegate?("Some Argument") // Not allowed
```

Ale `Invoke` lze použít k oddělení `?` ze seznamu argumentů a stále je označen jako zlepšování prostřednictvím `null`-kontrola blok standardní:

```csharp
public event EventHandler HandoffOccurred;
public override bool ContinueUserActivity (UIApplication application, NSUserActivity userActivity, UIApplicationRestorationHandler completionHandler)
{
    HandoffOccurred?.Invoke (this, userActivity.UserInfo);
    return true;
}
```

## <a name="string-interpolation"></a>Řetězec interpolace
`String.Format` Funkce tradičně použila indexy zástupnými symboly v řetězec formátu, například `String.Format("Expected: {0} Received: {1}.", expected, received`). Samozřejmě přidání nová hodnota má vždy podílejí nepříjemných málo úlohy počítají argumenty, spojená s přečíslováním zástupné symboly a vkládání nové argument do správné pořadí v seznamu argumentů.

C# 6 je nová funkce interpolace řetězec výrazně vylepšuje `String.Format`. Teď můžete přímo název proměnné v řetězci předponu `$`. Například:

```csharp
$"Expected: {expected} Received: {received}."
```

Proměnné se samozřejmě zaškrtnutí a proměnnou chybný nebo není k dispozici způsobí chybu kompilátoru.

Zástupné symboly nemusí být jednoduché proměnné, že může být jakýkoli výraz. V rámci těchto zástupných symbolů, můžete použít uvozovky *bez* uvozovací znaky těchto uvozovky. Pro instanci, pamatujte `"s"` v následujícím:

```csharp
var s = $"Timestamp: {DateTime.Now.ToString ("s", System.Globalization.CultureInfo.InvariantCulture )}"
```

Řetězec interpolace podporuje zarovnání a formátování syntaxe `String.Format`. Stejně jako jste psali dřív `{index, alignment:format}`, v C# 6 napíšete `{placeholder, alignment:format}`:

```csharp
using static System.Linq.Enumerable;
using System;

class Program
{
    static void Main ()
    {
        var values = new int[] { 1, 2, 3, 4, 12, 123456 };
        foreach (var s in values.Select (i => $"The value is { i,10:N2}.")) {
            Console.WriteLine (s);
        }
    Console.WriteLine ($"Minimum is { values.Min(i => i):N2}.");
    }
}
```
Výsledkem:

    The value is       1.00.
    The value is       2.00.
    The value is       3.00.
    The value is       4.00.
    The value is      12.00.
    The value is 123,456.00.
    Minimum is 1.00.

Řetězec interpolace je syntaktická sugar pro `String.Format`: nelze použít s `@""` textové literály a není kompatibilní s `const`, i když jsou použity žádné zástupné symboly:

```csharp
const string s = $"Foo"; //Error : const requires value
```

V použití běžně vytváření argumenty funkce s řetězec interpolace budete muset stále opatrně uvozovací znaky, kódování a problémy jazykovou verzi. Dotazy SQL a adresa URL se samozřejmě důležité pro úpravu. Stejně jako u `String.Format`, string interpolace používá `CultureInfo.CurrentCulture`. Pomocí `CultureInfo.InvariantCulture` je o něco více rozvláčný:

```csharp
Thread.CurrentThread.CurrentCulture  = new CultureInfo ("de");
Console.WriteLine ($"Today is: {DateTime.Now}"); //"21.05.2015 13:52:51"
Console.WriteLine ($"Today is: {DateTime.Now.ToString(CultureInfo.InvariantCulture)}"); //"05/21/2015 13:52:51"
```

## <a name="initialization"></a>Inicializace

C# 6 poskytuje řadu stručným způsobů určení vlastností, pole a členy.

### <a name="auto-property-initialization"></a>Inicializace automaticky – vlastnost

Auto vlastnosti teď jde inicializovat na stručným stejným způsobem jako pole. Neměnné automaticky vlastnosti může být zapsán s pouze getter:

```csharp
class ToDo
{
    public DateTime Due { get; set; } = DateTime.Now.AddDays(1);
    public DateTime Created { get; } = DateTime.Now;
```

V konstruktoru můžete nastavit hodnotu jen getter automaticky vlastnosti:

```csharp
class ToDo
{
    public DateTime Due { get; set; } = DateTime.Now.AddDays(1);
    public DateTime Created { get; } = DateTime.Now;
    public string Description { get; }

    public ToDo (string description)
    {
        this.Description = description; //Can assign (only in constructor!)
    }
```

Tato inicializace vlastností automaticky je obecné funkce úspory místa a skvělým nástrojem pro vývojáře, které chtějí zdůraznil neměnitelnosti v jejich objekty.

### <a name="index-initializers"></a>Inicializátory indexu

C# 6 zavádí inicializátory indexu, které vám umožní nastavit klíč a hodnotu v typy, které mají indexer. Obvykle je to pro `Dictionary`– styl datové struktury:

```csharp
partial void ActivateHandoffClicked (WatchKit.WKInterfaceButton sender)
{
    var userInfo = new NSMutableDictionary {
        ["Created"] = NSDate.Now,
        ["Due"] = NSDate.Now.AddSeconds(60 * 60 * 24),
        ["Task"] = Description
    };
    UpdateUserActivity ("com.xamarin.ToDo.edit", userInfo, null);
    statusLabel.SetText ("Check phone");
}
```

### <a name="expression-bodied-function-members"></a>Výraz vozidlo funkce členy

Lambda funkce mají několik výhod, z nichž jeden je jednoduše ukládání místa. Členy třídy výraz vozidlo podobně povolovat malé funkce vyjádřeno další stručně, než bylo možné v předchozích verzích C# 6.

Výraz vozidlo funkce členy použijte šipku syntaxe lambda spíše než tradiční bloku syntaxe:

```csharp
public override string ToString () => $"{FirstName} {LastName}";
```

Všimněte si, že syntaxe lambda šipku nepoužívá explicitního `return`. Pro funkce, který vrací `void`, výraz musí být také příkaz:

```csharp
public void Log(string message) => System.Console.WriteLine($"{DateTime.Now.ToString ("s", System.Globalization.CultureInfo.InvariantCulture )}: {message}");
```

Výraz vozidlo členy, je stále platí pravidlo, `async` je podporována pro metody, ale ne vlastnosti:

```csharp
//A method, so async is valid
public async Task DelayInSeconds(int seconds) => await Task.Delay(seconds * 1000);
//The following property will not compile
public async Task<int> LeisureHours => await Task.FromResult<char> (DateTime.Now.DayOfWeek.ToString().First()) == 'S' ? 16 : 5;
```

## <a name="exceptions"></a>Výjimky

Neexistuje žádné dva způsoby, jak o něm: zpracování výjimek je obtížné získat správné. Nové funkce v C# 6 zajistěte zpracování výjimek, větší flexibilitu a konzistentní.

### <a name="exception-filters"></a>Filtry výjimek

Podle definice výskytu výjimek v neobvyklé okolnosti a může být velmi obtížné důvod a kód o *všechny* způsobů, jak může dojít k výjimce určitého typu. C# 6 uvádí možnost ochrana obslužnou rutinu spuštění s filtrem vyhodnotit modulu runtime. To se provádí přidáním `when (bool)` vzor po normální `catch(ExceptionType)` deklarace. V následujícím příkladu filtr odlišuje Chyba analýzy týkající se `date` parametr rozdíl od jiných chybám analýzy.

```csharp
public void ExceptionFilters(string aFloat, string date, string anInt)
{
    try
    {
        var f = Double.Parse(aFloat);
        var d = DateTime.Parse(date);
        var n = Int32.Parse(anInt);
    } catch (FormatException e) when (e.Message.IndexOf("DateTime") > -1) {
        Console.WriteLine ($"Problem parsing \"{nameof(date)}\" argument");
    } catch (FormatException x) {
        Console.WriteLine ("Problem parsing some other argument");
    }
}
```

### <a name="await-in-catchfinally"></a>await v catch... finally...

`async` Možnosti zavedené v C# 5 byly herní měniče pro jazyk. V jazyce C# 5 `await` nebylo povoleno v `catch` a `finally` blokuje obtěžující danou hodnotu `async/await` schopností. C# 6 odebere toto omezení, což asynchronní výsledky být konzistentně očekávaná prostřednictvím programu jak je znázorněno v následujícím fragmentu kódu:

```csharp
async void SomeMethod()
{
    try {
        //...etc...
    } catch (Exception x) {
        var diagnosticData = await GenerateDiagnosticsAsync (x);
        Logger.log (diagnosticData);
    } finally {
        await someObject.FinalizeAsync ();
    }
}
```

## <a name="summary"></a>Souhrn

Jazyk C# stále vyvíjí aby vývojáři zvýšit produktivitu při také povýšení drželi osvědčených postupů a podpůrné nástroje. Tento dokument je uveden přehled nových funkcí jazyka v C# 6 a stručně ukázaly, jak se používají.

## <a name="related-links"></a>Související odkazy

- [Nové jazykové funkce v jazyce C# 6](https://github.com/dotnet/roslyn/wiki/New-Language-Features-in-C%23-6)
- [Pomocí jazyka C# 6 sešitu](https://developer.xamarin.com/workbooks/csharp/csharp6/csharp6.workbook)
