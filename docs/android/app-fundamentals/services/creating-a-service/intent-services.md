---
title: Záměrné služeb v Xamarin.Android
ms.prod: xamarin
ms.assetid: A5B86FE4-C8E2-4B0A-84CA-EF8F5119E31B
ms.technology: xamarin-android
author: topgenorth
ms.author: toopge
ms.date: 02/16/2018
ms.openlocfilehash: 80849213649707615f8bd8e941e1a51c6b54e76e
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/04/2018
---
# <a name="intent-services-in-xamarinandroid"></a>Záměrné služeb v Xamarin.Android

## <a name="intent-services-overview"></a>Přehled záměrné služby

Spustit i vázaný služby, spusťte na hlavní vlákno, což znamená, že pokud chcete zachovat výkon smooth, služba potřebuje pro práci se asynchronně. Jedním z nejjednodušší způsobů, jak tuto situaci řešit je s _pracovní fronty procesoru vzor_, kde je umístěn pracovní provést ve frontě, která je obsluhovány pomocí jedno vlákno. 

[ `IntentService` ](https://developer.xamarin.com/api/type/Android.App.IntentService/) Je podtřídou třídy `Service` třídu, která poskytuje Android konkrétní implementaci tohoto vzoru. Bude spravovat pracovní queueing, spuštění pracovní vlákno pro služby do fronty, a přijímání požadavků vypnout fronty se mají v pracovní vlákno. `IntentService` Tiše zastaví sám a odebrat pracovní vlákno, pokud není k dispozici žádné další práci ve frontě.
 
Pracovní je odeslána do fronty vytvořením `Intent` a následné předání, `Intent` k `StartService` metoda.

Není možné zastavit nebo přerušení `OnHandleIntent` metoda `IntentService` Pokud je v provozu. Vzhledem k tomuto chování `IntentService` by měly být udržovány bezstavové &ndash; neměli spoléhat na aktivní připojení nebo komunikaci od zbytku aplikace. `IntentService` Je určen statelessly zpracovávat požadavky práci.

Existují dva požadavky pro vytváření podtříd `IntentService`:

1. Nový typ (vytvořený vytváření podtříd `IntentService`) pouze přepsání `OnHandleIntent` metoda.
2. Konstruktor pro nový typ vyžaduje řetězce, který se používá k pojmenování pracovní vlákno, který bude zpracovávat žádosti. Název této pracovní vlákno se používá hlavně při ladění aplikace.

Následující kód ukazuje `IntentService` implementace s přepsané `OnHandleIntent` metoda:

```csharp
[Service]
public class DemoIntentService: IntentService
{
    public DemoIntentService () : base("DemoIntentService")
    {
    }
    
    protected override void OnHandleIntent (Android.Content.Intent intent)
    {
        Console.WriteLine ("perform some long running work");
        ...
        Console.WriteLine ("work complete");
    }
}
```

Posílá pracovní `IntentService` po vytvoření instance `Intent` a pak volání [ `StartService` ](https://developer.xamarin.com/api/member/Android.Content.Context.StartService/p/Android.Content.Intent/) metoda této záměrem jako parametr. Záměr se předají do služby jako parametr v `OnHandleIntent` metoda. Tento fragment kódu je příklad odesílání požadavku pracovní do záměrem: 

```csharp
// This code might be called from within an Activity, for example in an event
// handler for a button click.
Intent downloadIntent = new Intent(this, typeof(DemoIntentService));

// This is just one example of passing some values to an IntentService via the Intent:
downloadIntent.Put
("file_to_download", "http://www.somewhere.com/file/to/download.zip");

StartService(downloadIntent);
```

`IntentService` Můžete rozbalit hodnoty ze záměru, jak je předvedeno v tento fragment kódu:  

```csharp
protected override void OnHandleIntent (Android.Content.Intent intent)
{
    string fileToDownload = intent.GetStringExtra("file_to_download");
    
    Log.Debug("DemoIntentService", $"File to download: {fileToDownload}.");
}
```


## <a name="related-links"></a>Související odkazy

- [IntentService](https://developer.xamarin.com/api/type/Android.App.IntentService/)
- [StartService](https://developer.xamarin.com/api/member/Android.Content.Context.StartService/p/Android.Content.Intent/)
