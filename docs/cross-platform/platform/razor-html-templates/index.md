---
title: Vytváření zobrazení HTML pomocí šablon Razor
description: " Pomocí celé obrazovky webové stránky k vykreslení HTML může být jednoduchý a efektivní způsob vykreslení komplexní formátování způsobem, a platformy, zejména pokud již máte jazyka HTML, Javascript a CSS z projektu webu."
ms.prod: xamarin
ms.assetid: D8B87C4F-178E-48D9-BE43-85066C46F05C
author: asb3993
ms.author: amburns
ms.date: 02/18/2018
ms.openlocfilehash: 48d7778bf3225401f2819909ae6be320cfa881e3
ms.sourcegitcommit: 0a72c7dea020b965378b6314f558bf5360dbd066
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 05/09/2018
---
# <a name="building-html-views-using-razor-templates"></a>Vytváření zobrazení HTML pomocí šablon Razor

Na světě pro vývoj mobilních řešení obvykle termín "hybridní aplikace" odkazuje na aplikaci, která uvede některé (nebo všechny) jeho obrazovky jako stránky HTML v ovládacím prvku hostované webového prohlížeče.

Existují některé vývojových prostředí, které vám umožní sestavení mobilní aplikace zcela v HTML a Javascript a však tyto aplikace můžete trpí problémy s výkonem při pokusu o provedení komplexní zpracování nebo důsledky uživatelského rozhraní a jsou také omezena platformy Funkce, které mají přístup.

Xamarin nabízí nejlepší z obou světů, zejména v případě, že použití modulu ukázka Razor HTML. Pomocí Xamarinu máte možnost vytvořit napříč platformami šablony HTML zobrazení, které používat Javascript a CSS, ale také mít úplný přístup k rozhraní API základní platformy a rychlé zpracování pomocí jazyka C#.

Tento dokument vysvětluje, jak použít modul ukázka sestavení zobrazení HTML + Javascript + CSS, která lze použít v rámci mobilních platforem pomocí Xamarinu syntaxi Razor.

## <a name="using-web-views-programmatically"></a>Použití webového zobrazení prostřednictvím kódu programu

Před jsme Další informace o syntaxi Razor Tato část obsahuje informace o použití webové zobrazení na Zobrazit obsah HTML přímo – konkrétně obsah HTML, který je vytvářena v rámci aplikace.

Xamarin poskytuje úplný přístup k základní platformu rozhraní API na iOS a Android, takže můžete snadno vytvořit a zobrazení HTML pomocí jazyka C#. Základní syntaxe pro každou platformu, jsou uvedeny níže.

### <a name="ios"></a>iOS

Zobrazení HTML v ovládacím prvku UIWebView v Xamarin.iOS trvá i po zadání několika řádků kódu:

```csharp
var webView = new UIWebView (View.Bounds);
View.AddSubview(webView);
string contentDirectoryPath = Path.Combine (NSBundle.MainBundle.BundlePath, "Content/");
var html = "<html><h1>Hello</h1><p>World</p></html>";
webView.LoadHtmlString(html, NSBundle.MainBundle.BundleUrl);
```

