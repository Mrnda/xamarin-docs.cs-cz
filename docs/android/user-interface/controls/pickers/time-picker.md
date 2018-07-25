---
title: Výběr času
description: Výběr času pomocí TimePickerDialog a DialogFragment
ms.prod: xamarin
ms.assetid: EB4E8206-E8AD-9F04-AC1C-82AC9364A9DD
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/06/2018
ms.openlocfilehash: 85d887cba7a596226d44bc13ca7155bc4a0d03ee
ms.sourcegitcommit: b56b3f906d2c05a3f1be219ef41be8b79e519b8e
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/25/2018
ms.locfileid: "39242300"
---
# <a name="time-picker"></a>Výběr času

Poskytnout způsob, jak může uživatel vybrat dobu, můžete použít [TimePicker](https://developer.xamarin.com/api/type/Android.Widget.TimePicker/). Aplikace pro Android se obvykle používá `TimePicker` s [TimePickerDialog](https://developer.xamarin.com/api/type/Android.App.TimePickerDialog/) pro výběr času hodnoty &ndash; to pomáhá zajistit konzistentní rozhraní napříč zařízeními a aplikacemi. `TimePicker` umožňuje uživatelům vybrat čas v režimu 24 hodin nebo 12hodinový dop. / odp.
`TimePickerDialog` je pomocnou třídu, která zapouzdřuje `TimePicker` v dialogovém okně.

[![Příklad snímek obrazovky dialogového okna pro výběr času v akci](time-picker-images/01-example-screen-sml.png)](time-picker-images/01-example-screen.png#lightbox)

## <a name="overview"></a>Přehled

Zobrazit moderních aplikací pro Android `TimePickerDialog` v [DialogFragment](https://developer.xamarin.com/api/type/Android.App.DialogFragment/). To umožňuje pro aplikace pro zobrazení `TimePicker` jako dialogové okno místní nabídky nebo ji vložit v nějaké aktivitě. Kromě toho `DialogFragment` spravuje životní cyklus a zobrazení dialogového okna, snižuje množství kódu, který musí být implementován.

Tato příručka ukazuje, jak používat `TimePickerDialog`ohraničenému `DialogFragment`. Ukázková aplikace zobrazí `TimePickerDialog` jako modální dialogové okno, když uživatel klikne na tlačítko pro aktivitu. Když nastavíte čas tímto uživatelem, ukončí dialogové okno a obslužná rutina aktualizace `TextView` na obrazovce aktivita s časem, který byl vybrán.

## <a name="requirements"></a>Požadavky

Ukázkové aplikace pro tohoto průvodce cílí na Android 4.1 (úroveň rozhraní API
16) nebo vyšší, ale je možné využít Androidu 3.0 (úroveň rozhraní API 11 nebo vyšší). Je možné pro podporu starší verze systému Android a uveďte v4 knihovnu pro Android podporují do projektu a některé změny kódu.

## <a name="using-the-timepicker"></a>Použití TimePicker

Tento příklad rozšiřuje `DialogFragment`; podtřídy provádění `DialogFragment` (volá `TimePickerFragment` níže) hostitelem a zobrazí `TimePickerDialog`. Ukázková aplikace poprvé spustí, zobrazí **VYSKLADNĚNÍ čas** výše uvedené tlačítko `TextView` , který se použije k zobrazení ve vybraném čase:

[![Obrazovka počáteční ukázkové aplikace](time-picker-images/02-initial-app-screen-sml.png)](time-picker-images/02-initial-app-screen.png#lightbox)

Po kliknutí **VYSKLADNĚNÍ čas** tlačítko, například spuštění aplikace `TimePickerDialog` jak je vidět na tomto snímku obrazovky:

[![Snímek obrazovky aplikace zobrazí výchozí dialogové okno Výběr času](time-picker-images/03-am-pm-time-dialog-sml.png)](time-picker-images/03-am-pm-time-dialog.png#lightbox)

V `TimePickerDialog`, výběrem čas a kliknutím na **OK** tlačítko způsobí, že `TimePickerDialog` k vyvolání metody [IOnTimeSetListener.OnTimeSet](https://developer.xamarin.com/api/member/Android.App.TimePickerDialog+IOnTimeSetListener.OnTimeSet/p/Android.Widget.TimePicker/System.Int32/System.Int32/System.Int32/).
Toto rozhraní je implementováno hostování `DialogFragment` (`TimePickerFragment`, které jsou popsány níže). Kliknutím **zrušit** tlačítko způsobí, že fragment a dialogové okno se zavře.

`DialogFragment` Vrátí ve vybraném čase k hostování Actvity v jednom ze tří způsobů:

1. **Vyvolání metody nebo nastavení vlastnosti** &ndash; The aktivity můžete zadat vlastnosti nebo metody speciálně pro nastavení této hodnoty.

2. **Vyvolání události** &ndash; `DialogFragment` můžete definovat událost, která bude vyvolána při `OnTimeSet` je vyvolána.

3. **Pomocí `Action`**  &ndash; `DialogFragment` můžete vyvolat `Action<DateTime>` k zobrazení času v aktivitě. Aktivita se poskytuje `Action<DateTime` při vytváření instance `DialogFragment`. 

Tato ukázka použije třetí techniku, která vyžaduje, aby aktivita napájení `Action<DateTime>` obslužná rutina `DialogFragment`.



## <a name="start-an-app-project"></a>Spuštění projektu aplikace

Začít nový projekt Android s názvem **TimePickerDemo** (Pokud nejste obeznámeni s vytvářením projekty Xamarin.Android, přečtěte si téma [Hello, Android](~/android/get-started/hello-android/hello-android-quickstart.md) informace o vytvoření nového projektu).

Upravit **Resources/layout/Main.axml** a jeho obsah nahraďte následující kód XML:

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

Toto je základní [LinearLayout](https://developer.xamarin.com/api/type/Android.Widget.LinearLayout/) s [TextView](https://developer.xamarin.com/api/type/Android.Widget.TextView/) , který zobrazí čas a [tlačítko](https://developer.xamarin.com/api/type/Android.Widget.Button/) , který se otevře `TimePickerDialog`. Všimněte si, že toto rozložení používá pevně zakódované řetězce a dimenze, aby aplikace jednodušší a srozumitelnější &ndash; produkční aplikace obvykle používá prostředky pro tyto hodnoty (jak je vidět v [DatePicker](https://github.com/xamarin/recipes/blob/master/Recipes/android/controls/datepicker/select_a_date/Resources/layout/Main.axml) příklad kódu).

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

Při sestavení a spuštění tohoto příkladu, měli byste vidět úvodní obrazovka podobně jako na následujícím snímku obrazovky:

[![Úvodní obrazovka aplikace](time-picker-images/02-initial-app-screen-sml.png)](time-picker-images/02-initial-app-screen.png#lightbox)

Kliknutím **výběr času** tlačítko nemá žádný účinek protože `DialogFragment` nebyla ještě implementována pro zobrazení `TimePicker`.
Dalším krokem je vytvoření tohoto `DialogFragment`.



## <a name="extending-dialogfragment"></a>Rozšíření DialogFragment

Chcete-li rozšířit `DialogFragment` pro použití s `TimePicker`, je potřeba vytvořit podtřídu, který je odvozen z `DialogFragment` a implementuje `TimePickerDialog.IOnTimeSetListener`. Přidejte následující třídy, která se **MainActivity.cs**:

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

To `TimePickerFragment` třída je rozdělené do menších a je vysvětleno v další části.


### <a name="dialogfragment-implementation"></a>Implementace DialogFragment

`TimePickerFragment` implementuje několik metod: metoda factory, metodu instance dialogové okno a `OnTimeSet` metodu obslužné rutiny vyžadují `TimePickerDialog.IOnTimeSetListener`.

-   `TimePickerFragment` je podtřídou třídy `DialogFragment`. Implementuje navíc `TimePickerDialog.IOnTimeSetListener` rozhraní (to znamená, poskytuje požadované `OnTimeSet` metoda):

    ```csharp
    public class TimePickerFragment : DialogFragment, TimePickerDialog.IOnTimeSetListener
    ```

-   `TAG` je inicializován pro účely protokolování (*MyTimePickerFragment* lze změnit na jakýkoli řetězec, kterou chcete použít). `timeSelectedHandler` Akce je inicializován na prázdný delegáta k zabránění výjimky odkaz s hodnotou null:

    ```csharp
    public static readonly string TAG = "MyTimePickerFragment";
    Action<DateTime> timeSelectedHandler = delegate { };
    ```

-   `NewInstance` Výrobní metoda je volána k vytvoření instance nového `TimePickerFragment`. Tato metoda přebírá `Action<DateTime>` obslužná rutina, která je volána, když uživatel klikne **OK** tlačítko `TimePickerDialog`:

    ```csharp
    public static TimePickerFragment NewInstance(Action<DateTime> onTimeSelected)
    {
        TimePickerFragment frag = new TimePickerFragment();
        frag.timeSelectedHandler = onTimeSelected;
        return frag;
    }
    ```

-   Fragment se nezobrazí, volá Android `DialogFragment` metoda [OnCreateDialog](https://developer.xamarin.com/api/member/Android.App.DialogFragment.OnCreateDialog/p/Android.OS.Bundle/). 
    Tato metoda vytvoří nový `TimePickerDialog` objektu a inicializuje ji s aktivitou, objekt zpětného volání (což je aktuální instancí třídy `TimePickerFragment`) a aktuální čas:

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

-   Když uživatel změní nastavení času `TimePicker` dialogového okna, `OnTimeSet` vyvolání metody. `OnTimeSet` vytvoří `DateTime` pomocí aktuálního data a sloučení v čase (hodinu a minutu) vybraný uživatelem:

    ```csharp
    public void OnTimeSet(TimePicker view, int hourOfDay, int minute)
    {
        DateTime currentTime = DateTime.Now;
        DateTime selectedTime = new DateTime(currentTime.Year, currentTime.Month, currentTime.Day, hourOfDay, minute, 0);
    ```


-   To `DateTime` objekt je předán do `timeSelectedHandler` , který je zaregistrován `TimePickerFragment` objektu v okamžiku vytvoření. `OnTimeSet` vyvolá tuto obslužnou rutinu aktualizovat zobrazení času aktivity na vybraném časovém (Tato obslužná rutina je implementována v další části):

    ```csharp
    timeSelectedHandler (selectedTime);
    ```



## <a name="displaying-the-timepickerfragment"></a>Zobrazení TimePickerFragment

Teď, když `DialogFragment` byl implementován, je čas vytvořit instanci `DialogFragment` pomocí `NewInstance` výrobní metoda a zobrazit tak, že vyvolá [DialogFragment.Show](https://developer.xamarin.com/api/member/Android.App.DialogFragment.Show/p/Android.App.FragmentManager/System.String/):

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

Po `TimeSelectOnClick` vytvoří instanci `TimePickerFragment`, vytváří a předává pro anonymní metody, která aktualizuje zobrazení času aktivity s hodnotou času předané v delegátovi. Nakonec se spustí `TimePicker` fragment dialogového okna (prostřednictvím `DialogFragment.Show`) zobrazíte `TimePicker` uživateli.

Na konci `OnCreate` metodu, přidejte následující řádek, který připojte obslužné rutiny události **VYSKLADNĚNÍ čas** tlačítko, které spustí dialogové okno:

```csharp
timeSelectButton.Click += TimeSelectOnClick;
```

Při **VYSKLADNĚNÍ čas** po kliknutí na tlačítko, `TimeSelectOnClick` bude vyvoláno pro zobrazení `TimePicker` fragment dialogové okno pro uživatele.



## <a name="try-it"></a>Můžete je vyzkoušejte.

Sestavte a spusťte aplikaci. Po kliknutí **VYSKLADNĚNÍ čas** tlačítko, `TimePickerDialog` se zobrazí ve výchozím formátu pro čas aktivity (v tomto režimu AM/PM případu, 12 hodin):

[![Čas dialogového okna se zobrazí v režimu AM/PM](time-picker-images/03-am-pm-time-dialog-sml.png)](time-picker-images/03-am-pm-time-dialog.png#lightbox)
   
Po kliknutí na **OK** v `TimePicker` dialogového okna, obslužná rutina aktualizace aktivity `TextView` zvoleném časovém a ukončení:

[![Čas A/M je zobrazen ve TextView aktivity](time-picker-images/04-after-time-dialog-sml.png)](time-picker-images/04-after-time-dialog.png#lightbox)

V dalším kroku přidejte následující řádek kódu, který `OnCreateDialog` ihned po `is24HourFormat` je deklarovány a inicializovány:

```csharp
is24HourFormat = true;
```

Tato změna způsobí, že příznak předán `TimePickerDialog` konstruktoru bude `true` tak tohoto režimu 24 hodin se používá namísto formát času hostování aktivity. Když sestavujete a znovu spusťte aplikaci, klikněte na tlačítko **VYSKLADNĚNÍ čas** tlačítko, `TimePicker` dialog se teď zobrazuje ve 24hodinovém formátu:

[![Dialogové okno TimePicker ve 24hodinovém formátu](time-picker-images/05-24hr-time-dialog-sml.png)](time-picker-images/05-24hr-time-dialog.png#lightbox)

Protože obslužná rutina zavolá [DateTime.ToShortTimeString](https://msdn.microsoft.com/en-us/library/system.datetime.toshortdatestring%28v=vs.110%29.aspx) tisknout čas na aktivitu `TextView`, čas je stále vytištěn v výchozí 12hodinový formát dop. / odp.



## <a name="summary"></a>Souhrn

Tento článek vysvětlil, jak zobrazit `TimePicker` widgetu jako modální dialogové okno automaticky otevíraného okna z aktivitu pro Android. Zadaný vzorek `DialogFragment` implementace a popsaných `IOnTimeSetListener` rozhraní. Tento příklad také ukazuje jak `DialogFragment` mohou komunikovat s hostiteli aktivita k zobrazení ve vybraném čase.


## <a name="related-links"></a>Související odkazy

- [DialogFragment](https://developer.xamarin.com/api/type/Android.App.DialogFragment/)
- [TimePicker](https://developer.xamarin.com/api/type/Android.Widget.TimePicker/)
- [TimePickerDialog](https://developer.xamarin.com/api/type/Android.App.TimePickerDialog/)
- [TimePickerDialog.IOnTimeSetListener](https://developer.xamarin.com/api/type/Android.App.TimePickerDialog+IOnTimeSetListener/)
- [TimePickerDemo (ukázka)](https://developer.xamarin.com/samples/monodroid/UserInterface/TimePickerDemo)
