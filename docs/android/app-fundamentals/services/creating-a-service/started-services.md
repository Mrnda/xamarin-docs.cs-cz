---
title: Spuštěné služby s Xamarin.Android
ms.prod: xamarin
ms.assetid: 8CC3A850-4CD2-4F93-98EE-AF3470794000
ms.technology: xamarin-android
author: topgenorth
ms.author: toopge
ms.date: 02/16/2018
ms.openlocfilehash: c0aeeaad3798dd840e69c6da6d7857298f4da3c1
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/04/2018
ms.locfileid: "30767463"
---
# <a name="started-services-with-xamarinandroid"></a>Spuštěné služby s Xamarin.Android

## <a name="started-services-overview"></a>Přehled služby Začínáme

Začínáme služby obvykle provádět pracovní jednotka bez zadání žádné přímé zpětnou vazbu nebo výsledky do klienta. Příklad jednotky práce je služba, která odesílá soubor na server. Klient bude žádost o služby k nahrání souboru ze zařízení na web. Služba bude tiše nahrát soubor (i v případě, že aplikace nemá žádné aktivity v popředí) a ukončete sám po dokončení nahrávání. Je důležité si uvědomit, že spuštěna služba bude spuštěna ve vlákně UI aplikace. To znamená, že pokud služba je pro práci, která bude blokovat vlákna uživatelského rozhraní, musí vytvořit a vyřazení vláken podle potřeby.

Na rozdíl od vázané služby není k dispozici žádné komunikační kanál mezi "čistý" Začínáme služby a jeho klienty. To znamená, že bude spuštěna služba implementovat některé metody životního cyklu jiný než vázané služba. V následujícím seznamu jsou uvedeny běžné metody životní cyklus v spuštěná služba:

* `OnCreate` &ndash; Volá se jednou při prvním spuštění služby. Toto je, kde by měla být implementována inicializace kódu.
* `OnBind` &ndash; Tato metoda musí být implementované všechny třídy služby, ale spuštěná služba obvykle nemá navázaný na klienta. Z tohoto důvodu se spustila služba právě vrací `null`. Naproti tomu hybridní služby (což je kombinace vázané služby a spuštěná služba) má k implementaci a vrátit `Binder` pro klienta.
* `OnStartCommand` &ndash; Voláno pro každý požadavek pro spuštění služby, buď v reakci na hovor na `StartService` nebo restartování systému. Toto je, kde můžete službu začít jakékoli dlouho běžící úlohy. Vrátí metodu `StartCommandResult` hodnotu, která určuje, jak nebo pokud systém by měl zpracovat restartování služby po vypnutí kvůli nedostatku paměti. Toto volání probíhá na hlavní vlákno. Tato metoda je podrobněji popsané v níže.
* `OnDestroy` &ndash; Tato metoda je volána, když je zničen službu. Se používá k provádění všech konečné vyčistit vyžaduje.

Je důležité metoda spustila službu `OnStartCommand` metoda. Bude volána pokaždé, když služba obdrží požadavek na nějakou práci. Následující fragment kódu je příkladem `OnStartCommand`: 

```csharp
public override StartCommandResult OnStartCommand (Android.Content.Intent intent, StartCommandFlags flags, int startId)
{
    // This method executes on the main thread of the application.
    Log.Debug ("DemoService", "DemoService started");
    ...
    return StartCommandResult.Sticky;
}
```

První parametr je `Intent` objekt obsahující metadata o pracovní postup. Druhý parametr obsahuje `StartCommandFlags` hodnotu, která obsahuje některé informace o volání metody. Tento parametr má jednu nebo dvě možné hodnoty:

* `StartCommandFlag.Redelivery` &ndash; To znamená, že `Intent` je znovu doručení předchozí `Intent`. Tato hodnota získáte při měla služba vrátila `StartCommandResult.RedeliverIntent` , ale byla zastavena, než ji může být správně vypnut.
* `StartCommandFlag.Retry` &dash; Tato hodnota přijetí při předchozím `OnStartCommand` volání se nezdařilo a Android se snaží se záměrem stejný jako předchozí neúspěšný pokus znovu spustit službu.
 