Najdete v článku [iOS UIWebView](http://docs.xamarin.com/recipes/ios/content_controls/web_view/) recepty další podrobnosti o použití UIWebView ovládacího prvku.

### <a name="android"></a>Android

Zobrazení HTML ve webovém zobrazení ovládacího prvku s použitím Xamarin.Android se provádí v několika řádků kódu:

```csharp
// webView is declared in an AXML layout file
var webView = FindViewById<WebView> (Resource.Id.webView);
var html = "<html><h1>Hello</h1><p>World</p></html>";
webView.LoadDataWithBaseURL("file:///android_asset/", html, "text/html", "UTF-8", null);
```

Najdete v článku [Android webové zobrazení](http://docs.xamarin.com/recipes/android/controls/webview/) recepty další podrobnosti o použití ovládacího prvku webového zobrazení.

### <a name="specifying-the-base-directory"></a>Zadání základního adresáře

V obou platformy je parametr, který určuje základní adresář pro stránku HTML. Toto je umístění v systému souborů zařízení, která se používá k překladu relativní odkazy na prostředky, jako jsou bitové kopie a souborů CSS. Jako jsou například značky

```html
<link rel="stylesheet" href="style.css" />
<img src="monkey.jpg" />
<script type="text/javascript" src="jscript.js">
```

odkazují na tyto soubory: **style.css**, **monkey.jpg** a **jscript.js**. Nastavení základního adresáře informuje webového zobrazení, kde jsou tyto soubory umístěny aby mohly být načtena do stránky.

#### <a name="ios"></a>iOS

Výstup šablony je vykreslen v iOS pomocí následujícího kódu C#:

```csharp
webView.LoadHtmlString (page, NSBundle.MainBundle.BundleUrl);
```

Základní adresář je zadán jako `NSBundle.MainBundle.BundleUrl` které odkazuje na adresář, který bude aplikace nainstalována v. Všechny soubory v **prostředky** složky se zkopírují do tohoto umístění, jako **style.css** souboru zobrazeny zde:

 ![iPhoneHybrid řešení](images/image1_240x163.png)

Akce sestavení pro všechny statické soubory obsahu by měla být **BundleResource**:

 ![Akce sestavení projektu iOS: BundleResource](images/image2_250x131.png)

#### <a name="android"></a>Android

Android vyžaduje taky základního adresáře mají být předány jako parametr, pokud řetězce html se zobrazí ve webovém zobrazení.

```csharp
webView.LoadDataWithBaseURL("file:///android_asset/", page, "text/html", "UTF-8", null);
```

Speciální řetězec **file:///android_asset/** odkazuje na složku Android prostředky ve vaší aplikaci, zobrazí zde obsahující **style.css** souboru.

 ![AndroidHybrid řešení](images/image3_240x167.png)

Akce sestavení pro všechny statické soubory obsahu by měla být **AndroidAsset**.

 ![Akce sestavení projektu pro Android: AndroidAsset](images/image4_250x71.png)

### <a name="calling-c-from-html-and-javascript"></a>Volání jazyka C# z HTML a Javascript

Když stránku html je načten do webové zobrazení, se zpracuje odkazy a formuláře jako kdyby stránky byl načten ze serveru. To znamená, že pokud uživatel klikne na odkaz nebo odešle formulář webového zobrazení se pokusí o přejděte do zadaného cíle.

Pokud je odkaz k externímu serveru (například google.com) webové zobrazení se pokusí načíst externí web (za předpokladu, že existuje připojení k Internetu).

```html
<a href="http://google.com/">Google</a>
```

Pokud je relativní odkaz webové zobrazení se pokusí načíst tento obsah ze základního adresáře. Samozřejmě není nutné pro tento postup, žádné síťové připojení, protože obsah se ukládá v aplikaci na zařízení.

```html
<a href="somepage.html">Local content</a>
```

Akce formuláře podle stejného pravidla.

```html
<form method="get" action="http://google.com/"></form>
<form method="get" action="somepage.html"></form>
```

Nebudete k hostování webový server na straně klienta; ale můžete použít stejné techniky komunikaci serveru v dnešních přizpůsobivý návrh vzory volat služby prostřednictvím metody GET protokolu HTTP a zpracování odpovědi asynchronně emitování Javascript (nebo volání Javascript už hostovaný ve webovém zobrazení). To umožňuje snadno předat data z HTML zpět do kódu jazyka C# pro zpracování a zobrazení výsledků zpět na stránku HTML.

IOS a Android poskytují mechanismus pro kód aplikace zachytávat tyto události navigace, aby mohl kód aplikace odpovídat (v případě potřeby). Tato funkce je nezbytné k vytváření hybridní aplikace, protože umožňuje pracovat s webového zobrazení nativního kódu.

#### <a name="ios"></a>iOS

Povolit aplikaci kód pro zpracování požadavku navigace (například klikněte na odkaz) může být přepsána ShouldStartLoad událostí na webové zobrazení v iOS. Parametry metody zadejte všechny informace

```csharp
bool HandleShouldStartLoad (UIWebView webView, NSUrlRequest request, UIWebViewNavigationType navigationType) {
    // return true if handled in code
    // return false to let the web view follow the link
}
```

a pak mu přiřaďte obslužné rutiny události:

```csharp
webView.ShouldStartLoad += HandleShouldStartLoad;
```

#### <a name="android"></a>Android

V systému Android jednoduše podtřídami WebViewClient a pak implementace kód pro požadavek na navigace.

```csharp
class HybridWebViewClient : WebViewClient {
    public override bool ShouldOverrideUrlLoading (WebView webView, string url) {
        // return true if handled in code
        // return false to let the web view follow the link
    }
}
```

a pak nastavte klienta ve webovém zobrazení:

```csharp
webView.SetWebViewClient (new HybridWebViewClient ());
```

### <a name="calling-javascript-from-c"></a>Volání jazyka Javascript z jazyka C#

Kromě informuje webové zobrazení načíst novou stránku HTML, C# – kód můžete spustit i Javascript v rámci aktuálně zobrazenou stránku. Celý bloky kódu Javascript lze vytvořit pomocí jazyka C# řetězce a spouštět, nebo může vytvořit volání metod pro jazyk Javascript, která je již k dispozici na stránce prostřednictvím `script` značky.

#### <a name="android"></a>Android

Vytvoření kódu jazyka Javascript provést, a pak ho pomocí předpony "javascript:" a vyzvat webové zobrazení načíst tento řetězec:

```csharp
var js = "alert('test');";
webView.LoadUrl ("javascript:" + js);
```

#### <a name="ios"></a>iOS

webové zobrazení iOS poskytovat metodu konkrétně k volání jazyka Javascript:

```csharp
var js = "alert('test');";
webView.EvaluateJavascript (js);
```

### <a name="summary"></a>Souhrn

V této části obsahuje zavedla funkce ovládací prvky webového zobrazení pro Android a iOS, která dejte nám sestavovat hybridní aplikace s Xamarinem, včetně:

-  Umožňuje načíst z řetězce vygenerované v kódu HTML
-  Možnost, chcete-li místní soubory (šablon stylů CSS, Javascript, obrázky a další soubory HTML),
-  Možnost za účelem zachycení požadavků navigace v kódu jazyka C#
-  Možnost Javascript volat z kódu jazyka C#.


V další části zavádí Razor, který umožňuje snadno vytvářet HTML k použití v hybridní aplikace.

## <a name="what-is-razor"></a>Co je Razor?

Syntaxe Razor je modul ukázka, která byla zavedená s architekturou ASP.NET MVC, původně ke spuštění na serveru a generují kód HTML ke zpracování do webových prohlížečů.

Ukázka modul Razor rozšiřuje standardní syntaxi HTML pomocí jazyka C#, aby mohli express rozložení a snadno začlenit šablony stylů CSS a Javascript. Šablony můžete odkazovat třídu modelu, které mohou být jakéhokoli typu vlastní a jejíž vlastnosti lze přistupovat přímo z šablony. Jeden z jeho hlavní výhody je schopnost snadno kombinovat syntaxe HTML a C#.

Šablon Razor nejsou omezeny na straně serveru pomocí, můžete je také zařadit do aplikace Xamarin. Pomocí šablon Razor spolu se schopností pro práci s webové zobrazení prostřednictvím kódu programu umožňuje sofistikované napříč platformami hybridní aplikace má být sestaven s funkcí Xamarin.

### <a name="razor-template-basics"></a>Základy šablony Razor

Soubory šablon Razor **.cshtml** příponu souboru. Mohou být přidány do projektu Xamarin v části textu ukázka **nový soubor** dialogové okno:

 ![Nový soubor – šablony Razor](images/image5_400x201.png)

Jednoduchou šablonu Razor ( **RazorView.cshtml**) jsou uvedeny níže.

```html
@model string
<html>
    <body>
    <h1>@Model</h1>
    </body>
</html>
```

Všimněte si následující rozdíly proti regulární soubor HTML:

-  `@` Symbol má zvláštní význam v rámci šablon Razor – znamená, že následující výraz jde C# k vyhodnocení.
- `@model` Direktiva vždy zobrazí jako první řádek souboru šablony Razor.
-  `@model` – Direktiva by měl následovat typu. V tomto příkladu je jednoduchý řetězec předávány do šablony, ale to může být vlastní třídy.
-  Když `@Model` se odkazuje v rámci šablony poskytuje odkaz na objekt předaný šablony, když se vygeneruje (v tomto příkladu je řetězec).
-  Prostředí IDE automaticky vygeneruje třídu pro šablony (soubory s **.cshtml** rozšíření). Tento kód můžete zobrazit, ale by neměla být upravována.
 ![RazorView.cshtml](images/image6_125x34.png) třídu jmenuje RazorView tak, aby odpovídaly .cshtml název souboru šablony. Je tento název, který slouží k odkazování na šabloně v kódu jazyka C#.
- `@using` příkazy může být také součástí v horní části Razor šablonu, která má obsahovat další obory názvů.


Finální výstup HTML lze vygenerovat pak pomocí následujícího kódu C#. Všimněte si, že určíme modelu, který má být řetězec "Hello, World", které se začlení do výstup vykreslené šablony.

```csharp
var template = new RazorView () { Model = "Hello World" };
var page = template.GenerateString ();
```

Toto je výstup zobrazí ve webovém zobrazení v systému iOS simulátoru a emulátoru Android:

 ![Hello World](images/image7_523x135.png)

### <a name="more-razor-syntax"></a>Další syntaxe Razor

V této části, které vytvoříme zavádět některé základní syntaxe Razor vám pomůže začít používat ho. Příklady v této části naplnit následující třídy s daty a zobrazit ji pomocí syntaxe Razor:

```csharp
public class Monkey {
    public string Name { get; set; }
    public DateTime Birthday { get; set; }
    public List<string> FavoriteFoods { get; set; }
}
```

Všechny příklady použít následující kód inicializace dat

```csharp
var animal = new Monkey {
    Name = "Rupert",
    Birthday=new DateTime(2011, 04, 01),
    FavoriteFoods = new List<string>
        {"Bananas", "Banana Split", "Banana Smoothie"}
};
```

#### <a name="displaying-model-properties"></a>Zobrazení vlastností modelu

Když je model třídu s vlastnostmi, že je možné snadno najít v šabloně Razor jak ukazuje tento příklad šablony:

```html
@model Monkey
<html>
    <body>
    <h1>@Model.Name</h1>
    <p>Birthday: @(Model.Birthday.ToString("d MMMM yyyy"))</p>
    </body>
</html>
```

To může být vykreslen na řetězec pomocí následujícího kódu:

```csharp
var template = new RazorView () { Model = animal };
var page = template.GenerateString ();
```

Finální výstup je tady uvedené v webové zobrazení v systému iOS simulátoru a emulátoru Android:

 ![Rupert](images/image8_516x160.png)

#### <a name="c-statements"></a>Příkazy jazyka C#

Složitější C# můžete zahrnout do šablony, například aktualizace vlastností modelu a výpočet stáří v tomto příkladu:

```html
@model Monkey
<html>
    <body>
    @{
        Model.Name = "Rupert X. Monkey";
        Model.Birthday = new DateTime(2011,3,1);
    }
    <h1>@Model.Name</h1>
    <p>Birthday: @Model.Birthday.ToString("d MMMM yyyy")</p>
    <p>Age: @(Math.Floor(DateTime.Now.Date.Subtract (Model.Birthday.Date).TotalDays/365)) years old</p>
    </body>
</html>
```

Můžete napsat složité jeden řádek C# výrazy (např. formátování stáří) tím, že kód s `@()`.

Vícenásobné příkazy jazyka C# lze zapsat tím, že je s `@{}`.

#### <a name="if-else-statements"></a>If-else – příkazy

Kód větví rozdíl lze vyjádřit pomocí `@if` jak je znázorněno v tomto příkladu šablony.

```html
@model Monkey
<html>
    <body>
    <h1>@Model.Name</h1>
    <p>Birthday: @(Model.Birthday.ToString("d MMMM yyyy"))</p>
    <p>Age: @(Math.Floor(DateTime.Now.Date.Subtract (Model.Birthday.Date).TotalDays/365)) years old</p>
    <p>Favorite Foods:</p>
    @if (Model.FavoriteFoods.Count == 0) {
        <p>No favorites</p>
    } else {
        <p>@Model.FavoriteFoods.Count favorites</p>
    }
    </body>
</html>
```

#### <a name="loops"></a>Smyčky

Ve smyčce konstrukce jako `foreach` lze také přidat. `@` Předponu lze použít na proměnnou smyčky ( `@food` v tomto případě) k vykreslení ve formátu HTML.

```html
@model Monkey
<html>
    <body>
    <h1>@Model.Name</h1>
    <p>Birthday: @Model.Birthday.ToString("d MMMM yyyy")</p>
    <p>Age: @(Math.Floor(DateTime.Now.Date.Subtract (Model.Birthday.Date).TotalDays/365)) years old</p>
    <p>Favorite Foods:</p>
    @if (Model.FavoriteFoods.Count == 0) {
        <p>No favorites</p>
    } else {
        <ul>
            @foreach (var food in @Model.FavoriteFoods) {
                <li>@food</li>
            }
        </ul>
    }
    </body>
</html>
```

Výstup výše uvedené šablony je zobrazena systémem iOS simulátoru a emulátoru Android:

 ![Rupert X opic](images/image9_520x277.png)

Tato část obsahuje zahrnutých základy používání šablon Razor k vykreslení jednoduché zobrazení jen pro čtení. V další části vysvětluje, jak vytvářet podrobnější aplikace pomocí syntaxe Razor, které se dají přijímají vstup uživatele a zajistit vzájemnou funkční spolupráci mezi Javascript v zobrazení HTML a C#.

## <a name="using-razor-templates-with-xamarin"></a>Pomocí šablon Razor pomocí Xamarinu

Tato část vysvětluje, jak používat sestavení hybridní aplikace pomocí šablony řešení v sadě Visual Studio for Mac. Nejsou k dispozici ze tří šablony **soubor > Nový > řešení...**  okno:

- **Android > aplikace > aplikace Android webového zobrazení**
- **iOS > aplikace > aplikace webového zobrazení**
- **Projektu ASP.NET MVC**



**Nové řešení** okno vypadat třeba takto pro iPhone a Android projekty – popis řešení na pravé straně označuje podporu pro modul ukázka Razor.

 ![Vytváření iPhone a řešení pro Android](images/image13_1139x959.png)

Všimněte si, že můžete snadno přidat **.cshtml** Razor šablona *žádné* existující projekt Xamarin, není potřeba použít tyto šablony řešení. iOS projekty nevyžadují scénáře použití syntaxe Razor buď; jednoduše přidání ovládacího prvku UIWebView všechna zobrazení prostřednictvím kódu programu a šablon Razor může vykreslit celý v kódu jazyka C#.

Níže jsou uvedeny výchozí šablony řešení obsah pro iPhone a Android projekty:

 ![iPhone a Android šablony](images/image10_428x310.png)

Šablony poskytují infrastrukturu aplikace připravené na cestu k načtení šablonu Razor s objekt modelu dat, zpracovat vstup uživatele a komunikovat s uživatele na základě jazyka Javascript.

Důležitou součástí řešení jsou:

-  Statický obsah, jako **style.css** souboru.
-  Soubory šablon Razor .cshtml jako **RazorView.cshtml** .
-  Model třídy, které se odkazuje v šablonách Razor, například **ExampleModel.cs** .
-  Třídy specifické pro platformu, která vytvoří zobrazení webové a vykreslí šablony, jako `MainActivity` v systému Android a `iPhoneHybridViewController` v systému iOS.


Následující část popisuje, jak fungují projektů.

### <a name="static-content"></a>Statický obsah

Statický obsah zahrnuje šablony stylů CSS, bitové kopie, soubory Javascript nebo jiný obsah, který může být odkazované z nebo odkazuje soubor HTML, který se zobrazí ve webovém zobrazení.

Šablony projektů zahrnují minimální šablony stylů na ukazují, jak ve vaší aplikaci hybridní zahrnout statický obsah. Šablony stylů CSS je odkazováno v šabloně takto:

```html
<link rel="stylesheet" href="style.css" />
```

Můžete přidat libovolnou šablony stylů a soubory Javascript budete potřebovat, včetně architektury, jako je JQuery.

### <a name="razor-cshtml-templates"></a>Syntaxe Razor cshtml šablony

Šablona obsahuje Razor **.cshtml** soubor, který byl předem zápis kódu ke komunikaci dat mezi HTML/Javascript a C#. To vám umožní sestavení sofistikované hybridní aplikace, které není právě zobrazení jen pro čtení dat z modelu, ale také přijímají vstup uživatele v kódu HTML a předejte ji zpět do kódu jazyka C# pro zpracování nebo úložiště.

#### <a name="rendering-the-template"></a>Vykreslování šablony

Volání `GenerateString` na šabloně vykreslí připravené pro zobrazení ve webovém zobrazení HTML. Pokud šablona používá model má být zadáno před vykreslování. Tento diagram znázorňuje, jak vykreslování funguje – není, webového zobrazení v době běhu pomocí zadané základní adresář najít zadané soubory se vyřeší statické prostředky.

 ![Vývojový diagram syntaxe Razor](images/image12_700x421.png)

#### <a name="calling-c-code-from-the-template"></a>Volání kódu jazyka C# ze šablony

Komunikace z vykreslené webové zobrazení zpětné volání jazyka C# se provádí nastavením adresu URL pro webové zobrazení, a pak brání žádosti v jazyce C# nativní žádost zpracovat. bez opětovného načtení webové zobrazení.

Příklad si můžete prohlédnout ve zpracování RazorView na tlačítko. Tlačítko má následující HTML:

```html
<input type="button" name="UpdateLabel" value="Click" onclick="InvokeCSharpWithFormValues(this)" />
```

`InvokeCSharpWithFormValues` Funkce Javascript, která čte všechny hodnoty z formuláře HTML a nastaví `location.href` pro webové zobrazení:

```javascript
location.href = "hybrid:" + elm.name + "?" + qs;
```

To se pokusí přejděte webové zobrazení na adresu URL pomocí vlastní schéma (např. `hybrid:`)

```
hybrid:UpdateLabel?textbox=SomeValue&UpdateLabel=Click
```

Nativní webové zobrazení zpracuje požadavek tato navigační, máme možnost zachytávat ho. V iOS to se provádí zpracování události UIWebView HandleShouldStartLoad. V systému Android můžeme jednoduše podtřídami WebViewClient použité ve formuláři a přepsání ShouldOverrideUrlLoading.

Interní položky tyto dvě sběrače navigace je v podstatě stejné.

Nejdřív zkontrolujte adresu URL, kterou webové zobrazení je pokus o načtení, a pokud nezačíná vlastní schéma (`hybrid:`), povolit navigační proběhnout jako normální.

Pro vlastní schéma adresy URL, vše, co je v adrese URL mezi schéma a "?" je název metody pro zpracování (v tomto případě "UpdateLabel"). Vše v řetězci dotazu jednal jako parametry pro volání metody:

```csharp
var resources = url.Substring(scheme.Length).Split('?');
var method = resources [0];
var parameters = System.Web.HttpUtility.ParseQueryString(resources[1]);
```

`UpdateLabel` v této ukázce nepodporuje minimální množství zacházení s řetězci v parametru textbox (předřazení "C# říká" řetězec) a pak zavolá zpátky do webové zobrazení.

Po zpracování adresy URL, metodu zruší navigaci, tak, aby webové zobrazení nebude pokoušet o navigaci na adresu URL vlastní dokončit.

#### <a name="manipulating-the-template-from-c"></a>Manipulace s šablony C#

Volání metody Javascript ve webovém zobrazení je potřeba komunikaci vykreslené webové zobrazení HTML z jazyka C#. V systému iOS, to se provádí volání `EvaluateJavascript` na UIWebView:

```csharp
webView.EvaluateJavascript (js);
```

V systému Android, Javascript může vyvolat ve webovém zobrazení Načítání Javascript jako adresu URL pomocí `"javascript:"` schéma adresy URL:

```csharp
webView.LoadUrl ("javascript:" + js);
```

## <a name="making-an-app-truly-hybrid"></a>Vytváření aplikace opravdu hybridní

Tyto šablony neprovádějte použití nativní ovládací prvky na jednotlivých platformách – celou obrazovku, naplní se jedné webové zobrazení.

Může být HTML ideální pro při vytváření prototypu a zobrazení druhy věcí webu je vhodné v například formátovaného textu a přizpůsobivé rozložení. Nicméně ne všechny úlohy jsou vhodné pro HTML a Javascript – posouvání prostřednictvím dlouhými seznamy dat, například provede lépe pomocí nativní ovládací prvky uživatelského rozhraní, jako je například (UITableView v systému iOS) nebo ListView v systému Android.

Webové zobrazení v šabloně se dají snadno rozšířit s ovládacími prvky specifické pro platformu – stačí upravit **MainStoryboard.storyboard** v Návrháři iOS nebo **Resources/layout/Main.axml** v systému Android.

### <a name="razortodo-sample"></a>Ukázka RazorTodo

[RazorTodo](https://github.com/xamarin/mobile-samples/tree/master/RazorTodo) úložiště obsahuje dvě samostatné řešení zobrazíte rozdíly mezi aplikací se úplně řízené HTML a aplikace, která kombinuje HTML s nativní ovládací prvky:

-  **RazorTodo** -řízené úplně HTML aplikace pomocí šablon Razor.
-  **RazorNativeTodo** – používá ovládací prvky zobrazení seznamu nativní pro iOS a Android, ale zobrazí se obrazovka upravit s HTML a syntaxe Razor.


Tyto aplikace Xamarin spustit na iOS a Android, přenosné knihovny tříd (PCLs) sdílet společný kód, jako jsou třídy databáze a modelu. Syntaxe Razor **.cshtml** šablony může být i součástí PCL, se snadno sdílet napříč platformami.

Obě ukázkových aplikací začlenit Twitter sdílení a je rozhraní API z nativní platforma demonstraci toho, že hybridní aplikace pomocí Xamarinu mít pořád přístup k základní funkce z řízené šablony zobrazení HTML Razor.

**RazorTodo** aplikace používá pro zobrazení seznamu a upravit šablony HTML Razor. To znamená, že jsme mohou vytvářet aplikace téměř úplně ve sdílené knihovny přenosných tříd (včetně databáze a **.cshtml** šablon Razor). Na následujících snímcích obrazovky zobrazit iOS a Android.

 ![RazorTodo](images/Both_700x290.png)

**RazorNativeTodo** aplikace používá pro zobrazení upravit šablonu HTML Razor, ale implementuje seznam nativní posouvání na každou platformu. To poskytuje řadu výhod, včetně:

-  Výkon – nativní posouvání ovládací prvky použijte virtualizaci zajistit rychlé, technologie smooth posouvání i s velmi dlouhý seznam data.
-  Nativní zkušenosti – prvky uživatelského rozhraní specifických pro platformy, se snadno povoleno, jako je podpora index fast posouvání v iOS a Android.


Hlavní výhoda vytváření hybridní aplikace pomocí Xamarinu je, můžete začít s úplně řízené HTML uživatelské rozhraní (např. první ukázka) a poté přidejte funkce specifické pro platformu vyžádání (jak je ukázáno v druhé ukázkové). Nativní seznamu obrazovky a HTML Razor upravit obrazovky na obou iOS a Android, které jsou uvedeny níže.

 ![RazorNativeTodo](images/BothNative_700x290.png)

## <a name="summary"></a>Souhrn

Tento článek obsahuje vysvětlení funkce ovládací prvky webového zobrazení k dispozici na iOS a Android, která usnadňují vytváření hybridní aplikace.

Potom popsané, modul Razor ukázka a syntaxe, které lze snadno generují kód HTML v pomocí aplikace Xamarin. **cshtml** soubory šablon Razor. Také popsané sady Visual Studio pro Mac šablony řešení, které vám umožní rychle začít vytvářet hybridní aplikace s funkcí Xamarin.

Nakonec zavedl RazorTodo vzorků, které ukazují, jak spojovat zobrazení web se nativní uživatelská rozhraní a rozhraní API.

### <a name="related-links"></a>Související odkazy

- [Ukázka RazorTodo](https://github.com/xamarin/mobile-samples/tree/master/RazorTodo)
- [MVC 3 – zobrazovací modul Razor (Microsoft)](http://www.asp.net/mvc/videos/mvc-3/mvc-3-razor-view-engine)
- [Úvod do rozhraní ASP.NET Web programování pomocí syntaxe Razor (Microsoft)](http://www.asp.net/web-pages/tutorials/basics/2-introduction-to-asp-net-web-programming-using-the-razor-syntax)
