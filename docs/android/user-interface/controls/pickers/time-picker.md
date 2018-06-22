---
title: Výběr času
description: Výběr čas pomocí TimePickerDialog a DialogFragment
ms.prod: xamarin
ms.assetid: EB4E8206-E8AD-9F04-AC1C-82AC9364A9DD
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/06/2018
ms.openlocfilehash: c4261e3dccaccc4c88afe9c1033fb16b730fea6e
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/04/2018
ms.locfileid: "30769894"
---
# <a name="time-picker"></a>Výběr času

Chcete-li poskytují způsob, jak uživateli vybrat dobu, můžete použít [TimePicker](https://developer.xamarin.com/api/type/Android.Widget.TimePicker/). Aplikace pro Android se obvykle používají `TimePicker` s [TimePickerDialog](https://developer.xamarin.com/api/type/Android.App.TimePickerDialog/) pro výběr hodnotu času &ndash; tyto zásady pomáhají zajistit konzistentní rozhraní mezi zařízeními a aplikacemi. `TimePicker` umožňuje uživatelům vyberte denní dobu, v režimu dop. / odp 24 hodin nebo 12 hodin.
`TimePickerDialog` je pomocná třída, který zapouzdřuje `TimePicker` v dialogu.

[![Příklad snímek obrazovky dialogového okna pro výběr čas v akci](time-picker-images/01-example-screen-sml.png)](time-picker-images/01-example-screen.png#lightbox)

## <a name="overview"></a>Přehled

Moderní aplikace pro Android se zobrazí `TimePickerDialog` v [DialogFragment](https://developer.xamarin.com/api/type/Android.App.DialogFragment/). To umožňuje pro aplikace pro zobrazení `TimePicker` jako dialogové okno místní nebo vložit aktivitu. Kromě toho `DialogFragment` spravuje životní cyklus a zobrazení dialogového okna, snižuje množství kód, který musí být implementována.

Tato příručka ukazuje, jak používat `TimePickerDialog`, zabalené v `DialogFragment`. Ukázková aplikace zobrazí `TimePickerDialog` jako modální dialogové okno, když uživatel klikne na tlačítko na aktivitu. Pokud čas je nastaven uživatelem, ukončí dialogové okno a obslužná rutina aktualizace `TextView` na obrazovce aktivity s časem, který byl vybrán.

## <a name="requirements"></a>Požadavky

Ukázkové aplikace pro tato příručka cílem Android 4.1 (API úrovně
16) nebo vyšší, ale je lze použít s Androidem 3.0 (API úrovně 11 nebo vyšší). Je možné podporují starší verze Android s přidáním systému v4 knihovna pro Android podporují a některé změny kódu projektu.

## <a name="using-the-timepicker"></a>Pomocí TimePicker

V tomto příkladu rozšiřuje `DialogFragment`; implementace podtřídami `DialogFragment` (nazývá `TimePickerFragment` níže) hostitelem a zobrazí `TimePickerDialog`. Při prvním spuštění ukázkové aplikace se zobrazí **vybrat čas** výše uvedené tlačítko `TextView` který se použije k zobrazení vybrané času:

[![Počáteční ukázkové aplikace obrazovky](time-picker-images/02-initial-app-screen-sml.png)](time-picker-images/02-initial-app-screen.png#lightbox)

Když kliknete **vybrat čas** tlačítko spustí aplikaci příklad `TimePickerDialog` jak je vidět na tomto snímku obrazovky:

[![Snímek obrazovky dialogového okna Výběr času výchozí zobrazí aplikace](time-picker-images/03-am-pm-time-dialog-sml.png)](time-picker-images/03-am-pm-time-dialog.png#lightbox)

V `TimePickerDialog`, čas výběrem a kliknutím na **OK** tlačítko příčiny `TimePickerDialog` k vyvolání metody [IOnTimeSetListener.OnTimeSet](https://developer.xamarin.com/api/member/Android.App.TimePickerDialog+IOnTimeSetListener.OnTimeSet/p/Android.Widget.TimePicker/System.Int32/System.Int32/System.Int32/).
Toto rozhraní je implementováno modulem, který je hostitelem `DialogFragment` (`TimePickerFragment`, které jsou popsány níže). Kliknutím **zrušit** tlačítko způsobí, že fragment a dialogovém okně můžete zrušit.

`DialogFragment` vybraný čas vrátí k hostování Actvity v jednom ze tří způsobů:

1. **Vyvolání metody nebo nastavením vlastnosti** &ndash; The aktivitu můžete zadat vlastnosti nebo metody speciálně pro nastavení této hodnoty.

2. **Vyvolání události** &ndash; `DialogFragment` můžete definovat na událost, která bude vyvolána při `OnTimeSet` je volána.

3. **Pomocí `Action`**  &ndash; `DialogFragment` můžete vyvolat `Action<DateTime>` k zobrazení času v aktivitě. Poskytne aktivity `Action<DateTime` při vytváření instance `DialogFragment`. 

Tato ukázka použije třetí technika, který vyžaduje, aby aktivity napájení `Action<DateTime>` obslužná rutina `DialogFragment`.



## <a name="start-an-app-project"></a>Spusťte projekt aplikace

Spusťte nový projekt Android s názvem **TimePickerDemo** (Pokud nejste obeznámeni s vytváření projektů Xamarin.Android, přečtěte si téma [Hello, Android](~/android/get-started/hello-android/hello-android-quickstart.md) Další informace o vytvoření nového projektu).

Upravit **Resources/layout/Main.axml** a nahraďte jeho obsah následující kód XML:

```xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:orientation="vertical"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:layout_gravity="center_horizontal"
    android:padding="16dp">
    <Button
        android:id="@+id/select_button"
        android:paddingLeft="24dp"
        android:paddingRight="24dp"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:text="PICK TIME"
        android:textSize="20dp" />
    <TextView
        android:id="@+id/time_display"
        android:layout_height="wrap_content"
        android:layout_width="match_parent"
        android:paddingTop="22dp"
        android:text="Picked time will be displayed here"
        android:textSize="24dp" />
</LinearLayout>
```

Toto je základní [LinearLayout](https://developer.xamarin.com/api/type/Android.Widget.LinearLayout/) s [TextView](https://developer.xamarin.com/api/type/Android.Widget.TextView/) , zobrazí dobu a [tlačítko](https://developer.xamarin.com/api/type/Android.Widget.Button/) , otevře se `TimePickerDialog`. Všimněte si, že toto rozložení používá pevně řetězce a dimenze k vytvoření aplikace jednodušší a srozumitelnější &ndash; produkční aplikace obvykle používá prostředky pro tyto hodnoty (jak je vidět v [ovládací prvek DatePicker](https://github.com/xamarin/recipes/blob/master/android/controls/datepicker/select_a_date/Resources/layout/Main.axml) příklad kódu).

Upravit **MainActivity.cs** a nahraďte jeho obsah následujícím kódem:

```csharp
using Android.App;
using Android.Widget;
using Android.OS;
using System;
using Android.Util;
using Android.Text.Format;

namespace TimePickerDemo
{
    [Activity(Label = "TimePickerDemo", MainLauncher = true, Icon = "@drawable/icon")]
    public class MainActivity : Activity
    {
        TextView timeDisplay;
        Button timeSelectButton;

        protected override void OnCreate(Bundle bundle)
        {
            base.OnCreate(bundle);
            SetContentView(Resource.Layout.Main);
            timeDisplay = FindViewById<TextView>(Resource.Id.time_display);
            timeSelectButton = FindViewById<Button>(Resource.Id.select_button);
        }
    }
}
```

Když sestavení a spuštění tohoto příkladu, byste měli vidět úvodní obrazovka, podobně jako na následujícím snímku obrazovky:

[![Úvodní obrazovka aplikace](time-picker-images/02-initial-app-screen-sml.png)](time-picker-images/02-initial-app-screen.png#lightbox)

Kliknutím na **vybrat čas** tlačítko se nic nestane. protože `DialogFragment` není dosud implementována pro zobrazení `TimePicker`.
Dalším krokem je vytvoření to `DialogFragment`.



## <a name="extending-dialogfragment"></a>Rozšíření DialogFragment

Chcete-li rozšířit `DialogFragment` pro použití s `TimePicker`, je nutné vytvořit podtřídu, který je odvozený od `DialogFragment` a implementuje `TimePickerDialog.IOnTimeSetListener`. Přidejte následující třídy k **MainActivity.cs**:

```csharp
public class TimePickerFragment : DialogFragment, TimePickerDialog.IOnTimeSetListener
{
    public static readonly string TAG = "MyTimePickerFragment";
    Action<DateTime> timeSelectedHandler = delegate { };

    public static TimePickerFragment NewInstance(Action<DateTime> onTimeSelected)
    {
        TimePickerFragment frag = new TimePickerFragment();
        frag.timeSelectedHandler = onTimeSelected;
        return frag;
    }

    public override Dialog OnCreateDialog (Bundle savedInstanceState)
    {
        DateTime currentTime = DateTime.Now;
        bool is24HourFormat = DateFormat.Is24HourFormat(Activity);
        TimePickerDialog dialog = new TimePickerDialog
            (Activity, this, currentTime.Hour, currentTime.Minute, is24HourFormat);
        return dialog;
    }

    public void OnTimeSet(TimePicker view, int hourOfDay, int minute)
    {
        DateTime currentTime = DateTime.Now;
        DateTime selectedTime = new DateTime(currentTime.Year, currentTime.Month, currentTime.Day, hourOfDay, minute, 0);
        Log.Debug(TAG, selectedTime.ToLongTimeString());
        timeSelectedHandler (selectedTime);
    }
}
```

To `TimePickerFragment` třída je rozdělit na menší části a vysvětlené v další části.


### <a name="dialogfragment-implementation"></a>Implementace DialogFragment

`TimePickerFragment` implementuje několik metod: metoda factory, vytváření instancí dialogové okno metoda a `OnTimeSet` metoda obslužná rutina vyžaduje `TimePickerDialog.IOnTimeSetListener`.

-   `TimePickerFragment` je podtřídou třídy `DialogFragment`. Také implementuje `TimePickerDialog.IOnTimeSetListener` rozhraní (to znamená, poskytuje požadované `OnTimeSet` metoda):

    ```csharp
    public class TimePickerFragment : DialogFragment, TimePickerDialog.IOnTimeSetListener
    ```

-   `TAG` inicializovaná pro účely protokolování (*MyTimePickerFragment* lze změnit na jakémkoli řetězec, kterou chcete použít). `timeSelectedHandler` Delegáta prázdné, aby se zabránilo výjimky odkazu s hodnotou null je inicializováno akce:

    ```csharp
    public static readonly string TAG = "MyTimePickerFragment";
    Action<DateTime> timeSelectedHandler = delegate { };
    ```

-   `NewInstance` Factory metoda je volána k vytvoření instance nového `TimePickerFragment`. Tato metoda přebírá `Action<DateTime>` obslužná rutina, která je volána, když uživatel klikne **OK** v tlačítko `TimePickerDialog`:

    ```csharp
    public static TimePickerFragment NewInstance(Action<DateTime> onTimeSelected)
    {
        TimePickerFragment frag = new TimePickerFragment();
        frag.timeSelectedHandler = onTimeSelected;
        return frag;
    }
    ```

-   Po který se má zobrazit fragment Android volá `DialogFragment` metoda [OnCreateDialog](https://developer.xamarin.com/api/member/Android.App.DialogFragment.OnCreateDialog/p/Android.OS.Bundle/). 
    Tato metoda vytvoří novou `TimePickerDialog` objektu a inicializuje s aktivitou, objekt zpětného volání (což je aktuální instancí třídy `TimePickerFragment`) a aktuální čas:

    ```csharp
    public override Dialog OnCreateDialog (Bundle savedInstanceState)
    {
        DateTime currentTime = DateTime.Now;
        bool is24HourFormat = DateFormat.Is24HourFormat(Activity);
        TimePickerDialog dialog = new TimePickerDialog
            (Activity, this, currentTime.Hour, currentTime.Minute, is24HourFormat);
        return dialog;
    }
    ```

-   Když uživatel změní nastavení času `TimePicker` dialogové okno, `OnTimeSet` metoda je volána. `OnTimeSet` vytvoří `DateTime` pomocí k aktuálnímu datu a sloučí v čase (hodin a minut) vybrané uživatelem:

    ```csharp
    public void OnTimeSet(TimePicker view, int hourOfDay, int minute)
    {
        DateTime currentTime = DateTime.Now;
        DateTime selectedTime = new DateTime(currentTime.Year, currentTime.Month, currentTime.Day, hourOfDay, minute, 0);
    ```


-   To `DateTime` je předán objekt `timeSelectedHandler` , není zaregistrována `TimePickerFragment` objektu v okamžiku vytvoření. `OnTimeSet` Vyvolá této obslužné rutiny aktualizace zobrazení času aktivity pro vybrané časové (Tato obslužná rutina je implementována v další části):

    ```csharp
    timeSelectedHandler (selectedTime);
    ```



## <a name="displaying-the-timepickerfragment"></a>Zobrazení TimePickerFragment

Teď, když `DialogFragment` byla implementována, je čas vytvořit instanci `DialogFragment` pomocí `NewInstance` metoda factory a zobrazit ji vyvoláním [DialogFragment.Show](https://developer.xamarin.com/api/member/Android.App.DialogFragment.Show/p/Android.App.FragmentManager/System.String/):

Přidejte následující metodu do `MainActivity`:

```csharp
void TimeSelectOnClick (object sender, EventArgs eventArgs)
{
    TimePickerFragment frag = TimePickerFragment.NewInstance (
        delegate (DateTime time)
        {
            timeDisplay.Text = time.ToShortTimeString();
        });

    frag.Show(FragmentManager, TimePickerFragment.TAG);
}
```

Po `TimeSelectOnClick` vytvoří `TimePickerFragment`, vytvoří a předá delegáta pro anonymní metodu, která aktualizuje zobrazení času aktivity času předané hodnotou. Nakonec se spustí `TimePicker` fragment dialogové okno (prostřednictvím `DialogFragment.Show`) k zobrazení `TimePicker` uživateli.

Na konci `OnCreate` metoda, přidejte následující řádek pro připojení k obslužné rutiny události **vybrat čas** tlačítka, které spouští dialogové okno:

```csharp
timeSelectButton.Click += TimeSelectOnClick;
```

Když **vybrat čas** po kliknutí na tlačítko `TimeSelectOnClick` bude volána k zobrazení `TimePicker` fragment dialogové okno pro uživatele.



## <a name="try-it"></a>Můžete je vyzkoušejte.

Sestavte a spusťte aplikaci. Když kliknete **vybrat čas** tlačítko `TimePickerDialog` se zobrazí ve formátu času výchozí aktivity (v tomto režimu dop. / odp případu, 12 hodin):

[![Zobrazí Dialog čas v režimu dop. / odp](time-picker-images/03-am-pm-time-dialog-sml.png)](time-picker-images/03-am-pm-time-dialog.png#lightbox)
   
Když kliknete na tlačítko **OK** v `TimePicker` dialogové okno, obslužná rutina aktualizace aktivity `TextView` s zvoleném časovém a ukončí:

[![Zobrazení času A/M v TextView aktivity](time-picker-images/04-after-time-dialog-sml.png)](time-picker-images/04-after-time-dialog.png#lightbox)

V dalším kroku přidejte následující řádek kódu `OnCreateDialog` ihned po `is24HourFormat` je deklarovaný a inicializovat:

```csharp
is24HourFormat = true;
```

Tato změna vynutí příznak předaný `TimePickerDialog` konstruktor být `true` tak, že režimu 24 hodin se používá namísto formát času hostování aktivity. Po vytvoření a znovu spusťte aplikaci, klikněte na tlačítko **vybrat čas** tlačítko `TimePicker` dialogové okno se nyní zobrazí ve 24hodinovém formátu:

[![Dialogové okno TimePicker ve 24hodinovém formátu](time-picker-images/05-24hr-time-dialog-sml.png)](time-picker-images/05-24hr-time-dialog.png#lightbox)

Protože obslužná rutina volá [DateTime.ToShortTimeString](https://msdn.microsoft.com/en-us/library/system.datetime.toshortdatestring%28v=vs.110%29.aspx) čas na aktivitu vytisknout `TextView`, čas je stále vytištěno ve výchozím formátu dop. / odp 12 hodin.



## <a name="summary"></a>Souhrn

Postupy: zobrazení popsané v tomto článku `TimePicker` pomůcky jako místní modální dialogové okno z Android aktivity. Je poskytovala ukázku `DialogFragment` implementace a `IOnTimeSetListener` rozhraní. Tato ukázka také ukázán jak `DialogFragment` mohou komunikovat s hostiteli aktivita se má zobrazit vybraný čas.


## <a name="related-links"></a>Související odkazy

- [DialogFragment](https://developer.xamarin.com/api/type/Android.App.DialogFragment/)
- [TimePicker](https://developer.xamarin.com/api/type/Android.Widget.TimePicker/)
- [TimePickerDialog](https://developer.xamarin.com/api/type/Android.App.TimePickerDialog/)
- [TimePickerDialog.IOnTimeSetListener](https://developer.xamarin.com/api/type/Android.App.TimePickerDialog+IOnTimeSetListener/)
- [TimePickerDemo (sample)](https://developer.xamarin.com/samples/monodroid/UserInterface/TimePickerDemo)