Nakonec třetí parametr není celočíselná hodnota, která je jedinečná pro aplikace, která identifikuje požadavek. Je možné, že více volající může vyvolat stejného objektu služby. Tato hodnota se používá k přidružení požadavek na zastavení služby se daný požadavek na spuštění služby. Bude se podrobněji popsána v části [Probíhá zastavování služby](#Stopping_the_Service). 

Hodnota `StartCommandResult` je vrácena službou jako návrh do systému Android k tomu, co dělat, když je služba ukončen z důvodu omezení prostředku. Existují tři možné hodnoty pro `StartCommandResult`:

* **[StartCommandResult.NotSticky](https://developer.xamarin.com/api/field/Android.App.StartCommandResult.NotSticky/)**  &ndash; tato hodnota informuje Android, která není nutné restartovat službu, který má byly ukončeny. Jako příklad toho zvažte službu, která generuje miniatury pro galerie v aplikaci. Pokud je ukončená služby, není klíčové okamžitě znovu vytvořit miniaturu &ndash; miniaturu můžete znovu vytvořit při příštím spuštění aplikace.
* **[StartCommandResult.Sticky](https://developer.xamarin.com/api/field/Android.App.StartCommandResult.Sticky/)**  &ndash; sdělí Android se restartovat službu, ale ne k poskytování poslední záměr, který spustil službu. Pokud neexistují žádné čekající záměry pro zpracování, pak `null` bude zadaná pro parametr záměrné. Příkladem může být aplikace na hudební přehrávač; Služba restartuje připravené k přehrání Hudba, ale přehraje poslední skladbu. 
* **[StartCommandResult.RedeliverIntent](https://developer.xamarin.com/api/field/Android.App.StartCommandResult.RedeliverIntent/)**  &ndash; tato hodnota je bude říct Android a restartujte službu znovu poskytovat poslední `Intent`. Příkladem je služba, která stáhne soubor dat pro aplikaci. Pokud služba je ukončená, ještě potřeba stáhnout datového souboru. Vrácením `StartCommandResult.RedeliverIntent`, když Android restartuje službu také zajistí záměr (který obsahuje adresu URL souboru ke stažení) ke službě. Tím povolíte stahování se znovu spustit nebo obnovit (v závislosti na přesné implementace kód).

Je čtvrtý hodnota `StartCommandResult` &ndash; `StartCommandResult.ContinuationMask`. Tato hodnota je vrácen rutinou `OnStartCommand` , a popisuje, jak Android bude služba má byly ukončeny. Tato hodnota se obvykle nepoužívá ke spuštění služby.

Události životního cyklu u klíče Začínáme služby jsou uvedeny v tomto diagramu: 

![Diagram zobrazující pořadí, ve kterém jsou volány metody životního cyklu](started-services-images/started-service-01.png "diagram zobrazující pořadí, ve kterém jsou volány metody životního cyklu.")


<a name="Stopping_the_Service" />

## <a name="stopping-the-service"></a>Probíhá zastavování služby

Spuštěná služba bude dál běžet bez omezení; Android ponechá služba spuštěná, dokud dostatek systémových prostředků. Buď klienta potřeba zastavit službu nebo službu přestat, samotné až skončíte svou práci. Existují dva způsoby, jak zastavit službu: 
 
* **[Android.Content.Context.StopService()](https://developer.xamarin.com/api/member/Android.Content.Context.StopService/p/Android.Content.Intent/)**  &ndash; (například aktivitu) může klient požadovat služby zastavte volání `StopService` metoda: 

    ```csharp
    StopService(new Intent(this, typeof(DemoService));
    ```

* **[Android.App.Service.StopSelf()](https://developer.xamarin.com/api/member/Android.App.Service.StopSelf()/)**  &ndash; služby může sám sebe ukončit vyvoláním `StopSelf`:

    ```csharp
    StopSelf();
    ```
    
### <a name="using-startid-to-stop-a-service"></a>Použití startId k zastavení služby

Více volající může požádat o spuštění služby. Pokud je požadavek nezpracovaných spuštění, můžete použít službu `startId` předá do `OnStartCommand` zabránit předčasně zastavenou službu. `startId` Odpovídá nejnovější volání `StartService`a se zvýší pokaždé, když je volána. Proto pokud další žádost o `StartService` ještě neskončilo volání `OnStartCommand`, můžete volat službu `StopSelfResult`, předání nejnovější hodnotu `startId` obdržela (namísto volání jednoduše `StopSelf`). Pokud volání `StartService` ještě neskončilo odpovídající volání `OnStartCommand`, systém nebude zastavit službu, protože `startId` používány `StopSelf` volání nebude odpovídat na nejnovější `StartService` volání.


## <a name="related-links"></a>Související odkazy

- [StartedServicesDemo (sample)](https://developer.xamarin.com/samples/monodroid/ApplicationFundamentals/ServiceSamples/StartedServicesDemo/)
- [Android.App.Service](https://developer.xamarin.com/api/type/Android.App.Service)
- [Android.App.StartCommandFlags](https://developer.xamarin.com/api/type/Android.App.StartCommandFlags)
- [Android.App.StartCommandResult](https://developer.xamarin.com/api/type/Android.App.StartCommandResult)
- [Android.Content.BroadcastReceiver](https://developer.xamarin.com/api/type/Android.Content.BroadcastReceiver/)
- [Android.Content.Intent](https://developer.xamarin.com/api/type/Android.Content.Intent)
- [Android.OS.Handler](https://developer.xamarin.com/api/type/Android.OS.Handler/)
- [Android.Widget.Toast](https://developer.xamarin.com/api/type/Android.Widget.Toast/)
- [Ikony stavového řádku](http://developer.android.com/guide/practices/ui_guidelines/icon_design_status_bar.html)
