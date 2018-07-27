---
title: Vytváření zobrazení HTML pomocí šablon Razor
description: " Použití na webové stránce celé obrazovky k vykreslení HTML může být jednoduchý a efektivní způsob vykreslení složité formátování způsobem napříč platformami, zejména v případě, že už máte HTML, Javascript a šablon stylů CSS z projektu webu."
ms.prod: xamarin
ms.assetid: D8B87C4F-178E-48D9-BE43-85066C46F05C
author: asb3993
ms.author: amburns
ms.date: 07/24/2018
ms.openlocfilehash: 7e569aaddef912d9534e98f2f987ad5dfca8a5a6
ms.sourcegitcommit: 46bb04016d3c35d91ff434b38474e0cb8197961b
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/26/2018
ms.locfileid: "39270129"
---
# <a name="building-html-views-using-razor-templates"></a>Vytváření zobrazení HTML pomocí šablon Razor

Ve světě vývoje pro mobilní zařízení termín "hybridní aplikace" obvykle odkazuje na aplikaci, která představuje některé (nebo všechny) jeho obrazovky jako HTML stránky uvnitř ovládacího prvku hostované webového prohlížeče.

Existují některé vývojová prostředí, které umožňují sestavení mobilní aplikace kompletně v kódu HTML a Javascript, ale tato aplikace může trpět problémy s výkonem při pokusu o provedení komplexní zpracování nebo účinky uživatelského rozhraní a jsou také omezena na platformě Funkce, které mají přístup.

Xamarin nabízí to nejlepší z obou světů, zejména v případě, že využívá modul šablon Razor HTML. S využitím kódu Xamarin máte flexibilitu při sestavování multiplatformních šablony HTML zobrazení využívající jazyk Javascript a CSS, ale také mít úplný přístup k rozhraní API základní platformy a rychlé zpracování pomocí jazyka C#.

Tento dokument popisuje, jak používat syntaxi Razor modul šablon sestavení zobrazení HTML + Javascript + CSS, které lze použít v rámci mobilních platforem pomocí Xamarinu.

## <a name="using-web-views-programmatically"></a>Pomocí webového zobrazení prostřednictvím kódu programu

Předtím, než jsme Další informace o syntaxi Razor Tato část popisuje, jak pomocí webového zobrazení pro zobrazení obsahu HTML přímo – konkrétně obsah ve formátu HTML, který je vygenerován v rámci aplikace.

Xamarin poskytuje úplný přístup k rozhraní API základní platformy v Iosu a Androidu, tak, aby byl snadno vytvořit a zobrazit HTML pomocí jazyka C#. Základní syntaxe pro každou platformu najdete níž.

### <a name="ios"></a>iOS

Zobrazení HTML v ovládacím prvku UIWebView v Xamarin.iosu také trvá jenom pár řádků kódu:

```csharp
var webView = new UIWebView (View.Bounds);
View.AddSubview(webView);
string contentDirectoryPath = Path.Combine (NSBundle.MainBundle.BundlePath, "Content/");
var html = "<html><h1>Hello</h1><p>World</p></html>";
webView.LoadHtmlString(html, NSBundle.MainBundle.BundleUrl);
```

