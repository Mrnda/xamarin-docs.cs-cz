---
title: Souhrn kapitoly 20. Asynchronní a souborové I/O
description: 'Vytváření mobilních aplikací s Xamarin.Forms: Souhrn kapitoly 20. Asynchronní a souborové I/O'
ms.prod: xamarin
ms.technology: xamarin-forms
ms.assetid: D595862D-64FD-4C0D-B0AD-C1F440564247
author: charlespetzold
ms.author: chape
ms.date: 11/07/2017
ms.openlocfilehash: 2ff54b65b1dca9798c91f147da7e8482649e40d2
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/12/2018
ms.locfileid: "38996279"
---
# <a name="summary-of-chapter-20-async-and-file-io"></a>Souhrn kapitoly 20. Asynchronní a souborové I/O

 Grafické uživatelské rozhraní musí reagovat na události uživatelského vstupu postupně. Z toho vyplývá, veškeré zpracování událostí uživatelského vstupu, které se musí vyskytovat v jednom vlákně, často označované jako *hlavního vlákna* nebo *vlákno uživatelského rozhraní*.

Uživatelé očekávají, že grafické uživatelské rozhraní bude reagovat. To znamená, že program rychle musí zpracovávat události vstupu uživatele. Pokud to není možné, pak zpracování musí být předané centrům sekundární vlákna exekuce.

Několik ukázkové aplikace v této příručce používají [ `WebRequest` ](xref:System.Net.WebRequest) třídy. V této třídě [ `BeginGetReponse` ](xref:System.Net.WebRequest.BeginGetResponse(System.AsyncCallback,System.Object)) metoda začíná pracovní podproces, který volá funkci zpětného volání, jakmile se dokončí. Však spuštění této funkce zpětného volání v pracovní vlákno, takže program musí volat [ `Device.BeginInvokeOnMainThread` ](xref:Xamarin.Forms.Device.BeginInvokeOnMainThread(System.Action)) metody pro přístup k uživatelské rozhraní.

Více moderní přístup pro asynchronní zpracování je k dispozici v rozhraní .NET a C#. To zahrnuje [ `Task` ](xref:System.Threading.Tasks.Task) a [ `Task<TResult>` ](xref:System.Threading.Tasks.Task`1) třídami a ostatními typy v [ `System.Threading` ](xref:System.Threading) a [ `System.Threading.Tasks` ](xref:System.Threading.Tasks) obory názvů, a také 5.0 C# `async` a `await` klíčová slova. To je tato kapitola, která se zaměřuje na.

## <a name="from-callbacks-to-await"></a>Z zpětná volání k operátoru await

`Page` Třída obsahuje tři asynchronní metody k zobrazení polí výstrahy:

- [`DisplayAlert`](xref:Xamarin.Forms.Page.DisplayAlert(System.String,System.String,System.String)) Vrátí `Task` objektu
- [`DisplayAlert`](xref:Xamarin.Forms.Page.DisplayAlert(System.String,System.String,System.String,System.String)) Vrátí `Task<bool>` objektu
- [`DisplayActionSheet`](xref:Xamarin.Forms.Page.DisplayActionSheet(System.String,System.String,System.String,System.String[])) Vrátí `Task<string>` objektu

`Task` Objekty znamenat, že tyto metody implementovat založené na úlohách asynchronního vzoru označovaného jako TAP. Tyto `Task` objekty jsou rychle vrátil z metody. `Task<T>` Hodnoty představují "příslib", která vrátí hodnotu typu `TResult` budou dostupné po dokončení úlohy. `Task` Návratová hodnota Určuje asynchronní akce se bude vrátil dokončení ale bez hodnot.

V těchto případech `Task` je dokončená, když je uživatel nezavře pole výstrahy.  

### <a name="an-alert-with-callbacks"></a>Výstraha se zpětná volání

[ **AlertCallbacks** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter20/AlertCallbacks) Ukázka předvádí, jak zpracovat `Task<bool>` vrátí objekty a `Device.BeginInvokeOnMainThread` volání pomocí metod zpětného volání.

### <a name="an-alert-with-lambdas"></a>Výstraha s výrazy lambda

[ **AlertLambdas** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter20/AlertLambdas) Ukázka předvádí, jak používat anonymní lambda funkce pro zpracování `Task` a `Device.BeginInvokeOnMainThread` volání.  

### <a name="an-alert-with-await"></a>Výstraha se await

Jednodušší přístup zahrnuje `async` a `await` klíčová slova zavedená v jazyce C# 5. [ **AlertAwait** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter20/AlertAwait) ukázce jejich použití.

