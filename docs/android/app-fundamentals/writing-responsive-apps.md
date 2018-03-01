---
title: "Zápis reaguje aplikace"
ms.topic: article
ms.prod: xamarin
ms.assetid: 452DF940-6331-55F0-D130-002822BBED55
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/15/2018
ms.openlocfilehash: 67cad90f99614c3df90f483c9c6dbcec2df5113a
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 02/27/2018
---
# <a name="writing-responsive-applications"></a>Zápis reaguje aplikace

Jeden z klíčů udržování přizpůsobivý grafickým uživatelským rozhraním je k tomu grafického uživatelského rozhraní není zablokování dlouhotrvající úlohy na vlákna na pozadí. Může se stát, že chceme vypočítat hodnotu pro zobrazení pro uživatele, ale, že hodnota má 5 sekund vypočítat:

```csharp
public class ThreadDemo : Activity
{
    TextView textview;

    protected override void OnCreate (Bundle bundle)
    {
        base.OnCreate (bundle);

        // Create a new TextView and set it as our view
        textview = new TextView (this);
        textview.Text = "Working..";

        SetContentView (textview);

        SlowMethod ();
    }

    private void SlowMethod ()
    {
        Thread.Sleep (5000);
        textview.Text = "Method Complete";
    }
}
```

To bude fungovat, ale aplikace se "zablokuje" 5 sekund a hodnota se vypočítá. Během této doby nebude odpovídat aplikace zásahu uživatele. Chcete-li se tomuto problému vyhnout, chceme dělat naše výpočty v vlákna na pozadí:

```csharp
public class ThreadDemo : Activity
{
    TextView textview;

    protected override void OnCreate (Bundle bundle)
    {
        base.OnCreate (bundle);

        // Create a new TextView and set it as our view
        textview = new TextView (this);
        textview.Text = "Working..";

        SetContentView (textview);

        ThreadPool.QueueUserWorkItem (o => SlowMethod ());
    }

    private void SlowMethod ()
    {
        Thread.Sleep (5000);
        textview.Text = "Method Complete";
    }
}
```

Nyní jsme vypočítat hodnotu na vlákna na pozadí, tak zůstane během výpočtu přizpůsobivý naše grafického uživatelského rozhraní. Ale pokud se provádí výpočet, vaší aplikace dojde k chybě, a to v protokolu:

```shell
E/mono    (11207): EXCEPTION handling: Android.Util.AndroidRuntimeException: Exception of type 'Android.Util.AndroidRuntimeException' was thrown.
E/mono    (11207):
E/mono    (11207): Unhandled Exception: Android.Util.AndroidRuntimeException: Exception of type 'Android.Util.AndroidRuntimeException' was thrown.
E/mono    (11207):   at Android.Runtime.JNIEnv.CallVoidMethod (IntPtr jobject, IntPtr jmethod, Android.Runtime.JValue[] parms)
E/mono    (11207):   at Android.Widget.TextView.set_Text (IEnumerable`1 value)
E/mono    (11207):   at MonoDroidDebugging.Activity1.SlowMethod ()
```

Je to proto, že je nutné aktualizovat grafické uživatelské rozhraní z vlákna grafickým uživatelským rozhraním. Naše kód aktualizace grafické uživatelské rozhraní z fondu podprocesů vlákno způsobuje aplikaci havárií. Potřebujeme k výpočtu naše hodnotu ve vlákně na pozadí, ale pak dělat naše aktualizace ve vlákně grafického uživatelského rozhraní, které se zpracovávají s [Activity.RunOnUIThread](https://developer.xamarin.com/api/member/Android.App.Activity.RunOnUiThread/(System.Action)):

```csharp
public class ThreadDemo : Activity
{
    TextView textview;

    protected override void OnCreate (Bundle bundle)
    {
        base.OnCreate (bundle);

        // Create a new TextView and set it as our view
        textview = new TextView (this);
        textview.Text = "Working..";

        SetContentView (textview);

        ThreadPool.QueueUserWorkItem (o => SlowMethod ());
    }

    private void SlowMethod ()
    {
        Thread.Sleep (5000);
        RunOnUiThread (() => textview.Text = "Method Complete");
    }
}
```

Tento kód funguje podle očekávání. Tomto grafickém uživatelském rozhraní zůstává reakce a získá správně aktualizován po výpočtu komple.

Všimněte si, že tento postup není právě použít pro výpočet nákladné hodnotu. Slouží pro dlouhodobou úlohu, kterou lze provést na pozadí, jako jsou volání webové služby nebo stahování dat na internet.