Zobrazit [iOS UIWebView](http://docs.xamarin.com/recipes/ios/content_controls/web_view/) recepty podrobné informace o použití ovládacího prvku UIWebView.

### <a name="android"></a>Android

Zobrazení HTML v ovládacím prvku WebView Xamarin.Android pomocí provádí pouhými několika řádky kódu:

```csharp
// webView is declared in an AXML layout file
var webView = FindViewById<WebView> (Resource.Id.webView);

// enable Javascript execution in your html view so you can provide "alerts" and other js
webView.SetWebChromeClient(new WebChromeClient());

var html = "<html><h1>Hello</h1><p>World</p></html>";
webView.LoadDataWithBaseURL("file:///android_asset/", html, "text/html", "UTF-8", null);
```

Zobrazit [Android WebView](http://docs.xamarin.com/recipes/android/controls/webview/) recepty podrobné informace o použití ovládacího prvku WebView.

### <a name="specifying-the-base-directory"></a>Zadání základního adresáře

Na obě platformy je parametr, který určuje základní adresář pro stránku HTML. Toto je umístění v systému souborů zařízení, který se používá k překladu relativní odkazy na zdroje, jako jsou obrázky a soubory šablon stylů CSS. Třeba jako značky

```html
<link rel="stylesheet" href="style.css" />
<img src="monkey.jpg" />
<script type="text/javascript" src="jscript.js">
```

Přečtěte si tyto soubory: **style.css**, **monkey.jpg** a **jscript.js**. Nastavení základní adresář určuje ve webovém zobrazení, kde jsou tyto soubory umístěny, mohou být načtena do stránky.

#### <a name="ios"></a>iOS

Výstup šablony se vykreslí v systému iOS pomocí následujícího kódu jazyka C#:

```csharp
webView.LoadHtmlString (page, NSBundle.MainBundle.BundleUrl);
```

Základní adresář je zadán jako `NSBundle.MainBundle.BundleUrl` odkazuje na adresář, který je aplikace nainstalovaná v. Všechny soubory v **prostředky** složce se zkopírují do tohoto umístění, jako **style.css** souboru je vidět tady:

 ![iPhoneHybrid řešení](images/image1_240x163.png)

Akce sestavení pro všechny statické soubory obsahu by měl být **BundleResource**:

 ![Akce sestavení projektu pro iOS: BundleResource](images/image2_250x131.png)

#### <a name="android"></a>Android

Android vyžaduje také základní adresář se mají předat jako parametr při zobrazení řetězcích html ve webovém zobrazení.

```csharp
webView.LoadDataWithBaseURL("file:///android_asset/", page, "text/html", "UTF-8", null);
```

Speciální řetězec **file:///android_asset/** odkazuje na složku Assetů Androidu ve vaší aplikaci, je znázorněno zde obsahující **style.css** souboru.

 ![AndroidHybrid řešení](images/image3_240x167.png)

Akce sestavení pro všechny statické soubory obsahu by měl být **AndroidAsset**.

 ![Akce sestavení projektu pro Android: AndroidAsset](images/image4_250x71.png)

### <a name="calling-c-from-html-and-javascript"></a>Volání jazyka C# z HTML a Javascript

Při načítání stránky html do webové zobrazení zpracovává odkazy a formuláře jako kdyby stránky byl načten ze serveru. To znamená, že když uživatel klikne na odkaz nebo odeslání formuláře ve webovém zobrazení se pokusí o přejděte do zadaného cíle.

Pokud je odkaz na externí server (například google.com) ve webovém zobrazení se pokusí načíst externí web (za předpokladu, že se připojení k Internetu).

```html
<a href="http://google.com/">Google</a>
```

Pokud je relativní odkaz ve webovém zobrazení se pokusí načíst tento obsah ze základního adresáře. Žádné síťové připojení je samozřejmě vyžaduje, aby to fungovalo, jak je uložen obsah v aplikaci na zařízení.

```html
<a href="somepage.html">Local content</a>
```

Akce formuláře pomocí stejného pravidla.

```html
<form method="get" action="http://google.com/"></form>
<form method="get" action="somepage.html"></form>
```

Nebudete k hostování webového serveru na klienta. ale můžete použít stejné techniky komunikaci serveru v dnešních responzivní návrhové vzory volat služby přes HTTP GET a zpracovávají odpovědi asynchronně ve výstupu JavaScriptu (nebo volání Javascript již hostována ve webovém zobrazení). To umožňuje snadno předat data z kódu HTML do kódu jazyka C# pro zpracování a zobrazit výsledky zpět na stránku HTML.

IOS a Androidu poskytují mechanismus pro kód aplikace, aby se zachytily tyto události navigace tak, aby kód aplikace může reagovat (v případě potřeby). Tato funkce je nezbytné k vytváření hybridních aplikací, protože umožňuje pracovat s ve webovém zobrazení nativního kódu.

#### <a name="ios"></a>iOS

ShouldStartLoad události ve webovém zobrazení v Iosu můžete být potlačena za účelem povolení aplikace kód pro zpracování požadavku navigace (například kliknutím na odkaz). Parametry metody zadejte všechny informace

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

V systému Android jednoduše podtřídy WebViewClient a potom implementovat kód reagovat na požadavek pro navigaci.

```csharp
class HybridWebViewClient : WebViewClient {
    public override bool ShouldOverrideUrlLoading (WebView webView, IWebResourceRequest request) {
        // return true if handled in code
        // return false to let the web view follow the link
    }
}
```

a pak nastavíte klienta ve webovém zobrazení:

```csharp
webView.SetWebViewClient (new HybridWebViewClient ());
```

### <a name="calling-javascript-from-c"></a>Volání JavaScriptu z jazyka C#

Kromě sděluje webové zobrazení k načtení nové stránky HTML, můžete kód jazyka C# také spustit jazyka Javascript v rámci aktuálně zobrazené stránky. Celý bloky kódu Javascript lze vytvořit pomocí jazyka C# řetězce a spustit, nebo můžete vytvářet volání metody Javascript, které jsou už k dispozici na stránce prostřednictvím `script` značky.

#### <a name="android"></a>Android

Vytvoření kódu jazyka Javascript provést a pak ho pomocí předpony "jazyka javascript:" a dáte pokyn, aby ve webovém zobrazení se načíst tento řetězec:

```csharp
var js = "alert('test');";
webView.LoadUrl ("javascript:" + js);
```

#### <a name="ios"></a>iOS

iOS webové zobrazení poskytují metody speciálně pro volání JavaScriptu:

```csharp
var js = "alert('test');";
webView.EvaluateJavascript (js);
```

### <a name="summary"></a>Souhrn

Tato část obsahuje přichází s funkcemi webové ovládací prvky zobrazení na Android a iOS, která nám vytvářet hybridní aplikace s využitím kódu Xamarin, včetně:

-  Umožňuje načíst z řetězce vygenerované v kódu HTML
-  Možnost odkazovat na místní soubory (šablon stylů CSS, Javascript, obrázky a další soubory HTML),
-  Umožňuje zachytávat žádosti o navigaci v kódu jazyka C#
-  Možnost volání JavaScriptu z kódu jazyka C#.


V další části se zavádí Razor, který usnadňuje vytváření kódu HTML k použití v hybridních aplikacích.

## <a name="what-is-razor"></a>Co je Razor?

Razor je modul šablon, které se zavedly s architekturou ASP.NET MVC, původně ke spuštění na serveru a generují kód HTML ke zpracování do webových prohlížečů.

Modul šablon Razor rozšiřuje standardní syntaxe kódu HTML pomocí jazyka C#, aby mohli express rozložení a snadno začlenit šablon stylů CSS a JavaScriptu. Šablony mohou odkazovat na třídu modelu, který může být libovolný vlastní typ a jejíž vlastnosti lze přistupovat přímo z šablony. Jeden z jeho hlavní výhody je možnost snadno kombinovat syntaxi jazyka HTML a C#.

Šablony Razor neomezují jenom na použití na straně serveru, může být také zahrnuté v aplikacích Xamarin. Použití šablony Razor spolu se schopností pracuje s webová zobrazení prostřednictvím kódu programu umožní sofistikované hybridní napříč platformami aplikací vytvořených pomocí Xamarinu.

### <a name="razor-template-basics"></a>Základy šablona Razor

Soubory šablony Razor **.cshtml** příponu souboru. Mohly být přidány do projektu Xamarin v části Šablonování textu **nový soubor** dialogové okno:

 ![Nový soubor – šablona Razor](images/image5_400x201.png)

Jednoduchá šablona Razor ( **RazorView.cshtml**) je uveden níže.

```html
@model string
<html>
    <body>
    <h1>@Model</h1>
    </body>
</html>
```

Všimněte si, že následující rozdíly proti regulárním souborem HTML:

-  `@` Symbol má zvláštní význam v šablony Razor – to znamená, že je na následující výraz jazyka C# k vyhodnocení.
- `@model` Direktiva se vždy zobrazí jako první řádek souboru šablony Razor.
-  `@model` – Direktiva by měl být následován znakem typu. V tomto příkladu je jednoduchý řetězec předávaný do šablony, ale to může být jakékoli vlastní třídy.
-  Když `@Model` se odkazuje v rámci šablony, poskytuje odkaz na objekt předaný do šablony generování (v tomto příkladu je řetězec).
-  Integrované vývojové prostředí bude automaticky generovat částečné třídy pro šablony (soubory s **.cshtml** rozšíření). Tento kód můžete zobrazit, ale by neměla být upravována.
 ![RazorView.cshtml](images/image6_125x34.png) částečné třídy názvem RazorView tak, aby odpovídaly .cshtml název souboru šablony. Je tento název, který se používá k odkazování na šablony v kódu jazyka C#.
- `@using` příkazy může být také součástí v horní části šablona Razor, který chcete zahrnout další obory názvů.


Finální výstup ve formátu HTML pak dá vygenerovat pomocí následujícího kódu jazyka C#. Všimněte si, že určíme modelu, který má být řetězec "Hello World", který bude součástí výstup vykresleného šablony.

```csharp
var template = new RazorView () { Model = "Hello World" };
var page = template.GenerateString ();
```

Zde je výstup zobrazí ve webovém zobrazení v systému iOS Simulator a emulátoru Androidu:

 ![Hello World](images/image7_523x135.png)

### <a name="more-razor-syntax"></a>Další syntaxe Razor

V této části, které chceme zavést některé základní syntaxe Razor až vám pomůžou začít používat ho. Příklady v této části vyplnit následující třídy s daty a zobrazit ji pomocí syntaxe Razor:

```csharp
public class Monkey {
    public string Name { get; set; }
    public DateTime Birthday { get; set; }
    public List<string> FavoriteFoods { get; set; }
}
```

Všechny příklady použijte následující kód inicializace dat

```csharp
var animal = new Monkey {
    Name = "Rupert",
    Birthday=new DateTime(2011, 04, 01),
    FavoriteFoods = new List<string>
        {"Bananas", "Banana Split", "Banana Smoothie"}
};
```

#### <a name="displaying-model-properties"></a>Zobrazení vlastností modelu

Když je model třídu s vlastnostmi, jejich jsou snadno odkazovat v šabloně Razor jak je znázorněno v tomto příkladu šablony:

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

Finální výstup je znázorněna zde ve webovém zobrazení v systému iOS Simulator a emulátoru Androidu:

 ![Rupert](images/image8_516x160.png)

#### <a name="c-statements"></a>Příkazy jazyka C#

Složitější C# mohou být součástí šablony, například vlastnosti aktualizace modelu nebo výpočet věku v tomto příkladu:

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

Můžete psát složité jedním řádkem výrazy jazyka C# (jako je formátování stáří) tím, že kód `@()`.

Vícenásobné příkazy jazyka C# lze zapsat tím, že je s `@{}`.

#### <a name="if-else-statements"></a>If-else – příkazy

Kód pro větvení lze vyjádřit pomocí `@if` jak je znázorněno v tomto příkladu šablony.

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

Opakování ve smyčce konstrukce, jako `foreach` lze také přidat. `@` Na proměnná smyčky for lze použít předponu ( `@food` v tomto případě) k vykreslení ve formátu HTML.

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

Výstup výše uvedené šablony se zobrazí pro iOS Simulator a emulátoru Androidu:

 ![Opic Rupert X](images/image9_520x277.png)

Tato část zahrnují základní informace o používání šablony Razor k vykreslení jednoduché zobrazení jen pro čtení. Další část popisuje postup vytvoření kompletní aplikace s využitím syntaxe Razor, které přijímají vstup uživatele a zajistit vzájemnou funkční spolupráci mezi jazyka Javascript v zobrazení HTML a C#.

## <a name="using-razor-templates-with-xamarin"></a>Pomocí šablony Razor s využitím kódu Xamarin

Tato část vysvětluje, jak použít sestavení hybridní aplikace pomocí šablony řešení v sadě Visual Studio pro Mac. Nejsou k dispozici tři šablony **soubor > Nový > řešení...**  okno:

- **Android > aplikace > aplikace WebView pro Android**
- **iOS > aplikace > aplikace WebView**
- **Projekt ASP.NET MVC**



**Nové řešení** okno vypadá takto pro iPhone a projekty Android – popis řešení na pravé straně zvýrazní podporu pro modul šablon Razor.

 ![Vytváření pro iPhone a Android řešení](images/image13_1139x959.png)

Všimněte si, že můžete snadno přidat **.cshtml** šablona Razor *jakékoli* existující projekt Xamarin, není nutné tyto šablony řešení používat. Scénáře použití Razor; buď nevyžadují žádné projekty iOS jednoduše přidejte ovládací prvek UIWebView všechna zobrazení prostřednictvím kódu programu a šablony Razor můžete zobrazit celý v kódu jazyka C#.

Výchozí šablony řešení obsah pro iPhone a Android projekty jsou uvedeny níže:

 ![iPhone a Android šablony](images/image10_428x310.png)

Šablony vám poskytnou infrastruktury aplikace připravené pro go k načtení šablony Razor s objektem modelu dat, zpracovat vstup uživatele a komunikovat s uživateli prostřednictvím JavaScriptu.

Důležité části řešení jsou:

-  Statický obsah, jako **style.css** souboru.
-  Soubory šablon Razor cshtml, jako jsou **RazorView.cshtml** .
-  Třídy, které jsou odkazovány v rámci šablon Razor, například modelů **ExampleModel.cs** .
-  Třídy specifické pro platformu, která vytváří webové zobrazení a vykreslí šablony, jako `MainActivity` v Androidu a `iPhoneHybridViewController` v systému iOS.


Následující část vysvětluje, jak pracují projektech.

### <a name="static-content"></a>Statický obsah

Šablony stylů CSS, obrázky, soubory jazyka Javascript nebo další obsah, který můžete odkazovat pomocí souboru HTML, který se zobrazí ve webovém zobrazení nebo odkazované z obsahuje statický obsah.

Šablony projektů zahrnují minimální stylů demonstrují, jakým způsobem chcete zahrnout statický obsah v hybridní aplikaci. Šablony stylů CSS je odkazováno v šabloně takto:

```html
<link rel="stylesheet" href="style.css" />
```

Můžete přidat libovolné šablony stylů a soubory jazyka Javascript, které potřebujete, včetně architektury, jako je JQuery.

### <a name="razor-cshtml-templates"></a>Razor cshtml šablony

Šablona obsahuje Razor **.cshtml** soubor, který obsahuje předem zapsané kódu, který pomáhá komunikovat data mezi HTML a JavaScriptu a C#. To vám umožní sestavení sofistikované hybridní aplikace, které není jen zobrazení jen pro čtení dat z modelu, ale také přijímají vstup uživatele v kódu HTML a předat ji zpět do kódu jazyka C# pro zpracování nebo uložení.

#### <a name="rendering-the-template"></a>Vykreslování šablony

Volání `GenerateString` na šabloně vykreslí připravené k zobrazení ve webovém zobrazení HTML. Pokud šablona používá model, pak by měl být zadán před vykreslování. Tento diagram znázorňuje, jak vykreslování funguje – Ne, statické prostředky jsou vyřešeny za běhu pomocí zadaného základního adresáře najít zadané soubory ve webovém zobrazení.

 ![Vývojový diagram Razor](images/image12_700x421.png)

#### <a name="calling-c-code-from-the-template"></a>Volání jazyka C# kódu ze šablony

Komunikace z vykreslené webové zobrazení zpětné volání jazyka C# se provádí nastavením adresu URL ve webovém zobrazení, a potom zachycení žádosti v jazyce C# nativní žádost zpracovat. bez opětovné načítání ve webovém zobrazení.

Příklad si můžete prohlédnout ve zpracování RazorView pro tlačítko. Tlačítko má následující kód HTML:

```html
<input type="button" name="UpdateLabel" value="Click" onclick="InvokeCSharpWithFormValues(this)" />
```

`InvokeCSharpWithFormValues` Funkce Javascript, která čte všechny hodnoty z formuláře HTML a nastaví `location.href` ve webovém zobrazení:

```javascript
location.href = "hybrid:" + elm.name + "?" + qs;
```

To se pokusí přejděte ve webovém zobrazení na adresu URL s vlastním schématem (např.) `hybrid:`)

```
hybrid:UpdateLabel?textbox=SomeValue&UpdateLabel=Click
```

Nativní webové zobrazení zpracuje požadavek pro navigaci, máme možnost zachytit. V Iosu to se provádí zpracování UIWebView HandleShouldStartLoad události. V Androidu můžeme jednoduše podtřídy WebViewClient použité ve formuláři a přepsat ShouldOverrideUrlLoading.

Interní informace o těchto dvou sběrače navigace je v podstatě stejné.

Nejdřív zkontrolujte adresu URL, kterou webové zobrazení se pokouší načíst, a pokud se nespustí s vlastním schématem (`hybrid:`), povolit navigaci na výskyt normálním způsobem.

Pro vlastní schéma adresy URL, všechno v adrese URL mezi schéma a "?" je název metody pro zpracování (v tomto případě "UpdateLabel"). Všechno, co v řetězci dotazu se zpracuje jako parametry pro volání metody:

```csharp
var resources = url.Substring(scheme.Length).Split('?');
var method = resources [0];
var parameters = System.Web.HttpUtility.ParseQueryString(resources[1]);
```

`UpdateLabel` v této ukázce neodpovídá minimální množství zacházení s řetězci v textovém poli parametru (předřazení "C# říká" na řetězec) a pak zavolá zpět do zobrazení pro web.

Po zpracování adresy URL, metoda zruší navigaci tak, aby ve webovém zobrazení nepokouší dokončit, že přejdete na vlastní adresu URL.

#### <a name="manipulating-the-template-from-c"></a>Manipulace s šablony z jazyka C#

Komunikace s vykreslené zobrazení web HTML z jazyka C# provádí volání Javascript ve webovém zobrazení. V systémech iOS, to se provádí voláním `EvaluateJavascript` na UIWebView:

```csharp
webView.EvaluateJavascript (js);
```

V systému Android, může být vyvolána Javascript ve webovém zobrazení tím, Javascript zde slouží jako adresu URL pomocí `"javascript:"` schéma adresy URL:

```csharp
webView.LoadUrl ("javascript:" + js);
```

## <a name="making-an-app-truly-hybrid"></a>Provádění aplikace skutečně hybridní

Neprovádějte tyto šablony využívají nativní ovládací prvky na jednotlivých platformách – celou obrazovku, naplní se jedné webové zobrazení.

HTML může být velmi vhodné pro vytváření prototypů a zobrazení různých věcí na webu je nejlépe například formátovaného textu a přizpůsobivé rozložení. Ale ne všechny úkoly jsou vhodné pro HTML a Javascript – procházení dlouhý seznam dat, například provádí lepší použití nativních ovládacích prvků uživatelského rozhraní, jako je například (UITableView v Iosu) nebo ListView v Androidu.

Webové zobrazení v šabloně se dají rozšířit snadno s ovládacími prvky pro konkrétní platformu – stačí upravit **MainStoryboard.storyboard** v návrháři pro iOS nebo **Resources/layout/Main.axml** v Androidu.

### <a name="razortodo-sample"></a>Ukázka RazorTodo

[RazorTodo](https://github.com/xamarin/mobile-samples/tree/master/RazorTodo) úložiště obsahuje dvě samostatná řešení Chcete-li zobrazit rozdíly mezi o zcela HTML řízenou aplikaci a aplikace, která kombinuje HTML s nativní ovládací prvky:

-  **RazorTodo** -driven zcela HTML aplikace pomocí šablony Razor.
-  **RazorNativeTodo** – používá ovládací prvky zobrazení seznamu nativní pro iOS a Android, ale zobrazí na obrazovce pro úpravy s jazykem HTML a syntaxe Razor.


Tyto aplikace Xamarin běží na iOS a Android pomocí přenosné knihovny tříd (PCLs) Chcete-li sdílet společný kód například databázi a namodelují třídy. Razor **.cshtml** šablony můžou být i součástí PCL tak snadno sdíleny napříč platformami.

Obě ukázkových aplikací začlenit sdílení na Twitteru a převod textu na řeč rozhraní API z nativních platformy představením toho, že hybridní aplikace s využitím kódu Xamarin mít dál přístup k základní funkce ze zobrazení řízené šablony HTML Razor.

**RazorTodo** aplikace používá pro zobrazení seznamu a upravit šablony HTML Razor. To znamená, že jsme aplikaci sestavit, téměř zcela do sdílené knihovny přenosných tříd (včetně databáze a **.cshtml** šablony Razor). Následujících snímcích obrazovky zobrazit aplikacích pro iOS a Android.

 ![RazorTodo](images/Both_700x290.png)

**RazorNativeTodo** aplikace používá pro zobrazení pro úpravy šablony HTML Razor, ale implementuje nativní posuvný seznam na jednotlivých platformách. To poskytuje řadu výhod, včetně následujících:

-  Výkon – nativní ovládací prvky umožňující posouvání pomocí virtualizace zajistit rychlé a plynulé posouvání i s velmi dlouhé seznamy data.
-  Nativní prostředí – prvky uživatelského rozhraní pro konkrétní platformu se snadno povolena, jako je například podpora index rychlé posouvání v iOS a Android.


Klíčovou výhodou vytváření hybridních aplikací s využitím kódu Xamarin je, že můžete začít s úplně řízené HTML uživatelského rozhraní (např. první příklad) a pak přidejte funkce specifické pro platformu v případě potřeby (jako u druhého se zobrazí vzorek). Nativní seznam obrazovek a HTML Razor úpravy obrazovky v obou iOS a Android jsou uvedeny níže.

 ![RazorNativeTodo](images/BothNative_700x290.png)

## <a name="summary"></a>Souhrn

Tento článek obsahuje vysvětlení funkce ovládacích prvků webového zobrazení k dispozici v Iosu a Androidu, která usnadňují vytváření hybridních aplikací.

Potom popsáno, modul šablon Razor a syntaxi, která lze snadno generují kód HTML v aplikacích Xamarin pro použití. **cshtml** soubory šablon Razor. Také popisuje sady Visual Studio pro Mac šablony řešení, které vám umožní rychle začít vytvářet hybridní aplikace s využitím kódu Xamarin.

Nakonec zavedené RazorTodo vzorků, které ukazují, jak kombinovat webové zobrazení nativní uživatelská rozhraní a rozhraní API.

### <a name="related-links"></a>Související odkazy

- [Ukázka RazorTodo](https://github.com/xamarin/mobile-samples/tree/master/RazorTodo)
- [MVC 3 – zobrazovací modul Razor (Microsoft)](http://www.asp.net/mvc/videos/mvc-3/mvc-3-razor-view-engine)
- [Úvod k programování v rozhraní ASP.NET Web používající syntaxi Razor (Microsoft)](http://www.asp.net/web-pages/tutorials/basics/2-introduction-to-asp-net-web-programming-using-the-razor-syntax)
