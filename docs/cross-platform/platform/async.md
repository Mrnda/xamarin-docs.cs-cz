---
title: "Async – přehled"
description: "Nejnovější verzi jazyka C# – verze 5 – zavedená dvě nová klíčová slova a express asynchronních operací: async a operátoru await. Tato klíčová slova umožňuje psát jednoduché kód, který využívá Task Parallel Library provádět dlouhotrvající operace (například přístup k síti) v jiné vlákno a snadný přístup k výsledky při dokončení. Nejnovější verze Xamarin.iOS a Xamarin.Android podporu async a operátoru await – tento dokument obsahuje vysvětlení a příklady pomocí nové syntaxe s funkcí Xamarin."
ms.topic: article
ms.prod: xamarin
ms.assetid: F87BF587-AB64-4C60-84B1-184CAE36ED65
ms.technology: xamarin-cross-platform
author: asb3993
ms.author: amburns
ms.date: 03/22/2017
ms.openlocfilehash: 3de0e09b15b704db5e67fbbee6ba9bac86f58557
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 02/27/2018
---
# <a name="async-support-overview"></a>Přehled asynchronních podpory

_Nejnovější verzi jazyka C# – verze 5 – zavedená dvě nová klíčová slova a express asynchronních operací: async a operátoru await. Tato klíčová slova umožňuje psát jednoduché kód, který využívá Task Parallel Library provádět dlouhotrvající operace (například přístup k síti) v jiné vlákno a snadný přístup k výsledky při dokončení. Nejnovější verze Xamarin.iOS a Xamarin.Android podporu async a operátoru await – tento dokument obsahuje vysvětlení a příklady pomocí nové syntaxe s funkcí Xamarin._

Asynchronní podpora pro Xamarin je založená na foundation Mono 3.0 a upgraduje profilem rozhraní API z se mobilní zařízení verzi Silverlight jako mobilní zařízení verzi rozhraní .NET 4.5.

## <a name="overview"></a>Přehled

Tento dokument zavádí nové async a operátoru await klíčová slova pak nevystavíte slabé stránky zabezpečení prostřednictvím jednoduché příklady implementace asynchronních metod v Xamarin.iOS a Xamarin.Android.

Podrobnější informace o nových asynchronní funkcí jazyka C# 5 (včetně spoustu ukázky a scénáře použití různých) naleznete v dokumentaci MSDN [asynchronní programování s Async a Await](http://msdn.microsoft.com/en-us/library/vstudio/hh191443.aspx).

Ukázkovou aplikaci Díky jednoduché asynchronní webové žádosti (bez blokování hlavní vlákno) a aktualizuje uživatelského rozhraní stažené html a počet znaků.

 [ ![](async-images/AsyncAwait_427x368.png "Ukázkové aplikace odešle požadavek jednoduchého asynchronní webového bez blokování hlavního vlákna pak aktualizuje uživatelského rozhraní stažené html a počet znaků")](async-images/AsyncAwait.png)

Asynchronní podpora pro Xamarin je založená na foundation Mono 3.0 a upgraduje profilem rozhraní API z se mobilní zařízení verzi Silverlight jako mobilní zařízení verzi rozhraní .NET 4.5.

## <a name="requirements"></a>Požadavky

Funkce jazyka C# 5 vyžadují Mono 3.0, který je součástí Xamarin.iOS 6.4 a Xamarin.Android 4.8. Zobrazí se výzva k upgradu vaší Mono, Xamarin.iOS, Xamarin.Android a Xamarin.Mac jej využít.

## <a name="using-async-amp-await"></a>Použití modifikátoru async &amp; operátoru await

 `async` a `await` jsou nové C# jazyka funkce, které spolupracují s Task Parallel Library snadno napsat zařazování kód k provedení dlouhotrvající úlohy bez blokování hlavního vlákna vaší aplikace.

## <a name="async"></a>async

### <a name="declaration"></a>Deklarace

`async` – Klíčové slovo je umístěn v deklaraci metody (nebo na lambda nebo anonymní metody) k označení, že obsahuje kód, který můžete spustit asynchronně, ie. nejsou blokovány volajícího přístup z více vláken.

Metody označené jako `async` by měla obsahovat alespoň jeden await výraz nebo příkaz. Pokud žádné `await`s nacházejí v metodě pak spustí synchronně (stejné jako kdyby nebyla žádná `async` modifikátor). Také způsobí to upozornění kompilátoru (ale ne k chybě).

### <a name="return-types"></a>Návratové typy

Asynchronní metody by měla vrátit `Task`, `Task<TResult>` nebo `void`.

