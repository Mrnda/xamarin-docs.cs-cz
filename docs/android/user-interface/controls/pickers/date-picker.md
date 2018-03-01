---
title: "Výběr data"
description: "Výběr kalendářních dat pomocí DatePickerDialog a DialogFragment"
ms.topic: article
ms.prod: xamarin
ms.assetid: F2BCD8D4-8957-EA53-C5A8-6BB603ADB47B
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 01/22/2018
ms.openlocfilehash: 3de935fd407524d7ba62a93205e333c7dd7adde0
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 02/27/2018
---
# <a name="date-picker"></a>Výběr data

## <a name="overview"></a>Přehled

Existují situace, kdy musí uživatel vstupní data do aplikace pro Android. Jako pomoc s tímto, poskytuje rozhraní Android [ `DatePicker` ](https://developer.xamarin.com/api/type/Android.Widget.DatePicker/) pomůcky a [ `DatePickerDialog` ](https://developer.xamarin.com/api/type/Android.App.DatePickerDialog/) . `DatePicker` Umožňuje vybrat rok, měsíc a den v konzistentní rozhraní mezi zařízeními a aplikacemi. `DatePickerDialog` Je pomocná třída, který zapouzdřuje `DatePicker` v dialogu.

Moderní aplikace pro Android by se měla zobrazovat `DatePickerDialog` v [ `DialogFragment` ](https://developer.xamarin.com/api/type/Android.App.DialogFragment/). To vám umožní aplikaci zobrazíte ovládací prvek DatePicker jako dialogové okno místní nebo vložené v aktivitě. Kromě toho `DialogFragment` bude spravovat životní cyklus a zobrazení dialogového okna, snižuje množství kód, který musí být implementována.

Tato příručka popisuje, jak používat `DatePickerDialog`, zabalené v `DialogFragment`. Ukázkové aplikace se zobrazí `DatePickerDialog` jako modální dialogové okno, když uživatel klikne na tlačítko na aktivitu. Pokud bude nastaveno uživatelem, `TextView` aktualizuje data, která byla vybrána.

[![Tlačítko – snímek obrazovky vyberte datum následuje dialogové okno pro výběr data](date-picker-images/image-01-sml.png)](date-picker-images/image-01.png)

## <a name="requirements"></a>Požadavky

Ukázkové aplikace pro tato příručka cílem Android 4.1 (API úrovně
16) nebo vyšší, ale platí Androidem 3.0 (API úrovně 11 nebo vyšší). Je možné podporují starší verze Android s přidáním systému v4 knihovna pro Android podporují a některé změny kódu projektu.

## <a name="using-the-datepicker"></a>Pomocí ovládací prvek DatePicker

Tato ukázka se rozšířit `DialogFragment`. Podtřídy hostitele, který se zobrazí `DatePickerDialog`:

![Dialogové okno pro výběr data Closeup](date-picker-images/image-02.png)

Když uživatel vybere datum a klikne **OK** tlačítko `DatePickerDialog` bude volat metodu [ `IOnDateSetListener.OnDateSet` ](https://developer.xamarin.com/api/member/Android.App.DatePickerDialog+IOnDateSetListener.OnDateSet/p/Android.Widget.DatePicker/System.Int32/System.Int32/System.Int32/).
Toto rozhraní je implementováno modulem, který je hostitelem `DialogFragment`. Pokud uživatel klikne **zrušit** tlačítko, pak fragment a dialogové okno bude zavřít sami.

Existuje několik způsobů `DialogFragment` vrátit hostování aktivity vybraným datem:

1. **Volání metody nebo nastavte vlastnost** &ndash; The aktivitu můžete zadat vlastnosti nebo metody speciálně pro nastavení této hodnoty.

2. **Aktivuje událost** &ndash; `DialogFragment` můžete definovat na událost, která bude vyvolána při `OnDateSet` je volána.

3. **Použijte `Action`**  &ndash; `DialogFragment` můžete vyvolat `Action<DateTime>` chcete zobrazit datum v aktivitě. Poskytne aktivity `Action<DateTime` při vytváření instance `DialogFragment`. Tato ukázka třetí způsobem a aby museli aktivity `Action<DateTime>` k `DialogFragment`.


<a name="extending_dialogfragment" />

### <a name="extending-dialogfragment"></a>Rozšíření DialogFragment

Prvním krokem při zobrazení `DatePickerDialog` je podtřídou `DialogFragment` a mějte ho implementovat `IOnDateSetListener` rozhraní:

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

`NewInstance` Metoda je volána k vytvoření instance nového `DatePickerFragment`. Tato metoda přebírá `Action<DateTime>` , bude volána, když uživatel klikne na **OK** v tlačítko `DatePickerDialog`.

Po který se má zobrazit fragment Android zavolá metodu `OnCreateDialog`. Tato metoda vytvoří novou `DatePickerDialog` objektu a provést jeho inicializaci s aktuálním datem a objekt zpětného volání (což je aktuální instancí třídy `DatePickerFragment`).


> [!NOTE]
> **Poznámka:** mějte na paměti, hodnota měsíce při `IOnDateSetListener.OnDateSet` je volána v rozsahu 0 až 11 a není 1 do 12. Den v měsíci bude v rozsahu od 1 do 31 (podle toho, která byla vybrána měsíce).


<a name="date_picker_fragment" />

### <a name="showing-the-datepickerfragment"></a>Zobrazuje DatePickerFragment

Teď, když `DialogFragment` byl implementovat, bude v této části Zkontrolujte použití fragment v aktivitě. Ukázkové aplikace, která doprovází tento průvodce, vytvoří instanci aktivity `DialogFragment` pomocí `NewInstance` metoda factory a poté zobrazte jeho vyvolání `DialogFragment.Show`. Jako součást vytváření instancí `DialogFragment`, předá aktivity `Action<DateTime>`, který se zobrazí datum v `TextView` hostující pomocí aktivity:

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

<a name="summary" />

## <a name="summary"></a>Souhrn

Tato ukázka popsané postupy: zobrazení `DatePicker` pomůcky jako místní modální dialogové okno jako součást Android aktivitu. Je k dispozici na ukázkové DialogFragment implementace a popsané `IOnDateSetListener` rozhraní. Tato ukázka také ukázán, jak DialogFragment může komunikovat s hostiteli aktivita se má zobrazit vybraným datem.


## <a name="related-links"></a>Související odkazy

- [DialogFragment](https://developer.xamarin.com/api/type/Android.App.DialogFragment/)
- [DatePicker](https://developer.xamarin.com/api/type/Android.Widget.DatePicker/)
- [DatePickerDialog](https://developer.xamarin.com/api/type/Android.App.DatePickerDialog/)
- [DatePickerDialog.IOnDateSetListener](https://developer.xamarin.com/api/type/Android.App.DatePickerDialog+IOnDateSetListener/)
- [Vyberte datum](https://github.com/xamarinhttps://developer.xamarin.com/recipes/tree/master/android/controls/datepicker/select_a_date)
