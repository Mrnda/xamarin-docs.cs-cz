---
title: Výběr data
description: Výběr kalendářních dat pomocí DatePickerDialog a DialogFragment
ms.prod: xamarin
ms.assetid: F2BCD8D4-8957-EA53-C5A8-6BB603ADB47B
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 01/22/2018
ms.openlocfilehash: 8f4e6318d904efb2f77c36732fc6519699f72ac9
ms.sourcegitcommit: b56b3f906d2c05a3f1be219ef41be8b79e519b8e
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/25/2018
ms.locfileid: "39241133"
---
# <a name="date-picker"></a>Výběr data

## <a name="overview"></a>Přehled

Existují situace, kdy musí uživatel vstupní data do aplikace pro Android. Pro účely pomoci s tím, poskytuje architekturu Androidu [ `DatePicker` ](https://developer.xamarin.com/api/type/Android.Widget.DatePicker/) widgetů a [ `DatePickerDialog` ](https://developer.xamarin.com/api/type/Android.App.DatePickerDialog/) . `DatePicker` Umožňuje uživatelům vybrat rok, měsíc a den v jednotné rozhraní napříč zařízeními a aplikacemi. `DatePickerDialog` Je pomocná třída, která zapouzdřuje `DatePicker` v dialogovém okně.

Moderní aplikace pro Android má zobrazit `DatePickerDialog` v [ `DialogFragment` ](https://developer.xamarin.com/api/type/Android.App.DialogFragment/). To vám umožní aplikaci zobrazit jako dialogové okno automaticky otevíraného okna Ovládací prvek DatePicker nebo vložené v nějaké aktivitě. Kromě toho `DialogFragment` budou spravovat životní cyklus a zobrazení dialogového okna, snižuje množství kódu, který musí být implementován.

Tento průvodce vám ukáže, jak používat `DatePickerDialog`ohraničenému `DialogFragment`. Ukázková aplikace se zobrazí `DatePickerDialog` jako modální dialogové okno, když uživatel klikne na tlačítko pro aktivitu. Když datum je nastavené uživatelem, `TextView` se aktualizuje a data, která byla vybrána.

[![Snímek obrazovky vybrat datum tlačítko a potom dialogové okno pro výběr data](date-picker-images/image-01-sml.png)](date-picker-images/image-01.png#lightbox)

## <a name="requirements"></a>Požadavky

Ukázkové aplikace pro tohoto průvodce cílí na Android 4.1 (úroveň rozhraní API
16) nebo vyšší, ale se vztahuje na Androidu 3.0 (úroveň rozhraní API 11 nebo vyšší). Je možné pro podporu starší verze systému Android a uveďte v4 knihovnu pro Android podporují do projektu a některé změny kódu.

## <a name="using-the-datepicker"></a>Použití ovládací prvek DatePicker

Tato ukázka se rozšíří `DialogFragment`. Podtřídy hostitele, který se zobrazí `DatePickerDialog`:

![Dialogové okno pro výběr data detail](date-picker-images/image-02.png)

Když uživatel vybere datum a klikne **OK** tlačítko, `DatePickerDialog` zavolá metodu [ `IOnDateSetListener.OnDateSet` ](https://developer.xamarin.com/api/member/Android.App.DatePickerDialog+IOnDateSetListener.OnDateSet/p/Android.Widget.DatePicker/System.Int32/System.Int32/System.Int32/).
Toto rozhraní je implementováno hostování `DialogFragment`. Pokud uživatel klikne **zrušit** tlačítko, pak fragment a dialogové okno se zavřít sami.

Existuje několik způsobů, jak `DialogFragment` vybraným datem mohli vrátit k hostování aktivity:

1. **Vyvolat metodu nebo nastavení vlastnosti** &ndash; The aktivity můžete zadat vlastnosti nebo metody speciálně pro nastavení této hodnoty.

2. **Vyvolání události** &ndash; `DialogFragment` můžete definovat událost, která bude vyvolána při `OnDateSet` je vyvolána.

3. **Použití `Action`**  &ndash; `DialogFragment` můžete vyvolat `Action<DateTime>` zobrazí datum v rámci aktivity. Aktivita se poskytuje `Action<DateTime` při vytváření instance `DialogFragment`. Tato ukázka třetí postup použít a aby museli aktivity `Action<DateTime>` k `DialogFragment`.



### <a name="extending-dialogfragment"></a>Rozšíření DialogFragment

Prvním krokem v zobrazování `DatePickerDialog` je podtřídou `DialogFragment` a jeho implementaci `IOnDateSetListener` rozhraní:

```csharp
public class DatePickerFragment : DialogFragment, 
                                  DatePickerDialog.IOnDateSetListener
{
    // TAG can be any string of your choice.
    public static readonly string TAG = "X:" + typeof (DatePickerFragment).Name.ToUpper();
    
    // Initialize this value to prevent NullReferenceExceptions.
    Action<DateTime> _dateSelectedHandler = delegate { };
    
    public static DatePickerFragment NewInstance(Action<DateTime> onDateSelected)
    {
        DatePickerFragment frag = new DatePickerFragment();
        frag._dateSelectedHandler = onDateSelected;
        return frag;
    }
    
    public override Dialog OnCreateDialog(Bundle savedInstanceState)
    {
        DateTime currently = DateTime.Now;
        DatePickerDialog dialog = new DatePickerDialog(Activity, 
                                                       this, 
                                                       currently.Year, 
                                                       currently.Month - 1,
                                                       currently.Day);
        return dialog;
    }
    
    public void OnDateSet(DatePicker view, int year, int monthOfYear, int dayOfMonth)
    {
        // Note: monthOfYear is a value between 0 and 11, not 1 and 12!
        DateTime selectedDate = new DateTime(year, monthOfYear + 1, dayOfMonth);
        Log.Debug(TAG, selectedDate.ToLongDateString());
        _dateSelectedHandler(selectedDate);
    }
}
```

`NewInstance` Je vyvolána metoda pro vytvoření instance nového `DatePickerFragment`. Tato metoda přebírá `Action<DateTime>` , která se vyvolá, když uživatel klikne na **OK** tlačítko `DatePickerDialog`.

Fragment se nezobrazí, Android bude volat metodu `OnCreateDialog`. Tato metoda vytvoří nový `DatePickerDialog` objektu a inicializovat s aktuálním datem a objekt zpětného volání (což je aktuální instancí třídy `DatePickerFragment`).


> [!NOTE]
> Mějte na paměti, která hodnota měsíce při `IOnDateSetListener.OnDateSet` je vyvolán v rozsahu 0 až 11, a ne 1 až 12. Den v měsíci, bude v rozsahu od 1 do 31 (podle toho, která byla vybrána měsíc).



### <a name="showing-the-datepickerfragment"></a>Zobrazuje DatePickerFragment

Teď, když `DialogFragment` byla implementována, tato část se zaměřuje použití fragment v nějaké aktivitě. V ukázkové aplikaci, který doprovází tento průvodce, vytvoří instanci aktivity `DialogFragment` pomocí `NewInstance` výrobní metoda a poté zobrazte ji vyvolat `DialogFragment.Show`. Jako součást vytváření instancí `DialogFragment`, předá aktivity `Action<DateTime>`,. Tím se zobrazí datum v `TextView` , který je hostitelem aktivity:

```csharp
[Activity(Label = "@string/app_name", MainLauncher = true, Icon = "@drawable/icon")]
public class MainActivity : Activity
{
    TextView _dateDisplay;
    Button _dateSelectButton;

    protected override void OnCreate(Bundle bundle)
    {
        base.OnCreate(bundle);
        SetContentView(Resource.Layout.Main);

        _dateDisplay = FindViewById<TextView>(Resource.Id.date_display);
        _dateSelectButton = FindViewById<Button>(Resource.Id.date_select_button);
        _dateSelectButton.Click += DateSelect_OnClick;
    }

    void DateSelect_OnClick(object sender, EventArgs eventArgs)
    {
        DatePickerFragment frag = DatePickerFragment.NewInstance(delegate(DateTime time)
                                                                 {
                                                                     _dateDisplay.Text = time.ToLongDateString();
                                                                 });
        frag.Show(FragmentManager, DatePickerFragment.TAG);
    }
}
```


## <a name="summary"></a>Souhrn

Tato ukázka popsané způsob zobrazení `DatePicker` widgetu jako modální dialogové okno místní nabídky v rámci aktivitu pro Android. Ukázková implementace DialogFragment k dispozici a popsaných `IOnDateSetListener` rozhraní. Tato ukázka také jsme vám ukázali, jak DialogFragment může komunikovat s hostiteli aktivita k zobrazení vybrané datum.


## <a name="related-links"></a>Související odkazy

- [DialogFragment](https://developer.xamarin.com/api/type/Android.App.DialogFragment/)
- [DatePicker](https://developer.xamarin.com/api/type/Android.Widget.DatePicker/)
- [DatePickerDialog](https://developer.xamarin.com/api/type/Android.App.DatePickerDialog/)
- [DatePickerDialog.IOnDateSetListener](https://developer.xamarin.com/api/type/Android.App.DatePickerDialog+IOnDateSetListener/)
- [Výběr data](https://github.com/xamarin/recipes/tree/master/Recipes/android/controls/datepicker/select_a_date)