Zadejte `Task` návratový typ, pokud metoda nevrátí jakoukoli jinou hodnotu.

Zadejte `Task<TResult>` Pokud metoda musí vrátit hodnotu, kde `TResult` je typ vracené (například `int`, například).

`void` Vrátí typ se používá hlavně pro obslužné rutiny událostí, které se vyžadují. Kód, který volá asynchronní metody, vrácení void nelze `await` na výsledek.

### <a name="parameters"></a>Parametry

Asynchronní metody nelze deklarovat `ref` nebo `out` parametry.

## <a name="await"></a>await

Operátor await lze použít k úloze uvnitř metody označené jako asynchronní. Ho způsobí, že metoda zastavit provádění v tomto bodě a počkejte na dokončení úlohy.

Pomocí await neblokuje volajícího přístup z více vláken – místo ovládací prvek je vrácen volajícímu. To znamená, zda není blokován volající vlákno, tak například uživatel nebude při čeká na úlohu zablokované rozhraní přístup z více vláken.

Po dokončení úlohy metoda obnoví provádění na stejném místě v kódu. To zahrnuje vrácením oboru zkuste bloku try-catch-finally – (Pokud je k dispozici). await nelze použít v catch nebo nakonec blokovat.

Další informace o [await na webu MSDN](http://msdn.microsoft.com/en-us/library/vstudio/hh156528.aspx).

## <a name="exception-handling"></a>Zpracování výjimek

Výjimky, ke kterým došlo v asynchronní metody jsou uložené v úloze a vyvolá, když je úloha `await`ed. Tyto výjimky můžete zachycení a zpracovává uvnitř bloku try-catch.

## <a name="cancellation"></a>Zrušení

Asynchronní metody, které trvat dlouhou dobu pro dokončení by měly podporovat zrušení. Zrušení se obvykle vyvolá následujícím způsobem:

- A `CancellationTokenSource` je vytvořen objekt.
- `CancellationTokenSource.Token` Instanci předávána zrušit asynchronní metody.
- Zrušení se požaduje při volání `CancellationTokenSource.Cancel` metoda.

Potom úlohu se zruší a uznává zrušení.

Další informace o zrušení najdete v tématu [postupy: zrušení asynchronní úlohy](http://msdn.microsoft.com/en-us/library/vstudio/jj155761.aspx) na webu MSDN.

## <a name="example"></a>Příklad

Stažení [příklad Xamarin řešení](https://developer.xamarin.com/samples/mobile/AsyncAwait/) (pro iOS a Android) zobrazíte pracovní příkladem `async` a `await` v mobilních aplikacích. Ukázkový kód je podrobněji popsána v této části.

### <a name="writing-an-async-method"></a>Zápis asynchronní metody

Metodu ukazuje, jak kód `async` metoda s `await`ed úloh:

```csharp
public async Task<int> DownloadHomepage()
{
    var httpClient = new HttpClient(); // Xamarin supports HttpClient!

    Task<string> contentsTask = httpClient.GetStringAsync("http://xamarin.com"); // async method!

    // await! control returns to the caller and the task continues to run on another thread
    string contents = await contentsTask;

    ResultEditText.Text += "DownloadHomepage method continues after async call. . . . .\n";

    // After contentTask completes, you can calculate the length of the string.
    int exampleInt = contents.Length;

    ResultEditText.Text += "Downloaded the html and found out the length.\n\n\n";

    ResultEditText.Text += contents; // just dump the entire HTML

    return exampleInt; // Task<TResult> returns an object of type TResult, in this case int
}
```

Všimněte si těchto bodů:

-  Obsahuje deklarace metody `async` – klíčové slovo.
-  Návratový typ `Task<int>` , můžete přístup k volání kódu `int` hodnotu, která se počítá v této metodě.
-  Příkaz return je `return exampleInt;` kterého je objekt, celé číslo – skutečnost, že metoda vrátí `Task<int>` je součástí vylepšení jazyk.


### <a name="calling-an-async-method-1"></a>Volání metody asynchronní 1

Toto tlačítko klikněte na událost obslužné rutiny lze nalézt v Android ukázkovou aplikaci k volání metody popsané výše:

```csharp
GetButton.Click += async (sender, e) => {

    Task<int> sizeTask = DownloadHomepage();

    ResultTextView.Text = "loading...";
    ResultEditText.Text = "loading...\n";

    // await! control returns to the caller
    var intResult = await sizeTask;

    // when the Task<int> returns, the value is available and we can display on the UI
    ResultTextView.Text = "Length: " + intResult ;
    // "returns" void, since it's an event handler
};
```

Poznámky:

-  Anonymní delegát má předponu async – klíčové slovo.
-  Asynchronní metoda DownloadHomepage vrátí úlohu, <int> který je uložený v sizeTask proměnné.
-  Kód čeká na proměnnou sizeTask.  *To* je umístění, které je pozastaven, metodu a řízení se vrátí do volající kód, dokud neskončí asynchronní úkol ve vlastním vlákně.
-  Provádění nemá *není* se zastaví, pokud bude úloha vytvořena na první řádek metody, bez ohledu na úlohu, vytváří existuje. Klíčové slovo await označuje umístění, kde je pozastaven provádění.
-  Po dokončení asynchronní úloha je nastavena zavřete a provádění pokračuje v původním vláknu, z řádku await.


### <a name="calling-an-async-method-2"></a>Volání metody asynchronní 2

V ukázkové aplikaci iOS v příkladu se k předvedení alternativní způsob zapíše trochu jinak. Spíše než použití delegáta anonymní tento příklad deklaruje `async` obslužné rutiny události, který je přiřazen jako obslužná rutina události regulární:

```csharp
GetButton.TouchUpInside += HandleTouchUpInside;
```

Obslužná rutina události je pak definováno, jak je vidět tady:

```csharp
async void HandleTouchUpInside (object sender, EventArgs e)
{
    ResultLabel.Text = "loading...";
    ResultTextView.Text = "loading...\n";

    // await! control returns to the caller
    var intResult = await DownloadHomepage();

    // when the Task<int> returns, the value is available and we can display on the UI
    ResultLabel.Text = "Length: " + intResult ;
}
```

Některé důležité body:

-  Metoda je označena jako `async` , ale vrací `void` . To se obvykle pouze provádí pro obslužné rutiny událostí (jinak by vrátit `Task` nebo `Task<TResult>` ).
-  Kód `await` s na `DownloadHomepage` metoda přímo na přiřazení proměnné ( `intResult` ) na rozdíl od předchozí příklad, kdy jsme použili středně pokročilé `Task<int>` proměnné tak, aby odkazovaly úlohu.  *To* je místo, kde ovládací prvek je vrácen volajícímu až po dokončení asynchronní metody na jiné vlákno.
-  Pokud lze asynchronní metodu dokončení a vrátí, provádění pokračuje v `await` což znamená, že výsledkem celé číslo se vrátí a potom zpracován widget uživatelského rozhraní.


## <a name="summary"></a>Souhrn

Použití modifikátoru async a operátoru await výrazně zjednodušuje je kód potřebný k spawn dlouhotrvající operace na vlákna na pozadí bez blokování hlavního vlákna. Budou také snadno přístup výsledků po dokončení úlohy.

Tento dokument poskytl přehled o nové klíčová slova jazyka a příklady pro Xamarin.iOS i Xamarin.Android.



## <a name="related-links"></a>Související odkazy

- [AsyncAwait (ukázka)](https://developer.xamarin.com/samples/mobile/AsyncAwait/)
- [Zpětná volání jako naše generace přejít na – příkaz](http://tirania.org/blog/archive/2013/Aug-15.html)
- [Data (iOS) (ukázka)](https://developer.xamarin.com/samples/monotouch/Data/)
- [HttpClient (iOS) (ukázka)](https://developer.xamarin.com/samples/monotouch/HttpClient/)
- [MapKitSearch (iOS) (ukázka)](https://github.com/xamarin/monotouch-samples/tree/master/MapKitSearch)
- [Webinář: C# asynchronní na iOS a Android (video)](http://xamarin.wistia.com/medias/k27mc627xz)
- [Asynchronní programování pomocí modifikátoru Async a operátoru Await (MSDN)](http://msdn.microsoft.com/en-us/library/vstudio/hh191443.aspx)
- [Jemnou ladění aplikace Async (MSDN)](http://msdn.microsoft.com/en-us/library/vstudio/jj155761.aspx)
- [Await a uživatelského rozhraní a blokování! Jejda Moje! (MSDN)](http://blogs.msdn.com/b/pfxteam/archive/2011/01/13/10115163.aspx)
- [Zpracování úloh po dokončení (MSDN)](http://blogs.msdn.com/b/pfxteam/archive/2012/08/02/processing-tasks-as-they-complete.aspx)
- [Asynchronní vzor založený na úlohách (TAP)](http://msdn.microsoft.com/en-us/library/hh873175.aspx)
- [Asynchrony v C# 5 (blog Erica Lippert) – o zavedení klíčová slova](http://blogs.msdn.com/b/ericlippert/archive/2010/11/11/whither-async.aspx)
