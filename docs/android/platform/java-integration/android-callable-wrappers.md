---
title: "Android – obálky s možností"
ms.topic: article
ms.prod: xamarin
ms.assetid: C33E15FA-1E2B-819A-C656-CA588D611492
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/15/2018
ms.openlocfilehash: f618f7257ab082a2a5b0aa587b135ad169d15133
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 02/27/2018
---
# <a name="android-callable-wrappers"></a>Android – obálky s možností

Android – obálky s možností (ACWs) se vyžadují při každém Android runtime vyvolá spravovaného kódu. Tyto obálky se vyžadují, protože neexistuje žádný způsob, jak zaregistrovat třídy s obrázky (Android runtime) v době běhu. (Konkrétně [JNI DefineClass() funkce](http://docs.oracle.com/javase/1.5.0/docs/guide/jni/spec/functions.html#wp15986) není podporovaný modulem Android runtime.} Android – obálky s možností se proto tvoří pro nedostatečná podpora registrace typu modulu runtime. 

*Pokaždé, když* kódu pro systém Android se musí provést `virtual` nebo rozhraní metody, které se `overridden` nebo implementují ve spravovaném kódu, Xamarin.Android musíte zadat Java proxy tak, aby tato metoda je odeslat do příslušného spravovaného typu. Tyto typy proxy Java jsou Java kód, který má "stejné" základní třídy a rozhraní seznamu Java jako spravovaný typ, implementace stejné konstruktory a deklarace žádné přepsaného základní třídy a metody rozhraní. 

Android – obálky s možností jsou generované **monodroid.exe** programu během [proces sestavení](~/android/deploy-test/building-apps/build-process.md): jsou generovány pro všechny typy, které dědí (přímo ani nepřímo) [ Java.Lang.Object](https://developer.xamarin.com/api/type/Java.Lang.Object/). 


<a name="ACW_Naming" />

## <a name="android-callable-wrapper-naming"></a>Pojmenování Android obálka volatelná aplikacemi

Názvy balíčku pro Android – obálky s možností jsou založené na MD5SUM sestavení kvalifikovaný název typu, která je exportována. Tato pojmenování technika umožňuje na stejný typ plně kvalifikovaný název bylo k dispozici podle různých sestavení bez Úvod k chybě při vytváření balíčků. 

Z důvodu této schéma pojmenování MD5SUM nelze přistupovat přímo vaší typy podle názvu. Například následující `adb` příkaz nebude fungovat, protože název typu `my.ActivityType` nevygenerovala ve výchozím nastavení: 

```shell
adb shell am start -n My.Package.Name/my.ActivityType
```

Pokud se pokusíte-typu podle názvu se také mohou zobrazit chyby takto:

```shell
java.lang.ClassNotFoundException: Didn't find class "com.company.app.MainActivity"
on path: DexPathList[[zip file "/data/app/com.company.App-1.apk"] ...
```

Pokud jste *provést* vyžadují přístup k typům podle názvu, můžou deklarovat název pro tento typ v deklarace atributu. Zde je kód, který deklaruje aktivitu se plně kvalifikovaný název například `My.ActivityType`:

```csharp
namespace My {
    [Activity]
    public partial class ActivityType : Activity {
        /* ... */
    }
}
```

`ActivityAttribute.Name` Vlastnost lze nastavit explicitně deklarovat název této aktivity: 

```csharp
namespace My {
    [Activity(Name="my.ActivityType")]
    public partial class ActivityType : Activity {
        /* ... */
    }
}
```

Po přidání nastavení této vlastnosti `my.ActivityType` je přístupná z externí kód a název `adb` skripty. `Name` Atribut lze nastavit pro mnoho různých typů, včetně `Activity`, `Application`, `Service`, `BroadcastReceiver`, a `ContentProvider`: 

-   [ActivityAttribute.Name](https://developer.xamarin.com/api/property/Android.App.ActivityAttribute.Name/)
-   [ApplicationAttribute.Name](https://developer.xamarin.com/api/property/Android.App.ApplicationAttribute.Name/)
-   [ServiceAttribute.Name](https://developer.xamarin.com/api/property/Android.App.ServiceAttribute.Name/)
-   [BroadcastReceiverAttribute.Name](https://developer.xamarin.com/api/property/Android.Content.BroadcastReceiverAttribute.Name/)
-   [ContentProviderAttribute.Name](https://developer.xamarin.com/api/property/Android.Content.ContentProviderAttribute.Name/)

Na základě MD5SUM pojmenování ACW byla zavedena v Xamarin.Android 5.0. Další informace o pojmenovávání atribut najdete v tématu [RegisterAttribute](https://developer.xamarin.com/api/type/Android.Runtime.RegisterAttribute/). 


<a name="Implementing_Interfaces" />

## <a name="implementing-interfaces"></a>Implementace rozhraní

Nastanou chvíle, kdy budete muset implementovat Android rozhraní, jako například [Android.Content.IComponentCallbacks](https://developer.xamarin.com/api/type/Android.Content.IComponentCallbacks/). Vzhledem k tomu, že všechny Android třídy a rozhraní rozšířit [Android.Runtime.IJavaObject](https://developer.xamarin.com/api/type/Android.Runtime.IJavaObject/) rozhraní, vzniká otázka: Jak můžeme implementovat `IJavaObject`? 

Výše proběhla odpověď na otázku: z důvodu všechny typy Android musí implementovat `IJavaObject` je tak, aby měla Android obálka volatelná aplikacemi k poskytování Android, tj. Java proxy pro daný typ Xamarin.Android. Vzhledem k tomu **monodroid.exe** pouze vyhledá `Java.Lang.Object` podtřídy, a `Java.Lang.Object` implementuje `IJavaObject,` odpověď je zřejmé: podtřídami `Java.Lang.Object`: 

```csharp
class MyComponentCallbacks : Java.Lang.Object, Android.Content.IComponentCallbacks {

    public void OnConfigurationChanged (Android.Content.Res.Configuration newConfig)
    {
        // implementation goes here...
    } 

    public void OnLowMemory ()
    {
        // implementation goes here...
    }
}
```

<a name="Implementation_Details" />

## <a name="implementation-details"></a>Podrobnosti implementace

*Zbývající části této stránky poskytuje podrobnosti implementace mohou změnit bez předchozího upozornění* (a je zde uvedené, protože vývojáři budou chcete zjistit, co se děje). 

Například uděleno následující zdroje C#:

```csharp
using System;
using Android.App;
using Android.OS;

namespace Mono.Samples.HelloWorld
{
    public class HelloAndroid : Activity
    {
        protected override void OnCreate (Bundle savedInstanceState)
        {
            base.OnCreate (savedInstanceState);
            SetContentView (R.layout.main);
        }
    }
}
```

**Mandroid.exe** program vygeneruje následující Android obálka volatelná aplikacemi: 

```java
package mono.samples.helloWorld;

public class HelloAndroid
    extends android.app.Activity
{
    static final String __md_methods;
    static {
        __md_methods = "n_onCreate:(Landroid/os/Bundle;)V:GetOnCreate_Landroid_os_Bundle_Handler\n" + "";
        mono.android.Runtime.register (
            "Mono.Samples.HelloWorld.HelloAndroid, HelloWorld, Version=1.0.0.0, 
            Culture=neutral, PublicKeyToken=null", HelloAndroid.class, __md_methods);
    }

    public HelloAndroid ()
    {
        super ();
        if (getClass () == HelloAndroid.class)
            mono.android.TypeManager.Activate (
                "Mono.Samples.HelloWorld.HelloAndroid, HelloWorld, Version=1.0.0.0, 
                Culture=neutral, PublicKeyToken=null", "", this, new java.lang.Object[] {  });
    }

    @Override
    public void onCreate (android.os.Bundle p0)
    {
        n_onCreate (p0);
    }

    private native void n_onCreate (android.os.Bundle p0);
}
```

Všimněte si, že se zachová, i základní třídy, a `native` metoda deklarace jsou k dispozici pro každou metodu, která je přepsána v rámci spravovaného kódu. 
