---
title: Práce s manifestu Androidu
ms.prod: xamarin
ms.assetid: CB7CCF60-FEF1-3B28-215F-159391E74347
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/05/2018
ms.openlocfilehash: 0857b70e6e1d9104f62ec2e26f8edbab385d06f3
ms.sourcegitcommit: b56b3f906d2c05a3f1be219ef41be8b79e519b8e
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/25/2018
ms.locfileid: "39242248"
---
# <a name="working-with-the-android-manifest"></a>Práce s manifestu Androidu


## <a name="overview"></a>Přehled

**Soubor AndroidManifest.xml** je soubor výkonné platformy Android, která umožňuje popisují funkčnosti a požadavků vaší aplikace pro Android. Práce s ní však není snadné. Xamarin.Android pomáhá minimalizovat tento potíže s tím, že můžete přidat vlastní atributy do vaší třídy, které se pak použije k automatickému generování manifestu pro vás. Naším cílem je, že 99 % naši uživatelé by nikdy muset ručně upravit **AndroidManifest.xml**. 

**Soubor AndroidManifest.xml** je generována jako součást procesu sestavení a XML nalezen v rámci **Properties/AndroidManifest.xml** je sloučen s XML, který je generován z vlastních atributů. Výsledné sloučené **AndroidManifest.xml** se nachází v **obj** podadresář; například se nachází v **obj/Debug/android/AndroidManifest.xml** pro sestavení pro ladění . Proces sloučení je jednoduché: vlastních atributů v rámci kód používá ke generování elementů XML a *vloží* tyto prvky do **AndroidManifest.xml**. 



## <a name="the-basics"></a>Základní informace

