---
title: "Návod - ukládají stav aktivity"
description: "Budeme mít zahrnutých teoreticky za uložení stavu v Průvodci životního cyklu aktivity; Nyní projděme příklad."
ms.topic: article
ms.prod: xamarin
ms.assetid: A6090101-67C6-4BDD-9416-F2FB74805A87
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/16/2018
ms.openlocfilehash: 36cabddc2439d64ad2d1135bbd0d453a7f411750
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 02/27/2018
---
# <a name="walkthrough---saving-the-activity-state"></a>Návod - ukládají stav aktivity

_Budeme mít zahrnutých teoreticky za uložení stavu v Průvodci životního cyklu aktivity; Nyní projděme příklad._

## <a name="activity-state-walkthrough"></a>Návod stavu aktivity

Umožňuje otevřít **ActivityLifecycle_Start** projektu (v [ActivityLifecycle](https://developer.xamarin.com/samples/monodroid/ActivityLifecycle) ukázkové), jej sestavit a spustit. Toto je velmi jednoduchý projekt, který má dvě aktivity k předvedení životního cyklu aktivity a jak se nazývají různé metody životního cyklu. Při spuštění aplikace, na obrazovce `MainActivity` se zobrazí: 

[ ![Aktivity A obrazovky](saving-state-images/01-activity-a-sml.png)](saving-state-images/01-activity-a.png)

### <a name="viewing-state-transitions"></a>Přechody stavu zobrazení.

Každá metoda v této ukázce se zapíše do okna výstupu IDE aplikace k označení stavu aktivity. (Chcete-li otevřít okno výstupu v sadě Visual Studio, zadejte **CTRL-ALT-O**; Chcete-li otevřít okno výstupu v sadě Visual Studio pro Mac, klikněte na tlačítko **zobrazení > dotyková zařízení > výstupu aplikace**.)

Při prvním spuštění aplikace, ve výstupním okně zobrazí změny stavu *aktivity A*: 

```shell
[ActivityLifecycle.MainActivity] Activity A - OnCreate
[ActivityLifecycle.MainActivity] Activity A - OnStart
[ActivityLifecycle.MainActivity] Activity A - OnResume
```

Když kliknete na **spuštění aktivity B** tlačítko vidíme *aktivity A* pozastavení a ukončení při *aktivity B* prochází její změny stavu: 

```shell
[ActivityLifecycle.MainActivity] Activity A - OnPause
[ActivityLifecycle.SecondActivity] Activity B - OnCreate
[ActivityLifecycle.SecondActivity] Activity B - OnStart
[ActivityLifecycle.SecondActivity] Activity B - OnResume
[ActivityLifecycle.MainActivity] Activity A - OnStop
```

V důsledku toho *aktivity B* spustit a zobrazit místě *aktivity A*: 

[ ![Obrazovka aktivitě B](saving-state-images/02-activity-b-sml.png)](saving-state-images/02-activity-b.png)

Když kliknete na **zpět** tlačítko *aktivity B* zničena a *aktivity A* obnovení: 

```shell
[ActivityLifecycle.SecondActivity] Activity B - OnPause
[ActivityLifecycle.MainActivity] Activity A - OnRestart
[ActivityLifecycle.MainActivity] Activity A - OnStart
[ActivityLifecycle.MainActivity] Activity A - OnResume
[ActivityLifecycle.SecondActivity] Activity B - OnStop
[ActivityLifecycle.SecondActivity] Activity B - OnDestroy
```
### <a name="adding-a-click-counter"></a>Přidání čítačů klikněte na

V dalším kroku vytvoříme tak, aby jsme tlačítka, který sleduje a zobrazuje počet, který po kliknutí na změnit aplikace. Nejprve přidejme `_counter` proměnnou instance `MainActivity`: 

```csharp
int _counter = 0;
```

V dalším kroku tady upravit **Resource/layout/Main.axml** rozložení souboru a přidejte nový `clickButton` který zobrazí počet, kolikrát uživatel klikne na tlačítko. Výsledná **Main.axml** by měl vypadat takto: 

```xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:orientation="vertical"
    android:layout_width="fill_parent"
    android:layout_height="fill_parent">
    <Button
        android:id="@+id/myButton"
        android:layout_width="fill_parent"
        android:layout_height="wrap_content"
        android:text="@string/mybutton_text" />
    <Button
        android:id="@+id/clickButton"
        android:layout_width="fill_parent"
        android:layout_height="wrap_content"
        android:text="@string/counterbutton_text" />
</LinearLayout>
```

Pojďme přidejte následující kód do konce [OnCreate](https://developer.xamarin.com/api/member/Android.App.Activity.OnCreate/p/Android.OS.Bundle/) metoda v `MainActivity` &ndash; tento kód obslužné rutiny události z kliknutí `clickButton`:

```csharp
var clickbutton = FindViewById<Button> (Resource.Id.clickButton);
clickbutton.Text = Resources.GetString (
    Resource.String.counterbutton_text, _counter);
clickbutton.Click += (object sender, System.EventArgs e) =>
{
    _counter++;
    clickbutton.Text = Resources.GetString (
        Resource.String.counterbutton_text, _counter);
} ;
```

Když jsme sestavení a znovu spusťte aplikaci, nové tlačítko se zobrazuje, který zvýší a zobrazí hodnotu `_counter` na každou klikněte na:

[![Přidat počet dotykového ovládání](saving-state-images/03-touched-sml.png)](saving-state-images/03-touched.png)

Ale pokud jsme otočení zařízení do režimu na šířku, dojde ke ztrátě tento počet:

[ ![Otáčení na šířku nastaví počet zpět na nulu](saving-state-images/05-rotate-nosave-sml.png)](saving-state-images/05-rotate-nosave.png)

Zkoumání výstupu aplikace, jsme to vidět *aktivity A* byla pozastavena, byla zastavena, zničen, znovu vytvořit, restartovat a pak obnovit během otočení z na výšku do režimu na šířku: 

```shell
[ActivityLifecycle.MainActivity] Activity A - OnPause
[ActivityLifecycle.MainActivity] Activity A - OnStop
[ActivityLifecycle.MainActivity] Activity A - On Destroy

[ActivityLifecycle.MainActivity] Activity A - OnCreate
[ActivityLifecycle.MainActivity] Activity A - OnStart
[ActivityLifecycle.MainActivity] Activity A - OnResume
```

Protože *aktivity A* je zrušen a znovu znovu vytvoří, když zařízení otočen, dojde ke ztrátě jeho stav instance. Potom přidáme kód pro uložení a obnovení stavu instance.

### <a name="adding-code-to-preserve-instance-state"></a>Přidání kódu do stavu Instance zachovat

Umožňuje přidat metodu k `MainActivity` uložit stav instance. Před *aktivity A* je zničený, automaticky Android volá [OnSaveInstanceState](https://developer.xamarin.com/api/member/Android.App.Activity.OnSaveInstanceState/p/Android.OS.Bundle/) a předá [sady](https://developer.xamarin.com/api/type/Android.OS.Bundle/) že budeme moci použít k uložení naše stavu instance. Umožňuje použít k uložení naše klikněte na počet jako celé číslo:

```csharp
protected override void OnSaveInstanceState (Bundle outState)
{
    outState.PutInt ("click_count", _counter);
    Log.Debug(GetType().FullName, "Activity A - Saving instance state");

    // always call the base implementation!
    base.OnSaveInstanceState (outState);    
}
```

Když *aktivity A* se znovu vytvoří a obnovena, Android předá tento `Bundle` zpět do našich `OnCreate` metoda. Umožňuje přidat kód pro `OnCreate` k obnovení `_counter` hodnotu z předané v doplňku `Bundle`. Přidejte následující kód bezprostředně před řádek kde `clickbutton` je definována: 

```csharp
if (bundle != null)
{
    _counter = bundle.GetInt ("click_count", 0);
    Log.Debug(GetType().FullName, "Activity A - Recovered instance state");
}
```

Sestavení a znovu spusťte aplikaci a pak klikněte na tlačítko druhý několikrát. Když jsme otočení zařízení do režimu na šířku, se zachová, i počet!

[ ![Otočení obrazovky zobrazuje počet čtyři zachovaná](saving-state-images/06-rotate-save-sml.png)](saving-state-images/06-rotate-save.png)


Podívejme se na okno výstupu a zobrazit, co se stalo:
    
```shell
[ActivityLifecycle.MainActivity] Activity A - OnPause
[ActivityLifecycle.MainActivity] Activity A - Saving instance state
[ActivityLifecycle.MainActivity] Activity A - OnStop
[ActivityLifecycle.MainActivity] Activity A - On Destroy

[ActivityLifecycle.MainActivity] Activity A - OnCreate
[ActivityLifecycle.MainActivity] Activity A - Recovered instance state
[ActivityLifecycle.MainActivity] Activity A - OnStart
[ActivityLifecycle.MainActivity] Activity A - OnResume
``` 

Před [OnStop](https://developer.xamarin.com/api/member/Android.App.Activity.OnStop/) byla volána metoda, našeho nového `OnSaveInstanceState` byla volána metoda uložit `_counter` hodnotu `Bundle`. Android předán to `Bundle` zpět do us při ji volat naše `OnCreate` metoda a jsme byli schopni použít k obnovení `_counter` hodnotu kde jsme skončil.


## <a name="summary"></a>Souhrn

V této walkthough jsme použili naše znalosti o životním cyklu aktivity pro zachování dat o stavu. 



## <a name="related-links"></a>Související odkazy

- [ActivityLifecycle (ukázka)](https://developer.xamarin.com/samples/monodroid/ActivityLifecycle)
- [Životní cyklus aktivity](~/android/app-fundamentals/activity-lifecycle/index.md)
- [Android aktivity](https://developer.xamarin.com/api/type/Android.App.Activity/)
