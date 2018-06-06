---
title: Dělení na vlákna v Xamarin.iOS
description: Tento dokument popisuje, jak používat rozhraní API System.Threading – aplikace pro Xamarin.iOS. Popisuje, knihovně Task Parallel Library, vytváření reaguje aplikace a uvolňování paměti.
ms.prod: xamarin
ms.assetid: 50BCAF3B-1020-DDC1-0339-7028985AAC72
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.openlocfilehash: 05d015d8d255ccc8c6230b1a89e098e187b22b37
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 06/05/2018
ms.locfileid: "34784914"
---
# <a name="threading-in-xamarinios"></a>Dělení na vlákna v Xamarin.iOS

Modul runtime Xamarin.iOS poskytuje vývojářům přístup k .NET dělení na vlákna rozhraní API, i explicitně při použití vláken (`System.Threading.Thread, System.Threading.ThreadPool`) a implicitně při použití vzory asynchronního delegáta nebo metody BeginXXX, jakož i úplné rozsahu z rozhraní API, která podporují Task Parallel Library.



Xamarin důrazně doporučuje používat [Task Parallel Library](http://msdn.microsoft.com/library/dd460717.aspx) (TPL) pro vytváření aplikací mít několik důvodů:
-  Plánovač TPL výchozí budou delegovat to provedení úlohy do fondu vláken, který naopak se dynamicky zvýší počet vláken, které jsou potřeba, protože probíhá proces, při vyloučení scénář, kde skončili neslučitelných pro doba využití procesoru příliš mnoho vláken. 
-  Je snazší vezměte v úvahu operace z hlediska TPL úlohy. Můžete snadno s nimi manipulovat, k jejich plánování, serializaci jejich spuštění nebo spuštění mnoho paralelně s bohatou sadu rozhraní API. 
-  Je základem pro programování s novou C# asynchronní jazyk rozšíření. 


Fond vláken se pomalu zvýší počet vláken, podle potřeby závislosti na počtu jader procesoru, které jsou k dispozici v systému, zatížení systému a požadavky vašich aplikací. Tento fond vláken můžete použít buď vyvoláním metody v `System.Threading.ThreadPool` nebo pomocí výchozí `System.Threading.Tasks.TaskScheduler` (součástí *paralelní rozhraní*).

Obvykle vývojáři použít vláken, když potřebují k vytvoření fungující aplikací a není chcete blokovat spuštění smyčky hlavní uživatelské rozhraní.

 <a name="Developing_Responsive_Applications" />


## <a name="developing-responsive-applications"></a>Vývoj aplikací reakce

Přístup k elementům uživatelského rozhraní musí být omezeno na stejném vlákně, v kterém běží hlavní smyčky pro vaši aplikaci. Pokud chcete změnit hlavní uživatelské rozhraní z vlákna, by měl fronty kód pomocí [NSObject.InvokeOnMainThread](https://developer.xamarin.com/api/type/Foundation.NSObject/), podobné výjimky:

```csharp
MyThreadedRoutine ()  
{  
    var result = DoComputation ();  

    // we want to update an object that is managed by the main
    // thread; To do so, we need to ensure that we only access
    // this from the main thread:

    InvokeOnMainThread (delegate {  
        label.Text = "The result is: " + result;  
    });
}
```

Výše vyvolá kód uvnitř delegát v kontextu hlavní vlákno, aniž by jakékoli časování, které může potenciálně dojít k chybě aplikace.

 <a name="Threading_and_Garbage_Collection" />


## <a name="threading-and-garbage-collection"></a>Zřetězení a systém kolekce paměti

V průběhu provádění jazyka Objective-C runtime vytvoření a uvolnění objektů. Pokud objekty jsou označeny "auto-verzi" runtime jazyka Objective-C vydá těchto objektů do je aktuální vlákno `NSAutoReleasePool`. Xamarin.iOS vytvoří jednu `NSAutoRelease` fond pro každé vlákno z `System.Threading.ThreadPool` a pro hlavní vlákno. Toto rozšíření se vztahuje žádné podprocesy vytvoří pomocí výchozího TaskScheduler v System.Threading.Tasks.

Pokud vytvoříte vlastní vláken pomocí `System.Threading` je nutné zadat, které vlastníte `NSAutoRelease` fondu zabránit úniku dat. K tomu jednoduše zabalit vašeho vlákna v následující část kódu:

```csharp
void MyThreadStart (object arg)
{
   using (var ns = new NSAutoReleasePool ()){
      // Your code goes here.
   }
}
```

Poznámka: Od verze Xamarin.iOS 5.2 nemáte poskytnout vlastní `NSAutoReleasePool` už jeden bude zadán automaticky za vás.


## <a name="related-links"></a>Související odkazy

- [Práce s vláknem uživatelského rozhraní](~/ios/user-interface/ios-ui/ui-thread.md)
