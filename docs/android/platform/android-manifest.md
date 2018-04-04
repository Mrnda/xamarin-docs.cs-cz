---
title: Práce s manifestu systému Android.
ms.prod: xamarin
ms.assetid: CB7CCF60-FEF1-3B28-215F-159391E74347
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/05/2018
ms.openlocfilehash: 18817063900437baa625d8572f0ae28fec77be1e
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/04/2018
---
# <a name="working-with-the-android-manifest"></a>Práce s manifestu systému Android.


## <a name="overview"></a>Přehled

**AndroidManifest.xml** je výkonný soubor v Android platforma, která vám umožní popisovat funkcích a požadavcích vaší aplikace do systému Android. Práce s ní však není jednoduché. Chcete-li minimalizovat tím, že se můžete přidat vlastní atributy do vaší třídy, které pak bude sloužit k automatickému generování manifestu pro vás tento problém s pomáhá Xamarin.Android. Naším cílem je, že 99 % naši uživatelé by nikdy potřeba upravit ručně **AndroidManifest.xml**. 

**AndroidManifest.xml** se vygeneruje jako součást procesu sestavení a XML v rámci nalezen **Properties/AndroidManifest.xml** je sloučen s XML, které se generují z vlastních atributů. Výsledná sloučit **AndroidManifest.xml** se nachází v **obj** podadresáři; například se nachází v **obj/Debug/android/AndroidManifest.xml** pro sestavení pro ladění . Proces slučovat je jednoduchá: používá vlastní atributy v rámci kód ke generování elementů XML a *vloží* tyto elementy do **AndroidManifest.xml**. 



## <a name="the-basics"></a>Základní informace

