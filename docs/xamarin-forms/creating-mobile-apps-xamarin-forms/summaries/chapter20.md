---
title: "Souhrn kapitoly 20. Asynchronní a soubor vstupně-výstupních operací"
ms.topic: article
ms.prod: xamarin
ms.technology: xamarin-forms
ms.assetid: D595862D-64FD-4C0D-B0AD-C1F440564247
author: charlespetzold
ms.author: chape
ms.date: 11/07/2017
ms.openlocfilehash: 0ac316bc2cef04a80958c047427845dbdcc4137f
ms.sourcegitcommit: 8e722d72c5d1384889f70adb26c5675544897b1f
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 03/15/2018
---
# <a name="summary-of-chapter-20-async-and-file-io"></a>Souhrn kapitoly 20. Asynchronní a soubor vstupně-výstupních operací

 Grafické uživatelské rozhraní musí reagovat na vstup uživatele události postupně. To znamená, že veškeré zpracování uživatelského vstupu událostí, které se musí vyskytovat v jedním vláknem, často říká *hlavního vlákna* nebo *vlákna uživatelského rozhraní*.

Uživatelé očekávají, že jako odpovídající grafické uživatelské rozhraní. To znamená, že program rychle musí zpracovat události vstup uživatele. Pokud tento způsob není možný, pak zpracování musí být předané centrům sekundární vláken provádění.

