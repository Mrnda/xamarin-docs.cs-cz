---
title: Práce s vlákny v Xamarin.iOS
description: Tento dokument popisuje, jak používat rozhraní API System.Threading aplikace pro Xamarin.iOS. Tento článek popisuje The Task Parallel Library, vytváření aplikací s rychlou odezvou a uvolňování paměti.
ms.prod: xamarin
ms.assetid: 50BCAF3B-1020-DDC1-0339-7028985AAC72
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 06/05/2017
ms.openlocfilehash: 8e4ee10fdabdcbb4c6cefe02b15dc93459708364
ms.sourcegitcommit: aa9b9b203ab4cd6a6b4fd51e27d865e2abf582c1
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/30/2018
ms.locfileid: "39350417"
---
# <a name="threading-in-xamarinios"></a>Práce s vlákny v Xamarin.iOS

Modul runtime Xamarin.iOS poskytuje vývojářům přístup k rozhraní .NET dělení na vlákna rozhraní API, explicitně při používání vláken (`System.Threading.Thread, System.Threading.ThreadPool`) a implicitně, při použití vzorů asynchronní delegáta nebo metody BeginXXX, stejně jako plnou škálu rozhraní API, které podporují Task Parallel Library.



Xamarin důrazně doporučuje, abyste použili [Task Parallel Library](http://msdn.microsoft.com/library/dd460717.aspx) (TPL) pro vytváření aplikací z několika důvodů:
-  Výchozím plánovačem TPL delegují provedení úlohy do fondu vláken, která zase se dynamicky zvýší počet vláken, které jsou potřeba, protože probíhá proces, při obcházení scénář, kde nakonec soutěží o ceny čas procesoru příliš mnoho vláken. 
-  Je snazší uvažovat o operací z hlediska TPL úlohy. Můžete snadno s nimi manipulovat, k jejich plánování, serializaci jejich spuštění nebo spuštění mnoho souběžně s bohatou sadu rozhraní API. 
-  Je základem pro programování v jazyce nového jazyka C# asynchronní jazyková rozšíření. 


Fondu vláken se pomalu zvýší počet vláken podle potřeby na základě počtu jader procesoru, které jsou k dispozici v systému, zatížení systému a požadavky vašich aplikací. Můžete použít tento fond vláken vyvoláním metody v `System.Threading.ThreadPool` nebo s použitím výchozí `System.Threading.Tasks.TaskScheduler` (součástí *paralelní architektury*).

Vývojáři obvykle použijte vlákna, pokud jsou nutné k vytváření aplikací s rychlou odezvou a není chcete blokovat spuštění smyčky hlavní uživatelské rozhraní.

 <a name="Developing_Responsive_Applications" />


## <a name="developing-responsive-applications"></a>Vývoj aplikací s rychlou odezvou

Přístup k elementům uživatelského rozhraní by měl být omezený na stejném vlákně, na kterém běží hlavní smyčka pro vaši aplikaci. Pokud chcete provést změny do hlavní uživatelské rozhraní z vlákna, by měl fronty kód pomocí [NSObject.InvokeOnMainThread](https://developer.xamarin.com/api/type/Foundation.NSObject/), tímto způsobem:

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

Výše uvedené volá kód uvnitř delegáta v souvislosti s hlavním vlákně, aniž by to způsobilo všechny konflikty časování, které potenciálně mohlo dojít k selhání aplikace.

 <a name="Threading_and_Garbage_Collection" />


## <a name="threading-and-garbage-collection"></a>Zřetězení a systém kolekce paměti

Při spuštění runtime Objective-C vytvoříte a uvolnit objekty. Pokud objekty jsou označeny "auto-vydání" modul runtime Objective-C vydá těchto objektů do vlákna na aktuální `NSAutoReleasePool`. Xamarin.iOS vytváří jeden `NSAutoRelease` pro každé vlákno z fondu `System.Threading.ThreadPool` a pro hlavní vlákno. Zahrnuje to při rozšíření i pro žádného vlákna, vytvoří pomocí výchozího TaskScheduler v System.Threading.Tasks.

Pokud vytvoříte vlastní vláken pomocí `System.Threading` je nutné zadat vlastníte `NSAutoRelease` fondu, aby se data, abyste zabránili úniku. Uděláte to tak, stačí zabalte vaše vlákno v následující část kódu:

```csharp
void MyThreadStart (object arg)
{
   using (var ns = new NSAutoReleasePool ()){
      // Your code goes here.
   }
}
```

Poznámka: Od verze Xamarin.iOS 5.2 není nutné poskytnout vlastní `NSAutoReleasePool` zobrazovat jako jedna, poskytneme vám automaticky za vás.


## <a name="related-links"></a>Související odkazy

- [Práce s vláknem uživatelského rozhraní](~/ios/user-interface/ios-ui/ui-thread.md)