V době kompilace sestavení hledat jinou hodnotu než`abstract` třídy, které jsou odvozeny od [aktivity](https://developer.xamarin.com/api/type/Android.App.Activity/) a mají [ `[Activity]` ](https://developer.xamarin.com/api/type/Android.App.ActivityAttribute/) atribut deklarován na ně. K sestavení manifestu pak používá tyto třídy a atributy. Zvažte například následující kód: 

```csharp
namespace Demo
{
    public class MyActivity : Activity
    {
    }
}
```

Výsledkem nic generován v **AndroidManifest.xml**. Pokud chcete `<activity/>` element má být vygenerován, budete muset použít [ `[Activity]` ](https://developer.xamarin.com/api/type/Android.App.Activity/Attribute) vlastních atributů: 

```csharp
namespace Demo
{
    [Activity]
    public class MyActivity : Activity
    {
    }
}
```

Tento příklad způsobí, že následující fragment kódu xml mají být přidány do **AndroidManifest.xml**:

```xml
<activity android:name="md5a7a3c803e481ad8926683588c7e9031b.MainActivity" />
```

`[Activity]` Atribut nemá žádný vliv `abstract` typy; `abstract` typy jsou ignorovány.



### <a name="activity-name"></a>Název aktivity

Od verze Xamarin.Android 5.1, název typu aktivity podle MD5SUM sestavení kvalifikovaný název typu, která je exportována. To umožňuje stejný plně kvalifikovaný název a je třeba zadat ze dvou různých sestavení není získat balení chyby. (Před Xamarin.Android 5.1, byl vytvořen výchozí název typu aktivity z malého obor názvů a název třídy.) 

Pokud chcete přepsat toto výchozí nastavení a explicitně zadáte název aktivity, použijte [ `Name` ](https://developer.xamarin.com/api/property/Android.App.ActivityAttribute.Name/) vlastnost: 

```csharp
[Activity (Name="awesome.demo.activity")]
public class MyActivity : Activity
{
}
```

Tento příklad vytvoří následující fragment kódu xml:

```xml
<activity android:name="awesome.demo.activity" />
```

*Poznámka:*: měli byste použít `Name` vlastnosti pouze z důvodů zpětné kompatibility, jako například přejmenování může zpomalit vyhledávání typu za běhu. Pokud máte starší kód, který předpokládá, že výchozí název typu aktivity na vycházet z malého obor názvů a název třídy, najdete v části [Android s obálku pojmenování](https://developer.xamarin.com/releases/android/xamarin.android_5/xamarin.android_5.1/#Android_Callable_Wrapper_Naming) pro tipy k zachování kompatibility. 


### <a name="activity-title-bar"></a>Aktivita záhlaví

Ve výchozím nastavení Android poskytuje vaší aplikaci záhlaví při spuštění. Hodnota použité pro tento [ `/manifest/application/activity/@android:label` ](http://developer.android.com/guide/topics/manifest/activity-element.html#label). Ve většině případů bude tato hodnota liší od vašeho název třídy. Pokud chcete zadat popisek vaší aplikace v záhlaví okna, použijte [ `Label` ](https://developer.xamarin.com/api/property/Android.App.ActivityAttribute.Label/) vlastnost.
Příklad: 

```csharp
[Activity (Label="Awesome Demo App")]
public class MyActivity : Activity
{
}
```

Tento příklad vytvoří následující fragment kódu xml:

```xml
<activity android:label="Awesome Demo App" 
          android:name="md5a7a3c803e481ad8926683588c7e9031b.MainActivity" />
```


### <a name="launchable-from-application-chooser"></a>Launchable z výběru aplikace

Ve výchozím nastavení vaši aktivitu nezobrazí v obrazovce spuštění aplikace pro Android. Je to proto, že budou pravděpodobně řada aktivit ve vaší aplikaci a ikonu nechcete, aby pro každé z nich. Chcete-li určit, která by měla být launchable od spuštění aplikace, použijte [ `MainLauncher` ](https://developer.xamarin.com/api/property/Android.App.ActivityAttribute.MainLauncher/) vlastnost. Příklad: 

```csharp
[Activity (Label="Awesome Demo App", MainLauncher=true)] 
public class MyActivity : Activity
{
}
```

Tento příklad vytvoří následující fragment kódu xml:

```xml
<activity android:label="Awesome Demo App" 
          android:name="md5a7a3c803e481ad8926683588c7e9031b.MainActivity">
  <intent-filter>
    <action android:name="android.intent.action.MAIN" />
    <category android:name="android.intent.category.LAUNCHER" />
  </intent-filter>
</activity>
```



### <a name="activity-icon"></a>Ikona aktivity

Ve výchozím nastavení bude vaše aktivity udělená výchozí ikonu Spouštěče poskytované systémem. Pokud chcete používat vlastní ikonou, nejprve přidejte vaše **.png** k **prostředky/drawable**, nastavte jeho akce sestavení na **AndroidResource**, použije [ `Icon` ](https://developer.xamarin.com/api/property/Android.App.ActivityAttribute.Icon/) vlastnosti k určení ikony použít. Příklad: 

```csharp
[Activity (Label="Awesome Demo App", MainLauncher=true, Icon="@drawable/myicon")] 
public class MyActivity : Activity
{
}
```

Tento příklad vytvoří následující fragment kódu xml:

```xml
<activity android:icon="@drawable/myicon" android:label="Awesome Demo App" 
          android:name="md5a7a3c803e481ad8926683588c7e9031b.MainActivity">
  <intent-filter>
    <action android:name="android.intent.action.MAIN" />
    <category android:name="android.intent.category.LAUNCHER" />
  </intent-filter>
</activity>
```


### <a name="permissions"></a>Oprávnění

Když přidáte oprávnění pro Android Manifest (jak je popsáno v [přidat oprávnění pro Android Manifest](https://developer.xamarin.com/recipes/android/general/projects/add_permissions_to_android_manifest/)), tato oprávnění se zaznamenávají v **Properties/AndroidManifest.xml**. Například pokud nastavíte `INTERNET` oprávnění, následující prvek přidán na **Properties/AndroidManifest.xml**: 

```xml
<uses-permission android:name="android.permission.INTERNET" />
```

Sestavení pro ladění automaticky nastaví některá oprávnění, aby snazší ladění (například `INTERNET` a `READ_EXTERNAL_STORAGE`) &ndash; tato nastavení jsou nastavené v vygenerovaného **obj/Debug/android/AndroidManifest.xml** a nejsou zobrazí v povolené **požadovaná oprávnění** nastavení. 

Například, pokud byste zkontrolovat vygenerovaný soubor manifestu v **obj/Debug/android/AndroidManifest.xml**, mohou se zobrazit následující přidány elementy oprávnění: 

```xml
<uses-permission android:name="android.permission.INTERNET" />
<uses-permission android:name="android.permission.READ_EXTERNAL_STORAGE" />
```

Verze sestavení verze manifestu (v **obj/Debug/android/AndroidManifest.xml**), tato oprávnění se *není* automaticky nakonfigurované. Pokud zjistíte, že přepnutí na sestavení pro vydání způsobí, že aplikaci ztratíte oprávnění, která byla k dispozici v sestavení ladicí verze, ověřte, že jste toto oprávnění nastavili explicitně **požadovaná oprávnění** nastavení pro vaši aplikaci (viz  **Sestavení > aplikace pro Android** v sadě Visual Studio pro Mac; najdete v části **vlastnosti > Android Manifest** v sadě Visual Studio). 




## <a name="advanced-features"></a>Pokročilé funkce


### <a name="intent-actions-and-features"></a>Záměrné akce a funkce

Android manifest poskytuje způsob pro popis možností vaši aktivitu. To se provádí prostřednictvím [záměry](http://developer.android.com/guide/topics/manifest/intent-filter-element.html) a [ `[IntentFilter]` ](https://developer.xamarin.com/api/type/Android.App.IntentFilterAttribute/) vlastních atributů. Můžete zadat akce, které jsou vhodné pro vaši aktivitu [ `IntentFilter` ](https://developer.xamarin.com/api/constructor/Android.App.IntentFilterAttribute.IntentFilterAttribute/p/System.String[]/) konstruktor a kategorie, které jsou vhodné s [ `Categories` ](https://developer.xamarin.com/api/property/Android.App.IntentFilterAttribute.Categories/) vlastnost. Je třeba zadat alespoň jednu aktivitu (což je proto aktivity jsou uvedeny v konstruktoru). `[IntentFilter]` může být zadaný několikrát, a každé použití výsledků v samostatném `<intent-filter/>` v rámci `<activity/>`. Příklad:

```csharp
[Activity (Label="Awesome Demo App", MainLauncher=true, Icon="@drawable/myicon")] 
[IntentFilter (new[]{Intent.ActionView}, 
        Categories=new[]{Intent.CategorySampleCode, "my.custom.category"})]
public class MyActivity : Activity
{
}
```

Tento příklad vytvoří následující fragment kódu xml:

```xml
<activity android:icon="@drawable/myicon" android:label="Awesome Demo App" 
          android:name="md5a7a3c803e481ad8926683588c7e9031b.MainActivity">
  <intent-filter>
    <action android:name="android.intent.action.MAIN" />
    <category android:name="android.intent.category.LAUNCHER" />
  </intent-filter>
  <intent-filter>
    <action android:name="android.intent.action.VIEW" />
    <category android:name="android.intent.category.SAMPLE_CODE" />
    <category android:name="my.custom.category" />
  </intent-filter>
</activity>
```


### <a name="application-element"></a>Element aplikace

Android manifest také poskytuje způsob, jak deklarovat vlastnosti pro celou aplikaci. To se provádí prostřednictvím `<application>` elementu a jeho protějšek [aplikace](https://developer.xamarin.com/api/type/Android.App.ApplicationAttribute/) vlastních atributů. Všimněte si, že jsou nastavení (sestavení celou) pro celou aplikaci spíše než nastavení pro aktivity. Obvykle je deklarovat `<application>` vlastnosti pro celou aplikaci a potom toto nastavení přepsat (podle potřeby) na základě za aktivitu. 

Například následující `Application` přidání atributu do **AssemblyInfo.cs** označíte, že aplikace mohou být vyladěnou, zda je jeho uživatelem čitelný název **aplikace My**, že se používá `Theme.Light` styl jako výchozí motiv pro všechny aktivity: 

```csharp
[assembly: Application (Debuggable=true,   
                        Label="My App",   
                        Theme="@android:style/Theme.Light")]
```

Toto prohlášení způsobí, že následující fragment kódu XML vygeneruje **obj/Debug/android/AndroidManifest.xml**:

```xml
<application android:label="My App" 
             android:debuggable="true" 
             android:theme="@android:style/Theme.Light"
                ... />
```
V tomto příkladu všechny aktivity v aplikaci, je výchozí nastavení `Theme.Light` stylu. Pokud nastavíte aktivity motiv na `Theme.Dialog`, pouze to, že aktivity použije `Theme.Dialog` stylu při všechny aktivity v aplikaci, je výchozí nastavení `Theme.Light` styl jako sada v `<application>` element. 

`Application` Element není jediný způsob, jak nakonfigurovat `<application>` atributy. Alternativně můžete vložit atributy přímo do `<application>` element **Properties/AndroidManifest.xml**. Tato nastavení jsou sloučeny do konečné `<application>` element, který se nachází v **obj/Debug/android/AndroidManifest.xml**. Všimněte si, že obsah **Properties/AndroidManifest.xml** vždy přepsat data poskytnutá podle vlastních atributů. 

Existuje mnoho atributů celou aplikaci, která můžete konfigurovat v `<application>` element; Další informace o těchto nastaveních najdete v tématu [veřejné vlastnosti](https://developer.xamarin.com/api/type/Android.App.ApplicationAttribute/#Public_Properties) části [ApplicationAttribute](https://developer.xamarin.com/api/type/Android.App.ApplicationAttribute/). 



## <a name="list-of-custom-attributes"></a>Seznam vlastních atributů

-   [Android.App.ActivityAttribute](https://developer.xamarin.com/api/type/Android.App.ActivityAttribute/) : generuje [/manifest/application/activity](http://developer.android.com/guide/topics/manifest/activity-element.html) XML fragment 
-   [Android.App.ApplicationAttribute](https://developer.xamarin.com/api/type/Android.App.ApplicationAttribute/) : generuje [/manifest/aplikace](http://developer.android.com/guide/topics/manifest/application-element.html) XML fragment 
-   [Android.App.InstrumentationAttribute](https://developer.xamarin.com/api/type/Android.App.InstrumentationAttribute/) : generuje [/manifest/instrumentace](http://developer.android.com/guide/topics/manifest/instrumentation-element.html) XML fragment 
-   [Android.App.IntentFilterAttribute](https://developer.xamarin.com/api/type/Android.App.IntentFilterAttribute/) : generuje [//intent-filter](http://developer.android.com/guide/topics/manifest/intent-filter-element.html) XML fragment 
-   [Android.App.MetaDataAttribute](https://developer.xamarin.com/api/type/Android.App.MetaDataAttribute/) : generuje [//meta-data](http://developer.android.com/guide/topics/manifest/meta-data-element.html) XML fragment 
-   [Android.App.PermissionAttribute](https://developer.xamarin.com/api/type/Android.App.PermissionAttribute/) : generuje [//permission](http://developer.android.com/guide/topics/manifest/permission-element.html) XML fragment 
-   [Android.App.PermissionGroupAttribute](https://developer.xamarin.com/api/type/Android.App.PermissionGroupAttribute/) : generuje [//permission-group](http://developer.android.com/guide/topics/manifest/permission-group-element.html) XML fragment 
-   [Android.App.PermissionTreeAttribute](https://developer.xamarin.com/api/type/Android.App.PermissionTreeAttribute/) : generuje [//permission-tree](http://developer.android.com/guide/topics/manifest/permission-tree-element.html) XML fragment 
-   [Android.App.ServiceAttribute](https://developer.xamarin.com/api/type/Android.App.ServiceAttribute/) : generuje [/manifest/application/service](http://developer.android.com/guide/topics/manifest/service-element.html) XML fragment 
-   [Android.App.UsesLibraryAttribute](https://developer.xamarin.com/api/type/Android.App.UsesLibraryAttribute/) : generuje [/manifest/application/uses-library](http://developer.android.com/guide/topics/manifest/uses-library-element.html) XML fragment 
-   [Android.App.UsesPermissionAttribute](https://developer.xamarin.com/api/type/Android.App.UsesPermissionAttribute/) : generuje [/manifest/uses-permission](http://developer.android.com/guide/topics/manifest/uses-permission-element.html) XML fragment 
-   [Android.Content.BroadcastReceiverAttribute](https://developer.xamarin.com/api/type/Android.Content.BroadcastReceiverAttribute/) : generuje [/manifest/application/receiver](http://developer.android.com/guide/topics/manifest/receiver-element.html) XML fragment 
-   [Android.Content.ContentProviderAttribute](https://developer.xamarin.com/api/type/Android.Content.ContentProviderAttribute/) : generuje [/manifest/application/provider](http://developer.android.com/guide/topics/manifest/provider-element.html) XML fragment 
-   [Android.Content.GrantUriPermissionAttribute](https://developer.xamarin.com/api/type/Android.Content.GrantUriPermissionAttribute/) : generuje [/manifest/application/provider/grant-uri-permission](http://developer.android.com/guide/topics/manifest/grant-uri-permission-element.html) XML fragment