Několik ukázkových programů v této příručce použili [ `WebRequest` ](https://developer.xamarin.com/api/type/System.Net.WebRequest/) třídy. V této třídě [ `BeginGetReponse` ](https://developer.xamarin.com/api/member/System.Net.WebRequest.BeginGetResponse/p/System.AsyncCallback/System.Object/) metoda spustí pracovní vlákno, které zavolá funkci zpětného volání při dokončení. Ale této funkce zpětného volání běží pracovní vlákno, takže program musí volat [ `Device.BeginInvokeOnMainThread` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Device.BeginInvokeOnMainThread/p/System.Action/) metoda pro přístup k uživatelské rozhraní.

Více moderní přístup k asynchronní zpracování je k dispozici v rozhraní .NET a C#. To zahrnuje [ `Task` ](https://developer.xamarin.com/api/type/System.Threading.Tasks.Task/) a [ `Task<TResult>` ](https://developer.xamarin.com/api/type/System.Threading.Tasks.Task%3CTResult%3E/) třídami a ostatními typy v [ `System.Threading` ](https://developer.xamarin.com/api/namespace/System.Threading/) a [ `System.Threading.Tasks` ](https://developer.xamarin.com/api/namespace/System.Threading.Tasks/) obory názvů, a také 5.0 C# `async` a `await` klíčová slova. Je tato kapitola, která se zaměřuje na.

## <a name="from-callbacks-to-await"></a>Z zpětná volání a operátoru await

`Page` Vlastní třídy obsahuje tři asynchronní metody pro zobrazení výstrah polí:

- [`DisplayAlert`](https://developer.xamarin.com/api/member/Xamarin.Forms.Page.DisplayAlert/p/System.String/System.String/System.String/) Vrátí `Task` objektu
- [`DisplayAlert`](https://developer.xamarin.com/api/member/Xamarin.Forms.Page.DisplayAlert/p/System.String/System.String/System.String/System.String/) Vrátí `Task<bool>` objektu
- [`DisplayActionSheet`](https://developer.xamarin.com/api/member/Xamarin.Forms.Page.DisplayActionSheet/p/System.String/System.String/System.String/System.String[]/) Vrátí `Task<string>` objektu

`Task` Objekty znamenat, že tyto metody implementovat úloh based Asynchronous Pattern označuje jako klepnutím. Tyto `Task` objekty jsou rychle vrátila z metody. `Task<T>` Hodnot tvoří "promise", která vrátí hodnotu typu `TResult` budou k dispozici po dokončení úlohy. `Task` Návratová hodnota určuje, že se vrátil dokončení ale bez hodnoty asynchronní akce.

V těchto případech `Task` je dokončená, když uživatel zavře pole výstrahy.  

### <a name="an-alert-with-callbacks"></a>Výstraha s zpětných volání

[ **AlertCallbacks** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter20/AlertCallbacks) příklad znázorňuje způsob zpracování `Task<bool>` vrátit objekty a `Device.BeginInvokeOnMainThread` volání pomocí metody zpětného volání.

### <a name="an-alert-with-lambdas"></a>Výstraha s lambdas

[ **AlertLambdas** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter20/AlertLambdas) příklad ukazuje, jak používat anonymní lambda funkce pro zpracování `Task` a `Device.BeginInvokeOnMainThread` volání.  

### <a name="an-alert-with-await"></a>Výstraha s await

Zahrnuje více Nejjednodušším způsobem `async` a `await` klíčová slova zavedená v C# 5. [ **AlertAwait** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter20/AlertAwait) příklad znázorňuje jejich použití.

### <a name="an-alert-with-nothing"></a>Výstraha se nic

Asynchronní metoda vrátí-li `Task` místo `Task<TResult>`, potom program nepotřebuje k používání některé z těchto postupů, pokud nepotřebuje vědět při dokončení asynchronní úlohu. [ **NothingAlert** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter20/NothingAlert) příklad znázorňuje to.

### <a name="saving-program-settings-asynchronously"></a>Ukládá se nastavení programu asynchronně

[ **SaveProgramChanges** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter20/SaveProgramSettings) příklad ukazuje použití [ `SavePropertiesAsync` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Application.SavePropertiesAsync()/) metodu `Application` se uložit nastavení programu, které se změnit bez přepsání `OnSleep` metoda.

### <a name="a-platform-independent-timer"></a>Časovač nezávislé na platformě

Je možné použít [ `Task.Delay` ](https://developer.xamarin.com/api/member/System.Threading.Tasks.Task.Delay/p/System.Int32/) vytvořit časovač nezávislé na platformě. [ **TaskDelayClock** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter20/TaskDelayClock) příklad znázorňuje to.

## <a name="file-inputoutput"></a>Vstupní a výstupní soubor

Obvyklým .NET [ `System.IO` ](https://developer.xamarin.com/api/namespace/System.IO/) obor názvů byl zdroj podpora souborů vstupně-výstupní operace. I když některé metody v tomto oboru názvů podporovat asynchronní operace, většina nepodporují. Obor názvů také podporuje několik jednoduchý způsob volání, která provádějí vstupně-výstupních operací funkce sofistikované souboru.

### <a name="good-news-and-bad-news"></a>Dobrá zpráva a chybný zprávy

Všechny platformy nepodporuje místní úložiště Xamarin.Forms podporu aplikací &mdash; úložiště, které je privátní k aplikaci.

Knihovny Xamarin.iOS a Xamarin.Android zahrnují verzi rozhraní .NET, která má Xamarin výslovně přizpůsobit pro těchto dvou platforem. Patří mezi ně třídy z `System.IO` můžete provádět vstupně-výstupní soubor s místním úložištěm aplikace v těchto dvou platforem.

Ale pokud hledáte tyto `System.IO` třídy v Xamarin.Forms PCL, nebude možné najít je. Problém je, že soubor Microsoft zcela renovována vstupně-výstupních operací pro rozhraní API systému Windows Runtime. Programy, které jsou cílené na Windows 8.1, Windows Phone 8.1 a univerzální platformu Windows nepoužívejte `System.IO` pro soubor vstupně-výstupní operace.

To znamená, že budete muset použít [ `DependencyService` ](https://developer.xamarin.com/api/type/Xamarin.Forms.DependencyService/) (nejprve popsané v [ **kapitoly 9. Volání rozhraní API specifické pro platformu** ](chapter09.md) implementovat vstupně-výstupní soubor.

### <a name="a-first-shot-at-cross-platform-file-io"></a>První snímek v souboru napříč platformami vstupně-výstupních operací

[ **TextFileTryout** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter20/TextFileTryout) definuje ukázková [ `IFileHelper` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter20/TextFileTryout/TextFileTryout/TextFileTryout/IFileHelper.cs) rozhraní pro vstupně-výstupní soubor a implementace tohoto rozhraní ve všech platformách. Implementace prostředí Windows Runtime však nepodporují pomocí metod v tomto rozhraní vzhledem k tomu, že jsou asynchronní metody prostředí Windows Runtime souboru vstupně-výstupní operace.

### <a name="accommodating-windows-runtime-file-io"></a>Možnost využití vstupně-výstupní soubor prostředí Windows Runtime

Aplikace běžící v rámci prostředí Windows Runtime použít třídy v [ `Windows.Storage` ](https://msdn.microsoft.com/library/windows/apps/windows.storage.aspx) a [ `Windows.Storage.Streams` ](https://msdn.microsoft.com/library/windows/apps/windows.storage.streams.aspx) obory názvů pro soubor vstupně-výstupní operace, včetně aplikace místní úložiště. Protože Microsoft určili, že všechny operace, které vyžadují více než 50 milisekundách musí být asynchronní k zabránění blokování vlákna uživatelského rozhraní, jsou většinou asynchronní tyto metody vstupně-výstupní soubor.

Kód demonstraci tento nový přístup bude v knihovně, aby se může použít jiné aplikace.

## <a name="platform-specific-libraries"></a>Specifické pro platformu knihovny

Je výhodné opakovaně použitelný kód uložit do knihovny. To je samozřejmě obtížnější, když jsou různé části opakovaně použitelný kód pro úplně jinou operační systémy.

[ **Xamarin.FormsBook.Platform** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Platform) řešení ukazuje jeden ze způsobů. Toto řešení obsahuje sedm různých projektů:

- [**Xamarin.FormsBook.Platform**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Platform/Xamarin.FormsBook.Platform), normální PCL Xamarin.Forms
- [**Xamarin.FormsBook.Platform.iOS**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Platform/Xamarin.FormsBook.Platform.iOS), knihovny tříd s iOS
- [**Xamarin.FormsBook.Platform.Android**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Platform/Xamarin.FormsBook.Platform.Android), knihovnu aplikace Android – třída
- [**Xamarin.FormsBook.Platform.UWP**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Platform/Xamarin.FormsBook.Platform.UWP), a Universal Windows class library
- [**Xamarin.FormsBook.Platform.Windows**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Platform/Xamarin.FormsBook.Platform.Windows), a PCL for Windows 8.1.
- [**Xamarin.FormsBook.Platform.WinPhone**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Platform/Xamarin.FormsBook.Platform.WinPhone), PCL pro Windows Phone 8.1
- [**Xamarin.FormsBook.Platform.WinRT**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Platform/Xamarin.FormsBook.Platform.WinRT), sdílený projekt pro kód, který je společný pro všechny platformy systému Windows

Všechny projekty jednotlivé platformy (s výjimkou produktů **Xamarin.FormsBook.Platform.WinRT**) odkazující na **Xamarin.FormsBook.Platform**. Tři projekty Windows obsahují odkaz na **Xamarin.FormsBook.Platform.WinRT**.

Všechny projekty obsahují statického `Toolkit.Init` metodou, jak zajistit, že knihovny je načtena, pokud ji není přímo odkazuje na projekt v řešení Xamarin.Forms aplikace.

**Xamarin.FormsBook.Platform** projekt obsahuje nové [ `IFileHelper` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Platform/Xamarin.FormsBook.Platform/IFileHelper.cs) rozhraní. Všechny metody nyní mají názvy s `Async` přípony a vraťte se `Task` objekty.

**Xamarin.FormsBook.Platform.WinRT** projekt obsahuje [ `FileHelper` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Platform/Xamarin.FormsBook.Platform.WinRT/FileHelper.cs) třídu pro prostředí Windows Runtime.

**Xamarin.FormsBook.Platform.iOS** projekt obsahuje [ `FileHelper` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Platform/Xamarin.FormsBook.Platform.iOS/FileHelper.cs) třídu pro iOS. Tyto metody musí být nyní asynchronní. Některé metody použít asynchronní verze metody definované v `StreamWriter` a `StreamReader`: [ `WriteAsync` ](https://developer.xamarin.com/api/member/System.IO.StreamWriter.WriteAsync/p/System.String/) a [ `ReadToEndAsync` ](https://developer.xamarin.com/api/member/System.IO.StreamReader.ReadToEndAsync()/). Ostatní převést výsledek, který má `Task` pomocí [ `FromResult` ](https://developer.xamarin.com/api/member/System.Threading.Tasks.Task.FromResult%7BTResult%7D/p/TResult/) metoda.

**Xamarin.FormsBook.Platform.Android** projekt obsahuje podobná [ `FileHelper` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Platform/Xamarin.FormsBook.Platform.Android/FileHelper.cs) třídu pro Android.

**Xamarin.FormsBook.Platform** projekt obsahuje také [ `FileHelper` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Platform/Xamarin.FormsBook.Platform/FileHelper.cs) třídu, která usnadňuje použití `DependencyService` objektu.

Pokud chcete použít tyto knihovny, řešení aplikace musí obsahovat všechny projekty v **Xamarin.FormsBook.Platform** řešení a všechny projekty aplikací musí mít odkaz na knihovnu odpovídající v ** Xamarin.FormsBook.Platform**.

[ **TextFileAsync** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter20/TextFileAsync) řešení ukazuje, jak používat **Xamarin.FormsBook.Platform** knihovny. Všechny projekty má volání `Toolkit.Init`. Aplikace využívá asynchronní vstupně funkce.

### <a name="keeping-it-in-the-background"></a>Zachování na pozadí

Metody v knihovny, které provádět volání do více asynchronních metod &mdash; , jako `WriteFileAsync` a `ReadFileASync` metody v prostředí Windows Runtime [ `FileHelper` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Platform/Xamarin.FormsBook.Platform.WinRT/FileHelper.cs) třída &mdash; můžete provedeny poněkud efektivnější pomocí [ `ConfigureAwait` ](https://developer.xamarin.com/api/member/System.Threading.Tasks.Task%3CTResult%3E.ConfigureAwait/p/System.Boolean/) metoda zrušení přepnutím zpět vlákna uživatelského rozhraní.

### <a name="dont-block-the-ui-thread"></a>Nedošlo k blokování vlákna uživatelského rozhraní!

Někdy je tempting nepoužívejte `ContinueWith` nebo `await` pomocí [ `Result` ](https://developer.xamarin.com/api/property/System.Threading.Tasks.Task%3CTResult%3E.Result/) vlastnosti na metodu. To je nutno pro můžete blokovat vlákna uživatelského rozhraní nebo i zablokování aplikace.

## <a name="your-own-awaitable-methods"></a>Vlastní může používat await metody

Můžete spouštět nějaký kód asynchronně předáním do jednoho z [ `Task.Run` ](https://developer.xamarin.com/api/member/System.Threading.Tasks.Task.Run/p/System.Action/) metody. Můžete volat `Task.Run` v rámci asynchronní metody, která zpracovává některé režijní náklady.

Různé `Task.Run` vzory jsou popsané dále.

### <a name="the-basic-mandelbrot-set"></a>Základní sady Mandelbrot

Kreslení Mandelbrot nastavit v reálném čase [ **Xamarin.Forms.Toolkit** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit) knihovna má [ `Complex` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/Complex.cs) struktura podobná v `System.Numerics` obor názvů.

[ **MandelbrotSet** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter20/MandelbrotSet) ukázka má `CalculateMandeblotAsync` metoda ve svém kódu souboru, který vypočítá základní černobílý sadu Mandelbrot a používá [ `BmpMaker` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/BmpMaker.cs)se umístí na rastrový obrázek.

### <a name="marking-progress"></a>Označení průběh

Sestava pokroku z asynchronní metodu můžete vytvořit instanci [ `Progress<T>` ](https://developer.xamarin.com/api/type/System.Progress%3CT%3E/) třídy a definovat asynchronní metodu tak, aby měl argument typu [ `IProgress<T>` ](https://developer.xamarin.com/api/type/System.IProgress%3CT%3E/). Tento postup je znázorněn v [ **MandelbrotProgress** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter20/MandelbrotProgress) ukázka.

### <a name="cancelling-the-job"></a>Probíhá zrušení úlohy

Je také možné zapsat asynchronní metodu být zrušit. Začínat třídy s názvem [ `CancellationTokenSource` ](https://developer.xamarin.com/api/type/System.Threading.CancellationTokenSource/). [ `Token` ](https://developer.xamarin.com/api/property/System.Threading.CancellationTokenSource.Token/) Vlastnost je hodnota typu [ `CancellationToken` ](https://developer.xamarin.com/api/type/System.Threading.CancellationToken/). To je předán asynchronní funkce. Program zavolá [ `Cancel` ](https://developer.xamarin.com/api/member/System.Threading.CancellationTokenSource.Cancel()/) metodu `CancellationTokenSource` (obvykle v odpovědi na akce uživatelem) Chcete-li zrušit asynchronní funkce.

Asynchronní metoda může pravidelně kontrolovat, [ `IsCancellationRequested` ](https://developer.xamarin.com/api/property/System.Threading.CancellationToken.IsCancellationRequested/) vlastnost `CancellationToken` a ukončení, pokud je vlastnost `true`, stačí zavolat [ `ThrowIfCancellationRequested` ](https://developer.xamarin.com/api/member/System.Threading.CancellationToken.ThrowIfCancellationRequested()/) metoda v takovém případě metodu končí [ `OperationCancelledException` ](https://developer.xamarin.com/api/type/System.OperationCanceledException/).

[ **MandelbrotCancellation** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter20/MandelbrotCancellation) příklad ukazuje použití zrušit funkce.

### <a name="an-mvvm-mandelbrot"></a>Mandelbrot modelem MVVM

[ **MandelbrotXF** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter20/MandelbrotXF) ukázka má rozsáhlejší uživatelské rozhraní a většinou je založena na [ `MandelbrotModel` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter20/MandelbrotXF/MandelbrotXF/MandelbrotXF/MandelbrotModel.cs) a [ `MandelbrotViewModel` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter20/MandelbrotXF/MandelbrotXF/MandelbrotXF/MandelbrotViewModel.cs)třídy:

[![Trojitá snímek obrazovky Mandelbrot X F](images/ch20fg13-small.png "rozhraní MVVM Mandelbrot")](images/ch20fg13-large.png#lightbox "Mandelbrot rozhraní MVVM")

## <a name="back-to-the-web"></a>Zpět na webu

[ `WebRequest` ](https://developer.xamarin.com/api/type/System.Net.WebRequest/) Třída používaná v některé ukázky používá protokol stejné asynchronní označovaný jako asynchronní programovací Model nebo APM. Tyto třídy můžete převést moderní klepněte na protokol pomocí jedné z `FromAsync` metody v [ `TaskFactory` ](https://developer.xamarin.com/api/type/System.Threading.Tasks.TaskFactory%3CTResult%3E/) třídy. [ **ApmToTap** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter20/ApmToTap) příklad znázorňuje to.



## <a name="related-links"></a>Související odkazy

- [Úplný text 20 kapitoly (PDF)](https://download.xamarin.com/developer/xamarin-forms-book/XamarinFormsBook-Ch20-Apr2016.pdf)
- [Ukázky kapitoly 20](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter20)
- [Práce se soubory](~/xamarin-forms/app-fundamentals/files.md)