V době kompilace, jsou prohledávány sestavení pro jinou hodnotu než`abstract` třídy, které jsou odvozeny z [aktivity](https://developer.xamarin.com/api/type/Android.App.Activity/) a mít [ `[Activity]` ](https://developer.xamarin.com/api/type/Android.App.ActivityAttribute/) atribut deklarován s nimi. K sestavení manifestu potom použije tyto třídy a atributy. Zvažte například následující kód: 

```csharp
namespace Demo
{
    public class MyActivity : Activity
    {
    }
}
```

Výsledkem je nic generovaných ve **AndroidManifest.xml**. Chcete-li `<activity/>` prvek, který chcete vygenerovat, budete muset použít [ `[Activity]` ](https://developer.xamarin.com/api/type/Android.App.Activity/Attribute) vlastního atributu: 

```csharp
namespace Demo
{
    [Activity]
    public class MyActivity : Activity
    {
    }
}
```

Tento příklad způsobí, že chcete přidat do následující fragment xml **AndroidManifest.xml**:

```xml
<activity android:name="md5a7a3c803e481ad8926683588c7e9031b.MainActivity" />
```

`[Activity]` Atribut nemá žádný vliv `abstract` typy; `abstract` typy jsou ignorovány.



### <a name="activity-name"></a>Název aktivity

Od verze Xamarin.Android 5.1, název typu aktivity je založen na MD5SUM název kvalifikovaný pro sestavení typu exportované. Díky tomu stejný plně kvalifikovaný název musí poskytnout dvě různá sestavení a nelze získat chybu balení. (Před Xamarin.Android 5.1, byl vytvořen výchozí název typu aktivity z oboru názvů malými písmeny a název třídy.) 

Pokud chcete toto výchozí nastavení přepsat a explicitně zadat název aktivity, použijte [ `Name` ](https://developer.xamarin.com/api/property/Android.App.ActivityAttribute.Name/) vlastnost: 

```csharp
[Activity (Name="awesome.demo.activity")]
public class MyActivity : Activity
{
}
```

Tento příklad vytvoří následující fragment xml:

```xml
<activity android:name="awesome.demo.activity" />
```

*Poznámka:*: měli byste použít `Name` vlastnosti pouze z důvodů zpětné kompatibility, jako například přejmenování může zpomalit vyhledání typu za běhu. Pokud máte starší verzi kódu, který očekává, že výchozí název typu aktivity založený na obor názvů malými písmeny a názvu třídy, najdete v části [Android Volatelných obálky pojmenování](https://developer.xamarin.com/releases/android/xamarin.android_5/xamarin.android_5.1/#Android_Callable_Wrapper_Naming) tipy týkající se zachování kompatibility. 


### <a name="activity-title-bar"></a>Aktivita záhlaví

Ve výchozím nastavení Android poskytuje aplikace záhlaví při jejím spuštění. Hodnota použitá pro tuto [ `/manifest/application/activity/@android:label` ](http://developer.android.com/guide/topics/manifest/activity-element.html#label). Ve většině případů bude tato hodnota lišit od názvem své třídy. Pokud chcete nastavit popisek vaší aplikace v záhlaví okna, použijte [ `Label` ](https://developer.xamarin.com/api/property/Android.App.ActivityAttribute.Label/) vlastnost.
Příklad: 

```csharp
[Activity (Label="Awesome Demo App")]
public class MyActivity : Activity
{
}
```

Tento příklad vytvoří následující fragment xml:

```xml
<activity android:label="Awesome Demo App" 
          android:name="md5a7a3c803e481ad8926683588c7e9031b.MainActivity" />
```


### <a name="launchable-from-application-chooser"></a>Spustitelná z výběru aplikace

Ve výchozím nastavení vaše aktivity nezobrazí na obrazovce spuštění aplikace pro Android. Je to proto, že budou pravděpodobně řada aktivit ve vaší aplikaci, a nechcete, ikona pro každý z nich. Chcete-li určit, které z nich musí být spustitelné ze Spouštěče aplikací, použijte [ `MainLauncher` ](https://developer.xamarin.com/api/property/Android.App.ActivityAttribute.MainLauncher/) vlastnost. Příklad: 

```csharp
[Activity (Label="Awesome Demo App", MainLauncher=true)] 
public class MyActivity : Activity
{
}
```

Tento příklad vytvoří následující fragment xml:

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

Ve výchozím nastavení bude vaše aktivita udělená výchozí ikonu Spouštěče poskytované systémem. Pokud chcete použít vlastní ikonu, nejprve přidat vaše **.png** k **prostředky/drawable**, nastavte jeho akce sestavení na **AndroidResource**, pak použít [ `Icon` ](https://developer.xamarin.com/api/property/Android.App.ActivityAttribute.Icon/) vlastnosti a určit tak ikonu se má použít. Příklad: 

```csharp
[Activity (Label="Awesome Demo App", MainLauncher=true, Icon="@drawable/myicon")] 
public class MyActivity : Activity
{
}
```

Tento příklad vytvoří následující fragment xml:

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

Po přidání oprávnění pro Android Manifest (jak je popsáno v [přidat oprávnění pro Manifest v Androidu](https://github.com/xamarin/recipes/tree/master/Recipes/android/general/projects/add_permissions_to_android_manifest)), tato oprávnění se zaznamenávají do **Properties/AndroidManifest.xml**. Pokud nastavíte například `INTERNET` oprávnění, následující prvek přidán na **Properties/AndroidManifest.xml**: 

```xml
<uses-permission android:name="android.permission.INTERNET" />
```

Automaticky nastaví některá oprávnění, aby bylo snazší ladění sestavení pro ladění (například `INTERNET` a `READ_EXTERNAL_STORAGE`) &ndash; tato nastavení jsou nastavené v generované **obj/Debug/android/AndroidManifest.xml** a nejsou uvedené jako povolené v **požadovaná oprávnění** nastavení. 

Například, když si zblízka vygenerovaný soubor manifestu v **obj/Debug/android/AndroidManifest.xml**, může se zobrazit následující přidat prvky oprávnění: 

```xml
<uses-permission android:name="android.permission.INTERNET" />
<uses-permission android:name="android.permission.READ_EXTERNAL_STORAGE" />
```

Ve verzi sestavení verze manifestu (na **obj/Debug/android/AndroidManifest.xml**), tato oprávnění jsou *není* automaticky nakonfigurované. Pokud zjistíte, že přepnutí na sestavení pro vydání způsobí, že vaše aplikace ztratí oprávnění, která byla k dispozici v sestavení ladění, ověřte, že máte explicitně nastavit tato oprávnění **požadovaná oprávnění** nastavení pro vaši aplikaci (viz  **Sestavení > aplikace pro Android** v sadě Visual Studio pro Mac, najdete v článku **vlastnosti > Manifest v Androidu** v sadě Visual Studio). 




## <a name="advanced-features"></a>Pokročilé funkce


### <a name="intent-actions-and-features"></a>Záměrné akce a funkce

Android manifest poskytuje způsob, jak můžete k popisu funkce vaše aktivity. To se dělá pomocí [záměry](http://developer.android.com/guide/topics/manifest/intent-filter-element.html) a [ `[IntentFilter]` ](https://developer.xamarin.com/api/type/Android.App.IntentFilterAttribute/) vlastního atributu. Můžete zadat akce, které jsou vhodné pro vaši aktivitu s [ `IntentFilter` ](https://developer.xamarin.com/api/constructor/Android.App.IntentFilterAttribute.IntentFilterAttribute/p/System.String[]/) konstruktor a jsou vhodné pomocí kategorií, které [ `Categories` ](https://developer.xamarin.com/api/property/Android.App.IntentFilterAttribute.Categories/) vlastnost. Třeba zadat alespoň jednu aktivitu (což je důvod, proč aktivit jsou k dispozici v konstruktoru). `[IntentFilter]` je možné poskytnout víc než jednou, a každé použití výsledků v samostatném `<intent-filter/>` element v rámci `<activity/>`. Příklad:

```csharp
[Activity (Label="Awesome Demo App", MainLauncher=true, Icon="@drawable/myicon")] 
[IntentFilter (new[]{Intent.ActionView}, 
        Categories=new[]{Intent.CategorySampleCode, "my.custom.category"})]
public class MyActivity : Activity
{
}
```

Tento příklad vytvoří následující fragment xml:

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


### <a name="application-element"></a>Aplikaci prvku

Android manifest také poskytuje způsob, jak deklarovat vlastnosti pro celou aplikaci. To se provádí prostřednictvím `<application>` elementu a jeho protějšek [aplikace](https://developer.xamarin.com/api/type/Android.App.ApplicationAttribute/) vlastního atributu. Všimněte si, že jde o celou aplikaci (sestavení) nastavení než nastavení podle aktivity. Obvykle je deklarovat `<application>` vlastnosti pro celou aplikaci a pak tato nastavení přepsat (podle potřeby) na základě za aktivity. 

Například následující `Application` přidání atributu do **AssemblyInfo.cs** k označení, že aplikace může být ladění, zda je jeho název čitelný pro uživatele **Moje aplikace**, a používá `Theme.Light` styl jako výchozí motiv pro všechny aktivity: 

```csharp
[assembly: Application (Debuggable=true,   
                        Label="My App",   
                        Theme="@android:style/Theme.Light")]
```

Tato deklarace způsobí, že následující fragment XML vygeneruje **obj/Debug/android/AndroidManifest.xml**:

```xml
<application android:label="My App" 
             android:debuggable="true" 
             android:theme="@android:style/Theme.Light"
                ... />
```
V tomto příkladu všechny aktivity v aplikaci budou ve výchozím nastavení `Theme.Light` style. Pokud nastavíte motiv aktivity na `Theme.Dialog`, pouze to, že aktivity použije `Theme.Dialog` stylu při všechny aktivity v aplikaci budou ve výchozím nastavení `Theme.Light` styl nastavený v `<application>` element. 

`Application` Element není jediným způsobem, jak nakonfigurovat `<application>` atributy. Alternativně můžete vložit přímo do atributů `<application>` prvek **Properties/AndroidManifest.xml**. Tato nastavení jsou sloučeny do konečné `<application>` element, který se nachází v **obj/Debug/android/AndroidManifest.xml**. Všimněte si, že obsah **Properties/AndroidManifest.xml** vždy přepíší data zadané vlastní atributy. 

Existuje mnoho atributů celou aplikaci, která můžete konfigurovat v `<application>` element; Další informace o těchto nastaveních najdete v tématu [veřejné vlastnosti](https://developer.xamarin.com/api/type/Android.App.ApplicationAttribute/#Public_Properties) část [ApplicationAttribute](https://developer.xamarin.com/api/type/Android.App.ApplicationAttribute/). 



## <a name="list-of-custom-attributes"></a>Seznam vlastních atributů

-   [Android.App.ActivityAttribute](https://developer.xamarin.com/api/type/Android.App.ActivityAttribute/) : vygeneruje [/manifest/application/activity](http://developer.android.com/guide/topics/manifest/activity-element.html) XML fragment 
-   [Android.App.ApplicationAttribute](https://developer.xamarin.com/api/type/Android.App.ApplicationAttribute/) : vygeneruje [/manifest/aplikace](http://developer.android.com/guide/topics/manifest/application-element.html) XML fragment 
-   [Android.App.InstrumentationAttribute](https://developer.xamarin.com/api/type/Android.App.InstrumentationAttribute/) : vygeneruje [/manifest/instrumentace](http://developer.android.com/guide/topics/manifest/instrumentation-element.html) XML fragment 
-   [Android.App.IntentFilterAttribute](https://developer.xamarin.com/api/type/Android.App.IntentFilterAttribute/) : vygeneruje [//intent-filter](http://developer.android.com/guide/topics/manifest/intent-filter-element.html) XML fragment 
-   [Android.App.MetaDataAttribute](https://developer.xamarin.com/api/type/Android.App.MetaDataAttribute/) : vygeneruje [//meta-data](http://developer.android.com/guide/topics/manifest/meta-data-element.html) XML fragment 
-   [Android.App.PermissionAttribute](https://developer.xamarin.com/api/type/Android.App.PermissionAttribute/) : vygeneruje [//permission](http://developer.android.com/guide/topics/manifest/permission-element.html) XML fragment 
-   [Android.App.PermissionGroupAttribute](https://developer.xamarin.com/api/type/Android.App.PermissionGroupAttribute/) : vygeneruje [//permission-group](http://developer.android.com/guide/topics/manifest/permission-group-element.html) XML fragment 
-   [Android.App.PermissionTreeAttribute](https://developer.xamarin.com/api/type/Android.App.PermissionTreeAttribute/) : vygeneruje [//permission-tree](http://developer.android.com/guide/topics/manifest/permission-tree-element.html) XML fragment 
-   [Android.App.ServiceAttribute](https://developer.xamarin.com/api/type/Android.App.ServiceAttribute/) : vygeneruje [/manifest/application/service](http://developer.android.com/guide/topics/manifest/service-element.html) XML fragment 
-   [Android.App.UsesLibraryAttribute](https://developer.xamarin.com/api/type/Android.App.UsesLibraryAttribute/) : vygeneruje [/manifest/application/uses-library](http://developer.android.com/guide/topics/manifest/uses-library-element.html) XML fragment 
-   [Android.App.UsesPermissionAttribute](https://developer.xamarin.com/api/type/Android.App.UsesPermissionAttribute/) : vygeneruje [/manifest/uses-permission](http://developer.android.com/guide/topics/manifest/uses-permission-element.html) XML fragment 
-   [Android.Content.BroadcastReceiverAttribute](https://developer.xamarin.com/api/type/Android.Content.BroadcastReceiverAttribute/) : vygeneruje [/manifest/application/receiver](http://developer.android.com/guide/topics/manifest/receiver-element.html) XML fragment 
-   [Android.Content.ContentProviderAttribute](https://developer.xamarin.com/api/type/Android.Content.ContentProviderAttribute/) : vygeneruje [/manifest/application/provider](http://developer.android.com/guide/topics/manifest/provider-element.html) XML fragment 
-   [Android.Content.GrantUriPermissionAttribute](https://developer.xamarin.com/api/type/Android.Content.GrantUriPermissionAttribute/) : vygeneruje [/manifest/application/provider/grant-uri-permission](http://developer.android.com/guide/topics/manifest/grant-uri-permission-element.html) XML fragment

