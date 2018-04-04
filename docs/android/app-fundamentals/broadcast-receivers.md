---
title: Příjemci všesměrového vysílání v Xamarin.Android
description: Tato část popisuje postup použití příjemce všesměrového vysílání.
ms.prod: xamarin
ms.assetid: B2727160-12F2-43EE-84B5-0B15C8FCF4BD
ms.technology: xamarin-android
author: topgenorth
ms.author: toopge
ms.date: 03/19/2018
ms.openlocfilehash: 75d42da4ba01aaefded0081da02b8e1651695f46
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/04/2018
---
# <a name="broadcast-receivers-in-xamarinandroid"></a>Příjemci všesměrového vysílání v Xamarin.Android

_Tato část popisuje postup použití příjemce všesměrového vysílání._


## <a name="broadcast-receiver-overview"></a>Přehled všesměrového vysílání příjemce

A _všesměrového vysílání příjemce_ je Android součásti, které umožní aplikaci reagovat na zprávy (Android [ `Intent` ](https://developer.xamarin.com/api/type/Android.Content.Intent/)), jsou vysílání Android operačního systému nebo aplikace. Postupujte podle všesměrové _publikování a odběru_ modelu &ndash; událost způsobí, že vysílání na publikovat a přijatých ty součásti, které jsou máte zájem o události. 

Android identifikuje dva typy vysílání:

* **Explicitní vysílání** &ndash; tyto typy vysílání zacílit na konkrétní aplikaci. Explicitní všesměrové vysílání slouží nejčastěji spuštění aktivity. Příkladem explicitní všesměrové vysílání když aplikace potřebuje k vytočte telefonní číslo; se bude odesílat záměrem zacílený telefonní aplikaci pro Android a předejte podél telefonní číslo chcete vytočit. Android pak směrovat záměr telefonní aplikace.
* **Implicitní broadcase** &ndash; tyto vysílání se odesílají na všechny aplikace na zařízení. Je například implicitní všesměrové vysílání `ACTION_POWER_CONNECTED` záměr. Tento záměr je publikován pokaždé, když Android zjistí, zda je ukládání baterie na zařízení. Android bude směrovat tato záměr na všechny aplikace, která byla zaregistrovaná pro tuto událost.

Příjemce všesměrového vysílání je podtřídou třídy `BroadcastReceiver` typu a musí přepsat [ `OnReceive` ](https://developer.xamarin.com/api/member/Android.Content.BroadcastReceiver.OnReceive/p/Android.Content.Context/Android.Content.Intent/) metoda. Android, budou spuštěny `OnReceive` na hlavní vlákno, takže tato metoda by se měly navrhovat rychle provést. Potřeba dát pozor, pokud při vytváření kopie vlákna v `OnReceive` protože Android může po dokončení metody ukončit proces. Pokud příjemce všesměrového vysílání, musíte provést dlouhotrvající pracovní, pak se doporučuje naplánovat _úlohy_ pomocí `JobScheduler` nebo _Firebase úlohy dispečera_. Plánování práce s úlohou budou popsané v samostatné průvodce.

_Záměrné filtru_ slouží k registraci všesměrového vysílání přijímač tak, aby Android může správně směrovat zprávy. Záměrné filtru lze zadat v době běhu (to se někdy označuje jako _kontextu registrovaný příjemce_ nebo jako _dynamickou registraci_) nebo může být staticky definovaných v Android Manifest ( _zaregistrován manifest příjemce_). Poskytuje atribut C# Xamarin.Android `IntentFilterAttribute`, který bude staticky registrovat záměrné filtru (to bude popsané podrobněji dál v této příručce). Od verze Android 8.0, není možné pro aplikaci staticky zaregistrovat pro implicitní všesměrové vysílání.

Základní rozdíl mezi zaregistrován manifest příjemce a příjemce kontextu registrovaný je, že příjemce kontextu registrovaný bude reagovat jen na všesměrové když aplikace běží, zatímco zaregistrován manifest příjemce může reagovat na vysílá, i když není aplikace spuštěna.  

Existují dvě sady rozhraní API pro správu všesměrového vysílání příjemce a odesílání vysílání:

1. **`Context`** &ndash; `Android.Content.Context` Třídu lze použít k registraci všesměrového vysílání příjemce, který bude reagovat na systémové události. `Context` Se také používá k publikování systémové vysílání.
2. **[`LocalBroadcastManager`](https://developer.android.com/reference/android/support/v4/content/LocalBroadcastManager.html#sendBroadcast(android.content.Intent))** &ndash; Toto je rozhraní API, které je k dispozici prostřednictvím [balíček NuGet knihovny podporu Xamarin v4](https://www.nuget.org/packages/Xamarin.Android.Support.v4/). Tato třída slouží k udržování všesměrové vysílání a všesměrového vysílání příjemci izolované v kontextu aplikace, která je jejich používání. Tato třída může být užitečné brání reagovat na jen aplikace vysílání nebo odesílání zpráv do privátní příjemci jiných aplikací.

Všesměrového vysílání příjemce nemusí být zobrazeny dialogová okna a není doporučeno zahájíte aktivita z v rámci všesměrového vysílání příjemce. Pokud příjemce všesměrového vysílání, musíte upozornit uživatele, ho měli publikovat oznámení.

Není možné vytvořit vazbu nebo spusťte službu z v rámci všesměrového vysílání příjemce. 

Tato příručka popisuje postup vytvoření všesměrového vysílání příjemce a a zaregistrujte ho tak, aby obdržet všesměrové vysílání.

## <a name="creating-a-broadcast-receiver"></a>Vytvoření příjemce všesměrového vysílání

Vytvoření všesměrového vysílání příjemce v Xamarin.Android, aplikace by měly podtřídy `BroadcastReceiver` třídy, se s adorn `BroadcastReceiverAttribute`a přepsat `OnReceive` metoda:

```csharp
[BroadcastReceiver(Enabled = true, Exported = false)]
public class SampleReceiver : BroadcastReceiver
{
    public override void OnReceive(Context context, Intent intent)
    {
        // Do stuff here.
        
        String value = intent.GetStringExtra("key");
    }
}
```

Když Xamarin.Android kompiluje třídy, bude se také aktualizovat AndroidManifest s meta-data potřebná k registraci příjemce. Pro staticky registrovat všesměrového vysílání příjemce `Enabled` musí být správně nastavena na `true`, jinak nebude možné vytvořit instanci příjemce Android.
 
`Exported` Vlastnost určuje, jestli všesměrového vysílání příjemce může přijímat zprávy z mimo aplikaci. Pokud vlastnost není explicitně nastavena, výchozí hodnota vlastnosti je určen podle Android na základě, pokud jsou všechny záměr filtry přidružené k příjemce všesměrového vysílání. Pokud je alespoň jeden filtr záměr pro všesměrového vysílání příjemce pak Android bude předpokládat, že `Exported` vlastnost je `true`. Pokud neexistují žádné záměr filtrů přiřazených k příjemce všesměrového vysílání, pak Android bude předpokládat, že hodnota je `false`. 

`OnReceive` Metoda získá odkaz na `Intent` který byl odeslán k příjemce všesměrového vysílání. Tato díky je možné pro odesílatele záměru předat hodnoty všesměrového vysílání příjemce.

### <a name="statically-registering-a-broadcast-receiver-with-an-intent-filter"></a>Filtr záměr staticky registrace příjemce všesměrového vysílání

Když `BroadcastReceiver` opatřen s [ `IntentFilterAttribute` ](https://developer.xamarin.com/api/type/Android.App.IntentFilterAttribute/), Xamarin.Android přidá nezbytné `<intent-filter>` elementu, který chcete pro Android manifestu v době kompilace. Následující fragment kódu je příkladem všesměrového vysílání příjemce, který se spustí po dokončení spuštění (Pokud je uživatel byla udělena příslušná oprávnění Android) zařízení:

```csharp
[BroadcastReceiver(Enabled = true)]
[IntentFilter(new[] { Android.Content.Intent.ActionBootCompleted })]
public class MyBootReceiver : BroadcastReceiver
{
    public override void OnReceive(Context context, Intent intent)
    {
        // Work that should be done when the device boots.     
    }
}
```

Je také možné vytvořit filtr záměrné, který bude reagovat na vlastních tříd Intent. Podívejte se na následující příklad: 

```csharp
[BroadcastReceiver(Enabled = true)]
[IntentFilter(new[] { "com.xamarin.example.TEST" })]
public class MySampleBroadcastReceiver : BroadcastReceiver
{
    public override void OnReceive(Context context, Intent intent)
    {
        // Do stuff here
    }
}
```

Aplikace, které cílí na Android 8.0 (API úrovně 26) nebo vyšší nemusí staticky zaregistrovat pro implicitní všesměrové vysílání. Aplikace může stále staticky zaregistrovat explicitní všesměrové vysílání. Existuje malé seznam implicitní vysílání, které jsou vyloučené z tohoto omezení. Tyto výjimky jsou popsány v [implicitní vysílání výjimky](https://developer.android.com/guide/components/broadcast-exceptions.html) průvodce v Android dokumentaci. Aplikace, které zajímá implicitní všesměrové vysílání, musíte udělat proto dynamicky pomocí `RegisterReceiver` metoda. To je popsána dále.  

### <a name="context-registering-a-broadcast-receiver"></a>Kontext registrace všesměrového vysílání příjemce 

Kontext – registrace (také označované jako dynamické registraci) příjemce provádí volání `RegisterReceiver` metoda a všesměrového vysílání příjemce musí neregistrované pomocí volání `UnregisterReceiver` metoda. Aby se zabránilo unikající prostředky, je potřeba zrušit registraci příjemce, když už není relevantní pro daný kontext (aktivity nebo služby). Služba může například vysílání záměrem k informování aktivitu, která jsou k dispozici, který se má zobrazit uživateli aktualizace. Při spuštění aktivity by zaregistrovat pro tyto záměry. Při aktivity se přesune do na pozadí a již nebude viditelná pro uživatele, se musí zrušit příjemce protože uživatelské rozhraní pro zobrazení aktualizací již není viditelný. Následující fragment kódu je příklad toho, jak se zaregistrovat a zrušit příjemce všesměrového vysílání v rámci aktivity:

```csharp
[Activity(Label = "MainActivity", MainLauncher = true, Icon = "@mipmap/icon")]
public class MainActivity: Activity 
{
    MySampleBroadcastReceiver receiver;
    
    protected override void OnCreate(Bundle savedInstanceState)
    {
        base.OnCreate(savedInstanceState);
        receiver = new MySampleBroadcastReceiver()
        
        // Code omitted for clarity
    }
    
    protected override OnResume() 
    {
        base.OnResume();
        RegisterReceiver(receiver, new IntentFilter("com.xamarin.example.TEST"));
        // Code omitted for clarity
    }
    
    protected override OnPause() 
    {
        UnregisterReceiver(receiver);
        // Code omitted for clarity
        base.OnPause();
    }
}
```

V předchozím příkladu aktivity vycházejí do popředí, se zaregistruje všesměrového vysílání příjemce, který bude naslouchat vlastní záměr pomocí `OnResume` metoda životního cyklu. Při přesunu aktivity do na pozadí `OnPause()` metoda zruší registraci příjemce.

## <a name="publishing-a-broadcast"></a>Publikování vysílání

Vysílání může být publikována do všechny aplikace nainstalované v zařízení vytvoření záměrné objektu a její odeslání `SendBroadcast` nebo `SendOrderedBroadcast` metoda.  

1. **Metody Context.SendBroadcast** &ndash; existuje několik implementace této metody.
   Tyto metody se vysílání cílem celý systém. Thatwill všesměrového vysílání příjemci dostávat záměr v neurčitém pořadí. To zajišťuje značnou flexibilitu ale znamená, že je možné pro jiné aplikace při registraci a přijímat záměr. To může představovat možné bezpečnostní riziko. Aplikace je nutné implementovat přidání zabezpečení před neoprávněným přístupem. Jedním z možných řešení je použití `LocalBroadcastManager` kterého bude pouze odeslání zprávy v prostoru soukromé aplikace. Tento fragment kódu je jedním z příkladů jak dispatch záměrem pomocí jedné z `SendBroadcast` metody:

   ```csharp
   Intent message = new Intent("com.xamarin.example.TEST");
   // If desired, pass some values to the broadcast receiver.
   intent.PutExtra("key", "value");
   SendBroadcast(intent);
   ```

    Tento fragment kódu je další příklad odesílání vysílání pomocí `Intent.SetAction` metodu, jak identifikovat akce:
    
    ```csharp 
    Intent intent = new Intent();
    intent.SetAction("com.xamarin.example.TEST");
    intent.PutExtra("key", "value");
    SendBroadcast(intent);
    ```
   
2. **Context.SendOrderedBroadcast** &ndash; metodu je velmi podobné `Context.SendBroadcast`, s rozdílem je, že bude záměr publikované jeden v čase k příjemce, v pořadí, recievers registraci.
   
### <a name="localbroadcastmanager"></a>LocalBroadcastManager

[V4 knihovna podpory Xamarin](https://www.nuget.org/packages/Xamarin.Android.Support.v4/) poskytuje třídu pomocníka názvem [ `LocalBroadcastManager` ](https://developer.android.com/reference/android/support/v4/content/LocalBroadcastManager.html). `LocalBroadcastManager` Je určený pro aplikace, které nechcete odeslat nebo přijmout vysílání z jiných aplikací na zařízení. `LocalBroadcastManager` Bude publikovat pouze zprávy v rámci této aplikace. Jiné aplikace na zařízení nemůže přijímat zprávy, které jsou publikovány pomocí `LocalBroadcastManager`. 

Tento fragment kódu ukazuje, jak odeslat záměrné pomocí `LocalBroadcastManager`:

```csharp
Intent message = new Intent("com.xamarin.example.TEST");
// If desired, pass some values to the broadcast receiver.
intent.PutExtra("key", "value");
Android.Support.V4.Content.LocalBroadcastManager.GetInstance(this).SendBroadcast(message);
``` 

## <a name="related-links"></a>Související odkazy

- [BroadcastReceiver](https://developer.xamarin.com/api/type/Android.Content.BroadcastReceiver/)
- [Context.RegisterReceiver](https://developer.xamarin.com/api/member/Android.Content.Context.RegisterReceiver/p/Android.Content.BroadcastReceiver/Android.Content.IntentFilter/System.String/Android.OS.Handler/)
- [Context.SendBroadcast](https://developer.xamarin.com/api/member/Android.Content.Context.SendBroadcast/p/Android.Content.Intent/)
- [Context.UnregisterReceiver](https://developer.xamarin.com/api/member/Android.Content.Context.UnregisterReceiver/p/Android.Content.BroadcastReceiver/)
- [Záměr](https://developer.xamarin.com/api/type/Android.Content.Intent/)
- [IntentFilter](https://developer.xamarin.com/api/type/Android.App.IntentFilterAttribute/)
- [LocalBroadcastManager](https://developer.android.com/reference/android/support/v4/content/LocalBroadcastManager.html#sendBroadcast(android.content.Intent))
- [Místní oznámení v Android](~/android/app-fundamentals/notifications/local-notifications.md)
- [Podpory Android v4 knihovny](https://www.nuget.org/packages/Xamarin.Android.Support.v4/)