### <a name="an-alert-with-nothing"></a>Výstraha se nic

Pokud asynchronní metoda vrátí `Task` spíše než `Task<TResult>`, program nebude muset použít libovolnou z následujících postupů, pokud nepotřebuje vědět, až se dokončí asynchronní úlohu. [ **NothingAlert** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter20/NothingAlert) příklad ukazuje to.

### <a name="saving-program-settings-asynchronously"></a>Ukládá se nastavení programu asynchronně

[ **SaveProgramChanges** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter20/SaveProgramSettings) ukázka demonstruje použití [ `SavePropertiesAsync` ](xref:Xamarin.Forms.Application.SavePropertiesAsync) metoda `Application` uložte nastavení programu při změně bez přepsání `OnSleep` metody.

### <a name="a-platform-independent-timer"></a>Časovač nezávislá na platformě

Je možné použít [ `Task.Delay` ](xref:System.Threading.Tasks.Task.Delay(System.Int32)) vytvoření časovače nezávislá na platformě. [ **TaskDelayClock** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter20/TaskDelayClock) příklad ukazuje to.

## <a name="file-inputoutput"></a>Vstupní a výstupní soubor

Tradičně .NET [ `System.IO` ](xref:System.IO) obor názvů byl zdroj vstupu a výstupu souborů podpory. I když některé metody v tomto oboru názvů podporovat asynchronní operace, většina to nejde. Obor názvů podporuje také několik jednoduchý způsob volání, které provádějí funkce sofistikované souboru vstupně-výstupních operací.

### <a name="good-news-and-bad-news"></a>Dobrá zpráva a špatné

Všechny platformy podporuje místní úložiště aplikací Xamarin.Forms podporu &mdash; úložiště, které patří k aplikaci.

Knihovny Xamarin.iOS a Xamarin.Android obsahují verzi rozhraní .NET, která má Xamarin výslovně přizpůsobené u těchto dvou platforem. Mezi ně patří třídy z `System.IO` , můžete použít k provádění vstupně-výstupní soubor s místním úložištěm aplikace v těchto dvou platforem.

Nicméně pokud hledáte toto `System.IO` třídy v Xamarin.Forms PCL, které nenajdete je. Problém je tento soubor Microsoft zcela přepracované vstupně-výstupních operací pro rozhraní API architektury Windows Runtime. Programy, které cílí na Windows 8.1, Windows Phone 8.1 a univerzální platformu Windows nepoužívejte `System.IO` pro soubor vstupně-výstupních operací.

To znamená, že budete muset použít [ `DependencyService` ](xref:Xamarin.Forms.DependencyService) (nejprve podrobněji [ **kapitoly 9. Volání rozhraní API pro konkrétní platformu** ](chapter09.md) provádět vstupně-výstupní operace souboru.

### <a name="a-first-shot-at-cross-platform-file-io"></a>První snímek na vstupu a výstupu souborů napříč platformami

[ **TextFileTryout** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter20/TextFileTryout) definuje ukázková [ `IFileHelper` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter20/TextFileTryout/TextFileTryout/TextFileTryout/IFileHelper.cs) rozhraní pro vstup a výstup souborů a implementace tohoto rozhraní ve všech platformách. Ale implementace modulu Windows Runtime nefungují pomocí metod v tomto rozhraní, protože jsou asynchronní vstupně-výstupní metody souboru Windows Runtime.

### <a name="accommodating-windows-runtime-file-io"></a>Narážely vstup a výstup souborů Windows Runtime

Použít třídy v programy spuštěné v rámci modulu Windows Runtime [ `Windows.Storage` ](https://msdn.microsoft.com/library/windows/apps/windows.storage.aspx) a [ `Windows.Storage.Streams` ](https://msdn.microsoft.com/library/windows/apps/windows.storage.streams.aspx) obory názvů pro soubor vstupně-výstupních operací, včetně místní úložiště aplikací. Protože Microsoft určili, že všechny operace, které vyžadují více než 50 MS by měl být asynchronní k zabránění blokování vlákna uživatelského rozhraní, jsou většinou asynchronní tyto metody vstupně-výstupní operace souboru.

Ukázka tento nový přístup kódu bude v knihovně tak, aby ho můžete použít jiné aplikace.

## <a name="platform-specific-libraries"></a>Knihovny pro konkrétní platformu

Je výhodné pro uložení opakovaně použitelný kód v knihovnách. To je samozřejmě obtížnější, když jsou různý kód opakovaně použitelné pro zcela různé operační systémy.

[ **Xamarin.FormsBook.Platform** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Platform) řešení ukazuje jeden ze způsobů. Toto řešení obsahuje sedm různých projektech:

- [**Xamarin.FormsBook.Platform**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Platform/Xamarin.FormsBook.Platform), normální PCL Xamarin.Forms
- [**Xamarin.FormsBook.Platform.iOS**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Platform/Xamarin.FormsBook.Platform.iOS), knihovny tříd s iOS
- [**Xamarin.FormsBook.Platform.Android**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Platform/Xamarin.FormsBook.Platform.Android), knihovny tříd Androidu
- [**Xamarin.FormsBook.Platform.UWP**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Platform/Xamarin.FormsBook.Platform.UWP), knihovnu tříd Universal Windows
- [**Xamarin.FormsBook.Platform.Windows**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Platform/Xamarin.FormsBook.Platform.Windows), PCL pro Windows 8.1.
- [**Xamarin.FormsBook.Platform.WinPhone**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Platform/Xamarin.FormsBook.Platform.WinPhone), PCL pro Windows Phone 8.1
- [**Xamarin.FormsBook.Platform.WinRT**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Platform/Xamarin.FormsBook.Platform.WinRT), sdílený projekt pro kód, který je společné pro všechny platformy Windows

Všechny projekty jednotlivé platformy (s výjimkou produktů **Xamarin.FormsBook.Platform.WinRT**) mají odkazy na **Xamarin.FormsBook.Platform**. Tři projekty Windows obsahují odkaz na **Xamarin.FormsBook.Platform.WinRT**.

Všechny projekty obsahují statickou `Toolkit.Init` metodou, jak zajistit, že je načíst knihovnu, pokud to není přímo odkazuje projekt v řešení aplikace Xamarin.Forms.

**Xamarin.FormsBook.Platform** projekt obsahuje nové [ `IFileHelper` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Platform/Xamarin.FormsBook.Platform/IFileHelper.cs) rozhraní. Všechny metody teď mají názvy s `Async` přípony a vraťte se `Task` objekty.

**Xamarin.FormsBook.Platform.WinRT** projekt obsahuje [ `FileHelper` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Platform/Xamarin.FormsBook.Platform.WinRT/FileHelper.cs) třídu pro prostředí Windows Runtime.

**Xamarin.FormsBook.Platform.iOS** projekt obsahuje [ `FileHelper` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Platform/Xamarin.FormsBook.Platform.iOS/FileHelper.cs) třídy pro iOS. Tyto metody musí být nyní asynchronní. Některé metody použít asynchronní verze metod definovaných v `StreamWriter` a `StreamReader`: [ `WriteAsync` ](xref:System.IO.StreamWriter.WriteAsync(System.String)) a [ `ReadToEndAsync` ](xref:System.IO.StreamReader.ReadToEndAsync). Ostatní výsledek pro převedení `Task` pomocí [ `FromResult` ](xref:System.Threading.Tasks.Task.FromResult*) metody.

**Xamarin.FormsBook.Platform.Android** projekt obsahuje podobná [ `FileHelper` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Platform/Xamarin.FormsBook.Platform.Android/FileHelper.cs) třída pro Android.

**Xamarin.FormsBook.Platform** také obsahuje projekt [ `FileHelper` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Platform/Xamarin.FormsBook.Platform/FileHelper.cs) třídu, která usnadňuje použití nástroje `DependencyService` objektu.

Pokud chcete použít tyto knihovny, řešení pro aplikace musí zahrnovat všechny projekty v **Xamarin.FormsBook.Platform** řešení a všechny projekty aplikace musí mít odkaz na knihovnu odpovídající v  **Xamarin.FormsBook.Platform**.

[ **TextFileAsync** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter20/TextFileAsync) řešení ukazuje, jak používat **Xamarin.FormsBook.Platform** knihovny. Každý z projektů má volání `Toolkit.Init`. Aplikace využívá asynchronní vstupně funkce.

### <a name="keeping-it-in-the-background"></a>Zachování na pozadí

Metody v knihovnách, které provádět volání do více asynchronních metod &mdash; , jako `WriteFileAsync` a `ReadFileASync` metody v modulu Windows Runtime [ `FileHelper` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Platform/Xamarin.FormsBook.Platform.WinRT/FileHelper.cs) třídy &mdash; provádět mírně efektivnější pomocí [ `ConfigureAwait` ](xref:System.Threading.Tasks.Task`1.ConfigureAwait(System.Boolean)) metoda vyhnout přepnutí zpět do vlákna uživatelského rozhraní.

### <a name="dont-block-the-ui-thread"></a>Nedošlo k blokování vlákna uživatelského rozhraní.

Někdy je lákavé nepoužívejte `ContinueWith` nebo `await` pomocí [ `Result` ](xref:System.Threading.Tasks.Task`1.Result) vlastnost na metodu. Mělo by se vyhnout může zablokovat vlákno uživatelského rozhraní nebo dokonce zablokování aplikace.

## <a name="your-own-awaitable-methods"></a>Očekávatelný metody

Kód lze spouštět asynchronně pomocí předání do jednoho z [ `Task.Run` ](xref:System.Threading.Tasks.Task.Run(System.Action)) metody. Můžete volat `Task.Run` v rámci asynchronní metody, která zpracovává některé režijní náklady.

Různé `Task.Run` vzory jsou popsány níže.

### <a name="the-basic-mandelbrot-set"></a>Základní sada Mandelbrot

Chcete-li nakreslit Mandelbrot nastavit v reálném čase [ **Xamarin.Forms.Toolkit** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit) knihovna má [ `Complex` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/Complex.cs) strukturou podobnou té v `System.Numerics` obor názvů.

[ **MandelbrotSet** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter20/MandelbrotSet) ukázka má `CalculateMandeblotAsync` metody v souboru kódu na pozadí, který vypočítá základní sadu černobílé Mandelbrot a používá [ `BmpMaker` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/BmpMaker.cs)umístění na rastrový obrázek.

### <a name="marking-progress"></a>Označení průběh

Pro informace o průběhu z asynchronní metody můžete vytvořit instanci [ `Progress<T>` ](xref:System.Progress`1) třídy a definujte vaši asynchronní metoda může mít argument typu [ `IProgress<T>` ](xref:System.IProgress`1). To je patrné [ **MandelbrotProgress** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter20/MandelbrotProgress) vzorku.

### <a name="cancelling-the-job"></a>Probíhá zrušení úlohy

Můžete taky psát asynchronní metody na zrušit. Začněte s třídou s názvem [ `CancellationTokenSource` ](xref:System.Threading.CancellationTokenSource). [ `Token` ](xref:System.Threading.CancellationTokenSource.Token) Vlastnost je hodnota typu [ `CancellationToken` ](xref:System.Threading.CancellationToken). To je předáno asynchronní funkci. Program zavolá [ `Cancel` ](xref:System.Threading.CancellationTokenSource.Cancel) metoda `CancellationTokenSource` (obecně v reakci na akci uživatel) pro zrušení asynchronní funkce.

Asynchronní metoda může pravidelně kontrolovat [ `IsCancellationRequested` ](xref:System.Threading.CancellationToken.IsCancellationRequested) vlastnost `CancellationToken` a pokud je vlastnost `true`, nebo jednoduše zavoláte [ `ThrowIfCancellationRequested` ](xref:System.Threading.CancellationToken.ThrowIfCancellationRequested) metoda v Metoda takovém končí [ `OperationCancelledException` ](xref:System.OperationCanceledException).

[ **MandelbrotCancellation** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter20/MandelbrotCancellation) ukázka demonstruje použití zrušitelný funkce.

### <a name="an-mvvm-mandelbrot"></a>Mandelbrot modelem MVVM

[ **MandelbrotXF** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter20/MandelbrotXF) ukázka má rozsáhlejší uživatelské rozhraní a většinou je na základě [ `MandelbrotModel` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter20/MandelbrotXF/MandelbrotXF/MandelbrotXF/MandelbrotModel.cs) a [ `MandelbrotViewModel` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter20/MandelbrotXF/MandelbrotXF/MandelbrotXF/MandelbrotViewModel.cs)třídy:

[![Trojitá snímek Mandelbrot X F](images/ch20fg13-small.png "MVVM Mandelbrot")](images/ch20fg13-large.png#lightbox "MVVM Mandelbrot")

## <a name="back-to-the-web"></a>Zpět do webového rozhraní

[ `WebRequest` ](xref:System.Net.WebRequest) Třída používaná v některých vzorcích používá zastaralý asynchronní protokol volá asynchronní programovací Model nebo APM. Takové třídy lze převést na moderní klepnutím na protokol pomocí jedné z `FromAsync` metody v [ `TaskFactory` ](xref:System.Threading.Tasks.TaskFactory`1) třídy. [ **ApmToTap** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter20/ApmToTap) příklad ukazuje to.



## <a name="related-links"></a>Související odkazy

- [20 kapitoly textu v plném znění (PDF)](https://download.xamarin.com/developer/xamarin-forms-book/XamarinFormsBook-Ch20-Apr2016.pdf)
- [Ukázky kapitoly 20](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter20)
- [Práce se soubory](~/xamarin-forms/app-fundamentals/files.md)
